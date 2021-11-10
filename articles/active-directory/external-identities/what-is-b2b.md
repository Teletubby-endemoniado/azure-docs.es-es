---
title: ¿Qué es la colaboración B2B en Azure Active Directory?
description: La colaboración de Azure Active Directory B2B admite el acceso de usuarios invitados, para poder compartir recursos de forma segura y colaborar con asociados externos.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 10/21/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12d37973813fe2eee4972d2063d40df9e46b9b08
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130222483"
---
# <a name="what-is-guest-user-access-in-azure-active-directory-b2b"></a>¿Qué es el acceso de usuarios invitados en Azure Active Directory B2B?

La colaboración de negocio a negocio (B2B) de Azure Active Directory (Azure AD) es una característica de External Identities que le permite invitar a los usuarios invitados a colaborar con una organización. La colaboración B2B le permite compartir de forma segura las aplicaciones y los servicios de una empresa con los usuarios invitados de cualquier otra organización, al tiempo que mantiene el control sobre sus propios datos corporativos. Trabaje de forma segura con asociados externos, grandes o pequeños, incluso si no tienen Azure AD o un departamento de TI. Un proceso de invitación y canje sencillo permite a los asociados usar sus propias credenciales para acceder a los recursos de su empresa. Los desarrolladores pueden usar las API de negocio a negocio de Azure AD para personalizar el proceso de invitación o escribir aplicaciones como los portales de suscripción de autoservicio. Para obtener información sobre las licencias y los precios relacionados con los usuarios invitados, consulte [Precios de Azure Active Directory for External Identities](https://azure.microsoft.com/pricing/details/active-directory/external-identities/).  

> [!IMPORTANT]
>
> - **A partir del 12 de julio de 2021**, si los clientes de Azure AD B2B configuran nuevas integraciones de Google para usarlas con registro de autoservicio para sus aplicaciones personalizadas o de línea de negocio, la autenticación con identidades de Google no funcionará hasta que las autenticaciones se trasladen a las vistas web del sistema. [Más información](google-federation.md#deprecation-of-web-view-sign-in-support).
> - **A partir del 30 de septiembre de 2021,** Google [retira la compatibilidad con el inicio de sesión en la vista web insertada](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html). Si sus aplicaciones autentican a los usuarios con una vista web insertada y va a usar la federación de Google con [Azure AD B2C](../../active-directory-b2c/identity-provider-google.md) o Azure AD B2B para las [invitaciones de usuarios externos](google-federation.md) o el [registro de autoservicio](identity-providers.md), los usuarios de Google Gmail no podrán autenticarse. [Más información](google-federation.md#deprecation-of-web-view-sign-in-support).
> - **A partir del 1 de noviembre de 2021**, comenzaremos a implementar un cambio para activar la característica de código de acceso de un solo uso por correo electrónico para todos los inquilinos existentes y habilitarla de manera predeterminada para los nuevos inquilinos. Como parte de este cambio, Microsoft dejará de crear cuentas e inquilinos nuevos y no administrados ("virales") de Azure AD durante el canje de invitación de colaboración B2B. Para minimizar las interrupciones durante las vacaciones y los bloqueos de implementación, la mayoría de los inquilinos verán los cambios implementados en enero de 2022. Vamos a habilitar la característica de código de acceso de un solo uso por correo electrónico, ya que proporciona un método eficaz de autenticación de reserva para usuarios invitados. Pero si no quiere permitir que esta característica se active automáticamente, puede [deshabilitarla](one-time-passcode.md#disable-email-one-time-passcode).

## <a name="collaborate-with-any-partner-using-their-identities"></a>Colaboración con cualquier asociado mediante sus identidades

Con Azure AD B2B, el asociado utiliza su propia solución de administración de identidades, así que no hay ninguna sobrecarga administrativa externa para su organización. Los usuarios invitados inician sesión en las aplicaciones y los servicios con sus propias identidades profesionales, educativas o sociales.

- El socio utiliza sus propias identidades y credenciales; Azure AD no es necesario.
- No es necesario administrar las cuentas externas o las contraseñas.
- No es necesario sincronizar las cuentas o administrar los ciclos de vida de las cuentas.  

## <a name="easily-invite-guest-users-from-the-azure-ad-portal"></a>Facilidad de la invitación a los usuarios invitados desde el portal de Azure AD

Como administrador, puede agregar fácilmente usuarios invitados a su organización en Azure Portal.

- [Cree un usuario invitado](b2b-quickstart-add-guest-users-portal.md) en Azure AD, de forma similar a cómo se agrega un nuevo usuario.
- Asigne los usuarios invitados a las aplicaciones o los grupos.
- Envíe una invitación por correo electrónico que contenga un vínculo de canje o enviar un vínculo directo a una aplicación que desee compartir.

![Captura de pantalla que muestra la página de entrada de invitación Nuevo usuario invitado](media/what-is-b2b/add-a-b2b-user-to-azure-portal.png)

- Los usuarios invitados siguen unos [pasos de canje](redemption-experience.md) sencillos para iniciar sesión.

![Captura de pantalla que muestra la página Revisar permisos](media/what-is-b2b/consentscreen.png)


## <a name="use-policies-to-securely-share-your-apps-and-services"></a>Uso de directivas para compartir de forma segura sus aplicaciones y servicios

Puede usar las directivas de autorización para proteger el contenido corporativo. Las directivas de acceso condicional, como la autenticación multifactor, se pueden aplicar:

- En el nivel de inquilino.
- En el nivel de aplicación.
- A usuarios invitados específicos para proteger los datos y las aplicaciones empresariales.

![Captura de pantalla que muestra la opción Acceso condicional](media/what-is-b2b/tutorial-mfa-policy-2.png)



## <a name="let-application-and-group-owners-manage-their-own-guest-users"></a>Permitir que los propietarios de aplicaciones y grupos administren sus propios usuarios invitados

Puede delegar la administración de usuarios invitados a los propietarios de aplicaciones para que puedan agregar usuarios invitados directamente a cualquier aplicación que deseen compartir, ya sea una aplicación de Microsoft o no.

- Los administradores configuran la administración de aplicaciones y grupos de autoservicio.
- Los usuarios no administradores usan su [panel de acceso](https://myapps.microsoft.com) para agregar usuarios invitados a aplicaciones o grupos.

![Captura de pantalla que muestra el Panel de acceso de los usuarios invitados](media/what-is-b2b/access-panel-manage-app.png)

## <a name="customize-the-onboarding-experience-for-b2b-guest-users"></a>Personalización de la experiencia de incorporación de usuarios invitados de B2B

Incorpore a los asociados externos de manera personalizada según las necesidades de su organización.

- Use la [administración de derechos de Azure AD](../governance/entitlement-management-overview.md) para configurar directivas que [administren el acceso para los usuarios externos](../governance/entitlement-management-external-users.md#how-access-works-for-external-users).
- Use la [API de invitación de colaboración B2B](/graph/api/resources/invitation) para personalizar las experiencias de incorporación.

## <a name="integrate-with-identity-providers"></a>Integración con proveedores de identidades

Azure AD admite proveedores de identidades externos (como Facebook), cuentas de Microsoft, Google o proveedores de identidades empresariales. Puede configurar la federación con proveedores de identidades para que los usuarios externos puedan iniciar sesión con sus cuentas empresariales o sociales existentes en lugar de crear una nueva cuenta solo para la aplicación. Más información sobre los [proveedores de identidades para External Identities](identity-providers.md).

![Captura de pantalla que muestra la página Proveedores de identidades](media/what-is-b2b/identity-providers.png)


## <a name="create-a-self-service-sign-up-user-flow"></a>Creación de un flujo de usuario de registro de autoservicio

Con un flujo de usuario de registro de autoservicio, puede crear una experiencia de registro para los usuarios externos que quieran tener acceso a sus aplicaciones. Como parte del flujo de registro, puede proporcionar opciones para diferentes proveedores de identidades sociales o empresariales y recopilar información sobre el usuario. Obtenga información sobre el [registro de autoservicio y cómo configurarlo](self-service-sign-up-overview.md).

También puede usar [conectores de API](api-connectors-overview.md) para integrar los flujos de usuarios de registro de autoservicio con sistemas en la nube externos. Puede conectarse con flujos de trabajo de aprobación personalizados, realizar la comprobación de identidades, validar la información proporcionada por el usuario, etc.

![Captura de pantalla que muestra la página de flujos de usuario](media/what-is-b2b/self-service-sign-up-user-flow-overview.png)
<!--TODO: Add screenshot with API connectors -->

## <a name="next-steps"></a>Pasos siguientes

- [Precios de identidades externas](external-identities-pricing.md)
- [Agregar usuarios invitados de colaboración B2B en el portal](add-users-administrator.md)
- [Información sobre el proceso de canje de invitaciones](redemption-experience.md)
