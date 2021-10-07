---
title: Configuración de una aplicación web que llama a las API web | Azure
titleSuffix: Microsoft identity platform
description: Aprenda a configurar el código de una aplicación web que llama a las API web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/25/2020
ms.author: jmprieur
ms.custom: aaddev, devx-track-python
ms.openlocfilehash: e9a5b288976d375d9f773fee3dc1ea671ed902b4
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124786529"
---
# <a name="a-web-app-that-calls-web-apis-code-configuration"></a>Aplicación web que llama a las API web: Configuración del código

Como se puede observar en el escenario de [Aplicación web que permite iniciar sesión a los usuarios](scenario-web-app-sign-user-overview.md), la aplicación web usa el [flujo de código de autorización](v2-oauth2-auth-code-flow.md) de OAuth 2.0 para iniciar la sesión del usuario. Este flujo tiene dos pasos:

1. Solicitud de un código de autorización. Esta parte delega a la Plataforma de identidad de Microsoft un diálogo privado con el usuario. Durante ese diálogo, el usuario inicia sesión y da su consentimiento al uso de las API web. Cuando este diálogo privado finaliza correctamente, la aplicación web recibe un código de autorización en el URI de redirección.
1. Solicite un token de acceso para la API mediante el canje del código de autorización.

Los escenarios de [aplicación web que permite iniciar sesión a los usuarios](scenario-web-app-sign-user-overview.md) abarcaba solo el primer paso. Aquí aprenderá a modificar la aplicación web para que no solo inicie la sesión de los usuarios, sino que también llame a las API web.

## <a name="microsoft-libraries-supporting-web-apps"></a>Bibliotecas de Microsoft que admiten aplicaciones web

Las bibliotecas de Microsoft siguientes admiten aplicaciones web:

[!INCLUDE [active-directory-develop-libraries-webapp](../../../includes/active-directory-develop-libraries-webapp.md)]

Seleccione la pestaña correspondiente a la plataforma que le interese:

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

## <a name="client-secrets-or-client-certificates"></a>Secretos de cliente o certificados de cliente

Dado que la aplicación web ahora llama a una API web de nivel inferior, proporcione un secreto de cliente o un certificado de cliente en el archivo *appsettings.json*. También puede agregar una sección que especifique:

- La dirección URL de la API web de nivel inferior.
- Los ámbitos necesarios para llamar a la API.

En el ejemplo siguiente, la sección `GraphBeta` especifica esta configuración.

```JSON
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-app-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    "TenantId": "common"

   // To call an API
   "ClientSecret": "[Copy the client secret added to the app from the Azure portal]",
   "ClientCertificates": [
  ]
 },
 "GraphBeta": {
    "BaseUrl": "https://graph.microsoft.com/beta",
    "Scopes": "user.read"
    }
}
```

En lugar de un secreto de cliente, puede proporcionar un certificado de cliente. En el fragmento de código siguiente se muestra el uso de un certificado almacenado en Azure Key Vault.

```JSON
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-app-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    "TenantId": "common"

   // To call an API
   "ClientCertificates": [
      {
        "SourceType": "KeyVault",
        "KeyVaultUrl": "https://msidentitywebsamples.vault.azure.net",
        "KeyVaultCertificateName": "MicrosoftIdentitySamplesCert"
      }
   ]
  },
  "GraphBeta": {
    "BaseUrl": "https://graph.microsoft.com/beta",
    "Scopes": "user.read"
  }
}
```

