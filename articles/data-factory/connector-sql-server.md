---
title: Copia y transformación de datos con SQL Server como origen o destino
titleSuffix: Azure Data Factory & Azure Synapse
description: Aprenda a copiar y transformar los datos con la base de datos SQL Server como origen o destino, en un entorno local o en una máquina virtual de Azure mediante canalizaciones de Azure Data Factory o Azure Synapse Analytics.
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 42040653f432577457cea6e5325fe686878e9da3
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124820161"
---
# <a name="copy-and-transform-data-to-and-from-sql-server-by-using-azure-data-factory-or-azure-synapse-analytics"></a>Copia y transformación de datos en SQL Server mediante Azure Data Factory o Azure Synapse Analytics

> [!div class="op_single_selector" title1="Seleccione la versión de Azure Data Factory que usa:"]
> * [Versión 1](v1/data-factory-sqlserver-connector.md)
> * [Versión actual](connector-sql-server.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe cómo usar la actividad de copia de las canalizaciones de Azure Data Factory y Azure Synapse para copiar datos con la base de datos de SQL Server como origen o destino y cómo usar Data Flow para transformar los datos de la base de datos de SQL Server.  Para obtener más información, lea el artículo de introducción para [Azure Data Factory](introduction.md) o [Azure Synapse Analytics](../synapse-analytics/overview-what-is.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector SQL Server es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Asignación de Data Flow](concepts-data-flow-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)

Puede copiar datos desde una base de datos de SQL Server en cualquier almacén de datos receptor compatible. O bien puede copiar datos desde cualquier almacén de datos de origen compatible a una base de datos de SQL Server. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector SQL Server admite las siguientes funcionalidades:

- SQL Server versión 2005 y posteriores.
- La copia de datos con autenticación de SQL o Windows.
- Como origen, la recuperación de datos mediante una consulta SQL o un procedimiento almacenado. También puede optar por la copia en paralelo desde un origen de SQL Server, consulte la sección [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database) para obtener detalles.
- Como receptor, la creación automática de la tabla de destino si no existe en función del esquema de origen; la anexión de datos a una tabla o la invocación de un procedimiento almacenado con lógica personalizada durante la copia. 

No se admite la base de datos local LocalDB de [SQL Server Express](/sql/database-engine/configure-windows/sql-server-express-localdb).


## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [data-factory-v2-integration-runtime-requirements](includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-sql-server-linked-service-using-ui"></a>Creación de un servicio vinculado de SQL Server mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado de SQL Server en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Data Factory.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque SQL y seleccione el conector de SQL Server.

    :::image type="content" source="media/connector-sql-server/sql-server-connector.png" alt-text="Captura de pantalla del conector de SQL Server.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

    :::image type="content" source="media/connector-sql-server/configure-sql-server-linked-service.png" alt-text="Captura de pantalla de la configuración del servicio vinculado de SQL Server.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de canalizacione sde Data Factory y SQL Server específicas del conector de base de datos SQL Server.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las propiedades siguientes son compatibles con el servicio vinculado SQL Server:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type se debe establecer en: **SqlServer**. | Sí |
| connectionString |Especifique la información de **connectionString** necesaria para conectarse a la base de datos SQL Server mediante autenticación de SQL o autenticación de Windows. Consulte los ejemplos siguientes.<br/>También puede asignar una contraseña en Azure Key Vault. Si se trata de la autenticación de SQL, extraiga la configuración `password` de la cadena de conexión. Para obtener más información, vea el ejemplo de JSON debajo de la tabla y consulte el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md). |Sí |
| userName |Especifique el nombre de usuario si usa la autenticación de Windows. Un ejemplo es **domainname\\username**. |No |
| password |Especifique la contraseña de la cuenta de usuario que se especificó para el nombre de usuario. Marque este campo como **SecureString** para almacenarlo de forma segura. O bien puede [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |No |
| alwaysEncryptedSettings | Especifique la información **alwaysencryptedsettings** necesaria para permitir que Always Encrypted proteja los datos confidenciales almacenados en un servidor SQL mediante una identidad administrada o una entidad de servicio. Para obtener más información, vea el ejemplo de JSON debajo de la tabla y consulte la sección [Uso de Always Encrypted](#using-always-encrypted). Si no se especifica, la configuración predeterminada Always Encrypted está deshabilitada. |No |
| connectVia | Este [entorno de ejecución de integración](concepts-integration-runtime.md) se usa para conectarse al almacén de datos. Obtenga más información en la sección [Requisitos previos](#prerequisites). Si no se especifica, se usa el valor predeterminado de Azure Integration Runtime. |No |

> [!NOTE]
> La característica [**Always Encrypted**](/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-ver15&preserve-view=true) de SQL Server ya no se admite. 

>[!TIP]
>Si recibió un error con el código de error "UserErrorFailedToConnectToSqlServer" y un mensaje parecido a "The session limit for the database is XXX and has been reached" (El límite de sesión de la base de datos es XXX y ya se ha alcanzado), agregue `Pooling=false` a la cadena de conexión e inténtelo de nuevo.

**Ejemplo 1: Uso de la autenticación de SQL**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo 2: Uso de la autenticación de SQL con una contraseña en Azure Key Vault**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo 3: Uso de la autenticación de Windows**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;",
            "userName": "<domain\\username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo 4: Uso de Always Encrypted**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
        },
        "alwaysEncryptedSettings": {
            "alwaysEncryptedAkvAuthType": "ServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de SQL Server.

Las siguientes propiedades son compatibles para copiar datos con una base de datos SQL Server como origen y destino:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos se debe establecer en **SqlServerTable**. | Sí |
| esquema | Nombre del esquema. |No para el origen, sí para el receptor  |
| table | Nombre de la tabla o vista. |No para el origen, sí para el receptor  |
| tableName | Nombre de la tabla o vista con el esquema. Esta propiedad permite la compatibilidad con versiones anteriores. Para la nueva carga de trabajo use `schema` y `table`. | No para el origen, sí para el receptor |

**Ejemplo**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "schema": "<schema_name>",
            "table": "<table_name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, vea el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admiten el origen y receptor de SQL Server.

### <a name="sql-server-as-a-source"></a>SQL Server como origen

>[!TIP]
>Para cargar datos desde SQL Server de manera eficaz mediante la creación de particiones de datos, consulte [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database).

Si va a copiar datos desde SQL Server, establezca el tipo de origen de la actividad de copia en **SqlSource**. En la sección source de la actividad de copia se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en **SqlSource**. | Sí |
| sqlReaderQuery |Use la consulta SQL personalizada para leer los datos. Un ejemplo es `select * from MyTable`. |No |
| sqlReaderStoredProcedureName |Esta propiedad es el nombre del procedimiento almacenado que lee datos de la tabla de origen. La última instrucción SQL debe ser una instrucción SELECT del procedimiento almacenado. |No |
| storedProcedureParameters |Estos parámetros son para el procedimiento almacenado.<br/>Los valores permitidos son pares de nombre o valor. Los nombres y las mayúsculas y minúsculas de los parámetros tienen que coincidir con las mismas características de los parámetros de procedimiento almacenado. |No |
| isolationLevel | Especifica el comportamiento de bloqueo de transacción para el origen de SQL. Los valores permitidos son: **ReadCommitted**, **ReadUncommitted**, **RepeatableRead**, **Serializable** y **Snapshot**. Si no se especifica, se utiliza el nivel de aislamiento predeterminado de la base de datos. Vea [este documento](/dotnet/api/system.data.isolationlevel) para obtener más detalles. | No |
| partitionOptions | Especifica las opciones de creación de particiones de datos que se usan para cargar datos desde SQL Server. <br>Los valores permitidos son: **None** (valor predeterminado), **PhysicalPartitionsOfTable** y **DynamicRange**.<br>Cuando se habilita una opción de partición (es decir, el valor no es `None`), el grado de paralelismo para cargar datos de manera simultánea desde SQL Server se controla mediante la opción [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) en la actividad de copia. | No |
| partitionSettings | Especifique el grupo de configuración para la creación de particiones de datos. <br>Se aplica si la opción de partición no es `None`. | No |
| ***En`partitionSettings`:*** | | |
| partitionColumnName | Especifique el nombre de la columna de origen **de tipo entero o date/datetime*** (`int`, `smallint`, `bigint`, `date`, `smalldatetime`, `datetime`, `datetime2` o `datetimeoffset`) que se va a usar en la creación de particiones por rangos para la copia en paralelo. Si no se especifica, el índice o la clave primaria de la tabla se detectan automáticamente y se usan como columna de partición.<br>Se aplica si la opción de partición es `DynamicRange`. Si usa una consulta para recuperar datos de origen, enlace `?AdfDynamicRangePartitionCondition ` en la cláusula WHERE. Para obtener un ejemplo, vea la sección [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database). | No |
| partitionUpperBound | Valor máximo de la columna de partición para la división del rango de partición. Este valor se usa para decidir el intervalo de particiones, no para filtrar las filas de la tabla. Se crean particiones de todas las filas de la tabla o el resultado de la consulta y se copian. Si no se especifica, la actividad de copia detecta automáticamente el valor.  <br>Se aplica si la opción de partición es `DynamicRange`. Para obtener un ejemplo, vea la sección [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database). | No |
| partitionLowerBound | Valor mínimo de la columna de partición para la división del rango de partición. Este valor se usa para decidir el intervalo de particiones, no para filtrar las filas de la tabla. Se crean particiones de todas las filas de la tabla o el resultado de la consulta y se copian. Si no se especifica, la actividad de copia detecta automáticamente el valor.<br>Se aplica si la opción de partición es `DynamicRange`. Para obtener un ejemplo, vea la sección [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database). | No |

**Tenga en cuenta los siguientes puntos:**

- Si se especifica **sqlReaderQuery** para **SqlSource**, la actividad de copia ejecuta la consulta en el origen de SQL Server para obtener los datos. También puede indicar un procedimiento almacenado mediante la definición de **sqlReaderStoredProcedureName** y **storedProcedureParameters** si el procedimiento almacenado adopta parámetros.
- Al usar el procedimiento almacenado del origen para recuperar datos, tenga en cuenta que si está diseñado para devolver otro esquema cuando se pasa un valor de parámetro diferente, es posible que encuentre un error o vea un resultado inesperado al importar el esquema desde la interfaz de usuario, o bien al copiar datos en la base de datos SQL con la creación automática de tablas.

**Ejemplo: Uso de la consulta SQL**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Ejemplo: Uso de un procedimiento almacenado**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Definición del procedimiento almacenado**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sql-server-as-a-sink"></a>SQL Server como receptor

> [!TIP]
> Más información sobre los comportamientos de escritura, las configuraciones y los procedimientos recomendados que se admiten en [Procedimiento recomendado para cargar datos en SQL Server](#best-practice-for-loading-data-into-sql-server).

Si va a copiar datos en SQL Server, establezca el tipo de receptor de la actividad de copia en **SqlSink**. La sección sink de la actividad de copia admite las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del receptor de la actividad de copia debe establecerse en **SqlSink**. | Sí |
| preCopyScript |Esta propiedad especifica una consulta SQL para que la actividad de copia se ejecute antes de escribir datos en SQL Server. Solo se invoca una vez por cada copia que se ejecuta. Puede usar esta propiedad para limpiar los datos cargados previamente. |No |
| tableOption | Especifica si [se crea automáticamente la tabla de receptores](copy-activity-overview.md#auto-create-sink-tables) según el esquema de origen, si no existe. No se admite la creación automática de tablas cuando el receptor especifica un procedimiento almacenado. Los valores permitidos son: `none` (valor predeterminado), `autoCreate`. |No |
| sqlWriterStoredProcedureName | El nombre del procedimiento almacenado que define cómo se aplican los datos de origen en una tabla de destino. <br/>Este procedimiento almacenado se *invoca por lote*. Para las operaciones que solo se ejecuta una vez y que no tiene nada que ver con los datos de origen, como por ejemplo, eliminar o truncar, use la propiedad `preCopyScript`.<br>Vea el ejemplo de [Invocación del procedimiento almacenado desde el receptor de SQL](#invoke-a-stored-procedure-from-a-sql-sink). | No |
| storedProcedureTableTypeParameterName |Nombre del parámetro del tipo de tabla especificado en el procedimiento almacenado.  |No |
| sqlWriterTableType |Nombre del tipo de tabla que se usará en el procedimiento almacenado. La actividad de copia dispone que los datos que se mueven estén disponibles en una tabla temporal con este tipo de tabla. El código de procedimiento almacenado puede combinar los datos copiados con datos existentes. |No |
| storedProcedureParameters |Parámetros del procedimiento almacenado.<br/>Los valores permitidos son pares de nombre y valor. Los nombres y las mayúsculas y minúsculas de los parámetros deben coincidir con las mismas características de los parámetros de procedimiento almacenado. | No |
| writeBatchSize |Número de filas que se va a insertar en la tabla SQL *por lote*.<br/>Los valores permitidos son enteros para el número de filas. De manera predeterminada, el servicio determina dinámicamente el tamaño adecuado del lote en función del tamaño de fila. |No |
| writeBatchTimeout |Esta propiedad especifica el tiempo de espera para que la operación de inserción por lotes se complete antes de que se agote el tiempo de espera.<br/>Los valores permitidos son para el intervalo de tiempo. Un ejemplo es "00:30:00" para 30 minutos. Si no se especifica ningún valor, el valor predeterminado es "02:00:00". |No |
| maxConcurrentConnections |Número máximo de conexiones simultáneas establecidas en el almacén de datos durante la ejecución de la actividad. Especifique un valor solo cuando quiera limitar las conexiones simultáneas.| No |

**Ejemplo 1: Anexión de datos**

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "tableOption": "autoCreate",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Ejemplo 2: Invocación de un procedimiento almacenado durante la copia**

Para más información, vea [Invocación del procedimiento almacenado desde el receptor de SQL ](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "storedProcedureTableTypeParameterName": "MyTable",
                "sqlWriterTableType": "MyTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="parallel-copy-from-sql-database"></a>Copia en paralelo desde una base de datos SQL

En la actividad de copia, el conector de SQL Server proporciona creación de particiones de datos integrada para copiar los datos en paralelo. Puede encontrar las opciones de creación de particiones de datos en la pestaña **Origen** de la actividad de copia.

:::image type="content" source="./media/connector-sql-server/connector-sql-partition-options.png" alt-text="Captura de pantalla de las opciones de partición":::

Al habilitar la copia con particiones, la actividad de copia ejecuta consultas en paralelo en el origen de SQL Server para cargar los datos por particiones. El grado en paralelo se controla mediante el valor [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) de la actividad de copia. Por ejemplo, si establece `parallelCopies` en cuatro, el servicio genera y ejecuta al mismo tiempo cuatro consultas de acuerdo con la configuración y la opción de partición que ha especificado, y cada consulta recupera una porción de datos de SQL Server.

Se sugiere habilitar la copia en paralelo con la creación de particiones de datos, especialmente si se cargan grandes cantidades de datos de SQL Server. Estas son algunas configuraciones sugeridas para diferentes escenarios. Cuando se copian datos en un almacén de datos basado en archivos, se recomienda escribirlos en una carpeta como varios archivos (solo especifique el nombre de la carpeta), en cuyo caso el rendimiento es mejor que escribirlos en un único archivo.

| Escenario                                                     | Configuración sugerida                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Carga completa de una tabla grande con particiones físicas.        | **Opción de partición**: particiones físicas de la tabla. <br><br/>Durante la ejecución, el servicio detecta automáticamente las particiones físicas y copia los datos por particiones. <br><br/>Para comprobar si la tabla tiene una partición física o no, puede hacer referencia a [esta consulta](#sample-query-to-check-physical-partition). |
| Carga completa de una tabla grande, sin particiones físicas, aunque con una columna de tipo entero o datetime para la creación de particiones de datos. | **Opciones de partición**: partición por rangos dinámica.<br>**Columna de partición** (opcional): especifique la columna usada para crear la partición de datos. Si no se especifica, se usa la columna de clave principal.<br/>**Límite de partición superior** y **límite de partición inferior** (opcional): especifique si quiere determinar el intervalo de la partición. No es para filtrar las filas de la tabla, se crean particiones de todas las filas de la tabla y se copian. Si no se especifica, la actividad de copia detecta automáticamente los valores y puede tardar mucho tiempo en función de los valores MIN y MAX. Se recomienda proporcionar límite superior e inferior. <br><br>Por ejemplo, si la columna de partición "ID" tiene valores que van de 1 a 100 y establece el límite inferior en 20 y el superior en 80, con la copia en paralelo establecida en 4, el servicio recupera los datos en 4 particiones: identificadores del rango <=20, del rango [21, 50], del rango [51, 80] y del rango >=81, respectivamente. |
| Carga de grandes cantidades de datos mediante una consulta personalizada, sin particiones físicas, aunque con una columna de tipo entero o date/datetime para la creación de particiones de datos. | **Opciones de partición**: partición por rangos dinámica.<br>**Consulta**: `SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`.<br>**Columna de partición**: especifique la columna usada para crear la partición de datos.<br>**Límite de partición superior** y **límite de partición inferior** (opcional): especifique si quiere determinar el intervalo de la partición. No es para filtrar las filas de la tabla, se crean particiones de todas las filas del resultado de la consulta y se copian. Si no se especifica, la actividad de copia detecta automáticamente el valor.<br><br>Durante la ejecución, el servicio reemplaza `?AdfRangePartitionColumnName` por el nombre real de la columna y los rangos de valores de cada partición y se los envía a SQL Server. <br>Por ejemplo, si la columna de partición "ID" tiene valores que van de 1 a 100 y establece el límite inferior en 20 y el superior en 80, con la copia en paralelo establecida en 4, el servicio recupera los datos en 4 particiones: identificadores del rango <=20, del rango [21, 50], del rango [51, 80] y del rango >=81, respectivamente. <br><br>A continuación se muestran más consultas de ejemplo para distintos escenarios:<br> 1. Consulta de la tabla completa: <br>`SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition`<br> 2. Consulta de una tabla con selección de columnas y filtros adicionales de la cláusula where: <br>`SELECT <column_list> FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`<br> 3. Consulta con subconsultas: <br>`SELECT <column_list> FROM (<your_sub_query>) AS T WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`<br> 4. Consulta con partición en subconsulta: <br>`SELECT <column_list> FROM (SELECT <your_sub_query_column_list> FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition) AS T`
|

Procedimientos recomendados para cargar datos con la opción de partición:

1. Seleccione una columna distintiva como columna de partición (como clave principal o clave única) para evitar la asimetría de datos. 
2. Si la tabla tiene una partición integrada, use la opción de partición "Particiones físicas de tabla" para obtener un mejor rendimiento.    
3. Si usa Azure Integration Runtime para copiar datos, puede establecer "[unidades de integración de datos (DIU)](copy-activity-performance-features.md#data-integration-units)" mayores (> 4) para usar más recursos de cálculo. Compruebe los escenarios aplicables allí.
4. "[Grado de paralelismo de copia](copy-activity-performance-features.md#parallel-copy)" controla los números de partición. Si se establece en un número demasiado grande, puede resentirse el rendimiento, así que se recomienda establecerlo como (DIU o número de nodos de IR autohospedados) * (2 a 4).

**Ejemplo: carga completa de una tabla grande con particiones físicas**

```json
"source": {
    "type": "SqlSource",
    "partitionOption": "PhysicalPartitionsOfTable"
}
```

**Ejemplo: consulta con partición por rangos dinámica**

```json
"source": {
    "type": "SqlSource",
    "query": "SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column (optional) to decide the partition stride, not as data filter>",
        "partitionLowerBound": "<lower_value_of_partition_column (optional) to decide the partition stride, not as data filter>"
    }
}
```

### <a name="sample-query-to-check-physical-partition"></a>Consulta de ejemplo para comprobar la partición física

```sql
SELECT DISTINCT s.name AS SchemaName, t.name AS TableName, pf.name AS PartitionFunctionName, c.name AS ColumnName, iif(pf.name is null, 'no', 'yes') AS HasPartition
FROM sys.tables AS t
LEFT JOIN sys.objects AS o ON t.object_id = o.object_id
LEFT JOIN sys.schemas AS s ON o.schema_id = s.schema_id
LEFT JOIN sys.indexes AS i ON t.object_id = i.object_id 
LEFT JOIN sys.index_columns AS ic ON ic.partition_ordinal > 0 AND ic.index_id = i.index_id AND ic.object_id = t.object_id 
LEFT JOIN sys.columns AS c ON c.object_id = ic.object_id AND c.column_id = ic.column_id 
LEFT JOIN sys.partition_schemes ps ON i.data_space_id = ps.data_space_id 
LEFT JOIN sys.partition_functions pf ON pf.function_id = ps.function_id 
WHERE s.name='[your schema]' AND t.name = '[your table name]'
```

Si la tabla tiene una partición física, verá "HasPartition" como "yes" como en el caso siguiente.

:::image type="content" source="./media/connector-azure-sql-database/sql-query-result.png" alt-text="Resultado de la consulta SQL":::

## <a name="best-practice-for-loading-data-into-sql-server"></a>Procedimiento recomendado para cargar datos en SQL Server

Al copiar datos en SQL Server, puede requerir un comportamiento de escritura diferente:

- [Anexión](#append-data): mis datos de origen solo tienen registros nuevos.
- [Actualización e inserción](#upsert-data): mis datos de origen tienen inserciones y actualizaciones.
- [Sobrescritura](#overwrite-the-entire-table): quiero volver a cargar toda la tabla de dimensiones cada vez.
- [Escritura con lógica personalizada](#write-data-with-custom-logic): necesito un procesamiento adicional antes de la inserción final en la tabla de destino.

Consulte las secciones correspondientes sobre cómo configurar estas operaciones y los procedimientos recomendados.

### <a name="append-data"></a>Anexión de datos

La anexión de datos es el comportamiento predeterminado de este conector de receptor de SQL Server. El servicio realiza una inserción masiva para escribir en la tabla de forma eficaz. Puede configurar el origen y el receptor según corresponda en la actividad de copia.

### <a name="upsert-data"></a>Actualización e inserción de datos

**Opción 1:** si tiene una gran cantidad de datos para copiar, puede cargar de forma masiva todos los registros en una tabla de almacenamiento provisional mediante la actividad de copia y luego ejecutar una actividad de procedimiento almacenado para aplicar una instrucción [MERGE](/sql/t-sql/statements/merge-transact-sql) o INSERT/UPDATE de una sola vez. 

La actividad de copia actualmente no admite de forma nativa la carga de datos en una tabla temporal de base de datos. Hay una forma avanzada de configurarla con una combinación de varias actividades. Vea [Optimización de escenarios de upsert masivo de SQL Database](https://github.com/scoriani/azuresqlbulkupsert). A continuación se muestra un ejemplo de uso de una tabla permanente como almacenamiento provisional.

Por ejemplo, puede crear una canalización con una **actividad de copia** encadenada con una **actividad de procedimiento almacenado**. La primera copia datos del almacén de origen en una tabla de almacenamiento provisional de SQL Server, por ejemplo, **UpsertStagingTable**, como nombre de la tabla en el conjunto de datos. Luego, la segunda invoca un procedimiento almacenado para combinar datos de origen de la tabla de almacenamiento provisional en la tabla de destino y limpiar la tabla de almacenamiento provisional.

:::image type="content" source="./media/connector-azure-sql-database/azure-sql-database-upsert.png" alt-text="Upsert":::

En la base de datos, defina un procedimiento almacenado con la lógica MERGE, como en el ejemplo siguiente, al que se señala desde la actividad de procedimiento almacenado anterior. Suponga que el destino es la tabla **Marketing** con tres columnas: **ProfileID**, **State** y **Category**. Realice la operación upsert de acuerdo con la columna **ProfileID**.

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING UpsertStagingTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE UpsertStagingTable
END
```

**Opción 2:** puede optar por [invocar un procedimiento almacenado en la actividad de copia](#invoke-a-stored-procedure-from-a-sql-sink). Este enfoque ejecuta cada lote (según indica la propiedad `writeBatchSize`) en la tabla de origen en lugar de usar la inserción masiva como enfoque predeterminado en la actividad de copia.

### <a name="overwrite-the-entire-table"></a>Sobrescritura de toda la tabla

Puede configurar la propiedad **preCopyScript** en un receptor de la actividad de copia. En este caso, para cada actividad de copia que se ejecuta, el servicio ejecuta primero el script. Después, ejecuta la copia para insertar los datos. Por ejemplo, para sobrescribir toda la tabla con los datos más recientes, especifique un script para eliminar primero todos los registros antes de realizar la carga masiva de los nuevos datos desde el origen.

### <a name="write-data-with-custom-logic"></a>Escritura de datos con lógica personalizada

Los pasos necesarios para escribir datos con lógica personalizada son similares a los que se describen en la sección [Actualización e inserción de datos](#upsert-data). Si necesita aplicar procesamiento adicional antes de la inserción final de los datos de origen en la tabla de destino, puede cargar en una tabla de almacenamiento provisional y, luego, invocar una actividad de procedimiento almacenado, o bien invocar un procedimiento almacenado en un receptor de actividad de copia para aplicar datos.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Invocación del procedimiento almacenado desde el receptor de SQL

Al copiar datos en una base de datos de SQL Server, también se puede configurar e invocar un procedimiento almacenado especificado por el usuario con parámetros adicionales en cada lote de la tabla de origen. La característica de procedimiento almacenado aprovecha los [parámetros con valores de tabla](/dotnet/framework/data/adonet/sql/table-valued-parameters).

Cuando los mecanismos de copia integrados no prestan el servicio, se puede usar un procedimiento almacenado. Por ejemplo, si quiere aplicar un procesamiento adicional antes de la inserción final de los datos de origen en la tabla de destino. Otros ejemplos de procesamiento adicional son cuando quiere combinar columnas, buscar valores adicionales e insertar datos en más de una tabla.

En el ejemplo siguiente se muestra cómo usar un procedimiento almacenado para realizar una operación UPSERT en una tabla en la base de datos SQL Server. Supongamos que los datos de entrada y la tabla **Marketing** del receptor tienen tres columnas: **ProfileID**, **State** y **Category**. Realice una operación UPSERT en función de la columna **ProfileID** y aplíquela solo a una categoría específica llamada "ProductA".

1. En la base de datos, defina el tipo de tabla con el mismo nombre que **sqlWriterTableType**. El esquema del tipo de tabla es el mismo que el que devuelven los datos de entrada.

    ```sql
    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL,
        [Category] [varchar](256) NOT NULL
    )
    ```

2. En la base de datos, defina el procedimiento almacenado con el mismo nombre que **sqlWriterStoredProcedureName**. Dicho procedimiento administra los datos de entrada del origen especificado y los combina en la tabla de salida. El nombre del parámetro del tipo de tabla del procedimiento almacenado es el mismo que el de **tableName** que se ha definido en el conjunto de datos.

    ```sql
    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
    AS
    BEGIN
    MERGE [dbo].[Marketing] AS target
    USING @Marketing AS source
    ON (target.ProfileID = source.ProfileID and target.Category = @category)
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT MATCHED THEN
        INSERT (ProfileID, State, Category)
        VALUES (source.ProfileID, source.State, source.Category);
    END
    ```

3. Defina la sección de **receptor SQL** en la actividad de copia como se indica a continuación:

    ```json
    "sink": {
        "type": "SqlSink",
        "sqlWriterStoredProcedureName": "spOverwriteMarketing",
        "storedProcedureTableTypeParameterName": "Marketing",
        "sqlWriterTableType": "MarketingType",
        "storedProcedureParameters": {
            "category": {
                "value": "ProductA"
            }
        }
    }
    ```

## <a name="mapping-data-flow-properties"></a>Propiedades de Asignación de instancias de Data Flow

Al transformar datos en el flujo de datos de asignación, puede leer y escribir en las tablas de la base de datos de SQL Server. Para más información, vea la [transformación de origen](data-flow-source.md) y la [transformación de receptor](data-flow-sink.md) en los flujos de datos de asignación.

> [!NOTE]
> Para acceder a la instancia de SQL Server local, debe usar la [red virtual administrada](managed-virtual-network-private-endpoint.md) o el área de trabajo de Synapse de Azure Data Factory mediante un punto de conexión privado. Consulte este [tutorial](tutorial-managed-virtual-network-on-premise-sql-server.md) para ver los pasos detallados.

### <a name="source-transformation"></a>Transformación de origen

En la tabla siguiente se indican las propiedades que admite el origen de SQL Server. Puede editar estas propiedades en la pestaña **Source options** (Opciones de origen).

| Nombre | Descripción | Obligatorio | Valores permitidos | Propiedad de script de flujo de datos |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Tabla | Si selecciona Tabla como entrada, el flujo de datos captura todos los datos de la tabla especificada en el conjunto de datos. | No | - |- |
| Consultar | Si selecciona Consultar como entrada, especifique una consulta SQL para capturar datos del origen, lo que invalida cualquier tabla que especifique en el conjunto de datos. El uso de consultas es una excelente manera de reducir las filas para pruebas o búsquedas.<br><br>La cláusula **Ordenar por** no se admite, pero puede establecer una instrucción SELECT FROM completa. También puede usar las funciones de tabla definidas por el usuario. **select * from udfGetData()** es un UDF in SQL que devuelve una tabla que puede utilizar en el flujo de datos.<br>Ejemplo de consulta: `Select * from MyTable where customerId > 1000 and customerId < 2000`| No | String | Query |
| Tamaño de lote | Especifique un tamaño de lote para fragmentar datos grandes en lecturas. | No | Entero | batchSize |
| Nivel de aislamiento | Elija uno de los siguientes niveles de aislamiento:<br>- Read Committed<br>- Read Uncommitted (predeterminado)<br>- Repeatable Read<br>- Serializable<br>- None (ignorar el nivel de aislamiento) | No | <small>READ_COMMITTED<br/>READ_UNCOMMITTED<br/>REPEATABLE_READ<br/>SERIALIZABLE<br/>NONE</small> |isolationLevel |

#### <a name="sql-server-source-script-example"></a>Ejemplo de script de origen de SQL Server

Cuando se usa SQL Server como tipo de origen, el script de flujo de datos asociado es:

```
source(allowSchemaDrift: true,
    validateSchema: false,
    isolationLevel: 'READ_UNCOMMITTED',
    query: 'select * from MYTABLE',
    format: 'query') ~> SQLSource
```

### <a name="sink-transformation"></a>Transformación de receptor

En la tabla siguiente se indican las propiedades que admite el receptor de SQL Server. Puede editar estas propiedades en la pestaña **Opciones del receptor**.

| Nombre | Descripción | Obligatorio | Valores permitidos | Propiedad de script de flujo de datos |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Método de actualización | Especifique qué operaciones se permiten en el destino de la base de datos. El valor predeterminado es permitir solamente las inserciones.<br>Para actualizar, upsert o eliminar filas, se requiere una [transformación de alteración de fila](data-flow-alter-row.md) a fin de etiquetar filas para esas acciones. | Sí | `true` o `false` | deletable <br/>insertable <br/>updateable <br/>upsertable |
| Columnas de clave | En el caso de las actualizaciones, upserts y eliminaciones, se deben establecer columnas de clave para determinar la fila que se va a modificar.<br>El nombre de columna que elija como clave se usará como parte de las operaciones posteriores de actualización, upsert y eliminación. Por lo tanto, debe seleccionar una columna que exista en la asignación del receptor. | No | Array | claves |
| Omitir escritura de columnas de clave | Si no quiere escribir el valor en la columna de clave, seleccione "Skip writing key columns" (Omitir escritura de columnas de clave). | No | `true` o `false` | skipKeyWrites |
| Acción Table |determina si se deben volver a crear o quitar todas las filas de la tabla de destino antes de escribir.<br>- **Ninguno**: no se realizará ninguna acción en la tabla.<br>- **Volver a crear**: se quitará la tabla y se volverá a crear. Obligatorio si se crea una nueva tabla dinámicamente.<br>- **Truncar**: se quitarán todas las filas de la tabla de destino. | No | `true` o `false` | recreate<br/>truncate |
| Tamaño de lote | Especifique el número de filas que se escriben en cada lote. Los tamaños de lote más grandes mejoran la compresión y la optimización de memoria, pero se arriesgan a obtener excepciones de memoria al almacenar datos en caché. | No | Entero | batchSize |
| Scripts SQL anteriores y posteriores | Especifique scripts de SQL de varias líneas que se ejecutarán antes (preprocesamiento) y después (procesamiento posterior) de que los datos se escriban en la base de datos del receptor. | No | String | preSQLs<br>postSQLs |

#### <a name="sql-server-sink-script-example"></a>Ejemplo de script de receptor de SQL Server

Cuando se usa SQL Server como tipo de receptor, el script de flujo de datos asociado es:

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
    skipDuplicateMapOutputs: true) ~> SQLSink
```

## <a name="data-type-mapping-for-sql-server"></a>Asignación de tipos de datos para SQL Server

Cuando copia datos con SQL Server como origen y destino, se usan las siguientes asignaciones de tipos de datos de SQL Server para los tipos de datos provisionales de Azure Data Factory. Las canalizaciones de Synapse, que implementan Data Factory, usan las mismas asignaciones.  Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para más información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Tipos de datos de SQL Server | Tipo de datos provisionales de Data Factory |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |DateTime |
| Datetime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| imagen |Byte[] |
| int |Int32 |
| money |Decimal |
| NCHAR |String, Char[] |
| ntext |String, Char[] |
| NUMERIC |Decimal |
| NVARCHAR |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| SMALLINT |Int16 |
| SMALLMONEY |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| TINYINT |Int16 |
| UNIQUEIDENTIFIER |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| Xml |String |

>[!NOTE]
> En el caso de los tipos de datos que se asignan al tipo decimal provisional, la actividad de copia actualmente admite una precisión de hasta 28. Si tiene datos que requieren una precisión mayor que 28, considere la posibilidad de convertir a una cadena en una consulta SQL.

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).

## <a name="getmetadata-activity-properties"></a>Propiedades de la actividad GetMetadata

Para información detallada sobre las propiedades, consulte [Actividad de obtención de metadatos](control-flow-get-metadata-activity.md). 

## <a name="using-always-encrypted"></a>Uso de Always Encrypted

Al copiar datos desde o hacia SQL Server con [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine), siga estos pasos: 

1. Almacene la [clave maestra de columna (CMK)](/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted?view=sql-server-ver15&preserve-view=true) en una instancia de [Azure Key Vault](../key-vault/general/overview.md). Obtenga más información acerca de la [configuración de Always Encrypted con Azure Key Vault](../azure-sql/database/always-encrypted-azure-key-vault-configure.md?tabs=azure-powershell)

2. Asegúrese de conceder acceso al almacén de claves donde se almacena la [clave maestra de columna (CMK)](/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted?view=sql-server-ver15&preserve-view=true). Consulte este [artículo](/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted?view=sql-server-ver15&preserve-view=true#key-vaults) para obtener información acerca de los permisos necesarios.

3. Cree un servicio vinculado para conectarse a la base de datos SQL y habilitar la función "Always Encrypted" mediante una identidad administrada o una entidad de servicio. 


>[!NOTE]
>La función [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) de SQL Server admite los siguientes escenarios: 
>1. Los almacenes de datos de origen o receptores usan la identidad administrada o la entidad de servicio como tipo de autenticación del proveedor de claves.
>2. Los almacenes de datos de origen y receptores usan la identidad administrada como tipo de autenticación del proveedor de claves.
>3. Los almacenes de datos de origen y receptores usan la misma entidad de servicio que el tipo de autenticación del proveedor de claves.

## <a name="troubleshoot-connection-issues"></a>Solución de problemas de conexión

1. Configure su instancia de SQL Server para que acepte conexiones remotas. Inicie **SQL Server Management Studio**, haga clic con el botón derecho en **servidor** y seleccione **Propiedades**. Seleccione **Conexiones** en la lista y marque la casilla **Permitir conexiones remotas con este servidor**.

    :::image type="content" source="media/copy-data-to-from-sql-server/AllowRemoteConnections.png" alt-text="Habilitación de conexiones remotas":::

    Consulte los pasos detallados en el artículo [Configurar la opción de configuración del servidor Acceso remoto](/sql/database-engine/configure-windows/configure-the-remote-access-server-configuration-option).

2. Inicie el **Administrador de configuración de SQL Server**. Expanda **Configuración de red de SQL Server** para la instancia que desee y seleccione **Protocols for MSSQLSERVER** (Protocolos para MSSQLSERVER). Los protocolos aparecen en el panel derecho. Para habilitar TCP/IP, haga clic con el botón derecho en **TCP/IP** y seleccione **Habilitar**.

    :::image type="content" source="./media/copy-data-to-from-sql-server/EnableTCPProptocol.png" alt-text="Habilitar TCP/IP":::

    Para obtener más información y maneras alternativas de habilitar el protocolo TCP/IP, consulte [Habilitar o deshabilitar un protocolo de red de servidor](/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol).

3. En la misma ventana, haga doble clic en **TCP/IP** para abrir la ventana **TCP/IP Properties** (Propiedades de TCP/IP).
4. Cambie a la pestaña **Direcciones IP** . Desplácese hacia abajo hasta la sección **IPAll**. Escriba el **puerto TCP**. El valor predeterminado es **1433**.
5. Cree una **regla del Firewall de Windows** en la máquina para permitir el tráfico entrante a través de este puerto. 
6. **Verifique la conexión**: use SQL Server Management Studio en una máquina diferente para conectarse a SQL Server con un nombre completo. Un ejemplo es `"<machine>.<domain>.corp.<company>.com,1433"`.

## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
