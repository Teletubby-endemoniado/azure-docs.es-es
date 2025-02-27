---
title: Copia y transformación de datos en Azure Database for PostgreSQL
titleSuffix: Azure Data Factory & Azure Synapse
description: Aprenda a copiar y transformar datos en Azure Database for PostgreSQL mediante Azure Data Factory y Synapse Analytics.
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 10/25/2021
ms.openlocfilehash: ea2b0acf7c10f942d77c70a574fb19f0c29ca336
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131473231"
---
# <a name="copy-and-transform-data-in-azure-database-for-postgresql-using-azure-data-factory-or-synapse-analytics"></a>Copie y transforme datos en Azure Database for PostgreSQL mediante Azure Data Factory o Synapse Analytics.

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se explica el uso de la actividad de copia en las canalizaciones de Azure Data Factory y Synapse Analytics para copiar datos desde y hacia Azure Database for PostgreSQL, y cómo usar Data Flow para transformarlos en datos de Azure Database for PostgreSQL. Para obtener más información, lea los artículos de introducción para [Azure Data Factory](introduction.md) y [Synapse Analytics](../synapse-analytics/overview-what-is.md).

Ese conector se usa especialmente para el [servicio Azure Database for PostgreSQL](../postgresql/overview.md). Para copiar datos desde una base de datos PostgreSQL genérica ubicada en el entorno local o en la nube, use el [conector PostgreSQL](connector-postgresql.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de Azure Database for PostgreSQL es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Asignación de Data Flow](concepts-data-flow-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Actualmente, el flujo de datos admite Azure Database for PostgreSQL con la opción Servidor único, pero no con un servidor flexible ni de Hiperescala (Citus). En cambio, el flujo de datos de Azure Synapse Analytics admite todos los tipos de PostgreSQL.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-azure-database-for-postgresql-using-ui"></a>Creación de un servicio vinculado a Azure Database for PostgreSQL mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a Azure Database for PostgreSQL en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Data Factory":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque PostgreSQL y seleccione el conector de Azure Database for PostgreSQL.

    :::image type="content" source="media/connector-azure-database-for-postgresql/azure-database-for-postgresql-connector.png" alt-text="Selección del conector de Azure Database for PostgreSQL.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

    :::image type="content" source="media/connector-azure-database-for-postgresql/configure-azure-database-for-postgresql-linked-service.png" alt-text="Configuración de un servicio vinculado a Azure Database for PostgreSQL.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas para el conector de Azure Database for PostgreSQL.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Database for PostgreSQL:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **AzurePostgreSql**. | Sí |
| connectionString | Cadena de conexión de ODBC para conectarse a Azure Database for PostgreSQL.<br/>También puede establecer una contraseña en Azure Key Vault y extraer la configuración de `password` de la cadena de conexión. Consulte los siguientes ejemplos y el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md) con información detallada. | Sí |
| connectVia | Esta propiedad representa el [tiempo de ejecución de integración](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) (si el almacén de datos se encuentra en una red privada) o Azure Integration Runtime. Si no se especifica, se usará Azure Integration Runtime. |No |

Una cadena de conexión típica es `Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Aquí encontrará más propiedades que puede establecer para su caso:

| Propiedad | Descripción | Opciones | Obligatorio |
|:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| Método que usa el controlador para cifrar los datos enviados entre el controlador y el servidor de bases de datos. Por ejemplo: `EncryptionMethod=<0/1/6>;`| 0 (sin cifrado) **(valor predeterminado)** / 1 (SSL) / 6 (RequestSSL) | No |
| ValidateServerCertificate (VSC) | Determina si el controlador valida el certificado que envía el servidor de bases de datos cuando está habilitado el cifrado SSL (método de cifrado = 1). Por ejemplo: `ValidateServerCertificate=<0/1>;`| 0 (deshabilitado) **(valor predeterminado)** / 1 (habilitado) | No |

**Ejemplo**:

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
        }
    }
}
```

**Ejemplo**:

***Guardar la contraseña en Azure Key Vault***

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que Azure Database for PostgreSQL admite en los conjuntos de datos.

