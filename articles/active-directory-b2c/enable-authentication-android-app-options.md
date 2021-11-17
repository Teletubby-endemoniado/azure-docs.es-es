---
title: Habilitación de las opciones de aplicaciones móviles de Android mediante Azure Active Directory B2C
description: En este artículo se tratan diversas maneras de habilitar las opciones de aplicaciones móviles de Android mediante Azure Active Directory B2C.
services: active-directory-b2c
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 11/11/2021
ms.author: kengaderdus
ms.subservice: B2C
ms.custom: b2c-support
ms.openlocfilehash: 8631c5c0852f915596e3b51c76054301eca64c02
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132289209"
---
# <a name="configure-authentication-options-in-an-android-app-by-using-azure-ad-b2c"></a>Configuración de las opciones de autenticación en una aplicación de Android mediante Azure AD B2C 

En este artículo se describe cómo puede habilitar, personalizar y mejorar la experiencia de autenticación de Azure Active Directory B2C (Azure AD B2C) para una aplicación Android. 

Antes de comenzar, familiarícese con los siguientes artículos: 
* [Configuración de la autenticación en una aplicación Android de ejemplo mediante Azure AD B2C](configure-authentication-sample-android-app.md)
* [Habilitación de la autenticación en una aplicación de Android propia mediante Azure AD B2C](enable-authentication-android-app.md)

[!INCLUDE [active-directory-b2c-app-integration-custom-domain](../../includes/active-directory-b2c-app-integration-custom-domain.md)]

Para usar un dominio personalizado y el identificador de inquilino en la dirección URL de autenticación, siga las instrucciones que se indican en [Habilitación de dominios personalizados](custom-domain.md). Busque el objeto de configuración de la Biblioteca de autenticación de Microsoft (MSAL) y, después, actualice las *autoridades* con el nombre de dominio y el id. de inquilino personalizados.


#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)

El código de Kotlin siguiente muestra el objeto de configuración de MSAL antes del cambio:

```kotlin
val parameters = AcquireTokenParameters.Builder()
        .startAuthorizationFromActivity(activity)
        .fromAuthority("https://contoso.b2clogin.com/fabrikamb2c.contoso.com/B2C_1_susi")
        // More settings here
        .build()

b2cApp!!.acquireToken(parameters)
```

El código de Kotlin siguiente muestra el objeto de configuración de MSAL después del cambio:

```kotlin
val parameters = AcquireTokenParameters.Builder()
        .startAuthorizationFromActivity(activity)
        .fromAuthority("https://custom.domain.com/00000000-0000-0000-0000-000000000000/B2C_1_susi")
        // More settings here
        .build()

b2cApp!!.acquireToken(parameters)
```


#### <a name="java"></a>[Java](#tab/java)

El código de Kotlin siguiente muestra el objeto de configuración de MSAL antes del cambio:

```java
AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(getActivity())
    .fromAuthority("https://contoso.b2clogin.com/fabrikamb2c.contoso.com/B2C_1_susi")
    // More settings here
    .build();

b2cApp.acquireToken(parameters);
```
El código de Kotlin siguiente muestra el objeto de configuración de MSAL después del cambio:

```java
AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(getActivity())
    .fromAuthority("https://custom.domain.com/00000000-0000-0000-0000-000000000000/B2C_1_susi")
    // More settings here
    .build();

b2cApp.acquireToken(parameters);
```

--- 

[!INCLUDE [active-directory-b2c-app-integration-login-hint](../../includes/active-directory-b2c-app-integration-login-hint.md)]

1. Si usa una directiva personalizada, agregue la notificación de entrada necesaria, como se describe en [Configuración del inicio de sesión directo](direct-signin.md#prepopulate-the-sign-in-name). 
1. Busque el objeto de configuración de MSAL y, después, agregue el método `withLoginHint()` con la sugerencia de inicio de sesión.

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)


```kotlin
val parameters = AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(activity)
    .withLoginHint("bob@contoso.com") 
    // More settings here
    .build()

b2cApp!!.acquireToken(parameters)
```

