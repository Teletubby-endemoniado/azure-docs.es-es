---
title: Qué es el aprendizaje automático automatizado AutoML
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo Azure Machine Learning puede generar automáticamente un modelo con los parámetros y los criterios que le proporcione con el aprendizaje automático automatizado.
services: machine-learning
ms.service: machine-learning
ms.subservice: automl
ms.topic: conceptual
author: cartacioS
ms.author: sacartac
ms.date: 07/01/2021
ms.custom: automl
ms.openlocfilehash: c08eae9654e01fda15889ac6fe65f99de2d73bb6
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130261812"
---
# <a name="what-is-automated-machine-learning-automl"></a>¿Qué es el aprendizaje automático automatizado (AutoML)?

El aprendizaje automático automatizado, también denominado ML automatizado o AutoML, es el proceso de automatizar las tareas lentas e iterativas del desarrollo de modelos de Machine Learning. Permite que los desarrolladores, analistas y científicos de datos creen modelos de aprendizaje automático con un escalado, eficiencia y productividad altos, al mismo tiempo que mantiene la calidad del modelo. El aprendizaje automático de Azure Machine Learning se basa en una innovación de la [división Microsoft Research](https://www.microsoft.com/research/project/automl/).

El desarrollo de modelos de Machine Learning tradicional consume muchos recursos, que requieren un conocimiento del dominio y tiempo significativos para generar y comparar docenas de modelos. Gracias al aprendizaje automático automatizado, reducirá el tiempo necesario para obtener modelos de aprendizaje automático listos para producción con gran eficiencia y facilidad.

## <a name="ways-to-use-automl-in-azure-machine-learning"></a>Formas de usar AutoML en Azure Machine Learning

Azure Machine Learning ofrece las dos experiencias siguientes para trabajar con el aprendizaje automático automatizado. Consulte las secciones siguientes para comprender la [disponibilidad de las características en cada experiencia](#parity).

* Para clientes con experiencia en código, el [SDK de Azure Machine Learning para Python](/python/api/overview/azure/ml/intro).  Introducción con el [Tutorial: Uso del aprendizaje automático para predecir tarifas de taxi](tutorial-auto-train-models.md).

* Para clientes con poca experiencia en código o ninguna, Estudio de Azure Machine Learning en [https://ml.azure.com](https://ml.azure.com/).  Comience por estos tutoriales:
    * [Tutorial: Creación de un modelo de clasificación con aprendizaje automático automatizado en Azure Machine Learning](tutorial-first-experiment-automated-ml.md).
    *  [Tutorial: Previsión de la demanda con aprendizaje automático automatizado](tutorial-automated-ml-forecast.md)

<a name="parity"></a>

## <a name="automl-settings-and-configuration"></a>Valores y configuración de AutoML

### <a name="experiment-settings"></a>Configuración del experimento 

Los valores siguientes le permiten configurar el experimento de aprendizaje automático automatizado. 

| |SDK de Python|La experiencia web de Studio|
|----|:----:|:----:|
|**Divide los datos en conjuntos de entrenamiento y validación**| ✓|✓
|**Admite tareas de Machine Learning: clasificación, regresión y predicción**| ✓| ✓
|**Admite tareas de Computer Vision (versión preliminar): clasificación de imágenes, detección de objetos y segmentación de instancias**| ✓| 
|**Optimiza en función de la métrica principal**| ✓| ✓
|**Admite el proceso de Azure Machine Learning como destino de proceso** | ✓|✓
|**Configura el horizonte de previsión, los intervalos de destino y el período acumulado**|✓|✓
|**Establece criterios de salida** |✓|✓ 
|**Establece el número de iteraciones simultáneas**| ✓|✓
|**Quitar columnas**| ✓|✓
|**Bloquea algoritmos**|✓|✓
|**Validación cruzada** |✓|✓
|**Admite el aprendizaje en clústeres de Azure Databricks**| ✓|
|**Muestra nombres de características con ingeniería**|✓|
|**Resumen de características**| ✓|
|**Caracterización para festivos**|✓|
|**Niveles de detalle del archivo de registro**| ✓|

### <a name="model-settings"></a>Configuración del modelo

Estos valores se pueden aplicar al mejor modelo como resultado del experimento de aprendizaje automático automatizado.

| |SDK de Python|La experiencia web de Studio|
|----|:----:|:----:|
|**Registro, implementación y explicación mejores del modelo**| ✓|✓|
|**Habilita los modelos del conjunto de votación y el conjunto de pila**| ✓|✓|
|**Muestra el mejor modelo en función de una métrica que no es la principal**|✓||
|**Habilita o deshabilita la compatibilidad con el modelo de ONNX**|✓||
|**Prueba del modelo** | ✓| |

### <a name="run-control-settings"></a>Ejecuta la configuración de un control

Estos valores le permiten revisar y controlar las ejecuciones de los experimentos y sus ejecuciones secundarias. 

| |SDK de Python|La experiencia web de Studio|
|----|:----:|:----:|
|**Ejecuta una tabla de resumen**| ✓|✓|
|**Cancelar ejecuciones y ejecuciones secundarias**| ✓|✓|
|**Obtiene límites de protección**| ✓|✓|
|**Pausar y reanudar ejecuciones**| ✓| |

## <a name="when-to-use-automl-classification-regression-forecasting--computer-vision"></a>Cuándo usar AutoML: clasificación, regresión, previsión y Computer Vision

Aplique aprendizaje automático automatizado cuando quiera que Azure Machine Learning entrene y ajuste un modelo automáticamente con la métrica de destino que especifique. El aprendizaje automático automatizado democratiza el proceso de desarrollo del modelo de Machine Learning y permite que los usuarios (independientemente de su experiencia en ciencia de datos) identifiquen una canalización de aprendizaje automático completa para cualquier problema.

Los científicos de datos, analistas y desarrolladores de todos los sectores pueden usar el aprendizaje automático para:
+ Implementar soluciones de ML sin grandes conocimientos de programación.
+ Ahorrar tiempo y recursos.
+ Aprovechar los procedimientos recomendados de la ciencia de datos.
+ Proporcionar soluciones ágiles a los problemas.

### <a name="classification"></a>clasificación

La clasificación es una tarea común del aprendizaje automático. La clasificación es un tipo de aprendizaje supervisado en el que los modelos aprenden mediante datos de entrenamiento y aplican lo que han aprendido a nuevos datos. Azure Machine Learning ofrece caracterizaciones específicas para estas tareas, como los caracterizadores de texto de red neuronal profunda para la clasificación. Más información sobre las [opciones de caracterización](how-to-configure-auto-features.md#featurization). 

El objetivo principal de los modelos de clasificación es predecir en qué categorías se incluirán los nuevos datos en función de lo aprendido de los datos de entrenamiento. Algunos ejemplos comunes de clasificación son la detección de fraudes, el reconocimiento de escritura a mano y la detección de objetos. Obtenga más información y vea un ejemplo en [Creación de un modelo de clasificación con aprendizaje automático](tutorial-first-experiment-automated-ml.md).

Consulte ejemplos de clasificación y aprendizaje automático automatizado en estos cuadernos de Python: [Detección de fraudes](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb), [Predicción de Marketing](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) y [Clasificación de datos de grupo de noticias](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb)

### <a name="regression"></a>Regresión

De forma similar a la clasificación, las tareas de regresión también son una tarea de aprendizaje supervisada común. Azure Machine Learning ofrece [caracterizaciones específicamente para estas tareas](how-to-configure-auto-features.md#featurization).

A diferencia de la clasificación, donde los valores de salida pronosticados son categóricos, los modelos de regresión predicen valores de salida numéricos basados en predicciones independientes. En la regresión, el objetivo es ayudar a establecer la relación entre esas variables de predicción independientes mediante la estimación de cómo una variable afecta a las otras. Por ejemplo, el precio de un automóvil según características como, el kilometraje de gas, la clasificación de seguridad, etc. Puede encontrar más información y ver un ejemplo en el artículo sobre la [regresión con aprendizaje automático automatizado](tutorial-auto-train-models.md).

Vea ejemplos de regresión y aprendizaje automático automatizado para predicciones en estos cuadernos de Python: [Predicción del rendimiento de la CPU](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-explanation-featurization/auto-ml-regression-explanation-featurization.ipynb), 

### <a name="time-series-forecasting"></a>Previsión de series temporales

La creación de previsiones es una parte integral de cualquier empresa, ya sea de los ingresos, los inventarios, las ventas o los clientes. Puede usar el aprendizaje automático automatizado para combinar las técnicas y enfoques y obtener una previsión recomendada y de alta calidad de series temporales. Puede encontrar más información en este artículo de procedimientos: [Aprendizaje automático automatizado para un modelo de previsión de series temporales](how-to-auto-train-forecast.md). 

Un experimento automatizado de series temporales se trata como un problema de regresión multivariante. Los valores de series temporales anteriores se "dinamizan" para convertirse en dimensiones adicionales para el regresor junto con otros indicadores. Este enfoque, a diferencia de los métodos clásicos de series temporales, tiene la ventaja de incorporar de forma natural varias variables contextuales y su relación entre sí durante el entrenamiento. El aprendizaje automático automatizado aprende un modelo único (a menudo, internamente bifurcado) para todos los elementos en el conjunto de datos y horizontes de predicción. Por tanto, hay más datos disponibles para calcular los parámetros del modelo, y se hace posible la generalización hasta series totalmente nuevas.

La configuración de previsión avanzada incluye:
* detección y caracterización de festividades
* aprendizaje de series temporales y DNN (ARIMA, Prophet, ForecastTCN)
* compatibilidad con gran cantidad de modelos a través de la agrupación
* validación cruzada de origen variable
* retardos configurables
* características de agregado en periodos acumulados


Vea ejemplos de regresión y aprendizaje automático automatizado para predicciones en estos cuadernos de Python: [Previsión de ventas](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb), [Previsión de la demanda](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb) y [Previsión de producción de bebidas](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb).

### <a name="computer-vision-preview"></a>Computer Vision (versión preliminar)

> [!IMPORTANT]
> Esta característica actualmente está en su versión preliminar pública. Esta versión preliminar se proporciona sin un contrato de nivel de servicio. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

ML automatizado para imágenes (versión preliminar) agrega compatibilidad con tareas de Computer Vision, lo que permite generar fácilmente modelos entrenados en datos de imagen para escenarios como la clasificación de imágenes y la detección de objetos. 

Con esta funcionalidad, puede: 
 
* Integrar sin problemas con la funcionalidad de [etiquetado de datos de Azure Machine Learning](./how-to-create-image-labeling-projects.md).
* Usar datos etiquetados para generar modelos de imagen.
* Optimizar el rendimiento del modelo especificando el algoritmo de modelo y ajustando los hiperparámetros. 
* Descargar o implementar el modelo resultante como un servicio web en Azure Machine Learning. 
* Operacionalizar a gran escala, aprovechando las funcionalidades de [MLOps](concept-model-management-and-deployment.md) y [canalizaciones de ML](concept-ml-pipelines.md) de Azure Machine Learning. 

La creación de modelos de aprendizaje automático automatizado para tareas de visión se admite a través del SDK de Python de Azure ML. Se puede acceder a las ejecuciones, modelos y salidas de experimentación resultantes desde la UI de Estudio de Azure Machine Learning.

Aprenda a [configurar el entrenamiento de AutoML para modelos de Computer Vision](how-to-auto-train-image-models.md).

![Ejemplos de tareas de Computer Vision. Imagen de: http://cs231n.stanford.edu/slides/2021/lecture_15.pdf](./media/concept-automated-ml/automl-computer-vision-tasks.png) Imagen de: http://cs231n.stanford.edu/slides/2021/lecture_15.pdf

ML automatizado para imágenes admite las siguientes tareas de Computer Vision: 

Tarea | Descripción
----|----
Clasificación de imágenes de varias clases | Tareas en las que una imagen se clasifica con una sola etiqueta de un conjunto de clases; por ejemplo, cada imagen se clasifica como una imagen de un "gato", un "perro" o un "pato".
Clasificación de imágenes con varias etiquetas | Tareas en las que una imagen podría tener una o varias etiquetas de un conjunto de etiquetas; por ejemplo, una imagen podría etiquetarse con "gato" y "perro".
Detección de objetos| Tareas para identificar objetos en una imagen y localizar cada objeto con un rectángulo delimitador, por ejemplo, localizar todos los perros y gatos de una imagen y dibujar un rectángulo delimitador alrededor de cada uno.
Segmentación de instancias | Tareas para identificar objetos de una imagen en el nivel de píxel, dibujando un polígono alrededor de cada objeto de la imagen.

## <a name="how-automated-ml-works"></a>Funcionamiento del aprendizaje automático

Durante el entrenamiento, Azure Machine Learning crea una serie de canalizaciones en paralelo que prueban distintos parámetros y algoritmos. El servicio recorre en iteración los algoritmos de ML que corresponden con las selecciones de características, de forma que cada iteración genera un modelo con una puntuación de entrenamiento. Cuanto mayor sea la puntuación, mejor se "ajustará" el modelo a sus datos.  Se detendrá una vez que logre los criterios de salida definidos en el experimento. 

Si usa **Azure Machine Learning**, puede diseñar y ejecutar sus experimentos de entrenamiento de Machine Learning automatizado mediante los siguientes pasos:

1. **Identifique el problema de Machine Learning** que quiere solucionar: clasificación, previsión, regresión o Computer Vision (versión preliminar).

1. **Elija si desea usar el SDK de Python o la experiencia web de Studio**: Aprenda sobre la paridad entre el [SDK de Python y la experiencia web de Studio](#parity).

   * Si busca una experiencia sin código o limitada, pruebe la experiencia web de Azure Machine Learning Studio en [https://ml.azure.com](https://ml.azure.com/).  
   * Los desarrolladores de Python pueden consultar el [SDK de Python de Azure Machine Learning](how-to-configure-auto-train.md). 
    
1. **Especifique el origen y el formato de los datos de entrenamiento etiquetados**: matrices de Numpy o dataframes de Pandas.

1. **Configure el destino de proceso para el entrenamiento del modelo**, puede ser [un equipo local, los procesos de Azure Machine Learning, las máquinas virtuales remotas o Azure Databricks](how-to-set-up-training-targets.md).

1. **Configure los parámetros de aprendizaje de automático automatizado** que determinan el número de iteraciones en diferentes modelos, las configuraciones de hiperparámetros, la caracterización y preprocesamiento de datos y las métricas que se deben observar para seleccionar al mejor modelo.  
1. **Envíe la ejecución del entrenamiento.**

1. **Revisión de los resultados** 

En el siguiente diagrama se muestra este proceso. 
![Aprendizaje automático automatizado](./media/concept-automated-ml/automl-concept-diagram2.png)


También puede inspeccionar la información de ejecución registrada, que [contiene las métricas](how-to-understand-automated-ml.md) que se recopilan durante la ejecución. La ejecución del entrenamiento genera un objeto serializado de Python (archivo `.pkl`) que contiene el modelo y el preprocesamiento de los datos.

Aunque se automatiza la creación del modelo, también puede [conocer las características importantes o pertinentes](how-to-configure-auto-train.md#explain) de los modelos generados.


> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]


## <a name="feature-engineering"></a>Ingeniería de características

La ingeniería de características es el proceso de usar el conocimiento de dominio de los datos para crear características que ayuden a mejorar los algoritmos de ML. En Azure Machine Learning, se aplican técnicas de escalado y normalización para facilitar la ingeniería de características. En conjunto, estas técnicas y la ingeniería de características se conocen como caracterización.

En el caso de los experimentos de aprendizaje automático automatizado, la caracterización se aplica automáticamente, pero también se puede personalizar en función de los datos. [Más información sobre qué caracterización se incluye](how-to-configure-auto-features.md#featurization).  

> [!NOTE]
> Los pasos de la caracterización del aprendizaje automático automatizado (normalización de características, control de los datos que faltan, conversión de valores de texto a numéricos, etc.) se convierten en parte del modelo subyacente. Cuando se usa el modelo para realizar predicciones, se aplican automáticamente a los datos de entrada los mismos pasos de caracterización que se aplican durante el entrenamiento.

### <a name="automatic-featurization-standard"></a>Caracterización automática (estándar)

En todos los experimentos de aprendizaje automático automatizado, los datos se escalan y se normalizan automáticamente para ayudar a que los algoritmos funcionen bien. Durante el entrenamiento del modelo, se aplicará una de las siguientes técnicas de escalado o normalización para todos los modelos. Obtenga información sobre cómo AutoML ayuda a [prevenir el sobreajuste y los datos desequilibrados](concept-manage-ml-pitfalls.md) en los modelos.

|Escalado y procesamiento| Descripción |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Estandariza las características al quitar la media y realizar un escalado según la varianza de la unidad.  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Transforma las características al escalar cada una según el mínimo y máximo de esa columna.  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Escala cada característica según su valor absoluto máximo. |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) | Escala las características según su rango cuantil. |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Reducción de dimensionalidad lineal mediante la descomposición en valores singulares de los datos a fin de proyectarlos en un espacio dimensional inferior. |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Este convertidor realiza la reducción de dimensionalidad lineal por medio de la descomposición en valores singulares truncados (SVD). Al contrario que PCA, este programa de estimación no centra los datos antes de calcular la descomposición en valores singulares, lo que significa que puede trabajar con matrices scipy.sparse de forma eficaz. |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Cada muestra (es decir, cada fila de la matriz de datos) con al menos un componente distinto de cero vuelve a escalarse independientemente de las demás muestras, de modo que su norma vectorial (l1 o l2) sea igual a uno. |

### <a name="customize-featurization"></a>Personalización de la caracterización

También hay disponibles técnicas de ingeniería de características adicionales, como la codificación y las transformaciones. 

Para habilitar esta configuración, realice lo siguiente:

+ Azure Machine Learning Studio: Habilite **Automatic featurization** (Características automáticas) en la sección **View additional configuration** (Ver configuración adicional) siguiendo [estos pasos](how-to-use-automated-ml-for-ml-models.md#customize-featurization).

+ SDK de Python: Especifique `"feauturization": 'auto' / 'off' / 'FeaturizationConfig'` en el objeto [AutoMLConfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig). Más información sobre cómo [habilitar la ingeniería de características](how-to-configure-auto-features.md). 

## <a name="ensemble-models"></a><a name="ensemble"></a> Modelos de conjunto

El aprendizaje automático automatizado admite modelos de conjunto, que están habilitados de forma predeterminada. El aprendizaje de conjunto mejora los resultados del aprendizaje automático y su rendimiento predictivo mediante la combinación de varios modelos en lugar de usar modelos únicos. Las iteraciones de conjunto aparecen como las iteraciones finales de la ejecución. El aprendizaje automático automatizado usa los métodos de conjunto de votaciones y apilamiento para combinar modelos:

* **Votación**: realiza la predicción en función de la media ponderada de las probabilidades de clase predichas (para tareas de clasificación) o de los destinos de regresión predichos (para tareas de regresión).
* **Apilamiento**: el apilamiento combina modelos heterogéneos y entrena un metamodelo basado en la salida de los modelos individuales. Los metamodelos predeterminados actuales son LogisticRegression para las tareas de clasificación y ElasticNet para las tareas de regresión y previsión.

El [algoritmo de selección de conjunto de Caruana](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) con inicialización de conjunto ordenado se utiliza para decidir qué modelos se van a utilizar en el conjunto. En un nivel alto, este algoritmo inicializa el conjunto con hasta cinco modelos con las mejores puntuaciones individuales y comprueba que estos modelos se encuentran en un umbral del 5 % de la mejor puntuación para evitar un conjunto inicial deficiente. A continuación, para cada iteración de conjunto, se agrega un nuevo modelo al conjunto existente y se calcula la puntuación resultante. Si un nuevo modelo ha mejorado la puntuación de conjunto existente, el conjunto se actualiza para incluir el nuevo modelo.

Consulte el [procedimiento](how-to-configure-auto-train.md#ensemble) para cambiar la configuración del conjunto predeterminado en el aprendizaje automático automatizado.

## <a name="guidance-on-local-vs-remote-managed-ml-compute-targets"></a><a name="local-remote"></a>Guía sobre los destinos de proceso de ML administrados de forma local o remota

La interfaz web siempre usa para el aprendizaje automático automatizado un [destino de proceso](concept-compute-target.md) remoto.  Pero cuando use el SDK de Python, elegirá un destino de proceso o bien local o remoto para el entrenamiento de ML automatizado.

* **Proceso local**: el entrenamiento se produce en el proceso del equipo portátil o máquina virtual local. 
* **Proceso remoto**: el entrenamiento se produce en los clústeres de proceso de Machine Learning.  

### <a name="choose-compute-target"></a>Selección del destino de proceso
Tenga en cuenta estos factores al elegir el destino de proceso:

 * **Elija un proceso local**: si su escenario es de exploraciones iniciales o demostraciones con datos reducidos y entrenamientos cortos (es decir, segundos o un par de minutos por cada ejecución secundaria), el entrenamiento en el equipo local puede ser la mejor opción.  No hay tiempo de instalación y los recursos de infraestructura (su equipo o máquina virtual) están disponibles directamente.
 * **Elija un clúster de proceso de ML remoto**: si va a realizar un entrenamiento con conjuntos de datos de mayor tamaño, como en los entrenamientos de producción con la creación de modelos que necesiten entrenamientos más largos, el proceso remoto proporciona un rendimiento de tiempo de extremo a extremo mucho mejor, ya que `AutoML` pondrá en paralelo los entrenamientos en los nodos del clúster. En un proceso remoto, el tiempo de inicio para la infraestructura interna agregará alrededor de 1,5 minutos por ejecución secundaria, además de minutos adicionales para la infraestructura de clústeres si las máquinas virtuales todavía no están en funcionamiento.

### <a name="pros-and-cons"></a>Ventajas y desventajas
Cuando elija entre local y remoto tenga en cuenta estas ventajas y desventajas.

|  | Ventajas (a favor)  |Desventajas (en contra)  |
|---------|---------|---------|
|**Destino de proceso local** |  <li> No hay tiempo de inicio del entorno   | <li>  Subconjunto de características<li>  No se pueden realizar ejecuciones en paralelo <li> Peor para datos de gran tamaño <li>Sin streaming de datos durante el entrenamiento <li>  No hay características basadas en DNN <li> Solo SDK de Python |
|**Clústeres de proceso de ML remotos**|  <li> Conjunto completo de características <li> Realización de ejecuciones secundarias en paralelo <li>   Compatibilidad con datos de gran tamaño<li>  Características basadas en DNN <li>  Escalabilidad dinámica del clúster de proceso a petición <li> Sin experiencia de código (interfaz de usuario web) también disponible  |  <li> Tiempo de inicio de los nodos de clúster <li> Tiempo de inicio para cada ejecución secundaria    |

### <a name="feature-availability"></a>Disponibilidad de características 

Hay más características disponibles cuando se usa el proceso remoto, tal como se muestra en la tabla siguiente. 

| Característica                                                    | Remote | Local | 
|------------------------------------------------------------|--------|-------|
| Streaming de datos (compatibilidad con datos de gran tamaño, hasta 100 GB)          | ✓      |       | 
| Características de texto y entrenamiento basados en DNN-BERT             | ✓      |       |
| Compatibilidad de GPU integrada (entrenamiento e inferencia)        | ✓      |       |
| Compatibilidad con el etiquetado y la clasificación de imágenes (versión preliminar)        | ✓      |       |
| Modelos Auto-ARIMA, Prophet y ForecastTCN para la previsión | ✓      |       | 
| Varias ejecuciones/iteraciones en paralelo                       | ✓      |       |
| Creación de modelos con la funcionalidad de interpretación en la interfaz de usuario de la experiencia web de AutoML Studio      | ✓      |       |
| Personalización de ingeniería de características en la interfaz de usuario de la experiencia web de Studio| ✓      |       |
| Ajuste de hiperparámetros de Azure ML                             | ✓      |       |
| Compatibilidad con el flujo de trabajo de canalización de Azure ML                         | ✓      |       |
| Continuación de una ejecución                                             | ✓      |       |
| Previsión                                                | ✓      | ✓     |
| Computer Vision (versión preliminar)                                  | ✓      |       |
| Creación y ejecución de experimentos en cuadernos                    | ✓      | ✓     |
| Registro y visualización de la información y las métricas del experimento en la interfaz de usuario | ✓      | ✓     |
| Límites de protección de datos                                            | ✓      | ✓     |


<a name="use-with-onnx"></a>

## <a name="automl--onnx"></a>AutoML y ONNX

Con Azure Machine Learning, puede usar Machine Learning para generar un modelo de Python y convertirlo al formato ONNX. Una vez que los modelos están en el formato ONNX, se pueden ejecutar en varias plataformas y dispositivos. Más información sobre la [aceleración de los modelos de Machine Learning con ONNX](concept-onnx.md).

Consulte cómo convertir al formato ONNX [en este ejemplo de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb). Aprenda cuáles [son los algoritmos que se admiten en ONNX](how-to-configure-auto-train.md#supported-models).

El entorno de ejecución de ONNX también es compatible con C#, por lo que puede usar el modelo creado automáticamente en sus aplicaciones de C# sin necesidad de volver a codificar o experimentar alguna de las latencias de red que presentan los puntos de conexión REST. Obtenga más detalles sobre el [uso de un modelo AutoML de ONNX en una aplicación .NET con ML.NET](./how-to-use-automl-onnx-model-dotnet.md) y la [creación de inferencias en modelos de ONNX con la API de C# del entorno de ejecución de ONNX](https://onnxruntime.ai/docs/api/csharp-api.html). 

## <a name="next-steps"></a>Pasos siguientes

Hay varios recursos que le ayudarán a ponerse en marcha con AutoML. 

### <a name="tutorials-how-tos"></a>Tutoriales y procedimientos
Los tutoriales son ejemplos de introducción de un extremo a otro de escenarios de AutoML.
+ **Para una experiencia basada en código**, consulte [Tutorial: Entrenamiento de un modelo de regresión con AutoML y Python](tutorial-auto-train-models.md).

+ **Para una experiencia sin código o con poco código**, consulte [Tutorial: Entrenamiento de un modelo de clasificación con aprendizaje automático automatizado sin código en el Estudio de Azure Machine Learning](tutorial-first-experiment-automated-ml.md).

+ **Para usar AutoML para entrenar modelos de Computer Vision**, consulte el [Tutorial: Entrenamiento de un modelo de detección de objetos (versión preliminar) con AutoML y Python](tutorial-auto-train-image-models.md).
   
Los artículos de procedimientos proporcionan información adicional sobre la funcionalidad que ofrece AutoML. Por ejemplo, 

+ Configure de los experimentos de aprendizaje automáticos
    + [Sin código en Estudio de Azure Machine Learning](how-to-use-automated-ml-for-ml-models.md). 
    + [Con el SDK para Python](how-to-configure-auto-train.md).

+  Obtenga información sobre cómo [entrenar modelos de previsión con datos de series temporales](how-to-auto-train-forecast.md).

+  Obtenga más información sobre cómo [entrenar modelos de Computer Vision con Python](how-to-auto-train-image-models.md).
   
### <a name="jupyter-notebook-samples"></a>Muestras de cuaderno de Jupyter 

Revise los ejemplos de código y los casos de uso detallados en el repositorio del [cuaderno de GitHub para obtener muestras de aprendizaje automático automatizado](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/).

### <a name="python-sdk-reference"></a>Referencia de SDK de Python

Profundizar en sus conocimientos sobre los patrones de diseño y las especificaciones de clases del SDK con la [documentación de referencia de la clase AutoML](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig). 

> [!Note]
> Las funcionalidades del aprendizaje automático automatizado también están disponibles en otras soluciones de Microsoft como,[ML.NET](/dotnet/machine-learning/automl-overview), [HDInsight](../hdinsight/spark/apache-spark-run-machine-learning-automl.md), [Power BI](/power-bi/service-machine-learning-automated) y [SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)