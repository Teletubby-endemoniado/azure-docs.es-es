---
title: Cambios en la documentación de SQL Server en Azure Virtual Machines
description: Conozca las nuevas características y mejoras de las diferentes versiones de SQL Server en Azure Virtual Machines.
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.subservice: service-overview
ms.topic: reference
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 09/01/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: 592f2ae9b3d09ce84296dca31d68031b7d5fff84
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131080923"
---
# <a name="documentation-changes-for-sql-server-on-azure-virtual-machines"></a>Cambios en la documentación de SQL Server en Azure Virtual Machines
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Al implementar una máquina virtual (VM) de Azure con SQL Server instalado en ella, ya sea manualmente o a través de una imagen integrada, puede aprovechar las características de Azure para mejorar su experiencia. En este artículo se resumen los cambios en la documentación asociados a las nuevas características y mejoras de las versiones recientes de [SQL Server en Azure Virtual Machines (VM)](https://azure.microsoft.com/services/virtual-machines/sql-server/). Para más información sobre SQL Server en máquinas virtuales de Azure, consulte la [introducción](sql-server-on-azure-vm-iaas-what-is-overview.md). 

## <a name="november-2021"></a>Noviembre de 2021

| Cambios | Detalles |
| --- | --- |
| **Evaluación de SQL** | Ahora es posible evaluar el estado de la máquina virtual con SQL Server en Azure Portal mediante [SQL Assessment](sql-assessment-for-sql-vm.md) para obtener recomendaciones que mejoren el rendimiento e identificar las configuraciones de procedimientos recomendados que faltan. Esta funcionalidad actualmente está en su versión preliminar. |


## <a name="october-2021"></a>Octubre de 2021

| Cambios | Detalles |
| --- | --- |
| **SQL La extensión IaaS ahora admite Ubuntu** | Se ha agregado compatibilidad para [registrar](../linux/sql-iaas-agent-extension-register-vm-linux.md) la máquina virtual con SQL Server que se ejecuta en Ubuntu Linux con la [extensión IaaS de SQL Server](../linux/sql-server-iaas-agent-extension-linux.md) para una funcionalidad limitada. | 


## <a name="september-2021"></a>Septiembre de 2021

| Cambios | Detalles |
| --- | --- |
| **El modo completo de la extensión IaaS de SQL ya no requiere reiniciar** | El reinicio del servicio SQL Server ya no es necesario al registrar la VM de SQL Server en [modo completo](sql-agent-extension-manually-register-single-vm.md#full-mode) de la [extensión Agente de IaaS de SQL](sql-server-iaas-agent-extension-automate-management.md). | 


## <a name="july-2021"></a>Julio de 2021

| Cambios | Detalles |
| --- | --- |
| **Reparación de la extensión IaaS de SQL Server en el portal** | Ahora es posible comprobar el estado de la extensión del agente IaaS de SQL Server desde Azure Portal y [repararlo](sql-agent-extension-manually-register-single-vm.md#repair-extension), si es necesario. | 


## <a name="june-2021"></a>Junio de 2021

| Cambios | Detalles |
| --- | --- |
| **Mejoras de seguridad en Azure Portal** | Cuando haya habilitado [Azure Defender para SQL](../../../security-center/defender-for-sql-usage.md), puede ver las recomendaciones de Security Center en el [recurso de máquinas virtuales de SQL de Azure Portal](manage-sql-vm-portal.md#security-center). | 

## <a name="may-2021"></a>Mayo de 2021

| Cambios | Detalles |
| --- | --- |
| **Actualización de contenido de HADR** | Hemos actualizado y mejorado nuestro contenido de alta disponibilidad y recuperación ante desastres (HADR). Ahora hay el artículo [Información general del clúster de conmutación por error de Windows Server](hadr-windows-server-failover-cluster-overview.md) disponible, así como un [procedimiento consolidado de configuración del quórum](hadr-cluster-quorum-configure-how-to.md) para las máquinas virtuales de SQL Server.  Además, hemos mejorado los [procedimientos recomendados del clúster](hadr-cluster-best-practices.md) con recomendaciones de configuración más completas que se han adoptado en la nube.| 


## <a name="april-2021"></a>Abril de 2021

| Cambios | Detalles |
| --- | --- |
| **Migración de alta disponibilidad a una máquina virtual** | Azure Migrate ofrece compatibilidad con la migración mediante lift-and-shift de toda la solución de alta disponibilidad a SQL Server en máquinas virtuales de Azure. Traiga el [grupo de disponibilidad](../../migration-guides/virtual-machines/sql-server-availability-group-to-sql-on-azure-vm.md) o la [instancia de clúster de conmutación por error](../../migration-guides/virtual-machines/sql-server-failover-cluster-instance-to-sql-on-azure-vm.md) a máquinas virtuales de SQL Server Azure Migrate hoy mismo. | 


## <a name="march-2021"></a>Marzo de 2021

| Cambios | Detalles |
| --- | --- |
| **Actualización de procedimientos recomendados de rendimiento** | Hemos reescrito, actualizado y puesto al día la documentación de procedimientos recomendados de rendimiento, dividiendo un artículo en una serie que contiene: [una lista de comprobación](performance-guidelines-best-practices-checklist.md), la [guía de tamaño de máquinas virtuales](performance-guidelines-best-practices-vm-size.md), la [guía de almacenamiento](performance-guidelines-best-practices-storage.md) y [una recopilación de instrucciones base de referencia](performance-guidelines-best-practices-collect-baseline.md).   | 



## <a name="2020"></a>2020

| Cambios | Detalles |
| --- | --- |
| **Soporte de Azure Government** | Ahora es posible registrar máquinas virtuales con SQL Server con la extensión Agente de IaaS de SQL para máquinas virtuales hospedadas en la nube de [Azure Government](https://azure.microsoft.com/global-infrastructure/government/). | 
| **Familia Azure SQL** | SQL Server en Azure Virtual Machines ahora forma parte de la [familia de productos de Azure SQL](../../azure-sql-iaas-vs-paas-what-is-overview.md). Eche un vistazo a nuestro [nuevo aspecto](../index.yml). No ha cambiado nada en el producto, pero la documentación pretende facilitar la decisión sobre los productos de Azure SQL. | 
| **Nombre de red distribuida (DNN)** | Ahora, SQL Server 2019 en Windows Server 2016+ ofrece en versión preliminar compatibilidad con el enrutamiento del tráfico a la instancia de clúster de conmutación por error (FCI) mediante el uso de un [nombre de red distribuida](./failover-cluster-instance-distributed-network-name-dnn-configure.md) en lugar de con Azure Load Balancer. Esta compatibilidad simplifica y agiliza la conexión a la solución de alta disponibilidad en Azure. | 
| **FCI con discos compartidos de Azure** | Ahora es posible implementar la [instancia de clúster de conmutación por error (FCI)](failover-cluster-instance-overview.md) mediante [discos compartidos de Azure](failover-cluster-instance-azure-shared-disks-manually-configure.md). |
| **Documentos de FCI reorganizados** | La documentación sobre las [instancias de clúster de conmutación por error con SQL Server en Azure Virtual Machines](failover-cluster-instance-overview.md) se ha vuelto a redactar y reorganizar para mayor claridad. Hemos separado parte del contenido de configuración, como los [procedimientos recomendados de configuración de clústeres](hadr-cluster-best-practices.md), cómo preparar una [máquina virtual para una FCI de SQL Server](failover-cluster-instance-prepare-vm.md) y cómo configurar [Azure Load Balancer](./availability-group-vnn-azure-load-balancer-configure.md). | 
| **Migración del registro a un disco Ultra** | Obtenga información acerca de cómo puede [migrar el archivo de registro a un disco Ultra](storage-migrate-to-ultradisk.md) para aprovechar el alto rendimiento y la baja latencia. | 
| **Creación de un grupo de disponibilidad con Azure PowerShell** | Ahora es posible simplificar la creación de un grupo de disponibilidad mediante [Azure PowerShell](availability-group-az-commandline-configure.md), así como la CLI de Azure. | 
| **Configuración AG en el portal** | Ahora es posible [configurar el grupo de disponibilidad a través de Azure Portal](availability-group-azure-portal-configure.md). Esta característica se encuentra actualmente en versión preliminar y en implementación, de modo que si la región deseada no está disponible, vuelva a comprobarlo pronto. | 
| **Registro de extensión automático** | Ahora puede habilitar la característica de [Registro automático](sql-agent-extension-automatic-registration-all-vms.md) para registrar automáticamente todas las VM con SQL Server ya implementadas en la suscripción con la [extensión Agente de IaaS de SQL](sql-server-iaas-agent-extension-automate-management.md). Esto se aplica a todas las máquinas virtuales existentes, y también se registrarán automáticamente todas las VM con SQL Server agregadas en el futuro.   | 
| **DNN para AG** | Ahora puede configurar un [cliente de escucha de nombre de red distribuida (DNN)](availability-group-distributed-network-name-dnn-listener-configure.md) para SQL Server 2019 CU8 y versiones posteriores para reemplazar el [cliente de escucha de VNN](availability-group-overview.md#connectivity) tradicional, lo que evita la necesidad de una instancia de Azure Load Balancer.   | 
| &nbsp; | &nbsp; |

## <a name="2019"></a>2019

|Cambios | Detalles |
 --- | --- |
| **Réplica de DR gratuita en Azure** | Puede hospedar una [instancia pasiva gratuita](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) para la recuperación ante desastres en Azure para su instancia de SQL Server local si tiene [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot:primaryr3). | 
| **Registro en masa de la extensión de IaaS de SQL** | Ahora puede [registrar masivamente](sql-agent-extension-manually-register-vms-bulk.md) las máquinas virtuales con SQL Server con la [extensión Agente de IaaS de SQL](sql-server-iaas-agent-extension-automate-management.md). | 
|**Configuración del almacenamiento optimizado para el rendimiento** | Ya se puede [personalizar totalmente la configuración del almacenamiento](storage-configuration.md#new-vms) al crear una nueva máquina virtual con SQL Server. |
|**Recursos compartidos de archivos premium para FCI** | Ahora puede crear una instancia de clúster de conmutación por error mediante un [recurso compartido de archivos Premium](failover-cluster-instance-premium-file-share-manually-configure.md) en lugar del método original de [Espacios de almacenamiento directo](failover-cluster-instance-storage-spaces-direct-manually-configure.md). 
| **Azure Dedicated Host** | Puede ejecutar VM con SQL Server en [Azure Dedicated Host](dedicated-host.md). | 
| **Migración de VM con SQL Server a una región diferente** | Use Azure Site Recovery para [migrar las máquinas virtuales con SQL Server de una región de Azure a otra](move-sql-vm-different-region.md). |
|  **Nuevos modos de instalación de IaaS de SQL** | Ahora es posible instalar la extensión IaaS de SQL Server en [modo ligero](sql-server-iaas-agent-extension-automate-management.md) para evitar tener que reiniciar el servicio SQL Server.  |
| **Modificación de la edición de SQL Server** | Ahora puede cambiar la [propiedad edition](change-sql-server-edition.md) de la VM con SQL Server. |
| **Cambios en la extensión Agente de IaaS de SQL** | Puede [registrar la VM con SQL Server con la extensión Agente de IaaS de SQL](sql-agent-extension-manually-register-single-vm.md) mediante los nuevos modos de IaaS de SQL. Esta funcionalidad incluye [imágenes de Windows Server 2008](sql-server-iaas-agent-extension-automate-management.md#management-modes).|
| **Imágenes del tipo "traiga su propia licencia" mediante la Ventaja híbrida de Azure** | Ahora es posible cambiar al [tipo de licencia de pago por uso](licensing-model-azure-hybrid-benefit-ahb-change.md#remarks) las imágenes del tipo "traiga su propia licencia" implementadas desde Azure Marketplace.| 
| **Nueva administración de VM con SQL Server en Azure Portal** | Ahora existe una nueva forma de administrar una máquina virtual con SQL Server en Azure Portal. Para más información, consulte [Administración de máquinas virtuales con SQL Server en Azure Portal](manage-sql-vm-portal.md).  | 
| **Compatibilidad ampliada con SQL Server 2008 y SQL Server 2008 R2** | [Amplíe la compatibilidad](sql-server-2008-extend-end-of-support.md) con SQL Server 2008 y SQL Server 2008 R2 mediante la migración *tal cual* a una máquina virtual de Azure. | 
| **Compatibilidad de imágenes personalizada** | Ahora puede instalar la [extensión IaaS de SQL Server](sql-server-iaas-agent-extension-automate-management.md#installation) en las imágenes de SQL Server y de sistemas operativos personalizadas, lo que ofrece una funcionalidad limitada de [licencias flexibles](licensing-model-azure-hybrid-benefit-ahb-change.md). Al registrar una imagen personalizada con la extensión Agente de IaaS de SQL, especifique el tipo de licencia como "AHUB". De lo contrario, el registro no podrá realizarse. | 
| **Compatibilidad de instancias con nombre** | Ahora puede usar la [extensión IaaS de SQL Server](sql-server-iaas-agent-extension-automate-management.md#installation) con una instancia con nombre, si la instancia predeterminada se ha desinstalado correctamente. | 
| **Renovación del portal** | La experiencia de Azure Portal para implementar una VM con SQL Server se ha renovado para mejorar la facilidad de uso. Para más información, vea el breve [inicio rápido](sql-vm-create-portal-quickstart.md) y la guía [paso a paso](create-sql-vm-portal.md) detallada para implementar una máquina virtual con SQL Server.|
| **Mejora del portal** | Ahora es posible cambiar el modelo de licencias para una máquina virtual con SQL Server del tipo "pago por uso" a "traiga su propia licencia" mediante [Azure Portal](licensing-model-azure-hybrid-benefit-ahb-change.md#change-license-model).|
| **Simplificación de la implementación de un grupo de disponibilidad en una instancia de VM con SQL Server mediante la CLI de Azure** | Ahora es más fácil que nunca implementar un grupo de disponibilidad en una máquina virtual con SQL Server en Azure. Puede usar la [CLI de Azure](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid&preserve-view=true) para crear el clúster de conmutación por error de Windows, el equilibrador de carga interno y los clientes de escucha de grupo de disponibilidad, todo ello desde la línea de comandos. Para más información, consulte [Uso de la CLI de Azure para configurar un grupo de disponibilidad Always On para SQL Server en una máquina virtual de Azure](./availability-group-az-commandline-configure.md). | 
| &nbsp; | &nbsp; |

## <a name="2018"></a>2018 

 Cambios | Detalles |
| --- | --- |
|  **Nuevo proveedor de recursos para un clúster de SQL Server** | Un nuevo proveedor de recursos (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) que define los metadatos del clúster de conmutación por error de Windows. Al unir una máquina virtual con SQL Server a *SqlVirtualMachineGroups*, se arranca el servicio Clúster de conmutación por error de Windows Server (WSFC) y la máquina virtual se une al clúster.  |
| **Configuración automatizada de la implementación de un grupo de disponibilidad con plantillas de inicio rápido de Azure** |Ahora es posible crear el clúster de conmutación por error de Windows, unirlo a las máquinas virtuales VM con SQL Server, crear el cliente de escucha y configurar el equilibrador de carga interno con dos plantillas de inicio rápido de Azure. Para más información, consulte [Uso de plantillas de inicio rápido de Azure para configurar un grupo de disponibilidad Always On para SQL Server en una máquina virtual de Azure](availability-group-quickstart-template-configure.md). | 
| **Registro automático en la extensión Agente de IaaS de SQL** | Las máquinas virtuales con SQL Server implementadas después de este mes se registran automáticamente con la extensión Agente de IaaS de SQL. Las máquinas virtuales con SQL Server implementadas antes de este mes todavía deben registrarse manualmente. Para más información, consulte [Registro de una máquina virtual con SQL Server en Azure con la extensión Agente de IaaS de SQL](sql-agent-extension-manually-register-single-vm.md).|
|**Nueva extensión Agente de IaaS de SQL** |  Un nuevo proveedor de recursos (Microsoft.SqlVirtualMachine) proporciona una mejor administración de las máquinas virtuales con SQL Server. Para más información sobre cómo registrar las máquinas virtuales, consulte [Registro de una máquina virtual con SQL Server en Azure con la extensión Agente de IaaS de SQL](sql-agent-extension-manually-register-single-vm.md). |
|**Cambiar el modelo de licencia** | Ahora puede cambiar entre los modelos de "pago por uso" y "traiga su propia licencia" para la máquina virtual con SQL Server mediante PowerShell o la CLI de Azure. Para más información, consulte [Modificación del modelo de licencia para una máquina virtual con SQL Server en Azure](licensing-model-azure-hybrid-benefit-ahb-change.md). | 
| &nbsp; | &nbsp; |

## <a name="additional-resources"></a>Recursos adicionales

**Máquinas virtuales Windows**:

* [Introducción a SQL Server en máquinas virtuales Windows](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Aprovisionamiento de SQL Server en una máquina virtual Windows](create-sql-vm-portal.md)
* [Migración de una base de datos a SQL Server en una máquina virtual de Azure](migrate-to-vm-from-sql-server.md)
* [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](business-continuity-high-availability-disaster-recovery-hadr-overview.md)
* [Directrices de rendimiento para SQL Server en Azure Virtual Machines](./performance-guidelines-best-practices-checklist.md)
* [Estrategias de desarrollo y patrones de aplicación de SQL Server en Azure Virtual Machines](application-patterns-development-strategies.md)

**Máquinas virtuales Linux**:

* [Introducción a SQL Server en máquinas virtuales Linux](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md)
* [Aprovisionamiento de una máquina virtual Linux con SQL Server en Azure Portal](../linux/sql-vm-create-portal-quickstart.md)
* [Preguntas más frecuentes (Linux)](../linux/frequently-asked-questions-faq.yml)
* [Documentación de SQL Server en Linux](/sql/linux/sql-server-linux-overview)
