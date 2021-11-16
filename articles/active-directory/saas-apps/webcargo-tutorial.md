---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Webcargo | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Webcargo.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/23/2021
ms.author: jeedes
ms.openlocfilehash: 8c8c4257aa168195b5c8cedfa7660ea923adfec4
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132313276"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-webcargo"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Webcargo

En este tutorial va a aprender a integrar Webcargo con Azure Active Directory (Azure AD). Al integrar Webcargo con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Webcargo.
* Permitir que los usuarios inicien sesión automáticamente en Webcargo con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) de Webcargo.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Webcargo admite el SSO iniciado por **SP e IDP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-webcargo-from-the-gallery"></a>Incorporación de Webcargo desde la galería

Para configurar la integración de Webcargo en Azure AD, tiene que agregar Webcargo desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Webcargo** en el cuadro de búsqueda.
1. Seleccione **Webcargo** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-webcargo"></a>Configuración y prueba del inicio de sesión único de Azure AD para Webcargo

Configure y pruebe el SSO de Azure AD con Webcargo con un usuario de prueba de nombre **B.Simon**. Para que el SSO funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Webcargo.

Para configurar y probar el SSO de Azure AD con Webcargo, lleve a cabo los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Webcargo](#configure-webcargo-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Webcargo](#create-webcargo-test-user)** , para tener un homólogo de B.Simon en Webcargo que esté vinculado a la representación de Azure AD del usuario.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Webcargo**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice el siguiente paso:

    En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://www.webcargo.net/sso/azure/account-id/<ID>`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.webcargo.net/sso/azure/account-id/<ID>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Webcargo](mailto:tickets@webcargo.uservoice.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Webcargo**, copie las direcciones URL adecuadas según sus requisitos.

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

En esta sección va a permitir que B.Simon use el inicio de sesión único de Azure al concederle acceso a Webcargo.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Webcargo**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-webcargo-sso"></a>Configuración del inicio de sesión único de Webcargo

Para configurar el inicio de sesión único en **Webcargo**, es preciso enviar el **Certificado (Base 64)** descargado y las direcciones URL correspondientes copiadas de Azure Portal al [equipo de soporte técnico de Webcargo](mailto:tickets@webcargo.uservoice.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-webcargo-test-user"></a>Creación de un usuario de prueba de Webcargo

En esta sección va a crear un usuario de nombre Britta Simon en Webcargo. Colabore con el [equipo de soporte técnico de Webcargo](mailto:tickets@webcargo.uservoice.com) para agregar los usuarios a la plataforma Webcargo. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto redirige a la dirección URL de inicio de sesión de Webcargo, donde puede iniciar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de Webcargo e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de Webcargo para la que ha configurado el SSO. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Webcargo en Aplicaciones, si se ha configurado en modo SP, se le redirige a la página de inicio de sesión de la aplicación para iniciar el flujo de inicio de sesión; si se ha configurado en modo IDP, debería iniciar sesión automáticamente en la instancia de Webcargo para la que ha configurado el SSO. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Webcargo, puede aplicar el control de sesión, que protege frente a la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con las aplicaciones de Microsoft Defender para la nube](/cloud-app-security/proxy-deployment-aad).
