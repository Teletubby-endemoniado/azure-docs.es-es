---
title: ¿Qué es el nivel de servicio Hiperescala?
description: En este artículo se describe el nivel de servicio Hiperescala en el modelo de compra basado en núcleo virtual en Azure SQL Database, y se explica la diferencia entre los niveles de servicio Uso general y Crítico para la empresa.
services: sql-database
ms.service: sql-database
ms.subservice: service-overview
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: dimitri-furman
ms.author: dfurman
ms.reviewer: mathoma
ms.date: 9/9/2021
ms.openlocfilehash: f5cc4321f49a2cee75f8111bd975f750f075680f
ms.sourcegitcommit: 61e7a030463debf6ea614c7ad32f7f0a680f902d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2021
ms.locfileid: "129094184"
---
# <a name="hyperscale-service-tier"></a>Nivel de servicio Hiperescala
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Azure SQL Database se basa en la arquitectura del motor de base de datos de SQL Server que se ajusta al entorno en la nube, con el fin de garantizar una disponibilidad del 99,99 % incluso en los casos de error de la infraestructura. Hay tres modelos de arquitectura que se usan en Azure SQL Database:

- De uso general/Estándar
- Hiperescala
- Crítico para la empresa/Premium

El nivel de servicio Hiperescala de Azure SQL Database es el nivel de servicio más reciente en el modelo de compra basado en núcleo virtual. Este nivel de servicio es un nivel altamente escalable de almacenamiento y de rendimiento de proceso que aprovecha la arquitectura de Azure para escalar horizontalmente el almacenamiento y los recursos de proceso para una base de datos de Azure SQL considerablemente más allá de los límites disponibles para los niveles de servicio Uso general y Crítico para la empresa.

> [!NOTE]
>
> - Para más información acerca de los niveles de servicios Uso general y Crítico para la empresa en el modelo de compra basado en núcleo virtual, consulte los niveles de servicio [Uso general](service-tier-general-purpose.md) y [Crítico para la empresa](service-tier-business-critical.md). Para ver una comparación entre el modelo de compra basado en núcleo virtual y el modelo de compra basado en DTU, consulte [Modelos de compra y recursos de Azure SQL Database](purchasing-models.md).
> - Actualmente, el nivel de servicio Hiperescala solo está disponible para Azure SQL Database, pero no para Azure SQL Managed Instance.

## <a name="what-are-the-hyperscale-capabilities"></a>¿Cuáles son las funcionalidades de Hiperescala?

El nivel de servicio Hiperescala en Azure SQL Database proporciona las siguientes funcionalidades adicionales:

- Compatibilidad con bases de datos con un tamaño de hasta 100 TB
- Copias de seguridad de base de datos casi instantáneas (basadas en las instantáneas almacenadas en Azure Blob Storage) independientemente del tamaño sin efecto de la E/S en recursos de proceso  
- Restauraciones rápidas de base de datos (basadas en instantáneas de archivos) en minutos en lugar de horas o días (no el tamaño de la operación de datos)
- Mayor rendimiento general debido a un mayor rendimiento de los registros de transacciones y tiempos más rápidos de confirmación de estas, independientemente de los volúmenes de datos
- Rápido escalado horizontal: puede aprovisionar una o varias [réplicas de solo lectura](service-tier-hyperscale-replicas.md) para la descarga de la carga de trabajo de lectura y para su uso como esperas activas
- Rápido escalado vertical: en tiempo constante, puede escalar verticalmente los recursos de proceso para dar cabida a cargas de trabajo pesadas cuando sea necesario y, después, reducir verticalmente los recursos de proceso cuando no sean necesarios.

El nivel de servicio Hiperescala elimina muchos de los límites prácticos que tradicionalmente se ven en las bases de datos en la nube. Donde la mayoría de las otras bases de datos están limitados por los recursos disponibles en un único nodo, las bases de datos en el nivel de servicio Hiperescala no tienen límites de este tipo. Con su arquitectura de almacenamiento flexible, el almacenamiento crece a medida que sea necesario. De hecho, las bases de datos de Hiperescala no se crean con un tamaño máximo definido. Una base de datos de hiperescala aumenta según sea necesario, y se le cobrará solo la capacidad que use. Para cargas de trabajo de lectura intensiva, el nivel de servicio Hiperescala proporciona rápida escalabilidad horizontal mediante el aprovisionamiento de réplicas adicionales según sea necesario para descargar las cargas de trabajo de lectura.

