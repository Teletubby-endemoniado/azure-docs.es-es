---
title: 'Proveedores de conectividad y ubicaciones: Azure ExpressRoute | Microsoft Docs'
description: Este artículo ofrece una introducción detallada de ubicaciones en la que se ofrecen los servicios e información sobre cómo conectarse a regiones de Azure. Ordenado por proveedor de conectividad.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 08/26/2021
ms.author: duau
ms.openlocfilehash: 7703c1bba0faf69ee5d2640497df64cc1a418564
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128665271"
---
# <a name="expressroute-connectivity-partners-and-peering-locations"></a>Asociados de conectividad de ExpressRoute y ubicaciones de emparejamiento

> [!div class="op_single_selector"]
> * [Ubicaciones por proveedor](expressroute-locations.md)
> * [Proveedores por ubicación](expressroute-locations-providers.md)


Las tablas de este artículo proporcionan información acerca de la cobertura geográfica y las ubicaciones de ExpressRoute, los proveedores de conectividad de ExpressRoute y los integradores de sistemas (SI) de ExpressRoute.

> [!Note]
> Las regiones de Azure y las ubicaciones de ExpressRoute son dos conceptos diferentes y conocer dicha diferencia es fundamental para explorar la conectividad de red híbrida de Azure. 
>
>

## <a name="azure-regions"></a>Regiones de Azure
Las regiones de Azure son centros de datos globales en los que se encuentran los recursos de proceso, red y almacenamiento de Azure. Al crear un recurso de Azure, los clientes deben seleccionar la ubicación del recurso. Esta ubicación determina el centro de datos (o la zona de disponibilidad) de Azure en que se crea el recurso.

## <a name="expressroute-locations"></a>Ubicaciones de ExpressRoute
Las ubicaciones de ExpressRoute (a las que a veces se denomina ubicaciones de emparejamiento o ubicaciones de punto de encuentro) son instalaciones de ubicación compartida en las que se encuentran los dispositivos de Microsoft Enterprise Edge (MSEE). Las ubicaciones de ExpressRoute son el punto de entrada a la red de Microsoft (y se distribuyen globalmente, lo que proporciona a los clientes la oportunidad de conectarse a la red de Microsoft en todo el mundo). Estas ubicaciones son donde los asociados de ExpressRoute y los clientes ExpressRoute Direct emiten conexiones cruzadas a la red de Microsoft. En general, no es preciso que la ubicación de ExpressRoute coincida con la región de Azure. Por ejemplo, un cliente puede crear un circuito ExpressRoute con la ubicación de recursos *Este de EE. UU.* , en la ubicación de emparejamiento *Seattle*.

Tendrá acceso a los servicios de Azure en todas las regiones dentro de una región geopolítica si se conectó al menos a una ubicación de ExpressRoute dentro de la región geopolítica.

[!INCLUDE [expressroute-azure-regions-geopolitical-region](../../includes/expressroute-azure-regions-geopolitical-region.md)]

## <a name="expressroute-connectivity-providers"></a><a name="partners"></a>Proveedores de conectividad ExpressRoute

La siguiente tabla muestra las ubicaciones por proveedor de servicios. Si desea ver los proveedores disponibles según la ubicación, consulte [Proveedores de servicios por ubicación](expressroute-locations-providers.md).


### <a name="global-commercial-azure"></a>Servicios comerciales globales de Azure

