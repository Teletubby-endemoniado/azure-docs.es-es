---
title: Copia de datos de PostgreSQL
titleSuffix: Azure Data Factory & Azure Synapse
description: Obtenga información sobre cómo copiar datos de PostgreSQL en almacenes de datos receptores compatibles a través de una actividad de copia en una canalización de Azure Data Factory o Synapse Analytics.
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.author: jianleishen
ms.openlocfilehash: 4f9ac376379141f91bacd3c941b15a819f12bb2c
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124824731"
---
# <a name="copy-data-from-postgresql-using-azure-data-factory-or-synapse-analytics"></a>Copia de datos de PostgreSQL con Azure Data Factory o Synapse Analytics
> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
> * [Versión 1](v1/data-factory-onprem-postgresql-connector.md)
> * [Versión actual](connector-postgresql.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se resume el uso de la actividad de copia en canalizaciones de Azure Data Factory y Synapse Analytics para copiar datos de una base de datos PostgreSQL. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector PostgreSQL es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos desde la base de datos PostgreSQL en cualquier almacén de datos receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector PostgreSQL es compatible con la **versión 7.4 y posterior** de PostgreSQL.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [data-factory-v2-integration-runtime-requirements](includes/data-factory-v2-integration-runtime-requirements.md)]

El entorno de Integration Runtime proporciona un controlador de PostgreSQL integrado a partir de la versión 3.7, por lo tanto, no es necesario que instale uno manualmente.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-postgresql-using-ui"></a>Creación de un servicio vinculado a PostgreSQL mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a PostgreSQL en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Data Factory":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque PostgreSQL y seleccione el conector de PostgreSQL.

    :::image type="content" source="media/connector-postgresql/postgresql-connector.png" alt-text="Seleccione el conector de PostgreSQL.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

    :::image type="content" source="media/connector-postgresql/configure-postgresql-linked-service.png" alt-text="Configuración de un servicio vinculado en PostgreSQL.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector PostgreSQL.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado PostgreSQL:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **PostgreSql** | Sí |
| connectionString | Cadena de conexión de ODBC para conectarse a Azure Database for PostgreSQL. <br/>También puede colocar la contraseña en Azure Key Vault y extraer la configuración de `password` de la cadena de conexión. Consulte los siguientes ejemplos y el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md) con información detallada. | Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Obtenga más información en la sección [Requisitos previos](#prerequisites). Si no se especifica, se usará Azure Integration Runtime. |No |

Una cadena de conexión típica es `Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Más propiedades que puede establecer para su caso:

| Propiedad | Descripción | Opciones | Obligatorio |
|:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| Método que usa el controlador para cifrar los datos enviados entre el controlador y el servidor de bases de datos. Por ejemplo, `EncryptionMethod=<0/1/6>;`| 0 (sin cifrado) **(valor predeterminado)** / 1 (SSL) / 6 (RequestSSL) | No |
| ValidateServerCertificate (VSC) | Determina si el controlador valida el certificado que envía el servidor de bases de datos cuando está habilitado el cifrado SSL (método de cifrado = 1). Por ejemplo, `ValidateServerCertificate=<0/1>;`| 0 (deshabilitado) **(valor predeterminado)** / 1 (habilitado) | No |

**Ejemplo**:

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo: Almacenamiento de la contraseña en Azure Key Vault**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "connectionString": "Server=<server>;Database=<database>;Port=<port>;UID=<username>;",
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

Si usa el servicio vinculado PostgreSQL con la siguiente carga, todavía se admite tal cual, aunque se aconseja usar la versión nueva.

**Carga anterior:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "username": "<username>",
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

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de PostgreSQL.

Para copiar datos de PostgreSQL, se admiten las propiedades siguientes:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **PostgreSqlTable** | Sí |
| esquema | Nombre del esquema. |No (si se especifica "query" en el origen de la actividad)  |
| table | Nombre de la tabla. |No (si se especifica "query" en el origen de la actividad)  |
| tableName | Nombre de la tabla con el esquema. Esta propiedad permite la compatibilidad con versiones anteriores. Use `schema` y `table` para la carga de trabajo nueva. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**

```json
{
    "name": "PostgreSQLDataset",
    "properties":
    {
        "type": "PostgreSqlTable",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<PostgreSQL linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Si estaba usando un conjunto de datos de tipo `RelationalTable`, todavía se admite tal cual, aunque se aconseja usar el nuevo en el futuro.

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen de PostgreSQL.

### <a name="postgresql-as-source"></a>PostgreSQL como origen

Para copiar datos desde PostgreSQL, en la sección **source** de la actividad de copia se admiten las propiedades siguientes:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **PostgreSqlSource** | Sí |
| Query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"query": "SELECT * FROM \"MySchema\".\"MyTable\""`. | No (si se especifica "tableName" en el conjunto de datos) |

> [!NOTE]
> Los nombres de esquema y tabla distinguen mayúsculas de minúsculas. Incluya los nombres entre comillas dobles `""` en la consulta.

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<PostgreSQL input dataset name>",
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
                "type": "PostgreSqlSource",
                "query": "SELECT * FROM \"MySchema\".\"MyTable\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Si estaba usando un origen de tipo `RelationalSource`, todavía se admite tal cual, aunque se aconseja usar el nuevo en el futuro.

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).


## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
