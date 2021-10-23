---
title: Soporte de localización con Microsoft Azure Maps
description: Vea qué regiones admite Azure Maps con servicios como los de mapas, búsqueda, enrutamiento, meteorología e incidentes de tráfico. Obtenga información sobre cómo configurar el parámetro View.
author: stevemunk
ms.author: v-munksteve
ms.date: 12/07/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
ms.openlocfilehash: 7a48190d58097d1cbbdf1223c25916ee6f6ca0e3
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "130001568"
---
# <a name="localization-support-in-azure-maps"></a>Soporte de localización en Azure Maps

Azure Maps admite varios idiomas y vistas según el país o región. En este artículo se proporcionan los idiomas y las vistas compatibles para que pueda realizar su implementación de Azure Maps.

## <a name="azure-maps-supported-languages"></a>Idiomas admitidos en Azure Maps

Los servicios de Azure Maps se han localizado en una variedad de idiomas. En la tabla siguiente se proporcionan los códigos de idioma admitidos para cada servicio.  
  
| ID         | Nombre                   |  Mapas | Search | Enrutamiento | Tiempo | Incidentes de tráfico | Control de mapa JS |
|------------|------------------------|:-----:|:------:|:-------:|:--------:|:-----------------:|:--------------:|
| af-ZA      | Afrikáans              |       |   ✓   |    ✓    |          |                   |                |
| ar-SA      | Árabe                 |   ✓   |   ✓   |    ✓    |   ✓     |         ✓         |        ✓      |
| bn-BD      | Bengalí (Bangladesh)    |       |       |         |     ✓    |                   |                |
| bn-IN      | Bengalí (India)         |       |       |         |     ✓    |                   |                |
| bs-BA      | Bosnio                |       |       |         |     ✓    |                   |                |
| eu-ES      | Vasco                 |       |   ✓   |         |          |                   |                |
| bg-BG      | Búlgaro              |   ✓   |   ✓   |    ✓   |     ✓    |                   |        ✓       |
| ca-ES      | Catalán                |       |    ✓  |         |    ✓     |                   |                |
| zh-HanS    | Chino (simplificado)   |       |  zh-CN |        |   zh-CN   |                   |                |
| zh-HanT    | Chino (RAE de Hong Kong)|       |        |        |   zh-HK   |                   |                |
| zh-HanT    | Chino (Taiwán)       | zh-TW |  zh-TW |  zh-TW |   zh-TW   |                   |      zh-TW     |
| hr-HR      | Croata               |       |    ✓   |        |    ✓     |                   |                |
| cs-CZ      | Checo                  |   ✓   |    ✓   |    ✓  |    ✓     |         ✓         |        ✓       |
| da-DK      | Danés                 |   ✓   |    ✓   |    ✓   |    ✓    |        ✓          |        ✓       |
| nl-BE      | Neerlandés (Bélgica)        |       |    ✓   |         |     ✓   |                   |                |
| nl-NL      | Neerlandés (Países Bajos)    |   ✓   |    ✓   |    ✓   |     ✓    |        ✓         |        ✓       |
| en-AU      | Inglés (Australia)    |   ✓   |    ✓   |    ✓   |     ✓    |       ✓          |        ✓       |
| en-NZ      | Inglés (Nueva Zelanda)  |   ✓   |    ✓   |    ✓   |     ✓    |       ✓          |        ✓       |
| en-GB      | Inglés (Gran Bretaña)|   ✓   |    ✓   |    ✓   |     ✓    |       ✓          |        ✓       |
| es-ES      | Inglés (EE. UU.)          |   ✓   |    ✓   |    ✓   |      ✓   |       ✓          |        ✓       |
| et-EE      | Estonio               |       |    ✓   |        |      ✓    |        ✓         |                |
| fil-PH     | Filipino               |       |         |        |     ✓    |                   |                |
| fi-FI      | Finés                |   ✓   |    ✓   |    ✓   |      ✓   |         ✓         |        ✓       |
| fr-FR      | Francés                 |   ✓   |    ✓   |    ✓   |      ✓   |         ✓         |        ✓       |
| fr-CA      | Francés (Canadá)        |       |    ✓   |         |     ✓    |                   |                |
| gl-ES      | Gallego               |       |    ✓   |         |          |                   |                |
| de-DE      | Alemán                 |   ✓   |    ✓   |    ✓   |   ✓      |         ✓         |        ✓       |
| el-GR      | Griego                  |   ✓   |    ✓   |    ✓   |    ✓     |         ✓         |        ✓       |
| gu-IN      | Gujarati               |       |         |        |     ✓    |                   |                |
| he-IL      | Hebreo                 |       |    ✓   |         |     ✓    |         ✓         |                |
| hi-IN      | Hindi                  |       |        |         |     ✓    |                   |                |
| hu-HU      | Húngaro              |   ✓   |    ✓  |    ✓    |     ✓    |         ✓         |        ✓       |
| is-IS      | Islandés              |       |       |          |     ✓    |                   |                |
| id-ID      | Indonesio             |   ✓   |   ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| it-IT      | Italiano                |   ✓   |   ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| ja-JP      | Japonés               |       |        |         |     ✓    |                   |                |
| kn-IN      | Canarés                |       |        |         |     ✓    |                   |                |
| kk-KZ      | Kazajo                 |       |    ✓   |         |     ✓    |                   |                |
| ko-KR      | Coreano                 |   ✓   |        |    ✓    |     ✓    |                   |        ✓       |
| es-419     | Español (América latina) |       |    ✓   |         |           |                   |                |
| lv-LV      | Letón                |       |    ✓   |         |     ✓    |         ✓         |                |
| lt-LT      | Lituano             |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓      |
| mk-MK      | Macedonio             |       |         |         |     ✓    |                   |                |
| ms-MY      | Malayo (latino)          |   ✓   |    ✓   |    ✓    |    ✓     |                   |        ✓       |
| mr-IN      | Maratí                |       |         |         |     ✓    |                   |                |
| nb-NO      | Noruego bokmal       |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| NGT        | Neutral Ground Truth, idiomas oficiales para todas las regiones en alfabetos locales si están disponibles |   ✓     |        |         |       |        |      ✓          |
| NGT-Latn   | Neutral Ground Truth, exónimos latinos. Si está disponible, se usará el alfabeto latino |   ✓     |        |         |         |                |        ✓         |
| pl-PL      | Polaco                 |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| pt-BR      | Portugués (Brasil)    |   ✓   |    ✓   |    ✓    |      ✓   |                   |        ✓       |
| pt-PT      | Portugués (Portugal)  |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| pa-IN      | Punjabi                |       |         |         |     ✓    |                   |                |
| ro-RO      | Rumano               |       |    ✓    |         |     ✓    |         ✓         |                |
| ru-RU      | Ruso                |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| sr-Cyrl-RS | Serbio (cirílico)     |       |  sr-RS  |         |  sr-RS    |                   |                |
| sr-Latn-RS | Serbio (latino)        |       |         |         |  sr-latn  |                   |                |
| sk-SK      | Eslovaco                 |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| sl-SI      | Esloveno              |   ✓   |    ✓   |    ✓    |     ✓    |                   |        ✓       |
| es-ES      | Español                |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| es-MX      | Español (México)       |   ✓   |        |    ✓    |     ✓    |                   |        ✓       |
| sv-SE      | Sueco                |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| ta-IN      | Tamil (India)          |       |        |          |     ✓    |                    |                |
| te-IN      | Telugu (India)         |       |        |          |     ✓    |                    |                |
| th-TH      | Tailandés                   |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| tr-TR      | Turco                |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| uk-UA      | Ucraniano              |       |    ✓   |          |     ✓    |                   |                |
| ur-PK      | Urdu                   |       |        |          |     ✓    |                   |                |
| uz-Latn-UZ | Uzbeko                  |       |        |          |     ✓    |                   |                |
| vi-VN      | Vietnamita             |       |    ✓   |          |     ✓    |                   |                |

