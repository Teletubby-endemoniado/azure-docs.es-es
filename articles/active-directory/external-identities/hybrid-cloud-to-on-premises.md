---
title: 'Concesión a los usuarios B2B acceso a las aplicaciones locales: Azure AD'
description: Se muestra cómo proporcionar a los usuarios B2B de la nube acceso a aplicaciones locales con la colaboración B2B de Azure AD.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 11/05/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ebaa3c9b6a931a01eb65a28cd48cf29e74c13b3
ms.sourcegitcommit: 591ffa464618b8bb3c6caec49a0aa9c91aa5e882
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2021
ms.locfileid: "131892407"
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>Conceder a los usuarios B2B de Azure AD acceso a las aplicaciones locales

Las organizaciones que usan las funcionalidades de colaboración B2B de Azure Active Directory (Azure AD) para invitar a los usuarios invitados de organizaciones asociadas a su instancia de Azure AD, ahora pueden proporcionar a estos usuarios B2B acceso a las aplicaciones locales. Estas aplicaciones locales pueden usar la autenticación basada en SAML o la autenticación integrada de Windows (IWA) con la delegación limitada de Kerberos (KCD).

## <a name="access-to-saml-apps"></a>Acceso a las aplicaciones SAML

Si la aplicación local usa la autenticación basada en SAML, estas aplicaciones pueden estar fácilmente disponibles para los usuarios de colaboración de B2B de Azure AD mediante Azure Portal con Azure AD Application Proxy.

Debe realizar las acciones siguientes:

- Habilite Application Proxy e instale un conector. Para instrucciones, consulte [Publicación de aplicaciones mediante Azure AD Application Proxy](../app-proxy/application-proxy-add-on-premises-application.md).
- Publique la aplicación basada en SAML en el entorno local a través de Azure AD Application Proxy; para ello, siga las instrucciones de [Inicio de sesión único de SAML para aplicaciones en el entorno local con Application Proxy](../app-proxy/application-proxy-configure-single-sign-on-on-premises-apps.md).
- Asigne usuarios de B2B de Azure AD a la aplicación SAML.

Una vez completados los pasos anteriores, la aplicación debería estar en funcionamiento. Para probar el acceso B2B de Azure AD:
1.  Abra un explorador y vaya a la dirección URL externa que creó al publicar la aplicación.
2.  Inicie sesión con la cuenta de B2B de Azure AD que asignó a la aplicación. Debería poder abrir la aplicación y acceder a ella con el inicio de sesión único.

## <a name="access-to-iwa-and-kcd-apps"></a>Acceso a aplicaciones IWA y KCD

Para proporcionar a los usuarios B2B acceso a las aplicaciones locales que están protegidas con la autenticación integrada de Windows y la delegación restringida de Kerberos, necesita los siguientes componentes:

