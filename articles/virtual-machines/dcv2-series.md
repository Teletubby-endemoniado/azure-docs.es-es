---
title: 'Serie DCsv2: Azure Virtual Machines'
description: Especificaciones de las máquinas virtuales de la serie DCsv2.
author: mmcrey
ms.service: virtual-machines
ms.subservice: vm-sizes-general
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: jushiman
ms.openlocfilehash: 89714a766dfd4ddc2c952554ab12068121543044
ms.sourcegitcommit: 37cc33d25f2daea40b6158a8a56b08641bca0a43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130070781"
---
# <a name="dcsv2-series"></a>Serie DCsv2

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

Las máquinas virtuales de la serie DCsv2 pueden ayudar a proteger la confidencialidad y la integridad de los datos y del código mientras se están procesando en la nube pública. La serie DCsv2 aprovecha Intel® Software Guard Extensions, que permite a los clientes usar enclaves seguros para la protección.

Estas máquinas cuentan con tecnología de Intel® Xeon E-2288G (Coffee Lake) a 3.7 GHz y tecnología SGX. Con Intel® Turbo Boost Max Technology 3.0, estas máquinas pueden llegar a 5.0 GHz. 

> [!NOTE]
> El hyperthreading está deshabilitado para una posición de seguridad agregada. Los precios se basan en el rendimiento superior de los núcleos físicos frente a los virtuales, así como en las funcionalidades de seguridad únicas de la serie DC.

Entre los casos de uso confidencial de ejemplo se incluyen las bases de datos, las cadenas de bloques, el análisis de datos entre varias partes, la detección de fraudes, la prevención del blanqueo de dinero, el análisis de uso, el análisis de la inteligencia y el aprendizaje automático.

## <a name="configuration"></a>Configuración

[Turbo Boost Max 3.0](https://www.intel.com/content/www/us/en/gaming/resources/turbo-boost.html): compatible (la máquina virtual de inquilinos notificará 3.7 GHz, pero alcanzará las velocidades de Turbo)<br>
[Hyper-Threading](https://www.intel.com/content/www/us/en/gaming/resources/hyper-threading.html): no compatible<br>
[Premium Storage](premium-storage-performance.md): compatible (no compatible con Standard_DC8_v2)<br>
[Almacenamiento en caché Premium Storage](premium-storage-performance.md): se admite<br>
[Migración en vivo](maintenance-and-updates.md): No compatible<br>
[Actualizaciones con conservación de memoria](maintenance-and-updates.md): No compatible<br>
[Compatibilidad con generación de VM](generation-2.md): Generación 2<br>
[Redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md): No compatible<br>
[Discos de sistema operativo efímero](ephemeral-os-disks.md): Compatible <br>

## <a name="technical-specifications"></a>Especificaciones técnicas

| Size             | Núcleos físicos | Memoria: GiB | GiB de almacenamiento temporal (SSD) | Discos de datos máx. | Rendimiento máximo de almacenamiento temporal y en caché: IOPS / MBps (tamaño de caché en GiB) | Nº máx. de NIC / ancho de banda de red esperado (MBps) | Memoria EPC (MiB) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|----------------------------------------------|---------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2000/16                                                                                               | 2   | 28                                      |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4000/32                                                                                               | 2  | 56                                          |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8000/64                                                                                               | 2  | 112                                          |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16000/128                                                                                         | 2   | 168                                         |


## <a name="get-started"></a>Introducción

- Creación de máquinas virtuales DCsv2 mediante [Azure Portal](./linux/quick-create-portal.md) o [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview)
- Las VM de la serie DCsv2 son [VM de 2ª generación](./generation-2.md#creating-a-generation-2-vm) y solo admiten imágenes de `Gen2`.
- Actualmente está disponible en las regiones enumeradas en [Productos de Azure por región](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines&regions=all).

## <a name="more-sizes-and-information"></a>Más tamaños e información

- [Uso general](sizes-general.md)
- [Memoria optimizada](sizes-memory.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)
- [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/)
- [Más información sobre los tipos de disco](./disks-types.md#ultra-disks)

Calculadora de precios: [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/)

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
