---
title: Solución de errores de incorporación de Azure Automanage habituales
description: Errores de incorporación de Automanage habituales y cómo solucionarlos
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: conceptual
ms.date: 01/14/2021
ms.openlocfilehash: 95d2f429c1ecb308d65aa01be0a6f3fa226b6168
ms.sourcegitcommit: 557ed4e74f0629b6d2a543e1228f65a3e01bf3ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2021
ms.locfileid: "129458176"
---
# <a name="troubleshoot-common-automanage-onboarding-errors"></a>Solución de errores de incorporación de Automanage habituales
Es posible que Automanage no pueda incorporar una máquina en el servicio. En este documento se explica cómo solucionar errores de implementación, se comparten algunos motivos comunes por los que se puede producir un error en las implementaciones y se describen los siguientes pasos posibles en la mitigación.

## <a name="troubleshooting-deployment-failures"></a>Solución de problemas con errores de implementación
La incorporación de una máquina a Automanage dará lugar a la creación de una implementación de Azure Resource Manager. Para más información, consulte la implementación para obtener más detalles sobre el motivo del error. Hay vínculos a las implementaciones en el control flotante de detalles del error, que se muestran a continuación.

:::image type="content" source="media\common-errors\failure-flyout.png" alt-text="Control flotante de detalles del error de Automanage.":::

### <a name="check-the-deployments-for-the-resource-group-containing-the-failed-machine"></a>Compruebe las implementaciones en el grupo de recursos que contiene la máquina con errores
El control flotante del error contendrá un vínculo a las implementaciones del grupo de recursos que contiene la máquina que generó el error durante la incorporación. El control flotante también contendrá un nombre de prefijo que se puede usar para filtrar las implementaciones. Al hacer clic en el vínculo de implementación, tendrá acceso a la hoja Implementaciones, donde puede filtrar las implementaciones para ver las implementaciones de Automanage en la máquina. Si va a realizar la implementación en varias regiones, asegúrese de hacer clic en la implementación en la región correcta.

### <a name="check-the-deployments-for-the-subscription-containing-the-failed-machine"></a>Compruebe las implementaciones en la suscripción que contiene la máquina con errores
Si no ve ningún error en la implementación del grupo de recursos, el siguiente paso sería examinar las implementaciones de la suscripción que contienen la máquina virtual en la que se produjo un error de incorporación. Haga clic en el vínculo **Deployments for subscription** (Implementaciones de suscripción) en el control flotante del error y filtre las implementaciones mediante el filtro **Automanage-DefaultResourceGroup**. Use el nombre del grupo de recursos de la hoja Error para filtrar las implementaciones. El nombre de la implementación tendrá como sufijo el nombre de una región. Si va a realizar la implementación en varias regiones, asegúrese de hacer clic en la implementación en la región correcta.

### <a name="check-deployments-in-a-subscription-linked-to-a-log-analytics-workspace"></a>Comprobación de las implementaciones en una suscripción vinculada a un área de trabajo de Log Analytics
Si no ve ninguna implementación con errores en el grupo de recursos o la suscripción que contienen la máquina con errores, y si esta máquina está conectada a un área de trabajo de Log Analytics en una suscripción diferente, vaya a la suscripción vinculada a su área de trabajo de Log Analytics y compruebe si hay implementaciones con errores.

## <a name="common-deployment-errors"></a>Errores de implementación frecuentes

Error |  Mitigación
:-----|:-------------|
Error de permisos insuficiente de la cuenta de Automanage | Este error puede ocurrir si ha cambiado recientemente una suscripción que contiene una nueva cuenta de Automanage a un nuevo inquilino. Los pasos para resolver este error se encuentran [aquí](./repair-automanage-account.md).
Región del área de trabajo que no cumple los requisitos de asignación de región | Automanage no pudo incorporar su máquina, pero el área de trabajo de Log Analytics a la que se vincula la máquina actualmente no se asigna a una región de Automation compatible. Asegúrese de que el área de trabajo de Log Analytics existente y la cuenta de Automation se encuentran en una [asignación de región compatible](../automation/how-to/region-mappings.md).
"Acceso denegado debido a la asignación de denegación con el nombre 'Asignación de denegación del sistema creada por la aplicación administrada'" | Se ha creado un objeto [denyAssignment](../role-based-access-control/deny-assignments.md) en el recurso, lo que ha impedido que Automanage accediera a él. Es posible que este objeto denyAssignment lo haya creado [Blueprint](../governance/blueprints/concepts/resource-locking.md) o una [aplicación administrada](../azure-resource-manager/managed-applications/overview.md).
"OS Information: Name='(null)', ver='(null)', agent status='Not Ready'" | Asegúrese de que está ejecutando una [versión mínima admitida del agente](/troubleshoot/azure/virtual-machines/support-extensions-agent-version), que el agente se está ejecutando ([Linux](/troubleshoot/azure/virtual-machines/linux-azure-guest-agent) y [Windows](/troubleshoot/azure/virtual-machines/windows-azure-guest-agent)) y que el agente está actualizado ([Linux](../virtual-machines/extensions/update-linux-agent.md) y [Windows](../virtual-machines/extensions/agent-windows.md)).
"No se puede determinar el sistema operativo para el nombre del sistema operativo de la máquina virtual:, ver. Compruebe que el agente de máquina virtual se está ejecutando y que el estado actual es Preparado". | Asegúrese de que está ejecutando una [versión mínima admitida del agente](/troubleshoot/azure/virtual-machines/support-extensions-agent-version), que el agente se está ejecutando ([Linux](/troubleshoot/azure/virtual-machines/linux-azure-guest-agent) y [Windows](/troubleshoot/azure/virtual-machines/windows-azure-guest-agent)) y que el agente está actualizado ([Linux](../virtual-machines/extensions/update-linux-agent.md) y [Windows](../virtual-machines/extensions/agent-windows.md)).
"La máquina virtual ha indicado un error al procesar la extensión 'IaaSAntimalware'" | Asegúrese de que no tiene otra oferta de antimalware o antivirus instalada en la máquina virtual. Si se produce un error, póngase en contacto con el soporte técnico.
Área de trabajo de ASC: Automanage no admite actualmente el servicio Log Analytics en este valor de _location_. | Compruebe que la máquina virtual se encuentra en una [región admitida](./automanage-virtual-machines.md#supported-regions).
Se produjo un error en la implementación de la plantilla debido a la infracción de la directiva. Consulte los detalles para obtener más información. | Una directiva impide que Automanage incorpore la máquina virtual. Compruebe las directivas que se aplican a la suscripción o al grupo de recursos que contiene a la máquina virtual que quiere incorporar a Automanage.
"Error en la asignación; no hay información adicional disponible" | Abra un caso con el soporte técnico de Microsoft Azure.

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre Azure Automanage](./automanage-virtual-machines.md)

> [!div class="nextstepaction"]
> [Habilitar Automanage para máquinas en Azure Portal](quick-create-virtual-machines-portal.md)