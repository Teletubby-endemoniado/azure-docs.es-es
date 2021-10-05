---
title: Copia de datos de SAP Business Warehouse a través de Open Hub
titleSuffix: Azure Data Factory & Azure Synapse
description: Aprenda a copiar datos de SAP Business Warehouse (BW) con Open Hub a almacenes de datos receptores compatibles mediante una actividad de copia en una canalización de Azure Data Factory o Synapse Analytics.
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 9e4bc92687c098709de5298d7ea22ed81b1e3783
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124836126"
---
# <a name="copy-data-from-sap-business-warehouse-via-open-hub-using-azure-data-factory-or-synapse-analytics"></a>Copia de datos de SAP Business Warehouse con Open Hub mediante Azure Data Factory o Synapse Analytics

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se explica el uso de la actividad de copia en canalizaciones de Azure Data Factory y Synapse Analytics para copiar datos de una instancia de SAP Business Warehouse (BW) con Open Hub. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

>[!TIP]
>Para información sobre la compatibilidad general con el escenario de integración de datos de SAP, consulte el [informe técnico sobre la integración de datos de SAP](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) que contiene una introducción detallada con comparaciones y una guía sobre cada conector de SAP.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de SAP Business Warehouse a través de Open Hub es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar los datos desde SAP Business Warehouse con Open Hub en cualquier almacén de datos de receptores que se admita. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

Concretamente, este conector de Open Hub para SAP Business Warehouse admite:

- SAP Business Warehouse **versión 7.01 o posteriores (en una pila de paquetes de soporte técnico de SAP reciente lanzada después de 2015)** . Este conector no admite SAP BW/4HANA.
- Copiar datos con la tabla local del destino Open Hub que, de forma subyacente, puede ser DSO, InfoCube, MultiProvider, DataSource, etc.
- Copiar datos con la autenticación básica.
- Conexión a un servidor de aplicaciones SAP o a un servidor de mensajes SAP.
- Recuperación de datos mediante RFC.

## <a name="sap-bw-open-hub-integration"></a>Integración de Open Hub con SAP BW 

