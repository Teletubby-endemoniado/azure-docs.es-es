---
title: 'Tutorial: Integración de Azure Active Directory con Beeline | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Beeline.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/15/2021
ms.author: jeedes
ms.openlocfilehash: ff9611559467f24d1b01d4e628bdcf9642cca88a
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132336152"
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a>Tutorial: Integración de Azure Active Directory con Beeline

En este tutorial, obtendrá información sobre cómo integrar Beeline con Azure Active Directory (Azure AD). Al integrar Beeline con Azure AD, puede hacer lo siguiente:

* Controlar quién tiene acceso a Beeline en Azure AD.
* Permitir que los usuarios inicien sesión automáticamente en Beeline con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para el inicio de sesión único (SSO) en Beeline.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Beeline solo admite el inicio de sesión único iniciado por **IDP**.

## <a name="add-beeline-from-the-gallery"></a>Agregar BeeLine desde la galería

Para configurar la integración de Beeline en Azure AD, deberá agregar Beeline desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Beeline** en el cuadro de búsqueda.
1. Seleccione **Beeline** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-beeline"></a>Configuración y prueba del SSO de Azure AD para Beeline

Configure y pruebe el inicio de sesión único de Azure AD con Beeline mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Beeline.

Para configurar y probar el inicio de sesión único de Azure AD con Beeline, haga lo siguiente:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Beeline](#configure-beeline-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Beeline](#create-beeline-test-user)** , para tener un homólogo de B.Simon en Beeline que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación de **Beeline**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la página **Configurar inicio de sesión único con SAML** realice los siguientes pasos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://projects.beeline.com/<ProjInstance_Name>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://projects.beeline.com/<ProjInstance_Name>/SSO_External.ashx`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de Beeline](https://www.beeline.com/contact-support/) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. La aplicación Beeline espera las aserciones de SAML en un formato específico. Trabaje primero con el [equipo de soporte técnico de Beeline](https://www.beeline.com/contact-support/) para determinar el identificador de usuario correcto que se asignará a la aplicación. Siga también las indicaciones del [equipo de soporte técnico de Beeline](https://www.beeline.com/contact-support/) en relación con el atributo que desean usar para esta asignación. Puede administrar el valor de este atributo desde la pestaña **User Attributes** (Atributos de usuario) de la aplicación. La siguiente captura de pantalla le muestra un ejemplo de esto. Aquí hemos asignado la notificación **Id. de usuario** con el atributo **userprincipalname**, que proporciona el identificador de usuario único que se enviará a la aplicación Beeline en cada respuesta SAML correcta.

    ![imagen](common/edit-attribute.png)

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

7. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Beeline**, seleccione **Propiedades** y copie la URL de acceso de usuario.

    ![Copiar la URL de acceso de usuario](media/beeline-tutorial/client-access-url.png)

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

En esta sección, va a permitir que B.Simon acceda a Beeline mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Beeline**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-beeline-sso"></a>Configuración del inicio de sesión único de Beeline

Para configurar el inicio de sesión único en **Beeline**, tiene que enviar el **XML de metadatos de federación** descargado y la dirección URL de acceso de usuario de las propiedades de Azure Portal al [equipo de soporte técnico de Beeline](https://www.beeline.com/contact-support/). Necesitan los metadatos y la dirección URL de acceso de usuario para que la conexión de inicio de sesión único de SAML se configure correctamente en ambos sitios.

### <a name="create-beeline-test-user"></a>Creación de un usuario de prueba de Beeline

En esta sección, creará un usuario llamado Britta Simon en Beeline. La aplicación Beeline requiere que todos los usuarios se aprovisionen en la aplicación antes de efectuar el inicio de sesión único. Trabaje con el [servicio de soporte técnico de Beeline](https://www.beeline.com/contact-support/) para aprovisionar todos estos usuarios en la aplicación.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones.

* En Azure Portal, haga clic en "Probar esta aplicación". Al hacerlo, debería iniciar sesión automáticamente en la instancia de Beeline en la que configuró el inicio de sesión único.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Beeline en Aplicaciones, debería iniciar sesión automáticamente en la versión de Beeline para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Beeline, puede aplicar el control de sesión, que protege a la organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
