---
title: 'Inicio rápido: Pausa y reanudación del proceso en un grupo de SQL dedicado (anteriormente SQL DW) con Azure PowerShell'
description: Puede utilizar Azure PowerShell para pausar y reanudar un grupo de SQL dedicado (anteriormente SQL DW). (almacenamiento de datos).
services: synapse-analytics
author: julieMSFT
ms.author: jrasnick
manager: craigg
ms.reviewer: igorstan
ms.date: 03/20/2019
ms.topic: quickstart
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.custom: devx-track-azurepowershell, seo-lt-2019, azure-synapse, mode-api
ms.openlocfilehash: 5d0aab60ffc11fbc2ef5fe32055403b15f679b30
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131003511"
---
# <a name="quickstart-pause-and-resume-compute-in-dedicated-sql-pool-formerly-sql-dw-with-azure-powershell"></a>Inicio rápido: Pausa y reanudación del proceso en un grupo de SQL dedicado (anteriormente SQL DW) con Azure PowerShell

Puede utilizar Azure PowerShell para pausar y reanudar los recursos de proceso del un grupo de SQL dedicado (anteriormente SQL DW).
Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

En este inicio rápido se da por supuesto que ya tiene un grupo de SQL dedicado (anteriormente SQL DW) que puede pausar y reanudar. Si tiene que crearlo, puede seguir las instrucciones del artículo sobre [creación y conexión desde Azure Portal](create-data-warehouse-portal.md) para crear un grupo de SQL dedicado (anteriormente SQL DW) llamado **mySampleDataWarehouse**.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en la suscripción de Azure con el comando [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) y siga las instrucciones de la pantalla.

```powershell
Connect-AzAccount
```

Para ver qué suscripción está usando, ejecute [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

```powershell
Get-AzSubscription
```

Si necesita usar una suscripción diferente de la predeterminada, ejecute [Set-AzContext](/powershell/module/az.accounts/set-azcontext?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-dedicated-sql-pool-formerly-sql-dw-information"></a>Búsqueda de información del grupo de SQL dedicado (anteriormente SQL DW)

Busque el nombre de la base de datos, el nombre del servidor y el grupo de recursos del grupo de SQL dedicado (anteriormente SQL DW) que tiene previsto pausar y reanudar.

Siga estos pasos para encontrar información de ubicación de un grupo de SQL dedicado (anteriormente SQL DW):

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Haga clic en **Azure Synapse Analytics (formerly SQL DW)** a la izquierda de la página de Azure Portal.
1. Seleccione **mySampleDataWarehouse** en la página de **Azure Synapse Analytics (formerly SQL DW)** . Se abre el grupo de SQL.

    ![Nombre del servidor y grupo de recursos](./media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

1. Escriba el nombre del grupo de SQL dedicado (anteriormente SQL DW), que es el nombre de la base de datos. Además, anote el nombre del servidor y el grupo de recursos.
1. Use solo la primera parte del nombre del servidor en los cmdlets de PowerShell. En la imagen anterior, el nombre completo del servidor es sqlpoolservername.database.windows.net. Se usará **sqlpoolservername** como nombre del servidor en el cmdlet de PowerShell.

## <a name="pause-compute"></a>Pausa del proceso

Para ahorrar costos, puede pausar y reanudar recursos de proceso a petición. Por ejemplo, si no va a usar la base de datos durante la noche y los fines de semana, puede pausarla durante esas horas y reanudarla durante el día.

> [!NOTE]
> No se le cobrará por recursos de proceso mientras la base de datos se encuentre en pausa. Sin embargo, se le seguirá cobrando por el almacenamiento.

Para pausar una base de datos, use el cmdlet [Suspend-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). En el ejemplo siguiente, se pausa un grupo de SQL denominado **mySampleDataWarehouse** hospedado en un servidor denominado **sqlpoolservername**. El servidor está en un grupo de recursos de Azure denominado **myResourceGroup**.

```powershell
Suspend-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
```

En el ejemplo siguiente se recupera la base de datos en el objeto $database. A continuación, canaliza el objeto a [Suspend-AzSqlDatabase](/powershell/module/az.sql/suspend-azsqldatabase?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Los resultados se almacenan en el objeto resultDatabase. El comando final muestra los resultados.

```powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzSqlDatabase
$resultDatabase
```

## <a name="resume-compute"></a>Reanudación del proceso

Para iniciar una base de datos, use el cmdlet [Resume-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). En el ejemplo siguiente se inicia una base de datos denominada **mySampleDataWarehouse** que está hospedada en un servidor denominado **sqlpoolservername**. El servidor está en un grupo de recursos de Azure denominado **myResourceGroup**.

```powershell
Resume-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" -DatabaseName "mySampleDataWarehouse"
```

En el ejemplo siguiente se recupera la base de datos en el objeto $database. A continuación, canaliza el objeto a [Resume-AzSqlDatabase](/powershell/module/az.sql/resume-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) y almacena los resultados en $resultDatabase. El comando final muestra los resultados.

```powershell
$database = Get-AzSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "sqlpoolservername" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Resume-AzSqlDatabase
$resultDatabase
```

## <a name="check-status-of-your-sql-pool-operation"></a>Comprobación del estado de una operación del grupo de SQL

Para comprobar el estado del grupo de SQL dedicado (anteriormente SQL DW), utilice el cmdlet [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/Get-AzSqlDatabaseActivity?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

```powershell
Get-AzSqlDatabaseActivity -ResourceGroupName "myResourceGroup" -ServerName "sqlpoolservername" -DatabaseName "mySampleDataWarehouse"
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Se le cobran las unidades de almacenamiento de datos y los datos almacenados en el grupo de SQL dedicado (anteriormente SQL DW). Estos recursos de proceso y de almacenamiento se facturan por separado.

- Si quiere conservar los datos de almacenamiento, pause el proceso.
- Si quiere eliminar cobros futuros, puede eliminar el grupo de SQL.

Siga estos pasos para limpiar los recursos según estime oportuno.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en el grupo de SQL.

    ![Limpieza de recursos](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Para pausar el proceso, haga clic en el botón **Pausar**. Cuando el grupo de SQL esté en pausa, verá un botón **Iniciar**.  Para reanudar el proceso, haga clic en **Iniciar**.

3. Para quitar el grupo de SQL para que no le cobren por proceso o almacenamiento, haga clic en **Eliminar**.

4. Para quitar el servidor SQL que creó, haga clic en **sqlpoolservername.database.windows.net** y, luego, en **Eliminar**.  Debe tener cuidado con este procedimiento, ya que la eliminación del servidor elimina también todas las bases de datos asignadas al servidor.

5. Para quitar el grupo de recursos, haga clic en **myResourceGroup** y luego haga clic en **Eliminar grupo de recursos**.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el grupo de SQL, diríjase al artículo sobre [carga de datos en un grupo de SQL dedicado (anteriormente SQL DW)](./load-data-from-azure-blob-storage-using-copy.md). Para más información acerca de la administración de funcionalidades de proceso, consulte el artículo sobre [introducción a la administración de proceso](sql-data-warehouse-manage-compute-overview.md).
