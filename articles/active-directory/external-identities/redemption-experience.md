---
title: 'Canje de invitación en colaboración B2B: Azure AD'
description: Describe la experiencia de canje de invitación de colaboración B2B de Azure AD para los usuarios finales, incluido el acuerdo con los términos de privacidad.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 10/21/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.collection: M365-identity-device-management
ms.openlocfilehash: a3d20d621b648fb84f2cedee08f206d6eb257f51
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130227723"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Experiencia de invitación de colaboración B2B de Azure Active Directory

En este artículo se describen las maneras en las que los usuarios invitados pueden acceder a los recursos y el proceso de consentimiento que se encuentran. Si envía un correo electrónico de invitación al invitado, la invitación incluye un vínculo que este puede canjear para acceder a la aplicación o al portal. El correo electrónico de invitación es solo uno de los modos de acceso de los invitados a los recursos. Como alternativa, puede agregar invitados a su directorio y proporcionarles un vínculo directo al portal o a la aplicación que desee compartir. Independientemente del método que usen, la primera vez se les guiará por un proceso de consentimiento. Este proceso garantiza que los invitados acepten los términos de privacidad y los [términos de uso](../conditional-access/terms-of-use.md) que haya configurado.

Al agregar un usuario invitado al directorio, la cuenta de este tiene un estado de consentimiento (visible en PowerShell) que se establece inicialmente en **PendingAcceptance**. Esta configuración permanece hasta que el invitado acepta la invitación, la política de privacidad y los términos de uso. Después de eso, el estado de consentimiento cambia a **Accepted** y las páginas de consentimiento dejan de aparecer para el invitado.

