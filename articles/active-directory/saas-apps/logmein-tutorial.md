---
title: 'Tutorial: Integración de Azure Active Directory con LogMeIn | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y LogMeIn.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/01/2021
ms.author: jeedes
ms.openlocfilehash: ca6d094c14968f2208da95c4631cb06232db2806
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132310886"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-logmein"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con LogMeIn

En este tutorial, aprenderá a integrar LogMeIn con Azure Active Directory (Azure AD). Al integrar LogMeIn con Azure AD, podrá:

* Controlar en Azure AD quién tiene acceso a LogMeIn.
* Permitir que los usuarios inicien sesión automáticamente en LogMeIn con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en LogMeIn.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* LogMeIn admite el inicio de sesión único iniciado por **SP e IDP**.
* LogMeIn admite el [aprovisionamiento automatizado de usuarios](logmein-provisioning-tutorial.md).

## <a name="adding-logmein-from-the-gallery"></a>Incorporación de LogMeIn desde la galería

Para configurar la integración de LogMeIn en Azure AD, deberá agregar LogMeIn desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **LogMeIn** en el cuadro de búsqueda.
1. Seleccione **LogMeIn** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-logmein"></a>Configuración y prueba del inicio de sesión único de Azure AD para LogMeIn

Configure y pruebe el inicio de sesión único de Azure AD con LogMeIn mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de LogMeIn.

Para configurar y probar el inicio de sesión único de Azure AD con LogMeIn, lleve a cabo los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en LogMeIn](#configure-logmein-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba de LogMeIn](#create-logmein-test-user)** : para tener un homólogo de B.Simon en LogMeIn que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **LogMeIn**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    a. En el cuadro de texto **Dirección URL de inicio de sesión**, escriba la dirección URL: `https://authentication.logmeininc.com/login?service=https%3A%2F%2Fmyaccount.logmeininc.com`

1. La aplicación LogMeIn espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. En la siguiente captura de pantalla se muestra la lista de atributos predeterminados, donde a **Unique User Identifier** (Identificador de usuario único) se le ha asignado **user.userprincipalname**. La aplicación LogMeIn espera que a **Unique User Identifier** (Identificador de usuario único) se le haya asignado **user.mail**, por lo que se debe editar la asignación de atributos. Para ello, haga clic en el icono **Edit** (Editar) y modifíquela.

    ![imagen](common/default-attributes.png)

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

6. En la sección **Configurar LogMeIn**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección va a permitir que B.Simon acceda a LogMeIn mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **LogMeIn**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-logmein-sso"></a>Configuración del inicio de sesión único de LogMeIn

1. Para automatizar la configuración en LogMeIn, debe instalar la **extensión del navegador de inicio de sesión seguro de Aplicaciones**. Para ello, haga clic en **Instalar la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

1. Después de agregar la extensión al explorador, haga clic en **Configurar LogMeIn** para ir a dicha aplicación. Ahí, escriba las credenciales de administrador para iniciar sesión en LogMeIn. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 5.

    ![Configuración](common/setup-sso.png)

1. Si quiere configurar LogMeIn manualmente, en otra ventana del explorador web, inicie sesión en su sitio empresarial de LogMeIn como administrador.

1. Vaya a la pestaña **Identity Provider** (Proveedor de identidades) y, en el cuadro de texto **Metadata url** (Dirección URL de metadatos), pegue la **dirección URL de metadatos de federación** que copió desde Azure Portal.

    ![Captura de pantalla de la dirección URL de metadatos de federación. ](./media/logmein-tutorial/configuration.png)

1. Haga clic en **Save**(Guardar).

### <a name="create-logmein-test-user"></a>Creación de un usuario de prueba de LogMeIn

1. En otra ventana del explorador, inicie sesión en el sitio web de LogMeIn como administrador.

1. Vaya a la pestaña **Users** (Usuarios) y haga clic en **Add a user** (Agregar usuario).

    ![Captura de pantalla del botón para agregar un usuario.](./media/logmein-tutorial/add-user.png)

1. Rellene los campos obligatorios en la página siguiente y haga clic en **Save** (Guardar).

    ![Captura de pantalla de los campos de usuario.](./media/logmein-tutorial/create-user.png)

> [!NOTE]
> LogMeIn también admite el aprovisionamiento automático de usuarios. [Aquí](./logmein-provisioning-tutorial.md) puede encontrar más detalles sobre cómo configurar el aprovisionamiento automático de usuarios.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de LogMeIn, donde puede iniciar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de LogMeIn e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de LogMeIn para la que configuró el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de LogMeIn en Aplicaciones, si se ha configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión; y si se ha configurado en modo IDP, se debería iniciar sesión automáticamente en la instancia de LogMeIn para la que haya configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado LogMeIn, puede aplicar controles de sesión, que protegen contra la filtración y la infiltración de la información confidencial de la organización en tiempo real. Los controles de sesión proceden del acceso condicional. [Aprenda a aplicar el control de sesión con las aplicaciones de Microsoft Defender para la nube](/cloud-app-security/proxy-deployment-aad).
