---
title: Carga e indexación de vídeos con Azure Video Analyzer for Media (anteriormente, Video Indexer)
description: En este tema se muestra cómo usar las API para cargar e indexar los vídeos con Azure Video Analyzer for Media (anteriormente, Video Indexer).
ms.service: azure-video-analyzer
ms.topic: article
ms.date: 11/15/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: f2a9aa291937afcb55739cfdb4532d0a59f2bc9f
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132519598"
---
# <a name="upload-and-index-your-videos"></a>Carga e indexación de los vídeos  

Una vez que se ha cargado el vídeo, Azure Video Analyzer for Media (anteriormente, Video Indexer) codifica (opcionalmente) el vídeo (como se describe en el artículo). Al crear una cuenta de Video Analyzer for Media, puede elegir una cuenta de evaluación gratuita (con la que obtendrá un número determinado de minutos gratuitos de indexación) o una opción de pago (con la que no está limitado por la cuota). Con la versión de evaluación gratuita, Video Analyzer for Media proporciona hasta 600 minutos de indexación gratuita a los usuarios del sitio web y hasta 2400 minutos de indexación gratuita para los usuarios de la API. Con la opción de pago, se crea una cuenta de Video Analyzer for Media que está [conectada a su suscripción de Azure y a una cuenta de Azure Media Services](connect-to-azure.md). Se paga por minutos de indexación. Para más información, consulte [Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/).

Al cargar vídeos con la API de Video Analyzer for Media, tiene las siguientes opciones de carga: 

* cargar el vídeo desde una dirección URL (opción preferida);
* enviar el archivo de vídeo como una matriz de bytes en el cuerpo de la solicitud.
* Para usar el recurso de Azure Media Services existente, proporcione el [identificador del recurso](../../media-services/latest/assets-concept.md) (solo se admite en cuentas de pago).

En el artículo se muestra cómo cargar e indexar los vídeos con estas opciones:

* El sitio web de Video Analyzer for Media 
* Las API de Video Analyzer for Media 

## <a name="supported-file-formats-for-video-analyzer-for-media"></a>Formatos de archivo admitidos para Video Analyzer for Media

Consulte el artículo sobre [contenedores de entrada y formatos de archivo](../../media-services/latest/encode-media-encoder-standard-formats-reference.md) para obtener una lista de los formatos de archivo que puede usar con Video Analyzer for Media.

## <a name="video-files-storage"></a>Almacenamiento de archivos de vídeo

- Con una cuenta de pago de Video Analyzer for Media, se crea una cuenta de Video Analyzer for Media que está conectada a su suscripción de Azure y a una cuenta de Azure Media Services. Para más información, consulte [Creación de una cuenta de Video Analyzer for Media conectada a Azure](connect-to-azure.md).
- Los archivos de vídeo se almacenan en Azure Storage mediante Azure Media Services. No existe limitación de tiempo.
- Siempre tiene la opción de eliminar los archivos de audio y vídeo, así como los metadatos y la información extraída de ellos por Video Analyzer for Media. Una vez que se elimina un archivo de Video Analyzer for Media, el archivo y sus metadatos y conclusiones se quitan permanentemente de Video Analyzer for Media. Sin embargo, si ha implementado su propia solución de copia de seguridad en Azure Storage, el archivo permanece en el almacenamiento de Azure.
- La persistencia de un vídeo es idéntico, independientemente de si la carga se realiza en el sitio web de Video Analyzer for Media o mediante la API de carga.
   
## <a name="upload-and-index-a-video-using-the-video-analyzer-for-media-website"></a>Carga e indexación de un vídeo mediante el sitio web de Video Analyzer for Media

> [!NOTE]
> El nombre del vídeo no debe superar los 80 caracteres.

