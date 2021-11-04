---
title: Uso de ML automatizado en canalizaciones de ML
titleSuffix: Azure Machine Learning
description: AutoMLStep permite usar el aprendizaje automático automatizado en las canalizaciones.
services: machine-learning
ms.service: machine-learning
ms.subservice: automl
ms.author: laobri
author: lobrien
manager: cgronlun
ms.date: 10/21/2021
ms.topic: how-to
ms.custom: devx-track-python, automl
ms.openlocfilehash: a3b1adb3a60b40b027b844230d7c6fca64d8cbe4
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131553311"
---
# <a name="use-automated-ml-in-an-azure-machine-learning-pipeline-in-python"></a>Uso de ML automatizado en una canalización de Azure Machine Learning en Python


La funcionalidad de ML automatizada de Azure Machine Learning le ayuda a detectar modelos de alto rendimiento sin necesidad de volver a implementar todos los enfoques posibles. En combinación con las canalizaciones de Azure Machine Learning, puede crear flujos de trabajo que se pueden implementar y que pueden detectar rápidamente el algoritmo que funcione mejor para sus datos. En este artículo se muestra cómo unir de forma eficaz un paso de preparación de datos a un paso de ML automatizado. ML automatizado puede detectar rápidamente el algoritmo que funciona mejor para sus datos, al tiempo que le conduce hacia MLOps y a la operacionalización del ciclo de vida del modelo con las canalizaciones.

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://azure.microsoft.com/free/).

* Un área de trabajo de Azure Machine Learning. Consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).  

* Conocimientos del SDK y las prestaciones del [aprendizaje automático automatizado](concept-automated-ml.md) y las [canalizaciones de aprendizaje automático](concept-ml-pipelines.md) de Azure.

## <a name="review-automated-mls-central-classes"></a>Revisión de las clases centrales de ML automatizado

El ML automatizado en una canalización se representa mediante un objeto `AutoMLStep`. La clase `AutoMLStep` es una subclase de `PipelineStep`. Un gráfico de objetos `PipelineStep` define un `Pipeline`.

Hay varias subclases de `PipelineStep`. Además del objeto `AutoMLStep`, en este artículo se muestra un objeto `PythonScriptStep` para la preparación de datos y otro para registrar el modelo.

La manera preferida de trasladar inicialmente los datos _a_ una canalización de ML es con objetos `Dataset`. Para trasladar los datos _entre_ pasos y, posiblemente, guardar la salida de datos de las ejecuciones, el método preferido es usar los objetos [`OutputFileDatasetConfig`](/python/api/azureml-core/azureml.data.outputfiledatasetconfig) y [`OutputTabularDatasetConfig`](/python/api/azureml-core/azureml.data.output_dataset_config.outputtabulardatasetconfig). Para que se use con `AutoMLStep`, el objeto `PipelineData` debe transformarse en un objeto `PipelineOutputTabularDataset`. Para más información, consulte [Entrada y salida de datos desde canalizaciones de ML](how-to-move-data-in-out-of-pipelines.md).

