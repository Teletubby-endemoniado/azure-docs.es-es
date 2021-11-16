---
title: Defender-IoT-micro-agent para Azure RTOS API
description: API de referencia para Defender-IoT-micro-agent para Azure RTOS.
ms.topic: reference
ms.date: 11/09/2021
ms.author: mlottner
ms.openlocfilehash: ad79ec853f3ee625c9b8c3c4bb4063aee87481f1
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132286647"
---
# <a name="defender-iot-micro-agent-for-azure-rtos-api-preview"></a>Defender-IoT-micro-agent para Azure RTOS API (versión preliminar)

Las API de Defender para IoT se rigen por la [licencia de API de Microsoft y Términos de uso](/legal/microsoft-apis/terms-of-use).

Esta API está diseñada para que se use solo con Defender-IoT-micro-agent para Azure RTOS API. Para ver otros recursos, consulte el [recurso de GitHub de Defender-IoT-micro-agent for Azure RTOS](https://github.com/azure-rtos/azure-iot-preview/releases).

## <a name="enable-defender-iot-micro-agent-for-azure-rtos"></a>Habilitación de Defender-IoT-micro-agent para Azure RTOS

**nx_azure_iot_security_module_enable**

### <a name="prototype"></a>Prototipo

```c
UINT nx_azure_iot_security_module_enable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Descripción

Esta rutina habilita el subsistema de Azure IoT Defender-IoT-micro-agent. Una máquina de estados interna administra la recopilación de eventos de seguridad y los envía a Azure IoT Hub. Solo se necesita una instancia de NX_AZURE_IOT_SECURITY_MODULE para administrar la recopilación de datos.

### <a name="parameters"></a>Parámetros

| Nombre | Descripción |
|---------|---------|
| nx_azure_iot_ptr  [in]    | Puntero a un objeto `NX_AZURE_IOT`.  |

### <a name="return-values"></a>Valores devueltos

|Valores devueltos  |Descripción |
|---------|---------|
|NX_AZURE_IOT_SUCCESS|   Módulo de seguridad de IoT de Azure habilitado correctamente.     |
|NX_AZURE_IOT_FAILURE   |  No se pudo habilitar el módulo de seguridad de IoT de Azure debido a un error interno.    |
|NX_AZURE_IOT_INVALID_PARAMETER   |  El módulo de seguridad requiere una instancia de #NX_AZURE_IOT válida.      |

### <a name="allowed-from"></a>Permitido desde

Subprocesos

## <a name="disable-azure-iot-defender-iot-micro-agent"></a>Deshabilitación de Azure IoT Defender-IoT-micro-agent

**nx_azure_iot_security_module_disable**

### <a name="prototype"></a>Prototipo

```c
UINT nx_azure_iot_security_module_disable(NX_AZURE_IOT *nx_azure_iot_ptr);
```

### <a name="description"></a>Descripción

Esta rutina deshabilita el subsistema de Azure IoT Defender-IoT-micro-agent.

### <a name="parameters"></a>Parámetros

| Nombre | Descripción |
|---------|---------|
| nx_azure_iot_ptr  [in]    | Puntero a `NX_AZURE_IOT`. Si es NULL, la instancia del singleton está deshabilitada. |

### <a name="return-values"></a>Valores devueltos

|Valores devueltos  |Descripción |
|---------|---------|
|NX_AZURE_IOT_SUCCESS     |   Correcto cuando el módulo de seguridad de IoT de Azure se ha deshabilitado correctamente.      |
|NX_AZURE_IOT_INVALID_PARAMETER   |  La instancia de IoT Hub de Azure es diferente de la instancia compuesta de singleton.       |
|NX_AZURE_IOT_FAILURE    |  No se pudo deshabilitar el módulo de seguridad de IoT de Azure debido a un error interno.       |

### <a name="allowed-from"></a>Permitido desde

Subprocesos

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo empezar a usar Azure RTOS Defender-IoT-micro-agent, consulte los siguientes artículos:

- Consulte la [información general](iot-security-azure-rtos.md) de Defender para IoT RTOS Defender-IoT-micro-agent.
