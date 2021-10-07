---
title: Filtrar los resultados de la búsqueda
titleSuffix: Azure Cognitive Search
description: Puede filtrar por identidad de seguridad del usuario, idioma, geolocalización o valores numéricos para reducir los resultados de búsqueda para las consultas de Azure Cognitive Search, un servicio de búsqueda en la nube hospedado de Microsoft Azure.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/28/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: f2356235bf70a5fdd3e284c26d421e16ca94fb59
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129208502"
---
# <a name="filters-in-azure-cognitive-search"></a>Filtros de Azure Cognitive Search 

Un *filtro* proporciona criterios para seleccionar los documentos que se usan en una consulta. Un filtro puede ser un valor único o una [expresión de filtro](search-query-odata-filter.md) de OData. A diferencia de la búsqueda de texto completo, un valor o una expresión de filtro solo devuelve una coincidencia estricta.

Algunas experiencias de búsqueda, como la [navegación por facetas](search-faceted-navigation.md), dependen de los filtros como parte de la implementación, pero puede usar filtros siempre que quiera establecer el ámbito de una consulta en valores específicos. Por otro lado, si su objetivo es limitar una consulta a campos específicos, hay métodos alternativos, los cuales se describen a continuación.

## <a name="when-to-use-a-filter"></a>Cuándo se debe usar un filtro

Los filtros son fundamentales para varias experiencias de búsqueda, como la búsqueda de servicios cercanos, la navegación por facetas y los filtros de seguridad que muestran únicamente los documentos que un usuario tiene permiso para ver. Si implementa alguna de estas experiencias, se necesita un filtro. El filtro asociado a la consulta de búsqueda proporciona coordenadas de geolocalización, la categoría de faceta que seleccionó el usuario o el id. de seguridad del solicitante.

Entre los escenarios habituales se incluyen los siguientes:

+ Segmentación de los resultados de la búsqueda en función del contenido del índice. A partir de un esquema con la ubicación, las categorías y los servicios de los hoteles, puede crear un filtro para que coincida explícitamente con los criterios (por ejemplo, en Seattle, sobre el agua, con vistas). 

+ Implementación de una experiencia de búsqueda con un requisito de filtro:

  + La [navegación por facetas](search-faceted-navigation.md) usa un filtro para devolver la categoría de faceta que seleccionó el usuario.
  + La búsqueda georreferenciada usa un filtro para pasar las coordenadas de la ubicación actual en aplicaciones que buscan servicios cercanos. 
  + Los [filtros de seguridad](search-security-trimming-for-azure-search.md) pasan los identificadores de seguridad como criterios de filtro, donde una coincidencia en el índice actúa como un proxy para los derechos de acceso al documento.

+ Realización de una "búsqueda de números". Los campos numéricos se pueden recuperar y pueden aparecer en los resultados de búsqueda, pero no se pueden hacer búsquedas (de texto completo) en ellos individualmente. Si necesita criterios de selección basados en datos numéricos, use un filtro.

### <a name="alternative-methods-for-reducing-scope"></a>Métodos alternativos para reducir el ámbito

Si quiere un efecto de restricción en los resultados de búsqueda, los filtros no son la única opción. Estas alternativas podrían ser una solución mejor, en función de su objetivo:

+ El parámetro de consulta `searchFields` restringe la consulta a unos campos específicos. Por ejemplo, si el índice proporciona campos independientes para descripciones en inglés y español, puede usar searchFields para definir como destino los campos que quiera usar para la búsqueda de texto completo. 

+ El parámetro `$select` se usa para especificar qué campos se deben incluir en un conjunto de resultados, lo que recorta de forma eficaz la respuesta antes de enviarla a la aplicación que realiza la llamada. Este parámetro no refina la consulta ni reduce la colección de documentos, pero, si el objetivo es una respuesta menor, considere la opción de usar este parámetro. 