## <a name="azure-maps-supported-views"></a>Vistas admitidas en Azure Maps

> [!Note]
> El 1 de agosto de 2019 se lanzó Azure Maps en los países o regiones siguientes:
>
> * Argentina
> * India
> * Marruecos
> * Pakistán
>
> Después del 1 de agosto de 2019, el parámetro **Vista** definirá el contenido del mapa devuelto para las nuevas regiones o países enumerados anteriormente. El parámetro **Vista** de Azure Maps (también denominado "parámetro de región de usuario") es un código de país ISO-3166 de dos letras que mostrará los mapas correctos para ese país o región, especificando qué conjunto de contenido con conflictos geopolíticos se devuelve a través de los servicios de Azure Maps, incluidas las fronteras y las etiquetas que se muestran en el mapa. 

Asegúrese de configurar el parámetro **Vista** según lo necesario para las API de REST y los SDK que sus servicios usan.
  
### <a name="rest-apis"></a>API Rest
  
Asegúrese de haber configurado el parámetro Vista tal como se ha especificado. El parámetro Vista especifica qué conjunto de contenido con conflictos geopolíticos se devuelve a través de los servicios de Azure Maps. 

Servicios REST de Azure Maps afectados:

* Obtener icono del mapa
* Obtener imagen de mapa
* Obtener búsqueda aproximada
* Obtener búsqueda de POI
* Obtener búsqueda de POI por categoría
* Obtener búsqueda cercana
* Obtener dirección de búsqueda
* Obtener la dirección de búsqueda estructurada
* Obtener la dirección de búsqueda inversa
* Obtener la dirección de búsqueda inversa entre calles
* Búsqueda de POST dentro de geometría
* Búsqueda de POST del lote de direcciones
* Búsqueda de POST del lote de direcciones inversas
* Búsqueda de POST a lo largo de la ruta
* Búsqueda de POST aproximada del lote

