---
title: Selección de un algoritmo de Machine Learning
titleSuffix: Azure Machine Learning
description: Cómo seleccionar Azure Machine Learning para el aprendizaje supervisado y sin supervisar en experimentos de agrupación en clústeres, clasificación o regresión.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: FrancescaLazzeri
ms.author: lazzeri
ms.reviewer: cgronlun
ms.date: 10/21/2021
ms.openlocfilehash: cae79937f1faa36488820d6266484fb43330e821
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131560803"
---
# <a name="how-to-select-algorithms-for-azure-machine-learning"></a>Selección de algoritmos para Azure Machine Learning

"¿Qué algoritmo de Machine Learning debo usar?" es una pregunta habitual El algoritmo que se selecciona depende principalmente de dos aspectos diferentes del escenario de ciencia de datos:

 - **¿Qué quiere hacer con los datos?** En concreto, ¿cuál es la cuestión empresarial a la que desea dar respuesta aprendiendo de sus datos pasados?

 - **¿Cuáles son los requisitos de su escenario de ciencia de datos?** En concreto, ¿cuál es la precisión, el tiempo de entrenamiento, la linealidad, el número de parámetros y el número de características que admite la solución?

 ![Consideraciones a la hora de elegir algoritmos: ¿Qué desea saber? ¿Cuáles son los requisitos del escenario?](./media/how-to-select-algorithms/how-to-select-algorithms.png)

## <a name="business-scenarios-and-the-machine-learning-algorithm-cheat-sheet"></a>Escenarios empresariales y la hoja de características clave de los algoritmos de Machine Learning

La [hoja de características clave de Azure Machine Learning](./algorithm-cheat-sheet.md?WT.mc_id=docs-article-lazzeri) sirve de ayuda en la primera consideración: **¿Qué quiere hacer con los datos?** En la hoja de características clave de los algoritmos de Machine Learning, busque la tarea que desea realizar y, después, busque un algoritmo del [diseñador de Azure Machine Learning](./concept-designer.md?WT.mc_id=docs-article-lazzeri) para la solución de análisis predictivo. 

El diseñador de Machine Learning proporciona una completa gama de algoritmos, como [Bosque de decisión multiclase](./algorithm-module-reference/multiclass-decision-forest.md?WT.mc_id=docs-article-lazzeri), [Sistemas de recomendación](./algorithm-module-reference/evaluate-recommender.md?WT.mc_id=docs-article-lazzeri), [Regresión de red neuronal](./algorithm-module-reference/neural-network-regression.md?WT.mc_id=docs-article-lazzeri), [Red neuronal multiclase](./algorithm-module-reference/multiclass-neural-network.md?WT.mc_id=docs-article-lazzeri) y [Agrupación en clústeres K-Means](./algorithm-module-reference/k-means-clustering.md?WT.mc_id=docs-article-lazzeri). Cada algoritmo está diseñado para solucionar un tipo distinto de problema de Machine Learning. Vea la [referencia de algoritmos y componentes del diseñador de Azure Machine Learning](./component-reference/component-reference.md?WT.mc_id=docs-article-lazzeri) para obtener una lista completa, junto con documentación sobre cómo funciona cada algoritmo y cómo ajustar los parámetros para optimizar el algoritmo.

> [!NOTE]
> Para descargar la hoja de características clave de los algoritmos de Machine Learning, vaya a [Hoja de características clave de los algoritmos de Machine Learning](./algorithm-cheat-sheet.md?WT.mc_id=docs-article-lazzeri).
> 
> 

Junto con la guía de la hoja de características clave de los algoritmos de Machine Learning, tenga en cuenta otros requisitos al elegir un algoritmo de Machine Learning para su solución. A continuación se indican otros factores que deben tenerse en cuenta, como la precisión, el tiempo de entrenamiento, la linealidad, el número de parámetros y el número de características.

## <a name="comparison-of-machine-learning-algorithms"></a>Comparación de algoritmos de aprendizaje automático

Algunos algoritmos de aprendizaje hacen determinadas suposiciones sobre la estructura de los datos o los resultados deseados. Si encuentra uno que se ajuste a sus necesidades, este puede ofrecerle resultados más útiles, predicciones más precisas o tiempos de entrenamiento más cortos.

En la tabla siguiente se resumen algunas de las características más importantes de los algoritmos de las familias de clasificación, regresión y agrupación en clústeres:

