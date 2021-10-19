---
title: 'Tutorial: configuración de Chaos para aprovisionar usuarios automáticamente con Azure Active Directory | Microsoft Docs'
description: Aprenda a aprovisionar y cancelar el aprovisionamiento de forma automática de las cuentas de usuario de Azure AD para Chaos.
services: active-directory
author: twimmers
writer: twimmers
manager: beatrizd
ms.assetid: 1a831f80-8fe0-4cb5-a639-ab9e2a0e90f7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/27/2021
ms.author: thwimmer
ms.openlocfilehash: a25acc98fcac7ae9c6728c5fff0a9274d66fdab1
ms.sourcegitcommit: 54e7b2e036f4732276adcace73e6261b02f96343
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2021
ms.locfileid: "129812501"
---
# <a name="tutorial-configure-chaos-for-automatic-user-provisioning"></a>Tutorial: configuración de Chaos para el aprovisionamiento automático de usuarios

En este tutorial, se describen los pasos que debe realizar en Chaos y Azure Active Directory (Azure AD) para configurar el aprovisionamiento automático de usuarios. Cuando se configura, Azure AD aprovisiona y desaprovisiona usuarios y grupos de manera automática en [Tribeloo](https://www.tribeloo.com/) mediante su servicio de aprovisionamiento. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funcionalidades admitidas
> [!div class="checklist"]
> * Crear usuarios en Chaos
> * Quitar usuarios de Chaos cuando ya no necesiten acceso
> * Mantener los atributos de usuario sincronizados entre Azure AD y Chaos
> * [Inicio de sesión único](../manage-apps/add-application-portal-setup-oidc-sso.md) en Chaos (recomendado)

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* [Un inquilino de Azure AD](../develop/quickstart-create-new-tenant.md)
* Una cuenta de usuario en Azure AD con [permiso](../roles/permissions-reference.md) para configurar el aprovisionamiento (por ejemplo, Administrador de aplicaciones, Administrador de aplicaciones en la nube, Propietario de la aplicación o Administrador global). 
* Un nombre de dominio verificado asignado al inquilino de Azure y usado para los correos electrónicos de los usuarios respectivos.
* Estar familiarizado con el [inicio de sesión corporativo de Chaos](https://docs.chaosgroup.com/display/KB/Corporate+Sign+In).
* Haber leído y aceptado los [términos de uso](https://www.chaosgroup.com/en/terms) de Chaos, la [declaración de privacidad](https://www.chaosgroup.com/corporate/privacy-notice) y el [EULA](https://www.chaosgroup.com/eula) relacionados con el inicio de sesión corporativo.

## <a name="step-1-plan-your-provisioning-deployment"></a>Paso 1. Planeación de la implementación de aprovisionamiento
1. Obtenga información sobre [cómo funciona el servicio de aprovisionamiento](../app-provisioning/user-provisioning.md).
1. Determine quién estará en el [ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
1. Determine qué datos quiere [asignar entre Azure AD y Chaos](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-chaos-to-support-provisioning-with-azure-ad"></a>Paso 2. Configuración de Chaos para admitir el aprovisionamiento con Azure AD

Abra una [Incidencia de soporte técnico](https://support.chaos.com) o póngase en contacto con el administrador de cuentas clave de Chaos para solicitar que el inicio de sesión corporativo se establezca para su empresa.

## <a name="step-3-add-chaos-from-the-azure-ad-application-gallery"></a>Paso 3. Incorporación de Chaos desde la galería de aplicaciones de Azure AD

Para empezar a administrar el aprovisionamiento de Chaos, agregue Chaos desde la galería de aplicaciones de Azure AD. Si ha configurado previamente Tribeloo para SSO, puede usar la misma aplicación. Sin embargo, se recomienda que cree una aplicación independiente al probar la integración inicialmente. Puede encontrar más información sobre cómo agregar una aplicación desde la galería [aquí](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Paso 4. Determinar quién estará en el ámbito de aprovisionamiento 

El servicio de aprovisionamiento de Azure AD le permite definir quién se aprovisionará, en función de la asignación a la aplicación y de los atributos del usuario o grupo. Si elige el ámbito del que se aprovisionará en la aplicación en función de la asignación, puede usar los pasos [siguientes](../manage-apps/assign-user-or-group-access-portal.md) para asignar usuarios y grupos a la aplicación. Si elige el ámbito del que se aprovisionará en función únicamente de los atributos del usuario o grupo, puede usar un filtro de ámbito, tal como se describe [aquí](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md). 

* Al asignar usuarios y grupos a Chaos, debe seleccionar un rol que no sea **Acceso predeterminado**. Los usuarios con el rol de acceso predeterminado se excluyen del aprovisionamiento y se marcarán como no autorizados en los registros de aprovisionamiento. Si el único rol disponible en la aplicación es el rol de acceso predeterminado, puede [actualizar el manifiesto de aplicación](../develop/howto-add-app-roles-in-azure-ad-apps.md) para agregar roles adicionales. 

* Empiece por algo pequeño. Pruebe con un pequeño conjunto de usuarios y grupos antes de implementarlo en todos. Cuando el ámbito del aprovisionamiento se define en los usuarios y grupos asignados, puede controlarlo asignando uno o dos usuarios o grupos a la aplicación. Cuando el ámbito se establece en todos los usuarios y grupos, puede especificar un [filtro de ámbito basado en atributos](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-chaos"></a>Paso 5. Configuración del aprovisionamiento automático de usuarios en Chaos 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD a fin de crear, actualizar y deshabilitar usuarios o grupos en Chaos en función de las asignaciones de usuarios y grupos de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-chaos-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Chaos en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

1. En la lista de aplicaciones, seleccione **Chaos**.

    ![Vínculo a Chaos en la lista de aplicaciones](common/all-applications.png)

1. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

1. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña de aprovisionamiento automático](common/provisioning-automatic.png)

1. En la sección **Credenciales de administrador**, escriba los valores de **URL de inquilino** y del **Token secreto** de Chaos. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Chaos. Si la conexión no se establece, asegúrese de que la cuenta de Chaos tiene permisos de administrador y vuelva a intentarlo.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

1. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que deba recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

1. Seleccione **Guardar**.

1. En la sección **Asignaciones**, seleccione **Sincronizar usuarios de Azure Active Directory con Chaos**.

1. Examine los atributos de usuario que se sincronizan entre Azure AD y Chaos en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades **Coincidentes** se usan para establecer correspondencia con las cuentas del usuario en Chaos a fin de realizar operaciones de actualización. Si decide cambiar el [atributo de destino coincidente](../app-provisioning/customize-application-attributes.md), deberá asegurarse de que la API de Chaos admite el filtrado de usuarios basado en ese atributo. Seleccione el botón **Guardar** para confirmar los cambios.

   |Atributo|Tipo|Compatible con el filtrado|
   |---|---|---|
   |userName|String|&check;
   |active|Boolean|
   |emails[type eq "work"].value|String|
   |name.givenName|String|
   |name.familyName|String|

1. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

1. Para habilitar el servicio de aprovisionamiento de Azure AD para Chaos, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

1. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea aprovisionar en Chaos.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

1. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia el ciclo de sincronización inicial de todos los usuarios y grupos definidos en **Ámbito** en la sección **Configuración**. El ciclo de sincronización inicial tarda más tiempo en realizarse que los ciclos posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. 

## <a name="step-6-monitor-your-deployment"></a>Paso 6. Supervisión de la implementación
Una vez configurado el aprovisionamiento, use los recursos siguientes para supervisar la implementación:

* Use los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md) para determinar qué usuarios se han aprovisionado correctamente o sin éxito.
* Consulte la [barra de progreso](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) para ver el estado del ciclo de aprovisionamiento y cuánto falta para que finalice.
* Si la configuración de aprovisionamiento parece estar en mal estado, la aplicación pasará a estar en cuarentena. Más información sobre los estados de cuarentena [aquí](../app-provisioning/application-provisioning-quarantine-status.md). 


## <a name="more-resources"></a>Más recursos

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Inicio de sesión corporativo de Chaos](https://docs.chaosgroup.com/display/KB/Corporate+Sign+In)
* [Configuración del inicio de sesión corporativo de Chaos con Azure](https://docs.chaosgroup.com/display/KB/Configuring+Corporate+Sign+In+with+Azure)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)