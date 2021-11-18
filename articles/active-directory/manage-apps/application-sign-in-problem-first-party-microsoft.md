---
title: Problemas de inicio de sesión en una aplicación de Microsoft
titleSuffix: Azure AD
description: Solución de problemas comunes que aparecen al iniciar sesión en aplicaciones propias de Microsoft mediante Azure AD (por ejemplo, Microsoft 365).
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: troubleshooting
ms.date: 09/10/2018
ms.author: davidmu
ms.reviewer: alamaral
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2cf35dccf824f2d1f36f79f1ea833464fef5316
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132548508"
---
# <a name="problems-signing-in-to-a-microsoft-application"></a>Problemas de inicio de sesión en una aplicación de Microsoft

Las aplicaciones de Microsoft (como Exchange, SharePoint, Yammer, etc.) se asignan y administran de manera algo diferente que las aplicaciones SaaS de terceros u otras aplicaciones que integra con Azure AD para el inicio de sesión único.

Hay tres maneras principales en que un usuario puede acceder a una aplicación publicada por Microsoft.

- En el caso de aplicaciones de Microsoft 365 u otros conjuntos de aplicaciones de pago, a los usuarios se les concede acceso mediante la **asignación de licencias** o mediante la funcionalidad de asignación de licencias basada en grupo.

- Para aplicaciones que Microsoft o un tercero publica de manera gratuita para que todo el mundo las use, a los usuarios se les puede conceder acceso mediante **consentimiento del usuario**. Esto significa que inician sesión en la aplicación con su cuenta profesional o educativa de Azure AD, que les permite tener acceso a un conjunto limitado de datos en sus cuentas.

- Además, para aplicaciones que Microsoft o un tercero publica de manera gratuita para que todo el mundo las use, a los usuarios se les puede conceder acceso mediante **consentimiento del administrador**. Esto significa que un administrador ha determinado que todos los miembros de la organización pueden usar la aplicación, por lo que inicia sesión en la aplicación con una cuenta de administrador global y concede acceso a todos ellos.

