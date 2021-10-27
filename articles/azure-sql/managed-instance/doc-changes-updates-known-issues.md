---
title: Problemas conocidos
titleSuffix: Azure SQL Managed Instance
description: Obtenga información sobre los problemas conocidos actualmente de Azure SQL Managed Instance y sus posibles soluciones alternativas o resoluciones.
services: sql-database
author: MashaMSFT
ms.author: mathoma
ms.service: sql-managed-instance
ms.subservice: service-overview
ms.custom: references_regions
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/24/2021
ms.openlocfilehash: e66333fa1174eb9a614f2028641a83a8391e39e0
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130047953"
---
# <a name="known-issues-with-azure-sql-managed-instance"></a>Problemas conocidos de Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

En este artículo se enumeran los problemas conocidos actualmente de [Azure SQL Managed Instance](https://azure.microsoft.com/updates/?product=sql-database&query=sql%20managed%20instance), así como su fecha de resolución o posible solución alternativa. Para obtener más información sobre Azure SQL Managed Instance, vea la [introducción](sql-managed-instance-paas-overview.md) y las [novedades](doc-changes-updates-release-notes-whats-new.md). 

 
## <a name="known-issues"></a>Problemas conocidos

|Incidencia  |Fecha de detección  |Estado  |Fecha de resolución  |
|---------|---------|---------|---------|
|[Mensaje de error engañoso en Azure Portal que sugiere la recreación de la entidad de servicio](#misleading-error-message-on-azure-portal-suggesting-recreation-of-the-service-principal)|Septiembre de 2021|||
|[Cambiar el tipo de conexión no afecta a las conexiones a través del punto de conexión del grupo de conmutación por error](#changing-the-connection-type-does-not-affect-connections-through-the-failover-group-endpoint)|Enero de 2021|Tiene solución alternativa||
|[Se puede producir un error transitorio en el procedimiento sp_send_dbmail cuando se usa el parámetro @query](#procedure-sp_send_dbmail-may-transiently-fail-when--parameter-is-used)|Enero de 2021|Tiene solución alternativa||
|[Se pueden ejecutar transacciones distribuidas después de quitar la instancia administrada del grupo de confianza del servidor](#distributed-transactions-can-be-executed-after-removing-managed-instance-from-server-trust-group)|Octubre de 2020|Tiene solución alternativa||
|[No se pueden ejecutar transacciones distribuidas después de la operación de escalado de la instancia administrada](#distributed-transactions-cannot-be-executed-after-managed-instance-scaling-operation)|Octubre de 2020|Tiene solución alternativa||
|[No se puede crear una instancia de SQL Managed Instance con el mismo nombre que el servidor lógico eliminado anteriormente](#cannot-create-sql-managed-instance-with-the-same-name-as-logical-server-previously-deleted)|Agosto de 2020|Tiene solución alternativa||
|[BULK INSERT](/sql/t-sql/statements/bulk-insert-transact-sql)/[OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql) de Azure SQL y la instrucción `BACKUP`/`RESTORE` de SQL Managed Instance no pueden usar la identidad administrada de Azure AD para autenticarse en Azure Storage|Septiembre de 2020|Tiene solución alternativa||
|[La entidad de servicio no puede acceder a Azure AD y AKV](#service-principal-cannot-access-azure-ad-and-akv)|Agosto de 2020|Tiene solución alternativa||
|[La restauración de la copia de seguridad manual sin CHECKSUM puede devolver un error](#restoring-manual-backup-without-checksum-might-fail)|Mayo de 2020|Resuelto|Junio de 2020|
|[El agente deja de responder al modificar, deshabilitar o habilitar los trabajos existentes](#agent-becomes-unresponsive-upon-modifying-disabling-or-enabling-existing-jobs)|Mayo de 2020|Resuelto|Junio de 2020|
|[Permisos en el grupo de recursos no aplicados a Instancia administrada de SQL](#permissions-on-resource-group-not-applied-to-sql-managed-instance)|Febrero de 2020|Resuelto|Noviembre de 2020|
|[Limitación de la conmutación por error manual a través del portal para grupos de conmutación por error](#limitation-of-manual-failover-via-portal-for-failover-groups)|Enero de 2020|Tiene solución alternativa||
|[Los roles del Agente SQL necesitan permisos de ejecución (EXECUTE) explícitos para los inicios de sesión que no sean sysadmin](#in-memory-oltp-memory-limits-are-not-applied)|Diciembre de 2019|Tiene solución alternativa||
|[Los trabajos del Agente SQL pueden ser interrumpidos por el reinicio del proceso del agente](#sql-agent-jobs-can-be-interrupted-by-agent-process-restart)|Diciembre de 2019|Resuelto|Marzo de 2020|
|[No se admiten inicios de sesión y usuarios de Azure AD en SSDT](#azure-ad-logins-and-users-are-not-supported-in-ssdt)|Noviembre de 2019|No hay solución alternativa||
|[No se aplican los límites de memoria de OLTP en memoria](#in-memory-oltp-memory-limits-are-not-applied)|Octubre de 2019|Tiene solución alternativa||
|[Se mostró un error al intentar quitar un archivo que no está vacío](#wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty)|Octubre de 2019|Tiene solución alternativa||
|[Las operaciones de cambio de nivel de servicio y creación de instancia se bloquean con la restauración en curso de la base de datos](#change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore)|Septiembre de 2019|Tiene solución alternativa||
|[Es posible que sea necesario volver a configurar Resource Governor en el nivel de servicio Crítico para la empresa después de la conmutación por error](#resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover)|Septiembre de 2019|Tiene solución alternativa||
|[Los cuadros de diálogo de Service Broker entre bases de datos se deben volver a inicializar después de la actualización del nivel de servicio](#cross-database-service-broker-dialogs-must-be-reinitialized-after-service-tier-upgrade)|Agosto de 2019|Tiene solución alternativa||
|[No se admite la suplantación de tipos de inicio de sesión de Azure AD](#impersonation-of-azure-ad-login-types-is-not-supported)|Julio de 2019|No hay solución alternativa||
|[@queryNo se admite el parámetro  en sp_send_db_mail](#-parameter-not-supported-in-sp_send_db_mail)|Abril de 2019|Resuelto|Enero de 2021|
|[La replicación transaccional debe volver a configurarse después de la conmutación por error geográfica](#transactional-replication-must-be-reconfigured-after-geo-failover)|Marzo de 2019|No hay solución alternativa||
|[La base de datos temporal se usa durante la operación RESTORE](#temporary-database-is-used-during-restore-operation)||Tiene solución alternativa||
|[Se vuelve a crear la estructura y el contenido de TEMPDB](#tempdb-structure-and-content-is-re-created)||No hay solución alternativa||
|[Exceder el espacio de almacenamiento con archivos de base de datos pequeños](#exceeding-storage-space-with-small-database-files)||Tiene solución alternativa||
|[Se muestran valores de GUID en lugar de nombres de base de datos](#guid-values-shown-instead-of-database-names)||Tiene solución alternativa||
|[Los registros de errores no se conservan](#error-logs-arent-persisted)||No hay solución alternativa||
|[No se admite el ámbito de transacción en dos bases de datos de la misma instancia](#transaction-scope-on-two-databases-within-the-same-instance-isnt-supported)||Tiene solución alternativa|Marzo de 2020|
|[Los módulos de CLR y los servidores vinculados en algún momento no pueden hacer referencia a una dirección IP local](#clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address)||Tiene solución alternativa||
|No se verifica la coherencia de la base de datos mediante DBCC CHECKDB después de restaurar la base de datos de Azure Blob Storage.||Resuelto|Noviembre de 2019|
|La restauración de una base de datos a un momento dado del nivel de servicio Crítico para la empresa al De uso general no se completará correctamente si la base de datos de origen contiene objetos OLTP en memoria.||Resuelto|Octubre de 2019|
|Característica Correo electrónico de base de datos con servidores de correo externos (que no son de Azure) mediante una conexión segura||Resuelto|Octubre de 2019|
|Las bases de datos independientes no se admiten en la Instancia administrada de SQL||Resuelto|Agosto de 2019|
||||| 

## <a name="resolved"></a>Resuelto

### <a name="restoring-manual-backup-without-checksum-might-fail"></a>La restauración de la copia de seguridad manual sin CHECKSUM puede devolver un error

En determinadas circunstancias, es posible que no se restaure la copia de seguridad manual de las bases de datos que se realizó en una instancia administrada sin CHECKSUM. En tal caso, vuelva a intentar restaurar la copia de seguridad hasta que complete el proceso correctamente.

**Solución alternativa**: realice copias de seguridad manuales de las bases de datos en instancias administradas con CHECKSUM habilitado.

### <a name="agent-becomes-unresponsive-upon-modifying-disabling-or-enabling-existing-jobs"></a>El agente deja de responder al modificar, deshabilitar o habilitar los trabajos existentes

En determinadas circunstancias, la modificación, deshabilitación o habilitación de un trabajo existente puede hacer que el agente deje de responder. El problema se mitiga automáticamente tras la detección, lo que da como resultado un reinicio del proceso del agente.

### <a name="permissions-on-resource-group-not-applied-to-sql-managed-instance"></a>Permisos en el grupo de recursos no aplicados a la Instancia administrada de SQL

Cuando el rol de Azure del colaborador de SQL Managed Instance se aplica a un grupo de recursos (RG), no se aplica a SQL Managed Instance y no tiene ningún efecto.

**Solución alternativa**: configure un rol de colaborador de SQL Managed Instance para los usuarios en el nivel de suscripción.

### <a name="sql-agent-jobs-can-be-interrupted-by-agent-process-restart"></a>Los trabajos del Agente SQL pueden ser interrumpidos por el reinicio del proceso del agente

**(Se resuelve en marzo de 2020)** El Agente SQL crea una sesión cada vez que se inicia un trabajo, lo que aumenta gradualmente el consumo de memoria. Para evitar alcanzar el límite de memoria interna que bloquearía la ejecución de los trabajos programados, el proceso del agente se reiniciará una vez que el consumo de memoria alcance el umbral. Puede provocar interrumpir la ejecución de los trabajos que se ejecutan en el momento del reinicio.

### <a name="query-parameter-not-supported-in-sp_send_db_mail"></a>@queryNo se admite el parámetro en sp_send_db_mail

El `@query`parámetro del procedimiento [sp_send_db_mail](/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) no funciona.

## <a name="has-workaround"></a>Tiene solución alternativa

### <a name="changing-the-connection-type-does-not-affect-connections-through-the-failover-group-endpoint"></a>Cambiar el tipo de conexión no afecta a las conexiones a través del punto de conexión del grupo de conmutación por error

Si una instancia participa en un grupo de [conmutación por error automática](../database/auto-failover-group-overview.md), el cambio del [tipo de conexión](../managed-instance/connection-types-overview.md) de la instancia no afecta a las conexiones establecidas a través del punto de conexión del agente de escucha del grupo de conmutación por error.

**Solución alternativa**: quitar y volver a crear el grupo de conmutación por error automática después de cambiar el tipo de conexión.

### <a name="procedure-sp_send_dbmail-may-transiently-fail-when-query-parameter-is-used"></a>Se puede producir un error transitorio en el procedimiento sp_send_dbmail cuando se usa el parámetro @query

Se puede producir un error transitorio en el procedimiento `sp_send_dbmail` cuando se usa el parámetro `@query`. Cuando se produce este problema, todas las ejecuciones del procedimiento sp_send_dbmail devuelven el error `Msg 22050, Level 16, State 1` y el mensaje `Failed to initialize sqlcmd library with error number -2147467259`. Para poder ver este error correctamente, se debe llamar al procedimiento con el valor predeterminado 0 del parámetro `@exclude_query_output`; de lo contrario, el error no se propagará.
Este problema se debe a un error conocido relacionado con la forma en que `sp_send_dbmail` usa la suplantación y la agrupación de conexiones.
Para evitar este problema, encapsule el código para el envío de un correo electrónico en una lógica de reintento que dependa del parámetro de salida `@mailitem_id`. Si se produce un error en la ejecución, el valor del parámetro será NULL, lo que indicará que debe llamar una vez más a `sp_send_dbmail` para enviar correctamente el correo electrónico. Este es un ejemplo de esta lógica de reintento.

```sql
CREATE PROCEDURE send_dbmail_with_retry AS
BEGIN
    DECLARE @miid INT
    EXEC msdb.dbo.sp_send_dbmail
        @recipients = 'name@mail.com', @subject = 'Subject', @query = 'select * from dbo.test_table',
        @profile_name ='AzureManagedInstance_dbmail_profile', @execute_query_database = 'testdb',
        @mailitem_id = @miid OUTPUT

    -- If sp_send_dbmail returned NULL @mailidem_id then retry sending email.
    --
    IF (@miid is NULL)
    EXEC msdb.dbo.sp_send_dbmail
        @recipients = 'name@mail.com', @subject = 'Subject', @query = 'select * from dbo.test_table',
        @profile_name ='AzureManagedInstance_dbmail_profile', @execute_query_database = 'testdb',
END
```

### <a name="distributed-transactions-can-be-executed-after-removing-managed-instance-from-server-trust-group"></a>Se pueden ejecutar transacciones distribuidas después de quitar la instancia administrada del grupo de confianza del servidor

Los [grupos de confianza del servidor](../managed-instance/server-trust-group-overview.md) se usan para establecer confianza entre instancias administradas, lo que constituye un requisito previo para ejecutar [transacciones distribuidas](../database/elastic-transactions-overview.md). Después de quitar una instancia administrada del grupo de confianza del servidor o de eliminar el grupo, es posible que pueda seguir ejecutando transacciones distribuidas. Existe una solución alternativa que puede aplicar para asegurarse de que las transacciones distribuidas están deshabilitadas: la [conmutación por error manual iniciada por el usuario](../managed-instance/user-initiated-failover.md) en la instancia administrada.

### <a name="distributed-transactions-cannot-be-executed-after-managed-instance-scaling-operation"></a>No se pueden ejecutar transacciones distribuidas después de la operación de escalado de la instancia administrada

Las operaciones de escalado de SQL Managed Instance que incluyen un cambio de nivel de servicio o del número de núcleos virtuales restablecen la configuración del grupo de confianza del servidor en el back-end y deshabilitan la ejecución de [transacciones distribuidas](../database/elastic-transactions-overview.md). Como solución alternativa, elimine y cree un [grupo de confianza de servidor](../managed-instance/server-trust-group-overview.md) en Azure Portal.

### <a name="cannot-create-sql-managed-instance-with-the-same-name-as-logical-server-previously-deleted"></a>No se puede crear una instancia de SQL Managed Instance con el mismo nombre que el servidor lógico eliminado anteriormente

Se crea un registro de DNS de `<name>.database.windows.com` cuando se crea un [servidor lógico en Azure](../database/logical-servers.md) para Azure SQL Database, y cuando se crea una instancia de SQL Managed Instance. El registro de DNS debe ser único. Así, si crea un servidor lógico para SQL Database y luego lo elimina, hay un período de umbral de siete días para que el nombre se libere de los registros. Durante ese período no se puede crear ninguna instancia de SQL Managed Instance con el mismo nombre que el servidor lógico eliminado. Como solución alternativa, use otro nombre para la instancia de SQL Managed Instance o cree una incidencia de soporte técnico para liberar el nombre del servidor lógico.  


### <a name="bulk-insert-and-backuprestore-statements-should-use-sas-key-to-access-azure-storage"></a>Las instrucciones BULK INSERT y BACKUP/RESTORE deben usar la clave SAS para acceder a Azure Storage

Actualmente no se admite la sintaxis `DATABASE SCOPED CREDENTIAL` con una identidad administrada para la autenticación en Azure Storage. Microsoft recomienda usar una [firma de acceso compartido](../../storage/common/storage-sas-overview.md) para la [credencial con ámbito de base de datos](/sql/t-sql/statements/create-credential-transact-sql#d-creating-a-credential-using-a-sas-token) al acceder a Azure Storage para la inserción masiva, las instrucciones `BACKUP` y `RESTORE`, o la función `OPENROWSET`. Por ejemplo:

```sql
CREATE DATABASE SCOPED CREDENTIAL sas_cred WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************';
GO
CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
  WITH ( TYPE = BLOB_STORAGE, LOCATION = 'https://****************.blob.core.windows.net/invoices', CREDENTIAL= sas_cred );
GO
BULK INSERT Sales.Invoices FROM 'inv-2017-12-08.csv' WITH (DATA_SOURCE = 'MyAzureBlobStorage');
```

Para otro ejemplo del uso de `BULK INSERT` con una clave SAS, consulte el artículo sobre la [firma de acceso compartido para autenticarse eb Storage](/sql/t-sql/statements/bulk-insert-transact-sql#f-importing-data-from-a-file-in-azure-blob-storage). 

### <a name="service-principal-cannot-access-azure-ad-and-akv"></a>La entidad de servicio no puede acceder a Azure AD y AKV

En algunas circunstancias, puede existir un problema con la entidad de servicio que se utiliza para acceder a los servicios de Azure AD y Azure Key Vault (AKV). En consecuencia, este problema afecta al uso de la autenticación de Azure AD y al cifrado de datos transparente (TDE) con Instancia administrada de SQL. Esto podría experimentarse como un problema de conectividad intermitente o no podría ejecutar instrucciones como `CREATE LOGIN/USER FROM EXTERNAL PROVIDER` o `EXECUTE AS LOGIN/USER`. La configuración de TDE con clave administrada por el cliente en una nueva Instancia administrada de SQL de Azure también podría no funcionar en algunas circunstancias.

**Solución alternativa**: Para evitar que se produzca este problema en SQL Managed Instance antes de ejecutar cualquier comando de actualización, o en caso de que ya se haya experimentado este problema después de actualizar los comandos, vaya a Azure Portal y acceda a la [página Administrador de Active Directory](../database/authentication-aad-configure.md?tabs=azure-powershell#azure-portal) de SQL Managed Instance. Compruebe si puede ver el mensaje de error "Instancia administrada necesita una entidad de seguridad para acceder a Azure Active Directory. Haga clic aquí para crear una entidad de servicio". En caso de que aparezca este mensaje de error, haga clic en él y siga las instrucciones paso a paso que se proporcionan hasta que se resuelva el error.

### <a name="limitation-of-manual-failover-via-portal-for-failover-groups"></a>Limitación de la conmutación por error manual a través del portal para grupos de conmutación por error

Si un grupo de conmutación por error abarca instancias de distintas suscripciones o grupos de recursos de Azure, la conmutación por error manual no se puede iniciar desde la instancia principal del grupo de conmutación por error.

**Solución alternativa**: inicie la conmutación por error mediante el portal desde la base de datos geográfica secundaria.

### <a name="sql-agent-roles-need-explicit-execute-permissions-for-non-sysadmin-logins"></a>Los roles del Agente SQL necesitan permisos de ejecución (EXECUTE) explícitos para los inicios de sesión que no sean sysadmin

Si los inicios de sesión que no son de sysadmin se agregan a cualquiera de los [roles fijos de base de datos del Agente SQL](/sql/ssms/agent/sql-server-agent-fixed-database-roles), existe un problema en el que los permisos EXECUTE explícitos deben concederse a los tres procedimientos almacenados en la base de datos maestra para que estos inicios de sesión funcionen. Si se encuentra este problema, aparece el mensaje de error "Se denegó el permiso EXECUTE en el objeto <nombre_objeto> (Microsoft SQL Server, error: 229)".

**Solución alternativa**: Una vez que agregue inicios de sesión a un rol fijo de base de datos del Agente SQL (SQLAgentUserRole, SQLAgentReaderRole o SQLAgentOperatorRole), para cada uno de los inicios de sesión agregados a estos roles, ejecute el siguiente script T-SQL para conceder explícitamente permisos EXECUTE a los procedimientos almacenados enumerados.

```sql
USE [master]
GO
CREATE USER [login_name] FOR LOGIN [login_name];
GO
GRANT EXECUTE ON master.dbo.xp_sqlagent_enum_jobs TO [login_name];
GRANT EXECUTE ON master.dbo.xp_sqlagent_is_starting TO [login_name];
GRANT EXECUTE ON master.dbo.xp_sqlagent_notify TO [login_name];
```

### <a name="in-memory-oltp-memory-limits-are-not-applied"></a>No se aplican los límites de memoria de OLTP en memoria

El nivel de servicio Crítico para la empresa no aplicará correctamente los [límites de memoria máximos para los objetos optimizados para memoria](../managed-instance/resource-limits.md#in-memory-oltp-available-space) en algunos casos. La Instancia administrada de SQL puede permitir que la carga de trabajo use más memoria para las operaciones de OLTP en memoria, lo que puede afectar a la disponibilidad y la estabilidad de la instancia. Es posible que las consultas de OLTP en memoria que alcanzan los límites no generen errores de inmediato. Pronto se resolverá este problema. Las consultas que usan en mayor grado OLTP en memoria producirán un error antes si llegan a los [límites](../managed-instance/resource-limits.md#in-memory-oltp-available-space).

**Solución alternativa**: [Supervise el uso de almacenamiento de OLTP en memoria](../in-memory-oltp-monitor-space.md) con [SQL Server Management Studio](/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage#bkmk_Monitoring) para asegurarse de que la carga de trabajo no esté usando más memoria de la disponible. Aumente los límites de memoria que dependen del número de núcleos virtuales, u optimice la carga de trabajo para usar menos memoria.
 
### <a name="wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty"></a>Se mostró un error al intentar quitar un archivo que no está vacío

SQL Server y SQL Managed Instance [no permiten al usuario quitar un archivo que no está vacío](/sql/relational-databases/databases/delete-data-or-log-files-from-a-database#Prerequisites). Si intenta quitar un archivo de datos no vacío mediante una instrucción `ALTER DATABASE REMOVE FILE`, el error `Msg 5042 – The file '<file_name>' cannot be removed because it is not empty` no se mostrará inmediatamente. SQL Managed Instance seguirá intentando quitar el archivo y se producirá un error en la operación después de 30 minutos con `Internal server error`.

**Solución alternativa**: quite el contenido del archivo mediante el comando `DBCC SHRINKFILE (N'<file_name>', EMPTYFILE)`. Si este es el único archivo del grupo de archivos, debe eliminar los datos de la tabla o partición asociada a este grupo de archivos antes de reducir el archivo y, opcionalmente, cargar estos datos en otra tabla o partición.

### <a name="change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore"></a>Las operaciones de cambio de nivel de servicio y creación de instancia se bloquean con la restauración en curso de la base de datos

La instrucción `RESTORE` en curso, el proceso de migración del servicio de migración de datos y la restauración a un momento dado integrada bloquean la actualización de un nivel de servicio o del cambio de tamaño de la instancia existente, así como la creación de nuevas instancias hasta que finalice el proceso de restauración. 

El proceso de restauración bloqueará estas operaciones en las instancias administradas y en los grupos de instancias de la misma subred en la que se ejecute el proceso de restauración. Las instancias de los grupos de instancias no se ven afectadas. No se producirá ningún error en las operaciones de creación o cambio del nivel de servicio ni se agotará su tiempo de espera. Continuarán una vez que el proceso de restauración se complete o cancele.

**Solución alternativa**: espere a que finalice el proceso de restauración; también puede cancelarlo si la operación de creación o actualización del nivel de servicio tiene mayor prioridad.

### <a name="resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover"></a>Es posible que sea necesario volver a configurar Resource Governor en el nivel de servicio Crítico para la empresa después de la conmutación por error

La característica [Resource Governor](/sql/relational-databases/resource-governor/resource-governor) que le permite limitar los recursos asignados a la carga de trabajo de usuario puede clasificar incorrectamente alguna carga de trabajo de usuario después de una conmutación por error o un cambio de nivel de servicio iniciado por el usuario (por ejemplo, el cambio de número máximo de núcleos virtuales o tamaño máximo de almacenamiento de instancia).

**Solución alternativa**: Ejecute `ALTER RESOURCE GOVERNOR RECONFIGURE` periódicamente o como parte de un trabajo del Agente SQL que ejecuta la tarea de SQL cuando la instancia se inicia si usa [Resource Governor](/sql/relational-databases/resource-governor/resource-governor).

### <a name="cross-database-service-broker-dialogs-must-be-reinitialized-after-service-tier-upgrade"></a>Los cuadros de diálogo de Service Broker entre bases de datos se deben volver a inicializar después de la actualización del nivel de servicio

Los cuadros de diálogo de Service Broker entre bases de datos dejarán de enviar mensajes a los servicios de otras bases de datos después de la operación para cambiar el nivel de servicio. Los mensajes *no se pierden* y se pueden encontrar en la cola del remitente. Todos los cambios de tamaño de almacenamiento en núcleos virtuales o instancias de SQL Managed Instance harán que cambie un valor `service_broke_guid` de la vista [sys.databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql) para todas las bases de datos. Todos los `DIALOG` creados con la instrucción [BEGIN DIALOG](/sql/t-sql/statements/begin-dialog-conversation-transact-sql) que hace referencia a los Service Broker de otra base de datos dejarán de entregar mensajes al servicio de destino.

**Solución alternativa**: detenga todas las actividades que usen conversaciones con cuadros de diálogo de Service Broker entre bases de datos antes de actualizar un nivel de servicio y vuelva a inicializarlos después. Si quedan mensajes que no se entregaron después del cambio de nivel de servicio, lea los mensajes de la cola de origen y vuelva a enviarlos a la cola de destino.

### <a name="temporary-database-is-used-during-restore-operation"></a>La base de datos temporal se usa durante la operación RESTORE

Cuando una base de datos se restaura en SQL Managed Instance, el servicio de restauración crea primero una base de datos vacía con el nombre deseado para asignar el nombre a la instancia. Después de un tiempo, esta base de datos se quitará y se iniciará la restauración de la base de datos real. 

La base de datos que se encuentra en estado de *restauración* será temporal y tendrá un valor de GUID aleatorio en lugar del nombre. El nombre temporal se cambiará al nombre deseado especificado en la instrucción `RESTORE` una vez que finalice el proceso de restauración. 

En la fase inicial, un usuario puede acceder a la base de datos vacía e incluso crear tablas o cargar datos en esta base de datos. Esta base de datos temporal se quitará cuando el servicio de restauración inicie la segunda fase.

**Solución alternativa**: No acceda a la base de datos que va a restaurar hasta que vea que la restauración se ha completado.

### <a name="exceeding-storage-space-with-small-database-files"></a>Exceder el espacio de almacenamiento con archivos de base de datos pequeños

Se puede producir un error en las instrucciones `CREATE DATABASE`, `ALTER DATABASE ADD FILE` y `RESTORE DATABASE` porque la instancia puede alcanzar el límite de almacenamiento de Azure.

Cada instancia de uso general de SQL Managed Instance tiene hasta 35 TB de almacenamiento reservado para el espacio en disco Premium de Azure. Cada archivo de base de datos se coloca en un disco físico independiente. Los posibles tamaños de disco son: 128 GB, 256 GB, 512 GB, 1 TB o 4 TB. El espacio no utilizado en el disco no se cobra, pero la suma total de los tamaños de disco Premium de Azure no puede superar los 35 TB. En algunos casos, una instancia administrada que no necesita 8 TB en total puede superar los 35 TB de límite de Azure en tamaño de almacenamiento debido a la fragmentación interna.

Por ejemplo, una instancia de uso general de SQL Managed Instance puede tener un archivo grande de 1,2 TB almacenado en un disco de 4 TB. También podría tener 248 archivos cada uno de 1 GB situados en discos independientes de 128 GB. En este ejemplo:

- El tamaño de almacenamiento total del disco es de 1 x 4 TB + 248 x 128 GB = 35 TB.
- El espacio total reservado para las bases de datos en la instancia es de 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.

Este ejemplo ilustra que, en determinadas circunstancias, debido a una distribución específica de archivos, una instancia de SQL Managed Instance podría alcanzar el límite de 35 TB que está reservado para un disco adjunto Premium de Azure cuando no se lo espere.

En este ejemplo, las bases de datos existentes seguirán funcionando y pueden crecer sin ningún problema, siempre y cuando no se agreguen nuevos archivos. No se podrían crear ni restaurar nuevas bases de datos porque no hay suficiente espacio para nuevas unidades de disco, incluso si el tamaño total de todas las bases de datos no alcanza el límite de tamaño de la instancia. El error que se devuelve en ese caso no está claro.

También puede [identificar el número de archivos restantes](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) mediante el uso de vistas del sistema. Si se alcanza este límite, intente [vaciar y eliminar algunos de los archivos más pequeños mediante la instrucción DBCC SHRINKFILE](/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) o cambie al [nivel Crítico para la empresa, que no tiene este límite](../managed-instance/resource-limits.md#service-tier-characteristics).


### <a name="guid-values-shown-instead-of-database-names"></a>Se muestran valores de GUID en lugar de nombres de base de datos

Varias vistas del sistema, contadores de rendimiento, mensajes de error, XEvents y entradas de registro de errores muestran identificadores de base de datos GUID en lugar de los nombres reales de base de datos. No confíe en estos identificadores GUID porque se reemplazarán por los nombres reales de las bases de datos en el futuro.

**Solución alternativa**: Use la vista `sys.databases` para resolver el nombre real de la base de datos del nombre de la base de datos física, especificado en forma de identificadores de base de datos GUID:

```tsql
SELECT name as ActualDatabaseName, physical_database_name as GUIDDatabaseIdentifier 
FROM sys.databases
WHERE database_id > 4;
```

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>No se admite el ámbito de transacción en dos bases de datos de la misma instancia

**(Se resuelve en marzo de 2020)** La clase `TransactionScope` de .NET no funciona si dos consultas se envían a dos bases de datos de la misma instancia en el mismo ámbito de transacción:

```csharp
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

**Solución alternativa (no es necesaria desde marzo de 2020)** : use [SqlConnection.ChangeDatabase(String)](/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) para utilizar otra base de datos en un contexto de conexión en lugar de usar dos conexiones.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>Los módulos de CLR y los servidores vinculados en algún momento no pueden hacer referencia a una dirección IP local.

Los módulos de CLR en SQL Managed Instance y las consultas distribuidas o los servidores vinculados que hacen referencia a una instancia actual en algún momento no pueden resolver la dirección IP de una instancia local. Este error es un problema transitorio.

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>No se admite el ámbito de transacción en dos bases de datos de la misma instancia

**(Se resuelve en marzo de 2020)** La clase `TransactionScope` de .NET no funciona si dos consultas se envían a dos bases de datos de la misma instancia en el mismo ámbito de transacción:

```csharp
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

**Solución alternativa (no es necesaria desde marzo de 2020)** : use [SqlConnection.ChangeDatabase(String)](/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) para utilizar otra base de datos en un contexto de conexión en lugar de usar dos conexiones.


## <a name="no-resolution"></a>No hay ninguna resolución

### <a name="misleading-error-message-on-azure-portal-suggesting-recreation-of-the-service-principal"></a>Mensaje de error engañoso en Azure Portal que sugiere la recreación de la entidad de servicio

La hoja de _administración de Active Directory_ de Azure Portal para Azure SQL Managed Instance puede mostrar el siguiente mensaje de error aunque la entidad de servicio ya exista:

"Instancia administrada necesita una entidad de seguridad para acceder a Azure Active Directory. Haga clic aquí para crear una entidad de servicio".

Puede ignorar este mensaje de error si la entidad de servicio de la instancia administrada ya existe o la autenticación de AAD en la instancia administrada funciona. 

Para comprobar si existe una entidad de servicio, vaya a la página _Aplicaciones empresariales_ en Azure Portal, seleccione _Identidades administradas_ en la lista desplegable _Tipo de aplicación_, seleccione _Aplicar_ y escriba el nombre de la instancia administrada en el cuadro de búsqueda. Si el nombre de instancia aparece en la lista de resultados, la entidad de servicio ya existe y no se necesitan más acciones.

Si ya ha seguido las instrucciones del mensaje de error y ha hecho clic en el vínculo del mensaje de error, se ha recreado la entidad de servicio de la instancia administrada. En ese caso, asigne permisos de lectura de Azure AD a la entidad de servicio recién creada para que la autenticación de Azure AD funcione correctamente. Para ello, siga estas [instrucciones](../database/authentication-aad-configure.md?tabs=azure-powershell#powershell) mediante Azure PowerShell.

### <a name="azure-ad-logins-and-users-are-not-supported-in-ssdt"></a>No se admiten inicios de sesión y usuarios de Azure AD en SSDT

SQL Server Data Tools no es totalmente compatible con los inicios de sesión y los usuarios de Azure AD.

### <a name="impersonation-of-azure-ad-login-types-is-not-supported"></a>No se admite la suplantación de tipos de inicio de sesión de Azure AD

No se admite la suplantación con `EXECUTE AS USER` o `EXECUTE AS LOGIN` de las entidades de seguridad de Azure Active Directory (Azure AD) siguientes:
- Usuarios de Azure AD con alias. Se devuelve el error siguiente en este caso: `15517`.
- Inicios de sesión y usuarios de Azure AD basados en aplicaciones de Azure AD o entidades de servicio. Se devuelven los errores siguientes en este caso: `15517` y `15406`.

### <a name="transactional-replication-must-be-reconfigured-after-geo-failover"></a>La replicación transaccional debe volver a configurarse después de la conmutación por error geográfica

Si la replicación transaccional se habilita en una base de datos en un grupo de conmutación por error automática, el administrador de la Instancia administrada de SQL debe limpiar todas las publicaciones de la base de datos principal anterior y volver a configurarlas en la nueva base principal después de que se produzca una conmutación por error a otra región. Para más información, consulte [Replicación](../managed-instance/transact-sql-tsql-differences-sql-server.md#replication).

### <a name="tempdb-structure-and-content-is-re-created"></a>Se vuelve a crear la estructura y el contenido de TEMPDB

La base de datos `tempdb` siempre se divide en 12 archivos de datos y la estructura de archivos no se puede cambiar. No se puede cambiar el tamaño máximo por archivo y no se pueden agregar nuevos archivos a `tempdb`. Siempre se vuelve a crear `Tempdb` como base de datos vacía cuando se inicia la instancia o se realiza una conmutación por error. Cualquier cambio que se realice en `tempdb` no se conservará.


### <a name="error-logs-arent-persisted"></a>Los registros de errores no se conservan

Los registros de errores que están disponibles en la Instancia administrada de SQL no se conservan y su tamaño no está incluido en el límite de almacenamiento máximo. Es posible que se borren automáticamente los registros de errores en caso de conmutación por error. Puede haber huecos en el historial del registro de errores porque la Instancia administrada de SQL se ha migrado varias veces en varias máquinas virtuales.

## <a name="contribute-to-content"></a>Contribución al contenido

Para colaborar en la documentación de Azure SQL, consulte la [Guía para colaboradores de Microsoft Docs](/contribute/).

## <a name="next-steps"></a>Pasos siguientes

Para obtener una lista de actualizaciones y mejoras de SQL Managed Instance, vea las [actualizaciones del servicio SQL Managed Instance](https://azure.microsoft.com/updates/?product=sql-database&query=sql%20managed%20instance).

Para ver las actualizaciones y mejoras de todos los servicios de Azure, consulte las [actualizaciones de los servicios](https://azure.microsoft.com/updates).
