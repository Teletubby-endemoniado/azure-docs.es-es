---
title: 'Tutorial: Integración de Azure Active Directory con iQualify LMS | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e iQualify LMS.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 399cd4ca84918b8d925de8349b36cac97bbab2c4
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124809525"
---
# <a name="tutorial-azure-active-directory-integration-with-iqualify-lms"></a>Tutorial: Integración de Azure Active Directory con iQualify LMS

En este tutorial, aprenderá a integrar iQualify LMS con Azure Active Directory (Azure AD).
La integración de iQualify LMS con Azure AD ofrece las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a iQualify LMS.
* Puede permitir que los usuarios inicien sesión automáticamente en iQualify LMS (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la integración de Azure AD con iQualify LMS, necesita lo siguiente:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para inicio de sesión único en iQualify LMS

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* iQualify LMS admite SSO iniciado por **SP y IDP**
* iQualify LMS admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-iqualify-lms-from-the-gallery"></a>Incorporación de iQualify LMS desde la galería

Para configurar la integración de iQualify LMS en Azure AD, será preciso que agregue iQualify LMS desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar iQualify LMS desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **iQualify LMS**, seleccione **iQualify LMS** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![iQualify LMS en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con iQualify LMS con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de iQualify LMS.

Para configurar y probar el inicio de sesión único de Azure AD con iQualify LMS, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de iQualify LMS](#configure-iqualify-lms-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de iQualify LMS](#create-iqualify-lms-test-user)**: para tener un homólogo de Britta Simon en iQualify LMS que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con iQualify LMS, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **iQualify LMS**, seleccione **Inicio de sesión único**.

   ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

   ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en el modo iniciado por **IDP** siga estos pasos:

   ![Captura de pantalla que muestra la configuración básica de SAML, donde se puede escribir el identificador y la dirección U R L de respuesta y seleccionar Guardar.](common/idp-intiated.png)
   
   1. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente:

      * Entorno de producción: `https://<yourorg>.iqualify.com/`
      * Entorno de prueba: `https://<yourorg>.iqualify.io`

   2. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:

      * Entorno de producción: `https://<yourorg>.iqualify.com/auth/saml2/callback` 
      * Entorno de prueba: `https://<yourorg>.iqualify.io/auth/saml2/callback`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

   ![Captura de pantalla que muestra Establecer direcciones U R L adicionales donde puede escribir una U R L de inicio de sesión.](common/metadata-upload-additional-signon.png)

   En el cuadro de texto **Dirección URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:

      * Entorno de producción: `https://<yourorg>.iqualify.com/login`
      * Entorno de prueba: `https://<yourorg>.iqualify.io/login`

   > [!NOTE]
   > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de iQualify LMS](https://www.iqualify.com/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

6. La aplicación iQualify LMS espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![Captura de pantalla que muestra User Attributes (Atributos de usuario) con el icono de edición seleccionado.](common/edit-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, edite las notificaciones mediante el **icono Editar** o agregue notificaciones mediante **Agregar nueva notificación** para configurar el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | Nombre | Atributo de origen|
    | --- | --- |
    | email | user.userprincipalname |
    | first_name | user.givenname |
    | last_name | user.surname |
    | person_id | "su atributo" |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![Captura de pantalla que muestra User claims (Reclamaciones de usuario) con la opción Add new claim (Agregar nueva reclamación).](common/new-save-attribute.png)

    ![Captura de pantalla que muestra Manage user claims (Administrar reclamaciones de usuario) para escribir los valores que se describen.](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

    > [!Note]
    > El atributo **person_id** es **opcional**

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

9. En la sección **Set up iQualify LMS** (Configurar iQualify LMS), copie las direcciones URL adecuadas en función sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-iqualify-lms-single-sign-on"></a>Configuración del inicio de sesión único en iQualify LMS

1. Abra una nueva ventana del explorador e inicie sesión en el entorno de iQualify como administrador.

1. Una vez que haya iniciado sesión, haga clic en su avatar en la esquina superior derecha y, después, en **Account settings** (Configuración de la cuenta)

    ![Configuración de la cuenta](./media/iqualify-tutorial/setting1.png)

1. En el área de configuración de cuenta, haga clic en el menú de la cinta de opciones de la izquierda y luego en **INTEGRATIONS** (INTEGRACIONES)

    ![INTEGRACIONES](./media/iqualify-tutorial/setting2.png)

1. En INTEGRACIONES, haga clic en el icono de **SAML**.

    ![Icono de SAML](./media/iqualify-tutorial/setting3.png)

1. En el cuadro de diálogo **Configuración de la autenticación SAML** , realice los pasos siguientes:

    ![Configuración de la autenticación SAML](./media/iqualify-tutorial/setting4.png)

    a. En el cuadro **SAML SINGLE SIGN-ON SERVICE URL** (Dirección URL de inicio de sesión único de SAML), pegue el valor de **URL de inicio de sesión único** que copió de la ventana de configuración de la aplicación Azure AD.

    b. En el cuadro **SAML LOGOUT URL** (Dirección URL de cierre de sesión de SAML), pegue el valor de **URL de cierre de sesión** que copió de la ventana de configuración de la aplicación Azure AD.

    c. Abra el certificado descargado en el Bloc de notas, copie el contenido y péguelo en el cuadro **CERTIFICADO PÚBLICO**.

    d. En **LOGIN BUTTON LABEL** (Etiqueta del botón de inicio de sesión) escriba el nombre del botón que se mostrará en la página de inicio de sesión.

    e. Haga clic en **GUARDAR**.

    f. Haga clic en **ACTUALIZAR**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a iQualify LMS.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **iQualify LMS**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **iQualify LMS**.

    ![Vínculo a iQualify LMS en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-iqualify-lms-test-user"></a>Creación de un usuario de prueba en iQualify LMS

En esta sección, se crea un usuario llamado a Britta Simon en iQualify LMS. iQualify LMS admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en iQualify LMS, se crea después de la autenticación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de iQualify LMS del panel de acceso, debería entrar en la página de inicio de sesión de la aplicación iQualify LMS. 

   ![página de inicio de sesión](./media/iqualify-tutorial/login.png) 

Haga clic en el botón **Sign in with Azure AD** (Iniciar sesión con Azure AD), con ello debería iniciar sesión automáticamente en su aplicación iQualify LMS.

Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510). 

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)