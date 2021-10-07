---
title: Copia de los datos al índice de búsqueda
description: Obtenga información sobre cómo insertar o copiar datos en un índice de Azure Search mediante la actividad de copia en una canalización de Azure Data Factory o Synapse Analytics.
titleSuffix: Azure Data Factory & Azure Synapse
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 289a347e0007547b1fdba2ffd2b3a4674efb524d
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124744093"
---
# <a name="copy-data-to-an-azure-cognitive-search-index-using-azure-data-factory-or-synapse-analytics"></a>Copia de datos a un índice de Azure Cognitive Search mediante Azure Data Factory o Synapse Analytics

> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
> * [Versión 1](v1/data-factory-azure-search-connector.md)
> * [Versión actual](connector-azure-search.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe el uso de la actividad de copia en una canalización de Azure Data Factory o Synapse Analytics para copiar datos en un índice de Azure Cognitive Search. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde cualquier almacén de datos de origen compatible en un índice de búsqueda. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-azure-search-using-ui"></a>Creación de un servicio vinculado a Azure Search mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a Azure Search en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña "Administrar" de su área de trabajo de Azure Data Factory o Synapse y seleccione "Servicios vinculados"; a continuación, haga clic en "Nuevo":

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Data Factory":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Synapse":::

2. Encuentre "Buscar" y seleccione el conector de Azure Search.

   :::image type="content" source="media/connector-azure-search/azure-search-connector.png" alt-text="Selección del conector de Azure Search":::    


1. Configure los detalles del servicio, pruebe la conexión y cree el servicio vinculado.

   :::image type="content" source="media/connector-azure-search/configure-azure-search-linked-service.png" alt-text="Configuración de un servicio vinculado a Azure Search":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector de Azure Cognitive Search.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Azure Cognitive Search:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **AzureSearch**. | Sí |
| url | URL del servicio de búsqueda. | Sí |
| key | Clave de administración del servicio de búsqueda. Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) (si el almacén de datos se encuentra en una red privada) o Azure Integration Runtime. Si no se especifica, se usará Azure Integration Runtime. |No |

> [!IMPORTANT]
> Cuando se copian datos desde un almacén de datos en la nube al índice de búsqueda, en el servicio vinculado de Azure Cognitive Search, debe hacer referencia a Azure Integration Runtime con región explícita en connactVia. Establezca como región aquella en la que reside el servicio de búsqueda. Obtenga más información acerca de [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime).

**Ejemplo**:

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": {
                "type": "SecureString",
                "value": "<AdminKey>"
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades compatibles con el conjunto de datos de Azure Cognitive Search.

Para copiar datos en Azure Cognitive Search, se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **AzureSearchIndex**. | Sí |
| indexName | Nombre del índice de búsqueda. El servicio no crea el índice. El índice debe existir en Azure Cognitive Search. | Sí |

**Ejemplo**:

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "typeProperties" : {
            "indexName": "products"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Cognitive Search linked service name>",
            "type": "LinkedServiceReference"
        }
   }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades compatibles con el origen de Azure Cognitive Search.

### <a name="azure-cognitive-search-as-sink"></a>Azure Cognitive Search como receptor

Si va a copiar datos a Azure Cognitive Search, establezca el tipo de origen de la actividad de copia en **AzureSearchIndexSink**. Se admiten las siguientes propiedades en la sección **sink** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **AzureSearchIndexSink**. | Sí |
| writeBehavior | Especifica si, cuando ya haya un documento en el índice, se realizará una operación de combinación o de reemplazo. Consulte la propiedad [WriteBehavior](#writebehavior-property).<br/><br/>Los valores permitidos son: **Merge** (valor predeterminado) y **Upload**. | No |
| writeBatchSize | Carga datos en el índice de búsqueda cuando el tamaño del búfer alcanza el valor de writeBatchSize. Consulte la propiedad [WriteBatchSize](#writebatchsize-property) para obtener más información.<br/><br/>Los valores permitidos son: enteros de 1 a 1000; el valor predeterminado es 1000. | No |
| maxConcurrentConnections |Número máximo de conexiones simultáneas establecidas en el almacén de datos durante la ejecución de la actividad. Especifique un valor solo cuando quiera limitar las conexiones simultáneas.| No |

### <a name="writebehavior-property"></a>Propiedad WriteBehavior

AzureSearchSink realiza una operación upsert al escribir los datos. Es decir, al crear un documento, si la clave de este ya se encuentra en el índice de búsqueda, Azure Cognitive Search actualiza el documento existente en lugar de generar una excepción de conflicto.

AzureSearchSink proporciona los siguientes dos comportamientos de upsert (mediante el SDK de Azure Search):

- **Combinar**: combina todas las columnas del nuevo documento con el existente. En las columnas con valor null del nuevo documento, se conserva el valor del existente.
- **Cargar**: el nuevo documento reemplaza al existente. En cuanto a las columnas no especificadas en el nuevo documento, el valor se establece en null con independencia de que haya un valor distinto de null en el documento existente.

El comportamiento predeterminado es **Combinar**.

### <a name="writebatchsize-property"></a>Propiedad WriteBatchSize

El servicio Azure Cognitive Search permite la creación de documentos como lotes. Un lote puede contener entre 1 y 1000 acciones. Una acción controla un documento para llevar a cabo la operación de combinación o de carga.

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyToAzureSearch",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Cognitive Search output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSearchIndexSink",
                "writeBehavior": "Merge"
            }
        }
    }
]
```

## <a name="data-type-support"></a>Compatibilidad con tipos de datos

En la tabla siguiente se especifica si se admite o no un tipo de datos de Azure Cognitive Search.

| Tipo de datos de Azure Cognitive Search | Compatible con el receptor de Azure Cognitive Search |
| ---------------------- | ------------------------------ |
| String | Y |
| Int32 | Y |
| Int64 | Y |
| Double | Y |
| Boolean | Y |
| DataTimeOffset | Y |
| Matriz de cadenas | N |
| GeographyPoint | N |

Actualmente no se admiten otros tipos de datos, por ejemplo, ComplexType. Para obtener una lista completa de los tipos de datos que admite Azure Cognitive Search, consulte [Tipos de datos admitidos (Azure Cognitive Search)](/rest/api/searchservice/supported-data-types).

## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).