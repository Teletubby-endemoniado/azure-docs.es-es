---
title: Analizadores para procesamientos lingüísticos y textuales
titleSuffix: Azure Cognitive Search
description: Asigne analizadores a los campos de texto que permiten búsquedas de un índice para reemplazar la API Lucene estándar personalizada por otras alternativas personalizadas, predefinidas o específicas del lenguaje.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/08/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 68b6f6794a690313648dfaaaaf49fdd3150b6171
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124735340"
---
# <a name="analyzers-for-text-processing-in-azure-cognitive-search"></a>Analizadores para procesamientos textuales en Búsqueda cognitiva de Azure

Un *analizador* es un componente del [motor de búsqueda de texto completo](search-lucene-query-architecture.md) responsable del procesamiento de cadenas durante el indexado y la ejecución de consultas. El procesamiento de texto (también conocido como análisis léxico) es transformativo, modificando una cadena mediante acciones como las siguientes:

+ Eliminación de las palabras no esenciales (palabras irrelevantes) y los signos de puntuación
+ División de frases y palabras con guiones en las partes que los componen
+ Las palabras en mayúsculas se ponen en minúsculas
+ Reducción de las palabras a sus formas raíz primitivas para mejorar la eficacia del almacenamiento, de modo que se puedan encontrar coincidencias con independencia del tiempo verbal

El análisis se aplica a campos `Edm.String` que están marcados como "buscables", lo que indica que se puede realizar una búsqueda de texto completo. 

En el caso de los campos de esta configuración, el análisis se produce durante la indexación cuando se crean los tokens y, después, de nuevo durante la ejecución de consultas cuando se analizan las consultas y el motor busca tokens coincidentes. Es más probable que se produzca una coincidencia si se usa el mismo analizador para la indexación y las consultas, pero puede establecer el analizador de cada carga de trabajo de forma independiente, según sus requisitos.

Los tipos de consulta que *no* son búsquedas de texto completo como, por ejemplo, los filtros o una búsqueda aproximada, no pasan por la fase de análisis en el lado de la consulta. En su lugar, el analizador envía esas cadenas directamente al motor de búsqueda mediante el patrón que se proporciona como base para la coincidencia. Normalmente, estos formularios de consulta requieren tokens de cadena completa para que la coincidencia de patrones funcione. Para asegurarse de que se usan tokens de términos completos durante la indexación, es posible que necesite [analizadores personalizados](index-add-custom-analyzers.md). Para más información sobre cuándo y por qué se analizan los términos de consulta, consulte [Búsqueda de texto completo en Azure Cognitive Search](search-lucene-query-architecture.md).

Para más información sobre el análisis léxico, escuche el siguiente clip de vídeo para obtener una breve explicación.

