---
title: Almacenamiento de resultados de consultas desde un grupo de SQL sin servidor
description: En este artículo, aprenderá a almacenar los resultados de las consultas en Storage mediante un grupo de SQL sin servidor.
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: ba93588dc686a57c05371b86d8f9e058f2bd2cf2
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131460040"
---
# <a name="store-query-results-to-storage-using-serverless-sql-pool-in-azure-synapse-analytics"></a>Almacenamiento de resultados de consultas en Storage mediante un grupo de SQL sin servidor en Azure Synapse Analytics

En este artículo, aprenderá a almacenar los resultados de las consultas en Storage mediante un grupo de SQL sin servidor.

## <a name="prerequisites"></a>Requisitos previos

El primer paso es **crear una base de datos** en la que se ejecutarán las consultas. Luego, se inicializan los objetos, para lo que hay que ejecutar un [script de instalación](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql) en esa base de datos. Este script de instalación creará los orígenes de datos, las credenciales con ámbito de base de datos y los formatos de archivo externos que se usan para leer datos en estos ejemplos.

Siga las instrucciones de este artículo para crear orígenes de datos, credenciales con ámbito de base de datos y formatos de archivo externos que se usan para escribir datos en el almacenamiento de salida.

## <a name="create-external-table-as-select"></a>CREATE EXTERNAL TABLE AS SELECT

Puede usar la instrucción CREATE EXTERNAL TABLE AS SELECT (CETAS) para almacenar los resultados de la consulta en Storage.

> [!NOTE]
> Cambie la primera línea de la consulta, es decir, [mydbname], para usar la base de datos que ha creado.

```sql
USE [mydbname];
GO

CREATE DATABASE SCOPED CREDENTIAL [SasTokenWrite]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
     SECRET = 'sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-04-18T20:42:12Z&st=2019-04-18T12:42:12Z&spr=https&sig=lQHczNvrk1KoYLCpFdSsMANd0ef9BrIPBNJ3VYEIq78%3D';
GO

CREATE EXTERNAL DATA SOURCE [MyDataSource] WITH (
    LOCATION = 'https://<storage account name>.blob.core.windows.net/csv', CREDENTIAL = [SasTokenWrite]
);
GO

CREATE EXTERNAL FILE FORMAT [ParquetFF] WITH (
    FORMAT_TYPE = PARQUET,
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
);
GO

CREATE EXTERNAL TABLE [dbo].[PopulationCETAS] WITH (
        LOCATION = 'populationParquet/',
        DATA_SOURCE = [MyDataSource],
        FILE_FORMAT = [ParquetFF]
) AS
SELECT
    *
FROM
    OPENROWSET(
        BULK 'csv/population-unix/population.csv',
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT = 'CSV', PARSER_VERSION = '2.0'
    ) WITH (
        CountryCode varchar(4),
        CountryName varchar(64),
        Year int,
        PopulationCount int
    ) AS r;

```

> [!NOTE]
> Debe modificar este script y cambiar la ubicación de destino para ejecutarlo de nuevo. No se pueden crear tablas externas en la ubicación en la que ya tenga algunos datos.

## <a name="use-the-external-table"></a>Uso de una tabla externa

La tabla externa que se crea mediante CETAS se puede usar como una tabla externa normal.

> [!NOTE]
> Cambie la primera línea de la consulta, es decir, [mydbname], para usar la base de datos que ha creado.

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM PopulationCETAS
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="remarks"></a>Comentarios

Una vez que se almacenan los resultados, los datos de la tabla externa no se pueden modificar. Este script no se puede repetir porque CETAS no sobrescribirá los datos subyacentes creados en la ejecución anterior. Vote por los siguientes elementos de comentarios si algunos de ellos son necesarios en sus escenarios o proponga unos nuevos en el sitio de comentarios de Azure:
- [Habilitación de la inserción de nuevos datos en tablas externas](https://feedback.azure.com/d365community/forum/9b9ba8e4-0825-ec11-b6e6-000d3a4f07b8)
- [Habilitación de la eliminación de datos de tablas externas](https://feedback.azure.com/d365community/idea/fb5a00c9-0a25-ec11-b6e6-000d3a4f07b8)
- [Especificación de particiones en CETAS](https://feedback.azure.com/d365community/idea/e28278db-0a25-ec11-b6e6-000d3a4f07b8)
- [Especificación de tamaños y recuentos de archivos](https://feedback.azure.com/d365community/idea/262048b9-0925-ec11-b6e6-000d3a4f07b8)

Los únicos tipos de salida que se admiten son Parquet y CSV. Puede votar por los otros tipos en el [sitio de comentarios de Azure](https://feedback.azure.com/d365community/forum/9b9ba8e4-0825-ec11-b6e6-000d3a4f07b8).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo realizar consultas en distintos tipos de archivo, consulte los artículos [Consulta de archivos .csv](query-single-csv-file.md), [Consulta de archivos Parquet](query-parquet-files.md) y [Consulta de archivos JSON](query-json-files.md).
