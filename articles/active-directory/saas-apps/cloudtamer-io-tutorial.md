---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con cloudtamer.io | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y cloudtamer.io.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/26/2021
ms.author: jeedes
ms.openlocfilehash: 329121332449316f3582ee38c11ad843b25ab812
ms.sourcegitcommit: 61f87d27e05547f3c22044c6aa42be8f23673256
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2021
ms.locfileid: "132054132"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cloudtamerio"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con cloudtamer.io

En este tutorial aprenderá a integrar cloudtamer.io con Azure Active Directory (Azure AD). Al integrar cloudtamer.io con Azure AD, puede hacer lo siguiente:

* Puede controlar en Azure AD quién tiene acceso a cloudtamer.io.
* Permitir que los usuarios puedan iniciar sesión automáticamente en cloudtamer.io con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) de cloudtamer.io.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* cloudtamer.io admite el SSO iniciado por **SP e IDP**.
* cloudtamer.io admite el aprovisionamiento de usuarios **Just-In-Time**.


## <a name="adding-cloudtamerio-from-the-gallery"></a>Adición de cloudtamer.io desde la galería

Para configurar la integración de cloudtamer.io en Azure AD, será preciso que agregue cloudtamer.io desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **cloudtamer.io** en el cuadro de búsqueda.
1. Seleccione **cloudtamer.io** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-sso-for-cloudtamerio"></a>Configuración y prueba del inicio de sesión único de Azure AD para cloudtamer.io

Configure y pruebe el inicio de sesión único de Azure AD con cloudtamer.io mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de cloudtamer.io.