1. Inicie sesión en el sitio web de [Video Analyzer for Media](https://www.videoindexer.ai/).
1. Para cargar un vídeo, presione el botón **Upload** (Cargar) o el vínculo.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/video-indexer-get-started/video-indexer-upload.png" alt-text="Cargar":::
1. Una vez cargado el vídeo, Video Analyzer for Media inicia la indexación y el análisis del vídeo.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/video-indexer-get-started/progress.png" alt-text="Progreso de la carga":::
1. Cuando Video Analyzer for Media haya terminado el análisis, recibirá un correo electrónico con un vínculo al vídeo y una breve descripción de lo que se ha encontrado en él. Por ejemplo: personas, temas u OCR.

## <a name="upload-and-index-with-api"></a>Carga e indexación con API

Use la API para [Cargar vídeo](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Upload-Video) para cargar e indexar los vídeos según una dirección URL. En el ejemplo de código siguiente incluye el código con comentarios que muestra cómo cargar la matriz de bytes. 

### <a name="configurations-and-params"></a>Configuración y parámetros

En esta sección se describen algunos de los parámetros opcionales y cuándo es conveniente establecerlos. Para obtener la información más actualizada sobre parámetros, consulte la API [Upload video](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Upload-Video).

#### <a name="externalid"></a>externalId 

Este parámetro permite especificar un identificador que se asociará con el vídeo. El identificador puede aplicarse a la integración del sistema de "Administración de contenido de vídeo" (VCM) externa. Los vídeos del portal de Video Analyzer for Media se pueden buscar con el identificador externo especificado.

#### <a name="callbackurl"></a>callbackUrl

[!INCLUDE [callback url](./includes/callback-url.md)]

##### <a name="other-considerations"></a>Otras consideraciones

- Video Analyzer for Media devuelve los parámetros existentes proporcionados en la dirección URL original.
- La dirección URL proporcionada debe estar codificada.

#### <a name="indexingpreset"></a>indexingPreset

Use este parámetro para definir el paquete de inteligencia artificial que le gustaría aplicar al archivo de audio o vídeo. Este parámetro se utiliza para configurar el proceso de indexación. Puede especificar los siguientes valores:

- `AudioOnly`: indexar y extraer información detallada solo con audio (y omitir el vídeo).
- `VideoOnly`: indexar y extraer información detallada solo con vídeo (y omitir el audio).
- `Default`: indexar y extraer información detallada con audio y vídeo.
- `DefaultWithNoiseReduction`: indexar y extraer información detallada de audio y vídeo, mientras se aplican algoritmos de reducción de ruido en la secuencia de audio.

    El valor `DefaultWithNoiseReduction` se asigna ahora al valor predeterminado (en desuso).
- `BasicAudio`: indexar y extraer información detallada con solo audio (y omitir el vídeo), que incluya solo las características básicas de audio (transcripción, traducción, leyendas de salida de formato y subtítulos).
- `AdvancedAudio`: indexar y extraer información detallada solo con audio (y omitir el vídeo), que incluya las características avanzadas de audio (detección de eventos de audio), además del análisis de audio estándar.
- `AdvancedVideo` - Indexar y extraer información solo con vídeo (y omitir el audio), que incluya las características avanzadas de vídeo (Seguimiento de personas observadas), además del análisis de vídeo estándar.
- `AdvancedVideoAndAudio` - Indexar y extraer información con análisis avanzados de audio y vídeo. 

> [!NOTE]
> Los valores preestablecidos avanzados (enumerados anteriormente) incluyen modelos que se encuentran en versión preliminar pública. Una vez que estos modelos tengan disponibilidad general, podría haber implicaciones en el precio. 

Video Indexer cubre hasta dos pistas de audio. Si hay más pistas de audio en el archivo, se tratarán como una pista.<br/>
Si desea indexar las pistas por separado, tendrá que extraer el archivo de audio correspondiente e indexarlo como `AudioOnly`.

El precio depende de la opción de indexación seleccionada. Para más información, consulte [Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/).

#### <a name="priority"></a>priority

Video Analyzer for Media indexa los vídeos según su prioridad. Use el parámetro **priority** para especificar la prioridad del índice. Los siguientes valores son válidos: **Low**, **Normal** (valor predeterminado) y **High**.

El parámetro **priority** solo se admite en cuentas de pago.

#### <a name="streamingpreset"></a>streamingPreset

Una vez cargado el vídeo, Video Analyzer for Media codifica el vídeo de manera opcional. Después, continúa con la indexación y el análisis del vídeo. Cuando Video Analyzer for Media ha terminado de analizar, recibirá una notificación con el identificador de vídeo.  

Cuando se usa [Upload Video](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Upload-Video) o [Re-Index Video](https://api-portal.videoindexer.ai/api-details#api=Operations&operation=Re-Index-Video) API, uno de los parámetros opcionales es `streamingPreset`. Si establece `streamingPreset` en `Default`, `SingleBitrate` o en `AdaptiveBitrate`, se desencadenará el proceso de codificación. Una vez realizados los trabajos de indexación y codificación, el vídeo se publica para que también pueda transmitirlo. El punto de conexión de streaming desde el que va a transmitir el vídeo debe estar en estado **Running** (En ejecución).

En el caso de SingleBitrate, se aplicará el costo del codificador estándar según la salida. Si el alto de vídeo es mayor o igual que 720, Video Analyzer for Media lo codifica como 1280 x 720. De lo contrario, como 640 x 468.
La configuración predeterminada es la [codificación compatible con el contenido](../../media-services/latest/encode-content-aware-concept.md).

Para ejecutar los trabajos de indexación y codificación, la cuenta de [Azure Media Services conectada a la cuenta de Video Analyzer for Media](connect-to-azure.md) requiere unidades reservadas. Para más información, consulte [Escalado del procesamiento de elementos multimedia](../../media-services/previous/media-services-scale-media-processing-overview.md). Como son trabajos de proceso intensivo, se recomienda encarecidamente el tipo de unidad S3. El número de unidades reservadas define el número máximo de trabajos que se pueden ejecutar en paralelo. La recomendación de referencia son 10 unidades reservadas S3. 

Si solo desea indexar el vídeo pero no codificarlo, establezca `streamingPreset` en `NoStreaming`.

#### <a name="videourl"></a>videoUrl

Una dirección URL del archivo de audio y vídeo que se va a indexar. La dirección URL debe apuntar a un archivo multimedia (no se admiten páginas HTML). El archivo se puede proteger mediante un token de acceso que se proporciona como parte del URI y el punto de conexión que se aplica al archivo debe estar protegido con TLS 1.2 o una versión posterior. La dirección URL debe estar codificada. 

Si el parámetro `videoUrl` no se especifica, Video Analyzer for Media espera que pase el archivo como contenido del cuerpo de varias partes o formulario.

### <a name="code-sample"></a>Ejemplo de código

En los fragmentos de código de C# a continuación se muestra el uso de todas las API de Video Analyzer for Media juntas.

### <a name="classic-account"></a>[Cuenta clásica](#tab/With-classic-account/)

Tras copiar este código en la plataforma de desarrollo, deberá proporcionar dos parámetros: la clave de autenticación de API Management y la dirección URL del vídeo.

* Clave de API: la clave de API es la clave de suscripción personal a API Management, que le permitirá obtener un token de acceso para realizar operaciones en la cuenta de Video Analyzer for Media. 

    Para obtener la clave de API, siga este flujo:

    * Vaya a https://api-portal.videoindexer.ai/.
    * Inicio de sesión
    * Vaya a **Products** -> **Authorization** -> **Authorization subscription** (Productos > Autorización > Suscripción de autorización).
    * Copie el valor de **Primary Key** (Clave principal).
* Dirección URL del vídeo: la dirección URL del archivo de audio o vídeo que se va a indexar. La dirección URL debe apuntar a un archivo multimedia (no se admiten páginas HTML). El archivo se puede proteger mediante un token de acceso que se proporciona como parte del URI y el punto de conexión que se aplica al archivo debe estar protegido con TLS 1.2 o una versión posterior. La dirección URL debe estar codificada.

El resultado de la ejecución correcta del código de ejemplo incluirá una dirección URL del widget de información detallada y una dirección URL del widget del reproductor, que permitirán examinar la información detallada y el vídeo cargado, respectivamente. 


```csharp
public async Task Sample()
{
    var apiUrl = "https://api.videoindexer.ai";
    var apiKey = "..."; // replace with API key taken from https://aka.ms/viapi

    System.Net.ServicePointManager.SecurityProtocol =
        System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

    // create the http client
    var handler = new HttpClientHandler();
    handler.AllowAutoRedirect = false;
    var client = new HttpClient(handler);
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);

    // obtain account information and access token
    string queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"generateAccessTokens", "true"},
            {"allowEdit", "true"},
        });
    HttpResponseMessage result = await client.GetAsync($"{apiUrl}/auth/trial/Accounts?{queryParams}");
    var json = await result.Content.ReadAsStringAsync();
    var accounts = JsonConvert.DeserializeObject<AccountContractSlim[]>(json);
    
    // take the relevant account, here we simply take the first, 
    // you can also get the account via accounts.First(account => account.Id == <GUID>);
    var accountInfo = accounts.First();

    // we will use the access token from here on, no need for the apim key
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // upload a video
    var content = new MultipartFormDataContent();
    Console.WriteLine("Uploading...");
    // get the video from URL
    var videoUrl = "VIDEO_URL"; // replace with the video URL

    // as an alternative to specifying video URL, you can upload a file.
    // remove the videoUrl parameter from the query params below and add the following lines:
    //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
    //byte[] buffer =new byte[video.Length];
    //video.Read(buffer, 0, buffer.Length);
    //content.Add(new ByteArrayContent(buffer));

    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"name", "video_name"},
            {"description", "video_description"},
            {"privacy", "private"},
            {"partition", "partition"},
            {"videoUrl", videoUrl},
        });
    var uploadRequestResult = await client.PostAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos?{queryParams}", content);
    var uploadResult = await uploadRequestResult.Content.ReadAsStringAsync();

    // get the video ID from the upload result
    string videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
    Console.WriteLine("Uploaded");
    Console.WriteLine("Video ID:");
    Console.WriteLine(videoId);

    // wait for the video index to finish
    while (true)
    {
        await Task.Delay(10000);

        queryParams = CreateQueryString(
            new Dictionary<string, string>()
            {
                {"accessToken", accountInfo.AccessToken},
                {"language", "English"},
            });

        var videoGetIndexRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/Index?{queryParams}");
        var videoGetIndexResult = await videoGetIndexRequestResult.Content.ReadAsStringAsync();

        string processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

        Console.WriteLine("");
        Console.WriteLine("State:");
        Console.WriteLine(processingState);

        // job is finished
        if (processingState != "Uploaded" && processingState != "Processing")
        {
            Console.WriteLine("");
            Console.WriteLine("Full JSON:");
            Console.WriteLine(videoGetIndexResult);
            break;
        }
    }

    // search for the video
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"id", videoId},
        });

    var searchRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/Search?{queryParams}");
    var searchResult = await searchRequestResult.Content.ReadAsStringAsync();
    Console.WriteLine("");
    Console.WriteLine("Search:");
    Console.WriteLine(searchResult);

    // Generate video access token (used for get widget calls)
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var videoTokenRequestResult = await client.GetAsync($"{apiUrl}/auth/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/AccessToken?allowEdit=true");
    var videoAccessToken = (await videoTokenRequestResult.Content.ReadAsStringAsync()).Replace("\"", "");
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // get insights widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
            {"widgetType", "Keywords"},
            {"allowEdit", "true"},
        });
    var insightsWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/InsightsWidget?{queryParams}");
    var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
    Console.WriteLine("Insights Widget url:");
    Console.WriteLine(insightsWidgetLink);

    // get player widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
        });
    var playerWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/PlayerWidget?{queryParams}");
    var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
     Console.WriteLine("");
     Console.WriteLine("Player Widget url:");
     Console.WriteLine(playerWidgetLink);
     Console.WriteLine("\nPress Enter to exit...");
     String line = Console.ReadLine();
     if (line == "enter")
     {
         System.Environment.Exit(0);
     }

}

private string CreateQueryString(IDictionary<string, string> parameters)
{
    var queryParameters = HttpUtility.ParseQueryString(string.Empty);
    foreach (var parameter in parameters)
    {
        queryParameters[parameter.Key] = parameter.Value;
    }

    return queryParameters.ToString();
}

public class AccountContractSlim
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public string Location { get; set; }
    public string AccountType { get; set; }
    public string Url { get; set; }
    public string AccessToken { get; set; }
}
```

### <a name="arm-account"></a>[Cuenta de ARM](#tab/with-arm-account-account/)

Después de copiar este proyecto de C# en la plataforma de desarrollo, deberá ejecutar lo siguiente: 
1. Vaya a Program.cs y rellene ```SubscriptionId``` con el ID de suscripción.
2. Vaya a Program.cs y rellene ```ResourceGroup``` con el grupo de recursos.
3. Vaya a Program.cs y rellene ```AccountName``` con el nombre de la cuenta.
4. Vaya a Program.cs y rellene ```VideoUrl``` con la URL del vídeo.
5. Asegúrese de que dotnet 6.0 esté instalado. Si no es así, [instálela](https://dotnet.microsoft.com/download/dotnet/6.0).
6. Asegúrese de que CLI de Azure esté instalado. Si no es así, [instálela](/cli/azure/install-azure-cli).
7. Abra el terminal y vaya a la carpeta VideoIndexerArm.
8. Inicio de sesión en Azure: ```az login --use-device```
9. Compile el proyecto: ```dotnet build```
10. Ejecute el proyecto: ```dotnet run```

```csharp
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Azure.Identity" Version="1.4.1" />
    <PackageReference Include="Microsoft.Identity.Client" Version="4.36.2" />
  </ItemGroup>

</Project>
```

```csharp
using System;
using System.Collections.Generic;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text.Json;
using System.Text.Json.Serialization;
using System.Threading.Tasks;
using System.Web;
using Azure.Core;
using Azure.Identity;


namespace VideoIndexerArm
{
    public class Program
    {
        private const string AzureResourceManager = "https://management.azure.com";
        private const string SubscriptionId = ""; // Your azure subscription
        private const string ResourceGroup = ""; // Your resource group
        private const string AccountName = ""; // Your account name
        private const string VideoUrl = ""; // The video url you would like to index

        public static async Task Main(string[] args)
        {
            // Build Azure Video Analyzer for Media resource provider client that has access token throuhg ARM
            var videoIndexerResourceProviderClient = await VideoIndexerResourceProviderClient.BuildVideoIndexerResourceProviderClient();

            // Get account details
            var account = await videoIndexerResourceProviderClient.GetAccount();
            var accountId = account.Properties.Id;
            var accountLocation = account.Location;
            Console.WriteLine($"account id: {accountId}");
            Console.WriteLine($"account location: {accountLocation}");

            // Get account level access token for Azure Video Analyzer for Media 
            var accessTokenRequest = new AccessTokenRequest
            {
                PermissionType = AccessTokenPermission.Contributor,
                Scope = ArmAccessTokenScope.Account
            };

            var accessToken = await videoIndexerResourceProviderClient.GetAccessToken(accessTokenRequest);
            var apiUrl = "https://api.videoindexer.ai";
            System.Net.ServicePointManager.SecurityProtocol = System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;


            // Create the http client
            var handler = new HttpClientHandler();
            handler.AllowAutoRedirect = false;
            var client = new HttpClient(handler);

            // Upload a video
            var content = new MultipartFormDataContent();
            Console.WriteLine("Uploading...");
            // Get the video from URL

            // As an alternative to specifying video URL, you can upload a file.
            // Remove the videoUrl parameter from the query params below and add the following lines:
            // FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
            // byte[] buffer =new byte[video.Length];
            // video.Read(buffer, 0, buffer.Length);
            // content.Add(new ByteArrayContent(buffer));

            var queryParams = CreateQueryString(
                new Dictionary<string, string>()
                {
            {"accessToken", accessToken},
            {"name", "video sample"},
            {"description", "video_description"},
            {"privacy", "private"},
            {"partition", "partition"},
            {"videoUrl", VideoUrl},
                });
            var uploadRequestResult = await client.PostAsync($"{apiUrl}/{accountLocation}/Accounts/{accountId}/Videos?{queryParams}", content);
            var uploadResult = await uploadRequestResult.Content.ReadAsStringAsync();

            // Get the video ID from the upload result
            string videoId = JsonSerializer.Deserialize<Video>(uploadResult).Id;
            Console.WriteLine("Uploaded");
            Console.WriteLine("Video ID:");
            Console.WriteLine(videoId);

            // Wait for the video index to finish
            while (true)
            {
                await Task.Delay(10000);

                queryParams = CreateQueryString(
                    new Dictionary<string, string>()
                    {
                {"accessToken", accessToken},
                {"language", "English"},
                    });

                var videoGetIndexRequestResult = await client.GetAsync($"{apiUrl}/{accountLocation}/Accounts/{accountId}/Videos/{videoId}/Index?{queryParams}");
                var videoGetIndexResult = await videoGetIndexRequestResult.Content.ReadAsStringAsync();

                string processingState = JsonSerializer.Deserialize<Video>(videoGetIndexResult).State;

                Console.WriteLine("");
                Console.WriteLine("State:");
                Console.WriteLine(processingState);

                // Job is finished
                if (processingState != "Uploaded" && processingState != "Processing")
                {
                    Console.WriteLine("");
                    Console.WriteLine("Full JSON:");
                    Console.WriteLine(videoGetIndexResult);
                    break;
                }
            }

            // Search for the video
            queryParams = CreateQueryString(
                new Dictionary<string, string>()
                {
            {"accessToken", accessToken},
            {"id", videoId},
                });

            var searchRequestResult = await client.GetAsync($"{apiUrl}/{accountLocation}/Accounts/{accountId}/Videos/Search?{queryParams}");
            var searchResult = await searchRequestResult.Content.ReadAsStringAsync();
            Console.WriteLine("");
            Console.WriteLine("Search:");
            Console.WriteLine(searchResult);

            // Get insights widget url
            queryParams = CreateQueryString(
                new Dictionary<string, string>()
                {
            {"accessToken", accessToken},
            {"widgetType", "Keywords"},
            {"allowEdit", "true"},
                });
            var insightsWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountLocation}/Accounts/{accountId}/Videos/{videoId}/InsightsWidget?{queryParams}");
            var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
            Console.WriteLine("Insights Widget url:");
            Console.WriteLine(insightsWidgetLink);

            // Get player widget url
            queryParams = CreateQueryString(
                new Dictionary<string, string>()
                {
            {"accessToken", accessToken},
                });
            var playerWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountLocation}/Accounts/{accountId}/Videos/{videoId}/PlayerWidget?{queryParams}");
            var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
            Console.WriteLine("");
            Console.WriteLine("Player Widget url:");
            Console.WriteLine(playerWidgetLink);
            Console.WriteLine("\nPress Enter to exit...");
            String line = Console.ReadLine();
            if (line == "enter")
            {
                System.Environment.Exit(0);
            }

        }

        static string CreateQueryString(IDictionary<string, string> parameters)
        {
            var queryParameters = HttpUtility.ParseQueryString(string.Empty);
            foreach (var parameter in parameters)
            {
                queryParameters[parameter.Key] = parameter.Value;
            }

            return queryParameters.ToString();
        }

        public class VideoIndexerResourceProviderClient
        {
            private readonly string armAaccessToken;

            async public static Task<VideoIndexerResourceProviderClient> BuildVideoIndexerResourceProviderClient()
            {
                var tokenRequestContext = new TokenRequestContext(new[] { $"{AzureResourceManager}/.default" });
                var tokenRequestResult = await new DefaultAzureCredential().GetTokenAsync(tokenRequestContext);
                return new VideoIndexerResourceProviderClient(tokenRequestResult.Token);
            }
            public VideoIndexerResourceProviderClient(string armAaccessToken)
            {
                this.armAaccessToken = armAaccessToken;
            }

            public async Task<string> GetAccessToken(AccessTokenRequest accessTokenRequest)
            {
                Console.WriteLine($"Getting access token. {JsonSerializer.Serialize(accessTokenRequest)}");
                // Set the generateAccessToken (from video indexer) http request content
                var jsonRequestBody = JsonSerializer.Serialize(accessTokenRequest);
                var httpContent = new StringContent(jsonRequestBody, System.Text.Encoding.UTF8, "application/json");

                // Set request uri
                var requestUri = $"{AzureResourceManager}/subscriptions/{SubscriptionId}/resourcegroups/{ResourceGroup}/providers/Microsoft.VideoIndexer/accounts/{AccountName}/generateAccessToken?api-version=2021-08-16-preview";

                // Generate access token from video indexer
                var client = new HttpClient(new HttpClientHandler());
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", armAaccessToken);
                var result = await client.PostAsync(requestUri, httpContent);
                var jsonResponseBody = await result.Content.ReadAsStringAsync();
                return JsonSerializer.Deserialize<GenerateAccessTokenResponse>(jsonResponseBody).AccessToken;
            }

            public async Task<Account> GetAccount()
            {

                Console.WriteLine($"Getting account.");
                // Set request uri
                var requestUri = $"{AzureResourceManager}/subscriptions/{SubscriptionId}/resourcegroups/{ResourceGroup}/providers/Microsoft.VideoIndexer/accounts/{AccountName}/?api-version=2021-08-16-preview";

                // Get account
                var client = new HttpClient(new HttpClientHandler());
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", armAaccessToken);
                var result = await client.GetAsync(requestUri);
                var jsonResponseBody = await result.Content.ReadAsStringAsync();
                return JsonSerializer.Deserialize<Account>(jsonResponseBody);
            }
        }

        public class AccessTokenRequest
        {
            [JsonPropertyName("permissionType")]
            public AccessTokenPermission PermissionType { get; set; }

            [JsonPropertyName("scope")]
            public ArmAccessTokenScope Scope { get; set; }

            [JsonPropertyName("projectId")]
            public string ProjectId { get; set; }
            
            [JsonPropertyName("videoId")]
            public string VideoId { get; set; }
        }

        [JsonConverter(typeof(JsonStringEnumConverter))]
        public enum AccessTokenPermission
        {
            Reader,
            Contributor,
            MyAccessAdministrator,
            Owner,
        }

        [JsonConverter(typeof(JsonStringEnumConverter))]
        public enum ArmAccessTokenScope
        {
            Account,
            Project,
            Video
        }

        public class GenerateAccessTokenResponse
        {
            [JsonPropertyName("accessToken")]
            public string AccessToken { get; set; }

        }
        public class AccountProperties
        {
            [JsonPropertyName("accountId")]
            public string Id { get; set; }
        }

        public class Account
        {
            [JsonPropertyName("properties")]
            public AccountProperties Properties { get; set; }

            [JsonPropertyName("location")]
            public string Location { get; set; }

        }

        public class Video
        {
            [JsonPropertyName("id")]
            public string Id { get; set; }

            [JsonPropertyName("state")]
            public string State { get; set; }
        }
    }
}

```

### <a name="common-errors"></a>Errores comunes

La operación Upload puede devolver los códigos de estado que aparecen en la siguiente tabla.

|status code|ErrorType (en el cuerpo de la respuesta)|Descripción|
|---|---|---|
|409|VIDEO_INDEXING_IN_PROGRESS|El mismo vídeo ya se está procesando en la cuenta especificada.|
|400|VIDEO_ALREADY_FAILED|El mismo vídeo no se pudo procesar en la cuenta especificada hace menos de 2 horas. Los clientes de API deberán esperar al menos 2 horas antes de volver a cargar un vídeo.|
|429||Las cuentas de prueba tienen permitido realizar hasta 5 cargas por minuto. Las cuentas de prueba tienen permitido realizar hasta 50 cargas por minuto.|

## <a name="uploading-considerations-and-limitations"></a>Consideraciones y limitaciones para la carga
 
- El nombre del vídeo no debe superar los 80 caracteres.
- Al cargar el vídeo según la dirección URL (opción preferida) el punto de conexión debe estar protegido con TLS 1.2 (o una versión posterior).
- El tamaño de carga con la opción de dirección URL se limita a 30 GB.
- La longitud de la dirección URL de la solicitud está limitada a 6144 caracteres en los que la longitud de la dirección URL de la cadena de consulta está limitada a 4096 caracteres.
- El tamaño de carga con la opción de matriz de bytes se limita a 2 GB.
- La opción de matriz de bytes agota el tiempo de espera a los 30 minutos.
- La dirección URL que se proporciona en el parámetro `videoURL` debe estar codificada.
- La indexación de los recursos de Media Services tiene la misma limitación que la indexación desde la dirección URL.
- Video Analyzer for Media tiene una duración máxima de 4 horas por archivo.
- Se debe poder acceder a la dirección URL (por ejemplo, una dirección URL pública). 

    Si se trata de una dirección URL privada, es necesario especificar el token de acceso en la solicitud.
- La dirección URL tiene que apuntar a un archivo multimedia válido, no a una página web, como un vínculo a la página `www.youtube.com`.
- En una cuenta de pago, puede cargar hasta 50 películas por minuto y en una cuenta de prueba hasta 5 películas por minuto.

> [!Tip]
> Es recomendable que use .NET Framework versión 4.6.2 o superior porque las versiones anteriores de .NET Framework no usan de forma predeterminada TLS 1.2.
>
> Si debe usar versiones anteriores de .NET Framework, agregue una línea en el código antes de realizar la llamada a la API REST:  <br/> System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls12;

## <a name="firewall"></a>Firewall

Consulte [Cuenta de almacenamiento que está detrás de un firewall](faq.yml#can-a-storage-account-connected-to-the-media-services-account-be-behind-a-firewall).

## <a name="next-steps"></a>Pasos siguientes

[Examen de la salida de Azure Video Analyzer for Media producida por la API](video-indexer-output-json-v2.md)
