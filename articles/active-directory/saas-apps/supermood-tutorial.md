---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Supermood | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Supermood.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/31/2019
ms.author: jeedes
ms.openlocfilehash: 606d6563ed78e291f975262864d74739656fd6f6
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124752011"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-supermood"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Supermood

En este tutorial, aprenderá cómo integrar Supermood con Azure Active Directory (Azure AD). Al integrar Supermood con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Supermood.
* Permitir a los usuarios iniciar sesión automáticamente en Supermood con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Supermood.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Supermood admite el inicio de sesión único iniciado por **SP e IDP**.
* Supermood admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-supermood-from-the-gallery"></a>Adición de Supermood desde la galería

Para configurar la integración de Supermood en Azure AD, será preciso agregar Supermood desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Supermood** en el cuadro de búsqueda.
1. Seleccione **Supermood** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-supermood"></a>Configuración y prueba del inicio de sesión único de Azure AD para Supermood

Configure y pruebe el inicio de sesión único de Azure AD con Supermood mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Supermood.

Para configurar y probar el inicio de sesión único de Azure AD con Supermood, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Supermood](#configure-supermood-sso)**: para configurar los valores del inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba de Supermood](#create-supermood-test-user)**: para tener un homólogo de B.Simon en Supermood que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Supermood**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    a. Seleccione **Establecer direcciones URL adicionales**.
    
    b. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL: `https://supermood.co/auth/sso/saml20`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://supermood.co/app/#!/loginv2`

1. Haga clic en **Save**(Guardar).

1. La aplicación Supermood espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación Supermood espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen|
    | ---------------| ------|
    | firstName | user.givenname |
    | lastName | user.surname |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

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

En esta sección, va a conceder a B.Simon acceso a Supermood mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Supermood**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-supermood-sso"></a>Configuración del inicio de sesión único de Supermood

1. Vaya a su panel de administración de Supermood.co como Administrador de seguridad.

1. Haga clic en **My account** (Mi cuenta) (abajo a la izquierda) y **Single Sign On (SSO)** (Inicio de sesión único [SSO]).

    ![Sencillo del certificado](./media/supermood-tutorial/tutorial_supermood_single.png)

1. En **Your SAML 2.0 configurations** (Sus configuraciones SAML 2.0), haga clic en **Add an SAML 2.0 configuration for an email domain** (Agregar una configuración de SAML 2.0 para un dominio de correo electrónico).

    ![Agregar del certificado](./media/supermood-tutorial/tutorial_supermood_add.png)

1. En **Add an SAML 2.0 configuration for an email domain** (Agregar una configuración de SAML 2.0 para un dominio de correo electrónico). sección, realice los siguientes pasos:

    ![SAML del certificado](./media/supermood-tutorial/tutorial_supermood_saml.png)

    a. En el cuadro de texto **email domain for this Identity provider** (dominio de correo electrónico para este proveedor de identidades), escriba su dominio.

    b. En el cuadro de texto **Use a metadata URL** (Utilizar una dirección URL de metadatos), pegue la **dirección URL de metadatos de federación de aplicación** que ha copiado de Azure Portal.

    c. Haga clic en **Agregar**.

### <a name="create-supermood-test-user"></a>Creación de un usuario de prueba de Supermood

En esta sección, se crea un usuario llamado Britta Simon en Supermood. Supermood admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe en Supermood, se crea otro después de la autenticación. Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Supermood](mailto:hello@supermood.fr).

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Supermood en el panel de acceso, debería iniciar sesión automáticamente en la versión de Supermood para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Pruebe Supermood con Azure AD](https://aad.portal.azure.com/).