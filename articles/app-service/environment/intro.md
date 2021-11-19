---
title: Introducción a ASEv2
description: Aprenda la forma en que las instancias de la versión 2 de Azure App Service Environment ayudan a escalar, proteger y optimizar las aplicaciones en un entorno completamente aislado y dedicado.
author: madsd
ms.topic: overview
ms.date: 11/15/2021
ms.author: madsd
ms.openlocfilehash: f704cf8bcd1efdc9a415b8c94662570869787491
ms.sourcegitcommit: 2ed2d9d6227cf5e7ba9ecf52bf518dff63457a59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132523340"
---
# <a name="introduction-to-app-service-environment-v2"></a>Introducción a la versión 2 de App Service Environment
> [!NOTE]
> En este artículo se aborda App Service Environment v2, que se usa con planes de App Service aislados. Hay una versión más reciente que resulta más fácil de usar y se ejecuta en una infraestructura más eficaz. Para aprender más sobre la nueva versión, empiece por consultar la [Introducción a App Service Environment](overview.md).
> 

## <a name="overview"></a>Información general

Azure App Service Environment, versión 2, es una característica de Azure App Service que proporciona un entorno completamente aislado y dedicado para ejecutar de forma segura las aplicaciones de App Service a gran escala. Esta funcionalidad puede hospedar sus:

* Aplicaciones web de Windows
* Aplicaciones web de Linux 
* Contenedores de Docker
* Aplicaciones móviles
* Functions

Las instancias de App Service Environment (ASE) son adecuadas para cargas de trabajo de aplicaciones que necesitan:

* Una gran escala
* Aislamiento y acceso a redes seguro
* Alta utilización de memoria

Los clientes pueden crear varias instancias de ASE en una o en varias regiones de Azure. Esta flexibilidad hace que las instancias de ASE sean perfectas para las capas de aplicación sin estado de escalado horizontal que apoyan a cargas de trabajo elevadas de solicitudes por segundo (RPS).

Los ASE hospedan aplicaciones de un solo cliente y lo hacen en una de sus redes virtuales. Los clientes tienen un mayor control sobre el tráfico de red entrante y saliente de la aplicación. Las aplicaciones pueden establecer conexiones seguras a alta velocidad por redes virtuales a los recursos corporativos locales.

