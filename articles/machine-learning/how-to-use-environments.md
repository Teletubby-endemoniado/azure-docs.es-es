---
title: Uso de entornos de software
titleSuffix: Azure Machine Learning
description: Cree y administre entornos para el entrenamiento y la implementación de modelos. Administre paquetes de Python y otros valores para el entorno.
services: machine-learning
author: saachigopal
ms.author: sagopal
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.date: 08/11/2021
ms.topic: how-to
ms.custom: devx-track-python
ms.openlocfilehash: 845e852f2ef3155fce451f7e80f5c8f43eb8abf6
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129355220"
---
# <a name="create--use-software-environments-in-azure-machine-learning"></a>Creación y uso de entornos de software en Azure Machine Learning


En este artículo, aprenda a crear y administrar [entornos](/python/api/azureml-core/azureml.core.environment.environment) de Azure Machine Learning. Use los entornos para realizar un seguimiento de las dependencias de software de sus proyectos y reproducirlas a medida que evolucionan.

La administración de dependencias de software es una tarea común para los desarrolladores. Quiere asegurarse de que las compilaciones se puedan reproducir sin demasiada configuración manual del software. La clase `Environment` de Azure Machine Learning representa las soluciones para el desarrollo local, como PIP y Conda, y el desarrollo en la nube de Conda y distribuido mediante funcionalidades de Docker.

En los ejemplos de este artículo se muestran los siguientes procedimientos:

* Crear un entorno y especificar las dependencias de paquetes.
* Recuperar y actualizar los entornos.
* Usar un entorno para entrenamiento.
* Usar un entorno para la implementación de servicios web.

Para obtener información general de alto nivel sobre cómo funcionan los entornos en Azure Machine Learning, consulte [¿Qué son los entornos de ML?](concept-environments.md) Para obtener información sobre la configuración de entornos de desarrollo, consulte [este artículo](how-to-configure-environment.md).

## <a name="prerequisites"></a>Prerrequisitos

* El [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/install) (1.13.0 o posterior)
* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

## <a name="create-an-environment"></a>Creación de un entorno

En las secciones siguientes se exploran las diversas formas en que puede crear un entorno para sus experimentos.

### <a name="instantiate-an-environment-object"></a>Creación de instancias de un objeto de entorno

Para crear manualmente un entorno, importe la clase `Environment` desde el SDK. A continuación, use el siguiente código para crear instancias de un objeto de entorno.

```python
from azureml.core.environment import Environment
Environment(name="myenv")
```

### <a name="use-a-curated-environment"></a>Uso de un entorno mantenido

Los entornos seleccionados contienen colecciones de paquetes de Python y están disponibles en el área de trabajo de forma predeterminada. Estos entornos están respaldados por imágenes de Docker en caché, lo que reduce el costo de preparación de la ejecución. Para empezar, puede elegir uno de los entornos seleccionados populares: 

* El entorno _AzureML-lightgbm-3.2-ubuntu18.04-py37-cpu_ contiene Scikit-learn, LightGBM, XGBoost, Dask, así como otros SDK de Python de AzureML y paquetes adicionales.

* El entorno _AzureML-sklearn-0.24-ubuntu18.04-py37-cpu_ contiene paquetes de ciencia de datos comunes. Estos paquetes incluyen Scikit-Learn, Pandas, Matplotlib y un conjunto mayor de paquetes de azureml-sdk.

Para obtener una lista de los entornos seleccionados, consulte el [artículo sobre entornos seleccionados](resource-curated-environments.md).

Use el método `Environment.get` para seleccionar uno de los entornos mantenidos:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
env = Environment.get(workspace=ws, name="AzureML-sklearn-0.24-ubuntu18.04-py37-cpu")
```

Puede enumerar los entornos mantenidos y sus paquetes mediante el código siguiente:

```python
envs = Environment.list(workspace=ws)

for env in envs:
    if env.startswith("AzureML"):
        print("Name",env)
        print("packages", envs[env].python.conda_dependencies.serialize_to_string())
