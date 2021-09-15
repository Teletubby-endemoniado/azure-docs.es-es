---
title: Copia de datos desde y hacia Salesforce Service Cloud
titleSuffix: Azure Data Factory & Azure Synapse
description: Obtenga información sobre cómo copiar datos desde Salesforce Service Cloud a almacenes de datos receptores compatibles, o bien desde almacenes de datos de origen compatibles a Salesforce Service Cloud a través de una actividad de copia de una canalización de Data Factory.
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 08/30/2021
ms.openlocfilehash: ef7a8ffa73fe03776be38debc523f9d616bda7b1
ms.sourcegitcommit: 851b75d0936bc7c2f8ada72834cb2d15779aeb69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2021
ms.locfileid: "123316835"
---
# <a name="copy-data-from-and-to-salesforce-service-cloud-by-using-azure-data-factory"></a>Copia de datos con Salesforce Service Cloud como origen y destino mediante Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos con Salesforce Service Cloud como origen o destino. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que presenta información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de Salesforce Service Cloud es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos de Salesforce Service Cloud a cualquier almacén de datos receptor compatible. También puede copiar datos de cualquier almacén de datos de origen compatible a Salesforce Service Cloud. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector de Salesforce Service Cloud admite:

- Ediciones de Salesforce Developer, Professional, Enterprise o Unlimited.
- La copia de datos desde y hacia producción, espacio aislado y dominio personalizado de Salesforce.