* ASE viene con su propio plan de tarifa. Vea cómo la [oferta Aislado](https://channel9.msdn.com/Shows/Azure-Friday/Security-and-Horsepower-with-App-Service-The-New-Isolated-Offering?term=app%20service%20environment) ayuda a potenciar la hiperescala y la seguridad.
* [App Service Environments v2](https://channel9.msdn.com/Blogs/Azure/Azure-Application-Service-Environments-v2-Private-PaaS-Environments-in-the-Cloud?term=app%20service%20environment) proporciona un entorno que protege las aplicaciones de una subred de la red y proporciona su propia implementación privada de Azure App Service.
* Varias instancias de ASE pueden utilizarse para escalar horizontalmente. Para más información, consulte [Configuración de una huella de aplicaciones distribuidas geográficamente](app-service-app-service-environment-geo-distributed-scale.md).
* Las instancias de ASE pueden utilizarse para configurar la arquitectura de seguridad, como se muestra en Profundización de AzureCon. Para ver cómo se ha configurado la arquitectura de seguridad mostrada en la inmersión en AzureCon, consulte el artículo sobre cómo implementar una [arquitectura de seguridad en capas](app-service-app-service-environment-layered-security.md) con entornos de App Service.
* Las aplicaciones que se ejecutan en ASE pueden tener su acceso validado por dispositivos de subida como firewalls de aplicación web (WAF). Para más información, consulte [Firewall de aplicaciones web (WAF)][AppGW].
* App Service Environment se puede implementar en Availability Zones (AZ) mediante el anclaje de zonas.  Consulte [Compatibilidad de App Service Environment con Availability Zones][ASEAZ] para más información.

## <a name="dedicated-environment"></a>Entorno dedicado

Un ASE es un entorno dedicado que es exclusivo de un solo cliente y puede hospedar 200 instancias totales del plan de App Service. Un único plan de App Service de SKU aislada puede tener hasta 100 instancias en ella. Cuando se suman todas las instancias de todos los planes de App Service en ese ASE, el total debe ser menor o igual que 200.

Una instancia de ASE se compone de front-end y trabajos. Los servidores front-end son responsables de la terminación HTTP/HTTPS y del equilibrio de carga automático de la solicitudes de aplicación en una instancia de ASE. Los servidores front-end se agregan automáticamente ya que los planes de App Service en la instancia de ASE se escalan horizontalmente.

Los trabajos son roles que hospedan las aplicaciones del cliente. Los trabajos están disponibles en tres tamaños fijos:

* Una unidad vCPU/3,5 GB de RAM
* Dos unidades vCPU/7 GB de RAM
* Cuatro unidades vCPU/14 GB de RAM

Los clientes no necesitan administrar los servidores front-end ni los trabajos. Toda la infraestructura se agrega automáticamente cuando los clientes escalan horizontalmente sus planes de App Service. Cuando se crean o reducen horizontalmente planes de App Service en un ASE, la infraestructura necesaria se agrega o elimina según corresponda.

Hay una tarifa plana mensual para una instancia de ASE que paga por la infraestructura y no cambia con el tamaño de la instancia de ASE. Además, existe un costo por vCPU de plan de App Service. Todas las aplicaciones hospedadas en una instancia de ASE están en el SKU de precios Aislado. Para más información sobre los precios de una instancia de ASE, consulte la página [Precios de App Service][Pricing] y revise las opciones disponibles para los ASE.

## <a name="virtual-network-support"></a>Compatibilidad con redes virtuales

La característica de ASE es una implementación de Azure App Service directamente en la red virtual de Azure Resource Manager de un cliente. Para obtener más información acerca de redes virtuales de Azure, consulte las [Preguntas más frecuentes sobre las redes virtuales de Azure](../../virtual-network/virtual-networks-faq.md). Una instancia de ASE siempre existe en una red virtual y, más concretamente, dentro de una subred de una red virtual. Puede usar las características de seguridad de las redes virtuales para controlar las comunicaciones de red entrantes y salientes de las aplicaciones.

Una instancia de ASE puede tener conexión a Internet con una dirección IP pública o tener una conexión interna solo con la dirección de un equilibrador de carga interno (ILB) de Azure.

Los [grupos de seguridad de red][NSGs] restringen las comunicaciones de red entrantes a la subred donde reside una instancia de ASE. Puede usar grupos de seguridad de red para ejecutar aplicaciones tras dispositivos y servicios ascendentes, como WAF y proveedores de SaaS de red.

Las aplicaciones, además, suelen requerir acceso a recursos corporativos, como bases de datos internas y servicios web. Si implementa la instancia de ASE en una red virtual que tiene una conexión VPN a la red local, las aplicaciones de la instancia de ASE podrán tener acceso a los recursos locales. La capacidad es cierta independientemente de si la red privada virtual es una VPN de [sitio a sitio](../../vpn-gateway/vpn-gateway-multi-site.md) o de [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Para más información sobre cómo funciona ASE con redes virtuales y redes locales, consulte [Consideraciones de red de App Service Environment][ASENetwork].

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Application-Service-Environments-v2-Private-PaaS-Environments-in-the-Cloud/player]

## <a name="app-service-environment-v1"></a>App Service Environment v1

App Service Environment tiene tres versiones: ASEv1, ASEv2 y ASEv3. La información anterior se basa en ASEv2. En esta sección se muestran las diferencias entre ASEv1 y ASEv2. Para más información, consulte la [introducción a App Service Environment v3](./overview.md).

En ASEv1, debe administrar todos los recursos de forma manual. Esto incluye los front-end, los trabajos y las direcciones IP utilizadas para los enlaces TLS/SSL basados en IP. Antes de escalar horizontalmente el plan de App Service, primero debe escalar horizontalmente el grupo de trabajo en el que desea hospedarlo.

ASEv1 utiliza un modelo de precios diferente al de ASEv2. En ASEv1, paga por cada vCPU asignada. Esto incluye las vCPU usadas por servidores front-end y trabajos que no hospedan ninguna carga de trabajo. En ASEv1, el tamaño de escala máxima predeterminada de una instancia de ASE es de 55 hosts en total. Esto incluye servidores front-end y trabajos. Una ventaja de ASEv1 es que se puede implementar en una red virtual clásica y en una red virtual de Resource Manager. Para más información sobre ASEv1, consulte [Introducción a App Service Environment v1][ASEv1Intro].

<!--Links-->
[App Service Environments v2]: https://channel9.msdn.com/Blogs/Azure/Azure-Application-Service-Environments-v2-Private-PaaS-Environments-in-the-Cloud?term=app%20service%20environment
[Isolated offering]: https://channel9.msdn.com/Shows/Azure-Friday/Security-and-Horsepower-with-App-Service-The-New-Isolated-Offering?term=app%20service%20environment
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/network-security-groups-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../overview.md
[mobileapps]: /previous-versions/azure/app-service-mobile/app-service-mobile-value-prop
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
[ConfigureSSL]: ../configure-ssl-certificate.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: ./integrate-with-application-gateway.md
[AppGW]: ../../web-application-firewall/ag/ag-overview.md
[ASEAZ]: https://azure.github.io/AppService/2019/12/12/App-Service-Environment-Support-for-Availability-Zones.html