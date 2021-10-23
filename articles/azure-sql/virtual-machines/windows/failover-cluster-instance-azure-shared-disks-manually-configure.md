---
title: Creación de una FCI con discos compartidos de Azure
description: Use discos compartidos de Azure para crear una instancia de clúster de conmutación por error (FCI) con SQL Server en Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
author: rajeshsetlem
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.subservice: hadr
ms.custom: na, devx-track-azurepowershell
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/26/2020
ms.author: rsetlem
ms.reviewer: mathoma
ms.openlocfilehash: 2964c068c9d60e9922b6544ca29dce20b2ac3dc2
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130167031"
---
# <a name="create-an-fci-with-azure-shared-disks-sql-server-on-azure-vms"></a>Creación de una FCI con discos compartidos de Azure (SQL Server en VM de Azure)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

En este artículo se explica cómo crear una instancia de clúster de conmutación por error (FCI) mediante discos compartidos de Azure con SQL Server en Azure Virtual Machines (VM). 

Para más información, consulte la información general de [FCI con SQL Server en VM de Azure](failover-cluster-instance-overview.md) y [Procedimientos recomendados de clúster](hadr-cluster-best-practices.md). 

> [!NOTE]
> Ahora es posible migrar mediante lift and shift la solución de instancia de clúster de conmutación por error a SQL Server en máquinas virtuales de Azure mediante Azure Migrate. Consulte [Migración de una instancia de clúster de conmutación por error](../../migration-guides/virtual-machines/sql-server-failover-cluster-instance-to-sql-on-azure-vm.md) para más información. 

## <a name="prerequisites"></a>Requisitos previos 

Antes de completar las instrucciones de este artículo, ya debe tener:

- Suscripción a Azure. Comience a usarla [gratis](https://azure.microsoft.com/free/). 
- [Dos o más máquinas virtuales de Azure con Windows](failover-cluster-instance-prepare-vm.md). Ultra Disks admite [conjuntos de disponibilidad](../../../virtual-machines/windows/tutorial-availability-sets.md) y [grupos con ubicación por proximidad](../../../virtual-machines/co-location.md#proximity-placement-groups) (PPG) compatibles con SSD Premium y [zonas de disponibilidad](../../../virtual-machines/windows/create-portal-availability-zone.md#confirm-zone-for-managed-disk-and-ip-address). Todos los nodos deben existir en el mismo [grupo con ubicación por proximidad](../../../virtual-machines/co-location.md#proximity-placement-groups).
- Una cuenta que tenga permisos para crear objetos en máquinas virtuales de Azure y en Active Directory.
- La versión más reciente de [PowerShell](/powershell/azure/install-az-ps). 

## <a name="add-azure-shared-disk"></a>Adición de un disco compartido de Azure
Implemente un disco SSD Premium administrado con la característica de disco compartido habilitada. Establezca `maxShares` para que se **alinee con el número de nodos de clúster** a fin de que el disco pueda compartirse en todos los nodos de FCI. 

Agregue un disco compartido de Azure mediante el procedimiento siguiente: 

1. Guarde el siguiente script como *SharedDiskConfig.json*: 

   ```JSON
   { 
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "dataDiskName": {
         "type": "string",
         "defaultValue": "mySharedDisk"
       },
       "dataDiskSizeGB": {
         "type": "int",
         "defaultValue": 1024
       },
       "maxShares": {
         "type": "int",
         "defaultValue": 2
       }
     },
     "resources": [
       {
         "type": "Microsoft.Compute/disks",
         "name": "[parameters('dataDiskName')]",
         "location": "[resourceGroup().location]",
         "apiVersion": "2019-07-01",
         "sku": {
           "name": "Premium_LRS"
         },
         "properties": {
           "creationData": {
             "createOption": "Empty"
           },
           "diskSizeGB": "[parameters('dataDiskSizeGB')]",
           "maxShares": "[parameters('maxShares')]"
         }
       }
     ] 
   }
   ```

2. Ejecute *SharedDiskConfig.json* mediante PowerShell: 

   ```powershell
   $rgName = < specify your resource group name>
       $location = 'westcentralus'
       New-AzResourceGroupDeployment -ResourceGroupName $rgName `
   -TemplateFile "SharedDiskConfig.json"
   ```

3. Para cada VM, inicialice los discos compartidos adjuntos como tabla de particiones GUID (GPT) y formatéelos como un nuevo sistema de archivos de tecnología (NTFS). Para ello, ejecute este comando: 

    ```powershell
    $resourceGroup = "<your resource group name>"
    $location = "<region of your shared disk>"
    $ppgName = "<your proximity placement groups name>"
    $vm = Get-AzVM -ResourceGroupName "<your resource group name>" `
        -Name "<your VM node name>"
    $dataDisk = Get-AzDisk -ResourceGroupName $resourceGroup `
        -DiskName "<your shared disk name>"
    $vm = Add-AzVMDataDisk -VM $vm -Name "<your shared disk name>" `
        -CreateOption Attach -ManagedDiskId $dataDisk.Id `
        -Lun <available LUN - check disk setting of the VM>
    Update-AzVm -VM $vm -ResourceGroupName $resourceGroup
    ```

## <a name="create-failover-cluster"></a>Creación de un clúster de conmutación por error

Para crear el clúster de conmutación por error, necesita:

- Los nombres de las máquinas virtuales que se convertirán en nodos del clúster.
- Un nombre para el clúster de conmutación por error.
- Una dirección IP para el clúster de conmutación por error. Puede usar una dirección IP que no se utilice en la misma red virtual de Azure y subred como nodos del clúster.

# <a name="windows-server-2012-2016"></a>[Windows Server 2012-2016](#tab/windows2012)

El siguiente script de PowerShell crea un clúster de conmutación por error. Actualice el script con los nombres de los nodos (los nombres de las máquinas virtuales) y una dirección IP disponible desde la red virtual de Azure.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") -StaticAddress <n.n.n.n> -NoStorage
```   

# <a name="windows-server-2019"></a>[Windows Server 2019](#tab/windows2019)

El siguiente script de PowerShell crea un clúster de conmutación por error. Actualice el script con los nombres de los nodos (los nombres de las máquinas virtuales) y una dirección IP disponible desde la red virtual de Azure.

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") -StaticAddress <n.n.n.n> -NoStorage -ManagementPointNetworkType Singleton 
```

Para más información, consulte [Clúster de conmutación por error: objeto de red en clúster](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97).

---

## <a name="configure-quorum"></a>Configuración de un cuórum

Dado que el testigo de disco es la opción de cuórum más resistente y la solución FCI usa discos compartidos de Azure, se recomienda configurar un testigo de disco como solución de cuórum. 

Si tiene un número par de votos en el clúster, configure la [solución de cuórum](hadr-cluster-quorum-configure-how-to.md) que mejor se adapte a sus necesidades empresariales. Para más información, consulte [Cuórum con VM SQL Server](hadr-windows-server-failover-cluster-overview.md#quorum). 

## <a name="validate-cluster"></a>Validar el clúster
Valide el clúster en la interfaz de usuario o con PowerShell.

Para validar el clúster con la interfaz de usuario, realice los pasos siguientes en una de las máquinas virtuales:

1. En **Administrador del servidor**, seleccione **Herramientas** y, después, seleccione **Administrador de clústeres de conmutación por error**.
1. En **Administrador de clústeres de conmutación por error**, seleccione **Acción** y, a continuación, seleccione **Validar configuración**.
1. Seleccione **Next** (Siguiente).
1. En **Seleccionar servidores o un clúster**, escriba el nombre de ambas máquinas virtuales.
1. En **Opciones de pruebas**, seleccione **Ejecutar solo las pruebas que seleccione**. 
1. Seleccione **Next** (Siguiente).
1. En **Selección de pruebas**, elija todas las pruebas, *excepto* **Almacenamiento**.

## <a name="test-cluster-failover"></a>Conmutación por error del clúster de prueba

Pruebe la conmutación por error del clúster. En **Administrador de clústeres de conmutación por error**, haga clic con el botón derecho en el clúster y seleccione **Más acciones** > **Mover principales recursos de clúster** > **Seleccionar nodo** y, después, seleccione el otro nodo del clúster. Mueva el recurso de clúster principal a cada nodo del clúster y, después, devuélvalo al nodo principal. Si puede mover correctamente el clúster a cada nodo, está listo para instalar SQL Server.  

:::image type="content" source="media/failover-cluster-instance-premium-file-share-manually-configure/test-cluster-failover.png" alt-text="Prueba de la conmutación por error del clúster moviendo el recurso principal a los demás nodos":::

## <a name="create-sql-server-fci"></a>Crear la FCI de SQL Server

Después de haber configurado el clúster de conmutación por error y todos los componentes del clúster, incluido el almacenamiento, puede crear la FCI de SQL Server.

1. Establezca la conexión a la primera máquina virtual mediante el Protocolo de escritorio remoto (RDP).

1. En **Administrador de clústeres de conmutación por error**, asegúrese de que todos los recursos principales de clúster estén en la primera máquina virtual. Si es necesario, mueva todos los recursos a esa máquina virtual.

1. Localice los medios de instalación. Si la máquina virtual usa una de las imágenes de Azure Marketplace, los medios se encuentran en `C:\SQLServer_<version number>_Full`. 

1. Seleccione **Setup** (Configuración).

1. En **Centro de instalación de SQL Server**, seleccione **Instalación**.

1. Seleccione **Nueva instalación de clúster de conmutación por error de SQL Server**. Siga las instrucciones del asistente para instalar la FCI de SQL Server.

Es preciso que los directorios de datos de FCI estén en los discos compartidos de Azure. 

1. Después de completar las instrucciones en el asistente, el programa de instalación instalará una FCI de SQL Server en el primer nodo.

1. Una vez que el programa de instalación instale la FCI en el primer nodo, conéctese al segundo nodo con RDP.

1. Abra el **Centro de instalación de SQL Server** y, a continuación, seleccione **Instalación**.

1. Seleccione **Agregar nodo a clúster de conmutación por error de SQL Server**. Siga las instrucciones del asistente para instalar el servidor de SQL Server y agregarlo a la FCI.

   >[!NOTE]
   >Si usó una imagen de la galería de Azure Marketplace con SQL Server, las herramientas de SQL Server estaban incluidas en la imagen. Si no usó alguna de estas imágenes, instale las herramientas de SQL Server por separado. Para más información, consulte [Descargar SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms).
   >

## <a name="register-with-the-sql-vm-rp"></a>Registro con el proveedor de recursos de VM de SQL

Para administrar la VM con SQL Server desde el portal, regístrela con la extensión Agente de IaaS de SQL en [modo de administración ligera](sql-agent-extension-manually-register-single-vm.md#lightweight-mode); actualmente, es el único modo que se admite con FCI y SQL Server en las VM de Azure. 

Registre una VM con SQL Server en modo ligero con PowerShell:  

```powershell-interactive
# Get the existing compute VM
$vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>

# Register SQL VM with 'Lightweight' SQL IaaS agent
New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
   -LicenseType PAYG -SqlManagementType LightWeight  
```

## <a name="configure-connectivity"></a>Configuración de la conectividad 

Puede configurar un nombre de red virtual o un nombre de red distribuida para una instancia de clúster de conmutación por error. [Revise las diferencias entre los dos](hadr-windows-server-failover-cluster-overview.md#virtual-network-name-vnn) y, a continuación, implemente un [nombre de red distribuida](failover-cluster-instance-distributed-network-name-dnn-configure.md) o un [nombre de red virtual](failover-cluster-instance-vnn-azure-load-balancer-configure.md) para la instancia de clúster de conmutación por error.  

## <a name="limitations"></a>Limitaciones

- Solo se admite el registro con la extensión Agente de IaaS de SQL en [modo de administración ligero](sql-server-iaas-agent-extension-automate-management.md#management-modes).

## <a name="next-steps"></a>Pasos siguientes

Si aún no lo ha hecho, configure la conectividad a su FCI con un [nombre de red virtual y un equilibrador de carga de Azure](failover-cluster-instance-vnn-azure-load-balancer-configure.md) o un [nombre de red distribuida (DNN)](failover-cluster-instance-distributed-network-name-dnn-configure.md). 

Si los discos compartidos de Azure no son la solución de almacenamiento de la FCI adecuada para usted, considere la posibilidad de crear la FCI mediante [recursos compartidos de archivos Premium](failover-cluster-instance-premium-file-share-manually-configure.md) o [Espacios de almacenamiento directo](failover-cluster-instance-storage-spaces-direct-manually-configure.md) en su lugar. 

Para más información, consulte la introducción a [FCI con SQL Server en VM de Azure](failover-cluster-instance-overview.md) y los [procedimientos recomendados de configuración de clúster](hadr-cluster-best-practices.md).


Para obtener más información, consulte:

- [Clúster de conmutación por error de Windows Server con SQL Server en máquinas virtuales de Azure](hadr-windows-server-failover-cluster-overview.md)
- [Instancias de clúster de conmutación por error con SQL Server en Azure Virtual Machines](failover-cluster-instance-overview.md)
- [Información general de las instancias de clúster de conmutación por error](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
- [Configuración de alta disponibilidad y recuperación ante desastres para SQL Server en máquinas virtuales de Azure](hadr-cluster-best-practices.md)
