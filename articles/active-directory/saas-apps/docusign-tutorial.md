---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con DocuSign | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único (SSO) entre Azure Active Directory y DocuSign.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/26/2021
ms.author: jeedes
ms.openlocfilehash: 1cf1da4b583e78ebf9e5cf9e17da4674d9401388
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124770251"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-docusign"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con DocuSign

En este tutorial, obtendrá información sobre cómo integrar DocuSign con Microsoft Azure Active Directory (Azure AD). Al integrar DocuSign con Azure AD, puede hacer lo siguiente:

* Use Azure AD para controlar quién tiene acceso a DocuSign.
* Habilite el inicio de sesión automático en DocuSign para los usuarios mediante sus cuentas de Azure AD.
* Administrar sus cuentas en una ubicación central: Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción de DocuSign habilitada para el inicio de sesión único (SSO).

> [!NOTE]
> Esta integración también está disponible para usarse desde el entorno de la nube del gobierno de EE. UU de Azure AD. Puede encontrar esta aplicación en la galería de aplicaciones de la nube del gobierno de EE. UU. de Azure AD y configurarla de la misma manera que en la nube pública.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba para comprobar que:

* DocuSign admite el inicio de sesión único iniciado por el proveedor de servicios o **SP**.

* DocuSign admite el aprovisionamiento de usuarios **Just-In-Time**.

* DocuSign admite el [aprovisionamiento automático de usuarios](./docusign-provisioning-tutorial.md).

## <a name="adding-docusign-from-the-gallery"></a>Adición de DocuSign desde la galería

Para configurar la integración de DocuSign en Azure AD, deberá agregar DocuSign desde la galería a la lista de aplicaciones SaaS administradas:

1. Inicie sesión en Azure Portal mediante una cuenta profesional o educativa, o bien una cuenta personal de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **DocuSign** en el cuadro de búsqueda.
1. Seleccione **DocuSign** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-sso-for-docusign"></a>Configuración y prueba del inicio de sesión único de Azure AD para DocuSign

Configure y pruebe el inicio de sesión único de Azure AD con DocuSign mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, tiene que establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de DocuSign.

Para configurar y probar el inicio de sesión único de Azure AD con DocuSign, siga estos pasos:

