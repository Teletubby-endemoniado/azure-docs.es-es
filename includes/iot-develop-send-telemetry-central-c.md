---
title: Archivo de inclusión
description: Archivo de inclusión
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 09/10/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: 7f02dba36ceee1d3acba9f359f26fc3bbe7aa1ae
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131861072"
---
[![Examinar el código](../articles/iot-develop/media/common/browse-code.svg)](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/pnp)

En este artículo de inicio rápido, se explica un flujo de trabajo básico de desarrollo de aplicaciones de Azure IoT. En primer lugar, cree una aplicación de Azure IoT Central para hospedar dispositivos. A continuación, usará un ejemplo del SDK de dispositivo IoT de Azure para crear un controlador de temperatura, conectarlo de forma segura a IoT Central y enviar datos de telemetría. La aplicación de ejemplo del controlador de temperatura se ejecuta en la máquina local y genera datos del sensor simulados para enviarlos a IoT Central.

## <a name="prerequisites"></a>Prerrequisitos
Este inicio rápido se ejecuta en Windows, Linux y Raspberry Pi. Se ha probado en las siguientes versiones de sistema operativo y dispositivo:

- Windows 10
- Ubuntu 20.04 LTS
- Raspberry Pi OS, versión 10 (buster), que se ejecuta en un dispositivo Raspberry Pi 3 Model B+

Instale los requisitos previos restantes para su sistema operativo.

### <a name="linux-or-raspberry-pi-os"></a>Linux o Raspberry Pi OS
Para realizar este inicio rápido en Linux y Raspberry Pi OS, instale el software siguiente:

Instale **GCC**, **Git**, **cmake** y las dependencias necesarias con el comando `apt-get`:

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Compruebe que la versión de `cmake` es posterior a **2.8.12** y que la versión de **GCC** es posterior a **4.4.7**.

```sh
cmake --version
gcc --version
```

### <a name="windows"></a>Windows
Para completar este inicio rápido en Windows, instale Visual Studio 2019 y agregue los componentes necesarios para el desarrollo en C y C++.

