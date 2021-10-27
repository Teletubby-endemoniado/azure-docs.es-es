---
title: Inicio rápido de conexión de ATSAME54-XPro de Microchip a Azure IoT Central
description: Use software insertado de Azure RTOS para conectar un dispositivo ATSAME54-XPro de Microchip a Azure IoT y enviar telemetría.
author: timlt
ms.author: timlt
ms.service: iot-develop
ms.devlang: c
ms.topic: quickstart
ms.date: 10/18/2021
zone_pivot_groups: iot-develop-toolset
ms.openlocfilehash: 2b2b0cdb4c81a8ca501c58fce9ddb4771a5bbaba
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130167664"
---
# <a name="quickstart-connect-a-microchip-atsame54-xpro-evaluation-kit-to-iot-central"></a>Inicio rápido: Conexión de un kit de evaluación ATSAME54-XPro de Microchip a IoT Central

**Se aplica a**: [desarrollo de dispositivos insertados](about-iot-develop.md#embedded-device-development)<br>
**Tiempo total para realizarlo**: 45 minutos

:::zone pivot="iot-toolset-cmake"
[![Examinar el código](media/common/browse-code.svg)](https://github.com/azure-rtos/getting-started/tree/master/Microchip/ATSAME54-XPRO)
:::zone-end
:::zone pivot="iot-toolset-iar-ewarm, iot-toolset-mplab"
[![Examinar el código](media/common/browse-code.svg)](https://github.com/azure-rtos/samples/)
:::zone-end

En este artículo de inicio rápido, usará Azure RTOS conectar ATSAME54-XPro de Microchip (en adelante, Microchip E54) a Azure IoT.

Deberá completar las siguientes tareas:

* Instalar un conjunto de herramientas de desarrollo insertadas para programar un Microchip E54 en C
* Compilar una imagen y guardarla en Microchip E54
* Usar Azure IoT Central para crear componentes de nube, ver las propiedades, ver la telemetría de los dispositivos y llamar a comandos directos

:::zone pivot="iot-toolset-cmake"

## <a name="prerequisites"></a>Requisitos previos

* Un equipo con Microsoft Windows 10
* [Git](https://git-scm.com/downloads) para clonar el repositorio
* Hardware

  * [ATSAME54-XPro de Microchip](https://www.microchip.com/developmenttools/productdetails/atsame54-xpro) (Microchip E54)
  * Conector USB 2.0 A macho a cable macho micro USB
  * Acceso a Ethernet por cable
  * Cable Ethernet
  * Opcional: Sensor [Weather Click](https://www.mikroe.com/weather-click). Puede agregar este sensor al dispositivo para supervisar las condiciones meteorológicas. Incluso si no tiene este sensor, puede completar este artículo de inicio rápido.
  * Opcional: Adaptador [mikroBUS Xplained Pro](https://www.microchip.com/Developmenttools/ProductDetails/ATMBUSADAPTER-XPRO). Use este adaptador para conectar el sensor Weather Click a Microchip E54. Incluso si no tiene el sensor y este adaptador, puede completar este artículo inicio rápido.

## <a name="prepare-the-development-environment"></a>Preparación del entorno de desarrollo

Para configurar el entorno de desarrollo, primero tiene que clonar un repositorio de GitHub que contenga todos los recursos que necesita para el inicio rápido. Luego, instalará un conjunto de herramientas de programación.

### <a name="clone-the-repo-for-the-quickstart"></a>Clonación del repositorio para el inicio rápido

Clone el siguiente repositorio para descargar todo el código de dispositivo de ejemplo, los scripts de instalación y las versiones sin conexión de la documentación. Si ya ha clonado anteriormente este repositorio en otro inicio rápido, no es necesario que lo haga de nuevo.

Para clonar el repositorio, ejecute el siguiente comando:

```shell
git clone --recursive https://github.com/azure-rtos/getting-started.git
```

### <a name="install-the-tools"></a>Instalar las herramientas

El repositorio clonado contiene un script de instalación que instala y configura las herramientas necesarias. Si ha instalado estas herramientas en otro inicio rápido de dispositivo insertado, no es necesario volver a hacerlo.

> [!NOTE]
> El script de instalación instala las siguientes herramientas:
>
> * [CMake](https://cmake.org): compilación
> * [ARM GCC](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm): compilación
> * [Termite](https://www.compuphase.com/software_termite.htm): supervise la salida del puerto serie de los dispositivos conectados.

Para instalar las herramientas:

1. En el Explorador de archivos, vaya a la siguiente ruta de acceso en el repositorio y ejecute el script de instalación denominado ***get-toolchain.bat***:

    *getting-started\tools\get-toolchain.bat*

1. Después de la instalación, abra una nueva ventana de consola para reconocer los cambios de configuración realizados con el script de instalación. Use esta consola para realizar el resto de las tareas de programación del inicio rápido. Puede usar la línea de comandos de Windows, PowerShell o Git Bash para Windows.
1. Ejecute el código siguiente para confirmar la instalación de CMake versión 3.14 o posterior.

    ```shell
    cmake --version
    ```

1. Instale [Microchip Studio for AVR&reg; and SAM devices](https://www.microchip.com/en-us/development-tools-tools-and-software/microchip-studio-for-avr-and-sam-devices#). Microchip Studio es un entorno de desarrollo de dispositivos que incluye las herramientas para programar y guardar imágenes en Microchip E54. En este tutorial, usará Microchip Studio solo para guardar la imagen en Microchip E54. La instalación tarda varios minutos y le pide varias veces que apruebe la instalación de los componentes.

[!INCLUDE [iot-develop-embedded-create-central-app-with-device](../../includes/iot-develop-embedded-create-central-app-with-device.md)]

## <a name="prepare-the-device"></a>Preparar el dispositivo

Para conectar Microchip E54 a Azure, modificará un archivo de configuración de las opciones de Azure IoT, recompilará la imagen y la guardará en el dispositivo.

### <a name="add-configuration"></a>Adición de configuración

1. Abra el siguiente archivo en un editor de texto:

    *getting-started\Microchip\ATSAME54-XPRO\app\azure_config.h*

1. Establezca las constantes de información del dispositivo de Azure IoT en los valores que guardó después de crear los recursos de Azure.

    |Nombre invariable|Value|
    |-------------|-----|
    | `IOT_DPS_ID_SCOPE` | {*el valor de ámbito de id.* } |
    | `IOT_DPS_REGISTRATION_ID` | {*el valor de identificador de dispositivo*} |
    | `IOT_DEVICE_SAS_KEY` | {*el valor de la clave principal*} |

1. Guarde y cierre el archivo.

### <a name="connect-the-device"></a>Conexión del dispositivo

1. En Microchip E54, busque el botón **Reset**, el puerto **Ethernet** y el puerto microUSB, que tiene la etiqueta **Debug USB**. Todos los componentes se resaltan en la siguiente imagen:

    ![Búsqueda de componentes clave en la placa del kit de evaluación de Microchip E54](media/quickstart-devkit-microchip-atsame54-xpro/microchip-xpro-board.png)

1. Conecte el cable microUSB al puerto **Debug USB** de Microchip E54 y, luego, conéctelo al equipo.

    > [!NOTE]
    > De manera opcional, para más información sobre cómo configurar y empezar a trabajar con Microchip E54, consulte la [guía del usuario de SAM E54 Xplained Pro](http://ww1.microchip.com/downloads/en/DeviceDoc/70005321A.pdf).

1. Use el cable Ethernet para conectar Microchip E54 a un puerto Ethernet.

### <a name="optional-install-a-weather-sensor"></a>Opcional: Instalación de un sensor meteorológico

Si tiene el sensor Weather Click y el adaptador mikroBUS Xplained Pro, siga los pasos de esta sección; de lo contrario, vaya [Compilación de la imagen](#build-the-image). Incluso si no tiene un sensor puede completar este artículo de inicio rápido. El código de ejemplo del dispositivo devuelve datos simulados si no tiene un sensor real.

1. Si tiene el sensor Weather Click y el adaptador de mikroBUS Xplained Pro, instálelos en Microchip E54 como se muestra en la siguiente fotografía:

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/sam-e54-sensor.png" alt-text="Instalación del sensor Weather Click y el adaptador mikroBUS Xplained Pro en Microchip ES4":::

1. Vuelva a abrir el archivo de configuración que editó anteriormente:

    *getting-started\Microchip\ATSAME54-XPRO\app\azure_config.h*

1. Establezca el valor de la constante `__SENSOR_BME280__` en **1**, como se muestra en el código siguiente del archivo de encabezado. Al establecer este valor, el dispositivo puede usar los datos reales del sensor Weather Click.

    `#define __SENSOR_BME280__ 1`

1. Guarde y cierre el archivo.

### <a name="build-the-image"></a>Compilación de la imagen

1. En la consola o en el Explorador de archivos, ejecute el script ***rebuild.bat*** en la siguiente ruta de acceso para compilar la imagen:

    *getting-started\Microchip\ATSAME54-XPRO\tools\rebuild.bat*

1. Una vez finalizada la compilación, confirme que el archivo binario se ha creado en la siguiente ruta de acceso:

    *getting-started\Microchip\ATSAME54-XPRO\build\app\atsame54_azure_iot.bin*

### <a name="flash-the-image"></a>Guardar la imagen en la memoria flash

1. Abra la consola **Inicio de Windows > Microchip Studio Command Prompt** y vaya a la carpeta del archivo binario de Microchip E54 que ha creado.

    *getting-started\Microchip\ATSAME54-XPRO\build\app*

1. Use la utilidad *atprogram* para guardar la imagen binaria en Microchip E54:

    ```shell
    atprogram --tool edbg --interface SWD --device ATSAME54P20A program --chiperase --file atsame54_azure_iot.bin --verify
    ```

    > [!NOTE]
    > Para más información sobre el uso de Atmel-ICE y las herramientas de atprogram con Microchip E54, consulte [Uso de Atmel-ICE para la programación de AVR en producción masiva](http://ww1.microchip.com/downloads/en/AppNotes/00002466A.pdf).

    Una vez que termine de guardar la imagen, la consola confirma que la programación se ha realizado correctamente:

    ```output
    Firmware check OK
    Programming and verification completed successfully.
    ```

### <a name="confirm-device-connection-details"></a>Confirmación de los detalles de conexión del dispositivo

Puede usar la aplicación **Termite** para supervisar la comunicación y confirmar que el dispositivo está configurado correctamente.

1. Inicie **Termite**.

    > [!TIP]
    > Si tiene problemas para que el dispositivo se inicialice o se conecte después de guardar la imagen, consulte Solución de problemas](troubleshoot-embedded-device-quickstarts.md) para ver pasos adicionales.

1. Haga clic en **Configuración**.

1. En el cuadro de diálogo **Serial port settings** (Configuración de puerto serie), compruebe los siguientes valores y actualícelos si es necesario:
    * **Baud rate** (Velocidad en baudios): 115.200
    * **Puerto**: Puerto al que está conectado Microchip E54. Si hay varias opciones de puerto en la lista desplegable, busque ahí el puerto correcto que debe usar. Abra el **Administrador de dispositivos** de Windows y, en **Puertos**, identifique qué puerto debe usar.
    * **Control de flujo**: DTR/DSR

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/termite-settings.png" alt-text="Captura de pantalla de la configuración del puerto serie en la aplicación Termite":::

1. Seleccione **Aceptar**.

1. Presione el botón **Reset** (Restablecer) en el dispositivo. El botón está rotulado en el dispositivo y se encuentra cerca del conector micro USB.

1. En la aplicación **Termite**, confirme los siguientes valores de punto de control para comprobar que el dispositivo se ha inicializado y está conectado a Azure IoT.

    ```output
    Starting Azure thread

    Initializing DHCP
        IP address: 192.168.0.21
        Mask: 255.255.255.0
        Gateway: 192.168.0.1
    SUCCESS: DHCP initialized

    Initializing DNS client
        DNS address: 75.75.75.75
    SUCCESS: DNS client initialized

    Initializing SNTP client
        SNTP server 0.pool.ntp.org
        SNTP IP address: 45.55.58.103
        SNTP time update: Jun 5, 2021 20:2:46.32 UTC 
    SUCCESS: SNTP initialized

    Initializing Azure IoT DPS client
        DPS endpoint: global.azure-devices-provisioning.net
        DPS ID scope: ***
        Registration ID: mydevice
    SUCCESS: Azure IoT DPS client initialized

    Initializing Azure IoT Hub client
        Hub hostname: ***.azure-devices.net
        Device id: mydevice
        Model id: dtmi:azurertos:devkit:gsg;1
    Connected to IoT Hub
    SUCCESS: Azure IoT Hub client initialized
    ```

Mantenga abierto Termite para supervisar la salida del dispositivo en los pasos siguientes.

:::zone-end
:::zone pivot="iot-toolset-iar-ewarm"

## <a name="prerequisites"></a>Requisitos previos

* Un equipo con Microsoft Windows 10

* Hardware

  * [ATSAME54-XPro de Microchip](https://www.microchip.com/developmenttools/productdetails/atsame54-xpro) (Microchip E54)
  * Conector USB 2.0 A macho a cable macho micro USB
  * Acceso a Ethernet por cable
  * Cable Ethernet

* [Termite](https://www.compuphase.com/software_termite.htm). En la página web, en **Downloads and license** (Descargas y licencia), seleccione la instalación completa. Termite es un terminal RS232 que usará para supervisar la salida del dispositivo.

* IAR Embedded Workbench for ARM (EW for ARM). Puede descargar e instalar una [evaluación gratuita de 14 días de IAR EW for ARM](https://www.iar.com/products/architectures/arm/iar-embedded-workbench-for-arm/).

* Descargue el archivo [Azure_RTOS_6.1_ATSAME54-XPRO_IAR_Samples_2020_10_10.zip](https://github.com/azure-rtos/samples/releases/download/v6.1_rel/Azure_RTOS_6.1_ATSAME54-XPRO_IAR_Samples_2020_10_10.zip) y extráigalo en un directorio de trabajo. Elija un directorio con una ruta de acceso corta para evitar errores del compilador al compilar.

[!INCLUDE [iot-develop-embedded-create-central-app-with-device](../../includes/iot-develop-embedded-create-central-app-with-device.md)]

## <a name="prepare-the-device"></a>Preparar el dispositivo

Para conectar Microchip E54 a Azure, lo conectará a su equipo, modificará un archivo de configuración de las opciones de Azure IoT, recompilará la imagen y la guardará en la memoria flash del dispositivo.

### <a name="connect-the-device"></a>Conexión del dispositivo

1. En Microchip E54, busque el botón **Reset**, el puerto **Ethernet** y el puerto microUSB, que tiene la etiqueta **Debug USB**. Todos los componentes se resaltan en la siguiente imagen:

    ![Búsqueda de componentes clave en la placa del kit de evaluación de Microchip E54](media/quickstart-devkit-microchip-atsame54-xpro/microchip-xpro-board.png)

1. Conecte el cable microUSB al puerto **Debug USB** de Microchip E54 y, luego, conéctelo al equipo.

    > [!NOTE]
    > De manera opcional, para más información sobre cómo configurar y empezar a trabajar con Microchip E54, consulte la [guía del usuario de SAM E54 Xplained Pro](http://ww1.microchip.com/downloads/en/DeviceDoc/70005321A.pdf).

1. Use el cable Ethernet para conectar Microchip E54 a un puerto Ethernet.

### <a name="configure-termite"></a>Configuración de Termite

Usará la aplicación **Termite** para supervisar la comunicación y confirmar que el dispositivo está configurado correctamente. En esta sección, va a configurar **Termite** para supervisar el puerto serie del dispositivo.

1. Inicie **Termite**.

1. Haga clic en **Configuración**.

1. En el cuadro de diálogo **Serial port settings** (Configuración de puerto serie), compruebe los siguientes valores y actualícelos si es necesario:
    * **Baud rate** (Velocidad en baudios): 115.200
    * **Puerto**: Puerto al que está conectado Microchip E54. Si hay varias opciones de puerto en la lista desplegable, busque ahí el puerto correcto que debe usar. Abra el **Administrador de dispositivos** de Windows y, en **Puertos**, identifique qué puerto debe usar.
    * **Control de flujo**: DTR/DSR

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/termite-settings.png" alt-text="Captura de pantalla de la configuración del puerto serie en la aplicación Termite":::

1. Seleccione **Aceptar**.

Termite ya está listo para recibir la salida de Microchip E54.

### <a name="configure-build-flash-and-run-the-image"></a>Configurar, compilar, guardar en la memoria flash y ejecutar la imagen

1. Abra la aplicación **IAR EW for ARM** en el equipo.

1. Seleccione **File > Open workspace** (Archivo > Abrir área de trabajo), vaya a la carpeta **same54Xpro\iar** que se encuentra fuera de la carpeta de trabajo en la que extrajo el archivo ZIP y abra el área de trabajo **_azure_rtos.eww_** de EWARM.

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/open-project-iar.png" alt-text="Apertura del área de trabajo de IAR":::

1. Haga clic con el botón derecho en el proyecto **sample_azure_iot_embedded_sdk_pnp** en el panel izquierdo **Workspace** (Área de trabajo) y seleccione **Set as active** (Establecer como activa).

1. Expanda el ejemplo, después, expanda la carpeta **Sample** (Ejemplo) y abra el archivo sample_config.h.

1. Cerca de la parte superior del archivo, quite la marca de comentario de la directiva `#define ENABLE_DPS_SAMPLE`.

    ```c
    #define ENABLE_DPS_SAMPLE
    ```

1. Establezca las constantes de información del dispositivo de Azure IoT en los valores que guardó después de crear los recursos de Azure. La constante `ENDPOINT` se establece en el punto de conexión global de Azure Device Provisioning Service (DPS).

    |Nombre invariable|Value|
    |-------------|-----|
    | `ENDPOINT` | "global.azure-devices-provisioning.net" |
    | `ID_SCOPE` | {*el valor de ámbito de id.* } |
    | `REGISTRATION_ID` | {*el valor de identificador de dispositivo*} |
    | `DEVICE_SYMMETRIC_KEY` | {*el valor de la clave principal*} |

    > [!NOTE]
    > Los valores `ENDPOINT`, `ID_SCOPE` y `REGISTRATION_ID` se establecen en una instrucción `#ifndef ENABLE_DPS_SAMPLE`. Asegúrese de que establece los valores en la instrucción `#else` que se usará cuando el valor `ENABLE_DPS_SAMPLE` se defina.

1. Guarde el archivo.

1. Seleccione **Project > Batch Build** (Proyecto > Generación por lotes). A continuación, seleccione **build_all** y **Make** (Crear) para crear todos los proyectos. Verá la salida de compilación en el panel **Build** (Compilar). Confirme la compilación correcta y la vinculación de todos los proyectos de ejemplo.

1. Seleccione el botón verde **Download and Debug** (Descargar y depurar) en la barra de herramientas para descargar el programa.

1. Una vez que la imagen haya terminado de descargarse, seleccione **Go** (Ir) para ejecutar el ejemplo.

### <a name="confirm-device-connection-details"></a>Confirmación de los detalles de conexión del dispositivo

En la aplicación **Termite**, confirme los siguientes valores de punto de control para comprobar que el dispositivo se ha inicializado y está conectado a Azure IoT.

```output
DHCP In Progress...
IP address: 192.168.0.22
Mask: 255.255.255.0
Gateway: 192.168.0.1
DNS Server address: 75.75.75.75
SNTP Time Sync...
SNTP Time Sync successfully.
[INFO] Azure IoT Security Module has been enabled, status=0
Start Provisioning Client...
[INFO] IoTProvisioning client connect pending
Registered Device Successfully.
IoTHub Host Name: iotc-597ffb0b-3dbe-4784-ba3c-fdefd120a44a.azure-devices.net; Device ID: mydevice.
Connected to IoTHub.
Telemetry message send: {"temperature":22}.
Receive twin properties: {"desired":{"$version":1},"reported":{"maxTempSinceLastReboot":22,"$version":8}}
Failed to parse value
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
```

Mantenga abierto Termite para supervisar la salida del dispositivo en los pasos siguientes.

:::zone-end
:::zone pivot="iot-toolset-mplab"

## <a name="prerequisites"></a>Requisitos previos

* Un equipo con Microsoft Windows 10

* Hardware

  * [ATSAME54-XPro de Microchip](https://www.microchip.com/developmenttools/productdetails/atsame54-xpro) (Microchip E54)
  * Conector USB 2.0 A macho a cable macho micro USB
  * Acceso a Ethernet por cable
  * Cable Ethernet

* [Termite](https://www.compuphase.com/software_termite.htm). En la página web, en **Downloads and license** (Descargas y licencia), seleccione la instalación completa. Termite es un terminal RS232 que usará para supervisar la salida del dispositivo.

* [MPLAB X IDE 5.35](https://www.microchip.com/mplab/mplab-x-ide).

* [MPLAB XC32/32++ compilador 2.4.0 o posterior](https://www.microchip.com/mplab/compilers).

* Descargue el archivo [Azure_RTOS_6.1_ATSAME54-XPRO_MPLab_Samples_2020_10_10.zip](https://github.com/azure-rtos/samples/releases/download/v6.1_rel/Azure_RTOS_6.1_ATSAME54-XPRO_MPLab_Samples_2020_10_10.zip) y extráigalo en un directorio de trabajo. Elija un directorio con una ruta de acceso corta para evitar errores del compilador al compilar.

[!INCLUDE [iot-develop-embedded-create-central-app-with-device](../../includes/iot-develop-embedded-create-central-app-with-device.md)]

## <a name="prepare-the-device"></a>Preparar el dispositivo

Para conectar Microchip E54 a Azure, lo conectará a su equipo, modificará un archivo de configuración de las opciones de Azure IoT, recompilará la imagen y la guardará en la memoria flash del dispositivo.

### <a name="connect-the-device"></a>Conexión del dispositivo

1. En Microchip E54, busque el botón **Reset**, el puerto **Ethernet** y el puerto microUSB, que tiene la etiqueta **Debug USB**. Todos los componentes se resaltan en la siguiente imagen:

    ![Búsqueda de componentes clave en la placa del kit de evaluación de Microchip E54](media/quickstart-devkit-microchip-atsame54-xpro/microchip-xpro-board.png)

1. Conecte el cable microUSB al puerto **Debug USB** de Microchip E54 y, luego, conéctelo al equipo.

    > [!NOTE]
    > De manera opcional, para más información sobre cómo configurar y empezar a trabajar con Microchip E54, consulte la [guía del usuario de SAM E54 Xplained Pro](http://ww1.microchip.com/downloads/en/DeviceDoc/70005321A.pdf).

1. Use el cable Ethernet para conectar Microchip E54 a un puerto Ethernet.

### <a name="configure-termite"></a>Configuración de Termite

Usará la aplicación **Termite** para supervisar la comunicación y confirmar que el dispositivo está configurado correctamente. En esta sección, va a configurar **Termite** para supervisar el puerto serie del dispositivo.

1. Inicie **Termite**.

1. Haga clic en **Configuración**.

1. En el cuadro de diálogo **Serial port settings** (Configuración de puerto serie), compruebe los siguientes valores y actualícelos si es necesario:
    * **Baud rate** (Velocidad en baudios): 115.200
    * **Puerto**: Puerto al que está conectado Microchip E54. Si hay varias opciones de puerto en la lista desplegable, busque ahí el puerto correcto que debe usar. Abra el **Administrador de dispositivos** de Windows y, en **Puertos**, identifique qué puerto debe usar.
    * **Control de flujo**: DTR/DSR

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/termite-settings.png" alt-text="Captura de pantalla de la configuración del puerto serie en la aplicación Termite":::

1. Seleccione **Aceptar**.

Termite ya está listo para recibir la salida de Microchip E54.

### <a name="configure-build-flash-and-run-the-image"></a>Configurar, compilar, guardar en la memoria flash y ejecutar la imagen

1. Abra **MPLAB X IDE** en el equipo.

1. Seleccione **File > Open project** (Archivo > Abrir proyecto). En el cuadro de diálogo Abrir proyecto, vaya a la carpeta **same54Xpro\mplab** fuera de la carpeta de trabajo donde extrajo el archivo ZIP. Seleccione todos los proyectos (no seleccione las carpetas **common_hardware_code** ni **docs**) y, a continuación, seleccione **Open Project** (Abrir proyecto).

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/open-project-mplab.png" alt-text="Abrir proyectos en el IDE de MPLab":::

1. Haga clic con el botón derecho en el proyecto **sample_azure_iot_embedded_sdk_pnp** en el panel izquierdo **Projects** (Proyectos) y seleccione **Set as Main Project** (Establecer como proyecto principal).

1. Expanda el proyecto **sample_azure_iot_embedded_sdk_pnp**, después expanda la carpeta **Header Files** y abra el archivo sample_config.h.

1. Cerca de la parte superior del archivo, quite la marca de comentario de la directiva `#define ENABLE_DPS_SAMPLE`.

    ```c
    #define ENABLE_DPS_SAMPLE
    ```

1. Establezca las constantes de información del dispositivo de Azure IoT en los valores que guardó después de crear los recursos de Azure. La constante `ENDPOINT` se establece en el punto de conexión global de Azure Device Provisioning Service (DPS).

    |Nombre invariable|Value|
    |-------------|-----|
    | `ENDPOINT` | "global.azure-devices-provisioning.net" |
    | `ID_SCOPE` | {*el valor de ámbito de id.* } |
    | `REGISTRATION_ID` | {*el valor de identificador de dispositivo*} |
    | `DEVICE_SYMMETRIC_KEY` | {*el valor de la clave principal*} |

    > [!NOTE]
    > Los valores `ENDPOINT`, `ID_SCOPE` y `REGISTRATION_ID` se establecen en una instrucción `#ifndef ENABLE_DPS_SAMPLE`. Asegúrese de que establece los valores en la instrucción `#else` que se usará cuando el valor `ENABLE_DPS_SAMPLE` se defina.

1. Guarde el archivo.

1. Antes de compilar el ejemplo, debe compilar las bibliotecas dependientes del proyecto **sample_azure_iot_embedded_pnp**: **threadx**, **netxduo** y **same54_lib**. Para compilar cada biblioteca, haga clic con el botón derecho en su proyecto en el panel **Projects** (Proyectos) y seleccione **Build** (Compilar). Espere a que se complete cada compilación antes de pasar a la biblioteca siguiente.

1. Después de que todas las bibliotecas de los requisitos previos se hayan compilado correctamente, haga clic con el botón derecho en el proyecto **sample_azure_iot_embedded_pnp** y seleccione **Build** (Compilar).

1. Seleccione **Debug > Debug Main Project** (Depurar > Depurar proyecto principal) en el menú superior para descargar e iniciar el programa.

1. Si aparece el cuadro de diálogo **Tool not Found** (No se encontró la herramienta), seleccione **connect SAM E54 board** (conectar panel de SAM E54) y, después, seleccione **OK** (Aceptar).

1. El programa puede tardar unos minutos en descargarse y empezar a ejecutarse. Una vez que el programa se haya descargado correctamente y se esté ejecutando, verá el siguiente estado en el panel **Output** (Salida) del IDE de MPLAB X.

    ```output
    Programming complete
    
    Running
    ```

### <a name="confirm-device-connection-details"></a>Confirmación de los detalles de conexión del dispositivo

En la aplicación **Termite**, confirme los siguientes valores de punto de control para comprobar que el dispositivo se ha inicializado y está conectado a Azure IoT.

```output
DHCP In Progress...
IP address: 192.168.0.22
Mask: 255.255.255.0
Gateway: 192.168.0.1
DNS Server address: 75.75.75.75
SNTP Time Sync...
SNTP Time Sync successfully.
[INFO] Azure IoT Security Module has been enabled, status=0
Start Provisioning Client...
[INFO] IoTProvisioning client connect pending
Registered Device Successfully.
IoTHub Host Name: iotc-597ffb0b-3dbe-4784-ba3c-fdefd120a44a.azure-devices.net; Device ID: mydevice.
Connected to IoTHub.
Telemetry message send: {"temperature":22}.
Receive twin properties: {"desired":{"$version":1},"reported":{"maxTempSinceLastReboot":22,"$version":8}}
Failed to parse value
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
Telemetry message send: {"temperature":22}.
```

Mantenga abierto Termite para supervisar la salida del dispositivo en los pasos siguientes.

:::zone-end

## <a name="verify-the-device-status"></a>Comprobación del estado del dispositivo

Para ver el estado del dispositivo en el portal de IoT Central:

:::zone pivot="iot-toolset-cmake"
1. En el panel de la aplicación, seleccione **Dispositivos** en el menú de navegación lateral.
1. Confirme que **Estado del dispositivo** se ha actualizado a *Aprovisionado*.
1. Confirme que **Plantilla de dispositivo** se ha actualizado a *Guía de introducción*.

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/iot-central-device-view-status.png" alt-text="Captura de pantalla del estado del dispositivo en IoT Central":::
:::zone-end
:::zone pivot="iot-toolset-iar-ewarm, iot-toolset-mplab"
1. En el panel de la aplicación, seleccione **Dispositivos** en el menú de navegación lateral.
1. Confirme que **Estado del dispositivo** se ha actualizado a *Aprovisionado*.
1. Confirme que **Plantilla de dispositivo** se ha actualizado a *Termostato*.

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/iot-central-device-view-status-iar.png" alt-text="Captura de pantalla del estado del dispositivo en IoT Central":::
:::zone-end

## <a name="view-telemetry"></a>Ver datos de telemetría

Con IoT Central, puede ver el flujo de telemetría desde el dispositivo hasta la nube.

Para ver la telemetría en el portal de IoT Central:

1. En el panel de la aplicación, seleccione **Dispositivos** en el menú de navegación lateral.
1. Seleccione el dispositivo de la lista.
1. En la pestaña **Información general**, puede ver la telemetría a medida que el dispositivo envía mensajes a la nube.

    :::zone pivot="iot-toolset-cmake"
    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/iot-central-device-telemetry.png" alt-text="Captura de pantalla de la telemetría del dispositivo en IoT Central":::
    :::zone-end
    :::zone pivot="iot-toolset-iar-ewarm, iot-toolset-mplab"
    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/iot-central-device-telemetry-iar.png" alt-text="Captura de pantalla de la telemetría del dispositivo en IoT Central":::
    :::zone-end

    > [!NOTE]
    > También puede supervisar la telemetría desde el dispositivo mediante la aplicación Termite.

## <a name="call-a-direct-method-on-the-device"></a>Llamar a un método directo en un dispositivo

También puede usar IoT Central para llamar a un método directo que haya implementado en el dispositivo. Los métodos directos tienen un nombre y, opcionalmente, pueden tener una carga de JSON, una conexión configurable y un tiempo de espera del método. En esta sección, se llama a un método que le permite apagar o encender un LED.

Para llamar a un método en el portal de IoT Central:
:::zone pivot="iot-toolset-cmake"

1. Seleccione la pestaña **Comando** en la página del dispositivo.

1. En la lista desplegable **Estado**, seleccione **Verdadero** y, a continuación, **Ejecutar**.  La luz LED debe encenderse.

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/iot-central-invoke-method.png" alt-text="Captura de pantalla de la llamada a un método directo en un dispositivo en IoT Central":::

1. En la lista desplegable **Estado**, seleccione **Falso** y, luego, elija **Ejecutar**. La luz LED debe apagarse.
:::zone-end
:::zone pivot="iot-toolset-iar-ewarm, iot-toolset-mplab"

1. Seleccione la pestaña **Comando** en la página del dispositivo.

1. En el campo **Since** (Desde), use los selectores de fecha y hora para establecer una hora y, a continuación, seleccione **Run** (Ejecutar).

    :::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/iot-central-invoke-method-iar.png" alt-text="Captura de pantalla de la llamada a un método directo en un dispositivo en IoT Central":::

1. Puede ver la invocación del comando en Termite:

    ```output
    Receive method call: getMaxMinReport, with payload:"2021-10-14T17:45:00.000Z"
    ```

    > [!NOTE]
    > También puede ver la invocación del comando y la respuesta en la pestaña **Datos sin procesar** en la página del dispositivo en IoT Central.
:::zone-end

## <a name="view-device-information"></a>Ver la información del dispositivo

Puede ver la información del dispositivo en IoT Central.

En la página del dispositivo, seleccione la pestaña **Acerca de**.

:::zone pivot="iot-toolset-cmake"
:::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/iot-central-device-about.png" alt-text="Captura de pantalla de la información del dispositivo en IoT Central":::
:::zone-end
:::zone pivot="iot-toolset-iar-ewarm, iot-toolset-mplab"
:::image type="content" source="media/quickstart-devkit-microchip-atsame54-xpro/iot-central-device-about-iar.png" alt-text="Captura de pantalla de la información del dispositivo en IoT Central":::
:::zone-end

## <a name="troubleshoot-and-debug"></a>Solución de problemas y depuración

Si tiene problemas para compilar el código del dispositivo, guardar la imagen en la memoria flash del dispositivo o conectarse, consulte [Solución de problemas](troubleshoot-embedded-device-quickstarts.md).

:::zone pivot="iot-toolset-cmake"
Para depurar la aplicación, consulte [Depuración con Visual Studio Code](https://github.com/azure-rtos/getting-started/blob/master/docs/debugging.md).
:::zone-end
:::zone pivot="iot-toolset-iar-ewarm"
Para obtener ayuda para depurar la aplicación, consulte las opciones de **Help** (Ayuda) en **IAR EW for ARM**.  
:::zone-end
:::zone pivot="iot-toolset-mplab"
Para obtener ayuda para depurar la aplicación, consulte las opciones de **Help** (Ayuda) en el **IDE de MPLAB X**.  
:::zone-end

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita los recursos de Azure que se han creado en este inicio rápido, puede eliminarlos desde el portal de IoT Central.

Para quitar la aplicación de ejemplo entera de Azure IoT Central y todos sus dispositivos y recursos:

1. Seleccione **Administración**  > **Su aplicación**.
1. Seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este artículo de inicio rápido, ha compilado una imagen personalizada que contiene código de ejemplo de Azure RTOS y, después, ha guardado la imagen en el dispositivo Microchip E54. También ha usado el portal de IoT Central para crear recursos de Azure, conectar Microchip E54 de manera segura a Azure, ver la telemetría y enviar mensajes.

Como siguiente paso, explore los artículos a continuación para más información sobre el uso de los SDK de dispositivos IoT para conectar dispositivos a Azure IoT. 

> [!div class="nextstepaction"]
> [Conexión de un dispositivo simulado a IoT Central](quickstart-send-telemetry-central.md)
> [!div class="nextstepaction"]
> [Conexión de un dispositivo simulado a IoT Hub](quickstart-send-telemetry-iot-hub.md)

> [!IMPORTANT]
> Azure RTOS proporciona a los OEM componentes para proteger la comunicación y crear código y aislamiento de datos mediante los mecanismos de protección de hardware MCU/MPU subyacentes. Sin embargo, cada OEM es responsable en última instancia de garantizar que el dispositivo cumpla los requisitos de seguridad en constante evolución.
