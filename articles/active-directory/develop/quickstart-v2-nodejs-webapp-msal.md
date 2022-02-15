---
title: 'Inicio rápido: Adición de autenticación a una aplicación web de Node.js con MSAL Node | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, aprenderá a implementar la autenticación con una aplicación web de Node.js y la biblioteca de autenticación de Microsoft (MSAL) para Node.js.
services: active-directory
author: mmacy
manager: celested
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.workload: identity
ms.date: 11/22/2021
ROBOTS: NOINDEX
ms.author: marsma
ms.custom: aaddev, "scenarios:getting-started", "languages:js", devx-track-js, mode-api
ms.openlocfilehash: 1c2165569be8b59c0234e0e56d147d53ea66331a
ms.sourcegitcommit: 8d56da997b0b3ab07f91f6bef0c5661847758c4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2022
ms.locfileid: "138039348"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-nodejs-web-app-using-the-auth-code-flow"></a>Inicio rápido: Inicio de sesión de los usuarios y obtención de un token de acceso en una aplicación web de Node.js mediante el flujo de código de autorización


> [!div renderon="docs"]
> ¡Bienvenido! Probablemente esta no sea la página que esperaba. Mientras trabajamos en una corrección, este vínculo debería llevarle al artículo correcto:
>
> > [Inicio rápido: Aplicación web de Node.js que inicia la sesión de los usuarios con el nodo MSAL](web-app-quickstart.md?pivots=devlang-nodejs-msal)
> 
> Lamentamos las molestias y agradecemos su paciencia mientras trabajamos para resolverlo.

> [!div renderon="portal" class="sxs-lookup"]
> En este inicio rápido, descargará y ejecutará un código de ejemplo que muestra de qué forma puede una aplicación web de Node.js realizar el inicio de sesión de los usuarios mediante el flujo de código de autorización. En el ejemplo de código se muestra cómo obtener un token de acceso para llamar a Microsoft Graph API.
> 
> Para ilustrar este tema, consulte el apartado en el que se explica el [funcionamiento del ejemplo](#how-the-sample-works).
> 
> En este inicio rápido se usa la biblioteca de autenticación de Microsoft para Node.js (MSAL Node) con el flujo del código de autorización.
> 
> ## <a name="prerequisites"></a>Requisitos previos
> 
> * Suscripción a Azure. [Creación de una suscripción de Azure gratis](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
> * [Node.js](https://nodejs.org/en/download/)
> * [Visual Studio Code](https://code.visualstudio.com/download) u otro editor de código
> 
> #### <a name="step-1-configure-the-application-in-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
> Para que funcione el código de ejemplo de este inicio rápido, es preciso crear un secreto de cliente y agregar la siguiente dirección URL de respuesta: `http:/> /localhost:3000/redirect`.
> > [!div class="nextstepaction"]
> > [Hacer este cambio por mí]()
> 
> > [!div class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-windows-desktop/green-check.png) La aplicación está configurada con estos atributos >.
> 
> #### <a name="step-2-download-the-project"></a>Paso 2: Descarga del proyecto
> 
> Ejecute el proyecto con un servidor web mediante Node.js.
> 
> > [!div class="nextstepaction"]
> > [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-node/archive/main.zip)
> 
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Paso 3: La aplicación está configurada y lista para ejecutarse
> 
> Ejecute el proyecto mediante Node.js.
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
> 1. Seleccione **Iniciar sesión** para comenzar el proceso de inicio de sesión.
> 
>     La primera vez que inicie sesión, se le pedirá que dé su consentimiento para permitir que la aplicación acceda a su perfil e inicie sesión automáticamente. Tras iniciar sesión correctamente, verá un mensaje de registro en la línea de comandos.
> 
> ## <a name="more-information"></a>Más información
> 
> ### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo
> 
> El ejemplo hospeda un servidor web en localhost, puerto 3000. Cuando un explorador web accede a este sitio, el ejemplo redirige de inmediato al usuario a una página de autenticación de Microsoft. Por eso el ejemplo no contiene elementos HTML ni de presentación. Si el resultado de la autenticación es satisfactorio, se muestra el mensaje "OK" (Correcto).
> 
> ### <a name="msal-node"></a>MSAL Node
> 
> La biblioteca MSAL Node inicia la sesión de los usuarios y solicita los tokens que se usan para acceder a una API protegida por la plataforma de identidad de Microsoft. Puede descargar la versión más reciente mediante el administrador de paquetes de Node.js (npm):
> 
> ```console
> npm install @azure/msal-node
> ```
> 
> ## <a name="next-steps"></a>Pasos siguientes
> 
> > [!div class="nextstepaction"]
> > [Adición de autenticación a una aplicación web existente: ejemplo de código de GitHub >](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/msal-node-samples/auth-code)
