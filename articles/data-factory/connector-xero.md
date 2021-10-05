---
title: Copia de datos de Xero
description: Aprenda a copiar datos de Xero en almacenes de datos receptores compatibles mediante una actividad de copia de una canalización de Azure Data Factory o Synapse Analytics.
titleSuffix: Azure Data Factory & Azure Synapse
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.author: jianleishen
ms.openlocfilehash: 17b92068145ac07833f7e73cc4d694a89f39ad7f
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124779851"
---
# <a name="copy-data-from-xero-using-azure-data-factory-or-synapse-analytics"></a>Copia de datos de Xero con Azure Data Factory o Synapse Analytics

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se explica cómo usar la actividad de copia de una canalización de Azure Data Factory o Synapse Analytics para copiar datos de Xero. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector Xero es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar datos de Xero en cualquier almacén de datos receptor admitido. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

En concreto, este conector Xero admite lo siguiente:

- Autenticación de OAuth 2.0 y OAuth 1.0. En OAuth 1.0, el conector admite la [aplicación privada](https://developer.xero.com/documentation/getting-started/getting-started-guide) Xero pero no una aplicación pública.
- Todas las tablas de Xero (puntos de conexión de API), excepto "Reports" (Informes).

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-xero-using-ui"></a>Creación de un servicio vinculado en Xero mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado en Xero en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Data Factory":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque Xero y seleccione el conector de Xero.

   :::image type="content" source="media/connector-xero/xero-connector.png" alt-text="Seleccione el conector de Xero.":::    


1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

   :::image type="content" source="media/connector-xero/configure-xero-linked-service.png" alt-text="Configuración de un servicio vinculado en Xero.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector Xero.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Xero:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **Xero** | Sí |
| connectionProperties | Grupo de propiedades que define cómo conectarse a Xero. | Sí |
| ***En`connectionProperties`:*** | | |
| host | El punto de conexión del servidor de Xero (`api.xero.com`).  | Sí |
| authenticationType | Los valores permitidos son `OAuth_2.0` y `OAuth_1.0`. | Sí |
| consumerKey | Para OAuth 2.0, especifique el **id. de cliente** para la aplicación Xero.<br>Para OAuth 1.0, especifique la clave de consumidor asociada a la aplicación Xero.<br>Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| privateKey | Para OAuth 2.0, especifique el **secreto de cliente*** para la aplicación Xero.<br>Para OAuth 1.0, especifique la clave privada del archivo .pem que se generó para la aplicación privada Xero; consulte [Creación de un par de claves pública y privada](https://developer.xero.com/documentation/auth-and-limits/create-publicprivate-key). Asegúrese de **generar el archivo privatekey.pem con numbits de 512** con `openssl genrsa -out privatekey.pem 512`; 1024 no se admite. Incluya todo el texto del archivo .pem, así como los finales de línea Unix (\n). Vea el ejemplo a continuación.<br/><br>Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| tenantId | Identificador de inquilino asociado a la aplicación Xero. Aplicable para la autenticación de OAuth 2.0.<br>Aprenda a obtener el identificador de inquilino en la sección [Comprobación de los inquilinos a los que tiene autorizado el acceso](https://developer.xero.com/documentation/oauth2/auth-flow). | Sí para la autenticación de OAuth 2.0 |
| refreshToken | Aplicable para la autenticación de OAuth 2.0.<br/>El token de actualización de OAuth 2.0 está asociado a la aplicación Xero y se usa para actualizar el token de acceso, que expira después de 30 minutos. Obtenga información sobre cómo funciona el flujo de autorización de Xero y cómo obtener el token de actualización en [este artículo](https://developer.xero.com/documentation/oauth2/auth-flow). Para obtener un token de actualización, debe solicitar el [ámbito offline_access](https://developer.xero.com/documentation/oauth2/scopes). <br/>**Limitación conocida**: tenga en cuenta que Xero restablece el token de actualización después de que se use para la actualización del token de acceso. En el caso de cargas de trabajo operativas, antes de que se ejecute la actividad de copia, debe establecer un token de actualización válido para que lo use el servicio.<br/>Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí para la autenticación de OAuth 2.0 |
| useEncryptedEndpoints | Especifica si los puntos de conexión de origen de datos se cifran mediante HTTPS. El valor predeterminado es true.  | No |
| useHostVerification | Especifica si se requiere que el nombre de host del certificado del servidor coincida con el nombre de host del servidor al conectarse a través de TLS. El valor predeterminado es true.  | No |
| usePeerVerification | Especifica si se debe verificar la identidad del servidor al conectarse a través de TLS. El valor predeterminado es true.  | No |

**Ejemplo: autenticación de OAuth 2.0**

```json
{
    "name": "XeroLinkedService",
    "properties": {
        "type": "Xero",
        "typeProperties": {
            "connectionProperties": { 
                "host": "api.xero.com",
                "authenticationType":"OAuth_2.0", 
                "consumerKey": {
                    "type": "SecureString",
                    "value": "<client ID>"
                },
                "privateKey": {
                    "type": "SecureString",
                    "value": "<client secret>"
                },
                "tenantId": "<tenant ID>", 
                "refreshToken": {
                    "type": "SecureString",
                    "value": "<refresh token>"
                }, 
                "useEncryptedEndpoints": true, 
                "useHostVerification": true, 
                "usePeerVerification": true
            }            
        }
    }
}
```

**Ejemplo: autenticación de OAuth 1.0**

```json
{
    "name": "XeroLinkedService",
    "properties": {
        "type": "Xero",
        "typeProperties": {
            "connectionProperties": {
                "host": "api.xero.com", 
                "authenticationType":"OAuth_1.0", 
                "consumerKey": {
                    "type": "SecureString",
                    "value": "<consumer key>"
                },
                "privateKey": {
                    "type": "SecureString",
                    "value": "<private key>"
                }, 
                "useEncryptedEndpoints": true,
                "useHostVerification": true,
                "usePeerVerification": true
            }
        }
    }
}
```

**Valor de clave privada de ejemplo:**

Incluya todo el texto del archivo .pem, así como los finales de línea Unix (\n).

```
"-----BEGIN RSA PRIVATE KEY-----\nMII***************************************************P\nbu****************************************************s\nU/****************************************************B\nA*****************************************************W\njH****************************************************e\nsx*****************************************************l\nq******************************************************X\nh*****************************************************i\nd*****************************************************s\nA*****************************************************dsfb\nN*****************************************************M\np*****************************************************Ly\nK*****************************************************Y=\n-----END RSA PRIVATE KEY-----"
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Xero.

Para copiar datos de Xero, establezca la propiedad type del conjunto de datos en **XeroObject**. Se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del conjunto de datos debe establecerse en: **XeroObject** | Sí |
| tableName | Nombre de la tabla. | No (si se especifica "query" en el origen de la actividad) |

**Ejemplo**

```json
{
    "name": "XeroDataset",
    "properties": {
        "type": "XeroObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Xero linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen de Xero.

### <a name="xero-as-source"></a>Xero como origen

Para copiar datos de Xero, establezca el tipo de origen de la actividad de copia en **XeroSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **XeroSource** | Sí |
| Query | Use la consulta SQL personalizada para leer los datos. Por ejemplo: `"SELECT * FROM Contacts"`. | No (si se especifica "tableName" en el conjunto de datos) |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromXero",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Xero input dataset name>",
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
                "type": "XeroSource",
                "query": "SELECT * FROM Contacts"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Tenga en cuenta lo siguiente al especificar la consulta de Xero:

- Las tablas con elementos complejos se dividirán en varias tablas. Por ejemplo, las transacciones bancarias tienen una estructura de datos compleja "LineItems", así que los datos de la transacción bancaria se asignan a las tablas `Bank_Transaction` y `Bank_Transaction_Line_Items`, con la clave externa `Bank_Transaction_ID` para vincularlas.

- Los datos de Xero están disponibles a través de dos esquemas: `Minimal` (predeterminado) y `Complete`. El esquema Complete contiene tablas de llamadas de requisitos previos que necesitan datos adicionales (por ejemplo, la columna ID) antes de realizar la consulta deseada.

Las siguientes tablas tienen la misma información en los esquemas Minimal y Complete. Para reducir el número de llamadas API, use un esquema Minimal (predeterminado).

- Bank_Transactions
- Contact_Groups 
- Contactos 
- Contacts_Sales_Tracking_Categories 
- Contacts_Phones 
- Contacts_Addresses 
- Contacts_Purchases_Tracking_Categories 
- Credit_Notes 
- Credit_Notes_Allocations 
- Expense_Claims 
- Expense_Claim_Validation_Errors
- Facturas 
- Invoices_Credit_Notes
- Invoices_ Prepayments 
- Invoices_Overpayments 
- Manual_Journals 
- Overpayments 
- Overpayments_Allocations 
- Prepayments 
- Prepayments_Allocations 
- Receipts 
- Receipt_Validation_Errors 
- Tracking_Categories

Las siguientes tablas solo se pueden consultar con el esquema Complete:

- Complete.Bank_Transaction_Line_Items 
- Complete.Bank_Transaction_Line_Item_Tracking 
- Complete.Contact_Group_Contacts 
- Complete.Contacts_Contact_ Persons 
- Complete.Credit_Note_Line_Items 
- Complete.Credit_Notes_Line_Items_Tracking 
- Complete.Expense_Claim_ Payments 
- Complete.Expense_Claim_Receipts 
- Complete.Invoice_Line_Items 
- Complete.Invoices_Line_Items_Tracking
- Complete.Manual_Journal_Lines 
- Complete.Manual_Journal_Line_Tracking 
- Complete.Overpayment_Line_Items 
- Complete.Overpayment_Line_Items_Tracking 
- Complete.Prepayment_Line_Items 
- Complete.Prepayment_Line_Item_Tracking 
- Complete.Receipt_Line_Items 
- Complete.Receipt_Line_Item_Tracking 
- Complete.Tracking_Category_Options

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).


## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de los almacenes de datos que admite la actividad de copia, consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats).
