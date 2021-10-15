---
title: Valores de seguridad predeterminados de Azure Active Directory
description: Directivas de seguridad predeterminadas que ayudan a proteger a las organizaciones frente a ataques comunes en Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 09/27/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: lvandenende
ms.collection: M365-identity-device-management
ms.custom: contperf-fy20q4
ms.openlocfilehash: b88cb7baa3c8eb8d40bd23d8697f563406bd1fdb
ms.sourcegitcommit: c27f71f890ecba96b42d58604c556505897a34f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2021
ms.locfileid: "129535011"
---
# <a name="what-are-security-defaults"></a>¿Cuáles son los valores de seguridad predeterminados?

La administración de la seguridad puede resultar difícil porque los ataques comunes relacionados con la identidad, como la difusión o reproducción de contraseñas y la suplantación de identidad, se están volviendo cada vez más populares. Los valores predeterminados de seguridad facilitan la protección de la organización frente a estos ataques con opciones de configuración de seguridad preconfiguradas:

- Exigir que todos los usuarios se registren en Azure AD Multi-Factor Authentication.
- Exigir que los administradores realicen la autenticación multifactor.
- Bloquear los protocolos de autenticación heredados.
- Exigir que los usuarios realicen la autenticación multifactor cuando sea necesario.
- Proteger las actividades con privilegios, como el acceso a Azure Portal.

![Captura de pantalla de Azure Portal con el botón de alternancia para habilitar los valores predeterminados de seguridad](./media/concept-fundamentals-security-defaults/security-defaults-azure-ad-portal.png)
 
Para más información sobre por qué los valores predeterminados de seguridad se están poniendo a disposición de todos, vea la entrada de blog de Alex Weinert, [Presentación de los valores predeterminados de seguridad](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/introducing-security-defaults/ba-p/1061414).

## <a name="availability"></a>Disponibilidad

Microsoft pone los valores de seguridad predeterminados a disposición de todos los usuarios. El objetivo es asegurarse de que todas las organizaciones gocen de un nivel básico de seguridad sin ningún costo adicional. Los valores predeterminados de seguridad se activan en Azure Portal. Si el inquilino se creó a partir del 22 de octubre de 2019, puede que los valores predeterminados de seguridad ya estén habilitados en el inquilino. Para proteger a todos los usuarios, los valores predeterminados de seguridad se están implementando en todos los nuevos inquilinos en el momento de su creación.

### <a name="whos-it-for"></a>¿Para quiénes son?

- Para organizaciones que buscan aumentar la posición de seguridad, pero no saben cómo empezar o por dónde.
- Para organizaciones que utilizan el nivel gratis de licencias de Azure Active Directory.

### <a name="who-should-use-conditional-access"></a>¿Quién debe usar el acceso condicional?

- Si es una organización que usa actualmente directivas de acceso condicional para unificar las señales, tomar decisiones y aplicar las directivas de la organización, es probable que los valores predeterminados de seguridad no sean los más adecuados. 
- Si es una organización con licencias de Azure Active Directory Premium, es probable que los valores predeterminados de seguridad tampoco le convengan.
- Sin embargo, si su organización tiene requisitos de seguridad complejos, debería tener en cuenta el acceso condicional.

## <a name="policies-enforced"></a>Directivas aplicadas

### <a name="unified-multi-factor-authentication-registration"></a>Registro unificado de Multi-Factor Authentication

Todos los usuarios del inquilino deben registrarse para la autenticación multifactor (MFA) en la forma del servicio Azure AD Multi-Factor Authentication. Los usuarios tendrán 14 días para registrarse en Azure AD Multi-Factor Authentication con la aplicación Microsoft Authenticator. Una vez transcurridos los 14 días, el usuario no podrá iniciar sesión hasta que se complete el registro. El período de 14 días de un usuario comienza después del primer inicio de sesión interactivo correcto después de habilitar los valores de seguridad predeterminados.

### <a name="protecting-administrators"></a>Protección de los administradores

Los usuarios con acceso a cuentas con privilegios tienen un mayor acceso a su entorno. Dadas las facultades de estas cuentas, debe tratarlas con un cuidado especial. Un método común para mejorar la protección de las cuentas con privilegios es exigir una forma de verificación de la cuenta más estricta para iniciar sesión. En Azure AD, puede exigir el uso de Multi-Factor Authentication para conseguir una verificación de cuentas más estricta.

Una vez finalizado el registro con Azure AD Multi-Factor Authentication, los roles de administrador de Azure AD siguientes deberán realizar una autenticación adicional cada vez que inicien sesión:

- Administrador global
- Administrador de aplicaciones
- Administrador de autenticación
- Administrador de facturación
- Administrador de aplicaciones en la nube
- Administrador de acceso condicional
- Administrador de Exchange
- Administrador del departamento de soporte técnico
- Administrador de contraseñas
- Administrador de autenticación con privilegios
- Administrador de seguridad
- Administrador de SharePoint
- Administrador de usuarios

