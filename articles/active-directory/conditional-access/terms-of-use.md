---
title: 'Términos de uso: Azure Active Directory | Microsoft Docs'
description: Introducción al uso de Términos de uso de Azure Active Directory para presentar información a empleados o invitados antes de obtener acceso.
services: active-directory
ms.service: active-directory
ms.subservice: compliance
ms.topic: how-to
ms.date: 07/12/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: 754520be35cbac4321fb7fbb65135016642a7ee1
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131018326"
---
# <a name="azure-active-directory-terms-of-use"></a>Términos de uso de Azure Active Directory

Las directivas de los términos de uso de Azure AD ofrecen un método sencillo que las organizaciones pueden usar para presentar información a los usuarios finales. Esta presentación garantiza que los usuarios ven las declinaciones de responsabilidades pertinentes de los requisitos legales o de cumplimiento. En este artículo se describe cómo empezar a trabajar con las directivas de los términos de uso.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Vídeos de introducción

El vídeo siguiente ofrece una introducción rápida a las directivas de los términos de uso.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Para obtener más vídeos, consulte:
- [Implementación de una directiva de los términos de uso de Azure Active Directory](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Lanzamiento de una directiva de los términos de uso de Azure Active Directory](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>¿Qué puedo hacer con los Términos de uso?

Las directivas de los términos de uso de Azure AD tienen las siguientes funcionalidades:

- Requerir a empleados o invitados que acepten la directiva de los términos de uso antes de obtener acceso.
- Requerir a empleados o invitados que acepten la directiva de los términos de uso en todos los dispositivos antes de obtener acceso.
- Requerir a empleados o invitados que acepten la directiva de los términos de uso en una programación periódica.
- Requerir a empleados o invitados que acepten la directiva de los términos de uso antes de registrar información de seguridad en Azure AD Multi-Factor Authentication (MFA).
- Requerir a empleados o invitados que acepten la directiva de los términos de uso antes de registrar información de seguridad en el autoservicio de restablecimiento de contraseña (SSPR) de Azure AD.
- Presentar una directiva de términos de uso generales para todos los usuarios de la organización.
- Presentar directivas de condiciones de uso específicas basadas en los atributos de un usuario (como médicos frente a enfermeras, o empleados nacionales frente a internacionales) utilizando [grupos dinámicos](../enterprise-users/groups-dynamic-membership.md)).
- Presentar directivas de términos de uso específicas al acceder a aplicaciones con una alta repercusión en la empresa, como Salesforce.
- Presentar directivas de términos de uso en diferentes idiomas.
- Mostrar quién ha aceptado o no las directivas de términos de uso.
- Ayudar a cumplir los reglamentos de privacidad.
- Mostrar un registro de la actividad de la directiva de los términos de uso a efectos de cumplimiento y auditoría.
- Crear y administrar las directivas de los términos de uso mediante [Microsoft Graph API](/graph/api/resources/agreement) (actualmente en versión preliminar).

## <a name="prerequisites"></a>Prerrequisitos

Para usar y configurar las directivas de los términos de uso de Azure AD, debe cumplir los siguientes requisitos:

- Suscripción a Azure AD Premium P1, P2, EMS E3 o EMS E5.
   - Si no tiene una de estas suscripciones, puede [obtener Azure AD Premium](../fundamentals/active-directory-get-started-premium.md) o [habilitar la evaluación gratuita de Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).
- Una de las siguientes cuentas de administrador para el directorio que quiera configurar:
   - Administrador global
   - Administrador de seguridad
   - Administrador de acceso condicional

## <a name="terms-of-use-document"></a>Documento de términos de uso

Las directivas de los términos de uso de Azure AD emplean el formato PDF para presentar el contenido. Este archivo PDF puede tener cualquier contenido, como documentos de contratos existentes, lo que le permite recopilar acuerdos de usuario final durante el inicio de sesión de usuario. Para admitir usuarios en dispositivos móviles, el tamaño de fuente recomendado en el PDF es 24 puntos.

## <a name="add-terms-of-use"></a>Agregar términos de uso

Una vez que haya finalizado el documento de la directiva de los términos de uso, use el procedimiento siguiente para agregarlo.

