---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con Adaptive Shield'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Adaptive Shield.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/04/2021
ms.author: jeedes
ms.openlocfilehash: 7639f247c55f051a869f93ee4d297b871ccbe6fa
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132281191"
---
# <a name="tutorial-azure-ad-sso-integration-with-adaptive-shield"></a>Tutorial: Integración del inicio de sesión único de Azure AD con Adaptive Shield

En este tutorial obtendrá más información sobre cómo integrar Adaptive Shield con Azure Active Directory (Azure AD). Al integrar Adaptive Shield con Azure AD, puede:

* Controlar quién tiene acceso a Adaptive Shield en Azure AD.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Adaptive Shield con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción de Adaptive Shield con el inicio de sesión único (SSO) habilitado.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Adaptive Shield admite el inicio de sesión único iniciado por **SP**.

* Adaptive Shield admite el aprovisionamiento de usuarios **Just-In-Time**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-adaptive-shield-from-the-gallery"></a>Incorporación de Adaptive Shield desde la galería

Para configurar la integración de Adaptive Shield en Azure AD, deberá agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Adaptive Shield** en el cuadro de búsqueda.
1. Seleccione **Adaptive Shield** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-adaptive-shield"></a>Configuración y prueba del inicio de sesión único de Azure AD para Adaptive Shield

Configure y pruebe el inicio de sesión único de Azure AD con Adaptive Shield mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Adaptive Shield.

Para configurar y probar el inicio de sesión único de Azure AD con Adaptive Shield, complete los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de Adaptive Shield](#configure-adaptive-shield-sso)** : para configurar los valores del inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Adaptive Shield](#create-adaptive-shield-test-user)** : para tener un homólogo de B.Simon en Adaptive Shield vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **Adaptive Shield**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **Identificador (id. de entidad)** , escriba una de las URL siguientes:

    | **Entorno** | **URL** |
    |---------|--------|
    | **Espacio aislado** |`https://staging-dashboard.adaptive-shield.com/api/sso/saml` |
    | **Producción** |`https://dashboard.adaptive-shield.com/api/sso/saml` |

    b. En el cuadro de texto **URL de respuesta**, escriba una de las siguientes direcciones URL:

    | **Entorno** | **URL** |
    |---------|--------|
    | **Espacio aislado** | `https://staging-dashboard.adaptive-shield.com/api/sso/saml` |
    | **Producción** | `https://dashboard.adaptive-shield.com/api/sso/saml` |
    
    c. En el cuadro de texto **Sign on URL** (Dirección URL de inicio de sesión), escriba una de las siguientes direcciones URL:

    | **Entorno** | **URL** |
    |---------|--------|
    | **Espacio aislado** | `https://staging-dashboard.adaptive-shield.com` |
    | **Producción** |`https://dashboard.adaptive-shield.com` |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Adaptive Shield**, copie las direcciones URL adecuadas en función de sus necesidades.

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

En esta sección, concederá acceso a Adaptive Shield a B.Simon para permitirle usar el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Adaptive Shield**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-adaptive-shield-sso"></a>Configuración del inicio de sesión único de Adaptive Shield

Para configurar el inicio de sesión único en **Adaptive Shield**, es preciso enviar el **certificado (Base64)** descargado y las direcciones URL correspondientes copiadas de Azure Portal al [equipo de soporte técnico de Adaptive Shield](mailto:support@adaptive-shield.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-adaptive-shield-test-user"></a>Creación de un usuario de prueba de Adaptive Shield

En esta sección, se crea un usuario llamado Britta Simon en Adaptive Shield. Adaptive Shield admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si aún no existe un usuario en Adaptive Shield, se crea uno después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la dirección URL de inicio de sesión de Adaptive Shield, donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de Adaptive Shield e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Adaptive Shield en Aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de esta aplicación. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Adaptive Shield, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender for Cloud Apps](/cloud-app-security/proxy-deployment-aad).
