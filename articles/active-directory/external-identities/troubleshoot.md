---
title: 'Solución de problemas de colaboración B2B: Azure Active Directory | Microsoft Docs'
description: Recursos para solucionar problemas comunes con la colaboración B2B de Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: troubleshooting
ms.date: 07/13/2021
tags: active-directory
ms.author: mimart
author: msmimart
ms.custom:
- it-pro
- seo-update-azuread-jan"
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc88bd47c568483f7b175d84d8173b0daf3cb2f5
ms.sourcegitcommit: 37cc33d25f2daea40b6158a8a56b08641bca0a43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130070439"
---
# <a name="troubleshooting-azure-active-directory-b2b-collaboration"></a>Solución de problemas de colaboración B2B de Azure Active Directory

Estos son algunos de los recursos para solucionar problemas comunes relacionados con la colaboración B2B de Azure Active Directory (Azure AD).

   > [!IMPORTANT]
   >
   > - **A partir del 12 de julio de 2021**, si los clientes de Azure AD B2B configuran nuevas integraciones de Google para usarlas con registro de autoservicio para sus aplicaciones personalizadas o de línea de negocio, la autenticación con identidades de Google no funcionará hasta que las autenticaciones se trasladen a las vistas web del sistema. [Más información](google-federation.md#deprecation-of-web-view-sign-in-support).
   > - **A partir del 30 de septiembre de 2021,** Google [retira la compatibilidad con el inicio de sesión en la vista web insertada](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html). Si sus aplicaciones autentican a los usuarios con una vista web insertada y va a usar la federación de Google con [Azure AD B2C](../../active-directory-b2c/identity-provider-google.md) o Azure AD B2B para las [invitaciones de usuarios externos](google-federation.md) o el [registro de autoservicio](identity-providers.md), los usuarios de Google Gmail no podrán autenticarse. [Más información](google-federation.md#deprecation-of-web-view-sign-in-support).
   > - **A partir del 1 de noviembre de 2021**, Microsoft dejará de admitir el canje de invitaciones mediante la creación de cuentas e inquilinos de Azure AD no administrados para escenarios de colaboración B2B. En ese momento, comenzaremos a implementar un cambio para activar la característica de código de acceso de un solo uso por correo electrónico para todos los inquilinos existentes y habilitarla de manera predeterminada para los nuevos inquilinos. Si no desea permitir que esta característica se active automáticamente, puede [deshabilitarla](one-time-passcode.md#disable-email-one-time-passcode).

## <a name="ive-added-an-external-user-but-do-not-see-them-in-my-global-address-book-or-in-the-people-picker"></a>He agregado un usuario externo, pero no lo veo en mi libreta de direcciones global o en el selector de personas

En los casos donde los usuarios externos no se rellenan en la lista, el objeto puede tardar unos minutos en replicarse.

## <a name="a-b2b-guest-user-is-not-showing-up-in-sharepoint-onlineonedrive-people-picker"></a>Un usuario invitado de B2B no aparece en el selector de personas de SharePoint Online/OneDrive

La posibilidad de buscar usuarios invitados existentes en el selector de personas de SharePoint Online (SPO) está desactivada de manera predeterminada para así coincidir con el comportamiento heredado.

Para habilitarla, use la configuración "ShowPeoplePickerSuggestionsForGuestUsers" en el nivel de inquilino y colección de sitios. Para configurarla, use los cmdlets Set-SPOTenant y Set-SPOSite, que permiten que los miembros busquen a todos los usuarios invitados existentes en el directorio. Los cambios en el ámbito del inquilino no afectan los sitios de SPO que ya se aprovisionaron.

## <a name="my-guest-invite-settings-and-domain-restrictions-arent-being-respected-by-sharepoint-onlineonedrive"></a>Ni SharePoint Online ni OneDrive respetan la configuración de invitación de invitados ni las restricciones de dominio.

De forma predeterminada, SharePoint Online y OneDrive tienen su propio conjunto de opciones de usuario externo y no usan la configuración de Azure AD.  Para asegurarse de que las opciones sean coherentes entre esas aplicaciones, debe habilitar la [integración de SharePoint y OneDrive con Azure AD B2B](/sharepoint/sharepoint-azureb2b-integration-preview).
## <a name="invitations-have-been-disabled-for-directory"></a>Las invitaciones se han deshabilitado para el directorio

Si recibe una notificación de que no tiene permisos para invitar a usuarios, vaya a Azure Active Directory > Configuración de usuario > Usuarios externos > Administrar la configuración de la colaboración externa y compruebe que la cuenta de usuario está autorizada para invitar a usuarios externos.

![Captura de pantalla que muestra la configuración de los usuarios externos](media/troubleshoot/external-user-settings.png)

Si hace poco ha modificado estos valores o asignado el rol de invitador invitado a un usuario, podría haber un retraso de 15 a 60 minutos para que los cambios surtan efecto.

## <a name="the-user-that-i-invited-is-receiving-an-error-during-redemption"></a>El usuario al que he invitado está recibiendo un error durante el canje

Estos son algunos de los errores comunes:

### <a name="invitees-admin-has-disallowed-emailverified-users-from-being-created-in-their-tenant"></a>El administrador del invitado no permite que los usuarios de EmailVerified se creen en su inquilino

Cuando se invita a usuarios cuya organización usa Azure Active Directory, pero donde no existe la cuenta de usuario en concreto (por ejemplo, el usuario no existe en Azure AD contoso.com). El administrador de contoso.com puede tener una directiva en lugar de evitar que se creen usuarios. El usuario debe consultar al administrador y determinar si se permiten los usuarios externos. Es posible que el administrador del usuario externo necesite permitir usuarios comprobados por correo electrónico en el dominio (consulte este [artículo](/powershell/module/msonline/set-msolcompanysettings) sobre cómo permitir usuarios comprobados por correo electrónico).

![Error que indica que el inquilino no permite usuarios comprobado por correo electrónico](media/troubleshoot/allow-email-verified-users.png)

### <a name="external-user-does-not-exist-already-in-a-federated-domain"></a>El usuario externo no se encuentra ya en un dominio federado

Si va a usar la autenticación de federación y el usuario no existe en Azure Active Directory, no se puede invitar al usuario.

Para resolver este problema, el administrador del usuario externo debe sincronizar la cuenta del usuario con Azure Active Directory.

### <a name="external-user-has-a-proxyaddress-that-conflicts-with-a-proxyaddress-of-an-existing-local-user"></a>El usuario externo tiene una dirección proxy que entra en conflicto con la dirección proxy de un usuario local existente

Cuando comprobamos si se puede invitar a un usuario a su inquilino, una de las cosas que miramos es si se producirá una colisión en la dirección proxy. Esto incluye cualquier dirección proxy para el usuario en su propio inquilino principal, así como cualquier dirección proxy para los usuarios locales en el inquilino de usted. En el caso de los usuarios externos, agregaremos el correo electrónico a la dirección proxy del usuario B2B existente. En el caso de los usuarios locales, puede pedirles que inicien sesión con la cuenta que ya tienen.

## <a name="i-cant-invite-an-email-address-because-of-a-conflict-in-proxyaddresses"></a>No puedo invitar a una dirección de correo electrónico debido a un conflicto en las direcciones proxy

Esto sucede cuando otro objeto del directorio tiene la misma dirección de correo electrónico invitada que una de sus direcciones proxy. Para corregir este conflicto, quite el correo electrónico del objeto de [usuario](/graph/api/resources/user?view=graph-rest-1.0&preserve-view=true) y elimine también el objeto de [contacto](/graph/api/resources/contact?view=graph-rest-1.0&preserve-view=true) asociado antes de intentar invitar a este correo electrónico de nuevo.

## <a name="the-guest-user-object-doesnt-have-a-proxyaddress"></a>El objeto de usuario invitado no tiene una dirección proxy

Al invitar a un usuario invitado externo, se puede producir un conflicto con un [objeto de contacto](/graph/api/resources/contact?view=graph-rest-1.0&preserve-view=true) existente. Cuando esto sucede, el usuario invitado se crea sin dirección proxy. Esto significa que el usuario no podrá canjear esta cuenta mediante el [canje Just-In-Time](redemption-experience.md#redemption-through-a-direct-link) o la [autenticación de código de acceso de un solo uso por correo electrónico](one-time-passcode.md#user-experience-for-one-time-passcode-guest-users).

## <a name="how-does--which-is-not-normally-a-valid-character-sync-with-azure-ad"></a>¿Cómo "\#", que habitualmente no es un carácter válido, se sincroniza con Azure AD?

"\#" es un carácter reservado en los UPN para la colaboración B2B de Azure AD o los usuarios externos, porque la cuenta de invitado user@contoso.com se convierte en user_contoso.com#EXT#@fabrikam.onmicrosoft.com. Por lo tanto, los UPN con \# que proceden del entorno local no tienen permiso para iniciar sesión en Azure Portal. 

## <a name="i-receive-an-error-when-adding-external-users-to-a-synchronized-group"></a>Recibo un error al agregar los usuarios externos a un grupo sincronizado

Los usuarios externos pueden agregarse únicamente a los grupos "Seguridad" o "Asignado", y no a aquellos que se controlan localmente.

## <a name="my-external-user-did-not-receive-an-email-to-redeem"></a>Mi usuario externo no ha recibido un correo electrónico para realizar el canje

El invitado debe ponerse en contacto con su ISP o comprobar su filtro de correo no deseado para asegurarse de que se permite la siguiente dirección: Invites@microsoft.com

> [!NOTE]
>
> - Para el servicio de Azure operado por 21Vianet en China, la dirección del remitente es Invites@oe.21vianet.com.
> - Para la nube de Azure AD para el gobierno, la dirección del remitente es invites@azuread.us.

## <a name="i-notice-that-the-custom-message-does-not-get-included-with-invitation-messages-at-times"></a>Tenga en cuenta que, en ocasiones, el mensaje personalizado no se incluye en los mensajes de invitación

Para cumplir con las leyes de privacidad, nuestras API no incluye mensajes personalizados en la invitación por correo electrónico cuando:

- El invitador no tiene una dirección de correo electrónico en el inquilino que invita.
- Cuando una entidad de seguridad de App Service envía la invitación

Si este escenario es importante para usted, puede suprimir nuestro correo electrónico de invitación de API y enviarlo a través del mecanismo de correo electrónico de su elección. Consulte al asesor legal de su organización para asegurarse de que cualquier correo electrónico que envíe de esta forma también cumple las leyes de privacidad.

## <a name="you-receive-an-aadsts65005-error-when-you-try-to-log-in-to-an-azure-resource"></a>Se produce un error "AADSTS65005" al intentar iniciar sesión en un recurso de Azure

Un usuario con una cuenta de invitado no puede iniciar sesión y recibe el mensaje de error siguiente:

```plaintext
    AADSTS65005: Using application 'AppName' is currently not supported for your organization contoso.com because it is in an unmanaged state. An administrator needs to claim ownership of the company by DNS validation of contoso.com before the application AppName can be provisioned.
```

El usuario tiene una cuenta de usuario de Azure y es un inquilino viral no administrado o que se ha abandonado. Además, no hay administradores globales en el inquilino.

Para resolver este problema, debe asumir el inquilino abandonado. Consulte [Adquisición de un directorio no administrado como administrador en Azure Active Directory](../enterprise-users/domains-admin-takeover.md). También debe tener acceso al DNS accesible desde Internet para el sufijo de dominio en cuestión con el fin de proporcionar pruebas directas de que controla el espacio de nombres. Después de que el inquilino se devuelva a un estado administrado, hable con el cliente para ver si abandonar a los usuarios y al nombre de dominio comprobado es la mejor opción para su organización.

## <a name="a-guest-user-with-a-just-in-time-or-viral-tenant-is-unable-to-reset-their-password"></a>Un usuario invitado con un inquilino Just-In-Time o "viral" no puede restablecer su contraseña

Si el inquilino de la identidad es un inquilino Just-In-Time (JIT) o un inquilino "viral" (es decir, un inquilino de Azure que es independiente y no está administrado), solamente el usuario invitado podrá restablecer su contraseña. A veces, una organización [asumirá la administración de los inquilinos virales](../enterprise-users/domains-admin-takeover.md) que se crean cuando los empleados usan sus direcciones de correo electrónico del trabajo para registrarse en los servicios. Después de que la organización se haga cargo de un inquilino viral, solo un administrador de dicha organización puede restablecer la contraseña del usuario o habilitar SSPR. Si es necesario, como la organización que invita, puede quitar la cuenta del usuario invitado del directorio y volver a enviar una invitación.

## <a name="a-guest-user-is-unable-to-use-the-azuread-powershell-v1-module"></a>Un usuario invitado no puede usar el módulo de PowerShell v1 de Azure AD.

A partir del 18 de noviembre de 2019, los usuarios invitados del directorio (definidos como cuentas de usuario en las que la propiedad **userType** es igual a **Invitado**) no pueden usar el módulo de PowerShell v1 de Azure AD. En el futuro, un usuario deberá ser un usuario miembro (aquellos en los que **userType** es igual a **Miembro**) o usar el módulo de PowerShell V2 de Azure AD.

## <a name="in-an-azure-us-government-tenant-i-cant-invite-a-b2b-collaboration-guest-user"></a>En un inquilino de Azure Gobierno de EE. UU., no se puede invitar a un usuario invitado de colaboración B2B

Dentro de la nube de Azure Gobierno de EE. UU., la colaboración B2B solo se admite actualmente entre los inquilinos que se encuentran ambos dentro de la nube de Azure Gobierno de EE. UU. y que admiten ambos la colaboración B2B. Si invita a un usuario de un inquilino que no forma parte de la nube de Azure Gobierno de EE. UU. o que todavía no admite la colaboración B2B, se producirá un error. Para más información sobre limitaciones, consulte las [variaciones P1 y P2 de Azure Active Directory Premium](../../azure-government/compare-azure-government-global-azure.md#azure-active-directory-premium-p1-and-p2).

## <a name="i-receive-the-error-that-azure-ad-cannot-find-the-aad-extensions-app-in-my-tenant"></a>Aparece un error que indica que Azure AD no puede encontrar la aplicación aad-extensions-app en mi inquilino

Al usar las características de registro de autoservicio, como los flujos de usuario o los atributos de usuario personalizados, se crea automáticamente una aplicación llamada `aad-extensions-app. Do not modify. Used by AAD for storing user data.`. Lo utilizan las identidades externas de Azure AD para almacenar información acerca de los usuarios que se registran y los atributos personalizados recopilados.

Si ha eliminado la aplicación `aad-extensions-app` accidentalmente, tiene 30 días para recuperarla. Puede restaurar la aplicación mediante el módulo de PowerShell de Azure AD.

1. Inicie el módulo de PowerShell de Azure AD y ejecute `Connect-AzureAD`.
1. Inicie sesión como administrador global del inquilino de Azure AD para el que desea restaurar la aplicación eliminada.
1. Ejecute el comando de PowerShell `Get-AzureADDeletedApplication`.
1. Busque la aplicación en la lista cuyo nombre para mostrar comienza por `aad-extensions-app` y copie el valor de la propiedad `ObjectId`.
1. Ejecute el comando de PowerShell `Restore-AzureADDeletedApplication -ObjectId {id}`. Reemplace la parte `{id}` del comando por el valor de `ObjectId` del paso anterior.

Ahora debería ver la aplicación restaurada en Azure Portal.

## <a name="a-guest-user-was-invited-successfully-but-the-email-attribute-is-not-populating"></a>Se ha invitado correctamente a un usuario, pero el atributo de correo electrónico no se rellena

Supongamos que ha invitado accidentalmente a un usuario con una dirección de correo electrónico que coincide con un objeto de usuario que ya está en el directorio. Se ha creado el objeto de usuario invitado, pero la dirección de correo electrónico se ha agregado a la propiedad `otherMail`, en lugar de a las propiedades `mail` o `proxyAddresses`. Para evitar este problema, puede buscar objetos de usuario en conflicto en el directorio de Azure AD mediante estos pasos de PowerShell:

1. Abra el módulo de PowerShell de Azure AD y ejecute `Connect-AzureAD`.
1. Inicie sesión como administrador global del inquilino de Azure AD en el que quiere comprobar los objetos de contacto duplicados.
1. Ejecute el comando de PowerShell `Get-AzureADContact -All $true | ? {$_.ProxyAddresses -match 'user@domain.com'}`.
1. Ejecute el comando de PowerShell `Get-AzureADContact -All $true | ? {$_.Mail -match 'user@domain.com'}`.

## <a name="next-steps"></a>Pasos siguientes

[Obtención de soporte técnico para la colaboración B2B](../fundamentals/active-directory-troubleshooting-support-howto.md)