1. Inicie sesión en Azure como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

    ![Acceso condicional: hoja de Términos de uso](./media/terms-of-use/tou-blade.png)

1. Haga clic en **Nuevos términos**.

    ![Panel de Nuevos términos de uso para especificar la configuración de los términos de uso](./media/terms-of-use/new-tou.png)

1. En el cuadro **Nombre**, escriba un nombre para la directiva de los términos de uso que se usará en Azure Portal.
1. En el cuadro **Nombre para mostrar**, escriba un título que vean los usuarios cada vez que inicien sesión.
1. En **Documento Términos de uso**, vaya a su PDF finalizado de la directiva de los términos de uso y selecciónelo.
1. Seleccione el idioma para su documento de la directiva de los términos de uso. La opción de idioma permite cargar varias directivas de términos de uso, cada una con un idioma diferente. La versión de la directiva de los términos de uso que verá un usuario final se basará en sus preferencias del explorador.
1. Para requerir a los usuarios finales que vean la directiva de los términos de uso antes de aceptarlos, establezca **Requerir a los usuarios que expandan los términos de uso** en **Activado**.
1. Para requerir a los usuarios finales que acepten la directiva de los términos de uso en todos los dispositivos desde los que obtienen acceso, establezca **Requerir que los usuarios concedan su consentimiento en todos los dispositivos** en **Activado**. Es posible que los usuarios deban instalar aplicaciones adicionales si esta opción está habilitada. Para obtener más información, consulte [Términos de uso por dispositivo](#per-device-terms-of-use).
1. Si quiere hacer expirar consentimientos de la directiva de los términos de uso según una programación, establezca **Expirar consentimientos** en **Activado**. Al establecerse en Activado, se muestran dos opciones de configuración de la programación adicionales.

    ![Configuración de Expirar autorizaciones para establecer la fecha de inicio, la frecuencia y la duración](./media/terms-of-use/expire-consents.png)

1. Use las opciones de configuración **Expirar a partir del** y **Frecuencia** para especificar la programación para las expiraciones de la directiva de los términos de uso. En la siguiente tabla se muestra el resultado de un par de opciones de configuración de ejemplo:

   | Expirar a partir del | Frecuencia | Resultado |
   | --- | --- | --- |
   | La fecha de hoy  | Mensual | A partir de hoy, los usuarios deben aceptar la directiva de los términos de uso y, después, volver a aceptarlos cada mes. |
   | Fecha del futuro  | Mensual | A partir de hoy, los usuarios deben aceptar la directiva de los términos de uso. Cuando la fecha del futuro llegue, expirarán las autorizaciones y, luego, los usuarios deberán volver a aceptar cada mes.  |

   Por ejemplo, si establece la fecha de inicio de la expiración en **1 de enero** y la frecuencia en **Mensual**, este es el modo en que pueden producirse las expiraciones para dos usuarios:

   | Usuario | Primera fecha de aceptación | Primera fecha de expiración | Segunda fecha de expiración | Tercera fecha de expiración |
   | --- | --- | --- | --- | --- |
   | Alice | 1 ene | 1 feb | 1 mar | 1 abr |
   | Bob | 15 ene | 1 feb | 1 mar | 1 abr |

1. Use la opción **Duración antes de solicitar que se acepten las condiciones de nuevo (días)** para especificar el número de días antes de que el usuario tenga que volver a aceptar la directiva de los términos de uso. Esto permite a los usuarios seguir su propia programación. Por ejemplo, si establece la duración en **30** días, este es el modo en que pueden producirse las expiraciones para dos usuarios:

   | Usuario | Primera fecha de aceptación | Primera fecha de expiración | Segunda fecha de expiración | Tercera fecha de expiración |
   | --- | --- | --- | --- | --- |
   | Alice | 1 ene | 31 ene | 2 mar | 1 abr |
   | Bob | 15 ene | 14 feb | 16 mar | 15 abr |

   Es posible usar las opciones de configuración **Expirar autorizaciones** y **Duración antes de solicitar que se acepten las condiciones de nuevo (días)** de forma conjunta, pero normalmente usará una o la otra.

1. En **Acceso condicional**, use la lista **Exigir con plantillas de directiva de acceso condicional** para seleccionar la plantilla para exigir la directiva de los términos de uso.

    ![Lista de desplegable de acceso condicional para seleccionar una plantilla de directiva](./media/terms-of-use/conditional-access-templates.png)

   | Plantilla | Descripción |
   | --- | --- |
   | **Acceso a las aplicaciones en la nube para todos los invitados** | Se creará una directiva de acceso condicional para todos los invitados y todas las aplicaciones en la nube. Esta directiva afecta a Azure Portal. Una vez que se crea, es posible que se le pida cerrar e iniciar sesión. |
   | **Acceso a las aplicaciones en la nube para todos los usuarios** | Se creará una directiva de acceso condicional para todos los usuarios y todas las aplicaciones en la nube. Esta directiva afecta a Azure Portal. Una vez que se crea, se le pedirá que cierre e inicie sesión. |
   | **Directiva personalizada** | Seleccione los usuarios, los grupos y las aplicaciones a los que se aplicará esta directiva de los términos de uso. |
   | **Crear directiva de acceso condicional posteriormente** | Esta directiva de los términos de uso aparecerá en la lista de control de concesiones al crear una directiva de acceso condicional. |

   >[!IMPORTANT]
   >Los controles de la directiva de acceso condicional (incluidas las directivas de los términos de uso) no admiten el cumplimiento en las cuentas de servicio. Se recomienda excluir todas las cuentas de servicio de la directiva de acceso condicional.

    Las directivas personalizadas de acceso condicional permiten directivas de términos de uso pormenorizadas, hasta una aplicación en la nube o un grupo de usuarios específicos. Para más información, consulte [Inicio rápido: Solicitar la aceptación de los términos de uso antes de acceder a aplicaciones en la nube](require-tou.md).

1. Haga clic en **Crear**.

    Si ha seleccionado una plantilla de acceso condicional personalizada, aparece entonces una nueva pantalla que le permite crear la directiva de acceso condicional personalizada.

   ![Panel Nuevo acceso condicional si eligió la plantilla de directiva de acceso condicional personalizada](./media/terms-of-use/custom-policy.png)

   Ahora debería ver las nuevas directivas de los términos de uso.

   ![Nuevos términos de uso incluidos en la hoja Términos de uso](./media/terms-of-use/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Ver quién los ha aceptado y rechazado

La hoja Términos de uso muestra un recuento de los usuarios que los han aceptado y rechazado. Estos recuentos, y quién aceptó o rechazó la directiva de los términos de uso, se almacenan durante la vigencia de dicha directiva.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).

    ![Hoja Términos de uso que indica el número de usuarios que han aceptado o rechazado los términos de uso](./media/terms-of-use/view-tou.png)

1. En una directiva de los términos de uso, haga clic en los números situados en **Aceptado** o **Rechazado** para ver el estado actual de los usuarios.

    ![Panel Autorizaciones de términos de uso que indica los usuarios que han aceptado los términos de uso](./media/terms-of-use/accepted-tou.png)

1. Para ver el historial de un usuario individual, haga clic en los puntos suspensivos ( **...** ) y, a continuación, en **Ver historial**.

    ![Menú contextual Ver historial de un usuario](./media/terms-of-use/view-history-menu.png)

   En el panel Ver historial, puede ver un historial de todas las aceptaciones, rechazos y expiraciones.

   ![Panel Ver historial que muestra el historial de aceptaciones, rechazos y la expiraciones de un usuario](./media/terms-of-use/view-history-pane.png)

## <a name="view-azure-ad-audit-logs"></a>Visualización de registros de auditoría de Azure AD

Si quiere ver la actividad adicional, las directivas de los términos de uso de Azure AD incluyen registros de auditoría. Cada consentimiento de usuario desencadena un evento en los registros de auditoría que se almacena durante **30 días**. Puede ver estos registros en el portal o descargarlos como un archivo .csv.

Para empezar a trabajar con los registros de auditoría de Azure AD, use el procedimiento siguiente:

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).
1. Seleccione una directiva de términos de uso.
1. Haga clic en **Ver registros de auditoría**.

    ![Hoja Términos de uso con la opción Ver registros de auditoría resaltada](./media/terms-of-use/audit-tou.png)

