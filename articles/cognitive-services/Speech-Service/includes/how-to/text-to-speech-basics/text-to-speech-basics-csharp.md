---
author: PatrickFarley
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2020
ms.author: pafarley
ms.custom: devx-track-csharp
ms.openlocfilehash: 41ba1bb39b4cf92b84ccc6ff0365cb6735a65bcc
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2021
ms.locfileid: "130143771"
---
En este inicio rápido aprenderá patrones de diseño comunes para realizar la síntesis de texto a voz mediante el SDK de voz. Para empezar, puede realizar una configuración y síntesis básicas y, después, pasar a ejemplos más avanzados para el desarrollo de aplicaciones personalizadas, entre las que se incluyen:

* Obtención de respuestas como flujos de datos en memoria
* Personalización de la frecuencia de muestreo y la velocidad de bits de salida
* Envío de solicitudes de síntesis mediante SSML (lenguaje de marcado de síntesis de voz)
* Uso de voces neuronales

## <a name="skip-to-samples-on-github"></a>Pasar a los ejemplos en GitHub

Si quiere pasar directamente al código de ejemplo, consulte los [ejemplos del inicio rápido de C#](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/dotnet/text-to-speech) en GitHub.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por sentado que tiene una cuenta de Azure y una suscripción al servicio de voz. Si no dispone de una cuenta y una suscripción, [pruebe el servicio de voz de forma gratuita](../../../overview.md#try-the-speech-service-for-free).

## <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

En primer lugar, deberá instalar Speech SDK. Utilice las siguientes instrucciones en función de la plataforma:

* <a href="/azure/cognitive-services/speech-service/quickstarts/setup-platform?pivots=programming-language-csharp&tabs=dotnet" target="_blank">.NET Framework </a>
* <a href="/azure/cognitive-services/speech-service/quickstarts/setup-platform?pivots=programming-language-csharp&tabs=dotnetcore" target="_blank">.NET Core </a>
* <a href="/azure/cognitive-services/speech-service/quickstarts/setup-platform?pivots=programming-language-csharp&tabs=unity" target="_blank">Unity </a>
* <a href="/azure/cognitive-services/speech-service/quickstarts/setup-platform?pivots=programming-language-csharp&tabs=uwps" target="_blank">UWP </a>
* <a href="/azure/cognitive-services/speech-service/quickstarts/setup-platform?pivots=programming-language-csharp&tabs=xaml" target="_blank">Xamarin </a>

## <a name="import-dependencies"></a>Dependencias de importación

Para ejecutar los ejemplos de este artículo, incluya las siguientes instrucciones de `using` al principio del script.

```csharp
using System;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
```

## <a name="create-a-speech-configuration"></a>Creación de una configuración de voz

Para llamar al servicio de voz con Speech SDK, debe crear un elemento [`SpeechConfig`](/dotnet/api/microsoft.cognitiveservices.speech.speechconfig). Esta clase incluye información sobre la suscripción, como la clave de voz y la región o ubicación asociada, el punto de conexión, el host, o el token de autorización.

> [!NOTE]
> Debe crear siempre una configuración, independientemente de si va a realizar reconocimiento de voz, síntesis de voz, traducción o reconocimiento de intenciones.

Existen diversas maneras para inicializar un elemento [`SpeechConfig`](/dotnet/api/microsoft.cognitiveservices.speech.speechconfig):

* Con una suscripción: pase una clave y la región o ubicación asociada.
* Con un punto de conexión: pase un punto de conexión del servicio de voz. La clave y el token de autorización son opcionales.
* Con un host: pase una dirección de host. La clave y el token de autorización son opcionales.
* Con un token de autorización: pase el token de autorización y la región o ubicación asociadas.

En este ejemplo, se crea un elemento [`SpeechConfig`](/dotnet/api/microsoft.cognitiveservices.speech.speechconfig) mediante una clave de voz y una región o ubicación. Para obtener estas credenciales, siga los pasos descritos en [Prueba gratuita del servicio Voz](../../../overview.md#try-the-speech-service-for-free). Cree también código reutilizable básico para usarlo en el resto del artículo y que modificará cuando realice distintas personalizaciones.

```csharp
public class Program
{
    static async Task Main()
    {
        await SynthesizeAudioAsync();
    }

    static async Task SynthesizeAudioAsync()
    {
        var config = SpeechConfig.FromSubscription("<paste-your-speech-key-here>", "<paste-your-speech-location/region-here>");
    }
}
```

## <a name="select-synthesis-language-and-voice"></a>Selección del idioma y la voz de síntesis

El servicio Text to Speech de Azure admite más de 250 voces y más de 70 idiomas y variantes.
Puede obtener la [lista completa](../../../language-support.md#neural-voices) o probarlos en la [demostración de texto a voz](https://azure.microsoft.com/services/cognitive-services/text-to-speech/#features).
Especifique el idioma o la voz de [`SpeechConfig`](/dotnet/api/microsoft.cognitiveservices.speech.speechconfig) para que coincida con el texto de entrada y use la voz que prefiera.

```csharp
static async Task SynthesizeAudioAsync()
{
    var config = SpeechConfig.FromSubscription("<paste-your-speech-key-here>", "<paste-your-speech-location/region-here>");
    // Note: if only language is set, the default voice of that language is chosen.
    config.SpeechSynthesisLanguage = "<your-synthesis-language>"; // e.g. "de-DE"
    // The voice setting will overwrite language setting.
    // The voice setting will not overwrite the voice element in input SSML.
    config.SpeechSynthesisVoiceName = "<your-wanted-voice>";
}
```

## <a name="synthesize-speech-to-a-file"></a>Síntesis de voz en un archivo

A continuación, creará un objeto [`SpeechSynthesizer`](/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesizer), que ejecuta conversiones de texto a voz y envía la salida a altavoces, archivos u otros flujos de salida. [`SpeechSynthesizer`](/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesizer) acepta como parámetros tanto el objeto [`SpeechConfig`](/dotnet/api/microsoft.cognitiveservices.speech.speechconfig) que se creó creado en el paso anterior y un objeto [`AudioConfig`](/dotnet/api/microsoft.cognitiveservices.speech.audio.audioconfig) que especifica cómo se deben controlar los resultados de la salida.

Para empezar, cree un objeto `AudioConfig` para escribir automáticamente la salida en un archivo `.wav`, para lo que se usa la función `FromWavFileOutput()`, y cree las instancias necesarias con una instrucción `using`. En este contexto, una instrucción `using` desecha automáticamente los recursos no administrados y hace que el objeto deje de estar dentro del ámbito después de la eliminación.

```csharp
static async Task SynthesizeAudioAsync()
{
    var config = SpeechConfig.FromSubscription("<paste-your-speech-key-here>", "<paste-your-speech-location/region-here>");
    using var audioConfig = AudioConfig.FromWavFileOutput("path/to/write/file.wav");
}
```

A continuación, cree una instancia de `SpeechSynthesizer` con otra instrucción `using`. Use los objetos `config` y `audioConfig` como parámetros. Después, para ejecutar la síntesis de voz y realizar la escritura en un archivo solo hay que ejecutar `SpeakTextAsync()` con una cadena de texto.

```csharp
static async Task SynthesizeAudioAsync()
{
    var config = SpeechConfig.FromSubscription("<paste-your-speech-key-here>", "<paste-your-speech-location/region-here>");
    using var audioConfig = AudioConfig.FromWavFileOutput("path/to/write/file.wav");
    using var synthesizer = new SpeechSynthesizer(config, audioConfig);
    await synthesizer.SpeakTextAsync("A simple test to write to a file.");
}
```

Ejecute el programa y se escribirá un archivo `.wav` sintetizado en la ubicación que haya especificado. Este es un buen ejemplo del uso más básico, pero a continuación puede examinar cómo personalizar la salida y controlar la respuesta de la salida como una secuencia en memoria para trabajar con escenarios personalizados.

## <a name="synthesize-to-speaker-output"></a>Síntesis a la salida de altavoz

En algunos casos, puede que desee enviar la voz sintetizada directamente a un altavoz. Para ello, omita el parámetro `AudioConfig` al crear `SpeechSynthesizer` en el ejemplo anterior. De esta forma, se sintetiza en el dispositivo de salida activo actual.

```csharp
static async Task SynthesizeAudioAsync()
{
    var config = SpeechConfig.FromSubscription("<paste-your-speech-key-here>", "<paste-your-speech-location/region-here>");
    using var synthesizer = new SpeechSynthesizer(config);
    await synthesizer.SpeakTextAsync("Synthesizing directly to speaker output.");
}
```

## <a name="get-result-as-an-in-memory-stream"></a>Obtención del resultado en forma de secuencia en memoria

Para muchos de los escenarios del desarrollo de aplicaciones de voz, es probable que necesite los datos de audio resultantes en forma de secuencia en memoria, en lugar de que se escriban directamente en un archivo. Esto le permitirá crear un comportamiento personalizado, lo que incluye:

* Abstraer la matriz de bytes resultante como una secuencia que permita la búsqueda para los servicios de bajada personalizados.
* Integrar el resultado con otros servicios o API.
* Modificar los datos de audio, escribir encabezados `.wav` personalizados, etc.

Es sencillo hacer este cambio en el ejemplo anterior. En primer lugar, quite el bloqueo de `AudioConfig`, porque a partir de este momento administrará el comportamiento de la salida de forma manual, ya que así obtiene mayor control. Después, use `null` en `AudioConfig` en el constructor `SpeechSynthesizer`.

> [!NOTE]
> Al usar `null` en `AudioConfig`, en lugar de omitirlo como en el ejemplo de salida del altavoz anterior, el audio no se reproducirá de forma predeterminada en el dispositivo de salida activo actual.

Esta vez se guarda el resultado en una variable [`SpeechSynthesisResult`](/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesisresult). La propiedad `AudioData` contiene un `byte []` de los datos de salida. Puede trabajar con `byte []` manualmente, o bien puede usar la clase [`AudioDataStream`](/dotnet/api/microsoft.cognitiveservices.speech.audiodatastream) para administrar la secuencia en memoria. En este ejemplo, se usa la función estática `AudioDataStream.FromResult()` para obtener una secuencia del resultado.

```csharp
static async Task SynthesizeAudioAsync()
{
    var config = SpeechConfig.FromSubscription("<paste-your-speech-key-here>", "<paste-your-speech-location/region-here>");
    using var synthesizer = new SpeechSynthesizer(config, null);

    var result = await synthesizer.SpeakTextAsync("Getting the response as an in-memory stream.");
    using var stream = AudioDataStream.FromResult(result);
}
```

A partir de aquí se puede implementar cualquier comportamiento personalizado mediante el objeto `stream` resultante.

## <a name="customize-audio-format"></a>Personalización del formato de audio

En la siguiente sección se muestra cómo personalizar los atributos de la salida de audio, entre los que se incluyen:

* Tipo de archivo de audio
* Frecuencia de muestreo
* Profundidad de bits

Para cambiar el formato de audio se usa la función `SetSpeechSynthesisOutputFormat()` en el objeto `SpeechConfig`. Esta función espera un elemento `enum` del tipo [`SpeechSynthesisOutputFormat`](/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesisoutputformat), que se usa para seleccionar el formato de salida. En los documentos de referencia encontrará una [lista de los formatos de audio](/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesisoutputformat) disponibles.

Hay varias opciones para los distintos tipos de archivo, por lo que puede elegir la que necesite. Tenga en cuenta que, por definición, los formatos sin procesar, como `Raw24Khz16BitMonoPcm`, no incluyen encabezados de audio. Los formatos sin procesar solo se deben usar cuando se sepa que la implementación de bajada puede descodificar una secuencia de bits sin procesar, o bien si planea crear manualmente encabezados basados en la profundidad de bits, la frecuencia de muestreo, el número de canales, etc.

En este ejemplo, se especifica un formato RIFF de alta fidelidad `Riff24Khz16BitMonoPcm`, para lo que se establece `SpeechSynthesisOutputFormat` en el objeto `SpeechConfig`. Al igual que en el ejemplo de la sección anterior, se usa [`AudioDataStream`](/dotnet/api/microsoft.cognitiveservices.speech.audiodatastream) para obtener una secuencia en memoria del resultado y, después, escribirla en un archivo.

```csharp
static async Task SynthesizeAudioAsync()
{
    var config = SpeechConfig.FromSubscription("<paste-your-speech-key-here>", "<paste-your-speech-location/region-here>");
    config.SetSpeechSynthesisOutputFormat(SpeechSynthesisOutputFormat.Riff24Khz16BitMonoPcm);

    using var synthesizer = new SpeechSynthesizer(config, null);
    var result = await synthesizer.SpeakTextAsync("Customizing audio output format.");

    using var stream = AudioDataStream.FromResult(result);
    await stream.SaveToWaveFileAsync("path/to/write/file.wav");
}
```

Si vuelve a ejecutar el programa, se escribirá un archivo `.wav` en la ruta de acceso especificada.

## <a name="use-ssml-to-customize-speech-characteristics"></a>Uso de SSML para personalizar las características de la voz

El lenguaje de marcado de síntesis de voz (SSML) permite ajustar el tono, la pronunciación, la velocidad del habla, el volumen, etc. de la salida de texto a voz mediante el envío de solicitudes desde un lenguaje de definición de esquema XML. En esta sección se muestra un ejemplo de cambio de voz, pero para obtener una guía más detallada, consulte el [artículo de procedimientos de SSML](../../../speech-synthesis-markup.md).

Para empezar a usar SSML para la personalización, realice un cambio sencillo que cambie la voz.
En primer lugar, cree un archivo XML para la configuración de SSML en el directorio raíz del proyecto, en este ejemplo `ssml.xml`. El elemento raíz es siempre `<speak>` y si se ajusta el texto en un elemento `<voice>`, se puede cambiar la voz mediante el parámetro `name`. Consulte la [lista completa](../../../language-support.md#neural-voices) de voces **neuronales** admitidas.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-US-ChristopherNeural">
    When you're on the freeway, it's a good idea to use a GPS.
  </voice>
</speak>
```

A continuación, debe cambiar la solicitud de síntesis de voz para que haga referencia al archivo XML. La solicitud es básicamente la misma, pero en lugar de usar la función `SpeakTextAsync()`, se usa `SpeakSsmlAsync()`. Esta función espera una cadena XML, por lo que lo primero que se debe hacer es cargar la configuración de SSML como una cadena, para lo que se usa `File.ReadAllText()`. Desde aquí, el objeto de resultado es exactamente el mismo que el de los ejemplos anteriores.

> [!NOTE]
> Si usa Visual Studio, es probable que la configuración de compilación no encuentre el archivo XML de forma predeterminada. Para solucionarlo, haga clic con el botón derecho en el archivo XML y seleccione **Propiedades**. Cambie **Acción de compilación** a *Contenido* y cambie **Copiar en el directorio de resultados** a *Copiar siempre*.

```csharp
public static async Task SynthesizeAudioAsync()
{
    var config = SpeechConfig.FromSubscription("<paste-your-speech-key-here>", "<paste-your-speech-location/region-here>");
    using var synthesizer = new SpeechSynthesizer(config, null);

    var ssml = File.ReadAllText("./ssml.xml");
    var result = await synthesizer.SpeakSsmlAsync(ssml);

    using var stream = AudioDataStream.FromResult(result);
    await stream.SaveToWaveFileAsync("path/to/write/file.wav");
}
```

> [!NOTE]
> Para cambiar la voz sin usar SSML, puede establecer la propiedad en `SpeechConfig` mediante `SpeechConfig.SpeechSynthesisVoiceName = "en-US-ChristopherNeural";`

## <a name="get-facial-pose-events"></a>Obtención de eventos de postura facial

La voz puede ser una buena forma de dotar de animación a las expresiones faciales.
Para representar las posiciones principales en el habla observada, como la colocación de los labios, la mandíbula y la lengua al producir un fonema determinado, con frecuencia se usan [visemas](../../../how-to-speech-synthesis-viseme.md).
Puede suscribirse al evento viseme en Speech SDK.
Después, puede aplicar eventos de visema para animar la cara de un personaje mientras se reproduce el audio de voz.
Aprenda a [obtener eventos de visema](../../../how-to-speech-synthesis-viseme.md#get-viseme-events-with-the-speech-sdk).
