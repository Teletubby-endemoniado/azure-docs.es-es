---
title: Configuración de la caché y el enriquecimiento incremental (versión preliminar)
titleSuffix: Azure Cognitive Search
description: Habilite el almacenamiento en caché y conserve el estado del contenido enriquecido para el procesamiento controlado en un conjunto de aptitudes cognitivas. Esta característica actualmente está en su versión preliminar pública.
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/06/2021
ms.openlocfilehash: 5ea2c908cce37e19023e27b0e3e4cc76f778b7f0
ms.sourcegitcommit: 1d56a3ff255f1f72c6315a0588422842dbcbe502
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2021
ms.locfileid: "129620172"
---
# <a name="configure-caching-for-incremental-enrichment-in-azure-cognitive-search"></a>Configuración del almacenamiento en caché para el enriquecimiento incremental en Azure Cognitive Search

> [!IMPORTANT] 
> Esta característica se encuentra en versión preliminar pública en los [Términos de uso complementarios](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). La [API REST de versión preliminar](/rest/api/searchservice/index-preview) admite esta característica.

En este artículo se explica cómo agregar almacenamiento en caché a una canalización de enriquecimiento para que pueda modificar incrementalmente los pasos sin tener que volver a compilarlo todo cada vez. De forma predeterminada, un conjunto de aptitudes no tiene estado y el cambio de cualquier parte de su composición requiere una nueva ejecución completa del indexador. Con el enriquecimiento incremental, el indexador puede determinar qué partes del árbol del documento deben actualizarse en función de los cambios detectados en las definiciones del conjunto de aptitudes o el indexador. La salida procesada existente se conserva y se reutiliza siempre que sea posible. 

El contenido almacenado en caché se coloca en Azure Storage con la información de la cuenta proporcionada. El contenedor, denominado `ms-az-search-indexercache-<alpha-numerc-string>`, se crea al ejecutar el indexador. Debe considerarse como un componente interno administrado por el servicio de búsqueda y no debe modificarse.

Si no está familiarizado con la configuración de indexadores, comience con la [información general sobre el indexador](search-indexer-overview.md) y, después, continúe con los [conjuntos de aptitudes](cognitive-search-working-with-skillsets.md) para obtener información sobre las canalizaciones de enriquecimiento. Para obtener más información sobre los conceptos clave, vea el tema sobre el [enriquecimiento incremental](cognitive-search-incremental-indexing-conceptual.md).

## <a name="enable-caching-on-an-existing-indexer"></a>Habilitación del almacenamiento en caché en un indexador existente

Si tiene un indexador existente que ya tiene un conjunto de aptitudes, siga los pasos de esta sección para agregar el almacenamiento en caché. Como operación única, tendrá que restablecer y volver a ejecutar el indexador en su totalidad para que el procesamiento incremental pueda surtir efecto.

### <a name="step-1-get-the-indexer-definition"></a>Paso 1: Obtención de la definición del indexador

Comience con un indexador válido y existente que tenga estos componentes: origen de datos, conjunto de aptitudes, índice. El indexador debe ser ejecutable.

Con un cliente de API, construya una [solicitud GET Indexer](/rest/api/searchservice/get-indexer) para obtener la configuración actual del indexador. Cuando se usa la versión preliminar de API para realizar una solicitud GET Indexer, se agrega a las definiciones una propiedad `cache` establecida en NULL.

```http
GET https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]?api-version=2020-06-30-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
```

Copie la definición del indexador de la respuesta.

### <a name="step-2-modify-the-cache-property-in-the-indexer-definition"></a>Paso 2: Modificación de la propiedad de caché en la definición del indexador

De manera predeterminada, la propiedad `cache` es nula. Use un cliente de API para establecer la configuración de caché (el portal no admite esta actualización en particular). 

Modifique el objeto de caché para incluir las siguientes propiedades obligatorias y opcionales: 

+ La propiedad `storageConnectionString` es obligatoria y debe establecerse en una cadena de conexión de Azure Storage. 
+ La propiedad booleana `enableReprocessing` es opcional (`true` de forma predeterminada) e indica que el enriquecimiento incremental está habilitado. Cuando sea necesario, puede establecerla en `false` para suspender el procesamiento incremental mientras se están llevando a cabo otras operaciones con un uso intensivo de recursos, como la indexación de nuevos documentos, y, luego, volver a cambiarla a `true` más adelante.

```json
{
    "name": "<YOUR-INDEXER-NAME>",
    "targetIndexName": "<YOUR-INDEX-NAME>",
    "dataSourceName": "<YOUR-DATASOURCE-NAME>",
    "skillsetName": "<YOUR-SKILLSET-NAME>",
    "cache" : {
        "storageConnectionString" : "<YOUR-STORAGE-ACCOUNT-CONNECTION-STRING>",
        "enableReprocessing": true
    },
    "fieldMappings" : [],
    "outputFieldMappings": [],
    "parameters": []
}
```