*Microsoft.Identity.Web* proporciona varias maneras de describir certificados, tanto mediante configuración como mediante código. Para más información, consulte [Microsoft.Identity.Web: uso de certificados](https://github.com/AzureAD/microsoft-identity-web/wiki/Using-certificates) en GitHub.

## <a name="startupcs"></a>Startup.cs

La aplicación web tendrá que adquirir un token para la API de nivel inferior. Para especificarlo, agregue la línea `.EnableTokenAcquisitionToCallDownstreamApi()` después de `.AddMicrosoftIdentityWebApi(Configuration)`. Esta línea expone el servicio `ITokenAcquisition`, que puede usar en las acciones del controlador y la página. Sin embargo, como verá en las dos opciones siguientes, se puede hacer de forma más sencilla. También deberá elegir una implementación de la caché de tokens, por ejemplo `.AddInMemoryTokenCaches()`, en *Startup.cs*:

   ```csharp
   using Microsoft.Identity.Web;

   public class Startup
   {
     // ...
     public void ConfigureServices(IServiceCollection services)
     {
     // ...
     services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
             .AddMicrosoftIdentityWebApp(Configuration, Configuration.GetSection("AzureAd"))
               .EnableTokenAcquisitionToCallDownstreamApi(new string[]{"user.read" })
               .AddInMemoryTokenCaches();
      // ...
     }
     // ...
   }
   ```

Los ámbitos que se pasan a `EnableTokenAcquisitionToCallDownstreamApi` son opcionales y permiten que la aplicación web solicite los ámbitos y el consentimiento del usuario a esos ámbitos cuando inicie sesión. Si no especifica los ámbitos, *Microsoft.Identity.Web* habilitará una experiencia de consentimiento incremental.

Si no desea adquirir el token por sí mismo, *Microsoft.Identity.Web* proporciona dos mecanismos para llamar a una API web desde una aplicación web. La opción que elija dependerá de si desea llamar a Microsoft Graph o a otra API.

### <a name="option-1-call-microsoft-graph"></a>Opción 1: Llamada a Microsoft Graph

Si desea llamar a Microsoft Graph, *Microsoft.Identity.Web* le permite usar directamente `GraphServiceClient` (expuesto por el SDK de Microsoft Graph) en las acciones de la API. Para exponer Microsoft Graph:

1. Agregue el paquete NuGet [Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph) al proyecto.
1. Agregue `.AddMicrosoftGraph()` después de `.EnableTokenAcquisitionToCallDownstreamApi()` en el archivo *Startup.cs*. `.AddMicrosoftGraph()` tiene varias invalidaciones. Cuando se utiliza la invalidación que toma una sección de la configuración como parámetro, el código se convierte en:

   ```csharp
   using Microsoft.Identity.Web;

   public class Startup
   {
     // ...
     public void ConfigureServices(IServiceCollection services)
     {
     // ...
     services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
             .AddMicrosoftIdentityWebApp(Configuration, Configuration.GetSection("AzureAd"))
               .EnableTokenAcquisitionToCallDownstreamApi(new string[]{"user.read" })
                  .AddMicrosoftGraph(Configuration.GetSection("GraphBeta"))
               .AddInMemoryTokenCaches();
      // ...
     }
     // ...
   }
   ```

### <a name="option-2-call-a-downstream-web-api-other-than-microsoft-graph"></a>Opción 2: Llamada a una API web de nivel inferior que no sea Microsoft Graph

Para llamar a una API web que no sea Microsoft Graph, *Microsoft.Identity.Web* proporciona `.AddDownstreamWebApi()`, que solicita tokens y llama a la API web de nivel inferior.

   ```csharp
   using Microsoft.Identity.Web;

   public class Startup
   {
     // ...
     public void ConfigureServices(IServiceCollection services)
     {
     // ...
     services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
             .AddMicrosoftIdentityWebApp(Configuration, "AzureAd")
               .EnableTokenAcquisitionToCallDownstreamApi(new string[]{"user.read" })
                  .AddDownstreamWebApi("MyApi", Configuration.GetSection("GraphBeta"))
               .AddInMemoryTokenCaches();
      // ...
     }
     // ...
   }
   ```

### <a name="summary"></a>Resumen

Al igual que con las API web, puede elegir varias implementaciones de la memoria caché de tokens. Para más información, consulte [Microsoft.Identity.Web: serialización de la memoria caché de tokens](https://aka.ms/ms-id-web/token-cache-serialization) en GitHub.

En la imagen siguiente se muestran las distintas posibilidades de *Microsoft.Identity.Web* y su impacto en el archivo *Startup.cs*:

:::image type="content" source="media/scenarios/microsoft-identity-web-startup-cs.svg" alt-text="Diagrama de bloques que muestra las opciones de configuración del servicio en startup punto c s para llamar a una API web y especificar una implementación de la caché de tokens":::

> [!NOTE]
> Para comprender por completo los ejemplos de código que se indican a continuación, familiarícese con los [aspectos básicos de ASP.NET Core](/aspnet/core/fundamentals) y, en particular, con la [inserción de dependencias](/aspnet/core/fundamentals/dependency-injection) y las [opciones](/aspnet/core/fundamentals/configuration/options).

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Dado que el usuario que inicia sesión se delega al middleware de OpenID Connect (OIDC), debe interactuar con el proceso de OIDC. La forma de interactuar dependerá del marco que use.

En el caso de ASP.NET, se suscribirá a los eventos middleware de OIDC:

- Permitirá que ASP.NET Core solicite un código de autorización mediante el middleware de Open ID Connect. ASP.NET o ASP.NET Core permitirán al usuario iniciar sesión y dar su consentimiento.
- Suscribirá la aplicación web para recibir el código de autorización. Esta suscripción se realiza mediante un delegado de C#.
- Cuando se recibe el código de autenticación, se usarán las bibliotecas de MSAL para canjearlo. Los tokens de acceso y los tokens de actualización resultantes se almacenan en la caché de tokens. La caché puede utilizarse en otras partes de la aplicación, como controladores, para adquirir otros tokens de forma silenciosa.

Los ejemplos de código de este artículo y el siguiente se han extraído del [ejemplo de aplicación web de ASP.NET](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect). Es posible que desee consultar dicho ejemplo para obtener detalles completos de la implementación.

# <a name="java"></a>[Java](#tab/java)

Los ejemplos de código de este artículo y el siguiente se han extraído de la [aplicación web de Java que llama a Microsoft Graph](https://github.com/Azure-Samples/ms-identity-java-webapp), un ejemplo de aplicación web que usa MSAL para Java.
El ejemplo permite actualmente que MSAL para Java genere la dirección URL del código de autorización y controla la navegación al punto de conexión de autorización para la Plataforma de identidad de Microsoft. También es posible usar la seguridad de Sprint para iniciar la sesión del usuario. Es posible que desee consultar dicho ejemplo para obtener detalles completos de la implementación.

# <a name="python"></a>[Python](#tab/python)

Los ejemplos de código de este artículo y el siguiente se han extraído de la [aplicación web de Python que llama a Microsoft Graph](https://github.com/Azure-Samples/ms-identity-python-webapp), un ejemplo de aplicación web que usa MSAL.Python.
El ejemplo permite actualmente que MSAL.Python genere la dirección URL del código de autorización y controla la navegación al punto de conexión de autorización de la Plataforma de identidad de Microsoft. Es posible que desee consultar dicho ejemplo para obtener detalles completos de la implementación.

---

## <a name="code-that-redeems-the-authorization-code"></a>El código que canjea el código de autorización.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Microsoft.Identity.Web simplifica el código mediante la definición de la configuración correcta de OpenID Connect, la suscripción al evento recibido del código y el canje del código. No es necesario ningún código adicional para canjear el código de autorización. Vea [Código fuente de Microsoft.Identity.Web](https://github.com/AzureAD/microsoft-identity-web/blob/c29f1a7950b940208440bebf0bcb524a7d6bee22/src/Microsoft.Identity.Web/WebAppExtensions/WebAppCallsWebApiAuthenticationBuilderExtensions.cs#L140) para obtener información detallada sobre cómo funciona esto.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET controla las cosas de forma similar, a ASP.NET Core, con la excepción de que la configuración de OpenIdConnect y la suscripción al evento `OnAuthorizationCodeReceived` se produce en el archivo [App_Start\Startup.Auth.cs](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs). Los conceptos también son similares a los de ASP.NET Core, salvo que en ASP.NET debe especificar `RedirectUri` en [Web.config#L15](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/master/WebApp/Web.config#L15). Esta configuración es un poco menos robusta que la de ASP.NET Core, ya que deberá cambiarla al implementar la aplicación.

Este es el código de Startup.Auth.cs:

```csharp
public partial class Startup
{
  public void ConfigureAuth(IAppBuilder app)
  {
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    // Custom middleware initialization. This is activated when the code obtained from a code_grant is present in the query string (&code=<code>).
    app.UseOAuth2CodeRedeemer(
        new OAuth2CodeRedeemerOptions
        {
            ClientId = AuthenticationConfig.ClientId,
            ClientSecret = AuthenticationConfig.ClientSecret,
            RedirectUri = AuthenticationConfig.RedirectUri
        }
      );

  app.UseOpenIdConnectAuthentication(
      new OpenIdConnectAuthenticationOptions
      {
        // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0.
        Authority = AuthenticationConfig.Authority,
        ClientId = AuthenticationConfig.ClientId,
        RedirectUri = AuthenticationConfig.RedirectUri,
        PostLogoutRedirectUri = AuthenticationConfig.RedirectUri,
        Scope = AuthenticationConfig.BasicSignInScopes + " Mail.Read", // A basic set of permissions for user sign-in and profile access "openid profile offline_access"
        TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = false,
            // In a real application, you would use IssuerValidator for additional checks, such as making sure the user's organization has signed up for your app.
            //     IssuerValidator = (issuer, token, tvp) =>
            //     {
            //        //if(MyCustomTenantValidation(issuer))
            //        return issuer;
            //        //else
            //        //    throw new SecurityTokenInvalidIssuerException("Invalid issuer");
            //    },
            //NameClaimType = "name",
        },
        Notifications = new OpenIdConnectAuthenticationNotifications()
        {
            AuthorizationCodeReceived = OnAuthorizationCodeReceived,
            AuthenticationFailed = OnAuthenticationFailed,
        }
      });
  }

  private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification context)
  {
      // Upon successful sign-in, get the access token and cache it by using MSAL.
      IConfidentialClientApplication clientApp = MsalAppBuilder.BuildConfidentialClientApplication(new ClaimsPrincipal(context.AuthenticationTicket.Identity));
      AuthenticationResult result = await clientApp.AcquireTokenByAuthorizationCode(new[] { "Mail.Read" }, context.Code).ExecuteAsync();
  }

  private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
  {
      notification.HandleResponse();
      notification.Response.Redirect("/Error?message=" + notification.Exception.Message);
      return Task.FromResult(0);
  }
}
```

# <a name="java"></a>[Java](#tab/java)

Consulte [Aplicación web que permite iniciar sesión a los usuarios: configuración del código](scenario-web-app-sign-user-app-configuration.md?tabs=java#initialization-code) para comprender cómo el ejemplo de Java obtiene el código de autorización. Una vez que la aplicación recibe el código, [AuthFilter.java#L51-L56](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java#L51-L56):

1. Delega en el método `AuthHelper.processAuthenticationCodeRedirect` en [AuthHelper.java#L67-L97](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L67-L97).
1. Llama a `getAuthResultByAuthCode`.

```Java
class AuthHelper {
  // Code omitted
  void processAuthenticationCodeRedirect(HttpServletRequest httpRequest, String currentUri, String fullUrl)
            throws Throwable {

  // Code omitted
  AuthenticationResponse authResponse = AuthenticationResponseParser.parse(new URI(fullUrl), params);

  // Code omitted
  IAuthenticationResult result = getAuthResultByAuthCode(
                    httpRequest,
                    oidcResponse.getAuthorizationCode(),
                    currentUri);

// Code omitted
  }
}
```

El método `getAuthResultByAuthCode` se define en [AuthHelper.java#L176](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L176). Crea un MSAL `ConfidentialClientApplication` y luego llama a `acquireToken()` con `AuthorizationCodeParameters` creado a partir del código de autorización.

```Java
   private IAuthenticationResult getAuthResultByAuthCode(
            HttpServletRequest httpServletRequest,
            AuthorizationCode authorizationCode,
            String currentUri) throws Throwable {

        IAuthenticationResult result;
        ConfidentialClientApplication app;
        try {
            app = createClientApplication();

            String authCode = authorizationCode.getValue();
            AuthorizationCodeParameters parameters = AuthorizationCodeParameters.builder(
                    authCode,
                    new URI(currentUri)).
                    build();

            Future<IAuthenticationResult> future = app.acquireToken(parameters);

            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        }

        if (result == null) {
            throw new ServiceUnavailableException("authentication result was null");
        }

        SessionManagementHelper.storeTokenCacheInSession(httpServletRequest, app.tokenCache().serialize());

        return result;
    }

    private ConfidentialClientApplication createClientApplication() throws MalformedURLException {
        return ConfidentialClientApplication.builder(clientId, ClientCredentialFactory.create(clientSecret)).
                authority(authority).
                build();
    }
```

# <a name="python"></a>[Python](#tab/python)

El flujo de código de autorización se solicita como se muestra en [Aplicación web que permite iniciar sesión a los usuarios: configuración del código](scenario-web-app-sign-user-app-configuration.md?tabs=python#initialization-code). A continuación, el código se recibe en la función de `authorized`, que Flask redirige desde la dirección URL de `/getAToken`. Consulte [app.py#L30-L44](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L30-L44) para ver el contexto completo de este código:

```python
 @app.route("/getAToken")  # Its absolute URL must match your app's redirect_uri set in AAD.
def authorized():
    if request.args['state'] != session.get("state"):
        return redirect(url_for("login"))
    cache = _load_cache()
    result = _build_msal_app(cache).acquire_token_by_authorization_code(
        request.args['code'],
        scopes=app_config.SCOPE,  # Misspelled scope would cause an HTTP 400 error here.
        redirect_uri=url_for("authorized", _external=True))
    if "error" in result:
        return "Login failure: %s, %s" % (
            result["error"], result.get("error_description"))
    session["user"] = result.get("id_token_claims")
    _save_cache(cache)
    return redirect(url_for("index"))
```

---

En lugar de un secreto de cliente, la aplicación cliente confidencial también puede demostrar su identidad mediante un certificado de cliente o una aserción de cliente.
El uso de aserciones de cliente es un escenario avanzado, que se detalla en [Aserciones de cliente](msal-net-client-assertions.md).

## <a name="token-cache"></a>Memoria caché de tokens

> [!IMPORTANT]
> La implementación de la caché de tokens para aplicaciones web o API web es distinta a la implementación de las aplicaciones de escritorio, que a menudo están [basadas en archivos](msal-net-token-cache-serialization.md).
> Por motivos de seguridad y rendimiento, es importante asegurarse de que, para las aplicaciones web y las API web, haya una caché de tokens por cuenta de usuario. Es preciso serializar la caché de tokens de cada cuenta.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

En el tutorial de ASP.NET Core se usa la inserción de dependencias para permitir decidir la implementación de la caché de tokens en el archivo Startup.cs de la aplicación. Microsoft.Identity.Web incluye varios serializadores de caché de tokens pregenerados que se describen en [Serialización del almacenamiento en caché de los tokens](msal-net-token-cache-serialization.md). Una posibilidad interesante es elegir las [cachés de memoria distribuidas](/aspnet/core/performance/caching/distributed#distributed-memory-cache) de ASP.NET Core:

```csharp
// Use a distributed token cache by adding:
    services.AddMicrosoftIdentityWebAppAuthentication(Configuration, "AzureAd")
            .EnableTokenAcquisitionToCallDownstreamApi(
                initialScopes: new string[] { "user.read" })
            .AddDistributedTokenCaches();

// Then, choose your implementation.
// For instance, the distributed in-memory cache (not cleared when you stop the app):
services.AddDistributedMemoryCache();

// Or a Redis cache:
services.AddStackExchangeRedisCache(options =>
{
 options.Configuration = "localhost";
 options.InstanceName = "SampleInstance";
});

// Or even a SQL Server token cache:
services.AddDistributedSqlServerCache(options =>
{
 options.ConnectionString = _config["DistCache_ConnectionString"];
 options.SchemaName = "dbo";
 options.TableName = "TestCache";
});
```

Para información detallada sobre los proveedores de cachés de tokens, vea también el artículo [Serialización de la caché de tokens](https://aka.ms/ms-id-web/token-cache-serialization) de Microsoft.Identity.Web y el tutorial de la fase de las aplicaciones web de [Tutoriales de aplicación web de ASP.NET Core | Cachés de tokens](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache).

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

La implementación de la caché de tokens para aplicaciones web o API web es distinta a la implementación de las aplicaciones de escritorio, que a menudo están [basadas en archivos](msal-net-token-cache-serialization.md).

La implementación de la aplicación web puede usar la sesión ASP.NET o la memoria del servidor. Vea por ejemplo cómo se enlaza la implementación de la memoria caché después de la creación de la aplicación de MSAL.NET en [MsalAppBuilder.cs#L39-L51](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/79e3e1f084cd78f9170a8ca4077869f217735a1a/WebApp/Utils/MsalAppBuilder.cs#L57-L58):


En primer lugar, para usar estas implementaciones:
- Agregue el paquete NuGet Microsoft.Identity.Web. Estos serializadores de caché de tokens no se incorporan en MSAL.NET directamente para evitar dependencias no deseadas. Además de un nivel superior para ASP.NET Core, Microsoft.Identity.Web incorpora clases que son asistentes para MSAL.NET, 
- En el código, use el espacio de nombres Microsoft.Identity.Web:

  ```csharp
  #using Microsoft.Identity.Web
  ```
- Una vez que haya creado la aplicación cliente confidencial, agregue la serialización de la caché de tokens que prefiera.

```csharp
public static class MsalAppBuilder
{
  private static IConfidentialClientApplication clientapp;

  public static IConfidentialClientApplication BuildConfidentialClientApplication()
  {
    if (clientapp == null)
    {
      clientapp = ConfidentialClientApplicationBuilder.Create(AuthenticationConfig.ClientId)
            .WithClientSecret(AuthenticationConfig.ClientSecret)
            .WithRedirectUri(AuthenticationConfig.RedirectUri)
            .WithAuthority(new Uri(AuthenticationConfig.Authority))
            .Build();

      // After the ConfidentialClientApplication is created, we overwrite its default UserTokenCache serialization with our implementation
      clientapp.AddInMemoryTokenCache();
    }
    return clientapp;
  }
```

En lugar de `clientapp.AddInMemoryTokenCache()`, también puede usar implementaciones de serialización de caché más avanzadas, como Redis, SQL, CosmosDB o memoria distribuida. Este es un ejemplo para Redis:

```csharp
  clientapp.AddDistributedTokenCache(services =>
  {
    services.AddStackExchangeRedisCache(options =>
    {
        options.Configuration = "localhost";
        options.InstanceName = "SampleInstance";
    });
  });
```

Para más información, vea [Serialización de la memoria caché de tokens en MASL.NET](./msal-net-token-cache-serialization.md).

# <a name="java"></a>[Java](#tab/java)

Java de MSAL proporciona métodos para serializar y deserializar la caché de tokens. En el ejemplo de Java se controla la serialización de la sesión, como se muestra en el método `getAuthResultBySilentFlow` de [AuthHelper.java#L99-L122](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L99-L122):

```Java
IAuthenticationResult getAuthResultBySilentFlow(HttpServletRequest httpRequest, HttpServletResponse httpResponse)
      throws Throwable {

  IAuthenticationResult result =  SessionManagementHelper.getAuthSessionObject(httpRequest);

  IConfidentialClientApplication app = createClientApplication();

  Object tokenCache = httpRequest.getSession().getAttribute("token_cache");
  if (tokenCache != null) {
      app.tokenCache().deserialize(tokenCache.toString());
  }

  SilentParameters parameters = SilentParameters.builder(
          Collections.singleton("User.Read"),
          result.account()).build();

  CompletableFuture<IAuthenticationResult> future = app.acquireTokenSilently(parameters);
  IAuthenticationResult updatedResult = future.get();

  // Update session with latest token cache.
  SessionManagementHelper.storeTokenCacheInSession(httpRequest, app.tokenCache().serialize());

  return updatedResult;
}
```

Los detalles de la clase `SessionManagementHelper` se proporcionan en el [ejemplo de MSAL para Java](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/SessionManagementHelper.java).

# <a name="python"></a>[Python](#tab/python)

En el ejemplo de Python, se garantiza una memoria caché por cuenta mediante la recreación de una aplicación cliente confidencial para cada solicitud y su posterior serialización en la memoria caché de la sesión de Flask:

```python
from flask import Flask, render_template, session, request, redirect, url_for
from flask_session import Session  # https://pythonhosted.org/Flask-Session
import msal
import app_config


app = Flask(__name__)
app.config.from_object(app_config)
Session(app)

# Code omitted here for simplicity

def _load_cache():
    cache = msal.SerializableTokenCache()
    if session.get("token_cache"):
        cache.deserialize(session["token_cache"])
    return cache

def _save_cache(cache):
    if cache.has_state_changed:
        session["token_cache"] = cache.serialize()

def _build_msal_app(cache=None):
    return msal.ConfidentialClientApplication(
        app_config.CLIENT_ID, authority=app_config.AUTHORITY,
        client_credential=app_config.CLIENT_SECRET, token_cache=cache)
```

---

## <a name="next-steps"></a>Pasos siguientes

En este momento, cuando el usuario inicia sesión, un token se almacena en la caché de tokens. A continuación, veremos cómo se usa en otras partes de la aplicación web.

[Eliminación de cuentas de la caché de tokens durante el cierre de sesión global](scenario-web-app-call-api-sign-in.md)