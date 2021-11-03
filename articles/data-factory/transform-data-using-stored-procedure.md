---
title: Transformación de datos mediante la actividad de procedimiento almacenado
titleSuffix: Azure Data Factory & Azure Synapse
description: Se explica cómo usar la actividad de procedimiento almacenado de SQL Server para invocar un procedimiento almacenado en una instancia de Azure SQL Database o Data Warehouse desde una canalización de Azure Data Factory o Synapse Analytics.
ms.service: data-factory
ms.subservice: tutorials
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 30e40c20aa8e11add35b270baf867a5fd5e46058
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131016579"
---
# <a name="transform-data-by-using-the-sql-server-stored-procedure-activity-in-azure-data-factory-or-synapse-analytics"></a>Transformación de datos mediante la actividad de procedimiento almacenado de SQL Server en Azure Data Factory o Synapse Analytics
> [!div class="op_single_selector" title1="Seleccione la versión del servicio Data Factory que usa:"]
> * [Versión 1](v1/data-factory-stored-proc-activity.md)
> * [Versión actual](transform-data-using-stored-procedure.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Las actividades de transformación de una [canalización](concepts-pipelines-activities.md) de Data Factory o Synapse se usan para transformar y procesar los datos sin procesar a fin de convertirlos en predicciones y conclusiones. La actividad de procedimiento almacenado es una de las actividades de transformación que admiten las canalizaciones. Este artículo se basa en el artículo sobre [transformación de datos](transform-data.md), que presenta información general de la transformación de datos y las actividades de transformación admitidas.

> [!NOTE]
> Si no está familiarizado con Azure Data Factory, vea [Introducción a Azure Data Factory](introduction.md) y siga el tutorial sobre la [Transformación de datos](tutorial-transform-data-spark-powershell.md) antes de leer este artículo.  Para más información sobre Synapse Analytics, lea [¿Qué es Azure Synapse Analytics?](../synapse-analytics/overview-what-is.md).

Puede usar la actividad de procedimiento almacenado para invocar un procedimiento almacenado en uno de los siguientes almacenes de datos de la empresa o en una máquina virtual (VM) de Azure: 

- Azure SQL Database
- Azure Synapse Analytics
- Base de datos de SQL Server.  Si se usa SQL Server, se debe instalar el entorno de ejecución de integración autohospedado en el mismo equipo que hospeda la base de datos o en un equipo independiente que tenga acceso a la base de datos. El entorno de ejecución de integración autohospedado es un componente que conecta orígenes de datos locales o en la máquina virtual de Azure con servicios en la nube de forma segura y administrada. Consulte el artículo sobre el [entorno de ejecución de integración autohospedado](create-self-hosted-integration-runtime.md) para más información.

> [!IMPORTANT]
> Al copiar datos en Azure SQL Database o SQL Server, se puede configurar **SqlSink** en la actividad de copia para invocar un procedimiento almacenado mediante la propiedad **sqlWriterStoredProcedureName**. Para más información sobre la propiedad, consulte los artículos sobre los conectores siguientes: [Azure SQL Database](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md). No se admite la invocación de un procedimiento almacenado al copiar datos en Azure Synapse Analytics mediante una actividad de copia. Pero se puede usar la actividad de procedimiento almacenado para invocar un procedimiento almacenado de Azure Synapse Analytics. 
>
> Al copiar datos de Azure SQL Database, SQL Server o Azure Synapse Analytics, se puede configurar **SqlSource** en la actividad de copia para invocar un procedimiento almacenado de lectura de datos desde la base de datos de origen mediante la propiedad **sqlReaderStoredProcedureName**. Para más información, consulte los artículos sobre los conectores siguientes: [Azure SQL Database](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md), [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md)          

 

## <a name="syntax-details"></a>Detalles de la sintaxis
Este es el formato JSON para definir una actividad de procedimiento almacenado:

```json
{
    "name": "Stored Procedure Activity",
    "description":"Description",
    "type": "SqlServerStoredProcedure",
    "linkedServiceName": {
        "referenceName": "AzureSqlLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "storedProcedureName": "usp_sample",
        "storedProcedureParameters": {
            "identifier": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" }

        }
    }
}
```

En la tabla siguiente se describen estas propiedades JSON:

| Propiedad                  | Descripción                              | Obligatorio |
| ------------------------- | ---------------------------------------- | -------- |
| name                      | Nombre de la actividad                     | Sí      |
| description               | Texto que describe para qué se usa la actividad. | No       |
| type                      | Para la actividad de procedimiento almacenado, el tipo de actividad es **SqlServerStoredProcedure** | Sí      |
| linkedServiceName         | Referencia a **Azure SQL Database**, **Azure Synapse Analytics** o **SQL Server** registrada como un servicio vinculado en Data Factory. Para obtener más información sobre este servicio vinculado, vea el artículo [Compute linked services](compute-linked-services.md) (Servicios vinculados de procesos). | Sí      |
| storedProcedureName       | Especifique el nombre del procedimiento almacenado que se invocará. | Sí      |
| storedProcedureParameters | Especifique los valores para los parámetros del procedimiento almacenado. Use `"param1": { "value": "param1Value","type":"param1Type" }` para pasar valores de parámetros y su tipo compatible con el origen de datos. Si necesita pasar NULL para un parámetro, use `"param1": { "value": null }` (todo en minúsculas). | No       |

## <a name="parameter-data-type-mapping"></a>Asignación de tipos de datos de parámetros
El tipo de datos que especifique para el parámetro es el tipo de servicio interno que se asigna al tipo de datos del origen de datos que usa. Puede encontrar las descripciones de las asignaciones de tipos de datos para el origen de datos en la documentación sobre los conectores. Por ejemplo:

- [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#data-type-mapping-for-azure-synapse-analytics)
- [Asignación de tipo de datos para Azure SQL Database](connector-azure-sql-database.md#data-type-mapping-for-azure-sql-database)
- [Asignación de tipos de datos para Oracle](connector-oracle.md#data-type-mapping-for-oracle)
- [Asignación de tipos de datos para SQL Server](connector-sql-server.md#data-type-mapping-for-sql-server)

## <a name="next-steps"></a>Pasos siguientes
Vea los siguientes artículos, en los que se explica cómo transformar datos de otras maneras: 

* [Actividad U-SQL](transform-data-using-data-lake-analytics.md)
* [Actividad de Hive](transform-data-using-hadoop-hive.md)
* [Actividad de Pig](transform-data-using-hadoop-pig.md)
* [Actividad MapReduce](transform-data-using-hadoop-map-reduce.md)
* [Actividad de streaming de Hadoop](transform-data-using-hadoop-streaming.md)
* [Actividad de Spark](transform-data-using-spark.md)
* [Actividad personalizada de .NET](transform-data-using-dotnet-custom-activity.md)
* [Actividad de procedimiento almacenado](transform-data-using-stored-procedure.md)
