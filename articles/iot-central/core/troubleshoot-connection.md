---
title: Solución de problemas de conexiones de dispositivos a Azure IoT Central | Microsoft Docs
description: Solucione los problemas por los que no se ven los datos de los dispositivos en IoT Central.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/13/2020
ms.topic: troubleshooting
ms.service: iot-central
ms.custom: device-developer, devx-track-azurecli
ms.openlocfilehash: a929e5d4bccc3a6b2d27125de6aad4472364d67e
ms.sourcegitcommit: c434baa76153142256d17c3c51f04d902e29a92e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2021
ms.locfileid: "132179140"
---
# <a name="troubleshoot-why-data-from-your-devices-isnt-showing-up-in-azure-iot-central"></a>Solucione los problemas por los que no se muestran los datos de los dispositivos en Azure IoT Central.

Este documento ayuda a averiguar por qué los datos que envían los dispositivos a IoT Central pueden no aparecer en la aplicación.

Hay dos áreas principales para investigar:

- Resolución de problemas de dispositivos
  - Problemas de autenticación como que el dispositivo tenga credenciales no válidas
  - Problemas de conectividad de red
  - El dispositivo no está aprobado o está bloqueado
- Problemas de forma de la carga útil del dispositivo

Esta guía de solución de problemas se centra en los relacionados con la conectividad de los dispositivos y en los de la forma de su carga útil.

## <a name="device-connectivity-issues"></a>Resolución de problemas de dispositivos

Esta sección le ayudará a determinar si los datos llegan a IoT Central.

Si todavía no lo ha hecho, instale la herramienta `az cli` y la extensión `azure-iot`.

Para más información sobre cómo instalar `az cli`, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

