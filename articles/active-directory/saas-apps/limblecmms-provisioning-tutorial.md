---
title: 'Tutorial: Configuración de LimbleCMMS para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo aprovisionar y desaprovisionar automáticamente cuentas de usuario de Azure AD para LimbleCMMS.
services: active-directory
author: twimmers
writer: twimmers
manager: beatrizd
ms.assetid: 5e0d5369-7230-4a16-bc3f-9eac2bc80a8c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/07/2021
ms.author: thwimmer
ms.openlocfilehash: 5ad0ef1911a76b0bc99966e76f66a8b1cd8a0215
ms.sourcegitcommit: 86ca8301fdd00ff300e87f04126b636bae62ca8a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2021
ms.locfileid: "122195644"
---
# <a name="tutorial-configure-limblecmms-for-automatic-user-provisioning"></a>Tutorial: Configuración de LimbleCMMS para el aprovisionamiento automático de usuarios

En este tutorial se describen los pasos que debe realizar en LimbleCMMS y Azure Active Directory (Azure AD) para configurar el aprovisionamiento automático de usuarios. Cuando se configura, Azure AD aprovisiona y desaprovisiona usuarios y grupos de manera automática en [LimbleCMMS](https://limblecmms.com/) mediante su servicio de aprovisionamiento. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funcionalidades admitidas
> [!div class="checklist"]
> * Crear usuarios en LimbleCMMS.
> * Eliminar usuarios de LimbleCMMS cuando ya no necesiten acceso.
> * Crear grupos en LimbleCMMS.
> * Agregar o quitar usuarios de grupos en LimbleCMMS.
> * Eliminar grupos de LimbleCMMS.
> * Mantener los atributos de usuario sincronizados entre Azure AD y LimbleCMMS.
> * Aprovisionar grupos y pertenencias a grupos en LimbleCMMS.
> * [Inicio de sesión único](../manage-apps/add-application-portal-setup-oidc-sso.md) en LimbleCMMS (recomendado).

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* [Un inquilino de Azure AD](../develop/quickstart-create-new-tenant.md). 
* Una cuenta de usuario en Azure AD con [permiso](../roles/permissions-reference.md) para configurar el aprovisionamiento (por ejemplo, Administrador de aplicaciones, Administrador de aplicaciones en la nube, Propietario de la aplicación o Administrador global). 
* Un [inquilino de LimbleCMMS](https://limblecmms.com/signup/?plan=business-yearly) con licencia de Business Plus o ediciones superiores.
* Una cuenta de usuario en LimbleCMMS con permisos de superadministrador.
* Inicio de sesión único habilitado en el inquilino de LimbleCMMS (póngase en contacto con el administrador del equipo de éxito del cliente).
* Al menos un grupo que tenga pensado aprovisionar en LimbleCMMS (los permisos de LimbleCMMS se basan en grupos; si no aprovisiona un grupo, los usuarios que se aprovisionan no tendrán permisos asociados a ellos). 


## <a name="step-1-plan-your-provisioning-deployment"></a>Paso 1. Planeación de la implementación de aprovisionamiento
1. Obtenga información sobre [cómo funciona el servicio de aprovisionamiento](../app-provisioning/user-provisioning.md).
1. Determine quién estará en el [ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
1. Determine qué datos quiere [asignar entre Azure AD y LimbleCMMS](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-limblecmms-to-support-provisioning-with-azure-ad"></a>Paso 2. Configuración de LimbleCMMS para admitir el aprovisionamiento con Azure AD

1. Inicie sesión en LimbleCMMS como **superadministrador**.
1. Vaya a **Advanced Settings > Manage SSO** (Configuración avanzada > Administrar SSO).
      ![Administración del inicio de sesión único](media/limblecmms-provisioning-tutorial/limble-manage-sso.png)
1. Seleccione **Azure Active Directory** proveedor de SSO.
1. [Configure OIDC](https://help.limblecmms.com/en/articles/4446986-active-directory-oidc-sso-setup-guide) para admitir el inicio de sesión único.
1. Haga clic en el botón **Generate SCIM Token** (Generar token SCIM) para recuperar el token SCIM y guárdelo para un paso posterior.
1. Haga clic en **"Enable SSO"** (Habilitar SSO).

## <a name="step-3-add-limblecmms-from-the-azure-ad-application-gallery"></a>Paso 3. Adición de LimbleCMMS desde la galería de aplicaciones de Azure AD

Para empezar a administrar el aprovisionamiento de LimbleCMMS, agregue LimbleCMMS desde la galería de aplicaciones de Azure AD. Si ya ha configurado LimbleCMMS para el inicio de sesión único, puede usar la misma aplicación. Sin embargo, se recomienda que cree una aplicación independiente al probar la integración inicialmente. Puede encontrar más información sobre cómo agregar una aplicación desde la galería [aquí](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Paso 4. Determinar quién estará en el ámbito de aprovisionamiento 

El servicio de aprovisionamiento de Azure AD le permite definir quién se aprovisionará, en función de la asignación a la aplicación y de los atributos del usuario o grupo. Si elige el ámbito del que se aprovisionará en la aplicación en función de la asignación, puede usar los pasos [siguientes](../manage-apps/assign-user-or-group-access-portal.md) para asignar usuarios y grupos a la aplicación. Si elige el ámbito del que se aprovisionará en función únicamente de los atributos del usuario o grupo, puede usar un filtro de ámbito, tal como se describe [aquí](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Al asignar usuarios y grupos a LimbleCMMS, debe seleccionar un rol que no sea **Acceso predeterminado**. Los usuarios con el rol de acceso predeterminado se excluyen del aprovisionamiento y se marcarán como no autorizados en los registros de aprovisionamiento. Si el único rol disponible en la aplicación es el rol de acceso predeterminado, puede [actualizar el manifiesto de aplicación](../develop/howto-add-app-roles-in-azure-ad-apps.md) para agregar roles adicionales. 

* Empiece por algo pequeño. Pruebe con un pequeño conjunto de usuarios y grupos antes de implementarlo en todos. Cuando el ámbito del aprovisionamiento se define en los usuarios y grupos asignados, puede controlarlo asignando uno o dos usuarios o grupos a la aplicación. Cuando el ámbito se establece en todos los usuarios y grupos, puede especificar un [filtro de ámbito basado en atributos](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-limblecmms"></a>Paso 5. Configuración del aprovisionamiento automático de usuarios en LimbleCMMS 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD para crear, actualizar y deshabilitar usuarios o grupos en LimbleCMMS en función de las asignaciones de grupos y usuarios de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-limblecmms-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para LimbleCMMS en Azure AD, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

1. En la lista de aplicaciones, seleccione **LimbleCMMS**.

    ![Vínculo a LimbleCMMS en la lista de aplicaciones](common/all-applications.png)

1. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

1. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña de aprovisionamiento automático](common/provisioning-automatic.png)

1. En la sección **Credenciales de administrador**, escriba los valores de **URL de inquilino** y **Token secreto** de LimbleCMMS. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a LimbleCMMS.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

1. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que deba recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

1. Seleccione **Guardar**.

1. En la sección **Asignaciones**, seleccione **Sincronizar usuarios de Azure Active Directory con LimbleCMMS**.

1. Examine los atributos de usuario que se sincronizan entre Azure AD y LimbleCMMS en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de LimbleCMMS con el objetivo de realizar operaciones de actualización. Si decide cambiar el [atributo de destino coincidente](../app-provisioning/customize-application-attributes.md), deberá asegurarse de que la API de LimbleCMMS admite el filtrado de usuarios basado en ese atributo. Seleccione el botón **Guardar** para confirmar los cambios.

   |Atributo|Tipo|Compatible con el filtrado|
   |---|---|---|
   |userName|String|&check;
   |active|Boolean|   
   |emails[type eq "work"].value|String|
   |name.givenName|String|
   |name.familyName|String|
   |externalId|String|


1. En la sección **Asignaciones**, seleccione **Sincronizar grupos de Azure Active Directory con LimbleCMMS**.

1. Examine los atributos de grupo que se sincronizan entre Azure AD y LimbleCMMS en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para establecer coincidencias con los grupos de LimbleCMMS con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

      |Atributo|Tipo|Compatible con el filtrado|
      |---|---|---|
      |DisplayName|String|&check;
      |members|Referencia|
      |externalId|String|      

1. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

1. Para habilitar el servicio de aprovisionamiento de Azure AD para LimbleCMMS, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

1. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que quiere que se aprovisionen en LimbleCMMS.

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

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)