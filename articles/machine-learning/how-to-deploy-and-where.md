---
title: Implementación de modelos de Machine Learning
titleSuffix: Azure Machine Learning
description: Aprenda cómo y dónde implementar modelos de aprendizaje automático. Realice implementaciones en Azure Container Instances, Azure Kubernetes Service y FPGA.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: ssambare
author: shivanissambare
ms.date: 11/12/2021
ms.topic: how-to
ms.custom: devx-track-python, deploy, devx-track-azurecli, contperf-fy21q2, contperf-fy21q4, mktng-kw-nov2021
adobe-target: true
ms.openlocfilehash: c24aa6fd09539c1bd9536ba0cecb3815edb79e2f
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132488190"
---
# <a name="deploy-machine-learning-models-to-azure"></a>Implementación de modelos de aprendizaje automático en Azure 

Aprenda a implementar el modelo de aprendizaje automático o aprendizaje profundo como un servicio web en la nube de Azure.

[!INCLUDE [endpoints-option](../../includes/machine-learning-endpoints-preview-note.md)]

## <a name="workflow-for-deploying-a-model"></a>Flujo de trabajo para implementar un modelo

El flujo de trabajo es siempre parecido independientemente de donde implemente el modelo:

1. Registre el modelo.
1. Prepare un script de entrada.
1. Prepare una configuración de inferencia.
1. Implemente el modelo localmente para asegurarse de que todo funciona.
1. Elija un destino de proceso.
1. Implemente el modelo en la nube.
1. Pruebe el servicio web resultante.

Para más información sobre los conceptos implicados en el flujo de trabajo de implementación del aprendizaje automático, consulte [Administración, implementación y supervisión de modelos con Azure Machine Learning](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Requisitos previos

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

[!INCLUDE [cli10-only](../../includes/machine-learning-cli-version-1-only.md)]

- Un área de trabajo de Azure Machine Learning. Para más información, consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).
- Un modelo. En los ejemplos de este artículo se usa un modelo entrenado previamente.
- Una máquina que puede ejecutar Docker, como una [instancia de proceso.](how-to-create-manage-compute-instance.md)

# <a name="python"></a>[Python](#tab/python)

- Un área de trabajo de Azure Machine Learning. Para más información, consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).
- Un modelo. En los ejemplos de este artículo se usa un modelo entrenado previamente.
- El [kit de desarrollo de software (SDK) de Azure Machine Learning para Python](/python/api/overview/azure/ml/intro).
- Una máquina que puede ejecutar Docker, como una [instancia de proceso](how-to-create-manage-compute-instance.md).
---

## <a name="connect-to-your-workspace"></a>Conexión con su área de trabajo

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

Para ver las áreas de trabajo a las que tiene acceso, use los siguientes comandos:

```azurecli-interactive
az login
az account set -s <subscription>
az ml workspace list --resource-group=<resource-group>
```

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core import Workspace
ws = Workspace(subscription_id="<subscription_id>",
               resource_group="<resource_group>",
               workspace_name="<workspace_name>")
```

Para más información sobre el uso del SDK para conectarse a un área de trabajo, consulte la documentación del [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/intro#workspace).


---

## <a name="register-the-model"></a><a id="registermodel"></a> Registro del modelo

En una situación común, un servicio de aprendizaje automático implementado necesita los siguientes componentes:
    
+ Recursos que representan el modelo específico que quiere implementar (por ejemplo: un archivo de modelo de PyTorch).
+ Código que se va a ejecutar en el servicio, que ejecuta el modelo en una entrada determinada.

Azure Machine Learning le permite separar la implementación en dos componentes independientes, de modo que pueda mantener el mismo código y simplemente actualizar el modelo. Definimos el mecanismo por el que se carga un modelo _por separado_ del código como "registrar el modelo".

Al registrar un modelo, se carga el modelo en la nube (en la cuenta de almacenamiento predeterminada del área de trabajo) y, a continuación, se monta en el mismo proceso en el que se ejecuta el servicio web.

En los ejemplos siguientes se muestra cómo registrar un modelo.

[!INCLUDE [trusted models](../../includes/machine-learning-service-trusted-model.md)]

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

Los comandos siguientes descargan un modelo y luego lo registran en el área de trabajo de Azure Machine Learning:

```azurecli-interactive
wget https://aka.ms/bidaf-9-model -O model.onnx --show-progress
az ml model register -n bidaf_onnx \
    -p ./model.onnx \
    -g <resource-group> \
    -w <workspace-name>