Para [instalar](/cli/azure/azure-cli-reference-for-IoT#extension-reference-installation) la extensión `azure-iot`, ejecute el comando siguiente:

```azurecli
az extension add --name azure-iot
```

> [!NOTE]
> Es posible que se le pida que instale la biblioteca `uamqp` la primera vez que ejecute un comando de extensión.

Cuando haya instalado la extensión `azure-iot`, inicie el dispositivo para ver si los mensajes que envía van a IoT Central.

Use los siguientes comandos para iniciar sesión en la suscripción en la que tenga la aplicación IoT Central:

```azurecli
az login
az account set --subscription <your-subscription-id>
```

Para supervisar la telemetría que el dispositivo envía, use el siguiente comando:

```azurecli
az iot central diagnostics monitor-events --app-id <app-id> --device-id <device-name>
```

Si el dispositivo se ha conectado correctamente a IoT Central, verá una salida similar a la siguiente:

```output
Monitoring telemetry.
Filtering on device: device-001
{
    "event": {
        "origin": "device-001",
        "module": "",
        "interface": "",
        "component": "",
        "payload": {
            "temp": 65.57910343679293,
            "humid": 36.16224660107426
        }
    }
}
```

Para supervisar las actualizaciones de propiedades que el dispositivo intercambia con IoT Central, use el siguiente comando de versión preliminar:

```azurecli
az iot central diagnostics monitor-properties --app-id <app-id> --device-id <device-name>
```

Si el dispositivo envía correctamente las actualizaciones de propiedades, verá una salida similar a la siguiente:

```output
Changes in reported properties:
version : 32
{'state': 'true', 'name': {'value': {'value': 'Contoso'}, 'status': 'completed', 'desiredVersion': 7, 'ad': 'completed', 'av': 7, 'ac
': 200}, 'brightness': {'value': {'value': 2}, 'status': 'completed', 'desiredVersion': 7, 'ad': 'completed', 'av': 7, 'ac': 200}, 'p
rocessorArchitecture': 'ARM', 'swVersion': '1.0.0'}
```

Si ve que los datos aparecen en el terminal, es que están llegando hasta la aplicación IoT Central.

Si no ve ningún dato después de unos minutos, pruebe a presionar la tecla `Enter` o `return`, por si la salida está atascada.

Si sigue sin ver ningún dato en el terminal, es probable que el dispositivo tenga problemas de conectividad de red o que no esté enviando los datos correctamente a IoT Central.

### <a name="check-the-provisioning-status-of-your-device"></a>Comprobación del estado de aprovisionamiento del dispositivo

Si los datos no aparecen en el monitor, compruebe el estado de aprovisionamiento del dispositivo; para ello, ejecute el siguiente comando:

```azurecli
az iot central device registration-info --app-id <app-id> --device-id <device-name>
```

La salida siguiente muestra un ejemplo de un dispositivo que no se pudo conectar:

```json
{
  "@device_id": "v22upeoqx6",
  "device_registration_info": {
    "device_status": "blocked",
    "display_name": "Environmental Sensor - v22upeoqx6",
    "id": "v22upeoqx6",
    "instance_of": "urn:krhsi_k0u:modelDefinition:w53jukkazs",
    "simulated": false
  },
  "dps_state": {
    "error": "Device is blocked from connecting to IoT Central application. Unblock the device in IoT Central and retry. Learn more:
https://aka.ms/iotcentral-docs-dps-SAS",
    "status": null
  }
}
```

| Estado de aprovisionamiento de dispositivos | Descripción | Posible mitigación |
| - | - | - |
| aprovisionado | No hay ningún problema reconocible inmediatamente. | N/D |
| Registrado | El dispositivo aún no se ha conectado a IoT Central. | Compruebe si hay problemas de conectividad en los registros del dispositivo. |
| Bloqueado | El dispositivo no se puede conectar a IoT Central. | El dispositivo está bloqueado y no se pudo conectar a la aplicación IoT Central. Desbloquee el dispositivo en IoT Central e inténtelo de nuevo. Para más información, consulte [Bloqueo de dispositivos](concepts-get-connected.md#device-status-values). |
| No aprobado | El dispositivo no está aprobado. | El dispositivo no tiene aprobación para conectarse a la aplicación IoT Central. Apruebe el dispositivo en IoT Central e inténtelo de nuevo. Para más información, consulte [Aprobación de dispositivos](concepts-get-connected.md#device-registration). |
| Sin asociar | El dispositivo no está asociado a una plantilla de dispositivo. | Asócielo a una plantilla de dispositivo para que IoT Central sepa cómo analizar los datos. |

Más información acerca de [códigos de estado de dispositivo](concepts-get-connected.md#device-status-values).

### <a name="error-codes"></a>Códigos de error

Si sigue sin poder diagnosticar por qué los datos no se muestran en `monitor-events`, el siguiente paso es buscar los códigos de error que el dispositivo indique.

Inicie una sesión de depuración en el dispositivo o recopile los registros de este. Busque los códigos de error que el dispositivo notifique.

En las tablas siguientes se muestran códigos de error comunes y las posibles acciones para mitigarlos.

Si ve problemas relacionados con el flujo de autenticación:

| Código de error | Descripción | Posible mitigación |
| - | - | - |
| 400 | El cuerpo de la solicitud no es válido. Por ejemplo, no se puede analizar o el objeto no se puede comprobar. | Asegúrese de estar enviando el cuerpo de solicitud correcto como parte del flujo de atestación o bien use un SDK de dispositivo. |
| 401 | El token de autorización no se puede comprobar. Por ejemplo, ha expirado o no es aplicable al URI de la solicitud. Este código de error también se devuelve a los dispositivos como parte del flujo de atestación de TPM. | Asegúrese de que el dispositivo tenga las credenciales correctas. |
| 404 | La instancia de Device Provisioning Service o un recurso como, por ejemplo, una inscripción, no existe. | [Abra una incidencia en el departamento de asistencia al cliente](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). |
| 412 | La etiqueta `ETag` de la solicitud no coincide con la etiqueta `ETag` del recurso existente, según las especificaciones de RFC7232. | [Abra una incidencia en el departamento de asistencia al cliente](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). |
| 429 | El servicio está limitando las operaciones. Para conocer los límites de servicio específicos, consulte [Límites de IoT Hub Device Provisioning Service](../../azure-resource-manager/management/azure-subscription-service-limits.md#iot-hub-device-provisioning-service-limits). | Reduzca la frecuencia de los mensajes y divida las responsabilidades entre más dispositivos. |
| 500 | Se ha producido un error interno. | [Abra una incidencia en el departamento de asistencia al cliente](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) para comprobar si pueden ayudarle. |

### <a name="detailed-authorization-error-codes"></a>Códigos de error de autorización detallados

| Error | Subcódigo de error | Notas |
| - | - | - |
| 401 No autorizado | 401002 | El dispositivo usa credenciales no válidas o expiradas. DPS notifica este error. |
| 401 No autorizado | 400209 | El dispositivo está esperando la aprobación de un operador o un operador lo ha bloqueado. |
| 401 IoTHubUnauthorized |  | El dispositivo usa el token de seguridad expirado. Este error lo notifica IoT Hub. |
| 401 IoTHubUnauthorized | DEVICE_DISABLED | El dispositivo está deshabilitado en este centro de IoT y se ha movido a otro centro de IoT. Vuelva a aprovisionar el dispositivo. |
| 401 IoTHubUnauthorized | DEVICE_BLOCKED | Un operador ha bloqueado este dispositivo. |



### <a name="file-upload-error-codes"></a>Códigos de error de carga de archivos

A continuación se incluye una lista de códigos de error comunes que pueden mostrarse cuando un dispositivo intenta cargar un archivo en la nube. Recuerde que antes de que el dispositivo pueda cargar un archivo, debe configurar las [cargas de archivos del dispositivo](howto-configure-file-uploads.md) en la aplicación.

| Código de error | Descripción | Posible mitigación |
| - | - | - |
| 403006  | Ha superado el número de operaciones de carga de archivos simultáneas. Cada cliente de dispositivo se limita a diez cargas de archivos simultáneas. | Asegúrese de que el dispositivo notifica rápidamente a IoT Central que se ha completado la operación de carga de archivos. Si eso no funciona, intente reducir el tiempo de espera de solicitud. |

## <a name="payload-shape-issues"></a>Problemas con la forma de la carga útil

Cuando haya establecido que el dispositivo está enviando datos a IoT Central, el siguiente paso es asegurarse de que los envía en un formato válido.

Hay dos categorías principales de problemas comunes que hacen que los datos del dispositivo no aparezcan en IoT Central:

- La plantilla de dispositivo para los datos del dispositivo no coincide:
  - No coinciden los nombres, por ejemplo, hay errores tipográficos o las mayúsculas y las minúsculas no concuerdan.
  - Propiedades no modeladas en las que el esquema no se define en la plantilla de dispositivo.
  - No coincide el esquema, por ejemplo, un tipo se define en la plantilla como `boolean`, pero los datos son una cadena.
  - El mismo nombre de telemetría se define en varias interfaces, pero el dispositivo no es compatible con IoT Plug and Play.
- La forma de los datos es un formato JSON no válido. Para más información, consulte [Cargas de telemetría, propiedades y comandos](concepts-telemetry-properties-commands.md).

Para detectar las categorías del problema, ejecute el comando más apropiado para su escenario:

- Para comprobar la telemetría, use el comando de la versión preliminar:

    ```azurecli
    az iot central diagnostics validate-messages --app-id <app-id> --device-id <device-name>
    ```

- Para comprobar las actualizaciones de las propiedades, use el comando de la versión preliminar:

    ```azurecli
    az iot central diagnostics validate-properties --app-id <app-id> --device-id <device-name>
    ```

Es posible que se le pida que instale la biblioteca `uamqp` la primera vez que ejecute un comando `validate`.

La siguiente salida muestra mensajes de error y advertencia de ejemplo del comando validate:

```output
Validating telemetry.
Filtering on device: v22upeoqx6.
Exiting after 300 second(s), or 10 message(s) have been parsed (whichever happens first).
[WARNING] [DeviceId: v22upeoqx6] No encoding found. Expected encoding 'utf-8' to be present in message header.

[WARNING] [DeviceId: v22upeoqx6] Content type '' is not supported. Expected Content type is 'application/json'.

[ERROR] [DeviceId: v22upeoqx6] [TemplateId: urn:krhsi_k0u:modelDefinition:w53jukkazs] Datatype of field 'humid' does not match the da
tatype 'double'. Data '56'. All dates/times/datetimes/durations must be ISO 8601 compliant.
```

Si prefiere usar una GUI, use la vista **Datos sin procesar** de IoT Central y compruebe si algo no se está modelando. La vista **Datos sin procesar** no detecta si el dispositivo está enviando JSON con formato incorrecto.

:::image type="content" source="media/troubleshoot-connection/raw-data-view.png" alt-text="Captura de pantalla de la vista de datos sin procesar":::

Cuando haya detectado el problema, es posible que tenga que actualizar el firmware del dispositivo o crear una nueva plantilla de dispositivo que contenga los datos no modelados previamente.

Si decide crear una plantilla que modele los datos correctamente, migre los dispositivos de la plantilla anterior a la nueva. Para más información, consulte [Administración de dispositivos en la aplicación de Azure IoT Central](howto-manage-devices-individually.md).

## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/community/). Como alternativa, puede abrir una [incidencia de soporte técnico de Azure](https://portal.azure.com/#create/Microsoft.Support).

Para más información, consulte [Opciones de ayuda y soporte técnico de IoT de Azure](../../iot-fundamentals/iot-support-help.md).
