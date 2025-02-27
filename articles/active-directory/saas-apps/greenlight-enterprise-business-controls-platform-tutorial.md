---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con Greenlight Enterprise Business Controls Platform'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Greenlight Enterprise Business Controls Platform.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/18/2021
ms.author: jeedes
ms.openlocfilehash: 3b80c7c8c0a3fa0a61270146d7d3ec79a1d88774
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132311552"
---
# <a name="tutorial-azure-ad-sso-integration-with-greenlight-enterprise-business-controls-platform"></a>Tutorial: Integración del inicio de sesión único de Azure AD con Greenlight Enterprise Business Controls Platform

En este tutorial, aprenderá a integrar Greenlight Enterprise Business Controls Platform con Azure Active Directory (Azure AD). Al integrar Greenlight Enterprise Business Controls Platform con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a Greenlight Enterprise Business Controls Platform.
* Permitir que los usuarios inicien sesión automáticamente en Greenlight Enterprise Business Controls Platform con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Greenlight Enterprise Business Controls Platform.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Greenlight Enterprise Business Controls Platform admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="add-greenlight-enterprise-business-controls-platform-from-the-gallery"></a>Adición de Greenlight Enterprise Business Controls Platform desde la galería

Para configurar la integración de Greenlight Enterprise Business Controls Platform en Azure AD, es preciso agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la Galería**, escriba **Greenlight Enterprise Business Controls Platform** en el cuadro de búsqueda.
1. Seleccione **Greenlight Enterprise Business Controls Platform** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-greenlight-enterprise-business-controls-platform"></a>Configuración y prueba del inicio de sesión único de Azure AD para Greenlight Enterprise Business Controls Platform

Configure y pruebe el inicio de sesión único de Azure AD con Greenlight Enterprise Business Controls Platform mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Greenlight Enterprise Business Controls Platform.

Para configurar y probar el inicio de sesión único de Azure AD con Greenlight Enterprise Business Controls Platform, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Greenlight Enterprise Business Controls Platform](#configure-greenlight-enterprise-business-controls-platform-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Greenlight Enterprise Business Controls Platform](#create-greenlight-enterprise-business-controls-platform-test-user)** , para tener un homólogo de B.Simon en Greenlight Enterprise Business Controls Platform que esté vinculado a la representación Azure AD del usuario.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Greenlight Enterprise Business Controls Platform**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

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
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-greenlight-enterprise-business-controls-platform-sso"></a>Configuración del inicio de sesión único de Greenlight Enterprise Business Controls Platform

Para configurar el inicio de sesión único en **Greenlight Enterprise Business Controls Platform**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de Greenlight Enterprise Business Controls Platform](mailto:support@greenlightcorp.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-greenlight-enterprise-business-controls-platform-test-user"></a>Creación de un usuario de prueba de Greenlight Enterprise Business Controls Platform

En esta sección, creará un usuario llamado B.Simon en Greenlight Enterprise Business Controls Platform. Trabaje con el [equipo de soporte técnico de GreenLight Enterprise Business Controls Platform](mailto:support@greenlightcorp.com) para agregar los usuarios a Greenlight Enterprise Business Controls Platform. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de Greenlight Enterprise Business Controls Platform, donde podrá poner en marcha el flujo de inicio de sesión.  

* Acceda directamente a la dirección URL de inicio de sesión de Greenlight Enterprise Business Controls Platform y ponga en marcha el flujo de inicio de sesión desde ahí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Debería iniciarse la sesión automáticamente en la instancia de Greenlight Enterprise Business Controls Platform para la que configuró el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Greenlight Enterprise Business Controls Platform, si se ha configurado el modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para poner en marcha el flujo de inicio de sesión y, si ha configurado el modo IDP, se debería iniciar sesión automáticamente en la instancia de Greenlight Enterprise Business Controls Platform para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Greenlight Enterprise Business Controls Platform, puede aplicar controles de sesión que protejan la filtración e infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con las aplicaciones de Microsoft Defender para la nube](/cloud-app-security/proxy-deployment-aad).
