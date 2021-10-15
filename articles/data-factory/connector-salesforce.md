---
title: Copia de datos desde y hacia Salesforce
titleSuffix: Azure Data Factory & Azure Synapse
description: Obtenga información sobre cómo copiar datos desde Salesforce a almacenes de datos receptores compatibles, o bien desde almacenes de datos de origen compatibles a Salesforce a través de una actividad de copia de una canalización de Azure Data Factory o Azure Synapse Analytics.
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/29/2021
ms.openlocfilehash: 6a1c13d8557b49b1481e94bc95eef10fd5e658f6
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129230254"
---
# <a name="copy-data-from-and-to-salesforce-using-azure-data-factory-or-azure-synapse-analytics"></a>Copia de datos en Salesforce con origen y destino mediante Azure Data Factory o Azure Synapse Analytics

> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
> * [Versión 1](v1/data-factory-salesforce-connector.md)
> * [Versión actual](connector-salesforce.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se resume el uso de la actividad de copia en canalizaciones de Azure Data Factory y Azure Synapse para copiar datos con Salesforce como origen y destino. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que presenta información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de Salesforce es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos desde Salesforce a cualquier almacén de datos receptor compatible. También puede copiar datos desde cualquier almacén de datos de origen compatible a Salesforce. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector de Salesforce admite:

- Ediciones de Salesforce Developer, Professional, Enterprise o Unlimited.
- La copia de datos desde y hacia producción, espacio aislado y dominio personalizado de Salesforce.

El conector de Salesforce se basa en la API REST/Bulk de Salesforce. Al copiar datos desde Salesforce, el conector elige automáticamente entre REST y las API masivas en función del tamaño de los datos; cuando el conjunto de resultados es grande, se usa la API masiva para mejorar el rendimiento. Puede establecer explícitamente la versión de API que se usa para leer y escribir datos mediante la [propiedad `apiVersion`](#linked-service-properties) en el servicio vinculado.

>[!NOTE]
>El conector ya no establece la versión predeterminada para Salesforce API. Por compatibilidad con versiones anteriores, si se estableció una versión de API predeterminada antes, sigue funcionando. El valor predeterminado es 45,0 para source y 40,0 para sink.

## <a name="prerequisites"></a>Prerrequisitos

El permiso API debe estar habilitado en Salesforce.

## <a name="salesforce-request-limits"></a>Límites de solicitudes de Salesforce

Salesforce tiene límites para el número total de solicitudes de API y el de solicitudes de API simultáneas. Tenga en cuenta los siguientes puntos:

- Si el número de solicitudes simultáneas supera el límite, se produce la limitación y verá errores aleatorios.
- Si el número total de solicitudes supera el límite, la cuenta de Salesforce se bloqueará durante 24 horas.

También podría recibir el mensaje de error "REQUEST_LIMIT_EXCEEDED" en ambos escenarios. Consulte la sección "API Request Limits" (Límites de solicitudes de API) en [Salesforce Developer Limits](https://developer.salesforce.com/docs/atlas.en-us.218.0.salesforce_app_limits_cheatsheet.meta/salesforce_app_limits_cheatsheet/salesforce_app_limits_platform_api.htm) (Límites de Salesforce Developer) para más información.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-salesforce-using-ui"></a>Creación de un servicio vinculado a Salesforce mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a Salesforce en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Data Factory.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque Salesforce y seleccione el conector de Salesforce.

    :::image type="content" source="media/connector-salesforce/salesforce-connector.png" alt-text="Captura de pantalla del conector de Salesforce.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

    :::image type="content" source="media/connector-salesforce/configure-salesforce-linked-service.png" alt-text="Captura de pantalla de la configuración del servicio vinculado en Salesforce.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades específicas para el conector de Salesforce.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado Salesforce.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type |La propiedad type debe establecerse en: **Salesforce**. |Sí |
| environmentUrl | Especifique la URL de la instancia de Salesforce. <br> - El valor predeterminado es `"https://login.salesforce.com"`. <br> - Para copiar datos desde el espacio aislado, especifique `"https://test.salesforce.com"`. <br> - Para copiar datos del dominio personalizado, especifique, por ejemplo, `"https://[domain].my.salesforce.com"`. |No |
| username |Especifique el nombre de usuario de la cuenta de usuario. |Sí |
| password |Especifique la contraseña para la cuenta de usuario.<br/><br/>Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |Sí |
| securityToken |Especifique el token de seguridad para la cuenta de usuario. <br/><br/>Para más información acerca de los tokens de seguridad en general, consulte [Security and the API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm)(Seguridad y la API). El token de seguridad solo se puede omitir si agrega la dirección IP de Integration Runtime a la [lista de direcciones IP de confianza](https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/security_networkaccess.htm) en Salesforce. Cuando use Azure IR, consulte [Direcciones IP de Azure Integration Runtime](azure-integration-runtime-ip-addresses.md).<br/><br/>Para obtener instrucciones sobre cómo restablecer u obtener un token de seguridad consulte el artículo sobre [obtención de un token de seguridad](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). |No |
| apiVersion | Especifique la versión de la API REST/Bulk de Salesforce que se va a usar, por ejemplo, `52.0`. | No |
| connectVia | El [entorno de ejecución de integración](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Si no se especifica, se usará Azure Integration Runtime. | No |

**Ejemplo: Almacenamiento de credenciales**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
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
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Salesforce.

Para copiar datos desde y hacia Salesforce, establezca la propiedad type del conjunto de datos en **SalesforceObject**. Se admiten las siguientes propiedades.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en **SalesforceObject**.  | Sí |
| objectApiName | El nombre del objeto de Salesforce desde el que se van a recuperar los datos. | No para el origen, sí para el receptor |

> [!IMPORTANT]
> La parte "__c" del **nombre de la API** es necesaria para cualquier objeto personalizado.

:::image type="content" source="media/copy-data-from-salesforce/data-factory-salesforce-api-name.png" alt-text="Nombre de API de la conexión a Salesforce":::

**Ejemplo**:

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "typeProperties": {
            "objectApiName": "MyTable__c"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

>[!NOTE]
>Por compatibilidad con versiones anteriores: Cuando se copian datos de Salesforce, se podrá seguir usando el conjunto de datos de tipo "RelationalTable" anterior, pero se sugiere cambiar al nuevo tipo "SalesforceObject".

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **RelationalTable**. | Sí |
| tableName | Nombre de la tabla de Salesforce. | No (si se especifica "query" en el origen de la actividad) |

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades admitidas por el origen y el receptor de Salesforce.

### <a name="salesforce-as-a-source-type"></a>Salesforce como tipo de origen

Para copiar datos desde Salesforce, establezca el tipo de origen de la actividad de copia en **SalesforceSource**. En la sección **source** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **SalesforceSource**. | Sí |
| Query |Utilice la consulta personalizada para leer los datos. Puede usar una consulta de SQL-92 o de [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm). Consulte más sugerencias en la sección [Sugerencias de consulta](#query-tips). Si no se especifica la consulta, se recuperarán todos los datos del objeto de Salesforce especificado en "objectApiName" en el conjunto de datos. | No (si se especifica "objectApiName" en el conjunto de datos) |
| readBehavior | Indica si se van a consultar los registros existentes o todos, incluso los que se eliminaron. Si no se especifica, el comportamiento predeterminado es el primero. <br>Valores permitidos: **query** (valor predeterminado), **queryAll**.  | No |

> [!IMPORTANT]
> La parte "__c" del **nombre de la API** es necesaria para cualquier objeto personalizado.

:::image type="content" source="media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png" alt-text="Conexión a Salesforce - Lista de nombres de API":::

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Por compatibilidad con versiones anteriores: Cuando se copian datos de Salesforce, se podrá seguir usando la copia de tipo "RelationalSource" anterior, pero se sugiere cambiar al nuevo tipo "SalesforceSource".

### <a name="salesforce-as-a-sink-type"></a>Salesforce como tipo de receptor

Para copiar datos hacia Salesforce, establezca el tipo de receptor de la actividad de copia en **SalesforceSink**. En la sección **sink** de la actividad de copia se admiten las siguientes propiedades.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del receptor de la actividad de copia debe establecerse en: **SalesforceSink**. | Sí |
| writeBehavior | El comportamiento de escritura de la operación.<br/>Los valores permitidos son: **Insert** y **Upsert**. | No (el valor predeterminado es Insert) |
| externalIdFieldName | El nombre del campo de identificador externo para la operación de upsert. El campo especificado debe definirse como "Campo de identificador externo" en el objeto de Salesforce. No puede tener valores NULL en los datos de entrada correspondientes. | Sí para "Upsert" |
| writeBatchSize | El recuento de filas de datos escritos en Salesforce en cada lote. | No (el valor predeterminado es 5000) |
| ignoreNullValues | Indica si se omiten los valores NULL de los datos de entrada durante la operación de escritura.<br/>Los valores permitidos son **true** y **false**.<br>- **True**: deje los datos del objeto de destino sin cambiar cuando realice una operación upsert o update. Inserta un valor predeterminado definido al realizar una operación insert.<br/>- **False**: actualice los datos del objeto de destino a NULL cuando realice una operación upsert o update. Inserta un valor NULL al realizar una operación insert. | No (el valor predeterminado es false) |
| maxConcurrentConnections |Número máximo de conexiones simultáneas establecidas en el almacén de datos durante la ejecución de la actividad. Especifique un valor solo cuando quiera limitar las conexiones simultáneas.| No |

**Ejemplo: receptor de Salesforce en la actividad de copia**

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
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

### <a name="retrieve-data-from-a-salesforce-report"></a>Recuperación de datos a partir de un informe de Salesforce

Puede recuperar datos de informes de Salesforce especificando una consulta como `{call "<report name>"}`. Un ejemplo es `"query": "{call \"TestReport\"}"`.

### <a name="retrieve-deleted-records-from-the-salesforce-recycle-bin"></a>Recuperación de los registros eliminados desde la papelera de reciclaje de Salesforce

Para consultar los registros eliminados temporalmente de la papelera de reciclaje de Salesforce, puede especificar `readBehavior` como `queryAll`. 

### <a name="difference-between-soql-and-sql-query-syntax"></a>Diferencias entre la sintaxis de consulta SQL y SOQL

Al copiar datos desde Salesforce, puede usar consultas SOQL o consultas SQL. Tenga en cuenta que estas dos tienen diferente compatibilidad con sintaxis y funciones, no las mezcle. Es recomendable usar la consulta SOQL, que se admite de forma nativa en Salesforce. En la tabla siguiente se muestran las diferencias principales:

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

## <a name="data-type-mapping-for-salesforce"></a>Asignación de tipos de datos para Salesforce

Al copiar datos desde Salesforce, se usan las siguientes asignaciones de tipos de datos de Salesforce en los tipos de datos provisionales del servicio. Para más información acerca de la forma en que la actividad de copia asigna el tipo de datos y el esquema de origen al receptor, consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md).

| Tipos de datos de Salesforce | Tipo de datos provisional del servicio |
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
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).