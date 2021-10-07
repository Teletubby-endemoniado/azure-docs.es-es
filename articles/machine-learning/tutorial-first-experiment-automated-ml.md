---
title: 'Tutorial: Aprendizaje automático automatizado y entrenamiento de modelos de clasificación sin código'
titleSuffix: Azure Machine Learning
description: Entrene un modelo de clasificación sin necesidad de escribir ninguna línea de código mediante el aprendizaje automático automatizado de Azure Machine Learning en la interfaz de usuario de Studio.
services: machine-learning
ms.service: machine-learning
ms.subservice: automl
ms.topic: tutorial
author: cartacioS
ms.author: sacartac
ms.reviewer: nibaccam
ms.date: 07/01/2021
ms.custom: automl, FY21Q4-aml-seo-hack, contperf-fy21q4
ms.openlocfilehash: e3664e9a42a502509e74c43643d47f18e153ff6e
ms.sourcegitcommit: f29615c9b16e46f5c7fdcd498c7f1b22f626c985
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2021
ms.locfileid: "129429972"
---
# <a name="tutorial-train-a-classification-model-with-no-code-automl-in-the-azure-machine-learning-studio"></a>Tutorial: Entrenamiento de un modelo de clasificación con aprendizaje automático automatizado sin código en el Estudio de Azure Machine Learning

Aprenda a entrenar un modelo de clasificación con aprendizaje automático automatizado sin código en el Estudio de Azure Machine Learning. Este modelo de clasificación predice si un cliente suscribirá un depósito a plazo fijo con una institución financiera.

Con el aprendizaje automático automatizado, puede automatizar las tareas que requieren mucho tiempo. El aprendizaje automático recorre en iteración rápidamente muchas combinaciones de algoritmos e hiperparámetros para ayudarle a encontrar el mejor modelo según una métrica de éxito de su elección.

En este tutorial no escribirá absolutamente nada de código, usará la interfaz de Estudio de Azure Machine Learning para realizar el entrenamiento.  Aprenderá a realizar las siguientes tareas:

> [!div class="checklist"]
> * Cree un área de trabajo de Azure Machine Learning.
> * Ejecución de un experimento de aprendizaje automático automatizado.
> * Explore los detalles del modelo.
> * Implemente el modelo recomendado.

Pruebe también el aprendizaje automático automatizado para estos otros tipos de modelos:

* Para ver un ejemplo sin código de previsión, consulte [Tutorial: Previsión de la demanda con aprendizaje automático automatizado](tutorial-automated-ml-forecast.md).
* Para obtener un ejemplo de código de un modelo de regresión, consulte [Tutorial: Modelo de regresión con aprendizaje automático automatizado](tutorial-auto-train-models.md).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/).

* Descargue el archivo de datos [**bankmarketing_train.csv**](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv). La columna **y** indica si un cliente se ha suscrito a un depósito a plazo fijo, que se identifica más adelante como la columna de destino para las predicciones de este tutorial. 

## <a name="create-a-workspace"></a>Crear un área de trabajo

Un área de trabajo de Azure Machine Learning es un recurso básico de la nube que se usa para experimentar, entrenar e implementar modelos de aprendizaje automático. Vincula la suscripción y el grupo de recursos de Azure con un objeto fácilmente consumido del servicio. 

Existen muchas [maneras de crear un área de trabajo](how-to-manage-workspace.md). En este tutorial, creará un área de trabajo mediante Azure Portal, una consola basada en web para administrar los recursos de Azure.

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> Tome nota del **área de trabajo** y de la **suscripción**. Los necesitará para asegurarse de que crea el experimento en el lugar correcto. 

## <a name="sign-in-to-the-studio"></a>Inicio de sesión en Estudio de Azure Machine Learning

Lleve a cabo los siguientes pasos de configuración y ejecución del experimento a través de Azure Machine Learning Studio en https://ml.azure.com, una interfaz web consolidada que incluye herramientas de aprendizaje automático para llevar a la práctica escenarios de ciencia de datos para los profesionales de ciencia de datos de todos los niveles de conocimiento. Studio no se admite en los exploradores Internet Explorer.

