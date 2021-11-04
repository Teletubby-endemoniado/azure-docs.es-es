---
author: eric-urban
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: eur
ms.custom: devx-track-js
ms.openlocfilehash: 017fb0117f26000d5cc35f4967bf56d8c714de6f
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131506603"
---
:::row:::
    :::column span="3":::
        El SDK de Voz para JavaScript está disponible como paquete npm. Consulte <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">microsoft-cognitiveservices-speech-sdk </a> y su repositorio de GitHub complementario <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">cognitive-services-speech-sdk-js </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="JavaScript" src="https://docs.microsoft.com/media/logos/logo_js.svg"  width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Aunque el SDK de Voz para JavaScript está disponible como un paquete npm y los exploradores web de cliente y Node.js pueden utilizarlo, tenga en cuenta las distintas implicaciones arquitectónicas de cada entorno. Por ejemplo, la instancia de <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">Document Object Model (DOM) </a> no está disponible para las aplicaciones del lado servidor, igual que el <a href="https://nodejs.org/api/fs.html" target="_blank">sistema de archivos </a> no está disponible para las aplicaciones del lado cliente.

### <a name="nodejs-package-manager-npm"></a>Administrador de paquetes de Node.js (NPM)

Para instalar el SDK de Voz para JavaScript, ejecute el siguiente comando `npm install`.

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

### <a name="html-script-tag"></a>Etiqueta de script HTML

Como alternativa, podría incluir directamente una etiqueta `<script>` en el elemento HTML `<head>`, confiando en la distribución NPM <a href="https://www.jsdelivr.com/package/npm/microsoft-cognitiveservices-speech-sdk" target="_blank">**JSDelivr**</a>.

```html
<script src="https://cdn.jsdelivr.net/npm/microsoft-cognitiveservices-speech-sdk@latest/distrib/browser/microsoft.cognitiveservices.speech.sdk.bundle-min.js">
</script>
```

Para más información, vea la <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser" target="_blank">guía de inicio rápido del SDK de voz del explorador web </a>.
