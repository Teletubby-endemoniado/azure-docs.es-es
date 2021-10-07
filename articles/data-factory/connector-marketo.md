---
title: Copia de datos de Marketo (versión preliminar)
description: Aprenda a copiar datos de Marketo en almacenes de datos receptores compatibles mediante una actividad de copia en una canalización de Azure Data Factory o Synapse Analytics.
titleSuffix: Azure Data Factory & Azure Synapse
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.author: chez
author: chez-charlie
ms.openlocfilehash: 16353c131f87118bb64d3be7b4d5dd36744c2a10
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124743975"
---
# <a name="copy-data-from-marketo-using-azure-data-factory-or-synapse-analytics-preview"></a>Copia de datos de Marketo con Azure Data Factory o Synapse Analytics (versión preliminar)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se explica cómo usar la actividad de copia en una canalización de Azure Data Factory o Synapse Analytics para copiar datos de Marketo. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

> [!IMPORTANT]
> Este conector está actualmente en versión preliminar. Puede probarlo y enviarnos sus comentarios. Si desea depender de los conectores de versión preliminar en la solución, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/).

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de Marketo es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos de Marketo en cualquier almacén de datos receptor admitido. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

Actualmente, no se admite la instancia de Marketo que está integrada con CRM externo.

>[!NOTE]
>Este conector de Marketo se basa en la API REST de Marketo. Tenga en cuenta que Marketo tiene un [límite de solicitudes simultáneas](https://developers.marketo.com/rest-api/) en el lado del servicio. Si recibe el error "Error while attempting to use REST API: Max rate limit '100' exceeded with in '20' secs (606)" (Error al intentar usar la API de REST: el límite de velocidad máx. ('100') se superó en '20' s (606)) o el error "Error while attempting to use REST API: Concurrent access limit '10' reached (615)" (Error al intentar usar la API de REST: se alcanzó el límite de acceso simultáneo de '10' (615)), considere la posibilidad de reducir las ejecuciones de actividad de copia simultáneas para reducir el número de solicitudes al servicio.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-marketo-using-ui"></a>Creación de un servicio vinculado a Marketo mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a Marketo en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Data Factory.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque Marketo y seleccione el conector de Marketo.

   :::image type="content" source="media/connector-marketo/marketo-connector.png" alt-text="Captura de pantalla del conector de Marketo.":::    


1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

   :::image type="content" source="media/connector-marketo/configure-marketo-linked-service.png" alt-text="Captura de pantalla de la configuración del servicio vinculado en Marketo.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector de Marketo.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Marketo:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **Marketo** | Sí |
| endpoint | Punto de conexión del servidor de Marketo (es decir, 123-ABC-321.mktorest.com).  | Sí |
| clientId | Identificador de cliente del servicio Marketo.  | Sí |
| clientSecret | Secreto de cliente del servicio Marketo. Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| useEncryptedEndpoints | Especifica si los puntos de conexión de origen de datos se cifran mediante HTTPS. El valor predeterminado es true.  | No |
| useHostVerification | Especifica si se requiere que el nombre de host del certificado del servidor coincida con el nombre de host del servidor al conectarse a través de TLS. El valor predeterminado es true.  | No |
| usePeerVerification | Especifica si se debe verificar la identidad del servidor al conectarse a través de TLS. El valor predeterminado es true.  | No |

**Ejemplo**:

```json
{
    "name": "MarketoLinkedService",
    "properties": {
        "type": "Marketo",
        "typeProperties": {
            "endpoint" : "123-ABC-321.mktorest.com",
            "clientId" : "<clientId>",
            "clientSecret": {
                "type": "SecureString",
                "value": "<clientSecret>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades compatibles con el conjunto de datos de Marketo.

Para copiar datos de Marketo, establezca la propiedad type del conjunto de datos en **MarketoObject**. Se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **MarketoObject** | Sí |
| tableName | Nombre de la tabla. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**

```json
{
    "name": "MarketoDataset",
    "properties": {
        "type": "MarketoObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Marketo linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades compatibles con el origen de Marketo.

### <a name="marketo-as-source"></a>Marketo como origen

Para copiar datos de Marketo, establezca el tipo de origen de la actividad de copia en **MarketoSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **MarketoSource** | Sí |
| Query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM Activitiy_Types"`. | No (si se especifica "tableName" en el conjunto de datos) |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromMarketo",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Marketo input dataset name>",
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
                "type": "MarketoSource",
                "query": "SELECT top 1000 * FROM Activitiy_Types"
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
