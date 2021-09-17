---
title: 'Tutorial: Migración de MySQL a Azure Database for MySQL sin conexión mediante DMS'
titleSuffix: Azure Database Migration Service
description: Aprenda a hacer una migración sin conexión de una instancia local de MySQL a Azure Database for MySQL mediante Azure Database Migration Service.
services: dms
author: sumitgaurin
ms.author: sgaur
manager: arthiaga
ms.reviewer: arthiaga
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: tutorial
ms.date: 04/11/2021
ms.openlocfilehash: cafe928ad8baed2a597bfef9bbedca8ca72e1d76
ms.sourcegitcommit: 47491ce44b91e546b608de58e6fa5bbd67315119
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2021
ms.locfileid: "122202014"
---
# <a name="tutorial-migrate-mysql-to-azure-database-for-mysql-offline-using-dms"></a>Tutorial: Migración de MySQL a Azure Database for MySQL sin conexión mediante DMS

Azure Database Migration Service se puede usar para hacer una única migración de base de datos completa de una instancia local de MySQL a [Azure Database for MySQL](../mysql/index.yml) con ayuda de funcionalidad de migración de datos de alta velocidad. En este tutorial, migrará una base de datos de ejemplo a partir de una instancia local de MySQL 5.7 a Azure Database for MySQL (v5.7) mediante una actividad de migración sin conexión en Azure Database Migration Service. Aunque en los artículos se da por supuesto que el origen es una instancia de base de datos MySQL y el destino es Azure Database for MySQL, se puede usar para migrar de una instancia de Azure Database for MySQL a otra simplemente cambiando el nombre y las credenciales del servidor de origen. También se admite la migración desde servidores MySQL de una versión inferior (v5.6 y versiones por encima) a versiones posteriores.

