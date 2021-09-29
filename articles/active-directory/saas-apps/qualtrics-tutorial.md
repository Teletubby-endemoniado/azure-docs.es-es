---
title: 'Tutorial: Integración de Azure Active Directory con SAP Qualtrics | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SAP Qualtrics.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/18/2021
ms.author: jeedes
ms.openlocfilehash: 4573cfc7e2901d57256320c16f5f0074f89b0ddb
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124825797"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sap-qualtrics"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con SAP Qualtrics

En este tutorial aprenderá a integrar SAP Qualtrics con Azure Active Directory (Azure AD). Al integrar SAP Qualtrics con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a SAP Qualtrics.
* Permitir que los usuarios inicien sesión automáticamente en SAP Qualtrics con sus cuentas de Azure AD.
* Administrar sus cuentas en una ubicación central: Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesitará lo siguiente:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en SAP Qualtrics.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SAP Qualtrics admite el inicio de sesión único iniciado por **SP** e **IDP**.
* SAP Qualtrics admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="add-sap-qualtrics-from-the-gallery"></a>Adición de SAP Qualtrics desde la galería

Para configurar la integración de SAP Qualtrics en Azure AD, deberá agregar SAP Qualtrics desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel izquierdo, seleccione **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **SAP Qualtrics** en el cuadro de búsqueda.
1. Seleccione **SAP Qualtrics** en los resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sap-qualtrics"></a>Configuración y prueba del inicio de sesión único de Azure AD para SAP Qualtrics

Configure y pruebe el inicio de sesión único de Azure AD con SAP Qualtrics mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de SAP Qualtrics.

Para configurar y probar el inicio de sesión único de Azure AD con SAP Qualtrics, es preciso completar los siguientes bloques de creación:

1. [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso), para permitir que los usuarios puedan utilizar esta característica.
    1. [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con B. Simon.
    1. [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para que B. Simon pueda usar el inicio de sesión único de Azure AD.
1. [Configuración del inicio de sesión único de SAP Qualtrics](#configure-sap-qualtrics-sso), para configurar los valores de inicio de sesión único en la aplicación.
    1. [Creación de un usuario de prueba en SAP Qualtrics](#create-sap-qualtrics-test-user), para tener un homólogo de B.Simon en SAP Qualtrics vinculado a la representación del usuario en Azure AD.
1. [Comprobación del inicio de sesión único](#test-sso), para verificar que la configuración funciona correctamente.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **SAP Qualtrics**, busque la sección **Administrar**. Seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, seleccione el icono con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la página **Configurar el inicio de sesión único con SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:
    
    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:

    `https://< DATACENTER >.qualtrics.com`
   
    b. En el cuadro de texto **Dirección URL de respuesta**, escriba una dirección URL con el siguiente patrón:

    `https://< DATACENTER >.qualtrics.com/login/v1/sso/saml2/default-sp`

    c. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL con el siguiente patrón:

    `https://< brandID >.< DATACENTER >.qualtrics.com`

1. Seleccione **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:

    `https://< brandID >.< DATACENTER >.qualtrics.com`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de inicio de sesión, el identificador, la dirección URL de respuesta y el estado de la retransmisión reales. Póngase en contacto con el [equipo de soporte técnico de Qualtrics](https://www.qualtrics.com/support/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione el icono Copiar para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Seleccione la casilla **Mostrar contraseña** y, a continuación, anote la contraseña.
   1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a SAP Qualtrics mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales** > **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **SAP Qualtrics**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. Después, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** en la lista de usuarios. A continuación, elija **Seleccionar** en la parte inferior de la pantalla.
1. Si espera algún valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione el rol adecuado para el usuario en la lista. A continuación, elija **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, seleccione **Asignar**.

## <a name="configure-sap-qualtrics-sso"></a>Configuración del inicio de sesión único de SAP Qualtrics

Para configurar el inicio de sesión único en SAP Qualtrics, deberá enviar la **dirección URL de metadatos de federación de aplicación** de Azure Portal al [equipo de soporte técnico de SAP Qualtrics](https://www.qualtrics.com/support/). El equipo de soporte técnico se asegurará de que la conexión de inicio de sesión único de SAML esté establecida correctamente en ambos lados.

### <a name="create-sap-qualtrics-test-user"></a>Creación de un usuario de prueba de SAP Qualtrics

SAP Qualtrics admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. El usuario no tiene que hacer nada en esta sección. Si un usuario no existe ya en SAP Qualtrics, se crea uno nuevo después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de SAP Qualtrics, desde donde puede poner en marcha el flujo de inicio de sesión.  

* Vaya directamente a la URL de inicio de sesión de SAP Qualtrics y ponga en marcha el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de SAP Qualtrics para la que configuró el inicio de sesión único.

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de SAP Qualtrics en Mis aplicaciones, si se ha configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión; y si se ha configurado en modo IDP, se debería iniciar sesión automáticamente en la instancia de SAP Qualtrics para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Después de configurar SAP Qualtrics, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. Para más información, consulte [Cómo aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).