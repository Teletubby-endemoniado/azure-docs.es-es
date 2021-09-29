---
title: Inserción de widgets de Azure Video Analyzer for Media (anteriormente, Video Indexer) en las aplicaciones
description: Obtenga información sobre cómo insertar widgets de Azure Video Analyzer for Media (anteriormente, Video Indexer) en las aplicaciones.
ms.topic: how-to
ms.date: 01/25/2021
ms.author: juliako
ms.custom: devx-track-js
ms.openlocfilehash: 7ab396292a1fe7d44391f9f6ba75d69a57e57dd3
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128615954"
---
# <a name="embed-video-analyzer-for-media-widgets-in-your-apps"></a>Inserción de widgets de Video Analyzer for Media en las aplicaciones

En este artículo se muestra cómo puede insertar widgets de Azure Video Analyzer for Media (anteriormente, Video Indexer) en las aplicaciones. Video Analyzer for Media admite la inserción de tres tipos de widgets en las aplicaciones: *Cognitive Insights*, *Player* y *Editor*.

A partir de la versión 2, la dirección URL base del widget incluye la región de la cuenta especificada. Por ejemplo, se genera una cuenta en la región Oeste de EE. UU.: `https://www.videoindexer.ai/embed/insights/.../?location=westus2`.

## <a name="widget-types"></a>Tipos de widget

### <a name="cognitive-insights-widget"></a>Widget Cognitive Insights

Un widget Cognitive Insights incluye todas las conclusiones visuales que se extrajeron del proceso de indexación de vídeo. El widget Cognitive Insights admite los siguientes parámetros de URL opcionales:

|Nombre|Definición|Descripción|
|---|---|---|
|`widgets` | Cadenas separadas por coma | Le permite controlar las conclusiones que desea representar.<br/>Ejemplo: `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,keywords` representa solo las conclusiones de interfaz de usuario de personas y palabras clave.<br/>Opciones disponibles: people, animatedCharacters ,keywords, labels, sentiments, emotions, topics, keyframes, transcript, ocr, speakers, scenes y namedEntities.|
|`controls`|Cadenas separadas por coma|Le permite controlar los controles que quiere representar.<br/>Ejemplo: `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?controls=search,download` solo representa la opción de búsqueda y el botón de descarga.<br/>Opciones disponibles: buscar, descargar, valores preestablecidos, idioma.|
|`language`|Código corto de idioma (nombre de idioma)|Controla el idioma de las conclusiones.<br/>Ejemplo: `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?language=es-es` <br/>o bien `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?language=spanish`|
|`locale` | Código corto de idioma | Controla el idioma de la interfaz de usuario. El valor predeterminado es `en`. <br/>Ejemplo: `locale=de`.|
|`tab` | Pestaña seleccionada de manera predeterminada | Controla la pestaña **Conclusiones** que se representa de manera predeterminada. <br/>Ejemplo: `tab=timeline` representa las conclusiones con la pestaña **Escala de tiempo** seleccionada.|
|`location` ||El parámetro `location` debe estar incluido en los vínculos incrustados. Vea [cómo obtener el nombre de su región](regions.md). Si su cuenta se encuentra en versión preliminar, use `trial` para el valor de ubicación. `trial` es el valor predeterminado para el parámetro `location`.| 

### <a name="player-widget"></a>Widget Player

Puede usar el widget Player para transmitir vídeo mediante la velocidad de bits adaptable. El widget Player admite los siguientes parámetros de URL opcionales:

|Nombre|Definición|Descripción|
|---|---|---|
|`t` | Segundos desde el inicio | Hace que el reproductor comience a reproducir desde un momento especificado.<br/> Ejemplo: `t=60`. |
|`captions` | Código de idioma | Recupera el subtítulo en el idioma especificado durante la carga del widget para que esté disponible en el menú **Subtítulos**.<br/> Ejemplo: `captions=en-US`. |
|`showCaptions` | Un valor booleano | Hace que el reproductor se cargue con los títulos ya habilitados.<br/> Ejemplo: `showCaptions=true`. |
|`type`| | Activa una máscara del reproductor de audio (se quita la parte de vídeo).<br/> Ejemplo: `type=audio`. |
|`autoplay` | Un valor booleano | Indica si el reproductor debe comenzar a reproducir el vídeo una vez esté cargado. El valor predeterminado es `true`.<br/> Ejemplo: `autoplay=false`. |
|`language`/`locale` | Código de idioma | Controla el idioma del reproductor. El valor predeterminado es `en-US`.<br/>Ejemplo: `language=de-DE`.|
|`location` ||El parámetro `location` debe estar incluido en los vínculos incrustados. Vea [cómo obtener el nombre de su región](regions.md). Si su cuenta se encuentra en versión preliminar, use `trial` para el valor de ubicación. `trial` es el valor predeterminado para el parámetro `location`.| 

