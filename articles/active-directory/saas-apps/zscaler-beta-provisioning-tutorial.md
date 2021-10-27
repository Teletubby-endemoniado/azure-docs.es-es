---
title: 'Tutorial: Configuración de Zscaler Beta para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Zscaler Beta.
services: active-directory
author: twimmers
writer: twimmers
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 809e400bd2dc8e5426670fde9cafd298c8e64ec8
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130045680"
---
# <a name="tutorial-configure-zscaler-beta-for-automatic-user-provisioning"></a>Tutorial: Configuración de Zscaler Beta para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en Zscaler Beta y Azure Active Directory (Azure AD) para configurar Azure AD con el objetivo de aprovisionar y desaprovisionar automáticamente usuarios o grupos en Zscaler Beta.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
>


## <a name="prerequisites"></a>Requisitos previos

En el escenario que se describe en este tutorial se supone que ya cuenta con los elementos siguientes:

* Un inquilino de Azure AD
* Un inquilino de Zscaler Beta
* Una cuenta de usuario de Zscaler Beta con permisos de administrador

> [!NOTE]
> La integración de aprovisionamiento de Azure AD se basa en la SCIM API de Zscaler Beta, que está disponible para los desarrolladores de Zscaler Beta para las cuentas con el paquete Enterprise.

## <a name="adding-zscaler-beta-from-the-gallery"></a>Incorporación de Zscaler Beta desde la galería

Antes de configurar Zscaler Beta para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Zscaler Beta desde la galería de aplicaciones de Azure AD hasta la lista de aplicaciones SaaS administradas.

**Para agregar Zscaler Beta desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Zscaler Beta**, seleccione **Zscaler Beta** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Zscaler Beta en la lista de resultados](common/search-new-app.png)

## <a name="assigning-users-to-zscaler-beta"></a>Asignación de usuarios a Zscaler Beta

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han "asignado" a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Zscaler Beta. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Zscaler Beta:

* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-zscaler-beta"></a>Sugerencias importantes para asignar usuarios a Zscaler Beta

* Se recomienda asignar un único usuario de Azure AD a Zscaler Beta para probar la configuración de aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a Zscaler Beta, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-zscaler-beta"></a>Configuración del aprovisionamiento automático de usuarios en Zscaler Beta

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios o grupos en Zscaler Beta en función de las asignaciones de grupos y usuarios de Azure AD.

