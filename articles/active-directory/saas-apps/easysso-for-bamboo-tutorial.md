---
title: 'Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con EasySSO for Bamboo | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y EasySSO for Bamboo.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/02/2021
ms.author: jeedes
ms.openlocfilehash: 76c203339744e73d9239d44dfe984834b333cf93
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124753541"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-easysso-for-bamboo"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con EasySSO for Bamboo

En este tutorial aprenderá a integrar EasySSO for Bamboo con Azure Active Directory (Azure AD). Al integrar EasySSO for Bamboo con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a Bamboo.
* Permitir que los usuarios inicien sesión automáticamente en Bamboo con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Una suscripción habilitada para el inicio de sesión único de EasySSO for Bamboo.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* EasySSO for Bamboo admite el inicio de sesión único iniciado por **SP e IDP**.
* EasySSO for Bamboo admite el aprovisionamiento de usuarios **Just-In-Time**.

## <a name="add-easysso-for-bamboo-from-the-gallery"></a>Adición de EasySSO for Bamboo desde la galería

Para configurar la integración de EasySSO for Bamboo en Azure AD, deberá agregar EasySSO for Bamboo desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en Azure Portal con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **EasySSO for Bamboo** en el cuadro de búsqueda.
1. Seleccione **EasySSO for Bamboo** en el panel de resultados y, a continuación, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-sso-for-easysso-for-bamboo"></a>Configuración y prueba del inicio de sesión único de Azure AD para EasySSO for Bamboo

Configure y pruebe el inicio de sesión único de Azure AD con EasySSO for Bamboo mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es preciso establecer una relación de vinculación entre un usuario de Azure AD y el usuario correspondiente de EasySSO for Bamboo.

