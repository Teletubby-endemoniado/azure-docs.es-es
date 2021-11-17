---
title: Restauración de bases de datos de SQL Server en una máquina virtual de Azure
description: En este artículo se describe cómo restaurar bases de datos SQL Server que se ejecutan en una máquina virtual de Azure y cuyas copias de seguridad se realizan con Azure Backup. También puede usar la restauración entre regiones para restaurar las bases de datos en una región secundaria.
ms.topic: conceptual
ms.date: 11/02/2021
author: v-amallick
ms.service: backup
ms.author: v-amallick
ms.openlocfilehash: 0e687cbae798a480eaf417de04b653b88d2e2c94
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131431119"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Restauración de bases de datos SQL Server en máquinas virtuales de Azure

En este artículo se describe cómo restaurar una base de datos SQL Server que se ejecuta en una máquina virtual (VM) de Azure cuya copia de seguridad se ha realizado en un almacén de Azure Backup Recovery Services con el servicio [Azure Backup](backup-overview.md).

En este artículo se describe cómo restaurar las bases de datos SQL Server. Para obtener más información, consulte [Copia de seguridad de bases de datos de SQL Server en máquinas virtuales de Azure](backup-azure-sql-database.md).

## <a name="restore-to-a-time-or-a-recovery-point"></a>Restauración a un momento dado o a un punto de recuperación

Azure Backup puede restaurar las bases de datos SQL Server que se ejecutan en las máquinas virtuales de Azure como se indica a continuación:

- Restaure a una fecha u hora específicas (hasta los segundos) mediante copias de seguridad del registro de transacciones. Azure Backup determina automáticamente la copia de seguridad diferencial completa apropiada y la cadena de copias de seguridad de registros necesarias para restaurar los datos en función del tiempo seleccionado.
- Restaure una copia de seguridad completa o diferencial específica para restaurar a un punto de recuperación específico.

## <a name="restore-prerequisites"></a>Requisitos previos de la restauración

Antes de restaurar una base de datos, tenga en cuenta lo siguiente:

