---
title: 'Azure AD Connect: Autenticación en la nube mediante un lanzamiento preconfigurado | Microsoft Docs'
description: En este artículo se explica cómo migrar desde la autenticación federada a la autenticación en la nube mediante un lanzamiento preconfigurado.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 06/03/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35dcb1125451e378d79aa7bab4c4e066ccab9acb
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130044315"
---
# <a name="migrate-to-cloud-authentication-using-staged-rollout"></a>Migración a la autenticación en la nube mediante un lanzamiento almacenado preconfigurado

El lanzamiento preconfigurado le permite probar de forma selectiva grupos de usuarios con funcionalidades de autenticación en la nube, como Azure AD Multi-Factor Authentication (MFA), Acceso condicional, Identity Protection para credenciales filtradas, Identity Governance y otros, antes de utilizar sus propios dominios.  En este artículo se describe cómo realizar el cambio. Sin embargo, antes de comenzar el lanzamiento preconfigurado, debe tener en cuenta las consecuencias en caso de que se cumplan las condiciones siguientes:
    
-  Usa actualmente un servidor de Multi-Factor Authentication local. 
-  Usa tarjetas inteligentes para la autenticación. 
-  El servidor actual ofrece ciertas características de solo federación.

Antes de probar esta característica, le recomendamos que revise la guía sobre cómo elegir el método de autenticación correcto. Para más información, consulte la tabla "Comparación de métodos" del artículo [Selección del método de autenticación adecuado para su solución de identidad híbrida de Azure Active Directory](./choose-ad-authn.md#comparing-methods).

Para información general sobre la característica, vea este vídeo "Azure Active Directory: ¿Qué es el lanzamiento preconfigurado?" :

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE3inQJ]



## <a name="prerequisites"></a>Requisitos previos

-   Tiene un inquilino de Azure Active Directory (Azure AD) con dominios federados.

-   Ha decidido pasar a una de estas dos opciones:
    - **Opción A** - *sincronización de hash de contraseña (sincronización)* .  Para más información, consulte [¿Qué es la sincronización de hash de contraseña?](whatis-phs.md) 
    - **Opción B** - *autenticación transferida*.  Para más información, consulte [¿Qué es la autenticación de paso a través?](how-to-connect-pta.md)  
    
    Para ambas opciones, se recomienda habilitar el inicio de sesión único (SSO) para lograr una experiencia de inicio de sesión silenciosa. 
    En el caso de los dispositivos Windows 7 u 8.1 unidos a un dominio, se recomienda usar el SSO de conexión directa. Para obtener más información, consulte [¿Qué es SSO de conexión directa?](how-to-connect-sso.md) 
    Para Windows 10, Windows Server 2016 y versiones posteriores, se recomienda usar el SSO a través del [token de actualización principal (PRT)](../devices/concept-primary-refresh-token.md) con [dispositivos unidos a Azure AD](../devices/concept-azure-ad-join.md), [dispositivos híbridos unidos a Azure AD](../devices/concept-azure-ad-join-hybrid.md) o dispositivos personales registrados a través de Agregar cuenta profesional o educativa.

-   Ha configurado todas las directivas adecuadas de acceso condicional y personalización de marca del inquilino que necesita para los usuarios que se van a migrar a la autenticación en la nube.

-   Si tiene previsto usar Azure AD Multi-Factor Authentication, le recomendamos que use el [registro combinado para el autoservicio de restablecimiento de contraseña (SSPR) y Multi-Factor Authentication](../authentication/concept-registration-mfa-sspr-combined.md) para que los usuarios registren sus métodos de autenticación una sola vez. Nota: cuando se usa SSPR para restablecer la contraseña o cambiar la contraseña mediante la página MyProfile en el lanzamiento preconfigurado, Azure AD Connect necesita sincronizar el nuevo hash de contraseña, lo que puede tardar hasta dos minutos después del restablecimiento.

-   Para usar la característica de lanzamiento preconfigurado, debe ser administrador global en el inquilino.

