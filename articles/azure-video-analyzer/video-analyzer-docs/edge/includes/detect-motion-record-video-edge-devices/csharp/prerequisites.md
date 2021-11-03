---
author: Juliako
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 04/07/2021
ms.author: juliako
ms.custom: ignite-fall-2021
ms.openlocfilehash: 7491b1db1124e905299a57d87ec12ca2e3d60271
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131030754"
---
* Una cuenta de Azure que incluya una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), en caso de que aún no lo haya hecho.

    [!INCLUDE [azure-subscription-permissions](../../common-includes/azure-subscription-permissions.md)]
* [Visual Studio Code](https://code.visualstudio.com/) con las siguientes extensiones:
    * [Herramientas de Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
    
    [!INCLUDE [install-docker-prompt](../../common-includes/install-docker-prompt.md)]   
    * [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)
* [SDK de .NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1).         

## <a name="set-up-azure-resources"></a>Configuración de los recursos de Azure

[![Implementación en Azure](https://aka.ms/deploytoazurebutton)](https://aka.ms/ava-click-to-deploy)  
[!INCLUDE [resources](../../../includes/common-includes/azure-resources.md)]

## <a name="overview"></a>Información general

> [!div class="mx-imgBorder"]
> :::image type="content" source="./../../../media/detect-motion-record-video-edge-devices/overview.png" alt-text="Publicación de eventos de inferencia asociados en el centro de IoT Edge":::

En el diagrama anterior se muestra cómo fluyen las señales en este inicio rápido. Un [módulo perimetral](https://github.com/Azure/video-analyzer/tree/main/edge-modules/sources/rtspsim-live555) simula una cámara IP que hospeda un servidor de protocolo RTSP. Un nodo de [origen RTSP](./../../../../pipeline.md#rtsp-source) extrae la fuente de vídeo de este servidor y envía fotogramas de vídeo al nodo del [procesador de detección del movimiento](./../../../../pipeline.md#motion-detection-processor). El origen RTSP envía los mismos fotogramas de vídeo a un nodo de [procesador de la puerta de señales](./../../../../pipeline.md#signal-gate-processor), que permanece cerrado hasta que un evento lo desencadena.

Cuando el procesador de detección del movimiento detecta movimiento en el vídeo, envía un evento al nodo de procesador de la puerta de señales y lo desencadena. La puerta se abre durante el tiempo configurado y envía fotogramas de vídeo al nodo del [receptor de archivos](./../../../../pipeline.md#file-sink). Este nodo del receptor registra el vídeo como un archivo MP4 en el sistema de archivos local del dispositivo perimetral. El archivo se guarda en la ubicación configurada.

En este inicio rápido realizará lo siguiente:

1. Creará e implementará la canalización.
1. Interpretará los resultados.
1. Limpieza de recursos.

## <a name="set-up-your-development-environment"></a>Configurado su entorno de desarrollo
[!INCLUDE [setup development environment](./../../../includes/set-up-dev-environment/csharp/csharp-set-up-dev-env.md)]