```

> [!WARNING]
>  No empiece el nombre de su propio entorno con el prefijo _AzureML_. Este prefijo está reservado para entornos mantenidos.

Para personalizar un entorno mantenido, clone y cambie el nombre del entorno. 
```python 
env = Environment.get(workspace=ws, name="AzureML-sklearn-0.24-ubuntu18.04-py37-cpu")
curated_clone = env.clone("customize_curated")
```


### <a name="use-conda-dependencies-or-pip-requirements-files"></a>Uso de dependencias de Conda o archivos de requisitos de PIP

Puede crear un entorno a partir de una especificación de Conda o de un archivo de requisitos de PIP. Use el método [`from_conda_specification()`](/python/api/azureml-core/azureml.core.environment.environment#from-conda-specification-name--file-path-) o el método [`from_pip_requirements()`](/python/api/azureml-core/azureml.core.environment.environment#from-pip-requirements-name--file-path-). En el argumento del método, incluya el nombre del entorno y la ruta de acceso del archivo que desee. 

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

# From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv",
                                          file_path = "path-to-pip-requirements-file")                                          
```

### <a name="enable-docker"></a>Habilitación de Docker

Azure Machine Learning compila una imagen de Docker y crea un entorno de Python en ese contenedor de acuerdo con las especificaciones. Las imágenes de Docker se almacenan en caché y se reutilizan: la primera vez que se ejecutan en un entorno nuevo suele tardar más, ya que la imagen debe compilarse. En las ejecuciones locales, especifique Docker en [RunConfiguration](/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py&preserve-view=true#variables). 

De manera predeterminada, la imagen de Docker recién creada aparece en el registro de contenedor que está asociado al área de trabajo.  El nombre del repositorio tiene el formato *azureml/azureml_\<uuid\>* . La parte del identificador único (*uuid*) del nombre corresponde a un hash calculado a partir de la configuración del entorno. Esta correspondencia permite que el servicio determine si ya existe para reutilizar una imagen del entorno dado.

#### <a name="use-a-prebuilt-docker-image"></a>Uso de una imagen de Docker precompilada

De forma predeterminada, el servicio usa automáticamente una de las [imágenes base](https://github.com/Azure/AzureML-Containers) basadas en Ubuntu Linux, en particular la definida por `azureml.core.environment.DEFAULT_CPU_IMAGE`. A continuación, instala los paquetes de Python especificados definidos por el entorno de Azure ML proporcionado. Hay otras imágenes base de GPU y CPU de Azure ML disponibles en el [repositorio](https://github.com/Azure/AzureML-Containers) de contenedor. También es posible usar una [imagen base de Docker personalizada](./how-to-deploy-custom-container.md).

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
```

>[!IMPORTANT]
> Azure Machine Learning solo admite imágenes de Docker que proporcionan el software siguiente:
> * Ubuntu 18.04 o posterior.
> * Conda 4.7.# o posterior.
> * Python 3.6+.
> * Se requiere un shell compatible con POSIX disponible en /bin/sh en cualquier imagen de contenedor usada para el entrenamiento. 

#### <a name="use-your-own-dockerfile"></a>Uso de un Dockerfile propio 

También puede especificar un Dockerfile personalizado. Es más sencillo empezar a partir de una de las imágenes base de Azure Machine Learning mediante el comando ```FROM``` de Docker y, después, agregar sus propios pasos personalizados. Use este enfoque si necesita instalar paquetes que no son de Python como dependencias. Recuerde establecer la imagen base en None.

Tenga en cuenta que Python es una dependencia implícita en Azure Machine Learning por lo que un dockerfile personalizado debe tener Python instalado.

```python
# Specify docker steps as a string. 
dockerfile = r"""
FROM mcr.microsoft.com/azureml/openmpi3.1.2-ubuntu18.04
RUN echo "Hello from custom container!"
"""

# Set base image to None, because the image is defined by dockerfile.
myenv.docker.base_image = None
myenv.docker.base_dockerfile = dockerfile

# Alternatively, load the string from a file.
myenv.docker.base_image = None
myenv.docker.base_dockerfile = "./Dockerfile"
```

Cuando se utilizan imágenes de Docker personalizadas, se recomienda anclar las versiones del paquete para garantizar mejor la reproducibilidad.

#### <a name="specify-your-own-python-interpreter"></a>Especificación de un intérprete de Python propio

En algunas situaciones, es posible que la imagen base personalizada ya contenga un entorno de Python con paquetes que quiera usar.

Para usar sus propios paquetes instalados y deshabilitar Conda, establezca el parámetro `Environment.python.user_managed_dependencies = True`. Asegúrese de que la imagen base contenga un intérprete de Python, así como los paquetes que necesita el script de entrenamiento.

Por ejemplo, para trabajar en un entorno base de Miniconda que tenga instalado el paquete NumPy, especifique primero un Dockerfile con un paso para instalar el paquete. A continuación, establezca las dependencias administradas por el usuario en `True`. 

También puede especificar una ruta de acceso a un intérprete de Python específico dentro de la imagen mediante el establecimiento de la variable `Environment.python.interpreter_path`.

```python
dockerfile = """
FROM mcr.microsoft.com/azureml/openmpi3.1.2-ubuntu18.04:20210615.v1
RUN conda install numpy
"""

myenv.docker.base_image = None
myenv.docker.base_dockerfile = dockerfile
myenv.python.user_managed_dependencies=True
myenv.python.interpreter_path = "/opt/miniconda/bin/python"
```

> [!WARNING]
> Si instala algunas dependencias de Python en la imagen de Docker y olvida definir `user_managed_dependencies=True`, dichos paquetes no existirán en el entorno de ejecución, lo que provocará errores en tiempo de ejecución. De forma predeterminada, Azure ML creará un entorno de Conda con las dependencias especificadas y realizará la ejecución en ese entorno en lugar de usar las bibliotecas de Python que haya instalado en la imagen base.

#### <a name="retrieve-image-details"></a>Recuperación de los detalles de la imagen

En el caso de un entorno registrado, se pueden recuperar los detalles de la imagen mediante el código siguiente, donde `details` es una instancia de [DockerImageDetails](/python/api/azureml-core/azureml.core.environment.dockerimagedetails) (SDK de AzureML para Python > = 1.11) y proporciona toda la información sobre la imagen del entorno, como el dockerfile, el registro y el nombre de la imagen.

```python
details = environment.get_image_details(workspace=ws)
```

Para obtener los detalles de la imagen de un entorno autoguardado a partir de una ejecución, use el código siguiente:

```python
details = run.get_environment().get_image_details(workspace=ws)
```

### <a name="use-existing-environments"></a>Uso de entornos existentes

Si tiene un entorno de Conda en el equipo local, puede usar el servicio para crear un objeto de entorno. Mediante esta estrategia, puede volver a usar el entorno interactivo local en ejecuciones remotas.

En el código siguiente se crea un objeto de entorno a partir del entorno Conda existente `mycondaenv`. Utiliza el método [`from_existing_conda_environment()`](/python/api/azureml-core/azureml.core.environment.environment#from-existing-conda-environment-name--conda-environment-name-).

``` python
myenv = Environment.from_existing_conda_environment(name="myenv",
                                                    conda_environment_name="mycondaenv")
```

Una definición de entorno se puede guardar en un directorio en un formato fácilmente editable con el método [`save_to_directory()`](/python/api/azureml-core/azureml.core.environment.environment#save-to-directory-path--overwrite-false-). Una vez modificado, se pueden crear instancias de un nuevo entorno mediante la carga de archivos desde el directorio.

```python
# save the enviroment
myenv.save_to_directory(path="path-to-destination-directory", overwrite=False)
# modify the environment definition
newenv = Environment.load_from_directory(path="path-to-source-directory")
```

### <a name="implicitly-use-the-default-environment"></a>Uso implícito del entorno predeterminado

Si no especifica un entorno en la configuración de ejecución del script antes de enviar la ejecución, se crea un entorno predeterminado de manera automática.

```python
from azureml.core import ScriptRunConfig, Experiment, Environment
# Create experiment 
myexp = Experiment(workspace=ws, name = "environment-example")

# Attach training script and compute target to run config
src = ScriptRunConfig(source_directory=".", script="example.py", compute_target="local")

# Submit the run
run = myexp.submit(config=src)

# Show each step of run 
run.wait_for_completion(show_output=True)
```

## <a name="add-packages-to-an-environment"></a>Adición de paquetes a un entorno

Agregue paquetes a un entorno con los archivos de Conda, pip o Private Wheel. Especifique cada dependencia del paquete mediante la clase [`CondaDependency`](/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies). Agréguelo a `PythonSection` del entorno.

### <a name="conda-and-pip-packages"></a>Paquetes Conda y pip

Si hay un paquete disponible en un repositorio de paquetes Conda, se recomienda usar la instalación de Conda en lugar de la instalación de pip. Los paquetes de Conda suelen incluir archivos binarios pregenerados que hacen que la instalación sea más confiable.

En el ejemplo siguiente se agrega al entorno `myenv`. Agrega la versión 1.17.0 de `numpy`. También se agrega el paquete de `pillow`. En el ejemplo se usan el método [`add_conda_package()`](/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies#add-conda-package-conda-package-) y el método [`add_pip_package()`](/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies#add-pip-package-pip-package-), respectivamente.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs numpy version 1.17.0 conda package
conda_dep.add_conda_package("numpy==1.17.0")

# Installs pillow package
conda_dep.add_pip_package("pillow")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

>[!IMPORTANT]
> Si usa la misma definición de entorno para otra ejecución, Azure Machine Learning Service vuelve a usar la imagen almacenada en caché de su entorno. Si crea un entorno con una dependencia de paquete desanclada (por ejemplo, ```numpy```), ese entorno seguirá usando la versión del paquete instalada _en el momento de la creación del entorno_. Además, cualquier entorno futuro con una definición coincidente seguirá usando la versión anterior. Para más información, consulte [Compilación, almacenamiento en caché y reutilización de entornos](./concept-environments.md#environment-building-caching-and-reuse).

### <a name="private-python-packages"></a>Paquetes privados de Python

Para usar paquetes de Python de forma privada y segura sin exponerlos a la red pública de Internet, consulte el artículo [Cómo usar paquetes privados de Python](how-to-use-private-python-packages.md).

## <a name="manage-environments"></a>Administración de entornos

Administre los entornos para que pueda actualizarlos, realizarles un seguimiento y reutilizarlos en todos los destinos de proceso y con otros usuarios del área de trabajo.

### <a name="register-environments"></a>Registro de entornos

El entorno se registra automáticamente en el área de trabajo cuando se envía una ejecución o se implementa un servicio web. También puede registrar manualmente el entorno mediante el método [`register()`](/python/api/azureml-core/azureml.core.environment%28class%29#register-workspace-). Esta operación convierte el entorno en una entidad con control de versiones y de la que se realiza el seguimiento en la nube. La entidad se puede compartir entre los usuarios del área de trabajo.

En el código siguiente se registra el entorno `myenv` en el área de trabajo `ws`.

```python
myenv.register(workspace=ws)
```

Cuando se usa el entorno por primera vez en el entrenamiento o la implementación, se registra en el área de trabajo. A continuación, se compila e implementa en el destino de proceso. El servicio almacena los entornos en caché. La reutilización de un entorno almacenado en caché tarda mucho menos tiempo que el uso de un nuevo servicio o uno que se haya actualizado.

### <a name="get-existing-environments"></a>Obtención de los entornos existentes

La clase `Environment` ofrece métodos que le permiten recuperar entornos existentes en el área de trabajo. Puede recuperar entornos por nombre, como lista, por ejecución de entrenamiento específica. Esta información es útil para la solución de problemas, la auditoría y la reproducibilidad.

#### <a name="view-a-list-of-environments"></a>Visualización de una lista de entornos

Vea los entornos en el área de trabajo mediante la clase [`Environment.list(workspace="workspace_name")`](/python/api/azureml-core/azureml.core.environment%28class%29#list-workspace-). A continuación, seleccione un entorno para reutilizar.

#### <a name="get-an-environment-by-name"></a>Obtención de un entorno por nombre

También puede obtener un entorno específico por nombre y versión. En el código siguiente se usa el método [`get()`](/python/api/azureml-core/azureml.core.environment%28class%29#get-workspace--name--version-none-) para recuperar la versión `1` del entorno `myenv` del área de trabajo `ws`.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="train-a-run-specific-environment"></a>Entrenamiento de un entorno específico de la ejecución

Para obtener el entorno que se usó para una ejecución específica una vez finalizado el entrenamiento, use el método [`get_environment()`](/python/api/azureml-core/azureml.core.run.run#get-environment--) en la clase `Run`.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>Actualización de un entorno existente

Imagine que cambia un entorno existente, por ejemplo, al agregar un paquete de Python. Tardará un tiempo en compilarse, ya que, después, se crea una nueva versión del entorno cuando envía una ejecución, implementa un modelo o registra manualmente el entorno. El control de versiones le permite ver los cambios en el entorno a lo largo del tiempo. 

Para actualizar la versión de un paquete de Python en un entorno existente, especifique el número de versión de ese paquete. Si no usa el número de versión exacto, Azure Machine Learning volverá a usar el entorno existente con sus versiones de paquete originales.

### <a name="debug-the-image-build"></a>Depuración de la compilación de la imagen

En el ejemplo siguiente se usa el método [`build()`](/python/api/azureml-core/azureml.core.environment%28class%29#build-workspace--image-build-compute-none-) para crear manualmente un entorno como imagen de Docker. Supervisa los registros de salida de la compilación de imágenes mediante [`wait_for_completion()`](/python/api/azureml-core/azureml.core.image%28class%29#wait-for-creation-show-output-false-). La imagen compilada aparece después en la instancia de Azure Container Registry del área de trabajo. Esta información resulta útil para la depuración.

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

Resulta útil compilar imágenes de forma local mediante el método [`build_local()`](/python/api/azureml-core/azureml.core.environment.environment#build-local-workspace--platform-none----kwargs-). Para compilar una imagen de Docker, establezca el parámetro opcional `useDocker=True`. Para insertar la imagen resultante en el registro de contenedor del área de trabajo de AzureML, establezca `pushImageToWorkspaceAcr=True`.

```python
build = env.build_local(workspace=ws, useDocker=True, pushImageToWorkspaceAcr=True)
```

> [!WARNING]
>  Si se cambia el orden de las dependencias o de los canales de un entorno, se generará un nuevo entorno y, por tanto, será necesario volver a compilar la imagen. Además, al llamar al método `build()` de una imagen existente se actualizarán sus dependencias si hay nuevas versiones. 

### <a name="utilize-adminless-azure-container-registry-acr-with-vnet"></a>Uso de Azure Container Registry (ACR) sin administración con una red virtual

Ya no es necesario que los usuarios tengan habilitado el modo de administración en su área de trabajo asociada a ACR en escenarios de red virtual. Asegúrese de que el tiempo de compilación de la imagen derivada en el proceso sea inferior a 1 hora para habilitar la compilación correcta. Una vez que la imagen se inserta en la instancia de ACR del área de trabajo, ahora solo se puede acceder a esta imagen con una identidad de proceso. Para más información sobre la configuración, vea [Uso de identidades administradas con Azure Machine Learning](./how-to-use-managed-identities.md).

## <a name="use-environments-for-training"></a>Uso de entornos para el entrenamiento

Para enviar una ejecución de entrenamiento, debe combinar el entorno, el [destino de proceso](concept-compute-target.md) y el script de Python de entrenamiento en una configuración de ejecución. Esta configuración es un objeto contenedor que se usa para enviar ejecuciones.

Cuando envía una ejecución de entrenamiento, la compilación de un nuevo entorno puede tardar varios minutos. La duración depende del tamaño de las dependencias necesarias. Los entornos se almacenan en caché en el servicio. Por tanto, siempre y cuando la definición del entorno permanezca inalterada, incurrirá en el tiempo de configuración completo una sola vez.

A continuación se proporciona un ejemplo de ejecución de script local en el que se usaría [`ScriptRunConfig`](/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig) como objeto contenedor.

```python
from azureml.core import ScriptRunConfig, Experiment
from azureml.core.environment import Environment

exp = Experiment(name="myexp", workspace = ws)
# Instantiate environment
myenv = Environment(name="myenv")

# Configure the ScriptRunConfig and specify the environment
src = ScriptRunConfig(source_directory=".", script="train.py", compute_target="local", environment=myenv)

# Submit run 
run = exp.submit(src)
```

> [!NOTE]
> Para deshabilitar el historial de ejecución o ejecutar instantáneas, use el valor de `src.run_config.history`.

>[!IMPORTANT]
> Use las SKU de CPU para cualquier compilación de imagen en el proceso. 

Si no especifica el entorno en la configuración de ejecución, el servicio crea un entorno predeterminado cuando envía la ejecución.

## <a name="use-environments-for-web-service-deployment"></a>Uso de entornos para la implementación de servicios web

Puede usar entornos al implementar el modelo como servicio web. Esta funcionalidad permite un flujo de trabajo conectado y reproducible. En este flujo de trabajo, puede entrenar, probar e implementar el modelo con las mismas bibliotecas tanto en el proceso de entrenamiento como en el proceso de inferencia.


Si va a definir su propio entorno para la implementación del servicio web, tendrá que enumerar `azureml-defaults` con la versión >= 1.0.45 como dependencia PIP. Este paquete contiene la funcionalidad necesaria para hospedar el modelo como un servicio web.

Para implementar un servicio web, combine el entorno, el proceso de inferencia, el script de puntuación y el modelo registrado en el objeto de implementación, [`deploy()`](/python/api/azureml-core/azureml.core.model.model#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-). Para obtener más información, consulte [How and where to deploy models](how-to-deploy-and-where.md) (Cómo y dónde implementar modelos).

En este ejemplo, supongamos que ha completado una ejecución de entrenamiento. Ahora desea implementar ese modelo en Azure Container Instances. Al compilar el servicio web, los archivos de modelo y puntuación se montan en la imagen, y la pila de inferencia de Azure Machine Learning se agrega a la imagen.

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import AciWebservice, Webservice

# Register the model to deploy
model = run.register_model(model_name = "mymodel", model_path = "outputs/model.pkl")

# Combine scoring script & environment in Inference configuration
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

# Set deployment configuration
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)

# Define the model, inference, & deployment configuration and web service name and location to deploy
service = Model.deploy(
    workspace = ws,
    name = "my_web_service",
    models = [model],
    inference_config = inference_config,
    deployment_config = deployment_config)
```

## <a name="notebooks"></a>Cuaderno

Los ejemplos de código de este artículo también se incluyen en el [cuaderno sobre el uso de los entornos](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/using-environments/using-environments.ipynb).

Para instalar un entorno de Conda como kernel en un cuaderno, consulte [Adición de un nuevo kernel de Jupyter](./how-to-access-terminal.md#add-new-kernels).

En [Implementación de un modelo mediante una imagen base personalizada de Docker](./how-to-deploy-custom-container.md), se muestra cómo implementar un modelo mediante una imagen base personalizada de Docker.

En este [cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/spark) se muestra cómo implementar un modelo de Spark como servicio web.

## <a name="create-and-manage-environments-with-the-azure-cli"></a>Creación y administración de entornos con la CLI de Azure

[!INCLUDE [cli-version-info](../../includes/machine-learning-cli-version-1-only.md)]

La [CLI de Azure Machine Learning](reference-azure-machine-learning-cli.md) refleja la mayor parte de la funcionalidad del SDK de Python. Puede usarla para crear y administrar entornos. Los comandos que se describen en esta sección muestran la funcionalidad fundamental.

El siguiente comando aplica la técnica scaffolding a los archivos para una definición de entorno predeterminada en el directorio especificado. Estos archivos son archivos JSON. Funcionan como la clase correspondiente en el SDK. Puede usar los archivos para crear nuevos entornos que tengan una configuración personalizada. 

```azurecli-interactive
az ml environment scaffold -n myenv -d myenvdir
```

Ejecute el siguiente comando para registrar un entorno desde un directorio especificado.

```azurecli-interactive
az ml environment register -d myenvdir
```

Ejecute el comando siguiente para enumerar todos los entornos registrados.

```azurecli-interactive
az ml environment list
```

Descargue un entorno registrado con el siguiente comando.

```azurecli-interactive
az ml environment download -n myenv -d downloaddir
```

## <a name="create-and-manage-environments-with-visual-studio-code"></a>Creación y administración de entornos con Visual Studio Code

Con la extensión de Azure Machine Learning, puede crear y administrar entornos en Visual Studio Code. Para más información, consulte [Administración de recursos de Azure Machine Learning con la extensión de VS Code (versión preliminar)](how-to-manage-resources-vscode.md#environments).

## <a name="next-steps"></a>Pasos siguientes

* Para usar un destino de proceso administrado para entrenar un modelo, consulte [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md).
* Después de tener un modelo entrenado, aprenda [cómo y dónde implementar los modelos](how-to-deploy-and-where.md).
* Consulte la [referencia de SDK de la clase `Environment`](/python/api/azureml-core/azureml.core.environment%28class%29).
