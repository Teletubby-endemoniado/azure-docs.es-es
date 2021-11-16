---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con embed signage'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y embed signage.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/01/2021
ms.author: jeedes
ms.openlocfilehash: 6f5a8742e831be54073cf23d820706be1df7fb2e
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132330412"
---
# <a name="tutorial-azure-ad-sso-integration-with-embed-signage"></a>Tutorial: Integración del inicio de sesión único de Azure AD con embed signage

En este tutorial, aprenderá a integrar embed signage con Azure Active Directory (Azure AD). Al integrar embed signage con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a embed signage.
* Permitir que los usuarios puedan iniciar sesión automáticamente en embed signage con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en embed signage.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* embed signage admite el inicio de sesión único iniciado por **IDP**.

## <a name="add-embed-signage-from-the-gallery"></a>Incorporación de embed signage desde la galería

Para configurar la integración de embed signage en Azure AD, deberá agregar esta aplicación desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **embed signage** en el cuadro de búsqueda.
1. Seleccione **embed signage** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-embed-signage"></a>Configuración y prueba del inicio de sesión único de Azure AD para embed signage

Configure y pruebe el inicio de sesión único de Azure AD con embed signage con un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de embed signage.

Para configurar y probar el inicio de sesión único de Azure AD con embed signage, complete los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de embed signage](#configure-embed-signage-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en embed signage](#create-embed-signage-test-user)** , para tener un homólogo de B.Simon en embed signage que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **embed signage**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la página **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://app.embedsignage.com/auth/saml/<ID>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://app.embedsignage.com/auth/saml/login/<ID>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte de cliente de embed signage](mailto:support@embedsignage.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Configurar embed signage**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon acceda a embed signage mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **embed signage**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-embed-signage-sso"></a>Configuración del inicio de sesión único de embed signage

1. Inicie sesión como administrador en el sitio de la compañía de embed signage.

1. Vaya a **Configuración de la cuenta** y haga clic en **Seguridad** > **Inicio de sesión único**.

1. En la sección **Inicio de sesión único**, siga estos pasos:

    ![Captura de pantalla que muestra la cuenta de inicio de sesión único.](./media/embed-signage-tutorial/settings.png "Cuenta de SSO")

    1. **Habilite** la casilla de inicio de sesión único.

    1. Abra el archivo **XML de metadatos de federación** descargado de Azure Portal y cargue el archivo en **Archivo XML de metadatos**.

    1. Haga clic en **Guardar cambios**.

### <a name="create-embed-signage-test-user"></a>Creación de un usuario de prueba de embed signage

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de embed signage como administrador.

1. Vaya a **Configuración de la cuenta** y haga clic en **Usuarios** > **Nuevo usuario**.

1. En la sección **Configuración** rellene los campos obligatorios manualmente en la página siguiente y haga clic en **Crear usuario**.  

    ![Captura de pantalla que muestra la creación de usuarios.](./media/embed-signage-tutorial/account.png "Usuarios de SSO")

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones.

* Haga clic en Probar esta aplicación en Azure Portal; debería iniciar sesión automáticamente en la instancia de embed signage para la que configuró el inicio de sesión único.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de embed signage en Aplicaciones, se debería iniciar sesión automáticamente en la versión de embed signage para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado embed signage, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
