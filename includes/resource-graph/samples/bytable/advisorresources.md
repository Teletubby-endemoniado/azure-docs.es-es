---
author: DCtheGeek
ms.service: resource-graph
ms.topic: include
ms.date: 09/03/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: c7bf4e3fb3f5584a72435ed7ef23f76afdebde6e
ms.sourcegitcommit: f2d0e1e91a6c345858d3c21b387b15e3b1fa8b4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2021
ms.locfileid: "123536032"
---
### <a name="get-cost-savings-summary-from-azure-advisor"></a>Obtención de un resumen de ahorro de costos de Azure Advisor

Esta consulta resume el ahorro de costos de cada recomendación de [Azure Advisor](../../../../articles/advisor/advisor-overview.md).

```kusto
AdvisorResources
| where type == 'microsoft.advisor/recommendations'
| where properties.category == 'Cost'
| extend
    resources = tostring(properties.resourceMetadata.resourceId),
    savings = todouble(properties.extendedProperties.savingsAmount),
    solution = tostring(properties.shortDescription.solution),
    currency = tostring(properties.extendedProperties.savingsCurrency)
| summarize
    dcount(resources),
    bin(sum(savings), 0.01)
    by solution, currency
| project solution, dcount_resources, sum_savings, currency
| order by sum_savings desc
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interactive
az graph query -q "AdvisorResources | where type == 'microsoft.advisor/recommendations' | where properties.category == 'Cost' | extend resources = tostring(properties.resourceMetadata.resourceId), savings = todouble(properties.extendedProperties.savingsAmount), solution = tostring(properties.shortDescription.solution), currency = tostring(properties.extendedProperties.savingsCurrency) | summarize dcount(resources), bin(sum(savings), 0.01) by solution, currency | project solution, dcount_resources, sum_savings, currency | order by sum_savings desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "AdvisorResources | where type == 'microsoft.advisor/recommendations' | where properties.category == 'Cost' | extend resources = tostring(properties.resourceMetadata.resourceId), savings = todouble(properties.extendedProperties.savingsAmount), solution = tostring(properties.shortDescription.solution), currency = tostring(properties.extendedProperties.savingsCurrency) | summarize dcount(resources), bin(sum(savings), 0.01) by solution, currency | project solution, dcount_resources, sum_savings, currency | order by sum_savings desc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Pruebe a ejecutar esta consulta en el Explorador de Azure Resource Graph:

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AdvisorResources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.advisor%2frecommendations%27%0a%7c%20where%20properties.category%20%3d%3d%20%27Cost%27%0a%7c%20extend%0a%09resources%20%3d%20tostring(properties.resourceMetadata.resourceId)%2c%0a%09savings%20%3d%20todouble(properties.extendedProperties.savingsAmount)%2c%0a%09solution%20%3d%20tostring(properties.shortDescription.solution)%2c%0a%09currency%20%3d%20tostring(properties.extendedProperties.savingsCurrency)%0a%7c%20summarize%0a%09dcount(resources)%2c%0a%09bin(sum(savings)%2c%200.01)%0a%09by%20solution%2c%20currency%0a%7c%20project%20solution%2c%20dcount_resources%2c%20sum_savings%2c%20currency%0a%7c%20order%20by%20sum_savings%20desc" target="_blank">portal.azure.com</a>
- Portal de Azure Government: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AdvisorResources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.advisor%2frecommendations%27%0a%7c%20where%20properties.category%20%3d%3d%20%27Cost%27%0a%7c%20extend%0a%09resources%20%3d%20tostring(properties.resourceMetadata.resourceId)%2c%0a%09savings%20%3d%20todouble(properties.extendedProperties.savingsAmount)%2c%0a%09solution%20%3d%20tostring(properties.shortDescription.solution)%2c%0a%09currency%20%3d%20tostring(properties.extendedProperties.savingsCurrency)%0a%7c%20summarize%0a%09dcount(resources)%2c%0a%09bin(sum(savings)%2c%200.01)%0a%09by%20solution%2c%20currency%0a%7c%20project%20solution%2c%20dcount_resources%2c%20sum_savings%2c%20currency%0a%7c%20order%20by%20sum_savings%20desc" target="_blank">portal.azure.us</a>
- Portal de Azure China 21Vianet: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AdvisorResources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.advisor%2frecommendations%27%0a%7c%20where%20properties.category%20%3d%3d%20%27Cost%27%0a%7c%20extend%0a%09resources%20%3d%20tostring(properties.resourceMetadata.resourceId)%2c%0a%09savings%20%3d%20todouble(properties.extendedProperties.savingsAmount)%2c%0a%09solution%20%3d%20tostring(properties.shortDescription.solution)%2c%0a%09currency%20%3d%20tostring(properties.extendedProperties.savingsCurrency)%0a%7c%20summarize%0a%09dcount(resources)%2c%0a%09bin(sum(savings)%2c%200.01)%0a%09by%20solution%2c%20currency%0a%7c%20project%20solution%2c%20dcount_resources%2c%20sum_savings%2c%20currency%0a%7c%20order%20by%20sum_savings%20desc" target="_blank">portal.azure.cn</a>

---

### <a name="list-arc-enabled-servers-not-running-latest-released-agent-version"></a>Enumeración de los servidores habilitados para Arc que no ejecutan la versión del agente publicada más recientemente

Esta consulta devuelve todos los servidores habilitados para Arc que ejecutan una versión obsoleta del agente de Connected Machine. Los agentes que tengan el estado **Expirado** se excluirán de los resultados. La consulta usa _leftouter_ `join` para agrupar las recomendaciones de Advisor sobre los agentes de Connected Machine identificados como obsoletos, y las máquinas de equipo híbrido para filtrar cualquier agente que no se haya comunicado con Azure durante un periodo de tiempo.

```kusto
AdvisorResources
| where type == 'microsoft.advisor/recommendations'
| where properties.category == 'HighAvailability'
| where properties.shortDescription.solution == 'Upgrade to the latest version of the Azure Connected Machine agent'
| project
        id,
        JoinId = toupper(properties.resourceMetadata.resourceId),
        machineName = tostring(properties.impactedValue),
        agentVersion = tostring(properties.extendedProperties.installedVersion),
        expectedVersion = tostring(properties.extendedProperties.latestVersion)
