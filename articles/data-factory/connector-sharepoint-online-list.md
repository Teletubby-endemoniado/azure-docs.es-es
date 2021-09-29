---
title: Copia de datos de SharePoint Online List
titleSuffix: Azure Data Factory & Azure Synapse
description: Aprenda a copiar datos de una lista de SharePoint Online en almacenes de datos receptores compatibles mediante una actividad de copia efectuada en una canalización de Azure Data Factory o Azure Synapse Analytics.
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.author: jianleishen
ms.openlocfilehash: ad9adb2c9f6b8308aba79f9e9bd1eadbf755760a
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124763944"
---
# <a name="copy-data-from-sharepoint-online-list-by-using-azure-data-factory-or-azure-synapse-analytics"></a>Copia de datos de una lista de SharePoint Online mediante Azure Data Factory o Azure Synapse Analytics
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se resume el uso de la actividad de copia en canalizaciones de Azure Data Factory y Azure Synapse para copiar datos desde SharePoint Online List. El artículo se basa en [Actividad de copia](copy-activity-overview.md), en el que se ofrece información general acerca de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de lista de SharePoint Online es compatible con las siguientes actividades:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos de una lista de SharePoint Online a cualquier almacén de datos receptor compatible. Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).

En concreto, este conector de lista de SharePoint Online usa la autenticación de entidad de servicio y recupera los datos mediante el protocolo OData.