Para copiar datos de Azure Database for PostgreSQL, establezca la propiedad type del conjunto de datos en **AzurePostgreSqlTable**. Se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad "type" del conjunto de datos debe establecerse en **AzurePostgreSqlTable**. | Sí |
| tableName | Nombre de la tabla | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**:

```json
{
    "name": "AzurePostgreSqlDataset",
    "properties": {
        "type": "AzurePostgreSqlTable",
        "linkedServiceName": {
            "referenceName": "<AzurePostgreSql linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Para ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte [Canalizaciones y actividades](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades compatibles con el origen de Azure Database for PostgreSQL.

### <a name="azure-database-for-postgresql-as-source"></a>Azure Database for PostgreSQL como origen

Para copiar datos de Azure Database for PostgreSQL, establezca el tipo de origen de la actividad de copia en **AzurePostgreSqlSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad "type" del origen de la actividad de copia debe establecerse en **AzurePostgreSqlSource**. | Sí |
| Query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `SELECT * FROM mytable` o `SELECT * FROM "MyTable"`. Tenga en cuenta que en PostgreSQL el nombre de la entidad se trata sin distinción de mayúsculas y minúsculas si no está entre comillas. | No (si se especifica la propiedad tableName en el conjunto de datos) |
| partitionOptions | Especifica las opciones de creación de particiones de datos que se usan para cargar datos desde Azure SQL Database. <br>Los valores permitidos son: **None** (valor predeterminado), **PhysicalPartitionsOfTable** y **DynamicRange**.<br>Cuando se habilita una opción de partición (es decir, no `None`), el grado de paralelismo para cargar datos de forma simultánea desde una instancia de Azure SQL Database se controla mediante el valor [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) en la actividad de copia. | No |
| partitionSettings | Especifique el grupo de configuración para la creación de particiones de datos. <br>Se aplica si la opción de partición no es `None`. | No |
| ***En`partitionSettings`:*** | | |
| partitionNames | Lista de particiones físicas que deben copiarse. <br>Se aplica si la opción de partición es `PhysicalPartitionsOfTable`. Si usa una consulta para recuperar datos de origen, enlace `?AdfTabularPartitionName` en la cláusula WHERE. Para ver un ejemplo, consulte la sección [Copia en paralelo desde Azure Database for PostgreSQL](#parallel-copy-from-azure-database-for-postgresql). | No |
| partitionColumnName | Especifique el nombre de la columna de origen **de tipo entero o date/datetime** (`int`, `smallint`, `bigint`, `date`, `timestamp without time zone`, `timestamp with time zone` o `time without time zone`) que se va a usar en la creación de particiones por rangos para la copia en paralelo. Si no se especifica, se detectará automáticamente la clave principal de la tabla y se usará como columna de partición.<br>Se aplica si la opción de partición es `DynamicRange`. Si usa una consulta para recuperar datos de origen, enlace `?AdfRangePartitionColumnName ` en la cláusula WHERE. Para ver un ejemplo, consulte la sección [Copia en paralelo desde Azure Database for PostgreSQL](#parallel-copy-from-azure-database-for-postgresql). | No |
| partitionUpperBound | El valor máximo de la columna de partición para copiar datos. <br>Se aplica si la opción de partición es `DynamicRange`. Si usa una consulta para recuperar datos de origen, enlace `?AdfRangePartitionUpbound` en la cláusula WHERE. Para ver un ejemplo, consulte la sección [Copia en paralelo desde Azure Database for PostgreSQL](#parallel-copy-from-azure-database-for-postgresql). | No |
| partitionLowerBound | El valor mínimo de la columna de partición para copiar datos. <br>Se aplica si la opción de partición es `DynamicRange`. Si usa una consulta para recuperar datos de origen, enlace `?AdfRangePartitionLowbound` en la cláusula WHERE. Para ver un ejemplo, consulte la sección [Copia en paralelo desde Azure Database for PostgreSQL](#parallel-copy-from-azure-database-for-postgresql). | No |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromAzurePostgreSql",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AzurePostgreSql input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzurePostgreSqlSource",
                "query": "<custom query e.g. SELECT * FROM mytable>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-database-for-postgresql-as-sink"></a>Azure Database for PostgreSQL como receptor

Para copiar datos en Azure Database for PostgreSQL, se admiten las siguientes propiedades en la sección de **receptor** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad "type" del receptor de la actividad de copia debe establecerse en **AzurePostgreSQLSink**. | Sí |
| preCopyScript | Especifique una consulta de SQL para que la actividad de copia se ejecute antes de escribir datos en Azure Database for PostgreSQL en cada ejecución. Puede usar esta propiedad para limpiar los datos cargados previamente. | No |
| writeMethod | El método usado para escribir datos en Azure Database for PostgreSQL.<br>Los valores admitidos son:**CopyCommand** (valor predeterminado, que es más eficaz), **BulkInsert**. | No |
| writeBatchSize | Número de filas cargadas en Azure Database for PostgreSQL por lote.<br>El valor permitido es un entero que representa el número de filas. | No (el valor predeterminado es 1 000 000) |
| writeBatchTimeout | Tiempo de espera para que la operación de inserción por lotes se complete antes de que se agote el tiempo de espera.<br>Los valores permitidos son las cadenas de intervalo de tiempo. Un ejemplo es 00:30:00 (30 minutos). | No (el valor predeterminado es 00:30:00) |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyToAzureDatabaseForPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure PostgreSQL output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzurePostgreSQLSink",
                "preCopyScript": "<custom SQL script>",
                "writeMethod": "CopyCommand",
                "writeBatchSize": 1000000
            }
        }
    }
]
```

