---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con TINFOIL SECURITY'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y TINFOIL SECURITY.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/20/2021
ms.author: jeedes
ms.openlocfilehash: 48d46916143fd219c81354a2d3f62ea260c9a2fb
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132303971"
---
# <a name="tutorial-azure-ad-sso-integration-with-tinfoil-security"></a>Tutorial: Integración del inicio de sesión único de Azure AD con TINFOIL SECURITY

En este tutorial, aprenderá a integrar TINFOIL SECURITY con Azure Active Directory (Azure AD). Al integrar TINFOIL SECURITY con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a TINFOIL SECURITY.
* Permitir que los usuarios inicien sesión automáticamente en TINFOIL SECURITY con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en TINFOIL SECURITY.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* TINFOIL SECURITY admite el inicio de sesión único iniciado por **IDP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-tinfoil-security-from-the-gallery"></a>Agregar TINFOIL SECURITY desde la galería

Para configurar la integración de TINFOIL SECURITY en Azure AD, debe agregar TINFOIL SECURITY desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **TINFOIL SECURITY** en el cuadro de búsqueda.
1. Seleccione **TINFOIL SECURITY** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-tinfoil-security"></a>Configuración y prueba del inicio de sesión único de Azure AD para TINFOIL SECURITY

Configure y pruebe el inicio de sesión único de Azure AD con TINFOIL SECURITY con un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de TINFOIL SECURITY.

Para configurar y probar el inicio de sesión único de Azure AD con TINFOIL SECURITY, complete los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de TINFOIL SECURITY](#configure-tinfoil-security-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de TINFOIL SECURITY](#create-tinfoil-security-test-user)** : para tener un homólogo de B.Simon en TINFOIL SECURITY que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **TINFOIL SECURITY**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, la aplicación está preconfigurada y las direcciones URL necesarias ya se han rellenado previamente con Azure. El usuario debe guardar la configuración, para lo que debe hacer clic en el botón **Guardar**.

1. La aplicación Visitly espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación Visitly espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen |
    | ------------------- | -------------|
    | accountid | UXXXXXXXXXXXXX |

    > [!NOTE]
    > El valor de accountid se explica más adelante en el tutorial.

1. En la sección **Certificado de firma de SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Certificado de firma de SAML**.

    ![Edición del certificado de firma de SAML](common/edit-certificate.png)

1. En la sección **Certificado de firma de SAML**, copie el valor de **Huella digital** y guárdelo en el equipo.

    ![Copia del valor de la huella digital](common/copy-thumbprint.png)

1. En la sección **Configurar TINFOIL SECURITY**, copie las direcciones URL adecuadas según sus requisitos.

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

En esta sección, habilitará a B.Simon para que use el inicio de sesión único de Azure concediéndole acceso a TINFOIL SECURITY.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **TINFOIL SECURITY**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-tinfoil-security-sso"></a>Configuración del inicio de sesión único de TINFOIL SECURITY

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de TINFOIL SECURITY.

1. En la barra de herramientas de la parte superior, haga clic en el icono de **Mi cuenta**.

    ![Panel](./media/tinfoil-security-tutorial/account.png "Panel")

1. Haga clic en **Seguridad**.

    ![Seguridad](./media/tinfoil-security-tutorial/details.png "Seguridad")

1. Siga estos pasos en la página de configuración **Inicio de sesión único** :

    ![Inicio de sesión único](./media/tinfoil-security-tutorial/certificate.png "Inicio de sesión único")

    a. Seleccione **Habilitar SAML**.

    b. Haga clic en **Configuración manual**.

    c. En el cuadro de texto **Dirección URL de publicación de SAML**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    d. En el cuadro de texto **SAML Certificate Fingerprint** (Huella digital de certificado de SAML), pegue el valor de **Huella digital** del certificado que haya copiado de la sección **Certificado de firma de SAML**.
  
    e. Copie el valor del **identificador de cuenta** y péguelo en el cuadro de texto **Atributo de origen** de la sección **Atributos y notificaciones de usuario** de Azure Portal.

    f. Haga clic en **Save**(Guardar).

### <a name="create-tinfoil-security-test-user"></a>Creación de un usuario de prueba de TINFOIL SECURITY

Para permitir que los usuarios de Azure AD inicien sesión en TINFOIL SECURITY, deben aprovisionarse en TINFOIL SECURITY. En el caso de TINFOIL SECURITY, el aprovisionamiento es una tarea manual.

**Siga estos pasos para que se aprovisione un usuario:**

1. Si el usuario forma parte de una cuenta de empresa, deberá [ponerse en contacto con el equipo de soporte técnico de TINFOIL SECURITY](https://www.tinfoilsecurity.com/contact) para que se cree la cuenta del usuario.

1. Si el usuario es usuario habitual de SaaS de TINFOIL SECURITY, puede agregar un colaborador a cualquiera de sus sitios. Esta acción desencadena un proceso para enviar una invitación al correo electrónico especificado y crear una cuenta de usuario de TINFOIL SECURITY.

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de TINFOIL SECURITY que ofrezca TINFOIL SECURITY para aprovisionar cuentas de usuario de Azure AD.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones.

* Haga clic en Probar esta aplicación en Azure Portal. Se debería iniciar sesión automáticamente en la instancia de TINFOIL SECURITY para la que configuró el inicio de sesión único.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de TINFOIL SECURITY en Aplicaciones, debería iniciar sesión automáticamente en la instancia de TINFOIL SECURITY para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya configurado TINFOIL SECURITY, puede aplicar el control de sesión, que protege a la organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