> [!VIDEO https://www.youtube.com/embed/Y_X6USgvB1g?version=3&start=132&end=189]

## <a name="default-analyzer"></a>Analizador predeterminado  

En Azure Cognitive Search, se invoca automáticamente un analizador en todos los campos de cadena marcados como "searchable". 

De forma predeterminada, Azure Cognitive Search usa el [analizador estándar de Apache Lucene (Lucene estándar)](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html) que divide el texto en elementos siguiendo las reglas de ["Segmentación de texto Unicode"](https://unicode.org/reports/tr29/). Además, el analizador estándar convierte todos los caracteres en minúsculas. Los documentos indexados y lo términos de búsqueda son sometidos a análisis durante la indexación y el procesamiento de consultas.  

Puede invalidar el valor predeterminado campo por campo. Los analizadores alternativos pueden ser un [analizador del lenguaje](index-add-language-analyzers.md) para el procesamiento lingüístico, un [analizador personalizado](index-add-custom-analyzers.md) o un analizador integrado de la [lista de analizadores disponibles](index-add-custom-analyzers.md#built-in-analyzers).

## <a name="types-of-analyzers"></a>Tipos de analizadores

En la lista siguiente se describen los analizadores que están disponibles en Búsqueda cognitiva de Azure.

| Category | Descripción |
|----------|-------------|
| [Analizador Lucene estándar](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Predeterminada. No se requieren ninguna especificación ni configuración. Este analizador de uso general funciona bien para la mayoría de lenguajes y escenarios.|
| Analizadores integrados | Se consume tal cual y se le hace referencia por su nombre. Hay dos tipos: de lenguaje e independientes del lenguaje. </br></br>Los [analizadores especializados (independientes del idioma)](index-add-custom-analyzers.md#built-in-analyzers) se usan cuando las entradas de texto requieren procesamiento especializado o un procesamiento mínimo. Entre los analizadores de esta categoría se incluyen **Asciifolding**, **Keyword**, **Pattern**, **Simple**, **Stop**, y **Whitespace**. </br></br>Los [analizadores de lenguaje](index-add-language-analyzers.md) se utilizan cuando se necesita compatibilidad lingüística enriquecida con lenguajes individuales. Búsqueda cognitiva de Azure admite 35 analizadores de lenguaje de Lucene y 50 analizadores de procesamiento de lenguaje natural de Microsoft. |
|[Analizadores personalizados](/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Hacen referencia a una configuración definida por el usuario de una combinación de los elementos existentes, que consta de un tokenizador (obligatorio) y filtros opcionales (char o token).|

Algunos analizadores integrados, como **Pattern** o **Stop**, admiten un conjunto limitado de opciones de configuración. Para establecer estas opciones, cree un analizador personalizado, que conste del analizador integrado y una de las opciones alternativas documentadas en [Analizadores integrados](index-add-custom-analyzers.md#built-in-analyzers). Como en cualquier configuración personalizada, proporcione a la nueva configuración un nombre, como *myPatternAnalyzer* para distinguirla del analizador de Lucene Pattern.

## <a name="how-to-specify-analyzers"></a>Especificación de analizadores

La configuración de un analizador es opcional. Como norma general, pruebe a usar primero el analizador estándar de Lucene predeterminado para ver cómo funciona. Si las consultas no devuelven los resultados esperados, el cambio a un analizador diferente suele ser la solución adecuada.

1. Al crear una definición de campo en el [índice](/rest/api/searchservice/create-index), establezca la propiedad "analyzer" en uno de los siguientes valores: [analizador integrado](index-add-custom-analyzers.md#built-in-analyzers) como **keyword**, un [analizador de lenguaje](index-add-language-analyzers.md) como `en.microsoft`, o un analizador personalizado (que se defina en el mismo esquema del índice).  
 
   ```json
     "fields": [
    {
      "name": "Description",
      "type": "Edm.String",
      "retrievable": true,
      "searchable": true,
      "analyzer": "en.microsoft",
      "indexAnalyzer": null,
      "searchAnalyzer": null
    },
   ```

   Si va a usar un [analizador de lenguaje](index-add-language-analyzers.md), debe usar la propiedad "analyzer" para especificarlo. Las propiedades "searchAnalyzer" e "indexAnalyzer" no se aplican a los analizadores de lenguaje.

1. También puede establecer "indexAnalyzer" y "searchAnalyzer" para que varíe el analizador de cada carga de trabajo. Estas propiedades se establecen juntas y reemplazan a la propiedad "analyzer", que debe ser null. Si una de esas actividades requiere una transformación específica que otras no necesitan, puede usar diferentes analizadores para la indexación y las consultas.

   ```json
     "fields": [
    {
      "name": "ProductGroup",
      "type": "Edm.String",
      "retrievable": true,
      "searchable": true,
      "analyzer": null,
      "indexAnalyzer": "keyword",
      "searchAnalyzer": "standard"
    },
   ```

1. En el caso de los analizadores personalizados, cree una entrada en la sección **[analizadores]** del índice y, a continuación, asigne el analizador personalizado a la definición de campo mediante cualquiera de los dos pasos anteriores. Para más información, consulte [Create Index](/rest/api/searchservice/create-index) y también [Incorporación de analizadores personalizados](index-add-custom-analyzers.md).

## <a name="when-to-add-analyzers"></a>Cuándo agregar analizadores

El mejor momento para agregar y asignar analizadores es durante el desarrollo activo, cuando quitar y volver a crear índices se convierte en una rutina.

Dado que los analizadores se usan para tokenizar los términos, debe asignar un analizador al crear el campo. De hecho, no se permite asignar una propiedad analyzer ni indexAnalyzer a un campo que ya se ha creado físicamente (aunque se puede cambiar la propiedad searchAnalyzer en cualquier momento sin que afecte al índice).

Para cambiar el analizador de un campo existente, tendrá que quitar y volver a crear el índice entero (no puede volver a generar campos individuales). En el caso de los índices de producción, puede aplazar una nueva generación mediante la creación de un nuevo campo con la nueva asignación del analizador y empezar a usarla en lugar de la antigua. Use [Update Index](/rest/api/searchservice/update-index) para incorporar el nuevo campo y [mergeOrUpload](/rest/api/searchservice/addupdate-or-delete-documents) para rellenarlo. Más adelante, como parte de un mantenimiento planeado del índice, puede limpiarlo para quitar campos obsoletos.

Llame a [Update Index](/rest/api/searchservice/update-index) para agregar un nuevo campo a un índice existente y a [mergeOrUpload](/rest/api/searchservice/addupdate-or-delete-documents) para rellenarlo.

Para agregar un analizador personalizado a un índice existente, pase la marca "allowIndexDowntime" de [Actualizar índice](/rest/api/searchservice/update-index) si quiere evitar este error:

*"No se permite la actualización del índice porque provocaría tiempo de inactividad. Para agregar nuevos analizadores, tokenizadores, filtros de token o filtros de caracteres a un índice existente, establezca el parámetro de consulta "allowIndexDowntime" en "true" en la solicitud de actualización del índice. Tenga en cuenta que esta operación hará que el índice pase a estar sin conexión durante al menos unos segundos, de modo que las solicitudes de indexación y consulta darán error. El rendimiento y la disponibilidad de escritura del índice pueden ser desiguales durante varios minutos después de que se actualice el índice, o durante más tiempo en el caso de índices muy grandes."*

## <a name="recommendations-for-working-with-analyzers"></a>Recomendaciones para trabajar con analizadores

En esta sección se proporcionan consejos para trabajar con los analizadores.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Un analizador para lectura y escritura, salvo que tenga requisitos específicos

Azure Cognitive Search le permite especificar diferentes analizadores para indexación y búsqueda mediante las propiedades de campo adicionales indexAnalyzer y searchAnalyzer. Si no se especifica, el analizador establecido con la propiedad analyzer se usa para la indexación y la búsqueda. Si el analizador no se especifica, se usará el analizador Lucene estándar predeterminado.

Una regla general es usar el mismo analizador de para los índices y las consultas, a menos que los requisitos específicos indiquen lo contrario. Asegúrese de realizar pruebas exhaustivas. Si el procesamiento de texto es diferente en el momento de la búsqueda y la indexación, corre el riesgo de que se produzca una falta de coincidencia entre los términos de la consulta y los términos indexados si las configuraciones del analizador de búsqueda y el de indexación no están alineadas.

### <a name="test-during-active-development"></a>Prueba durante el desarrollo activo

El reemplazo del analizador estándar requiere que se reconstruya el índice. Si es posible, decida qué analizadores se van a usar durante el desarrollo activo antes de poner un índice en producción.

### <a name="inspect-tokenized-terms"></a>Inspección de términos con tokens

Si se produce un error en una búsqueda para devolver los resultados esperados, el escenario más probable es que se creen discrepancias de tokens entre las entradas de término en la consulta y términos con tokens en el índice. Si los tokens no son los mismos, las coincidencias no pueden materializarse. Para inspeccionar la salida del tokenizador recomendamos usar la [API de análisis](/rest/api/searchservice/test-analyzer) como herramienta de investigación. La respuesta consta de los tokens que genera un analizador concreto.

<a name="examples"></a>

## <a name="rest-examples"></a>Ejemplos de REST

Los ejemplos siguientes muestran las definiciones del analizador en algunos escenarios claves.

+ [Ejemplo de analizador personalizado](#Custom-analyzer-example)
+ [Ejemplo de asignación de analizadores a un campo](#Per-field-analyzer-assignment-example)
+ [Mezcla de analizadores para indexación y búsqueda](#Mixing-analyzers-for-indexing-and-search-operations)
+ [Ejemplo de analizador de idioma](#Language-analyzer-example)

<a name="Custom-analyzer-example"></a>

### <a name="custom-analyzer-example"></a>Ejemplo de analizador personalizado

Este ejemplo ilustra una definición del analizador con opciones personalizadas. Las opciones personalizadas de los filtros de char, tokenizadores y filtros de token se especifican por separado como construcciones con nombre y, después, se hace referencia a ellas en la definición del analizador. Los elementos predefinidos se usan tal cual y se hace referencia a ellos por su nombre.

Descripción de este ejemplo:

+ Los analizadores son una propiedad de la clase de campo de un campo que permite la búsqueda.

+ Un analizador personalizado forma parte de una definición de índice. Su personalización puede ser escasa (por ejemplo, la personalización de una sola opción de un filtro) o puede estar personalizado en varios lugares.

+ En este caso, el analizador personalizado es "my_analyzer", que a su vez utiliza un tokenizador estándar personalizado, "my_standard_tokenizer", y dos filtros de token: lowercase y el filtro de asciifolding personalizado "my_asciifolding".

+ También define 2 filtros de char personalizados "map_dash" y "remove_whitespace". La primera de ellas reemplaza todos los guiones por caracteres de subrayado, mientras que la segunda quita todos los espacios. Los espacios tienen que tener codificación UTF-8 en las reglas de asignación. Los filtros de char se aplican antes de la tokenización y afectarán a los tokens resultantes (el tokenizador estándar se interrumpe en guiones y espacios, pero no en un carácter de subrayado).

```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash",
              "remove_whitespace"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        },
        {
           "name":"remove_whitespace",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["\\u0020=>"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizerV2",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
```

<a name="Per-field-analyzer-assignment-example"></a>

### <a name="per-field-analyzer-assignment-example"></a>Ejemplo de asignación de analizador por campo

El analizador estándar es el predeterminado. Imagine que desea reemplazar el analizador predeterminado por otro analizador predefinido, como el analizador de patrón. Si no va a establecer opciones personalizadas, basta con que lo especifique por nombre en la definición del campo.

El elemento "analizador" reemplaza el analizador estándar campo a campo. No se produce un reemplazo global. En este ejemplo, `text1` utiliza el analizador de patrón y `text2`, que no especifica un analizador, usa el predeterminado.

```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
```

<a name="Mixing-analyzers-for-indexing-and-search-operations"></a>

### <a name="mixing-analyzers-for-indexing-and-search-operations"></a>Mezcla de analizadores para las operaciones de indexación y búsqueda

Las API incluyen atributos de índice adicionales para especificar diferentes analizadores para las operaciones de indexación y búsqueda. Los atributos searchAnalyzer y indexAnalyzer se deben especificar como un par, de forma que se reemplaza el atributo único analyzer.


```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
```

<a name="Language-analyzer-example"></a>

### <a name="language-analyzer-example"></a>Ejemplo de analizador de idioma

Los campos que contienen cadenas en diferentes idiomas pueden utilizar un analizador del lenguaje, mientras que otros campos conservan el predeterminado (o usan otro analizador predefinido o personalizado). Si utiliza un analizador del lenguaje, debe hacerlo tanto para las operaciones de indexación como para las de búsqueda. Los campos que usan un analizador del lenguaje no pueden tener un analizador para la indexación y otro para la búsqueda.

```json
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
```

## <a name="c-examples"></a>Ejemplos de C#

Si usa los ejemplos de código del SDK de. NET, puede anexar estos ejemplos para usar o configurar los analizadores.

+ [Asignar un analizador integrado](#Assign-a-language-analyzer)
+ [Configurar un analizador](#Define-a-custom-analyzer)

<a name="Assign-a-language-analyzer"></a>

### <a name="assign-a-language-analyzer"></a>Asignar un analizador de idioma

Cualquier analizador que se usa tal cual, sin ninguna configuración, se especifica en una definición de campo. No es necesario crear una entrada en la sección **[analizadores]** del índice. 

Los analizadores de lenguaje se usan tal cual. Para usarlos, llame a [LexicalAnalyzer](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzer) especificando el tipo [LexicalAnalyzerName](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzername) y proporcionando un analizador de texto compatible con Azure Cognitive Search.

Los analizadores personalizados se especifican de forma similar en la definición de campo, pero, para que funcionen, debe especificar el analizador en la definición del índice, como se describe en la sección siguiente.

```csharp
    public partial class Hotel
    {
       . . . 
        [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.EnLucene)]
        public string Description { get; set; }

        [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.FrLucene)]
        [JsonPropertyName("Description_fr")]
        public string DescriptionFr { get; set; }

        [SearchableField(AnalyzerName = "url-analyze")]
        public string Url { get; set; }
      . . .
    }
```

<a name="Define-a-custom-analyzer"></a>

### <a name="define-a-custom-analyzer"></a>Definir un analizador personalizado

Cuando se necesita personalización o configuración, debe agregar una construcción de analizador a un índice. Una vez definida, puede agregarla a la definición de campo, como se muestra en el ejemplo anterior.

Cree un objeto [CustomAnalyzer](/dotnet/api/azure.search.documents.indexes.models.customanalyzer). Un analizador personalizado es una combinación definida por el usuario de un tokenizador conocido, cero o más filtros de token y cero o más nombres de filtros de caracteres:

+ [CustomAnalyzer.Tokenizer](/dotnet/api/microsoft.azure.search.models.customanalyzer.tokenizer)
+ [CustomAnalyzer.TokenFilters](/dotnet/api/microsoft.azure.search.models.customanalyzer.tokenfilters)
+ [CustomAnalyzer.CharFilters](/dotnet/api/microsoft.azure.search.models.customanalyzer.charfilters)

En el ejemplo siguiente se crea un analizador personalizado denominado "url-analyze" que usa el [tokenizador uax_url_email](/dotnet/api/microsoft.azure.search.models.customanalyzer.tokenizer) y el [filtro de token Lowercase](/dotnet/api/microsoft.azure.search.models.tokenfiltername.lowercase).

```csharp
private static void CreateIndex(string indexName, SearchIndexClient adminClient)
{
   FieldBuilder fieldBuilder = new FieldBuilder();
   var searchFields = fieldBuilder.Build(typeof(Hotel));

   var analyzer = new CustomAnalyzer("url-analyze", "uax_url_email")
   {
         TokenFilters = { TokenFilterName.Lowercase }
   };

   var definition = new SearchIndex(indexName, searchFields);

   definition.Analyzers.Add(analyzer);

   adminClient.CreateOrUpdateIndex(definition);
}
```

Para obtener más ejemplos, vea [CustomAnalyzerTests.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Microsoft.Azure.Search/tests/Tests/CustomAnalyzerTests.cs).

## <a name="next-steps"></a>Pasos siguientes

Puede consultar una descripción detallada de la ejecución de consultas en [Búsqueda de texto completo en Azure Cognitive Search](search-lucene-query-architecture.md). En el artículo se usan ejemplos que explican comportamientos que, a simple vista, podrían parecer contradictorios.

Para más información sobre los analizadores, consulte los siguientes artículos:

+ [Adición de un analizador de idioma](index-add-language-analyzers.md)
+ [Adición de un analizador personalizado](index-add-custom-analyzers.md)
+ [Creación de un índice de búsqueda](search-what-is-an-index.md)
+ [Creación de un índice para varios idiomas](search-language-support.md)