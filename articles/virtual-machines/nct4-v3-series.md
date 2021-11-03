---
title: Serie NCas T4 v3
description: Especificaciones de las VM de la serie NCas T4 v3.
ms.service: virtual-machines
ms.subservice: vm-sizes-gpu
author: vikancha-MSFT
ms.topic: conceptual
ms.date: 01/12/2021
ms.author: vikancha
ms.openlocfilehash: 8a93d84fea79935f96498eff7d391271e6f4c7ef
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131013106"
---
# <a name="ncast4_v3-series"></a>Serie NCasT4_v3 

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

Las máquinas virtuales de la serie NCas T4_v3 usan la tecnología de las GPU [Nvidia Tesla T4](https://www.nvidia.com/en-us/data-center/tesla-t4/) y las CPU AMD EPYC 7V12 (Rome). Las máquinas virtuales cuentan con hasta 4 GPU NVIDIA T4 con 16 GB de memoria cada una, hasta 64 núcleos de procesador AMD EPYC 7V12 (Rome) sin multiproceso (frecuencia base de 2,45 GHz, frecuencia máxima de todos los núcleos de 3,1 GHz y frecuencia máxima de un solo núcleo de 3,3 GHz) y 440 GiB de memoria del sistema. Estas máquinas virtuales son idóneas para implementar servicios de inteligencia artificial, como la inferencia en tiempo real de solicitudes generadas por el usuario, o bien para cargas de trabajo de visualización y gráficos interactivos mediante el controlador GRID de NVIDIA y tecnología de GPU virtual. Las cargas de trabajo de proceso de GPU estándar basadas en CUDA, TensorRT, Caffe, ONNX y otros marcos, o las aplicaciones gráficas con aceleración por GPU basadas en OpenGL y DirectX, se pueden implementar de forma económica, cerca de los usuarios, en la serie NCasT4_v3.

<br>

[ACU](acu.md): 230-260<br>
[Premium Storage](premium-storage-performance.md): Compatible<br>
[Almacenamiento en caché de Premium Storage](premium-storage-performance.md): Compatible<br>
[Ultra Disks](disks-types.md#ultra-disks): Compatible ([más información](https://techcommunity.microsoft.com/t5/azure-compute/ultra-disk-storage-for-hpc-and-gpu-vms/ba-p/2189312) sobre la disponibilidad, el uso y el rendimiento) <br>
[Migración en vivo](maintenance-and-updates.md): No compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): No compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 1 y 2<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): Compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): Compatible<br>
Interconexión de NVIDIA NVLink: No compatible<br>
<br>

| Size | vCPU | Memoria: GiB | GiB de almacenamiento temporal (SSD) | GPU | Memoria de GPU: GiB | Discos de datos máx. | Nº máx. de NIC/ancho de banda de red esperado (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC4as_T4_v3 |4 |28 |180 | 1 | 16 | 8 | 2 / 8000 |
| Standard_NC8as_T4_v3 |8 |56 |360 | 1 | 16 | 16 | 4 / 8000  |
| Standard_NC16as_T4_v3 |16 |110 |360 | 1 | 16 | 32 | 8 / 8000  |
| Standard_NC64as_T4_v3 |64 |440 |2880 | 4 | 64 | 32 | 8 / 32 000  |




## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

Para aprovechar las funcionalidades de GPU de las VM de la serie NCasT4_v3 de Azure que ejecutan Windows o Linux, deben instalarse controladores de GPU de NVIDIA.

Para instalar manualmente los controladores de GPU de NVIDIA, consulte [Instalación de controladores de GPU de la serie N para Windows](./windows/n-series-driver-setup.md) para obtener información sobre los sistemas operativos y controladores compatibles, así como sobre los pasos de instalación y verificación.

La extensión del controlador de GPU de NVIDIA de Azure implementará controladores CUDA en las máquinas virtuales de la serie NCasT4_v3. En el caso de las cargas de trabajo de visualización y gráficos, instale manualmente los controladores GRID compatibles con Azure.

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