- Puede restaurar la base de datos en una instancia de SQL Server en la misma región de Azure.
- El servidor de destino debe estar registrado como origen en el mismo almacén.
- Si tiene varias instancias que se ejecutan en un servidor, todas las instancias deben estar en funcionamiento. De lo contrario, el servidor no aparecerá en la lista de servidores de destino para restaurar la base de datos. Para más información, consulte los [pasos de solución de problemas](backup-sql-server-azure-troubleshoot.md#faulty-instance-in-a-vm-with-multiple-sql-server-instances).
- Para restaurar una base de datos cifrada TDE en otra de SQL Server, primero debe [restaurar el certificado en el servidor de destino](/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server).
- Las bases de datos habilitadas para [CDC](/sql/relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server) deben restaurarse con la opción [Restaurar como archivos](#restore-as-files).
- Antes de realizar una restauración de la base de datos "maestra", inicie la instancia de SQL Server en modo de usuario único con la opción de inicio **-m AzureWorkloadBackup**.
  - El valor de **-m** es el nombre del cliente.
  - Solo el nombre de cliente especificado puede abrir la conexión.
- Para todas las bases de datos del sistema (modelo, master, msdb), detenga el servicio Agente SQL Server antes de desencadenar la restauración.
- Cierre las aplicaciones que pueden intentar quitar una conexión a cualquiera de estas bases de datos.

## <a name="restore-a-database"></a>Restaurar una base de datos

Para realizar la restauración, necesita los siguientes permisos:

- Permisos de **operador de copia de seguridad** en el almacén en el que se realiza la restauración.
- Acceso de **colaborador (escritura)** a la máquina virtual de origen de la que se realiza la copia de seguridad.
- Acceso de **colaborador (escritura)** a la máquina virtual de destino:
  - Si va a restaurar en la misma VM, esta será la VM de origen.
  - Si va a restaurar en una ubicación alternativa, esta será la nueva VM de destino.

Realice la restauración como sigue:

1. En Azure Portal, vaya al **Centro de copia de seguridad** y haga clic en **Restaurar**.

   :::image type="content" source="./media/backup-azure-sql-database/backup-center-restore-inline.png" alt-text="Captura de pantalla que muestra el inicio del proceso de restauración." lightbox="./media/backup-azure-sql-database/backup-center-restore-expanded.png":::

1. Seleccione **SQL en Azure VM** como tipo de origen de datos, seleccione la base de datos que desea restaurar y haga clic en **Continuar**.

   :::image type="content" source="./media/backup-azure-sql-database/sql-restore.png" alt-text="Captura de pantalla que muestra cómo seleccionar el tipo de origen de datos.":::

1. En **Restaurar configuración**, especifique dónde o cómo quiere restaurar los datos:
   
   - **Ubicación alternativa**: restaura la base de datos en una ubicación alternativa y mantiene la base de datos de origen original.
   - **Sobrescribir la base de datos**: restaura los datos en la misma instancia de SQL Server que el origen. Esta opción sobrescribe la base de datos original.

        > [!IMPORTANT]
        > Si la base de datos seleccionada pertenece a un grupo de disponibilidad AlwaysOn, SQL no permite sobrescribir la base de datos. Solo está disponible **Ubicación alternativa**.
        >
   - **Restaurar como archivos**: En lugar de restaurar como una base de datos, restaure los archivos de copia de seguridad que se pueden recuperar como base de datos más tarde en cualquier máquina en la que los archivos están presentes mediante SQL Server Management Studio.

### <a name="restore-to-an-alternate-location"></a>Restauración a una ubicación alternativa

1. En el menú **Restaurar configuración**, en **Where to Restore** (Ubicación de restauración), seleccione **Ubicación alternativa**.
1. Seleccione el nombre de SQL Server y la instancia en la que desea restaurar la base de datos.
1. En el cuadro de diálogo **Nombre de la base de datos restaurada**, escriba el nombre de la base de datos de destino.
1. Si procede, seleccione **Overwrite if the DB with the same name already exists on selected SQL instance** (Sobrescribir si ya existe una base de datos con el mismo nombre en la instancia de SQL seleccionada).
1. Seleccione **Punto de restauración** y seleccione si [restaurar a un momento dado](#restore-to-a-specific-point-in-time) o [restaurar a un punto de recuperación específico](#restore-to-a-specific-restore-point).

   :::image type="content" source="./media/backup-azure-sql-database/sql-alternate-location-recovery.png" alt-text="Captura de pantalla que muestra la selección del punto de restauración.":::

   :::image type="content" source="./media/backup-azure-sql-database/restore-points-sql-inline.png" alt-text="Captura de pantalla que muestra la restauración a un momento dado." lightbox="./media/backup-azure-sql-database/restore-points-sql-expanded.png":::

1. En el menú **Configuración avanzada**:

    - Si desea mantener la base de datos no operativa después de la restauración, habilite **Restaurar con NORECOVERY**.
    - Si desea cambiar la ubicación de restauración en el servidor de destino, escriba nuevas rutas de acceso de destino.

        ![Escribir rutas de acceso de destino](./media/backup-azure-sql-database/target-paths.png)

1. Seleccione **Aceptar** para desencadenar la restauración. Realice un seguimiento del progreso de la restauración en el área **Notificaciones** o en la visualización **Trabajos de copia de seguridad** del almacén.

    > [!NOTE]
    > La restauración a un momento dado solo está disponible para copias de seguridad de registros de bases de datos con un modelo de recuperación optimizado para cargas masivas y completas de registros.

### <a name="restore-and-overwrite"></a>Restauración y sobrescritura

1. En el menú **Restaurar configuración**, en **Where to Restore** (Ubicación de restauración), seleccione **Sobrescribir la base de datos** > **Aceptar**.

    ![Seleccionar Sobrescribir la base de datos](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. En **Seleccionar punto de restauración**, seleccione **Registros (a un momento dado)** a [Restauración a un momento dado](#restore-to-a-specific-point-in-time). O bien, seleccione **Completa y diferencial** para restaurar a un [punto de recuperación específico](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > La restauración a un momento dado solo está disponible para copias de seguridad de registros de bases de datos con un modelo de recuperación optimizado para cargas masivas y completas de registros.

### <a name="restore-as-files"></a>Restaurar como archivos

Para restaurar datos de copia de seguridad como archivos .bak en lugar de una base de datos, seleccione **Restaurar como archivos**. Cuando los archivos se vuelcan a una ruta de acceso especificada, puede llevar estos archivos a cualquier máquina en la que quiera restaurarlos como base de datos. Dado que puede mover estos archivos a cualquier máquina, ahora puede restaurar los datos entre suscripciones y regiones.

1. En **¿Dónde y cómo se realiza la restauración?** , seleccione **Restaurar como archivos**.
1. Elija el nombre del servidor SQL Server en el que desea restaurar los archivos de copia de seguridad.
1. En **Destination path on the server** (Ruta de acceso de destino en el servidor), especifique la ruta de acceso de carpetas en el servidor seleccionado en el paso 2. Se trata de la ubicación en la que el servicio volcará todos los archivos de copia de seguridad necesarios. Típicamente, una ruta de acceso a un recurso compartido de red o una ruta de acceso de un recurso compartido de archivos de Azure montado cuando se especifica como una ruta de acceso de destino facilita el acceso a estos archivos de parte de otras máquinas en la red o en el mismo recurso compartido de archivos de Azure montado en ellas.<BR>

   >[!Note]
   >Para restaurar los archivos de copia de seguridad de base de datos de un recurso compartido de archivos de Azure montado en la máquina virtual registrada de destino, asegúrese de que NT AUTHORITY\SYSTEM tenga acceso al recurso compartido de archivos. Puede realizar los pasos que se indican a continuación para conceder los permisos de lectura y escritura al AFS montado en la máquina virtual:
   >
   >- Ejecutar `PsExec -s cmd` para entrar en el shell de NT AUTHORITY\SYSTEM
   >   - Ejecute `cmdkey /add:<storageacct>.file.core.windows.net /user:AZURE\<storageacct> /pass:<storagekey>`
   >   - Comprobar el acceso con `dir \\<storageacct>.file.core.windows.net\<filesharename>`
   >- Iniciar una restauración como archivos desde el almacén de copia de seguridad en `\\<storageacct>.file.core.windows.net\<filesharename>` como ruta de acceso.<BR>
   >Puede descargar PsExec de la página [Sysinternals](/sysinternals/downloads/psexec).

1. Seleccione **Aceptar**.

    ![Seleccionar Restaurar como archivos](./media/backup-azure-sql-database/restore-as-files.png)

1. Seleccione **Punto de restauración** y seleccione si [restaurar a un momento dado](#restore-to-a-specific-point-in-time) o [restaurar a un punto de recuperación específico](#restore-to-a-specific-restore-point).

1. Todos los archivos de copia de seguridad asociados con el punto de recuperación seleccionado se vuelcan en la ruta de acceso de destino. Puede restaurar los archivos como una base de datos en cualquier máquina en la que estén presentes mediante SQL Server Management Studio.

    ![Archivos de copia de seguridad restaurados en la ruta de acceso de destino](./media/backup-azure-sql-database/sql-backup-files.png)

### <a name="restore-to-a-specific-point-in-time"></a>Restauración a un momento dado

Si ha seleccionado **Registros (punto en el tiempo)** como el tipo de restauración, haga lo siguiente:

1. En **Fecha y hora de restauración**, abra el calendario. En el calendario, las fechas que contienen puntos de recuperación se muestran en negrita, y la fecha actual está resaltada.
1. Seleccione una fecha con puntos de recuperación. No puede seleccionar fechas que no tengan puntos de recuperación.

    ![Abrir el calendario](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

1. Una vez seleccionada una fecha, el gráfico de escala de tiempo muestra los puntos de recuperación disponibles en un rango continuo.
1. Especifique una hora para la recuperación en el gráfico de escala de tiempo o seleccione una hora. Después, seleccione **Aceptar**.

### <a name="restore-to-a-specific-restore-point"></a>Restauración a un punto de restauración específico

Si ha seleccionado **Completo y diferencial** como el tipo de restauración, haga lo siguiente:

1. Seleccione un punto de recuperación de la lista y seleccione **Aceptar** para completar el procedimiento de punto de restauración.

    ![Elegir un punto de recuperación completo](./media/backup-azure-sql-database/choose-full-recovery-point.png)

    >[!NOTE]
    > De forma predeterminada, se muestran los puntos de recuperación de los 30 últimos días. Para mostrar los puntos de recuperación anteriores a 30 días, seleccione **Filtrar** y elija un intervalo personalizado.

### <a name="restore-databases-with-large-number-of-files"></a>Restauración de bases de datos con un gran número de archivos

Si el tamaño de la cadena total de archivos en una base de datos es mayor que un [determinado límite](backup-sql-server-azure-troubleshoot.md#size-limit-for-files), Azure Backup almacena la lista de archivos de la base de datos en un componente pit diferente, de modo que no podrá definir la ruta de la restauración de destino durante la operación de restauración. En su lugar, los archivos se restaurarán en la ruta predeterminada SQL.

  ![Restauración de bases de datos con archivos de gran tamaño](./media/backup-azure-sql-database/restore-large-files.jpg)

## <a name="cross-region-restore"></a>Restauración entre regiones

Restauración entre regiones (CRR), una de las opciones de restauración, le permite restaurar bases de datos SQL hospedadas en máquinas virtuales de Azure en una región secundaria, que es una región emparejada de Azure.

Para incorporar la característica, lea la [sección Antes de empezar](./backup-create-rs-vault.md#set-cross-region-restore).

Para ver si la opción CRR está habilitada, siga las instrucciones de [Configuración de la restauración entre regiones](backup-create-rs-vault.md#configure-cross-region-restore).

### <a name="view-backup-items-in-secondary-region"></a>Ver los elementos de copia de seguridad de la región secundaria

Si la opción CRR está habilitada, puede ver los elementos de copia de seguridad de la región secundaria.

1. En el portal, vaya a **Almacén de Recovery Services** > **Elementos de copia de seguridad**.
1. Seleccione **Región secundaria** para ver los elementos de la región secundaria.

>[!NOTE]
>En la lista solo se mostrarán los tipos de administración de copia de seguridad que admiten la característica CRR. Actualmente, solo se admite la restauración de datos de regiones secundarias en una región secundaria.

![Elementos de copia de seguridad en la región secundaria](./media/backup-azure-sql-database/backup-items-secondary-region.png)

![Bases de datos en la región secundaria](./media/backup-azure-sql-database/databases-secondary-region.png)

### <a name="restore-in-secondary-region"></a>Restauración en la región secundaria

La experiencia del usuario de restauración de la región secundaria será similar a la de restauración de la región primaria. Al configurar los detalles en el panel Restaurar configuración para configurar la restauración, se le pedirá que proporcione solo los parámetros de la región secundaria. Debe existir un almacén en la región secundaria y el servidor SQL se debe registrar en el almacén de la región secundaria.

![¿Dónde y cómo se realiza la restauración?](./media/backup-azure-sql-database/restore-secondary-region.png)

![Notificación de desencadenador de restauración en curso](./media/backup-azure-arm-restore-vms/restorenotifications.png)

>[!NOTE]
>- Una vez que se desencadena la restauración y se ha iniciado la fase de transferencia de datos, no se puede cancelar el trabajo de restauración.
>- El rol y el nivel de acceso necesarios para realizar la operación de restauración en varias regiones son el rol _Operador de copia de seguridad_ en la suscripción y el acceso de _Colaborador (escritura)_ en las máquinas virtuales de origen y de destino. Para ver los trabajos de copias de seguridad, _Lector de copia de seguridad_ es el permiso mínimo necesario en la suscripción.

### <a name="monitoring-secondary-region-restore-jobs"></a>Supervisión de trabajos de restauración en la región secundaria

1. En Azure Portal, vaya al **Centro de copia de seguridad** > **Trabajos de copia de seguridad**.
1. Filtre la operación por **CrossRegionRestore** para ver los trabajos de la región secundaria.

   :::image type="content" source="./media/backup-azure-sql-database/backup-center-jobs-inline.png" alt-text="Captura de pantalla que muestra los trabajos de copia de seguridad filtrados." lightbox="./media/backup-azure-sql-database/backup-center-jobs-expanded.png":::

## <a name="next-steps"></a>Pasos siguientes

[Administrar y supervisar](manage-monitor-sql-database-backup.md) bases de datos SQL Server cuya copia de seguridad se ha realizado con Azure Backup.
