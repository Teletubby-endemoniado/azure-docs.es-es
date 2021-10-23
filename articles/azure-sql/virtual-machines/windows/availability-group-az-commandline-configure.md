---
title: Configuración de un grupo de disponibilidad (PowerShell y CLI de Azure)
description: Use PowerShell o la CLI de Azure para crear el clúster de conmutación por error de Windows, el cliente de escucha de grupo de disponibilidad y el equilibrador de carga interno en una máquina virtual con SQL Server en Azure.
services: virtual-machines-windows
documentationcenter: na
author: rajeshsetlem
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.subservice: hadr
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/20/2020
ms.author: rsetlem
ms.reviewer: mathoma
ms.custom: seo-lt-2019, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 605a95142b74c06ae1b7a78cd832ab535b7345bf
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130167079"
---
# <a name="use-powershell-or-az-cli-to-configure-an-availability-group-for-sql-server-on-azure-vm"></a>Uso de PowerShell o la CLI de Azure para configurar un grupo de disponibilidad para SQL Server en una máquina virtual de Azure 
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

En este artículo se describe cómo usar [PowerShell](/powershell/scripting/install/installing-powershell) o la [CLI de Azure](/cli/azure/sql/vm) para implementar un clúster de conmutación por error de Windows, agregarle máquinas virtuales con SQL Server, así como crear el equilibrador de carga interno y el cliente de escucha de un grupo de disponibilidad Always On. 

La implementación del grupo de disponibilidad se sigue realizando manualmente con SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL). 

Si bien en este artículo se usa PowerShell y la CLI de Azure para configurar el entorno del grupo de disponibilidad, también es posible hacerlo mediante [Azure Portal](availability-group-azure-portal-configure.md), [plantillas de inicio rápido de Azure](availability-group-quickstart-template-configure.md) o [manualmente](availability-group-manually-configure-tutorial.md). 

> [!NOTE]
> Ahora es posible migrar mediante lift-and-shift la solución de grupo de disponibilidad a SQL Server en máquinas virtuales de Azure mediante Azure Migrate. Para más información, consulte [Migración del grupo de disponibilidad](../../migration-guides/virtual-machines/sql-server-availability-group-to-sql-on-azure-vm.md). 

## <a name="prerequisites"></a>Requisitos previos

Para configurar un grupo de disponibilidad Always On, debe cumplir los siguientes requisitos previos: 

- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- Un grupo de recursos con un controlador de dominio. 
- Una o varias [máquinas virtuales en Azure, unidas a un dominio, con SQL Server 2016 (o superior) Enterprise Edition](./create-sql-vm-portal.md) en el *mismo* conjunto de disponibilidad o en zonas de disponibilidad *distintas* que se hayan [registrado con la extensión Agente de IaaS de SQL](sql-agent-extension-manually-register-single-vm.md).  
- La versión más reciente de [PowerShell](/powershell/scripting/install/installing-powershell) o la [CLI de Azure](/cli/azure/install-azure-cli). 
- Dos direcciones IP disponibles (no utilizadas por ninguna entidad). Una es para el equilibrador de carga interno. La otra es para el cliente de escucha de grupo de disponibilidad dentro de la misma subred que el grupo de disponibilidad. Si se usa un equilibrador de carga existente, solo se necesita una dirección IP disponible para la escucha de grupo de disponibilidad. 

## <a name="permissions"></a>Permisos

Los siguientes permisos de cuenta son necesarios para configurar el grupo de disponibilidad Always On mediante la CLI de Azure: 

- Una cuenta de usuario de dominio existente que tenga el permiso **Crear objeto de equipo** en el dominio. Por ejemplo, una cuenta de administrador de dominio normalmente tiene permisos suficientes (como account@domain.com). _Esta cuenta también debe formar parte del grupo de administradores local en cada máquina virtual para crear el clúster._
- La cuenta de usuario del dominio que controla SQL Server. 
 
## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento 

El clúster necesita una cuenta de almacenamiento para que actúe como el testigo en la nube. Puede usar una cuenta de almacenamiento existente o crear una desde cero. Si quiere utilizar una cuenta de almacenamiento existente, avance a la siguiente sección. 

