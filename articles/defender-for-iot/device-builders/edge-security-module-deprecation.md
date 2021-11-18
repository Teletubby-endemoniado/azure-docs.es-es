---
title: Compatibilidad y retirada de características
description: Defender para IoT seguirá dando soporte a C, C# y Edge hasta el 1 de marzo de 2022.
ms.date: 11/09/2021
ms.topic: how-to
ms.openlocfilehash: 441f01261ff3f22a39ac6754221fb671a362c387
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132281880"
---
# <a name="feature-support-and-retirement"></a>Compatibilidad y retirada de características

En este artículo se describen las características de Microsoft Defender para IoT y la compatibilidad con diferentes funcionalidades dentro de Defender para IoT.

## <a name="defender-for-iot-c-c-and-edge-defender-iot-micro-agent-deprecation"></a>Retirada de Defender-IoT-micro-agent de Defender para IoT de C, C# y Edge

El nuevo microagente reemplazará al agente Defender-IoT-micro-agent de C, C# y Edge.  

El desarrollo del microagente nuevo se basa en el conocimiento y la experiencia obtenidos no solo durante el desarrollo del módulo de seguridad clásico, sino también de los clientes y los comentarios de asociados, y tiene cuatro mejoras importantes:

- **Valor de seguridad de profundidad**: el nuevo agente se ejecutará en el nivel de host, lo que proporcionará más visibilidad a las operaciones subyacentes del dispositivo y mayor cobertura de seguridad.

- **Mayor rendimiento de los dispositivos y menor superficie**: se consigue gracias a una RAM y a una superficie de memoria ROM pequeñas, así como un bajo consumo de la CPU.  

- **Plug and play**: el nuevo microagente ya no tiene dependencias en el nivel de kernel y todas sus dependencias de software se proporcionan como parte de su paquete. El microagente admite una arquitectura de CPU común.

- **Fácil de implementar**: El microagente admite diferentes modelos de distribución, a través de código fuente y en forma de paquete binario. 

### <a name="timeline"></a>Escala de tiempo 

Defender para IoT seguirá dando soporte a C, C# y Edge hasta el 1 de marzo de 2022. 

## <a name="micro-agent-preview-support"></a>Compatibilidad con la versión preliminar del microagente

Durante la versión preliminar, el microagente puede experimentar cambios importantes sin previo aviso.

## <a name="next-steps"></a>Pasos siguientes

Consulte las [Preguntas más frecuentes sobre el agente de Microsoft Defender para IoT](resources-agent-frequently-asked-questions.md).
