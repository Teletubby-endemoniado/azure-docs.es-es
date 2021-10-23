---
title: 'Application Proxy de Azure Active Directory: historial de lanzamiento de versiones'
description: En este artículo se enumeran todas las versiones de Application Proxy de Azure Active Directory y se describen las nuevas características y los problemas corregidos.
services: active-directory
author: kenwith
manager: karenh444
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.date: 04/27/2021
ms.subservice: app-proxy
ms.author: kenwith
ms.reviewer: ashishj
ms.openlocfilehash: 7b00c6fb425f04d84c6f5d5c46b42865c05d5e02
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129988912"
---
# <a name="azure-ad-application-proxy-version-release-history"></a>Proxy de aplicación de Azure AD ofrece estas ventajas: Historial de lanzamiento de versiones
En este artículo se enumeran las versiones y características de Azure Active Directory (Azure AD) Application Proxy que se han publicado. El equipo de Azure AD actualiza periódicamente Application Proxy con nuevas características y funciones. Los conectores de Application Proxy se [actualizan de forma automática cuando se publica una nueva versión principal](application-proxy-faq.yml#why-is-my-connector-still-using-an-older-version-and-not-auto-upgraded-to-latest-version-). 

Se recomienda asegurarse de que las actualizaciones automáticas estén habilitadas para los conectores con el fin de asegurarse de tener las características y correcciones de errores más recientes. Soporte técnico de Microsoft podría pedirle que instale la versión más reciente del conector para resolver un problema.

Esta es una lista de recursos relacionados:

| Resource                                         | Detalles                                                      |
| ------------------------------------------------ | ------------------------------------------------------------ |
| Procedimientos para habilitar Application Proxy                  | Los requisitos previos para habilitar Application Proxy e instalar y registrar un conector se describen en este [tutorial](application-proxy-add-on-premises-application.md). |
| Descripción de los conectores del Proxy de aplicación de Azure AD | Obtenga más información sobre la [administración de conectores](application-proxy-connectors.md) y cómo los conectores [se actualizan de forma automática](application-proxy-connectors.md#automatic-updates). |
| Descarga del conector de Azure Active Directory Application Proxy    | [Descargue el conector más reciente](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download). |

## <a name="1519750"></a>1.5.1975.0

### <a name="release-status"></a>Estado de la versión

22 de julio de 2020: Publicada para descarga. Esta versión solo está disponible para su instalación a través de la página de descarga. 

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras
-   Se mejoró la compatibilidad con los entornos de nube de Azure Government. Para conocer los pasos para instalar correctamente el conector para la nube de Azure Government, consulte los [requisitos previos](../hybrid/reference-connect-government-cloud.md#allow-access-to-urls) y siga los [pasos de instalación](../hybrid/reference-connect-government-cloud.md#install-the-agent-for-the-azure-government-cloud).
- Compatibilidad con el uso del cliente web de Servicios de Escritorio remoto con Application Proxy. Para obtener más información, consulte [Publicación del Escritorio Remoto con Azure AD Application Proxy](application-proxy-integrate-with-remote-desktop-services.md).
- Se mejoraron las negociaciones de la extensión websocket. 
- Compatibilidad con el enrutamiento optimizado entre grupos de conectores y servicios en la nube de Application Proxy basados en la región. Consulte [Optimización del flujo de tráfico con Application Proxy de Azure Active Directory](application-proxy-network-topology.md) para obtener más información. 

### <a name="fixed-issues"></a>Problemas corregidos
- Se corrigió un problema de websocket que forzaba cadenas en minúsculas.
- Se corrigió un problema que hacía que los conectores no respondieran en ocasiones.

## <a name="1516260"></a>1.5.1626.0

### <a name="release-status"></a>Estado de la versión

17 de julio de 2020: publicado para descarga. Esta versión solo está disponible para su instalación a través de la página de descarga. 

### <a name="fixed-issues"></a>Problemas corregidos
- Se resolvió un problema de fuga de memoria presente en la versión anterior.
- Mejoras generales para en la compatibilidad con WebSocket.

## <a name="1515260"></a>1.5.1526.0

### <a name="release-status"></a>Estado de la versión

7 de julio de 2020: Publicada para descarga. Esta versión solo está disponible para su instalación a través de la página de descarga. 

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras
-   Los conectores solo usan TLS 1.2 para todas las conexiones. Para más información, consulte [Requisitos previos del conector](application-proxy-add-on-premises-application.md#prerequisites).
- Señalización mejorada entre el conector y los servicios de Azure. Esto incluye la compatibilidad con sesiones confiables para la comunicación de WCF entre el conector y los servicios de Azure, así como mejoras de almacenamiento en caché de DNS para las comunicaciones de WebSocket.
- Compatibilidad con la configuración de un proxy entre el conector y la aplicación de back-end. Para obtener más información, consulte [Trabajo con servidores proxy locales existentes](application-proxy-configure-connectors-with-proxy-servers.md).

### <a name="fixed-issues"></a>Problemas corregidos
- Se quitó el retroceso al puerto 8080 para las comunicaciones del conector con los servicios de Azure.
- Se han agregado seguimientos de depuración para comunicaciones de WebSocket. 
- Se resolvió conservando el atributo SameSite cuando se establece en cookies de aplicación de back-end.

## <a name="156120"></a>1.5.612.0

### <a name="release-status"></a>Estado de la versión

20 de septiembre de 2018: publicado para descarga.

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras

- Se ha agregado compatibilidad con WebSocket para la aplicación QlikSense. Para más información sobre cómo integrar QlikSense con Application Proxy, vea este [tutorial](application-proxy-qlik.md). 
- Se ha mejorado el asistente para la instalación con el fin de facilitar la configuración de un proxy de salida. 
- Se ha establecido TLS 1.2 como el protocolo predeterminado para los conectores. 
- Se ha agregado un nuevo contrato de licencia para el usuario final (CLUF).  

### <a name="fixed-issues"></a>Problemas corregidos

- Se ha corregido un error que causaba algunas fugas de memoria en el conector.
- Se ha actualizado la versión de Azure Service Bus, que incluye una corrección de errores para los problemas de tiempo de espera del conector.

## <a name="154020"></a>1.5.402.0

### <a name="release-status"></a>Estado de la versión

19 de enero de 2018: publicado para descarga.

### <a name="fixed-issues"></a>Problemas corregidos

- Se ha agregado compatibilidad con dominios personalizados que necesitan traducción de dominios en la cookie.

## <a name="151320"></a>1.5.132.0

### <a name="release-status"></a>Estado de la versión 

25 de mayo de 2017: publicado para descarga. 

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras 

Se ha mejorado el control sobre los límites de conexiones salientes de los conectores. 

## <a name="15360"></a>1.5.36.0

### <a name="release-status"></a>Estado de la versión

15 de abril de 2017: publicado para descarga.

### <a name="new-features-and-improvements"></a>Nuevas características y mejoras

- Se ha simplificado la incorporación y la administración con menos puertos necesarios. Ahora en Application Proxy solo es necesario abrir dos puertos de salida estándar: 443 y 80. En Application Proxy se siguen usando solo conexiones de salida, por lo que todavía no se necesita ningún componente en una red perimetral. Para más información, vea la  [documentación sobre la configuración](application-proxy-add-on-premises-application.md).  
- Si el proxy externo o el firewall lo admite, ahora puede abrir la red por DNS en lugar de por intervalo de direcciones IP. Los servicios de Application Proxy requieren conexiones a *.msappproxy.net *.servicebus.windows.net únicamente.


## <a name="earlier-versions"></a>Versiones anteriores

Si usa una versión del conector de Application Proxy anterior a 1.5.36.0, actualice a la versión más reciente para asegurarse de que tiene todas las características admitidas más recientes.

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Acceso remoto a aplicaciones locales mediante Azure AD Application Proxy](application-proxy.md).
- Para empezar a usar Application Proxy, consulte [Tutorial: Adición de una aplicación local para el acceso remoto mediante Application Proxy](application-proxy-add-on-premises-application.md).
