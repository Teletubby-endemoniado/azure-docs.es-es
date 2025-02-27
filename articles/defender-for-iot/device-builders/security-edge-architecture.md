---
title: azureiotsecurity de Defender para IoT para IoT Edge
description: Conozca la arquitectura y las capacidades de azureiotsecurity de Microsoft Defender para IoT para IoT Edge.
ms.topic: conceptual
ms.date: 11/09/2021
ms.openlocfilehash: 55681d731193e0f73a9fb21b415b03d4411ee6fe
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132318966"
---
# <a name="microsoft-defender-for-iot-edge-azureiotsecurity"></a>azureiotsecurity de Microsoft Defender para IoT Edge

[Azure IoT Edge](../../iot-edge/index.yml) proporciona capacidades de enorme eficacia para administrar y procesar flujos de trabajo empresariales en el perímetro. El papel esencial que IoT Edge desempeña en entornos de IoT lo hace especialmente atrayente para atacantes malintencionados.

azureiotsecurity de Defender para IoT proporciona una solución de seguridad completa para dispositivos IoT Edge. El módulo de Defender para IoT recopila, agrega y analiza los datos de seguridad sin procesar del sistema operativo y del sistema de contenedores para generar alertas y recomendaciones de seguridad que requieren acción.

Al igual que los agentes de seguridad de Defender para IoT de los dispositivos IoT, el módulo de Defender para IoT Edge se puede personalizar profundamente a través de su módulo gemelo. Vea [Configurar agentes de seguridad](how-to-agent-configuration.md) para obtener más información.

azureiotsecurity de Defender para IoT para IoT Edge ofrece las siguientes características:

- Recopila eventos de seguridad sin procesar del sistema operativo subyacente (Linux) y de los sistemas de contenedor de IoT Edge.

  Consulte [Configuración del agente de Defender para IoT](how-to-agent-configuration.md) para obtener más información sobre los recopiladores de datos de seguridad disponibles.

- Análisis de manifiestos de implementación de IoT Edge.

- Agrega eventos de seguridad sin procesar a los mensajes enviados a través de [IoT Edge Hub](../../iot-edge/iot-edge-runtime.md#iot-edge-hub).

- Quita la configuración usando el módulo gemelo azureiotsecurity.

  Consulte [Configuración de un agente de Defender para IoT](how-to-agent-configuration.md) para obtener más información.

azureiotsecurity de Defender para IoT para IoT Edge se ejecuta en un modo con privilegios en IoT Edge. El modo con privilegios es necesario para permitir que el módulo supervise el sistema operativo y otros módulos de IoT Edge.

## <a name="module-supported-platforms"></a>Plataformas admitidas por el módulo

azureiotsecurity de Defender para IoT para IoT Edge solo está disponible actualmente en Linux.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha conocido la arquitectura y las funcionalidades de azureiotsecurity de Defender para IoT para IoT Edge.

Para seguir obteniendo información sobre la implementación de Defender para IoT, lea los siguientes artículos:

- Implementación de [azureiotsecurity para IoT Edge](how-to-deploy-edge.md)
- Aprenda a [configurar el microagente de Defender para IoT](how-to-agent-configuration.md).
- Obtenga información sobre cómo [habilitar el servicio Defender para IoT en IoT Hub](quickstart-onboard-iot-hub.md)
- Obtenga más información sobre el servicio en las [preguntas más frecuentes de Defender para IoT](resources-agent-frequently-asked-questions.md)
