---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Greenlight Enterprise Business Controls Platform | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Greenlight Enterprise Business Controls Platform.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/04/2020
ms.author: jeedes
ms.openlocfilehash: 74cffc8c495e76ba631db49c69eb63e188bf4b30
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124833508"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-greenlight-enterprise-business-controls-platform"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Greenlight Enterprise Business Controls Platform

En este tutorial, aprenderá a integrar Greenlight Enterprise Business Controls Platform con Azure Active Directory (Azure AD). Al integrar Greenlight Enterprise Business Controls Platform con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a Greenlight Enterprise Business Controls Platform.
* Permitir que los usuarios inicien sesión automáticamente en Greenlight Enterprise Business Controls Platform con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Greenlight Enterprise Business Controls Platform.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Greenlight Enterprise Business Controls Platform admite el inicio de sesión único iniciado por **SP e IDP**

* Una vez configurado Greenlight Enterprise Business Controls Platform, puede aplicar controles de sesión que protejan la filtración e infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-greenlight-enterprise-business-controls-platform-from-the-gallery"></a>Incorporación de Greenlight Enterprise Business Controls Platform desde la galería

Para configurar la integración de Greenlight Enterprise Business Controls Platform en Azure AD, es preciso agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la Galería**, escriba **Greenlight Enterprise Business Controls Platform** en el cuadro de búsqueda.
1. Seleccione **Greenlight Enterprise Business Controls Platform** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-sso-for-greenlight-enterprise-business-controls-platform"></a>Configuración y prueba del inicio de sesión único de Azure AD para Greenlight Enterprise Business Controls Platform

Configure y pruebe el inicio de sesión único de Azure AD con Greenlight Enterprise Business Controls Platform mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Greenlight Enterprise Business Controls Platform.

Para configurar y probar el inicio de sesión único de Azure AD con Greenlight Enterprise Business Controls Platform, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Greenlight Enterprise Business Controls Platform](#configure-greenlight-enterprise-business-controls-platform-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Greenlight Enterprise Business Controls Platform](#create-greenlight-enterprise-business-controls-platform-test-user)** , para tener un homólogo de B.Simon en Greenlight Enterprise Business Controls Platform que esté vinculado a la representación Azure AD del usuario.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Greenlight Enterprise Business Controls Platform**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<CUSTOMER_NAME>.gltcloud.com/ebcpplatform/saml`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<CUSTOMER_NAME>.gltcloud.com/ebcpplatform/saml`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<CUSTOMER_NAME>.gltcloud.com/ebcpplatform/saml`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico del cliente de Greenlight Enterprise Business Controls Platform](mailto:support@greenlightcorp.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Set up Greenlight Enterprise Business Controls Platform** (Configurar Greenlight Enterprise Business Controls Platform), copie las direcciones URL que necesite.

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

En esta sección va a permitir que B.Simon acceda a Greenlight Enterprise Business Controls Platform mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Greenlight Enterprise Business Controls Platform**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-greenlight-enterprise-business-controls-platform-sso"></a>Configuración del inicio de sesión único de Greenlight Enterprise Business Controls Platform

Para configurar el inicio de sesión único en **Greenlight Enterprise Business Controls Platform**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de Greenlight Enterprise Business Controls Platform](mailto:support@greenlightcorp.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-greenlight-enterprise-business-controls-platform-test-user"></a>Creación de un usuario de prueba de Greenlight Enterprise Business Controls Platform

En esta sección, creará un usuario llamado B.Simon en Greenlight Enterprise Business Controls Platform. Trabaje con el [equipo de soporte técnico de GreenLight Enterprise Business Controls Platform](mailto:support@greenlightcorp.com) para agregar los usuarios a Greenlight Enterprise Business Controls Platform. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Greenlight Enterprise Business Controls Platform en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Greenlight Enterprise Business Controls Platform para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Prueba de Greenlight Enterprise Business Controls Platform con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesiones en Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Protección de Greenlight Enterprise Business Controls Platform con visibilidad y controles avanzados](/cloud-app-security/proxy-intro-aad)