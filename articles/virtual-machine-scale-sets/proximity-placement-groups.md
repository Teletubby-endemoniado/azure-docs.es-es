---
title: Grupos con ubicación por proximidad para conjuntos de escalado de máquinas virtuales
description: Obtenga información sobre la creación y el uso de grupos de selección de ubicación de proximidad para conjuntos de escalado de máquinas virtuales Windows en Azure.
author: cynthn
ms.author: cynthn
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: proximity-placement-groups
ms.date: 07/01/2019
ms.reviewer: zivr
ms.custom: mimckitt, devx-track-azurepowershell
ms.openlocfilehash: b5533f14a239e3e2d01c597d57ab2b2726e930b9
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122693269"
---
# <a name="creating-and-using-proximity-placement-groups-using-powershell"></a>Creación y uso de grupos de selección de ubicación de proximidad con PowerShell

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado uniformes

Para conseguir máquinas virtuales lo más cercanas posible con la menor latencia posible, debe implementar el conjunto de escalado dentro de un [grupo de selección de ubicación de proximidad](../virtual-machines/co-location.md#proximity-placement-groups).

Un grupo de selección de ubicación de proximidad es una agrupación lógica que se usa para asegurarse de que los recursos de proceso de Azure se encuentran físicamente cercanos entre sí. Los grupos de selección de ubicación de proximidad son útiles para las cargas de trabajo en las que la latencia baja es un requisito.


## <a name="create-a-proximity-placement-group"></a>Creación de un grupo de selección de ubicación por proximidad
Cree un grupo de selección de ubicación de proximidad con el cmdlet [New-AzProximityPlacementGroup](/powershell/module/az.compute/new-azproximityplacementgroup). 

```azurepowershell-interactive
$resourceGroup = "myPPGResourceGroup"
$location = "East US"
$ppgName = "myPPG"
New-AzResourceGroup -Name $resourceGroup -Location $location
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard
```

## <a name="list-proximity-placement-groups"></a>Enumeración de los grupos de selección de ubicación de proximidad

Puede enumerar todos los grupos de selección de ubicación de proximidad mediante el cmdlet [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup).

```azurepowershell-interactive
Get-AzProximityPlacementGroup
```


## <a name="create-a-scale-set"></a>Creación de un conjunto de escalado

Cree una escala en el grupo de selección de ubicación de proximidad con `-ProximityPlacementGroup $ppg.Id` para hacer referencia al identificador de grupo de selección de ubicación de proximidad cuando use [New-AzVMSS](/powershell/module/az.compute/new-azvmss) para crear el conjunto de escalado.

```azurepowershell-interactive
$scalesetName = "myVM"

New-AzVmss `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -VMScaleSetName $scalesetName `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ProximityPlacementGroup $ppg.Id
```

Puede ver la instancia en el grupo de selección de ubicación mediante [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup).

```azurepowershell-interactive
  Get-AzProximityPlacementGroup `
   -ResourceId $ppg.Id | Format-Table `
   -Wrap `
   -Property VirtualMachineScaleSets
```

## <a name="next-steps"></a>Pasos siguientes

También puede usar la [CLI de Azure](../virtual-machines/linux/proximity-placement-groups.md) para crear grupos de selección de ubicación de proximidad.