-   Para habilitar el *inicio de sesión único de conexión directa* en un bosque específico de Active Directory, debe ser administrador de dominio.

-  Si va a implementar Azure AD híbrido o la unión a Azure AD, debe cambiar a la actualización 1903 de Windows 10.


## <a name="supported-scenarios"></a>Escenarios admitidos

Se admiten estos escenarios en el lanzamiento preconfigurado: La característica solo funciona en los siguientes casos:

- Usuarios que se aprovisionan en Azure AD mediante Azure AD Connect. No se aplica a los usuarios solo de nube.

- El tráfico de inicio de sesión del usuario en los exploradores y clientes de *autenticación moderna*. Las aplicaciones o los servicios en la nube que usan la autenticación heredada revertirán a los flujos de autenticación federada. Un ejemplo podría ser Exchange Online con la autenticación moderna desactivada o Outlook 2010, que no admite autenticación moderna.

- El tamaño de grupo está limitado actualmente a 50 000 usuarios.  Si tiene grupos de más de 50 000 usuarios, se recomienda dividir este grupo en varios grupos para la implementación por fases.

- Adquisición de token de actualización principal de Unión híbrida a Windows 10 o Unión a Azure AD sin línea de visión con el servidor de federación para Windows 10 versión 1903 y posteriores, cuando el UPN del usuario se puede enrutar y el sufijo de dominio se comprueba en Azure AD.

## <a name="unsupported-scenarios"></a>Escenarios no admitidos

Los siguientes escenarios no se admiten en el lanzamiento preconfigurado:

- No se admite la autenticación heredada, como POP3 y SMTP.

- Ciertas aplicaciones envían el parámetro de consulta "domain_hint" a Azure AD durante la autenticación. Estos flujos continuarán, y los usuarios habilitados para el lanzamiento preconfigurado seguirán usando la federación en la autenticación.

<!-- -->

- El administrador puede implementar la autenticación en la nube mediante grupos de seguridad. Para evitar la latencia de la sincronización cuando se usan grupos de seguridad de Active Directory locales, se recomienda usar grupos de seguridad de nube. Se aplican las siguientes condiciones:

    - Puede usar hasta 10 grupos por característica. Es decir, puede usar 10 grupos por cada *sincronización de hash de contraseña*, *autenticación de paso a través* e *inicio de sesión único de conexión directa*.
    - *No se admiten* grupos anidados. 
    - Los grupos dinámicos *no se admiten* en el lanzamiento preconfigurado.
    - Los objetos de contacto dentro del grupo impedirán que se agregue el grupo.

- La primera vez que se agrega un grupo de seguridad para el lanzamiento preconfigurado, está limitado a 200 usuarios para evitar que se agote el tiempo de espera de la experiencia de usuario. Después de agregar el grupo, puede agregarle más usuarios directamente, según sea necesario.

- Mientras los usuarios están en el lanzamiento preconfigurado, la directiva de expiración de contraseñas se establece en 90 días sin opción de personalizarla. 

- Adquisición de token de actualización principal de Unión híbrida a Windows 10 o Unión a Azure AD para versiones de Windows 10 anteriores a la 1903. Este escenario revertirá al punto de conexión de WS-Trust del servidor de federación, incluso si el usuario que inicia sesión está en el ámbito del lanzamiento preconfigurado.

- Adquisición de token de actualización principal de Unión híbrida a Windows 10 o Unión a Azure AD para todas las versiones, cuando el UPN local del usuario no se puede enrutar. Este escenario revertirá al punto de conexión de WS-Trust durante el modo de implementación por fases, pero dejará de funcionar cuando se complete la migración por fases y el inicio de sesión de usuario ya no dependa del servidor de federación.

- Si tiene una configuración de VDI no persistente con Windows 10 versión 1903 o posterior, debe permanecer en un dominio federado. No se admite el traslado a un dominio administrado en VDI no persistente. Para más información, consulte [Identidad de dispositivo y virtualización del escritorio](../devices/howto-device-identity-virtual-desktop-infrastructure.md).