> [!IMPORTANT]
> - **A partir del 12 de julio de 2021**, si los clientes de Azure AD B2B configuran nuevas integraciones de Google para usarlas con registro de autoservicio para sus aplicaciones personalizadas o de línea de negocio, la autenticación con identidades de Google no funcionará hasta que las autenticaciones se trasladen a las vistas web del sistema. [Más información](google-federation.md#deprecation-of-web-view-sign-in-support).
> - **A partir del 30 de septiembre de 2021,** Google [retira la compatibilidad con el inicio de sesión en la vista web insertada](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html). Si sus aplicaciones autentican a los usuarios con una vista web insertada y va a usar la federación de Google con [Azure AD B2C](../../active-directory-b2c/identity-provider-google.md) o Azure AD B2B para las [invitaciones de usuarios externos](google-federation.md) o el [registro de autoservicio](identity-providers.md), los usuarios de Google Gmail no podrán autenticarse. [Más información](google-federation.md#deprecation-of-web-view-sign-in-support).
> - **A partir del 1 de noviembre de 2021**, comenzaremos a implementar un cambio para activar la característica de código de acceso de un solo uso por correo electrónico para todos los inquilinos existentes y habilitarla de manera predeterminada para los nuevos inquilinos. Como parte de este cambio, Microsoft dejará de crear cuentas e inquilinos nuevos y no administrados ("virales") de Azure AD durante el canje de invitación de colaboración B2B. Para minimizar las interrupciones durante las vacaciones y los bloqueos de implementación, la mayoría de los inquilinos verán los cambios implementados en enero de 2022. Vamos a habilitar la característica de código de acceso de un solo uso por correo electrónico, ya que proporciona un método eficaz de autenticación de reserva para usuarios invitados. Pero si no quiere permitir que esta característica se active automáticamente, puede [deshabilitarla](one-time-passcode.md#disable-email-one-time-passcode). 

## <a name="redemption-and-sign-in-through-a-common-endpoint"></a>Canje e inicio de sesión mediante un punto de conexión común

Ahora, los usuarios invitados pueden iniciar sesión en las aplicaciones multiinquilino o las aplicaciones propias de Microsoft mediante un punto de conexión común (una dirección URL); por ejemplo, `https://myapps.microsoft.com`. Anteriormente, si se utilizaba una dirección URL común, el usuario invitado accedía automáticamente al inquilino principal en lugar de al inquilino del recursos para autenticarse, por lo que se necesitaba un vínculo específico del inquilino (por ejemplo, `https://myapps.microsoft.com/?tenantid=<tenant id>`). Ahora, el usuario invitado puede ir a la dirección URL común de la aplicación, elegir **Opciones de inicio de sesión** y seleccionar **Sign in to an organization** (Iniciar sesión en una organización). A continuación, el usuario tiene que escribir el nombre de la organización.

![Inicio de sesión con un punto de conexión común](media/redemption-experience/common-endpoint-flow-small.png)

Después, el usuario accede automáticamente al punto de conexión del inquilino, donde puede iniciar sesión con su dirección de correo electrónico o seleccionar un proveedor de identidades que haya configurado.

## <a name="redemption-through-a-direct-link"></a>Canje a través de un vínculo directo

En lugar de la invitación por correo electrónico o la dirección URL común de la aplicación, también puede proporcionar al invitado un vínculo directo a la aplicación o al portal. Primero debe agregar el usuario invitado a su directorio mediante [Azure Portal](./b2b-quickstart-add-guest-users-portal.md) o [PowerShell](./b2b-quickstart-invite-powershell.md). Puede usar cualquiera de las [maneras personalizables para implementar las aplicaciones para los usuarios](../manage-apps/end-user-experiences.md), incluidos los vínculos de inicio de sesión en directo. Cuando un invitado usa un vínculo directo en lugar de la invitación por correo electrónico, también se le guía por la experiencia de consentimiento inicial.

> [!NOTE]
> Los vínculos directos son específicos del inquilino. En otras palabras, contienen un identificador del inquilino o un dominio verificado para que el invitado puede autenticarse en el inquilino donde se encuentra la aplicación compartida. Estos son algunos ejemplos de vínculos directos con el contexto del inquilino:
 > - Panel de acceso de aplicaciones: `https://myapps.microsoft.com/?tenantid=<tenant id>`
 > - Panel de acceso de aplicaciones para un dominio comprobado: `https://myapps.microsoft.com/<;verified domain>`
 > - Azure Portal: `https://portal.azure.com/<tenant id>`
 > - Aplicación individual: consulte cómo usar un [vínculo de inicio de sesión en directo](../manage-apps/end-user-experiences.md#direct-sign-on-links)

En algunos casos se recomienda el correo electrónico de invitación en lugar del vínculo directo. Si estos casos especiales son importantes para su organización, se recomienda que invite a los usuarios mediante métodos que aún envíen el correo electrónico de invitación:
 - El usuario no tiene una cuenta de Azure AD, MSA o de correo electrónico en una organización federada. A menos que use la característica de código de acceso de un solo uso, el invitado deberá canjear la invitación por correo electrónico para que se le guíe para crear una MSA.
 - A veces, el objeto de usuario invitado puede no tener una dirección de correo electrónico debido a un conflicto con un objeto de contacto (por ejemplo, un objeto de contacto de Outlook). En este caso, el usuario debe hacer clic en la dirección URL de canje en el correo electrónico de invitación.
 - El usuario puede iniciar sesión con un alias de la dirección de correo electrónico a la que se invitó. (Un alias es una dirección de correo electrónico adicional asociada con una cuenta de correo electrónico). En este caso, el usuario debe hacer clic en la dirección URL de canje en el correo electrónico de invitación.

## <a name="redemption-through-the-invitation-email"></a>Canje a través del correo electrónico de invitación

Al agregar un usuario invitado al directorio [mediante Azure Portal](./b2b-quickstart-add-guest-users-portal.md), se envía un correo electrónico de invitación al invitado. También puede enviar correos electrónicos de invitación cuando [use PowerShell](./b2b-quickstart-invite-powershell.md) para agregar usuarios invitados al directorio. Esta es una descripción de la experiencia del invitado cuando canjea el vínculo del correo electrónico.

1. El invitado recibe un [correo electrónico de invitación](./invitation-email-elements.md) que se envía desde **Microsoft Invitations**.
2. El invitado selecciona **Aceptar invitación** en el correo electrónico.
3. El invitado usará sus propias credenciales para iniciar sesión en el directorio. Si el invitado no tiene una cuenta que se pueda federar al directorio y la característica [código de acceso de un solo uso (OTP) de correo electrónico](./one-time-passcode.md) no está habilitada, se solicita al invitado que cree una cuenta [MSA](https://support.microsoft.com/help/4026324/microsoft-account-how-to-create) personal o una [cuenta de autoservicio de Azure AD](../enterprise-users/directory-self-service-signup.md). Consulte [Flujo de canje de invitación](#invitation-redemption-flow) para más información.
4. Al invitado se le guiará por la [experiencia de consentimiento](#consent-experience-for-the-guest) que se describe a continuación.

## <a name="redemption-limitation-with-conflicting-contact-object"></a>Limitación de canje con el objeto Contact en conflicto
A veces, el correo electrónico del usuario invitado externo puede entrar en conflicto con un objeto [Contact](/graph/api/resources/contact?view=graph-rest-1.0&preserve-view=true) existente, lo que da lugar a que el usuario invitado se cree sin una propiedad proxyAddress. Se trata de una limitación conocida que impide que los usuarios invitados hagan lo siguiente: 
- Canjear una invitación a través de un vínculo directo mediante [IdP de SAML/WS-Fed](./direct-federation.md), [cuentas de Microsoft](./microsoft-account.md), [federación de Google](./google-federation.md) o cuentas de [código de acceso de un solo uso de correo electrónico](./one-time-passcode.md). 
- Canjear una invitación a través de un vínculo de canje de correo electrónico de invitación mediante [IdP de SAML/WS-Fed](./direct-federation.md) y cuentas de [código de acceso de un solo uso de correo electrónico](./one-time-passcode.md).
- Volver a iniciar sesión en una aplicación después del canje mediante cuentas de [IdP de SAML/WS-Fed](./direct-federation.md) y de [federación de Google](./google-federation.md).

Para desbloquear a los usuarios que no pueden canjear una invitación debido a un objeto [Contact](/graph/api/resources/contact?view=graph-rest-1.0&preserve-view=true) en conflicto, siga estos pasos:
1. Elimine el objeto Contact en conflicto.
2. Elimine el usuario invitado en Azure Portal (la propiedad "Invitación aceptada" del usuario debe estar en un estado pendiente).
3. Vuelva a invitar al usuario externo.
4. Espere a que el usuario canjee la invitación.
5. Vuelva a agregar el correo electrónico de contacto del usuario en Exchange y cualquier DL de la que debe formar parte.

## <a name="invitation-redemption-flow"></a>Flujo del canje de invitación

Cuando un usuario hace clic en el vínculo **Aceptar invitación** de un [correo electrónico de invitación](invitation-email-elements.md), Azure AD canjea automáticamente la invitación basándose en el flujo de canje, del modo que se muestra a continuación:

![Captura de pantalla que muestra el diagrama de flujo del canje](media/redemption-experience/invitation-redemption-flow.png)

**Si el nombre principal de usuario (UPN) coincide con una cuenta existente de Azure AD y de MSA personal, se le pedirá al usuario que elija la cuenta con la que desea realizar el canje.*

1. Azure AD realiza la detección basada en usuarios para determinar si el usuario existe en un [inquilino de Azure AD existente](./what-is-b2b.md#easily-invite-guest-users-from-the-azure-ad-portal).

2. Si un administrador ha habilitado la [federación de IdP de SAML/WS-Fed](./direct-federation.md), Azure AD comprueba si el sufijo de dominio del usuario coincide con el dominio de un proveedor de identidades SAML/WS-Fed configurado y redirige al usuario al proveedor de identidades configurado previamente.

3. Si un administrador ha habilitado la [federación de Google](./google-federation.md), Azure AD comprueba si el sufijo de dominio del usuario es gmail.com o googlemail.com y redirige al usuario a Google.

4. El proceso de canje comprueba si el usuario tiene una [cuenta Microsoft (MSA)](https://support.microsoft.com/help/4026324/microsoft-account-how-to-create) personal existente para canjes Just-In-Time (JIT), pero no para el canje de vínculos de correo electrónico de invitación. Si el usuario ya tiene una MSA existente, la usará para iniciar sesión.

5. Una vez identificado el **directorio principal** del usuario, el usuario se envía al proveedor de identidades correspondiente para iniciar sesión.  

6. Si en los pasos 1 a 4 no se encuentra el directorio principal del usuario al que se ha enviado la invitación, Azure AD determina si el inquilino que realiza la invitación tiene habilitada la característica de [código de acceso de un solo uso (OTP) de correo electrónico](./one-time-passcode.md) para los invitados.

7. Si [el código de acceso de un solo uso de correo electrónico para invitados está habilitado](./one-time-passcode.md#when-does-a-guest-user-get-a-one-time-passcode), se envía un código de acceso al usuario por medio del correo electrónico de invitación. El usuario recuperará el código de acceso y lo escribirá en la página de inicio de sesión de Azure AD.

8. Si el código de acceso de un solo uso de correo electrónico para invitados está deshabilitado, Azure AD comprueba el sufijo de dominio para determinar si pertenece a una cuenta de consumidor. Si es así, se pedirá al usuario que cree una [cuenta Microsoft](https://support.microsoft.com/help/4026324/microsoft-account-how-to-create) personal. En caso contrario, se pedirá al usuario que cree una [cuenta de autoservicio de Azure AD](../enterprise-users/directory-self-service-signup.md).

9. Azure AD intenta crear una [cuenta de autoservicio de Azure AD](../enterprise-users/directory-self-service-signup.md) mediante la verificación del acceso al correo electrónico. Para realizar la comprobación de la cuenta, se envía un código al correo electrónico, código que el usuario debe recuperar y enviar a Azure AD. Sin embargo, si el inquilino del usuario al que se ha enviado la invitación está federado o si el campo AllowEmailVerifiedUsers está establecido en false en el inquilino del usuario invitado, este no puede completar el canje y el flujo da como resultado un error. Para más información, consulte [Solución de problemas de colaboración de Azure Active Directory B2B](./troubleshoot.md#the-user-that-i-invited-is-receiving-an-error-during-redemption).

10. Se solicita al usuario que cree una [cuenta Microsoft (MSA)](https://support.microsoft.com/help/4026324/microsoft-account-how-to-create) personal.

11. Después de autenticarse en el proveedor de identidades correcto, se redirige al usuario a Azure AD para completar la [experiencia de consentimiento](#consent-experience-for-the-guest).  

En el caso de canjes Just-in-Time (JIT), en los que el canje se realiza por medio de un vínculo de aplicación con inquilinos, no están disponibles los pasos del 8 al 10. Si un usuario alcanza el paso 6 y la característica de código de acceso de un solo uso de correo electrónico no está habilitada, recibe un mensaje de error y no puede canjear la invitación. Para evitar este error, los administradores deben [habilitar el código de acceso de un solo uso de correo electrónico](./one-time-passcode.md#when-does-a-guest-user-get-a-one-time-passcode) o asegurarse de que el usuario haga clic en un vínculo de invitación.

## <a name="consent-experience-for-the-guest"></a>Experiencia de consentimiento para el invitado

Cuando un invitado iniciar sesión por primera vez para acceder a los recursos de una organización asociada, se le guía por las siguientes páginas. 

1. El invitado debe revisar la página **Revisar permisos**, donde se describe la declaración de privacidad de la organización anfitriona. Para poder continuar, el usuario debe **Aceptar** el uso de su información de acuerdo con las directivas de privacidad de la organización anfitriona.

   ![Captura de pantalla que muestra la página Revisar permisos](media/redemption-experience/review-permissions.png) 

   > [!NOTE]
   > Para obtener información sobre cómo puede, como administrador de inquilinos, vincular con la declaración de privacidad de su organización, vea [Procedimiento: Incorporación de información de privacidad de su organización en Azure Active Directory](../fundamentals/active-directory-properties-area.md).

2. Si se han configurado términos de uso, el invitado debe abrirlos y revisarlos y seleccionar **Aceptar**. 

   ![Captura de pantalla en la que se muestran los nuevos términos de uso](media/redemption-experience/terms-of-use-accept.png) 

   Puede configurar los [Términos de uso](../conditional-access/terms-of-use.md) en **External Identities** > **Términos de uso**.

3. A menos que se especifique otra cosa, al invitado se le redirige al panel de acceso a las aplicaciones, que enumera las aplicaciones a las que puede acceder.

   ![Captura de pantalla que muestra el panel de acceso a las aplicaciones](media/redemption-experience/myapps.png) 

> [!NOTE]
> La experiencia de consentimiento solo aparece después de que el usuario inicia sesión y no antes. Hay algunos escenarios en los que la experiencia de consentimiento no se mostrará al usuario, por ejemplo:
> - El usuario ya aceptó la experiencia de consentimiento
> - El administrador [concede el consentimiento del administrador para todo el inquilino a una aplicación](../manage-apps/grant-admin-consent.md)

En su directorio, el valor de **Invitación aceptada** cambia a **Sí**. Si se creó una MSA, el **Origen** del usuario muestra **Cuenta Microsoft**. Para más información sobre las propiedades de la cuenta de usuario invitado, consulte [Propiedades de un usuario de colaboración B2B de Azure Active Directory](user-properties.md). Si ve un error que requiere el consentimiento del administrador al acceder a una aplicación, consulte [Cómo conceder consentimiento de administrador a las aplicaciones](../develop/v2-admin-consent.md).

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es la colaboración B2B de Azure AD?](what-is-b2b.md)
- [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](add-users-administrator.md)
- [¿Cómo agregan los trabajadores de la información usuarios de colaboración B2B a Azure Active Directory?](add-users-information-worker.md)
- [Incorporación de usuarios de colaboración B2B de Azure Active Directory con PowerShell](customize-invitation-api.md#powershell)
- [Salir de una organización como usuario invitado](leave-the-organization.md)