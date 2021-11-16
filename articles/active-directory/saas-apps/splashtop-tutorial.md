---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Splashtop | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Splashtop.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/19/2021
ms.author: jeedes
ms.openlocfilehash: 42a6c809dbf67a796cf2cab7535fc48d9de914c9
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132329125"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-splashtop"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Splashtop

En este tutorial, aprenderá a integrar Splashtop con Azure Active Directory (Azure AD). Al integrar Splashtop con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a Splashtop.
* Permitir a los usuarios iniciar sesión automáticamente en Splashtop con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en Splashtop.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Splashtop admite el SSO iniciado por **SP**.
* Splashtop admite el [aprovisionamiento y desaprovisionamiento **automático** de usuarios](splashtop-provisioning-tutorial.md) (recomendado).

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-splashtop-from-the-gallery"></a>Incorporación de Splashtop desde la galería

Para configurar la integración de Splashtop en Azure AD, tendrá que agregar Splashtop desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Splashtop** en el cuadro de búsqueda.
1. Seleccione **Splashtop** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-splashtop"></a>Configuración y prueba del SSO de Azure AD para Splashtop

Configure y pruebe el inicio de sesión único de Azure AD con Splashtop con un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de Splashtop.

Para configurar y probar el SSO de Azure AD con Splashtop, lleve a cabo los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Splashtop](#configure-splashtop-sso)** : para configurar los valores del inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de Splashtop](#create-splashtop-test-user)** : para tener un homólogo de B.Simon en Splashtop que esté vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de aplicaciones de **Splashtop**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz para **Configuración básica de SAML** y edite la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://my.splashtop.com/login/sso`

1. La aplicación Splashtop espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de pantalla muestra la lista de atributos predeterminados, donde **nameidentifier** se asigna con **user.userprincipalname**. La aplicación TicketManager espera que **nameidentifier** se asigne a **user.mail**, por lo que necesita editar la asignación de atributos; para ello, haga clic en el icono **Editar** y cambie esa asignación.

    ![Captura de pantalla que muestra User Attributes (Atributos de usuario) con el icono de edición seleccionado.](common/edit-attribute.png)

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **Certificado (Base64)** y seleccione **Descargar** para descargarlo y guardarlo en el equipo.

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

1. En la sección **Configurar Splashtop**, copie las direcciones URL adecuadas en función de sus necesidades.

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

En esta sección, va a permitir que B.Simon acceda a Splashtop mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Splashtop**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-splashtop-sso"></a>Configuración del inicio de sesión único de Splashtop

En esta sección, debe solicitar un nuevo método de inicio de sesión único desde el [portal web de Splashtop](https://my.splashtop.com/login).
1. En el portal web de Splashtop, vaya a la pestaña **Account info** / **Team** (Información de cuenta > Equipo) y desplácese hacia abajo hasta encontrar la sección **Single Sign On** (Inicio de sesión único). A continuación, haga clic en **Apply for new SSO method** (Solicitar nuevo método de SSO).

    ![Captura de pantalla que muestra la página de inicio de sesión único, donde puede seleccionar la opción para solicitar el nuevo método de S S O.](media/splashtop-tutorial/new-method.png)

1. En la ventana de solicitud, escriba un valor para **SSO name** (Nombre de SSO). Por ejemplo, New Azure, seleccione **Azure** como el tipo de IDP e inserte los valores de **Dirección URL de inicio de sesión** e **Identificador de Azure AD** copiados desde la aplicación Splashtop en Azure Portal.

    ![Captura de pantalla que muestra la página para solicitar el método S S O, donde puede especificar un nombre y otra información.](media/splashtop-tutorial/new-azure.png)

1. En información del certificado, haga clic con el botón derecho en el archivo de certificado descargado de la aplicación Splashtop en Azure Portal, edítelo en el Bloc de notas, copie el contenido y péguelo en el campo **Descargar certificado (Base64)** .

    ![Captura de pantalla que muestra cómo seleccionar un archivo de certificado y abrirlo con el Bloc de notas.](media/splashtop-tutorial/certificate.png)
    ![Captura de pantalla que muestra el contenido del archivo de certificado.](media/splashtop-tutorial/file.png)
    ![Captura de pantalla que muestra el cuadro de texto para descargar certificado.](media/splashtop-tutorial/azure.png)

1. Eso es todo. Haga clic en **Save** (Guardar) y el equipo de validación de SSO de Splashtop se pondrá en contacto con usted para obtener la información de comprobación y, a continuación, activará el método de SSO.

### <a name="create-splashtop-test-user"></a>Creación de un usuario de prueba de Splashtop

1. Después de activar el método de SSO, compruebe el método de SSO recién creado para habilitarlo en la sección **Inicio de sesión único**.

    ![Captura de pantalla que muestra la página de inicio de sesión único, donde puede habilitar el nuevo método.](media/splashtop-tutorial/enable.png)

1. Invite al usuario de prueba, por ejemplo, `B.Simon@contoso.com`, al equipo de Splashtop con el método de SSO recién creado.

    ![Captura de pantalla que muestra la página para invitar a usuarios, donde puede seleccionar el nuevo método.](media/splashtop-tutorial/invite.png)

1. Para cambiar una cuenta de Splashtop existente a una cuenta de SSO, consulte [Instrucciones](https://support-splashtopbusiness.splashtop.com/hc/en-us/articles/360038685691-How-to-associate-SSO-method-to-existing-team-admin-member-).

1. Eso es todo. Puede usar la cuenta de SSO para iniciar sesión en el portal web de Splashtop o en la aplicación de negocio de Splashtop.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de Splashtop, donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de Splashtop e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Splashtop en Aplicaciones, se le redirigirá a la URL de inicio de sesión de esa plataforma. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Pasos siguientes

Una vez que se ha configurado Splashtop, puede aplicar el control de sesión, que protege su organización en tiempo real frente a la filtración e infiltración de información confidencial. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
