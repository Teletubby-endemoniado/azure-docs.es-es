---
title: 'Inicio rápido: Incorporación del inicio de sesión con Microsoft Identity a una aplicación web de ASP.NET Core | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, aprenderá cómo una aplicación implementa el inicio de sesión de Microsoft en una aplicación web de ASP.NET mediante OpenID Connect.
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
ms.custom: devx-track-csharp, aaddev, identityplatformtop40, "scenarios:getting-started", "languages:aspnet-core", mode-other
ms.openlocfilehash: fd12703bc6cbaca1b2813ad75011caf39ae96475
ms.sourcegitcommit: 8d56da997b0b3ab07f91f6bef0c5661847758c4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2022
ms.locfileid: "138042374"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Inicio rápido: Adición del inicio de sesión con Microsoft a una aplicación web de ASP.NET Core


> [!div renderon="docs"]
> ¡Bienvenido! Probablemente esta no sea la página que esperaba. Mientras trabajamos en una corrección, este vínculo debería llevarle al artículo correcto:
>
> > [Inicio rápido: Aplicación web de ASP.NET Core con inicio de sesión de usuario](web-app-quickstart.md?pivots=devlang-aspnet-core)
> 
> Lamentamos las molestias y agradecemos su paciencia mientras trabajamos para resolverlo.

> [!div renderon="portal" class="sxs-lookup"]
> En este inicio rápido, descargará y ejecutará un código de ejemplo que muestra cómo una aplicación web de ASP.NET Core puede realizar el inicio de sesión de los usuarios desde cualquier organización de Azure Active Directory.  
> 
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
> Para que el código de ejemplo de este inicio rápido funcione:
> - En **URI de redirección**, escriba **https://localhost:44321/** y **https://localhost:44321/signin-oidc** .
> - En **Dirección URL de cierre de sesión del canal frontal**, escriba **https://localhost:44321/signout-oidc** . 
> 
> El punto de conexión de autorización emitirá tokens de identificador de solicitud.
> > [!div class="nextstepaction"]
> > [Hacer este cambio por mí]()
> 
> > [!div class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-aspnet-webapp/green-check.png) La aplicación está configurada con estos atributos.
> 
> #### <a name="step-2-download-the-aspnet-core-project"></a>Paso 2: Descarga del proyecto de ASP.NET Core
> 
> Ejecute el proyecto.
> 
> > [!div class="nextstepaction"]
> > [Descargar el código de ejemplo](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1.zip)
> 
> [!INCLUDE [active-directory-develop-path-length-tip](../../../includes/active-directory-develop-path-length-tip.md)]
> 
> 
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Paso 3: La aplicación está configurada y lista para ejecutarse
> Hemos configurado el proyecto con los valores de las propiedades de su aplicación y está preparado para ejecutarse.
> 
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
> 
> ## <a name="more-information"></a>Más información
> 
> En esta sección, se proporciona una introducción al código necesario para el inicio de sesión de usuarios. Esta introducción puede ser útil para comprender cómo funciona el código, cuáles son los argumentos principales y cómo agregar el inicio de sesión a una aplicación ASP.NET Core existente.
> 
> > [!div class="sxs-lookup" renderon="portal"]
> > ### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo
> >
> > ![Diagrama de la interacción entre el explorador web, la aplicación web y la plataforma de identidad de Microsoft en la aplicación de ejemplo.](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)
> 
> ### <a name="startup-class"></a>Clase Startup
> 
> El middleware *Microsoft.AspNetCore.Authentication* usa una clase `Startup` que se ejecuta al iniciarse el proceso de hospedaje:
> 
> ```csharp
> public void ConfigureServices(IServiceCollection services)
> {
>     services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
>         .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"));
> 
>     services.AddControllersWithViews(options =>
>     {
>         var policy = new AuthorizationPolicyBuilder()
>             .RequireAuthenticatedUser()
>             .Build();
>         options.Filters.Add(new AuthorizeFilter(policy));
>     });
>    services.AddRazorPages()
>         .AddMicrosoftIdentityUI();
> }
> ```
> 
> El método `AddAuthentication()` configura el servicio para agregar la autenticación basada en cookies. Esta autenticación se usa en escenarios de explorador y para establecer el desafío en OpenID Connect.
> 
> La línea que contiene `.AddMicrosoftIdentityWebApp` agrega la autenticación de la Plataforma de identidad de Microsoft a la aplicación. Posteriormente, la aplicación se configura para iniciar sesión mediante la plataforma de identidad de Microsoft de acuerdo con la información de la sección `AzureAD` del archivo de configuración *appsettings.json*:
> 
> | Clave de *appsettings.json* | Descripción                                                                                                                                                          |
> |------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | `ClientId`             | El Id. de aplicación (cliente) de la aplicación registrada en Azure Portal.                                                                                       |
> | `Instance`             | El punto de conexión del servicio de token de seguridad (STS) para que el usuario se autentique. Normalmente, este valor es `https://login.microsoftonline.com/`, que indica la nube pública de Azure. |
> | `TenantId`             | Nombre o identificador (GUID) del inquilino, o bien `common`, para el inicio de sesión de usuarios con cuentas profesionales o educativas o cuentas personales de Microsoft.                             |
> 
> El método `Configure()` contiene dos métodos importantes, `app.UseAuthentication()` y `app.UseAuthorization()`, que habilitan su funcionalidad con nombre. También en el método `Configure()`, debe registrar las rutas de Microsoft Identity Web mediante al menos una llamada a `endpoints.MapControllerRoute()` o a `endpoints.MapControllers()`:
> 
> ```csharp
> app.UseAuthentication();
> app.UseAuthorization();
> 
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapControllerRoute(
>         name: "default",
>         pattern: "{controller=Home}/{action=Index}/{id?}");
>     endpoints.MapRazorPages();
> });
> ```
> 
> ### <a name="attribute-for-protecting-a-controller-or-methods"></a>Atributo para proteger el controlador o los métodos
> 
> Puede proteger un controlador o los métodos de controlador mediante el atributo `[Authorize]`. Este atributo restringe el acceso al controlador o los métodos al permitir únicamente usuarios autenticados. Si el usuario aún no se ha autenticado, se puede iniciar un desafío de autenticación para acceder al controlador.
> 
> [!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
> 
> ## <a name="next-steps"></a>Pasos siguientes
> 
> El repositorio de GitHub que contiene este tutorial de ASP.NET Core incluye instrucciones y más ejemplos de código que muestran cómo:
> 
> - Agregar la autenticación a una nueva aplicación web de ASP.NET Core.
> - Llamar a Microsoft Graph, otras API de Microsoft o sus propias API web.
> - Agregar la autorización.
> - Iniciar la sesión de usuarios en nubes nacionales o con identidades sociales.
> 
> > [!div class="nextstepaction"]
> > [Tutoriales de aplicaciones web de ASP.NET Core en GitHub](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
