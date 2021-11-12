---
title: 'Regresión de árbol de decisión incrementado: referencia de componente'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el componente Regresión de árbol de decisión incrementado de Azure Machine Learning para crear un conjunto de árboles de regresión mediante la potenciación.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 08/24/2020
ms.openlocfilehash: 53c5ddf3fec898819efa5ce355b3fdac1d38d07a
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131566066"
---
# <a name="boosted-decision-tree-regression-component"></a>Componente Regresión de árbol de decisión incrementado

En este artículo se describe un componente del diseñador de Azure Machine Learning.

Use este componente para crear un conjunto de árboles de regresión mediante la potenciación. *Potenciación* significa que cada árbol depende de árboles anteriores. El algoritmo aprende ajustando el valor residual de los árboles que le preceden. Por lo tanto, la potenciación de un conjunto de árboles de decisión tiende a mejorar la precisión a pesar de correr el riesgo de tener menor cobertura.  

Este componente se basa en el algoritmo LightGBM.
  
Este método de regresión es un método de aprendizaje supervisado y, por lo tanto, requiere un *conjunto de datos con etiquetas*. La columna con la etiqueta debe contener valores numéricos.  

> [!NOTE]
> Use este componente solo con conjuntos de datos que empleen variables numéricas.  

Cuando haya definido el modelo, entrénelo usando el [modelo de entrenamiento](./train-model.md).

  
## <a name="more-about-boosted-regression-trees"></a>Más información sobre los árboles de regresión potenciados  

La potenciación es uno de los distintos métodos clásicos para crear modelos de conjuntos, junto con la agregación, los bosques aleatorios, etc.  En Azure Machine Learning, los árboles de decisión potenciados utilizan una implementación eficaz del algoritmo de potenciación del gradiente MART. La potenciación del gradiente es una técnica de aprendizaje automático para problemas de regresión. Genera cada árbol de regresión de forma gradual, usando una función de pérdida predefinida para medir el error en cada paso y corregirlo en el siguiente. Por lo tanto, el modelo de predicción es en realidad un conjunto de modelos de predicción más débiles.  
  
En los problemas de regresión, la potenciación genera una serie de árboles de forma gradual y después selecciona el árbol óptimo mediante una función arbitraria de pérdida diferenciable.  
  
Para más información, vea estos artículos:  
  
