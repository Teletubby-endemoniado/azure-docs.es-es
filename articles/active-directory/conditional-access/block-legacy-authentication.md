---
title: 'Bloqueo de la autenticación heredada: Azure Active Directory'
description: Aprenda a mejorar su seguridad mediante el bloqueo de la autenticación heredada con el acceso condicional de Azure AD.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 11/12/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: calebb, dawoo, jebeckha, grtaylor
ms.collection: M365-identity-device-management
ms.openlocfilehash: 707e5b1e2c9adbe3b30a73a75e89ae7cf7a1beab
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132520776"
---
# <a name="how-to-block-legacy-authentication-to-azure-ad-with-conditional-access"></a>Procedimientos: Bloqueo de la autenticación heredada en Azure AD con acceso condicional   

Para brindar a los usuarios un acceso sencillo a las aplicaciones en la nube, Azure Active Directory (Azure AD) admite una amplia variedad de protocolos de autenticación, incluida la autenticación heredada. No obstante, la autenticación heredada no admite la autenticación multifactor (MFA). En muchos entornos, MFA es un requisito común para enfrentar el robo de identidad. 

> [!NOTE]
> A partir del 1 de octubre de 2022, comenzaremos a deshabilitar permanentemente la autenticación básica para Exchange Online en todos los inquilinos de Microsoft 365 independientemente del uso, excepto para la autenticación SMTP.