#### <a name="java"></a>[Java](#tab/java)

```java
AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(getActivity())
    .withLoginHint("bob@contoso.com")  
    // More settings here
    .build();

b2cApp.acquireToken(parameters);

```

--- 

[!INCLUDE [active-directory-b2c-app-integration-domain-hint](../../includes/active-directory-b2c-app-integration-domain-hint.md)]

1. Compruebe el nombre de dominio del proveedor de identidades externo. Para más información, consulte [Redirección del inicio de sesión a un proveedor social](direct-signin.md#redirect-sign-in-to-a-social-provider). 
1. Cree o use un objeto de lista existente para almacenar parámetros de consulta adicionales.
1. Agregue a la lista el parámetro `domain_hint` con el nombre de dominio correspondiente (por ejemplo, `facebook.com`).
1. Pase la lista de parámetros de consulta adicionales al método `withAuthorizationQueryStringParameters` del objeto de configuración de MSAL.

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)

```kotlin
val extraQueryParameters: MutableList<Pair<String, String>> = ArrayList()
extraQueryParameters.add(Pair("domain_hint", "facebook.com"))

val parameters = AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(activity)
    .withAuthorizationQueryStringParameters(extraQueryParameters) 
    // More settings here
    .build()

b2cApp!!.acquireToken(parameters)
```

#### <a name="java"></a>[Java](#tab/java)

```java
List<Pair<String, String>> extraQueryParameters = new ArrayList<>();
extraQueryParameters.add( new Pair<String, String>("domain_hint", "facebook.com"));

AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(getActivity())
    .withAuthorizationQueryStringParameters(extraQueryParameters) 
    // More settings here
    .build();

b2cApp.acquireToken(parameters);
```

--- 

[!INCLUDE [active-directory-b2c-app-integration-ui-locales](../../includes/active-directory-b2c-app-integration-ui-locales.md)]

1. [Configure la personalización de idioma](language-customization.md).
1. Cree o use un objeto de lista existente para almacenar parámetros de consulta adicionales.
1. Agregue a la lista el parámetro `ui_locales` con el código de idioma correspondiente (por ejemplo, `en-us`).
1. Pase la lista de parámetros de consulta adicionales al método `withAuthorizationQueryStringParameters` del objeto de configuración de MSAL.

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)

```kotlin
val extraQueryParameters: MutableList<Pair<String, String>> = ArrayList()
extraQueryParameters.add(Pair("ui_locales", "en-us"))

val parameters = AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(activity)
    .withAuthorizationQueryStringParameters(extraQueryParameters) 
    // More settings here
    .build()

b2cApp!!.acquireToken(parameters)
```

#### <a name="java"></a>[Java](#tab/java)

```java
List<Pair<String, String>> extraQueryParameters = new ArrayList<>();
extraQueryParameters.add( new Pair<String, String>("ui_locales", "en-us"));

AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(getActivity())
    .withAuthorizationQueryStringParameters(extraQueryParameters) 
    // More settings here
    .build();

b2cApp.acquireToken(parameters);
```

--- 

[!INCLUDE [active-directory-b2c-app-integration-custom-parameters](../../includes/active-directory-b2c-app-integration-custom-parameters.md)]

1. Configure el elemento [ContentDefinitionParameters](customize-ui-with-html.md#configure-dynamic-custom-page-content-uri).
1. Cree o use un objeto de lista existente para almacenar parámetros de consulta adicionales.
1. Agregue el parámetro de cadena de consulta personalizado, como `campaignId`. Establezca el valor del parámetro (por ejemplo, `germany-promotion`).
1. Pase la lista de parámetros de consulta adicionales al método `withAuthorizationQueryStringParameters` del objeto de configuración de MSAL.

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)

