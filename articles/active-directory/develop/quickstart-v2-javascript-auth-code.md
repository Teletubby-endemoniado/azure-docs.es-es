---
title: 'Inicio rápido: Inicio de sesión de los usuarios en aplicaciones de página única (SPA) de JavaScript con código de autorización | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, obtendrá información sobre cómo una aplicación de página única (SPA) de JavaScript puede iniciar la sesión de usuarios para cuentas personales, profesionales y educativas mediante el flujo de código de autorización.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.workload: identity
ms.date: 11/12/2021
ROBOTS: NOINDEX
ms.author: marsma
ms.custom: aaddev, "scenarios:getting-started", "languages:JavaScript", devx-track-js, mode-other
ms.openlocfilehash: afd45394932c772692eb9ffaa995bdf8dbf11ce2
ms.sourcegitcommit: 8d56da997b0b3ab07f91f6bef0c5661847758c4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2022
ms.locfileid: "138031975"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-javascript-spa-using-the-auth-code-flow-with-pkce"></a>Inicio rápido: Inicio de sesión de los usuarios y obtención de un token de acceso en una aplicación SPA de JavaScript mediante el flujo de código de autorización con PKCE

> [!div renderon="docs"]
> ¡Bienvenido! Probablemente esta no sea la página que esperaba. Mientras trabajamos en una corrección, este vínculo debería llevarle al artículo correcto:
>
> > [Inicio rápido: Aplicación de página única de JavaScript con inicio de sesión de usuario](single-page-app-quickstart.md?pivots=devlang-javascript)
> 
> Lamentamos las molestias y agradecemos su paciencia mientras trabajamos para resolverlo.

> [!div renderon="portal" class="sxs-lookup"]
> En este inicio rápido descargará y ejecutará un código de ejemplo que muestra cómo una aplicación de página única de JavaScript puede iniciar la sesión de usuarios y llamar a Microsoft Graph API mediante el flujo de código de autorización con la clave de prueba para el intercambio de códigos (PKCE). En el ejemplo de código se muestra cómo obtener un token de acceso para llamar a Microsoft Graph API o a cualquier API web.
> 
> Para ilustrar este tema, consulte el apartado en el que se explica el [funcionamiento del ejemplo](#how-the-sample-works).
> 
> ## <a name="prerequisites"></a>Requisitos previos
> 
> * Una suscripción a Azure: [cree una de forma gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
> * [Node.js](https://nodejs.org/en/download/)
> * [Visual Studio Code](https://code.visualstudio.com/download) u otro editor de código
> 
> 
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
> Para que el código de ejemplo de este inicio rápido funcione, agregue un **URI de redirección** de `http://localhost:3000/`.
> > [!div class="nextstepaction"]
> > [Realizar estos cambios por mí]()
> 
> > [!div class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-javascript/green-check.png) La aplicación está configurada con estos atributos.
> 
> #### <a name="step-2-download-the-project"></a>Paso 2: Descarga del proyecto
> 
> Ejecutar el proyecto con un servidor Web mediante Node.js
> 
> > [!div class="nextstepaction"]
> > [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-javascript-v2/archive/master.zip)
> 
> > [!div class="sxs-lookup"]
> > > [!NOTE]
> > > `Enter_the_Supported_Account_Info_Here`
> 
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Paso 3: La aplicación está configurada y lista para ejecutarse
> 
> Hemos configurado el proyecto con los valores de las propiedades de su aplicación.
> 
> Ejecute el proyecto con un servidor web mediante Node.js.
> 
> 1. Para iniciar el servidor, ejecute los siguientes comandos desde el directorio del proyecto:
> 
>     ```console
>     npm install
>     npm start
>     ```
> 
> 1. Vaya a `http://localhost:3000/`.
> 
> 1. Seleccione **Iniciar sesión** para iniciar el proceso de inicio de sesión y, luego, llame a Microsoft Graph API.
> 
>     La primera vez que inicie sesión, se le pedirá que dé su consentimiento para permitir que la aplicación acceda a su perfil e inicie sesión automáticamente. Después de que haya iniciado sesión correctamente, se muestra en la página la información del perfil de usuario.
> 
> ## <a name="more-information"></a>Más información
> 
> ### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo
> 
> ![Diagrama que muestra el flujo de código de autorización para una aplicación de página única.](media/quickstart-v2-javascript-auth-code/diagram-01-auth-code-flow.png)
> 
> ### <a name="msaljs"></a>MSAL.js
> 
> La biblioteca MSAL.js inicia la sesión de los usuarios y solicita los tokens que se usan para acceder a una API protegida por la plataforma de identidad de Microsoft. El archivo *index.html* del ejemplo contiene una referencia a la biblioteca:
> 
> ```html
> <script type="text/javascript" src="https://alcdn.msauth.net/browser/2.0.0-beta.0/js/msal-browser.js" integrity=
> "sha384-r7Qxfs6PYHyfoBR6zG62DGzptfLBxnREThAlcJyEfzJ4dq5rqExc1Xj3TPFE/9TH" crossorigin="anonymous"></script>
> ```
> 
> Si tiene Node.js instalado, también puede descargar la versión más reciente mediante el administrador de paquetes de Node.js (npm):
> 
> ```console
> npm install @azure/msal-browser
> ```
> 
> ## <a name="next-steps"></a>Pasos siguientes
> 
> Para encontrar una guía paso a paso más detallada sobre la compilación de la aplicación usada en este inicio rápido, consulte:
> 
> > [!div class="nextstepaction"]
> > [Tutorial para iniciar sesión y llamar a MS Graph](./tutorial-v2-javascript-auth-code.md)
