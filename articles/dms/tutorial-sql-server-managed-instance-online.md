---
title: 'Tutorial: Migración de SQL Server en línea a SQL Managed Instance'
titleSuffix: Azure Database Migration Service
description: Obtenga información sobre cómo realizar una migración en línea de una instancia local de SQL Server a una Instancia administrada de Azure SQL mediante Azure Database Migration Service.
services: dms
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: tutorial
ms.date: 08/20/2021
ms.openlocfilehash: 4c9028b559c537c0707b0ab3264fbfa11d26a350
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128664165"
---
# <a name="tutorial-migrate-sql-server-to-an-azure-sql-managed-instance-online-using-dms"></a>Tutorial: Migración de SQL Server a una Instancia administrada de Azure SQL en línea mediante DMS

Azure Database Migration Service se puede usar para migrar las bases de datos de una instancia de SQL Server a una [Instancia administrada de Azure SQL](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md) con un tiempo de inactividad mínimo. Para ver otros métodos que pueden requerir trabajo manual, vea el artículo [Migración de una instancia de SQL Server a una Instancia administrada de Azure SQL](../azure-sql/migration-guides/managed-instance/sql-server-to-managed-instance-guide.md).

En este tutorial, se va a migrar la base de datos [AdventureWorks2016](/sql/samples/adventureworks-install-configure#download-backup-files) de una instancia local de SQL Server a una instancia de SQL Managed Instance con un tiempo de inactividad mínimo mediante Azure Database Migration Service.

Aprenderá a:
> [!div class="checklist"]
>
> * Registrar el proveedor de recursos de Azure DataMigration.
> * Crear una instancia de Azure Database Migration Service.
> * Crear un proyecto de migración e iniciar una migración en línea mediante Azure Database Migration Service.
> * Supervisar la migración
> * Realizar la migración total cuando esté listo.

> [!IMPORTANT]
> En el caso de las migraciones en línea de SQL Server a SQL Managed Instance mediante Azure Database Migration Service, es preciso proporcionar la copia de seguridad de la base de datos completa y las posteriores copias de seguridad del registro en el recurso compartido de red SMB que el servicio puede usar para migrar bases de datos. Azure Database Migration Service no se inicia copias de seguridad y en su lugar usa las copias de seguridad existentes, que puede que ya tenga como parte de su plan de recuperación ante desastres, para la migración.
> Asegúrese de que realiza las [copias de seguridad mediante la opción WITH CHECKSUM](/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server?preserve-view=true&view=sql-server-2017). Cada copia de seguridad se puede escribir en un archivo de copia de seguridad independiente o en varios archivos de copia de seguridad. Sin embargo, no se admite la anexación de varias copias de seguridad (es decir, el registro completo y de transacciones) en un único medio de copia de seguridad. Por último, puede usar copias de seguridad comprimidas para reducir la probabilidad de experimentar posibles problemas asociados con la migración de copias de seguridad de gran tamaño.

> [!NOTE]
> El uso de Azure Database Migration Service para realizar una migración en línea requiere la creación de una instancia basada en el plan de tarifa Premium.

> [!IMPORTANT]
> Para disfrutar de una experiencia de migración óptima, Microsoft recomienda crear una instancia de Azure Database Migration Service en la misma región de Azure que la base de datos de destino. Si los datos se transfieren entre diferentes regiones o ubicaciones geográficas, el proceso de migración puede verse afectado y pueden producirse errores.

> [!IMPORTANT]
> Reduzca la duración del proceso de migración en línea tanto como sea posible para minimizar el riesgo de interrupción causado por la reconfiguración de la instancia o el mantenimiento planeado. Si esto ocurre, el proceso de migración volverá a empezar desde el principio. En el caso del mantenimiento planeado, hay un período de gracia de 36 horas para que se reinicie la migración.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

En este artículo se describe una migración en línea desde SQL Server hasta una Instancia administrada de SQL. Para migraciones sin conexión, vea [Migración de SQL Server a una Instancia administrada sin conexión de SQL mediante DMS](tutorial-sql-server-to-managed-instance.md).

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesita:

* Descargar e instalar [SQL Server 2016 o una versión posterior](https://www.microsoft.com/sql-server/sql-server-downloads).
* Habilitar el protocolo TCP/IP, que se deshabilita de forma predeterminada durante la instalación de SQL Server Express, siguiendo las instrucciones del artículo [Habilitar o deshabilitar un protocolo de red de servidor](/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
* [Restaure la base de datos AdventureWorks2016 a la instancia de SQL Server.](/sql/samples/adventureworks-install-configure#restore-to-sql-server)
* Cree una instancia de Azure Virtual Network para Azure Database Migration Service mediante el modelo de implementación de Azure Resource Manager, que proporciona conectividad de sitio a sitio a los servidores de origen local mediante [ExpressRoute](../expressroute/expressroute-introduction.md) o [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md). [Obtenga información sobre topologías de red para migraciones de Instancia administrada de SQL mediante Azure Database Migration Service](./resource-network-topologies.md). Para más información sobre la creación de una red virtual, consulte la documentación de [Virtual Network](../virtual-network/index.yml)y, especialmente, los artículos de inicio rápido con detalles paso a paso.

    > [!NOTE]
    > Durante la configuración de la red virtual, si usa ExpressRoute con emparejamiento de red a Microsoft, agregue los siguientes [puntos de conexión](../virtual-network/virtual-network-service-endpoints-overview.md) de servicio a la subred en la que se aprovisionará el servicio:
    >
    > * Punto de conexión de base de datos de destino (por ejemplo, punto de conexión de SQL, punto de conexión de Cosmos DB, etc.)
    > * Punto de conexión de Storage
    > * Punto de conexión de Service Bus
    >
    > Esta configuración es necesaria porque Azure Database Migration Service no tiene conexión a Internet.
    >
    >Si no dispone de conectividad de sitio a sitio entre la red local y Azure, o si el ancho de banda de conectividad de sitio a sitio es limitado, considere la posibilidad de usar Azure Database Migration Service en modo híbrido (versión preliminar). El modo híbrido hace uso de un trabajo de migración local junto con una instancia de Azure Database Migration Service que se ejecuta en la nube. Para crear una instancia de Azure Database Migration Service en el modo híbrido, consulte el artículo [Creación de una instancia de Azure Database Migration Service en modo híbrido mediante Azure Portal](./quickstart-create-data-migration-service-portal.md).

    > [!IMPORTANT]
    > Con respecto a la cuenta de almacenamiento que se usa como parte de la migración, debe:
    > * Optar por permitir que todas las redes tengan acceso a la cuenta de almacenamiento
    > * Active la [delegación de subred](../virtual-network/manage-subnet-delegation.md) en la subred de MI y actualice las reglas de firewall de la cuenta de almacenamiento para permitir esta subred.

* Asegúrese de que las reglas del grupo de seguridad de red de la red virtual no bloquean el puerto de salida 443 de ServiceTag para ServiceBus, Storage y AzureMonitor. Para más información sobre el filtrado del tráfico con grupos de seguridad de red para redes virtuales, vea el artículo [Filtrado del tráfico de red con grupos de seguridad de red](../virtual-network/virtual-network-vnet-plan-design-arm.md).
* Configure el [Firewall de Windows para acceder al motor de base de datos de origen](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Abra el firewall de Windows para que Azure Database Migration Service pueda acceder al servidor SQL Server de origen; de forma predeterminada, es el puerto TCP 1433. Si la instancia predeterminada está escuchando en otro puerto, agréguelo al firewall.
* Si se ejecutan varias instancias con nombre de SQL Server con puertos dinámicos, puede ser conveniente habilitar el servicio SQL Browser y permitir el acceso al puerto UDP 1434 mediante los firewalls para que Azure Database Migration Service pueda conectarse a una instancia con nombre en el servidor de origen.
* Si va a usar un dispositivo de firewall delante de las bases de datos de origen, puede que sea necesario agregar reglas de firewall para permitir que Azure Database Migration Service acceda a las bases de datos de origen para realizar la migración, así como archivos a través del puerto SMB 445.
* Cree una Instancia administrada de SQL mediante los pasos que se describen en el artículo [Creación de una Instancia administrada de SQL en Azure Portal](../azure-sql/managed-instance/instance-create-quickstart.md).
* Asegúrese de que los inicios de sesión usados para conectar la instancia de SQL Server de origen y SQL Managed Instance de destino sean miembros del rol de servidor sysadmin.
* Especifique un recurso compartido de red SMB que contenga todos los archivos de copia de seguridad de la base de datos completa y los posteriores archivos de copia de seguridad de registros de transacciones, ya que Azure Database Migration Service puede usarlos para la migración de la base de datos.
* Asegúrese de que la cuenta de servicio que ejecuta la instancia de SQL Server de origen tiene privilegios de escritura en el recurso compartido de red que haya creado, y que la cuenta del equipo del servidor de origen tiene acceso de lectura y escritura para el mismo recurso compartido.
* Anote un usuario de Windows (y su contraseña) que tenga privilegios de control total sobre el recurso compartido de red que creó anteriormente. Azure Database Migration Service suplanta la credencial de usuario para cargar los archivos de copia de seguridad en el contenedor de Azure Storage para la operación de restauración.
* Cree un identificador de aplicación de Azure Active Directory que genere la clave del identificador de aplicación que puede usar Azure Database Migration Service para conectarse a la instancia administrada de Azure Database y al contenedor de Azure Storage. Para más información, consulte el artículo [Uso del portal para crear una aplicación de Azure Active Directory y una entidad de servicio con acceso a los recursos](../active-directory/develop/howto-create-service-principal-portal.md).

  > [!NOTE]
  > El identificador de aplicación utilizado por el Azure Database Migration Service admite la autenticación secreta (basada en contraseña) para las entidades de servicio. No admite la autenticación basada en certificados.

  > [!NOTE]
  > Azure Database Migration Service requiere el permiso de colaborador sobre la suscripción correspondiente al identificador de aplicación especificado. También puede crear roles personalizados que concedan los permisos específicos que Azure Database Migration Service requiere. A fin de obtener instrucciones paso a paso sobre el uso de roles personalizados, vea el artículo [Roles personalizados para las migraciones en línea de SQL Server a Instancias administradas de SQL](./resource-custom-roles-sql-db-managed-instance.md).

* Cree o anote el **nivel de rendimiento estándar**, cuenta de almacenamiento de Azure, que permite que el servicio DMS cargue los archivos de copia de seguridad de la base de datos a y los use para migrar las bases de datos.  Asegúrese de crear la cuenta Azure Storage en la misma región en la que se creó la instancia de Azure Database Migration Service.

  > [!NOTE]
  > Al migrar una base de datos protegida mediante [Cifrado de datos transparente](../azure-sql/database/transparent-data-encryption-tde-overview.md) a una instancia administrada con la opción de migración en línea, se debe migrar el certificado correspondiente del entorno local o la instancia de SQL Server de la VM de Azure antes de restaurar la base de datos. Para consultar los pasos detallados, consulte [Migración de un certificado TDE a Instancia administrada](../azure-sql/database/transparent-data-encryption-tde-overview.md).

[!INCLUDE [resource-provider-register](../../includes/database-migration-service-resource-provider-register.md)]

## <a name="create-an-azure-database-migration-service-instance"></a>Creación de una instancia de Azure Database Migration Service

1. En el menú de Azure Portal o en la página **principal**, seleccione **Crear un recurso**. Busque y seleccione **Azure Database Migration Service**.

    ![Azure Marketplace](media/tutorial-sql-server-to-managed-instance-online/portal-marketplace.png)

2. En la pantalla **Azure Database Migration Service**, seleccione **Crear**.

    ![Creación de una instancia de Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance-online/dms-create-service-1.png)

3. En la pantalla de aspectos básicos **Crear el servicio de migración**:

     - Seleccione la suscripción.
     - Cree un grupo de recursos o seleccione uno existente.
     - Especifique un nombre para la instancia de Azure Database Migration Service.
     - Seleccione la ubicación en la que quiere crear la instancia de Azure Database Migration Service.
     - Elija **Azure** como modo de servicio.
     - Seleccione una SKU del plan de tarifa Premium. 
     
      > [!NOTE]
      > Las migraciones en línea solo se admiten cuando se usa el nivel Premium.

     - Para más información sobre los costos y planes de tarifa, vea la [página de precios](https://aka.ms/dms-pricing).

    ![Configuración de los valores básicos de la instancia de Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance-online/dms-create-service-2.png)

     - Seleccione **Siguiente: Redes**.

4. En la pantalla de red **Crear el servicio de migración**:

    - Seleccione una red virtual existente o cree una nueva. La red virtual proporciona a Azure Database Migration Service acceso al servidor SQL Server de origen y a la instancia de Azure SQL Managed Instance de destino.
     
    - Para más información sobre cómo crear una red virtual en Azure Portal, consulte el artículo [Creación de una red virtual con Azure Portal](../virtual-network/quick-create-portal.md).
    
    - Para más información, consulte el artículo [Topologías de red para migraciones de Azure SQL Managed Instance mediante Azure Database Migration Service](./resource-network-topologies.md).

      ![Configuración de la red de la instancia de Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance-online/dms-create-service-3.png)

    - Seleccione **Revisar y crear** para revisar los detalles y luego **Crear** para crear el servicio.

## <a name="create-a-migration-project"></a>Creación de un proyecto de migración

Después de crear una instancia del servicio, búsquela en Azure Portal, ábrala y cree un proyecto de migración.

1. En el menú de Azure Portal, seleccione **Todos los servicios**. Busque y seleccione **Azure Database Migration Service**.

    ![Búsqueda de todas las instancias de Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance-online/dms-search.png)

2. En la pantalla **Azure Database Migration Services**, seleccione el nombre de la instancia de Azure Database Migration Service que creó.

3. Seleccione **Nuevo proyecto de migración**.

     ![Buscar una instancia de Azure Database Migration Service](media/tutorial-sql-server-to-managed-instance-online/dms-create-project-1.png)

4. En la pantalla **New migration project** (Nuevo proyecto de migración), especifique el nombre del proyecto; en el cuadro de texto **Source server type** (Tipo de servidor de origen), seleccione **SQL Server**; en el cuadro de texto **Target server type** (Tipo de servidor de destino), seleccione **Azure SQL Database Managed Instance** (Instancia administrada de Azure SQL Database) y, finalmente, en **Choose type of activity** (Elegir tipo de actividad), seleccione **Online data migration** (Migración de datos en línea).

   ![Creación de un proyecto de Database Migration Service](media/tutorial-sql-server-to-managed-instance-online/dms-create-project-2.png)

5. Seleccione **Crear y ejecutar una actividad** para crear el proyecto y ejecutar la actividad de migración.

## <a name="specify-source-details"></a>Especificación de los detalles de origen

1. En la pantalla **Seleccionar origen**, especifique los detalles de conexión de la instancia de SQL Server de origen.

    Asegúrese de usar un nombre de dominio completo (FQDN) para el nombre de la instancia de SQL Server de origen. También puede usar la dirección IP en los casos en que no sea posible la resolución de nombres de DNS.

2. Si no ha instalado ningún certificado de confianza en el servidor, seleccione la casilla **Certificado de servidor de confianza**.

    Si no hay ningún certificado de confianza instalado, SQL Server genera un certificado autofirmado cuando se inicia la instancia. Este certificado se usa para cifrar las credenciales de las conexiones del cliente.

    > [!CAUTION]
    > Las conexiones TLS cifradas mediante un certificado autofirmado no proporcionan una gran seguridad. Son susceptibles de sufrir ataques de tipo "Man in the middle". No debe confiar en TLS con certificados autofirmados en un entorno de producción, ni en servidores conectados a Internet.

    ![Detalles del origen](media/tutorial-sql-server-to-managed-instance-online/dms-source-details.png)

3. Seleccione **Siguiente: Seleccionar destino**.

## <a name="specify-target-details"></a>Especificación de los detalles de destino

1. En la pantalla **Seleccionar destino**, especifique el **id. de la aplicación** y la **clave** que la instancia de DMS puede usar para conectarse a la instancia de destino de SQL Managed Instance y la cuenta de Azure Storage.

    Para más información, consulte el artículo [Uso del portal para crear una aplicación de Azure Active Directory y una entidad de servicio con acceso a los recursos](../active-directory/develop/howto-create-service-principal-portal.md).

2. Seleccione la **suscripción** que contiene la instancia de destino de SQL Managed Instance y, después, seleccione la instancia de SQL Managed Instance de destino.

    Si aún no ha aprovisionado la Instancia administrada de SQL, seleccione el [vínculo](../azure-sql/managed-instance/instance-create-quickstart.md) para hacerlo. Cuando la Instancia administrada de SQL esté lista, regrese a este proyecto específico para ejecutar la migración.

3. Proporcione el **Usuario de SQL** y la **Contraseña** para conectarse a la Instancia administrada de SQL.

    ![Selección del destino](media/tutorial-sql-server-to-managed-instance-online/dms-target-details.png)

4. Seleccione **Next: Select databases** (Siguiente: Seleccionar bases de datos).

## <a name="specify-source-databases"></a>Especificación de las bases de datos de origen

1. En la pantalla **Seleccionar bases de datos**, seleccione las bases de datos de origen que quiere migrar.

  ![Selección de las bases de datos de origen](media/tutorial-sql-server-to-managed-instance-online/dms-source-database.png)

  > [!IMPORTANT]
  > Si usa SQL Server Integration Services (SSIS), DMS no admite actualmente la migración de la base de datos de catálogo de los proyectos o paquetes de SSIS (SSISDB) desde SQL Server hasta la Instancia administrada de SQL. Sin embargo, puede aprovisionar SSIS en Azure Data Factory (ADF) y volver a implementar los proyectos o paquetes de SSIS en la SSISDB de destino que hospeda la Instancia administrada de SQL. Para más información acerca de la migración de paquetes de SSIS, consulte el artículo [Migración de paquetes de SQL Server Integration Services a Azure](./how-to-migrate-ssis-packages.md).

2. Seleccione **Siguiente: Configuración de valores de migración**.

## <a name="configure-migration-settings"></a>Configuración de valores de migración

1. En la pantalla **Configurar los valores de la migración**, proporcione los detalles siguientes:

    | Parámetro | Descripción |
    |--------|---------|
    |**Recurso compartido de la ubicación de la red SMB** | Recurso compartido de red SMB o recurso compartido de archivos de Azure que contiene los archivos de la copia de seguridad de la base de datos completa y los archivos de la copia de seguridad del registro de transacciones que Azure Database Migration Service puede usar para la migración. La cuenta de servicio que ejecuta la instancia de SQL Server de origen debe tener privilegios de lectura y escritura en este recurso compartido de red. Proporcione un FQDN o direcciones IP del servidor en el recurso compartido de red como, por ejemplo, "\\\servername.domainname.com\backupfolder" o "\\\IP address\backupfolder". Para mejorar el rendimiento, se recomienda usar una carpeta independiente para cada base de datos que se va a migrar. Puede proporcionar la ruta de acceso del recurso compartido de archivos de nivel de base de datos mediante la opción **Configuración avanzada**. Si tiene problemas para conectarse al recurso compartido de SMB, consulte el apartado sobre los[recursos compartidos de SMB](known-issues-azure-sql-db-managed-instance-online.md#smb-file-share-connectivity). |
    |**Nombre de usuario** | Asegúrese de que el usuario de Windows tiene privilegio de control total sobre el recurso compartido de red que especificó anteriormente. Azure Database Migration Service suplanta la credencial de usuario para cargar los archivos de copia de seguridad en el contenedor de Azure Storage para la operación de restauración. Si va a usar el recurso compartido de archivos de Azure, use el nombre de la cuenta de almacenamiento que lleva antepuesto AZURE\ como nombre de usuario. |
    |**Contraseña** | Contraseña del usuario. Si va a usar el recurso compartido de archivos de Azure, use una clave de cuenta de almacenamiento como contraseña. |
    |**Suscripción de la cuenta de Azure Storage** | Seleccione la suscripción que contiene la cuenta de Azure Storage. |
    |**Cuenta de Azure Storage** | Seleccione la cuenta de Azure Storage en la que DMS puede cargar los archivos de copia de seguridad del recurso compartido de red SMB y úsela para la migración de base de datos.  Para que el rendimiento de la carga de archivos sea óptimo, se recomienda seleccionar la cuenta de almacenamiento en la misma región que el servicio DMS. |

    ![Configuración de valores de migración](media/tutorial-sql-server-to-managed-instance-online/dms-configure-migration-settings.png)

    > [!NOTE]
    > Si Azure Database Migration Service muestra el error "Error del sistema 53" o "Error del sistema 57", la causa podría ser el resultado de una incapacidad de Azure Database Migration Service para acceder al recurso compartido de archivos de Azure. Si encuentra alguno de estos errores, conceda acceso a la cuenta de almacenamiento desde la red virtual mediante las instrucciones que se indican [aquí](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network).

    > [!IMPORTANT]
    > Si la funcionalidad de comprobación de bucle invertido está habilitada y el recurso compartido de archivos y SQL Server de origen están en el mismo equipo, el origen no podrá tener acceso al recurso compartido de archivos mediante el FQDN. Para corregir este problema, deshabilite la funcionalidad de comprobación de bucle invertido con las instrucciones que aparecen [aquí](https://support.microsoft.com/help/926642/error-message-when-you-try-to-access-a-server-locally-by-using-its-fqd).

2. Seleccione **Siguiente: Resumen**.

## <a name="review-the-migration-summary"></a>Examen del resumen de la migración

1. En la pantalla **Resumen**, en el cuadro de texto **Nombre de actividad**, especifique un nombre para la actividad de migración.

2. Revise y compruebe los detalles relacionados con el proyecto de migración.

    ![Resumen del proyecto de migración](media/tutorial-sql-server-to-managed-instance-online/dms-project-summary.png)

## <a name="run-and-monitor-the-migration"></a>Ejecución y supervisión de la migración

1. Seleccione **Iniciar migración**.

2. Aparece la ventana de actividad de migración que muestra el estado de migración de las bases de datos actuales. Seleccione **Actualizar** para ver la información actualizada.

   ![Actividad de migración en curso](media/tutorial-sql-server-to-managed-instance-online/dms-monitor-migration.png)

    Puede ampliar aún más las bases de datos y las categorías de inicio de sesión para supervisar el estado de migración de los respectivos objetos de servidor.

   ![Estado de la actividad de migración](media/tutorial-sql-server-to-managed-instance-online/dms-monitor-migration-extend.png)

## <a name="performing-migration-cutover"></a>Realización de la migración total

Una vez restaurada la copia de seguridad de la base de datos completa en la instancia de destino de la Instancia administrada de SQL, la base de datos estará disponible para realizar una migración total.

1. Cuando esté listo para completar la migración de la base de datos en línea, seleccione **Iniciar transición**.

2. Detenga todo el tráfico de entrada a las bases de datos de origen.

3. Realice la [copia del final del registro], proporcione el archivo de copia de seguridad en el recurso compartido de red SMB y espere a que se restaure la copia de seguridad del registro de transacciones final.

    En ese momento, verá que el valor de **Cambios pendientes** es 0.

4. Seleccione **Confirmar** y, después, **Aplicar**.

    ![Preparación de la migración completa](media/tutorial-sql-server-to-managed-instance-online/dms-complete-cutover.png)

    > [!IMPORTANT]
    > Después de la migración, la disponibilidad de SQL Managed Instance con un nivel de servicio crítico para la empresa puede tardar mucho más que para uso general, ya que se deben inicializar tres réplicas secundarias para el grupo de alta disponibilidad de AlwaysOn. La duración de esta operación depende del tamaño de los datos. Para obtener más información, consulte [Duración de las operaciones de administración](../azure-sql/managed-instance/management-operations-overview.md#duration).

5. Cuando el estado de la migración de la base de datos muestre **Completado**, conecte las aplicaciones a la nueva instancia destino de la Instancia administrada de SQL.

    ![Migración completa](media/tutorial-sql-server-to-managed-instance-online/dms-cutover-complete.png)

## <a name="additional-resources"></a>Recursos adicionales

* Para consultar un tutorial que muestra cómo migrar una base de datos a SQL Managed Instance mediante el comando T-SQL RESTORE, consulte [Restauración de una copia de seguridad para SQL Managed Instance mediante el comando Restore](../azure-sql/managed-instance/restore-sample-database-quickstart.md).
* Para más información sobre SQL Managed Instance, consulte [¿Qué es SQL Managed Instance?](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md).
* Para más información sobre cómo conectar las aplicaciones a SQL Managed Instance, consulte [Conexión de aplicaciones](../azure-sql/managed-instance/connect-application-instance.md).
