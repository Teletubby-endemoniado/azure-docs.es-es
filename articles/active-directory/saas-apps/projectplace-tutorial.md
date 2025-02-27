---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con Projectplace'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Projectplace.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/13/2021
ms.author: jeedes
ms.openlocfilehash: ea2cfac86a601305cd63414582721b6b6df864bb
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132333891"
---
# <a name="tutorial-azure-ad-sso-integration-with-projectplace"></a>Tutorial: Integración del inicio de sesión único de Azure AD con Projectplace

En este tutorial aprenderá a integrar Projectplace con Azure Active Directory (Azure AD). Cuando integre Projectplace con Azure AD, podrá hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Projectplace
* Permitir que los usuarios inicien sesión automáticamente en Projectplace con sus cuentas de Azure AD
* Administrar las cuentas desde una ubicación central (Azure Portal).
* Los usuarios se pueden aprovisionar automáticamente en Projectplace.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Projectplace.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Projectplace admite el SSO iniciado por **SP e IDP**, así como el aprovisionamiento de usuarios **Just In Time**.

## <a name="add-projectplace-from-the-gallery"></a>Incorporación de Projectplace desde la galería

Para configurar la integración de Projectplace en Azure AD, es preciso agregar esta solución desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Projectplace** en el cuadro de búsqueda.
1. Seleccione **Projectplace** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-projectplace"></a>Configuración y prueba del inicio de sesión único de Azure AD para Projectplace

Configure y pruebe el inicio de sesión único de Azure AD con Projectplace con un usuario de prueba llamado **B.Simon**. Para que el SSO funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Projectplace.

Para configurar y probar el inicio de sesión único de Azure AD con Projectplace, complete los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
   1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
   1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Projectplace](#configure-projectplace-sso)** , para configurar los valores de inicio de sesión único en la aplicación.
   1. **[Creación de un usuario de prueba de Projectplace](#create-projectplace-test-user)** : el objetivo es tener un homólogo de B.Simon en Projectplace que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **Projectplace**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si quiere configurar la aplicación en el modo iniciado por **IDP**, la aplicación está preconfigurado y las direcciones URL necesarias ya se hayan rellenado previamente con Azure. El usuario debe guardar la configuración, para lo que debe hacer clic en el botón **Guardar**.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://service.projectplace.com`

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el icono de **copia** para copiar la **dirección URL de metadatos de federación de aplicación** según sus requisitos y guárdela en el Bloc de notas.

   ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

1. En la sección **Set up Projectplace** (Configurar Projectplace), copie las direcciones URL adecuadas según sus necesidades.

   ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B. Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B. Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `BrittaSimon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B. Simon acceda a Projectplace mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Projectplace**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B. Simon** en la lista de usuarios y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-projectplace-sso"></a>Configuración del inicio de sesión único de Projectplace

Para configurar el inicio de sesión único en **Projectplace**, deberá enviar la **dirección URL de metadatos de federación de aplicación** desde Azure Portal al [equipo de soporte técnico de Projectplace](https://success.planview.com/Projectplace/Support). El equipo se asegurará de que la conexión de inicio de sesión único de SAML esté establecida correctamente en ambos lados.

>[!NOTE]
>La configuración del inicio de sesión único la debe realizar el [equipo de soporte técnico de Projectplace](https://success.planview.com/Projectplace/Support). Tan pronto como se complete la configuración, recibirá una notificación. 

### <a name="create-projectplace-test-user"></a>Creación de un usuario de prueba de Projectplace

>[!NOTE]
>Puede omitir este paso si tiene habilitado el aprovisionamiento en Projectplace. Puede pedir al [equipo de soporte técnico de Projectplace](https://success.planview.com/Projectplace/Support) que habilite el aprovisionamiento; una vez hecho, se crean los usuarios en Projectplace durante el primer inicio de sesión.

Para permitir que los usuarios de Azure AD inicien sesión en Projectplace, debe agregarlos a este. Y los debe agregar manualmente.

**Para crear una cuenta de usuario, siga estos pasos:**

1. Inicie sesión en el sitio de la compañía **Projectplace** como administrador.

2. Vaya a **People** (Personas) y haga clic en **Members** (Miembros):
   
    ![Vaya a People (Personas) y seleccione Members (Miembros)](./media/projectplace-tutorial/members.png "Personas")

3. Seleccione **Add Member** (Agregar miembro):
   
    ![Seleccione Add Member](./media/projectplace-tutorial/issues.png "Agregar miembros") (Agregar miembro)

4. En la sección **Add Member** (Agregar miembro), realice los siguientes pasos.
   
    ![Sección Add Member (Agregar miembro)](./media/projectplace-tutorial/account.png "Nuevos miembros")
   
    1. En el cuadro **New Members** (Nuevos miembros), escriba la dirección de correo electrónico de una cuenta de Azure AD válida que quiera agregar.
   
    1. Seleccione **Enviar**.

   Se enviará un mensaje de correo electrónico con un vínculo para confirmar la cuenta antes de que se active al titular de la cuenta de Azure AD.

>[!NOTE]
>Puede usar también cualquier otra API o herramienta de creación de cuentas de usuario proporcionada por Projectplace para configurar cuentas de usuario de Azure AD.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de Projectplace, donde puede iniciar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de Projectplace e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de Projectplace para la que configuró el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Projectplace en Aplicaciones, si se ha configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión; y si se ha configurado en modo IDP, se debería iniciar sesión automáticamente en la instancia de Projectplace para la que ha configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado Projectplace, puede aplicar el control de sesión, que protege su organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
