---
title: Creación de un host bastión con Azure PowerShell | Microsoft Docs
description: Aprenda a crear un host de Azure Bastion con PowerShell.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 09/22/2021
ms.author: cherylmc
ms.custom: ignite-fall-2021
ms.openlocfilehash: faa6fe1b7fdccf81f83b591c1f0258d27dc9fdd0
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131071538"
---
# <a name="create-an-azure-bastion-host-using-azure-powershell"></a>Creación de un host Azure Bastion con Azure PowerShell

En este artículo se muestra cómo crear un host de Azure Bastion mediante PowerShell. Una vez que haya aprovisionado el servicio Azure Bastion en la red virtual, ya puede disponer de la completa experiencia de RDP/SSH en todas las máquinas virtuales de la misma red virtual. La implementación de Azure Bastion se realiza por red virtual, no por suscripción o cuenta, ni por máquina virtual.

Opcionalmente, puede crear un host de Azure Bastion mediante los métodos siguientes:
* [Azure Portal](./tutorial-create-host-portal.md)
* [CLI de Azure](create-host-cli.md)

[!INCLUDE [About SKUs](../../includes/bastion-sku-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Compruebe que tiene una suscripción a Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) o registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

 > [!NOTE]
 > En este momento no se admite el uso de Azure Bastion con las zonas de DNS privado de Azure. Antes de empezar, asegúrese de que la red virtual en la que planea implementar el recurso de Bastion no esté vinculada a una zona de DNS privado.
 >

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Creación de un host de Bastion

Esta sección le ayuda a crear un nuevo recurso Azure Bastion con Azure PowerShell.

1. Cree una red virtual y una subred Azure Bastion. Debe crear la subred Azure Bastion con el valor de nombre **AzureBastionSubnet**. Este valor permite a Azure saber en qué subred se deben implementar los recursos de Bastion. Esta es diferente de una subred de puerta de enlace de VPN.

   [!INCLUDE [Note about BastionSubnet size.](../../includes/bastion-subnet-size.md)]

   ```azurepowershell-interactive
   $subnetName = "AzureBastionSubnet"
   $subnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   $vnet = New-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myBastionRG" -Location "westeurope" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

2. Cree una dirección IP pública para Azure Bastion. La IP pública es la dirección IP pública del recurso de Bastion en la que se accederá a RDP/SSH (a través del puerto 443). La dirección debe estar en la misma región que el recurso de Bastion que está creando.

   ```azurepowershell-interactive
   $publicip = New-AzPublicIpAddress -ResourceGroupName "myBastionRG" -name "myPublicIP" -location "westeurope" -AllocationMethod Static -Sku Standard
   ```

3. Cree un nuevo recurso Azure Bastion en AzureBastionSubnet de su red virtual. El recurso de Bastion tarda aproximadamente cinco minutos en crearse e implementarse.

   ```azurepowershell-interactive
   $bastion = New-AzBastion -ResourceGroupName "myBastionRG" -Name "myBastion" -PublicIpAddress $publicip -VirtualNetwork $vnet
   ```
## <a name="disassociate-the-vm-public-ip-address"></a>Desasociación de la dirección IP pública de una máquina virtual

Azure Bastion no usa la dirección IP pública para conectarse a la máquina virtual cliente. Si no necesita la dirección IP pública de la máquina virtual, puede desasociar la dirección IP pública mediante los pasos de este artículo: [Desasociación de una dirección IP pública de una máquina virtual de Azure](../virtual-network/ip-services/remove-public-ip-address-vm.md).

## <a name="next-steps"></a>Pasos siguientes

* Para más información, lea las [P+F sobre Bastion](bastion-faq.md).
* Para usar grupos de seguridad de red con la subred de Azure Bastion, consulte [Trabajo con grupos de seguridad de red](bastion-nsg.md).
