---
title: Preguntas más frecuentes sobre Azure Virtual WAM | Microsoft Docs
description: Vea las respuestas a preguntas frecuentes sobre redes, clientes, puertas de enlace, dispositivos, asociados y conexiones de Azure Virtual WAN.
author: cherylmc
ms.service: virtual-wan
ms.topic: troubleshooting
ms.date: 08/18/2021
ms.author: cherylmc
ms.openlocfilehash: e28d5c9358077e072c31026bdc164a9b2037a40a
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132492481"
---
# <a name="virtual-wan-faq"></a>Preguntas más frecuentes sobre Virtual WAN

### <a name="is-azure-virtual-wan-in-ga"></a>¿Qué es Azure Virtual WAN en GA?

Sí, Azure Virtual WAN está disponible con carácter general (GA). Sin embargo, Virtual WAN consta de varias características y escenarios. Hay características o escenarios dentro de Virtual WAN en los que Microsoft aplica la etiqueta de versión preliminar. En esos casos, la característica específica o el propio escenario se encuentra en versión preliminar. Si no usa una característica específica en versión preliminar, se aplica la compatibilidad GA normal. Para más información sobre la compatibilidad de la versión preliminar, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="does-the-user-need-to-have-hub-and-spoke-with-sd-wanvpn-devices-to-use-azure-virtual-wan"></a>¿El usuario necesita disponer de una topología radial con los dispositivos SD-WAN/VPN para usar Azure Virtual WAN?

Virtual WAN proporciona muchas funcionalidades integradas en un único panel, como la conectividad VPN de sitio a sitio, la conectividad de usuario y P2S, la conectividad de ExpressRoute, la conectividad de Virtual Network, la interconectividad de ExpressRoute de VPN, la conectividad transitiva de red virtual a red virtual, el enrutamiento centralizado, Azure Firewall y la seguridad de Firewall Manager, la supervisión, el cifrado de ExpressRoute y muchas otras funcionalidades. No es necesario disponer de todos estos casos de uso para empezar a usar Virtual WAN. Puede empezar a trabajar con uno solo de estos.

La arquitectura de Virtual WAN es una arquitectura "hub and spoke" con escalado y rendimiento integrados, donde las ramas (dispositivos VPN/SD-WAN), los usuarios (clientes de VPN de Azure, openVPN o IKEv2), los circuitos ExpressRoute y las instancias de Virtual Network sirven como radios para los centros de conectividad virtuales. Todos los centros de conectividad están conectados en una malla completa en una red WAN virtual estándar, lo que facilita al usuario el uso de la red troncal de Microsoft para la conectividad de cualquier tipo (cualquier radio). En el caso de la topología radial de los dispositivos SD-WAN/VPN, los usuarios pueden configurarla manualmente en el portal de Azure Virtual WAN o usar el CPE del asociado de Virtual WAN (SD-WAN/VPN) para configurar la conectividad con Azure.

