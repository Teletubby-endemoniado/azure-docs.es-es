---
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: include
ms.custom:
- mvc
- mqtt
- devx-track-python
- devx-track-azurecli
ms.date: 09/14/2020
ms.openlocfilehash: 8e9388f8b929743dddde0db21270e92eddf08a40
ms.sourcegitcommit: f2eb1bc583962ea0b616577f47b325d548fd0efa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2021
ms.locfileid: "114732288"
---
 En este inicio rápido se usan dos aplicaciones de Python: una aplicación de un dispositivo simulado que responde a métodos directos llamados desde una aplicación de back-end y una aplicación de back-end que llama a los métodos directos en el dispositivo simulado.

## <a name="prerequisites"></a>Prerrequisitos

* Una cuenta de Azure con una suscripción activa. [cree una de forma gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

* [Python 3.7+](https://www.python.org/downloads/). Para obtener información acerca de otras versiones de Python compatibles, consulte el tema acerca de las [características de los dispositivos IoT de Azure](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device#azure-iot-device-features).

* [Un proyecto de Python de ejemplo](https://github.com/Azure-Samples/azure-iot-samples-python/) de GitHub. Descargue o clone los ejemplos mediante el botón **Código** del repositorio de GitHub.

* El puerto 8883 abierto en el firewall. En el dispositivo de ejemplo de este inicio rápido se usa el protocolo MQTT, que se comunica mediante el puerto 8883. Este puerto puede estar bloqueado en algunos entornos de red corporativos y educativos. Para más información y para saber cómo solucionar este problema, consulte el artículo sobre la [conexión a IoT Hub (MQTT)](../articles/iot-hub/iot-hub-mqtt-support.md#connecting-to-iot-hub).

[!INCLUDE [azure-cli-prepare-your-environment.md](azure-cli-prepare-your-environment-no-header.md)]

[!INCLUDE [iot-hub-cli-version-info](iot-hub-cli-version-info.md)]

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Si ha completado el anterior [Quickstart: Send telemetry from a device to an IoT hub](../articles/iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-python) (Inicio rápido: enviar datos de telemetría desde un dispositivo a IoT Hub), puede omitir este paso.

[!INCLUDE [iot-hub-include-create-hub](iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Registrar un dispositivo

Si ha completado el anterior [Quickstart: Send telemetry from a device to an IoT hub](../articles/iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-python) (Inicio rápido: enviar datos de telemetría desde un dispositivo a IoT Hub), puede omitir este paso.

Debe registrar un dispositivo con IoT Hub antes de poder conectarlo. En esta guía de inicio rápido, usará Azure Cloud Shell para registrar un dispositivo simulado.

1. Ejecute los siguientes comandos en Azure Cloud Shell para crear la identidad del dispositivo.

    **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    **MyPythonDevice**: es el nombre del dispositivo que se va a registrar. Se recomienda usar **MyPythonDevice** como se muestra. Si elige otro nombre distinto para el dispositivo, tendrá que usarlo en todo el artículo y actualizar el nombre del dispositivo en las aplicaciones de ejemplo antes de ejecutarlas.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyPythonDevice
    ```

2. Ejecute los siguientes comandos en Azure Cloud Shell para obtener la _cadena de conexión del dispositivo_ que acaba de registrar:

    **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    ```azurecli-interactive
    az iot hub device-identity connection-string show --hub-name {YourIoTHubName} --device-id MyPythonDevice --output table
    ```

    Anote la cadena de conexión del dispositivo, que se parecerá a esta:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Usará este valor más adelante en este inicio rápido.

3. También necesita una _cadena de conexión de servicio_ para permitir que la aplicación back-end se conecte a IoT Hub y recupere los mensajes. El comando siguiente recupera la cadena de conexión del servicio de su instancia de IoT Hub:

    **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    ```azurecli-interactive
    az iot hub connection-string show \
      --policy-name service \
      --hub-name {YourIoTHubName} \
      --output table
    ```

    Anote la cadena de conexión del servicio, que se parecerá a esta:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

    Usará este valor más adelante en este inicio rápido. Esta cadena de conexión del servicio no es la que anotó en el paso anterior.

## <a name="listen-for-direct-method-calls"></a>Escuchas para llamadas de método directo

La aplicación del dispositivo simulado se conecta a un punto de conexión específico del dispositivo en IoT Hub, envía los datos de telemetría simulados y escucha llamadas de método directo desde el centro. En este inicio rápido, la llamada de método directo desde el centro indica al dispositivo que debe cambiar el intervalo en el que envía los datos de telemetría. El dispositivo simulado envía una confirmación al centro después de que ejecuta el método directo.

1. En una ventana de terminal local, vaya a la carpeta raíz del proyecto de Python de muestra. A continuación, vaya a la carpeta **iot-hub\Quickstarts\simulated-device-2**.

1. Abra el archivo **SimulatedDevice.py** en el editor de texto de su elección.

    Reemplace el valor de la variable `CONNECTION_STRING` por la cadena de conexión del dispositivo que anotó anteriormente. Luego, guarde los cambios realizados en **SimulatedDevice.py**.

1. En la ventana de terminal local, ejecute los comandos siguientes para instalar las bibliotecas necesarias para la aplicación de dispositivo simulado:

    ```cmd/sh
    pip install azure-iot-device
    ```

1. En la ventana de terminal local, ejecute los comandos siguientes para ejecutar la aplicación de dispositivo simulado:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    La siguiente captura de pantalla muestra la salida en la que la aplicación de dispositivo simulado envía datos de telemetría a IoT Hub:

    ![Ejecutar el dispositivo simulado](./media/quickstart-control-device-python/simulated-device-1.png)

## <a name="call-the-direct-method"></a>Llamar al método directo

La aplicación back-end se conecta a un punto de conexión de servicio en IoT Hub. La aplicación realiza llamadas de método directo a un dispositivo con IoT Hub y escucha las confirmaciones. Normalmente, una aplicación back-end de IoT Hub se ejecuta en la nube.

1. En otra ventana de terminal local, vaya a la carpeta raíz del proyecto de Python de muestra. A continuación, vaya a la carpeta **iot-hub\Quickstarts\back-end-application**.

1. Abra el archivo **BackEndApplication.py** en el editor de texto de su elección.

    Reemplace el valor de la variable `CONNECTION_STRING` por la cadena de conexión del servicio que anotó anteriormente. Luego, guarde los cambios en el archivo **BackEndApplication.py**.

1. En la ventana de terminal local, ejecute los comandos siguientes para instalar las bibliotecas necesarias para la aplicación de dispositivo simulado:

    ```cmd/sh
    pip install azure-iot-hub
    ```

1. En la ventana de terminal local, ejecute los comandos siguientes para ejecutar la aplicación back-end:

    ```cmd/sh
    python BackEndApplication.py
    ```

    En la siguiente captura de pantalla se muestra la salida en la que la aplicación realiza una llamada de método directo al dispositivo y recibe una confirmación:

    ![Ejecutar la aplicación back-end](./media/quickstart-control-device-python/backend-application.png)

    Después de ejecutar la aplicación back-end, verá un mensaje en la ventana de consola que ejecuta el dispositivo simulado y cambiará la velocidad a la que envía mensajes:

    ![Cambio en el cliente simulado](./media/quickstart-control-device-python/simulated-device-2.png)
