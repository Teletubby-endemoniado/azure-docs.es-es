---
title: 'Tutorial: integración de Azure Active Directory con RingCentral | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y RingCentral.
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
ms.openlocfilehash: 6f18ebeb71f597bc7988c8f97840ac1a1e38ebb9
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124830310"
---
# <a name="tutorial-integrate-ringcentral-with-azure-active-directory"></a>Tutorial: Integración de RingCentral con Azure Active Directory

En este tutorial, aprenderá a integrar RingCentral con Azure Active Directory (Azure AD). Al integrar RingCentral con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a RingCentral.
* Permitir que los usuarios puedan iniciar sesión automáticamente en RingCentral con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción de RingCentral habilitada para el inicio de sesión único (SSO).

> [!NOTE]
> Esta integración también está disponible para usarse desde el entorno de la nube del gobierno de EE. UU de Azure AD. Puede encontrar esta aplicación en la galería de aplicaciones de la nube del gobierno de EE. UU. de Azure AD y configurarla de la misma manera que en la nube pública.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* RingCentral admite SSO iniciado por **IDP**.

* RingCentral admite el [aprovisionamiento automático de usuarios](ringcentral-provisioning-tutorial.md).

## <a name="add-ringcentral-from-the-gallery"></a>Agregar RingCentral desde la galería

Para configurar la integración de RingCentral en Azure AD, será preciso que agregue RingCentral desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **RingCentral** en el cuadro de búsqueda.
1. Seleccione **RingCentral** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-ringcentral"></a>Configuración y prueba del inicio de sesión único de Azure AD para RingCentral

Configure y pruebe el inicio de sesión único de Azure AD con RingCentral utilizando un usuario de prueba llamado **Britta Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de RingCentral.

Para configurar y probar el inicio de sesión único de Azure AD con RingCentral, realice los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en RingCentral](#configure-ringcentral-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba en RingCentral](#create-ringcentral-test-user)** , para tener un homólogo de B.Simon en RingCentral que esté vinculado a su representación en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **RingCentral**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si tiene el **archivo de metadatos del proveedor de servicios**, lleve a cabo los siguientes pasos:

    1. Haga clic en **Cargar el archivo de metadatos**.
    1. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.
    1. Una vez que se haya cargado correctamente el archivo de metadatos, los valores **Identificador** y **URL de respuesta** se rellenan automáticamente en la sección **Configuración básica de SAML**.

    > [!Note]
    > Obtendrá el **archivo de metadatos del proveedor de servicios** en la página Configuración de inicio de sesión único de RingCentral que se explica más adelante en el tutorial.

1. Si no tiene el **archivo de metadatos del proveedor de servicios**, escriba los valores para los campos siguientes:

    a. En el cuadro de texto **Identificador**, escriba una de las direcciones URL:
  
    | Identificador |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    b. En el cuadro de texto **URL de respuesta**, escriba una de las siguientes direcciones URL:

    | URL de respuesta |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado Britta Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `Britta Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `BrittaSimon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección va a permitir que B. Simon acceda a RingCentral mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **RingCentral**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-ringcentral-sso"></a>Configuración del inicio de sesión único de RingCentral

1. Para automatizar la configuración en RingCentral, debe instalar la **extensión de explorador de inicio de sesión seguro de Mis aplicaciones**. Para ello, haga clic en **Instale la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

1. Después de agregar la extensión al explorador, haga clic en **Configurar RingCentral** para ir a esta aplicación. En ella, escriba las credenciales de administrador para iniciar sesión en RingCentral. La extensión de explorador configurará automáticamente la aplicación y automatizará los pasos 3 a 7.

    ![Configuración](common/setup-sso.png)

1. Si quiere configurar RingCentral manualmente, abra una nueva ventana del explorador web, inicie sesión en el sitio de empresa de RingCentral como administrador y lleve a cabo los siguientes pasos:

1. En la parte superior, haga clic en **Herramientas**.

    ![Captura de pantalla que muestra la opción de herramientas seleccionada en el sitio de la compañía RingCentral.](./media/ringcentral-tutorial/ringcentral-1.png)

1. Vaya a **Inicio de sesión único**.

    ![Captura de pantalla que muestra la opción de inicio de sesión único seleccionada en el menú de herramientas.](./media/ringcentral-tutorial/ringcentral-2.png)

1. En la página **Inicio de sesión único**, en la sección **Configuración de inicio de sesión único**, desde **Paso 1** haga clic en **Editar** y realice los siguientes pasos:

    ![Captura de pantalla que muestra la página de configuración del inicio de sesión único, donde puede seleccionar la opción para editar.](./media/ringcentral-tutorial/ringcentral-3.png)

1. En la página **Configurar inicio de sesión único** realice los siguientes pasos:

    ![Captura de pantalla que muestra la página de configuración del inicio de sesión único, donde puede cargar los metadatos del proveedor de identidades.](./media/ringcentral-tutorial/ringcentral-4.png)

    a. Haga clic en **Examinar** para cargar el archivo de metadatos que descargó de Azure Portal.

    b. Después de cargar los metadatos los valores se rellenan automáticamente en la sección **Información general de inicio de sesión único**.

    c. En la sección **Asignación de atributos**, seleccione **Asignar atributo de correo electrónico a** como `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. Haga clic en **Save**(Guardar).

    e. En **Paso 2** haga clic en **Descargar** para descargar el **archivo de metadatos del proveedor de servicios** y cargarlo en la sección **Configuración básica de SAML** para rellenar de forma automática los valores **Identificador** y **URL de respuesta** en Azure Portal.

    ![Captura de pantalla que muestra la página de configuración del inicio de sesión único, donde puede seleccionar la opción para descargar.](./media/ringcentral-tutorial/ringcentral-6.png) 

    f. En la misma página, vaya a la sección **Habilitar Inicio de sesión único** y realice los siguientes pasos:

    ![Captura de pantalla que muestra la sección para habilitar el inicio de sesión único, donde puede finalizar la configuración.](./media/ringcentral-tutorial/ringcentral-5.png)

    * Seleccione **Habilitar inicio de sesión único**.

    * Seleccione **Permitir a los usuarios iniciar sesión con credenciales de inicio de sesión único o de RingCentral**.

    * Haga clic en **Save**(Guardar).

### <a name="create-ringcentral-test-user"></a>Creación de un usuario de prueba de RingCentral

En esta sección, creará un usuario llamado Britta Simon en RingCentral. Colabore con el [equipo de soporte técnico de RingCentral](https://success.ringcentral.com/RCContactSupp) para agregar usuarios en la plataforma de RingCentral. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

RingCentral también admite el aprovisionamiento automático de usuarios. [Aquí](./ringcentral-provisioning-tutorial.md) puede encontrar más detalles sobre cómo configurar el aprovisionamiento automático de usuarios.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones.

* Haga clic en Probar esta aplicación en Azure Portal; debería iniciar sesión automáticamente en la instancia de RingCentral para la que ha configurado el inicio de sesión único.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de RingCentral en Aplicaciones, debería iniciar sesión automáticamente en la instancia de RingCentral para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado RingCentral, puede aplicar el control de sesión, que protege su organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).