+ [https://wikipedia.org/wiki/Gradient_boosting#Gradient_tree_boosting](https://wikipedia.org/wiki/Gradient_boosting)

    Este artículo de la Wikipedia sobre potenciación del gradiente proporciona información general sobre los árboles potenciados. 
  
-  [https://research.microsoft.com/apps/pubs/default.aspx?id=132652](https://research.microsoft.com/apps/pubs/default.aspx?id=132652)  

    Microsoft Research: de RankNet a LambdaRank y a LambdaMART (información general) Por J. C. Burges.

El método de potenciación de gradientes también se puede usar para clasificar problemas reduciéndolos a la regresión con una función de pérdida adecuada. Para más información sobre la implementación de árboles potenciados para tareas de clasificación, vea [Árbol de decisión potenciado de segunda clase](./two-class-boosted-decision-tree.md).  

## <a name="how-to-configure-boosted-decision-tree-regression"></a>Cómo configurar la regresión del árbol de decisión potenciado

1.  Agregue el componente **Regresión de árbol de decisión incrementado** a la canalización. Puede encontrar este componente en **Machine Learning**, **Inicializar**, en la categoría **Regresión**. 
  
2.  Especifique cómo quiere que se entrene el modelo, estableciendo la opción **Create trainer mode** (Crear modo entrenador).  
  
    -   **Single Parameter** (Parámetro único): seleccione esta opción si sabe cómo quiere configurar el modelo y proporcione un conjunto específico de valores como argumentos. 
     
    -   **Parameter Range** (Intervalo de parámetros): seleccione esta opción si no está seguro de los mejores parámetros y quiere ejecutar un barrido de parámetros. Seleccione un rango de valores que iterar y el módulo [Optimización de hiperparámetros de un modelo](tune-model-hyperparameters.md) itera en todas las combinaciones posibles de los valores proporcionados para determinar los hiperparámetros que generan los resultados óptimos.    
   
  
3. **Número máximo de hojas por árbol**: indique el número máximo de nodos terminales (hojas) que se pueden crear en un árbol.  

    Al aumentar este valor, podría aumentar el tamaño del árbol y obtener una mayor precisión, a riesgo de experimentar un sobreajuste y un mayor tiempo de entrenamiento.  

4. **Número mínimo de muestras por nodo hoja**: indique el número mínimo de casos necesarios para crear un nodo terminal (hoja) en un árbol.

    Al aumentar este valor, aumenta el umbral para crear reglas nuevas. Por ejemplo, con el valor predeterminado de 1, incluso un solo caso puede provocar que se cree una regla nueva. Si aumenta el valor a 5, los datos de entrenamiento tienen que contener, como mínimo, cinco casos que cumplan las mismas condiciones.

5. **Velocidad de aprendizaje**: escriba un número entre 0 y 1 que defina el tamaño de paso durante el aprendizaje. La velocidad de aprendizaje determina la rapidez o lentitud con la que el aprendiz converge en la solución óptima. Si el tamaño del paso es demasiado grande, puede pasar por alto la solución óptima. Si el tamaño del paso es demasiado pequeño, el entrenamiento tarda más tiempo en converger en la mejor solución.

6. **Número de árboles construidos**: indica el número total de árboles de decisión que se va a crear en el conjunto. Al crear más árboles de decisión, puede obtener una mejor cobertura, pero el tiempo de entrenamiento aumenta.

    Si se establece el valor en 1; sin embargo, al hacerlo, solo se genera un único árbol (el árbol con el conjunto inicial de parámetros) y no se realizan iteraciones adicionales.

7. **Número de iniciación aleatorio**: introduzca un número entero no negativo opcional para que se use como valor de inicialización aleatorio. Al especificar un valor, se garantiza la reproducibilidad durante las ejecuciones que tienen los mismos datos y parámetros.

    De forma predeterminada, el valor de inicialización aleatorio se establece en 0, lo que significa que el valor de inicialización inicial se obtiene a partir del reloj del sistema.
  

9. Entrenamiento del modelo:

    + Si establece **Crear el modo de entrenador** en **Parámetro único**, conecte un conjunto de datos etiquetado y el componente [Entrenar modelo](train-model.md).  
  
    + Si establece **Create trainer mode** (Crear el modo de entrenador) en **Parameter Range** (Intervalo de parámetros), conecte un conjunto de datos etiquetado y entrene el modelo mediante [Tune Model Hyperparameters](tune-model-hyperparameters.md) (Optimizar los hiperparámetros del modelo).  
  
    > [!NOTE]
    > 
    > Si pasa un intervalo de parámetros a [Train Model](train-model.md) (Entrenar modelo), solo utiliza el valor predeterminado en la lista de parámetros única.  
    > 
    > Si pasa un único conjunto de valores de parámetro al componente [Optimizar los hiperparámetros del modelo](tune-model-hyperparameters.md), cuando espera un intervalo de valores para cada parámetro, omite los valores y usa los valores predeterminados para el aprendiz.  
    > 
    > Si selecciona la opción **Parameter Range** (Intervalo de parámetros) y especifica un valor único para algún parámetro, ese valor único que haya especificado se utilizará en todo el barrido, incluso si otros parámetros cambian en un intervalo de valores.
    

10. Envíe la canalización.  
  
## <a name="results"></a>Results

Una vez completado el entrenamiento:

+ Para usar el modelo para la puntuación, conecte [Entrenamiento del modelo](train-model.md) a [Puntuación del modelo](./score-model.md) para predecir los valores de ejemplos de nuevas entradas.

+ Para guardar una instantánea del modelo entrenado, seleccione la pestaña **Outputs** (Salidas) en el panel derecho de **Trained model** (Modelo entrenado) y haga clic en el icono **Register dataset** (Registrar conjunto de datos). La copia del modelo entrenado se guarda como un componente en el árbol de componentes y no se actualiza en las posteriores ejecuciones de la canalización.

## <a name="next-steps"></a>Pasos siguientes

Vea el [conjunto de componentes disponibles](component-reference.md) para Azure Machine Learning. 