> [!WARNING]
> Asegúrese de que el directorio tenga al menos dos cuentas que tengan asignados privilegios de administrador global. Esto le ayudará en caso de que un administrador global esté bloqueado. Para obtener más información, consulte el artículo [Administración de cuentas de acceso de emergencia en Azure AD](../roles/security-emergency-access.md).

### <a name="protecting-all-users"></a>Protección de todos los usuarios

Se tiende a pensar que las cuentas de administrador son las únicas cuentas que necesitan capas adicionales de autenticación. Los administradores tienen un amplio acceso a información confidencial y pueden realizar cambios en la configuración de toda la suscripción. Sin embargo, los atacantes suelen dirigirse a los usuarios finales. 

Una vez que estos atacantes obtienen acceso, pueden solicitar acceso a información privilegiada en nombre del titular de la cuenta original. Incluso pueden descargar todo el directorio para realizar un ataque de suplantación de identidad (phishing) en toda la organización. 

Un método común para mejorar la protección de todos los usuarios es exigir a todos una forma más estricta de verificación de cuentas, como Multi-Factor Authentication (MFA). Cuando los usuarios finalicen el registro de Multi-Factor Authentication, se les pedirá otra autenticación siempre que sea necesario. Se solicitará a los usuarios principalmente cuando se autentiquen mediante un nuevo dispositivo o aplicación, o cuando desempeñen tareas y roles críticos. Esta funcionalidad protege todas las aplicaciones registradas con Azure AD, incluidas las aplicaciones SaaS.

### <a name="blocking-legacy-authentication"></a>Bloqueo de la autenticación heredada

Para brindar a los usuarios un acceso sencillo a las aplicaciones en la nube, Azure AD admite una variedad de protocolos de autenticación, incluida la autenticación heredada. La *autenticación heredada* es un término que hace referencia a una solicitud de autenticación realizada por:

- Clientes que no usan la autenticación moderna (por ejemplo, el cliente de Office 2010).
- Cualquier cliente que use protocolos de correo antiguos, como IMAP, SMTP o POP3.

Hoy en día, la mayoría de los intentos de inicio de sesión que ponen en peligro la seguridad proceden de la autenticación heredada. La autenticación heredada no admite Multi-Factor Authentication. Incluso si tiene una directiva de Multi-Factor Authentication habilitada en el directorio, un atacante puede autenticarse mediante un protocolo antiguo y omitir Multi-Factor Authentication. 

Después de habilitar los valores de seguridad predeterminados en el inquilino, se bloquearán todas las solicitudes de autenticación realizadas con un protocolo antiguo. Los valores predeterminados de seguridad bloquean la autenticación básica de Exchange Active Sync.

> [!WARNING]
> Antes de habilitar los valores predeterminados de seguridad, asegúrese de que los administradores no estén usando protocolos de autenticación antiguos. Para más información, consulte [Cómo cambiar la autenticación heredada](concept-fundamentals-block-legacy-authentication.md).

- [Configuración de una aplicación o un dispositivo multifunción para enviar correos electrónicos mediante Microsoft 365](/exchange/mail-flow-best-practices/how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365)

### <a name="protecting-privileged-actions"></a>Protección de acciones con privilegios

Las organizaciones usan diversos servicios de Azure que se administran mediante la API de Azure Resource Manager, entre ellos:

- Azure Portal 
- Azure PowerShell 
- Azure CLI

El uso de Azure Resource Manager para administrar los servicios es una acción con privilegios elevados. Azure Resource Manager puede modificar las configuraciones de todo el inquilino, como la configuración del servicio y la facturación de la suscripción. La autenticación de factor único es vulnerable a una variedad de ataques, como la suplantación de identidad (phishing) y la difusión de contraseñas. 

Es importante comprobar la identidad de los usuarios que quieren acceder a Azure Resource Manager y actualizar las configuraciones. Para comprobar su identidad, solicite una autenticación adicional antes de permitir el acceso.

Después de habilitar los valores de seguridad predeterminados en el inquilino, se pedirá a todos los usuarios que accedan al Azure Portal, Azure PowerShell o la CLI de Azure que completen una autenticación adicional. Esta directiva se aplica a todos los usuarios que acceden a Azure Resource Manager, independientemente de si son administradores o usuarios. 

> [!NOTE]
> Los inquilinos de Exchange Online anteriores a 2017 tienen la autenticación moderna deshabilitada de forma predeterminada. Para evitar la posibilidad de que se produzca un bucle de inicio de sesión durante la autenticación a través de estos inquilinos, debe [habilitar la autenticación moderna](/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online).

> [!NOTE]
> La cuenta de sincronización de Azure AD Connect se excluye de los valores predeterminados de seguridad y no se le pedirá que se registre ni que realice la autenticación multifactor. Las organizaciones no deben usar esta cuenta para otros fines.

