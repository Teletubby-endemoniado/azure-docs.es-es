---
title: 'Inicio rápido: Creación y modificación de un circuito con ExpressRoute: Azure PowerShell'
description: En este tutorial se muestra cómo crear, aprovisionar, comprobar, actualizar, eliminar y desaprovisionar un circuito ExpressRoute.
services: expressroute
author: duongau
ms.author: duau
ms.date: 10/12/2020
ms.topic: quickstart
ms.service: expressroute
ms.custom: devx-track-azurepowershell, mode-api
ms.openlocfilehash: 8b2ad2e4d899603c5e20870a7c6e01c3b83d60e2
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131073137"
---
# <a name="quickstart-create-and-modify-an-expressroute-circuit-using-azure-powershell"></a>Inicio rápido: Creación y modificación de un circuito ExpressRoute mediante Azure PowerShell

En este inicio rápido, se muestra cómo crear un circuito ExpressRoute mediante cmdlets de PowerShell y el modelo de implementación de Azure Resource Manager. También puede comprobar el estado, la actualización, la eliminación o el desaprovisionamiento de un circuito.

## <a name="prerequisites"></a>Prerrequisitos

* Revise los [Requisitos previos y lista de comprobación de ExpressRoute](expressroute-prerequisites.md) y los [Flujos de trabajo de ExpressRoute para aprovisionamiento de circuitos y estados de circuitos de ExpressRoute](expressroute-workflows.md) antes de comenzar la configuración.
* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Azure PowerShell instalado localmente o Azure Cloud Shell

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>Crear y aprovisionar un circuito ExpressRoute
### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Iniciar sesión en la cuenta de Azure y seleccione la suscripción

[!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]

### <a name="get-the-list-of-supported-providers-locations-and-bandwidths"></a>Obtención de la lista de proveedores, ubicaciones y anchos de banda admitidos
Para crear un circuito ExpressRoute, necesita la lista de proveedores de conectividad, ubicaciones y opciones de ancho de banda admitidas.

El cmdlet de PowerShell **Get-AzExpressRouteServiceProvider** devuelve esta información, que se usará en pasos posteriores:

```azurepowershell-interactive
Get-AzExpressRouteServiceProvider
```

Compruebe si aparece su proveedor de conectividad. Tome nota de la información siguiente, ya que la va a necesitar para crear un circuito:

* Nombre
* PeeringLocations
* BandwidthsOffered

Ahora está listo para crear un circuito ExpressRoute.

### <a name="create-an-expressroute-circuit"></a>Creación de un circuito ExpressRoute
Si todavía no tiene un grupo de recursos, debe crear uno antes de crear ExpressRoute. Para ello, ejecute el siguiente comando:

```azurepowershell-interactive
New-AzResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```

En el ejemplo siguiente se muestra cómo crear un circuito ExpressRoute de 200 Mbps a través de Equinix en Silicon Valley. Si usa otro proveedor y otra configuración, reemplace esa información al realizar la solicitud. Use el ejemplo siguiente para solicitar una nueva clave de servicio:

```azurepowershell-interactive
New-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Asegúrese de que especifica el nivel y la familia correctos de SKU.

* El nivel de SKU determina si un circuito ExpressRoute es [Local](expressroute-faqs.md#expressroute-local), Estándar o [Premium](expressroute-faqs.md#expressroute-premium). Puede especificar *Local*, *Estándar o *Premium*. No se puede cambiar la SKU de *Estándar o Premium* a *Local*.
* La familia de SKU determina el tipo de facturación. Puede seleccionar *MeteredData* para el plan de datos limitado y *UnlimitedData* para el plan de datos ilimitado. Puede cambiar el tipo de facturación de *MeteredData* a *UnlimitedData*, pero no puede cambiar el tipo de *UnlimitedData* a *MeteredData*. Un circuito *Local* siempre es *UnlimitedData*.

> [!IMPORTANT]
> El circuito ExpressRoute se factura a partir del momento en que se emite una clave de servicio. Asegúrese de realizar esta operación cuando el proveedor de conectividad esté listo para aprovisionar el circuito.
>

La respuesta contiene la clave del servicio. Puede obtener una descripción detallada de todos los parámetros ejecutando el siguiente comando:

```azurepowershell-interactive
get-help New-AzExpressRouteCircuit -detailed
```


### <a name="list-all-expressroute-circuits"></a>Lista de todos los circuitos ExpressRoute
Para obtener una lista de todos los circuitos ExpressRoute que haya creado, ejecute el comando **Get-AzExpressRouteCircuit**:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

La respuesta será similar al siguiente ejemplo:

```azurepowershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                    "Name": "Standard_MeteredData",
                                    "Tier": "Standard",
                                    "Family": "MeteredData"
                                    }
