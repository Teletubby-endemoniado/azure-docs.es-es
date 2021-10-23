---
title: Alta disponibilidad, recuperación ante desastres y continuidad empresarial
description: Obtenga información sobre las opciones de alta disponibilidad, recuperación ante desastres (HADR) y continuidad empresarial disponibles para SQL Server en VM de Azure, como los grupos de disponibilidad Always On, una instancia de clúster de conmutación por error, creación de reflejo de la base de datos, trasvase de registros y copia de seguridad y restauración en Azure Storage.
services: virtual-machines-windows
documentationcenter: na
author: rajeshsetlem
editor: ''
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.subservice: hadr
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2020
ms.author: rsetlem
ms.reviewer: mathoma
ms.openlocfilehash: f5dd02d8abf70d3812fe0d07baa8ddf08bdc8a7a
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130166066"
---
# <a name="business-continuity-and-hadr-for-sql-server-on-azure-virtual-machines"></a>Continuidad empresarial y HADR para SQL Server en Azure Virtual Machines
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

La continuidad empresarial implica continuar el negocio en caso de desastre, planear la recuperación y asegurarse de que los datos tienen una alta disponibilidad. SQL Server en Azure Virtual Machines puede ayudar a disminuir el costo de una solución de base de datos de alta disponibilidad y recuperación ante desastres (HADR). 

La mayoría de las soluciones HADR de SQL Server son compatibles con las máquinas virtuales, bien como soluciones exclusivas de Azure o como soluciones híbridas. En una solución exclusiva de Azure, todo el sistema HADR se ejecuta en Azure. En una configuración híbrida, una parte de la solución se ejecuta en Azure y la otra parte se ejecuta localmente en su organización. La flexibilidad del entorno Azure permite migrar total o parcialmente a Azure a fin de satisfacer los requisitos de presupuesto y HADR de sus sistemas de bases de datos de SQL Server.

En este artículo se comparan y contrastan las soluciones de continuidad empresarial que hay disponibles para SQL Server en máquinas virtuales de Azure. 

## <a name="overview"></a>Información general

Depende de usted el garantizar que su sistema de base de datos tiene las funcionalidades de HADR que exige el contrato de nivel de servicio (SLA). El hecho de que Azure proporcione mecanismos de alta disponibilidad, como la recuperación del servicio en los servicios en la nube y la detección de recuperación de errores para las máquinas virtuales, no garantiza por sí solo que pueda cumplir con el contrato de nivel de servicio. Si bien estos mecanismos ayudan a proteger la alta disponibilidad de la máquina virtual, no protegen la disponibilidad de la instancia de SQL Server que se ejecuta dentro de la máquina virtual. 

Es posible que la instancia de SQL Server no funcione a pesar de que la máquina virtual esté en línea y en buen estado. Incluso con los mecanismos de alta disponibilidad que proporciona Azure, es posible que se produzcan tiempos de inactividad de las máquinas virtuales debidos a eventos como la recuperación errores de software o hardware o las actualizaciones del sistema operativo.