```

Establezca `-p` en la ruta de acceso de una carpeta o un archivo que desee registrar.

Para obtener más información sobre `az ml model register`, vea la [documentación de referencia](/cli/azure/ml(v1)/model).

### <a name="register-a-model-from-an-azure-ml-training-run"></a>Registro de un modelo a partir de una ejecución de entrenamiento de Azure ML

Si necesita registrar un modelo creado anteriormente por medio de un trabajo de entrenamiento de Azure Machine Learning, puede especificar el experimento, la ejecución y la ruta de acceso al modelo:

```azurecli-interactive
az ml model register -n bidaf_onnx --asset-path outputs/model.onnx --experiment-name myexperiment --run-id myrunid --tag area=qna
```

El parámetro `--asset-path` hace referencia a la ubicación del modelo en la nube. En este ejemplo, se usa la ruta de acceso de un único archivo. Para incluir varios archivos en el registro del modelo, establezca `--asset-path` en la ruta de acceso de una carpeta que contiene los archivos.

Para obtener más información sobre `az ml model register`, vea la [documentación de referencia](/cli/azure/ml(v1)/model).

# <a name="python"></a>[Python](#tab/python)

### <a name="register-a-model-from-a-local-file"></a>Registrar un modelo desde un archivo local

Puede registrar un modelo proporcionando la ruta de acceso local de dicho modelo. Puede proporcionar la ruta de acceso de una carpeta o un único archivo en la máquina local.
<!-- pyhton nb call -->
[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=register-model-from-local-file-code)]


Para incluir varios archivos en el registro del modelo, establezca `model_path` en la ruta de acceso de una carpeta que contiene los archivos.

Para más información, consulte la documentación de la [clase Model](/python/api/azureml-core/azureml.core.model.model).


### <a name="register-a-model-from-an-azure-ml-training-run"></a>Registro de un modelo a partir de una ejecución de entrenamiento de Azure ML

  Al usar el SDK para entrenar un modelo, puede recibir un objeto [Run](/python/api/azureml-core/azureml.core.run.run) o [AutoMLRun](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun), en función de cómo haya entrenado el modelo. Cada objeto se puede usar para registrar un modelo creado por una ejecución del experimento.

  + Registre un modelo desde un objeto `azureml.core.Run`:
 
    ```python
    model = run.register_model(model_name='bidaf_onnx',
                               tags={'area': 'qna'},
                               model_path='outputs/model.onnx')
    print(model.name, model.id, model.version, sep='\t')
    ```

    El parámetro `model_path` hace referencia a la ubicación del modelo en la nube. En este ejemplo, se usa la ruta de acceso de un único archivo. Para incluir varios archivos en el registro del modelo, establezca `model_path` en la ruta de acceso de una carpeta que contiene los archivos. Para obtener más información, consulte la documentación [Run.register_model](/python/api/azureml-core/azureml.core.run.run#register-model-model-name--model-path-none--tags-none--properties-none--model-framework-none--model-framework-version-none--description-none--datasets-none--sample-input-dataset-none--sample-output-dataset-none--resource-configuration-none----kwargs-).

  + Registre un modelo desde un objeto `azureml.train.automl.run.AutoMLRun`:

    ```python
    description = 'My AutoML Model'
    model = run.register_model(description = description,
                                tags={'area': 'qna'})

    print(run.model_id)
    ```

    En este ejemplo, los parámetros `metric` y `iteration`, no se especifican, por lo que se registrará la iteración con la mejor métrica principal. Se utiliza el valor `model_id` devuelto de la ejecución en lugar de un nombre de modelo.

    Para más información, consulte la documentación de [AutoMLRun.register_model](/python/api/azureml-train-automl-client/azureml.train.automl.run.automlrun#register-model-model-name-none--description-none--tags-none--iteration-none--metric-none-).

    Para implementar un modelo registrado con `AutoMLRun`, se recomienda hacerlo a través del [botón de implementación con un solo clic en el Estudio de Azure Machine Learning](how-to-use-automated-ml-for-ml-models.md#deploy-your-model). 

---

## <a name="define-a-dummy-entry-script"></a>Definición de un script de entrada ficticio

El script de entrada recibe los datos enviados a un servicio web implementado y los pasa al modelo. Seguidamente, devuelve la respuesta del modelo al cliente. *El script es específico para su modelo*. El script de entrada debe entender los datos que el modelo espera y devuelve.

Estas son las dos tareas que debe realizar en el script de entrada:

1. Cargar el modelo (mediante una función llamada `init()`)
1. Ejecutar el modelo sobre los datos de entrada (mediante una función llamada `run()`)

Para la implementación inicial, use un script de entrada ficticio que imprima los datos que recibe.

:::code language="python" source="~/azureml-examples-main/python-sdk/tutorials/deploy-local/source_dir/echo_score.py":::

Guarde este archivo como `echo_score.py` dentro de un directorio denominado `source_dir`. Este script ficticio devuelve los datos que le envía, por lo que no usa el modelo. Pero resulta útil para probar que el script de puntuación se está ejecutando.

## <a name="define-an-inference-configuration"></a>Definición de una configuración de inferencia

Una configuración de inferencia describe el contenedor de Docker y los archivos que se usarán al inicializar el servicio web. Todos los archivos del directorio de origen, incluidos los subdirectorios, se comprimirán y cargarán en la nube al implementar el servicio web.

La configuración de inferencia siguiente especifica que la implementación del aprendizaje automático usará el archivo `echo_score.py` del directorio `./source_dir` para procesar las solicitudes entrantes y que usará la imagen de Docker con los paquetes de Python especificados en el entorno `project_environment`.

Puede usar cualquier [entorno mantenido de inferencia de Azure Machine Learning](concept-prebuilt-docker-images-inference.md#list-of-prebuilt-docker-images-for-inference) como imagen base de Docker al crear el entorno del proyecto. Instalaremos las dependencias necesarias en la parte superior y almacenaremos la imagen de Docker resultante en el repositorio asociado al área de trabajo.

> [!NOTE]
> La carga del [directorio de origen de inferencia](/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py#constructor&preserve-view=true) de Azure Machine Learning no respeta **.gitignore** ni **.amlignore**.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

Una configuración de inferencia mínima se puede escribir de la siguiente manera:

:::code language="json" source="~/azureml-examples-main/python-sdk/tutorials/deploy-local/dummyinferenceconfig.json":::

Guarde el archivo con el nombre `dummyinferenceconfig.json`.


[Consulte este artículo](./reference-azure-machine-learning-cli.md#inference-configuration-schema) para obtener una explicación más detallada de las configuraciones de inferencia. 

# <a name="python"></a>[Python](#tab/python)

En el ejemplo siguiente se muestra cómo crear un entorno mínimo sin dependencias de PIP, mediante el script de puntuación ficticio que definió anteriormente.

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=inference-configuration-code)]

Para obtener más información sobre los entornos, consulte el tema sobre la [creación y administración de entornos de entrenamiento e implementación](how-to-use-environments.md).

Para más información sobre la configuración de inferencia, consulte la documentación de la clase [InferenceConfig](/python/api/azureml-core/azureml.core.model.inferenceconfig).

---


## <a name="define-a-deployment-configuration"></a>Definición de una configuración de implementación

Una configuración de implementación especifica la cantidad de memoria y los núcleos que necesita el servicio web para ejecutarse. También proporciona detalles de configuración del servicio web subyacente. Por ejemplo, una configuración de implementación permite especificar que el servicio necesita 2 gigabytes de memoria, 2 núcleos de CPU, 1 núcleo de GPU y que desea habilitar el escalado automático.

Las opciones disponibles para una configuración de implementación varían en función del destino de proceso que elija. En una implementación local, lo único que puede especificar es en qué puerto se va a atender el servicio web.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

[!INCLUDE [aml-local-deploy-config](../../includes/machine-learning-service-local-deploy-config.md)]

Para obtener más información, vea el [esquema de implementación](./reference-azure-machine-learning-cli.md#deployment-configuration-schema).

# <a name="python"></a>[Python](#tab/python)

El siguiente código de Python muestra cómo crear una configuración de implementación local: 

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=deployment-configuration-code)]

---

## <a name="deploy-your-machine-learning-model"></a>Implementación del modelo de aprendizaje automático

Ahora está preparado para implementar el modelo. 

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

Reemplace `bidaf_onnx:1` por el nombre del modelo y su número de versión.

```azurecli-interactive
az ml model deploy -n myservice \
    -m bidaf_onnx:1 \
    --overwrite \
    --ic dummyinferenceconfig.json \
    --dc deploymentconfig.json \
    -g <resource-group> \
    -w <workspace-name>
