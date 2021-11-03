---
title: Qué son los destinos de proceso
titleSuffix: Azure Machine Learning
description: Aprenda a designar un recurso de proceso o un entorno para entrenar o implementar el modelo con Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 10/21/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: 96f0f011382fb631416665cad58ca7d7324fcb24
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131076628"
---
# <a name="what-are-compute-targets-in-azure-machine-learning"></a>¿Qué son los destinos de proceso en Azure Machine Learning?

Un *destino de proceso* es un entorno o recurso de proceso designado en el que se ejecuta el script de entrenamiento o se hospeda la implementación del servicio. Esta ubicación podría ser su equipo local o un recurso de proceso basado en la nube. El uso de los destinos de proceso facilita el cambio posterior del entorno de proceso sin tener que cambiar el código.

En un ciclo de vida de desarrollo de modelos típico, puede:

1. Comenzar desarrollando y experimentando con una pequeña cantidad de datos. En esta etapa, use su entorno local, como un equipo local o una máquina virtual (VM) basada en la nube, como destino de proceso.
1. Escalar verticalmente a datos más grandes o realizar un [entrenamiento distribuido](how-to-train-distributed-gpu.md) mediante el uso de uno de estos [destinos de proceso de entrenamiento](#train).
1. Después de que el modelo esté listo, impleméntelo en un entorno de hospedaje web con uno de estos [destinos de proceso de implementación](#deploy).

Los recursos de proceso que use para los destinos de proceso están asociados a un [área de trabajo](concept-workspace.md). Los usuarios del área de trabajo comparten los recursos de proceso que no sean el equipo local.

## <a name="training-compute-targets"></a><a name="train"></a> Entrenamiento de destinos de proceso

Azure Machine Learning tiene distintas modalidades de soporte técnico en los diferentes destinos de proceso. Un ciclo de vida de desarrollo de modelos típico comienza con el desarrollo o la experimentación en una pequeña cantidad de datos. En esta etapa, use un entorno local, como el equipo local o una máquina virtual basada en la nube. A medida que escala verticalmente el entrenamiento a conjuntos de datos grandes o realiza el [entrenamiento distribuido](how-to-train-distributed-gpu.md), use un proceso de Azure Machine Learning para crear un clúster con uno o varios nodos que se escala automáticamente cada vez que se envía una ejecución. También puede adjuntar su propio recurso de proceso, aunque el soporte técnico para escenarios distintos puede variar.

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

Obtenga más información sobre cómo [enviar una ejecución de entrenamiento a un destino de proceso](how-to-set-up-training-targets.md).

## <a name="compute-targets-for-inference"></a><a name="deploy"></a> Destinos de proceso para inferencia

Al realizar la inferencia, Azure Machine Learning crea un contenedor de Docker que hospeda el modelo y los recursos asociados necesarios para utilizarlo. A continuación, este contenedor se usa en un destino de proceso.

[!INCLUDE [aml-deploy-target](../../includes/aml-compute-target-deploy.md)]

Aprenda [dónde y cómo implementar el modelo en un destino de proceso](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Proceso de Azure Machine Learning (administrado)

Azure Machine Learning crea y administra un recurso de proceso administrado. Dicho proceso está optimizado para cargas de trabajo de Machine Learning. Los clústeres de procesos y las [instancias de procesos](concept-compute-instance.md) de Azure Machine Learning son los únicos procesos administrados.

Puede crear instancias de procesos o clústeres de procesos de Azure Machine Learning con cualquiera de las siguientes opciones:

* [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md).
* El SDK de Python y la CLI de Azure:
    * [Instancia de proceso](how-to-create-manage-compute-instance.md).
    * [Clúster de proceso](how-to-create-attach-compute-cluster.md).
* Una plantilla de Azure Resource Manager Para ver una plantilla de ejemplo, consulte [Creación de un clúster de proceso de Azure Machine Learning](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.machinelearningservices/machine-learning-compute-create-amlcompute).
* Una [extensión de aprendizaje automático para la CLI de Azure](reference-azure-machine-learning-cli.md#resource-management).

Cuando se crean, estos recursos de proceso forman parte automáticamente del área de trabajo, a diferencia de otros tipos de destinos de proceso.


|Capacidad  |Clúster de proceso  |Instancia de proceso  |
|---------|---------|---------|
|Clúster de uno o varios nodos     |    **&check;**       |    Clúster de un solo nodo     |
|Se escala automáticamente cada vez que se envía una ejecución     |     **&check;**      |         |
|Administración del clúster automático y programación de trabajos     |   **&check;**        |     **&check;**      |
|Es compatible con recursos de CPU y GPU     |  **&check;**         |    **&check;**       |


> [!NOTE]
> Cuando un *clúster* de proceso está inactivo, se escala automáticamente a 0 nodos, por lo que no paga cuando no se usa. Una *instancia* de proceso está siempre activa y no se escala automáticamente. Debe [detener la instancia de proceso](how-to-create-manage-compute-instance.md#manage) cuando no la esté usando para evitar costos adicionales.

### <a name="supported-vm-series-and-sizes"></a>Series y tamaños de maquina virtual compatibles

Al seleccionar un tamaño de nodo para un recurso de proceso administrado en Azure Machine Learning, puede elegir entre varios tamaños de máquina virtual disponibles en Azure. Azure ofrece una variedad de tamaños para Windows y Linux para diferentes cargas de trabajo. Para más información, consulte [Tamaños de las máquinas virtuales Linux en Azure](../virtual-machines/sizes.md).

Hay algunas excepciones y limitaciones a la hora de elegir un tamaño de máquina virtual:

* Algunas series de máquinas virtuales no se admiten en Azure Machine Learning.
* Algunas series de máquinas virtuales están restringidas. Para usar una serie restringida, póngase en contacto con el soporte técnico y solicite un aumento de la cuota para la serie. Para información sobre cómo ponerse en contacto con el soporte técnico, consulte [Opciones de soporte técnico de Azure](https://azure.microsoft.com/support/options/).

Consulte la tabla siguiente para más información sobre las series admitidas y las restricciones.

| **Series de maquinas virtuales compatibles**  | **Restricciones** | **Categoría** | **Compatible con** |
|------------|------------|------------|------------|
| [DDSv4](../virtual-machines/ddv4-ddsv4-series.md#ddsv4-series) | Ninguno. | Uso general | Clústeres de proceso e instancia |
| [Dv2](../virtual-machines/dv2-dsv2-series.md#dv2-series) | Ninguno. | Uso general | Clústeres de proceso e instancia |
| [Dv3](../virtual-machines/dv3-dsv3-series.md#dv3-series) | Ninguno.| Uso general | Clústeres de proceso e instancia |
| [DSv2](../virtual-machines/dv2-dsv2-series.md#dsv2-series) | Ninguno. | Uso general | Clústeres de proceso e instancia |
| [DSv3](../virtual-machines/dv3-dsv3-series.md#dsv3-series) | Ninguno.| Uso general | Clústeres de proceso e instancia |
| [EAv4](../virtual-machines/eav4-easv4-series.md) | Ninguno. | Memoria optimizada | Clústeres de proceso e instancia |
| [Ev3](../virtual-machines/ev3-esv3-series.md) | Ninguno. | Memoria optimizada | Clústeres de proceso e instancia |
| [FSv2](../virtual-machines/fsv2-series.md) | Ninguno. | Proceso optimizado | Clústeres de proceso e instancia |
| [FX](../virtual-machines/fx-series.md) | Requiere aprobación. | Proceso optimizado | Clústeres de proceso |
| [H](../virtual-machines/h-series.md) | Ninguno. | Informática de alto rendimiento | Clústeres de proceso e instancia |
| [HB](../virtual-machines/hb-series.md) | Requiere aprobación. | Informática de alto rendimiento | Clústeres de proceso e instancia |
| [HBv2](../virtual-machines/hbv2-series.md) | Requiere aprobación. |  Informática de alto rendimiento | Clústeres de proceso e instancia |
| [HBv3](../virtual-machines/hbv3-series.md) | Requiere aprobación. |  Informática de alto rendimiento | Clústeres de proceso e instancia |
| [HC](../virtual-machines/hc-series.md) | Requiere aprobación. |  Informática de alto rendimiento | Clústeres de proceso e instancia |
| [LSv2](../virtual-machines/lsv2-series.md) | Ninguno. |  Almacenamiento optimizado | Clústeres de proceso e instancia |
| [M](../virtual-machines/m-series.md) | Requiere aprobación. | Memoria optimizada | Clústeres de proceso e instancia |
| [NC](../virtual-machines/nc-series.md) | Ninguno. |  GPU | Clústeres de proceso e instancia |
| [NC Promo](../virtual-machines/nc-series.md) | Ninguno. | GPU | Clústeres de proceso e instancia |
| [NCv2](../virtual-machines/ncv2-series.md) | Requiere aprobación. | GPU | Clústeres de proceso e instancia |
| [NCv3](../virtual-machines/ncv3-series.md) | Requiere aprobación. | GPU | Clústeres de proceso e instancia |
| [ND](../virtual-machines/nd-series.md) | Requiere aprobación. | GPU | Clústeres de proceso e instancia |
| [NDv2](../virtual-machines/ndv2-series.md) | Requiere aprobación. | GPU | Clústeres de proceso e instancia |
| [NV](../virtual-machines/nv-series.md) | Ninguno. | GPU | Clústeres de proceso e instancia |
| [NVv3](../virtual-machines/nvv3-series.md) | Requiere aprobación. | GPU | Clústeres de proceso e instancia |
| [NCasT4_v3](../virtual-machines/nct4-v3-series.md) | Requiere aprobación. | GPU | Clústeres de proceso e instancia |
| [NDasrA100_v4](../virtual-machines/nda100-v4-series.md) | Requiere aprobación. | GPU | Clústeres de proceso e instancia |


Aunque Azure Machine Learning admite estas series de máquinas virtuales, puede que no estén disponibles en todas las regiones de Azure. Para comprobar si las series de máquinas virtuales están disponibles o no, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

> [!NOTE]
> Azure Machine Learning no es compatible con todos los tamaños de máquina virtual que admite Azure Compute. Para una lista de los tamaños de máquina virtual disponibles, use uno de los métodos siguientes:
> * [REST API](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/machinelearningservices/resource-manager/Microsoft.MachineLearningServices/stable/2020-08-01/examples/ListVMSizesResult.json)
> * [SDK de Python](/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-)
>

Si usa los destinos de proceso habilitados para GPU, es importante asegurarse de que se han instalado los controladores CUDA correctos en el entorno de entrenamiento. Use la tabla siguiente para determinar la versión correcta de CUDA que se usará:

| **Arquitectura de GPU**  | **Serie de máquinas virtuales de Azure** | **Versiones de CUDA admitidas** |
|------------|------------|------------|
| Ampere | NDA100_v4 | Versión 11.0 y posteriores |
| Turing | NCT4_v3 | 10.0+ |
| Volta | NCv3, NDv2 | 9.0+ |
| Pascal | NCv2, ND | 9.0+ |
| Maxwell | NV, NVv3 | 9.0+ |
| Kepler | NC, NC Promo| 9.0+ |

Además de garantizar que la versión de CUDA y el hardware sean compatibles, también asegúrese de que la versión de CUDA sea compatible con la versión del marco de aprendizaje automático que utiliza: 

- Para PyTorch, puede comprobar la compatibilidad [aquí](https://pytorch.org/get-started/previous-versions/). 
- Para Tensorflow, puede comprobar la compatibilidad [aquí](https://www.tensorflow.org/install/source#gpu).

### <a name="compute-isolation"></a>Aislamiento de proceso

El proceso de Azure Machine Learning ofrece tamaños de máquina virtual que están aislados para un tipo concreto de hardware y dedicados a un solo cliente. Los tamaños de máquina virtual aislados son más adecuados para cargas de trabajo que requieren un alto grado de aislamiento de otros clientes por motivos como, por ejemplo, el cumplimiento normativo y de requisitos legales. Usar un tamaño aislado garantiza que la máquina virtual será la única que se ejecute en esa instancia de servidor específica.

Las ofertas de máquinas virtuales aisladas actuales incluyen:

* Standard_M128ms
* Standard_F72s_v2
* Standard_NC24s_v3
* Standard_NC24rs_v3*

*Compatible con RDMA

Para más información sobre el aislamiento, consulte [Aislamiento en la nube pública de Azure](../security/fundamentals/isolation-choices.md).

## <a name="unmanaged-compute"></a>Proceso no administrado

Azure Machine Learning *no* administra un destino de proceso no administrado. Cree este tipo de destino de proceso fuera de Azure Machine Learning y luego adjúntelo al área de trabajo. Estos recursos de proceso no administrados pueden requerir pasos adicionales para mantener o mejorar el rendimiento de las cargas de trabajo de Machine Learning. 

Azure Machine Learning admite los siguientes tipos de proceso no administrado:

* Equipo local
* Máquinas virtuales remotas
* HDInsight de Azure
* Azure Batch
* Azure Databricks
* Análisis con Azure Data Lake
* Azure Container Instances
* Azure Kubernetes Service y Kubernetes habilitado para Azure Arc (versión preliminar)

Para más información, consulte [Configuración de destinos de proceso para el entrenamiento y la implementación de modelos](how-to-attach-compute-targets.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo:
* [Usar un destino de proceso para entrenar el modelo](how-to-set-up-training-targets.md)
* [Implementar el modelo para un destino de proceso](how-to-deploy-and-where.md)
