---
title: Archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 06/18/2021
ms.openlocfilehash: 2e44972188b318e4c2f0d61b1ccd9222e7b2fce9
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129713406"
---
**Los destinos de proceso se pueden reutilizar de un trabajo de entrenamiento al siguiente**. Por ejemplo, después de adjuntar una VM remota al área de trabajo, puede reutilizarla para varios trabajos. En el caso de las canalizaciones de aprendizaje automático, use el [paso de canalización](/python/api/azureml-pipeline-steps/azureml.pipeline.steps) adecuado para cada destino de proceso.

Puede usar cualquiera de los siguientes recursos para un destino de proceso de entrenamiento para la mayoría de los trabajos. No todos los recursos se pueden usar para el aprendizaje automático automatizado, canalizaciones de aprendizaje automático o el diseñador. Azure Databricks se puede usar como un recurso de entrenamiento para ejecuciones locales y canalizaciones de aprendizaje automático, pero no como destino remoto de otro entrenamiento.

|&nbsp;Objetivos del aprendizaje|[Aprendizaje automático automatizado](../articles/machine-learning/concept-automated-ml.md) | [Canalizaciones de aprendizaje automático](../articles/machine-learning/concept-ml-pipelines.md) | [Diseñador de Azure Machine Learning](../articles/machine-learning/concept-designer.md)
|----|:----:|:----:|:----:|
|[Equipo local](../articles/machine-learning/how-to-attach-compute-targets.md#local)| Sí | &nbsp; | &nbsp; |
|[Clúster de proceso de Azure Machine Learning](../articles/machine-learning/how-to-create-attach-compute-cluster.md)| Sí | Sí | Sí |
|[Instancia de proceso de Azure Machine Learning](../articles/machine-learning/how-to-create-manage-compute-instance.md) | Sí (mediante el SDK)  | Sí |  |
|[Máquina virtual remota](../articles/machine-learning/how-to-attach-compute-targets.md#vm) | Sí  | Sí | &nbsp; |
|[Grupos de Apache Spark (versión preliminar)](../articles/machine-learning/how-to-attach-compute-targets.md#synapse)| Sí (solo en el modo local del SDK) | Sí | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/how-to-attach-compute-targets.md#databricks)| Sí (solo en el modo local del SDK) | Sí | &nbsp; |
|[Análisis con Azure Data Lake](../articles/machine-learning/how-to-attach-compute-targets.md#adla) | &nbsp; | Sí | &nbsp; |
|[HDInsight de Azure](../articles/machine-learning/how-to-attach-compute-targets.md#hdinsight) | &nbsp; | Sí | &nbsp; |
|[Azure Batch](../articles/machine-learning/how-to-attach-compute-targets.md#azbatch) | &nbsp; | Sí | &nbsp; |
|[Azure Kubernetes Service](../articles/machine-learning/how-to-attach-compute-targets.md#kubernetes) (versión preliminar) | Sí | Sí | Sí |
|[Kubernetes habilitado para Azure Arc](../articles/machine-learning/how-to-attach-compute-targets.md#kubernetes) (versión preliminar) | Sí | Sí | Sí |

> [!TIP]
> La instancia de proceso tiene un disco de SO de 120 GB. Si se queda sin espacio en disco, [use el terminal](../articles/machine-learning/how-to-access-terminal.md) para borrar al menos entre 1 y 2 GB antes de [detener o reiniciar](../articles/machine-learning/how-to-create-manage-compute-instance.md#manage) la instancia de proceso.
