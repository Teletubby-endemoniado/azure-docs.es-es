---
title: 'Condiciones en una directiva de acceso condicional: Azure Active Directory'
description: Qué son las condiciones en una directiva de acceso condicional de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 09/13/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae75a0526dcccb39ba6d3b6b7779b9f6b3051039
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128592023"
---
# <a name="conditional-access-conditions"></a>Acceso condicional: Condiciones

Dentro de una directiva de acceso condicional, un administrador puede usar señales de condiciones como el riesgo, la plataforma del dispositivo o la ubicación para mejorar sus decisiones de directivas. 

[![Definición de una directiva de acceso condicional y especificación de condiciones](./media/concept-conditional-access-conditions/conditional-access-conditions.png)](./media/concept-conditional-access-conditions/conditional-access-conditions.png#lightbox)

Se pueden combinar varias condiciones para crear directivas de acceso condicional específicas y concretas.

Por ejemplo, al tener acceso a una aplicación confidencial, un administrador puede factorizar la información de riesgo de inicio de sesión de Identity Protection y la ubicación en su decisión de acceso, además de otros controles como autenticación multifactor.

## <a name="sign-in-risk"></a>Riesgo de inicio de sesión

En el caso de clientes con acceso a [Identity Protection](../identity-protection/overview-identity-protection.md), se puede evaluar el riesgo de inicio de sesión como parte de una directiva de acceso condicional. Un riesgo de inicio de sesión representa la probabilidad de que el propietario de la identidad no haya autorizado una solicitud de autenticación determinada. Puede encontrar más información sobre el riesgo de inicio de sesión en los artículos [¿Cuáles son los riesgos?](../identity-protection/concept-identity-protection-risks.md#sign-in-risk) e [Instrucciones: Configuración y habilitación de directivas de riesgo](../identity-protection/howto-identity-protection-configure-risk-policies.md).

## <a name="user-risk"></a>Riesgo de usuario 

En el caso de los clientes con acceso a [Identity Protection](../identity-protection/overview-identity-protection.md), el riesgo del usuario se puede evaluar como parte de una directiva de acceso condicional. Un riesgo de usuario representa la probabilidad de que una identidad o cuenta determinada esté en peligro. Puede encontrar más información sobre el riesgo del usuario en los artículos [¿Cuáles son los riesgos?](../identity-protection/concept-identity-protection-risks.md#user-linked-detections) e [Instrucciones: Configuración y habilitación de directivas de riesgo](../identity-protection/howto-identity-protection-configure-risk-policies.md).

## <a name="device-platforms"></a>Plataformas de dispositivo

La plataforma de dispositivo se caracteriza por el sistema operativo que se ejecuta en un dispositivo. Azure AD identifica la plataforma mediante el uso de la información proporcionada por el dispositivo, como las cadenas de agente de usuario. Como las cadenas de agente de usuario se pueden modificar, esta información no se comprueba. La plataforma de dispositivo se debe usar junto con las directivas de cumplimiento de dispositivos de Microsoft Intune o como parte de una instrucción de bloque. El valor predeterminado es aplicarlo a todas las plataformas de dispositivo.

El acceso condicional de Azure AD admite las siguientes plataformas de dispositivo:

- Android
- iOS
- Windows Phone
- Windows
- macOS

Si bloquea la autenticación heredada con la condición **Otros clientes**, también puede establecer la condición de la plataforma del dispositivo.

> [!IMPORTANT]
> Microsoft le recomienda que tenga una directiva de acceso condicional para las plataformas de dispositivos que no sean compatibles. Por ejemplo, si quiere bloquear el acceso a los recursos corporativos desde Linux o cualquier otro cliente no compatible, debe configurar una directiva con una condición de plataformas de dispositivos que incluya cualquier dispositivo, excluya las plataformas de dispositivo compatibles y conceda el control establecido para bloquear el acceso.

## <a name="locations"></a>Ubicaciones

Al configurar la ubicación como una condición, las organizaciones pueden elegir incluir o excluir ubicaciones. Estas ubicaciones con nombre pueden incluir la información de red IPv4 pública, el país o la región, o incluso áreas desconocidas que no se asignan a países o regiones en concreto. Solo los intervalos IP se pueden marcar como una ubicación de confianza.

Al incluir **cualquier ubicación**, esta opción incluye cualquier dirección IP en Internet, no solo en las ubicaciones con nombre configuradas. Al seleccionar **cualquier ubicación**, los administradores pueden optar por excluir **todas las ubicaciones de confianza** o **seleccionadas**.

Por ejemplo, algunas organizaciones pueden optar por no requerir la autenticación multifactor cuando los usuarios están conectados a la red en una ubicación de confianza, como su oficina central física. Los administradores pueden crear una directiva que incluya cualquier ubicación, pero excluya las ubicaciones seleccionadas para las redes de la oficina central.

Para más información sobre las ubicaciones, consulte el artículo [¿Qué es la condición de ubicación del acceso condicional de Azure Active Directory?](location-condition.md)

## <a name="client-apps"></a>Aplicaciones cliente

De manera predeterminada, todas las directivas de acceso condicional recién creadas se aplicarán a todos los tipos de aplicaciones cliente, incluso si la condición de las aplicaciones cliente no está configurada. 

> [!NOTE]
> El comportamiento de la condición de las aplicaciones cliente se actualizó en agosto de 2020. Si ya tiene directivas de acceso condicional, estas permanecerán sin cambios. Sin embargo, si hace clic en una directiva existente, se habrá quitado el botón de alternancia de configuración, y estarán seleccionadas las aplicaciones cliente a las que se aplica la directiva.

> [!IMPORTANT]
> Los inicios de sesión desde clientes de autenticación heredada no admiten MFA y no pasan la información del estado de los dispositivos a Azure AD, por lo que se bloquearán mediante controles de concesión de acceso condicional, como la exigencia de MFA o dispositivos compatibles. Si tiene cuentas que deben usar la autenticación heredada, debe excluir esas cuentas de la directiva, o bien configurar la directiva para que solo se aplique a los clientes de autenticación moderna.

Cuando el botón de alternancia **Configurar** está establecido en **Sí**, se aplica a los elementos seleccionados; cuando está establecido en **No**, se aplica a todas las aplicaciones cliente, incluidos los clientes de autenticación moderna y heredada. Este botón de alternancia no aparece en las directivas creadas antes de agosto de 2020.

- Clientes de autenticación moderna
   - Browser
      - Estos incluyen aplicaciones basadas en web que usan protocolos como SAML, WS-Federation, OpenID Connect o servicios registrados como un cliente de OAuth Confidential.
   - Aplicaciones móviles y aplicaciones de escritorio
      -  Esta opción incluye aplicaciones como las aplicaciones de teléfono y escritorio de Office.
- Clientes de autenticación heredada
   - Clientes de Exchange ActiveSync
      - Esta selección incluye cualquier uso del protocolo Exchange ActiveSync (EAS).
      - Cuando la directiva bloquea el uso de Exchange ActiveSync, el usuario afectado recibirá un único mensaje de correo electrónico de cuarentena. Este mensaje de correo electrónico proporciona información sobre por qué está bloqueado e incluye instrucciones de corrección si es posible.
      - Los administradores pueden aplicar directivas solo a las plataformas admitidas (como iOS, Android y Windows) mediante Microsoft Graph API de acceso condicional.
   - Otros clientes
      - Esta opción incluye clientes que usan protocolos de autenticación básicos/heredados que no admiten la autenticación moderna.
         - SMTP autenticado: usado por clientes POP e IMAP para enviar mensajes de correo electrónico.
         - Detección automática: usada por clientes Outlook y EAS para buscar y conectarse a buzones en Exchange Online.
         - Exchange Online PowerShell: se usa para conectarse a Exchange Online con PowerShell remoto. Si bloquea la autenticación básica para Exchange Online PowerShell, debe usar el módulo de Exchange Online PowerShell para conectarse. Para obtener instrucciones, consulte [Conexión a Exchange Online PowerShell con autenticación multifactor](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell).
         - Servicios web Exchange (EWS): una interfaz de programación que se usa en Outlook, Outlook para Mac y aplicaciones de terceros.
         - IMAP4: usado por clientes de correo electrónico IMAP.
         - MAPI sobre HTTP (MAPI/HTTP): usado por Outlook 2010 y versiones posteriores.
         - Libreta de direcciones sin conexión (OAB): una copia de las colecciones de listas de direcciones que Outlook descarga y usa.
         - Outlook en cualquier lugar (RPC a través de HTTP): usado por Outlook 2016 y versiones anteriores.
         - Servicio Outlook: usado por la aplicación de correo electrónico y calendario de Windows 10.
         - POP3: usado por clientes de correo electrónico POP.
         - Servicios web de creación de informes: se usan para recuperar datos de informes en Exchange Online.

Estas condiciones se suelen usar cuando se requiere un dispositivo administrado, se bloquea la autenticación heredada y se bloquea la aplicación web, pero se permiten aplicaciones móviles o de escritorio.

### <a name="supported-browsers"></a>Exploradores compatibles

Esta configuración funciona con todos los exploradores. Sin embargo, para satisfacer una directiva de dispositivo, como un requisito de dispositivo compatible, se admiten los sistemas operativos y exploradores siguientes:

| SO | Exploradores |
| :-- | :-- |
| Windows 10 | Microsoft Edge, Internet Explorer, Chrome, [Firefox 91+](https://support.mozilla.org/kb/windows-sso) |
| Windows 8 / 8.1 | Internet Explorer, Chrome |
| Windows 7 | Internet Explorer, Chrome |
| iOS | Microsoft Edge, Intune Managed Browser, Safari |
| Android | Microsoft Edge, Intune Managed Browser, Chrome |
| Windows Phone | Microsoft Edge, Internet Explorer |
| Windows Server 2019 | Microsoft Edge, Internet Explorer, Chrome |
| Windows Server 2016 | Internet Explorer |
| Windows Server 2012 R2 | Internet Explorer |
| Windows Server 2008 R2 | Internet Explorer |
| macOS | Microsoft Edge, Chrome, Safari |

Estos exploradores admiten la autenticación de dispositivo, lo que permite identificar y validar el dispositivo con respecto a una directiva. Se produce un error en la comprobación del dispositivo si el explorador se ejecuta en modo privado o si las cookies están deshabilitadas.

> [!NOTE]
> Edge 85+ requiere que el usuario inicie sesión en el explorador para pasar correctamente la identidad del dispositivo. De lo contrario, se comporta como Chrome sin la extensión de cuentas. Este inicio de sesión podría no producirse automáticamente en un escenario de Unión a Azure AD híbrido. Safari es compatible con el acceso condicional basado en dispositivos, pero no puede satisfacer las condiciones **Requerir aplicación cliente aprobada** o **Requerir la directiva de protección de aplicaciones**. Un navegador administrado como Microsoft Edge cumple ambos requisitos.

#### <a name="why-do-i-see-a-certificate-prompt-in-the-browser"></a>¿Por qué veo una solicitud de certificado en el explorador?

En Windows 7, iOS, Android y macOS, Azure AD identifica el dispositivo mediante un certificado de cliente que se aprovisiona cuando el dispositivo está registrado con Azure AD.  Cuando un usuario inicia sesión por primera vez a través del explorador, se le pide que seleccione el certificado. El usuario debe seleccionar este certificado antes de usar el explorador.

#### <a name="chrome-support"></a>Compatibilidad con Chrome

Para la compatibilidad con Chrome en **Windows 10 Creators Update (versión 1703)** o posterior, instale la [extensión de cuentas de Windows 10](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji). Esta extensión es necesaria cuando una directiva de acceso condicional requiere detalles específicos del dispositivo.

Para implementar automáticamente esta extensión en los exploradores de Chrome, cree la siguiente clave del Registro:

- Ruta de acceso HKEY_LOCAL_MACHINE \Software\Policies\Google\Chrome\ExtensionInstallForcelist
- Nombre 1
- Tipo REG_SZ (cadena)
- Datos ppnbnpeolgkicgegkbkbjmhlideopiji;https\://clients2.google.com/service/update2/crx

Para la compatibilidad con Chrome en **Windows 8.1 y 7**, cree la siguiente clave del Registro:

- Ruta de acceso HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Chrome\AutoSelectCertificateForUrls
- Nombre 1
- Tipo REG_SZ (cadena)
- Datos {"pattern":"https://device.login.microsoftonline.com","filter":{"ISSUER":{"CN":"MS-Organization-Access"}}}

### <a name="supported-mobile-applications-and-desktop-clients"></a>Clientes de escritorio y aplicaciones móviles compatibles

Las organizaciones pueden seleccionar **Aplicaciones móviles y clientes de escritorio** como aplicación cliente.

Esta configuración afecta a los intentos de acceso realizados desde las siguientes aplicaciones móviles y aplicaciones de escritorio:

| Aplicaciones cliente | Servicio de destino | Plataforma |
| --- | --- | --- |
| Aplicación de Dynamics CRM | Dynamics CRM | Windows 10, Windows 8.1, iOS y Android |
| Aplicación de Correo electrónico/Calendario/People, Outlook 2016, Outlook 2013 (con la autenticación moderna)| Exchange Online | Windows 10 |
| Directiva de MFA y de ubicación para las aplicaciones. No se admiten las directivas basadas en dispositivos.| Cualquier servicio de aplicaciones de Mis aplicaciones | Android e iOS |
| Microsoft Teams Services: esta aplicación cliente controla todos los servicios que admiten Microsoft Teams y todas sus aplicaciones cliente: escritorio de Windows, iOS, Android, WP y cliente web. | Equipos de Microsoft | Windows 10, Windows 8.1, Windows 7, iOS, Android y macOS |
| Aplicaciones de Office 2016, Office 2013 (con autenticación moderna), [cliente de sincronización de OneDrive](/onedrive/enable-conditional-access) | SharePoint | Windows 8.1, Windows 7, Windows 7 |
| Aplicaciones de Office 2016, aplicaciones universales de Office, Office 2013 (con autenticación moderna), [cliente de sincronización de OneDrive](/onedrive/enable-conditional-access) | SharePoint Online | Windows 10 |
| Office 2016 (solo Word, Excel, PowerPoint y OneNote). | SharePoint | macOS |
| Office 2019| SharePoint | Windows 10, macOS |
| Aplicaciones móviles de Office | SharePoint | Android, iOS |
| Aplicación de Yammer para Office | Yammer | Windows 10, iOS y Android |
| Outlook 2019 | SharePoint | Windows 10, macOS |
| Outlook 2016 (Office para macOS) | Exchange Online | macOS |
| Outlook 2016, Outlook 2013 (con autenticación moderna) y Skype Empresarial (con autenticación moderna) | Exchange Online | Windows 8.1, Windows 7, Windows 7 |
| Aplicación móvil de Outlook | Exchange Online | Android, iOS |
| Power BI app | Servicio Power BI | Windows 10, Windows 8.1, Windows 7, Android e iOS |
| Skype Empresarial | Exchange Online| Android, iOS |
| Aplicación de Visual Studio Team Services | Visual Studio Team Services | Windows 10, Windows 8.1, Windows 7, iOS y Android |

### <a name="exchange-activesync-clients"></a>Clientes de Exchange ActiveSync

- Las organizaciones solo pueden seleccionar clientes de Exchange ActiveSync al asignar directivas a usuarios o grupos. Si selecciona **Todos los usuarios**, **Todos los usuarios externos e invitados** o **Roles del directorio**, todos los usuarios estarán sujetos a la directiva.
- Al crear una directiva asignada a los clientes de Exchange ActiveSync, **Exchange Online** debe ser la única aplicación en la nube asignada a la directiva. 
- Las organizaciones pueden restringir el ámbito de esta directiva a plataformas específicas mediante la condición **Plataformas de dispositivo**.

Si el control de acceso que se asigna a la directiva usa **Requerir aplicación cliente aprobada**, se dirige al usuario para que instale y use el cliente móvil de Outlook. En el caso de que se requiera **autenticación multifactor**, **Términos de uso** o **controles personalizados**, los usuarios afectados se bloquearán, ya que la autenticación básica no admite estos controles.

Para más información, consulte los siguientes artículos.

- [Bloqueo de la autenticación heredada en Azure AD con acceso condicional](block-legacy-authentication.md)
- [Uso obligatorio de aplicaciones cliente aprobadas para el acceso a aplicaciones en la nube mediante el acceso condicional](app-based-conditional-access.md)

### <a name="other-clients"></a>Otros clientes

Si selecciona **Otros clientes**, puede especificar una condición que afecta a las aplicaciones que usan la autenticación básica con protocolos de correo electrónico como IMAP, MAPI, POP, SMTP y aplicaciones de Office más antiguas que no usan la autenticación moderna.

## <a name="device-state-preview"></a>Estado del dispositivo (versión preliminar)

La condición del estado del dispositivo se puede usar para excluir dispositivos que están unidos a Azure AD híbrido o dispositivos marcados como compatibles con una directiva de cumplimiento de Microsoft Intune de las directivas de acceso condicional de una organización.

Por ejemplo, *Todos los usuarios* que acceden a la aplicación en la nube *Administración de Microsoft Azure*, incluido **Todos los estados de dispositivo**, excluidos **Unido a Azure AD híbrido de dispositivo** y **Dispositivo marcado como compatible** y para *Controles de acceso*, **Bloquear**. 
   - En este ejemplo se crea una directiva que solo permite el acceso a la Administración de Microsoft Azure desde dispositivos unidos a Azure AD híbrido o desde dispositivos marcados como compatibles.

> [!IMPORTANT]
> El estado del dispositivo y los filtros para dispositivos no se pueden usar juntos en la directiva de acceso condicional. Los filtros para dispositivos proporcionan una selección de destino más granular, incluida la compatibilidad con la información de estado del dispositivo de destino a través de las propiedades `trustType` y `isCompliant`.

## <a name="filters-for-devices-preview"></a>Filtros para dispositivos (versión preliminar)

Hay una nueva condición opcional en el acceso condicional denominada filtros para dispositivos. Al configurar filtros para dispositivos como condición, las organizaciones pueden elegir incluir o excluir dispositivos según unos filtros mediante una expresión de regla en las propiedades del dispositivo. La expresión de regla de los filtros de dispositivos se puede crear mediante el generador de reglas o la sintaxis de regla. Esta experiencia es similar a la que se usa para las reglas de pertenencia dinámica para grupos. Para más información, consulte el artículo [Acceso condicional: Filtros para dispositivos (versión preliminar)](concept-condition-filters-for-devices.md).

## <a name="next-steps"></a>Pasos siguientes

- [Acceso condicional: Concesión](concept-conditional-access-grant.md)

- [Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)