```

# <a name="python"></a>[Python](#tab/python)


[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=deploy-model-code)]

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=deploy-model-print-logs)]

Para más información, consulte la documentación de [Model.deploy()](/python/api/azureml-core/azureml.core.model.model#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) y [Webservice](/python/api/azureml-core/azureml.core.webservice.webservice).

---

## <a name="call-into-your-model"></a>Llamada al modelo

Vamos a comprobar que el modelo de eco se implementó correctamente. Debe poder realizar una solicitud de ejecución simple, así como una solicitud de puntuación:

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

```azurecli-interactive
curl -v http://localhost:32267
curl -v -X POST -H "content-type:application/json" \
    -d '{"query": "What color is the fox", "context": "The quick brown fox jumped over the lazy dog."}' \
    http://localhost:32267/score
```

# <a name="python"></a>[Python](#tab/python)
<!-- python nb call -->
[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=call-into-model-code)]

---

## <a name="define-an-entry-script"></a>Definición de un script de entrada

Ahora es el momento de cargar realmente el modelo. En primer lugar, modifique el script de entrada:


:::code language="python" source="~/azureml-examples-main/python-sdk/tutorials/deploy-local/source_dir/score.py":::

Guarde este archivo como `score.py` dentro de `source_dir`.

Observe el uso de la variable de entorno `AZUREML_MODEL_DIR` para buscar el modelo registrado. Ahora que ha agregado algunos paquetes pip.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

:::code language="json" source="~/azureml-examples-main/python-sdk/tutorials/deploy-local/inferenceconfig.json":::

Guarde este archivo como `inferenceconfig.json`. 

# <a name="python"></a>[Python](#tab/python)

```python
env = Environment(name='myenv')
python_packages = ['nltk', 'numpy', 'onnxruntime']
for package in python_packages:
    env.python.conda_dependencies.add_pip_package(package)

