---
title: 'Inicio rápido: Aplicación web ASP.NET que inicia la sesión de los usuarios'
titleSuffix: Microsoft identity platform
description: Descargue y ejecute un ejemplo de código que muestre cómo una aplicación web de ASP.NET puede hacer que usuarios de Azure AD inicien sesión.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.workload: identity
ms.date: 11/22/2021
ROBOTS: NOINDEX
ms.author: jmprieur
ms.custom: devx-track-csharp, aaddev, identityplatformtop40, "scenarios:getting-started", "languages:ASP.NET", contperf-fy21q1, mode-other
ms.openlocfilehash: b784950dcd1a5b8f1a55d45719a79a64f1fbdfba
ms.sourcegitcommit: 3f20f370425cb7d51a35d0bba4733876170a7795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2022
ms.locfileid: "137805059"
---
# <a name="quickstart-aspnet-web-app-that-signs-in-azure-ad-users"></a>Inicio rápido: aplicación web de ASP.NET que hace que usuarios de Azure AD inicien sesión

En este inicio rápido, descargará y ejecutará un ejemplo de código que muestra cómo una aplicación web de ASP.NET puede realizar el inicio de sesión de usuarios con cuentas de Azure Active Directory (Azure AD).

#### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
Para que el código de ejemplo de este inicio rápido funcione, agregue **https://localhost:44368/** en **URI de redirección**.

> [!div class="nextstepaction"]
> [Hacer este cambio por mí]()

> [!div class="alert alert-info"]
> ![Ya configurada](media/quickstart-v2-aspnet-webapp/green-check.png) La aplicación está configurada con este atributo.

#### <a name="step-2-download-the-project"></a>Paso 2: Descarga del proyecto

