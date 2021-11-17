---
title: 'Tutorial: Integración del inicio de sesión único de Azure Active Directory con Check Point CloudGuard Posture Management | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Check Point CloudGuard Posture Management.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/13/2021
ms.author: jeedes
ms.openlocfilehash: db7e60255369c853ec3986d8446d47466048d1b9
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132288038"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-check-point-cloudguard-posture-management"></a>Tutorial: Integración del inicio de sesión único de Azure Active Directory con Check Point CloudGuard Posture Management

En este tutorial, aprenderá a integrar Check Point CloudGuard Posture Management en Azure Active Directory (Azure AD). Al integrar Check Point CloudGuard Posture Management en Azure AD, podrá:

- Controlar en Azure AD quién tiene acceso a Check Point CloudGuard Posture Management.
- Permitir que los usuarios inicien sesión automáticamente en Check Point CloudGuard Posture Management con sus cuentas de Azure AD.
- Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para el inicio de sesión único Check Point CloudGuard Posture Management.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Check Point CloudGuard Posture Management admite el inicio de sesión único iniciado por **SP e IDP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="adding-check-point-cloudguard-posture-management-from-the-gallery"></a>Incorporación Check Point CloudGuard Posture Management desde la galería

Para configurar la integración de Check Point CloudGuard Posture Management en Azure AD, debe agregar Check Point CloudGuard Posture Management desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En el cuadro de búsqueda de la sección **Agregar desde la galería**, escriba **Check Point CloudGuard Posture Management**.
1. Seleccione **Check Point CloudGuard Posture Management** del panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-check-point-cloudguard-posture-management"></a>Configuración y prueba del inicio de sesión único de Azure AD para Check Point CloudGuard Posture Management

Configure y pruebe el inicio de sesión único de Azure AD con Check Point CloudGuard Posture Management mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario correspondiente de Check Point CloudGuard Posture Management.