- Si tiene una relación de confianza de certificado híbrido de Windows Hello para empresas con certificados emitidos a través del servidor de federación que actúa como Autoridad de registro o usuarios de tarjeta inteligente, el escenario no es compatible con un lanzamiento preconfigurado. 

- La inscripción de Autopilot no se admite en el lanzamiento preconfigurado. Los usuarios habilitados para el lanzamiento preconfigurado seguirán usando la autenticación federada en el momento de la inscripción de Autopilot. Si el dispositivo tiene Windows 10 versión 1903 o posterior, después de la inscripción de Autopilot, todas las solicitudes de autenticación pasarán por el lanzamiento preconfigurado. 

  >[!NOTE]
  >Sigue siendo necesario realizar el traslado final de la autenticación federada a la nube mediante Azure AD Connect o PowerShell. El lanzamiento preconfigurado no cambia de dominio federado a administrado.  Para más información sobre la migración de dominios, consulte [Migración de la federación a la sincronización de hash de contraseña](./migrate-from-federation-to-cloud-authentication.md) y [Migración de la federación a la autenticación de paso a través](./migrate-from-federation-to-cloud-authentication.md).
  
## <a name="get-started-with-staged-rollout"></a>Introducción al lanzamiento preconfigurado

Para probar el inicio de sesión de *sincronización de hash de contraseñas* mediante el lanzamiento preconfigurado, siga las instrucciones del trabajo previo en la sección siguiente.

