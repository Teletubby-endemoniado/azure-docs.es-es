---
title: 'Tutorial: Integración de Azure Active Directory con RolePoint | Microsoft Docs'
description: En este tutorial, aprenderá a configurar el inicio de sesión único entre Azure Active Directory y RolePoint.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/15/2019
ms.author: jeedes
ms.openlocfilehash: 7a2a0985b7e1dabace66548abd3faadf993d89ee
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124821775"
---
# <a name="tutorial-azure-active-directory-integration-with-rolepoint"></a>Tutorial: Integración de Azure Active Directory con RolePoint

En este tutorial, aprenderá a integrar RolePoint con Azure Active Directory (Azure AD).
Esta integración ofrece las siguientes ventajas:

* Puede usar Azure AD para controlar quién tiene acceso a RolePoint.
* Puede permitir que los usuarios inicien sesión automáticamente en RolePoint (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para más información acerca de la integración de aplicaciones SaaS con Azure AD, consulte [Inicio de sesión único en aplicaciones de Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Para configurar la integración de Azure AD con RolePoint, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción de RolePoint habilitada para el inicio de sesión único.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial configurará y probará el inicio de sesión único de Azure AD en un entorno de prueba.

* RolePoint admite el inicio de sesión único iniciado por SP.

## <a name="add-rolepoint-from-the-gallery"></a>Adición de RolePoint desde la galería

Para configurar la integración de RolePoint en Azure AD, deberá agregar RolePoint desde la galería a la lista de aplicaciones SaaS administradas.

1. En [Azure Portal](https://portal.azure.com), en el panel izquierdo, seleccione **Azure Active Directory**:

    ![Seleccione Azure Active Directory.](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**:

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación, seleccione **Nueva aplicación** en la parte superior de la ventana:

    ![Seleccionar Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **RolePoint**. Seleccione **RolePoint** en los resultados de búsqueda y, a continuación, seleccione **Agregar**.

     ![Search Results](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con RolePoint con un usuario de prueba llamado Britta Simon.
Para habilitar el inicio de sesión único, tendrá que establecer una relación entre un usuario de Azure AD y el usuario correspondiente de RolePoint.

Para configurar y probar el inicio de sesión único de Azure AD con RolePoint, debe hacer lo siguiente:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** para permitir que los usuarios utilicen esta característica.
2. **[Configure el inicio de sesión único en RolePoint](#configure-rolepoint-single-sign-on)** en el lado de la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** para probar el inicio de sesión único de Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** para permitir el inicio de sesión único de Azure AD para el usuario.
5. **[Cree un usuario de prueba de RolePoint](#create-a-rolepoint-test-user)** que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** para comprobar que la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con RolePoint, haga lo siguiente:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación RolePoint, seleccione **Inicio de sesión único**:

    ![Selección de inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Selección de un método de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, seleccione el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**:

    ![Icono Editar](common/edit-urls.png)

4. En el cuadro de diálogo **Configuración básica de SAML**, siga los pasos que se indican a continuación.

    ![Cuadro de diálogo Configuración básica de SAML](common/sp-identifier.png)

    1. En el cuadro **URL de inicio de sesión**, escriba una dirección URL con el siguiente formato:

       `https://<subdomain>.rolepoint.com/login`

    1. En el cuadro **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente formato:

       `https://app.rolepoint.com/<instancename>`

    > [!NOTE]
    > Estos valores son marcadores de posición. Debe utilizar la dirección URL y el identificador reales de inicio de sesión. Se recomienda que utilice un valor de cadena único en el identificador. Póngase en contacto con el [equipo de soporte técnico de cliente de RolePoint](mailto:info@rolepoint.com) para obtener estos valores. También puede consultar los patrones que se muestran en el cuadro de diálogo **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione el vínculo **Descargar** situado junto a **XML de metadatos de federación**, según sus requisitos, y guarde el archivo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Configurar RolePoint**, copie las direcciones URL adecuadas según sus necesidades:

    ![Copia de las direcciones URL de configuración](common/copy-configuration-urls.png)

    1. **Dirección URL de inicio de sesión**

    1. **Identificador de Azure AD**.

    1. **Dirección URL de cierre de sesión**


### <a name="configure-rolepoint-single-sign-on"></a>Configuración del inicio de sesión único de RolePoint

Para configurar el inicio de sesión único en RolePoint, tiene que trabajar con el [equipo de soporte técnico de RolePoint](mailto:info@rolepoint.com). Envíe a este equipo el archivo XML de metadatos de federación y las direcciones URL que obtuvo de Azure Portal. Se ocupará de configurar RolePoint para garantizar que la conexión de inicio de sesión único de SAML está establecida correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección creará un usuario de prueba llamado Britta Simon en Azure Portal.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**:

    ![Selección de Todos los usuarios](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la ventana:

    ![Selección de Nuevo usuario](common/new-user.png)

3. En el cuadro de diálogo **Usuario**, siga los pasos que se indican a continuación.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    1. En el cuadro **Nombre**, escriba **BrittaSimon**.
  
    1. En el cuadro **Nombre de usuario**, escriba **BrittaSimon@\<yourcompanydomain>.\<extension>** (Por ejemplo, BrittaSimon@contoso.com).

    1. Seleccione **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a RolePoint.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **RolePoint**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **RolePoint**.

    ![Lista de aplicaciones](common/all-applications.png)

3. En el panel izquierdo, seleccione **Usuarios y grupos**:

    ![Seleccionar Usuarios y grupos](common/users-groups-blade.png)

4. Seleccione **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Selección de Agregar usuario](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la ventana.

6. Si espera algún valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione el rol adecuado para el usuario en la lista. Haga clic en el botón **Seleccionar** de la parte inferior de la ventana.

7. En el cuadro de diálogo **Agregar asignación**, seleccione **Asignar**.

### <a name="create-a-rolepoint-test-user"></a>Creación de un usuario de prueba de RolePoint

A continuación, deberá crear un usuario llamado a Britta Simon en RolePoint. Trabaje con el [equipo de soporte técnico de RolePoint](mailto:info@rolepoint.com) para agregar usuarios a RolePoint. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

Ahora, debe probar la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al seleccionar el icono de RolePoint en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de RolePoint para la que configuró el inicio de sesión único. Para más información acerca del Panel de acceso, consulte [Access and use apps on the My Apps portal](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510) (Uso de aplicaciones y acceso a ellas en el portal Aplicaciones).

## <a name="additional-resources"></a>Recursos adicionales

- [Tutoriales para integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)