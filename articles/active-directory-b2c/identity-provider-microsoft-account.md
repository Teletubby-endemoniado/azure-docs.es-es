---
title: Configuración del registro y del inicio de sesión con una cuenta de Microsoft
titleSuffix: Azure AD B2C
description: Permita suscribirse e iniciar sesión a los clientes con cuentas Microsoft en las aplicaciones con Azure Active Directory B2C.
services: active-directory-b2c
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/16/2021
ms.custom: project-no-code
ms.author: kengaderdus
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 5da695eeee6ef9ed26dcdbd29cf5b5684e277b12
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130227774"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Configuración de la suscripción y del inicio de sesión con una cuenta Microsoft mediante Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

### <a name="verify-the-applications-publisher-domain"></a>Comprobación del dominio del publicador de la aplicación
A partir de noviembre de 2020, los nuevos registros de aplicaciones se muestran como no comprobados en el mensaje de consentimiento del usuario, a menos que [se haya comprobado el dominio del publicador de la aplicación](../active-directory/develop/howto-configure-publisher-domain.md) ***y además*** se haya comprobado la identidad de la empresa con Microsoft Partner Network y se haya asociado a la aplicación. [Obtenga más información](../active-directory/develop/publisher-verification-overview.md) sobre este cambio. Tenga en cuenta que, en los flujos de usuario de Azure AD B2C, el dominio del publicador solo aparece cuando se usa una [cuenta de Microsoft](../active-directory-b2c/identity-provider-azure-ad-single-tenant.md) u otro inquilino de Azure AD como proveedor de identidades. Para cumplir estos nuevos requisitos, haga lo siguiente:

