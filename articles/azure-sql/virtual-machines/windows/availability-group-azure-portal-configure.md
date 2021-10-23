---
title: Configuración de un grupo de disponibilidad (Azure Portal)
description: Use Azure Portal para crear el clúster de conmutación por error de Windows, la escucha de grupo de disponibilidad y el equilibrador de carga interno en una VM con SQL Server en Azure.
services: virtual-machines-windows
documentationcenter: na
author: rajeshsetlem
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.subservice: hadr
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/20/2020
ms.author: rsetlem
ms.reviewer: mathoma
ms.custom: seo-lt-2019, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 36b2d461b73b778541b79142633a6aafca9509e0
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130162917"
---
# <a name="use-azure-portal-to-configure-an-availability-group-preview-for-sql-server-on-azure-vm"></a>Uso de Azure Portal para configurar un grupo de disponibilidad (versión preliminar) para SQL Server en máquinas virtuales de Azure 
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

En este artículo se explica cómo usar [Azure Portal](https://portal.azure.com) para configurar un grupo de disponibilidad para SQL Server en máquinas virtuales de Azure. 

Use Azure Portal para crear un nuevo clúster o incorporar un clúster existente y, luego, cree el grupo de disponibilidad, la escucha y el equilibrador de carga interno. 

Esta funcionalidad actualmente está en su versión preliminar. 

Si bien en este artículo se usa Azure Portal para configurar el entorno del grupo de disponibilidad, también es posible hacerlo mediante [PowerShell o la CLI de Azure](availability-group-az-commandline-configure.md), [plantillas de inicio rápido de Azure](availability-group-quickstart-template-configure.md) o [manualmente](availability-group-manually-configure-tutorial.md). 

> [!NOTE]
> Ahora es posible migrar mediante lift and shift la solución de grupo de disponibilidad a SQL Server en máquinas virtuales de Azure mediante Azure Migrate. Para más información, consulte [Migración del grupo de disponibilidad](../../migration-guides/virtual-machines/sql-server-availability-group-to-sql-on-azure-vm.md). 


## <a name="prerequisites"></a>Requisitos previos

Para configurar un grupo de disponibilidad Always On mediante Azure Portal, debe cumplir los siguientes requisitos previos: 

- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- Un grupo de recursos con un controlador de dominio. 
- Una o varias [máquinas virtuales de Azure con SQL Server 2016 (o posterior) Enterprise Edition](./create-sql-vm-portal.md) unidas a dominio en el *mismo* conjunto de disponibilidad o en zonas de disponibilidad *distintas* que se hayan [registrado en la extensión Agente de IaaS de SQL en modo de administración completa](sql-agent-extension-manually-register-single-vm.md) y que usen la misma cuenta de dominio para el servicio SQL Server en cada máquina virtual.
- Dos direcciones IP disponibles (no utilizadas por ninguna entidad). Una es para el equilibrador de carga interno. La otra es para el cliente de escucha de grupo de disponibilidad dentro de la misma subred que el grupo de disponibilidad. Si se usa un equilibrador de carga existente, solo se necesita una dirección IP disponible para la escucha de grupo de disponibilidad. 

## <a name="permissions"></a>Permisos

Se necesitan los siguientes permisos de cuenta para configurar el grupo de disponibilidad mediante Azure Portal: 

- Una cuenta de usuario de dominio existente que tenga el permiso **Crear objeto de equipo** en el dominio. Por ejemplo, una cuenta de administrador de dominio normalmente tiene permisos suficientes (como account@domain.com). _Esta cuenta también debe formar parte del grupo de administradores local en cada máquina virtual para crear el clúster._
- La cuenta de usuario del dominio que controla SQL Server. Debe ser la misma cuenta para cada VM con SQL Server que se pretenda agregar al grupo de disponibilidad. 

## <a name="configure-cluster"></a>Configuración del inicio de sesión del clúster

Configure el clúster mediante Azure Portal. Puede crear un nuevo clúster, o bien, si ya tiene uno existente, puede incorporarlo a la extensión Agente de IaaS de SQL para poder administrar el portal.


### <a name="create-a-new-cluster"></a>Creación de un clúster

Si ya tiene un clúster, omita esta sección y pase a [Incorporación de un clúster existente](#onboard-existing-cluster) en su lugar. 

Si aún no tiene un clúster, créelo mediante Azure Portal con estos pasos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya al recurso [Máquinas virtuales SQL](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines). 
1. Seleccione **Alta disponibilidad** en **Configuración**. 
1. Seleccione **+ New Windows Server failover cluster** (Nuevo clúster de conmutación por error de Windows Server) para abrir la página **Configure Windows Failover cluster** (Configurar clúster de conmutación por error de Windows).  

   :::image type="content" source="media/availability-group-az-portal-configure/create-new-cluster.png" alt-text="Creación de un nuevo clúster mediante la selección de + Nuevo clúster en el portal":::

1. Asigne un nombre al clúster y proporcione una cuenta de almacenamiento para usarla como testigo en la nube. Use una cuenta de almacenamiento existente o seleccione **Crear nueva** para crear una nueva. El nombre de la cuenta de almacenamiento debe tener entre 3 y 24 caracteres, y solo puede contener números y letras minúsculas.

   :::image type="content" source="media/availability-group-az-portal-configure/configure-new-cluster-1.png" alt-text="Asignación de nombre, cuenta de almacenamiento y credenciales al clúster":::

1. Expanda **Windows Server Failover Cluster credentials** (Credenciales del clúster de conmutación por error de Windows Server) para proporcionar las [credenciales](/rest/api/sqlvm/sqlvirtualmachinegroups/createorupdate#wsfcdomainprofile) para la cuenta de servicio de SQL Server, así como las cuentas de operador de clúster y de arranque si son diferentes a la cuenta usada para el servicio de SQL Server. 

   :::image type="content" source="media/availability-group-az-portal-configure/configure-new-cluster-2.png" alt-text="Asignación de credenciales para la cuenta de servicio de SQL, la cuenta de operador de clúster y la cuenta de arranque del clúster":::

1. Seleccione las VM con SQL Server que quiere agregar al clúster. Observe si se requiere un reinicio y proceda con precaución. Solo son visibles las máquinas virtuales registradas en la extensión Agente de IaaS de SQL en modo de administración completa y que se encuentran en la misma ubicación, el mismo dominio y la misma red virtual que la VM principal con SQL Server. 
1. Seleccione **Aplicar** para crear el clúster. Puede comprobar el estado de la implementación en el **registro de actividad**, que es accesible desde el icono de campana de la barra de navegación superior. 
1. Para que un clúster de conmutación por error sea compatible con Microsoft, debe pasar la validación del clúster. Conéctese a la máquina virtual mediante el método que prefiera, por ejemplo, Protocolo de escritorio remoto (RDP), y compruebe que el clúster pasa la validación antes de continuar. Si no lo hace, el clúster queda en un estado no admitido. Puede validar el clúster mediante el Administrador de clústeres de conmutación por error (FCM) o el siguiente comando de PowerShell:

    ```powershell
    Test-Cluster –Node ("<node1>","<node2>") –Include "Inventory", "Network", "System Configuration"
    ```
    


### <a name="onboard-existing-cluster"></a>Incorporación de un clúster existente

Si ya tiene un clúster configurado en el entorno de VM con SQL Server, puede incorporarlo desde Azure Portal.

Para hacerlo, siga estos pasos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya al recurso [Máquinas virtuales SQL](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines). 
1. Seleccione **Alta disponibilidad** en **Configuración**. 
1. Seleccione **Onboard existing Windows Server Failover Cluster** (Incorporar clúster de conmutación por error existente de Windows Server) para abrir la página **Onboard Windows Server Failover Cluster** (Incorporar clúster de conmutación por error de Windows Server). 

   :::image type="content" source="media/availability-group-az-portal-configure/onboard-existing-cluster.png" alt-text="Incorporación de un clúster existente desde la página Alta disponibilidad del recurso de máquinas virtuales de SQL":::

1. Revise la configuración del clúster. 
1. Seleccione **Aplicar** para incorporar el clúster y luego **Sí** en el símbolo del sistema para continuar.

## <a name="create-availability-group"></a>Crear grupo de disponibilidad

Una vez creado o incorporado el clúster, cree el grupo de disponibilidad mediante Azure Portal. Para hacerlo, siga estos pasos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya al recurso [Máquinas virtuales SQL](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines). 
1. Seleccione **Alta disponibilidad** en **Configuración**. 
1. Seleccione **+ New Always On availability group** (Nuevo grupo de disponibilidad Always On) para abrir la página **Crear grupo de disponibilidad**.

   :::image type="content" source="media/availability-group-az-portal-configure/create-new-availability-group.png" alt-text="Selección de Nuevo grupo de disponibilidad Always On para abrir la página Crear grupo de disponibilidad.":::

1. Escriba un nombre para el grupo de disponibilidad. 
1. Seleccione **Configure listener** (Configurar escucha) para abrir la página **Configure availability group listener** (Configurar escucha de grupo de disponibilidad). 

   :::image type="content" source="media/availability-group-az-portal-configure/create-availability-group.png" alt-text="Asignación de un nombre para el grupo de disponibilidad y configuración de una escucha":::

1. Rellene los valores y use un equilibrador de carga existente o bien seleccione **Crear nuevo** para crear un nuevo equilibrador de carga.  Seleccione **Aplicar** para guardar la configuración y crear la escucha y el equilibrador de carga. 

   :::image type="content" source="media/availability-group-az-portal-configure/configure-new-listener.png" alt-text="Especificación de los valores del formulario para crear la nueva escucha y el equilibrador de carga":::

1. Seleccione **+ Select replica** (Seleccionar réplica) para abrir la página **Configure availability group replicas** (Configurar réplicas de grupo de disponibilidad).
1. Seleccione las máquinas virtuales que quiere agregar al grupo de disponibilidad y luego la configuración de grupo de disponibilidad que mejor se adapte a sus necesidades empresariales. Seleccione **Aplicar** para guardar la configuración. 

   :::image type="content" source="media/availability-group-az-portal-configure/add-replicas.png" alt-text="Selección de las máquinas virtuales que se van a agregar al grupo de disponibilidad y configuración de las opciones adecuadas para la empresa":::

1. Compruebe la configuración del grupo de disponibilidad y seleccione **Aplicar** para crearlo. 

Puede comprobar el estado de la implementación en el **registro de actividad**, que es accesible desde el icono de campana de la barra de navegación superior. 

  > [!NOTE]
  > **Synchronization health** (Estado de sincronización), en la página **Alta disponibilidad** de Azure Portal, muestra **Incorrecto** hasta que se agregan bases de datos al grupo de disponibilidad. 


## <a name="add-database-to-availability-group"></a>Incorporación de una base de datos al grupo de disponibilidad

Agregue las bases de datos al grupo de disponibilidad una vez finalizada la implementación. En los pasos siguientes se usa SQL Server Management Studio (SSMS), pero también se puede usar [Transact-SQL o PowerShell](/sql/database-engine/availability-groups/windows/availability-group-add-a-database). 

Para agregar bases de datos al grupo de disponibilidad mediante SQL Server Management Studio, siga estos pasos:

1. Conéctese a una de las VM con SQL Server mediante el método que prefiera, como conexión a Escritorio remoto (RDP). 
1. Abra SQL Server Management Studio (SSMS).
1. Conéctese a la instancia de SQL Server. 
1. Expanda **Alta disponibilidad de Always On** en el **Explorador de objetos**.
1. Expanda **Grupos de disponibilidad**, haga clic con el botón derecho en el grupo de disponibilidad y seleccione **Agregar base de datos...**

   :::image type="content" source="media/availability-group-az-portal-configure/add-database.png" alt-text="Clic con el botón derecho en el grupo de disponibilidad en el Explorador de objetos y selección de Agregar base de datos":::

1. Siga los mensajes para seleccionar las bases de datos que quiere agregar al grupo de disponibilidad. 
1. Seleccione **Aceptar** para guardar la configuración y agregar la base de datos al grupo de disponibilidad. 
1. Una vez agregada la base de datos, actualice el **Explorador de objetos** para confirmar el estado de la base de datos como `synchronized`. 

Una vez agregadas las bases de datos, puede comprobar el estado del grupo de disponibilidad en Azure Portal: 

:::image type="content" source="media/availability-group-az-portal-configure/healthy-availability-group.png" alt-text="Comprobación del estado del grupo de disponibilidad en la página Alta disponibilidad de Azure Portal después de sincronizar las bases de datos":::

## <a name="add-more-vms"></a>Incorporación de más máquinas virtuales

Para agregar más VM con SQL Server al clúster, siga estos pasos: 

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya al recurso [Máquinas virtuales SQL](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines). 
1. Seleccione **Alta disponibilidad** en **Configuración**. 
1. Seleccione **Configure Windows Server Failover Cluster** (Configurar clúster de conmutación por error de Windows Server) para abrir la página **Configure Windows Server Failover Cluster** (Configurar clúster de conmutación por error de Windows Server). 

   :::image type="content" source="media/availability-group-az-portal-configure/configure-existing-cluster.png" alt-text="Selección de Configurar clúster de conmutación por error de Windows Server para agregar máquinas virtuales al clúster.":::

1. Expanda **Windows Server Failover Cluster credentials** (Credenciales del clúster de conmutación por error de Windows Server) y escriba las cuentas usadas para el servicio de SQL Server, el operador de clúster y el arranque del clúster. 
1. Seleccione las VM con SQL Server que quiere agregar al clúster. 
1. Seleccione **Aplicar**. 

Puede comprobar el estado de la implementación en el **registro de actividad**, que es accesible desde el icono de campana de la barra de navegación superior. 

## <a name="configure-quorum"></a>Configuración de un cuórum

Aunque el testigo de disco es la opción de cuórum más resistente, requiere un disco compartido de Azure que impone algunas limitaciones al grupo de disponibilidad. Por lo tanto, el testigo en la nube es la solución de cuórum recomendada para los clústeres que hospedan grupos de disponibilidad de SQL Server en VM de Azure. 

Si tiene un número par de votos en el clúster, configure la [solución de cuórum](hadr-cluster-quorum-configure-how-to.md) que mejor se adapte a sus necesidades empresariales. Para más información, consulte [Cuórum con VM con SQL Server](hadr-windows-server-failover-cluster-overview.md#quorum). 


## <a name="modify-availability-group"></a>Modificación de un grupo de disponibilidad 


Es posible **Add more replicas** (Agregar más réplicas) al grupo de disponibilidad, **Configure the Listener** (Configurar la escucha) o **Delete the Listener** (Eliminar la escucha) en la página **Alta disponibilidad** de Azure Portal si se seleccionan los puntos suspensivos (...) junto al grupo de disponibilidad: 

:::image type="content" source="media/availability-group-az-portal-configure/configure-listener.png" alt-text="Selección de los puntos suspensivos junto al grupo de disponibilidad y de Agregar réplica para agregar más réplicas al grupo de disponibilidad.":::

## <a name="remove-cluster"></a>Eliminación del clúster

Quite todas las VM con SQL Server del clúster para destruirlo y luego quite los metadatos del clúster de la extensión Agente de IaaS de SQL. Para ello, puede usar la versión más reciente de la [CLI de Azure](/cli/azure/install-azure-cli) o PowerShell. 

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

En primer lugar, quite todas las VM con SQL Server del clúster. Esto quita físicamente los nodos del clúster y lo destruye:  

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

az sql vm group delete --name <cluster name> --resource-group <resource group name>
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

Remove-AzSqlVMGroup -ResourceGroupName "<resource group name>" -Name "<cluster name>"
```

---

## <a name="troubleshooting"></a>Solución de problemas

Si tiene problemas, puede ver el historial de implementación y revisar los errores comunes y sus soluciones. 

### <a name="check-deployment-history"></a>Visualización del historial de implementación

Los cambios en el clúster y el grupo de disponibilidad por medio del portal se realizan mediante implementaciones. El historial de implementación puede proporcionar más detalles en caso de problemas a la hora de crear, o incorporar el clúster, o de crear el grupo de disponibilidad.

Para ver los registros de la implementación y consultar el historial de implementación, siga estos pasos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).
1. Vaya al grupo de recursos.
1. Seleccione **Implementaciones** en **Configuración**.
1. Seleccione la implementación que le interese para obtener más información sobre ella. 


   :::image type="content" source="media/availability-group-az-portal-configure/failed-deployment.png" alt-text="Selección de la implementación que interesa para obtener más información." :::

### <a name="common-errors"></a>Errores comunes

Revise los siguientes errores comunes y sus soluciones. 

#### <a name="the-account-which-is-used-to-start-up-sql-service-is-not-a-domain-account"></a>La cuenta que se usa para iniciar el servicio SQL no es una cuenta de dominio

Esto indica que el proveedor de recursos no ha podido acceder al servicio de SQL Server con las credenciales proporcionadas. Algunas soluciones comunes:
- Asegúrese de que el controlador de dominio se está ejecutando.
- Compruebe que las credenciales proporcionadas en el portal coinciden con las del servicio de SQL Server. 


## <a name="next-steps"></a>Pasos siguientes

Una vez que se haya implementado el grupo de disponibilidad, puede optimizar la [configuración de alta disponibilidad y recuperación ante desastres para SQL Server en máquinas virtuales de Azure](hadr-cluster-best-practices.md). 


Para obtener más información, consulte:

- [Clúster de conmutación por error de Windows Server con SQL Server en máquinas virtuales de Azure](hadr-windows-server-failover-cluster-overview.md)
- [Grupos de disponibilidad Always On para SQL Server en Azure Virtual Machines](availability-group-overview.md)
- [Introducción a los grupos de disponibilidad Always On](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)
