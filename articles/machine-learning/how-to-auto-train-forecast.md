---
title: Configuración de AutoML para la previsión de series temporales
titleSuffix: Azure Machine Learning
description: Configure AutoML de Azure Machine Learning para entrenar modelos de previsión de series temporales con el SDK de Azure Machine Learning para Python.
services: machine-learning
author: nibaccam
ms.author: nibaccam
ms.service: machine-learning
ms.subservice: automl
ms.topic: how-to
ms.custom: contperf-fy21q1, automl, FY21Q4-aml-seo-hack
ms.date: 06/11/2021
ms.openlocfilehash: f8e036a77603c1e0833117a4562ad9dfd93c7ac9
ms.sourcegitcommit: f29615c9b16e46f5c7fdcd498c7f1b22f626c985
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2021
ms.locfileid: "129427198"
---
# <a name="set-up-automl-to-train-a-time-series-forecasting-model-with-python"></a>Configuración de AutoML para entrenar un modelo de previsión de series temporales con Python

En este artículo, obtendrá información sobre cómo configurar el entrenamiento de AutoML para modelos de previsión de series temporales con el aprendizaje automático automatizado de Azure Machine Learning en el [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/).

Para ello, haremos lo siguiente: 

> [!div class="checklist"]
> * Preparar los datos para el modelado de series temporales.
> * Configurar parámetros específicos de las serie temporales en un objeto [`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig).
> * Ejecutar predicciones con los datos de serie temporal.

Para una experiencia con poco código, consulte [Tutorial: Previsión de la demanda con aprendizaje automático automatizado](tutorial-automated-ml-forecast.md) para ver un ejemplo de previsión de series temporales con AutoML en [Estudio de Azure Machine Learning](https://ml.azure.com/).

A diferencia de los métodos de series temporales clásicos, en el aprendizaje automático automatizado, los valores de series temporales anteriores se "dinamizan" para convertirse en dimensiones adicionales del regresor, junto con otros indicadores. Este enfoque incorpora varias variables contextuales y su relación entre sí durante el entrenamiento. Dado que varios factores pueden influir en una previsión, este método se adapta bien a los escenarios de previsión reales. Por ejemplo, al prever ventas, las interacciones de las tendencias históricas, la tasa de cambio y el precio son motores conjuntos del resultado de ventas. 


## <a name="prerequisites"></a>Requisitos previos

Para realizar este artículo, necesitará lo siguiente 

* Un área de trabajo de Azure Machine Learning. Para crear el área de trabajo, consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

* En este artículo se presupone una familiarización con la configuración de un experimento de aprendizaje de automático automatizado. Siga el [tutorial](tutorial-auto-train-models.md) o los [procedimientos](how-to-configure-auto-train.md) para ver los principales modelos de diseño del experimento de aprendizaje automático automatizado.

    [!INCLUDE [automl-sdk-version](../../includes/machine-learning-automl-sdk-version.md)]
## <a name="preparing-data"></a>Preparación de los datos

La diferencia más importante entre el tipo de tarea de regresión de previsión y el de regresión en ML automatizado es que se incluye una característica en los datos que representa una serie de tiempo válida. Una serie de tiempo normal tiene una frecuencia coherente y bien definida, y un valor en cada punto de un intervalo de tiempo continuo. 

Tenga en cuenta la siguiente instantánea de un archivo `sample.csv`.
Este es un conjunto de datos de ventas diarios para una empresa que tiene dos almacenes diferentes, A y B. 

Además, hay características para

 *  `week_of_year`: permite que el modelo detecte la estacionalidad semanal.
* `day_datetime`: representa una serie temporal limpia con una frecuencia diaria.
* `sales_quantity`: columna de destino para la ejecución de predicciones. 

```output
day_datetime,store,sales_quantity,week_of_year
9/3/2018,A,2000,36
9/3/2018,B,600,36
9/4/2018,A,2300,36
9/4/2018,B,550,36
9/5/2018,A,2100,36
9/5/2018,B,650,36
9/6/2018,A,2400,36
9/6/2018,B,700,36
9/7/2018,A,2450,36
9/7/2018,B,650,36
```


Lea los datos en una trama de datos de Pandas y use la función `to_datetime` para asegurarse de que la serie temporal sea de tipo `datetime`.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["day_datetime"] = pd.to_datetime(data["day_datetime"])
```

En este caso los datos ya están ordenados de manera ascendente por el campo de hora `day_datetime`. Sin embargo, al configurar un experimento, asegúrese de que la columna de tiempo deseada esté ordenada de manera ascendente para compilar una serie de tiempo válida. 

El código siguiente: 
* Asume que los datos contienen 1000 registros; y realiza una división determinista en los datos para crear conjuntos de datos de entrenamiento y prueba. 
* Identifica la columna de etiqueta como `sales_quantity`.
* Separa el campo de etiqueta de `test_data` para formar el conjunto `test_target`.

```python
train_data = data.iloc[:950]
test_data = data.iloc[-50:]

label =  "sales_quantity"
 
test_labels = test_data.pop(label).values
```

> [!IMPORTANT]
> Al entrenar un modelo para la previsión de valores futuros, asegúrese de que todas las características del entrenamiento se pueden usar al ejecutar predicciones para su horizonte previsto. <br> <br>Por ejemplo, al crear una previsión de demanda, incluir una característica para el precio de cotización actual podría aumentar la precisión del entrenamiento de manera exponencial. Sin embargo, si quiere una previsión con un horizonte lejano, no es posible predecir con precisión valores de cotización futuros correspondientes a momentos futuros en la serie temporal y la precisión del modelo podría verse afectada.

<a name="config"></a>

## <a name="training-and-validation-data"></a>Datos de entrenamiento y validación

Puede especificar conjuntos distintos de entrenamiento y validación directamente en el objeto `AutoMLConfig`.   Obtenga más información sobre [AutoMLConfig](#configure-experiment).

En el caso de la previsión de series temporales, para la validación solo se usa la **validación cruzada de origen variable (ROCV)** de manera predeterminada. Pase los datos de entrenamiento y validación juntos y establezca el número de iteraciones de validación cruzada con el parámetro `n_cross_validations` en `AutoMLConfig`. ROCV divide la serie en datos de entrenamiento y validación con un punto temporal de origen. Al deslizar el origen en el tiempo, se generan subconjuntos de validación cruzada. Esta estrategia conserva la integridad de los datos de la serie temporal y elimina el riesgo de perder datos.

![Validación cruzada de origen variable](./media/how-to-auto-train-forecast/ROCV.svg)

También puede traer sus propios datos de validación. Obtenga más información en [Configuración de las divisiones de datos y la validación cruzada en aprendizaje automático automatizado](how-to-configure-cross-validation-data-splits.md#provide-validation-data).


```python
automl_config = AutoMLConfig(task='forecasting',
                             n_cross_validations=3,
                             ...
                             **time_series_settings)
```

Obtenga más información sobre cómo AutoML aplica la validación cruzada para [evitar un sobreajuste de los modelos](concept-manage-ml-pitfalls.md#prevent-overfitting).

## <a name="configure-experiment"></a>Configuración del experimento

El objeto [`AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) define la configuración y los datos necesarios para una tarea de aprendizaje automático automatizado. La configuración de un modelo de previsión es similar a la configuración de un modelo de regresión estándar, pero existen determinados modelos, opciones de configuración y pasos de caracterización que son específicos para los datos de series temporales. 

### <a name="supported-models"></a>Modelos admitidos
El aprendizaje automático automatizado prueba automáticamente diferentes algoritmos y modelos como parte de la creación del modelo y del proceso de optimización. Como usuario, no hay ninguna necesidad de especificar el algoritmo. En el caso de los experimentos de previsión, los modelos nativos de serie temporal y de aprendizaje profundo forman parte del sistema de recomendaciones. En la tabla siguiente se resume este subconjunto de modelos. 

>[!Tip]
> Los modelos de regresión tradicionales también se prueban como parte del sistema de recomendaciones para los experimentos de previsión. Vea la [tabla de modelos compatibles](how-to-configure-auto-train.md#supported-models) para la lista completa de modelos. 

Modelos| Descripción | Ventajas
----|----|---
Prophet (versión preliminar)|Prophet funciona mejor con series temporales que tienen efectos estacionales fuertes y varias estaciones de datos históricos. Para aprovechar este modelo, instálelo localmente mediante `pip install fbprophet`. | Es preciso y rápido, sólido para valores atípicos, datos que faltan y cambios drásticos de la serie temporal.
Auto-ARIMA (versión preliminar)|La media móvil integrada autorregresiva (ARIMA) funciona mejor cuando los datos son estacionales. Esto significa que sus propiedades estadísticas, como la media y la varianza, son constantes en todo el conjunto. Por ejemplo, si lanza una moneda al aire, la probabilidad de obtener cara es del 50 %, independientemente de si la lanza hoy, mañana o el año siguiente.| Excelente para las series univariables, ya que los valores anteriores se usan para predecir los futuros.
ForecastTCN (versión preliminar)| ForecastTCN es un modelo de red neuronal diseñado para abordar las tareas de previsión más exigentes. Captura tendencias locales y globales no lineales en los datos y las relaciones entre series temporales.|Capaz de aprovechar tendencias complejas en los datos y escalar fácilmente a los mayores conjuntos de datos.

### <a name="configuration-settings"></a>Parámetros de configuración

Al igual que en un problema de regresión, debe definir los parámetros de entrenamiento estándar como tipo de tarea, número de iteraciones, datos de entrenamiento y número de validaciones cruzadas. Para las tareas de previsión existen parámetros adicionales que se deben establecer y que afectan al experimento. 

En la tabla siguiente se resumen estos parámetros adicionales. Consulte la [documentación de referencia para la clase ForecastingParameter](/python/api/azureml-automl-core/azureml.automl.core.forecasting_parameters.forecastingparameters) para obtener más información sobre los patrones de diseño de sintaxis.

| Nombre del&nbsp;parámetro | Descripción | Obligatorio |
|-------|-------|-------|
|`time_column_name`|Se utiliza para especificar la columna de fecha y hora en los datos de entrada utilizada que se usa para compilar la serie temporal y se deduce su frecuencia.|✓|
|`forecast_horizon`|Define el número de períodos futuros que le gustaría pronosticar. El horizonte está en las unidades de la frecuencia de la serie temporal. Las unidades se basan en el intervalo de tiempo de los datos de entrenamiento (por ejemplo, semanales, mensuales) que debe predecir el pronosticador.|✓|
|`enable_dnn`|[Habilitar las DNN de previsión]().||
|`time_series_id_column_names`|Nombres de columna que se usan para identificar de forma única la serie temporal en los datos que tienen varias filas con la misma marca de tiempo. Si no se definen los identificadores de serie temporal, el conjunto de datos se presupone una serie temporal. Para más información sobre las series temporales únicas, consulte [energy_demand_notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand).||
|`freq`| Frecuencia del conjunto de datos de la serie temporal. Este parámetro representa el período según el cual se espera que se produzcan eventos, y cuenta con valores como "diario", "semanal", "anual", etc. La frecuencia debe ser un [alias de desplazamiento de Pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects). Obtenga más información sobre la [frecuencia].(#frequency-target-data-aggregation)||
|`target_lags`|Número de filas para retrasar los valores de destino en función de la frecuencia de los datos. El retraso se representa como una lista o un entero único. El retraso se debe usar cuando la relación entre las variables independientes y la variable dependiente no coincide o está en correlación de forma predeterminada. ||
|`feature_lags`| El ML automatizado decidirá automáticamente las características que se van a retardar cuando se establezcan `target_lags` y `feature_lags` se establezca en `auto`. Habilitar los retardos de características puede ayudarle a mejorar la precisión. Los retardos de características están deshabilitados de forma predeterminada. ||
|`target_rolling_window_size`|*n* períodos históricos que se utilizarán para generar valores previstos, < = tamaño del conjunto de entrenamiento. Si se omite, *n* es el tamaño total del conjunto de entrenamiento. Especifique este parámetro si solo desea tener en cuenta una determinada cantidad de historial al entrenar el modelo. Más información sobre [agregación de ventanas con desplazamiento de objetivo](#target-rolling-window-aggregation).||
|`short_series_handling_config`| Permite controlar series temporales breves para evitar errores provocados por la falta de datos durante el entrenamiento. De manera predeterminada, el control de series breves está establecido en `auto`. Más información sobre el [control de series breves](#short-series-handling).||
|`target_aggregation_function`| Esta es la función que se va a usar para agregar la columna de destino de la serie temporal de acuerdo con la frecuencia especificada a través del parámetro `freq`. El parámetro `freq` se debe establecer para poder usar `target_aggregation_function`. El valor predeterminado es `None`; por lo que el uso de `sum` es suficiente en la mayoría de escenarios.<br> Obtenga más información sobre la [agregación de la columna de destino](#frequency--target-data-aggregation). 


El código siguiente: 
* Usa la clase [`ForecastingParameters`](/python/api/azureml-automl-core/azureml.automl.core.forecasting_parameters.forecastingparameters) para definir los parámetros de previsión que se van a emplear en el entrenamiento del experimento.
* Establece `time_column_name` en el campo `day_datetime` en el conjunto de datos. 
* Define el parámetro `time_series_id_column_names` en `"store"`. Esto garantiza que se creen **dos grupos de series temporales independientes** para los datos, uno para los almacenes A y B.
* Establece el `forecast_horizon` en 50 para predecir el conjunto de pruebas completo. 
* Establece una ventana de previsión en 10 períodos con `target_rolling_window_size`
* Especifica un retardo único en los valores de destino para dos períodos por delante con el parámetro `target_lags`. 
* Establece `target_lags` en el valor "auto" recomendado, que detectará automáticamente este valor.

```python
from azureml.automl.core.forecasting_parameters import ForecastingParameters

forecasting_parameters = ForecastingParameters(time_column_name='day_datetime', 
                                               forecast_horizon=50,
                                               time_series_id_column_names=["store"],
                                               freq='W',
                                               target_lags='auto',
                                               target_rolling_window_size=10)
                                              
```

Estos valores `forecasting_parameters` se pasan en el objeto de `AutoMLConfig` estándar junto con el tipo de tarea `forecasting`, la métrica principal, los criterios de salida y los datos de entrenamiento. 

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             experiment_timeout_minutes=15,
                             enable_early_stopping=True,
                             training_data=train_data,
                             label_column_name=label,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **forecasting_parameters)
```

La cantidad de datos necesarios para entrenar correctamente un modelo de previsión con aprendizaje automático automatizado se ve influida por los valores de `forecast_horizon`, `n_cross_validations` y `target_lags` o `target_rolling_window_size` especificados al configurar `AutoMLConfig`. 

La fórmula siguiente calcula la cantidad de datos históricos que se necesitarían para construir características de serie temporal.

Datos históricos mínimos necesarios: (2x `forecast_horizon`) + #`n_cross_validations` + max(max(`target_lags`), `target_rolling_window_size`)

Se generará una excepción de error para todas las series del conjunto de datos que no cumplan con la cantidad necesaria de datos históricos para la configuración pertinente especificada. 

### <a name="featurization-steps"></a>Pasos de caracterización

En todos los experimentos de aprendizaje automático automatizado se aplican técnicas de escalado automático y normalización a los datos de forma predeterminada. Estas técnicas son tipos de **caracterización** que ayudan a *ciertos* algoritmos que son sensibles a las características a diferentes escalas. Más información sobre los pasos de caracterización predeterminados en [caracterización en AutoML](how-to-configure-auto-features.md#automatic-featurization)

Sin embargo, los siguientes pasos solo se realizan para los tipos de tarea `forecasting`:

* Detectar la frecuencia de muestreo de la serie temporal (por ejemplo, cada hora, cada día, cada semana, etc.) y crear nuevos registros para los momentos ausentes para que la serie sea continua.
* Imputar los valores que faltan en las columnas de destino (copia de los valores nulos hacia adelante) y de característica (con la mediana de los valores de la columna).
* Crear características basadas en los identificadores de serie temporal para habilitar los efectos fijos en las diferentes series.
* Crear características basadas en el tiempo para ayudar a aprender los patrones estacionales.
* Codificar las variables categóricas en cantidades numéricas.

Para obtener un resumen de las características que se crean como resultado de estos pasos, consulte [Transparencia de caracterización](how-to-configure-auto-features.md#featurization-transparency).

> [!NOTE]
> Los pasos de la caracterización del aprendizaje automático automatizado (normalización de características, control de los datos que faltan, conversión de valores de texto a numéricos, etc.) se convierten en parte del modelo subyacente. Cuando se usa el modelo para realizar predicciones, se aplican automáticamente a los datos de entrada los mismos pasos de caracterización que se aplican durante el entrenamiento.

#### <a name="customize-featurization"></a>Personalización de la caracterización

También tiene la opción de personalizar los valores de la caracterización para asegurarse de que los datos y las características que se usan para entrenar el modelo de Machine Learning generan predicciones pertinentes. 

Las personalizaciones admitidas para tareas `forecasting` incluyen:

|Personalización|Definición|
|--|--|
|**Actualización del propósito de la columna**|Invalida el tipo de característica detectado automáticamente para la columna especificada.|
|**Actualización de parámetros del transformador** |Actualizar los parámetros para el transformador especificado. Actualmente admite *Imputer* (fill_value y median).|
|**Quitar columnas** |Especifica las columnas que se van a eliminar de la caracterización.|

Para personalizar las caracterizaciones con el SDK, especifique `"featurization": FeaturizationConfig` en el objeto `AutoMLConfig`. Más información sobre [caracterizaciones personalizadas](how-to-configure-auto-features.md#customize-featurization).

>[!NOTE]
> La funcionalidad **Quitar columnas** está en desuso a partir de la versión 1.19 del SDK. Quite columnas del conjunto de datos como parte de la limpieza de datos antes de consumirlos en el experimento de aprendizaje automático automatizado. 

```python
featurization_config = FeaturizationConfig()

# `logQuantity` is a leaky feature, so we remove it.
featurization_config.drop_columns = ['logQuantitity']

# Force the CPWVOL5 feature to be of numeric type.
featurization_config.add_column_purpose('CPWVOL5', 'Numeric')

# Fill missing values in the target column, Quantity, with zeroes.
featurization_config.add_transformer_params('Imputer', ['Quantity'], {"strategy": "constant", "fill_value": 0})

# Fill mising values in the `INCOME` column with median value.
featurization_config.add_transformer_params('Imputer', ['INCOME'], {"strategy": "median"})
```

Si usa Azure Machine Learning Studio para el experimento, consulte [Personalización de la caracterización en Studio](how-to-use-automated-ml-for-ml-models.md#customize-featurization).

## <a name="optional-configurations"></a>Configuraciones opcionales

Hay configuraciones opcionales adicionales disponibles para las tareas de previsión, como la habilitación del aprendizaje profundo y la especificación de una agregación de ventana con desplazamiento de objetivo. 

### <a name="frequency--target-data-aggregation"></a>Frecuencia y agregación de datos de destino

Aproveche la frecuencia, el parámetro `freq`, para evitar errores que pueden causar los datos irregulares, es decir, datos que no siguen una cadencia establecida, como datos de cada hora o diarios. 

En cuanto a los datos que son muy irregulares o en las distintas necesidades empresariales, los usuarios pueden establecer opcionalmente la frecuencia de previsión que quieren (`freq`), y especificar el valor `target_aggregation_function` para agregar la columna de destino de la serie temporal. Aprovechar estos dos valores en el objeto `AutoMLConfig` puede ahorrarle tiempo a la hora de preparar los datos. 

Cuando se usa el parámetro `target_aggregation_function`,
* Los valores de la columna de destino se agregan en función de la operación especificada. Normalmente, `sum` es adecuado para la mayoría de los escenarios.

* Las columnas de predicción numéricas de los datos se agregan en función de la suma, la media, el valor mínimo y el valor máximo. Como resultado, los ML automatizados generan nuevas columnas con el sufijo del nombre de la función de agregación y aplican la operación de agregado seleccionada. 

* En el caso de las columnas de predicción de categorías, los datos se agregan en función del modo, que es la categoría más destacada en la ventana.

* Las columnas de predicción de fecha se agregan en función del valor mínimo, el valor máximo y el modo. 

Las operaciones de agregación admitidas para los valores de columna de destino incluyen:

|Función | description
|---|---
|`sum`| La suma de los valores de destino
|`mean`| La media o promedio de los valores de destino
|`min`| El valor mínimo de un destino  
|`max`| El valor máximo de un destino  

### <a name="enable-deep-learning"></a>Habilitar el aprendizaje profundo.

> [!NOTE]
> La compatibilidad con DNN para la previsión en el aprendizaje automático automatizado se encuentra en **versión preliminar** y no es compatible con las ejecuciones locales ni las ejecuciones iniciadas en Databricks.

También puede aplicar el aprendizaje profundo con redes neuronal profundas, o DNN, para mejorar las puntuaciones del modelo. El aprendizaje profundo de ML automatizado permite pronosticar datos de series temporales de variable única y de varias variables.

Los modelos de aprendizaje profundo tienen tres capacidades intrínsecas:
1. Pueden aprender de las asignaciones arbitrarias de las entradas a las salidas.
1. Admiten varias entradas y salidas.
1. Pueden extraer automáticamente patrones en datos de entrada que abarquen secuencias largas. 

Para habilitar el aprendizaje profundo, establezca el `enable_dnn=True` en el objeto `AutoMLConfig`.

```python
automl_config = AutoMLConfig(task='forecasting',
                             enable_dnn=True,
                             ...
                             **forecasting_parameters)
```
> [!Warning]
> Al habilitar DNN para los experimentos creados con el SDK, las [mejores explicaciones del modelo](how-to-machine-learning-interpretability-automl.md) se deshabilitan.

Para habilitar DNN para un experimento de AutoML creado en Azure Machine Learning Studio, consulte los [procedimientos para la configuración del tipo de tarea](how-to-use-automated-ml-for-ml-models.md#create-and-run-experiment).

Consulte en el [cuaderno de previsión de producción de bebidas](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb) un ejemplo de código detallado que aprovecha las DNN.

### <a name="target-rolling-window-aggregation"></a>Agregación de ventanas con desplazamiento de objetivo
A menudo, la mejor información que puede tener una previsión es el valor reciente del objetivo.  Las agregaciones de ventanas con desplazamiento permiten la incorporación de una agregación gradual de valores de datos como características. La generación y el uso de estas características como datos contextuales extra contribuye a la precisión del modelo de entrenamiento.

Por ejemplo, suponga que desea predecir la demanda de energía. Puede que desee agregar una característica de ventana con desplazamiento de tres días para tener en cuenta los cambios térmicos de los espacios calentados. En este ejemplo, cree esta ventana estableciendo `target_rolling_window_size= 3` en el constructor `AutoMLConfig`. 

En la tabla se muestra el diseño de características que se produce cuando se aplica la agregación de ventanas. Las columnas para los valores **mínimo, máximo** y **suma** se generan en una ventana deslizante de tres según la configuración definida. Cada fila tiene una nueva característica calculada, en el caso de la marca de tiempo del 8 de septiembre de 2017 4:00 a. m., los valores máximo, mínimo y suma se calculan con los **valores de la demanda** del 8 de septiembre de 2017, de la 1:00 a. m. a las 3:00 a. m. Esta ventana tiene tres turnos juntos para rellenar los datos de las filas restantes.

![Ventana con desplazamiento de objetivo](./media/how-to-auto-train-forecast/target-roll.svg)

Consulte un ejemplo de código de Python que aplica las [características para agregar ventanas con desplazamiento de objetivo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb).

### <a name="short-series-handling"></a>Control de series breves

El ML automatizado considera una serie temporal como **serie breve** si no hay suficientes puntos de datos para llevar a cabo las fases de entrenamiento y validación del desarrollo del modelo. El número de puntos de datos varía para cada experimento y depende de max_horizon, el número de divisiones de validación cruzada y la longitud de la retrospectiva del modelo, que es el máximo de historial necesario para construir las características de la serie temporal. Para ver el cálculo exacto, consulte la [documentación de referencia de short_series_handling_configuration](/python/api/azureml-automl-core/azureml.automl.core.forecasting_parameters.forecastingparameters#short-series-handling-configuration).

El aprendizaje automático automatizado ofrece un control de series breves de manera predeterminada con el parámetro `short_series_handling_configuration` en el objeto `ForecastingParameters`. 

Para habilitar el control de series breves, también se debe definir el parámetro `freq`. Para definir una frecuencia por hora, estableceremos `freq='H'`. Consulte [aquí](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#dateoffset-objects) las opciones de cadenas de frecuencia. Para cambiar el comportamiento predeterminado, `short_series_handling_configuration = 'auto'`, actualice el parámetro `short_series_handling_configuration` en el objeto `ForecastingParameter`.  

```python
from azureml.automl.core.forecasting_parameters import ForecastingParameters

forecast_parameters = ForecastingParameters(time_column_name='day_datetime', 
                                            forecast_horizon=50,
                                            short_series_handling_configuration='auto',
                                            freq = 'H',
                                            target_lags='auto')
```
En la siguiente tabla se resumen las opciones disponibles para `short_series_handling_config`.
 
|Configuración|Descripción
|---|---
|`auto`| El siguiente es el comportamiento predeterminado para el control de series breves: <li> *Si todas las series son breves*, se rellenan los datos. <br> <li> *Si no todas las series son breves*, se anulan las series breves. 
|`pad`| Si `short_series_handling_config = pad`, el aprendizaje automático automatizado agrega valores aleatorios a cada serie breve que se encuentra. A continuación se enumeran los tipos de columna y con qué se rellenan: <li>Columnas de objetos con NaN <li> Columnas numéricas con 0 <li> Columnas booleanas o lógicas con False <li> La columna de destino se rellena con valores aleatorios con una media de cero y una desviación estándar de 1. 
|`drop`| Si `short_series_handling_config = drop`, el aprendizaje automático automatizado quita las series breves, y no se usarán para el entrenamiento ni la predicción. Las predicciones para estas series devolverán NaN.
|`None`| No se rellena ni anula ninguna serie

>[!WARNING]
>El relleno puede afectar a la precisión del modelo resultante, ya que estamos introduciendo datos artificiales tan solo para superar el entrenamiento sin errores. <br> <br> Si muchas de las series son breves, puede que también vea algún impacto en los resultados de la capacidad de explicación.

## <a name="run-the-experiment"></a>Ejecución del experimento 

Cuando tenga el objeto `AutoMLConfig` listo, puede enviar el experimento. Una vez finalizado el modelo, recupere la iteración con la mejor ejecución.

```python
ws = Workspace.from_config()
experiment = Experiment(ws, "Tutorial-automl-forecasting")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
``` 
 
## <a name="forecasting-with-best-model"></a>Previsión con el mejor modelo

Use la mejor iteración del modelo para la previsión de valores para el conjunto de datos de prueba.

La función [forecast_quantiles()](/python/api/azureml-train-automl-client/azureml.train.automl.model_proxy.modelproxy#forecast-quantiles-x-values--typing-any--y-values--typing-union-typing-any--nonetype----none--forecast-destination--typing-union-typing-any--nonetype----none--ignore-data-errors--bool---false-----azureml-data-abstract-dataset-abstractdataset) permite especificaciones de cuándo se deben iniciar las predicciones, a diferencia del método `predict()`, que se usa normalmente para las tareas de clasificación y regresión. El método forecast_quantiles() genera de manera predeterminada una previsión de punto o una previsión de media o mediana que no tiene un cono de incertidumbre en torno a ella. 

En el siguiente ejemplo, primero se reemplazan todos los valores de `y_pred` con `NaN`. En este caso, el origen de la previsión está al final de los datos de entrenamiento. Sin embargo, si reemplazó solo la segunda mitad de `y_pred` con `NaN`, la función dejó intactos los valores numéricos de la primera, pero realizó la previsión de los valores de `NaN` de la segunda mitad. La función devuelve tanto los valores previstos como las características alineadas.

También puede usar el parámetro `forecast_destination` de la función `forecast_quantiles()` para la previsión de valores hasta una fecha especificada.

```python
label_query = test_labels.copy().astype(np.float)
label_query.fill(np.nan)
label_fcst, data_trans = fitted_model.forecast_quantiles(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```

A menudo, los clientes quieren comprender las predicciones en un cuantil específico de la distribución. Por ejemplo, cuando se usa la previsión para controlar el inventario, como artículos de alimentación o máquinas virtuales para un servicio en la nube. En tales casos, el punto de control suele ser algo parecido a "queremos que el elemento tenga existencias y no esté agotado el 99 % del tiempo". A continuación se muestra cómo especificar qué cuantiles desea ver para las predicciones, como el percentil 50 o 95. Si no especifica un cuantil, como en el ejemplo de código mencionado anteriormente, solo se generan las predicciones del percentil 50. 

```python
# specify which quantiles you would like 
fitted_model.quantiles = [0.05,0.5, 0.9]
fitted_model.forecast_quantiles(
    test_data, label_query, forecast_destination=pd.Timestamp(2019, 1, 8))
```
 
Calcule la raíz del error cuadrático medio (RMSE) entre los valores de `actual_labels` reales y los previstos en `predict_labels`.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(actual_labels, predict_labels))
rmse
```
 
 
Ahora que se ha determinado la precisión del modelo global, el siguiente paso más realista es usar este para prever valores futuros desconocidos. 

Proporcione un conjunto de datos en el mismo formato que el de prueba (`test_data`) pero con fechas y horas futuras y el conjunto de predicción resultante son los valores previstos para cada paso de la serie temporal. Supongamos que los últimos registros de la serie temporal en el conjunto de datos fueron del 31/12/2018. Para prever la demanda para el día siguiente (o para los períodos que necesite previsión, <= `forecast_horizon`), cree un único registro de serie de tiempo para cada almacén para el 01/01/2019.

```output
day_datetime,store,week_of_year
01/01/2019,A,1
01/01/2019,A,1
```

Repita los pasos necesarios para cargar estos datos futuros a una trama de datos y ejecute `best_run.forecast_quantiles(test_data)` para predecir los valores futuros.

> [!NOTE]
> Las predicciones en el ejemplo no se admiten para la previsión con aprendizaje automático automatizado cuando se habilitan `target_lags` o `target_rolling_window_size`.

## <a name="forecasting-at-scale"></a>Previsión a escala 

Hay escenarios en los que un único modelo de aprendizaje automático no es suficiente y se necesitan varios modelos de aprendizaje automático. Por ejemplo, predecir las ventas de cada tienda de una marca por separado o adaptar una experiencia a usuarios individuales. La creación de un modelo para cada instancia puede dar lugar a mejores resultados en muchos problemas de aprendizaje automático. 

La agrupación es un concepto de previsión de series temporales que permite combinar estas para entrenar un modelo individual por grupo. Este enfoque puede ser especialmente útil si tiene series temporales que requieren suavizado o relleno o entidades en el grupo que pueden beneficiarse del historial o las tendencias de otras entidades. Muchos modelos y previsiones de series temporales jerárquicas son soluciones basadas en el aprendizaje automático automatizado para estos escenarios de previsión a gran escala. 

### <a name="many-models"></a>Many Models

La solución de muchos modelos de Azure Machine Learning con aprendizaje automático automatizado permite a los usuarios entrenar y administrar millones de modelos en paralelo. Muchos modelos El acelerador de soluciones aprovecha las [canalizaciones de Azure Machine Learning](concept-ml-pipelines.md) para entrenar el modelo. En concreto, se utiliza un objeto [Pipeline](/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29) y [ParalleRunStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallelrunstep) que requieren unos parámetros de configuración específicos establecidos a través de [ParallelRunConfig](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallelrunconfig). 


En el diagrama siguiente se muestra el flujo de trabajo de la solución de muchos modelos. 

![Diagrama de concepto de muchos modelos](./media/how-to-auto-train-forecast/many-models.svg)

En el código siguiente se muestran los parámetros clave que los usuarios necesitan para configurar la ejecución de muchos modelos.

```python
from azureml.train.automl.runtime._many_models.many_models_parameters import ManyModelsTrainParameters

partition_column_names = ['Store', 'Brand']
automl_settings = {"task" : 'forecasting',
                   "primary_metric" : 'normalized_root_mean_squared_error',
                   "iteration_timeout_minutes" : 10, #This needs to be changed based on the dataset. Explore how long training is taking before setting this value 
                   "iterations" : 15,
                   "experiment_timeout_hours" : 1,
                   "label_column_name" : 'Quantity',
                   "n_cross_validations" : 3,
                   "time_column_name": 'WeekStarting',
                   "max_horizon" : 6,
                   "track_child_runs": False,
                   "pipeline_fetch_max_batch_size": 15,}

mm_paramters = ManyModelsTrainParameters(automl_settings=automl_settings, partition_column_names=partition_column_names)

```

### <a name="hierarchical-time-series-forecasting"></a>Previsión de series temporales jerárquicas

En la mayoría de las aplicaciones, los clientes tienen la necesidad de comprender sus previsiones a nivel macro y micro de la empresa; ya sea prediciendo las ventas de productos en diferentes ubicaciones geográficas, o comprendiendo la demanda de personal prevista para las diferentes organizaciones de una empresa. La capacidad de entrenar un modelo de aprendizaje automático para prever de forma inteligente los datos de la jerarquía es esencial. 

Una serie temporal jerárquica es una estructura en la que cada una de las series únicas se organiza en una jerarquía basada en dimensiones como, por ejemplo, ubicación geográfica o tipo de producto. En el ejemplo siguiente se muestran datos con atributos únicos que forman una jerarquía. Nuestra jerarquía se define por el tipo de producto, como auriculares o tabletas, la categoría de producto, que divide los tipos de producto en accesorios y dispositivos, y la región en la que se venden los productos. 

![Tabla de datos sin procesar de ejemplo para datos jerárquicos](./media/how-to-auto-train-forecast/hierarchy-data-table.svg)
 
Para visualizarlo aún más, los niveles hoja de la jerarquía contienen todas las series temporales con combinaciones únicas de valores de atributo. Cada nivel superior de la jerarquía tiene en cuenta una dimensión menos para definir la serie temporal y agrega cada conjunto de nodos secundarios del nivel inferior a un nodo primario.
 
![Objeto visual de jerarquía para datos](./media/how-to-auto-train-forecast/data-tree.svg)

La solución de serie temporal jerárquica se basa en la solución de muchos modelos y comparte una configuración similar.

En el código siguiente se muestran los parámetros clave para configurar las ejecuciones de la previsión de series temporales jerárquicas. 

```python

from azureml.train.automl.runtime._hts.hts_parameters import HTSTrainParameters

model_explainability = True

engineered_explanations = False # Define your hierarchy. Adjust the settings below based on your dataset.
hierarchy = ["state", "store_id", "product_category", "SKU"]
training_level = "SKU"# Set your forecast parameters. Adjust the settings below based on your dataset.
time_column_name = "date"
label_column_name = "quantity"
forecast_horizon = 7


automl_settings = {"task" : "forecasting",
                   "primary_metric" : "normalized_root_mean_squared_error",
                   "label_column_name": label_column_name,
                   "time_column_name": time_column_name,
                   "forecast_horizon": forecast_horizon,
                   "hierarchy_column_names": hierarchy,
                   "hierarchy_training_level": training_level,
                   "track_child_runs": False,
                   "pipeline_fetch_max_batch_size": 15,
                   "model_explainability": model_explainability,# The following settings are specific to this sample and should be adjusted according to your own needs.
                   "iteration_timeout_minutes" : 10,
                   "iterations" : 10,
                   "n_cross_validations": 2}

hts_parameters = HTSTrainParameters(
    automl_settings=automl_settings,
    hierarchy_column_names=hierarchy,
    training_level=training_level,
    enable_engineered_explanations=engineered_explanations
)
```

## <a name="example-notebooks"></a>Cuadernos de ejemplo

Consulte los [cuadernos de ejemplo de previsión](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) para ejemplos de código detallados de la configuración de predicciones avanzada, que incluye:

* [detección y caracterización de festividades](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [validación cruzada de origen variable](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [retrasos configurables](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-bike-share/auto-ml-forecasting-bike-share.ipynb)
* [características de agregado en períodos acumulados](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)
* [DNN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [cómo y dónde implementar un modelo](how-to-deploy-and-where.md).
* Obtenga más información sobre la [Capacidad de interpretación: explicaciones de los modelos en el aprendizaje automático automatizado (versión preliminar)](how-to-machine-learning-interpretability-automl.md). 
* Consulte [Tutorial: Entrenamiento de un modelo de regresión con AutoML y Python](tutorial-auto-train-models.md) para obtener un ejemplo completo para crear experimentos con aprendizaje automático automatizado.
