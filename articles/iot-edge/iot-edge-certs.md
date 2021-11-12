---
title: Certificados para la seguridad del dispositivo en Azure IoT Edge | Microsoft Docs
description: Azure IoT Edge usa certificados para validar los dispositivos, los módulos y los dispositivos hoja, y establecer conexiones seguras entre ellos.
author: stevebus
ms.author: stevebus
ms.reviewer: kgremban
ms.date: 10/25/2021
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: mqtt
ms.openlocfilehash: 65b8bd55763de286ceccc7acd720d521d98f09d1
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131473075"
---
# <a name="understand-how-azure-iot-edge-uses-certificates"></a>Información sobre los certificados de Azure IoT Edge

[!INCLUDE [iot-edge-version-201806-or-202011](../../includes/iot-edge-version-201806-or-202011.md)]

Los módulos y los dispositivos IoT de bajada usan los certificados de IoT Edge para comprobar la identidad y la legitimidad del módulo del entorno de ejecución del [centro de IoT Edge](iot-edge-runtime.md#iot-edge-hub). Estas comprobaciones permiten una conexión TLS (seguridad de la capa de transporte) segura entre el entorno de ejecución, los módulos y los dispositivos IoT. Al igual que el propio IoT Hub, IoT Edge requiere una conexión segura y cifrada de los dispositivos IoT (u hoja) de nivel inferior y módulos de IoT Edge. Para establecer una conexión TLS segura, el módulo del centro de IoT Edge presenta una cadena de certificados de servidor para conectar a los clientes con el fin de comprobar su identidad.

>[!NOTE]
>En este artículo se habla sobre los certificados que se usan para proteger las conexiones entre los distintos componentes de un dispositivo IoT Edge o entre un dispositivo IoT Edge y los dispositivos hoja. También puede usar certificados para autenticar el dispositivo IoT Edge en IoT Hub. Estos certificados de autenticación son diferentes y no se tratan en este artículo. Para obtener más información sobre cómo autenticar el dispositivo con certificados, consulte [Creación y aprovisionamiento de un dispositivo IoT Edge mediante certificados X.509](how-to-provision-devices-at-scale-linux-x509.md).

En este artículo se explica cómo los certificados de IoT Edge pueden trabajar en escenarios de producción, desarrollo y prueba.

<!--1.2-->
:::moniker range=">=iotedge-2020-11"

## <a name="changes-in-version-12"></a>Cambios en la versión 1.2

* Se ha cambiado el nombre del **certificado de entidad de certificación del dispositivo** por **certificado de entidad de certificación perimetral**.
* El **certificado de CA de carga de trabajo** está en desuso. Ahora el administrador de seguridad de IoT Edge genera el certificado de servidor del centro de IoT Edge directamente desde el certificado de CA perimetral, sin el certificado de CA de carga de trabajo intermedio entre ellos.

:::moniker-end

## <a name="iot-edge-certificates"></a>Certificados de IoT Edge

Existen dos escenarios comunes para configurar certificados en un dispositivo IoT Edge. A veces, el usuario final o el operador de un dispositivo compra un dispositivo genérico de un fabricante y, a continuación, administra los certificados. En otras ocasiones, el fabricante trabaja bajo contrato para crear un dispositivo personalizado para el operador y crea alguna firma de certificado inicial antes de entregar el dispositivo. El diseño del certificado de IoT Edge intenta tener en cuenta ambos escenarios.

La ilustración siguiente muestra el uso de certificados de IoT Edge. Puede haber cero, uno o varios certificados de firma intermedios entre el certificado de CA raíz y el certificado de CA del dispositivo, según el número de entidades involucradas. Aquí se muestra un caso.

<!--1.1-->
:::moniker range="iotedge-2018-06"
![Diagrama de relaciones típicas de certificado](./media/iot-edge-certs/edgeCerts-general.png)

> [!NOTE]
> Actualmente, una limitación en libiothsm impide el uso de certificados que expiran el 1 de enero de 2038 o en una fecha posterior. Esta limitación se aplica al certificado de CA del dispositivo, a los certificados del paquete de confianza y a los certificados de identificador de dispositivo usados para los métodos de aprovisionamiento X.509.

:::moniker-end

<!--1.2-->
:::moniker range=">=iotedge-2020-11"

:::image type="content" source="./media/iot-edge-certs/iot-edge-certs-general-1-2.png" alt-text="Diagrama de las relaciones típicas de certificado de IoT Edge.":::

:::moniker-end

### <a name="certificate-authority"></a>Entidad de certificación

La entidad de certificación, o "CA" para abreviar, es una entidad que emite certificados digitales. Una entidad de certificación actúa como un tercero de confianza entre el propietario y el receptor del certificado. Un certificado digital certifica la propiedad de una clave pública por parte del receptor del certificado. La cadena de certificados de confianza funciona mediante la emisión de un certificado raíz inicialmente, que es la base de confianza de todos los certificados emitidos por la autoridad. Después, el propietario puede usar el certificado raíz para emitir más certificados intermedios (certificados "hoja").

### <a name="root-ca-certificate"></a>Certificado de entidad de certificación raíz

Un certificado de CA raíz es la raíz de confianza de todo el proceso. En escenarios de producción, este certificado de CA será normalmente un certificado adquirido a través de una entidad de certificación comercial de confianza, como Baltimore, Verisign o DigiCert. Para tener un control completo sobre los dispositivos que se conectan a los dispositivos de IoT Edge, es posible usar una entidad de certificación de nivel corporativo. En cualquier caso, la cadena de certificados completa del centro de IoT Edge se distribuye en él, por lo que los dispositivos hoja IoT deben confiar en el certificado raíz. Puede almacenar el certificado de CA raíz en el almacén de la entidad de certificación raíz de confianza o proporcionar los detalles del certificado en el código de la aplicación.

### <a name="intermediate-certificates"></a>Certificados intermedios

En un proceso de fabricación típico para crear dispositivos seguros, rara vez se usan certificados de CA raíz directamente, principalmente debido al riesgo de pérdida o exposición. El certificado de entidad de certificación raíz crea y firma digitalmente uno o varios certificados de este tipo intermedios. Puede haber solo uno o bien una cadena de estos certificados intermedios. Los escenarios que requieren una cadena de certificados intermedios incluyen:

* Una jerarquía de departamentos dentro de un fabricante.

* Varias empresas implicadas en serie en la producción de un dispositivo.

* Un cliente que compra un certificado de CA raíz y deriva un certificado de firma para que el fabricante firme los dispositivos que realizan en nombre de ese cliente.

En cualquier caso, el fabricante utiliza un certificado de CA intermedio al final de esta cadena para firmar el certificado de CA de dispositivo colocado en el dispositivo final. Por lo general, estos certificados intermedios están estrechamente protegidos en la planta industrial. Se someten a procesos estrictos, tanto físicos como electrónicos, para su uso.

<!--1.1-->
:::moniker range="iotedge-2018-06"
### <a name="device-ca-certificate"></a>Certificado de entidad de certificación de dispositivo

El certificado de CA de dispositivo se genera a partir del certificado de CA intermedio final y obtiene la firma de este en el proceso. Este certificado se instala en el dispositivo de IoT Edge, preferiblemente en un almacenamiento seguro, como un módulo de seguridad de hardware (HSM). Además, un certificado de CA de dispositivo identifica un dispositivo IoT Edge. El certificado de CA del dispositivo puede firmar otros certificados.

### <a name="iot-edge-workload-ca"></a>Entidad de certificación de carga de trabajo de IoT Edge

Este certificado, el primero en el lado del "operador" del proceso, se genera mediante el [Administrador de seguridad de IoT Edge](iot-edge-security-manager.md) cuando IoT Edge se inicia por primera vez. El certificado se genera a partir del certificado de CA de dispositivo y obtiene la firma de este. Este certificado, que es simplemente otro certificado de firma intermedio, se usa para generar y firmar otros certificados utilizados por el entorno de ejecución de IoT Edge. Hoy en día, es principalmente el certificado de servidor del centro de IoT Edge que se describe en la sección siguiente, pero, en el futuro, puede incluir otros certificados para autenticar los componentes de IoT Edge.

El propósito de este certificado intermedio de "carga de trabajo" consiste en dividir las preocupaciones entre el fabricante del dispositivo y el operador del dispositivo. Imagine un escenario donde un dispositivo IoT Edge se vende o se transfiere de un cliente a otro. Es probable que quiera que el certificado de CA de dispositivo proporcionado por el fabricante sea inmutable. Sin embargo, los certificados de "carga de trabajo" específicos para el funcionamiento del dispositivo deben borrarse y volver a crearse para la nueva implementación.

:::moniker-end

<!--1.2-->
:::moniker range=">=iotedge-2020-11"
### <a name="edge-ca-certificate"></a>Certificado de CA perimetral

El certificado de CA perimetral se genera a partir del certificado de CA intermedio final y obtiene la firma de este en el proceso. Este certificado se instala en el dispositivo de IoT Edge, preferiblemente en un almacenamiento seguro, como un módulo de seguridad de hardware (HSM). Además, un certificado de CA perimetral identifica un dispositivo IoT Edge. El certificado de CA perimetral puede firmar otros certificados.

:::moniker-end

### <a name="iot-edge-hub-server-certificate"></a>Certificado de servidor del centro de IoT Edge

El certificado de servidor del centro de IoT Edge es el certificado real presente en los dispositivos hoja y en los módulos para la verificación de identidad durante el establecimiento de la conexión TLS que requiere IoT Edge. Este certificado presenta la cadena completa de certificados de firma que se utilizó para generarlo hasta el certificado de CA raíz, en el que debe confiar el dispositivo hoja IoT. Cuando lo genera IoT Edge, el nombre común de este certificado del centro de IoT Edge se establece en la propiedad "hostname" en el archivo de configuración después de la conversión a minúsculas.

>[!Tip]
>Como el certificado de servidor del centro de IoT Edge usa la propiedad hostname del dispositivo como nombre común, ningún otro certificado de la cadena debe usar el mismo nombre común.

<!--1.2-->
:::moniker range=">=iotedge-2020-11"
El [Administrador de seguridad de IoT Edge](iot-edge-security-manager.md) genera el certificado del centro de IoT Edge, el primero en el lado del "operador" del proceso, al iniciar IoT Edge por primera vez. Este certificado se genera a partir del certificado de CA perimetral y obtiene la firma de este.
:::moniker-end

## <a name="production-implications"></a>Implicaciones de producción

Dado que los procesos de fabricación y funcionamiento son independientes, tenga en cuenta las implicaciones siguientes al preparar los dispositivos de producción:

<!--1.1-->
:::moniker range="iotedge-2018-06"

* Con cualquier proceso basado en certificados, el certificado de CA raíz y todos los certificados de CA intermedios deben protegerse y supervisarse durante todo el proceso de implementación de un dispositivo IoT Edge. El fabricante del dispositivo IoT Edge debe tener procesos seguros en lugar de un uso y almacenamiento adecuados de sus certificados intermedios. Además, el certificado de CA de dispositivo se debe mantener en un almacenamiento tan seguro como sea posible en el dispositivo, preferiblemente un módulo de seguridad de hardware.

* El centro de IoT Edge presenta el certificado de servidor del centro de IoT Edge a los dispositivos y módulos del cliente de conexión. El nombre común del certificado de entidad de certificación del dispositivo **no debe ser** el mismo que el de la propiedad "hostname" que se usará en el archivo de configuración en el dispositivo de IoT Edge. El nombre utilizado por los clientes para conectarse a IoT Edge (por ejemplo, mediante el parámetro GatewayHostName de la cadena de conexión o el comando CONNECT en MQTT) **no puede ser** el mismo que el nombre común usado en el certificado de entidad de certificación del dispositivo. Esta restricción se debe a que el centro de IoT Edge presenta su cadena de certificados completa para la comprobación por parte de los clientes. Si el certificado de servidor del centro de IoT Edge y el certificado de CA de dispositivo tienen el mismo nombre común, permanecerá en un bucle de comprobación y se invalidará el certificado.

* Dado que el demonio de seguridad de IoT Edge usa el certificado de CA de dispositivo para generar los certificados de IoT Edge finales, debe ser un certificado de firma, lo que significa que tiene funcionalidades de firma de certificados. Al aplicar "restricciones básicas V3 CA:True" al certificado de CA de dispositivo, se configurarán automáticamente las propiedades de uso de clave necesarias.
:::moniker-end

<!--1.2-->
:::moniker range=">=iotedge-2020-11"

* Con cualquier proceso basado en certificados, el certificado de CA raíz y todos los certificados de CA intermedios deben protegerse y supervisarse durante todo el proceso de implementación de un dispositivo IoT Edge. El fabricante del dispositivo IoT Edge debe tener procesos seguros en lugar de un uso y almacenamiento adecuados de sus certificados intermedios. Además, el certificado de CA perimetral se debe mantener en un almacenamiento tan seguro como sea posible en el propio dispositivo, preferiblemente un módulo de seguridad de hardware.

* El centro de IoT Edge presenta el certificado de servidor del centro de IoT Edge a los dispositivos y módulos del cliente de conexión. El nombre común (NC) del certificado de entidad de certificación perimetral **no debe ser** el mismo que el "nombre de host" que se usará en el archivo de configuración en el dispositivo IoT Edge. El nombre utilizado por los clientes para conectarse a IoT Edge (por ejemplo, mediante el parámetro GatewayHostName de la cadena de conexión o el comando CONNECT en MQTT) **no puede ser** el mismo que el nombre común usado en el certificado de entidad de certificación perimetral. Esta restricción se debe a que el centro de IoT Edge presenta su cadena de certificados completa para la comprobación por parte de los clientes. Si el certificado de servidor del centro de IoT Edge y el certificado de CA perimetral tienen el mismo nombre común, se genera un bucle de comprobación y se invalida el certificado.

* Como el demonio de seguridad de IoT Edge usa el certificado de CA perimetral para generar los certificados de IoT Edge finales, debe ser un certificado de firma, lo que significa que tiene funcionalidades de firma de certificados. Al aplicar "restricciones básicas V3 CA:True" al certificado de CA perimetral, se configurarán automáticamente las propiedades de uso de clave necesarias.
:::moniker-end

## <a name="devtest-implications"></a>Implicaciones de desarrollo y pruebas

Para facilitar los escenarios de desarrollo y pruebas, Microsoft proporciona un conjunto de [scripts de comodidad](https://github.com/Azure/iotedge/tree/master/tools/CACertificates) para generar certificados que no sean de producción adecuados para IoT Edge en el escenario de puerta de enlace transparente. Para ver ejemplos de cómo funcionan los scripts, consulte [Creación de certificados de demostración para probar las características del dispositivo IoT Edge](how-to-create-test-certificates.md).

>[!Tip]
> Para conectar las aplicaciones y los dispositivos "hoja" IoT del dispositivo que usan nuestro SDK de dispositivo IoT mediante IoT Edge, debe agregar el parámetro opcional GatewayHostName al final de la cadena de conexión del dispositivo. Cuando se genera el certificado de servidor del centro de IoT Edge, se basa en una versión en minúsculas del nombre de host del archivo de configuración; por tanto, para que los nombres coincidan y la comprobación del certificado TLS se realice correctamente, debe escribir el parámetro GatewayHostName en minúsculas.

## <a name="example-of-iot-edge-certificate-hierarchy"></a>Ejemplo de jerarquía de certificados de IoT Edge

Para ilustrar un ejemplo de la ruta de acceso de este certificado, la siguiente captura de pantalla es de un dispositivo IoT Edge en funcionamiento configurado como puerta de enlace transparente. OpenSSL se usa para conectarse al centro de IoT Edge, validar y volcar los certificados.

<!--1.1-->
:::moniker range="iotedge-2018-06"
![Captura de pantalla de la jerarquía de certificados en cada nivel](./media/iot-edge-certs/iotedge-cert-chain.png)

Puede ver la jerarquía de profundidad del certificado representada en la captura de pantalla:

| Tipo de certificado | Nombre del certificado|
|--|--|
| Certificado de entidad de certificación raíz | Solo prueba de certificado de entidad de certificación de Azure IoT Hub |
| Certificado de entidad de certificación intermedio | Solo prueba de certificado intermedio de Azure IoT Hub |
| Certificado de entidad de certificación de dispositivo | iotgateway.ca ("iotgateway" se ha pasado como nombre de certificado de CA a los scripts correspondientes) |
| Certificado de entidad de certificación de carga de trabajo | Entidad de certificación de carga de trabajo de IoT Edge |
| Certificado de servidor del centro de IoT Edge | iotedgegw.local (coincide con la propiedad "hostname" del archivo de configuración) |
:::moniker-end

<!--1.2-->
:::moniker range=">=iotedge-2020-11"

![Captura de pantalla de la jerarquía de certificados en cada nivel](./media/iot-edge-certs/iot-edge-cert-chain-1-2.png)

Puede ver la jerarquía de profundidad del certificado representada en la captura de pantalla:

| Tipo de certificado | Nombre del certificado |
|--|--|
| Certificado de entidad de certificación raíz | Solo prueba de certificado de entidad de certificación de Azure IoT Hub |
| Certificado de entidad de certificación intermedio | Solo prueba de certificado intermedio de Azure IoT Hub |
| Certificado de entidad de certificación de dispositivo | iotgateway.ca ("iotgateway" se ha pasado como nombre de certificado de CA a los scripts correspondientes) |
| Certificado de servidor del centro de IoT Edge | iotedgegw.local (coincide con la propiedad "hostname" del archivo de configuración) |
:::moniker-end

## <a name="next-steps"></a>Pasos siguientes

[Información sobre los módulos de Azure IoT Edge](iot-edge-modules.md)

[Configuración de un dispositivo IoT Edge para que actúe como puerta de enlace transparente](how-to-create-transparent-gateway.md)
