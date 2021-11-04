---
author: eric-urban
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/18/2020
ms.author: eur
ms.custom: devx-track-csharp
ms.openlocfilehash: 8119e4a8fa1d1aaf5a838ea5f3ddd961c4fb4042
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131501098"
---
El contenedor proporciona las API de punto de conexión de consulta basadas en WebSocket, a las que se accede mediante el [SDK de Voz](../index.yml). De forma predeterminada, el SDK de Voz usa servicios de voz en línea. Para usar el contenedor, deberá cambiar el método de inicialización.

> [!TIP]
> Al usar el SDK de Voz con los contenedores, no es necesario proporcionar la [clave de suscripción del recurso de Voz de Azure o un token de portador de autenticación](../rest-speech-to-text.md#authentication).

Consulte los ejemplos más abajo.

# <a name="c"></a>[C#](#tab/csharp)

Cambie de usar esta llamada de inicialización en la nube de Azure:

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

Para usar esta llamada con el [host](/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost) de contenedor:

```csharp
var config = SpeechConfig.FromHost(
    new Uri("ws://localhost:5000"));
```

# <a name="python"></a>[Python](#tab/python)

Cambie de usar esta llamada de inicialización en la nube de Azure:

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

Para usar esta llamada con el [punto de conexión](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig) de contenedor:

```python
speech_config = speechsdk.SpeechConfig(
    endpoint="ws://localhost:5000/speech/recognition/conversation/cognitiveservices/v1"
```

---
