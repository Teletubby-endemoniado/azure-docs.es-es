---
title: Uso de la API de Azure Video Analyzer for Media (anteriormente, Video Indexer)
description: En este artículo se describe cómo empezar a usar la API de Azure Video Analyzer for Media (anteriormente, Video Indexer).
ms.date: 01/07/2021
ms.topic: tutorial
ms.custom: devx-track-csharp
ms.openlocfilehash: 83aa673625ad2119adc35571a8aaa98d3a3736a0
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128658647"
---
# <a name="tutorial-use-the-video-analyzer-for-media-api"></a>Tutorial: Uso de la API de Video Analyzer para Media

Azure Video Analyzer for Media (anteriormente, Video Indexer) consolida varias tecnologías de inteligencia artificial (IA) de audio y vídeo ofrecidas por Microsoft en un servicio integrado que facilita el desarrollo. Las API están diseñadas para que los desarrolladores puedan centrarse en el uso de las tecnologías de IA de multimedia sin preocuparse por la escala, el alcance global, la disponibilidad y la confiabilidad de las plataformas en la nube. Puede usar la API para cargar los archivos, obtener información de vídeo detallada, obtener direcciones URL de widgets de reproductor e información insertables, etc.

Al crear una cuenta de Video Analyzer for Media, puede elegir una cuenta de evaluación gratuita (con la que obtendrá un número determinado de minutos gratuitos de indexación) o una opción de pago (con la que no está limitado por la cuota). Con una versión de evaluación gratuita, Video Analyzer for Media proporciona hasta 600 minutos de indexación gratuita a los usuarios del sitio web y hasta 2400 minutos de indexación gratuita para los usuarios de la API. Con una opción de pago, se crea una cuenta de Video Analyzer for Media que está [conectada a su suscripción de Azure y a una cuenta de Azure Media Services](connect-to-azure.md). Se paga por minutos de indexación. Para más información, consulte [Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/).

