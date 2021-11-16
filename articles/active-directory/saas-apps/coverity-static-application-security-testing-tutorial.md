---
title: 'Tutorial: Integración del inicio de sesión único (SSO ) de Azure AD con Coverity Static Application Security Testing'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Coverity Static Application Security Testing.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/03/2021
ms.author: jeedes
ms.openlocfilehash: 1ab33f1905b69c0a90fbd0584982d693dcfa86d5
ms.sourcegitcommit: 27ddccfa351f574431fb4775e5cd486eb21080e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "132000585"
---
# <a name="tutorial-azure-ad-sso-integration-with-coverity-static-application-security-testing"></a>Tutorial: Integración del inicio de sesión único (SSO ) de Azure AD con Coverity Static Application Security Testing

En este tutorial, aprenderá a integrar Coverity Static Application Security Testing con Azure Active Directory (Azure AD). Al integrar Coverity Static Application Security Testing con Azure AD, podrá hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Coverity Static Application Security Testing.
* Permitir que los usuarios inicien sesión automáticamente en Coverity Static Application Security Testing con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción que permita utilizar el inicio de sesión único (SSO) en Coverity Static Application Security Testing.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Coverity Static Application Security Testing permite utilizar el servicio de inicio de sesión único con **SP e IDP**.

## <a name="add-coverity-static-application-security-testing-from-the-gallery"></a>Adición de Coverity Static Application Security Testing desde la galería

Para configurar la integración de Coverity Static Application Security Testing en Azure AD, debe agregar dicha herramienta desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Coverity Static Application Security Testing** en el cuadro de búsqueda.
1. Seleccione **Coverity Static Application Security Testing** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-coverity-static-application-security-testing"></a>Configuración y comprobación del inicio de sesión único de Azure AD en Coverity Static Application Security Testing

Configure y pruebe el inicio de sesión único de Azure AD con Coverity Static Application Security Testing utilizando un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario de Coverity Static Application Security Testing relacionado.

Para configurar y probar el inicio de sesión único de Azure AD con Coverity Static Application Security Testing, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Coverity Static Application Security Testing](#configure-coverity-static-application-security-testing-sso)** : para configurar los valores de inicio de sesión único en el lado de la aplicación.
    1. **[Creación de un usuario de prueba en Coverity Static Application Security Testing](#create-coverity-static-application-security-testing-test-user)** : para tener un homólogo de B.Simon en Coverity Static Application Security Testing que esté vinculado a la representación del usuario de Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Coverity Static Application Security Testing**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos: 

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<COVERITYURL>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<COVERITYURL>`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<COVERITYURL>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Coverity Static Application Security Testing Client](mailto:software-integrity-support@synopsys.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (PEM)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificate-base64-download.png)

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

En esta sección, va a permitir que B.Simon acceda a Coverity Static Application Security Testing utilizando el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Coverity Static Application Security Testing**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-coverity-static-application-security-testing-sso"></a>Configuración del inicio de sesión único de Coverity Static Application Security Testing

Para configurar el inicio de sesión único en **Coverity Static Application Security Testing**, tiene que enviar el **certificado (PEM)** descargado y las direcciones URL apropiadas que haya copiado de Azure Portal al [equipo de soporte técnico de Coverity Static Application Security Testing](mailto:software-integrity-support@synopsys.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-coverity-static-application-security-testing-test-user"></a>Creación de un usuario de prueba en Coverity Static Application Security Testing

En esta sección, va a crear una usuaria llamado Britta Simon en Coverity Static Application Security Testing. Colabore con el [equipo de soporte técnico de Coverity Static Application Security Testing](mailto:software-integrity-support@synopsys.com) para agregar usuarios a esta plataforma. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Al hacerlo, accederá automáticamente a la dirección URL de Coverity Static Application Security Testing, donde podrá comenzar el flujo de inicio de sesión.  

* Acceda directamente a la dirección URL de inicio de sesión único de Coverity Static Application Security Testing Sign y comience el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* En Azure Portal, haga clic en **Probar esta aplicación**. Al hacerlo, debería iniciar sesión automáticamente en la instancia de Coverity Static Application Security Testing en la que configuró el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Cuando haga clic en el icono de Coverity Static Application Security Testing de Aplicaciones, si seleccionó el modo SP en la configuración, debería acceder automáticamente a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión. Por el contrario, si selección el modo IDP en la configuración, debería iniciar sesión automáticamente en la instancia de Coverity Static Application Security Testing en la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Coverity Static Application Security Testing, podrá aplicar el control de sesión, que protege la información confidencial de la organización en tiempo real de posibles filtraciones. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).