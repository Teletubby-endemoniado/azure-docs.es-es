---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Oktopost SAML | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Oktopost SAML.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/05/2021
ms.author: jeedes
ms.openlocfilehash: 77b651283a20c055ad43b6cca7d5a1aa7dad4609
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132300284"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-oktopost-saml"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Oktopost SAML

En este tutorial, aprenderá a integrar Oktopost SAML con Azure Active Directory (Azure AD). Al integrar Oktopost SAML con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Oktopost SAML.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Oktopost SAML con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en Oktopost SAML.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Oktopost SAML admite el inicio de sesión único iniciado por **SP e IDP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.


## <a name="adding-oktopost-saml-from-the-gallery"></a>Incorporación de Oktopost SAML desde la galería

Para configurar la integración de Oktopost SAML en Azure AD, es preciso agregar Oktopost SAML desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Oktopost SAML** en el cuadro de búsqueda.
1. Seleccione **Oktopost SAML** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-sso-for-oktopost-saml"></a>Configuración y prueba del inicio de sesión único de Azure AD para Oktopost SAML

Configure y pruebe el inicio de sesión único de Azure AD con Oktopost SAML con un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Oktopost SAML.

Para configurar y probar el inicio de sesión único de Azure AD con Oktopost SAML, lleve a cabo los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Oktopost SAML](#configure-oktopost-saml-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Oktopost SAML](#create-oktopost-saml-test-user)** : para tener un homólogo de B.Simon en Oktopost SAML que esté vinculado a su representación en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Oktopost SAML**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://app.oktopost.com/auth/login`


1. Haga clic en **Save**(Guardar).

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configuración de Oktopost SAML**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon utilice el inicio de sesión único de Azure para acceder a Oktopost SAML.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Oktopost SAML**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-oktopost-saml-sso"></a>Configuración del inicio de sesión único de Oktopost SAML

1. Inicie sesión en Oktopost SAML como administrador.

1. Haga clic en el **icono de usuario > Configuración**.

    ![Configuración de Oktopost SAML](./media/oktopost-saml-tutorial/settings.png)

1. En **Configuración**, vaya a la página **Seguridad > Inicio de sesión único** y realice los siguientes pasos.

    ![Configuración de Oktopost SAML](./media/oktopost-saml-tutorial/configure-sso.png)

    a. Establezca **Habilitar el inicio de sesión único** en **Sí**.

    b. En el cuadro de texto **SAML endpoint** (Punto de conexión de SAML), pegue el valor de **Login URL** (Dirección URL de inicio de sesión) que ha copiado de Azure Portal.

    c. En el cuadro de texto **Issuer** (Emisor), pegue el valor de **Identificador de Azure AD** que ha copiado de Azure Portal.

    d. Abra en el Bloc de notas el archivo del **certificado (Base64)** que descargó de Azure Portal, copie el contenido y, luego, péguelo en el cuadro de texto **Certificate X.509** (Certificado).
    
    e. Haga clic en **Save**(Guardar).

### <a name="create-oktopost-saml-test-user"></a>Creación de un usuario de prueba de Oktopost SAML

1. Inicie sesión en Oktopost SAML como administrador.

1. Haga clic en el **icono de usuario > Configuración**.

    ![Usuario de prueba 1 de Oktopost SAML](./media/oktopost-saml-tutorial/settings.png)

1.  Vaya a **Administración de usuarios > Usuarios > Agregar usuario**.

    ![Usuario de prueba 2 de Oktopost SAML](./media/oktopost-saml-tutorial/add-user-1.png)

1. Rellene los campos obligatorios del menú emergente y haga clic en **Enviar**.

    ![Usuario de prueba 3 de Oktopost SAML](./media/oktopost-saml-tutorial/add-user-2.png)

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de Oktopost SAML, donde puede iniciar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de Oktopost SAML e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Se debería iniciar sesión automáticamente en la instancia de Oktopost SAML para la que configuró el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Oktopost SAML en Mis aplicaciones, si se ha configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión; y si se ha configurado en modo IDP, debería iniciar sesión automáticamente en la instancia de Oktopost SAML para la que ha configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).


## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Oktopost SAML, puede aplicar el control de sesión, que protege a su organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-any-app).
