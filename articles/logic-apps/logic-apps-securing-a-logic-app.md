---
title: Acceso seguro y datos
description: Proteja el acceso a las entradas, las salidas, los desencadenadores basados en solicitudes, el historial de ejecución, las tareas de administración y el acceso a otros recursos en Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: rarayudu, azla
ms.topic: how-to
ms.date: 09/13/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: d3814c0888499a1b31d707560f6acd26c572b95e
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131040668"
---
# <a name="secure-access-and-data-in-azure-logic-apps"></a>Proteger el acceso y los datos en Azure Logic Apps

Azure Logic Apps se basa en [Azure Storage](../storage/index.yml) para almacenar y [cifrar los datos en reposo](../security/fundamentals/encryption-atrest.md) automáticamente. Este cifrado protege los datos y le ayuda a cumplir los compromisos de cumplimiento y seguridad de la organización. De forma predeterminada, Azure Storage usa claves que administra Microsoft para cifrar sus datos. Para más información, consulte [Cifrado de Azure Storage para datos en reposo](../storage/common/storage-service-encryption.md).

Para controlar el acceso y proteger los datos confidenciales en Azure Logic Apps aún más, puede configurar más seguridad en estas áreas:

* [Acceso de las llamadas entrantes a desencadenadores basados en solicitud](#secure-inbound-requests)
* [Acceso a las operaciones de las aplicaciones lógicas](#secure-operations)
* [Acceso a las entradas y salidas del historial de ejecución](#secure-run-history)
* [Acceso a las entradas de parámetros](#secure-action-parameters)
* [Acceso de las llamadas salientes a otros servicios y sistemas](#secure-outbound-requests)
* [Bloquear la creación de conexiones para conectores específicos](#block-connections)
* [Guía de aislamiento para aplicaciones lógicas](#isolation-logic-apps)
* [Base de referencia de seguridad de Azure para Azure Logic Apps](../logic-apps/security-baseline.md)

Para obtener más información sobre la seguridad en Azure, consulte estos temas:

* [Información general del cifrado de Azure](../security/fundamentals/encryption-overview.md)
* [Cifrado en reposo de datos de Azure](../security/fundamentals/encryption-atrest.md)
* [Azure Security Benchmark](../security/benchmarks/overview.md)

<a name="secure-inbound-requests"></a>

## <a name="access-for-inbound-calls-to-request-based-triggers"></a>Acceso de las llamadas entrantes a desencadenadores basados en solicitud

Las llamadas entrantes que una aplicación lógica recibe a través de un desencadenador basado en solicitud, como el desencadenador [Request](../connectors/connectors-native-reqres.md) (Solicitud) o el desencadenador [HTTP Webhook](../connectors/connectors-native-webhook.md) (Webhook de HTTP), admiten el cifrado y se protegen con la [versión 1.2 de Seguridad de la capa de transporte (TLS), como mínimo](https://en.wikipedia.org/wiki/Transport_Layer_Security), que antes se conocía como Capa de sockets seguros (SSL). Logic Apps aplica esta versión al recibir una llamada entrante al desencadenador Request (Solicitud) o una devolución de llamada al desencadenador o acción HTTP Webhook (Webhook de HTTP). Si obtiene errores de protocolo de enlace TLS, asegúrese de usar TLS 1.2. Para más información, consulte [Solución del problema de TLS 1.0](/security/solving-tls1-problem).

Para las llamadas entrantes, use los siguientes conjuntos de cifrado:

* TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
* TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

> [!NOTE]
> Para obtener compatibilidad con las versiones anteriores, Azure Logic Apps admite actualmente algunos conjuntos de cifrado antiguos. Sin embargo, *no use* conjuntos de cifrado antiguos cuando desarrolle nuevas aplicaciones, ya que estos conjuntos *puede que no* se admitan en el futuro. 
>
> Por ejemplo, puede encontrar los siguientes conjuntos de cifrado si inspecciona los mensajes de protocolo de enlace TLS mientras usa el servicio Azure Logic Apps o mediante una herramienta de seguridad en la dirección URL de la aplicación lógica. Recuerde que *no debe usar* estos conjuntos antiguos:
>
>
> * TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
> * TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
> * TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
> * TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
> * TLS_RSA_WITH_AES_256_GCM_SHA384
> * TLS_RSA_WITH_AES_128_GCM_SHA256
> * TLS_RSA_WITH_AES_256_CBC_SHA256
> * TLS_RSA_WITH_AES_128_CBC_SHA256
> * TLS_RSA_WITH_AES_256_CBC_SHA
> * TLS_RSA_WITH_AES_128_CBC_SHA
> * TLS_RSA_WITH_3DES_EDE_CBC_SHA

En la siguiente lista se incluyen más formas de limitar el acceso a los desencadenadores que reciben llamadas entrantes a la aplicación lógica para que solo los clientes autorizados puedan llamar a la aplicación lógica:

* [Generación de firmas de acceso compartido (SAS)](#sas)
* [Habilitación de Azure Active Directory Open Authentication (Azure AD OAuth)](#enable-oauth)
* [Exposición de su aplicación lógica con Azure API Management](#azure-api-management)
* [Direcciones IP entrantes restringidas](#restrict-inbound-ip-addresses)

<a name="sas"></a>

### <a name="generate-shared-access-signatures-sas"></a>Generación de firmas de acceso compartido (SAS)

Cada de punto de conexión de la solicitud de una aplicación lógica tiene una [Firma de acceso compartido (SAS)](/rest/api/storageservices/constructing-a-service-sas) en la dirección URL del punto de conexión con el formato siguiente:

`https://<request-endpoint-URI>sp=<permissions>sv=<SAS-version>sig=<signature>`

Cada dirección URL contiene el parámetro de consulta `sp`, `sv` y `sig`, tal y como se describe en esta tabla:

| Parámetro de consulta | Descripción |
|-----------------|-------------|
| `sp` | Especifica los permisos para los métodos HTTP permitidos que se usarán. |
| `sv` | Especifica la versión de SAS que se usará para generar la firma. |
| `sig` | Especifica la firma que se usará para autenticar el acceso al desencadenador. Esta firma se genera mediante el algoritmo SHA256 con una clave de acceso secreta en todas las rutas de acceso de direcciones URL y propiedades. Esta clave nunca se expone ni se publica, y se mantiene cifrada y almacenada con la aplicación lógica. La aplicación lógica solo autoriza los desencadenadores que contienen una firma válida creada con la clave secreta. |
|||

Las llamadas entrantes a un punto de conexión de solicitud solo pueden usar un esquema de autorización, ya sea SAS u [Azure Active Directory Open Authentication](#enable-oauth). Aunque el uso de un esquema no deshabilita el otro, si se utilizan ambos al mismo tiempo se produce un error porque el servicio no sabe cuál elegir.

Para obtener más información sobre la protección del acceso con la Firma de acceso compartido, consulte estas secciones del tema:

* [Regenerar las claves de acceso](#access-keys)
* [Crear direcciones URL de devolución de llamada de expiración próxima](#expiring-urls)
* [Crear direcciones URL con clave principal o secundaria](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Regenerar las claves de acceso

Para generar una nueva clave de acceso de seguridad en cualquier momento, use la API REST de Azure o Azure Portal. Se invalidan todas las direcciones URL generadas previamente que usan la clave anterior y ya no tienen autorización para desencadenar la aplicación lógica. Las direcciones URL que se recuperan tras la regeneración se firman con la nueva clave de acceso.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica donde está la clave que quiere regenerar.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Claves de acceso**.

1. Seleccione la clave que quiere regenerar y finalice el proceso.

<a name="expiring-urls"></a>

#### <a name="create-expiring-callback-urls"></a>Crear direcciones URL de devolución de llamada de expiración próxima

Si comparte la dirección URL del punto de conexión de un desencadenador basado en solicitudes con otras partes, puede generar direcciones URL de devolución de llamada con claves específicas y fechas de expiración. De esta forma puede acumular claves fácilmente o restringir el acceso para desencadenar la aplicación lógica en función de un determinado intervalo de tiempo. Para especificar una fecha de expiración para una dirección URL, use la [API REST de Logic Apps](/rest/api/logic/workflowtriggers); por ejemplo:

```http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

En el cuerpo, incluya la propiedad `NotAfter` mediante una cadena de fecha de JSON. Esta propiedad devuelve una dirección URL de devolución de llamada que solo es válida hasta la fecha y hora `NotAfter`.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Creación de direcciones URL con clave secreta principal o secundaria

Al generar o enumerar direcciones URL de devolución de llamada para desencadenadores basados en solicitudes, puede especificar qué clave se va a usar para firmar la dirección URL. Para generar una dirección URL firmada por una clave específica, use la [API REST de Logic Apps](/rest/api/logic/workflowtriggers); por ejemplo:

```http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

En el cuerpo, incluya la propiedad `KeyType` como `Primary` o `Secondary`. Esta propiedad devuelve una dirección URL firmada por una clave de seguridad especificada.

<a name="enable-oauth"></a>

### <a name="enable-azure-active-directory-open-authentication-azure-ad-oauth"></a>Habilitación de Azure Active Directory Open Authentication (Azure AD OAuth)

Para las llamadas entrantes a un punto de conexión que se haya creado con un desencadenador basado en una solicitud, puede habilitar [Azure AD OAuth](../active-directory/develop/index.yml) mediante la definición o la incorporación de una directiva de autorización para la aplicación lógica. De este modo, las llamadas entrantes usan [tokens de acceso](../active-directory/develop/access-tokens.md) de OAuth para la autorización.

Cuando la aplicación lógica recibe una solicitud entrante que incluye un token de acceso de OAuth, Azure Logic Apps compara las notificaciones del token con las especificadas en cada directiva de autorización. Si existe una coincidencia entre las notificaciones del token y todas las notificaciones en al menos una directiva, la autorización de la solicitud entrante se realiza correctamente. El token puede tener más notificaciones que el número especificado por la directiva de autorización.

> [!NOTE]
> Para el tipo de recurso **Aplicación lógica (estándar)** en una instancia de Azure Logic Apps de inquilino único, Azure AD OAuth no está disponible actualmente para las llamadas entrantes a desencadenadores basados en solicitudes, como el desencadenador de solicitud y el desencadenador de webhook HTTP.

#### <a name="considerations-before-you-enable-azure-ad-oauth"></a>Consideraciones antes de habilitar Azure AD OAuth

* Una llamada entrante al punto de conexión de la solicitud solo puede usar un esquema de autorización, ya sea Azure AD OAuth o [firma de acceso compartido (SAS)](#sas). Aunque el uso de un esquema no deshabilita el otro, si se utilizan ambos al mismo tiempo se produce un error porque el servicio Logic Apps no sabe cuál elegir.

* Solo se admiten esquemas de autorización de [tipo portador](../active-directory/develop/active-directory-v2-protocols.md#tokens) para los tokens de acceso de Azure AD OAuth, lo que significa que en el encabezado `Authorization` para el token de acceso se debe especificar el tipo `Bearer`.

* La aplicación lógica está limitada a un número máximo de directivas de autorización. Cada directiva de autorización también tiene un número máximo de [notificaciones](../active-directory/develop/developer-glossary.md#claim). Para más información, consulte el artículo de [límites y configuración para Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#authentication-limits).

* Una directiva de autorización debe incluir al menos la notificación de **Emisor**, que tiene un valor que comienza por `https://sts.windows.net/` o `https://login.microsoftonline.com/` (OAuth V2) como identificador de emisor de Azure AD.

  Por ejemplo, supongamos que la aplicación lógica tiene una directiva de autorización que requiere dos tipos de notificaciones: **Emisor** y **Audiencia**. En este ejemplo de [sección de carga](../active-directory/develop/access-tokens.md#payload-claims) para un token de acceso descodificado se incluyen los dos tipos de notificaciones, donde `aud` es el valor **Audiencia** y `iss` el valor **Emisor**:

  ```json
  {
      "aud": "https://management.core.windows.net/",
      "iss": "https://sts.windows.net/<Azure-AD-issuer-ID>/",
      "iat": 1582056988,
      "nbf": 1582056988,
      "exp": 1582060888,
      "_claim_names": {
         "groups": "src1"
      },
      "_claim_sources": {
         "src1": {
            "endpoint": "https://graph.windows.net/7200000-86f1-41af-91ab-2d7cd011db47/users/00000-f433-403e-b3aa-7d8406464625d7/getMemberObjects"
         }
      },
      "acr": "1",
      "aio": "AVQAq/8OAAAA7k1O1C2fRfeG604U9e6EzYcy52wb65Cx2OkaHIqDOkuyyr0IBa/YuaImaydaf/twVaeW/etbzzlKFNI4Q=",
      "amr": [
         "rsa",
         "mfa"
      ],
      "appid": "c44b4083-3bb0-00001-b47d-97400853cbdf3c",
      "appidacr": "2",
      "deviceid": "bfk817a1-3d981-4dddf82-8ade-2bddd2f5f8172ab",
      "family_name": "Sophia Owen",
      "given_name": "Sophia Owen (Fabrikam)",
      "ipaddr": "167.220.2.46",
      "name": "sophiaowen",
      "oid": "3d5053d9-f433-00000e-b3aa-7d84041625d7",
      "onprem_sid": "S-1-5-21-2497521184-1604012920-1887927527-21913475",
      "puid": "1003000000098FE48CE",
      "scp": "user_impersonation",
      "sub": "KGlhIodTx3XCVIWjJarRfJbsLX9JcdYYWDPkufGVij7_7k",
      "tid": "72f988bf-86f1-41af-91ab-2d7cd011db47",
      "unique_name": "SophiaOwen@fabrikam.com",
      "upn": "SophiaOwen@fabrikam.com",
      "uti": "TPJ7nNNMMZkOSx6_uVczUAA",
      "ver": "1.0"
   }
   ```

#### <a name="enable-azure-ad-oauth-for-your-logic-app"></a>Habilitación de Azure AD OAuth para la aplicación lógica

Siga estos pasos tanto para Azure Portal como para la plantilla de Azure Resource Manager:

<a name="define-authorization-policy-portal"></a>

#### <a name="portal"></a>[Portal](#tab/azure-portal)

En [Azure Portal](https://portal.azure.com), agregue una o varias directivas de autorización a la aplicación lógica:

1. En [Azure Portal](https://portal.microsoft.com), busque y abra la aplicación lógica en el Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Autorización**. Una vez que se abra el panel Autorización, seleccione **Agregar directiva**.

   ![Selección de "Autorización" > "Agregar directiva"](./media/logic-apps-securing-a-logic-app/add-azure-active-directory-authorization-policies.png)

1. Proporcione información sobre la directiva de autorización; puede especificar los [tipos de notificaciones](../active-directory/develop/developer-glossary.md#claim) y los valores que espera la aplicación lógica en el token de acceso presentado por cada llamada entrante al desencadenador de solicitud:

   ![Proporcionar información de la directiva de autorización](./media/logic-apps-securing-a-logic-app/set-up-authorization-policy.png)

   | Propiedad | Obligatorio | Descripción |
   |----------|----------|-------------|
   | **Nombre de directiva** | Sí | El nombre que quiere usar para la directiva de autorización. |
   | **Notificaciones** | Sí | Los tipos de notificaciones y los valores que acepta la aplicación lógica de las llamadas entrantes. El valor de la notificación está limitado a un [número máximo de caracteres](logic-apps-limits-and-config.md#authentication-limits). Estos son los tipos de notificaciones disponibles: <p><p>- **Emisor** <br>- **Audiencia** <br>- **Subject** (Asunto) <br>- **Id. de JWT** (identificador de JSON Web Token) <p><p>Como mínimo, la lista **Notificaciones** debe incluir la notificación de **Emisor**, que tiene un valor que comienza por `https://sts.windows.net/` o `https://login.microsoftonline.com/` como identificador de emisor de Azure AD. Para más información sobre estos tipos de notificaciones, consulte [Notificaciones de tokens de seguridad de Azure AD](../active-directory/azuread-dev/v1-authentication-scenarios.md#claims-in-azure-ad-security-tokens). También puede especificar su propio tipo de notificaciones y valor. |
   |||

1. Para agregar otra notificación, seleccione una de estas opciones:

   * Para agregar otro tipo de notificaciones, seleccione **Add standard claim** (Agregar notificación estándar), seleccione el tipo de notificación y especifique su valor.

   * Para agregar su propia notificación, seleccione **Agregar notificación personalizada**. Para obtener más información, consulte [Procedimientos: Proporcionar notificaciones opcionales a la aplicación](../active-directory/develop/active-directory-optional-claims.md). La notificación personalizada se almacena después como parte del id. de JWT; por ejemplo, `"tid": "72f988bf-86f1-41af-91ab-2d7cd011db47"`. 

1. Para agregar otra directiva de autorización, seleccione **Agregar directiva**. Repita los pasos anteriores para configurar la directiva.

1. Cuando finalice, seleccione **Guardar**.

1. Para incluir el encabezado `Authorization` del token de acceso en las salidas del desencadenador basado en la solicitud, consulte [Inclusión del encabezado "Authorization" en las salidas del desencadenador por solicitud](#include-auth-header).

Las propiedades de flujo de trabajo, como las directivas, no aparecen en la vista de código de la aplicación lógica en Azure Portal. Para acceder a las directivas mediante programación, llame a la siguiente API a través de Azure Resource Manager: `https://management.azure.com/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group-name}/providers/Microsoft.Logic/workflows/{your-workflow-name}?api-version=2016-10-01&_=1612212851820`. Asegúrese de reemplazar los valores de marcador de posición para el identificador de suscripción de Azure, el nombre del grupo de recursos y el nombre del flujo de trabajo.

<a name="define-authorization-policy-template"></a>

#### <a name="resource-manager-template"></a>[Plantilla de Resource Manager](#tab/azure-resource-manager)

En la plantilla de ARM, defina una directiva de autorización con estos pasos y la siguiente sintaxis:

1. En la sección `properties` de la [definición de los recursos de la aplicación lógica](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#logic-app-resource-definition), agregue un objeto `accessControl`, si no existe ninguno, que contenga un objeto `triggers`.

   Para más información sobre el objeto `accessControl`, consulte [Restricción de los intervalos IP entrantes en la plantilla de Azure Resource Manager](#restrict-inbound-ip-template) y [Referencia de plantillas de flujos de trabajo de Microsoft.Logic](/azure/templates/microsoft.logic/2019-05-01/workflows).

1. En el objeto `triggers`, agregue un objeto `openAuthenticationPolicies` que contenga el objeto `policies` donde pueda definir una o varias directivas de autorización.

1. Proporciónele un nombre a la directiva de autorización, establezca el tipo de directiva en `AAD`e incluya una matriz de `claims` donde se especifiquen uno o varios tipos de notificaciones.

   Como mínimo, la matriz de `claims` debe incluir el tipo de notificación de emisor en el que se establece la propiedad `name` de la notificación en `iss` y `value`, para que empiece por `https://sts.windows.net/` o `https://login.microsoftonline.com/` como identificador del emisor de Azure AD. Para más información sobre estos tipos de notificaciones, consulte [Notificaciones de tokens de seguridad de Azure AD](../active-directory/azuread-dev/v1-authentication-scenarios.md#claims-in-azure-ad-security-tokens). También puede especificar su propio tipo de notificaciones y valor.

1. Para incluir el encabezado `Authorization` del token de acceso en las salidas del desencadenador basado en la solicitud, consulte [Inclusión del encabezado "Authorization" en las salidas del desencadenador por solicitud](#include-auth-header).

Esta es la sintaxis que debe seguir:

```json
"resources": [
   {
      // Start logic app resource definition
      "properties": {
         "state": "<Enabled-or-Disabled>",
         "definition": {<workflow-definition>},
         "parameters": {<workflow-definition-parameter-values>},
         "accessControl": {
            "triggers": {
               "openAuthenticationPolicies": {
                  "policies": {
                     "<policy-name>": {
                        "type": "AAD",
                        "claims": [
                           {
                              "name": "<claim-name>",
                              "value": "<claim-value>"
                           }
                        ]
                     }
                  }
               }
            },
         },
      },
      "name": "[parameters('LogicAppName')]",
      "type": "Microsoft.Logic/workflows",
      "location": "[parameters('LogicAppLocation')]",
      "apiVersion": "2016-06-01",
      "dependsOn": [
      ]
   }
   // End logic app resource definition
],
```

---

<a name="include-auth-header"></a>

#### <a name="include-authorization-header-in-request-trigger-outputs"></a>Inclusión del encabezado "Authorization" en las salidas del desencadenador por solicitud

En el caso de las aplicaciones lógicas que [permiten Azure Active Directory Open Authentication (Azure AD OAuth)](#enable-oauth) para autorizar el acceso de las llamadas entrantes a desencadenadores por solicitud, puede habilitar que las salidas del desencadenador Request o HTTP Webhook incluyan el encabezado `Authorization` del token de acceso de OAuth. En la definición de JSON subyacente del desencadenador, agregue y establezca la propiedad `operationOptions` en `IncludeAuthorizationHeadersInOutputs`. Este es un ejemplo del desencadenador Request (Solicitud):

```json
"triggers": {
   "manual": {
      "inputs": {
         "schema": {}
      },
      "kind": "Http",
      "type": "Request",
      "operationOptions": "IncludeAuthorizationHeadersInOutputs"
   }
}
```

Para más información, consulte los temas siguientes:

* [Referencia de esquema para los tipos de desencadenador y de acción: desencadenador Request (Solicitud)](../logic-apps/logic-apps-workflow-actions-triggers.md#request-trigger)
* [Referencia de esquema para los tipos de desencadenador y de acción: desencadenador HTTP Webhook](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)
* [Referencia de esquema para los tipos de desencadenador y de acción: opciones de operación](../logic-apps/logic-apps-workflow-actions-triggers.md#operation-options)

<a name="azure-api-management"></a>

### <a name="expose-your-logic-app-with-azure-api-management"></a>Exposición de su aplicación lógica con Azure API Management

Para obtener más protocolos y opciones de autenticación, considere la posibilidad de exponer la aplicación lógica como una API mediante Azure API Management. Este servicio proporciona funcionalidades completas de supervisión, seguridad, directiva y documentación para cualquier punto de conexión. API Management puede exponer un punto de conexión público o privado para la aplicación lógica. Para autorizar el acceso a este punto de conexión, puede usar Azure Active Directory Open Authentication (Azure AD OAuth), el certificado del cliente u otros estándares de seguridad. Cuando API Management recibe una solicitud, el servicio la envía a la aplicación lógica y hace cualquier transformación o restricción necesaria en el proceso. Para que solo API Management pueda llamar a la aplicación lógica, puede [limitar el número de direcciones IP de entrada de la aplicación lógica](#restrict-inbound-ip).

Para más información, revise la siguiente documentación:

* [Acerca de API Management](../api-management/api-management-key-concepts.md)
* [Protección de un back-end de API web en Azure API Management mediante la autorización de OAuth 2.0 con Azure AD](../api-management/api-management-howto-protect-backend-with-aad.md)
* [Protección de API mediante la autenticación de certificados de cliente en API Management](../api-management/api-management-howto-mutual-certificates-for-clients.md)
* [Directivas de autenticación de Azure API Management](../api-management/api-management-authentication-policies.md)

<a name="restrict-inbound-ip"></a>

### <a name="restrict-inbound-ip-addresses"></a>Direcciones IP entrantes restringidas

Junto con la Firma de acceso compartido (SAS), es posible que quiera especificar el límite de clientes que pueden llamar a su aplicación lógica. Por ejemplo, si administra el punto de conexión de solicitud mediante [Azure API Management](../api-management/api-management-key-concepts.md), puede restringir la aplicación lógica para que solo acepte solicitudes procedentes de la dirección IP de la [instancia de servicio de API Management que crea](../api-management/get-started-create-service-instance.md).

Independientemente de las direcciones IP que se especifiquen, es posible ejecutar una aplicación lógica que tenga un desencadenador basado en una solicitud mediante la [API REST de Logic Apps: desencadenadores de flujo de trabajo: ejecutar](/rest/api/logic/workflowtriggers/run) o de API Management. Sin embargo, en este escenario aún se necesita la [autenticación](../active-directory/develop/authentication-vs-authorization.md) con la API REST de Azure. Todos los eventos aparecen en el registro de auditoría de Azure. Asegúrese de establecer las directivas de control de acceso como corresponda.

Para restringir las direcciones IP de entrada para la aplicación lógica, siga estos pasos tanto para Azure Portal como para la plantilla de Azure Resource Manager:

<a name="restrict-inbound-ip-portal"></a>

#### <a name="portal"></a>[Portal](#tab/azure-portal)

En [Azure Portal](https://portal.azure.com), este filtro afecta a los desencadenadores *y* a las acciones, contrariamente a la descripción del portal en **Direcciones IP entrantes permitidas**. Para configurar este filtro por separado para los desencadenadores y las acciones, use el objeto `accessControl` en una plantilla de Azure Resource Manager de la aplicación lógica o la [operación de la API REST de Logic Apps: Flujo de trabajo: crear o actualizar](/rest/api/logic/workflows/createorupdate).

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración de flujo de trabajo**.

1. En la sección **Configuración de control de acceso**, en **Direcciones IP entrantes permitidas**, elija la ruta de acceso del escenario:

   * Para que pueda llamar a la aplicación lógica solo como una aplicación lógica anidada mediante la [acción integrada de Azure Logic Apps](../logic-apps/logic-apps-http-endpoint.md), seleccione **Cualquier otra aplicación lógica**, que *solo* funciona cuando se usa la **Azure Logic Apps** para llamar a la aplicación lógica anidada.

     Esta opción escribe una matriz vacía en el recurso de aplicación lógica y requiere que solo las llamadas desde aplicaciones lógicas primarias que usan la acción integrada de **Azure Logic Apps** puedan desencadenar la aplicación lógica anidada.

   * Para que la aplicación lógica solo se pueda llamar como aplicación anidada mediante la acción HTTP, seleccione **Intervalos IP específicos**, *no* **Cualquier otra aplicación lógica**. Cuando aparezca el cuadro **Intervalos de IP para desencadenadores**, escriba las [direcciones IP de salida](../logic-apps/logic-apps-limits-and-config.md#outbound) de la aplicación lógica primaria. Un intervalo IP válido usa estos formatos: *x.x.x.x/x* o *x.x.x.x-x.x.x.x*.

     > [!NOTE]
     > Si usa la opción **Cualquier otra aplicación lógica** y la acción HTTP para llamar a la aplicación lógica anidada, la llamada se bloqueará y recibirá un error "401 No autorizado".

   * En el caso de los escenarios en los que quiere restringir las llamadas entrantes desde otras direcciones IP, cuando aparezca el cuadro **Intervalos de IP para desencadenadores**, especifique los intervalos de direcciones IP que acepta el desencadenador. Un intervalo IP válido usa estos formatos: *x.x.x.x/x* o *x.x.x.x-x.x.x.x*.

1. Si quiere, en **Restringir las llamadas para obtener mensajes de entrada y salida del historial de ejecución a las direcciones IP proporcionadas**, puede especificar los intervalos de direcciones IP para las llamadas entrantes que pueden acceder a los mensajes de entrada y salida en el historial de ejecución.

<a name="restrict-inbound-ip-template"></a>

#### <a name="resource-manager-template"></a>[Plantilla de Resource Manager](#tab/azure-resource-manager)

En la plantilla de ARM, especifique los intervalos de direcciones IP de entrada permitidos en la definición de recurso de la aplicación lógica mediante la sección `accessControl`. En esta parte, use las secciones `triggers`, `actions` y la sección `contents` opcional según corresponda, incluida la sección `allowedCallerIpAddresses` con la propiedad `addressRange` y establezca el valor de propiedad en el intervalo IP permitido con el formato *x.x.x.x/x* o *x.x.x.x-x.x.x.x*.

* Si la aplicación lógica anidada usa la opción **Cualquier otra aplicación lógica**, que permite llamadas entrantes solo desde otras aplicaciones lógicas que usan la acción integrada de Azure Logic Apps, establezca la propiedad `allowedCallerIpAddresses` en una matriz vacía ( **[]** ) y *omita* la propiedad `addressRange`.

* Si la aplicación lógica anidada usa la opción **Intervalos IP específicos** para otras llamadas entrantes, como otras aplicaciones lógicas que usan la acción HTTP, incluya la sección `allowedCallerIpAddresses` y establezca la propiedad `addressRange` en el intervalo de direcciones IP permitido.

En este ejemplo se muestra una definición de recursos para una aplicación lógica anidada que permite las llamadas entrantes solo desde aplicaciones lógicas que usan la acción integrada de Azure Logic Apps:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               <workflow-definition>
            },
            "parameters": {
            },
            "accessControl": {
               "triggers": {
                  "allowedCallerIpAddresses": []
               },
               "actions": {
                  "allowedCallerIpAddresses": []
               },
               // Optional
               "contents": {
                  "allowedCallerIpAddresses": []
               }
            },
            "endpointsConfiguration": {}
         }
      }
   ],
   "outputs": {}
}
```

En este ejemplo se muestra una definición de recursos para una aplicación lógica anidada que permite las llamadas entrantes desde aplicaciones lógicas que usan la acción HTTP:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               <workflow-definition>
            },
            "parameters": {
            },
            "accessControl": {
               "triggers": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     }
                  ]
               },
               "actions": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     }
                  ]
               }
            },
            "endpointsConfiguration": {}
         }
      }
   ],
   "outputs": {}
}
```

---

<a name="secure-operations"></a>

## <a name="access-to-logic-app-operations"></a>Acceso a las operaciones de las aplicaciones lógicas

Puede permitir que solo determinados usuarios o grupos ejecuten tareas específicas, como administrar, editar y ver aplicaciones lógicas. Para controlar los permisos, use el [control de acceso basado en roles de Azure (RBAC de Azure)](../role-based-access-control/role-assignments-portal.md). Puede asignar roles integrados o personalizados a los miembros que tienen acceso a la suscripción de Azure. Azure Logic Apps tiene estos roles específicos:

* [Colaborador de aplicación lógica](../role-based-access-control/built-in-roles.md#logic-app-contributor): Le permite administrar aplicaciones lógicas, pero no puede cambiar el acceso a ellas.

* [Operador de aplicación lógica](../role-based-access-control/built-in-roles.md#logic-app-operator): Le permite leer, habilitar y deshabilitar aplicaciones lógicas, pero no puede editarlas ni actualizarlas.

* [Colaborador](../role-based-access-control/built-in-roles.md#contributor): concede acceso completo para administrar todos los recursos, pero no le permite asignar roles en RBAC de Azure, administrar asignaciones en Azure Blueprints ni compartir galerías de imágenes.

  Por ejemplo, imagine que tiene que trabajar con una aplicación lógica que no ha creado y autenticar las conexiones usadas que usa el flujo de trabajo de esa aplicación lógica. La suscripción de Azure necesita permisos de colaborador para el grupo de recursos que contiene el recurso de esa aplicación lógica. Si crea un recurso de aplicación lógica, tendrá acceso de colaborador automáticamente.

Para evitar que otros usuarios cambien o elimine la aplicación lógica, puede usar el [bloqueo de recursos de Azure](../azure-resource-manager/management/lock-resources.md). Esta funcionalidad evita que otros usuarios cambien o eliminen los recursos de producción. Para obtener más información sobre la seguridad de la conexión, revise [Configuración de conexión en Azure Logic Apps](../connectors/apis-list.md#connection-configuration) y [Seguridad y cifrado de la conexión](../connectors/apis-list.md#connection-security-encyrption).

<a name="secure-run-history"></a>

## <a name="access-to-run-history-data"></a>Acceso a los datos del historial de ejecución

Durante la ejecución de una aplicación lógica, todos los datos [se cifran en tránsito](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) con Seguridad de la capa de transporte (TLS) y [en reposo](../security/fundamentals/encryption-atrest.md). Cuando finaliza la ejecución de la aplicación lógica, puede ver el historial de esa ejecución, incluidos los pasos que se ejecutaron junto con el estado, la duración, las entradas y las salidas de cada acción. Este completo detalle proporciona información sobre cómo se ejecuta la aplicación lógica y dónde puede empezar a solucionar los problemas que surjan.

Cuando visualiza el historial de ejecución de la aplicación lógica, Logic Apps autentica su acceso y proporciona vínculos a las entradas y salidas para las solicitudes y respuestas de cada ejecución. Sin embargo, para las acciones que controlan contraseñas, secretos, claves u otra información confidencial, es recomendable evitar que otros usuarios vean los datos y accedan a ellos. Por ejemplo, si la aplicación lógica obtiene un secreto de [Azure Key Vault](../key-vault/general/overview.md) para usarlo al autenticar una acción HTTP, puede ocultar ese secreto de la vista.

Para controlar el acceso a las entradas y salidas del historial de ejecución de la aplicación lógica, tiene estas opciones:

* [Restricción del acceso por intervalo de direcciones IP](#restrict-ip).

  Esta opción le permite proteger el acceso al historial de ejecución en función de las solicitudes de un intervalo de direcciones IP específico.

* [Protección de los datos del historial de ejecución mediante ofuscación](#obfuscate).

  En muchos desencadenadores y acciones, puede proteger las entradas, las salidas, o ambas, en el historial de ejecución de una aplicación lógica.

<a name="restrict-ip"></a>

### <a name="restrict-access-by-ip-address-range"></a>Restringir el acceso por intervalo de direcciones IP

Puede limitar el acceso a las entradas y salidas del historial de ejecución de la aplicación lógica para que solo las solicitudes de intervalos de direcciones IP específicas puedan ver los datos.

Por ejemplo, para impedir que alguien tenga acceso a las entradas y salidas, especifique un intervalo de direcciones IP como, por ejemplo, `0.0.0.0-0.0.0.0`. Solo una persona con permisos de administrador puede quitar esta restricción, lo que proporciona la posibilidad de usar el acceso "Just-In-Time" para los datos de la aplicación lógica.

Para especificar los intervalos IP permitidos, siga estos pasos tanto para Azure Portal como para la plantilla de Azure Resource Manager:

#### <a name="portal"></a>[Portal](#tab/azure-portal)

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración de flujo de trabajo**.

1. En **Configuración del control de acceso** > **Direcciones IP entrantes permitidas**, seleccione **Intervalos IP específicos**.

1. En **Intervalos IP para contenido**, especifique los intervalos de direcciones IP que pueden acceder a contenido desde las entradas y salidas.

   Un intervalo IP válido usa estos formatos: *x.x.x.x/x* o *x.x.x.x-x.x.x.x*

#### <a name="resource-manager-template"></a>[Plantilla de Resource Manager](#tab/azure-resource-manager)

En la plantilla de ARM, especifique los intervalos IP mediante la sección `accessControl` con la sección `contents` en la definición de recurso de la aplicación lógica; por ejemplo:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
            "parameters": {},
            "accessControl": {
               "contents": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     },
                     {
                        "addressRange": "2001:0db8::/64"
                     }
                  ]
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

---

<a name="obfuscate"></a>

### <a name="secure-data-in-run-history-by-using-obfuscation"></a>Protección del historial de ejecución mediante ofuscación

Muchos desencadenadores y acciones cuentan con una configuración para proteger las entradas, las salidas, o ambas, en el historial de ejecución de una aplicación lógica. Todos los *[conectores administrados](/connectors/connector-reference/connector-reference-logicapps-connectors) y [conectores personalizados](/connectors/custom-connectors/)* admiten estas opciones. Sin embargo, las siguientes [operaciones integradas](../connectors/built-in.md) ***no admiten estas opciones***:
     
| Proteger entradas: no se admite | Proteger salidas: no se admite |
|-----------------------------|------------------------------|
| Anexar a la variable de matriz <br>Anexar a la variable de cadena <br>Reducir variable <br>For Each <br>Si <br>Incrementar variable <br>Inicializar la variable <br>Periodicidad <br>Ámbito <br>Establecer la variable <br>Switch <br>Terminate <br>Until | Anexar a la variable de matriz <br>Anexar a la variable de cadena <br>Compose <br>Reducir variable <br>For Each <br>Si <br>Incrementar variable <br>Inicializar la variable <br>Parse JSON <br>Periodicidad <br>Response <br>Ámbito <br>Establecer la variable <br>Switch <br>Terminate <br>Until <br>Esperar |
|||

#### <a name="considerations-for-securing-inputs-and-outputs"></a>Consideraciones para proteger las entradas y salidas

Antes de usar esta configuración para proteger los datos, hay algunos aspectos que se deben tener en cuenta:
                 
* Cuando se ocultan las entradas o las salidas de un desencadenador o una acción, Logic Apps no envía los datos protegidos a Azure Log Analytics. Además, no se pueden agregar [propiedades con seguimiento](../logic-apps/monitor-logic-apps-log-analytics.md#extend-data) al desencadenador o acción para su supervisión.

* La [API de Logic Apps para controlar el historial del flujo de trabajo](/rest/api/logic/) no devuelve salidas protegidas.

* Para proteger las salidas de una acción que oculta las entradas o explícitamente las salidas, active de forma manual **Salidas seguras** en esa acción.

* Asegúrese de activar las **Entradas seguras** o las **Salidas seguras** en las acciones de nivel inferior en las que espera que el historial de ejecución oculte los datos.

  **Configuración de las salidas seguras**

  Al activar manualmente **Salidas seguras** en un desencadenador o una acción, Logic Apps oculta estas salidas en el historial de ejecución. Si una acción de nivel inferior usa explícitamente estas salidas protegidas como entradas, Logic Apps oculta las entradas de esta acción en el historial de ejecución, pero *no habilita* la opción de **Entradas seguras** de la acción.

  ![Salidas seguras como entradas y repercusión descendente en la mayoría de las acciones](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow.png)

  Las acciones de redacción, análisis JSON y respuesta solo tienen la opción **Entradas seguras**. Cuando está activada, esta opción también oculta las salidas de estas acciones. Si estas acciones usan explícitamente las salidas protegidas de nivel superior como entradas, Logic Apps ocultará las entradas y salidas de estas acciones, pero *no habilitará* la opción **Entradas seguras** de estas acciones. Si una acción de nivel inferior usa explícitamente las salidas ocultas de las acciones de redacción, análisis JSON o respuesta como entradas, Logic Apps *no oculta las entradas o salidas de la acción de nivel inferior*.

  ![Salidas seguras como entradas y repercusión descendente en acciones especificas](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow-special.png)

  **Opción Entradas seguras**

  Al activar manualmente **Entradas seguras** en un desencadenador o una acción, Logic Apps oculta estas entradas en el historial de ejecución. Si en una acción de nivel inferior se usan de forma explícita las salidas visibles de ese desencadenador o acción como entradas, Logic Apps oculta las entradas de esta acción de nivel inferior en el historial de ejecución, pero *no habilita* las **Entradas seguras** en esta acción y no oculta las salidas de la acción.

  ![Entradas seguras y repercusión descendente en la mayoría de las acciones](./media/logic-apps-securing-a-logic-app/secure-inputs-impact-on-downstream.png)

  Si las acciones de redacción, análisis JSON y respuesta usan explícitamente las salidas visibles del desencadenador o la acción que tiene las entradas protegidas, Logic Apps oculta las entradas y salidas de estas acciones, pero *no habilita* la opción **Entradas seguras** de la acción. Si una acción de nivel inferior usa explícitamente las salidas ocultas de las acciones de redacción, análisis JSON o respuesta como entradas, Logic Apps *no oculta las entradas o salidas de la acción de nivel inferior*.

  ![Entradas seguras y repercusión descendente en determinadas acciones](./media/logic-apps-securing-a-logic-app/secure-inputs-flow-special.png)

#### <a name="secure-inputs-and-outputs-in-the-designer"></a>Protección de entradas y salidas en el diseñador

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

   ![Abrir la aplicación lógica en el Diseñador de aplicación lógica](./media/logic-apps-securing-a-logic-app/open-sample-logic-app-in-designer.png)

1. En el desencadenador o la acción donde quiera proteger los datos confidenciales, seleccione los puntos suspensivos ( **...** ) y, luego, elija **Configuración**.

   ![Abrir configuración de desencadenador o acción](./media/logic-apps-securing-a-logic-app/open-action-trigger-settings.png)

1. Active **Entradas seguras**, **Salidas seguras** o ambas opciones. Cuando haya finalizado, seleccione **Listo**.

   ![Active "Entradas seguras" o "Salidas seguras".](./media/logic-apps-securing-a-logic-app/turn-on-secure-inputs-outputs.png)

   Ahora, la acción o el desencadenador muestra un icono de candado en la barra de título.

   ![La barra de título de acción o desencadenador muestra el icono de candado](./media/logic-apps-securing-a-logic-app/lock-icon-action-trigger-title-bar.png)

   Los tokens que representan las salidas protegidas de acciones anteriores también muestran los iconos de candado. Por ejemplo, al seleccionar un resultado de este tipo en la lista de contenido dinámico para usarlo en una acción, el token muestra un icono de candado.

   ![Seleccionar token para la salida protegida](./media/logic-apps-securing-a-logic-app/select-secured-token.png)

1. Una vez ejecutada la aplicación lógica, puede ver el historial de esa ejecución.

   1. En el panel **Información general** de la aplicación lógica, seleccione la ejecución que quiera ver.

   1. En el panel **Ejecución de aplicación lógica**, expanda las acciones que quiera revisar.

      Si decide ocultar las entradas y las salidas, esos valores aparecerán ahora ocultos.

      ![Entradas y salidas ocultas del historial de ejecución](./media/logic-apps-securing-a-logic-app/hidden-data-run-history.png)

<a name="secure-data-code-view"></a>

#### <a name="secure-inputs-and-outputs-in-code-view"></a>Protección de entradas y salidas en la vista Código

En la definición de desencadenador o acción subyacente, agregue o actualice la matriz `runtimeConfiguration.secureData.properties` con uno de estos valores o con ambos:

* `"inputs"`: Protege las entradas en el historial de ejecución.
* `"outputs"`: Protege las salidas en el historial de ejecución.

```json
"<trigger-or-action-name>": {
   "type": "<trigger-or-action-type>",
   "inputs": {
      <trigger-or-action-inputs>
   },
   "runtimeConfiguration": {
      "secureData": {
         "properties": [
            "inputs",
            "outputs"
         ]
      }
   },
   <other-attributes>
}
```

<a name="secure-action-parameters"></a>

## <a name="access-to-parameter-inputs"></a>Acceso a entradas de parámetros

Si implementa en entornos diferentes, considere la posibilidad de parametrizar los valores de la definición de flujo de trabajo que varían en función de esos entornos. De este modo, puede evitar los datos incluidos en el código mediante una [plantilla de Azure Resource Manager ](../azure-resource-manager/templates/overview.md) para implementar la aplicación lógica, proteger los datos confidenciales definiendo parámetros seguros y pasar dichos datos como entradas separadas mediante los [parámetros de la plantilla](../azure-resource-manager/templates/parameters.md) con un [archivo de parámetros](../azure-resource-manager/templates/parameter-files.md).

Por ejemplo, si autentica acciones HTTP con [Azure Active Directory Open Authentication](#azure-active-directory-oauth-authentication) (Azure AD OAuth), puede definir y ocultar los parámetros que aceptan el identificador de cliente y el secreto de cliente utilizados para la autenticación. Para definir estos parámetros para la aplicación lógica, use la sección `parameters` dentro de la definición de flujo de trabajo de la aplicación lógica y Resource Manager para la implementación. Para proteger los valores de parámetros que no quiera mostrar al editar la aplicación lógica o visualizar el historial de ejecución, defina los parámetros con el tipo `securestring` o `secureobject` y use la codificación necesaria. Los parámetros de este tipo no se devuelven con la definición del recurso y no son accesibles al visualizar el recurso después de la implementación. Para tener acceso a estos valores de parámetro durante el tiempo de ejecución, use la expresión `@parameters('<parameter-name>')` dentro de la definición del flujo de trabajo. Esta expresión solo se evalúa en tiempo de ejecución y se describe con el [lenguaje de definición de flujo de trabajo](../logic-apps/logic-apps-workflow-definition-language.md).

> [!NOTE]
> Si usa un parámetro en el encabezado o el cuerpo de una solicitud, ese parámetro se puede ver al visualizar el historial de ejecución de la aplicación lógica y la solicitud HTTP saliente. Asegúrese de definir también las directivas de acceso al contenido según corresponda.
> También puede usar la [ofuscación](#obfuscate) para ocultar entradas y salidas en el historial de ejecución. Los encabezados de autorización nunca son visibles a través de entradas o salidas. Por lo tanto, si aquí se usa un secreto, no se podrá recuperar.

Para más información, consulte las secciones siguientes en este tema:

* [Parámetros protegidos en las definiciones de flujo de trabajo](#secure-parameters-workflow)
* [Protección del historial de ejecución mediante ofuscación](#obfuscate)

Si [automatiza la implementación de aplicaciones lógicas con plantillas de Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede definir [parámetros de plantilla](../azure-resource-manager/templates/parameters.md) protegidos, que se evalúan en la implementación, mediante los tipos `securestring` y `secureobject`. Para definir parámetros de plantilla, use la sección `parameters` de nivel superior de la plantilla, que es independiente y diferente de la sección `parameters` de la definición de flujo de trabajo. Para proporcionar valores para los parámetros de plantilla, use un [archivo de parámetros](../azure-resource-manager/templates/parameter-files.md) independiente.

Por ejemplo, si usa secretos, puede definir y usar parámetros de plantilla protegidos que recuperen dichos secretos de [Azure Key Vault](../key-vault/general/overview.md) en la implementación. A continuación, puede hacer referencia a Key Vault y al secreto en el archivo de parámetros. Para más información, consulte los temas siguientes:

* [Uso de Azure Key Vault para pasar valores confidenciales durante la implementación](../azure-resource-manager/templates/key-vault-parameter.md)
* [Parámetros seguros en plantillas de Azure Resource Manager](#secure-parameters-deployment-template) más adelante en este tema

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-workflow-definitions"></a>Parámetros protegidos en las definiciones de flujo de trabajo

Para proteger la información confidencial en la definición del flujo de trabajo de aplicación lógica, use parámetros seguros, de forma que esta información no esté visible después de guardar la aplicación lógica. Por ejemplo, supongamos que tiene una acción HTTP que requiere autenticación básica que usa un nombre de usuario y una contraseña. En la definición del flujo, la sección `parameters` define los parámetros `basicAuthPasswordParam` y `basicAuthUsernameParam` mediante el tipo `securestring`. A continuación, la definición de la acción hace referencia a estos parámetros en la sección `authentication`.

```json
"definition": {
   "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
   "actions": {
      "HTTP": {
         "type": "Http",
         "inputs": {
            "method": "GET",
            "uri": "https://www.microsoft.com",
            "authentication": {
               "type": "Basic",
               "username": "@parameters('basicAuthUsernameParam')",
               "password": "@parameters('basicAuthPasswordParam')"
            }
         },
         "runAfter": {}
      }
   },
   "parameters": {
      "basicAuthPasswordParam": {
         "type": "securestring"
      },
      "basicAuthUsernameParam": {
         "type": "securestring"
      }
   },
   "triggers": {
      "manual": {
         "type": "Request",
         "kind": "Http",
         "inputs": {
            "schema": {}
         }
      }
   },
   "contentVersion": "1.0.0.0",
   "outputs": {}
}
```

<a name="secure-parameters-deployment-template"></a>

### <a name="secure-parameters-in-azure-resource-manager-templates"></a>Parámetros seguros en plantillas de Azure Resource Manager

Una [plantilla de Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) para una aplicación lógica tiene varias secciones `parameters`. Para proteger las contraseñas, las claves, los secretos y otros datos confidenciales, defina parámetros seguros en el nivel de plantilla y de definición de flujo de trabajo mediante el tipo `securestring` o `secureobject`. Después, puede almacenar estos valores en [Azure Key Vault](../key-vault/general/overview.md) y usar el [archivo de parámetros](../azure-resource-manager/templates/parameter-files.md) para hacer referencia al almacén de claves y al secreto. A continuación, la plantilla recupera esa información en la implementación. Para obtener más información, consulte [Uso de Azure Key Vault para pasar valores confidenciales durante la implementación](../azure-resource-manager/templates/key-vault-parameter.md).

Aquí tiene más información sobre estas secciones `parameters`:

* En el nivel superior de la plantilla, una sección `parameters` define los parámetros de los valores que utiliza la plantilla durante la *implementación*. Por ejemplo, estos valores pueden incluir cadenas de conexión para un entorno de implementación concreto. Después, puede almacenar estos valores en un [archivo de parámetros](../azure-resource-manager/templates/parameter-files.md), lo que facilita la modificación de estos valores.

* Dentro de la definición de recursos de la aplicación lógica, pero fuera de la definición del flujo de trabajo, una sección `parameters` especifica los valores de los parámetros de la definición del flujo de trabajo. En esta sección, puede asignar estos valores mediante expresiones de plantilla que hagan referencia a los parámetros de la plantilla. Estas expresiones se evalúan en la implementación.

* Dentro de la definición del flujo de trabajo, una sección `parameters` define los parámetros que usa la aplicación lógica en tiempo de ejecución. Después, puede hacer referencia a estos parámetros en el flujo de trabajo de la aplicación lógica mediante expresiones de definición de flujo de trabajo, que se evalúan en tiempo de ejecución.

Esta plantilla de ejemplo tiene varias definiciones de parámetros seguros que usan el tipo `securestring`:

| Nombre de parámetro | Descripción |
|----------------|-------------|
| `TemplatePasswordParam` | Parámetro de plantilla que acepta una contraseña que se pasa, a continuación, al parámetro `basicAuthPasswordParam` de la definición del flujo de trabajo. |
| `TemplateUsernameParam` | Parámetro de plantilla que acepta un nombre de usuario que se pasa a continuación al parámetro `basicAuthUserNameParam` de la definición del flujo de trabajo |
| `basicAuthPasswordParam` | Parámetro de definición de flujo de trabajo que acepta la contraseña para la autenticación básica en una acción HTTP |
| `basicAuthUserNameParam` | Parámetro de definición de flujo de trabajo que acepta el nombre de usuario para la autenticación básica en una acción HTTP |
|||

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "LogicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {
            "description": "Name of the Logic App."
         }
      },
      "TemplatePasswordParam": {
         "type": "securestring"
      },
      "TemplateUsernameParam": {
         "type": "securestring"
      },
      "LogicAppLocation": {
         "type": "string",
         "defaultValue": "[resourceGroup().location]",
         "allowedValues": [
            "[resourceGroup().location]",
            "eastasia",
            "southeastasia",
            "centralus",
            "eastus",
            "eastus2",
            "westus",
            "northcentralus",
            "southcentralus",
            "northeurope",
            "westeurope",
            "japanwest",
            "japaneast",
            "brazilsouth",
            "australiaeast",
            "australiasoutheast",
            "southindia",
            "centralindia",
            "westindia",
            "canadacentral",
            "canadaeast",
            "uksouth",
            "ukwest",
            "westcentralus",
            "westus2"
         ],
         "metadata": {
            "description": "Location of the Logic App."
         }
      }
   },
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
               "actions": {
                  "HTTP": {
                     "type": "Http",
                     "inputs": {
                        "method": "GET",
                        "uri": "https://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "@parameters('basicAuthUsernameParam')",
                           "password": "@parameters('basicAuthPasswordParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": {
                  "basicAuthPasswordParam": {
                     "type": "securestring"
                  },
                  "basicAuthUsernameParam": {
                     "type": "securestring"
                  }
               },
               "triggers": {
                  "manual": {
                     "type": "Request",
                     "kind": "Http",
                     "inputs": {
                        "schema": {}
                     }
                  }
               },
               "contentVersion": "1.0.0.0",
               "outputs": {}
            },
            "parameters": {
               "basicAuthPasswordParam": {
                  "value": "[parameters('TemplatePasswordParam')]"
               },
               "basicAuthUsernameParam": {
                  "value": "[parameters('TemplateUsernameParam')]"
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="secure-outbound-requests"></a>

## <a name="access-for-outbound-calls-to-other-services-and-systems"></a>Acceso de las llamadas salientes a otros servicios y sistemas

En función de la funcionalidad del punto de conexión de destino, las llamadas salientes enviadas por el [desencadenador HTTP o la acción HTTP](../connectors/connectors-native-http.md) admiten el cifrado y están protegidas mediante el protocolo [Seguridad de la capa de transporte (TLS) 1.0, 1.1 o 1.2](https://en.wikipedia.org/wiki/Transport_Layer_Security), conocido anteriormente como Capa de sockets seguros (SSL). Logic Apps negocia con el punto de conexión de destino el uso de la versión más alta que sea compatible. Por ejemplo, si el punto de conexión de destino admite la versión 1.2, el desencadenador o la acción HTTP usan primero esta versión. De lo contrario, el conector utiliza la siguiente versión compatible más alta.

A continuación se muestra información acerca de los certificados autofirmados de TLS/SSL:

* En el caso de aplicaciones lógicas en el entorno global multiinquilino de Azure Logic Apps, las operaciones HTTP no permiten certificados de TLS/SSL autofirmados. Si la aplicación lógica realiza una llamada HTTP a un servidor y presenta un certificado autofirmado de TLS/SSL, la llamada HTTP produce un error `TrustFailure`.

* En el caso de aplicaciones lógicas en el entorno de Azure Logic Apps de inquilino único, las operaciones HTTP admiten certificados TLS/SSL autofirmados. Sin embargo, debe completar algunos pasos adicionales para este tipo de autenticación. De lo contrario, se produce un error en la llamada. Para obtener más información, revise [Autenticación con certificados TLS/SSL para Azure Logic Apps de inquilino único](../connectors/connectors-native-http.md#tlsssl-certificate-authentication).

  Si quiere usar el certificado de cliente o Azure Active Directory y Open Authentication (Azure AD OAuth) con el tipo de credencial "Certificado" en su lugar, todavía tiene que completar algunos pasos adicionales para este tipo de autenticación. De lo contrario, se produce un error en la llamada. Para obtener más información, revise [Certificado de cliente o Azure Active Directory y Open Authentication (Azure AD OAuth) con el tipo de credencial "Certificado" para Azure Logic Apps de inquilino único](../connectors/connectors-native-http.md#client-certificate-authentication).

* En el caso de las aplicaciones lógicas en un [entorno de servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), el conector HTTP permite certificados autofirmados para los protocolos de enlace TLS/SSL. Sin embargo, primero debe [habilitar la compatibilidad con certificados autofirmados](../logic-apps/create-integration-service-environment-rest-api.md#request-body) para un ISE existente o un ISE nuevo mediante la API REST de Logic Apps, y, después, instalar el certificado público en la ubicación de `TrustedRoot`.

Estas son otras formas en que se puede ayudar a proteger los punto de conexión que controlan las llamadas enviadas desde su aplicación lógica:

* [Incorporación de la autenticación a las solicitudes salientes](#add-authentication-outbound).

  Cuando se usa el desencadenador o la acción HTTP para realizar llamadas salientes, se puede agregar autenticación a la solicitud enviada por la aplicación lógica. Por ejemplo, puede seleccionar estos tipos de autenticación:

  * [Autenticación básica](#basic-authentication)

  * [Autenticación de certificados de clientes](#client-certificate-authentication)

  * [Autenticación de Active Directory OAuth](#azure-active-directory-oauth-authentication)

  * [Autenticación de identidad administrada](#managed-identity-authentication)

* Restricción del acceso desde direcciones IP de aplicación lógica.

  Todas las llamadas a los puntos de conexión desde aplicaciones lógicas proceden de direcciones IP designadas específicas que se basan en las regiones de las aplicaciones lógicas. Puede agregar un filtrado que solo acepta solicitudes provenientes de esas direcciones IP. Para obtener esas direcciones IP, consulte [Límites y configuración de Azure Logic Apps](logic-apps-limits-and-config.md#firewall-ip-configuration).

* Mejore la seguridad de las conexiones a sistemas locales.

  Azure Logic Apps ofrece integración con estos servicios para permitir una comunicación local más segura y confiable.

  * Puerta de enlace de datos local

    Muchos conectores administrados de Azure Logic Apps proporcionan conexiones seguras a sistemas locales, como el sistema de archivos, SQL, SharePoint y DB2. La puerta de enlace envía datos desde orígenes locales en canales cifrados hasta Azure Service Bus. Todo el tráfico se origina como tráfico de salida seguro desde el agente de la puerta de enlace. Obtenga información sobre [el funcionamiento de la puerta de enlace de datos local](logic-apps-gateway-install.md#gateway-cloud-service).

  * Conexión mediante Azure API Management

    [Azure API Management](../api-management/api-management-key-concepts.md) ofrece opciones de conexión local, como la integración de [ExpressRoute](../expressroute/expressroute-introduction.md) y una red privada virtual sitio a sitio para un proxy seguro y comunicación con sistemas locales. Si tiene una API que proporciona acceso al sistema local y expuso esa API mediante la creación de una [instancia del servicio API Management](../api-management/get-started-create-service-instance.md), puede llamar a esa API en el flujo de trabajo de la aplicación lógica seleccionando la acción o el desencadenador de API Management integrados en el Diseñador de aplicación lógica.

    > [!NOTE]
    > El conector muestra solo los servicios de API Management donde cuenta con permisos para ver y conectarse, pero no muestra los servicios de API Management basados en el consumo.

    1. En el Diseñador de aplicación lógica, escriba `api management` en el cuadro de búsqueda. Elija el paso en función de si va a agregar un desencadenador o una acción:<p>

       * Si agrega un desencadenador, que siempre es el primer paso en el flujo de trabajo, seleccione **Choose an Azure API Management trigger** (Elegir un desencadenador de Azure API Management).

       * Si agrega una acción, seleccione **Choose an Azure API Management action** (Elegir una acción de Azure API Management).

       En este ejemplo se agrega un desencadenador:

       ![Incorporación del desencadenador de Azure API Management](./media/logic-apps-securing-a-logic-app/select-api-management.png)

    1. Seleccione su instancia de servicio API Management creada anteriormente.

       ![Selección de la instancia de servicio API Management](./media/logic-apps-securing-a-logic-app/select-api-management-service-instance.png)

    1. Seleccione la llamada API que se va a usar.

       ![Selección de la API existente](./media/logic-apps-securing-a-logic-app/select-api.png)

<a name="add-authentication-outbound"></a>

### <a name="add-authentication-to-outbound-calls"></a>Incorporación de la autenticación en las llamadas salientes

Los extremos HTTP y HTTPS admiten varios tipos de autenticación. En algunos desencadenadores y acciones que se usan para enviar llamadas o solicitudes salientes a estos puntos de conexión, se puede especificar un tipo de autenticación. En el Diseñador de aplicación lógica, los desencadenadores y las acciones que admiten la elección de un tipo de autenticación tienen una propiedad **Autenticación**. Sin embargo, es posible que esta propiedad no aparezca siempre de forma predeterminada. En estos casos, en el desencadenador o la acción, abra la lista **Agregar nuevo parámetro** y seleccione **Autenticación**.

> [!IMPORTANT]
> Para proteger la información confidencial que controla la aplicación lógica, use los parámetros seguros y codifique los datos según sea necesario.
> Para obtener más información acerca de cómo usar y proteger los parámetros, consulte [Acceso a las entradas de parámetro](#secure-action-parameters).

<a name="authentication-types-supported-triggers-actions"></a>

#### <a name="authentication-types-for-triggers-and-actions-that-support-authentication"></a>Tipos de autenticación para desencadenadores y acciones que admiten la autenticación

En esta tabla se identifican los tipos de autenticación que están disponibles en los desencadenadores y las acciones donde puede seleccionar un tipo de autenticación:

| Tipo de autenticación | Desencadenadores y acciones admitidos |
|---------------------|--------------------------------|
| [Basic](#basic-authentication) | Azure API Management, Azure App Services, HTTP, HTTP y Swagger, webhook HTTP |
| [Certificado de cliente](#client-certificate-authentication) | Azure API Management, Azure App Services, HTTP, HTTP y Swagger, webhook HTTP |
| [Active Directory OAuth](#azure-active-directory-oauth-authentication) | Azure API Management, Azure App Services, Azure Functions, HTTP, HTTP + Swagger, webhook HTTP |
| [Sin formato](#raw-authentication) | Azure API Management, Azure App Services, Azure Functions, HTTP, HTTP + Swagger, webhook HTTP |
| [Identidad administrada](#managed-identity-authentication) | **Aplicación lógica (consumo)** : <p><p>- **Integrados**: Azure API Management, Azure App Services, Azure Functions, HTTP, HTTP Webhook <p><p>- **Conector administrado** (versión preliminar): <p><p>--- **Autenticación única**: Azure AD Identity Protection, Azure Automation, Azure Container Instance, Azure Data Explorer, Azure Data Factory, Azure Data Lake, Azure Event Grid, Azure IoT Central V3, Azure Key Vault, Azure Resource Manager, Azure Sentinel, HTTP con Azure AD <p><p>--- **Autenticación múltiple**: Azure Blob Storage, SQL Server <p><p>___________________________________________________________________________________________<p><p>**Aplicación lógica (estándar)** : <p><p>- **Integrados**: HTTP, HTTP Webhook <p><p>- **Conector administrado** (versión preliminar): <p>--- **Autenticación única**: Azure AD Identity Protection, Azure Automation, Azure Container Instance, Azure Data Explorer, Azure Data Factory, Azure Data Lake, Azure Event Grid, Azure IoT Central V3, Azure Key Vault, Azure Resource Manager, Azure Sentinel, HTTP con Azure AD <p><p>--- **Autenticación múltiple**: Azure Blob Storage, SQL Server |
|||

<a name="basic-authentication"></a>

#### <a name="basic-authentication"></a>Autenticación básica

Si la opción [Básica](../active-directory-b2c/secure-rest-api.md) está disponible, especifique estos valores de propiedad:

| Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
|---------------------|-----------------|----------|-------|-------------|
| **Autenticación** | `type` | Sí | Básico | Tipo de autenticación que se debe usar. |
| **Nombre de usuario** | `username` | Sí | <*nombre-de-usuario*>| Nombre de usuario para autenticar el acceso al extremo del servicio de destino. |
| **Contraseña** | `password` | Sí | <*contraseña*> | Contraseña para autenticar el acceso al extremo del servicio de destino. |
||||||

Al usar [parámetros protegidos](#secure-action-parameters) para administrar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para acceder a estos valores de parámetros en tiempo de ejecución. Esta definición de acción HTTP de ejemplo especifica el `type` de autenticación como `Basic` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Basic",
         "username": "@parameters('userNameParam')",
         "password": "@parameters('passwordParam')"
      }
  },
  "runAfter": {}
}
```

<a name="client-certificate-authentication"></a>

#### <a name="client-certificate-authentication"></a>Autenticación de certificados de cliente

Si la opción [Certificado de cliente](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) está disponible, especifique estos valores de propiedad:

| Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
|---------------------|-----------------|----------|-------|-------------|
| **Autenticación** | `type` | Sí | **Certificado de cliente** <br>or <br>`ClientCertificate` | Tipo de autenticación que se debe usar. Puede administrar certificados con [Azure API Management](../api-management/api-management-howto-mutual-certificates.md). <p></p>**Nota**: Los conectores personalizados no admiten la autenticación basada en certificados para las llamadas entrantes y salientes. |
| **Pfx** | `pfx` | Sí | <*contenido-archivo-pfx-codificado*> | El contenido codificado en base 64 del archivo de intercambio de información personal (PFX) <p><p>Para convertir el archivo PFX en formato codificado en Base64, puede usar PowerShell siguiendo estos pasos: <p>1. Guarde el contenido del certificado en una variable: <p>   `$pfx_cert = get-content 'c:\certificate.pfx' -Encoding Byte` <p>2. Convierta el contenido del certificado mediante la función `ToBase64String()` y guarde el contenido en un archivo de texto: <p>   `[System.Convert]::ToBase64String($pfx_cert) | Out-File 'pfx-encoded-bytes.txt'` <p><p>**Solución de problemas**: Si usa el comando `cert mmc/PowerShell`, podría obtener este error: <p><p>`Could not load the certificate private key. Please check the authentication certificate password is correct and try again.` <p><p>Para resolver este error, intente convertir el archivo PFX en un archivo PEM y nuevamente mediante el comando `openssl`: <p><p>`openssl pkcs12 -in certificate.pfx -out certificate.pem` <br>`openssl pkcs12 -in certificate.pem -export -out certificate2.pfx` <p><p>Después, al obtener la cadena codificada en base64 para el archivo PFX recién convertido del certificado, la cadena funcionará en Azure Logic Apps. |
| **Contraseña** | `password`| No | <*contraseña-archivo-pfx*> | La contraseña para acceder al archivo PFX |
|||||

Al usar [parámetros protegidos](#secure-action-parameters) para administrar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para acceder a estos valores de parámetros en tiempo de ejecución. Esta definición de acción HTTP de ejemplo especifica el `type` de autenticación como `ClientCertificate` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ClientCertificate",
         "pfx": "@parameters('pfxParam')",
         "password": "@parameters('passwordParam')"
      }
   },
   "runAfter": {}
}
```

> [!IMPORTANT]
> Si tiene un recurso **Aplicación lógica (estándar)** en Azure Logic Apps de inquilino único y quiere usar una operación HTTP con un certificado TSL/SSL, un certificado de cliente o Azure Active Directory en Open Authentication (Azure AD OAuth) con el tipo de credencial `Certificate`, asegúrese de completar los pasos de configuración adicionales para este tipo de autenticación. De lo contrario, se produce un error en la llamada. Para obtener más información, revise [Autenticación en un entorno de inquilino único](../connectors/connectors-native-http.md#single-tenant-authentication).

Para obtener más información acerca de cómo proteger los servicios mediante la autenticación de certificados de cliente, consulte estos temas:

* [Cómo mejorar la seguridad de las API mediante la autenticación de certificados de cliente en Azure API Management](../api-management/api-management-howto-mutual-certificates-for-clients.md)
* [Cómo mejorar la seguridad de los servicios back-end con la autenticación de certificados de cliente en Azure API Management](../api-management/api-management-howto-mutual-certificates.md)
* [Cómo mejorar la seguridad de los servicios RESTful mediante certificados de cliente](../active-directory-b2c/secure-rest-api.md)
* [Credenciales de certificado para la autenticación de aplicaciones](../active-directory/develop/active-directory-certificate-credentials.md)
* [Uso de un certificado TLS/SSL en el código de Azure App Service](../app-service/configure-ssl-certificate-in-code.md)

<a name="azure-active-directory-oauth-authentication"></a>

#### <a name="azure-active-directory-open-authentication"></a>Azure Active Directory Open Authentication

En los desencadenadores Request (Solicitud), se puede usar [Azure Active Directory Open Authentication (Azure AD OAuth)](../active-directory/develop/index.yml) para autenticar las llamadas entrantes después de [configurar las directivas de autorización de Azure AD](#enable-oauth) de la aplicación lógica. En el caso del resto de desencadenadores y acciones que proporcionan el tipo de autenticación **Active Directory OAuth**, especifique estos valores de propiedad:

| Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
|---------------------|-----------------|----------|-------|-------------|
| **Autenticación** | `type` | Sí | **Active Directory OAuth** <br>or <br>`ActiveDirectoryOAuth` | Tipo de autenticación que se debe usar. Actualmente, Logic Apps sigue el [protocolo OAuth 2.0](../active-directory/develop/v2-overview.md). |
| **Autoridad** | `authority` | No | <*URL-for-authority-token-issuer*> | Dirección URL de la autoridad que proporciona el token de acceso, como `https://login.microsoftonline.com/` para las regiones de servicio globales de Azure. Para otras nubes nacionales, consulte [Puntos de conexión de Azure AD de autenticación: elección de una autoridad de identidad](../active-directory/develop/authentication-national-cloud.md#azure-ad-authentication-endpoints). |
| **Inquilino** | `tenant` | Sí | <*tenant-ID*> | El identificador del inquilino de Azure AD |
| **Audiencia** | `audience` | Sí | <*resource-to-authorize*> | Recurso que quiere usar para la autorización; por ejemplo, `https://management.core.windows.net/` |
| **Id. de cliente** | `clientId` | Sí | <*client-ID*> | El identificador de cliente para la aplicación que solicita autorización |
| **Tipo de credencial** | `credentialType` | Sí | Certificado <br>or <br>Secreto | El tipo de credencial que el cliente usa para solicitar autorización. Esta propiedad y el valor no aparecen en la definición subyacente de la aplicación lógica, pero determina las propiedades que se muestran para el tipo de credencial seleccionado. |
| **Secreto** | `secret` | Sí, pero solo para el tipo de credencial de "Secreto". | <*secreto-de-cliente*> | Secreto de cliente para solicitar la autorización |
| **Pfx** | `pfx` | Sí, pero solo para el tipo de credencial de "Certificado". | <*contenido-archivo-pfx-codificado*> | El contenido codificado en base 64 del archivo de intercambio de información personal (PFX) |
| **Contraseña** | `password` | Sí, pero solo para el tipo de credencial de "Certificado". | <*contraseña-archivo-pfx*> | La contraseña para acceder al archivo PFX |
|||||

Al usar [parámetros protegidos](#secure-action-parameters) para administrar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para acceder a estos valores de parámetros en tiempo de ejecución. Esta definición de acción HTTP de ejemplo especifica el `type` de autenticación como `ActiveDirectoryOAuth`, el tipo de credencial `Secret` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ActiveDirectoryOAuth",
         "tenant": "@parameters('tenantIdParam')",
         "audience": "https://management.core.windows.net/",
         "clientId": "@parameters('clientIdParam')",
         "credentialType": "Secret",
         "secret": "@parameters('secretParam')"
     }
   },
   "runAfter": {}
}
```

> [!IMPORTANT]
> Si tiene un recurso **Aplicación lógica (estándar)** en Azure Logic Apps de inquilino único y quiere usar una operación HTTP con un certificado TSL/SSL, un certificado de cliente o Azure Active Directory en Open Authentication (Azure AD OAuth) con el tipo de credencial `Certificate`, asegúrese de completar los pasos de configuración adicionales para este tipo de autenticación. De lo contrario, se produce un error en la llamada. Para obtener más información, revise [Autenticación en un entorno de inquilino único](../connectors/connectors-native-http.md#single-tenant-authentication).

<a name="raw-authentication"></a>

#### <a name="raw-authentication"></a>Autenticación sin formato

Si la opción **Raw** está disponible, puede usar este tipo de autenticación cuando tenga que usar [esquemas de autenticación](https://iana.org/assignments/http-authschemes/http-authschemes.xhtml) que no siguen el [protocolo OAuth 2.0](https://oauth.net/2/). Con este tipo, se crea manualmente el valor del encabezado de autorización que se envía con la solicitud saliente y se especifica ese valor de encabezado en el desencadenador o la acción.

Por ejemplo, el siguiente es un encabezado de ejemplo para una solicitud HTTPS que sigue el [protocolo OAuth 1.0](https://tools.ietf.org/html/rfc5849):

```text
Authorization: OAuth realm="Photos",
   oauth_consumer_key="dpf43f3p2l4k3l03",
   oauth_signature_method="HMAC-SHA1",
   oauth_timestamp="137131200",
   oauth_nonce="wIjqoS",
   oauth_callback="http%3A%2F%2Fprinter.example.com%2Fready",
   oauth_signature="74KNZJeDHnMBp0EMJ9ZHt%2FXKycU%3D"
```

En el desencadenador o la acción que admite la autenticación sin formato, especifique estos valores de propiedad:

| Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
|---------------------|-----------------|----------|-------|-------------|
| **Autenticación** | `type` | Sí | Raw | Tipo de autenticación que se debe usar. |
| **Valor** | `value` | Sí | <*valor de encabezado de autorización*> | Valor del encabezado de autorización que se va a usar para la autenticación. |
||||||

Al usar [parámetros protegidos](#secure-action-parameters) para administrar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para acceder a estos valores de parámetros en tiempo de ejecución. Esta definición de acción HTTP de ejemplo especifica el `type` de autenticación como `Raw` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Raw",
         "value": "@parameters('authHeaderParam')"
      }
   },
   "runAfter": {}
}
```

<a name="managed-identity-authentication"></a>

#### <a name="managed-identity-authentication"></a>Autenticación de identidad administrada

Cuando la opción [Identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) está disponible en el [desencadenador o la acción que admite la autenticación de identidad administrada](#add-authentication-outbound), la aplicación lógica puede usar esta identidad para autenticar el acceso a recursos de Azure que están protegidos por Azure Active Directory (Azure AD), en lugar de credenciales, secretos o tokens de Azure AD. Azure administra esta identidad y le ayuda a proteger las credenciales porque, de esta forma, no tiene que administrar secretos ni usar directamente tokens de Azure AD. Obtenga más información sobre [Servicios de Azure que admiten las identidades administradas para la autenticación de Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

* El tipo de recurso **Aplicación lógica (consumo)** puede usar la identidad asignada por el sistema o una *única* identidad asignada por el usuario creada manualmente.

* El tipo de recurso **Aplicación lógica (estándar)** solo puede usar la identidad asignada por el sistema, que se habilita de forma automática. La identidad asignada por el usuario no está disponible actualmente.

1. Para que la aplicación lógica pueda usar una identidad administrada, siga los pasos descritos en [Autenticación de acceso a los recursos de Azure con identidades administradas en Azure Logic Apps](../logic-apps/create-managed-service-identity.md). En estos pasos se habilita la identidad administrada en la aplicación lógica y se configura el acceso de dicha identidad al recurso de Azure de destino.

1. Para que una función de Azure pueda usar una identidad administrada, primero [habilite la autenticación de Azure Functions](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-functions).

1. En el desencadenador o la acción que admite el uso de una identidad administrada, proporcione esta información:

   **Acciones y desencadenadores integrados**

   | Propiedad (diseñador) | Propiedad (JSON) | Obligatorio | Value | Descripción |
   |---------------------|-----------------|----------|-------|-------------|
   | **Autenticación** | `type` | Sí | **Identidad administrada** <br>or <br>`ManagedServiceIdentity` | Tipo de autenticación que se debe usar. |
   | **Identidad administrada** | `identity` | Sí | * **Identidad administrada asignada por el sistema** <br>or <br>`SystemAssigned` <p><p>* <*Id. de identidad asignada por el usuario*> | Identidad administrada que se debe usar. |
   | **Audiencia** | `audience` | Sí | <*Id-recurso-destino*> | El Id. de recurso para el recurso de destino al que quiere obtener acceso. <p>Por ejemplo, `https://storage.azure.com/` hace que los [tokens de acceso](../active-directory/develop/access-tokens.md) para la autenticación sean válidos con todas las cuentas de almacenamiento. Sin embargo, también puede especificar una dirección URL de servicio raíz, como `https://fabrikamstorageaccount.blob.core.windows.net` para una cuenta de almacenamiento específica. <p>**Nota**: La propiedad **Audiencia** puede estar oculta en algunos desencadenadores o acciones. Para que la propiedad sea visible, en el desencadenador o la acción, abra la lista **Agregar nuevo parámetro** y seleccione **Público**. <p><p>**Importante**: Asegúrese de que el identificador de este recurso de destino *coincide exactamente* con el valor esperado en Azure AD, incluida toda barra diagonal necesaria al final. Por lo tanto, el Id. de recurso de `https://storage.azure.com/` para todas las cuentas de Azure Blob Storage requiere una barra diagonal final. Sin embargo, el Id. de recurso de una cuenta de almacenamiento específica no requiere una barra diagonal final. Para buscar estos Id. de recursos, consulte [Servicios de Azure que admiten la autenticación de Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). |
   |||||

   Al usar [parámetros protegidos](#secure-action-parameters) para administrar y proteger la información confidencial, por ejemplo, en una [plantilla de Azure Resource Manager para automatizar la implementación](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), puede usar expresiones para acceder a estos valores de parámetros en tiempo de ejecución. Por ejemplo, esta definición de acción HTTP especifica el `type` de autenticación como `ManagedServiceIdentity` y usa la [función parameters()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) para obtener los valores de parámetro:

   ```json
   "HTTP": {
      "type": "Http",
      "inputs": {
         "method": "GET",
         "uri": "@parameters('endpointUrlParam')",
         "authentication": {
            "type": "ManagedServiceIdentity",
            "identity": "SystemAssigned",
            "audience": "https://management.azure.com/"
         },
      },
      "runAfter": {}
   }
   ```

   **Desencadenadores y acciones del conector administrado**

   | Propiedad (diseñador) | Obligatorio | Value | Descripción |
   |---------------------|----------|-------|-------------|
   | **Nombre de la conexión** | Sí | <*connection-name*> ||
   | **Identidad administrada** | Sí | **Identidad administrada asignada por el sistema** <br>or <br> <*user-assigned-managed-identity-name*> | Tipo de autenticación que se debe usar. |
   |||||

<a name="block-connections"></a>

## <a name="block-creating-connections"></a>Bloquear la creación de conexiones

Si su organización no permite la conexión a recursos específicos mediante el uso de sus conectores en Azure Logic Apps, puede [bloquear la capacidad para crear esas conexiones](../logic-apps/block-connections-connectors.md) para conectores específicos en flujos de trabajo de aplicaciones lógicas mediante el uso de [Azure Policy](../governance/policy/overview.md). Para obtener más información, consulte [Bloquear las conexiones creadas por conectores específicos en Azure Logic Apps](../logic-apps/block-connections-connectors.md).

<a name="isolation-logic-apps"></a>

## <a name="isolation-guidance-for-logic-apps"></a>Guía de aislamiento para aplicaciones lógicas

Puede usar Azure Logic Apps en [Azure Government](../azure-government/documentation-government-welcome.md), que admite todos los niveles de impacto en las regiones que se describen en la [guía de aislamiento de nivel de impacto 5 de Azure Government](../azure-government/documentation-government-impact-level-5.md). Para cumplir estos requisitos, Logic Apps admite la funcionalidad para que cree y ejecute flujos de trabajo en un entorno con recursos dedicados de modo que pueda reducir el impacto en el rendimiento de otros inquilinos de Azure en las aplicaciones lógicas y evitar compartir recursos informáticos con otros inquilinos.

* Para ejecutar su propio código o realizar la transformación XML, [cree y llame a una función de Azure](../logic-apps/logic-apps-azure-functions.md), en vez de usar la [funcionalidad de código en línea](../logic-apps/logic-apps-add-run-inline-code.md) o proporcionar [ensamblados para usarlos como mapas](../logic-apps/logic-apps-enterprise-integration-maps.md), respectivamente. Asimismo, configure el entorno de hospedaje de la aplicación de funciones para cumplir los requisitos de aislamiento.

  Por ejemplo, para cumplir los requisitos de nivel de impacto 5, cree la aplicación de funciones con el [plan de App Service](../azure-functions/dedicated-plan.md) mediante el plan de tarifa [**aislado**](../app-service/overview-hosting-plans.md) junto con una instancia de [App Service Environment (ASE)](../app-service/environment/intro.md) que también usa el plan de tarifa **aislado**. En este entorno, las aplicaciones de funciones se ejecutan en máquinas virtuales y redes virtuales de Azure dedicadas, que proporcionan aislamiento de red sobre el aislamiento de proceso para las aplicaciones y las máximas posibilidades de escalabilidad horizontal.

  Para más información, revise la siguiente documentación:

  * [Planes de Azure App Service](../app-service/overview-hosting-plans.md)
  * [Opciones de redes de Azure Functions](../azure-functions/functions-networking-options.md)
  * [Hosts dedicados de Azure para máquinas virtuales](../virtual-machines/dedicated-hosts.md)
  * [Aislamiento de máquina virtual de Azure](../virtual-machines/isolation.md)
  * [Implementación de servicios de Azure dedicados en las redes virtuales](../virtual-network/virtual-network-for-azure-services.md)

* En función de si usa [Azure Logic Apps de inquilino único y multiinquilino](logic-apps-overview.md#resource-environment-differences), tiene estas opciones:

  * Con las aplicaciones lógicas basadas en un solo inquilino, puede comunicarse de forma privada y segura entre flujos de trabajo de aplicación lógica y una instancia de Azure Virtual Network mediante la configuración de puntos de conexión privados para el tráfico entrante, así como usar la integración de Virtual Network para el tráfico saliente. Para obtener más información, revise [Protección del tráfico entre redes virtuales y Azure Logic Apps de inquilino único mediante puntos de conexión privados](secure-single-tenant-workflow-virtual-network-private-endpoint.md).

  * Con las aplicaciones lógicas basadas en varios inquilinos, puede crear y ejecutar las aplicaciones lógicas en un [entorno del servicio de integración (ISE)](connect-virtual-network-vnet-isolated-environment-overview.md). De esa forma, las aplicaciones lógicas se ejecutan en recursos dedicados y pueden acceder a recursos protegidos por una instancia de Azure Virtual Network. Para obtener más control sobre las claves de cifrado utilizadas por Azure Storage, puede configurar, usar y administrar su propia clave mediante [Azure Key Vault](../key-vault/general/overview.md). Esta funcionalidad también se conoce como "Bring Your Own Key" (BYOK) y la clave se denomina "clave administrada por el cliente". Para obtener más información, revise [Configure claves administradas por el cliente para cifrar los datos en reposo para los entornos del servicio de integración (ISE) en Azure Logic Apps](../logic-apps/customer-managed-keys-integration-service-environment.md).

  > [!IMPORTANT]
  > Algunas redes virtuales de Azure usan puntos de conexión privados ([Azure Private Link](../private-link/private-link-overview.md)) para proporcionar acceso a los servicios de PaaS de Azure, como Azure Storage, Azure Cosmos DB o Azure SQL Database, servicios de asociados o servicios de clientes que se hospedan en Azure.
  >
  > Si los flujos de trabajo necesitan acceso a redes virtuales que usan puntos de conexión privados y desea desarrollar esos flujos de trabajo mediante el tipo de recurso **Aplicación lógica (consumo)** , *debe crear y ejecutar las aplicaciones lógicas en un entorno del servicio de integración*. Sin embargo, si desea desarrollar esos flujos de trabajo mediante el tipo de recurso **Aplicación lógica (estándar)** , *no necesita un entorno del servicio de integración*. 
  > En su lugar, los flujos de trabajo pueden comunicarse de forma privada y segura con redes virtuales mediante el uso de puntos de conexión privados para el tráfico entrante y la integración de Virtual Network para el tráfico saliente. Para obtener más información, revise [Protección del tráfico entre redes virtuales y Azure Logic Apps de inquilino único mediante puntos de conexión privados](secure-single-tenant-workflow-virtual-network-private-endpoint.md).

Para obtener más información sobre el aislamiento, revise la siguiente documentación:

* [Aislamiento en la nube pública de Azure](../security/fundamentals/isolation-choices.md)
* [Seguridad para aplicaciones IaaS altamente confidenciales en Azure](/azure/architecture/reference-architectures/n-tier/high-security-iaas)

## <a name="next-steps"></a>Pasos siguientes

* [Base de referencia de seguridad de Azure para Azure Logic Apps](../logic-apps/security-baseline.md)
* [Automatización de la implementación para Azure Logic Apps](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
* [Supervisión de aplicaciones lógicas](../logic-apps/monitor-logic-apps-log-analytics.md)