```kotlin
val extraQueryParameters: MutableList<Pair<String, String>> = ArrayList()
extraQueryParameters.add(Pair("campaignId", "germany-promotion"))

val parameters = AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(activity)
    .withAuthorizationQueryStringParameters(extraQueryParameters) 
    // More settings here
    .build()

b2cApp!!.acquireToken(parameters)
```

#### <a name="java"></a>[Java](#tab/java)

```java
List<Pair<String, String>> extraQueryParameters = new ArrayList<>();
extraQueryParameters.add( new Pair<String, String>("campaignId", "germany-promotion"));

AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(getActivity())
    .withAuthorizationQueryStringParameters(extraQueryParameters) 
    // More settings here
    .build();

b2cApp.acquireToken(parameters);
```

--- 

[!INCLUDE [active-directory-b2c-app-integration-id-token-hint](../../includes/active-directory-b2c-app-integration-id-token-hint.md)]

1. En la directiva personalizada, defina un [perfil técnico de sugerencias de token de identificador](id-token-hint.md).
1. En el código, genere o adquiera un token de identificador y, después, establezca el token en una variable (por ejemplo, `idToken`). 
1. Cree o use un objeto de lista existente para almacenar parámetros de consulta adicionales.
1. Agregue el parámetro `id_token_hint` con la variable correspondiente que almacena el token de identificador.
1. Pase la lista de parámetros de consulta adicionales al método `withAuthorizationQueryStringParameters` del objeto de configuración de MSAL.

#### <a name="kotlin"></a>[Kotlin](#tab/kotlin)

```kotlin
val extraQueryParameters: MutableList<Pair<String, String>> = ArrayList()
extraQueryParameters.add(Pair("id_token_hint", idToken))

val parameters = AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(activity)
    .withAuthorizationQueryStringParameters(extraQueryParameters) 
    // More settings here
    .build()

b2cApp!!.acquireToken(parameters)
```

#### <a name="java"></a>[Java](#tab/java)

```java
List<Pair<String, String>> extraQueryParameters = new ArrayList<>();
extraQueryParameters.add( new Pair<String, String>("id_token_hint", idToken));

AcquireTokenParameters parameters = new AcquireTokenParameters.Builder()
    .startAuthorizationFromActivity(getActivity())
    .withAuthorizationQueryStringParameters(extraQueryParameters) 
    // More settings here
    .build();

b2cApp.acquireToken(parameters);
```

--- 

## <a name="embedded-web-view-experience"></a>Experiencia de vista web insertada

Se requieren exploradores web para la autenticación no interactiva. De forma predeterminada, la biblioteca MSAL usa la vista web del sistema. Durante el inicio de sesión, la biblioteca MSAL muestra la vista web del sistema Android con la interfaz de usuario de Azure AD B2C.  

Para obtener más información, consulte el artículo [Habilitación de SSO entre aplicaciones en Android mediante MSAL](../active-directory/develop/msal-android-single-sign-on.md#sso-through-system-browser).

En función de sus requisitos, puede usar la vista web insertada. Hay diferencias de comportamiento visual y de inicio de sesión único entre la vista web insertada y la vista web del sistema en MSAL.

![Captura de pantalla que muestra la diferencia entre la experiencia de vista web del sistema y la experiencia de vista web insertada.](./media/enable-authentication-android-app-options/system-web-browser-vs-embedded-view.png)

> [!IMPORTANT]
> Se recomienda usar el valor predeterminado de la plataforma, que normalmente es el explorador del sistema. El explorador del sistema la mejor opción para recordar a los usuarios que han iniciado sesión antes. Algunos proveedores de identidades, como Google, no admiten una experiencia de vista insertada.

Para cambiar este comportamiento, abra el archivo *app/src/main/res/raw/auth_config_b2c.json*. A continuación, agregue el atributo `authorization_user_agent` con el valor `WEBVIEW`. En el ejemplo siguiente se muestra cómo cambiar el tipo de vista web a vista insertada:

```json
{
  "authorization_user_agent": "WEBVIEW" 
}
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre la configuración de Android, consulte [Opciones de configuración de MSAL para Android](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki).
