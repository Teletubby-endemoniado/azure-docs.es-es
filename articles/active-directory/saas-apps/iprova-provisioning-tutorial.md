---
title: 'Tutorial: Configuración de iProva para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente cuentas de usuario en iProva.
services: active-directory
author: twimmers
writer: twimmers
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/29/2019
ms.author: thwimmer
ms.openlocfilehash: e3ff62f4099e4098c2ca695d0e7c07bbc5c08b0a
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121746098"
---
# <a name="tutorial-configure-iprova-for-automatic-user-provisioning"></a>Tutorial: Configuración de iProva para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se deben realizar en iProva y Azure Active Directory (Azure AD) a fin de configurar Azure AD para aprovisionar y desaprovisionar automáticamente usuarios o grupos en [iProva](https://www.iProva.com/). Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md). Antes de intentar usar este tutorial, asegúrese de que conoce y cumple todos los requisitos. Si tiene alguna duda, póngase en contacto con Infoland.

> [!NOTE]
> Este conector está actualmente en versión preliminar pública. Para más información sobre los términos de uso generales de Microsoft Azure para las características en versión preliminar, consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="capabilities-supported"></a>Funcionalidades admitidas
> [!div class="checklist"]
> * Crear usuarios en iProva
> * Quitar o deshabilitar usuarios de iProva cuando ya no necesiten acceso
> * Mantener los atributos de usuario sincronizados entre Azure AD e iProva
> * Aprovisionar grupos y pertenencias a grupos en iProva
> * [Inicio de sesión único](./iprova-tutorial.md) en iProva (recomendado)

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* [Un inquilino de Azure AD](../develop/quickstart-create-new-tenant.md).
* Una cuenta de usuario en Azure AD con [permiso](../roles/permissions-reference.md) para configurar el aprovisionamiento (por ejemplo, Administrador de aplicaciones, Administrador de aplicaciones en la nube, Propietario de la aplicación o Administrador global).
* [Un inquilino de iProva](https://www.iProva.com/).
* Una cuenta de usuario de iProva con permisos de administrador

## <a name="step-1-plan-your-provisioning-deployment"></a>Paso 1. Planeación de la implementación de aprovisionamiento
1. Obtenga información sobre [cómo funciona el servicio de aprovisionamiento](../app-provisioning/user-provisioning.md).
2. Determine quién estará en el [ámbito de aprovisionamiento](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
3. Determine qué datos quiere [asignar entre Azure AD e iProva](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-iprova-to-support-provisioning-with-azure-ad"></a>Paso 2. Configuración de iProva para admitir el aprovisionamiento con Azure AD

1. Inicie sesión en la [consola de administración de iProva](https://www.iProva.com/). Vaya a **Go to > Application Management** (Ir a > Administración de aplicaciones).

    ![Consola de administración de iProva](media/iprova-provisioning-tutorial/admin.png)

2.  Haga clic en **External user management** (Administración de usuarios externos).

    ![Incorporación de SCIM en iProva](media/iprova-provisioning-tutorial/external.png)

3. Para agregar un nuevo proveedor, haga clic en el icono **más**. En el cuadro de diálogo **Add provider** (Agregar proveedor), indique un valor en **Title** (Título). Puede optar por agregar un valor en **IP-based access restriction** (Restricción de acceso basada en IP). Haga clic en el botón **OK** (Aceptar).

    ![Incorporación en iProva](media/iprova-provisioning-tutorial/add.png)

    ![Incorporación de proveedor en iProva](media/iprova-provisioning-tutorial/addprovider.png)

4.  Haga clic en el botón **Permanent token** (Token permanente). Copie el valor de **Permanent token** (Token permanente) y guárdelo, ya que esta será la única vez que podrá verlo. Este valor se escribirá en el campo Token secreto de la pestaña Aprovisionamiento de la aplicación iProva en Azure Portal.

    ![Creación del token en iProva](media/iprova-provisioning-tutorial/token.png)

## <a name="step-3-add-iprova-from-the-azure-ad-application-gallery"></a>Paso 3. Adición de iProva desde la galería de aplicaciones de Azure AD

Para empezar a administrar el aprovisionamiento de iProva, agregue iProva desde la galería de aplicaciones de Azure AD. Si ha configurado previamente iProva para el inicio de sesión único, puede usar la misma aplicación. Sin embargo, se recomienda que cree una aplicación independiente al probar la integración inicialmente. Puede encontrar más información sobre cómo agregar una aplicación desde la galería [aquí](../manage-apps/add-application-portal.md). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Paso 4. Determinar quién estará en el ámbito de aprovisionamiento 

El servicio de aprovisionamiento de Azure AD le permite definir quién se aprovisionará, en función de la asignación a la aplicación y de los atributos del usuario o grupo. Si elige el ámbito del que se aprovisionará en la aplicación en función de la asignación, puede usar los pasos [siguientes](../manage-apps/assign-user-or-group-access-portal.md) para asignar usuarios y grupos a la aplicación. Si elige el ámbito del que se aprovisionará en función únicamente de los atributos del usuario o grupo, puede usar un filtro de ámbito, tal como se describe [aquí](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Al asignar usuarios y grupos a para empezar a administrar el aprovisionamiento en iProva, debe seleccionar un rol que no sea **Acceso predeterminado**. Los usuarios con el rol de acceso predeterminado se excluyen del aprovisionamiento y se marcarán como no autorizados en los registros de aprovisionamiento. Si el único rol disponible en la aplicación es el rol de acceso predeterminado, puede [actualizar el manifiesto de aplicación](../develop/howto-add-app-roles-in-azure-ad-apps.md) para agregar roles adicionales. 

* Empiece por algo pequeño. Pruebe con un pequeño conjunto de usuarios y grupos antes de implementarlo en todos. Cuando el ámbito del aprovisionamiento se define en los usuarios y grupos asignados, puede controlarlo asignando uno o dos usuarios o grupos a la aplicación. Cuando el ámbito se establece en todos los usuarios y grupos, puede especificar un [filtro de ámbito basado en atributos](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

## <a name="step-5-configure-automatic-user-provisioning-to-iprova"></a>Paso 5. Configuración del aprovisionamiento automático de usuarios en iProva 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD para crear, actualizar y deshabilitar usuarios o grupos en iProva en función de las asignaciones de usuarios o grupos de Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-iprova-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para iProva en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **iProva**.

    ![Vínculo a iProva en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Captura de pantalla de las opciones de administración con la opción Aprovisionamiento seleccionada.](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Captura de pantalla de la lista desplegable Modo de aprovisionamiento con la opción Automático seleccionada.](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, escriba los valores de **URL base de SCIM 2.0 y token permanente** que recuperó anteriormente en los campos **URL de inquilino** y agregue /scim/. Agregue también el **token secreto**. Puede generar un token secreto en iProva mediante el botón de **token permanente**. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a iProva. Si la conexión no se establece, asegúrese de que la cuenta de iProva tiene permisos de administrador y pruebe otra vez. 

    ![URL de inquilino + Token](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Haga clic en **Save**(Guardar).

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to iProva** (Sincronizar usuarios de Azure Active Directory con iProva).

9. Revise los atributos de usuario que se sincronizan entre Azure AD e iProva en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de iProva con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

   |Atributo|Tipo|
   |---|---|
   |active|Boolean|
   |DisplayName|String|
   |emails[type eq "work"].value|String|
   |preferredLanguage|String|
   |userName|String|
   |phoneNumbers[type eq "work"].value|String|
   |externalId|String|



10. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to iProva** (Sincronizar grupos de Azure Active Directory con iProva).

11. Revise los atributos de grupo que se sincronizan entre Azure AD e iProva en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para establecer correspondencia con los grupos en iProva con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

      |Atributo|Tipo|
      |---|---|
      |DisplayName|String|
      |members|Referencia|
      |externalId|String|

12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de aprovisionamiento de Azure AD para iProva, cambie el valor de **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

14. Seleccione los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que quiere aprovisionar en iProva.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. 


## <a name="step-6-monitor-your-deployment"></a>Paso 6. Supervisión de la implementación
Una vez configurado el aprovisionamiento, use los recursos siguientes para supervisar la implementación:

1. Use los [registros de aprovisionamiento](../reports-monitoring/concept-provisioning-logs.md) para determinar qué usuarios se han aprovisionado correctamente o sin éxito.
2. Consulte la [barra de progreso](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) para ver el estado del ciclo de aprovisionamiento y cuánto falta para que finalice.
3. Si la configuración de aprovisionamiento parece estar en mal estado, la aplicación pasará a estar en cuarentena. Más información sobre los estados de cuarentena [aquí](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="change-log"></a>Registro de cambios

* 17/06/2020: se ha quitado el atributo de extensión de empresa "Manager".

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)
