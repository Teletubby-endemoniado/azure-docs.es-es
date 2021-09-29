---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Catchpoint'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Catchpoint.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/16/2020
ms.author: jeedes
ms.openlocfilehash: 9847543eaa79b76301229b24f93cc5683b965c5b
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124765891"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-catchpoint"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Catchpoint

En este tutorial aprenderá a integrar Catchpoint con Azure Active Directory (Azure AD). Al integrar Catchpoint con Azure AD, puede hacer lo siguiente:

- Controlar el acceso de los usuarios a Catchpoint desde Azure AD.
- Habilitar el inicio de sesión automático de Catchpoint para los usuarios con cuentas de Azure AD.
- Administrar sus cuentas en una ubicación central: Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

- Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
- Una suscripción de Catchpoint con el inicio de sesión único (SSO) habilitado.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

- Catchpoint admite el inicio de sesión único iniciado por SP e IDP.
- Catchpoint admite el aprovisionamiento de usuarios Just-In-Time (JIT).

## <a name="add-catchpoint-from-the-gallery"></a>Incorporación de Catchpoint desde la galería

Para configurar la integración de Catchpoint en Azure AD, agregue Catchpoint a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta de Microsoft profesional, educativa o personal.
1. En el panel izquierdo, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Catchpoint** en el cuadro de búsqueda.
1. Seleccione **Catchpoint** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-catchpoint"></a>Configuración y prueba del inicio de sesión único de Azure AD para Catchpoint

Para que el inicio de sesión único funcione, debe vincular un usuario de Azure AD con un usuario en Catchpoint. En este tutorial, configuraremos un usuario de prueba llamado **B.Simon**.

Lleve a cabo las secciones siguientes:

