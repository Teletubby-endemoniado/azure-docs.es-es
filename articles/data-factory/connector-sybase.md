---
title: Copia de datos de Sybase
description: Obtenga información sobre cómo copiar datos de Sybase en almacenes de datos receptores compatibles a través de una actividad de copia en una canalización de Azure Data Factory o Synapse Analytics.
titleSuffix: Azure Data Factory & Azure Synapse
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.author: jianleishen
ms.openlocfilehash: d487a862c6079272c400db964d2a8e898d3121a6
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124820102"
---
# <a name="copy-data-from-sybase-using-azure-data-factory-or-synapse-analytics"></a>Copia de datos de Sybase con Azure Data Factory o Synapse Analytics
> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
> * [Versión 1](v1/data-factory-onprem-sybase-connector.md)
> * [Versión actual](connector-sybase.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe el uso de la actividad de copia en una canalización de Azure Data Factory o Synapse Analytics para copiar datos de Sybase. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector Sybase es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos desde la base de datos Sybase en cualquier almacén de datos receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector Sybase admite las siguientes funcionalidades:

- SAP Sybase SQL Anywhere (ASA) **versión 16 y posteriores**.
- La copia de datos con autenticación **básica** o de **Windows**.

No se admite Sybase IQ y ASE. En su lugar, puede usar el conector ODBC genérico con el controlador de Sybase.

## <a name="prerequisites"></a>Requisitos previos

Para usar este conector Sybase, necesitará lo siguiente:

- Configurar un entorno Integration Runtime autohospedado. Consulte el artículo sobre [Integration Runtime autohospedado](create-self-hosted-integration-runtime.md) para más información.
- Instalar el [proveedor de datos para Sybase iAnywhere.Data.SQLAnywhere](https://go.microsoft.com/fwlink/?linkid=324846) 16 o posterior en la máquina de Integration Runtime.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-sybase-using-ui"></a>Creación de un servicio vinculado a Sybase mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a Sybase en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar en el área de trabajo de Azure Data Factory o Synapse, seleccione Servicios vinculados y, después, haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Data Factory":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Cree un servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque Sybase y seleccione el conector Sybase.

   :::image type="content" source="media/connector-sybase/sybase-connector.png" alt-text="Seleccione el conector Sybase.":::    


1. Configure los detalles del servicio, pruebe la conexión y cree el servicio vinculado.

   :::image type="content" source="media/connector-sybase/configure-sybase-linked-service.png" alt-text="Configure un servicio vinculado a Sybase.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector Sybase.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado Sybase:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **Sybase** | Sí |
| server | Nombre del servidor de Sybase. |Sí |
| database | Nombre de la base de datos Sybase. |Sí |
| authenticationType | Tipo de autenticación usado para conectarse a la base de datos Sybase.<br/>Los valores permitidos son: **Basic** y **Windows**. |Sí |
| username | Especifique el nombre de usuario para conectarse a la base de datos de Sybase. |Sí |
| password | Especifique la contraseña de la cuenta de usuario especificada para el nombre de usuario. Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Tal y como se mencionó en los [requisitos previos](#prerequisites), se requiere un entorno Integration Runtime autohospedado. |Sí |

**Ejemplo**:

```json
{
    "name": "SybaseLinkedService",
    "properties": {
        "type": "Sybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "authenticationType": "Basic",
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Sybase.

Para copiar datos de Sybase, se admiten las propiedades siguientes:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **SybaseTable** | Sí |
| tableName | Nombre de la tabla de la base de datos Sybase. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**

```json
{
    "name": "SybaseDataset",
    "properties": {
        "type": "SybaseTable",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Sybase linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Si estaba usando un conjunto de datos de tipo `RelationalTable`, todavía se admite tal cual, aunque se aconseja usar el nuevo en el futuro.

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen de Sybase.

### <a name="sybase-as-source"></a>Sybase como origen

Para copiar datos desde Sybase, en la sección **source** de la actividad de copia se admiten las propiedades siguientes:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **SybaseSource** | Sí |
| Query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM MyTable"`. | No (si se especifica "tableName" en el conjunto de datos) |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromSybase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Sybase input dataset name>",
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
                "type": "SybaseSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Si estaba usando un origen de tipo `RelationalSource`, todavía se admite tal cual, aunque se aconseja usar el nuevo en el futuro.

## <a name="data-type-mapping-for-sybase"></a>Asignación de tipos de datos para Sybase

Al copiar datos desde Sybase, se utilizan las siguientes asignaciones de tipos de datos de Sybase a los tipos de datos provisionales usados internamente dentro del servicio. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

Sybase admite tipos de T-SQL. Para ver una tabla de asignación de tipos SQL a tipos de datos provisionales del servicio, consulte la sección [Conector Azure SQL Database: asignación de tipo de datos](connector-azure-sql-database.md#data-type-mapping-for-azure-sql-database).

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).



## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
