---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con Watch by Colors'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Watch by Colors.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/19/2021
ms.author: jeedes
ms.openlocfilehash: 3a48fd180e0f40039f36e0f76471fdba6e101e52
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132303537"
---
# <a name="tutorial-azure-ad-sso-integration-with-watch-by-colors"></a>Tutorial: Integración del inicio de sesión único de Azure AD con Watch by Colors

En este tutorial, obtendrá información sobre cómo integrar Watch by Colors con Azure Active Directory (Azure AD). Al integrar Watch by Colors con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Watch by Colors.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Watch by Colors con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Watch by Colors.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Watch by Colors admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="add-watch-by-colors-from-the-gallery"></a>Incorporación de Watch by Colors desde la galería

Para configurar la integración de Watch by Colors en Azure AD, será preciso que agregue Watch by Colors desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Watch by Colors** en el cuadro de búsqueda.
1. Seleccione **Watch by Colors** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-watch-by-colors"></a>Configuración y prueba del inicio de sesión único de Azure AD para Watch by Colors

Configure y pruebe el inicio de sesión único de Azure AD con Watch by Colors mediante un usuario de prueba llamado **B. Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Watch by Colors.

Para configurar y probar el inicio de sesión único de Azure AD con Watch by Colors, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Watch by Colors](#configure-watch-by-colors-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación del usuario de prueba de Watch by Colors](#create-watch-by-colors-test-user)** , para tener un homólogo de B. Simon en Watch by Colors vinculado a su representación en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **Watch by Colors**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, la aplicación está preconfigurada en el modo iniciado por **IDP** y las direcciones URL necesarias ya se han rellenado previamente con Azure. El usuario debe guardar la configuración, para lo que debe hacer clic en el botón **Guardar**.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://app.colorscorporation.com/login`

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

En esta sección, va a permitir que B. Simon acceda a Watch by Colors mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Watch by Colors**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-watch-by-colors-sso"></a>Configuración del inicio de sesión único en Watch by Colors

1. Para automatizar la configuración en Watch by Colors, es preciso instalar la **extensión de inicio de sesión seguro de Mis aplicaciones** , para lo que es preciso hacer clic en **Instale la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Configurar Watch by Colors** para ir a la aplicación Watch by Colors. En ella, escriba las credenciales de administrador para iniciar sesión en Watch by Colors. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 5.

    ![Configuración](common/setup-sso.png)

3. Si desea configurar Watch by Colors de forma manual, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de Watch by Colors como administrador y haga lo siguiente:

4. En la esquina superior derecha de la página, haga clic en su **perfil** > **Account Settings** (Configuración de cuenta)  > **SSO (Single Sign On)** [SSO (Inicio de sesión único)].

    ![Captura de pantalla que muestra la página Account Settings (Configuración de cuenta) en la que la opción S S O está deshabilitada.](./media/watch-by-colors-tutorial/account.png)

5. Siga estos pasos en la página **SSO (Single Sign On)** [SSO (Inicio de sesión único)]:

    ![Captura de pantalla que muestra la pestaña SAML Setup (Configuración de SAML) donde se puede habilitar SAML.](./media/watch-by-colors-tutorial/profile.png)

    a. Alterne la configuración de la opción **Habilitar SAML** en **Activada**.

    b. En el cuadro de texto **Dirección URL**, pegue la **Dirección URL de metadatos de federación** que ha copiado de Azure Portal.

    c. Haga clic en **Importar** y, a continuación, los siguientes campos se rellenan automáticamente de forma automática en la página.

    d. Haga clic en **Save**(Guardar).

### <a name="create-watch-by-colors-test-user"></a>Creación de un usuario de prueba de Watch by Colors

Para permitir que los usuarios de Azure AD inicien sesión en Watch by Colors, tienen que aprovisionarse en Watch by Colors. En Watch by Colors, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en Watch by Colors como administrador de seguridad.

1. En la esquina superior derecha de la página, haga clic en su **perfil** > **Usuarios** > **Agregar usuario**.

    ![Captura de pantalla que muestra la página Users (Usuarios).](./media/watch-by-colors-tutorial/user.png)

1. En la página **User Details** (Detalles del usuario), siga estos pasos:

    ![Captura de pantalla que muestra User Details (Detalles del usuario) donde puede especificar los valores descritos.](./media/watch-by-colors-tutorial/details.png)

    a. En el cuadro de texto **First Name** (Nombre), escriba el nombre del usuario, en este caso **B**.

    b. En el cuadro de texto **Last Name** (Apellidos), escriba el nombre de usuario, en este caso **Simon**.

    c. En el cuadro de texto **E-mail** (Correo electrónico), escriba el correo electrónico del usuario, por ejemplo, `B.Simon@contoso.com`.

    d. En el cuadro de texto **Password** (Contraseña), escriba su contraseña.

    e. Seleccione **Account Permissions** (Permisos de cuenta) en función de su organización.

    f. Haga clic en **Save**(Guardar).

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirige a la dirección URL de inicio de sesión de Watch by Colors, donde puede iniciar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de Watch by Colors e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; se debería iniciar sesión automáticamente en la instancia de Watch by Colors en la que haya configurado el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Watch by Colors en Aplicaciones, si ha configurado en modo SP, se le redirige a la página de inicio de sesión de la aplicación para iniciar el flujo de inicio de sesión. Si ha configurado en modo IDP, se debería iniciar sesión automáticamente en la instancia de Watch by Colors en la que haya configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Cuando haya configurado Watch by Colors, puede aplicar el control de sesión, que protege a la organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con las aplicaciones de Microsoft Defender para la nube](/cloud-app-security/proxy-deployment-aad).
