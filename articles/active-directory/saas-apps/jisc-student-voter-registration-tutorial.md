---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Jisc Student Voter Registration'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Jisc Student Voter Registration.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/18/2021
ms.author: jeedes
ms.openlocfilehash: 2441fedaab5c8ae0ef746acfb4290b3b10e34853
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132295271"
---
# <a name="tutorial-azure-ad-sso-integration-with-jisc-student-voter-registration"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con Jisc Student Voter Registration

En este tutorial, obtendrá información sobre cómo integrar Jisc Student Voter Registration con Azure Active Directory (Azure AD). Cuando integre Jisc Student Voter Registration con Azure AD, podrá:

* Controlar en Azure AD quién tiene acceso a Jisc Student Voter Registration.
* Permitir que los usuarios puedan iniciar sesión automáticamente en Jisc Student Voter Registration con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) de Jisc Student Voter Registration.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Jisc Student Voter Registration admite el inicio de sesión único iniciado por **SP**.
* Jisc Student Voter Registration admite el aprovisionamiento de usuarios **Just-In-Time**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-jisc-student-voter-registration-from-the-gallery"></a>Incorporación de Jisc Student Voter Registration desde la galería

Para configurar la integración de Jisc Student Voter Registration en Azure AD, es preciso agregar esta aplicación desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Jisc Student Voter Registration** en el cuadro de búsqueda.
1. Seleccione **Jisc Student Voter Registration** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-jisc-student-voter-registration"></a>Configuración y prueba del inicio de sesión único de Azure AD para Jisc Student Voter Registration

Configure y pruebe el inicio de sesión único de Azure AD con Jisc Student Voter Registration mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Jisc Student Voter Registration.

Para configurar y probar el inicio de sesión único de Azure AD con Jisc Student Voter Registration, realice los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en Jisc Student Voter Registration](#configure-jisc-student-voter-registration-sso)**: para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en Jisc Student Voter Registration](#create-jisc-student-voter-registration-test-user)**: para tener un homólogo de B.Simon en Jisc Student Voter Registration vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **Jisc Student Voter Registration**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos:

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://www.studentvoterregistration.ac.uk/consent`

1. La aplicación Jisc Student Voter Registration espera las aserciones de SAML en un formato específico, lo cual requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación Jisc Student Voter Registration espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.

    | Nombre |  Atributo de origen|
    | ---------|  --------- |
    | postalcode | user.postalcode |
    | Identificador de usuario único | user.objectid |

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Set up Jisc Student Voter Registration** (Configurar Jisc Student Voter Registration), copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección va a permitir que B.Simon acceda a Jisc Student Voter Registration mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Jisc Student Voter Registration**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-jisc-student-voter-registration-sso"></a>Configuración del inicio de sesión único de Jisc Student Voter Registration

Para configurar el inicio de sesión único en **Jisc Student Voter Registration**, es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de Jisc Student Voter Registration](mailto:studentvote@jisc.ac.uk). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-jisc-student-voter-registration-test-user"></a>Creación de un usuario de prueba de Jisc Student Voter Registration

En esta sección se crea un usuario llamado B.Simon en Jisc Student Voter Registration. Jisc Student Voter Registration admite el aprovisionamiento de usuarios Just-In-Time que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si aún no existe aún un usuario en Jisc Student Voter Registration, se crea uno después de la autenticación.

## <a name="test-sso"></a>Prueba de SSO

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esta acción le redirigirá a la URL de inicio de sesión de Jisc Student Voter Registration, desde donde puede poner en marcha el flujo de inicio de sesión. 

* Vaya a la URL de inicio de sesión de Jisc Student Voter Registration directamente y ponga en marcha el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de Jisc Student Voter Registration en Aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de Jisc Student Voter Registration. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurada Jisc Student Voter Registration, puede aplicar el control de sesión, que protege la información confidencial de la organización de la filtración y la infiltración en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Defender para aplicaciones en la nube](/cloud-app-security/proxy-deployment-aad).