Con el siguiente fragmento de código se crea una cuenta de almacenamiento: 

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interactive
# Create the storage account
# example: az storage account create -n 'cloudwitness' -g SQLVM-RG -l 'West US' `
#  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true

az storage account create -n <name> -g <resource group name> -l <region> `
  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true
```

>[!TIP]
> Puede aparecer el error `az sql: 'vm' is not in the 'az sql' command group` si usa una versión obsoleta de la CLI de Azure. Descargue la [versión más reciente de la CLI de Azure](/cli/azure/install-azure-cli-windows) para solucionar este error.


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell-interactive
# Create the storage account
# example: New-AzStorageAccount -ResourceGroupName SQLVM-RG -Name cloudwitness `
#    -SkuName Standard_LRS -Location West US -Kind StorageV2 `
#    -AccessTier Hot -EnableHttpsTrafficOnly $true

New-AzStorageAccount -ResourceGroupName <resource group name> -Name <name> `
    -SkuName Standard_LRS -Location <region> -Kind StorageV2 `
    -AccessTier Hot -EnableHttpsTrafficOnly $true
```

---

## <a name="define-cluster-metadata"></a>Definición de metadatos de clúster

El grupo de comandos [az sql vm group](/cli/azure/sql/vm/group) de la CLI de Azure administra los metadatos del servicio Clúster de conmutación por error de Windows Server (WSFC) que hospeda el grupo de disponibilidad. Los metadatos del clúster engloban el dominio de Active Directory, las cuentas de clúster, las cuentas de almacenamiento que se van a usar como testigo en la nube y la versión de SQL Server. Use [az sql vm group create](/cli/azure/sql/vm/group#az_sql_vm_group_create) para definir los metadatos del WSFC de forma que, cuando se agregue la primera máquina virtual con SQL Server, el clúster se cree tal y como esté definido. 

Con el siguiente fragmento de código se definen los metadatos del clúster:

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interactive
# Define the cluster metadata
# example: az sql vm group create -n Cluster -l 'West US' -g SQLVM-RG `
#  --image-offer SQL2017-WS2016 --image-sku Enterprise --domain-fqdn domain.com `
#  --operator-acc vmadmin@domain.com --bootstrap-acc vmadmin@domain.com --service-acc sqlservice@domain.com `
#  --sa-key '4Z4/i1Dn8/bpbseyWX' `
#  --storage-account 'https://cloudwitness.blob.core.windows.net/'

az sql vm group create -n <cluster name> -l <region ex:eastus> -g <resource group name> `
  --image-offer <SQL2016-WS2016 or SQL2017-WS2016> --image-sku Enterprise --domain-fqdn <FQDN ex: domain.com> `
  --operator-acc <domain account ex: testop@domain.com> --bootstrap-acc <domain account ex:bootacc@domain.com> `
  --service-acc <service account ex: testservice@domain.com> `
  --sa-key '<PublicKey>' `
  --storage-account '<ex:https://cloudwitness.blob.core.windows.net/>'
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell-interactive
# Define the cluster metadata
# example: $group = New-AzSqlVMGroup -Name Cluster -Location West US ' 
#  -ResourceGroupName SQLVM-RG -Offer SQL2017-WS2016
#  -Sku Enterprise -DomainFqdn domain.com -ClusterOperatorAccount vmadmin@domain.com
#  -ClusterBootstrapAccount vmadmin@domain.com  -SqlServiceAccount sqlservice@domain.com 
#  -StorageAccountUrl '<ex:https://cloudwitness.blob.core.windows.net/>' `
#  -StorageAccountPrimaryKey '4Z4/i1Dn8/bpbseyWX'

$storageAccountPrimaryKey = ConvertTo-SecureString -String "<PublicKey>" -AsPlainText -Force
$group = New-AzSqlVMGroup -Name <name> -Location <regio> 
  -ResourceGroupName <resource group name> -Offer <SQL201?-WS201?> 
  -Sku Enterprise -DomainFqdn <FQDN> -ClusterOperatorAccount <domain account> 
  -ClusterBootstrapAccount <domain account>  -SqlServiceAccount <service account> 
  -StorageAccountUrl '<ex:StorageAccountUrl>' `
  -StorageAccountPrimaryKey $storageAccountPrimaryKey
```