### <a name="editor-widget"></a>Widget Editor

Puede usar el widget Editor para crear nuevos proyectos y administrar las conclusiones de vídeo. El widget Editor admite los siguientes parámetros de URL opcionales:

|Nombre|Definición|Descripción|
|---|---|---|
|`accessToken`<sup>*</sup> | String | Proporciona acceso a vídeos que solo están en la cuenta que se usa para insertar el widget.<br> El widget Editor requiere el parámetro `accessToken`. |
|`language` | Código de idioma | Controla el idioma del reproductor. El valor predeterminado es `en-US`.<br/>Ejemplo: `language=de-DE`. |
|`locale` | Código corto de idioma | Controla el idioma de la información. El valor predeterminado es `en`.<br/>Ejemplo: `language=de`. |
|`location` ||El parámetro `location` debe estar incluido en los vínculos incrustados. Vea [cómo obtener el nombre de su región](regions.md). Si su cuenta se encuentra en versión preliminar, use `trial` para el valor de ubicación. `trial` es el valor predeterminado para el parámetro `location`.| 

<sup>*</sup>El propietario debe proporcionar `accessToken` con precaución.

## <a name="embed-videos"></a>Inserción de vídeos

En esta sección se describe la inserción de vídeos [mediante el portal](#the-portal-experience) o [ensamblando la dirección URL manualmente](#assemble-the-url-manually) en aplicaciones. 

El parámetro `location` debe estar incluido en los vínculos incrustados. Vea [cómo obtener el nombre de su región](regions.md). Si su cuenta se encuentra en versión preliminar, use `trial` para el valor de ubicación. `trial` es el valor predeterminado para el parámetro `location`. Por ejemplo: `https://www.videoindexer.ai/accounts/00000000-0000-0000-0000-000000000000/videos/b2b2c74b8e/?location=trial`.

### <a name="the-portal-experience"></a>Experiencia del portal

Para insertar un vídeo, use el portal como se describe a continuación:

1. Inicie sesión en el sitio web de [Video Analyzer for Media](https://www.videoindexer.ai/).
1. Seleccione el vídeo con el que quiera trabajar y presione **Reproducir**.
1. Elija el tipo de widget que quiera (**Cognitive Insights**, **Player** o **Editor**).
1. Haga clic en **&lt;/&gt; Insertar**.
5. Copie el código para insertar (aparece en **Copy the embedded code** (Copiar el código insertado) en el cuadro de diálogo **Compartir e insertar**).
6. Agregue el código a la aplicación.

> [!NOTE]
> Al compartir un vínculo para el widget **Reproductor** o **Conclusiones**, se incluirá el token de acceso y se concederán permisos de solo lectura a su cuenta.

### <a name="assemble-the-url-manually"></a>Ensamblado manual de la dirección URL

#### <a name="public-videos"></a>Vídeos públicos

Puede insertar vídeos públicos ensamblando la dirección URL de la siguiente manera:

`https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<videoId>`
  
  
#### <a name="private-videos"></a>Vídeos privados

Para insertar un vídeo privado, antes debe pasar un token de acceso (use [Get Video Access Token](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Get-Video-Access-Token) en el atributo `src` de iframe:

`https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<videoId>/?accessToken=<accessToken>`
  
### <a name="provide-editing-insights-capabilities"></a>Proporcionar funcionalidades de edición de conclusiones

Para proporcionar funcionalidades de conclusiones de edición en el widget insertado, tiene que pasar un token de acceso que incluya permisos de edición. Use [Get Video Access Token](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Get-Video-Access-Token) con `&allowEdit=true`.

## <a name="widgets-interaction"></a>Interacción de widgets

El widget Cognitive Insights puede interactuar con un vídeo en su aplicación. En esta sección se muestra cómo conseguir esta interacción.

![Widget Cognitive Insights para Video Analyzer for Media](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="flow-overview"></a>Información general sobre el flujo

Al editar las transcripciones, se produce el siguiente flujo:

1. La transcripción se edita en la escala de tiempo.
1. Video Analyzer for Media obtiene estas actualizaciones y las guarda en el archivo [Ediciones de transcripción de origen](customize-language-model-with-website.md#customize-language-models-by-correcting-transcripts) en el modelo de lenguaje.
1. Los subtítulos se actualizan:

    * Si usa el widget del reproductor de Video Analyzer for Media, se actualizan automáticamente.
    * Si usa un reproductor externo, puede obtener un nuevo archivo de subtítulos mediante una llamada **Get video captions**.

### <a name="cross-origin-communications"></a>Comunicaciones entre orígenes

Para que los widgets de Video Analyzer for Media se comuniquen con otros componentes, el servicio Video Analyzer for Media:

- Usa el método HTML5 de comunicación entre orígenes `postMessage`.
- Valida el mensaje mediante el origen VideoIndexer.ai.

Si implementa su propio código del reproductor y realizar la integración con los widgets Cognitive Insights, es su responsabilidad validar el origen del mensaje que proviene de VideoIndexer.ai.

### <a name="embed-widgets-in-your-app-or-blog-recommended"></a>Inserción de widgets en la aplicación o blog (recomendado)

En esta sección se muestra cómo lograr la interacción entre dos widgets de Video Analyzer for Media para que cuando un usuario seleccione el control de información de la aplicación, el reproductor salte al momento pertinente.

1. Copie el código para insertar del widget Player.
2. Copie el código para insertar de Cognitive Insights.
3. Agregue el [archivo mediador](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) para controlar la comunicación entre los dos widgets:<br/> 
`<script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>`

Ahora, cuando un usuario selecciona el control de conclusión en la aplicación, el reproductor salta al momento pertinente.

Para más información, consulte [la demo de Video Analyzer for Media - Embed both Widgets](https://codepen.io/videoindexer/pen/NzJeOb) (Video Analyzer for Media: insertar ambos widgets).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Inserte el widget Cognitive Insights y utilice Azure Media Player para reproducir el contenido.

En esta sección se muestra cómo lograr la interacción entre un widget Cognitive Insights y una instancia de Azure Media Player mediante el [complemento AMP](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js).

1. Agregue un complemento Video Analyzer for Media para el reproductor de AMP:<br/> `<script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>`
2. Cree una instancia de Azure Media Player con el complemento de Video Analyzer for Media.

    ```javascript
    // Init the source.
    function initSource() {
        var tracks = [{
        kind: 'captions',
        // To load vtt from VI, replace it with your vtt URL.
        src: this.getSubtitlesUrl("c4c1ad4c9a", "English"),
        srclang: 'en',
        label: 'English'
        }];
        myPlayer.src([
        {
            "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
            "type": "application/vnd.ms-sstr+xml"
        }
        ], tracks);
    }

    // Init your AMP instance.
    var myPlayer = amp('vid1', { /* Options */
        "nativeControlsForTouch": false,
        autoplay: true,
        controls: true,
        width: "640",
        height: "400",
        poster: "",
        plugins: {
        videobreakedown: {}
        }
    }, function () {
        // Activate the plug-in.
        this.videobreakdown({
        videoId: "c4c1ad4c9a",
        syncTranscript: true,
        syncLanguage: true,
        location: "trial" /* location option for paid accounts (default is trial) */
        });

        // Set the source dynamically.
        initSource.call(this);
    });
    ```

3. Copie el código para insertar de Cognitive Insights.

Ahora puede comunicarse con Azure Media Player.

Para más información consulte la [demostración Azure Media Player + VI Insights](https://codepen.io/videoindexer/pen/rYONrO).

### <a name="embed-the-video-analyzer-for-media-cognitive-insights-widget-and-use-a-different-video-player"></a>Inserción del widget Cognitive Insights de Video Analyzer for Media y uso de un reproductor de vídeo diferente

Si usa un reproductor de vídeo que no sea Azure Media Player, tiene que manipular manualmente el reproductor de vídeo para lograr la comunicación.

1. Inserte el reproductor de vídeo.

    Por ejemplo, un reproductor HTML5 estándar:

    ```html
    <video id="vid1" width="640" height="360" controls autoplay preload>
       <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
       Your browser does not support the video tag.
    </video>
    ```

2. Inserte el widget Cognitive Insights.
3. Implemente la comunicación para el reproductor mediante la escucha del evento "mensaje". Por ejemplo:

    ```javascript
    <script>
    
        (function(){
        // Reference your player instance.
        var playerInstance = document.getElementById('vid1');
        
        function jumpTo(evt) {
          var origin = evt.origin || evt.originalEvent.origin;
        
          // Validate that the event comes from the videoindexer domain.
          if ((origin === "https://www.videoindexer.ai") && evt.data.time !== undefined){
                
            // Call your player's "jumpTo" implementation.
            playerInstance.currentTime = evt.data.time;
               
            // Confirm the arrival to us.
            if ('postMessage' in window) {
              evt.source.postMessage({confirm: true, time: evt.data.time}, origin);
            }
          }
        }
        
        // Listen to the message event.
        window.addEventListener("message", jumpTo, false);
          
        }())    
        
    </script>
    ```

Para más información consulte la [demostración Azure Media Player + VI Insights](https://codepen.io/videoindexer/pen/YEyPLd).

## <a name="adding-subtitles"></a>Adición de subtítulos

Si inserta conclusiones de Video Analyzer for Media con su propia instancia de [Azure Media Player](https://aka.ms/azuremediaplayer), puede usar el método `GetVttUrl` para obtener subtítulos. También puede llamar a un método de JavaScript desde el complemento AMP de Video Analyzer for Media `getSubtitlesUrl` (como se ha mostrado anteriormente).

## <a name="customizing-embeddable-widgets"></a>Personalización de widgets que se pueden insertar

### <a name="cognitive-insights-widget"></a>Widget Cognitive Insights

Puede elegir los tipos de conclusiones que desee. Para ello, especifíquelas como valor en el siguiente parámetro URL agregado al código para insertar que obtiene (de la API o de la aplicación web): `&widgets=<list of wanted widgets>`.

Los valores posibles son: `people`, `animatedCharacters` , `keywords`, `labels`, `sentiments`, `emotions`, `topics`, `keyframes`, `transcript`, `ocr`, `speakers`, `scenes` y `namedEntities`.

Por ejemplo, si quiere insertar un widget que contiene solo conclusiones de personas y palabras clave, la dirección URL para insertar iframe tendría este aspecto:

`https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,keywords`

También se puede personalizar el título de la ventana de IFrame proporcionando `&title=<YourTitle>` a la dirección URL de IFrame. (Personaliza el valor HTML `<title>`).
   
Por ejemplo, si quiere asignar a la ventana de IFrame el título "MyInsights", la dirección URL tendrá este aspecto:

`https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?title=MyInsights`

Tenga en cuenta que esta opción solo es relevante en aquellos casos en los que necesite abrir la información detallada en una nueva ventana.

### <a name="player-widget"></a>Widget Player

Si inserta el reproductor de Video Analyzer for Media, puede elegir el tamaño del reproductor al especificar el tamaño de IFrame.

Por ejemplo:

`<iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/" frameborder="0" allowfullscreen />`

De manera predeterminada, el reproductor de Video Analyzer for Media tiene subtítulos generados automáticamente que se basan en la transcripción del vídeo. La transcripción se extrae del vídeo con el idioma de origen que se seleccionó cuando se cargó el vídeo.

Si desea insertar con un idioma diferente, puede agregar `&captions=<Language Code>` a la dirección URL del reproductor de inserción. Si quiere que los subtítulos se muestren de forma predeterminada, puede pasar &showCaptions=true.

La dirección URL para insertar tendrá un aspecto similar al siguiente:

`https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/?captions=en-us`

#### <a name="autoplay"></a>Reproducción automática

De forma predeterminada, el reproductor iniciará la reproducción del vídeo. puede elegir no pasar `&autoplay=false` a la dirección URL de inserción anterior.

## <a name="code-samples"></a>Ejemplos de código

Consulte el repositorio de [ejemplos de código](https://github.com/Azure-Samples/media-services-video-indexer/tree/master/Embedding%20widgets) que contiene ejemplos de API y widgets de Video Analyzer for Media:

| Archivo/carpeta                       | Descripción                                |
|-----------------------------------|--------------------------------------------|
| `azure-media-player`              | Cargue vídeo de Video Analyzer for Media en una instancia personalizada de Azure Media Player.                        |
| `azure-media-player-vi-insights`  | Inserta VI Insights con una instancia de Azure Media Player personalizada.                             |
| `control-vi-embedded-player`      | Inserta VI Player y lo controla desde fuera.                                    |
| `custom-index-location`           | Inserta VI Insights desde una ubicación externa personalizada (puede ser un blob).     |
| `embed-both-insights`             | Uso básico de VI Insights, tanto Player como Insights.                            |
| `embed-insights-with-AMP`         | Inserta el widget de VI Insights con una instancia de Azure Media Player personalizada.                      |
| `customize-the-widgets`           | Inserta widgets de VI con opciones personalizadas.                                     |
| `embed-both-widgets`              | Inserta VI Player e Insights y establece la comunicación entre ellos.                      |
| `url-generator`                   | Genera la dirección URL de inserción personalizada de widgets según las opciones especificadas por el usuario.             |
| `html5-player`                    | Inserta VI Insights con un reproductor de vídeo HTML5 predeterminado.                           |

## <a name="supported-browsers"></a>Exploradores compatibles

Para más información, consulte los [exploradores compatibles](video-indexer-overview.md#supported-browsers).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo ver y editar la información de Video Analyzer for Media, consulte [Visualización y edición de la información detallada de Video Analyzer for Media](video-indexer-view-edit.md).

Además, consulte [Video Analyzer for Media de CodePen](https://codepen.io/videoindexer/pen/eGxebZ).