> [!TIP]
> Este conector permite copiar datos de una **lista** de SharePoint Online, pero no de un archivo. Aprenda a copiar un archivo en la sección [Copia de un archivo de SharePoint Online](#copy-file-from-sharepoint-online).

## <a name="prerequisites"></a>Prerrequisitos

El conector de lista de SharePoint Online usa la autenticación de entidad de servicio para conectarse a SharePoint. Siga estos pasos para configurarlo:

1. Registre una entidad de aplicación en Azure Active Directory (Azure AD) como se indica en [Registro de la aplicación con un inquilino de Azure AD](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Anote los siguientes valores; los usará para definir el servicio vinculado:

    - Identificador de aplicación
    - Clave de la aplicación
    - Id. de inquilino

2. Conceda permiso para el sitio de SharePoint Online a la aplicación registrada: 

    > [!NOTE]
    > Esta operación requiere el permiso de propietario del sitio de SharePoint Online. Para localizar el propietario, vaya a la página principal del sitio -> haga clic en "X Members" (Miembros de X) en la esquina derecha -> compruebe quién tiene el rol "Owner" (Propietario).

    1. Abra el vínculo del sitio de SharePoint Online, por ejemplo `https://[your_site_url]/_layouts/15/appinv.aspx` (reemplace la dirección URL del sitio).
    2. Busque el identificador de la aplicación que registró, rellene los campos vacíos y haga clic en "Create" (Crear).

        - App Domain (Dominio de aplicación): `localhost.com`
        - Redirect URL (Dirección URL de redireccionamiento): `https://www.localhost.com`
        - Permission Request XML (XML de solicitud de permiso):

        ```xml
        <AppPermissionRequests AllowAppOnlyPolicy="true">
            <AppPermissionRequest Scope="http://sharepoint/content/sitecollection/web" Right="Read"/>
        </AppPermissionRequests>
        ```

        :::image type="content" source="media/connector-sharepoint-online-list/sharepoint-online-grant-permission.png" alt-text="conceder permiso en SharePoint":::

    3. Haga clic en "Trust It" (Confiar) para esta aplicación.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-a-sharepoint-online-list-using-ui"></a>Creación de un servicio vinculado a SharePoint Online List mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a SharePoint Online List en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Data Factory.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque SharePoint y seleccione el conector de SharePoint Online List.

    :::image type="content" source="media/connector-sharepoint-online-list/sharepoint-online-list-connector.png" alt-text="Captura de pantalla del conector de SharePoint Online List.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

    :::image type="content" source="media/connector-sharepoint-online-list/configure-sharepoint-online-list-linked-service.png" alt-text="Captura de pantalla de la configuración del servicio vinculado para SharePoint Online List.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que puede usar para definir entidades específicas del conector de lista de SharePoint Online.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con un servicio vinculado de lista de SharePoint Online:

| **Propiedad**        | **Descripción**                                              | **Obligatorio** |
| ------------------- | ------------------------------------------------------------ | ------------ |
| type                | La propiedad type debe establecerse en  **SharePointOnlineList**.  | Sí          |
| siteUrl             | Dirección URL del sitio de SharePoint Online: por ejemplo, `https://contoso.sharepoint.com/sites/siteName`. | Sí          |
| servicePrincipalId  | Identificador de la aplicación (cliente) registrada en Azure Active Directory. | Sí          |
| servicePrincipalKey | La clave de la aplicación. Marque este campo como **SecureString** para almacenarlo de forma segura, o bien haga [referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí          |
| tenantId            | Identificador del inquilino donde reside la aplicación.          | Sí          |
| connectVia          | Instancia de [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Obtenga más información en [Prerrequisitos](#prerequisites), anteriormente en este artículo. Si no se especifica, se usa el valor predeterminado de Azure Integration Runtime. | No           |

**Ejemplo**:

```json
{
    "name": "SharePointOnlineList",
    "properties": {
        "type": "SharePointOnlineList",
        "typeProperties": {
            "siteUrl": "<site URL>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenantId": "<tenant ID>"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Para ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos y servicios vinculados](concepts-datasets-linked-services.md). En la sección siguiente se proporciona una lista de las propiedades que admite el conjunto de datos de la tabla de SAP.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del conjunto de datos debe establecerse en **SharePointOnlineLResource**. | Sí |
| listName | Nombre de la lista de SharePoint Online. | Sí |

**Ejemplo**

```json
{
    "name": "SharePointOnlineListDataset",
    "properties":
    {
        "type": "SharePointOnlineListResource",
        "linkedServiceName": {
            "referenceName": "<SharePoint Online List linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties":
        {
            "listName": "<name of the list>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Para ver una lista completa de las secciones y propiedades que hay disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md).  En la sección siguiente se incluye una lista de las propiedades que admite el origen de lista de SharePoint Online.

### <a name="sharepoint-online-list-as-source"></a>Lista de SharePoint Online como origen

Para copiar datos de la lista de SharePoint Online, se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del origen de la actividad de copia debe establecerse en **SharePointOnlineListSource**. | Sí |
| Query | Opciones de consulta de OData personalizadas para filtrar datos. Ejemplo: `"$top=10&$select=Title,Number"`. | No |
| httpRequestTimeout | El tiempo (en segundos) que la solicitud HTTP espera para obtener una respuesta. El valor predeterminado es 300 (5 minutos). | No |

**Ejemplo**

```json
"activities":[
    {
        "name": "CopyFromSharePointOnlineList",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SharePoint Online List input dataset name>",
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
            "source": {
                "type": "SharePointOnlineListSource",
                "query": "<OData query e.g. $top=10&$select=Title,Number>"
            }, 
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

> [!NOTE]
> No es posible seleccionar más de una *opción* de tipo de datos para un origen de la lista de SharePoint Online.

## <a name="data-type-mapping-for-sharepoint-online-list"></a>Asignación de tipos de datos para la lista de SharePoint Online

Cuando se copian datos de una lista de SharePoint Online, se usan las siguientes asignaciones entre los tipos de datos de la lista de SharePoint Online y los tipos de datos provisionales usados internamente por el servicio.

| **Tipo de datos de SharePoint Online**                 | **Tipo de datos de OData**                                  | **Tipo de datos provisional** |
| ----------------------------------------------- | ---------------------------------------------------- | ---------------------------------------- |
| Línea de texto única                             | Edm.String                                           | String                                   |
| Varias líneas de texto                          | Edm.String                                           | String                                   |
| Opción (menú para elegir)                    | Edm.String                                           | String                                   |
| Número (1, 1,0, 100)                            | Edm.Double                                           | Double                                   |
| Moneda ($, ¥, &euro;)                              | Edm.Double                                           | Double                                   |
| Fecha y hora                                   | Edm.DateTime                                         | DateTime                                 |
| Búsqueda (información que ya hay en el sitio)       | Edm.Int32                                            | Int32                                    |
| Sí/no (casilla)                              | Edm.Boolean                                          | Boolean                                  |
| Persona o grupo                                 | Edm.Int32                                            | Int32                                    |
| Hipervínculo o imagen                            | Edm.String                                           | String                                   |
| Calculado (cálculo basado en otras columnas) | Edm.String / Edm.Double / Edm.DateTime / Edm.Boolean | String/Double/DateTime/Boolean     |
| Datos adjuntos                                      | No compatible                                        |                                          |
| Resultado de tarea                                    | No compatible                                        |                                          |
| Datos externos                                   | No compatible                                        |                                          |
| Metadatos administrados                                | No compatible                                        |                                          |

## <a name="copy-file-from-sharepoint-online"></a>Copia de un archivo de SharePoint Online

Puede copiar un archivo de SharePoint Online si utiliza una **actividad web** para autenticar y obtener el token de acceso de SPO y, a continuación, pasa a una **actividad de copia** posterior para copiar los datos con el **conector HTTP como origen**.

:::image type="content" source="media/connector-sharepoint-online-list/sharepoint-online-copy-file-flow.png" alt-text="flujo de copia de archivo de SharePoint":::

1. Siga las indicaciones de la sección [Prerrequisitos](#prerequisites) para crear una aplicación de AAD y conceder permiso a SharePoint Online. 

2. Cree una **actividad web** para obtener el token de acceso de SharePoint Online:

    - **URL**: `https://accounts.accesscontrol.windows.net/[Tenant-ID]/tokens/OAuth/2`. Reemplace el identificador de inquilino.
    - **Método**: POST
    - **Encabezados**:
        - Content-Type: application/x-www-form-urlencoded
    - **Body** (Cuerpo):  `grant_type=client_credentials&client_id=[Client-ID]@[Tenant-ID]&client_secret=[Client-Secret]&resource=00000003-0000-0ff1-ce00-000000000000/[Tenant-Name].sharepoint.com@[Tenant-ID]`. Reemplace el identificador de cliente (identificador de aplicación), el secreto de cliente (clave de aplicación), el identificador de inquilino y el nombre de inquilino (del inquilino de SharePoint).

    > [!CAUTION]
    > Establezca la opción Secure Output (Salida segura) en true en la actividad web para evitar que el valor de token se registre como texto sin formato. Todas las actividades adicionales que consuman este valor deben tener la opción Entrada segura establecida en true.

3. Encadene con una **actividad de copia** utilizando el conector HTTP como origen para copiar el contenido del archivo de SharePoint Online:

    - Servicio vinculado HTTP:
        - **Base URL** (URL base): `https://[site-url]/_api/web/GetFileByServerRelativeUrl('[relative-path-to-file]')/$value`. Reemplace la dirección URL del sitio y la ruta de acceso relativa al archivo. Por ejemplo, use una ruta de acceso relativa al archivo como `/sites/site2/Shared Documents/TestBook.xlsx`.
        - **Authentication type** (Tipo de autenticación): Anonymous (Anónimos) *(para usar el token de portador configurado en el origen de la actividad de copia más adelante)*
    - Conjunto de datos: elija el formato que desee. Para copiar el archivo tal cual, seleccione el tipo binario.
    - Origen de actividad de copia:
        - **Request method** (Método de solicitud): GET
        - **Additional header** (Encabezado adicional): use la siguiente expresión `@{concat('Authorization: Bearer ', activity('<Web-activity-name>').output.access_token)}`, que usa el token de portador generado por la actividad web ascendente como encabezado de autorización. Reemplace el nombre de la actividad web.
    - Configure el receptor de la actividad de copia como de costumbre.

> [!NOTE]
> Incluso si una aplicación de Azure AD tiene permisos `FullControl` en SharePoint Online, no puede copiar archivos de las bibliotecas de documentos con IRM habilitado.

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
