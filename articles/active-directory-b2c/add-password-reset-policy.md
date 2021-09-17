---
title: Configuración de un flujo de restablecimiento de contraseña en Azure AD B2C
titleSuffix: Azure AD B2C
description: Obtenga información sobre cómo configurar un flujo de restablecimiento de contraseña en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/01/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: ddf46df1af8dc41e4f50c92eee527ac25e77b08a
ms.sourcegitcommit: 28cd7097390c43a73b8e45a8b4f0f540f9123a6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2021
ms.locfileid: "122777590"
---
# <a name="set-up-a-password-reset-flow-in-azure-active-directory-b2c"></a>Configuración de un flujo de restablecimiento de contraseña en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

## <a name="overview"></a>Información general

En un [recorrido de registro e inicio de sesión](add-sign-up-and-sign-in-policy.md), los usuarios pueden restablecer su propia contraseña mediante el vínculo **¿Ha olvidado la contraseña?** El flujo de autoservicio de restablecimiento de contraseña se aplica a las cuentas locales de Azure AD B2C que usan una [dirección de correo electrónico](sign-in-options.md#email-sign-in) o un [nombre de usuario](sign-in-options.md#username-sign-in) con una contraseña para el inicio de sesión.

El flujo de restablecimiento de contraseña consta de los siguientes pasos:

![Flujo de restablecimiento de contraseña](./media/add-password-reset-policy/password-reset-flow.png)

**1.** En la página de registro e inicio de sesión, el usuario hace clic en el vínculo **¿Ha olvidado la contraseña?** Azure AD B2C inicia el flujo de restablecimiento de contraseña.

**2.** El usuario proporciona su dirección de correo electrónico y selecciona **Enviar código de verificación**. Azure AD B2C envía el código de verificación a la bandeja de entrada del usuario. A continuación, el usuario copia el código de verificación del correo electrónico, lo escribe en la página de restablecimiento de contraseña de Azure AD B2C y selecciona **Comprobar código**.

**3.** A continuación, el usuario puede escribir una nueva contraseña. (Una vez comprobado el correo electrónico, el usuario puede seleccionar el botón **Cambiar correo electrónico**; vea [Ocultar el botón Cambiar correo electrónico](#hiding-the-change-email-button) a continuación).

> [!TIP]
> El flujo del autoservicio de restablecimiento de contraseña permite a los usuarios cambiar su contraseña cuando el usuario la olvida y quiere restablecerla. 
> - En los casos en los que un usuario conoce su contraseña y quiere cambiarla, use un [flujo de cambio de contraseña](add-password-change-policy.md). 
> - En los casos en los que quiere forzar a los usuarios a restablecer sus contraseñas (por ejemplo, cuando inician sesión por primera vez, cuando un administrador las ha restablecido o después de que se hayan migrado a Azure AD B2C con contraseñas aleatorias), use un flujo de [restablecimiento de contraseña forzado](force-password-reset.md).

### <a name="hiding-the-change-email-button"></a>Ocultar el botón Cambiar correo electrónico

Una vez comprobado el correo electrónico, el usuario puede seleccionar **Cambiar correo electrónico**, escribir el otro correo electrónico y repetir la comprobación del correo electrónico desde el principio. Si prefiere ocultar el botón **Cambiar correo electrónico**, puede modificar el archivo CSS para ocultar los elementos HTML asociados en la página. Por ejemplo, puede agregar la entrada CSS siguiente a selfAsserted.HTML y [personalizar la interfaz de usuario con plantillas HTML](customize-ui-with-html.md).

```html
<style type="text/css">
   .changeClaims
   {
     visibility: hidden;
   }
</style>
```

Tenga en cuenta que el nombre predeterminado del botón **Cambiar correo electrónico** de la página selfasserted.html es `changeclaims`. Para encontrar el nombre del botón, inspeccione el origen de la página de registro con una herramienta del explorador (por ejemplo, Inspeccionar).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="self-service-password-reset-recommended"></a>Autoservicio de restablecimiento de contraseña (recomendado)

La nueva experiencia de restablecimiento de contraseña ahora forma parte de la directiva de registro o inicio de sesión. Cuando el usuario selecciona el vínculo **¿Olvidó la contraseña?** , se le envía inmediatamente a la experiencia de contraseña olvidada. La aplicación ya no necesita controlar el [código de error AADB2C90118](#password-reset-policy-legacy) y no necesita una directiva distinta para el restablecimiento de contraseña.

::: zone pivot="b2c-user-flow"

La experiencia de autoservicio de restablecimiento de contraseña se puede configurar para los flujos de usuario de **inicio de sesión (recomendado)** o de **registro e inicio de sesión (recomendado)** . Si no tiene este tipo de flujo de usuario, cree un flujo de usuario de [registro e inicio de sesión](add-sign-up-and-sign-in-policy.md). 

Para habilitar el autoservicio de restablecimiento de contraseña para el flujo de usuario de registro o inicio de sesión:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el icono **Directorios y suscripciones** en la barra de herramientas del portal.
1. En la página **Configuración del portal | Directorios y suscripciones**, busque el directorio de Azure AD B2C en la lista **Nombre de directorio** y seleccione **Cambiar**.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. Seleccione **Flujos de usuario**.
1. Seleccione un flujo de usuario de registro o inicio de sesión (de tipo **recomendado**) que quiera personalizar.
1. En el menú de la izquierda, en **Configuración**, seleccione **Propiedades**.
1. En **Configuración de la contraseña**, seleccione **Autoservicio de restablecimiento de contraseña**.
1. Seleccione **Guardar**.
1. En el menú de la izquierda, en **Personalizar**, seleccione **Diseños de página**.
1. En **Versión de diseño de página**, elija **2.1.3** o superior.
1. Seleccione **Guardar**.

::: zone-end

::: zone pivot="b2c-custom-policy"

En las secciones siguientes se describe cómo agregar una experiencia de autoservicio de contraseña a una directiva personalizada. El ejemplo se basa en los archivos de directivas incluidos en el [paquete de inicio de directivas personalizadas](./tutorial-create-user-flows.md?pivots=b2c-custom-policy#custom-policy-starter-pack). 

> [!TIP]
> Puede encontrar un ejemplo completo de la directiva de registro o inicio de sesión con contrzseña en [GitHub](https://github.com/azure-ad-b2c/samples/tree/master/policies/embedded-password-reset).

### <a name="indicate-a-user-selected-the-forgot-your-password-link"></a>Indicación a un usuario seleccionado el vínculo ¿Olvidó la contraseña?

Para indicar a la directiva que el usuario ha seleccionado el vínculo **¿Olvidó la contraseña?** , defina una notificación booleana. Esta notificación se usará para dirigir el recorrido del usuario al perfil técnico de contraseña olvidada. Esta notificación también se puede emitir al token, de modo que la aplicación sabe que el usuario ha iniciado sesión mediante el flujo de contraseña olvidada.

Las notificaciones se declaran en el [esquema de notificaciones](claimsschema.md). Abra el archivo de extensiones de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.

1. Busque el elemento [BuildingBlocks](buildingblocks.md). Si el elemento no existe, agréguelo.
1. Busque el elemento [ClaimsSchema](claimsschema.md). Si el elemento no existe, agréguelo.
1. Agregue las notificaciones siguientes al elemento **ClaimsSchema**. 

```XML
<!-- 
<BuildingBlocks>
  <ClaimsSchema> -->
    <ClaimType Id="isForgotPassword">
      <DisplayName>isForgotPassword</DisplayName>
      <DataType>boolean</DataType>
      <AdminHelpText>Whether the user has selected Forgot your Password</AdminHelpText>
    </ClaimType>
  <!--
  </ClaimsSchema>
</BuildingBlocks> -->
```

### <a name="upgrade-the-page-layout-version"></a>Actualización de la versión de diseño de página

La [versión de diseño de página](contentdefinitions.md#migrating-to-page-layout) `2.1.2` es necesaria para habilitar el flujo de autoservicio de restablecimiento de contraseña dentro del recorrido de registro o inicio de sesión.

1. Busque el elemento [BuildingBlocks](buildingblocks.md). Si el elemento no existe, agréguelo.
1. Busque el elemento [ContentDefinitions](contentdefinitions.md). Si el elemento no existe, agréguelo.
1. Modifique el elemento **DataURI** en el elemento **ContentDefinition** con el identificador **api.signuporsignin**, como se muestra a continuación.

```xml
<!-- 
<BuildingBlocks>
  <ContentDefinitions> -->
    <ContentDefinition Id="api.signuporsignin">
      <DataUri>urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:2.1.2</DataUri>
    </ContentDefinition>
  <!-- 
  </ContentDefinitions>
</BuildingBlocks> -->
```

Para iniciar la notificación `isForgotPassword`, se usa un perfil técnico de transformación de notificaciones. Más adelante se hará referencia a este perfil técnico. Cuando se invoca, establecerá el valor de la notificación `isForgotPassword` en `true`. Busque el elemento `ClaimsProviders`. Si el elemento no existe, agréguelo. Luego, agregue el siguiente proveedor de notificaciones:  

```xml
<!-- 
<ClaimsProviders> -->
  <ClaimsProvider>
    <DisplayName>Local Account</DisplayName>
    <TechnicalProfiles>
      <TechnicalProfile Id="ForgotPassword">
        <DisplayName>Forgot your password?</DisplayName>
        <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="isForgotPassword" DefaultValue="true" AlwaysUseDefaultValue="true"/>
        </OutputClaims>
      </TechnicalProfile>
      <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
        <Metadata>
          <Item Key="setting.forgotPasswordLinkOverride">ForgotPasswordExchange</Item>
        </Metadata>
      </TechnicalProfile>
    </TechnicalProfiles>
  </ClaimsProvider>
<!-- 
</ClaimsProviders> -->
```

El perfil técnico `SelfAsserted-LocalAccountSignin-Email` `setting.forgotPasswordLinkOverride` define el intercambio de notificaciones de restablecimiento de contraseña que se ejecuta en el recorrido del usuario. 

### <a name="add-the-password-reset-sub-journey"></a>Adición del subrecorrido de restablecimiento de contraseña

El recorrido ahora incluirá la funcionalidad para que el usuario inicie sesión, se registre y realice el restablecimiento de contraseña. Para organizar mejor el recorrido del usuario, se puede usar un [subrecorrido](subjourneys.md) para administrar el flujo de restablecimiento de contraseña.

El subrecorrido se llamará desde el recorrido del usuario y llevará a cabo los pasos específicos para proporcionar al usuario la experiencia de restablecimiento de contraseña. Use el subrecorrido de tipo `Call` para que una vez que se complete el subrecorrido, se devuelva el control al paso de orquestación que lo inició.

Busque el elemento `SubJourneys`. Si el elemento no existe, agréguelo después del elemento `User Journeys`. Luego, agregue el siguiente subrecorrido:

```xml
<!--
<SubJourneys>-->
  <SubJourney Id="PasswordReset" Type="Call">
    <OrchestrationSteps>
      <!-- Validate user's email address. -->
      <OrchestrationStep Order="1" Type="ClaimsExchange">
        <ClaimsExchanges>
          <ClaimsExchange Id="PasswordResetUsingEmailAddressExchange" TechnicalProfileReferenceId="LocalAccountDiscoveryUsingEmailAddress" />
        </ClaimsExchanges>
      </OrchestrationStep>

      <!-- Collect and persist a new password. -->
      <OrchestrationStep Order="2" Type="ClaimsExchange">
        <ClaimsExchanges>
          <ClaimsExchange Id="NewCredentials" TechnicalProfileReferenceId="LocalAccountWritePasswordUsingObjectId" />
        </ClaimsExchanges>
      </OrchestrationStep>
    </OrchestrationSteps>
  </SubJourney>
<!--
</SubJourneys>-->
```

### <a name="prepare-your-user-journey"></a>Preparación del recorrido del usuario

Deberá conectar el vínculo **¿Olvidó la contraseña?** al subrecorrido de contraseña olvidada. Para ello, haga referencia al identificador de subrecorrido de contraseña olvidada dentro del elemento **ClaimsProviderSelection** del paso **CombinedSignInAndSignUp**.

Si no tiene su propio recorrido de usuario personalizado con un paso **CombinedSignInAndSignUp**, use el procedimiento siguiente para duplicar un recorrido de usuario de registro o inicio de sesión existente. De lo contrario, continúe con la sección siguiente.

1. Abra el archivo *TrustFrameworkBase.xml* del paquete de inicio.
2. Busque y copie todo el contenido del elemento **UserJourney** que incluye `Id="SignUpOrSignIn"`.
3. Abra el archivo *TrustFrameworkExtensions.xml* y busque el elemento **UserJourneys**. Si el elemento no existe, agréguelo.
4. Cree un elemento secundario del elemento **UserJourneys**; para ello, pegue todo el contenido del elemento **UserJourney** que copió en el paso 2.
5. Cambie el identificador del recorrido del usuario. Por ejemplo, `Id="CustomSignUpSignIn"`.

### <a name="connect-the-forgot-password-link-to-the-forgot-password-sub-journey"></a>Conexión del vínculo ¿Olvidó la contraseña? al subrecorrido de contraseña olvidada 

En el recorrido del usuario, puede representar el subrecorrido de contraseña olvidada como **ClaimsProviderSelection**. Al agregar este elemento se conecta el vínculo **¿Olvidó la contraseña?** al subrecorrido de contraseña olvidada.

1. En el recorrido del usuario, busque el elemento del paso de orquestación que incluye `Type="CombinedSignInAndSignUp"` o `Type="ClaimsProviderSelection"`. Normalmente es el primer paso de orquestación. El elemento **ClaimsProviderSelections** contiene una lista de proveedores de identidades que puede usar un usuario para iniciar sesión. Agregue la línea siguiente:
    
    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ForgotPasswordExchange" />
    ```

1. En el paso de orquestación siguiente, agregue un elemento **ClaimsExchange**. Agregue la línea siguiente:

    ```xml
    <ClaimsExchange Id="ForgotPasswordExchange" TechnicalProfileReferenceId="ForgotPassword" />
    ```
    
1. Agregue el siguiente paso de orquestación entre el paso actual y el paso siguiente. El nuevo paso de orquestación que agrega comprueba si la notificación `isForgotPassword` existe. Si la notificación existe, invoca el [subrecorrido de restablecimiento de contraseña](#add-the-password-reset-sub-journey). 

    ```xml
    <OrchestrationStep Order="3" Type="InvokeSubJourney">
      <Preconditions>
        <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
          <Value>isForgotPassword</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
      </Preconditions>
      <JourneyList>
        <Candidate SubJourneyReferenceId="PasswordReset" />
      </JourneyList>
    </OrchestrationStep>
    ```
    
1. Después de agregar el nuevo paso de orquestación, vuelva a enumerar los pasos de forma secuencial sin omitir los enteros de 1 a N.

### <a name="set-the-user-journey-to-be-executed"></a>Establecimiento del recorrido de usuario que se va a ejecutar

Ahora que ha modificado o creado un recorrido de usuario, en la sección **Usuario de confianza**, especifique el recorrido que ejecutará Azure AD B2C para esta directiva personalizada. En el elemento [RelyingParty](relyingparty.md), busque el elemento **DefaultUserJourney**. Actualice **DefaultUserJourney ReferenceId** para que coincida con el identificador del recorrido del usuario en el que ha agregado el elemento **ClaimsProviderSelections**.

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="CustomSignUpSignIn" />
  ...
</RelyingParty>
```

### <a name="indicate-the-forgot-password-flow-to-your-app"></a>Indicación del flujo de contraseña olvidada a la aplicación

Es posible que la aplicación deba detectar si el usuario inició sesión mediante el flujo de usuario de contraseña olvidada. La solicitud **isForgotPassword** contiene un valor booleano que lo indica, que se puede emitir en el token enviado a la aplicación. Si es necesario, agregue `isForgotPassword` a las notificaciones de salida en la sección **Usuario de confianza**. La aplicación puede comprobar la notificación `isForgotPassword` para determinar si el usuario restablece la contraseña.

```xml
<RelyingParty>
  <OutputClaims>
    ...
    <OutputClaim ClaimTypeReferenceId="isForgotPassword" DefaultValue="false" />
  </OutputClaims>
</RelyingParty>
```


### <a name="upload-the-custom-policy"></a>Carga de la directiva personalizada

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione el icono **Directorios y suscripciones** en la barra de herramientas del portal.
1. En la página **Configuración del portal | Directorios y suscripciones**, busque el directorio de Azure AD B2C en la lista **Nombre de directorio** y seleccione **Cambiar**.
1. En Azure Portal, busque y seleccione **Azure AD B2C**.
1. En **Directivas**, seleccione **Identity Experience Framework**.
1. Seleccione **Cargar directiva personalizada** y, luego, cargue los dos archivos de directiva modificados en el orden siguiente:
   1. La directiva de extensión, por ejemplo `TrustFrameworkExtensions.xml`.
   2. La directiva de usuario de confianza, por ejemplo `SignUpSignIn.xml`.

::: zone-end

### <a name="test-the-password-reset-flow"></a>Prueba del flujo de restablecimiento de contraseña

1. Seleccione un flujo de usuario de registro o inicio de sesión (de tipo recomendado) que quiera probar.
1. Seleccione **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *webapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Seleccione **Ejecutar flujo de usuario**.
1. En la página de registro o inicio de sesión, seleccione **¿Olvidó la contraseña?**
1. Compruebe la dirección de correo electrónico de la cuenta que creó anteriormente y, luego seleccione **Continuar**.
1. Ahora tiene la oportunidad de cambiar la contraseña para el usuario. Cambie la contraseña y seleccione **Continuar**. El token se devuelve al `https://jwt.ms` y debe mostrarse.
1. Compruebe el valor de la notificación `isForgotPassword` del token devuelto. Si existe y está establecido en true, indica que el usuario ha restablecido la contraseña.

## <a name="password-reset-policy-legacy"></a>Directiva de restablecimiento de contraseña (heredada)

Si la experiencia de [autoservicio de restablecimiento de contraseña](#self-service-password-reset-recommended) no está habilitada, al hacer clic en este vínculo no se desencadena automáticamente un flujo de usuario de restablecimiento de contraseña. sino que se devuelve a la aplicación el código de error `AADB2C90118`. Para controlar este código de error, la aplicación debe reinicializar la biblioteca de autenticación para autenticar un flujo de usuario de restablecimiento de contraseña de Azure AD B2C.

En el diagrama siguiente:

1. Desde la aplicación, el usuario hace clic en Iniciar sesión. La aplicación inicia una solicitud de autorización y lleva al usuario a Azure AD B2C para que finalice el proceso. La solicitud de autorización especifica el nombre de la directiva de registro o inicio de sesión, por ejemplo, **B2C_1_signup_signin**.
1. El usuario selecciona el vínculo **¿Olvidó la contraseña?** Azure AD B2C devuelve el código de error AADB2C90118 a la aplicación.
1. La aplicación controla el código de error e inicia una nueva solicitud de autorización. La solicitud de autorización especifica el nombre de la directiva de restablecimiento de contraseña, por ejemplo, **B2C_1_pwd_reset**.

![Flujo de usuario de restablecimiento de contraseña heredada](./media/add-password-reset-policy/password-reset-flow-legacy.png)

Para ver un ejemplo, eche un vistazo a un [ejemplo sencillo de ASP.NET](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI) que muestra la vinculación de los flujos de usuario.

::: zone pivot="b2c-user-flow"

### <a name="create-a-password-reset-user-flow"></a>Creación de un flujo de usuario de restablecimiento de contraseña

Para permitir que los usuarios de la aplicación restablezcan la contraseña, se crea un flujo de usuario de restablecimiento de contraseña.

1. En el menú de información general del inquilino de Azure AD B2C, seleccione **Flujos de usuario** y **Nuevo flujo de usuario**.
1. En la página **Crear un flujo de usuario**, seleccione el flujo de usuario **Restablecer contraseña**. 
1. En **Seleccione una versión**, elija **Recomendada** y, luego, seleccione **Crear**.
1. Escriba un **nombre** para el flujo de usuario. Por ejemplo, *passwordreset1*.
1. En **Proveedores de identidades**, habilite **Reset password using username** (Restablecer contraseña mediante el nombre de usuario) o **Reset password using email address** (Restablecer contraseña mediante la dirección de correo electrónico).
1. En **Autenticación multifactor**, si quiere exigir la comprobación de la identidad de los usuarios con un segundo método de autenticación, elija el tipo de método y cuándo aplicar la autenticación multifactor (MFA). [Más información](multi-factor-authentication.md).
1. En **Acceso condicional**, si ha configurado directivas de acceso condicional para el inquilino de Azure AD B2C y quiere habilitarlas para este flujo de usuario, active la casilla **Aplicar directivas de acceso condicional**. No es necesario especificar un nombre de directiva. [Más información](conditional-access-user-flow.md?pivots=b2c-user-flow).
1. 1. En **Notificaciones de la aplicación**, seleccione **Mostrar más** y elija las notificaciones que quiere que se devuelvan en los tokens de autorización enviados de vuelta a la aplicación. Por ejemplo, seleccione **Id. de objeto del usuario**.
1. Seleccione **Aceptar**.
1. Seleccione **Crear** para agregar el flujo de usuario. El prefijo *B2C_1* se anexa automáticamente al nombre.

### <a name="test-the-user-flow"></a>Prueba del flujo de usuario

1. Seleccione el flujo de usuario que ha creado para abrir su página de información general y, luego, elija **Ejecutar flujo de usuario**.
1. En **Aplicación**, seleccione la aplicación web denominada *webapp1* que registró anteriormente. La **dirección URL de respuesta** debe mostrar `https://jwt.ms`.
1. Haga clic en **Ejecutar flujo de usuario**, compruebe la dirección de correo electrónico de la cuenta que creó previamente y seleccione **Continuar**.
1. Ahora puede cambiar la contraseña del usuario. Cambie la contraseña y seleccione **Continuar**. El token se devuelve al `https://jwt.ms` y debe mostrarse.

::: zone-end

::: zone pivot="b2c-custom-policy"

### <a name="create-a-password-reset-policy"></a>Crear una directiva de restablecimiento de contraseña

Las directivas personalizadas son un conjunto de archivos XML que se cargan en el inquilino de Azure AD B2C para definir recorridos de usuario. Proporcionamos paquetes de inicio con varias directivas predefinidas, entre las que se incluyen: registro e inicio de sesión, restablecimiento de contraseña y directiva de edición de perfiles. Para obtener información, consulte [Introducción a las directivas personalizadas en Azure AD B2C](tutorial-create-user-flows.md?pivots=b2c-custom-policy).

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

Configure un [restablecimiento de contraseña forzoso](force-password-reset.md).
