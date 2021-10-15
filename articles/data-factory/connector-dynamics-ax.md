---
title: Copia de datos de Dynamics AX
description: Aprenda a copiar datos de Dynamics AX en almacenes de datos receptores compatibles mediante una actividad de copia en una canalización de Synapse Analytics o de Azure Data Factory.
titleSuffix: Azure Data Factory & Azure Synapse
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 249feb6b906db9c89b9b77dff022effe5bce4e83
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124778008"
---
# <a name="copy-data-from-dynamics-ax-using-azure-data-factory-or-synapse-analytics"></a>Copia de datos de Dynamics AX mediante Azure Data Factory o Synapse Analytics

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se resume el uso de la actividad de copia en canalizaciones de Azure Data Factory y Synapse Analytics para copiar datos de Dynamics AX. El artículo se basa en [Actividad de copia](copy-activity-overview.md), en el que se ofrece información general acerca de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de Dynamics AX es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos desde Dynamics AX en cualquier almacén de datos de receptor compatible. Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).

De forma específica, este conector de Dynamics AX admite la copia de datos desde Dynamics AX mediante el **protocolo OData** con la **autenticación de la entidad de servicio**.

>[!TIP]
>También puede usar este conector para copiar los datos desde **Dynamics 365 Finance and Operations**. Consulte la [compatibilidad con OData](/dynamics365/unified-operations/dev-itpro/data-entities/odata) y el [método de autenticación](/dynamics365/unified-operations/dev-itpro/data-entities/services-home-page#authentication) de Dynamics 365.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-dynamics-ax-using-ui"></a>Creación de un servicio vinculado a Dynamics AX mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a Dynamics AX en la interfaz Azure Portal usuario.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Data Factory":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque Dynamics y seleccione el conector de Dynamics AX.

    :::image type="content" source="media/connector-dynamics-ax/dynamics-ax-connector.png" alt-text="Seleccione el conector de Dynamics AX.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

    :::image type="content" source="media/connector-dynamics-ax/configure-dynamics-ax-linked-service.png" alt-text="Configuración de un servicio vinculado en Dynamics AX.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que puede usar para definir entidades de Data Factory específicas del conector de Dynamics AX.

## <a name="prerequisites"></a>Requisitos previos

Antes de usar la autenticación de entidad de servicio, siga estos pasos:

1. Registre una entidad de aplicación en Azure Active Directory (Azure AD) como se indica en [Registro de la aplicación con un inquilino de Azure AD](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Anote los siguientes valores; los usará para definir el servicio vinculado:

    - Identificador de aplicación
    - Clave de la aplicación
    - Id. de inquilino

2. Vaya a Dynamics AX y conceda este permiso adecuado de entidad de servicio para tener acceso a su Dynamics AX.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Dynamics AX:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** debe establecerse en **Dynamics AX**. |Sí |
| url | El extremo de OData de instancia de Dynamics AX (o Dynamics 365 Finance and Operations). |Sí |
| servicePrincipalId | Especifique el id. de cliente de la aplicación. | Sí |
| servicePrincipalKey | Especifique la clave de la aplicación. Marque este campo como **SecureString** para almacenarlo de forma segura, o bien haga [referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| tenant | Especifique la información del inquilino (nombre de dominio o identificador de inquilino) en el que reside la aplicación. Para recuperarla, mantenga el puntero del mouse en la esquina superior derecha de Azure Portal. | Sí |
| aadResourceId | Especifique el recurso de AAD para el cual solicita autorización. Por ejemplo, si es la dirección URL de Dynamics es `https://sampledynamics.sandbox.operations.dynamics.com/data/`, el recurso AAD correspondiente suele ser `https://sampledynamics.sandbox.operations.dynamics.com`. | Sí |
| connectVia | Instancia de [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Se puede elegir Azure Integration Runtime o un Integration Runtime autohospedado (si el almacén de datos se encuentra en una red privada). Si no se especifica, se usa el valor predeterminado de Azure Integration Runtime. |No |

**Ejemplo**

```json
{
    "name": "DynamicsAXLinkedService",
    "properties": {
        "type": "DynamicsAX",
        "typeProperties": {
            "url": "<Dynamics AX instance OData endpoint>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource, e.g. https://sampledynamics.sandbox.operations.dynamics.com>"
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}

```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Dynamics AX.

Para ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos y servicios vinculados](concepts-datasets-linked-services.md). 

Para copiar datos desde Dynamics AX, establezca la propiedad **type** del conjunto de datos en **DynamicsAXResource**. Se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del conjunto de datos debe establecerse en **DynamicsAXResource**. | Sí |
| path | La ruta de acceso a la entidad de OData de Dynamics AX. | Sí |

**Ejemplo**

```json
{
    "name": "DynamicsAXResourceDataset",
    "properties": {
        "type": "DynamicsAXResource",
        "typeProperties": {
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Dynamics AX linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

En esta sección se proporciona una lista de las propiedades que admite el origen de Dynamics AX.

Para ver una lista completa de las secciones y propiedades que hay disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md). 

### <a name="dynamics-ax-as-source"></a>Dynamics AX como origen

Para copiar datos desde Dynamics AX, establezca el tipo de **origen** en actividad de copia en **DynamicsAXSource**. La sección **source** de la actividad de copia admite las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del origen de la actividad de copia debe establecerse en **DynamicsAXSource**. | Sí |
| Query | Opciones de consulta de OData para filtrar datos. Ejemplo: `"?$select=Name,Description&$top=5"`.<br/><br/>**Nota**: El conector copia datos de la dirección URL combinada: `[URL specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Para más información, consulte el artículo sobre [componentes de URL de OData](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | No |
| httpRequestTimeout | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para leer los datos de la respuesta. Si no se especifica, el valor predeterminado es **00:30:00** (30 minutos). | No |

**Ejemplo**

```json
"activities":[
    {
        "name": "CopyFromDynamicsAX",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics AX input dataset name>",
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
                "type": "DynamicsAXSource",
                "query": "$top=10"
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

Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).