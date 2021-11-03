---
title: 'Ejemplo de CLI: grupo de conmutación por error en un grupo elástico de Azure SQL Database'
description: Script de ejemplo de la CLI de Azure para crear un grupo elástico de Azure SQL Database, agregarlo a un grupo de conmutación por error y probar la conmutación por error.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: rothja
ms.author: jroth
ms.reviewer: mathoma
ms.date: 07/16/2019
ms.openlocfilehash: 04389f3cc5604e0412776e2c02b4e3e4fcc8738b
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131013733"
---
# <a name="use-cli-to-add-an-azure-sql-database-elastic-pool-to-a-failover-group"></a>Use la CLI para agregar un grupo elástico de Azure SQL Database a un grupo de conmutación por error

Este ejemplo de script de la CLI de Azure crea una base de datos única, la agrega a un grupo elástico, crea un grupo de conmutación por error y prueba la conmutación por error.

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Ejecute el script.

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/failover-groups/add-elastic-pool-to-failover-group-az-cli.sh "Add elastic pool to a failover group")]

### <a name="clean-up-deployment"></a>Limpieza de la implementación

Use el siguiente comando para quitar el grupo de recursos y todos los recursos que tenga asociados.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Referencia de ejemplo

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Descripción |
|---|---|
| [az sql elastic-pool](/cli/azure/sql/elastic-pool) | Comandos del grupo de bases de datos elásticas. |
| [az sql failover-group ](/cli/azure/sql/failover-group) | Comandos de grupo de conmutación por error. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure/overview).

Encontrará más ejemplos de scripts de la CLI de Azure de SQL Database en los [scripts de la CLI de Azure de Azure SQL Database](../../azure-sql/database/az-cli-script-samples-content-guide.md).
