---
title: 'Tutorial: integración de Azure Active Directory con Tangoe Command Premium Mobile | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Tangoe Command Premium Mobile.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/02/2021
ms.author: jeedes
ms.openlocfilehash: a2806ffc460ebf213fd2bcad3b09c1a955e79cbf
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132282752"
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Tutorial: Integración de Azure Active Directory con Tangoe Command Premium Mobile

En este tutorial, obtendrá información sobre cómo integrar Tangoe Command Premium Mobile con Azure Active Directory (Azure AD). Al integrar Tangoe Command Premium Mobile con Azure AD, puede hacer lo siguiente:

* Controlar desde Azure AD quién tiene acceso a Tangoe Command Premium Mobile.
* Permitir que los usuarios inicien sesión automáticamente en Tangoe Command Premium Mobile con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Tangoe Command Premium Mobile, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción de Tangoe Command Premium Mobile con el inicio de sesión único habilitado.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Tangoe Command Premium Mobile admite el SSO iniciado por **SP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-tangoe-command-premium-mobile-from-the-gallery"></a>Incorporación de Tangoe Command Premium Mobile desde la galería

Para configurar la integración de Tangoe Command Premium Mobile en Azure AD, deberá agregar Tangoe Command Premium Mobile desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Tangoe Command Premium Mobile** en el cuadro de búsqueda.
1. Seleccione **Tangoe Command Premium Mobile** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-tangoe-command-premium-mobile"></a>Configuración y prueba del SSO de Azure AD para Tangoe Command Premium Mobile

Configure y pruebe el SSO de Azure AD con Tangoe Command Premium Mobile mediante un usuario de prueba llamado **B.Simon**. Para que el SSO funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Tangoe Command Premium Mobile.

Para configurar y probar el SSO de Azure AD con Tangoe Command Premium Mobile, realice los pasos siguientes:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del SSO de Tangoe Command Premium Mobile](#configure-tangoe-command-premium-mobile-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Tangoe Command Premium Mobile](#create-tangoe-command-premium-mobile-test-user)** : para tener un homólogo de B.Simon en Tangoe Command Premium Mobile que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Tangoe Command Premium Mobile** , busque la sección **Administrar** y seleccione **inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<TENANT_ISSUER>&TARGET=<TARGET_PAGE_URL>`

    b. En el cuadro de texto **URL de respuesta**, escriba la dirección URL: `https://sso.tangoe.com/sp/ACS.saml2`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Tangoe Command Premium Mobile Client](https://www.tangoe.com/contact-us/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

6. En la sección **Configurar Tangoe Command Premium Mobile**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, permitirá que B.Simon use el inicio de sesión único de Azure al concederle acceso a Tangoe Command Premium Mobile.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Tangoe comando Premium Mobile**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-tangoe-command-premium-mobile-sso"></a>Configuración del SSO de Tangoe Command Premium Mobile

Para configurar el inicio de sesión único en **Tangoe Command Premium Mobile**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de Tangoe Command Premium Mobile](https://www.tangoe.com/contact-us/). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-tangoe-command-premium-mobile-test-user"></a>Creación de un usuario de prueba de Tangoe Command Premium Mobile

En esta sección, creará un usuario llamado Britta Simon en Tangoe Command Premium Mobile. Trabaje con el [equipo de soporte técnico de Tangoe Command Premium Mobile](https://www.tangoe.com/contact-us/) para agregar los usuarios a la plataforma de Tangoe Command Premium Mobile. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de Tangoe Command Premium Mobile, donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de Tangoe Command Premium Mobile e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Tangoe Command Premium Mobile en Aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de dicha aplicación. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Tangoe Command Premium Mobile, puede aplicar el control de sesión, que protege a la organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender for Cloud Apps](/cloud-app-security/proxy-deployment-aad).