1. [Configuración del inicio de sesión único en Azure AD](#configure-azure-ad-sso), para que los usuarios puedan usar esta característica.
    1. [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con B. Simon.
    1. [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para que B. Simon pueda usar el inicio de sesión único de Azure AD.
1. [Configuración del inicio de sesión único en DocuSign](#configure-docusign-sso), para configurar los valores de Inicio de sesión único en la aplicación.
    1. [Creación de un usuario de prueba de DocuSign](#create-docusign-test-user), para generar un homólogo de B.Simon en DocuSign que esté vinculado a la representación de ella en Azure AD.
1. [Pruebe el inicio de sesión único](#test-sso) para comprobar que la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Para habilitar el inicio de sesión único de Azure AD en Azure Portal, siga estos pasos:

1. En Azure Portal, en la página de integración de la aplicación **DocuSign**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, seleccione el icono con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:

    `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. En el cuadro de texto **Identificador (id. de entidad)** , escriba una dirección URL con el siguiente patrón:

    `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    c. En el cuadro de texto **Dirección URL de respuesta**, escriba una dirección URL con alguno de los siguientes patrones:
    
    | URL de respuesta |
    |-------------|
    | Producción: |
    | `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/<IDPID>` |
    | `https://<subdomain>.docusign.net/SAML/` |
    | Instancia de QA:|
    | `https://<SUBDOMAIN>.docusign.com/organizations/saml2` |

    > [!NOTE]
    > Estos valores entre corchetes son marcadores de posición. Reemplácelos por los valores reales de la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta. Estos detalles se explican en la sección "Visualización de los puntos de conexión SAML 2.0" más adelante en este tutorial.

1. En la página **Configuración del inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** . Seleccione **Descargar** para descargar el certificado y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Set up DocuSign** (Configurar DocuSign), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, creará un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. En la parte superior de la pantalla, seleccione **Nuevo usuario**.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba **B.Simon**.  
   1. En el campo **Nombre de usuario**, escriba `<username>@<companydomain>.<extension>`. Por ejemplo: `B.Simon@contoso.com`.
   1. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a DocuSign para que este usuario pueda utilizar el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **DocuSign**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario** y, después, en el cuadro de diálogo **Agregar asignación**, selección **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** en la lista **Usuarios** y, luego, pulse el botón **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-docusign-sso"></a>Configuración del inicio de sesión único de DocuSign

1. Para automatizar la configuración de DocuSign, tiene que instalar la extensión del explorador de inicio de sesión seguro de Mis aplicaciones. Para ello, seleccione **Instale la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador seleccione **Setup DocuSign** (Instalar DocuSign). Se le dirigirá a la aplicación DocuSign. En ella, escriba las credenciales de administrador para iniciar sesión en DocuSign. La extensión del explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 5.

    ![Configuración](common/setup-sso.png)

3. Si quiere configurar DocuSign manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio web de empresa de DocuSign como administrador.

4. En la esquina superior derecha de la página, seleccione el logotipo del perfil y, después, **Go to Admin** (Ir al administrador).
  
    ![Go to Admin (Ir al administrador) bajo el perfil][51]

5. En la página de soluciones de dominio, seleccione **Domains** (Dominios).

    ![Soluciones de dominio/Domains (Dominios)][50]

6. En la sección **Domains** (Dominios), seleccione **CLAIM DOMAIN** (NOTIFICAR DOMINIO).

    ![Opción Claim Domain (Notificar dominio)][52]

7. En el cuadro de diálogo **Claim a domain** (Notificar un dominio), en el cuadro **Domain Name** (Nombre de dominio), escriba el dominio de la compañía y seleccione **CLAIM** (NOTIFICAR). Asegúrese de que comprueba el dominio y que su estado es activo.

    ![Cuadro de diálogo Claim a Domain/Domain Name (Notificar un dominio/Nombre de dominio)][53]

8. En la página de soluciones de dominio, seleccione **Identity Providers** (Proveedores de identidades).
  
    ![Opción Identity Providers (Proveedores de identidades)][54]

9. En la sección **Identity Providers** (Proveedores de identidades), seleccione en **ADD IDENTITY PROVIDER** (AGREGAR PROVEEDOR DE IDENTIDADES).

    ![Opción Add Identity Provider (Agregar proveedor de identidades)][55]

10. En la página **Identity Provider Settings** (Configuración del proveedor de identidades), siga estos pasos:

    ![Campos Identity Provider Settings (Configuración del proveedor de identidades)][56]

    a. En el cuadro **Name** (Nombre), escriba un nombre único para la configuración. No use espacios.

    b. En el cuadro **Identity Provider Issuer** (Emisor de proveedor de identidades), pegue el valor de **Identificador de Azure AD** que ha copiado de Azure Portal.

    c. En el cuadro **Identity provider Login URL** (Dirección URL de inicio de sesión del proveedor de identidades), pegue la **Dirección URL de inicio de sesión** copiada de Azure Portal.

    d. En el cuadro **Identity Provider Logout URL** (Dirección URL de cierre de sesión del proveedor de identidades), pegue el valor de **Dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    e. Seleccione **Sign AuthN Request** (Firmar solicitud de autenticación).

    f. Para **Send AuthN request by** (Enviar solicitud de autenticación por), seleccione **POST**.

    g. En **Send logout request by** (Enviar solicitud de cierre de sesión por), seleccione **GET**.

    h. En la sección **Custom Attribute Mapping** (Asignación de atributos personalizados), seleccione **ADD NEW MAPPING** (AGREGAR NUEVA ASIGNACIÓN).

       ![Interfaz de usuario de Custom Attribute Mapping (Asignación de atributos personalizados)][62]

    i. Elija el campo que desea asignar a la notificación de Azure AD. En este ejemplo, la notificación **emailaddress** está asociada al valor de `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`. Este es el nombre de notificación predeterminado de Azure AD para la notificación de correo electrónico. Seleccione **SAVE** (GUARDAR).

       ![Campos de Custom Attribute Mapping (Asignación de atributos personalizados)][57]

       > [!NOTE]
       > Utilice el **identificador de usuario** adecuado para asignar el usuario de Azure AD a la asignación de usuarios de DocuSign. Seleccione el campo apropiado y escriba el valor adecuado según la configuración de la organización.

    j. En la sección **Identity Provider Certificates** (Certificados del proveedor de identidades), haga clic en **ADD CERTIFICATE** (AGREGAR CERTIFICADO), cargue el certificado que ha descargado del portal de Azure AD y haga clic en **SAVE** (GUARDAR).

       ![Identity Provider Certificates (Certificados del proveedor de identidades)/Add Certificate (Agregar certificado)][58]

    k. En la sección **Identity Providers** (Proveedores de identidades), seleccione **ACTIONS** (ACCIONES) y luego **Endpoints** (Puntos de conexión).

       ![Identity Providers (Proveedores de identidades)/Endpoints (Puntos de conexión)][59]

    l. En la sección **Visualización de los puntos de conexión SAML 2.0** del portal de administración de DocuSign, siga estos pasos:

       ![Visualización de los puntos de conexión SAML 2.0][60]
       
       1. Copie la información de **Service Provider Issuer URL** (Dirección URL del emisor del proveedor de servicios) y luego péguela en el cuadro **Identificador** de la sección **Configuración básica de SAML** de Azure Portal.
       
       1. Copie la información de **Service Provider Assertion Consumer Service URL** (URL del Servicio de consumidor de aserciones) y luego péguela en el cuadro **URL de respuesta** de la sección **Configuración básica de SAML** de Azure Portal.
       
       1. Copie la información de **Service Provider Issuer URL** (Dirección URL del emisor del proveedor de servicios) y luego péguela en el cuadro **URL de inicio de sesión**  de la sección **Configuración básica de SAML** de Azure Portal. Al final del valor de **Service Provider Login URL** (Dirección URL de inicio de sesión del proveedor de servicios) figura el valor de IDPID.

       1. Seleccione **Cerrar**.

### <a name="create-docusign-test-user"></a>Creación de un usuario de prueba en DocuSign

En esta sección, se crea un usuario llamado B.Simon en DocuSign. DocuSign admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe en DocuSign, se crea otro después de la autenticación.

> [!Note]
> Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de DocuSign](https://support.docusign.com/).

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

1. Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de DocuSign, donde puede iniciar el flujo de inicio de sesión. 

2. Vaya directamente a la dirección URL de inicio de sesión de DocuSign e inicie el flujo de inicio de sesión desde allí.

3. Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de DocuSign en Aplicaciones, se debería iniciar sesión automáticamente en la versión de DocuSign para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).


## <a name="next-steps"></a>Pasos siguientes

Una vez configurado DocuSign, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).

<!--Image references-->

[50]: ./media/docusign-tutorial/tutorial-docusign-18.png
[51]: ./media/docusign-tutorial/tutorial-docusign-21.png
[52]: ./media/docusign-tutorial/tutorial-docusign-22.png
[53]: ./media/docusign-tutorial/tutorial-docusign-23.png
[54]: ./media/docusign-tutorial/tutorial-docusign-19.png
[55]: ./media/docusign-tutorial/tutorial-docusign-20.png
[56]: ./media/docusign-tutorial/tutorial-docusign-24.png
[57]: ./media/docusign-tutorial/tutorial-docusign-25.png
[58]: ./media/docusign-tutorial/tutorial-docusign-26.png
[59]: ./media/docusign-tutorial/tutorial-docusign-27.png
[60]: ./media/docusign-tutorial/tutorial-docusign-28.png
[61]: ./media/docusign-tutorial/tutorial-docusign-29.png
[62]: ./media/docusign-tutorial/tutorial-docusign-30.png