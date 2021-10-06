---
title: Ejemplos de CLI de Azure para Azure Cosmos DB | Microsoft Docs
description: En este artículo se enumeran varios ejemplos de código de la CLI de Azure disponibles para interactuar con Azure Cosmos DB. Vea ejemplos de la CLI específicos de la API.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 09/17/2021
ms.author: mjbrown
ms.custom: devx-track-azurecli, seo-azure-cli
keywords: cosmos db, muestras de la cli de Azure, muestras de código de la cli de Azure, muestras de script de la cli de Azure
ms.openlocfilehash: 4529b51ff5109bfa6b8814b23e7e82844e7fb95c
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128567725"
---
# <a name="azure-cli-samples-for-azure-cosmos-db-core-sql-api"></a>Ejemplos de la CLI de Azure para la API de Azure Cosmos DB Core (SQL)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

En la tabla siguiente se incluyen vínculos a scripts de ejemplo de la CLI de Azure para Azure Cosmos DB. Use los vínculos de la derecha para ir a ejemplos específicos de la API. Los ejemplos comunes son los mismos en todas las API. Hay páginas de referencia para todos los comandos de CLI de Azure Cosmos DB disponibles en la [referencia de la CLI de Azure](/cli/azure/cosmosdb). Encontrará más ejemplos de scripts de la CLI de Azure Cosmos DB en el [repositorio de la CLI de Azure Cosmos DB en GitHub](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).

Estos ejemplos requieren la versión 2.12.1 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli)

Para obtener ejemplos de la CLI de Azure para otras API, consulte [Ejemplos de la CLI para Cassandra](../cassandra/cli-samples.md), [Ejemplos de la CLI para MongoDB API](../mongodb/cli-samples.md), [Ejemplos de la CLI para Gremlin](../graph/cli-samples.md), [Ejemplos de la CLI para Table](../table/cli-samples.md).

## <a name="common-samples"></a>Ejemplos comunes

Estos ejemplos se aplican a todas las API de Azure Cosmos DB

|Tarea | Descripción |
|---|---|
| [Agregar o regiones de conmutación por error](../scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | Agrega una región, cambia la prioridad de conmutación por error y desencadena una conmutación por error manual.|
| [Claves de cuenta y cadenas de conexión](../scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | Muestra las claves de cuenta y las claves de solo lectura, vuelve a generar las claves y enumera las cadenas de conexión.|
| [Protección con firewall de IP](../scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una cuenta de Cosmos con firewall de IP configurado.|
| [Proteger una cuenta nueva con puntos de conexión de servicio](../scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una cuenta de Cosmos y la protege con puntos de conexión de servicio.|
| [Proteger una cuenta existente con puntos de conexión de servicio](../scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| Actualiza una cuenta de Cosmos para protegerla con puntos de conexión de servicio cuando la subred se configura finalmente.|
|||

## <a name="core-sql-api-samples"></a>Ejemplos de API Core (SQL)

|Tarea | Descripción |
|---|---|
| [Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos](../scripts/cli/sql/create.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una cuenta, una base de datos y un contenedor de API Core (SQL) de Azure Cosmos DB. |
| [Creación de una cuenta, una base de datos y un contenedor de Azure Cosmos con escalabilidad automática](../scripts/cli/sql/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| Crea una cuenta, una base de datos y un contenedor con escalabilidad automática para API Core (SQL) de Azure Cosmos DB. |
| [Operaciones de capacidad de proceso](../scripts/cli/sql/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | Lee, actualiza y migra entre la escalabilidad automática y la capacidad de proceso estándar en una base de datos y un contenedor.|
| [Bloquear recursos contra la eliminación](../scripts/cli/sql/lock.md?toc=%2fcli%2fazure%2ftoc.json)| Bloquee los recursos para impedir que se eliminen.|
|||