1. [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso), para que los usuarios puedan utilizar esta característica.
   - [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con B.Simon.
   - [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para que B.Simon pueda usar el inicio de sesión único de Azure AD.
1. [Configuración del inicio de sesión único de Catchpoint](#configure-catchpoint-sso), para configurar los valores de inicio de sesión único en la aplicación.
   - [Creación de un usuario de prueba de Catchpoint](#create-a-catchpoint-test-user), para permitir la vinculación de la cuenta de prueba de B.Simon en Azure AD a una cuenta de usuario similar en Catchpoint.
1. [Prueba del inicio de sesión único](#test-sso), para comprobar que la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos en Azure Portal para habilitar el inicio de sesión único de Azure AD:

1. Inicie sesión en Azure Portal.
1. En la página de integración de aplicaciones de **Catchpoint**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, seleccione el icono con forma de lápiz para editar los valores de **Configuración básica de SAML**.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. Configure el modo de inicio para Catchpoint:

   - En el modo iniciado por **IDP**, escriba los valores de los siguientes campos:
     - En **Identificador**: `https://portal.catchpoint.com/SAML2`
     - En **URL de respuesta**: `https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`
   - En el modo Iniciado por **SP**, seleccione **Establecer direcciones URL adicionales** y escriba el siguiente valor:
     - En **URL de inicio de sesión**: `https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`

1. La aplicación Catchpoint espera las aserciones de SAML en un formato concreto. Agregue asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. En la tabla siguiente se incluye la lista de atributos predeterminados:

   | Nombre                   | Atributo de origen       |
   | ---------------------- | ---------------------- |
   | Nombre de pila              | user.givenneame        |
   | Surname                | user.surname           |
   | Dirección de correo electrónico           | user.mail              |
   | Nombre                   | user.userprincipalname |
   | Identificador de usuario único | user.userprincipalname |

   ![Captura de pantalla de la sección Atributos y reclamaciones del usuario](common/default-attributes.png)

1. Además, la aplicación Catchpoint espera que se pase otro atributo en una respuesta SAML. Consulte la tabla siguiente. Este atributo también se rellena previamente, pero puede revisarlo y actualizarlo para que se ajuste a sus necesidades.

   | Nombre      | Atributo de origen  |
   | --------- | ----------------- |
   | espacio de nombres | user.assignedrole |

   > [!NOTE]
   > La notificación `namespace` debe asignarse con el nombre de cuenta. Este nombre de cuenta debe configurarse con un rol de Azure AD para que se devuelva en la respuesta de SAML. Para más información sobre los roles en Azure AD, consulte [Configuración de la notificación de rol emitida en el token SAML para aplicaciones empresariales](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui).

1. Vaya a la página **Configurar el inicio de sesión único con SAML**. En la sección **Certificado de firma de SAML**, localice **Certificado (Base 64)** . Seleccione **Descargar** para guardar el certificado en el equipo.

   ![El vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Catchpoint**, copie las direcciones URL que necesitará en un paso posterior.

   ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba de Azure AD llamado B.Simon en Azure Portal.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, escriba `B.Simon@contoso.com`:
   1. Active la casilla de verificación **Mostrar contraseña**. Anote el valor de contraseña que se muestra.
   1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, va a permitir que B.Simon acceda a Catchpoint mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales** > **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Catchpoint**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** en la lista de usuarios. Haga clic en **Seleccionar** en la parte inferior de la pantalla.
1. Si ha configurado los roles tal y como se explica en el apartado anterior, puede seleccionarlo en la lista desplegable **Seleccionar un rol**.
1. En el cuadro de diálogo **Agregar asignación**, seleccione **Asignar**.

## <a name="configure-catchpoint-sso"></a>Configuración del inicio de sesión único de Catchpoint

1. En otra ventana del explorador web, inicie sesión en la aplicación Catchpoint como administrador.

1. Seleccione el icono **Settings** (Configuración) y después **SSO Identity Provider** (Proveedor de identidades de SSO).

   ![Captura de pantalla de configuración de Catchpoint con el proveedor de identidades de SSO seleccionado](./media/catchpoint-tutorial/configuration1.png)

1. En la página **Single Sign-On** (Inicio de sesión único), indique la información de los siguientes campos:

   ![Captura de pantalla de la página de inicio de sesión único de Catchpoint](./media/catchpoint-tutorial/configuration2.png)

   | Campo                        | Value                                                                                                           |
   | ---------------------------- | --------------------------------------------------------------------------------------------------------------- |
   | **Espacio de nombres**                | Un valor de espacio de nombres válido.                                                                                        |
   | **Identity Provider Issuer** (Emisor de proveedor de identidades) | El valor de `Azure AD Identifier` de Azure Portal.                                                          |
   | **Single Sign On Url** (Dirección URL de inicio de sesión)       | El valor de `Login URL` de Azure Portal.                                                                    |
   | **Certificate**              | El contenido del archivo de `Certificate (Base64)` descargado de Azure Portal. Utilice el Bloc de notas para ver y copiar. |

   También puede cargar el archivo **XML de metadatos de federación** seleccionando la opción **Upload Metadata** (Cargar metadatos).

1. Seleccione **Guardar**.

### <a name="create-a-catchpoint-test-user"></a>Creación de un usuario de prueba de Catchpoint

Catchpoint admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No tiene elementos de acción en esta sección. Si un usuario no existe ya en Catchpoint, se crea uno nuevo después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones.

#### <a name="sp-initiated"></a>Iniciado por SP:

- Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de Catchpoint, donde puede iniciar el flujo de inicio de sesión.

- Vaya directamente a la dirección URL de inicio de sesión de Catchpoint e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

- Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de Catchpoint para la que ha configurado el inicio de sesión único.

También puede usar Aplicaciones de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Catchpoint en Mis aplicaciones, si se ha configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para comenzar el flujo de inicio de sesión; y si se ha configurado en modo IDP, debería iniciar sesión automáticamente en la instancia de Catchpoint para la que haya configurado el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

> [!NOTE]
> Cuando inicie sesión en la aplicación Catchpoint mediante la página de inicio de sesión, después de proporcionar los valores de **Catchpoint Credentials** (Credenciales de Catchpoint), escriba el valor de **Namespace** (Espacio de nombres) válido en el campo **Company Credentials(SSO)** (Credenciales de la compañía [SSO]) y seleccione **Login** (Inicio de sesión).
>
> ![Configuración de Catchpoint](./media/catchpoint-tutorial/loginimage.png)

## <a name="next-steps"></a>Pasos siguientes

Después de configurar Catchpoint, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión es una extensión de acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).