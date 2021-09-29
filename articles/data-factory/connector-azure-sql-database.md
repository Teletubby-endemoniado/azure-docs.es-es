---
title: Copia y transformación de datos en Azure SQL Database
titleSuffix: Azure Data Factory & Azure Synapse
description: Aprenda a copiar datos con Azure SQL Database como origen o destino y a transformarlos en Azure SQL Database mediante canalizaciones de Azure Data Factory o Azure Synapse Analytics.
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: ce4d1030999978ba5814d7978238c6f70026b34c
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124785123"
---
# <a name="copy-and-transform-data-in-azure-sql-database-by-using-azure-data-factory-or-azure-synapse-analytics"></a>Copia y transformación de datos en Azure SQL Database mediante Azure Data Factory o Azure Synapse Analytics

> [!div class="op_single_selector" title1="Seleccione la versión de Azure Data Factory que usa:"]
>
> - [Versión 1](v1/data-factory-azure-sql-connector.md)
> - [Versión actual](connector-azure-sql-database.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se explica el uso de la actividad de copia de canalizaciones de Azure Data Factory o Azure Synapse para copiar datos con Azure SQL Database como origen y destino, y el uso de Data Flow para transformarlos en Azure SQL Database. Para obtener más información, lea el artículo de introducción para [Azure Data Factory](introduction.md) o [Azure Synapse Analytics](../synapse-analytics/overview-what-is.md).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de Azure SQL Database es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con tabla de [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Asignación de Data Flow](concepts-data-flow-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
- [Actividad GetMetadata](control-flow-get-metadata-activity.md)

Para la actividad de copia, este conector de Azure SQL Database admite estas funciones:

- Copia de datos mediante la autenticación con SQL y la autenticación de tokens de aplicaciones de Azure Active Directory (Azure AD) con una entidad de servicio o identidades administradas para los recursos de Azure.
- Como origen, la recuperación de datos mediante una consulta SQL o un procedimiento almacenado. También puede optar por la copia en paralelo desde un origen de Azure SQL Database; vea la sección [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database) para obtener detalles.
- Como receptor, la creación automática de la tabla de destino si no existe en función del esquema de origen; la anexión de datos a una tabla o la invocación de un procedimiento almacenado con lógica personalizada durante la copia.

Si usa el [nivel sin servidor](../azure-sql/database/serverless-tier-overview.md) de Azure SQL Database, tenga en cuenta que cuando se pausa el servidor, se produce un error en la ejecución de la actividad en lugar de esperar a que esté lista la reanudación automática. Puede agregar reintentos de actividad o encadenar actividades adicionales para asegurarse de que el servidor está activo en la ejecución real.


> [!IMPORTANT]
> Si copia los datos mediante el entorno de ejecución de integración de Azure, configure una [regla de firewall de nivel de servidor](../azure-sql/database/firewall-configure.md) para que los servicios de Azure puedan acceder al servidor.
> Si copia los datos mediante un entorno de ejecución de integración autohospedado, configure el firewall para permitir el intervalo IP apropiado. Dicho intervalo incluye la dirección IP de la máquina que se utiliza para conectarse a Azure SQL Database.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-an-azure-sql-database-linked-service-using-ui"></a>Creación de un servicio vinculado de Azure SQL Database mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado de Azure SQL Database en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

   # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

   :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Data Factory.":::

   # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

   :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

   

2. Busque SQL y seleccione el conector de Azure SQL Database.

    :::image type="content" source="media/connector-azure-sql-database/azure-sql-database-connector.png" alt-text="Seleccione el conector de Azure SQL Database.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

    :::image type="content" source="media/connector-azure-sql-database/configure-azure-sql-database-linked-service.png" alt-text="Captura de pantalla de la configuración del servicio vinculado de Azure SQL Database.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporciona información acerca de las propiedades que se usan para definir las entidades de canalizaciones de Data Factory o Synapse específicas de un conector de Azure SQL Database.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Estas propiedades son compatibles con un servicio vinculado de Azure SQL Database:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** debe establecerse en **AzureSqlDatabase**. | Sí |
| connectionString | Especifique la información necesaria para conectarse a la base de datos de Azure SQL para la propiedad **connectionString**. <br/>También puede poner una contraseña o una clave de entidad de servicio Azure Key Vault. Si se trata de la autenticación de SQL, extraiga la configuración `password` de la cadena de conexión. Para obtener más información, vea el ejemplo de JSON debajo de la tabla y consulte el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| servicePrincipalId | Especifique el id. de cliente de la aplicación. | Sí, al utilizar la autenticación de Azure AD con una entidad de servicio |
| servicePrincipalKey | Especifique la clave de la aplicación. Marque este campo como **SecureString** para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí, al utilizar la autenticación de Azure AD con una entidad de servicio |
| tenant | Especifique la información del inquilino, como el nombre de dominio o identificador de inquilino, en el que reside la aplicación. Para recuperarlo, mantenga el puntero del mouse en la esquina superior derecha de Azure Portal. | Sí, al utilizar la autenticación de Azure AD con una entidad de servicio |
| azureCloudType | Para la autenticación de la entidad de servicio, especifique el tipo de entorno de nube de Azure en el que está registrada la aplicación de Azure AD. <br/> Los valores permitidos son **AzurePublic**, **AzureChina**, **AzureUsGovernment** y **AzureGermany**. De forma predeterminada, se usa el entorno en la nube de la canalización de Data Factory o Synapse. | No |
| alwaysEncryptedSettings | Especifique la información **alwaysencryptedsettings** necesaria para permitir que Always Encrypted proteja los datos confidenciales almacenados en un servidor SQL mediante una identidad administrada o una entidad de servicio. Para obtener más información, vea el ejemplo de JSON debajo de la tabla y consulte la sección [Uso de Always Encrypted](#using-always-encrypted). Si no se especifica, la configuración predeterminada Always Encrypted está deshabilitada. |No |
| connectVia | Este [entorno de ejecución de integración](concepts-integration-runtime.md) se usa para conectarse al almacén de datos. Se puede usar Azure Integration Runtime o un entorno de ejecución de integración autohospedado, si el almacén de datos se encuentra en una red privada. Si no se especifica, se usa el valor predeterminado de Azure Integration Runtime. | No |

> [!NOTE]
> La característica [**Always Encrypted**](/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-ver15&preserve-view=true) de Azure SQL Database ya no se admite. 

Para ver los distintos tipos de autenticación, consulte las secciones siguientes acerca de requisitos previos y ejemplos de JSON, respectivamente:

- [Autenticación de SQL](#sql-authentication)
- [Autenticación de token de la aplicación de Azure AD: entidad de servicio](#service-principal-authentication)
- [Autenticación de token de la aplicación de Azure AD: identidades administradas de recursos de Azure](#managed-identity)

>[!TIP]
>Si recibió un error con el código de error "UserErrorFailedToConnectToSqlServer" y un mensaje parecido a "The session limit for the database is XXX and has been reached" (El límite de sesión de la base de datos es XXX y ya se ha alcanzado), agregue `Pooling=false` a la cadena de conexión e inténtelo de nuevo. `Pooling=false` también se recomienda para la configuración de servicios vinculados de tipo **SHIR (entorno de ejecución de integración autohospedado).** Es posible agregar una agrupación y otros parámetros de conexión como nuevos nombres y valores de parámetro en la sección **Propiedades de conexión adicionales** del formulario de creación de servicios vinculados.

### <a name="sql-authentication"></a>Autenticación SQL

**Ejemplo: uso de la autenticación de SQL**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo: contraseña de Azure Key Vault**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30",
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

**Ejemplo: Use Always Encrypted**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
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

### <a name="service-principal-authentication"></a>Autenticación de entidad de servicio

Para usar la autenticación de token de aplicación de Azure AD basada en la entidad de servicio, realice estos pasos:

1. [Cree una aplicación de Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal) desde Azure Portal. Anote el nombre de la aplicación y los siguientes valores, que definen el servicio vinculado:

    - Identificador de aplicación
    - Clave de la aplicación
    - Id. de inquilino

2. [Aprovisione un administrador de Azure Active Directory](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-database) para el servidor en Azure Portal, si aún no lo ha hecho. El administrador de Azure AD debe ser un usuario de Azure AD o un grupo de Azure AD, pero no puede ser una entidad de servicio. Este paso se realiza con el fin de que, en el siguiente paso, pueda usar una identidad de Azure AD para crear un usuario de base de datos independiente para la entidad de servicio.

3. [Cree usuarios de bases de datos independientes](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) para la entidad de servicio. Conéctese a la base de datos de la que desea copiar datos (o a la que desea copiarlos) mediante alguna herramienta como SQL Server Management Studio, con una identidad de Azure AD que tenga al menos permiso para MODIFICAR CUALQUIER USUARIO. Ejecute el T-SQL siguiente:
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. Conceda a la entidad de servicio los permisos necesarios, tal como lo haría normalmente para los usuarios de SQL, u otros usuarios. Ejecute el código siguiente: Para más opciones, consulte [este documento](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql).

    ```sql
    ALTER ROLE [role name] ADD MEMBER [your application name];
    ```

5. Configure un servicio vinculado a Azure SQL Database en un área de trabajo de Azure Data Factory o Synapse.

#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Ejemplo de servicio vinculado que usa la autenticación de entidad de servicio

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;Connection Timeout=30",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a> Identidades administradas para la autenticación de recursos de Azure

Un área de trabajo de Data Factory o de Synapse se puede asociar con una [identidad administrada para recursos de Azure](data-factory-service-identity.md) que represente el servicio cuando se autentica en otros recursos de Azure. Esta identidad administrada se puede usar para la autenticación de Azure SQL Database. Puede acceder a la factoría designada o al área de trabajo de Synapse y copiar datos desde la base de datos o en la base de datos usando esta identidad.

Para usar la autenticación de identidad administrada, siga estos pasos.

1. [Aprovisione un administrador de Azure Active Directory](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-database) para el servidor en Azure Portal, si aún no lo ha hecho. El administrador de Azure AD puede ser un usuario de Azure AD o un grupo de Azure AD. Si concede al grupo con identidad administrada un rol de administrador, omita los pasos 3 y 4. El administrador tiene acceso total a la base de datos.

2. [Cree usuarios de bases de datos independientes](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) para la identidad administrada. Conéctese a la base de datos de la que desea copiar datos (o a la que desea copiarlos) mediante alguna herramienta como SQL Server Management Studio, con una identidad de Azure AD que tenga al menos permiso para MODIFICAR CUALQUIER USUARIO. Ejecute el T-SQL siguiente:
  
    ```sql
    CREATE USER [your_resource_name] FROM EXTERNAL PROVIDER;
    ```

3. Conceda a la identidad administrada los permisos necesarios, tal como lo haría normalmente para los usuarios de SQL y otros usuarios. Ejecute el código siguiente: Para más opciones, consulte [este documento](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql).

    ```sql
    ALTER ROLE [role name] ADD MEMBER [your_resource_name];
    ```

4. Configure un servicio vinculado de Azure SQL Database.

**Ejemplo**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;Connection Timeout=30"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea obtener una lista completa de secciones y propiedades disponibles para definir los conjuntos de datos, consulte [Conjuntos de datos](./concepts-datasets-linked-services.md).

Las siguientes propiedades se admiten con el conjunto de datos de Azure SQL Database:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del conjunto de datos debe establecerse en **AzureSqlTable**. | Sí |
| esquema | Nombre del esquema. |No para el origen, sí para el receptor  |
| table | Nombre de la tabla o vista. |No para el origen, sí para el receptor  |
| tableName | Nombre de la tabla o vista con el esquema. Esta propiedad permite la compatibilidad con versiones anteriores. Para la nueva carga de trabajo use `schema` y `table`. | No para el origen, sí para el receptor |

### <a name="dataset-properties-example"></a>Ejemplo de propiedades de un conjunto de datos

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
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

Para ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que el origen y el receptor de Azure SQL Database admiten.

### <a name="azure-sql-database-as-the-source"></a>Azure SQL Database como origen

>[!TIP]
>Para cargar datos desde Azure SQL Database de manera eficaz mediante la creación de particiones de datos, vea [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database).

Para copiar datos desde Azure SQL Database, se admiten las siguientes propiedades en la sección de **origen** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del origen de la actividad de copia debe establecerse en **AzureSqlSource**. Todavía se admite el tipo "SqlSource" para la compatibilidad con versiones anteriores. | Sí |
| sqlReaderQuery | Esta propiedad usa la consulta SQL personalizada para leer los datos. Un ejemplo es `select * from MyTable`. | No |
| sqlReaderStoredProcedureName | Nombre del procedimiento almacenado que lee datos de la tabla de origen. La última instrucción SQL debe ser una instrucción SELECT del procedimiento almacenado. | No |
| storedProcedureParameters | Parámetros del procedimiento almacenado.<br/>Los valores permitidos son pares de nombre o valor. Los nombres y las mayúsculas y minúsculas de los parámetros tienen que coincidir con las mismas características de los parámetros de procedimiento almacenado. | No |
| isolationLevel | Especifica el comportamiento de bloqueo de transacción para el origen de SQL. Los valores permitidos son: **ReadCommitted**, **ReadUncommitted**, **RepeatableRead**, **Serializable** y **Snapshot**. Si no se especifica, se utiliza el nivel de aislamiento predeterminado de la base de datos. Vea [este documento](/dotnet/api/system.data.isolationlevel) para obtener más detalles. | No |
| partitionOptions | Especifica las opciones de creación de particiones de datos que se usan para cargar datos desde Azure SQL Database. <br>Los valores permitidos son: **None** (valor predeterminado), **PhysicalPartitionsOfTable** y **DynamicRange**.<br>Cuando se habilita una opción de partición (es decir, no `None`), el grado de paralelismo para cargar datos de forma simultánea desde una instancia de Azure SQL Database se controla mediante el valor [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) en la actividad de copia. | No |
| partitionSettings | Especifique el grupo de configuración para la creación de particiones de datos. <br>Se aplica si la opción de partición no es `None`. | No |
| ***En`partitionSettings`:*** | | |
| partitionColumnName | Especifique el nombre de la columna de origen **de tipo entero o date/datetime*** (`int`, `smallint`, `bigint`, `date`, `smalldatetime`, `datetime`, `datetime2` o `datetimeoffset`) que se va a usar en la creación de particiones por rangos para la copia en paralelo. Si no se especifica, el índice o la clave primaria de la tabla se detectan automáticamente y se usan como columna de partición.<br>Se aplica si la opción de partición es `DynamicRange`. Si usa una consulta para recuperar datos de origen, enlace `?AdfDynamicRangePartitionCondition ` en la cláusula WHERE. Para obtener un ejemplo, vea la sección [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database). | No |
| partitionUpperBound | Valor máximo de la columna de partición para la división del rango de partición. Este valor se usa para decidir el intervalo de particiones, no para filtrar las filas de la tabla. Se crean particiones de todas las filas de la tabla o el resultado de la consulta y se copian. Si no se especifica, la actividad de copia detecta automáticamente el valor.  <br>Se aplica si la opción de partición es `DynamicRange`. Para obtener un ejemplo, vea la sección [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database). | No |
| partitionLowerBound | Valor mínimo de la columna de partición para la división del rango de partición. Este valor se usa para decidir el intervalo de particiones, no para filtrar las filas de la tabla. Se crean particiones de todas las filas de la tabla o el resultado de la consulta y se copian. Si no se especifica, la actividad de copia detecta automáticamente el valor.<br>Se aplica si la opción de partición es `DynamicRange`. Para obtener un ejemplo, vea la sección [Copia en paralelo desde una base de datos SQL](#parallel-copy-from-sql-database). | No |

**Tenga en cuenta los siguientes puntos:**

- Si se especifica **sqlReaderQuery** para **AzureSqlSource**, la actividad de copia ejecuta la consulta en el origen de Azure SQL Database para obtener los datos. También puede indicar un procedimiento almacenado mediante la definición de **sqlReaderStoredProcedureName** y **storedProcedureParameters** si el procedimiento almacenado adopta parámetros.
- Al usar el procedimiento almacenado del origen para recuperar datos, tenga en cuenta que si está diseñado para devolver otro esquema cuando se pasa un valor de parámetro diferente, es posible que encuentre un error o vea un resultado inesperado al importar el esquema desde la interfaz de usuario, o bien al copiar datos en la base de datos SQL con la creación automática de tablas.

#### <a name="sql-query-example"></a>Ejemplo de consulta SQL

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "AzureSqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>Ejemplo de procedimiento almacenado

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "AzureSqlSource",
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

### <a name="stored-procedure-definition"></a>Definición del procedimiento almacenado

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

### <a name="azure-sql-database-as-the-sink"></a>Azure SQL Database como receptor

> [!TIP]
> Más información sobre los comportamientos de escritura, las configuraciones y los procedimientos recomendados que se admiten en [Procedimiento recomendado para cargar datos en Azure SQL Database](#best-practice-for-loading-data-into-azure-sql-database).

Para copiar datos en Azure SQL Database, se admiten las siguientes propiedades en la sección de **receptor** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del receptor de la actividad de copia debe establecerse en **AzureSqlSink**. Todavía se admite el tipo "SqlSink" para la compatibilidad con versiones anteriores. | Sí |
| preCopyScript | Especifique una consulta SQL para que la actividad de copia se ejecute antes de escribir datos en Azure SQL Database. Solo se invoca una vez por cada copia que se ejecuta. Esta propiedad se usa para limpiar los datos cargados previamente. | No |
| tableOption | Especifica si [se crea automáticamente la tabla de receptores](copy-activity-overview.md#auto-create-sink-tables) según el esquema de origen, si no existe. <br>No se admite la creación automática de tablas cuando el receptor especifica un procedimiento almacenado. <br>Los valores permitidos son: `none` (valor predeterminado), `autoCreate`. | No |
| sqlWriterStoredProcedureName | El nombre del procedimiento almacenado que define cómo se aplican los datos de origen en una tabla de destino. <br/>Este procedimiento almacenado se *invoca por lote*. Para las operaciones que solo se ejecuta una vez y que no tiene nada que ver con los datos de origen, como por ejemplo, eliminar o truncar, use la propiedad `preCopyScript`.<br>Vea el ejemplo de [Invocación del procedimiento almacenado desde el receptor de SQL](#invoke-a-stored-procedure-from-a-sql-sink). | No |
| storedProcedureTableTypeParameterName |Nombre del parámetro del tipo de tabla especificado en el procedimiento almacenado.  |No |
| sqlWriterTableType |Nombre del tipo de tabla que se usará en el procedimiento almacenado. La actividad de copia dispone que los datos que se mueven estén disponibles en una tabla temporal con este tipo de tabla. El código de procedimiento almacenado puede combinar los datos copiados con datos existentes. |No |
| storedProcedureParameters |Parámetros del procedimiento almacenado.<br/>Los valores permitidos son pares de nombre y valor. Los nombres y las mayúsculas y minúsculas de los parámetros deben coincidir con las mismas características de los parámetros de procedimiento almacenado. | No |
| writeBatchSize | Número de filas que se va a insertar en la tabla SQL *por lote*.<br/> El valor que se permite es un **entero** (número de filas). De manera predeterminada, el servicio determina dinámicamente el tamaño adecuado del lote en función del tamaño de fila. | No |
| writeBatchTimeout | Tiempo que se concede a la operación de inserción por lotes para que finalice antes de que se agote el tiempo de espera.<br/> El valor permitido es **intervalo de tiempo**. Un ejemplo es "00:30:00" (30 minutos). | No |
| disableMetricsCollection | El servicio recopila métricas, como las DTU de Azure SQL Database, para la optimización del rendimiento de copia y la obtención de recomendaciones, lo que proporciona acceso adicional a la base de datos maestra. Si le preocupa este comportamiento, especifique `true` para desactivarlo. | No (el valor predeterminado es `false`) |
| maxConcurrentConnections |Número máximo de conexiones simultáneas establecidas en el almacén de datos durante la ejecución de la actividad. Especifique un valor solo cuando quiera limitar las conexiones simultáneas.| No |

**Ejemplo 1: Anexión de datos**

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSqlSink",
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
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSqlSink",
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

En la actividad de copia, el conector de Azure SQL Database proporciona creación de particiones de datos integrada para copiar los datos en paralelo. Puede encontrar las opciones de creación de particiones de datos en la pestaña **Origen** de la actividad de copia.

:::image type="content" source="./media/connector-sql-server/connector-sql-partition-options.png" alt-text="Captura de pantalla de las opciones de partición":::

Al habilitar la copia con particiones, la actividad de copia ejecuta consultas en paralelo en el origen de Azure SQL Database para cargar los datos por particiones. El grado en paralelo se controla mediante el valor [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) de la actividad de copia. Por ejemplo, si establece `parallelCopies` en cuatro, el servicio genera y ejecuta al mismo tiempo cuatro consultas de acuerdo con la configuración y la opción de partición que ha especificado, y cada consulta recupera una porción de la instancia de Azure SQL Database.

Se sugiere habilitar la copia en paralelo con la creación de particiones de datos, especialmente si se cargan grandes cantidades de datos de Azure SQL Database. Estas son algunas configuraciones sugeridas para diferentes escenarios. Cuando se copian datos en un almacén de datos basado en archivos, se recomienda escribirlos en una carpeta como varios archivos (solo especifique el nombre de la carpeta), en cuyo caso el rendimiento es mejor que escribirlos en un único archivo.

| Escenario                                                     | Configuración sugerida                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Carga completa de una tabla grande con particiones físicas.        | **Opción de partición**: particiones físicas de la tabla. <br><br/>Durante la ejecución, el servicio detecta automáticamente las particiones físicas y copia los datos por particiones. <br><br/>Para comprobar si la tabla tiene una partición física o no, puede hacer referencia a [esta consulta](#sample-query-to-check-physical-partition). |
| Carga completa de una tabla grande, sin particiones físicas, aunque con una columna de tipo entero o datetime para la creación de particiones de datos. | **Opciones de partición**: partición por rangos dinámica.<br>**Columna de partición** (opcional): especifique la columna usada para crear la partición de datos. Si no se especifica, se usa la columna de índice o clave principal.<br/>**Límite de partición superior** y **límite de partición inferior** (opcional): especifique si quiere determinar el intervalo de la partición. No es para filtrar las filas de la tabla, se crean particiones de todas las filas de la tabla y se copian. Si no se especifica, la actividad de copia detecta automáticamente los valores.<br><br>Por ejemplo, si la columna de partición "ID" tiene valores que van de 1 a 100 y establece el límite inferior en 20 y el superior en 80, con la copia en paralelo establecida en 4, el servicio recupera los datos en 4 particiones: identificadores del rango <=20, del rango [21, 50], del rango [51, 80] y del rango >=81, respectivamente. |
| Carga de grandes cantidades de datos mediante una consulta personalizada, sin particiones físicas, aunque con una columna de tipo entero o date/datetime para la creación de particiones de datos. | **Opciones de partición**: partición por rangos dinámica.<br>**Consulta**: `SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`.<br>**Columna de partición**: especifique la columna usada para crear la partición de datos.<br>**Límite de partición superior** y **límite de partición inferior** (opcional): especifique si quiere determinar el intervalo de la partición. No es para filtrar las filas de la tabla, se crean particiones de todas las filas del resultado de la consulta y se copian. Si no se especifica, la actividad de copia detecta automáticamente el valor.<br><br>Durante la ejecución, el servicio reemplaza `?AdfRangePartitionColumnName` por el nombre real de la columna y los rangos de valor para cada partición y los envía a Azure SQL Database. <br>Por ejemplo, si la columna de partición "ID" tiene valores que van de 1 a 100 y establece el límite inferior en 20 y el superior en 80, con la copia en paralelo establecida en 4, el servicio recupera los datos en 4 particiones: identificadores del rango <=20, del rango [21, 50], del rango [51, 80] y del rango >=81, respectivamente. <br><br>A continuación se muestran más consultas de ejemplo para distintos escenarios:<br> 1. Consulta de la tabla completa: <br>`SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition`<br> 2. Consulta de una tabla con selección de columnas y filtros adicionales de la cláusula where: <br>`SELECT <column_list> FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`<br> 3. Consulta con subconsultas: <br>`SELECT <column_list> FROM (<your_sub_query>) AS T WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`<br> 4. Consulta con partición en subconsulta: <br>`SELECT <column_list> FROM (SELECT <your_sub_query_column_list> FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition) AS T`
|

Procedimientos recomendados para cargar datos con la opción de partición:

1. Seleccione una columna distintiva como columna de partición (como clave principal o clave única) para evitar la asimetría de datos. 
2. Si la tabla tiene una partición integrada, use la opción de partición "Particiones físicas de tabla" para obtener un mejor rendimiento.    
3. Si usa Azure Integration Runtime para copiar datos, puede establecer "[unidades de integración de datos (DIU)](copy-activity-performance-features.md#data-integration-units)" mayores (> 4) para usar más recursos de cálculo. Compruebe los escenarios aplicables allí.
4. "[Grado de paralelismo de copia](copy-activity-performance-features.md#parallel-copy)" controla los números de partición. Si se establece en un número demasiado grande, puede resentirse el rendimiento, así que se recomienda establecerlo como (DIU o número de nodos de IR autohospedados) * (2 a 4).

**Ejemplo: carga completa de una tabla grande con particiones físicas**

```json
"source": {
    "type": "AzureSqlSource",
    "partitionOption": "PhysicalPartitionsOfTable"
}
```

**Ejemplo: consulta con partición por rangos dinámica**

```json
"source": {
    "type": "AzureSqlSource",
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

## <a name="best-practice-for-loading-data-into-azure-sql-database"></a>Procedimiento recomendado para cargar datos en Azure SQL Database

Al copiar datos en Azure SQL Database, puede requerir un comportamiento de escritura diferente:

- [Anexión](#append-data): mis datos de origen solo tienen registros nuevos.
- [Actualización e inserción](#upsert-data): mis datos de origen tienen inserciones y actualizaciones.
- [Sobrescritura](#overwrite-the-entire-table): quiero recargar una tabla de dimensiones completa cada vez.
- [Escritura con lógica personalizada](#write-data-with-custom-logic): necesito un procesamiento adicional antes de la inserción final en la tabla de destino.

Consulte las secciones correspondientes sobre cómo configurar en el servicio y los procedimientos recomendados.

### <a name="append-data"></a>Anexión de datos

La anexión de datos es el comportamiento predeterminado de este conector de receptor de Azure SQL Database. El servicio realiza una inserción masiva para escribir en la tabla de forma eficaz. Puede configurar el origen y el receptor según corresponda en la actividad de copia.

### <a name="upsert-data"></a>Actualización e inserción de datos

**Opción 1:** si tiene una gran cantidad de datos para copiar, puede cargar de forma masiva todos los registros en una tabla de almacenamiento provisional mediante la actividad de copia y luego ejecutar una actividad de procedimiento almacenado para aplicar una instrucción [MERGE](/sql/t-sql/statements/merge-transact-sql) o INSERT/UPDATE de una sola vez. 

La actividad de copia actualmente no admite de forma nativa la carga de datos en una tabla temporal de base de datos. Hay una forma avanzada de configurarla con una combinación de varias actividades; consulte [Optimización de escenarios de upsert masivo de Azure SQL Database](https://github.com/scoriani/azuresqlbulkupsert). A continuación se muestra un ejemplo de uso de una tabla permanente como almacenamiento provisional.

Por ejemplo, puede crear una canalización con una **actividad de copia** encadenada con una **actividad de procedimiento almacenado**. La primera copia datos del almacén de origen en una tabla de almacenamiento provisional de Azure SQL Database, por ejemplo, **UpsertStagingTable**, como nombre de la tabla en el conjunto de datos. Luego, la segunda invoca un procedimiento almacenado para combinar datos de origen de la tabla de almacenamiento provisional en la tabla de destino y limpiar la tabla de almacenamiento provisional.

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

**Opción 3:** puede usar un [flujo de datos de asignación](#sink-transformation), que ofrece métodos insert/upsert/update integrados.

### <a name="overwrite-the-entire-table"></a>Sobrescritura de toda la tabla

Puede configurar la propiedad **preCopyScript** en un receptor de la actividad de copia. En este caso, para cada actividad de copia que se ejecuta, el servicio ejecuta primero el script. Después, ejecuta la copia para insertar los datos. Por ejemplo, para sobrescribir toda la tabla con los datos más recientes, especifique un script para eliminar primero todos los registros antes de realizar la carga masiva de los nuevos datos desde el origen.

### <a name="write-data-with-custom-logic"></a>Escritura de datos con lógica personalizada

Los pasos necesarios para escribir datos con lógica personalizada son similares a los que se describen en la sección [Actualización e inserción de datos](#upsert-data). Si necesita aplicar procesamiento adicional antes de la inserción final de los datos de origen en la tabla de destino, puede cargarlos en una tabla de almacenamiento provisional y luego invocar una actividad de procedimiento almacenado, o invocar un procedimiento almacenado en un receptor de actividad de copia para aplicar datos, o usar un flujo de datos de asignación.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Invocación del procedimiento almacenado desde el receptor de SQL

Al copiar datos en Azure SQL Database, también se puede configurar e invocar un procedimiento almacenado especificado por el usuario con parámetros adicionales en cada lote de la tabla de origen. La característica de procedimiento almacenado aprovecha los [parámetros con valores de tabla](/dotnet/framework/data/adonet/sql/table-valued-parameters).

Cuando los mecanismos de copia integrados no prestan el servicio, se puede usar un procedimiento almacenado. Por ejemplo, si quiere aplicar un procesamiento adicional antes de la inserción final de los datos de origen en la tabla de destino. Otros ejemplos de procesamiento adicional son cuando quiere combinar columnas, buscar valores adicionales e insertar datos en más de una tabla.

En el ejemplo siguiente se muestra cómo usar un procedimiento almacenado para realizar una operación upsert en una tabla de Azure SQL Database. Supongamos que los datos de entrada y la tabla **Marketing** del receptor tienen tres columnas: **ProfileID**, **State** y **Category**. Realice una operación UPSERT en función de la columna **ProfileID** y aplíquela solo a una categoría específica llamada "ProductA".

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

3. En la canalización de Azure Data Factory o Synapse, defina la sección del **receptor SQL** en la actividad de copia como se indica a continuación:

    ```json
    "sink": {
        "type": "AzureSqlSink",
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

Al transformar datos en el flujo de datos de asignación, puede leer y escribir en las tablas de Azure SQL Database. Para más información, vea la [transformación de origen](data-flow-source.md) y la [transformación de receptor](data-flow-sink.md) en los flujos de datos de asignación.

### <a name="source-transformation"></a>Transformación de origen

La configuración específica de Azure SQL Database está disponible en la pestaña **Source Options** (Opciones de origen) de la transformación de origen.

**Entrada:** Seleccione si desea señalar el origen en una tabla (equivale a ```Select * from <table-name>```) o escribir una consulta SQL personalizada.

**Consultar** Si selecciona Consulta en el campo de entrada, escriba una consulta SQL para el origen. Esta configuración invalidará cualquier tabla que haya elegido en el conjunto de datos. Las cláusulas **Ordenar por** no se admiten aquí, pero puede establecer una instrucción SELECT FROM completa. También puede usar las funciones de tabla definidas por el usuario. **select * from udfGetData()** es un UDF in SQL que devuelve una tabla. Esta consulta genera una tabla de origen que puede usar en el flujo de datos. El uso de consultas también es una excelente manera de reducir las filas para pruebas o búsquedas.

**Procedimiento almacenado**: elija esta opción si desea generar una proyección y datos de origen a partir de un procedimiento almacenado que se ejecuta desde la base de datos de origen. Puede escribir el esquema, el nombre del procedimiento y los parámetros, o bien hacer clic en Actualizar para pedir al servicio que detecte los esquemas y los nombres de los procedimientos. Después, puede hacer clic en Importar para importar todos los parámetros de procedimiento mediante el formulario ``@paraName``.

:::image type="content" source="media/data-flow/stored-procedure-2.png" alt-text="Procedimiento almacenado":::

- Ejemplo de SQL: ```Select * from MyTable where customerId > 1000 and customerId < 2000```
- Ejemplo de SQL con parámetros: ``"select * from {$tablename} where orderyear > {$year}"``

**Tamaño del lote**: escriba un tamaño de lote para fragmentar datos grandes en lecturas.

**Nivel de aislamiento**: El valor predeterminado de los orígenes de SQL en Mapping Data Flow es de lectura no confirmada. Puede cambiar el nivel de aislamiento aquí a uno de estos valores:

- Read Committed
- Read Uncommitted
- Repeatable Read
- Serializable
- Ninguno (ignorar el nivel de aislamiento)

:::image type="content" source="media/data-flow/isolationlevel.png" alt-text="Nivel de aislamiento"::::

### <a name="sink-transformation"></a>Transformación de receptor

La configuración específica de Azure SQL Database está disponible en la pestaña **Configuración** de la transformación de receptor.

**Método de actualización**: determina qué operaciones se permiten en el destino de la base de datos. El valor predeterminado es permitir solamente las inserciones. Para realizar las operaciones update, upsert o delete rows, se requiere una transformación de alteración de filas para etiquetar esas acciones. En el caso de las actualizaciones, upserts y eliminaciones, se debe establecer una o varias columnas de clave para determinar la fila que se va a modificar.

:::image type="content" source="media/data-flow/keycolumn.png" alt-text="Columnas de clave":::

El nombre de columna que elija aquí como clave se usará en el servicio como parte de las operaciones posteriores de actualización, upsert y eliminación. Por lo tanto, debe seleccionar una columna que exista en la asignación del receptor. Si no quiere escribir el valor en esta columna de clave, haga clic en "Skip writing key columns" (Omitir escritura de columnas de clave).

Puede parametrizar la columna de clave que se usa aquí para actualizar la tabla de Azure SQL Database de destino. Si tiene varias columnas para una clave compuesta, haga clic en "Expresión personalizada" y podrá agregar contenido dinámico mediante el [lenguaje de expresiones](control-flow-expression-language-functions.md) de flujo de datos del servicio, que puede incluir una matriz de cadenas con nombres de columna para una clave compuesta.

**Acción de tabla**: determina si se deben volver a crear o quitar todas las filas de la tabla de destino antes de escribir.

- None (Ninguna): no se realizará ninguna acción en la tabla.
- Recreate (Volver a crear): se quitará la tabla y se volverá a crear. Obligatorio si se crea una nueva tabla dinámicamente.
- Truncate (Truncar): se quitarán todas las filas de la tabla de destino.

**Tamaño del lote**: controla el número de filas que se escriben en cada cubo. Los tamaños de lote más grandes mejoran la compresión y la optimización de memoria, pero se arriesgan a obtener excepciones de memoria al almacenar datos en caché.

**Usar TempDB**: de manera predeterminada, Data Factory utilizará una tabla temporal global para almacenar los datos como parte del proceso de carga. También puede desactivar la opción "Usar TempDB" y, en su lugar, pedir al servicio que almacene la tabla de almacenamiento temporal en una base de datos de usuario que se encuentra en la base de datos que se utiliza para este receptor.

:::image type="content" source="media/data-flow/tempdb.png" alt-text="Usar TempDB":::

**Scripts SQL anteriores y posteriores**: escriba scripts de SQL de varias líneas que se ejecutarán antes (preprocesamiento) y después (procesamiento posterior) de que los datos se escriban en la base de datos del receptor.

:::image type="content" source="media/data-flow/prepost1.png" alt-text="Scripts previos y posteriores al procesamiento de SQL":::

### <a name="error-row-handling"></a>Control de filas de errores

Al escribir en la base de datos de Azure SQL, es posible que se produzcan errores en determinadas filas de datos debido a las restricciones establecidas por el destino. Estos son algunos de los errores comunes:

*    Los datos binarios o de tipo cadena se truncarían en una tabla.
*    No se puede insertar el valor NULL en la columna.
*    La instrucción INSERT entra en conflicto con la restricción CHECK.

De forma predeterminada, la ejecución de un flujo de datos no funcionará al recibir el primer error. Puede optar por **Continuar en caso de error**, que permite que el flujo de datos se complete, aunque haya filas individuales con errores. El servicio proporciona diferentes opciones para controlar estas filas de error.

**Transaction Commit** (Confirmación de transacción): elija si los datos se escriben en una única transacción o en lotes. Una sola transacción proporcionará peor rendimiento, pero ningún dato escrito será visible para otros usuarios hasta que finalice la transacción.  

**Output rejected data** (Datos rechazados de salida): si está habilitada, puede generar las filas de error en un archivo CSV en Azure Blob Storage o en una cuenta de Azure Data Lake Storage Gen2 de su elección. Las filas de error se escribirán con tres columnas adicionales: la operación SQL, como INSERT o UPDATE, el código de error de flujo de datos y el mensaje de error de la fila.

**Report success on error** (Notificar éxito cuando hay error): si está habilitada, el flujo de datos se marcará como correcto, aunque se encuentren filas de error. 

:::image type="content" source="media/data-flow/sql-error-row-handling.png" alt-text="Control de filas de errores":::


## <a name="data-type-mapping-for-azure-sql-database"></a>Asignación de tipos de Azure SQL Database

Al copiar datos desde o a Azure SQL Database, se utilizan las siguientes asignaciones de tipos de datos de Azure SQL Database en los tipos de datos provisionales de Azure Data Factory. La característica de canalización de Synapse usa las mismas asignaciones, que Azure Data Factory implementa directamente.  Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para más información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Tipo de datos de Azure SQL Database | Tipo de datos provisionales de Data Factory |
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
| TINYINT |Byte |
| UNIQUEIDENTIFIER |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| Xml |String |

>[!NOTE]
> En el caso de los tipos de datos que se asignan al tipo decimal provisional, la actividad de copia actualmente admite una precisión de hasta 28. Si tiene datos con una precisión mayor de 28, considere la posibilidad de convertirlos en una cadena de consulta SQL.

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
>3. Los almacenes de datos de origen y receptores usan la misma entidad de servicio como tipo de autenticación del proveedor de claves.

## <a name="next-steps"></a>Pasos siguientes

Consulte los [formatos y almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de los almacenes de datos que la actividad de copia admite como orígenes y receptores.