### <a name="sdks"></a>SDK

Asegúrese de haber configurado el parámetro **Vista** según sea necesario y de tener la última versión de los SDK web y de Android. SDK afectados:

* SDK web de Azure Maps
* Android SDK para Azure Maps

De manera predeterminada, el parámetro Vista está establecido en **Unificado**, aunque no lo haya definido en la solicitud. Determine la ubicación de los usuarios. A continuación, establezca el parámetro **Vista** correctamente para esa ubicación. También tiene la opción de establecer "View=Auto", lo que devolverá los datos del mapa según la dirección IP de la solicitud.  El parámetro **Vista** de Azure Maps se debe usar de conformidad con las leyes aplicables, incluidas aquellas relacionadas con la asignación del país o región donde están disponibles los mapas, las imágenes y otros datos y contenido de terceros a los que está autorizado a acceder a través de Azure Maps.

La siguiente tabla proporciona vistas compatibles.

| Ver         | Descripción                            |  Mapas | Search | Control de mapa JS |
|--------------|----------------------------------------|:-----:|:------:|:--------------:|
| AE           | Emiratos Árabes Unidos (vista árabe)     |   ✓   |        |     ✓          |
| AR           | Argentina (vista argentina)           |   ✓   |    ✓   |     ✓          |
| BH           | Baréin (vista árabe)                  |   ✓   |        |     ✓          |
| IN           | India (vista india)                    |   ✓   |   ✓    |     ✓          |
| IQ           | Irak (vista árabe)                     |   ✓   |        |     ✓          |
| JO           | Jordania (vista árabe)                   |   ✓   |        |     ✓          |
| KW           | Kuwait (vista árabe)                   |   ✓   |        |     ✓          |
| LB           | Líbano (vista árabe)                  |   ✓   |        |     ✓          |
| MA           | Marruecos (vista marroquí)                |   ✓   |   ✓    |     ✓         |
| OM           | Omán (vista árabe)                     |   ✓   |        |     ✓          |
| PK           | Pakistán (vista pakistaní)              |   ✓   |    ✓   |     ✓         |
| PS           | Autoridad Nacional Palestina (vista árabe)    |   ✓   |        |     ✓          |
| QA           | Catar (vista árabe)                    |   ✓   |        |     ✓          |
| SA           | Arabia Saudí (vista árabe)             |   ✓   |        |     ✓          |
| SY           | Siria (vista árabe)                    |   ✓   |        |     ✓          |
| YE           | Yemen (vista árabe)                    |   ✓   |        |     ✓          |
| Automático         | Devuelva los datos del mapa según la dirección IP de la solicitud.|   ✓   |    ✓   |     ✓          |
| Unificado      | Vista unificada (otros)                  |   ✓   |   ✓     |     ✓          |