inference_config = InferenceConfig(environment=env, source_directory='./source_dir', entry_script='./score.py')
```

Para más información, consulte la documentación de [LocalWebservice](/python/api/azureml-core/azureml.core.webservice.local.localwebservice), [Model.deploy()](/python/api/azureml-core/azureml.core.model.model#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) y [Webservice](/python/api/azureml-core/azureml.core.webservice.webservice).

---

## <a name="deploy-again-and-call-your-service"></a>Nueva implementación y llamada al servicio

Vuelva a implementar el servicio:

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

Reemplace `bidaf_onnx:1` por el nombre del modelo y su número de versión.

```azurecli-interactive
az ml model deploy -n myservice \
    -m bidaf_onnx:1 \
    --overwrite \
    --ic inferenceconfig.json \
    --dc deploymentconfig.json \
    -g <resource-group> \
    -w <workspace-name>
```

# <a name="python"></a>[Python](#tab/python)

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=re-deploy-model-code)]

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=re-deploy-model-print-logs)]

Para más información, consulte la documentación de [Model.deploy()](/python/api/azureml-core/azureml.core.model.model#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) y [Webservice](/python/api/azureml-core/azureml.core.webservice.webservice).

---
A continuación, asegúrese de que puede enviar una solicitud post al servicio:

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

```azurecli-interactive
curl -v -X POST -H "content-type:application/json" \
    -d '{"query": "What color is the fox", "context": "The quick brown fox jumped over the lazy dog."}' \
    http://localhost:32267/score
