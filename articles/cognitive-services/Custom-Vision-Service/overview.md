---
title: ¿Qué es Custom Vision?
titleSuffix: Azure Cognitive Services
description: Aprenda a usar el servicio Azure Custom Vision para compilar modelos de inteligencia artificial personalizados para detectar objetos o clasificar imágenes.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 08/25/2021
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: image recognition, image identifier, image recognition app, custom vision
ms.openlocfilehash: 6bf2f9de103416bc9dd35216bf85c609be966a23
ms.sourcegitcommit: 901ea2c2e12c5ed009f642ae8021e27d64d6741e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2021
ms.locfileid: "132372518"
---
# <a name="what-is-custom-vision"></a>¿Qué es Custom Vision?

Azure Custom Vision es un servicio de reconocimiento de imágenes que permite compilar, implementar y mejorar sus propios identificadores de imágenes. Un identificador de imágenes aplica etiquetas (que representan clasificaciones u objetos) a las imágenes, en función de sus características visuales detectadas. A diferencia del servicio [Computer Vision](../computer-vision/overview.md), Custom Vision te permite especificar sus propias etiquetas y entrenar modelos personalizados para detectarlas.

Esta documentación contiene los siguientes tipos de artículos:
* Los [inicios rápidos](./getting-started-build-a-classifier.md) son instrucciones paso a paso que permiten realizar llamadas al servicio y obtener los resultados en un breve período de tiempo.
* Las [guías paso a paso](./test-your-model.md) contienen instrucciones para usar el servicio de maneras más específicas o personalizadas.
* Los [tutoriales](./iot-visual-alerts-tutorial.md) son guías más largas que muestran cómo usar este servicio como componente en soluciones empresariales más amplias.
<!--* The [conceptual articles](Vision-API-How-to-Topics/call-read-api.md) provide in-depth explanations of the service's functionality and features.-->

## <a name="what-it-does"></a>Qué hace

El servicio Custom Vision usa un algoritmo de aprendizaje automático para analizar las imágenes. El desarrollador debe enviar los grupos de imágenes que presenten y carezcan de las características en cuestión. Las imágenes las etiqueta el propio usuario en el momento del envío. Luego, el algoritmo se entrena con estos datos y calcula su propia precisión probándose a sí mismo en esas mismas imágenes. Una vez que el algoritmo se ha entrenado, puede probarlo, volver a entrenarlo y, por último, usarlo en la aplicación de reconocimiento de imágenes para [clasificar imágenes](getting-started-build-a-classifier.md). También puede [exportar el mismo modelo](export-your-model.md) para su uso sin conexión.

### <a name="classification-and-object-detection"></a>Clasificación y detección de objetos

La funcionalidad de Custom Vision puede dividirse en dos características. La **[clasificación de las imágenes](getting-started-build-a-classifier.md)** aplica una o varias etiquetas a una imagen. La **[detección de objetos](get-started-build-detector.md)** es similar, pero también devuelve las coordenadas de la imagen donde pueden encontrarse las etiquetas aplicadas.

### <a name="optimization"></a>Optimization

El servicio Custom Vision está optimizado para reconocer rápidamente las diferencias principales entre las imágenes, para que pueda empezar a crear el prototipo de su modelo con una pequeña cantidad de datos. Cincuenta imágenes por etiqueta suele ser un buen comienzo. Sin embargo, el servicio no es óptimo para detectar diferencias sutiles en las imágenes (por ejemplo, detectar grietas menores o abolladuras en escenarios de control de calidad).

Además, puede elegir entre distintas variaciones del algoritmo de Custom Vision que están optimizadas para imágenes con cierto material temático&mdash;, por ejemplo, puntos de referencia o artículos en venta. Para obtener más información, vea [Selección de un dominio](select-domain.md).

## <a name="what-it-includes"></a>Qué incluye

El servicio Custom Vision está disponible como un conjunto de SDK nativos, así como mediante una interfaz basada en web desde el [sitio web de Custom Vision](https://customvision.ai/). Puede crear, probar y entrenar un modelo mediante cualquiera de las interfaces, o usar ambas.

![Sitio web de Custom Vision en una ventana del explorador Chrome](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Seguridad y privacidad de datos

Al igual que sucede con todas las instancias de Cognitive Services, los desarrolladores que usan Custom Vision Service deben estar al tanto de las directivas de Microsoft sobre los datos de clientes. Para más información, consulte la [página de Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) en Microsoft Trust Center.

## <a name="next-steps"></a>Pasos siguientes

Siga la guía [Creación de un clasificador](getting-started-build-a-classifier.md) para empezar a usar Custom Vision en el portal web, o bien complete un [inicio rápido](quickstarts/image-classification.md) para implementar los escenarios básicos en el código.