> [!IMPORTANT]
> Para las migraciones en línea, puede usar herramientas de código abierto, como [MyDumper/MyLoader](https://centminmod.com/mydumper.html), con [replicación de datos de entrada](../mysql/concepts-data-in-replication.md). 

> [!NOTE]
> Para obtener una versión que admite scripts basada en PowerShell de esta experiencia de migración, consulte la [migración sin conexión mediante scripts a Azure Database for MySQL](./migrate-mysql-to-azure-mysql-powershell.md).

> [!NOTE]
> Los servicios Amazon Relational Database Service (RDS) para MySQL y Amazon Aurora (basado en MySQL) también se admiten como orígenes de la migración.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Migre el esquema de la base de datos mediante la utilidad mysqldump.
> * Crear una instancia de Azure Database Migration Service.
> * Crear un proyecto de migración mediante Azure Database Migration Service.
> * Ejecutar la migración.
> * Supervisar la migración

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesita:

* Disponga de una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free).
* Disponer de una base de datos MySQL local con la versión 5.7. Si no la tiene, descargue e instale [MySQL Community edition](https://dev.mysql.com/downloads/mysql/) 5.7.
* La migración sin conexión de MySQL solo se admite en la SKU de DMS Premium.
* [Creación de una instancia en Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-portal.md). Consulte el artículo [Uso de MySQL Workbench para conectarse y consultar datos en Azure Database for MySQL](../mysql/connect-workbench.md) y obtenga más información sobre cómo conectarse y crear una base de datos mediante la aplicación Workbench. La versión de Azure Database for MySQL debe ser igual o superior a la versión local de MySQL. Por ejemplo, MySQL 5.7 admite la migración a Azure Database for MySQL 5.7 o la actualización a la versión 8. 
* Cree una instancia de Microsoft Azure Virtual Network para Azure Database Migration Service con el modelo de implementación de Azure Resource Manager, que proporciona conectividad de sitio a sitio a los servidores de origen local mediante [ExpressRoute](../expressroute/expressroute-introduction.md) o [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md). Para más información sobre la creación de una red virtual, consulte la documentación de [Virtual Network](../virtual-network/index.yml)y, especialmente, los artículos de inicio rápido con detalles paso a paso.

    > [!NOTE]
    > Durante la configuración de la red virtual, si usa ExpressRoute con emparejamiento de red a Microsoft, agregue los [puntos de conexión](../virtual-network/virtual-network-service-endpoints-overview.md) de servicio siguientes a la subred en la que se aprovisionará el servicio:
    >
    > * Punto de conexión de base de datos de destino (por ejemplo, punto de conexión de SQL, punto de conexión de Cosmos DB, etc.)
    > * Punto de conexión de Storage
    > * Punto de conexión de Service Bus
    >
    > Esta configuración es necesaria porque Azure Database Migration Service no tiene conexión a Internet.

* Asegúrese de que las reglas del grupo de seguridad de red de la red virtual no bloquean el puerto de salida 443 de ServiceTag para ServiceBus, Storage y AzureMonitor. Para más información sobre el filtrado del tráfico con grupos de seguridad de red para redes virtuales, vea el artículo [Filtrado del tráfico de red con grupos de seguridad de red](../virtual-network/virtual-network-vnet-plan-design-arm.md).
* Abra el firewall de Windows para permitir conexiones de la red virtual para que Azure Database Migration Service puede acceder al servidor MySQL de origen que, de manera predeterminada, usa el puerto TCP 3306.
* Cuando se usa un dispositivo de firewall frente a las bases de datos de origen, puede que sea necesario agregar reglas de firewall para permitir conexiones desde la red virtual para que Azure Database Migration Service acceda a las bases de datos de origen para realizar la migración.
* Cree una [regla de firewall](../azure-sql/database/firewall-configure.md) de nivel de servidor o [configure puntos de conexión de servicio de red virtual](../mysql/howto-manage-vnet-using-portal.md) para que la instancia de Azure Database for MySQL de destino permita la red virtual para que Azure Database Migration Service acceda a las bases de datos de destino.
* La versión de la instancia de MySQL de origen debe ser compatible con MySQL community edition. Para determinar la versión de la instancia de MySQL, en la utilidad de MySQL o en MySQL Workbench, ejecute el siguiente comando:

    ```
    SELECT @@version;
    ```

* Azure Database for MySQL solo es compatible con tablas de InnoDB. Para convertir tablas de MyISAM a InnoDB, consulte el artículo [Converting Tables from MyISAM to InnoDB](https://dev.mysql.com/doc/refman/5.7/en/converting-tables-to-innodb.html) (Cómo convertir tablas de MyISAM a InnoDB)
* El usuario debe tener privilegios para leer datos en la base de datos de origen.

## <a name="sizing-the-target-azure-database-for-mysql-instance"></a>Dimensionamiento de la instancia de Azure Database for MySQL de destino

Para preparar el servidor de destino de Azure Database for MySQL para cargas de datos más rápidas mediante Azure Database Migration Service, se recomiendan los siguientes parámetros de servidor y cambios de configuración. 

* max_allowed_packet: establézcalo en 1 073 741 824 (es decir, 1 GB) para evitar cualquier problema de conexión debido a filas largas. 
* slow_query_log: establézcalo en OFF para desactivar el registro de consultas lentas. Esto eliminará la sobrecarga causada por un registro de consultas lento durante las cargas de datos.
* query_store_capture_mode: establézcalo en NONE para desactivar el Almacén de consultas. Esto eliminará la sobrecarga causada por las actividades de muestreo en el Almacén de consultas.
* innodb_buffer_pool_size: innodb_buffer_pool_size solo se puede aumentar escalando el proceso para el servidor de Azure Database for MySQL. Escale verticalmente el servidor a una SKU de uso general de 64 núcleos virtuales del plan de tarifa del portal durante la migración para aumentar innodb_buffer_pool_size. 
* innodb_io_capacity e innodb_io_capacity_max: cambie a 9000 de los parámetros del servidor en Azure Portal para mejorar el uso de la E/S a fin de optimizar la velocidad de la migración.
* innodb_write_io_threads e innodb_write_io_threads: cambie a 4 desde los parámetros del servidor en Azure Portal para mejorar la velocidad de la migración.
* Escalar verticalmente la capa de almacenamiento: las IOPs para el servidor de Azure Database for MySQL aumentan progresivamente con el aumento de la capa de almacenamiento. 
    * En la opción de implementación de servidor único, para cargas más rápidas, se recomienda aumentar el nivel de almacenamiento para aumentar las E/S por segundo aprovisionadas. 
    * En la opción de implementación de servidor flexible, se recomienda escalar (aumentar o disminuir) las IOPS independientemente del tamaño de almacenamiento. 
    * Tenga en cuenta que el tamaño del almacenamiento solo se puede escalar verticalmente, no reducir.

Una vez completada la migración, puede revertir los parámetros del servidor y la configuración a los valores requeridos por la carga de trabajo. 


## <a name="migrate-database-schema"></a>Migración del esquema de base de datos

Para transferir todos los objetos de base de datos, como esquemas de tablas, índices y procedimientos almacenados, es necesario extraer el esquema de la base de datos de origen y aplicarlo a la base de datos de destino. Para extraer el esquema, puede usar mysqldump con el parámetro `--no-data`. Para ello, necesita una máquina que pueda conectarse a la base de datos de MySQL de origen y a la instancia de Azure Database for MySQL de destino.

Para exportar el esquema mediante mysqldump, ejecute el siguiente comando:

```
mysqldump -h [servername] -u [username] -p[password] --databases [db name] --no-data > [schema file path]
```

Por ejemplo:

```
mysqldump -h 10.10.123.123 -u root -p --databases migtestdb --no-data > d:\migtestdb.sql
```

Para importar el esquema a la instancia de Azure Database for MySQL de destino, ejecute el siguiente comando:

```
mysql.exe -h [servername] -u [username] -p[password] [database]< [schema file path]
 ```

Por ejemplo:

```
mysql.exe -h mysqlsstrgt.mysql.database.azure.com -u docadmin@mysqlsstrgt -p migtestdb < d:\migtestdb.sql
 ```

Si el esquema tiene claves externas o desencadenadores, la tarea de migración regulará la carga de datos en paralelo durante la migración. No es necesario descartar las claves externas ni los desencadenadores durante la migración del esquema.

[!INCLUDE [resource-provider-register](../../includes/database-migration-service-resource-provider-register.md)]

## <a name="create-a-database-migration-service-instance"></a>Creación de una instancia de Azure Database Migration Service

1. En Azure Portal, seleccione + **Crear un recurso**, busque Azure Database Migration Service y, después, seleccione **Azure Database Migration Service** en la lista desplegable.

    ![Azure Marketplace](media/tutorial-mysql-to-azure-mysql-offline-portal/03-dms-portal-marketplace.png)

2. En la pantalla **Azure Database Migration Service**, seleccione **Crear**.

    ![Creación de una instancia de Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/04-dms-portal-marketplace-create.png)
  
3. En la pantalla **Crear el servicio de migración**, especifique un nombre para el servicio, la suscripción y un grupo de recursos nuevo o existente.

4. Seleccione un plan de tarifa y pase a la pantalla de redes. La funcionalidad de migración sin conexión solo está disponible en el plan de tarifa Premium.

    Para más información sobre los costos y planes de tarifa, vea la [página de precios](https://aka.ms/dms-pricing).

    ![Configuración básica de Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/05-dms-portal-create-basic.png)

5. Seleccione una red virtual existente en la lista o proporcione el nombre de la nueva red virtual que se va a crear. Acceda a la pantalla de revisión y creación. Como alternativa, puede agregar etiquetas al servicio mediante la pantalla de etiquetas.

    La red virtual proporciona a Azure Database Migration Service acceso al servidor SQL Server de origen y a la instancia de Azure SQL Database de destino.

    ![Configuración de red de Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/06-dms-portal-create-networking.png)

    Para más información sobre cómo crear una red virtual en Azure Portal, consulte el artículo [Creación de una red virtual con Azure Portal](../virtual-network/quick-create-portal.md).

6. Revise las configuraciones y seleccione **Crear** para crear el servicio.
    
    ![Creación de Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/07-dms-portal-create-submit.png)

## <a name="create-a-migration-project"></a>Creación de un proyecto de migración

Después de crear el servicio, búsquelo en Azure Portal, ábralo y cree un proyecto de migración.  

1. En Azure Portal, seleccione **Todos los servicios**, busque Azure Database Migration Service y, luego, elija **Azure Database Migration Services**.

    ![Búsqueda de todas las instancias de Azure Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/08-01-dms-portal-search-service.png)

2. Seleccione la instancia del servicio de migración en los resultados de búsqueda y seleccione **Nuevo proyecto de migración**.
    
    ![Creación de un proyecto de migración](media/tutorial-mysql-to-azure-mysql-offline-portal/08-02-dms-portal-new-project.png)

3. En la pantalla **Nuevo proyecto de migración**, especifique un nombre para el proyecto. En el cuadro de selección **Tipo de servidor de origen**, seleccione **MySQL** y, en el cuadro selección **Tipo de servidor de destino**, seleccione **Azure Database for MySQL**. Por último, en el cuadro de selección **Tipo de actividad de migración**, seleccione **Migración de datos**. Seleccione **Crear y ejecutar una actividad**.

    ![Creación de un proyecto de Database Migration Service](media/tutorial-mysql-to-azure-mysql-offline-portal/09-dms-portal-project-mysql-create.png)

    > [!NOTE]
    > Como alternativa, puede elegir **Crear solo un proyecto** para crear el proyecto de migración ahora y ejecutar la migración más adelante.

## <a name="configure-migration-project"></a>Configuración del proyecto de migración

1. En la pantalla **Seleccionar origen**, especifique los detalles de conexión de la instancia de origen de MySQL Server y seleccione **Siguiente: Seleccionar destino>>** .

    ![Pantalla Agregar detalles de origen](media/tutorial-mysql-to-azure-mysql-offline-portal/10-dms-portal-project-mysql-source.png)

2. En la pantalla **Seleccionar destino**, especifique los detalles de conexión para la instancia de destino de Azure Database for MySQL y seleccione **Siguiente: Seleccionar bases de datos>>** .

    ![Pantalla Agregar detalles de destino](media/tutorial-mysql-to-azure-mysql-offline-portal/11-dms-portal-project-mysql-target.png)

3. En la pantalla **Seleccionar bases de datos**, asigne el origen a la base de datos de destino para la migración y seleccione **Siguiente: Configurar los valores de migración>>** . Puede seleccionar la opción **Make Source Server Readonly** (Hacer que el servidor de origen sea de solo lectura) para que el origen sea de solo lectura, pero tenga en cuenta que esta es una configuración de nivel de servidor. Si la opción se selecciona, establecerá todo el servidor como de solo lectura, no únicamente las bases de datos seleccionadas.
    
    Si la base de datos de destino contiene el mismo nombre de base de datos que la de origen, Azure Database Migration Service selecciona la base de datos de destino de forma predeterminada.
    ![Pantalla Seleccionar detalles de la base de datos](media/tutorial-mysql-to-azure-mysql-offline-portal/12-dms-portal-project-mysql-select-db.png)
    
    > [!NOTE] 
    > Aunque puede seleccionar varias bases de datos en este paso, hay límites en cuanto al número y la rapidez con que se pueden migrar las bases de datos de esta manera, ya que cada base de datos compartirá recursos de proceso. Con la configuración predeterminada de la SKU Premium, cada tarea de migración intentará migrar dos tablas a la vez. Estas tablas pueden proceder de cualquiera de las bases de datos seleccionadas. Si el proceso no es lo suficientemente rápido, puede dividir las actividades de migración de bases de datos en diferentes tareas de migración y escalarlas en varios servicios. Además, hay un límite de diez instancias de Azure Database Migration Service por suscripción y región.
    > Para un control más detallado sobre el rendimiento de la migración y la paralelización, consulte el artículo sobre [PowerShell: Ejecución de una migración sin conexión desde la base de datos MySQL a Azure Database for MySQL mediante DMS](./migrate-mysql-to-azure-mysql-powershell.md).

4. En la pantalla **Configurar los valores de la migración**, seleccione las tablas que formarán parte de la migración. A continuación, seleccione **Siguiente: Resumen>>** . Si las tablas de destino contienen datos, no se seleccionarán de forma predeterminada, pero puede seleccionarlos explícitamente y se truncarán antes de iniciar la migración.

    ![Pantalla Seleccionar tablas](media/tutorial-mysql-to-azure-mysql-offline-portal/13-dms-portal-project-mysql-select-tbl.png)

5. En la pantalla **Resumen**, en el cuadro de texto **Nombre de la actividad**, especifique un nombre para la actividad de migración y, a continuación, revise el resumen para asegurarse de que los detalles de origen y de destino coincidan con los que especificó anteriormente.

    ![Resumen del proyecto de migración](media/tutorial-mysql-to-azure-mysql-offline-portal/14-dms-portal-project-mysql-activity-summary.png)

6. Seleccione **Iniciar migración**. Aparecerá la ventana de actividad de migración. El **estado** de la actividad es **Inicializando**. El **estado** cambia a **En ejecución** cuando se inician las migraciones de tabla.
 
     ![Ejecución de la migración](media/tutorial-mysql-to-azure-mysql-offline-portal/15-dms-portal-project-mysql-running.png)

## <a name="monitor-the-migration"></a>Supervisión de la migración

1. En la pantalla de la actividad de migración, seleccione **Actualizar** para ver la información actualizada y el progreso en relación con el número de tablas completadas. 

2. Haga clic en el nombre de la base de datos en la pantalla de actividad para ver el estado de cada tabla a medida que se migran. Seleccione **Actualizar** para ver la información actualizada. 

     ![Supervisión de la migración](media/tutorial-mysql-to-azure-mysql-offline-portal/16-dms-portal-project-mysql-monitor.png)

## <a name="complete-the-migration"></a>Completar la migración

1. En la pantalla de la actividad de migración, seleccione **Actualizar** para actualizar la vista hasta que el valor de **Estado** de la migración sea **Completado**.

    ![Completar migración](media/tutorial-mysql-to-azure-mysql-offline-portal/17-dms-portal-project-mysql-complete.png)

## <a name="post-migration-activities"></a>Actividades posteriores a la migración

La migración total en una migración sin conexión es un proceso dependiente de la aplicación que queda fuera del alcance de este documento, pero se recomienda llevar a cabo las siguientes actividades después de la migración:

1. Crear inicios de sesión, roles y permisos según los requisitos de la aplicación.
2. Volver a crear todos los desencadenadores de la base de datos de destino que se retiraron en el paso previo a la migración.
3. Hacer pruebas de seguridad de la aplicación en la base de datos de destino para verificar la migración. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando Database Migration Service, puede eliminar el servicio mediante los pasos siguientes:

1. En Azure Portal, seleccione **Todos los servicios**, busque Azure Database Migration Service y, luego, elija **Azure Database Migration Services**.

    ![Buscar todas las instancias de DMS](media/tutorial-mysql-to-azure-mysql-offline-portal/08-01-dms-portal-search-service.png)

2. Seleccione la instancia del servicio de migración en los resultados de la búsqueda. A continuación, seleccione **Eliminar servicio**.
    
    ![Elimine el servicio de migración.](media/tutorial-mysql-to-azure-mysql-offline-portal/18-dms-portal-delete.png)

3. En el cuadro de diálogo de confirmación, escriba el nombre del servicio en el cuadro de texto **TYPE THE DATABASE MIGRATION SERVICE** (ESCRIBIR EL NOMBRE DE DATABASE MIGRATION SERVICE) y seleccione **Eliminar**.

    ![Confirmar la eliminación del servicio de migración](media/tutorial-mysql-to-azure-mysql-offline-portal/19-dms-portal-deleteconfirm.png)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre los problemas conocidos y las limitaciones al llevar a cabo migraciones mediante DMS, consulte el artículo [Solucionar problemas y errores comunes de Azure Database Migration Service](./known-issues-troubleshooting-dms.md).
* Para solucionar problemas de conectividad de la base de datos de origen al usar DMS, consulte el artículo [Solución de errores de DMS al conectarse a las bases de datos de origen](./known-issues-troubleshooting-dms-source-connectivity.md).
* Para más información acerca de Azure Database Migration Service, consulte el artículo [¿Qué es Azure Database Migration Service?](./dms-overview.md)
* Para información sobre Azure Database for MySQL, consulte el artículo [¿Qué es Azure Database for MySQL?](../mysql/overview.md)
* Para obtener directrices sobre el uso de DMS a través de PowerShell, consulte el artículo sobre [PowerShell: Ejecución de una migración sin conexión desde la base de datos MySQL a Azure Database for MySQL mediante DMS](./migrate-mysql-to-azure-mysql-powershell.md).
