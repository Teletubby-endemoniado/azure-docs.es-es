---
title: Información general sobre la migración compatible con la plataforma de recursos de IaaS desde el modelo de implementación clásica a Azure Resource Manager
description: Realice un recorrido por la migración compatible con la plataforma de recursos del modelo clásico a Azure Resource Manager.
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.subservice: classic-to-arm-migration
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: 1d5391d90c4770377a85eb6d78cd8cc30f10ccee
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130249217"
---
# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Migración compatible con la plataforma de recursos de IaaS del modelo clásico al de Azure Resource Manager

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows

> [!IMPORTANT]
> En la actualidad, aproximadamente el 90 % de las VM de IaaS usan [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/). A partir del 28 de febrero de 2020, las máquinas virtuales clásicas han quedado en desuso y se van a retirar por completo el 1 de marzo de 2023. [Obtenga más información]( https://aka.ms/classicvmretirement) sobre esta caída en desuso y [cómo se verá afectado](classic-vm-deprecation.md#how-does-this-affect-me).



En este artículo se proporciona información general sobre la herramienta de migración compatible con la plataforma, se describe cómo migrar recursos de los modelos de implementación de Azure Service Manager (ASM, conocidos como clásicos) a Resource Manager (ARM) y se detalla cómo conectar recursos de los dos modelos de implementación que coexisten en la suscripción mediante puertas de enlace de sitio a sitio de red virtual. Se puede leer más información sobre [características y ventajas de Azure Resource Manager](../azure-resource-manager/management/overview.md). 

ASM admite dos productos de proceso diferentes, Azure Virtual Machines (clásico), también conocido como máquinas virtuales de IaaS, y [Azure Cloud Services (clásico)](../cloud-services/index.yml), también conocido como máquinas virtuales PaaS o roles web o de trabajo. Este documento solo habla sobre la migración de Azure Virtual Machines (clásico).

## <a name="goal-for-migration"></a>Objetivo para la migración
Resource Manager permite implementar aplicaciones complejas a través de plantillas, configura las máquinas virtuales mediante extensiones de máquina virtual e incorpora la administración de acceso y el etiquetado. Azure Resource Manager incluye una implementación escalable en paralelo para máquinas virtuales en conjuntos de disponibilidad. El nuevo modelo de implementación también proporciona administración del ciclo de vida para procesos, redes y almacenamiento por separado. Por último, también se centra en habilitar la seguridad de forma predeterminada con la aplicación de máquinas virtuales en una red virtual.

En Azure Resource Manager, se admiten casi todas las características del modelo de implementación clásica en cuanto a proceso, red y almacenamiento. Para sacar partido de las nuevas funcionalidades de Resource Manager, puede migrar las implementaciones existentes desde el modelo de implementación clásica.

## <a name="supported-resources--configurations-for-migration"></a>Recursos admitidos y configuraciones para la migración

### <a name="supported-resources-for-migration"></a>Recursos que se admiten en la migración
* Virtual Machines (servicio en la nube con máquinas virtuales)
* [Cloud Services (con roles web o de trabajo)](../cloud-services-extended-support/in-place-migration-overview.md)
* Conjuntos de disponibilidad
* Cuentas de almacenamiento
* Virtual Networks
* Puertas de enlace de VPN
* [Puertas de enlace de ExpressRoute](../expressroute/expressroute-howto-move-arm.md) _(en la misma suscripción que solo la red virtual)_
* Grupos de seguridad de red
* Tablas de ruta
* Direcciones IP reservadas

## <a name="supported-configurations-for-migration"></a>Configuraciones necesarias para la migración
Estos recursos de IaaS clásicos se admiten durante la migración

| Servicio | Configuración |
| --- | --- |
| Azure AD Domain Services | [Redes virtuales que contienen servicios de dominio de Azure AD](../active-directory-domain-services/migrate-from-classic-vnet.md) |

## <a name="supported-scopes-of-migration"></a>Ámbitos admitidos de la migración
Hay cuatro maneras diferentes de completar la migración de los recursos de proceso, red y almacenamiento:

* [Migración de máquinas virtuales (NO en una red virtual)](#migration-of-virtual-machines-not-in-a-virtual-network)
* [Migración de máquinas virtuales (en una red virtual)](#migration-of-virtual-machines-in-a-virtual-network)
* [Migración de cuentas de almacenamiento](#migration-of-storage-accounts)
* [Migración de recursos sin asociar](#migration-of-unattached-resources)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migración de máquinas virtuales (NO en una red virtual)
En el modelo de implementación de Resource Manager, la seguridad de las aplicaciones se aplica de forma predeterminada. En este modelo, todas las máquinas virtuales deben estar en una red virtual. La plataforma Azure reinicia (`Stop`, `Deallocate` y `Start`) las máquinas virtuales como parte de la migración. Hay dos opciones para las redes virtuales a las que se Virtual Machines va a migrar:

* Puede solicitar que la plataforma cree una red virtual y migrar la máquina virtual a ella.
* Puede migrar la máquina virtual a una red virtual existente en Resource Manager.

> [!NOTE]
> En este ámbito de migración, es posible que haya un determinado período durante la migración en que no se permitan las operaciones en el plano de administración y el plano de datos.
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migración de máquinas virtuales (en una red virtual)
Para la mayoría de las configuraciones de máquina virtual, solo se migran los metadatos entre el modelo de implementación clásica y el de Resource Manager. Las máquinas virtuales subyacentes se ejecutan en el mismo hardware, en la misma red y con el mismo almacenamiento. Es posible que haya un determinado período durante la migración en que no se permitan las operaciones en el plano de administración. Sin embargo, el plano de datos sigue funcionando. Es decir, las aplicaciones que se ejecutan en máquinas virtuales (clásicas) no experimentan tiempo de inactividad durante la migración.

Actualmente no se admiten las siguientes configuraciones. Si se agrega compatibilidad en el futuro, es posible que algunas máquinas virtuales en esta configuración sufran tiempos de inactividad (pasen por operaciones de detención, desasignación y reinicio).

* Tiene más de un conjunto de disponibilidad en un único servicio en la nube.
* Tiene uno o varios conjuntos de disponibilidad y máquinas virtuales que no están en un conjunto de disponibilidad en un único servicio en la nube.

> [!NOTE]
> En este ámbito de migración, es posible que haya un determinado período durante la migración en que no se permitan las operaciones en el plano de administración. Para determinadas configuraciones descritas anteriormente, se producirá un tiempo de inactividad en el plano de datos.
>

### <a name="migration-of-storage-accounts"></a>Migración de cuentas de almacenamiento
Para lograr una migración sin problemas, se pueden implementar máquinas virtuales de Resource Manager en una cuenta de almacenamiento clásico. Con esta funcionalidad, los recursos de procesos y redes se pueden y se deben migrar con independencia de las cuentas de almacenamiento. Una vez migradas las máquinas virtuales y la red virtual, se deben migrar las cuentas de almacenamiento para completar el proceso de migración.

Si la cuenta de almacenamiento no tiene ningún disco o datos de Máquinas virtuales asociados y solo tiene blobs, archivos, tablas y colas, entonces la migración a Azure Resource Manager se puede realizar como una migración independiente sin dependencias.

> [!NOTE]
> El modelo de implementación de Resource Manager carece del concepto de discos e imágenes de la implementación clásica. Cuando se migra la cuenta de almacenamiento, los discos e imágenes de la implementación clásica ya no son visibles en Azure Portal pero los discos duros virtuales de respaldo permanecen en la cuenta de almacenamiento.

En las capturas de pantalla siguientes se muestra cómo actualizar una cuenta de almacenamiento de la implementación clásica a una cuenta de almacenamiento de Azure Resource Manager mediante Azure Portal:
1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Vaya a la cuenta de almacenamiento.
3. En la sección **Configuración**, haga clic en **Migrar a Azure Resource Manager**.
4. Haga clic en **Validar** para determinar la viabilidad de la migración.
5. Si la validación es correcta, haga clic en **Preparar** para crear una cuenta de almacenamiento migrada.
6. Escriba **Sí** para confirmar la migración y haga clic en **Confirmar** para finalizar la migración.

    ![Validar la cuenta de almacenamiento](../../includes/media/storage-account-upgrade-classic/storage-migrate-resource-manager-1.png)
    
    ![Preparar la cuenta de almacenamiento](../../includes/media/storage-account-upgrade-classic/storage-migrate-resource-manager-2.png)
    
    ![Finalizar la migración de la cuenta de almacenamiento](../../includes/media/storage-account-upgrade-classic/storage-migrate-resource-manager-3.png)

### <a name="migration-of-unattached-resources"></a>Migración de recursos sin asociar
Las cuentas de almacenamiento que no tengan discos o datos de máquinas virtuales asociados se puede migrar de forma independiente.

Los grupos de seguridad de red, las tablas de ruta y las IP reservadas que no están conectadas a ninguna máquina virtual y red virtual también se pueden migrar de forma independiente.

<br>

## <a name="unsupported-features-and-configurations"></a>Configuraciones y características no admitidas
Actualmente no se admiten ciertas características y configuraciones, aunque en las siguientes secciones encontrará una descripción con nuestras recomendaciones.

### <a name="unsupported-features"></a>Características no admitidas
Actualmente no se admiten las siguientes características. Opcionalmente, puede quitar estas configuraciones, migrar las máquinas virtuales y después volver a habilitar dichas configuraciones en el modelo de implementación de Resource Manager.

| Proveedor de recursos | Característica | Recomendación |
| --- | --- | --- |
| Proceso | Discos de máquinas virtuales no asociados. | Los blobs de VHD que hay detrás de estos discos se migrarán cuando lo haga la cuenta de almacenamiento |
| Proceso | Imágenes de máquina virtual. | Los blobs de VHD que hay detrás de estos discos se migrarán cuando lo haga la cuenta de almacenamiento |
| Red | ACL de puntos de conexión. | Quitar las ACL de los puntos de conexión y vuelva a intentar la migración. |
| Red | Application Gateway | Quite Application Gateway antes de comenzar la migración y, después, vuelva a crearlo una vez que la migración se complete. |
| Red | Redes virtuales que usan el emparejamiento de VNET. | Migrar Virtual Network a Resource Manager y, después, del mismo nivel. Más información acerca del [emparejamiento de VNET](../virtual-network/virtual-network-peering-overview.md). |

### <a name="unsupported-configurations"></a>Configuraciones no admitidas
Actualmente no se admiten las siguientes configuraciones.

| Servicio | Configuración | Recomendación |
| --- | --- | --- |
| Resource Manager |Control de acceso basado en rol (RBAC) para recursos clásicos |Puesto que el identificador URI de los recursos se modifica después de la migración, se recomienda planear las actualizaciones de directiva del control de acceso basado en rol que deben producirse después de la migración. |
| Proceso |Varias subredes asociadas con una máquina virtual |Actualice la configuración de subred para que solo haga referencia a una subred. Puede que para ello sea necesario quitar una NIC secundaria (que hace referencia a otra subred) de la máquina virtual y asociarla de nuevo una vez finalizada la migración. |
| Proceso |Máquinas virtuales que pertenecen a una red virtual, pero no tienen una subred explícita asignada |Opcionalmente, puede eliminar la máquina virtual. |
| Proceso |Máquinas virtuales que tienen alertas, directivas de escalado automático |Se efectúa la migración y se descartan estos valores. Es muy recomendable evaluar el entorno antes de realizar la migración. Como alternativa, puede reconfigurar los valores de las alertas una vez completada la migración. |
| Proceso |Extensiones XML de máquina virtual (BGInfo 1.*, depurador de Visual Studio, Web Deploy y depuración remota) |Esto no se admite. Se recomienda que quite estas extensiones de la máquina virtual para continuar la migración o se quitarán automáticamente durante el proceso. |
| Proceso |Diagnóstico de arranque con Almacenamiento premium |Deshabilite la característica de diagnósticos de arranque para las máquinas virtuales antes de continuar con la migración. Puede volver a habilitar los diagnósticos de arranque en la pila de Resource Manager una vez completada la migración. Además, se deben eliminar los blobs que se utilizan para los registros de captura de pantalla y de serie, por lo que ya no se cobra por los blobs. |
| Proceso | Servicios en la nube que contienen más de un conjunto de disponibilidad o varios. |Actualmente no se admite. Mueva Virtual Machines al mismo conjunto de disponibilidad antes de la migración. |
| Proceso | VM con extensión de Azure Security Center | Azure Security Center instala automáticamente las extensiones en las máquinas virtuales para supervisar la seguridad y generar alertas. Si está habilitada la directiva de Azure Security Center en la suscripción, estas extensiones se suelen instalar automáticamente. Para migrar las máquinas virtuales, deshabilite la directiva de Security Center en la suscripción; esta operación quitará la extensión de supervisión de Security Center de las máquinas virtuales. |
| Proceso | VM con extensión de instantánea o copia de seguridad | Estas extensiones se instalan en una máquina virtual configurada con el servicio Azure Backup. Aunque no se admite la migración de estas máquinas virtuales, siga las instrucciones de [Preguntas más frecuentes sobre la migración del método clásico al de Azure Resource Manager](./migration-classic-resource-manager-faq.yml) para conservar las copias de seguridad realizadas antes de la migración.  |
| Proceso | VM con la extensión de Azure Site Recovery | Estas extensiones se instalan en una máquina virtual configurada con el servicio Azure Site Recovery. Aunque la migración del almacenamiento que se usa con Site Recovery funcionará, la replicación actual se verá afectada. Debe deshabilitar y habilitar la replicación de la VM después de la migración del almacenamiento. |
| Red |Redes virtuales que contienen máquinas virtuales y roles web y de trabajo |Actualmente no se admite. Mueva los roles web y de trabajo a su propia Virtual Network antes de la migración. Una vez que se migra Virtual Network clásica, Virtual Network de Azure Resource Manager se puede emparejar con Virtual Network clásica para lograr una configuración similar a la anterior.|
| Red | Circuitos ExpressRoute clásicos |Actualmente no se admite. Estos circuitos se deben migrar a Azure Resource Manager antes de comenzar la migración de IaaS. Para obtener más información, consulte la [transición de los circuitos ExpressRoute del modelo de implementación clásica al modelo de implementación de Resource Manager](../expressroute/expressroute-move.md).|
| Azure App Service |Redes virtuales que contienen entornos de App Service |Actualmente no se admite. |
| HDInsight de Azure |Redes virtuales que contienen servicios de HDInsight |Actualmente no se admite. |
| Dynamics Lifecycle Services |Redes virtuales que contienen máquinas virtuales administradas por Dynamics Lifecycle Services |Actualmente no se admite. |
| Azure API Management |Redes virtuales que contienen implementaciones de Azure API Management |Actualmente no se admite. Para migrar la red virtual de IaaS, cambie la red virtual de la implementación de API Management que no sea una operación de tiempo de inactividad. |

## <a name="next-steps"></a>Pasos siguientes

* [Profundización técnica en la migración compatible con la plataforma de la implementación clásica a la de Azure Resource Manager](migration-classic-resource-manager-deep-dive.md)
* [Planning for migration of IaaS resources from classic to Azure Resource Manager](migration-classic-resource-manager-plan.md) (Planificación de la migración de recursos de IaaS del modelo clásico a Azure Resource Manager)
* [Migración de recursos de IaaS de la implementación clásica a Resource Manager con Azure PowerShell](migration-classic-resource-manager-ps.md)
* [Migración de recursos de IaaS de la implementación clásica a Azure Resource Manager con la CLI de Azure](migration-classic-resource-manager-cli.md)
* [Herramientas de la comunidad para ayudar con la migración de recursos de IaaS del modelo de implementación clásica a Azure Resource Manager](migration-classic-resource-manager-community-tools.md)
* [Revisión de los errores más comunes en la migración](migration-classic-resource-manager-errors.md)
* [Revisión de las preguntas más frecuentes acerca de cómo migrar recursos de IaaS del modelo de implementación clásica a Azure Resource Manager](migration-classic-resource-manager-faq.yml)
