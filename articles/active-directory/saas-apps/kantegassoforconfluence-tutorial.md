---
title: 'Tutorial: Integración de Azure Active Directory con Kantega SSO para Confluence | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Kantega SSO para Confluence.
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
ms.openlocfilehash: 789579404f7588f38f2e2ce977fc9a895ede68fa
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124822448"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a>Tutorial: Integración de Azure Active Directory con Kantega SSO para Confluence

En este tutorial obtendrá información sobre cómo integrar Kantega SSO para Confluence con Azure Active Directory (Azure AD).
La integración de Kantega SSO para Confluence con Azure AD le proporciona las siguientes ventajas:

* En Azure AD puede controlar quién tiene acceso a Kantega SSO para Confluence.
* Puede permitir que los usuarios inicien sesión automáticamente en Kantega SSO para Confluence (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para configurar la integración de Azure AD con Kantega SSO para Confluence, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Suscripción de Kantega SSO para Confluence con el inicio de sesión único habilitado

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Kantega SSO para Confluence admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="adding-kantega-sso-for-confluence-from-the-gallery"></a>Incorporación de Kantega SSO para Confluence desde la galería

Para configurar la integración de Kantega SSO para Confluence en Azure AD, tiene que agregar Kantega SSO para Confluence desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Kantega SSO para Confluence desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Kantega SSO para Confluence**, seleccione **Kantega SSO para Confluence** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Kantega SSO para Confluence en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Kantega SSO para Confluence con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Kantega SSO para Confluence.

Para configurar y probar el inicio de sesión único de Azure AD con Kantega SSO para Confluence, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Kantega SSO para Confluence](#configure-kantega-sso-for-confluence-single-sign-on)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Kantega SSO para Confluence](#create-kantega-sso-for-confluence-test-user)** : para tener un homólogo de Britta Simon en Kantega SSO para Confluence que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Kantega SSO para Confluence, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Kantega SSO para Confluence**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    ![Captura de pantalla que muestra la sección "Configuración básica de S A M L" con los campos "Identificador" y "Dirección U R L de respuesta" resaltados y el botón "Guardar" seleccionado.](common/idp-intiated.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Kantega SSO para Confluence](common/metadata-upload-additional-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Estos valores se reciben durante la configuración del complemento de Confluence, que se explica más adelante en el tutorial.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

7. En la sección **Set up Kantega SSO for Confluence** (Configurar Kantega SSO para Confluence), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-kantega-sso-for-confluence-single-sign-on"></a>Configuración del inicio de sesión único en Kantega SSO para Confluence

1. En otra ventana del explorador web, inicie sesión en el **Portal de administración de Confluence** como administrador.

1. Mantenga el mouse encima del icono de engranaje y haga clic en **Complementos**.

    ![Captura de pantalla que muestra el menú del icono de engranaje y la opción "Add-ons" (Complementos) seleccionada.](./media/kantegassoforconfluence-tutorial/addon1.png)

1. En la pestaña **ATLASSIAN MARKETPLACE** (MARKETPLACE DE ATLASSIAN), haga clic en **Find new add-ons** (Buscar nuevos complementos).

    ![Captura de pantalla que muestra la pestaña "ATTLASSIAN MARKETPLACE" (MARKETPLACE DE ATLASSIAN) con la opción "Find new add-ons" (Buscar nuevos complementos) seleccionada.](./media/kantegassoforconfluence-tutorial/addon.png)

1. Busque **Kantega SSO para Confluence (SAML Kerberos)** y haga clic en el botón **Instalar** para instalar el nuevo complemento SAML.

    ![Captura de pantalla que muestra la página "Find new add-ons" (Buscar nuevos complementos) con el texto "Kantega S S O for Confluence S A M L Kerberos" en el cuadro de búsqueda y el botón "Install" (Instalar) seleccionado.](./media/kantegassoforconfluence-tutorial/addon2.png)

1. Se inicia la instalación del complemento.

    ![Captura de pantalla que muestra la pantalla "Installing" (Instalando) del complemento.](./media/kantegassoforconfluence-tutorial/addon3.png)

1. Una vez completada la instalación. Haga clic en **Cerrar**.

    ![Captura de pantalla que muestra la pantalla "Installed and ready to go" (Instalado y listo para usarse) con la acción "Close" (Cerrar) seleccionada.](./media/kantegassoforconfluence-tutorial/addon33.png)

1. Haga clic en **Administrar**.

    ![Captura de pantalla que muestra el complemento "Kantega Single Sign-on with Kerberos and S A M L" (Inicio de sesión único de Kantega con Kerberos y S A M L) con el botón "Manage" (Administrar) seleccionado.](./media/kantegassoforconfluence-tutorial/addon34.png)

1. Haga clic en **Configurar** para configurar el nuevo complemento.

    ![Captura de pantalla que muestra la página "Kantega Single Sign-on with Kerberos and S A M L" (Inicio de sesión único de Kantega con Kerberos y S A M L) con el botón "Configure" (Configurar) seleccionado.](./media/kantegassoforconfluence-tutorial/addon35.png)

1. Este nuevo complemento también puede encontrarse en la pestaña **USERS & SECURITY** (USUARIOS Y SEGURIDAD).

    ![Captura de pantalla que muestra la pestaña "USERS & SECURITY" (Usuarios y seguridad) con la acción "Kantega Single Sign-on" (Inicio de sesión único de Kantega) seleccionada.](./media/kantegassoforconfluence-tutorial/addon36.png)

1. En la sección **SAML**. Seleccione **Azure Active Directory (Azure AD)** en la lista desplegable **Agregar proveedor de identidades**.

    ![Captura de pantalla que muestra la sección "S A M L" con las opciones "Add Identity provider" (Agregar proveedor de identidades) y "Azure Active Directory (Azure AD)" seleccionadas.](./media/kantegassoforconfluence-tutorial/addon4.png)

1. Seleccione el nivel de suscripción **Básica**.

    ![Captura de pantalla que muestra la página "Preparing Azure AD" (Preparación de Azure A D) con la opción "Basic" (Básica) seleccionada.](./media/kantegassoforconfluence-tutorial/addon5.png)

1. En la sección **Agregar propiedades**, siga estos pasos:

    ![Captura de pantalla que muestra la sección "App properties" (Propiedades de la aplicación) con el campo "App I D U R L" (Dirección U R L del identificador de aplicación) y el botón "Copy" (Copiar) resaltados, y el botón "Next" (Siguiente) seleccionado.](./media/kantegassoforconfluence-tutorial/addon6.png)

    a. Copie el valor de **URI de id. de aplicación** y úselo en los campos **Identificador, URL de respuesta y URL de inicio de sesión** de la sección **Configuración básica de SAML**  de Azure Portal.

    b. Haga clic en **Next**.

1. En la sección **Importar metadatos**, siga estos pasos: 

    ![Captura de pantalla que muestra la sección "Importación de metadatos" con la opción "Archivo de metadatos en mi equipo" seleccionada.](./media/kantegassoforconfluence-tutorial/addon7.png)

    a. Seleccione **Archivo de metadatos en el equipo** y cargue el archivo de metadatos que descargó desde Azure Portal.

    b. Haga clic en **Next**.

1. En la sección **Name and SSO location** (Nombre y ubicación de SSO), siga estos pasos:

    ![Captura de pantalla que muestra la opción "Name and S S O location" (Nombre y ubicación de S S O) y el botón "Siguiente" seleccionado.](./media/kantegassoforconfluence-tutorial/addon8.png)

    a. Agregue el nombre del proveedor de identidades en el cuadro de texto **Nombre del proveedor de identidades** (por ejemplo, Azure AD).

    b. Haga clic en **Next**.

1. Compruebe el certificado de firma y haga clic en **Siguiente**.

    ![Captura de pantalla que muestra la sección "Signature verification" (Comprobación de firmas) con el botón "Next" (Siguiente) seleccionado.](./media/kantegassoforconfluence-tutorial/addon9.png)

1. En la sección **Cuentas de usuario de Confluence**, siga estos pasos:

    ![Captura de pantalla que muestra la sección "Confluence user accounts" (Cuentas de usuario de Confluence) con la opción "Create users in Confluence's Internal Directory if needed" (Crear usuarios en el directorio interno de Confluence si es necesario) y el botón "Next" (Siguiente) seleccionados.](./media/kantegassoforconfluence-tutorial/addon10.png)

    a. Seleccione **Create users in Confluence's internal Directory if needed** (Crear usuarios en el directorio interno de Confluence si es necesario) y escriba el nombre adecuado del grupo de usuarios (puede ser un número múltiple de grupos separados por coma).

    b. Haga clic en **Next**.

1. Haga clic en **Finalizar**

    ![Captura de pantalla de la página "Summary" (Resumen) con el botón "Finish" (Finalizar) seleccionado.](./media/kantegassoforconfluence-tutorial/addon11.png)

1. En la sección **Known domains for Azure AD** (Dominios conocidos para Azure AD), siga estos pasos: 

    ![Captura de pantalla que muestra la página "Known domains for Azure AD" (Dominios conocidos para Azure AD) con el cuadro de texto "Known domains" (Dominios conocidos) resaltado y el botón "Save" (Guardar) seleccionado.](./media/kantegassoforconfluence-tutorial/addon12.png)

    a. Seleccione **Known domains** (Dominios conocidos) en el panel izquierdo de la página.

    b. Escriba el nombre de dominio en el cuadro de texto **Known domains** (Dominios conocidos).

    c. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Kantega SSO para Confluence.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Kantega SSO para Confluence**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Kantega SSO para Confluence**.

    ![Vínculo a Kantega SSO para Confluence en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-kantega-sso-for-confluence-test-user"></a>Creación de un usuario de prueba de Kantega SSO para Confluence

Para permitir que los usuarios de Azure AD inicien sesión en Confluence, tienen que aprovisionarse en Confluence. En el caso de Kantega SSO para Confluence, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la empresa de Kantega SSO para Confluence como administrador.

1. Mantenga el mouse encima del icono de engranaje y haga clic en **Administración de usuarios**.

    ![Captura de pantalla que muestra el icono de engranaje y la opción "User management" (Administración de usuarios) seleccionada.](./media/kantegassoforconfluence-tutorial/user1.png)

1. En la sección Usuarios, haga clic en la pestaña **Agregar usuarios**. En la página del cuadro de diálogo **Agregar un usuario**, realice los siguientes pasos:

    ![Agregar empleado](./media/kantegassoforconfluence-tutorial/user2.png)

    a. En el cuadro de texto **Nombre de usuario**, escriba el correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.

    b. En el cuadro de texto **Nombre completo**, escriba el nombre completo de un usuario, por ejemplo, Britta Simon.

    c. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.

    d. En el cuadro de texto **Contraseña**, escriba la contraseña del usuario.

    e. Haga clic en **Confirmar contraseña** y vuelva a escribir la contraseña.

    f. Haga clic en el botón **Agregar**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Kantega SSO para Confluence en el panel de acceso, debería iniciar sesión automáticamente en la versión de Kantega SSO para Confluence para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)