1. Inicie sesión en [Azure Machine Learning Studio](https://ml.azure.com).

1. Seleccione la suscripción y el área de trabajo que ha creado.

1. Seleccione **Comenzar**.

1. Seleccione **ML automatizado** en la sección **Autor** en el panel de la izquierda.

   Como es su primer experimento de aprendizaje automático automatizado, verá una lista vacía y vínculos a la documentación.

   ![Página de introducción](./media/tutorial-first-experiment-automated-ml/get-started.png)

1. Seleccione **+New automated ML run** (+Nueva ejecución de ML automatizado). 

## <a name="create-and-load-dataset"></a>Creación y carga de un conjunto de datos

Antes de configurar el experimento, cargue el archivo de datos en el área de trabajo en forma de conjunto de datos de Azure Machine Learning. Así podrá asegurarse de que los datos tienen el formato adecuado para el experimento.

1. Cree un nuevo conjunto de datos; para ello, seleccione **From local files** (Desde archivos locales) en la lista desplegable **+ Create dataset** (+ Crear conjunto de datos). 

    1. En el formulario **Basic info** (Información básica), asígnele un nombre al conjunto de datos y, si lo desea, incluya una descripción. Actualmente, la interfaz de aprendizaje automático automatizado solo admite TabularDatasets, por lo que el valor predeterminado del tipo de conjunto de datos es *Tabular*.

    1. Seleccione **Next** (Siguiente) en la parte inferior izquierda.

    1. En el formulario **Datastore and file selection** (Selección del archivo y el almacén de datos), seleccione el almacén de archivos predeterminado que se configuró automáticamente durante la creación del área de trabajo, **workspaceblobstore (Azure Blob Storage)** . Aquí es donde se cargará el archivo de datos para que esté disponible en el área de trabajo.

    1. Haga clic en **Examinar**.
    
    1. Seleccione el archivo **bankmarketing_train.csv** en el equipo local. Este es el archivo que descargó como [requisito previo](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv).

    1. Asigne un nombre único al conjunto de datos y proporcione una descripción opcional. 

    1. Seleccione **Next** (Siguiente) en la parte inferior izquierda para cargarlo en el contenedor predeterminado que se configuró automáticamente durante la creación del área de trabajo.  
    
       Una vez completada la carga, el formulario de **configuración y vista previa** se rellena de forma inteligente en función del tipo de archivo. 
       
    1. Compruebe que el formulario **Settings y Preview** (Configuración y vista previa) se rellenan como se indica a continuación y seleccione **Next** (Siguiente).
        
        Campo|Descripción| Valor para el tutorial
        ---|---|---
        Formato de archivo|Define el diseño y el tipo de datos almacenados en un archivo.| Delimitado
        Delimitador|Uno o más caracteres para especificar el límite entre&nbsp; regiones independientes en texto sin formato u otros flujos de datos. |Coma
        Encoding|Identifica qué tabla de esquema de bit a carácter se va a usar para leer el conjunto de elementos.| UTF-8
        Encabezados de columna| Indica cómo se tratarán los encabezados del conjunto de datos, si existen.| Todos los archivos tienen los mismos encabezados
        Omitir filas | Indica el número de filas, si hay alguna, que se omiten en el conjunto de datos.| None

    1. El formulario **Scheme** (Esquema) permite una configuración adicional de los datos para este experimento. En este ejemplo, seleccione el conmutador de alternancia de la característica **day_of_week** para excluirla. Seleccione **Next** (Siguiente).
         ![Formulario de esquema](./media/tutorial-first-experiment-automated-ml/schema-tab-config.gif)
    1. En el formulario **Confirm details** (Confirmar detalles), compruebe que la información coincide con lo rellenado anteriormente en los formularios **Basic info** (Información básica), Datastore and file selection (Selección del archivo y el almacén de datos) y **Settings and preview** (Configuración y vista previa).
    
    1. Seleccione **Create** (Crear) para completar la creación del conjunto de datos.
    
    1. Seleccione el conjunto de datos cuando aparezca en la lista.
    
    1. Revise **Vista previa de los datos** para asegurarse de que no se incluyó **day_of_week** y seleccione **Cerrar**.

    1. Seleccione **Siguiente**.

## <a name="configure-run"></a>Configuración de la ejecución

Una vez cargados y configurados los datos, puede configurar el experimento. Este programa de instalación incluye tareas de diseño de experimentos, como la selección del tamaño del entorno de proceso y la especificación de la columna que se quiere predecir. 

1. Seleccione el botón de opción **Crear nuevo**.

1. Rellene el formulario **Configure Run** (Configurar ejecución) como se indica a continuación:
    1. Escriba el nombre del experimento: `my-1st-automl-experiment`.

    1. Seleccione **y** como la columna de destino en la que desea realizar las predicciones. Esta columna indica si el cliente se suscribió a un depósito a plazo o no.
    
    1. Seleccione **Create a new compute** (Crear un proceso) y configure el destino de proceso. Un destino de proceso es un entorno de recursos locales o en la nube que se usa para ejecutar el script de entrenamiento o para hospedar la implementación de un servicio. En este experimento se usa un proceso en la nube. 
        1. Rellene el formulario **Máquina virtual** para configurar el proceso.

            Campo | Descripción | Valor para el tutorial
            ----|---|---
            Prioridad de &nbsp;máquina&nbsp;virtual |Seleccione qué prioridad debe tener el experimento.| Dedicado
            Tipo de&nbsp;máquina&nbsp;virtual| Seleccione el tipo de máquina virtual del proceso.|CPU (Unidad central de procesamiento)
            Tamaño de la&nbsp;máquina&nbsp;virtual| Seleccione el tamaño de la máquina virtual para el proceso. Se proporciona una lista de los tamaños recomendados en función de los datos y el tipo de experimento. |Standard_DS12_V2
        
        1. Seleccione **Siguiente** para rellenar el formulario **Parámetros de configuración**.
        
            Campo | Descripción | Valor para el tutorial
            ----|---|---
            Nombre del proceso |  Un nombre único que identifique el contexto del proceso. | automl-compute
            Nodos mín./máx.| Para generar perfiles de datos, debe especificar uno o más nodos.|Número mínimo de nodos: 1<br>Número máximo de nodos: 6
            Segundos de inactividad antes de la reducción vertical | Tiempo de inactividad antes de que el clúster se escale automáticamente hasta el número mínimo de nodos.|1800 (valor predeterminado)
            Configuración avanzada | Valores para configurar y autorizar una red virtual para el experimento.| None               

        1. Seleccione **Crear** para crear el destino de proceso. 

            **Tarda unos minutos en completarse.** 

             ![Página Configuración](./media/tutorial-first-experiment-automated-ml/compute-settings.png)

        1. Después de la creación, seleccione el nuevo destino de proceso en la lista desplegable.

    1. Seleccione **Next** (Siguiente).

1. En el formulario **Select task and settings** (Configuración y selección de tarea), realice la configuración del experimento de aprendizaje automático automatizado especificando el tipo de tarea de aprendizaje automático y los valores de configuración.
    
    1.  Seleccione **Classification** (Clasificación) como tipo de tarea de aprendizaje automático.

    1. Seleccione **View additional configuration settings** (Ver opciones de configuración adicionales) y rellene los campos como se indica a continuación. Esta configuración es para controlar mejor el trabajo de entrenamiento. De lo contrario, los valores predeterminados se aplican en función de la selección y los datos del experimento.

        Configuraciones&nbsp;adicionales|Descripción|Valor&nbsp;para&nbsp;tutorial
        ------|---------|---
        Métrica principal| Métrica de evaluación por la que se medirá el algoritmo de aprendizaje automático.|AUC_weighted
        Explicación del mejor modelo| Muestra automáticamente la posible explicación relativa al mejor modelo creado mediante ML automatizado.| Habilitar
        Algoritmos bloqueados | Algoritmos que desea excluir del trabajo de entrenamiento.| None
        Criterios de exclusión| Si se cumplen los criterios, se detiene el trabajo de entrenamiento. |Tiempo del&nbsp;trabajo de&nbsp;entrenamiento (en horas): 1 <br> Umbral de&nbsp;puntuación&nbsp;de métrica: None
        Validación | Elija un tipo de validación cruzada y un número de pruebas.|Tipo de validación:<br>validación cruzada de&nbsp;k iteraciones&nbsp; <br> <br> Número de validaciones: 2
        Simultaneidad| Número máximo de iteraciones paralelas ejecutadas por iteración| Máximo de iteraciones&nbsp;simultáneas&nbsp;: 5
        
        Seleccione **Guardar**.
    
1. Para ejecutar el experimento, seleccione **Finalizar**. La pantalla **Run Details** (Detalles de ejecución) se abrirá con **Run status** (Estado de ejecución) al comenzar la preparación del experimento. Este estado se actualiza a medida que el experimento progresa. También aparecen notificaciones en la esquina superior derecha de Studio, para informarle del estado de su experimento.

>[!IMPORTANT]
> La preparación necesita de **10 a 15 minutos** en preparar la ejecución del experimento.
> Una vez que se ejecuta, se tarda de **2 a 3 minutos más para cada iteración**.  <br> <br>
> En producción, probablemente puede descansar un poco. Pero para este tutorial se recomienda empezar por explorar los algoritmos probados de la pestaña **Models** (Modelos) que se completan mientras los demás siguen en ejecución. 

##  <a name="explore-models"></a>Exploración de modelos

Vaya a la pestaña **Models** (Modelos) para ver los algoritmos (modelos) probados. De forma predeterminada, los modelos se ordenan por puntuación de las métricas a medida que se completan. De manera predeterminada, el modelo con la mayor puntuación según la métrica **AUC_weighted** elegida aparece en la parte superior de la lista.

Mientras espera a que terminen todos los modelos del experimento, seleccione **Algorithm name** (Nombre de algoritmo) de un modelo completado para explorar los detalles de rendimiento. 

A continuación, se desplazará a las pestañas **Details** (Detalles) y **Metrics** (Métricas) para ver las propiedades, las métricas y los gráficos de rendimiento del modelo seleccionado. 

![Detalles de la ejecución de iteración](./media/tutorial-first-experiment-automated-ml/run-detail.gif)

## <a name="model-explanations"></a>Explicaciones del modelo

Mientras espera a que terminen los modelos, puede echar un vistazo a las explicaciones de estos y ver qué características de datos (sin o con diseño) han influido en las predicciones de un modelo determinado. 

Estas explicaciones del modelo se pueden generar a petición y se resumen en el panel de explicaciones del modelo que forma parte de la pestaña **Explicaciones (versión preliminar)** .

Para generar explicaciones del modelo: 
 
1. Seleccione **Ejecución 1** en la parte superior para volver a la pantalla **Modelos**. 
1. Seleccione la pestaña **Modelos**.
1. En este tutorial, seleccione el primer modelo **MaxAbsScaler, LightGBM**.
1. Seleccione el botón **Explicar modelo** de la parte superior. A la derecha aparece el panel **Explicar modelo**. 
1. Seleccione **automl-compute**, que ha creado anteriormente. Este clúster de proceso inicia una ejecución secundaria para generar las explicaciones del modelo.
1. Seleccione **Crear** en la parte inferior. Aparece un mensaje de éxito verde en la parte superior de la pantalla. 
    >[!NOTE]
    > La ejecución de la explicación tarda entre dos y cinco minutos en terminar.
1. Seleccione el botón **Explicaciones (versión preliminar)** . Esta pestaña se rellena una vez completada la ejecución de la explicación.
1. En el lado izquierdo, expanda el panel y seleccione la fila que indica **Sin diseño** en **Características**. 
1. Seleccione la pestaña **Agregar importancia de la característica** de la derecha. Este gráfico muestra qué características de datos han influido en las predicciones del modelo seleccionado. 

    En este ejemplo, la *duración* parece haber influido más en las predicciones de este modelo.
    
    ![Panel de explicación del modelo](media/tutorial-first-experiment-automated-ml/model-explanation-dashboard.png)

## <a name="deploy-the-best-model"></a>Implementación del mejor modelo

La interfaz del aprendizaje automático automatizado permite implementar el mejor modelo como un servicio web en pocos pasos. La implementación es la integración del modelo para que pueda predecir datos nuevos e identificar posibles áreas de oportunidad. 

En este experimento, la implementación em un servicio web significa que la institución financiera tiene ahora una solución web iterativa y escalable para identificar posibles clientes de depósitos a plazo fijo. 

Compruebe si la ejecución del experimento ha finalizado. Para ello, seleccione **Run 1** (Ejecución 1) en la parte superior de la pantalla para volver a la página de ejecución primaria. Se muestra un estado **Completed** (Completado) en la esquina superior izquierda de la pantalla. 

Una vez completada la ejecución del experimento, la página **Details** (Detalles) se rellena con una sección **Best model summary** (Mejor resumen del modelo). En el contexto de este experimento, **VotingEnsemble** se considera el mejor modelo, según la métrica **AUC_weighted**.  

Se implementa este modelo, pero se recomienda que la implementación tarda unos 20 minutos en completarse. El proceso de implementación conlleva varios pasos, como el registro del modelo, la generación de recursos y su configuración para el servicio web.

1. Seleccione **VotingEnsemble** para abrir la página específica del modelo.

1. Seleccione el botón **Deploy** (Implementar) en la parte superior izquierda.

1. Rellene el panel **Deploy Model** (Implementar modelo) como se indica a continuación:

    Campo| Value
    ----|----
    Nombre de implementación| my-automl-deploy
    Descripción de implementación| Implementación de mi primer experimento de aprendizaje automático automatizado
    Compute type (Tipo de proceso) | Seleccione Azure Compute Instance (ACI) (Instancia de proceso de Azure [ACI])
    Enable authentication (Habilitar autenticación)| Deshabilitar. 
    Use custom deployments (Usar implementaciones personalizadas)| Deshabilitar. Permite que se generen automáticamente el archivo de controlador predeterminado (script de puntuación) y el archivo de entorno. 
    
    En este ejemplo, usamos los valores predeterminados que se proporcionan en el menú *Advanced* (Avanzada). 

1. Seleccione **Implementar**.  

    En la parte superior de la pantalla **Run** (Ejecutar) aparece un mensaje verde que indica que la operación se ha realizado correctamente y, en el panel **Model summary** (Resumen de modelo), aparece un mensaje de estado bajo **Deploy status** (Estado de implementación). Seleccione **Refresh** (Actualizar) periódicamente para comprobar el estado de la implementación.
    
Ya tiene un servicio web operativo para generar predicciones. 

Continúe con [**Pasos siguientes**](#next-steps) para aprender a consumir el nuevo servicio web y probar las predicciones con la compatibilidad con Azure Machine Learning incorporada en Power BI.

## <a name="clean-up-resources"></a>Limpieza de recursos

Los archivos de implementación son mayores que los archivos de datos y del experimento, por lo que cuesta más almacenarlos. Elimine solo los archivos de implementación para minimizar costos en su cuenta y si desea conservar el área de trabajo y los archivos del experimento. Elimine todo el grupo de recursos completo si no planea usar ninguno de los archivos.  

### <a name="delete-the-deployment-instance"></a>Eliminación de la instancia de implementación

Elimine solo la instancia de implementación de Azure Machine Learning de \//ml.azure.com/ si desea mantener el área de trabajo y el grupo de recursos para otros tutoriales y para su exploración. 

1. Vaya a [Azure Machine Learning](https://ml.azure.com/). Vaya al área de trabajo y, a la izquierda, en el panel **Assets** (Recursos), seleccione **Endpoints** (Puntos de conexión). 

1. Seleccione la implementación que desea eliminar y seleccione **Eliminar.** 

1. Seleccione **Continuar**.

### <a name="delete-the-resource-group"></a>Eliminar el grupo de recursos

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha usado la interfaz de aprendizaje automático automatizado de Azure Machine Learning para crear e implementar un modelo de clasificación. Para más información y ver los pasos siguientes, consulte estos artículos:

> [!div class="nextstepaction"]
> [Consumo de un servicio web](/power-bi/connect-data/service-aml-integrate?context=azure%2fmachine-learning%2fcontext%2fml-context)

+ Más información acerca del [aprendizaje automático automatizado](concept-automated-ml.md).
+ Para más información sobre las métricas de clasificación y los gráficos, consulte el artículo de [descripción de los resultados de aprendizaje automático automatizado](how-to-understand-automated-ml.md).
+ Más información sobre la [caracterización](how-to-configure-auto-features.md#featurization).
+ Más información acerca de la [generación de perfiles de datos](how-to-connect-data-ui.md#profile).


>[!NOTE]
> Este conjunto de conjuntos de marketing bancario está disponible bajo [licencia de Creative Commons (CCO: dominio público)](https://creativecommons.org/publicdomain/zero/1.0/). Todos los derechos de los contenidos individuales de la base de datos tienen la licencia [Database Contents License](https://creativecommons.org/publicdomain/zero/1.0/) y están disponibles en [Kaggle](https://www.kaggle.com/janiobachmann/bank-marketing-dataset). Este conjunto de datos estaba disponible originalmente en la [base de datos de aprendizaje automático de UCI](https://archive.ics.uci.edu/ml/datasets/bank+marketing).<br><br>
> [Moro et al., 2014] S. Moro, P. Cortez y P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, Elsevier, 62:22-31, June 2014.