- **Autenticación mediante Azure AD Application Proxy**. Los usuarios B2B deben poder autenticarse en la aplicación local. Para ello, debe publicar la aplicación local a través de Azure AD Application Proxy. Para más información, consulte el [Tutorial: Adición de una aplicación local para el acceso remoto mediante Application Proxy](../app-proxy/application-proxy-add-on-premises-application.md).
- **Autorización mediante un objeto de usuario B2B en el directorio local**. La aplicación debe poder realizar comprobaciones de acceso de usuario y conceder acceso a los recursos correctos. IWA y KCD requieren un objeto de usuario en Windows Server Active Directory local para realizar esta autorización. Como se describe en [Cómo funciona el inicio de sesión único con KCD](../app-proxy/application-proxy-configure-single-sign-on-with-kcd.md#how-single-sign-on-with-kcd-works), Application Proxy necesita este objeto de usuario para suplantar al usuario y obtener un token de Kerberos para la aplicación. 

   > [!NOTE]
   > Al configurar Application Proxy de Azure AD, asegúrese de que la opción **Identidad de inicio de sesión delegada** esté establecida en **Nombre principal del usuario** (valor predeterminado) en la configuración de autenticación integrada de Windows (IWA).

   En el escenario de usuario B2B, hay dos métodos que se pueden usar para crear los objetos de usuario invitado que son necesarios para la autorización en el directorio local:

   - Microsoft Identity Manager (MIM) y el agente de administración de MIM para Microsoft Graph.
   - Un script de PowerShell, que es una solución más ligera que no requiere MIM.

En el siguiente diagrama se proporciona información general de alto nivel de cómo funcionan juntos Azure AD Application Proxy y la generación del objeto de usuario B2B en el directorio local para conceder a los usuarios B2B acceso a sus aplicaciones locales IWA y KCD. Los pasos numerados se describen en detalle más adelante en el diagrama.

![Diagrama de soluciones de MIM y script B2B](media/hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  Se invita a un usuario de una organización asociada (el inquilino de Fabrikam) al inquilino de Contoso.
2.  Se crea un objeto de usuario invitado en el inquilino de Contoso (por ejemplo, un objeto de usuario con un UPN de guest_fabrikam.com#EXT#@contoso.onmicrosoft.com).
3.  El invitado de Fabrikam se importa desde Contoso mediante MIM o el script de PowerShell para B2B.
4.  Se crea una representación o "huella" del objeto de usuario invitado de Fabrikam (Guest#EXT#) en el directorio local, Contoso.com, mediante MIM o el script de PowerShell para B2B.
5.  El usuario invitado accede a la aplicación local, app.contoso.com.
6.  La solicitud de autenticación se autoriza mediante Application Proxy, por medio de la delegación restringida de Kerberos. 
7.  Dado que el objeto de usuario invitado existe localmente, la autenticación se realiza correctamente.

### <a name="lifecycle-management-policies"></a>Directivas de administración del ciclo de vida

Puede administrar los objetos de usuario B2B locales mediante directivas de administración del ciclo de vida. Por ejemplo:

- Puede configurar directivas de autenticación multifactor (MFA) para el usuario invitado para usar MFA durante la autenticación de Application Proxy. Para más información, consulte [Acceso condicional para usuarios de colaboración B2B](conditional-access.md).
- Cualquier patrocinio, revisión de acceso, verificación de cuenta, etc. que se realice sobre el usuario B2B en la nube se aplica a los usuarios locales. Por ejemplo, si se elimina el usuario en la nube mediante las directivas de administración del ciclo de vida, también se elimina el usuario local mediante la sincronización MIM o la sincronización de Azure AD Connect. Para más información, consulte [Administración del acceso de los invitados con las revisiones de acceso de Azure AD](../governance/manage-guest-access-with-access-reviews.md).

### <a name="create-b2b-guest-user-objects-through-mim"></a>Creación de objetos de usuario invitado B2B mediante MIM

Si necesita información acerca de cómo usar MIM 2016 Service Pack 1 y el agente de administración de MIM para Microsoft Graph a fin de crear los objetos del usuario invitado en el directorio local, consulte [Colaboración de negocio a negocio (B2B) de Azure AD con Microsoft Identity Manager(MIM) 2016 SP1 con el proxy de aplicación de Azure](/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario).

## <a name="license-considerations"></a>Consideraciones sobre licencias

Asegúrese de que dispone de las licencias de acceso cliente (CAL) correctas para los usuarios invitados externos que acceden a las aplicaciones locales. Para más información, consulte la sección "External Connector" de [Licencias de acceso de cliente y licencias de administración](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx). Hable con su representante o distribuidor local de Microsoft para informarse sobre sus necesidades concretas de licencia.

## <a name="next-steps"></a>Pasos siguientes

- Consulte también [Colaboración B2B de Azure Active Directory para organizaciones híbridas](hybrid-organizations.md).

- Para información general sobre Azure AD Connect, consulte [Integración de los directorios locales con Azure Active Directory](../hybrid/whatis-hybrid-identity.md).
