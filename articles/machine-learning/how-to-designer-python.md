---
title: Ejecutar Script de Python en el diseñador
titleSuffix: Azure Machine Learning
description: Aprenda a usar el módulo de ejecución de script de Python en el diseñador de Azure Machine Learning para ejecutar operaciones personalizadas escritas en Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: mldata
author: likebupt
ms.author: keli19
ms.date: 10/21/2021
ms.topic: how-to
ms.custom: designer, devx-track-python
ms.openlocfilehash: 7c944ddd07f549a4956d334fea809d80f02db337
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131564886"
---
# <a name="run-python-code-in-azure-machine-learning-designer"></a>Ejecutar código de Python en el diseñador de Azure Machine Learning

En este artículo aprenderá a usar el componente [Execute Python Script](algorithm-module-reference/execute-python-script.md) (Ejecutar script de Python) para agregar lógica personalizada al diseñador de Azure Machine Learning. En el siguiente procedimiento se usa la biblioteca de Pandas para realizar la ingeniería de una característica simple.

Puede usar el editor de código integrado para agregar rápidamente lógica sencilla de Python. Si desea agregar código más complejo o cargar más bibliotecas de Python, debe usar el método de archivo ZIP.

El entorno de ejecución predeterminado usa la distribución Anacondas de Python. Para obtener una lista completa de los paquetes preinstalados, consulte la página de [referencia del componente Execute Python Script](algorithm-module-reference/execute-python-script.md).

![Ejecución de asignación de entrada de Python](media/how-to-designer-python/execute-python-map.png)

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="execute-python-written-in-the-designer"></a>Ejecución de Python escrita en el diseñador

### <a name="add-the-execute-python-script-component"></a>Incorporación del componente Execute Python Script (Ejecutar script de Python)

1. Busque el componente **Execute Python Script** (Ejecutar script de Python) en la paleta del diseñador. Se puede encontrar en la sección **Python Language** (Lenguaje Python).

1. Arrastre el componente y colóquelo en el lienzo de la canalización.

### <a name="connect-input-datasets"></a>Conexión de conjuntos de datos de entrada

En este artículo se utiliza el conjunto de datos de ejemplo **Automobile price data (Raw)** (Datos de precios de automóviles, sin procesar). 

1. Arrastre y coloque el conjunto de datos en el lienzo de la canalización.

1. Conecte el puerto de salida del conjunto de datos al puerto de entrada superior izquierdo del componente **Execute Python Script** (Ejecutar script de Python). El diseñador expone la entrada como un parámetro al script del punto de entrada.
    
    El puerto de entrada derecho está reservado para las bibliotecas comprimidas de Python.

    ![Conexión de conjuntos de datos](media/how-to-designer-python/connect-dataset.png)
        

1. Tome nota del puerto de entrada que use. El diseñador asigna el puerto de entrada izquierdo a la variable `dataset1` y el puerto de entrada del medio a `dataset2`. 

Los componentes de entrada son opcionales, ya que puede generar o importar datos directamente en el componente **Execute Python Script** (Ejecutar script de Python).

### <a name="write-your-python-code"></a>Escritura del código de Python

El diseñador proporciona un script de punto de entrada inicial para que pueda editar y escribir su propio código de Python. 

En este ejemplo, se usa Pandas para combinar dos columnas que se encuentran en el conjunto de automóviles, **Price** y **Horsepower**, y crear con ellas una nueva columna, **Dollars per horsepower**. Esta columna representa la cantidad que se paga por cada caballo de potencia; esta información puede resultar una característica útil para decidir si un coche supone una buena oportunidad en términos de costo. 

1. Seleccione el componente **Execute Python Script** (Ejecutar script de Python).

1. En el panel que aparece a la derecha del lienzo, seleccione el cuadro de texto **Python script**(Script de Python).

1. Copie y pegue el siguiente código en el cuadro de texto.

    ```python
    import pandas as pd
    
    def azureml_main(dataframe1 = None, dataframe2 = None):
        dataframe1['Dollar/HP'] = dataframe1.price / dataframe1.horsepower
        return dataframe1
    ```
    La canalización debería ser similar a la siguiente imagen:
    
    ![Ejecución de la canalización de Python](media/how-to-designer-python/execute-python-pipeline.png)

    El script de punto de entrada debe contener la función `azureml_main`. Hay dos parámetros de función que se asignan a los dos puertos de entrada del componente **Execute Python Script** (Ejecutar script de Python).

    El valor devuelto debe ser una trama de datos de Pandas. Puede devolver hasta dos tramas de datos como salidas del componente.
    
1. Envíe la canalización.

Ahora tiene un conjunto de datos con la nueva característica **Dollars/HP**, que podría ser útil para entrenar un recomendador de coches. Este es un ejemplo de extracción de características y reducción de la dimensionalidad. 

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [importar sus propios datos](how-to-designer-import-data.md) en el diseñador de Azure Machine Learning.
