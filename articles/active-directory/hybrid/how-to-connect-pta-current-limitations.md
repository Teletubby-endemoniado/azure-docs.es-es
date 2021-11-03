---
title: 'Autenticación de paso a través de Azure AD: limitaciones actuales | Microsoft Docs'
description: En este artículo se describen las limitaciones actuales de la autenticación de paso a través de Azure Active Directory (Azure AD).
services: active-directory
keywords: Autenticación de paso a través de Azure AD Connect, instalación de Active Directory, componentes necesarios para Azure AD, SSO, inicio de sesión único
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/04/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8dc639c5dd2098871e05e6ce39bfac3c529d2694
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131063278"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Autenticación de paso a través de Azure Active Directory: limitaciones actuales

## <a name="supported-scenarios"></a>Escenarios admitidos

Se admiten los escenarios siguientes:

- El usuario inicia sesión en aplicaciones basadas en explorador web.
- El usuario inicia sesión en clientes Outlook que usan protocolos heredados como Exchange ActiveSync, EAS, SMTP, POP e IMAP.
- El usuario inicia sesión en aplicaciones del cliente de Office heredadas que admiten la [autenticación moderna](https://www.microsoft.com/en-us/microsoft-365/blog/2015/11/19/updated-office-365-modern-authentication-public-preview): versiones Office 2013 y Office 2016.
- El usuario inicia sesión en aplicaciones de protocolo heredado, como la versión 1.0 (y otras) de PowerShell.
- Azure AD se combina con los dispositivos con Windows 10.
- Contraseñas de aplicaciones de Multi-Factor Authentication.

## <a name="unsupported-scenarios"></a>Escenarios no admitidos

Los siguientes escenarios _no_ son compatibles:

- Detección de usuarios con [credenciales filtradas](../identity-protection/overview-identity-protection.md).
- Azure AD Domain Services necesita tener habilitada la sincronización de hash de contraseñas en el inquilino. Por lo tanto, los inquilinos que usan la autenticación de paso a través _únicamente_ no funcionan en escenarios que necesitan Azure AD Domain Services.
- La autenticación de paso a través no se viene integrada en [Azure AD Connect Health](./whatis-azure-ad-connect.md).
- El inicio de sesión en dispositivos unidos a Azure AD (AADJ) con una contraseña temporal o expirada no se admite para los usuarios de autenticación transferida. Se mostrará el error "No se permite el método de inicio de sesión que intenta usar".  Estos usuarios deben iniciar sesión en un explorador para actualizar su contraseña temporal.

> [!IMPORTANT]
> Como solución alternativa _solo_ para escenarios no admitidos (excepto la integración de Azure AD Connect Health), habilite la sincronización de hash de contraseñas en la página [Características opcionales](how-to-connect-install-custom.md#optional-features) del Asistente para Azure AD Connect.
> 
> [!NOTE]
> Si habilita la sincronización de hash de contraseña, tiene la opción de autenticación mediante conmutación por error si se interrumpe la infraestructura local. La conmutación por error de la autenticación de paso a través a la sincronización de hash de contraseña no es automática. Deberá cambiar el método de inicio de sesión manualmente con Azure AD Connect. Si el servidor que ejecuta Azure AD Connect deja de funcionar, necesitará la ayuda de Soporte técnico de Microsoft para desactivar la autenticación de paso a través.

## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido](how-to-connect-pta-quick-start.md): poner en marcha la autenticación de paso a través de Azure AD.
- [Migración de AD FS a la autenticación de paso a través](https://aka.ms/ADFSTOPTADPDownload): una guía detallada para migrar desde AD FS (u cualquier otra tecnología de federación) a la autenticación de paso a través.
- [Bloqueo inteligente](../authentication/howto-password-smart-lockout.md): Obtenga información sobre cómo configurar la funcionalidad de bloqueo inteligente en el inquilino para proteger las cuentas de usuario.
- [Profundización técnica](how-to-connect-pta-how-it-works.md): Conozca cómo funciona la característica de autenticación de paso a través.
- [Preguntas más frecuentes](how-to-connect-pta-faq.yml): encuentre respuesta a las preguntas más frecuentes sobre la característica de autenticación de paso a través.
- [Solución de problemas](tshoot-connect-pass-through-authentication.md): Obtenga información sobre cómo resolver problemas comunes relacionados con la característica de autenticación de paso a través.
- [Análisis a fondo de la seguridad](how-to-connect-pta-security-deep-dive.md): Obtenga información técnica detallada sobre la característica de autenticación de paso a través.
- [SSO de conexión directa de Azure AD](how-to-connect-sso.md): Más información sobre esta característica complementaria.
- [UserVoice](https://feedback.azure.com/d365community/forum/22920db1-ad25-ec11-b6e6-000d3a4f0789): Use el foro de Azure Active Directory para solicitar nuevas características.