## <a name="parallel-copy-from-azure-database-for-postgresql"></a>Copia paralela de Azure Database for PostgreSQL

En la actividad de copia, el conector de Azure Database for PostgreSQL proporciona la opción de crear particiones de datos integradas para copiar los datos en paralelo. Puede encontrar las opciones de creación de particiones de datos en la pestaña **Origen** de la actividad de copia.

![Captura de pantalla de las opciones de partición](.\media\connector-azure-database-for-postgresql/connector-postgresql-partition-options.png)

Al habilitar la copia con particiones, la actividad de copia ejecuta consultas en paralelo en el origen de Azure Database for PostgreSQL para cargar los datos por particiones. El grado en paralelo se controla mediante el valor [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) de la actividad de copia. Por ejemplo, si establece `parallelCopies` en cuatro, el servicio genera y ejecuta al mismo tiempo cuatro consultas de acuerdo con la configuración y la opción de partición que ha especificado, y cada consulta recupera una porción de la instancia de Azure Database for PostgreSQL.

Se sugiere habilitar la copia en paralelo con la creación de particiones de datos, especialmente si se cargan grandes cantidades de datos de Azure Database for PostgreSQL. Estas son algunas configuraciones sugeridas para diferentes escenarios. Cuando se copian datos en un almacén de datos basado en archivos, se recomienda escribirlos en una carpeta como varios archivos (solo especifique el nombre de la carpeta), en cuyo caso el rendimiento es mejor que escribirlos en un único archivo.