Además, el tiempo necesario para crear copias de seguridad de bases de datos o para escalar o reducir verticalmente ya no está ligado al volumen de los datos en la base de datos. Pueden crearse copias de seguridad de las bases de datos de hiperescala de manera prácticamente instantánea. También puede escalar o reducir verticalmente una base de datos de decenas de terabytes en cuestión de minutos. Esta funcionalidad le libra de la preocupación de estar atado por las opciones de la configuración inicial.

Para más información sobre los tamaños de proceso para el nivel de servicio Hiperescala, consulte [Características de los niveles de servicios](service-tiers-vcore.md#service-tiers).

## <a name="who-should-consider-the-hyperscale-service-tier"></a>Quién debe tener en cuenta el nivel de servicio Hiperescala

El nivel de servicio Hiperescala está pensado para la mayoría de las cargas de trabajo empresariales, ya que proporciona una gran flexibilidad y un alto rendimiento con recursos de proceso y almacenamiento escalables de forma independiente. Con la capacidad de almacenamiento con escalabilidad automática hasta 100 TB, es una excelente opción para los clientes que:

- Tienen bases de datos de gran tamaño en el entorno local y desean modernizar sus aplicaciones pasándose a la nube
- Ya están en la nube y están limitados por las restricciones de tamaño máximo de base de datos de otros niveles de servicio (1-4 TB)
- Tienen bases de datos más pequeñas, pero requieren un escalado de proceso vertical y horizontal rápido, alto rendimiento, copia de seguridad instantánea y una rápida restauración de bases de datos.

El nivel de servicio Hiperescala admite una amplia gama de cargas de trabajo de SQL Server, desde OLTP puro hasta análisis puro, pero está optimizado principalmente para cargas de trabajo OLTP y de procesamiento híbrido transaccional y analítico (HTAP).

> [!IMPORTANT]
> Los grupos elásticos no admiten el nivel de servicio Hiperescala.

## <a name="hyperscale-pricing-model"></a>Modelo de precios de Hiperescala

El nivel de servicio Hiperescala solo está disponible en el [modelo de núcleo virtual](service-tiers-vcore.md). Para alinearse con la nueva arquitectura, el modelo de precios es ligeramente diferente de los niveles de servicio Se uso general o Crítico para la empresa:

- **Proceso**:

  El precio de la unidad de proceso de Hiperescala es por réplica. El precio de la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) se aplica automáticamente a las réplicas de alta disponibilidad y con nombre. De manera predeterminada, se crea una réplica principal y una [réplica de alta disponibilidad](service-tier-hyperscale-replicas.md) secundaria por base de datos de Hiperescala.  Los usuarios pueden ajustar el número total de réplicas de alta disponibilidad de 0 a 4, en función del [Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/azure-sql-database/) necesario. 

- **Almacenamiento**:

  No es necesario especificar el tamaño máximo de datos al configurar una base de datos Hiperescala. En el nivel Hiperescala, se le cobra por el almacenamiento de su base de datos según la asignación real. El almacenamiento se asigna automáticamente entre 40 GB y 100 TB, en incrementos de 10 GB. Si es necesario, pueden crecer simultáneamente varios archivos de datos. Se crea una base de datos de Hiperescala con un tamaño inicial de 10 GB y empieza a crecer 10 GB cada 10 minutos, hasta que alcanza el tamaño de 40 GB.

Para más información sobre los precios de Hiperescala, consulte [Precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/)

## <a name="distributed-functions-architecture"></a>Arquitectura de funciones distribuidas