CircuitProvisioningState          : Enabled
ServiceProviderProvisioningState  : NotProvisioned
ServiceProviderNotes              :
ServiceProviderProperties         : {
                                    "ServiceProviderName": "Equinix",
                                    "PeeringLocation": "Silicon Valley",
                                    "BandwidthInMbps": 200
                                    }
ServiceKey                        : **************************************
Peerings                          : []
```

Esta información se puede recuperar en cualquier momento con el cmdlet `Get-AzExpressRouteCircuit` . Si se realiza la llamada sin parámetros, se obtendrá una lista de todos los circuitos. La clave de servicio se muestra en el campo *ServiceKey*:

```azurepowershell-interactive
Get-AzExpressRouteCircuit
```

La respuesta será similar al siguiente ejemplo:

```azurepowershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                    "Name": "Standard_MeteredData",
                                    "Tier": "Standard",
                                    "Family": "MeteredData"
                                    }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                    "ServiceProviderName": "Equinix",
                                    "PeeringLocation": "Silicon Valley",
                                    "BandwidthInMbps": 200
                                    }
ServiceKey                       : **************************************
Peerings                         : []
```

### <a name="send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>Envío de la clave de servicio al proveedor de conectividad para el aprovisionamiento
*ServiceProviderProvisioningState* proporciona información sobre el estado actual del aprovisionamiento en el lado del proveedor de servicios. El estado proporciona el estado del lado Microsoft. Para obtener más información sobre los estados de aprovisionamiento del circuito, consulte el artículo [Flujos de trabajo](expressroute-workflows.md#expressroute-circuit-provisioning-states).

Cuando se crea un nuevo circuito ExpressRoute, dicho circuito tiene el siguiente estado:

```azurepowershell
ServiceProviderProvisioningState : NotProvisioned
CircuitProvisioningState         : Enabled
```

El circuito pasa al estado siguiente cuando el proveedor de conectividad lo habilita para el usuario:

```azurepowershell
ServiceProviderProvisioningState : Provisioning
Status                           : Enabled
```

Para usar el circuito ExpressRoute, debe tener el siguiente estado:

```azurepowershell
ServiceProviderProvisioningState : Provisioned
CircuitProvisioningState         : Enabled
```

### <a name="periodically-check-the-status-and-the-state-of-the-circuit-key"></a>Comprobación periódica del estado y la condición de la clave del circuito
La comprobación del estado y la condición de la clave de servicio le informará cuando el proveedor haya aprovisionado el circuito. Después de configurar el circuito, *ServiceProviderProvisioningState* aparece como *Provisioned*, tal como se muestra en el ejemplo siguiente:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

La respuesta será similar al siguiente ejemplo:

```azurepowershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                    "Name": "Standard_MeteredData",
                                    "Tier": "Standard",
                                    "Family": "MeteredData"
                                    }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                    "ServiceProviderName": "Equinix",
                                    "PeeringLocation": "Silicon Valley",
                                    "BandwidthInMbps": 200
                                    }
