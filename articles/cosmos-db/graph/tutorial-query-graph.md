---
title: ¿Cómo consultar datos del gráfico en Azure Cosmos DB?
description: Obtenga información sobre cómo consultar datos de gráficos desde Azure Cosmos DB mediante consultas de Gremlin.
author: manishmsfte
ms.author: mansha
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: tutorial
ms.date: 11/08/2021
ms.reviewer: sngun
ms.custom: devx-track-csharp
ms.openlocfilehash: ac6aef98b06c485bc16777b130c39f9006e6ffff
ms.sourcegitcommit: 61f87d27e05547f3c22044c6aa42be8f23673256
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2021
ms.locfileid: "132060610"
---
# <a name="tutorial-query-azure-cosmos-db-gremlin-api-by-using-gremlin"></a>Tutorial: Consulta de Gremlin API de Azure Cosmos DB mediante Gremlin
[!INCLUDE[appliesto-gremlin-api](../includes/appliesto-gremlin-api.md)]

[Gremlin API](graph-introduction.md) de Azure Cosmos DB admite consultas de [Gremlin](https://github.com/tinkerpop/gremlin/wiki). En este artículo se proporcionan consultas y documentos de ejemplo a modo de introducción. En el artículo [Compatibilidad con Gremlin](gremlin-support.md) se incluye una referencia de Gremlin detallada.

En este artículo se tratan las tareas siguientes: 

> [!div class="checklist"]
> * Consulta de datos con Gremlin

## <a name="prerequisites"></a>Prerrequisitos

Para que estas consultas funcionen, debe tener una cuenta de Azure Cosmos DB, así como datos del gráfico en el contenedor. ¿No tiene nada de lo anterior? Complete el [inicio rápido en 5 minutos](create-graph-dotnet.md) o el [tutorial de desarrolladores](tutorial-query-graph.md) para crear una cuenta y rellenar la base de datos. Puede ejecutar las siguientes consultas en la [consola de Gremlin](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) o en el controlador de Gremlin que prefiera.

## <a name="count-vertices-in-the-graph"></a>Recuento de vértices del gráfico

En el siguiente fragmento se muestra cómo contar el número de vértices del gráfico:

```
g.V().count()
```

## <a name="filters"></a>Filtros

Puede aplicar filtros mediante los pasos `has` y `hasLabel` de Gremlin y combinarlos con `and`, `or` y `not` para crear filtros más complejos. Azure Cosmos DB proporciona indexación independiente del esquema de todas las propiedades de los vértices y grados para consultas rápidas:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Proyección

Puede proyectar determinadas propiedades en los resultados de la consulta mediante el paso `values`:

```
g.V().hasLabel('person').values('name')
```

## <a name="find-related-edges-and-vertices"></a>Búsqueda de bordes y vértices relacionados

Hasta ahora, solo hemos visto operadores de consulta que funcionan en cualquier base de datos. Los grafos son rápidos y eficientes para operaciones de cruce seguro si tiene que ir a bordes y vértices relacionados. Encontremos a todos los amigos de Thomas. Lo hacemos usando el paso `outE` de Gremlin para encontrar todos los bordes exteriores de Thomas y atravesando a continuación por los vértices interiores de esos bordes mediante el paso `inV` de Gremlin:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

La siguiente consulta realiza dos saltos para encontrar a todos los "amigos de los amigos" de Thomas, llamando a `outE` y `inV` dos veces. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Puede crear consultas más complejas e implementar una lógica de cruce seguro del grafo eficaz con Gremlin, incluidas la combinación de expresiones de filtro, la realización de bucles mediante el paso `loop` y la implementación de la navegación condicional mediante el paso `choose`. Obtenga más información sobre lo que puede hacer con [Compatibilidad con Gremlin](gremlin-support.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha hecho lo siguiente:

> [!div class="checklist"]
> * Ha obtenido información sobre cómo realizar consultas con Graph 

Ahora puede continuar a la sección de conceptos para obtener más información sobre Cosmos DB.

> [!div class="nextstepaction"]
> [Distribución global](../distribute-data-globally.md) 

