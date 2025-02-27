---
title: 'Tutorial: integración de Azure Active Directory con HappyFox | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y HappyFox.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/25/2021
ms.author: jeedes
ms.openlocfilehash: 9a48ec4cd9bbc62b3a8a54ecaab743749ca6c767
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132291471"
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a>Tutorial: integración de Azure Active Directory con HappyFox

En este tutorial, aprenderá a integrar HappyFox con Azure Active Directory (Azure AD). Si integra HappyFox con Azure AD, podrá hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a HappyFox.
* Permitir que los usuarios puedan iniciar sesión automáticamente en HappyFox con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en HappyFox.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* HappyFox admite el inicio de sesión único iniciado por **SP**.

* HappyFox admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="add-happyfox-from-the-gallery"></a>Adición de HappyFox desde la galería

Para configurar la integración de HappyFox en Azure AD, será preciso que lo agregue desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **HappyFox** en el cuadro de búsqueda.
1. Seleccione **HappyFox** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-happyfox"></a>Configuración y prueba del inicio de sesión único de Azure AD para HappyFox

Configure y pruebe el inicio de sesión único de Azure AD con HappyFox mediante un usuario de prueba llamado **B. Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario correspondiente de HappyFox.

Para configurar y probar el inicio de sesión único de Azure AD con HappyFox, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en HappyFox](#configure-happyfox-sso)** : para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación del usuario de prueba de HappyFox](#create-happyfox-test-user)** : para tener un homólogo de B.Simon en HappyFox vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **HappyFox**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.happyfox.com/`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.happyfox.com/saml/metadata/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de HappyFox](https://support.happyfox.com/home) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up HappyFox** (Configurar HappyFox), copie las direcciones URL que necesite.

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

En esta sección va a conceder a B.Simon acceso a HappyFox mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **HappyFox**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-happyfox-sso"></a>Configuración del inicio de sesión único en HappyFox

1. En otra ventana del explorador web, inicie sesión en el inquilino de HappyFox como administrador.

2. Vaya a **Manage** (Administrar), haga clic en la pestaña **Integrations** (Integraciones).

    ![Captura de pantalla que muestra la página "Manage" (Administrar) con la pestaña "Integrations" (Integraciones) seleccionada.](./media/happyfox-tutorial/header.png) 

3. En la pestaña de integraciones, haga clic en **Configure** (Configurar) en **SAML Integration** (Integración de SAML) para abrir la configuración del inicio de sesión único.

    ![Captura de pantalla que muestra la configuración "S A M L Integration" (Integración de S A M L) con la acción "Configure" (Configurar) seleccionada.](./media/happyfox-tutorial/configure.png)

4. En la sección de configuración de SAML, pegue el valor de **Dirección URL de inicio de sesión** que ha copiado de Azure Portal en el cuadro de texto **SSO Target URL** (Dirección URL de destino de SSO).

    ![Captura de pantalla que muestra la sección "S A M L Configuration" (Configuración de S A M L) con el cuadro de texto "S S O Target U R L" (Dirección U R L de destino de S S O) resaltado.](./media/happyfox-tutorial/target.png)

5. Abra el certificado descargado desde Azure Portal en el bloc de notas y pegue el contenido en la sección **IdP Signature** (Firma del proveedor de identidades).

    ![Captura de pantalla que muestra la sección "I d P Signature" (Firma del proveedor de identidades) resaltada.](./media/happyfox-tutorial/certificate.png)

6. Haga clic en el botón **Guardar configuración**.

    ![Configurar inicio de sesión único](./media/happyfox-tutorial/save-settings.png)

### <a name="create-happyfox-test-user"></a>Creación del usuario de prueba de HappyFox

En esta sección, se crea un usuario llamado Britta Simon en HappyFox. HappyFox admite el aprovisionamiento de usuarios Just-In-Time, habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe en HappyFox, se crea uno después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante Mis aplicaciones.

1. Al hacer clic en el icono de HappyFox en Aplicaciones, debería obtener la página de inicio de sesión de la aplicación HappyFox. Debería ver el botón **"SAML"** en la página de inicio de sesión.

    ![Complemento](./media/happyfox-tutorial/apps.png)

2. Haga clic en el botón **SAML** para iniciar sesión en HappyFox con su cuenta de Azure AD.

Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado HappyFox, podrá aplicar el control de sesión, que protege la información confidencial de su organización en tiempo real de posibles filtraciones. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
