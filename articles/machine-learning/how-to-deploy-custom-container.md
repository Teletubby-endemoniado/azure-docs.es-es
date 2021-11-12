---
title: Implementación de un contenedor personalizado como un punto de conexión en línea administrado
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar un contenedor personalizado para usar servidores de código abierto en Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: mlops
ms.author: ssambare
author: shivanissambare
ms.reviewer: larryfr
ms.date: 10/21/2021
ms.topic: how-to
ms.custom: deploy, devplatv2
ms.openlocfilehash: fdbe6f6232bcd4d53ce3473a80de2829f02fb6bf
ms.sourcegitcommit: 61f87d27e05547f3c22044c6aa42be8f23673256
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2021
ms.locfileid: "132056670"
---
# <a name="deploy-a-tensorflow-model-served-with-tf-serving-using-a-custom-container-in-a-managed-online-endpoint-preview"></a>Implementación de un modelo de TensorFlow servido con TF Serving mediante un contenedor personalizado en un punto de conexión en línea administrado (versión preliminar)

Obtenga información sobre cómo implementar un contenedor personalizado como un punto de conexión en línea administrado en Azure Machine Learning.

Las implementaciones de contenedores personalizados pueden usar servidores web distintos del servidor Flask de Python predeterminado que usa Azure Machine Learning. Los usuarios de estas implementaciones todavía pueden aprovechar las ventajas de la supervisión, el escalado, las alertas y la autenticación integradas de Azure Machine Learning.

[!INCLUDE [preview disclaimer](../../includes/machine-learning-preview-generic-disclaimer.md)]

> [!WARNING]
> Es posible que Microsoft no pueda ayudar a solucionar los problemas derivados de una imagen personalizada. Si tiene problemas, se le pedirá que use la imagen predeterminada o una de las imágenes que proporciona Microsoft para ver si el problema es específico de la imagen.

## <a name="prerequisites"></a>Requisitos previos

* Instale y configure la CLI de Azure y la extensión de Machine Learning. Para más información, consulte [Instalación, configuración y uso de la CLI v2 (versión preliminar)](how-to-configure-cli.md). 

* Debe tener un grupo de recursos de Azure, en el que usted (o la entidad de servicio que use) necesita tener acceso de `Contributor`. Tendrá un grupo de recursos de este tipo si configuró la extensión de Machine Learning según el artículo anterior. 

* Debe tener un área de trabajo de Azure Machine Learning. Tendrá este tipo de área de trabajo si configuró la extensión de ML según el artículo anterior.

* Si aún no ha establecido los valores predeterminados de la CLI de Azure, debe guardar la configuración predeterminada. Para evitar tener que pasar repetidamente los valores, ejecute:

   ```azurecli
   az account set --subscription <subscription id>
   az configure --defaults workspace=<azureml workspace name> group=<resource group>

* To deploy locally, you must have [Docker engine](https://docs.docker.com/engine/install/) running locally. This step is **highly recommended**. It will help you debug issues.

## Download source code

To follow along with this tutorial, download the source code below.

```azurecli
git clone https://github.com/Azure/azureml-examples --depth 1
cd azureml-examples/cli
```

## <a name="initialize-environment-variables"></a>Inicialización de las variables de entorno

Defina las variables de entorno:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-tfserving.sh" id="initialize_variables":::

## <a name="download-a-tensorflow-model"></a>Descarga de un modelo de TensorFlow

Descargue y descomprima un modelo que divide una entrada entre dos y agrega 2 al resultado:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-tfserving.sh" id="download_and_unzip_model":::

## <a name="run-a-tf-serving-image-locally-to-test-that-it-works"></a>Ejecución local de una imagen de TF Serving para probar que funciona

Use Docker para ejecutar la imagen localmente para realizar pruebas:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-tfserving.sh" id="run_image_locally_for_testing":::

### <a name="check-that-you-can-send-liveness-and-scoring-requests-to-the-image"></a>Comprobar que pueden enviar solicitudes de ejecución y puntuación a la imagen

En primer lugar, compruebe que el contenedor está "activo", lo que significa que el proceso dentro del contenedor todavía está en ejecución. Debería obtener una respuesta 200 (OK).

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-tfserving.sh" id="check_liveness_locally":::

A continuación, compruebe que puede obtener predicciones sobre datos sin etiquetar:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-tfserving.sh" id="check_scoring_locally":::

### <a name="stop-the-image"></a>Detención de la imagen

Ahora que ha realizado las pruebas localmente, detenga la imagen:

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-tfserving.sh" id="stop_image":::

## <a name="create-a-yaml-file-for-your-endpoint-and-deployment"></a>Creación de un archivo YAML para el punto de conexión y la implementación

Puede configurar la implementación en la nube mediante YAML. Eche un vistazo al YAML de muestra para este ejemplo:

__tfserving-endpoint.yml__

:::code language="yaml" source="~/azureml-examples-main/cli/endpoints/online/custom-container/tfserving-endpoint.yml":::

__tfserving-deployment.yml__

:::code language="yaml" source="~/azureml-examples-main/cli/endpoints/online/custom-container/tfserving-deployment.yml":::

Hay algunos conceptos importantes que se pueden observar en este código YAML:

### <a name="readiness-route-vs-liveness-route"></a>Ruta de preparación frente a ruta de ejecución

Un servidor HTTP define rutas de acceso tanto para la _ejecución_ como para la _preparación_. Se usa una ruta de ejecución para comprobar si el servidor se está ejecutando. Se usa una ruta de preparación para comprobar si el servidor está listo para realizar trabajo. En la inferencia del aprendizaje automático, un servidor puede responder 200 OK a una solicitud de comprobación de ejecución antes de cargar un modelo. El servidor puede responder 200 OK a una solicitud de preparación solo después de que el modelo se haya cargado en memoria.

Revise la [documentación de Kubernetes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) para más información sobre los sondeos de ejecución y preparación.

Tenga en cuenta que esta implementación usa la misma ruta de acceso para la ejecución y la preparación, ya que TF Serving solo define una ruta de ejecución.

### <a name="locating-the-mounted-model"></a>Búsqueda del modelo montado

Al implementar un modelo como un punto de conexión en tiempo real, Azure Machine Learning _monta_ el modelo en el punto de conexión. El montaje del modelo le permite implementar nuevas versiones del modelo sin tener que crear una nueva imagen de Docker. De manera predeterminada, un modelo registrado con el nombre *foo* y la versión *1* se ubicaría en la siguiente ruta de acceso dentro del contenedor implementado: `/var/azureml-app/azureml-models/foo/1`

Por ejemplo, si tiene una estructura de directorios de `/azureml-examples/cli/endpoints/online/custom-container` en el equipo local, donde el modelo se denomina `half_plus_two`:

:::image type="content" source="./media/how-to-deploy-custom-container/local-directory-structure.png" alt-text="Diagrama que muestra una vista de árbol de la estructura de directorios local.":::

y `tfserving-deployment.yml` contiene:

```yaml
model:
    name: tfserving-mounted
    version: 1
    local_path: ./half_plus_two