En su entrada de blog del 12 de marzo de 2020, [New tools to block legacy authentication in your organization](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/new-tools-to-block-legacy-authentication-in-your-organization/ba-p/1225302#), Alex Weinert, Director de seguridad de identidades en Microsoft, destaca los motivos por los que las organizaciones deben bloquear la autenticación heredada, y señala otras herramientas que Microsoft proporciona para realizar esta tarea:

> Para que MFA sea efectivo, también es necesario bloquear la autenticación heredada. Esto se debe a que los protocolos de autenticación heredados como POP, SMTP, IMAP y MAPI no pueden aplicar MFA, lo que los convierte en los puntos de entrada preferidos de los adversarios que atacan su organización.
> 
>Las cifras relativas a la autenticación heredada que ha arrojado un análisis de tráfico de Azure Active Directory (Azure AD) son inapelables:
> 
> - Más del 99 por ciento de los ataques de difusión de contraseña usan protocolos de autenticación heredados.
> - Más del 97 por ciento de los ataques de obtención de credenciales usan la autenticación heredada.
> - Las cuentas de Azure AD de las organizaciones que han deshabilitado la autenticación heredada experimentaron un 67 % menos de riesgos que aquellas en las que la autenticación heredada está habilitada.
>

Si el entorno está listo para bloquear la autenticación heredada con el fin de mejorar la protección del inquilino, puede lograr este objetivo con el acceso condicional. En este artículo se explica cómo configurar las directivas de acceso condicional que bloquean la autenticación heredada para todas las cargas de trabajo en el inquilino. 

Al implementar la protección de bloqueo de autenticación heredada, se recomienda un enfoque por fases, en lugar de deshabilitarlo para todos los usuarios a la vez. Los clientes pueden optar por empezar primero a deshabilitar la autenticación básica por protocolo, aprovechando las directivas de autenticación de Exchange Online y, opcionalmente, bloqueando también la autenticación heredada a través de directivas de acceso condicional cuando estén listas.

Los clientes sin licencias que incluyen el acceso condicional pueden usar los[valores predeterminados de seguridad](../fundamentals/concept-fundamentals-security-defaults.md) para bloquear la autenticación heredada.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se supone que está familiarizado con los [conceptos básicos](overview.md) del acceso condicional de Azure AD.

> [!NOTE]
> Las directivas de acceso condicional se aplican una vez que se completa la autenticación en una fase. El acceso condicional no pretende ser una primera línea de defensa de una organización en escenarios como los ataques por denegación de servicio (DoS), pero puede usar señales de estos eventos para determinar el acceso.

## <a name="scenario-description"></a>Descripción del escenario

Azure AD admite varios de los protocolos de autenticación y autorización usados más comúnmente, incluida la autenticación heredada. La autenticación heredada se refiere a la autenticación básica, un método estándar del sector ampliamente usado para recopilar información de nombre de usuario y contraseña. Normalmente, los clientes de autenticación heredados no pueden aplicar ningún tipo de autenticación de segundo factor. Algunos ejemplos de aplicaciones que usan normalmente o solo usan la autenticación heredada son:

- Microsoft Office 2013 o versiones posteriores.
- Aplicaciones que usan protocolos de correo como POP, IMAP y SMTP AUTH.

Para más información sobre la compatibilidad con la autenticación moderna en Office, consulte [Cómo funciona la autenticación moderna para las aplicaciones de cliente de Office](/microsoft-365/enterprise/modern-auth-for-office-2013-and-2016?view=o365-worldwide).

La autenticación de un solo factor (por ejemplo, nombre de usuario y contraseña) ya no es suficiente. No se recomiendan las contraseñas, porque son fáciles de adivinar y porque los usuarios humanos no suelen elegir contraseñas seguras. Las contraseñas también son vulnerables ante una variedad de ataques, como suplantación de identidad (phishing) y difusión de contraseña. Una de las medidas más sencillas que puede tomar para protegerse contra las amenazas para las contraseñas es implementar la autenticación multifactor (MFA). Con MFA, incluso si el atacante cuenta con la contraseña del usuario, la contraseña por sí sola no es suficiente para autenticarse de manera correcta y acceder a los datos.

¿Cómo se puede evitar que las aplicaciones que usan la autenticación heredada accedan a los recursos del inquilino? La recomendación es simplemente bloquearlas con una directiva de acceso condicional. Si es necesario, puede permitir que solo ciertos usuarios y ubicaciones de red específicas usen aplicaciones basadas en la autenticación heredada.

Las directivas de acceso condicional se aplican una vez que se completa la autenticación en una fase. Por lo tanto, el acceso condicional no está pensado como primera línea de defensa en escenarios de tipo ataques por denegación de servicio (DoS), pero puede usar señales de estos eventos (por ejemplo, el nivel de riesgo de inicio de sesión, la ubicación de la solicitud, etc.) para determinar el acceso.

## <a name="implementation"></a>Implementación

En esta sección se explica cómo configurar una directiva de acceso condicional para bloquear la autenticación heredada. 

### <a name="messaging-protocols-that-support-legacy-authentication"></a>Protocolos de mensajería que admiten la autenticación heredada

Los siguientes protocolos de mensajería admiten autenticación heredada:

- SMTP autenticado: se usa para enviar mensajes de correo electrónico autenticados.
- Detección automática: usada por clientes Outlook y EAS para buscar y conectarse a buzones en Exchange Online.
- Exchange ActiveSync (EAS): se usa para conectarse a los buzones en Exchange Online.
- Exchange Online PowerShell: se usa para conectarse a Exchange Online con PowerShell remoto. Si bloquea la autenticación básica para Exchange Online PowerShell, debe usar el módulo de Exchange Online PowerShell para conectarse. Para obtener instrucciones, consulte [Conexión a Exchange Online PowerShell con autenticación multifactor](/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/mfa-connect-to-exchange-online-powershell).
- Servicios web Exchange (EWS): una interfaz de programación que se usa en Outlook, Outlook para Mac y aplicaciones de terceros.
- IMAP4: usado por clientes de correo electrónico IMAP.
- SMTP sobre HTTP (MAPI/HTTP): protocolo de acceso al buzón de correo principal utilizado por Outlook 2010 SP2 y versiones posteriores.
- Libreta de direcciones sin conexión (OAB): una copia de las colecciones de listas de direcciones que Outlook descarga y usa.
- Outlook Anywhere (RPC a través de HTTP): protocolo de acceso a buzones de correo heredado admitido por todas las versiones Outlook actuales.
- POP3: usado por clientes de correo electrónico POP.
- Servicios web de creación de informes: se usan para recuperar datos de informes en Exchange Online.
- Servicio Outlook: usado por la aplicación de correo electrónico y calendario de Windows 10.
- Otros clientes: otros protocolos que usen autenticación heredada.

Para más información sobre estos protocolos y servicios de autenticación, vea [Informes de actividad de inicio de sesión en el portal de Azure Active Directory](../reports-monitoring/concept-sign-ins.md#filter-sign-in-activities).

### <a name="identify-legacy-authentication-use"></a>Identificación del uso de la autenticación heredada

Para poder bloquear la autenticación heredada en su directorio, primero debe entender si los usuarios tienen aplicaciones que la usen y cómo afecta a su directorio global. Se pueden usar los registros de inicio de sesión de Azure AD para saber si usa la autenticación heredada.

1. Vaya a **Azure Portal** > **Azure Active Directory** > **Inicios de sesión**.
1. Agregue la columna Aplicación cliente si no se muestra; para ello, haga clic en **Columnas** > **Aplicación cliente**.
1. **Agregar filtros** > **Aplicación cliente** > seleccione todos los protocolos de autenticación heredados. Seleccione fuera del cuadro de diálogo de filtrado para aplicar las selecciones y cierre el cuadro de diálogo.
1. Si ha activado la [versión preliminar de nuevos informes de actividad de inicio de sesión](../reports-monitoring/concept-all-sign-ins.md), repita los pasos anteriores también en la pestaña de **inicios de sesión de usuario (no interactivos)** .

Al filtrar solo se muestran los intentos de inicio de sesión que se realizaron con protocolos de autenticación heredada. Al hacer clic en cada intento de inicio de sesión individual se muestran más detalles. El campo **Aplicación cliente** en la pestaña **Información básica** indicará qué protocolo de autenticación heredada se usó.

Estos registros indicarán qué usuarios dependen todavía de la autenticación heredada y qué aplicaciones usan protocolos heredados para realizar solicitudes de autenticación. Para los usuarios que no aparecen en estos registros y se confirme que no van a usar la autenticación heredada, implemente una directiva de acceso condicional solo para estos usuarios.

## <a name="block-legacy-authentication"></a>Bloquear la autenticación heredada 

Hay dos maneras de usar las directivas de acceso condicional para bloquear la autenticación heredada.

- [Bloqueo directo de la autenticación heredada](#directly-blocking-legacy-authentication)
- [Bloqueo indirecto de la autenticación heredada](#indirectly-blocking-legacy-authentication)
 
### <a name="directly-blocking-legacy-authentication"></a>Bloqueo directo de la autenticación heredada

La forma más sencilla de bloquear la autenticación heredada en toda la organización es mediante la configuración de una directiva de acceso condicional que se aplica específicamente a los clientes de autenticación heredados y bloquea el acceso. Al asignar usuarios y aplicaciones a la directiva, asegúrese de excluir los usuarios y las cuentas de servicio que todavía deben iniciar sesión con la autenticación heredada. Al elegir las aplicaciones en la nube en las que se va a aplicar esta directiva, seleccione Todas las aplicaciones en la nube, aplicaciones de destino como Office 365 (recomendado) o, como mínimo, Office 365 Exchange Online. Configure la condición de aplicaciones cliente; para ello, seleccione **Clientes de Exchange ActiveSync** y **Otros clientes**. Para bloquear el acceso a estas aplicaciones cliente, configure los controles de acceso para bloquear el acceso.

![Condición de aplicaciones cliente configurada para bloquear la autenticación heredada](./media/block-legacy-authentication/client-apps-condition-configured-yes.png)

### <a name="indirectly-blocking-legacy-authentication"></a>Bloqueo indirecto de la autenticación heredada

Incluso si la organización no está lista para bloquear la autenticación heredada en toda la organización, debe asegurarse de que los inicios de sesión que usan la autenticación heredada no omiten las directivas que requieren controles de concesión, como requerir la autenticación multifactor o los dispositivos unidos a Azure AD híbrido o compatibles. Durante la autenticación, los clientes de autenticación heredada no admiten el envío de información sobre MFA, el cumplimiento del dispositivo o el estado de la unión a Azure AD. Por lo tanto, aplique directivas con controles de concesión a todas las aplicaciones cliente para que se bloqueen los inicios de sesión basados en la autenticación heredada que no puedan satisfacer los controles de concesión. Con la disponibilidad general de la condición de aplicaciones de cliente en agosto de 2020, las directivas de acceso condicional recién creadas se aplican a todas las aplicaciones cliente de forma predeterminada.

![Configuración predeterminada de la condición de aplicaciones cliente](./media/block-legacy-authentication/client-apps-condition-configured-no.png)

## <a name="what-you-should-know"></a>Qué debería saber

La directiva de acceso condicional puede tardar hasta 24 horas en entrar en vigor.

Al bloquear el acceso mediante **Otros clientes** también se impide que PowerShell de Exchange Online y Dynamics 365 usen la autenticación básica.

La configuración de una directiva para **otros clientes** bloquea toda la organización ante determinados clientes como SPConnect. Este bloqueo se produce porque clientes más antiguos se autentican de formas inesperadas. Este problema no aplica a las aplicaciones principales de Office, como los clientes de Office anteriores.

Puede seleccionar todos los controles de concesión disponibles para la condición **Otros clientes**, pero la experiencia del usuario final siempre es la misma: el acceso bloqueado.

### <a name="sharepoint-online"></a>SharePoint Online

Para bloquear el acceso de usuarios a través de la autenticación heredada a SharePoint Online, las organizaciones deben deshabilitar la autenticación heredada en SharePoint con el comando `Set-SPOTenant` de PowerShell y establecer el parámetro `-LegacyAuthProtocolsEnabled` en `$false`. Puede encontrar más información sobre la configuración de este parámetro en el documento de referencia de PowerShell para SharePoint con respecto a [Set-SPOTenant](/powershell/module/sharepoint-online/set-spotenant).

## <a name="next-steps"></a>Pasos siguientes

- [Determinación del impacto mediante el modo de solo informe de acceso condicional](howto-conditional-access-insights-reporting.md)
- Si todavía no sabe cómo configurar las directivas de acceso condicional, consulte [Exigir MFA para aplicaciones específicas con acceso condicional de Azure Active Directory](../authentication/tutorial-enable-azure-mfa.md) para ver un ejemplo.
- Para más información sobre la compatibilidad con la autenticación moderna, consulte [Cómo funciona la autenticación moderna para las aplicaciones de cliente de Office](/office365/enterprise/modern-auth-for-office-2013-and-2016) 
- [Configuración de una aplicación o un dispositivo multifunción para enviar correos electrónicos mediante Microsoft 365](/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365)
