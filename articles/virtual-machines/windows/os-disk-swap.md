---
title: Intercambio de discos del sistema operativo en una máquina virtual de Azure con PowerShell
description: Intercambie el disco del sistema operativo que se usa en una máquina virtual de Azure mediante PowerShell.
author: cynthn
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 04/24/2018
ms.author: cynthn
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 2892ae3ff10110c2e45d29edd009a9116da0b166
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122687511"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-powershell"></a>Intercambio del disco del sistema operativo que se usa en una máquina virtual de Azure mediante PowerShell

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles 

Si ya tiene una máquina virtual, pero quiere intercambiar el disco por uno de copia de seguridad u otro disco del sistema operativo, puede usar Azure PowerShell para intercambiar los discos del sistema operativo. No es necesario eliminar ni volver a crear la máquina virtual. Incluso puede utilizar un disco administrado de otro grupo de recursos, siempre y cuando no esté en uso.

 

Es necesario que la máquina virtual esté detenida o sin asignar; a continuación, el identificador de recurso del disco administrado se puede reemplazar con el identificador de recurso de otro disco administrado.

Asegúrese de que el tipo de almacenamiento y el tamaño de la máquina virtual son compatibles con el disco que quiere adjuntar. Por ejemplo, si el disco que quiere usar está en Premium Storage, la máquina virtual debe ser compatible con Premium Storage (por ejemplo, debe tener un tamaño de la serie DS). Ambos discos también deben tener el mismo tamaño.
Y asegúrese de que no está combinando una máquina virtual sin cifrar con un disco de sistema operativo cifrado, ya que esto no se admite. Si la máquina virtual no usa Azure Disk Encryption, el disco del sistema operativo que se intercambia no debe usar tampoco Azure Disk Encryption. Si los discos usan conjuntos de cifrado de disco, ambos discos deben pertenecer al mismo conjunto de cifrado de disco.

Obtenga una lista de discos de un grupo de recursos mediante [Get-AzDisk](/powershell/module/az.compute/get-azdisk).

```azurepowershell-interactive
Get-AzDisk -ResourceGroupName myResourceGroup | Format-Table -Property Name
```
 
Cuando tenga el nombre del disco que quiera usar, establézcalo como el disco del sistema operativo para la máquina virtual. En este ejemplo se detiene o desasigna la máquina virtual llamada *myVM* y se asigna el disco llamado *newDisk* como el nuevo disco del sistema operativo. 
 
```azurepowershell-interactive 
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name 

# Update the VM with the new OS disk
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm 

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup

```

**Pasos siguientes**

Para crear una copia de un disco, consulte [Instantánea de un disco](snapshot-copy-managed-disk.md).