```

el modelo se ubicará en `/var/azureml-app/azureml-models/tfserving-deployment/1` en la implementación:

:::image type="content" source="./media/how-to-deploy-custom-container/deployment-location.png" alt-text="Diagrama que muestra una vista de árbol de la estructura de directorios de implementación.":::

Opcionalmente, puede configurar `model_mount_path`. Permite cambiar la ruta de acceso donde se monta el modelo. Por ejemplo, puede tener el parámetro `model_mount_path` en _tfserving-deployment.yml_:

> [!IMPORTANT]
> `model_mount_path` debe ser una ruta de acceso absoluta válida en Linux (el sistema operativo de la imagen de contenedor).

```YAML
name: tfserving-deployment
endpoint_name: tfserving-endpoint
model:
  name: tfserving-mounted
  version: 1
  local_path: ./half_plus_two
model_mount_path: /var/tfserving-model-mount
.....
```

el modelo se ubicará en `/var/tfserving-model-mount/tfserving-deployment/1` en la implementación. Tenga en cuenta que ya no está en `azureml-app/azureml-models`, sino en la ruta de acceso de montaje que especificó:

:::image type="content" source="./media/how-to-deploy-custom-container/mount-path-deployment-location.png" alt-text="Diagrama que muestra una vista de árbol de la estructura de directorios de implementación al usar mount_model_path.":::

### <a name="create-your-endpoint-and-deployment"></a>Creación del punto de conexión y la implementación

Ahora que ha comprendido cómo se construye el código YAML, cree el punto de conexión.

```azurecli
az ml online-endpoint create --name tfserving-endpoint -f endpoints/online/custom-container/tfserving-endpoint.yml
```

La creación de una implementación puede tardar unos minutos.



```azurecli
az ml online-deployment create --name tfserving-deployment -f endpoints/online/custom-container/tfserving-deployment.yml
```

### <a name="invoke-the-endpoint"></a>Invocación del punto de conexión

Una vez completada la implementación, compruebe si puede realizar una solicitud de puntuación al punto de conexión implementado.

:::code language="azurecli" source="~/azureml-examples-main/cli/deploy-tfserving.sh" id="invoke_endpoint":::

### <a name="delete-endpoint-and-model"></a>Eliminación del punto de conexión y el modelo

Ahora que ha realizado la puntuación correctamente con el punto de conexión, puede eliminarlo:

```azurecli
az ml online-endpoint delete --name tfserving-endpoint
```

```azurecli
az ml model delete -n tfserving-mounted --version 1
```

## <a name="next-steps"></a>Pasos siguientes

- [Implementación segura para puntos de conexión en línea (versión preliminar)](how-to-safely-rollout-managed-endpoints.md)
- [Solución de problemas de implementación de puntos de conexión en línea administrados](./how-to-troubleshoot-online-endpoints.md)
- [Ejemplo de Torchserve](https://github.com/Azure/azureml-examples/blob/main/cli/deploy-torchserve.sh)