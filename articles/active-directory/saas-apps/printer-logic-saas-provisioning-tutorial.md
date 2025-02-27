---
title: 'Tutorial: Configuración de PrinterLogic SaaS para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Azure AD para PrinterLogic SaaS.
services: active-directory
documentationcenter: ''
author: twimmers
writer: twimmers
manager: beatrizd
ms.assetid: 001cfccf-b8a4-46e6-b355-94e8b694b122
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/02/2020
ms.author: thwimmer
ms.openlocfilehash: 1806a488eb247ae858e55b564a6023247e157006
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2021
ms.locfileid: "122323013"
---
# <a name="tutorial-configure-printerlogic-saas-for-automatic-user-provisioning"></a>Tutorial: Configuración de PrinterLogic SaaS para el aprovisionamiento automático de usuarios

En este tutorial se describen los pasos que debe realizar en PrinterLogic SaaS y Azure Active Directory (Azure AD) para configurar el aprovisionamiento automático de usuarios. Cuando se configura, Azure AD aprovisiona y cancela el aprovisionamiento de usuarios y grupos de manera automática en [PrinterLogic SaaS](https://www.printerlogic.com/) mediante el servicio de aprovisionamiento de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funcionalidades admitidas
> [!div class="checklist"]
> * Crear usuarios en PrinterLogic SaaS
> * Eliminar usuarios de PrinterLogic SaaS cuando ya no necesiten acceso
> * Mantener los atributos de usuario sincronizados entre Azure AD y PrinterLogic SaaS
> * Aprovisionar grupos y pertenencias a grupos en PrinterLogic SaaS
> * [Inicio de sesión único](./printerlogic-saas-tutorial.md) en PrinterLogic SaaS (recomendado)

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* [Un inquilino de Azure AD](../develop/quickstart-create-new-tenant.md) 
* Una cuenta de usuario en Azure AD con [permiso](../roles/permissions-reference.md) para configurar el aprovisionamiento (por ejemplo, Administrador de aplicaciones, Administrador de aplicaciones en la nube, Propietario de la aplicación o Administrador global). 
* Un inquilino de [PrinterLogic SaaS](https://www.printerlogic.com/).

## <a name="step-1-plan-your-provisioning-deployment"></a>Paso 1. Planeación de la implementación de aprovisionamiento
1. Obtenga información sobre [cómo funciona el servicio de aprovisionamiento](../app-provisioning/user-provisioning.md).
2. Determine quién estará en el [ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Determine qué datos quiere [asignar entre Azure AD y PrinterLogic SaaS](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-printerlogic-saas-to-support-provisioning-with-azure-ad"></a>Paso 2. Configuración de PrinterLogic SaaS para admitir el aprovisionamiento con Azure AD

1. En PrinterLogic, vaya a **Tools (Herramientas) > Settings (Configuración) > General**.

2. Vaya a la sección **Identity Provider Settings** (Configuración del proveedor de identidades).

3. Haga clic en la opción **SCIM**.

4. Asegúrese de que **Azure AD** está seleccionado en el menú desplegable.

5. Haga clic en **Generate SCIM Token** (Generar token de SCIM).

      ![Token de SCIM](media/printer-logic-saas-provisioning-tutorial/token.png)

6. Copie y guarde el **token de portador**. Este valor se escribirá en el campo **Token secreto** de la pestaña Aprovisionamiento en la aplicación de PrinterLogic SaaS en Azure Portal.

7. Escriba https://gw.app.printercloud.com/{instance_name}/scim/v2 en el campo **URL de inquilino** de la pestaña Aprovisionamiento en la aplicación de PrinterLogic SaaS en Azure Portal.

## <a name="step-3-add-printerlogic-saas-from-the-azure-ad-application-gallery"></a>Paso 3. Incorporación de PrinterLogic SaaS desde la galería de aplicaciones de Azure AD

Para empezar a administrar el aprovisionamiento de PrinterLogic SaaS, agregue PrinterLogic SaaS desde la galería de aplicaciones de Azure AD. Si ha configurado previamente PrinterLogic SaaS para SSO, puede usar la misma aplicación. Sin embargo, se recomienda que cree una aplicación independiente al probar la integración inicialmente. Puede encontrar más información sobre cómo agregar una aplicación desde la galería [aquí](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Paso 4. Determinar quién estará en el ámbito de aprovisionamiento 

El servicio de aprovisionamiento de Azure AD le permite definir quién se aprovisionará, en función de la asignación a la aplicación y de los atributos del usuario o grupo. Si elige el ámbito del que se aprovisionará en la aplicación en función de la asignación, puede usar los pasos [siguientes](../manage-apps/assign-user-or-group-access-portal.md) para asignar usuarios y grupos a la aplicación. Si elige el ámbito del que se aprovisionará en función únicamente de los atributos del usuario o grupo, puede usar un filtro de ámbito, tal como se describe [aquí](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Al asignar usuarios y grupos a PrinterLogic SaaS, debe seleccionar un rol que no sea **Acceso predeterminado**. Los usuarios con el rol de acceso predeterminado se excluyen del aprovisionamiento y se marcarán como no autorizados en los registros de aprovisionamiento. Si el único rol disponible en la aplicación es el rol de acceso predeterminado, puede [actualizar el manifiesto de aplicación](../develop/howto-add-app-roles-in-azure-ad-apps.md) para agregar roles adicionales. 

* Empiece por algo pequeño. Pruebe con un pequeño conjunto de usuarios y grupos antes de implementarlo en todos. Cuando el ámbito del aprovisionamiento se define en los usuarios y grupos asignados, puede controlarlo asignando uno o dos usuarios o grupos a la aplicación. Cuando el ámbito se establece en todos los usuarios y grupos, puede especificar un [filtro de ámbito basado en atributos](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-printerlogic-saas"></a>Paso 5. Configuración del aprovisionamiento automático de usuarios en PrinterLogic SaaS 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD a fin de crear, actualizar y deshabilitar usuarios o grupos en TestApp en función de las asignaciones de grupos o usuarios de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-printerlogic-saas-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para PrinterLogic SaaS en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **PrinterLogic SaaS**.

    ![Vínculo de PrinterLogic SaaS en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña de aprovisionamiento automático](common/provisioning-automatic.png)

5. En la sección **Credenciales del administrador**, escriba la dirección URL de inquilino de PrinterLogic SaaS y el token secreto recuperado anteriormente en el paso 2. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a PrinterLogic SaaS. Si la conexión no se establece, asegúrese de que la cuenta de PrinterLogic SaaS tenga permisos de administrador y pruebe de nuevo.

    ![Token de Azure AD](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que deba recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Seleccione **Guardar**.

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Priority Matrix** (Sincronizar usuarios de Azure Active Directory con PrinterLogic SaaS).

9. Revise los atributos de usuario que se sincronizan entre Azure AD y PrinterLogic SaaS en la sección **Attribute-Mapping** (Asignación de atributos). Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de PrinterLogic SaaS con el objetivo de realizar operaciones de actualización. Si decide cambiar el [atributo de destino coincidente](../app-provisioning/customize-application-attributes.md), deberá asegurarse de que la API de PrinterLogic SaaS admite el filtrado de usuarios en función de ese atributo. Seleccione el botón **Guardar** para confirmar los cambios.

   |Atributo|Tipo|Compatible con el filtrado|
   |---|---|---|
   |userName|String|&check;|
   |active|Boolean|
   |title|String|
   |name.givenName|String|
   |name.familyName|String|
   |emails[type eq "work"].value|String|
   |externalId|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|String|
   |urn:ietf:params:scim:schemas:extension:printercloud:2.0:User:authPin|String|
   |urn:ietf:params:scim:schemas:extension:printercloud:2.0:User:authPinUser|String|
   |urn:ietf:params:scim:schemas:extension:printercloud:2.0:User:badgeId|String|

10. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Priority Matrix** (Sincronizar grupos de Azure Active Directory con PrinterLogic SaaS).

11. Revise los atributos de grupo que se sincronizan entre Azure AD y PrinterLogic SaaS en la sección **Attribute-Mapping** (Asignación de atributos). Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con los grupos de PrinterLogic SaaS con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

      |Atributo|Tipo|Compatible con el filtrado|
      |---|---|---|
      |DisplayName|String|&check;|
      |externalId|String|
      |members|Referencia|

12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de aprovisionamiento de Azure AD para PrinterLogic SaaS, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

14. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en PrinterLogic SaaS.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia el ciclo de sincronización inicial de todos los usuarios y grupos definidos en **Ámbito** en la sección **Configuración**. El ciclo de sincronización inicial tarda más tiempo en realizarse que los ciclos posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. 

## <a name="step-6-monitor-your-deployment"></a>Paso 6. Supervisión de la implementación
Una vez configurado el aprovisionamiento, use los recursos siguientes para supervisar la implementación:

1. Use los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md) para determinar qué usuarios se han aprovisionado correctamente o sin éxito.
2. Consulte la [barra de progreso](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) para ver el estado del ciclo de aprovisionamiento y cuánto falta para que finalice.
3. Si la configuración de aprovisionamiento parece estar en mal estado, la aplicación pasará a estar en cuarentena. Más información sobre los estados de cuarentena [aquí](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)