```

# <a name="python"></a>[Python](#tab/python)

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=send-post-request-code)]

---

## <a name="choose-a-compute-target"></a>Elección de un destino de proceso

[!INCLUDE [aml-deploy-target](../../includes/aml-compute-target-deploy.md)]

## <a name="deploy-to-cloud"></a>Implementación en la nube

Una vez que haya confirmado que el servicio funciona localmente y ha elegido un destino de proceso remoto, estará listo para la implementación en la nube. 

Cambie la configuración de implementación para que se corresponda con el destino de proceso que ha elegido, en este caso Azure Container Instances:

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

Las opciones disponibles para una configuración de implementación varían en función del destino de proceso que elija.

:::code language="json" source="~/azureml-examples-main/python-sdk/tutorials/deploy-local/re-deploymentconfig.json":::

Guarde el archivo como `re-deploymentconfig.json`.

Para más información, consulte [esta referencia](./reference-azure-machine-learning-cli.md#deployment-configuration-schema).

# <a name="python"></a>[Python](#tab/python)

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=deploy-model-on-cloud-code)]

---

Vuelva a implementar el servicio:


# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)

Reemplace `bidaf_onnx:1` por el nombre del modelo y su número de versión.

```azurecli-interactive
az ml model deploy -n myservice \
    -m bidaf_onnx:1 \
    --overwrite \
    --ic inferenceconfig.json \
    --dc re-deploymentconfig.json \
    -g <resource-group> \
    -w <workspace-name>
```

Para ver los registros del servicio, use el siguiente comando:

```azurecli-interactive
az ml service get-logs -n myservice \
    -g <resource-group> \
    -w <workspace-name>
