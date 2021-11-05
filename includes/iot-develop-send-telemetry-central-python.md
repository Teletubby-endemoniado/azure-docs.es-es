---
title: Archivo de inclusión
description: Archivo de inclusión
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 04/28/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: e5d83a17edd2cff6c58ee7ac646c2bdf2fe30f5e
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131861105"
---
[![Examinar el código](../articles/iot-develop/media/common/browse-code.svg)](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/pnp)

En este artículo de inicio rápido, se explica un flujo de trabajo básico de desarrollo de aplicaciones de Azure IoT. En primer lugar, cree una aplicación de Azure IoT Central para hospedar dispositivos. A continuación, usará un ejemplo del SDK de dispositivo IoT de Azure para crear un controlador de temperatura, conectarlo de forma segura a IoT Central y enviar datos de telemetría. La aplicación de ejemplo del controlador de temperatura se ejecuta en la máquina local y genera datos del sensor simulados para enviarlos a IoT Central.

## <a name="prerequisites"></a>Prerrequisitos
Este inicio rápido se ejecuta en Windows, Linux y Raspberry Pi. Se ha probado en las siguientes versiones de sistema operativo y dispositivo:

- Windows 10
- Ubuntu 20.04 LTS
- Raspberry Pi OS, versión 10 (buster), que se ejecuta en un dispositivo Raspberry Pi 3 Model B+

Instale los siguientes requisitos previos en la máquina de desarrollo:

- [Python](https://www.python.org/downloads/) versión 3.7 o cualquier versión posterior. Para comprobar la versión de Python, ejecute `python3 --version`.
- [Git](https://git-scm.com/downloads).

[!INCLUDE [iot-develop-create-central-app-with-device](iot-develop-create-central-app-with-device.md)]

## <a name="run-the-device-sample"></a>Ejecución del ejemplo de dispositivo
En esta sección, configurará el entorno local, instalará el SDK de dispositivo de Azure IoT para Python y ejecutará un ejemplo que crea un controlador de temperatura.

### <a name="configure-your-environment"></a>Configurar su entorno

1. Abra una consola como CMD de Windows, PowerShell o Bash.

1. Establezca las siguientes variables de entorno con los comandos adecuados para la consola. El dispositivo usa estos valores para conectarse a IoT Central. Para `IOTHUB_DEVICE_DPS_ID_SCOPE`, `IOTHUB_DEVICE_DPS_DEVICE_KEY` y `IOTHUB_DEVICE_DPS_DEVICE_ID`, use los valores de conexión del dispositivo que guardó anteriormente.

    **CMD (Windows)**

    ```console
    set IOTHUB_DEVICE_SECURITY_TYPE=DPS
    set IOTHUB_DEVICE_DPS_ID_SCOPE=<application ID scope>
    set IOTHUB_DEVICE_DPS_DEVICE_KEY=<device primary key>
    set IOTHUB_DEVICE_DPS_DEVICE_ID=<your device ID>
    set IOTHUB_DEVICE_DPS_ENDPOINT=global.azure-devices-provisioning.net
    ```

    > [!NOTE]
    > En el caso del símbolo de comandos de Windows, no hay comillas alrededor de los valores de variable.

    **PowerShell**

    ```azurepowershell
    $env:IOTHUB_DEVICE_SECURITY_TYPE='DPS'
    $env:IOTHUB_DEVICE_DPS_ID_SCOPE='<application ID scope>'
    $env:IOTHUB_DEVICE_DPS_DEVICE_KEY='<device primary key>'
    $env:IOTHUB_DEVICE_DPS_DEVICE_ID='<your device ID>'
    $env:IOTHUB_DEVICE_DPS_ENDPOINT='global.azure-devices-provisioning.net'
    ```

    **Bash**

    ```bash
    export IOTHUB_DEVICE_SECURITY_TYPE='DPS'
    export IOTHUB_DEVICE_DPS_ID_SCOPE='<application ID scope>'
    export IOTHUB_DEVICE_DPS_DEVICE_KEY='<device primary key>'
    export IOTHUB_DEVICE_DPS_DEVICE_ID='<your device ID>'
    export IOTHUB_DEVICE_DPS_ENDPOINT='global.azure-devices-provisioning.net' 
    ```

### <a name="install-the-sdk-and-samples"></a>Instalación del SDK y los ejemplos

1. Copie el SDK de dispositivo de Azure IoT para Python en la máquina local.

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-python
    ```
1. Vaya al directorio de ejemplo.

    **Windows**
    ```console
    cd azure-iot-sdk-python\azure-iot-device\samples\pnp
    ```

    **Linux o Raspberry Pi OS**
    ```console
    cd azure-iot-sdk-python/azure-iot-device/samples/pnp
    ```

1. Instale el SDK de Azure IoT para Python.
    ```console
    pip3 install azure-iot-device
    ```

### <a name="run-the-code"></a>Ejecución del código

1. En la consola, ejecute el siguiente ejemplo de código desde el SDK. El ejemplo crea un controlador de temperatura con sensores de termostato.
    ```console
    python3 temp_controller_with_thermostats.py
    ```

    Una vez que el dispositivo se conecta a la aplicación IoT Central, se conecta a la instancia del dispositivo que creó en la aplicación y comienza a enviar telemetría. En la consola se muestran los detalles de conexión y la salida de telemetría: 
    
    ```output
    Device was assigned
    iotc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.azure-devices.net
    my-sdk-device
    Updating pnp properties for root interface
    {'serialNumber': 'alohomora'}
    Updating pnp properties for thermostat1
    {'thermostat1': {'maxTempSinceLastReboot': 98.34, '__t': 'c'}}
    Updating pnp properties for thermostat2
    {'thermostat2': {'maxTempSinceLastReboot': 48.92, '__t': 'c'}}
    Updating pnp properties for deviceInformation
    {'deviceInformation': {'swVersion': '5.5', 'manufacturer': 'Contoso Device Corporation', 'model': 'Contoso 4762B-turbo', 'osName': 'Mac Os', 'processorArchitecture': 'x86-64', 'processorManufacturer': 'Intel', 'totalStorage': 1024, 'totalMemory': 32, '__t': 'c'}}
    Listening for command requests and property updates
    Press Q to quit
    Sending telemetry from various components
    Sent message
    {"temperature": 33}
    ```