> [!NOTE]
> La caché del indexador requiere una cuenta de almacenamiento de uso general v2. Para más información, consulte los [diferentes tipos de cuentas de almacenamiento](../storage/common/storage-account-overview.md#types-of-storage-accounts).

### <a name="step-3-reset-the-indexer"></a>Paso 3: Restablecimiento del indizador

Se requiere un restablecimiento del indizador al configurar el enriquecimiento incremental de los indizadores existentes para asegurarse de que todos los documentos se encuentran en un estado coherente. Puede usar el portal o un cliente de API y la [API REST para restablecer el indexador](/rest/api/searchservice/reset-indexer) para esta tarea.

```http
POST https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]/reset?api-version=2020-06-30-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
```

### <a name="step-4-save-the-updated-definition"></a>Paso 4: Almacenamiento de la definición actualizada

[Actualice el indexador](/rest/api/searchservice/preview-api/update-indexer) con una solicitud PUT; el cuerpo de la solicitud debe contener la definición de indexador actualizada que tiene la propiedad de caché. Si aparece un mensaje de tipo 400, compruebe la definición del indexador para asegurarse de que se cumplen todos los requisitos (origen de datos, conjunto de aptitudes, índice).

```http
PUT https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]?api-version=2020-06-30-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
{
    "name" : "<YOUR-INDEXER-NAME>",
    ...
    "cache": {
        "storageConnectionString": "<YOUR-STORAGE-ACCOUNT-CONNECTION-STRING>",
        "enableReprocessing": true
    }
}
```

Si ahora emite otra solicitud GET en el indexador, la respuesta del servicio incluirá una propiedad `ID` en el objeto de caché. La cadena alfanumérica se anexa al nombre del contenedor que incluirá todos los resultados almacenados en caché y el estado intermedio de cada documento procesado por este indexador. El identificador se usará para asignar el nombre único a la memoria caché en Blob Storage.

```http
    "cache": {
        "ID": "<ALPHA-NUMERIC STRING>",
        "enableReprocessing": true,
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<YOUR-STORAGE-ACCOUNT>;AccountKey=<YOUR-STORAGE-KEY>;EndpointSuffix=core.windows.net"
    }
```

### <a name="step-5-run-the-indexer"></a>Paso 5: Ejecución del indexador

Para ejecutar el indexador, puede usar el portal o la API. En la lista de indexadores del portal, seleccione el indexador y haga clic en **Ejecutar**. Una ventaja de usar el portal es que puede supervisar el estado del indexador y anotar la duración del trabajo y el número de documentos que se procesan. Las páginas del portal se actualizan cada pocos minutos.

Como alternativa, puede usar REST para [ejecutar el indexador](/rest/api/searchservice/run-indexer):

```http
POST https://[YOUR-SEARCH-SERVICE].search.windows.net/indexers/[YOUR-INDEXER-NAME]/run?api-version=2020-06-30-Preview
Content-Type: application/json
api-key: [YOUR-ADMIN-KEY]
```

Una vez que se ejecuta el indexador, puede encontrar la caché en Azure Blob Storage. El nombre del contenedor adopta el siguiente formato: `ms-az-search-indexercache-<YOUR-CACHE-ID>`

> [!NOTE]
> Restablecer y volver a ejecutar el indexador produce una recompilación completa para que se pueda almacenar en caché el contenido. Todos los enriquecimientos cognitivos se volverán a ejecutar en todos los documentos.
>

### <a name="step-6-modify-a-skillset-and-confirm-incremental-enrichment"></a>Paso 6: Modificación de un conjunto de aptitudes y confirmación del enriquecimiento incremental

Para modificar un conjunto de aptitudes, puede usar el portal o la API. Por ejemplo, si utiliza la traducción de texto, un simple cambio en línea de `en` a `es` u otro idioma es suficiente para la prueba de concepto del enriquecimiento incremental.

Ejecute de nuevo el indexador. Solo se actualizan esas partes de un árbol de documentos enriquecido. Si ha usado el [inicio rápido del portal](cognitive-search-quickstart-blob.md) como prueba de concepto, con la modificación de la habilidad de traducción de texto a "es" observará que solo se actualizan 8 documentos en lugar de los 14 originales. Los archivos de imagen no afectados por el proceso de traducción se reutilizan de la memoria caché.

## <a name="enable-caching-on-new-indexers"></a>Habilitación del almacenamiento en caché en nuevos indexadores

Para configurar el enriquecimiento incremental de un nuevo indexador, lo único que tiene que hacer es incluir la propiedad `cache` en la carga de definición del indexador al llamar a [Crear indexador (2020-06-30-Preview)](/rest/api/searchservice/preview-api/create-indexer). 


```json
{
    "name": "<YOUR-INDEXER-NAME>",
    "targetIndexName": "<YOUR-INDEX-NAME>",
    "dataSourceName": "<YOUR-DATASOURCE-NAME>",
    "skillsetName": "<YOUR-SKILLSET-NAME>",
    "cache" : {
        "storageConnectionString" : "<YOUR-STORAGE-ACCOUNT-CONNECTION-STRING>",
        "enableReprocessing": true
    },
    "fieldMappings" : [],
    "outputFieldMappings": [],
    "parameters": []
    }
}
```

## <a name="checking-for-cached-output"></a>Comprobación de la salida almacenada en caché

El indexador crea, utiliza y administra la memoria caché, y su contenido no se representa en un formato legible para el usuario. La mejor manera de determinar si se usa la memoria caché es mediante la ejecución del indexador y la comparación de las métricas anteriores y posteriores para el tiempo de ejecución y los recuentos de documentos. 

Pongamos por ejemplo un conjunto de aptitudes que se inicia con el análisis de imágenes y el reconocimiento óptico de caracteres (OCR) de los documentos digitalizados, seguido del análisis de nivel inferior del texto resultante. Si modifica una habilidad de texto de nivel inferior, el indexador puede recuperar la imagen procesada previamente y el contenido de OCR de la memoria caché, actualizando y procesando solo los cambios relacionados con el texto indicados por las ediciones. Normalmente, verá menos documentos en el recuento de documentos (por ejemplo, 8/8 en lugar de 14/14 en la ejecución original), tiempos de ejecución más cortos y menos cargos en la factura.

## <a name="working-with-the-cache"></a>Trabajo con la memoria caché

Una vez que la memoria caché está operativa, los indexadores comprueban la memoria caché cada vez que se llame a la tarea de [ejecución del indexador](/rest/api/searchservice/run-indexer) para ver qué partes de la salida existente se pueden usar. 

En la tabla siguiente se resume el modo en que varias API se relacionan con la memoria caché:

| API           | Impacto en la memoria caché     |
|---------------|------------------|
| [Crear indexador (2020-06-30-Preview)](/rest/api/searchservice/preview-api/create-indexer) | Crea y ejecuta un indexador en el primer uso, incluida la creación de una caché si la definición del indexador lo especifica. |
| [Ejecutar indexador](/rest/api/searchservice/run-indexer) | Ejecuta una canalización de enriquecimiento a petición. Esta API lee de la memoria caché si existe o crea una memoria caché si agregó el almacenamiento en caché a una definición de indexador actualizada. Al ejecutar un indexador que tiene habilitado el almacenamiento en caché, el indexador omite los pasos si se puede usar la salida almacenada en caché. Puede usar la versión preliminar de esta API o la que está disponible con carácter general.|
| [Restablecer indexador](/rest/api/searchservice/reset-indexer)| Borra el indexador de cualquier información de indexación incremental. La siguiente ejecución del indexador (ya sea a petición o programada) es un reprocesamiento completo desde cero, lo que incluye volver a ejecutar todas las aptitudes y volver a generar la memoria caché. Es funcionalmente equivalente a eliminar el indexador y volver a crearlo. Puede usar la versión preliminar de esta API o la que está disponible con carácter general.|
| [Restablecer aptitudes](/rest/api/searchservice/preview-api/reset-skills) | Especifica las aptitudes que se deben volver a ejecutar en la siguiente ejecución del indexador, aunque no se hayan modificado las aptitudes. La memoria caché se actualiza en consecuencia. Las salidas, como un almacén de información o un índice de búsqueda, se actualizan con datos reutilizables de la memoria caché más el nuevo contenido de acuerdo con las aptitudes actualizadas. |

Para obtener más información sobre cómo controlar lo que ocurre en la memoria caché, vea la información sobre la [administración de la memoria caché](cognitive-search-incremental-indexing-conceptual.md#cache-management).

## <a name="next-steps"></a>Pasos siguientes

El enriquecimiento incremental es aplicable en los indexadores que contienen aptitudes. Como paso siguiente, visite la documentación del conjunto de aptitudes para comprender los conceptos y la composición. 

Además, una vez habilitada la memoria caché, querrá conocer los parámetros y las API que se incluyen en el almacenamiento en caché, incluido cómo invalidar y forzar comportamientos concretos. Para más información, consulte los vínculos siguientes:

+ [Conceptos de conjunto de aptitudes y composición](cognitive-search-working-with-skillsets.md)
+ [Creación de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Introducción al enriquecimiento incremental y al almacenamiento en caché](cognitive-search-incremental-indexing-conceptual.md)