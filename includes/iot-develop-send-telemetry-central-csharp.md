---
title: Archivo de inclusión
description: Archivo de inclusión
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 04/28/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: b4ff1f14ac628f28e67f4b619983760d06f60d8b
ms.sourcegitcommit: 557ed4e74f0629b6d2a543e1228f65a3e01bf3ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2021
ms.locfileid: "129516509"
---
[![Examinar el código](../articles/iot-develop/media/common/browse-code.svg)](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/main/iot-hub/Samples/device/PnpDeviceSamples)

En este artículo de inicio rápido, se explica un flujo de trabajo básico de desarrollo de aplicaciones de Azure IoT. En primer lugar, cree una aplicación de Azure IoT Central para hospedar dispositivos. A continuación, utilizaremos un ejemplo de un SDK de dispositivo IoT de Azure para ejecutar un controlador de temperatura simulado, conectarlo de forma segura a IoT Central y enviar datos de telemetría.

## <a name="prerequisites"></a>Requisitos previos
- [Visual Studio (Community, Professional o Enterprise) 2019](https://visualstudio.microsoft.com/downloads/).
- Una copia local del repositorio de GitHub de [ejemplos de Microsoft Azure IoT para C# (.NET)](https://github.com/Azure-Samples/azure-iot-samples-csharp). Descargue una copia del repositorio y extráigala: [Descargar archivo ZIP](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/main.zip).

[!INCLUDE [iot-develop-create-central-app-with-device](iot-develop-create-central-app-with-device.md)]

## <a name="run-a-simulated-device"></a>Ejecución de un dispositivo simulado
En esta sección, configurará el entorno local y ejecutará un ejemplo que crea un controlador de temperatura simulado.

Ejecución de la aplicación de ejemplo en Visual Studio

1. En la carpeta en la que ha descomprimido los ejemplos de Azure IoT para C#, abra el archivo de solución *azure-iot-samples-csharp-main\iot-hub\Samples\device\IoTHubDeviceSamples.sln* en Visual Studio. 

1. En el **Explorador de soluciones**, seleccione el archivo de proyecto **PnpDeviceSamples > TemperatureController**, haga clic con el botón derecho en él y seleccione **Establecer como proyecto de inicio**.

1. Haga clic con el botón derecho en el proyecto **TemperatureController**, seleccione **Propiedades**, seleccione la pestaña **Depurar** y agregue las siguientes variables de entorno al proyecto:

    | Nombre | Value |
    | ---- | ----- |
    | IOTHUB_DEVICE_SECURITY_TYPE | DPS |
    | IOTHUB_DEVICE_DPS_ENDPOINT | global.azure-devices-provisioning.net |
    | IOTHUB_DEVICE_DPS_ID_SCOPE | El valor del ámbito del identificador que anotó anteriormente. |
    | IOTHUB_DEVICE_DPS_DEVICE_ID | sample-device-01 |
    | IOTHUB_DEVICE_DPS_DEVICE_KEY | El valor de la clave de dispositivo generado que anotó anteriormente. |

1. Guarde el archivo de proyecto **TemperatureController** actualizado.

1. Presione CTRL + F5 para ejecutar el ejemplo.

    Una vez conectado el dispositivo simulado con su aplicación de IoT Central, se inicia el envío de datos de telemetría. En la consola se muestran los detalles de conexión y la salida de telemetría: 
    
    ```output
        [05/04/2021 11:53:50]info: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Press Control+C to quit the sample.
        [05/04/2021 11:53:50]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Set up the device client.
        [05/04/2021 11:53:50]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Initializing via DPS
        [05/04/2021 11:53:56]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Set handler for 'reboot' command.
        [05/04/2021 11:53:57]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Connection status change registered - status=Connected, reason=Connection_Ok.
        [05/04/2021 11:53:57]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Set handler for "getMaxMinReport" command.
        [05/04/2021 11:53:57]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Set handler to receive 'targetTemperature' updates.
        [05/04/2021 11:53:57]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Property: Update - component = 'deviceInformation', properties update is complete.
        [05/04/2021 11:53:58]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Property: Update - { "serialNumber": "SR-123456" } is complete.
        [05/04/2021 11:53:58]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Telemetry: Sent - component="thermostat1", { "temperature": 44.9 } in °C.
        [05/04/2021 11:53:58]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Property: Update - component="thermostat1", { "maxTempSinceLastReboot": 44.9 } in °C is complete.
        [05/04/2021 11:53:58]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Telemetry: Sent - component="thermostat2", { "temperature": 40.8 } in °C.
    ```