A diferencia de los motores de base de datos tradicionales, que han centralizada todas las funciones de administración de datos en una ubicación o proceso (incluso las llamadas bases de datos distribuidas en producción actualmente tienen varias copias de un motor de datos monolítico), una base de datos de hiperescala separa el motor de procesamiento de consultas, donde la semántica de los diversos motores de datos difieren, de los componentes que proporcionan almacenamiento a largo plazo y durabilidad de los datos. De este modo, la capacidad de almacenamiento se puede escalar horizontalmente fácilmente en cuanto sea necesario (el destino inicial es de 100 TB). Las réplicas de alta disponibilidad y con nombre comparten los mismos componentes de almacenamiento, por lo que no se requiere ninguna copia de datos para poner en marcha una nueva réplica.

El siguiente diagrama ilustra los diferentes tipos de nodos en una base de datos de hiperescala:

![arquitectura](./media/service-tier-hyperscale/hyperscale-architecture.png)

Una base de datos de Hiperescala contiene los distintos tipos de componentes siguientes:

### <a name="compute"></a>Proceso

El nodo de ejecución es la ubicación en la que reside el motor relacional. Es donde se produce el procesamiento de lenguajes, consultas y transacciones. Todas las interacciones de usuario con una base de datos de hiperescala se producen a través de estos nodos de ejecución. Los nodos de ejecución tienen memorias caché basadas en SSD (con la etiqueta RBPEX, extensión del grupo de búferes resistentes, en el diagrama anterior) para minimizar el número de recorridos de ida y vuelta de red necesarios para capturar una página de datos. Hay un nodo de ejecución principal donde se procesan todas las transacciones y las cargas de trabajo de lectura y escritura. Hay uno o varios nodos de ejecución secundarios que actúan como nodos en espera activa para fines de conmutación por error, además de actuar como nodos de ejecución de solo lectura para la descarga de cargas de trabajo de lectura (si se desea esta funcionalidad).

