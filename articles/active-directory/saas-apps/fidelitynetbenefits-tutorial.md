---
title: 'Tutorial: Integración de Azure Active Directory con Fidelity NetBenefits | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Fidelity NetBenefits.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/12/2019
ms.author: jeedes
ms.openlocfilehash: 1cbe4e16c92f6d090f2aeb45d0aa4e7a56aac702
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124835289"
---
# <a name="tutorial-azure-active-directory-integration-with-fidelity-netbenefits"></a>Tutorial: Integración de Azure Active Directory con Fidelity NetBenefits

En este tutorial, obtendrá información sobre cómo integrar Fidelity NetBenefits con Azure Active Directory (Azure AD).
La integración de Fidelity NetBenefits con Azure AD proporciona las siguientes ventajas:

* En Azure AD se puede controlar quién tiene acceso a Fidelity NetBenefits.
* Puede permitir que los usuarios inicien sesión automáticamente en Fidelity NetBenefits (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Fidelity NetBenefits se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción que permita el inicio de sesión único en Fidelity NetBenefits

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Fidelity NetBenefits admite SSO iniciado por **IDP**.

* Fidelity NetBenefits admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="adding-fidelity-netbenefits-from-the-gallery"></a>Adición de Fidelity NetBenefits desde la galería

Para configurar la integración de Fidelity NetBenefits en Azure AD, será preciso que agregue Fidelity NetBenefits desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Fidelity NetBenefits desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Fidelity NetBenefits**, seleccione **Fidelity NetBenefits** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Fidelity NetBenefits en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Fidelity NetBenefits con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Fidelity NetBenefits.

Para configurar y probar el inicio de sesión único de Azure AD con Fidelity NetBenefits, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en Fidelity NetBenefits](#configure-fidelity-netbenefits-single-sign-on)**: para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Crear un usuario de prueba de Fidelity NetBenefits](#create-fidelity-netbenefits-test-user)**: para tener un homólogo de Britta Simon en Fidelity NetBenefits que esté vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Fidelity NetBenefits, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Fidelity NetBenefits**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la página **Configurar inicio de sesión único con SAML** realice los siguientes pasos:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de Fidelity NetBenefits](common/idp-intiated.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente:

    Para un entorno de pruebas: `urn:sp:fidelity:geninbndnbparts20:uat:xq1`

    Para un entorno de producción: `urn:sp:fidelity:geninbndnbparts20`

    b. En el cuadro de texto **URL de respuesta**, escriba la dirección URL que va a proporcionar Fidelity en el momento de la implementación, o bien póngase en contacto con un responsable del servicio de atención al cliente de Fidelity.

5. La aplicación Fidelity NetBenefits espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde **nameidentifier** se asigna con **user.userprincipalname**. La aplicación Fidelity NetBenefits espera que el valor de **nameidentifier** se corresponda con **employeeid** o con cualquier otro recurso que sea aplicable a su organización, como **nameidentifier**, por lo que deberá modificar la asignación de atributos. Para ello, debe hacer clic en el icono **Editar** y cambiarla.

    ![imagen](common/edit-attribute.png)

    >[!Note]
    >Fidelity NetBenefits admite la federación estática y dinámica. La opción estática significa que no usará el aprovisionamiento de usuarios Just-In-Time basado en SAML y la opción dinámica significa que admite el aprovisionamiento de usuarios Just-In-Time. Para utilizar el aprovisionamiento basado en JIT, los clientes tienen que agregar algunas notificaciones más en Azure AD, como la fecha de nacimiento del usuario, etc. Estos detalles los proporciona el **responsable del servicio de atención al cliente de Fidelity** asignado, quien debe habilitar esta federación dinámica para su instancia.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

7. En la sección **Set up Fidelity NetBenefits** (Configurar Fidelity NetBenefits), copie las direcciones URL copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-fidelity-netbenefits-single-sign-on"></a>Configuración del inicio de sesión único en Fidelity NetBenefits

Para configurar el inicio de sesión único en **Fidelity NetBenefits**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de Fidelity NetBenefits](mailto:SSOMaintenance@fmr.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Fidelity NetBenefits para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**, **Fidelity NetBenefits**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Fidelity NetBenefits**.

    ![Vínculo de Fidelity NetBenefits en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-fidelity-netbenefits-test-user"></a>Creación de un usuario de prueba de Fidelity NetBenefits

En esta sección, creará un usuario llamado Britta Simon en Fidelity NetBenefits. Si va a crear la federación estática, trabaje con el **responsable del servicio de atención al cliente de Fidelity** para crear usuarios en la plataforma Fidelity NetBenefits. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

Para la federación dinámica, los usuarios se crean mediante el aprovisionamiento de usuarios Just-In-Time. Para utilizar el aprovisionamiento basado en JIT, los clientes tienen que agregar algunas notificaciones más en Azure AD, como la fecha de nacimiento del usuario, etc. Estos detalles los proporciona el **responsable del servicio de atención al cliente de Fidelity** asignado, quien debe habilitar esta federación dinámica para su instancia.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Fidelity NetBenefits en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Fidelity NetBenefits para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)