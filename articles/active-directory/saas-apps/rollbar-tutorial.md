---
title: 'Tutorial: Integración de Azure Active Directory con Rollbar | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Rollbar.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/31/2021
ms.author: jeedes
ms.openlocfilehash: 1761979d554bc3318ef8f2e06fe872a6e674d0c4
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132341680"
---
# <a name="tutorial-azure-active-directory-integration-with-rollbar"></a>Tutorial: integración de Azure Active Directory con Rollbar

En este tutorial, aprenderá a integrar Rollbar con Azure Active Directory (Azure AD). Al integrar Rollbar con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Rollbar.
* Permitir que los usuarios inicien sesión automáticamente en Rollbar con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Rollbar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en Rollbar.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Rollbar admite el inicio de sesión único iniciado por **SP e IDP**.
* Rollbar admite el [aprovisionamiento de usuarios automático](rollbar-provisioning-tutorial.md).

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-rollbar-from-the-gallery"></a>Incorporación de Rollbar desde la galería

Para configurar la integración de Rollbar en Azure AD, deberá agregar Rollbar desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Rollbar** en el cuadro de búsqueda.
1. Seleccione **Rollbar** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-rollbar"></a>Configuración y prueba del SSO de Azure AD para Rollbar

Configure y pruebe el inicio de sesión único de Azure AD con Rollbar mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Rollbar.

Para configurar y probar el inicio de sesión único de Azure AD con Rollbar, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Rollbar](#configure-rollbar-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en Rollbar](#create-rollbar-test-user)** : para tener un homólogo de B.Simon en Rollbar que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Rollbar**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    a. En el cuadro de texto **Identificador**, escriba la dirección URL: `https://saml.rollbar.com`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://rollbar.com/<ACCOUNT_NAME>/saml/sso/azure/`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://rollbar.com/<ACCOUNT_NAME>/saml/login/azure/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Rollbar](mailto:support@rollbar.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

7. En la sección **Configurar Rollbar**, copie la dirección o direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon acceda a Rollbar mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Rollbar**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-rollbar-sso"></a>Configuración del inicio de sesión único en Rollbar

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de su compañía de Rollbar.

1. Haga clic en **Configuración de perfil** en la esquina superior derecha y, luego, en **Configuración de nombre de cuenta**.

    ![Captura de pantalla muestra la configuración de nombre de cuenta seleccionada en Configuración de perfil.](./media/rollbar-tutorial/general.png)

1. Haga clic en **Proveedor de identidades** en SEGURIDAD.

    ![Captura de pantalla que muestra el proveedor de identidades seleccionado en SEGURIDAD.](./media/rollbar-tutorial/security.png)

1. En la sección **Proveedores de identidades SAML**, realice los pasos siguientes:

    ![Captura de pantalla que muestra el proveedor de identidades SAML, donde puede especificar los valores descritos.](./media/rollbar-tutorial/configure.png)

    a. Seleccione **AZURE** en el menú desplegable **Proveedor de identidades de SAML**.

    b. Abra el archivo de metadatos en el Bloc de notas, copie el contenido del mismo en el Portapapeles y, luego, péguelo en el cuadro de texto **Metadatos de SAML**.

    c. Haga clic en **Save**(Guardar).

1. Después de hacer clic en el botón Guardar, la pantalla se verá de la siguiente manera:

    ![Captura de pantalla que muestra los resultados en la página Proveedor de identidades SAML.](./media/rollbar-tutorial/identity-provider.png)

    > [!NOTE]
    > Para completar el paso siguiente, primero debe haberse agregado como usuario de la aplicación Rollbar en Azure.
    
    a. Si necesita que los usuarios se autentiquen a través de Azure, haga clic en **Log in via your identity provider** (Iniciar sesión a través del proveedor de identidades propio) para volver a autenticar a través de Azure.  

    b.  Cuando se le haya redirigido a la pantalla, seleccione la casilla **Require login via SAML Identity Provider** (Requerir inicio de sesión a través del proveedor de identidades SAML).

    b. Haga clic en **Save**(Guardar).

### <a name="create-rollbar-test-user"></a>Creación de un usuario de prueba de Rollbar

Para habilitar a los usuarios de Azure AD para que inicien sesión en Rollbar, tienen que aprovisionarse en dicha solución. En el caso de Rollbar, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión como administrador en el sitio de su compañía de Rollbar.

1. Haga clic en **Configuración de perfil** en la esquina superior derecha y, luego, en **Configuración de nombre de cuenta**.

    ![Usuario](./media/rollbar-tutorial/general.png)

1. Haga clic en **Usuarios**.

    ![Agregar empleado](./media/rollbar-tutorial/user.png)

1. Haga clic en **Invite Team Members** (Invitar a miembros del equipo).

    ![Captura de pantalla que muestra la opción para invitar a miembros del equipo seleccionada.](./media/rollbar-tutorial/invite-user.png)

1. En el cuadro de texto, escriba el nombre de usuario como **brittasimon\@contoso.com** y haga clic en **Add/Invite** (Agregar o invitar).

    ![Captura de pantalla que muestra la opción para agregar o invitar a los miembros con una dirección proporcionada.](./media/rollbar-tutorial/add-user.png)

1. El usuario recibe una invitación y, después de aceptarla, se crea en el sistema.

> [!NOTE]
> Rollbar también admite el aprovisionamiento automático de usuarios. [Aquí](./rollbar-provisioning-tutorial.md) puede encontrar más detalles sobre cómo configurar el aprovisionamiento automático de usuarios.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de Rollbar, donde puede iniciar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de Rollbar e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de Rollbar para la que ha configurado el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Rollbar en Mis aplicaciones, si ha realizado la configuración en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión. Si ha realizado la configuración en modo IDP, debería iniciar sesión automáticamente en la instancia de Rollbar para la que ha configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Rollbar, podrá aplicar el control de sesión, que protege a la organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