El motor de base de datos que se ejecuta en nodos de ejecución de Hyperscale es el mismo que en otros niveles de servicio de Azure SQL Database. Cuando los usuarios interactúan con el motor de base de datos en nodos de ejecución de Hyperscale, el área expuesta compatible y el comportamiento del motor son los mismos que en otros niveles de servicio, con la excepción de [limitaciones conocidas](#known-limitations).

### <a name="page-server"></a>Servidor de páginas

Los servidores de páginas son sistemas que representan un motor de almacenamiento escalado horizontalmente.  Cada servidor de páginas es responsable de un subconjunto de las páginas en la base de datos.  Nominalmente, cada servidor de páginas controla hasta 128 GB o hasta 1 TB de datos. No se comparten datos en más de un servidor de páginas (fuera de las réplicas del servidor de páginas que se mantienen por motivos de redundancia y disponibilidad). El trabajo de un servidor de páginas es servir las páginas de la base de datos a los nodos de ejecución a petición, y conservar las páginas actualizadas a medida que las transacciones actualizan los datos. Los servidores de páginas se mantienen actualizados reproduciendo entradas del registro de transacciones del servicio de registro. Los servidores de páginas también mantienen memorias caché de cobertura basadas en SSD para mejorar el rendimiento. El almacenamiento a largo plazo de las páginas de datos se mantiene en Azure Storage para aumentar la confiabilidad.

### <a name="log-service"></a>Servicio de registros

El servicio de registros acepta entradas de registro de transacciones desde la réplica de proceso principal, las conserva en una memoria caché duradera y las reenvía al resto de las réplicas de proceso (para que puedan actualizar sus memorias caché), así como los servidores de páginas pertinentes, para que los datos puedan actualizarse allí. De este modo, todos los cambios en los datos desde la réplica de proceso principal se propagan mediante el servicio de registros a todas las réplicas de proceso secundarias y los servidores de página. Por último, las entradas de registro de transacciones se insertan en el almacenamiento a largo plazo en Azure Storage, que es un repositorio de almacenamiento casi infinito. Este mecanismo elimina la necesidad de truncamiento frecuente del registro. El servicio de registro también tiene memoria local y cachés SSD para acelerar el acceso a las entradas de registro.

### <a name="azure-storage"></a>Almacenamiento de Azure

Azure Storage contiene todos los archivos de datos de una base de datos. Los servidores de página mantienen los archivos de datos en Azure Storage actualizados. Este almacenamiento se usa con fines de copia de seguridad, así como para la replicación entre regiones de Azure. Las copias de seguridad se implementan mediante instantáneas de almacenamiento de archivos de datos. Las operaciones de restauración mediante instantáneas son rápidas independientemente del tamaño de los datos. Los datos se pueden restaurar a cualquier punto en el tiempo dentro del período de retención de copia de seguridad de la base de datos.

## <a name="backup-and-restore"></a>Copia de seguridad y restauración

Las copias de seguridad están basadas en instantáneas de archivos y, por tanto, se realizan de forma prácticamente instantánea. La separación del almacenamiento y los procesos permite insertar la operación de copia de seguridad y restauración en la capa de almacenamiento para reducir la carga de procesamiento en replica de proceso principal. Como resultado, la copia de seguridad de base de datos no afecta al rendimiento del nodo de ejecución principal. De forma similar, la recuperación a un momento dado (PITR) se realiza revirtiendo las instantáneas de los archivos y, por tanto, no depende de la operación de datos. La restauración de una base de datos de Hiperescala en la misma región de Azure es una operación de tiempo constante, e incluso las bases de datos de varios terabytes se pueden restaurar en minutos en lugar de horas o días. La creación de bases de datos nuevas mediante la restauración de una copia de seguridad existente también aprovecha esta característica: la creación de copias de base de datos para fines de desarrollo o pruebas, incluso de bases de datos de varios terabytes, es factible en cuestión de minutos.

Para la restauración geográfica de bases de datos de Hiperescala, consulte [Restauración de una base de datos de Hiperescala en una región diferente](#restoring-a-hyperscale-database-to-a-different-region).

## <a name="scale-and-performance-advantages"></a>Ventajas de escala y rendimiento

Con la capacidad de aumentar o disminuir rápidamente los nodos de ejecución adicionales de solo lectura, la arquitectura de Hiperescala permite obtener importantes funcionalidades de escalado de lectura y también puede liberar el nodo de ejecución principal para atender más solicitudes de escritura. Además, los nodos de ejecución se pueden escalar o reducir verticalmente rápidamente debido a la arquitectura de almacenamiento compartido de la arquitectura de Hiperescala.

## <a name="create-a-hyperscale-database"></a>Creación de una base de datos de Hiperescala

Las bases de datos de Hiperescala se pueden crear desde [Azure Portal](https://portal.azure.com), [T-SQL](/sql/t-sql/statements/create-database-transact-sql), [PowerShell](/powershell/module/azurerm.sql/new-azurermsqldatabase) o la [CLI](/cli/azure/sql/db#az_sql_db_create). Las bases de datos de Hiperescala solo están disponibles cuando se usa el [modelo de compra basado en núcleo virtual](service-tiers-vcore.md).

El siguiente comando de Transact-SQL crea una base de datos de Hiperescala. Debe especificar tanto la edición como el servicio objetivo en la instrucción `CREATE DATABASE`. Consulte los [límites de recursos](./resource-limits-vcore-single-databases.md#hyperscale---provisioned-compute---gen4) para obtener una lista de los objetivos de servicio válidos.

```sql
-- Create a Hyperscale Database
CREATE DATABASE [HyperscaleDB1] (EDITION = 'Hyperscale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```

Esto creará una base de datos Hiperescala en el hardware de Gen5 con cuatro núcleos.

## <a name="upgrade-existing-database-to-hyperscale"></a>Actualización de una base de datos existente a Hiperescala

Puede mover las bases de datos de Azure SQL Database existentes a Hiperescala con [Azure Portal](https://portal.azure.com), [T-SQL](/sql/t-sql/statements/alter-database-transact-sql), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase) o la [CLI](/cli/azure/sql/db#az_sql_db_update). Actualmente, esta migración es unidireccional. No se pueden trasladar bases de datos de Hiperescala a otro nivel de servicio, excepto mediante la exportación e importación de datos. Para las pruebas de concepto (POC), se recomienda que haga una copia de las bases de datos de producción y la migre a Hiperescala. La migración de una base de datos de Azure SQL Database existente al nivel Hiperescala tiene el tamaño de una operación de datos.

El siguiente comando de T-SQL traslada una base de datos al nivel de servicio Hiperescala. Debe especificar tanto la edición como el servicio objetivo en la instrucción `ALTER DATABASE`.

```sql
-- Alter a database to make it a Hyperscale Database
ALTER DATABASE [DB2] MODIFY (EDITION = 'Hyperscale', SERVICE_OBJECTIVE = 'HS_Gen5_4');
GO
```

> [!NOTE]
> Para trasladar una base de datos que forma parte de una relación de [replicación geográfica](active-geo-replication-overview.md), ya sea como principal o como secundaria, a Hiperescala, debe detener la replicación. Las bases de datos de [un grupo de conmutación por error](auto-failover-group-overview.md) deben quitarse primero del grupo.
>
> Una vez que se ha trasladado una base de datos a Hiperescala, puede crear una nueva replicación geográfica de Hiperescala de esa base de datos. La replicación geográfica en Hiperescala está en versión preliminar con ciertas [limitaciones](active-geo-replication-overview.md).

## <a name="database-high-availability-in-hyperscale"></a>Alta disponibilidad de la base de datos en Hiperescala

Como en todos los demás niveles de servicio, Hiperescala garantiza la durabilidad de los datos para las transacciones confirmadas, independientemente de la disponibilidad de la réplica de proceso. El grado de tiempo de inactividad debido a que la réplica principal no esté disponible depende del tipo de conmutación por error (planeada frente a no planeada) y de la presencia de al menos una réplica de alta disponibilidad. En una conmutación por error planeada (es decir, un evento de mantenimiento), el sistema crea la nueva réplica principal antes de iniciar la conmutación por error, o bien usa una réplica de alta disponibilidad existente como destino de la conmutación por error. En una conmutación por error no planeada (es decir, un error de hardware en la réplica principal), el sistema usa una réplica del alta disponibilidad como destino de la conmutación por error, si existe, o crea una réplica principal a partir del grupo de capacidad de proceso disponible. En el último caso, la duración del tiempo de inactividad es mayor debido a pasos adicionales necesarios para crear la nueva réplica principal.

Para el contrato de nivel de servicio de Hiperescala, consulte [Contrato de nivel de servicio para Azure SQL Database](https://azure.microsoft.com/support/legal/sla/azure-sql-database).

## <a name="disaster-recovery-for-hyperscale-databases"></a>Recuperación ante desastres para bases de datos Hiperescala

### <a name="restoring-a-hyperscale-database-to-a-different-region"></a>Restauración de una base de datos de Hiperescala en una región diferente

Si necesita restaurar una base de datos Hiperescala de Azure SQL Database en una región distinta a donde se hospeda actualmente —como parte de una operación de recuperación ante desastres, una exploración en profundidad, una reubicación o cualquier otro motivo—, el método principal es realizar una restauración geográfica de la base de datos. Esto implica exactamente los mismos pasos que seguiría para restaurar cualquier otra base de datos de SQL Database en una región diferente:

1. Cree un [servidor](logical-servers.md) en la región de destino si ahí todavía no tiene un servidor adecuado.  Este servidor debe pertenecer a la misma suscripción que el servidor original (origen).
2. Siga las instrucciones del tema [Restauración geográfica](./recovery-using-backups.md#geo-restore) de la página dedicada a la restauración de una base de datos de Azure SQL a partir de copias de seguridad automáticas.

> [!NOTE]
> Debido a que el origen y el destino están en regiones separadas, la base de datos no puede compartir el almacenamiento de instantáneas con la base de datos de origen como hace en las restauraciones no geográficas, que se completan rápidamente independientemente del tamaño de la base de datos. En el caso de una restauración geográfica de una base de datos Hiperescala, será una operación de tamaño de datos, incluso si el destino está en la región emparejada del almacenamiento con replicación geográfica. Por esto, la duración de una restauración geográfica será proporcional al tamaño de la base de datos que se está restaurando. Si el destino se encuentra en la región emparejada, la transferencia de datos se realizará dentro de una región, lo que será mucho más rápido que una transferencia entre regiones, pero aun así será una operación de tamaño de datos.

## <a name="available-regions"></a><a name=regions></a>Regiones disponibles

El nivel Hiperescala de Azure SQL Database está habilitado en la gran mayoría de las regiones de Azure. Si desea crear una base de datos Hiperescala en una región donde este nivel no está habilitado de forma predeterminada, puede enviar una solicitud de incorporación a través de Azure Portal. Para obtener instrucciones, consulte [Solicitud de aumentos de cuota para Azure SQL Database](quota-increase-request.md). Cuando envíe la solicitud, utilice las siguientes directrices:

- Use el tipo de cuota [Acceso a la región](quota-increase-request.md#region) de SQL Database.
- En la descripción, agregue la SKU o el total de núcleos de proceso, incluidas las réplicas de alta disponibilidad y con nombre, e indique que está solicitando capacidad de Hiperescala.
- Especifique también una proyección del tamaño total, en TB, de todas las bases de datos a lo largo del tiempo.


## <a name="known-limitations"></a>Restricciones conocidas

Estas son las limitaciones actuales para el nivel de servicio Hiperescala en disponibilidad general.  Estamos trabajando activamente para eliminar tantas limitaciones como sea posible.

| Incidencia | Descripción |
| :---- | :--------- |
| En el panel Administrar copias de seguridad de un servidor no se muestran las bases de datos Hiperescala, que se filtran desde la vista.  | Hiperescala tiene un método independiente para administrar las copias de seguridad y, por tanto, la configuración de la retención de copias de seguridad correspondiente a la retención a largo plazo y a un momento dado no se aplican. En consecuencia, las bases de datos de Hiperescala no aparecen en el panel Administración de copias de seguridad.<br><br>En el caso de las bases de datos migradas a Hiperescala desde otros niveles de servicio de Azure SQL Database, se conservan copias de seguridad previas a la migración durante el período de [retención de copias de seguridad](automated-backups-overview.md#backup-retention) de la base de datos de origen. Estas copias de seguridad se pueden utilizar para [restaurar](recovery-using-backups.md#programmatic-recovery-using-automated-backups) la base de datos de origen a un momento anterior a la migración.|
| Restauración a un momento dado | No se puede restaurar una base de datos de Hiperescala en una base de datos que no sea de Hiperescala, ni se puede restaurar una base de datos que no sea de Hiperescala en una base de datos de Hiperescala. Cuando se trata de una base de datos que no es de Hiperescala, pero que ha migrado a Hiperescala cambiando su nivel de servicio, la restauración a un punto en el tiempo anterior a la migración y dentro del período de retención de la copia de seguridad de la base de datos es posible [con programación](recovery-using-backups.md#programmatic-recovery-using-automated-backups). La base de datos restaurada no será de Hiperescala. |
| Al cambiar el nivel de servicio de Azure SQL Database a Hiperescala, se producirá un error en la operación si la base de datos tiene archivos de datos de más de 1 TB | En algunos casos, es posible solucionar este problema si se [reducen](file-space-manage.md#shrinking-data-files) los archivos de gran tamaño para que tengan menos de 1 TB antes de intentar cambiar el nivel de servicio a Hiperescala. Use la siguiente consulta para determinar el tamaño actual de los archivos de base de datos. `SELECT file_id, name AS file_name, size * 8. / 1024 / 1024 AS file_size_GB FROM sys.database_files WHERE type_desc = 'ROWS'`;|
| Instancia administrada de SQL | Actualmente Azure SQL Managed Instance no es compatible con las bases de datos de Hiperescala. |
| Grupos elásticos |  Los grupos elásticos no son compatibles actualmente con Hiperescala.|
| La migración a Hiperescala actualmente es una operación unidireccional. | Una vez que una base de datos se migra a Hiperescala, no puede migrarse directamente a un nivel de servicio que no sea Hiperescala. En la actualidad, la única manera de migrar una base de datos de Hiperescala a un recursos que no sea de Hiperescala es exportar o importar mediante un archivo bacpac u otras tecnologías de movimiento de datos (copia masiva, Azure Data Factory, Azure Databricks, SSIS, etc.) No se admite la exportación o importación de bacpac desde Azure Portal, desde PowerShell mediante [New-AzSqlDatabaseExport](/powershell/module/az.sql/new-azsqldatabaseexport) o [New-AzSqlDatabaseImport](/powershell/module/az.sql/new-azsqldatabaseimport), desde la CLI de Azure con [az sql db export](/cli/azure/sql/db#az_sql_db_export) y [az sql db import](/cli/azure/sql/db#az_sql_db_import) ni desde la [API REST](/rest/api/sql/). Se admite la importación y exportación de bases de datos de Hiperescala (200 GB como máximo) mediante SSMS y [SqlPackage](/sql/tools/sqlpackage) versión 18.4 y posteriores. En el caso de las bases de datos de mayor tamaño, la importación y exportación de bacpac puede tardar mucho tiempo y producir errores por diversos motivos.|
| Migración de bases de datos con objetos OLTP en memoria | Hiperescala admite un subconjunto de objetos OLTP en memoria, incluidos los tipos de tablas optimizadas para memoria, las variables de tablas y los módulos compilados de forma nativa. Sin embargo, cuando hay presente cualquier tipo de objeto OLTP en memoria en la base de datos que se está migrando, no se admite la migración desde los niveles de servicio Premium y Crítico para la empresa a Hiperescala. Para migrar este tipo de base de datos a Hiperescala, se deben quitar todos los objetos OLTP en memoria y sus dependencias. Después de migrar la base de datos, estos objetos se pueden volver a crear. En este momento no se admiten tablas optimizadas para memoria, duraderas y no duraderas, en Hiperescala, y deben cambiarse a tablas de disco.|
| Replicación geográfica  | La [replicación geográfica](active-geo-replication-overview.md) en Hiperescala está ahora en versión preliminar pública. |
| Características de bases de datos inteligentes | Con la excepción de la opción "Forzar plan", todas las demás opciones de ajuste automático no se admiten aún en Hiperescala: puede parecer que las opciones están habilitadas, pero no se realizarán recomendaciones ni acciones. |
| Información del rendimiento de las consultas | La información de rendimiento de consultas no se admite actualmente para las bases de datos de Hiperescala. |
| Reducir base de datos | DBCC SHRINKDATABASE o DBCC SHRINKFILE no se admite actualmente con las bases de datos de Hiperescala. |
| Comprobación de la integridad de la base de datos | DBCC CHECKDB no se admite actualmente con las bases de datos de Hiperescala. DBCC CHECKFILEGROUP y DBCC CHECKTABLE se pueden usar como solución alternativa. Consulte [Integridad de datos en Azure SQL Database](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/) para más información sobre la administración de esta en Azure SQL Database. |
| Trabajos elásticos | No se admite el uso de una base de datos de Hiperescala como base de datos de trabajo. Sin embargo, los trabajos elásticos pueden tener como destino bases de datos de Hiperescala, de la misma manera que cualquier otra base de datos de Azure SQL. |

## <a name="next-steps"></a>Pasos siguientes

- Para consultar preguntas frecuentes sobre Hiperescala, consulte [Preguntas más frecuentes acerca de Hiperescala](service-tier-hyperscale-frequently-asked-questions-faq.yml).
- Para más información sobre los niveles de servicio, consulte [Niveles de servicio](purchasing-models.md).
- Consulte [Introducción a los límites de recursos de un servidor](resource-limits-logical-server.md) para obtener información acerca de los límites en los niveles de servidor y suscripción.
- Para conocer los límites del modelo de compras para una base de datos única, consulte [Límites del modelo de compra basado en núcleo virtual de Azure SQL Database para una base de datos única](resource-limits-vcore-single-databases.md).
- Para obtener una lista de características y una comparación, consulte [Características comunes de SQL](features-comparison.md).
