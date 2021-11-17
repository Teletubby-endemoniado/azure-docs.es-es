---
title: Novedades de Azure Backup
description: Obtenga información acerca de las nuevas características de Azure Backup.
ms.topic: conceptual
ms.date: 10/20/2021
ms.openlocfilehash: 78d6b8cee1ad2442278497c5ca3e282b19d1beb6
ms.sourcegitcommit: 27ddccfa351f574431fb4775e5cd486eb21080e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "131997255"
---
# <a name="whats-new-in-azure-backup"></a>Novedades de Azure Backup

Azure Backup constantemente recibe mejoras y nuevas características que mejoran la protección de los datos en Azure. Estas nuevas características amplían la protección de datos a nuevos tipos de cargas de trabajo, mejoran la seguridad y también la disponibilidad de los datos de copia de seguridad. Además, agregan nuevas funcionalidades de administración, supervisión y automatización.

Para obtener más información acerca de las nuevas versiones, puede marcar esta página o [suscribirse a las actualizaciones con este vínculo](https://azure.microsoft.com/updates/?query=backup).

## <a name="updates-summary"></a>Resumen de actualizaciones

- Octubre de 2021
  - [Varias copias de seguridad al día para Azure Files (en versión preliminar)](#multiple-backups-per-day-for-azure-files-in-preview)
  - [Métricas y alertas de métricas de Azure Backup (versión preliminar)](#azure-backup-metrics-and-metrics-alerts-in-preview)
- Julio de 2021
  - [La compatibilidad con el nivel de acceso de archivo de SQL Server en máquinas virtuales de Azure para Azure Backup ya está disponible con carácter general](#archive-tier-support-for-sql-server-in-azure-vm-for-azure-backup-is-now-generally-available)
- Mayo de 2021
  - [La copia de seguridad de blobs de Azure ya está disponible con carácter general](#backup-for-azure-blobs-is-now-generally-available)
- Abril de 2021
  - [Mejoras en el cifrado mediante claves administradas por el cliente para Azure Backup (en versión preliminar)](#enhancements-to-encryption-using-customer-managed-keys-for-azure-backup-in-preview)
- Marzo de 2021
  - [Azure Disk Backup ya está disponible con carácter general](#azure-disk-backup-is-now-generally-available).
  - [El centro de Backup ya está disponible de forma general](#backup-center-is-now-generally-available).
  - [Compatibilidad del nivel de acceso de archivo para Azure Backup (en versión preliminar)](#archive-tier-support-for-azure-backup-in-preview)
- Febrero de 2021
  - [Copia de seguridad de blobs de Azure (en versión preliminar)](#backup-for-azure-blobs-in-preview)
- Enero de 2021
  - [Azure Disk Backup (en versión preliminar)](#azure-disk-backup-in-preview)
  - [Cifrado en reposo mediante claves administradas por el cliente ahora (disponible con carácter general)](#encryption-at-rest-using-customer-managed-keys)
- Noviembre de 2020
  - [Plantilla de Azure Resource Manager para la copia de seguridad de recursos compartido de archivos de Azure (AFS)](#azure-resource-manager-template-for-afs-backup)
  - [Copias de seguridad incrementales para bases de datos de SAP HANA en máquinas virtuales de Azure (en versión preliminar)](#incremental-backups-for-sap-hana-databases-in-preview)
- Septiembre de 2020
  - [Centro de copia de seguridad (en versión preliminar)](#backup-center-in-preview)
  - [Copia de seguridad de Azure Database for PostgreSQL (en versión preliminar)](#back-up-azure-database-for-postgresql-in-preview)
  - [Copia de seguridad y restauración selectivas de discos](#selective-disk-backup-and-restore)
  - [Restauración entre regiones de bases de datos SQL Server y SAP HANA en máquinas virtuales de Azure ( en versión preliminar)](#cross-region-restore-for-sql-server-and-sap-hana-in-preview)
  - [Compatibilidad de la copia de seguridad de máquinas virtuales con hasta 32 discos (disponibilidad general)](#support-for-backup-of-vms-with-up-to-32-disks)
  - [Experiencia de configuración de copia de seguridad simplificada para SQL en las máquinas virtuales de Azure](#simpler-backup-configuration-for-sql-in-azure-vms)
  - [Copia de seguridad de SAP HANA en Azure Virtual Machines para Red Hat Enterprise Linux (en versión preliminar)](#back-up-sap-hana-in-rhel-azure-virtual-machines-in-preview)
  - [Almacenamiento con redundancia de zona para datos de copia de seguridad (en versión preliminar)](#zone-redundant-storage-zrs-for-backup-data-in-preview)
  - [Eliminación temporal de las cargas de trabajo de SQL Server y SAP HANA en máquinas virtuales de Azure](#soft-delete-for-sql-server-and-sap-hana-workloads)

## <a name="multiple-backups-per-day-for-azure-files-in-preview"></a>Varias copias de seguridad al día para Azure Files (en versión preliminar)

Un objetivo de punto de recuperación bajo es un requisito clave para Azure Files que contiene los datos críticos para la empresa que se actualizan con frecuencia. Para garantizar una pérdida mínima de datos en caso de que se produzca algún desastre o cambios no deseados en el contenido del recurso compartido de archivos, es posible que prefiera realizar copias de seguridad con una periodicidad superior a una vez al día.

Mediante Azure Backup no solo se pueden crear directivas de copia de seguridad, sino también modificar las directivas de copia de seguridad existentes para tomar varias instantáneas al día. Con esta funcionalidad, también se puede definir la duración en que se desencadenarían los trabajos de copia de seguridad. Esta funcionalidad le permite alinear la programación de copia de seguridad con las horas de trabajo cuando hay actualizaciones frecuentes en el contenido de Azure Files.

Para más información, consulte el artículo en que se indica [cómo configurar varias copias de seguridad al día mediante la directiva de copia de seguridad](/azure/backup/manage-afs-backup#create-a-new-policy).

## <a name="azure-backup-metrics-and-metrics-alerts-in-preview"></a>Métricas y alertas de métricas de Azure Backup (versión preliminar)

Azure Backup ahora ofrece un conjunto de métricas integradas a través de [Azure Monitor](/azure/azure-monitor/essentials/data-platform-metrics) que le permiten supervisar el estado de sus copias de seguridad. También puede configurar reglas de alertas que desencadenan alertas cuando las métricas superan los umbrales definidos.

Azure Backup ofrece estas funcionalidades clave:
 
- Capacidad de ver métricas de fábrica relacionadas con el estado de las copias de seguridad y restauraciones de los elementos de copia de seguridad, junto con las tendencias asociadas.
- Capacidad de escribir reglas de alertas personalizadas en estas métricas para supervisar de forma eficaz el estado de los elementos de copia de seguridad.
- Capacidad de enrutar las alertas de métricas activadas por varios canales de notificación compatibles con Azure Monitor, como correo electrónico, ITSM, webhook, aplicaciones lógicas, entre otros.
 
Actualmente, Azure Backup admite métricas integradas para estos tipos de cargas de trabajo:

- Azure VM
- Bases de datos SQL en VM de Azure
- Bases de datos de SAP HANA en VM de Azure
- Azure Files

Para más detalles, consulte [Supervisión del estado de las copias de seguridad mediante las métricas de Azure Backup (versión preliminar)](metrics-overview.md).

## <a name="archive-tier-support-for-sql-server-in-azure-vm-for-azure-backup-is-now-generally-available"></a>La compatibilidad con el nivel de acceso de archivo de SQL Server en máquinas virtuales de Azure para Azure Backup ya está disponible con carácter general

Azure Backup permite mover los puntos de retención a largo plazo para Azure Virtual Machines y SQL Server en Azure Virtual Machines al nivel de acceso de archivo de bajo costo. También puede restaurar desde los puntos de recuperación en el nivel de acceso de archivo del almacén.

Además de la funcionalidad de mover los puntos de recuperación:

- Azure Backup proporciona recomendaciones para mover un conjunto específico de puntos de recuperación para copias de seguridad de Azure Virtual Machines que garantizarán un ahorro de costos.
- Tiene la capacidad de mover todos sus puntos de recuperación a un elemento de copia de seguridad determinado de una vez mediante scripts de ejemplo.
- Puede ver el uso del almacenamiento de archivo en el panel del almacén.

Para obtener más información, consulte [Compatibilidad del nivel de acceso de archivo (versión preliminar)](./archive-tier-support.md).

## <a name="backup-for-azure-blobs-is-now-generally-available"></a>La copia de seguridad de blobs de Azure ya está disponible con carácter general

La copia de seguridad operativa de blobs de Azure es una solución de protección de datos local administrada que permite proteger los datos de blobs en bloques contra la pérdida de datos en diversos escenarios, como daños, eliminación de blobs o eliminación accidental de cuentas de almacenamiento.

Al ser una solución de copia de seguridad operativa, los datos de copia de seguridad se almacenan localmente en la cuenta de almacenamiento de origen y se pueden recuperar a partir de un momento dado seleccionado, lo que proporciona un medio sencillo y rentable de proteger los datos de blobs. Para conseguir esto, la solución usa la funcionalidad de restauración a un momento dado de blobs disponible en Blob Storage.

La copia de seguridad operativa de blobs se integra con las herramientas de administración de Azure Backup, incluido el Centro de copia de seguridad, para ayudarle a administrar la protección de los datos de blob de forma eficaz y a gran escala. Además de las funcionalidades disponibles anteriormente, ahora puede configurar y administrar la copia de seguridad operativa de blobs mediante la vista **Protección de datos** de las cuentas de almacenamiento, también [mediante PowerShell](backup-blobs-storage-account-ps.md). Además, el servicio Backup ahora ofrece una experiencia mejorada para administrar las asignaciones de roles necesarias para configurar la copia de seguridad operativa.

Para más información, consulte [Introducción a la copia de seguridad operativa de blobs de Azure](blob-backup-overview.md).

## <a name="azure-disk-backup-is-now-generally-available"></a>Azure Disk Backup ya está disponible con carácter general.

Azure Backup ofrece administración del ciclo de vida de las instantáneas a Azure Managed Disks, ya que automatiza la creación periódica de instantáneas y las conserva durante el tiempo configurado mediante la directiva de copia de seguridad.

Para obtener más información, consulte [Información general sobre Azure Disk Backup](disk-backup-overview.md).

## <a name="backup-center-is-now-generally-available"></a>El centro de Backup ya está disponible de forma general.

El centro de Backup simplifica la administración de la protección de datos a gran escala, ya que le permite consultar, regular, supervisar, dirigir y optimizar la administración de copias de seguridad desde una única consola central.

Para obtener más información, consulte [Información general sobre el centro de copias de seguridad](backup-center-overview.md).

## <a name="archive-tier-support-for-azure-backup-in-preview"></a>Compatibilidad del nivel de acceso de archivo para Azure Backup (en versión preliminar)

Azure Backup ahora le permite reducir el costo de las copias de seguridad de retención a largo plazo con la disponibilidad del nivel de archivo para Azure Virtual Machines y los servidores de SQL Server en Azure Virtual Machines.

Para obtener más información, consulte [Compatibilidad del nivel de acceso de archivo (versión preliminar)](archive-tier-support.md).

## <a name="backup-for-azure-blobs-in-preview"></a>Copia de seguridad de blobs de Azure (en versión preliminar)

La copia de seguridad operativa de blobs es una solución de protección de datos local administrada que le permite proteger los blobs en bloques de diversos escenarios en los que se hayan perdido datos; por ejemplo, si se producen daños, si se realizan eliminaciones de blobs y la eliminación accidental de cuentas de almacenamiento. Los datos se almacenan localmente dentro de la propia cuenta de almacenamiento de origen y se pueden recuperar a un momento determinado en el tiempo cuando sea necesario. Por tanto, proporciona un medio sencillo, seguro y rentable para proteger sus blobs.

La copia de seguridad operativa de blobs se integra con el Centro de copias de seguridad, entre otras capacidades de administración de Backup, para proporcionar un solo panel que pueda ayudarle a controlar, supervisar, usar y analizar las copias de seguridad a escala.

Para obtener más in, consulte [Información general de la copia de seguridad operativa de Azure Blobs (en versión preliminar)](blob-backup-overview.md).

## <a name="azure-disk-backup-in-preview"></a>Azure Disk Backup (en versión preliminar)

Azure Disk Backup ofrece una solución inmediata que le proporciona la oportunidad de administrar el ciclo de vida de las instantáneas para [Azure Managed Disks](../virtual-machines/managed-disks-overview.md); para ello, solo debe automatizar la creación periódica de instantáneas y guardarlas durante la duración configurada mediante la directiva de copia de seguridad. Puede administrar las instantáneas de disco sin costos de infraestructura, sin necesidad de realizar un scripting personalizado y sin sufrir ninguna sobrecarga de administración. Esta solución de copia de seguridad coherente con los bloqueos realiza en un momento dado una copia de seguridad de un disco administrado mediante [instantáneas incrementales](../virtual-machines/disks-incremental-snapshots.md), y admite la realización de varias copias de seguridad al día. También es una solución sin agente que no afecta al rendimiento de las aplicaciones de producción. Igualmente, admite las copias de seguridad y la restauración de los discos de datos y del sistema operativo (incluidos los discos compartidos), tanto si están conectados actualmente a una máquina virtual de Azure en ejecución como si no.

Para más información, consulte [Azure Disk Backup (en versión preliminar)](disk-backup-overview.md).

## <a name="encryption-at-rest-using-customer-managed-keys"></a>Cifrado en reposo con claves administradas por el cliente

La compatibilidad con el cifrado en reposo mediante claves administradas por el cliente ahora está disponible con carácter general. Esto le ofrece la capacidad de cifrar los datos de copia de seguridad en los almacenes de Recovery Services con sus propias claves almacenadas en Azure Key Vault. La clave de cifrado utilizada para cifrar las copias de seguridad en el almacén de Recovery Services puede ser diferente de las que se usan para cifrar el origen. Los datos se protegen mediante una clave de cifrado de datos (DEK) basada en AES 256, que, a su vez, está protegida con las claves del usuario almacenadas en Key Vault. En comparación con el cifrado mediante claves administradas por la plataforma (que está disponible de manera predeterminada), esto le proporciona más control sobre las claves y puede ayudarlo a satisfacer mejor sus necesidades de cumplimiento.

Para más información, consulte [Cifrado de datos de copia de seguridad mediante claves administradas por el cliente](encryption-at-rest-with-cmk.md).

## <a name="azure-resource-manager-template-for-afs-backup"></a>Plantilla de Azure Resource Manager para la copia de seguridad de AFS

Azure Backup permite ahora configurar la copia de seguridad de recursos compartidos de archivos de Azure existentes mediante una plantilla de Azure Resource Manager (ARM). La plantilla configura la protección para un recurso compartido de archivos de Azure existente al especificar los detalles correspondientes para la directiva de copia de seguridad y el almacén de Recovery Services. Opcionalmente, crea una directiva de copia de seguridad y un almacén de Recovery Services nuevos, y registra la cuenta de almacenamiento que contiene el recurso compartido de archivos en el almacén de Recovery Services.

Para más información, consulte [Plantillas de Azure Resource Manager para Azure Backup](backup-rm-template-samples.md).

## <a name="incremental-backups-for-sap-hana-databases-in-preview"></a>Copias de seguridad incrementales para bases de datos SAP HANA (en versión preliminar)

Azure Backup ahora admite las copias de seguridad incrementales para las bases de datos SAP HANA hospedadas en máquinas virtuales de Azure. Esto permite realizar copias de seguridad más rápidas y rentables de sus datos de SAP HANA.

Para obtener más información, consulte las [distintas opciones disponibles durante la creación de una directiva de copia de seguridad](./sap-hana-faq-backup-azure-vm.yml) y [cómo crear una directiva de copia de seguridad para las bases de datos SAP HANA](tutorial-backup-sap-hana-db.md#creating-a-backup-policy).

## <a name="backup-center-in-preview"></a>Centro de copia de seguridad (en versión preliminar)

Azure Backup ha habilitado una nueva funcionalidad de administración nativa para todo el estado de la copia de seguridad desde una consola central. El centro de copias de seguridad proporciona la capacidad de supervisar, operar, administrar y optimizar la protección de datos a escala de manera unificada con las experiencias de administración nativas de Azure.

Con el centro de copias de seguridad, obtiene una vista agregada del inventario entre suscripciones, ubicaciones, grupos de recursos, almacenes e incluso inquilinos con Azure Lighthouse. Además, dicho centro es también un centro de actividades desde el que puede desencadenar actividades relacionadas con la copia de seguridad, como la configuración y restauración de copias de seguridad y la creación de directivas o almacenes, todo desde un único lugar. Asimismo, con la integración sin problemas con Azure Policy, ahora puede controlar el entorno y realizar un seguimiento del cumplimiento desde la perspectiva de una copia de seguridad. Las directivas integradas de Azure específicas para Azure Backup también permiten configurar las copias de seguridad a escala.

Para obtener más información, consulte [Información general sobre el centro de copias de seguridad](backup-center-overview.md).

## <a name="back-up-azure-database-for-postgresql-in-preview"></a>Copia de seguridad de Azure Database for PostgreSQL (en versión preliminar)

Azure Backup y los servicios de base de datos de Azure se han unido para crear una solución de copia de seguridad de clase empresarial para Azure PostgreSQL (actualmente en versión preliminar). Ahora puede satisfacer sus necesidades de protección de datos y cumplimiento con una directiva de copia de seguridad controlada por el cliente que permita la retención de copias de seguridad durante un máximo de 10 años. Con ella, tiene un control detallado para administrar las operaciones de creación y restauración de copias de seguridad en el nivel de base de datos individual. Del mismo modo, puede realizar restauraciones a través de las versiones de PostgreSQL o Blob Storage fácilmente.

Para más información, consulte el artículo [Copia de seguridad de Azure Database for PostgreSQL](backup-azure-database-postgresql.md).

## <a name="selective-disk-backup-and-restore"></a>Copia de seguridad y restauración selectivas de discos

Azure Backup admite la copia de seguridad de todos los discos (sistema operativo y datos) en una máquina virtual junto con la solución de copia de seguridad de máquinas virtuales. Ahora, con la funcionalidad de copia de seguridad y restauración selectivas de discos, puede realizar una copia de seguridad de un subconjunto de los discos de datos de una máquina virtual. Esto proporciona una solución eficaz y rentable para sus necesidades de copia de seguridad y restauración. Cada punto de recuperación contiene solo los discos que se incluyen en la operación de copia de seguridad.

Para obtener más información, consulte [Copias de seguridad y restauración selectivas de discos para máquinas virtuales de Azure](selective-disk-backup-restore.md).

## <a name="cross-region-restore-for-sql-server-and-sap-hana-in-preview"></a>Restauración entre regiones para SQL Server y SAP HANA (en versión preliminar)

Con la introducción de la restauración entre regiones, ahora puede iniciar las restauraciones en una región secundaria a fin de mitigar los problemas de tiempo de inactividad reales en una región primaria de su entorno. Esto hace que las restauraciones de la región secundaria las controle por completo el cliente. Azure Backup usa los datos de copia de seguridad replicados en la región secundaria para dichas restauraciones.

Ahora, además de admitir la restauración entre regiones para las máquinas virtuales de Azure, la característica se ha ampliado a la restauración de bases de datos SQL y SAP HANA también en máquinas virtuales de Azure.

Para obtener más información, consulte [Restauración entre regiones para bases de datos SQL](restore-sql-database-azure-vm.md#cross-region-restore) y [Restauración entre regiones para bases de datos SAP HANA](sap-hana-db-restore.md#cross-region-restore).

## <a name="support-for-backup-of-vms-with-up-to-32-disks"></a>Compatibilidad de la copia de seguridad de máquinas virtuales con hasta 32 discos

Hasta ahora, Azure Backup ha admitido 16 discos administrados por cada máquina virtual. Ahora, Azure Backup admite la copia de seguridad de hasta 32 de discos administrados por cada máquina virtual.

Para obtener más información, consulte la matriz de [compatibilidad con almacenamiento de máquina virtual](backup-support-matrix-iaas.md#vm-storage-support).

## <a name="simpler-backup-configuration-for-sql-in-azure-vms"></a>Configuración de copia de seguridad más sencilla para SQL en las máquinas virtuales de Azure

La configuración de las copias de seguridad para SQL Server en las máquinas virtuales de Azure es aún más fácil gracias a la configuración de copia de seguridad en línea integrada en el panel de la máquina virtual de Azure Portal. En unos pocos pasos, puede habilitar la copia de seguridad de SQL Server para proteger todas las bases de datos actuales, así como las que se agregarán en el futuro.

Para obtener más información, consulte [Copia de seguridad de una instancia de SQL Server desde el panel de máquina virtual](backup-sql-server-vm-from-vm-pane.md).

## <a name="back-up-sap-hana-in-rhel-azure-virtual-machines-in-preview"></a>Copia de seguridad de SAP HANA en Azure Virtual Machines para Red Hat Enterprise Linux (en versión preliminar)

Azure Backup es la solución de copias de seguridad nativa de Azure y tiene la certificación BackInt de SAP. Azure Backup ha agregado compatibilidad con Red Hat Enterprise Linux (RHEL), uno de los sistemas operativos Linux más usados que ejecutan SAP HANA.

Para obtener más información, consulte la matriz de [compatibilidad de escenarios para copia de seguridad de bases de datos SAP HANA](sap-hana-backup-support-matrix.md#scenario-support).

## <a name="zone-redundant-storage-zrs-for-backup-data-in-preview"></a>Almacenamiento con redundancia de zona para datos de copia de seguridad (en versión preliminar)

Azure Storage proporciona un equilibrio fantástico entre alto rendimiento, alta disponibilidad y alta resistencia de datos con sus variadas opciones de redundancia. Azure Backup le permite ampliar también estas ventajas a los datos de copias de seguridad, con opciones para almacenar las copias de seguridad en almacenamiento con redundancia local (LRS) y almacenamiento con redundancia geográfica (GRS). Ahora, hay opciones de durabilidad adicionales con la compatibilidad agregada para el almacenamiento con redundancia de zona (ZRS).

Para obtener más información, consulte [Establecimiento de la redundancia de almacenamiento para el almacén de Recovery Services](backup-create-rs-vault.md#set-storage-redundancy).

## <a name="soft-delete-for-sql-server-and-sap-hana-workloads"></a>Eliminación temporal de las cargas de trabajo de SQL Server y SAP HANA

Cada vez es mayor la preocupación que generan problemas de seguridad como malware, ransomware e intrusión. Estos problemas de seguridad pueden ser costosos, en términos de dinero y datos. Para protegerse contra dichos ataques, Azure Backup proporciona características de seguridad que protegen los datos de las copias de seguridad incluso después de su eliminación.

Una de estas características es la eliminación temporal. Con la eliminación temporal, aunque un actor malintencionado elimine una copia de seguridad (o se eliminen por accidente datos de copia de seguridad), los datos de copia de seguridad se conservan durante 14 días adicionales, lo que permite la recuperación de ese elemento de copia de seguridad sin pérdida de datos. La retención adicional de 14 días de los datos de copia de seguridad con el estado de "eliminación temporal" no tiene costo alguno para el cliente.

Ahora, además de la compatibilidad con la eliminación temporal para las máquinas virtuales de Azure, las cargas de trabajo de SQL Server y SAP HANA en las máquinas virtuales de Azure también están protegidas a través de la eliminación temporal.

Para obtener más información, consulte [Eliminación temporal de servidores SQL Server en máquinas virtuales de Azure e instancias de SAP HANA en cargas de trabajo de máquinas virtuales de Azure](soft-delete-sql-saphana-in-azure-vm.md).

## <a name="enhancements-to-encryption-using-customer-managed-keys-for-azure-backup-in-preview"></a>Mejoras en el cifrado mediante claves administradas por el cliente para Azure Backup (en versión preliminar)

Azure Backup ahora proporciona funcionalidades mejoradas (en versión preliminar) para administrar el cifrado con claves administradas por el cliente. Azure Backup permite traer sus propias claves para cifrar los datos de copia de seguridad en los almacenes de Recovery Services, lo que proporciona un mejor control.

- Admite identidades administradas asignadas por el usuario para conceder permisos a las claves para administrar el cifrado de datos en el almacén de Recovery Services.
- Habilita el cifrado con claves administradas por el cliente al crear un almacén de Recovery Services.
  >[!NOTE]
  >Esta característica está en versión preliminar limitada actualmente. Para registrarse, rellene [este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapURDNTVVhGOUxXSVBZMEwxUU5FNDkyQkU4Ny4u) y escríbanos a [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com).
- Permite usar directivas de Azure para auditar y aplicar el cifrado mediante claves administradas por el cliente.
>[!NOTE]
>- Las funcionalidades anteriores solo se admiten a través de Azure Portal; PowerShell no se admite actualmente.<br>Si usa PowerShell para administrar las claves de cifrado para la copia de seguridad, no se recomienda actualizar las claves desde el portal.<br>Si actualiza la clave desde el portal, no puede usar PowerShell para actualizar también la clave de cifrado hasta que esté disponible una actualización de PowerShell que admita el nuevo modelo. Sin embargo, puede seguir actualizando la clave desde Azure Portal.
>- La directiva de auditoría se puede usar para auditar almacenes con cifrado mediante claves administradas por el cliente habilitadas después del 01/04/2021.  
>- En el caso de los almacenes con el cifrado de CMK habilitado antes de esta fecha, pudiera ser que la directiva no se aplicara o que se mostraran falsos resultados negativos (es decir, estos almacenes se pueden presentar como no compatibles, a pesar de tener habilitado el cifrado de CMK). [Más información](encryption-at-rest-with-cmk.md#using-azure-policies-for-auditing-and-enforcing-encryption-utilizing-customer-managed-keys-in-preview).

Para más información, consulte [Cifrado para Azure Backup mediante claves administradas por el cliente](encryption-at-rest-with-cmk.md). 

## <a name="next-steps"></a>Pasos siguientes

- [Guía y procedimientos recomendados de Azure Backup](guidance-best-practices.md)
