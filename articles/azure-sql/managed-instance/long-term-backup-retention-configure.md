---
title: Retención de copias de seguridad a largo plazo de Azure SQL Managed Instance
description: Obtenga información sobre cómo almacenar y restaurar copias de seguridad automatizadas en contenedores de Azure Blob Storage independientes para una instancia administrada de Azure SQL mediante Azure Portal y PowerShell.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: backup-restore
ms.custom: devx-track-azurepowershell
ms.devlang: ''
ms.topic: how-to
author: SQLSourabh
ms.author: sourabha
ms.reviewer: mathoma
ms.date: 09/12/2021
ms.openlocfilehash: 25040c5017a4d0a990d9b1fddb342cab94c97e85
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130239385"
---
# <a name="manage-azure-sql-managed-instance-long-term-backup-retention"></a>Administración de la retención de las copias de seguridad a largo plazo de Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

En Azure SQL Managed Instance, puede configurar una directiva de [retención de copias de seguridad a largo plazo](../database/long-term-retention-overview.md) (LTR) como característica en versión preliminar pública. Esto permite conservar automáticamente copias de seguridad de bases de datos en contenedores de Azure Blob Storage independientes durante un máximo de 10 años. Posteriormente, puede recuperar una base de datos mediante esas copias de seguridad con Azure Portal y PowerShell.

   > [!IMPORTANT]
   > La LTR para las instancias administradas está disponible actualmente en versión preliminar pública en las regiones públicas de Azure. 

En las secciones siguientes se explica cómo usar Azure Portal, PowerShell y la CLI de Azure para configurar la retención de copias de seguridad a largo plazo, ver las copias de seguridad en el almacenamiento de Azure SQL y realizar una restauración a partir de una copia de seguridad del almacenamiento de Azure SQL.

## <a name="prerequisites"></a>Prerrequisitos

# <a name="portal"></a>[Portal](#tab/portal)

Una suscripción de Azure activa.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Prepare el entorno para la CLI de Azure.

[!INCLUDE[azure-cli-prepare-your-environment-no-header](../../../includes/azure-cli-prepare-your-environment-no-header.md)]

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Prepare el entorno para PowerShell.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database; sin embargo, todo el desarrollo futuro se realizará en el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos.

En **Get-AzSqlInstanceDatabaseLongTermRetentionBackup** y **Restore-AzSqlInstanceDatabase**, deberá tener uno de los siguientes roles:

- Rol de propietario de la suscripción o
- rol de colaborador de instancia administrada o
- Rol personalizado con los permisos siguientes:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionManagedInstanceBackups/read`
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/read`

En **Remove-AzSqlInstanceDatabaseLongTermRetentionBackup**, deberá tener uno de los roles siguientes:

- Rol de propietario de la suscripción o
- Rol personalizado con el permiso siguiente:
  - `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

> [!NOTE]
> El rol de colaborador de instancia administrada no tiene permiso para eliminar las copias de seguridad de LTR.

Se pueden conceder permisos de Azure RBAC en el ámbito de la *suscripción* o del *grupo de recursos*. Sin embargo, para acceder a las copias de seguridad de LTR que pertenecen a una instancia descartada, se debe conceder el permiso en el ámbito de la *suscripción* de dicha instancia.

- `Microsoft.Sql/locations/longTermRetentionManagedInstances/longTermRetentionDatabases/longTermRetentionManagedInstanceBackups/delete`

---

## <a name="create-long-term-retention-policies"></a>Creación de directivas de retención a largo plazo

Puede configurar SQL Managed Instance para [retener copias de seguridad automatizadas](../database/long-term-retention-overview.md) durante un período superior al período de retención del nivel de servicio.

# <a name="portal"></a>[Portal](#tab/portal)

1. En Azure Portal, seleccione la instancia administrada y, después, haga clic en **Copias de seguridad**. En la pestaña **Directivas de retención**, seleccione las bases de datos en las que quiere establecer o modificar directivas de retención de copias de seguridad a largo plazo. Los cambios no se aplicarán a las bases de datos que queden sin seleccionar. 

   ![vínculo para administrar copias de seguridad](./media/long-term-backup-retention-configure/ltr-configure-ltr.png)

2. En el panel **Configurar directivas**, especifique el período de retención que quiera para las copias de seguridad semanales, mensuales o anuales. Seleccione un período de retención de "0" para indicar que no se debe establecer una retención de copias de seguridad a largo plazo.

   ![configurar directivas](./media/long-term-backup-retention-configure/ltr-configure-policies.png)

3. Cuando haya terminado, haga clic en **Aplicar**.

> [!IMPORTANT]
> Cuando se habilita una directiva de retención de copia de seguridad a largo plazo, la primera copia de seguridad puede tardar hasta 7 días en mostrarse y estar disponible para restaurar. Para más información sobre la cadencia de retención de copia de seguridad a largo plazo, vea [Retención de copia de seguridad a largo plazo](../database/long-term-retention-overview.md).

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Ejecute el comando [az sql midb show](/cli/azure/sql/midb#az_sql_midb_show) para obtener los detalles de la base de datos de Instancia administrada.

    ```azurecli
    az sql midb show /
    --resource-group mygroup /
    --managed-instance myinstance /
    --name mymanageddb /
    --subscription mysubscription
    ```

2. Ejecute el comando [az sql midb ltr-policy set](/cli/azure/sql/midb/ltr-policy#az_sql_midb_ltr_policy_set) para crear una directiva de LTR. En el ejemplo siguiente se establece una directiva de retención a largo plazo durante 12 semanas para la copia de seguridad semanal.

    ```azurecli
    az sql midb ltr-policy set /
    --resource-group mygroup /
    --managed-instance myinstance /
    --name mymanageddb /
    --weekly-retention "P12W"
    ```

    En este ejemplo se establece una directiva de retención de 12 semanas para la copia de seguridad semanal, de 5 años para la copia de seguridad anual y la semana del 15 de abril en la que se va a realizar la copia de seguridad de LTR anual.

    ```azurecli
    az sql midb ltr-policy set /
    --resource-group mygroup /
    --managed-instance myinstance /
    --name mymanageddb /
    --weekly-retention "P12W" /
    --yearly-retention "P5Y" /
    --week-of-year 16
    ```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
# get the Managed Instance
$subId = "<subscriptionId>"
$instanceName = "<instanceName>"
$resourceGroup = "<resourceGroupName>"
$dbName = "<databaseName>"

Connect-AzAccount

Select-AzSubscription -SubscriptionId $subId

$instance = Get-AzSqlInstance -Name $instanceName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
$LTRPolicy = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup 
    WeeklyRetention = 'P12W'
}
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetention = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
$LTRPolicy = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup 
    WeeklyRetention = 'P12W' 
    YearlyRetention = 'P5Y' 
    WeekOfYear = '16'
}
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy
```

---

## <a name="view-backups-and-restore-from-a-backup"></a>Visualización y restauración de copias de seguridad

# <a name="portal"></a>[Portal](#tab/portal)

Vea las copias de seguridad que se han conservado para una base de datos concreta con una directiva LTR y realice la restauración a partir de esas copias de seguridad.

1. En Azure Portal, seleccione la instancia administrada y, después, haga clic en **Copias de seguridad**. En la pestaña **Copias de seguridad disponibles**, seleccione la base de datos de la que desea ver las copias de seguridad disponibles. Haga clic en **Administrar**.

   ![seleccionar base de datos](./media/long-term-backup-retention-configure/ltr-available-backups-select-database.png)

1. En el panel **Administrar copias de seguridad**, revise las copias de seguridad disponibles.

   ![ver copias de seguridad](./media/long-term-backup-retention-configure/ltr-available-backups.png)

1. Seleccione la copia de seguridad desde la que desea restaurar, haga clic en **Restaurar** y, a continuación, especifique el nuevo nombre de la base de datos. En esta página, la copia de seguridad y el origen se rellenarán previamente. 

   ![Selección de copia de seguridad para restaurar](./media/long-term-backup-retention-configure/ltr-available-backups-restore.png)
   
   ![Restauración](./media/long-term-backup-retention-configure/ltr-restore.png)

1. Haga clic en **Revisar y crear** para revisar los detalles de la restauración. Haga clic en **Crear** para restaurar la base de datos a partir de la copia de seguridad elegida.

1. En la barra de herramientas, haga clic en el icono de notificación para ver el estado del trabajo de restauración.

   ![progreso del trabajo de restauración](./media/long-term-backup-retention-configure/restore-job-progress-long-term.png)

1. Cuando se complete el trabajo de restauración, abra la página **Managed Instance Overview** (Visión general de la instancia administrada) para ver la base de datos recién restaurada.

