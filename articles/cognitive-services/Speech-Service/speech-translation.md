---
title: 'Introducción a la traducción de voz: servicio Voz'
titleSuffix: Azure Cognitive Services
description: La traducción de voz le permite incorporar en sus aplicaciones, herramientas y dispositivos una traducción de voz completa, en varios idiomas y en tiempo real. La misma API puede usarse para la traducción de voz a voz y de voz a texto. En este artículo encontrará información general sobre las ventajas y las funcionalidades del servicio de traducción de voz.
services: cognitive-services
author: eric-urban
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: eur
ms.custom: devx-track-csharp, cog-serv-seo-aug-2020
keywords: traducción de voz
ms.openlocfilehash: 3c71682028f1fb54b55e9faddde5928883f44916
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132485573"
---
# <a name="what-is-speech-translation"></a>¿Qué es la traducción de voz?

En esta introducción, descubrirá las ventajas y funcionalidades del servicio de traducción de voz, un servicio que le permite agregar a las secuencias de audio traducciones de voz a voz y [de voz a texto en varios idiomas](language-support.md#speech-translation) y en tiempo real. Con el SDK de voz, sus aplicaciones, herramientas y los dispositivos tienen acceso a las transcripciones de origen y a las salidas de traducción del audio proporcionadas. A medida que se detecta la voz, se van devolviendo resultados provisionales de transcripción y traducción. Asimismo, los resultados finales pueden convertirse en voz sintetizada.

Esta documentación contiene los siguientes tipos de artículos:

* Los **inicios rápidos** son instrucciones de inicio que le guiarán a la hora de hacer solicitudes al servicio.
* Las **guías de procedimientos** contienen instrucciones para usar el servicio de una manera más específica o personalizada.
* Los **conceptos** proporcionan explicaciones detalladas sobre la funcionalidad y las características del servicio.
* Los **tutoriales** son guías más largas que muestran cómo usar el servicio como componente de soluciones empresariales más amplias.

## <a name="core-features"></a>Características principales

* Traducción de voz a texto con resultados de reconocimiento.
* Traducción de voz a voz.
* Compatibilidad para traducir a varios idiomas de destino.
* Resultados de reconocimiento y traducción provisionales.

## <a name="get-started"></a>Introducción 

Consulte la [guía de inicio rápido](get-started-speech-translation.md) para empezar a usar la traducción de voz. El servicio de traducción de voz está disponible con el [SDK de Voz](speech-sdk.md) y la [CLI de Voz](spx-overview.md).

## <a name="sample-code"></a>Código de ejemplo

Hay un ejemplo de código para el SDK de voz disponible en GitHub. En estos ejemplos se tratan escenarios comunes como la lectura de audio de un archivo o flujo, el reconocimiento o la traducción continuos y al inicio, y el trabajo con modelos personalizados.

* [Ejemplos de conversión de voz a texto y de traducción (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Guías de migración

Si sus aplicaciones, herramientas o productos usan [Translator Speech API](./how-to-migrate-from-translator-speech-api.md), hemos creado guías que le ayudarán a migrar al servicio de voz.

* [Migración de Translator Speech API al servicio de voz](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Documentos de referencia

* [Acerca del SDK de Voz](./speech-sdk.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [API REST: Speech-to-text](rest-speech-to-text.md) (API de REST: Voz a texto)
* [API REST: Text-to-speech](rest-text-to-speech.md) (API de REST: Texto a voz)
* [API REST: Batch transcription and customization](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) (API de REST: Transcripción y personalización de Azure Batch)

## <a name="next-steps"></a>Pasos siguientes

* Complete la [guía de inicio rápido](get-started-speech-translation.md) de traducción de voz
* [Obtenga una clave de suscripción gratuita a los servicios de Voz](overview.md#try-the-speech-service-for-free)
* [Obtención del SDK de voz](speech-sdk.md)
