---
title: 'Serie NV: Azure Virtual Machines'
description: Especificaciones de las máquinas virtuales de la serie NV.
author: vikancha-MSFT
ms.service: virtual-machines
ms.subservice: vm-sizes-gpu
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: a51801c1eafa5c3992fc0fd4fe33e55bdf8d9387
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122696037"
---
# <a name="nv-series"></a>Serie NV

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

Las máquinas virtuales de la serie NV dispone de tarjetas GPU [Tesla M60 de NVIDIA](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) y tecnología NVIDIA GRID para aplicaciones aceleradas de escritorio y escritorios virtuales donde los clientes pueden visualizar sus datos o simulaciones. Los usuarios pueden visualizar sus flujos de trabajo con muchos gráficos en las instancias de NV para obtener una excelente funcionalidad gráfica y ejecutar, además, cargas de trabajo de precisión únicas, como la codificación y la representación. Las máquinas virtuales de la serie NV también funcionan con CPU Intel Xeon E5-2690 v3 (Haswell).

Cada GPU de instancias de NV viene con una licencia de GRID. Esta licencia le ofrece flexibilidad para utilizar una instancia de NV como estación de trabajo virtual para un solo usuario; también se pueden conectar 25 usuarios simultáneos a la VM para un escenario de aplicación virtual.

[Premium Storage](premium-storage-performance.md): No compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): No compatible<br>
[Migración en vivo](maintenance-and-updates.md): No compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): No compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): No compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): No compatible <br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. NIC | Estaciones de trabajo virtuales | Aplicaciones virtuales |
|---|---|---|---|---|---|---|---|---|---|
| Standard_NV6  | 6  | 56  | 340  | 1 | 8  | 24 | 1 | 1 | 25  |
| Standard_NV12 | 12 | 112 | 680  | 2 | 16 | 48 | 2 | 2 | 50  |
| Standard_NV24 | 24 | 224 | 1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = media tarjeta M60.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

Para aprovechar las funcionalidades de GPU de las máquinas virtuales de la serie N de Azure, deben instalarse controladores de GPU de NVIDIA.

La [extensión de controlador de GPU de NVIDIA](./extensions/hpccompute-gpu-windows.md) instala los controladores CUDA de NVIDIA o GRID adecuados en una máquina virtual de la serie N. Instale o administre la extensión mediante Azure Portal o con herramientas como las plantillas de Azure PowerShell o Azure Resource Manager. Consulte la [documentación de la extensión de controlador de GPU de NVIDIA](./extensions/hpccompute-gpu-windows.md) para los sistemas operativos compatibles y los pasos de implementación. Para una información general sobre las extensiones de máquina virtual, consulte [Características y extensiones de las máquinas virtuales de Azure](./extensions/overview.md).

Si decide instalar manualmente los controladores de GPU de NVIDIA, consulte el artículo sobre la [instalación de controladores GPU de la serie N para Windows](./windows/n-series-driver-setup.md) o el artículo sobre la [instalación de controladores GPU de la serie N para Linux](./linux/n-series-driver-setup.md) para obtener información sobre los sistemas operativos compatibles, los controladores, la instalación y los pasos de verificación.

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
