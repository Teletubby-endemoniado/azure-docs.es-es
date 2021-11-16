---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Datadog | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Datadog.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/11/2021
ms.author: jeedes
ms.openlocfilehash: 23bdf05960af9eb94a2b003af113e7d66c4ea69e
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132338352"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-datadog"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Datadog

En este tutorial, aprenderá a integrar Datadog con Azure Active Directory (Azure AD). Al integrar Datadog con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Datadog.
* Permitir que los usuarios inicien sesión automáticamente en Datadog con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Datadog.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Datadog admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="add-datadog-from-the-gallery"></a>Adición de Datadog desde la galería

Para configurar la integración de Datadog en Azure AD, deberá agregar Datadog desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Datadog** en el cuadro de búsqueda.
1. Seleccione **Datadog** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-datadog"></a>Configuración y prueba del inicio de sesión único de Azure AD para Datadog

Configure y pruebe el inicio de sesión único de Azure AD con Datadog mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Datadog.

Para configurar y probar el inicio de sesión único de Azure AD con Datadog, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Datadog](#configure-datadog-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    1. Creación de un usuario de prueba de Datadog : para tener un homólogo de B.Simon en Datadog que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **Datadog**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.

1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

1. En la sección **Configuración básica de SAML**, el usuario no tiene que hacer nada porque la aplicación ya se ha integrado previamente con Azure.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://app.datadoghq.com/account/login/id/<CUSTOM_IDENTIFIER>`

    > [!NOTE]
    > Este valor no es real. Actualice el valor con la dirección URL de inicio de sesión real en la [configuración de SAML de Datadog](https://app.datadoghq.com/organization-settings/login-methods/saml). También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal. Para usar el inicio de sesión comenzado por IdP y el inicio de sesión comenzado por SP juntos, se necesitan ambas versiones de la dirección URL de ACS configurada en Azure.

1. Haga clic en **Save**(Guardar).

1. En la página **Configuración del inicio de sesión único con SAML**, en **User Attributes & Claims** (Atributos y notificaciones del usuario), haga clic en el icono de lápiz para modificar la configuración.

1. Haga clic en el botón **Agregar una notificación de grupo**. De forma predeterminada en Azure AD, el nombre de la notificación de grupo es una dirección URL. Por ejemplo, `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups`. Si quiere cambiarla por un valor de nombre para mostrar, como **grupos**, seleccione **Opciones avanzadas** y, luego, cambie el nombre de la notificación de grupo por **grupos**.

   > [!NOTE]
   > El atributo de origen se establece en `Group ID`. Este es el UUID del grupo en Azure AD. Esto significa que Azure AD envía el identificador de grupo como valor de atributo de notificación de grupo, no como nombre del grupo. Debe cambiar las asignaciones en Datadog para que la asignación sea al identificador de grupo en lugar de al nombre del grupo. Para más información, consulte [Asignaciones SAML de Datadog](https://docs.datadoghq.com/account_management/saml/#mapping-saml-attributes-to-datadog-roles).

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

1. En la sección **Configurar Datadog**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon acceda a Datadog mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Datadog**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-datadog-sso"></a>Configuración del inicio de sesión único de Datadog

Para configurar el inicio de sesión único en **Datadog**, es preciso cargar el **XML de metadatos de federación** descargado en la [configuración de SAML de Datadog](https://app.datadoghq.com/organization-settings/login-methods/saml).

## <a name="test-sso"></a>Prueba de SSO 

Pruebe la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de Datadog, donde puede iniciar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de Datadog y comience el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en Datadog para la que ha configurado el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Datadog en el portal de Aplicaciones, si está configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión y, si está configurado en modo IDP, debería iniciar sesión automáticamente en la versión de Datadog para la que configuró el inicio de sesión único. Para más información sobre Aplicaciones, consulte [Introducción al portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

### <a name="enable-all-users-from-your-tenant-to-authenticate-with-the-app"></a>Habilitación de todos los usuarios del inquilino para que se autentiquen con la aplicación

En esta sección, habilitará a todos los usuarios del inquilino para que accedan a Datadog si un usuario tiene una cuenta en Datadog.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Datadog**.
1. En la página de información general de la aplicación, en **Administrar**, seleccione **Propiedades**.

    ![Vínculo "Propiedades"](common/properties.png)

1. En **¿Asignación de usuarios?** , seleccione **No**.

    ![No es necesaria la asignación de usuarios.](common/user-assignment-not-required.png)

1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Datadog, puede aplicar el control de sesión, que protege la filtración e infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-any-app).
