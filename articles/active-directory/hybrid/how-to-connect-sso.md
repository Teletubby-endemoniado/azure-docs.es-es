---
title: 'Azure AD Connect: Inicio de sesión único de conexión directa | Microsoft Docs'
description: En este tema se describe el inicio de sesión único de conexión directa de Azure Active Directory (Azure AD) y cómo permite proporcionar un verdadero inicio de sesión único a los usuarios de equipos de escritorio corporativos dentro de la red de la empresa.
services: active-directory
keywords: qué es Azure AD Connect, instalar Active Directory, componentes necesarios para Azure AD, SSO, inicio de sesión único
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/13/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0cf360196b3fee73dc91ea7936f2a8faad2dedc5
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129231641"
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Inicio de sesión único de conexión directa de Azure Active Directory

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>¿Qué es el inicio de sesión único de conexión directa de Azure Active Directory?

El inicio de sesión único de conexión directa de Azure Active Directory (SSO de conexión directa de Azure AD) permite iniciar sesión automáticamente a los usuarios en dispositivos corporativos conectados a la red de la empresa. Si se habilita, los usuarios no tienen que escribir la contraseña para iniciar sesión en Azure AD y, generalmente, ni siquiera los nombres de usuario. Esta característica proporciona a los usuarios un acceso sencillo a las aplicaciones en la nube sin necesidad de componentes locales adicionales.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

SSO de conexión directa se puede combinar con los métodos de inicio de sesión mediante [sincronización de hash de contraseñas](how-to-connect-password-hash-synchronization.md) o [autenticación de paso a través](how-to-connect-pta.md). El inicio de sesión único de conexión directa _no_ se puede aplicar a Servicios de federación de Active Directory (AD FS).

![Inicio de sesión único de conexión directa](./media/how-to-connect-sso/sso1.png)

## <a name="sso-via-primary-refresh-token-vs-seamless-sso"></a>Inicio de sesión único mediante el token de actualización principal frente a inicio de sesión único de conexión directa

En Windows 10, Windows Server 2016 y versiones posteriores, se recomienda usar el inicio de sesión único mediante el token de actualización principal (PRT). En Windows 7 y Windows 8.1, se recomienda usar el inicio de sesión único de conexión directa.
Para el inicio de sesión único de conexión directa, es necesario que el dispositivo del usuario esté unido a un dominio, pero este inicio de sesión no se usa en [dispositivos unidos a Azure AD](../devices/concept-azure-ad-join.md) ni en [dispositivos unidos a Azure AD híbrido](../devices/concept-azure-ad-join-hybrid.md). El inicio de sesión único en dispositivos unidos a Azure AD, unidos a Azure AD híbrido y registrados en Azure AD funciona según el [token de actualización principal (PRT)](../devices/concept-primary-refresh-token.md)

El inicio de sesión único mediante PRT funciona una vez que los dispositivos se registran con Azure AD en el caso de los dispositivos unidos a Azure AD híbrido, unidos a Azure AD o dispositivos personales registrados mediante Agregar cuenta profesional o educativa. Para más información sobre cómo funciona el inicio de sesión único con Windows 10 mediante PRT, consulte: [Token de actualización principal (PRT) y Azure AD](../devices/concept-primary-refresh-token.md).


## <a name="key-benefits"></a>Ventajas principales

- *Mejor experiencia del usuario*
  - Los usuarios inician sesión automáticamente tanto en las aplicaciones basadas en la nube como en las locales.
  - Los usuarios no tienen que escribir sus contraseñas varias veces.
