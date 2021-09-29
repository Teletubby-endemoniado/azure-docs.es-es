---
title: 'Tutorial: Integración del inicio de sesión único de Azure AD con F5 | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único (SSO) entre Azure Active Directory y F5.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: jeedes
ms.openlocfilehash: c8fb1afec6b065b127541b2def5d53f401210d8d
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124790745"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-f5"></a>Tutorial: Integración del inicio de sesión único (SSO) de Azure Active Directory con F5

En este tutorial, obtendrá información sobre cómo integrar F5 con Azure Active Directory (Azure AD). Al integrar F5 con Azure AD, puede hacer lo siguiente:

* Controlar en Azure AD quién tiene acceso a F5.
* Permitir que los usuarios inicien sesión automáticamente en F5 con sus cuentas de Azure AD.
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).

* Una suscripción habilitada para el inicio de sesión único (SSO) en F5.

* La implementación de la solución conjunta requiere la licencia siguiente:
    * F5 BIG-IP&reg; Best bundle (o)

    * Licencia independiente de F5 BIG-IP Access Policy Manager&trade; (APM)

    * Licencia del complemento F5 BIG-IP Access Policy Manager&trade; (APM) en una instalación de F5 BIG-IP&reg; Local Traffic Manager&trade; (LTM) ya existente.

    * Además de la licencia anterior, el sistema F5 también puede tener licencia para:

        * Una suscripción de filtrado de direcciones URL para usar la base de datos de categorías de URL.

        * Una suscripción a F5 IP Intelligence para detectar y bloquear a atacantes conocidos y tráfico malintencionado.

        * Un módulo de seguridad de hardware (HSM) de red para proteger y administrar claves digitales para la autenticación segura.

* El sistema F5 BIG-IP se aprovisiona con módulos de APM (LTM es opcional).

