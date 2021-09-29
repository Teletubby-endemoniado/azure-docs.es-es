---
title: Comparación de Azure Video Analyzer for Media (anteriormente Video Indexer) y los valores preestablecidos de Azure Media Services v3
description: En este artículo se comparan las funcionalidades de Azure Video Analyzer for Media (anteriormente Video Indexer) y los valores preestablecidos de Azure Media Services v3.
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: juliako
ms.openlocfilehash: 66e62013c4719bf0b20808a240bdfa44483ab5b1
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128607622"
---
# <a name="compare-azure-media-services-v3-presets-and-video-analyzer-for-media"></a>Comparación de los valores preestablecidos de Azure Media Services v3 y Video Analyzer for Media 

En este artículo se comparan las funcionalidades de las **API de Video Analyzer for Media (anteriormente Video Indexer)** y las **API de Media Services v3**. 

Actualmente, hay una superposición entre las características que ofrecen las [API de Video Analyzer for Media](https://api-portal.videoindexer.ai/) y las [API de Media Services v3](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). En la tabla siguiente se ofrecen las instrucciones actuales para entender las similitudes y diferencias. 

## <a name="compare"></a>Comparación

|Característica|API de Video Analyzer for Media |Valores predeterminados del analizador de vídeo y el analizador de audio<br/>en Azure Media Services v3|
|---|---|---|
|Información de medios sociales|[Mejorado](video-indexer-output-json-v2.md). |[Aspectos básicos](../../media-services/latest/analyze-video-audio-files-concept.md)|
|Experiencias|Consulte la lista completa de características admitidas: <br/> [Información general](video-indexer-overview.md)|Devuelve solo información de vídeo.|
|Facturación|[Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Cumplimiento normativo|Para conocer las actualizaciones en materia de cumplimiento más actuales, consulte [Azure Compliance Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) y busque "Video Analyzer for media" para ver si cumple con un certificado de interés.|Para conocer las actualizaciones en materia de cumplimiento más actuales, visite [Azure Compliance Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) y busque "Media Services" para ver si cumple con un certificado de interés.|
|Versión de prueba gratuita|Este de EE. UU.|No disponible|
|Disponibilidad en regiones|Consulte la [disponibilidad de Cognitive Services por región](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services).|Consulte la [disponibilidad de Media Services por región](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Pasos siguientes

[Información general de Video Analyzer for Media](video-indexer-overview.md)

[Introducción a Media Services v3](../../media-services/latest/media-services-overview.md)
