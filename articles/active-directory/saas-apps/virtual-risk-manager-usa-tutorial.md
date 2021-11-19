---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con Virtual Risk Manager - USA'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Virtual Risk Manager - USA.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/15/2021
ms.author: jeedes
ms.openlocfilehash: a0bfc4a2a7394c1ea22921759f8ea3454e408ce2
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132557190"
---
# <a name="tutorial-azure-ad-sso-integration-with-virtual-risk-manager---usa"></a>Tutorial: Integración del inicio de sesión único de Azure AD con Virtual Risk Manager - USA

En este tutorial, aprenderá a integrar Virtual Risk Manager - USA con Azure Active Directory (Azure AD). Al integrar Virtual Risk Manager - USA con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Virtual Risk Manager - USA.
* Permitir que los usuarios inicien sesión automáticamente en Virtual Risk Manager - USA con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción que permita utilizar el inicio de sesión único (SSO) en Virtual Risk Manager - USA.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Virtual Risk Manager - USA permite utilizar el inicio de sesión único con **IDP**.

* Virtual Risk Manager - USA admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="add-virtual-risk-manager---usa-from-the-gallery"></a>Adición de Virtual Risk Manager - USA desde la galería

Para configurar la integración de Virtual Risk Manager - USA en Azure AD, es preciso agregar la aplicación desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Virtual Risk Manager - USA** en el cuadro de búsqueda.
1. Seleccione **Virtual Risk Manager - USA** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-virtual-risk-manager---usa"></a>Configuración y comprobación del inicio de sesión único de Azure AD con Virtual Risk Manager - USA

Configure y pruebe el inicio de sesión único de Azure AD con Virtual Risk Manager - USA con un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Virtual Risk Manager - USA.

Para configurar y probar el inicio de sesión único de Azure AD con Virtual Risk Manager - USA, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Virtual Risk Manager - USA](#configure-virtual-risk-manager---usa-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en Virtual Risk Manager - USA](#create-virtual-risk-manager---usa-test-user)** : para tener un homólogo de B.Simon en Virtual Risk Manager - USA que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Virtual Risk Manager - USA**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, la aplicación está preconfigurada en el modo iniciado por **IDP** y las direcciones URL necesarias ya se han rellenado previamente con Azure. El usuario debe guardar la configuración, para lo que debe hacer clic en el botón **Guardar**.

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

En esta sección, va a permitir que B.Simon acceda a Virtual Risk Manager - USA utilizando el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Virtual Risk Manager - USA**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-virtual-risk-manager---usa-sso"></a>Configuración del inicio de sesión único en Virtual Risk Manager - USA

Para configurar el inicio de sesión único en **Virtual Risk Manager - USA**, debe enviar la **dirección URL de metadatos de federación de la aplicación** al [equipo de soporte técnico de Virtual Risk Manager - USA](mailto:globalsupport@edriving.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-virtual-risk-manager---usa-test-user"></a>Creación de un usuario de prueba en Virtual Risk Manager - USA

En esta sección, va a crear una usuaria llamado Britta Simon en Virtual Risk Manager - USA. Virtual Risk Manager - USA admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si no existe ningún usuario en Virtual Risk Manager - USA, se creará uno después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones.

* Haga clic en Probar esta aplicación en Azure Portal. Debería iniciar sesión automáticamente en la instancia de Virtual Risk Manager - USA para la que configuró el inicio de sesión único.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Virtual Risk Manager - USA del panel de acceso, debería iniciar sesión automáticamente en la versión de Virtual Risk Manager - USA para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Virtual Risk Manager - USA, puede aplicar el control de sesión, que protege en tiempo real la información confidencial de su organización frente a posibles filtraciones. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).