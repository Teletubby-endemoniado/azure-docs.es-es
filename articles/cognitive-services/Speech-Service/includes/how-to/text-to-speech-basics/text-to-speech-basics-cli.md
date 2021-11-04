---
author: eric-urban
ms.service: cognitive-services
ms.topic: include
ms.date: 08/11/2020
ms.author: eur
ms.openlocfilehash: 0bc81935e597198ed1c65a732ae1feaee7aa6226
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131510793"
---
En este inicio rápido aprenderá patrones de diseño comunes para realizar la síntesis de texto a voz mediante el SDK de voz. Para empezar, puede realizar una configuración y síntesis básicas y, después, pasar a ejemplos más avanzados para el desarrollo de aplicaciones personalizadas, entre las que se incluyen:

* Obtención de respuestas como flujos de datos en memoria
* Personalización de la frecuencia de muestreo y la velocidad de bits de salida
* Envío de solicitudes de síntesis mediante SSML (lenguaje de marcado de síntesis de voz)
* Uso de voces neuronales

## <a name="prerequisites"></a>Prerrequisitos

En este artículo se da por sentado que tiene una cuenta de Azure y una suscripción al servicio de voz. Si no dispone de una cuenta y una suscripción, [pruebe el servicio de voz de forma gratuita](../../../overview.md#try-the-speech-service-for-free).

[!INCLUDE [SPX Setup](../../spx-setup.md)]

## <a name="synthesize-speech-to-a-speaker"></a>Síntesis de voz en un altavoz

Ahora está listo para ejecutar la CLI de Voz para sintetizar voz a partir de texto. Desde la línea de comandos, cambie al directorio que contiene el archivo binario de la CLI de Voz. Luego, ejecute el siguiente comando.

```bash
spx synthesize --text "The speech synthesizer greets you!"
```

La CLI de Voz creará un lenguaje natural en inglés a través del altavoz del equipo.

## <a name="synthesize-speech-to-a-file"></a>Síntesis de voz en un archivo

Ejecute el siguiente comando para cambiar la salida del altavoz a un archivo `.wav`.

```bash
spx synthesize --text "The speech synthesizer greets you!" --audio output greetings.wav
```

La CLI de Voz generará lenguaje natural en inglés en el archivo de audio `greetings.wav`.
En Windows, puede reproducir el archivo de audio escribiendo `start greetings.wav`.
