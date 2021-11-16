---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con iHASCO Training | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e iHASCO Training.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/25/2021
ms.author: jeedes
ms.openlocfilehash: f1d5726df4f7e112529e6271813b9d2b732e63da
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132336038"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-ihasco-training"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con iHASCO Training

En este tutorial, aprenderá a integrar iHASCO Training con Azure Active Directory (Azure AD). Al integrar iHASCO Training con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a iHASCO Training.
* Permitir que los usuarios inicien sesión automáticamente en iHASCO Training con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en iHASCO Training.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* iHASCO Training admite el inicio de sesión único iniciado por **SP**.
* iHASCO Training admite el aprovisionamiento de usuarios **Just-In-Time**.


## <a name="adding-ihasco-training-from-the-gallery"></a>Adición de iHASCO Training desde la galería

Para configurar la integración de iHASCO Training en Azure AD, tendrá que agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **iHASCO Training** en el cuadro de búsqueda.
1. Seleccione **iHASCO Training** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-sso-for-ihasco-training"></a>Configuración y prueba del SSO de Azure AD para iHASCO Training

Configure y pruebe el inicio de sesión único de Azure AD con iHASCO Training mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario correspondiente de iHASCO Training.

Para configurar y probar el inicio de sesión único de Azure AD con iHASCO Training, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en iHASCO Training](#configure-ihasco-training-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en iHASCO Training](#create-ihasco-training-test-user)** : para tener un homólogo de B.Simon en iHASCO Training que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **iHASCO Training**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://authentication.ihasco.co.uk/saml2/<ID>/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://authentication.ihasco.co.uk/saml2/<ID>/acs`
    
    c. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://app.ihasco.co.uk/<ID>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de iHASCO Training](mailto:support@ihasco.co.uk) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar iHASCO Training**, copie las direcciones URL que necesite.

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

En esta sección, va a permitir que B.Simon acceda a iHASCO Training mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **iHASCO Training**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-ihasco-training-sso"></a>Configuración del inicio de sesión único de iHASCO Training

1. Inicie sesión en el sitio web de iHASCO Training como administrador.

1. Haga clic en **Settings** (Configuración) en el panel de navegación superior derecho, desplácese hasta el icono **ADVANCED** (OPCIONES AVANZADAS) y haga clic en **Configure Single Sign On** (Configurar inicio de sesión único).

    ![Captura de pantalla del botón de inicio de sesión único de iHASCO Training.](./media/ihasco-training-tutorial/settings.png)

1. En la pestaña **IDENTITY PROVIDERS** (PROVEEDORES DE IDENTIDADES), haga clic en **Add provider** (Agregar proveedor) y seleccione **SAML2**.

    ![Captura de pantalla de los proveedores de identidades de iHASCO Training.](./media/ihasco-training-tutorial/add-provider.png)

1. Realice los pasos siguientes en la página **Single Sign On / New SAML2** (Inicio de sesión único: nuevo SAML2):

    ![Captura de pantalla del inicio de sesión único de iHASCO Training.](./media/ihasco-training-tutorial/single-sign-on.png)

    a. En **GENERAL**, escriba una **Descripción** para identificar esta configuración.

    b. Under **IDENTITY PROVIDER DETAILS** (DETALLES DEL PROVEEDOR DE IDENTIDIDADES), en el cuadro de texto **Single Sign-on URL** (Dirección URL de inicio de sesión único), pegue el valor de la **Dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    c. En el cuadro de texto **URL de cierre de sesión único**, pegue el valor de **URL de cierre de sesión** que ha copiado de Azure Portal.

    d. En el cuadro de texto **Entity ID** (Id. de entidad), pegue el valor de **Identificador** que ha copiado de Azure Portal.

    e. En el Bloc de notas, abra el **Certificado (Base64)** que ha descargado de Azure Portal, copie el contenido y, luego, péguelo en el cuadro de texto **X509 (Public) Certificate** (Certificado X509 público).

    f. En **USER ATTRIBUTE MAPPING** (ASIGNACIÓN DE ATRIBUTOS DE USUARIO), en **Email address** (Dirección de correo electrónico), escriba el valor como `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    g. En **First name** (Nombre), escriba el valor como `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    h. En **Last name** (Apellidos), escriba el valor como `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.

    i. Haga clic en **Save**(Guardar).

    j. Haga clic en **Enable now** (Habilitar ahora) después de volver a cargar la página.

1. Haga clic en **Security** (Seguridad) en el panel de navegación de la izquierda y seleccione **Single Sign On provider** (Proveedor de inicio de sesión único) como método de **registro** y **Azure AD configuration** (Configuración de Azure AD) como **Selected provider** (Proveedor seleccionado).

    ![Captura de pantalla de la seguridad de iHASCO Training.](./media/ihasco-training-tutorial/security.png)

1. Haga clic en **Guardar cambios**.

### <a name="create-ihasco-training-test-user"></a>Creación de un usuario de prueba de iHASCO Training

En esta sección se creará un usuario llamado Britta Simon en iHASCO Training. iHASCO Training admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si todavía no existe ningún usuario en iHASCO Training, se crea uno después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de iHASCO Training, donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de iHASCO Training e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de iHASCO Training en Mis aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de la aplicación. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).


## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado iHASCO Training, podrá aplicar el control de sesión, que protege a la organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-any-app).
