---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con NegometrixPortal Single Sign On (SSO)'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y NegometrixPortal Single Sign On (SSO).
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/10/2021
ms.author: jeedes
ms.openlocfilehash: 3fdc55289c33482db7384eed51ae3a2e75f7ad99
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131051560"
---
# <a name="tutorial-azure-ad-sso-integration-with-negometrixportal-single-sign-on-sso"></a>Tutorial: Integración del inicio de sesión único de Azure AD con NegometrixPortal Single Sign On (SSO)

En este tutorial aprenderá a integrar NegometrixPortal Single Sign On (SSO) con Azure Active Directory (Azure AD). Al integrar NegometrixPortal Single Sign On (SSO) con Azure AD, podrá:

* Controlar en Azure AD quién tiene acceso a NegometrixPortal Single Sign On (SSO).
* Permitir que los usuarios inicien sesión automáticamente en NegometrixPortal Single Sign On (SSO) con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en NegometrixPortal Single Sign On (SSO).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* NegometrixPortal Single Sign On (SSO) admite el inicio de sesión único iniciado por **SP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-negometrixportal-single-sign-on-sso-from-the-gallery"></a>Incorporación de NegometrixPortal Single Sign On (SSO) desde la galería

Para configurar la integración de NegometrixPortal Single Sign On (SSO) en Azure AD, es preciso agregarla desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **NegometrixPortal Single Sign On (SSO)** en el cuadro de búsqueda.
1. Seleccione **NegometrixPortal Single Sign On (SSO)** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-negometrixportal-single-sign-on-sso"></a>Configuración y prueba del inicio de sesión único de Azure AD para NegometrixPortal Single Sign On (SSO)

Configure y pruebe el inicio de sesión único de Azure AD con NegometrixPortal Single Sign On (SSO) mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de NegometrixPortal Single Sign On (SSO).

Para configurar y probar el inicio de sesión único de Azure AD con NegometrixPortal Single Sign On (SSO), es preciso seguir estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en NegometrixPortal Single Sign On (SSO)](#configure-negometrixportal-single-sign-on-sso-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en NegometrixPortal Single Sign On (SSO)](#create-negometrixportal-single-sign-on-sso-test-user)** : para tener un homólogo de B.Simon en NegometrixPortal Single Sign On (SSO) vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **NegometrixPortal Single Sign On (SSO)** , busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://portal.negometrix.com/sso/<CUSTOMURL>`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico al cliente de NegometrixPortal Single Sign On (SSO)](mailto:sander.hoek@negometrix.com) para obtener este valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación NegometrixPortal Single Sign On (SSO) espera las aserciones de SAML en un formato específico, lo cual requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, NegometrixPortal Single Sign On (SSO) espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen|
    | ---------------|  --------- |
    | upn | user.userprincipalname |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en el botón de copia para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

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

En esta sección va a permitir que B.Simon acceda a NegometrixPortal Single Sign On (SSO) mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **NegometrixPortal Single Sign On (SSO)** .
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-negometrixportal-single-sign-on-sso-sso"></a>Configuración del inicio de sesión único en NegometrixPortal Single Sign On (SSO)

Para configurar el inicio de sesión único en **NegometrixPortal Single Sign On (SSO)** , debe enviar la **Dirección URL de metadatos de federación de aplicación** al [equipo de soporte técnico de NegometrixPortal Single Sign On (SSO)](mailto:sander.hoek@negometrix.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-negometrixportal-single-sign-on-sso-test-user"></a>Creación de un usuario de prueba en NegometrixPortal Single Sign On (SSO)

En esta sección creará un usuario llamado B.Simon en NegometrixPortal Single Sign On (SSO). Trabaje con el [equipo de soporte técnico de NegometrixPortal Single Sign On (SSO)](mailto:sander.hoek@negometrix.com) para agregar los usuarios a la plataforma de NegometrixPortal Single Sign On (SSO). Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de NegometrixPortal Single Sign On (SSO), donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de NegometrixPortal Single Sign On (SSO) e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de NegometrixPortal Single Sign On (SSO) en Aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de NegometrixPortal Single Sign On (SSO). Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado NegometrixPortal Single Sign On (SSO), puede aplicar el control de sesión, que protege a la organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).
