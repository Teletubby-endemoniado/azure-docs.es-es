---
title: Proporcionar notificaciones opcionales a las aplicaciones de Azure AD
titleSuffix: Microsoft identity platform
description: Procedimientos para agregar notificaciones personalizadas o adicionales a los tokens SAML 2.0 y JSON Web Token (JWT) que emite la Plataforma de identidad de Microsoft.
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 7/19/2021
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, keyam
ms.custom: aaddev
ms.openlocfilehash: 1d910871008bee6ba1a2820a68b3225c3fdfd67d
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131032266"
---
# <a name="provide-optional-claims-to-your-app"></a>Proporcionar notificaciones opcionales a la aplicación

Los desarrolladores de aplicaciones pueden usar notificaciones opcionales en sus aplicaciones de Azure AD para especificar qué notificaciones desean incluir en los tokens que se envían a las aplicaciones.

Estas notificaciones opcionales sirven para:

- Seleccionar las notificaciones adicionales que se incluirán en los tokens para la aplicación.
- Cambiar el comportamiento de determinadas notificaciones que la Plataforma de identidad de Microsoft devuelve en tokens.
- Agregar notificaciones personalizadas para la aplicación y acceder a ellas.

Para obtener las listas de notificaciones estándar, vea la documentación de notificaciones de [token de acceso](access-tokens.md) y de [id_token](id-tokens.md).

Aunque las notificaciones opcionales se admiten en los tokens de formato de las versiones 1.0 y 2.0, así como en los tokens SAML, estos tokens proporcionan la mayoría de sus valores al pasar de la versión 1.0 a la 2.0. Uno de los objetivos de la [Plataforma de identidad de Microsoft](./v2-overview.md) es conseguir tamaños de token menores para garantizar el rendimiento óptimo de los clientes. Como resultado, varias notificaciones que antes se incluían en los tokens de identificación y acceso ya no aparecen en los de la versión 2.0 y deben solicitarse específicamente para cada aplicación.

**Tabla 1: Aplicabilidad**

| Tipo de cuenta               | Tokens de la versión 1.0 | Tokens de la versión 2.0 |
|----------------------------|-------------|-------------|
| Cuenta personal de Microsoft | N/D         | Compatible   |
| Cuenta de Azure AD           | Compatible   | Compatible   |

## <a name="v10-and-v20-optional-claims-set"></a>Conjunto de notificaciones opcionales de las versiones 1.0 y 2.0

