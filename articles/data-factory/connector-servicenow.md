---
title: Copia de datos de ServiceNow
titleSuffix: Azure Data Factory & Azure Synapse
description: Obtenga información sobre cómo copiar datos de ServiceNow en almacenes de datos receptores compatibles a través de una actividad de copia en una canalización de Azure Data Factory o Synapse Analytics.
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 505874b03e5a5c187f0bb5ea638d09808ffb418c
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130045624"
---
# <a name="copy-data-from-servicenow-using-azure-data-factory-or-synapse-analytics"></a>Copia de datos de ServiceNow con Azure Data Factory o Synapse Analytics
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se resume el uso de la actividad de copia en canalizaciones de Azure Data Factory y Synapse Analytics para copiar datos de ServiceNow. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector ServiceNow es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos de ServiceNow en cualquier almacén de datos de receptor compatible. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

El servicio proporciona un controlador integrado para permitir la conectividad.  Por lo tanto, no es necesario instalar manualmente ningún controlador con este conector.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-servicenow-using-ui"></a>Creación de un servicio vinculado a ServiceNow mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a ServiceNow en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Data Factory":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque ServiceNow y seleccione el conector de ServiceNow.

    :::image type="content" source="media/connector-servicenow/servicenow-connector.png" alt-text="Seleccione el conector de ServiceNow.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

    :::image type="content" source="media/connector-servicenow/configure-servicenow-linked-service.png" alt-text="Configure un servicio vinculado a ServiceNow.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas para el conector de ServiceNow.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de ServiceNow:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **ServiceNow** | Sí |
| endpoint | El punto de conexión del servidor de ServiceNow (`http://<instance>.service-now.com`).  | Sí |
| authenticationType | Tipo de autenticación que se debe usar. <br/>Los valores permitidos son: **Basic** y **OAuth2** | Sí |
| username | Nombre de usuario utilizado para conectarse al servidor de ServiceNow para la autenticación Basic y OAuth2.  | Sí |
| password | Contraseña correspondiente al nombre de usuario para la autenticación Basic y OAuth2. Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| clientId | Id. de cliente para la autenticación OAuth2.  | No |
| clientSecret | Secreto de cliente para la autenticación OAuth2. Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | No |
| useEncryptedEndpoints | Especifica si los puntos de conexión de origen de datos se cifran mediante HTTPS. El valor predeterminado es true.  | No |
| useHostVerification | Especifica si se requiere que el nombre de host del certificado del servidor coincida con el nombre de host del servidor al conectarse a través de TLS. El valor predeterminado es true.  | No |
| usePeerVerification | Especifica si se debe verificar la identidad del servidor al conectarse a través de TLS. El valor predeterminado es true.  | No |

**Ejemplo**:

```json
{
    "name": "ServiceNowLinkedService",
    "properties": {
        "type": "ServiceNow",
        "typeProperties": {
            "endpoint" : "http://<instance>.service-now.com",
            "authenticationType" : "Basic",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades compatibles con el conjunto de datos de ServiceNow.

Para copiar datos de ServiceNow, establezca la propiedad type del conjunto de datos en **ServiceNowObject**. Se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **ServiceNowObject** | Sí |
| tableName | Nombre de la tabla. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**

```json
{
    "name": "ServiceNowDataset",
    "properties": {
        "type": "ServiceNowObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<ServiceNow linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades compatibles con el origen de ServiceNow.

### <a name="servicenow-as-source"></a>ServiceNow como origen

Para copiar datos de ServiceNow, establezca el tipo de origen de la actividad de copia en **ServiceNowSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **ServiceNowSource** | Sí |
| Query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM Actual.alm_asset"`. | No (si se especifica "tableName" en el conjunto de datos) |

Tenga en cuenta lo siguiente cuando especifique el esquema y la columna para ServiceNow en la consulta, y **consulte los [consejos de rendimiento](#performance-tips) en la implicación de rendimiento de copia**.

- **Esquema:** especifique el esquema como `Actual` o `Display` en la consulta de ServiceNow, que puede considerar como el parámetro de `sysparm_display_value` verdadero o falso al llamar a las [API de REST de ServiceNow](https://developer.servicenow.com/app.do#!/rest_api_doc?v=jakarta&id=r_AggregateAPI-GET). 
- **Columna:** el nombre de columna del valor real del esquema `Actual` es `[column name]_value`, mientras que el valor de visualización del esquema `Display` es `[column name]_display_value`. Tenga en cuenta que el nombre de columna se debe asignar al esquema que se usa en la consulta.

**Consulta de ejemplo:** 
`SELECT col_value FROM Actual.alm_asset` O `SELECT col_display_value FROM Display.alm_asset`

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromServiceNow",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ServiceNow input dataset name>",
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
                "type": "ServiceNowSource",
                "query": "SELECT * FROM Actual.alm_asset"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```
## <a name="performance-tips"></a>Consejos de rendimiento

### <a name="schema-to-use"></a>Esquema que se va a usar

ServiceNow tiene dos esquemas diferentes: uno se denomina **"Real"** , ya que devuelve los datos reales y el otro se denomina **"Mostrar"** , ya que devuelve los valores de visualización de datos. 

Si tiene un filtro en su consulta, use el esquema "Real" que tenga un mejor rendimiento de copia. Al realizar una consulta con el esquema "Real", ServiceNow admite de forma nativa el filtro al obtener los datos que usará para devolver solo el conjunto de resultados filtrado; asimismo, al consultar el esquema "Mostrar", ADF recupera todos los datos y aplica el filtro internamente.

### <a name="index"></a>Índice

El índice de la tabla de ServiceNow puede ayudarle a mejorar el rendimiento de las consultas; para ello, consulte [Create a table index](https://docs.servicenow.com/bundle/quebec-platform-administration/page/administer/table-administration/task/t_CreateCustomIndex.html) (Crear un índice de tabla).

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).


## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
