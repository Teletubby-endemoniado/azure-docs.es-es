---
title: Inicio de sesión de usuario basado en SMS para Azure Active Directory
description: Aprenda a configurar y permitir que los usuarios inicien sesión en Azure Active Directory mediante SMS
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/18/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: anjusingh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cf0cc0ebc60a5b401ac8bd9726e39d23d8504a0
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124766111"
---
# <a name="configure-and-enable-users-for-sms-based-authentication-using-azure-active-directory"></a>Configuración y habilitación de los usuarios para la autenticación basada en SMS mediante Azure Active Directory 

Para simplificar y proteger el inicio de sesión en aplicaciones y servicios, Azure Active Directory (Azure AD) proporciona varias opciones de autenticación. La autenticación basada en SMS permite a los usuarios iniciar sesión sin proporcionar, o inclusive sin saber, su nombre de usuario y contraseña. Después de que un administrador de identidades cree su cuenta, puede escribir su número de teléfono en la solicitud de inicio de sesión. Reciben un código de autenticación a través de un mensaje de texto que pueden proporcionar para completar el inicio de sesión. Este método de autenticación simplifica el acceso a las aplicaciones y los servicios, sobre todo para los trabajadores que están en primera línea.

En este artículo se muestra cómo habilitar la autenticación basada en SMS para usuarios o grupos seleccionados en Azure AD. Para ver una lista de las aplicaciones que admiten el uso del inicio de sesión basado en SMS, consulte [Compatibilidad con aplicaciones para la autenticación basada en SMS](how-to-authentication-sms-supported-apps.md).

## <a name="before-you-begin"></a>Antes de empezar

Para completar este artículo, necesitará los siguientes recursos y privilegios:

* Una suscripción de Azure activa.
    * Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un inquilino de Azure Active Directory asociado a la suscripción.
    * Si es necesario, [cree un inquilino de Azure Active Directory][create-azure-ad-tenant] o [asocie una suscripción a Azure con su cuenta][associate-azure-ad-tenant].
* Necesita privilegios de *administrador global* en el inquilino de Azure AD para permitir la autenticación basada en SMS.
* Cada usuario que esté habilitado en la directiva de métodos de autenticación de mensaje de texto debe tener licencia, aunque no la usen. Cada usuario habilitado debe tener una de las siguientes licencias de Azure AD, EMS o Microsoft 365:
    * [Microsoft 365 F1 o F3][m365-firstline-workers-licensing]
    * [Azure Active Directory Premium P1 o P2][azure-ad-pricing]
    * [Enterprise Mobility + Security (EMS) E3 o E5][ems-licensing] o [Microsoft 365 E3 o E5][m365-licensing]
    * [Office 365 F3][o365-f3]

## <a name="known-issues"></a>Problemas conocidos

A continuación se enumeran algunos problemas conocidos:

* La autenticación basada en SMS no es compatible actualmente con Multi-Factor Authentication de Azure AD.
* A excepción de los Teams, la autenticación basada en SMS no es compatible con las aplicaciones de Office nativas.
* No se recomienda la autenticación basada en SMS para las cuentas B2B.
* Los usuarios federados no se autenticarán en el inquilino principal. Solo se autentican en la nube.
* Si el método de inicio de sesión predeterminado de un usuario es un mensaje de texto o una llamada al número de teléfono, el código SMS o la llamada de voz se envía automáticamente durante la autenticación multifactor. A partir de junio de 2021, algunas aplicaciones pedirán a los usuarios que elijan **Texto** o **Llamada** primero. Esta opción evita el envío de demasiados códigos de seguridad para diferentes aplicaciones. Si el método de inicio de sesión predeterminado es la aplicación Microsoft Authenticator ([la cual recomendamos encarecidamente](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/it-s-time-to-hang-up-on-phone-transports-for-authentication/ba-p/1751752)), la notificación de la aplicación se enviará automáticamente.

## <a name="enable-the-sms-based-authentication-method"></a>Habilitación del método de autenticación basado en SMS

Para habilitar y usar la autenticación basada en SMS en su organización se necesitan tres pasos principales:

* Habilitar la directiva de métodos de autenticación.
* Seleccionar los usuarios o grupos que pueden usar el método de autenticación basado en SMS.
* Asignar un número de teléfono para cada cuenta de usuario.
    * Este número de teléfono se puede asignar en Azure Portal (que se muestra en este artículo) y en *Mi personal* o *Mi cuenta*.

En primer lugar, vamos a habilitar la autenticación basada en SMS para el inquilino de Azure AD.

