---
title: 'Tutorial: Integración de Azure Active Directory con JOBHUB | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y JOBHUB.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/16/2019
ms.author: jeedes
ms.openlocfilehash: 5b0ed5b76aa9b097fcd4559a88822fb37e0d4788
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124739473"
---
# <a name="tutorial-integrate-jobhub-with-azure-active-directory"></a>Tutorial: Integración de JOBHUB con Azure Active Directory

En este tutorial, obtendrá información sobre cómo integrar JOBHUB con Azure Active Directory (Azure AD). Al integrar JOBHUB con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a JOBHUB.
* Permitir que los usuarios puedan iniciar sesión automáticamente en JOBHUB con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede obtener la evaluación gratuita de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).
* Suscripción habilitada para el inicio de sesión único (SSO) en JOBHUB

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba. JOBHUB admite el inicio de sesión único iniciado por **SP**.

## <a name="adding-jobhub-from-the-gallery"></a>Adición de JOBHUB desde la galería

Para configurar la integración de JOBHUB en Azure AD, será preciso agregar JOBHUB desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **JOBHUB** en el cuadro de búsqueda.
1. Seleccione **JOBHUB** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el inicio de sesión único de Azure AD con JOBHUB utilizando un usuario de prueba llamado **Britta Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de JOBHUB.

Para configurar y probar el inicio de sesión único de Azure AD con JOBHUB, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración del inicio de sesión único en JOBHUB](#configure-jobhub-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de JOBHUB](#create-jobhub-test-user)** : para tener un homólogo de Britta Simon en JOBHUB que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **JOBHUB**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, especifique los valores de los siguientes campos:  En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://pasona.jobhub.jp/saml/init`

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de atención al cliente de JOBHUB](mailto:platform@pasonagroup.co.jp) para obtener el valor. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la sección **Certificado de firma de SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Certificado de firma de SAML**.

    ![Edición del certificado de firma de SAML](common/edit-certificate.png)

1. En la sección **Certificado de firma de SAML**, copie el valor de **Huella digital** en el equipo.

    ![Copia del valor de la huella digital](common/copy-thumbprint.png)

1. En la sección **Set up JOBHUB** (Configurar JOBHUB), copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-jobhub-sso"></a>Configuración del inicio de sesión único de JOBHUB

Para configurar el inicio de sesión único en **JOBHUB**, es preciso enviar el **valor de la huella digital** y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de JOBHUB](mailto:platform@pasonagroup.co.jp). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado Britta Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `Britta Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `BrittaSimon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, se habilita a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a JOBHUB.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **JOBHUB**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En la lista de usuarios del cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-jobhub-test-user"></a>Creación de un usuario de prueba de JOBHUB

En esta sección, creará un usuario llamado Britta Simon en JOBHUB. Trabaje con el [equipo de soporte técnico de JOBHUB](mailto:platform@pasonagroup.co.jp) para agregar los usuarios a la plataforma de JOBHUB. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-sso"></a>Prueba de SSO

Al seleccionar el icono de JOBHUB en el panel de acceso, debería iniciar sesión automáticamente en la versión de JOBHUB para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)