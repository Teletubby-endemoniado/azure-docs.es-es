---
title: Evaluación de los resultados de los experimentos de aprendizaje automático automatizado
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo ver y evaluar los gráficos y las métricas de cada una de las ejecuciones de experimentos de aprendizaje automático automatizado.
services: machine-learning
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: automl
ms.date: 10/21/2021
ms.topic: how-to
ms.custom: contperf-fy21q2, automl
ms.openlocfilehash: 3b638535784e35089050f3f7e55d85232800ef06
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131561563"
---
# <a name="evaluate-automated-machine-learning-experiment-results"></a>Evaluación de los resultados del experimento de aprendizaje automático automatizado

En este artículo, aprenderá a evaluar y comparar los modelos entrenados por el experimento de aprendizaje automático automatizado (ML automatizado). En el transcurso de un experimento de ML automatizado, se crean muchas ejecuciones y cada una de ellas crea un modelo. Para cada modelo, ML automatizado genera gráficos y métricas de evaluación que le ayudan a medir el rendimiento del modelo. 

Por ejemplo, ML automatizado genera los siguientes gráficos en función del tipo de experimento.

| clasificación| Regresión y previsión |
| ----------------------------------------------------------- | ---------------------------------------- |
| [Matriz de confusión](#confusion-matrix)                       | [Histograma de valores residuales](#residuals)        |
| [Curva de característica operativa del receptor (ROC)](#roc-curve) | [Valores predichos frente a valores verdaderos](#predicted-vs-true) |
| [Curva de precisión-coincidencia (PR)](#precision-recall-curve)      |                                          |
| [Curva de elevación](#lift-curve)                                   |                                          |
| [Curva de beneficios acumulativos](#cumulative-gains-curve)           |                                          |
| [Curva de calibración](#calibration-curve)                     |                     


## <a name="prerequisites"></a>Requisitos previos

- Suscripción a Azure. (Si no tiene una suscripción a Azure, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar).
- Un experimento Azure Machine Learning creado con cualquiera de las siguientes opciones:
  - [Estudio de Azure Machine Learning](how-to-use-automated-ml-for-ml-models.md) (no se requiere código)
  - El [SDK de Azure Machine Learning para Python](how-to-configure-auto-train.md)

## <a name="view-run-results"></a>Visualización de los resultados de las ejecuciones

Una vez completado el experimento de ML automatizado, se puede encontrar un historial de las ejecuciones mediante:
  - Un explorador con [Estudio de Azure Machine Learning](overview-what-is-machine-learning-studio.md)
  - Un cuaderno de Jupyter con el [widget Jupyter de RunDetails](/python/api/azureml-widgets/azureml.widgets.rundetails)

En el vídeo y los pasos siguientes, se muestra cómo ver el historial de ejecuciones y los gráficos y las métricas de evaluación del modelo en el estudio:

1. [Inicie sesión en el estudio](https://ml.azure.com/) y vaya al área de trabajo.
1. En el menú de la izquierda, seleccione **Experimentos**.
1. Seleccione el experimento en la lista de experimentos.
1. En la tabla de la parte inferior de la página, seleccione una ejecución de ML automatizado.
1. En la pestaña **Modelos**, seleccione una opción en **Nombre del algoritmo** correspondiente al modelo que quiere evaluar.
1. En la pestaña **Métricas**, utilice las casillas de la izquierda para ver las métricas y los gráficos.

![Pasos para ver métricas en el estudio](./media/how-to-understand-automated-ml/how-to-studio-metrics.gif)

## <a name="classification-metrics"></a>Métricas de clasificación

ML automatizado calcula las métricas de rendimiento para cada modelo de clasificación generado para el experimento. Estas métricas se basan en la implementación de scikit-learn. 

Muchas métricas de clasificación se definen para la clasificación binaria en dos clases y requieren el promedio de las clases para generar una puntuación para la clasificación de varias clases. Scikit-learn proporciona varios métodos de promedio, tres de los cuales expone ML automatizado: **macro**, **micro**, y **ponderado**.

- **Macro**: se calcula la métrica para cada clase y se toma el promedio no ponderado
- **Micro**: se calcula la métrica de forma global mediante el recuento del total de verdaderos positivos, falsos negativos y falsos positivos (independientemente de las clases).
- **Ponderado**: se calcula la métrica para cada clase y se toma el promedio ponderado en función del número de muestras por clase.

Aunque cada método de promedio tiene sus ventajas, una consideración común al seleccionar el método adecuado es el desequilibrio de clases. Si las clases tienen números de muestras diferentes, podría ser más informativo usar un promedio de macros, donde las clases minoritarias reciben la misma ponderación que las clases mayoritarias. Más información sobre [las métricas binarias frente a las métricas multiclase en ML automatizado](#binary-vs-multiclass-classification-metrics). 

En la tabla siguiente se resumen las métricas de rendimiento de modelo que el aprendizaje automático automatizado calcula para cada modelo de clasificación generado para el experimento. Para obtener más información, consulte la documentación de scikit-learn vinculada en el campo **Cálculo** de cada métrica. 

> [!NOTE]
> Vea la sección de [métricas de imagen](#metrics-for-image-models-preview) para obtener más detalles sobre las métricas de los modelos de clasificación de imágenes.

|Métrica|Descripción|Cálculo|
|--|--|---|
|AUC | AUC es el área bajo la [curva de característica operativa del receptor](#roc-curve).<br><br> **Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [0, 1]<br> <br>Entre los nombres de métricas admitidos se incluyen: <li>`AUC_macro`: es la media aritmética del parámetro AUC para cada clase.<li> `AUC_micro`: se calcula mediante el recuento del total de verdaderos positivos, falsos negativos y falsos positivos. <li> `AUC_weighted`: es la media aritmética de la puntuación para cada clase, ponderada por el número de instancias verdaderas en cada clase. <li> `AUC_binary`: es el valor de AUC al tratar una clase específica como `true` y combinar todas las demás clases como `false`.<br><br>|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.roc_auc_score.html) | 
|accuracy| La precisión es la relación de predicciones que coinciden exactamente con las etiquetas de clase. <br> <br>**Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [0, 1]|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.accuracy_score.html)|
|average_precision|La precisión promedio resume una curva de precisión-recuperación como la media ponderada de las precisiones conseguidas en cada umbral, donde el aumento de la recuperación del umbral anterior se usa como peso. <br><br> **Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [0, 1]<br> <br>Entre los nombres de métricas admitidos se incluyen:<li>`average_precision_score_macro`: es la media aritmética de la puntuación de precisión media de cada clase.<li> `average_precision_score_micro`: se calcula mediante el recuento del total de verdaderos positivos, falsos negativos y falsos positivos.<li>`average_precision_score_weighted`: es la media aritmética de la puntuación de precisión promedio para cada clase, ponderada por el número de instancias verdaderas en cada clase. <li> `average_precision_score_binary`: es el valor de la precisión media al tratar una clase específica como `true` y combinar todas las demás clases como `false`.|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.average_precision_score.html)|
balanced_accuracy|La precisión equilibrada es la media aritmética de recuperación para cada clase.<br> <br>**Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [0, 1]|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.recall_score.html)|
f1_score|La puntuación F1 es la media armónica de precisión y recuperación. Es una buena medida equilibrada de falsos positivos y falsos negativos. Sin embargo, no tiene en cuenta los verdaderos negativos. <br> <br>**Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [0, 1]<br> <br>Entre los nombres de métricas admitidos se incluyen:<li>  `f1_score_macro`: media aritmética de la puntuación F1 para cada clase. <li> `f1_score_micro`: se calcula mediante el recuento del total de verdaderos positivos, falsos negativos y falsos positivos. <li> `f1_score_weighted`: media ponderada por frecuencia de clase de la puntuación F1 para cada clase. <li> `f1_score_binary`: es el valor de F1 al tratar una clase específica como `true` y combinar todas las demás clases como `false`.|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.f1_score.html)|
log_loss|Se trata de la función de pérdida que se usa en la regresión logística (multinomial) y sus extensiones, como las redes neuronales, definida como la verosimilitud logarítmica negativa de las etiquetas verdaderas, dadas las predicciones de un clasificador probabilístico. <br><br> **Objetivo:** cuanto más cercano a 0, mejor <br> **Intervalo:** [0, inf)|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.log_loss.html)|
norm_macro_recall| La coincidencia de macro normalizada es la coincidencia de macro promediada y normalizada para que el rendimiento aleatorio tenga una puntuación de 0 y el rendimiento perfecto una puntuación de 1. <br> <br>**Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [0, 1] |`(recall_score_macro - R)`&nbsp;/&nbsp;`(1 - R)` <br><br>donde, `R` es el valor esperado de `recall_score_macro` para las predicciones aleatorias.<br><br>`R = 0.5`&nbsp;para&nbsp; clasificación&nbsp;binaria. <br>`R = (1 / C)` para problemas de clasificación de clase C.|
matthews_correlation | El coeficiente de correlación de Matthews es una medida equilibrada de precisión, que se puede usar incluso si una clase tiene muchos más ejemplos que otra. Un coeficiente de 1 indica una predicción perfecta, 0 predicción aleatoria y -1 predicción inversa.<br><br> **Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [-1, 1]|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.matthews_corrcoef.html)|
Precisión|La precisión es la capacidad de un modelo para evitar que las etiquetas negativas se etiqueten como positivas. <br><br> **Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [0, 1]<br> <br>Entre los nombres de métricas admitidos se incluyen: <li> `precision_score_macro`: es la media aritmética de la precisión para cada clase. <li> `precision_score_micro`: se calcula de forma global mediante el recuento del total de verdaderos positivos y falsos positivos. <li> `precision_score_weighted`: es la media aritmética de la precisión para cada clase, ponderada por el número de instancias verdaderas en cada clase. <li> `precision_score_binary`: es el valor de la precisión al tratar una clase específica como `true` y combinar todas las demás clases como `false`.|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.precision_score.html)|
Coincidencia| La coincidencia es la capacidad de un modelo para detectar todas las muestras positivas. <br><br> **Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [0, 1]<br> <br>Entre los nombres de métricas admitidos se incluyen: <li>`recall_score_macro`: es la media aritmética de la coincidencia para cada clase. <li> `recall_score_micro`: se calcula de forma global mediante el recuento del total de verdaderos positivos, falsos negativos y falsos positivos.<li> `recall_score_weighted`: es la media aritmética de la coincidencia para cada clase, ponderada por el número de instancias verdaderas en cada clase. <li> `recall_score_binary`: es el valor de la coincidencia al tratar una clase específica como `true` y combinar todas las demás clases como `false`.|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.recall_score.html)|
weighted_accuracy|La precisión ponderada es la precisión en la que cada muestra está ponderada por el número total de muestras que pertenecen a la misma clase. <br><br>**Objetivo:** cuanto más cercano a 1, mejor <br>**Intervalo:** [0, 1]|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.accuracy_score.html)|

### <a name="binary-vs-multiclass-classification-metrics"></a>Métricas de clasificación binaria y de varias clases

El aprendizaje automático automatizado detecta automáticamente si los datos son binarios y también permite a los usuarios activar métricas de clasificación binaria incluso si los datos son multiclase mediante la especificación de una clase `true`. Las métricas de clasificación multiclase se notifican independientemente de si un conjunto de datos tiene dos o más clases. Las métricas de clasificación binaria solo se notifican cuando los datos son binarios o los usuarios activan la opción. 

> [!Note]
> Cuando se detecta una tarea de clasificación binaria, se usa `numpy.unique` para buscar el conjunto de etiquetas y la etiqueta posterior se usará como clase `true`. Dado que hay un procedimiento de ordenación en `numpy.unique`, la elección de la clase `true` será estable.

Tenga en cuenta que las métricas de clasificación multiclase están pensadas para la clasificación multiclase. Cuando estas métricas se aplican a un conjunto de datos binario, no tratarán a ninguna clase como clase `true`, como cabría esperar. Las métricas que se han diseñado claramente para multiclase tienen como sufijo `micro`, `macro` o `weighted`. Entre los ejemplos se incluyen `average_precision_score`, `f1_score`, `precision_score`, `recall_score` y `AUC`. Por ejemplo, en lugar de calcular la recuperación como `tp / (tp + fn)`, la recuperación media multiclase (`micro`, `macro` o `weighted`) promedia ambas clases de un conjunto de datos de clasificación binaria. Esto es equivalente a calcular la recuperación de la clase `true` y de la clase `false` por separado y, a continuación, tomar la media de ambas.

Además, aunque se admite la detección automática de clasificación binaria, se recomienda especificar siempre la clase `true` manualmente para asegurarse de que las métricas de clasificación binaria se calculan para la clase correcta.

Para activar las métricas de los conjuntos de datos de clasificación binaria cuando el propio conjunto de datos es multiclase, los usuarios solo tienen que especificar la clase que se va a tratar como `true` y se calcularán estas métricas.

## <a name="confusion-matrix"></a>Matriz de confusión

Las matrices de confusión proporcionan un aspecto visual de cómo un modelo de aprendizaje automático comete errores sistemáticos en sus predicciones para los modelos de clasificación. La palabra "confusión" en el nombre proviene de un modelo "confuso" o de un mal etiquetado de muestras. Una celda de la fila `i` y la columna `j` de una matriz de confusión contiene el número de muestras del conjunto de datos de evaluación que pertenecen a la clase `C_i` y que el modelo clasificó como clase `C_j`.

En el estudio, una celda más oscura indica un número mayor de muestras. Al seleccionar **Normalizada** en la lista desplegable se normalizará cada fila de la matriz para mostrar el porcentaje de la clase `C_i` que se predice que sea la clase `C_j`. La ventaja de la vista **Fila** predeterminada es que puede ver si el desequilibrio en la distribución de clases reales hizo que el modelo clasificase erróneamente muestras de la clase minoritaria, un problema común en conjuntos de datos desequilibrados.

La matriz de confusión de un modelo adecuado tendrá la mayoría de las muestras a lo largo de la diagonal.

### <a name="confusion-matrix-for-a-good-model"></a>Matriz de confusión para un buen modelo 
![Matriz de confusión para un buen modelo ](./media/how-to-understand-automated-ml/chart-confusion-matrix-good.png)

### <a name="confusion-matrix-for-a-bad-model"></a>Matriz de confusión para un mal modelo
![Matriz de confusión para un mal modelo](./media/how-to-understand-automated-ml/chart-confusion-matrix-bad.png)

## <a name="roc-curve"></a>Curva ROC

La curva de característica operativa del receptor (ROC) traza la relación entre la tasa de verdaderos positivos (TPR) y la tasa de falsos positivos (FPR) a medida que cambia el umbral de decisión. La curva ROC puede ser menos informativa al entrenar modelos con conjuntos de datos con un gran desequilibrio de clases, ya que la mayoría de las clases pueden ahogar contribuciones de las clases minoritarias.

El área bajo la curva (AUC) se puede interpretar como la proporción de las muestras clasificadas correctamente. Más concretamente, AUC es la probabilidad de que el clasificador clasifique una muestra positiva aleatoriamente seleccionada más arriba que una muestra negativa elegida aleatoriamente. La forma de la curva proporciona una intuición de la relación entre TPR y FPR en función del umbral de clasificación o límite de decisión.

Una curva que se aproxima a la esquina superior izquierda del gráfico se está aproximando a un valor de TPR del 100 % y a un valor de FPR del 0 %, el mejor modelo posible. Un modelo aleatorio produciría una curva ROC a lo largo de la línea `y = x` desde la esquina inferior izquierda hasta la parte superior derecha. Un modelo peor que el aleatorio tendría una curva ROC que cae por debajo de la línea `y = x`.
> [!TIP]
> En el caso de los experimentos de clasificación, cada uno de los gráficos de líneas producidos para los modelos de ML automatizado se puede utilizar para evaluar el modelo por clase o promediar todas las clases. Puede cambiar entre estas diferentes vistas haciendo clic en las etiquetas de clase de la leyenda que se encuentra a la derecha del gráfico.

### <a name="roc-curve-for-a-good-model"></a>Curva ROC para un buen modelo
![Curva ROC para un buen modelo](./media/how-to-understand-automated-ml/chart-roc-curve-good.png)

### <a name="roc-curve-for-a-bad-model"></a>Curva ROC para un mal modelo
![Curva ROC para un mal modelo](./media/how-to-understand-automated-ml/chart-roc-curve-bad.png)

## <a name="precision-recall-curve"></a>Curva de precisión-coincidencia

La curva de precisión-coincidencia traza la relación entre la precisión y la coincidencia a medida que cambia el umbral de decisión. La coincidencia es la capacidad de un modelo para detectar todas las muestras positivas y la precisión es la capacidad de un modelo de evitar el etiquetado de muestras negativas como positivas. Algunos problemas empresariales pueden requerir una coincidencia más alta y una precisión mayor en función de la importancia relativa de evitar falsos negativos frente a falsos positivos.
> [!TIP]
> En el caso de los experimentos de clasificación, cada uno de los gráficos de líneas producidos para los modelos de ML automatizado se puede utilizar para evaluar el modelo por clase o promediar todas las clases. Puede cambiar entre estas diferentes vistas haciendo clic en las etiquetas de clase de la leyenda que se encuentra a la derecha del gráfico.
### <a name="precision-recall-curve-for-a-good-model"></a>Curva de precisión-coincidencia para un buen modelo
![Curva de precisión-coincidencia para un buen modelo](./media/how-to-understand-automated-ml/chart-precision-recall-curve-good.png)

### <a name="precision-recall-curve-for-a-bad-model"></a>Curva de precisión-coincidencia para un mal modelo
![Curva de precisión-coincidencia para un mal modelo](./media/how-to-understand-automated-ml/chart-precision-recall-curve-bad.png)

## <a name="cumulative-gains-curve"></a>Curva de beneficios acumulativos

La curva de ganancias acumulativas traza el porcentaje de muestras positivas correctamente clasificadas como una función del porcentaje de muestras consideradas cuando consideramos muestras en el orden de probabilidad predicha.

Para calcular el beneficio, ordene primero todas las muestras, desde la probabilidad más alta hasta la más baja predichas por el modelo. Después, tome `x%` de las predicciones de mayor confianza. Divida el número de muestras positivas detectadas en `x%` por el número total de muestras positivas para obtener el beneficio. El beneficio acumulativo es el porcentaje de muestras positivas que se detectan al considerar algún porcentaje de los datos que es más probable que pertenezcan a la clase positiva.

Un modelo perfecto clasificará todas las muestras positivas por encima de todas las muestras negativas, lo que proporciona una curva de beneficios acumulativos formada por dos segmentos rectos. La primera es una línea con pendiente `1 / x` de `(0, 0)` a `(x, 1)` donde `x` es la fracción de ejemplos que pertenecen a la clase positiva (`1 / num_classes` si las clases están equilibradas). La segunda es una línea horizontal de `(x, 1)` a `(1, 1)`. En el primer segmento, todas las muestras positivas se clasifican correctamente y la ganancia acumulativa va a `100%` dentro del primer `x%` de muestras que se consideran.

El modelo aleatorio de línea base tendrá una curva de beneficios acumulativos conforme a `y = x` donde para el `x%` de muestras consideradas solo se detectaron un `x%` aproximadamente de las muestras positivas totales. Un modelo perfecto para un conjunto de datos equilibrado tendrá una curva promedio micro y una línea promedio macro que tiene pendiente `num_classes` hasta que el beneficio acumulativo sea del 100 % y luego horizontal hasta que el porcentaje de datos sea 100.
> [!TIP]
> En el caso de los experimentos de clasificación, cada uno de los gráficos de líneas producidos para los modelos de ML automatizado se puede utilizar para evaluar el modelo por clase o promediar todas las clases. Puede cambiar entre estas diferentes vistas haciendo clic en las etiquetas de clase de la leyenda que se encuentra a la derecha del gráfico.
### <a name="cumulative-gains-curve-for-a-good-model"></a>Curva de beneficios acumulativos para un buen modelo
![Curva de beneficios acumulativos para un buen modelo](./media/how-to-understand-automated-ml/chart-cumulative-gains-curve-good.png)

### <a name="cumulative-gains-curve-for-a-bad-model"></a>Curva de beneficios acumulativos para un mal modelo
![Curva de beneficios acumulativos para un mal modelo](./media/how-to-understand-automated-ml/chart-cumulative-gains-curve-bad.png)

## <a name="lift-curve"></a>Curva de elevación

Una curva de elevación muestra cuánto mejor se comporta un modelo en comparación con un modelo aleatorio. La elevación se define como la relación entre el beneficio acumulativo y el beneficio acumulativo de un modelo aleatorio (que siempre debe ser `1`).

Este rendimiento relativo tiene en cuenta el hecho de que la clasificación se hace más difícil a medida que el número de clases aumenta. (Un modelo aleatorio predice incorrectamente más muestras de un conjunto de datos con diez clases, en comparación con un conjunto de datos con dos clases).

La curva de elevación de línea base es la línea `y = 1` en la que el rendimiento del modelo es coherente con el de un modelo aleatorio. En general, la curva de elevación para un buen modelo será más alta en ese gráfico y más alejada del eje X, lo que muestra que cuando el modelo tiene más confianza en sus predicciones, se desempeña mucho mejor que las suposiciones aleatorias.

> [!TIP]
> En el caso de los experimentos de clasificación, cada uno de los gráficos de líneas producidos para los modelos de ML automatizado se puede utilizar para evaluar el modelo por clase o promediar todas las clases. Puede cambiar entre estas diferentes vistas haciendo clic en las etiquetas de clase de la leyenda que se encuentra a la derecha del gráfico.
### <a name="lift-curve-for-a-good-model"></a>Curva de elevación para un buen modelo
![Curva de elevación para un buen modelo](./media/how-to-understand-automated-ml/chart-lift-curve-good.png)
 
### <a name="lift-curve-for-a-bad-model"></a>Curva de elevación para un mal modelo
![Curva de elevación para un mal modelo](./media/how-to-understand-automated-ml/chart-lift-curve-bad.png)

## <a name="calibration-curve"></a>Curva de calibración

La curva de calibración traza la confianza de un modelo en sus predicciones con respecto a la proporción de muestras positivas en cada nivel de confianza. Un modelo bien calibrado clasificará correctamente el 100 % de las predicciones a las que asigna un 100 % de confianza, el 50 % de las predicciones a las que asigna un 50 % de confianza, el 20 % de las predicciones a las que asigna un 20 % de confianza, etc. Un modelo perfectamente calibrado tendrá una curva de calibración conforme a la línea `y = x` donde el modelo predice perfectamente la probabilidad de que las muestras pertenezcan a cada clase.

Un modelo con un exceso de confianza predecirá en exceso las probabilidades cercanas a cero y uno, y muy ocasionalmente se mostrará incierto sobre la clase de cada muestra y la curva de calibración tendrá un aspecto similar a una "S" hacia atrás. Un modelo con poca confianza asignará una probabilidad más baja en promedio a la clase que predice y la curva de calibración asociada tendrá un aspecto similar a una "S". La curva de calibración no representa la capacidad de un modelo para clasificar correctamente, sino su capacidad para asignar correctamente la confianza a sus predicciones. Un mal modelo puede seguir teniendo una buena curva de calibración si el modelo asigna correctamente baja confianza y alta incertidumbre.

> [!NOTE]
> La curva de calibración es sensible al número de muestras, por lo que un conjunto de validación pequeño puede generar resultados ruidosos que pueden ser difíciles de interpretar. Esto no significa necesariamente que el modelo no esté bien calibrado.

### <a name="calibration-curve-for-a-good-model"></a>Curva de calibración para un buen modelo
![Curva de calibración para un buen modelo](./media/how-to-understand-automated-ml/chart-calibration-curve-good.png)

### <a name="calibration-curve-for-a-bad-model"></a>Curva de calibración para un mal modelo
![Curva de calibración para un mal modelo](./media/how-to-understand-automated-ml/chart-calibration-curve-bad.png)

## <a name="regressionforecasting-metrics"></a>Métricas de regresión y previsión

ML automatizado calcula las mismas métricas de rendimiento para cada modelo generado, independientemente de si se trata de un experimento de regresión o previsión. Estas métricas también se someten a la normalización para habilitar la comparación entre los modelos entrenados en los datos con diferentes intervalos. Para obtener más información, vea la sección [Normalización de métricas](#metric-normalization).  

En la tabla siguiente se resumen las métricas de rendimiento del modelo generadas para los experimentos de regresión y previsión. Al igual que las métricas de clasificación, estas métricas también se basan en las implementaciones de scikit-learn. En el campo **Cálculo** se vincula la documentación de scikit-learn para cada métrica.

|Métrica|Descripción|Cálculo|
--|--|--|
explained_variance|La varianza explicada calcula la medida en que un modelo da cuenta de la variación en la variable de destino. Es el porcentaje de reducción de la varianza de los datos originales con respecto a la varianza de los errores. Cuando la media de los errores es 0, es igual que el coeficiente de determinación (consulte r2_score a continuación). <br> <br> **Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** (-inf, 1]|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.explained_variance_score.html)|
mean_absolute_error|El error absoluto medio es el valor esperado del valor absoluto de la diferencia entre el destino y la predicción.<br><br> **Objetivo:** cuanto más cercano a 0, mejor <br> **Intervalo:** [0, inf) <br><br> Tipos: <br>`mean_absolute_error` <br>  `normalized_mean_absolute_error`: es mean_absolute_error dividido por el intervalo de los datos. | [Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.mean_absolute_error.html)|
mean_absolute_percentage_error|El error de porcentaje absoluto medio (MAPE) es una medida de la diferencia promedio entre un valor predicho y el valor real.<br><br> **Objetivo:** cuanto más cercano a 0, mejor <br> **Intervalo:** [0, inf) ||
median_absolute_error|El error medio absoluto es la media de todas las diferencias absolutas entre el destino y la predicción. Esta pérdida es estable para los valores atípicos.<br><br> **Objetivo:** cuanto más cercano a 0, mejor <br> **Intervalo:** [0, inf)<br><br>Tipos: <br> `median_absolute_error`<br> `normalized_median_absolute_error`: es median_absolute_error dividido por el intervalo de los datos. |[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.median_absolute_error.html)|
r2_score|R<sup>2</sup> (el coeficiente de determinación) mide la reducción proporcional del error cuadrado medio (MSE) con respecto a la varianza total de los datos observados. <br> <br> **Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [-1, 1]<br><br>Nota: R<sup>2</sup> a menudo tiene el intervalo [-inf, 1]. El MSE puede ser mayor que la varianza observada, por lo que R<sup>2</sup> puede tener valores negativos arbitrariamente grandes, en función de los datos y las predicciones del modelo. Los clips de ML automatizado informaron puntuaciones de R<sup>2</sup> de -1, por lo que un valor de -1 para R<sup>2</sup> probablemente significa que la puntuación real de R<sup>2</sup> es menor que -1. Tenga en cuenta el resto de valores de métricas y las propiedades de los datos al interpretar una puntuación negativa de R<sup>2</sup>.|[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.r2_score.html)|
root_mean_squared_error |La raíz del error cuadrático medio (RMSE) es la raíz cuadrada de la diferencia cuadrática esperada entre el destino y la predicción. Para un estimador no sesgado, RMSE es igual a la desviación estándar.<br> <br> **Objetivo:** cuanto más cercano a 0, mejor <br> **Intervalo:** [0, inf)<br><br>Tipos:<br> `root_mean_squared_error` <br> `normalized_root_mean_squared_error`: es root_mean_squared_error dividido por el intervalo de los datos. |[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.mean_squared_error.html)|
root_mean_squared_log_error|La raíz del error logarítmico cuadrático medio es la raíz cuadrada del error logarítmico cuadrático esperado.<br><br>**Objetivo:** cuanto más cercano a 0, mejor <br> **Intervalo:** [0, inf) <br> <br>Tipos: <br>`root_mean_squared_log_error` <br> `normalized_root_mean_squared_log_error`: es root_mean_squared_log_error dividido por el intervalo de los datos.  |[Cálculo](https://scikit-learn.org/0.22/modules/generated/sklearn.metrics.mean_squared_log_error.html)|
spearman_correlation| La correlación de Spearman es una medida no paramétrica de la monotonicidad de la relación entre dos conjuntos de datos. A diferencia de la correlación de Pearson, la correlación de Spearman no asume que los conjuntos de datos se distribuyen normalmente. Como sucede con otros coeficientes de correlación, Spearman varía entre -1 y + 1, y 0 implica que no hay ninguna correlación. Las correlaciones de -1 o 1 implican una relación monotónica exacta. <br><br> Spearman es una métrica de correlación de orden de clasificación, lo que significa que los cambios en los valores predichos o reales no cambiarán el resultado de Spearman si no cambian el orden de clasificación de los valores predichos o reales.<br> <br> **Objetivo:** cuanto más cercano a 1, mejor <br> **Intervalo:** [-1, 1]|[Cálculo](https://docs.scipy.org/doc/scipy-1.5.2/reference/generated/scipy.stats.spearmanr.html)|

### <a name="metric-normalization"></a>Normalización de métricas

ML automatizado normaliza las métricas de regresión y previsión, lo que permite realizar una comparación entre los modelos entrenados en los datos con diferentes intervalos. Un modelo entrenado en datos con un intervalo mayor tiene un error mayor que el mismo modelo entrenado en datos con un intervalo más pequeño, a menos que ese error esté normalizado.

Aunque no hay ningún método estándar para normalizar las métricas de error, ML automatizado realiza el enfoque común de dividir el error por el intervalo de los datos: `normalized_error = error / (y_max - y_min)`

>[!Note]
>El rango de datos no se guarda con el modelo. Si hace la inferencia con el mismo modelo en un conjunto de prueba de datos de exclusión, `y_min` y `y_max` pueden cambiar según los datos de prueba y es posible que las métricas normalizadas no se usen directamente para comparar el rendimiento del modelo en los conjuntos de entrenamiento y prueba. Puede pasar el valor de `y_min` y `y_max` del conjunto de entrenamiento para que la comparación sea equitativa.

Al evaluar un modelo de previsión en datos de series temporales, ML automatizado realiza pasos adicionales para asegurar que la normalización ocurra por identificador de serie temporal (intervalo de agregación), porque cada serie temporal probablemente tenga una distribución diferente de valores de destino.
## <a name="residuals"></a>Valores residuales

El gráfico de valores residuales es un histograma de los errores de predicción (residuales) generados para los experimentos de regresión y previsión. Los valores residuales se calculan como `y_predicted - y_true` para todas las muestras y luego se muestran como un histograma para mostrar el sesgo del modelo.

En este ejemplo, observe que ambos modelos están ligeramente sesgados para predecir menos que el valor real. Esto no es infrecuente para un conjunto de datos con una distribución sesgada de los objetivos reales, pero indica un peor rendimiento del modelo. Un buen modelo tendrá una distribución de valores residuales que alcanza el máximo en cero con pocos valores residuales en los extremos. Un modelo peor tendrá una distribución de valores residuales dispersos con menos muestras alrededor de cero.

### <a name="residuals-chart-for-a-good-model"></a>Gráfico de valores residuales para un buen modelo
![Gráfico de valores residuales para un buen modelo](./media/how-to-understand-automated-ml/chart-residuals-good.png)

### <a name="residuals-chart-for-a-bad-model"></a>Gráfico de valores residuales para un mal modelo
![Gráfico de valores residuales para un mal modelo](./media/how-to-understand-automated-ml/chart-residuals-bad.png)

## <a name="predicted-vs-true"></a>Valores predichos frente a valores verdaderos

Para el experimento de regresión y previsión, el gráfico de predicho frente a verdadero traza la relación entre la característica de destino (valores verdaderos/reales) y las predicciones del modelo. Los valores verdaderos se discretizan a lo largo del eje X y para cada rango, el valor medio predicho se traza con barras de error. Esto le permite ver si un modelo está sesgado hacia la predicción de ciertos valores. La línea muestra la predicción promedio y el área sombreada indica la variación de las predicciones alrededor de esa media.

A menudo, el valor verdadero más común tendrá las predicciones más precisas con la varianza más baja. La distancia de la línea de tendencia de la línea `y = x` ideal donde hay pocos valores verdaderos es una buena medida del rendimiento del modelo en valores atípicos. Puede usar el histograma situado en la parte inferior del gráfico para razonar sobre la distribución de datos real. Incluir más muestras de datos, donde la distribución es dispersa, puede mejorar el rendimiento del modelo en datos no visibles.

En este ejemplo, observe que el mejor modelo tiene una línea de valores predichos frente a valores verdaderos que está más próxima a la línea `y = x` ideal.

### <a name="predicted-vs-true-chart-for-a-good-model"></a>Gráfico de valores predichos frente a valores verdaderos para un buen modelo
![Gráfico de valores predichos frente a valores verdaderos para un buen modelo](./media/how-to-understand-automated-ml/chart-predicted-true-good.png)

### <a name="predicted-vs-true-chart-for-a-bad-model"></a>Gráfico de valores predichos frente a valores verdaderos para un mal modelo
![Gráfico de valores predichos frente a valores verdaderos para un mal modelo](./media/how-to-understand-automated-ml/chart-predicted-true-bad.png)

## <a name="metrics-for-image-models-preview"></a>Métricas de modelos de imagen (versión preliminar)

ML automatizado usa las imágenes del conjunto de datos de validación para evaluar el rendimiento del modelo. El rendimiento del modelo se mide en un **nivel de época** para comprender cómo progresa el entrenamiento. Una época concluye cuando un conjunto de datos completo ha pasado hacia delante y hacia atrás a través de la red neuronal exactamente una vez. 

### <a name="image-classification-metrics"></a>Métricas de clasificación de imágenes

La métrica principal para la evaluación es **accuracy** en los modelos de clasificación binarios y multiclase, e **IoU** ([Intersection over Union](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.jaccard_score.html#sklearn.metrics.jaccard_score)) en los modelos de clasificación multietiqueta.
Las métricas de clasificación de los modelos de clasificación de imágenes son las mismas que las definidas en la sección de [métricas de clasificación](#classification-metrics). Los valores de pérdida asociados a una época también se registran, lo que puede ayudar a supervisar cómo progresa el entrenamiento y a determinar si el modelo está sobreajustado o subajustado.

Cada predicción de un modelo de clasificación está asociada a una puntuación de confianza que indica el nivel de confianza con el que se ha realizado la predicción. Los modelos de clasificación de imágenes multietiqueta se evalúan de manera predeterminada con un umbral de puntuación de 0,5, lo que significa que solo las predicciones con al menos este nivel de confianza se consideran como una predicción positiva en la clase asociada. La clasificación multiclase no usa un umbral de puntuación, sino que la clase con la puntuación de confianza máxima se considera la predicción. 

#### <a name="epoch-level-metrics-for-image-classification"></a>Métricas de nivel de época para la clasificación de imágenes
A diferencia de las métricas de clasificación para conjuntos de datos tabulares, los modelos de clasificación de imágenes registran todas las métricas de clasificación en un nivel de época, como se muestra a continuación.

![Gráficos de nivel de época para la clasificación de imágenes](./media/how-to-understand-automated-ml/image-classification-accuracy.png)

#### <a name="summary-metrics-for-image-classification"></a>Métricas de resumen para la clasificación de imágenes

Además de las métricas escalares que se registran en el nivel de época, el modelo de clasificación de imágenes también registra métricas de resumen como [matriz de confusión](#confusion-matrix), [gráficos de clasificación](#roc-curve) que incluyen curva ROC, curva de precisión-recuperación e informe de clasificación para el modelo de la mejor época en la que se obtiene la puntuación de métrica principal (accuracy) más alta.

El informe de clasificación proporciona los valores de nivel de clase de métricas como precision, recall, f1-score, support, auc y average_precision, con varios niveles de promedio: micro, macro y ponderado, como se muestra a continuación.
Vea las definiciones de las métricas de la sección de [métricas de clasificación](#classification-metrics).

![Informe de clasificación para la clasificación de imágenes](./media/how-to-understand-automated-ml/image-classification-report.png)

### <a name="object-detection-and-instance-segmentation-metrics"></a>Métricas de detección de objetos y segmentación de instancias

Cada predicción de un modelo de detección de objetos de imagen o segmentación de instancias está asociada a una puntuación de confianza.
Las predicciones con una puntuación de confianza mayor que el umbral de puntuación se devuelven como predicciones y se usan en el cálculo de métricas, cuyo valor predeterminado es específico del modelo y se puede consultar desde la página de [optimización de hiperparámetros](how-to-auto-train-image-models.md#model-specific-hyperparameters) (hiperparámetro `box_score_threshold`).

El cálculo de métricas de un modelo de segmentación de instancias y detección de objetos de imagen se basa en una medida de superposición definida por una métrica denominada **IoU** ([Intersection over Union](https://en.wikipedia.org/wiki/Jaccard_index)), que se calcula dividiendo el área de superposición entre la verdad terreno y las predicciones por el área de unión de la verdad terreno y las predicciones. La IoU calculada a partir de cada predicción se compara con un **umbral de superposición** denominado umbral de IoU que determina cuánto debe superponerse una predicción con la verdad terreno anotada de un usuario para que se considere como una predicción positiva. Si la IoU calculada a partir de la predicción es menor que el umbral de superposición, la predicción no se consideraría como una predicción positiva en la clase asociada.

La métrica principal para la evaluación de modelos de detección de objetos de imagen y segmentación de instancias es la **precisión media (mAP)**. mAP es el valor medio de la precisión media (AP) en todas las clases. Los modelos de detección de objetos de ML automatizado admiten el cálculo de mAP con los dos métodos populares siguientes.

**Métricas de Pascal VOC**: 

mAP de [Pascal VOC](http://host.robots.ox.ac.uk/pascal/VOC/voc2012/htmldoc/devkit_doc.html#SECTION00044000000000000000) es la forma predeterminada de cálculo de mAP para modelos de segmentación de instancias o detección de objetos. El método mAP de estilo Pascal VOC calcula el área bajo una versión de la curva de precisión-recuperación. La primera p(ri), que es la precisión en la recuperación i, se calcula para todos los valores de recuperación únicos. p(ri) luego se reemplaza por la precisión máxima obtenida de cualquier recuperación r' >= rᵢ. El valor de precisión disminuye de forma continuada en esta versión de la curva. La métrica mAP de Pascal VOC se evalúa de manera predeterminada con un umbral de IoU de 0,5. En este [blog](https://jonathan-hui.medium.com/map-mean-average-precision-for-object-detection-45c121a31173) hay una explicación detallada de este concepto.


**Métricas de COCO**: 

El [método de evaluación COCO](https://cocodataset.org/#detection-eval) usa un método interpolado de 101 puntos para el cálculo de AP junto con un promedio de más de diez umbrales de IoU. AP@[.5:.95] corresponde al promedio de AP para IoU de 0,5 a 0,95 con un tamaño de paso de 0,05. ML automatizado registra las doce métricas definidas por el método COCO, lo que incluye AP y AR (recuperación media) en varias escalas en los registros de aplicación, mientras que la interfaz de usuario de métricas muestra solo la mAP en un umbral de IoU de 0,5. 

> [!TIP]
> La evaluación del modelo de detección de objetos de imagen puede usar métricas de COCO si el hiperparámetro `validation_metric_type` está establecido en "coco", como se explica en la sección de [optimización de hiperparámetros](how-to-auto-train-image-models.md#task-specific-hyperparameters).

#### <a name="epoch-level-metrics-for-object-detection-and-instance-segmentation"></a>Métricas de nivel de época para la detección de objetos y la segmentación de instancias
Los valores de mAP, precisión y recuperación se registran en un nivel de época para los modelos de segmentación de instancias o detección de objetos de imagen. Las métricas mAP, precision y recall también se registran en un nivel de clase con el nombre "per_label_metrics". "per_label_metrics" debe verse como una tabla. 

> [!NOTE]
> Las métricas de nivel de época precision, recall y per_label_metrics no están disponibles cuando se usa el método "coco".

![Gráficos de nivel de época para la detección de objetos](./media/how-to-understand-automated-ml/image-object-detection-map.png)

## <a name="model-explanations-and-feature-importances"></a>Explicaciones del modelo e importancias de las características

Si bien las métricas y los gráficos de evaluación del modelo son buenos para medir la calidad general de un modelo, inspeccionar qué características del conjunto de datos usó para hacer sus predicciones es esencial cuando se practica la IA responsable. Este es el motivo por el que ML automatizado proporciona un panel de explicaciones del modelo para medir y notificar las contribuciones relativas de las características del conjunto de datos. Consulte la manera de [ver el panel de explicaciones en el Estudio de Azure Machine Learning](how-to-use-automated-ml-for-ml-models.md#model-explanations-preview).

Para obtener una primera experiencia de código, consulte la manera de configurar explicaciones del [modelo para experimentos de ML automatizado con el SDK de Python de Azure Machine Learning](how-to-machine-learning-interpretability-automl.md).

> [!NOTE]
> La interpretabilidad, la mejor explicación del modelo, no está disponible para los experimentos de previsión de ML automatizados que recomiendan los algoritmos siguientes como el mejor modelo: 
> * TCNForecaster
> * AutoArima
> * ExponentialSmoothing
> * Prophet
> * Media 
> * Naive
> * Media estacional 
> * Naive estacional

## <a name="next-steps"></a>Pasos siguientes
* Pruebe los [cuadernos de ejemplo de la explicación del modelo de aprendizaje automático automatizado](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).
* En el caso de preguntas específicas sobre ML automatizado, puede escribir a askautomatedml@microsoft.com.