- *Es fácil de implementar y administrar*
  - No se necesitan componentes adicionales en local para que esto funcione.
  - Funciona con cualquier método de autenticación en la nube, ya sea [sincronización de hash de contraseña](how-to-connect-password-hash-synchronization.md) o [autenticación de paso a través](how-to-connect-pta.md).
  - Puede extenderse a algunos o a todos los usuarios mediante la directiva de grupo.
  - Se pueden registrar dispositivos que no tengan Windows 10 en Azure AD sin necesidad de contar con ninguna infraestructura de AD FS. Esta funcionalidad necesita la versión 2.1 o posterior del [cliente para unirse al área de trabajo](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Características destacadas

- El nombre de usuario de inicio de sesión puede ser el predeterminado local (`userPrincipalName`) u otro atributo `Alternate ID`) configurado en Azure AD Connect. Ambos casos de uso funcionan porque SSO de conexión directa usa la notificación `securityIdentifier` en el vale Kerberos para buscar el objeto de usuario correspondiente en Azure AD.
- SSO de conexión directa es una característica oportunista. Si, por algún motivo, genera un error, la experiencia de inicio de sesión del usuario se revierte a su comportamiento habitual; es decir, el usuario deberá escribir su contraseña en la página de inicio de sesión.
- Si una aplicación, (por ejemplo, `https://myapps.microsoft.com/contoso.com`) reenvía un parámetro `domain_hint` (OpenID Connect) o `whr` (SAML) que identifica el inquilino, o bien el parámetro `login_hint` que identifica el usuario, en la solicitud de inicio de sesión de Azure AD, los usuarios inician sesión automáticamente sin necesidad de escribir nombres de usuario ni contraseñas.
- Los usuarios también obtienen una experiencia de inicio de sesión silenciosa si una aplicación (por ejemplo, `https://contoso.sharepoint.com`) envía solicitudes de inicio de sesión a los puntos de conexión configurados como inquilinos de Azure AD; es decir, `https://login.microsoftonline.com/contoso.com/<..>` o `https://login.microsoftonline.com/<tenant_ID>/<..>`, en lugar del punto de conexión común de Azure AD; es decir, `https://login.microsoftonline.com/common/<...>`.
- Se admite el cierre de sesión. Esto permite que los usuarios elijan otra cuenta de Azure AD con la cual iniciar sesión, en lugar de iniciar sesión automáticamente con SSO de conexión directa.
- Se admiten los clientes Win32 de Microsoft 365 (Outlook, Word, Excel, etc.) con las versiones 16.0.8730 y posteriores mediante un flujo no interactivo. En OneDrive, tendrá que activar la [función de configuración silenciosa de OneDrive](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894) para disfrutar de una experiencia de inicio de sesión silenciosa.
- Puede habilitarse a través de Azure AD Connect.
- Es una característica gratuita y no es necesario usar ninguna versión de pago de Azure AD para usarla.
- Se admite en clientes basados en el explorador web y en clientes de Office que admiten la [autenticación moderna](/office365/enterprise/modern-auth-for-office-2013-and-2016) en plataformas con la funcionalidad de autenticación de Kerberos:

| SO\Explorador |Internet Explorer|Microsoft Edge\*\*\*\*|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Sí\*|Sí|Sí|Sí\*\*\*|N/D
|Windows 8.1|Sí\*|Sí*\*\*\*|Sí|Sí\*\*\*|N/D
|Windows 8|Sí\*|N/D|Sí|Sí\*\*\*|N/D
|Windows Server 2012 R2 o versiones posteriores|Sí\*\*|N/D|Sí|Sí\*\*\*|N/D
|Mac OS X|N/D|N/D|Sí\*\*\*|Sí\*\*\*|Sí\*\*\*

 > [!NOTE]
 >Microsoft Edge (heredado) ya no se admite.


\*Requiere Internet Explorer, versión 11 o posterior. ([A partir del 17 de agosto de 2021, las aplicaciones y servicios de Microsoft 365 no admitirán IE 11](https://techcommunity.microsoft.com/t5/microsoft-365-blog/microsoft-365-apps-say-farewell-to-internet-explorer-11-and/ba-p/1591666)).

\*\*Requiere Internet Explorer, versión 11 o posterior. Deshabilite el modo de protección mejorada.

\*\*\*Requiere [configuración adicional](how-to-connect-sso-quick-start.md#browser-considerations).

\*\*\*\*Microsoft Edge basado en Chromium

## <a name="next-steps"></a>Pasos siguientes

- [**Inicio rápido**](how-to-connect-sso-quick-start.md): desarrollo y ejecución de SSO de conexión directa de Azure AD.
- [**Plan de implementación**](../manage-apps/plan-sso-deployment.md): paso a paso.
- [**Profundización técnica**](how-to-connect-sso-how-it-works.md): descripción del funcionamiento de esta característica.
- [**Preguntas más frecuentes**](how-to-connect-sso-faq.yml): obtenga respuestas a las preguntas más frecuentes.
- [**Solución de problemas**](tshoot-connect-sso.md): información para resolver problemas habituales de esta característica.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): para la tramitación de solicitudes de nuevas características.