| join kind=leftouter(
    Resources
    | where type == 'microsoft.hybridcompute/machines'
    | project
        machineId = toupper(id),
        status = tostring (properties.status)
    ) on $left.JoinId == $right.machineId
| where status != 'Expired'
| summarize by id, machineName, agentVersion, expectedVersion
| order by tolower(machineName) asc
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interactive
az graph query -q "AdvisorResources | where type == 'microsoft.advisor/recommendations' | where properties.category == 'HighAvailability' | where properties.shortDescription.solution == 'Upgrade to the latest version of the Azure Connected Machine agent' | project  id,  JoinId = toupper(properties.resourceMetadata.resourceId),  machineName = tostring(properties.impactedValue),  agentVersion = tostring(properties.extendedProperties.installedVersion),  expectedVersion = tostring(properties.extendedProperties.latestVersion) | join kind=leftouter( Resources | where type == 'microsoft.hybridcompute/machines' | project  machineId = toupper(id),  status = tostring (properties.status) ) on $left.JoinId == $right.machineId | where status != 'Expired' | summarize by id, machineName, agentVersion, expectedVersion | order by tolower(machineName) asc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "AdvisorResources | where type == 'microsoft.advisor/recommendations' | where properties.category == 'HighAvailability' | where properties.shortDescription.solution == 'Upgrade to the latest version of the Azure Connected Machine agent' | project  id,  JoinId = toupper(properties.resourceMetadata.resourceId),  machineName = tostring(properties.impactedValue),  agentVersion = tostring(properties.extendedProperties.installedVersion),  expectedVersion = tostring(properties.extendedProperties.latestVersion) | join kind=leftouter( Resources | where type == 'microsoft.hybridcompute/machines' | project  machineId = toupper(id),  status = tostring (properties.status) ) on $left.JoinId == $right.machineId | where status != 'Expired' | summarize by id, machineName, agentVersion, expectedVersion | order by tolower(machineName) asc"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png":::Pruebe a ejecutar esta consulta en el Explorador de Azure Resource Graph:

- Azure Portal: <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AdvisorResources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.advisor%2frecommendations%27%0a%7c%20where%20properties.category%20%3d%3d%20%27HighAvailability%27%0a%7c%20where%20properties.shortDescription.solution%20%3d%3d%20%27Upgrade%20to%20the%20latest%20version%20of%20the%20Azure%20Connected%20Machine%20agent%27%0a%7c%20project%0a%09%09id%2c%0a%09%09JoinId%20%3d%20toupper(properties.resourceMetadata.resourceId)%2c%0a%09%09machineName%20%3d%20tostring(properties.impactedValue)%2c%0a%09%09agentVersion%20%3d%20tostring(properties.extendedProperties.installedVersion)%2c%0a%09%09expectedVersion%20%3d%20tostring(properties.extendedProperties.latestVersion)%0a%7c%20join%20kind%3dleftouter(%0a%09Resources%0a%09%7c%20where%20type%20%3d%3d%20%27microsoft.hybridcompute%2fmachines%27%0a%09%7c%20project%0a%09%09machineId%20%3d%20toupper(id)%2c%0a%09%09status%20%3d%20tostring%20(properties.status)%0a%09)%20on%20%24left.JoinId%20%3d%3d%20%24right.machineId%0a%7c%20where%20status%20!%3d%20%27Expired%27%0a%7c%20summarize%20by%20id%2c%20machineName%2c%20agentVersion%2c%20expectedVersion%0a%7c%20order%20by%20tolower(machineName)%20asc" target="_blank">portal.azure.com</a>
- Portal de Azure Government: <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AdvisorResources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.advisor%2frecommendations%27%0a%7c%20where%20properties.category%20%3d%3d%20%27HighAvailability%27%0a%7c%20where%20properties.shortDescription.solution%20%3d%3d%20%27Upgrade%20to%20the%20latest%20version%20of%20the%20Azure%20Connected%20Machine%20agent%27%0a%7c%20project%0a%09%09id%2c%0a%09%09JoinId%20%3d%20toupper(properties.resourceMetadata.resourceId)%2c%0a%09%09machineName%20%3d%20tostring(properties.impactedValue)%2c%0a%09%09agentVersion%20%3d%20tostring(properties.extendedProperties.installedVersion)%2c%0a%09%09expectedVersion%20%3d%20tostring(properties.extendedProperties.latestVersion)%0a%7c%20join%20kind%3dleftouter(%0a%09Resources%0a%09%7c%20where%20type%20%3d%3d%20%27microsoft.hybridcompute%2fmachines%27%0a%09%7c%20project%0a%09%09machineId%20%3d%20toupper(id)%2c%0a%09%09status%20%3d%20tostring%20(properties.status)%0a%09)%20on%20%24left.JoinId%20%3d%3d%20%24right.machineId%0a%7c%20where%20status%20!%3d%20%27Expired%27%0a%7c%20summarize%20by%20id%2c%20machineName%2c%20agentVersion%2c%20expectedVersion%0a%7c%20order%20by%20tolower(machineName)%20asc" target="_blank">portal.azure.us</a>
- Portal de Azure China 21Vianet: <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AdvisorResources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.advisor%2frecommendations%27%0a%7c%20where%20properties.category%20%3d%3d%20%27HighAvailability%27%0a%7c%20where%20properties.shortDescription.solution%20%3d%3d%20%27Upgrade%20to%20the%20latest%20version%20of%20the%20Azure%20Connected%20Machine%20agent%27%0a%7c%20project%0a%09%09id%2c%0a%09%09JoinId%20%3d%20toupper(properties.resourceMetadata.resourceId)%2c%0a%09%09machineName%20%3d%20tostring(properties.impactedValue)%2c%0a%09%09agentVersion%20%3d%20tostring(properties.extendedProperties.installedVersion)%2c%0a%09%09expectedVersion%20%3d%20tostring(properties.extendedProperties.latestVersion)%0a%7c%20join%20kind%3dleftouter(%0a%09Resources%0a%09%7c%20where%20type%20%3d%3d%20%27microsoft.hybridcompute%2fmachines%27%0a%09%7c%20project%0a%09%09machineId%20%3d%20toupper(id)%2c%0a%09%09status%20%3d%20tostring%20(properties.status)%0a%09)%20on%20%24left.JoinId%20%3d%3d%20%24right.machineId%0a%7c%20where%20status%20!%3d%20%27Expired%27%0a%7c%20summarize%20by%20id%2c%20machineName%2c%20agentVersion%2c%20expectedVersion%0a%7c%20order%20by%20tolower(machineName)%20asc" target="_blank">portal.azure.cn</a>

---

