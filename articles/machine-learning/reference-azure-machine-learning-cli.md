---
title: Instalación y uso de la CLI de Azure Machine Learning
description: Aprenda a usar la extensión de la CLI de Azure para ML para crear y administrar recursos, como el área de trabajo, los almacenes de datos, los conjuntos de datos, las canalizaciones, los modelos y las implementaciones.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: jordane
author: jpe316
ms.date: 04/02/2021
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: 41f12d9421a49cb926a666469beda97cd8f53eac
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131559739"
---
# <a name="install--use-the-cli-extension-for-azure-machine-learning"></a>Instalación y uso de la extensión de la CLI para Azure Machine Learning

[!INCLUDE [cli-version-info](../../includes/machine-learning-cli-version-1-only.md)]

La CLI de Azure Machine Learning es una extensión de la [CLI de Azure](/cli/azure/), una interfaz de la línea de comandos multiplataforma para la plataforma Azure. Esta extensión proporciona comandos para trabajar con Azure Machine Learning. Permite automatizar las actividades de aprendizaje automático. En esta lista se muestran algunas acciones de ejemplo que puede realizar con la extensión de la CLI:

+ Realizar experimentos para crear modelos de aprendizaje automático

+ Registrar los modelos de aprendizaje automático para el uso del cliente

+ Empaquetar, implementar y realizar un seguimiento del ciclo de vida de los modelos de aprendizaje automático

La CLI no sustituye al SDK de Azure Machine Learning. Es una herramienta complementaria que está optimizada para administrar tareas con muchos parámetros y que están bien adaptadas para la automatización.

## <a name="prerequisites"></a>Prerrequisitos

* Para usar la CLI, debe tener una suscripción de Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://azure.microsoft.com/free/).

* Para usar los comandos de la CLI de este documento desde su **entorno local**, necesita la [CLI de Azure](/cli/azure/install-azure-cli).

    Si usa el [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/), la CLI es accesible a través del explorador y reside en la nube.

## <a name="full-reference-docs"></a>Documentos de referencia completos

Busque los [documentos de referencia completos sobre la extensión azure-cli-ml de la CLI de Azure](/cli/azure/ml(v1)/).

## <a name="connect-the-cli-to-your-azure-subscription"></a>Conexión de la CLI a una suscripción de Azure

> [!IMPORTANT]
> Puede omitir esta sección si usa Azure Cloud Shell. Cloud Shell se autentica automáticamente mediante la cuenta con la que inicia sesión en su suscripción a Azure.

Hay varias maneras de autenticarse en la suscripción a Azure desde la CLI. La manera más básica consiste en autenticarse interactivamente a través de un explorador. Para autenticarse de forma interactiva, abra una línea de comandos o un terminal y use el siguiente comando:

```azurecli-interactive
az login
```

Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión. De lo contrario, tendrá que abrir un explorador y seguir las instrucciones de la línea de comandos. Las instrucciones implican navegar a [https://aka.ms/devicelogin](https://aka.ms/devicelogin) y escribir un código de autorización.

[!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)]

Para obtener otros métodos de autenticación, consulte [Inicio de sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli).

## <a name="install-the-extension"></a>Instalación de la extensión

Para instalar la extensión de la CLI (v1):
```azurecli-interactive
az extension add -n azure-cli-ml
```

## <a name="update-the-extension"></a>Actualización de la extensión

Para actualizar la extensión de la CLI de Machine Learning, use el siguiente comando:

```azurecli-interactive
az extension update -n azure-cli-ml
```

## <a name="remove-the-extension"></a>Eliminación de la extensión

Para eliminar la extensión de la CLI, use el siguiente comando:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Administración de recursos

Los siguientes comandos muestran cómo utilizar la CLI para administrar los recursos que usa Azure Machine Learning.

