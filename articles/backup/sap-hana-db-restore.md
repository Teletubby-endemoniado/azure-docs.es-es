---
title: Restauración de bases de datos de SAP HANA en máquinas virtuales de Azure
description: En este artículo se describe cómo restaurar bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure. También puede usar la restauración entre regiones para restaurar las bases de datos en una región secundaria.
ms.topic: conceptual
ms.date: 11/02/2021
author: v-amallick
ms.service: backup
ms.author: v-amallick
ms.openlocfilehash: 427dc637a0aa44ab6a0627b7a844ad6c7d3bb46e
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131431562"
---
# <a name="restore-sap-hana-databases-on-azure-vms"></a>Restauración de bases de datos de SAP HANA en máquinas virtuales de Azure

En este artículo se describe cómo restaurar bases de datos de SAP HANA que se ejecutan en una máquina virtual (VM) de Azure de la que el servicio Azure Backup ha realizado una copia de seguridad en un almacén de Recovery Services. Las restauraciones se pueden usar para crear copias de los datos para escenarios de desarrollo y pruebas, o para volver a un estado anterior.

Para más información sobre cómo hacer una copia de seguridad de bases de datos de SAP HANA, consulte [Copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure](./backup-azure-sap-hana-database.md).

## <a name="restore-to-a-point-in-time-or-to-a-recovery-point"></a>Restauración a un momento dado o a un punto de recuperación

Azure Backup puede restaurar bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure como se indica a continuación:

* Restaurar a una fecha u hora específicas (con precisión de segundos) mediante copias de seguridad de registros. Azure Backup determina automáticamente la copia de seguridad diferencial o completa apropiada, y la cadena de copias de seguridad de registros necesarias para restaurar los datos en función del tiempo seleccionado.

* Restaurar una copia de seguridad completa o diferencial específica para restaurar a un punto de recuperación específico.

## <a name="prerequisites"></a>Prerrequisitos

Antes de restaurar una base de datos, tenga en cuenta lo siguiente:

* Solo puede restaurar la base de datos en una instancia de SAP HANA que se encuentre en la misma región.

* La instancia de destino debe estar registrada en el mismo almacén que la de origen.

* Azure Backup no puede identificar dos instancias de SAP HANA diferentes en la misma máquina virtual. Por lo tanto, no es posible restaurar los datos de una instancia en otra de la misma VM.

* Para asegurarse de que la instancia de SAP HANA de destino está lista para la restauración, compruebe su estado de **Preparación para la copia de seguridad**:

  1. En Azure Portal, vaya al **Centro de copias de seguridad** y haga clic en **+Copia de seguridad**.

     :::image type="content" source="./media/sap-hana-db-restore/backup-center-configure-inline.png" alt-text="Captura de pantalla que muestra cómo iniciar el proceso para comprobar si la instancia SAP HANA de destino está lista para la restauración." lightbox="./media/sap-hana-db-restore/backup-center-configure-expanded.png":::

  1. Seleccione **SAP HANA en Azure VM** como tipo de origen de datos, seleccione el almacén en el que está registrada la instancia de SAP HANA y, a continuación, haga clic en **Continuar**.

     :::image type="content" source="./media/sap-hana-db-restore/hana-select-vault.png" alt-text="Captura de pantalla en la que se muestra la elección de SAP HANA en Azure VM.":::

  1. En **Detectar bases de datos en máquinas virtuales**, Seleccione **Ver detalles**.

     :::image type="content" source="./media/sap-hana-db-restore/hana-discover-databases.png" alt-text="Captura de pantalla que muestra cómo ver los detalles de la base de datos.":::

  1. Revise el estado de **preparación para la copia de seguridad** de la máquina virtual de destino.

     :::image type="content" source="./media/sap-hana-db-restore/hana-select-virtual-machines-inline.png" alt-text="Captura de pantalla que muestra los servidores protegidos." lightbox="./media/sap-hana-db-restore/hana-select-virtual-machines-expanded.png":::

