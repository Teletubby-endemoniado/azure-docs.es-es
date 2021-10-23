---
title: 'Tutorial: Configuración de Netpresenter para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a aprovisionar y desaprovisionar de forma automática las cuentas de usuario de Azure AD para Netpresenter.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: fc948927-6169-4da7-9829-3f25ec5be5c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2021
ms.author: Zhchia
ms.openlocfilehash: 0a4961a23c8ea9a894028af0fc596d399c0f3e8f
ms.sourcegitcommit: e82ce0be68dabf98aa33052afb12f205a203d12d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2021
ms.locfileid: "129661689"
---
# <a name="tutorial-configure-netpresenter-for-automatic-user-provisioning"></a>Tutorial: Configuración de Netpresenter para el aprovisionamiento automático de usuarios

En este tutorial se describen los pasos que debe seguir en Netpresenter y Azure Active Directory (Azure AD) para configurar el aprovisionamiento automático de usuarios. Una vez configurado, Azure AD aprovisiona y desaprovisiona usuarios y grupos de manera automática en [Netpresenter](https://www.Netpresenter.com/) mediante el servicio de aprovisionamiento de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funcionalidades admitidas
> [!div class="checklist"]
> * Creación de usuarios en Netpresenter
> * Eliminación de usuarios de Netpresenter cuando ya no necesiten acceso
> * Sincronización de los atributos de usuario entre Azure AD y Netpresenter

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* [Un inquilino de Azure AD](../develop/quickstart-create-new-tenant.md) 
* Una cuenta de usuario en Azure AD con [permiso](../roles/permissions-reference.md) para configurar el aprovisionamiento (por ejemplo, Administrador de aplicaciones, Administrador de aplicaciones en la nube, Propietario de la aplicación o Administrador global). 
* Una cuenta de administrador con Netpresenter.

## <a name="step-1-plan-your-provisioning-deployment"></a>Paso 1. Planeación de la implementación de aprovisionamiento
1. Obtenga información sobre [cómo funciona el servicio de aprovisionamiento](../app-provisioning/user-provisioning.md).
2. Determine quién estará en el [ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Determine qué datos quiere [asignar entre Azure AD y Netpresenter](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-netpresenter-to-support-provisioning-with-azure-ad"></a>Paso 2. Configuración de Netpresenter para admitir el aprovisionamiento con Azure AD

1. Inicie sesión en Netpresenter con una cuenta de administrador.
2. Haga clic en el icono de engranaje para ir a la página de configuración.
3. En la página de configuración, haga clic en **System** (Sistema) para abrir el submenú y, después, en **Azure AD**.
4. Haga clic en el botón **Generate Token** (Generar token).
5. Guarde **SCIM Endpoint URL** (Dirección URL del punto de conexión de SCIM) y **Token** en un lugar seguro; los necesitará en el **paso 5**.

   ![Token y dirección URL](media/netpresenter/get-token-and-url.png)

1. **Opcional**: en **Sign in options** (Opciones de inicio de sesión), se puede habilitar o deshabilitar "Force sign in with Microsoft" (Forzar inicio de sesión con Microsoft). Al habilitarlo, los usuarios con una cuenta de Azure AD perderán la capacidad de iniciar sesión con su cuenta local.

## <a name="step-3-add-netpresenter-from-the-azure-ad-application-gallery"></a>Paso 3. Incorporación de Netpresenter desde la galería de aplicaciones de Azure AD

Para empezar a administrar el aprovisionamiento de Netpresenter, agréguelo desde la galería de aplicaciones de Azure AD. Si ha configurado Netpresenter previamente para el inicio de sesión único, puede usar la misma aplicación. Sin embargo, se recomienda que cree una aplicación independiente al probar la integración inicialmente. Puede encontrar más información sobre cómo agregar una aplicación desde la galería [aquí](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Paso 4. Determinar quién estará en el ámbito de aprovisionamiento 

El servicio de aprovisionamiento de Azure AD le permite definir quién se aprovisionará, en función de la asignación a la aplicación y de los atributos del usuario o grupo. Si elige el ámbito del que se aprovisionará en la aplicación en función de la asignación, puede usar los pasos [siguientes](../manage-apps/assign-user-or-group-access-portal.md) para asignar usuarios y grupos a la aplicación. Si elige el ámbito del que se aprovisionará en función únicamente de los atributos del usuario o grupo, puede usar un filtro de ámbito, tal como se describe [aquí](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Al asignar usuarios y grupos a Netpresenter, debe seleccionar un rol distinto de **Acceso predeterminado**. Los usuarios con el rol de acceso predeterminado se excluyen del aprovisionamiento y se marcarán como no autorizados en los registros de aprovisionamiento. Si el único rol disponible en la aplicación es el rol de acceso predeterminado, puede [actualizar el manifiesto de la aplicación](../develop/howto-add-app-roles-in-azure-ad-apps.md) para agregar otros roles. 

* Empiece por algo pequeño. Pruebe con un pequeño conjunto de usuarios y grupos antes de implementarlo en todos. Cuando el ámbito del aprovisionamiento se define en los usuarios y grupos asignados, puede controlarlo asignando uno o dos usuarios o grupos a la aplicación. Cuando el ámbito se establece en todos los usuarios y grupos, puede especificar un [filtro de ámbito basado en atributos](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-netpresenter"></a>Paso 5. Configuración del aprovisionamiento automático de usuarios en Netpresenter 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD a fin de crear, actualizar y deshabilitar usuarios o grupos en TestApp en función de las asignaciones de grupos o usuarios de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-netpresenter-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios en Netpresenter en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Netpresenter**.

    ![Vínculo de Netpresenter de la lista Aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña de aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, escriba los valores de URL de inquilino y de token secreto. Haga clic en **Probar conexión** para asegurarse de que Azure AD pueda conectarse a Netpresenter. Si la conexión no se establece, asegúrese de que la cuenta de Netpresenter tenga permisos de administrador e inténtelo de nuevo.

    ![Token](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que deba recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Seleccione **Guardar**.

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Netpresenter** (Sincronizar usuarios de Azure Active Directory con Netpresenter).

9. Revise los atributos de usuario que se sincronizan entre Azure AD y Netpresenter en la sección **Asignaciones de atributos**. Los atributos seleccionados como propiedades **Coincidentes** se usan para buscar coincidencias con las cuentas de usuario de Netpresenter con el objetivo de realizar operaciones de actualización. Si decide cambiar el [atributo de destino coincidente](../app-provisioning/customize-application-attributes.md), deberá asegurarse de que la API de Netpresenter admita el filtrado de usuarios basado en ese atributo. Seleccione el botón **Guardar** para confirmar los cambios.

    |Atributo|Tipo|Compatible con el filtrado
    |---|---|---|
    |userName|String|&check;
    |externalId|String|&check;
    |emails[type eq "work"].value|String|&check;
    |active|Boolean|
    |name.givenName|String|
    |name.familyName|String|
    |phoneNumbers[type eq "work"].value|String|
    |phoneNumbers[type eq "mobile"].value|String|

10. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Para habilitar el servicio de aprovisionamiento de Azure AD en Netpresenter, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

12. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Netpresenter.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

13. Cuando esté preparado para realizar el aprovisionamiento, haga clic en **Guardar**.

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
