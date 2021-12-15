---
title: 'Inicio rápido: Inicio de sesión de usuarios en aplicaciones de página única (SPA) de JavaScript React con código de autenticación y llamada a Microsoft Graph | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, obtendrá información sobre la forma en que una aplicación de página única (SPA) de JavaScript React puede iniciar la sesión de usuarios de cuentas personales, profesionales y educativas mediante el flujo de código de autorización, ya llamar a Microsoft Graph.
services: active-directory
author: j-mantu
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.workload: identity
ms.date: 11/12/2021
ROBOTS: NOINDEX
ms.author: jamesmantu
ms.custom: aaddev, "scenarios:getting-started", "languages:JavaScript", devx-track-js, mode-other
ms.openlocfilehash: 2cdbab9976e4df00012597ec1f8c865c945e3e9e
ms.sourcegitcommit: ee9bae378f0b2b63b356a3ef3131640572f8c795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2021
ms.locfileid: "133790701"
---
# <a name="quickstart-sign-in-and-get-an-access-token-in-a-react-spa-using-the-auth-code-flow"></a>Inicio rápido: Inicio de sesión y obtención de un token de acceso en una SPA de React mediante el flujo de código de autorización

En este inicio rápido descargará y ejecutará un código de ejemplo que muestra cómo una aplicación de página única de JavaScript React puede realizar el inicio de sesión de usuarios y llamar a Microsoft Graph API mediante el flujo de código de autorización. En el ejemplo de código se muestra cómo obtener un token de acceso para llamar a Microsoft Graph API o a cualquier API web.

Para ilustrar este tema, consulte el apartado en el que se explica el [funcionamiento del ejemplo](#how-the-sample-works).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una de forma gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) u otro editor de código

#### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal

Estos ejemplos de código requieren un **URI de redirección** de `http://localhost:3000/`.
> [!div class="nextstepaction"]
> [Realizar estos cambios por mí]()

> [!div class="alert alert-info"]
> ![Ya configurada](media/quickstart-v2-javascript/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-the-project"></a>Paso 2: Descarga del proyecto

Ejecutar el proyecto con un servidor Web mediante Node.js

> [!div class="nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-javascript-react-spa/archive/main.zip)

> [!div class="sxs-lookup"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`


#### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Paso 3: La aplicación está configurada y lista para ejecutarse
Hemos configurado el proyecto con los valores de las propiedades de su aplicación.

#### <a name="step-4-run-the-project"></a>Paso 4: Ejecución del proyecto

Ejecute el proyecto con un servidor web mediante Node.js:

1. Para iniciar el servidor, ejecute los siguientes comandos desde el directorio del proyecto:
    ```console
    npm install
    npm start
    ```
1. Vaya a `http://localhost:3000/`.

1. Seleccione **Iniciar sesión** para iniciar el proceso de inicio de sesión y, luego, llame a Microsoft Graph API.

    La primera vez que inicie sesión, se le pedirá que dé su consentimiento para permitir que la aplicación acceda a su perfil e inicie sesión automáticamente. Una vez que haya iniciado sesión correctamente, haga clic en **Request Profile Information** (Solicitar información del perfil) para mostrar la información del perfil en la página.

## <a name="more-information"></a>Más información

### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo

![Diagrama que muestra el flujo de código de autorización para una aplicación de página única.](media/quickstart-v2-javascript-auth-code/diagram-01-auth-code-flow.png)

### <a name="msaljs"></a>msal.js

La biblioteca MSAL.js inicia la sesión de los usuarios y solicita los tokens que se usan para acceder a una API protegida por la plataforma de identidad de Microsoft.

Si tiene Node.js instalado, también puede descargar la versión más reciente mediante el administrador de paquetes de Node.js (npm):

```console
npm install @azure/msal-browser @azure/msal-react
```

## <a name="next-steps"></a>Pasos siguientes

A continuación, pruebe un tutorial paso a paso para aprender a crear una SPA de React desde cero que inicia la sesión de los usuarios y llama a Microsoft Graph API para obtener datos de perfil de usuario:

> [!div class="nextstepaction"]
> [Tutorial: Inicio de sesión de usuarios y llamada a Microsoft Graph](tutorial-v2-react.md)
