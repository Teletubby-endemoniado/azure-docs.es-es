---
title: 'Tutorial: Integración de Azure Active Directory con Perception Estados Unidos (no UltiPro) | Documentos de Microsoft'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Perception Estados Unidos (no UltiPro).
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: 5b2e6ae3e7f5f3aa45be9aae219f14ed4ecfd228
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124738208"
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Tutorial: Integración de Azure Active Directory con Perception Estados Unidos (no UltiPro)

En este tutorial, obtendrá información sobre cómo integrar Perception Estados Unidos (no UltiPro) con Azure Active Directory (Azure AD).
La integración de Perception Estados Unidos (no UltiPro) con Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Perception Estados Unidos (no UltiPro).
* Puede permitir que los usuarios inicien sesión automáticamente en Perception Estados Unidos (no UltiPro) (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

Para configurar la integración de Azure AD con Perception Estados Unidos (no UltiPro), necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en Perception Estados Unidos (no UltiPro)

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Perception Estados Unidos (no UltiPro) admite el inicio de sesión único iniciado por **IDP**

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Agregar Perception Estados Unidos (no UltiPro) desde la galería

Para configurar la integración de Perception Estados Unidos (no UltiPro) en Azure AD, deberá agregar Perception Estados Unidos (no UltiPro) desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Perception Estados Unidos (no UltiPro) desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Perception Estados Unidos (no UltiPro)** , seleccione **Perception Estados Unidos (no UltiPro)** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Perception Estados Unidos (no UltiPro) en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Perception Estados Unidos (no UltiPro) con un usuario de prueba llamado **Britta Simon**.
Para que funcione el inicio de sesión único, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Perception Estados Unidos (no UltiPro).

Para configurar y probar el inicio de sesión único de Azure AD con Perception Estados Unidos (no UltiPro), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Perception Estados Unidos (no UltiPro)](#configure-perception-united-states-non-ultipro-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Perception Estados Unidos (no UltiPro)](#create-perception-united-states-non-ultipro-test-user)** : para tener un homólogo de Britta Simon en Perception Estados Unidos (no UltiPro) que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Perception Estados Unidos (no UltiPro), realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Perception Estados Unidos (no UltiPro)** , seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la página **Configurar inicio de sesión único con SAML** realice los siguientes pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de Perception Estados Unidos (no UltiPro)](common/idp-intiated.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL: `https://perception.kanjoya.com/sp`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://perception.kanjoya.com/sso?idp=<entity_id>`

    c. La aplicación **Perception United States (no UltiPro)** requiere que el valor de **Identificador de Azure AD** como <entity_id>, que puede obtener de la sección **Configuración de Perception United States (Non-UltiPro)** , esté codificado en URI. Para obtener el valor codificado como URI, use el siguiente vínculo: **http://www.url-encode-decode.com/** .

    d. Después de obtener el valor codificado como URI, combínelo con la **Dirección URL de respuesta** según se indica a continuación:

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    e. Pegue el valor anterior en el cuadro de texto **Dirección URL de respuesta**.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Set up Perception Estados Unidos (no UltiPro)** (Configurar Perception Estados Unidos (no UltiPro)), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión   

### <a name="configure-perception-united-states-non-ultipro-single-sign-on"></a>Configuración del inicio de sesión único en Perception Estados Unidos (no UltiPro)

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de Perception Estados Unidos (no UltiPro) como administrador.

2. En la barra de herramientas principal, haga clic en **Account Settings** (Configuración de la cuenta).

    ![Captura de pantalla que muestra la opción Account settings (Configuración de la cuenta) seleccionada en la barra de herramientas principal.](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

3. En la página **Account Settings** (Configuración de la cuenta), realice los pasos siguientes:

    ![Usuario de Perception Estados Unidos (no UltiPro)](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. En el cuadro de texto **Company Name** (Nombre de la empresa), escriba el nombre de la **empresa**.
    
    b. En el cuadro de texto **Account Name** (Nombre de la cuenta), escriba el nombre de la **cuenta**.

    c. En el cuadro de texto **Default Reply-To Email** (Correo electrónico de respuesta predeterminado), escriba un **correo electrónico** válido.

    d. Seleccione **SSO Identity Provider** (Proveedor de identidades de SSO) como **SAML 2.0**.

4. En la página **SSO Configuration** (Configuración de SSO), realice los siguientes pasos:

    ![Configuración de SSO de Perception Estados Unidos (no UltiPro)](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Seleccione **SAML NameID Type** (Tipo de identificador de nombre de SAML) como **EMAIL** (correo electrónico).

    b. En el cuadro de texto **SSO Configuration Name** (nombre de configuración de SSO), escriba el nombre de su **configuración**.
    
    c. En el cuadro de texto **Nombre del proveedor de identidad**, pegue el valor de **Identificador de Azure AD** que ha copiado de Azure Portal. 

    d. En el cuadro de texto **SAML Domain** (dominio de SAML), escriba el dominio, por ejemplo @contoso.com.

    e. Haga clic en **Upload Again** (cargar de nuevo) para cargar el archivo **XML de metadatos**.

    f. Haga clic en **Update**(Actualizar).

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

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Perception Estados Unidos (no UltiPro).

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y luego **Perception Estados Unidos (no UltiPro)** .

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Perception Estados Unidos (no UltiPro)** .

    ![Vínculo a Perception Estados Unidos (no UltiPro) en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-perception-united-states-non-ultipro-test-user"></a>Creación de un usuario de prueba de Perception Estados Unidos (no UltiPro)

En esta sección, creará un usuario llamado Britta Simon en Perception Estados Unidos (no UltiPro). Trabaje con el [equipo de soporte técnico de Perception Estados Unidos (no UltiPro)](https://www.ultimatesoftware.com/Contact/ContactUs) para agregar los usuarios en la plataforma de Perception Estados Unidos (no UltiPro).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Perception Estados Unidos (no UltiPro) en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Perception Estados Unidos (no UltiPro) para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)