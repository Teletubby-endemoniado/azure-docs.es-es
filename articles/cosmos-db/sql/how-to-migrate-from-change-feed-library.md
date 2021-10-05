---
title: Migración desde la biblioteca de procesadores de fuente de cambios al SDK de Azure Cosmos DB para .NET V3
description: Obtenga información sobre cómo migrar la aplicación desde el uso de la biblioteca de procesadores de fuente de cambios al SDK de Azure Cosmos DB V3.
author: ealsur
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: how-to
ms.date: 09/13/2021
ms.author: maquaran
ms.custom: devx-track-dotnet
ms.openlocfilehash: ad060c3fce28ef70137e0f25e09a1e4ea5fb9a09
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128561485"
---
# <a name="migrate-from-the-change-feed-processor-library-to-the-azure-cosmos-db-net-v3-sdk"></a>Migración desde la biblioteca de procesadores de fuente de cambios al SDK de Azure Cosmos DB para .NET V3
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

En este artículo se describen los pasos necesarios para migrar el código de una aplicación existente que usa la [biblioteca de procesadores de fuente de cambios](https://github.com/Azure/azure-documentdb-changefeedprocessor-dotnet) a la característica de [fuente de cambios](../change-feed.md) en la versión más reciente del SDK de .NET (que también se conoce como el SDK de .NET V3).

## <a name="required-code-changes"></a>Cambios de código necesarios

El SDK de .NET V3 tiene varios cambios importantes; estos son los pasos principales para migrar la aplicación:

1. Convierta las instancias de `DocumentCollectionInfo` en referencias de `Container` para los contenedores supervisados y de concesiones.
1. Las personalizaciones que usan `WithProcessorOptions` se deben actualizar para usar `WithLeaseConfiguration` y `WithPollInterval` para los intervalos, `WithStartTime`[para la hora de inicio](./change-feed-processor.md#starting-time) y `WithMaxItems` para definir el número máximo de elementos.
1. Establezca el `processorName` en `GetChangeFeedProcessorBuilder` para que coincida con el valor configurado en `ChangeFeedProcessorOptions.LeasePrefix`, o bien, puede usar `string.Empty`.
1. Los cambios ya no se entregan como `IReadOnlyList<Document>` sino que, en su lugar, se trata de un `IReadOnlyCollection<T>` donde `T` es un tipo que debe definir. Ya no hay ninguna clase de elemento base.
1. Para controlar los cambios, ya no necesita una implementación de `IChangeFeedObserver`, sino que debe [definir un delegado](change-feed-processor.md#implementing-the-change-feed-processor). El delegado puede ser una función estática o, si necesita mantener el estado entre las ejecuciones, puede crear su propia clase y pasar un método de instancia como delegado.

Por ejemplo, si el código original para compilar el procesador de fuente de cambios tiene el siguiente aspecto:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=ChangeFeedProcessorLibrary)]

El código migrado tendrá el siguiente aspecto:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=ChangeFeedProcessorMigrated)]

Para el delegado, puede tener un método estático para recibir los eventos. Si fuera a consumir información de `IChangeFeedObserverContext`, puede migrarla para usar `ChangeFeedProcessorContext`:

* `ChangeFeedProcessorContext.LeaseToken` se puede usar en lugar de `IChangeFeedObserverContext.PartitionKeyRangeId`
* `ChangeFeedProcessorContext.Headers` se puede usar en lugar de `IChangeFeedObserverContext.FeedResponse`
* `ChangeFeedProcessorContext.Diagnostics` contiene información detallada sobre la latencia de solicitud para la solución de problemas

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=Delegate)]

## <a name="state-and-lease-container"></a>Contenedor de estado y concesión

De manera similar a la biblioteca de procesadores de fuente de cambios, la característica de fuente de cambios del SDK de .NET V3 usa un [contenedor de concesión](change-feed-processor.md#components-of-the-change-feed-processor) para almacenar el estado. Sin embargo, los esquemas son diferentes.

El procesador de fuente de cambios del SDK V3 detectará cualquier estado de la biblioteca anterior y lo migrará al esquema nuevo de manera automática en la primera ejecución del código de aplicación migrada. 

Puede detener la aplicación de manera segura con el código anterior, migrar el código a la versión nueva, iniciar la aplicación migrada y todos los cambios que se hayan producido mientras se detuvo la aplicación se recogerán y procesarán con la versión nueva.

## <a name="additional-resources"></a>Recursos adicionales

* [SDK de Azure Cosmos DB](sql-api-sdk-dotnet.md)
* [Ejemplos de uso en GitHub](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [Ejemplos adicionales en GitHub](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Pasos siguientes

Puede obtener más información sobre el procesador de la fuente de cambios en los siguientes artículos:

* [Introducción a la fuente de cambios](change-feed-processor.md)
* [Uso del calculador de la fuente de cambios](how-to-use-change-feed-estimator.md)
* [Hora de inicio del procesador de la fuente de cambios](./change-feed-processor.md#starting-time)
* ¿Intenta planear la capacidad de una migración a Azure Cosmos DB?
    * Si lo único que sabe es el número de núcleos virtuales y servidores del clúster de bases de datos existente, lea sobre el [cálculo de unidades de solicitud mediante núcleos o CPU virtuales](../convert-vcore-to-request-unit.md). 
    * Si conoce las velocidades de solicitud típicas de la carga de trabajo de base de datos actual, lea sobre el [cálculo de las unidades de solicitud mediante la herramienta de planeamiento de capacidad de Azure Cosmos DB](estimate-ru-with-capacity-planner.md).