Para configurar y probar el inicio de sesión único de Azure AD con Check Point CloudGuard Posture Management, complete los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
   1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
   1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Check Point CloudGuard Posture Management](#configure-check-point-cloudguard-posture-management-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
   1. **[Creación de un usuario de prueba en Check Point CloudGuard Posture Management](#create-check-point-cloudguard-posture-management-test-user)** : para tener un homólogo de B.Simon en Check Point CloudGuard Posture Management vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Check Point CloudGuard Posture Management**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice el siguiente paso:

   En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://secure.dome9.com/sso/saml/<YOURCOMPANYNAME>`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

   En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://secure.dome9.com/sso/saml/<YOURCOMPANYNAME>`

   > [!NOTE]
   > Estos valores no son reales. Actualice estos valores con los valores reales de URL de respuesta y URL de inicio de sesión. Obtendrá el valor de `<company name>` en la sección **Configuración del inicio de sesión único en Check Point CloudGuard Posture Management**, que se explica más adelante en el tutorial. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación Check Point CloudGuard Posture Management espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

   ![imagen](common/edit-attribute.png)

1. Además de lo anterior, la aplicación Check Point CloudGuard Posture Management espera que se usen algunos atributos más en la respuesta de SAML, que se muestran a continuación. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

   | Nombre     | Atributo de origen   |
   | -------- | ------------------ |
   | memberof | user.assignedroles |

   > [!NOTE]
   > Haga clic [aquí](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui) para aprender a crear roles en Azure AD.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

   ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Set up Check Point CloudGuard Posture Management** (Configurar Check Point CloudGuard Posture Management), copie las direcciones URL que necesite.

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

En esta sección va a permitir que B.Simon use el inicio de sesión único de Azure al acceder a Check Point CloudGuard Posture Management.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Check Point CloudGuard Posture Management**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si ha configurado los roles tal y como se explica en el apartado anterior, puede seleccionarlo en la lista desplegable **Seleccionar un rol**.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-check-point-cloudguard-posture-management-sso"></a>Configuración del inicio de sesión único en Check Point CloudGuard Posture Management

1. Para automatizar la configuración en Check Point CloudGuard Posture Management, debe instalar la **extensión del explorador de inicio de sesión seguro de Aplicaciones**. Para ello, haga clic en **Instalar la extensión**.

   ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Setup Check Point CloudGuard Posture Management** (Configurar Check Point CloudGuard Posture Management) para ir a la aplicación del mismo nombre. Desde allí, proporcione las credenciales de administrador para iniciar sesión en Check Point CloudGuard Posture Management. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 6.

   ![Configuración](common/setup-sso.png)

3. Si quiere configurar Check Point CloudGuard Posture Management manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa Check Point CloudGuard Posture Management como administrador y lleve a cabo los siguientes pasos:

4. Haga clic en **Profile Settings** (Configuración de perfil) en la esquina superior derecha y, luego, en **Account Settings** (Configuración de la cuenta).

   ![Captura de pantalla que muestra el menú "Profile Settings" (Configuración de perfil) con la opción "Account Settings" (Configuración de la cuenta) seleccionada.](./media/dome9arc-tutorial/account-settings.png)

5. Vaya a **SSO** y después haga clic en **ENABLE** (HABILITAR).

   ![Captura de pantalla que muestra la pestaña "SSO" y la opción "Enable" (Habilitar) seleccionada.](./media/dome9arc-tutorial/enable.png)

6. En la sección de configuración de SSO, realice los pasos siguientes:

   ![Configuración de Check Point CloudGuard Posture Management](./media/dome9arc-tutorial/configuration.png)

   a. Escriba el nombre de la compañía en el cuadro de texto **Account ID** (Id. de cuenta). Este valor se usará en las direcciones URL de **respuesta** y de **inicio de sesión** que se mencionan en la sección **Configuración básica de SAML** de Azure Portal.

   b. En el cuadro de texto **Issuer** (Emisor), pega el valor de **Identificador de Azure AD** que copiaste de Azure Portal.

   c. En el cuadro de texto **Idp endpoint url** (URL de punto de conexión de Idp), pegue el valor de **Login URL** (Dirección URL de inicio de sesión) que ha copiado de Azure Portal.

   d. Abra el certificado descargado con codificación Base64 en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **X.509 certificate** (Certificado X.509).

   e. Haga clic en **Save**(Guardar).

### <a name="create-check-point-cloudguard-posture-management-test-user"></a>Creación de un usuario de prueba en Check Point CloudGuard Posture Management

Para permitir que los usuarios de Azure AD inicien sesión en Check Point CloudGuard Posture Management, tienen que aprovisionarse en la aplicación. Check Point CloudGuard Posture Management admite el aprovisionamiento Just-In-Time, pero para que funcione correctamente, el usuario debe seleccionar un **rol** determinado y asignarlo al usuario.

> [!Note]
> Para la creación de **roles** y otros detalles, póngase en contacto con el [equipo de soporte técnico al cliente de Check Point CloudGuard Posture Management](mailto:Dome9@checkpoint.com).

**Para aprovisionar una cuenta de usuario manualmente, realice estos pasos:**

1. Inicie sesión en el sitio corporativo Check Point CloudGuard Posture Management como administrador.

2. Haga clic en **Users & Roles** (Usuarios y roles) y luego en **Users** (Usuarios).

   ![Captura de pantalla que muestra "Users & Roles" (Usuarios y roles) con la acción "Users" (Usuarios) seleccionada.](./media/dome9arc-tutorial/users.png)

3. Haga clic en **ADD USER** (Agregar usuario).

   ![Captura de pantalla que muestra "Users & Roles" (Usuarios y roles) con el botón "ADD USER" (Agregar usuario) seleccionado.](./media/dome9arc-tutorial/add-user.png)

4. En la sección **Crear usuario** , lleve a cabo estos pasos:

   ![Agregar empleado](./media/dome9arc-tutorial/create-user.png)

   a. En el cuadro de texto **Email** (Correo electrónico), escriba el correo electrónico del usuario, en el ejemplo B.Simon@contoso.com.

   b. En el cuadro de texto **Nombre**, escribe el nombre del usuario, en este caso, B.

   c. En el cuadro de texto **Apellidos**, escriba el apellido del usuario, en este caso Simon.

   d. Asegúrese de que **SSO User** (Usuario de SSO) esté establecido en **On** (Activado).

   e. Haga clic en **CREATE** (Crear).

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de Check Point CloudGuard Posture Management, desde donde puede poner en marcha el flujo de inicio de sesión.

* Acceda directamente a la URL de inicio de sesión de Check Point CloudGuard Posture Management y ponga en marcha el flujo de inicio de sesión desde ahí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de Check Point CloudGuard Posture Management para la que haya configurado el inicio de sesión único.

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Check Point CloudGuard Posture Management en Aplicaciones, si está configurada en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para iniciar el flujo de inicio de sesión; si está configurada en modo IDP, debería iniciar sesión automáticamente en la instancia de Check Point CloudGuard Posture Management para la que haya configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurada Check Point CloudGuard Posture Management, puede aplicar el control de sesión para proteger la información confidencial de la organización de la filtración y la infiltración en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-any-app).
