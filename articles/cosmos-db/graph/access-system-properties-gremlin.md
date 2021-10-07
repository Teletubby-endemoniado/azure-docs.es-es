---
title: Acceso a las propiedades de documentos del sistema mediante Azure Cosmos DB Graph
description: Obtenga información sobre cómo leer y escribir propiedades de documentos del sistema de Cosmos DB mediante la API de Gremlin.
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: how-to
ms.date: 09/16/2021
author: manishmsfte
ms.author: mansha
ms.openlocfilehash: 8a0574d5622ae0ceceb52be72ccd8c5d99a18529
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128614814"
---
# <a name="system-document-properties"></a>Propiedades de documentos del sistema
[!INCLUDE[appliesto-gremlin-api](../includes/appliesto-gremlin-api.md)]

Azure Cosmos DB tiene [propiedades del sistema](/rest/api/cosmos-db/databases) como ```_ts```, ```_self```, ```_attachments```, ```_rid``` y ```_etag``` en todos los documentos. Además, el motor de Gremlin agrega las propiedades ```inVPartition``` y ```outVPartition``` en los bordes. De forma predeterminada, estas propiedades están disponibles para el recorrido. Sin embargo, es posible incluir propiedades específicas o todas ellas en el recorrido de Gremlin.

```console
g.withStrategies(ProjectionStrategy.build().IncludeSystemProperties('_ts').create())
```

## <a name="e-tag"></a>E-Tag

Esta propiedad se usa para el control de simultaneidad optimista. Si la aplicación necesita interrumpir la operación en unos recorridos distintos, puede usar la propiedad e-Tag para evitar la pérdida de datos en las operaciones de escritura simultáneas.

```console
g.withStrategies(ProjectionStrategy.build().IncludeSystemProperties('_etag').create()).V('1').has('_etag', '"00000100-0000-0800-0000-5d03edac0000"').property('test', '1')
```

## <a name="time-to-live-ttl"></a>Período de vida (TTL)

Si la colección tiene habilitada la caducidad del documento y los documentos tienen la propiedad `ttl` establecida, esta propiedad estará disponible en el recorrido de Gremlin como una propiedad normal del vértice o borde. `ProjectionStrategy` no es necesario para habilitar la exposición de la propiedad de período de vida.

* Utilice el siguiente comando para establecer el período de vida de un nuevo vértice:

  ```console
  g.addV(<ID>).property('ttl', <expirationTime>)
  ```

  Por ejemplo, un vértice creado con el siguiente recorrido se elimina automáticamente después de *123 segundos*:

  ```console
  g.addV('vertex-one').property('ttl', 123)
  ```

* Utilice el siguiente comando para establecer el período de vida de un vértice existente:

  ```console
  g.V().hasId(<ID>).has('pk', <pk>).property('ttl', <expirationTime>)
  ```

* Cuando la propiedad de período de vida se aplica en los vértices, no se aplica automáticamente a los bordes, ya que los bordes son registros independientes del almacén de la base de datos. Utilice el siguiente comando para establecer el período de vida de los vértices y todos sus bordes de entrada y salida:

  ```console
  g.V().hasId(<ID>).has('pk', <pk>).as('v').bothE().hasNot('ttl').property('ttl', <expirationTime>)
  ```

Puede establecer la propiedad TTL del contenedor en -1 o en **On (no default)** desde Azure Portal. De ese modo, el valor de TTL será infinito para cualquier elemento a menos que se establezca explícitamente.

## <a name="next-steps"></a>Pasos siguientes
* [Simultaneidad optimista de Cosmos DB](../faq.yml#how-does-the-sql-api-provide-concurrency-)
* [Período de vida (TTL)](../time-to-live.md) en Azure Cosmos DB
