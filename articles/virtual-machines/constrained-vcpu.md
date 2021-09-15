---
title: Tamaños de vCPU restringidos
description: Enumera los tamaños VM que pueden tener un recuento de vCPU restringido.
author: mimckitt
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 03/09/2018
ms.author: mimckitt
ms.openlocfilehash: 2f86f559bfba9c2fcc75649153db450abb877b4d
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122696535"
---
# <a name="constrained-vcpu-capable-vm-sizes"></a>Tamaños de VM que admiten vCPU restringidas

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

> [!TIP]
> Pruebe la **[herramienta de selección de máquinas virtuales](https://aka.ms/vm-selector)** para buscar otros tamaños que se adapten mejor a la carga de trabajo.

Algunas cargas de trabajo de base de datos como SQL Server u Oracle requieren mucha memoria, almacenamiento y ancho de banda de E/S, pero no un recuento de núcleos alto. Muchas cargas de trabajo de base de datos no consumen demasiados recursos de CPU. Azure ofrece determinados tamaños de VM que permiten restringir el recuento de vCPU de VM para reducir el costo de licencias de software y mantener la misma memoria, almacenamiento y ancho de banda de E/S.

Se puede restringir el número de vCPU a la mitad o un cuarto del tamaño de VM original. Estos nuevos tamaños de VM tienen un sufijo que especifica el número de vCPU activas para facilitar su identificación.

Por ejemplo, el tamaño de VM Standard_GS5 actual incluye 32 vCPU, 448 GB de RAM, 64 discos (hasta 256 TB) y 80 000 E/S por segundo o 2 GB/s de ancho de banda de E/S. Los nuevos tamaños de VM Standard_GS5-16 y Standard_GS5-8 incluyen 16 y 8 vCPU activas, respectivamente y mantienen el resto de especificaciones de Standard_GS5 para memoria, almacenamiento y ancho de banda de E/S.

Las tarifas de licencias que se cobran para SQL Server u Oracle están restringidas al nuevo recuento de vCPU y otros productos deben cobrarse según el nuevo recuento de vCPU. Esto provoca un aumento de entre el 50 % y el 75 % de la relación de las especificaciones de VM con vCPU activas (facturables). Estos nuevos tamaños de máquina virtual permiten que las cargas de trabajo de los clientes usen la misma memoria, almacenamiento y ancho de banda de entrada y salida, al tiempo que se optimiza el costo que generan por las licencias de software. En este momento, el costo de proceso, que incluye licencias de sistema operativo, es igual que el del tamaño original. Para más información, consulte [Azure VM sizes for more cost-effective database workloads](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/) (Tamaños de VM de Azure para cargas de trabajo de base de datos más rentables).


| Nombre                | vCPU | Especificaciones           |
|---------------------|------|-----------------|
| Standard_M8-2ms     | 2    | Igual que M8ms    |
| Standard_M8-4ms     | 4    | Igual que M8ms    |
| Standard_M16-4ms    | 4    | Igual que M16ms   |
| Standard_M16-8ms    | 8    | Igual que M16ms   |
| Standard_M32-8ms    | 8    | Igual que M32ms   |
| Standard_M32-16ms   | 16   | Igual que M32ms   |
| Standard_M64-32ms   | 32   | Igual que M64ms   |
| Standard_M64-16ms   | 16   | Igual que M64ms   |
| Standard_M128-64ms  | 64   | Igual que M128ms  |
| Standard_M128-32ms  | 32   | Igual que M128ms  |
| Standard_E4-2s_v3   | 2    | Igual que E4s_v3  |
| Standard_E8-4s_v3   | 4    | Igual que E8s_v3  |
| Standard_E8-2s_v3   | 2    | Igual que E8s_v3  |
| Standard_E16-8s_v3  | 8    | Igual que E16s_v3 |
| Standard_E16-4s_v3  | 4    | Igual que E16s_v3 |
| Standard_E32-16s_v3 | 16   | Igual que E32s_v3 |
| Standard_E32-8s_v3  | 8    | Igual que E32s_v3 |
| Standard_E64-32s_v3 | 32   | Igual que E64s_v3 |
| Standard_E64-16s_v3 | 16   | Igual que E64s_v3 |
| Standard_E4-2s_v4   | 2    | Igual que E4s_v4  |
| Standard_E8-4s_v4   | 4    | Igual que E8s_v4  |
| Standard_E8-2s_v4   | 2    | Igual que E8s_v4  |
| Standard_E16-8s_v4  | 8    | Igual que E16s_v4 |
| Standard_E16-4s_v4  | 4    | Igual que E16s_v4 |
| Standard_E32-16s_v4 | 16   | Igual que E32s_v4 |
| Standard_E32-8s_v4  | 8    | Igual que E32s_v4 |
| Standard_E64-32s_v4 | 32   | Igual que E64s_v4 |
| Standard_E64-16s_v4 | 16   | Igual que E64s_v4 |
| Standard_E4-2ds_v4  | 2    | Igual que E4ds_v4 |
| Standard_E8-4ds_v4  | 4    | Igual que E8ds_v4 |
| Standard_E8-2ds_v4  | 2    | Igual que E8ds_v4 |
| Standard_E16-8ds_v4 | 8    | Igual que E16ds_v4|
| Standard_E16-4ds_v4 | 4    | Igual que E16ds_v4|
| Standard_E32-16ds_v4| 16   | Igual que E32ds_v4|
| Standard_E32-8ds_v4 | 8    | Igual que E32ds_v4|
| Standard_E64-32ds_v4| 32   | Igual que E64ds_v4|
| Standard_E64-16ds_v4| 16   | Igual que E64ds_v4|
| Standard_E4-2as_v4  | 2    | Igual que E4as_v4 |
| Standard_E8-4as_v4  | 4    | Igual que E8as_v4 |
| Standard_E8-2as_v4  | 2    | Igual que E8as_v4 |
| Standard_E16-8as_v4 | 8    | Igual que E16as_v4|
| Standard_E16-4as_v4 | 4    | Igual que E16as_v4|
| Standard_E32-16as_v4| 16   | Igual que E32as_v4|
| Standard_E32-8as_v4 | 8    | Igual que E32as_v4|
| Standard_E64-32as_v4| 32   | Igual que E64as_v4|
| Standard_E64-16as_v4| 16   | Igual que E64as_v4|
| Standard_E96-48as_v4| 48   | Igual que E96as_v4|
| Standard_E96-24as_v4| 24   | Igual que E96as_v4|
| Standard_GS4-8      | 8    | Igual que GS4     |
| Standard_GS4-4      | 4    | Igual que GS4     |
| Standard_GS5-16     | 16   | Igual que GS5     |
| Standard_GS5-8      | 8    | Igual que GS5     |
| Standard_DS11-1_v2  | 1    | Igual que DS11_v2 |
| Standard_DS12-2_v2  | 2    | Igual que DS12_v2 |
| Standard_DS12-1_v2  | 1    | Igual que DS12_v2 |
| Standard_DS13-4_v2  | 4    | Igual que DS13_v2 |
| Standard_DS13-2_v2  | 2    | Igual que DS13_v2 |
| Standard_DS14-8_v2  | 8    | Igual que DS14_v2 |
| Standard_DS14-4_v2  | 4    | Igual que DS14_v2 |
| Standard_M416-208s_v2 | 208    | Igual que M416s_v2|
| Standard_M416-208ms_v2 | 208    | Igual que M416ms_v2 |

## <a name="other-sizes"></a>Otros tamaños
- [Proceso optimizado](./sizes-compute.md)
- [Memoria optimizada](./sizes-memory.md)
- [Almacenamiento optimizado](./sizes-storage.md)
- [GPU](./sizes-gpu.md)
- [Proceso de alto rendimiento](./sizes-hpc.md)

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre cómo las [unidades de proceso de Azure (ACU)](./acu.md) pueden ayudarlo a comparar el rendimiento en los distintos SKU de Azure.
