---
title: 'Tamaños de las máquinas virtuales de Azure: memoria | Microsoft Docs'
description: Enumera los tamaños diferentes optimizados para memoria disponibles para las máquinas virtuales en Azure. Se proporciona información sobre el número de unidades vCPU, discos de datos y NIC, así como sobre el rendimiento de almacenamiento y el ancho de banda de red para los tamaños de esta serie.
services: virtual-machines
documentationcenter: ''
author: brbell
tags: azure-resource-manager,azure-service-management
keywords: Aislamiento de máquina virtual, máquina virtual aislada, aislamiento, aislada
ms.assetid: ''
ms.service: virtual-machines
ms.subservice: vm-sizes-memory
ms.devlang: na
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 10/20/2021
ms.author: brbell
ms.openlocfilehash: 50a187435b1a47fe2559ce5ce2d36905570b8419
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131456601"
---
# <a name="memory-optimized-virtual-machine-sizes"></a>Tamaños de máquina virtual optimizada para memoria

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

> [!TIP]
> Pruebe la **[herramienta Selector de máquinas virtuales](https://aka.ms/vm-selector)** para buscar otros tamaños que se adapten mejor a la carga de trabajo.

Los tamaños de máquinas virtuales optimizadas para memoria ofrecen una relación alta de memoria a CPU, que es excelente para servidores de bases de datos relacionales, memorias caché de medianas a grandes y análisis en memoria. En este artículo, se proporciona información acerca del número de vCPU, discos de datos y tarjetas de interfaz de red, así como del rendimiento del almacenamiento y del ancho de banda de red para cada tamaño de esta agrupación.

- Las [series de Dv2 y DSv2](dv2-dsv2-series-memory.md), una evolución de la serie D original, presentan una CPU más eficaz. La serie Dv2 es un 35 % aproximadamente más rápida que la serie D. Se ejecuta en procesadores Intel&reg; Xeon&reg; 8171M de 2,1 GHz (Skylake), Intel&reg; Xeon&reg; E5-2673 v4 de 2,3 GHz (Broadwell) o Intel&reg; Xeon&reg; E5-2673 v3 de 2,4 GHz (Haswell) y con la tecnología Intel Turbo Boost Technology 2.0. La serie Dv2 tiene las mismas configuraciones de disco y memoria que la serie D.

    Las series Dv2 y DSv2 son ideales para las aplicaciones que requieren vCPU más rápidas o mejor rendimiento del almacenamiento temporal, o tienen mayor demanda de memoria. Ofrecen una combinación eficaz para muchas aplicaciones de clase empresarial.

- Las [series Eav4 y Easv4](eav4-easv4-series.md) utilizan el procesador EPYC<sup>TM</sup> 7452 de 2,35 GHz de AMD en una configuración de varios subprocesos con una caché L3 de hasta 256 MB, lo que aumenta las opciones para ejecutar la mayoría de las cargas de trabajo optimizadas para memoria. Las series Eav4 y Easv4 tienen las mismas configuraciones de memoria y disco que las series Ev3 y Esv3.

- Las [series Ev3 y Esv3](ev3-esv3-series.md) tienen el procesador Intel&reg; Xeon&reg; 8171M de 2,1 GHz (Skylake) o el procesador Intel&reg; Xeon&reg; E5-2673 v4 de 2,3 GHz (Broadwell) en una configuración de Hyper-Threading. Gracias a esto, proporcionan una mejor propuesta de valor para la mayoría de las cargas de trabajo de uso general y la serie Ev3 se equipara con las máquinas virtuales de uso general de la mayoría de las demás nubes. Se ha ampliado la memoria (de 7 GiB/vCPU a 8 GiB/vCPU), y los límites de disco y red se han ajustado por núcleo para equipararse con el cambio a Hyper-Threading. La serie Ev3 es la continuación de los tamaños de máquina virtual de memoria alta de las familias D/Dv2.

- Las [series Ev4 y Esv4](ev4-esv4-series.md) se ejecutan en los procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake) de segunda generación en una configuración con Hyper-Threading y resultan muy convenientes para las diversas aplicaciones empresariales de uso intensivo de memoria y tienen hasta 504 GiB de RAM. Presenta las tecnologías [Intel&reg; Turbo Boost 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), [Intel&reg; Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) e [Intel&reg; Advanced Vector Extensions 512 (Intel AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html). Las series Ev4 y Esv4 no incluyen ningún disco temporal local. Para más información, consulte [Tamaños de máquina virtual de Azure sin disco temporal local](azure-vms-no-temp-disk.yml).

- Las [series Edv4 y Edsv4](edv4-edsv4-series.md) se ejecutan en los procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Cascade Lake) de segunda generación, y resultan muy convenientes para bases de datos muy grandes u otras aplicaciones que se benefician de recuentos muy elevados de vCPU y grandes cantidades de memoria. Además, estos tamaños de máquina virtual incluyen el rápido almacenamiento local en SSD, que es mayor, para las aplicaciones que se benefician del almacenamiento local de alta velocidad y baja latencia. Presenta una velocidad de reloj turbo de todos los núcleos de 3,4 GHz, las tecnologías [Intel&reg; Turbo Boost 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), [Intel&reg; Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) y las extensiones de vector avanzadas 512 de [Intel&reg; (Intel AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html).

- En las [series Easv5 y Eadsv5](easv5-eadsv5-series.md) se usa el procesador de tercera generación EPYC<sup>TM</sup> 7763v de AMD en una configuración multiproceso con una caché L3 de hasta 256 MB, lo que aumenta las opciones de los clientes para ejecutar la mayoría de las cargas de trabajo optimizadas para memoria. Estas máquinas virtuales ofrecen una combinación de CPU virtuales y memoria para cumplir los requisitos asociados a la mayoría de las aplicaciones empresariales que consumen mucha memoria, como servidores de bases de datos relacionales y cargas de trabajo de análisis en memoria. 

- Las [series Edv5 y Edsv5](edv5-edsv5-series.md) se ejecutan en procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Ice Lake) en una configuración Hyper-Threaded, son idóneas para distintas aplicaciones empresariales con un uso intensivo de memoria y cuentan con hasta 512 GiB de RAM, tecnología [Intel&reg; Turbo Boost 2.0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), tecnología [Intel&reg; Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) e [Intel&reg; Advanced Vector Extensions 512 (Intel&reg; AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html). También admiten [Intel&reg; Deep Learning Boost](https://software.intel.com/content/www/us/en/develop/topics/ai/deep-learning-boost.html). Estos nuevos tamaños de VM tendrán un almacenamiento local un 50 % más grande, así como una mayor E/S por segundo del disco local para lectura y escritura, en comparación con los tamaños [Ev3/Esv3](./ev3-esv3-series.md) con [VM de segunda generación](./generation-2.md). Cuenta con una velocidad de reloj en todos los núcleos de 3,4 GHz.

- Las [series Ev5 y Esv5](ev5-esv5-series.md) se ejecutan en procesadores Intel&reg; Xeon&reg; Platinum 8272CL (Ice Lake) en una configuración Hyper-Threaded, son idóneas para diversas aplicaciones empresariales de uso intensivo de memoria y cuentan con hasta 512 GiB de RAM. Cuenta con una velocidad de reloj en todos los núcleos de 3,4 GHz.

- La [serie M](m-series.md) ofrece un elevado recuento de vCPU (hasta 128 vCPU) y una gran cantidad de memoria (hasta 3,8 TiB). También es muy conveniente para bases de datos extremadamente grandes u otras aplicaciones que se benefician de un elevado número de vCPU y grandes cantidades de memoria.

- La [serie Mv2](mv2-series.md) ofrece el mayor número de vCPU (hasta 416 vCPU) y la memoria más grande (hasta 11,4 TiB) de todas las máquinas virtuales en la nube. Es ideal para bases de datos extremadamente grandes u otras aplicaciones que se benefician de un elevado número de vCPU y grandes cantidades de memoria.

Azure Compute ofrece tamaños de máquinas virtuales que están aislados para un tipo concreto de hardware y dedicados a un solo cliente. Estos tamaños de máquina virtual son más adecuados para cargas de trabajo que requieren un alto grado de aislamiento de otros clientes como, por ejemplo, las cargas de trabajo que incluyen elementos como el cumplimiento normativo y los requisitos legales. Los clientes también puede elegir subdividir aún más los recursos de estas máquinas virtuales aisladas mediante la [compatibilidad de Azure para máquinas virtuales anidadas](https://azure.microsoft.com/blog/nested-virtualization-in-azure/). Consulte las páginas de las familias de máquinas virtuales que aparecen a continuación para ver las opciones de las máquinas virtuales aisladas.

## <a name="other-sizes"></a>Otros tamaños

- [Uso general](sizes-general.md)
- [Proceso optimizado](sizes-compute.md)
- [Almacenamiento optimizado](sizes-storage.md)
- [GPU optimizada](sizes-gpu.md)
- [Proceso de alto rendimiento](sizes-hpc.md)
- [Generaciones anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.

Para más información sobre cómo Azure asigna nombres a las máquinas virtuales, consulte [Convenciones de nomenclatura de los tamaños de las máquinas virtuales de Azure](./vm-naming-conventions.md).
