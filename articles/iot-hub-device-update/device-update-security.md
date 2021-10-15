---
title: Seguridad de Device Update para Azure IoT Hub | Microsoft Docs
description: Comprenda cómo garantiza Device Update para IoT Hub que los dispositivos se actualizan de forma segura.
author: andrewbrownmsft
ms.author: andbrown
ms.date: 10/5/2021
ms.topic: conceptual
ms.service: iot-hub
ms.openlocfilehash: 5c9684a3cb4798fbcba23a04cb21ba1da36b39ad
ms.sourcegitcommit: 57b7356981803f933cbf75e2d5285db73383947f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2021
ms.locfileid: "129545442"
---
# <a name="device-update-security-model"></a>Modelo de seguridad de Device Update

Device Update para IoT Hub ofrece un método seguro para implementar actualizaciones del firmware, las imágenes y las aplicaciones del dispositivo en los dispositivos IoT. El flujo de trabajo proporciona un canal seguro de un extremo a otro con un modelo de cadena de custodia completo que el dispositivo puede usar para comprobar que una actualización es de confianza, sin modificar e intencional.

Cada paso del flujo de trabajo de Device Update se protege mediante varias características y procesos de seguridad para garantizar que cada paso de la canalización realiza una entrega segura al siguiente. El código de referencia del agente de Device Update identifica y administra adecuadamente las solicitudes de actualización ilegítimas. El agente de referencia también comprueba todas las descargas para asegurarse de que el contenido sea de confianza, sin modificar e intencional.

## <a name="summary"></a>Resumen

A medida que las actualizaciones se importan en su instancia de Device Update, el servicio carga y comprueba los archivos binarios de la actualización para asegurarse de que un usuario malintencionado no los haya modificado ni intercambiado. Una vez comprobados, el servicio Device Update genera un [manifiesto de actualización](./update-manifest.md) interno con los códigos hash de archivo del manifiesto de importación y otros metadatos. A continuación, el servicio Device Update firma este manifiesto de actualización.

Una vez ingeridos en el servicio y almacenados en Azure, el servicio de almacenamiento de Azure cifra automáticamente los archivos binarios de actualización y los metadatos del cliente asociados en reposo. El servicio Device Update no proporciona automáticamente cifrado adicional, pero permite a los desarrolladores cifrar el contenido por sí mismos antes de que el contenido llegue al servicio Device Update.

Cuando se implementa una actualización en dispositivos desde el servicio Device Update, se envía un mensaje firmado a través del canal de IoT Hub protegido al dispositivo. El agente de Device Update valida la firma de la solicitud como auténtica. 

Cualquier descarga binaria resultante se protege mediante la validación de la firma del manifiesto de actualización. El manifiesto de actualización contiene los códigos hash de los archivos binarios, por lo que una vez que el manifiesto sea de confianza, Device Update Agent confía en los códigos hash y los compara con los archivos binarios. Una vez que el archivo binario de actualización se ha descargado y comprobado, se entrega al instalador del dispositivo.

## <a name="implementation-details"></a>Detalles de la implementación

Para asegurarse de que el servicio Device Update se reduce verticalmente para dispositivos simples y de bajo rendimiento, el modelo de seguridad usa claves asimétricas sin formato y firmas sin formato. Usan formatos basados en JSON, como los tokens JSON Web Token y las claves JSON Web Key.

### <a name="securing-update-content-via-the-update-manifest"></a>Protección del contenido de la actualización mediante el manifiesto de actualización

El manifiesto de actualización se valida con dos firmas. Las firmas se crean mediante una estructura que consta de claves de *firma* y claves *raíz*.

Device Update Agent tiene claves públicas insertadas que se usan para todos los dispositivos compatibles con Device Update. Estas son las claves *raíz*. Microsoft controla las claves privadas correspondientes.

Microsoft también genera un par de claves pública y privada que no se incluye en Device Update Agent ni se almacena en el dispositivo. Esta es la clave de *firma*.

Cuando se importa una actualización en Device Update para IoT Hub y el servicio genera el manifiesto de actualización, el servicio firma el manifiesto con la clave de firma e incluye la propia clave de firma, que está firmada por una clave raíz. Cuando se envía al dispositivo el manifiesto de actualización, Device Update Agent recibe los siguientes datos de firma:

1. El propio valor de la firma.
2. El algoritmo usado para generar nº 1.
3. La información de clave pública de la clave de firma utilizada para generar nº 1.
4. La firma de la clave de firma pública en nº 3.
5. El identificador de clave pública de la clave raíz utilizada para generar nº 3.
6. El algoritmo utilizado para generar nº 4.

