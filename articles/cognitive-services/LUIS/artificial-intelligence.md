---
title: Inteligencia artificial
description: LUIS usa la inteligencia artificial (IA) para proporcionar comprensión del lenguaje sobre los datos, en función del esquema que se haya definido.
ms.topic: conceptual
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 08/07/2020
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: 623b50bee72f99be4d31159165f1af5b98f67b88
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124750745"
---
# <a name="artificial-intelligence-in-language-understanding-luis"></a>La inteligencia artificial en la comprensión del lenguaje (LUIS)

LUIS usa la inteligencia artificial (IA) para proporcionar comprensión del lenguaje natural (NLU) sobre los datos, en función del esquema que se haya definido.

## <a name="natural-language-processing-nlp"></a>Procesamiento del lenguaje natural (NLP)

La comprensión del lenguaje natural (NLU) es un subtema específico del procesamiento del lenguaje natural (NLP).

El procesamiento del lenguaje natural es un concepto más amplio que abarca cualquier forma de procesamiento de datos de texto, lo que incluye lo siguiente:

* Tokenización
* Etiquetado de categorías gramaticales (POS)
* Segmentación
* Análisis morfológico
* Similitud semántica
* Discourse
* Traducción

## <a name="natural-language-processing-in-luis"></a>Procesamiento del lenguaje natural en LUIS

El procesamiento del lenguaje natural está disponible para la aplicación de LUIS de las siguientes maneras:
* [Comprensión del lenguaje natural](#natural-language-processing-nlp) (LUIS)
* Aspectos de NLP configurables en LUIS:
    * [Tokenización](luis-language-support.md#tokenization)
    * [Configuración de la API](luis-reference-application-settings.md) de morfología a través de signos diacríticos, puntuación y formas de palabras
* Procesamiento previo o posterior de la expresión de consulta proporcionado por otros servicios de [Cognitive Services](../what-are-cognitive-services.md), como:
    * [Traducción](../translator/translator-overview.md)

## <a name="natural-language-understanding-nlu"></a>Comprensión del lenguaje natural (NLU)

NLU es la capacidad de _transformar_ una instrucción lingüística en una representación que permite comprender los usuarios de forma natural. La comprensión del lenguaje natural sigue siendo todo un desafío y se considera un problema _complicado para la IA_.

LUIS está diseñado para centrarse en la intención y la extracción, lo que incluye la capacidad de identificar:
* Lo que quiere el usuario.
* El tema del que habla.

LUIS tiene pocos conocimientos o ninguno sobre los aspectos más amplios de _NLP_, como la similitud semántica, sin una identificación explícita a través de ejemplos. Por ejemplo, los siguientes tokens (palabras) son tres cosas diferentes hasta que se usan en contextos similares en los ejemplos proporcionados:

* comprar
* comprando
* comprado

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre el [ciclo de vida de desarrollo](luis-concept-app-iteration.md) de una aplicación de LUIS.