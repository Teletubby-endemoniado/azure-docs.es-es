---
title: 'Tamaños de las máquinas virtuales de Azure: GPU | Microsoft Docs'
description: Enumera los tamaños diferentes optimizados para GPU disponibles para las máquinas virtuales en Azure. Se proporciona información sobre el número de unidades vCPU, discos de datos y NIC, así como sobre el rendimiento de almacenamiento y el ancho de banda de red para los tamaños de esta serie.
author: vikancha-MSFT
ms.service: virtual-machines
ms.subservice: vm-sizes-gpu
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 8a39809cf787237a38d5246c32e481fb82ac2629
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131474560"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Tamaños de máquinas virtuales optimizadas para GPU

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

> [!TIP]
> Pruebe la **[herramienta Selector de máquinas virtuales](https://aka.ms/vm-selector)** para buscar otros tamaños que se adapten mejor a la carga de trabajo.

Los tamaños de máquina virtual optimizada para GPU son máquinas virtuales especializadas con GPU individuales, múltiples o fraccionarias. Estos tamaños están diseñados para cargas de trabajo de proceso intensivo, uso intensivo de gráficos y visualización. En este artículo se proporciona información sobre el número y el tipo de GPU, vCPU, discos de datos y NIC. El ancho de banda de red y el rendimiento del almacenamiento también se incluyen para cada tamaño de esta agrupación.

- Los tamaños de las series [NCv3](ncv3-series.md) y [NC T4_v3](nct4-v3-series.md) se han optimizado en las aplicaciones de procesos intensivos con aceleración de GPU. Algunos ejemplos son las simulaciones y las aplicaciones basadas en CUDA y en OpenCL, la inteligencia artificial y el aprendizaje profundo. La serie NC T4 v3 se centra en las cargas de trabajo de inferencia que incluyen la GPU NVIDIA Tesla T4 y el procesador AMD EPYC2 Rome. La serie NCv3 se centra en las cargas de trabajo de procesos e IA de alto rendimiento e incorpora la GPU de NVIDIA Tesla V100.

- El tamaño de la serie [ND A100 v4](nda100-v4-series.md) va dirigido al entrenamiento de aprendizaje profundo de escalabilidad vertical y horizontal y a las aplicaciones HPC aceleradas. La serie ND A100 v4 emplea 8 GPU NVIDIA A100 TensorCore, cada una con una conexión HDR Gigabit Mellanox InfiniBand de 200 Gigabits y 40 GB de memoria de GPU.

- Los tamaños de las series [NV](nv-series.md) y [NVv3](nvv3-series.md) están optimizados y diseñados para escenarios de visualización remota, streaming, juegos, codificación y VDI mediante marcos como OpenGL y DirectX. Estas máquinas virtuales están respaldadas por la GPU NVIDIA Tesla M60.

- Los tamaños de las máquinas virtuales de la serie [NVv4](nvv4-series.md) están optimizados y diseñados para VDI y la visualización remota. Con las GPU con particiones, NVv4 ofrece el tamaño adecuado para las cargas de trabajo que requieren recursos de GPU más pequeños. Estas máquinas virtuales usan la GPU AMD Radeon Instinct MI25. Las máquinas virtuales NVv4 actualmente solo admiten el sistema operativo invitado Windows.

- La máquina virtual de la [serie NDm A100 v4](ndm-a100-v4-series.md) es una nueva incorporación insignia a la familia de GPU de Azure, diseñada para el entrenamiento de aprendizaje profundo de alto nivel y para cargas de trabajo de HPC de escalabilidad vertical y horizontal estrechamente acopladas. La serie NDm A100 v4 comienza con una sola máquina virtual (VM) y ocho GPU NVIDIA Ampere A100 Tensor Core de 80 GB.


## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos y controladores compatibles

Para aprovechar las funcionalidades de GPU de las máquinas virtuales de la serie N de Azure, deben instalarse controladores de GPU de AMD o NVIDIA.

- Para las máquinas virtuales en las que las GPU de NVIDIA realizan la copia de seguridad, la [extensión de controlador de GPU de NVIDIA](./extensions/hpccompute-gpu-windows.md) instala los controladores CUDA de NVIDIA o GRID adecuados. Instale o administre la extensión mediante Azure Portal o con herramientas como las plantillas de Azure PowerShell o Azure Resource Manager. Consulte la [documentación de la extensión de controlador de GPU de NVIDIA](./extensions/hpccompute-gpu-windows.md) para los sistemas operativos compatibles y los pasos de implementación. Para una información general sobre las extensiones de máquina virtual, consulte [Características y extensiones de las máquinas virtuales de Azure](./extensions/overview.md).

   Como alternativa, puede instalar controladores de GPU de NVIDIA manualmente. Consulte [Instalación de controladores de GPU de NVIDIA en VM de la serie N con Windows](./windows/n-series-driver-setup.md) o [Instalación de controladores de GPU de NVIDIA en máquinas virtuales de la serie N con Linux](./linux/n-series-driver-setup.md) para obtener información sobre los sistemas operativos compatibles, los controladores, la instalación y los pasos de comprobación.

- En el caso de las máquinas virtuales respaldadas por GPU de AMD, la [extensión de controlador de GPU de AMD](./extensions/hpccompute-amd-gpu-windows.md) instala los controladores de AMD adecuados. Instale o administre la extensión mediante Azure Portal o con herramientas como las plantillas de Azure PowerShell o Azure Resource Manager. Para una información general sobre las extensiones de máquina virtual, consulte [Características y extensiones de las máquinas virtuales de Azure](./extensions/overview.md).

   Como alternativa, puede instalar controladores de GPU de AMD manualmente. Para conocer los sistemas operativos y los controladores admitidos, así como los pasos de instalación y comprobación, consulte [Instalación de controladores de GPU de AMD en máquinas virtuales de la serie N con Windows](./windows/n-series-amd-driver-setup.md).

## <a name="deployment-considerations"></a>Consideraciones de la implementación

- Para ver la disponibilidad de máquinas virtuales de la serie N, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services/).

- Las máquinas virtuales de la serie N solo se pueden implementar en el modelo de implementación de Resource Manager.

- Las máquinas virtuales de serie N difieren en el tipo de Azure Storage que admiten en sus discos. Las máquinas virtuales NC y NV solo admiten discos de máquina virtual respaldados por Disk Storage (HDD) estándar. Todas las demás máquinas virtuales de GPU admiten discos de máquina virtual respaldados por Disk Storage Estándar y Disk Storage Prémium (SSD).

- Si desea implementar más de un pequeño número de máquinas virtuales de la serie N, considere la posibilidad de usar una suscripción de pago por uso u otras opciones de compra. Si usa una [cuenta gratuita de Azure](https://azure.microsoft.com/free/), solo puede usar un número limitado de núcleos de proceso de Azure.

- Es posible que necesite aumentar la cuota de núcleos (por región) de la suscripción de Azure y la cuota independiente para los núcleos NC, NCv2, NCv3, ND, NDv2, NV o NVv2. Para solicitar un aumento de cuota, [abra una solicitud de soporte técnico al cliente en línea](../azure-portal/supportability/how-to-create-azure-support-request.md) sin cargo alguno. Los límites predeterminados pueden variar según la categoría de suscripción.

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Proceso optimizado](sizes-compute.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