1. En la pantalla de registros de auditoría de Azure AD, puede filtrar la información mediante las listas proporcionadas para seleccionar información de registro de auditoría específica.

    También puede hacer clic en **Descargar** para descargar la información en un archivo .csv y usarla localmente.

   ![Pantalla de Registros de auditoría de Azure AD que muestra las opciones de Fecha, Directiva de destino, Iniciado por y Actividad](./media/terms-of-use/audit-logs-tou.png)

   Si hace clic en un registro, aparecerá un panel con detalles de actividad adicionales.

   ![Detalles de actividad para un registro que muestra las opciones de Actividad, Estado de actividad, Iniciada por y Directiva de destino](./media/terms-of-use/audit-log-activity-details.png)

## <a name="what-terms-of-use-looks-like-for-users"></a>Cómo aparecen los términos de uso para los usuarios

Una vez que se crea y aplica la directiva de los términos de uso, los usuarios, que están dentro del ámbito, verán la siguiente pantalla durante el inicio de sesión.

![Ejemplo de términos de uso que aparece cuando un usuario inicia sesión](./media/terms-of-use/user-tou.png)

Los usuarios pueden ver la directiva de los términos de uso y, si es necesario, usar los botones para acercar y alejar.

![Vista de los términos de uso con los botones de zoom](./media/terms-of-use/zoom-buttons.png)

