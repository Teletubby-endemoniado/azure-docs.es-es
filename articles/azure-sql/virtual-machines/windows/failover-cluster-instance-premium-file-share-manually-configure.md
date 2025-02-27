---
title: Creación de una FCI con un recurso compartido de archivos Premium
description: Use un recurso compartido de archivos Premium (PFS) para crear una instancia de clúster de conmutación por error (FCI) con SQL Server en Azure Virtual Machines.
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
ms.date: 11/10/2021
ms.author: rsetlem
ms.reviewer: mathoma
ms.openlocfilehash: 8d9e94ae12600223c6f54fb75cbc6c9648ecf7f9
ms.sourcegitcommit: 512e6048e9c5a8c9648be6cffe1f3482d6895f24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2021
ms.locfileid: "132156958"
---
# <a name="create-an-fci-with-a-premium-file-share-sql-server-on-azure-vms"></a>Creación de una FCI con un recurso compartido de archivos Premium (SQL Server en VM de Azure)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!TIP]
> Elimine la necesidad de una instancia de Azure Load Balancer o un nombre de red distribuida (DNN) para la instancia de clúster de conmutación por error mediante la creación de las máquinas virtuales de SQL Server en [varias subredes](failover-cluster-instance-prepare-vm.md#subnets) dentro de la misma red virtual de Azure.


En este artículo se explica cómo crear una instancia de clúster de conmutación por error (FCI) con SQL Server en Azure Virtual Machines (VM) con un [recurso compartido de archivos Premium](../../../storage/files/storage-how-to-create-file-share.md).

Los recursos compartidos de archivos Premium son recursos compartidos de archivos de baja latencia constante con respaldo de SSD que son totalmente compatibles para su uso con la instancia del clúster de conmutación por error en SQL Server 2012 y versiones posteriores en Windows Server 2012 y versiones posteriores. Los recursos compartidos de archivos Premium ofrecen mayor flexibilidad, lo que le permite cambiar el tamaño y escalar el recurso compartido de archivos sin tiempo de inactividad.

Para obtener más información, consulte información general de [FCI con SQL Server en VM de Azure](failover-cluster-instance-overview.md) y [procedimientos recomendados del clúster](hadr-cluster-best-practices.md). 

> [!NOTE]
> Ahora es posible migrar mediante lift and shift la solución de instancia de clúster de conmutación por error a SQL Server en máquinas virtuales de Azure mediante Azure Migrate. Consulte [Migración de una instancia de clúster de conmutación por error](../../migration-guides/virtual-machines/sql-server-failover-cluster-instance-to-sql-on-azure-vm.md) para más información. 

## <a name="prerequisites"></a>Requisitos previos

Antes de completar las instrucciones de este artículo, ya debe tener:

- Suscripción a Azure.
- Una cuenta que tenga permisos para crear objetos en máquinas virtuales de Azure y en Active Directory.
- [Dos o más instancias de Microsoft Azure Virtual Machines preparadas](failover-cluster-instance-prepare-vm.md) en un [conjunto de disponibilidad](../../../virtual-machines/windows/tutorial-availability-sets.md#create-an-availability-set) o [zonas de disponibilidad](../../../virtual-machines/windows/create-portal-availability-zone.md#confirm-zone-for-managed-disk-and-ip-address) diferentes.
- Un [recurso compartido de archivos premium](../../../storage/files/storage-how-to-create-file-share.md) que se usará como unidad en clúster, en función de la cuota de almacenamiento de la base de datos de los archivos de datos.
- La versión más reciente de [PowerShell](/powershell/azure/install-az-ps). 

## <a name="mount-premium-file-share"></a>Montaje del recurso compartido de archivos Premium

Para montar el recurso compartido de archivos Premium, realice estos pasos: 

1. Inicie sesión en [Azure Portal](https://portal.azure.com). y vaya a la cuenta de almacenamiento.
1. Vaya a **Recursos compartidos de archivos** en **Almacenamiento de datos** y, luego, seleccione el recurso compartido de archivos prémium que quiere usar para el almacenamiento SQL.
1. Seleccione **Conectar** para mostrar la cadena de conexión del recurso compartido de archivos.
1. En la lista desplegable, seleccione la letra de unidad que quiere usar, elija **Clave de cuenta de almacenamiento** como método de autenticación y, luego, copie el bloque de código en un editor de texto, como el Bloc de notas.

   :::image type="content" source="media/failover-cluster-instance-premium-file-share-manually-configure/premium-file-storage-commands.png" alt-text="Copia del comando de PowerShell desde el portal de conexión del recurso compartido de archivos":::

1. Aplique el protocolo de escritorio remoto (RDP) para conectarse a la VM con SQL Server con la **cuenta que la FCI de SQL Server usará para la cuenta de servicio**.
1. Abra una consola de comandos de PowerShell de administración.
1. Ejecute el comando que copió anteriormente en el editor de texto desde el portal de recursos compartidos de archivos.
1. Vaya al recurso compartido mediante el Explorador de archivos o el cuadro de diálogo **Ejecutar** (Windows + R en el teclado). Use la ruta de acceso a la red `\\storageaccountname.file.core.windows.net\filesharename`. Por ejemplo: `\\sqlvmstorageaccount.file.core.windows.net\sqlpremiumfileshare`
1. Cree al menos una carpeta en el recurso compartido de archivos recién conectado para colocar los archivos de datos de SQL.
1. Repita estos pasos en cada VM con SQL Server que participará en el clúster.

  > [!IMPORTANT]
  > Considere la posibilidad de usar un recurso compartido de archivos independiente para los archivos de copia de seguridad a fin de ahorrar la capacidad de operaciones de entrada/salida por segundo (IOPS) y espacio de este recurso compartido para usarla para el archivo de registro y de datos. Puede usar un recurso compartido de archivos Premium o Estándar para los archivos de copia de seguridad.

## <a name="create-windows-failover-cluster"></a>Creación de un clúster de conmutación por error de Windows

Los pasos necesarios para crear un clúster de conmutación por error de Windows Server varían en función de si se han implementado las máquinas virtuales con SQL Server en una sola subred o en varias. Para crear el clúster, siga los pasos del tutorial de un [escenario de varias subredes](availability-group-manually-configure-tutorial-multi-subnet.md#add-failover-cluster-feature) o un [escenario de una sola subred](availability-group-manually-configure-tutorial-single-subnet.md#create-the-cluster). Aunque estos tutoriales son para crear un grupo de disponibilidad, los pasos para crear el clúster son los mismos. 


## <a name="configure-quorum"></a>Configuración de un cuórum

El testigo en la nube es la solución de cuórum recomendada para este tipo de configuración de clúster con SQL Server en máquinas virtuales de Azure.  

Si tiene un número par de votos en el clúster, configure la [solución de cuórum](hadr-cluster-quorum-configure-how-to.md) que mejor se adapte a sus necesidades empresariales. Para más información, consulte [Cuórum con VM SQL Server](hadr-windows-server-failover-cluster-overview.md#quorum). 

## <a name="validate-cluster"></a>Validar el clúster

Valide el clúster en una de las máquinas virtuales mediante la interfaz de usuario del administrador de clústeres de conmutación por error o PowerShell.

Para validar el clúster con la interfaz de usuario, realice los pasos siguientes en una de las máquinas virtuales:

1. En **Administrador del servidor**, seleccione **Herramientas** y, después, seleccione **Administrador de clústeres de conmutación por error**.
1. En **Administrador de clústeres de conmutación por error**, seleccione **Acción** y, a continuación, seleccione **Validar configuración**.
1. Seleccione **Next** (Siguiente).
1. En **Seleccionar servidores o un clúster**, escriba el nombre de ambas máquinas virtuales.
1. En **Opciones de pruebas**, seleccione **Ejecutar solo las pruebas que seleccione**. 
1. Seleccione **Next** (Siguiente).
1. En **Selección de pruebas**, seleccione todas las pruebas excepto **Almacenamiento** y **Espacios de almacenamiento directo** como se muestra aquí:

   :::image type="content" source="media/failover-cluster-instance-premium-file-share-manually-configure/cluster-validation.png" alt-text="Seleccionar pruebas de validación de clústeres":::

1. Seleccione **Next** (Siguiente).
1. En **Confirmación**, seleccione **Siguiente**. El Asistente para **validar una configuración** ejecuta las pruebas de validación.


Para validar el clúster con PowerShell, ejecute el siguiente script en una sesión de PowerShell de administrador de una de las máquinas virtuales:

```powershell
Test-Cluster –Node ("<node1>","<node2>") –Include "Inventory", "Network", "System Configuration"
```



## <a name="test-cluster-failover"></a>Conmutación por error del clúster de prueba

Pruebe la conmutación por error del clúster. En **Administrador de clústeres de conmutación por error**, haga clic con el botón derecho en el clúster y seleccione **Más acciones** > **Mover principales recursos de clúster** > **Seleccionar nodo** y, después, seleccione el otro nodo del clúster. Mueva el recurso de clúster principal a cada nodo del clúster y, después, devuélvalo al nodo principal. Si puede mover correctamente el clúster a cada nodo, está listo para instalar SQL Server.  

:::image type="content" source="media/failover-cluster-instance-premium-file-share-manually-configure/test-cluster-failover.png" alt-text="Prueba de la conmutación por error del clúster moviendo el recurso principal a los demás nodos":::


## <a name="create-sql-server-fci"></a>Crear la FCI de SQL Server

Después de haber configurado el clúster de conmutación por error, puede crear la FCI de SQL Server.

1. Conéctese a la primera máquina virtual con RDP.

1. En **Administrador de clústeres de conmutación por error**, asegúrese de que todos los recursos principales de clúster estén en la primera máquina virtual. Si es necesario, mueva todos los recursos a esta máquina virtual.

1. Localice los medios de instalación. Si la máquina virtual usa una de las imágenes de Azure Marketplace, los medios se encuentran en `C:\SQLServer_<version number>_Full`. 

1. Seleccione **Setup** (Configuración).

1. En el **Centro de instalación de SQL Server**, seleccione **Instalación**.

1. Seleccione **Nueva instalación de clúster de conmutación por error de SQL Server** y, a continuación, siga las instrucciones del asistente para instalar la FCI de SQL Server.

1. En la página **Configuración de red de clúster**, la IP que proporcione varía en función de si las máquinas virtuales con SQL Server se implementaron en una sola subred o en varias. 

   1. En el caso de un **entorno de una sola subred**, especifique la dirección IP que planea agregar a [Azure Load Balancer](failover-cluster-instance-vnn-azure-load-balancer-configure.md)
   1. En el caso de un **entorno de varias subredes**, especifique la dirección IP secundaria en la subred de la _primera_ máquina virtual con SQL Server que designó anteriormente como la [dirección IP del nombre de red de la instancia de clúster de conmutación por error](failover-cluster-instance-prepare-vm.md#assign-secondary-ip-addresses):

   :::image type="content" source="./media/failover-cluster-instance-azure-shared-disk-manually-configure/sql-install-cluster-network-secondary-ip-vm-1.png" alt-text="especifique la dirección IP secundaria en la subred de la primera máquina virtual con SQL Server que designó anteriormente como la dirección IP del nombre de red de la instancia de clúster de conmutación por error":::

1. En **Configuración del motor de base de datos**, los directorios de datos deben estar en el recurso compartido de archivos Premium. Escriba la ruta de acceso completa del recurso compartido,con este formato: `\\storageaccountname.file.core.windows.net\filesharename\foldername`. Aparece una advertencia que le notifica que ha especificado un servidor de archivos como directorio de datos. Se espera esta advertencia. Para evitar posibles errores, asegúrese de que la cuenta de usuario que usó para tener acceso a la máquina virtual mediante RDP cuando conservó el recurso compartido de archivos sea la misma que usa el servicio SQL Server.

   :::image type="content" source="media/failover-cluster-instance-premium-file-share-manually-configure/use-file-share-as-data-directories.png" alt-text="Uso del recurso compartido de archivos como directorios de datos SQL":::

1. Después de completar los pasos del asistente, el programa de instalación instalará una FCI de SQL Server en el primer nodo.

1. Tras completar correctamente la instalación de instancia de clúster de conmutación por error en el primer nodo, conéctese al segundo nodo mediante RDP.

1. Abra el **Centro de instalación de SQL Server** y, a continuación, seleccione **Instalación**.

1. Seleccione **Agregar nodo a clúster de conmutación por error de SQL Server**. Siga las instrucciones del asistente para instalar SQL Server y agregar el nodo a la instancia de clúster de conmutación por error.

1. En el caso de un escenario de varias subredes, en **Configuración de red en clúster**, especifique la dirección IP secundaria en la subred de la _segunda_ máquina virtual con SQL Server que designó anteriormente como la [dirección IP del nombre de red de la instancia de clúster de conmutación por error](failover-cluster-instance-prepare-vm.md#assign-secondary-ip-addresses)

    :::image type="content" source="./media/failover-cluster-instance-azure-shared-disk-manually-configure/sql-install-cluster-network-secondary-ip-vm-2.png" alt-text="escriba la dirección IP secundaria en la subred de la segunda subred de máquina virtual con SQL Server que designó anteriormente como la dirección IP del nombre de red de la instancia de clúster de conmutación por error":::

    Después de seleccionar **Siguiente** en **Configuración de red de clúster**, el programa de instalación muestra un cuadro de diálogo que indica que el programa de instalación de SQL Server detectó varias subredes como en la imagen de ejemplo.  Seleccione **Sí** para confirmar la acción. 

    :::image type="content" source="./media/failover-cluster-instance-azure-shared-disk-manually-configure/sql-install-multi-subnet-confirmation.png" alt-text="Confirmación de varias subredes":::
   

1. Después de completar las instrucciones del asistente, el programa de instalación agrega el segundo nodo FCI de SQL Server. 

1. Repita estos pasos en cualquier otro nodo que desee agregar a la instancia de clúster de conmutación por error de SQL Server. 


>[!NOTE]
> Las imágenes de la galería de Azure Marketplace vienen con SQL Server Management Studio instalado. Si no ha usado una imagen de Marketplace, [descargue SQL Server Management Studio (SSMS)](/sql/ssms/ownload-sql-server-management-studio-ssms).


## <a name="register-with-sql-iaas-extension"></a>Registro mediante la extensión de IaaS de SQL 

Para administrar la VM con SQL Server desde el portal, regístrela con la extensión Agente de IaaS de SQL en [modo de administración ligera](sql-agent-extension-manually-register-single-vm.md#lightweight-mode); actualmente, es el único modo que se admite con FCI y SQL Server en las VM de Azure. 

Registre una máquina virtual con SQL Server en modo ligero con PowerShell ((-LicenseType puede ser `PAYG` o `AHUB`):

```powershell-interactive
# Get the existing compute VM
$vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
         
# Register SQL VM with 'Lightweight' SQL IaaS agent
New-AzSqlVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
   -LicenseType ???? -SqlManagementType LightWeight  
```

## <a name="configure-connectivity"></a>Configuración de la conectividad

Si implementó las máquinas virtuales de SQL Server en varias subredes, omita este paso. Si implementó las máquinas virtuales con SQL Server en una sola subred, deberá configurar un componente adicional para enrutar el tráfico a la instancia de clúster de conmutación por error. Puede configurar un nombre de red virtual (VNN) con Azure Load Balancer o un nombre de red distribuida para una instancia de clúster de conmutación por error. [Revise las diferencias entre los dos](hadr-windows-server-failover-cluster-overview.md#virtual-network-name-vnn) y, luego, implemente un [nombre de red distribuida](failover-cluster-instance-distributed-network-name-dnn-configure.md) o un [nombre de red virtual y Azure Load Balancer](failover-cluster-instance-vnn-azure-load-balancer-configure.md) para la instancia de clúster de conmutación por error.  

## <a name="limitations"></a>Limitaciones

- No se admite el Coordinador de transacciones distribuidas de Microsoft (MSDTC) en Windows Server 2016 y versiones anteriores. 
- La secuencia de archivos no se admite en los clústeres de conmutación por error con un recurso compartido de archivos Premium. Para usar la secuencia de archivos, implemente el clúster con [Espacios de almacenamiento directo](failover-cluster-instance-storage-spaces-direct-manually-configure.md) o [discos compartidos de Azure](failover-cluster-instance-azure-shared-disks-manually-configure.md) en su lugar.
- Solo se admite el registro con la extensión Agente de IaaS de SQL en [modo de administración ligero](sql-server-iaas-agent-extension-automate-management.md#management-modes). 
- Las instantáneas de base de datos no se admiten actualmente en [Azure Files debido a las limitaciones de archivos dispersos](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
- Puesto que no se admiten instantáneas de base de datos, CHECKDB para bases de datos de usuario vuelve a CHECKDB WITH TABLOCK. TABLOCK limita las comprobaciones que se llevan a cabo; DBCC CHECKCATALOG no se ejecuta en la base de datos y los datos de Service Broker no se validan.
- No se admite CHECKDB en la base de datos MASTER y MSDB. 
- Las bases de datos que usan la característica OLTP en memoria no se admiten en una instancia de clúster de conmutación por error implementada con un recurso compartido de archivos prémium. Si su empresa requiere OLTP en memoria, considere la posibilidad de implementar la instancia de clúster de conmutación por error con [discos compartidos de Azure](failover-cluster-instance-azure-shared-disks-manually-configure.md) o [Espacios de almacenamiento directo](failover-cluster-instance-storage-spaces-direct-manually-configure.md).

## <a name="next-steps"></a>Pasos siguientes

Si los recursos compartidos de archivos Premium no son la solución de almacenamiento de la FCI adecuada para usted, considere la posibilidad de crear la FCI mediante [discos compartidos de Azure](failover-cluster-instance-azure-shared-disks-manually-configure.md) o [Espacios de almacenamiento directo](failover-cluster-instance-storage-spaces-direct-manually-configure.md) en su lugar. 

Para obtener más información, consulte:

- [Clúster de conmutación por error de Windows Server con SQL Server en máquinas virtuales de Azure](hadr-windows-server-failover-cluster-overview.md)
- [Instancias de clúster de conmutación por error con SQL Server en Azure Virtual Machines](failover-cluster-instance-overview.md)
- [Información general de las instancias de clúster de conmutación por error](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
- [Configuración de alta disponibilidad y recuperación ante desastres para SQL Server en máquinas virtuales de Azure](hadr-cluster-best-practices.md)