ServiceKey                       : **************************************
Peerings                         : []
```

### <a name="create-your-routing-configuration"></a>Creación de la configuración de enrutamiento
Consulte [Creación y modificación del enrutamiento de un circuito ExpressRoute mediante PowerShell](expressroute-howto-routing-arm.md) para ver las instrucciones paso a paso.

> [!IMPORTANT]
> Estas instrucciones se aplican solo a los circuitos creados con proveedores de servicios que ofrecen servicios de conectividad de nivel 2. Si usa un proveedor de servicios que ofrece servicios administrados de nivel 3 (normalmente VPN IP, como MPLS), el mismo proveedor de conectividad configura y administra el enrutamiento.
>

### <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>Vinculación de una red virtual a un circuito ExpressRoute
A continuación, vincule una red virtual a su circuito ExpressRoute. Consulte el artículo [Vinculación de redes virtuales a circuitos ExpressRoute](expressroute-howto-linkvnet-arm.md) al trabajar con el modelo de implementación de Resource Manager.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Obtención del estado de un circuito ExpressRoute
Esta información se puede recuperar en cualquier momento con el cmdlet **Get-AzExpressRouteCircuit**. Si se realiza la llamada sin parámetros, se obtendrá una lista de todos los circuitos.

```azurepowershell-interactive
Get-AzExpressRouteCircuit
```

La respuesta es similar al siguiente ejemplo:

```azurepowershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                    "Name": "Standard_MeteredData",
                                    "Tier": "Standard",
                                    "Family": "MeteredData"
                                    }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                        "ServiceProviderName": "Equinix",
                                        "PeeringLocation": "Silicon Valley",
                                        "BandwidthInMbps": 200
                                    }
ServiceKey                       : **************************************
Peerings                         : []
```

Se puede obtener información sobre un circuito ExpressRoute específico si se pasa el nombre del grupo de recursos y el nombre del circuito como parámetro a la llamada:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

La respuesta será similar al siguiente ejemplo:

```azurepowershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"################################"
ProvisioningState                : Succeeded
Sku                              : {
                                        "Name": "Standard_MeteredData",
                                        "Tier": "Standard",
                                        "Family": "MeteredData"
                                    }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                        "ServiceProviderName": "Equinix",
                                        "PeeringLocation": "Silicon Valley",
                                        "BandwidthInMbps": 200
                                    }
