---
title: 'Tutorial: Configuración de Asana para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda cómo aprovisionar y desaprovisionar de forma automática las cuentas de usuario de Azure AD para Asana.
services: active-directory
author: twimmers
writer: twimmers
manager: beatrizd
ms.assetid: 274810a2-bd74-4500-95f1-c720abf23541
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: thwimmer
ms.openlocfilehash: fca7efc24770b9a920c88082566d0e7509ffe513
ms.sourcegitcommit: 5af89a2a7b38b266cc3adc389d3a9606420215a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "131990436"
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Tutorial: Configuración de Asana para el aprovisionamiento automático de usuarios

En este tutorial, se describen los pasos que debe realizar en Asana y Azure Active Directory (Azure AD) para configurar el aprovisionamiento automático de usuarios. Cuando se configura, Azure AD aprovisiona y desaprovisiona usuarios y grupos de manera automática en [Asana](https://www.asana.com/) mediante su servicio de aprovisionamiento. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funcionalidades admitidas
> [!div class="checklist"]
> * Crear usuarios en Asana.
> * Quitar usuarios de Asana cuando ya no necesiten acceso.
> * Mantener los atributos de usuario sincronizados entre Azure AD y Asana.
> * Aprovisionar grupos y pertenencias a grupos en Asana.
> * [Inicio de sesión único](asana-tutorial.md) en Asana (recomendado).

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD
* Un inquilino de Asana con el plan [Enterprise](https://www.asana.com/pricing) o uno superior habilitado
* Una cuenta de usuario de Asana con permisos de administrador

> [!NOTE]
> La integración del aprovisionamiento de Azure AD depende de la [API de Asana](https://asana.com/developers/api-reference/users), que está disponible para Asana.

## <a name="step-1-plan-your-provisioning-deployment"></a>Paso 1. Planeación de la implementación de aprovisionamiento
1. Obtenga información sobre [cómo funciona el servicio de aprovisionamiento](../app-provisioning/user-provisioning.md).
1. Determine quién estará en el [ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
1. Determine qué datos quiere [asignar entre Azure AD y Asana](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-asana-to-support-provisioning-with-azure-ad"></a>Paso 2. Configuración de Asana para admitir el aprovisionamiento con Azure AD
 > [!TIP]
 > Para habilitar el inicio de sesión único basado en SAML para Asana, siga las instrucciones proporcionadas en Azure Portal. El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático, aunque estas dos características se complementan entre sí.

### <a name="generate-secret-token-in-asana"></a>Generación de tokens secretos en Asana

* Inicie sesión en [Asana](https://app.asana.com/) mediante la cuenta de administrador.
* Seleccione la foto de perfil de la barra superior y seleccione la configuración del nombre de organización actual.
* Vaya a la pestaña **Service Accounts** (Cuentas de servicio).
* Seleccione **Add Service Account** (Agregar cuenta de servicio).
* Actualice los valores de **Name** (Nombre) y **About** (Acerca de) y la foto de perfil, según sea necesario. Copie el token del campo **Token** y selecciónelo en Save Changes (Guardar cambios).

## <a name="step-3-add-asana-from-the-azure-ad-application-gallery"></a>Paso 3. Adición de Asana desde la galería de aplicaciones de Azure AD

Para empezar a administrar el aprovisionamiento de Asana, agregue Asana desde la galería de aplicaciones de Azure AD. Si ha configurado previamente Asana para SSO, puede usar la misma aplicación. Sin embargo, se recomienda que cree una aplicación independiente al probar la integración inicialmente. Puede encontrar más información sobre cómo agregar una aplicación desde la galería [aquí](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Paso 4. Determinar quién estará en el ámbito de aprovisionamiento 

El servicio de aprovisionamiento de Azure AD le permite definir quién se aprovisionará, en función de la asignación a la aplicación y de los atributos del usuario o grupo. Si elige el ámbito del que se aprovisionará en la aplicación en función de la asignación, puede usar los pasos [siguientes](../manage-apps/assign-user-or-group-access-portal.md) para asignar usuarios y grupos a la aplicación. Si elige el ámbito del que se aprovisionará en función únicamente de los atributos del usuario o grupo, puede usar un filtro de ámbito, tal como se describe [aquí](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

* Al asignar usuarios y grupos a Asana, debe seleccionar un rol que no sea **Acceso predeterminado**. Los usuarios con el rol de acceso predeterminado se excluyen del aprovisionamiento y se marcarán como no autorizados en los registros de aprovisionamiento. Si el único rol disponible en la aplicación es el rol de acceso predeterminado, puede [actualizar el manifiesto de aplicación](../develop/howto-add-app-roles-in-azure-ad-apps.md) para agregar más roles. 

* Empiece por algo pequeño. Pruebe con un pequeño conjunto de usuarios y grupos antes de implementarlo en todos. Cuando el ámbito del aprovisionamiento se define en los usuarios y grupos asignados, puede controlarlo asignando uno o dos usuarios o grupos a la aplicación. Cuando el ámbito se establece en todos los usuarios y grupos, puede especificar un [filtro de ámbito basado en atributos](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-asana"></a>Paso 5. Configuración del aprovisionamiento automático de usuarios en Asana 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD a fin de crear, actualizar y deshabilitar usuarios o grupos en Asana en función de las asignaciones de usuarios y grupos de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-asana-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Asana en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

1. En la lista de aplicaciones, seleccione **Asana**.

    ![Vínculo a Asana en la lista de aplicaciones](common/all-applications.png)

1. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

1. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña de aprovisionamiento automático](common/provisioning-automatic.png)

1. En la sección **Credenciales del administrador**, escriba la dirección URL del inquilino de Asana y el token secreto que proporciona Asana. Haga clic en **Probar conexión** para asegurarse de que Azure AD pueda conectarse a Asana. Si se produce un error en la conexión, póngase en contacto con Asana para comprobar la configuración de la cuenta.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

1. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que deba recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

1. Seleccione **Guardar**.

1. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Asana** (Sincronizar usuarios de Azure Active Directory con Asana).

1. Revise los atributos de usuario que se sincronizan entre Azure AD y Asana en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Asana con el objetivo de realizar operaciones de actualización. Si decide cambiar el [atributo de destino coincidente](../app-provisioning/customize-application-attributes.md), deberá asegurarse de que la API de Asana admita el filtrado de usuarios en función de ese atributo. Seleccione el botón **Guardar** para confirmar los cambios.

   |Atributo|Tipo|Compatible con el filtrado|Requerido por Asana|
   |---|---|---|---|
   |userName|String|&check;|&check;|   
   |active|Boolean|||
   |name.formatted|String|||
   |preferredLanguage|String|||
   |title|String|||
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String|||


1. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to Asana** (Sincronizar grupos de Azure Active Directory con Asana).

1. Revise los atributos de grupo que se sincronizan entre Azure AD y Asana en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para establecer correspondencia con los grupos de Asana a fin de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

      |Atributo|Tipo|Compatible con el filtrado|Requerido por Asana|
      |---|---|---|---|
      |DisplayName|String|&check;|&check;      
      |members|Referencia|||

1. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

1. Para habilitar el servicio de aprovisionamiento de Azure AD para Asana, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

1. Defina los usuarios y los grupos que le gustaría aprovisionar en Asana; para ello, elija los valores adecuados en **Ámbito** en la sección **Configuración**.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

1. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia el ciclo de sincronización inicial de todos los usuarios y grupos definidos en **Ámbito** en la sección **Configuración**. El ciclo inicial tarda más tiempo en ejecutarse que los ciclos siguientes, que se producen aproximadamente cada 40 minutos, siempre y cuando el servicio de aprovisionamiento de Azure AD esté en ejecución. 

## <a name="step-6-monitor-your-deployment"></a>Paso 6. Supervisión de la implementación
Una vez configurado el aprovisionamiento, use los recursos siguientes para supervisar la implementación:

* Use los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md) para determinar qué usuarios se han aprovisionado correctamente o sin éxito.
* Consulte la [barra de progreso](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) para ver el estado del ciclo de aprovisionamiento y cuánto falta para que finalice
* Si la configuración de aprovisionamiento parece estar en mal estado, la aplicación pasará a estar en cuarentena. Más información sobre los estados de cuarentena [aquí](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="change-log"></a>Registro de cambios

* 06/11/2021: se ha agregado compatibilidad con **externalId, name.givenName y name.familyName**. Se ha agregado compatibilidad con **preferredLanguage, title y urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department**. Y se ha habilitado el **aprovisionamiento de grupos**.

## <a name="more-resources"></a>Más recursos

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)