---
title: Definición de un perfil técnico de RESTful en una directiva personalizada
titleSuffix: Azure AD B2C
description: Defina un perfil técnico de RESTful en una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 05/03/2021
ms.author: kengaderdus
ms.subservice: B2C
ms.openlocfilehash: 16360bd1c4abf435333b40d17d0386b9f7ce0aa8
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "131012764"
---
# <a name="define-a-restful-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Definición de un perfil técnico de RESTful en una directiva personalizada en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) le permite integrar su servicio RESTful propio. Azure AD B2C envía datos al servicio RESTful en una colección de notificaciones de entrada y recibe los datos en una colección de notificaciones de salida. Para más información, vea [Integración de intercambios de notificaciones de la API de REST en la directiva personalizada de Azure AD B2C](api-connectors-overview.md).  

## <a name="protocol"></a>Protocolo

El atributo **Name** del elemento **Protocol** tiene que establecerse en `Proprietary`. El atributo **handler** debe contener el nombre completo del ensamblado de controlador de protocolo que usa Azure AD B2C: `Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

En el ejemplo siguiente se muestra un perfil técnico de RESTful:

```xml
<TechnicalProfile Id="REST-UserMembershipValidator">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="input-claims"></a>Notificaciones de entrada

El elemento **InputClaims** contiene una lista de notificaciones para enviar a la API REST. También se puede asignar el nombre de la notificación al nombre definido en la API REST. En el ejemplo siguiente se muestra la asignación entre la directiva y la API REST. La notificación **givenName** se envía a la API REST como **firstName**, mientras que **surname** se envía como **lastName**. La notificación **email** se establece tal cual.

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="email" />
  <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
  <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
</InputClaims>
```

El elemento **InputClaimsTransformations** puede contener una colección de elementos **InputClaimsTransformation** que se usan para modificar las notificaciones de entrada o generar otras nuevas antes del envío a la API REST.

## <a name="send-a-json-payload"></a>Envío de una carga de JSON

El perfil técnico de la API REST permite enviar una carga de JSON compleja a un punto de conexión.

Para enviar una carga de JSON compleja:

1. Compile la carga de JSON con la transformación de notificaciones [GenerateJson](json-transformations.md).
1. En el perfil técnico de la API REST:
    1. Agregue una transformación de notificaciones de entrada con una referencia a la transformación de notificaciones `GenerateJson`.
    1. Establezca la opción de metadatos `SendClaimsIn` en `body`.
    1. Establezca la opción de metadatos `ClaimUsedForRequestPayload` en el nombre de la notificación que contiene la carga de JSON.
    1. En la notificación de entrada, agregue una referencia a la notificación de entrada que contiene la carga de JSON.

En el siguiente ejemplo, `TechnicalProfile` envía un correo electrónico de verificación mediante un servicio de correo electrónico de terceros (en este caso, SendGrid).

```xml
<TechnicalProfile Id="SendGrid">
  <DisplayName>Use SendGrid's email API to send the code the the user</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://api.sendgrid.com/v3/mail/send</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="ClaimUsedForRequestPayload">sendGridReqBody</Item>
    <Item Key="DefaultUserMessageIfRequestFailed">Cannot process your request right now, please try again later.</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_SendGridApiKey" />
  </CryptographicKeys>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="GenerateSendGridRequestBody" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="sendGridReqBody" />
  </InputClaims>
</TechnicalProfile>
```

## <a name="output-claims"></a>Notificaciones de salida

El elemento **OutputClaims** contiene una lista de notificaciones devuelta por la API REST. Es posible que tenga que asignar el nombre de la notificación definida en la directiva al nombre definido en la API REST. También puede incluir notificaciones que la API REST no devuelve, siempre que establezca el atributo `DefaultValue`.

El elemento **OutputClaimsTransformations** puede contener una colección de elementos **OutputClaimsTransformation** que se usan para modificar las notificaciones de salida o para generar nuevas.

En el ejemplo siguiente se muestra la notificación devuelta por la API REST:

- La notificación **MembershipId** que se asigna al nombre de notificación **loyaltyNumber**.

El perfil técnico también devuelve notificaciones, que no son devueltas por el proveedor de identidades:

- La notificación **loyaltyNumberIsNew** que tiene un valor predeterminado establecido en `true`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="MembershipId" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumberIsNew" DefaultValue="true" />
</OutputClaims>
```

