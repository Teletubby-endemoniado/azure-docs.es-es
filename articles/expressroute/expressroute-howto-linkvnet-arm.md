---
title: 'Tutorial: Vinculación de redes virtuales a circuitos ExpressRoute - Azure PowerShell'
description: En este tutorial se proporciona información general sobre cómo vincular redes virtuales a circuitos ExpressRoute mediante el modelo de implementación de Resource Manager y Azure PowerShell.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: tutorial
ms.date: 08/10/2021
ms.author: duau
ms.custom: seodec18, devx-track-azurepowershell
ms.openlocfilehash: 366426ee04bd13239a734bbc721cbd6822a34ddd
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128650028"
---
# <a name="tutorial-connect-a-virtual-network-to-an-expressroute-circuit"></a>Tutorial: Conexión de una red virtual a un circuito ExpressRoute
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [CLI de Azure](howto-linkvnet-cli.md)
> * [Vídeo: Azure Portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (clásico)](expressroute-howto-linkvnet-classic.md)
>

Este artículo lo ayudará a vincular redes virtuales a circuitos ExpressRoute de Azure a través del modelo de implementación de Resource Manager y PowerShell. Las redes virtuales pueden estar en la misma suscripción o formar parte de otra suscripción. En este artículo también se muestra cómo actualizar el vínculo de una red virtual.

* Es posible vincular hasta 10 redes virtuales a un circuito ExpressRoute estándar. Todas las redes virtuales deben pertenecer a la misma región geopolítica al utilizar un circuito de ExpressRoute estándar. 

* Una única red virtual se puede vincular a 16 circuitos ExpressRoute como máximo. Use los pasos de este artículo para crear un nuevo objeto de conexión para cada circuito ExpressRoute al que quiere conectarse. Los circuitos ExpressRoute pueden estar en la misma suscripción, en suscripciones diferentes o en una combinación de ambas.

* Si habilita el complemento prémium de ExpressRoute, puede vincular redes virtuales fuera de la región geopolítica del circuito ExpressRoute. El complemento Premium también le permitirá conectar más de 10 redes virtuales al circuito ExpressRoute en función del ancho de banda elegido. Consulte las [preguntas más frecuentes](expressroute-faqs.md) para obtener más detalles sobre el complemento premium.

* Para crear la conexión desde el circuito de ExpressRoute a la puerta de enlace de red virtual de ExpressRoute de destino, el número de espacios de direcciones anunciados desde las redes virtuales locales o emparejadas debe ser igual o menor que **200**. Una vez creada correctamente la conexión, puede agregar espacios de direcciones adicionales, hasta 1000, a las redes virtuales locales o emparejadas.

En este tutorial, obtendrá información sobre cómo:
> [!div class="checklist"]
> - Conexión de una red virtual en la misma suscripción a un circuito
> - Conexión de una red virtual en una suscripción diferente a un circuito
> - Modificación de una conexión de red virtual
> - Configuración de FastPath de ExpressRoute

## <a name="prerequisites"></a>Requisitos previos

* Revise los [requisitos previos](expressroute-prerequisites.md), los [requisitos de enrutamiento](expressroute-routing.md) y los [flujos de trabajo](expressroute-workflows.md) antes de comenzar la configuración.