El conector de Salesforce se basa en la API REST/Bulk de Salesforce. De forma predeterminada, al copiar datos de Salesforce, el conector usa [v45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) y elige automáticamente entre las API de REST y Bulk en función del tamaño de los datos: cuando el conjunto de resultados es grande, se usa la API de Bulk para obtener un rendimiento mayor; al escribir datos en Salesforce, el conector utiliza [v40](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) de la API de Bulk. También puede establecer de forma explicita la versión de la API que se va a usar para leer y escribir datos a través de [`apiVersion` propiedad](#linked-service-properties) en el servicio vinculado.

## <a name="prerequisites"></a>Prerrequisitos

El permiso API debe estar habilitado en Salesforce. Para más información, consulte [How do I enable API access in Salesforce by permission set?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/) (Procedimientos para habilitar el acceso de API en Salesforce mediante un conjunto de permisos)

## <a name="salesforce-request-limits"></a>Límites de solicitudes de Salesforce

Salesforce tiene límites para el número total de solicitudes de API y el de solicitudes de API simultáneas. Tenga en cuenta los siguientes puntos:

- Si el número de solicitudes simultáneas supera el límite, se produce la limitación y verá errores aleatorios.
- Si el número total de solicitudes supera el límite, la cuenta de Salesforce se bloqueará durante 24 horas.

También podría recibir el mensaje de error "REQUEST_LIMIT_EXCEEDED" en ambos escenarios. Consulte la sección "API Request Limits" (Límites de solicitudes de API) en [Salesforce Developer Limits](https://developer.salesforce.com/docs/atlas.en-us.218.0.salesforce_app_limits_cheatsheet.meta/salesforce_app_limits_cheatsheet/salesforce_app_limits_platform_api.htm) (Límites de Salesforce Developer) para más información.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-salesforce-service-cloud-using-ui"></a>Creación de un servicio vinculado a Salesforce Service Cloud mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a Salesforce Service Cloud en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña "Administrar" de su área de trabajo de Azure Data Factory o Synapse y seleccione "Servicios vinculados"; a continuación, haga clic en "Nuevo":

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Data Factory":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Synapse":::

2. Busque Salesforce y seleccione el conector de Salesforce Service Cloud.

    :::image type="content" source="media/connector-salesforce-service-cloud/salesforce-service-cloud-connector.png" alt-text="Selección del conector de Salesforce Service Cloud":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el servicio vinculado.

    :::image type="content" source="media/connector-salesforce-service-cloud/configure-salesforce-service-cloud-linked-service.png" alt-text="Configuración de un servicio vinculado a Salesforce Service Cloud":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector de Salesforce Service Cloud.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado Salesforce.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type |La propiedad type debe establecerse en **SalesforceServiceCloud**. |Sí |
| environmentUrl | Especifique la dirección URL de la instancia de Salesforce Service Cloud. <br> - El valor predeterminado es `"https://login.salesforce.com"`. <br> - Para copiar datos desde el espacio aislado, especifique `"https://test.salesforce.com"`. <br> - Para copiar datos del dominio personalizado, especifique, por ejemplo, `"https://[domain].my.salesforce.com"`. |No |
| username |Especifique el nombre de usuario de la cuenta de usuario. |Sí |
| password |Especifique la contraseña para la cuenta de usuario.<br/><br/>Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |Sí |
| securityToken |Especifique el token de seguridad para la cuenta de usuario. <br/><br/>Para más información acerca de los tokens de seguridad en general, consulte [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm)(Seguridad y la API). El token de seguridad solo se puede omitir si agrega la dirección IP de Integration Runtime a la [lista de direcciones IP de confianza](https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/security_networkaccess.htm) en Salesforce. Cuando use Azure IR, consulte [Direcciones IP de Azure Integration Runtime](azure-integration-runtime-ip-addresses.md).<br/><br/>Para obtener instrucciones sobre cómo restablecer u obtener un token de seguridad consulte el artículo sobre [obtención de un token de seguridad](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Marque este campo como SecureString para almacenarlo de forma segura en Data Factory o [para hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |No |
| apiVersion | Especifique la versión de la API REST/Bulk de Salesforce que se va a usar, por ejemplo, `48.0`. De forma predeterminada, el conector usa [V45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) para copiar datos de Salesforce y [v40](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) para copiar datos en Salesforce. | No |
| connectVia | El [entorno de ejecución de integración](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Si no se especifica, se usará Azure Integration Runtime. | No |

**Ejemplo: Almacenamiento de credenciales en Data Factory**

```json
{
    "name": "SalesforceServiceCloudLinkedService",
    "properties": {
        "type": "SalesforceServiceCloud",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo: Almacenamiento de credenciales en Key Vault**

```json
{
    "name": "SalesforceServiceCloudLinkedService",
    "properties": {
        "type": "SalesforceServiceCloud",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades compatibles con el conjunto de datos de Salesforce Service Cloud.

Para copiar datos con Salesforce Service Cloud como origen y destino, se admiten las siguientes propiedades.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en **SalesforceServiceCloudObject**.  | Sí |
| objectApiName | El nombre del objeto de Salesforce desde el que se van a recuperar los datos. | No para el origen, sí para el receptor |

> [!IMPORTANT]
> La parte "__c" del **nombre de la API** es necesaria para cualquier objeto personalizado.

![Data Factory - Conexión a Salesforce - Nombre de la API](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Ejemplo**:

```json
{
    "name": "SalesforceServiceCloudDataset",
    "properties": {
        "type": "SalesforceServiceCloudObject",
        "typeProperties": {
            "objectApiName": "MyTable__c"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Salesforce Service Cloud linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **RelationalTable**. | Sí |
| tableName | Nombre de la tabla de Salesforce Service Cloud. | No (si se especifica "query" en el origen de la actividad) |

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades admitidas por el origen y el receptor de Salesforce Service Cloud.

### <a name="salesforce-service-cloud-as-a-source-type"></a>Salesforce Service Cloud como tipo de origen

Para copiar datos de Salesforce Service Cloud, en la sección **source** (origen) de la actividad de copia se admiten las siguientes propiedades.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en **SalesforceServiceCloudSource**. | Sí |
| Query |Utilice la consulta personalizada para leer los datos. Puede usar una consulta de SQL-92 o de [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm). Consulte más sugerencias en la sección [Sugerencias de consulta](#query-tips). Si no se especifica la consulta, se recuperarán todos los datos del objeto de Salesforce Service Cloud especificado en "objectApiName" en el conjunto de datos. | No (si se especifica "objectApiName" en el conjunto de datos) |
| readBehavior | Indica si se van a consultar los registros existentes o todos, incluso los que se eliminaron. Si no se especifica, el comportamiento predeterminado es el primero. <br>Valores permitidos: **query** (valor predeterminado), **queryAll**.  | No |

> [!IMPORTANT]
> La parte "__c" del **nombre de la API** es necesaria para cualquier objeto personalizado.

![Data Factory - Conexión a Salesforce - Lista de nombres de API](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromSalesforceServiceCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce Service Cloud input dataset name>",
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
                "type": "SalesforceServiceCloudSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="salesforce-service-cloud-as-a-sink-type"></a>Salesforce Service Cloud como un tipo de receptor

Para copiar datos a Salesforce Service Cloud, en la sección **source** (origen) de la actividad de copia se admiten las siguientes propiedades.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del receptor de la actividad de copia debe establecerse en: **SalesforceServiceCloudSink**. | Sí |
| writeBehavior | El comportamiento de escritura de la operación.<br/>Los valores permitidos son: **Insert** y **Upsert**. | No (el valor predeterminado es Insert) |
| externalIdFieldName | El nombre del campo de identificador externo para la operación de upsert. El campo especificado debe definirse como "Campo de identificador externo" en el objeto de Salesforce Service Cloud. No puede tener valores NULL en los datos de entrada correspondientes. | Sí para "Upsert" |
| writeBatchSize | El recuento de filas de datos escritos en Salesforce Service Cloud en cada lote. | No (el valor predeterminado es 5000) |
| ignoreNullValues | Indica si se omiten los valores NULL de los datos de entrada durante la operación de escritura.<br/>Los valores permitidos son **true** y **false**.<br>- **True**: deje los datos del objeto de destino sin cambiar cuando realice una operación upsert o update. Inserta un valor predeterminado definido al realizar una operación insert.<br/>- **False**: actualice los datos del objeto de destino a NULL cuando realice una operación upsert o update. Inserta un valor NULL al realizar una operación insert. | No (el valor predeterminado es false) |
| maxConcurrentConnections |Número máximo de conexiones simultáneas establecidas en el almacén de datos durante la ejecución de la actividad. Especifique un valor solo cuando quiera limitar las conexiones simultáneas.| No |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyToSalesforceServiceCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce Service Cloud output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceServiceCloudSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Sugerencias de consulta

### <a name="retrieve-data-from-a-salesforce-service-cloud-report"></a>Recuperación de datos a partir de un informe de Salesforce Service Cloud

Puede recuperar datos de informes de Salesforce Service Cloud especificando una consulta como `{call "<report name>"}`. Un ejemplo es `"query": "{call \"TestReport\"}"`.

### <a name="retrieve-deleted-records-from-the-salesforce-service-cloud-recycle-bin"></a>Recuperación de los registros eliminados desde la papelera de reciclaje de Salesforce Service Cloud

Para consultar los registros eliminados temporalmente de la papelera de reciclaje de Salesforce Service Cloud, puede especificar `readBehavior` como `queryAll`. 

### <a name="difference-between-soql-and-sql-query-syntax"></a>Diferencias entre la sintaxis de consulta SQL y SOQL

Al copiar datos de Salesforce Service Cloud, puede usar consultas SOQL o consultas SQL. Tenga en cuenta que estas dos tienen diferente compatibilidad con sintaxis y funciones, no las mezcle. Es recomendable que use la consulta SOQL que se admite de forma nativa en Salesforce Service Cloud. En la tabla siguiente se muestran las diferencias principales:

| Sintaxis | Modo SOQL | Modo SQL |
|:--- |:--- |:--- |
| Selección de columnas | Necesita enumerar los campos que se van a copiar en la consulta, por ejemplo `SELECT field1, filed2 FROM objectname`. | `SELECT *` se admite además de la selección de columna. |
| Comillas | Los nombres de objetos o de campos no pueden entrecomillarse. | Los nombres de objetos o de campos no pueden entrecomillarse, por ejemplo `SELECT "id" FROM "Account"`. |
| Formato de fecha y hora |  Consulte más detalles [aquí](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_dateformats.htm) y ejemplos en la sección siguiente. | Consulte más detalles [aquí](/sql/odbc/reference/develop-app/date-time-and-timestamp-literals) y ejemplos en la sección siguiente. |
| Valores booleanos | Se representan como `False` y `True`, por ejemplo, `SELECT … WHERE IsDeleted=True`. | Se representan como 0 o 1, por ejemplo `SELECT … WHERE IsDeleted=1`. |
| Cambio del nombre de la columna | No compatible. | Admitido, por ejemplo: `SELECT a AS b FROM …`. |
| Relación | Admitido, por ejemplo: `Account_vod__r.nvs_Country__c`. | No compatible. |

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>Recuperación de datos mediante el uso de una cláusula where en la columna DateTime

Cuando se especifica la consulta SQL o SOQL, preste atención a la diferencia del formato de fecha y hora. Por ejemplo:

* **Ejemplo SOQL**:`SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **Ejemplo de SQL**: `SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}`

### <a name="error-of-malformed_query-truncated"></a>Error de MALFORMED_QUERY: Truncated

Si aparece un error "MALFORMED_QUERY: Truncated", normalmente se debe a que tiene la columna de tipo JunctionIdList en datos y Salesforce tiene una restricción en cuanto a la admisión de tales datos con un gran número de filas. Para solucionarlo, pruebe a excluir la columna JunctionIdList o limite el número de filas que desea copiar (puede dividir el proceso en varias ejecuciones de la actividad de copia).

## <a name="data-type-mapping-for-salesforce-service-cloud"></a>Asignación de tipos de datos para Salesforce Service Cloud

Al copiar datos de Salesforce Service Cloud, se usan las siguientes asignaciones de tipos de datos de Salesforce Service Cloud en los tipos de datos provisionales de Data Factory. Para más información acerca de la forma en que la actividad de copia asigna el tipo de datos y el esquema de origen al receptor, consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md).

| Tipo de datos de Salesforce Service Cloud | Tipo de datos provisionales de Data Factory |
|:--- |:--- |
| Numeración automática |String |
| Casilla de verificación |Boolean |
| Moneda |Decimal |
| Date |DateTime |
| Fecha y hora |DateTime |
| Email |String |
| ID |String |
| Relación de búsqueda |String |
| Lista desplegable de selección múltiple |String |
| Number |Decimal |
| Percent |Decimal |
| Teléfono |String |
| Lista desplegable |String |
| Texto |String |
| Área de texto |String |
| Área de texto (largo) |String |
| Área de texto (enriquecido) |String |
| Texto (cifrado) |String |
| URL |String |

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).


## <a name="next-steps"></a>Pasos siguientes
Para ver la lista de almacenes de datos que la actividad de copia de Data Factory admite como orígenes y receptores consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).