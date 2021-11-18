---
title: 'Controles de concesión en una directiva de acceso condicional: Azure Active Directory'
description: Qué son los controles de concesión en una directiva de acceso condicional de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 11/04/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: calebb, sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9636841a53621d7b840123e72846b31bf3c6334e
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132705443"
---
# <a name="conditional-access-grant"></a>Acceso condicional: Conceder

Dentro de una directiva de acceso condicional, un administrador puede hacer uso de los controles de acceso para conceder o bloquear el acceso a los recursos.

![Directiva de acceso condicional con un control de concesión que requiere autenticación multifactor](./media/concept-conditional-access-grant/conditional-access-grant.png)

## <a name="block-access"></a>Bloquear acceso

El bloqueo tiene en cuenta las asignaciones y evita el acceso en función de la configuración de la directiva de acceso condicional.

El bloqueo es un control eficaz y se debe manejar con el conocimiento adecuado. Las directivas con instrucciones de bloque pueden tener efectos secundarios imprevistos. Las pruebas y la validación adecuadas son vitales antes de habilitarlas a gran escala. Los administradores deben usar herramientas del acceso condicional como el [modo de solo informe](concept-conditional-access-report-only.md) y la [herramienta What If](what-if-tool.md) al realizar cambios.

## <a name="grant-access"></a>Conceder acceso

Los administradores pueden optar por aplicar uno o varios controles al conceder acceso. Entre estos controles se incluyen las siguientes opciones: 

