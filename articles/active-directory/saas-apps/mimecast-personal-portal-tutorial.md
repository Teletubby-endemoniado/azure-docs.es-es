---
title: 'Tutorial: Integración de Azure Active Directory con Mimecast Personal Portal | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Mimecast Personal Portal.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/15/2021
ms.author: jeedes
ms.openlocfilehash: f6cc85a437e455becb516859fb3bacae0086cb85
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132325968"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mimecast-personal-portal"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Mimecast Personal Portal

En este tutorial, aprenderá a integrar Mimecast Personal Portal con Azure Active Directory (Azure AD). Al integrar Mimecast Personal Portal con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a Mimecast Personal Portal.
* Permitir que los usuarios inicien sesión automáticamente en Mimecast Personal Portal con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Mimecast Personal Portal

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Mimecast Personal Portal admite el SSO iniciado por **SP**.
 
## <a name="add-mimecast-personal-portal-from-the-gallery"></a>Adición de Mimecast Personal Portal desde la galería

Para configurar la integración de Mimecast Personal Portal en Azure AD, deberá agregar esta solución desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Add from the gallery** (Agregar desde la galería), escriba **Mimecast Personal Portal** en el cuadro de búsqueda.
1. Seleccione **Mimecast Personal Portal** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-mimecast-personal-portal"></a>Configuración y prueba del inicio de sesión único de Azure AD para Mimecast Personal Portal

Configure y pruebe el inicio de sesión único de Azure AD con Mimecast Personal Portal mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una vinculación entre un usuario de Azure AD y el usuario relacionado de Mimecast Personal Portal.

