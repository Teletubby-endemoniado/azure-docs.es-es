---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Nitro Productivity Suite | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Nitro Productivity Suite.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/28/2020
ms.author: jeedes
ms.openlocfilehash: 85c880cc318b025a9ca70a19eaa8c743c3e243e8
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124762443"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-nitro-productivity-suite"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Nitro Productivity Suite

En este tutorial aprenderá a integrar Nitro Productivity Suite con Azure Active Directory (Azure AD). Al integrar Nitro Productivity Suite con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Nitro Productivity Suite.
* Permitir que los usuarios inicien sesión automáticamente en Nitro Productivity Suite con sus cuentas de Azure AD.
* Administrar sus cuentas en una ubicación central: Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesitará lo siguiente:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una [suscripción de nivel Enterprise](https://www.gonitro.com/pricing) a Nitro Productivity Suite.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Nitro Productivity Suite admite el inicio de sesión único iniciado por **SP** e **IDP**.
* Nitro Productivity Suite admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="add-nitro-productivity-suite-from-the-gallery"></a>Adición de Nitro Productivity Suite desde la galería

Para configurar la integración de Nitro Productivity Suite en Azure AD, deberá agregar Nitro Productivity Suite desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel izquierdo, seleccione **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Nitro Productivity Suite** en el cuadro de búsqueda.
1. Seleccione **Nitro Productivity Suite** en los resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-nitro-productivity-suite"></a>Configuración y prueba del inicio de sesión único Azure AD para Nitro Productivity Suite

Configure y pruebe el inicio de sesión único de Azure AD con Nitro Productivity Suite mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Nitro Productivity Suite.

Para configurar y probar el inicio de sesión único de Azure AD con Nitro Productivity Suite, es preciso completar los siguientes bloques de creación:

1. [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso), para permitir que los usuarios puedan utilizar esta característica.

    a. [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con B. Simon.
    
    b. [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para que B. Simon pueda usar el inicio de sesión único de Azure AD.
    
2. [Creación de un usuario de prueba de Nitro Productivity Suite](#create-a-nitro-productivity-suite-test-user), para tener un homólogo de B.Simon en Nitro Productivity Suite vinculado a la representación del usuario en Azure AD.
1. [Comprobación del inicio de sesión único](#test-sso), para verificar que la configuración funciona correctamente.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **Nitro Productivity Suite**, busque la sección **Administrar**. Seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la sección **Certificado de firma de SAML**, localice **Certificado (Base 64)** . Seleccione **Descargar** para descargar el certificado y guárdelo en el equipo.

    ![Captura de pantalla de la sección Certificado de firma de SAML, con el vínculo Descargar resaltado](common/certificatebase64.png)
    
1. En la sección **Configurar Nitro Productivity Suite**, seleccione el icono de copia situado junto a **Dirección URL de inicio de sesión**.
    
    ![Captura de pantalla de la sección Configurar Nitro Productivity Suite con la dirección URL y el icono de copia resaltados](common/copy-configuration-urls.png)
    
1. En el [portal de administración de Nitro](https://admin.gonitro.com/), en la página **Enterprise Settings** (Configuración de la empresa), busque la sección **Single Sign-On** (Inicio de sesión único). Seleccione **Setup SAML SSO** (Configurar el inicio de sesión único de SAML).

    a. Pegue el valor de **Dirección URL de inicio de sesión** del paso anterior en el campo **Sign In URL** (Dirección URL de inicio de sesión).
    
    b. Cargue el archivo de **Certificado (Base 64)** del paso anterior en el campo **X509 Signing Certificate** (Certificado de firma X509).
    
    c. Seleccione **Submit** (Enviar).
    
    d. Seleccione **Enable Single Sign-On**(Habilitar inicio de sesión único).


1. Vuelva a [Azure Portal](https://portal.azure.com/). En la página **Configurar el inicio de sesión único con SAML**, seleccione el icono con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Captura de pantalla de la página Configurar el inicio de sesión único con SAML con el icono de lápiz resaltado](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si quiere configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, copie y pegue el valor del campo **SAML Entity ID** (Identificador de entidad de SAML) del [portal de administración de Nitro](https://admin.gonitro.com/). Debe tener el siguiente patrón: `urn:auth0:gonitro-prod:<ENVIRONMENT>`.

    b. En el cuadro de texto **URL de respuesta**, copie y pegue el valor del campo **ACS URL (URL de ACS)** del [portal de administración de Nitro](https://admin.gonitro.com/). Debe tener el siguiente patrón: `https://gonitro-prod.eu.auth0.com/login/callback?connection=<ENVIRONMENT>`.

1. Seleccione **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://sso.gonitro.com/login`

1. Seleccione **Guardar**.

1. La aplicación Nitro Productivity Suite espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![Captura de pantalla de los atributos predeterminados](common/default-attributes.png)

1. Además de los atributos anteriores, la aplicación Nitro Productivity Suite espera que se devuelvan algunos atributos más en la respuesta de SAML. Estos atributos se rellenan previamente, pero puede revisarlos según sus requisitos.
    
    | Nombre  |  Atributo de origen|
    | ---------------| --------------- |
    | employeeNumber |  user.objectid |


### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Seleccione la casilla **Mostrar contraseña** y, a continuación, anote la contraseña.
   1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección va a permitir que B.Simon acceda a Nitro Productivity Suite mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales** > **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Nitro Productivity Suite**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. Después, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** en la lista de usuarios. A continuación, elija **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, seleccione **Asignar**.

### <a name="create-a-nitro-productivity-suite-test-user"></a>Creación de un usuario de prueba de Nitro Productivity Suite

Nitro Productivity Suite admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. El usuario no tiene que hacer nada en esta sección. Si un usuario no existe ya en Nitro Productivity Suite, se crea uno nuevo después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

#### <a name="sp-initiated"></a>Iniciado por SP:

1. Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de Nitro Productivity Suite, donde puede iniciar el flujo de inicio de sesión.  

2. Vaya directamente a la dirección URL de inicio de sesión de Nitro Productivity Suite e inicie el flujo de inicio de sesión desde allí.

#### <a name="idp-initiated"></a>Iniciado por IDP:

* Haga clic en **Probar esta aplicación** en Azure Portal; debería iniciar sesión automáticamente en la instancia de Nitro Productivity Suite para la que configuró el inicio de sesión único. 

También puede usar el Panel de acceso de Microsoft para probar la aplicación en cualquier modo. Al hacer clic en el icono de Nitro Productivity Suite en el Panel de acceso, si está configurado en modo SP, se le redirigirá a la página de inicio de sesión de la aplicación para iniciar el flujo de inicio de sesión y, si está configurado en modo IDP, debería iniciar sesión automáticamente en la instancia de Nitro Productivity Suite para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).


## <a name="next-steps"></a>Pasos siguientes

Después de configurar Nitro Productivity Suite, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. Los controles de sesión proceden del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).