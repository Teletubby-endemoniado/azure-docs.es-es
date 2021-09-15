---
title: 'Idiomas admitidos: Text Analytics API'
titleSuffix: Azure Cognitive Services
description: Una lista de los idiomas naturales admitidos por Text Analytics API. En este artículo se explica qué idiomas se admiten para cada operación.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/06/2021
ms.author: aahi
ms.openlocfilehash: 917a2206d59ef145beb2c5355002827069b86e92
ms.sourcegitcommit: 7854045df93e28949e79765a638ec86f83d28ebc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122867314"
---
# <a name="text-analytics-api-v3-language-support"></a>Idiomas admitidos en Text Analytics API v3 

#### <a name="sentiment-analysis"></a>[Análisis de sentimiento](#tab/sentiment-analysis)

> [!NOTE]
> Los idiomas se agregan a medida que se lanzan nuevas [versiones del modelo](concepts/model-versioning.md) para características concretas de Text Analytics. La versión del modelo actual para Análisis de sentimiento es `2020-04-01`.

| Idioma              | Código de lenguaje | Compatibilidad con v3.x | A partir de la versión del modelo v3: |              Notas |
|:----------------------|:-------------:|:----------:|:--------------------------:|-------------------:|
| Chino simplificado    |   `zh-hans`   |     ✓      |         2019-10-01         | También se acepta `zh` |
| Chino (tradicional)   |   `zh-hant`   |    ✓      |         2019-10-01         |                    |
| Neerlandés                 |     `nl`      |     ✓      |         2019-10-01        |                    |
| Inglés               |     `en`      |     ✓      |         2019-10-01         |                    |
| Francés                |     `fr`      |     ✓      |         2019-10-01         |                    |
| Alemán                |     `de`      |     ✓      |         2019-10-01         |                    |
| Hindi                 |    `hi`       |     ✓      |         2020-04-01         |                    |
| Italiano               |     `it`      |     ✓      |         2019-10-01         |                    |
| Japonés              |     `ja`      |     ✓      |         2019-10-01         |                    |
| Coreano                |     `ko`      |    ✓      |         2019-10-01         |                    |
| Noruego (Bokmål)   |     `no`      |     ✓      |         2020-04-01         |                    |
| Portugués (Brasil)   |    `pt-BR`    |     ✓      |         2020-04-01         |                    |
| Portugués (Portugal) |    `pt-PT`    |     ✓      |         2019-10-01         | También se acepta `pt` |
| Español               |     `es`      |     ✓      |         2019-10-01         |                    |
| Turco               |     `tr`      |     ✓       |         2020-04-01        |                    |

### <a name="opinion-mining-v31-only"></a>Minería de opiniones (solo v3.1)

| Idioma              | Código de lenguaje | A partir de la versión del modelo v3: |              Notas |
|:----------------------|:-------------:|:------------------------------------:|-------------------:|
| Inglés               |     `en`      |              2020-04-01              |                    |


#### <a name="named-entity-recognition-ner"></a>[Reconocimiento de entidades con nombre (NER)](#tab/named-entity-recognition)

> [!NOTE]
> * Solo se devuelven las entidades "Persona", "Ubicación" y "Organización" para los idiomas marcados con *.
> * Los idiomas se agregan a medida que se lanzan nuevas [versiones del modelo](concepts/model-versioning.md) para características concretas de Text Analytics. La versión del modelo actual para NER es `2021-06-01`.

