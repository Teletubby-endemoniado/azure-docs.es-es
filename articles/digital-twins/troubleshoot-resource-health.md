---
title: 'Solución de problemas: Resource Health'
titleSuffix: Azure Digital Twins
description: Aprenda a usar Azure Resource Health para comprobar el estado de la instancia de Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 10/7/2021
ms.topic: how-to
ms.service: digital-twins
ms.custom: contperf-fy22q1
ms.openlocfilehash: 9c816e613fe6f495de9abb57e2ac041960bffdbe
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2021
ms.locfileid: "130131455"
---
# <a name="troubleshooting-azure-digital-twins-resource-health"></a>Solución de problemas de Azure Digital Twins: Estado de los recursos

[Azure Service Health](../service-health/index.yml) es un conjunto de experiencias que pueden ayudarle a diagnosticar y obtener soporte técnico para los problemas de servicio que afectan a los recursos de Azure. Contiene información sobre **Resource Health**, **Service Health** y el **estado** e informes sobre la información del estado actual y anterior.

## <a name="use-azure-resource-health"></a>Uso de Azure Resource Health

[Azure Resource Health](../service-health/resource-health-overview.md) puede ayudarle a supervisar si la instancia de Azure Digital Twins está en funcionamiento. También permite saber si una interrupción regional afecta al estado de la instancia.

Para comprobar el estado de la instancia, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la instancia de Azure Digital Twins. Escriba el nombre en la barra de búsqueda para encontrarla. 

2. En el menú de la instancia, seleccione **Resource Health** en Support + troubleshooting (Soporte técnico y solución de problemas). Esto le llevará a la página para ver el historial de estado de los recursos. 

    :::image type="content" source="media/troubleshoot-resource-health/resource-health.png" alt-text="Captura de pantalla que muestra la página &quot;Resource Health&quot;. Hay una sección &quot;Historial de estado&quot; que muestra un informe diario de los últimos nueve días.":::

En la imagen anterior, esta instancia se muestra como **Disponible** y lo ha estado durante los últimos nueve días. Para obtener más información sobre el estado Disponible y los otros tipos de estado que pueden aparecer, consulte [Información general de Resource Health](../service-health/resource-health-overview.md).

También puede encontrar más información sobre las distintas comprobaciones que entran en el estado de los recursos para los diferentes tipos de recursos de Azure en [Tipos de recursos y comprobaciones de estado en Azure Resource Health](../service-health/resource-health-checks-resource-types.md).

## <a name="use-azure-service-health"></a>Uso de Azure Service Health

[Azure Service Health](../service-health/service-health-overview.md) puede ayudarle a comprobar el estado de todo el servicio de Azure Digital Twins en una región determinada y a tener en cuenta eventos como problemas de servicio continuos y próximos mantenimientos planeados.

Para comprobar el estado del servicio, inicie sesión en [Azure Portal](https://portal.azure.com) y vaya al servicio **Service Health**. Escriba "Service Health" en la barra de búsqueda para encontrarlo. 

A continuación, puede filtrar los problemas del servicio por suscripción, región y servicio.

Para obtener más información sobre el uso de Azure Service Health, consulte [Información general de Service Health](../service-health/service-health-overview.md).

## <a name="use-azure-status"></a>Uso del estado de Azure

La página [Estado de Azure](../service-health/azure-status-overview.md) proporciona una visión global del estado de los servicios y regiones de Azure. Aunque Azure Service Health y Azure Resource Health están personalizados para el recurso en cuestión, el estado de Azure tiene un ámbito mayor y puede ser útil para comprender incidentes de amplio impacto.

Para comprobar el estado de Azure, vaya a la página [Estado de Azure](https://status.azure.com/status/). La página muestra una tabla de servicios de Azure junto con indicadores de estado por región. Puede ver Azure Digital Twins si busca su entrada de tabla en la página.

Para obtener más información sobre el uso de la página de estado de Azure, consulte [Introducción al estado de Azure](../service-health/azure-status-overview.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte otras formas de supervisar la instancia de Azure Digital Twins en los siguientes artículos:
* [Solución de problemas: métricas](troubleshoot-metrics.md)
* [Solución de problemas: registros de diagnóstico](troubleshoot-diagnostics.md)
* [Solución de problemas: alertas](troubleshoot-alerts.md)
