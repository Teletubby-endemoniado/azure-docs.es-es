---
title: Creación de la página de aterrizaje de su oferta de SaaS comercializable en el marketplace comercial
description: Obtenga información sobre cómo crear una página de aterrizaje para su oferta de SaaS comercializable.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 10/25/2021
ms.openlocfilehash: 8a0e7ab7fad8b0da025f143ab2401e5740972ec7
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131078263"
---
# <a name="build-the-landing-page-for-your-transactable-saas-offer-in-the-commercial-marketplace"></a>Creación de la página de aterrizaje de su oferta de SaaS comercializable en el marketplace comercial

Este artículo le guía por el proceso de creación de una página de aterrizaje para una aplicación de SaaS comercializable que se venderá en el marketplace comercial de Microsoft.

## <a name="overview"></a>Información general

Puede pensar en la página de aterrizaje como el "recibidor" de la oferta de software como servicio (SaaS). Después de que el comprador se suscriba a una oferta, el marketplace comercial le dirigirá a la página de aterrizaje para activar y configurar su suscripción a la aplicación de SaaS. Piense en esto como un paso de confirmación de pedido que permite al comprador ver lo que ha comprado y confirmar los detalles de su cuenta. Con Azure Active Directory (Azure AD) y Microsoft Graph, habilitará el inicio de sesión único (SSO) para el comprador y obtendrá detalles importantes sobre el comprador que puede usar para confirmar y activar su suscripción, incluidos su nombre, dirección de correo electrónico y organización.

Dado que la información necesaria para activar la suscripción es limitada y la proporcionan Azure AD y Microsoft Graph, no debería haber ninguna necesidad de solicitar información que requiera más que el consentimiento básico. Si necesita detalles de usuario que requieran un consentimiento adicional para la aplicación, debe solicitar esta información una vez completada la activación de la suscripción. Esto permite la activación de la suscripción sin problemas para el comprador y reduce el riesgo de abandono.

La página de aterrizaje suele incluir:

- Presente el nombre de la oferta y el plan adquirido, así como los términos de facturación.
- Presente los detalles de la cuenta del comprador, incluidos el nombre y los apellidos, la organización y el correo electrónico.
- Pida al comprador que confirme o sustituya los distintos detalles de la cuenta.
- Guíe al comprador en los pasos siguientes después de la activación. Por ejemplo, recibir un correo electrónico de bienvenida, administrar la suscripción, obtener soporte técnico o leer la documentación.

> [!NOTE]
> También se dirigirá al comprador a la página de aterrizaje al administrar su suscripción después de la activación. Después de activar la suscripción del comprador, debe usar el SSO para permitir que el usuario inicie sesión. Se recomienda dirigir al usuario a un perfil de cuenta o a una página de configuración.

Las siguientes secciones le guiarán en el proceso de creación de una página de aterrizaje:

1. [Cree un registro de aplicaciones de Azure AD](#create-an-azure-ad-app-registration) para la página de aterrizaje.
1. [Use un ejemplo de código como punto de partida](#use-a-code-sample-as-a-starting-point) para su aplicación.
1. [Use dos aplicaciones de Azure AD para mejorar la seguridad en producción](#use-two-azure-ad-apps-to-improve-security-in-production).
1. [Resuelva el token de identificación de compra de Marketplace](#resolve-the-marketplace-purchase-identification-token) agregado a la dirección URL por el marketplace comercial.
1. [Consulte la información de las notificaciones codificadas en el token de identificación](#read-information-from-claims-encoded-in-the-id-token) que se recibió de Azure AD después del inicio de sesión y que se envió con la solicitud.
1. [Use la API de Microsoft Graph](#use-the-microsoft-graph-api) para recopilar información adicional, según sea necesario.

## <a name="create-an-azure-ad-app-registration"></a>Creación de un registro de aplicación de Azure AD

El marketplace comercial está totalmente integrado con Azure AD. Los compradores llegan al marketplace autenticados con una [cuenta de Azure AD o una cuenta de Microsoft (MSA)](../active-directory/fundamentals/active-directory-whatis.md#terminology). Después de la compra, el comprador va del marketplace comercial a la dirección URL de la página de aterrizaje para activar y administrar su suscripción de su aplicación de SaaS. Debe permitir que el comprador inicie sesión en la aplicación con el SSO de Azure AD. (La dirección URL de la página de aterrizaje se especifica en la página [Configuración técnica](plan-saas-offer.md#technical-information) de la oferta.)

[!INCLUDE [pound-sign-note](./includes/pound-sign-note.md)]

El primer paso para usar la identidad es asegurarse de que la página de aterrizaje esté registrada como aplicación de Azure AD. Registrar la aplicación le permite usar Azure AD para autenticar usuarios y solicitar acceso a recursos de usuarios. Se puede considerar la definición de la aplicación, lo que permite que el servicio sepa cómo emitir tokens para la aplicación en función de la configuración de la aplicación.

### <a name="register-a-new-application-using-the-azure-portal"></a>Registro de una aplicación nueva mediante Azure Portal

Para empezar, siga las instrucciones para [registrar una nueva aplicación](../active-directory/develop/quickstart-register-app.md). Para permitir que los usuarios de otras empresas visiten la aplicación, debe elegir una de las opciones de multiinquilino cuando se le pregunte quién puede usar la aplicación.

Si tiene previsto consultar la API de Microsoft Graph, [configure la nueva aplicación para acceder a las API web](../active-directory/develop/quickstart-configure-app-access-web-apis.md). Al seleccionar los permisos de API para esta aplicación, el valor predeterminado **User.Read** es suficiente para recopilar información básica sobre el comprador para que el proceso de incorporación sea automático y sin problemas. No solicite ningún permiso de la API con la etiqueta **necesita el consentimiento del administrador**, ya que esto impedirá que todos los usuarios que no sean administradores visiten la página de aterrizaje.

Si requiere permisos elevados como parte del proceso de incorporación o aprovisionamiento, considere la posibilidad de usar la funcionalidad de [consentimiento incremental](../active-directory/azuread-dev/azure-ad-endpoint-comparison.md) de Azure AD para que todos los compradores enviados desde el Marketplace puedan interactuar inicialmente con la página de aterrizaje.

## <a name="use-a-code-sample-as-a-starting-point"></a>Uso de un ejemplo de código como punto de partida

Hemos proporcionado varias aplicaciones de ejemplo que implementan un sitio web sencillo con el inicio de sesión de Azure AD habilitado. Una vez registrada la aplicación en Azure AD, la hoja **Inicio rápido** ofrece una lista de tipos de aplicaciones y pilas de desarrollo habituales, tal como se muestra en la Figura 1. Elija el que coincida con su entorno y siga las instrucciones para descargarlo y configurarlo.

***Figura 1: Hoja de inicio rápido en Azure Portal***

:::image type="content" source="./media/azure-ad-saas/azure-ad-quickstart-blade.png" alt-text="Muestra la hoja de inicio rápido en Azure Portal.":::

Después de descargar el código y configurar el entorno de desarrollo, cambie los valores de configuración de la aplicación para que reflejen el identificador de la aplicación, el identificador del inquilino y el secreto de cliente que anotó en el procedimiento anterior. Tenga en cuenta que los pasos exactos variarán en función del ejemplo que use.

## <a name="use-two-azure-ad-apps-to-improve-security-in-production"></a>Uso de dos aplicaciones de Azure AD para mejorar la seguridad en producción

En este artículo se presenta una versión simplificada de la arquitectura para implementar una página de aterrizaje para su oferta de SaaS del marketplace comercial. Al ejecutar la página en producción, se recomienda mejorar la seguridad mediante la comunicación con las API de cumplimiento de SaaS solo a través de una aplicación diferente y protegida. Esto requiere la creación de dos aplicaciones nuevas:

- En primer lugar, la aplicación de la página de aterrizaje multiinquilino que se ha descrito hasta este punto, excepto sin la funcionalidad para ponerse en contacto con las API de cumplimiento de SaaS. Esta funcionalidad se descargará en otra aplicación, como se describe a continuación.
- En segundo lugar, una aplicación para poseer las comunicaciones con las API de cumplimiento de SaaS. Esta aplicación debe ser de un solo inquilino, solo para su uso por parte de la organización, y se puede establecer una lista de control de acceso para limitar el acceso a las API solo desde esta aplicación.

Esto permite que la solución funcione en escenarios que tengan en cuenta el principio de [separación de preocupaciones](/dotnet/architecture/modern-web-apps-azure/architectural-principles#separation-of-concerns). Por ejemplo, la página de aterrizaje usa la primera aplicación de Azure AD registrada para que el usuario inicie sesión. Una vez que el usuario ha iniciado sesión, la página de aterrizaje usa la segunda aplicación de Azure AD para solicitar un token de acceso para llamar a las API de cumplimiento de SaaS y llamar a la operación de resolución.

## <a name="resolve-the-marketplace-purchase-identification-token"></a>Resolución del token de identificación de la compra de Marketplace

Cuando se envía al comprador a la página de aterrizaje, se agrega un token al parámetro de dirección URL. Este token es diferente del token emitido por Azure AD y del token de acceso que se usa para la autenticación entre servicios, y se usa como entrada para la llamada de resolución de la [API de cumplimiento de SaaS](./partner-center-portal/pc-saas-fulfillment-api-v2.md#resolve-a-purchased-subscription) para obtener los detalles de la suscripción. Como con todas las llamadas a las API de cumplimiento de SaaS, la solicitud de servicio a servicio se autenticará con un token de acceso que se basa en el usuario de identificador de la aplicación de Azure AD de la aplicación para la autenticación de servicio a servicio.

> [!NOTE]
> En la mayoría de los casos, es preferible hacer esta llamada desde una segunda aplicación de un único inquilino. Consulte [Uso de dos aplicaciones de Azure AD para mejorar la seguridad en producción](#use-two-azure-ad-apps-to-improve-security-in-production) más adelante en este artículo.

### <a name="request-an-access-token"></a>Solicitud de un token de acceso

Para autenticar la aplicación con las API de cumplimiento de SaaS, necesita un token de acceso, que se puede generar mediante una llamada al punto de conexión de OAuth de Azure AD. Consulte [Cómo obtener el token de autorización del editor](./partner-center-portal/pc-saas-registration.md#how-to-get-the-publishers-authorization-token).

### <a name="call-the-resolve-endpoint"></a>Llamada al punto de conexión de resolución

Las API de cumplimiento de SaaS implementan el punto de conexión de [resolución](./partner-center-portal/pc-saas-fulfillment-api-v2.md#resolve-a-purchased-subscription) al que se puede llamar para confirmar la validez del token de Marketplace y devolver información acerca de la suscripción.

## <a name="read-information-from-claims-encoded-in-the-id-token"></a>Lectura de la información de las notificaciones codificadas en el token de identificador

Como parte del flujo de [OpenID Connect](../active-directory/develop/v2-protocols-oidc.md), Azure AD agrega un [token de identificador](../active-directory/develop/id-tokens.md) a la solicitud cuando se envía al comprador a la página de aterrizaje. Este token contiene varios elementos de información básica que podrían ser útiles en el proceso de activación, incluida la información que se muestra en esta tabla.

| Value | Descripción |
| ------------ | ------------- |
| aud | Audiencia objetivo para este token. En este caso, debe coincidir con el identificador de la aplicación y debe validadarse. |
| preferred_username | Nombre de usuario principal del usuario visitante. Puede ser una dirección de correo electrónico, un número de teléfono u otro identificador. |
| email | Dirección de correo electrónico del usuario. Tenga en cuenta que este campo puede estar vacío. |
| name | Valor en lenguaje natural que identifica al firmante del token. En este caso, será el nombre del comprador. |
| oid | Identificador en el sistema de identidades de Microsoft que identifica de forma única al usuario entre aplicaciones. Microsoft Graph devuelve este valor como la propiedad Id. de una cuenta de usuario determinada. |
| tid | Identificador que representa el inquilino de Azure AD de donde proviene el comprador. En el caso de una identidad de MSA, siempre será ``9188040d-6c67-4c5b-b112-36a304b66dad``. Para obtener más información, consulte la nota de la siguiente sección: Uso de la API de Microsoft Graph. |
| sub | Identificador que identifica de forma única al usuario en esta aplicación específica. |
|||

## <a name="use-the-microsoft-graph-api"></a>Uso de Microsoft Graph API

El token de identificador contiene información básica para identificar al comprador, pero el proceso de activación puede requerir detalles adicionales, como la empresa del comprador, para completar el proceso de incorporación. Use la [API de Microsoft Graph](/graph/use-the-api) para solicitar esta información para evitar que el usuario vuelva a escribir estos detalles. Los permisos **User.Read** estándar incluyen la siguiente información, de forma predeterminada.

| Value | Descripción |
| ------------ | ------------- |
| DisplayName | Nombre que se muestra en la libreta de direcciones para el usuario. |
| givenName | Nombre del usuario. |
| jobTitle | Cargo del usuario. |
| mail | Dirección SMTP del usuario. |
| mobilePhone | Número de teléfono móvil principal del usuario. |
| preferredLanguage | Código ISO 639-1 del idioma preferido del usuario. |
| surname | Apellidos del usuario. |
|||

Se pueden seleccionar propiedades adicionales, como el nombre de la empresa del usuario o la ubicación (país) del usuario, para su inclusión en la solicitud. Consulte las [propiedades del tipo de recurso de usuario](/graph/api/resources/user#properties) para obtener más detalles.

La mayoría de las aplicaciones que se registran con Azure AD conceden permisos delegados para leer la información del usuario del inquilino de Azure AD de su empresa. Cualquier solicitud a Microsoft Graph de esta información debe ir acompañada de un token de acceso para la autenticación. Los pasos específicos para generar el token de acceso dependerán de la pila de tecnología que esté usando, pero el código de ejemplo contendrá un ejemplo. Para más información, consulte [Obtener acceso en nombre de un usuario](/graph/auth-v2-user).

> [!NOTE]
> Las cuentas del inquilino de MSA (con el identificador de inquilino ``9188040d-6c67-4c5b-b112-36a304b66dad``) no devolverán más información de la que ya se ha recopilado con el token de identificador. Por lo tanto, puede omitir esta llamada a Graph API para estas cuentas.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una oferta de SaaS en el marketplace comercial](create-new-saas-offer.md)