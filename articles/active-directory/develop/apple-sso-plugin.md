---
title: Complemento de Microsoft Enterprise SSO para dispositivos Apple
titleSuffix: Microsoft identity platform | Azure
description: Obtenga información sobre el complemento de SSO de Azure Active Directory para dispositivos iOS, iPadOS y macOS.
services: active-directory
author: brandwe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/10/2021
ms.author: brandwe
ms.reviewer: brandwe
ms.custom: aaddev, has-adal-ref
ms.openlocfilehash: 7fbe4e45e48d3416f530b6845faf702959f92463
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131067344"
---
# <a name="microsoft-enterprise-sso-plug-in-for-apple-devices-preview"></a>Complemento Microsoft Enterprise SSO para dispositivos Apple (versión preliminar)

> [!IMPORTANT]
> Esta característica [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

El *complemento Microsoft Enterprise SSO para dispositivos Apple* proporciona cuentas de inicio de sesión único (SSO) para Azure Active Directory (Azure AD) en macOS, iOS y iPadOS para todas las aplicaciones que admiten la característica de [inicio de sesión único empresarial](https://developer.apple.com/documentation/authenticationservices) de Apple. El complemento proporciona SSO para las aplicaciones antiguas de las que puede depender su empresa, pero que aún no admiten las bibliotecas o protocolos de identidad más recientes. Microsoft ha trabajado estrechamente con Apple para desarrollar este complemento con el fin de aumentar la facilidad de uso de su aplicación y ofrecer la máxima protección disponible.

El complemento Enterprise SSO es actualmente una característica integrada de las siguientes aplicaciones:

* [Microsoft Authenticator](https://support.microsoft.com/account-billing/how-to-use-the-microsoft-authenticator-app-9783c865-0308-42fb-a519-8cf666fe0acc): iOS, iPadOS
* [Portal de empresa](/mem/intune/apps/apps-company-portal-macos) de Microsoft Intune: macOS

## <a name="features"></a>Características

El complemento Microsoft Enterprise SSO para dispositivos de Apple ofrece las siguientes ventajas:

- Proporciona SSO para las cuentas de Azure AD en todas las aplicaciones que admiten la característica Enterprise SSO de Apple.
- Se puede habilitar con cualquier solución de administración de dispositivos móviles (MDM).
- Extiende el inicio de sesión único a las aplicaciones que aún no usan las bibliotecas de la Plataforma de identidad de Microsoft.
- Extiende el inicio de sesión único a las aplicaciones que usan OAuth 2, OpenID Connect y SAML.

## <a name="requirements"></a>Requisitos

Para usar el complemento Microsoft Enterprise SSO en dispositivos Apple:

- El dispositivo debe *admitir* y tener una aplicación instalada que incluya el complemento Microsoft Enterprise SSO para dispositivos Apple:
  - iOS 13.0 y versiones posteriores: [aplicación Microsoft Authenticator](https://support.microsoft.com/account-billing/how-to-use-the-microsoft-authenticator-app-9783c865-0308-42fb-a519-8cf666fe0acc)
  - iPadOS 13.0 y versiones posteriores: [aplicación Microsoft Authenticator](https://support.microsoft.com/account-billing/how-to-use-the-microsoft-authenticator-app-9783c865-0308-42fb-a519-8cf666fe0acc)
  - macOS 10.15 y versiones posteriores: [aplicación Portal de empresa de Intune](/mem/intune/user-help/enroll-your-device-in-intune-macos-cp)
- El dispositivo debe *inscribirse en MDM*, por ejemplo, por medio de Microsoft Intune.
- Se debe *insertar en el dispositivo* la configuración para habilitar el complemento Enterprise SSO. Apple requiere esta restricción de seguridad.

### <a name="ios-requirements"></a>Requisitos de iOS
- iOS 13.0 o posterior debe estar instalado en el dispositivo.
- Debe instalarse una aplicación de Microsoft que proporcione el complemento Microsoft Enterprise SSO para dispositivos de Apple en el dispositivo. En el caso de la versión preliminar pública, estas aplicaciones se refieren a la [aplicación Microsoft Authenticator](https://support.microsoft.com/account-billing/how-to-use-the-microsoft-authenticator-app-9783c865-0308-42fb-a519-8cf666fe0acc).


### <a name="macos-requirements"></a>Requisitos de macOS
- macOS 10.15 o una versión posterior debe estar instalado en el dispositivo. 
- Debe instalarse una aplicación de Microsoft que proporcione el complemento Microsoft Enterprise SSO para dispositivos de Apple en el dispositivo. En el caso de la versión preliminar pública, estas aplicaciones incluyen la [aplicación Portal de empresa de Intune](/mem/intune/user-help/enroll-your-device-in-intune-macos-cp).

## <a name="enable-the-sso-plug-in"></a>Habilitación del complemento de SSO

Use la siguiente información para habilitar el complemento de SSO mediante MDM.
### <a name="microsoft-intune-configuration"></a>Configuración de Microsoft Intune

Si usa Microsoft Intune como servicio MDM, puede usar la configuración del perfil de configuración integrado para habilitar el complemento Microsoft Enterprise SSO:

1. Establezca la configuración de la [extensión de la aplicación de SSO](/mem/intune/configuration/device-features-configure#single-sign-on-app-extension) de un perfil de configuración. 
1. Si el perfil todavía no está asignado, [asígnelo a un grupo de usuarios o dispositivos](/mem/intune/configuration/device-profile-assign).

La configuración del perfil que habilita el complemento SSO se aplica automáticamente a los dispositivos del grupo la siguiente vez que cada dispositivo se registra con Intune.

### <a name="manual-configuration-for-other-mdm-services"></a>Configuración manual para otros servicios de MDM

Si no usa Intune para MDM, puede configurar una carga de perfil de inicio de sesión único extensible para dispositivos Apple. Use los parámetros siguientes para configurar el complemento Microsoft Enterprise SSO y sus opciones de configuración.

Configuración de iOS:

- **Identificador de extensión**: `com.microsoft.azureauthenticator.ssoextension`
- **Identificador del equipo**: este campo no es necesario en iOS.

Configuración de macOS:

- **Identificador de extensión**: `com.microsoft.CompanyPortalMac.ssoextension`
- **Identificador del equipo**: `UBF8T346G9`

Configuración común:

- **Tipo**: Redirigir
  - `https://login.microsoftonline.com`
  - `https://login.microsoft.com`
  - `https://sts.windows.net`
  - `https://login.partner.microsoftonline.cn`
  - `https://login.chinacloudapi.cn`
  - `https://login.microsoftonline.de`
  - `https://login.microsoftonline.us`
  - `https://login.usgovcloudapi.net`
  - `https://login-us.microsoftonline.com`
  
### <a name="more-configuration-options"></a>Más opciones de configuración
Se pueden agregar otras opciones de configuración para extender la funcionalidad de SSO a otras aplicaciones.

#### <a name="enable-sso-for-apps-that-dont-use-a-microsoft-identity-platform-library"></a>Habilitación del inicio de sesión único para aplicaciones que no usan una biblioteca de plataforma de identidad de Microsoft

El complemento de inicio de sesión único permite que cualquier aplicación participe en el SSO aunque no se haya desarrollado con un SDK de Microsoft como la Biblioteca de autenticación de Microsoft (MSAL).

El complemento de SSO se instala automáticamente en dispositivos que cumplen los siguientes requisitos:
* Se ha descargado la aplicación Authenticator en iOS o iPad o la aplicación Portal de empresa de Intune en macOS.
* Se ha registrado el dispositivo en la organización. 

Es probable que la organización ya use la aplicación Authenticator en escenarios como la autenticación multifactor, la autenticación sin contraseña y el acceso condicional. Si hace uso de un proveedor de MDM, puede activar el complemento de SSO para las aplicaciones. Microsoft ha facilitado la configuración del complemento dentro de Microsoft Endpoint Manager en Intune. Para configurar estas aplicaciones de forma que usen el complemento de inicio de sesión único, se usa una lista de permitidos.

>[!IMPORTANT]
> El complemento Microsoft Enterprise SSO solo admite aplicaciones que usan webviews o tecnologías de red nativas de Apple. No es compatible con las aplicaciones que distribuyen su propia implementación de niveles de red.  

Use los parámetros siguientes para configurar el complemento Microsoft Enterprise SSO en dispositivos que no usan una biblioteca de la Plataforma de identidad de Microsoft.

#### <a name="enable-sso-for-all-managed-apps"></a>Habilitación del inicio de sesión único para todas las aplicaciones administradas

- **Clave**: `Enable_SSO_On_All_ManagedApps`
- **Tipo**: `Integer`
- **Valor**: 1 o 0.

Cuando esta marca está activa (su valor está establecido en `1`), todas las aplicaciones administradas por MDM que no están en `AppBlockList` pueden participar en el inicio de sesión único.

#### <a name="enable-sso-for-specific-apps"></a>Habilitación del inicio de sesión único para aplicaciones específicas

- **Clave**: `AppAllowList`
- **Tipo**: `String`
- **Valor**: lista delimitada por comas de identificadores de agrupación de aplicaciones que pueden participar en el inicio de sesión único.
- **Ejemplo**: `com.contoso.workapp, com.contoso.travelapp`

>[!NOTE]
> Safari y Safari View Service pueden participar en el inicio de sesión único de manera predeterminada. Se pueden configurar para que *no* participen en el inicio de sesión único si se agregan los identificadores de agrupación de Safari y Safari View Service en AppBlockList. Identificadores de agrupación de iOS: [com.apple.mobilesafari, com.apple.SafariViewService]; identificador de agrupación de macOS: com.apple.Safari

#### <a name="enable-sso-for-all-apps-with-a-specific-bundle-id-prefix"></a>Habilitación del inicio de sesión único para todas las aplicaciones con un prefijo de identificador de agrupación específico
- **Clave**: `AppPrefixAllowList`
- **Tipo**: `String`
- **Valor**: lista delimitada por comas de los prefijos de identificadores de agrupación de las aplicaciones que pueden participar en el inicio de sesión único. Este parámetro permite que todas las aplicaciones que comienzan con un prefijo determinado participen en el SSO.
- **Ejemplo**: `com.contoso., com.fabrikam.`

#### <a name="disable-sso-for-specific-apps"></a>Deshabilitación del inicio de sesión único para aplicaciones específicas

- **Clave**: `AppBlockList`
- **Tipo**: `String`
- **Valor**: lista delimitada por comas de identificadores de agrupación de aplicaciones que no pueden participar en el inicio de sesión único.
- **Ejemplo**: `com.contoso.studyapp, com.contoso.travelapp`

Para *deshabilitar* el inicio de sesión único para Safari o Safari View Service, debe hacerlo explícitamente agregando sus identificadores de agrupación a `AppBlockList`: 

- iOS: `com.apple.mobilesafari`, `com.apple.SafariViewService`
- macOS: `com.apple.Safari`

#### <a name="enable-sso-through-cookies-for-a-specific-application"></a>Habilitación de SSO mediante cookies para una aplicación específica

Algunas aplicaciones que tienen una configuración de red avanzada pueden experimentar problemas inesperados cuando se habilita en ellas el inicio de sesión único. Por ejemplo, podría aparecer un error que indica que una solicitud de red se ha cancelado o interrumpido.

Si los usuarios tienen problemas para iniciar sesión en una aplicación incluso después de habilitarla a través de la otra configuración, intente agregarla a `AppCookieSSOAllowList` para resolver los problemas.

- **Clave**: `AppCookieSSOAllowList`
- **Tipo**: `String`
- **Valor**: lista delimitada por comas de los prefijos de identificadores de lote de las aplicaciones que pueden participar en el inicio de sesión único. Todas las aplicaciones que comienzan con los prefijos de la lista podrán participar en el SSO.
- **Ejemplo**: `com.contoso.myapp1, com.fabrikam.myapp2`

**Otros requisitos**: para habilitar el inicio de sesión único para aplicaciones mediante `AppCookieSSOAllowList`, también debe agregar sus prefijos de identificador de agrupación, `AppPrefixAllowList`.

Pruebe esta configuración solo con aplicaciones que tengan errores de inicio de sesión inesperados. 

#### <a name="summary-of-keys"></a>Resumen de claves

| Clave | Tipo | Valor |
|--|--|--|
| `Enable_SSO_On_All_ManagedApps` | Entero | `1` para habilitar el inicio de sesión único para todas las aplicaciones administradas, `0` para deshabilitar el inicio de sesión único para todas las aplicaciones administradas. |
| `AppAllowList` | String<br/>*(lista delimitada por comas)* | Identificadores de agrupación de las aplicaciones que pueden participar en el inicio de sesión único. |
| `AppBlockList` | String<br/>*(lista delimitada por comas)* | Identificadores de agrupación de las aplicaciones que no pueden participar en el inicio de sesión único. |
| `AppPrefixAllowList` | String<br/>*(lista delimitada por comas)* | Prefijos de identificadores de agrupación de las aplicaciones que pueden participar en el inicio de sesión único. |
| `AppCookieSSOAllowList` | String<br/>*(lista delimitada por comas)* | Prefijos de identificadores de agrupación de aplicaciones que pueden participar en el inicio de sesión único, pero que usan una configuración de red especial y tienen problemas con el inicio de sesión único con la otra configuración. Las aplicaciones que agregue a `AppCookieSSOAllowList` también se deben agregar a `AppPrefixAllowList`. |

#### <a name="settings-for-common-scenarios"></a>Configuraciones para escenarios comunes

- *Escenario*: Quiero habilitar el inicio de sesión único para la mayoría de las aplicaciones administradas, pero no para todas ellas.

    | Clave | Valor |
    | -------- | ----------------- |
    | `Enable_SSO_On_All_ManagedApps` | `1` |
    | `AppBlockList` | Identificadores de agrupación (lista delimitada por comas) de las aplicaciones que quiere evitar que participen en el inicio de sesión único. |

- *Escenario*: Quiero deshabilitar el inicio de sesión único para Safari, que está habilitado de manera predeterminada, pero habilitar el inicio de sesión único para todas las aplicaciones administradas.

    | Clave | Valor |
    | -------- | ----------------- |
    | `Enable_SSO_On_All_ManagedApps` | `1` |
    | `AppBlockList` | Identificadores de agrupación (lista delimitada por comas) de las aplicaciones de Safari que quiere evitar que participen en el inicio de sesión único.<ul><li>Para iOS: `com.apple.mobilesafari`, `com.apple.SafariViewService`</li><li>Para macOS: `com.apple.Safari`</li></ul> |

- *Escenario*: Quiero habilitar el inicio de sesión único en todas las aplicaciones administradas y en algunas aplicaciones no administradas, pero deshabilitar el inicio de sesión único para algunas otras aplicaciones.

    | Clave | Valor |
    | -------- | ----------------- |
    | `Enable_SSO_On_All_ManagedApps` | `1` |
    | `AppAllowList` | Identificadores de agrupación (lista delimitada por comas) de las aplicaciones en las que quiere habilitar la participación en el inicio de sesión único. |
    | `AppBlockList` | Identificadores de agrupación (lista delimitada por comas) de las aplicaciones que quiere evitar que participen en el inicio de sesión único. |


##### <a name="find-app-bundle-identifiers-on-ios-devices"></a>Detección de identificadores de agrupación de aplicaciones en dispositivos iOS

Apple no ofrece una manera fácil de obtener los identificadores de agrupación desde App Store. La manera más sencilla de obtener estos identificadores de las aplicaciones que desee usar para SSO es preguntar al proveedor o al desarrollador de la aplicación. Si esa opción no está disponible, puede usar la configuración de MDM para obtener los identificadores de agrupación: 

1. Habilite temporalmente la siguiente marca en la configuración de MDM:

    - **Clave**: `admin_debug_mode_enabled`
    - **Tipo**: `Integer`
    - **Valor**: 1 o 0
1. Cuando esta marca esté activada, inicie sesión en las aplicaciones iOS del el dispositivo de las que desea conocer el identificador de agrupación. 
1. En la aplicación Authenticator, seleccione **Ayuda**  >  **Enviar registros**  >  **Ver registros**. 
1. En el archivo de registro, busque la siguiente línea: `[ADMIN MODE] SSO extension has captured following app bundle identifiers`. Esta línea debería capturar todos los identificadores de agrupación de aplicaciones visibles para la extensión de SSO. 

Use los identificadores de agrupación para configurar el inicio de sesión único para las aplicaciones. 

#### <a name="allow-users-to-sign-in-from-unknown-applications-and-the-safari-browser"></a>Permitir que el usuario inicie sesión desde aplicaciones desconocidas y el explorador Safari

De manera predeterminada, el complemento Microsoft Enterprise SSO proporciona SSO para aplicaciones autorizadas solo cuando un usuario ha iniciado sesión desde una aplicación que usa una biblioteca de la Plataforma de identidad de Microsoft como MSAL o la Biblioteca de autenticación de Azure Active Directory (ADAL). El complemento Microsoft Enterprise SSO también puede adquirir una credencial compartida cuando lo llama otra aplicación que usa una biblioteca de la Plataforma de identidad de Microsoft durante una nueva adquisición de token.

La habilitación de la marca `browser_sso_interaction_enabled` permite que las aplicaciones que no usan una biblioteca de la Plataforma de identidad de Microsoft realicen el arranque inicial y obtengan una credencial compartida. El explorador Safari también puede realizar el arranque inicial y obtener una credencial compartida. 

Si el complemento Microsoft Enterprise SSO aún no tiene una credencial compartida, intentará obtener una cada vez que se solicite un inicio de sesión desde una dirección URL de Azure AD en el explorador Safari, ASWebAuthenticationSession, SafariViewController u otra aplicación nativa permitida. 

Use estos parámetros para habilitar la marca: 

- **Clave**: `browser_sso_interaction_enabled`
- **Tipo**: `Integer`
- **Valor**: 1 o 0

macOS requiere esta configuración para poder ofrecer una experiencia uniforme en todas las aplicaciones. En iOS y iPadOS, esta configuración no es necesaria, ya que la mayoría de las aplicaciones usan la aplicación Authenticator para el inicio de sesión. De todos modos, se recomienda habilitar esta marca porque si algunas de las aplicaciones no usan la aplicación Authenticator en iOS o iPadOS, con ella mejorará la experiencia. Este valor está deshabilitado de manera predeterminada.

#### <a name="disable-asking-for-mfa-during-initial-bootstrapping"></a>Deshabilitación de la solicitud de MFA durante el arranque inicial

De manera predeterminada, el complemento Microsoft Enterprise SSO solicita al usuario la autenticación multifactor durante el arranque inicial y al obtener una credencial compartida. Esta autenticación se pide aunque no sea necesaria para la aplicación que el usuario ha abierto. Este comportamiento permite que la credencial compartida se use fácilmente en todas las demás aplicaciones sin necesidad de pedirla al usuario más adelante si se precisa la autenticación multifactor. Dado que el usuario recibe en conjunto menos avisos, esta configuración suele ser una buena decisión.

La habilitación de `browser_sso_disable_mfa` desactiva MFA durante el arranque inicial y mientras se obtiene la credencial compartida. En este caso, el usuario solo recibe el aviso cuando una aplicación o un recurso requieren MFA. 

Para habilitar la marca, use estos parámetros:

- **Clave**: `browser_sso_disable_mfa`
- **Tipo**: `Integer`
- **Valor**: 1 o 0

Se recomienda mantener esta marca deshabilitada porque reduce el número de veces que se solicita al usuario que inicie sesión. Si la organización raramente usa la autenticación multifactor, quizá le interese habilitarla. La opción recomendada, sin embargo, es usar MFA con mayor frecuencia. Este es el motivo por el que la marca está deshabilitada de manera predeterminada.

#### <a name="disable-oauth-2-application-prompts"></a>Deshabilitación de los avisos de aplicaciones OAuth 2

Si una aplicación solicita a los usuarios que inicien sesión aunque el complemento Microsoft Enterprise SSO funcione para otras aplicaciones del dispositivo, es posible que la aplicación omita el inicio de sesión único en el nivel de protocolo.  Estas aplicaciones también omiten las credenciales compartidas, porque el complemento proporciona el inicio de sesión único anexando las credenciales a las solicitudes de red realizadas por las aplicaciones permitidas.

Estos parámetros especifican si la extensión SSO debe impedir que las aplicaciones nativas y web omitan el inicio de sesión único en la capa de protocolo y fuercen la aparición de un mensaje de inicio de sesión para el usuario.

Para una experiencia de SSO coherente en todas las aplicaciones del dispositivo, se recomienda habilitar una de estas opciones, que están deshabilitadas de forma predeterminada.
  
Deshabilitar la solicitud de la aplicación y mostrar el selector de cuenta:

- **Clave**: `disable_explicit_app_prompt`
- **Tipo**: `Integer`
- **Valor**: 1 o 0
  
Deshabilitar la solicitud de la aplicación y seleccionar una cuenta de la lista de cuentas de SSO que coincidan automáticamente:
- **Clave**: `disable_explicit_app_prompt_and_autologin`
- **Tipo**: `Integer`
- **Valor**: 1 o 0


#### <a name="use-intune-for-simplified-configuration"></a>Uso de Intune para la configuración simplificada

Puede usar  Intune como servicio MDM para facilitar la configuración del complemento Microsoft Enterprise SSO. Por ejemplo, puede usar Intune para habilitar el complemento y agregar aplicaciones antiguas a una lista de aplicaciones permitidas para que obtengan el SSO. 

Para obtener más información, consulte la [documentación de configuración de Intune](/intune/configuration/ios-device-features-settings).

## <a name="use-the-sso-plug-in-in-your-application"></a>Uso del complemento de inicio de sesión único en la aplicación

La versión 1.1.0 y versiones posteriores de [MSAL para dispositivos Apple](https://github.com/AzureAD/microsoft-authentication-library-for-objc) admiten el complemento Microsoft Enterprise SSO para dispositivos Apple. Es el método recomendado para agregar compatibilidad con el complemento de Microsoft Enterprise SSO. Garantiza que obtiene todas las funcionalidades de la Plataforma de identidad de Microsoft.

Si va a compilar una aplicación para escenarios de trabajadores de primera línea, consulte la información de configuración en [Modo de dispositivo compartido para dispositivos iOS](msal-ios-shared-devices.md).

## <a name="understand-how-the-sso-plug-in-works"></a>Descripción de cómo funciona el complemento de SSO

El complemento Microsoft Enterprise SSO se basa en el [marco Enterprise SSO de Apple](https://developer.apple.com/documentation/authenticationservices/asauthorizationsinglesignonprovider?language=objc). Los proveedores de identidades que se unen al marco pueden interceptar el tráfico de red a sus dominios y mejorar o cambiar el modo en que se administran esas solicitudes. Por ejemplo, el complemento de SSO puede mostrar más opciones en la interfaz de usuario para recopilar credenciales de usuario final de forma segura, exigir MFA o proporcionar tokens a la aplicación de forma silenciosa.

Las aplicaciones nativas también pueden implementar operaciones personalizadas y comunicarse directamente con el complemento de inicio de sesión único. Para obtener más información, consulte este [vídeo de la Conferencia mundial para desarrolladores de Apple de 2019](https://developer.apple.com/videos/play/tech-talks/301/).

### <a name="applications-that-use-msal"></a>Aplicaciones que usan MSAL

La versión 1.1.0 y versiones posteriores de [MSAL para dispositivos Apple](https://github.com/AzureAD/microsoft-authentication-library-for-objc) admiten el complemento Microsoft Enterprise SSO para dispositivos Apple de forma nativa en cuentas profesionales y educativas. 

Si ha seguido [todos los pasos recomendados](./quickstart-v2-ios.md) y ha usado el [formato de URI de redirección](./redirect-uris-ios.md) predeterminado, no necesita realizar ninguna configuración especial. En los dispositivos que tienen el complemento SSO, MSAL lo invoca automáticamente para todas las solicitudes de tokens interactivas y silenciosas. También lo invoca para las operaciones de enumeración y eliminación de cuentas. Dado que MSAL implementa el protocolo del complemento de inicio de sesión único nativo que depende de operaciones personalizadas, esta configuración proporciona la mejor experiencia nativa para el usuario final. 

Si MDM no habilita el complemento de inicio de sesión único, pero la aplicación Microsoft Authenticator existe en el dispositivo, MSAL la usa en su lugar con todas las solicitudes de token interactivas. El complemento de inicio de sesión único comparte el inicio de sesión único con la aplicación Authenticator.

### <a name="applications-that-dont-use-msal"></a>Aplicaciones que no usan MSAL

Las aplicaciones que no usan una biblioteca de la Plataforma de identidad de Microsoft, como MSAL, también pueden obtener el inicio de sesión único si un administrador las agrega a la lista de permitidos. 

No es necesario cambiar el código de esas aplicaciones, siempre y cuando se cumplan las siguientes condiciones:

- La aplicación usa marcos de Apple para ejecutar solicitudes de red. Estos marcos incluyen [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) y [NSURLSession](https://developer.apple.com/documentation/foundation/nsurlsession), por ejemplo. 
- La aplicación utiliza protocolos estándar para comunicarse con Azure AD. Estos protocolos incluyen, por ejemplo, OAuth 2, SAML y WS-Federation.
- La aplicación no recopila el nombre de usuario y la contraseña de texto no cifrado de la interfaz de usuario nativa.

En este caso, el inicio de sesión único se proporciona cuando la aplicación crea una solicitud de red y abre un explorador web para iniciar la sesión del usuario. Cuando a un usuario se le redirige a una dirección URL de inicio de sesión de Azure AD, el complemento de inicio de sesión único la comprueba y busca una credencial de inicio de sesión único para ella. Si encuentra la credencial, el complemento de inicio de sesión único la pasa a Azure AD, que autoriza a la aplicación para que lleve a cabo la solicitud de red sin pedir al usuario que escriba sus credenciales. Además, si Azure AD conoce el dispositivo, el complemento de inicio de sesión único también aprueba el certificado de este para satisfacer la comprobación de acceso condicional basada en dispositivo. 

Para admitir el inicio de sesión único en aplicaciones que no son de MSAL, el complemento de SSO implementa un protocolo similar al del complemento de explorador de Windows descrito en [¿Qué es un token de actualización principal?](../devices/concept-primary-refresh-token.md#browser-sso-using-prt) 

En comparación con las aplicaciones basadas en MSAL, el complemento de SSO actúa de modo más transparente con las aplicaciones que no son de MSAL. Se integra con la experiencia de inicio de sesión del explorador existente que proporcionan las aplicaciones. 

El usuario final ve la experiencia familiar y no tiene que iniciar sesión de nuevo en cada aplicación. Por ejemplo, en lugar de mostrar el selector de cuentas nativo, el complemento de inicio de sesión único agrega sesiones de inicio de sesión único a la experiencia del selector de cuentas basado en web. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información en [Modo de dispositivo compartido para dispositivos iOS](msal-ios-shared-devices.md).
