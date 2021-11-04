---
author: eric-urban
ms.service: cognitive-services
ms.topic: include
ms.date: 10/20/2020
ms.author: eur
ms.openlocfilehash: 05d94ddec3c4f5302eb83c430642190759a00561
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131508949"
---
## <a name="install-the-speech-sdk"></a>Instalación de Speech SDK

En primer lugar, deberá instalar <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">Speech SDK para JavaScript</a>. Utilice las siguientes instrucciones en función de la plataforma:

- <a href="/azure/cognitive-services/speech-service/speech-sdk?tabs=nodejs#get-the-speech-sdk" target="_blank">Node.js <span 
class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="/azure/cognitive-services/speech-service/speech-sdk?tabs=browser#get-the-speech-sdk" target="_blank">Explorador web </a>

## <a name="create-voice-signatures"></a>Creación de firmas de voz

(Puede omitir este paso si no quiere usar perfiles de usuario inscritos previamente para identificar participantes concretos).

Si quiere inscribir perfiles de usuario, el primer paso consiste en crear firmas de voz para los participantes en la conversación a fin de que puedan identificarse como hablantes únicos. El archivo de audio `.wav` de entrada para la creación de firmas de voz debe ser de 16 bits, con frecuencia de muestreo de 16 kHz y un formato de canal único (mono). La longitud recomendada para cada muestra de audio está entre 30 segundos y dos minutos. Una muestra de audio demasiado corta dará como resultado una precisión reducida al reconocer el hablante. El archivo `.wav` debe ser una muestra de voz de **una persona** para que se cree un perfil de voz único.

El ejemplo siguiente muestra cómo crear una firma de voz [mediante la API REST](https://aka.ms/cts/signaturegenservice) de JavaScript. Tenga en cuenta que debe sustituir la información real de `subscriptionKey`, `region` y la ruta de acceso a un archivo `.wav` de ejemplo.

```javascript
const fs = require('fs');
const axios = require('axios');
const formData = require('form-data');
 
const subscriptionKey = 'your-subscription-key';
const region = 'your-region';
 
async function createProfile() {
    let form = new formData();
    form.append('file', fs.createReadStream('path-to-voice-sample.wav'));
    let headers = form.getHeaders();
    headers['Ocp-Apim-Subscription-Key'] = subscriptionKey;
 
    let url = `https://signature.${region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData`;
    let response = await axios.post(url, form, { headers: headers });
    
    // get signature from response, serialize to json string
    return JSON.stringify(response.data.Signature);
}
 
async function main() {
    // use this voiceSignature string with conversation transcription calls below
    let voiceSignatureString = await createProfile();
    console.log(voiceSignatureString);
}
main();
```

La ejecución de este script devuelve una cadena de firma de voz en la variable `voiceSignatureString`. Ejecute la función dos veces, de modo que tenga dos cadenas para usarlas como entrada para las variables `voiceSignatureStringUser1` y `voiceSignatureStringUser2` a continuación.

> [!NOTE]
> Las firmas de voz **solo** pueden crearse mediante la API REST.

## <a name="transcribe-conversations"></a>Transcripción de conversaciones

El siguiente código de ejemplo muestra cómo transcribir conversaciones en tiempo real para dos hablantes. Se da por supuesto que ya ha creado cadenas de firma de voz para cada hablante como se muestra arriba. Sustituya la información real de `subscriptionKey`, `region` y la ruta de acceso `filepath` por la del audio que desea transcribir.

Si no usa perfiles de usuario inscritos previamente, se tardarán unos segundos más en completar el primer reconocimiento de usuarios desconocidos como speaker1, speaker2, etc.

> [!NOTE]
> Asegúrese de que se usa el mismo valor `subscriptionKey` en la aplicación para la creación de firmas o se producirán errores. 

Este código de ejemplo realiza las tareas siguientes:

* Crea un flujo de entrada que se usará para la transcripción y escribe el archivo `.wav` de ejemplo en él.
* Crea una `Conversation` mediante `createConversationAsync()`.
* Crea un `ConversationTranscriber` mediante el constructor.
* Agrega participantes a la conversación. Las cadenas `voiceSignatureStringUser1` y `voiceSignatureStringUser2` deben aparecer como salidas de los pasos anteriores.
* Se registra en eventos y comienza la transcripción.
* Si quiere diferenciar a los hablantes sin proporcionar ejemplos de voz, habilite la característica `DifferentiateGuestSpeakers` como en la [Introducción a la transcripción de conversaciones](../../../conversation-transcription.md). 

```javascript
(function() {
    "use strict";
    var sdk = require("microsoft-cognitiveservices-speech-sdk");
    var fs = require("fs");
    
    var subscriptionKey = "your-subscription-key";
    var region = "your-region";
    var filepath = "audio-file-to-transcribe.wav"; // 8-channel audio
    
    var speechTranslationConfig = sdk.SpeechTranslationConfig.fromSubscription(subscriptionKey, region);
    var audioConfig = sdk.AudioConfig.fromWavFileInput(fs.readFileSync(filepath));
    speechTranslationConfig.setProperty("ConversationTranscriptionInRoomAndOnline", "true");

    // en-us by default. Adding this code to specify other languages, like zh-cn.
    speechTranslationConfig.speechRecognitionLanguage = "en-US";
    
    // create conversation and transcriber
    var conversation = sdk.Conversation.createConversationAsync(speechTranslationConfig, "myConversation");
    var transcriber = new sdk.ConversationTranscriber(audioConfig);
    
    // attach the transcriber to the conversation
    transcriber.joinConversationAsync(conversation,
    function () {
        // add first participant using voiceSignature created in enrollment step
        var user1 = sdk.Participant.From("user1@example.com", "en-us", voiceSignatureStringUser1);
        conversation.addParticipantAsync(user1,
        function () {
            // add second participant using voiceSignature created in enrollment step
            var user2 = sdk.Participant.From("user2@example.com", "en-us", voiceSignatureStringUser2);
            conversation.addParticipantAsync(user2,
            function () {
                transcriber.sessionStarted = function(s, e) {
                console.log("(sessionStarted)");
                };
                transcriber.sessionStopped = function(s, e) {
                console.log("(sessionStopped)");
                };
                transcriber.canceled = function(s, e) {
                console.log("(canceled)");
                };
                transcriber.transcribed = function(s, e) {
                console.log("(transcribed) text: " + e.result.text);
                console.log("(transcribed) speakerId: " + e.result.speakerId);
                };
    
                // begin conversation transcription
                transcriber.startTranscribingAsync(
                function () { },
                function (err) {
                    console.trace("err - starting transcription: " + err);
                });
        },
        function (err) {
            console.trace("err - adding user1: " + err);
        });
    },
    function (err) {
        console.trace("err - adding user2: " + err);
    });
    },
    function (err) {
    console.trace("err - " + err);
    });
}()); 
```