El almacenamiento con redundancia geográfica (GRS) de Azure se implementa con una característica denominada replicación geográfica. Es posible que GRS no sea una solución de recuperación ante desastres adecuada para sus bases de datos. Dado que la replicación geográfica envía los datos de forma asincrónica, las actualizaciones recientes se pueden perder en caso de desastre. Encontrará más información sobre las limitaciones de la replicación geográfica en la sección [Compatibilidad de la replicación geográfica](#geo-replication-support).

> [!NOTE]
> Ahora es posible migrar mediante lift and shift la solución de [instancia de clúster de conmutación por error](../../migration-guides/virtual-machines/sql-server-failover-cluster-instance-to-sql-on-azure-vm.md) y [grupo de disponibilidad](../../migration-guides/virtual-machines/sql-server-availability-group-to-sql-on-azure-vm.md) a SQL Server en máquinas virtuales de Azure mediante Azure Migrate. 


## <a name="deployment-architectures"></a>Arquitecturas de implementación
Azure admite estas tecnologías de SQL Server para la continuidad empresarial:

* [Grupos de disponibilidad Always On](/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server)
* [Instancias de clúster de conmutación por error (FCI) de Always On](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)
* [Trasvase de registros](/sql/database-engine/log-shipping/about-log-shipping-sql-server)
* [Copia de seguridad y restauración de SQL Server con Azure Blob Storage](/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service)
* [Creación de reflejo de la base de datos](/sql/database-engine/database-mirroring/database-mirroring-sql-server): en desuso en SQL Server 2016
* [Azure Site Recovery](../../../site-recovery/site-recovery-sql.md)

Es posible combinar las tecnologías para implementar una solución SQL Server que cuente con funcionalidades de alta disponibilidad y de recuperación ante desastres. Según la tecnología que se use, una implementación híbrida puede requerir un túnel VPN con la red virtual de Azure. En las secciones siguientes se muestran algunas arquitecturas de implementación de ejemplo.

## <a name="azure-only-high-availability-solutions"></a>Solo Azure: soluciones de alta disponibilidad

Puede tener una solución de alta disponibilidad para SQL Server en un nivel de base de datos con grupos de disponibilidad Always On. También puede crear una solución de alta disponibilidad en un nivel de instancia con instancias de clúster de conmutación por error Always On. Si quiere obtener protección adicional, puede crear redundancia en ambos niveles creando de grupos de disponibilidad en instancias de clúster de conmutación por error. 

| Technology | Arquitecturas de ejemplo |
| --- | --- |
| **Grupos de disponibilidad** |Todas las réplicas de disponibilidad que se ejecutan en Azure Virtual Machines de la misma región proporcionan alta disponibilidad. Debe configurar una máquina virtual de controlador de dominio, ya que los clústeres de conmutación por error de Windows requieren un dominio de Active Directory.<br/><br/> Para una mayor redundancia y disponibilidad, las máquinas virtuales de Azure se pueden implementar en [zonas de disponibilidad](../../../availability-zones/az-overview.md) distintas, tal como se documenta en la [información general sobre los grupos de disponibilidad](availability-group-overview.md). Si las VM con SQL Server de un grupo de disponibilidad se implementan en zonas de disponibilidad, use [Azure Standard Load Balancer](../../../load-balancer/load-balancer-overview.md) para el cliente de escucha, tal como se documenta en los artículos [CLI de la máquina virtual de Azure SQL](./availability-group-az-commandline-configure.md) y [Plantillas de inicio rápido de Azure](availability-group-quickstart-template-configure.md).<br/> ![Diagrama que muestra el "controlador de dominio" encima del "clúster de WSFC", formado por la "réplica principal", la "réplica secundaria" y el "testigo del recurso compartido de archivos".](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/azure-only-ha-always-on.png)<br/>Para más información, vea [Configuración de grupos de disponibilidad en Azure (GUI)](./availability-group-quickstart-template-configure.md). |
| **Instancias de clúster de conmutación por error** |Las instancias de clúster de conmutación por error son compatibles con VM con SQL Server. Como la característica de FCI requiere almacenamiento compartido, son cinco las soluciones que funcionarán con SQL Server en VM de Azure: <br/><br/> - El uso de [discos compartidos de Azure](failover-cluster-instance-azure-shared-disks-manually-configure.md) para Windows Server 2019. Los discos administrados compartidos son un producto de Azure que permite conectar un disco administrado a varias máquinas virtuales de manera simultánea. Las VM del clúster pueden leer o escribir en el disco adjunto en función de la reserva elegida por la aplicación en clúster mediante las reservas persistentes de SCSI (SCSI PR). SCSI PR es una solución de almacenamiento estándar del sector que usan las aplicaciones que se ejecutan en el entorno local de una red de área de almacenamiento (SAN). La habilitación de SCSI PR en un disco administrado le permite migrar estas aplicaciones a Azure tal cual. <br/><br/>- El uso de [Espacios de almacenamiento directo \(S2D\)](failover-cluster-instance-storage-spaces-direct-manually-configure.md) para proporcionar una SAN virtual basada en software para Windows Server 2016 y versiones posteriores.<br/><br/>- El uso de un [recurso compartido de archivos Premium](failover-cluster-instance-premium-file-share-manually-configure.md) para Windows Server 2012 y versiones posteriores. Los recursos compartidos de archivos Premium están respaldados por SSD, tienen una latencia constantemente baja y son completamente compatibles para usarlos con FCI.<br/><br/>- El uso de almacenamiento compatible con una solución de asociado para la agrupación en clústeres. Si quiere ver un ejemplo específico de uso de SIOS DataKeeper, consulte la entrada de blog sobre los [clústeres de conmutación por error y SIOS DataKeeper](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).<br/><br/>- El uso de almacenamiento en bloque compartido para un destino iSCSI remoto a través de Azure ExpressRoute. Por ejemplo, NetApp Private Storage (NPS) expone un destino iSCSI a través de ExpressRoute con Equinix a las máquinas virtuales de Azure.<br/><br/>Para las soluciones de almacenamiento compartido y de replicación de datos de asociados de Microsoft, póngase en contacto con el proveedor en caso de que surja cualquier problema relacionado con el acceso a datos en la conmutación por error.<br/><br/>|

## <a name="azure-only-disaster-recovery-solutions"></a>Solo Azure: Soluciones de recuperación ante desastres
Puede tener una solución de recuperación ante desastres para las bases de datos de SQL Server en Azure mediante el uso de grupos de disponibilidad, creación de reflejo de bases de datos o copias de seguridad y restauración con blobs de almacenamiento.

| Technology | Arquitecturas de ejemplo |
| --- | --- |
| **Grupos de disponibilidad** |Réplicas de disponibilidad que se ejecutan en varios centros de datos en máquinas virtuales de Azure para la recuperación ante desastres. Esta solución entre regiones ayuda a protegerse frente a interrupciones en todo el sitio. <br/> ![Diagrama que muestra dos regiones con una "réplica principal" y una "réplica secundaria" conectadas mediante una "confirmación asincrónica".](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/azure-only-dr-alwayson.png)<br/>Dentro de una región, todas las réplicas deben estar en el mismo servicio en la nube y la misma red virtual. Como cada región tendrá una red virtual independiente, estas soluciones requieren conectividad de red a red. Para más información, consulte [Configuración de una conexión de red a red mediante Azure Portal](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md). Para instrucciones detalladas, consulte [Configuración de un grupo de disponibilidad Always On de SQL Server en distintas regiones de Azure](availability-group-manually-configure-multiple-regions.md).|
| **Creación de reflejo de la base de datos** |Los servidores principal y de reflejo se ejecutan en distintos centros de datos para la recuperación ante desastres. Debe implementarlos con certificados de servidor. No se admite la creación de reflejo de la base de datos de SQL Server para SQL Server 2008 ni para SQL Server 2008 R2 en una máquina virtual de Azure. <br/>![Diagrama que muestra la "entidad de seguridad" en una región conectada al "reflejo en otra región con 'alto rendimiento'".](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/azure-only-dr-dbmirroring.png) |
| **Copia de seguridad y restauración con Azure Blob Storage** |Las bases de datos de producción de las que se realizó una copia de seguridad directamente en Blob Storage en otro centro de datos para la recuperación ante desastres.<br/>![Diagrama que muestra una "base de datos" en una región que realiza una copia de seguridad en "Blob Storage" en otra región.](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/azure-only-dr-backup-restore.png)<br/>Para más información, consulte [Copia de seguridad y restauración de SQL Server en VM de Azure](../../../azure-sql/virtual-machines/windows/backup-restore.md). |
| **Replicación y conmutación por error de SQL Server en Azure con Azure Site Recovery** |La instancia de SQL Server de producción de un centro de datos de Azure replicado directamente en una instancia de Azure Storage de un centro de datos de Azure distinto para la recuperación ante desastres.<br/>![Diagrama que muestra una "base de datos" en un centro de datos de Azure con "replicación de ASR" para la recuperación ante desastres en otro centro de datos. ](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/azure-only-dr-standalone-sqlserver-asr.png)<br/>Para obtener más información, consulte [Proteger SQL Server con la recuperación ante desastres de SQL Server y Azure Site Recovery](../../../site-recovery/site-recovery-sql.md). |


## <a name="hybrid-it-disaster-recovery-solutions"></a>TI híbrida: Soluciones de recuperación ante desastres
Puede tener una solución de recuperación ante desastres para las bases de datos de SQL Server en un entorno de TI híbrida mediante grupos de disponibilidad, creación de reflejo de base de datos, trasvase de registros y copia de seguridad y restauración con Azure Blob Storage.

| Technology | Arquitecturas de ejemplo |
| --- | --- |
| **Grupos de disponibilidad** |Algunas réplicas de disponibilidad se ejecutan en máquinas virtuales de Azure y réplicas se ejecutan localmente para la recuperación ante desastres a través de sitios. El sitio de producción puede ser local o encontrarse en un centro de datos de Azure.<br/>![Grupos de disponibilidad](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/hybrid-dr-alwayson.png)<br/>Dado que todas las réplicas de disponibilidad deben estar en el mismo clúster de conmutación por error, este debe abarcar ambas redes (un clúster de conmutación por error de varias subredes). Esta configuración requiere una conexión VPN entre Azure y la red local.<br/><br/>Para que se produzca una recuperación ante desastres correcta de las bases de datos, también debe instalar un controlador de dominio de réplica en el sitio de recuperación ante desastres.|
| **Creación de reflejo de la base de datos** |Un asociado se ejecuta en una máquina virtual de Azure y otro en el entorno local para la recuperación ante desastres entre sitios mediante el uso de certificados de servidor. Los asociados no necesitan estar en el mismo dominio de Active Directory y no se requiere ninguna conexión VPN.<br/>![Creación de reflejo de la base de datos](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/hybrid-dr-dbmirroring.png)<br/>Otro escenario de creación de reflejo de la base de datos implica a un asociado que se ejecuta en una máquina virtual de Azure y los demás que se ejecutan localmente en el mismo dominio de Active Directory para la recuperación ante desastres entre sitios. Se requiere una [conexión VPN entre la red virtual de Azure y la red local](../../../vpn-gateway/tutorial-site-to-site-portal.md).<br/><br/>Para que se produzca una recuperación ante desastres correcta de las bases de datos, también debe instalar un controlador de dominio de réplica en el sitio de recuperación ante desastres. No se admite la creación de reflejo de la base de datos de SQL Server para SQL Server 2008 ni para SQL Server 2008 R2 en una máquina virtual de Azure. |
| **Trasvase de registros** |Un servidor se ejecuta en una máquina virtual de Azure y  otro localmente para la recuperación ante desastres a través de sitios. El trasvase de registros depende del uso compartido de archivos de Windows, de modo que se requiere una conexión VPN entre la red virtual de Azure y la red local.<br/>![Trasvase de registros](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/hybrid-dr-log-shipping.png)<br/>Para que se produzca una recuperación ante desastres correcta de las bases de datos, también debe instalar un controlador de dominio de réplica en el sitio de recuperación ante desastres. |
| **Copia de seguridad y restauración con Azure Blob Storage** |Bases de datos de producción locales con copia de seguridad directa en Azure Blob Storage para la recuperación ante desastres.<br/>![Copia de seguridad y restauración](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/hybrid-dr-backup-restore.png)<br/>Para más información, consulte [Copia de seguridad y restauración de SQL Server en Azure Virtual Machines](../../../azure-sql/virtual-machines/windows/backup-restore.md). |
| **Replicación y conmutación por error de SQL Server en Azure con Azure Site Recovery** |Instancia de SQL Server de producción local replicada directamente en una instancia de Azure Storage para la recuperación ante desastres.<br/>![Replicación con Azure Site Recovery](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/hybrid-dr-standalone-sqlserver-asr.png)<br/>Para obtener más información, consulte [Proteger SQL Server con la recuperación ante desastres de SQL Server y Azure Site Recovery](../../../site-recovery/site-recovery-sql.md). |


## <a name="free-dr-replica-in-azure"></a>Réplica de DR gratuita en Azure

Si tiene [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot:primaryr3), puede implementar planes de recuperación ante desastres (DR) híbridos con SQL Server sin incurrir en costos de licencias adicionales para la instancia de recuperación ante desastres pasiva.

Por ejemplo, puede tener dos instancias secundarias pasivas gratis si las tres réplicas se hospedan en Azure: 

![Dos instancias pasivas gratis si todo está en Azure](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/failover-with-primary-in-azure.png)

O bien, puede configurar un entorno de conmutación por error híbrido, con una instancia principal con licencia, una instancia pasiva para alta disponibilidad, una instancia pasiva gratis para la recuperación ante desastres local y otra instancia pasiva gratis para la recuperación ante desastres en Azure:

![Tres instancias pasivas gratis si el entorno es híbrido con una réplica principal local](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/hybrid-with-primary-on-prem.png)

Para más información, consulte los [términos de licencia del producto](https://www.microsoft.com/licensing/product-licensing/products). 

Para habilitar esta ventaja, vaya al [recurso de máquina virtual de SQL Server](manage-sql-vm-portal.md#access-the-resource). Seleccione **Configurar** en **Configuración** y elija la opción **Recuperación ante desastres** en **Licencia de SQL Server**. Active la casilla para confirmar que esta VM con SQL Server se usará como una réplica pasiva y, a continuación, seleccione **Aplicar** para guardar la configuración. 

![Configuración de una réplica de recuperación ante desastres en Azure](./media/business-continuity-high-availability-disaster-recovery-hadr-overview/dr-replica-in-portal.png)


## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Consideraciones importantes para HADR de SQL Server en Azure
Las máquinas virtuales de Azure, el almacenamiento y la conexión de red tienen características operativas diferentes de las de una infraestructura TI local y no virtualizada. Para implementar correctamente una solución HADR de SQL Server en Azure es necesario conocer estas diferencias y diseñar la solución adaptada a ellas.

### <a name="high-availability-nodes-in-an-availability-set"></a>Nodos de alta disponibilidad en un conjunto de disponibilidad
Los conjuntos de disponibilidad de Azure permiten colocar los nodos de alta disponibilidad en dominios de error y dominios de actualización independientes. La plataforma Azure asigna un dominio de actualización y un dominio de error a cada máquina virtual del conjunto de disponibilidad. Esta configuración en un centro de datos garantiza que, durante un evento de mantenimiento planeado o no planeado, hay al menos una máquina virtual disponible y cumple el contrato de nivel de servicio de Azure al 99,95 %. 

Para configurar una alta disponibilidad, coloque todas las máquinas virtuales con SQL Server en el mismo conjunto de disponibilidad para evitar la pérdida de datos o de la aplicación durante un evento de mantenimiento. Tenga en cuenta que solo las máquinas virtuales del mismo servicio en la nube puede participar en el mismo conjunto de disponibilidad. Para más información, consulte [Administración de la disponibilidad de las máquinas virtuales](../../../virtual-machines/availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="high-availability-nodes-in-an-availability-zone"></a>Nodos de alta disponibilidad en una zona de disponibilidad
Las zonas de disponibilidad son ubicaciones físicas exclusivas dentro de una región de Azure. Cada zona de disponibilidad consta de uno o varios centros de datos equipados con alimentación, refrigeración y redes independientes. La separación física de las zonas de disponibilidad dentro de una región ayuda a proteger las aplicaciones y los datos frente a los errores del centro de datos al garantizar que hay al menos una máquina virtual disponible y cumple el contrato de nivel de servicio de Azure al 99,99 %. 

Para configurar la alta disponibilidad, coloque las máquinas virtuales con SQL Server participantes dispersas por las zonas de disponibilidad de la región. Habrá cargos adicionales por las transferencias de red a red entre las zonas de disponibilidad. Para más información, consulte [Zonas de disponibilidad](../../../availability-zones/az-overview.md). 

### <a name="network-latency-in-hybrid-it"></a>Latencia de red en TI híbrida
Implemente la solución HADR partiendo de la suposición de que podría haber períodos de tiempo con una alta latencia de red entre la red local y Azure. Al implementar réplicas en Azure, use la confirmación asincrónica en lugar de la confirmación sincrónica para el modo de sincronización. Al implementar servidores de creación de reflejo de la base de datos tanto en el entorno local como en Azure, use el modo de alto rendimiento en lugar del modo de alta seguridad.

Consulte los [procedimientos recomendados para la configuración de alta disponibilidad y recuperación ante desastres](hadr-cluster-best-practices.md) para ver la configuración del clúster y de HADR que pueden ayudar a adaptarse al entorno en la nube. 

### <a name="geo-replication-support"></a>Compatibilidad de la replicación geográfica
La replicación geográfica en discos de Azure no admite que el archivo de datos y el archivo de registro de la misma base de datos se almacenen en discos independientes. La GRS replica los cambios en cada disco independiente y asincrónicamente. Este mecanismo garantiza el orden de escritura en un único disco en la copia con replicación geográfica pero no a través de las copias con replicación geográfica de varios discos. Si configura una base de datos para almacenar su archivo de datos y su archivo de registro en discos independientes, los discos recuperados después de un desastre pueden contener una copia más actualizada del archivo de datos que el archivo de registro, lo que interrumpe el registro de escritura previa en SQL Server y las propiedades ACID (atomicidad, coherencia, aislamiento y durabilidad) de las transacciones. 

Si no tiene la opción de deshabilitar la replicación geográfica en la cuenta de almacenamiento, conserve todos los archivos de registro y datos de una base de datos en el mismo disco. Si debe usar más de un disco debido al tamaño de la base de datos, implemente una de las soluciones de recuperación de desastres enumeradas anteriormente para garantizar la redundancia de datos.

## <a name="next-steps"></a>Pasos siguientes

Decida si la mejor solución de continuidad empresarial para su negocio es un [grupo de disponibilidad](availability-group-overview.md) o una [instancia de clúster de conmutación por error](failover-cluster-instance-overview.md). Luego, revise los [procedimientos recomendados](hadr-cluster-best-practices.md) para configurar el entorno para obtener alta disponibilidad y recuperación ante desastres.
