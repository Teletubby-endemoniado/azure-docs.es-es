---
title: Copia de datos desde Azure Database for MariaDB
description: Obtenga información sobre cómo copiar datos desde Azure Database for MariaDB en almacenes de datos de receptores admitidos mediante una actividad de copia en una canalización de Azure Data Factory o Synapse Analytics.
titleSuffix: Azure Data Factory & Azure Synapse
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: bea2662f0c9e11703d0267484a7a15f41c1bbe7a
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124761854"
---
# <a name="copy-data-from-azure-database-for-mariadb-using-azure-data-factory-or-synapse-analytics"></a>Copia de datos desde Azure Database for MariaDB mediante Azure Data Factory o Synapse Analytics

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe el uso de la actividad de copia en una canalización de Azure Data Factory o Synapse Analytics para copiar datos de Azure Database for MariaDB. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de Azure Database for MariaDB es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)
 
Puede copiar datos desde Azure Database for MariaDB en cualquier almacén de datos de receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

El servicio proporciona un controlador integrado para habilitar la conectividad. Por lo tanto, no es necesario instalar manualmente ningún controlador mediante este conector.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-azure-database-for-mariadb-using-ui"></a>Creación de un servicio vinculado a Azure Database for MariaDB mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a Azure Database for MariaDB en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña "Administrar" de su área de trabajo de Azure Data Factory o Synapse y seleccione "Servicios vinculados"; a continuación, haga clic en "Nuevo":

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Data Factory.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Captura de pantalla de la creación de un servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque Maria y seleccione el conector de Azure Database for MariaDB.

   :::image type="content" source="media/connector-azure-database-for-mariadb/azure-database-for-mariadb-connector.png" alt-text="Captura de pantalla del conector de Azure Database for MariaDB":::    


1. Configure los detalles del servicio, pruebe la conexión y cree el servicio vinculado.

   :::image type="content" source="media/connector-azure-database-for-mariadb/configure-azure-database-for-mariadb-linked-service.png" alt-text="Captura de pantalla de la configuración del servicio vinculado a Azure Database for MariaDB":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes, se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas para el conector de Azure Database for MariaDB.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Database for MariaDB:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **AzureMariaDB**. | Sí |
| connectionString | Cadena de conexión para conectarse a Azure Database for MariaDB. La encontrará en Azure Portal -> su instancia de Azure Database for MariaDB -> Cadenas de conexión -> ADO.NET uno. <br/> También puede colocar la contraseña en Azure Key Vault y extraer la configuración de `pwd` de la cadena de conexión. Consulte los siguientes ejemplos y el artículo [Almacenamiento de credenciales en Azure Key Vault](store-credentials-in-key-vault.md) con información detallada. | Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Si no se especifica, se usará Azure Integration Runtime. |No |

**Ejemplo**:

```json
{
    "name": "AzureDatabaseForMariaDBLinkedService",
    "properties": {
        "type": "AzureMariaDB",
        "typeProperties": {
            "connectionString": "Server={your_server}.mariadb.database.azure.com; Port=3306; Database={your_database}; Uid={your_user}@{your_server}; Pwd={your_password}; SslMode=Preferred;"
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
    "name": "AzureDatabaseForMariaDBLinkedService",
    "properties": {
        "type": "AzureMariaDB",
        "typeProperties": {
            "connectionString": "Server={your_server}.mariadb.database.azure.com; Port=3306; Database={your_database}; Uid={your_user}@{your_server}; SslMode=Preferred;",
            "pwd": { 
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

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección, se proporciona una lista de las propiedades que admite el conjunto de datos de Azure Database for MariaDB.

Para copiar datos de Azure Database for MariaDB, se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **AzureMariaDBTable**. | Sí |
| tableName | Nombre de la tabla. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**

```json
{
    "name": "AzureDatabaseForMariaDBDataset",
    "properties": {
        "type": "AzureMariaDBTable",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Database for MariaDB linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección, se proporciona una lista de las propiedades que admite el origen de Azure Database for MariaDB.

### <a name="azure-database-for-mariadb-as-source"></a>Azure Database for MariaDB como origen

Para copiar datos de Azure Database for MariaDB, se admiten las siguientes propiedades en la sección de **origen** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **AzureMariaDBSource**. | Sí |
| Query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM MyTable"`. | No (si se especifica "tableName" en el conjunto de datos) |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromAzureDatabaseForMariaDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Database for MariaDB input dataset name>",
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
                "type": "AzureMariaDBSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```
## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
