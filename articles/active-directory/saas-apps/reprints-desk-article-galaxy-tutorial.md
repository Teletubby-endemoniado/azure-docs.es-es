---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Reprints Desk - Article Galaxy | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Reprints Desk - Article Galaxy.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/21/2020
ms.author: jeedes
ms.openlocfilehash: 33077902722a32f792c12598f7d555880050a574
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132309045"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-reprints-desk---article-galaxy"></a>Integración del inicio de sesión único (SSO) de Azure Active Directory con Reprints Desk - Article Galaxy

En este tutorial, aprenderá a integrar Reprints Desk - Article Galaxy con Azure Active Directory (Azure AD). Al integrar Reprints Desk - Article Galaxy con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Reprints Desk - Article Galaxy.
* Permitir que los usuarios inicien sesión automáticamente en Reprints Desk - Article Galaxy con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción a Reprints Desk - Article Galaxy habilitada para el inicio de sesión único.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Reprints Desk - Article Galaxy admite el inicio de sesión único iniciado por **IDP**.

* Reprints Desk - Article Galaxy admite el aprovisionamiento de usuarios **Just-In-Time**.

* [Una vez que configure Reprints Desk - Article Galaxy, puede aplicar controles de sesión, que protegen la filtración de la información confidencial de la organización en tiempo real. Los controles de sesión se extienden desde el acceso condicional. Obtenga información sobre cómo aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube.](/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-reprints-desk---article-galaxy-from-the-gallery"></a>Adición de Reprints Desk - Article Galaxy desde la galería

Para configurar la integración de Reprints Desk - Article Galaxy en Azure AD, hay que agregar Reprints Desk - Article Galaxy desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Reprints Desk - Article Galaxy** en el cuadro de búsqueda.
1. Seleccione **Reprints Desk - Article Galaxy** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.


## <a name="configure-and-test-azure-ad-single-sign-on-for-reprints-desk---article-galaxy"></a>Configuración y prueba del inicio de sesión único de Azure AD para Reprints Desk - Article Galaxy

Configure y pruebe el inicio de sesión único de Azure AD con Reprints Desk - Article Galaxy con un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Reprints Desk - Article Galaxy.

Para configurar y probar el inicio de sesión único de Azure AD con Reprints Desk - Article Galaxy, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    * **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    * **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Reprints Desk - Article Galaxy](#configure-reprints-desk-article-galaxy-sso)**: para configurar los valores de inicio de sesión único en la aplicación.
    * **[Creación de un usuario de prueba de Reprints Desk - Article Galaxy](#create-reprints-desk-article-galaxy-test-user)**: para tener un homólogo de B.Simon en Reprints Desk - Article Galaxy que esté vinculado con la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Reprints Desk - Article Galaxy**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, la aplicación está preconfigurada y las direcciones URL necesarias ya se han rellenado previamente con Azure. El usuario debe guardar la configuración, para lo que debe hacer clic en el botón **Guardar**.


1. La aplicación Reprints Desk - Article Galaxy espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación Reprints Desk - Article Galaxy espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre | Atributo de origen|
    | ------------ | --------- |
    | firstname | user.givenname |
    | lastname | user.surname |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Configurar Reprints Desk - Article Galaxy**, copie las direcciones URL adecuadas según sus requisitos.

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

En esta sección, va a conceder a B. Simon acceso a Reprints Desk - Article Galaxy mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Reprints Desk - Article Galaxy**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-reprints-desk-article-galaxy-sso"></a>Configuración del inicio de sesión único de Reprints Desk - Article Galaxy

Para configurar el inicio de sesión único en **Reprints Desk - Article Galaxy**, debe enviar el archivo **XML de metadatos de federación** descargado y las direcciones URL correspondientes copiadas de Azure Portal al [equipo de soporte técnico de Reprints Desk - Article Galaxy](mailto:customersupport@reprintsdesk.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-reprints-desk-article-galaxy-test-user"></a>Creación de un usuario de prueba de Reprints Desk - Article Galaxy

En esta sección, se crea un usuario llamado B.Simon en Reprints Desk - Article Galaxy. Reprints Desk - Article Galaxy admite el aprovisionamiento de usuarios Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario ya no existe en Reprints Desk - Article Galaxy, se crea otro después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Reprints Desk - Article Galaxy en el panel de acceso, iniciará sesión automáticamente en la instancia de Reprints Desk - Article Galaxy para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Prueba de Reprints Desk - Article Galaxy con Azure AD](https://aad.portal.azure.com/)

- [¿Qué es el control de sesión en Microsoft Defender para aplicaciones en la nube?](/cloud-app-security/proxy-intro-aad)

- [Protección de Reprints Desk - Article Galaxy con visibilidad y controles avanzados](/cloud-app-security/proxy-intro-aad)