| Idioma               | Código de lenguaje | Compatibilidad con v3.x | A partir de la versión del modelo de la versión 3: |       Notas        |
|:-----------------------|:-------------:|:----------:|:-------------------------------:|:------------------:|
| Árabe                 |     `ar`      |      ✓*    |               2019-10-01        |                    |
| Chino simplificado     |   `zh-hans`   |     ✓      |               15-01-2021        | También se acepta `zh` |
| Chino (tradicional)   |   `zh-hant`   |     ✓*      |               2019-10-01        |                    |
| Checo                 |     `cs`      |     ✓*      |               2019-10-01        |                    |
| Danés                |     `da`      |     ✓*      |               2019-10-01        |                    |
| Neerlandés                 |     `nl`      |     ✓*      |               2019-10-01        |                    |
| Inglés                |     `en`      |     ✓      |               2019-10-01        |                    |
| Finés               |     `fi`      |     ✓*      |               2019-10-01        |                    |
| Francés                 |     `fr`      |     ✓      |               15-01-2021        |                    |
| Alemán                 |     `de`      |     ✓      |               15-01-2021        |                    |
| Hebreo                |     `he`      |     ✓*      |               2019-10-01        |                    |
| Húngaro             |     `hu`      |     ✓*      |               2019-10-01        |                    |
| Italiano               |     `it`      |     ✓       |               15-01-2021        |                    |
| Japonés              |     `ja`      |     ✓       |               15-01-2021        |                    |
| Coreano                |     `ko`      |     ✓       |               15-01-2021        |                    |
| Noruego (Bokmål)   |     `no`      |     ✓*      |               2019-10-01        | También se acepta `nb` |
| Polaco                |     `pl`      |     ✓*      |               2019-10-01        |                    |
| Portugués (Brasil)   |    `pt-BR`    |     ✓       |               15-01-2021        |                    |
| Portugués (Portugal) |    `pt-PT`    |     ✓       |               15-01-2021        | También se acepta `pt` |
| Ruso              |     `ru`      |     ✓*       |               2019-10-01        |                    |
| Español               |     `es`      |     ✓       |               2020-04-01        |                    |
| Sueco               |     `sv`      |     ✓*      |               2019-10-01        |                    |
| Turco               |     `tr`      |     ✓*      |               2019-10-01        |                    |

#### <a name="key-phrase-extraction"></a>[Extracción de frases clave](#tab/key-phrase-extraction)

> [!NOTE]
> Los idiomas se agregan a medida que se lanzan nuevas [versiones del modelo](concepts/model-versioning.md) para características concretas de Text Analytics. La versión del modelo actual para Extracción de frases clave es `2021-06-01`.

| Idioma              | Código de lenguaje |  Compatibilidad con v3.x | A partir de la versión del modelo de la versión 3: |       Notas        |
|:----------------------|:-------------:|:----------:|:-----------------------------------------:|:------------------:|
| Afrikáans             |     `af`      |     ✓      |                01-07-2020                 |                    |
| Búlgaro             |     `bg`      |     ✓      |                01-07-2020                 |                    |
| Catalán               |     `ca`      |     ✓      |                01-07-2020                 |                    |
| Chino simplificado    |     `zh-hans` |     ✓      |                01-06-2021                 |                    |
| Croata              |     `hr`      |     ✓      |                01-07-2020                 |                    |
| Danés                |     `da`      |     ✓      |                2019-10-01                 |                    |
| Neerlandés                 |     `nl`      |     ✓      |                2019-10-01                 |                    |
| Inglés               |     `en`      |     ✓      |                2019-10-01                 |                    |
| Estonio              |     `et`      |     ✓      |                01-07-2020                 |                    |
| Finés               |     `fi`      |     ✓      |                2019-10-01                 |                    |
| Francés                |     `fr`      |     ✓      |                2019-10-01                 |                    |
| Alemán                |     `de`      |     ✓      |                2019-10-01                 |                    |
| Griego                 |     `el`      |     ✓      |                01-07-2020                 |                    |
| Húngaro             |     `hu`      |     ✓      |                01-07-2020                 |                    |
| Italiano               |     `it`      |     ✓      |                2019-10-01                 |                    |
| Indonesio            |     `id`      |     ✓      |                01-07-2020                 |                    |
| Japonés              |     `ja`      |     ✓      |                2019-10-01                 |                    |
| Coreano                |     `ko`      |     ✓      |                2019-10-01                 |                    |
| Letón               |     `lv`      |     ✓      |                01-07-2020                 |                    |
| Noruego (Bokmål)   |     `no`      |     ✓      |                01-07-2020                 | También se acepta `nb` |
| Polaco                |     `pl`      |    ✓      |                2019-10-01                 |                    |
| Portugués (Brasil)   |    `pt-BR`    |     ✓      |                2019-10-01                 |                    |
| Portugués (Portugal) |    `pt-PT`    |    ✓      |                2019-10-01                 | También se acepta `pt` |
| Rumano              |     `ro`      |     ✓      |                01-07-2020                 |                    |
| Ruso               |     `ru`      |     ✓      |                2019-10-01                 |                    |
| Español               |     `es`      |     ✓      |                2019-10-01                 |                    |
| Eslovaco                |     `sk`      |     ✓      |                01-07-2020                 |                    |
| Esloveno             |     `sl`      |     ✓      |                01-07-2020                 |                    |
| Sueco               |     `sv`      |     ✓      |                2019-10-01                 |                    |
| Turco               |     `tr`      |     ✓      |                01-07-2020                 |                    |

