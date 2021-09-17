---
title: Conversión de almacenamiento de discos administrados entre diferentes tipos de discos mediante la CLI de Azure
description: Cómo convertir discos administrados de Azure entre los diferentes tipos de discos mediante la CLI de Azure.
author: roygara
ms.service: virtual-machines
ms.collection: linux
ms.topic: how-to
ms.date: 02/13/2021
ms.author: albecker
ms.subservice: disks
ms.openlocfilehash: 385d801d43a41bb836e04398427fff6c01b1e357
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122692134"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-or-premium-to-standard"></a>Conversión de almacenamiento de Azure Managed Disks de estándar a Premium o de Premium a estándar

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Conjuntos de escalado flexibles 

Hay cuatro tipos de discos de Azure Managed Disks: Azure Ultra Disks, SSD Premium, SSD estándar y HDD estándar. Puede cambiar entre SSD Premium, SSD estándar y HDD estándar según las necesidades de rendimiento. Todavía no puede cambiar de o a un disco Ultra; debe implementar uno nuevo.

Esta funcionalidad no se admite para discos no administrados. Sin embargo, puede [convertir un disco no administrado en un disco administrado](convert-unmanaged-to-managed-disks.md) de manera sencilla para poder cambiar entre los tipos de disco.

En este artículo se muestra cómo convertir discos administrados de un tipo de disco a otro mediante la CLI de Azure. Para instalar o actualizar la herramienta, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="before-you-begin"></a>Antes de empezar

* La conversión de disco requiere reiniciar la máquina virtual (VM), por lo que debe programar la migración del almacenamiento de discos durante una ventana de mantenimiento existente previamente.
* En el caso de los discos no administrados, primero [debe convertirlos en discos administrados](convert-unmanaged-to-managed-disks.md) para poder cambiar entre las opciones de almacenamiento.


## <a name="switch-all-managed-disks-of-a-vm-between-from-one-account-to-another"></a>Cambio de todos los discos administrados de una máquina virtual entre una cuenta y otra

En este ejemplo se muestra cómo convertir todos los discos de una máquina virtual en Premium Storage. Sin embargo, al cambiar la variable sku en este ejemplo, puede convertir el tipo de disco de la máquina virtual en SSD estándar o HDD estándar. Para poder usar discos administrados prémium, la máquina virtual debe usar un [tamaño de máquina virtual](../sizes.md) que admita Premium Storage. En este ejemplo también se pasa a un tamaño que admite el almacenamiento Premium.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS, StandardSSD_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the SKU of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the SKU of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="switch-individual-managed-disks-from-one-disk-type-to-another"></a>Cambio de discos administrados individuales de un tipo de disco a otro

En el caso de la carga de trabajo de desarrollo y pruebas, puede que prefiera tener una combinación de discos estándar y Premium para reducir los costos. Puede optar por actualizar solamente los discos que necesitan un rendimiento más eficaz. En este ejemplo se muestra cómo convertir un solo disco de máquina virtual de almacenamiento estándar al prémium. Sin embargo, al cambiar la variable sku en este ejemplo, puede convertir el tipo de disco de la máquina virtual en SSD estándar o HDD estándar. Para usar Managed Disks premium, la máquina virtual debe usar un [Tamaño de máquina virtual](../sizes.md) que admita Premium Storage. En este ejemplo también se pasa a un tamaño que admite el almacenamiento Premium.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from Standard to Premium
size='Standard_DS2_v2'

#Choose between Standard_LRS, StandardSSD_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports Premium storage 
#Skip this step if converting storage from Premium to Standard
az vm resize --ids $vmId --size $size

# Update the SKU
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="switch-managed-disks-from-one-disk-type-to-another"></a>Cambio de los discos administrados de un tipo de disco a otro

Siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione la máquina virtual de la lista de **Virtual Machines**.
3. Si la máquina virtual no se detiene, seleccione la opción **Detener** que se encuentra en la parte superior del panel **Información general** de la máquina virtual y espere a que se detenga.
4. En el panel de la máquina virtual, seleccione **Discos** del menú.
5. Seleccione el disco que quiera convertir.
6. Seleccione **Configuración** del menú.
7. Cambie el **tipo de cuenta** del tipo de disco original al tipo de disco deseado.
8. Seleccione **Guardar** y cierre el panel de discos.

La actualización del tipo de disco es instantánea. Puede reiniciar la máquina virtual después de la conversión.

## <a name="next-steps"></a>Pasos siguientes

Realice una copia de solo lectura de una máquina virtual mediante [instantáneas](snapshot-copy-managed-disk.md).