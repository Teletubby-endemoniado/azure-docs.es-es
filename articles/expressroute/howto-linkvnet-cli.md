---
title: 'Tutorial: Vinculación de redes virtuales a circuitos ExpressRoute - CLI de Azure'
description: En este tutorial, se muestra cómo vincular redes virtuales a circuitos ExpressRoute mediante el modelo de implementación de Resource Manager y la CLI de Azure.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: tutorial
ms.date: 08/10/2021
ms.author: duau
ms.openlocfilehash: b7a6cf1bdb490683faa5285811c83d220ac80cca
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128662889"
---
# <a name="tutorial-connect-a-virtual-network-to-an-expressroute-circuit-using-cli"></a>Tutorial: Conexión de una red virtual a un circuito ExpressRoute mediante la CLI

En este tutorial se muestra cómo vincular redes virtuales (VNet) a circuitos de Azure ExpressRoute mediante la CLI de Azure. Para realizar la vinculación mediante la CLI de Azure, las redes virtuales deben crearse mediante el modelo de implementación de Resource Manager. Pueden estar en la misma suscripción o formar parte de otra suscripción. Si quiere usar un método diferente para conectar su red virtual con un circuito ExpressRoute, seleccione un artículo de la siguiente lista:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [CLI de Azure](howto-linkvnet-cli.md)
> * [Vídeo: Azure Portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (clásico)](expressroute-howto-linkvnet-classic.md)
> 

En este tutorial, obtendrá información sobre cómo:
> [!div class="checklist"]
> - Conexión de una red virtual en la misma suscripción a un circuito
> - Conexión de una red virtual en una suscripción diferente a un circuito
> - Modificación de una conexión de red virtual
> - Configuración de FastPath de ExpressRoute

## <a name="prerequisites"></a>Requisitos previos

* Necesitará la versión más reciente de la interfaz de la línea de comandos (CLI). Para más información, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).
* Revise los [requisitos previos](expressroute-prerequisites.md), los [requisitos de enrutamiento](expressroute-routing.md) y los [flujos de trabajo](expressroute-workflows.md) antes de comenzar la configuración.
* Tiene que tener un circuito ExpressRoute activo. 
  * Siga las instrucciones para [crear un circuito ExpressRoute](howto-circuit-cli.md) y habilite el circuito mediante el proveedor de conectividad. 
  * Asegúrese de que dispone de un emparejamiento privado de Azure configurado para el circuito. Consulte el artículo de [configuración del enrutamiento](howto-routing-cli.md) para obtener instrucciones sobre el enrutamiento. 
  * Asegúrese de que se haya configurado un emparejamiento privado de Azure. El emparejamiento BGP entre la red y Microsoft debe establecerse para habilitar la conectividad de un extremo a otro.
  * Asegúrese de que ha creado y aprovisionado totalmente una red virtual y una puerta de enlace de red virtual. Siga las instrucciones para realizar la [Configuración de una puerta de enlace de red virtual en ExpressRoute](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md). Asegúrese de usar `--gateway-type ExpressRoute`.
* Es posible vincular hasta 10 redes virtuales a un circuito ExpressRoute estándar. Todas las redes virtuales deben pertenecer a la misma región geopolítica al utilizar un circuito de ExpressRoute estándar. 
* Una única red virtual se puede vincular a 16 circuitos ExpressRoute como máximo. Use el procedimiento siguiente para crear un nuevo objeto de conexión para cada circuito ExpressRoute al que quiere conectarse. Los circuitos ExpressRoute pueden estar en la misma suscripción, en suscripciones diferentes o en una combinación de ambas.
* Si habilita el complemento prémium de ExpressRoute, puede vincular redes virtuales fuera de la región geopolítica del circuito ExpressRoute. El complemento Premium también le permitirá conectar más de 10 redes virtuales al circuito ExpressRoute en función del ancho de banda elegido. Consulte las [preguntas más frecuentes](expressroute-faqs.md) para obtener más detalles sobre el complemento premium.

* Para crear la conexión desde el circuito de ExpressRoute a la puerta de enlace de red virtual de ExpressRoute de destino, el número de espacios de direcciones anunciados desde las redes virtuales locales o emparejadas debe ser igual o menor que **200**. Una vez creada correctamente la conexión, puede agregar hasta un máximo de 1000 espacios de direcciones adicionales a las redes virtuales locales o emparejadas.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Conexión de una red virtual en la misma suscripción a un circuito

Puede conectar una puerta de enlace de red virtual a un circuito ExpressRoute mediante el ejemplo. Asegúrese de que la puerta de enlace de red virtual se crea y está lista para vincular antes de ejecutar el comando.