ServiceKey                       : **************************************
Peerings                         : []
```

Puede obtener una descripción detallada de todos los parámetros ejecutando el siguiente comando:

```azurepowershell-interactive
get-help Get-AzExpressRouteCircuit -detailed
```

## <a name="modifying-an-expressroute-circuit"></a><a name="modify"></a>Modificación de un circuito ExpressRoute
Puede modificar determinadas propiedades de un circuito ExpressRoute sin afectar a la conectividad.

Puede hacer las siguientes tareas sin experimentar tiempo de inactividad:

* Habilitar o deshabilitar el complemento ExpressRoute Premium en su circuito ExpressRoute. No se admite el cambio de la SKU de *Estándar o Premium* a *Local*.
* Aumente el ancho de banda del circuito ExpressRoute si el puerto tiene capacidad disponible. No se admite la degradación del ancho de banda de un circuito.
* Cambio del plan de medición de datos limitados a datos ilimitados. No se admite cambiar el plan de medición de datos ilimitados a datos limitados.
* Puede habilitar y deshabilitar *Allow Classic Operations*(Permitir operaciones clásicas).

Para más información sobre los límites y las limitaciones, consulte las [preguntas más frecuentes de ExpressRoute](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Para habilitar el complemento ExpressRoute Premium
Puede habilitar el complemento ExpressRoute Premium para el circuito existente mediante el siguiente fragmento de PowerShell:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

El circuito ahora tiene habilitadas las características del complemento ExpressRoute Premium. La facturación de la capacidad del complemento Premium comenzará en cuanto el comando se ejecute correctamente.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Para deshabilitar el complemento ExpressRoute Premium
> [!IMPORTANT]
> Esta operación puede producir un error si usa recursos que son más grandes de lo que está permitido para el circuito estándar.
>

Tenga en cuenta la información siguiente:

* Debe asegurarse de que el número de redes virtuales vinculadas al circuito es inferior a 10 antes de realizar la degradación de Premium a Estándar. Si no lo hace, se producirá un error en la solicitud de actualización y se le facturará con las tarifas de nivel premium.
* Tiene que desvincular primero todas las redes virtuales en otras regiones geopolíticas. Si no elimina el vínculo, se producirá un error en la solicitud de actualización y se le continuará facturando con las tarifas de nivel Premium.
* La tabla de enrutamiento tiene que tener menos de 4.000 rutas para el emparejamiento entre pares privados. Si la tabla de rutas tiene más de 4000 rutas, se quitará la sesión de BGP. La sesión de BGP no se volverá a habilitar hasta que el número de prefijos anunciados sea inferior a 4000.

Puede deshabilitar el complemento ExpressRoute Premium en el circuito existente mediante el siguiente cmdlet de PowerShell:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Para actualizar el ancho de banda del circuito ExpressRoute
Consulte la página [P+F de ExpressRoute](expressroute-faqs.md)para conocer las opciones de ancho de banda compatibles con su proveedor. Puede elegir cualquier tamaño mayor que el tamaño de su circuito existente.

> [!IMPORTANT]
> Si el puerto existente no tiene la capacidad adecuada, tendrá que volver a crear el circuito ExpressRoute. El circuito no se puede actualizar si no hay más capacidad disponible en la ubicación.
>
> No podrá reducir el ancho de banda de un circuito ExpressRoute sin interrupciones. Para degradar un ancho de banda, es necesario desaprovisionar el circuito ExpressRoute y luego volver a aprovisionar un nuevo circuito ExpressRoute.
>

Cuando haya decidido el tamaño que necesita, use el comando siguiente para cambiar el tamaño del circuito:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

El circuito se actualizará en el lado de Microsoft. Así pues, debe ponerse en contacto con su proveedor de conectividad para actualizar las configuraciones de su parte para que coincidan con este cambio. Después de realizar esta notificación, se le empezará a facturar por la opción de ancho de banda actualizada.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Para pasar la SKU de limitada a ilimitada
Puede cambiar la SKU de un circuito ExpressRoute mediante el siguiente fragmento de código de PowerShell:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Para controlar el acceso a los entornos de implementación clásica y del Resource Manager
Revise las instrucciones que se ofrecen en [Transición de los circuitos ExpressRoute desde el modelo de implementación clásica al modelo de implementación de Resource Manager](expressroute-howto-move-arm.md).

## <a name="deprovisioning-an-expressroute-circuit"></a><a name="delete"></a>Desaprovisionamiento de un circuito ExpressRoute
Tenga en cuenta la información siguiente:

* Todas las redes virtuales deben desvincularse del circuito ExpressRoute. Si se produce un error en esta operación, compruebe si hay alguna red virtual vinculada al circuito.
* Si el estado de aprovisionamiento del proveedor de servicios del circuito ExpressRoute es **Aprovisionando** o **Aprovisionado**, debe colaborar con su proveedor de servicios para que desaprovisionen el circuito. Se le siguen reservando recursos y facturándole por ello hasta que el proveedor de servicios complete el desaprovisionamiento del circuito y nos lo notifique.
* Si el proveedor de servicios ha desaprovisionado el circuito, es decir, el estado de aprovisionamiento del proveedor de servicios está establecido en **No aprovisionado**, puede eliminar el circuito. Se detendrá la facturación del circuito.

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede eliminar el circuito ExpressRout con la ejecución del siguiente comando:

```azurepowershell-interactive
Remove-AzExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Pasos siguientes

Después de crear el circuito y aprovisionarlo con el proveedor, continúe en el paso siguiente para configurar el emparejamiento:

> [!div class="nextstepaction"]
> [Crear y modificar el enrutamiento para el circuito ExpressRoute](expressroute-howto-routing-arm.md)
