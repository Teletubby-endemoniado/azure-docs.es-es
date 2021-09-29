---
author: dominicbetts
ms.author: dobett
ms.service: iot-develop
ms.topic: include
ms.date: 04/28/2021
ms.openlocfilehash: c4e0070a9647412a873953af59338b13f4c653fd
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128910034"
---
Si va a desarrollar para *servicios restringidos*, puede usar IoT Plug and Play con:

- El [SDK de Azure para las bibliotecas cliente de IoT de C insertadas](https://aka.ms/embeddedcsdk).
- [Azure RTOS](/azure/rtos/overview-rtos).

En este artículo se incluyen vínculos y recursos para estos escenarios restringidos.

## <a name="prerequisites"></a>Requisitos previos

En muchos de los ejemplos siguientes se requiere un dispositivo de hardware específico y los requisitos previos son diferentes en cada ejemplo. Para conocer de forma detallada los requisitos previos, la configuración y la compilación, siga el vínculo al ejemplo correspondiente.

## <a name="use-the-sdk-for-embedded-c"></a>Uso del SDK para C insertado

El SDK para C insertado ofrece una solución ligera para conectar dispositivos restringidos a servicios de Azure IoT, incluido el uso de las [convenciones de IoT Plug and Play](../articles/iot-develop/concepts-convention.md). Los siguientes vínculos incluyen ejemplos para dispositivos basados en MCU y también con fines educativos y de depuración.

### <a name="use-an-mcu-based-device"></a>Uso de un dispositivo basado en MCU

Para ver un tutorial completo donde se usa el SDK para C insertado, Device Provisioning Service e IoT Plug and Play en un MCU, consulte [Reasignación de la placa de desarrollo PIC-IoT Wx para conectarse a Azure mediante IoT Hub Device Provisioning Service](https://github.com/Azure-Samples/Microchip-PIC-IoT-Wx).

### <a name="introductory-samples"></a>Muestras de introducción

El repositorio del SDK para C insertado contiene [varias muestras](https://github.com/Azure/azure-sdk-for-c/tree/master/sdk/samples/iot#iot-hub-plug-and-play-sample) que indican cómo usar IoT Plug and Play:

> [!NOTE]
> Estas muestras se presentan en ejecución en Windows y Linux con fines educativos y de depuración. En un escenario de producción, las muestras solo están pensadas para dispositivos restringidos.

- [Ejemplo de Thermostat con el SDK para C insertado](https://github.com/Azure/azure-sdk-for-c/blob/main/sdk/samples/iot/paho_iot_pnp_sample.c)

- [Ejemplo de controlador de temperatura con el SDK para C insertado](https://github.com/Azure/azure-sdk-for-c/blob/main/sdk/samples/iot/paho_iot_pnp_component_sample.c)

## <a name="using-azure-rtos"></a>Uso de Azure RTOS

Azure RTOS incluye una capa ligera que agrega conectividad nativa a los servicios en la nube de Azure IoT. Esta capa proporciona un mecanismo sencillo para conectar dispositivos restringidos a Azure IoT y, al mismo tiempo, aprovechar las características avanzadas de Azure RTOS. Para obtener más información, consulte [Qué es Microsoft Azure RTOS](/azure/rtos/overview-rtos).

### <a name="toolchains"></a>Cadenas de herramientas

Los ejemplos de Azure RTOS se proporcionan con distintos tipos de combinaciones de IDE y cadena de herramientas, como:

- IAR: IDE de [Workbench insertado](https://www.iar.com/iar-embedded-workbench/) de IAR
- GCC/CMake: se basa en el sistema de compilación [CMake](https://cmake.org/) de código abierto y en la [cadena de herramientas insertada de Arm de GNU](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm).
- MCUExpresso: [IDE de MCUXpresso ](https://www.nxp.com/design/software/development-software/mcuxpresso-software-and-tools-/mcuxpresso-integrated-development-environment-ide:MCUXpresso-IDE) de NXP
- STM32Cube: [IDE de STM32Cube](https://www.st.com/en/development-tools/stm32cubeide.html) de STMicroelectronic
- MPLAB: [IDE de MPLAB X](https://www.microchip.com/mplab/mplab-x-ide) de Microchip

### <a name="samples"></a>Ejemplos

En la tabla siguiente se muestran ejemplos que ilustran cómo empezar a trabajar en distintos dispositivos con Azure RTOS e IoT Plug and Play:

Fabricante | Dispositivo | Ejemplos |
| --- | --- | --- |
| Microchip | [ATSAME54-XPRO](https://www.microchip.com/developmenttools/productdetails/atsame54-xpro) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/Microchip/ATSAME54-XPRO) • [IAR](https://aka.ms/azrtos-sample/e54-iar) • [MPLAB](https://aka.ms/azrtos-sample/e54-mplab)
| MXCHIP | [AZ3166](../articles/iot-develop/quickstart-devkit-mxchip-az3166.md) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/MXChip/AZ3166)
| NXP | [MIMXRT1060-EVK](https://www.nxp.com/design/development-boards/i-mx-evaluation-and-development-boards/mimxrt1060-evk-i-mx-rt1060-evaluation-kit:MIMXRT1060-EVK) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/NXP/MIMXRT1060-EVK) • [IAR](https://aka.ms/azrtos-sample/rt1060-iar) • [MCUXpresso](https://aka.ms/azrtos-sample/rt1060-mcuxpresso)
| STMicroelectronics | [32F746GDISCOVERY](https://www.st.com/en/evaluation-tools/32f746gdiscovery.html) | [IAR](https://aka.ms/azrtos-sample/f746g-iar) • [STM32Cube](https://aka.ms/azrtos-sample/f746g-cubeide)
| STMicroelectronics | [B-L475E-IOT01](https://www.st.com/en/evaluation-tools/b-l475e-iot01a.html) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/STMicroelectronics/STM32L4_L4%2B) • [IAR](https://aka.ms/azrtos-sample/l4s5-iar) • [STM32Cube](https://aka.ms/azrtos-sample/l4s5-cubeide)
| STMicroelectronics | [B-L4S5I-IOT01](https://www.st.com/en/evaluation-tools/b-l4s5i-iot01a.html) | [GCC/CMake](https://github.com/azure-rtos/getting-started/tree/master/STMicroelectronics/STM32L4_L4%2B) • [IAR](https://aka.ms/azrtos-sample/l4s5-iar) • [STM32Cube](https://aka.ms/azrtos-sample/l4s5-cubeide)
