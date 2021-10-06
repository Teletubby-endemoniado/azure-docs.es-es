---
title: 'Tutorial: Integración de Azure Active Directory con Signagelive | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Signagelive.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/01/2021
ms.author: jeedes
ms.openlocfilehash: 4b139069227095140b965fa817eead890adcf4d3
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124808370"
---
# <a name="tutorial-azure-active-directory-integration-with-signagelive"></a>Tutorial: Integración de Azure Active Directory con Signagelive

En este tutorial, aprenderá a integrar Signagelive con Azure Active Directory (Azure AD). La integración de Signagelive con Azure AD permite:

* Controlar en Azure AD quién tiene acceso a Signagelive.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Signagelive con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en Signagelive.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Signagelive admite el inicio de sesión único iniciado por SP.
* Signagelive admite el [aprovisionamiento automático de usuarios](signagelive-provisioning-tutorial.md).

## <a name="add-signagelive-from-the-gallery"></a>Adición de Signagelive desde la galería

Para configurar la integración de Signagelive en Azure AD, deberá agregar Signagelive desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Signagelive** en el cuadro de búsqueda.
1. Seleccione **Signagelive** en el panel de resultados y, después, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-signagelive"></a>Configuración y prueba del inicio de sesión único de Azure AD para Signagelive

Utilice un usuario de prueba llamado **B.Simon** para configurar y probar el inicio de sesión único de Azure AD con Signagelive mediante. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Signagelive.

Para configurar y probar el inicio de sesión único de Azure AD con Signagelive, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)**, para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Signagelive](#configure-signagelive-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en Signagelive](#create-signagelive-test-user)** : para tener un homólogo de B. Simon en Signagelive que esté vinculado a la representación de ella en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Signagelive**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga este paso:

    En el cuadro **URL de inicio de sesión**, escriba una dirección URL con el siguiente formato: `https://login.signagelive.com/sso/<ORGANIZATIONALUNITNAME>`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Para obtener este valor, póngase en contacto con el [equipo de soporte técnico de Signagelive](mailto:support@signagelive.com). También puede hacer referencia a los patrones de la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione **Descargar** para descargar el **certificado (sin procesar)** de las opciones proporcionadas según sus requisitos. Luego, guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificateraw.png)

6. En la sección **Set up Signagelive** (Configurar Signagelive), copie las direcciones URL que necesite.

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

En esta sección, va a permitir que B. Simon use el inicio de sesión único de Azure, para lo que le concederá acceso a Signagelive.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Signagelive**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-signagelive-sso"></a>Configuración del inicio de sesión único en Signagelive

Para configurar el inicio de sesión único en Signagelive, envíe el **certificado (sin procesar)** descargado y las direcciones URL copiadas de Azure Portal al [equipo de soporte técnico de Signagelive](mailto:support@signagelive.com). Estas garantizan que la conexión de inicio de sesión único de SAML está establecida correctamente en ambos lados.

### <a name="create-signagelive-test-user"></a>Creación de un usuario de prueba de Signagelive

En esta sección, creará un usuario llamado Britta Simon en Signagelive. Trabaje con el [equipo de soporte técnico de Signagelive](mailto:support@signagelive.com) para agregar los usuarios a la plataforma de Signagelive. Los usuarios deben crearse y activarse antes de usar el inicio de sesión único.

Signagelive también admite el aprovisionamiento automático de usuarios. [Aquí](./signagelive-provisioning-tutorial.md) puede encontrar más detalles sobre cómo configurar el aprovisionamiento automático de usuarios.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de Signagelive, donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de Signagelive e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Signagelive que se encuentra en Aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de dicha aplicación. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Signagelive, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).