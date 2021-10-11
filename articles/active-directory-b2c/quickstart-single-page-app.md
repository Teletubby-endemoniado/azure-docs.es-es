---
title: 'Inicio rápido: Configuración del inicio de sesión de una aplicación de página única (SPA)'
titleSuffix: Azure AD B2C
description: En este inicio rápido, se ejecuta una aplicación de página única de ejemplo que usa Azure Active Directory B2C para proporcionar el inicio de sesión de la cuenta.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.date: 04/04/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 7dc02eb3c74208cf0d438640434430c7c04aeb9c
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129353659"
---
# <a name="quickstart-set-up-sign-in-for-a-single-page-app-using-azure-active-directory-b2c"></a>Inicio rápido: Configuración del inicio de sesión de una aplicación de página única mediante Azure Active Directory B2C

Azure Active Directory B2C (Azure AD B2C) proporciona administración de identidades en la nube para mantener la protección de su aplicación, empresa y clientes. Azure AD B2C permite que las aplicaciones puedan autenticarse con cuentas de redes sociales y cuentas de empresa mediante protocolos estándar abiertos. 

En esta guía de inicio rápido, se utiliza una aplicación de página única para iniciar sesión mediante un proveedor de identidades de redes sociales y llamar a una API web protegida por Azure AD B2C.

<!--[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] -->

## <a name="prerequisites"></a>Prerrequisitos

- [Visual Studio Code](https://code.visualstudio.com/)
- [Node.js](https://nodejs.org/en/download/)
- Cuenta de redes sociales de Facebook, Google o Microsoft
- Ejemplo de código de GitHub: [ms-identity-b2c-javascript-spa](https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa):

    Puede [descargar el archivo .zip](https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa/archive/main.zip) o clonar el repositorio.

    ```console
    git clone https://github.com/Azure-Samples/ms-identity-b2c-javascript-spa.git
    ```

## <a name="run-the-application"></a>Ejecución de la aplicación

1. Inicie el servidor mediante la ejecución de los siguientes comandos desde el símbolo del sistema de Node.js:

    ```console
    npm install
    npm update
    npm start
    ```

    El servidor que *server.js* inicia muestra el puerto en el que está escuchando:

    ```console
    Listening on port 6420...
    ```

1. Vaya a la dirección URL de la aplicación. Por ejemplo, `http://localhost:6420`.

    ![Aplicación de página única de ejemplo en el explorador](./media/quickstart-single-page-app/sample-app-spa.png)

## <a name="sign-in-using-your-account"></a>Inicio de sesión mediante su cuenta

1. Seleccione **Iniciar sesión** para iniciar el recorrido del usuario.
1. Azure AD B2C presenta una página de inicio de sesión para una empresa ficticia llamada "Fabrikam" para la aplicación web de ejemplo. Para registrarse con un proveedor de identidades de redes sociales, seleccione el botón del proveedor de identidades que desee usar.

    ![Página de inicio de sesión o registro que muestra los botones del proveedor de identidades](./media/quickstart-single-page-app/sign-in-or-sign-up-spa.png)

    Debe autenticarse (iniciar sesión) con las credenciales de su cuenta de redes sociales y autorizar a la aplicación para que lea la información de su cuenta de redes sociales. Al conceder acceso, la aplicación puede recuperar la información del perfil de la cuenta de redes sociales como el nombre y la ciudad.

1. Finalice el proceso de inicio de sesión para el proveedor de identidades.

## <a name="access-a-protected-api-resource"></a>Acceso a un recurso de API protegido

Seleccione **Call API** (Llamar a la API) para obtener el nombre para mostrar de la llamada de API web como un objeto JSON.

![Aplicación de ejemplo en el explorador con la respuesta de la API web](./media/quickstart-single-page-app/call-api-spa.png)

La aplicación de página única de ejemplo incluye un token de acceso en la solicitud al recurso de API web protegido.

<!-- ## Clean up resources

You can use your Azure AD B2C tenant if you plan to try other Azure AD B2C quickstarts or tutorials. When no longer needed, you can [delete your Azure AD B2C tenant](faq.yml#how-do-i-delete-my-azure-ad-b2c-tenant-).-->

## <a name="next-steps"></a>Pasos siguientes

<!---In this quickstart, you used a sample single-page application to:

- Sign in with a social identity provider
- Create an Azure AD B2C user account (created automatically at sign-in)
- Call a web API protected by Azure AD B2C -->

- Introducción a la creación del [inquilino de Azure Active Directory B2C en Azure Portal](tutorial-create-tenant.md)
