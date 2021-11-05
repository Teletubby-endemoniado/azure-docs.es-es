---
title: Referencia de cambios importantes en Azure Active Directory
description: Obtenga información sobre los cambios realizados en los protocolos de Azure AD que pueden afectar a la aplicación.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: reference
ms.date: 10/22/2021
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev, has-adal-ref
ms.openlocfilehash: 04dffd4dcebee3cee9023cf445fda6e3fd05b008
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131050686"
---
# <a name="whats-new-for-authentication"></a>Novedades en la autenticación

> Para recibir notificaciones de las actualizaciones de esta página, pegue esta dirección URL en el lector de fuentes RSS:<br/>`https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us`

El sistema de autenticación altera y agrega características constantemente para aumentar la seguridad y mejorar el cumplimiento de normas. Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los detalles siguientes:

- Características más recientes
- Problemas conocidos
- Cambios en el protocolo
- Funciones obsoletas

> [!TIP]
> Esta página se actualiza regularmente, por lo que se recomienda visitarla a menudo. Salvo que se indique lo contrario, estos cambios solo entrarán en vigor en las aplicaciones recién registradas.

## <a name="upcoming-changes"></a>Próximos cambios

Sin próximos cambios que deban tenerse en cuenta. 

## <a name="october-2021"></a>Octubre de 2021

### <a name="error-50105-has-been-fixed-to-not-return-interaction_required-during-interactive-authentication"></a>Se ha corregido el error 50105 para no devolver `interaction_required` durante la autenticación interactiva

**Fecha de entrada en vigor**: octubre de 2021

**Puntos de conexión afectados**: v2.0 y v1.0