Para configurar y probar el inicio de sesión único de Azure AD con EasySSO for Bamboo, complete los siguientes pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único en EasySSO for Bamboo](#configure-easysso-for-bamboo-sso)** : para configurar los valores de inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de EasySSO for Bamboo](#create-easysso-for-bamboo-test-user)** , para tener un homólogo de B.Simon en EasySSO for Bamboo vinculado a la representación del usuario en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En Azure Portal, en la página de integración de la aplicación **EasySSO for Bamboo**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configuración del inicio de sesión único con SAML**, haga clic en el icono de lápiz de **Configuración básica de SAML** para editar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice los siguientes pasos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<SERVER_BASE_URL>/plugins/servlet/easysso/saml`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<SERVER_BASE_URL>/plugins/servlet/easysso/saml`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SERVER_BASE_URL>/login.jsp`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de EasySSO](mailto:support@techtime.co.nz) para obtener estos valores en caso de duda. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. La aplicación EasySSO for Bamboo espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados.

    ![imagen](common/default-attributes.png)

1. Además de lo anterior, la aplicación EasySSO for Bamboo espera que se devuelvan algunos atributos más, que se muestran a continuación, en la respuesta de SAML. Estos atributos también se rellenan previamente, pero puede revisarlos según sus requisitos.
    
    | Nombre | Atributo de origen |
    | ---------------| --------- |
    | urn:oid:0.9.2342.19200300.100.1.1 | user.userprincipalname |
    | urn:oid:0.9.2342.19200300.100.1.3 | user.mail |
    | urn:oid:2.16.840.1.113730.3.1.241 | user.displayname |
    | urn:oid:2.5.4.4 | user.surname |
    | urn:oid:2.5.4.42 | user.givenname |
    
    En caso de que los usuarios de Azure AD tengan **sAMAccountName** configurado, tendría que asignar **urn:oid:0.9.2342.19200300.100.1.1** en el atributo **sAMAccountName**.
    
1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en los vínculos **Descargar** de las opciones **Certificado (Base64)** o **XML de metadatos de federación** y guarde los archivos que desee en su equipo. Los necesitará más adelante para configurar EasySSO para Bamboo.

    ![Vínculo de descarga del certificado](./media/easysso-for-bamboo-tutorial/certificate.png)
    
    Si va a realizar la configuración de EasySSO for Bamboo manualmente con certificado, también necesitará copiar la **dirección URL de inicio de sesión** y el **identificador de Azure AD** en la siguiente sección y guardarlos en el equipo.

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

En esta sección va a permitir que B.Simon acceda a EasySSO for Bamboo mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **EasySSO for Bamboo**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.
1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.
1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que se asigne un rol a los usuarios, puede seleccionarlo en la lista desplegable **Seleccionar un rol**. Si no se ha configurado ningún rol para esta aplicación, verá seleccionado el rol "Acceso predeterminado".
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

## <a name="configure-easysso-for-bamboo-sso"></a>Configuración del inicio de sesión único en EasySSO for Bamboo

1. Para automatizar la configuración en Zoom, debe instalar la **extensión del navegador de inicio de sesión seguro de Aplicaciones**. Para ello, haga clic en **Instalar la extensión**.

    ![Extensión Mis aplicaciones](common/install-myappssecure-extension.png)

2. Después de agregar la extensión al explorador, haga clic en **Set up Zoom** (Configurar Zoom) para ir a la aplicación del mismo nombre. Ahí, escriba las credenciales de administrador para iniciar sesión en Zoom. La extensión del navegador configurará automáticamente la aplicación y automatizará los pasos del 3 al 10.

    ![Configuración](common/setup-sso.png)

3. Si quiere configurar Zoom manualmente, en otra ventana del explorador web, inicie sesión en su sitio de la compañía de Zoom como administrador.

1. Vaya a la sección **Manage Apps** (Administrar aplicaciones). 

    ![Administración de aplicaciones](./media/easysso-for-bamboo-tutorial/bamboo-admin-1.png)

2. En el lado izquierdo, busque **EasySSO** y haga clic en él.

    ![EasySSO](./media/easysso-for-bamboo-tutorial/bamboo-admin-2.png)

3. Seleccione la opción **SAML**. Esto le llevará a la sección de configuración de SAML.

    ![SAML](./media/easysso-for-bamboo-tutorial/bamboo-admin-3.png)

4. Seleccione la pestaña **Certificados** en la parte superior y aparecerá la siguiente pantalla:

    ![URL de metadatos](./media/easysso-for-bamboo-tutorial/bamboo-admin-4.png)

5. Ahora, busque el **certificado (Base64)** o el **archivo de metadatos** que ha guardado en los pasos anteriores de configuración del **inicio de sesión único de Azure AD**. Dispone de las siguientes opciones para continuar:

    a. Use el **archivo de metadatos** de federación de la aplicación que descargó en un archivo local del equipo. Seleccione el botón de radio **Upload** (Cargar) y siga el cuadro de diálogo de carga de archivos específico del sistema operativo.

    **OR**

    b. Abra el **archivo de metadatos** de federación de la aplicación para ver el contenido del archivo (en cualquier editor de texto sin formato) y cópielo en el Portapapeles. Seleccione la opción **Input** (Entrada) y pegue el contenido del Portapapeles en el campo de texto.
 
    **OR**

    c.  Configuración totalmente manual. Abra el **certificado (Base 64)** de federación de la aplicación para ver el contenido del archivo (en cualquier editor de texto sin formato) y cópielo en el Portapapeles. Péguelo en el campo de texto **IdP Token Signing Certificates** (Certificado de firma de tokens del IdP). A continuación, vaya a la pestaña **General** y rellene los campos **POST Binding URL** (Dirección URL de enlace POST) y **Entity ID** (Identificador de entidad) con los valores respectivos de **Dirección URL de inicio de sesión** e **Identificador de Azure AD** que guardó anteriormente.
 
6. Haga clic en el botón **Save** (Guardar) en la parte inferior de la página. Verá que el contenido de los archivos de metadatos o del certificado se analiza en los campos de configuración. La configuración de EasySSO for Bamboo se ha completado.

7. Para obtener la mejor experiencia de prueba, vaya a la pestaña **Look & Feel** (Apariencia y aspecto) y active la opción **SAML Login Button** (Botón de inicio de sesión de SAML). De esta forma, habilitará un botón independiente en la pantalla de inicio de sesión de Bamboo para probar específicamente la integración de SAML de Azure AD de un extremo a otro. También puede dejar este botón activo y configurar su selección de ubicación, color y traducción para el modo de producción.

    ![Look & Feel (Apariencia y aspecto)](./media/easysso-for-bamboo-tutorial/bamboo-admin-5.png)

    > [!NOTE]
    > Si tiene algún problema, póngase en contacto con el [equipo de soporte técnico de EasySSO](mailto:support@techtime.co.nz).

### <a name="create-easysso-for-bamboo-test-user"></a>Creación de un usuario de prueba de EasySSO for Bamboo

En esta sección se crea una usuaria llamada Britta Simon en Bamboo. EasySSO for Bamboo admite el aprovisionamiento de usuarios Just-In-Time, que está **deshabilitado** de forma predeterminada. Para habilitar el aprovisionamiento de usuarios, debe marcar explícitamente la opción **Create user on successful login** (Crear usuario después de un inicio de sesión correcto) en la sección General de la configuración del complemento de EasySSO. Si el usuario no existe en Bamboo, se crea uno tras la autenticación.

Sin embargo, si no desea habilitar el aprovisionamiento automático de usuarios en el primer inicio de sesión del usuario, los usuarios deben existir en los directorios de usuarios de back-end para que la instancia de Bamboo los utilice, como LDAP o Atlassian Crowd.

![Aprovisionamiento de usuarios](./media/easysso-for-bamboo-tutorial/admin.png)

## <a name="test-sso"></a>Prueba de SSO 

### <a name="idp-initiated-workflow"></a>Flujo de trabajo iniciado por IDP

En esta sección probará la configuración de inicio de sesión único de Azure AD mediante Aplicaciones.

Al hacer clic en el icono de EasySSO for Bamboo en Aplicaciones, debería iniciar sesión automáticamente en la instancia de EasySSO for Bamboo para la que configurara el inicio de sesión único. Para más información acerca de Aplicaciones, consulte [Inicio de sesión e inicio de aplicaciones desde el portal Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

### <a name="sp-initiated-workflow"></a>Flujo de trabajo iniciado por SP

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el botón **SAML Login** (Inicio de sesión de SAML) de Bamboo.

![Inicio de sesión de SAML del usuario](./media/easysso-for-bamboo-tutorial/bamboo-admin-7.png)

En este escenario se supone que ha habilitado el **botón de inicio de sesión de SAML** en **Look & Feel** (Apariencia y aspecto) en la página de configuración de EasySSO de Bamboo (consulte más arriba). Abra la dirección URL de inicio de sesión de Bamboo en el modo incógnito del explorador para evitar interferencias con las sesiones existentes. Haga clic en el **botón de inicio de sesión de SAML** y se le redirigirá al flujo de autenticación de usuarios de Azure AD. Una vez completada correctamente, se le redirigirá de nuevo a la instancia de Bamboo como usuario autenticado mediante SAML.

Existe la posibilidad de que aparezca la siguiente pantalla después de que se le redirija de nuevo desde Azure AD.

![Pantalla de error de EasySSO](./media/easysso-for-bamboo-tutorial/bamboo-admin-8.png)

En este caso, tiene que seguir las [instrucciones de esta página]( https://techtime.co.nz/display/TECHTIME/EasySSO+How+to+get+the+logs#EasySSOHowtogetthelogs-RETRIEVINGTHELOGS) para acceder al archivo **atlassian-bamboo.log**. Los detalles del error estarán disponibles mediante el identificador de referencia que se encuentra en la página de error de EasySSO.

Si tiene algún problema con la síntesis de los mensajes de registro, póngase en contacto con el [equipo de soporte técnico de EasySSO](mailto:support@techtime.co.nz).

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado EasySSO for Bamboo, puede aplicar el control de sesión, que protege la información confidencial de la organización de la filtración y la infiltración en tiempo real. El control de sesión procede del acceso condicional. [Aprenda a aplicar el control de sesión con Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).