Ejecute el proyecto con Visual Studio 2019.
> [!div class="sxs-lookup nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip)

[!INCLUDE [active-directory-develop-path-length-tip](../../../includes/active-directory-develop-path-length-tip.md)]


#### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Paso 3: La aplicación está configurada y lista para ejecutarse
Hemos configurado el proyecto con los valores de las propiedades de su aplicación.

1. Extraiga el archivo. zip en una carpeta local que esté próxima a la carpeta raíz. Por ejemplo, en *C:\Azure-Samples*.
   
   Se recomienda extraer el archivo en un directorio próximo a la raíz de la unidad para evitar errores provocados por las limitaciones de longitud de la ruta de acceso en Windows.
2. Abra la solución en Visual Studio (*AppModelv2-WebApp-OpenIDConnect-DotNet.sln*).
3. Según la versión de Visual Studio que use, es posible que tenga que hacer clic con el botón derecho en el proyecto **AppModelv2-WebApp-OpenIDConnect-DotNet** y después en **Restaurar paquetes NuGet**.
4. Abra la Consola del Administrador de paquetes seleccionando **Ver**  >  **Otras ventanas**  >  **Consola del Administrador de paquetes**. A continuación, ejecute `Update-Package Microsoft.CodeDom.Providers.DotNetCompilerPlatform -r`.

> [!NOTE]
> `Enter_the_Supported_Account_Info_Here`

## <a name="more-information"></a>Más información

En esta sección, se proporciona una introducción al código necesario para el inicio de sesión de usuarios. Esta introducción puede ser útil para comprender cómo funciona el código, cuáles son los argumentos principales y cómo agregar el inicio de sesión a una aplicación ASP.NET existente.


### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo

![Diagrama de la interacción entre el explorador web, la aplicación web y la plataforma de identidad de Microsoft en la aplicación de ejemplo.](media/quickstart-v2-aspnet-webapp/aspnetwebapp-intro.svg)

### <a name="owin-middleware-nuget-packages"></a>Paquetes NuGet del middleware OWIN

Puede configurar la canalización de autenticación con la autenticación basada en cookies mediante OpenID Connect en ASP.NET con paquetes del middleware OWIN. Puede instalar estos paquetes mediante la ejecución del siguiente comando en la Consola del Administrador de paquetes de Visual Studio:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

### <a name="owin-startup-class"></a>Clase de inicio de OWIN

El middleware OWIN usa una *clase de inicio* que se ejecuta cuando se inicia el proceso de hospedaje. En este inicio rápido, el archivo *startup.cs* se encuentra en la carpeta raíz. En el código siguiente se muestran los parámetros que usa este inicio rápido:

```csharp
public void Configuration(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());
    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            // Sets the client ID, authority, and redirect URI as obtained from Web.config
            ClientId = clientId,
            Authority = authority,
            RedirectUri = redirectUri,
            // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it's using the home page
            PostLogoutRedirectUri = redirectUri,
            Scope = OpenIdConnectScope.OpenIdProfile,
            // ResponseType is set to request the code id_token, which contains basic information about the signed-in user
            ResponseType = OpenIdConnectResponseType.CodeIdToken,
            // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
            // To only allow users from a single organization, set ValidateIssuer to true and the 'tenant' setting in Web.config to the tenant name
            // To allow users from only a list of specific organizations, set ValidateIssuer to true and use the ValidIssuers parameter
            TokenValidationParameters = new TokenValidationParameters()
            {
                ValidateIssuer = false // Simplification (see note below)
            },
            // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to the OnAuthenticationFailed method
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = OnAuthenticationFailed
            }
        }
    );
}
```

> |Where  | Descripción |
> |---------|---------|
> | `ClientId`     | El identificador de la aplicación registrada en Azure Portal. |
> | `Authority`    | El punto de conexión del servicio de token de seguridad (STS) para que el usuario se autentique. Normalmente, es `https://login.microsoftonline.com/{tenant}/v2.0` para la nube pública. En esta URL, *{tenant}* es el nombre del inquilino, su descripción o `common` si se hace referencia al punto de conexión común. El punto de conexión común se usa para las aplicaciones multiinquilino. |
> | `RedirectUri`  | La dirección URL a la que se envía a los usuarios después de la autenticación en la plataforma de identidad de Microsoft. |
> | `PostLogoutRedirectUri`     | La dirección URL a donde se envía a los usuarios después de cerrar sesión. |
> | `Scope`     | La lista de ámbitos que se solicitan, separados por espacios. |
> | `ResponseType`     | La solicitud de que la respuesta de la autenticación contenga un código de autorización y un token de identificador. |
> | `TokenValidationParameters`     | Una lista de parámetros para la validación del token. En este caso, `ValidateIssuer` está establecido en `false` para indicar que puede aceptar inicios de sesión desde cualquier tipo de cuenta: personal, profesional o educativa. |
> | `Notifications`     | Una lista de los delegados que se pueden ejecutar en mensajes `OpenIdConnect`. |


> [!NOTE]
> Establecer `ValidateIssuer = false` es una simplificación para este inicio rápido. En las aplicaciones reales, valide el emisor. Consulte los ejemplos para entender cómo hacerlo.

### <a name="authentication-challenge"></a>Desafío de autenticación

Puede forzar a un usuario para que inicie sesión si solicita un desafío de autenticación en el controlador:

```csharp
public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties{ RedirectUri = "/" },
            OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
```

> [!TIP]
> La solicitud de un desafío de autenticación mediante este método es opcional. Normalmente se usa cuando se desea que una vista sea accesible tanto para usuarios autenticados como no autenticados. También puede proteger los controladores mediante el método descrito en la sección siguiente.

### <a name="attribute-for-protecting-a-controller-or-a-controller-actions"></a>Atributo para proteger un controlador o las acciones de un controlador

Puede proteger un controlador o sus acciones mediante el atributo `[Authorize]`. Este atributo restringe el acceso al controlador o a sus acciones al permitir que solo los usuarios autenticados puedan acceder a estas acciones. Cuando un usuario no autenticado intente acceder a una de las acciones o de los controladores representados por el atributo `[Authorize]`, se producirá automáticamente un desafío de autenticación.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Visite el tutorial de ASP.NET para acceder a una guía completa paso a paso sobre la creación de aplicaciones y nuevas características, que incluye una explicación completa de este inicio rápido.

> [!div class="nextstepaction"]
> [Incorporación del inicio de sesión a una aplicación web ASP.NET](tutorial-v2-asp-webapp.md)
