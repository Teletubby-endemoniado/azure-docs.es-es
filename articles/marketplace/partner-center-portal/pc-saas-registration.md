---
title: 'Registro de una aplicación SaaS: Azure Marketplace'
description: Aprenda a usar Azure Portal para registrar una aplicación SaaS y recibir un token de seguridad de Azure Active Directory.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/10/2020
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: 1c769cdac870c7384495d41158bd7ad516608575
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128632963"
---
# <a name="register-a-saas-application"></a>Registro de una aplicación SaaS

En este artículo se explica cómo registrar una aplicación SaaS mediante [Azure Portal](https://portal.azure.com/) de Microsoft y cómo obtener el token de acceso del editor (token de acceso de Azure Active Directory). El editor usará este token para autenticar la aplicación SaaS mediante una llamada a las API de suministro de SaaS.  Las API de suministro usan las credenciales de cliente de OAuth 2.0 para conceder flujo en los puntos de conexión de Azure Active Directory (versión 1.0) con el fin de crear una solicitud de token de acceso de servicio a servicio.

Azure Marketplace no impone ninguna restricción en el método de autenticación que el servicio SaaS use para los usuarios finales. El flujo siguiente solo es necesario para autenticar el servicio de SaaS en Azure Marketplace.

Para obtener más información sobre Azure AD (Active Directory), vea [¿Qué es la autenticación?](../../active-directory/develop/authentication-vs-authorization.md)

## <a name="register-an-azure-ad-secured-app"></a>Registrar una aplicación con protección de Azure AD

Cualquier aplicación que quiera usar las funciones de Azure AD debe registrarse primero en un inquilino de Azure AD. Este proceso de registro implica proporcionar a Azure AD algunos detalles sobre la aplicación. Para registrar una aplicación nueva mediante Azure Portal, realice los pasos siguientes:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Si la cuenta proporciona acceso a más de uno, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
3. En el panel de navegación izquierdo, haga clic en el servicio **Azure Active Directory**, haga clic en **Registros de aplicaciones** y haga clic en **Nuevo registro de aplicación**.

    ![Registros de aplicaciones SaaS de AD](./media/saas-offer-app-registration-v1.png)

4. En la página Crear, escriba la información de registro de la aplicación:
    -   **Name**: Especifique un nombre de aplicación significativo.
    -   **Tipo de aplicación**:  
        
        Seleccione **Aplicación web/API** para las [aplicaciones cliente](../../active-directory/develop/developer-glossary.md#client-application) y las [aplicaciones de recursos/API](../../active-directory/develop/developer-glossary.md#resource-server) que están instaladas en un servidor seguro. Esta configuración se usa para [clientes web](../../active-directory/develop/developer-glossary.md#web-client) confidenciales de OAuth y [clientes basados en agente de usuario](../../active-directory/develop/developer-glossary.md#user-agent-based-client) públicos.
        La misma aplicación también puede exponer tanto un cliente como un recurso o API.

        Para obtener ejemplos específicos de aplicaciones web, consulte las guías de inicio rápido de configuraciones que están disponibles en la sección [Introducción](../../active-directory/develop/quickstart-create-new-tenant.md) de la [Guía de desarrolladores de Azure AD](../../active-directory/develop/index.yml).

5. Cuando termine, haga clic en **Registrar**.  Azure AD asigna un *identificador de aplicación* único a la nueva aplicación. Se recomienda registrar una aplicación que acceda solo a la API y como inquilino único.

6. Para crear un secreto de cliente, vaya a la página **Certificados y secretos** y haga clic en **+ Nuevo secreto de cliente**.  Asegúrese de copiar el valor del secreto para usarlo en el código.

El **identificador de aplicación de Azure AD** está asociado al identificador del editor, por lo que debe asegurarse de que en todas las ofertas se use el mismo *identificador de aplicación*.

>[!Note]
>Si el publicador tiene dos o más cuentas diferentes en el Centro de partners, los detalles del registro de aplicación de Azure AD solo se pueden usar en una cuenta. No se admite el uso del mismo par de id. de inquilino e id. de la aplicación para una oferta en una cuenta de publicador diferente.

## <a name="how-to-get-the-publishers-authorization-token"></a>Cómo obtener el token de autorización del editor

Una vez que haya registrado la aplicación, puede solicitar mediante programación el token de autorización del editor (token de acceso de Azure AD, mediante el punto de conexión V1 de Azure AD). El editor debe usar este token al llamar a las distintas API de suministro de SaaS. Este token es válido durante una hora únicamente. 

Para más información sobre estos tokens, consulte [Tokens de acceso de Azure Active Directory](../../active-directory/develop/access-tokens.md).  Tenga en cuenta que, en el flujo siguiente, se usa el token del punto de conexión V1.

### <a name="get-the-token-with-an-http-post"></a>Obtención del token con una solicitud HTTP POST

#### <a name="http-method"></a>Método HTTP

Post<br>

##### <a name="request-url"></a>*Request URL* (URL de la solicitud) 

`https://login.microsoftonline.com/*{tenantId}*/oauth2/token`

##### <a name="uri-parameter"></a>*Parámetro de URI*

|  Nombre de parámetro    |  Obligatorio         |  Descripción |
|  ---------------   |  ---------------  | ------------ |
|  `tenantId`        |  True      |  Identificador de inquilino de la aplicación de AAD registrada. |

##### <a name="request-header"></a>*Encabezado de solicitud*

|  Nombre de encabezado       |  Obligatorio         |  Descripción |
|  ---------------   |  ---------------  | ------------ |
|  `content-type`    |  True      |  Tipo de contenido asociado a la solicitud. El valor predeterminado es `application/x-www-form-urlencoded`. |

##### <a name="request-body"></a>*Cuerpo de la solicitud*

|  Nombre de propiedad     |  Obligatorio         |  Descripción |
|  ---------------   |  ---------------  | ------------ |
|  `grant_type`      |  True      |  Tipo de concesión. Mediante `"client_credentials"`. |
|  `client_id`       |  True      |  Identificador de cliente o aplicación asociado a la aplicación de Azure AD. |
|  `client_secret`   |  True      |  Secreto asociado a la aplicación de Azure AD. |
|  `resource`        |  True      |  Recurso de destino para el que se solicita el token. Use `20e940b3-4c77-4b0b-9a53-9e16a1b010a7` porque la API de SaaS de Marketplace es siempre el recurso de destino en este caso. |

##### <a name="response"></a>*Respuesta*

|  Nombre     |  Tipo         |  Descripción |
|  ------   |  ---------------  | ------------ |
|  200 OK   |  TokenResponse    |  La solicitud se realizó correctamente. |

##### <a name="tokenresponse"></a>*TokenResponse*

Respuesta de ejemplo:

```json
{
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "20e940b3-4c77-4b0b-9a53-9e16a1b010a7",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }
```

| Elemento | Descripción |
| ------- | ----------- |
| `access_token` | Este elemento es el `<access_token>` (token de acceso) que pasará como parámetro de autorización al llamar a todas las API de suministro de SaaS y de medición de Marketplace. Al llamar a una API de REST protegida, el token se inserta en el campo `Authorization` del encabezado de la solicitud como un token de "portador", lo que permite a la API autenticar el llamador. | 
| `expires_in` | El número de segundos que el token de acceso sigue siendo válido, antes de expirar, desde la hora de emisión. La hora de emisión puede encontrarse en la notificación `iat` del token. |
| `expires_on` | La hora a la que expira el token de acceso. La fecha se representa como el número de segundos desde "1970-01-01T0:0:0Z UTC" (se corresponde con la notificación `exp` del token). |
| `not_before` | El intervalo de tiempo cuando el token de acceso tiene efecto y se puede aceptar. La fecha se representa como el número de segundos desde "1970-01-01T0:0:0Z UTC" (se corresponde con la notificación `nbf` del token). |
| `resource` | El recurso para el que se solicitó el token de acceso, que coincide con el parámetro de la cadena de consulta `resource` de la solicitud. |
| `token_type` | El tipo de token, que es un token de acceso al "Portador", lo que significa que el recurso puede permitir el acceso al portador del token. |

## <a name="next-steps"></a>Pasos siguientes

La aplicación protegida por Azure AD ahora puede usar la [API de suministro de SaaS versión 2](./pc-saas-fulfillment-api-v2.md).
