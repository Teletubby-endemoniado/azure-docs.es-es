---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con una organización de GitHub Enterprise Cloud | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y una organización de GitHub Enterprise Cloud.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/08/2021
ms.author: jeedes
ms.openlocfilehash: e6a56077adc6456d5d1ea6d5180a21560e193bb8
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132285792"
---
# <a name="tutorial-azure-ad-sso-integration-with-a-github-enterprise-cloud-organization"></a>Tutorial: Integración del inicio de sesión único de Azure AD con una organización de GitHub Enterprise Cloud

En este tutorial, aprenderá a integrar una **organización** de GitHub Enterprise Cloud con Azure Active Directory (Azure AD). Al integrar una organización de GitHub Enterprise Cloud con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a la organización de GitHub Enterprise Cloud.
* Administrar el acceso a la organización de GitHub Enterprise Cloud en una ubicación central: Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una organización GitHub creada en [GitHub Enterprise Cloud](https://help.github.com/articles/github-s-products/#github-enterprise), que requiere el [plan de facturación de GitHub Enterprise](https://help.github.com/articles/github-s-billing-plans/#billing-plans-for-organizations).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* GitHub admite el inicio de sesión único iniciado por **SP**

* GitHub admite el aprovisionamiento de usuarios (invitaciones de la organización) [**automatizado**](github-provisioning-tutorial.md).

## <a name="adding-github-from-the-gallery"></a>Adición de GitHub desde la galería

Para configurar la integración de GitHub en Azure AD, debe agregar GitHub desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **GitHub** en el cuadro de búsqueda.
1. Seleccione **GitHub Enterprise Cloud - Organization** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-github"></a>Configuración y prueba del inicio de sesión único de Azure AD para GitHub

Configure y pruebe el inicio de sesión único de Azure AD con GitHub mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de GitHub.

Para configurar y probar el inicio de sesión único de Azure AD con GitHub, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en GitHub](#configure-github-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en GitHub](#create-github-test-user)** , para tener un homólogo de B.Simon en GitHub vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **GitHub**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://github.com/orgs/<Organization ID>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://github.com/orgs/<Organization ID>/saml/consume`

    c. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://github.com/orgs/<Organization ID>/sso`

    > [!NOTE]
    > Tenga en cuenta que estos no son valores reales. Tiene que actualizarlos con el identificador, la dirección URL de respuesta y la URL de inicio de sesión reales. Aquí le recomendamos que utilice el valor de cadena único en el identificador. Vaya a la sección de administración de GitHub para recuperar estos valores.

5. La aplicación GitHub espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde a **Unique User Identifier (Name ID)** (Identificador de usuario único [Identificador de nombre]) se le ha asignado **user.userprincipalname**. La aplicación GitHub espera que al **identificador de usuario único (Identificador de nombre)** se le haya asignado **user.mail**, por lo que se debe editar la asignación de atributos. Para ello, haga clic en el icono **Editar** y cambie la asignación de atributos.

    ![Captura de pantalla que muestra la sección "User Attributes" (Atributos de usuario) con el icono "Edit" (Editar) resaltado.](common/edit-attribute.png)

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

7. En la sección **Set up GitHub** (Configurar GitHub), copie las direcciones URL que necesite.

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

En esta sección va a permitir que B.Simon acceda a GitHub mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **GitHub**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".

    ![rol de usuario](./media/github-tutorial/user-role.png)

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-github-sso"></a>Configuración del inicio de sesión único de GitHub

1. En otra ventana del explorador web, inicie sesión en el sitio de la organización de GitHub como administrador.

2. Vaya a **Configuración** y haga clic en **Seguridad**.

    ![Captura de pantalla que muestra el menú "Organization settings" (Configuración de la organización) de GitHub con la opción "Security" (Seguridad) seleccionada.](./media/github-tutorial/security.png)

3. Active la casilla **Habilitar autenticación SAML** para ver los campos de configuración del inicio de sesión único y siga estos pasos:

    ![Captura de pantalla que muestra la sección "SAML Single Sign-On" (Inicio de sesión único de SAML) con los cuadros de texto "Enable SAML authentication" (Habilitar autenticación SAML) y URL resaltados.](./media/github-tutorial/authentication.png)

    a. Copie el valor de **Single sign-on URL** (Dirección URL de inicio de sesión único) y pegue el valor en el cuadro de texto **URL de inicio de sesión** de la sección **Configuración básica de SAML** de Azure Portal.
    
    b. Copie el valor de **Assertion Consumer Service URL** (Dirección URL del Servicio de consumidor de aserciones) y péguelo en el cuadro de texto **Dirección URL de respuesta** de **Configuración básica de SAML** en Azure Portal.

4. Configure los campos siguientes:

    ![Captura de pantalla que muestra los cuadros de texto "URL de inicio de sesión", "Emisor" y "Certificado público".](./media/github-tutorial/configure.png)

    a. En el cuadro de texto **Sign-On URL** (Dirección URL de inicio de sesión), pegue el valor de la **URL de inicio de sesión** que ha copiado de Azure Portal.

    b. En el cuadro de texto **Emisor**, pegue el valor del **Identificador de Azure AD** que ha copiado de Azure Portal.

    c. Abra el certificado descargado de Azure Portal en el Bloc de notas y pegue el contenido en el cuadro de texto **Certificado público**.

    d. Haga clic en el icono **Editar** para editar el **Método de firma** y el **Método de resumen** desde **RSA-SHA1** y **SHA1** hasta **RSA-SHA256** y **SHA256** tal como se muestra a continuación.
    
    e. Actualice la **dirección URL del servicio de consumidor de aserciones (URL de respuesta)** con respecto a la dirección URL predeterminada, con el fin de que la dirección URL de GitHub coincida con la del registro de aplicaciones de Azure.

    ![Captura de pantalla que muestra la imagen.](./media/github-tutorial/certificate.png)

5. Haga clic en **Test SAML configuration** (Probar configuración de SAML) para configura que no hay errores de validación durante el SSO.

    ![Captura de pantalla que muestra la configuración](./media/github-tutorial/test.png)

6. Haga clic en **Guardar**

> [!NOTE]
> El inicio de sesión único en GitHub realiza la autenticación en una organización específica de GitHub, pero no reemplaza la autenticación propia de GitHub. Por tanto, si la sesión del usuario en github.com ha expirado, puede que se le pida que se autentique con la contraseña o el identificador de GitHub durante el proceso de inicio de sesión único.

### <a name="create-github-test-user"></a>Creación de un usuario de prueba en GitHub

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en GitHub. GitHub admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](github-provisioning-tutorial.md) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

