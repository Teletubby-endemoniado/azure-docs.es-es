---
author: eric-urban
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: eur
ms.openlocfilehash: 55cb81958e91a6c17e548eb02161154abba8aa66
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131507461"
---
En este inicio rápido, usará el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) y el servicio Language Understanding (LUIS) para reconocer los intentos de los datos de audio capturados de un micrófono. En concreto, usará el SDK de Voz para capturar la voz y un dominio predefinido de LUIS para identificar las intenciones de automatización doméstica, como encender y apagar una luz. 

Tras cumplir algunos requisitos previos, no es preciso seguir muchos pasos para el reconocimiento de voz y la identificación de intenciones a través de un micrófono:

> [!div class="checklist"]
>
> * Cree un objeto `SpeechConfig` a partir de la clave y la región de suscripción.
> * Cree un objeto `IntentRecognizer` con el objeto `SpeechConfig` anterior.
> * Con el objeto `IntentRecognizer`, inicie el proceso de reconocimiento de una única expresión.
> * Inspeccione el objeto `IntentRecognitionResult` devuelto.