* Tiene que tener un circuito ExpressRoute activo. 
  * Siga las instrucciones para [crear un circuito ExpressRoute](expressroute-howto-circuit-arm.md) y habilite el circuito mediante el proveedor de conectividad. 
  * Asegúrese de que dispone de un emparejamiento privado de Azure configurado para el circuito. Consulte el artículo de [configuración del enrutamiento](expressroute-howto-routing-arm.md) para obtener instrucciones sobre el enrutamiento. 
  * Asegúrese de que el emparejamiento privado de Azure se configure y establezca el emparejamiento BGP entre la red y Microsoft para la conectividad de un extremo a otro.
  * Asegúrese de que ha creado y aprovisionado totalmente una red virtual y una puerta de enlace de red virtual. Siga las instrucciones para [crear una puerta de enlace de red virtual en ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Una puerta de enlace de red virtual para ExpressRoute usa el GatewayType "ExpressRoute", no VPN.

### <a name="working-with-azure-powershell"></a>Trabajo con Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Conexión de una red virtual en la misma suscripción a un circuito
Puede conectar una puerta de enlace de red virtual a un circuito ExpressRoute mediante el siguiente cmdlet. Asegúrese de que la puerta de enlace de red virtual se crea y está lista para vincular antes de ejecutar el cmdlet:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Conexión de una red virtual en una suscripción diferente a un circuito
Puede compartir un circuito ExpressRoute entre varias suscripciones. En la ilustración siguiente se muestra un sencillo esquema de cómo funciona el uso compartido de circuitos ExpressRoute entre varias suscripciones.

> [!NOTE]
> No se admite la conexión de redes virtuales entre nubes soberanas de Azure y la nube pública de Azure. Solo puede vincular redes virtuales de distintas suscripciones de la misma nube.

Cada una de las nubes más pequeñas dentro de la nube de gran tamaño se usa para representar las suscripciones que pertenecen a diferentes departamentos dentro de una organización. Cada departamento de la organización usa su propia suscripción para implementar sus servicios, pero puede compartir un único circuito ExpressRoute para volver a conectarse a la red local. Un solo departamento (en este ejemplo: TI) puede ser el propietario de ExpressRoute. Otras suscripciones dentro de la organización pueden usar el circuito ExpressRoute.

> [!NOTE]
> Los cargos de conectividad y ancho de banda de un circuito ExpressRoute recaerán en el propietario de la suscripción. Todas las redes virtuales comparten el mismo ancho de banda.
> 

:::image type="content" source="./media/expressroute-howto-linkvnet-classic/cross-subscription.png" alt-text="Conectividad entre suscripciones":::

### <a name="administration---circuit-owners-and-circuit-users"></a>Administración: los propietarios de circuito y los usuarios de circuito

El "propietario del circuito" es un usuario avanzado autorizado del recurso de circuito ExpressRoute. Puede crear autorizaciones que los "propietarios del circuito", a su vez, pueden canjear. Los usuarios del circuito son propietarios de puertas de enlace de red virtual que no están en la misma suscripción que el circuito ExpressRoute. Los usuarios del circuito pueden canjear autorizaciones (una autorización por red virtual).

El propietario del circuito tiene la capacidad de modificar y revocar las autorizaciones en cualquier momento. La revocación de una autorización dará como resultado la eliminación de todas las conexiones de vínculos de la suscripción cuyo acceso se haya revocado.

### <a name="circuit-owner-operations"></a>Operaciones del propietario del circuito

**Creación de una autorización**

El propietario del circuito crea una autorización, que crea una clave de autorización que puede usar un usuario del circuito para conectar sus puertas de enlace de red virtual al circuito ExpressRoute. Una autorización solo es válida para una conexión.

El fragmento de código del cmdlet siguiente muestra cómo crear una autorización:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```

La respuesta a los comandos anteriores contendrá la clave y el estado de la autorización:

```azurepowershell
Name                   : MyAuthorization1
Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
AuthorizationKey       : ####################################
AuthorizationUseStatus : Available
ProvisioningState      : Succeeded
```

**Revisión de autorizaciones**

El propietario del circuito puede revisar todas las autorizaciones emitidas en un circuito concreto ejecutando el siguiente cmdlet:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Adición de autorizaciones**

El propietario del circuito puede agregar las autorizaciones mediante el siguiente cmdlet:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Eliminación de autorizaciones**

El propietario del circuito puede revocar o eliminar las autorizaciones al usuario ejecutando el siguiente cmdlet:

```azurepowershell-interactive
Remove-AzExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a>Operaciones del usuario del circuito

El usuario del circuito necesita el Id. de mismo nivel y una clave de autorización del propietario del circuito. la clave de autorización es un GUID.

El id. de mismo nivel se puede comprobar con el siguiente comando:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Canje de una autorización de conexión**

El usuario del circuito puede ejecutar el siguiente cmdlet para canjear una autorización de vínculo:

```azurepowershell-interactive
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Liberación de una autorización de conexión**

Puede liberar una autorización eliminando la conexión que vincula el circuito ExpressRoute a la red virtual.

## <a name="modify-a-virtual-network-connection"></a>Modificación de una conexión de red virtual
Puede actualizar ciertas propiedades de una conexión de red virtual. 

**Actualización del peso de la conexión**

La red virtual puede estar conectada a varios circuitos ExpressRoute. Es posible que reciba el mismo prefijo de más de un circuito ExpressRoute. Para elegir la conexión por la que enviar el tráfico destinado a este prefijo, puede cambiar el valor de *RoutingWeight* de una conexión. El tráfico se enviará por la conexión con el valor más alto de *RoutingWeight*.

```azurepowershell-interactive
$connection = Get-AzVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

El intervalo de *RoutingWeight* abarca de 0 a 32 000. El valor predeterminado es 0.

## <a name="configure-expressroute-fastpath"></a>Configuración de FastPath de ExpressRoute 
Puede habilitar [FastPath de ExpressRoute](expressroute-about-virtual-network-gateways.md) si la puerta de enlace de red virtual es Ultra Performance o ErGw3AZ. FastPath mejora el rendimiento de la ruta de acceso a los datos, como con paquetes y conexiones por segundo entre la red local y la red virtual. 

**Configuración de FastPath en una conexión nueva**

```azurepowershell-interactive 
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG" 
$gw = Get-AzVirtualNetworkGateway -Name "MyGateway" -ResourceGroupName "MyRG" 
$connection = New-AzVirtualNetworkGatewayConnection -Name "MyConnection" -ResourceGroupName "MyRG" -ExpressRouteGatewayBypass -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute -Location "MyLocation" 
``` 

**Actualización de una conexión existente para habilitar FastPath**

```azurepowershell-interactive 
$connection = Get-AzVirtualNetworkGatewayConnection -Name "MyConnection" -ResourceGroupName "MyRG" 
$connection.ExpressRouteGatewayBypass = $True
Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
``` 

> [!NOTE]
> Puede usar [Connection Monitor](how-to-configure-connection-monitor.md) para comprobar que el tráfico llega al destino mediante FastPath.
>

## <a name="enroll-in-expressroute-fastpath-features-preview"></a>Inscripción en las características de FastPath de ExpressRoute (versión preliminar)

La compatibilidad de FastPath con el emparejamiento de redes virtuales está ahora en versión preliminar pública.

### <a name="fastpath-and-virtual-network-peering"></a>FastPath y emparejamiento de red virtual

Con FastPath y el emparejamiento de red virtual, puede habilitar la conectividad de ExpressRoute directamente a las máquinas virtuales de una red virtual local o emparejada, omitiendo la puerta de enlace de red virtual de ExpressRoute en la ruta de acceso de datos.

Para inscribirse en esta versión preliminar, ejecute el comando de Azure PowerShell siguiente en la suscripción de Azure de destino:

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName ExpressRouteVnetPeeringGatewayBypass -ProviderNamespace Microsoft.Network
```

> [!NOTE] 
> Todas las conexiones configuradas para FastPath en la suscripción de destino se inscribirán en esta versión preliminar. No se recomienda habilitar esta versión preliminar en las suscripciones de producción.
> Si ya tiene FastPath configurado y desea inscribirse en la característica en versión preliminar, debe hacer lo siguiente:
> 1. Inscríbase en la característica en versión preliminar de FastPath con el comando de Azure PowerShell anterior.
> 1. Deshabilite y vuelva a habilitar FastPath en la conexión de destino.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita la conexión a ExpressRoute, desde la suscripción en que se encuentra la puerta de enlace, use el comando `Remove-AzVirtualNetworkGatewayConnection` para quitar el vínculo entre la pasarela y el circuito.

```azurepowershell-interactive
Remove-AzVirtualNetworkGatewayConnection "MyConnection" -ResourceGroupName "MyRG"
```

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información acerca de ExpressRoute, consulte P+F de ExpressRoute.

> [!div class="nextstepaction"]
> [P+F de ExpressRoute](expressroute-faqs.md)
