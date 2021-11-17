---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con SURFsecureID - Azure MFA'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SURFsecureID - Azure MFA.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/20/2021
ms.author: jeedes
ms.openlocfilehash: 8034af23b35545e1e5f5508632b0b8737537b0f5
ms.sourcegitcommit: 61f87d27e05547f3c22044c6aa42be8f23673256
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2021
ms.locfileid: "132063889"
---
# <a name="tutorial-azure-ad-sso-integration-with-surfsecureid---azure-mfa"></a>Tutorial: Integración del inicio de sesión único de Azure AD con SURFsecureID - Azure MFA

En este tutorial, aprenderá a integrar SURFsecureID - Azure MFA con Azure Active Directory (Azure AD). Al integrar SURFsecureID - Azure MFA con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a SURFsecureID - Azure MFA.
* Permitir que los usuarios inicien sesión automáticamente en SURFsecureID - Azure MFA con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Prerrequisitos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único (SSO) en SURFsecureID - Azure MFA.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* SURFsecureID - Azure MFA admite el inicio de sesión único iniciado por **SP**.

> [!NOTE]
> El identificador de esta aplicación es un valor de cadena fijo, por lo que solo se puede configurar una instancia en un inquilino.

## <a name="add-surfsecureid---azure-mfa-from-the-gallery"></a>Adición de SURFsecureID - Azure MFA desde la galería

Para configurar la integración de SURFsecureID - Azure MFA en Azure AD, debe agregar SURFsecureID - Azure MFA desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **SURFsecureID - Azure MFA** en el cuadro de búsqueda.
1. Seleccione **SURFsecureID - Azure MFA** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-surfsecureid---azure-mfa"></a>Configuración y prueba del inicio de sesión único de Azure AD para SURFsecureID - Azure MFA

Configure y pruebe el inicio de sesión único de Azure AD con SURFsecureID - Azure MFA mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de SURFsecureID - Azure MFA.

Para configurar y probar el inicio de sesión único de Azure AD con SURFsecureID - Azure MFA, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de SURFsecureID - Azure MFA](#configure-surfsecureid---azure-mfa-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba en SURFsecureID - Azure MFA](#create-surfsecureid---azure-mfa-test-user)** para tener un homólogo de B.Simon en SURFsecureID - Azure MFA vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **SURFsecureID - Azure MFA**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, siga estos pasos:

    a. En el cuadro de texto **Identificador (id. de entidad)** , escriba una de las URL siguientes:

    | **Entorno** | **URL** |
    |-------|------|
    | Ensayo | `https://azuremfa.test.surfconext.nl/saml/metadata` |
    | Producción | `https://azuremfa.surfconext.nl/saml/metadata` |

    b. En el cuadro de texto **URL de respuesta**, escriba una de las siguientes direcciones URL:

    | **Entorno** | **URL** |
    |-------|------|
    | Ensayo | `https://azuremfa.test.surfconext.nl/saml/acs` |
    | Producción | `https://azuremfa.surfconext.nl/saml/acs` |

    b. En el cuadro de texto **Sign on URL** (Dirección URL de inicio de sesión), escriba una de las siguientes direcciones URL:
    
    | **Entorno** | **URL** |
    |-------|------|
    | Ensayo | `https://sa.test.surfconext.nl` |
    | Producción | `https://sa.surfconext.nl` |

1. La aplicación SURFsecureID - Azure MFA espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación SURFsecureID - Azure MFA espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.
    
    | Nombre | Atributo de origen |
    | --------- | --------- |
    | urn:mace:dir:attribute-def:mail | user.mail |

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

En esta sección, va a permitir que B.Simon acceda a SURFsecureID - Azure MFA mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **SURFsecureID - Azure MFA**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-surfsecureid---azure-mfa-sso"></a>Configuración del inicio de sesión único de SURFsecureID - Azure MFA

Para configurar el inicio de sesión único en **SURFsecureID - Azure MFA**, debe enviar la **dirección URL de metadatos de federación de la aplicación** al [equipo de soporte técnico de SURFsecureID - Azure MFA](mailto:support@surfconext.nl). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-surfsecureid---azure-mfa-test-user"></a>Creación de un usuario de prueba de SURFsecureID - Azure MFA

En esta sección, creará un usuario llamado Britta Simon en SURFsecureID - Azure MFA. Colabore con el equipo de soporte técnico de [SURFsecureID - Azure MFA](mailto:support@surfconext.nl) para agregar los usuarios a la plataforma SURFsecureID - Azure MFA. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD con las siguientes opciones. 

* Haga clic en **Probar esta aplicación** en Azure Portal. Esto le redirigirá a la dirección URL de inicio de sesión de SURFsecureID - Azure MFA, donde puede iniciar el flujo de inicio de sesión. 

* Vaya directamente a la dirección URL de inicio de sesión de SURFsecureID - Azure MFA e inicie el flujo de inicio de sesión desde allí.

* Puede usar Mis aplicaciones de Microsoft. Al hacer clic en el icono de SURFsecureID - Azure MFA en Aplicaciones, se le redirigirá a la dirección URL de inicio de sesión de SURFsecureID - Azure MFA. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado SURFsecureID - Azure MFA, puede aplicar el control de sesión, que protege la filtración y la infiltración de la información confidencial de la organización en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).