* Para más información acerca de los tipos de restauración que admite SAP HANA, consulte la nota [1642148](https://launchpad.support.sap.com/#/notes/1642148) de SAP HANA.

## <a name="restore-a-database"></a>Restaurar una base de datos

Para realizar la restauración, necesita los siguientes permisos:

* Permisos de **operador de copia de seguridad** en el almacén en el que se realiza la restauración.
* Acceso de **colaborador (escritura)** a la máquina virtual de origen de la que se realiza la copia de seguridad.
* Acceso de **colaborador (escritura**) a la VM de destino:
  * Si va a restaurar en la misma VM, esta será la VM de origen.
  * Si va a restaurar en una ubicación alternativa, esta será la nueva VM de destino.

1. En Azure Portal, vaya al **Centro de copia de seguridad** y haga clic en **Restaurar**.

   :::image type="content" source="./media/sap-hana-db-restore/backup-center-restore-inline.png" alt-text="Captura de pantalla que muestra cómo empezar a restaurar una base de datos de SAP HANA." lightbox="./media/sap-hana-db-restore/backup-center-restore-expanded.png":::

1. Seleccione **SAP HANA en Azure VM** como tipo de origen de datos, seleccione la base de datos que desea restaurar y, a continuación, haga clic en **Continuar**.

   :::image type="content" source="./media/sap-hana-db-restore/hana-restore-select-database.png" alt-text="Captura de pantalla que muestra la restauración de los elementos de copia de seguridad.":::

1. En **Configuración de la restauración**, especifique dónde o cómo quiere restaurar los datos:

   * **Ubicación alternativa**: restaura la base de datos en una ubicación alternativa y mantiene la base de datos de origen original.

   * **Sobrescribir la base de datos**: restaura los datos en la misma instancia de SAP HANA que el origen. Esta opción sobrescribe la base de datos original.

   :::image type="content" source="./media/sap-hana-db-restore/hana-restore-configuration.png" alt-text="Captura de pantalla que muestra la restauración de la configuración.":::

### <a name="restore-to-alternate-location"></a>Restauración a una ubicación alternativa

1. En el menú **Restaurar configuración**, en **Where to Restore** (Ubicación de restauración), seleccione **Ubicación alternativa**.

   :::image type="content" source="./media/sap-hana-db-restore/hana-alternate-location-recovery.png" alt-text="Captura de pantalla que muestra la restauración en una ubicación alternativa.":::

1. Seleccione el nombre de host de SAP HANA y el nombre de la instancia en la que desea restaurar la base de datos.
1. Compruebe si la instancia de SAP HANA de destino está lista para la restauración; para ello, compruebe su estado de **Preparación para la copia de seguridad**. Consulte la sección [Requisitos previos](#prerequisites) para más información.
1. En el cuadro de diálogo **Nombre de la base de datos restaurada**, escriba el nombre de la base de datos de destino.

    > [!NOTE]
    > Las restauraciones de Contenedor de base de datos única (SDC) deben seguir estas [comprobaciones](backup-azure-sap-hana-database-troubleshoot.md#single-container-database-sdc-restore).

1. Si procede, seleccione **Sobrescribir si ya existe una base de datos con el mismo nombre en la instancia de HANA seleccionada**.

1. En **Seleccionar punto de restauración**, seleccione **Registros (a un momento dado)** a [Restauración a un momento dado](#restore-to-a-specific-point-in-time). O bien, seleccione **Completa y diferencial** para [restaurar a un punto de recuperación específico](#restore-to-a-specific-recovery-point).

### <a name="restore-and-overwrite"></a>Restauración y sobrescritura

1. En el menú **Restaurar configuración**, en **Where to Restore** (Ubicación de restauración), seleccione **Sobrescribir la base de datos** > **Aceptar**.

   :::image type="content" source="./media/sap-hana-db-restore/hana-overwrite-database.png" alt-text="Captura de pantalla que muestra cómo sobrescribir la base de datos.":::

1. En **Seleccionar punto de restauración**, seleccione **Registros (a un momento dado)** a [Restauración a un momento dado](#restore-to-a-specific-point-in-time). O bien, seleccione **Completa y diferencial** para [restaurar a un punto de recuperación específico](#restore-to-a-specific-recovery-point).

### <a name="restore-as-files"></a>Restaurar como archivos

>[!Note]
>La restauración como archivos no funciona en el recurso compartido CIFS, pero funciona para NFS.

Para restaurar datos de copia de seguridad como archivos en lugar de una base de datos, seleccione **Restaurar como archivos**. Cuando los archivos se vuelcan en una ruta de acceso especificada, puede llevar estos archivos a cualquier máquina de SAP HANA en la que quiera restaurarlos como base de datos. Dado que puede mover estos archivos a cualquier máquina, ahora puede restaurar los datos entre suscripciones y regiones.

1. En el menú **Restaurar configuración**, en **¿Dónde y cómo se realiza la restauración?** , seleccione **Restaurar como archivos**.
1. Seleccione el nombre de servidor de HANA o **host** en el que quiere restaurar los archivos de copia de seguridad.
1. En **Ruta de acceso de destino en el servidor**, especifique la ruta de acceso de carpetas del servidor seleccionado en el paso 2. Se trata de la ubicación en la que el servicio volcará todos los archivos de copia de seguridad necesarios.

    Los archivos que se vuelcan son:

    * Archivos de copia de seguridad de bases de datos
    * Archivos de catálogo
    * Archivos de metadatos JSON (para cada archivo de copia de seguridad implicado)

    Típicamente, una ruta de acceso a un recurso compartido de red o una ruta de acceso de un recurso compartido de archivos de Azure montado cuando se especifica como una ruta de acceso de destino facilita el acceso a estos archivos de parte de otras máquinas en la red o en el mismo recurso compartido de archivos de Azure montado en ellas.

    >[!NOTE]
    >Para restaurar los archivos de copia de seguridad de base de datos en un recurso compartido de archivos de Azure montado en la VM registrada de destino, asegúrese de que la cuenta raíz tenga accesos de lectura o escritura en el recurso compartido de archivos de Azure.

   :::image type="content" source="./media/sap-hana-db-restore/hana-restore-as-files.png" alt-text="Captura de pantalla que muestra cómo elegir la ruta de acceso de destino.":::

1. Seleccione el **Punto de restauración** correspondiente en el que se restaurarán todas las carpetas y archivos de copia de seguridad.

   :::image type="content" source="./media/sap-hana-db-restore/hana-select-recovery-point-inline.png" alt-text="Captura de pantalla que muestra la selección del punto de restauración." lightbox="./media/sap-hana-db-restore/hana-select-recovery-point-expanded.png":::

1. Todos los archivos de copia de seguridad asociados con el punto de restauración seleccionado se vuelcan en la ruta de acceso de destino.
1. En función del tipo de punto de restauración elegido (**Momento dado** o **Completa y diferencial**), se crearán una o varias carpetas en la ruta de acceso de destino. Una de las carpetas, denominada `Data_<date and time of restore>`, contiene las copias de seguridad completas y, la otra, llamada `Log`, contiene las copias de seguridad de registros y otras copias de seguridad (como diferenciales e incrementales).
1. Mueva estos archivos restaurados al servidor de SAP HANA en el que quiere restaurarlos como base de datos.
1. A continuación, siga estos pasos:
    1. Establezca permisos en la carpeta o el directorio en que se almacenan los archivos de copia de seguridad mediante el comando siguiente:

        ```bash
        chown -R <SID>adm:sapsys <directory>
        ```

    1. Ejecute el siguiente conjunto de comandos como `<SID>adm`.

        ```bash
        su - <sid>adm
        ```

    1. Genere el archivo de catálogo para la restauración. Extraiga **BackupId** del archivo de metadatos JSON para la copia de seguridad completa, que se usará más adelante en la operación de restauración. Asegúrese de que las copias de seguridad completas y de registros se encuentran en carpetas diferentes y elimine los archivos de catálogo y los archivos de metadatos JSON de estas carpetas.

        ```bash
        hdbbackupdiag --generate --dataDir <DataFileDir> --logDirs <LogFilesDir> -d <PathToPlaceCatalogFile>
        ```

        En el comando anterior:

        * `<DataFileDir>`: carpeta que contiene las copias de seguridad completas
        * `<LogFilesDir>`: la carpeta que contiene las copias de seguridad de registros, copias de seguridad diferenciales e incrementales (si las hay)
        * `<PathToPlaceCatalogFile>`: carpeta donde se debe colocar el archivo de catálogo generado

    1. Realice la restauración con el archivo de catálogo recién generado mediante HANA Studio o ejecute la consulta de restauración HDBSQL con este catálogo recién generado. Las consultas HDBSQL se enumeran a continuación:

    * Para restaurar a un momento dado:

        Si está creando una nueva base de datos restaurada, ejecute el comando HDBSQL para crear una nueva base de datos `<DatabaseName>` y, a continuación, detenga la base de datos para la restauración. Sin embargo, si solo está restaurando una base de datos existente, ejecute el comando HDBSQL para detenerla.

        A continuación, ejecute el siguiente comando para restaurar la base de datos:

        ```hdbsql
        RECOVER DATABASE FOR <DatabaseName> UNTIL TIMESTAMP '<TimeStamp>' CLEAR LOG USING SOURCE '<DatabaseName@HostName>'  USING CATALOG PATH ('<PathToGeneratedCatalogInStep3>') USING LOG PATH (' <LogFileDir>') USING DATA PATH ('<DataFileDir>') USING BACKUP_ID <BackupIdFromJsonFile> CHECK ACCESS USING FILE
        ```

        * `<DatabaseName>`: nombre de la base de datos nueva o existente que quiere restaurar
        * `<Timestamp>`: marca de tiempo exacta de la restauración a un momento dado
        * `<DatabaseName@HostName>`: nombre de la base de datos cuya copia de seguridad se usa para la restauración y nombre del servidor de SAP HANA o **host** en que reside esta base de datos. La opción `USING SOURCE <DatabaseName@HostName>` especifica que la copia de seguridad de datos (usada para la restauración) es de una base de datos con un SID o nombre diferente al de la máquina de SAP HANA de destino. Por lo tanto, no es necesario especificarla para las restauraciones realizadas en el mismo servidor de HANA del que se toma la copia de seguridad.
        * `<PathToGeneratedCatalogInStep3>`: ruta de acceso al archivo de catálogo generado en el **paso C**
        * `<DataFileDir>`: carpeta que contiene las copias de seguridad completas
        * `<LogFilesDir>`: la carpeta que contiene las copias de seguridad de registros, copias de seguridad diferenciales e incrementales (si las hay)
        * `<BackupIdFromJsonFile>`: **BackupId** extraído en el **paso C**

    * Para restaurar una copia de seguridad completa o diferencial determinada:

        Si está creando una nueva base de datos restaurada, ejecute el comando HDBSQL para crear una nueva base de datos `<DatabaseName>` y, a continuación, detenga la base de datos para la restauración. Sin embargo, si solo está restaurando una base de datos existente, ejecute el comando HDBSQL para detenerla:

        ```hdbsql
        RECOVER DATA FOR <DatabaseName> USING BACKUP_ID <BackupIdFromJsonFile> USING SOURCE '<DatabaseName@HostName>'  USING CATALOG PATH ('<PathToGeneratedCatalogInStep3>') USING DATA PATH ('<DataFileDir>')  CLEAR LOG
        ```

        * `<DatabaseName>`: nombre de la base de datos nueva o existente que quiere restaurar
        * `<Timestamp>`: marca de tiempo exacta de la restauración a un momento dado
        * `<DatabaseName@HostName>`: nombre de la base de datos cuya copia de seguridad se usa para la restauración y nombre del servidor de SAP HANA o **host** en que reside esta base de datos. La opción `USING SOURCE <DatabaseName@HostName>` especifica que la copia de seguridad de datos (usada para la restauración) es de una base de datos con un SID o nombre diferente al de la máquina de SAP HANA de destino. Por lo tanto, no es necesario especificarla para las restauraciones realizadas en el mismo servidor de HANA del que se toma la copia de seguridad.
        * `<PathToGeneratedCatalogInStep3>`: ruta de acceso al archivo de catálogo generado en el **paso C**
        * `<DataFileDir>`: carpeta que contiene las copias de seguridad completas
        * `<LogFilesDir>`: la carpeta que contiene las copias de seguridad de registros, copias de seguridad diferenciales e incrementales (si las hay)
        * `<BackupIdFromJsonFile>`: **BackupId** extraído en el **paso C**

### <a name="restore-to-a-specific-point-in-time"></a>Restauración a un momento dado

Si ha seleccionado **Registros (punto en el tiempo)** como el tipo de restauración, haga lo siguiente:

1. Seleccione un punto de recuperación en el gráfico del registro y seleccione **Aceptar** para elegir el punto de restauración.

    ![Punto de restauración](media/sap-hana-db-restore/restore-point.png)

1. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración.

    ![Selección de restauración](media/sap-hana-db-restore/restore-restore.png)

1. Realice un seguimiento del progreso de la restauración en el área **Notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

    ![Restauración desencadenada correctamente](media/sap-hana-db-restore/restore-triggered.png)

### <a name="restore-to-a-specific-recovery-point"></a>Restauración a un punto de recuperación específico

Si ha seleccionado **Completo y diferencial** como el tipo de restauración, haga lo siguiente:

1. Seleccione un punto de recuperación en la lista y seleccione **Aceptar** para elegir el punto de restauración.

    ![Restauración a un punto de recuperación específico](media/sap-hana-db-restore/specific-recovery-point.png)

1. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración.

    ![Inicio del trabajo de restauración](media/sap-hana-db-restore/restore-specific.png)

1. Realice un seguimiento del progreso de la restauración en el área **Notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

    ![Progreso de la restauración](media/sap-hana-db-restore/restore-progress.png)

    > [!NOTE]
    > En las restauraciones de Contenedor de varias bases de datos (MDC), cuando la base de datos del sistema se restaura en una instancia de destino, es necesario volver a ejecutar el script de registro previo. Solo entonces las restauraciones de bases de datos de inquilino subsiguientes se realizarán correctamente. Para más información, consulte [Solución de problemas: restauración de MDC](backup-azure-sap-hana-database-troubleshoot.md#multiple-container-database-mdc-restore).

## <a name="cross-region-restore"></a>Restauración entre regiones

Restauración entre regiones (CRR), una de las opciones de restauración, le permite restaurar bases de datos SAP HANA hospedadas en máquinas virtuales de Azure en una región secundaria, que es una región emparejada de Azure.

Para la incorporación a la característica, lea la [sección Antes de empezar](./backup-create-rs-vault.md#set-cross-region-restore).

Para ver si la opción CRR está habilitada, siga las instrucciones de [Configuración de la restauración entre regiones](backup-create-rs-vault.md#configure-cross-region-restore).

### <a name="view-backup-items-in-secondary-region"></a>Ver los elementos de copia de seguridad de la región secundaria

Si la opción CRR está habilitada, puede ver los elementos de copia de seguridad de la región secundaria.

1. En el portal, vaya a **Almacén de Recovery Services** > **Elementos de copia de seguridad**.
1. Seleccione **Región secundaria** para ver los elementos de la región secundaria.

>[!NOTE]
>En la lista solo se mostrarán los tipos de administración de copia de seguridad que admiten la característica CRR. Actualmente, solo se admite la restauración de datos de regiones secundarias en una región secundaria.

![Elementos de copia de seguridad en la región secundaria](./media/sap-hana-db-restore/backup-items-secondary-region.png)

![Bases de datos en la región secundaria](./media/sap-hana-db-restore/databases-secondary-region.png)

### <a name="restore-in-secondary-region"></a>Restauración en la región secundaria

La experiencia del usuario de restauración de la región secundaria será similar a la de restauración de la región primaria. Al configurar los detalles en el panel Restaurar configuración para configurar la restauración, se le pedirá que proporcione solo los parámetros de la región secundaria. Debe existir un almacén en la región secundaria y el servidor SAP HANA debe estar registrado en dicho almacén.

![¿Dónde y cómo se realiza la restauración?](./media/sap-hana-db-restore/restore-secondary-region.png)

![Notificación de desencadenador de restauración en curso](./media/backup-azure-arm-restore-vms/restorenotifications.png)

>[!NOTE]
>* Una vez que se desencadena la restauración y se ha iniciado la fase de transferencia de datos, no se puede cancelar el trabajo de restauración.
>* El rol y el nivel de acceso necesarios para realizar la operación de restauración en varias regiones son el rol _Operador de copia de seguridad_ en la suscripción y el acceso de _Colaborador (escritura)_ en las máquinas virtuales de origen y de destino. Para ver los trabajos de copias de seguridad, _Lector de copia de seguridad_ es el permiso mínimo necesario en la suscripción.

### <a name="monitoring-secondary-region-restore-jobs"></a>Supervisión de trabajos de restauración en la región secundaria

1. En Azure Portal, vaya al **Centro de copia de seguridad** > **Trabajos de copia de seguridad**.
1. Filtre **Operación** por el valor **CrossRegionRestore** para ver los trabajos de la región secundaria.

   :::image type="content" source="./media/sap-hana-db-restore/hana-view-jobs-inline.png" alt-text="Captura de pantalla que muestra los trabajos de copia de seguridad filtrados." lightbox="./media/sap-hana-db-restore/hana-view-jobs-expanded.png":::

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga información acerca de cómo](sap-hana-db-manage.md) administrar bases de datos de SAP HANA de las que se realizan copias de seguridad mediante Azure Backup.
