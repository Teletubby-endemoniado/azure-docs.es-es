---
author: Juliako
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 11/04/2021
ms.author: juliako
ms.custom: ignite-fall-2021
ms.openlocfilehash: a7d745c4e2dfd364e9cb0d62225abf74bd1741a5
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131860480"
---
* Una cuenta de Azure que incluya una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), en caso de que aún no lo haya hecho.

    > [!NOTE]
    > Necesitará una suscripción a Azure con al menos un rol de colaborador. Si no tiene los permisos adecuados, póngase en contacto con el administrador de la cuenta para que se los conceda.
    * [Visual Studio Code](https://code.visualstudio.com/) con las siguientes extensiones:
        * [Herramientas de Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

        [!INCLUDE [install-docker-prompt](../../common-includes/install-docker-prompt.md)]
        * [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
    * [Python 3](https://www.python.org/downloads/) (3.6.9 o posterior), [Pip 3](https://pip.pypa.io/en/stable/installing/) y, opcionalmente, [venv](https://docs.python.org/3/library/venv.html).
* Lea el inicio rápido [Detección de movimiento y emisión de eventos](../../../detect-motion-emit-events-quickstart.md)
## <a name="set-up-azure-resources"></a>Configuración de los recursos de Azure

[![Implementación en Azure](https://aka.ms/deploytoazurebutton)](https://aka.ms/ava-click-to-deploy)  
[!INCLUDE [resources](../../../includes/common-includes/azure-resources.md)]

## <a name="overview"></a>Información general
En este inicio rápido, usará Live Video Analyzer para detectar objetos, como vehículos y personas. Publicará eventos de inferencia asociados en IoT Edge Hub.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./../../../media/analyze-live-video-use-your-model-http/overview.png" alt-text="Publicación de eventos de inferencia asociados en el centro de IoT Edge":::

En este diagrama se muestra cómo fluyen las señales en este inicio rápido. Un [módulo perimetral](https://github.com/Azure/video-analyzer/tree/main/edge-modules/sources/rtspsim-live555) simula una cámara IP que hospeda un servidor con el Protocolo de streaming en tiempo real (RTSP). Un [nodo de origen RTSP](./../../../../pipeline.md#rtsp-source) extrae la fuente de vídeo de este servidor y envía los fotogramas de vídeo al [nodo procesador de extensiones HTTP](./../../../../pipeline.md#http-extension-processor).

El nodo de extensión HTTP desempeña el rol de un proxy. Se muestrean los fotogramas de vídeo entrantes que configuró mediante el campo samplingOptions y se convierten en el tipo de imagen especificado. Luego, se retransmiten las imágenes a través de REST a otro módulo perimetral que ejecuta un modelo de IA detrás de un punto de conexión HTTP. En este ejemplo, el módulo perimetral se crea mediante el modelo YOLOv3, que puede detectar muchos tipos de objetos. El nodo procesador de extensiones HTTP recopila los resultados de la detección y publica los eventos en el [nodo receptor de mensajes de IoT Hub](./../../../../pipeline.md#iot-hub-message-sink). que posteriormente los envía a [IoT Edge Hub](../../../../../../iot-fundamentals/iot-glossary.md?view=iotedge-2018-06&preserve-view=true#iot-edge-hub).

En este inicio rápido realizará lo siguiente:

* Creará e implementará la canalización en directo.
* Interpretará los resultados.
* Limpieza de recursos.
## <a name="set-up-your-development-environment"></a>Configurado su entorno de desarrollo
[!INCLUDE [setup development environment](./../../../includes/set-up-dev-environment/python/python-set-up-dev-env.md)]
