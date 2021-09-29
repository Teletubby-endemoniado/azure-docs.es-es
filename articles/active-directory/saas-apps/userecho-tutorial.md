---
title: 'Tutorial: Integración de Azure Active Directory con UserEcho | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y UserEcho.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: fb98d34d5f31b339010fa2c47a135dcf0a1cdf7c
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124737978"
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Tutorial: integración de Azure Active Directory con UserEcho

En este tutorial, aprenderá a integrar UserEcho con Azure Active Directory (Azure AD).
Integrar UserEcho con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a UserEcho.
* Puede permitir que los usuarios inicien sesión automáticamente en UserEcho (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Para configurar la integración de Azure AD con UserEcho, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción a UserEcho habilitada para inicio de sesión único

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* UserEcho admite SSO iniciado por **SP**

## <a name="adding-userecho-from-the-gallery"></a>Incorporación de UserEcho desde la galería

Para configurar la integración de UserEcho en Azure AD, deberá agregar UserEcho desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar UserEcho desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **UserEcho**, seleccione **UserEcho** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![UserEcho en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con UserEcho con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de UserEcho.

Para configurar y probar el inicio de sesión único de Azure AD con UserEcho, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de UserEcho](#configure-userecho-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de UserEcho](#create-userecho-test-user)** : para tener un homólogo de Britta Simon en UserEcho que esté vinculado a su representación en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con UserEcho, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **UserEcho**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de UserEcho](common/sp-identifier.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.userecho.com/`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de UserEcho](https://feedback.userecho.com/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up UserEcho** (Configurar UserEcho), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-userecho-single-sign-on"></a>Configuración del inicio de sesión único en UserEcho

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de UserEcho como administrador.

2. En la barra de herramientas de la parte superior, haga clic en el nombre de usuario para expandir el menú y, después, haga clic en **Instalación**.
   
    ![Captura de pantalla que muestra Setup (Configuración) seleccionado en el sitio de UserEcho.](./media/userecho-tutorial/tutorial_userecho_06.png) 

3. Haga clic en **Integraciones**.
   
    ![Captura de pantalla que muestra la opción Integrations (Integraciones) seleccionada en el menú Settings (Configuración).](./media/userecho-tutorial/tutorial_userecho_07.png) 

4. Haga clic en **Sitio web** y, después, en **Inicio de sesión único (SAML2)** .
   
    ![Captura de pantalla que muestra la opción Single sign-on SAML2 (Inicio de sesión único [S A M L 2]) seleccionada en el menú Integrations (Integraciones).](./media/userecho-tutorial/tutorial_userecho_08.png) 

5. Siga estos pasos en la página **Inicio de sesión único (SAML)** :
   
    ![Captura de pantalla que muestra la página Single Sign-on SAML (Inicio de sesión único [S A M L]), donde puede escribir los valores descritos.](./media/userecho-tutorial/tutorial_userecho_09.png)
    
    a. En **Habilitado para SAML**, seleccione **Sí**.
    
    b. Pegue el valor de **Dirección URL de inicio de sesión** que ha copiado de Azure Portal en el cuadro de texto **SAML SSO URL** (Dirección URL de inicio de sesión único de SAML).
    
    c. Pegue el valor de **Dirección URL de cierre de sesión** que ha copiado de Azure Portal en el cuadro de texto **Remote Logout URL** (Dirección URL de cierre de sesión remoto).
    
    d. Abra el certificado descargado en el Bloc de notas, copie el contenido y luego péguelo en el cuadro de texto **Certificado X.509** .
    
    e. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba brittasimon@yourcompanydomain.extension. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a UserEcho para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y luego **UserEcho**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **UserEcho**.

    ![Vínculo a UserEcho en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-userecho-test-user"></a>Creación de un usuario de prueba en UserEcho

El objetivo de esta sección es crear una usuaria de prueba llamada Britta Simon en UserEcho.

**Para crear una usuaria llamada Britta Simon en UserEcho, realice los pasos siguientes:**

1. Inicie sesión en el sitio de compañía de UserEcho como administrador.

2. En la barra de herramientas de la parte superior, haga clic en el nombre de usuario para expandir el menú y, después, haga clic en **Instalación**.
   
    ![Captura de pantalla que muestra Setup (Configuración) seleccionado en el sitio de UserEcho.](./media/userecho-tutorial/tutorial_userecho_06.png)

3. Haga clic en **Usuarios** para expandir la sección **Usuarios**.
   
    ![Captura de pantalla que muestra la opción Users (Usuarios) seleccionada en el menú Settings (Configuración).](./media/userecho-tutorial/tutorial_userecho_10.png)

4. Haga clic en **Usuarios**.
   
    ![Captura de pantalla que muestra la pestaña Users (Usuarios) seleccionada.](./media/userecho-tutorial/tutorial_userecho_11.png)

5. Haga clic en **Invitar a un nuevo usuario**.
   
    ![Captura de pantalla que muestra la opción Invite a new user (Invitar a un nuevo usuario).](./media/userecho-tutorial/tutorial_userecho_12.png)

6. En el cuadro de diálogo **Invitar a un nuevo usuario** , realice los pasos siguientes:
   
    ![Captura de pantalla que muestra el cuadro de diálogo Invite a new user (Invitar a un nuevo usuario), donde puede especificar los datos del usuario.](./media/userecho-tutorial/tutorial_userecho_13.png)

    a. En el cuadro de texto **Nombre**, escriba el nombre de un usuario, por ejemplo, Britta Simon.
    
    b.  En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.
    
    c. Haga clic en **Invitar**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de UserEcho en el Panel de acceso, debería iniciar sesión automáticamente en la versión de UserEcho para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)