Device Update Agent usa la información definida anteriormente para validar que la firma de la clave de firma pública está firmada por la clave raíz. Después, Device Update Agent valida que la firma del manifiesto de actualización está firmada por la clave de firma. Si todas las firmas son correctas, el manifiesto de actualización es de confianza para Device Update Agent. Dado que el manifiesto de actualización incluye los códigos hash de archivo que corresponden a los archivos de la actualización, los archivos de la actualización también pueden ser de confianza si los códigos hash coinciden.

Disponer de claves raíz y de firma permite a Microsoft rotar periódicamente la clave de firma, un procedimiento recomendado de seguridad.

### <a name="json-web-signature-jws"></a>Firma web JSON (JWS)

El elemento `updateManifestSignature` se utiliza para asegurarse de que la información contenida en el elemento `updateManifest` no se ha manipulado. `updateManifestSignature` se genera mediante una firma web JSON con claves JSON Web Key, lo que permite la comprobación del origen. La firma es una cadena codificada como dirección URL Base 64 con tres secciones delimitadas por el carácter ".".  Consulte los [métodos auxiliares de jws_util.h](https://github.com/Azure/iot-hub-device-update/tree/main/src/utils/jws_utils) para analizar y comprobar claves y tokens JSON.

La firma web JSON es un [estándar propuesto por IETF](https://tools.ietf.org/html/rfc7515) ampliamente utilizado para firmar el contenido mediante estructuras de datos basadas en JSON. Es una forma de garantizar la integridad de los datos mediante la comprobación de la firma de los datos. Puede encontrar más información en la entrada [RFC 7515](https://www.rfc-editor.org/info/rfc7515) correspondiente a la firma web JSON (JWS).

### <a name="json-web-token"></a>JSON Web Token

Los tokens JSON Web Token son un método abierto [estándar](https://tools.ietf.org/html/rfc7519) del sector para representar las notificaciones de forma segura entre dos partes.

### <a name="root-keys"></a>Claves raíz

Cada dispositivo con Device Update debe contener un conjunto de claves raíz. Estas claves son la raíz de confianza de todas las firmas de Device Update. Todas las firmas se deben encadenar mediante una de estas claves raíz para que se considere legítima.

El conjunto de claves raíz cambiará a lo largo del tiempo, ya que es adecuado rotar periódicamente las claves de firma por motivos de seguridad. Como resultado, el software del agente de Device Update deberá actualizarse con el conjunto más reciente de claves raíz a intervalos especificados por el equipo de Device Update. 

### <a name="signatures"></a>Prototipos

Todas las firmas se adaptarán mediante una clave de firma (pública) firmada por una de las claves raíz. La firma identificará la clave raíz que se usó para firmar la clave de firma. 

Un agente de Device Update debe validar las firmas; para ello, validará primero que la firma de la clave de firma (pública) es correcta, válida y ha sido firmada por una de las claves raíz aprobadas. Una vez que la clave de firma se valide correctamente, la propia firma se puede validar con la clave pública de firma que ahora es de confianza.

Las claves de firma se rotan con una cadencia mucho más rápida que las claves raíz, por lo que se deben esperar mensajes firmados por distintas claves de firma. 

El servicio Device Update administra la revocación de las claves de firma, por lo que los usuarios no deben intentar almacenar en caché las claves de firma. Use siempre la clave de firma que acompaña a una firma.

### <a name="securing-the-device"></a>Protección del dispositivo

Es importante asegurarse de que los recursos de seguridad relacionados con Device Update estén adecuadamente protegidos en el dispositivo. Los recursos, como las claves raíz, se deben proteger frente a modificaciones. Hay varias maneras de hacerlo, como el uso de dispositivos de seguridad (TPM, SGX, HSM u otros dispositivos de seguridad) o incluso codificarlos de forma rígida en el agente de Device Update Agent como se hace en la implementación de referencia. Esta última requiere que el código del agente de Device Update esté firmado digitalmente y que la compatibilidad con la integridad de código del sistema esté habilitada para protegerse frente a la modificación malintencionada del código del agente.

Se pueden garantizar medidas de seguridad adicionales, como asegurarse de que la entrega de componente a componente se realiza de forma segura. Por ejemplo, el registro de una cuenta aislada específica para ejecutar los distintos componentes y limitar las comunicaciones basadas en la red (por ejemplo, las llamadas API REST) solo a localhost.

**[Paso siguiente: más información sobre el uso de RBAC de Azure por parte de Device Update](.\device-update-control-access.md)**