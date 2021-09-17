---
title: 'Serie Mv2: Azure Virtual Machines'
description: Especificaciones de las máquinas virtuales de la serie Mv2.
author: ayshakeen
ms.service: virtual-machines
ms.subservice: vm-sizes-memory
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: jushiman
ms.openlocfilehash: 75a9a8f9888c0e3773135aa407938601b74bab0e
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122690275"
---
# <a name="mv2-series"></a>Serie Mv2

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

La serie Mv2 ofrece una plataforma de alto rendimiento y baja latencia que se ejecuta en un procesador Intel® Xeon® Platinum 8180M de 2,5 GHz (Skylake) con tecnología Hyper-Threading con una frecuencia básica de 2,5 GHz y una frecuencia turbo máxima de 3,8 GHz. Todos los tamaños de máquina virtual de la serie Mv2 pueden usar discos persistentes estándar y premium. Las instancias de la serie Mv2 son tamaños de máquinas virtuales optimizados para memoria que proporcionan un rendimiento de proceso sin precedentes para admitir grandes bases de datos y cargas de trabajo en memoria, con una relación elevada de memoria y CPU que es perfecta para servidores de bases de datos relacionales, grandes almacenamientos en caché y análisis en memoria.

Las máquinas virtuales de la serie Mv2 cuentan con la tecnología Hyper-Threading de Intel®.

[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Migración en vivo](maintenance-and-updates.md): No compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): No compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 2<br>
[Acelerador de escritura](./how-to-enable-write-accelerator.md): Compatible<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): No compatible <br>
<br>

|Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Rendimiento máximo del disco sin almacenamiento en la caché: IOPS / MBps | Nº máx. NIC | Ancho de banda de red esperado (Mbps) |
|---|---|---|---|---|---|---|---|---|
| Standard_M208ms_v2<sup>1</sup> | 208 | 5700 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 | 16000 |
| Standard_M208s_v2<sup>1</sup> | 208 | 2850 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 | 16000 |
| Standard_M416ms_v2<sup>1,2</sup> | 416 | 11 400 | 8192 | 64 | 250 000 / 1600 (14 080) | 80000 / 2000 | 8 | 32000 |
| Standard_M416s_v2<sup>1,2</sup> | 416 | 5700 | 8192 | 64 | 250 000 / 1600 (14 080) | 80000 / 2000 | 8 | 32000 |

<sup>1</sup> Las máquinas virtuales de la serie Mv2 solo son de generación 2 y admiten un subconjunto de imágenes admitidas de la generación 2. A continuación puede encontrar la lista completa de imágenes admitidas de la serie Mv2. Si usa Linux, consulte [Compatibilidad para máquinas virtuales de generación 2 en Azure](./generation-2.md) para obtener instrucciones sobre cómo buscar y seleccionar una imagen. Si usa Windows, consulte [Compatibilidad para máquinas virtuales de generación 2 en Azure](./generation-2.md) para obtener instrucciones sobre cómo buscar y seleccionar una imagen. 

- Windows Server 2019 o posterior
- SUSE Linux Enterprise Server 12 SP4 y las versiones posteriores o SUSE Linux Enterprise Server 15 SP1 y las versiones posteriores.
- Red Hat Enterprise Linux 7.6, 7.7, 8.1 o posterior 
- Oracle Enterprise Linux 7.7 o posterior

<sup>2</sup> [Tamaños de núcleos restringidos disponibles](./constrained-vcpu.md).


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
