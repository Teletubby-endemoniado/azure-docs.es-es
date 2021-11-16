---
title: Compatibilidad del sistema operativo e información general de la cartera de agentes (versión preliminar)
description: Microsoft Defender para IoT proporciona una amplia cartera de agentes en función del tipo de dispositivo.
ms.date: 11/09/2021
ms.topic: conceptual
ms.openlocfilehash: fbad1c486a9bf553b7f7effffe8dcf5e12b5d03a
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132319023"
---
# <a name="agent-portfolio-overview-and-os-support-preview"></a>Compatibilidad del sistema operativo e información general de la cartera de agentes (versión preliminar)

Microsoft Defender para IoT proporciona una amplia cartera de agentes en función del tipo de dispositivo.

## <a name="standalone-agent"></a>Agente independiente

El agente independiente cubre la mayoría de los sistemas operativos Linux; se puede implementar como paquete binario o como código fuente que se puede incorporar como parte del firmware y permitir la modificación y personalización en función de las necesidades del cliente. Estos son algunos ejemplos de sistemas operativos admitidos:

| Sistema operativo | AMD64 | ARM32v7 | ARM64 |
|--|--|--|--|
| Debian 9 | ✓ | ✓ | |
| Ubuntu 18.04 | ✓ |  | ✓ |
| Ubuntu 20.04 | ✓ |  | |

Para obtener una vista más detallada de las dependencias del sistema operativo del microagente, consulte [Dependencias de Linux](concept-micro-agent-linux-dependencies.md#linux-dependencies).

Para obtener más información, sistemas operativos admitidos o solicitar acceso al código fuente para poder incorporarlo como parte del firmware del dispositivo, póngase en contacto con el administrador de cuentas o envíe un correo electrónico a <defender_micro_agent@microsoft.com>.

## <a name="azure-rtos-micro-agent"></a>Microagente de Azure RTOS

El microagente de Microsoft Defender para IoT proporciona una solución de seguridad completa y ligera para los dispositivos que usan Azure RTOS. Proporciona cobertura para amenazas comunes y posibles actividades malintencionadas en dispositivos del sistema operativo en tiempo real (RTOS). El microagente viene integrado como parte del componente de Azure RTOS NetX Duo y supervisa la actividad de red del dispositivo.

El microagente de Microsoft Defender para IoT viene integrado como parte del componente de Azure RTOS NetX Duo y supervisa la actividad de red del dispositivo. El microagente se compone de una solución de seguridad completa y ligera que proporciona cobertura para las amenazas comunes y las posibles actividades malintencionadas en los dispositivos del sistema operativo en tiempo real (RTOS).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las [dependencias de Linux de microagente (versión preliminar)](concept-micro-agent-linux-dependencies.md).

Más información en [Introducción al microagente independiente (versión preliminar)](concept-standalone-micro-agent-overview.md).
