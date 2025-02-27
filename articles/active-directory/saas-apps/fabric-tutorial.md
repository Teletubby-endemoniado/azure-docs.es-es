---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Fabric | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Fabric.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/08/2021
ms.author: jeedes
ms.openlocfilehash: 950468ff48a0d384d46a04fa3b2a93b0419c2fbe
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132314149"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-fabric"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Fabric

En este tutorial, aprenderá a integrar Fabric con Azure Active Directory (Azure AD). Al integrar Fabric con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Fabric.
* Permitir que los usuarios inicien sesión automáticamente en Fabric con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para el inicio de sesión único (SSO) en Fabric.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Fabric admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-fabric-from-the-gallery"></a>Incorporación de Fabric desde la galería

Para configurar la integración de Fabric en Azure AD, será preciso que agregue Fabric desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Fabric** en el cuadro de búsqueda.
1. Seleccione **Fabric** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-sso-for-fabric"></a>Configuración y prueba del SSO de Azure AD para Fabric

Configure y pruebe el inicio de sesión único (SSO) de Azure AD con Fabric mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Fabric.

Para configurar y probar el inicio de sesión único de Azure AD con Fabric, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Fabric](#configure-fabric-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación del usuario de prueba de Fabric](#create-fabric-roles)** , para tener un homólogo de B.Simon en Fabric vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **Fabric**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:

   1. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente:  
      `https://<HOSTNAME>`

   1. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón:  
      `https://<HOSTNAME>:<PORT>/api/authenticate`
    
   1. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:   
      `https://<HOSTNAME>:<PORT>`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y las direcciones URL de inicio de sesión y de respuesta reales. Póngase en contacto con el equipo de COE de K2View para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Fabric**, copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

1. En la sección **Cifrado de tokens**, seleccione **Importar certificado** y cargue el archivo de certificado de Fabric. Póngase en contacto con el equipo de COE de K2View para obtenerlo.

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

En esta sección, va a permitir que B.Simon utilice el inicio de sesión único de Azure, ya que le concederá acceso a Fabric.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Fabric**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-fabric-sso"></a>Configuración de inicio de sesión único en Fabric

Para configurar el inicio de sesión único en **Fabric**, envíe el **Certificado (Base64)** descargado y las direcciones URL correspondientes copiadas de Azure Portal al equipo de soporte técnico de COE de K2View. El equipo configurará el valor para establecer la conexión de SSO de SAML correctamente en ambos lados.

Para más información, consulte *Configuración de SAML de Fabric* y *Guía de configuración de SAML de Azure AD* en la [base de conocimiento de K2view](https://support.k2view.com/knowledge-base.html).

### <a name="create-fabric-roles"></a>Creación de roles de Fabric

Trabaje con el equipo de soporte técnico de COE de K2View para establecer roles de Fabric que coincidan con los grupos de Azure AD y que sean pertinentes para los usuarios que van a usar Fabric. Proporcionará al equipo de Fabric los id. de grupo, ya que se envían en la respuesta de SAML.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* En Azure Portal, seleccione **Probar esta aplicación**. Esta acción le redirigirá a la dirección URL de inicio de sesión de Fabric, donde puede comenzar el flujo de inicio de sesión. 

* Acceda directamente a la dirección URL de inicio de sesión de Fabric y comience el flujo de inicio de sesión desde ahí.

* Puede usar Mis aplicaciones de Microsoft. Al seleccionar el icono de **Fabric** en el portal Mis aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de Fabric. Para más información acerca del portal Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).


## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Fabric, puede aplicar el control de sesión, que protege a su organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-any-app).