En la pantalla siguiente se muestra cómo aparece la directiva de los términos de uso en los dispositivos móviles.

![Ejemplo de términos de uso que aparece cuando un usuario inicia sesión en un dispositivo móvil](./media/terms-of-use/mobile-tou.png)

Los usuarios solo tienen que aceptar la directiva de los términos de uso una vez y ya no volverán a verla en posteriores inicios de sesión.

### <a name="how-users-can-review-their-terms-of-use"></a>Cómo los usuarios pueden revisar los términos de uso

Los usuarios pueden revisar y consultar las directivas de los términos de uso que han aceptado mediante el procedimiento siguiente.

1. Inicie sesión en [https://myaccount.microsoft.com/](https://myaccount.microsoft.com/).
1. Seleccione **Settings & Privacy** (Configuración y privacidad).
1. Seleccione **Privacidad**.
1. En **Organization's notice** (Aviso de la organización), seleccione **View** (Ver) junto a la declaración de las condiciones de uso que desea revisar.

## <a name="edit-terms-of-use-details"></a>Edición de los detalles de los términos de uso

Algunos de los detalles de las directivas de los términos de uso se pueden editar, pero no se puede modificar un documento existente. El siguiente procedimiento describe cómo editar dichos detalles.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).
1. Seleccione la directiva de los términos de uso que quiere editar.
1. Haga clic en **Editar términos**.
1. En el panel Edición de condiciones de uso, puede cambiar lo siguiente:
    - **Nombre**: es el nombre interno de las condiciones de uso que no se comparten con los usuarios finales.
    - **Nombre para mostrar**: este es el nombre que los usuarios finales pueden observar al ver las condiciones de uso.
    - **Requerir a los usuarios que expandan las condiciones de uso**: si se **Activa** esta opción, el usuario final tendrá que expandir el documento de la directiva de las condiciones de uso antes de aceptarla.
    - (Versión preliminar) Puede **actualizar un documento de condiciones de uso ya existente**.
    - Puede agregar un idioma a un documento de condiciones de uso ya existente.

   Si hay otras opciones de configuración que quiere cambiar, como documento PDF, requerir que los usuarios concedan su consentimiento en todos los dispositivos, expirar autorizaciones, duración antes de solicitar que se acepten los términos de nuevo o directiva de acceso condicional, debe crear una nueva instancia de términos de uso.

    ![Edición que muestra diferentes opciones de idioma ](./media/terms-of-use/edit-terms-use.png)

1. Cuando haya terminado, haga clic en **Guardar** para guardar los cambios.

## <a name="update-the-version-or-pdf-of-an-existing-terms-of-use"></a>Actualización de la versión o PDF de las condiciones de uso existentes