#### <a name="entity-linking"></a>[Entity Linking](#tab/entity-linking)

> [!NOTE]
> Los idiomas se agregan a medida que se lanzan nuevas [versiones del modelo](concepts/model-versioning.md) para características concretas de Text Analytics. La versión del modelo actual para la vinculación de entidad es `2020-02-01`.

| Idioma | Código de lenguaje |  Compatibilidad con v3.x | A partir de la versión del modelo de la versión 3: | Notas |
|:---------|:-------------:|:----------:|:-----------------------------------------:|:-----:|
| Inglés  |     `en`      |     ✓      |                2019-10-01                 |       |
| Español  |     `es`      |    ✓      |                2019-10-01                 |       |

#### <a name="text-analytics-for-health"></a>[Text Analytics for Health](#tab/health)

> [!NOTE]
> * El contenedor usa versiones de modelo diferentes que el SDK y los puntos de conexión de la API.
> * Los idiomas se agregan a medida que se lanzan nuevas versiones del modelo para características concretas de Text Analytics. Las [versiones del modelo](concepts/model-versioning.md) actuales de Text Analytics para el estado son:
>    * API y SDK: `2021-05-15`
>    * Contenedor: `2021-03-01`


| Idioma | Código de lenguaje |  Compatibilidad con v3.x | A partir de la versión del modelo de la versión 3: | Notas |
|:---------|:-------------:|:----------:|:-----------------------------------------:|:-----:|
| Inglés  |     `en`      |     ✓      |                Punto de conexión de API: 2020-11-01 <br> Contenedor: 2020-04-16                |       |

#### <a name="personally-identifiable-information-pii"></a>[Información de identificación personal](#tab/pii)

> [!NOTE]
> Los idiomas se agregan a medida que se lanzan nuevas [versiones del modelo](concepts/model-versioning.md) para características concretas de Text Analytics. La versión del modelo actual para PII es `2021-01-15`.

| Idioma               | Código de lenguaje | Compatibilidad con v3.x | A partir de la versión del modelo de la versión 3: |       Notas        |
|:-----------------------|:-------------:|:----------:|:-------------------------------:|:------------------:|
| Chino simplificado     |   `zh-hans`   |     ✓      |               15-01-2021        | También se acepta `zh` |
| Inglés                |     `en`      |     ✓      |               01-07-2020        |                    |
| Francés                 |     `fr`      |     ✓      |               15-01-2021        |                    |
| Alemán                 |     `de`      |     ✓      |               15-01-2021        |                    |
| Italiano               |     `it`      |     ✓       |               15-01-2021        |                    |
| Japonés              |     `ja`      |     ✓       |               15-01-2021        |                    |
| Coreano                |     `ko`      |     ✓       |               15-01-2021        |                    |
| Portugués (Brasil)   |    `pt-BR`    |     ✓       |               15-01-2021        |                    |
| Portugués (Portugal) |    `pt-PT`    |     ✓       |               15-01-2021        | También se acepta `pt` |
| Español               |     `es`      |     ✓       |               2020-04-01        |                    |

