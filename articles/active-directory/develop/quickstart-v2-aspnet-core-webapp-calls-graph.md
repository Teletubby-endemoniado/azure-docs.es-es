---
title: 'Inicio rápido: aplicación web ASP.NET Core que inicia la sesión de los usuarios y llama a Microsoft Graph | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, obtendrá información sobre la forma en que una aplicación usa Microsoft.Identity.Web para implementar el inicio de sesión de Microsoft en una aplicación web ASP.NET Core mediante OpenID Connect y llama a Microsoft Graph.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.workload: identity
ms.date: 11/22/2021
ms.author: jmprieur
ms.custom: devx-track-csharp, aaddev, "scenarios:getting-started", "languages:aspnet-core", mode-other
ms.openlocfilehash: 887a1e072f2ea9325fa1113446c57e5d68dbd691
ms.sourcegitcommit: 34d047300d800cf6ff7d9dd3e573a0d785f61abc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2022
ms.locfileid: "135935317"
---
# <a name="quickstart-aspnet-core-web-app-that-signs-in-users-and-calls-microsoft-graph-on-their-behalf"></a>Inicio rápido: aplicación web ASP.NET Core que inicia la sesión de los usuarios y llama a Microsoft Graph en su nombre

En este inicio rápido, descargará y ejecutará un código de ejemplo que muestra la forma en que una aplicación web de ASP.NET Core puede realizar el inicio de sesión de los usuarios desde cualquier organización de Azure Active Directory y llama a Microsoft Graph.

