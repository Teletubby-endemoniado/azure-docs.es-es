---
title: 'PowerShell: Configuración de la replicación geográfica activa para Azure SQL Database'
description: Utilice un script de ejemplo de Azure PowerShell para configurar la replicación geográfica activa para Azure SQL Database y para conmutarla por error.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1, devx-track-azurepowershell
ms.devlang: PowerShell
ms.topic: sample
author: emlisa
ms.author: emlisa
ms.reviewer: mathoma
ms.date: 03/12/2019
ms.openlocfilehash: de6e32476e5b70433d76518bd9b7c462ded0ebe1
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130167368"
---
# <a name="use-powershell-to-configure-active-geo-replication-for-a-database-in-azure-sql-database"></a>Uso de PowerShell para configurar la replicación geográfica activa para una base de datos en Azure SQL Database

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

En este script de ejemplo de Azure PowerShell se configura la replicación geográfica activa para una base de datos en Azure SQL Database y se conmuta por error a una réplica secundaria de la base de datos.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar PowerShell de manera local, en este tutorial se requiere la versión 1.4.0 de Azure PowerShell o posterior. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

## <a name="sample-scripts"></a>Muestras de scripts

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/setup-geodr-and-failover/setup-geodr-and-failover-single-database.ps1?highlight=18-21 "Set up active geo-replication for single database")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Use el siguiente comando para quitar el grupo de recursos y todos los recursos que tenga asociados.

```powershell
Remove-AzResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Crea un servidor que hospeda las bases de datos y los grupos elásticos. |
| [New-AzSqlElasticPool](/powershell/module/az.sql/new-azsqlelasticpool) | Crea un grupo elástico. |
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | Actualiza las propiedades de la base de datos o traslada una base de datos a un grupo elástico, fuera de este o entre grupos elásticos. |
| [New-AzSqlDatabaseSecondary](/powershell/module/az.sql/new-azsqldatabasesecondary)| Crea una base de datos secundaria para una base de datos existente e inicia la replicación de datos. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase)| Obtiene una o más bases de datos. |
| [Set-AzSqlDatabaseSecondary](/powershell/module/az.sql/set-azsqldatabasesecondary)| Convierte una base de datos secundaria en principal para iniciar la conmutación por error.|
| [Get-AzSqlDatabaseReplicationLink](/powershell/module/az.sql/get-azsqldatabasereplicationlink) | Obtiene los vínculos de replicación geográfica entre una instancia de Azure SQL Database y un grupo de recursos o servidor SQL lógico. |
| [Remove-AzSqlDatabaseSecondary](/powershell/module/az.sql/remove-azsqldatabasesecondary) | Finaliza una replicación de datos entre una base de datos y la base de datos secundaria especificada. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/).

Encontrará más ejemplos de scripts de PowerShell de SQL Database en los [scripts de PowerShell de Azure SQL Database](../powershell-script-content-guide.md).