#### <a name="language-detection"></a>[Detección de idioma](#tab/language-detection)

> [!NOTE]
> Los idiomas se agregan a medida que se lanzan nuevas [versiones del modelo](concepts/model-versioning.md) para características concretas de Text Analytics. La versión del modelo actual para Detección de idioma es `2021-01-05`.

Text Analytics API puede detectar una amplia variedad de idiomas, variantes, dialectos y algunos idiomas regionales o culturales, y puede devolver idiomas con su nombre y código. Los parámetros de código de idioma de Detección de idioma de Text Analytics se ajustan al estándar [BCP-47](https://tools.ietf.org/html/bcp47) y la mayoría de ellos cumplen con los identificadores [ISO-639-1](https://www.iso.org/iso-639-language-codes.html). 

Si tiene contenido que se expresa en un idioma que se usa con menos frecuencia, puede probar Detección de idioma para ver si devuelve un código. La respuesta para los idiomas que no se pueden detectar es `unknown`.

| Idioma | Código de lenguaje | Compatibilidad con v3.x | A partir de la versión del modelo de la versión 3: |
|:-|:-:|:-:|:-:|
|Afrikáans|`af`|✓|    |
|Albanés|`sq`|✓|    |
|Amárico|`am`|✓|05-01-2021|
|Árabe|`ar`|✓|    |
|Armenio|`hy`|✓|    |
|Asamés|`as`|✓|05-01-2021|
|Azerbaiyano|`az`|✓|05-01-2021|
|Vasco|`eu`|✓|    |
|Bielorruso|`be`|✓|    |
|Bengalí|`bn`|✓|    |
|Bosnio|`bs`|✓|01-09-2020|
|Búlgaro|`bg`|✓|    |
|Birmano|`my`|✓|    |
|Catalán|`ca`|✓|    |
|Camboyano|`km`|✓|    |
|Chino|`zh`|✓|    |
|Chino simplificado|`zh_chs`|✓|    |
|Chino tradicional|`zh_cht`|✓|    |
|Corso|`co`|✓|05-01-2021|
|Croata|`hr`|✓|    |
|Checo|`cs`|✓|    |
|Danés|`da`|✓|    |
|Dari|`prs`|✓|01-09-2020|
|Divehi|`dv`|✓|    |
|Neerlandés|`nl`|✓|    |
|Inglés|`en`|✓|    |
|Esperanto|`eo`|✓|    |
|Estonio|`et`|✓|    |
|Fiyiano|`fj`|✓|01-09-2020|
|Finés|`fi`|✓|    |
|Francés|`fr`|✓|    |
|Gallego|`gl`|✓|    |
|Georgiano|`ka`|✓|    |
|Alemán|`de`|✓|    |
|Griego|`el`|✓|    |
|Gujarati|`gu`|✓|    |
|Haitiano|`ht`|✓|    |
|Hausa|`ha`|✓|05-01-2021|
|Hebreo|`he`|✓|    |
|Hindi|`hi`|✓|    |
|Hmong Daw|`mww`|✓|01-09-2020|
|Húngaro|`hu`|✓|    |
|Islandés|`is`|✓|    |
|Igbo|`ig`|✓|05-01-2021|
|Indonesio|`id`|✓|    |
|Inuktitut|`iu`|✓|    |
|Irlandés|`ga`|✓|    |
|Italiano|`it`|✓|    |
|Japonés|`ja`|✓|    |
|Javanés|`jv`|✓|05-01-2021|
|Canarés|`kn`|✓|    |
|Kazajo|`kk`|✓|01-09-2020|
|Kinyarwanda|`rw`|✓|05-01-2021|
|Kirguís|`ky`|✓|05-01-2021|
|Coreano|`ko`|✓|    |
|Kurdo|`ku`|✓|    |
|Lao|`lo`|✓|    |
|Latín|`la`|✓|    |
|Letón|`lv`|✓|    |
|Lituano|`lt`|✓|    |
|Luxemburgués|`lb`|✓|05-01-2021|
|Macedonio|`mk`|✓|    |
|Malgache|`mg`|✓|01-09-2020|
|Malayo|`ms`|✓|    |
|Malayalam|`ml`|✓|    |
|Maltés|`mt`|✓|    |
|Maori|`mi`|✓|01-09-2020|
|Maratí|`mr`|✓|01-09-2020|
|Mongol|`mn`|✓|05-01-2021|
|Nepalí|`ne`|✓|05-01-2021|
|Noruego|`no`|✓|    |
|Noruego nynorsk|`nn`|✓|    |
|Odia|`or`|✓|    |
|Pastún|`ps`|✓|    |
|Persa|`fa`|✓|    |
|Polaco|`pl`|✓|    |
|Portugués|`pt`|✓|    |
|Punjabi|`pa`|✓|    |
|Otomí Querétaro|`otq`|✓|01-09-2020|
|Rumano|`ro`|✓|    |
|Ruso|`ru`|✓|    |
|Samoano|`sm`|✓|01-09-2020|
|Serbio|`sr`|✓|    |
|Shona|`sn`|✓|05-01-2021|
|Sindhi|`sd`|✓|05-01-2021|
|Cingalés|`si`|✓|    |
|Eslovaco|`sk`|✓|    |
|Esloveno|`sl`|✓|    |
|Somalí|`so`|✓|    |
|Español|`es`|✓|    |
|Sundanés|`su`|✓|05-01-2021|
|Swahili|`sw`|✓|    |
|Sueco|`sv`|✓|    |
|Tagalo|`tl`|✓|    |
|Tahitiano|`ty`|✓|01-09-2020|
|Tayiko|`tg`|✓|05-01-2021|
|Tamil|`ta`|✓|    |
|Tatar|`tt`|✓|05-01-2021|
|Telugu|`te`|✓|    |
|Tailandés|`th`|✓|    |
|Tibetano|`bo`|✓|05-01-2021|
|Tigriña|`ti`|✓|05-01-2021|
|Tongano|`to`|✓|01-09-2020|
|Turco|`tr`|✓|05-01-2021|
|Turcomano|`tk`|✓|05-01-2021|
|Ucraniano|`uk`|✓||
|Urdu|`ur`|✓||
|Uzbeko|`uz`|✓||
|Vietnamita|`vi`|✓||
|Galés|`cy`|✓|| 
|Xhosa|`xh`|✓|05-01-2021|
|Yidis|`yi`|✓||
|Yoruba|`yo`|✓|05-01-2021|
|Maya Yucateco| `yua` | ✓| |
|Zulú|`zu`|✓|05-01-2021|


#### <a name="text-summarization"></a>[Resumen de texto](#tab/summarization)

| Idioma | Código de lenguaje |  Compatibilidad con v3.x | A partir de la versión del modelo de la versión 3: | Notas |
|:---------|:-------------:|:----------:|:-----------------------------------------:|:-----:|
| Chino simplificado     |   `zh-hans`   |     ✓      |               2021-08-01        | También se acepta `zh` |
| Inglés  |     `en`      |     ✓      |                2021-08-01                 |       |
| Francés                 |     `fr`      |     ✓      |               2021-08-01        |                    |
| Alemán                 |     `de`      |     ✓      |               2021-08-01        |                    |
| Italiano               |     `it`      |     ✓       |               2021-08-01        |                    |
| Japonés              |     `ja`      |     ✓       |               2021-08-01        |                    |
| Coreano                |     `ko`      |     ✓       |               2021-08-01        |                    |
| Español               |     `es`      |     ✓       |               2021-08-01        |                    |
| Portugués (Brasil)   |    `pt-BR`    |     ✓       |               2021-08-01        |                    |
| Portugués (Portugal) |    `pt-PT`    |     ✓       |               2021-08-01        | También se acepta `pt` |

---

## <a name="see-also"></a>Consulte también

* [¿Qué es Text Analytics API?](overview.md)   
* [Versiones del modelo](concepts/model-versioning.md)