## <a name="deployment-considerations"></a>Consideraciones de la implementación

A continuación, se muestran consideraciones adicionales relacionadas con la implementación de los valores de seguridad predeterminados.

### <a name="authentication-methods"></a>Métodos de autenticación

Estos valores predeterminados de seguridad gratis permiten el registro y el uso de Azure AD Multi-Factor Authentication **solo mediante la aplicación Microsoft Authenticator con notificaciones**. El acceso condicional permite el uso de cualquier método de autenticación que el administrador decida habilitar.

| Método | Valores predeterminados de seguridad | Acceso condicional |
| --- | --- | --- |
| Notificación a través de aplicación móvil | X | X |
| Código de verificación de aplicación móvil o token de hardware | X* | X |
| Mensaje de texto al teléfono |   | X |
| Llamada al teléfono |   | X |
| Contraseñas de aplicación |   | X*** |

- ** Los usuarios pueden usar códigos de verificación de la aplicación Microsoft Authenticator, pero solo pueden registrarse mediante la opción de notificación.
- *** Las contraseñas de aplicación solo están disponibles en MFA por usuario con escenarios de autenticación heredados si las habilitan los administradores.

> [!WARNING]
> No deshabilite los métodos de la organización si está usando Valores predeterminados de seguridad. La deshabilitación de los métodos puede dar lugar a su propia expulsión del inquilino. Deje todos los **Métodos disponibles para los usuarios** habilitados en el [portal de configuración del servicio MFA](../authentication/howto-mfa-getstarted.md#choose-authentication-methods-for-mfa).

### <a name="disabled-mfa-status"></a>Estado de MFA deshabilitado

Si su organización es un usuario anterior de Azure AD Multi-Factor Authentication basado en usuarios, no se alarme si no ve usuarios con el estado **Habilitado** o **Aplicado** en la página de estado de Multi-Factor Authentication. **Deshabilitado** es el estado adecuado para los usuarios que utilizan valores predeterminados de seguridad o Azure AD Multi-Factor Authentication basado en el acceso condicional.

### <a name="conditional-access"></a>Acceso condicional

Puede usar el acceso condicional para configurar directivas similares a los valores predeterminados de seguridad, pero con más granularidad, incluidas las exclusiones de usuario, que no están disponibles en los valores predeterminados de seguridad. Si usa acceso condicional y tiene habilitadas directivas de acceso condicional en su entorno, los valores de seguridad predeterminados no estarán disponibles. Si tiene una licencia que proporciona acceso condicional, pero no tiene ninguna directiva de acceso condicional habilitada en su entorno, puede usar los valores de seguridad predeterminados hasta que habilite las directivas de acceso condicional. Para más información sobre las licencias de Azure AD, consulte la [página de precios de Azure AD](https://www.microsoft.com/security/business/identity-access-management/azure-ad-pricing).

![Mensaje de advertencia que indica que puede tener valores predeterminados de seguridad o acceso condicional, pero no ambos](./media/concept-fundamentals-security-defaults/security-defaults-conditional-access.png)

Estas son guías paso a paso sobre cómo puede usar el acceso condicional para configurar un conjunto de directivas, que forman un buen punto inicial para proteger las identidades:

- [Exigir autenticación multifactor para administradores](../conditional-access/howto-conditional-access-policy-admin-mfa.md)
- [Exigir autenticación multifactor para la administración de Azure](../conditional-access/howto-conditional-access-policy-azure-management.md)
- [Bloquear la autenticación heredada](../conditional-access/howto-conditional-access-policy-block-legacy.md)
- [Exigir autenticación multifactor para todos los usuarios](../conditional-access/howto-conditional-access-policy-all-users-mfa.md)

## <a name="enabling-security-defaults"></a>Habilitación de los valores de seguridad predeterminados

Para habilitar los valores de seguridad predeterminados en su directorio:

1. Inicie sesión en  [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a  **Azure Active Directory** > **Propiedades**.
1. Seleccione **Administrar valores predeterminados de seguridad**.
1. Establezca **Habilitar valores predeterminados de seguridad** en **Sí**.
1. Seleccione **Guardar**.

## <a name="disabling-security-defaults"></a>Deshabilitación de los valores predeterminados de seguridad

Las organizaciones que decidan implementar directivas de acceso condicional que reemplacen los valores predeterminados de seguridad deben deshabilitar estos últimos. 

![Mensaje de advertencia para deshabilitar los valores predeterminados de seguridad y permitir el acceso condicional](./media/concept-fundamentals-security-defaults/security-defaults-disable-before-conditional-access.png)

Para deshabilitar los valores predeterminados de seguridad en el directorio:

1. Inicie sesión en  [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a  **Azure Active Directory** > **Propiedades**.
1. Seleccione **Administrar valores predeterminados de seguridad**.
1. Establezca **Activación de los valores predeterminados de seguridad**  en **No**.
1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](../conditional-access/concept-conditional-access-policy-common.md)
