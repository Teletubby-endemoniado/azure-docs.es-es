---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/17/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 5e73564010d4a78c14c87ed402d2aa2fc2501d84
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128909216"
---
|Nombre<br /><sub>(Azure Portal)</sub> |Descripción |Efectos |Versión<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Los servidores SQL Server con auditoría en el destino de la cuenta de almacenamiento se deben configurar con una retención de 90 días o superior.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F89099bee-89e0-4b26-a5f4-165451757743) |Con fines de investigación de incidentes, se recomienda establecer la retención de datos de auditoría de las instancias de SQL Server en el destino de la cuenta de almacenamiento en al menos 90 días. Confirme que cumple las reglas de retención necesarias para las regiones en las que trabaja. A veces, es necesario para cumplir con los estándares normativos. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditingRetentionDays_Audit.json) |