1.  Inicie sesión en Azure y vaya a [Condiciones de uso](https://aka.ms/catou).
2.  Seleccione la directiva de los términos de uso que quiere editar.
3.  Haga clic en **Editar términos**.
4.  En el idioma en el que desea actualizar a una nueva versión, haga clic en **Actualizar** en la columna de acción.

    ![Panel Edición de términos de uso que muestra las opciones de nombre y expansión](./media/terms-of-use/edit-terms-use.png)

5.  En el panel de la derecha, cargue el PDF de la nueva versión.
6.  También hay una opción de alternancia aquí, **Require reaccept** (Requerir nueva aceptación), en caso de que desee requerir a los usuarios que acepten esta nueva versión la próxima vez que inicien sesión. Si activa esta opción, la próxima vez que intenten acceder al recurso definido en la directiva de acceso condicional, se les pedirá que acepten esta nueva versión. Si no requiere una nueva aceptación por parte de los usuarios, su consentimiento anterior seguirá siendo el actual y solo los usuarios nuevos que no hayan dado su consentimiento anteriormente o cuyo consentimiento expire verán la nueva versión. Hasta que expire la sesión, **Require reaccept** (Requerir nueva aceptación) no exige que los usuarios acepten las nuevas condiciones de uso. Si desea asegurarse de que vuelve a aceptar, elimine o vuelva a crear o cree nuevas condiciones de uso para este caso.

    ![Edición de condiciones de uso con la opción de nueva aceptación resaltada](./media/terms-of-use/re-accept.png)

7.  Una vez que haya cargado el nuevo PDF y elegido volver a aceptar, haga clic en Agregar en la parte inferior del panel.
8.  Ahora verá la versión más reciente en la columna Documento.

## <a name="view-previous-versions-of-a-tou"></a>Visualización de versiones anteriores de los términos de uso

1.  Inicie sesión en Azure y vaya a **Términos de uso** en https://aka.ms/catou.
2.  Seleccione la directiva de los términos de uso de la que desea ver un historial de versiones.
3.  Haga clic en **Idiomas e historial de versiones**.
4.  Haga clic en **See previous versions** (Ver versiones anteriores).

    ![Detalles del documento que incluyen las versiones de idioma](./media/terms-of-use/document-details.png)

5.  Puede hacer clic en el nombre del documento para descargar esa versión.

## <a name="see-who-has-accepted-each-version"></a>Ver quién ha aceptado cada versión

1.  Inicie sesión en Azure y vaya a **Términos de uso** en https://aka.ms/catou.
2.  Para ver quién ha aceptado actualmente las condiciones de uso, haga clic en el número que aparece debajo de la columna **Aceptado** de las condiciones de uso que desee.
3.  De forma predeterminada, en la página siguiente se muestra el estado actual de aceptación de las condiciones de uso por parte de cada usuario.
4.  Si desea ver los eventos de consentimiento anteriores, puede seleccionar **Todos** en la lista desplegable **Estado actual**. Ahora puede ver los eventos detallados de cada usuario sobre cada versión y qué sucedió.
5.  Como alternativa, puede seleccionar una versión específica de la lista desplegable **Versión** para ver quién ha aceptado esa versión específica.


## <a name="add-a-tou-language"></a>Adición de un idioma de términos de uso

El siguiente procedimiento describe cómo agregar un idioma de términos de uso.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).
1. Seleccione la directiva de los términos de uso que quiere editar.
1. Haga clic en **Editar términos**.
1. Haga clic en **Agregar idioma** en la parte inferior de la página.
1. En el panel para agregar idioma de los términos de uso, cargue el PDF localizado y seleccione el idioma.

    ![Panel de Términos de uso seleccionados y que muestra la pestaña Idiomas en los detalles](./media/terms-of-use/select-language.png)

1. Haga clic en **Agregar idioma**.
1. Haga clic en **Guardar**

1. Haga clic en **Agregar** para agregar el idioma.

## <a name="per-device-terms-of-use"></a>Términos de uso por dispositivo

