---
title: Detección del desfase de datos en los conjuntos de datos (versión preliminar)
titleSuffix: Azure Machine Learning
description: Aprenda a configurar la detección del desfase de datos en Azure Learning. Cree monitores de conjuntos de datos (versión preliminar), supervise el desfase de datos y configure alertas.
services: machine-learning
ms.service: machine-learning
ms.subservice: mldata
ms.reviewer: sgilley
ms.author: wibuchan
author: buchananwp
ms.date: 10/21/2021
ms.topic: how-to
ms.custom: data4ml, contperf-fy21q2
ms.openlocfilehash: f80489726617b05e4a4c025893fdb24ec1f2d05c
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132335716"
---
# <a name="detect-data-drift-preview-on-datasets"></a>Detección del desfase de datos (versión preliminar) en los conjuntos de datos

Obtenga información sobre cómo supervisar el desfase de datos y establecer alertas cuando el desfase sea alto.  

Con los monitores del conjunto de datos de Azure Machine Learning (versión preliminar), puede:
* **Analizar el desfase de los datos** para comprender cómo cambian con el tiempo.
* **Supervisar los datos del modelo** para conocer las diferencias entre los conjuntos de datos de entrenamiento y de servicio.  Comience por [recopilar datos de modelo de los modelos implementados](how-to-enable-data-collection.md).
* **Supervisar los datos nuevos** para conocer las diferencias entre los conjuntos de datos de destino y los de referencia.
* **Perfilar características en los datos** para realizar un seguimiento de cómo cambian las propiedades estadísticas con el tiempo.
* **Configurar alertas sobre el desfase de datos** para tener advertencias tempranas de posibles problemas. 
* **[Crear una nueva versión del conjunto de datos](how-to-version-track-datasets.md)** al determinar que los datos se han desfasado demasiado.

Para crear el monitor, se usa un [conjunto de datos de Azure Machine Learning](how-to-create-register-datasets.md). El conjunto de datos debe incluir una columna de marca de tiempo.

Puede ver las métricas de desfase de datos con el SDK de Python o en Azure Machine Learning Studio.  Se pueden encontrar otras métricas e información detallada a través del recurso de [Azure Aplicación Insights](../azure-monitor/app/app-insights-overview.md) asociado al área de trabajo de Azure Machine Learning.

> [!IMPORTANT]
> La detección de un desfase de datos en conjuntos de datos se encuentra actualmente en versión preliminar pública.
> Se ofrece la versión preliminar sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

Para crear y trabajar con conjuntos de datos, necesita:
* Suscripción a Azure. Si no tiene una suscripción de Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://azure.microsoft.com/free/).
* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).
* El [SDK de Azure Machine Learning para Python instalado](/python/api/overview/azure/ml/install), que incluye el paquete azureml-datasets.
* Datos estructurados (tabulares) con una marca de tiempo especificada en la ruta de acceso del archivo, el nombre de archivo o la columna de los datos.

## <a name="what-is-data-drift"></a>¿Qué es el desfase de datos?

El desfase de datos es una de las principales razones por las que la precisión del modelo empeora con el tiempo.  Para los modelos de Machine Learning, el desfase de datos es el cambio en los datos de entrada del modelo que conduce a la degradación del rendimiento del modelo.  La supervisión del desfase de datos ayuda a detectar estos problemas de rendimiento del modelo.

Entre las causas del desfase de datos se incluyen:

- Cambios del proceso ascendente, como un sensor que se reemplaza que cambia las unidades de medida de pulgadas a centímetros. 
- Los problemas de calidad de los datos, como un sensor roto que siempre lee 0.
- Desfase natural en los datos, como la temperatura media que cambia con las estaciones.
- Cambio en relación entre las características o el turno covariable. 

Azure Machine Learning simplifica la detección de desfases al calcular una sola métrica que abstrae la complejidad de los conjuntos de datos que se comparan.  Estos conjuntos de datos pueden tener cientos de características y decenas de miles de filas. Una vez que se detecta el desfase, se exploran las características que lo causan.  A continuación, inspeccione las métricas a nivel de característica para depurar y aislar la causa principal del desfase.