Para configurar y probar el SSO de Azure AD con cloudtamer.io, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en cloudtamer.io](#configure-cloudtamerio-sso)** : para configurar los valores de inicio de sesión único en el lado aplicación.
    1. **[Creación de un usuario de prueba de cloudtamer.io](#create-cloudtamerio-test-user)** : para tener un homólogo de B.Simon en cloudtamer.io que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.
1. **[Aserciones de grupo](#group-assertions)** : para establecer aserciones de grupo para Azure AD y cloudtamer.io.

### <a name="begin-cloudtamerio-sso-configuration"></a>Inicio de la configuración del inicio de sesión único en cloudtamer.io

1. Inicie sesión en el sitio web de cloudtamer.io como administrador.

1. Haga clic en **+** (icono más) de la parte superior derecha y seleccione **IDMS**.

    ![Captura de pantalla de la creación de IDMS.](./media/cloudtamer-io-tutorial/idms-creation.png)

1. Seleccione **SAML 2.0** como el tipo de IDMS.

1. Deje esta pantalla abierta y copie los valores de esta pantalla en la configuración de Azure AD.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **cloudtamer.io**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, pegue el valor de **SERVICE PROVIDER ISSUER (ENTITY ID)** (EMISOR DE PROVEEDOR DE SERVICIO [ID DE ENTIDAD]) de cloudtamer.io en este cuadro.

    b. En el cuadro de texto **URL de respuesta**, pegue el valor de **SERVICE PROVIDER ACS URL** (URL DEL ACS DEL PROVEEDOR DE SERVICIO) de cloudtamer.io en este cuadro.

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, pegue el valor de **SERVICE PROVIDER ACS URL** (URL DEL ACS DEL PROVEEDOR DE SERVICIO) de cloudtamer.io en este cuadro.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Configuración de cloudtamer.io**, copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon use el inicio de sesión único de Azure concediéndole acceso a cloudtamer.io.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **cloudtamer.io**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-cloudtamerio-sso"></a>Configuración del inicio de sesión único de cloudtamer.io

1. En la página **Add IDMS** (Agregar IDMS), realice los pasos siguientes:

    ![Captura de pantalla de la adición de IDMS.](./media/cloudtamer-io-tutorial/configuration.png)

    a. En **IDMS Name** (Nombre de IDMS) asigne un nombre que los usuarios reconocerán en la pantalla Inicio de sesión.

    b. En el cuadro de texto **IDENTITY PROVIDER ISSUER (ENTITY ID)** (EMISOR DE PROVEEDOR DE IDENTIDADES [ID DE ENTIDAD]), pegue el valor de **Identificador** que ha copiado en Azure Portal.

    c. Abra el archivo **XML de metadatos de federación** descargado de Azure Portal en el Bloc de notas y pegue el contenido en el cuadro de texto **IDENTITY PROVIDER METADATA** (METADATOS DEL PROVEEDOR DE IDENTIDADES).

    d. Copie el valor de **IDENTITY PROVIDER ISSUER (ENTITY ID)** (EMISOR DE PROVEEDOR DE IDENTIDADES [ID DE ENTIDAD]) y péguelo en el cuadro de texto **Identificador** de la sección Configuración básica de SAML en Azure Portal.

    e. Copie el valor de **SERVICE PROVIDER ACS URL** (URL DEL ACS DEL PROVEEDOR DE SERVICIO) y péguelo en el cuadro de texto **URL de respuesta** de la sección Configuración básica de SAML de Azure Portal.

    f. En la asignación de aserciones, escriba los valores siguientes:

    | Campo | Value |
    |-----------|-------|
    | Nombre | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |
    | Apellido | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |
    | Correo electrónico | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` |
    |  Nombre de usuario | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` |
    |

1. Haga clic en **Create IDMS** (Crear IDMS).


### <a name="create-cloudtamerio-test-user"></a>Creación de un usuario de prueba de cloudtamer.io

En esta sección, se crea un usuario llamado Britta Simon en cloudtamer.io. cloudtamer.io admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe aún en cloudtamer.io, se crea uno después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de cloudtamer.io, donde podrá comenzar el flujo de inicio de sesión.  

* Vaya directamente a la dirección URL de inicio de sesión de cloudtamer.io y comience el flujo de inicio de sesión desde ahí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de cloudtamer.io para la que configuró el inicio de sesión único. 

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de cloudtamer.io en Aplicaciones, si se ha configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión; y si se ha configurado en modo IDP, debería iniciar sesión automáticamente en la instancia de cloudtamer.io para la que configuró el SSO. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="group-assertions"></a>Aserciones de grupo

Para administrar fácilmente los permisos de usuario de cloudtamer.io mediante grupos de Azure Active Directory existentes, siga estos pasos:

### <a name="azure-ad-configuration"></a>Configuración de Azure AD

1. En Azure Portal, vaya a **Azure Active Directory** > **Aplicaciones empresariales**.
1. En la lista, seleccione la aplicación empresarial para cloudtamer.io.
1. En **Información general**, en el menú de la izquierda, seleccione **Inicio de sesión único**.
1. En **Inicio de sesión único**, en **Atributos y notificaciones de usuario**, seleccione **Editar**.
1. Seleccione **Agregar una notificación de grupo**. 
   > [!NOTE]
   > Solo puede tener una notificación de grupo. Si esta opción está deshabilitada, es posible que ya tenga definida una notificación de grupo.
1. En **Notificaciones de grupo**, seleccione los grupos que se deben devolver en la notificación:
   - Si siempre tiene asignados todos los grupos que va a usar en cloudtamer.io a esta aplicación empresarial, seleccione **Grupos asignados a la aplicación**.
   - Si desea que aparezcan todos los grupos (esta selección puede provocar un gran número de aserciones de grupo y puede estar sujeta a límites), seleccione **Grupos asignados a la aplicación**.
1. En **Atributo de origen**, deje el **identificador de grupo** predeterminado.
1. Seleccione la casilla **Personalizar el nombre de la notificación del grupo**.
1. En **Nombre**, escriba **memberOf**.
1. Seleccione **Guardar** para completar la configuración en Azure AD.

### <a name="cloudtamerio-configuration"></a>Configuración de cloudtamer.io

1. En cloudtamer.io, vaya a **Users** > **Identity Management Systems** (Usuarios > Sistemas de administración de identidades).
1. Seleccione el IDMS que ha creado para Azure AD.
1. En la página de información general, seleccione la pestaña **User Group Associations** (Asociaciones de grupos de usuarios).
1. Para cada asignación de grupo de usuarios que desee, complete estos pasos:
   1. Seleccione **Add** > **Add New** (Agregar > Agregar nuevo).
   1. En el cuadro de diálogo que aparece:
      1. En **Name** (Nombre), escriba **memberOf**.
      1. En **Regex**, escriba el identificador de objeto (de Azure AD) del grupo que desea buscar.
      1. En **User Group** (Grupo de usuarios), seleccione el grupo interno de cloudtamer.io que desea asignar al grupo en **Regex**.
      1. Active la casilla **Update on Login** (Actualizar al iniciar sesión).
   1. Seleccione **Add** (Agregar) para agregar la asociación de grupo.

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado cloudtamer.io, puede aplicar el control de sesión, que protege tanto la filtración como la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).