> [!NOTE]
> Desde aquí, puede conectarse a la base de datos restaurada mediante SQL Server Management Studio para realizar las tareas necesarias, como [extraer un bit de datos de la base de datos restaurada para copiarlo en la base de datos existente o para eliminar la base de datos existente y cambiar el nombre de la base de datos restaurada por el nombre de la base de datos existente](../database/recovery-using-backups.md#point-in-time-restore).

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

### <a name="view-ltr-policies"></a>Visualización de directivas de LTR

Ejecute el comando [az sql midb ltr-policy show](/cli/azure/sql/midb/ltr-policy#az_sql_midb_ltr_policy_show) para ver la directiva de LTR de una base de datos única dentro de una instancia.

```azurecli
az sql midb ltr-policy show \
    --resource-group mygroup \
    --managed-instance myinstance \
    --name mymanageddb
```

### <a name="view-ltr-backups"></a>Visualización de copias de seguridad de LTR

Use el comando [az sql midb ltr-backup list](/cli/azure/sql/midb/ltr-backup#az_sql_midb_ltr_backup_list) para ver las copias de seguridad de LTR dentro de una instancia.

```azurecli
az sql midb ltr-backup list \
    --resource-group mygroup \
    --location eastus2 \
    --managed-instance myinstance
```

### <a name="delete-ltr-backups"></a>Eliminación de copias de seguridad de LTR

Ejecute el comando [az sql midb ltr-backup delete](/cli/azure/sql/midb/ltr-backup#az_sql_midb_ltr_backup_delete) para quitar una copia de seguridad de LTR. Puede ejecutar [az sql midb ltr-backup list](/cli/azure/sql/midb/ltr-backup#az_sql_midb_ltr_backup_list) para obtener la copia de seguridad `name`.

```azurecli
az sql midb ltr-backup delete \
    --location eastus2 \
    --managed-instance myinstance \
    --database mymanageddb \
    --name "3214b3fb-fba9-43e7-96a3-09e35ffcb336;132292152080000000"
```

> [!IMPORTANT]
> La eliminación de la copia de seguridad de LTR no es reversible. Para eliminar una copia de seguridad de LTR una vez eliminada la instancia, debe tener el permiso del ámbito de la suscripción. Puede configurar notificaciones sobre cada eliminación en Azure Monitor filtrando por la operación "Elimina una copia de seguridad de retención a largo plazo". El registro de actividad contiene información sobre quién y cuándo realizó la solicitud. Consulte [Creación de alertas del registro de actividad](../../azure-monitor/alerts/alerts-activity-log.md) para obtener instrucciones detalladas.

### <a name="restore-from-ltr-backups"></a>Restauración desde copias de seguridad de LTR

Ejecute el comando [az sql midb ltr-backup restore](/cli/azure/sql/midb/ltr-backup#az_sql_midb_ltr_backup_restore) para restaurar la base de datos a partir de una copia de seguridad de LTR. Puede ejecutar [az sql midb ltr-backup show](/cli/azure/sql/midb/ltr-backup#az_sql_midb_ltr_backup_show) para obtener `backup-id`.

1. Cree una variable para `backup-id` con el comando `az sql db ltr-backup show` para su uso futuro.

   ```azurecli
    get_backup_id=$(az sql midb ltr-backup show
       --location eastus2 \
       --managed-instance myinstance \
       --database mydb \
       --name "3214b3fb-fba9-43e7-96a3-09e35ffcb336;132292152080000000" \
       --query 'id' \
       --output tsv)
   ```
2. Restauración de la base de datos a partir de una copia de seguridad de LTR

    ```azurecli
    az sql midb ltr-backup restore \
        --dest-database targetmidb \
        --dest-mi myinstance \
        --dest-resource-group mygroup \
        --backup-id $get_backup_id
    ```

> [!IMPORTANT]
> Para restaurar a partir de una copia de seguridad de LTR una vez eliminada la instancia, debe tener los permisos de la suscripción de la instancia de SQL Server y dicha suscripción debe estar activa.

> [!NOTE]
> Desde aquí, puede conectarse a la base de datos restaurada mediante SQL Server Management Studio para realizar las tareas necesarias, como extraer un bit de datos de la base de datos restaurada para copiarlo en la base de datos existente o para eliminar la base de datos existente y cambiar el nombre de la base de datos restaurada por el nombre de la base de datos existente. Consulte la [restauración a un momento dado](../database/recovery-using-backups.md#point-in-time-restore).

# <a name="powershell"></a>[PowerShell](#tab/powershell)

### <a name="view-ltr-policies"></a>Visualización de directivas de LTR

En este ejemplo se muestra cómo enumerar las directivas de LTR de una instancia para una base de datos única.

```powershell
# gets the current version of LTR policy for a database
$LTRPolicies = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup
}
Get-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy 
```

En este ejemplo se muestra cómo enumerar las directivas de LTR para todas las bases de datos de una instancia.

```powershell
# gets the current version of LTR policy for all of the databases on an instance

$Databases = Get-AzSqlInstanceDatabase -ResourceGroupName $resourceGroup -InstanceName $instanceName

$LTRParams = @{
    InstanceName = $instanceName
    ResourceGroupName = $resourceGroup
}

foreach($database in $Databases.Name){
    Get-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRParams  -DatabaseName $database
 }
```

### <a name="clear-an-ltr-policy"></a>Borrado de una directiva LTR

En este ejemplo se muestra cómo borrar una directiva de LTR de una base de datos.

```powershell
# remove the LTR policy from a database
$LTRPolicy = @{
    InstanceName = $instanceName 
    DatabaseName = $dbName 
    ResourceGroupName = $resourceGroup 
    RemovePolicy = $true
}
Set-AzSqlInstanceDatabaseBackupLongTermRetentionPolicy @LTRPolicy
```

### <a name="view-ltr-backups"></a>Visualización de copias de seguridad de LTR

Este ejemplo muestra cómo enumerar las copias de seguridad de LTR de una instancia.

```powershell

$instance = Get-AzSqlInstance -Name $instanceName -ResourceGroupName $resourceGroup

# get the list of all LTR backups in a specific Azure region
# backups are grouped by the logical database id, within each group they are ordered by the timestamp, the earliest backup first
Get-AzSqlInstanceDatabaseLongTermRetentionBackup -Location $instance.Location

# get the list of LTR backups from the Azure region under the given managed instance
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam

# get the LTR backups for a specific database from the Azure region under the given managed instance
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    DatabaseName = $dbName
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam

# list LTR backups only from live databases (you have option to choose All/Live/Deleted)
$LTRBackupParam = @{
    Location = $instance.Location 
    DatabaseState = 'Live'
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam

# only list the latest LTR backup for each database
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    OnlyLatestPerDatabase = $true
}
Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam 
```

### <a name="delete-ltr-backups"></a>Eliminación de copias de seguridad de LTR

En este ejemplo se muestra cómo eliminar una copia de seguridad de LTR desde la lista de copias de seguridad.

```powershell
# remove the earliest backup
# get the LTR backups for a specific database from the Azure region under the given managed instance
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    DatabaseName = $dbName
}
$ltrBackups = Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam
$ltrBackup = $ltrBackups[0]
Remove-AzSqlInstanceDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

> [!IMPORTANT]
> La eliminación de la copia de seguridad de LTR no es reversible. Para eliminar una copia de seguridad de LTR una vez eliminada la instancia, debe tener el permiso del ámbito de la suscripción. Puede configurar notificaciones sobre cada eliminación en Azure Monitor filtrando por la operación "Elimina una copia de seguridad de retención a largo plazo". El registro de actividad contiene información sobre quién y cuándo realizó la solicitud. Consulte [Creación de alertas del registro de actividad](../../azure-monitor/alerts/alerts-activity-log.md) para obtener instrucciones detalladas.

### <a name="restore-from-ltr-backups"></a>Restauración desde copias de seguridad de LTR

Este ejemplo muestra cómo restaurar desde una copia de seguridad de LTR. Observe que aunque esta interfaz no cambió, el parámetro de id. de recurso requiere ahora el id. de recurso de la copia de seguridad de LTR.

```powershell
# restore a specific LTR backup as an P1 database on the instance $instanceName of the resource group $resourceGroup
$LTRBackupParam = @{
    Location = $instance.Location 
    InstanceName = $instanceName
    DatabaseName = $dbname
    OnlyLatestPerDatabase = $true
}
$ltrBackup = Get-AzSqlInstanceDatabaseLongTermRetentionBackup @LTRBackupParam 

$RestoreLTRParam = @{
    TargetInstanceName          = $instanceName 
    TargetResourceGroupName     = $resourceGroup 
    TargetInstanceDatabaseName  = $dbName
    FromLongTermRetentionBackup = $true
    ResourceId                  = $ltrBackup.ResourceId 
}
Restore-AzSqlInstanceDatabase @RestoreLTRParam
```

> [!IMPORTANT]
> Para restaurar a partir de una copia de seguridad de LTR una vez eliminada la instancia, debe tener los permisos de la suscripción de la instancia de SQL Server y dicha suscripción debe estar activa. También debe omitir el parámetro -ResourceGroupName opcional.

> [!NOTE]
> Desde aquí, puede conectarse a la base de datos restaurada mediante SQL Server Management Studio para realizar las tareas necesarias, como extraer un bit de datos de la base de datos restaurada para copiarlo en la base de datos existente o para eliminar la base de datos existente y cambiar el nombre de la base de datos restaurada por el nombre de la base de datos existente. Consulte la [restauración a un momento dado](../database/recovery-using-backups.md#point-in-time-restore).

---

## <a name="next-steps"></a>Pasos siguientes

- Para aprender sobre las copias de seguridad automáticas generadas por el servicio, consulte [copias de seguridad automáticas](../database/automated-backups-overview.md)
- Para más información sobre la retención de copia de seguridad a largo plazo, consulte sobre la [retención de copia de seguridad a largo plazo](../database/long-term-retention-overview.md).
