---
title: 'Tutorial: Integración de Azure Active Directory con SpringCM | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SpringCM.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/24/2021
ms.author: jeedes
ms.openlocfilehash: b75de6a5495372a7d79b452b12d79a2612e64f6c
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132282811"
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>Tutorial: Integración de Azure Active Directory con SpringCM

En este tutorial, obtendrá información sobre cómo integrar SpringCM con Azure Active Directory (Azure AD). Al integrar SpringCM con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a SpringCM.
* Permitir que los usuarios inicien sesión automáticamente en SpringCM con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SpringCM, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único en SpringCM.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SpringCM admite el SSO iniciado por **SP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-springcm-from-the-gallery"></a>Incorporación de SpringCM desde la galería

Para configurar la integración de SpringCM en Azure AD, deberá agregar SpringCM desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **SpringCM** en el cuadro de búsqueda.
1. Seleccione **SpringCM** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-springcm"></a>Configuración y prueba del SSO de Azure AD para SpringCM

Configure y pruebe el SSO de Azure AD con SpringCM mediante un usuario de prueba llamado **B.Simon**. Para que el SSO funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de SpringCM.

Para configurar y probar el SSO de Azure AD con SpringCM, lleve a cabo los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del SSO en SpringCM](#configure-springcm-sso)** : para definir los valores de inicio de sesión único en la aplicación.
    1. **[Creación del usuario de prueba en SpringCM](#create-springcm-test-user)** : para tener un homólogo de B.Simon en SpringCM vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **SpringCM**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<IDENTIFIER>`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico para clientes de SpringCM](https://knowledge.springcm.com/support) para obtener el valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (sin procesar)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificateraw.png)

6. En la sección **Set up SpringCM** (Configurar SpringCM), copie las direcciones URL que necesite.

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

En esta sección, va a permitir que B.Simon acceda a SpringCM mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **SpringCM**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-springcm-sso"></a>Configuración del SSO de SpringCM

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de **SpringCM** como administrador.

1. En el menú de la parte superior, haga clic en **IR A**, haga clic en **Preferencias** y, luego, en la sección **Preferencias de cuenta**, haga clic en **SSO de SAML**.

    ![SSO de SAML](./media/spring-cm-tutorial/preferences.png "SSO de SAML")

1. En la sección de configuración del proveedor de identidades, realice los pasos siguientes:

    ![Configuración del proveedor de identidades](./media/spring-cm-tutorial/configuration.png "Configuración del proveedor de identidades")

    a. Para cargar el certificado descargado de Azure Active Directory, haga clic en **Seleccionar certificado de emisor** o **Cambiar el certificado del emisor**.

    b. En el cuadro de texto **Emisor**, pegue el valor del **Identificador de Azure AD** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Extremo iniciado por el proveedor de servicios**, peque el valor de **Dirección URL de inicio de sesión** que copió de Azure Portal.

    d. Seleccione **SAML Enabled** (SAML habilitado) como **Enable** (Habilitar).

    e. Haga clic en **Save**(Guardar).

### <a name="create-springcm-test-user"></a>Creación del usuario de prueba en SpringCM

Para permitir que los usuarios de Azure Active Directory inicien sesión en SpringCM, deben aprovisionarse en SpringCM. En el caso de SpringCM, el aprovisionamiento es una tarea manual.

> [!NOTE]
> Para obtener más información, consulte [Create and Edit a SpringCM User](http://community.springcm.com/s/article/Create-and-Edit-a-SpringCM-User-1619481053) (Creación y edición de un usuario de SpringCM). 

**Para aprovisionar cuentas de usuario en SpringCM, realice los siguientes pasos:**

1. Inicie sesión en su sitio de compañía **SpringCM** como administrador.

1. Haga clic en **GOTO** y, luego, en **ADDRESS BOOK** (LIBRETA DE DIRECCIONES).

    ![Crear usuario](./media/spring-cm-tutorial/user.png "Crear usuario")

1. Haga clic en **Create User**(Crear usuario).

1. Seleccione un **Rol de usuario**.

1. Seleccione **Enviar correo electrónico de activación**.

1. Escriba el nombre, el apellido y la dirección de correo electrónico de una cuenta de usuario de Azure Active Directory válida que quiera aprovisionar en los cuadros de texto relacionados.

1. Agregue el usuario a un **Grupo de seguridad**.

1. Haga clic en **Save**(Guardar).

   > [!NOTE]
   > Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de SpringCM ofrecida por SpringCM para aprovisionar cuentas de usuario de Azure AD.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de SpringCM, donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de SpringCM e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de SpringCM en Aplicaciones, se le redirigirá a la URL de inicio de sesión de esa plataforma. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado SpringCM, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender for Cloud Apps](/cloud-app-security/proxy-deployment-aad).
