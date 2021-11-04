---
title: 'Tutorial: Modelo de regresión de entrenamiento mediante ML automatizado'
titleSuffix: Azure Machine Learning
description: Entrene un modelo de regresión para predecir tarifas de taxi de Nueva York con el SDK de Python para Azure Machine Learning mediante el modelo de ML automatizado de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: automl
ms.topic: tutorial
author: cartacioS
ms.author: sacartac
ms.reviewer: nibaccam
ms.date: 10/21/2021
ms.custom: devx-track-python, automl, FY21Q4-aml-seo-hack, contperf-fy21q4
ms.openlocfilehash: caa8f387debbc5afa9f53f76c100268312d7b9b3
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131555825"
---
# <a name="tutorial-train-a-regression-model-with-automl-and-python"></a>Tutorial: Entrenamiento de un modelo de regresión con AutoML y Python

En este tutorial, aprenderá a entrenar un modelo de regresión con el SDK de Python para Azure Machine Learning mediante el modelo de ML automatizado de Azure Machine Learning. Este modelo de regresión predice las tarifas de taxi de Nueva York. 

Este proceso acepta valores de configuración y datos de entrenamiento y, después, itera automáticamente las combinaciones de diferentes métodos de estandarización o normalización de características, modelos y configuraciones de hiperparámetros para llegar al mejor modelo. 

![Diagrama de flujo](./media/tutorial-auto-train-models/flow2.png)

En este tutorial escribirá código con el SDK de Python.  Aprenderá las siguientes tareas:

> [!div class="checklist"]
> * Descargar, transformar y limpiar datos mediante Azure Open Datasets
> * Entrenar un modelo de regresión con aprendizaje automático automatizado
> * Calcular la precisión del modelo

Para AutoML sin código, pruebe los siguientes tutoriales: 

* [Tutorial: Entrenamiento de modelos de clasificación sin código](tutorial-first-experiment-automated-ml.md)

