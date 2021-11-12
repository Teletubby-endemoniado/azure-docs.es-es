---
title: Series Dv3 y DSv3
description: Especificaciones de las máquinas virtuales de las series Dv3 y DSv3.
author: joelpelley
ms.service: virtual-machines
ms.subservice: vm-sizes-general
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: jushiman
ms.openlocfilehash: e6439acb72bffabf117387182ee6eb08f1041873
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131437284"
---
# <a name="dv3-and-dsv3-series"></a>Series Dv3 y DSv3

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

La serie Dv3 se ejecuta en procesadores Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2,1 GHz (Skylake), Intel® Xeon® E5-2673 v4 2,3 GHz (Broadwell) o Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell) en una configuración de Hyper-Threading, lo que proporciona una mejor propuesta de valor para la mayoría de las cargas de trabajo de uso general. Se ha ampliado la memoria (de ~3,5 GiB/vCPU a 4 GiB/vCPU), y los límites de disco y red se han ajustado por núcleo para equipararse con el cambio a hyperthreading. La serie Dv3 ya no tiene los tamaños de máquina virtual de memoria alta de la serie D/Dv2, que se han pasado a las series [Ev3 y Esv3](ev3-esv3-series.md) optimizadas para memoria.

Algunos casos de uso de la serie D son las aplicaciones empresariales, las bases de datos relacionales, el almacenamiento en caché en memoria y el análisis.

## <a name="dv3-series"></a>Serie Dv3

Los tamaños de la serie Dv3 se ejecutan en procesadores Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2,1 GHz (Skylake), Intel® Xeon® E5-2673 v4 2,3 GHz (Broadwell) o Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell) con la [tecnología Intel&reg; Turbo Boost 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html). Los tamaños de la serie Dv3 ofrecen una combinación de vCPU, memoria y almacenamiento local adecuados para la mayoría de las cargas de trabajo de producción.

El almacenamiento en disco de datos se factura de forma independiente a las máquinas virtuales. Para usar discos de Premium Storage, utilice los tamaños Dsv3. El precio y los medidores de facturación para los tamaños Dsv3 son los mismos que para la serie Dv3.

Las máquinas virtuales de la serie Dv3 cuentan con la tecnología Hyper-Threading de Intel®.

[ACU](acu.md): 160-190<br>
[Premium Storage](premium-storage-performance.md): No compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): No compatible<br>
[Migración en vivo](maintenance-and-updates.md): Compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): Compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): No compatible <br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal: IOPS/Mbps de lectura/Mbps de escritura | N.º máx. de NIC/ ancho de banda de red esperado |
|---|---|---|---|---|---|---|
| Standard_D2_v3<sup>1</sup>  | 2  | 8   | 50   | 4  | 3000/46/23     | 2/1000  |
| Standard_D4_v3  | 4  | 16  | 100  | 8  | 6000/93/46     | 2/2000  |
| Standard_D8_v3  | 8  | 32  | 200  | 16 | 12000/187/93   | 4/4000  |
| Standard_D16_v3 | 16 | 64  | 400  | 32 | 24000/375/187  | 8/8000  |
| Standard_D32_v3 | 32 | 128 | 800  | 32 | 48000/750/375  | 8/16000 |
| Standard_D48_v3 | 48 | 192 | 1200 | 32 | 96000/1000/500 | 8/24000 |
| Standard_D64_v3 | 64 | 256 | 1600 | 32 | 96000/1000/500 | 8/30000 |

<sup>1</sup> Las redes aceleradas solo se pueden aplicarse a una única NIC. 

## <a name="dsv3-series"></a>Serie Dsv3

Los tamaños de la serie Dsv3 se ejecutan en procesadores Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2,1 GHz (Skylake), Intel® Xeon® E5-2673 v4 2,3 GHz (Broadwell) o Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell) con la [tecnología Intel&reg; Turbo Boost 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html) y usan Premium Storage. Los tamaños de la serie Dsv3 ofrecen una combinación de vCPU, memoria y almacenamiento local adecuados para la mayoría de las cargas de trabajo de producción.

Las máquinas virtuales de la serie DSv3 cuentan con la tecnología Hyper-Threading de Intel®.

[ACU](acu.md): 160-190<br>
[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Migración en vivo](maintenance-and-updates.md): Compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): Compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1 y 2<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): Compatible <br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS/Mbps (tamaño de caché en GiB) | Rendimiento máximo de almacenamiento en caché y almacenamiento temporal expandidos: IOPS/MBps<sup>2</sup> | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS/Mbps | Rendimiento máximo del disco sin almacenamiento en la caché expandido: IOPS/MBps<sup>1</sup> | N.º máx. de NIC/ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|---|---|
| Standard_D2s_v3<sup>2</sup>  | 2  | 8   | 16  | 4  | 4000/32 (50)       | 4000/100    |3200/48    | 4000/100   | 2/1000  |
| Standard_D4s_v3  | 4  | 16  | 32  | 8  | 8000/64 (100)      | 8000/200    |6400/96    | 8000/200   | 2/2000  |
| Standard_D8s_v3  | 8  | 32  | 64  | 16 | 16000/128 (200)    | 16 000/400   |12800/192  | 16 000/400  | 4/4000  |
| Standard_D16s_v3 | 16 | 64  | 128 | 32 | 32000/256 (400)    | 32 000/800   |25600/384  | 32 000/800  | 8/8000  |
| Standard_D32s_v3 | 32 | 128 | 256 | 32 | 64000/512 (800)    | 64 000/1600  |51200/768  | 64 000/1600 | 8/16000 |
| Standard_D48s_v3 | 48 | 192 | 384 | 32 | 96000/768 (1200)   | 96 000/2000  |76800/1152 | 80000/2000 | 8/24000 |
| Standard_D64s_v3 | 64 | 256 | 512 | 32 | 128000/1024 (1600) | 128 000/2000 |80000/1200 | 80000/2000 | 8/30000 |

<sup>1</sup> Las máquinas virtuales de la serie Dsv3 pueden [expandir](./disk-bursting.md) su rendimiento de disco y llegar a una expansión máxima de hasta 30 minutos cada vez.<br>
<sup>2</sup> Las redes aceleradas solo se pueden aplicar a una única NIC. 


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Otros tamaños e información

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

Calculadora de precios: [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/)

Para obtener más información sobre los tipos de discos, vea [¿Qué tipos de disco están disponibles en Azure?](disks-types.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