| Escenario                                                     | Configuración sugerida                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Carga completa de una tabla grande con particiones físicas.        | **Opción de partición**: particiones físicas de la tabla. <br><br/>Durante la ejecución, el servicio detecta automáticamente las particiones físicas y copia los datos por particiones. |
| Carga completa de una tabla grande, sin particiones físicas, aunque con una columna de enteros para la creación de particiones de datos. | **Opciones de partición**: partición por rangos dinámica.<br>**Columna de partición**: especifique la columna usada para crear la partición de datos. Si no se especifica, se usa la columna de clave principal. |
| Cargue una gran cantidad de datos mediante una consulta personalizada con particiones físicas. | **Opción de partición**: particiones físicas de la tabla.<br>**Consulta**: `SELECT * FROM ?AdfTabularPartitionName WHERE <your_additional_where_clause>`.<br>**Nombre de la partición**: especifique los nombres de las particiones desde las que se copiarán los datos. Si no se especifican, el servicio detecta automáticamente las particiones físicas en la tabla que ha especificado en el conjunto de datos de PostgreSQL.<br><br>Durante la ejecución, el servicio reemplaza `?AdfTabularPartitionName` por el nombre real de la partición y se lo envía a Azure Database for PostgreSQL. |
| Carga de grandes cantidades de datos mediante una consulta personalizada, sin particiones físicas, aunque cuenta con una columna de enteros para la creación de particiones de datos. | **Opciones de partición**: partición por rangos dinámica.<br>**Consulta**: `SELECT * FROM ?AdfTabularPartitionName WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Columna de partición**: especifique la columna usada para crear la partición de datos. Puede crear particiones en la columna con el tipo de datos entero o date/datetime.<br>**Límite de partición superior** y **límite de partición inferior**: especifique si quiere filtrar en la columna de partición para recuperar solo los datos entre el intervalo inferior y el superior.<br><br>Durante la ejecución, el servicio reemplaza `?AdfRangePartitionColumnName`, `?AdfRangePartitionUpbound`, y `?AdfRangePartitionLowbound` por el nombre real de la columna y los rangos de valor para cada partición y los envía a Azure Database for PostgreSQL. <br>Por ejemplo, si establece la columna de partición "ID" con un límite inferior de 1 y un límite superior de 80, con la copia en paralelo establecida en 4, el servicio recupera los datos de 4 particiones. Los identificadores están comprendidos entre [1, 20], [21, 40], [41, 60] y [61, 80] respectivamente. |

Procedimientos recomendados para cargar datos con la opción de partición:

1. Seleccione una columna distintiva como columna de partición (como clave principal o clave única) para evitar la asimetría de datos. 
2. Si la tabla tiene una partición integrada, use la opción de partición "Particiones físicas de tabla" para obtener un mejor rendimiento.    
3. Si usa Azure Integration Runtime para copiar datos, puede establecer "[unidades de integración de datos (DIU)](copy-activity-performance-features.md#data-integration-units)" mayores (> 4) para usar más recursos de cálculo. Compruebe los escenarios aplicables allí.
4. "[Grado de paralelismo de copia](copy-activity-performance-features.md#parallel-copy)" controla los números de partición. Si se establece en un número demasiado grande, puede resentirse el rendimiento, así que se recomienda establecerlo como (DIU o número de nodos de IR autohospedados) * (2 a 4).

**Ejemplo: carga completa de una tabla grande con particiones físicas**

```json
"source": {
    "type": "AzurePostgreSqlSource",
    "partitionOption": "PhysicalPartitionsOfTable"
}
```

**Ejemplo: consulta con partición por rangos dinámica**

```json
"source": {
    "type": "AzurePostgreSqlSource",
    "query": "SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column (optional) to decide the partition stride, not as data filter>",
        "partitionLowerBound": "<lower_value_of_partition_column (optional) to decide the partition stride, not as data filter>"
    }
}
```

## <a name="mapping-data-flow-properties"></a>Propiedades de Asignación de instancias de Data Flow

Al transformar datos en el flujo de datos de asignación, puede leer tablas y escribir en ellas desde Azure Database for PostgreSQL. Para más información, vea la [transformación de origen](data-flow-source.md) y la [transformación de receptor](data-flow-sink.md) en los flujos de datos de asignación. Puede optar por usar un conjunto de datos de Azure Database for PostgreSQL o un [conjunto de datos insertado](data-flow-source.md#inline-datasets) como tipo de origen y receptor.

### <a name="source-transformation"></a>Transformación de origen

En la tabla siguiente se enumeran las propiedades compatibles con el origen de Azure Database for PostgreSQL. Puede editar estas propiedades en la pestaña **Source options** (Opciones de origen).

| Nombre | Descripción | Obligatorio | Valores permitidos | Propiedad de script de flujo de datos |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Tabla | Si selecciona Tabla como entrada, el flujo de datos captura todos los datos de la tabla especificada en el conjunto de datos. | No | - |*(solo para conjunto de datos en línea)*<br>tableName |
| Consultar | Si selecciona Consultar como entrada, especifique una consulta SQL para capturar datos del origen, lo que invalida cualquier tabla que especifique en el conjunto de datos. El uso de consultas es una excelente manera de reducir las filas para pruebas o búsquedas.<br><br>La cláusula **Ordenar por** no se admite, pero puede establecer una instrucción SELECT FROM completa. También puede usar las funciones de tabla definidas por el usuario. **select * from udfGetData()** es un UDF in SQL que devuelve una tabla que puede utilizar en el flujo de datos.<br>Ejemplo de consulta: `select * from mytable where customerId > 1000 and customerId < 2000` o `select * from "MyTable"`. Tenga en cuenta que en PostgreSQL el nombre de la entidad se trata sin distinción de mayúsculas y minúsculas si no está entre comillas.| No | String | Query |
| Tamaño de lote | Especifique un tamaño de lote para fragmentar datos de gran tamaño en lotes. | No | Entero | batchSize |
| Nivel de aislamiento | Elija uno de los siguientes niveles de aislamiento:<br>- Read Committed<br>- Read Uncommitted (predeterminado)<br>- Repeatable Read<br>- Serializable<br>- None (ignorar el nivel de aislamiento) | No | <small>READ_COMMITTED<br/>READ_UNCOMMITTED<br/>REPEATABLE_READ<br/>SERIALIZABLE<br/>NONE</small> |isolationLevel |

#### <a name="azure-database-for-postgresql-source-script-example"></a>Ejemplo de script de origen de Azure Database for PostgreSQL

Cuando se usa Azure Database for PostgreSQL como tipo de origen, el script de flujo de datos asociado es:

```
source(allowSchemaDrift: true,
    validateSchema: false,
    isolationLevel: 'READ_UNCOMMITTED',
    query: 'select * from mytable',
    format: 'query') ~> AzurePostgreSQLSource