* Aunque es opcional, es muy recomendable implementar los sistemas F5 en un [grupo de dispositivos de sincronización/conmutación por error](https://techdocs.f5.com/content/techdocs/en-us/bigip-14-1-0/big-ip-device-service-clustering-administration-14-1-0.html) (S/F DG), que incluye el par activo en espera, con una dirección IP flotante para la alta disponibilidad (HA). Se puede lograr una mayor redundancia de la interfaz mediante el protocolo de control de agregación de vínculos (LACP). LACP administra las interfaces físicas conectadas como una sola interfaz virtual (grupo agregado) y detecta cualquier error de interfaz dentro del grupo.

* En el caso de las aplicaciones Kerberos, una cuenta de servicio de AD local para la delegación restringida.  Consulte la [Documentación de F5](https://support.f5.com/csp/article/K43063049) para crear una cuenta de delegación de AD.

## <a name="access-guided-configuration"></a>Configuración de acceso guiado

* La configuración de acceso guiado es compatible con F5 TMOS versión 13.1.0.8 y posteriores. Si el sistema BIG-IP ejecuta una versión anterior a la 13.1.0.8, consulte la sección **Configuración avanzada**.

* La configuración de acceso guiado presenta una experiencia de usuario completamente nueva y simplificada. Esta arquitectura basada en el flujo de trabajo proporciona pasos de configuración intuitivos y reentrantes adaptados a la topología seleccionada.

* Antes de continuar con la configuración, actualice la configuración guiada; para ello, descargue el último paquete de casos de uso desde [downloads.f5.com](https://login.f5.com/resource/login.jsp?ctx=719748). Para actualizar, siga el procedimiento siguiente.

    >[!NOTE]
    >Las capturas de pantalla siguientes corresponden a la versión más reciente (BIG-IP 15.0 con AGC versión 5.0). Los pasos de configuración siguientes son válidos para este caso de uso desde la versión 13.1.0.8 hasta la última versión de BIG-IP.

1. En la interfaz de usuario web de F5 BIG-IP, haga clic en **Access >> Guided Configuration** (Acceso >> Configuración guiada).

2. En la página **Guided Configuration** (Configuración guiada), haga clic en **Upgrade Guided Configuration** (Actualizar configuración guiada) en la esquina superior izquierda.

    ![Captura de pantalla que muestra la página "Guided Configuration" (Configuración guiada) con la acción "Upgrade Guided Configuration" (Actualizar configuración guiada) seleccionada.](./media/kerbf5-tutorial/configure14.png) 

3. En la pantalla emergente Upgrade Guided Configuration (Actualizar configuración guiada), seleccione **Choose File** (Elegir archivo) para cargar el paquete de casos de uso descargado y haga clic en el botón **Upload and Install** (Cargar e instalar).

    ![Captura de pantalla que muestra la pantalla emergente "Upgrade Guided Configuration" (Actualizar configuración guiada) con las opciones "Choose File" (Elegir archivo) y "Upload and Install" (Cargar e instalar) seleccionadas.](./media/kerbf5-tutorial/configure15.png) 

4. Cuando se complete la actualización, haga clic en el botón **Continue** (Continuar).

    ![Captura de pantalla que muestra el cuadro de diálogo "Guided Configuration update is complete" (Se ha completado la actualización de la configuración guiada) y el botón "Continue" (Continuar) seleccionado.](./media/kerbf5-tutorial/configure16.png)

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* F5 admite el inicio de sesión único iniciado por **SP e IDP**.
* El inicio de sesión único de F5 se puede configurar de tres maneras diferentes:

- [Configuración del inicio de sesión único de F5 para la aplicación Kerberos](#configure-f5-single-sign-on-for-kerberos-application)

- [Configuración del inicio de sesión único de F5 para la aplicación basada en encabezados](headerf5-tutorial.md)

- [Configuración del inicio de sesión único de F5 para la aplicación Kerberos avanzada](advance-kerbf5-tutorial.md)

### <a name="key-authentication-scenarios"></a>Escenarios de autenticación de claves

Además de la compatibilidad de la integración nativa de Azure Active Directory con protocolos de autenticación modernos, como Open ID Connect, SAML y WS-Fed, F5 amplía el acceso seguro para aplicaciones de autenticación heredadas para el acceso interno y externo con Azure AD, lo que permite escenarios modernos (por ejemplo, el acceso sin contraseña) para estas aplicaciones. Esto incluye:

* Aplicaciones de autenticación basadas en encabezados

* Aplicaciones de autenticación Kerberos

* Autenticación anónima o aplicaciones sin autenticación integrada

* Aplicaciones de autenticación NTLM (protección con dos preguntas para el usuario)

* Aplicaciones basadas en formularios (protección con dos preguntas para el usuario)

## <a name="adding-f5-from-the-gallery"></a>Adición de F5 desde la galería

Para configurar la integración de F5 en Azure AD, será preciso agregar F5 desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **F5** en el cuadro de búsqueda.
1. Seleccione **F5** en el panel de resultados y agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on-for-f5"></a>Configuración y prueba del inicio de sesión único de Azure AD para F5

Configure y pruebe el inicio de sesión único de Azure AD con F5 mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de F5.

Para configurar y probar el inicio de sesión único de Azure AD con F5, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
    1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
    1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para habilitar a B.Simon para que use el inicio de sesión único de Azure AD.
1. **[Configuración del inicio de sesión único de F5](#configure-f5-sso)** , para configurar los valores de Inicio de sesión único en la aplicación.
    1. **[Creación de un usuario de prueba de F5](#create-f5-test-user)** , para tener un homólogo de B.Simon en F5 que esté vinculado a su representación en Azure AD.
1. **[Prueba del inicio de sesión único](#test-sso)** : para comprobar si la configuración funciona.

## <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **F5**, busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, elija **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

1. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, escriba los valores de los siguientes campos:

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<YourCustomFQDN>.f5.com/`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<YourCustomFQDN>.f5.com/`

1. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<YourCustomFQDN>.f5.com/`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de F5](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y **Certificado (Base 64)** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Set up F5** (Configurar F5), copie las direcciones URL adecuadas según sus necesidades.

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

En esta sección, va a permitir que B.Simon acceda a F5 mediante el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **F5**.
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.
1. Haga clic en **Acceso condicional**.
1. Haga clic en **Nueva directiva**.
1. Ahora puede ver la aplicación F5 como un recurso para la directiva de acceso condicional y aplicar cualquier acceso condicional, como la autenticación multifactor, el control de acceso basado en dispositivo o la directiva de protección de identidades.

## <a name="configure-f5-sso"></a>Configuración del inicio de sesión único de F5

- [Configuración del inicio de sesión único de F5 para la aplicación basada en encabezados](headerf5-tutorial.md)

- [Configuración del inicio de sesión único de F5 para la aplicación Kerberos avanzada](advance-kerbf5-tutorial.md)

### <a name="configure-f5-single-sign-on-for-kerberos-application"></a>Configuración del inicio de sesión único de F5 para la aplicación Kerberos

### <a name="guided-configuration"></a>Configuración guiada

1. Abra una nueva ventana del explorador web e inicie sesión en el sitio de empresa de F5 (Kerberos) como administrador y haga lo siguiente:

1. Debe importar el certificado de metadatos en F5, se usará más adelante en el proceso de instalación.

1. Vaya a **System > Certificate Management > Traffic Certificate Management > SSL Certificate List** (Sistema > Administración de certificados > Administración de certificados de tráfico > Lista de certificados SSL). Seleccione **Import** (Importar) en la esquina derecha. Especifique un valor en **Certificate Name** (Nombre de certificado) (se hará referencia a él más adelante en la configuración). En **Certificate Source** (Origen del certificado), seleccione Upload file (Cargar archivo) y especifique el certificado descargado de Azure al configurar el inicio de sesión único de SAML. Haga clic en **Import**.

    ![Captura de pantalla que muestra la página "S S L Certificate/Key Source" (Origen de la clave o certificado S S L) con el cuadro "Certificate Name" (Nombre de certificado) resaltado y las opciones "Upload File" (Cargar archivo) y el botón "Import" (Importar) seleccionados.](./media/kerbf5-tutorial/configure01.png) 

1. Además, necesitará el **Certificado SSL para el nombre de host de la aplicación. Vaya a System > Certificate Management > Traffic Certificate Management > SSL Certificate List** (Sistema > Administración de certificados > Administración de certificados de tráfico > Lista de certificados SSL). Seleccione **Import** (Importar) en la esquina derecha. El campo **Import Type** (Tipo de importación) será **PKCS 12 (IIS)** . Especifique un valor para **Key Name** (Nombre de clave) (se hará referencia a él más adelante en la configuración) y especifique el archivo PFX. Especifique el campo **Password** (Contraseña) del archivo PFX. Haga clic en **Import**.

    >[!NOTE]
    >En el ejemplo, el nombre de la aplicación es `Kerbapp.superdemo.live` y se usa un certificado comodín; el nombre de clave es `WildCard-SuperDemo.live`.

    ![Captura de pantalla que muestra la página "S S L Certificate/Key Source" (Origen de la clave o certificado S S L) con los valores especificados y el botón "Import" (Importar) seleccionado.](./media/kerbf5-tutorial/configure02.png) 
 
1. Usaremos la experiencia guiada para configurar la federación de Azure AD y el acceso a las aplicaciones. Vaya a la página **Main** (Principal) de F5 BIG-IP y seleccione **Access > Guided Configuration > Federation > SAML Service Provider** (Acceso > Configuración guiada > Federación > Proveedor de servicios SAML). Haga clic en **Next** (Siguiente) y, a continuación, en **Next** (Siguiente) para comenzar la configuración.

    ![Captura de pantalla que muestra la página "Guided Configuration" (Configuración guiada) con el icono "Federation" (Federación) resaltado y la opción "S A M L Service Provider" (Proveedor de servicios de S A M L) seleccionada.](./media/kerbf5-tutorial/configure03.png) 

    ![Captura de pantalla que muestra la página "Guided Configuration - S A M L Service Provide" (Configuración guiada: proveedor de servicios de S A M L) con el botón "Next" (Siguiente) seleccionado.](./media/kerbf5-tutorial/configure04.png)

1. Proporcione un valor para **Configuration Name** (Nombre de configuración). Especifique el valor de **Entity ID** (Identificador de entidad) (el mismo que configuró en la configuración de la aplicación de Azure AD). Especifique el campo **Host name** (Nombre de host). Agregue un valor para **Description** (Descripción) como referencia. Acepte las entradas predeterminadas restantes y seleccione y, a continuación, haga clic en **Save & Next** (Guardar y siguiente).

    ![Captura de pantalla que muestra el elemento "Service Provider Properties" (Propiedades del proveedor de servicios) con los cuadros de texto "Host name" (Nombre de host) y "Description" (Descripción) resaltados y el botón "Guardar y siguiente" (Save & Next) seleccionado.](./media/kerbf5-tutorial/configure05.png) 

1. En este ejemplo, vamos a crear un nuevo servidor virtual como 192.168.30.200 con el puerto 443. Especifique la dirección IP del servidor virtual en el campo **Destination Address** (Dirección de destino). Seleccione el campo **SSL Profile** (Perfil SSL) del cliente y seleccione Create new (Crear nuevo). Especifique el certificado de aplicación cargado previamente (el certificado comodín en este ejemplo) y la clave asociada y, a continuación, haga clic en **Save & Next** (Guardar y siguiente).

    >[!NOTE]
    >En este ejemplo, el servidor web interno se ejecuta en el puerto 80 y queremos publicarlo en el 443.

    ![Captura de pantalla que muestra la página "Virtual Server Properties" (Propiedades del servidor virtual) con el cuadro de texto "Destination Address" (Dirección de destino) resaltado y el botón "Save & Next" (Guardar y siguiente) seleccionado.](./media/kerbf5-tutorial/configure06.png)

1. En **Select method to configure your IdP connector** (Seleccionar el método para configurar el conector del proveedor de identidades), especifique Metadata (Metadatos), haga clic en Choose File (Elegir archivo) y cargue el archivo XML de metadatos que descargó antes desde Azure AD. Especifique un campo **Name** (Nombre) único para el conector del proveedor de identidades de SAML. Elija el **Certificado de firma de metadatos** que cargó anteriormente. Haga clic en **Save & Next** (Guardar y siguiente).

    ![Captura de pantalla que muestra la página "External Identity Provider Connector Settings" (Configuración del conector del proveedor de identidades externo) con el cuadro de texto "Name" (Nombre) resaltado y el botón "Save & Next" (Guardar y siguiente) seleccionado.](./media/kerbf5-tutorial/configure07.png)  

1. En **Select a Pool** (Seleccionar un grupo), especifique **Create New** (Crear nuevo) (también puede seleccionar un grupo que ya existe). Deje los otros valores como predeterminados.    En Pool Servers (Servidores de grupo), escriba la dirección IP en **IP Address/Node Name** (Dirección IP/Nombre de nodo). Especifique el campo **Port** (Puerto). Haga clic en **Save & Next** (Guardar y siguiente).
 
    ![Captura de pantalla que muestra la página "Pool Properties" (Propiedades del grupo) con los cuadros de texto "IP Address/Node Name" (Dirección IP o nombre de nodo) y "Port" (Puerto) resaltados y el botón "Save & Next" (Guardar y siguiente) seleccionado.](./media/kerbf5-tutorial/configure08.png)

1. En la pantalla Single Sign-On Settings (Configuración de inicio de sesión único), seleccione **Enable Single Sign-On** (Habilitar inicio de sesión único). En **Selected Single Sign-On Type** (Tipo de inicio de sesión único seleccionado), elija **Kerberos**. Reemplace **session.saml.last.Identity** por **session.saml.last.attr.name.Identity** en **Username Source** (esta variable se establece mediante la asignación de notificaciones en Azure AD). Seleccione **Show Advanced Setting** (Mostrar opciones avanzadas). En **Kerberos Realm** (Dominio kerberos), escriba el nombre de dominio. En **Account Name/ Account Password** (Nombre y contraseña de cuenta) especifique la cuenta y la contraseña de delegación de APM. Especifique la dirección IP del controlador de dominio en el campo **KDC**. Haga clic en **Save & Next** (Guardar y siguiente).

    ![Captura de pantalla que muestra la página "Single Sign-On Settings" (Configuración de inicio de sesión único) con cuadros de texto resaltados y el botón "Save & Next" (Guardar y siguiente) seleccionado.](./media/kerbf5-tutorial/configure09.png)   

1. Para los fines de esta guía, se omitirán las comprobaciones del punto de conexión.  Consulte la documentación de F5 para más información.  En la pantalla, seleccione **Save & Next** (Guardar y siguiente).

    ![Captura de pantalla que muestra la página "Endpoint Checks Properties" (Propiedades de las comprobaciones del punto de conexión) y el botón "Save & Next" (Guardar y siguiente) seleccionado.](./media/kerbf5-tutorial/configure10.png) 

1. Acepte los valores predeterminados y haga clic en **Save & Next** (Guardar y siguiente). Consulte la documentación de F5 para más información sobre la configuración de la administración de sesiones de SAML.


    ![Captura de pantalla que muestra la página "Timeout Settings" (Configuración del tiempo de expiración) con el botón "Save & Next" (Guardar y siguiente) seleccionado.](./media/kerbf5-tutorial/configure11.png) 
 
1. Revise la pantalla de resumen y seleccione **Deploy** (Implementar) para configurar BIG-IP.
 
    ![Captura de pantalla que muestra la página "Your application is ready to be deployed" (La aplicación está lista para la implementación) con la sección "Summary" (Resumen) resaltada y el botón "Deploy" (Implementar) seleccionado.](./media/kerbf5-tutorial/configure12.png)

1. Una vez configurada la aplicación, haga clic en **Finish** (Finalizar).

    ![Captura de pantalla que muestra la página "Your application is deployed" (La aplicación se ha implementado) con el botón "Finish" (Finalizar) seleccionado.](./media/kerbf5-tutorial/configure13.png)

## <a name="advanced-configuration"></a>Configuración avanzada

>[!NOTE]
>Como referencia, haga clic [aquí](https://techdocs.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-single-sign-on-11-5-0/2.html).

### <a name="configuring-an-active-directory-aaa-server"></a>Configuración de un servidor de AAA de Active Directory

Configure un servidor de AAA de Active Directory en el Administrador de directivas de acceso (APM) para especificar los controladores de dominio y las credenciales que usará APM para autenticar a los usuarios.

1. En la pestaña Main (Principal), haga clic en **Access Policy > AAA Servers > Active Directory** (Directiva de acceso > Servidores de AAA > Active Directory). Se abre la pantalla con la lista de servidores de Active Directory.

2. Haga clic en **Crear**. Se abre la pantalla New Server properties (Propiedades del nuevo servidor).

3. En el campo **Name** (Nombre), escriba un nombre único para el servidor de autenticación.

4. En el campo **Domain Name** (Nombre de dominio), escriba el nombre del dominio de Windows.

5. Para la configuración de **Server Connection** (Conexión del servidor), seleccione una de estas opciones:

   * Seleccione **Use Pool** (Usar grupo) para configurar la alta disponibilidad del servidor de AAA.

   * Seleccione **Direct** (Directo) para configurar el servidor de AAA para la funcionalidad independiente.

6. Si ha seleccionado **Direct** (Directo), escriba un nombre en el campo **Domain Controller** (Controlador de dominio).

7. Si ha seleccionado **Use Pool** (Usar grupo), configure el grupo:

   * Escriba un nombre en el campo **Domain Controller Pool Name** (Nombre del grupo del controlador de dominio).

   * Especifique los **controladores de dominio** del grupo; para ello, escriba la dirección IP y el nombre de host de cada uno y haga clic en el botón **Add** (Agregar).

   * Para supervisar el estado del servidor de AAA, tiene la opción de seleccionar un monitor de estado: en este caso, solo es adecuado el monitor **gateway_icmp**; puede seleccionarlo en la lista **Server Pool Monitor** (Monitor del grupo de servidores).

8. En el campo **Admin Name** (Nombre del administrador), escriba un nombre con distinción entre mayúsculas y minúsculas para un administrador que tenga permisos administrativos en Active Directory. APM usa la información de los campos **Admin Name** (Nombre del administrador) y **Admin Password** (Contraseña del administrador) para la consulta de AD. Si Active Directory está configurado para consultas anónimas, no es necesario proporcionar un nombre de administrador. De lo contrario, APM necesita una cuenta con privilegios suficientes para enlazar a un servidor de Active Directory, capturar información del grupo de usuarios y capturar las directivas de contraseñas de Active Directory para admitir la funcionalidad relacionada con la contraseña. (APM debe capturar directivas de contraseñas, por ejemplo, si selecciona la opción Preguntar al usuario si desea cambiar la contraseña antes de la expiración en una acción de consulta de AD). Si no proporciona la información de la cuenta de administrador en esta configuración, APM usa la cuenta de usuario para capturar la información. Esto funciona si la cuenta de usuario tiene privilegios suficientes.

9. En el campo **Admin Password** (Contraseña del administrador), escriba la contraseña del administrador asociada con el nombre de dominio.

10. En el campo **Verify Admin Password** (Verificar contraseña del administrador), vuelva a escribir la contraseña del administrador asociada con la configuración del **nombre de dominio**.

11. En el campo **Group Cache Lifetime** (Duración de la memoria caché de grupo), escriba el número de días. La duración predeterminada es de 30 días.

12. En el campo **Password Security Object Cache Lifetime** (Duración de la memoria caché del objeto de seguridad de contraseñas), escriba el número de días. La duración predeterminada es de 30 días.

13. En la lista **Kerberos Preauthentication Encryption Type** (Tipo de cifrado de la autenticación previa de Kerberos), seleccione un tipo de cifrado. El valor predeterminado es **None** (Ninguno). Si especifica un tipo de cifrado, el sistema BIG-IP incluye los datos de la autenticación previa de Kerberos en el primer paquete de solicitud del servicio de autenticación (AS-REQ).

14. En el campo **Timeout** (Tiempo de expiración), escriba un intervalo de tiempo de espera (en segundos) para el servidor de AAA. (Esta configuración es opcional).

15. Haga clic en **Finished** (Finalizado). El nuevo servidor se muestra en la lista. Esto agrega el nuevo servidor de Active Directory a la lista servidores de Active Directory.

    ![Captura de pantalla que muestra las secciones "General Properties" (Propiedades generales) y "Configuration" (Configuración).](./media/kerbf5-tutorial/configure17.png)

### <a name="saml-configuration"></a>Configuración de SAML

1. Debe importar el certificado de metadatos en F5, se usará más adelante en el proceso de instalación. Vaya a **System > Certificate Management > Traffic Certificate Management > SSL Certificate List** (Sistema > Administración de certificados > Administración de certificados de tráfico > Lista de certificados SSL). Seleccione **Import** (Importar) en la esquina derecha.

    ![Captura de pantalla que muestra la página "Import S S L Certificate/Key Source" (Importar origen de la clave o certificado S S L) con el botón "Import" (Importar) seleccionado.](./media/kerbf5-tutorial/configure18.png)

2. Para configurar el IDP de SAML, **vaya a Access > Federation > SAML: Service Provider > External Idp Connectors** (Acceso > Federación > SAML: Proveedor de servicios > Conectores de IDP externos) y haga clic en **Create > From Metadata** (Crear > Desde metadatos).

    ![Captura de pantalla que muestra la página "S A M L Service Provider" (Proveedor de servicios de S A M L) con la opción "From Metadata" (Desde metadatos) seleccionada en la lista desplegable "Create" (Crear).](./media/kerbf5-tutorial/configure19.png)

    ![Captura de pantalla que muestra el cuadro de diálogo "Create New S A M L I d P Connector" (Crear nuevo conector de I d P de S A M L).](./media/kerbf5-tutorial/configure20.png)

    ![Captura de pantalla que muestra la ventana "Edit S A M L I d P Connector" (Editar conector de I d P de S A M L) con la opción "General Settings" (Configuración general) seleccionada.](./media/kerbf5-tutorial/configure21.png)

    ![Captura de pantalla que muestra la ventana "Edit S A M L I d P Connector" (Editar conector de I d P de S A M L) con la opción "Single Sign On Service Settings" (Configuración del servicio de inicio de sesión único) seleccionada.](./media/kerbf5-tutorial/configure22.png)

    ![Captura de pantalla que muestra la ventana "Edit S A M L I d P Connector" (Editar conector de I d P de S A M L) con la opción "Security Settings" (Configuración de seguridad) seleccionada.](./media/kerbf5-tutorial/configure23.png)

    ![Captura de pantalla que muestra la ventana "Edit S A M L I d P Connector" (Editar conector de I d P de S A M L) con la opción "S L O Service Settings" (Configuración del servicio de cierre de sesión único) seleccionada.](./media/kerbf5-tutorial/configure24.png)

1. Para configurar el SP de SAML, vaya a **Access > Federation > SAML Service Provider > Local SP Services** (Acceso > Federación > Proveedor de servicios de SAML > Servicios del SP local) y haga clic en **Create** (Crear). Complete la siguiente información y haga clic en **OK** (Aceptar).

    * Nombre de tipo: KerbApp200SAML
    * Identificador de entidad*: https://kerb-app.com.cutestat.com
    * Configuración del nombre del SP
    * Esquema: https
    * Host: kerbapp200.superdemo.live
    * Descripción: kerbapp200.superdemo.live

     ![Captura de pantalla que muestra la ventana "Edit S A M L S P Service" (Editar servicio del S P de S A M L) con la opción "General Settings" (Configuración general) seleccionada.](./media/kerbf5-tutorial/configure25.png)

     b. Seleccione la configuración de SP, KerbApp200SAML y haga clic en **Bind/UnBind IdP Connectors** (Enlazar/Desenlazar conectores del IdP).

     ![Captura de pantalla que muestra la página "S A M L Service Provider - Local S P Services" (Proveedor de servicios de S A M L: servicios del S P local) con la opción "KerbApp200SAML" seleccionada.](./media/kerbf5-tutorial/configure26.png)

     ![Captura de pantalla que muestra el botón "Bind/Unbind I d P Connectors" (Enlazar/Desenlazar conectores del I d P) seleccionado.](./media/kerbf5-tutorial/configure27.png)

     c. Haga clic en **Add New Row** (Agregar nueva fila) y seleccione el **Conector del IdP externo** creado en el paso anterior, haga clic en **Update**(Actualizar) y, a continuación, haga clic en **OK** (Aceptar).

     ![Captura de pantalla que muestra la ventana "Edit S A M L I d Ps that use this S P" (Editar los I d P de S A M L que usan este S P) con el botón "Add New Row" (Agregar nueva fila) seleccionado.](./media/kerbf5-tutorial/configure28.png)

1. Para configurar el SSO de Kerberos, vaya a **Access > Single Sign-on > Kerberos** (Acceso > Inicio de sesión único > Kerberos), complete la información y haga clic en **Finished** (Finalizado).

    >[!Note]
    > Necesitará que la cuenta de delegación de Kerberos se cree y se especifique. Consulte la sección KCD (consulte el apéndice para las referencias de variables).

    * **Username Source** (Origen del nombre de usuario): session.saml.last.attr.name.http:\//schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname

    * **User Realm Source** (Origen del dominio del usuario): session.logon.last.domain

        ![Captura de pantalla que muestra la página "Single Sign-On - Properties" (Inicio de sesión único: propiedades) con los cuadros de texto "Username Source" (Origen del nombre de usuario) y "User Realm Source" (Origen del dominio del usuario) resaltados.](./media/kerbf5-tutorial/configure29.png)

1. Para configurar el perfil de acceso, vaya a **Access > Profile/Policies > Access Profile** (Acceso > Perfil/Directivas > Perfil de acceso) (por directivas de sesión), haga clic en **Create** (Crear), complete la siguiente información y haga clic en **Finished** (Finalizado).

    * Nombre: KerbApp200
    * Tipo de perfil: All
    * Ámbito del perfil: Perfil
    * Lenguajes: Inglés

        ![Captura de pantalla que muestra la página "Profiles/Policies - Properties" (Perfil/Directivas: propiedades) con los cuadros de texto "Name" (Nombre), "Profile Type" (Tipo de perfil) y "Languages" (Idiomas) resaltados.](./media/kerbf5-tutorial/configure30.png)

1. Haga clic en el nombre, KerbApp200, complete la siguiente información y haga clic en **Update** (Actualizar).

    * Cookie de dominio: superdemo.live
    * Configuración de SSO: KerAppSSO_sso

        ![Captura de pantalla que muestra la página "S S O/Auth Domains" (S S O/Dominios de autenticación) con el cuadro de texto "Domain Cookie" (Cookie de dominio) y la lista desplegable "S S O Configuration" (Configuración de S S O) resaltados y el botón "Update" (Actualizar) seleccionado.](./media/kerbf5-tutorial/configure31.png)

1. Haga clic en **Access Policy** (Directiva de acceso) y, a continuación, en **Edit Access Policy** (Editar directiva de acceso) para el perfil "KerbApp200".

    ![Captura de pantalla que muestra la página "Access Policy" (Directiva de acceso) con la acción "Edit Access Policy" (Editar directiva de acceso) para el perfil KerbApp200 seleccionada.](./media/kerbf5-tutorial/configure32.png)

    ![Captura de pantalla que muestra la página "Access Policy" (Directiva de acceso) y el cuadro de diálogo "S A M L Authentication S P" (S P de autenticación de S A M L).](./media/kerbf5-tutorial/configure33.png)

    ![Captura de pantalla que muestra la página "Access Policy" (Directiva de acceso) y el cuadro de diálogo "Variable Assign" (Asignación de variables) con los cuadros de texto "Assignment" (Asignación) resaltados.](./media/kerbf5-tutorial/configure34.png)

    * **session.logon.last.usernameUPN   expr {[mcget {session.saml.last.identity}]}**

    * **session.ad.lastactualdomain  TEXT superdemo.live**

        ![Captura de pantalla que muestra la página "Access Policy" (Directiva de acceso) y el cuadro de diálogo "Active Directory" con el cuadro de texto "SearchFilter" (Filtro de búsqueda) resaltado.](./media/kerbf5-tutorial/configure35.png)

    * **(userPrincipalName=%{session.logon.last.usernameUPN})**

        ![Captura de pantalla que muestra la página "Access Policy" (Directiva de acceso) con el cuadro de diálogo "A D Query - Branch Rules" (Consulta de A D: reglas de rama).](./media/kerbf5-tutorial/configure36.png)

        ![Captura de pantalla que muestra los cuadros de texto "Custom Variable" (Variable personalizada) y "Custom Expression" (Expresión personalizada) resaltados.](./media/kerbf5-tutorial/configure37.png)

    * **session.logon.last.username  expr { "[mcget {session.ad.last.attr.sAMAccountName}]" }**

        ![Captura de pantalla que muestra el cuadro de texto "Username from Logon Page" (Nombre de usuario de la página de inicio de sesión) resaltado.](./media/kerbf5-tutorial/configure38.png)

    * **mcget {session.logon.last.username}**
    * **mcget {session.logon.last.password**

1. Para agregar un nuevo nodo, vaya a **Local Traffic > Nodes > Node List (Tráfico local > Nodos > Lista de nodos), haga clic en Create (Crear)** , complete la siguiente información y, a continuación, haga clic en **Finished** (Finalizado).

    * Nombre: KerbApp200
    * Descripción: KerbApp200
    * Dirección: 192.168.20.200

        ![Captura de pantalla que muestra la página "New Node" (Nuevo nodo) con los cuadros de texto "Name" (Nombre), "Description" (Descripción) y "Address" (Dirección) resaltados, y el botón "Finished" (Finalizado) seleccionado.](./media/kerbf5-tutorial/configure39.png)

1. Para crear un nuevo grupo, vaya a **Local Traffic > Pools > Pool List (Tráfico local > Grupos > Lista de grupos), haga clic en Create (Crear)** , complete la siguiente información y, a continuación, haga clic en **Finished** (Finalizado).

    * Nombre: KerbApp200-Pool
    * Descripción: KerbApp200-Pool
    * Monitores de estado: http
    * Dirección: 192.168.20.200
    * Puerto de servicio: 81

        ![Captura de pantalla que muestra la página "New Pool" (Nuevo grupo) con los valores especificados y el botón "Finished" (Finalizado) seleccionado.](./media/kerbf5-tutorial/configure40.png)

1. Para crear un servidor virtual, vaya a **Local Traffic > Virtual Servers > Virtual Server List > +** (Tráfico local > Servidores virtuales > Lista de servidores virtuales > +), complete la siguiente información y haga clic en **Finished** (Finalizado).

    * Nombre: KerbApp200
    * Dirección de destino/Máscara: Host 192.168.30.200
    * Puerto de servicio: Puerto 443 HTTPS
    * Perfil de acceso: KerbApp200
    * Especifique el perfil de acceso creado en el paso anterior.

        ![Captura de pantalla que muestra la página "Virtual Server List" (Lista de servidores virtuales) con los cuadros de texto "Name" (Nombre), "Destination Address/Mask" (Dirección de destino y máscara) y "Service Port" (Puerto de servicio) resaltados.](./media/kerbf5-tutorial/configure41.png)

        ![Captura de pantalla que muestra la página "Virtual Server List" (Lista de servidores virtuales) con la lista desplegable "Access Profile" (Perfil de acceso) resaltada.](./media/kerbf5-tutorial/configure42.png)

### <a name="setting-up-kerberos-delegation"></a>Configuración de la delegación Kerberos 

>[!NOTE]
>Como referencia, haga clic [aquí](https://www.f5.com/pdf/deployment-guides/kerberos-constrained-delegation-dg.pdf).

*  **Paso 1:** Creación de una cuenta de delegación

    **Ejemplo**:
    * Nombre de dominio: **superdemo.live**

    * Nombre de cuenta de SAM: **big-ipuser**

    * New-ADUser -Name "APM Delegation Account" -UserPrincipalName host/big-ipuser.superdemo.live@superdemo.live -SamAccountName "big-ipuser" -PasswordNeverExpires $true -Enabled $true -AccountPassword (Read-Host -AsSecureString "Password!1234")

* **Paso 2:** Establecer SPN (en la cuenta de delegación de APM)

    **Ejemplo**:
    * setspn –A **host/big-ipuser.superdemo.live** big-ipuser

* **Paso 3:** La delegación de SPN (para la cuenta de App Service) configura la delegación adecuada para la cuenta de delegación de F5.
    En el ejemplo siguiente, se configura para KCD la cuenta de delegación de APM para la aplicación FRP-App1.superdemo. live.

    ![Configuración de F5 (Kerberos)](./media/kerbf5-tutorial/configure43.png)

* Proporcione los detalles tal como se mencionó en el documento de referencia anterior en [esta documentación](https://techdocs.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-single-sign-on-11-5-0/2.html).

### <a name="create-f5-test-user"></a>Creación de un usuario de prueba de F5

En esta sección, creará un usuario llamado B.Simon en F5. Trabaje con el [equipo de soporte técnico del cliente de F5](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) para agregar los usuarios a la plataforma de F5. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único. 

## <a name="test-sso"></a>Prueba de SSO 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de F5 en el panel de acceso, debería iniciar sesión automáticamente en la versión de F5 para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](./tutorial-list.md)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [¿Qué es el acceso condicional en Azure Active Directory?](../conditional-access/overview.md)

- [Prueba de F5 con Azure AD](https://aad.portal.azure.com/)

- [Configuración del inicio de sesión único de F5 para la aplicación basada en encabezados](headerf5-tutorial.md)

- [Configuración del inicio de sesión único de F5 para la aplicación Kerberos avanzada](advance-kerbf5-tutorial.md)

- [Integración de F5 BIG-IP APM y Azure AD para el acceso híbrido seguro](../manage-apps/f5-aad-integration.md)

- [Tutorial para implementar la máquina virtual de F5 BIG-IP Virtual Edition en IaaS de Azure para el acceso híbrido seguro](../manage-apps/f5-bigip-deployment-guide.md)

- [Tutorial para la integración del inicio de sesión único de Azure Active Directory con F5 BIG-IP para una VPN sin contraseña](../manage-apps/f5-aad-password-less-vpn.md)