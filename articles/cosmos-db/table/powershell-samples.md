---
title: Ejemplos de Azure PowerShell para Table API de Azure Cosmos DB
description: Obtenga ejemplos de Azure PowerShell para realizar tareas comunes en Table API de Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
ms.date: 01/20/2021
ms.author: mjbrown
ms.openlocfilehash: 04567339ed39d53b925084098ffde42a84efb6ee
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121786456"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-table-api"></a>Ejemplos de Azure PowerShell para Table API de Azure Cosmos DB
[!INCLUDE[appliesto-table-api](../includes/appliesto-table-api.md)]

En la tabla siguiente se incluyen vínculos a scripts de Azure PowerShell usados habitualmente para Azure Cosmos DB. Use los vínculos de la derecha para ir a ejemplos específicos de la API. Los ejemplos comunes son los mismos en todas las API. En la [referencia de Azure PowerShell](/powershell/module/az.cosmosdb) hay páginas de referencia para todos los cmdlets de PowerShell de Azure Cosmos DB disponibles. El módulo `Az.CosmosDB` ahora es parte del módulo `Az`. [Descargue e instale](/powershell/azure/install-az-ps) la versión más reciente del módulo Az Module para obtener los cmdlets de Azure Cosmos DB. También puede obtener la versión más reciente de la [Galería de PowerShell](https://www.powershellgallery.com/packages/Az/5.4.0). También puede bifurcar estos ejemplos de PowerShell para Cosmos DB desde nuestro repositorio de GitHub, [Ejemplos de PowerShell para Cosmos DB en GitHub](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb).

## <a name="common-samples"></a>Ejemplos comunes

|Tarea | Descripción |
|---|---|
|[Actualización de una cuenta](../scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Actualice el nivel de coherencia predeterminado de una cuenta de Cosmos DB. |
|[Actualización de las regiones de una cuenta](../scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Actualice las regiones de una cuenta de Cosmos DB. |
|[Cambio de la prioridad de la conmutación por error o desencadenamiento de la conmutación por error](../scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cambie la prioridad de la conmutación por error regional de una cuenta de Azure Cosmos o desencadene una conmutación por error manual. |
|[Claves de cuenta o cadenas de conexión](../scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenga las claves principales y secundarias, las cadenas de conexión o vuelva a generar una clave de cuenta de una cuenta de Azure Cosmos DB. |
|[Creación de una cuenta de Cosmos con firewall de IP](../scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta de Azure Cosmos DB con firewall de IP habilitado. |
|||

## <a name="table-api-samples"></a>Ejemplos de Table API

|Tarea | Descripción |
|---|---|
|[Crear una cuenta y tabla](../scripts/powershell/table/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Cree una cuenta y una tabla de Azure Cosmos. |
|[Creación de una cuenta y una tabla con escalabilidad automática](../scripts/powershell/table/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crea una cuenta y una tabla con escalabilidad automática de Azure Cosmos. |
|[Enumerar u obtener tablas](../scripts/powershell/table/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Enumere u obtenga las tablas. |
|[Operaciones de capacidad de proceso](../scripts/powershell/table/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Operaciones relacionadas con la capacidad de proceso de una tabla, como obtener, actualizar y migrar entre la escalabilidad automática y la capacidad de proceso estándar. |
|[Bloquear recursos contra la eliminación](../scripts/powershell/table/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Evite la eliminación de recursos con bloqueos de recursos. |
|||
