---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Bright Pattern Omnichannel Contact Center | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Bright Pattern Omnichannel Contact Center.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/18/2019
ms.author: jeedes
ms.openlocfilehash: fb5c3a5811afdd003bf91a99bd64ed5fa8952a49
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124750001"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-bright-pattern-omnichannel-contact-center"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Bright Pattern Omnichannel Contact Center

En este tutorial, obtendrá información sobre cómo integrar Bright Pattern Omnichannel Contact Center con Azure Active Directory (Azure AD). Al integrar Bright Pattern Omnichannel Contact Center con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Bright Pattern Omnichannel Contact Center.
* Permitir que los usuarios inicien sesión automáticamente Bright Pattern Omnichannel Contact Center mediante sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para inicio de sesión único (SSO) en Bright Pattern Omnichannel Contact Center.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.



* Bright Pattern Omnichannel Contact Center admite SSO iniciado por **SP e IDP**
* Bright Pattern Omnichannel Contact Center admite el aprovisionamiento de usuarios **Just-In-Time**


## <a name="adding-bright-pattern-omnichannel-contact-center-from-the-gallery"></a>Agregar Bright Pattern Omnichannel Contact Center desde la galería

Para configurar la integración de Bright Pattern Omnichannel Contact Center en Azure AD, deberá agregar Bright Pattern Omnichannel Contact Center desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Bright Pattern Omnichannel Contact Center** en el cuadro de búsqueda.
1. Seleccione **Bright Pattern Omnichannel Contact Center** del panel de resultados y luego agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-bright-pattern-omnichannel-contact-center"></a>Configuración y prueba del inicio de sesión único de Azure AD para Bright Pattern Omnichannel Contact Center

Configure y pruebe el SSO de Azure AD con Bright Pattern Omnichannel Contact Center mediante un usuario de prueba llamado **B.Simon**. Para que SSO funcione, tiene que establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Bright Pattern Omnichannel Contact Center.

Para configurar y probar el SSO de Azure AD con Bright Pattern Omnichannel Contact Center, complete los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Bright Pattern Omnichannel Contact Center](#configure-bright-pattern-omnichannel-contact-center-sso)** : para la configuración del inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Bright Pattern Omnichannel Contact Center](#create-bright-pattern-omnichannel-contact-center-test-user)**, para tener un homólogo de B.Simon en Bright Pattern Omnichannel Contact Center que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Bright Pattern Omnichannel Contact Center**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `<SUBDOMAIN>_sso`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.brightpattern.com/agentdesktop/sso/redirect`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.brightpattern.com/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico al cliente de Bright Pattern Omnichannel Contact Center](mailto:support@brightpattern.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación Bright Pattern Omnichannel Contact Center espera las aserciones de SAML en un formato específico, lo cual requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/edit-attribute.png)

1. Además de lo anterior, la aplicación Bright Pattern Omnichannel Contact Center espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Espacio de nombres  |
    | ---------------| --------------- |
    | firstName | user.givenname |
    | lastName | user.surname |
    | email | user.mail |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Bright Pattern Omnichannel Contact Center**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, habilitará a B.Simon para usar el inicio de sesión único de Azure al concederle acceso a Bright Pattern Omnichannel Contact Center.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Bright Pattern Omnichannel Contact Center**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-bright-pattern-omnichannel-contact-center-sso"></a>Configuración del SSO de Bright Pattern Omnichannel Contact Center

Para configurar el inicio de sesión único en **Bright Pattern Omnichannel Contact Center**, debe enviar el **certificado (Base64)** descargado y las direcciones URL adecuadas copiadas desde Azure Portal al [equipo de soporte técnico de Bright Pattern Omnichannel Contact Center](mailto:support@brightpattern.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-bright-pattern-omnichannel-contact-center-test-user"></a>Creación de un usuario de prueba Bright Pattern Omnichannel Contact Center

En esta sección, se crea un usuario llamado B.Simon en Bright Pattern Omnichannel Contact Center. Bright Pattern Omnichannel Contact Center admite el aprovisionamiento de usuarios Just-in-time, que está habilitado de manera predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe en Bright Pattern Omnichannel Contact Center, se crea uno nuevo después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Bright Pattern Omnichannel Contact Center en el panel de acceso, debería iniciar sesión automáticamente en la versión de Bright Pattern Omnichannel Contact Center para la que configurara el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Prueba de Bright Pattern Omnichannel Contact Center con Azure AD](https://aad.portal.azure.com/)