> [!TIP]
> También puede optar por habilitar el inicio de sesión único basado en SAML para Zscaler Beta siguiendo las instrucciones del [tutorial de inicio de sesión único de Zscaler Beta](zscaler-beta-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

> [!NOTE]
> Cuando se aprovisionan o se desaprovisionan usuarios y grupos, se recomienda reiniciar el aprovisionamiento periódicamente para garantizar que las pertenencias a grupos se actualicen correctamente. Al ejecutar un reinicio, el servicio tendrá que evaluar de nuevo todos los grupos y actualizar las pertenencias.  

### <a name="to-configure-automatic-user-provisioning-for-zscaler-beta-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Zscaler Beta en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, a continuación, **Zscaler Beta**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Zscaler Beta**.

    ![El vínculo de Zscaler Beta en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Hay una lista de pestañas organizadas en categorías, titulada ZScaler Beta: Aprovisionamiento y aplicación empresarial. La pestaña Aprovisionamiento de la categoría Administrar está seleccionada.](./media/zscaler-beta-provisioning-tutorial/provisioning-tab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![El modo Automático se ha seleccionado en la lista desplegable Modo de aprovisionamiento. Hay campos para las credenciales de administrador, que se usan para conectarse a la API de Zscaler Beta, y hay un botón Probar conexión.](./media/zscaler-beta-provisioning-tutorial/provisioning-credentials.png)

5. En la sección **Admin Credentials** (Credenciales de administrador), especifique los valores de **Tenant URL** (URL de inquilino) y **Secret Token** (Token secreto) de la cuenta de Zscaler Beta, como se describe en el paso 6.

6. Para obtener la **dirección URL del inquilino** y el **token secreto**, vaya a **Administration > Authentication Settings** (Administración > Configuración de autenticación) en la interfaz de usuario del portal de Zscaler Beta y haga clic en  **SAML** en **Authentication Type** (Tipo de autenticación).

    ![En Configuración de la autenticación, en el perfil de autenticación, el tipo de directorio seleccionado es BD hospedada y el tipo de autenticación seleccionado es SAML.](./media/zscaler-beta-provisioning-tutorial/secret-token-1.png)

    Haga clic en **Configure SAML** (Configurar SAML) para abrir las opciones de **configuración de SAML**.

    ![En Configure SAML (Configurar SAML), se seleccionan las opciones Enable SAML Auto-Provisioning (Habilitar aprovisionamiento automático de SAML) y Enable SCIM-Based Provisioning (Habilitar aprovisionamiento basado en SCIM). Los cuadros de texto URL Base y Token de portador aparecen resaltados.](./media/zscaler-beta-provisioning-tutorial/secret-token-2.png)

    Seleccione **Enable SCIM-Based Provisioning** (Habilitar aprovisionamiento basado en SCIM) para recuperar **Base URL** (URL Base) y **Bearer Token** (Token de portador) y, después, guarde la configuración. Copie la **dirección URL Base** en la **dirección URL del inquilino** y el **token de portador** en el **token secreto** en Azure Portal.

7. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Zscaler Beta. Si la conexión no se establece, asegúrese de que la cuenta de Zscaler Beta tiene permisos de administrador y pruebe de nuevo.

    ![En las credenciales de administrador, los campos URL de inquilino y Token secreto contienen valores y el botón Probar conexión aparece resaltado.](./media/zscaler-beta-provisioning-tutorial/test-connection.png)

8. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![El cuadro de texto Correo electrónico de notificación está vacío y la casilla Enviar una notificación por correo electrónico cuando se produzca un error está desactivada.](./media/zscaler-beta-provisioning-tutorial/notification.png)

9. Haga clic en **Save**(Guardar).

10. En la sección **Mappings** (Asignaciones), seleccione **Synchronize Azure Active Directory Users to Zscaler Beta** (Sincronizar usuarios de Azure Active Directory con Zscaler Beta).

    ![La casilla Synchronize Azure Active Directory Users to ZscalerBeta (Sincronizar usuarios de Azure Active Directory con ZscalerBeta) está seleccionada y habilitada.](./media/zscaler-beta-provisioning-tutorial/user-mappings.png)

11. Examine los atributos de usuario que se sincronizan entre Azure AD y Zscaler Beta en la sección **Attribute Mappings** (Asignaciones de atributos). Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Zscaler Beta con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![En la sección Asignaciones de atributos de los atributos de usuario, se muestran los atributos de Active Directory junto a los atributos de Zscaler Beta con los que están sincronizados. Un par de atributos aparecen como coincidentes.](./media/zscaler-beta-provisioning-tutorial/user-attribute-mappings.png)

12. En la sección **Mappings** (Asignaciones), seleccione **Synchronize Azure Active Directory Groups to Zscaler Beta** (Sincronizar grupos de Azure Active Directory con Zscaler Beta).

    ![La opción Synchronize Azure Active Directory Groups to ZScalerBeta (Sincronizar grupos de Azure Active Directory con ZScalerBeta) está seleccionada y habilitada.](./media/zscaler-beta-provisioning-tutorial/group-mappings.png)

13. Examine los atributos de grupo que se sincronizan entre Azure AD y Zscaler Beta en la sección **Asignaciones de atributos**. Los atributos seleccionados como propiedades de **Matching** (Coincidencia) se usan para buscar coincidencias con los grupos de Zscaler Beta para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![En la sección Asignaciones de atributos de los atributos de los grupos, se muestran los atributos de Active Directory junto a los atributos de Zscaler Beta con los que están sincronizados. Un par de atributos aparecen como coincidentes.](./media/zscaler-beta-provisioning-tutorial/group-attribute-mappings.png)

14. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

15. Para habilitar el servicio de aprovisionamiento de Azure AD para Zscaler Beta, cambie el valor de **Provisioning Status** (Estado de aprovisionamiento) a **On** (Activado) en la sección **Settings** (Configuración).

    ![El estado de aprovisionamiento aparece y está establecido en Activado.](./media/zscaler-beta-provisioning-tutorial/provisioning-status.png)

16. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Zscaler Beta.

    ![Aparece la lista desplegable Ámbito y la opción Sincronizar solo los usuarios y grupos asignados está seleccionada. El otro valor disponible es Sincronizar todos los usuarios y grupos.](./media/zscaler-beta-provisioning-tutorial/scoping.png)

17. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![El botón Save (Guardar) situado en la parte superior de Zscaler Beta: Aprovisionamiento aparece resaltado. También hay un botón Discard (Descartar).](./media/zscaler-beta-provisioning-tutorial/save-provisioning.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Zscaler Beta.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-03.png