---
title: Incorporación de un servidor habilitado para Azure Arc a Microsoft Sentinel
description: Aprenda a agregar servidores habilitados para Azure Arc a Microsoft Sentinel y a supervisar de forma proactiva su estado de seguridad.
ms.date: 07/16/2021
ms.topic: conceptual
ms.openlocfilehash: 9a7f7fb8edb6edf18d9f8d5fed0608520cb3b80f
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132306498"
---
# <a name="onboard-azure-arc-enabled-servers-to-microsoft-sentinel"></a>Incorporación de servidores habilitados para Azure Arc a Microsoft Sentinel

En este artículo encontrará información que le permitirá incorporar el servidor habilitado para Azure Arc a [Microsoft Sentinel](../../sentinel/overview.md) y comenzar a recopilar eventos relacionados con la seguridad. Microsoft Sentinel proporciona una única solución para la detección de alertas, la visibilidad de amenazas y la búsqueda proactiva y la respuesta contra amenazas en toda la empresa.

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que cumple los requisitos siguientes:

- Un [área de trabajo de Log Analytics.](../../azure-monitor/logs/data-platform-logs.md) Para obtener más información sobre las áreas de trabajo de Log Analytics, consulte [Diseño de su implementación de Azure Monitor Logs](../../azure-monitor/logs/design-logs-deployment.md).

- Microsoft Sentinel está [habilitado en la suscripción](../../sentinel/quickstart-onboard.md).

- La máquina o el servidor están conectados a servidores habilitados para Azure Arc.

## <a name="onboard-azure-arc-enabled-servers-to-microsoft-sentinel"></a>Incorporación de servidores habilitados para Azure Arc a Microsoft Sentinel

Microsoft Sentinel incluye una serie de conectores para las soluciones de Microsoft que están listos para usar y que proporcionan integración en tiempo real. Tanto en las máquinas físicas como en las virtuales, puede instalar el agente de Log Analytics que recopila los registros y los reenvía a Microsoft Sentinel. Los servidores habilitados para Azure Arc admiten la implementación del agente de Log Analytics mediante los métodos siguientes:

- Uso del marco de extensiones de VM.

    Esta característica de los servidores habilitados para Azure Arc le permite implementar la extensión de máquina virtual del agente de Log Analytics en un servidor de Windows o Linux que no sea de Azure. Las extensiones de máquina virtual se pueden administrar con los siguientes métodos en las máquinas híbridas o en servidores administrados por otros habilitados para Azure Arc:

    - [Azure Portal](manage-vm-extensions-portal.md)
    - La [CLI de Azure](manage-vm-extensions-cli.md)
    - [Azure PowerShell](manage-vm-extensions-powershell.md)
    - [Plantillas de Azure Resource Manager](manage-vm-extensions-template.md)

- Uso de Azure Policy.

    Con este enfoque, puede usar la directiva integrada [Implementación del agente de Log Analytics en máquinas de Azure Arc de Linux o de Windows](../../governance/policy/samples/built-in-policies.md#monitoring) de Azure Policy para auditar si el servidor habilitado para Azure Arc tiene instalado el agente de Log Analytics. Si el agente no está instalado, lo implementa automáticamente mediante una tarea de corrección. Como alternativa, si planea supervisar las máquinas con Azure Monitor para VM, puede usar en su lugar la iniciativa [Habilitar Azure Monitor para VM](../../governance/policy/samples/built-in-initiatives.md#monitoring) para instalar y configurar el agente de Log Analytics.

Se recomienda instalar el agente de Log Analytics para Windows o Linux con Azure Policy.

Una vez conectados los servidores habilitados para Arc, los datos comienzan a transmitirse a Microsoft Sentinel y podrá comenzar a trabajar con ellos. Puede ver los registros en los [libros integrados](../../sentinel/get-visibility.md) y comenzar a crear consultas en Log Analytics para [investigar los datos](../../sentinel/investigate-cases.md).

## <a name="next-steps"></a>Pasos siguientes

Empiece a [detectar amenazas con Microsoft Sentinel](../../sentinel/detect-threats-built-in.md).