```

### <a name="sink-transformation"></a>Transformación de receptor

En la tabla siguiente se enumeran las propiedades compatibles con el receptor de Azure Database for PostgreSQL. Puede editar estas propiedades en la pestaña **Opciones del receptor**.

| Nombre | Descripción | Obligatorio | Valores permitidos | Propiedad de script de flujo de datos |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Método de actualización | Especifique qué operaciones se permiten en el destino de la base de datos. El valor predeterminado es permitir solamente las inserciones.<br>Para actualizar, upsert o eliminar filas, se requiere una [transformación de alteración de fila](data-flow-alter-row.md) a fin de etiquetar filas para esas acciones. | Sí | `true` o `false` | deletable <br/>insertable <br/>updateable <br/>upsertable |
| Columnas de clave | En el caso de las actualizaciones, upserts y eliminaciones, se deben establecer columnas de clave para determinar la fila que se va a modificar.<br>El nombre de columna que elija como clave se usará como parte de las operaciones posteriores de actualización, upsert y eliminación. Por lo tanto, debe seleccionar una columna que exista en la asignación del receptor. | No | Array | claves |
| Omitir escritura de columnas de clave | Si no quiere escribir el valor en la columna de clave, seleccione "Skip writing key columns" (Omitir escritura de columnas de clave). | No | `true` o `false` | skipKeyWrites |
| Acción Table |determina si se deben volver a crear o quitar todas las filas de la tabla de destino antes de escribir.<br>- **Ninguno**: no se realizará ninguna acción en la tabla.<br>- **Volver a crear**: se quitará la tabla y se volverá a crear. Obligatorio si se crea una nueva tabla dinámicamente.<br>- **Truncar**: se quitarán todas las filas de la tabla de destino. | No | `true` o `false` | recreate<br/>truncate |
| Tamaño de lote | Especifique el número de filas que se escriben en cada lote. Los tamaños de lote más grandes mejoran la compresión y la optimización de memoria, pero se arriesgan a obtener excepciones de memoria al almacenar datos en caché. | No | Entero | batchSize |
| Scripts SQL anteriores y posteriores | Especifique scripts de SQL de varias líneas que se ejecutarán antes (preprocesamiento) y después (procesamiento posterior) de que los datos se escriban en la base de datos del receptor. | No | String | preSQLs<br>postSQLs |

#### <a name="azure-database-for-postgresql-sink-script-example"></a>Ejemplo de script de receptor de Azure Database for PostgreSQL

Cuando se usa Azure Database for PostgreSQL como tipo de receptor, el script de flujo de datos asociado es:

```
IncomingStream sink(allowSchemaDrift: true,
    validateSchema: false,
    deletable:false,
    insertable:true,
    updateable:true,
    upsertable:true,
    keys:['keyColumn'],
    format: 'table',
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> AzurePostgreSQLSink
```

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para más información sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