En este artículo se muestra cómo los desarrolladores pueden sacar partido de la [API de Video Analyzer for Media](https://api-portal.videoindexer.ai/).

## <a name="subscribe-to-the-api"></a>Suscripción a la API

1. Inicie sesión en el [portal para desarrolladores de Video Analyzer for Media](https://api-portal.videoindexer.ai/).

    Revise una nota de la versión con respecto a la [información de inicio de sesión](release-notes.md#october-2020).
    
     ![Inicio de sesión en el portal para desarrolladores de Video Analyzer for Media](./media/video-indexer-use-apis/sign-in.png)

   > [!Important]
   > * Debe usar el mismo proveedor que usó al suscribirse a Video Analyzer for Media.
   > * Las cuentas personales de Google y Microsoft (Outlook/Live) solo se pueden usar para las cuentas de evaluación gratuita. Las cuentas conectadas a Azure requieren Azure AD.
   > * Solo puede haber una cuenta activa por correo electrónico. Si un usuario intenta iniciar sesión con user@gmail.com para LinkedIn y después con user@gmail.com para Google, este último muestra una página de error que indica que el usuario ya existe.

2. Suscríbase.

   Seleccione la pestaña [Products](https://api-portal.videoindexer.ai/products) (Productos). Seleccione Authorization y suscríbase.
    
   ![Pestaña Productos del Portal para desarrolladores de Video Indexer](./media/video-indexer-use-apis/authorization.png)

   > [!NOTE]
   > Los nuevos usuarios se suscriben automáticamente a Authorization.
    
   Después de suscribirse, podrá encontrar su suscripción en **Productos** -> **Autorización**. En la página Suscripción, encontrará las claves principal y secundaria. Debe proteger las claves. Las claves solo debe usarlas el código del servidor. No deben estar disponibles en el cliente (.js, .html, etc.).

   ![Suscripción y claves en el Portal para desarrolladores de Video Indexer](./media/video-indexer-use-apis/subscriptions.png)

> [!TIP]
> El usuario de Video Analyzer for Media puede usar una única clave de suscripción para conectarse a varias cuentas de Video Analyzer for Media. A continuación, puede vincular estas cuentas de Video Analyzer for Media a distintas cuentas de Media Services.

## <a name="obtain-access-token-using-the-authorization-api"></a>Obtención del token de acceso mediante Authorization API

Una vez suscrito a la API de autorización, puede obtener tokens de acceso. Estos tokens de acceso se usan para autenticarse en Operations API.

Cada llamada a Operations API debe asociarse con un token de acceso, correspondiente al ámbito de autorización de la llamada.

- Nivel de usuario: los tokens de acceso de nivel de usuario permiten realizar operaciones en el nivel de **usuario**. Por ejemplo, obtener cuentas asociadas.
- Nivel de cuenta: los tokens de acceso de nivel de cuenta permiten realizar operaciones en el nivel de **cuenta** o de **vídeo**. Por ejemplo, cargar vídeo, enumerar todos los vídeos, obtener información de vídeo, etc.
- Nivel de vídeo: los tokens de acceso de nivel de vídeo permiten realizar operaciones en un **vídeo** concreto. Por ejemplo, obtener información de vídeo, descargar títulos, obtener widgets, etc.

Puede controlar el nivel de permiso de los tokens de dos maneras:

* Para los tokens de **cuenta**, puede la API para **obtener un token de acceso de cuenta con permiso** y especificar el tipo de permiso (**lector**/**colaborador**/**MyAccessManager**/**propietario**).
* Para todos los tipos de tokens (incluidos los tokens de **cuenta**), puede especificar **allowEdit=true/false**. **false** es el equivalente de un permiso **lector** (solo lectura) y **true** es el equivalente de un permiso **colaborador** (lectura y escritura).

En la mayoría de los escenarios de servidor a servidor probablemente use el mismo token de **cuenta**, porque abarca tanto operaciones de **cuenta** como de **vídeo**. Pero si va a realizar llamadas de cliente a Video Analyzer for Media (por ejemplo, desde JavaScript), probablemente quiera usar un token de acceso de **vídeo** para evitar que los clientes tengan acceso a toda la cuenta. Ese es también el motivo por el que, cuando se inserta código de cliente de Video Analyzer for Media en el cliente (por ejemplo, mediante los widgets **Obtener información** u **Obtener reproductor**), se debe proporcionar un token de acceso de **vídeo**.

Para facilitar las cosas, puede usar **Authorization** API > **GetAccounts** para obtener las cuentas sin obtener un token de usuario primero. También puede solicitar las cuentas con tokens válidos, para ahorrarse una llamada al obtener un token de cuenta.

Los tokens de acceso expiran en 1 hora. Asegúrese de que el token de acceso es válido antes de usar Operations API. Si expira, vuelva a llamar a la API de autorización para obtener un nuevo token de acceso.

Ya está listo para iniciar la integración con la API. Consulte [la descripción detallada de cada API REST de Video Analyzer for Media](https://api-portal.videoindexer.ai/).

## <a name="account-id"></a>Identificador de cuenta

El parámetro accountId es obligatorio en todas las llamadas a las API de operaciones. El identificador de cuenta es un GUID que puede obtenerse de las siguientes maneras:

* Use el **sitio web de Video Analyzer for Media** para obtener el identificador de cuenta:

    1. Vaya al sitio web de [Video Analyzer for Media](https://www.videoindexer.ai/) e inicie sesión.
    2. Vaya a la página **Configuración**.
    3. Copie el identificador de cuenta.

        ![Configuración e identificador de cuenta de Video Analyzer for Media](./media/video-indexer-use-apis/account-id.png)

* Use el **portal para desarrolladores de Video Analyzer for Media** para obtener el identificador de cuenta mediante programación.

    Use la API [Get account](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Get-Account).

    > [!TIP]
    > Para generar tokens de acceso para las cuentas, puede definir `generateAccessTokens=true`.

* Obtenga el identificador de cuenta de la dirección URL de una página de reproductor en su cuenta.

    Cuando vea un vídeo, el identificador aparece después de la sección `accounts` y antes de la sección `videos`.

    ```
    https://www.videoindexer.ai/accounts/00000000-f324-4385-b142-f77dacb0a368/videos/d45bf160b5/
    ```

## <a name="recommendations"></a>Recomendaciones

En esta sección se enumeran algunas recomendaciones para usar la API Video Analyzer for Media.

- Si va a cargar un vídeo, se recomienda colocar el archivo en una ubicación de red pública (por ejemplo, una cuenta de Azure Blob Storage). Obtenga el vínculo al vídeo y proporcione la dirección URL como parámetro del archivo de carga.

    La dirección URL proporcionada a Video Analyzer for Media tiene que apuntar a un archivo multimedia (audio o vídeo). Para comprobar la dirección URL (o la URL de SAS) fácilmente, péguela en un explorador: si el archivo se comienza a reproducir o a descargar, probablemente es una dirección URL válida. Si el explorador representa alguna visualización, probablemente no es un vínculo a un archivo, sino a una página HTML.

- Cuando se llama a la API que obtiene información detallada del vídeo especificado, se obtiene una salida JSON detallada como contenido de la respuesta. [Consulte más información sobre el código JSON devuelto en este tema](video-indexer-output-json-v2.md).

## <a name="code-sample"></a>Código de ejemplo

En el siguiente fragmento de código de C# se muestra el uso de todas las API de Video Analyzer for Media juntas.

```csharp
var apiUrl = "https://api.videoindexer.ai";
var accountId = "..."; 
var location = "westus2"; // replace with the account's location, or with “trial” if this is a trial account
var apiKey = "..."; 

System.Net.ServicePointManager.SecurityProtocol = System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

// create the http client
var handler = new HttpClientHandler(); 
handler.AllowAutoRedirect = false; 
var client = new HttpClient(handler);
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey); 

// obtain account access token
var accountAccessTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/AccessToken?allowEdit=true").Result;
var accountAccessToken = accountAccessTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// upload a video
var content = new MultipartFormDataContent();
Debug.WriteLine("Uploading...");
// get the video from URL
var videoUrl = "VIDEO_URL"; // replace with the video URL

// as an alternative to specifying video URL, you can upload a file.
// remove the videoUrl parameter from the query string below and add the following lines:
  //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
  //byte[] buffer = new byte[video.Length];
  //video.Read(buffer, 0, buffer.Length);
  //content.Add(new ByteArrayContent(buffer));

var uploadRequestResult = client.PostAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos?accessToken={accountAccessToken}&name=some_name&description=some_description&privacy=private&partition=some_partition&videoUrl={videoUrl}", content).Result;
var uploadResult = uploadRequestResult.Content.ReadAsStringAsync().Result;

// get the video id from the upload result
var videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
Debug.WriteLine("Uploaded");
Debug.WriteLine("Video ID: " + videoId);           

// obtain video access token
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
var videoTokenRequestResult = client.GetAsync($"{apiUrl}/auth/{location}/Accounts/{accountId}/Videos/{videoId}/AccessToken?allowEdit=true").Result;
var videoAccessToken = videoTokenRequestResult.Content.ReadAsStringAsync().Result.Replace("\"", "");

client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

// wait for the video index to finish
while (true)
{
  Thread.Sleep(10000);

  var videoGetIndexRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/Index?accessToken={videoAccessToken}&language=English").Result;
  var videoGetIndexResult = videoGetIndexRequestResult.Content.ReadAsStringAsync().Result;

  var processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

  Debug.WriteLine("");
  Debug.WriteLine("State:");
  Debug.WriteLine(processingState);

  // job is finished
  if (processingState != "Uploaded" && processingState != "Processing")
  {
      Debug.WriteLine("");
      Debug.WriteLine("Full JSON:");
      Debug.WriteLine(videoGetIndexResult);
      break;
  }
}

// search for the video
var searchRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/Search?accessToken={accountAccessToken}&id={videoId}").Result;
var searchResult = searchRequestResult.Content.ReadAsStringAsync().Result;
Debug.WriteLine("");
Debug.WriteLine("Search:");
Debug.WriteLine(searchResult);

// get insights widget url
var insightsWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/InsightsWidget?accessToken={videoAccessToken}&widgetType=Keywords&allowEdit=true").Result;
var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
Debug.WriteLine("Insights Widget url:");
Debug.WriteLine(insightsWidgetLink);

// get player widget url
var playerWidgetRequestResult = client.GetAsync($"{apiUrl}/{location}/Accounts/{accountId}/Videos/{videoId}/PlayerWidget?accessToken={videoAccessToken}").Result;
var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
Debug.WriteLine("");
Debug.WriteLine("Player Widget url:");
Debug.WriteLine(playerWidgetLink);
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado este tutorial, elimine los recursos que no planea usar.

## <a name="see-also"></a>Consulte también

- [Información general de Video Analyzer for Media](video-indexer-overview.md)
- [Regiones](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)

## <a name="next-steps"></a>Pasos siguientes

- [Examen detallado de la salida JSON](video-indexer-output-json-v2.md).
- Consulte el [código de ejemplo](https://github.com/Azure-Samples/media-services-video-indexer) que muestra un aspecto importante de la carga y la indexación de un vídeo. Seguir el código le proporcionará una buena idea de cómo usar nuestra API para las funcionalidades básicas. Asegúrese de leer los comentarios en línea y observe nuestros consejos de procedimientos recomendados.