Para obtener más información acerca de estos parámetros, consulte [Search Documents > Request > Query parameters](/rest/api/searchservice/search-documents#query-parameters) (Buscar en documentos > Solicitud > Parámetros de consulta).

## <a name="how-filters-are-executed"></a>Cómo se ejecutan los filtros

En el momento de la consulta, un analizador de filtro acepta criterios como entrada, convierte la expresión en expresiones booleanas atómicas representadas como un árbol y, posteriormente, evalúa el árbol de filtro a través de campos por los que se puede filtrar en un índice.

El filtrado se produce en paralelo con la búsqueda y define los documentos que se incluirán en un procesamiento descendente para la recuperación de documentos y la puntuación de importancia. Combinado con la cadena de búsqueda, el filtro reduce de forma eficaz el conjunto de recuperación de la operación de búsqueda posterior. Cuando se usa solo (por ejemplo, cuando la cadena de consulta está vacía en `search=*`), los criterios de filtro son la única entrada. 

## <a name="defining-filters"></a>Definición de filtros

Los filtros son expresiones de OData que se articulan en la [sintaxis de filtro](search-query-odata-filter.md) admitida por Cognitive Search.

Puede especificar un filtro para cada operación **search**, pero el filtro en sí puede incluir varios campos, varios criterios y, si usa una función **ismatch**, varias expresiones de búsqueda de texto completo. En una expresión de filtro de varias partes, puede especificar predicados en cualquier orden (respetando las reglas de prioridad del operador). No hay ninguna mejora apreciable del rendimiento si intenta reorganizar los predicados en una secuencia determinada.

Uno de los límites en una expresión de filtro es el límite de tamaño máximo de la solicitud. La solicitud completa, incluido el filtro, puede tener un máximo de 16 MB para POST u 8 KB para GET. También hay un límite en el número de cláusulas de la expresión de filtro. Como regla general, si tiene cientos de cláusulas, está en riesgo de alcanzar el límite. Es recomendable diseñar la aplicación de manera que no genere filtros de tamaño ilimitado.

Los ejemplos siguientes representan definiciones de filtro prototípicas en varias API.

```http
# Option 1:  Use $filter for GET
GET https://[service name].search.windows.net/indexes/hotels/docs?api-version=2020-06-30&search=*&$filter=Rooms/any(room: room/BaseRate lt 150.0)&$select=HotelId, HotelName, Rooms/Description, Rooms/BaseRate

# Option 2: Use filter for POST and pass it in the request body
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2020-06-30
{
    "search": "*",
    "filter": "Rooms/any(room: room/BaseRate lt 150.0)",
    "select": "HotelId, HotelName, Rooms/Description, Rooms/BaseRate"
}
```

```csharp
    parameters =
        new SearchParameters()
        {
            Filter = "Rooms/any(room: room/BaseRate lt 150.0)",
            Select = new[] { "HotelId", "HotelName", "Rooms/Description" ,"Rooms/BaseRate"}
        };

    var results = searchIndexClient.Documents.Search("*", parameters);
```

## <a name="filter-patterns"></a>Patrones de filtro

En los ejemplos siguientes se muestran varios patrones de uso para escenarios de filtro. Para obtener más ideas, consulte [OData expression syntax > Examples](./search-query-odata-filter.md#examples) (Sintaxis de expresiones de OData > Ejemplos).

+ La expresión **$filter** independiente, sin una cadena de consulta, resulta útil cuando la expresión de filtro puede definir completamente los documentos de interés. Sin una cadena de consulta, no hay ningún análisis lingüístico ni léxico, ninguna puntuación y ninguna clasificación. Observe que la cadena de búsqueda es simplemente un asterisco, que significa "coincidencia con todos los documentos".

  ```json
  {
    "search": "*",
    "filter": "Rooms/any(room: room/BaseRate ge 60 and room/BaseRate lt 300) and Address/City eq 'Honolulu"
  }
  ```

+ Combinación de una cadena de consulta y **$filter**, donde el filtro crea el subconjunto y la cadena de consulta proporciona las entradas de términos para la búsqueda de texto completo en el subconjunto filtrado. La adición de términos (cines a poca distancia) introduce puntuaciones de búsqueda en los resultados, donde los documentos que mejor coinciden con los términos se clasifican en primer lugar. Usar un filtro con una cadena de consulta es el patrón de uso más común.

  ```json
  {
    "search": "walking distance theaters",
    "filter": "Rooms/any(room: room/BaseRate ge 60 and room/BaseRate lt 300) and Address/City eq 'Seattle'"
  }

+ Compound queries, separated by "or", each with its own filter criteria (for example, 'beagles' in 'dog' or 'siamese' in 'cat'). Expressions combined with `or` are evaluated individually, with the union of documents matching each expression sent back in the response. This usage pattern is achieved through the `search.ismatchscoring` function. You can also use the non-scoring version, `search.ismatch`.

   ```http
   # Match on hostels rated higher than 4 OR 5-star motels.
   $filter=search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

   # Match on 'luxury' or 'high-end' in the description field OR on category exactly equal to 'Luxury'.
   $filter=search.ismatchscoring('luxury | high-end', 'Description') or Category eq 'Luxury'&$count=true
   ```

  También es posible combinar la búsqueda de texto completo a través de `search.ismatchscoring` con filtros mediante `and` en lugar de `or`, pero esto es funcionalmente equivalente a usar los parámetros `search` y `$filter` en una solicitud de búsqueda. Por ejemplo, las dos consultas siguientes producen el mismo resultado:

  ```http
  $filter=search.ismatchscoring('pool') and Rating ge 4

  search=pool&$filter=Rating ge 4
  ```

## <a name="field-requirements-for-filtering"></a>Requisitos de campo para filtrar

En la API REST, el filtrado está *activado* de forma predeterminada para campos simples. Los campos que se pueden filtrar aumentan el tamaño del índice. Asegúrese de definir `"filterable": false` para los campos que no tiene previsto usar en un filtro. Para obtener más información sobre la configuración de definiciones de campos, consulte [Create Index](/rest/api/searchservice/create-index) (Creación de índices).

En el SDK de. NET, el filtrado está *desactivado* de forma predeterminada. Para hacer que un campo se pueda filtrar, establezca la [propiedad IsFilterable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable) del objeto [SearchField](/dotnet/api/azure.search.documents.indexes.models.searchfield) correspondiente en `true`. En el ejemplo siguiente, el atributo se establece en la propiedad `BaseRate` de una clase de modelo que se asigna a la definición del índice.

```csharp
[IsFilterable, IsSortable, IsFacetable]
public double? BaseRate { get; set; }
```

### <a name="making-an-existing-field-filterable"></a>Hacer que un campo existente se pueda filtrar

No puede modificar los campos existentes para hacer que se puedan filtrar. Como alternativa, deberá agregar un campo nuevo o volver a generar el índice. Para obtener más información sobre cómo volver a generar un índice o volver a rellenar los campos, consulte [Recompilación de un índice de Azure Cognitive Search](search-howto-reindex.md).

## <a name="text-filter-fundamentals"></a>Conceptos básicos de filtro de texto

Los filtros de texto coinciden con los campos de cadena en las cadenas literales que se proporcionan en el filtro: `$filter=Category eq 'Resort and Spa'`.

A diferencia de la búsqueda de texto completo, no hay ningún análisis léxico ni separación de palabras para los filtros de texto, por lo que las comparaciones se aplican solo a coincidencias exactas. Por ejemplo, si un campo *f* contiene "sunny day", `$filter=f eq 'Sunny'` no coincide, pero `$filter=f eq 'sunny day'` sí. 

Las cadenas de texto distinguen mayúsculas de minúsculas. Las palabras en mayúsculas no se buscan en minúsculas: `$filter=f eq 'Sunny day'` no encontrará "sunny day".

### <a name="approaches-for-filtering-on-text"></a>Enfoques para el filtrado en el texto

| Enfoque | Descripción | Cuándo se usa |
|----------|-------------|-------------|
| [`search.in`](search-query-odata-search-in-function.md) | Una función que coincide con un campo en una lista delimitada de cadenas. | Se recomienda para los [filtros de seguridad](search-security-trimming-for-azure-search.md) y para los filtros donde se necesita que muchos valores de texto sin formato coincidan con un campo de cadena. La función **search.in** está diseñada para acelerar el proceso y es mucho más rápida que comparar explícitamente el campo con cada cadena mediante `eq` y `or`. | 
| [`search.ismatch`](search-query-odata-full-text-search-functions.md) | Una función que permite combinar las operaciones de búsqueda de texto completo con operaciones de filtro estrictamente booleano en la misma expresión de filtro. | Use **search.ismatch** (o su equivalente de puntuación, **search.ismatchscoring**) cuando quiera varias combinaciones de filtro de búsqueda en una sola solicitud. También puede usarla para que un filtro *contains* filtre una cadena parcial en una cadena mayor. |
| [`$filter=field operator string`](search-query-odata-comparison-operators.md) | Expresión definida por el usuario compuesta de campos, operadores y valores. | Se usa cuando se quieren buscar coincidencias exactas entre un campo de cadena y un valor de cadena. |

## <a name="numeric-filter-fundamentals"></a>Conceptos básicos de filtro numérico

Los campos numéricos no se pueden definir como `searchable` en el contexto de búsqueda de texto completo. Solo las cadenas están sujetas a la búsqueda de texto completo. Por ejemplo, si escribe 99.99 como término de búsqueda, no obtendrá artículos con un precio de 99.99 USD. Lo que se devolverá son elementos con el número 99 en los campos de cadena del documento. Por lo tanto, si tiene datos numéricos, se asume que los va a usar para los filtros, como rangos, facetas, grupos, etc. 

Los documentos que contienen campos numéricos (precio, tamaño, SKU, id.) proporcionan esos valores en los resultados de la búsqueda si el campo está marcado como `retrievable`. En resumen, la búsqueda de texto completo no es aplicable a los tipos de campo numéricos.

## <a name="next-steps"></a>Pasos siguientes

En primer lugar, pruebe el **Explorador de búsqueda** en el portal para enviar consultas con parámetros **$filter**. El [índice real-estate-sample](search-get-started-portal.md) proporciona resultados interesantes para las siguiente consultas filtradas al pegarlas en la barra de búsqueda:

```
# Geo-filter returning documents within 5 kilometers of Redmond, Washington state
# Use $count=true to get a number of hits returned by the query
# Use $select to trim results, showing values for named fields only
# Use search=* for an empty query string. The filter is the sole input

search=*&$count=true&$select=description,city,postCode&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5

# Numeric filters use comparison like greater than (gt), less than (lt), not equal (ne)
# Include "and" to filter on multiple fields (baths and bed)
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=baths gt 3 and beds gt 4

# Text filters can also use comparison operators
# Wrap text in single or double quotes and use the correct case
# Full text search is on John Leclerc, matching on John or Leclerc

search=John Leclerc&$count=true&$select=source,city,postCode,baths,beds&$filter=city gt 'Seattle'
```

Para trabajar con más ejemplos, consulte [OData Filter Expression Syntax > Examples](./search-query-odata-filter.md#examples) (Sintaxis de expresión de filtro de OData > Ejemplos).

## <a name="see-also"></a>Consulte también

+ [Funcionamiento de la búsqueda de texto completo en Azure Cognitive Search](search-lucene-query-architecture.md)
+ [API de REST de documentos de búsqueda](/rest/api/searchservice/search-documents)
+ [Sintaxis de consulta simplificada](/rest/api/searchservice/simple-query-syntax-in-azure-search)
+ [Sintaxis de consulta de Lucene](/rest/api/searchservice/lucene-query-syntax-in-azure-search)
+ [Tipos de datos admitidos](/rest/api/searchservice/supported-data-types)