El [servicio de Open Hub para SAP BW](https://wiki.scn.sap.com/wiki/display/BI/Overview+of+Open+Hub+Service) es una forma eficaz de extraer datos de SAP BW. En el siguiente diagrama se muestra uno de los flujos típicos que los clientes tienen en su sistema SAP, en cuyo caso, los datos fluyen desde SAP ECC -> PSA -> DSO -> Cube.

El destino Open Hub (OHD) para SAP BW define el destino al que se retransmitirán los datos de SAP. Todos los objetos compatibles con el proceso de transferencia de datos (DTP) de SAP pueden utilizarse como orígenes de datos de Open Hub; por ejemplo, DSO, InfoCube, DataSource, etc. Tipo de destino Open Hub: donde se almacenan los datos retransmitidos; puede tratarse de tablas de base de datos (locales o remotas) y de archivos planos. Este conector de Open Hube para SAP BW admite la copia de datos desde la tabla local de OHD a BW. Si usa otros tipos, puede conectarse directamente a la base de datos o al sistema de archivos con otros conectores.

:::image type="content" source="./media/connector-sap-business-warehouse-open-hub/sap-bw-open-hub.png" alt-text="Open Hub para SAP BW":::

## <a name="delta-extraction-flow"></a>Flujo de extracción diferencial

El conector Open Hub para SAP BW ofrece dos propiedades opcionales: `excludeLastRequest` y `baseRequestId`, que se pueden usar para administrar la carga diferencial de Open Hub. 

- **excludeLastRequestId**: Si desea excluir los registros de la última solicitud. El valor predeterminado es true. 
- **baseRequestId**: El identificador de la solicitud para la carga diferencial. Una vez establecido, solo se recuperarán los datos con el identificador de solicitud mayor que el valor de esta propiedad. 

En general, la extracción de InfoProviders de SAP consta de dos pasos: 

1. **Proceso de transferencia de datos (DTP) de SAP BW**: en este paso se copian los datos desde InfoProvider para SAP BW a una tabla de Open Hub para SAP BW. 

1. **Copia de datos**: en este paso, el conector lee la tabla de Open Hub. 

:::image type="content" source="media/connector-sap-business-warehouse-open-hub/delta-extraction-flow.png" alt-text="Flujo de extracción diferencial":::

En el primer paso, se ejecuta un DTP. Cada ejecución crea un nuevo identificador de solicitud SAP. El identificador de solicitud se almacena en la tabla de Open Hub y, luego, se usa el conector para identificar la diferencia. Los dos pasos se ejecutan de forma asincrónica: SAP desencadena el DTP y la copia de datos se desencadena mediante el servicio. 

De forma predeterminada, el servicio no lee los flujos diferenciales más recientes de la tabla de Open Hub (la opción de "exclusión de última solicitud" está establecida en true). De esta manera, los datos del servicio no están 100 % actualizados con los datos de la tabla de Open Hub (falta la última diferencia). A cambio, este procedimiento garantiza que no se pierden filas debido a la extracción asíncrona. Funciona correctamente incluso cuando el servicio lee la tabla de Open Hub mientras el DTP sigue escribiendo en la misma tabla. 

Por lo general, el identificador de solicitud copiado máximo se almacena en la última ejecución del servicio en un almacén de datos provisional (como Azure Blob en el diagrama anterior). Por lo tanto, el servicio no lee la misma solicitud por segunda vez en la ejecución posterior. Mientras tanto, tenga en cuenta que los datos no se eliminan automáticamente de la tabla de Open Hub.

Para una correcta administración de la diferencia, no está permitido disponer de identificadores de solicitud de diferentes DTP en la misma tabla de Open Hub. Por lo tanto, no debe crear más de un DTP para cada destino de Open Hub (OHD). Cuando necesite la extracción completa y diferencial del mismo InfoProvider, debe crear dos OHD para el mismo InfoProvider. 

## <a name="prerequisites"></a>Requisitos previos

Para usar este conector de Open Hub para SAP Business Warehouse, necesita:

- Configurar un entorno de ejecución de integración autohospedado con la versión 3.13 o posterior. Consulte el artículo sobre [Integration Runtime autohospedado](create-self-hosted-integration-runtime.md) para más información.

- Descargar el **[conector de SAP para .NET 3.0](https://support.sap.com/en/product/connectors/msnet.html) de 64 bits** del sitio web de SAP e instalarlo en la máquina del IR autohospedado. Al instalarlo, en la ventana de los pasos de configuración opcionales, asegúrese de seleccionar la opción **Install Assemblies to GAC** (Instalar ensamblados en GAC) como se muestra en la imagen siguiente. 

    :::image type="content" source="./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png" alt-text="Instalación del conector de SAP para .NET":::

- El usuario de SAP utilizado en el conector para BW necesita tener los siguientes permisos: 

    - Autorización para RFC y SAP BW. 
    - Permisos para la actividad “Ejecutar” del objeto de autorización “S_SDSAUTH”.

- Crear un tipo de destino Open Hub para SAP como **tabla de base de datos** con la opción "Technical Key" (Clave técnica) seleccionada.  También se recomienda dejar desmarcada la opción de eliminación de datos de la tabla, aunque no es necesario. Utilice DTP (con ejecución directa o integración en la cadena de procesos existente) para enviar los datos desde el objeto de origen (como un cubo) elegido hasta la tabla de destino Open Hub.

## <a name="getting-started"></a>Introducción

> [!TIP]
>
> Para ver un tutorial sobre el uso del conector de Open Hub para SAP BW, consulte [Carga de datos desde SAP Business Warehouse (BW)](load-sap-bw-data.md).

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que se usan para definir entidades específicas del conector de Open Hub para SAP Business Warehouse.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de Open Hub para SAP Business Warehouse:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **SapOpenHub** | Sí |
| server | Nombre del servidor en el que reside la instancia de SAP BW. | Sí |
| systemNumber | Número del sistema de SAP BW.<br/>Valor permitido: número decimal de dos dígitos que se representa en forma de cadena. | Sí |
| messageServer | El nombre de host del servidor de mensajes de SAP.<br/>Úselo para conectarse a un servidor de mensajes de SAP. | No |
| messageServerService | El nombre del servicio o el número de puerto del servidor de mensajes.<br/>Úselo para conectarse a un servidor de mensajes de SAP. | No |
| systemId | El id. del sistema SAP en el que se encuentra la tabla.<br/>Úselo para conectarse a un servidor de mensajes de SAP. | No |
| logonGroup | El grupo de inicio de sesión para el sistema SAP.<br/>Úselo para conectarse a un servidor de mensajes de SAP. | No |
| clientId | Identificador del cliente en el sistema SAP W.<br/>Valor permitido: número decimal de tres dígitos que se representa en forma de cadena. | Sí |
| language | Idioma que utiliza el sistema SAP. | No (el valor predeterminado es **EN**)|
| userName | Nombre del usuario que tiene acceso al servidor SAP. | Sí |
| password | Contraseña del usuario. Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Tal y como se mencionó en los [requisitos previos](#prerequisites), se requiere un entorno Integration Runtime autohospedado. |Sí |

**Ejemplo**:

```json
{
    "name": "SapBwOpenHubLinkedService",
    "properties": {
        "type": "SapOpenHub",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de Open Hub para SAP BW.

Para copiar datos desde y hacia Open Hub para SAP BW, establezca la propiedad type del conjunto de datos en **SapOpenHubTable**. Se admiten las siguientes propiedades.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en **SapOpenHubTable**.  | Sí |
| openHubDestinationName | Nombre del destino Open Hub desde el que copiar los datos. | Sí |

Si estaba configurando `excludeLastRequest` y `baseRequestId` en el conjunto de datos, todavía se admite tal cual, aunque se aconseja usar el nuevo modelo en el origen de la actividad en el futuro.

**Ejemplo**:

```json
{
    "name": "SAPBWOpenHubDataset",
    "properties": {
        "type": "SapOpenHubTable",
        "typeProperties": {
            "openHubDestinationName": "<open hub destination name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP BW Open Hub linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen Open Hub para SAP BW.

### <a name="sap-bw-open-hub-as-source"></a>Open Hub para SAP BW como origen

Para copiar datos desde Open Hub para SAP BW, en la sección **origen** de la actividad de copia se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad **type** del origen de la actividad de copia debe establecerse en **SapOpenHubSource**. | Sí |
| excludeLastRequest | Si desea excluir los registros de la última solicitud. | No (el valor predeterminado es **true**) |
| baseRequestId | El identificador de la solicitud para la carga diferencial. Una vez establecido, solo se recuperarán los datos con el identificador de solicitud **mayor que** el valor de esta propiedad.  | No |
| customRfcReadTableFunctionModule | Un módulo de función RFC personalizado que puede usarse para leer datos de la tabla de SAP. <br/> Puede usar el módulo de función RFC personalizado para definir cómo se recuperan los datos del sistema SAP y cómo se devuelven al servicio. El módulo de función personalizado debe tener una interfaz implementada (importación, exportación, tablas) que sea similar a `/SAPDS/RFC_READ_TABLE2`, que es la interfaz predeterminada que usa el servicio. | No |
| sapDataColumnDelimiter | El único carácter que se usa como delimitador que se pasa al RFC de SAP para dividir los datos de salida. | No |

>[!TIP]
>Si la tabla de Open Hub solo contiene los datos generados por un único identificador de solicitud y, por ejemplo, siempre hace una carga completa y sobrescribe los datos existentes en la tabla o solo ejecuta el DTP una vez por cada prueba, recuerde desactivar la opción "excludeLastRequest" para poder copiar los datos.

Para acelerar la carga de datos, puede establecer [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) en la actividad de copia para cargar datos desde Open Hub para SAP BW en paralelo. Por ejemplo, si establece `parallelCopies` en cuatro, el servicio ejecutará simultáneamente cuatro llamadas de RFC, y cada una recuperará una parte de los datos de la tabla de Open Hub para SAP BW, particionada por el identificador de solicitud de DTP y por el identificador de paquete. Esto se aplica cuando la suma de identificadores únicos de solicitud de DTP y de paquete es superior al valor de `parallelCopies`. Cuando se copian datos en un almacén de datos basado en archivos, también se recomienda escribir en una carpeta como varios archivos (solo especifique el nombre de la carpeta), en cuyo caso el rendimiento es mejor que escribir en un único archivo.

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromSAPBWOpenHub",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW Open Hub input dataset name>",
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
                "type": "SapOpenHubSource",
                "excludeLastRequest": true
            },
            "sink": {
                "type": "<sink type>"
            },
            "parallelCopies": 4
        }
    }
]
```

## <a name="data-type-mapping-for-sap-bw-open-hub"></a>Asignación de tipos de datos en Open Hub para SAP BW

Al copiar datos de Open Hub para SAP BW, se utilizan las siguientes asignaciones de tipos de datos de SAP BW en los tipos de datos provisionales usados internamente dentro del servicio. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Tipo ABAP para SAP | Tipo de datos de servicio provisional |
|:--- |:--- |
| C (cadena) | String |
| I (entero) | Int32 |
| F (flotante) | Double |
| D (fecha) | String |
| T (hora) | String |
| P (BCD empaquetado, moneda, decimal, cantidad) | Decimal |
| N (número) | String |
| X (binario y sin procesar) | String |

## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).

## <a name="troubleshooting-tips"></a>Sugerencias de solución de problemas

**Síntomas:** si ejecuta SAP BW en HANA y observa que solo se copia un subconjunto de datos mediante la actividad de copia (1 millón filas), la causa posible es que habilite la opción "SAP HANA Execution" (Ejecución de SAP HANA) en su DTP, en cuyo caso el servicio solo puede recuperar el primer lote de datos.

**Resolución:** Deshabilite la opción "Ejecución de SAP HANA" en DTP, vuelva a procesar los datos e intente ejecutar la actividad de copia de nuevo.

## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
