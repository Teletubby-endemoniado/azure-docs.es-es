---
title: 'Tutorial: Configuración de Box RingCentral para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a aprovisionar y desaprovisionar de forma automática las cuentas de usuario de Azure AD para RingCentral.
services: active-directory
author: twimmers
writer: twimmers
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/30/2019
ms.author: thwimmer
ms.openlocfilehash: 4999669aee35b9e0d7303ae1156f10318c775562
ms.sourcegitcommit: 5af89a2a7b38b266cc3adc389d3a9606420215a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "131989781"
---
# <a name="tutorial-configure-ringcentral-for-automatic-user-provisioning"></a>Tutorial: Configuración de RingCentral para el aprovisionamiento automático de usuarios

En este tutorial, se describen los pasos que debe realizar en RingCentral y Azure Active Directory (Azure AD) para configurar el aprovisionamiento automático de usuarios. Cuando se configura, Azure AD aprovisiona y desaprovisiona usuarios y grupos de manera automática en [RingCentral](https://www.ringcentral.com/office/plansandpricing.html) mediante el servicio de aprovisionamiento de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funcionalidades admitidas
> [!div class="checklist"]
> * Crear usuarios en RingCentral
> * Quitar usuarios de RingCentral cuando ya no necesiten acceso
> * Mantener los atributos de usuario sincronizados entre Azure AD y RingCentral
> * [Inicio de sesión único](./ringcentral-tutorial.md) en RingCentral (recomendado)

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* [Un inquilino de Azure AD](../develop/quickstart-create-new-tenant.md) 
* Una cuenta de usuario en Azure AD con [permiso](../roles/permissions-reference.md) para configurar el aprovisionamiento (por ejemplo, Administrador de aplicaciones, Administrador de aplicaciones en la nube, Propietario de la aplicación o Administrador global). 
* [Un inquilino de RingCentral](https://www.ringcentral.com/office/plansandpricing.html)
* Una cuenta de usuario de RingCentral con permisos de administrador.

> [!NOTE]
> Esta integración también está disponible para usarse desde el entorno de la nube del gobierno de EE. UU de Azure AD. Puede encontrar esta aplicación en la galería de aplicaciones de la nube del gobierno de EE. UU. de Azure AD y configurarla de la misma manera que en la nube pública.


## <a name="step-1-plan-your-provisioning-deployment"></a>Paso 1. Planeación de la implementación de aprovisionamiento
1. Obtenga información sobre [cómo funciona el servicio de aprovisionamiento](../app-provisioning/user-provisioning.md).
2. Determine quién estará en el [ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Determine qué datos [asignar entre Azure AD y RingCentral](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-ringcentral-to-support-provisioning-with-azure-ad"></a>Paso 2. Configuración de RingCentral para admitir el aprovisionamiento con Azure AD

Se requiere una cuenta de administrador de [RingCentral](https://www.ringcentral.com/office/plansandpricing.html) para autorizar en la sección Credenciales del administrador del paso 5.

En el portal de administración de RingCentral, en Account Settings -> Directory Integrations (Configuración de la cuenta -> Integración de directorios), establezca el valor *Directory Provider* (Proveedor de directorios) en la imagen de *SCIM*
![](https://user-images.githubusercontent.com/49566142/134523440-20320d8e-3c25-4358-9ace-d4888ce8e4ea.png).


> [!NOTE]
> Para asignar licencias a los usuarios, consulte el vínculo del vídeo [aquí](https://support.ringcentral.com/s/article/5-10-Adding-Extensions-via-Web?language).

## <a name="step-3-add-ringcentral-from-the-azure-ad-application-gallery"></a>Paso 3. Incorporación de RingCentral desde la galería de aplicaciones de Azure AD

Para empezar a administrar el aprovisionamiento de RingCentral, agregue RingCentral desde la galería de aplicaciones de Azure AD. Si ha configurado previamente RingCentral para el inicio de sesión único, puede usar la misma aplicación. Sin embargo, se recomienda que cree una aplicación independiente al probar la integración inicialmente. Puede encontrar más información sobre cómo agregar una aplicación desde la galería [aquí](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Paso 4. Determinar quién estará en el ámbito de aprovisionamiento 

El servicio de aprovisionamiento de Azure AD le permite definir quién se aprovisionará, en función de la asignación a la aplicación y de los atributos del usuario o grupo. Si elige el ámbito del que se aprovisionará en la aplicación en función de la asignación, puede usar los pasos [siguientes](../manage-apps/assign-user-or-group-access-portal.md) para asignar usuarios y grupos a la aplicación. Si elige el ámbito del que se aprovisionará en función únicamente de los atributos del usuario o grupo, puede usar un filtro de ámbito, tal como se describe [aquí](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Al asignar usuarios y grupos a RingCentral, debe seleccionar un rol que no sea **Acceso predeterminado**. Los usuarios con el rol de acceso predeterminado se excluyen del aprovisionamiento y se marcarán como no autorizados en los registros de aprovisionamiento. Si el único rol disponible en la aplicación es el rol de acceso predeterminado, puede [actualizar el manifiesto de aplicación](../develop/howto-add-app-roles-in-azure-ad-apps.md) para agregar roles adicionales. 

* Empiece por algo pequeño. Pruebe con un pequeño conjunto de usuarios y grupos antes de implementarlo en todos. Cuando el ámbito del aprovisionamiento se define en los usuarios y grupos asignados, puede controlarlo asignando uno o dos usuarios o grupos a la aplicación. Cuando el ámbito se establece en todos los usuarios y grupos, puede especificar un [filtro de ámbito basado en atributos](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-ringcentral"></a>Paso 5. Configuración del aprovisionamiento automático de usuarios en RingCentral 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD a fin de crear, actualizar y deshabilitar usuarios o grupos en TestApp en función de las asignaciones de grupos o usuarios de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-ringcentral-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para RingCentral en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **RingCentral**.

    ![Vínculo de RingCentral en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Captura de pantalla de las opciones de administración con la opción Aprovisionamiento seleccionada.](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Captura de pantalla de la lista desplegable Modo de aprovisionamiento con la opción Automático seleccionada.](common/provisioning-automatic.png)

5. En **Credenciales de administrador**, haga clic en **Autorizar**. Se le redirigirá a la página de inicio de sesión de RingCentral. Escriba su correo electrónico/número de teléfono y contraseña, y haga clic en el botón **Sign In** (Iniciar sesión). Haga clic en **Authorize** (Autorizar) en la página **Access Request** (Solicitud de acceso) de RingCentral. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a RingCentral. Si la conexión no se establece, asegúrese de que la cuenta de RingCentral tiene permisos de administrador y pruebe otra vez.

   ![AAD](./media/ringcentral-provisioning-tutorial/admincredentials.png)

   ![Acceso](./media/ringcentral-provisioning-tutorial/authorize.png)

   ![Autorizar](./media/ringcentral-provisioning-tutorial/accessrequest.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que deba recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Seleccione **Guardar**.

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Pingboard** (Sincronizar usuarios de Azure Active Directory con RingCentral).

9. Revise los atributos de usuario que se sincronizan entre Azure AD y RingCentral en la sección **Attribute-Mappings** (Asignaciones de atributos). Los atributos seleccionados como propiedades de **Matching** (Coincidencia) se usan para buscar coincidencias con las cuentas de usuario de RingCentral con el objetivo de realizar operaciones de actualización. Si decide cambiar el [atributo de destino coincidente](../app-provisioning/customize-application-attributes.md), deberá asegurarse de que la API de RingCentral admite el filtrado de usuarios basado en ese atributo. Seleccione el botón **Guardar** para confirmar los cambios.

   |Atributo|Tipo|
   |---|---|
   |userName|String|
   |externalId|String|
   |active|Boolean|
   |title|String|
   |emails[type eq "work"].value|String|
   |addresses[type eq "work"].country|String|
   |addresses[type eq "work"].region|String|
   |addresses[type eq "work"].locality|String|
   |addresses[type eq "work"].postalCode|String|
   |addresses[type eq "work"].streetAddress|String|
   |name.givenName|String|
   |name.familyName|String|
   |phoneNumbers[type eq "mobile"].value|String|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|String|

10. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Para habilitar el servicio de aprovisionamiento de Azure AD para RingCentral, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

12. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en RingCentral.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

13. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia el ciclo de sincronización inicial de todos los usuarios y grupos definidos en **Ámbito** en la sección **Configuración**. El ciclo de sincronización inicial tarda más tiempo en realizarse que los ciclos posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. 

## <a name="step-6-monitor-your-deployment"></a>Paso 6. Supervisión de la implementación
Una vez configurado el aprovisionamiento, use los recursos siguientes para supervisar la implementación:

1. Use los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md) para determinar qué usuarios se han aprovisionado correctamente o sin éxito.
2. Consulte la [barra de progreso](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) para ver el estado del ciclo de aprovisionamiento y cuánto falta para que finalice.
3. Si la configuración de aprovisionamiento parece estar en mal estado, la aplicación pasará a estar en cuarentena. Más información sobre los estados de cuarentena [aquí](../app-provisioning/application-provisioning-quarantine-status.md).

## <a name="change-log"></a>Registro de cambios

* 10/09/2020: se ha eliminado la compatibilidad con los atributos "displayName" y "manager".
* 03/15/2021: se ha actualizado el método de autorización de usar un token de portador permanente a un flujo de concesión de código de OAuth.
* 28/10/2021: se ha actualizado la asignación predeterminada a **mail-> emails[type eq “work”].value**.
* 28/10/2021: limitación de velocidad actualizada a 300/min para lectura, 1000/min para escritura.

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)