La opción de configuración **Requerir que los usuarios concedan su consentimiento en todos los dispositivos** le permite requerir a los usuarios finales que acepten la directiva de los términos de uso en todos los dispositivos desde los que obtienen acceso. Al usuario final se le pedirá que registre su dispositivo en Azure AD. Cuando el dispositivo se registra, el identificador de dispositivo se usa para aplicar la directiva de los términos de uso en cada dispositivo.

Esta es una lista de las plataformas y el software admitidos.

> [!div class="mx-tableFixed"]
> |  | iOS | Android | Windows 10 | Otros |
> | --- | --- | --- | --- | --- |
> | **Aplicación nativa** | Sí | Sí | Sí |  |
> | **Microsoft Edge** | Sí | Sí | Sí |  |
> | **Internet Explorer** | Sí | Sí | Sí |  |
> | **Chrome (con extensión)** | Sí | Sí | Sí |  |

Términos de uso por dispositivo tiene las siguientes restricciones:

- Un dispositivo solo puede conectarse a un inquilino.
- Un usuario debe tener permisos para conectarse a su dispositivo.
- No se admite la aplicación de inscripción de Intune. Asegúrese de que se excluye de cualquier directiva de acceso condicional que requiera la directiva de los términos de uso.
- No se admiten usuarios de Azure AD B2B.

Si el dispositivo del usuario no se une, recibirá un mensaje indicándole que debe conectarse a su dispositivo. Su experiencia dependerá de la plataforma y el software.

### <a name="join-a-windows-10-device"></a>Conexión a un dispositivo con Windows 10

