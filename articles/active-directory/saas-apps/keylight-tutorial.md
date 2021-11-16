---
title: 'Tutorial: Integración de Azure Active Directory con LockPath Keylight | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y LockPath Keylight.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/11/2021
ms.author: jeedes
ms.openlocfilehash: abaa441410f5cbccad3374fc75a074a7678e0d8f
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132294792"
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Tutorial: Integración de Azure Active Directory con LockPath Keylight

En este tutorial, obtendrá información sobre cómo integrar LockPath Keylight con Azure Active Directory (Azure AD). Al integrar LockPath Keylight con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a LockPath Keylight.
* Permitir que los usuarios inicien sesión automáticamente en LockPath Keylight con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con LockPath Keylight, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en LockPath Keylight.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* LockPath Keylight admite el inicio de sesión único iniciado por **SP**.
* LockPath Keylight admite aprovisionamiento de usuarios **Just-In-Time**.

## <a name="add-lockpath-keylight-from-the-gallery"></a>Incorporación de LockPath Keylight desde la galería

Para configurar la integración de LockPath Keylight en Azure AD, es preciso agregar LockPath Keylight desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **LockPath Keylight** en el cuadro de búsqueda.
1. Seleccione **LockPath Keylight** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-lockpath-keylight"></a>Configuración y prueba del inicio de sesión único de Azure AD para LockPath Keylight

Configure y pruebe el inicio de sesión único de Azure AD con LockPath Keylight mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de LockPath Keylight.

Para configurar y probar el inicio de sesión único de Azure AD con LockPath Keylight, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de LockPath Keylight](#configure-lockpath-keylight-sso)** : para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de LockPath Keylight](#create-lockpath-keylight-test-user)** : para tener un homólogo de B.Simon en LockPath Keylight que esté vinculado a la representación de ella en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **LockPath Keylight**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<COMPANY_NAME>.keylightgrc.com`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<COMPANY_NAME>.keylightgrc.com/Login.aspx`.

    c. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<COMPANY_NAME>.keylightgrc.com/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y las direcciones URL de inicio de sesión y de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de cliente de LockPath Keylight](https://www.lockpath.com/contact/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (sin procesar)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificateraw.png)

6. En la sección **Set up LockPath Keylight** (Configurar LockPath Keylight), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a B.Simon a LockPath Keylight para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **LockPath Keylight**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-lockpath-keylight-sso"></a>Configuración del inicio de sesión único de LockPath Keylight

1. Realice los pasos siguientes para habilitar el inicio de sesión único en LockPath Keylight:

    a. Inicie sesión en su cuenta de LockPath Keylight como administrador.

    b. En el menú de la parte superior, haga clic en el icono de la **persona** y seleccione **Keylight Setup** (Configuración de Keylight).

    ![Captura de pantalla que muestra el icono "Person" (Persona) seleccionado y la opción "Keylight Setup" (Configuración de Keylight) seleccionada en el menú desplegable.](./media/keylight-tutorial/setup-icon.png)

    c. En la vista de árbol de la izquierda, haga clic en **SAML**.

    ![Captura de pantalla que muestra la opción "S A M L" seleccionada en la vista de árbol.](./media/keylight-tutorial/treeview.png)

    d. En el cuadro de diálogo **SAML Settings** (Configuración de SAML), haga clic en **Edit** (Editar).

    ![Captura de pantalla que muestra la ventana "S A M L Settings" (Configuración de S A M L) con el botón "Edit" (Editar) seleccionado.](./media/keylight-tutorial/edit-icon.png)

1. En la página del cuadro de diálogo **Edit SAML Settings** (Editar la configuración de SAML), realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/keylight-tutorial/settings.png)

    a. Establezca **SAML authentication** (Autenticación SAML) como **Active** (Activada).

    b. En el cuadro de texto **Identity Provider Login URL** (Dirección URL de inicio de sesión único del proveedor de identidades), pegue el valor de **Dirección URL de inicio de sesión**  que ha copiado de Azure Portal.

    c. En el cuadro de texto **Identity Provider Logout URL** (Dirección URL de cierre de sesión único del proveedor de identidades), pegue el valor de **Dirección URL de cierre de sesión**  que ha copiado de Azure Portal.

    d. Haga clic en **Elegir archivo** para seleccionar el certificado de LockPath Keylight descargado y después haga clic en **Abrir** para cargarlo.

    e. Establezca **SAML User Id location** (Ubicación de id. de usuario de SAML) en **NameIdentifier element of the subject statement** (Elemento NameIdentifier de la instrucción Subject).

    f. Proporcione el **proveedor de servicios de Keylight** mediante el siguiente patrón: `https://<CompanyName>.keylightgrc.com`.

    g. Establezca **Auto-provision users** (Usuarios de aprovisionamiento automático) en **Active** (Activado).

    h. Establezca **Auto-provision account type** (Tipo de cuenta de aprovisionamiento automático) en **Full User** (Usuario completo).

    i. Establezca **Auto-provision security role** (Rol de seguridad de aprovisionamiento automático) y seleccione **Standard User with SAML** (Usuario estándar con SAML).

    j. Establezca **Auto-provision security config** (Configuración de seguridad de aprovisionamiento automático) y seleccione **Standard User Configuration** (Configuración de usuario estándar).

    k. En el cuadro **Atributo de correo electrónico**, escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    l. En el cuadro de texto **Atributo de nombre**, escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    m. En el cuadro de texto **Atributo de apellido**, escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

    n. Haga clic en **Save**(Guardar).

### <a name="create-lockpath-keylight-test-user"></a>Creación de un usuario de prueba de LockPath Keylight

En esta sección, creará un usuario llamado Britta Simon en LockPath Keylight. LockPath Keylight admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe aún en LockPath Keylight, se crea después de la autenticación. Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el [equipo de soporte técnico de cliente de LockPath Keylight](https://www.lockpath.com/contact/).

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de LockPath Keylight, donde podrá comenzar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de LockPath Keylight e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de LockPath Keylight en Aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de dicha aplicación. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado LockPath Keylight, puede aplicar el control de sesión, que protege su organización, en tiempo real, frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