Para configurar y probar el inicio de sesión único de Azure AD con Mimecast Personal Portal, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Mimecast Personal Portal](#configure-mimecast-personal-portal-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en Mimecast Personal Portal](#create-mimecast-personal-portal-test-user)** , para tener un usuario correspondiente a B.Simon en Mimecast Personal Portal que esté vinculado a la representación de usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Mimecast Personal Portal**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Basic SAML Configuration** (Configuración básica de SAML), si desea configurar la aplicación en modo iniciado por IDP, realice los pasos siguientes:

    a. En el cuadro de texto **Identificador**, escriba la dirección URL con el siguiente patrón: .

    | Region  |  Value | 
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Estados Unidos   | `https://us-api.mimecast.com/sso/<accountcode>`|
    | Sudáfrica    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Australia       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Internacional        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    > [!NOTE]
    > Encontrará el valor `accountcode` en Mimecast Personal Portal, en **Account** > **Settings** > **Account Code** (Cuenta > Configuración > Código de cuenta). Anexe el `accountcode` al identificador.

    b. En el cuadro de texto **URL de respuesta**, escriba la siguiente dirección URL: 

    | Region  |  Value |
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Estados Unidos   | `https://us-api.mimecast.com/login/saml`|
    | Sudáfrica    | `https://za-api.mimecast.com/login/saml`|
    | Australia       | `https://au-api.mimecast.com/login/saml`|
    | Internacional        | `https://jer-api.mimecast.com/login/saml`|

1. Si quiere volver a configurar la aplicación en modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: 

    | Region  |  Value |
    | --------------- | --------------- |
    | Europa          | `https://eu-api.mimecast.com/login/saml`|
    | Estados Unidos   | `https://us-api.mimecast.com/login/saml`|
    | Sudáfrica    | `https://za-api.mimecast.com/login/saml`|
    | Australia       | `https://au-api.mimecast.com/login/saml`|
    | Internacional        | `https://jer-api.mimecast.com/login/saml`|

1. Haga clic en **Save**(Guardar).

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

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

En esta sección, va a permitir que B.Simon acceda a Mimecast Personal Portal mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Mimecast Personal Portal**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-mimecast-personal-portal-sso"></a>Configuración del inicio de sesión único en Mimecast Personal Portal

1. Abra una nueva ventana del explorador web e inicie sesión en Mimecast Administration Console.

1. Vaya a **Administration** > **Services** > **Applications** (Administración > Servicios > Aplicaciones).

    ![Captura de pantalla que muestra la ventana de Mimecast con la opción Applications (Aplicaciones) seleccionada.](./media/mimecast-personal-portal-tutorial/services.png)

1. Haga clic en la pestaña **Authentication Profiles** (Perfiles de autenticación).
    
    ![Captura de pantalla que muestra la pestaña Application (Aplicación) con la opción Authentication Profiles (Perfiles de autenticación) seleccionada.](./media/mimecast-personal-portal-tutorial/authentication-profiles.png)

1. Haga clic en la pestaña **New Authentication Profile**(Nuevo perfil de autenticación).

    ![Captura de pantalla que muestra la pestaña New Authentication Profile (Nuevo perfil de autenticación) seleccionada.](./media/mimecast-personal-portal-tutorial/new-authenticatio-profile.png)

1. Proporcione una descripción válida en el cuadro de texto **Description** (Descripción) y seleccione la casilla **Enforce SAML Authentication for Mimecast Personal Portal** (Aplicar la autenticación SAML a Mimecast Personal Portal).

    ![Captura de pantalla que muestra la pestaña New Authentication Profile (Nuevo perfil de autenticación) seleccionada.](./media/mimecast-personal-portal-tutorial/selecting-personal-portal.png)

1. En la página de **configuración de SAML para Mimecast Personal Portal**, realice los pasos siguientes:

    ![Captura de pantalla que muestra dónde seleccionar la aplicación de la autenticación de SAML para la consola de administración.](./media/mimecast-personal-portal-tutorial/sso-settings.png)

    a. En **Provider** (Proveedor), seleccione **Azure Active Directory** en el menú desplegable.

    b. En el cuadro de texto **Metadata URL** (Dirección URL de metadatos), pegue el valor de **App Federation Metadata URL** (Dirección URL de metadatos de federación de aplicaciones) que copió de Azure Portal.

    c. Haga clic en **Import**. Después de importar la dirección URL de los metadatos, los campos se rellenarán automáticamente, sin necesidad de realizar ninguna acción.

    d. Asegúrese de desactivar las casillas **Use Password protected Context** (Usar contexto protegido por contraseña) y **Use Integrated Authentication Context** (Usar contexto de autenticación integrada).

    e. Haga clic en **Save**(Guardar).

### <a name="create-mimecast-personal-portal-test-user"></a>Creación de un usuario de prueba en Mimecast Personal Portal

1. Abra una nueva ventana del explorador web e inicie sesión en Mimecast Administration Console.

1. Vaya a **Administration** > **Directories** > **Internal Directories** (Administración > Directorios > Directorios Internos).

    ![Captura de pantalla que muestra la configuración de SAML de Mimecast Personal Portal, donde puede escribir los valores descritos.](./media/mimecast-personal-portal-tutorial/internal-directories.png)

1. Seleccione su dominio si se menciona más abajo. En caso contrario, cree un dominio nuevo haciendo clic en **New Domain** (Nuevo dominio).

    ![Captura de pantalla que muestra la ventana de Mimecast con la opción Internal Directories (Directorios internos) seleccionada.](./media/mimecast-personal-portal-tutorial/domain-name.png)

1. Haga clic en la pestaña **New Address** (Nueva dirección).

    ![Captura de pantalla que muestra el dominio seleccionado.](./media/mimecast-personal-portal-tutorial/new-address.png)

1. Proporcione la información de usuario necesaria en la página siguiente:

    ![Captura de pantalla que muestra la página donde puede especificar los valores descritos.](./media/mimecast-personal-portal-tutorial/user-information.png)

    a. En el cuadro de texto **Email Address** (Dirección de correo electrónico), escriba la dirección de correo electrónico del usuario como `B.Simon@yourdomainname.com`.

    b. En el cuadro de texto **Global name** (Nombre global), escriba el **nombre completo** del usuario.

    c. En los cuadros de texto **Password** (Contraseña) y **Confirm Password** (Confirmar contraseña), escriba la contraseña del usuario.

    d. Seleccione la casilla **Force Change at Login** (Forzar cambio al iniciar de sesión).

    e. Haga clic en **Save**(Guardar).

    f. Para asignar roles al usuario, haga clic en **Role Edit** (Editar rol) y asigne el rol necesario al usuario con arreglo a las necesidades de su organización.

    ![Captura de pantalla que muestra la configuración de direcciones, donde puede seleccionar Role Edit (Editar rol).](./media/mimecast-personal-portal-tutorial/assign-role.png)


## <a name="test-sso"></a>Prueba de SSO 
En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de Mimecast Personal Portal, donde puede iniciar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de Mimecast Personal Portal y comience el flujo de inicio de sesión desde ahí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de Mimecast Personal Portal para la que configuró el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Mimecast Personal Portal en Aplicaciones, si se ha configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión; y si se ha configurado en modo IDP, se debería iniciar sesión automáticamente en la instancia de Mimecast Personal Portal para la que haya configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Mimecast Personal Portal, puede aplicar el control de sesión, que protege en tiempo real a la organización frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-any-app).
