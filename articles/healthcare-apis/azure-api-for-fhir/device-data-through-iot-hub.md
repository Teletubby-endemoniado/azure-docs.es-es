---
title: Recepción de datos del dispositivo a través de Azure IoT Hub para Azure API for FHIR
description: En este tutorial, aprenderá a habilitar el enrutamiento de datos de dispositivos desde IoT Hub a Azure API for FHIR mediante el conector de Azure IoT para FHIR.
services: healthcare-apis
author: ms-puneet-nagpal
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: tutorial
ms.date: 09/10/2021
ms.author: rabhaiya
ms.openlocfilehash: 504b5755c84f179c56b124c5a31189806f6e1483
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130042377"
---
# <a name="receive-device-data-through-azure-iot-hub"></a>Recepción de datos del dispositivo mediante Azure IoT Hub

El conector de Azure IoT para Recursos Rápidos de Interoperabilidad en Salud (FHIR&#174;)* proporciona la funcionalidad de ingesta de datos de dispositivos de Internet de las cosas médicas (IoMT) en Azure API for FHIR. La guía de inicio rápido [Implementación del conector de Azure IoT para FHIR (versión preliminar) mediante Azure Portal](iot-fhir-portal-quickstart.md) muestra un ejemplo de un dispositivo administrado por Azure IoT Central que [envía datos de telemetría](iot-fhir-portal-quickstart.md#connect-your-devices-to-iot) al conector de Azure IoT para FHIR. El conector de Azure IoT para FHIR también puede trabajar con dispositivos aprovisionados y administrados mediante Azure IoT Hub. En este tutorial se proporciona el procedimiento para conectar y enrutar los datos del dispositivo desde Azure IoT Hub al conector de Azure IoT para FHIR.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure activa: [cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Un recurso de Azure API for FHIR con al menos un conector de Azure IoT para FHIR: [Implementación del conector de Azure IoT para FHIR (versión preliminar) mediante Azure Portal](iot-fhir-portal-quickstart.md)
- Un recurso de Azure IoT Hub conectado a dispositivos reales o simulados: [Creación de un centro de IoT con Azure Portal](../../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-csharp)

> [!TIP]
> Si usa una aplicación de dispositivo simulado de Azure IoT Hub, no dude en elegir la aplicación que prefiera entre los distintos lenguajes y sistemas compatibles.

## <a name="get-connection-string-for-azure-iot-connector-for-fhir-preview"></a>Obtención de la cadena de conexión para el conector de Azure IoT para FHIR (versión preliminar)

Azure IoT Hub requiere una cadena de conexión para conectarse de forma segura al conector de Azure IoT para FHIR. Cree una nueva cadena de conexión para el conector de Azure IoT para FHIR como se describe en [Generación de una cadena de conexión](iot-fhir-portal-quickstart.md#generate-a-connection-string). Conserve esta cadena de conexión para usarla en el paso siguiente.

El conector de Azure IoT para FHIR usa una instancia de Azure Event Hubs en segundo plano para recibir los mensajes del dispositivo. La cadena de conexión creada anteriormente es básicamente la cadena de conexión a este centro de eventos subyacente.

## <a name="connect-azure-iot-hub-with-the-azure-iot-connector-for-fhir-preview"></a>Conexión de Azure IoT Hub con el conector de Azure IoT para FHIR (versión preliminar)

Azure IoT Hub admite una característica llamada [enrutamiento de mensajes](../../iot-hub/iot-hub-devguide-messages-d2c.md) que proporciona la funcionalidad de enviar datos del dispositivo a varios servicios de Azure, como Event Hubs, una cuenta de Azure Storage y Service Bus. Azure IoT Connector for FHIR usa esta característica para conectarse y enviar datos del dispositivo desde Azure IoT Hub a su punto de conexión del centro de eventos.

> [!NOTE] 
> En este momento, solo se pueden usar el comando de PowerShell o de la CLI para [crear el enrutamiento de mensajes](../../iot-hub/tutorial-routing.md), porque el centro de eventos del conector de Azure IoT para FHIR no está hospedado en la suscripción del cliente, por lo que no será visible mediante Azure Portal. Sin embargo, una vez que se agregan los objetos de ruta de mensaje mediante PowerShell o la CLI, están visibles en Azure Portal y se pueden administrar desde allí.

La configuración de un enrutamiento de mensajes consta de dos pasos.

### <a name="add-an-endpoint"></a>Agregación de un extremo
En este paso se define el punto de conexión al que IoT Hub enrutará los datos. Puede crear este punto de conexión mediante el comando [Add-AzIotHubRoutingEndpoint](/powershell/module/az.iothub/Add-AzIotHubRoutingEndpoint) de PowerShell o el comando [az iot hub routing-endpoint create](/cli/azure/iot/hub/routing-endpoint) de la CLI, según sus preferencias.

Esta es la lista de parámetros que se usan con el comando para crear un punto de conexión:

|Parámetro de PowerShell|Parámetro de la CLI|Descripción|
|---|---|---|
|ResourceGroupName|resource-group|Nombre del grupo de recursos de su recurso de IoT Hub.|
|Nombre|hub-name|Nombre de su recurso de IoT Hub.|
|EndpointName|endpoint-name|Nombre que le gustaría asignar al punto de conexión que se va a crear.|
|EndpointType|endpoint-type|Tipo de punto de conexión al que necesita conectarse IoT Hub. Use el valor literal "EventHub" para PowerShell y "eventhub" para la CLI.|
|EndpointResourceGroup|endpoint-resource-group|Nombre del grupo de recursos para Azure IoT Connector para el recurso Azure API for FHIR FHIR. Puede obtener este valor en la página de información general de Azure API for FHIR.|
|EndpointSubscriptionId|endpoint-subscription-id|Identificador de suscripción de Azure IoT Connector para el recurso Azure API for FHIR FHIR. Puede obtener este valor en la página de información general de Azure API for FHIR.|
|ConnectionString|connection-string|Cadena de conexión al conector de Azure IoT para FHIR. Use el valor que obtuvo en el paso anterior.|

### <a name="add-a-message-route"></a>Adición de una ruta de mensajes
En este paso se define una ruta de mensajes mediante el punto de conexión creado anteriormente. Puede crear una ruta mediante el comando [Add-AzIotHubRoute](/powershell/module/az.iothub/Add-AzIoTHubRoute) de PowerShell o el comando [az iot hub route create](/cli/azure/iot/hub/route#az_iot_hub_route_create) de la CLI, según sus preferencias.

Esta es la lista de parámetros que se usan con el comando para agregar una ruta de mensaje:

|Parámetro de PowerShell|Parámetro de la CLI|Descripción|
|---|---|---|
|ResourceGroupName|g|Nombre del grupo de recursos de su recurso de IoT Hub.|
|Nombre|hub-name|Nombre de su recurso de IoT Hub.|
|EndpointName|endpoint-name|Nombre del punto de conexión que ha creado anteriormente.|
|RouteName|route-name|Nombre que desea asignar a la ruta de mensajes que se va a crear.|
|Source|source-type|Tipo de datos que se van a enviar al punto de conexión. Use el valor literal "DeviceMessages" para PowerShell y "devicemessages" para la CLI.|

## <a name="send-device-message-to-iot-hub"></a>Envío del mensaje de dispositivo a IoT Hub

Use el dispositivo (real o simulado) para enviar el mensaje de frecuencia cardíaca de ejemplo que se muestra a continuación a Azure IoT Hub. Este mensaje se enrutará al conector de Azure IoT para FHIR, donde el mensaje se transformará en un recurso de observación de FHIR y se almacenará en Azure API for FHIR.

```json
{
  "HeartRate": 80,
  "RespiratoryRate": 12,
  "HeartRateVariability": 64,
  "BodyTemperature": 99.08839032397609,
  "BloodPressure": {
    "Systolic": 23,
    "Diastolic": 34
  },
  "Activity": "walking"
}
```
> [!IMPORTANT]
> Asegúrese de enviar un mensaje de dispositivo que se ajuste a las [plantillas de asignación](iot-mapping-templates.md) configuradas con el conector de Azure IoT para FHIR.

## <a name="view-device-data-in-azure-api-for-fhir"></a>Visualización de los datos del dispositivo en Azure API for FHIR

Puede ver los recursos de observación de FHIR creados por Azure IoT Connector for FHIR mediante Postman. Para obtener más información, vea Acceso al servicio FHIR mediante [Postman](./../use-postman.md)y realice una solicitud para ver los recursos de Observación de FHIR con el valor de frecuencia del corazón enviado en el mensaje de `GET` ejemplo `https://your-fhir-server-url/Observation?code=http://loinc.org|8867-4` anterior.

> [!TIP]
> Asegúrese de que el usuario tiene acceso adecuado al plano de datos de Azure API for FHIR. Use el [control de acceso basado en roles de Azure (Azure RBAC)](configure-azure-rbac.md) para asignar los roles del plano de datos requeridos.


## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha configurado Azure IoT Hub para enrutar los datos del dispositivo al conector de Azure IoT para FHIR. Seleccione en los pasos siguientes para más información sobre el conector de Azure IoT para FHIR:

Conozca las distintas fases del flujo de datos en el conector de Azure IoT para FHIR.

>[!div class="nextstepaction"]
>[Flujo de datos del conector de Azure IoT para FHIR](iot-data-flow.md)

Aprenda a configurar el conector de IoT mediante plantillas de asignación de dispositivos y FHIR.

>[!div class="nextstepaction"]
>[Plantillas de asignación del conector de Azure IoT para FHIR (versión preliminar)](iot-mapping-templates.md)

*En Azure Portal, el conector de Azure IoT para FHIR se conoce como conector de IoT (versión preliminar). FHIR es una marca registrada de HL7 y se usa con el permiso de HL7.