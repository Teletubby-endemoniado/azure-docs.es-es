---
title: 'Tutorial: Configuración de GoToMeeting para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y GoToMeeting.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 6c150788f6b4c5439a20995c8db83b0f50c95a65
ms.sourcegitcommit: f29615c9b16e46f5c7fdcd498c7f1b22f626c985
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2021
ms.locfileid: "129427103"
---
# <a name="tutorial-configure-gotomeeting-for-automatic-user-provisioning"></a>Tutorial: Configuración de GoToMeeting para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es explicar los pasos que debe realizar en GoToMeeting y Azure AD para aprovisionar y cancelar el aprovisionamiento de cuentas de usuario de Azure AD para GoToMeeting automáticamente.

## <a name="prerequisites"></a>Prerrequisitos

En la situación descrita en este tutorial se supone que ya cuenta con los elementos siguientes:

*   Un inquilino de Azure Active Directory.
*   Una suscripción habilitada para el inicio de sesión único en GoToMeeting.
*   Una cuenta de usuario de GoToMeeting con permisos de administrador de equipo.

## <a name="assigning-users-to-gotomeeting"></a>Asignación de usuarios a GoToMeeting

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de cuentas de usuario, solo se sincronizarán los usuarios y grupos que se han "asignado" a una aplicación de Azure AD.

Antes de configurar y habilitar el servicio de aprovisionamiento, tiene que decidir qué usuarios o grupos de Azure AD representan a los usuarios que necesitan acceso a la aplicación GoToMeeting. Una vez decidido, puede asignar estos usuarios a la aplicación de GoToMeeting siguiendo estas instrucciones:

[Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-gotomeeting"></a>Sugerencias importantes para asignar usuarios a GoToMeeting

*   Se recomienda asignar un único usuario de Azure AD a GoToMeeting para probar la configuración de aprovisionamiento. Más tarde, se pueden asignar otros usuarios o grupos.

*   Al asignar a un usuario a GoToMeeting, tiene que seleccionar un rol de usuario válido. El rol "Acceso predeterminado" no funciona para realizar el aprovisionamiento.

## <a name="enable-automated-user-provisioning"></a>Habilitación del aprovisionamiento automático de usuarios

Esta sección lo guía a través de los pasos necesarios para conectar la API de aprovisionamiento de cuentas de usuario de GoToMeeting, así como para configurar el servicio de aprovisionamiento con el fin de crear, actualizar y deshabilitar cuentas de usuario asignadas en GoToMeeting en función de la asignación de grupos y usuarios de Azure AD.

> [!TIP]
> También puede decidir habilitar el inicio de sesión único basado en SAML para GoToMeeting siguiendo las instrucciones de [Azure Portal](https://portal.azure.com). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-account-provisioning"></a>Para configurar el aprovisionamiento automático de cuentas de usuario, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), vaya a la sección **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

1. Si ya ha configurado GoToMeeting para el inicio de sesión único, busque la instancia de GoToMeeting con el campo de búsqueda. En caso contrario, seleccione **Agregar** y busque **GoToMeeting** en la galería de aplicaciones. Seleccione GoToMeeting en los resultados de búsqueda y agréguelo a la lista de aplicaciones.

1. Seleccione la instancia de GoToMeeting y después la pestaña **Aprovisionamiento**.

1. Establezca el **modo de aprovisionamiento** en **Automático**. 

    ![Captura de pantalla de la pestaña Aprovisionamiento de GoToMeeting en Azure Portal. El modo de aprovisionamiento se establece en Automático y Nombre de usuario de administrador, Contraseña y Probar conexión se resaltan.](https://user-images.githubusercontent.com/49566142/135871050-9d63861d-7963-47e0-bbdf-0e7c947e0b41.png)


1. En la sección Credenciales de administrador, haga clic en **Autorizar** e inicie sesión en GoToMeeting en las ventanas emergentes que aparecen.
   

1. En Azure Portal, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a la aplicación de GoToMeeting. Si se produce un error de conexión, asegúrese de que su cuenta de GoToMeeting tiene permisos de administrador de equipo y vuelva a intentar el paso de **"Credenciales de administración"** .


1. Haga clic en **Guardar**.

1. En la sección Asignaciones seleccione **Synchronize Azure Active Directory Users to GoToMeeting** (Sincronizar usuarios de Azure Active Directory con GoToMeeting).

1. En la sección **Asignaciones de atributos**, revise los atributos de usuario que se sincronizarán entre Azure AD y GoToMeeting. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de GoToMeeting con el objetivo de realizar operaciones de actualización. Seleccione el botón Guardar para confirmar los cambios.

1. Para habilitar el servicio de aprovisionamiento de Azure AD para GoToMeeting, cambie el **Estado de aprovisionamiento** a **Activado** en la sección Configuración

1. Haga clic en **Guardar**.

Comienza la sincronización inicial de todos los usuarios y grupos asignados a GoToMeeting en la sección Usuarios y grupos. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si se está ejecutando el servicio. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y hacer clic en los vínculos a los registros de actividad de aprovisionamiento, que describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento en la aplicación de GoToMeeting.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configuración del inicio de sesión único](./citrix-gotomeeting-tutorial.md)
