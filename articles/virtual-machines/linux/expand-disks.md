---
title: Expansión de discos duros virtuales en máquinas virtuales Linux
description: Aprenda a expandir discos duros virtuales en una máquina virtual Linux con la CLI de Azure.
author: roygara
ms.service: virtual-machines
ms.collection: linux
ms.topic: how-to
ms.date: 11/02/2021
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions, ignite-fall-2021
ms.openlocfilehash: f9d38bdbbd21d2bc1d54e74c9fd413bbfc38e93a
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131448961"
---
# <a name="expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a>Expansión de discos duros virtuales en una VM Linux con la CLI de Azure

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Conjuntos de escalado flexibles 

En este artículo se describe cómo expandir discos administrados para una máquina virtual (VM) Linux con la CLI de Azure. Tiene la opción de [agregar discos de datos](add-disk.md) para proporcionar espacio de almacenamiento adicional, y también puede expandir un disco de datos existente. Normalmente, el tamaño predeterminado del disco duro virtual del sistema operativo (SO) es de 30 GB en una VM Linux en Azure. 

> [!WARNING]
> Asegúrese siempre de que el sistema de archivos está en buen estado, que el tipo de tabla de partición de disco admite el nuevo tamaño y no olvide hacer una copia de seguridad de los datos antes de realizar operaciones de cambio de tamaño de disco. Para más información, consulte el [inicio rápido de Azure Backup](../../backup/quick-backup-vm-portal.md). 

## <a name="expand-an-azure-managed-disk"></a>Expansión de un disco administrado de Azure

### <a name="resize-without-downtime-preview"></a>Cambio de tamaño sin tiempo de inactividad (versión preliminar)

Ahora puede cambiar el tamaño de los discos administrados sin desasignar la máquina virtual.

La versión preliminar del cambio tiene las limitaciones siguientes:

[!INCLUDE [virtual-machines-disks-expand-without-downtime-restrictions](../../../includes/virtual-machines-disks-expand-without-downtime-restrictions.md)]

Para registrar la característica, utilice el comando siguiente:

```azurecli
az feature register --namespace Microsoft.Compute --name LiveResize
```

Puede que el proceso de registro tarde unos minutos en completarse. Para confirmar que se ha registrado, use el comando siguiente:

```azurecli
az feature show --namespace Microsoft.Compute --name LiveResize
```

### <a name="get-started"></a>Primeros pasos

Asegúrese de que tiene instalada la versión más reciente de la [CLI de Azure](/cli/azure/install-az-cli2) y de que ha iniciado sesión en una cuenta de Azure con [az login](/cli/azure/reference-index#az_login).

En este artículo se requiere una máquina virtual existente en Azure con al menos un disco de datos adjunto y preparado. Si no dispone de una máquina virtual que pueda usar, consulte la sección sobre la [creación y preparación de máquinas virtuales con discos de datos](tutorial-manage-disks.md#create-and-attach-disks).

En los ejemplos siguientes, reemplace los nombres de parámetros de ejemplo, como *myResourceGroup* y *myVM*, con sus propios valores.

> [!IMPORTANT]
> Si ha habilitado **LiveResize** y el disco cumple los requisitos de [Cambio de tamaño sin tiempo de inactividad (versión preliminar)](#resize-without-downtime-preview), puede omitir los pasos 1 y 3. 

1. No se pueden realizar operaciones en los discos duros virtuales con la VM en ejecución. Desasigne la máquina virtual con [az vm deallocate](/cli/azure/vm#az_vm_deallocate). En el ejemplo siguiente se desasigna la máquina virtual denominada "*myVM*" en el grupo de recursos *myResourceGroup*:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > Debe desasignar la máquina virtual para expandir el disco duro virtual. Detener la VM con `az vm stop` no libera los recursos de proceso. Para liberar los recursos de proceso, use `az vm deallocate`.

1. Vea la lista de discos administrados de un grupo de recursos con [az disk list](/cli/azure/disk#az_disk_list). En el ejemplo siguiente se muestra una lista de discos administrados del grupo de recursos denominado *myResourceGroup*:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Expanda el disco necesario con [az disk update](/cli/azure/disk#az_disk_update). En el ejemplo siguiente se expande el disco administrado llamado *myDataDisk* a *200* GB:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > Si expande un disco administrado, el tamaño actualizado se redondea al tamaño de disco administrado más próximo. Para consultar una tabla de los tamaños y las capas disponibles para discos administrados, vea [Introducción a Azure Managed Disks: precios y facturación](../managed-disks-overview.md).

1. Inicie la máquina virtual con [az vm start](/cli/azure/vm#az_vm_start). En el ejemplo siguiente se inicia la máquina virtual llamada *myVM* en el grupo de recursos llamado *myResourceGroup*:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```


## <a name="expand-a-disk-partition-and-filesystem"></a>Expansión de una partición de disco y del sistema de archivos
Para usar el disco expandido, expanda la partición y el sistema de archivos subyacentes.

1. SSH en la máquina virtual con las credenciales adecuadas. Puede obtener la dirección IP pública de la VM con [az vm show](/cli/azure/vm#az_vm_show):

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --output tsv
    ```

1. Expanda la partición de disco y el sistema de archivos subyacentes.

    a. Si el disco ya está montado, desmóntelo:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Use `parted` para ver la información de disco y cambiar el tamaño de la partición:

    ```bash
    sudo parted /dev/sdc
    ```

    Consulte la información del diseño de partición existente con `print`. El resultado es similar al ejemplo siguiente, que muestra que el disco subyacente es de 215 GB:

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. Expanda la partición con `resizepart`. Escriba el número de partición, *1*, y un tamaño para la nueva partición:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. Para salir, escriba `quit`.

1. Una vez cambiado el tamaño de la partición, compruebe la coherencia de esta con `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. Cambie el tamaño del sistema de archivos con `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. Monte la partición en la ubicación deseada, por ejemplo, `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

1. Para comprobar que se ha cambiado el tamaño del disco de datos, use `df -h`. La siguiente salida de ejemplo muestra que la unidad de datos, */dev/sdc1*, tiene ahora 200 GB:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Pasos siguientes
* Si necesita almacenamiento adicional, también puede [agregar discos de datos a una VM Linux](add-disk.md). 
* Para más información acerca del cifrado de discos, consulte [Azure Disk Encryption para máquinas virtuales Linux](disk-encryption-overview.md).
