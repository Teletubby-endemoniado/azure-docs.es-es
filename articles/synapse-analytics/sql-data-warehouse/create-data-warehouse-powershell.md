---
title: 'Inicio rápido: Creación de un grupo de SQL dedicado (anteriormente SQL DW) con Azure PowerShell'
description: Cree rápidamente un grupo de SQL dedicado (anteriormente SQL DW) con una regla de firewall de nivel de servidor mediante Azure PowerShell.
services: synapse-analytics
author: XiaoyuMSFT
ms.author: xiaoyul
manager: craigg
ms.reviewer: igorstan
ms.date: 4/11/2019
ms.topic: quickstart
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.custom: devx-track-azurepowershell, seo-lt-2019, azure-synapse, mode-api
ms.openlocfilehash: 261d8cfe1264b793d68574c5050ef6dcceacb7f7
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131013391"
---
# <a name="quickstart-create-a-dedicated-sql-pool-formerly-sql-dw-with-azure-powershell"></a>Inicio rápido: Creación de un grupo de SQL dedicado (anteriormente SQL DW) con Azure PowerShell

Cree un grupo de SQL dedicado (anteriormente SQL DW) en Azure Synapse Analytics mediante Azure PowerShell.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

> [!IMPORTANT]
> La creación de un grupo de SQL dedicado (anteriormente SQL DW) puede dar lugar a un nuevo servicio facturable.  Para más información, consulte los [precios de Azure Synapse Analytics](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

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

## <a name="create-variables"></a>Creación de variables

Defina las variables que va a usar en los scripts de esta guía de inicio rápido.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The server name: Use a random value or replace with your own value (don't capitalize)
$servername = "server-$(Get-Random)"
# Set an admin name and password for your database
# The sign-in information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDataWarehouse"
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un [grupo de recursos de Azure](../../azure-resource-manager/management/overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) con el comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Un grupo de recursos es un contenedor en el que se implementan y se administran recursos de Azure como un grupo. En el ejemplo siguiente, se crea un grupo de recursos denominado `myResourceGroup` en la ubicación `westeurope`.

```powershell
New-AzResourceGroup -Name $resourcegroupname -Location $location
```

## <a name="create-a-server"></a>Creación de un servidor

Cree un [servidor lógico de SQL](/powershell/module/az.sql/new-azsqlserver?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) con el comando [New-AzSqlServer](../../azure-sql/database/logical-servers.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Un servidor contiene un conjunto de bases de datos administradas como un grupo. En el ejemplo siguiente se crea un servidor con nombre aleatorio en el grupo de recursos con un inicio de sesión de administrador denominado `ServerAdmin` y una contraseña `ChangeYourAdminPassword1`. Cambie estos valores predefinidos por los que prefiera.

```powershell
New-AzSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-level-firewall-rule"></a>Configuración de una regla de firewall de nivel de servidor

Para crear una [regla de firewall de nivel de servidor](../../azure-sql/database/firewall-configure.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json), ejecute el comando [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Las reglas de firewall a nivel de servidor permiten a una aplicación externa, como SQL Server Management Studio o la utilidad SQLCMD, conectarse a un grupo de SQL dedicado (anteriormente SQL DW) a través del firewall del servicio grupo de SQL dedicado.

En el ejemplo siguiente, el firewall está abierto solo para otros recursos de Azure. Para habilitar la conectividad externa, cambie la dirección IP a una dirección apropiada para su entorno. Para abrir todas las direcciones IP, utilice 0.0.0.0 como la dirección IP inicial y 255.255.255.255 como la dirección final.

```powershell
New-AzSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> Los puntos de conexión de SQL se comunican a través del puerto 1433. Si intenta conectarse desde dentro de una red corporativa, es posible que el firewall de la red no permita el tráfico de salida a través del puerto 1433. Si es así, no podrá conectarse al servidor a menos que el departamento de TI abra el puerto 1433.
>

## <a name="create-a-dedicated-sql-pool-formerly-sql-dw"></a>Creación de un grupo de SQL dedicado (anteriormente SQL DW)

En el siguiente ejemplo se crea un grupo de SQL dedicado (anteriormente SQL DW) mediante las variables definidas anteriormente.  Especifica el objetivo del servicio como DW100c, que es un punto de partida de bajo costo para un grupo de SQL dedicado (anteriormente SQL DW).

```powershell
New-AzSqlDatabase `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -Edition "DataWarehouse" `
    -RequestedServiceObjectiveName "DW100c" `
    -CollationName "SQL_Latin1_General_CP1_CI_AS" `
    -MaxSizeBytes 10995116277760
```

Los parámetros obligatorios son:

* **RequestedServiceObjectiveName**: la cantidad de [unidades de almacenamiento de datos](what-is-a-data-warehouse-unit-dwu-cdwu.md) que solicita. Si se aumenta esta cantidad, aumentará el costo de proceso. Para ver una lista de los valores compatibles, consulte los [límites de memoria y simultaneidad](memory-concurrency-limits.md).
* **DatabaseName**: El nombre del grupo de SQL dedicado (anteriormente SQL DW) que va a crear.
* **ServerName**: el nombre del servidor que se usa para la creación.
* **ResourceGroupName**: el grupo de recursos que está usando. Para buscar grupos de recursos que estén disponibles en su suscripción, use Get-AzureResource.
* **Edición**: Debe ser "DataWarehouse" para crear un grupo de SQL dedicado (anteriormente SQL DW).

Los parámetros opcionales son:

* **CollationName**: la intercalación predeterminada cuando no se especifica otra es SQL_Latin1_General_CP1_CI_AS. No se puede cambiar la intercalación de una base de datos.
* **MaxSizeBytes**: el tamaño máximo predeterminado de una base de datos es 240 TB. El tamaño máximo limita los datos del almacén de filas. Hay un almacenamiento ilimitado de los datos de las columnas.

Para más información sobre las opciones de parámetro, consulte [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

## <a name="clean-up-resources"></a>Limpieza de recursos

Otros tutoriales de inicio rápido de esta colección se basan en esta guía.

> [!TIP]
> Si tiene previsto seguir trabajando con otros tutoriales de inicio rápido, no elimine los recursos creados en esta guía de inicio rápido. Si no tiene previsto continuar, siga estos pasos para eliminar todos los recursos creados en esta guía de inicio rápido en Azure Portal.
>

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Pasos siguientes

Ya ha creado un grupo de SQL dedicado (anteriormente SQL DW) y una regla de firewall, y se ha conectado a su grupo de SQL dedicado. Para más información, vaya al artículo [Carga de datos en un grupo de SQL dedicado](./load-data-from-azure-blob-storage-using-copy.md).