```azurecli-interactive
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Conexión de una red virtual en una suscripción diferente a un circuito

Puede compartir un circuito ExpressRoute entre varias suscripciones. En la ilustración siguiente se muestra un sencillo esquema de cómo funciona el uso compartido de circuitos ExpressRoute entre varias suscripciones.

> [!NOTE]
> No se admite la conexión de redes virtuales entre nubes soberanas de Azure y la nube pública de Azure. Solo puede vincular redes virtuales de distintas suscripciones de la misma nube.

Cada una de las nubes más pequeñas dentro de la nube de gran tamaño se usa para representar las suscripciones que pertenecen a diferentes departamentos dentro de una organización. Cada departamento de la organización usa su propia suscripción para implementar sus servicios, pero puede compartir un único circuito ExpressRoute para volver a conectarse a la red local. Un solo departamento (en este ejemplo: TI) puede ser el propietario de ExpressRoute. Otras suscripciones dentro de la organización pueden usar el circuito ExpressRoute.

> [!NOTE]
> Los cargos de conectividad y ancho de banda de un circuito dedicado recaerán en el propietario del circuito ExpressRoute. Todas las redes virtuales comparten el mismo ancho de banda.
> 
> 

![Conectividad entre suscripciones](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Administración: los propietarios del circuito y los usuarios del circuito

El "propietario del circuito" es un usuario avanzado autorizado del recurso del circuito ExpressRoute. Puede crear autorizaciones que los "propietarios del circuito", a su vez, pueden canjear. Los usuarios del circuito son propietarios de puertas de enlace de red virtual (que no se incluyen en la misma suscripción que el circuito ExpressRoute). Los usuarios del circuito pueden canjear autorizaciones (una autorización por red virtual).

El propietario del circuito tiene la capacidad de modificar y revocar las autorizaciones en cualquier momento. Cuando se revoca una autorización, da como resultado la eliminación de todas las conexiones de vínculos de la suscripción cuyo acceso se haya revocado.

### <a name="circuit-owner-operations"></a>Operaciones del propietario del circuito

**Creación de una autorización**

El propietario del circuito crea una autorización, que crea una clave de autorización que puede usar un usuario del circuito para conectar sus puertas de enlace de red virtual al circuito ExpressRoute. Una autorización solo es válida para una conexión.

En el ejemplo siguiente, se muestra cómo crear una autorización:

```azurecli-interactive
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

La respuesta contendrá la clave y el estado de la autorización:

```output
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

**Revisión de autorizaciones**

El propietario del circuito puede revisar todas las autorizaciones emitidas en un circuito concreto ejecutando el siguiente ejemplo:

```azurecli-interactive
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**Adición de autorizaciones**

El propietario del circuito puede agregar las autorizaciones mediante el siguiente ejemplo:

```azurecli-interactive
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**Eliminación de autorizaciones**

El propietario del circuito puede revocar o eliminar las autorizaciones al usuario ejecutando el siguiente ejemplo:

```azurecli-interactive
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Operaciones del usuario del circuito

El usuario del circuito necesita el ID de mismo nivel y una clave de autorización del propietario del circuito. la clave de autorización es un GUID.

```azurecli-interactive
az network express-route show -n MyCircuit -g ExpressRouteResourceGroup
```

**Canje de una autorización de conexión**

El usuario del circuito puede ejecutar el siguiente ejemplo para canjear una autorización de vínculo:

```azurecli-interactive
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Liberación de una autorización de conexión**

Puede liberar una autorización eliminando la conexión que vincula el circuito ExpressRoute a la red virtual.

## <a name="modify-a-virtual-network-connection"></a>Modificación de una conexión de red virtual
Puede actualizar ciertas propiedades de una conexión de red virtual. 

**Actualización del peso de la conexión**

La red virtual puede estar conectada a varios circuitos ExpressRoute. Es posible que reciba el mismo prefijo de más de un circuito ExpressRoute. Para elegir la conexión por la que enviar el tráfico destinado a este prefijo, puede cambiar el valor de *RoutingWeight* de una conexión. El tráfico se enviará por la conexión con el valor más alto de *RoutingWeight*.

```azurecli-interactive
az network vpn-connection update --name ERConnection --resource-group ExpressRouteResourceGroup --routing-weight 100
```

El intervalo de *RoutingWeight* abarca de 0 a 32 000. El valor predeterminado es 0.

## <a name="configure-expressroute-fastpath"></a>Configuración de FastPath de ExpressRoute 
Puede habilitar [FastPath de ExpressRoute](expressroute-about-virtual-network-gateways.md) si la puerta de enlace de red virtual es Ultra Performance o ErGw3AZ. FastPath mejora el rendimiento de la ruta de acceso a los datos, como con paquetes y conexiones por segundo entre la red local y la red virtual. 

**Configuración de FastPath en una conexión nueva**

```azurecli-interactive
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --express-route-gateway-bypass true --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

**Actualización de una conexión existente para habilitar FastPath**

```azurecli-interactive
az network vpn-connection update --name ERConnection --resource-group ExpressRouteResourceGroup --express-route-gateway-bypass true
```

> [!NOTE]
> Puede usar [Connection Monitor](how-to-configure-connection-monitor.md) para comprobar que el tráfico llega al destino mediante FastPath.
>

## <a name="enroll-in-expressroute-fastpath-features-preview"></a>Inscripción en las características de FastPath de ExpressRoute (versión preliminar)

La compatibilidad de FastPath con el emparejamiento de redes virtuales está ahora en versión preliminar pública. La inscripción solo está disponible a través Azure PowerShell. Consulte las [características de la versión preliminar de FastPath](expressroute-howto-linkvnet-arm.md#enroll-in-expressroute-fastpath-features-preview) para obtener instrucciones sobre cómo inscribirse.

> [!NOTE] 
> Todas las conexiones configuradas para FastPath en la suscripción de destino se inscribirán en esta versión preliminar. No se recomienda habilitar esta versión preliminar en las suscripciones de producción.
> Si ya tiene FastPath configurado y desea inscribirse en la característica en versión preliminar, debe hacer lo siguiente:
> 1. Inscríbase en la característica en versión preliminar de FastPath con el comando de Azure PowerShell anterior.
> 1. Deshabilite y vuelva a habilitar FastPath en la conexión de destino.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita la conexión a ExpressRoute, desde la suscripción en que se encuentra la puerta de enlace, use el comando `az network vpn-connection delete` para quitar el vínculo entre la pasarela y el circuito.

```azurecli-interactive
az network vpn-connection delete --name ERConnection --resource-group ExpressRouteResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a conectar una red virtual a un circuito de la misma suscripción y de otra. Para obtener más información acerca de la puerta de enlace de ExpressRoute, consulte: 

> [!div class="nextstepaction"]
> [Acerca de las puertas de enlace de red virtual de ExpressRoute](expressroute-about-virtual-network-gateways.md)