1. [Compruebe la identidad de la empresa con la cuenta de Microsoft Partner Network (MPN)](/partner-center/verification-responses). Este proceso comprueba la información sobre su empresa y el contacto principal de esta.
1. Complete el proceso de comprobación del publicador para asociar su cuenta de MPN al registro de la aplicación usando una de las siguientes opciones:
   - Si el registro de la aplicación para el proveedor de identidades de la cuenta Microsoft está en un inquilino de Azure AD, [compruebe la aplicación en el portal de registro de aplicaciones](../active-directory/develop/mark-app-as-publisher-verified.md).
   - Si el registro de la aplicación para el proveedor de identidades de la cuenta de Microsoft está en un inquilino de Azure AD B2C, [marque la aplicación como publicador comprobado mediante las API de Microsoft Graph](../active-directory/develop/troubleshoot-publisher-verification.md#making-microsoft-graph-api-calls) (por ejemplo, mediante Probador de Graph). La interfaz de usuario para establecer el publicador comprobado de una aplicación está deshabilitada actualmente para inquilinos de Azure AD B2C.

## <a name="create-a-microsoft-account-application"></a>Creación de una aplicación de cuenta Microsoft

Para habilitar el inicio de sesión para los usuarios con una cuenta de Microsoft en Azure Active Directory B2C (Azure AD B2C), tiene que crear una aplicación en [Azure Portal](https://portal.azure.com). Para más información, consulte [Registro de una aplicación con la plataforma de identidad de Microsoft](../active-directory/develop/quickstart-register-app.md). Si todavía no tiene una cuenta Microsoft, puede obtenerla en [https://www.live.com/](https://www.live.com/).

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD. Seleccione el icono **Directorios y suscripciones** en la barra de herramientas del portal.
1. En la página **Configuración del portal | Directorios y suscripciones**, busque el directorio de Azure AD en la lista **Nombre de directorio** y, después, seleccione **Cambiar**.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Registros de aplicaciones**.
1. Seleccione **Nuevo registro**.
1. Escriba el **nombre** de la aplicación. Por ejemplo, *MSAapp1*.
1. En **Tipos de cuenta compatibles**, seleccione **Cuentas en cualquier directorio organizativo (cualquier directorio de Azure AD: multiinquilino) y cuentas de Microsoft personales (como Skype o Xbox)** .

   Para más información sobre las distintas opciones de tipo de cuenta, consulte [Inicio rápido: Registro de una aplicación en la plataforma de identidad de Microsoft](../active-directory/develop/quickstart-register-app.md).
1. En **URI de redirección (opcional)** , seleccione **Web** e introduzca `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Si usa un [dominio personalizado](custom-domain.md), escriba `https://your-domain-name/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Reemplace `your-tenant-name` por el nombre del inquilino y `your-domain-name` por el dominio personalizado.
1. Seleccione **Registrar**.
1. Registre el **Id. de aplicación (cliente)** que se muestra en la página de información general de la aplicación. Necesitará el id. de cliente al configurar el proveedor de identidades en la siguiente sección.
1. Seleccione **Certificates & secrets** (Certificados y secretos)
1. Haga clic en **Nuevo secreto de cliente**.
1. Escriba una **Descripción** del secreto, por ejemplo, *contraseña de la aplicación 1* y haga clic en **Agregar**.
1. Anote la contraseña de la aplicación que se muestra en la columna **Value**. Necesitará el secreto de cliente al configurar el proveedor de identidades en la siguiente sección.

::: zone pivot="b2c-user-flow"

## <a name="configure-microsoft-as-an-identity-provider"></a>Configuración de Microsoft como proveedor de identidades

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el icono **Directorios y suscripciones** en la barra de herramientas del portal.
1. En la página **Configuración del portal | Directorios y suscripciones**, busque el directorio de Azure AD B2C en la lista **Nombre de directorio** y seleccione **Cambiar**.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Proveedores de identidades** y, luego, **Cuenta Microsoft**.
1. Escriba un **nombre**. Por ejemplo, *MSA*.
1. En **Id. de cliente**, escriba el identificador (cliente) de la aplicación de Azure AD que creó anteriormente.
1. En **Secreto de cliente**, escriba el secreto de cliente que ha anotado.
1. Seleccione **Guardar**.

## <a name="add-microsoft-identity-provider-to-a-user-flow"></a>Adición del proveedor de identidades de Microsoft a un flujo de usuario 

En este punto se ha configurado el proveedor de identidades de Microsoft, pero aún no está disponible en ninguna de las páginas de inicio de sesión. Para agregar el proveedor de identidades de Microsoft a un flujo de usuario:

1. En el inquilino de Azure AD B2C, seleccione **Flujos de usuario**.
1. Haga clic en el flujo de usuario donde quiera agregar el proveedor de identidades de Microsoft.
1. En **Proveedores de identidades sociales**, seleccione **Cuenta de Microsoft**.
1. Seleccione **Guardar**.
1. Para probar la directiva, seleccione **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *testapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione el botón **Ejecutar flujo de usuario**.
1. En la página de registro o de inicio de sesión, seleccione **Microsoft** para iniciar sesión con la cuenta de Microsoft.

Si el proceso de inicio de sesión se completa correctamente, el explorador se redirige a `https://jwt.ms`, que muestra el contenido del token devuelto por Azure AD B2C.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="configuring-optional-claims"></a>Configuración de notificaciones opcionales

Si quiere obtener las notificaciones `family_name` y `given_name` de Azure AD, puede configurar notificaciones opcionales para la aplicación en la interfaz de usuario de Azure Portal o el manifiesto de aplicación. Para obtener más información, consulte [Procedimientos: Proporcionar notificaciones opcionales a la aplicación de Azure AD](../active-directory/develop/active-directory-optional-claims.md).

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Busque y seleccione **Azure Active Directory**.
1. En la sección **Administrar**, seleccione **Registros de aplicaciones**.
1. Seleccione en la lista la aplicación para la que desea configurar notificaciones opcionales.
1. En la sección **Administrar**, seleccione **Configuración del token (versión preliminar)** .
1. Seleccione **Agregar notificación opcional**.
1. Seleccione el tipo de token que desea configurar.
1. Seleccione las notificaciones opcionales que va a agregar.
1. Haga clic en **Agregar**.

## <a name="create-a-policy-key"></a>Creación de una clave de directiva

Ahora que creó la aplicación en el inquilino de Azure AD, deberá almacenar el secreto de cliente de esa aplicación en el inquilino de Azure AD B2C.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el icono **Directorios y suscripciones** en la barra de herramientas del portal.
1. En la página **Configuración del portal | Directorios y suscripciones**, busque el directorio de Azure AD B2C en la lista **Nombre de directorio** y seleccione **Cambiar**.
1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. En la página de introducción, seleccione **Identity Experience Framework**.
1. Seleccione **Claves de directiva** y luego **Agregar**.
1. En **Opciones**, elija `Manual`.
1. Escriba un **nombre** para la clave de directiva. Por ejemplo, `MSASecret`. Se agregará el prefijo `B2C_1A_` automáticamente al nombre de la clave.
1. En **Secreto**, escriba el secreto de cliente que anotó en la sección anterior.
1. En **Uso de claves**, seleccione `Signature`.
1. Haga clic en **Crear**.

## <a name="configure-microsoft-as-an-identity-provider"></a>Configuración de Microsoft como proveedor de identidades

Para permitir que los usuarios inicien sesión con una cuenta de Microsoft, deberá definir la cuenta como un proveedor de notificaciones con el que Azure AD B2C pueda comunicarse mediante un punto de conexión. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Puede definir Azure AD como proveedor de notificaciones. Para ello, agregue el elemento **ClaimsProvider** en el archivo de extensión de la directiva.

1. Abra el archivo de directiva *TrustFrameworkExtensions.xml*.
1. Busque el elemento **ClaimsProviders**. Si no existe, agréguelo debajo del elemento raíz.
1. Agregue un nuevo elemento **ClaimsProvider** tal como se muestra a continuación:

    ```xml
    <ClaimsProvider>
      <Domain>live.com</Domain>
      <DisplayName>Microsoft Account</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="MSA-MicrosoftAccount-OpenIdConnect">
          <DisplayName>Microsoft Account</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="ProviderName">https://login.live.com</Item>
            <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="scope">openid profile email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="client_id">Your Microsoft application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
            <OutputClaim ClaimTypeReferenceId="email" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Reemplace el valor de **client_id** por el *identificador de la aplicación (cliente)* de la aplicación de Azure AD.
1. Guarde el archivo.

Ya configuró la directiva para que Azure AD B2C sepa cómo comunicarse con su aplicación de cuenta de Microsoft en Azure AD.

[!INCLUDE [active-directory-b2c-add-identity-provider-to-user-journey](../../includes/active-directory-b2c-add-identity-provider-to-user-journey.md)]


```xml
<OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsignin">
  <ClaimsProviderSelections>
    ...
    <ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
  </ClaimsProviderSelections>
  ...
</OrchestrationStep>

<OrchestrationStep Order="2" Type="ClaimsExchange">
  ...
  <ClaimsExchanges>
    <ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-MicrosoftAccount-OpenIdConnect" />
  </ClaimsExchanges>
</OrchestrationStep>
```

[!INCLUDE [active-directory-b2c-configure-relying-party-policy](../../includes/active-directory-b2c-configure-relying-party-policy-user-journey.md)]

## <a name="test-your-custom-policy"></a>Prueba de la directiva personalizada

1. Seleccione la directiva de usuarios de confianza, por ejemplo `B2C_1A_signup_signin`.
1. En **Aplicación**, seleccione la aplicación web que [registró anteriormente](tutorial-register-applications.md). La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione el botón **Ejecutar ahora**.
1. En la página de registro o de inicio de sesión, seleccione **Microsoft** para iniciar sesión con la cuenta de Microsoft.

Si el proceso de inicio de sesión se completa correctamente, el explorador se redirige a `https://jwt.ms`, que muestra el contenido del token devuelto por Azure AD B2C.

::: zone-end