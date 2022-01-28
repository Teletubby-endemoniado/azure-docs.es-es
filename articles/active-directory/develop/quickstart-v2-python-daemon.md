---
title: 'Inicio rápido: Llamada a Microsoft Graph desde un demonio de Python | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, obtendrá información sobre la forma en que un proceso de Python puede obtener un token de acceso y llamar a una API protegida por la plataforma de identidad de Microsoft mediante la propia identidad de la aplicación.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.workload: identity
ms.date: 01/10/2022
ROBOTS: NOINDEX
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, devx-track-python, "scenarios:getting-started", "languages:Python", mode-api
ms.openlocfilehash: c762f47fe93780c6f405b3cabc546ecd6fec8f16
ms.sourcegitcommit: 3f20f370425cb7d51a35d0bba4733876170a7795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2022
ms.locfileid: "137803749"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-python-console-app-using-apps-identity"></a>Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de consola en Python mediante la identidad de la aplicación

En este inicio rápido descargará y ejecutará un código de ejemplo que muestra cómo una aplicación de Python puede obtener un token de acceso mediante la identidad de la aplicación para llamar a Microsoft Graph API y mostrar una [lista de usuarios](/graph/api/user-list) del directorio. En el ejemplo de código se muestra cómo se puede ejecutar un trabajo desatendido o un servicio de Windows con una identidad de aplicación, en lugar de la identidad de un usuario. 

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar esta muestra, necesita:

- [Python 2.7+](https://www.python.org/downloads/release/python-2713) o [Python 3+](https://www.python.org/downloads/release/python-364/)
- [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)

> [!div class="sxs-lookup"]
### <a name="download-and-configure-the-quickstart-app"></a>Descarga y configuración de la aplicación de inicio rápido

#### <a name="step-1-configure-your-application-in-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
Para que el ejemplo de código de este inicio rápido funcione, cree un secreto de cliente y agregue el permiso de aplicación **User.Read.All** de Graph API.
> [!div class="nextstepaction"]
> [Realizar estos cambios por mí]()

> [!div class="alert alert-info"]
> ![Ya configurada](media/quickstart-v2-netcore-daemon/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-the-python-project"></a>Paso 2: Descarga del proyecto de Python

> [!div class="sxs-lookup nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-python-daemon/archive/master.zip)

> [!div class="sxs-lookup"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

#### <a name="step-3-admin-consent"></a>Paso 3: Consentimiento de administrador

Si intenta ejecutar la aplicación en este momento, recibirá un error *HTTP 403 - Prohibido*: `Insufficient privileges to complete the operation`. Este error sucede porque cualquier *permiso de solo aplicación* requiere el consentimiento del administrador: un administrador global del directorio debe otorgar su consentimiento a la aplicación. Seleccione una de las opciones siguientes según el rol:

##### <a name="global-tenant-administrator"></a>Administrador de inquilinos global

Si es administrador global, vaya a la página **API Permissions** (Permisos de API), seleccione **Grant admin consent for Enter_the_Tenant_Name_Here** (Conceder consentimiento del administrador para _escribir_aquí_el_nombre_del_inquilino).
> [!div id="apipermissionspage"]
> [Ir a la página Permisos de API]()

##### <a name="standard-user"></a>Usuario estándar

Si es usuario estándar de su inquilino, pídale a un administrador global que conceda consentimiento del administrador para su aplicación. Para ello, proporcione la siguiente dirección URL a su administrador:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```


#### <a name="step-4-run-the-application"></a>Paso 4: Ejecución de la aplicación

Deberá instalar las dependencias de este ejemplo una vez.

```console
pip install -r requirements.txt
```

A continuación, ejecute la aplicación a través del símbolo del sistema o la consola:

```console
python confidential_client_secret_sample.py parameters.json
```

Debería ver en la salida de la consola algún fragmento en JSON que representa una lista de usuarios en el directorio de Azure AD.

> [!IMPORTANT]
> Esta aplicación de inicio rápido usa un secreto de cliente para identificarse como cliente confidencial. Como el secreto de cliente se agrega como texto sin formato a los archivos del proyecto, por motivos de seguridad, se recomienda que use un certificado en lugar de un secreto de cliente antes de considerar el uso de la aplicación en producción. Para más información sobre cómo usar un certificado, consulte [estas instrucciones](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/2-Call-MsGraph-WithCertificate/README.md) en el mismo repositorio de GitHub que este ejemplo, pero en la segunda carpeta, **2-Call-MsGraph-WithCertificate**.

## <a name="more-information"></a>Más información

### <a name="msal-python"></a>Python de MSAL

[MSAL para Python](https://github.com/AzureAD/microsoft-authentication-library-for-python) es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para acceder a una API protegida por la Plataforma de identidad de Microsoft. Como se ha descrito, en este inicio rápido se solicitan tokens mediante la propia identidad de la aplicación, en lugar de permisos delegados. El flujo de autenticación usado en este caso se conoce como *[flujo de OAuth de credenciales de cliente](v2-oauth2-client-creds-grant-flow.md)* . Para obtener más información sobre cómo usar MSAL para Python con aplicaciones de demonio, consulte [este artículo](scenario-daemon-overview.md).

 Para instalar MSAL para Python, ejecute el siguiente comando pip:

```powershell
pip install msal
```

### <a name="msal-initialization"></a>Inicialización de MSAL

Puede agregar la referencia de MSAL con el código siguiente:

```Python
import msal
```

A continuación, realice la inicialización de MSAL con el siguiente código:

```Python
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"])
```

> | Donde: |Descripción |
> |---------|---------|
> | `config["secret"]` | Es el secreto de cliente creado para la aplicación en Azure Portal. |
> | `config["client_id"]` | Es el **Identificador de aplicación (cliente)** de la aplicación registrada en Azure Portal. Puede encontrar este valor en la página **Información general** de la aplicación en Azure Portal. |
> | `config["authority"]`    | El punto de conexión STS para el usuario que se autenticará. Normalmente `https://login.microsoftonline.com/{tenant}` en la nube pública, donde {tenant} es el nombre o el identificador del inquilino.|

Para más información, consulte la [documentación de referencia de `ConfidentialClientApplication`](https://msal-python.readthedocs.io/en/latest/#confidentialclientapplication).

### <a name="requesting-tokens"></a>Solicitud de tokens

Para solicitar un token mediante la identidad de la aplicación, use el método `AcquireTokenForClient`:

```Python
result = None
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])
```

> |Donde:| Descripción |
> |---------|---------|
> | `config["scope"]` | Contiene los ámbitos solicitados. Con clientes confidenciales se debe usar un formato similar a `{Application ID URI}/.default` para indicar que los ámbitos que se solicitan son los definidos estáticamente en el objeto de aplicación establecido en Azure Portal (con Microsoft Graph, `{Application ID URI}` apunta a `https://graph.microsoft.com`). Con API web personalizadas, `{Application ID URI}` se define en la sección **Exponer una API** en **Registros de aplicaciones** de Azure Portal.|

Para más información, consulte la [documentación de referencia de `AcquireTokenForClient`](https://msal-python.readthedocs.io/en/latest/#msal.ConfidentialClientApplication.acquire_token_for_client).

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las aplicaciones demonio, consulte la página de aterrizaje del escenario.

> [!div class="nextstepaction"]
> [Aplicación demonio que llama a las API web](scenario-daemon-overview.md)
