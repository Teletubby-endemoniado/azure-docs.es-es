---
title: Implementación y configuración de Azure VMware Solution
description: Aprenda a usar la información recopilada en la fase de planeación para implementar y configurar la nube privada de Azure VMware Solution.
ms.topic: tutorial
ms.custom: contperf-fy22q1, devx-track-azurecli
ms.date: 07/28/2021
ms.openlocfilehash: 0df1634d047dfe6abfaa717fd10b75b99f100076
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128623938"
---
# <a name="deploy-and-configure-azure-vmware-solution"></a>Implementación y configuración de Azure VMware Solution

Una vez [planeada la implementación](plan-private-cloud-deployment.md), implementará y configurará la nube privada de Azure VMware Solution. 

En el diagrama se muestra el flujo de trabajo de implementación de Azure VMware Solution. 

:::image type="content" source="media/deploy-azure-vmware-solution-workflow.png" alt-text="Diagrama que muestra el flujo de trabajo de implementación de Azure VMware Solution" lightbox="media/deploy-azure-vmware-solution-workflow.png" border="false":::

En estos procedimientos, hará lo siguiente:

> [!div class="checklist"]
> * Registro del proveedor de recursos y creación de una nube privada
> * Conexión a una puerta de enlace de red virtual de ExpressRoute nueva o existente
> * Validación de la conexión de red

Cuando haya terminado, siga los pasos recomendados al final para continuar con los pasos de esta guía de introducción.

## <a name="register-the-microsoftavs-resource-provider"></a>Registre el proveedor de recursos Microsoft.AVS.

[!INCLUDE [register-resource-provider-steps](includes/register-resource-provider-steps.md)]

## <a name="create-an-azure-vmware-solution-private-cloud"></a>Creación de una nube privada de Azure VMware Solution

[!INCLUDE [create-private-cloud-azure-portal-steps](includes/create-private-cloud-azure-portal-steps.md)]

>[!NOTE]
>Puede encontrar información completa de este paso en el vídeo [Azure VMware Solution: Implementación](https://www.youtube.com/embed/gng7JjxgayI).


## <a name="connect-to-azure-virtual-network-with-expressroute"></a>Conexión a Azure Virtual Network con ExpressRoute

En la fase de planeamiento, ha definido si va a usar una puerta de enlace de red virtual de ExpressRoute *existente* o  *nueva*.  

:::image type="content" source="media/connect-expressroute-vnet-workflow.png" alt-text="Diagrama que muestra el flujo de trabajo para conectar Azure Virtual Network a ExpressRoute en Azure VMware Solution." border="false":::

>[!IMPORTANT]
>[!INCLUDE [disk-pool-planning-note](includes/disk-pool-planning-note.md)] 

### <a name="use-a-new-expressroute-virtual-network-gateway"></a>Uso de una nueva puerta de enlace de red virtual de ExpressRoute

>[!IMPORTANT]
>Debe tener una red virtual con una subred de puerta de enlace que aún **no** tenga una puerta de enlace de red virtual.

| Si | Entonces  |
| --- | --- |
| Si no tiene una red virtual...     |  Cree los siguientes elementos:<ol><li><a href="tutorial-configure-networking.md#create-a-vnet-manually">Red virtual</a></li><li><a href="../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet">GatewaySubnet</a></li><li><a href="tutorial-configure-networking.md#create-a-virtual-network-gateway">Puerta de enlace de red virtual</a></li><li><a href="tutorial-configure-networking.md#connect-expressroute-to-the-virtual-network-gateway">Conexión de ExpressRoute a la puerta de enlace</a></li></ol>        |
| Ya tiene una red virtual **sin** subred de puerta de enlace...   | Cree los siguientes elementos: <ol><li><a href="../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet">GatewaySubnet</a></li><li><a href="tutorial-configure-networking.md#create-a-virtual-network-gateway">Puerta de enlace de red virtual</a></li><li><a href="tutorial-configure-networking.md#connect-expressroute-to-the-virtual-network-gateway">Conexión de ExpressRoute a la puerta de enlace</a></li></ol>          |
| Ya tiene una red virtual **con** una subred de puerta de enlace... | Cree los siguientes elementos: <ol><li><a href="tutorial-configure-networking.md#create-a-virtual-network-gateway">Puerta de enlace de red virtual</a></li><li><a href="tutorial-configure-networking.md#connect-expressroute-to-the-virtual-network-gateway">Conexión de ExpressRoute a la puerta de enlace</a></li></ol>    |

### <a name="use-an-existing-virtual-network-gateway"></a>Uso de una puerta de enlace de red virtual existente

[!INCLUDE [connect-expressroute-to-vnet](includes/connect-expressroute-vnet.md)]


## <a name="validate-the-connection"></a>Validación de la conexión

Debe tener conectividad entre la instancia de Azure Virtual Network donde finaliza ExpressRoute y la nube privada de Azure VMware Solution. 

1. Use una [máquina virtual](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine) dentro de la instancia de Azure Virtual Network en la que finaliza ExpressRoute de Azure VMware Solution. Para más información, consulte [Conectar a Azure Virtual Network con ExpressRoute](#connect-to-azure-virtual-network-with-expressroute).  

   1. Inicie sesión en [Azure Portal](https://portal.azure.com).

   1. Vaya a una máquina virtual que se encuentre en estado de ejecución y, en **Configuración**, seleccione **Redes** y seleccione el recurso de la interfaz de red.

      :::image type="content" source="../virtual-network/media/diagnose-network-routing-problem/view-nics.png" alt-text="Captura de pantalla que muestra la configuración de la interfaz de red virtual.":::

   1. A la izquierda, seleccione **Rutas eficaces**. Verá una lista de prefijos de dirección que se encuentran dentro del bloque CIDR `/22` que especificó durante la fase de implementación.

1. Si desea iniciar sesión en vCenter y NSX-T Manager, abra un explorador web e inicie sesión en la misma máquina virtual que se usa para la validación de rutas de red.  

   Puede identificar las direcciones IP y las credenciales de la consola de NSX-T Manager y vCenter en Azure Portal.  Seleccione una nube privada y, después, **Administrar** > **Identidad**.

   :::image type="content" source="media/tutorial-access-private-cloud/ss4-display-identity.png" alt-text="Captura de pantalla que muestra las direcciones URL y las credenciales de NSX Manager y vCenter de la nube privada." border="true":::


## <a name="next-steps"></a>Pasos siguientes

En la siguiente sección, conectará Azure VMware Solution a la red local a través de ExpressRoute.

> [!div class="nextstepaction"]
> [Tutorial: Emparejamiento de entornos locales con una nube privada](tutorial-expressroute-global-reach-private-cloud.md)
