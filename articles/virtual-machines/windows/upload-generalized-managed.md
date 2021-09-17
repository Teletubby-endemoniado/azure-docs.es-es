---
title: Creación de una máquina virtual a partir de un disco duro virtual de Windows generalizado cargado
description: Cargue un VHD de Windows generalizado en Azure y úselo para crear máquinas virtuales nuevas en el modelo de implementación de Resource Manager.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 12/12/2019
ms.author: cynthn
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 4f31fff34dcaaa8cc30b1a894e24f3eda8e5764d
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122688590"
---
# <a name="upload-a-generalized-windows-vhd-and-use-it-to-create-new-vms-in-azure"></a>Carga de un VHD de Windows generalizado y su uso para crear máquinas virtuales nuevas en Azure

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles 

En este artículo se explica cómo usar PowerShell para cargar un VHD de una máquina virtual generalizada en Azure, crear una imagen a partir del VHD y crear una máquina virtual nueva desde esa imagen. Puede cargar un VHD exportado de una herramienta de visualización local o desde otra nube. Usar [Managed Disks](../managed-disks-overview.md) para la nueva VM simplifica la administración de la VM y proporciona una mejor disponibilidad cuando la VM se encuentra en un conjunto de disponibilidad. 

Para un script de ejemplo, consulte [Script de ejemplo para cargar un disco duro virtual en Azure y crear una máquina virtual nueva](/previous-versions/azure/virtual-machines/scripts/virtual-machines-windows-powershell-upload-generalized-script).

## <a name="before-you-begin"></a>Antes de empezar

- Antes de cargar los VHD en Azure, debe consultar [Preparación de un VHD o un VHDX de Windows antes de cargarlo en Azure](prepare-for-upload-vhd-image.md).
- Revise [Plan for the migration to Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) (Planeación de la migración a Managed Disks) antes de comenzar la migración a [Managed Disks](../managed-disks-overview.md).

 
## <a name="generalize-the-source-vm-by-using-sysprep"></a>Generalización de la VM de origen mediante Sysprep

Si aún no lo ha hecho, debe usar Sysprep en la máquina virtual antes de cargar el disco duro virtual en Azure. Entre otras características, Sysprep elimina toda la información personal de la cuenta y prepara, entre otras cosas, la máquina para usarse como imagen. Para más información acerca de Sysprep, consulte la [Introducción a Sysprep](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

Asegúrese de que los roles de servidor que se ejecutan en la máquina sean compatibles con Sysprep. Para más información, consulte [Sysprep Support for Server Roles](/windows-hardware/manufacture/desktop/sysprep-support-for-server-roles)(Compatibilidad de Sysprep con roles de servidor).

> [!IMPORTANT]
> Si tiene pensado ejecutar Sysprep antes de cargar el VHD en Azure por primera vez, asegúrese de que tiene [preparada la máquina virtual](prepare-for-upload-vhd-image.md). 
> 
> 

1. Inicie sesión en la máquina virtual de Windows.
1. Abra una ventana del símbolo del sistema como administrador. 
1. Elimine el directorio de Panther (C:\Windows\Panther).
1. Cambie el directorio a %windir%\system32\sysprep, y, después, ejecute `sysprep.exe`.
1. En **Herramienta de preparación del sistema**, seleccione **Iniciar la Configuración rápida (OOBE)** y asegúrese de que la casilla **Generalizar** está seleccionada.
1. En **Opciones de apagado**, seleccione **Apagar**.
1. Seleccione **Aceptar**.
   
    ![Iniciar Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
1. Cuando Sysprep finaliza, apaga la máquina virtual. No reinicie la VM.


## <a name="upload-the-vhd"></a>Carga del disco duro virtual 

Ahora puede cargar un disco duro virtual directamente en un disco administrado. Para obtener instrucciones al respecto, consulte [Carga de un disco duro virtual en Azure mediante Azure PowerShell](disks-upload-vhd-to-managed-disk-powershell.md).



Una vez que el disco duro virtual se carga en el disco administrado, debe usar [Get-AzDisk](/powershell/module/az.compute/get-azdisk) para obtener el disco administrado.

```azurepowershell-interactive
$disk = Get-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'myDiskName'
```

## <a name="create-the-image"></a>Crear la imagen
Cree una imagen administrada desde el disco administrado del SO generalizado. Reemplace los valores siguientes por su propia información.

En primer lugar, establezca algunas variables:

```powershell
$location = 'East US'
$imageName = 'myImage'
$rgName = 'myResourceGroup'
```

Cree la imagen mediante el disco administrado.

```azurepowershell-interactive
$imageConfig = New-AzImageConfig `
   -Location $location
$imageConfig = Set-AzImageOsDisk `
   -Image $imageConfig `
   -OsState Generalized `
   -OsType Windows `
   -ManagedDiskId $disk.Id
```

Cree la imagen.

```azurepowershell-interactive
$image = New-AzImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```

## <a name="create-the-vm"></a>Creación de la máquina virtual

Ahora que tiene una imagen, puede crear una o varias VM desde la imagen. En este ejemplo se crea una máquina virtual denominada *myVM* a partir de *myImage*, en *myResourceGroup*.


```powershell
New-AzVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Image $image.Id `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNSG" `
    -PublicIpAddressName "myPIP" `
    -OpenPorts 3389
```


## <a name="next-steps"></a>Pasos siguientes

Inicie sesión en la nueva máquina virtual. Para más información, consulte [Conexión a una máquina virtual de Azure donde se ejecuta Windows Server e inicio de sesión en ella](connect-logon.md).