Para solucionar el problema, comience con [Áreas problemáticas generales con el acceso a las aplicaciones para tener en cuenta](#general-problem-areas-with-application-access-to-consider) y luego lea el tutorial: Pasos para solucionar problemas de acceso a las aplicaciones de Microsoft para obtener más información.

## <a name="general-problem-areas-with-application-access-to-consider"></a>Áreas problemáticas generales con el acceso a las aplicaciones para tener en cuenta

Aquí tiene una lista de las áreas problemáticas generales en las que puede profundizar si sabe por dónde empezar. Para una introducción rápida, le recomendamos que lea el tutorial para comenzar rápidamente: Tutorial: Pasos para solucionar problemas de acceso a las aplicaciones de Microsoft.

- [Problemas con la cuenta del usuario](#problems-with-the-users-account)

- [Problemas con grupos](#problems-with-groups)

- [Problemas con las directivas de acceso condicional](#problems-with-conditional-access-policies)

- [Problemas con el consentimiento de la aplicación](#problems-with-application-consent)

## <a name="steps-to-troubleshoot-microsoft-application-access"></a>Pasos para solucionar problemas de acceso a las aplicaciones de Microsoft

Estos son algunos problemas comunes que pueden surgir cuando los usuarios no pueden iniciar sesión en una aplicación de Microsoft.

- Problemas generales para comprobar primero

  * Asegurarse de que el usuario inicia sesión en la **dirección URL correcta** y no en la dirección URL de la aplicación local

  * Que la cuenta del usuario **no está bloqueada**.

  * Asegurarse de que la **cuenta del usuario exista** en Azure Active Directory [Comprobar si existe una cuenta de usuario en Azure Active Directory](#problems-with-the-users-account)

  * Que la cuenta del usuario está **habilitada** para los inicios de sesión. [Comprobar el estado de la cuenta de un usuario](#problems-with-the-users-account)

  * Que la **contraseña del usuario no ha expirado o se ha olvidado**. [Restablecer la contraseña del usuario](#reset-a-users-password) o [habilitar el autoservicio de restablecimiento de contraseña](../authentication/tutorial-enable-sspr.md)

  * Que **Multi-Factor Authentication** no bloquea el acceso del usuario. [Comprobar el estado de la autenticación multifactor de un usuario](#check-a-users-multi-factor-authentication-status) o [comprobar la información de contacto de autenticación de un usuario](#check-a-users-authentication-contact-info)

  * Que una **directiva de acceso condicional** o una directiva de **protección de identidad** no bloquea el acceso del usuario. [Comprobar una directiva de acceso condicional específica](#problems-with-conditional-access-policies) o [comprobar la directiva de acceso condicional de una aplicación específica](#check-a-specific-applications-conditional-access-policy) o [deshabilitar una directiva de acceso condicional específica](#disable-a-specific-conditional-access-policy)

  * Que la **información de contacto de autenticación** del usuario está actualizada para permitir la aplicación de directivas de Multi-Factor Authentication o de acceso condicional. [Comprobar el estado de la autenticación multifactor de un usuario](#check-a-users-multi-factor-authentication-status) o [comprobar la información de contacto de autenticación de un usuario](#check-a-users-authentication-contact-info)

- Para las aplicaciones de **Microsoft** **que necesitan una licencia** (como Office365), estos son algunos problemas específicos que se deben comprobar una vez que haya descartado los problemas generales mencionados anteriormente:

  * Asegurarse de que el usuario tenga una **licencia asignada** [Comprobar las licencias asignadas de un usuario](#check-a-users-assigned-licenses) o [comprobar licencias asignadas de un grupo](#check-a-groups-assigned-licenses)

  * Si la licencia está **asignada a un** **grupo estático**, asegúrese de que el **usuario sea miembro** de dicho grupo. [Comprobación de la pertenencia a grupos de un usuario](#check-a-users-group-memberships)

  * Si la licencia está **asignada a un** **grupo dinámico**, asegúrese de que la **regla del grupo dinámico se haya establecido correctamente**. [Comprobar los criterios de pertenencia de un grupo dinámico](#check-a-dynamic-groups-membership-criteria)

  * Si la licencia está **asignada a un** **grupo dinámico**, asegúrese de que el grupo dinámico haya **terminado de procesar** su pertenencia y que el **usuario sea miembro** (esta operación puede tardar un rato). [Comprobación de la pertenencia a grupos de un usuario](#check-a-users-group-memberships)

  *  Una vez que se haya asegurado de que la licencia está asignada, compruebe que esta **no haya caducado**.

  *  Asegurarse de que la licencia **corresponde a la aplicación** para la que desean acceso.

- Para aplicaciones de **Microsoft** **que no necesitan licencia**, estos son algunos otros puntos para comprobar:

  * Si la aplicación solicita **permisos de nivel de usuario** (por ejemplo, acceso al buzón de este usuario), asegúrese de que el usuario ha iniciado sesión en la aplicación y ha realizado una **operación de consentimiento de nivel de usuario** para permitir que la aplicación acceda a sus datos.

  * Si la aplicación solicita **permisos de nivel de administrador** (por ejemplo, acceso a los buzones de todos los usuarios), asegúrese de que un administrador global haya realizado una **operación de consentimiento de nivel de administrador en nombre de todos los usuarios** de la organización.

## <a name="problems-with-the-users-account"></a>Problemas con la cuenta del usuario

El acceso a la aplicación se puede bloquear debido a un problema con un usuario que se asigna a la aplicación. A continuación, se muestran algunas maneras de solucionar problemas con los usuarios y la configuración de sus cuentas:

- [Comprobar si existe una cuenta de usuario en Azure Active Directory](#check-if-a-user-account-exists-in-azure-active-directory)

- [Comprobar el estado de la cuenta de un usuario](#check-a-users-account-status)

- [Restablecer la contraseña de un usuario](#reset-a-users-password)

- [Habilitar el autoservicio de restablecimiento de contraseña](#enable-self-service-password-reset)

- [Comprobar el estado de la autenticación multifactor de un usuario](#check-a-users-multi-factor-authentication-status)

- [Comprobar la información de contacto de autenticación de un usuario](#check-a-users-authentication-contact-info)

- [Comprobación de la pertenencia a grupos de un usuario](#check-a-users-group-memberships)

- [Comprobar las licencias asignadas de un usuario](#check-a-users-assigned-licenses)

- [Asignar una licencia a un usuario](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Comprobar si existe una cuenta de usuario en Azure Active Directory

Para comprobar si una cuenta de usuario existe, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los usuarios**.

6. **Busque** el usuario en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Compruebe las propiedades del objeto de usuario para asegurarse de que se muestran como esperaba y no falta ningún dato.

### <a name="check-a-users-account-status"></a>Comprobar el estado de la cuenta de un usuario

Para comprobar el estado de la cuenta de un usuario, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los usuarios**.

6. **Busque** el usuario en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Seleccione **Perfil**.

8. En **Configuración**, asegúrese de que **Bloquear inicio de sesión** esté establecido en **No**.

### <a name="reset-a-users-password"></a>Restablecer la contraseña de un usuario

Para restablecer la contraseña de un usuario, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los usuarios**.

6. **Busque** el usuario en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Seleccione el botón **Restablecer contraseña** situada en la parte superior del panel de usuario.

8. Seleccione el botón **Restablecer contraseña** en el panel **Restablecer contraseña** que aparece.

9. Copie la **contraseña temporal** o **escriba una contraseña nueva** para el usuario.

10. Comunique esta nueva contraseña al usuario, quien deberá cambiarla durante el siguiente inicio de sesión en Azure Active Directory.

### <a name="enable-self-service-password-reset"></a>Habilitar el autoservicio de restablecimiento de contraseña

Para habilitar el autoservicio de restablecimiento de contraseña, siga estos pasos de implementación:

- [Permitir que los usuarios restablezcan sus contraseñas de Azure AD](../authentication/tutorial-enable-sspr.md)

- [Permitir que los usuarios restablezcan o cambien sus contraseñas de AD locales](../authentication/tutorial-enable-sspr.md)

### <a name="check-a-users-multi-factor-authentication-status"></a>Comprobar el estado de la autenticación multifactor de un usuario

Para comprobar el estado de la autenticación multifactor de un usuario, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los usuarios**.

6. Seleccione el botón **Autenticación multifactor** en la parte superior del panel.

7. Después de que se haya cargado el **Portal de administración de Multi-factor Authentication**, asegúrese de que se encuentra en la pestaña **Usuarios**.

8. Para encontrar el usuario, filtre u ordene la lista de usuarios, o realice búsquedas en ella.

9. Seleccione el usuario de la lista de usuarios y **habilite**, **deshabilite** o **exija** autenticación multifactor, según desee.

   * **Nota**: Si un usuario se encuentra en un estado **Exigido**, puede establecerlo en **Deshabilitado** temporalmente para permitirle volver a su cuenta. Una vez de vuelta, puede cambiar de nuevo su estado a **Habilitado** para solicitarle que vuelva a registrar su información de contacto durante su siguiente inicio de sesión. Como alternativa, puede seguir los pasos descritos en [Comprobar la información de contacto de autenticación de un usuario](#check-a-users-authentication-contact-info) para comprobar o establecer estos datos por ellos.

### <a name="check-a-users-authentication-contact-info"></a>Comprobar la información de contacto de autenticación de un usuario

Para comprobar la información de contacto de autenticación de un usuario para Multi-Factor Authentication, acceso condicional, protección de identidad y restablecimiento de contraseña, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los usuarios**.

6. **Busque** el usuario en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Seleccione **Perfil**.

8. Desplácese hacia abajo hasta **Información de contacto para la autenticación**.

9. **Revise** los datos registrados para el usuario y actualícelos si es necesario.

### <a name="check-a-users-group-memberships"></a>Comprobación de la pertenencia a grupos de un usuario

Para comprobar la pertenencia a grupos de un usuario, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los usuarios**.

6. **Busque** el usuario en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Seleccione **Grupos** para ver de cuáles es miembro el usuario.

### <a name="check-a-users-assigned-licenses"></a>Comprobar las licencias asignadas de un usuario

Para comprobar las licencias asignadas de un usuario, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los usuarios**.

6. **Busque** el usuario en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Seleccione **Licencias** para ver cuáles tiene asignadas actualmente el usuario.

### <a name="assign-a-user-a-license"></a>Asignar una licencia a un usuario

Para asignar una licencia a un usuario, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los usuarios**.

6. **Busque** el usuario en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Seleccione **Licencias** para ver cuáles tiene asignadas actualmente el usuario.

8. Seleccione el botón **Asignar**.

9. Seleccione **uno o más productos** en la lista de productos disponibles.

10. **Opcional**: Haga clic en el elemento **Opciones de asignación** para asignar productos de forma granular. Cuando haya finalizado este procedimiento, haga clic en **Aceptar**.

11. Seleccione el botón **Asignar** para asignar estas licencias a este usuario.

## <a name="problems-with-groups"></a>Problemas con grupos

El acceso a la aplicación se puede bloquear debido a un problema con un grupo que se asigna a la aplicación. A continuación, se muestran algunas maneras de solucionar problemas con grupos y pertenencias a grupos:

- [Comprobar la pertenencia de un grupo](#check-a-groups-membership)

- [Comprobar los criterios de pertenencia de un grupo dinámico](#check-a-dynamic-groups-membership-criteria)

- [Comprobar las licencias asignadas de un grupo](#check-a-groups-assigned-licenses)

- [Volver a procesar las licencias de un grupo](#reprocess-a-groups-licenses)

- [Asignar una licencia a un grupo](#assign-a-group-a-license)

### <a name="check-a-groups-membership"></a>Comprobar la pertenencia de un grupo

Para comprobar la pertenencia de un grupo, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los grupos**.

6. **Busque** el grupo en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Seleccione **Miembros** para revisar la lista de usuarios asignados a este grupo.

### <a name="check-a-dynamic-groups-membership-criteria"></a>Comprobar los criterios de pertenencia de un grupo dinámico

Para comprobar los criterios de pertenencia de un grupo dinámico, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los grupos**.

6. **Busque** el grupo en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Seleccione **Reglas de pertenencia dinámica**.

8. Revise la regla **simple** o **avanzada** definida para este grupo y asegúrese de que el usuario que va a ser miembro de este grupo cumpla estos criterios.

### <a name="check-a-groups-assigned-licenses"></a>Comprobar licencias asignadas de un grupo

Para comprobar las licencias asignadas de un grupo, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los grupos**.

6. **Busque** el grupo en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Haga clic en **Licencias** para ver qué licencias tiene asignadas actualmente el grupo.

### <a name="reprocess-a-groups-licenses"></a>Volver a procesar las licencias de un grupo

Para volver a procesar las licencias asignadas de un grupo, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los grupos**.

6. **Busque** el grupo en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Haga clic en **Licencias** para ver qué licencias tiene asignadas actualmente el grupo.

8. Haga clic en el botón **Reprocesar** para asegurarse de que las licencias asignadas a los miembros de este grupo están actualizadas. Esta operación puede tardar mucho tiempo, en función del tamaño y la complejidad del grupo.

   >[!NOTE]
   >Para acelerar el proceso, considere la posibilidad de asignar temporalmente una licencia al usuario directamente. [Asignar una licencia a un usuario](#problems-with-application-consent)
   >
   >

### <a name="assign-a-group-a-license"></a>Asignar una licencia a un grupo

Para asignar una licencia a un grupo, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Usuarios y grupos** en el menú de navegación.

5. Seleccione **Todos los grupos**.

6. **Busque** el grupo en el que está interesado y **haga clic en la fila** para seleccionarlo.

7. Haga clic en **Licencias** para ver qué licencias tiene asignadas actualmente el grupo.

8. Seleccione el botón **Asignar**.

9. Seleccione **uno o más productos** en la lista de productos disponibles.

10. **Opcional**: Haga clic en el elemento **Opciones de asignación** para asignar productos de forma granular. Cuando haya finalizado este procedimiento, haga clic en **Aceptar**.

11. Seleccione el botón **Asignar** para asignar estas licencias a este grupo. Esta operación puede tardar mucho tiempo, en función del tamaño y la complejidad del grupo.

    >[!NOTE]
    >Para acelerar el proceso, considere la posibilidad de asignar temporalmente una licencia al usuario directamente. [Asignar una licencia a un usuario](#problems-with-application-consent)
    >
    >

## <a name="problems-with-conditional-access-policies"></a>Problemas con las directivas de acceso condicional

### <a name="check-a-specific-conditional-access-policy"></a>Comprobación de una directiva de acceso condicional específica

Para comprobar o validar una directiva de acceso condicional:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú de navegación.

5. Seleccione el elemento de navegación **Acceso condicional**.

6. Seleccione la directiva que le interese inspeccionar.

7. Revise que no haya condiciones, asignaciones u otras configuraciones específicas que puedan estar bloqueando el acceso del usuario.

   >[!NOTE]
   >Puede que desee deshabilitar temporalmente esta directiva para asegurarse de que no afecta a los inicios de sesión. Para ello, establezca el botón de alternancia **Habilitar directiva** en **No** y haga clic en el botón **Guardar**.
   >
   >

### <a name="check-a-specific-applications-conditional-access-policy"></a>Comprobación de la directiva de acceso condicional de una aplicación específica

Para comprobar o validar la directiva de acceso condicional configurada actualmente de una aplicación, siga estos pasos:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú de navegación.

5. Seleccione **Todas las aplicaciones**.

6. Busque la aplicación que le interesa o en la que el usuario intenta iniciar sesión por el nombre para mostrar o el identificador de la aplicación.

     >[!NOTE]
     >Si no ve la aplicación que busca, haga clic en el botón **Filtrar** y amplíe el ámbito de la lista a **Todas las aplicaciones**. Si desea ver más columnas, haga clic en el botón **Columnas** para agregar detalles adicionales para las aplicaciones.
     >
     >

7. Seleccione el elemento de navegación **Acceso condicional**.

8. Seleccione la directiva que le interese inspeccionar.

9. Revise que no haya condiciones, asignaciones u otras configuraciones específicas que puedan estar bloqueando el acceso del usuario.

     >[!NOTE]
     >Puede que desee deshabilitar temporalmente esta directiva para asegurarse de que no afecta a los inicios de sesión. Para ello, establezca el botón de alternancia **Habilitar directiva** en **No** y haga clic en el botón **Guardar**.
     >
     >

### <a name="disable-a-specific-conditional-access-policy"></a>Deshabilitación de una directiva de acceso condicional específica

Para comprobar o validar una directiva de acceso condicional:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **extensión de Azure Active Directory** haciendo clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo principal.

3. Escriba **"Azure Active Directory**" en el cuadro de búsqueda de filtrado y seleccione el elemento **Azure Active Directory**.

4. Seleccione **Aplicaciones empresariales** en el menú de navegación.

5. Seleccione el elemento de navegación **Acceso condicional**.

6. Seleccione la directiva que le interese inspeccionar.

7. Para deshabilitar la directiva, establezca el botón de alternancia **Habilitar directiva** en **No** y haga clic en el botón **Guardar**.

## <a name="problems-with-application-consent"></a>Problemas con el consentimiento de aplicación

El acceso a la aplicación se puede bloquear porque no se haya producido la operación de consentimiento de los permisos adecuados. A continuación, se muestran algunas maneras de solucionar problemas relacionados con el consentimiento de aplicación:

- [Realizar una operación de consentimiento de nivel de usuario](#perform-a-user-level-consent-operation)

- [Realizar la operación de consentimiento de nivel de administrador para cualquier aplicación](#perform-administrator-level-consent-operation-for-any-application)

- [Realizar el consentimiento de nivel de administrador para una aplicación de un solo inquilino](#perform-administrator-level-consent-for-a-single-tenant-application)

- [Realizar el consentimiento de nivel de administrador para una aplicación multiinquilino](#perform-administrator-level-consent-for-a-multi-tenant-application)

### <a name="perform-a-user-level-consent-operation"></a>Realizar una operación de consentimiento de nivel de usuario

- Para cualquier aplicación compatible con Open ID Connect que solicite permisos, al navegar a la pantalla de inicio de sesión de la aplicación se realiza un consentimiento de nivel de usuario a la aplicación para el usuario que inició sesión.

- Si desea realizar esto mediante programación, consulte [Solicitud de consentimiento de usuario individual](../develop/v2-permissions-and-consent.md#requesting-individual-user-consent).

### <a name="perform-administrator-level-consent-operation-for-any-application"></a>Realizar la operación de consentimiento de nivel de administrador para cualquier aplicación

- **Solo para las aplicaciones desarrolladas mediante el modelo de aplicación V1**, puede forzar que se produzca el consentimiento de nivel de administrador si agrega " **?prompt=admin\_consent**" al final de la dirección URL de inicio de sesión de una aplicación.

- Para **todas las aplicaciones desarrolladas mediante el modelo de aplicación V2**, puede forzar que se produzca este consentimiento de nivel de administrador si sigue las instrucciones de la sección **Solicitud de los permisos de un administrador de directorios** de [Uso del punto de conexión de consentimiento del administrador](../develop/v2-permissions-and-consent.md#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-single-tenant-application"></a>Realizar el consentimiento de nivel de administrador para una aplicación de un único inquilino

- Con las **aplicaciones de un único inquilino** que solicitan permisos (por ejemplo, las que desarrolla o posee su organización), puede realizar una operación de **consentimiento de nivel administrativo** en nombre de todos los usuarios. Para ello, inicie sesión como administrador global y haga clic en el botón **Conceder permisos** de la parte superior del panel **Registro de aplicación -&gt; Todas las aplicaciones -&gt; Seleccionar una aplicación - &gt; Permisos necesarios**.

- Para **todas las aplicaciones desarrolladas mediante el modelo de aplicación V1 o V2**, puede forzar que se produzca este consentimiento de nivel de administrador si sigue las instrucciones de la sección **Solicitud de los permisos de un administrador de directorios** de [Uso del punto de conexión de consentimiento del administrador](../develop/v2-permissions-and-consent.md#using-the-admin-consent-endpoint).

### <a name="perform-administrator-level-consent-for-a-multi-tenant-application"></a>Realizar el consentimiento de nivel de administrador para una aplicación multiinquilino

- Para **aplicaciones multiinquilino** que solicitan permisos (como una aplicación que desarrolla un tercero o Microsoft), puede realizar una operación de **consentimiento de nivel administrativo**. Inicie sesión como administrador global y haga clic en el botón **Conceder permisos** situado en el panel **Aplicaciones empresariales -&gt; Todas las aplicaciones -&gt; Seleccionar una aplicación -&gt; Permisos** (disponible pronto).

- También puede exigir que se produzca este consentimiento de nivel de administrador si sigue las instrucciones que aparecen en la sección **Solicitud de los permisos de un administrador de directorios** de [Uso del punto de conexión de consentimiento del administrador](../develop/v2-permissions-and-consent.md#using-the-admin-consent-endpoint).

## <a name="next-steps"></a>Pasos siguientes

[Uso del punto de conexión de consentimiento del administrador](../develop/v2-permissions-and-consent.md#using-the-admin-consent-endpoint)