```

# <a name="python"></a>[Python](#tab/python)


[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=re-deploy-service-code)]

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=re-deploy-service-print-logs)]

Para más información, consulte la documentación de [Model.deploy()](/python/api/azureml-core/azureml.core.model.model#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) y [Webservice](/python/api/azureml-core/azureml.core.webservice.webservice).

---


## <a name="call-your-remote-webservice"></a>Llamada al servicio web remoto

Al implementar de forma remota, es posible que tenga habilitada la autenticación de claves. En el ejemplo siguiente se muestra cómo obtener la clave de servicio con Python para realizar una solicitud de inferencia.

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=call-remote-web-service-code)]

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=call-remote-webservice-print-logs)]



Consulte el artículo sobre [aplicaciones cliente para consumir servicios web](how-to-consume-web-service.md) para obtener más clientes de ejemplo en otros lenguajes.

### <a name="understanding-service-state"></a>Descripción del estado del servicio

Durante la implementación del modelo, es posible que vea el cambio de estado del servicio mientras se implementa por completo.

En la tabla siguiente se describen los diferentes estados del servicio:

| Estado de WebService | Descripción | ¿Estado final?
| ----- | ----- | ----- |
| En transición | El servicio está en proceso de implementación. | No |
| Unhealthy (Incorrecto) | El servicio se ha implementado pero actualmente no se puede acceder a él.  | No |
| No programable | El servicio no se puede implementar en este momento debido a la falta de recursos. | No |
| Con error | No se pudo implementar el servicio debido a un error o a un bloqueo. | Sí |
| Healthy | El servicio está en buen estado y el punto de conexión está disponible. | Sí |

> [!TIP]
> Al realizar la implementación, las imágenes de Docker de los destinos de proceso se crean y se cargan desde Azure Container Registry (ACR). De manera predeterminada, Azure Machine Learning crea una instancia de ACR que usa el nivel de servicio *Básico*. Si se cambia la instancia de ACR del área de trabajo al nivel Estándar o Prémium, puede reducirse el tiempo que se tarda en compilar e implementar imágenes en los destinos de proceso. Para más información, consulte [Niveles de servicio de Azure Container Registry](../container-registry/container-registry-skus.md).

> [!NOTE]
> Si va a implementar un modelo en Azure Kubernetes Service (AKS), le recomendamos que habilite [Azure Monitor](../azure-monitor/containers/container-insights-enable-existing-clusters.md) para ese clúster. Esto le ayudará a comprender el estado general y el uso de recursos del clúster. Puede que le resulten de utilidad los siguientes recursos:
>
> * [Comprobación de eventos de Resource Health que afectan al clúster de AKS (versión preliminar)](../aks/aks-resource-health.md)
> * [Introducción a los diagnósticos de Azure Kubernetes Service (versión preliminar)](../aks/concepts-diagnostics.md)
>
> Si intenta implementar un modelo en un clúster con un estado incorrecto o sobrecargado, se espera que experimente problemas. Si necesita ayuda para solucionar problemas del clúster de AKS, póngase en contacto con el departamento de soporte técnico de AKS.

## <a name="delete-resources"></a>Eliminar recursos

# <a name="azure-cli"></a>[CLI de Azure](#tab/azcli)


[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/2.deploy-local-cli.ipynb?name=delete-resource-code)]

```azurecli-interactive
az ml service delete -n myservice
az ml service delete -n myaciservice
az ml model delete --model-id=<MODEL_ID>
```

Para eliminar un servicio web implementado, use `az ml service delete <name of webservice>`.

Para eliminar un modelo registrado del área de trabajo, use `az ml model delete <model id>`.

Más información sobre la [eliminación de un servicio web](/cli/azure/ml(v1)/computetarget/create#az_ml_service_delete) y la [eliminación de un modelo](/cli/azure/ml/model#az_ml_model_delete).

# <a name="python"></a>[Python](#tab/python)

[!Notebook-python[] (~/azureml-examples-main/python-sdk/tutorials/deploy-local/1.deploy-local.ipynb?name=delete-resource-code)]

Para eliminar un servicio web implementado, use `service.delete()`.
Para eliminar un modelo registrado, use `model.delete()`.

Para más información, consulte la documentación para [WebService.delete()](/python/api/azureml-core/azureml.core.webservice%28class%29#delete--) y [Model.delete()](/python/api/azureml-core/azureml.core.model.model#delete--).

---

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de implementaciones con errores](how-to-troubleshoot-deployment.md)
* [Actualización de servicio web](how-to-deploy-update-web-service.md)
* [Implementación con un solo clic de las ejecuciones de aprendizaje automático automatizado en el Estudio de Azure Machine Learning](how-to-use-automated-ml-for-ml-models.md#deploy-your-model)
* [Uso de TLS para proteger un servicio web con Azure Machine Learning](how-to-secure-web-service.md)
* [Supervisión de los modelos de Azure Machine Learning con Application Insights](how-to-enable-app-insights.md)
* [Creación de flujos de trabajo de aprendizaje automático controlados por eventos (versión preliminar)](how-to-use-event-grid.md)