**Protocolo afectado**: todos los flujos de usuario para aplicaciones [que requieren la asignación del usuario](../manage-apps/what-is-access-management.md#requiring-user-assignment-for-an-app)

**Cambio**

Se genera el error 50105 (la designación actual) cuando un usuario no asignado intenta iniciar sesión en una aplicación que un administrador ha marcado como necesidad de asignación de usuario.  Se trata de un patrón de control de acceso común y los usuarios a menudo deben encontrar un administrador para solicitar la asignación para desbloquear el acceso.  El error tenía un error que provocaría bucles infinitos en aplicaciones bien codificadas que controlaban correctamente la respuesta de error `interaction_required`. `interaction_required` indica a una aplicación que realice la autenticación interactiva, pero incluso después de hacerlo, Azure AD seguiría devolviendo una respuesta de error `interaction_required`.  

El escenario de error se ha actualizado, por lo que durante la autenticación no interactiva (donde `prompt=none` se usa para ocultar la experiencia de usuario), se indicará a la aplicación que realice la autenticación interactiva mediante una respuesta de error `interaction_required`. En la autenticación interactiva posterior, Azure AD contendrá ahora el usuario y mostrará un mensaje de error directamente, evitando que se produzca un bucle. 

Como recordatorio, Azure AD no admite aplicaciones que detectan códigos de error individuales, como la comprobación de cadenas para `AADSTS50105`. En su lugar, la [directriz de Azure AD](reference-aadsts-error-codes.md#handling-error-codes-in-your-application) es seguir los estándares y usar las [respuestas de autenticación estandarizadas](https://openid.net/specs/openid-connect-core-1_0.html#AuthError) como `interaction_required` y `login_required`. Se encuentran en el campo `error` estándar de la respuesta: los otros campos son para consumo humano durante la solución de problemas. 

Puede revisar el texto actual del error 50105 y mucho más en el servicio de búsqueda de errores: https://login.microsoftonline.com/error?code=50105. 

### <a name="appid-uri-in-single-tenant-applications-will-require-use-of-default-scheme-or-verified-domains"></a>El URI de AppId en aplicaciones de inquilino único requerirá el uso del esquema predeterminado o los dominios comprobados

**Fecha de entrada en vigor**: octubre de 2021

**Puntos de conexión afectados**: v2.0 y v1.0

**Protocolo afectado**: Todos los flujos

**Cambio**

En el caso de las aplicaciones de un solo inquilino, una solicitud para agregar o actualizar el URI de AppId (identifierUris) validará que ese dominio en el valor de URI forma parte de la lista de dominios comprobados en el inquilino del cliente o que el valor usa el esquema predeterminado (`api://{appId}`) proporcionado por AAD.
Esto podría impedir que las aplicaciones agreguen un URI de AppId si el dominio no está en la lista de dominios comprobados o si el valor no usa el esquema predeterminado.
Para obtener más información sobre los dominios comprobados, consulte la [documentación de dominios personalizados](../../active-directory/fundamentals/add-custom-domain.md).

El cambio no afecta a las aplicaciones existentes que usan dominios no comprobados en su URI de AppID. Valida solo las nuevas aplicaciones o cuando una aplicación existente actualiza los URI de un identificador o agrega uno nuevo a la colección identifierUri. Las nuevas restricciones solo se aplican a los URI agregados a la colección identifierUris de una aplicación después del 15/10/2021. Los URI de AppId que ya estén en la colección identifierUris de una aplicación cuando la restricción entre en vigor en el 15/10/2021 seguirán funcionando incluso si agrega nuevos URI a esa colección.

Si una solicitud produce un error en la comprobación de validación, la API de la aplicación para crear o actualizar devolverá `400 badrequest` al cliente que indica HostNameNotOnVerifiedDomain.

[!INCLUDE [active-directory-identifierUri](../../../includes/active-directory-identifier-uri-patterns.md)]

## <a name="august-2021"></a>Agosto de 2021

### <a name="conditional-access-will-only-trigger-for-explicitly-requested-scopes"></a>El acceso condicional solo se desencadenará para ámbitos solicitados explícitamente

**Fecha de entrada en vigor**: agosto de 2021, con lanzamiento gradual a partir de abril. 

**Puntos de conexión afectados**: v2.0

**Protocolo afectado**: todos los flujos que usan el [consentimiento dinámico](v2-permissions-and-consent.md#requesting-individual-user-consent)

En la actualidad, a las aplicaciones que usan el consentimiento dinámico se les otorgan todos los permisos para los que tienen consentimiento, incluso si no se solicitaron en el parámetro `scope` por nombre.  Esto puede hacer que se fuerce a una aplicación que solicite, por ejemplo, solo `user.read`, pero con consentimiento para `files.read`, a pasar el acceso condicional asignado para el permiso `files.read`. 

Con el fin de reducir el número de mensajes de acceso condicional innecesarios, Azure AD está cambiando la manera en que se proporcionan ámbitos no solicitados a las aplicaciones para que solo aquellos ámbitos solicitados explícitamente desencadenen el acceso condicional. Este cambio puede provocar que las aplicaciones que dependan del comportamiento anterior de Azure AD (es decir, proporcionar todos los permisos incluso aunque no se soliciten) se interrumpan, ya que los tokens que solicitan no tendrán permisos.

Ahora, las aplicaciones recibirán tokens de acceso con una combinación de permisos: los permisos solicitados y aquellos para los que se tiene consentimiento y que no requieren indicaciones de acceso condicional.  Los ámbitos del token de acceso se reflejan en el parámetro `scope` de la respuesta del token. 

Este cambio se realizará en todas las aplicaciones excepto en aquellas con una dependencia observada de este comportamiento.  Se informará a los desarrolladores en caso de estar exentos de este cambio, ya que podrían tener una dependencia de los avisos adicionales de acceso condicional. 

**Ejemplos**

Una aplicación tiene el consentimiento para `user.read`, `files.readwrite` y `tasks.read`. `files.readwrite` tiene aplicadas directivas de acceso condicional, mientras que los otros dos no. Si una aplicación realiza una solicitud de token para `scope=user.read` y el usuario que ha iniciado sesión actualmente no ha pasado ninguna directiva de acceso condicional, el token resultante será para los permisos `user.read` y `tasks.read`. Se incluye `tasks.read` porque la aplicación tiene el consentimiento y no requiere que se aplique una directiva de acceso condicional. 

Si, después, la aplicación solicita `scope=files.readwrite`, el acceso condicional que requiere el inquilino se desencadenará, obligando a la aplicación a mostrar un mensaje de autenticación interactiva en el que se puede satisfacer la directiva de acceso condicional.  El token devuelto tendrá los tres ámbitos. 

Si la aplicación realiza una última solicitud para cualquiera de los tres ámbitos (por ejemplo, `scope=tasks.read`), Azure AD verá que el usuario ya ha completado las directivas de acceso condicional necesarias para `files.readwrite` y, de nuevo, emitirá un token con los tres permisos. 


## <a name="june-2021"></a>Junio de 2021

### <a name="the-device-code-flow-ux-will-now-include-an-app-confirmation-prompt"></a>La experiencia de usuario para el flujo de código de dispositivo ahora incluirá un mensaje de confirmación de la aplicación

**Fecha de entrada en vigor**: junio de 2021

**Puntos de conexión afectados**: v2.0 y v1.0

**Protocolo afectado**: el [flujo de código de dispositivo](v2-oauth2-device-code.md)

Como mejora de seguridad, el flujo de código de dispositivo se actualizó para agregar un mensaje adicional, que valida que el usuario está iniciando sesión en la aplicación que espera. Esto se agrega para ayudar a evitar ataques de suplantación de identidad (phishing).

El mensaje que aparece tiene el siguiente aspecto:

:::image type="content" source="media/breaking-changes/device-code-flow-prompt.png" alt-text="Nuevo mensaje, donde se lee &quot;Intenta iniciar sesión en la CLI de Azure?&quot;.":::

## <a name="may-2020"></a>Mayo de 2020

### <a name="bug-fix-azure-ad-will-no-longer-url-encode-the-state-parameter-twice"></a>Corrección del error: Azure AD ya no codificará la dirección URL del parámetro de estado dos veces.

**Fecha de entrada en vigor**: marzo de 2021

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolo afectado**: todos los flujos que visitan el punto de conexión `/authorize` (flujo implícito y flujo de código de autorización)

Se encontró un error y se corrigió en la respuesta de autorización de Azure AD. Durante el tramo `/authorize` de autenticación, el parámetro `state` de la solicitud se incluye en la respuesta, con el fin de conservar el estado de la aplicación y ayudar a evitar ataques CSRF. En Azure AD, el parámetro `state` de la dirección URL se ha codificado de manera incorrecta antes de insertarlo en la respuesta, donde se ha codificado una vez más.  Como consecuencia, las aplicaciones podrían rechazar incorrectamente la respuesta de Azure AD.

Azure AD ya no hará doble codificación de este parámetro, lo que permite que las aplicaciones analicen correctamente el resultado. Este cambio se realizará para todas las aplicaciones. 

### <a name="azure-government-endpoints-are-changing"></a>Los puntos de conexión de Azure Government están cambiando

**Fecha efectiva**: 5 de mayo (finaliza en junio de 2020) 

**Puntos de conexión afectados**: All

**Protocolo afectado**: Todos los flujos

El 1 de junio de 2018, la autoridad oficial de Azure Active Directory (AAD) para Azure Government cambió de `https://login-us.microsoftonline.com` a `https://login.microsoftonline.us`. Este cambio también se aplica a Microsoft 365 GCC High y DoD, a los que también presta servicio Azure Government AAD. Si es propietario de una aplicación en un inquilino de la Administración Pública de EE. UU., debe actualizar la aplicación para que la sesión de los usuarios se inicie en el punto de conexión `.us`.  

A partir del 5 de mayo, Azure AD comenzará a aplicar el cambio del punto de conexión, lo que impide que los usuarios gubernamentales inicien sesión en aplicaciones hospedadas en inquilinos de la Administración Pública de EE. UU. mediante el punto de conexión público (`microsoftonline.com`).  Las aplicaciones afectadas comenzarán a experimentar un error `AADSTS900439` - `USGClientNotSupportedOnPublicEndpoint`. Este error indica que la aplicación está intentando iniciar la sesión de un usuario de la Administración Pública de EE. UU. en el punto de conexión de nube pública. Si su aplicación se encuentra en un inquilino en la nube pública y tiene previsto prestar servicio a usuarios de la Administración Pública de EE. UU., deberá [actualizar la aplicación para que los admita de forma explícita](./authentication-national-cloud.md). Esto puede requerir la creación de un nuevo registro de aplicaciones en la nube de la Administración Pública de EE. UU. 

La aplicación de este cambio se realizará mediante un lanzamiento gradual basado en la frecuencia con que los usuarios de la nube de la Administración Pública de EE. UU. inician sesión en la aplicación: se aplicará antes a las aplicaciones que inician sesión de usuarios de la Administración Pública de EE. UU. con poca frecuencia, mientras que las aplicaciones que los usuarios de la Administración Pública de EE. UU. usan con frecuencia serán las últimas a las que se aplicará. Esperamos que el cambio se haya completado en todas las aplicaciones en junio de 2020. 

Para obtener más información, consulte la [entrada de blog de Azure Government sobre esta migración](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/). 

## <a name="march-2020"></a>Marzo de 2020

### <a name="user-passwords-will-be-restricted-to-256-characters"></a>Las contraseñas de usuario tendrán un límite de 256 caracteres.

**Fecha efectiva**: 13 de marzo de 2020

**Puntos de conexión afectados**: All

**Protocolo afectado**: Todos los flujos de usuario.

Los usuarios con contraseñas de más de 256 caracteres que inicien sesión directamente en Azure AD (en lugar de en un IDP federado como ADFS) no podrán iniciar sesión a partir del 13 de marzo de 2020 y se les pedirá que restablezcan su contraseña.  Es posible que los administradores reciban solicitudes de ayuda para restablecer la contraseña de los usuarios.

El error en los registros de inicio de sesión será AADSTS 50052: InvalidPasswordExceedsMaxLength

Mensaje: `The password entered exceeds the maximum length of 256. Please reach out to your admin to reset the password.`

Corrección:

El usuario no puede iniciar sesión porque su contraseña supera la longitud máxima permitida. Debe ponerse en contacto con su administrador para restablecer la contraseña. Si SSPR está habilitado en su inquilino, puede restablecer la contraseña siguiendo el vínculo "¿Ha olvidado la contraseña?".



## <a name="february-2020"></a>Febrero de 2020

### <a name="empty-fragments-will-be-appended-to-every-http-redirect-from-the-login-endpoint"></a>Los fragmentos vacíos se anexarán a cada redirección HTTP desde el punto de conexión de inicio de sesión.

**Fecha efectiva**: 8 de febrero de 2020

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolo afectado**: Flujos de OAuth y OIDC que usan response_type=query: incluye el [flujo de código de autorización](v2-oauth2-auth-code-flow.md) en algunos casos y el [flujo implícito](v2-oauth2-implicit-grant-flow.md).

Cuando se envía una respuesta de autenticación desde login.microsoftonline.com a una aplicación a través de la redirección HTTP, el servicio anexará un fragmento vacío a la dirección URL de respuesta.  Esto evita una clase de ataques de redireccionamiento, ya que se asegura de que el explorador borre todo fragmento existente en la solicitud de autenticación.  Ninguna aplicación debe tener una dependencia de este comportamiento.


## <a name="august-2019"></a>Agosto de 2019

### <a name="post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored"></a>La semántica del formulario POST se aplicará más estrictamente y se omitirán los espacios y comillas.

**Fecha efectiva**: 2 de septiembre de 2019

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolo afectado**: Se usa POST en cualquier lugar([credenciales de cliente](./v2-oauth2-client-creds-grant-flow.md), [canje de código de autorización](./v2-oauth2-auth-code-flow.md), [ROPC](./v2-oauth-ropc.md), [OBO](./v2-oauth2-on-behalf-of-flow.md) y [canje de token de actualización](./v2-oauth2-auth-code-flow.md#refresh-the-access-token)).

A partir de la semana 9/2, las solicitudes de autenticación que usan el método POST se validarán con estándares HTTP más estrictos.  Concretamente, los espacios y las comillas dobles (") ya no se quitarán de los valores del formulario de solicitud. No se espera que estos cambios interrumpan ningún cliente existente y se asegurará de que las solicitudes enviadas a Azure AD se controlan de forma confiable cada vez. En el futuro (consulte más arriba), tenemos previsto rechazar además los parámetros duplicados y omitir la marca BOM dentro de las solicitudes.

Ejemplo:

En la actualidad, `?e=    "f"&g=h` se analiza exactamente igual que `?e=f&g=h`, por tanto `e` == `f`.  Con este cambio, ahora se analizaría para que `e` == `    "f"`; esto es improbable que sea un argumento válido y la solicitud no se realizará correctamente.


## <a name="july-2019"></a>Julio de 2019

### <a name="app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant"></a>Los tokens solo de aplicación para aplicaciones de inquilino único solo se emiten si la aplicación cliente existe en el inquilino de recursos.

**Fecha efectiva**: 26 de julio de 2019

**Puntos de conexión afectados**: [v1.0](../azuread-dev/v1-oauth2-client-creds-grant-flow.md) y [v2.0](./v2-oauth2-client-creds-grant-flow.md)

**Protocolo afectado**: [credenciales de cliente (tokens de solo aplicación)](../azuread-dev/v1-oauth2-client-creds-grant-flow.md)

El 26 de julio se aplicó un cambio de seguridad que modifica la manera en la que se emiten los tokens de solo aplicación (a través de la concesión de credenciales de cliente). Anteriormente, las aplicaciones podían obtener tokens para llamar a cualquier otra aplicación, sin tener en cuenta su presencia en el inquilino o los roles con consentimiento para esa aplicación.  Este comportamiento se ha actualizado de modo que, en el caso de los recursos (a veces denominados API web) configurados para ser inquilino único (el valor predeterminado), la aplicación cliente debe existir en el inquilino de recursos.  Tenga en cuenta que aún no se requiere que exista consentimiento entre el cliente y la API, y que las aplicaciones todavía deben realizar sus propias comprobaciones de autorización para asegurarse de que existe una notificación `roles` y de que contiene el valor esperado para la API.

El mensaje de error de este escenario indica actualmente:

`The service principal named <appName> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.`

Para solucionar este problema, use la experiencia de consentimiento del administrador con el fin de crear la entidad de servicio de la aplicación cliente en el inquilino o bien créela manualmente.  Este requisito garantiza que el inquilino ha dado permiso a la aplicación para que opere dentro del inquilino.

#### <a name="example-request"></a>Solicitud de ejemplo

`https://login.microsoftonline.com/contoso.com/oauth2/authorize?resource=https://gateway.contoso.com/api&response_type=token&client_id=14c88eee-b3e2-4bb0-9233-f5e3053b3a28&...` En este ejemplo, el inquilino de recursos (autoridad) es contoso.com, la aplicación de recursos es una aplicación de un solo inquilino denominada `gateway.contoso.com/api` para el inquilino de Contoso y la aplicación cliente es `14c88eee-b3e2-4bb0-9233-f5e3053b3a28`.  Si la aplicación cliente tiene una entidad de servicio en Contoso.com, esta solicitud puede continuar.  Si no la tiene, se producirá el error anterior en la solicitud.

Si la aplicación de puerta de enlace de Contoso fuera una aplicación de varios inquilinos, la solicitud continuaría, independientemente de que la aplicación cliente tuviera o no una entidad de servicio en Contoso.com.

### <a name="redirect-uris-can-now-contain-query-string-parameters"></a>Los identificadores URI de redireccionamiento ahora pueden contener parámetros de cadena de consulta.

**Fecha efectiva**: 22 de julio de 2019

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolo afectado**: Todos los flujos

Según la solicitud [RFC 6749](https://tools.ietf.org/html/rfc6749#section-3.1.2), las aplicaciones de Azure AD ahora pueden registrar y usar identificadores URI de redireccionamiento (respuesta) con parámetros de consulta estáticos (como `https://contoso.com/oauth2?idp=microsoft`) para solicitudes de OAuth 2.0.  Los identificadores URI de redireccionamiento dinámico siguen estando prohibidos, ya que representan un riesgo de seguridad, y no se pueden usar para conservar la información de estado a través de una solicitud de autenticación; para ello, use el parámetro `state`.

El parámetro de consulta estático está sujeto a la coincidencia de cadenas para los identificadores URI de redireccionamiento, al igual que cualquier otra parte del URI de redireccionamiento; si no hay ninguna cadena registrada que coincida con el parámetro redirect_uri descodificado con URI, se rechazará la solicitud.  Si se encuentra el identificador URI en el registro de la aplicación, se usará toda la cadena para redirigir al usuario, incluido el parámetro de consulta estático.

Tenga en cuenta que en este momento (final de julio de 2019), la experiencia de usuario del registro de aplicaciones en Azure Portal sigue bloqueando los parámetros de consulta.  Sin embargo, puede editar el manifiesto de aplicación manualmente para agregar parámetros de consulta y probar este procedimiento en la aplicación.


## <a name="march-2019"></a>Marzo de 2019

### <a name="looping-clients-will-be-interrupted"></a>Se interrumpirán los clientes en bucle

**Fecha efectiva**: 25 de marzo de 2019

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolo afectado**: Todos los flujos

En ocasiones, las aplicaciones cliente pueden comportarse de manera incorrecta, emitiendo cientos de veces la misma solicitud de inicio de sesión durante un breve período de tiempo.  Estas solicitudes pueden completarse satisfactoriamente o no, pero todas contribuyen a una deficiente experiencia para el usuario y a un incremento de las cargas de trabajo para el IDP, lo que aumenta la latencia para todos los usuarios y reduce la disponibilidad del IDP.  Estas aplicaciones operan fuera de los límites de uso normal y deben actualizarse para que se comporten correctamente.

A los clientes que emitan solicitudes duplicadas varias veces se les enviará un error `invalid_grant`: `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request`.

La mayoría de los clientes no tendrán que cambiar su comportamiento para evitar este error.  Solo los clientes incorrectamente configurados (aquellos sin almacenamiento en caché de tokens o los que ya presentan bucles de mensajes) se verán afectados por este error.  Se realiza un seguimiento de los clientes por instancia a nivel local (mediante cookie) sobre los siguientes factores:

* Sugerencia de usuario, si existe

* Ámbitos o recursos solicitados

* Id. de cliente

* URI de redireccionamiento

* Tipo y modo de respuesta

Las aplicaciones que realizan varias solicitudes (más de 15) en un breve período de tiempo (5 minutos) recibirán un error `invalid_grant` que explica que están en bucle.  Los tokens solicitados tienen una vigencia de una duración suficiente (10 minutos como mínimo, 60 minutos de forma predeterminada), por lo que las solicitudes repetidas durante este período de tiempo son innecesarias.

Todas las aplicaciones deben controlar `invalid_grant` mostrando un aviso interactivo, en lugar de solicitar un token de forma silenciosa.  Para evitar este error, los clientes deberían asegurarse de que almacenan en caché correctamente los tokens que reciben.


## <a name="october-2018"></a>Octubre de 2018

### <a name="authorization-codes-can-no-longer-be-reused"></a>No se podrán reutilizar los códigos de autorización

**Fecha efectiva**: 15 de noviembre de 2018

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolo afectado**: [flujo de código](v2-oauth2-auth-code-flow.md)

A partir del 15 de noviembre de 2018, Azure AD dejará de aceptar los códigos de autenticación que se usaban anteriormente para las aplicaciones. Este cambio de seguridad ayuda a poner Azure AD en consonancia con la especificación de OAuth y se aplicará en los puntos de conexión v1 y v2.

Si la aplicación reutiliza códigos de autorización para obtener tokens para varios recursos, es recomendable que use el código para obtener un token de actualización y, a continuación, utilice este para adquirir tokens adicionales para otros recursos. Los códigos de autorización solo se pueden usar una vez, pero los tokens de actualización se pueden usar varias veces en varios recursos. Cualquier nueva aplicación que intente reutilizar un código de autenticación durante el flujo de código de OAuth obtendrá el error invalid_grant.

Para más información acerca de los tokens de actualización, consulte [Actualización de los tokens de acceso](v2-oauth2-auth-code-flow.md#refresh-the-access-token).  Si usa ADAL o MSAL, la biblioteca lo controla automáticamente. Sustituya la segunda instancia de 'AcquireTokenByAuthorizationCodeAsync' por 'AcquireTokenSilentAsync'.

## <a name="may-2018"></a>Mayo de 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>No se puede usar los tokens de identificador para el flujo de OBO

**Fecha**: 1 de mayo de 2018

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolos afectados**: flujo implícito y [flujo con derechos delegados](v2-oauth2-on-behalf-of-flow.md)

A partir del 1 de mayo de 2018, id_tokens no se puede utilizar como instrucción de aserción en un flujo de OBO en las aplicaciones nuevas. En su lugar deben usarse tokens de acceso para proteger las API, incluso entre un cliente y el nivel intermedio de la misma aplicación. Las aplicaciones registradas antes del 1 de mayo de 2018 seguirán funcionando y podrán intercambiar id_tokens por un token de acceso (pero tenga en cuenta que este patrón no se considera un procedimiento recomendado).

Para ofrecer una solución alternativa al problema que pueda provocar este cambio, puede hacer lo siguiente:

1. Cree una API web para la aplicación con uno o más ámbitos. Este punto de entrada explícito le permitirá mayor control y seguridad.
1. En el manifiesto de la aplicación, [Azure Portal](https://portal.azure.com) o el [portal de registro de aplicaciones](https://apps.dev.microsoft.com), asegúrese de que la aplicación tiene permiso para emitir tokens de acceso mediante el flujo implícito, lo que se controla mediante la clave `oauth2AllowImplicitFlow`.
1. Cuando la aplicación cliente solicita un valor de id_token a través de `response_type=id_token`, también solicita un token de acceso (`response_type=token`) para la API web creada anteriormente. Por consiguiente, cuando se usa el punto de conexión v2.0 el parámetro `scope` debe ser similar a `api://GUID/SCOPE`. En el punto de conexión v1.0, el parámetro `resource` debe ser el identificador URI de la aplicación de la API web.
1. Pase este token de acceso al nivel intermedio en lugar de id_token.
