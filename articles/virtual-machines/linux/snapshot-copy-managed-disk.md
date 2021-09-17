---
title: Creación de una instantánea de un disco duro virtual con la CLI de Azure
description: Aprenda a crear una copia de un disco duro virtual en Azure como copia de seguridad o para solucionar problemas.
author: roygara
ms.author: rogarana
manager: twooley
ms.service: storage
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 07/11/2018
ms.openlocfilehash: 2b6a9baba63a1e1f8cec93f059b31dd95a02e63a
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122692813"
---
# <a name="create-a-snapshot-using-the-portal-or-azure-cli"></a>Creación de una instantánea con el portal o la CLI de Azure

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Conjuntos de escalado flexibles 

Tome una instantánea de un disco de datos o del sistema operativo para realizar una copia de seguridad o para solucionar problemas de la máquina virtual. Una instantánea es una copia completa de solo lectura de un disco duro virtual. 

## <a name="use-azure-cli"></a>Uso de CLI de Azure 

En el ejemplo siguiente, se requiere el uso de [Cloud Shell](https://shell.azure.com/bash) o tener instalada la CLI de Azure.

Los pasos siguientes muestran cómo realizar una instantánea mediante el comando **az snapshot create** con el parámetro **--source-disk**. En el siguiente ejemplo se supone que hay una máquina virtual denominada *myVM* en el grupo de recursos *myResourceGroup*.

Obtenga el Id. de disco mediante [az vm show](/cli/azure/vm#az_vm_show).

```azurecli-interactive
osDiskId=$(az vm show \
   -g myResourceGroup \
   -n myVM \
   --query "storageProfile.osDisk.managedDisk.id" \
   -o tsv)
```

Realice una instantánea denominada *osDisk-backup* mediante [az snapshot create](/cli/azure/snapshot#az_snapshot_create).

```azurecli-interactive
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

> [!NOTE]
> Si desea almacenar la instantánea en un almacenamiento resistente a zonas, debe crearla en una región que admita [zonas de disponibilidad](../../availability-zones/az-overview.md) e incluir el parámetro **--sku Standard_ZRS**.

Puede ver una lista de las instantáneas con [az snapshot list](/cli/azure/snapshot#az_snapshot_list).

```azurecli-interactive
az snapshot list \
   -g myResourceGroup \
   - table
```

## <a name="use-azure-portal"></a>Usar Azure Portal 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En la parte superior izquierda, haga clic en **Crear un recurso** y busque **instantánea**. Seleccione **Instantánea** desde los resultados de búsqueda.
3. En la hoja **Instantánea**, haga clic en **Crear**.
4. Escriba un **nombre** para la instantánea.
5. Seleccione un valor de grupo de recursos existente o escriba el nombre para crear uno. 
7. Como **disco de origen**, seleccione el disco administrado para la instantánea.
8. Seleccione el **tipo de cuenta** que se usará para almacenar la instantánea. Use **HDD estándar**, a menos que necesite almacenarla en un disco SSD de alto rendimiento.
9. Haga clic en **Crear**.


## <a name="next-steps"></a>Pasos siguientes

 Cree una máquina virtual a partir de una instantánea; para ello, cree primero un disco administrado con la instantánea y asocie el nuevo disco administrado como disco del sistema operativo. Para más información, consulte el script que se incluye en [Creación de una máquina virtual a partir de una instantánea](/previous-versions/azure/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot?toc=%2fcli%2fmodule%2ftoc.json).