## <a name="metadata"></a>Metadatos

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ServiceUrl | Sí | La dirección URL del punto de conexión de la API REST. |
| AuthenticationType | Sí | El tipo de autenticación realizada por el proveedor de notificaciones RESTful. Valores posibles: `None`, `Basic`, `Bearer`, `ClientCertificate` o `ApiKeyHeader`. <br /><ul><li>El valor `None` indica que la API de REST es anónima. </li><li>El valor `Basic` indica que la API REST se protege con autenticación básica HTTP. Solo los usuarios verificados, incluido Azure AD B2C, pueden acceder a la API. </li><li>El valor `ClientCertificate` (recomendado) indica que la API REST restringe el acceso mediante la autenticación de certificado de cliente. Solo pueden acceder a la API los servicios que tengan los certificados adecuados; por ejemplo, Azure AD B2C. </li><li>El valor `Bearer` indica que la API REST restringe el acceso mediante el token de portador de OAuth2 de cliente. </li><li>El valor `ApiKeyHeader` indica que la API REST está protegida con el encabezado HTTP de clave de API, como *x-functions-key*. </li></ul> |
| AllowInsecureAuthInProduction| No| Indica si `AuthenticationType` se puede establecer en `none` en el entorno de producción (el valor `DeploymentMode` de [TrustFrameworkPolicy](trustframeworkpolicy.md) se establece en `Production` o no se especifica). Valores posibles: true o false (predeterminado). |
| SendClaimsIn | No | Especifica cómo se envían las notificaciones de entrada al proveedor de notificaciones RESTful. Valores posibles: `Body` (predeterminado), `Form`, `Header`, `Url` o `QueryString`. <br /> El valor `Body` es la notificación de entrada que se envía en el cuerpo de la solicitud en formato JSON. <br />El valor `Form` es la notificación de entrada que se envía en el cuerpo de la solicitud en un formato de valor de clave separado por "&" (Y comercial). <br />El valor `Header` es la notificación de entrada que se envía en el cuerpo de la solicitud. <br />El valor `Url` es la notificación de entrada que se envía en la URL, por ejemplo, `https://api.example.com/{claim1}/{claim2}?{claim3}={claim4}`. La parte del nombre de host de la URL no puede contener notificaciones.  <br />El valor `QueryString` es la notificación de entrada que se envía en la cadena de consulta de la solicitud. <br />Los verbos HTTP invocados por cada uno de ellos son los siguientes:<br /><ul><li>`Body`: POST</li><li>`Form`: POST</li><li>`Header`: GET</li><li>`Url`: GET</li><li>`QueryString`: GET</li></ul> |
| ClaimsFormat | No | No se usa actualmente, se puede omitir. |
| ClaimUsedForRequestPayload| No | Nombre de una notificación de cadena que contiene la carga que se va a enviar a la API REST. |
| DebugMode | No | Ejecuta el perfil técnico en modo de depuración. Valores posibles: `true` o `false` (valor predeterminado). En el modo de depuración, la API REST puede devolver más información. consulte la sección [Devolución de mensajes de error](#returning-validation-error-message). |
| IncludeClaimResolvingInClaimsHandling  | No | En el caso de las notificaciones de entrada y salida, especifica si se incluye la [resolución de notificaciones](claim-resolver-overview.md) en el perfil técnico. Valores posibles: `true` o `false` (valor predeterminado). Si desea utilizar un solucionador de notificaciones en el perfil técnico, establézcalo en `true`. |
| ResolveJsonPathsInJsonTokens  | No | Indica si el perfil técnico resuelve las rutas de acceso JSON. Valores posibles: `true` o `false` (valor predeterminado). Use estos metadatos para leer datos de un elemento JSON anidado. En un objeto [OutputClaim](technicalprofiles.md#output-claims), establezca `PartnerClaimType` en el elemento de la ruta de acceso JSON que quiere generar. Por ejemplo: `firstName.localized` o `data.0.to.0.email`.|
| UseClaimAsBearerToken| No| Nombre de la notificación que contiene el token de portador.|

## <a name="error-handling"></a>Control de errores

Los metadatos siguientes se pueden usar para configurar los mensajes de error que se muestran cuando se produce un error de API REST. Los mensajes de error se pueden [localizar](localization-string-ids.md#restful-service-error-messages).

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| DefaultUserMessageIfRequestFailed | No | Mensaje de error personalizado predeterminado para todas las excepciones de la API REST.|
| UserMessageIfCircuitOpen | No | Mensaje de error cuando no se puede acceder a la API REST. Si no se especifica, se devolverá el atributo DefaultUserMessageIfRequestFailed. |
| UserMessageIfDnsResolutionFailed | No | Mensaje de error para la excepción de resolución DNS. Si no se especifica, se devolverá el atributo DefaultUserMessageIfRequestFailed. | 
| UserMessageIfRequestTimeout | No | Mensaje de error cuando se agota el tiempo de espera de la conexión. Si no se especifica, se devolverá el atributo DefaultUserMessageIfRequestFailed. | 

## <a name="cryptographic-keys"></a>Claves de cifrado

Si el tipo de autenticación se establece en `None`, no se usa el elemento **CryptographicKeys**.

```xml
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
</TechnicalProfile>
```

Si el tipo de autenticación se establece en `Basic`, el elemento **CryptographicKeys** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| BasicAuthenticationUsername | Sí | Nombre de usuario que se usa para la autenticación. |
| BasicAuthenticationPassword | Sí | Contraseña que se usa para la autenticación. |

En el ejemplo siguiente se muestra un perfil técnico con autenticación básica:

```xml
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Basic</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
  </CryptographicKeys>
</TechnicalProfile>
```

Si el tipo de autenticación se establece en `ClientCertificate`, el elemento **CryptographicKeys** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ClientCertificate | Sí | El certificado X509 (conjunto de claves RSA) que se va a usar para la autenticación. |

```xml
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">ClientCertificate</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
  </CryptographicKeys>
</TechnicalProfile>
```

Si el tipo de autenticación se establece en `Bearer`, el elemento **CryptographicKeys** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| BearerAuthenticationToken | No | El token de portador de OAuth 2.0. |

```xml
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_B2cRestClientAccessToken" />
  </CryptographicKeys>
</TechnicalProfile>
```

Si el tipo de autenticación se establece en `ApiKeyHeader`, el elemento **CryptographicKeys** contiene el atributo siguiente:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Nombre del encabezado HTTP, como `x-functions-key` o `x-api-key`. | Sí | Clave que se usa para la autenticación. |

> [!NOTE]
> En estos momentos, Azure AD B2C solo admite un encabezado HTTP para la autenticación. Si la llamada de RESTful requiere varios encabezados, como un identificador de cliente y un secreto de cliente, necesitará redirigir la solicitud mediante proxy de alguna manera.

```xml
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">ApiKeyHeader</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="x-functions-key" StorageReferenceId="B2C_1A_RestApiKey" />
  </CryptographicKeys>
</TechnicalProfile>
```

## <a name="returning-validation-error-message"></a>Devolución del mensaje de error de validación

Es posible la API REST tenga que devolver un mensaje de error, como "No se encuentra el usuario en el sistema CRM". Si se produce un error, la API de REST debe devolver un mensaje de error HTTP 4xx, como los códigos de estado de respuesta 400 (solicitud incorrecta) o 409 (conflicto). El cuerpo de la respuesta contiene un mensaje de error con formato JSON:

```json
{
  "version": "1.0.0",
  "status": 409,
  "code": "API12345",
  "requestId": "50f0bd91-2ff4-4b8f-828f-00f170519ddb",
  "userMessage": "Message for the user",
  "developerMessage": "Verbose description of problem and how to fix it.",
  "moreInfo": "https://restapi/error/API12345/moreinfo"
}
```

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| version | Sí | Su versión de la API de REST. Por ejemplo: 1.0.1 |
| status | Sí | Debe ser 409. |
| código | No | Código de error del proveedor de punto de conexión RESTful, que se muestra cuando `DebugMode` está habilitado. |
| requestId | No | Identificador de solicitud del proveedor de punto de conexión RESTful, que se muestra cuando `DebugMode` está habilitado. |
| userMessage | Sí | Mensaje de error que se muestra al usuario. |
| developerMessage | No | Descripción detallada del problema y cómo corregirlo, que se muestra cuando `DebugMode` está habilitado. |
| moreInfo | No | URI que señala a información adicional, que se muestra cuando `DebugMode` está habilitado. |


En el ejemplo siguiente se muestra una clase de C# que devuelve un mensaje de error:

```csharp
public class ResponseContent
{
  public string version { get; set; }
  public int status { get; set; }
  public string code { get; set; }
  public string userMessage { get; set; }
  public string developerMessage { get; set; }
  public string requestId { get; set; }
  public string moreInfo { get; set; }
}
```

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener ejemplos del uso de un perfil técnico de RESTful:

- [Integración de intercambios de notificaciones de la API de REST en la directiva personalizada de Azure AD B2C](api-connectors-overview.md)
- [Tutorial: Adición de un conector de API a un flujo de usuario de registro](add-api-connector.md)
- [Tutorial: Agregue los intercambios de notificaciones de la API de REST a directivas personalizadas de Azure Active Directory B2C](add-api-connector-token-enrichment.md)
- [Protección de los servicios de la API de REST](secure-rest-api.md)
