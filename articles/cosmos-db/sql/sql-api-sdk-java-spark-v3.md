---
title: Notas de la versión y recursos del conector OLTP de Azure Cosmos DB Apache Spark 3 para SQL API (versión preliminar)
description: Obtenga información sobre el conector OLTP de Azure Cosmos DB Apache Spark 3 para SQL API, como las fechas de lanzamiento, las fechas de retirada y los cambios realizados entre las diferentes versiones del SDK de Java de Azure Cosmos DB para SQL.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 06/21/2021
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 8dde6c0a9bffd9d34b1e7d36588e8b04bb80079e
ms.sourcegitcommit: dcf1defb393104f8afc6b707fc748e0ff4c81830
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2021
ms.locfileid: "123113687"
---
# <a name="azure-cosmos-db-apache-spark-3-oltp-connector-for-core-sql-api-release-notes-and-resources"></a>Conector OLTP de Azure Cosmos DB Apache Spark 3 para Core (SQL) API: notas de la versión y recursos
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [SDK de .NET Core v2](sql-api-sdk-dotnet-core.md)
> * [SDK de fuente de cambios de .NET, versión 2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [SDK de Java v4](sql-api-sdk-java-v4.md)
> * [Versión 2 del SDK de Java asincrónico](sql-api-sdk-async-java.md)
> * [SDK de Java v2 sincrónico](sql-api-sdk-java.md)
> * [Spring Data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data v3](sql-api-sdk-java-spring-v3.md)
> * [Conector Spark 3 OLTP](sql-api-sdk-java-spark-v3.md)
> * [Conector Spark 2 OLTP](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [Proveedor de recursos de REST](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-query-getting-started.md)
> * [Bulk Executor: .NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor: Java](sql-api-sdk-bulk-executor-java.md)

El **conector OLTP de Azure Cosmos DB Spark 3** proporciona compatibilidad con Apache Spark v3 en Azure Cosmos DB mediante SQL API.
[Azure Cosmos DB](../introduction.md) es un servicio de base de datos de distribución global que permite a los desarrolladores trabajar con datos mediante diversas API estándar, como SQL, MongoDB, Cassandra, Graph y Table.

## <a name="documentation"></a>Documentación

- [Introducción](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/cosmos/azure-cosmos-spark_3-1_2-12/docs/quick-start.md)
- [API de catálogo](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/cosmos/azure-cosmos-spark_3-1_2-12/docs/catalog-api.md)
- [Referencia de parámetros de configuración](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/cosmos/azure-cosmos-spark_3-1_2-12/docs/configuration-reference.md)


## <a name="version-compatibility"></a>Compatibilidad de versiones

| Conector     | Spark         | Versión mínima de Java | Versiones de Scala admitidas |
| ------------- | ------------- | -------------------- | -----------------------  |
| 4.0.0         | 3.1.1         |        8             | 2,12                     |

## <a name="download"></a>Descargar 

Puede usar la coordenada de Maven del archivo JAR para instalar automáticamente el conector de Spark en Databricks Runtime 8 desde Maven: `com.azure.cosmos.spark:azure-cosmos-spark_3-1_2-12:4.1.0`.

También puede realizar la integración con el conector Cosmos DB Spark en el proyecto de SBT:
```scala
libraryDependencies += "com.azure.cosmos.spark" % "azure-cosmos-spark_3-1_2-12" % "4.1.0"
```

El conector Cosmos DB Spark está disponible en el [repositorio central de Maven](https://search.maven.org/artifact/com.azure.cosmos.spark/azure-cosmos-spark_3-1_2-12/).

### <a name="general"></a>General

Si encuentra algún error, registre una incidencia [aquí](https://github.com/Azure/azure-sdk-for-java/issues/new).

Para sugerir una nueva característica o cambios que se podrían realizar, registre una incidencia de la misma manera que haría en el caso de un error.

[!INCLUDE[Changelog](~/azure-sdk-for-java-cosmos-db/sdk/cosmos/azure-cosmos-spark_3-1_2-12/CHANGELOG.md)]

## <a name="next-steps"></a>Pasos siguientes

Revise nuestra [guía de inicio rápido para trabajar con el conector OLTP de Azure Cosmos DB Spark 3](create-sql-api-spark.md).
