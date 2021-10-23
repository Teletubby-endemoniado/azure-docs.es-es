---
title: 'Acceso remoto a aplicaciones locales: Azure AD Application Proxy'
description: Azure Active Directory Application Proxy proporciona acceso remoto seguro a aplicaciones web locales. Después de un inicio de sesión único en Azure AD, los usuarios pueden acceder a las aplicaciones locales y en la nube mediante una dirección URL externa o un portal de aplicaciones interno. Por ejemplo, Application Proxy puede proporcionar acceso remoto e inicio de sesión único para Escritorio remoto, SharePoint, Teams, Tableau, Qlik y aplicaciones de línea de negocio (LOB).
services: active-directory
author: kenwith
manager: karenh444
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: conceptual
ms.date: 04/27/2021
ms.author: kenwith
ms.reviewer: ashishj
ms.openlocfilehash: cdffc6188256b5c69b765fe8f59efef835981468
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129988476"
---
# <a name="remote-access-to-on-premises-applications-through-azure-ad-application-proxy"></a>Acceso remoto a aplicaciones locales a través de Azure AD Application Proxy

Azure Active Directory Application Proxy proporciona acceso remoto seguro a aplicaciones web locales. Después de un inicio de sesión único en Azure AD, los usuarios pueden acceder a las aplicaciones locales y en la nube mediante una dirección URL externa o un portal de aplicaciones interno. Por ejemplo, Application Proxy puede proporcionar acceso remoto e inicio de sesión único para Escritorio remoto, SharePoint, Teams, Tableau, Qlik y aplicaciones de línea de negocio (LOB).

El proxy de aplicación de Azure AD es:

- **Fácil de usar**. Los usuarios pueden acceder a las aplicaciones locales del mismo modo en que acceden a Microsoft 365 y otras aplicaciones SaaS integradas con Azure AD. No es necesario cambiar o actualizar las aplicaciones para trabajar con el Proxy de aplicación.

- **Seguro**. Las aplicaciones locales pueden usar controles de autorización y análisis de seguridad de Azure. Por ejemplo, las aplicaciones locales pueden usar acceso condicional y verificación en dos pasos. Application Proxy no necesita que abra conexiones entrantes a través del firewall.

- **Rentable**. Las soluciones locales normalmente requieren la configuración y el mantenimiento de redes perimetrales, servidores perimetrales u otras infraestructuras complejas. Application Proxy se ejecuta en la nube, lo que hace que sea fácil de usar. Para usar Application Proxy, no es necesario cambiar la infraestructura de red ni instalar dispositivos adicionales en el entorno local.

## <a name="what-is-application-proxy"></a>¿Qué es Application Proxy?
Application Proxy es una característica de Azure AD que permite a los usuarios acceder a aplicaciones web locales desde un cliente remoto. Application Proxy incluye el servicio Application Proxy que se ejecuta en la nube y el conector Application Proxy que se ejecuta en un servidor local. Azure AD, el servicio Application Proxy y el conector Application Proxy funcionan juntos para pasar de forma segura el token de inicio de sesión del usuario de Azure AD a la aplicación web.

Application Proxy funciona con:

* Aplicaciones web que usan la [autenticación integrada de Windows](./application-proxy-configure-single-sign-on-with-kcd.md) para la autenticación.
* Aplicaciones web que usan el acceso basado en formularios o [basado en encabezados](./application-proxy-configure-single-sign-on-with-headers.md).
* API web que desea exponer a aplicaciones sofisticadas de diferentes dispositivos
* Aplicaciones que se hospedan detrás de una [puerta de enlace de escritorio remoto](./application-proxy-integrate-with-remote-desktop-services.md).
* Aplicaciones cliente enriquecidas que se integran con la biblioteca de autenticación de Microsoft (MSAL).

