---
title: 'Tutorial: Integración de Azure Active Directory con Attendance Management Services | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Attendance Management Services.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/14/2021
ms.author: jeedes
ms.openlocfilehash: 150d2583920f695735e8543d990c7f135a4ef950
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132349231"
---
# <a name="tutorial-azure-active-directory-integration-with-attendance-management-services"></a>Tutorial: Integración de Azure Active Directory con Attendance Management Services

En este tutorial va a aprender a integrar Attendance Management Services con Azure Active Directory (Azure AD). Al integrar Attendance Management Services con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Attendance Management Services.
* Permitir que los usuarios inicien sesión automáticamente en Attendance Management Services con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Attendance Management Services, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único de Attendance Management Services.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Attendance Management Services admite el SSO iniciado por **SP**.

## <a name="add-attendance-management-services-from-the-gallery"></a>Incorporación de Attendance Management Services desde la galería

Para configurar la integración de Attendance Management Services en Azure AD, es preciso agregar Attendance Management Services desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Attendance Management Services** en el cuadro de búsqueda.
1. Seleccione **Attendance Management Services** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-attendance-management-services"></a>Configuración y prueba del inicio de sesión único de Azure AD para Attendance Management Services

Configure y pruebe el SSO de Azure AD con Attendance Management Services mediante un usuario de prueba de nombre **B.Simon**. Para que el SSO funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Attendance Management Services.

Para configurar el SSO de Azure AD con Attendance Management Services, lleve a cabo los pasos siguientes:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Attendance Management Services](#configure-attendance-management-services-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Attendance Management Services](#create-attendance-management-services-test-user)** , para tener un homólogo de B.Simon en Attendance Management Services que esté vinculado a la representación del usuario de Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Attendance Management Services**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://id.obc.jp/<TENANT_INFORMATION>/`

    b. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://id.obc.jp/<TENANT_INFORMATION>/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la dirección URL de inicio de sesión reales. Póngase en contacto con el [equipo de soporte técnico de Attendance Management Services](https://www.obcnet.jp/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Attendance Management Services** (Configurar Attendance Management Services), copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección va a permitir que B.Simon use el inicio de sesión único de Azure al concederle acceso a Attendance Management Services.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Attendance Management Services**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-attendance-management-services-sso"></a>Configuración del inicio de sesión único de Attendance Management Services

1. En otra ventana del explorador, inicie sesión en su sitio de la empresa de Attendance Management Services como administrador.

1. Haga clic en **SAML authentication** (Autenticación SAML) en la sección **Administración de seguridad**.

    ![Captura de pantalla que muestra la autenticación SAML seleccionada en una página que usa caracteres no latinos.](./media/attendancemanagementservices-tutorial/security.png)

1. Lleve a cabo los siguiente pasos:

    ![Captura de pantalla que muestra una ventana donde puede realizar las tareas descritas en este paso.](./media/attendancemanagementservices-tutorial/authentication.png)

    a. Seleccione **Use SAML authentication** (Usar autenticación SAML).

    b. En el cuadro de texto **Identifier** (Identificador), pegue el valor de **Identificador de Azure AD** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Authentication endpoint URL** (Dirección URL de punto de conexión de autenticación), pegue el valor de la **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    d. Haga clic en **Seleccionar un archivo** para cargar el certificado que ha descargado de Azure AD.

    e. Seleccione **Disable password authentication** (Deshabilitar autenticación de contraseña).

    f. Haga clic en **Registro**.

### <a name="create-attendance-management-services-test-user"></a>Creación de un usuario de prueba de Attendance Management Services

Para permitir que los usuarios de Azure AD inicien sesión en Attendance Management Services, deben aprovisionarse en Attendance Management Services. En el caso de Attendance Management Services, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la empresa de Attendance Management Services como administrador.

1. Haga clic en **Administración de usuarios** en la sección **Administración de seguridad**.

    ![Captura de pantalla que muestra la opción User management (Administración de usuarios) seleccionada en una página que usa caracteres no latinos.](./media/attendancemanagementservices-tutorial/user.png)

1. Haga clic en **New rules login** (Nuevas reglas de inicio de sesión).

    ![Captura de pantalla que muestra la selección de la opción más.](./media/attendancemanagementservices-tutorial/login.png)

1. En la sección **OBCiD information** (Información de OBCiD), lleve a cabo estos pasos:

    ![Captura de pantalla que muestra una ventana donde puede realizar las tareas descritas.](./media/attendancemanagementservices-tutorial/new-user.png)

    a. En el cuadro de texto **OBCiD**, escriba el correo electrónico del usuario, por ejemplo, `BrittaSimon@contoso.com`.

    b. En el cuadro de texto **Contraseña**, escriba la contraseña del usuario.

    c. Haga clic en **Registro**.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirige a la dirección URL de inicio de sesión de Attendance Management Services, donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de Attendance Management Services e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Attendance Management Services en Aplicaciones, se le redirige a la dirección URL de inicio de sesión de Attendance Management Services. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Attendance Management Services, puede aplicar el control de sesión, que protege a la organización, en tiempo real, frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
