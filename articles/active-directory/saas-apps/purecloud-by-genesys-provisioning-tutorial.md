---
title: 'Tutorial: Configuración de PureCloud by Genesys para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a aprovisionar y cancelar el aprovisionamiento de forma automática de las cuentas de usuario de Azure AD para PureCloud by Genesys.
services: active-directory
author: twimmers
writer: twimmers
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/05/2020
ms.author: thwimmer
ms.openlocfilehash: 672fb74e170a66b12948258bd668f91c7e610aeb
ms.sourcegitcommit: 5af89a2a7b38b266cc3adc389d3a9606420215a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "131989154"
---
# <a name="tutorial-configure-purecloud-by-genesys-for-automatic-user-provisioning"></a>Tutorial: Configuración de PureCloud by Genesis para el aprovisionamiento automático de usuarios

En este tutorial, se describen los pasos que debe realizar en PureCloud by Genesys y Azure Active Directory (Azure AD) para configurar el aprovisionamiento automático de usuarios. Cuando se configura, Azure AD aprovisiona y anula el aprovisionamiento automáticamente de usuarios y grupos en [PureCloud by Genesys](https://www.genesys.com) mediante el servicio de aprovisionamiento de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funcionalidades admitidas
> [!div class="checklist"]
> * Crear usuarios en PureCloud by Genesys
> * Quitar usuarios de PureCloud by Genesys cuando ya no necesiten el acceso
> * Mantener los atributos de usuario sincronizados entre Azure AD y PureCloud by Genesys
> * Aprovisionar grupos y pertenencias a grupos en PureCloud by Genesys
> * [Inicio de sesión](./purecloud-by-genesys-tutorial.md) único en PureCloud by Genesys (recomendado)

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* [Un inquilino de Azure AD](../develop/quickstart-create-new-tenant.md) 
* Una cuenta de usuario en Azure AD con [permiso](../roles/permissions-reference.md) para configurar el aprovisionamiento (por ejemplo, Administrador de aplicaciones, Administrador de aplicaciones en la nube, Propietario de la aplicación o Administrador global). 
* Una [organización](https://help.mypurecloud.com/?p=81984) de PureCloud.
* Un usuario con [permisos](https://help.mypurecloud.com/?p=24360) para crear un cliente de OAuth.

> [!NOTE]
> Esta integración también está disponible para usarse desde el entorno de la nube del gobierno de EE. UU de Azure AD. Puede encontrar esta aplicación en la galería de aplicaciones de la nube del gobierno de EE. UU. de Azure AD y configurarla de la misma manera que en la nube pública.

## <a name="step-1-plan-your-provisioning-deployment"></a>Paso 1. Planeación de la implementación de aprovisionamiento
1. Obtenga información sobre [cómo funciona el servicio de aprovisionamiento](../app-provisioning/user-provisioning.md).
2. Determine quién estará en el [ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Determine qué datos se [asignarán entre Azure AD y PureCloud by Genesys](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-purecloud-by-genesys-to-support-provisioning-with-azure-ad"></a>Paso 2. Configuración de PureCloud by Genesys para admitir el aprovisionamiento con Azure AD

1. Cree un [cliente de OAuth](https://help.mypurecloud.com/?p=188023) configurado en la organización de PureCloud.
2. Genere un token [con el cliente de OAuth](https://developer.mypurecloud.com/api/rest/authorization/use-client-credentials.html).
3. Si quiere aprovisionar automáticamente la pertenencia a grupos en PureCloud, debe [crear grupos](https://help.mypurecloud.com/?p=52397) en PureCloud con un nombre idéntico al grupo en Azure AD.

## <a name="step-3-add-purecloud-by-genesys-from-the-azure-ad-application-gallery"></a>Paso 3. Agregación de PureCloud by Genesis desde la galería de aplicaciones de Azure AD

Para empezar a administrar el aprovisionamiento de PureCloud by Genesys, agregue PureCloud by Genesys desde la galería de aplicaciones de Azure AD. Si ha configurado previamente PureCloud by Genesys para el inicio de sesión único, puede usar la misma aplicación. Sin embargo, se recomienda que cree una aplicación independiente al probar la integración inicialmente. Puede encontrar más información sobre cómo agregar una aplicación desde la galería [aquí](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Paso 4. Determinar quién estará en el ámbito de aprovisionamiento 

El servicio de aprovisionamiento de Azure AD le permite definir quién se aprovisionará, en función de la asignación a la aplicación y de los atributos del usuario y grupo. Si elige el ámbito del que se aprovisionará en la aplicación en función de la asignación, puede usar los pasos [siguientes](../manage-apps/assign-user-or-group-access-portal.md) para asignar usuarios y grupos a la aplicación. Si elige el ámbito del que se aprovisionará en función únicamente de los atributos del usuario o grupo, puede usar un filtro de ámbito, tal como se describe [aquí](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Al asignar usuarios y grupos a PureCloud by Genesys, debe seleccionar un rol que no sea de **acceso predeterminado**. Los usuarios con el rol de acceso predeterminado se excluyen del aprovisionamiento y se marcarán como no autorizados en los registros de aprovisionamiento. Si el único rol disponible en la aplicación es el rol de acceso predeterminado, puede [actualizar el manifiesto de aplicación](../develop/howto-add-app-roles-in-azure-ad-apps.md) para agregar más roles. 

* Empiece por algo pequeño. Pruebe con un pequeño conjunto de usuarios y grupos antes de implementarlo en todos. Cuando el ámbito del aprovisionamiento se define en los usuarios y grupos asignados, puede controlar el aprovisionamiento asignando uno o dos usuarios o grupos a la aplicación. Cuando el ámbito se establece en todos los usuarios y grupos, puede especificar un [filtro de ámbito basado en atributos](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-purecloud-by-genesys"></a>Paso 5. Configuración del aprovisionamiento automático de usuarios para PureCloud by Genesys 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD a fin de crear, actualizar y deshabilitar usuarios o grupos en TestApp en función de las asignaciones de grupos o usuarios de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-purecloud-by-genesys-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para PureCloud by Genesys en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **PureCloud by Genesys**.

    ![El vínculo de PureCloud by Genesys en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Captura de pantalla de las opciones de administración con la opción Aprovisionamiento seleccionada.](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Captura de pantalla de la lista desplegable Modo de aprovisionamiento con la opción Automático seleccionada.](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, escriba la dirección URL de la API de PureCloud by Genesys y el token de OAuth en los campos **URL de inquilino** y **Token secreto** respectivamente. La dirección URL de la API se estructurará como `{{API Url}}/api/v2/scim/v2`, mediante la dirección URL de la API de la región PureCloud del [Centro para desarrolladores de PureCloud](https://developer.mypurecloud.com/api/rest/index.html). Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a PureCloud by Genesys. Si la conexión no se establece, asegúrese de que la cuenta de PureCloud by Genesys tiene permisos de administrador y pruebe otra vez.

    ![Captura de pantalla que muestra el cuadro de diálogo Credenciales de administrador, en el que se puede especificar el URL de inquilino y el secreto de inquilino.](./media/purecloud-by-genesys-provisioning-tutorial/provisioning.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que deba recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Seleccione **Guardar**.

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to PureCloud by Genesys** (Sincronizar usuarios de Azure Active Directory con PureCloud by Genesys).

9. Revise los atributos de usuario que se sincronizan entre Azure AD y PureCloud by Genesys en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de PureCloud by Genesys para las operaciones de actualización. Si decide cambiar el [atributo de destino coincidente](../app-provisioning/customize-application-attributes.md), deberá asegurarse de que la API de PureCloud by Genesys admite el filtrado de usuarios basado en ese atributo. Seleccione el botón **Guardar** para confirmar los cambios.

     |Atributo|Tipo|Compatible con el filtrado|
     |---|---|---|
     |userName|String|&check;|
     |active|Boolean|
     |DisplayName|String|
     |emails[type eq "work"].value|String|
     |title|String|
     |phoneNumbers[type eq "mobile"].value|String|
     |phoneNumbers[type eq "work"].value|String|
     |phoneNumbers[type eq "work2"].value|String|
     |phoneNumberss[type eq "work3"].value|String|
     |phoneNumbers[type eq "work4"].value|String|
     |phoneNumbers[type eq "home"].value|String|
     |phoneNumbers[type eq "microsoftteams"].value|String|
     |roles|String|
     |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String|
     |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Referencia|
     |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|String|
     |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|String|
     |urn:ietf:params:scim:schemas:extension:genesys:purecloud:2.0:User:externalIds[authority eq 'microsoftteams'].value|String|     
     |urn:ietf:params:scim:schemas:extension:genesys:purecloud:2.0:User:externalIds[authority eq 'ringcentral'].value|String|    
     |urn:ietf:params:scim:schemas:extension:genesys:purecloud:2.0:User:externalIds[authority eq 'zoomphone].value|String|

10. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to PureCloud by Genesys** (Sincronizar grupos de Azure Active Directory con PureCloud by Genesys).

11. Revise los atributos de grupo que se sincronizan entre Azure AD y PureCloud by Genesys en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para establecer coincidencias con los grupos de PureCloud by Genesys con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios. PureCloud by Genesis no admite la creación o eliminación de grupos y solo admite la actualización de grupos.

      |Atributo|Tipo|Compatible con el filtrado|
      |---|---|---|
      |DisplayName|String|&check;|
      |externalId|String|
      |members|Referencia|

12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de aprovisionamiento de Azure AD para PureCloud by Genesys, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

14. Elija los valores que quiera en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que quiere que se aprovisionen en PureCloud by Genesys.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia el ciclo de sincronización inicial de todos los usuarios y grupos definidos en **Ámbito** en la sección **Configuración**. El ciclo de sincronización inicial tarda más tiempo en realizarse que los ciclos posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. 

## <a name="step-6-monitor-your-deployment"></a>Paso 6. Supervisión de la implementación
Una vez configurado el aprovisionamiento, use los recursos siguientes para supervisar la implementación:

* Use los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md) para determinar qué usuarios se han aprovisionado correctamente o sin éxito.
* Consulte la [barra de progreso](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) para ver el estado del ciclo de aprovisionamiento y cuánto falta para que finalice.
* Si la configuración de aprovisionamiento parece estar en mal estado, la aplicación pasará a estar en cuarentena. Más información sobre los estados de cuarentena [aquí](../app-provisioning/application-provisioning-quarantine-status.md).

## <a name="change-log"></a>Registro de cambios

* 10/09/2020: se ha agregado compatibilidad con el atributo de empresa de extensión **employeeNumber**.
* 18/05/2021: se ha agregado compatibilidad con los atributos principales **phoneNumbers[type eq "work2"]** , **phoneNumbers[type eq "work3"]** , **phoneNumbers[type eq "work4"]** , **phoneNumbers[type eq "home"]** , **phoneNumbers[type eq "microsoftteams"]** y roles. Y también se ha agregado compatibilidad con los atributos de extensión personalizados **urn:ietf:params:scim:schemas:extension:genesys:purecloud:2.0:User:externalIds[authority eq 'microsoftteams']** , **urn:ietf:params:scim:schemas:extension:genesys:purecloud:2.0:User:externalIds[authority eq 'zoomphone]** y **urn:ietf:params:scim:schemas:extension:genesys:purecloud:2.0:User:externalIds[authority eq 'ringcentral']** .

## <a name="more-resources"></a>Más recursos

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)