Application Proxy admite el inicio de sesión único. Para más información sobre los métodos admitidos, consulte [Elección de un método de inicio de sesión único](../manage-apps/sso-options.md#choosing-a-single-sign-on-method).

Application Proxy se recomienda para dar a los usuarios remotos acceso a recursos internos. Application Proxy reemplaza la necesidad de una VPN o proxy inverso. No está pensado para los usuarios internos de la red corporativa.  Estos usuarios que usan innecesariamente Application Proxy pueden presentar problemas de rendimiento inesperados y no deseados.

## <a name="how-application-proxy-works"></a>¿Cómo funciona Application Proxy?

En el siguiente diagrama se muestra cómo funcionan conjuntamente Azure AD y Application Proxy para proporcionar inicio de sesión único a las aplicaciones locales.

![Diagrama de Proxy de la aplicación de Azure AD](./media/application-proxy/azureappproxxy.png)

1. Una vez que el usuario acceda a la aplicación a través de un punto de conexión, se le dirigirá a la página de inicio de sesión de Azure AD.
2. Después de un inicio de sesión correcto, Azure AD envía un token al dispositivo cliente del usuario.
3. El cliente envía el token al servicio Application Proxy, que recupera el nombre principal de usuario (UPN) y el nombre de entidad de seguridad (SPN) del token. A continuación, Application Proxy envía la solicitud al conector Application Proxy.
4. Si se ha configurado el inicio de sesión único, el conector realiza cualquier autenticación adicional necesaria en nombre del usuario.
5. El conector envía la solicitud a la aplicación local.
6. La respuesta se envía al usuario través del conector y el servicio Application Proxy.

> [!NOTE]
> Como la mayoría de los agentes híbridos de Azure AD, el conector Application Proxy no necesita que abra conexiones entrantes a través del firewall. El tráfico de usuario en el paso 3 finaliza en el servicio Application Proxy (en Azure AD). El conector Application Proxy (local) es responsable del resto de la comunicación.
>


| Componente | Descripción |
| --------- | ----------- |
| Punto de conexión  | El punto de conexión es una dirección URL o un [portal de usuario final](../manage-apps/end-user-experiences.md). Los usuarios pueden acceder a aplicaciones fuera de la red mediante el acceso a una dirección URL externa. Los usuarios dentro de la red pueden acceder a la aplicación a través de una dirección URL o un portal del usuario final. Cuando los usuarios usan uno de estos puntos de conexión, se autentican en Azure AD y, luego, se enrutan a través del conector a la aplicación local.|
| Azure AD | Azure AD realiza la autenticación mediante el directorio de inquilino almacenado en la nube. |
| Servicio Application Proxy | Este servicio se ejecuta en la nube como parte de Azure AD. Lo que hace es pasar el token de inicio de sesión del usuario al conector Application Proxy. Application Proxy reenvía los encabezados accesibles en la solicitud a la dirección IP del cliente y los establece según su protocolo en esta dirección. Si la solicitud entrante al proxy ya tiene ese encabezado, la dirección IP del cliente se agrega al final de la lista separada por comas que es el valor del encabezado.|
| Conector Application Proxy | El conector es un agente ligero que se ejecuta en un servidor de Windows dentro de la red. El conector administra la comunicación entre el servicio Application Proxy en la nube y la aplicación local. El conector solo usa conexiones salientes, por lo que no es necesario abrir ningún puerto de entrada ni colocar nada en la red perimetral. Los conectores no tienen estado y extraen la información de la nube según sea necesario. Para más información sobre los conectores, por ejemplo cómo se equilibra la carga y cómo se autentican, vea [Descripción de los conectores del Proxy de aplicación de Azure AD](application-proxy-connectors.md).|
| Active Directory (AD) | Active Directory se ejecuta en el entorno local para realizar la autenticación de las cuentas de dominio. Cuando se configura el inicio de sesión único, el conector se comunica con AD para realizar cualquier autenticación adicional necesaria.
| Aplicación local | Por último, el usuario puede acceder a una aplicación local.

## <a name="next-steps"></a>Pasos siguientes
Para empezar a usar Application Proxy, consulte [Tutorial: Adición de una aplicación local para el acceso remoto mediante Application Proxy](application-proxy-add-on-premises-application.md).