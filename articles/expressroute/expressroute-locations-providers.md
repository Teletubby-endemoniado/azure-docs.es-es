---
title: 'Ubicaciones y proveedores de conectividad: Azure ExpressRoute | Microsoft Docs'
description: Este artículo ofrece una introducción detallada de ubicaciones en la que se ofrecen los servicios e información sobre cómo conectarse a regiones de Azure. Se ordenan por ubicación.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/26/2021
ms.author: duau
ms.openlocfilehash: f8da20d9226073fb28e364fc4823b7333f4cdae0
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131442756"
---
# <a name="expressroute-partners-and-peering-locations"></a>Asociados de ExpressRoute y ubicaciones de emparejamiento

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
Las ubicaciones de ExpressRoute (a las que a veces se denomina ubicaciones de emparejamiento o ubicaciones de punto de encuentro) son instalaciones de ubicación compartida en las que se encuentran los dispositivos de Microsoft Enterprise Edge (MSEE). Las ubicaciones de ExpressRoute son el punto de entrada a la red de Microsoft (y se distribuyen globalmente, lo que proporciona a los clientes la oportunidad de conectarse a la red de Microsoft en todo el mundo). Estas ubicaciones son donde los asociados de ExpressRoute y los clientes ExpressRoute Direct emiten conexiones cruzadas a la red de Microsoft. En general, no es preciso que la ubicación de ExpressRoute coincida con la región de Azure. Por ejemplo, un cliente puede crear un circuito ExpressRoute con la ubicación de recursos *Este de EE. UU.* , en la ubicación de emparejamiento *Seattle*.

Tendrá acceso a los servicios de Azure en todas las regiones dentro de una región geopolítica si se conectó al menos a una ubicación de ExpressRoute dentro de la región geopolítica. 

[!INCLUDE [expressroute-azure-regions-geopolitical-region](../../includes/expressroute-azure-regions-geopolitical-region.md)]

## <a name="expressroute-connectivity-providers"></a><a name="partners"></a>Proveedores de conectividad ExpressRoute

La siguiente tabla muestra las ubicaciones de conectividad y los proveedores de servicios para cada ubicación. Si desea ver los proveedores de servicios y las ubicaciones en las que pueden prestar servicio, consulte [Ubicaciones por proveedor de servicios](expressroute-locations.md).

* Las **regiones de Azure locales** son a las que puede acceder [ExpressRoute Local](expressroute-faqs.md) en cada ubicación de emparejamiento. **n/d** indica que ExpressRoute Local no está disponible en esa ubicación de emparejamiento.