* [Tutorial: Previsión de la demanda con aprendizaje automático automatizado](tutorial-automated-ml-forecast.md)

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago](https://azure.microsoft.com/free/) de Azure Machine Learning.

* Complete los pasos que se describen en [Inicio rápido: Introducción a Azure Machine Learning](quickstart-create-resources.md) si aún no tiene un área de trabajo o una instancia de proceso de Azure Machine Learning.
* Después de completar la guía de inicio rápido:
    1. Seleccione **Cuadernos** en Studio.
    1. Seleccione la pestaña **Muestras**.
    1. Abra el cuaderno *tutorials/regression-automl-nyc-taxi-data/regression-automated-ml.ipynb*

Este tutorial también está disponible en [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials) si quiere ejecutarlo en su propio [entorno local](how-to-configure-environment.md#local). Para obtener los paquetes obligatorios, 
* [instale el cliente `automl` completo](https://github.com/Azure/azureml-examples/blob/main/python-sdk/tutorials/automl-with-azureml/README.md#setup-using-a-local-conda-environment).
* Ejecute `pip install azureml-opendatasets azureml-widgets` para obtener los paquetes requeridos.

## <a name="download-and-prepare-data"></a>Descargar y preparar los datos

Importe los paquetes necesarios. El paquete de Open Datasets contiene una clase que representa cada origen de datos (por ejemplo, `NycTlcGreen`) para filtrar fácilmente los parámetros de fecha antes de la descarga.

```python
from azureml.opendatasets import NycTlcGreen
import pandas as pd
from datetime import datetime
from dateutil.relativedelta import relativedelta
```

Empiece por crear una trama de datos para almacenar los datos de taxis. Cuando se trabaja en un entorno que no es de Spark, Open Datasets solo permite descargar un mes de datos a la vez con determinadas clases, para así evitar que se presenten errores `MemoryError` con grandes conjuntos de datos.

Para descargar los datos de los taxis, recupere de forma iterativa los datos de mes en mes y, antes de agregarlos a `green_taxi_df`, realice un muestreo aleatorio de 2000 registros de cada mes para evitar el sobredimensionamiento de la trama de datos. Después, obtenga una vista previa de los datos.


```python
green_taxi_df = pd.DataFrame([])
start = datetime.strptime("1/1/2015","%m/%d/%Y")
end = datetime.strptime("1/31/2015","%m/%d/%Y")

for sample_month in range(12):
    temp_df_green = NycTlcGreen(start + relativedelta(months=sample_month), end + relativedelta(months=sample_month)) \
        .to_pandas_dataframe()
    green_taxi_df = green_taxi_df.append(temp_df_green.sample(2000))

green_taxi_df.head(10)
```

|vendorID| lpepPickupDatetime|  lpepDropoffDatetime|    passengerCount| tripDistance|   puLocationId|   doLocationId|   pickupLongitude|    pickupLatitude| dropoffLongitude    |...|   paymentType |fareAmount |extra| mtaTax| improvementSurcharge|   tipAmount|  tollsAmount|    ehailFee|   totalAmount|    tripType|
|----|----|----|----|----|----|---|--|---|---|---|----|----|----|--|---|----|-----|----|----|----|
|131969|2|2015-01-11 05:34:44|2015-01-11 05:45:03|3|4.84|None|None|-73.88|40.84|-73.94|...|2|15.00|0.50|0.50|0,3|0.00|0.00|nan|16.30|
|1129817|2|2015-01-20 16:26:29|2015-01-20 16:30:26|1|0.69|None|None|-73.96|40.81|-73.96|...|2|4.50|1.00|0.50|0,3|0.00|0.00|nan|6.30|
|1278620|2|2015-01-01 05:58:10|2015-01-01 06:00:55|1|0.45|None|None|-73.92|40.76|-73.91|...|2|4.00|0.00|0.50|0,3|0.00|0.00|nan|4.80|
|348430|2|2015-01-17 02:20:50|2015-01-17 02:41:38|1|0.00|None|None|-73.81|40.70|-73.82|...|2|12.50|0.50|0.50|0,3|0.00|0.00|nan|13.80|
1269627|1|2015-01-01 05:04:10|2015-01-01 05:06:23|1|0.50|None|None|-73.92|40.76|-73.92|...|2|4.00|0.50|0.50|0|0.00|0.00|nan|5.00|
|811755|1|2015-01-04 19:57:51|2015-01-04 20:05:45|2|1.10|None|None|-73.96|40.72|-73.95|...|2|6.50|0.50|0.50|0,3|0.00|0.00|nan|7.80|
|737281|1|2015-01-03 12:27:31|2015-01-03 12:33:52|1|0.90|None|None|-73.88|40.76|-73.87|...|2|6.00|0.00|0.50|0,3|0.00|0.00|nan|6.80|
|113951|1|2015-01-09 23:25:51|2015-01-09 23:39:52|1|3.30|None|None|-73.96|40.72|-73.91|...|2|12.50|0.50|0.50|0,3|0.00|0.00|nan|13.80|
|150436|2|2015-01-11 17:15:14|2015-01-11 17:22:57|1|1.19|None|None|-73.94|40.71|-73.95|...|1|7.00|0.00|0.50|0,3|1,75|0.00|nan|9,55|
|432136|2|2015-01-22 23:16:33   2015-01-22 23:20:13 1   0.65|None|None|-73.94|40.71|-73.94|...|2|5.00|0.50|0.50|0,3|0.00|0.00|nan|6.30|

Quite algunas de las columnas que no necesite para realizar el entrenamiento o crear características adicionales.  La automatización del aprendizaje automático controlará automáticamente las características basadas en el tiempo, como **lpepPickupDatetime**.

```python
columns_to_remove = ["lpepDropoffDatetime", "puLocationId", "doLocationId", "extra", "mtaTax",
                     "improvementSurcharge", "tollsAmount", "ehailFee", "tripType", "rateCodeID",
                     "storeAndFwdFlag", "paymentType", "fareAmount", "tipAmount"
                    ]
for col in columns_to_remove:
    green_taxi_df.pop(col)

green_taxi_df.head(5)
```

### <a name="cleanse-data"></a>Limpieza de datos

Ejecute la función `describe()` en la nueva trama de datos para ver las estadísticas de resumen de cada campo.

```python
green_taxi_df.describe()
```

|vendorID|passengerCount|tripDistance|pickupLongitude|pickupLatitude|dropoffLongitude|dropoffLatitude|  totalAmount|month_num   day_of_month|day_of_week|hour_of_day
|----|----|---|---|----|---|---|---|---|---|---|
|count|48000.00|48000.00|48000.00|48000.00|48000.00|48000.00|48000.00|48000.00|48000.00|48000.00|
|mean|1,78|1.37|2.87|-73.83|40.69|-73.84|40.70|14.75|6.50|15.13|
|std|0,41|1.04|2,93|2.76|1.52|2.61|1.44|12.08|3.45|8.45|
|Min|1.00|0.00|0.00|-74.66|0.00|-74.66|0.00|-300.00|1.00|1.00|
|25 %|2.00|1.00|1.06|-73.96|40.70|-73.97|40.70|7.80|3,75|8.00|
|50 %|2.00|1.00|1.90|-73.94|40.75|-73.94|40.75|11.30|6.50|15.00|
|75 %|2.00|1.00|3.60|-73.92|40.80|-73.91|40.79|17.80|9.25|22.00|
|max|2.00|9.00|97.57|0.00|41.93|0.00|41.94|450.00|12.00|30.00|


Desde las estadísticas de resumen, verá que hay varios campos que tienen valores atípicos o valores que reducirán la precisión del modelo. En primer lugar, filtre los campos lat/long para que estén dentro de los límites del área de Manhattan. De este modo, filtrará los trayectos o los trayectos en taxi más largos que sean valores atípicos en lo que respecta a su relación con otras características.

Además, filtre el campo `tripDistance` para que sea mayor que cero, pero menor que 31 millas (la distancia del semiverseno entre los dos pares lat/long). Así elimina los recorridos atípicos largos que tengan un costo de trayecto incoherente.

Por último, el campo `totalAmount` tiene valores negativos para las tarifas de taxi, lo que no tiene sentido en el contexto de nuestro modelo, y el campo `passengerCount` tiene datos incorrectos y su valor mínimo es cero.

Filtre estas anomalías mediante funciones de consulta y, a continuación, quite las últimas columnas innecesarias para el entrenamiento.


```python
final_df = green_taxi_df.query("pickupLatitude>=40.53 and pickupLatitude<=40.88")
final_df = final_df.query("pickupLongitude>=-74.09 and pickupLongitude<=-73.72")
final_df = final_df.query("tripDistance>=0.25 and tripDistance<31")
final_df = final_df.query("passengerCount>0 and totalAmount>0")

columns_to_remove_for_training = ["pickupLongitude", "pickupLatitude", "dropoffLongitude", "dropoffLatitude"]
for col in columns_to_remove_for_training:
    final_df.pop(col)
```

Vuelva a llamar a la función `describe()` desde los datos para garantizar que la limpieza ha funcionado como se esperaba. Ahora tiene un conjunto preparado y limpio de datos meteorológicos, de taxis y de días festivos que se utilizará para entrenar el modelo de Machine Learning.

```python
final_df.describe()
```

## <a name="configure-workspace"></a>Configuración del área de trabajo

Cree un objeto de área de trabajo desde el área de trabajo existente. Un [área de trabajo](/python/api/azureml-core/azureml.core.workspace.workspace) es una clase que acepta la información de recursos y suscripciones de Azure. También crea un recurso en la nube para supervisar y realizar un seguimiento de las ejecuciones del modelo. `Workspace.from_config()` lee el archivo **config.json** y carga los detalles de autenticación en un objeto denominado `ws`. En el resto del código de este tutorial se usa `ws`.

```python
from azureml.core.workspace import Workspace
ws = Workspace.from_config()
```

## <a name="split-the-data-into-train-and-test-sets"></a>Dividir los datos en conjuntos de entrenamiento y prueba

Divida los datos en conjuntos de entrenamiento y prueba mediante la función `train_test_split` de la biblioteca `scikit-learn`. Esta función segrega los datos entre el conjunto de datos "x" (**características**) para el entrenamiento del modelo y el conjunto de datos "y" (**valores a predecir**) para la realización de pruebas.

El parámetro `test_size` determina el porcentaje de datos que se va a asignar a las pruebas. El parámetro `random_state` establece un valor de inicialización para el generador de números aleatorios, de forma que las divisiones de entrenamiento o prueba son deterministas.

```python
from sklearn.model_selection import train_test_split

x_train, x_test = train_test_split(final_df, test_size=0.2, random_state=223)
```

El propósito de este paso es tener puntos de datos para probar el modelo terminado que no se hayan usado para entrenar el modelo y así poder medir la precisión real.

En otras palabras, un modelo bien entrenado debe ser capaz predecir de manera precisa a partir de datos que aún no ha visto. Ahora tiene los datos preparados para entrenar automáticamente un modelo de aprendizaje automático.

## <a name="automatically-train-a-model"></a>Entrenamiento automático de un modelo

Para entrenar automáticamente un modelo, realice los pasos siguientes:
1. Defina la configuración de la ejecución del experimento. Asocie los datos de entrenamiento a la configuración y modifíquela para controlar el entrenamiento.
1. Envíe el experimento para la optimización del modelo. Después de enviar el experimento, el proceso se itera mediante distintos algoritmos de aprendizaje automático y configuraciones de hiperparámetros, de conformidad con las restricciones definidas. Elige el modelo de ajuste perfecto mediante la optimización de una métrica de precisión.

### <a name="define-training-settings"></a>Definición de la configuración del entrenamiento

Defina los parámetros del experimento y la configuración de los modelos para el entrenamiento. Vea la lista completa de [valores](how-to-configure-auto-train.md). El envío del experimento con esta configuración predeterminada tardará de 5 a 20 min, pero, si desea que la ejecución sea más breve, puede reducir el parámetro `experiment_timeout_hours`.

|Propiedad| Valor en este tutorial |Descripción|
|----|----|---|
|**iteration_timeout_minutes**|10|Límite de tiempo en minutos para cada iteración. Aumente este valor para conjuntos de datos mayores que necesiten más tiempo para cada iteración.|
|**experiment_timeout_hours**|0,3|Cantidad máxima de tiempo en horas que pueden tardar todas las iteraciones combinadas antes de que finalice el experimento.|
|**enable_early_stopping**|True|Marca para permitir la finalización prematura si la puntuación no mejora a corto plazo.|
|**primary_metric**| spearman_correlation | Métrica que desea optimizar. El modelo de ajuste perfecto se elegirá según esta métrica.|
|**featurization**| auto | Mediante el uso de **auto**, el experimento puede preprocesar los datos de entrada (administrar los datos que faltan, convertir texto a numérico, etc.).|
|**verbosity**| logging.INFO | Controla el nivel de registro.|
|**n_cross_validations**|5|Número de divisiones de la validación cruzada que se realizarán cuando no se especifiquen datos de validación.|

```python
import logging

automl_settings = {
    "iteration_timeout_minutes": 10,
    "experiment_timeout_hours": 0.3,
    "enable_early_stopping": True,
    "primary_metric": 'spearman_correlation',
    "featurization": 'auto',
    "verbosity": logging.INFO,
    "n_cross_validations": 5
}
```

Use la configuración de entrenamiento definida como parámetro `**kwargs` en un objeto `AutoMLConfig`. Además, puede especificar los datos de entrenamiento y el tipo de modelo, `regression` en este caso.

```python
from azureml.train.automl import AutoMLConfig

automl_config = AutoMLConfig(task='regression',
                             debug_log='automated_ml_errors.log',
                             training_data=x_train,
                             label_column_name="totalAmount",
                             **automl_settings)
```

> [!NOTE]
> Los pasos previos al procesamiento del aprendizaje automático (normalización de características, control de los datos que faltan, conversión de valores de texto a numéricos, etc.) se convierten en parte del modelo subyacente. Cuando se utiliza el modelo para las predicciones, se aplican automáticamente a los datos de entrada los mismos pasos previos al procesamiento que se aplican durante el entrenamiento.

### <a name="train-the-automatic-regression-model"></a>Entrenamiento del modelo de regresión automática

Cree un objeto de experimento en el área de trabajo. Un experimento actúa como un contenedor para las ejecuciones individuales. Pase el objeto `automl_config` definido al experimento y establezca la salida en `True` para ver el progreso durante la ejecución.

Tras iniciar el experimento, la salida que se muestra se actualiza en directo a medida que se ejecuta. Para cada iteración, verá el tipo de modelo, la duración de la ejecución y la precisión del entrenamiento. El campo `BEST` realiza el seguimiento de la mejor puntuación de entrenamiento en ejecución en función del tipo de métrica.

```python
from azureml.core.experiment import Experiment
experiment = Experiment(ws, "Tutorial-NYCTaxi")
local_run = experiment.submit(automl_config, show_output=True)
```

```output
Running on local machine
Parent Run ID: AutoML_1766cdf7-56cf-4b28-a340-c4aeee15b12b
Current status: DatasetFeaturization. Beginning to featurize the dataset.
Current status: DatasetEvaluation. Gathering dataset statistics.
Current status: FeaturesGeneration. Generating features for the dataset.
Current status: DatasetFeaturizationCompleted. Completed featurizing the dataset.
Current status: DatasetCrossValidationSplit. Generating individually featurized CV splits.
Current status: ModelSelection. Beginning model selection.

****************************************************************************************************
ITERATION: The iteration being evaluated.
PIPELINE: A summary description of the pipeline being evaluated.
DURATION: Time taken for the current iteration.
METRIC: The result of computing score on the fitted pipeline.
BEST: The best observed score thus far.
****************************************************************************************************

 ITERATION   PIPELINE                                       DURATION      METRIC      BEST
         0   StandardScalerWrapper RandomForest             0:00:16       0.8746    0.8746
         1   MinMaxScaler RandomForest                      0:00:15       0.9468    0.9468
         2   StandardScalerWrapper ExtremeRandomTrees       0:00:09       0.9303    0.9468
         3   StandardScalerWrapper LightGBM                 0:00:10       0.9424    0.9468
         4   RobustScaler DecisionTree                      0:00:09       0.9449    0.9468
         5   StandardScalerWrapper LassoLars                0:00:09       0.9440    0.9468
         6   StandardScalerWrapper LightGBM                 0:00:10       0.9282    0.9468
         7   StandardScalerWrapper RandomForest             0:00:12       0.8946    0.9468
         8   StandardScalerWrapper LassoLars                0:00:16       0.9439    0.9468
         9   MinMaxScaler ExtremeRandomTrees                0:00:35       0.9199    0.9468
        10   RobustScaler ExtremeRandomTrees                0:00:19       0.9411    0.9468
        11   StandardScalerWrapper ExtremeRandomTrees       0:00:13       0.9077    0.9468
        12   StandardScalerWrapper LassoLars                0:00:15       0.9433    0.9468
        13   MinMaxScaler ExtremeRandomTrees                0:00:14       0.9186    0.9468
        14   RobustScaler RandomForest                      0:00:10       0.8810    0.9468
        15   StandardScalerWrapper LassoLars                0:00:55       0.9433    0.9468
        16   StandardScalerWrapper ExtremeRandomTrees       0:00:13       0.9026    0.9468
        17   StandardScalerWrapper RandomForest             0:00:13       0.9140    0.9468
        18   VotingEnsemble                                 0:00:23       0.9471    0.9471
        19   StackEnsemble                                  0:00:27       0.9463    0.9471
```

## <a name="explore-the-results"></a>Exploración de los resultados

Explore los resultados del entrenamiento automático con un [widget de Jupyter](/python/api/azureml-widgets/azureml.widgets). El widget le permite ver un grafo y una tabla de todas las iteraciones de ejecución individuales, junto con los metadatos y las métricas de precisión del entrenamiento. Además, puede filtrar por métricas de precisión diferentes de la principal con el selector desplegable.

```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```

![Detalles de la ejecución del widget de Jupyter](./media/tutorial-auto-train-models/automl-dash-output.png)
![Trazado del widget de Jupyter](./media/tutorial-auto-train-models/automl-chart-output.png)

### <a name="retrieve-the-best-model"></a>Recuperación del mejor modelo

Seleccione el mejor modelo de las iteraciones. La función `get_output` devuelve la mejor ejecución y el modelo ajustado de la última invocación de ajuste. Mediante el uso de las sobrecargas en `get_output`, puede recuperar la mejor ejecución y el modelo ajustado para cualquier métrica registrada o para una iteración concreta.

```python
best_run, fitted_model = local_run.get_output()
print(best_run)
print(fitted_model)
```

### <a name="test-the-best-model-accuracy"></a>Prueba de la precisión del mejor modelo

Use el mejor modelo para ejecutar predicciones en el conjunto de datos de prueba para predecir las tarifas del taxi. La función `predict` usa el mejor modelo y predice los valores de "y", **precio del recorrido**, a partir del conjunto de datos `x_test`. Imprima los 10 primeros valores de costo predichos a partir de `y_predict`.

```python
y_test = x_test.pop("totalAmount")

y_predict = fitted_model.predict(x_test)
print(y_predict[:10])
```

Calcule el valor `root mean squared error` de los resultados. Convierta la trama de datos `y_test` en una lista para compararla con los valores predichos. La función `mean_squared_error` toma dos matrices de valores y calcula el error medio al cuadrado entre ellos. Al tomar la raíz cuadrada del resultado, se produce un error en las mismas unidades que la variable y, **costo**. Indica aproximadamente cuánto se alejan las predicciones de la tarifa del taxi de las reales.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

y_actual = y_test.values.flatten().tolist()
rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

Ejecute el siguiente código para calcular el error medio absoluto porcentual (MAPE) usando los conjuntos de datos `y_actual` y `y_predict` completos. Esta métrica calcula una diferencia absoluta entre cada valor predicho y real, y suma todas las diferencias. A continuación, expresa esa suma como porcentaje del total de los valores reales.

```python
sum_actuals = sum_errors = 0

for actual_val, predict_val in zip(y_actual, y_predict):
    abs_error = actual_val - predict_val
    if abs_error < 0:
        abs_error = abs_error * -1

    sum_errors = sum_errors + abs_error
    sum_actuals = sum_actuals + actual_val

mean_abs_percent_error = sum_errors / sum_actuals
print("Model MAPE:")
print(mean_abs_percent_error)
print()
print("Model Accuracy:")
print(1 - mean_abs_percent_error)
```

```output
Model MAPE:
0.14353867606052823

Model Accuracy:
0.8564613239394718
```


A partir de las dos métricas de precisión de la predicción, verá que el modelo es bastante bueno en la predicción de las tarifas de taxi a partir de las características del conjunto de datos, normalmente en un margen de +/-4,00 dólares estadounidenses y con un error del 15 %, aproximadamente.

El proceso tradicional de desarrollo de modelos de aprendizaje automático consume muchos recursos y requiere conocimiento del dominio y una inversión de tiempo significativa para ejecutar y comparar los resultados de docenas de modelos. El uso del aprendizaje automático automatizado es una manera estupenda de probar muchos modelos distintos para el escenario rápidamente.

## <a name="clean-up-resources"></a>Limpieza de recursos

No complete esta sección si planea ejecutar otros tutoriales de Azure Machine Learning.

### <a name="stop-the-compute-instance"></a>Detención de la instancia de proceso

[!INCLUDE [aml-stop-server](../../includes/aml-stop-server.md)]

### <a name="delete-everything"></a>Eliminar todo el contenido

Si no va a usar los recursos creados, elimínelos para no incurrir en cargos.

1. En Azure Portal, seleccione **Grupos de recursos** a la izquierda del todo.
1. En la lista, seleccione el grupo de recursos que creó.
1. Seleccione **Eliminar grupo de recursos**.
1. Escriba el nombre del grupo de recursos. A continuación, seleccione **Eliminar**.

También puede mantener el grupo de recursos pero eliminar una sola área de trabajo. Muestre las propiedades del área de trabajo y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de aprendizaje automático, ha realizado las tareas siguientes:

> [!div class="checklist"]
> * Ha configurado un área de trabajo y ha preparado datos para un experimento.
> * Ha realizado un entrenamiento mediante un modelo de regresión automatizado localmente con parámetros personalizados.
> * Ha explorado y revisado los resultados del entrenamiento.

[Implemente el modelo](tutorial-deploy-models-with-aml.md) con Azure Machine Learning.