Este enfoque de arriba abajo facilita el seguimiento de los datos en mayor medida que las técnicas tradicionales basadas en reglas. Las técnicas basadas en reglas, como el intervalo de datos permitidos o los valores únicos permitidos, pueden consumir mucho tiempo y ser propensos a errores.

En Azure Machine Learning, se usan monitores de conjunto de datos para detectar y alertar sobre el desfase de datos.
  
### <a name="dataset-monitors"></a>Monitores de conjuntos de datos 

Con un monitor de conjunto de datos puede:

* Detectar y avisar sobre el desfase de datos en los nuevos datos de un conjunto de datos.
* Analizar el desfase en datos históricos.
* Generar perfiles de datos nuevos a lo largo del tiempo.

El algoritmo de desfase de datos proporciona una medida global del cambio en los datos y la indicación de qué características son responsables de una investigación más detallada. Los monitores de conjuntos de datos generan otras métricas mediante la generación de perfiles de nuevos datos en el conjunto de datos de `timeseries`. 

Mediante [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) se pueden configurar alertas personalizadas en todas las métricas generadas por el monitor. Los monitores de conjuntos de datos se pueden usar para detectar rápidamente problemas de datos y reducir el tiempo necesario para depurar el problema mediante la identificación de las causas probables.  

Conceptualmente, hay tres escenarios principales para configurar los monitores de conjunto de datos en Azure Machine Learning.

Escenario | Descripción
---|---
Supervisión de datos de servicio de un modelo para el desfase de los datos de entrenamiento | Los resultados de este escenario se pueden interpretar como la supervisión de un proxy para la precisión del modelo, dado que la precisión del modelo se degrada si los datos de servicio se desfasan de los datos de entrenamiento.
Supervisión de un conjunto de datos de una serie temporal para el desfase desde un período de tiempo anterior | Este escenario es más general y se puede usar para supervisar los conjuntos de datos implicados en el flujo ascendente o descendente de la creación del modelo.  El conjunto de datos de destino debe tener una columna de marca de tiempo. El conjunto de datos de referencia puede ser cualquier conjunto de datos tabular que tenga características en común con el conjunto de datos de destino.
Análisis sobre datos pasados | Este escenario se puede utilizar para comprender los datos históricos e informar de las decisiones en la configuración de los monitores de conjunto de datos.

Los monitores del conjunto de datos dependen de los siguientes servicios de Azure.

|Servicio de Azure  |Descripción  |
|---------|---------|
| *Dataset* | El desfase usa conjuntos de datos de Machine Learning para recuperar los datos de entrenamiento y comparar los datos para el entrenamiento del modelo.  La generación de un perfil de datos se usa para generar algunas de las métricas que se han comunicado, como min, max, distinct values, distinct values count. |
| *Canalización y proceso de Azureml* | El trabajo de cálculo de desfase se hospeda en la canalización azureml.  El trabajo se desencadena a petición o mediante programación para ejecutarse en un proceso configurado en el momento de la creación del monitor de desfase.
| *Application Insights*| El desfase emite métricas a Application Insights que pertenecen al área de trabajo de Machine Learning.
| *Azure Blob Storage*| El desfase emite métricas en formato json a Azure Blob Storage.

### <a name="baseline-and-target-datasets"></a>Conjunto de datos de destino y de referencia 

Puede supervisar los [conjuntos de datos de Azure Machine Learning](how-to-create-register-datasets.md) para el desfase de datos. Al crear un monitor de conjunto de datos, hará referencia a:
* Conjunto de datos de referencia: normalmente, el conjunto de datos de entrenamiento para un modelo.
* Conjunto de datos de destino: normalmente, datos de entrada del modelo. Con el tiempo se compara con el conjunto de datos de referencia. Esta comparación significa que el conjunto de datos de destino debe tener especificada una columna de marca de tiempo.

El monitor comparará los conjuntos de datos de referencia y de destino.

## <a name="create-target-dataset"></a>Creación del conjunto de datos de destino