| **Algoritmo** | **Precisión** | **Tiempo de entrenamiento** | **Linealidad** | **Parámetros** | **Notas** |
| --- |:---:|:---:|:---:|:---:| --- |
| **Familia de clasificación** | | | | | |
| [Regresión logística de dos clases](./algorithm-module-reference/two-class-logistic-regression.md?WT.mc_id=docs-article-lazzeri) |Bueno  |Fast (rápido) |Sí |4 | |
| [Bosque de decisión de dos clases](./algorithm-module-reference/two-class-decision-forest.md?WT.mc_id=docs-article-lazzeri) |Excelente |Moderado |No |5 |Muestra los tiempos de puntuación más lentos. No se recomienda trabajar con Uno frente a todos multiclase, debido a los tiempos de puntuación más lentos ocasionados por el bloqueo de subprocesos en las predicciones de árbol acumuladas. |
| [Árbol de decisión ampliado de dos clases](./algorithm-module-reference/two-class-boosted-decision-tree.md?WT.mc_id=docs-article-lazzeri) |Excelente |Moderado |No |6 |Uso de memoria grande |
| [Red neuronal de dos clases](./algorithm-module-reference/two-class-neural-network.md?WT.mc_id=docs-article-lazzeri) |Bueno |Moderado |No |8 | |
| [Perceptrón promedio de dos clases](./algorithm-module-reference/two-class-averaged-perceptron.md?WT.mc_id=docs-article-lazzeri) |Bueno |Moderado |Sí |4 | |
| [Máquina de vectores de soporte de dos clases](./algorithm-module-reference/two-class-support-vector-machine.md?WT.mc_id=docs-article-lazzeri) |Bueno |Fast (rápido) |Sí |5 |Útil para conjuntos de características de gran tamaño |
| [Regresión logística multiclase](./algorithm-module-reference/multiclass-logistic-regression.md?WT.mc_id=docs-article-lazzeri) |Bueno |Fast (rápido) |Sí |4 | |
| [Bosque de decisión multiclase](./algorithm-module-reference/multiclass-decision-forest.md?WT.mc_id=docs-article-lazzeri) |Excelente |Moderado |No |5 |Muestra los tiempos de puntuación más lentos. |
| [Árbol de decisión ampliado multiclase](./algorithm-module-reference/multiclass-boosted-decision-tree.md?WT.mc_id=docs-article-lazzeri) |Excelente |Moderado |No |6 | Tiende a mejorar la precisión a pesar de correr el riesgo de tener menor cobertura. |
| [Red neuronal multiclase](./algorithm-module-reference/multiclass-neural-network.md?WT.mc_id=docs-article-lazzeri) |Bueno |Moderado |No |8 | |
| [Uno frente a todos multiclase](./algorithm-module-reference/one-vs-all-multiclass.md?WT.mc_id=docs-article-lazzeri) | - | - | - | - |Vea las propiedades del método de dos clases seleccionado |
| **Familia de regresión** | | | | | |
| [Regresión lineal](./algorithm-module-reference/linear-regression.md?WT.mc_id=docs-article-lazzeri) |Bueno |Fast (rápido) |Sí |4 | |
| [Regresión de bosque de decisión](./algorithm-module-reference/decision-forest-regression.md?WT.mc_id=docs-article-lazzeri)|Excelente |Moderado |No |5 | |
| [Regresión del árbol de decisión potenciado](./algorithm-module-reference/boosted-decision-tree-regression.md?WT.mc_id=docs-article-lazzeri) |Excelente |Moderado |No |6 |Uso de memoria grande |
| [Regresión de red neuronal](./algorithm-module-reference/neural-network-regression.md?WT.mc_id=docs-article-lazzeri) |Bueno |Moderado |No |8 | |
| **Familia de agrupación en clústeres** | | | | | |
| [Agrupación en clústeres K-Means](./algorithm-module-reference/k-means-clustering.md?WT.mc_id=docs-article-lazzeri) |Excelente |Moderado |Sí |8 |Un algoritmo de agrupación en clústeres |

## <a name="requirements-for-a-data-science-scenario"></a>Requisitos para un escenario de ciencia de datos

Una vez que sepa lo que quiere hacer con sus datos, necesita determinar los requisitos adicionales de su solución. 

