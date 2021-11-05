---
title: Servicios vinculados
titleSuffix: Azure Data Factory & Azure Synapse
description: Obtenga información sobre los servicios vinculados en Azure Data Factory y Azure Synapse Analytics. Los servicios vinculados vinculan los almacenes de datos y proceso al servicio.
author: dcstwh
ms.author: weetok
ms.reviewer: jburchel
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.openlocfilehash: ce62577dd73d6c6318c5358e4cff3ada4f4bc0a5
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131068874"
---
# <a name="linked-services-in-azure-data-factory-and-azure-synapse-analytics"></a>Servicios vinculados en Azure Data Factory y Azure Synapse Analytics

> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que esté usando:"]
> * [Versión 1](v1/data-factory-create-datasets.md)
> * [Versión actual](concepts-linked-services.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe qué son los servicios vinculados, cómo se definen en formato JSON y cómo se usan en Azure Data Factory y Azure Synapse Analytics.

Para obtener más información, lea el artículo de introducción para [Azure Data Factory](introduction.md) o [Azure Synapse](../synapse-analytics/overview-what-is.md).

## <a name="overview"></a>Información general

Azure Data Factory y Azure Synapse Analytics pueden tener una o varias canalizaciones. Una **canalización** es una agrupación lógica de **actividades** que realizan una tarea. Las actividades de una canalización definen las acciones que se van a realizar en los datos. Por ejemplo, podría usar una actividad de copia para copiar datos de SQL Server en una instancia de Azure Blob Storage. Después, podría usar una actividad de Hive que ejecute un script de Hive en un clúster de Azure HDInsight para procesar datos de Blob Storage con el fin de generar datos de salida. Finalmente, podría usar segunda actividad de copia para copiar los datos de salida en Azure Synapse Analytics, en función de qué soluciones de generación de informes de inteligencia empresarial (BI) estén integradas. Para más información sobre canalizaciones y actividades, consulte el artículo [Canalizaciones y actividades](concepts-pipelines-activities.md).

Ahora, un **conjunto de datos** es una vista con nombre de los datos que simplemente apunta o hace referencia a los datos que desea usar en sus **actividades** como entradas y salidas.

Antes de crear un conjunto de datos, debe crear un **servicio vinculado** para vincular su almacén de datos a Data Factory o al área de trabajo de Synapse. Los servicios vinculados son muy similares a las cadenas de conexión, que definen la información de conexión necesaria para que el servicio se conecte a recursos externos. Considérelos de esta forma: el conjunto de datos representa la estructura de los datos dentro de los almacenes de datos vinculados y el servicio vinculado define la conexión al origen de datos. Por ejemplo, un servicio vinculado Azure Storage vincula una cuenta de almacenamiento al servicio. Un conjunto de datos de blobs de Azure representa el contenedor de blobs y la carpeta dentro de esa cuenta de Azure Storage que contiene los blobs de entrada que se van a procesar.

Este es un escenario de ejemplo. Para copiar datos de Blob Storage en SQL Database, se crean dos servicios vinculados: Microsoft Azure Storage y Azure SQL Database. Después, creará dos conjuntos de datos: el conjunto de datos de un blob de Azure (que hace referencia al servicio vinculado Azure Storage) y el conjunto de datos de Azure SQL Table (que hace referencia al servicio vinculado Azure SQL Database). Los servicios vinculados de Azure Storage y Azure SQL Database contienen cadenas de conexión que el servicio usa en tiempo de ejecución para conectarse a Azure Storage y Azure SQL Database, respectivamente. El conjunto de datos Azure Blob especifica el contenedor de blobs y la carpeta de blobs que contiene los blobs de entrada de Blob Storage. El conjunto de datos Azure SQL Table especifica la tabla de SQL de SQL Database en la que se van a copiar los datos.

En el siguiente diagrama se muestra la relación entre la canalización, la actividad, el conjunto de datos y el servicio vinculado en el servicio:

:::image type="content" source="media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png" alt-text="Relación entre la canalización, la actividad, el conjunto de datos y los servicios vinculados":::

## <a name="linked-service-json"></a>Servicio vinculado JSON

Un servicio vinculado se define con formato JSON de la manera siguiente:

```json
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

La tabla siguiente describe las propiedades del JSON anterior:

Propiedad | Descripción | Obligatorio |
-------- | ----------- | -------- |
name | Nombre del servicio vinculado. Vea [Reglas de nomenclatura](naming-rules.md). |  Sí |
type | Tipo de servicio vinculado. Por ejemplo: AzureBlobStorage (almacén de datos) o AzureBatch (proceso). Vea la descripción de typeProperties. | Sí |
typeProperties | Las propiedades de tipo son diferentes para cada almacén de datos o proceso. <br/><br/> Para los tipos de almacenes de datos compatibles y sus propiedades de tipo, vea el artículo de [información general sobre los conectores](copy-activity-overview.md#supported-data-stores-and-formats). Vaya al artículo del conector del almacén de datos para obtener información acerca de las propiedades de tipo específicas de un almacén de datos. <br/><br/> Para los tipos de procesos compatibles y sus propiedades de tipo, vea [Servicios de proceso vinculados](compute-linked-services.md). | Sí |
connectVia | El entorno [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Puede usar los entornos Integration Runtime (autohospedado) (si el almacén de datos se encuentra en una red privada) o Azure Integration Runtime. Si no se especifica, se usará Azure Integration Runtime. | No

## <a name="linked-service-example"></a>Ejemplo de servicio vinculado

El siguiente servicio vinculado no es un servicio vinculado de Azure Blob Storage. Tenga en cuenta que el tipo está establecido en Azure Blob Storage. Las propiedades de tipo del servicio vinculado de Azure Blob Storage incluyen una cadena de conexión. El servicio utiliza esta cadena de conexión para conectarse al almacén de datos en el entorno de ejecución.

```json
{
    "name": "AzureBlobStorageLinkedService",
    "properties": {
        "type": "AzureBlobStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="create-linked-services"></a>Crear servicios vinculados

Se pueden crear servicios vinculados en la experiencia de usuario de Azure Data Factory mediante el [centro de administración](author-management-hub.md) y cualquier actividad, conjunto de datos o flujo de datos que haga referencia a ellos.

Los servicios vinculados se pueden crear mediante una de estas herramientas o SDK: [API de .NET](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [API REST](quickstart-create-data-factory-rest-api.md), [plantilla de Azure Resource Manager](quickstart-create-data-factory-resource-manager-template.md) y [Azure Portal](quickstart-create-data-factory-portal.md).


## <a name="data-store-linked-services"></a>Servicios vinculados con almacenes de datos

Puede encontrar la lista de almacenes de datos admitidos en el artículo [Introducción a los conectores](copy-activity-overview.md#supported-data-stores-and-formats). Haga clic en un almacén de datos para obtener información sobre las propiedades de conexión admitidas.

## <a name="compute-linked-services"></a>Servicios vinculados de proceso

Consulte [Entornos de proceso compatibles](compute-linked-services.md) para obtener detalles sobre los diferentes entornos de proceso a los que puede conectarse desde el servicio, así como las diferentes configuraciones.

## <a name="next-steps"></a>Pasos siguientes

- [Aprenda a usar credenciales de una identidad administrada asignada por el usuario en un servicio vinculado](credentials.md).

Vea los siguientes tutoriales para obtener instrucciones paso a paso sobre cómo crear canalizaciones y conjuntos de datos con una de estas herramientas o SDK.

- [Inicio rápido:Crear una factoría de datos mediante .NET](quickstart-create-data-factory-dot-net.md)
- [Inicio rápido: Crear una factoría de datos mediante PowerShell](quickstart-create-data-factory-powershell.md)
- [Inicio rápido: Crear una factoría de datos mediante la API REST](quickstart-create-data-factory-rest-api.md)
- [Inicio rápido: Crear una factoría de datos mediante Azure Portal](quickstart-create-data-factory-portal.md)
