---
title: 'Federación con un proveedor de identidades (IdP) SAML o WS-Fed para B2B: Azure AD'
description: Federe directamente con un proveedor de identidades SAML o WS-Fed para que los invitados puedan iniciar sesión en sus aplicaciones de Azure AD.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 09/20/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f8a535f7c74e32e59918badbda7747f24a1756f
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128623995"
---
# <a name="federation-with-samlws-fed-identity-providers-for-guest-users-preview"></a>Federación con proveedores de identidades SAML o WS-Fed para usuarios invitados (versión preliminar)

> [!NOTE]
>- La *federación directa* de Azure Active Directory ahora se conoce como *federación con un proveedor de identidades (IdP) SAML o WS-Fed*.
>- La federación con un proveedor de identidades de SAML o WS-Fed es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se indica cómo configurar una federación con cualquier organización cuyo proveedor de identidades (IdP) admita el protocolo SAML 2.0 o WS-Fed. Al configurar una federación con el proveedor de identidades de un asociado, los nuevos usuarios invitados de ese dominio pueden utilizar su propia cuenta de organización administrada por el proveedor de identidades para iniciar sesión con su inquilino de Azure AD y empezar a colaborar con usted. No es necesario que el usuario invitado cree otra cuenta de Azure AD.

> [!IMPORTANT]
> - Se ha eliminado la limitación que exigía que el dominio de la dirección URL de autenticación coincidiera con el dominio de destino o procediera de un proveedor de identidades permitido. Para obtener detalles, vea [Paso 1: Determinación de si el asociado necesita actualizar sus registros de texto DNS](#step-1-determine-if-the-partner-needs-to-update-their-dns-text-records).
>-  Ahora se recomienda que el asociado establezca la audiencia del proveedor de identidades basado en SAML o WS-Fed en una audiencia de inquilinos. Vea las secciones siguientes de atributos y notificaciones necesarios de [SAML 2.0](#required-saml-20-attributes-and-claims) y [WS-Fed](#required-ws-fed-attributes-and-claims).

## <a name="when-is-a-guest-user-authenticated-with-samlws-fed-idp-federation"></a>¿Cuándo se autentica un usuario invitado con la federación con un proveedor de identidades SAML o WS-Fed?

Después de configurar la federación con el proveedor de identidades SAML o WS-Fed de una organización, cualquier nuevo usuario invitado se autenticará mediante ese proveedor de identidades. Es importante tener en cuenta que la configuración de la federación no cambia el método de autenticación para los usuarios invitados que ya han canjeado una invitación. Estos son algunos ejemplos:

 - Si los usuarios invitados ya han canjeado sus invitaciones y posteriormente configura la federación con el proveedor de identidades SAML o WS-Fed de su organización, esos usuarios invitados seguirán utilizando el mismo método de autenticación que utilizaron antes de configurar la federación.
 - Si se configura una federación con el proveedor de identidades SAML o WS-Fed de una organización y se invita a los usuarios y, después, la organización asociada se traslada a Azure AD, los usuarios invitados que ya han canjeado las invitaciones seguirán utilizando el proveedor de identidades SAML o WS-Fed federado, siempre y cuando exista la directiva de federación en el inquilino.
 - Si elimina la federación con el proveedor de identidades SAML o WS-Fed de una organización, los usuarios invitados que usen actualmente este proveedor de identidades no podrán iniciar sesión.

En cualquiera de estos casos, puede actualizar el método de autenticación de un usuario invitado mediante el [restablecimiento de su estado de canje](reset-redemption-status.md).

La federación con un proveedor de identidades SAML o WS-Fed está asociada a espacios de nombres de dominio, como contoso.com y fabrikam.com. Al establecer una federación con AD FS o un proveedor de identidades de terceros, las organizaciones asocian uno o más espacios de nombres de dominio a estos proveedores de identidades.

## <a name="end-user-experience"></a>Experiencia del usuario final 

Con la federación con un proveedor de identidades SAML o WS-Fed, los usuarios invitados inician sesión en el inquilino de Azure AD mediante su propia cuenta de la organización. Cuando acceden a los recursos compartidos y se les pide que inicien sesión, se redirige a los usuarios al proveedor de identidades. Después del inicio de sesión correcto, vuelven a Azure AD para acceder a los recursos. Los tokens de actualización de estos usuarios son válidos durante 12 horas, la [duración predeterminada del token de actualización de acceso directo](../develop/active-directory-configurable-token-lifetimes.md#configurable-token-lifetime-properties) en Azure AD. Si el proveedor de identidades federado tiene el inicio de sesión único habilitado, el usuario experimentará un inicio de sesión único y no verá ninguna solicitud de inicio de sesión después de la autenticación inicial.

## <a name="sign-in-endpoints"></a>Puntos de conexión de inicio de sesión

Ahora, los usuarios invitados de la federación con un proveedor de identidades SAML o WS-Fed pueden iniciar sesión en sus aplicaciones multiinquilino o en las aplicaciones propias de Microsoft utilizando un [punto de conexión común](redemption-experience.md#redemption-and-sign-in-through-a-common-endpoint) (en otras palabras, una dirección URL general de la aplicación que no incluya el contexto del inquilino). Durante el proceso de inicio de sesión, el usuario invitado elige **Opciones de inicio de sesión** y después selecciona **Sign in to an organization** (Iniciar sesión en una organización). Después, escribe el nombre de la organización y continúa iniciando sesión con sus propias credenciales.

Los usuarios invitados de la federación con un proveedor de identidades SAML o WS-Fed también pueden usar puntos de conexión de la aplicación que incluyan la información del inquilino; por ejemplo:

  * `https://myapps.microsoft.com/?tenantid=<your tenant ID>`
  * `https://myapps.microsoft.com/<your verified domain>.onmicrosoft.com`
  * `https://portal.azure.com/<your tenant ID>`

También puede proporcionar a los usuarios invitados un vínculo directo a una aplicación o recurso que incluya la información del inquilino; por ejemplo, `https://myapps.microsoft.com/signin/Twitter/<application ID?tenantId=<your tenant ID>`.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="can-i-set-up-samlws-fed-idp-federation-with-azure-ad-verified-domains"></a>¿Puedo configurar la federación con un IdP de SAML o WS-Fed con dominios comprobados de Azure AD?
No, bloqueamos la federación con un proveedor de identidades de SAML o WS-Fed para dominios comprobados de Azure AD en favor de las funcionalidades nativas de dominio administrado de Azure AD. Verá un error en Azure Portal o PowerShell si intenta configurar la federación con el proveedor de identidades de SAML o WS-Fed con un dominio comprobado por DNS en Azure AD.

### <a name="can-i-set-up-samlws-fed-idp-federation-with-a-domain-for-which-an-unmanaged-email-verified-tenant-exists"></a>¿Puedo configurar la federación con un proveedor de identidades SAML o WS-Fed con un dominio para el que existe un inquilino no administrado (verificado por correo electrónico)? 
Sí, puede configurar la federación con el proveedor de identidades de SAML o WS-Fed con dominios que no están comprobados por DNS en Azure AD, incluidos inquilinos no administrados (comprobados por correo electrónico o "virales") de Azure AD. Estos inquilinos se crean cuando un usuario canjea una invitación B2B o realiza un registro de autoservicio para Azure AD mediante un dominio que no existe actualmente. Si el dominio no se ha comprobado y el inquilino no ha experimentado una [adquisición de administración](../enterprise-users/domains-admin-takeover.md), puede configurar la federación con ese dominio.

### <a name="how-many-federation-relationships-can-i-create"></a>¿Cuántas relaciones de federación puedo crear?
Actualmente, se admite un máximo de 1000 relaciones de federación. Este límite incluye tanto las [federaciones internas](/powershell/module/msonline/set-msoldomainfederationsettings) como las federaciones con un proveedor de identidades SAML o WS-Fed.

### <a name="can-i-set-up-federation-with-multiple-domains-from-the-same-tenant"></a>¿Puedo configurar la federación con varios dominios del mismo inquilino?
Actualmente no se admite la federación con un proveedor de identidades SAML o WS-Fed con varios dominios del mismo inquilino.

### <a name="do-i-need-to-renew-the-signing-certificate-when-it-expires"></a>¿Es necesario renovar el certificado de firma cuando expire?
Si especifica la dirección URL de metadatos en la configuración del proveedor de identidades, Azure AD renovará automáticamente el certificado de firma cuando expire. Sin embargo, si el certificado se gira por cualquier razón antes de la hora de expiración, o si no proporciona una dirección URL de metadatos, Azure AD no podrá renovarlo. En este caso, deberá actualizar manualmente el certificado de firma.

### <a name="if-samlws-fed-idp-federation-and-email-one-time-passcode-authentication-are-both-enabled-which-method-takes-precedence"></a>Si la federación con un proveedor de identidades SAML o WS-Fed y la autenticación con código de acceso de un solo uso por correo electrónico están habilitadas, ¿qué método tiene prioridad?
Cuando se establece la federación con un proveedor de identidades SAML o WS-Fed con una organización asociada, esta federación tiene prioridad sobre la autenticación con código de acceso de un solo uso por correo electrónico para los nuevos usuarios invitados de esa organización. Si un usuario invitado ha canjeado una invitación mediante la autenticación de código de acceso de un solo uso antes de configurar la federación con un proveedor de identidades SAML o WS-Fed, seguirá utilizando la autenticación de código de acceso de un solo uso.

### <a name="does-samlws-fed-idp-federation-address-sign-in-issues-due-to-a-partially-synced-tenancy"></a>¿Se ocupa la federación con un proveedor de identidades SAML o WS-Fed de los problemas de inicio de sesión debidos a unos inquilinos parcialmente sincronizados?
No, la característica del [código de acceso de un solo uso por correo electrónico](one-time-passcode.md) se debe usar en este escenario. Un "inquilinato parcialmente sincronizado" se refiere a un inquilino de Azure AD asociado en el que las identidades de usuario locales no están completamente sincronizadas con la nube. Un invitado cuya identidad aún no existe en la nube pero que intenta canjear su invitación de B2B no podrá iniciar sesión. La característica del código de acceso de un solo uso permitiría a este invitado iniciar sesión. La función de federación con un proveedor de identidades SAML o WS-Fed permite afrontar escenarios en los que el invitado tiene su propia cuenta de organización administrada por el proveedor de identidades, pero la organización no tiene ninguna presencia en Azure AD.

### <a name="once-samlws-fed-idp-federation-is-configured-with-an-organization-does-each-guest-need-to-be-sent-and-redeem-an-individual-invitation"></a>Una vez configurada la federación con un proveedor de identidades SAML o WS-Fed con una organización, ¿es necesario enviar cada invitado y canjear una invitación individual?
La configuración de la federación con un proveedor de identidades SAML o WS-Fed no cambia el método de autenticación para los usuarios invitados que ya han canjeado una invitación. Puede actualizar el método de autenticación de un usuario invitado mediante el [restablecimiento de su estado de canje](reset-redemption-status.md).

### <a name="is-there-a-way-to-send-a-signed-request-to-the-saml-identity-provider"></a>¿Hay alguna manera de enviar una solicitud firmada al proveedor de identidades de SAML?
Actualmente, la característica de federación de SAML o WS-Fed para Azure AD no admite el envío de un token de autenticación firmado al proveedor de identidades de SAML.

## <a name="step-1-determine-if-the-partner-needs-to-update-their-dns-text-records"></a>Paso 1: Determinación de si el asociado necesita actualizar sus registros de texto DNS

Según el proveedor de identidades del asociado, es posible que este tenga que actualizar sus registros DNS para habilitarle la federación. Siga estos pasos para determinar si se necesitan actualizaciones DNS.

1. Si el proveedor de identidades del asociado es uno de los permitidos, no se necesitan cambios de DNS (esta lista está sujeta a cambios):

     - accounts.google.com
     - pingidentity.com
     - login.pingone.com
     - okta.com
     - oktapreview.com
     - okta-emea.com
     - my.salesforce.com
     - federation.exostar.com
     - federation.exostartest.com
     - idaptive.app
     - idaptive.qa

2. Si el proveedor de identidades no es uno de los permitidos indicados en el paso anterior, compruebe su dirección URL de autenticación para ver si el dominio coincide con el dominio de destino o un host dentro del dominio de destino. Es decir, al configurar la federación para `fabrikam.com`:

     - Si la dirección URL de autenticación es `https://fabrikam.com` o `https://sts.fabrikam.com/adfs` (un host del mismo dominio), no se necesita ningún cambio de DNS.
     - Si la dirección URL de autenticación es `https://fabrikamconglomerate.com/adfs` o  `https://fabrikam.com.uk/adfs`, el dominio no coincide con el dominio fabrikam.com, por lo que el asociado tiene que agregar un registro de texto para la dirección URL de autenticación a su configuración DNS.

    > [!IMPORTANT]
    > Hay un problema conocido con el paso siguiente. Actualmente, la adición de un registro de texto DNS al dominio del IdP de federación no desbloquea la autenticación. Estamos trabajando activamente para solucionar este problema.

3. Si se necesitan cambios de DNS en función del paso anterior, pida al asociado que agregue un registro TXT a los registros DNS de su dominio, como en el ejemplo siguiente:

   `fabrikam.com.  IN   TXT   DirectFedAuthUrl=https://fabrikamconglomerate.com/adfs`

## <a name="step-2-configure-the-partner-organizations-idp"></a>Paso 2: Configuración del proveedor de identidades de la organización asociada

A continuación, la organización asociada debe configurar su proveedor de identidades con las notificaciones necesarias y las confianzas de los usuarios de confianza.

> [!NOTE]
> Para ilustrar cómo configurar un proveedor de identidades para la federación con un proveedor de identidades SAML o WS-Fed, usaremos los Servicios de federación de Active Directory (AD FS) como ejemplo. Consulte el artículo [Configuración de la federación con un proveedor de identidades SAML o WS-Fed con AD FS](direct-federation-adfs.md), que ofrece ejemplos de cómo configurar AD FS como un proveedor de identidades de SAML 2.0 o WS-Fed en preparación para la federación.

### <a name="saml-20-configuration"></a>Configuración de SAML 2.0

Azure AD B2B se puede configurar para federarse con proveedores de identidades que usan el protocolo SAML con los requisitos específicos que se indican a continuación. Para más información sobre cómo configurar una confianza entre su proveedor de identidades SAML y Azure AD, consulte [Uso de un proveedor de identidades (IdP) de SAML 2.0 para el inicio de sesión único](../hybrid/how-to-connect-fed-saml-idp.md).  

> [!NOTE]
> El dominio de destino para la federación con un proveedor de identidades SAML o WS-Fed no debe ser comprobado por DNS en Azure AD. Para obtener más información, consulte la sección [Preguntas frecuentes](#frequently-asked-questions).

#### <a name="required-saml-20-attributes-and-claims"></a>Atributos y notificaciones de SAML 2.0 necesarios
En las tablas siguientes se muestran los requisitos de los atributos y las notificaciones específicos que se deben configurar en el proveedor de identidades de terceros. Para configurar la federación, se deben recibir los siguientes atributos en la respuesta de SAML 2.0 del proveedor de identidades. Estos atributos se pueden configurar mediante la vinculación con el archivo XML del servicio de token de seguridad en línea o la introducción manual.

Atributos necesarios para la respuesta de SAML 2.0 desde el proveedor de identidades:

|Atributo  |Value  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Público     |`https://login.microsoftonline.com/<tenant ID>/` (audiencia de inquilinos recomendada). Reemplace `<tenant ID>` por el identificador de inquilino del inquilino de Azure AD en el que está configurando la federación.<br><br>`urn:federation:MicrosoftOnline` (Este valor se va a poner en desuso).          |
|Emisor     |El URI del emisor del asociado IdP, por ejemplo`http://www.example.com/exk10l6w90DHM0yi...`         |


Las notificaciones necesarias para el token de SAML 2.0 emitido por el proveedor de identidades:

|Atributo  |Value  |
|---------|---------|
|Formato de NameID     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

### <a name="ws-fed-configuration"></a>Configuración de WS-Fed

Se puede configurar Azure AD B2B para que funcione con proveedores de identidades que usan el protocolo WS-Fed con algunos requisitos específicos, que se indican a continuación. Actualmente, los dos proveedores de WS-Fed se han probado para determinar su compatibilidad con Azure AD e incluyen AD FS y Shibboleth. Para más información sobre cómo establecer la veracidad de un usuario de confianza entre un proveedor compatible con WS-Fed y Azure AD, consulte el documento "STS Integration Paper using WS Protocols" (Documento de integración con STS mediante protocolos WS) en los [documentos de compatibilidad del proveedor de identidades de Azure AD](https://www.microsoft.com/download/details.aspx?id=56843).

> [!NOTE]
> El dominio de destino para la federación no debe ser comprobado por DNS en Azure AD. Para obtener más información, consulte la sección [Preguntas frecuentes](#frequently-asked-questions).

#### <a name="required-ws-fed-attributes-and-claims"></a>Atributos y notificaciones de WS-Fed necesarios

En las tablas siguientes se muestran los requisitos de los atributos y las notificaciones específicos que se deben configurar en el proveedor de identidades de terceros que usa WS-Fed. Para configurar la federación, se deben recibir los siguientes atributos en el mensaje de WS-Fed del proveedor de identidades. Estos atributos se pueden configurar mediante la vinculación con el archivo XML del servicio de token de seguridad en línea o la introducción manual.

Atributos necesarios en el mensaje de WS-Fed desde el proveedor de identidades:
 
|Atributo  |Value  |
|---------|---------|
|PassiveRequestorEndpoint     |`https://login.microsoftonline.com/login.srf`         |
|Público     |`https://login.microsoftonline.com/<tenant ID>/` (audiencia de inquilinos recomendada). Reemplace `<tenant ID>` por el identificador del inquilino de Azure AD con el que está realizando la federación.<br><br>`urn:federation:MicrosoftOnline` (Este valor se va a poner en desuso).          |
|Emisor     |El URI del emisor del asociado IdP, por ejemplo`http://www.example.com/exk10l6w90DHM0yi...`         |

Las notificaciones necesarias para el token de WS-Fed emitido por el proveedor de identidades:

|Atributo  |Value  |
|---------|---------|
|ImmutableID     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

## <a name="step-3-configure-samlws-fed-idp-federation-in-azure-ad"></a>Paso 3: Configuración de la federación con un proveedor de identidades SAML o WS-Fed en Azure AD

A continuación, va a configurar la federación con el proveedor de identidades configurado en el paso 1 en Azure AD. Puede usar el portal de Azure AD o PowerShell. Pueden pasar de 5 a 10 minutos antes de que la directiva de la federación entre en vigor. Durante este tiempo, no intente canjear una invitación por el dominio de la federación. Los atributos siguientes son necesarios:

- URI del emisor del proveedor de identidades del asociado
- Punto de conexión de la autenticación pasiva del proveedor de identidades del asociado (solo se admite https)
- Certificado

### <a name="to-configure-federation-in-the-azure-ad-portal"></a>Configuración de la federación en el portal de Azure AD

1. Vaya a [Azure Portal](https://portal.azure.com/). En el panel izquierdo, seleccione **Azure Active Directory**. 
2. Seleccione **External Identities** > **Todos los proveedores de identidades**.
3. Seleccione **Nuevo IdP de SAML/WS-Fed**.

    ![Captura de pantalla que muestra el botón para agregar un nuevo proveedor de identidades de SAML o WS-Fed](media/direct-federation/new-saml-wsfed-idp.png)

4. En la página **New SAML/WS-Fed IdP** (Nuevo proveedor de identidades de SAMS/WS-FED), en **Protocolo de proveedor de identidades**, seleccione **SAML** o **WS-FED**.

    ![Captura de pantalla que muestra el botón de análisis en la página del proveedor de identidades de SAML o WS-Fed](media/direct-federation/new-saml-wsfed-idp-parse.png)

5. Especifique el nombre de dominio de su organización asociada, que será el nombre de dominio de destino para la federación.
6. Puede cargar un archivo de metadatos para rellenar los detalles correspondientes. Si decide escribir los metadatos manualmente, introduzca la siguiente información:
   - Nombre de dominio del proveedor de identidades del asociado
   - Identificador de entidad de proveedor de identidades del asociado
   - Punto de conexión de solicitante pasivo del proveedor de identidades del asociado
   - Certificado
   > [!NOTE]
   > La dirección URL de metadatos es opcional, pero se recomienda encarecidamente. Si proporciona la dirección URL de metadatos, Azure AD puede renovar automáticamente el certificado de firma cuando expire. Si el certificado se gira por cualquier razón antes de la hora de expiración, o si no proporciona una dirección URL de metadatos, Azure AD no podrá renovarlo. En este caso, deberá actualizar manualmente el certificado de firma.

7. Seleccione **Guardar**. 

### <a name="to-configure-samlws-fed-idp-federation-in-azure-ad-using-powershell"></a>Configuración de la federación con un proveedor de identidades SAML o WS-Fed en Azure AD con PowerShell

1. Instale la versión más reciente de Azure AD PowerShell para el módulo Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)). Si necesita pasos detallados, el Inicio rápido incluye la guía [Módulo de Powershell](b2b-quickstart-invite-powershell.md#prerequisites).
2. Ejecute el siguiente comando:

   ```powershell
   Connect-AzureAD
   ```

3. En el símbolo del sistema, inicie sesión con la cuenta de administrador global administrada.
4. Ejecute los siguientes comandos, reemplazando los valores del archivo de metadatos de la federación. Para el servidor de AD FS y Okta, el archivo de federación es federationmetadata.xml, por ejemplo: `https://sts.totheclouddemo.com/federationmetadata/2007-06/federationmetadata.xml`. 

   ```powershell
   $federationSettings = New-Object Microsoft.Open.AzureAD.Model.DomainFederationSettings
   $federationSettings.PassiveLogOnUri ="https://sts.totheclouddemo.com/adfs/ls/"
   $federationSettings.LogOffUri = $federationSettings.PassiveLogOnUri
   $federationSettings.IssuerUri = "http://sts.totheclouddemo.com/adfs/services/trust"
   $federationSettings.MetadataExchangeUri="https://sts.totheclouddemo.com/adfs/services/trust/mex"
   $federationSettings.SigningCertificate= <Replace with X509 signing cert’s public key>
   $federationSettings.PreferredAuthenticationProtocol="WsFed" OR "Samlp"
   $domainName = <Replace with domain name>
   New-AzureADExternalDomainFederation -ExternalDomainName $domainName  -FederationSettings $federationSettings
   ```

## <a name="step-4-test-samlws-fed-idp-federation-in-azure-ad"></a>Paso 4: Prueba de la federación con un proveedor de identidades SAML o WS-Fed en Azure AD
Ahora pruebe la configuración de la federación e invite a un nuevo usuario de B2B. Para más información, consulte [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](add-users-administrator.md).
 
## <a name="how-do-i-edit-a-samlws-fed-idp-federation-relationship"></a>¿Cómo se puede editar una relación de federación con un proveedor de identidades SAML o WS-Fed?

1. Vaya a [Azure Portal](https://portal.azure.com/). En el panel izquierdo, seleccione **Azure Active Directory**. 
2. Seleccione **External Identities**.
3. Seleccione **Todos los proveedores de identidades**.
4. En **SAML/WS-Fed identity providers** (Proveedores de identidades SAML/WS-Fed), seleccione el proveedor.
5. En el panel de detalles del proveedor de identidades, actualice los valores.
6. Seleccione **Guardar**.


## <a name="how-do-i-remove-federation"></a>¿Cómo se elimina la federación?

Puede eliminar la configuración de la federación. Si lo hace, los usuarios invitados de la federación que ya hayan canjeado sus invitaciones no podrán iniciar sesión. Sin embargo, puede concederles acceso de nuevo a sus recursos al [restablecer el estado de canje](reset-redemption-status.md). Para eliminar la federación con un proveedor de identidades en el portal de Azure AD:

1. Vaya a [Azure Portal](https://portal.azure.com/). En el panel izquierdo, seleccione **Azure Active Directory**.
2. Seleccione **External Identities**.
3. Seleccione **Todos los proveedores de identidades**.
4. Seleccione el proveedor de identidades y, a continuación, seleccione **Eliminar**.
5. Seleccione **Sí** para confirmar la eliminación. 

Para eliminar la federación con un proveedor de identidades mediante PowerShell:

1. Instale la versión más reciente de Azure AD PowerShell para el módulo Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Ejecute el siguiente comando:

   ```powershell
   Connect-AzureAD
   ```

3. En el símbolo del sistema, inicie sesión con la cuenta de administrador global administrada.
4. Escriba el comando siguiente:

   ```powershell
   Remove-AzureADExternalDomainFederation -ExternalDomainName  $domainName
   ```

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [experiencia de canje de invitación](redemption-experience.md) cuando los usuarios externos inician sesión con varios proveedores de identidades.