Elija las opciones y posibles compensaciones para los siguientes requisitos:

- Precisión
- Tiempo de entrenamiento
- Linealidad
- Cantidad de parámetros
- Cantidad de características

## <a name="accuracy"></a>Precisión

La precisión de Machine Learning mide la eficacia de un modelo como la proporción de resultados verdaderos en comparación con el total de casos. En el diseñador de Machine Learning, el [componente Evaluar modelo](./algorithm-module-reference/evaluate-model.md?WT.mc_id=docs-article-lazzeri) calcula un conjunto de métricas de evaluación estándar del sector. Puede usar este componente para medir la precisión de un modelo entrenado.

No siempre es necesario obtener la respuesta más precisa posible. A veces, una aproximación ya es útil, según para qué se la desee usar. Si es así, puede reducir drásticamente el tiempo de procesamiento mediante el uso de métodos más aproximados. Los métodos aproximados también tienden naturalmente a evitar el sobreajuste.

Hay tres formas de usar el componente Evaluar modelo:

- Generar puntuaciones sobre los datos de entrenamiento para evaluar el modelo
- Generar puntuaciones en el modelo, pero compararlas con las de un conjunto de pruebas reservado.
- Comparar las puntuaciones de dos modelos diferentes pero relacionados, usando el mismo conjunto de datos.

Para obtener una lista completa de las métricas y los enfoques que se pueden usar para evaluar la precisión de los modelos de Machine Learning, vea [Componente Evaluar modelo](./algorithm-module-reference/evaluate-model.md?WT.mc_id=docs-article-lazzeri).

## <a name="training-time"></a>Tiempo de entrenamiento

En el aprendizaje supervisado, el entrenamiento significa utilizar datos históricos para crear un modelo de Machine Learning que minimiza los errores. La cantidad de minutos u horas necesarios para entrenar un modelo varía mucho según el algoritmo. A menudo, el tiempo de entrenamiento depende en gran medida de la precisión; por lo general, un factor acompaña al otro. 

Además, algunos algoritmos son más sensibles a la cantidad de puntos de datos que otros. Puede elegir un algoritmo concreto porque tiene una limitación de tiempo, sobre todo cuando el conjunto de datos es grande.

En el diseñador de Machine Learning, la creación y el uso de un modelo de Machine Learning suele ser un proceso de tres pasos:

1.  Para configurar un modelo, elija un tipo de algoritmo concreto y, posteriormente, defina sus parámetros o hiperparámetros. 

2.  Especifique un conjunto de datos que esté etiquetado y tenga datos compatibles con el algoritmo. Conecte los datos y el modelo al [Componente Entrenar modelo](./algorithm-module-reference/train-model.md?WT.mc_id=docs-article-lazzeri).

3.  Una vez que finalice el entrenamiento, use el modelo entrenado con uno de los [componentes de puntuación](./algorithm-module-reference/score-model.md?WT.mc_id=docs-article-lazzeri) para realizar predicciones sobre datos nuevos.

## <a name="linearity"></a>Linealidad

En las estadísticas y el aprendizaje automático, la linealidad significa que hay una relación lineal entre una variable y una constante del conjunto de datos. Por ejemplo, los algoritmos de clasificación lineal asumen que las clases pueden estar separadas mediante una línea recta (o su análogo de mayores dimensiones).

Muchos algoritmos de aprendizaje automático hacen uso de la linealidad. En el diseñador de Azure Machine Learning, incluyen: 

- [Regresión logística multiclase](./algorithm-module-reference/multiclass-logistic-regression.md?WT.mc_id=docs-article-lazzeri)
- [Regresión logística de dos clases](./algorithm-module-reference/two-class-logistic-regression.md?WT.mc_id=docs-article-lazzeri)
- [Máquina de vectores de soporte](./algorithm-module-reference/two-class-support-vector-machine.md?WT.mc_id=docs-article-lazzeri)  

Los algoritmos de regresión lineal suponen que las tendencias de datos siguen una línea recta. Esta suposición no es perjudicial en ciertos problemas, pero en otros reduce la precisión. A pesar de sus inconvenientes, los algoritmos lineales son muy usados como primera estrategia. Tienden a ser algorítmicamente sencillos y rápidos para entrenar.

![Límite de clase no lineal](./media/how-to-select-algorithms/nonlinear-class-boundary.png)