**Para crear un usuario manualmente, siga los pasos siguientes:**

1. Inicie sesión en el sitio de la empresa de GitHub como administrador.

2. Haga clic en **Contactos**.

    ![Captura de pantalla que muestra el sitio de GitHub con los contactos seleccionados.](./media/github-tutorial/people.png "Personas")

3. Haga clic en **Invitar a miembros**.

    ![Captura de pantalla que muestra Invitar a usuarios](./media/github-tutorial/invite-member.png "Invitar a usuarios")

4. En la página de diálogo **Invitar a miembros**, realice los siguientes pasos:

    a. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de la cuenta de Britta Simon.

    ![Captura de pantalla que muestra invitar a personas](./media/github-tutorial/email-box.png "Invitar a contactos")

    b. Haga clic en **Enviar invitación**.

    ![Captura de pantalla que muestra la página de diálogo "Invitar a un miembro" con la opción "Miembro" seleccionada y el botón "Enviar invitación" seleccionado.](./media/github-tutorial/send-invitation.png "Invitar a contactos")

    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo y seguirá un vínculo para confirmar su cuenta antes de que se active.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de GitHub, donde podrá iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de GitHub e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de GitHub en Mis aplicaciones, se le redirigirá a la URL de inicio de sesión de la aplicación. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado GitHub, puede aplicar el control de sesión, que protege su organización, en tiempo real, frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