El conjunto de notificaciones opcionales disponibles de forma predeterminada para que las usen las aplicaciones se enumeran a continuación. Para agregar notificaciones opcionales personalizadas para la aplicación, consulte las [extensiones de directorio](#configuring-directory-extension-optional-claims) a continuación. Al agregar notificaciones al **token de acceso**, estas se aplicarán a los tokens de acceso solicitados *para* la aplicación (una API web), no a las notificaciones solicitadas *por* la aplicación. Independientemente de cómo el cliente acceda a la API, los datos correctos están presentes en el token de acceso que se usa para autenticarse en la API.

> [!NOTE]
>La mayoría de estas notificaciones puede incluirse en los JWT para los tokens v1.0 y v2.0, pero no en los tokens SAML, excepto donde se indique en la columna Tipo de token. Las cuentas de consumidor admiten un subconjunto de estas notificaciones, marcadas en la columna "Tipo de usuario".  Muchas de las notificaciones aquí incluidas no se aplican a los usuarios consumidores (no tienen inquilino, por lo que `tenant_ctry` no tiene ningún valor).

**Tabla 2: Conjunto de notificaciones opcionales de las versiones 1.0 y 2.0**

| Nombre                       |  Descripción   | Tipo de token | Tipo de usuario | Notas  |
|----------------------------|----------------|------------|-----------|--------|
| `acct`                | Estado de la cuenta de los usuarios de un inquilino | JWT, SAML | | Si el usuario es miembro del inquilino, el valor es `0`. Si es un invitado, el valor es `1`. |
| `auth_time`                | Momento de la última autenticación del usuario. Consulte las especificaciones de Open ID Connect| JWT        |           |  |
| `ctry`                     | País o región del usuario | JWT |  | Azure AD devuelve la notificación opcional `ctry` si existe, y el valor del campo es un código de país o región estándar de dos letras, como FR, JP, SZ, etc. |
| `email`                    | Correo electrónico direccionable de este usuario, si tiene uno.  | JWT, SAML | MSA, Azure AD | Si el usuario es un invitado en el inquilino, este valor se incluye de forma predeterminada.  Para los usuarios administrados (aquellos dentro del inquilino), se debe solicitar a través de esta notificación opcional o, únicamente en la versión 2.0, con el ámbito OpenID.  Para los usuarios administrados, se debe establecer la dirección de correo electrónico en el [portal de administración de Office](https://portal.office.com/adminportal/home#/users).|
| `fwd`                      | Dirección IP.| JWT    |   | Agrega la dirección IPv4 original del cliente solicitante (cuando se encuentra en una red virtual) |
| `groups`| Formato opcional de las notificaciones de grupo |JWT, SAML| |Se usa con la opción de configuración GroupMembershipClaims en el [manifiesto de la aplicación](reference-app-manifest.md), que se debe establecer también. Para más información, vea las [notificaciones de grupo](#configuring-groups-optional-claims) abajo. Para más información sobre las notificaciones de grupo, vea [Cómo configurar notificaciones de grupo](../hybrid/how-to-connect-fed-group-claims.md)
| `idtyp`                    | Tipo de token   | Tokens de acceso de JWT | Especial: únicamente en tokens de acceso de solo aplicación |  El valor es `app` cuando el token es un token de solo aplicación. Esta es la forma más precisa para que una API determine si un token es un token de aplicación o un token de usuario + aplicación.|
| `login_hint`               | Sugerencia de inicio de sesión   | JWT | MSA, Azure AD | Una notificación de sugerencia de inicio de sesión opaca y de confianza.  Esta notificación es el mejor valor que se puede usar para el parámetro `login_hint` de OAuth en todos los flujos para el inicio de sesión único.  También se puede pasar entre aplicaciones para ayudar en el inicio de sesión único en modo silencioso: la aplicación A puede iniciar la sesión de un usuario, leer la notificación `login_hint` y, a continuación, enviar la notificación y el contexto del inquilino actual a la aplicación B en la cadena o fragmento de la cadena de consulta cuando el usuario hace clic en un vínculo que le lleva a la aplicación B. Para evitar problemas de confiabilidad y condiciones de carrera, la notificación `login_hint` *no* incluye el inquilino actual del usuario y tiene como valor predeterminado el inquilino principal del usuario cuando se usa.  Si trabaja en un escenario de invitado, en el que el usuario es de otro inquilino, debe proporcionar un identificador de inquilino en la solicitud de inicio de sesión y pasar lo mismo a las aplicaciones con las que esté asociado. Sin embargo, esta notificación está pensada para su uso con la funcionalidad `login_hint` existente del SDK, como se ha expuesto. |
| `sid`                      | Identificador de sesión que se usa para el cierre de cada sesión de usuario. | JWT        |  Cuentas personal y de Azure AD.   |         |
| `tenant_ctry`              | País o región del inquilino de recursos | JWT | | Igual que `ctry`, salvo que un administrador lo establezca en un nivel de inquilino.  También debe ser un valor estándar de dos letras. |
| `tenant_region_scope`      | Región del inquilino de los recursos | JWT        |           | |
| `upn`                      | UserPrincipalName | JWT, SAML  |           | Un identificador del usuario que se puede usar con el parámetro username_hint.  No es un identificador duradero para el usuario y no debe usarse para identificar de forma exclusiva la información del usuario (por ejemplo, como una clave de base de datos). En su lugar, use el id. de objeto de usuario (`oid`) como clave de base de datos. Los usuarios que inician sesión con un [id. de inicio de sesión alternativo](../authentication/howto-authentication-use-email-signin.md) no deben mostrar su nombre principal de usuario (UPN). En su lugar, use las siguientes notificaciones de token de identificador para mostrar el estado de inicio de sesión al usuario: `preferred_username` o `unique_name` para los tokens v1 y `preferred_username` para los tokens v2. Aunque esta notificación se incluye automáticamente, puede especificarla como opcional para adjuntar propiedades adicionales y así modificarle el comportamiento para los usuarios invitados Debe usar la notificación `login_hint` para el uso de `login_hint`: los identificadores legibles como el UPN no son de confianza.|
| `verified_primary_email`   | Procede del valor PrimaryAuthoritativeEmail del usuario.      | JWT        |           |         |
| `verified_secondary_email` | Procede del valor SecondaryAuthoritativeEmail del usuario.   | JWT        |           |        |
| `vnet`                     | Información del especificador de la red virtual | JWT        |           |      |
| `xms_pdl`             | Ubicación de datos preferida   | JWT | | En los inquilinos multigeográficos, la ubicación de datos preferida es el código de tres letras que muestra la región geográfica en la que se encuentra el usuario. Para más información, vea la [documentación de Azure AD Connect acerca de la ubicación de datos preferida](../hybrid/how-to-connect-sync-feature-preferreddatalocation.md).<br/>Por ejemplo: `APC` para Asia Pacífico. |
| `xms_pl`                   | Idioma preferido del usuario  | JWT ||Idioma preferido del usuario, si se establece. Se origina desde su inquilino principal, en escenarios de acceso de invitado. Con formato LL-CC ("en-us"). |
| `xms_tpl`                  | Idioma preferido del inquilino| JWT | | Idioma preferido del inquilino de recursos, si se establece. Con formato LL ("en"). |
| `ztdid`                    | Identificador de implementación sin interacción | JWT | | La identidad del dispositivo usada en [Windows AutoPilot](/windows/deployment/windows-autopilot/windows-10-autopilot). |

## <a name="v20-specific-optional-claims-set"></a>Conjunto de notificaciones opcionales específicas de la versión 2.0

Estas notificaciones siempre se incluyen en los tokens de la versión 1.0 de Azure AD, pero no en los de la versión 2.0, a menos que se solicite. Estas notificaciones solo son válidas en los JWT (tokens de identificación y de acceso).

**Tabla 3: Notificaciones opcionales exclusivas de la versión 2.0**

| Notificación de JWT     | Nombre                            | Descripción                                | Notas |
|---------------|---------------------------------|-------------|-------|
| `ipaddr`      | Dirección IP                      | Dirección IP del cliente desde el que se inició sesión.   |       |
| `onprem_sid`  | Identificador de seguridad local |                                             |       |
| `pwd_exp`     | Tiempo de expiración de la contraseña        | Fecha y hora a la que expira la contraseña. |       |
| `pwd_url`     | Cambiar dirección URL de contraseña             | Dirección URL que el usuario puede visitar para cambiar la contraseña.   |   |
| `in_corp`     | Dentro de red corporativa        | Indica si el cliente ha iniciado sesión desde la red corporativa. En caso contrario, la notificación no se incluye.   |  En función de la configuración de las [IP de confianza](../authentication/howto-mfa-mfasettings.md#trusted-ips) de MFA.    |
| `family_name` | Apellido                       | Proporciona el apellido del usuario según está definido en el objeto de usuario. <br>"family_name": "Miller" | Se admite en MSA y Azure AD. Requiere el ámbito `profile`.   |
| `given_name`  | Nombre                      | Proporciona el nombre de pila o "dado" del usuario, tal como se establece en el objeto de usuario.<br>"given_name": "Frank"                   | Se admite en MSA y Azure AD.  Requiere el ámbito `profile`. |
| `upn`         | Nombre principal del usuario | Un identificador del usuario que se puede usar con el parámetro username_hint.  No es un identificador duradero para el usuario y no debe usarse para identificar de forma exclusiva la información del usuario (por ejemplo, como una clave de base de datos). En su lugar, use el id. de objeto de usuario (`oid`) como clave de base de datos. Los usuarios que inician sesión con un [id. de inicio de sesión alternativo](../authentication/howto-authentication-use-email-signin.md) no deben mostrar su nombre principal de usuario (UPN). En su lugar, use la notificación `preferred_username` para mostrar el estado de inicio de sesión al usuario. | Consulte a continuación las [propiedades adicionales](#additional-properties-of-optional-claims) de la configuración de la notificación. Requiere el ámbito `profile`.|

## <a name="v10-specific-optional-claims-set"></a>Conjunto de notificaciones opcionales específicas de la versión 1.0

Algunas de las mejoras del formato de token de la versión 2 están disponibles para las aplicaciones que usan el formato de token de la 1, ya que ayudan a mejorar la seguridad y la confiabilidad. Estas no surtirán efecto en los tokens de identificador solicitados desde el punto de conexión de la versión 2 ni en los tokens de acceso para las API que usan el formato de token de la versión 2. Estos solo se aplican a los token de JWT, no a los de SAML. 

**Tabla 4: Notificaciones opcionales exclusivas de la versión 1.0**


| Notificación de JWT     | Nombre                            | Descripción | Notas |
|---------------|---------------------------------|-------------|-------|
|`aud`          | Público | Siempre está presente en JWT, pero en los tokens de acceso v1 se puede emitir de varias maneras: cualquier URI de appID, con o sin una barra diagonal final, así como el identificador de cliente del recurso. Puede resultar difícil de codificar con esta aleatoriedad al realizar la validación de tokens.  Use las [propiedades adicionales para esta notificación](#additional-properties-of-optional-claims) a fin de asegurarse de que siempre se establezca en el identificador del cliente del recurso en los tokens de acceso de la versión 1. | Solo tokens de acceso JWT de la versión 1|
|`preferred_username` | Nombre de usuario preferido        | Proporciona la notificación de nombre de usuario preferido en los tokens de la versión 1. Esto facilita que las aplicaciones proporcionen sugerencias de nombre de usuario e indiquen nombres para mostrar legibles, independientemente de su tipo de token.  Se recomienda usar esta notificación opcional en lugar de `upn` o `unique_name`, por ejemplo. | Tokens de identificador y tokens de acceso de la versión 1 |

### <a name="additional-properties-of-optional-claims"></a>Propiedades adicionales de las notificaciones opcionales

Algunas notificaciones opcionales se pueden configurar para cambiar la manera de devolver la notificación. Estas propiedades adicionales se utilizan principalmente para ayudar a la migración de aplicaciones locales con expectativas de datos diferentes. Por ejemplo, `include_externally_authenticated_upn_without_hash` ayuda con los clientes que no pueden admitir almohadillas (`#`) en el UPN.

**Tabla 4: Valores para configurar las notificaciones opcionales**

| Nombre de propiedad  | Nombre de la propiedad adicional | Descripción |
|----------------|--------------------------|-------------|
| `upn`          |                          | Puede usarse en respuestas SAML y JWT y para los token v1.0 y v2.0. |
|                | `include_externally_authenticated_upn`  | Incluye el nombre principal de usuario invitado tal como se almacenó en el inquilino de recursos. Por ejemplo: `foo_hometenant.com#EXT#@resourcetenant.com` |
|                | `include_externally_authenticated_upn_without_hash` | Igual que antes, excepto que las marcas hash (`#`) se reemplazan con guiones bajos (`_`); por ejemplo, `foo_hometenant.com_EXT_@resourcetenant.com`|
| `aud`          |                          | En los tokens de acceso de la versión 1, se usa para cambiar el formato de la notificación `aud`.  Esto no tiene ningún efecto en los tokens de la versión 2 ni en los tokens de identificador de ninguna versión, donde la notificación `aud` es siempre el identificador del cliente. Use esta configuración para asegurarse de que la API pueda realizar la validación de las audiencias más fácilmente. Al igual que todas las notificaciones opcionales que afectan al token de acceso, el recurso de la solicitud debe establecer esta notificación opcional, ya que los recursos poseen el token de acceso.|
|                | `use_guid`               | Emite el identificador de cliente del recurso (API) en formato GUID como notificación `aud` siempre en lugar de que sea dependiente del entorno de ejecución. Por tanto, si un recurso establece esta marca y su identificador de cliente es `bb0a297b-6a42-4a55-ac40-09a501456577`, cualquier aplicación que solicite un token de acceso para ese recurso recibirá un token de acceso con `aud`: `bb0a297b-6a42-4a55-ac40-09a501456577`. </br></br> Sin este conjunto de notificaciones, una API podría obtener tokens con una notificación `aud` de `api://MyApi.com`, `api://MyApi.com/`, `api://myapi.com/AdditionalRegisteredField` o cualquier otro valor establecido como un URI de identificador de aplicación para esa API, así como el identificador de cliente del recurso. |

#### <a name="additional-properties-example"></a>Ejemplo de propiedades adicionales

```json
"optionalClaims": {
    "idToken": [
        {
            "name": "upn",
            "essential": false,
            "additionalProperties": [
                "include_externally_authenticated_upn"
            ]
        }
    ]
}
```

Este objeto OptionalClaims hace que el token de identificador devuelto al cliente incluya una notificación `upn` con el inquilino de inicio adicional y la información de los recursos del inquilino. La notificación `upn` solo cambia en el token si el usuario es un invitado en el inquilino (que usa un IDP diferente para la autenticación).

## <a name="configuring-optional-claims"></a>Configuración de notificaciones opcionales

> [!IMPORTANT]
> Los tokens de acceso **siempre** se generan mediante el manifiesto del recurso, no del cliente,  con lo cual en la solicitud `...scope=https://graph.microsoft.com/user.read...`, el recurso es Microsoft Graph API.  Por lo tanto, el token de acceso se crea usando el manifiesto de Microsoft Graph API, no el manifiesto del cliente.  Cambiar el manifiesto de la aplicación nunca hará que los tokens de Microsoft Graph API tengan un aspecto diferente.  Para confirmar que los cambios realizados en `accessToken` han surtido efecto, solicite un token para la aplicación, no otra aplicación.

Puede configurar notificaciones opcionales para la aplicación mediante la interfaz de usuario o el manifiesto.

1. Vaya a <a href="https://portal.azure.com/" target="_blank">Azure Portal</a>. 
1. Busque y seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Registros de aplicaciones**.
1. Seleccione en la lista la aplicación para la que desea configurar notificaciones opcionales.

**Configuración de notificaciones opcionales mediante la interfaz de usuario:**

[![Configuración de notificaciones opcionales en la interfaz de usuario](./media/active-directory-optional-claims/token-configuration.png)](./media/active-directory-optional-claims/token-configuration.png)

1. En **Administrar**, seleccione **Configuración del token**.
   - La hoja **Configuración del token** de la opción de la interfaz de usuario no está disponible para las aplicaciones registradas en un inquilino de Azure AD B2C que se puede configurar mediante la modificación del manifiesto de aplicación. Para más información, consulte [Adición de notificaciones y personalización de la entrada del usuario mediante directivas personalizadas en Azure Active Directory B2C](../../active-directory-b2c/configure-user-input.md).  

1. Seleccione **Agregar notificación opcional**.
1. Seleccione el tipo de token que desea configurar.
1. Seleccione las notificaciones opcionales que va a agregar.
1. Seleccione **Agregar**.


**Configuración de notificaciones opcionales mediante el manifiesto de aplicación:**

[![Muestra cómo configurar notificaciones opcionales mediante el manifiesto de aplicación.](./media/active-directory-optional-claims/app-manifest.png)](./media/active-directory-optional-claims/app-manifest.png)

1. En **Administrar**, seleccione **Manifiesto**. Se abrirá un editor de manifiestos basado en web que le permitirá editar el manifiesto. Si lo desea, puede seleccionar **Descargar**, editar el manifiesto de forma local y, a continuación, usar **Cargar** para volver a aplicarlo a la aplicación. Para más información sobre el manifiesto de aplicación, consulte el [artículo Descripción del manifiesto de aplicación de Azure AD](reference-app-manifest.md).

    La entrada siguiente del manifiesto de aplicación agrega las notificaciones opcionales `auth_time`, `ipaddr` y `upn` a los tokens de identificador, acceso y SAML.

    ```json
    "optionalClaims": {
        "idToken": [
            {
                "name": "auth_time",
                "essential": false
            }
        ],
        "accessToken": [
            {
                "name": "ipaddr",
                "essential": false
            }
        ],
        "saml2Token": [
            {
                "name": "upn",
                "essential": false
            },
            {
                "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                "source": "user",
                "essential": false
            }
        ]
    }
    ```

2. Cuando termine, seleccione **Guardar**. Ahora, las notificaciones opcionales especificadas se incluirán en los tokens de la aplicación.


### <a name="optionalclaims-type"></a>Tipo OptionalClaims

Indica las notificaciones opcionales solicitadas por una aplicación. Una aplicación puede configurar que las notificaciones opcionales se devuelvan en los tres tipos de tokens (de identificación, de acceso y SAML 2) que reciba desde el servicio de token de seguridad. La aplicación puede configurar un conjunto diferente de notificaciones opcionales que se devuelva en cada tipo de token. La propiedad OptionalClaims de la entidad de la aplicación es un objeto OptionalClaims.

**Tabla 5: Propiedades del tipo OptionalClaims**

| Nombre          | Tipo                       | Descripción                                           |
|---------------|----------------------------|-------------------------------------------------------|
| `idToken`     | Colección (OptionalClaim) | Notificaciones opcionales que se devuelven en el token de identificación de JWT.     |
| `accessToken` | Colección (OptionalClaim) | Notificaciones opcionales que se devuelven en el token de acceso de JWT. |
| `saml2Token`  | Colección (OptionalClaim) | Notificaciones opcionales que se devuelven en el token SAML de JWT.       |

### <a name="optionalclaim-type"></a>Tipo OptionalClaim

Contiene una notificación opcional asociada a una aplicación o una entidad de servicio. Las propiedades idToken, accessToken y saml2Token del tipo [OptionalClaims](/graph/api/resources/optionalclaims) son una colección de OptionalClaim.
Si lo admite una notificación concreta, también puede modificar el comportamiento de OptionalClaim con el campo AdditionalProperties.

**Tabla 6: Propiedades del tipo OptionalClaim**

| Nombre                   | Tipo                    | Descripción                                                                                                                                                                                                                                                                                                   |
|------------------------|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | Edm.String              | Nombre de la notificación opcional.                                                                                                                                                                                                                                                                               |
| `source`               | Edm.String              | Origen (objeto de directorio) de la notificación. Hay unas notificaciones predefinidas y otras definidas por el usuario desde las propiedades de extensión. Si el valor de origen es null, es una notificación opcional predefinida. Si el valor de origen es user, el valor de la propiedad name es la propiedad de extensión del objeto de usuario. |
| `essential`            | Edm.Boolean             | Si el valor es true, la notificación especificada por el cliente es necesaria garantizar una experiencia de autorización sin problemas para la tarea concreta solicitada por el usuario final. El valor predeterminado es false.                                                                                                                 |
| `additionalProperties` | Colección (Edm.String) | Propiedades adicionales de la notificación. Si existe una propiedad en esta colección, modifica el comportamiento de la notificación opcional especificada en la propiedad name.                                                                                                                                                   |

## <a name="configuring-directory-extension-optional-claims"></a>Configuración de notificaciones opcionales de extensión de directorio

Además del conjunto de notificaciones opcionales estándar, también se pueden configurar tokens para incluir las extensiones. Para obtener más información, consulte [la documentación de extensionProperty de Microsoft Graph](/graph/api/resources/extensionproperty).

Las notificaciones opcionales no admiten las extensiones abiertas y de esquema; solo las extensiones de directorio de estilo de AAD-Graph. Esta característica es útil para adjuntar información de usuario adicional que puede usar la aplicación, por ejemplo, un identificador adicional o la opción de configuración importante que el usuario haya establecido. Vea la parte inferior de esta página para obtener un ejemplo.

Las extensiones de esquema de directorio son una característica solo de Azure AD. Si el manifiesto de aplicación solicita una extensión personalizada y un usuario de MSA inicia sesión en la aplicación, estas extensiones no se devolverán.

### <a name="directory-extension-formatting"></a>Formato de extensión de directorio

Al configurar las notificaciones opcionales de la extensión de directorio mediante el manifiesto de aplicación, use el nombre completo de la extensión (en el formato: `extension_<appid>_<attributename>`). `<appid>` debe coincidir con el identificador de la aplicación que solicita la notificación.

En el JWT, estas notificaciones se emitirán con el siguiente formato de nombre: `extn.<attributename>`.

En los tokens SAML, estas notificaciones se emitirán con el siguiente formato de identificador uniforme de recursos: `http://schemas.microsoft.com/identity/claims/extn.<attributename>`.

## <a name="configuring-groups-optional-claims"></a>Configuración de notificaciones opcionales de grupo

En esta sección se describen las opciones de configuración de notificaciones opcionales para cambiar los atributos de grupo usados en las notificaciones de grupo, desde el atributo objectID de grupo predeterminado a los atributos que se sincronizan desde una instalación local de Active Directory de Windows. Puede configurar notificaciones opcionales de grupo para la aplicación mediante la interfaz de usuario o el manifiesto.

> [!IMPORTANT]
> Consulte [Configurar notificaciones de grupo de aplicaciones con Azure AD](../hybrid/how-to-connect-fed-group-claims.md) para obtener más detalles, incluidas algunas salvedades importantes sobre las notificaciones de grupo de los atributos locales.

**Configuración de notificaciones opcionales de grupo mediante la interfaz de usuario:**

1. Inicie sesión en <a href="https://portal.azure.com/" target="_blank">Azure Portal</a>.
1. Después de haberse autenticado, elija el inquilino de Azure AD; para ello, selecciónelo en la esquina superior derecha de la página.
1. Busque y seleccione **Azure Active Directory**.
1. En **Administrar**, seleccione **Registros de aplicaciones**.
1. Seleccione en la lista la aplicación para la que desea configurar notificaciones opcionales.
1. En **Administrar**, seleccione **Configuración del token**.
1. Seleccione **Agregar notificación de grupos**.
1. Seleccione los tipos de grupo que se van a devolver (**Grupos de seguridad**, **Roles de directorio**, **Todos los grupos** o **Grupos asignados a la aplicación**). La opción **Grupos asignados a la aplicación** solo incluye esos grupos. La opción **Todos los grupos** incluye **SecurityGroup**, **DirectoryRole** y **DistributionList**, pero no **Grupos asignados a la aplicación**. 
1. Opcional: seleccione las propiedades de tipo de token específico para modificar el valor de notificaciones de grupos para que contenga los atributos de grupo locales o para cambiar el tipo de notificaciones a un rol.
1. Seleccione **Guardar**.

**Configuración de notificaciones opcionales de grupo mediante el manifiesto de aplicación:**

1. Inicie sesión en <a href="https://portal.azure.com/" target="_blank">Azure Portal</a>.
1. Después de haberse autenticado, elija el inquilino de Azure AD; para ello, selecciónelo en la esquina superior derecha de la página.
1. Busque y seleccione **Azure Active Directory**.
1. Seleccione en la lista la aplicación para la que desea configurar notificaciones opcionales.
1. En **Administrar**, seleccione **Manifiesto**.
1. Agregue la siguiente entrada mediante el editor de manifiestos:

   Los valores válidos son:

   - "Todos" (esta opción incluye SecurityGroup, DirectoryRole y DistributionList)
   - "SecurityGroup"
   - "DirectoryRole"
   - "ApplicationGroup" (esta opción solo incluye los grupos asignados a la aplicación)

   Por ejemplo:

    ```json
    "groupMembershipClaims": "SecurityGroup"
    ```

   De forma predeterminada, los valores de ObjectID de grupo se emitirán en el valor de la notificación de grupo.  Para modificar el valor de la notificación para que contenga los atributos del grupo local, o para cambiar el tipo de notificación a rol, use la configuración de notificaciones opcionales de la siguiente manera:

1. Establezca las notificaciones opcionales para la configuración de nombre de grupo.

   Si quiere que los grupos del token contengan los atributos de grupo locales de AD en la sección de notificaciones opcionales, especifique a qué tipo de token se debe aplicar la notificación, el nombre de la notificación opcional solicitada y las propiedades adicionales que quiera usar.  Pueden aparecer varios tipos de token:

   - idToken para el token de OIDC ID
   - accessToken para el token de acceso OAuth
   - Saml2Token para los token de SAML.

   El tipo Saml2Token se aplica a los token con formato SAML 1.1 y SAML 2.0.

   Para cada tipo de token relevante, modifique las notificaciones de grupos para usar la sección OptionalClaims en el manifiesto. El esquema de OptionalClaims es el siguiente:

    ```json
    {
        "name": "groups",
        "source": null,
        "essential": false,
        "additionalProperties": []
    }
    ```

   | Esquema de notificaciones opcionales | Value |
   |----------|-------------|
   | **name:** | Debe ser "grupos" |
   | **source:** | No se usa. Omitir o especificar null |
   | **essential:** | No se usa. Omitir o especificar falso |
   | **additionalProperties:** | Lista de propiedades adicionales.  Las opciones válidas son "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name", "emit_as_roles". |

   En additionalProperties solo se necesita un valor de "sam_account_name", "dns_domain_and_sam_account_name", "netbios_domain_and_sam_account_name".  Si hay más de uno, se usa el primero y omiten los demás.

   Algunas aplicaciones requieren información de grupo sobre el usuario en la notificación de rol.  Para cambiar el tipo de notificación de una notificación de grupo a una notificación de rol, agregue "emit_as_roles" a las propiedades adicionales.  Los valores de grupo se emitirán en la notificación de rol.

   Si se usa "emit_as_roles", cualquier rol de aplicación configurado que se asigne al usuario no aparecerá en la notificación de rol.

**Ejemplos:**

1) Emita grupos como nombres de grupo en tokens de acceso OAuth con el formato dnsDomainName\sAMAccountName.

    **Configuración en la interfaz de usuario:**

    [![Configuración de notificaciones opcionales](./media/active-directory-optional-claims/groups-example-1.png)](./media/active-directory-optional-claims/groups-example-1.png)

    **Entrada en el manifiesto de aplicación:**

    ```json
    "optionalClaims": {
        "accessToken": [
            {
                "name": "groups",
                "additionalProperties": [
                    "dns_domain_and_sam_account_name"
                ]
            }
        ]
    }
    ```

2) Emita los nombres de grupo que se devolverán en el formato NetBIOSDomain\sAMAccountName como la notificación de roles de los token de SAML y OIDC ID.

    **Configuración en la interfaz de usuario:**

    '[![Notificaciones opcionales en el manifiesto](./media/active-directory-optional-claims/groups-example-2.png)](./media/active-directory-optional-claims/groups-example-2.png)

    **Entrada en el manifiesto de aplicación:**

    ```json
    "optionalClaims": {
        "saml2Token": [
            {
                "name": "groups",
                "additionalProperties": [
                    "netbios_name_and_sam_account_name",
                    "emit_as_roles"
                ]
            }
        ],
        "idToken": [
            {
                "name": "groups",
                "additionalProperties": [
                    "netbios_name_and_sam_account_name",
                    "emit_as_roles"
                ]
            }
        ]
    }
    ```

## <a name="optional-claims-example"></a>Ejemplo de notificaciones opcionales

En esta sección, conocerá un escenario para ver cómo puede usar la característica de notificaciones opcionales para su aplicación.
Hay varias opciones disponibles para actualizar las propiedades de una configuración de identidad de la aplicación y así habilitar y configurar las notificaciones opcionales:

- Puede usar la interfaz de usuario de **Configuración del token** (consulte el ejemplo siguiente).
- Puede usar el **Manifiesto** (consulte el ejemplo a continuación). Lea primero el documento [Manifiesto de aplicación de Azure Active Directory](./reference-app-manifest.md) para una introducción al manifiesto.
- También es posible escribir una aplicación que use [Microsoft Graph API](/graph/use-the-api) para actualizarla. El tipo [OptionalClaims](/graph/api/resources/optionalclaims) de la guía de referencia de Microsoft Graph API puede ayudarle con la configuración de las notificaciones opcionales.

**Ejemplo**:

En el siguiente ejemplo usará la interfaz de usuario de **Configuración del token** y el **Manifiesto** para agregar notificaciones opcionales a los tokens de acceso, identificador y SAML previstos para la aplicación. Se agregarán distintas notificaciones opcionales para cada tipo de token que la aplicación puede recibir:

- Los tokens de identificación contendrán el nombre principal de usuario de los usuarios federados en la forma completa (`<upn>_<homedomain>#EXT#@<resourcedomain>`).
- Ahora, los tokens de acceso que soliciten otros clientes para esta aplicación incluirán la notificación auth_time.
- Los tokens SAML contendrán ahora la extensión del esquema de directorio skypeId (en este ejemplo, el identificador de esta aplicación es ab603c56068041afb2f6832e2a17e237). Los tokens SAML expondrán el identificador de Skype como `extension_skypeId`.

**Configuración en la interfaz de usuario:**

1. Inicie sesión en <a href="https://portal.azure.com/" target="_blank">Azure Portal</a>.
1. Después de haberse autenticado, elija el inquilino de Azure AD; para ello, selecciónelo en la esquina superior derecha de la página.

1. Busque y seleccione **Azure Active Directory**.

1. En **Administrar**, seleccione **Registros de aplicaciones**.

1. Busque la aplicación para la que quiera configurar notificaciones opcionales en la lista y selecciónela.

1. En **Administrar**, seleccione **Configuración del token**.

1. Seleccione **Agregar notificación opcional**, elija el tipo de token de **Id.** , seleccione **upn** en la lista de notificaciones y, finalmente, **Agregar**.

1. Seleccione **Agregar notificación opcional**, elija el tipo de token de **Acceso**, seleccione **auth_time** en la lista de notificaciones y, finalmente, **Agregar**.

1. En la pantalla de información general de Configuración del token, seleccione el icono de lápiz situado junto a **upn**, seleccione el control de alternancia **Autenticado externamente** y, por último, **Guardar**.

1. Seleccione **Agregar notificación opcional**, elija el tipo de token **SAML**, seleccione **extn.skypeID** en la lista de notificaciones (solo es aplicable si ha creado un objeto de usuario de Azure AD denominado skypeID) y, finalmente, **Agregar**.

    [![Notificaciones opcionales para el token SAML](./media/active-directory-optional-claims/token-config-example.png)](./media/active-directory-optional-claims/token-config-example.png)

**Configuración en el manifiesto:**

1. Inicie sesión en <a href="https://portal.azure.com/" target="_blank">Azure Portal</a>.
1. Después de haberse autenticado, elija el inquilino de Azure AD; para ello, selecciónelo en la esquina superior derecha de la página.
1. Busque y seleccione **Azure Active Directory**.
1. Busque la aplicación para la que quiera configurar notificaciones opcionales en la lista y selecciónela.
1. En **Administrar**, seleccione **Manifiesto** para abrir el editor de manifiestos en línea.
1. Puede editar directamente el manifiesto mediante este editor. El manifiesto sigue el esquema de la [Entidad de la aplicación](./reference-app-manifest.md) y da formato al manifiesto automáticamente al guardarlo. Se agregarán los elementos nuevos a la propiedad `OptionalClaims`.

    ```json
    "optionalClaims": {
        "idToken": [
            {
                "name": "upn",
                "essential": false,
                "additionalProperties": [
                    "include_externally_authenticated_upn"
                ]
            }
        ],
        "accessToken": [
            {
                "name": "auth_time",
                "essential": false
            }
        ],
        "saml2Token": [
            {
                "name": "extension_ab603c56068041afb2f6832e2a17e237_skypeId",
                "source": "user",
                "essential": true
            }
        ]
    }
    ```

1. Cuando haya terminado de actualizar el manifiesto, seleccione **Guardar** para guardarlo.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las notificaciones estándares que proporciona Azure AD.

- [Tokens de identificador](id-tokens.md)
- [Tokens de acceso](access-tokens.md)