***Límite de clase no lineal** _: _la dependencia de un algoritmo de clasificación lineal se traduciría en una baja precisión.*

![Datos con tendencia no lineal](./media/how-to-select-algorithms/nonlinear-trend.png)

***Datos con tendencia no lineal** _: _el uso de un método de regresión lineal generaría más errores de los necesarios.*

## <a name="number-of-parameters"></a>Cantidad de parámetros

Los parámetros son los botones que un científico de datos activa al configurar un algoritmo. Son números que afectan al comportamiento del algoritmo, como la tolerancia a errores o la cantidad de iteraciones, o bien opciones de las variantes de comportamiento del algoritmo. Tanto el tiempo de entrenamiento como la precisión del algoritmo a veces pueden ser sensibles y requerir solo la configuración correcta. Normalmente, los algoritmos con parámetros de números grandes requieren la mayor cantidad de pruebas y errores posible para encontrar una buena combinación.

También existe el [Componente Optimizar hiperparámetros del modelo](./algorithm-module-reference/tune-model-hyperparameters.md?WT.mc_id=docs-article-lazzeri) en el diseñador de Machine Learning: el objetivo de este componente es determinar los hiperparámetros óptimos para un modelo de Machine Learning. El componente compila y prueba varios modelos con diferentes combinaciones de valores. Compara las métricas de todos los modelos para obtener las combinaciones de valores. 

Aunque esta es una excelente manera de asegurarse de que se ha distribuido el espacio de parámetros, el tiempo necesario para entrenar un modelo aumenta de manera exponencial con el número de parámetros. La ventaja es que tener muchos parámetros normalmente indica que un algoritmo tiene mayor flexibilidad. Por lo general, puede conseguir una precisión muy elevada, siempre y cuando se encuentre la combinación correcta de configuraciones de parámetros.

## <a name="number-of-features"></a>Cantidad de características

En Machine Learning, una característica es una variable cuantificable del fenómeno que intenta analizar. Para ciertos tipos de datos, la cantidad de características puede ser muy grande en comparación con la cantidad de puntos de datos. Este suele ser el caso de la genética o los datos textuales. 

Una gran cantidad de características puede trabar algunos algoritmos de aprendizaje, lo que hace que el tiempo de entrenamiento sea demasiado largo. Las [máquinas de vectores de soporte](./algorithm-module-reference/two-class-support-vector-machine.md?WT.mc_id=docs-article-lazzeri) son especialmente apropiadas para escenarios con un gran número de características. Por esta razón, se han usado en muchas aplicaciones, desde la recuperación de información hasta la clasificación de texto e imágenes. Las máquinas de vectores de soporte pueden usarse para tareas de clasificación y regresión.

Dada una salida específica, la selección de características hace referencia al proceso de aplicación de pruebas estadísticas a las entradas. para determinar qué columnas son más predecibles de la salida. El [componente Selección de características basada en filtro](./algorithm-module-reference/filter-based-feature-selection.md?WT.mc_id=docs-article-lazzeri) del diseñador de Machine Learning proporciona varios algoritmos de selección de características entre los que elegir. El componente incluye métodos de correlación como la correlación de Pearson y valores de la prueba χ2.

También puede usar el [componente Importancia de la característica de permutación](./algorithm-module-reference/permutation-feature-importance.md?WT.mc_id=docs-article-lazzeri) para calcular un conjunto de puntuaciones de importancia de la característica del conjunto de resultados. Luego, estas puntuaciones se pueden usan para ayudarle a determinar las mejores características que se deben usar en un modelo.

## <a name="next-steps"></a>Pasos siguientes

 - [Más información sobre el diseñador de Azure Machine Learning](./concept-designer.md?WT.mc_id=docs-article-lazzeri)
 - Para obtener descripciones de todos los algoritmos de aprendizaje automático disponibles en el diseñador de Azure Machine Learning, vea la [referencia de algoritmos y componentes del diseñador de Machine Learning](./component-reference/component-reference.md?WT.mc_id=docs-article-lazzeri)
 - Para explorar la relación entre aprendizaje profundo, aprendizaje automático e inteligencia artificial, consulte [Aprendizaje profundo frente a Aprendizaje automático](./concept-deep-learning-vs-machine-learning.md?WT.mc_id=docs-article-lazzeri)