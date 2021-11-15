---
title: 'Tutorial: Integración de Azure Active Directory con Trakstar | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Trakstar.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/27/2021
ms.author: jeedes
ms.openlocfilehash: 1ff1ef66fd8dc4e9d8d52b048da197274bdee89d
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131451338"
---
# <a name="tutorial-azure-active-directory-integration-with-trakstar"></a>Tutorial: integración de Azure Active Directory con Trakstar

En este tutorial, aprenderá a integrar Trakstar con Azure Active Directory (Azure AD). Al integrar Trakstar con Azure AD, se puede realizar lo siguiente:

* Controlar en Azure AD quién tiene acceso a Trakstar.
* Permitir que los usuarios inicien sesión automáticamente en Trakstar con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Trakstar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en Trakstar.
* SSO es una característica de pago de Trakstar. Para habilitarlo para su organización, póngase en contacto con el [equipo de soporte técnico de Trakstar](mailto:support@trakstar.com).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Trakstar admite el inicio de sesión único iniciado por **SP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-trakstar-from-the-gallery"></a>Adición de Trakstar desde la galería

Para configurar la integración de Trakstar en Azure AD, será preciso que agregue Trakstar desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Trakstar** en el cuadro de búsqueda.
1. Seleccione **Trakstar** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-trakstar"></a>Configuración y prueba del inicio de sesión único de Azure AD para Trakstar

Configure y pruebe el inicio de sesión único de Azure AD con Trakstar con un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Trakstar.

Para configurar y probar el inicio de sesión único de Azure AD con Trakstar, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Trakstar](#configure-trakstar-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación del usuario de prueba en Trakstar](#create-trakstar-test-user)** : para tener un homólogo de B.Simon en Trakstar vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **Trakstar**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, especifique los siguientes valores en los campos de entrada de texto correspondientes:

    | Nombre del campo | Value | Nota | 
    | ---------------------- | ----- | ---- |
    | **Dirección URL de respuesta (URL del Servicio de consumidor de aserciones)** | `https://app.trakstar.com/auth/saml/callback?namespace=<YOUR_NAMESPACE>` | Reemplace `<YOUR_NAMESPACE>` por un valor real, que esté visible en el campo **ACS (Consumer) URL** (Dirección URL de ACS [Consumidor]) en Trakstar Perform. Consulte la nota que aparece después de esta tabla. |
    | **Dirección URL de inicio de sesión** | `https://app.trakstar.com/auth/saml/?namespace=<YOUR_NAMESPACE>` | Esta dirección _URL es similar_ a la dirección URL anterior, pero no tiene la parte `/callback`. |
    | **Identificador (identificador de entidad)** | `https://app.trakstar.com` | |
    
    > [!NOTE]
    > Estos valores son solo ejemplos. Debe usar los valores específicos del espacio de nombres en Trakstar Perform, que están visibles si inicia sesión en la aplicación y va a **Settings** > **Authentication & SSO** > **SAML 2.0** > **Configure** (Configuración > Autenticación e inicio de sesión único > SAML 2.0 > Configurar).
    > 
    > Si no ve la pestaña **Authentication & SSO** (Autenticación e inicio de sesión único) en **Settings** (Configuración), es posible que no tenga la característica y deba ponerse en contacto con el servicio de asistencia al cliente de Trakstar. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (Base64)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

6. En la sección **Set up Trakstar** (Configurar Trakstar), copie las direcciones URL que necesite.

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

En esta sección, va a permitir que B.Simon acceda a Trakstar mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Trakstar**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-trakstar-sso"></a>Configuración del inicio de sesión único de Trakstar

Para configurar el inicio de sesión único en **Trakstar**, es preciso iniciar sesión como administrador e introducir el contenido del **certificado (Base64)** descargado y las direcciones URL apropiadas copiadas de Azure Portal. Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-trakstar-test-user"></a>Creación del usuario de prueba en Trakstar

En esta sección, creará un usuario llamado Britta Simon en Trakstar. Trabaje con el administrador de Trakstar para agregar los usuarios a la plataforma de Trakstar. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de Trakstar, donde puede comenzar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de Trakstar y comience el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Trakstar en Aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de Trakstar. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Trakstar, podrá aplicar el control de sesión, que protege a la organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).
