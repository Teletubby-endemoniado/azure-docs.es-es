---
title: 'Tutorial: Integración de Azure Active Directory con Predictix Ordering | Microsoft Docs'
description: Con este tutorial aprenderá a configurar el inicio de sesión único entre Azure Active Directory y Predictix Ordering.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 7408e5203d0b2bc05895ae8ba0403ab9718d1fb9
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124789814"
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-ordering"></a>Tutorial: Integración de Azure Active Directory con Predictix Ordering

En este tutorial aprenderá a integrar Predictix Ordering con Azure Active Directory (Azure AD).
Esta integración ofrece las siguientes ventajas:

* Puede usar Azure AD para controlar quién tiene acceso a Predictix Ordering.
* Puede permitir que los usuarios inicien sesión automáticamente en Predictix Ordering (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para más información acerca de la integración de aplicaciones SaaS con Azure AD, consulte [Inicio de sesión único en aplicaciones de Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Predictix Ordering, necesita:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción a Predictix Ordering con el inicio de sesión único habilitado.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial configurará y probará el inicio de sesión único de Azure AD en un entorno de prueba.

* Predictix Ordering admite el inicio de sesión único iniciado por SP.

## <a name="add-predictix-ordering-from-the-gallery"></a>Adición de Predictix Ordering desde la galería

Para configurar la integración de Predictix Ordering en Azure AD, deberá agregar la aplicación desde la galería a la lista de aplicaciones SaaS administradas.

1. En [Azure Portal](https://portal.azure.com), en el panel izquierdo, seleccione **Azure Active Directory**:

    ![Seleccione Azure Active Directory.](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**:

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación, seleccione **Nueva aplicación** en la parte superior de la ventana:

    ![Seleccionar Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Predictix Ordering**. Seleccione **Predictix Ordering** en los resultados de búsqueda y, después, **Agregar**.

     ![Search Results](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección podrá configurar y probar el inicio de sesión único de Azure AD con Predictix Ordering con un usuario de prueba llamado Britta Simon.
Para habilitar el inicio de sesión único, tendrá que establecer una relación entre un usuario de Azure AD y el usuario correspondiente de Predictix Ordering.

Para configurar y probar el inicio de sesión único de Azure AD con Predictix Ordering, es preciso completar los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** para permitir que los usuarios utilicen esta característica.
2. **[Configuración del inicio de sesión único en Predictix Ordering](#configure-predictix-ordering-single-sign-on)** en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** para probar el inicio de sesión único de Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** para permitir el inicio de sesión único de Azure AD para el usuario.
5. **[Creación de un usuario de prueba en Predictix Ordering](#create-a-predictix-ordering-test-user)** vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** para comprobar que la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Predictix Ordering, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Predictix Ordering**, seleccione **Inicio de sesión único**:

    ![Seleccionar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Selección de un método de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, seleccione el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**:

    ![Icono Editar](common/edit-urls.png)

4. En el cuadro de diálogo **Configuración básica de SAML**, siga los pasos que se indican a continuación.

    ![Cuadro de diálogo Configuración básica de SAML](common/sp-identifier.png)

    1. En el cuadro **URL de inicio de sesión**, escriba una dirección URL con el siguiente formato:

       `https://<companyname-pricing>.ordering.predictix.com/sso/request`

    1. En el cuadro **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente formato:

        ```https
        https://<companyname-pricing>.dev.ordering.predictix.com
        https://<companyname-pricing>.ordering.predictix.com
        ```

    > [!NOTE]
    > Estos valores son marcadores de posición. Debe utilizar la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Predictix Ordering](https://www.predix.io/support/) para obtener estos valores. También puede consultar los patrones que se muestran en el cuadro de diálogo **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione el vínculo **Descargar** situado junto a **Certificado (Base64)** , según sus requisitos, y guarde el certificado en el equipo:

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Predictix Ordering** (Configurar Predictix Ordering), copie las direcciones URL que necesite:

    ![Copia de las direcciones URL de configuración](common/copy-configuration-urls.png)

    1. **Dirección URL de inicio de sesión**

    2. **Identificador de Azure AD**.

    3. **Dirección URL de cierre de sesión**

### <a name="configure-predictix-ordering-single-sign-on"></a>Configuración del inicio de sesión único en Predictix Ordering

Para configurar el inicio de sesión único en Predictix Ordering, es preciso enviar el certificado descargado y las direcciones URL copiadas de Azure Portal al [equipo de soporte técnico de Predictix Ordering](https://www.predix.io/support/). El equipo se asegurará de que la conexión de inicio de sesión único de SAML esté establecida correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección creará un usuario de prueba llamado Britta Simon en Azure Portal.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**:

    ![Selección de Todos los usuarios](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Selección de Nuevo usuario](common/new-user.png)

3. En el cuadro de diálogo **Usuario**, siga los pasos que se indican a continuación.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    1. En el cuadro **Nombre**, escriba **BrittaSimon**.
  
    1. En el cuadro **Nombre de usuario**, escriba **BrittaSimon@\<yourcompanydomain>.\<extension>** (Por ejemplo, BrittaSimon@contoso.com).

    1. Seleccione **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección se habilita que Britta Simon use el inicio de sesión único de Azure AD concediéndole acceso a Predictix Ordering.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Predictix Ordering**:

    ![Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Predictix Ordering**.

    ![Lista de aplicaciones](common/all-applications.png)

3. En el panel izquierdo, seleccione **Usuarios y grupos**:

    ![Seleccionar Usuarios y grupos](common/users-groups-blade.png)

4. Seleccione **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Selección de Agregar usuario](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera algún valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione el rol adecuado para el usuario en la lista. Haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, seleccione **Asignar**.

### <a name="create-a-predictix-ordering-test-user"></a>Creación de un usuario de prueba de Predictix Ordering

A continuación debe crear un usuario llamado Britta Simon en Predictix Ordering. Trabaje con el [equipo de soporte técnico de Predictix Ordering](https://www.predix.io/support/) para agregar usuarios. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

Ahora, debe probar la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al seleccionar el icono de Predictix Ordering en el panel de acceso, debería iniciar sesión automáticamente en la instancia de Predictix Ordering para la que configurara el inicio de sesión único. Para más información, consulte [Acceso y uso del aplicaciones en el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Tutoriales para integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)