---
title: 'Inicio rápido: Incorporación del inicio de sesión con Microsoft a una aplicación web de Python | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, aprenderá cómo una aplicación web de Python puede iniciar la sesión de usuarios, obtener un token de acceso de la plataforma de identidad de Microsoft y llamar a Microsoft Graph API.
services: active-directory
author: abhidnya13
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.workload: identity
ms.date: 11/22/2021
ms.author: abpati
ms.custom: aaddev, devx-track-python, "scenarios:getting-started", "languages:Python", mode-api
ms.openlocfilehash: e108c50cd83126c1dc16f99287f3b41f48598c43
ms.sourcegitcommit: 34d047300d800cf6ff7d9dd3e573a0d785f61abc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2022
ms.locfileid: "135925819"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-python-web-app"></a>Inicio rápido: Adición del inicio de sesión con Microsoft a una aplicación web de Python

En este inicio rápido descargará y ejecutará un código de ejemplo que muestra cómo una aplicación web de Python puede realizar el inicio de sesión de usuarios y obtener un token de acceso para llamar a Microsoft Graph API. Los usuarios con una cuenta personal de Microsoft o una cuenta de cualquier organización en Azure Active Directory (Azure AD) pueden iniciar sesión en la aplicación.

Para ilustrar este tema, consulte el apartado en el que se explica el [funcionamiento del ejemplo](#how-the-sample-works).

## <a name="prerequisites"></a>Prerrequisitos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Python 2.7+](https://www.python.org/downloads/release/python-2713) o [Python 3+](https://www.python.org/downloads/release/python-364/)
- [Flask](http://flask.pocoo.org/), [Flask-Session](https://pypi.org/project/Flask-Session/), [solicitudes](https://requests.kennethreitz.org/en/master/)
- [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)

#### <a name="step-1-configure-your-application-in-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal

Para que el código de ejemplo de este inicio rápido funcione:

1. Agregue una dirección URL de respuesta como `http://localhost:5000/getAToken`.
1. Cree un secreto de cliente.
1. Agregue el permiso delegado User.ReadBasic.All de Microsoft Graph API.

> [!div class="nextstepaction"]
> [Realizar estos cambios por mí]()

> [!div class="alert alert-info"]
> ![Ya configurada](./media/quickstart-v2-aspnet-webapp/green-check.png) La aplicación está configurada con este atributo

#### <a name="step-2-download-your-project"></a>Paso 2: Descarga del proyecto

Descargue el proyecto y extraiga el archivo ZIP en la carpeta local más próxima a la carpeta raíz (por ejemplo, **C:\Azure-Samples**)
> [!div class="nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

> [!NOTE]
> `Enter_the_Supported_Account_Info_Here`

#### <a name="step-3-run-the-code-sample"></a>Paso 3: Ejecución del ejemplo de código

1. Deberá instalar la biblioteca Python de MSAL, el marco de Flask, las sesiones de Flask para la administración de sesiones del lado servidor y solicitudes mediante pip de la manera siguiente:

    ```shell
    pip install -r requirements.txt
    ```

2. Ejecute `app.py` desde el shell o la línea de comandos:

    ```shell
    python app.py
    ```

   > [!IMPORTANT]
   > Esta aplicación de inicio rápido usa un secreto de cliente para identificarse como cliente confidencial. Como el secreto de cliente se agrega como texto sin formato a los archivos del proyecto, por motivos de seguridad, se recomienda que use un certificado en lugar de un secreto de cliente antes de considerar el uso de la aplicación en producción. Para más información sobre cómo usar un certificado, consulte [estas instrucciones](./active-directory-certificate-credentials.md).

## <a name="more-information"></a>Más información

### <a name="how-the-sample-works"></a>Funcionamiento del ejemplo
![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-python-webapp/python-quickstart.svg)

### <a name="getting-msal"></a>Obtención de MSAL
MSAL es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para acceder a una API protegida por la Plataforma de identidad de Microsoft.
Puede agregar MSAL Python a la aplicación mediante Pip.

```Shell
pip install msal
```

### <a name="msal-initialization"></a>Inicialización de MSAL
Para agregar la referencia a MSAL Python, agregue el código siguiente en la parte superior del archivo en el que va a usar MSAL:

```Python
import msal
```

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las aplicaciones web que inician la sesión de usuarios en nuestra serie de escenarios de varias partes.

> [!div class="nextstepaction"]
> [Escenario: Aplicación web que permite iniciar sesión a los usuarios](scenario-web-app-sign-user-overview.md)
