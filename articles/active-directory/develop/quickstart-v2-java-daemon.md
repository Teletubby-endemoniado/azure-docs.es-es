---
title: 'Inicio rápido: Llamada a Microsoft Graph desde un demonio de Java | Azure'
titleSuffix: Microsoft identity platform
description: En este inicio rápido, obtendrá información sobre cómo una aplicación de Java puede obtener un token de acceso y llamar a una API protegida por un punto de conexión de la Plataforma de identidad de Microsoft mediante la propia identidad de la aplicación.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: portal
ms.workload: identity
ms.date: 01/10/2022
ms.author: marsma
ms.custom: aaddev, "scenarios:getting-started", "languages:Java", devx-track-java, mode-api
ms.openlocfilehash: 03cbfaf39178f62a570b18dc9a53d50153dbf9da
ms.sourcegitcommit: b55c580fe2bb9fbf275ddf414d547ddde8d71d8a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2022
ms.locfileid: "136836243"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-java-console-app-using-apps-identity"></a>Inicio rápido: Adquisición de un token y llamada a Microsoft Graph API desde una aplicación de consola de Java mediante la identidad de la aplicación

En este inicio rápido descargará y ejecutará un código de ejemplo que muestra cómo puede obtener una aplicación de Java un token de acceso mediante la identidad de la aplicación para llamar a Microsoft Graph API y mostrar una [lista de usuarios](/graph/api/user-list) en el directorio. En el ejemplo de código se muestra cómo se puede ejecutar un trabajo desatendido o un servicio de Windows con una identidad de aplicación, en lugar de la identidad de un usuario.

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar esta muestra, necesita:

- [Kit de desarrollo de Java (JDK)](https://openjdk.java.net/) 8 o posterior
- [Maven](https://maven.apache.org/)

> [!div class="sxs-lookup"]
### <a name="download-and-configure-the-quickstart-app"></a>Descarga y configuración de la aplicación de inicio rápido

#### <a name="step-1-configure-the-application-in-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
Para que el ejemplo de código de esta guía de inicio rápido funcione, debe crear un secreto de cliente y agregar el permiso de aplicación **User.Read.All** de Graph API.
> [!div class="nextstepaction"]
> [Realizar estos cambios por mí]()

> [!div class="alert alert-info"]
> ![Ya configurada](media/quickstart-v2-netcore-daemon/green-check.png) La aplicación está configurada con estos atributos.

#### <a name="step-2-download-the-java-project"></a>Paso 2: Descarga del proyecto de Java

> [!div class="sxs-lookup nextstepaction"]
> [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-java-daemon/archive/master.zip)

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

Si es usuario estándar de su inquilino, deberá pedir a un administrador global que conceda consentimiento del administrador para su aplicación. Para ello, proporcione la siguiente dirección URL a su administrador:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```
#### <a name="step-4-run-the-application"></a>Paso 4: Ejecución de la aplicación

Puede probar el ejemplo directamente mediante la ejecución del método main de ClientCredentialGrant.Java desde el IDE.

Desde el shell o la línea de comandos, ejecute:

```
$ mvn clean compile assembly:single
```

Se generará el archivo msal-client-credential-secret-1.0.0.jar en el directorio /targets. Ejecútelo con el archivo ejecutable de Java como se indica a continuación:

```
$ java -jar msal-client-credential-secret-1.0.0.jar
```

Después de la ejecución, la aplicación debe mostrar la lista de usuarios del inquilino configurado.

> [!IMPORTANT]
> Esta aplicación de inicio rápido usa un secreto de cliente para identificarse como cliente confidencial. Como el secreto de cliente se agrega como texto sin formato a los archivos del proyecto, por motivos de seguridad, se recomienda que use un certificado en lugar de un secreto de cliente antes de considerar el uso de la aplicación en producción. Para más información sobre cómo usar un certificado, consulte [estas instrucciones](https://github.com/Azure-Samples/ms-identity-java-daemon/tree/master/msal-client-credential-certificate) en el mismo repositorio de GitHub que este ejemplo, pero en la segunda carpeta, **msal-client-credential-certificate**.

## <a name="more-information"></a>Más información

### <a name="msal-java"></a>Java de MSAL

[MSAL para Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para acceder a una API protegida por la Plataforma de identidad de Microsoft. Como se ha descrito, en este inicio rápido se solicitan tokens mediante la propia identidad de la aplicación, en lugar de permisos delegados. El flujo de autenticación usado en este caso se conoce como *[flujo de OAuth de credenciales de cliente](v2-oauth2-client-creds-grant-flow.md)* . Para más información sobre cómo usar MSAL para Java con aplicaciones de demonio, consulte [este artículo](scenario-daemon-overview.md).

Agregue MSAL4J a la aplicación con Maven o Gradle para administrar las dependencias al realizar los cambios siguientes en el archivo pom.xml (Maven) o build.gradle (Gradle) de la aplicación.

En pom.xml:

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>msal4j</artifactId>
    <version>1.0.0</version>
</dependency>
```

En build.gradle:

```$xslt
compile group: 'com.microsoft.azure', name: 'msal4j', version: '1.0.0'
```

### <a name="msal-initialization"></a>Inicialización de MSAL

Para agregar una referencia a MSAL for Java, incorpore el código siguiente al principio del archivo en el que va a usar MSAL4J:

```Java
import com.microsoft.aad.msal4j.*;
```

A continuación, realice la inicialización de MSAL con el siguiente código:

```Java
IClientCredential credential = ClientCredentialFactory.createFromSecret(CLIENT_SECRET);

ConfidentialClientApplication cca =
        ConfidentialClientApplication
                .builder(CLIENT_ID, credential)
                .authority(AUTHORITY)
                .build();
```

> | Donde: |Descripción |
> |---------|---------|
> | `CLIENT_SECRET` | Es el secreto de cliente creado para la aplicación en Azure Portal. |
> | `CLIENT_ID` | Es el **Identificador de aplicación (cliente)** de la aplicación registrada en Azure Portal. Puede encontrar este valor en la página **Información general** de la aplicación en Azure Portal. |
> | `AUTHORITY`    | El punto de conexión STS para el usuario que se autenticará. Normalmente `https://login.microsoftonline.com/{tenant}` en la nube pública, donde {tenant} es el nombre o el identificador del inquilino.|

### <a name="requesting-tokens"></a>Solicitud de tokens

Para solicitar un token mediante la identidad de la aplicación, use el método `acquireToken`:

```Java
IAuthenticationResult result;
     try {
         SilentParameters silentParameters =
                 SilentParameters
                         .builder(SCOPE)
                         .build();

         // try to acquire token silently. This call will fail since the token cache does not
         // have a token for the application you are requesting an access token for
         result = cca.acquireTokenSilently(silentParameters).join();
     } catch (Exception ex) {
         if (ex.getCause() instanceof MsalException) {

             ClientCredentialParameters parameters =
                     ClientCredentialParameters
                             .builder(SCOPE)
                             .build();

             // Try to acquire a token. If successful, you should see
             // the token information printed out to console
             result = cca.acquireToken(parameters).join();
         } else {
             // Handle other exceptions accordingly
             throw ex;
         }
     }
     return result;
```

> |Donde:| Descripción |
> |---------|---------|
> | `SCOPE` | Contiene los ámbitos solicitados. Con clientes confidenciales se debe usar un formato similar a `{Application ID URI}/.default` para indicar que los ámbitos que se solicitan son los definidos estáticamente en el objeto de aplicación establecido en Azure Portal (con Microsoft Graph, `{Application ID URI}` apunta a `https://graph.microsoft.com`). Con API web personalizadas, `{Application ID URI}` se define en la sección **Exponer una API** en **Registros de aplicaciones** de Azure Portal.|

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las aplicaciones demonio, consulte la página de aterrizaje del escenario.

> [!div class="nextstepaction"]
> [Aplicación demonio que llama a las API web](scenario-daemon-overview.md)