| **Proveedor de servicios** | **Microsoft Azure** | **Microsoft 365**  | **Ubicaciones** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/connectivity-services/azure-expressroute)** |Compatible |Compatible |Melbourne, Sidney |
| **[Airtel](https://www.airtel.in/business/#/)** | Compatible | Compatible | Chennai (Madrás)2, Mumbai (Bombay)2 |
| **[AIS](https://business.ais.co.th/solution/en/azure-expressroute.html)** | Compatible | Compatible | Bangkok |
| **[Aryaka Networks](https://www.aryaka.com/)** |Compatible |Compatible |Ámsterdam, Chicago, Dallas, RAE de Hong Kong, Sao Paulo, Seattle, Silicon Valley, Singapur, Tokio, Washington DC |
| **[Centros de datos de Ascenty](https://www.ascenty.com/en/cloud/microsoft-express-route)** |Compatible |Compatible | Campinas, Sao Paulo |
| **[AT&amp;T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Compatible |Compatible |Ámsterdam, Chicago, Dallas, Frankfurt, Hong Kong, Londres, Silicon Valley, Singapur, Sidney, Tokio, Toronto, Washington DC |
| **[AT TOKYO](https://www.attokyo.com/connectivity/azure.html)** | Compatible | Compatible | Osaka, Tokyo2 |
| **[BICS](https://bics.com/bics-solutions-suite/cloud-connect/bics-cloud-connect-an-official-microsoft-azure-technology-partner/)** | Compatible | Compatible | Ámsterdam2, Londres2 |
| **[BBIX](https://www.bbix.net/en/service/ix/)** | Compatible | Compatible | Osaka, Tokyo |
| **[BCX](https://www.bcx.co.za/solutions/connectivity/data-networks)** |Compatible |Compatible |Ciudad del cabo, Johannesburgo|
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Compatible |Compatible |Montreal, Toronto, Ciudad de Quebec, Vancouver |
| **[British Telecom](https://www.globalservices.bt.com/en/solutions/products/cloud-connect-azure)** |Compatible |Compatible |Ámsterdam, Ámsterdam2, Chicago, Frankfurt, RAE de Hong Kong, Johannesburgo, Londres, Londres2, Newport (Gales), París, Sao Paulo, Silicon Valley, Singapur, Sídney, Tokio, Washington DC. |
| **[BSNL](https://www.bsnl.co.in/opencms/bsnl/BSNL/services/enterprises/cloudway.html)** |Compatible |Compatible |Chennai, Mumbai |
| **[C3ntro](https://www.c3ntro.com/)** |Compatible |Compatible |Miami |
| **CDC** | Compatible | Compatible | Canberra, Canberra 2 |
| **[CenturyLink Cloud Connect](https://www.centurylink.com/cloudconnect)** |Compatible |Compatible |Ámsterdam2, Chicago, Dublín, Frankfurt, Hong Kong, Las Vegas, Londres, Londres2, Nueva York, París, San Antonio, Silicon Valley, Singapur2, Tokio, Toronto, Washington DC, Washington DC2 |
| **[Chief Telecom](https://www.chief.com.tw/)** |Compatible |Compatible |Hong Kong, Taipéi |
| **China Mobile International** |Compatible |Compatible | Hong Kong, Hong Kong2, Singapur |
| **China Telecom Global** |Compatible |Compatible |Hong Kong, Hong Kong2 |
| **[China Unicom Global](https://cloudbond.chinaunicom.cn/home-en)** |Compatible |Compatible | Hong Kong, Singapur2, Tokio2 |
| **[Chunghwa Telecom](https://www.cht.com.tw/en/home/cht/about-cht/products-and-services/International/Cloud-Service)** |Compatible |Compatible |Taipéi |
| **[Claro](https://www.usclaro.com/enterprise-mnc/connectivity/mpls/)** |Compatible |Compatible |Miami |
| **[Cologix](https://www.cologix.com/hyperscale/microsoft-azure/)** |Compatible |Compatible |Chicago, Dallas, Mineápolis, Montreal, Toronto, Vancouver, Washington DC |
| **[Colt](https://www.colt.net/direct-connect/azure/)** |Compatible |Compatible |Ámsterdam, Ámsterdam2, Berlín, Chicago, Dublín, Fráncfort, Ginebra, Hong Kong, Londres, Londres2, Marsella, Milán, Munich, Newport, Nueva York, Osaka, París, Silicon Valley, Silicon Valley2, Singapur2, Tokio, Washington DC, Zúrich. |
| **[Comcast](https://business.comcast.com/landingpage/microsoft-azure)** |Compatible |Compatible |Chicago, Silicon Valley, Washington DC |
| **[CoreSite](https://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Compatible |Compatible |Chicago, Chicago2, Denver, Los Ángeles, Nueva York, Silicon Valley, Silicon Chicago2, Washington DC, Washington DC2 |
| **[DE-CIX](https://www.de-cix.net/en/de-cix-service-world/cloud-exchange/find-a-cloud-service/detail/microsoft-azure)** | Compatible |Compatible |Ámsterdam2, Dubái2, Fráncfort, Marsella, Bombay, Múnich, Nueva York |
| **[Devoli](https://devoli.com/expressroute)** | Compatible |Compatible | Auckland, Melbourne, Sidney |
| **[Deutsche Telekom AG IntraSelect](https://geschaeftskunden.telekom.de/vernetzung-digitalisierung/produkt/intraselect)** | Compatible |Compatible |Fráncfort |
| **[Deutsche Telekom AG](https://geschaeftskunden.telekom.de/vernetzung-digitalisierung/produkt/intraselect)** | Compatible |Compatible |Frankfurt2 |
| **du datamena** |Compatible |Compatible | Dubai2 |
| **eir** |Compatible |Compatible |Dublín|
| **[Epsilon Global Communications](https://www.epsilontel.com/solutions/direct-cloud-connect)** |Compatible |Compatible |Singapur, Singapur2 |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Compatible |Compatible |Ámsterdam, Ámsterdam2, Atlanta, Berlín, Bogotá, Canberra2, Chicago, Dallas, Dubái2, Dublín, Fráncfort, Fráncfort2, Ginebra, RAE de Hong Kong, Londres, Londres2, Los Ángeles*, Los Ángeles2, Melbourne, Miami, Milán, Nueva York, Osaka, París, Ciudad de Quebec, Río de Janeiro, São Paulo, Seattle, Seúl, Silicon Valley, Singapur, Singapur2, Estocolmo, Sídney, Tokio, Toronto, Washington DC, Zúrich</br></br> **Los nuevos circuitos ExpressRoute ya no se admiten con Equinix en Los Ángeles. Cree nuevos circuitos en Los Ángeles2.* |
| **Etisalat UAE** |Compatible |Compatible |Dubai|
| **[euNetworks](https://eunetworks.com/services/solutions/cloud-connect/microsoft-azure-expressroute/)** |Compatible |Compatible |Ámsterdam, Ámsterdam2, Dublín, Fráncfort, Londres |
| **[FarEasTone](https://www.fetnet.net/corporate/en/Enterprise.html)** |Compatible |Compatible |Taipéi|
| **[Fastweb](https://www.fastweb.it/grandi-aziende/cloud/scheda-prodotto/fastcloud-interconnect/)** | Compatible |Compatible |Milán|
| **[Fibrenoire](https://fibrenoire.ca/en/services/cloudextn-2/)** |Compatible |Compatible |Montreal|
| **[GBI](https://www.gbiinc.com/microsoft-azure/)** |Compatible |Compatible |Dubai2, Frankfurt|
| **[GÉANT](https://www.geant.org/Networks)** |Compatible |Compatible |Ámsterdam, Ámsterdam2, Dublín, Fráncfort, Marsella |
| **[GlobalConnect](https://www.globalconnect.no/tjenester/nettverk/cloud-access)** | Compatible |Compatible | Oslo, Stavanger | 
| **GTT** |Compatible |Compatible |Londres2 |
| **[Global Cloud Xchange (GCX)](https://globalcloudxchange.com/cloud-platform/cloud-x-fusion/)** | Compatible| Compatible | Chennai, Mumbai |
| **[iAdvantage](https://www.scx.sunevision.com/)** | Compatible | Compatible | Hong Kong2 |
| **Intelsat** | Compatible | Compatible | Washington DC2 |
| **[InterCloud](https://www.intercloud.com/)** |Compatible |Compatible |Ámsterdam, Chicago, Fráncfort, Hong Kong, Londres, Nueva York, París, Silicon Valley, Singapur, Tokyo, Washington DC, Zúrich |
| **[Internet2](https://internet2.edu/services/cloud-connect/#service-cloud-connect)** |Compatible |Compatible |Chicago, Dallas, Silicon Valley, Washington DC |
| **[Internet Initiative Japan Inc. - IIJ](https://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Compatible |Compatible |Osaka, Tokyo |
| **[Internet Solutions - Cloud Connect](https://www.is.co.za/solution/cloud-connect/)** |Compatible |Compatible |Ciudad del Cabo, Johannesburgo, Londres |
| **[Interxion](https://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Compatible |Compatible |Ámsterdam, Ámsterdam2, Copenhague, Dublín, Dublín2, Frankfurt, Londres, Madrid, Marsella, París, Zúrich |
| **[IRIDEOS](https://irideos.it/)** |Compatible |Compatible |Milán |
| **Iron Mountain** | Compatible |Compatible |Washington DC |
| **[IX Reach](https://www.ixreach.com/partners/cloud-partners/microsoft-azure/)**|Compatible |Compatible | Ámsterdam, Londres2, Silicon Valley, Toronto, Washington |
| **Jaguar Network** |Compatible |Compatible |Marsella, París |
| **[Jisc](https://www.jisc.ac.uk/microsoft-azure-expressroute)** |Compatible |Compatible |Londres, Newport (Gales) |
| **[KINX](https://www.kinx.net/service/cloudhub/ms-expressroute/?lang=en)** |Compatible |Compatible |Seúl |
| **[Kordia](https://www.kordia.co.nz/cloudconnect)** | Compatible |Compatible |Auckland, Sídney |
| **[KPN](https://www.kpn.com/zakelijk/cloud/connect.htm)** | Compatible | Compatible | Ámsterdam |
| **[KT](https://cloud.kt.com/)** | Compatible | Compatible | Seúl |
| **[Level 3 Communications](https://www.lumen.com/en-us/hybrid-it-cloud/cloud-connect.html)** |Compatible |Compatible |Ámsterdam, Chicago, Dallas, Londres, Newport (Gales), Sao Paulo, Seattle, Silicon Valley, Singapur y Washington DC |
| **LG CNS** |Compatible |Compatible |Busan, Seúl |
| **[Liquid Telecom](https://www.liquidtelecom.com/products-and-services/cloud.html)** |Compatible |Compatible |Ciudad del cabo, Johannesburgo |
| **[LGUplus](http://www.uplus.co.kr/)** |Compatible |Compatible |Seúl |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Compatible |Compatible |Ámsterdam, Atlanta, Auckland, Chennai, Chicago, Ciudad de Quebec, Dallas, Denver, Dubai2, Dublín, Estocolmo, Fráncfort, Ginebra, Hong Kong, Hong Kong2, Las Vegas, Londres, Londres2, Los Ángeles, Madrid, Melbourne, Miami, Minneapolis, Montreal, Múnich, Nueva York, Osaka, Oslo, París, Perth, Phoenix, San Antonio, Seattle, Silicon Valley, Singapur, Singapur2, Stavanger, Sídney, Sídney2, Tokio, Tokio2, Toronto, Vancouver, Washington D. C., Washington D. C. 2, Zúrich |
| **[MTN](https://www.mtnbusiness.co.za/en/Cloud-Solutions/Pages/microsoft-express-route.aspx)** |Compatible |Compatible |London |
| **MTN Global Connect** |Compatible |Compatible |Ciudad del cabo, Johannesburgo|
| **[National Telecom](https://www.nc.ntplc.co.th/cat/category/264/855/CAT+Direct+Cloud+Connect+for+Microsoft+ExpressRoute?lang=en_EN)** |Compatible |Compatible |Bangkok |
| **[Neutrona Networks](https://www.neutrona.com/index.php/azure-expressroute/)** |Compatible |Compatible |Dallas, Los Ángeles, Miami, Sao Paulo, Washington DC |
| **[Datos de última generación](https://vantage-dc-cardiff.co.uk/)** |Compatible |Compatible |Newport (Gales) |
| **[NEXTDC](https://www.nextdc.com/services/axon-ethernet/microsoft-expressroute)** |Compatible |Compatible |Melbourne, Perth, Sídney, Sídney2 |
| **[NOS](https://www.nos.pt/empresas/corporate/cloud/cloud/Pages/nos-cloud-connect.aspx)** |Compatible |Compatible |Ámsterdam2 |
| **[NTT Communications](https://www.ntt.com/en/services/network/virtual-private-network.html)** |Compatible |Compatible |Ámsterdam, RAE de Hong Kong, Yakarta, Londres, Los Ángeles, Osaka, Singapur, Sídney, Tokio, Washington DC |
| **[NTT EAST](https://business.ntt-east.co.jp/service/crossconnect/)** |Compatible |Compatible |Tokio |
| **[NTT Global DataCenters EMEA](https://hello.global.ntt/)** |Compatible |Compatible |Ámsterdam2, Berlín, Fráncfort, Londres2 |
| **[NTT SmartConnect](https://cloud.nttsmc.com/cxc/azure.html)** |Compatible |Compatible |Osaka |
| **[Ooredoo Cloud Connect](https://www.ooredoo.qa/portal/OoredooQatar/cloud-connect-expressroute)** |Compatible |Compatible |Marsella |
| **[Optus](https://www.optus.com.au/enterprise/)** |Compatible |Compatible |Melbourne, Sidney |
| **[Orange](https://www.orange-business.com/en/products/business-vpn-galerie)** |Compatible |Compatible |Ámsterdam, Ámsterdam2, Dubái2, Fráncfort, Johannesburgo, Londres, París, RAE de Hong Kong, Sao Paulo, Silicon Valley, Singapur, Sídney, Tokio, Washington DC |
| **[Orixcom](https://www.orixcom.com/cloud-solutions/)** | Compatible | Compatible | Dubai2 |
| **[PacketFabric](https://www.packetfabric.com/cloud-connectivity/microsoft-azure)** |Compatible |Compatible |Chicago, Dallas, Denver, Las Vegas, Silicon Valley, Washington DC. |
| **[PCCW Global Limited](https://consoleconnect.com/clouds/#azureRegions)** |Compatible |Compatible |Chicago, Hong Kong, Hong Kong2, Londres, Singapur2 |
| **[REANNZ](https://www.reannz.co.nz/products-and-services/cloud-connect/)** | Compatible | Compatible | Auckland |
| **[Retelit](https://www.retelit.it/EN/Home.aspx)** | Compatible | Compatible | Milán | 
| **[Sejong Telecom](https://www.sejongtelecom.net/en/pages/service/cloud_ms)** |Compatible |Compatible |Seúl |
| **[SES](https://www.ses.com/networks/signature-solutions/signature-cloud/ses-and-azure-expressroute)** | Compatible |Compatible | Londres2, Washington DC |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Compatible |Compatible |Chennai (Madrás), Mumbai (Bombay)2 |
| **[SingTel](https://www.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Compatible |Compatible |Hong Kong2, Singapore, Singapore2 |
| **[SK Telecom](http://b2b.tworld.co.kr/bizts/solution/solutionTemplate.bs?solutionId=0085)** |Compatible |Compatible |Seúl |
| **[Softbank](https://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Compatible |Compatible |Osaka, Tokyo |
| **[Sohonet](https://www.sohonet.com/fastlane/)** |Compatible |Compatible |Londres2 |
| **[Spark NZ](https://www.sparkdigital.co.nz/solutions/connectivity/cloud-connect/)** |Compatible |Compatible |Auckland, Sídney |
| **[Sprint](https://business.sprint.com/solutions/cloud-networking/)** |Compatible |Compatible |Chicago, Silicon Valley, Washington DC |
| **[Swisscom](https://www.swisscom.ch/en/business/enterprise/offer/cloud-data-center/microsoft-cloud-services/microsoft-azure-von-swisscom.html)** | Compatible | Compatible | Ginebra, Zurich |
| **[Tata Communications](https://www.tatacommunications.com/solutions/network/cloud-ready-networks/)** |Compatible |Compatible |Ámsterdam, Chennai, RAE de Hong Kong, Londres, Mumbai, Sao Paulo, Silicon Valley, Singapur, Washington DC |
| **[Telefónica](https://www.telefonica.com/es/home)** |Compatible |Compatible |Ámsterdam, Sao Paulo |
| **[Telehouse - KDDI](https://www.telehouse.net/solutions/cloud-services/cloud-link)** |Compatible |Compatible |Londres, Londres2, Singapur2, Tokio |
| **Telenor** |Compatible |Compatible |Ámsterdam, Londres, Oslo |
| **[Telia Carrier](https://www.teliacarrier.com/)** | Compatible | Compatible |Ámsterdam, Chicago, Dallas, Estocolmo, Fráncfort, Hong Kong, Londres, Oslo, París, Silicon Valley, Washington |
| **[Telin](https://www.telin.net/product/data-connectivity/telin-cloud-exchange)** | Compatible | Compatible |Yakarta |
| **Telmex Uninet**| Compatible | Compatible | Dallas |
| **[Telstra Corporation](https://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Compatible |Compatible |Melbourne, Singapur, Sidney |
| **[Telus](https://www.telus.com)** |Compatible |Compatible |Montreal, Seattle, Ciudad de Quebec, Toronto, Vancouver |
| **[Teraco](https://www.teraco.co.za/services/africa-cloud-exchange/)** |Compatible |Compatible |Ciudad del cabo, Johannesburgo |
| **[TIME dotCom](https://www.time.com.my/enterprise/connectivity/direct-cloud)** | Compatible | Compatible | Kuala Lumpur |
| **[Tokai Communications](https://www.tokai-com.co.jp/en/)** | Compatible | Compatible | Osaka, Tokyo2 |
| **[Transtelco](https://transtelco.net/enterprise-services/)** |Compatible |Compatible |Dallas, Queretaro (México)|
| **[T-Systems](https://geschaeftskunden.telekom.de/vernetzung-digitalisierung/produkt/intraselect)** |Compatible |Compatible |Fráncfort|
| **[UOLDIVEO](https://www.uoldiveo.com.br/)** |Compatible |Compatible |Sao Paulo |
| **[UIH](https://www.uih.co.th/en/network-solutions/global-network/cloud-direct-for-microsoft-azure-expressroute)** | Compatible | Compatible | Bangkok |
| **[Verizon](https://enterprise.verizon.com/products/network/application-enablement/secure-cloud-interconnect/)** |Compatible |Compatible |Ámsterdam, Chicago, Dallas, RAE de Hong Kong, Londres, Mumbai, Silicon Valley, Singapur, Sídney, Tokio, Toronto, Washington DC |
| **[Viasat](http://www.directcloud.viasatbusiness.com/)** | Compatible | Compatible | Washington DC2 |
| **[Vocus Group NZ](https://www.vocus.co.nz/business/cloud-data-centres)** | Compatible | Compatible | Auckland, Sídney |
| **Vodacom** |Compatible |Compatible |Ciudad del cabo, Johannesburgo|
| **[Vodafone](https://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Compatible |Compatible |Ámsterdam2, Londres, Singapur |
| **[Vodafone Idea](https://www.vodafone.in/business/enterprise-solutions/connectivity/vpn-extended-connect)** | Compatible | Compatible | Bombay2 |
| **[Zayo](https://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Compatible |Compatible |Ámsterdam, Chicago, Dallas, Denver, Londres, Los Ángeles, Montreal, Nueva York, París, Phoenix, Seattle, Silicon Valley, Toronto, Washington DC, Washington DC2 |

 **+** indica próximamente

### <a name="national-cloud-environment"></a>Entorno de nube nacional

Las nubes nacionales de Azure están aisladas entre sí y de los servicios comerciales globales de Azure. ExpressRoute para una nube de Azure no se puede conectar a las regiones de Azure de otras nubes. 

### <a name="us-government-cloud"></a>Nube del US Gov

| **Proveedor de servicios** | **Microsoft Azure** | **Office 365** | **Ubicaciones** |
| --- | --- | --- | --- |
| **[AT&amp;T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Compatible |Compatible |Chicago, Phoenix, Silicon Valley, Washington D. C. |
| **[CenturyLink Cloud Connect](https://www.centurylink.com/cloudconnect)** |Compatible |Compatible |Nueva York, Phoenix, San Antonio, Washington DC |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Compatible |Compatible |Atlanta, Chicago, Dallas, Nueva York, Seattle, Silicon Valley, Washington D. C. |
| **[Internet2]()** |Compatible |Compatible |Dallas |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Compatible |Compatible |Chicago, Silicon Valley, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Compatible | Compatible | Chicago, Dallas, San Antonio, Seattle, Washington DC |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Compatible |Compatible |Chicago, Dallas, Nueva York, Silicon Valley, Washington DC |

### <a name="china"></a>China

| **Proveedor de servicios** | **Microsoft Azure** | **Office 365** | **Ubicaciones** |
| --- | --- | --- | --- |
| **China Telecom** |Compatible |No compatible |Beijing, Beijing2, Shanghái, Shanghái2 |
| **China Unicom** | Compatible | No compatible | Beijing2, Shanghai2 |
| **[GDS](http://www.gds-services.com/en/about_2.html)** |Compatible |No compatible |Beijing2, Shanghai2 |

Para obtener más información, consulte [ExpressRoute en China](http://www.windowsazure.cn/home/features/expressroute/).

### <a name="germany"></a>Alemania

| **Proveedor de servicios** | **Microsoft Azure** | **Office 365** | **Ubicaciones** |
| --- | --- | --- | --- |
| **[Colt](https://www.colt.net/direct-connect/azure/)** |Compatible |No compatible |Fráncfort |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Compatible |No compatible |Fráncfort |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroute)** |Compatible |No compatible |Berlín |
| **Interxion** |Compatible |No compatible |Fráncfort |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Compatible  | No compatible | Berlín |
| **[T-Systems](https://geschaeftskunden.telekom.de/vernetzung-digitalisierung/produkt/intraselect)** |Compatible |No compatible |Berlín |

## <a name="connectivity-through-exchange-providers"></a>Conectividad a través de proveedores de intercambio

Si su proveedor de conectividad no aparece en la lista de las secciones anteriores, puede crear una conexión.

* Consulte con el proveedor de conectividad para ver si existe una conexión con cualquiera de los intercambios de la tabla anterior. Puede comprobar los vínculos siguientes para recopilar más información sobre los servicios ofrecidos por proveedores de Exchange. Varios proveedores de conectividad ya están conectados a los intercambios de Ethernet.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [DE-CIX](https://www.de-cix.net/en/de-cix-service-world/cloud-exchange)
  * [Equinix Cloud Exchange](https://www.equinix.com/interconnection-services/equinix-fabric)
  * [Interxion](https://www.interxion.com/products/interconnection/cloud-connect/)
  * [IX Reach](https://www.ixreach.com/partners/cloud-partners/microsoft-azure/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](https://www.nextdc.com/)
  * [PacketFabric](https://www.packetfabric.com/cloud-connectivity/microsoft-azure) 
  * [Teraco](https://www.teraco.co.za/platform-teraco/africa-cloud-exchange/)

* Haga que el proveedor de conectividad amplíe su red a la ubicación de emparejamiento que elija.
  * Asegúrese de que su proveedor de conectividad amplíe la conectividad con una alta disponibilidad para que no haya ningún punto único de error.
* Solicitar un circuito ExpressRoute con el intercambio como proveedor de conectividad para conectarse a Microsoft.
  * Siga los pasos de la sección [Creación de un circuito ExpressRoute](expressroute-howto-circuit-classic.md) para configurar la conectividad.

## <a name="connectivity-through-satellite-operators"></a>Conectividad a través de operadores satelitales
Si es un empleado remoto y no tiene conectividad de fibra o quiere explorar otras opciones de conectividad, puede comprobar los siguientes operadores satelitales. 

* Intelsat
* [SES](https://www.ses.com/networks/signature-solutions/signature-cloud/ses-and-azure-expressroute)
* [Viasat](http://www.directcloud.viasatbusiness.com/)

## <a name="connectivity-through-additional-service-providers"></a>Conectividad mediante proveedores de servicios adicionales

| **Proveedor de conectividad** | **Exchange** | **Ubicaciones** |
| --- | --- | --- |
| **[1CLOUDSTAR](https://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** | Equinix |Singapur |
| **[Airgate Technologies, Inc.](https://www.airgate.ca/)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](https://www.alaskacommunications.com/Business)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |Nueva York, Washington DC |
| **[Aptum Technologies](https://aptum.com/services/cloud/managed-azure/)**| Equinix | Montreal, Toronto |
| **[Arteria Networks Corporation](https://www.arteria-net.com/business/service/cloud/sca/)** |Equinix |Tokio |
| **[Axtel](https://alestra.mx/landing/expressrouteazure/)** |Equinix |Dallas|
| **[Beanfield Metroconnect](https://www.beanfield.com/business/cloud-exchange)** |Megaport |Toronto|
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | London |
| **[BICS](https://www.bics.com/services/capacity-solutions/cloud-connect/)** | Equinix | Ámsterdam, Fráncfort, Londres, Singapur y Washington DC |
| **[BroadBand Tower, Inc.](https://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Tokio |
| **[C3ntro Telecom](https://www.c3ntro.com/)** | Equinix, Megaport | Dallas |
| **[Chief](https://www.chief.com.tw/)** | Equinix | RAE de Hong Kong |
| **[Cinia](https://www.cinia.fi/palvelutiedotteet)** | Equinix, Megaport | Fráncfort, Hamburgo |
| **[CloudXpress](https://www2.telenet.be/content/www-telenet-be/fr/business/sme-le/aanbod/internet/cloudxpress)** | Equinix | Ámsterdam | 
| **[CMC Telecom](https://cmctelecom.vn/san-pham/value-added-service-and-it/cmc-telecom-cloud-express-en/)** | Equinix | Singapur | 
| **[CoreAzure](https://www.coreazure.com/)**| Equinix | London |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)**| Equinix | Dallas, Silicon Valley, Washington DC |
| **[Crown Castle](https://fiber.crowncastle.com/solutions/added/cloud-connect)**| Equinix | Atlanta, Chicago, Dallas, Los Ángeles, Nueva York, Washington DC |
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](https://www.epsilontel.com/solutions/cloud-connect/)** | Equinix | London, Singapore, Washington DC |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Ámsterdam |
| **[Exponencial E](https://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | London |
| **[Fastweb S.p.A](https://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Ámsterdam |
| **[Fibrenoire](https://www.fibrenoire.ca/en/cloudextn)** | Megaport | Ciudad de Quebec |
| **[FPT Telecom International](https://cloudconnect.vn/en)** |Equinix |Singapur|
| **[Gtt Communications Inc](https://www.gtt.net)** |Equinix | Washington DC |
| **[Gulf Bridge International](https://gbiinc.com/)** | Equinix | Ámsterdam |
| **[HSO](https://www.hso.co.uk/products/cloud-direct)** |Equinix | Londres, Slough |
| **[IVedha Inc](http://www.ivedha.com/cloud/manage-azure-cloud/express-route-4/)**| Equinix | Toronto |
| **[Kaalam Telecom Bahrain B.S.C](http://www.kalaam-telecom.com/azure/)**| Level 3 Communications |Ámsterdam |
| **LGA Telecom** |Equinix |Singapur|
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |RAE de Hong Kong 
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sidney |
| **[MainOne](https://www.mainone.net/services/connectivity/cloud-connect/)** |Equinix | Ámsterdam |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[MTN](https://www.mtnbusiness.co.za/en/Cloud-Solutions/Pages/microsoft-express-route.aspx)** | Teraco | Ciudad del cabo, Johannesburgo |
| **[NexGen Networks](https://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | London |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Equinix | Ámsterdam, Fráncfort |
| **[Oncore Cloud Service Inc](https://www.oncore.cloud/services/ue-for-expressroute)**| Equinix | Toronto |
| **[POST Telecom Luxembourg](https://www.teralinksolutions.com/cloud-connectivity/cloudbridge-to-azure-expressroute/)**|Equinix | Ámsterdam |
| **[Proximus](https://www.proximus.be/en/id_b_cl_proximus_external_cloud_connect/companies-and-public-sector/discover/magazines/expert-blog/proximus-external-cloud-connect.html)**|Equinix | Ámsterdam, Dublín, Londres, París |
| **[QSC AG](https://www2.qbeyond.de/en/)** |Interxion | Fráncfort |  
| **[RETN](https://retn.net/services/cloud-connect/)** | Equinix | Ámsterdam |
| **Rogers** | Cologix y Equinix | Montreal, Toronto |
| **[Spectrum Enterprise](https://enterprise.spectrum.com/services/cloud/cloud-connect.html)** | Equinix | Chicago, Dallas, Los Ángeles, Nueva York, Silicon Valley | 
| **[Tamares Telecom](http://www.tamarestelecom.com/our-services/#Connectivity)** | Equinix | London | 
| **[Tata Teleservices](https://www.tatateleservices.com/business-services/data-services/secure-cloud-connect)** | Tata Communications | Chennai, Mumbai |
| **[TDC Erhverv](https://tdc.dk/Produkter/cloudaccessplus)** | Equinix | Ámsterdam | 
| **[Telecom Italia Sparkle](https://www.tisparkle.com/our-platform/corporate-platform/sparkle-cloud-connect#catalogue)**| Equinix | Ámsterdam |
| **[Telekom Deutschland GmbH](https://cloud.telekom.de/de/infrastruktur/managed-it-services/managed-hybrid-infrastructure-mit-microsoft-azure)** | Interxion | Ámsterdam, Fráncfort |
| **[Telia](https://www.telia.se/foretag/losningar/produkter-tjanster/datanet)** | Equinix | Ámsterdam |
| **[ThinkTel](https://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Singapur |
| **[Venha Pra Nuvem](https://venhapranuvem.com.br/)** | Equinix | Sao Paulo |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | Nueva York |
| **[Windstream](https://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Silicon Valley, Washington DC |
| **[X2nsat Inc.](https://www.x2nsat.com/expressroute/)** |Coresite |Silicon Valley, Silicon Valley 2|
| **Zain** |Equinix |London|
| **[Zertia](https://www.zertia.es)**| Nivel 3 | Madrid |
| **[Zirro](https://zirro.com/services/)**| Cologix y Equinix | Montreal, Toronto |

## <a name="connectivity-through-datacenter-providers"></a>Conectividad mediante proveedores de centro de datos

| **Proveedor** | **Exchange** |
| --- | --- |
| **[CyrusOne](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport, PacketFabric |
| **[Cyxtera](https://www.cyxtera.com/data-center-services/interconnection)** | Megaport, PacketFabric |
| **[Databank](https://www.databank.com/platforms/connectivity/cloud-direct-connect/)** | Megaport |
| **[DataFoundry](https://www.datafoundry.com/services/cloud-connect/)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | IX Reach, Megaport PacketFabric |
| **[EdgeConnex](https://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport, PacketFabric |
| **[Flexential](https://www.flexential.com/connectivity/cloud-connect-microsoft-azure-expressroute)** | IX Reach, Megaport, PacketFabric |
| **[QTS Data Centers](https://www.qtsdatacenters.com/hybrid-solutions/connectivity/azure-cloud )** | Megaport, PacketFabric |
| **[Stream Data Centers]( https://www.streamdatacenters.com/products-services/network-cloud/ )** | Megaport |
| **[Centros de datos de RagingWire](https://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | IX Reach, Megaport, PacketFabric |
| **[Centros de datos de T5](https://t5datacenters.com/)** | IX Reach |
| **[vXchnge](https://www.vxchnge.com/colocation-services/interconnection)** | IX Reach, Megaport |

## <a name="connectivity-through-national-research-and-education-networks-nren"></a>Conectividad mediante redes nacionales de investigación y educación (NREN)

| **Proveedor**|
| --- |
| **AARNET**| 
| **DeIC, a través de GÉANT**|
| **GARR, a través de GÉANT**|
| **GÉANT**|
| **HEAnet a través de GÉANT**|
| **Internet2**|
| **JISC**|
| **RedIRIS a través de GÉANT**|
| **SINET**|
| **Surfnet a través de GÉANT**|

* Si su proveedor de conectividad no aparece aquí, compruebe si están conectados a alguno de los proveedores de intercambio de ExpressRoute enumerados anteriormente.

## <a name="expressroute-system-integrators"></a>Integradores de sistemas de ExpressRoute
Habilitar la conectividad privada para la adaptación a sus necesidades puede ser complicado según la escala de la red. Puede trabajar con cualquiera de los integradores de sistemas que aparecen en la tabla siguiente para ayudarle con la incorporación a ExpressRoute.

| **Integrador de sistemas** | **Continente** |
| --- | --- |
| **[Altogee](https://altogee.be/diensten/express-route/)** | Europa |
| **[Avanade Inc.](https://www.avanade.com/)** | Asia, Europa, Norteamérica y Sudamérica |
| **[Bright Skies GmbH](https://www.rackspace.com/bright-skies)** | Europa
| **[Ensyst](https://www.ensyst.com.au)** | Asia
| **[Equinix Professional Services](https://www.equinix.com/services/consulting/)** | Norteamérica |
| **[FlexManage](https://www.flexmanage.com/cloud)** | Norteamérica |
| **[Lightstream](https://www.lightstream.tech/partners/microsoft-azure/)** | Norteamérica |
| **[The IT Consultancy Group](https://itconsult.com.au/)** | Australia |
| **[MOQdigital](https://www.moqdigital.com/insights)** | Australia |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Europa (Alemania) |
| **[Nelite](https://www.exakis-nelite.com/offres/)** | Europa |
| **[Firma nueva](https://newsignature.com/technologies/express-route/)** | Europa |
| **[OneAs1a](https://www.oneas1a.com/connectivity.html)** | Asia |
| **[Orange Networks](https://www.orange-networks.com/blog/88-azureexpressroute)** | Europa |
| **[Perficient](https://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Norteamérica |
| **[Presidio](https://www.presidio.com/subpage/1107/microsoft-azure)** | Norteamérica |
| **[sol-tec](https://www.sol-tec.com/what-we-do/)** | Europa |
| **[Venha Pra Nuvem](https://venhapranuvem.com.br/)** | Sudamérica |
| **[Vigilant.IT](https://vigilant.it/networking-services/microsoft-azure-networking/)** | Australia |

## <a name="next-steps"></a>Pasos siguientes
* Para obtener más información acerca de ExpressRoute, consulte [P+F de ExpressRoute](expressroute-faqs.md).
* Asegúrese de que se cumplen todos los requisitos previos. Consulte [Requisitos previos de ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa de ubicación"
