---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Opal | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Opal.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.openlocfilehash: 7a7930270385519891829a0639d8c3e24d6dad74
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124738174"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-opal"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Opal

En este tutorial, aprenderá a integrar Opal con Azure Active Directory (Azure AD). Si integra Opal con Azure AD, podrá hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Opal.
* Permitir que los usuarios inicien sesión automáticamente en Opal con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Opal.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Opal admite inicio de sesión único iniciado por **IDP**

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="adding-opal-from-the-gallery"></a>Adición de Opal desde la galería

Para configurar la integración de Opal en Azure AD, será preciso agregar Opal desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Opal** en el cuadro de búsqueda.
1. Seleccione **Opal** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-opal"></a>Configuración y prueba del inicio de sesión único de Azure AD con Opal

Configure y pruebe el inicio de sesión único (SSO) de Azure AD con Opal utilizando un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Opal.

Para configurar y probar el inicio de sesión único de Azure AD con Opal, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Opal](#configure-opal-sso)**, para configurar el inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Opal](#create-opal-test-user)**, para tener un homólogo de B.Simon en Opal que esté vinculado a la representación de este usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Opal**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la página **Configurar el inicio de sesión único con SAML**, escriba los valores de los siguientes campos:

    1. En el cuadro de texto **Identificador**, escriba una dirección URL: `Opal`

    1. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:

        `https://<subdomain>.ouropal.com/auth/saml/callback`

        > [!NOTE]
        > El valor de dirección URL de respuesta no es real. Actualícelo con la dirección URL de respuesta real. Póngase en contacto con el [equipo de atención al cliente de Opal](mailto:support@workwithopal.com) para obtener el valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación Opal espera las aserciones de SAML en un formato específico, lo que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/edit-attribute.png)

1. Además de lo anterior, la aplicación Opal espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen|
    | ---------------| --------------- |
    | firstname | user.givenname |
    | lastname | user.surname |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Configurar Opal**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon acceda a Opal utilizando el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Opal**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-opal-sso"></a>Configuración del inicio de sesión único en Opal

Para configurar el inicio de sesión único en **Opal**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de Opal](mailto:support@workwithopal.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-opal-test-user"></a>Creación de un usuario de prueba de Opal

En esta sección, creará un usuario llamado Britta Simon en Opal. Trabaje con el [equipo de soporte técnico de Opal](mailto:support@workwithopal.com) para agregar los usuarios a la plataforma de Opal. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Opal en el panel de acceso, debería iniciar sesión automáticamente en la versión de Opal para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Pruebe Opal con Azure AD](https://aad.portal.azure.com/)