---

## <a name="add-vms-to-the-cluster"></a>Adición de máquinas virtuales al clúster

Al agregar la primera máquina virtual de SQL Server al clúster, se crea el clúster. El comando [az sql vm add-to-group](/cli/azure/sql/vm#az_sql-vm_add_to_group) crea el clúster con el nombre que se le haya dado, instala el rol de clúster en las máquinas virtuales de SQL Server y las agrega al clúster. Los usos posteriores del comando `az sql vm add-to-group` hacen que se agreguen más máquinas virtuales con SQL Server al clúster recién creado. 

Con el siguiente fragmento de código se crea el clúster y se agrega a él la primera máquina virtual de SQL Server: 

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interactive
# Add SQL Server VMs to cluster
# example: az sql vm add-to-group -n SQLVM1 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!
# example: az sql vm add-to-group -n SQLVM2 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

az sql vm add-to-group -n <VM1 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
az sql vm add-to-group -n <VM2 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
```
Use este comando para agregar cualquier otra máquina virtual con SQL Server al clúster. Modifique solo el parámetro `-n` para el nombre de la máquina virtual con SQL Server. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell-interactive
# Add SQL Server VMs to cluster
# example: $sqlvm1 = Get-AzSqlVM -Name SQLVM1 -ResourceGroupName SQLVM-RG
# $sqlvm2 = Get-AzSqlVM -Name SQLVM2  -ResourceGroupName SQLVM-RG

# $sqlvmconfig1 = Set-AzSqlVMConfigGroup -SqlVM $sqlvm1 `
#  -SqlVMGroup $group -ClusterOperatorAccountPassword Str0ngAzur3P@ssword! `
#  -SqlServiceAccountPassword Str0ngAzur3P@ssword! `
#  -ClusterBootstrapAccountPassword Str0ngAzur3P@ssword!

# $sqlvmconfig2 = Set-AzSqlVMConfigGroup -SqlVM $sqlvm2 `
#  -SqlVMGroup $group -ClusterOperatorAccountPassword Str0ngAzur3P@ssword! `
#  -SqlServiceAccountPassword Str0ngAzur3P@ssword! `
#  - ClusterBootstrapAccountPassword Str0ngAzur3P@ssword!

$sqlvm1 = Get-AzSqlVM -Name <VM1 Name> -ResourceGroupName <Resource Group Name>
$sqlvm2 = Get-AzSqlVM -Name <VM2 Name> -ResourceGroupName <Resource Group Name>

$sqlvmconfig1 = Set-AzSqlVMConfigGroup -SqlVM $sqlvm1 `
   -SqlVMGroup $group -ClusterOperatorAccountPassword <operator account password> `
   -SqlServiceAccountPassword <service account password> `
   -ClusterBootstrapAccountPassword <bootstrap account password>

Update-AzSqlVM -ResourceId $sqlvm1.ResourceId -SqlVM $sqlvmconfig1

$sqlvmconfig2 = Set-AzSqlVMConfigGroup -SqlVM $sqlvm2 `
   -SqlVMGroup $group -ClusterOperatorAccountPassword <operator account password> `
   -SqlServiceAccountPassword <service account password> `
   -ClusterBootstrapAccountPassword <bootstrap account password>

Update-AzSqlVM -ResourceId $sqlvm2.ResourceId -SqlVM $sqlvmconfig2
```

---

## <a name="configure-quorum"></a>Configuración de un cuórum

Aunque el testigo de disco es la opción de cuórum más resistente, requiere un disco compartido de Azure que impone algunas limitaciones al grupo de disponibilidad. Por lo tanto, el testigo en la nube es la solución de cuórum recomendada para los clústeres que hospedan grupos de disponibilidad de SQL Server en VM de Azure. 

Si tiene un número par de votos en el clúster, configure la [solución de cuórum](hadr-cluster-quorum-configure-how-to.md) que mejor se adapte a sus necesidades empresariales. Para más información, consulte [Cuórum con VM SQL Server](hadr-windows-server-failover-cluster-overview.md#quorum). 


## <a name="validate-cluster"></a>Validar el clúster 

Para que un clúster de conmutación por error sea compatible con Microsoft, debe pasar la validación del clúster. Conéctese a la máquina virtual mediante el método que prefiera, por ejemplo, Protocolo de escritorio remoto (RDP), y compruebe que el clúster pasa la validación antes de continuar. Si no lo hace, el clúster queda en un estado no admitido. 

Puede validar el clúster mediante el Administrador de clústeres de conmutación por error (FCM) o el siguiente comando de PowerShell:

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Inventory", "Network", "System Configuration"
   ```

## <a name="create-availability-group"></a>Crear grupo de disponibilidad

Cree el grupo de disponibilidad manualmente del modo habitual, ya sea mediante [SQL Server Management Studio](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio), [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell) o [Transact-SQL](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql). 

>[!IMPORTANT]
> *No* cree un cliente de escucha en este momento, ya que esto se realiza mediante la CLI de Azure en las secciones siguientes.  

## <a name="create-internal-load-balancer"></a>Creación del equilibrador de carga interno

[!INCLUDE [sql-ag-use-dnn-listener](../../includes/sql-ag-use-dnn-listener.md)]

El cliente de escucha de grupo de disponibilidad Always On requiere una instancia interna de Azure Load Balancer. El equilibrador de carga interno proporciona una dirección IP "flotante" para el cliente de escucha de grupo de disponibilidad, que permite conmutar por error y volver a conectarse de manera más rápida. Si las máquinas virtuales con SQL Server de un grupo de disponibilidad forman parte del mismo conjunto de disponibilidad, puede usar un equilibrador de carga básico. De lo contrario, debe usar uno estándar.  

> [!NOTE]
> El equilibrador de carga interno debe estar en la misma red virtual que las instancias de máquina virtual con SQL Server. 

Con el siguiente fragmento de código se crea la instancia del equilibrador de carga interno:

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interactive
# Create the internal load balancer
# example: az network lb create --name sqlILB -g SQLVM-RG --sku Standard `
# --vnet-name SQLVMvNet --subnet default

az network lb create --name sqlILB -g <resource group name> --sku Standard `
  --vnet-name <VNet Name> --subnet <subnet name>
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell-interactive
# Create the internal load balancer
# example: New-AzLoadBalancer -name sqlILB -ResourceGroupName SQLVM-RG  `
#  -Sku Standard -Location West US

New-AzLoadBalancer -name sqlILB -ResourceGroupName <resource group name> `
   -Sku Standard -Location <region>
```

---

>[!IMPORTANT]
> El recurso de IP pública de cada máquina virtual con SQL Server debe tener una SKU estándar para que sea compatible con el equilibrador de carga estándar. Para determinar la SKU del recurso de IP pública de la máquina virtual, vaya a **Grupo de recursos**, seleccione su recurso **Dirección IP pública** para la máquina virtual con SQL Server que desee y busque el valor que aparece debajo de **SKU** en el panel **Información general**.  

## <a name="create-listener"></a>Eliminar el agente de escucha

Después de haber creado el grupo de disponibilidad manualmente, puede crear el cliente de escucha mediante [az sql vm ag-listener](/cli/azure/sql/vm/group/ag-listener#az_sql_vm_group_ag_listener_create). 

El *identificador de recurso de subred* es el valor de `/subnets/<subnetname>` anexado al identificador de recurso del recurso de red virtual. Para detectar el identificador de recurso de subred:
   1. Vaya al grupo de recursos en [Azure Portal](https://portal.azure.com). 
   1. Seleccione el recurso de red virtual. 
   1. Seleccione **Propiedades** en el panel **Configuración**. 
   1. Encuentre el identificador de recurso de la red virtual y anéxele `/subnets/<subnetname>` al final para crear el identificador de recurso de subred. Por ejemplo:
      - Su identificador de recurso de red virtual es `/subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet`
      - Su nombre de subred es `default`
      - Por lo tanto, su identificador de recurso de subred es `/subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default`


Con el siguiente fragmento de código se creará el cliente de escucha de grupo de disponibilidad:

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interactive
# Create the availability group listener
# example: az sql vm group ag-listener create -n AGListener -g SQLVM-RG `
#  --ag-name SQLAG --group-name Cluster --ip-address 10.0.0.27 `
#  --load-balancer sqlilb --probe-port 59999  `
#  --subnet /subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default `
#  --sqlvms sqlvm1 sqlvm2

az sql vm group ag-listener create -n <listener name> -g <resource group name> `
  --ag-name <availability group name> --group-name <cluster name> --ip-address <ag listener IP address> `
  --load-balancer <lbname> --probe-port <Load Balancer probe port, default 59999>  `
  --subnet <subnet resource id> `
  --sqlvms <names of SQL VM's hosting AG replicas, ex: sqlvm1 sqlvm2>
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell-interactive

# example: New-AzAvailabilityGroupListener -Name AGListener -ResourceGroupName SQLVM-RG `
#    -AvailabilityGroupName SQLAG  -GroupName Cluster `
#    -IpAddress 10.0.0.27 -LoadBalancerResourceId sqlilb `
#    -ProbePort 59999 `
#    -SubnetId /subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default `
#    -SqlVirtualMachineId sqlvm1 sqlvm2


New-AzAvailabilityGroupListener -Name <listener name> -ResourceGroupName <resource group name> `
   -AvailabilityGroupName <availability group name> -GroupName <cluster name> `
   -IpAddress <ag listener IP address> -LoadBalancerResourceId <lbid> `
   -ProbePort <Load Balancer probe port, default 59999> `
   -SubnetId <subnet resource id> `
   -SqlVirtualMachineId <names of SQL VM's hosting AG replicas>
```

---

## <a name="modify-number-of-replicas"></a>Modificación del número de réplicas 
Hay una capa de complejidad agregada al implementar un grupo de disponibilidad en máquinas virtuales con SQL Server hospedadas en Azure. El proveedor de recursos y el grupo de máquinas virtuales ahora administran los recursos. En este sentido, al agregar o quitar réplicas en el grupo de disponibilidad, hay un paso más que consiste en actualizar los metadatos del cliente de escucha con información relativa a las máquinas virtuales con SQL Server. Cuando se modifica el número de réplicas del grupo de disponibilidad, hay que usar también el comando [az sql vm group ag-listener update](/cli/azure/sql/vm/group/ag-listener#az_sql_vm_group_ag_listener_update) para actualizar el cliente de escucha con los metadatos de las máquinas virtuales con SQL Server. 


### <a name="add-a-replica"></a>Adición de una réplica

Para agregar una réplica nueva al grupo de disponibilidad:

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Agregue la máquina virtual de SQL Server al grupo de clústeres:
   ```azurecli-interactive

   # Add the SQL Server VM to the cluster group
   # example: az sql vm add-to-group -n SQLVM3 -g SQLVM-RG --sqlvm-group Cluster `
   # -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

   az sql vm add-to-group -n <VM3 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
   -b <bootstrap account password> -p <operator account password> -s <service account password>
   ```

1. Use SQL Server Management Studio para agregar la instancia de SQL Server como una réplica dentro del grupo de disponibilidad.
1. Agregue a la escucha los metadatos de la máquina virtual de SQL Server:

   ```azurecli-interactive
   # Update the listener metadata with the new VM
   # example: az sql vm group ag-listener update -n AGListener `
   # -g sqlvm-rg --group-name Cluster --sqlvms sqlvm1 sqlvm2 sqlvm3

   az sql vm group ag-listener update -n <Listener> `
   -g <RG name> --group-name <cluster name> --sqlvms <SQL VMs, along with new SQL VM>
   ```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Agregue la máquina virtual de SQL Server al grupo de clústeres:

   ```powershell-interactive
   # Add the SQL Server VM to the cluster group
   # example: $sqlvm3 = Get-AzSqlVM -Name SQLVM3 -ResourceGroupName SQLVM-RG 
   # $group = Get-AzSqlVMGroup -ResourceGroupName SQLVM-RG -Name Cluster
   # $sqlvmconfig3 = Set-AzSqlVMConfigGroup -SqlVM $sqlvm3 -SqlVMGroup $group `
   #  -ClusterOperatorAccountPassword Str0ngAzur3P@ssword! `
   #  -SqlServiceAccountPassword Str0ngAzur3P@ssword! `
   #  -ClusterBootstrapAccountPassword Str0ngAzur3P@ssword!

   $sqlvm3 = Get-AzSqlVM -Name <VM1 Name> -ResourceGroupName <Resource Group Name>
   $group = Get-AzSqlVMGroup  -ResourceGroupName <resource group name> -Name <name>
   $sqlvmconfig3 = Set-AzSqlVMConfigGroup -SqlVM $sqlvm3 -SqlVMGroup $group `
   -ClusterOperatorAccountPassword <operator account password> `
   -SqlServiceAccountPassword <service account password> `
   -ClusterBootstrapAccountPassword <bootstrap account password>

   Update-AzSqlVM -ResourceId $sqlvm3.ResourceId -SqlVM $sqlvmconfig3
   ```

1. Use SQL Server Management Studio para agregar la instancia de SQL Server como una réplica dentro del grupo de disponibilidad.
1. Agregue a la escucha los metadatos de la máquina virtual de SQL Server:

   ```powershell-interactive
   # Update the listener metadata with the new VM
   # example: Update-AzAvailabilityGroupListener -Name AGListener -ResourceGroupName SQLVM-RG `
   #   -SqlVMGroupName Cluster -SqlVirtualMachineId SQLVM3

   Update-AzAvailabilityGroupListener -Name <Listener> -ResourceGroupName <Resource Group Name> `
      -SqlVMGroupName <cluster name> -SqlVirtualMachineId <SQL VMs, along with new SQL VM> 

   ```

---

### <a name="remove-a-replica"></a>Eliminación de una réplica

Para quitar una réplica del grupo de disponibilidad:

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Quite la réplica del grupo de disponibilidad mediante SQL Server Management Studio. 
1. Quite los metadatos de máquina virtual de SQL Server de la escucha:
   ```azurecli-interactive
   # Update the listener metadata by removing the VM from the SQLVMs list
   # example: az sql vm group ag-listener update -n AGListener `
   # -g sqlvm-rg --group-name Cluster --sqlvms sqlvm1 sqlvm2

   az sql vm group ag-listener update -n <Listener> `
   -g <RG name> --group-name <cluster name> --sqlvms <SQL VMs that remain>
   ```
1. Quite la máquina virtual de SQL Server del clúster:
   ```azurecli-interactive
   # Remove the SQL VM from the cluster
   # example: az sql vm remove-from-group --name SQLVM3 --resource-group SQLVM-RG

   az sql vm remove-from-group --name <SQL VM name> --resource-group <RG name> 
   ```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Quite la réplica del grupo de disponibilidad mediante SQL Server Management Studio. 
1. Quite los metadatos de máquina virtual de SQL Server de la escucha:

   ```powershell-interactive
   # Update the listener metadata by removing the VM from the SQLVMs list
   # example: Update-AzAvailabilityGroupListener -Name AGListener -ResourceGroupName SQLVM-RG `
   #  -SqlVMGroupName Cluster -SqlVirtualMachineId SQLVM3

   Update-AzAvailabilityGroupListener -Name <Listener> -ResourceGroupName <Resource Group Name> `
      -SqlVMGroupName <cluster name> -SqlVirtualMachineId <SQL VMs that remain>

   ```
1. Quite la máquina virtual de SQL Server del clúster:

   ```powershell-interactive
   # Remove the SQL VM from the cluster
   # example: $sqlvm = Get-AzSqlVM -Name SQLVM3 -ResourceGroupName SQLVM-RG
   #  $sqlvm. SqlVirtualMachineGroup = ""
   #  Update-AzSqlVM -ResourceId $sqlvm -SqlVM $sqlvm

   $sqlvm = Get-AzSqlVM -Name <VM Name> -ResourceGroupName <Resource Group Name>
      $sqlvm. SqlVirtualMachineGroup = ""
    
    Update-AzSqlVM -ResourceId $sqlvm -SqlVM $sqlvm
   ```

---

## <a name="remove-listener"></a>Quitar el agente de escucha
Si posteriormente necesita quitar el cliente de escucha de grupo de disponibilidad configurado con la CLI de Azure, deberá pasar por la extensión Agente de IaaS de SQL. Puesto que el cliente de escucha se registra mediante la extensión Agente de IaaS de SQL, no basta con eliminarlo mediante SQL Server Management Studio. 

El mejor método consiste en eliminarlo a través de la extensión Agente de IaaS de SQL mediante el siguiente fragmento de código en la CLI de Azure. Al hacerlo, se quitan los metadatos del cliente de escucha de grupo de disponibilidad de la extensión Agente de IaaS de SQL. También elimina físicamente el cliente de escucha de grupo de disponibilidad. 

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli-interactive
# Remove the availability group listener
# example: az sql vm group ag-listener delete --group-name Cluster --name AGListener --resource-group SQLVM-RG

az sql vm group ag-listener delete --group-name <cluster name> --name <listener name > --resource-group <resource group name>
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell-interactive
# Remove the availability group listener
# example: Remove-AzAvailabilityGroupListener -Name AGListener `
#   -ResourceGroupName SQLVM-RG -SqlVMGroupName Cluster

Remove-AzAvailabilityGroupListener -Name <Listener> `
   -ResourceGroupName <Resource Group Name> -SqlVMGroupName <cluster name>
```

---

## <a name="remove-cluster"></a>Eliminación del clúster

Quite todos los nodos del clúster para destruirlo y luego quite los metadatos del clúster de la extensión Agente de IaaS de SQL. Para ello, puede usar la CLI de Azure o PowerShell. 


# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

En primer lugar, quite todas las VM con SQL Server del clúster: 

```azurecli-interactive
# Remove the VM from the cluster metadata
# example: az sql vm remove-from-group --name SQLVM2 --resource-group SQLVM-RG

az sql vm remove-from-group --name <VM1 name>  --resource-group <resource group name>
az sql vm remove-from-group --name <VM2 name>  --resource-group <resource group name>
```

Si estas son las únicas máquinas virtuales del clúster, este se destruye. Si hay otras máquinas virtuales en el clúster aparte de las VM con SQL Server que se han quitado, las otras no se quitan y el clúster no se destruye. 

Luego, quite los metadatos del clúster de la extensión Agente de IaaS de SQL: 

```azurecli-interactive
# Remove the cluster from the SQL VM RP metadata
# example: az sql vm group delete --name Cluster --resource-group SQLVM-RG

az sql vm group delete --name <cluster name> Cluster --resource-group <resource group name>
```



# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

En primer lugar, quite todas las VM con SQL Server del clúster. Esto quita físicamente los nodos del clúster y lo destruye: 

```powershell-interactive
# Remove the SQL VM from the cluster
# example: $sqlvm = Get-AzSqlVM -Name SQLVM3 -ResourceGroupName SQLVM-RG
#  $sqlvm. SqlVirtualMachineGroup = ""
#  Update-AzSqlVM -ResourceId $sqlvm -SqlVM $sqlvm

$sqlvm = Get-AzSqlVM -Name <VM Name> -ResourceGroupName <Resource Group Name>
   $sqlvm. SqlVirtualMachineGroup = ""
   
   Update-AzSqlVM -ResourceId $sqlvm -SqlVM $sqlvm
```

Si estas son las únicas máquinas virtuales del clúster, este se destruye. Si hay otras máquinas virtuales en el clúster aparte de las VM con SQL Server que se han quitado, las otras no se quitan y el clúster no se destruye. 

Luego, quite los metadatos del clúster de la extensión Agente de IaaS de SQL: 

```powershell-interactive
# Remove the cluster metadata
# example: Remove-AzSqlVMGroup -ResourceGroupName "SQLVM-RG" -Name "Cluster"

Remove-AzSqlVMGroup -ResourceGroupName "<resource group name>" -Name "<cluster name> "
```

---

## <a name="next-steps"></a>Pasos siguientes

Una vez que se haya implementado el grupo de disponibilidad, puede optimizar la [configuración de alta disponibilidad y recuperación ante desastres para SQL Server en máquinas virtuales de Azure](hadr-cluster-best-practices.md). 


Para obtener más información, consulte:

- [Clúster de conmutación por error de Windows Server con SQL Server en máquinas virtuales de Azure](hadr-windows-server-failover-cluster-overview.md)
- [Grupos de disponibilidad Always On para SQL Server en Azure Virtual Machines](availability-group-overview.md)
- [Introducción a los grupos de disponibilidad Always On](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)