Si un usuario usa Windows 10 y Microsoft Edge, recibirá un mensaje similar al siguiente para [conectarse a su dispositivo](https://support.microsoft.com/account-billing/join-your-work-device-to-your-work-or-school-network-ef4d6adb-5095-4e51-829e-5457430f3973#to-join-an-already-configured-windows-10-device).

![Windows 10 y Microsoft Edge: mensaje que indica que el dispositivo debe estar registrado](./media/terms-of-use/per-device-win10-edge.png)

Si usa Chrome, se le pedirá que instale la [extensión de cuentas de Windows 10](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

### <a name="register-an-ios-device"></a>Registro en un dispositivo iOS

Si un usuario utiliza un dispositivo iOS, se le pedirá que instale la [aplicación Microsoft Authenticator](https://apps.apple.com/us/app/microsoft-authenticator/id983156458).

### <a name="register-an-android-device"></a>Registro de un dispositivo Android

Si un usuario usa un dispositivo Android, se le pedirá que instale la [aplicación Microsoft Authenticator](https://play.google.com/store/apps/details?id=com.azure.authenticator).

### <a name="browsers"></a>Exploradores

Si un usuario utiliza un explorador que no se admite, se le pedirá que use otro explorador.

![Mensaje que indica que el dispositivo debe estar registrado, pero que no se admite el explorador](./media/terms-of-use/per-device-browser-unsupported.png)

## <a name="delete-terms-of-use"></a>Eliminar términos de uso

Puede eliminar las directivas de los términos de uso anteriores mediante el procedimiento siguiente.

1. Inicie sesión en Azure y vaya a **Términos de uso** en [https://aka.ms/catou](https://aka.ms/catou).
1. Seleccione la directiva de los términos de uso que quiere eliminar.
1. Haga clic en **Eliminar términos**.
1. En el mensaje que le pregunta si desea continuar, haga clic en **Sí**.

    ![Mensaje de confirmación para eliminar los términos de uso](./media/terms-of-use/delete-tou.png)

   Ahora no debería ver la directiva de los términos de uso.

## <a name="user-acceptance-record-deletion"></a>Eliminación de registros de aceptación del usuario

Los registros de aceptación del usuario se eliminan:

- Cuando el administrador elimina explícitamente los términos de uso. Cuando esto sucede, también se eliminan todos los registros de aceptación asociados a esos términos de uso específicos.
- Cuando el inquilino pierde su licencia de Azure Active Directory Premium.
- Cuando se elimina el inquilino.

## <a name="policy-changes"></a>Cambios de directiva

Las directivas de acceso condicional surten efecto de inmediato. Cuando esto sucede, el administrador empezará a ver "nubes tristes" o "problemas de token de Azure AD". El administrador debe cerrar sesión e iniciarla de nuevo para satisfacer la nueva directiva.

> [!IMPORTANT]
> Los usuarios dentro del ámbito necesitarán cerrar sesión y volver a iniciarla para satisfacer una directiva nueva si:
>
> - se ha habilitado una directiva de acceso condicional en una directiva de los términos de uso,
> - o bien se crea una segunda directiva de los términos de uso.

## <a name="b2b-guests"></a>Invitados B2B

La mayoría de las organizaciones tienen un proceso en marcha para que sus empleados den su consentimiento para la directiva de los términos de uso y las declaraciones de privacidad de su organización. Sin embargo, ¿cómo puede forzar las mismas autorizaciones para los invitados negocio a negocio (B2B) de Azure AD cuando se agregan a través de SharePoint o Teams? Al usar el acceso condicional y las directivas de los términos de uso, puede forzar una directiva directamente hacia los usuarios invitados B2B. Durante el proceso de canje de la invitación, se presenta al usuario la directiva de los términos de uso. Esta compatibilidad se encuentra actualmente en versión preliminar.

Las directivas de los términos de uso solo se mostrarán cuando el usuario tenga una cuenta de invitado en Azure AD. Actualmente, SharePoint Online tiene una [experiencia de destinatario de uso compartido externa ad hoc](/sharepoint/what-s-new-in-sharing-in-targeted-release) para compartir un documento o una carpeta que no requiere que el usuario tenga una cuenta de invitado. En este caso, no se muestra una directiva de los términos de uso.

![Panel Usuarios y grupos: incluye la pestaña con la opción Todos los usuarios invitados seleccionada](./media/terms-of-use/b2b-guests.png)

## <a name="support-for-cloud-apps"></a>Compatibilidad con aplicaciones en la nube

Las directivas de los términos de uso se pueden usar para diversas aplicaciones en la nube, como Azure Information Protection y Microsoft Intune. Esta compatibilidad se encuentra actualmente en versión preliminar.

### <a name="azure-information-protection"></a>Azure Information Protection

Puede configurar una directiva de acceso condicional para la aplicación Azure Information Protection y requerir una directiva de términos de uso cuando un usuario accede a un documento protegido. Esto desencadenará una directiva de los términos de uso antes de que un usuario acceda a un documento protegido por primera vez.

![Panel de aplicaciones en la nube con la aplicación Microsoft Azure Information Protection seleccionada](./media/terms-of-use/cloud-app-info-protection.png)

### <a name="microsoft-intune-enrollment"></a>Inscripción en Microsoft Intune

Puede configurar una directiva de acceso condicional para la aplicación de inscripción en Microsoft Intune y requerir una directiva de los términos de uso antes de la inscripción de un dispositivo en Intune. Para obtener más información, consulte la entrada de blog [Choosing the right Terms solution for your organization](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409) (Elección de la solución de términos adecuada para su organización).

![Panel aplicaciones en la nube con la aplicación Microsoft Intune seleccionada](./media/terms-of-use/cloud-app-intune.png)

> [!NOTE]
> No se admite la aplicación de inscripción de Intune debido a los [Términos de uso por dispositivo](#per-device-terms-of-use).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P: No puedo iniciar sesión con PowerShell cuando están habilitados los términos de uso.**<br />
A. Los términos de uso solo se pueden aceptar al realizar una autenticación interactiva.

**P: ¿Cómo puedo ver si un usuario ha aceptado una instancia de Términos de uso y cuándo lo ha hecho?**<br />
A. En la hoja Términos de uso, haga clic en el número situado bajo **Aceptado**. También puede ver o buscar la actividad de aceptación en los registros de auditoría de Azure AD. Para más información, consulte Ver quién los ha aceptado y rechazado y [Visualización de registros de auditoría de Azure AD](#view-azure-ad-audit-logs).

**P: ¿Cuánto tiempo se almacena la información?**<br />
A. Los recuentos de usuarios en el informe Términos de uso y quién los ha aceptado o rechazado se almacenan mientras están vigentes los términos de uso. Los registros de auditoría de Azure AD se almacenan durante 30 días.

**P: ¿Por qué veo un número diferente de autorizaciones en el informe Términos de uso en comparación con los registros de auditoría de Azure AD?**<br />
A. El informe Términos de uso se almacena durante toda la vigencia de dicha directiva de los términos de uso, mientras que los registros de auditoría de Azure AD se almacenan durante 30 días. Además, el informe Términos de uso solo muestra el estado de consentimiento actual de los usuarios. Por ejemplo, si un usuario manifiesta su rechazo y luego su aceptación, el informe Términos de uso solo mostrará la aceptación de ese usuario. Si necesita ver el historial, puede usar los registros de auditoría de Azure AD.

**P: Si hay hipervínculos en el documento PDF de la directiva de los términos de uso, ¿los usuarios finales podrán hacer clic en ellos?**<br />
A. Sí, los usuarios finales pueden seleccionar hipervínculos a páginas adicionales, pero no se admiten vínculos a secciones del documento. Además, los hipervínculos en los PDF de la directiva de los términos de uso no funcionan cuando se accede a ellos desde el portal MyApps/MyAccount de Azure AD.

**P: ¿Puede una directiva de términos de uso admitir varios idiomas?**<br />
A. Sí. Actualmente, un administrador puede configurar hasta 108 idiomas diferentes para una sola directiva de términos de uso. Un administrador puede cargar varios documentos PDF y etiquetarlos con un idioma correspondiente (hasta 108). Cuando los usuarios finales inician sesión, nos fijamos en su preferencia de idioma del navegador y mostramos el documento coincidente. Si no hay ninguna coincidencia, mostraremos el documento predeterminado, que es el primer documento que se carga.

**P: ¿Cuándo se desencadena la directiva de los términos de uso?**<br />
A. La directiva de los términos de uso se desencadena durante la experiencia de inicio de sesión.

**P: ¿Qué aplicaciones puedo usar como destino de una directiva de términos de uso?**<br />
A. Puede crear una directiva de acceso condicional en las aplicaciones empresariales que usan autenticación moderna. Para más información, consulte [aplicaciones empresariales](./../manage-apps/view-applications-portal.md).

**P: ¿Puedo agregar varias directivas de términos de uso para un usuario o una aplicación determinados?**<br />
A. Sí, mediante la creación de varias directivas de acceso condicional cuyo destino sean dichos grupos o aplicaciones. Si un usuario se encuentra en el ámbito de varias directivas de términos de uso, aceptará primero una y después otra.

**P: ¿Qué ocurre si un usuario rechaza la directiva de los términos de uso?**<br />
R: El usuario será bloqueado y no podrá obtener acceso a la aplicación. El usuario tendría que iniciar sesión de nuevo y aceptar las condiciones con el fin de obtener acceso.

**P: ¿Es posible rechazar una directiva de términos de uso que se ha aceptado previamente?**<br />
A. Puede [revisar las directivas de términos de uso aceptadas previamente](#how-users-can-review-their-terms-of-use), pero actualmente no hay ninguna forma de rechazarlas.

**P: ¿Qué ocurre si también utilizo los términos y condiciones de Intune?**<br />
A. Si ha configurado los Términos de uso de Azure AD y los [Términos y condiciones de Intune](/intune/terms-and-conditions-create), se le pedirá al usuario que acepte ambos. Para obtener más información, consulte la entrada de blog [Choosing the right Terms solution for your organization](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409) (Elección de la solución de términos adecuada para su organización).

**P: ¿Qué puntos de conexión usa el servicio de términos de uso para la autenticación?**<br />
A. Los términos de uso usan los siguientes puntos de conexión para la autenticación: https://tokenprovider.termsofuse.identitygovernance.azure.com y https://account.activedirectory.windowsazure.com. Si su organización tiene una lista de direcciones URL permitidas para la inscripción, deberá agregar estos puntos de conexión a esa lista, además de los puntos de conexión de Azure AD, para iniciar sesión.

## <a name="next-steps"></a>Pasos siguientes

- [Inicio rápido: Solicitud de la aceptación de los términos de uso antes de acceder a aplicaciones en la nube](require-tou.md)