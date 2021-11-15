---
title: 'Crear un modelo de Python: referencia del componente'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el componente Crear modelo de Python de Azure Machine Learning para crear un componente de modelado o procesamiento de datos personalizado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 08/18/2021
ms.openlocfilehash: f007f39217ebfdc644bb8c9d9510e4e5a7cc80fd
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131565970"
---
# <a name="create-python-model-component"></a>Componente Crear un modelo de Python

En este artículo se describe un componente del diseñador de Azure Machine Learning.

Aprenda a usar el componente Crear modelo de Python para crear un modelo no entrenado a partir de un script de Python. Puede basar el modelo en cualquier aprendiz que esté incluido en un paquete de Python en el entorno del diseñador de Azure Machine Learning. 

Después de crear el modelo, puede usar [Train model](train-model.md) (Entrenar modelo) para entrenar el modelo en un conjunto de datos, al igual que cualquier otro aprendiz en Azure Machine Learning. El modelo entrenado puede pasarse a [Score Model](score-model.md) (Puntuar modelo) para hacer predicciones. Después, puede guardar el modelo entrenado y publicar el flujo de trabajo de puntuación como un servicio web.

> [!WARNING]
> Actualmente, no se puede conectar este componente al componente **Optimizar hiperparámetros del modelo**, ni pasar los resultados puntuados de un modelo de Python a [Evaluar modelo](evaluate-model.md). Si tiene que optimizar los hiperparámetros de un modelo o evaluarlo, puede escribir un script de Python personalizado mediante el componente [Ejecutar script de Python](execute-python-script.md).


## <a name="configure-the-component"></a>Configuración del componente

El uso de este componente requiere conocimientos intermedios o expertos de Python. El componente admite el uso de cualquier aprendiz que se incluya en los paquetes de Python ya instalados en Azure Machine Learning. Consulte la lista de paquetes de Python instalados previamente en [Execute Python Script](execute-python-script.md) (Ejecutar script de Python).

> [!NOTE]
> Tenga mucho cuidado al escribir el script y asegúrese de que no incluya ningún error de sintaxis, por ejemplo, el uso de un objeto no declarado o de un componente no importado.

> [!NOTE]
> Además, preste atención adicional a la lista de componentes preinstalados en [Ejecución de script de Python](execute-python-script.md). Importe solo los componentes preinstalados. No instale paquetes adicionales como "pip install xgboost" en este script; de lo contrario, se producirán errores al leer los modelos en los componentes de nivel inferior.
  
En este artículo se muestra cómo usar **Create Python Model** (Crear modelo de Python) con una canalización sencilla. Este es el diagrama de la canalización:

![Diagrama de Create Python Model (Crear modelo de Python)](./media/module/create-python-model.png)

1. Seleccione **Create Python Model** (Crear modelo de Python) y edite el script para implementar el proceso de modelado o administración de datos. Puede basar el modelo en cualquier aprendiz que se incluya en un paquete de Python en el entorno de Azure Machine Learning.

> [!NOTE]
> Preste atención a los comentarios del código de ejemplo del script y asegúrese de que el script cumple estrictamente el requisito, incluido el nombre de clase, los métodos y la firma del método. La infracción provocará excepciones. 
> **Create Python Model** (Crear modelo de Python) solo admite la creación de modelos basados en sklearn que se entrenarán mediante **Entrenar modelo**.

   El código de ejemplo siguiente del clasificador Naive Bayes de dos clases usa el conocido paquete *sklearn*:

   ```Python

   # The script MUST define a class named AzureMLModel.
   # This class MUST at least define the following three methods:
       # __init__: in which self.model must be assigned,
       # train: which trains self.model, the two input arguments must be pandas DataFrame,
       # predict: which generates prediction result, the input argument and the prediction result MUST be pandas DataFrame.
   # The signatures (method names and argument names) of all these methods MUST be exactly the same as the following example.

   # Please do not install extra packages such as "pip install xgboost" in this script,
   # otherwise errors will be raised when reading models in down-stream components.
   
   import pandas as pd
   from sklearn.naive_bayes import GaussianNB


   class AzureMLModel:
       def __init__(self):
           self.model = GaussianNB()
           self.feature_column_names = list()

       def train(self, df_train, df_label):
           # self.feature_column_names records the column names used for training.
           # It is recommended to set this attribute before training so that the
           # feature columns used in predict and train methods have the same names.
           self.feature_column_names = df_train.columns.tolist()
           self.model.fit(df_train, df_label)

       def predict(self, df):
           # The feature columns used for prediction MUST have the same names as the ones for training.
           # The name of score column ("Scored Labels" in this case) MUST be different from any other columns in input data.
           return pd.DataFrame(
               {'Scored Labels': self.model.predict(df[self.feature_column_names]), 
                'probabilities': self.model.predict_proba(df[self.feature_column_names])[:, 1]}
           )


   ```

2. Conecte el componente **Crear modelo de Python** que acaba de crear a **Entrenar modelo** y **Puntuar modelo**.

3. Si tiene que evaluar el modelo, agregue un componente [Ejecutar script de Python](execute-python-script.md) y edite el script de Python.

   El siguiente script es código de evaluación de ejemplo:

   ```Python


   # The script MUST contain a function named azureml_main
   # which is the entry point for this component.

   # imports up here can be used to 
   import pandas as pd

   # The entry point function MUST have two input arguments:
   #   Param<dataframe1>: a pandas.DataFrame
   #   Param<dataframe2>: a pandas.DataFrame
   def azureml_main(dataframe1 = None, dataframe2 = None):
    
       from sklearn.metrics import accuracy_score, precision_score, recall_score, roc_auc_score, roc_curve
       import pandas as pd
       import numpy as np
    
       scores = dataframe1.ix[:, ("income", "Scored Labels", "probabilities")]
       ytrue = np.array([0 if val == '<=50K' else 1 for val in scores["income"]])
       ypred = np.array([0 if val == '<=50K' else 1 for val in scores["Scored Labels"]])    
       probabilities = scores["probabilities"]
    
       accuracy, precision, recall, auc = \
       accuracy_score(ytrue, ypred),\
       precision_score(ytrue, ypred),\
       recall_score(ytrue, ypred),\
       roc_auc_score(ytrue, probabilities)
    
       metrics = pd.DataFrame();
       metrics["Metric"] = ["Accuracy", "Precision", "Recall", "AUC"];
       metrics["Value"] = [accuracy, precision, recall, auc]
    
       return metrics,

   ```

## <a name="next-steps"></a>Pasos siguientes

Vea el [conjunto de componentes disponibles](component-reference.md) para Azure Machine Learning. 