Para ilustrar este tema, consulte el apartado en el que se explica el [funcionamiento del ejemplo](#how-the-sample-works).

## <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal

Para que el código de ejemplo de este inicio rápido funcione, agregue un **URI de redirección**  de `https://localhost:44321/signin-oidc` y un valor para **Front-channel logout URL** (Dirección URL de cierre de sesión del canal frontal) de `https://localhost:44321/signout-oidc` en el registro de la aplicación.
> [!div class="nextstepaction"]
> [Hacer este cambio por mí]()

> [!div class="alert alert-info"]
> ![Ya configurada](media/quickstart-v2-aspnet-webapp/green-check.png) La aplicación está configurada con estos atributos.

## <a name="step-2-download-the-aspnet-core-project"></a>Paso 2: Descarga del proyecto de ASP.NET Core

Ejecute el proyecto.

> [!div class="nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1-callsgraph.zip)

[!INCLUDE [active-directory-develop-path-length-tip](../../../includes/active-directory-develop-path-length-tip.md)]


## <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Paso 3: La aplicación está configurada y lista para ejecutarse

Hemos configurado el proyecto con los valores de las propiedades de su aplicación y está preparado para ejecutarse.

> [!NOTE]
> `Enter_the_Supported_Account_Info_Here`

## <a name="about-the-code"></a>Sobre el código

En esta sección se proporciona información general sobre el código necesario para iniciar sesión a los usuarios y llamar a Microsoft Graph API en su nombre. Esta descripción puede ser útil para conocer el funcionamiento del código, los argumentos principales y si se desea agregar el inicio de sesión a una aplicación ASP.NET Core existente y agregar Microsoft Graph. Utiliza [Microsoft.Identity.Web](microsoft-identity-web.md), que es un contenedor de [MSAL.NET](msal-overview.md).

### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo

![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-aspnet-core-webapp-calls-graph/aspnetcorewebapp-intro.svg)

### <a name="startup-class"></a>Clase Startup

El middleware *Microsoft.AspNetCore.Authentication* usa una clase `Startup` que se ejecuta cuando se inicializa el proceso de hospedaje:

```csharp

  public void ConfigureServices(IServiceCollection services)
  {  
    // Get the scopes from the configuration (appsettings.json)
    var initialScopes = Configuration.GetValue<string>("DownstreamApi:Scopes")?.Split(' ');
  
      // Add sign-in with Microsoft
      services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
        .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"))

            // Add the possibility of acquiring a token to call a protected web API
            .EnableTokenAcquisitionToCallDownstreamApi(initialScopes)

                // Enables controllers and pages to get GraphServiceClient by dependency injection
                // And use an in memory token cache
                .AddMicrosoftGraph(Configuration.GetSection("DownstreamApi"))
                .AddInMemoryTokenCaches();

      services.AddControllersWithViews(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
          options.Filters.Add(new AuthorizeFilter(policy));
      });

      // Enables a UI and controller for sign in and sign out.
      services.AddRazorPages()
          .AddMicrosoftIdentityUI();
  }
```

El método `AddAuthentication()` configura el servicio para agregar la autenticación basada en cookies, que se usa en escenarios con explorador, y para establecer el desafío en OpenId Connect.

La línea que contiene `.AddMicrosoftIdentityWebApp` agrega la autenticación de la Plataforma de identidad de Microsoft a la aplicación. La proporciona [Microsoft.Identity.Web](microsoft-identity-web.md). Después se configura para iniciar sesión mediante la plataforma de identidad de Microsoft según la información de la sección `AzureAD` del archivo de configuración *appsettings.json*:

| Clave de *appsettings.json* | Descripción                                                                                                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ClientId`             | El **Id. de aplicación (cliente)** de la aplicación registrada en Azure Portal.                                                                                       |
| `Instance`             | El punto de conexión del servicio de token de seguridad (STS) para que el usuario se autentique. Normalmente, este valor es `https://login.microsoftonline.com/`, que indica la nube pública de Azure. |
| `TenantId`             | Nombre o id. (un GUID) del inquilino, o bien *common*, para el inicio de sesión de usuarios con cuentas profesionales o educativas o cuentas personales de Microsoft.                             |

El método `EnableTokenAcquisitionToCallDownstreamApi` permite a la aplicación adquirir un token para llamar a las API web protegidas. `AddMicrosoftGraph` permite a los controladores o páginas de Razor beneficiarse directamente de `GraphServiceClient` (mediante la inserción de dependencias) y el método `AddInMemoryTokenCaches` permite a la aplicación beneficiarse de un caché de token.

El método `Configure()` contiene dos métodos importantes, `app.UseAuthentication()` y `app.UseAuthorization()`, que habilitan su funcionalidad con nombre. También en el método `Configure()`, debe registrar las rutas de Microsoft Identity Web con al menos una llamada a `endpoints.MapControllerRoute()` o a `endpoints.MapControllers()`.

```csharp
app.UseAuthentication();
app.UseAuthorization();

app.UseEndpoints(endpoints =>
{

    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});

// endpoints.MapControllers(); // REQUIRED if MapControllerRoute() isn't called.
```

### <a name="protect-a-controller-or-a-controllers-method"></a>Protección de un controlador o del método de un controlador

Para proteger un controlador o sus métodos, aplique el atributo `[Authorize]` a la clase del controlador o a uno o varios de sus métodos. Este atributo `[Authorize]` restringe el acceso al controlador, ya que solo permite usuarios autenticados. Si el usuario aún no está autenticado, se puede iniciar un desafío de autenticación para acceder al controlador. En este inicio rápido, los ámbitos se leen desde el archivo de configuración:

```csharp
[AuthorizeForScopes(ScopeKeySection = "DownstreamApi:Scopes")]
public async Task<IActionResult> Index()
{
    var user = await _graphServiceClient.Me.Request().GetAsync();
    ViewData["ApiResult"] = user.DisplayName;

    return View();
}
```

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

El repositorio de GitHub que contiene el ejemplo de código de ASP.NET Core al que se hace referencia en este inicio rápido incluye instrucciones y más ejemplos de código que muestran cómo:

- Agregar autenticación a una nueva aplicación web de ASP.NET Core
- Llamar a Microsoft Graph u otras API de Microsoft en sus propias API web
- Agregar autorización
- Iniciar la sesión de usuarios en nubes nacionales o con identidades de redes sociales

> [!div class="nextstepaction"]
> [Tutoriales de aplicaciones web de ASP.NET Core en GitHub](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