`AutoMLStep` se configura a través de un objeto `AutoMLConfig`. `AutoMLConfig` es una clase flexible, como se describe en [Configuración de experimentos de ML automatizado en Python](./how-to-configure-auto-train.md#configure-your-experiment-settings). 

`Pipeline` se ejecuta en `Experiment`. La canalización `Run` tiene, para cada paso, un `StepRun` secundario. Las salidas de `StepRun` del ML automatizado son las métricas de entrenamiento y el modelo de máximo rendimiento.

Para hacer cosas concretas, este artículo crea una canalización simple para una tarea de clasificación. La tarea predice la supervivencia del Titanic, pero no hablaremos de los datos ni de la tarea, solo de pasada.

## <a name="get-started"></a>Introducción

### <a name="retrieve-initial-dataset"></a>Recuperación del conjunto de datos inicial

A menudo, un flujo de trabajo de ML comienza con datos de línea de base ya existentes. Este es un buen escenario para un conjunto de datos registrado. Los conjuntos de datos están visibles en el área de trabajo, admiten el control de versiones y se pueden explorar de forma interactiva. Hay muchas maneras de crear y rellenar un conjunto de datos, tal como se describe en [Creación de conjuntos de datos de Azure Machine Learning](how-to-create-register-datasets.md). Como vamos a usar el SDK de Python para crear nuestra canalización, use el SDK para descargar los datos de línea base y regístrelos con el nombre "titanic_ds".

```python
from azureml.core import Workspace, Dataset

ws = Workspace.from_config()
if not 'titanic_ds' in ws.datasets.keys() :
    # create a TabularDataset from Titanic training data
    web_paths = ['https://dprepdata.blob.core.windows.net/demo/Titanic.csv',
                 'https://dprepdata.blob.core.windows.net/demo/Titanic2.csv']
    titanic_ds = Dataset.Tabular.from_delimited_files(path=web_paths)

    titanic_ds.register(workspace = ws,
                                     name = 'titanic_ds',
                                     description = 'Titanic baseline data',
                                     create_new_version = True)

titanic_ds = Dataset.get_by_name(ws, 'titanic_ds')
```

En primer lugar, el código inicia sesión en el área de trabajo de Azure Machine Learning definida en el archivo **config.json** (para una explicación, vea [Creación de un archivo de configuración de área de trabajo](how-to-configure-environment.md#workspace). Si todavía no hay un conjunto de datos denominado `'titanic_ds'` registrado, se crea uno. El código descarga los datos CSV de la Web, los usa para crear una instancia de `TabularDataset` y luego registra el conjunto de datos con el área de trabajo. Por último, la función `Dataset.get_by_name()` asigna `Dataset` a `titanic_ds`. 

### <a name="configure-your-storage-and-compute-target"></a>Configuración del almacenamiento y el destino de proceso

Los recursos adicionales que necesitará la canalización son el almacenamiento y, por lo general, recursos de proceso de Azure Machine Learning. 

```python
from azureml.core import Datastore
from azureml.core.compute import AmlCompute, ComputeTarget

datastore = ws.get_default_datastore()

compute_name = 'cpu-cluster'
if not compute_name in ws.compute_targets :
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                                min_nodes=0,
                                                                max_nodes=1)
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)

    compute_target.wait_for_completion(
        show_output=True, min_node_count=None, timeout_in_minutes=20)

    # Show the result
    print(compute_target.get_status().serialize())

compute_target = ws.compute_targets[compute_name]
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

Los datos intermedios entre la preparación de datos y el paso de ML automatizado se pueden guardar en el almacén de datos predeterminado del área de trabajo, por lo que lo único que hay que hacer es llamar a `get_default_datastore()` en el objeto `Workspace`. 

Después, el código comprueba si el destino de proceso AML `'cpu-cluster'` ya existe. Si no, especificamos que queremos un pequeño destino de proceso basado en CPU. Si pretende usar las características de aprendizaje profundo de ML automatizado (por ejemplo, caracterización de texto con compatibilidad con DNN), debe elegir un proceso con sólida compatibilidad con GPU, como se describe en [Tamaños de máquinas virtuales optimizadas para GPU](../virtual-machines/sizes-gpu.md). 

El código se bloquea hasta que el destino se aprovisiona y luego imprime algunos detalles del destino de proceso que se acaba de crear. Por último, el destino de proceso con nombre se recupera del área de trabajo y se asigna a `compute_target`. 

### <a name="configure-the-training-run"></a>Configuración de la ejecución de entrenamiento

AutoMLStep configura sus dependencias automáticamente durante el envío del trabajo. El contexto del entorno de ejecución se establece mediante la creación y configuración de un objeto `RunConfiguration`. Aquí se establece el destino de proceso.

```python
from azureml.core.runconfig import RunConfiguration

aml_run_config = RunConfiguration()
# Use just-specified compute target ("cpu-cluster")
aml_run_config.target = compute_target
```

## <a name="prepare-data-for-automated-machine-learning"></a>Preparación de datos para el aprendizaje automático automatizado

### <a name="write-the-data-preparation-code"></a>Escritura del código de preparación de datos

El conjunto de datos de Titanic de línea base consta de datos numéricos y de texto mixtos, donde faltan algunos valores. Para prepararlo para el aprendizaje automático automatizado, el paso de la canalización de preparación de datos:

- rellenará los datos que faltan con datos aleatorios o con una categoría correspondiente a "Unknown",
- transformará datos de categorías en enteros,
- quitará columnas que no pretendemos usar,
- dividirá los datos en conjuntos de entrenamiento y de prueba y
- escribirá los datos transformados en las rutas de acceso de salida de `OutputFileDatasetConfig`.

```python
%%writefile dataprep.py
from azureml.core import Run

import pandas as pd 
import numpy as np 
import argparse

RANDOM_SEED=42

def prepare_age(df):
    # Fill in missing Age values from distribution of present Age values 
    mean = df["Age"].mean()
    std = df["Age"].std()
    is_null = df["Age"].isnull().sum()
    # compute enough (== is_null().sum()) random numbers between the mean, std
    rand_age = np.random.randint(mean - std, mean + std, size = is_null)
    # fill NaN values in Age column with random values generated
    age_slice = df["Age"].copy()
    age_slice[np.isnan(age_slice)] = rand_age
    df["Age"] = age_slice
    df["Age"] = df["Age"].astype(int)
    
    # Quantize age into 5 classes
    df['Age_Group'] = pd.qcut(df['Age'],5, labels=False)
    df.drop(['Age'], axis=1, inplace=True)
    return df

def prepare_fare(df):
    df['Fare'].fillna(0, inplace=True)
    df['Fare_Group'] = pd.qcut(df['Fare'],5,labels=False)
    df.drop(['Fare'], axis=1, inplace=True)
    return df 

def prepare_genders(df):
    genders = {"male": 0, "female": 1, "unknown": 2}
    df['Sex'] = df['Sex'].map(genders)
    df['Sex'].fillna(2, inplace=True)
    df['Sex'] = df['Sex'].astype(int)
    return df

def prepare_embarked(df):
    df['Embarked'].replace('', 'U', inplace=True)
    df['Embarked'].fillna('U', inplace=True)
    ports = {"S": 0, "C": 1, "Q": 2, "U": 3}
    df['Embarked'] = df['Embarked'].map(ports)
    return df
    
parser = argparse.ArgumentParser()
parser.add_argument('--output_path', dest='output_path', required=True)
args = parser.parse_args()
    
titanic_ds = Run.get_context().input_datasets['titanic_ds']
df = titanic_ds.to_pandas_dataframe().drop(['PassengerId', 'Name', 'Ticket', 'Cabin'], axis=1)
df = prepare_embarked(prepare_genders(prepare_fare(prepare_age(df))))

df.to_csv(os.path.join(args.output_path,"prepped_data.csv"))

print(f"Wrote prepped data to {args.output_path}/prepped_data.csv")
```

El fragmento de código anterior es un ejemplo completo, pero mínimo, de la preparación de datos para los datos de Titanic. El fragmento de código comienza con un "comando mágico" de Jupyter para enviar el código a un archivo. Si no está utilizando un cuaderno de Jupyter, elimine esa línea y cree el archivo manualmente.

Las distintas funciones `prepare_` del fragmento de código anterior modifican la columna pertinente en el conjunto de datos de entrada. Estas funciones hacen su trabajo en los datos una vez que estos se han cambiado a un objeto `DataFrame` Pandas. En cada caso, los datos que faltan se rellenan con datos aleatorios representativos o datos de categorías que indican "Unknown". Los datos de categorías basados en texto se asignan a enteros. Las columnas que ya no son necesarias se sobrescriben o se quitan. 

Una vez que el código define las funciones de preparación de datos, dicho código analiza el argumento de entrada, que es la ruta de acceso en la que queremos escribir nuestros datos.  (Estos valores se determinarán mediante objetos `OutputFileDatasetConfig` que se tratarán en el paso siguiente). El código recupera el objeto `Dataset` `'titanic_cs'` registrado, lo convierte en un objeto `DataFrame` de Pandas y llama a las distintas funciones de preparación de datos. 

Dado que `output_path` es un directorio, la llamada a `to_csv()` especifica el nombre de archivo `prepped_data.csv`.

### <a name="write-the-data-preparation-pipeline-step-pythonscriptstep"></a>Escritura del paso de canalización de preparación de datos (`PythonScriptStep`)

El código de preparación de datos descrito anteriormente debe estar asociado a un objeto `PythonScripStep` que se va a usar con una canalización. La ruta de acceso en la que se escribe la salida del CSV se genera mediante un objeto `OutputFileDatasetConfig`. Los recursos preparados anteriormente, como `ComputeTarget`, `RunConfig` y `'titanic_ds' Dataset`, se usan para completar la especificación.

```python
from azureml.data import OutputFileDatasetConfig
from azureml.pipeline.steps import PythonScriptStep

prepped_data_path = OutputFileDatasetConfig(name="output_path")

dataprep_step = PythonScriptStep(
    name="dataprep", 
    script_name="dataprep.py", 
    compute_target=compute_target, 
    runconfig=aml_run_config,
    arguments=["--output_path", prepped_data_path],
    inputs=[titanic_ds.as_named_input('titanic_ds')],
    allow_reuse=True
)
```

El objeto `prepped_data_path` es de tipo `OutputFileDatasetConfig`, que señala a un directorio.  Tenga en cuenta que se especifica en el parámetro `arguments`. Si revisa el paso anterior, verá que, en el código de preparación de datos, el valor del argumento `'--output_path'` es la ruta de acceso al directorio en el que se ha escrito el archivo CSV. 

## <a name="train-with-automlstep"></a>Entrenamiento con AutoMLStep

La configuración de un paso de canalización de ML automatizado se realiza con la clase `AutoMLConfig`. Esta clase flexible se describe en [Configuración de experimentos de ML automatizado en Python](./how-to-configure-auto-train.md). La entrada y la salida de datos son los únicos aspectos de la configuración que requieren atención especial en una canalización de ML. La entrada y la salida de `AutoMLConfig` en canalizaciones se describen detalladamente a continuación. Además de los datos, una ventaja de las canalizaciones de ML es la capacidad de usar distintos destinos de proceso para diferentes pasos. Puede optar por usar un recurso `ComputeTarget` más eficaz solo para el proceso de ML automatizado. Hacer esto es tan sencillo como asignar un objeto `RunConfiguration` más eficaz al parámetro `run_configuration` del objeto `AutoMLConfig`.

### <a name="send-data-to-automlstep"></a>Envío de datos a `AutoMLStep`

En una canalización de ML, los datos de entrada deben ser un objeto `Dataset`. El máximo rendimiento se consigue proporcionando los datos de entrada en forma de objetos `OutputTabularDatasetConfig`. Puede crear un objeto de ese tipo con `read_delimited_files()` en un elemento `OutputFileDatasetConfig`, como el objeto `prepped_data_path` y el objeto `prepped_data_path`.

```python
# type(prepped_data) == OutputTabularDatasetConfig
prepped_data = prepped_data_path.read_delimited_files()
```

Otra opción es usar objetos `Dataset` registrados en el área de trabajo:

```python
prepped_data = Dataset.get_by_name(ws, 'Data_prepared')
```

Comparación de las dos técnicas:

| Técnica | Ventajas y desventajas | 
|-|-|
|`OutputTabularDatasetConfig`| Rendimiento más alto | 
|| Ruta natural desde `OutputFileDatasetConfig` | 
|| Los datos no se conservan después de la ejecución de la canalización |
||  |
| `Dataset` registrado | Menor rendimiento |
| | Se puede generar de muchas maneras | 
| | Los datos se conservan y son visibles en toda el área de trabajo |
| | [Cuaderno que muestra la técnica de `Dataset` registrado](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/continuous-retraining/auto-ml-continuous-retraining.ipynb)


### <a name="specify-automated-ml-outputs"></a>Especificación de salidas de ML automatizado

Las salidas del objeto `AutoMLStep` son las puntuaciones de métricas finales del modelo de mayor rendimiento y el propio modelo. Para usar estas salidas en pasos posteriores de la canalización, prepare objetos `OutputFileDatasetConfig` para recibirlas.

```python
from azureml.pipeline.core import TrainingOutput, PipelineData

metrics_data = PipelineData(name='metrics_data',
                            datastore=datastore,
                            pipeline_output_name='metrics_output',
                            training_output=TrainingOutput(type='Metrics'))

model_data = PipelineData(name='best_model_data',
                          datastore=datastore,
                          pipeline_output_name='model_output',
                          training_output=TrainingOutput(type='Model'))
```

El fragmento de código anterior crea los dos objetos `PipelineData` para las métricas y la salida del modelo. Se les asigna un nombre, se asignan al almacén de datos predeterminado que se recuperó anteriormente y se asocian al `type` concreto de `TrainingOutput` del objeto `AutoMLStep`. Dado que asignamos `pipeline_output_name` en estos objetos `PipelineData`, sus valores estarán disponibles no solo desde el paso de canalización individual, sino también desde la canalización en conjunto, como se explica a continuación en la sección "Examen de los resultados de la canalización". 

### <a name="configure-and-create-the-automated-ml-pipeline-step"></a>Configuración y creación del paso de canalización de ML automatizado

Una vez definidas las entradas y salidas, es el momento de crear los objetos `AutoMLConfig` y `AutoMLStep`. Los detalles de la configuración dependerán de la tarea, tal como se describe en [Configuración de experimentos de ML automatizado en Python](./how-to-configure-auto-train.md). En la tarea de clasificación de supervivencia del Titanic, el fragmento de código siguiente muestra una configuración simple.

```python
from azureml.train.automl import AutoMLConfig
from azureml.pipeline.steps import AutoMLStep

# Change iterations to a reasonable number (50) to get better accuracy
automl_settings = {
    "iteration_timeout_minutes" : 10,
    "iterations" : 2,
    "experiment_timeout_hours" : 0.25,
    "primary_metric" : 'AUC_weighted'
}

automl_config = AutoMLConfig(task = 'classification',
                             path = '.',
                             debug_log = 'automated_ml_errors.log',
                             compute_target = compute_target,
                             run_configuration = aml_run_config,
                             featurization = 'auto',
                             training_data = prepped_data,
                             label_column_name = 'Survived',
                             **automl_settings)

train_step = AutoMLStep(name='AutoML_Classification',
    automl_config=automl_config,
    passthru_automl_config=False,
    outputs=[metrics_data,model_data],
    enable_default_model_output=False,
    enable_default_metrics_output=False,
    allow_reuse=True)
```
El fragmento de código muestra una expresión que se usa habitualmente con `AutoMLConfig`. Los argumentos que son más fluidos (hiperparámetro-ish) se especifican en un diccionario independiente, mientras que los valores con menos probabilidades de cambio se especifican directamente en el constructor `AutoMLConfig`. En este caso, `automl_settings` especifica una breve ejecución: la ejecución se detendrá después de solo dos iteraciones o quince minutos, lo que suceda primero.

El diccionario de `automl_settings` se pasa al constructor de `AutoMLConfig` como kwargs. Los demás parámetros no son complejos:

- `task` se establece en `classification` para este ejemplo. Otros valores válidos son `regression` y `forecasting`.
- `path` y `debug_log` describen la ruta de acceso al proyecto y un archivo local donde se escribirá la información de depuración. 
- `compute_target` es el `compute_target` que se definió previamente que, en este ejemplo, es un equipo basado en CPU económico. Si usa las prestaciones de aprendizaje profundo de AutoML, querrá cambiar el destino de proceso para que esté basado en GPU.
- `featurization` se establece en `auto`. Puede encontrar más detalles en la sección [Caracterización de datos](./how-to-configure-auto-train.md#data-featurization) del documento de configuración de ML automatizado. 
- `label_column_name` indica qué columna nos interesa predecir. 
- `training_data` se establece en los objetos `OutputTabularDatasetConfig` realizados a partir de los resultados del paso de preparación de datos. 

El propio `AutoMLStep` toma `AutoMLConfig` y tiene, como salidas, los objetos `PipelineData` creados para contener las métricas y los datos del modelo. 

>[!Important]
> Debe establecer `enable_default_model_output` y `enable_default_metrics_output` en `True` solo si va a usar `AutoMLStepRun`.

En este ejemplo, el proceso de ML automatizado realizará validaciones cruzadas en `training_data`. Puede controlar el número de validaciones cruzadas con el argumento `n_cross_validations`. Si ya ha dividido los datos de entrenamiento como parte de los pasos de preparación de datos, puede establecer `validation_data` en su propio `Dataset`.

Es posible que, ocasionalmente, vea el uso de `X` para las características de datos y `y` para las etiquetas de datos. Esta técnica está en desuso y debe utilizar `training_data` para la entrada. 

## <a name="register-the-model-generated-by-automated-ml"></a>Registro del modelo generado por ML automatizado 

El último paso en una canalización de ML simple es registrar el modelo creado. Al agregar el modelo al registro de modelo del área de trabajo, estará disponible en el portal y podrá tener versiones. Para registrar el modelo, escriba otro `PythonScriptStep` que tome la salida `model_data` del objeto `AutoMLStep`.

### <a name="write-the-code-to-register-the-model"></a>Escritura del código para registrar el modelo

Un modelo se registra en un `Workspace`. Probablemente esté familiarizado con el uso de `Workspace.from_config()` para iniciar sesión en el área de trabajo en el equipo local, pero hay otra manera de obtener el área de trabajo desde una canalización de ML en ejecución. `Run.get_context()` recupera el objeto `Run` activo. Este objeto `run` proporciona acceso a muchos objetos importantes, incluidos el objeto `Workspace` usado aquí.

```python
%%writefile register_model.py
from azureml.core.model import Model, Dataset
from azureml.core.run import Run, _OfflineRun
from azureml.core import Workspace
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("--model_name", required=True)
parser.add_argument("--model_path", required=True)
args = parser.parse_args()

print(f"model_name : {args.model_name}")
print(f"model_path: {args.model_path}")

run = Run.get_context()
ws = Workspace.from_config() if type(run) == _OfflineRun else run.experiment.workspace

model = Model.register(workspace=ws,
                       model_path=args.model_path,
                       model_name=args.model_name)

print("Registered version {0} of model {1}".format(model.version, model.name))
```

### <a name="write-the-pythonscriptstep-code"></a>Escritura del código de PythonScriptStep

`PythonScriptStep` de registro del modelo utiliza `PipelineParameter` para uno de sus argumentos. Los parámetros de canalización son argumentos para las canalizaciones que se pueden establecer fácilmente en el momento de la entrega de la ejecución. Una vez declarados, se pasan como argumentos normales. 

```python

from azureml.pipeline.core.graph import PipelineParameter

# The model name with which to register the trained model in the workspace.
model_name = PipelineParameter("model_name", default_value="TitanicSurvivalInitial")

register_step = PythonScriptStep(script_name="register_model.py",
                                       name="register_model",
                                       allow_reuse=False,
                                       arguments=["--model_name", model_name, "--model_path", model_data],
                                       inputs=[model_data],
                                       compute_target=compute_target,
                                       runconfig=aml_run_config)
```

## <a name="create-and-run-your-automated-ml-pipeline"></a>Creación y ejecución de la canalización de ML automatizado

La creación y ejecución de una canalización que contiene un objeto `AutoMLStep` no es diferente de una canalización normal. 

```python
from azureml.pipeline.core import Pipeline
from azureml.core import Experiment

pipeline = Pipeline(ws, [dataprep_step, train_step, register_step])

experiment = Experiment(workspace=ws, name='titanic_automl')

run = experiment.submit(pipeline, show_output=True)
run.wait_for_completion()
```

El código anterior combina los pasos de preparación de datos, ML automatizado y registro del modelo en un objeto `Pipeline`. Después, crea un objeto `Experiment`. El constructor `Experiment` recuperará el experimento con nombre si existe o lo creará si es necesario. Envía el objeto `Pipeline` a `Experiment` y crea un objeto `Run` que ejecutará la canalización de forma asincrónica. La función `wait_for_completion()` se bloquea hasta que se completa la ejecución.

### <a name="examine-pipeline-results"></a>Examen de los resultados de la canalización 

Cuando `run` finaliza, puede recuperar objetos `PipelineData` a los que se ha asignado `pipeline_output_name`. Puede descargar los resultados y cargarlos para su posterior procesamiento.  

```python
metrics_output_port = run.get_pipeline_output('metrics_output')
model_output_port = run.get_pipeline_output('model_output')

metrics_output_port.download('.', show_progress=True)
model_output_port.download('.', show_progress=True)
```

Los archivos descargados se escriben en el subdirectorio `azureml/{run.id}/`. El archivo de métricas tiene formato JSON y se puede convertir en un dataframe Pandas para su examen.

Para el procesamiento local, puede que necesite instalar paquetes pertinentes, como Pandas, Pickle, AzureML SDK, etc. En este ejemplo, es probable que el mejor modelo encontrado por ML automatizado dependa de XGBoost.

```bash
!pip install xgboost==0.90
```

```python
import pandas as pd
import json

metrics_filename = metrics_output._path_on_datastore
# metrics_filename = path to downloaded file
with open(metrics_filename) as f:
   metrics_output_result = f.read()
   
deserialized_metrics_output = json.loads(metrics_output_result)
df = pd.DataFrame(deserialized_metrics_output)
df
```

En el fragmento de código anterior se muestra el archivo de métricas que se va a cargar desde su ubicación en el almacén de archivos de Azure. También puede cargarlo desde el archivo descargado, tal y como se muestra en el comentario. Cuando se haya deserializado y convertido en un DataFrame Pandas, puede ver las métricas detalladas de cada una de las iteraciones del paso de ML automatizado.

El archivo de modelo se puede deserializar en un objeto `Model` que se puede usar para la inferencia, análisis de métricas adicionales, etc. 

```python
import pickle

model_filename = model_output._path_on_datastore
# model_filename = path to downloaded file

with open(model_filename, "rb" ) as f:
    best_model = pickle.load(f)

# ... inferencing code not shown ...
```

Para más información sobre cómo cargar y trabajar con modelos existentes, vea [Usar un modelo existente con Azure Machine Learning](how-to-deploy-and-where.md).

### <a name="download-the-results-of-an-automated-ml-run"></a>Descarga de los resultados de una ejecución de ML automatizado

Si ha seguido el artículo, tendrá una instancia del objeto `run`. Pero también puede recuperar los objetos `Run` completados desde `Workspace` por medio de un objeto `Experiment`.

El área de trabajo contiene un registro completo de todos los experimentos y ejecuciones. Puede usar el portal para buscar y descargar los resultados de los experimentos o usar código. Para acceder a los registros desde una ejecución histórica, utilice Azure Machine Learning para buscar el identificador de la ejecución en la que está interesado. Con ese identificador, puede elegir el objeto `run` específico por medio de `Workspace` y `Experiment`.

```python
# Retrieved from Azure Machine Learning web UI
run_id = 'aaaaaaaa-bbbb-cccc-dddd-0123456789AB'
experiment = ws.experiments['titanic_automl']
run = next(run for run in ex.get_runs() if run.id == run_id)
```

Tendría que cambiar las cadenas del código anterior a las características específicas de la ejecución histórica. En el fragmento de código anterior se supone que ha asignado `ws` al `Workspace` pertinente con el `from_config()` normal. El experimento de interés se recupera directamente y, después, el código busca el `Run` de interés mediante la coincidencia del valor de `run.id`.

Una vez que tenga un objeto `Run`, puede descargar las métricas y el modelo. 

```python
automl_run = next(r for r in run.get_children() if r.name == 'AutoML_Classification')
outputs = automl_run.get_outputs()
metrics = outputs['default_metrics_AutoML_Classification']
model = outputs['default_model_AutoML_Classification']

metrics.get_port_data_reference().download('.')
model.get_port_data_reference().download('.')
```

Cada objeto `Run` contiene objetos `StepRun` que contienen información sobre la ejecución del paso de canalización individual. `run` se busca para el objeto `StepRun` para `AutoMLStep`. Las métricas y el modelo se recuperan con sus nombres predeterminados, que están disponibles incluso si no pasa objetos `PipelineData` al parámetro `outputs` de `AutoMLStep`. 

Por último, el modelo y las métricas reales se descargan en el equipo local, como se explicó en la sección "Examen de los resultados de la canalización" anterior.

## <a name="next-steps"></a>Pasos siguientes

- Ejecutar este cuaderno de Jupyter en el que se muestra un [ejemplo completo de ML automatizado en una canalización](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/nyc-taxi-data-regression-model-building/nyc-taxi-data-regression-model-building.ipynb) que usa la regresión para predecir tarifas de taxis
- [Crear experimentos de ML automatizado sin escribir código](how-to-use-automated-ml-for-ml-models.md)
- Explorar diversos [cuadernos de Jupyter que muestran ML automatizado](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)
- Leer información sobre la integración de la canalización en [MLOps integral](./concept-model-management-and-deployment.md#automate-the-ml-lifecycle) o investigar el [repositorio de GitHub de MLOps](https://github.com/Microsoft/MLOpspython)
