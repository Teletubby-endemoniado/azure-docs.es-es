---
title: 'Serie M: Azure Virtual Machines'
description: Especificaciones de las máquinas virtuales de la serie M.
author: ayshakeen
ms.service: virtual-machines
ms.subservice: vm-sizes-memory
ms.topic: conceptual
ms.date: 03/31/2020
ms.author: jushiman
ms.openlocfilehash: 97691d3b9c2d94a42e3ff434a5bbaef1bd281975
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122698840"
---
# <a name="m-series"></a>Serie M

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

La serie M ofrece un elevado recuento de vCPU (hasta 128 vCPU) y una gran cantidad de memoria (hasta 3,8 TiB). También es muy conveniente para bases de datos extremadamente grandes u otras aplicaciones que se benefician de un elevado número de vCPU y grandes cantidades de memoria. Los tamaños de la serie M se admiten tanto en la CPU Intel&reg; Xeon&reg; E7-8890 v3 a 2,50 GHz como en Intel&reg; Xeon&reg; Platinum 8280M (Cascade Lake).

Las máquinas virtuales de la serie M cuentan con la tecnología Hyper-Threading de Intel&reg;.

[ACU](acu.md): 160-180<br>
[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Migración en vivo](maintenance-and-updates.md): No compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): No compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1 y 2<br>
[Acelerador de escritura](./how-to-enable-write-accelerator.md): Compatible<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): Compatible <br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS/Mbps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS/Mbps | Nº máx. NIC|Ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|---|
| Standard_M8ms <sup>3</sup>       | 8   | 218,75 | 256   | 8  | 10000/100 (793)     | 5000/125   | 4|2000  |
| Standard_M16ms <sup>3</sup>      | 16  | 437,5  | 512   | 16 | 20000/200 (1587)    | 10000/250  | 8|4000  |
| Standard_M32ts                   | 32  | 192    | 1024  | 32 | 40000/400 (3174)    | 20000/500  | 8|8000  |
| Standard_M32ls                   | 32  | 256    | 1024  | 32 | 40000/400 (3174)    | 20000/500  | 8|8000  |
| Standard_M32ms <sup>3</sup>      | 32  | 875    | 1024  | 32 | 40000/400 (3174)    | 20000/500  | 8|8000  |
| Standard_M64s <sup>1</sup>       | 64  | 1024   | 2048  | 64 | 80000/800 (6348)    | 40000/1000 | 8|16000 |
| Standard_M64ls <sup>1</sup>      | 64  | 512    | 2048  | 64 | 80000/800 (6348)    | 40000/1000 | 8|16000 |
| Standard_M64ms <sup>1,3</sup>    | 64  | 1792   | 2048  | 64 | 80000/800 (6348)    | 40000/1000 | 8|16000 |
| Standard_M128s <sup>1</sup>    | 128 | 2048   | 4096  | 64 | 160000/1600 (12696) | 80000/2000 | 8|30000 |
| Standard_M128ms <sup>1,2,3</sup> | 128 | 3892   | 4096  | 64 | 160000/1600 (12696) | 80000/2000 | 8|30000 |
| Standard_M64 <sup>1</sup>        | 64  | 1024   | 7168  | 64 | 80000/800 (1228)    | 40000/1000 | 8|16000 |
| Standard_M64m <sup>1</sup>       | 64  | 1792   | 7168  | 64 | 80000/800 (1228)    | 40000/1000 | 8|16000 |
| Standard_M128 <sup>1</sup>     | 128 | 2048   | 14336 | 64 | 250000/1600 (2456)  | 80000/2000 | 8|32000 |
| Standard_M128m <sup>1</sup>    | 128 | 3892   | 14336 | 64 | 250000/1600 (2456)  | 80000/2000 | 8|32000 |

<sup>1</sup> Si hay más de 64 vCPU, se necesitará una de estas versiones invitadas compatibles: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 y Red Hat Enterprise Linux, CentOS 7.3 u Oracle Linux 7.3 con LIS 4.2.1.

<sup>2</sup> La instancia está aislada en el hardware dedicado a un solo cliente.

<sup>3</sup> [Tamaños de núcleos restringidos disponibles](./constrained-vcpu.md).

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Otros tamaños e información

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

Calculadora de precios: [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/)

Más información sobre los tipos de disco: [Tipos de disco](./disks-types.md#ultra-disk)


## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