1. Para los nuevos usuarios, instale [Visual Studio (Community, Professional o Enterprise) 2019](https://visualstudio.microsoft.com/downloads/). Descargue la edición que desea instalar e inicie el instalador.
    > [!NOTE]
    > Para los usuarios de Visual Studio 2019 existentes, seleccione **Inicio** de Windows, escriba *Instalador de Visual Studio* e inicie el instalador.
1. En la pestaña **Cargas de trabajo** del instalador, seleccione la carga de trabajo **Desarrollo para el escritorio con C++** .
1. En la pestaña **Componentes individuales** del instalador, seleccione **Git para Windows**.
1. Ejecución de la instalación.

[!INCLUDE [iot-develop-create-central-app-with-device](iot-develop-create-central-app-with-device.md)]

## <a name="run-the-device-sample"></a>Ejecución del ejemplo de dispositivo
En esta sección, configurará el entorno local, instalará el SDK de dispositivo de Azure IoT para C y ejecutará un ejemplo que crea un controlador de temperatura.

### <a name="configure-your-environment"></a>Configurar su entorno

1. Abra una consola para instalar el SDK de dispositivo de C de Azure IoT y ejecute el ejemplo de código. En Windows, seleccione **Inicio**, escriba *Símbolo del sistema para desarrolladores para VS 2019* y abra la consola. En Linux y Raspberry Pi OS, abra un terminal de comandos de Bash. 

1. Establezca las siguientes variables de entorno con los comandos adecuados para la consola. El dispositivo usa estos valores para conectarse a IoT Central. Para `IOTHUB_DEVICE_DPS_ID_SCOPE`, `IOTHUB_DEVICE_DPS_DEVICE_KEY` y `IOTHUB_DEVICE_DPS_DEVICE_ID`, use los valores de conexión del dispositivo que guardó anteriormente.

    **CMD**

    ```console
    set IOTHUB_DEVICE_SECURITY_TYPE=DPS
    set IOTHUB_DEVICE_DPS_ID_SCOPE=<application ID scope>
    set IOTHUB_DEVICE_DPS_DEVICE_KEY=<device primary key>
    set IOTHUB_DEVICE_DPS_DEVICE_ID=<your device ID>
    set IOTHUB_DEVICE_DPS_ENDPOINT=global.azure-devices-provisioning.net
    ```

    > [!NOTE]
    > En el caso del símbolo de comandos de Windows, no hay comillas alrededor de los valores de variable.

    **Bash**

    ```bash
    export IOTHUB_DEVICE_SECURITY_TYPE='DPS'
    export IOTHUB_DEVICE_DPS_ID_SCOPE='<application ID scope>'
    export IOTHUB_DEVICE_DPS_DEVICE_KEY='<device primary key>'
    export IOTHUB_DEVICE_DPS_DEVICE_ID='<your device ID>'
    export IOTHUB_DEVICE_DPS_ENDPOINT='global.azure-devices-provisioning.net' 
    ```

### <a name="install-the-sdk-and-samples"></a>Instalación del SDK y los ejemplos

1. Cambie a la carpeta en la que quiere clonar el repositorio de ejemplo.

1. Copie el SDK de dispositivo de Azure IoT para C en la máquina local.

    ```console
    git clone https://github.com/Azure/azure-iot-sdk-c.git
    ```

1. Vaya a la carpeta raíz del SDK y ejecute el siguiente comando para actualizar las dependencias:
    ```console
    cd azure-iot-sdk-c
    git submodule update --init
    ```
    Esta operación tarda unos minutos.

1. Ejecute los siguientes comandos para compilar el SDK y los ejemplos:

    ```console
    cmake -Bcmake -Duse_prov_client=ON -Dhsm_type_symm_key=ON -Drun_e2e_tests=OFF
    cmake --build cmake
    ```

### <a name="run-the-code"></a>Ejecución del código

1. Ejecute el código de ejemplo con el comando adecuado para la consola:

    **CMD**
    ```console
    cmake\iothub_client\samples\pnp\pnp_temperature_controller\Debug\pnp_temperature_controller.exe
    ```

    **Bash**
    ```bash
    cmake/iothub_client/samples/pnp/pnp_temperature_controller/pnp_temperature_controller
    ```

    Una vez que el dispositivo se conecta a la aplicación IoT Central, se conecta a la instancia del dispositivo que creó en la aplicación y comienza a enviar telemetría. En la consola se muestran los detalles de conexión y la salida de telemetría: 
    
    ```output
    Info: Initiating DPS client to retrieve IoT Hub connection information
    -> 17:03:08 CONNECT | VER: 4 | KEEPALIVE: 0 | FLAGS: 194 | USERNAME: xxxxxxxxxxxxxxx/registrations/my-sdk-device/api-version=2019-03-31&ClientVersion=1.6.0 | PWD: XXXX | CLEAN: 1
    <- 17:03:09 CONNACK | SESSION_PRESENT: false | RETURN_CODE: 0x0
    -> 17:03:10 SUBSCRIBE | PACKET_ID: 1 | TOPIC_NAME: $dps/registrations/res/# | QOS: 1
    <- 17:03:11 SUBACK | PACKET_ID: 1 | RETURN_CODE: 1
    Info: Provisioning callback indicates success.  iothubUri=iotc-xxxxxxxxxxxx-xxxx-xxxx-xxxxxxxxxxxx.azure-devices.net, deviceId=my-sdk-device
    -> 17:03:27 DISCONNECT
    Info: DPS successfully registered.  Continuing on to creation of IoTHub device client handle.
    Info: Successfully created device client.  Hit Control-C to exit program
    
    Info: Sending serialNumber property to IoTHub
    Info: Sending device information property to IoTHub.  propertyName=swVersion, propertyValue="1.0.0.0"
    Info: Sending device information property to IoTHub.  propertyName=manufacturer, propertyValue="Sample-Manufacturer"
    Info: Sending device information property to IoTHub.  propertyName=model, propertyValue="sample-Model-123"
    Info: Sending device information property to IoTHub.  propertyName=osName, propertyValue="sample-OperatingSystem-name"
    Info: Sending device information property to IoTHub.  propertyName=processorArchitecture, propertyValue="Contoso-Arch-64bit"
    Info: Sending device information property to IoTHub.  propertyName=processorManufacturer, propertyValue="Processor Manufacturer(TM)"
    Info: Sending device information property to IoTHub.  propertyName=totalStorage, propertyValue=10000
    Info: Sending device information property to IoTHub.  propertyName=totalMemory, propertyValue=200
    Info: Sending maximumTemperatureSinceLastReboot property to IoTHub for component=thermostat1
    Info: Sending maximumTemperatureSinceLastReboot property to IoTHub for component=thermostat2
    ```