Los asociados de Virtual WAN proporcionan automatización de la conectividad, que es la capacidad de exportar la información del dispositivo a Azure, descargar la configuración de Azure y establecer la conectividad con el centro de conectividad de Azure Virtual WAN. Para la conectividad de punto a sitio/VPN de usuario, se admiten el [cliente de VPN de Azure](https://go.microsoft.com/fwlink/?linkid=2117554), OpenVPN o IKEv2.

### <a name="can-you-disable-fully-meshed-hubs-in-a-virtual-wan"></a>¿Se pueden deshabilitar los centros de conectividad totalmente en malla en una instancia de Virtual WAN?

Virtual WAN se ofrece en dos variedades: Básico y Estándar. En la instancia de Virtual WAN básica, los centros de conectividad no están en malla. En una instancia de Virtual WAN estándar, los centros de conectividad están en malla y se conectan automáticamente cuando la instancia se configura por primera vez. No es necesario que el usuario haga nada específico. El usuario tampoco tiene que deshabilitar ni habilitar la funcionalidad para obtener concentradores en malla. Virtual WAN ofrece muchas opciones de enrutamiento para dirigir el tráfico entre radios cualesquiera (VNet, VPN o ExpressRoute). Proporciona la facilidad de los centros de conectividad totalmente en malla, así como la flexibilidad de enrutar el tráfico según sus necesidades.

### <a name="how-are-availability-zones-and-resiliency-handled-in-virtual-wan"></a>¿Cómo se administran Availability Zones y la resistencia en Virtual WAN?

Virtual WAN es una colección de centros de conectividad y servicios que están disponibles en el centro de conectividad. El usuario puede tener tantas instancias de Virtual WAN como necesite. En un centro de Virtual WAN hay varios servicios, como VPN, ExpressRoute, etc. Cada uno de estos servicios se implementa automáticamente en Availability Zones (excepto Azure Firewall), siempre y cuando la región admita Availability Zones. Si una región se convierte en una zona de disponibilidad después de la implementación inicial en el centro de conectividad, el usuario puede volver a crear las puertas de enlace, lo que desencadenará una implementación de Availability Zones. Todas las puertas de enlace se aprovisionan en un centro de conectividad como activo-activo, lo que implica que hay resistencia integrada dentro de un centro de conectividad. Los usuarios pueden conectarse a varios centros de conectividad si desean resistencia entre regiones. 

Actualmente, Azure Firewall se puede implementar de modo que admita Availability Zones mediante el portal de Azure Firewall Manager, [PowerShell](/powershell/module/az.network/new-azfirewall#example-6--create-a-firewall-with-no-rules-and-with-availability-zones) o la CLI. De momento no hay forma de configurar un firewall existente para implementarlo en zonas de disponibilidad. Tiene que eliminar y volver a implementar la instancia de Azure Firewall. 

Aunque el concepto de Virtual WAN es global, el recurso de Virtual WAN real se basa en Resource Manager y se implementa de forma regional. En caso de que la propia región de Virtual WAN tenga un problema, todos los centros de conectividad de esa instancia de Virtual WAN seguirán funcionando tal cual, pero el usuario no podrá crear nuevos centros de conectividad hasta que la región de Virtual WAN esté disponible.

### <a name="what-client-does-the-azure-virtual-wan-user-vpn-point-to-site-support"></a>¿Qué cliente admite la red privada virtual de usuario (de punto a sitio) de Azure Virtual WAN?

Virtual WAN admite el [cliente de VPN de Azure](https://go.microsoft.com/fwlink/?linkid=2117554), el cliente de OpenVPN o cualquier cliente de IKEv2. La autenticación de Azure AD se admite con el cliente de VPN de Azure. Como mínimo, se requiere la versión 17763.0 o superior del sistema operativo de cliente de Windows 10.  Los clientes de OpenVPN pueden admitir la autenticación basada en certificados. Una vez que se haya seleccionado la autenticación basada en certificados en la puerta de enlace, verá el archivo .ovpn* que se va a descargar en el dispositivo. IKEv2 admite la autenticación basada en certificados y RADIUS. 

### <a name="for-user-vpn-point-to-site--why-is-the-p2s-client-pool-split-into-two-routes"></a>En el caso de la red privada virtual de usuario (de punto a sitio), ¿por qué el grupo de clientes de P2S se divide en dos rutas?

Cada puerta de enlace tiene dos instancias; la división se produce para que cada instancia de la puerta de enlace pueda asignar de forma independiente las direcciones IP de cliente para los clientes conectados, y el tráfico de la red virtual se redirija a la instancia de puerta de enlace correcta para evitar el salto de instancias entre puertas de enlace.

### <a name="how-do-i-add-dns-servers-for-p2s-clients"></a>¿Cómo puedo agregar servidores DNS para los clientes de P2S?

Hay dos opciones para agregar servidores DNS para los clientes de P2S. Se prefiere el primer método porque agrega los servidores DNS personalizados a la puerta de enlace en lugar del cliente.

1. Use el siguiente script de PowerShell para agregar los servidores DNS personalizados. Reemplace los valores por los de su entorno.

   ```powershell
   // Define variables
   $rgName = "testRG1"
   $virtualHubName = "virtualHub1"
   $P2SvpnGatewayName = "testP2SVpnGateway1"
   $vpnClientAddressSpaces = 
   $vpnServerConfiguration1Name = "vpnServerConfig1"
   $vpnClientAddressSpaces = New-Object string[] 2
   $vpnClientAddressSpaces[0] = "192.168.2.0/24"
   $vpnClientAddressSpaces[1] = "192.168.3.0/24"
   $customDnsServers = New-Object string[] 2
   $customDnsServers[0] = "7.7.7.7"
   $customDnsServers[1] = "8.8.8.8"
   $virtualHub = $virtualHub = Get-AzVirtualHub -ResourceGroupName $rgName -Name $virtualHubName
   $vpnServerConfig1 = Get-AzVpnServerConfiguration -ResourceGroupName $rgName -Name $vpnServerConfiguration1Name

   // Specify custom dns servers for P2SVpnGateway VirtualHub while creating gateway
   createdP2SVpnGateway = New-AzP2sVpnGateway -ResourceGroupName $rgname -Name $P2SvpnGatewayName -VirtualHub $virtualHub -VpnGatewayScaleUnit 1 -VpnClientAddressPool $vpnClientAddressSpaces -VpnServerConfiguration $vpnServerConfig1 -CustomDnsServer $customDnsServers

   // Specify custom dns servers for P2SVpnGateway VirtualHub while updating existing gateway
   $P2SVpnGateway = Get-AzP2sVpnGateway -ResourceGroupName $rgName -Name $P2SvpnGatewayName
   $updatedP2SVpnGateway = Update-AzP2sVpnGateway -ResourceGroupName $rgName -Name $P2SvpnGatewayName  -CustomDnsServer $customDnsServers 

   // Re-generate Vpn profile either from PS/Portal for Vpn clients to have the specified dns servers
   ```
2. O bien, si usa el cliente de VPN de Azure para Windows 10, puede modificar el archivo XML de perfil descargado y agregar las etiquetas **\<dnsservers>\<dnsserver> \</dnsserver>\</dnsservers>** antes de importarlo.

   ```powershell
      <azvpnprofile>
      <clientconfig>

          <dnsservers>
              <dnsserver>x.x.x.x</dnsserver>
              <dnsserver>y.y.y.y</dnsserver>
          </dnsservers>
    
      </clientconfig>
      </azvpnprofile>
   ```

### <a name="for-user-vpn-point-to-site--how-many-clients-are-supported"></a>En el caso de la red privada virtual de usuario (de punto a sitio), ¿cuántos clientes se admiten?

Cada puerta de enlace de VPN de punto a sitio de usuario tiene dos instancias. Cada instancia admite un número determinado de conexiones a medida que cambian las unidades de escalado. La unidad de escalado 1-3 admite 500 conexiones, la unidad de escalado 4-6 admite 1000 conexiones, la unidad de escalado 7-12 admite 5000 conexiones y la unidad de escalado 13-18 admite hasta 10 000 conexiones.

Como ejemplo, supongamos que el usuario elige 1 unidad de escalado. Cada unidad de escalado implica una puerta de enlace activo-activo implementada, y cada una de las instancias (2, en este caso) admitiría hasta 500 conexiones. Dado que puede obtener 500 conexiones x 2 por puerta de enlace, no significa que tenga previstas 1000 en lugar de 500 para esta unidad de escalado. Es posible que se deban atender las instancias en que la conectividad de las 500 conexiones adicionales pueda sufrir interrupciones si supera el recuento de conexiones recomendado. Además, asegúrese de planear el tiempo de inactividad en caso de que decida escalar o reducir verticalmente en la unidad de escalado o cambiar la configuración de punto a sitio en VPN Gateway.

### <a name="what-are-virtual-wan-gateway-scale-units"></a>¿Qué son las unidades de escalado de puerta de enlace de Virtual WAN?

Una unidad de escalado es una unidad definida para obtener un rendimiento agregado de una puerta de enlace de un centro de conectividad virtual. 1 unidad de escalado de VPN = 500 Mbps. 1 unidad de escalado de ExpressRoute = 2 Gbps. Ejemplo: 10 unidades de escalado de VPN significarían 500 Mbps * 10 = 5 Gbps

### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpn-gateway"></a>¿Cuál es la diferencia entre una puerta de enlace de Azure Virtual Network (VPN Gateway) y una instancia de VPN Gateway de Azure Virtual WAN?

Virtual WAN proporciona conectividad de sitio a sitio y a gran escala, y se ha creado para mejorar el rendimiento, la escalabilidad y la facilidad de uso. Cuando conecta un sitio a una instancia de VPN Gateway de Virtual WAN, esto es diferente a una puerta de enlace de red virtual normal que usa un tipo de puerta de enlace "VPN". Del mismo modo, cuando se conecta un circuito ExpressRoute a un centro de conectividad de Virtual WAN, se usa un recurso para la puerta de enlace de ExpressRoute que es distinto al de la puerta de enlace de red virtual normal que usa el tipo de puerta de enlace "ExpressRoute". 

Virtual WAN admite un rendimiento agregado de hasta 20 Gbps para VPN y ExpressRoute. Virtual WAN también tiene automatización para la conectividad con un ecosistema de asociados de dispositivos de la rama CPE. Los dispositivos de la rama CPE tienen una automatización integrada que se aprovisiona automáticamente y se conecta a Azure Virtual WAN. Estos dispositivos están disponibles en un creciente ecosistema de asociados de VPN y SD-WAN. Consulte la [lista de asociados preferidos](virtual-wan-locations-partners.md).

### <a name="how-is-virtual-wan-different-from-an-azure-virtual-network-gateway"></a>¿En qué difiere Virtual WAN de una puerta de enlace de Azure Virtual Network?

Una VPN de puerta de enlace de red virtual se limita a 30 túneles. En las conexiones, debe usar Virtual WAN para VPN a gran escala. Puede conectar hasta 1000 conexiones de rama por centro virtual con agregados de 20 Gbps por centro. Una conexión es un túnel de activo a activo del dispositivo VPN local al concentrador virtual. También puede tener varios centros virtuales por región, lo que significa que puede conectar más de 1000 ramas a una única región de Azure mediante la implementación de varios centros de conectividad de Virtual WAN en esa región de Azure, cada uno con su propia puerta de enlace de VPN de sitio a sitio.

### <a name="what-is-the-recommended-packets-per-second-limit-per-ipsec-tunnel"></a>¿Cuál es el límite recomendado de paquetes por segundo por túnel IPsec?

Recomendamos enviar alrededor de 95 000 paquetes por segundo (PPS) con el algoritmo GCMAES256 para el cifrado y la integridad IPsec para lograr un rendimiento óptimo. Aunque el tráfico no se bloquea si se envían más de 95 000 PPS, esto puede dar lugar a una degradación del rendimiento, como la pérdida de paquetes y de latencia. Cree túneles adicionales si se requiere más PPS.

### <a name="which-device-providers-virtual-wan-partners-are-supported"></a>¿Qué proveedores de dispositivos (asociados de Virtual WAN) se admiten?

En este momento, muchos asociados admiten la experiencia de Virtual WAN totalmente automatizada. Para más información, consulte [Asociados de Virtual WAN](virtual-wan-locations-partners.md).

### <a name="what-are-the-virtual-wan-partner-automation-steps"></a>¿Cuáles son los pasos de automatización de asociados de Virtual WAN?

Para conocer estos pasos, consulte [Automatización de asociados de Virtual WAN](virtual-wan-configure-automation-providers.md).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>¿Debo usar un dispositivo de asociado preferido?

No. Puede usar cualquier dispositivo compatible con VPN que cumpla los requisitos de Azure para la compatibilidad con IPsec de IKEv1 o IKEv2. Virtual WAN también tiene soluciones de asociado de CPE que automatizan la conectividad con Azure Virtual WAN, lo que facilita la configuración de conexiones VPN de IPsec a escala.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>¿Cómo automatizan la conectividad con Azure Virtual WAN los asociados de Virtual WAN?

Las soluciones de conectividad definidas por software suelen administrar sus dispositivos de rama con un controlador o un centro de aprovisionamiento de dispositivos. El controlador puede usar las API de Azure para automatizar la conectividad a Azure Virtual WAN. La automatización incluye la carga de información de la rama, la descarga de la configuración de Azure, la configuración de túneles IPsec en los centros de conectividad virtuales de Azure y la configuración automática de la conectividad desde el dispositivo de rama a Azure Virtual WAN. Cuando tiene cientos de ramas, la conexión mediante asociados de CPE de Virtual WAN es fácil ya que la experiencia de incorporación elimina la necesidad de configurar y administrar la conectividad de IPsec a gran escala. Para más información, consulte [Automatización de asociados de Virtual WAN](virtual-wan-configure-automation-providers.md).

### <a name="what-if-a-device-i-am-using-is-not-in-the-virtual-wan-partner-list-can-i-still-use-it-to-connect-to-azure-virtual-wan-vpn"></a>¿Qué ocurre si el dispositivo que uso no está en la lista de partners de Virtual WAN? ¿Lo puedo usar a pesar de todo para conectarme a una VPN de Azure Virtual WAN?

Sí, siempre que el dispositivo admita IPsec IKEv1 o IKEv2. Los asociados de Virtual WAN automatizan la conectividad desde el dispositivo hasta los puntos de conexión de VPN de Azure. Esto supone automatizar pasos como "carga de información de la rama", "IPsec y configuración" y "conectividad". Dado que el dispositivo no es del ecosistema de un asociado de Virtual WAN, tendrá que realizar el pesado trabajo de configurar manualmente Azure y actualizar el dispositivo para configurar la conectividad de IPsec.

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>¿Cómo consiguen los nuevos asociados que no aparecen en la lista de asociados de partida ser incluidos en ella?

Todas las API de Virtual WAN son API abiertas. Puede consultar la documentación de [Automatización de asociados de Virtual WAN](virtual-wan-configure-automation-providers.md) para valorar la viabilidad técnica. Un asociado ideal es aquel que tiene un dispositivo que se puede aprovisionar para la conectividad IPsec de IKEv1 o IKEv2. Una vez que la compañía haya completado el trabajo de automatización para su dispositivo CPE según las instrucciones de automatización proporcionadas anteriormente, puede ponerse en contacto con azurevirtualwan@microsoft.com para que se muestre en [Conectividad mediante asociados](virtual-wan-locations-partners.md#partners). Si es un cliente que desea que una determinada solución de la empresa aparezca como un asociado de Virtual WAN, haga que la empresa se ponga en contacto con Virtual WAN mediante el envío de un correo electrónico a azurevirtualwan@microsoft.com.

### <a name="how-is-virtual-wan-supporting-sd-wan-devices"></a>¿Cómo es que Virtual WAN admite dispositivos SD-WAN?

Los asociados de Virtual WAN automatizan la conectividad IPsec a los puntos de conexión de VPN de Azure. Si el asociado de Virtual WAN es un proveedor de SD-WAN, se supone que el controlador de SD-WAN administra la automatización y la conectividad IPsec a los puntos de conexión de VPN de Azure. Si el dispositivo SD-WAN necesita su propio punto de conexión en lugar de la VPN de Azure para obtener funcionalidad de SD-WAN propietaria, puede implementar el punto de conexión de SD-WAN en una red virtual de Azure y coexistir con Azure Virtual WAN.

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>¿Cuántos dispositivos VPN pueden conectarse a un único centro?

Se admiten hasta 1000 conexiones por concentrador virtual. Cada conexión consta de cuatro vínculos y cada conexión de vínculo admite dos túneles que están en una configuración activo-activo. Los túneles terminan en un elemento VPN Gateway del centro de conectividad virtual de Azure. Los vínculos representan el vínculo del ISP físico en la sucursal o el dispositivo VPN.

### <a name="what-is-a-branch-connection-to-azure-virtual-wan"></a>¿Qué es una conexión de rama a Azure Virtual WAN?

Una conexión desde una rama o un dispositivo VPN a Azure Virtual WAN es una conexión VPN que se conecta de forma virtual al sitio VPN y a Azure VPN Gateway en un centro de conectividad virtual.

### <a name="what-happens-if-the-on-premises-vpn-device-only-has-1-tunnel-to-an-azure-virtual-wan-vpn-gateway"></a>¿Qué ocurre si el dispositivo VPN local solo tiene un túnel a una puerta de enlace de VPN de Azure Virtual WAN?

Una conexión de Azure Virtual WAN se compone de dos túneles. Una puerta de enlace de VPN de Virtual WAN se implementa en un centro de conectividad virtual en modo activo-activo, lo que significa que hay distintos túneles desde dispositivos locales que terminan en distintas instancias. Esta es la recomendación para todos los usuarios. Sin embargo, si, por algún motivo, el usuario decide tener solo un túnel a una de las instancias de puerta de enlace de VPN de Virtual WAN (mantenimiento, revisiones, etc.), la instancia de puerta de enlace se desconectará, el túnel se moverá a la instancia activa secundaria y el usuario podría experimentar una reconexión. Las sesiones de BGP no se moverán entre las instancias.

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>¿Se puede conectar el dispositivo VPN local a varios centros de conectividad?

Sí. Al principio, el flujo de tráfico sería del dispositivo local al perímetro de red de Microsoft más cercano y, luego, al centro de conectividad virtual.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>¿Hay disponibles nuevos recursos de Resource Manager para Virtual WAN?
  
Sí, Virtual WAN presenta nuevos recursos de Resource Manager. Para más información, consulte la [Información general](virtual-wan-about.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>¿Puedo implementar y usar mi dispositivo virtual de red favorito (en una red virtual de NVA) con Azure Virtual WAN?

Sí, puede conectar su aplicación virtual de red (NVA) favorita a Azure Virtual WAN.

### <a name="can-i-create-a-network-virtual-appliance-inside-the-virtual-hub"></a>¿Puedo crear una aplicación virtual de red dentro del centro de conectividad virtual?

Una aplicación virtual de red (NVA) no se puede implementar en un centro de conectividad virtual. Sin embargo, puede crearlo en una red virtual radial que esté conectada al centro de conectividad virtual y habilitar el enrutamiento adecuado para dirigir el tráfico según sus necesidades.

### <a name="can-a-spoke-vnet-have-a-virtual-network-gateway"></a>¿Puede una red virtual de radio tener una puerta de enlace de red virtual?

No. La red virtual de radio no puede tener una puerta de enlace de red virtual si está conectada al centro de conectividad virtual.

### <a name="is-there-support-for-bgp-in-vpn-connectivity"></a>¿Existe compatibilidad con BGP en la conectividad VPN?

Sí, BGP se admite. Al crear una VPN de sitio, puede proporcionar en ella los parámetros de BGP. Como resultado, las conexiones creadas en Azure para ese sitio estarán habilitadas para BGP.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>¿Hay información de precios o licencias para Virtual WAN?

Sí. Consulte la página [Precios](https://azure.microsoft.com/pricing/details/virtual-wan/).

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>¿Es posible construir Azure Virtual WAN con una plantilla de Resource Manager?

Se puede crear una configuración simple de una instancia de Virtual WAN con un centro y un sitio VPN mediante una [plantilla de inicio rápido](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network). Virtual WAN es principalmente un servicio REST o basado en el portal.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other-v2v-transit"></a>¿Pueden las redes virtuales radiales conectadas a un centro virtual comunicarse entre sí (tránsito V2V)?

Sí. Virtual WAN estándar admite la conectividad transitiva entre redes virtuales mediante el centro de conectividad de Virtual WAN al que están conectadas las redes virtuales. En la terminología de Virtual WAN, llamamos a estas rutas de acceso "tránsito local de red virtual de Virtual WAN" en el caso de las redes virtuales conectadas a un centro de conectividad de Virtual WAN dentro de una única región, y "tránsito global de red virtual de Virtual WAN" en el caso de las redes virtuales conectadas mediante varios centros de conectividad de Virtual WAN en dos o más zonas.

En algunos escenarios, además del tránsito local o global de red virtual de Virtual WAN, las redes virtuales de radio también se pueden emparejar directamente entre sí mediante el [emparejamiento de red virtual](../virtual-network/virtual-network-peering-overview.md). En este caso, el emparejamiento de red virtual tiene prioridad sobre la conexión transitiva del centro de conectividad de Virtual WAN.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>¿Se permite la conectividad de rama a rama en Virtual WAN?

Sí, este tipo de conectividad está disponible en Virtual WAN. La rama se aplica conceptualmente a sitios VPN, circuitos ExpressRoute o usuarios de VPN de punto a sitio o de usuario. La conectividad de rama a rama está habilitada de forma predeterminada y se puede encontrar en la opción **Configuración** de WAN. De esta forma, los usuarios o ramas VPN se pueden conectar a otras ramas VPN y la conectividad de tránsito también se puede habilitar entre los usuarios de VPN y ExpressRoute.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>¿Atraviesa el tráfico de rama a rama Azure Virtual WAN?

Sí. El tráfico de rama a rama atraviesa Azure Virtual WAN.

### <a name="does-virtual-wan-require-expressroute-from-each-site"></a>¿La instancia de Virtual WAN requiere ExpressRoute desde cada sitio?

No. Virtual WAN no requiere ExpressRoute desde cada sitio. Los sitios pueden estar conectados a una red de proveedor mediante un circuito ExpressRoute. En el caso de los sitios conectados mediante ExpressRoute a un centro de conectividad virtual, así como a la VPN de IPsec en el mismo centro de conectividad, el centro de conectividad virtual proporciona conectividad de tránsito entre el usuario de VPN y el de ExpressRoute.

### <a name="is-there-a-network-throughput-or-connection-limit-when-using-azure-virtual-wan"></a>¿Hay un límite en el rendimiento o la conexión de la red al utilizar Azure Virtual WAN?

El rendimiento de red es por servicio en un centro de conectividad de Virtual WAN. Aunque puede tener tantas instancias de Virtual WAN como desee, cada una permite un centro de conectividad por región. En cada centro de conectividad, el rendimiento agregado de VPN es de hasta 20 Gbps, el rendimiento agregado de ExpressRoute es hasta 20 Gbps y el rendimiento agregado de VPN de punto a sitio o VPN de usuario es de hasta 20 Gbps. El enrutador del centro de conectividad virtual admite hasta 50 Gbps con los flujos de tráfico de red virtual a red virtual y supone una carga de trabajo total de 2000 máquinas virtuales en todas las redes virtuales conectadas a un solo centro de conectividad virtual. Este [límite](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-wan-limits) se puede aumentar abriendo una solicitud de soporte técnico de cliente en línea. Para conocer las consecuencias para los costos, consulte los costos de *Unidad de infraestructura de enrutamiento* en la página [Precios de Azure Virtual WAN](https://azure.microsoft.com/pricing/details/virtual-wan/). 

Cuando los sitios VPN se conectan a un centro de conectividad, lo hacen con conexiones. Virtual WAN admite hasta 1000 conexiones o 2000 túneles IPsec por centro de conectividad virtual. Cuando los usuarios remotos se conectan a un centro de conectividad virtual, se conectan a la instancia de VPN Gateway P2S, que admite hasta 10 000 usuarios en función de la unidad de escalado (ancho de banda) elegida para la instancia de VPN Gateway P2S en el centro de conectividad virtual.

### <a name="what-is-the-total-vpn-throughput-of-a-vpn-tunnel-and-a-connection"></a>¿Qué es el rendimiento total de VPN de un túnel y una conexión VPN?

El rendimiento total de VPN de un centro de conectividad es de hasta 20 Gbps en función de la unidad de escalado elegida de VPN Gateway. El rendimiento se comparte entre todas las conexiones existentes. Cada túnel de una conexión puede admitir hasta 1 Gbps.

### <a name="can-i-use-nat-t-on-my-vpn-connections"></a>¿Puedo usar NAT-T en las conexiones VPN?

Sí, se admite NAT traversal (NAT-T). VPN Gateway de Virtual WAN NO llevará a cabo ninguna funcionalidad similar a la de NAT en los paquetes internos hacia y desde los túneles de IPsec. En esta configuración, asegúrese de que el dispositivo local inicie el túnel IPsec.

### <a name="i-dont-see-the-20-gbps-setting-for-the-virtual-hub-in-portal-how-do-i-configure-that"></a>No veo la configuración de 20 Gbps para el centro de conectividad virtual en el portal. ¿Cómo puedo configurarlo?

Vaya a la instancia de VPN Gateway de un centro de conectividad del portal y haga clic en la unidad de escalado para cambiarla por la configuración adecuada.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>¿Azure Virtual WAN permite al dispositivo local usar varias ISP en paralelo o es siempre un solo túnel VPN?

Las soluciones en dispositivos locales pueden aplicar directivas de tráfico para dirigir el tráfico entre varios túneles en el centro de conectividad de Azure Virtual WAN (VPN Gateway en el centro de conectividad virtual).

### <a name="what-is-global-transit-architecture"></a>¿Qué es la arquitectura de tránsito global?

Para más información acerca de la arquitectura de tránsito global, consulte [Arquitectura de red de tránsito global y Virtual WAN](virtual-wan-global-transit-network-architecture.md).

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>¿Cómo se enruta el tráfico en la red troncal de Azure?

El tráfico sigue el patrón: dispositivo de rama -> ISP -> perímetro de red de Microsoft -> controlador de dominio de Microsoft (red virtual de centro) -> perímetro de red de Microsoft -> ISP -> dispositivo de rama.

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>En este modelo, ¿qué es necesario en cada sitio? ¿Solo una conexión a Internet?

Sí. Una conexión a Internet y un dispositivo físico que admita IPsec, preferiblemente de nuestros [asociados de Virtual WAN](virtual-wan-locations-partners.md) integrados. De forma opcional, puede administrar manualmente la configuración y la conectividad a Azure desde su dispositivo preferido.

### <a name="how-do-i-enable-default-route-00000-for-a-connection-vpn-expressroute-or-virtual-network"></a>¿Cómo habilito la ruta predeterminada (0.0.0.0/0) de una conexión (VPN, ExpressRoute o Virtual Network)?

Un centro de conectividad virtual puede propagar una ruta predeterminada aprendida en una red virtual, una VPN de sitio a sitio y una conexión ExpressRoute si la marca está "habilitada" en la conexión. Esta marca está visible cuando el usuario edita una conexión de red virtual, una conexión VPN o una conexión ExpressRoute. De forma predeterminada, esta marca está deshabilitada cuando un sitio o circuito de ExpressRoute se conecta a un centro. Está habilitada de forma predeterminada cuando se agrega una conexión de red virtual para conectar una red virtual a un centro de conectividad virtual.

La ruta predeterminada no se origina en el centro de conectividad de Virtual WAN; la ruta se propaga si ya la ha aprendido el centro de Virtual WAN como resultado de la implementación de un firewall en el centro o en caso de que otro sitio conectado tenga habilitada la tunelización forzada. Las rutas predeterminadas no se propaga entre los centros (entre centros).

### <a name="is-it-possible-to-create-multiple-virtual-wan-hubs-in-the-same-region"></a>¿Es posible crear varios centros de conectividad de Virtual WAN en la misma región?
Sí. Los clientes ahora pueden crear más de un centro de conectividad en la misma región para la misma instancia de Azure Virtual WAN. 


### <a name="how-does-the-virtual-hub-in-a-virtual-wan-select-the-best-path-for-a-route-from-multiple-hubs"></a>¿Cómo selecciona el centro de conectividad virtual de una instancia de Virtual WAN la mejor ruta desde varios centros?

Si un centro de conectividad virtual aprende la misma ruta de varios centros de conectividad remotos, el orden en el que decide es el siguiente:

1. Coincidencia de prefijo más larga.
1. Rutas locales en vez de rutas entre centros de conectividad.
1. Rutas estáticas en vez de BGP: en el contexto de la decisión tomada por el enrutador del centro de conectividad virtual. Sin embargo, si la decisión la toma VPN Gateway cuando un sitio anuncia rutas a través de BGP o proporciona prefijos de dirección estática, es posible que se prefieran rutas estáticas a rutas BGP.
1. ExpressRoute (ER) en lugar de VPN: se prefiere ER frente a VPN cuando el contexto es un centro de conectividad local. La conectividad de tránsito entre circuitos ExpressRoute solo está disponible a través de Global Reach. Por lo tanto, en escenarios en los que el circuito ExpressRoute está conectado a un centro de conectividad y hay otro circuito ExpressRoute conectado a un centro de conectividad diferente con una conexión VPN, es posible que se prefiera la VPN para escenarios entre centros de conectividad.
1. Longitud de la ruta de acceso de AS (los centros virtuales anteponen rutas con la ruta de acceso de AS 65520-65520 cuando se anuncian rutas entre sí).

### <a name="does-the-virtual-wan-hub-allow-connectivity-between-expressroute-circuits"></a>¿El centro de conectividad de Virtual WAN permite la conectividad entre circuitos ExpressRoute?

El tránsito entre ER y ER siempre se realiza a través de Global Reach. Las puertas de enlace de centro de conectividad virtual se implementan en las regiones de Azure o del controlador de dominio. Cuando dos circuitos ExpressRoute se conectan a través de Global Reach, no es necesario que el tráfico llegue desde los enrutadores perimetrales hasta el controlador de dominio del centro de conectividad.

### <a name="is-there-a-concept-of-weight-in-azure-virtual-wan-expressroute-circuits-or-vpn-connections"></a>¿Existe un concepto de peso en los circuitos ExpressRoute de Azure Virtual WAN o en las conexiones VPN?

Cuando varios circuitos ExpressRoute están conectados a un centro de conectividad virtual, el peso del enrutamiento en la conexión proporciona un mecanismo para que ExpressRoute en el centro de conectividad virtual prefiera un circuito antes que el otro. No hay ningún mecanismo para establecer un peso en una conexión VPN. Azure siempre prefiere una conexión ExpressRoute antes que una conexión VPN en un solo centro de conectividad.

### <a name="does-virtual-wan-prefer-expressroute-over-vpn-for-traffic-egressing-azure"></a>¿Virtual WAN prefiere ExpressRoute antes que una VPN para el tráfico que sale de Azure?

Sí. ¿Virtual WAN prefiere ExpressRoute antes que una VPN para el tráfico que sale de Azure?

### <a name="when-a-virtual-wan-hub-has-an-expressroute-circuit-and-a-vpn-site-connected-to-it-what-would-cause-a-vpn-connection-route-to-be-preferred-over-expressroute"></a>Cuando un centro de conectividad de Virtual WAN tiene un circuito ExpressRoute y un sitio VPN conectados a él, ¿qué haría que una ruta de conexión VPN se prefiriera por encima de ExpressRoute?

Cuando un circuito ExpressRoute se conecta a un centro de conectividad virtual, los enrutadores perimetrales de Microsoft son el primer nodo para la comunicación entre el entorno local y Azure. Estos enrutadores perimetrales se comunican con las puertas de enlace de ExpressRoute de Virtual WAN que, a su vez, aprenden las rutas del enrutador del centro de conectividad virtual que controla todas las rutas entre todas las puertas de enlace de Virtual WAN. Los enrutadores perimetrales de Microsoft prefieren procesar mejor las rutas de ExpressRoute del centro de conectividad virtual antes que las rutas aprendidas del entorno local.

Por algún motivo, si la conexión VPN se convierte en el medio principal por el que el centro de conectividad virtual aprende las rutas (por ejemplo, escenarios de conmutación por error entre ExpressRoute y VPN), a menos que el sitio de VPN tenga una longitud de ruta AS mayor, el centro de conectividad virtual seguirá compartiendo las rutas de VPN aprendidas con la puerta de enlace de ExpressRoute. Esto hace que los enrutadores perimetrales de Microsoft prefieran las rutas VPN antes que las rutas locales.

### <a name="when-two-hubs-hub-1-and-2-are-connected-and-there-is-an-expressroute-circuit-connected-as-a-bow-tie-to-both-the-hubs-what-is-the-path-for-a-vnet-connected-to-hub-1-to-reach-a-vnet-connected-in-hub-2"></a>Cuando dos centros de conectividad (1 y 2) están conectados y hay un circuito ExpressRoute conectado como un lazo para ambos centros de conectividad, ¿cuál es la ruta de acceso de una red virtual conectada al centro de conectividad 1 para llegar a una red virtual conectada en el centro de conectividad 2?

El comportamiento actual es preferir la ruta de acceso del circuito ExpressRoute frente a una conexión entre centros para la conectividad de red virtual a red virtual. Sin embargo, esto no se recomienda en una configuración de Virtual WAN. Para resolver esto, puede realizar una de estas dos acciones:

 * Configure varios circuitos ExpressRoute (proveedores distintos) para que se conecten a un centro de conectividad y usen la conectividad entre centros de conectividad que proporciona Virtual WAN para los flujos de tráfico entre regiones.

 * Póngase en contacto con el equipo del producto para participar en la versión preliminar pública. En esta versión preliminar, el tráfico entre los dos centros atraviesa el enrutador de Azure Virtual WAN en cada centro y usa una ruta de acceso de centro a centro en lugar de la ruta de acceso de ExpressRoute (que atraviesa los enrutadores perimetrales de Microsoft/MSEE). Para usar esta característica durante la versión preliminar, envíe un correo electrónico **previewpreferh2h@microsoft.com** con los Virtual WAN, el identificador de suscripción y la región de Azure. Espere una respuesta en un plazo de 48 horas laborables (de lunes a viernes) con la confirmación de que la característica está habilitada.

### <a name="can-hubs-be-created-in-different-resource-group-in-virtual-wan"></a>¿Se pueden crear concentradores en un grupo de recursos diferente en Virtual WAN?

Sí. Actualmente, esta opción solo está disponible a través de PowerShell. En el portal de Virtual WAN es necesario que los centros de conectividad estén en el mismo grupo de recursos que el propio recurso de Virtual WAN.

### <a name="what-is-the-recommended-hub-address-space-during-hub-creation"></a>¿Cuál es el espacio de direcciones de centro de conectividad recomendado durante la creación del centro?

El valor recomendado del espacio de direcciones de un centro de conectividad de Virtual WAN es /23. El centro de conectividad de Virtual WAN asigna subredes a varias puertas de enlace, como ExpressRoute, VPN de sitio a sitio, VPN de punto a sitio, Azure Firewall o un enrutador del centro de conectividad virtual. En los escenarios en los que se implementan aplicaciones virtuales de red (NVA) dentro de un centro de conectividad virtual, normalmente se crea una subred /28 para las instancias de NVA. Pero si el usuario tuviera que aprovisionar varias NVA, se podría asignar una subred /27. Por tanto, de cara a una arquitectura futura, mientras los centro de conectividad de Virtual WAN Hubs se implementan con un tamaño mínimo de /24, el espacio de direcciones de centro de conectividad recomendado en el momento de creación para que el usuario lo introduzca es /23.

### <a name="is-there-support-for-ipv6-in-virtual-wan"></a>¿Hay compatibilidad con IPv6 en un Virtual WAN?

IPv6 no se admite en el centro de conectividad de Virtual WAN y sus puertas de enlace. Si tiene una red virtual que tiene compatibilidad con IPv4 e IPv6 y desea conectar la red virtual a Virtual WAN, este escenario no se admite actualmente.

En el escenario de VPN de usuario de punto a sitio con salida de Internet a través de Azure Firewall, probablemente tendrá que desactivar la conectividad IPv6 en el dispositivo cliente para forzar el tráfico al centro de conectividad de Virtual WAN. Esto se debe a que, de forma predeterminada, los dispositivos modernos usan direcciones IPv6.

### <a name="what-is-the-recommended-api-version-to-be-used-by-scripts-automating-various-virtual-wan-functionalities"></a>¿Cuál es la versión de API recomendada para utilizar en los scripts que automatizan varias funcionalidades de Virtual WAN?

Se requiere una versión mínima de 05-01-2020 (1 de mayo de 2020).

### <a name="are-there-any-virtual-wan-limits"></a>¿Hay algún límite de Virtual WAN?

Consulte la sección [Límites de Virtual WAN](../azure-resource-manager/management/azure-subscription-service-limits.md#virtual-wan-limits) en la página de límites de suscripciones y servicios.

### <a name="what-are-the-differences-between-the-virtual-wan-types-basic-and-standard"></a>¿Cuáles son las diferencias entre los tipos de instancias de Virtual WAN (básico y estándar)?

Consulte [Redes WAN virtuales de tipo Básico y Estándar](virtual-wan-about.md#basicstandard). Para ver los precios, consulte la página de [precios](https://azure.microsoft.com/pricing/details/virtual-wan/).

### <a name="does-virtual-wan-store-customer-data"></a>¿Virtual WAN almacena datos de clientes?

No. Virtual WAN no almacena datos de clientes.

### <a name="are-there-any-managed-service-providers-that-can-manage-virtual-wan-for-users-as-a-service"></a>¿Hay proveedores de servicios administrados que pueden administrar Virtual WAN para usuarios como servicio?

Sí. Para obtener una lista de soluciones de proveedores de servicios administrados (MSP) habilitadas a través de Azure Marketplace, consulte el apartado [Ofertas de Azure Marketplace de proveedores de servicios administrados de redes de Azure asociados](../networking/networking-partners-msp.md#msp).

### <a name="how-does-virtual-wan-hub-routing-differ-from-azure-route-server-in-a-vnet"></a>¿En qué se diferencia el enrutamiento del centro de conectividad de Virtual WAN de Azure Route Server en una red virtual?

Azure Route Server proporciona un servicio de emparejamiento de Protocolo de puerta de enlace de borde (BGP) que las aplicaciones virtuales de red pueden usar para aprender las rutas desde el servidor de enrutamiento en una red virtual del centro de conectividad de implementación personal. El enrutamiento de WAN virtual ofrece varias funcionalidades, entre las que se incluyen el enrutamiento de tránsito entre redes virtuales, el enrutamiento personalizado, la asociación y propagación de rutas personalizadas y un servicio de centro de conectividad con malla completa y sin interacción, junto con los servicios de conectividad de ExpressRoute, VPN de sitio, VPN de punto a sitio a gran escala y de usuarios remotos, así como funcionalidades de centro seguro (Azure Firewall). Al establecer un emparejamiento BGP entre la aplicación virtual de red y Azure Route Server, puede anunciar las direcciones IP de su aplicación virtual a la red virtual. En todas las funcionalidades de enrutamiento avanzadas, como el enrutamiento de tránsito, el enrutamiento personalizado, etc., puede usar el enrutamiento de Virtual WAN.

### <a name="if-i-am-using-a-third-party-security-provider-zscaler-iboss-or-checkpoint-to-secure-my-internet-traffic-why-dont-i-see-the-vpn-site-associated-to-the-third-party-security-provider-in-the-azure-portal"></a>Si uso un proveedor de seguridad de terceros (ZScalar, iBoss o Checkpoint) para proteger el tráfico de Internet, ¿por qué no veo el sitio de VPN asociado a él en Azure Portal?

Cuando decide implementar un proveedor de seguridad asociado para proteger el acceso a Internet para sus usuarios, el proveedor de seguridad de terceros crea un sitio VPN en su nombre. Este sitio de VPN no aparece en Azure Portal porque el proveedor crea automáticamente el proveedor de seguridad de terceros y no es un sitio VPN creado por el usuario.

Para obtener más información sobre las opciones disponibles de proveedores de seguridad de terceros y cómo configurarlas, consulte [Implementación de un proveedor de seguridad asociado](../firewall-manager/deploy-trusted-security-partner.md).

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre Virtual WAN, consulte el artículo [acerca de Virtual WAN](virtual-wan-about.md).

