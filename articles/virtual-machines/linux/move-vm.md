---
title: Traslado de una máquina virtual mediante la CLI de Azure
description: Traslado de una máquina virtual a otra suscripción o grupo de recursos de Azure mediante la CLI de Azure.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: c2ceae3fbf1a59f5f148a488900e9bb88e6be4e4
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122688284"
---
# <a name="move-a-vm-to-another-subscription-or-resource-group"></a>Traslado de una máquina virtual a otra suscripción o grupo de recursos

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Conjuntos de escalado flexibles 

Este artículo le guiará en el procedimiento para mover una máquina virtual (VM) entre suscripciones o grupos de recursos. Mover una máquina virtual entre suscripciones puede ser útil si ha creado una máquina virtual en una suscripción personal y ahora quiere moverla a la suscripción de su compañía.

> [!IMPORTANT]
>Como parte de esta operación, se crean nuevos identificadores de recurso. Después de haber movido la VM, debe actualizar sus herramientas y scripts para usar los nuevos id. de recursos.
>


## <a name="use-the-azure-cli-to-move-a-vm"></a>Usar la CLI de Azure para mover una máquina virtual


Antes de poder mover la VM mediante la CLI de Azure, debe asegurarse de que las suscripciones de origen y de destino existen en el mismo inquilino. Para comprobar que ambas suscripciones tienen el mismo identificador de inquilino, use [az account show](/cli/azure/account).

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Si los identificadores de inquilino para las suscripciones de origen y destino no son los mismos, debe ponerse en contacto con [soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) para mover los recursos a un nuevo inquilino.

Para mover correctamente una máquina virtual, debe mover la máquina virtual y los recursos de apoyo. Use el comando [az resource list](/cli/azure/resource) para mostrar todos los recursos del grupo de recursos y sus identificadores. Ayuda a canalizar la salida de este comando a un archivo para que pueda copiar y pegar los identificadores en comandos posteriores.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```
La salida `table` no está disponible si usa `--interactive`. Cambie la salida a otra opción como `json`.

Para mover una máquina virtual y sus recursos a otro grupo de recursos, use [az resource move](/cli/azure/resource). En el ejemplo siguiente se muestra cómo mover una máquina virtual y los recursos más comunes que necesita. Use el parámetro **-ids** y pase una lista separada por comas (sin espacios) de identificadores de los recursos que se van a mover.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

Si quiere mover la máquina virtual y sus recursos a una suscripción diferente, agregue el parámetro **--destination-subscriptionId** para especificar la suscripción de destino.

Cuando se le pida que confirme que quiere mover los recursos especificados, escriba **Y** para confirmar.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Pasos siguientes
Puede mover muchos tipos diferentes de recursos entre suscripciones y grupos de recursos. Para obtener más información, consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](../../azure-resource-manager/management/move-resource-group-and-subscription.md).    