1. Inicie sesión en [Azure Portal][azure-portal] como *administrador global*.
1. Busque y seleccione **Azure Active Directory**.
1. En el menú de navegación de la parte izquierda de la ventana de Azure Active Directory, seleccione **Seguridad > Métodos de autenticación > Directiva de métodos de autenticación**.

    [![Busque y seleccione la ventana de la directiva de métodos de autenticación en Azure Portal.](media/howto-authentication-sms-signin/authentication-method-policy-cropped.png)](media/howto-authentication-sms-signin/authentication-method-policy.png#lightbox)

1. En la lista de métodos de autenticación disponibles, seleccione **Mensaje de texto**.
1. Establezca **Habilitar** en *Sí*.

    ![Habilitación de la autenticación de texto en la ventana de la directiva de métodos de autenticación](./media/howto-authentication-sms-signin/enable-text-authentication-method.png)

    Puede optar por habilitar la autenticación basada en SMS para *Todos los usuarios* o *Seleccionar usuarios* y grupos. En la siguiente sección, habilitará la autenticación basada en SMS para un usuario de prueba.

## <a name="assign-the-authentication-method-to-users-and-groups"></a>Asignación del método de autenticación a usuarios y grupos

Con la autenticación basada en SMS habilitada en el inquilino de Azure AD, seleccione ahora algunos usuarios o grupos que van a poder usar este método de autenticación.

1. En la ventana de la directiva de autenticación de mensajes de texto, establezca **Destino** en *Seleccionar usuarios*.
1. Elija **Agregar usuarios o grupos** y, luego, seleccione *Contoso User* (Usuario de Contoso) o *Contoso SMS Users* (Usuarios de SMS de Contoso) como usuario o grupo de prueba.

    [![Seleccione los usuarios o grupos que quiere habilitar para la autenticación basada en SMS en Azure Portal.](media/howto-authentication-sms-signin/add-users-or-groups-cropped.png)](media/howto-authentication-sms-signin/add-users-or-groups.png#lightbox)

1. Cuando haya seleccionado los usuarios o grupos, elija **Seleccionar** y, luego, **Guardar** la directiva de métodos de autenticación actualizada.

Cada usuario que esté habilitado en la directiva de métodos de autenticación de mensaje de texto debe tener licencia, aunque no la usen. Asegúrese de tener las licencias adecuadas para los usuarios que habilita en la directiva de métodos de autenticación, en especial cuando se habilita la característica para grupos de usuarios de gran tamaño.

## <a name="set-a-phone-number-for-user-accounts"></a>Establecimiento de un número de teléfono para las cuentas de usuario

Los usuarios ya están habilitados para la autenticación basada en SMS, pero su número de teléfono debe estar asociado con el perfil de usuario en Azure AD antes de poder iniciar sesión. El usuario puede [establecer este número de teléfono por sí mismo](https://support.microsoft.com/account-billing/set-up-sms-sign-in-as-a-phone-verification-method-0aa5b3b3-a716-4ff2-b0d6-31d2bcfbac42) en *Mi cuenta*, o bien se puede asignar mediante Azure Portal. Los *administradores globales*, *administradores de autenticación* o *administradores de autenticación con privilegios* pueden establecer los números de teléfono.

Cuando se establece un número de teléfono para el inicio de sesión con SMS, también se puede usar con [Multi-Factor Authentication de Azure AD][tutorial-azure-mfa] y el [autoservicio de restablecimiento de contraseña][tutorial-sspr].

1. Busque y seleccione **Azure Active Directory**.
1. En el menú de navegación del lado izquierdo de la ventana de Azure Active Directory, seleccione **Usuarios**.
1. Seleccione el usuario que habilitó para la autenticación basada en SMS en la sección anterior, por ejemplo, *Contoso User* (Usuario de Contoso) y, luego, seleccione **Métodos de autenticación**.
1. Seleccione **+ Agregar método de autenticación** y, después, en el menú desplegable *Elegir método*, elija **Número de teléfono**.

    Escriba el número de teléfono del usuario, incluido el código de país, por ejemplo, *+1 xxxxxxxxx*. Azure Portal valida que el número de teléfono tenga el formato correcto.

    A continuación, en el menú desplegable *Tipo de teléfono*, seleccione *Móvil*, *Teléfono móvil alternativo* u *Otros* según sea necesario.

    :::image type="content" source="media/howto-authentication-sms-signin/set-user-phone-number.png" alt-text="Establecimiento de un número de teléfono para un usuario en Azure Portal para usar con la autenticación basada en SMS":::

    El número de teléfono debe ser único en el inquilino. Si intenta usar el mismo número de teléfono para varios usuarios, se muestra un mensaje de error.

1. Para aplicar el número de teléfono a la cuenta de un usuario, seleccione **Agregar**.

Cuando se aprovisiona correctamente, aparece una marca de verificación en *SMS Sign-in enabled* (Habilitado para el inicio de sesión de SMS).

## <a name="test-sms-based-sign-in"></a>Prueba del inicio de sesión basado en SMS

Para probar la cuenta de usuario que ahora está habilitada para el inicio de sesión basado en SMS, siga estos pasos:

1. Abra una nueva ventana del explorador web en modo de incógnito o InPrivate en [https://www.office.com][office].
1. En la esquina superior derecha, seleccione **Iniciar sesión**.
1. En la solicitud de inicio de sesión, escriba el número de teléfono asociado al usuario en la sección anterior y, luego, seleccione **Siguiente**.

    ![Especificación de un número de teléfono en la solicitud de inicio de sesión del usuario de prueba](./media/howto-authentication-sms-signin/sign-in-with-phone-number.png)

1. Se envía un mensaje de texto al número de teléfono proporcionado. Para completar el proceso de inicio de sesión, escriba el código de 6 dígitos proporcionado en el mensaje de texto en la solicitud de inicio de sesión.

    ![Especificación del código de confirmación enviado a través de un mensaje de texto al número de teléfono del usuario](./media/howto-authentication-sms-signin/sign-in-with-phone-number-confirmation-code.png)

1. El usuario ya ha iniciado sesión sin necesidad de proporcionar un nombre de usuario o una contraseña.


## <a name="troubleshoot-sms-based-sign-in"></a>Solución de problemas de inicio de sesión basado en SMS

Los siguientes escenarios y pasos de solución de problemas pueden usarse si tiene problemas para habilitar y usar el inicio de sesión basado en SMS. Para ver una lista de las aplicaciones que admiten el uso del inicio de sesión basado en SMS, consulte [Compatibilidad con aplicaciones para la autenticación basada en SMS](how-to-authentication-sms-supported-apps.md).


### <a name="phone-number-already-set-for-a-user-account"></a>Número de teléfono ya establecido para una cuenta de usuario

Si un usuario ya se ha registrado en Multi-Factor Authentication de Azure AD o el autoservicio de restablecimiento de contraseña (SSPR), ya tiene un número de teléfono asociado a su cuenta. Este número de teléfono no está disponible automáticamente para su uso con el inicio de sesión basado en SMS.

Los usuarios que tengan un número de teléfono ya establecido para su cuenta verán un botón *Enable for SMS sign-in* (Habilitar para el inicio de sesión único) en su página **Mi perfil**. Al seleccionar este botón, la cuenta se habilita para su uso con el inicio de sesión basado en SMS y el registro anterior en Multi-Factor Authentication de Azure AD o SSPR.

Para más información sobre la experiencia de usuario final, consulte [Experiencia de usuario de inicio de sesión de SMS para el número de teléfono](https://support.microsoft.com/account-billing/set-up-sms-sign-in-as-a-phone-verification-method-0aa5b3b3-a716-4ff2-b0d6-31d2bcfbac42).

### <a name="error-when-trying-to-set-a-phone-number-on-a-users-account"></a>Error al intentar establecer un número de teléfono en la cuenta de un usuario

Si recibe un error al intentar establecer un número de teléfono para una cuenta de usuario en Azure Portal, revise los siguientes pasos de solución de problemas:

1. Asegúrese de que está habilitado para el inicio de sesión basado en SMS.
1. Confirme que la cuenta de usuario está habilitada en la directiva de métodos de autenticación de *mensaje de texto*.
1. Asegúrese de establecer el número de teléfono con el formato adecuado, tal como se ha validado en Azure Portal (por ejemplo, *+ 1 4251234567*).
1. Asegúrese de que el número de teléfono no se usa en ningún otro lugar del inquilino.
1. Compruebe que no hay ningún número de voz establecido en la cuenta. Si hay establecido un número de voz, elimínelo y vuelva a probar con el número de teléfono.

## <a name="next-steps"></a>Pasos siguientes

- Para ver una lista de las aplicaciones que admiten el uso del inicio de sesión basado en SMS, consulte [Compatibilidad con aplicaciones para la autenticación basada en SMS](how-to-authentication-sms-supported-apps.md).
- Puede encontrar más formas de iniciar sesión en Azure AD sin una contraseña, como las claves de seguridad de la aplicación Microsoft Authenticator o FIDO2, en [Opciones de autenticación sin contraseñas de Azure AD][concepts-passwordless].
- También puede utilizar Microsoft Graph API REST para [habilitar][rest-enable] o [deshabilitar][rest-disable] el inicio de sesión basado en SMS.


<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../fundamentals/active-directory-how-subscriptions-associated-directory.md
[concepts-passwordless]: concept-authentication-passwordless.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[tutorial-sspr]: tutorial-enable-sspr.md
[rest-enable]: /graph/api/phoneauthenticationmethod-enablesmssignin?tabs=http
[rest-disable]: /graph/api/phoneauthenticationmethod-disablesmssignin?tabs=http

<!-- EXTERNAL LINKS -->
[azure-portal]: https://portal.azure.com
[office]: https://www.office.com
[m365-firstline-workers-licensing]: https://www.microsoft.com/licensing/news/m365-firstline-workers
[azuread-licensing]: https://azure.microsoft.com/pricing/details/active-directory/
[ems-licensing]: https://www.microsoft.com/microsoft-365/enterprise-mobility-security/compare-plans-and-pricing
[m365-licensing]: https://www.microsoft.com/microsoft-365/compare-microsoft-365-enterprise-plans
[o365-f1]: https://www.microsoft.com/microsoft-365/business/office-365-f1?market=af
[o365-f3]: https://www.microsoft.com/microsoft-365/business/office-365-f3?activetab=pivot%3aoverviewtab
[azure-ad-pricing]: https://www.microsoft.com/security/business/identity-access-management/azure-ad-pricing