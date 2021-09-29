---
title: Copia y transformación de datos desde y hacia un punto de conexión de REST mediante Azure Data Factory
titleSuffix: Azure Data Factory & Azure Synapse
description: Aprenda a usar la actividad Copiar para copiar datos y Flujo de datos para transformar datos desde un origen REST local o en la nube a almacenes de datos receptores compatibles, o desde un almacén de datos de origen compatible a un receptor REST en canalizaciones de Azure Data Factory o Azure Synapse Analytics.
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.author: makromer
ms.openlocfilehash: dc9aec86e01655087a64c3ac0a494d448889f857
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124733083"
---
# <a name="copy-and-transform-data-from-and-to-a-rest-endpoint-by-using-azure-data-factory"></a>Copia y transformación de datos desde y hacia un punto de conexión de REST mediante Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos desde un punto de conexión de REST y hacia allí. El artículo se basa en [Actividad de copia en Azure Data Factory](copy-activity-overview.md), en el que se ofrece información general acerca de la actividad de copia.

Las diferencias entre este conector REST, el [conector HTTP](connector-http.md) y el [conector de tabla web](connector-web-table.md) son:

- El **conector REST** admite específicamente la copia de datos desde API de RESTful; 
- El **conector HTTP** es genérico y puede recuperar datos desde cualquier punto de conexión HTTP, por ejemplo, para descargar archivos. Antes de este conector REST, puede usar el conector HTTP para copiar datos de la API RESTful, lo que se admite, pero es menos funcional en comparación con el conector REST.
- El **conector de tabla web** extrae contenido de la tabla de una página web HTML.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde un origen REST a cualquier almacén de datos receptor compatible. También puede copiar datos desde cualquier almacén de datos de origen compatible hacia un receptor de REST. Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).

En concreto, este conector REST genérico admite lo siguiente:

