---
author: eric-urban
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: eur
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: a16d6056a7070acb6ff7954a624d4fb7f4b67278
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131507652"
---
## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?pivots=programming-language-python" target="_blank">Instale el SDK de Voz para su entorno de desarrollo y cree un proyecto de ejemplo vacío</a>.

## <a name="create-a-luis-app-for-intent-recognition"></a>Creación de una aplicación de LUIS para el reconocimiento de la intención

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project"></a>Apertura del proyecto

1. Abra el entorno de desarrollo integrado que prefiera.
2. Cree un proyecto y un archivo llamado `quickstart.py`y, a continuación, ábralo.

## <a name="start-with-some-boilerplate-code"></a>Inicio con código reutilizable

Vamos a agregar código que funcione como el esqueleto del proyecto.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-7)]

## <a name="create-a-speech-configuration"></a>Creación de una configuración de Voz

Para poder inicializar un objeto `IntentRecognizer`, es preciso crear una configuración que use la clave y ubicación del recurso de predicción de LUIS.

Inserte este código en `quickstart.py`. Asegúrese de actualizar estos valores:

* Reemplace `"YourLanguageUnderstandingSubscriptionKey"` por la clave de predicción de LUIS.
* Reemplace `"YourLanguageUnderstandingServiceRegion"` por la ubicación de LUIS. Use **Identificador de región** en [Región](../../../../regions.md).

>[!TIP]
> Si necesita ayuda para encontrar estos valores, consulte [Creación de una aplicación de LUIS para el reconocimiento de la intención](#create-a-luis-app-for-intent-recognition).

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=12)]

En este ejemplo se crea el objeto `SpeechConfig` mediante la clave y la región de LUIS. Para ver una lista completa de los métodos disponibles, consulte [Clase SpeechConfig](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig).

El SDK de Voz se usará de forma predeterminada para reconocer el uso de en-us como idioma. Para más información sobre cómo elegir el idioma de origen, consulte [Especificación del idioma de origen para la conversión de voz a texto](../../../../how-to-specify-source-language.md).

## <a name="initialize-an-intentrecognizer"></a>Inicialización de IntentRecognizer

Ahora, vamos a crear un objeto `IntentRecognizer`. Inserte este código justo debajo de la configuración de Voz.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=15)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Adición de un objeto LanguageUnderstandingModel e intenciones

Debe asociar un objeto `LanguageUnderstandingModel` con el reconocedor de intenciones y agregar las intenciones que desee que se reconozcan. Vamos a usar las intenciones del dominio precompilado para la automatización doméstica.

Inserte este código debajo de `IntentRecognizer`. Asegúrese de reemplazar `"YourLanguageUnderstandingAppId"` por el identificador de la aplicación de LUIS. 

>[!TIP]
> Si necesita ayuda para encontrar este valor, consulte [Creación de una aplicación de LUIS para el reconocimiento de la intención](#create-a-luis-app-for-intent-recognition).

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=19-27)]

En este ejemplo se usa la función `add_intents()` para agregar una lista de intenciones definidas explícitamente. Si desea agregar todas las intenciones de un modelo, use `add_all_intents(model)` y pase el modelo.

> [!NOTE]
> El SDK de Voz solo admite puntos de conexión de LUIS v2.0.
> Debe modificar manualmente la dirección URL del punto de conexión v3.0 que se encuentra en el campo de consulta de ejemplo para usar un patrón de dirección URL v2.0.
> Los puntos de conexión de LUIS v2.0 siempre siguen uno de estos dos patrones:
> * `https://{AzureResourceName}.cognitiveservices.azure.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`
> * `https://{Region}.api.cognitive.microsoft.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`

## <a name="recognize-an-intent"></a>Reconocimiento de una intención

En el objeto `IntentRecognizer`, va a llamar al método `recognize_once()`. Este método permite que el servicio Voz sepa que solo va a enviar una frase para el reconocimiento y que, una vez que se identifica la frase, se detendrá el reconocimiento de voz.

Inserte este código debajo del modelo.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=35)]

## <a name="display-the-recognition-results-or-errors"></a>Visualización de los resultados (o errores) del reconocimiento

Cuando el servicio Voz devuelva el resultado del reconocimiento, querrá hacer algo con él. Vamos a hacer algo tan sencillo como imprimir el resultado en la consola.

Debajo de la llamada a `recognize_once()`, agregue este código.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=38-47)]

## <a name="check-your-code"></a>Comprobación del código

En este momento, el código debe tener esta apariencia.

> [!NOTE]
> Se han agregado algunos comentarios a esta versión.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-47)]

## <a name="build-and-run-your-app"></a>Compilación y ejecución de la aplicación

Ejecute el ejemplo desde la consola o en el IDE:

```
python quickstart.py
```

Los próximos 15 segundos de la entrada de voz del micrófono se reconocen y se registran en la ventana de consola.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]