- [Requerir la autenticación multifactor (Azure AD Multi-Factor Authentication)](../authentication/concept-mfa-howitworks.md)
- [Requerir que el dispositivo esté marcado como compatible (Microsoft Intune)](/intune/protect/device-compliance-get-started)
- [Requerir un dispositivo unido a Azure AD híbrido](../devices/concept-azure-ad-join-hybrid.md)
- [Requerir aplicación cliente aprobada](app-based-conditional-access.md)
- [Requerir la directiva de protección de aplicaciones](app-protection-based-conditional-access.md)
- [Requerir cambio de contraseña](#require-password-change)

Cuando los administradores eligen combinar estas opciones, pueden elegir los siguientes métodos:

- Requerir todos los controles seleccionados (control **Y** control)
- Requerir uno de los controles seleccionados (control **O** control)

De forma predeterminada, el acceso condicional requiere todos los controles seleccionados.

### <a name="require-multi-factor-authentication"></a>Requerir autenticación multifactor

Al seleccionar esta casilla, los usuarios deberán ejecutar Azure AD Multi-Factor Authentication. Puede encontrar más información acerca de la implementación de Azure AD Multi-Factor Authentication en el artículo [Planeación de una implementación de Azure AD Multi-Factor Authentication basada en la nube](../authentication/howto-mfa-getstarted.md).

[Windows Hello para empresas](/windows/security/identity-protection/hello-for-business/hello-overview) satisface el requisito de la autenticación multifactor de las directivas de acceso condicional. 

### <a name="require-device-to-be-marked-as-compliant"></a>Requerir que el dispositivo esté marcado como compatible

Las organizaciones que han implementado Microsoft Intune pueden usar la información que devuelven sus dispositivos para identificar aquellos que satisfacen los requisitos de cumplimiento específicos. Esta información del cumplimiento de las directivas se reenvía de Intune a Azure AD donde se pueden tomar decisiones de acceso condicional para conceder o bloquear el acceso a los recursos. Para más información sobre las directivas de cumplimiento, consulte el artículo [Establecimiento de reglas en los dispositivos para permitir el acceso a recursos de su organización con Intune](/intune/protect/device-compliance-get-started).

Un dispositivo se puede marcar como compatible con Intune (para cualquier sistema operativo del dispositivo) o por el sistema MDM de terceros para dispositivos Windows 10. Puede encontrar una lista de sistemas MDM de terceros compatibles en el artículo [Compatibilidad con asociados de cumplimiento de dispositivos de terceros en Intune](/mem/intune/protect/device-compliance-partners).

Los dispositivos deben estar registrados en Azure AD para poder marcarlos como compatibles. Puede encontrar más información sobre el registro del dispositivo en el artículo [¿Qué es una identidad de dispositivo?](../devices/overview.md)

**Comentarios:**

- El requisito **Requerir que el dispositivo esté marcado como compatible**:
   - Solo admite dispositivos Windows (Windows 10 o superiores), iOS, Android y macOS registrados con Azure AD e inscritos con Intune.
   - Para los dispositivos inscritos con sistemas MDM de terceros, consulte [Compatibilidad con asociados de cumplimiento de dispositivos de terceros en Intune](/mem/intune/protect/device-compliance-partners).
   - El acceso condicional no puede considerar Microsoft Edge en modo InPrivate como un dispositivo compatible.

> [!NOTE]
> En Windows 7, iOS, Android, macOS y algunos exploradores web de terceros, Azure AD identifica el dispositivo mediante un certificado de cliente que se aprovisiona cuando el dispositivo se registra con Azure AD. Cuando un usuario inicia sesión por primera vez a través del explorador, se le pide que seleccione el certificado. El usuario final debe seleccionar este certificado para poder seguir usando el explorador.

### <a name="require-hybrid-azure-ad-joined-device"></a>Requerir un dispositivo unido a Azure AD híbrido

Las organizaciones pueden optar por usar la identidad del dispositivo como parte de la directiva de acceso condicional. Las organizaciones pueden requerir que los dispositivos estén unidos a Azure AD híbrido utilizando esta casilla. Para más información sobre las identidades del dispositivo, consulte el artículo [¿Qué es una identidad de dispositivo?](../devices/overview.md)

Al usar el [flujo de OAuth de código de dispositivo](../develop/v2-oauth2-device-code.md), no se admiten el control de concesión de dispositivo administrado ni la condición de estado de dispositivo. Esto se debe a que el dispositivo que realiza la autenticación no puede proporcionar su estado de dispositivo al que proporciona un código y el estado del dispositivo en el token está bloqueado en el dispositivo que realiza la autenticación. En su lugar, use el control de concesión de autenticación multifactor necesario.

**Comentarios:**

- El requisito **Requerir un dispositivo unido a Azure AD híbrido**:
   - Solo admite dispositivos Windows de nivel inferior (Windows 10) y Windows dispositivos actuales (Windows 10 y superiores).
   - El acceso condicional no puede considerar Microsoft Edge en modo InPrivate como un dispositivo unido a Azure AD híbrido.

### <a name="require-approved-client-app"></a>Requerir aplicación cliente aprobada

Las organizaciones pueden requerir que los intentos de acceso a las aplicaciones en la nube seleccionadas se tengan que realizar desde una aplicación cliente aprobada. Estas aplicaciones cliente aprobadas son compatibles con las [directivas de protección de aplicaciones de Intune](/intune/app-protection-policy) independientemente de cualquier otra solución de administración de dispositivos móviles (MDM).

Con el fin de aplicar este control de concesión, el acceso condicional requiere que el dispositivo esté registrado en Azure Active Directory, lo que requiere el uso de una aplicación de agente. La aplicación de agente puede ser Microsoft Authenticator para iOS, o bien Microsoft Authenticator o el portal de empresa de Microsoft para dispositivos Android. Si no hay ninguna aplicación de agente instalada en el dispositivo cuando el usuario intenta autenticarse, se le redirigirá a la tienda de aplicaciones adecuada para que instale la aplicación de agente necesaria.

Se ha confirmado que las siguientes aplicaciones cliente admiten esta configuración:

- Microsoft Azure Information Protection
- Microsoft Bookings
- Microsoft Cortana
- Microsoft Dynamics 365
- Microsoft Edge
- Microsoft Excel
- Microsoft Power Automate
- Microsoft Invoicing
- Microsoft Kaizala
- Microsoft Launcher
- Microsoft Lists
- Microsoft Office
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft Power Apps
- Microsoft Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Skype Empresarial
- Microsoft StaffHub
- Microsoft Stream
- Equipos de Microsoft
- Microsoft To-Do
- Microsoft Visio
- Microsoft Word
- Microsoft Yammer
- Microsoft Whiteboard
- Administrador de Microsoft 365

**Comentarios:**

- Las aplicaciones cliente aprobadas admiten la característica de administración de aplicaciones móviles de Intune.
- Requisito de la opción **Solicitar aplicación cliente aprobada**:
   - Solo admite iOS y Android como condición de plataformas de dispositivo.
   - Se requiere una aplicación de agente para registrar el dispositivo. La aplicación de agente puede ser Microsoft Authenticator para iOS, o bien Microsoft Authenticator o el portal de empresa de Microsoft para dispositivos Android.
- El acceso condicional no puede considerar Microsoft Edge en modo InPrivate como aplicación cliente aprobada.
- El uso de Azure AD Application Proxy para permitir que la aplicación móvil de Power BI se conecte a la versión local de Power BI Report Server no es compatible con las directivas de acceso condicional que requieren la aplicación de Microsoft Power BI como una aplicación cliente aprobada.

Consulte el artículo [Uso obligatorio de aplicaciones cliente aprobadas para el acceso a aplicaciones en la nube mediante el acceso condicional](app-based-conditional-access.md) para obtener ejemplos de configuración.

### <a name="require-app-protection-policy"></a>Requerir la directiva de protección de aplicaciones

En la directiva de acceso condicional, puede requerir que una [directiva de protección de aplicaciones de Intune](/intune/app-protection-policy) esté presente en la aplicación cliente antes de que el acceso esté disponible para las aplicaciones en la nube seleccionadas. 

Con el fin de aplicar este control de concesión, el acceso condicional requiere que el dispositivo esté registrado en Azure Active Directory, lo que requiere el uso de una aplicación de agente. La aplicación de agente puede ser Microsoft Authenticator para iOS o el portal de empresa de Microsoft para dispositivos Android. Si no hay ninguna aplicación de agente instalada en el dispositivo cuando el usuario intenta autenticarse, será redirigido a la tienda de aplicaciones para que instale la aplicación de agente.

Se requiere que las aplicaciones tengan el **SDK de Intune** con el **seguro de directivas** implementado y cumplan otros requisitos para admitir esta configuración. Los desarrolladores que implementan aplicaciones con el SDK de Intune pueden encontrar más información en la documentación del SDK sobre estos requisitos.

Se ha confirmado que las siguientes aplicaciones cliente admiten esta configuración:

- Microsoft Cortana
- Microsoft Edge
- Microsoft Excel
- Microsoft Lists (iOS)
- Microsoft Office
- Microsoft OneDrive
- Microsoft OneNote
- Microsoft Outlook
- Microsoft Planner
- Microsoft Power BI
- Microsoft PowerPoint
- Microsoft SharePoint
- Microsoft Teams
- Microsoft To-Do
- Microsoft Word
- MultiLine for Intune
- Nine Mail - Email & Calendar

> [!NOTE]
> Microsoft Kaizala, Microsoft Skype Empresarial y Microsoft Visio no admiten la concesión **Requerir directiva de protección de aplicaciones**. Si necesita que estas aplicaciones funcionen, use exclusivamente la concesión **Requerir aplicaciones aprobadas**. El uso de la cláusula  `or` entre las dos concesiones no funciona para estas tres aplicaciones.

**Comentarios:**

- Las aplicaciones para la directiva de protección de aplicaciones admiten la característica de administración de aplicaciones móviles de Intune con la protección de la directiva.
- Requisitos para la **Exigencia de la directiva de protección de aplicaciones**:
    - Solo admite iOS y Android como condición de plataformas de dispositivo.
    - Se requiere una aplicación de agente para registrar el dispositivo. En iOS, la aplicación de agente es Microsoft Authenticator y, en Android, es la aplicación Portal de empresa de Intune.

Consulte el artículo [Uso obligatorio de directivas de protección de aplicaciones y una aplicación cliente aprobada para el acceso a aplicaciones en la nube con acceso condicional](app-protection-based-conditional-access.md) para obtener ejemplos de configuración.

### <a name="require-password-change"></a>Requerir cambio de contraseña 

Cuando se detecta el riesgo del usuario, mediante las condiciones de la directiva de riesgo del usuario, los administradores pueden elegir que el usuario cambie la contraseña de forma segura mediante el autoservicio de restablecimiento de contraseña de Azure AD. Si se detecta un riesgo para el usuario, los usuarios pueden utilizar un autoservicio de restablecimiento de contraseña para que se corrijan automáticamente; este proceso cerrará el evento de riesgo de usuario para evitar que los administradores tengan ruidos innecesarios. 

Cuando se solicita a un usuario que cambie su contraseña, primero será necesario para completar la autenticación multifactor. Querrá asegurarse de que todos los usuarios se hayan registrado para la autenticación multifactor, de modo que estén preparados en caso de que se detecte un riesgo para su cuenta.  

> [!WARNING]
> Los usuarios deben haberse registrado previamente para el autoservicio de restablecimiento de contraseña antes de desencadenar la directiva de riesgo de usuario. 

Restricciones cuando se configura una directiva mediante el control de cambios de contraseña.  

1. La directiva debe estar asignada a "todas las aplicaciones en la nube". Este requisito impide que un atacante use una aplicación diferente para cambiar la contraseña del usuario y restablece el riesgo de la cuenta mediante el inicio de sesión en otra aplicación. 
1. Requerir cambio de contraseña no se puede usar con otros controles, como requerir un dispositivo compatible.  
1. El control de cambio de contraseña solo se puede usar con la condición de asignación de grupo y usuario, condición de asignación de aplicación en la nube (que debe establecerse en todos) y condiciones de riesgo del usuario. 

### <a name="terms-of-use"></a>Términos de uso

Si su organización ha redactado términos de uso, es posible que aparezcan otras opciones debajo de los controles de concesión. Estas opciones permiten a los administradores exigir la aceptación de los términos de uso como una condición para acceder a los recursos protegidos por la directiva. Puede encontrar más información sobre los términos de uso en el artículo [Términos de uso de Azure Active Directory](terms-of-use.md).

## <a name="next-steps"></a>Pasos siguientes

- [Acceso condicional: Controles de sesión](concept-conditional-access-session.md)

- [Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

- [Modo de solo informe](concept-conditional-access-report-only.md)
