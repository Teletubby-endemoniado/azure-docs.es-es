---
title: Copia de datos desde SAP BW
titleSuffix: Azure Data Factory & Azure Synapse
description: Obtenga información sobre cómo copiar datos desde SAP Business Warehouse en almacenes de datos receptores compatibles, a través de una actividad de copia en una canalización de Azure Data Factory o Synapse Analytics.
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 2cbdd3dc237c3f2e7b3cf23bb844a06fdd40d605
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132325854"
---
# <a name="copy-data-from-sap-business-warehouse-using-azure-data-factory-or-synapse-analytics"></a>Copia de datos desde SAP Business Warehouse con Azure Data Factory o Synapse Analytics
> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
> * [Versión 1](v1/data-factory-sap-business-warehouse-connector.md)
> * [Versión actual](connector-sap-business-warehouse.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se explica cómo usar la actividad de copia en canalizaciones de Azure Data Factory y Synapse Analytics para copiar datos desde una instancia de SAP Business Warehouse (BW). El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.

>[!TIP]
>Para obtener información sobre la compatibilidad general del servicio con el escenario de integración de datos de SAP, consulte el [informe técnico sobre la integración de datos de SAP mediante Azure Data Factory](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf), que contiene una introducción detallada con comparaciones y una guía sobre cada conector de SAP.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Este conector de SAP Business Warehouse es compatible con las actividades siguientes:

- [Actividad de copia](copy-activity-overview.md) con [matriz de origen o receptor compatible](copy-activity-overview.md)
- [Actividad de búsqueda](control-flow-lookup-activity.md)

Puede copiar los datos desde SAP Business Warehouse hasta cualquier almacén de datos de receptores que se admita. Consulte la tabla de [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats) para ver una lista de almacenes de datos que la actividad de copia admite como orígenes o receptores.

Concretamente, este conector SAP Business Warehouse admite:

- SAP Business Warehouse **versión 7.x**.
- La copia de datos de **InfoCubes y QueryCubes** (incluidas las consultas BEx) mediante consultas MDX.
- Copiar datos con la autenticación básica.

>[!NOTE]
>El conector SAP Business Warehouse actualmente no admite parámetros con MDX.  Si es necesario filtrar con parámetros MDX, puede considerar la posibilidad de usar el [conector SAP Open Hub](connector-sap-business-warehouse-open-hub.md) alternativo en su lugar.

## <a name="prerequisites"></a>Prerrequisitos

Para usar este conector SAP Business Warehouse, necesita hacer lo siguiente:

- Configurar un entorno Integration Runtime autohospedado. Consulte el artículo sobre [Integration Runtime autohospedado](create-self-hosted-integration-runtime.md) para más información.
- Instalar la **biblioteca SAP NetWeaver** en la máquina de Integration Runtime. Puede obtener la biblioteca SAP Netweaver desde el administrador de SAP o directamente desde el [centro de descarga de software de SAP](https://support.sap.com/swdc). Busque la **nota 1025361 de SAP** para obtener la ubicación de descarga de la versión más reciente. Asegúrese de elegir la biblioteca de SAP NetWeaver de **64 bits**, que coincida con la instalación de Integration Runtime. A continuación, instale todos los archivos incluidos en SAP NetWeaver RFC SDK según la nota de SAP. La biblioteca SAP NetWeaver también se incluye en la instalación de las herramientas de cliente de SAP.

>[!TIP]
>Para solucionar problemas de conectividad a SAP BW, asegúrese de lo siguiente:
>- Todas las bibliotecas de dependencias extraídas de NetWeaver RFC SDK están en la carpeta %windir%\system32. Normalmente contiene icudt34.dll, icuin34.dll, icuuc34.dll, libicudecnumber.dll, librfc32.dll, libsapucum.dll, sapcrypto.dll, sapcryto_old.dll, sapnwrfc.dll.
>- Los puertos necesarios usados para conectarse a SAP Server están habilitados en la máquina de IR autohospedado, que normalmente son 3300 y 3201.

## <a name="getting-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-sap-bw-using-ui"></a>Creación de un servicio vinculado a SAP BW mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado a SAP BW en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Data Factory":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Creación de un servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque SAP y seleccione SAP BW a través del conector MDX.

    :::image type="content" source="media/connector-sap-business-warehouse/sap-business-warehouse-connector.png" alt-text="Selección de SAP BW a través del conector MDX.":::    

1. Configure los detalles del servicio, pruebe la conexión y cree el servicio vinculado.

    :::image type="content" source="media/connector-sap-business-warehouse/configure-sap-business-warehouse-linked-service.png" alt-text="Configuración de un servicio vinculado a SAP BW.":::

## <a name="connector-configuration-details"></a>Detalles de configuración del conector

Las secciones siguientes proporcionan detalles sobre las propiedades que se usan para definir entidades de Data Factory específicas del conector SAP Business Warehouse.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado SAP Business Warehouse (BW):

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en: **SapBw** | Sí |
| server | Nombre del servidor en el que reside la instancia de SAP BW. | Sí |
| systemNumber | Número del sistema de SAP BW.<br/>Valor permitido: número decimal de dos dígitos que se representa en forma de cadena. | Sí |
| clientId | Identificador del cliente en el sistema SAP W.<br/>Valor permitido: número decimal de tres dígitos que se representa en forma de cadena. | Sí |
| userName | Nombre del usuario que tiene acceso al servidor SAP. | Sí |
| password | Contraseña del usuario. Marque este campo como SecureString para almacenarlo de forma segura, o bien [haga referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sí |
| connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Tal y como se mencionó en los [requisitos previos](#prerequisites), se requiere un entorno Integration Runtime autohospedado. |Sí |

**Ejemplo**:

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
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

Si desea ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte el artículo sobre [conjuntos de datos](concepts-datasets-linked-services.md). En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de SAP BW.

Para copiar datos desde SAP BW, establezca la propiedad type del conjunto de datos en **SapBwCube**. No hay ninguna propiedad específica del tipo compatible con el conjunto de datos de SAP BW de tipo RelationalTable.

**Ejemplo**:

```json
{
    "name": "SAPBWDataset",
    "properties": {
        "type": "SapBwCube",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP BW linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

Si estaba usando un conjunto de datos de tipo `RelationalTable`, todavía se admite tal cual, aunque se aconseja usar el nuevo en el futuro.

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

Si desea ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo sobre [canalizaciones](concepts-pipelines-activities.md). En esta sección se proporciona una lista de las propiedades que admite el origen de SAP BW.

### <a name="sap-bw-as-source"></a>SAP BW como origen

Para copiar datos desde SAP BW, en la sección **source** de la actividad de copia se admiten las siguientes propiedades:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type del origen de la actividad de copia debe establecerse en: **SapBwSource** | Sí |
| Query | Especifica la consulta MDX para leer datos de la instancia de SAP BW. | Sí |

**Ejemplo**:

```json
"activities":[
    {
        "name": "CopyFromSAPBW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW input dataset name>",
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
                "type": "SapBwSource",
                "query": "<MDX query for SAP BW>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Si estaba usando un origen de tipo `RelationalSource`, todavía se admite tal cual, aunque se aconseja usar el nuevo en el futuro.

## <a name="data-type-mapping-for-sap-bw"></a>Asignación de tipos de datos para SAP BW

Al copiar datos desde SAP BW, se utilizan las siguientes asignaciones de tipos de datos de la solución a los tipos de datos provisionales usados internamente dentro del servicio. Consulte el artículo sobre [asignaciones de tipos de datos y esquema](copy-activity-schema-and-type-mapping.md) para información sobre cómo la actividad de copia asigna el tipo de datos y el esquema de origen al receptor.

| Tipo de datos de SAP BW | Tipo de datos de servicio provisional |
|:--- |:--- |
| ACCP | Int |
| CHAR | String |
| CLNT | String |
| CURR | Decimal |
| CUKY | String |
| DEC | Decimal |
| FLTP | Double |
| INT1 | Byte |
| INT2 | Int16 |
| INT4 | Int |
| LANG | String |
| LCHR | String |
| LRAW | Byte[] |
| PREC | Int16 |
| QUAN | Decimal |
| RAW | Byte[] |
| RAWSTRING | Byte[] |
| STRING | String |
| UNIDAD | String |
| DATS | String |
| NUMC | String |
| TIMS | String |


## <a name="lookup-activity-properties"></a>Propiedades de la actividad de búsqueda

Para obtener información detallada sobre las propiedades, consulte [Actividad de búsqueda](control-flow-lookup-activity.md).


## <a name="next-steps"></a>Pasos siguientes
Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, vea [Almacenes de datos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