- La copia de datos desde un punto de conexión de REST mediante los métodos **GET** o **POST** y la copia de datos hacia un punto de conexión de REST mediante los métodos **POST**, **PUT** o **PATCH**.
- Copiar datos mediante el uso de una de las autenticaciones siguientes: **Anónima**, **Básica**, **Entidad de servicio de AAD** e **identidades administradas para recursos de Azure**.
- **[Paginación](#pagination-support)** en las API REST.
- En el caso de REST como origen, la copia de la respuesta JSON de REST [tal cual](#export-json-response-as-is) o su análisis mediante la [asignación de esquemas](copy-activity-schema-and-type-mapping.md#schema-mapping). Solo se admite la carga de respuesta en **JSON**.

> [!TIP]
> Para probar una solicitud REST para la recuperación de datos antes de configurar el conector REST en Data Factory, obtenga información sobre la especificación de API para los requisitos del cuerpo y del encabezado. Puede usar herramientas como Postman o un explorador web para validar.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [data-factory-v2-integration-runtime-requirements](includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-rest-linked-service-using-ui"></a>Creación de un servicio vinculado REST mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado REST en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Data Factory.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque REST y seleccione el conector de REST.

    :::image type="content" source="media/connector-rest/rest-connector.png" alt-text="Selección del conector de REST.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

    :::image type="content" source="media/connector-rest/configure-rest-linked-service.png" alt-text="Configuración del servicio vinculado REST.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que puede usar para definir entidades de Data Factory específicas del conector REST.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de REST:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** debe establecerse en **RestService**. | Sí |
| url | La dirección URL base del servicio REST. | Sí |
| enableServerCertificateValidation | Si se debe validar el certificado TLS/SSL del lado servidor al conectarse al punto de conexión. | No<br /> (El valor predeterminado es: **true**) |
| authenticationType | El tipo de autenticación usado para conectarse al servicio REST. Los valores que se permiten son: **Anónima**, **Básica**, **AadServicePrincipal** y **ManagedServiceIdentity**. No se admiten usuarios basados en OAuth. Además, puede configurar encabezados de autenticación en la propiedad `authHeader`. Haga referencia a las siguientes secciones correspondientes para obtener más información sobre propiedades y ejemplos, respectivamente.| Sí |
| authHeaders | Encabezados de solicitud HTTP adicionales para la autenticación.<br/> Por ejemplo, para usar la autenticación de clave de API, puede seleccionar el tipo de autenticación "Anónima" y especificar la clave de API en el encabezado. | No |
| connectVia | Instancia de [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Obtenga más información en la sección [Requisitos previos](#prerequisites). Si no se especifica, esta propiedad se usará Azure Integration Runtime. |No |

### <a name="use-basic-authentication"></a>Uso de la autenticación básica

Establezca la propiedad **authenticationType** en **Basic**. Además de las propiedades genéricas descritas en las secciones anteriores, especifique las siguientes:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| userName | El nombre de usuario para acceder al punto de conexión REST. | Sí |
| password | Contraseña del usuario (valor **userName**). Marque este campo como de tipo **SecureString** para almacenarlo de forma segura en Data Factory. También puede [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |

**Ejemplo**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<REST endpoint>",
            "userName": "<user name>",
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

### <a name="use-aad-service-principal-authentication"></a>Uso de la autenticación de entidad de servicio de AAD

Establezca la propiedad **authenticationType** en **AadServicePrincipal**. Además de las propiedades genéricas descritas en las secciones anteriores, especifique las siguientes:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| servicePrincipalId | Especifique el identificador de cliente de la aplicación de Azure Active Directory. | Sí |
| servicePrincipalKey | Especifique la clave de la aplicación de Azure Active Directory. Marque este campo como [SecureString](store-credentials-in-key-vault.md) para almacenarlo de forma segura en Data Factory, o bien **para hacer referencia a un secreto almacenado en Azure Key Vault**. | Sí |
| tenant | Especifique la información del inquilino (nombre de dominio o identificador de inquilino) en el que reside la aplicación. Para recuperarla, mantenga el puntero del mouse en la esquina superior derecha de Azure Portal. | Sí |
| aadResourceId | Especifique el recurso de AAD para el que solicita autorización, por ejemplo, `https://management.core.windows.net`.| Sí |
| azureCloudType | Para la autenticación de la entidad de servicio, especifique el tipo de entorno de nube de Azure en el que está registrada la aplicación de AAD. <br/> Los valores permitidos son **AzurePublic**, **AzureChina**, **AzureUsGovernment** y **AzureGermany**. De forma predeterminada, se usa el entorno de nube de la factoría de datos. | No |

**Ejemplo**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint e.g. https://www.example.com/>",
            "authenticationType": "AadServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "value": "<service principal key>",
                "type": "SecureString"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "aadResourceId": "<AAD resource URL e.g. https://management.core.windows.net>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-system-assigned-managed-identity-authentication"></a><a name="managed-identity"></a> Uso de la autenticación de identidad administrada asignada por el sistema

Establezca la propiedad **authenticationType** en **ManagedServiceIdentity**. Además de las propiedades genéricas descritas en las secciones anteriores, especifique las siguientes:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| aadResourceId | Especifique el recurso de AAD para el que solicita autorización, por ejemplo, `https://management.core.windows.net`.| Sí |

**Ejemplo**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint e.g. https://www.example.com/>",
            "authenticationType": "ManagedServiceIdentity",
            "aadResourceId": "<AAD resource URL e.g. https://management.core.windows.net>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-user-assigned-managed-identity-authentication"></a>Uso de la autenticación de identidad administrada asignada por el usuario
Establezca la propiedad **authenticationType** en **ManagedServiceIdentity**. Además de las propiedades genéricas descritas en las secciones anteriores, especifique las siguientes:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| aadResourceId | Especifique el recurso de AAD para el que solicita autorización, por ejemplo, `https://management.core.windows.net`.| Sí |
| credentials | Especifique la identidad administrada asignada por el usuario como objeto de credencial. | Sí |


**Ejemplo**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint e.g. https://www.example.com/>",
            "authenticationType": "ManagedServiceIdentity",
            "aadResourceId": "<AAD resource URL e.g. https://management.core.windows.net>",
            "credential": {
                "referenceName": "credential1",
                "type": "CredentialReference"
            }    
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-authentication-headers"></a>Uso de los encabezados de autenticación

Además, puede configurar los encabezados de solicitud para realizar la autenticación, junto con los tipos de autenticación integrados.

**Ejemplo: uso de la autenticación mediante clave de API**

```json
{
    "name": "RESTLinkedService",
    "properties": {
        "type": "RestService",
        "typeProperties": {
            "url": "<REST endpoint>",
            "authenticationType": "Anonymous",
            "authHeader": {
                "x-api-key": {
                    "type": "SecureString",
                    "value": "<API key>"
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

En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de REST. 

Para ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos y servicios vinculados](concepts-datasets-linked-services.md). 

Para copiar datos de REST, se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del conjunto de datos debe establecerse en **RestResource**. | Sí |
| relativeUrl | Dirección URL relativa al recurso que contiene los datos. Cuando no se especifica la propiedad, solo se usa la dirección URL especificada en la definición del servicio vinculado. El conector HTTP copia los datos de la dirección URL combinada: `[URL specified in linked service]/[relative URL specified in dataset]`. | No |

Si ha configurado `requestMethod`, `additionalHeaders`, `requestBody` y `paginationRules` en el conjunto de datos, todavía se admite tal cual, aunque se aconseja usar en el futuro el nuevo modelo en la actividad.

**Ejemplo**:

```json
{
    "name": "RESTDataset",
    "properties": {
        "type": "RestResource",
        "typeProperties": {
            "relativeUrl": "<relative url>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<REST linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

En esta sección se proporciona una lista de las propiedades compatibles con REST como origen y receptor.

Para ver una lista completa de las secciones y propiedades que hay disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md). 

### <a name="rest-as-source"></a>REST como origen

Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del origen de la actividad de copia debe establecerse en **RestSource**. | Sí |
| requestMethod | Método HTTP. Los valores permitidos son **GET** (valor predeterminado) y **POST**. | No |
| additionalHeaders | Encabezados de solicitud HTTP adicionales. | No |
| requestBody | Cuerpo de la solicitud HTTP. | No |
| paginationRules | Las reglas de paginación para componer las solicitudes de página siguiente. Vea la sección [Compatibilidad con la paginación](#pagination-support) para obtener más información. | No |
| httpRequestTimeout | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para leer los datos de la respuesta. El valor predeterminado es **00:01:40**.  | No |
| requestInterval | El tiempo de espera antes de enviar la solicitud de página siguiente. El valor predeterminado es **00:00:01** |  No |

>[!NOTE]
>El conector REST ignora cualquier encabezado "Accept" que se especifique en `additionalHeaders`. Como el conector REST solo admite la respuesta en JSON, generará automáticamente un encabezado de `Accept: application/json`.

**Ejemplo 1: Mediante el método Get con la paginación**

```json
"activities":[
    {
        "name": "CopyFromREST",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<REST input dataset name>",
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
                "type": "RestSource",
                "additionalHeaders": {
                    "x-user-defined": "helloworld"
                },
                "paginationRules": {
                    "AbsoluteUrl": "$.paging.next"
                },
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Ejemplo 2: Uso del método POST**

```json
"activities":[
    {
        "name": "CopyFromREST",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<REST input dataset name>",
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
                "type": "RestSource",
                "requestMethod": "Post",
                "requestBody": "<body for POST REST request>",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="rest-as-sink"></a>REST como receptor

Se admiten las siguientes propiedades en la sección **sink** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del receptor de la actividad de copia debe establecerse en **RestSink**. | Sí |
| requestMethod | Método HTTP. Los valores permitidos son **POST** (valor predeterminado), **PUT** y **PATCH**. | No |
| additionalHeaders | Encabezados de solicitud HTTP adicionales. | No |
| httpRequestTimeout | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para escribir los datos. El valor predeterminado es **00:01:40**.  | No |
| requestInterval | El intervalo de tiempo entre diferentes solicitudes en milisegundos. El valor del intervalo de solicitudes debe ser un número entre [10, 60000]. |  No |
| httpCompressionType | Tipo de compresión HTTP que se va a usar al enviar datos con un nivel de compresión óptimo. Los valores permitidos son **ninguno** y **gzip**. | No |
| writeBatchSize | Número de registros que se van a escribir en el receptor de REST por lote. El valor predeterminado es 10000. | No |

El conector de REST como receptor funciona con las API REST que aceptan JSON. Los datos se enviarán en JSON con el siguiente patrón. Según sea necesario, puede usar la [asignación de esquemas](copy-activity-schema-and-type-mapping.md#schema-mapping) de la actividad de copia para cambiar la forma de los datos de origen de modo que se ajusten a la carga esperada por la API REST.

```json
[
    { <data object> },
    { <data object> },
    ...
]
```

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyToREST",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<REST output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "RestSink",
                "requestMethod": "POST",
                "httpRequestTimeout": "00:01:40",
                "requestInterval": 10,
                "writeBatchSize": 10000,
                "httpCompressionType": "none",
            },
        }
    }
]
```

## <a name="mapping-data-flow-properties"></a>Propiedades de Asignación de instancias de Data Flow

REST se admite en flujos de datos de conjuntos de datos de integración y conjuntos de datos en línea.

### <a name="source-transformation"></a>Transformación de origen

| Propiedad | Descripción | Requerido |
|:--- |:--- |:--- |
| requestMethod | Método HTTP. Los valores permitidos son **GET** y **POST**. | Sí |
| relativeUrl | Dirección URL relativa al recurso que contiene los datos. Cuando no se especifica la propiedad, solo se usa la dirección URL especificada en la definición del servicio vinculado. El conector HTTP copia los datos de la dirección URL combinada: `[URL specified in linked service]/[relative URL specified in dataset]`. | No |
| additionalHeaders | Encabezados de solicitud HTTP adicionales. | No |
| httpRequestTimeout | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para escribir los datos. El valor predeterminado es **00:01:40**.  | No |
| requestInterval | El intervalo de tiempo entre diferentes solicitudes en milisegundos. El valor del intervalo de solicitudes debe ser un número entre [10, 60000]. |  No |
| QueryParameters.*request_query_parameter* O QueryParameters["request_query_parameter"] | El usuario define "request_query_parameter", que hace referencia a un nombre de parámetro de consulta en la siguiente dirección URL de solicitud HTTP. | No |

### <a name="sink-transformation"></a>Transformación de receptor

| Propiedad | Descripción | Requerido |
|:--- |:--- |:--- |
| additionalHeaders | Encabezados de solicitud HTTP adicionales. | No |
| httpRequestTimeout | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para escribir los datos. El valor predeterminado es **00:01:40**.  | No |
| requestInterval | El intervalo de tiempo entre diferentes solicitudes en milisegundos. El valor del intervalo de solicitudes debe ser un número entre [10, 60000]. |  No |
| httpCompressionType | Tipo de compresión HTTP que se va a usar al enviar datos con un nivel de compresión óptimo. Los valores permitidos son **ninguno** y **gzip**. | No |
| writeBatchSize | Número de registros que se van a escribir en el receptor de REST por lote. El valor predeterminado es 10000. | No |

Puede establecer los métodos delete, insert, update y upsert, así como los datos de fila relativos que se van a enviar al receptor REST para las operaciones CRUD.

:::image type="content" source="media/data-flow/data-flow-sink.png" alt-text="Receptor REST de flujo de datos":::

## <a name="sample-data-flow-script"></a>Script de flujo de datos de ejemplo

Observe el uso de una transformación alter row antes del receptor para indicar a ADF qué tipo de acción realizar con el receptor REST. Es decir, insert, update, upsert, delete.

```
AlterRow1 sink(allowSchemaDrift: true,
    validateSchema: false,
    deletable:true,
    insertable:true,
    updateable:true,
    upsertable:true,
    rowRelativeUrl: 'periods',
    insertHttpMethod: 'PUT',
    deleteHttpMethod: 'DELETE',
    upsertHttpMethod: 'PUT',
    updateHttpMethod: 'PATCH',
    timeout: 30,
    requestFormat: ['type' -> 'json'],
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> sink1
```

## <a name="pagination-support"></a>Compatibilidad con la paginación

Al copiar datos de las API REST, normalmente la API REST limita su tamaño de carga de respuesta de una única solicitud a un número razonable; mientras que, para devolver una gran cantidad de datos, divide el resultado en varias páginas y exige que los autores de la llamada envíen solicitudes consecutivas para obtener la página siguiente del resultado. Por lo general, la solicitud para una página es dinámica y está compuesta por la información devuelta de la respuesta de página anterior.

Este conector REST genérico admite los siguientes patrones de paginación: 

* Dirección URL absoluta o relativa de la siguiente solicitud = valor de propiedad en el cuerpo de la respuesta actual
* Dirección URL absoluta o relativa de la siguiente solicitud = valor de encabezado en los encabezados de la respuesta actual
* Parámetro de consulta de la siguiente solicitud = valor de propiedad en el cuerpo de la respuesta actual
* Parámetro de consulta de la siguiente solicitud = valor de encabezado en los encabezados de la respuesta actual
* Encabezado de la siguiente solicitud = valor de propiedad en el cuerpo de la respuesta actual
* Encabezado de la siguiente solicitud = valor de encabezado en los encabezados de la respuesta actual

Las **reglas de paginación** se definen como un diccionario en el conjunto de datos que contiene uno o más pares clave-valor que distinguen mayúsculas de minúsculas. La configuración se usará para generar la solicitud a partir de la segunda página. El conector detendrá la iteración cuando obtenga el código de estado HTTP 204 (sin contenido) o cualquiera de las expresiones JSONPath en "paginationRules" devuelva NULL.

**Claves admitidas** en las reglas de paginación:

| Clave | Descripción |
|:--- |:--- |
| AbsoluteUrl | Indica la dirección URL para emitir la siguiente solicitud. Puede ser una **dirección URL absoluta o relativa**. |
| QueryParameters.*request_query_parameter* O QueryParameters["request_query_parameter"] | El usuario define "request_query_parameter", que hace referencia a un nombre de parámetro de consulta en la siguiente dirección URL de solicitud HTTP. |
| Headers.*request_header* O Headers["request_header"] | El usuario define "request_header", que hace referencia a un nombre de encabezado en la siguiente solicitud HTTP. |

**Valores admitidos** en las reglas de paginación:

| Value | Descripción |
|:--- |:--- |
| Headers.*response_header* O Headers["response_header"] | El usuario define "response_header", que hace referencia a un nombre de encabezado en la respuesta HTTP actual, el valor que se usará para emitir la solicitud siguiente. |
| Una expresión JSONPath que empieza por "$" (que representa la raíz del cuerpo de respuesta) | El cuerpo de respuesta debe contener un único objeto JSON. La expresión JSONPath debe devolver un único valor primitivo, que se usará para emitir la solicitud siguiente. |

**Ejemplo**:

Facebook Graph API devuelve la respuesta en la siguiente estructura, en cuyo caso la dirección URL de la siguiente página se representa en ***paging.next***:

```json
{
    "data": [
        {
            "created_time": "2017-12-12T14:12:20+0000",
            "name": "album1",
            "id": "1809938745705498_1809939942372045"
        },
        {
            "created_time": "2017-12-12T14:14:03+0000",
            "name": "album2",
            "id": "1809938745705498_1809941802371859"
        },
        {
            "created_time": "2017-12-12T14:14:11+0000",
            "name": "album3",
            "id": "1809938745705498_1809941879038518"
        }
    ],
    "paging": {
        "cursors": {
            "after": "MTAxNTExOTQ1MjAwNzI5NDE=",
            "before": "NDMyNzQyODI3OTQw"
        },
        "previous": "https://graph.facebook.com/me/albums?limit=25&before=NDMyNzQyODI3OTQw",
        "next": "https://graph.facebook.com/me/albums?limit=25&after=MTAxNTExOTQ1MjAwNzI5NDE="
    }
}
```

La configuración del conjunto de datos REST correspondiente, especialmente la `paginationRules`, es como sigue:

```json
"typeProperties": {
    "source": {
        "type": "RestSource",
        "paginationRules": {
            "AbsoluteUrl": "$.paging.next"
        },
        ...
    },
    "sink": {
        "type": "<sink type>"
    }
}
```

## <a name="use-oauth"></a>Uso de OAuth
En esta sección se describe cómo usar una plantilla de solución para copiar datos del conector REST en Azure Data Lake Storage en formato JSON mediante OAuth. 

### <a name="about-the-solution-template"></a>Acerca de la plantilla de solución

La plantilla contiene dos actividades:
- La actividad **web** recupera el token de portador y, a continuación, lo pasa a la actividad de copia posterior como autorización.
- La actividad de **copia** se encarga de copiar los datos de REST en Azure Data Lake Storage.

La plantilla define dos parámetros:
- **SinkContainer** es la ruta de acceso de la carpeta raíz en la que se copian los datos en Azure Data Lake Storage. 
- **SinkDirectory** es la ruta de acceso del directorio en la raíz en el que se copian los datos en Azure Data Lake Storage. 

### <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Vaya a la plantilla **Copiar desde REST o HTTP con OAuth**. Cree una nueva conexión para la conexión de origen. 
    :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/source-connection.png" alt-text="Creación de conexiones":::

    A continuación se muestran los pasos principales para configurar el nuevo servicio vinculado (REST):
    
     1. En **URL base**, especifique el parámetro de dirección URL para su servicio REST de origen. 
     2. En **Tipo de autenticación**, elija *Anónima*.
        :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/new-rest-connection.png" alt-text="Nueva conexión REST":::

2. Cree una nueva conexión para la conexión de destino.  
    :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/destination-connection.png" alt-text="Conexión de Gen2 nueva":::

3. Seleccione **Usar esta plantilla**.
    :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/use-this-template.png" alt-text="Usar esta plantilla":::

4. Verá la canalización creada tal y como se muestra en el ejemplo siguiente:  :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/pipeline.png" alt-text="Captura de pantalla que muestra la canalización creada a partir de la plantilla.":::

5. Seleccione la actividad **web**. En **Configuración**, especifique la **URL**, **Método**, **Encabezados** y **Cuerpo** correspondientes para recuperar el token de portador OAuth de la API de inicio de sesión del servicio desde el que quiere copiar los datos. El marcador de posición de la plantilla muestra un ejemplo de OAuth de Azure Active Directory (AAD). Nota: la autenticación de AAD es compatible de forma nativa con el conector REST; este es solo un ejemplo de flujo de OAuth. 

    | Propiedad | Descripción |
    |:--- |:--- |
    | URL |Especifique la dirección URL de la que se recuperará el token de portador OAuth. p. ej., en este ejemplo es https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token |
    | Método | Método HTTP. Los valores permitidos son **POST** y **GET**. | 
    | encabezados | El usuario define el encabezado, que hace referencia a un nombre de encabezado en la solicitud HTTP. | 
    | Body | Cuerpo de la solicitud HTTP. | 

    :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/web-settings.png" alt-text="Canalización":::

6. En la actividad **Copiar datos**, seleccione la pestaña *Origen* y verá que el token de portador (access_token) recuperado del paso anterior se pasará a la actividad de copia de datos como **autorización** en los encabezados adicionales. Confirme la configuración de las siguientes propiedades antes de iniciar una ejecución de canalización.

    | Propiedad | Descripción |
    |:--- |:--- |
    | Método de solicitud | Método HTTP. Los valores permitidos son **Get** (valor predeterminado) y **Post**. | 
    | Encabezados adicionales | Encabezados de solicitud HTTP adicionales.| 

   :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/copy-data-settings.png" alt-text="Autenticación para la copia del origen":::

7. Seleccione **Depurar**, escriba los **parámetros** y, a continuación, seleccione **Finalizar**.
   :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/pipeline-run.png" alt-text="Ejecución de la canalización"::: 

8. Cuando la ejecución de la canalización se complete correctamente, verá un resultado similar al ejemplo siguiente: :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/run-result.png" alt-text="Resultado de la ejecución de la canalización"::: 

9. Haga clic en el icono de "Salida" de WebActivity en la columna **Acciones** para ver el access_token devuelto por el servicio.

   :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/token-output.png" alt-text="Salida del token"::: 

10. Haga clic en el icono de "Entrada" de CopyActivity en la columna **Acciones** para ver que el access_token recuperado por WebActivity se pasa a CopyActivity para la autenticación. 

    :::image type="content" source="media/solution-template-copy-from-rest-or-http-using-oauth/token-input.png" alt-text="Entrada del token":::
        
    >[!CAUTION] 
    >Para evitar que el token se registre como texto sin formato, habilite la opción "Salida segura" en la actividad web y "Entrada segura" en la actividad de copia.


## <a name="export-json-response-as-is"></a>Exportación de la respuesta JSON tal cual

Puede usar este conector REST para exportar la respuesta de JSON de la API REST tal cual a varios almacenes basados en archivos. Para lograr esa copia independiente del esquema, omita la sección “structure” (estructura, también denominada *schema*) en el conjunto de datos y la asignación de esquemas en la actividad de copia.

## <a name="schema-mapping"></a>Asignación de esquemas

Para copiar datos desde el punto de conexión de REST en un receptor tabular, vea [asignación de esquemas](copy-activity-schema-and-type-mapping.md#schema-mapping).

## <a name="next-steps"></a>Pasos siguientes

Para ver una lista de los almacenes de datos que la actividad de copia admite como orígenes y receptores en Azure Data Factory, consulte los [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