Para información sobre qué cmdlets de PowerShell usar, consulte la [versión preliminar de Azure AD 2.0](/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true#staged_rollout).

## <a name="pre-work-for-password-hash-sync"></a>Trabajo previo para la sincronización de hash de contraseña

1. Habilite *Sincronización de hash de contraseña* en la página [Características opcionales](how-to-connect-install-custom.md#optional-features) de Azure AD Connect. 

   ![Captura de pantalla de la página "Características opcionales" de Azure Active Directory Connect](media/how-to-connect-staged-rollout/staged-1.png)

1. Asegúrese de que se ha ejecutado un ciclo completo de *sincronización de hash de contraseña* de modo que todos los hash de contraseña de los usuarios se hayan sincronizado con Azure AD. Para comprobar el estado de la *sincronización de hash de contraseña*, puede usar el diagnóstico de PowerShell que se describe en [Solución de problemas de sincronización de hash de contraseñas con la sincronización de Azure AD Connect](tshoot-connect-password-hash-synchronization.md).

   ![Captura de pantalla del registro de solución de problemas de AAD Connect](./media/how-to-connect-staged-rollout/staged-2.png)

Si quiere probar el inicio de sesión de *autenticación de paso a través* con el lanzamiento preconfigurado, siga las instrucciones del trabajo previo de la sección siguiente para habilitarlo.

## <a name="pre-work-for-pass-through-authentication"></a>Trabajo previo para la autenticación de paso a través

1. Identifique un servidor que ejecute Windows Server 2012 R2 o posterior en el que quiera ejecutar el agente de *autenticación de paso a través*. 

   *No* elija el servidor de Azure AD Connect.  Asegúrese de que el servidor esté unido a un dominio, que pueda autenticar a los usuarios seleccionados con Active Directory y que pueda comunicarse con Azure AD en los puertos y direcciones URL de salida. Para más información, consulte la sección "Paso 1: Comprobación de requisitos previos" del [Inicio rápido: Inicio de sesión único de conexión directa de Azure AD](how-to-connect-sso-quick-start.md).

1. [Descargue el agente de autenticación de Azure AD Connect](https://aka.ms/getauthagent) e instálelo en el servidor. 

1. Para permitir [alta disponibilidad](how-to-connect-sso-quick-start.md), instale agentes de autenticación adicionales en otros servidores.

1. Asegúrese de que ha [configurado el bloqueo inteligente](../authentication/howto-password-smart-lockout.md) adecuadamente. De este forma se garantiza que individuos infiltrados no bloqueen las cuentas de Active Directory locales de los usuarios.

Se recomienda habilitar el *inicio de sesión único de conexión directa* con independencia del método de inicio de sesión (*sincronización de hash de contraseña* o *autenticación de paso a través*) que seleccione para el lanzamiento preconfigurado. Para habilitar el *inicio de sesión único de conexión directa*, siga las instrucciones del trabajo previo de la sección siguiente.

## <a name="pre-work-for-seamless-sso"></a>Trabajo previo para el inicio de sesión único de conexión directa

Habilite el *inicio de sesión único de conexión directa* en los bosques de Active Directory mediante PowerShell. Si tiene más de un bosque de Active Directory, habilítelo individualmente para cada uno. El *SSO de conexión directa* solo se desencadena en los usuarios seleccionados para el lanzamiento preconfigurado. No afecta a la configuración de federación existente.

Para habilitar el *inicio de sesión único de conexión directa*, haga lo siguiente:

1. Inicie sesión en el servidor de Azure AD Connect.

2. Vaya a la carpeta *%ProgramFiles%\\Microsoft Azure Active Directory Connect*.

3. Importe el módulo de PowerShell de *inicio de sesión único de conexión directa* mediante la ejecución del siguiente comando: 

   `Import-Module .\AzureADSSO.psd1`

4. Ejecute PowerShell como administrador. En PowerShell, llame a `New-AzureADSSOAuthenticationContext`. Este comando abre un panel en el que puede especificar las credenciales de administrador global del inquilino.

5. Llame a `Get-AzureADSSOStatus | ConvertFrom-Json`. Este comando muestra una lista de bosques de Active Directory (consulte la lista "Dominios") en los que se ha habilitado esta característica. De forma predeterminada, se establece en False en el nivel de inquilino.

   ![Ejemplo de la salida de Windows PowerShell](./media/how-to-connect-staged-rollout/staged-3.png)

6. Llame a `$creds = Get-Credential`. Cuando se le pida, escriba las credenciales del administrador de dominio correspondientes al bosque de Active Directory deseado.

7. Llame a `Enable-AzureADSSOForest -OnPremCredentials $creds`. Este comando crea la cuenta de equipo AZUREADSSOACC del controlador de dominio local para este bosque de Active Directory que se necesita para el *inicio de sesión único de conexión directa*.

8. Para el *inicio de sesión único de conexión directa*, es necesario que las direcciones URL estén en la zona de intranet. Para implementar estas direcciones URL mediante directivas de grupo, consulte [Inicio rápido: Inicio de sesión único de conexión directa de Azure AD](how-to-connect-sso-quick-start.md#step-3-roll-out-the-feature).

9. Para ver un tutorial completo, también puede descargar los [planes de implementación](https://aka.ms/SeamlessSSODPDownload) para el *inicio de sesión único de conexión directa*.

## <a name="enable-staged-rollout"></a>Habilitación del lanzamiento preconfigurado

Para implementar una característica concreta (*autenticación de paso a través*, *sincronización de hash de contraseña* o *inicio de sesión único de conexión directa*) en un conjunto seleccionado de usuarios de un grupo, siga las instrucciones de las secciones siguientes.

### <a name="enable-a-staged-rollout-of-a-specific-feature-on-your-tenant"></a>Habilitación de un lanzamiento preconfigurado de una característica específica en el inquilino

Puede implementar una de estas opciones:

- **Opción A** - *sincronización de hash de contraseña* + *inicio de sesión único de conexión directa*
- **Opción B** - *autenticación de paso a través* + *inicio de sesión único de conexión directa*
- **No se admite** - *sincronización de hash de contraseña* + *autenticación de paso a través* + *inicio de sesión único de conexión directa*

Haga lo siguiente:

1. Para acceder a la experiencia de usuario, inicie sesión en el [portal de Azure AD](https://aka.ms/stagedrolloutux).

2. Seleccione el vínculo **Habilitar el lanzamiento preconfigurado para el inicio de sesión de usuarios administrados**.

   Por ejemplo, si quiere habilitar la *Opción A*, deslice los controles **Sincronización de hash de contraseña** e **Inicio de sesión de conexión directa** a **Activado**, como se muestra en las imágenes siguientes.

   

  

3. Agregue los grupos a la característica para habilitar la *autenticación de paso a través* y el *inicio de sesión único de conexión directa*. Para evitar tiempo de espera en la experiencia del usuario, asegúrese de que los grupos de seguridad no contengan más de 200 miembros inicialmente.

   

   >[!NOTE]
   >Los miembros de un grupo se habilitan automáticamente para el lanzamiento preconfigurado. No se admiten grupos anidados y dinámicos en el lanzamiento preconfigurado.
   >Al agregar un nuevo grupo, los usuarios del grupo (hasta 200 usuarios para un grupo nuevo) se actualizarán para usar la autenticación administrada de forma inmediata. Al editar un grupo (agregar o eliminar usuarios), los cambios pueden tardar hasta 24 horas en surtir efecto.
   >El inicio de sesión único de conexión directa solo se aplicará si los usuarios están en el grupo de SSO de conexión directa y también en un grupo de PTA o de PHS.

## <a name="auditing"></a>Auditoría

Se han habilitado eventos de auditoría para las diferentes acciones que se realizan en el lanzamiento preconfigurado:

- Evento de auditoría cuando se habilita un lanzamiento preconfigurado para *sincronización de hash de contraseña*, *autenticación de paso a través* o *inicio de sesión único de conexión directa*.

  >[!NOTE]
  >Se registra un evento de auditoría cuando se activa el *inicio de sesión único de conexión directa* mediante el lanzamiento preconfigurado.

  ![Panel "Create rollout policy for feature" (Crear directiva de implementación de características): pestaña Activity (Actividad)](./media/how-to-connect-staged-rollout/staged-7.png)

  ![Panel "Create rollout policy for feature" (Crear directiva de implementación de características): pestaña Modified Properties (Propiedades modificadas)](./media/how-to-connect-staged-rollout/staged-8.png)

- Evento de auditoría cuando se agrega un grupo a *sincronización de hash de contraseñas*, *autenticación de paso a través* o *inicio de sesión único de conexión directa*.

  >[!NOTE]
  >Se registra un evento de auditoría cuando se agrega un grupo a *sincronización de hash de contraseña* en el lanzamiento preconfigurado.

  ![Panel "Add a group to feature rollout" (Agregar un grupo a la implementación de características): pestaña Activity (Actividad)](./media/how-to-connect-staged-rollout/staged-9.png)

  ![Panel "Add a group to feature rollout" (Agregar un grupo a la implementación de características): pestaña Modified Properties (Propiedades modificadas)](./media/how-to-connect-staged-rollout/staged-10.png)

- Evento de auditoría cuando un usuario que se agregó al grupo está habilitado para el lanzamiento preconfigurado.

  ![Panel "Add user to feature rollout" (Agregar un usuario a la implementación de características): pestaña Activity (Actividad)](media/how-to-connect-staged-rollout/staged-11.png)

  ![Panel "Add user to feature rollout" (Agregar un usuario a la implementación de características): pestaña Taget(s) (Destinos)](./media/how-to-connect-staged-rollout/staged-12.png)

## <a name="validation"></a>Validación

Para probar el inicio de sesión con *sincronización de hash de contraseña* o *autenticación de paso a través* (inicio de sesión con nombre de usuario y contraseña), haga lo siguiente:

1. En la extranet, vaya a la [página Aplicaciones](https://myapps.microsoft.com) en una sesión privada del explorador y, luego, escriba el UPN (UserPrincipalName) de la cuenta de usuario seleccionada para el lanzamiento preconfigurado.

   Los usuarios destinados al lanzamiento preconfigurado no se redirigen a la página de inicio de sesión federado, sino que se les pide que inicien sesión en la página de inicio de sesión con personalización de marca del inquilino de Azure AD.

1. Asegúrese de que el inicio de sesión se muestra correctamente en el [informe de actividad de inicio de sesión de Azure AD](../reports-monitoring/concept-sign-ins.md) mediante el filtrado por el nombre principal de usuario.

Para probar el inicio de sesión con *inicio de sesión único de conexión directa*:

1. En la intranet, vaya a la [página Aplicaciones](https://myapps.microsoft.com) en una sesión privada del explorador y, luego, escriba el UPN (UserPrincipalName) de la cuenta de usuario seleccionada para el lanzamiento preconfigurado.

   Los usuarios destinados al lanzamiento preconfigurado de *inicio de sesión único de conexión directa* reciben un mensaje de intento de inicio de sesión antes de que se inicie sesión en modo silencioso.

1. Asegúrese de que el inicio de sesión se muestra correctamente en el [informe de actividad de inicio de sesión de Azure AD](../reports-monitoring/concept-sign-ins.md) mediante el filtrado por el nombre principal de usuario.

   Para realizar un seguimiento de los inicios de sesión de los usuarios seleccionados para el lanzamiento preconfigurado que se siguen produciendo en los Servicios de federación de Active Directory (AD FS), siga las instrucciones que se indican en [Solución de problemas de AD FS: eventos y registro](/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-logging#types-of-events). Consulte la documentación del proveedor para saber cómo comprobar esto en los proveedores de federación de terceros.

## <a name="monitoring"></a>Supervisión
Puede supervisar los usuarios y grupos agregados o quitados de los lanzamientos preconfigurados y los inicios de sesión de los usuarios durante el lanzamiento preconfigurado mediante los nuevos libros de autenticación híbrida de Azure Portal.

 ![Libros de autenticación híbrida](./media/how-to-connect-staged-rollout/staged-13.png)

## <a name="remove-a-user-from-staged-rollout"></a>Eliminación de un usuario del lanzamiento preconfigurado

Al eliminar un usuario del grupo, se deshabilita el lanzamiento preconfigurado para ese usuario. Para deshabilitar la característica de lanzamiento preconfigurado, deslice el control de nuevo a **Desactivado**.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P: ¿Se puede usar esta funcionalidad en producción?**

A. Sí, esta característica se puede usar en el inquilino de producción, pero se recomienda que la pruebe primero en el inquilino de prueba.

**P: ¿Se puede usar esta característica para mantener una "coexistencia" permanente, en la que algunos usuarios usan la autenticación federada y otros la autenticación en la nube?**

A. No, esta característica está diseñada para probar la autenticación en la nube. Después de probar con éxito algunos grupos de usuarios, debe pasar a la autenticación en la nube. No se recomienda un estado mixto permanente, ya que este enfoque podría llevar a flujos de autenticación inesperados.

**P: ¿Se puede usar PowerShell para realizar el lanzamiento preconfigurado?**

A. Sí. Para información sobre cómo usar PowerShell para realizar el lanzamiento preconfigurado, consulte [Versión preliminar de Azure AD](/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true#staged_rollout).

## <a name="next-steps"></a>Pasos siguientes
- [Versión preliminar de Azure AD 2.0](/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true#staged_rollout )
- [Cambio del método de inicio de sesión a la sincronización de hash de contraseña](./migrate-from-federation-to-cloud-authentication.md)
- [Cambio del método de inicio de sesión a la autenticación de paso a través](./migrate-from-federation-to-cloud-authentication.md)
- [Guía interactiva de lanzamiento preconfigurado](https://mslearn.cloudguides.com/en-us/guides/Test%20migration%20to%20cloud%20authentication%20using%20staged%20rollout%20in%20Azure%20AD)
