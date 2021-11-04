---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con WEDO | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y WEDO.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/02/2021
ms.author: jeedes
ms.openlocfilehash: 4cf995376ad9b7b46acc85d0aa03c4079bf8ae3d
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131067047"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-wedo"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con WEDO

En este tutorial, aprenderá a integrar WEDO con Azure Active Directory (Azure AD). Al integrar WEDO con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a WEDO.
* Permitir que los usuarios inicien sesión automáticamente en WEDO con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en WEDO. Póngase en contacto con el [equipo de soporte técnico al cliente de WEDO](mailto:info@wedo.swiss) para obtener esta suscripción.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* WEDO admite el inicio de sesión único iniciado por **SP e IDP**.
* WEFO admite el [aprovisionamiento automatizado de usuarios](wedo-provisioning-tutorial.md).

## <a name="add-wedo-from-the-gallery"></a>Incorporación de WEDO desde la galería

Para configurar la integración de WEDO en Azure AD, deberá agregar WEDO desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **WEDO** en el cuadro de búsqueda.
1. Seleccione **WEDO** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-wedo"></a>Configuración y prueba del inicio de sesión único de Azure AD para WEDO

Configure y pruebe el inicio de sesión único de Azure AD con WEDO mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de WEDO.

Para configurar y probar el inicio de sesión único de Azure AD con WEDO, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de WEDO](#configure-wedo-sso)**, para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en WEDO](#create-wedo-test-user)**, para tener un homólogo de B.Simon en WEDO que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **WEDO**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<SUBDOMAIN>.wedo.swiss/sp/acs`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.wedo.swiss/sp/acs`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.wedo.swiss/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico al cliente de WEDO](mailto:info@wedo.swiss) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación WEDO espera las aserciones de SAML en un formato específico, lo que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    | Nombre | Atributo de origen|
    | ------------ | --------- |
    | email | user.email |
    | firstName | user.firstName |
    | lastName | user.lasttName |
    | userName | user.userName |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Configurar WEDO**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon acceda a WEDO mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **WEDO**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-wedo-sso"></a>Configuración del inicio de sesión único de WEDO

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en WEDO.

1. Inicie sesión en [WEDO](https://login.wedo.swiss/). Debe tener **rol de administrador**.
1. En la configuración del perfil, seleccione el menú **Authentication** (Autenticación) en la sección **Network settings** (Configuración de red).
1. En la página **SAML Authentication** (Autenticación SAML), realice los siguientes pasos:

   ![Vínculo de autenticación SAML](media/wedo-tutorial/network-security-authentification.png)

   a. Active la opción **Enable SAML Authentication** (Habilitar autenticación SAML).

   b. Seleccione la pestaña **Identity Provider metadata (XML)** (Metadatos del proveedor de identidades [XML]).

   c. Abra el archivo **XML de metadatos de federación** descargado de Azure Portal en el Bloc de notas, copie el contenido del XML de metadatos y péguelo en el cuadro de texto **X.509 Certificate** (Certificado X.509).

   d. Haga clic en **Save**(Guardar).

### <a name="create-wedo-test-user"></a>Creación de un usuario de prueba en WEDO

En esta sección, va a crear un usuario de prueba llamado Bob Simon en WEDO. La información debe coincidir con la indicada en **Creación de un usuario de prueba de Azure AD**.

1. En la configuración del perfil en WEDO, seleccione **Users** (Usuarios) en la sección **Network settings** (Configuración de red).
1. Haga clic en **Agregar usuario**.
1. En el menú emergente Agregar usuario, rellene la información del usuario

    a. First name (Nombre): `B`.

    b. Last name (Apellidos): `Simon`.

    c. Escriba la dirección de correo electrónico `username@companydomain.extension`. Por ejemplo, `B.Simon@contoso.com`. Es obligatorio tener un correo electrónico con el mismo dominio que el nombre corto de la empresa.

    d. User type (Tipo de usuario): `User`.

    e. Haga clic en **Crear usuario**.

    f. En la página **Select teams** (Seleccionar equipos), haga clic en **Save** (Guardar).

    g.  En la página **Invite user** (Invitar usuario), haga clic en **Yes** (Sí).

1. Valide el usuario mediante el vínculo recibido por correo electrónico.

> [!NOTE]
> Si desea crear un usuario falso (el correo electrónico anterior no existe en la red), póngase en contacto con [nuestro equipo de soporte técnico](mailto:info@wedo.swiss) para validar al usuario*.

> [!NOTE]
> WEDO también admite el aprovisionamiento automático de usuarios. [Aquí](./wedo-provisioning-tutorial.md) puede encontrar más detalles sobre cómo configurar el aprovisionamiento automático de usuarios.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. De este modo, accederá automáticamente a la dirección URL de inicio de sesión de WEDO, donde podrá iniciar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de WEDO e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Debería iniciar sesión automáticamente en la instancia de WEDO para la que ha configurado el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de WEDO en Aplicaciones, si se ha configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para iniciar el flujo de inicio de sesión; y si se ha configurado en modo IDP, se debería iniciar sesión automáticamente en la instancia de WEDO para la que ha configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez que configure WEDO, puede aplicar el control de sesión, que protege la filtración y la infiltración de los datos confidenciales de su organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).
