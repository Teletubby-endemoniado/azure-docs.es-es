---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Fivetran | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Fivetran.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/10/2021
ms.author: jeedes
ms.openlocfilehash: 05acfe25fd92c135e72d7930a403b7eff08b7f50
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132330066"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-fivetran"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Fivetran

En este tutorial aprenderá a integrar Fivetran con Azure Active Directory (Azure AD). Al integrar Fivetran con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Fivetran.
* Permitir que los usuarios inicien sesión automáticamente en Fivetran con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una cuenta de Fivetran.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Fivetran admite el inicio de sesión único iniciado por **IDP**.
* Fivetran admite el aprovisionamiento de usuarios **Just-In-Time**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-fivetran-from-the-gallery"></a>Adición de Fivetran desde la galería

Para configurar la integración de Fivetran en Azure AD, es preciso agregar Fivetran desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Fivetran** en el cuadro de búsqueda.
1. Seleccione **Fivetran** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-fivetran"></a>Configuración y prueba del inicio de sesión único de Azure AD para Fivetran

Configure y pruebe el inicio de sesión único de Azure AD con Fivetran mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Fivetran.

Para configurar y probar el inicio de sesión único de Azure AD con Fivetran, es preciso completar los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Fivetran](#configure-fivetran-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en Fivetran](#create-fivetran-test-user)** : para tener un homólogo de B.Simon en Fivetran que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Fivetran**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, la aplicación está preconfigurada y las direcciones URL necesarias ya se han rellenado previamente con Azure. El usuario debe guardar la configuración, para lo que debe hacer clic en el botón **Guardar**.

1. La aplicación Fivetran espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación Fivetran espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.
    
    | Nombre |  Atributo de origen|
    | -------------- | --------- |
    | Nombre | user.givenname |
    | Apellidos | user.surname |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Fivetran**, copie los valores de **Dirección URL de inicio de sesión** e **Identificador de Azure AD**.

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

En esta sección, va a permitir que B.Simon acceda a Fivetran mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Fivetran**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-fivetran-sso"></a>Configuración del inicio de sesión único de Fivetran

En esta sección, configurará el inicio de sesión único en la instancia de **Fivetran**.

1. En otra ventana del explorador web, inicie sesión en su cuenta de Fivetran como propietario de la cuenta.

1. Seleccione la flecha situada en la esquina superior izquierda de la ventana y, a continuación, seleccione **Administrar cuenta** en la lista desplegable.

   ![Captura de pantalla que muestra la opción de menú Administrar cuenta seleccionada.](media/fivetran-tutorial/menu.png)

1. Vaya a la sección **Configuración de SAML** de la página **Configuración**.

   ![Captura de pantalla que muestra el panel Configuración de SAML con las opciones de configuración resaltadas.](media/fivetran-tutorial/settings.png)

   1. En **Habilitar autenticación SAML**, seleccione **Activado**.
   1. En **Dirección URL de inicio de sesión**, pegue el valor de **Dirección URL de inicio de sesión** que copió de Azure Portal.
   1. En **Emisor**, pegue el valor de **Identificador de Azure AD** que copió de Azure Portal.
   1. Abra el archivo de certificado descargado en un editor de texto, copie el certificado en el Portapapeles y luego péguelo en el cuadro de texto **Certificado público**.
   1. Seleccione **GUARDAR CONFIGURACIÓN**.

### <a name="create-fivetran-test-user"></a>Creación de un usuario de prueba de Fivetran

En esta sección se crea un usuario llamado B.Simon en Fivetran. Fivetran admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si el usuario no existe en Fivetran, se crea uno nuevo después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones.

* Haga clic en Probar esta aplicación en Azure Portal y debería iniciar sesión automáticamente en la instancia de Fivetran para la que configuró el inicio de sesión único.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Fivetran en Aplicaciones, debería iniciar sesión automáticamente en la instancia de Fivetran para la que configuró el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado Fivetran, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-any-app).
