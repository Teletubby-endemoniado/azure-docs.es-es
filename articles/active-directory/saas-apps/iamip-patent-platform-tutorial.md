---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con IamIP Patent Platform | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y IamIP Patent Platform.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/28/2021
ms.author: jeedes
ms.openlocfilehash: 5de5587e7f517d28207bf041b655591736f84a57
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124833141"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-iamip-patent-platform"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con IamIP Patent Platform

En este tutorial, aprenderá a integrar IamIP Patent Platform con Azure Active Directory (Azure AD). Al integrar IamIP Patent Platform con Azure AD, puede:

* Usar Azure AD para controlar quién puede acceder a IamIP Patent Platform.
* Permitir que los usuarios inicien sesión automáticamente en IamIP Patent Platform con sus cuentas de Azure AD.
* Administrar sus cuentas en una ubicación central: Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en IamIP Patent Platform.

## <a name="tutorial-description"></a>Descripción del tutorial

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* IamIP Patent Platform admite el inicio de sesión único iniciado por SP e IDP.

## <a name="add-iamip-patent-platform-from-the-gallery"></a>Adición de IamIP Patent Platform desde la galería

Para configurar la integración de IamIP Patent Platform en Azure AD, deberá agregar IamIP Patent Platform desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta profesional o educativa o con una cuenta Microsoft personal.
1. En el panel izquierdo, seleccione **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **IamIP Patent Platform** en el cuadro de búsqueda.
1. Seleccione **IamIP Patent Platform** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-iamip-patent-platform"></a>Configuración y prueba del inicio de sesión único de Azure AD para IamIP Patent Platform

Configure y pruebe el inicio de sesión único de Azure AD con IamIP Patent Platform mediante un usuario de prueba llamado B.Simon. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario correspondiente de IamIP Patent Platform.

Para configurar y probar el inicio de sesión único de Azure AD con IamIP Patent Platform, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** para probar el inicio de sesión único de Azure AD.
    1. **[Concesión de acceso al usuario de prueba](#grant-access-to-the-test-user)** , para que el usuario pueda usar el inicio de sesión único de Azure AD.

1. **[Configuración del inicio de sesión único de IamIP Patent Platform](#configure-iamip-patent-platform-sso)** en la aplicación.
    1. **[Creación de un usuario de prueba de IamIP Patent Platform](#create-iamip-patent-platform-test-user)** como homólogo de la representación del usuario en Azure AD.

1. **[Prueba del inicio de sesión único](#test-sso)** para comprobar que la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal:

1. En Azure Portal, en la página de integración de la aplicación **IamIP Patent Platform**, en la sección **Administrar**, seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, seleccione el botón de lápiz para **Configuración básica de SAML** para editar la configuración:

   ![Botón de lápiz para Configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si tiene el archivo de metadatos del proveedor de servicios y quiere configurar el inicio de sesión único en modo iniciado por IDP, siga estos pasos:

    a. Seleccione **Cargar el archivo de metadatos**.

    ![Carga del archivo de metadatos](common/upload-metadata.png)

    b. Seleccione el botón de carpeta, seleccione el archivo de metadatos y, a continuación, seleccione **Cargar**.

    ![Botones de carpeta y de carga](common/browse-upload-metadata.png)

    c. Una vez que se haya cargado el archivo de metadatos, los valores de **Identificador** y **Dirección URL de respuesta** se rellenan automáticamente en la sección **Configuración básica de SAML**.

    > [!Note]
    > Si los valores de **Identificador** y **Dirección URL de respuesta** no se rellenan automáticamente, proporcione dichos valores manualmente según sus necesidades.

1. Si quiere configurar la aplicación en modo iniciado por SP, seleccione **Establecer direcciones URL adicionales** y lleve a cabo el paso siguiente:

    En el cuadro **URL de inicio de sesión**, escriba la dirección URL: `https://patents.iamip.com/login-user`.

1. En la página **Configurar inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione el vínculo **Descargar** para **Certificado (sin procesar)** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificateraw.png)

1. En la sección **Configurar IamIP Patent Platform**, copie las direcciones URL correspondientes según sus necesidades.

    ![Copiar direcciones URL de configuración](common/idp-intiated.png))

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, creará un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**. Seleccione **Usuarios** y, a continuación, seleccione **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades de **usuario**, realice estos pasos:
   1. En el cuadro **Nombre**, escriba **B.Simon**.  
   1. En el cuadro **Nombre de usuario**, escriba \<username>@\<companydomain>.\<extension>. Por ejemplo, `B.Simon@contoso.com`.
   1. Seleccione **Mostrar contraseña** y, a continuación, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Seleccione **Crear**.

### <a name="grant-access-to-the-test-user"></a>Concesión de acceso al usuario de prueba

En esta sección va a permitir que B.Simon acceda a IamIP Patent Platform mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales** y, a continuación, seleccione **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **IamIP Patent Platform**.
1. En la sección **Administrar** de la página de información general de la aplicación, seleccione **Usuarios y grupos**:
1. Seleccione **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** en la lista **Usuarios** y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-iamip-patent-platform-sso"></a>Configuración del inicio de sesión único en IamIP Patent Platform

Para configurar el inicio de sesión único en IamIP Patent Platform, es preciso enviar el certificado (sin procesar) descargado y las direcciones URL correspondientes que copió de Azure Portal al [equipo de soporte técnico de IamIP Patent Platform](mailto:info@iamip.com). Ellos configuran la conexión de inicio de sesión único de SAML de manera correcta en ambas partes.

### <a name="create-iamip-patent-platform-test-user"></a>Creación de un usuario de prueba en IamIP Patent Platform

Colabore con el [equipo de soporte técnico de IamIP Patent Platform](mailto:info@iamip.com) para agregar un usuario llamado B.Simon en IamIP Patent Platform. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la URL de inicio de sesión de IamIP Patent Platform, donde podrá poner en marcha el flujo de inicio de sesión.  

* Acceda directamente a la URL de inicio de sesión de IamIP Patent Platform y ponga en marcha el flujo de inicio de sesión desde ahí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Debería iniciarse la sesión automáticamente en la aplicación IamIP Patent Platform para la que configuró el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de IamIP Patent Platform en Aplicaciones, si se ha configurado el modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para poner en marcha el flujo de inicio de sesión y, si ha configurado el modo IDP, se debería iniciar sesión automáticamente en la aplicación IamIP Patent Platform para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado IamIP Patent Platform, puede aplicar controles de sesión, que protegen contra la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).