* **Zona** hace referencia a los [precios](https://azure.microsoft.com/pricing/details/expressroute/).

### <a name="global-commercial-azure"></a>Servicios comerciales globales de Azure
| **Ubicación** | **Dirección** | **Zona** | **Regiones de Azure locales** | **ER Direct** | **Proveedores de servicios** |
| --- | --- | --- | --- | --- | --- |
| **Ámsterdam** | [Equinix AM5](https://www.equinix.com/locations/europe-colocation/netherlands-colocation/amsterdam-data-centers/am5/) | 1 | Oeste de Europa | 10 Gb/s, 100 Gb/s | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, Telefonica, Telenor, Telia Carrier, Verizon y Zayo |
| **Ámsterdam2** | [Interxion AMS8](https://www.interxion.com/Locations/amsterdam/schiphol/) | 1 | Oeste de Europa | 10 Gb/s, 100 Gb/s | BICS, British Telecom, CenturyLink Cloud Connect, Colt, DE-CIX, Equinix, euNetworks, GÉANT, Interxion, NOS, NTT Global DataCenters EMEA, Orange, Vodafone |
| **Atlanta** | [Equinix AT2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/atlanta-data-centers/at2/) | 1 | N/D | 10 Gb/s, 100 Gb/s | Equinix, Megaport |
| **Auckland** | [Vocus Group NZ Albany](https://www.vocus.co.nz/business/cloud-data-centres) | 2 | N/D | 10 Gb/s | Devoli, Kordia, Megaport, REANNZ, Spark NZ, Vocus Group NZ |
| **Bangkok** | [AIS](https://business.ais.co.th/solution/en/azure-expressroute.html) | 2 | N/D | 10 Gb/s | AIS, National Telecom UIH |
| **Berlín** | [NTT GDC](https://www.e-shelter.de/en/location/berlin-1-data-center) | 1 | Norte de Alemania | 10 Gb/s | Colt, Equinix, NTT Global DataCenters EMEA|
| **Bogotá** | [Equinix BG1](https://www.equinix.com/locations/americas-colocation/colombia-colocation/bogota-data-centers/bg1/) | 4 | N/D | 10 Gb/s | Equinix |
| **Busán** | [LG CNS](https://www.lgcns.com/En/Service/DataCenter) | 2 | Corea del Sur | N/D | LG CNS |
| **Campinas** | [Ascenty](https://www.ascenty.com/en/data-centers-en/campinas/) | 3 | Sur de Brasil | 10 Gb/s, 100 Gb/s | Ascenty |
| **Canberra** | [CDC](https://cdcdatacentres.com.au/about-us/) | 1 | Centro de Australia | 10 Gb/s, 100 Gb/s | CDC |
| **Canberra 2** | [CDC](https://cdcdatacentres.com.au/about-us/) | 1 | Centro de Australia 2| 10 Gb/s, 100 Gb/s | CDC, Equinix |
| **Ciudad del cabo** | [Teraco CT1](https://www.teraco.co.za/data-centre-locations/cape-town/) | 3 | Oeste de Sudáfrica | 10 Gb/s | BCX, Internet Solutions - Cloud Connect, Liquid Telecom, MTN Global Connect, Teraco, Vodacom |
| **Chennai** | Tata Communications | 2 | Sur de la India | 10 Gb/s | BSNL, Global CloudXchange (GCX), SIFY, Tata Communications, VodafoneIdea |
| **Chennai2** | Airtel | 2 | Sur de la India | 10 Gb/s | Airtel |
| **Chicago** | [Equinix CH1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/chicago-data-centers/ch1/) | 1 | Centro-Norte de EE. UU | 10 Gb/s, 100 Gb/s | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Cologix, Colt, Comcast, Coresite, Equinix, InterCloud, Internet2, Level 3 Communications, Megaport, PacketFabric, PCCW Global Limited, Sprint, Telia Carrier, Verizon, Zayo |
| **Chicago2** | [CoreSite CH1](https://www.coresite.com/data-center/ch1-chicago-il) | 1 | Centro-Norte de EE. UU | 10 Gb/s, 100 Gb/s | CoreSite |
| **Copenhague** | [Interxion CPH1](https://www.interxion.com/Locations/copenhagen/) | 1 | N/D | 10 Gb/s | Interxion |
| **Dallas** | [Equinix DA3](https://www.equinix.com/locations/americas-colocation/united-states-colocation/dallas-data-centers/da3/) | 1 | N/D | 10 Gb/s, 100 Gb/s | Aryaka Networks, AT&T NetBond, Cologix, Equinix, Internet2, Level 3 Communications, Megaport, Neutrona Networks, PacketFabric, Telmex Uninet, Telia Carrier, Transtelco, Verizon, Zayo|
| **Denver** | [CoreSite DE1](https://www.coresite.com/data-centers/locations/denver/de1) | 1 | Centro-Oeste de EE. UU. | 10 Gb/s, 100 Gb/s | CoreSite, Megaport, PacketFabric, Zayo |
| **Dubai** | [PCCS](https://www.pacificcontrols.net/cloudservices/index.html) | 3 | Norte de Emiratos Árabes Unidos | N/D | Etisalat UAE |
| **Dubai2** | [du datamena](http://datamena.com/solutions/data-centre) | 3 | Norte de Emiratos Árabes Unidos | N/D | DE-CIX, du datamena, Equinix, GBI, Megaport, Orange, Orixcom |
| **Dublín** | [Equinix DB3](https://www.equinix.com/locations/europe-colocation/ireland-colocation/dublin-data-centers/db3/) | 1 | Norte de Europa | 10 Gb/s, 100 Gb/s | CenturyLink Cloud Connect, Colt, eir, Equinix, GEANT, euNetworks, Interxion, Megaport |
| **Dublin2** | [Interxion DUB2](https://www.interxion.com/locations/europe/dublin) | 1 | Norte de Europa | 10 Gb/s, 100 Gb/s | Interxion |
| **Fráncfort** | [Interxion FRA11](https://www.interxion.com/Locations/frankfurt/) | 1 | Centro-oeste de Alemania | 10 Gb/s, 100 Gb/s | AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, DE-CIX, Equinix, euNetworks, GBI, GEANT, InterCloud, Interxion, Megaport, NTT Global DataCenters EMEA, Orange, Telia Carrier, T-Systems |
| **Frankfurt2** | [Equinix FR7](https://www.equinix.com/locations/europe-colocation/germany-colocation/frankfurt-data-centers/fr7/) | 1 | Centro-oeste de Alemania | 10 Gb/s, 100 Gb/s | Deutsche Telekom AG, Equinix |
| **Ginebra** | [Equinix GV2](https://www.equinix.com/locations/europe-colocation/switzerland-colocation/geneva-data-centers/gv2/) | 1 | Oeste de Suiza | 10 Gb/s, 100 Gb/s | Colt, Equinix, Megaport, Swisscom |
| **RAE de Hong Kong** | [Equinix HK1](https://www.equinix.com/data-centers/asia-pacific-colocation/hong-kong-colocation/hong-kong-data-centers/hk1) | 2 | Este de Asia | 10 Gb/s | Aryaka Networks, British Telecom, CenturyLink Cloud Connect, Chief Telecom, China Telecom Global, China Unicom, Colt, Equinix, InterCloud, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telia Carrier y Verizon |
| **Hong Kong2** | [iAdvantage MEGA-i](https://www.iadvantage.net/index.php/locations/mega-i) | 2 | Este de Asia | 10 Gb/s | China Mobile International, China Telecom Global, iAdvantage, Megaport, PCCW Global Limited, SingTel |
| **Yakarta** | NTT Communications, Telin, Telkom Indonesia | 4 | N/D | 10 Gb/s | Telin |
| **Johannesburgo** | [Teraco JB1](https://www.teraco.co.za/data-centre-locations/johannesburg/#jb1) | 3 | Norte de Sudáfrica | 10 Gb/s | BCX, British Telecom, Internet Solutions - Cloud Connect, Liquid Telecom, MTN Global Connect, Orange, Teraco, Vodacom |
| **Kuala Lumpur** | [TIME dotCom Menara AIMS](https://www.time.com.my/enterprise/connectivity/direct-cloud) | 2 | N/D | N/D | TIME dotCom |
| **Las Vegas** | [Switch LV](https://www.switch.com/las-vegas) | 1 | N/D | 10 Gb/s, 100 Gb/s | CenturyLink Cloud Connect, Megaport, PacketFabric |
| **Londres** | [Equinix LD5](https://www.equinix.com/locations/europe-colocation/united-kingdom-colocation/london-data-centers/ld5/) | 1 | Sur de Reino Unido | 10 Gb/s, 100 Gb/s | AT&T NetBond, British Telecom, CenturyLink, Colt, Equinix, euNetworks, InterCloud, Internet Solutions - Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telehouse - KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **Londres2** | [Telehouse North Two](https://www.telehouse.net/data-centres/emea/uk-data-centres/london-data-centres/north-two) | 1 | Sur de Reino Unido | 10 Gb/s, 100 Gb/s | BICS, British Telecom, CenturyLink Cloud Connect, Colt, GTT, IX Reach, Equinix, JISC, Megaport, NTT Global DataCenters EMEA, SES, Sohonet, Telehouse - KDDI |
| **Los Ángeles** | [CoreSite LA1](https://www.coresite.com/data-centers/locations/los-angeles/one-wilshire) | 1 | N/D | 10 Gb/s, 100 Gb/s | CoreSite, Equinix*, Megaport, Neutrona Networks, NTT, Zayo</br></br> **Los nuevos circuitos ExpressRoute ya no se admiten con Equinix en Los Ángeles. Cree nuevos circuitos en Los Ángeles2.* |
| **Los Angeles2** | [Equinix LA1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/los-angeles-data-centers/la1/) | 1 | N/D | 10 Gb/s, 100 Gb/s | Equinix |
| **Madrid** | [Interxion MAD1](https://www.interxion.com/es/donde-estamos/europa/madrid) | 1 | Oeste de Europa | 10 Gb/s, 100 Gb/s | Interxion, Megaport |
| **Marsella** |[Interxion MRS1](https://www.interxion.com/Locations/marseille/) | 1 | Sur de Francia | N/D | Colt, DE-CIX, GÉANT, Interxion, Jaguar Network, Ooredoo Cloud Connect |
| **Melbourne** | [NextDC M1](https://www.nextdc.com/data-centres/m1-melbourne-data-centre) | 2 | Sudeste de Australia | 10 Gb/s, 100 Gb/s | AARNet, Devoli, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation, TPG Telecom |
| **Miami** | [Equinix MI1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/miami-data-centers/mi1/) | 1 | N/D | 10 Gb/s, 100 Gb/s | Claro, C3ntro, Equinix, Megaport, Neutrona Networks |
| **Milán** | [IRIDEOS](https://irideos.it/en/data-centers/) | 1 | N/D | 10 Gb/s | Colt, Equinix, Fastweb, IRIDEOS, Retelit |
| **Mineápolis** | [Cologix MIN1](https://www.cologix.com/data-centers/minneapolis/min1/) | 1 | N/D | 10 Gb/s, 100 Gb/s | Cologix, Megaport |
| **Montreal** | [Cologix MTL3](https://www.cologix.com/data-centers/montreal/mtl3/) | 1 | N/D | 10 Gb/s, 100 Gb/s | Bell Canada, Cologix, Fibrenoire, Megaport, Telus, Zayo |
| **Mumbai (Bombay)** | Tata Communications | 2 | Oeste de la India | 10 Gb/s | BSNL, DE-CIX, Global CloudXchange (GCX), Reliance Jio, Sify, Tata Communications, Verizon |
| **Mumbai2** | Airtel | 2 | Oeste de la India | 10 Gb/s | Airtel, Sify y Vodafone Idea |
| **Múnich** | [EdgeConneX](https://www.edgeconnex.com/locations/europe/munich/) | 1 | N/D | 10 Gb/s | Colt, DE-CIX, Megaport |
| **Nueva York** | [Equinix NY5](https://www.equinix.com/locations/americas-colocation/united-states-colocation/new-york-data-centers/ny5/) | 1 | N/D | 10 Gb/s, 100 Gb/s | CenturyLink Cloud Connect, Colt, Coresite, DE-CIX, Equinix, InterCloud, Megaport, Packet, Zayo |
| **Newport (Gales)** | [Datos de última generación](https://www.nextgenerationdata.co.uk) | 1 | Oeste de Reino Unido | 10 Gb/s, 100 Gb/s | British Telecom, Colt, Jisc, Level 3 Communications, Next Generation Data |
| **Osaka** | [Equinix OS1](https://www.equinix.com/locations/asia-colocation/japan-colocation/osaka-data-centers/os1/) | 2 | Japón Occidental | 10 Gb/s, 100 Gb/s | AT TOKYO, BBIX, Colt, Equinix, Internet Initiative Japan Inc. - IIJ, Megaport, NTT Communications, NTT SmartConnect, Softbank, Tokai Communications |
| **Oslo** | [DigiPlex Ulven](https://www.digiplex.com/locations/oslo-datacentre) | 1 | Este de Noruega | 10 Gb/s, 100 Gb/s | GlobalConnect, Megaport, Telenor, Telia Carrier |
| **París** | [Interxion PAR5](https://www.interxion.com/Locations/paris/) | 1 | Centro de Francia | 10 Gb/s, 100 Gb/s | British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Intercloud, Interxion, Jaguar Network, Megaport, Orange, Telia Carrier, Zayo |
| **Perth** | [NextDC P1](https://www.nextdc.com/data-centres/p1-perth-data-centre) | 2 | N/D | 10 Gb/s | Megaport, NextDC |
| **Phoenix** | [EdgeConneX PHX01](https://www.edgeconnex.com/locations/north-america/phoenix-az/) | 1 | Oeste de EE. UU. 3 | 10 Gb/s, 100 Gb/s | Megaport, Zayo |
| **Pune** | [STT GDC Pune DC1](https://www.sttelemediagdc.in/our-data-centres-in-india) | 2 | Centro de la India| 10 Gb/s | |
| **Quebec ciudad** | [Vantage](https://vantage-dc.com/data_centers/quebec-city-data-center-campus/) | 1 | Este de Canadá | 10 Gb/s, 100 Gb/s | Bell Canada, Equinix, Megaport, Telus |
| **Queretaro (México)** | [KIO Networks QR01](https://www.kionetworks.com/es-mx/) | 4 | N/D | 10 Gb/s | Transtelco|
| **Quincy** | [Sabey Datacenter (edificio A)](https://sabeydatacenters.com/data-center-locations/central-washington-data-centers/quincy-data-center) | 1 | Oeste de EE. UU. 2 | 10 Gb/s, 100 Gb/s | |
| **Rio de Janeiro** | [Equinix-RJ2](https://www.equinix.com/locations/americas-colocation/brazil-colocation/rio-de-janeiro-data-centers/rj2/) | 3 | Sur de Brasil | 10 Gb/s | Equinix |
| **San Antonio** | [CyrusOne SA1](https://cyrusone.com/locations/texas/san-antonio-texas/) | 1 | Centro-sur de EE. UU. | 10 Gb/s, 100 Gb/s | CenturyLink Cloud Connect, Megaport |
| **Sao Paulo** | [Equinix SP2](https://www.equinix.com/locations/americas-colocation/brazil-colocation/sao-paulo-data-centers/sp2/) | 3 | Sur de Brasil | 10 Gb/s, 100 Gb/s | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica y UOLDIVEO |
| **Sao Paulo2** | [TIVIT TSM](https://www.tivit.com/en/tivit/) | 3 | Sur de Brasil | 10 Gb/s, 100 Gb/s | |
| **Seattle** | [Equinix SE2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/seattle-data-centers/se2/) | 1 | Oeste de EE. UU. 2 | 10 Gb/s, 100 Gb/s | Aryaka Networks, Equinix, Level 3 Communications, Megaport, Telus, Zayo |
| **Seúl** | [KINX Gasan IDC](https://www.kinx.net/?lang=en) | 2 | Centro de Corea del Sur | 10 Gb/s, 100 Gb/s | KINX, KT, LG CNS, LGUplus, Equinix, Sejong Telecom, SK Telecom |
| **Seúl2** | [KT IDC](https://www.kt-idc.com/eng/introduce/sub1_4_10.jsp#tab) | 2 | Centro de Corea del Sur | N/D | |
| **Silicon Valley** | [Equinix SV1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/silicon-valley-data-centers/sv1/) | 1 | Oeste de EE. UU. | 10 Gb/s, 100 Gb/s | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Comcast, Coresite, Equinix, InterCloud, Internet2, IX Reach, Packet, PacketFabric, Level 3 Communications, Megaport, Orange, Sprint, Tata Communications, Telia Carrier, Verizon, Zayo |
| **Silicon Valley2** | [Coresite SV7](https://www.coresite.com/data-centers/locations/silicon-valley/sv7) | 1 | Oeste de EE. UU. | 10 Gb/s, 100 Gb/s | Colt, Coresite | 
| **Singapur** | [Equinix SG1](https://www.equinix.com/data-centers/asia-pacific-colocation/singapore-colocation/singapore-data-center/sg1) | 2 | Sudeste de Asia | 10 Gb/s, 100 Gb/s | Aryaka Networks, AT&T NetBond, British Telecom, China Mobile International, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Singapur2** | [Global Switch Tai Seng](https://www.globalswitch.com/locations/singapore-data-centres/) | 2 | Sudeste de Asia | 10 Gb/s, 100 Gb/s | CenturyLink Cloud Connect, China Unicom Global, Colt, Epsilon Global Communications, Equinix, Megaport, PCCW Global Limited, SingTel, Telehouse - KDDI |
| **Stavanger** | [Green Mountain DC1](https://greenmountain.no/dc1-stavanger/) | 1 | Oeste de Noruega | 10 Gb/s, 100 Gb/s |GlobalConnect, Megaport |
| **Estocolmo** | [Equinix SK1](https://www.equinix.com/locations/europe-colocation/sweden-colocation/stockholm-data-centers/sk1/) | 1 | N/D | 10 Gb/s | Equinix, Megaport, Telia Carrier |
| **Sidney** | [Equinix SY2](https://www.equinix.com/locations/asia-colocation/australia-colocation/sydney-data-centers/sy2/) | 2 | Este de Australia | 10 Gb/s, 100 Gb/s | AARNet, AT&T NetBond, British Telecom, Devoli, Equinix, Kordia, Megaport, NEXTDC, NTT Communications, Optus, Orange, Spark NZ, Telstra Corporation, TPG Telecom, Verizon, Vocus Group NZ |
| **Sydney2** | [NextDC S1](https://www.nextdc.com/data-centres/s1-sydney-data-centre) | 2 | Este de Australia | 10 Gb/s, 100 Gb/s | Megaport, NextDC |
| **Taipéi** | Chief Telecom | 2 | N/D | 10 Gb/s | Chief Telecom, Chunghwa Telecom, FarEasTone |
| **Tokio** | [Equinix TY4](https://www.equinix.com/locations/asia-colocation/japan-colocation/tokyo-data-centers/ty4/) | 2 | Japón Oriental | 10 Gb/s, 100 Gb/s | Aryaka Networks, AT&T NetBond, BBIX, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Intercloud, Internet Initiative Japan Inc. - IIJ, Megaport, NTT Communications, NTT EAST, Orange, Softbank, Telehouse - KDDI, Verizon |
| **Tokyo2** | [AT TOKYO](https://www.attokyo.com/) | 2 | Japón Oriental | 10 Gb/s, 100 Gb/s | AT TOKYO, China Unicom Global, Megaport, Tokai Communications |
| **Toronto** | [Cologix TOR1](https://www.cologix.com/data-centers/toronto/tor1/) | 1 | Centro de Canadá | 10 Gb/s, 100 Gb/s | AT&T NetBond, Bell Canada, CenturyLink Cloud Connect, Cologix, Equinix, IX Reach Megaport, Telus, Verizon, Zayo |
| **Toronto2** | [Allied REIT](https://www.alliedreit.com/property/905-king-st-w/) | 1 | Centro de Canadá | 10 Gb/s, 100 Gb/s | |
| **Vancouver** | [Cologix VAN1](https://www.cologix.com/data-centers/vancouver/van1/) | 1 | N/D | 10 Gb/s | Bell Canada, Cologix, Megaport, Telus |
| **Washington DC** | [Equinix DC2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/washington-dc-data-centers/dc2/), [Equinix DC6](https://www.equinix.com/data-centers/americas-colocation/united-states-colocation/washington-dc-data-centers/dc6) | 1 | Este de EE. UU., Este de EE. UU. 2 | 10 Gb/s, 100 Gb/s | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Cologix, Colt, Comcast, Coresite, Equinix, Internet2, InterCloud, Iron Mountain, IX Reach, Level 3 Communications, Megaport, Neutrona Networks, NTT Communications, Orange, PacketFabric, SES, Sprint, Tata Communications, Telia Carrier, Verizon, Zayo |
| **Washington DC2** | [Coresite VA2](https://www.coresite.com/data-center/va2-reston-va) | 1 | Este de EE. UU., Este de EE. UU. 2 | 10 Gb/s, 100 Gb/s | CenturyLink Cloud Connect, Coresite, Intelsat, Megaport, Viasat, Zayo | 
| **Zúrich** | [Interxion ZUR2](https://www.interxion.com/Locations/zurich/) | 1 | Norte de Suiza | 10 Gb/s, 100 Gb/s | Colt, Equinix, Intercloud, Interxion, Megaport y Swisscom |

 **+** indica próximamente

### <a name="national-cloud-environments"></a>Entornos de nube nacionales

Las nubes nacionales de Azure están aisladas entre sí y de los servicios comerciales globales de Azure. ExpressRoute para una nube de Azure no se puede conectar a las regiones de Azure de otras nubes.

### <a name="us-government-cloud"></a>Nube del US Gov
| **Ubicación** | **Dirección** | **Regiones de Azure locales**| **ER Direct** | **Proveedores de servicios** |
| --- | --- | --- | --- | --- |
| **Atlanta** | [Equinix AT1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/atlanta-data-centers/at1/) | N/D | 10 Gb/s, 100 Gb/s | Equinix |
| **Chicago** | [Equinix CH1](https://www.equinix.com/locations/americas-colocation/united-states-colocation/chicago-data-centers/ch1/) | N/D | 10 Gb/s, 100 Gb/s | AT&T NetBond, British Telecom, Equinix, Level 3 Communications, Verizon |
| **Dallas** | [Equinix DA3](https://www.equinix.com/locations/americas-colocation/united-states-colocation/dallas-data-centers/da3/) | N/D | 10 Gb/s, 100 Gb/s | Equinix, Internet2, Megaport, Verizon |
| **Nueva York** | [Equinix NY5](https://www.equinix.com/locations/americas-colocation/united-states-colocation/new-york-data-centers/ny5/) | N/D | 10 Gb/s, 100 Gb/s | Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | [CyrusOne Chandler](https://cyrusone.com/locations/arizona/phoenix-arizona-chandler/) | US Gov: Arizona | 10 Gb/s, 100 Gb/s | AT&T NetBond, CenturyLink Cloud Connect, Megaport |
| **San Antonio** | [CyrusOne SA2](https://cyrusone.com/locations/texas/san-antonio-texas-ii/) | US Gov Texas | 10 Gb/s, 100 Gb/s | CenturyLink Cloud Connect, Megaport |
| **Silicon Valley** | [Equinix SV4](https://www.equinix.com/locations/americas-colocation/united-states-colocation/silicon-valley-data-centers/sv4/) | N/D | 10 Gb/s, 100 Gb/s | AT&T, Equinix, Level 3 Communications, Verizon |
| **Seattle** | [Equinix SE2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/seattle-data-centers/se2/) | N/D | 10 Gb/s, 100 Gb/s | Equinix, Megaport |
| **Washington DC** | [Equinix DC2](https://www.equinix.com/locations/americas-colocation/united-states-colocation/washington-dc-data-centers/dc2/) | US DoD (este), US Gov Virginia | 10 Gb/s, 100 Gb/s | AT&T NetBond, CenturyLink Cloud Connect, Equinix, Level 3 Communications, Megaport, Verizon |

### <a name="china"></a>China
| **Ubicación** | **Dirección** | **Regiones de Azure locales** | **ER Direct** | **Proveedores de servicios** |
| --- | --- | --- | --- | --- |
| **Beijing** | China Telecom | N/D | 10 Gb/s | China Telecom |
| **Beijing2** | GDS | N/D | 10 Gb/s | China Telecom, China Unicom, GDS |
| **Shanghai** | China Telecom | N/D | 10 Gb/s | China Telecom |
| **Shanghai2** | GDS | N/D | 10 Gb/s | China Telecom, China Unicom, GDS |

Para más información, consulte [ExpressRoute en China](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Alemania
| **Ubicación** | **Proveedores de servicios** |
| --- | --- |
| **Berlín** |e-shelter, Megaport+, T-Systems |
| **Fráncfort** |Colt, Equinix e Interxion |

## <a name="connectivity-through-exchange-providers"></a><a name="c1partners"></a>Conectividad a través de proveedores de intercambio
Si su proveedor de conectividad no aparece en la lista de las secciones anteriores, puede crear una conexión.

* Consulte con el proveedor de conectividad para ver si existe una conexión con cualquiera de los intercambios de la tabla anterior. Puede comprobar los vínculos siguientes para recopilar más información sobre los servicios ofrecidos por proveedores de Exchange. Varios proveedores de conectividad ya están conectados a los intercambios de Ethernet.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [DE-CIX](https://www.de-cix.net/en/de-cix-service-world/cloud-exchange)
  * [Equinix Cloud Exchange](https://www.equinix.com/resources/videos/cloud-exchange-overview)
  * [InterXion](https://www.interxion.com/)
  * [NextDC](https://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
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

## <a name="connectivity-through-additional-service-providers"></a><a name="c1partners"></a>Conectividad a través de proveedores de servicios adicionales
| **Ubicación** | **Exchange** | **Proveedores de conectividad** |
| --- | --- | --- |
| **Ámsterdam** | Equinix, Interxion, Level 3 Communications | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf Bridge International, Kalaam Telecom Bahrain B.S.C, MainOne, Nianet, POST Telecom Luxembourg, Proximus, RETN, TDC Erhverv, Telecom Italia Sparkle, Telekom Deutschland GmbH, Telia |
| **Atlanta** | Equinix| Crown Castle
| **Ciudad del cabo** | Teraco | MTN |
| **Chennai** | Tata Communications | Tata Teleservices |
| **Chicago** | Equinix| Crown Castle, Spectrum Enterprise, Windstream |
| **Dallas** | Equinix, Megaport | Axtel, C3ntro Telecom, Cox Business, Crown Castle, Data Foundry, Spectrum Enterprise, Transtelco |
| **Fráncfort** | Interxion | BICS, Cinia, Equinix, Nianet, QSC AG, Telekom Deutschland GmbH |
| **Hamburgo** | Equinix | Cinia |
| **RAE de Hong Kong** | Equinix | Chief y Macroview Telecom |
| **Johannesburgo** | Teraco | MTN |
| **Londres** | BICS, Equinix, euNetworks| Bezeq International Ltd., CoreAzure, Epsilon Telecommunications Limited, Exponential E, HSO, NexGen Networks, Proximus, Tamares Telecom, Zain |
| **Los Ángeles** | Equinix |Crown Castle, Spectrum Enterprise, Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix| Airgate Technologies, Inc. Aptum Technologies, Rogers, Zirro |
| **Mumbai (Bombay)** | Tata Communications | Tata Teleservices |
| **Nueva York** |Equinix, Megaport | Altice Business, Crown Castle, Spectrum Enterprise, Webair |
| **París** | Equinix | Proximus |
| **Quebec ciudad** | Megaport | Fibrenoire |
| **Sao Paulo** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Silicon Valley** |Coresite, Equinix | Cox Business, Spectrum Enterprise, Windstream, X2nsat Inc. |
| **Singapur** |Equinix |1CLOUDSTAR, BICS, CMC Telecom, Epsilon Telecommunications Limited, LGA Telecom y United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sidney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix, Megaport | Airgate Technologies Inc., Beanfield Metroconnect, Aptum Technologies, IVedha Inc, Oncore Cloud Services Inc., Rogers, Thinktel, Zirro|
| **Washington DC** |Equinix | Altice Business, BICS, Cox Business, Crown Castle, Gtt Communications Inc, Epsilon Telecommunications Limited, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>Integradores de sistemas de ExpressRoute
Habilitar la conectividad privada para la adaptación a sus necesidades puede ser complicado según la escala de la red. Puede trabajar con cualquiera de los integradores de sistemas que aparecen en la tabla siguiente para ayudarle con la incorporación a ExpressRoute.

| **Continente** | **Integradores de sistemas** |
| --- | --- |
| **Asia** |Avanade Inc. y OneAs1a |
| **Australia** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Europa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Norteamérica** |Avanade Inc., Equinix Professional Services, FlexManage, Lightstream, Perficient y Presidio |
| **Sudamérica** |Avanade Inc., Venha Pra Nuvem |
## <a name="next-steps"></a>Pasos siguientes
* Para obtener más información acerca de ExpressRoute, consulte [P+F de ExpressRoute](expressroute-faqs.md).
* Asegúrese de que se cumplen todos los requisitos previos. Consulte [Requisitos previos de ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa de ubicación"
