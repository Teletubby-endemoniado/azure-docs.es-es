---
title: 'Inicio rápido: Llamada a Microsoft Graph desde una aplicación de consola de Node.js | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, descargará y ejecutará un código de ejemplo que muestra de qué forma una aplicación de consola de Node.js puede obtener un token de acceso y llamar a una API protegida por un punto de conexión de una plataforma de identidad de Microsoft mediante la propia identidad de la aplicación
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.date: 01/10/2022
ms.author: marsma
ms.custom: mode-api
ms.openlocfilehash: eeb50fc335835dcc2765c894e151f39cccbf25e4
ms.sourcegitcommit: b55c580fe2bb9fbf275ddf414d547ddde8d71d8a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2022
ms.locfileid: "136834874"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-nodejs-console-app-using-apps-identity"></a>Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de consola de Node.js mediante la identidad de la aplicación

En este inicio rápido descargará y ejecutará un código de ejemplo que muestra cómo puede obtener una aplicación de consola de Node.js un token de acceso mediante la identidad de la aplicación para llamar a Microsoft Graph API y mostrar una [lista de usuarios](/graph/api/user-list) en el directorio. En el ejemplo de código se muestra cómo se puede ejecutar un trabajo desatendido o un servicio de Windows con una identidad de aplicación, en lugar de la identidad de un usuario.

En este inicio rápido se usa la [biblioteca de autenticación de Microsoft para Node.js (MSAL Node)](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) con la [concesión de credenciales de cliente](v2-oauth2-client-creds-grant-flow.md).

## <a name="prerequisites"></a>Prerrequisitos

* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) u otro editor de código


### <a name="download-and-configure-the-sample-app"></a>Descarga y configuración de la aplicación de ejemplo

#### <a name="step-1-configure-the-application-in-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
Para que el ejemplo de código de esta guía de inicio rápido funcione, debe crear un secreto de cliente y agregar el permiso de aplicación **User.Read.All** de Graph API.
> [!div class="nextstepaction"]
> [Realizar estos cambios por mí]()

> [!div class="alert alert-info"]
> ![Ya configurada](media/quickstart-v2-netcore-daemon/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-the-nodejs-sample-project"></a>Paso 2: Descarga del proyecto de ejemplo en Node.js

> [!div class="sxs-lookup nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/azure-samples/ms-identity-javascript-nodejs-console/archive/main.zip)

> [!div class="sxs-lookup"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

#### <a name="step-3-admin-consent"></a>Paso 3: Consentimiento de administrador

Si intenta ejecutar la aplicación en este momento, recibirá un error *HTTP 403 - Prohibido*: `Insufficient privileges to complete the operation`. Este error sucede porque cualquier *permiso de solo aplicación* requiere el **consentimiento del administrador**: un administrador global del directorio debe otorgar su consentimiento a la aplicación. Seleccione una de las opciones siguientes según el rol:

##### <a name="global-tenant-administrator"></a>Administrador de inquilinos global

Si es administrador global, vaya a la página **Permisos de API**, seleccione **Grant admin consent for Enter_the_Tenant_Name_Here** (Conceder consentimiento del administrador para _escribir_aquí_el_nombre_del_inquilino).
> > [!div id="apipermissionspage"]
> > [Ir a la página Permisos de API]()

##### <a name="standard-user"></a>Usuario estándar

Si es usuario estándar de su inquilino, debe pedir a un administrador global que conceda **consentimiento del administrador** para su aplicación. Para ello, proporcione la siguiente dirección URL a su administrador:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```

#### <a name="step-4-run-the-application"></a>Paso 4: Ejecución de la aplicación

Busque la carpeta raíz del ejemplo (donde `package.json` reside) en una consola o un símbolo del sistema. Deberá instalar las dependencias de este ejemplo una vez:

```console
npm install
```

A continuación, ejecute la aplicación a través del símbolo del sistema o la consola:

```console
node . --op getUsers
```

Debería ver en la salida de la consola algún fragmento de JSON que represente una lista de los usuarios del directorio de Azure AD.

## <a name="about-the-code"></a>Sobre el código

A continuación, se describen algunos de los aspectos importantes de la aplicación de ejemplo.

### <a name="msal-node"></a>MSAL Node

[MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para acceder a una API protegida por la plataforma de identidad de Microsoft. Como se ha descrito, en este inicio rápido se solicitan tokens por permisos de aplicación (mediante la identidad propia de la aplicación), en lugar de permisos delegados. El flujo de autenticación usado en este caso se conoce como [flujo de credenciales de cliente de OAuth 2.0](v2-oauth2-client-creds-grant-flow.md). Para más información sobre cómo usar MSAL Node con aplicaciones de demonio, consulte [Escenario: aplicación demonio](scenario-daemon-overview.md).

 Para instalar MSAL Node, ejecute el siguiente comando npm.

```console
npm install @azure/msal-node --save
```

### <a name="msal-initialization"></a>Inicialización de MSAL

Puede agregar la referencia de MSAL con el código siguiente:

```javascript
const msal = require('@azure/msal-node');
```

A continuación, realice la inicialización de MSAL con el siguiente código:

```javascript
const msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_Here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_Id_Here",
        clientSecret: "Enter_the_Client_Secret_Here",
   }
};
const cca = new msal.ConfidentialClientApplication(msalConfig);
```

> | Donde: |Descripción |
> |---------|---------|
> | `clientId` | Es el **Identificador de aplicación (cliente)** de la aplicación registrada en Azure Portal. Puede encontrar este valor en la página **Información general** de la aplicación en Azure Portal. |
> | `authority`    | El punto de conexión STS para el usuario que se autenticará. Normalmente `https://login.microsoftonline.com/{tenant}` en la nube pública, donde {tenant} es el nombre o el identificador del inquilino.|
> | `clientSecret` | Es el secreto de cliente creado para la aplicación en Azure Portal. |

Para más información, consulte la [documentación de referencia de `ConfidentialClientApplication`](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-confidential-client-application.md).

### <a name="requesting-tokens"></a>Solicitud de tokens

Para solicitar un token mediante la identidad de la aplicación, use el método `acquireTokenByClientCredential`:

```javascript
const tokenRequest = {
    scopes: [ 'https://graph.microsoft.com/.default' ],
};

const tokenResponse = await cca.acquireTokenByClientCredential(tokenRequest);
```

> |Donde:| Descripción |
> |---------|---------|
> | `tokenRequest` | Contiene los ámbitos solicitados. Con clientes confidenciales, se debe usar el formato similar a `{Application ID URI}/.default` para indicar que los ámbitos que se solicitan son los definidos estáticamente en el objeto de aplicación establecido en Azure Portal (con Microsoft Graph, `{Application ID URI}` apunta a `https://graph.microsoft.com`). En el caso de las API web personalizadas, `{Application ID URI}` se define en la sección **Exponer una API** del registro de aplicación de Azure Portal. |
> | `tokenResponse` | La respuesta contiene un token de acceso para los ámbitos solicitados. |

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el desarrollo de aplicaciones de consola o demonio con MSAL Node, consulte el tutorial:

> [!div class="nextstepaction"]
> [Aplicación demonio que llama a las API web](tutorial-v2-nodejs-console.md)
