---
title: Imágenes de Docker precompiladas
titleSuffix: Azure Machine Learning
description: Imágenes de Docker precompiladas para inferencia (puntuación) en Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: ssambare
author: shivanissambare
ms.date: 05/25/2021
ms.topic: conceptual
ms.reviewer: larryfr
ms.custom: deploy, docker, prebuilt
ms.openlocfilehash: db019c225b3e3caaad1a02b30ec0b7278e5b9dcc
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123435913"
---
# <a name="prebuilt-docker-images-for-inference-preview"></a>Imágenes de Docker precompiladas para inferencia (versión preliminar)

Se usan imágenes de contenedor de Docker precompiladas para la inferencia [(versión preliminar)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) al implementar un modelo con Azure Machine Learning.  Las imágenes se precompilan con conocidos marcos de aprendizaje automático y paquetes de Python. También puede ampliar los paquetes para agregar otros paquetes mediante uno de los métodos siguientes:

* [Agregar paquetes de Python](how-to-prebuilt-docker-images-inference-python-extensibility.md).
* [Usar una imagen de inferencia precompilada como base para un nuevo Dockerfile](how-to-extend-prebuilt-docker-image-inference.md). Con este método, puede instalar **paquetes de Python y paquetes apt**.

## <a name="why-should-i-use-prebuilt-images"></a>¿Por qué debo usar imágenes precompiladas?

* Reduce la latencia de la implementación de modelo.
* Mejora la tasa de éxito de la implementación de modelo.
* Evite la compilación innecesaria de imágenes durante la implementación de modelo.
* Disponga solo de las dependencias y el derecho de acceso necesarios en la imagen o contenedor. 

## <a name="list-of-prebuilt-docker-images-for-inference"></a>Lista de imágenes de Docker precompiladas para la inferencia 

* Todas las imágenes de Docker se ejecutan como usuario no raíz.
* Se recomienda usar la etiqueta `latest` para las imágenes de Docker. Las imágenes de Docker precompiladas para la inferencia se publican en Microsoft Container Registry (MCR); para consultar la lista de etiquetas disponibles, siga las [instrucciones de su repositorio de GitHub](https://github.com/microsoft/ContainerRegistry#browsing-mcr-content).

### <a name="tensorflow"></a>TensorFlow

Versión del marco | CPU/GPU | Paquetes preinstalados | Ruta de acceso de MCR | Entorno mantenido
 --- | --- | --- | --- | --- |
 1.15 | CPU | pandas==0.25.1 </br> numpy==1.20.1 | `mcr.microsoft.com/azureml/tensorflow-1.15-ubuntu18.04-py37-cpu-inference:latest`  | AzureML-tensorflow-1.15-ubuntu18.04-py37-cpu-inference | 
2.4 | CPU | numpy>=1.16.0 </br> pandas~=1.1.x | `mcr.microsoft.com/azureml/tensorflow-2.4-ubuntu18.04-py37-cpu-inference:latest` | AzureML-tensorflow-2.4-ubuntu18.04-py37-cpu-inference |
2.4 | GPU | numpy >= 1.16.0 </br> pandas~=1.1.x </br> CUDA==11.0.3 </br> CuDNN==8.0.5.39 | `mcr.microsoft.com/azureml/tensorflow-2.4-ubuntu18.04-py37-cuda11.0.3-gpu-inference:latest` | AzureML-tensorflow-2.4-ubuntu18.04-py37-cuda11.0.3-gpu-inference |

### <a name="pytorch"></a>PyTorch

Versión del marco | CPU/GPU | Paquetes preinstalados | Ruta de acceso de MCR | Entorno mantenido
 --- | --- | --- | --- | --- |
 1.6 | CPU | numpy==1.20.1 </br> pandas==0.25.1 | `mcr.microsoft.com/azureml/pytorch-1.6-ubuntu18.04-py37-cpu-inference:latest` | AzureML-pytorch-1.6-ubuntu18.04-py37-cpu-inference |
1.7 | CPU | numpy>=1.16.0 </br> pandas~=1.1.x | `mcr.microsoft.com/azureml/pytorch-1.7-ubuntu18.04-py37-cpu-inference:latest` | AzureML-pytorch-1.7-ubuntu18.04-py37-cpu-inference |

### <a name="scikit-learn"></a>SciKit-Learn

Versión del marco | CPU/GPU | Paquetes preinstalados | Ruta de acceso de MCR | Entorno mantenido
 --- | --- | --- | --- | --- |
0.24.1  | CPU | scikit-learn==0.24.1 </br> numpy>=1.16.0 </br> pandas~=1.1.x | `mcr.microsoft.com/azureml/sklearn-0.24.1-ubuntu18.04-py37-cpu-inference:latest` | AzureML-sklearn-0.24.1-ubuntu18.04-py37-cpu-inference |

### <a name="onnx-runtime"></a>ONNX Runtime

Versión del marco | CPU/GPU | Paquetes preinstalados | Ruta de acceso de MCR | Entorno mantenido
 --- | --- | --- | --- | --- |
1.6 | CPU | numpy>=1.16.0 </br> pandas~=1.1.x | `mcr.microsoft.com/azureml/onnxruntime-1.6-ubuntu18.04-py37-cpu-inference:latest` |AzureML-onnxruntime-1.6-ubuntu18.04-py37-cpu-inference |

### <a name="xgboost"></a>XGBoost

Versión del marco | CPU/GPU | Paquetes preinstalados | Ruta de acceso de MCR | Entorno mantenido
 --- | --- | --- | --- | --- |
0.9 | CPU | scikit-learn==0.23.2 </br> numpy==1.20.1 </br> pandas==0.25.1 | `mcr.microsoft.com/azureml/xgboost-0.9-ubuntu18.04-py37-cpu-inference:latest` | AzureML-xgboost-0.9-ubuntu18.04-py37-cpu-inference | 

### <a name="no-framework"></a>Ningún marco

Versión del marco | CPU/GPU | Paquetes preinstalados | Ruta de acceso de MCR | Entorno mantenido
 --- | --- | --- | --- | --- |
N/D | CPU | N/D | `mcr.microsoft.com/azureml/minimal-ubuntu18.04-py37-cpu-inference:latest` | AzureML-minimal-ubuntu18.04-py37-cpu-inference  |

## <a name="next-steps"></a>Pasos siguientes

* [Agregue paquetes de Python a imágenes precompiladas](how-to-prebuilt-docker-images-inference-python-extensibility.md).
* [Use un paquete precompilado como base para un nuevo Dockerfile](how-to-extend-prebuilt-docker-image-inference.md).