+ Cree un grupo de recursos si todavía no tiene uno:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Cree un área de trabajo de Azure Machine Learning:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

    Para más información, consulte [az ml workspace create](/cli/azure/ml/workspace#az_ml_workspace_create).

+ Adjunte una configuración de área de trabajo a una carpeta para permitir el reconocimiento contextual de la CLI.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Este comando crea un subdirectorio `.azureml` que contiene archivos de entorno de conda y runconfig de ejemplo. También contiene un archivo `config.json` que se usa para comunicarse con el área de trabajo de Azure Machine Learning.

    Para obtener más información, consulte [az ml folder attach](/cli/azure/ml(v1)/folder#az_ml_folder_attach).

+ Adjunte un contenedor de blobs de Azure como almacén de datos.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

    Para más información, consulte [az ml datastore attach-blob](/cli/azure/ml/datastore#az_ml_datastore_attach-blob).

+ Cargue archivos a un almacén de archivos.

    ```azurecli-interactive
    az ml datastore upload  -n datastorename -p sourcepath
    ```

    Para más información, consulte [az ml datastore upload](/cli/azure/ml/datastore#az_ml_datastore_upload).

+ Adjunte un clúster de AKS como destino de proceso.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myresourcegroup -w myworkspace
    ```

    Para más información, consulte [az ml computetarget attach aks](/cli/azure/ml(v1)/computetarget/attach#az_ml_computetarget_attach-aks)

### <a name="compute-clusters"></a>Clústeres de proceso

+ Cree un nuevo clúster de proceso administrado.

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```



+ Cree un nuevo clúster de proceso administrado con identidad administrada.

  + Identidad administrada asignada por el usuario

    ```azurecli
    az ml computetarget create amlcompute --name cpu-cluster --vm-size Standard_NC6 --max-nodes 5 --assign-identity '/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'
    ```

  + Identidad administrada asignada por el sistema

    ```azurecli
    az ml computetarget create amlcompute --name cpu-cluster --vm-size Standard_NC6 --max-nodes 5 --assign-identity '[system]'
    ```
+ Agregue una identidad administrada a un clúster existente:

    + Identidad administrada asignada por el usuario
        ```azurecli
        az ml computetarget amlcompute identity assign --name cpu-cluster '/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'
        ```
    + Identidad administrada asignada por el sistema

        ```azurecli
        az ml computetarget amlcompute identity assign --name cpu-cluster '[system]'
        ```

Para más información, consulte [az ml computetarget create amlcompute](/cli/azure/ml(v1)/computetarget/create#az_ml_computetarget_create_amlcompute).

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-managed-identity-note.md)]

<a id="computeinstance"></a>

### <a name="compute-instance"></a>Instancia de proceso
Administrar instancias de proceso.  En todos los siguientes ejemplos, el nombre de la instancia de proceso es **CPU**

+ Crear una nueva computeinstance.

    ```azurecli-interactive
    az ml computetarget create computeinstance -n cpu -s "STANDARD_D3_V2" -v
    ```

    Para más información, consulte [az ml computetarget create computeinstance](/cli/azure/ml(v1)/computetarget/create#az_ml_computetarget_create_computeinstance).

+ Detener una computeinstance.

    ```azurecli-interactive
    az ml computetarget computeinstance stop -n cpu -v
    ```

    Para obtener más información, consulte [az ml computetarget computeinstance stop](/cli/azure/ml(v1)/computetarget/computeinstance#az_ml_computetarget_computeinstance_stop).

+ Iniciar una computeinstance.

    ```azurecli-interactive
    az ml computetarget computeinstance start -n cpu -v
    ```

    Para obtener más información, consulte [az ml computetarget computeinstance start](/cli/azure/ml(v1)/computetarget/computeinstance#az_ml_computetarget_computeinstance_start).

+ Reiniciar una computeinstance.

    ```azurecli-interactive
    az ml computetarget computeinstance restart -n cpu -v
    ```

    Para obtener más información, consulte [az ml computetarget computeinstance restart](/cli/azure/ml(v1)/computetarget/computeinstance#az_ml_computetarget_computeinstance_restart).

+ Elimine una computeinstance.

    ```azurecli-interactive
    az ml computetarget delete -n cpu -v
    ```

    Para más información, consulte [az ml computetarget delete computeinstance](/cli/azure/ml(v1)/computetarget#az_ml_computetarget_delete).


## <a name="run-experiments"></a><a id="experiments"></a>Ejecución de los experimentos

* Inicie la ejecución del experimento. Cuando use este comando, especifique el nombre del archivo runconfig (el texto que va antes de \*.runconfig, si mira el sistema de archivo) con respecto al parámetro -c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > El comando `az ml folder attach` crea un subdirectorio `.azureml`, que contiene dos archivos runconfig de ejemplo. 
    >
    > Si tiene un script de Python que crea un objeto de configuración de ejecución mediante programación, puede usar [RunConfig.save()](/python/api/azureml-core/azureml.core.runconfiguration#save-path-none--name-none--separate-environment-yaml-false-) para guardarlo como un archivo runconfig.
    >
    > El esquema runconfig completo se puede encontrar en este [archivo JSON](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json). El esquema se documenta automáticamente mediante la clave `description` de cada objeto. Además, hay enumeraciones de valores posibles y un fragmento de código de plantilla al final.

    Para obtener más información, consulte [az ml run submit-script](/cli/azure/ml(v1)/run#az_ml_run_submit_script).

* Vea una lista de los experimentos:

    ```azurecli-interactive
    az ml experiment list
    ```

    Para obtener más información, consulte [az ml experiment list](/cli/azure/ml(v1)/experiment#az_ml_experiment_list).

### <a name="hyperdrive-run"></a>Ejecución de HyperDrive

Puede usar HyperDrive con la CLI de Azure para realizar ejecuciones de ajuste de parámetros. En primer lugar, cree un archivo de configuración de HyperDrive con el siguiente formato. Consulte el artículo sobre el [ajuste de hiperparámetros del modelo](how-to-tune-hyperparameters.md) para obtener más información sobre los parámetros de ajuste de hiperparámetros.

```yml
# hdconfig.yml
sampling: 
    type: random # Supported options: Random, Grid, Bayesian
    parameter_space: # specify a name|expression|values tuple for each parameter.
    - name: --penalty # The name of a script parameter to generate values for.
      expression: choice # supported options: choice, randint, uniform, quniform, loguniform, qloguniform, normal, qnormal, lognormal, qlognormal
      values: [0.5, 1, 1.5] # The list of values, the number of values is dependent on the expression specified.
policy: 
    type: BanditPolicy # Supported options: BanditPolicy, MedianStoppingPolicy, TruncationSelectionPolicy, NoTerminationPolicy
    evaluation_interval: 1 # Policy properties are policy specific. See the above link for policy specific parameter details.
    slack_factor: 0.2
primary_metric_name: Accuracy # The metric used when evaluating the policy
primary_metric_goal: Maximize # Maximize|Minimize
max_total_runs: 8 # The maximum number of runs to generate
max_concurrent_runs: 2 # The number of runs that can run concurrently.
max_duration_minutes: 100 # The maximum length of time to run the experiment before cancelling.
```

Agregue este archivo junto con los archivos de configuración de ejecución. A continuación, envíe una ejecución de HyperDrive mediante:
```azurecli
az ml run submit-hyperdrive -e <experiment> -c <runconfig> --hyperdrive-configuration-name <hdconfig> my_train.py
```

Observe la sección *arguments* (argumentos) en runconfig y el *espacio de parámetros* en la configuración de HyperDrive. Contienen los argumentos de la línea de comandos que se van a pasar al script de aprendizaje. El valor de runconfig permanece igual para cada iteración, mientras que el intervalo de la configuración de HyperDrive se repite en interación. No especifique el mismo argumento en ambos archivos.

## <a name="dataset-management"></a>Administración de conjuntos de datos

Los siguientes comandos muestran cómo trabajar con conjuntos de datos en Azure Machine Learning:

+ Registre un conjunto de datos:

    ```azurecli-interactive
    az ml dataset register -f mydataset.json
    ```

    Para obtener información sobre el formato del archivo JSON que se usa para definir el conjunto de datos, use `az ml dataset register --show-template`.

    Para más información, consulte [az ml dataset register](/cli/azure/ml(v1)/dataset#az_ml_dataset_register).

+ Enumerar todos los conjuntos de datos de un área de trabajo:

    ```azurecli-interactive
    az ml dataset list
    ```

    Para más información, consulte [az ml dataset list](/cli/azure/ml(v1)/dataset#az_ml_dataset_list).

+ Obtenga los detalles de un conjunto de datos:

    ```azurecli-interactive
    az ml dataset show -n dataset-name
    ```

    Para más información, consulte [az ml dataset show](/cli/azure/ml(v1)/dataset#az_ml_dataset_show).

+ Anule el registro de un conjunto de datos:

    ```azurecli-interactive
    az ml dataset unregister -n dataset-name
    ```

    Para más información, consulte [az ml dataset unregister](/cli/azure/ml(v1)/dataset#az_ml_dataset_archive).

## <a name="environment-management"></a>Administrador de entornos

Los comandos siguientes muestran cómo crear, registrar y enumerar los [entornos](how-to-configure-environment.md) de Azure Machine Learning para el área de trabajo:

+ Creación de archivos de scaffolding para un entorno:

    ```azurecli-interactive
    az ml environment scaffold -n myenv -d myenvdirectory
    ```

    Para más información, consulte [az ml environment scaffold](/cli/azure/ml/environment#az_ml_environment_scaffold).

+ Registro de un entorno:

    ```azurecli-interactive
    az ml environment register -d myenvdirectory
    ```

    Para más información, consulte [az ml environment register](/cli/azure/ml/environment#az_ml_environment_register).

+ Lista de entornos registrados:

    ```azurecli-interactive
    az ml environment list
    ```

    Para más información, consulte [az ml environment list](/cli/azure/ml/environment#az_ml_environment_list).

+ Descargue un entorno registrado:

    ```azurecli-interactive
    az ml environment download -n myenv -d downloaddirectory
    ```

    Para más información, consulte [az ml environment download](/cli/azure/ml/environment#az_ml_environment_download).

### <a name="environment-configuration-schema"></a>Esquema de configuración del entorno

Si usa el comando `az ml environment scaffold`, se genera un archivo de plantilla `azureml_environment.json` que puede modificarse y usarse para crear configuraciones de entorno personalizadas con la CLI. El objeto de nivel superior se asigna de forma flexible a la clase [`Environment`](/python/api/azureml-core/azureml.core.environment%28class%29) en el SDK de Python. 

```json
{
    "name": "testenv",
    "version": null,
    "environmentVariables": {
        "EXAMPLE_ENV_VAR": "EXAMPLE_VALUE"
    },
    "python": {
        "userManagedDependencies": false,
        "interpreterPath": "python",
        "condaDependenciesFile": null,
        "baseCondaEnvironment": null
    },
    "docker": {
        "enabled": false,
        "baseImage": "mcr.microsoft.com/azureml/openmpi3.1.2-ubuntu18.04:20210615.v1",
        "baseDockerfile": null,
        "sharedVolumes": true,
        "shmSize": "2g",
        "arguments": [],
        "baseImageRegistry": {
            "address": null,
            "username": null,
            "password": null
        }
    },
    "spark": {
        "repositories": [],
        "packages": [],
        "precachePackages": true
    },
    "databricks": {
        "mavenLibraries": [],
        "pypiLibraries": [],
        "rcranLibraries": [],
        "jarLibraries": [],
        "eggLibraries": []
    },
    "inferencingStackVersion": null
}
```

En la tabla que aparece más abajo se detallan todos los campos de nivel superior del archivo JSON, su tipo y su descripción. Si un tipo de objeto está vinculado a una clase del SDK de Python, hay una coincidencia 1:1 débil entre cada campo JSON y el nombre de la variable pública en la clase de Python. En algunos casos, el campo puede asignarse a un argumento de constructor en lugar de a una variable de clase. Por ejemplo, el campo `environmentVariables` se asigna a la variable `environment_variables` en la clase [`Environment`](/python/api/azureml-core/azureml.core.environment%28class%29).

| Campo JSON | Tipo | Descripción |
|---|---|---|
| `name` | `string` | Nombre del entorno. No escriba **Microsoft** o **AzureML** al principio del nombre. |
| `version` | `string` | Versión del entorno. |
| `environmentVariables` | `{string: string}` | Mapa hash de nombres y valores de variables de entorno. |
| `python` | [`PythonSection`](/python/api/azureml-core/azureml.core.environment.pythonsection) que define el entorno de Python y el intérprete que se va a usar en el recurso de proceso de destino. |
| `docker` | [`DockerSection`](/python/api/azureml-core/azureml.core.environment.dockersection) | Sección que define la configuración para personalizar la imagen de Docker compilada según las especificaciones del entorno. |
| `spark` | [`SparkSection`](/python/api/azureml-core/azureml.core.environment.sparksection) | Sección que define la configuración de Spark. Solo se usa cuando el marco se establece en PySpark. |
| `databricks` | [`DatabricksSection`](/python/api/azureml-core/azureml.core.databricks.databrickssection) | Sección que define la configuración de las dependencias de las bibliotecas de Databricks. |
| `inferencingStackVersion` | `string` | Campo que especifica la versión de la pila de inferencia agregada a la imagen. Para evitar agregar una pila de inferencia, deje este campo como `null`. Valor válido: "latest" (más reciente). |

## <a name="ml-pipeline-management"></a>Administración de canalizaciones de aprendizaje automático

Los siguientes comandos muestran cómo trabajar con canalizaciones de aprendizaje automático:

+ Creación de una canalización de aprendizaje automático:

    ```azurecli-interactive
    az ml pipeline create -n mypipeline -y mypipeline.yml
    ```

    Para obtener más información, consulte [az ml pipeline create](/cli/azure/ml(v1)/pipeline#az_ml_pipeline_create).

    Para obtener más información sobre el archivo de canalización YAML, vea [Definición de canalizaciones de aprendizaje automático en YAML](reference-pipeline-yaml.md).

+ Ejecución de una canalización:

    ```azurecli-interactive
    az ml run submit-pipeline -n myexperiment -y mypipeline.yml
    ```

    Para obtener más información, consulte [az ml run submit-pipeline](/cli/azure/ml(v1)/run#az_ml_run_submit_pipeline).

    Para obtener más información sobre el archivo de canalización YAML, vea [Definición de canalizaciones de aprendizaje automático en YAML](reference-pipeline-yaml.md).

+ Programación de una canalización:

    ```azurecli-interactive
    az ml pipeline create-schedule -n myschedule -e myexperiment -i mypipelineid -y myschedule.yml
    ```

    Para obtener más información, consulte [az ml pipeline create-schedule](/cli/azure/ml(v1)/pipeline#az_ml_pipeline_create-schedule).

    Para obtener más información sobre el archivo YAML de programación de canalización, vea [Definición de canalizaciones de aprendizaje automático en YAML](reference-pipeline-yaml.md#schedules).

## <a name="model-registration-profiling-deployment"></a>Registro del modelo, generación de perfiles, implementación

Los siguientes comandos muestran cómo registrar un modelo entrenado e implementarlo como servicio de producción:

+ Registre un modelo con Azure Machine Learning:

    ```azurecli-interactive
    az ml model register -n mymodel -p sklearn_regression_model.pkl
    ```

    Para más información, consulte [az ml model register](/cli/azure/ml/model#az_ml_model_register).

+ **OPCIONAL** Genere un perfil del modelo para obtener los valores óptimos de CPU y memoria para la implementación.
    ```azurecli-interactive
    az ml model profile -n myprofile -m mymodel:1 --ic inferenceconfig.json -d "{\"data\": [[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}" -t myprofileresult.json
    ```

    Para más información, consulte [az ml model profile](/cli/azure/ml/model#az_ml_model_profile).

+ Implementación del modelo en AKS
    ```azurecli-interactive
    az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
    ```
    
    Para más información sobre el esquema del archivo de configuración de inferencia, consulte [Esquema de configuración de inferencia](#inferenceconfig).
    
    Para más información sobre el esquema del archivo de configuración de implementación, consulte [Esquema de configuración de implementación](#deploymentconfig).

    Para más información, consulte [az ml model deploy](/cli/azure/ml/model#az_ml_model_deploy).

<a id="inferenceconfig"></a>

## <a name="inference-configuration-schema"></a>Esquema de configuración de inferencia

[!INCLUDE [inferenceconfig](../../includes/machine-learning-service-inference-config.md)]

<a id="deploymentconfig"></a>

## <a name="deployment-configuration-schema"></a>Esquema de configuración de implementación

### <a name="local-deployment-configuration-schema"></a>Esquema de configuración de implementación local

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-local-deploy-config.md)]

### <a name="azure-container-instance-deployment-configuration-schema"></a>Esquema de configuración de implementación de Azure Container Instances 

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aci-deploy-config.md)]

### <a name="azure-kubernetes-service-deployment-configuration-schema"></a>Esquema de configuración de implementación de Azure Kubernetes Service

[!INCLUDE [deploymentconfig](../../includes/machine-learning-service-aks-deploy-config.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Referencia de comandos para la extensión de la CLI de Machine Learning](/cli/azure/ml).

* [Entrenamiento e implementación de modelos de aprendizaje automático mediante Azure Pipelines](/azure/devops/pipelines/targets/azure-machine-learning)