El conjunto de datos de destino debe tener configurado el rasgo `timeseries` especificando la columna de marca de tiempo de una columna de los datos o en una columna virtual derivada del patrón de ruta de los archivos. Cree el conjunto de datos con una marca de tiempo a través del [SDK de Python](#sdk-dataset) o [Azure Machine Learning Studio](#studio-dataset). Tiene que especificarse una columna que represente una "marca de tiempo" para poder agregar el rasgo `timeseries` al conjunto de datos. Si los datos se particionan en la estructura de carpetas con información temporal, como "{aaaaa/MM/dd}", cree una columna virtual mediante la configuración del patrón de ruta de acceso y establézcala como la "marca de tiempo de partición" para habilitar la funcionalidad de la API de serie temporal.

# <a name="python"></a>[Python](#tab/python)
<a name="sdk-dataset"></a>

El método [`Dataset`](/python/api/azureml-core/azureml.data.tabulardataset#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) de la clase [`with_timestamp_columns()`](/python/api/azureml-core/azureml.data.tabulardataset#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-) define la columna de marca de tiempo del conjunto de datos.

```python 
from azureml.core import Workspace, Dataset, Datastore

# get workspace object
ws = Workspace.from_config()

# get datastore object 
dstore = Datastore.get(ws, 'your datastore name')

# specify datastore paths
dstore_paths = [(dstore, 'weather/*/*/*/*/data.parquet')]

# specify partition format
partition_format = 'weather/{state}/{date:yyyy/MM/dd}/data.parquet'

# create the Tabular dataset with 'state' and 'date' as virtual columns 
dset = Dataset.Tabular.from_parquet_files(path=dstore_paths, partition_format=partition_format)

# assign the timestamp attribute to a real or virtual column in the dataset
dset = dset.with_timestamp_columns('date')

# register the dataset as the target dataset
dset = dset.register(ws, 'target')
```

> [!TIP]
> Para obtener un ejemplo completo de cómo usar el rasgo de `timeseries` de conjuntos de datos, vea el [cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb) o la [documentación del SDK de conjuntos de datos](/python/api/azureml-core/azureml.data.tabulardataset#with-timestamp-columns-timestamp-none--partition-timestamp-none--validate-false----kwargs-).

# <a name="studio"></a>[Estudio](#tab/azure-studio)

<a name="studio-dataset"></a>

Si crea un conjunto de datos mediante Azure Machine Learning Studio, asegúrese de que la ruta de acceso a los datos contiene información de marca de tiempo, incluya todas las subcarpetas con datos y establezca el formato de la partición.

En el ejemplo siguiente, se toman todos los datos de la subcarpeta *NoaaIsdFlorida/2019*, y el formato de la partición especifica el año, mes y día de la marca de tiempo.

[![Formato de partición](./media/how-to-monitor-datasets/partition-format.png)](media/how-to-monitor-datasets/partition-format-expand.png)

En la configuración de **Esquema**, especifique la columna **marca de tiempo** desde una columna virtual o real en el conjunto de datos especificado. Este tipo indica que los datos tienen un componente temporal. 

:::image type="content" source="media/how-to-monitor-datasets/timestamp.png" alt-text="Configuración de la marca de tiempo":::

Si los datos ya tienen particiones por fecha u hora, como es el caso aquí, también puede especificar la **marca de tiempo de partición**. Con ello se consigue un procesamiento más eficaz de fechas y se habilitan las API de serie temporal que puede utilizar durante el entrenamiento.

:::image type="content" source="media/how-to-monitor-datasets/timeseries-partitiontimestamp.png" alt-text="Marca de tiempo de partición":::

---

## <a name="create-dataset-monitor"></a>Creación de un monitor de conjunto de datos

Cree un monitor de conjunto de datos para detectar y alertar sobre el desfase de datos en un nuevo conjunto de datos.  Use el [SDK de Python](#sdk-monitor) o [Azure Machine Learning Studio](#studio-monitor).

# <a name="python"></a>[Python](#tab/python)
<a name="sdk-monitor"></a> Consulte la [Documentación de referencia del SDK de Python sobre el desfase de datos](/python/api/azureml-datadrift/azureml.datadrift) para obtener información completa. 

En el siguiente ejemplo se muestra cómo crear un monitor de conjunto de datos mediante el SDK de Python

```python
from azureml.core import Workspace, Dataset
from azureml.datadrift import DataDriftDetector
from datetime import datetime

# get the workspace object
ws = Workspace.from_config()

# get the target dataset
target = Dataset.get_by_name(ws, 'target')

# set the baseline dataset
baseline = target.time_before(datetime(2019, 2, 1))

# set up feature list
features = ['latitude', 'longitude', 'elevation', 'windAngle', 'windSpeed', 'temperature', 'snowDepth', 'stationName', 'countryOrRegion']

# set up data drift detector
monitor = DataDriftDetector.create_from_datasets(ws, 'drift-monitor', baseline, target, 
                                                      compute_target='cpu-cluster', 
                                                      frequency='Week', 
                                                      feature_list=None, 
                                                      drift_threshold=.6, 
                                                      latency=24)

# get data drift detector by name
monitor = DataDriftDetector.get_by_name(ws, 'drift-monitor')

# update data drift detector
monitor = monitor.update(feature_list=features)

# run a backfill for January through May
backfill1 = monitor.backfill(datetime(2019, 1, 1), datetime(2019, 5, 1))

# run a backfill for May through today
backfill1 = monitor.backfill(datetime(2019, 5, 1), datetime.today())

# disable the pipeline schedule for the data drift detector
monitor = monitor.disable_schedule()

# enable the pipeline schedule for the data drift detector
monitor = monitor.enable_schedule()
```

> [!TIP]
> Para ver un ejemplo completo de cómo configurar un conjunto de datos de `timeseries` y el detector de desfase de datos, consulte nuestro [cuaderno de ejemplos](https://aka.ms/datadrift-notebook).


# <a name="studio"></a>[Estudio](#tab/azure-studio)
<a name="studio-monitor"></a>

1. Diríjase a la [página principal de Studio](https://ml.azure.com).
1. Seleccione la pestaña **Conjuntos de datos** de la izquierda. 
1. Elija **Monitores de conjuntos de datos**.
   ![Lista de monitores](./media/how-to-monitor-datasets/monitor-list.png)

1. Haga clic en el botón **+Crear monitor** y continúe con el asistente; para ello, haga clic en **Siguiente**.  

:::image type="content" source="media/how-to-monitor-datasets/wizard.png" alt-text="Asistente para crear monitores":::

* **Seleccione el conjunto de datos de destino**.  El conjunto de datos de destino es un conjunto de datos tabular con columna de marca de tiempo especificada, que se analizará en términos de desfase de datos. El conjunto de datos de destino debe compartir características con el de referencia y debe ser un conjunto de datos de `timeseries` al que se anexan los nuevos datos. Los datos históricos del conjunto de datos de destino se pueden analizar o se pueden supervisar nuevos datos.

* **Seleccione el conjunto de datos de referencia.**  Seleccione el conjunto de datos tabular que se usará como referencia para la comparación del conjunto de datos de destino a lo largo del tiempo.  El conjunto de datos de referencia debe compartir características con el conjunto de datos de destino.  Seleccione un intervalo de tiempo para utilizar un segmento del conjunto de datos de destino, o especifique un conjunto de datos independiente para usarlo como referencia.

* **Supervise la configuración**.  Esta configuración es para la canalización del monitor de conjunto de datos programado que se creará. 

    | Configuración | Descripción | Sugerencias | Mutable | 
    | ------- | ----------- | ---- | ------- |
    | Nombre | Nombre del monitor del conjunto de datos. | | No |
    | Características | Lista de características que se analizarán para el desfase de datos a lo largo del tiempo. | Establézcalas en las características de salida de un modelo para medir el desfase del concepto. No incluya características que se desfasan de forma natural a lo largo del tiempo (mes, año, índice, etc.). Puede reposicionar el monitor de desfase de datos existente después de ajustar la lista de características. | Sí | 
    | Destino de proceso | Destino de proceso de Azure Machine Learning para ejecutar los trabajos del monitor de conjunto de datos. | | Sí | 
    | Habilitar | Habilitar o deshabilitar la programación en la canalización del monitor de conjunto de datos | Deshabilite esta programación para analizar los datos históricos con la configuración de reposición. Se puede habilitar una vez creado el monitor de conjunto de datos. | Sí | 
    | Frecuencia | Frecuencia que se usará para programar el trabajo de canalización y analizar los datos históricos si se ejecuta una reposición. Puede ser diario, semanal o mensual. | Cada ejecución compara los datos del conjunto de datos de destino de acuerdo con la frecuencia: <li>Diariamente: comparar el día completo más reciente del conjunto de datos de destino con la referencia <li>Semanalmente: comparar la semana completa más reciente (lunes-domingo) del conjunto de datos de destino con la referencia <li>Mensualmente: comparar el mes completo más reciente del conjunto de datos de destino con la referencia | No | 
    | Latencia | Tiempo, en horas, para que lleguen los datos en el conjunto de datos. Por ejemplo, si los datos tardan tres días en llegar a la instancia de SQL DB en la que se encapsula el conjunto de datos, establezca la latencia en 72. | No se puede cambiar una vez creado el monitor de conjunto de datos | No | 
    | Direcciones de correo | Direcciones de correo para alertas en función de la brecha del umbral de porcentaje del desfase de datos. | Los correos se envían a través de Azure Monitor. | Sí | 
    | Umbral | Umbral de porcentaje de desfase de datos para alertas de correo. | Se pueden establecer más alertas y eventos en muchas otras métricas del recurso de Application Insights asociado del área de trabajo. | Sí |

Una vez finalizado el asistente, el monitor de conjunto de resultados resultante aparecerá en la lista. Selecciónelo para ir a la página de detalles de ese monitor.

---

## <a name="understand-data-drift-results"></a>Descripción de los resultados del desfase de datos

En esta sección se muestran los resultados de la supervisión de un conjunto de datos, que se encuentra en la página **Conjuntos de datos** / **Monitores de conjuntos de datos** en Azure Studio.  Puede actualizar la configuración y analizar los datos existentes para un período de tiempo específico en esta página.  

Comience con la información de nivel superior sobre la magnitud del desfase de datos y el resaltado de las características que se van a investigar.

:::image type="content" source="media/how-to-monitor-datasets/drift-overview.png" alt-text="Información general sobre el desfase":::


| Métrica | Descripción | 
| ------ | ----------- | 
| Magnitud del desfase de datos | Un porcentaje de desfase entre la referencia y el conjunto de datos de destino a lo largo del tiempo. De 0 a 100, 0, donde 0 indica conjuntos de datos idénticos y 100 indica que el modelo de desfase de datos de Azure Machine Learning puede indicar por completo los dos conjuntos de datos. Se espera un ruido en el porcentaje exacto medido debido a las técnicas de aprendizaje automático que se usan para generar esta magnitud. | 
| Principales características de desfase | Muestra las características del conjunto de datos que presentan un desfase más importante y, en consecuencia, contribuyen en mayor medida a la métrica Magnitud del desfase. Debido al cambio de covariable, no es necesario que la distribución subyacente de una característica cambie para tener una importancia de la característica relativamente alta. |
| Umbral | La magnitud del desfase de datos más allá del umbral establecido desencadenará alertas. Se puede configurar en la configuración del monitor. | 

### <a name="drift-magnitude-trend"></a>Tendencia de la magnitud del desfase

Vea cómo el conjunto de datos difiere del conjunto de datos de destino en el período de tiempo especificado.  Cuanto más cerca del 100 %, más se diferencian los dos conjuntos de datos.

:::image type="content" source="media/how-to-monitor-datasets/drift-magnitude.png" alt-text="Tendencia de la magnitud del desfase":::

### <a name="drift-magnitude-by-features"></a>Magnitud del desfase según las características

Esta sección contiene información a nivel de características sobre el cambio en la distribución de la característica seleccionada, así como otras estadísticas, a lo largo del tiempo.

El conjunto de datos de destino también se perfila a lo largo del tiempo. La distancia estadística entre la distribución de referencia de cada característica se compara con el conjunto de datos de destino a lo largo del tiempo.  Conceptualmente, esto es similar a la magnitud del desfase de datos.  Sin embargo, esta distancia estadística es para una característica individual y no para todas las características. También están disponibles Mín, Máx y Media.

En Estudio de Azure Machine Learning, haga clic en una barra del gráfico para ver los detalles del nivel de características de esa fecha. De forma predeterminada, ve la distribución del conjunto de datos de referencia y la distribución de la ejecución más reciente de la misma característica.

:::image type="content" source="media/how-to-monitor-datasets/drift-by-feature.gif" alt-text="Magnitud del desfase según las características":::

Estas métricas también se pueden recuperar en el SDK de Python a través del método `get_metrics()` en un objeto `DataDriftDetector`.

### <a name="feature-details"></a>Detalles de la característica

Por último, desplácese hacia abajo para ver los detalles de cada característica individual.  Use las listas desplegables sobre el gráfico para seleccionar la característica y, además, seleccione la métrica que desea ver.

:::image type="content" source="media/how-to-monitor-datasets/numeric-feature.gif" alt-text="Gráfico de características numéricas y comparación":::

Las métricas del gráfico dependen del tipo de característica.

* Características numéricas

    | Métrica | Descripción |  
    | ------ | ----------- |  
    | Distancia de Wasserstein | Cantidad mínima de trabajo para transformar la distribución de la línea base en la distribución de destino. |
    | Valor medio | Valor promedio de la característica. |
    | Valor mínimo | Valor mínimo de la característica. |
    | Valor máximo | Valor máximo de la característica. |

* Características de categorías
    
    | Métrica | Descripción |  
    | ------ | ----------- |  
    | Distancia euclidiana     |  Se calcula para columnas de categorías.  La distancia euclidiana se calcula sobre dos vectores, generados a partir de la distribución empírica de la misma columna de categorías de dos conjuntos de valores.  0 indica que no hay ninguna diferencia en las distribuciones empíricas.    Cuanto más se desvíe de 0, mayor desfase habrá en la columna.  Se pueden observar tendencias a partir de un trazado de serie temporal de esta métrica, y puede resultar útil para revelar una característica de desfase.  |
    | Valores únicos | Número de valores únicos (cardinalidad) de la característica. |

En este gráfico, seleccione una sola fecha para comparar la distribución de características entre el destino y esta fecha para la característica mostrada. En el caso de las características numéricas, se muestran dos distribuciones de probabilidad.  Si la característica es numérica, se muestra un gráfico de barras.

:::image type="content" source="media/how-to-monitor-datasets/select-date-to-compare.gif" alt-text="Selección de una fecha para comparar con el destino":::

## <a name="metrics-alerts-and-events"></a>Métricas, alertas y eventos

Las métricas se pueden consultar en el recurso de [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) asociado con el área de trabajo de aprendizaje automático. Tiene acceso a todas las características de Application Insights, entre las que se incluye la configuración de reglas de alertas personalizadas y grupos de acciones para desencadenar una acción como un correo electrónico/SMS/notificación push/mensaje de voz o una función de Azure. Para más información, consulte la documentación de Application Insights completa. 

Para empezar, vaya a [Azure Porta](https://portal.azure.com)l y seleccione la página **Información general** de su área de trabajo.  El recurso de Application Insights asociado está en el extremo derecho:

[![Información general de Azure Portal](./media/how-to-monitor-datasets/ap-overview.png)](media/how-to-monitor-datasets/ap-overview-expanded.png)

Seleccione Registros (Analytics) en Supervisión, en el panel izquierdo:

![Información general de Application Insights](./media/how-to-monitor-datasets/ai-overview.png)

Las métricas del monitor de conjuntos de datos se almacenan como `customMetrics`. Para verlas, puede escribir y ejecutar una consulta después de configurar un monitor de conjuntos de datos:

[![Consulta de Log Analytics](./media/how-to-monitor-datasets/simple-query.png)](media/how-to-monitor-datasets/simple-query-expanded.png)

Después de identificar las métricas para configurar reglas de alerta, cree una regla:

![Nueva alerta de reglas](./media/how-to-monitor-datasets/alert-rule.png)

Para definir la acción que se debe realizar cuando se cumplan las condiciones establecidas, puede usar un grupo de acciones existente, o bien crear uno:

![Grupo de acciones nuevo](./media/how-to-monitor-datasets/action-group.png)


## <a name="troubleshooting"></a>Solución de problemas

Limitaciones y problemas conocidos de los monitores de desfase de datos:

* El intervalo de tiempo del análisis de datos históricos está limitado a 31 intervalos de la configuración de frecuencia del monitor. 
* Limitación de 200 características, a menos que no se especifique ninguna lista de características (se usan todas las características).
* El tamaño de proceso debe ser lo suficientemente grande como para controlar los datos.
* Asegúrese de que el conjunto de datos tiene datos dentro de las fechas de inicio y finalización de una ejecución de monitor determinada.
* Los monitores del conjunto de datos solo funcionarán en conjuntos de datos que contengan 50 filas, o más.
* Las columnas, o características, del conjunto de datos se clasifican como categóricas o numéricas en función de las condiciones de la tabla siguiente. Si la característica no cumple estas condiciones, por ejemplo, una columna de tipo cadena con > 100 valores únicos, la característica se quita de nuestro algoritmo de desfase de datos, pero se perfila aún así. 

    | Tipo de característica | Tipo de datos | Condición | Limitaciones | 
    | ------------ | --------- | --------- | ----------- |
    | Categorías | string, bool, int, float | El número de valores únicos de la característica es menor que 100 y menor que el 5 % del número de filas. | NULL se trata como su propia categoría. | 
    | Numérico | int, float | Los valores de la característica son de un tipo de datos numérico y no cumplen la condición de una característica de categoría. | Característica quitada si más del 15 % de los valores son NULL. | 

* Cuando haya creado un monitor de desfase de datos, pero no pueda ver datos en la página **Monitores de conjuntos de datos** en Azure Machine Learning Studio, intente lo siguiente.

    1. Compruebe si ha seleccionado el intervalo de fechas correcto en la parte superior de la página.  
    1. En la pestaña **Monitores de conjuntos de datos**, seleccione el vínculo de experimento para comprobar el estado de la ejecución.  El vínculo se encuentra en el extremo derecho de la tabla.
    1. Si la ejecución se completó correctamente, compruebe los registros del controlador para ver el número de métricas que se han generado o si hay algún mensaje de advertencia.  Busque registros de controladores en la pestaña **Output + logs** (Salida y registros) después de hacer clic en un experimento.

* Si la función `backfill()` del SDK no genera la salida esperada, puede deberse a un problema de autenticación.  Cuando cree el proceso para pasar esta función, no utilice `Run.get_context().experiment.workspace.compute_targets`.  En su lugar, use una [ServicePrincipalAuthentication](/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication) como la siguiente para crear el proceso que se pasa en esa función `backfill()`: 

  ```python
   auth = ServicePrincipalAuthentication(
          tenant_id=tenant_id,
          service_principal_id=app_id,
          service_principal_password=client_secret
          )
   ws = Workspace.get("xxx", auth=auth, subscription_id="xxx", resource_group="xxx")
   compute = ws.compute_targets.get("xxx")
   ```

* En el recopilador de datos del modelo, puede tardar hasta 10 minutos para que lleguen los datos a la cuenta de Blob Storage (pero normalmente, menos). En un script o Notebook, espere 10 minutos para asegurarse de que se ejecutarán las celdas siguientes.

    ```python
    import time
    time.sleep(600)
    ```

## <a name="next-steps"></a>Pasos siguientes

* Vaya a [Azure Machine Learning Studio](https://ml.azure.com) o al [cuaderno de Python](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datadrift-tutorial/datadrift-tutorial.ipynb) para configurar un monitor de conjunto de datos.
* Vea cómo configurar un desfase de datos en los [modelos implementados en Azure Kubernetes Service](./how-to-enable-data-collection.md).
* Configure los monitores de desfase de un conjunto de datos con [Event Grid](how-to-use-event-grid.md).
