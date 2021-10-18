---
title: Novedades de la versión
titleSuffix: Azure Machine Learning
description: Conozca las actualizaciones más recientes de Azure Machine Learning y los SDK de Python para el aprendizaje automático y la preparación de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: larryfr
author: BlackMist
ms.date: 10/04/2021
ms.openlocfilehash: 598cc1944cb49024d91da476b95f9aa0b25f6279
ms.sourcegitcommit: 54e7b2e036f4732276adcace73e6261b02f96343
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2021
ms.locfileid: "129811092"
---
# <a name="azure-machine-learning-release-notes"></a>Notas de la versión de Azure Machine Learning

En este artículo conocerá las versiones de Azure Machine Learning.  Para obtener el contenido completo de referencia del SDK, visite la página de referencia del [**SDK principal para Python**](/python/api/overview/azure/ml/intro) de Azure Machine Learning.

__Fuente RSS__: reciba notificaciones cuando esta página se actualice copiando y pegando la siguiente dirección URL en su lector de fuentes: `https://docs.microsoft.com/api/search/rss?search=%22Azure+machine+learning+release+notes%22&locale=en-us`

## <a name="2021-10-11"></a>11-10-2021

### <a name="azure-machine-learning-sdk-for-python-v1350"></a>SDK de Azure Machine Learning para Python v1.35.0
  + **azureml-automl-core**
    + Habilitación del cálculo de métricas binarias
  + **azureml-contrib-fairness**
    + Mejora del mensaje de error en la descarga fallida del panel de control
  + **azureml-core**
    + Se ha resuelto el error al especificar nubes no públicas para la autenticación de identidad administrada.
    + Las marcas experimentales Dataset.File.upload_directory() y Dataset.Tabular.register_pandas_dataframe() no se eliminan.
    + Las marcas experimentales ahora se quitan en el método partition_by() de la clase TabularDataset.
  + **azureml-pipeline-steps**
    + Ahora se eliminan las marcas experimentales para el parámetro `partition_keys` de la clase ParallelRunConfig.
  + **azureml-interpret**
    + paquete azureml-interpret actualizado a intepret-community 0.20.*
  + **azureml-mlflow**
    + Posibilidad de registrar artefactos e imágenes con MLflow utilizando subdirectorios
  + **azureml-responsibleai**
    + Mejora del mensaje de error en la descarga fallida del panel de control
  + **azureml-train-automl-client**
    + Se ha agregado compatibilidad con tareas de Computer Vision, como clasificación de imágenes, detección de objetos y segmentación de instancias. La documentación detallada se puede encontrar en: https://docs.microsoft.com/azure/machine-learning/how-to-auto-train-image-models
    + Habilitación del cálculo de métricas binarias
  + **azureml-train-automl-runtime**
    + Agregue compatibilidad con TCNForecaster a las ejecuciones de pruebas del modelo.
    + Actualice el formato de salida del modelo de prueba predictions.csv output. Las columnas de salida incluyen ahora los valores de objetivo originales y las características que se pasaron a la ejecución de la prueba. Esto se puede desactivar estableciendo `test_include_predictions_only=True` en `AutoMLConfig` o estableciendo `include_predictions_only=True` en `ModelProxy.test()`. Si el usuario ha solicitado incluir solo predicciones, el formato de salida es similar (la previsión es la misma que la regresión): Clasificación => [valores predichos] [probabilidades] Regresión => [valores predichos] otro (por defecto): Clasificación => [etiquetas de datos de prueba original] [valores predichos] [probabilidades] [características] Regresión => [etiquetas de datos de prueba original] [valores predichos] [características] El nombre de columna `[predicted values]` = `[label column name] + "_predicted"`. Los nombres de columna `[probabilities]` = `[class name] + "_predicted_proba"`. Si no se ha pasado ninguna columna de destino como entrada a la ejecución de pruebas, `[original test data labels]` no estará en la salida.

## <a name="2021-09-07"></a>07-09-2021

### <a name="azure-machine-learning-sdk-for-python-v1340"></a>SDK de Azure Machine Learning para Python v1.34.0
  + **azureml-automl-core**
    + Permite reajustar una canalización de previsión previamente entrenada.
    + Permite obtener predicciones sobre los datos de entrenamiento (predicción dentro de la muestra) para hacer previsiones.
  + **azureml-automl-runtime**
    + Permite devolver probabilidades de predicción desde un punto de conexión implementado de un modelo clasificador de AutoML.
    + Se ha agregado una opción de previsión para que los usuarios especifiquen que todas las predicciones deben ser enteros.
    + El nombre de la columna de destino se ha quitado de los nombres de las características de explicación del modelo en los experimentos locales con training_data_label_column_name
    + como entrada del conjunto de datos.
    + Permite reajustar una canalización de previsión previamente entrenada.
    + Permite obtener predicciones sobre los datos de entrenamiento (predicción dentro de la muestra) para hacer previsiones.
  + **azureml-core**
    + Permite establecer el tipo de columna de flujo, así como montar y descargar columnas de flujo en el conjunto de datos tabular.
    + Se han agregado nuevos campos opcionales a Kubernetes.attach_configuration(identity_type=None e identity_ids=None), que permiten asociar KubernetesCompute con una identidad SystemAssigned o una identidad UserAssigned. Se van a incluir nuevos campos de identidad al llamar a print(compute_target) o compute_target.serialize(): identity_type, identity_id, principal_id y tenant_id/client_id.
  + **azureml-dataprep**
    + Permite establecer el tipo de columna de flujo del conjunto de datos tabular. Permite montar y descargar columnas de flujo del conjunto de datos tabular.
  + **azureml-defaults**
    + La dependencia `azureml-inference-server-http==0.3.1` se ha agregado a `azureml-defaults`.
  + **azureml-mlflow**
    + Permite la paginación de list_experiments API agregando los parámetros opcionales `max_results` y `page_token`. Para obtener documentación, consulte los documentos oficiales de MLflow.
  + **azureml-sdk**
    + Se ha reemplazado la dependencia de un paquete en desuso (azureml-train) de azureml-sdk.
    + Se ha agregado azureml-responsibleai a algunos extras de azureml-sdk
  + **azureml-train-automl-client**
    + Expone los parámetros `test_data` y `test_size` en `AutoMLConfig`. Estos parámetros se pueden usar para iniciar automáticamente una serie de pruebas una vez completada 
    + la fase de entrenamiento del modelo. La serie de pruebas calculará las predicciones con el mejor modelo y generará métricas con arreglo a estas predicciones.

## <a name="2021-08-24"></a>2021-08-24

### <a name="azure-machine-learning-experimentation-user-interface"></a>Interfaz de usuario de Experimentación de Azure Machine Learning
  + **Run Delete**
    + Run Delete es una nueva funcionalidad que permite a los usuarios eliminar una o varias ejecuciones de su área de trabajo.
    + Esta funcionalidad puede ayudarles a reducir los costos de almacenamiento y a administrar la capacidad de almacenamiento eliminando periódicamente ejecuciones y experimentos directamente desde la interfaz de usuario.
  + **Batch Cancel Run**
    + Batch Cancel Run es una nueva funcionalidad que permite a los usuarios seleccionar una o varias ejecuciones para cancelarlas en la lista de ejecuciones. 
    + Esta funcionalidad puede ayudar a los usuarios a cancelar varias ejecuciones en cola y a liberar espacio en el clúster.

## <a name="2021-08-18"></a>18-08-2021

### <a name="azure-machine-learning-experimentation-user-interface"></a>Interfaz de usuario de Experimentación de Azure Machine Learning
  + **Nombre para mostrar de la ejecución**
    + El nombre para mostrar de la ejecución es un nombre para mostrar nuevo, modificable y opcional que se puede asignar a una ejecución. 
    + Este nombre puede ayudar a realizar un seguimiento más eficaz, organizar y detectar las ejecuciones. 
    + El formato predeterminado del nombre para mostrar de la ejecución es adjetivo_nombre_guid (ejemplo: awesome_watch_2i3uns). 
    + Este nombre predeterminado se puede editar por otro más personalizable. Se puede editar desde la página Detalles de ejecución de la interfaz de Estudio de Azure Machine Learning.   

## <a name="2021-08-02"></a>02/08/2021

### <a name="azure-machine-learning-sdk-for-python-v1330"></a>SDK de Azure Machine Learning para Python v1.33.0
  + **azureml-automl-core**
    + Se ha mejorado el control de errores en torno a la recuperación del modelo XGBoost.
    + Se ha agregado la posibilidad de convertir las predicciones de float a enteros para las tareas de previsión y regresión.
    + Se ha actualizado el valor predeterminado de enable_early_stopping en AutoMLConfig a True.
  + **azureml-automl-runtime**
    + Se ha agregado la posibilidad de convertir las predicciones de float a enteros para las tareas de previsión y regresión.
    + Se ha actualizado el valor predeterminado de enable_early_stopping en AutoMLConfig a True.
  + **azureml-contrib-automl-pipeline-steps**
    + Las series temporales jerárquicas (HTS) están habilitadas para las tareas de previsión a través de canalizaciones.
    + Se agregó compatibilidad con conjuntos de datos tabulares para inferencia.
    + Se puede especificar una ruta de acceso personalizada para los datos de inferencia.
  + **azureml-contrib-reinforcementlearning**
    + Algunas propiedades de `azureml.core.environment.DockerSection` están en desuso, como la propiedad `shm_size` usada por los roles de trabajo de Ray en trabajos de aprendizaje de refuerzo. Ahora, esta propiedad se puede especificar en `azureml.contrib.train.rl.WorkerConfiguration` en su lugar.
  + **azureml-core**
    + Se ha corregido un hipervínculo en la documentación de `ScriptRunConfig.distributed_job_config`.
    + Los clústeres de proceso de Azure Machine Learning ahora se pueden crear en una ubicación diferente de la ubicación del área de trabajo. Esto resulta útil para maximizar la asignación de capacidad inactiva y administrar el uso de cuotas en distintas ubicaciones, sin tener que crear más áreas de trabajo tan solo para usar la cuota y crear un clúster de proceso en una ubicación determinada. Para más información, consulte [Creación de un clúster de proceso de Azure Machine Learning](how-to-create-attach-compute-cluster.md?tabs=python).
    + Se ha agregado display_name como campo de nombre mutable del objeto Run.
    + El conjunto de datos from_files ahora admite la omisión de extensiones de datos para datos de entrada de gran tamaño.
  + **azureml-dataprep**
    + Se ha corregido un error de to_dask_dataframe debido a una condición de carrera.
    + El conjunto de datos from_files ahora admite la omisión de extensiones de datos para datos de entrada de gran tamaño.
  + **azureml-defaults**
    + Vamos a quitar la dependencia azureml-model-management-sdk==1.0.1b6.post1 de azureml-defaults.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret a interpret-community 0.19*.
  + **azureml-pipeline-core**
    + Las series temporales jerárquicas (HTS) están habilitadas para las tareas de previsión a través de canalizaciones.
  + **azureml-train-automl-client**
    + Se ha cambiado al uso del almacén de blobs para el almacenamiento en caché en Automated ML.
    + Las series temporales jerárquicas (HTS) están habilitadas para las tareas de previsión a través de canalizaciones.
    + Se ha mejorado el control de errores en torno a la recuperación del modelo XGBoost.
    + Se ha actualizado el valor predeterminado de enable_early_stopping en AutoMLConfig a True.
  + **azureml-train-automl-runtime**
    + Se ha cambiado al uso del almacén de blobs para el almacenamiento en caché en Automated ML.
    + Las series temporales jerárquicas (HTS) están habilitadas para las tareas de previsión a través de canalizaciones.
    + Se ha actualizado el valor predeterminado de enable_early_stopping en AutoMLConfig a True.


## <a name="2021-07-06"></a>06/07/2021

### <a name="azure-machine-learning-sdk-for-python-v1320"></a>SDK de Azure Machine Learning para Python v1.32.0
+ **Mejoras y correcciones de errores**
  + **azureml-core**
    + Se ha expuesto el diagnóstico de estado del área de trabajo en el SDK o la CLI.
  + **azureml-defaults**
    + Se ha agregado la dependencia `opencensus-ext-azure==1.0.8` a azureml-defaults.
  + **azureml-pipeline-core**
    + Se ha actualizado AutoMLStep para usar imágenes precompiladas cuando el entorno para el envío de trabajos coincide con el entorno predeterminado.
  + **azureml-responsibleai**
    + Se ha agregado un nuevo cliente de análisis de errores para cargar, descargar y enumerar informes de análisis de errores.
    + Se ha garantizado que las versiones de los paquetes `raiwidgets` y `responsibleai` estén sincronizados.
  + **azureml-train-automl-runtime**
    + Se ha establecido el tiempo asignado para buscar dinámicamente en varias estrategias de caracterización hasta un máximo de una cuarta parte del tiempo de espera total del experimento.


## <a name="2021-06-21"></a>21/06/2021

### <a name="azure-machine-learning-sdk-for-python-v1310"></a>SDK de Azure Machine Learning para Python v1.31.0
+ **Mejoras y correcciones de errores**
  + **azureml-core**
    + Se ha mejorado la documentación para la propiedad de plataforma en la clase Environment.
    + Se ha cambiado el tiempo de reducción vertical del nodo de proceso de AML de 120 segundos a 1800 segundos.
    + Se ha actualizado el vínculo de solución de problemas predeterminado que aparece en el portal para solucionar errores de ejecución a https://aka.ms/azureml-run-troubleshooting.
  + **azureml-automl-runtime**
    + Limpieza de datos: las muestras con valores de destino en [None, "", "nan", np.nan] se anularán antes de la caracterización o el entrenamiento del modelo.
  + **azureml-interpret**
    + Se ha aumentado el tiempo de espera para evitar errores de cola de tareas de vaciado en ejecuciones remotas de AzureML que usan ExplanationClient.
  + **azureml-pipeline-core**
    + Se ha agregado el parámetro jar al paso de Synapse.
  + **azureml-train-automl-runtime**
    + Se han corregido los límites de protección de cardinalidad alta para que estén más alineados con los documentos.

## <a name="2021-06-07"></a>07-06-2021

### <a name="azure-machine-learning-sdk-for-python-v1300"></a>SDK de Azure Machine Learning para Python v1.30.0
+ **Mejoras y correcciones de errores**
  + **azureml-core**
    + Anclaje de dependencia de `ruamel-yaml` a < 0.17.5 porque se publicó un cambio importante en la versión 0.17.5.
    + La propiedad `aml_k8s_config` se reemplaza por los parámetros `namespace`, `default_instance_type` e `instance_types` para la asociación de `KubernetesCompute`.
    + Las claves de sincronización del área de trabajo se cambiaron a una operación de larga duración.
  + **azureml-automl-runtime**
    + Se han corregido problemas por los que las ejecuciones con macrodatos pueden generar errores de tipo `Elements of y_test cannot be NaN`.
  + **azureml-mlflow**
    + Corrección de errores del complemento de implementación de MLFlow para modelos sin firma.
  + **azureml-pipeline-steps**
    + ParallelRunConfig: documento de actualización para process_count_per_node.
  + **azureml-train-automl-runtime**
    + Compatibilidad con cuantiles definidos por el usuario durante la inferencia de MM
    + Compatibilidad con forecast_quantiles durante la inferencia por lotes.
  + **azureml-contrib-automl-pipeline-steps**
    + Compatibilidad con cuantiles definidos por el usuario durante la inferencia de MM
    + Compatibilidad con forecast_quantiles durante la inferencia por lotes.

## <a name="2021-05-25"></a>25 de mayo de 2021

### <a name="announcing-the-cli-v2-preview-for-azure-machine-learning"></a>Anuncio de la CLI (v2) (versión preliminar) para Azure Machine Learning

La extensión `ml` de la CLI de Azure es la interfaz de próxima generación de Azure Machine Learning. Permite entrenar e implementar modelos desde la línea de comandos, e incluye características que aceleran el escalado vertical y horizontal de la ciencia de datos al tiempo que se hace un seguimiento del ciclo de vida del modelo. [Instalación e introducción](how-to-configure-cli.md).

### <a name="azure-machine-learning-sdk-for-python-v1290"></a>SDK de Azure Machine Learning para Python v1.29.0
+ **Mejoras y correcciones de errores**
  + **Cambios importantes**
    + Se ha eliminado la compatibilidad con Python 3.5.
  + **azureml-automl-runtime**
    + Se ha corregido un error que hacía que STLFeaturizer no funcionara si la longitud de la serie temporal era más corta que la estacionalidad. Este error se manifestaba como IndexError. El caso se controla ahora sin errores, aunque el componente estacional del STL solo conste de ceros en este caso.
  + **azureml-contrib-automl-dnn-vision**
    + Se ha agregado un método para la inferencia por lotes con rutas de acceso de archivo.
  + **azureml-contrib-gbdt**
    + El paquete azureml-contrib-gbdt ha quedado en desuso y es posible que no reciba actualizaciones futuras y se retire de la distribución por completo.
  + **azureml-core**
    + Se ha corregido la explicación del parámetro create_if_not_exists en Datastore.register_azure_blob_container.
    + Se ha agregado código de ejemplo a la clase DatasetConsumptionConfig.
    + Se ha agregado compatibilidad con step como eje alternativo para los valores de métrica escalares en run.log().
  + **azureml-dataprep**
    + El tamaño límite aceptado de la partición en `_with_partition_size()` es de 2 GB.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret a la versión más reciente del paquete interpret-core.
    + Se ha eliminado la compatibilidad con SHAP DenseData, que ha quedado en desuso en SHAP 0.36.0.
    + Se ha habilitado `ExplanationClient` para realizar la carga en un almacén de datos especificado por el usuario.
  + **azureml-mlflow**
    + Se ha movido azureml-mlflow a mlflow-skinny para reducir la superficie de dependencia y mantener la compatibilidad completa con el complemento.
  + **azureml-pipeline-core**
    + El ejemplo de código PipelineParameter se ha actualizado en el documento de referencia para usar el parámetro correcto.


## <a name="2021-05-10"></a>2021-05-10

### <a name="azure-machine-learning-sdk-for-python-v1280"></a>SDK de Azure Machine Learning para Python v1.28.0
+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + Se ha mejorado el script de puntuación de AutoML para que sea coherente con el diseñador.
    + Se ha corregido un error que hacía que la previsión con el modelo Prophet produjera un error "falta una columna" si se entrenaba con una versión anterior del SDK.
    + Se ha agregado el modelo ARIMAX a las listas de modelos públicos admitidos para la previsión del SDK de AutoML. En este caso, ARIMAX es una regresión con errores de ARIMA y un caso especial de los modelos de función de transferencia desarrollados por Box y Jenkins. Puede encontrar una explicación de las diferencias entre los dos enfoques en [El embrollo de los modelos de ARIMAX](https://robjhyndman.com/hyndsight/arimax/). A diferencia del resto de los modelos multivariante que usan características generadas automáticamente y dependientes del tiempo (hora del día, día del año, etc.) en AutoML, este modelo solo usa características proporcionadas por el usuario y facilita la interpretación de los coeficientes.
  + **azureml-contrib-dataset**
    + Se ha actualizado la descripción de la documentación con la indicación de que libfuse debe instalarse mientras se usa el montaje.
  + **azureml-core**
    + La imagen mantenida de la CPU predeterminada es ahora mcr.microsoft.com/azureml/openmpi3.1.2-ubuntu18.04. La imagen de la GPU predeterminada es ahora mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.2-cudnn8-ubuntu18.04.
    + Run.fail() ahora está en desuso; use Run.tag() para marcar la ejecución como con errores o use Run.cancel() para marcar la ejecución como cancelada.
    + Se ha actualizado la documentación con una nota que dice que libfuse debe instalarse al montar un conjunto de datos de archivos.
    + Se ha agregado la compatibilidad experimental register_dask_dataframe() al conjunto de datos tabular.
    + Compatibilidad de DatabricksStep con Azure Blob/ADL-S como entradas y salidas y exposición del parámetro permit_cluster_restart para permitir al cliente decidir si AML puede reiniciar el clúster cuando haya que agregar la configuración de acceso de E/S al clúster
  + **azureml-dataset-runtime**
    + azureml-dataset-runtime ahora admite las versiones pyarrow < 4.0.0
  + **azureml-mlflow**
    + Se ha agregado compatibilidad para la implementación en AzureML a través de nuestro complemento MLFlow.
  + **azureml-pipeline-steps**
    + Compatibilidad de DatabricksStep con Azure Blob/ADL-S como entradas y salidas y exposición del parámetro permit_cluster_restart para permitir al cliente decidir si AML puede reiniciar el clúster cuando haya que agregar la configuración de acceso de E/S al clúster
  + **azureml-synapse**
    + Habilitación de la audiencia en la autenticación msi
  + **azureml-train-automl-client**
    + Se ha agregado un vínculo cambiado para el documento de destino de proceso.


## <a name="2021-04-19"></a>19/04/2021

### <a name="azure-machine-learning-sdk-for-python-v1270"></a>SDK de Azure Machine Learning para Python v1.27.0
+ **Mejoras y correcciones de errores**
  + **azureml-core**
    + Se ha agregado la capacidad de reemplazar el valor de tiempo de espera predeterminado para la carga de artefactos a través de la variable de entorno "AZUREML_ARTIFACTS_DEFAULT_TIMEOUT".
    + Se ha corregido un error por el que no se respetaba la configuración de Docker en el objeto Environment de ScriptRunConfig.
    + Se permite la creación de particiones de un conjunto de datos al copiarlo en un destino.
    + Se ha agregado un modo personalizado a OutputDatasetConfig para permitir el paso de conjuntos de datos creados en canalizaciones mediante una función de vínculo. Aportan compatibilidad con las mejoras realizadas para habilitar la creación de particiones tabulares para PRS.
    + Se ha agregado un nuevo tipo de proceso KubernetesCompute a azureml-core.
  + **azureml-pipeline-core**
    + Adición de un modo personalizado a OutputDatasetConfig y posibilidad de que un usuario pase a través de los conjuntos de datos creados en las canalizaciones mediante una función de vínculo. Los destinos de ruta de acceso de archivo admiten marcadores de posición. Aportan compatibilidad con las mejoras realizadas para habilitar la creación de particiones tabulares para PRS.
    + Adición de un nuevo tipo de proceso KubernetesCompute a azureml-core.
  + **azureml-pipeline-steps**
    + Adición de un nuevo tipo de proceso KubernetesCompute a azureml-core.
  + **azureml-synapse**
    + Actualización de la dirección URL de la interfaz de usuario de Spark en el widget de azureml synapse
  + **azureml-train-automl-client**
    + El caracterizador STL para la tarea de previsión usa ahora una detección de estacionalidad más sólida basada en la frecuencia de la series temporales.
  + **azureml-train-core**
    + Se ha corregido un error por el que no se respetaba la configuración de Docker en el objeto Environment.
    + Adición de un nuevo tipo de proceso KubernetesCompute a azureml-core.


## <a name="2021-04-05"></a>05-04-2021

### <a name="azure-machine-learning-sdk-for-python-v1260"></a>SDK de Azure Machine Learning para Python v1.26.0
+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se ha corregido un problema por el que los modelos naive se recomiendan en las ejecuciones de AutoMLStep y se producían errores con las características de ventana de retardo y de desplazamiento de objetivo. Estos modelos no se recomiendan cuando se establecen los límites de destino o el tamaño de la ventana con desplazamiento de destino.
    +  Se ha cambiado la salida de la consola al enviar una ejecución de AutoML para mostrar un vínculo del portal a la ejecución.
  + **azureml-core**
    + Se ha agregado el modo HDFS en la documentación.
    + Se ha agregado compatibilidad para comprender las particiones del conjunto de datos de archivo basadas en una estructura global.
    + Se ha agregado compatibilidad para actualizar el registro de contenedor asociado al área de trabajo de AzureML.
    + Los atributos de entorno en desuso en DockerSection: "enabled", "shared_volume" y "arguments" forman parte ahora de DockerConfiguration en RunConfiguration.
    + Documentación actualizada de clonación de la CLI de canalización
    + Se han actualizado los URI del portal para incluir el inquilino para la autenticación.
    + Se ha quitado el nombre del experimento de los URI de ejecución para evitar redireccionamientos. 
    + Se ha actualizado el URI del experimento para usar el identificador del experimento.
    + Correcciones de errores para asociar el proceso remoto con la CLI de AzureML.
    + Se han actualizado los URI del portal para incluir el inquilino para la autenticación.
    + Se ha actualizado el URI del experimento para usar el identificador del experimento.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret para usar interpret-community 0.17.0.
  + **azureml-opendatasets**
    + Especifique la validación del tipo de fecha de inicio y fecha de finalización y la indicación de error si no es del tipo datetime.
  + **azureml-parallel-run**
    + [Característica experimental] Agregue el parámetro `partition_keys` a ParallelRunConfig, si se ha especificado, y los conjuntos de datos de entrada se particionarán en minilotes mediante las claves especificadas en él. Requiere que todos los conjuntos de datos de entrada sean conjuntos de datos con particiones.
  + **azureml-pipeline-steps**
    + Corrección de errores: compatibilidad con path_on_compute al pasar la configuración del conjunto de datos como descarga.
    + Se ha puesto en desuso RScriptStep en favor del uso de CommandStep para ejecutar scripts de R en canalizaciones. 
    + Se ha puesto en desuso EstimatorStep en favor del uso de CommandStep para ejecutar el entrenamiento de ML (incluido el entrenamiento distribuido) en las canalizaciones.
  + **azureml-sdk**
    + La actualización de Python requiere una versión < 3.9 para azureml-sdk.
  + **azureml-train-automl-client**
    +  Se ha cambiado la salida de la consola al enviar una ejecución de AutoML para mostrar un vínculo del portal a la ejecución.
  + **azureml-train-core**
    + Los atributos "enabled", "shared_volume" y "arguments" de DockerSection se han puesto en desuso en favor del uso de DockerConfiguration con ScriptRunConfig.
    +  Uso de Azure Open Datasets para el conjunto de datos de MNIST
    + Se han actualizado los mensajes de error de Hyperdrive.


## <a name="2021-03-22"></a>22-03-2021

### <a name="azure-machine-learning-sdk-for-python-v1250"></a>SDK de Azure Machine Learning para Python v1.25.0
+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    +  Se ha cambiado la salida de la consola al enviar una ejecución de AutoML para mostrar un vínculo del portal a la ejecución.
  + **azureml-core**
    + Empieza a admitir la actualización del registro de contenedor para el área de trabajo en el SDK y la CLI.
    + Los atributos "enabled", "shared_volume" y "arguments" de DockerSection se han puesto en desuso en favor del uso de DockerConfiguration con ScriptRunConfig.
    + Documentación actualizada de clonación de la CLI de canalización
    + Se han actualizado los URI del portal para incluir el inquilino para la autenticación.
    + Se ha quitado el nombre del experimento de los URI de ejecución para evitar redireccionamientos.
    + Se ha actualizado el URI del experimento para usar el identificador del experimento.
    + Correcciones de errores para la asociación del proceso remoto mediante la CLI de Azure
    + Se han actualizado los URI del portal para incluir el inquilino para la autenticación.
    + Se ha agregado compatibilidad para comprender las particiones del conjunto de datos de archivo basadas en una estructura global.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret para usar interpret-community 0.17.0.
  + **azureml-opendatasets**
    + Especifique la validación del tipo de fecha de inicio y fecha de finalización y la indicación de error si no es del tipo datetime.
  + **azureml-pipeline-core**
    + Corrección de errores: compatibilidad con path_on_compute al pasar la configuración del conjunto de datos como descarga.
  + **azureml-pipeline-steps**
    + Corrección de errores: compatibilidad con path_on_compute al pasar la configuración del conjunto de datos como descarga.
    + Se ha puesto en desuso RScriptStep en favor del uso de CommandStep para ejecutar scripts de R en canalizaciones. 
    + Se ha puesto en desuso EstimatorStep en favor del uso de CommandStep para ejecutar el entrenamiento de ML (incluido el entrenamiento distribuido) en las canalizaciones.
  + **azureml-train-automl-runtime**
    +  Se ha cambiado la salida de la consola al enviar una ejecución de AutoML para mostrar un vínculo del portal a la ejecución.
  + **azureml-train-core**
    + Los atributos "enabled", "shared_volume" y "arguments" de DockerSection se han puesto en desuso en favor del uso de DockerConfiguration con ScriptRunConfig.
    + Uso de Azure Open Datasets para el conjunto de datos de MNIST
    + Se han actualizado los mensajes de error de Hyperdrive.


## <a name="2021-03-31"></a>31-03-2021
### <a name="azure-machine-learning-studio-notebooks-experience-march-update"></a>Experiencia con Cuadernos de Azure Machine Learning Studio (actualización de marzo)
+ **Nuevas características:**
  + Representación de CSV/TSV. Los usuarios podrán representar un archivo TSV/CSV en formato de cuadrícula para facilitar el análisis de datos. 
  + Autenticación de SSO para la instancia de proceso. Ahora los usuarios pueden autenticar fácilmente cualquier instancia de proceso nueva directamente en la interfaz de usuario de Notebook, lo que facilita la autenticación y el uso de los SDK de Azure directamente en AzureML. 
  + Métricas de instancias de proceso. Los usuarios podrán ver las métricas del proceso, como el uso de la CPU y la memoria, mediante el terminal.
  + Detalles de los archivos. Ahora los usuarios pueden hacer clic en los tres puntos situados junto a un archivo para ver sus detalles, incluidos la hora de la última modificación y el tamaño del archivo.

+ **Mejoras y correcciones de errores**
  + Tiempos de carga de páginas mejorados.
  + Rendimiento mejorado.
  + Confiabilidad de kernel y velocidad mejoradas.
  + Mueva de manera permanente el panel de archivos de Notebook hacia arriba para obtener espacio vertical.
  + Ahora se puede hacer clic en los vínculos en el terminal.
  + Rendimiento de IntelliSense mejorado.


## <a name="2021-03-08"></a>2021-03-08

### <a name="azure-machine-learning-sdk-for-python-v1240"></a>SDK de Azure Machine Learning para Python v1.24.0
+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se han quitado las importaciones compatibles con versiones anteriores de `azureml.automl.core.shared`. Los errores de módulo no encontrado en el espacio de nombres `azureml.automl.core.shared` se pueden resolver mediante su importación desde `azureml.automl.runtime.shared`.
  + **azureml-contrib-automl-dnn-vision**
    + Modelo Yolo de detección de objetos expuestos.
  + **azureml-contrib-dataset**
    + Se ha agregado funcionalidad para filtrar los conjuntos de datos tabulares por valores de columna y los conjuntos de datos de archivo por metadatos.
  + **azureml-contrib-fairness**
    + Incluir esquema JSON en el elemento wheel para `azureml-contrib-fairness`
  + **azureml-contrib-mir**
    + Con la configuración show_output establecida en True cuando se implementan modelos, la configuración de la implementación y la configuración de inferencia se reproducirán antes de enviar la solicitud al servidor.
  + **azureml-core**
    + Se ha agregado funcionalidad para filtrar los conjuntos de datos tabulares por valores de columna y los conjuntos de datos de archivo por metadatos.
    + Anteriormente, era posible que los usuarios crearan configuraciones de aprovisionamiento para destinos de proceso que no cumplieran los requisitos de seguridad de contraseñas para el campo `admin_user_password` (es decir, que deben contener al menos 3 de los siguientes elementos: 1 letra minúscula, 1 letra mayúscula, 1 dígito y 1 carácter especial del conjunto siguiente: ``\`~!@#$%^&*()=+_[]{}|;:./'",<>?``). Si el usuario ha creado una configuración con una contraseña débil y ha ejecutado un trabajo con esa configuración, el trabajo produciría un error en tiempo de ejecución. Ahora, la llamada a `AmlCompute.provisioning_configuration` producirá una excepción `ComputeTargetException` junto con un mensaje de error que explica los requisitos de seguridad de la contraseña. 
    + Además, en algunos casos también era posible especificar una configuración con un número negativo de nodos máximos. Ya no es posible hacerlo. Ahora, `AmlCompute.provisioning_configuration` producirá una excepción `ComputeTargetException` si el argumento `max_nodes` es un entero negativo.
    + Con la configuración show_output establecida en True cuando se implementan modelos, se mostrarán la configuración de la implementación y la configuración de inferencia.
    + Con la configuración show_output establecida en True cuando se espera a que finalice la implementación del modelo, se mostrará el progreso de la operación de implementación.
    + Se permite al cliente especificar el directorio de configuración de autenticación de AzureML mediante la variable de entorno AZUREML_AUTH_CONFIG_DIR.
    + Anteriormente, era posible crear una configuración de aprovisionamiento con un número mínimo de nodos menor que el número máximo de nodos. El trabajo se ejecutaría pero se produciría un error en tiempo de ejecución. Este error se ha corregido. Si ahora intenta crear una configuración de aprovisionamiento con `min_nodes < max_nodes`, el SDK generará una excepción `ComputeTargetException`.
  + **azureml-interpret**
    + Se ha corregido el panel de explicaciones, que no mostraba la importancia de las características agregadas para las explicaciones de ingeniería dispersa.
    + Se ha optimizado el uso de memoria de ExplanationClient en el paquete azureml-interpret.
  + **azureml-train-automl-client**
    +  Se ha corregido show_output=False para devolver el control al usuario cuando se ejecuta con Spark.

## <a name="2021-02-28"></a>2021-02-28
### <a name="azure-machine-learning-studio-notebooks-experience-february-update"></a>Experiencia con cuadernos de Estudio de Azure Machine Learning (actualización de febrero)
+ **Nuevas características:**
  + [Terminal nativo (GA)](./how-to-access-terminal.md). Los usuarios ahora podrán acceder a un terminal integrado, así como a la operación de Git, por medio del terminal integrado.
  + Fragmentos de código de Notebook (versión preliminar). Ahora tiene disponibles a su alcance extractos de código comunes de Azure ML. Vaya al panel de fragmentos de código, accesible mediante la barra de herramientas, o active el menú de fragmentos en el código con CTRL + barra espaciadora.  
  + [Métodos abreviados de teclado](./how-to-run-jupyter-notebooks.md#useful-keyboard-shortcuts). Paridad completa con los métodos abreviados de teclado disponibles en Jupyter. 
  + Indicar parámetros de celda. Muestra a los usuarios qué celdas de un cuaderno son celdas de parámetros y pueden ejecutar cuadernos con parámetros mediante [Papermill](https://github.com/nteract/papermill) en la instancia de proceso.
  + Administrador de sesiones de terminal y kernel: los usuarios podrán administrar todas las sesiones de terminal y kernel que se ejecutan en la instancia de proceso.
  + Botón Compartir. Los usuarios ahora pueden compartir cualquier archivo en el explorador de archivos del cuaderno mediante un clic con el botón derecho en el archivo y el botón Compartir.


+ **Mejoras y correcciones de errores**
  + Se han mejorado los tiempos de carga de las páginas.
  + rendimiento mejorado. 
  + Se han mejorado la velocidad y la confiabilidad del kernel.
  + Se ha agregado una rueda giratoria para mostrar el progreso de todas las [operaciones de la instancia de proceso](./how-to-run-jupyter-notebooks.md#status-indicators) en curso.
  + Clic con el botón derecho en el explorador de archivos. Al hacer clic con el botón derecho en cualquier archivo, se abrirán las operaciones de archivos. 


## <a name="2021-02-16"></a>16-02-2021

### <a name="azure-machine-learning-sdk-for-python-v1230"></a>SDK de Azure Machine Learning para Python v1.23.0
+ **Mejoras y correcciones de errores**
  + **azureml-core**
    + [Característica experimental] Se ha agregado compatibilidad para vincular el área de trabajo de Synapse a AML como un servicio vinculado.
    + [Característica experimental] Se ha agregado compatibilidad para adjuntar el grupo de Spark de Synapse a AML como una instancia de proceso.
    + [Característica experimental] Se ha agregado compatibilidad para el acceso a datos basado en identidades. Los usuarios pueden registrar almacenes de datos y conjuntos de datos sin proporcionar credenciales. En tal caso, se usarán para la autenticación el token de AAD de los usuarios o la identidad administrada del destino de proceso. Obtenga más información [aquí](./how-to-identity-based-data-access.md).
  + **azureml-pipeline-steps**
    + [Característica experimental] Se ha agregado compatibilidad para [SynapseSparkStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.synapsesparkstep).
  + **azureml-synapse**
    + [Característica experimental] Se ha agregado compatibilidad con Spark Magic para ejecutar una sesión interactiva en el grupo de Spark de Synapse.
+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + En esta actualización, hemos agregado el suavizado exponencial Holt-Winters al cuadro de herramientas de previsión del SDK de AutoML. Dada una serie temporal, se selecciona el mejor modelo mediante [AICc (criterio de información de Akaike corregido)](https://otexts.com/fpp3/selecting-predictors.html#selecting-predictors) y se devuelve.
    + AutoML generará ahora dos archivos de registro en lugar de uno. Las instrucciones de registro irán a uno u otro en función del proceso en el que se generó la instrucción de registro.
    + Quite cualquier predicción innecesaria de la muestra durante el entrenamiento del modelo con validaciones cruzadas. Esto puede reducir el tiempo de entrenamiento del modelo en algunos casos, especialmente en el caso de los modelos de previsión de series temporales.
  + **azureml-contrib-fairness**
    + Se ha agregado un esquema JSON para las cargas de dashboardDictionary.
  + **azureml-contrib-interpret**
    + Se ha actualizado el archivo Léame de azureml-contrib-interpret para reflejar que el paquete se quitará en la siguiente actualización, después de entrar en desuso desde octubre; use el paquete azureml-interpret en su lugar.
  + **azureml-core**
    + Anteriormente, era posible crear una configuración de aprovisionamiento con un número mínimo de nodos menor que el número máximo de nodos. Esto ya se ha corregido. Si ahora intenta crear una configuración de aprovisionamiento con `min_nodes < max_nodes`, el SDK generará una excepción `ComputeTargetException`.
    +  Se ha corregido el error en wait_for_completion en AmlCompute que hacía que la función devolviera el flujo de control antes de que la operación se completara realmente.
    + Run.fail() ahora está en desuso; use Run.tag() para marcar la ejecución como con errores o use Run.cancel() para marcar la ejecución como cancelada.
    + Se muestra el mensaje de error "Se esperaba una cadena para el nombre del entorno, se encontró {}" cuando el nombre del entorno proporcionado no es una cadena.
  + **azureml-train-automl-client**
    + Se ha corregido un error que impedía la cancelación de los experimentos de AutoML realizados en clústeres de Azure Databricks.


## <a name="2021-02-09"></a>09-02-2021

### <a name="azure-machine-learning-sdk-for-python-v1220"></a>SDK de Azure Machine Learning para Python v1.22.0
+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se corrigió un error en el que se agregó una dependencia PIP adicional al archivo YML de Conda para los modelos de visión.
  + **azureml-automl-runtime**
    + Se corrigió un error en el que los modelos de previsión clásica (por ejemplo, AutoArima) podían recibir datos de entrenamiento en los que las filas con valores de destino imputados no estaban presentes. Este infringía el contrato de datos de estos modelos. * Se han corregido varios errores con el comportamiento de retardo por repetición en el operador de retardo de serie temporal. Anteriormente, la operación de retardo por repetición no marcaba todas las filas imputadas correctamente y, por tanto, no siempre generaba los valores de retardo de repetición correctos. También se han corregido algunos problemas de compatibilidad entre el operador de retardo y el operador de ventana con desplazamiento con el comportamiento de retraso por repetición. Anteriormente, provocada que el operador de ventana con desplazamiento quitara algunas filas de los datos de entrenamiento que de otro modo debería usar.
  + **azureml-core**
    + Adición de compatibilidad para la autenticación de tokens por público.
    + Agregue `process_count` a [PyTorchConfiguration](/python/api/azureml-core/azureml.core.runconfig.pytorchconfiguration) para admitir trabajos multiproceso de PyTorch de varios procesos.
  + **azureml-pipeline-steps**
    + [CommandStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.commandstep) ahora está disponible de forma general y ya no es experimental.
    + [ParallelRunConfig](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.parallelrunconfig): agregue los argumentos allowed_failed_count y allowed_failed_percent para comprobar el umbral de errores en el nivel de minilote. El umbral de error ahora tiene tres tipos:
       + error_threshold: número de elementos de minilote con errores permitidos. 
       + allowed_failed_count: número de minilotes con errores permitidos. 
       + allowed_failed_percent: porcentaje de minilotes con errores permitidos. 
       
       Un trabajo se detendrá si supera cualquiera de estos valores. error_threshold es necesario para mantener la compatibilidad con versiones anteriores. Establezca el valor en -1 para ignorarlo.
    + Control de espacio en blanco fijo en el nombre de AutoMLStep.
    + HyperDriveStep ahora admite ScriptRunConfig
  + **azureml-train-core**
    + Las ejecuciones de HyperDrive que se invoquen desde un ScriptRun ahora se considerarán una ejecución secundaria.
    + Agregue `process_count` a [PyTorchConfiguration](/python/api/azureml-core/azureml.core.runconfig.pytorchconfiguration) para admitir trabajos multiproceso de PyTorch de varios procesos.
  + **azureml-widgets**
    + Agregue el widget ParallelRunStepDetails para visualizar el estado de una clase ParallelRunStep.
    + Permite a los usuarios de hyperdrive ver un eje adicional en el gráfico de coordenadas paralelas que muestra el valor de métrica correspondiente a cada conjunto de hiperparámetros de cada ejecución secundaria.


 ## <a name="2021-01-31"></a>31-01-2021
### <a name="azure-machine-learning-studio-notebooks-experience-january-update"></a>Experiencia con Notebooks de Azure Machine Learning Studio (actualización de enero)
+ **Nuevas características:**
  + Markdown editor nativo en AzureML. Los usuarios ahora pueden representar y editar archivos de Markdown de forma nativa en AzureML Studio.
  + [Botón Ejecutar para scripts (.py, .R y .sh)](./how-to-run-jupyter-notebooks.md#run-a-notebook-or-python-script). Los usuarios ahora pueden ejecutar fácilmente el script de Python, R y Bash en AzureML.
  + [Explorador de variables](./how-to-run-jupyter-notebooks.md#explore-variables-in-the-notebook). Explore el contenido de las variables y las tramas de datos en un panel emergente. Los usuarios pueden comprobar fácilmente el tipo de datos, el tamaño y el contenido.
  + [Tabla de contenido](./how-to-run-jupyter-notebooks.md#navigate-with-a-toc). Vaya a las secciones del cuaderno que indican los encabezados de Markdown.
  + Exporte el cuaderno como Latex/HTML/Py. Cree archivos de cuaderno fáciles de compartir mediante la exportación a LaTex, HTML o .py
  + IntelliCode. Los resultados con tecnología de ML proporcionan una [experiencia inteligente para autocompletar contenido](/visualstudio/intellicode/overview) mejorada.

+ **Mejoras y correcciones de errores**
  + Se han mejorado los tiempos de carga de las páginas.
  + rendimiento mejorado. 
  + Se han mejorado la velocidad y la confiabilidad del kernel.
  

 ## <a name="2021-01-25"></a>25-01-2021

### <a name="azure-machine-learning-sdk-for-python-v1210"></a>SDK de Azure Machine Learning para Python v1.21.0
+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se ha corregido el texto de ayuda de la CLI al usar AmlCompute con la identidad UserAssigned.
  + **azureml-contrib-automl-dnn-vision**
    + Los botones de implementación y descarga estarán visibles para las ejecuciones de AutoML Vision y los modelos se pueden implementar o descargar de forma similar a otras ejecuciones de AutoML. Hay dos nuevos archivos (scoring_file_v_1_0_0.py y conda_env_v_1_0_0.yml) que contienen un script para ejecutar la inferencia y un archivo yml para volver a crear el entorno de Conda. También se ha cambiado el nombre del archivo "model.pth" para que use la extensión ".pt".
  + **azureml-core**
    + Compatibilidad de MSI con azure-cli-ml
    + Compatibilidad de Identidad administrada asignada por el usuario.
    + Con este cambio, los clientes deben ser capaces de proporcionar una identidad asignada por el usuario que se puede usar para capturar la clave del almacén de claves del cliente para el cifrado en reposo.
    +  Se ha corregido row_count=0 para el perfil de archivos muy grandes. Se ha corregido el error en la conversión doble para valores delimitados con relleno de espacio en blanco.
    + Se ha quitado la marca experimental para la disponibilidad general del conjunto de datos de salida.
    + Se ha actualizado la documentación sobre cómo capturar una versión específica de un modelo.
    + Se ha permitido actualizar el área de trabajo para el acceso al modo mixto en caso de vínculo privado.
    + Corrección para quitar el registro adicional en la característica del almacén de datos para reanudar la ejecución.
    + Se ha agregado compatibilidad con la CLI o el SDK para actualizar la identidad asignada por el usuario principal del área de trabajo.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret a interpret-community 0.16.0.
    + Optimizaciones de memoria para el cliente de explicación en azureml-interpret.
  + **azureml-train-automl-runtime**
    + Se ha habilitado el streaming para ejecuciones de ADB.
  + **azureml-train-core**
    + Corrección para quitar el registro adicional en la característica del almacén de datos para reanudar la ejecución.
  + **azureml-widgets**
    + Los clientes no deben ver los cambios en la visualización de datos de ejecución existente mediante el widget y ahora tendrán soporte técnico si, opcionalmente, usan hiperparámetros condicionales.
    + Ahora, el widget de ejecución de usuarios incluye una explicación detallada de por qué una ejecución está en el estado En cola.


 ## <a name="2021-01-11"></a>2021-01-11

### <a name="azure-machine-learning-sdk-for-python-v1200"></a>SDK de Azure Machine Learning para Python v1.20.0
+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se agregó framework_version en OptimizationConfig. Se usará cuando el modelo se registre con el marco MULTI.
  + **azureml-contrib-optimization**
    + Se agregó framework_version en OptimizationConfig. Se usará cuando el modelo se registre con el marco MULTI.
  + **azureml-pipeline-steps**
    + Se incluye CommandStep, que toma un comando para procesar. El comando puede incluir ejecutables, comandos de Shell, scripts, etc.
  + **azureml-core**
    + Ahora la creación del área de trabajo admite las identidades asignadas por el usuario. Se agregó compatibilidad con UAI desde SDK/CLI
    + Se corrigió un problema en service.reload() para recoger los cambios de score.py en la implementación local.
    + `run.get_details()` tiene un campo adicional denominado "submittedBy" que muestra el nombre del autor para esta ejecución.
    + Se editó la documentación del método Model.register para mencionar cómo registrar el modelo desde la ejecución directa
    + Se ha corregido el problema de control de cambios de estado de conexión de IoT-Server.
   

## <a name="2020-12-31"></a>31/12/2020
### <a name="azure-machine-learning-studio-notebooks-experience-december-update"></a>Experiencia con los cuadernos de Estudio de Azure Machine Learning (actualización de diciembre)
+ **Nuevas características:**
  + Búsqueda del nombre de archivo del usuario. Los usuarios ahora pueden buscar en todos los archivos que están guardados en un área de trabajo.
  + Compatibilidad en paralelo con Markdown por celda del cuaderno. En una celda del cuaderno, los usuarios ahora tienen la opción de ver en paralelo la representación del archivo Markdown y la sintaxis de Markdown.
  + Barra de estado de la celda. La barra de estado indica en qué estado se encuentra una celda de código, si la ejecución de una celda fue correcta y cuánto tiempo tardó en ejecutarse. 
   
+ **Mejoras y correcciones de errores**
  + Se han mejorado los tiempos de carga de las páginas.
  + rendimiento mejorado. 
  + Se han mejorado la velocidad y la confiabilidad del kernel.

  
## <a name="2020-12-07"></a>2020-12-07

### <a name="azure-machine-learning-sdk-for-python-v1190"></a>SDK de Azure Machine Learning para Python v1.19.0
+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se agregó compatibilidad experimental para los datos de prueba a AutoMLStep.
    + Se agregó la implementación básica inicial de la característica de ingestión del conjunto de prueba.
    + Se han pasado referencias a sklearn.externals.joblib para depender directamente de joblib.
    + presente un nuevo tipo de tarea AutoML de "image-instance-segmentation".
  + **azureml-automl-runtime**
    + Se agregó la implementación básica inicial de la característica de ingestión del conjunto de prueba.
    + Cuando todas las cadenas de una columna de texto tienen una longitud de exactamente 1 carácter, el caracterizador TfIdf word-gram no funcionará porque su tokenizador omite las cadenas con menos de 2 caracteres. El cambio de código actual permitirá a AutoML controlar este caso de uso.
    + presente un nuevo tipo de tarea AutoML de "image-instance-segmentation".
  + **azureml-contrib-automl-dnn-nlp**
    + PR inicial para el nuevo paquete dnn-nlp
  + **azureml-contrib-automl-dnn-vision**
    + presente un nuevo tipo de tarea AutoML de "image-instance-segmentation".
  + **azureml-contrib-automl-pipeline-steps**
    + Este nuevo paquete es responsable de crear los pasos necesarios en muchos escenarios de inferencia o entrenamiento de modelos. -También traslada el código de inferencia o entrenamiento en el paquete azureml.train.automl.runtime, de modo que las correcciones futuras estarían disponibles automáticamente a través de las versiones de entorno mantenido.
  + **azureml-contrib-dataset**
    + presente un nuevo tipo de tarea AutoML de "image-instance-segmentation".
  + **azureml-core**
    + Se agregó la implementación básica inicial de la característica de ingestión del conjunto de prueba.
    + Corrección de las advertencias xref para la documentación del paquete azureml-core
    + Correcciones de cadena de documentación para la característica de compatibilidad con comandos en SDK
    + Incorporación de una propiedad de comando a RunConfiguration. La característica permite a los usuarios ejecutar un comando real o archivos ejecutables en el proceso a través del SDK de AzureML.
    + Los usuarios pueden eliminar un experimento vacío dado el identificador de ese experimento.
  + **azureml-dataprep**
    + Se ha agregado compatibilidad del conjunto de datos para Spark creada con Scala 2.12. Se agrega a la compatibilidad con 2.11 existente.
  + **azureml-mlflow**
    + AzureML-MLflow agrega medidas de seguridad en scripts remotos para evitar la finalización temprana de las ejecuciones enviadas.
  + **azureml-pipeline-core**
    + Se ha corregido un error en el establecimiento de una canalización predeterminada para el punto de conexión de canalización creado a través de la interfaz de usuario
  + **azureml-pipeline-steps**
    + Se agregó compatibilidad experimental para los datos de prueba a AutoMLStep.
  + **azureml-tensorboard**
    + Corrección de las advertencias xref para la documentación del paquete azureml-core
  + **azureml-train-automl-client**
    + Se agregó compatibilidad experimental para los datos de prueba a AutoMLStep.
    + Se agregó la implementación básica inicial de la característica de ingestión del conjunto de prueba.
    + presente un nuevo tipo de tarea AutoML de "image-instance-segmentation".
  + **azureml-train-automl-runtime**
    + Se agregó la implementación básica inicial de la característica de ingestión del conjunto de prueba.
    + Corrija el cálculo de las explicaciones básicas del mejor modelo de AutoML si los modelos de AutoML se entrenan mediante la configuración de validation_size.
    + Se han pasado referencias a sklearn.externals.joblib para depender directamente de joblib.
  + **azureml-train-core**
    + HyperDriveRun.get_children_sorted_by_primary_metric() should complete faster now
    + Control de errores mejorado en el SDK de HyperDrive.
    +  Se han dejado de utilizar todas las clases Estimator a favor del uso de ScriptRunConfig para configurar ejecuciones de experimento. Entre las clases en desuso se incluyen:
        + MMLBase
        + Estimador
        + PyTorch 
        + TensorFlow 
        + Chainer 
        + SKLearn
    + Se han dejado de utilizar Nccl y Gloo como tipos de entrada válidos para las clases Estimator a favor del uso de PyTorchConfiguration con ScriptRunConfig.
    + Se ha dejado de utilizar Mpi como tipo de entrada válido para las clases Estimator a favor del uso de MpiConfiguration con ScriptRunConfig.
    + Incorporación de una propiedad de comando a RunConfiguration. La característica permite a los usuarios ejecutar un comando real o archivos ejecutables en el proceso a través del SDK de AzureML.

    +  Se han dejado de utilizar todas las clases Estimator a favor del uso de ScriptRunConfig para configurar ejecuciones de experimento. Entre las clases en desuso se incluyen: + MMLBaseEstimator + Estimator + PyTorch + TensorFlow + Chainer + SKLearn
    + Se han dejado de utilizar Nccl y Gloo como tipo de entrada válido para las clases Estimator a favor del uso PyTorchConfiguration con ScriptRunConfig. 
    + Se ha dejado de utilizar Mpi como tipo de entrada válido para las clases Estimator a favor del uso de MpiConfiguration con ScriptRunConfig.

## <a name="2020-11-30"></a>30/11/2020
### <a name="azure-machine-learning-studio-notebooks-experience-november-update"></a>Experiencia con los cuadernos de Estudio de Azure Machine Learning (actualización de noviembre)
+ **Nuevas características:**
   + Terminal nativo. Los usuarios ahora podrán acceder a un terminal integrado, así como a la operación de Git, por medio del [terminal integrado](./how-to-access-terminal.md).
  + Carpeta duplicada. 
  + Desplegable de costos de procesos. 
  + Pylance para procesos sin conexión. 

+ **Mejoras y correcciones de errores**
  + Se han mejorado los tiempos de carga de las páginas.
  + rendimiento mejorado. 
  + Se han mejorado la velocidad y la confiabilidad del kernel.
  + Carga de archivos grandes. Ahora puede cargar archivos de más de 95 MB.

## <a name="2020-11-09"></a>09-11-2020

### <a name="azure-machine-learning-sdk-for-python-v1180"></a>SDK de Azure Machine Learning para Python v1.18.0
+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    +  Se ha mejorado el control de las series temporales breves al permitir su relleno con ruido gaussiano.
  + **azureml-automl-runtime**
    + Se lanza ConfigException si una columna DateTime tiene un valor OutOfBoundsDatetime.
    + Se ha mejorado el control de las series temporales breves al permitir su relleno con ruido gaussiano.
    + Se asegura que cada columna de texto pueda aprovechar la transformación de char-gram con el intervalo de n-gram en función de la longitud de las cadenas de esa columna de texto.
    + Se proporcionan explicaciones de características sin procesar para el mejor modo para los experimentos de AutoML que se ejecutan en el proceso local del usuario.
  + **azureml-core**
    + Anclar el paquete: pyjwt para evitar la extracción de versiones importantes en las próximas versiones.
    + Al crear un experimento, se devolverá el experimento activo o último archivado con el mismo nombre dado si existe dicho experimento o un nuevo experimento.
    + La llamada a get_experiment por nombre devolverá el experimento activo o último archivado con el nombre especificado.
    + Los usuarios no pueden cambiar el nombre de un experimento mientras lo reactivan.
    + Se ha mejorado el mensaje de error para incluir posibles correcciones cuando un conjunto de datos se pasa incorrectamente a un experimento (por ejemplo, ScriptRunConfig). 
    + Se ha mejorado la documentación de `OutputDatasetConfig.register_on_complete` para incluir el comportamiento de lo que ocurrirá cuando el nombre ya exista.
    + Especificar nombres de entrada y salida del conjunto de datos que tienen el potencial de colisión con variables de entorno comunes producirá ahora una advertencia.
    + Se ha readaptado el parámetro `grant_workspace_access` al registrar los almacenes de datos. Establézcalo en `True` para acceder a los datos subyacentes a la red virtual desde Machine Learning Studio.
      [Más información](./how-to-enable-studio-virtual-network.md)
    + Se ha refinado la API del servicio vinculado. En lugar de proporcionar el identificador del recurso, tenemos tres parámetros independientes, sub_id, rg y name, definidos en la configuración.
    + Para permitir que los clientes resuelvan automáticamente los problemas de daños en los tokens, habilite la sincronización de tokens del área de trabajo para que sea un método público.
    + Este cambio permite usar una cadena vacía como valor de script_param.
  + **azureml-train-automl-client**
    +  Se ha mejorado el control de las series temporales breves al permitir su relleno con ruido gaussiano.
  + **azureml-train-automl-runtime**
    + Se lanza ConfigException si una columna DateTime tiene un valor OutOfBoundsDatetime.
    + Se ha agregado compatibilidad para proporcionar explicaciones de características sin procesar para el mejor modelo para los experimentos de AutoML que se ejecutan en el proceso local del usuario.
    + Se ha mejorado el control de las series temporales breves al permitir su relleno con ruido gaussiano.
  + **azureml-train-core**
    + Este cambio permite usar una cadena vacía como valor de script_param.
  + **azureml-train-restclients-hyperdrive**
    + El archivo Léame ha cambiado para ofrecer más contexto.
  + **azureml-widgets**
    + Se ha agregado compatibilidad de cadena a la biblioteca de gráficos/coordenadas paralelas para widget.

## <a name="2020-11-05"></a>05-11-2020

### <a name="data-labeling-for-image-instance-segmentation-polygon-annotation-preview"></a>Etiquetado de datos para la segmentación de instancias de imagen (anotación de polígono) (versión preliminar)

Ahora está disponible el tipo de proyecto de segmentación de instancias de imagen (anotaciones de polígono) en el etiquetado de datos, de modo que los usuarios pueden dibujar y anotar polígonos alrededor del contorno de los objetos de las imágenes. Los usuarios podrán asignar una clase y un polígono a cada objeto que le interese dentro de una imagen.

Obtenga más información sobre el [etiquetado de segmentación de instancias de imagen](how-to-label-data.md).



## <a name="2020-10-26"></a>26-10-2020

### <a name="azure-machine-learning-sdk-for-python-v1170"></a>SDK de Azure Machine Learning para Python v1.17.0
+ **nuevos ejemplos**
  + Puede encontrar un repositorio de ejemplos administrado por la comunidad en https://github.com/Azure/azureml-examples.
+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se corrigió un problema por el que get_output puede generar un error de tipo XGBoostError.
  + **azureml-automl-runtime**
    + Ahora, las características basadas en el calendario y la hora que ha creado AutoML tendrán el prefijo.
    + Se ha corregido un error de tipo IndexError que se produce durante el entrenamiento de StackEnsemble para los conjuntos de valores de clasificación que cuentan con un gran número de clases y submuestras habilitadas.
    + Se corrigió un problema por el que las predicciones de VotingRegressor podrían ser imprecisas después de reajustar el modelo.
  + **azureml-core**
    + Se agregaron detalles adicionales sobre la relación entre la configuración de implementación de AKS y los conceptos de Azure Kubernetes Service.
    + Compatibilidad con etiquetas de cliente del entorno. El usuario puede etiquetar entornos y hacer referencia a ellos mediante etiquetas.
  + **azureml-dataprep**
    + Se ha mejorado el mensaje de error al usar una versión de Spark que no es compatible actualmente con Scala 2.12.
  + **azureml-explain-model**
    + El paquete azureml-explain-model está oficialmente en desuso.
  + **azureml-mlflow**
    + Se resolvió un error en mlflow.projects.run del back-end de azureml, donde el estado de finalización no se controlaba correctamente.
  + **azureml-pipeline-core**
    + Se agrega compatibilidad para crear, enumerar y obtener la programación de canalización basada en un punto de conexión de canalización.
    +  Se ha mejorado la documentación de PipelineData.as_dataset con un ejemplo de uso que no es válido. Si se usa PipelineData.as_dataset de forma incorrecta, se generará un mensaje de tipo ValueException.
    + Se ha cambiado el cuaderno de canalizaciones de HyperDriveStep para registrar el mejor modelo de PipelineStep directamente después de la ejecución de HyperDriveStep.
  + **azureml-pipeline-steps**
    + Se ha cambiado el cuaderno de canalizaciones de HyperDriveStep para registrar el mejor modelo de PipelineStep directamente después de la ejecución de HyperDriveStep.
  + **azureml-train-automl-client**
    + Se corrigió un problema por el que get_output puede generar un error de tipo XGBoostError.

### <a name="azure-machine-learning-studio-notebooks-experience-october-update"></a>Experiencia con Notebooks de Azure Machine Learning Studio (actualización de octubre)
+ **Nuevas características:**
  + [Compatibilidad completa con redes virtuales](./how-to-enable-studio-virtual-network.md)
  + [Modo de enfoque](./how-to-run-jupyter-notebooks.md#focus-mode)
  + Guardado de cuadernos Ctrl-S
  + Números de línea

+ **Mejoras y correcciones de errores**
  + Mejora de la velocidad y la confiabilidad del kernel
  + Actualizaciones de la interfaz de usuario de los widgets de Jupyter

## <a name="2020-10-12"></a>12-10-2020

### <a name="azure-machine-learning-sdk-for-python-v1160"></a>SDK de Azure Machine Learning para Python v1.16.0
+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + AKSWebservice y AKSEndpoints ahora admiten límites de recursos de CPU y memoria a nivel de pod. Estos límites opcionales se pueden usar al establecer las marcas `--cpu-cores-limit` y `--memory-gb-limit` en las llamadas de la CLI aplicables
  + **azureml-core**
    + Anclaje de las versiones principales de las dependencias directas de azureml-core.
    + AKSWebservice y AKSEndpoints ahora admiten límites de recursos de CPU y memoria a nivel de pod. Más información sobre los [límites y recursos de Kubernetes](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#requests-and-limits).
    + Se ha actualizado run.log_table para permitir el registro de filas individuales.
    + Se ha agregado un método estático `Run.get(workspace, run_id)` para recuperar una ejecución solo mediante un área de trabajo. 
    + Se ha agregado un método de instancia `Workspace.get_run(run_id)` para recuperar una ejecución dentro de un área de trabajo.
    + Introducción a la propiedad command en la configuración de ejecución que permitirá a los usuarios enviar el comando en lugar de los scripts y argumentos.
  + **azureml-interpret**
    + Se ha corregido el comportamiento de la marca is_raw del cliente de explicación en azureml-interpret.
  + **azureml-sdk**
    + `azureml-sdk` admite oficialmente Python 3.8.
  + **azureml-train-core**
    + Adición del entorno mantenido TensorFlow 2.3.
    + Introducción a la propiedad command en la configuración de ejecución que permitirá a los usuarios enviar el comando en lugar de los scripts y argumentos.
  + **azureml-widgets**
    + Interfaz rediseñada para el widget de ejecución de scripts.


## <a name="2020-09-28"></a>28-09-2020

### <a name="azure-machine-learning-sdk-for-python-v1150"></a>SDK de Azure Machine Learning para Python v1.15.0
+ **Mejoras y correcciones de errores**
  + **azureml-contrib-interpret**
    + El explicador de LIME se ha movido del paquete azureml-contrib-interpret a interpret-community e image explainer se ha quitado del paquete azureml-contrib-interpret.
    + El panel de visualización se ha quitado del paquete azureml-contrib-interpret, el cliente de explicación se ha movido al paquete azureml-interpret y ha quedado en desuso en el paquete azureml-contrib-interpret, y se han actualizado los cuadernos para reflejar la API mejorada.
    + Se han corregido las descripciones del paquete pypi para azureml-interpret, azureml-explain-model, azureml-contrib-interpret y azureml-tensorboard.
  + **azureml-contrib-notebook**
    + Se ha anclado la dependencia nbcovert a < 6 para que Papermill 1.x siga funcionando.
  + **azureml-core**
    + Se han agregado parámetros al constructor TensorflowConfiguration y MpiConfiguration para permitir una inicialización más simplificada de los atributos de clase sin necesidad de que el usuario establezca cada atributo individual. Se ha agregado una clase PyTorchConfiguration para configurar trabajos distribuidos de PyTorch en ScriptRunConfig.
    + Se ha anclado la versión de azure-mgmt-resource para corregir el error de autenticación.
    + Se admite la implementación sin código de Triton.
    + Ahora se realiza un seguimiento de los directorios de salidas especificados en Run.start_logging() al usar la ejecución en escenarios interactivos. Los archivos sometidos a seguimiento son visibles en ML Studio tras una llamada a Run.complete().
    + Ahora se puede especificar la codificación de archivo durante la creación de un conjunto de datos con `Dataset.Tabular.from_delimited_files` y `Dataset.Tabular.from_json_lines_files` al pasar el argumento `encoding`. Las codificaciones admitidas son "utf8", "iso88591", "latin1","ascii","utf16", "utf32", "tf8bom" y "windows1252".
    + Se ha corregido un error que consiste en que un objeto de entorno no se pasa al constructor ScriptRunConfig.
    + Se ha actualizado Run.cancel() para permitir la cancelación de una ejecución local desde otra máquina.
  + **azureml-dataprep**
    +  Se han corregido problemas de tiempo de espera de montaje de conjunto de datos.
  + **azureml-explain-model**
    + Se han corregido las descripciones del paquete pypi para azureml-interpret, azureml-explain-model, azureml-contrib-interpret y azureml-tensorboard.
  + **azureml-interpret**
    + El panel de visualización se ha quitado del paquete azureml-contrib-interpret, el cliente de explicación se ha movido al paquete azureml-interpret y ha quedado en desuso en el paquete azureml-contrib-interpret, y se han actualizado los cuadernos para reflejar la API mejorada.
    + Se ha actualizado el paquete azureml-interpret para que dependa de interpret-community 0.15.0.
    + Se han corregido las descripciones del paquete pypi para azureml-interpret, azureml-explain-model, azureml-contrib-interpret y azureml-tensorboard.
  + **azureml-pipeline-core**
    +  Se ha corregido un problema de canalización con `OutputFileDatasetConfig` que consiste en que el sistema puede dejar de responder cuando se llama a `register_on_complete` con el parámetro `name` establecido en un nombre de conjunto de datos ya existente.
  + **azureml-pipeline-steps**
    + Se han quitado los cuadernos de Databricks obsoletos.
  + **azureml-tensorboard**
    + Se han corregido las descripciones del paquete pypi para azureml-interpret, azureml-explain-model, azureml-contrib-interpret y azureml-tensorboard.
  + **azureml-train-automl-runtime**
    + El panel de visualización se ha quitado del paquete azureml-contrib-interpret, el cliente de explicación se ha movido al paquete azureml-interpret y ha quedado en desuso en el paquete azureml-contrib-interpret, y se han actualizado los cuadernos para reflejar la API mejorada.
  + **azureml-widgets**
    + El panel de visualización se ha quitado del paquete azureml-contrib-interpret, el cliente de explicación se ha movido al paquete azureml-interpret y ha quedado en desuso en el paquete azureml-contrib-interpret, y se han actualizado los cuadernos para reflejar la API mejorada.

## <a name="2020-09-21"></a>2020-09-21

### <a name="azure-machine-learning-sdk-for-python-v1140"></a>SDK de Azure Machine Learning para Python v1.14.0
+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + La generación de perfiles de cuadrícula se quitó del SDK y ya no se admite.
  + **azureml-accel-models**
    + El paquete azureml-accel-models ahora admite TensorFlow 2.x.
  + **azureml-automl-core**
    + Se agregó el control de errores en get_output para casos en los que las versiones locales de pandas/sklearn no coinciden con las que se usan durante el entrenamiento.
  + **azureml-automl-runtime**
    + Se corrigió un error por el que se produciría un error en las iteraciones de AutoArima con PredictionException y el mensaje: "Silent failure occurred during prediction" (Error silencioso durante la predicción).
  + **azureml-cli-common**
    + La generación de perfiles de cuadrícula se quitó del SDK y ya no se admite.
  + **azureml-contrib-server**
    + Se actualizó la descripción del paquete para la página de información general PyPI.
  + **azureml-core**
    + La generación de perfiles de cuadrícula se quitó del SDK y ya no se admite.
    + Se redujo el número de mensajes de error cuando se produce un error al recuperar el área de trabajo.
    + No mostrar advertencia cuando se produce un error al capturar metadatos
    + Nuevo paso de Kusto y destino de proceso de Kusto.
    + Se actualizó el documento para el parámetro SKU. Se quitó la SKU en la funcionalidad de actualización del área de trabajo en la CLI y el SDK.
    + Se actualizó la descripción del paquete para la página de información general PyPI.
    + Se actualizó la documentación para entornos de AzureML.
    + Se expuso la configuración de recursos administrados del servicio para el área de trabajo AML en el SDK.
  + **azureml-dataprep**
    + Se habilitó el permiso de ejecución en los archivos para el montaje de DataSet.
  + **azureml-mlflow**
    + Se actualizaron la documentación y los ejemplos de cuaderno de MLflow para AzureML. 
    + Nueva compatibilidad de proyectos de MLflow con back-end de AzureML.
    + Compatibilidad del registro de modelos de MLflow.
    + Incorporación de compatibilidad con Azure RBAC para las operaciones de AzureML-MLflow 
    
  + **azureml-pipeline-core**
    + Se ha mejorado la documentación de los métodos PipelineOutputFileDataset.parse_*.
    + Nuevo paso de Kusto y destino de proceso de Kusto.
    + La propiedad Swaggerurl proporcionada para la entidad del punto de conexión de canalización a través de ese usuario puede ver la definición del esquema para el punto de conexión de canalización publicado.
  + **azureml-pipeline-steps**
    + Nuevo paso de Kusto y destino de proceso de Kusto.
  + **azureml-telemetry**
    + Se actualizó la descripción del paquete para la página de información general PyPI.
  + **azureml-train**
    + Se actualizó la descripción del paquete para la página de información general PyPI.
  + **azureml-train-automl-client**
    + Se agregó el control de errores en get_output para casos en los que las versiones locales de pandas/sklearn no coinciden con las que se usan durante el entrenamiento.
  + **azureml-train-core**
    + Se actualizó la descripción del paquete para la página de información general PyPI.
    
## <a name="2020-08-31"></a>31-08-2020

### <a name="azure-machine-learning-sdk-for-python-v1130"></a>SDK de Azure Machine Learning para Python v1.13.0
+ **Características en versión preliminar**
  + **azureml-core** Con la nueva funcionalidad de conjuntos de datos de salida, puede reescribir en el almacenamiento en la nube, incluido BLOB, ADLS Gen 1, ADLS Gen 2 y FileShare. Puede configurar dónde se envían los datos, cómo se devuelven los datos (montaje o carga), si se deben registrar los datos de salida para reutilizarlos y compartirlos posteriormente y pasar los datos intermedios entre los pasos de la canalización sin problemas. Esto habilita la reproducibilidad, el uso compartido, evita la duplicación de datos y mejora la productividad y la rentabilidad. [Aprenda a usarlo](/python/api/azureml-core/azureml.data.output_dataset_config.outputfiledatasetconfig)
    
+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se agregó el archivo validated_{platform}_requirements.txt para anclar todas las dependencias PIP para AutoML.
    + Esta versión admite modelos de más de 4 GB.
    + Dependencias de AutoML actualizadas: `scikit-learn` (ahora 0.22.1), `pandas` (ahora 0.25.1), `numpy` (ahora 1.18.2).
  + **azureml-automl-runtime**
    + Defina horovod para DNN de texto para que use siempre la compresión FP16.
    + Esta versión admite modelos de más de 4 GB.
    + Se corrigió un problema que provocaba un error de importación de AutoML: no se puede importar el nombre `RollingOriginValidator`.
    + Dependencias de AutoML actualizadas: `scikit-learn` (ahora 0.22.1), `pandas` (ahora 0.25.1), `numpy` (ahora 1.18.2).
  + **azureml-contrib-automl-dnn-forecasting**
    + Dependencias de AutoML actualizadas: `scikit-learn` (ahora 0.22.1), `pandas` (ahora 0.25.1), `numpy` (ahora 1.18.2).
  + **azureml-contrib-fairness**
    + Proporcione una breve descripción para azureml-contrib-fairness.
  + **azureml-contrib-pipeline-steps**
    + Se agregó un mensaje que indica que este paquete está en desuso y que el usuario debe usar azureml-pipeline-steps en su lugar.
  + **azureml-core**
    + Se ha agregado el comando de clave de lista para el área de trabajo.
    + Agregue el parámetro de etiquetas en la CLI y el SDK del área de trabajo.
    + Se corrigió el error que impedía enviar una ejecución secundaria con un conjunto de datos debido a `TypeError: can't pickle _thread.RLock objects`.
    + Se agregó un valor predeterminado y documentación de page_count para Model list().
    + Se modificó la CLI y el SDK para adoptar el parámetro adbworkspace y se agregó el ejecutor adb lin/unlink del área de trabajo.
    + Se corrigió un error en Dataset.update que provocaba que se actualizara la nueva versión del conjunto de datos, en lugar de la versión de la actualización del conjunto de datos llamada. 
    + Se corrigió un error en Dataset.get_by_name que mostraba las etiquetas de la versión más reciente del conjunto de datos incluso si se recuperaba una versión anterior concreta.
  + **azureml-interpret**
    + Se han agregado salidas de probabilidad a los explicadores de puntuación shap en azureml-interpret a partir del parámetro shap_values_output del explicador original.
  + **azureml-pipeline-core**
    + Documentación de `PipelineOutputAbstractDataset.register` mejorada.
  + **azureml-train-automl-client**
    + Dependencias de AutoML actualizadas: `scikit-learn` (ahora 0.22.1), `pandas` (ahora 0.25.1), `numpy` (ahora 1.18.2).
  + **azureml-train-automl-runtime**
    + Dependencias de AutoML actualizadas: `scikit-learn` (ahora 0.22.1), `pandas` (ahora 0.25.1), `numpy` (ahora 1.18.2).
  + **azureml-train-core**
    + Ahora, los usuarios deben proporcionar un argumento hyperparameter_sampling válido al crear un elemento HyperDriveConfig. Además, la documentación de HyperDriveRunConfig se ha editado para informar a los usuarios de la degradación de HyperDriveRunConfig.
    + Se ha revertido la versión predeterminada de PyTorch a 1.4.
    + Se ha agregado un entorno mantenido e imágenes de PyTorch 1.6 y TensorFlow 2.2.

### <a name="azure-machine-learning-studio-notebooks-experience-august-update"></a>Experiencia con Cuadernos de Azure Machine Learning Studio (actualización de agosto)
+ **Nuevas características:**
  + Nueva página de aterrizaje de introducción 
  
+ **Características en versión preliminar**
    + Característica de recopilación de los cuadernos. Con la característica [Recopilar](./how-to-run-jupyter-notebooks.md#clean-your-notebook-preview), los usuarios pueden limpiar fácilmente los cuadernos. Esta característica usa un análisis de dependencia automatizado del cuaderno y garantiza que el código esencial se conserve, pero se quitan las partes irrelevantes.

+ **Mejoras y correcciones de errores**
  + Mejora de la velocidad y la confiabilidad
  + Errores de modo oscuro corregidos
  + Errores de desplazamiento de salida corregidos
  + Ahora, la búsqueda de ejemplo busca en todo el contenido de todos los archivos del repositorio de cuadernos de muestra de Azure Machine Learning.
  + Ahora se pueden ejecutar celdas de R de varias líneas.
  + Ahora, la opción "Confío en el contenido de este archivo" se marca automáticamente después de la primera vez.
  + Cuadro de diálogo Resolución de conflictos mejorado con la opción "Crear una copia"
  
## <a name="2020-08-17"></a>17-08-2020

### <a name="azure-machine-learning-sdk-for-python-v1120"></a>SDK de Azure Machine Learning para Python v1.12.0

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se agregaron los parámetros image_name e image_label a Model.package() para habilitar el cambio de nombre de la imagen de paquete compilada.
  + **azureml-automl-core**
    + AutoML genera un nuevo código de error en la preparación de datos cuando se modifica el contenido mientras se lee.
  + **azureml-automl-runtime**
    + Se agregaron alertas para el usuario cuando los datos contienen valores que faltan, pero la caracterización está desactivada.
    + Se corrigieron errores de ejecución secundarios cuando los datos contienen NaN y la caracterización está desactivada.
    + AutoML genera un nuevo código de error en la preparación de datos cuando se modifica el contenido mientras se lee.
    + Se actualizó la normalización de las métricas de previsión que tienen lugar por grano.
    + Se mejoró el cálculo de cuantiles de previsión cuando se deshabilitan las características de lookback.
    + Se corrigió la administración de matrices dispersas booleanas al calcular las explicaciones después de AutoML.
  + **azureml-core**
    + Un nuevo método `run.get_detailed_status()` ahora muestra la explicación detallada del estado de ejecución actual. Actualmente solo se muestra la explicación del estado de `Queued`.
    + Se agregaron los parámetros image_name e image_label a Model.package() para habilitar el cambio de nombre de la imagen de paquete compilada.
    + Hay un método `set_pip_requirements()` nuevo para establecer la sección de PIP completa en [`CondaDependencies`](/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies) a la vez.
    + Se habilitó el registro del almacén datos ADLS Gen2 de credenciales.
    + Se mejoró el mensaje de error al intentar descargar o montar un tipo de conjunto de datos incorrecto.
    + Se actualizó el ejemplo de cuaderno del filtro de conjunto de datos de series temporales con más ejemplos de partition_timestamp que permite la optimización de los filtros.
    + Se cambia el SDK y la CLI para que acepten como parámetros a subscriptionId, resourceGroup, workspaceName, peConnectionName, en lugar de ArmResourceId al eliminar la conexión del punto de conexión privado.
    + Experimental Decorator muestra el nombre de clase para facilitar la identificación.
    + Las descripciones de los recursos que se encuentran dentro de los modelos ya no se generan automáticamente en función de una ejecución.
  + **azureml-datadrift**
    + Se marcó la API create_from_model en DataDriftDetector para indicar que está en desuso.
  + **azureml-dataprep**
    + Se mejoró el mensaje de error al intentar descargar o montar un tipo de conjunto de datos incorrecto.
  + **azureml-pipeline-core**
    + Se corrigió el error al deserializar el grafo de canalizaciones que contiene los conjuntos de datos registrados.
  + **azureml-pipeline-steps**
    + RScriptStep admite RSection desde azureml.core.environment.
    + Se quitó el parámetro passthru_automl_config de la API pública de `AutoMLStep` y se convirtió en un parámetro solamente interno.
  + **azureml-train-automl-client**
    + Se quitaron de AutoML las ejecuciones locales de entornos administrados asincrónicos. Todas las ejecuciones locales se ejecutarán en el entorno desde el que se iniciara la ejecución.
    + Se corrigieron problemas de instantáneas al enviar ejecuciones de AutoML sin scripts proporcionados por el usuario.
    + Se corrigieron errores de ejecución secundarios cuando los datos contienen NaN y la caracterización está desactivada.
  + **azureml-train-automl-runtime**
    + AutoML genera un nuevo código de error en la preparación de datos cuando se modifica el contenido mientras se lee.
    + Se corrigieron problemas de instantáneas al enviar ejecuciones de AutoML sin scripts proporcionados por el usuario.
    + Se corrigieron errores de ejecución secundarios cuando los datos contienen NaN y la caracterización está desactivada.
  + **azureml-train-core**
    + Se agregó compatibilidad para especificar opciones de PIP (por ejemplo,--extra-index-url) en el archivo de requisitos de PIP que se pasó a un [`Estimator`](/python/api/azureml-train-core/azureml.train.estimator.estimator) mediante el parámetro `pip_requirements_file`.


## <a name="2020-08-03"></a>03-08-2020

### <a name="azure-machine-learning-sdk-for-python-v1110"></a>SDK de Azure Machine Learning para Python v1.11.0

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Corrección de un error del marco del modelo y del error que provocaba que este no se pasara en el objeto de ejecución en la ruta de acceso de registro del modelo de la CLI.
    + Corrección del comando amlcompute identity show de la CLI para mostrar el id. de inquilino y el id. de entidad de seguridad. 
  + **azureml-train-automl-client**
    + Se agregó get_best_child () a AutoMLRun para capturar la mejor ejecución secundaria para una ejecución de AutoML sin descargar el modelo asociado.
    + Se agregó el objeto ModelProxy que permite la ejecución de predicción o previsión en un entorno de entrenamiento remoto sin descargar el modelo de forma local.
    + Las excepciones no controladas de AutoML ahora apuntan a una página HTTP de problemas conocidos que incluye más información sobre los errores.
  + **azureml-core**
    + Los nombres de modelo pueden tener hasta 255 caracteres.
    + El tipo de objeto devuelto de Environment.get_image_details() ha cambiado. La clase `DockerImageDetails` ha reemplazado a `dict`. Los detalles de la imagen están disponibles en las propiedades de la nueva clase. Los cambios son compatibles con versiones anteriores.
    + Se ha corregido un error de Environment.from_pip_requirements() para conservar la estructura de dependencias.
    + Se ha corregido un error de log_list que se producía al incluir un valor int y double en la misma lista.
    + Al habilitar el vínculo privado en un área de trabajo existente, tenga en cuenta que si hay destinos de proceso asociados con el área de trabajo, esos destinos no funcionarán si no están detrás de la misma red virtual que el punto de conexión privado del área de trabajo.
    + `as_named_input` se hizo opcional al usar conjuntos de datos en experimentos y se agregaron `as_mount` y `as_download` a `FileDataset`. El nombre de entrada se generará automáticamente si se llama a `as_mount` o `as_download`.
  + **azureml-automl-core**
    + Las excepciones no controladas de AutoML ahora apuntan a una página HTTP de problemas conocidos que incluye más información sobre los errores.
    + Se agregó get_best_child () a AutoMLRun para capturar la mejor ejecución secundaria para una ejecución de AutoML sin descargar el modelo asociado.
    + Se agregó el objeto ModelProxy que permite la ejecución de predicción o previsión en un entorno de entrenamiento remoto sin descargar el modelo de forma local.
  + **azureml-pipeline-steps**
    + Se agregaron las marcas `enable_default_model_output` y `enable_default_metrics_output` a `AutoMLStep`. Estas marcas se pueden usar para habilitar o deshabilitar las salidas predeterminadas.


## <a name="2020-07-20"></a>20-07-2020

### <a name="azure-machine-learning-sdk-for-python-v1100"></a>SDK de Azure Machine Learning para Python v1.10.0

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Al usar AutoML, si se pasa una ruta de acceso al objeto AutoMLConfig y aún no existe, se creará automáticamente.
    + Ahora, los usuarios pueden especificar una frecuencia de serie temporal para las tareas de previsión mediante el parámetro `freq`.
  + **azureml-automl-runtime**
    + Al usar AutoML, si se pasa una ruta de acceso al objeto AutoMLConfig y aún no existe, se creará automáticamente.
    + Ahora, los usuarios pueden especificar una frecuencia de serie temporal para las tareas de previsión mediante el parámetro `freq`.
    + La previsión de AutoML ahora admite la evaluación gradual, que se aplica al caso de uso de que la longitud de una prueba o un conjunto de validación sea mayor que el horizonte de entrada. El valor y_pred conocido se usa como contexto de previsión.
  + **azureml-core**
    + Se imprimirán mensajes de advertencia si no se descargó ningún archivo del almacén de datos en una ejecución.
    + Se agregó la documentación para `skip_validation` a `Datastore.register_azure_sql_database method`.
    + Los usuarios deben actualizar al SDK v1.10.0 o una versión posterior para crear un punto de conexión privado aprobado automáticamente. Esto incluye el recurso Notebook que se puede usar detrás de la red virtual.
    + Exponga NotebookInfo en la respuesta de get workspace.
    + Los cambios para que las llamadas muestren los destinos de proceso y para obtener el destino de proceso se completan correctamente en una ejecución remota. Ahora, las funciones del SDK para obtener el destino de proceso y mostrar los destinos de proceso del área de trabajo funcionarán en ejecuciones remotas.
    + Agregue mensajes de desuso a las descripciones de clase para las clases azureml.core.image.
    + Genere una excepción y limpie el área de trabajo y los recursos dependientes si se produce un error en la creación del punto de conexión privado.
    + Compatibilidad de la actualización de SKU del área de trabajo en el método de actualización del área de trabajo.
  + **azureml-datadrift**
    + Actualice la versión de matplotlib de 3.0.2 a 3.2.1 para que sea compatible con Python 3.8.
  + **azureml-dataprep**
    + Se ha agregado compatibilidad con los orígenes de datos de direcciones URL web con una solicitud `Range` o `Head`. 
    + Se ha mejorado la estabilidad para el montaje y la descarga del conjunto de datos de archivos.
  + **azureml-train-automl-client**
    + Se han corregido los problemas relacionados con la eliminación de `RequirementParseError` de setuptools.
    + Use Docker en lugar de Conda para las ejecuciones locales enviadas mediante "compute_target='local'".
    + Se ha corregido la duración de la iteración impresa en la consola. Anteriormente, la duración de la iteración se imprimía como la hora final de la ejecución menos su hora de creación. Se ha corregido para que sea igual a la hora final de ejecución menos la hora inicial.
    + Al usar AutoML, si se pasa una ruta de acceso al objeto AutoMLConfig y aún no existe, se creará automáticamente.
    + Ahora, los usuarios pueden especificar una frecuencia de serie temporal para las tareas de previsión mediante el parámetro `freq`.
  + **azureml-train-automl-runtime**
    + Se ha mejorado la salida de la consola cuando se produce un error en las explicaciones del mejor modelo.
    + Se ha cambiado el nombre del parámetro de entrada a "blocked_models" para quitar un término confidencial.
      + Se ha cambiado el nombre del parámetro de entrada a "allowed_models" para quitar un término confidencial.
    + Ahora, los usuarios pueden especificar una frecuencia de serie temporal para las tareas de previsión mediante el parámetro `freq`.

  
## <a name="2020-07-06"></a>06-07-2020

### <a name="azure-machine-learning-sdk-for-python-v190"></a>SDK de Azure Machine Learning para Python v 1.9.0

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se reemplazó get_model_path () por la variable de entorno AZUREML_MODEL_DIR en el script de puntuación generado automáticamente de AutoML. También se agregó la telemetría para realizar el seguimiento de los errores durante init().
    + Se quitó la capacidad de especificar `enable_cache` como parte de AutoMLConfig
    + Se corrigió un error en el que se puede producir un error en las ejecuciones con errores de servicio durante ejecuciones de previsión específicas
    + Control de errores mejorado en torno a modelos específicos durante `get_output`
    + Se corrigió la llamada a fitted_model.fit(X, y) para la clasificación con transformador y
    + Habilite el imputer de relleno directo personalizado para las tareas de pronóstico
    + Se usará una nueva clase ForecastingParameters en lugar de los parámetros de previsión en un formato dict
    + Detección automática de retraso de destino mejorada
    + Se agregó la disponibilidad limitada de características distribuidos de varios nodos y varias GPU con BERT
  + **azureml-automl-runtime**
    + Prophet ahora hace modelos de estacionalidad aditivos en lugar de multiplicativos.
    + Se corrigió el problema cuando los granos cortos, que tenían frecuencias diferentes a las de los granos largos, daban como resultado ejecuciones erróneas.
  + **azureml-contrib-automl-dnn-vision**
    + Recopilar estadísticas del sistema/GPU y medias de registro para el entrenamiento y la puntuación
  + **azureml-contrib-mir**
    + Se agregó compatibilidad con la marca enable-app-insights en ManagedInferencing
  + **azureml-core**
    + Un parámetro válido para estas API mediante la posibilidad de omitir la validación cuando no se puede tener acceso al origen de datos desde el proceso actual.
      + TabularDataset.time_before(end_time, include_boundary=True, validate=True)
      + TabularDataset.time_after(start_time, include_boundary=True, validate=True)
      + TabularDataset.time_recent(time_delta, include_boundary=True, validate=True)
      + TabularDataset.time_between(start_time, end_time, include_boundary=True, validate=True)
    + Se agregó compatibilidad con el filtrado del marco de trabajo para la lista de modelos y se agregó la muestra NCD AutoML en la parte posterior del cuaderno
    + En el caso de Store. Datastore.register_azure_blob_container y Datastore.register_azure_file_share (solo las opciones que admiten el token de SAS), actualizamos las cadenas de documento para el campo de `sas_token` modo que incluya los requisitos mínimos de permisos para escenarios típicos de lectura y escritura.
    + Deprecating _with_auth param in ws.get_mlflow_tracking_uri()
  + **azureml-mlflow**
    + Agregar compatibilidad para implementar modelos de file:// models with AzureML-MLflow
    + Deprecating _with_auth param in ws.get_mlflow_tracking_uri()
  + **azureml-opendatasets**
    + Los conjuntos de archivos de seguimiento de Covid-19 publicados recientemente ahora están disponibles con el SDK
  + **azureml-pipeline-core**
    + Advertencia de cierre de sesión cuando "azureml-defaults" no se incluye como parte de la dependencia de pip
    + Mejorar la representación de las Notas.
    + Se agregó compatibilidad con los saltos de línea entre comillas al analizar archivos delimitados en PipelineOutputFileDataset.
    + La clase PipelineDataset está en desuso. Para más información, consulte https://aka.ms/dataset-deprecation. Para aprender a usar el conjunto de datos con la canalización, consulte https://aka.ms/pipeline-with-dataset.
  + **azureml-pipeline-steps**
    + Documento actualizaciones de azureml-pipeline-steps.
    +  Se agregó soporte en ParallelRunConfig `load_yaml()` para que los usuarios definan entornos en línea con el resto de la configuración o en un archivo separado
  + **azureml-train-automl-client**.
    + Se quitó la capacidad de especificar `enable_cache` como parte de AutoMLConfig
  + **azureml-train-automl-runtime**
    + Se agregó la disponibilidad limitada de características distribuidos de varios nodos y varias GPU con BERT.
    + Se agregó el control de errores para los paquetes incompatibles en las ejecuciones automáticas de aprendizaje automático basado en ADB.
  + **azureml-widgets**
    + Actualizaciones de documentos para azureml-widgets.

  
## <a name="2020-06-22"></a>22-06-2020

### <a name="azure-machine-learning-sdk-for-python-v180"></a>SDK de Azure Machine Learning para Python v 1.8.0
  
  + **Características en versión preliminar**
    + **azureml-contrib-fairness** El paquete `azureml-contrib-fairness` proporciona integración entre la evaluación de equidad de código abierto y el paquete de mitigación de injusticia [Fairlearn](https://fairlearn.github.io) y el Azure Machine Learning Studio. En concreto, el paquete habilita los paneles de evaluación de equidad de modelo que se van a cargar como parte de una ejecución de AzureML y aparecen en Azure Machine Learning Studio

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Compatibilidad con la obtención de registros del contenedor de inicialización.
    + Se agregaron nuevos comandos de la CLI para administrar ComputeInstance
  + **azureml-automl-core**
    + Los usuarios ahora pueden habilitar la iteración del conjunto de pila para las tareas de los períodos de tiempo con una advertencia de que podría sobreajustar.
    + Se agregó un nuevo tipo de excepción de usuario, que se genera si se ha alterado el contenido del almacén de caché.
  + **azureml-automl-runtime**
    + El barrido de equilibrio de clases dejará de estar habilitado si el usuario deshabilita características.  
  + **azureml-contrib-notebook**
    + Mejoras de documentos en el paquete azureml-contrib-notebook.
  + **azureml-contrib-pipeline-steps**
    + Mejoras de documento en el paquete azureml-contrib--pipeline-steps.
  + **azureml-core**
    + Agregar funciones de set_connection, get_connection, list_connections, delete_connection para que el cliente opere en un recurso de conexión de área de trabajo
    + Actualizaciones de documentación del paquete azureml-coore/azureml.exceptions.
    + Actualizaciones de documentación para el paquete azureml-core.
    + Actualización de documento a la clase ComputeInstance.
    + Mejoras de documento en el azureml-core/azureml.core.compute.
    + Mejoras en los documentos para las clases relacionadas con WebService en azureml-core.
    + Admite el almacén de datos seleccionado por el usuario para almacenar datos de perfiles.
    + Se agregó la propiedad expandir y page_count para la API de la lista de modelos
    + Se corrigió un error en el que al quitar la propiedad sobrescribir se produciría un error de deserialización en la ejecución enviada.
    + Se corrigió una estructura de carpetas incoherente al descargar o montar un FileDataset que hace referencia a un único archivo.
    + Cargar un conjunto de datos de archivos de parquet a_spark_dataframe ahora es más rápido y admite todos los tipos de datos de parquet y Spark SQL.
    + Compatibilidad con la obtención de registros del contenedor de inicialización.
    + Las ejecuciones de AutoML ahora están marcadas como ejecución secundaria de paso de ejecución en paralelo.
  + **azureml-datadrift**
    + Mejoras de documentos en el paquete azureml-contrib-notebook.
  + **azureml-dataprep**
    + Cargar un conjunto de datos de archivos de parquet a_spark_dataframe ahora es más rápido y admite todos los tipos de datos de parquet y Spark SQL.
    + Mejor manejo de memoria para un problema de OutOfMemory para to_pandas_dataframe.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret para usar interpret-community versión 0.12.*
  + **azureml-mlflow**
    + Mejoras de documento en azureml-mlflow.
    + Agrega compatibilidad para el registro del modelo AML con MLFlow.
  + **azureml-opendatasets**
    + Se ha agregado compatibilidad para Python 3.8.
  + **azureml-pipeline-core**
    + Se ha actualizado la documentación de `PipelineDataset` para que quede claro que es una clase interna.
    + Actualizaciones de ParallelRunStep para aceptar varios valores para un argumento, por ejemplo: "--group_column_names", "Col1", "Col2", "Col3"
    + Se quitó el requisito de passthru_automl_config para el uso de datos intermedios con AutoMLStep en las canalizaciones.
  + **azureml-pipeline-steps**
    + Mejoras de documento en el paquete azureml-pipeline-steps.
    + Se quitó el requisito de passthru_automl_config para el uso de datos intermedios con AutoMLStep en las canalizaciones.
  + **azureml-telemetry**
    + Mejoras de documento en azureml-telemetry.
  + **azureml-train-automl-client**
    + Se corrigió un error donde `experiment.submit()` llamado dos veces en un objeto `AutoMLConfig` daba como resultado un comportamiento diferente.
    + Los usuarios ahora pueden habilitar la iteración del conjunto de pila para las tareas de los períodos de tiempo con una advertencia de que podría sobreajustar.
    + Se cambió el comportamiento de ejecución de AutoML para generar UserErrorException si el servicio produce un error de usuario
    + Corrige un error que hizo que azureml_automl.log no se generara o faltaran registros al realizar un experimento de AutoML en un destino de proceso remoto.
    + En el caso de los Conjuntos de datos de clasificación con clases desequilibradas, aplicaremos el equilibrio de peso, si el rastreador de características determina que para los datos submuestreados, el equilibrio de peso mejora el rendimiento de la tarea de clasificación en un determinado umbral.
    + Las ejecuciones de AutoML ahora están marcadas como ejecución secundaria de paso de ejecución en paralelo.
  + **azureml-train-automl-runtime**
    + Se cambió el comportamiento de ejecución de AutoML para generar UserErrorException si el servicio produce un error de usuario
    + Las ejecuciones de AutoML ahora están marcadas como ejecución secundaria de paso de ejecución en paralelo.

  
## <a name="2020-06-08"></a>08-06-2020

### <a name="azure-machine-learning-sdk-for-python-v170"></a>SDK de Azure Machine Learning para Python v 1.7.0

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se completó la eliminación de la generación de perfiles de modelos de mir contrib al limpiar los comandos y las dependencias de los paquetes de la CLI, la generación de perfiles de modelos está disponible en Core.
    + Actualiza la versión mínima de la CLI de Azure a 2.3.0
  + **azureml-automl-core**
    + Mejor mensaje de excepción en el paso de características fit_transform() debido a parámetros de transformador personalizados.
    + Agregue compatibilidad con varios idiomas para los modelos de transformador de aprendizaje profundo como BERT en los ML de aprendizaje automático.
    + Quitar de la documentación el parámetro de lag_length en desuso.
    + Se mejoró la documentación de los parámetros de previsión. El parámetro lag_length quedó en desuso.
  + **azureml-automl-runtime**
    + Se corrigió el error que se producía cuando una de las columnas de categorías está vacía en el tiempo de previsión y de prueba.
    + Corrija los errores de ejecución que se producen cuando se habilitan las características de lookback y los datos contienen granos cortos.
    + Se corrigió el problema con el mensaje de error de índice de tiempo duplicado cuando los intervalos o las ventanas graduales se establecieron en "automático".
    + Se corrigió el problema con los modelos Prophet y Arima en el conjuntos de datos, que contienen las características de lookback.
    + Se agregó compatibilidad con las fechas anteriores a 21-09-1677 o posteriores a 11-04-2262 en las columnas distintas de la fecha y hora en las tareas de previsión. Han mejoraron los mensajes de error.
    + Se mejoró la documentación de los parámetros de previsión. El parámetro lag_length quedó en desuso.
    + Mejor mensaje de excepción en el paso de características fit_transform() debido a parámetros de transformador personalizados.
    + Agregue compatibilidad con varios idiomas para los modelos de transformador de aprendizaje profundo como BERT en los ML de aprendizaje automático.
    + Las operaciones de caché que dan como resultado algún OSErrors producirán un error de usuario.
    + Se agregaron comprobaciones para garantizar que los datos de entrenamiento y validación tienen el mismo número y conjunto de columnas
    + Se corrigió un problema con el script de puntuación de AutoML generado automáticamente cuando los datos contienen comillas
    + Habilitar explicaciones para AutoML Prophet y modelos conjuntos que contienen el modelo Prophet.
    + Un problema de cliente reciente reveló un error de sitio activo en el que se registran los mensajes con clase-balance-barrido, incluso cuando la lógica de equilibrio de clase no está habilitada correctamente. Quitar esos registros o mensajes con este PR.
  + **azureml-cli-common**
    + Se completó la eliminación de la generación de perfiles de modelos de mir contrib al limpiar los comandos y las dependencias de los paquetes de la CLI, la generación de perfiles de modelos está disponible en Core.
  + **azureml-contrib-reinforcementlearning**
    + Herramienta de prueba de carga
  + **azureml-core**
    + Cambios de documentación en Script_run_config.py
    + Corrige un error con la impresión de la salida de ejecución de la CLI de envío-canalización
    + Mejoras en la documentación de azureml-core/azureml.data
    + Corrige el problema de recuperación de la cuenta de almacenamiento mediante el comando hdfs getconf
    + Documentación mejorada de register_azure_blob_container y register_azure_file_share
  + **azureml-datadrift**
    + Implementación mejorada para deshabilitar y habilitar monitores de derivación del conjunto de datos
  + **azureml-interpret**
    + En la explicación del cliente, quite NaNs o Infs antes de la serialización json al cargar los artefactos
    + Actualización a la versión más reciente de interpretate-community para mejorar los errores de memoria insuficiente para las explicaciones globales con muchas características y clases
    + Agregue el parámetro opcional true_ys para la carga de explicación para habilitar características adicionales en la interfaz de usuario de Studio.
    + Mejorar el rendimiento de download_model_explanations() y list_model_explanations()
    + Pequeños ajustes a lo cuadernos, para ayudar con la depuración
  + **azureml-opendatasets**
    + azureml-opendatasets necesita azureml-dataprep versión 1.4.0 o posterior. Advertencia agregada si se detecta una versión inferior
  + **azureml-pipeline-core**
    + Este cambio permite al usuario proporcionar un runconfig opcional al moduleVersion al llamar al module.Publish_python_script.
    + La habilitación de la cuenta de nodo puede ser un parámetro de canalización de ParallelRunStep en azureml.pipeline.steps
  + **azureml-pipeline-steps**
    + Este cambio permite al usuario proporcionar un runconfig opcional al moduleVersion al llamar al module.Publish_python_script.
  + **azureml-train-automl-client**
    + Agregue compatibilidad con varios idiomas para los modelos de transformador de aprendizaje profundo como BERT en los ML de aprendizaje automático.
    + Quitar de la documentación el parámetro de lag_length en desuso.
    + Se mejoró la documentación de los parámetros de previsión. El parámetro lag_length quedó en desuso.
  + **azureml-train-automl-runtime**
    + Habilitar explicaciones para AutoML Prophet y modelos conjuntos que contienen el modelo Prophet.
    + Actualizaciones de documentación de los paquetes azureml-train-automl-*.
  + **azureml-train-core**
    + Compatibilidad con la versión 2.1 de TensorFlow en el estimador de PyTorch
    + Mejoras al paquete azureml-train-core.
  
## <a name="2020-05-26"></a>26-05-2020

### <a name="azure-machine-learning-sdk-for-python-v160"></a>SDK de Azure Machine Learning para Python v1.6.0

+ **Nuevas características:**
  + **azureml-automl-runtime**
    + AutoML Forecasting ahora admite la previsión de clientes más allá del horizonte máximo especificado previamente sin necesidad de reentrenar el modelo. Si el destino de la previsión es posterior al horizonte máximo especificado, la función forecast() realizará predicciones puntuales hasta la fecha más alejada mediante un modo de operación recursivo. Para ver cómo se ilustra la nueva característica, consulte la sección "Previsión posterior al horizonte máximo" del cuaderno "forecasting-forecast-function" de la [carpeta](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)".
  
  + **azureml-pipeline-steps**
    + Se ha publicado ParallelRunStep y forma parte del paquete **azureml-pipeline-steps**. La funcionalidad ParallelRunStep del paquete **azureml-contrib-pipeline-steps** está en desuso. Cambios en la versión preliminar pública:
      + Se ha agregado el parámetro configurable opcional `run_max_try` para controlar el número máximo de llamadas para ejecutar el método en un lote dado, el valor predeterminado es 3.
      + Ya no se generan PipelineParameters automáticamente. Los siguientes valores configurables se pueden establecer como PipelineParameter de forma explícita.
        + mini_batch_size
        + node_count
        + process_count_per_node
        + logging_level
        + run_invocation_timeout
        + run_max_try
      + El valor predeterminado de process_count_per_node se ha cambiado a 1. El usuario debe ajustar este valor para mejorar el rendimiento. El procedimiento recomendado es establecerlo en el número de GPU o CPU que tiene el nodo.
      + ParallelRunStep no inserta ningún paquete, el usuario debe incluir los paquetes **azureml-core** y **azureml-dataprep[pandas, fuse]** en la definición del entorno. Si la imagen de Docker personalizada se usa con user_managed_dependencies, el usuario debe instalar Conda en la imagen.
      
+ **Cambios importantes**
  + **azureml-pipeline-steps**
    + Ha quedado en desuso azureml.dprep.Dataflow como tipo válido de entrada para AutoMLConfig
  + **azureml-train-automl-client**
    + Ha quedado en desuso azureml.dprep.Dataflow como tipo válido de entrada para AutoMLConfig

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se ha corregido un error en el que aparecía una advertencia durante `get_output` que pedía al usuario cambiar a una versión anterior del cliente.
    + Se ha actualizado Mac para que use cudatoolkit=9.0, ya que aún no está disponible en la versión 10.
    + Se eliminan las restricciones en los modelos prophet y xgboost cuando se entrenan en un proceso remoto.
    + Se ha mejorado el registro en AutoML.
    + Se ha mejorado el control de errores para la caracterización personalizada en las tareas de previsión.
    + Se ha agregado una funcionalidad que permite a los usuarios incluir características con desfase para generar previsiones.
    + Se han actualizado los mensajes de error para que muestren correctamente el error del usuario.
    + Se admite que cv_split_column_names se use con training_data
    + Se ha actualizado el registro del mensaje y seguimiento de la excepción.
  + **azureml-automl-runtime**
    + Se ha habilitado la protección para las imputaciones de los valores que faltan en la previsión.
    + Se ha mejorado el registro en AutoML.
    + Se ha agregado un control de errores minucioso para las excepciones que se produzcan en la preparación de datos.
    + Se eliminan las restricciones en los modelos prophet y xgboost cuando se entrenan en un proceso remoto.
    + `azureml-train-automl-runtime` y `azureml-automl-runtime` han actualizado las dependencias de `pytorch`, `scipy` y `cudatoolkit`. Ahora se admiten `pytorch==1.4.0`, `scipy>=1.0.0,<=1.3.1` y `cudatoolkit==10.1.243`.
    + Se ha mejorado el control de errores para la caracterización personalizada en las tareas de previsión.
    + Se ha mejorado el mecanismo de detección de frecuencia del conjunto de datos de previsión.
    + Se ha corregido un problema con el entrenamiento del modelo Prophet en algunos conjuntos de datos.
    + Se ha mejorado la detección automática del horizonte máximo durante la previsión.
    + Se ha agregado una funcionalidad que permite a los usuarios incluir características con desfase para generar previsiones.
    +  Se han agregado funcionalidades en la función de pronostico que permite realizar previsiones más allá del horizonte entrenado sin tener que volver a entrenar el modelo de previsión.
    + Se admite que cv_split_column_names se use con training_data
  + **azureml-contrib-automl-dnn-forecasting**
    + Se ha mejorado el registro en AutoML.
  + **azureml-contrib-mir**
    + Se ha agregado compatibilidad con servicios de Windows en ManagedInferencing
    + Se han eliminado flujos de trabajo de MIR antiguos, como el de asociación de procesos de MIR, clase SingleModelMirWebservice. Se ha incluido un generador de perfiles de modelos limpio en el paquete contrib-mir.
  + **azureml-contrib-pipeline-steps**
    + Corrección secundaria relacionada con la compatibilidad con YAML.
    + ParallelRunStep se ha publicado con disponibilidad general: azureml.contrib.pipeline.steps tiene un aviso de desuso y se mueve a azureml.pipeline.steps
  + **azureml-contrib-reinforcementlearning**
    + Herramienta para pruebas de carga de aprendizaje reforzado
    + El estimador de aprendizaje reforzado tiene valores predeterminados inteligentes
  + **azureml-core**
    + Se han eliminado flujos de trabajo de MIR antiguos, como el de asociación de procesos de MIR, clase SingleModelMirWebservice. Se ha incluido un generador de perfiles de modelos limpio en el paquete contrib-mir.
    + Se ha corregido la información que se proporciona al usuario en caso de error de generación de perfiles: se ha incluido el id. de solicitud y se ha rehecho el mensaje para que sea más significativo. Se ha agregado un nuevo flujo de trabajo de generación de perfiles a los ejecutores de generación de perfiles.
    + Se ha mejorado el texto de error en el caso de los errores de ejecución de conjuntos de datos.
    + Se ha agregado compatibilidad con la CLI de Private Link del área de trabajo.
    + Se ha agregado un parámetro opcional `invalid_lines` a `Dataset.Tabular.from_json_lines_files` que permite especificar cómo administrar las líneas que contienen código JSON no válido.
    + La creación de procesos basada en la ejecución quedará en desuso en la próxima versión. Se recomienda crear un clúster de Amlcompute real como destino de proceso persistente y usar el nombre del clúster como destino de proceso en la configuración de ejecución. Consulte un cuaderno de ejemplo aquí: aka.ms/amlcomputenb
    + Se han mejorado los mensajes de error en el caso de los errores de ejecución de conjuntos de datos.
  + **azureml-dataprep**
    + Se ha realizado una advertencia para actualizar la versión de pyarrow de forma más explícita.
    + Se ha mejorado el control de errores y el mensaje que se devuelve en caso de error al ejecutar el flujo de datos.
  + **azureml-interpret**
    + Actualizaciones de la documentación para el paquete azureml-interpret.
    + Se han reparado los paquetes y cuadernos de interpretación para que sean compatibles con la actualización más reciente de sklearn
  + **azureml-opendatasets**
    + Se devuelve None cuando no se devuelven datos.
    + Se ha mejorado el rendimiento de to_pandas_dataframe.
  + **azureml-pipeline-core**
    + Reparación rápida de ParallelRunStep cuando se ha interrumpido la carga desde YAML.
    + ParallelRunStep se ha publicado con disponibilidad general: azureml.contrib.pipeline.steps tiene un aviso de desuso y se mueve a azureml.pipeline.steps. Entre las características nuevas se incluyen: 1. Conjuntos de datos como PipelineParameter 2. Nuevo parámetro run_max_retry 3. Nombre de archivo de salida append_row configurable
  + **azureml-pipeline-steps**
    + Deprecated azureml.dprep.Dataflow como un tipo válido para los datos de entrada.
    + Reparación rápida de ParallelRunStep cuando se ha interrumpido la carga desde YAML.
    + ParallelRunStep se ha publicado con disponibilidad general: azureml.contrib.pipeline.steps tiene un aviso de desuso y se mueve a azureml.pipeline.steps. Entre las características nuevas se incluyen:
      + Conjuntos de datos como PipelineParameter
      + Nuevo parámetro run_max_retry
      + Nombre de archivo de salida append_row configurable
  + **azureml-telemetry**
    + Se ha actualizado el registro del mensaje y seguimiento de la excepción.
  + **azureml-train-automl-client**
    + Se ha mejorado el registro en AutoML.
    + Se han actualizado los mensajes de error para que muestren correctamente el error del usuario.
    + Se admite que cv_split_column_names se use con training_data
    + Deprecated azureml.dprep.Dataflow como un tipo válido para los datos de entrada.
    + Se ha actualizado Mac para que use cudatoolkit=9.0, ya que aún no está disponible en la versión 10.
    + Se eliminan las restricciones en los modelos prophet y xgboost cuando se entrenan en un proceso remoto.
    + `azureml-train-automl-runtime` y `azureml-automl-runtime` han actualizado las dependencias de `pytorch`, `scipy` y `cudatoolkit`. Ahora se admiten `pytorch==1.4.0`, `scipy>=1.0.0,<=1.3.1` y `cudatoolkit==10.1.243`.
    + Se ha agregado una funcionalidad que permite a los usuarios incluir características con desfase para generar previsiones.
  + **azureml-train-automl-runtime**
    + Se ha mejorado el registro en AutoML.
    + Se ha agregado un control de errores minucioso para las excepciones que se produzcan en la preparación de datos.
    + Se eliminan las restricciones en los modelos prophet y xgboost cuando se entrenan en un proceso remoto.
    + `azureml-train-automl-runtime` y `azureml-automl-runtime` han actualizado las dependencias de `pytorch`, `scipy` y `cudatoolkit`. Ahora se admiten `pytorch==1.4.0`, `scipy>=1.0.0,<=1.3.1` y `cudatoolkit==10.1.243`.
    + Se han actualizado los mensajes de error para que muestren correctamente el error del usuario.
    + Se admite que cv_split_column_names se use con training_data
  + **azureml-train-core**
    + Se ha agregado un conjunto nuevo de excepciones específicas de HyperDrive. azureml.train.hyperdrive ahora generará excepciones detalladas.
  + **azureml-widgets**
    + Los widgets de AzureML no aparecen en JupyterLab.
  

## <a name="2020-05-11"></a>2020-05-11

### <a name="azure-machine-learning-sdk-for-python-v150"></a>SDK de Azure Machine Learning para Python v1.5.0

+ **Nuevas características:**
  + **Características en versión preliminar**
    + **azureml-contrib-reinforcementlearning**
        + Azure Machine Learning ofrecerá compatibilidad en versión preliminar con el aprendizaje de refuerzo mediante el marco [Ray](https://ray.io). `ReinforcementLearningEstimator` permite el entrenamiento de los agentes de aprendizaje de refuerzo entre destinos de proceso de la CPU y la GPU en Azure Machine Learning.

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se corrigió un registro de advertencia que quedó activo por error en la solicitud de incorporación de cambios anterior. El registro se usó para la depuración y quedó activo por error.
    + Corrección del error: se informa a los clientes del error parcial durante la generación de perfiles.
  + **azureml-automl-core**
    + Se ha acelerado el modelo Prophet/AutoArima en la previsión de AutoML mediante la habilitación del ajuste paralelo para la serie temporal cuando los conjuntos de datos tienen varias series temporales. Para beneficiarse de esta nueva característica, le recomendamos que establezca "max_cores_per_iteration = -1" (es decir, que use todos los núcleos de la CPU disponibles) en AutoMLConfig.
    + Se corrigió la excepción KeyError al imprimir los límites de protección en la interfaz de la consola.
    + Se corrigió el mensaje de error de experimentation_timeout_hours.
    + Se dejaron de usar los modelos de TensorFlow para AutoML.
  + **azureml-automl-runtime**
    + Se corrigió el mensaje de error de experimentation_timeout_hours.
    + Se corrigió una excepción no clasificada al intentar realizar una deserialización desde el almacén de caché.
    + Se ha acelerado el modelo Prophet/AutoArima en la previsión de AutoML mediante la habilitación del ajuste paralelo para la serie temporal cuando los conjuntos de datos tienen varias series temporales.
    + Se corrigió la previsión con el periodo acumulado habilitado en los conjuntos de datos en los que el conjunto de pruebas o predicción no contiene uno de los intervalos del conjunto de entrenamiento.
    + Se mejoró la administración de los datos que faltan.
    + Se corrigió un problema con los intervalos de predicción durante la previsión en los conjuntos de datos, que contienen series temporales y que no están alineados en el tiempo.
    + Se agregó una validación mejorada de la forma de los datos para las tareas de previsión.
    + Se mejoró la detección de la frecuencia.
    + Se ha creado un mensaje de error mejorado que se muestra cuando no se puede generar la validación cruzada de iteraciones para las tareas de previsión.
    + Se corrigió la interfaz de la consola para imprimir el valor del límite de protección que falta correctamente.
    + Se aplicaron comprobaciones de tipos de datos en la entrada cv_split_indices de la clase AutoMLConfig.
  + **azureml-cli-common**
    + Corrección del error: se informa a los clientes del error parcial durante la generación de perfiles.
  + **azureml-contrib-mir**
    + Se agregó la clase azureml.contrib.mir.RevisionStatus, que retransmite información sobre la revisión de MIR implementada actualmente y la versión más reciente especificada por el usuario. Esta clase se incluye en el objeto MirWebservice en el atributo "deployment_status".
    + Se habilitó la actualización en Webservices del tipo MirWebservice y su clase secundaria SingleModelMirWebservice.
  + **azureml-contrib-reinforcementlearning**
    + Se agregó compatibilidad con Ray 0.8.3.
    + AmlWindowsCompute solo admite Azure Files como almacenamiento montado.
    + Se cambió el nombre de health_check_timeout por health_check_timeout_seconds.
    + Se corrigieron algunas descripciones de clases o métodos.
  + **azureml-core**
    + Se habilitaron las conversiones de WASB a blob en las nubes Azure Government y de China.
    + Se corrigió el error para permitir que los roles de lector usen los comandos az ml run de la CLI para obtener información de ejecución.
    + Se quitó el registro innecesario durante las ejecuciones remotas de Azure ML con conjuntos de datos de entrada.
    + RCranPackage ahora admite el parámetro "version" para la versión del paquete de CRAN.
    + Corrección del error: se informa a los clientes del error parcial durante la generación de perfiles.
    + Se agregó el control float de estilo europeo para azureml-core.
    + Se han habilitado características de Private Link del área de trabajo en el SDK de Azure ML.
    + Al crear un elemento TabularDataset con `from_delimited_files`, puede especificar si los valores vacíos se deben cargar como Ninguno o como una cadena vacía mediante el establecimiento del argumento booleano `empty_as_string`.
    + Se agregó el control float de estilo europeo para los conjuntos de datos.
    + Se mejoraron los mensajes de error en los errores de montaje del conjunto de datos.
  + **azureml-datadrift**
    + La consulta de resultados del desfase de datos del SDK producía un error en el que no se diferenciaban las métricas de características mínimas, máximas y medias, lo que daba lugar a valores duplicados. Para corregir este error, hemos agregado los prefijos "target" (destino) o "baseline" (base de referencia) a los nombres de las métricas. Los valores anteriores eran los siguientes: duplicate min, max, mean. Ahora son los siguientes: target_min, target_max, target_mean, baseline_min, baseline_max, baseline_mean.
  + **azureml-dataprep**
    + Se mejoró el control de los entornos de Python con la escritura restringida al garantizar las dependencias de .NET necesarias para la entrega de datos.
    + Se corrigió la creación del flujo de datos registrado con registros vacíos iniciales.
    + Se agregaron opciones de control de errores para `to_partition_iterator` similares a `to_pandas_dataframe`.
  + **azureml-interpret**
    + Se redujeron los límites de longitud de la ruta de acceso de explicación a fin de disminuir la probabilidad de superar el límite de Windows.
    + Se corrigió un error con las explicaciones dispersas creadas con la explicación de imitación mediante un modelo lineal suplente.
  + **azureml-opendatasets**
    + Se ha corregido el problema por el que las columnas MNIST se analizaban como si fueran una cadena, en lugar de como valores enteros.
  + **azureml-pipeline-core**
    + Se permitió la opción regenerate_outputs al usar un módulo insertado en una clase ModuleStep.
  + **azureml-train-automl-client**
    + Se dejaron de usar los modelos de TensorFlow para AutoML.
    + Se corrigieron los usuarios que permiten mostrar algoritmos incompatibles en modo local.
    + Se realizaron correcciones de la clase AutoMLConfig en la documentación.
    + Se aplicaron comprobaciones de tipos de datos en la entrada cv_split_indices de la clase AutoMLConfig.
    + Se corrigió el problema con los errores de ejecución de AutoML en show_output.
  + **azureml-train-automl-runtime**
    + Se ha corregido un error con las iteraciones de conjunto que impedía que el tiempo de espera de descarga de modelos se iniciara correctamente.
  + **azureml-train-core**
    + Se corrigió un error ortográfico en la clase azureml.train.dnn.Nccl.
    + Se agregó compatibilidad con la versión 1.5 de PyTorch en el estimador de PyTorch.
    + Se ha corregido el problema en el que la imagen del marco no se podía capturar en la región de Azure Government al usar los estimadores del marco de entrenamiento.

  
## <a name="2020-05-04"></a>04-05-2020
**Nueva experiencia de cuaderno**

Ya puede crear, editar y compartir archivos y cuadernos de aprendizaje automático directamente dentro de la experiencia de web de Studio de Azure Machine Learning. Puede usar todas las clases y métodos disponibles en el [SDK de Python de Azure Machine Learning](/python/api/overview/azure/ml/intro) desde estos cuadernos. Empiece [aquí](./how-to-run-jupyter-notebooks.md).

**Nuevas características presentadas:**

+ Editor mejorado (editor Monaco) usado por VS Code 
+ Mejoras de interfaz de usuario y experiencia del usuario
+ Barra de herramientas de celdas
+ Controles de Proceso y nueva barra de herramientas del cuaderno
+ Barra de estado del cuaderno 
+ Conmutación en línea de Kernel
+ Compatibilidad con R
+ Mejoras en la accesibilidad y la localización
+ Paleta de comandos
+ Métodos abreviados de teclado adicionales
+ Autoguardado
+ Rendimiento y confiabilidad mejorados

Acceda a las siguientes herramientas de creación basadas en web desde Studio:
    
| Herramienta basada en web  |     Descripción  |
|---|---|
| Cuadernos de Azure ML Studio   |     Creación de primera clase de archivos de cuadernos y compatibilidad con todas las operaciones disponibles en el SDK de Python de Azure ML. | 

## <a name="2020-04-27"></a>27-04-2020

### <a name="azure-machine-learning-sdk-for-python-v140"></a>SDK de Azure Machine Learning para Python v1.4.0

+ **Nuevas características:**
  + Los clústeres de AmlCompute ahora admiten la configuración de una identidad administrada en el clúster en el momento del aprovisionamiento. Simplemente, especifique si quiere usar una identidad asignada por el sistema o asignada por el usuario y, si opta por esta última, pase un valor de identityId. Después, puede configurar permisos para acceder a varios recursos, como Storage o ACR, de forma que la identidad del proceso se use para acceder de forma segura a los datos, en lugar del enfoque basado en tokens que AmlCompute emplea en la actualidad. Consulte nuestra referencia de SDK para obtener más información sobre los parámetros.
  

+ **Cambios importantes**
  + Los clústeres de AmlCompute admitían una característica en vista previa en torno a la creación basada en la ejecución que estamos planeando retirar en dos semanas. Puede continuar con la creación de destinos de proceso persistentes como siempre mediante la clase Amlcompute, pero el enfoque concreto para especificar el identificador "amlcompute" como destino de proceso en la configuración de ejecución no se admitirá en un futuro próximo. 

+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + Habilitar la compatibilidad para un tipo sin hash al calcular el número de valores únicos de una columna.
  + **azureml-core**
    + Estabilidad mejorada al leer desde Azure Blob Storage mediante TabularDataset.
    + Documentación mejorada para el parámetro `grant_workspace_msi` para `Datastore.register_azure_blob_store`.
    + Se corrigió el error con `datastore.upload` para admitir el argumento `src_dir` que termina con `/` o `\`.
    + Se ha agregado un mensaje de error accionable al intentar cargar en un almacén de datos de Azure Blob Storage que no tiene una clave de acceso ni un token de SAS.
  + **azureml-interpret**
    + Se ha agregado el límite superior al tamaño de archivo para los datos de visualización en las explicaciones cargadas.
  + **azureml-train-automl-client**
    + Comprobando explícitamente si hay parámetros label_column_name y weight_column_name para que AutoMLConfig sea del tipo cadena.
  + **azureml-contrib-pipeline-steps**
    + Ahora, ParallelRunStep admite ahora el parámetro de conjunto de datos como canalización. El usuario puede construir una canalización con un conjunto de datos de muestra y cambiar el conjunto de datos de entrada del mismo tipo (archivo o tabular) para la nueva ejecución de canalización.

  
## <a name="2020-04-13"></a>2020-04-13

### <a name="azure-machine-learning-sdk-for-python-v130"></a>SDK de Azure Machine Learning para Python v1.3.0

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se ha agregado telemetría adicional en torno a las operaciones posteriores al entrenamiento.
    + Acelera el aprendizaje automático de ARIMA mediante el entrenamiento de suma condicional de cuadrados (CSS) para la serie de longitud superior a 100. La longitud usada se almacena como la constante ARIMA_TRIGGER_CSS_TRAINING_LENGTH w/en la clase TimeSeriesInternal en /src/azureml-automl-core/azureml/automl/core/shared/constants.py
    + Se mejoró el registro de usuario de las ejecuciones de previsión; ahora se mostrará más información sobre qué fase se está ejecutando actualmente en el registro.
    + No se permite que target_rolling_window_size se establezca en valores inferiores a 2.
  + **azureml-automl-runtime**
    + Se ha mejorado el mensaje de error que se muestra cuando se encuentran marcas de tiempo duplicadas.
    + No se permite que target_rolling_window_size se establezca en valores inferiores a 2.
    + Se corrigió el error de imputación de retraso. El problema se debe a un número insuficiente de observaciones para descomponer una serie de forma estacional. Los datos "desestacionalizados" se usan para calcular una función de autocorrelación parcial (PACF) para determinar la longitud de retardo.
    + Se ha habilitado la personalización de caracterización de propósito de columna para las tareas de previsión mediante la configuración de caracterización. Ahora se admite el propósito de numérica y categórica como columna para las tareas de previsión.
    + Se ha habilitado la personalización de caracterización de eliminación de columnas para las tareas de previsión mediante la configuración de caracterización.
    + Se ha habilitado la personalización de imputación para las tareas de previsión mediante la configuración de caracterización. Ahora se admite la imputación de valores constantes para las columnas de destino y la imputación de valores de media, mediana, más frecuente y constante para los datos de entrenamiento.
  + **azureml-contrib-pipeline-steps**
    + Se acepta que los nombres de proceso de cadena se pasen a ParallelRunConfig.
  + **azureml-core**
    +  Se ha agregado la API Environment.clone(new_name) para crear una copia del objeto Environment.
    +  Environment.docker.base_dockerfile acepta filepath. Si puede resolver un archivo, el contenido se leerá en la propiedad del entorno base_dockerfile.
    + Se restablecen automáticamente los valores mutuamente excluyentes para base_image y base_dockerfile cuando el usuario establece manualmente un valor en Environment.docker.
    + Se agregó la marca user_managed en RSection, que indica si el entorno lo administra el usuario o AzureML.
    + Conjunto de datos: se ha corregido el error de descarga del conjunto de datos cuando la ruta de acceso a datos contiene caracteres Unicode.
    + Conjunto de datos: se ha mejorado el mecanismo de almacenamiento en caché del montaje de conjunto de datos para respetar el requisito de espacio en disco mínimo de Proceso de Azure Machine Learning, lo que evita que el nodo quede inutilizable y que se cancele el trabajo.
    + Conjunto de datos: se ha agregado un índice para la columna de serie temporal cuando se accede a un conjunto de datos de serie temporal en forma de tramas de datos de Pandas, que se usa para acelerar el acceso a los datos basado en la serie temporal.  Anteriormente, se daba al índice el mismo nombre que a la columna de marca de tiempo, lo que confundía a los usuarios sobre cuál era la columna de marca de tiempo real y cuál era el índice. Ahora no se proporciona ningún nombre específico para el índice, ya que no debe usarse como columna. 
    + Conjunto de datos: Se ha corregido el problema de autenticación del conjunto de datos en la nube soberana.
    + Conjunto de datos: Se ha corregido un error de `Dataset.to_spark_dataframe` para los conjuntos de datos creados desde almacenes de datos de Azure PostgreSQL.
  + **azureml-interpret**
    + Se han agregado puntuaciones globales a la visualización si los valores de importancia local están dispersos.
    + Se ha actualizado azureml-interpret para usar interpret-community 0.9.*
    + Se ha corregido un problema con la descarga de la explicación que tenía datos de evaluación dispersos.
    + Se ha agregado compatibilidad con el formato disperso del objeto de explicación en AutoML.
  + **azureml-pipeline-core**
    + Compatibilidad con ComputeInstance como destino de proceso en las canalizaciones.
  + **azureml-train-automl-client**
    + Se ha agregado telemetría adicional en torno a las operaciones posteriores al entrenamiento.
    + Se ha corregido la regresión en la detención temprana.
    + Deprecated azureml.dprep.Dataflow como un tipo válido para los datos de entrada.
    +  Se ha cambiado el tiempo de espera predeterminado del experimento de AutoML a seis días.
  + **azureml-train-automl-runtime**
    + Se ha agregado telemetría adicional en torno a las operaciones posteriores al entrenamiento.
    + Se ha agregado compatibilidad con sparse AutoML de un extremo a otro
  + **azureml-opendatasets**
    + Se ha agregado telemetría adicional para el monitor de servicio.
    + Se ha habilitado la puerta principal para blobs para aumentar la estabilidad. 

## <a name="2020-03-23"></a>2020-03-23

### <a name="azure-machine-learning-sdk-for-python-v120"></a>SDK de Azure Machine Learning para Python v1.2.0

+ **Cambios importantes**
  + Se ha retirado la compatibilidad con Python 2.7.

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Agrega "--subscription-id" a los comandos `az ml model/computetarget/service` en la CLI.
    + Se ha agregado compatibilidad para pasar la clave administrada por el cliente (CMK) vault_url, key_name y key_version para la implementación de ACI.
  + **azureml-automl-core** 
    + Se ha habilitado la imputación personalizada con un valor constante para las tareas de previsión de datos X e Y.
    + Se ha corregido el problema que mostraba los mensajes de error al usuario.    
  + **azureml-automl-runtime**
    + Se ha corregido el problema con la previsión en los conjuntos de datos, que contenía granos con una sola fila.
    + Se ha disminuido la cantidad de memoria necesaria para las tareas de previsión.
    + Se han agregado mejores mensajes de error si la columna de tiempo tiene un formato incorrecto.
    + Se ha habilitado la imputación personalizada con un valor constante para las tareas de previsión de datos X e Y.
  + **azureml-core**
    + Se ha agregado compatibilidad para cargar ServicePrincipal desde las variables de entorno: AZUREML_SERVICE_PRINCIPAL_ID, AZUREML_SERVICE_PRINCIPAL_TENANT_ID y AZUREML_SERVICE_PRINCIPAL_PASSWORD
    + Se ha presentado un nuevo parámetro `support_multi_line` en `Dataset.Tabular.from_delimited_files`: De forma predeterminada (`support_multi_line=False`), todos los saltos de línea, incluidos los de los valores de campo entre comillas, se interpretarán como un salto de registro. Leer los datos de esta manera es una forma más rápida y optimizada de ejecutar en paralelo en varios núcleos de CPU. Sin embargo, puede provocar la generación silenciosa de más registros con valores de campo mal alineados. Debe establecerse en `True` cuando se sabe que los archivos delimitados contienen saltos de línea entre comillas.
    + Se ha agregado la capacidad de registrar ADLS Gen2 en la CLI de Azure Machine Learning
    + Se ha cambiado el nombre del parámetro "fine_grain_timestamp" a "timestamp", y el parámetro "coarse_grain_timestamp" a "partition_timestamp" para el método with_timestamp_columns () de TabularDataset con el objetivo de reflejar mejor el uso de los parámetros.
    + Se ha aumentado la longitud máxima del nombre del experimento en 255.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret a interpret-community 0.7*.
  + **azureml-sdk**
    + Se han cambiado las dependencias con una versión compatible de Tilde para agregar compatibilidad de la revisión de parches en versiones preliminares y estables.


## <a name="2020-03-11"></a>11-03-2020

### <a name="azure-machine-learning-sdk-for-python-v115"></a>SDK de Azure Machine Learning para Python v1.1.5

+ **Característica en desuso**
  + **Python 2.7**
    + Última versión compatible con Python 2.7

+ **Cambios importantes**
  + **Versionamiento Semántico 2.0.0**
    + A partir de la versión 1.1, el SDK de Python para Azure Machine Learning adopta Versionamiento Semántico 2.0.0. [Obtenga más información aquí.](https://semver.org/) Todas las versiones posteriores seguirán un nuevo esquema de numeración y un contrato de Versionamiento Semántico. 

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se ha cambiado el nombre del comando de la CLI del punto de conexión de "az ml endpoint aks" a "az ml endpoint real time" para que sea coherente.
    + Se han actualizado las instrucciones de instalación de la CLI para la CLI de rama estable y experimental.
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
  + **azureml-automl-core**
    + Se ha habilitado la inferencia del modo de lotes (tomando varias filas a la vez) para los modelos ONNX de AutoML.
    + Se ha mejorado la detección de frecuencia en los conjuntos de datos, de la falta de datos o que contienen puntos de datos irregulares.
    + Se ha agregado la capacidad de quitar los puntos de datos que no cumplen con la frecuencia dominante.
    + Se ha cambiado la entrada del constructor para tomar una lista de opciones para aplicar las opciones de imputación para las columnas correspondientes.
    + El registro de errores se ha mejorado.
  + **azureml-automl-runtime**
    + Se ha corregido el problema con el error que se producía si el intervalo de agregación no estaba presente en el conjunto de entrenamiento que aparecía en el conjunto de pruebas.
    + Se ha quitado el requisito y_query durante la puntuación en el servicio de previsión.
    + Se ha corregido el problema con la previsión que se producía cuando el conjunto de datos contenía períodos cortos de agregación en intervalos de tiempo prolongados.
    + Se ha corregido el problema que se producía cuando se activaba el horizonte máximo automático y la columna de fecha contenía fechas en forma de cadenas. Se han agregado mensajes de error y conversión adecuados para cuando la conversión a fecha no es posible.
    + Uso de NumPy y SciPy nativos para serializar y deserializar los datos intermedios para FileCacheStore (se usa para las ejecuciones de AutoML locales)
    + Se ha corregido un problema por el que las ejecuciones secundarias con error se quedaban bloqueadas en el estado "En ejecución".
    + Mayor velocidad de caracterización.
    + Se ha corregido la comprobación de frecuencia durante la puntuación. Ahora, en las tareas de previsión, no se requiere una equivalencia de frecuencia estricta entre el conjunto de entrenamiento y de prueba.
    + Se ha cambiado la entrada del constructor para tomar una lista de opciones para aplicar las opciones de imputación para las columnas correspondientes.
    + Se han corregido errores relacionados con la selección del tipo de retardo.
    + Se ha corregido el error no clasificado generado en los conjuntos de datos, que tenían intervalos con la fila única.
    + Se ha corregido la lentitud de la detección de frecuencia.
    + Se ha corregido un error en el control de excepciones de AutoML que provocaba que el motivo real de un error de entrenamiento se reemplazara por un error de atributo.
  + **azureml-cli-common**
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
  + **azureml-contrib-mir**
    + Agrega funcionalidad en la clase MirWebservice para recuperar el token de acceso.
    + Se usa la autenticación de tokens para MirWebservice de forma predeterminada durante la llamada de MirWebservice.run(). Solo se actualiza si se produce un error en la llamada.
    + La implementación de MirWebservice ahora requiere SKU adecuadas[Standard_DS2_v2, Standard_F16, Standard_A2_v2] en lugar de [Ds2v2, A2v2 y F16] respectivamente.
  + **azureml-contrib-pipeline-steps**
    + Se ha agregado el parámetro opcional side_inputs a ParallelRunStep. Este parámetro se puede utilizar para montar la carpeta en el contenedor. Los tipos que se admiten actualmente son DataReference y PipelineData.
    + Los parámetros pasados en ParallelRunConfig ahora se pueden sobrescribir pasando parámetros de canalización. Se admiten los nuevos parámetros de canalización siguientes: aml_mini_batch_size, aml_error_threshold, aml_logging_level y aml_run_invocation_timeout (aml_node_count y aml_process_count_per_node ya formaban parte de una versión anterior).
  + **azureml-core**
    + Los servicios web implementados de AzureML ahora tendrán como valor predeterminado el registro `INFO`. Esto se puede controlar estableciendo la variable de entorno `AZUREML_LOG_LEVEL` en el servicio implementado.
    + El SDK de Python usa el servicio de detección para usar el punto de conexión "API" en lugar de "canalizaciones".
    + Intercambio a las nuevas rutas en todas las llamadas del SDK.
    + Cambios en el enrutamiento de llamadas a ModelManagementService a una nueva estructura unificada.
      + El método de actualización de áreas de trabajo se ha puesto a disposición del público.
      + Se agregó el parámetro image_build_compute al método de actualización de áreas de trabajo para permitir que el usuario actualice el proceso para la compilación de imágenes.
    + Se han agregado mensajes de desuso al flujo de trabajo de generación de perfiles antiguos. Se han corregido los límites de CPU y memoria de generación de perfiles.
    + Se ha agregado RSection como parte del entorno para ejecutar trabajos de R.
    + Se ha agregado validación a `Dataset.mount` para generar un error cuando el origen del conjunto de datos no esté accesible o no contenga datos.
    + Se ha agregado `--grant-workspace-msi-access` como parámetro adicional para la CLI del almacén de datos para registrar el contenedor de blobs de Azure, lo que le permitirá registrar el contenedor de blobs que está detrás de una red virtual.
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
    + Se ha corregido el problema en aks.py _deploy.
    + Valida la integridad de los modelos que se cargan para evitar errores de almacenamiento silenciosos.
    + Ahora, el usuario puede especificar un valor para la clave de autenticación al volver a generar las claves para los servicios web.
    + Se ha corregido un error por el que las letras mayúsculas no se podían usar como nombre de entrada de un conjunto de datos.
  + **azureml-defaults**
    + `azureml-dataprep` se instalará ahora como parte de `azureml-defaults`. Ya no es necesario instalar data prep[fuse] manualmente en los destinos de proceso para montar conjuntos de datos.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret a interpret-community 0.6*.
    + Se ha actualizado azureml-interpret para que dependa de interpret-community 0.5.0.
    + Se han agregado excepciones de azureml-style a azureml-interpret.
    + Se ha corregido la serialización de DeepScoringExplainer para los modelos de Keras.
  + **azureml-mlflow**
    + Se ha agregado compatibilidad para nubes soberanas a azureml.mlflow.
  + **azureml-pipeline-core**
    + El cuaderno de puntuación por lotes de la canalización ahora usa ParallelRunStep
    + Se ha corregido un error que hacía que los resultados de PythonScriptStep se reutilizaran incorrectamente a pesar de cambiar la lista de argumentos.
    + Se ha agregado la capacidad de establecer el tipo de las columnas cuando se llama a los métodos parse_ * en `PipelineOutputFileDataset`.
  + **azureml-pipeline-steps**
    + Se ha movido `AutoMLStep` al paquete `azureml-pipeline-steps`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
    + Se ha agregado un ejemplo de documentación para el conjunto de datos como entrada PythonScriptStep.
  + **azureml-tensorboard**
    + Se ha actualizado azureml-tensorboard para que admita TensorFlow 2.0.
    + Se muestra el número de puerto correcto al usar un puerto TensorBoard personalizado en una instancia de proceso.
  + **azureml-train-automl-client**
    + Se ha corregido un problema por el que algunos paquetes se instalaban en versiones incorrectas en ejecuciones remotas.
    + Se ha corregido el problema de invalidación de FeaturizationConfig que filtra la configuración de características personalizada.
  + **azureml-train-automl-runtime**
    + Se ha corregido el problema con la detección de frecuencia en las ejecuciones remotas.
    + Se ha movido `AutoMLStep` del paquete `azureml-pipeline-steps`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-train-core**
    + Compatibilidad con la versión 1.4 de PyTorch en el estimador de PyTorch
  
## <a name="2020-03-02"></a>2020-03-02

### <a name="azure-machine-learning-sdk-for-python-v112rc0-pre-release"></a>SDK de Azure Machine Learning para Python v1.1.2rc0 (versión preliminar)

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se ha habilitado la inferencia del modo de lotes (tomando varias filas a la vez) para los modelos ONNX de AutoML.
    + Se ha mejorado la detección de frecuencia en los conjuntos de datos, de la falta de datos o que contienen puntos de datos irregulares.
    + Se ha agregado la capacidad de quitar los puntos de datos que no cumplen con la frecuencia dominante.
  + **azureml-automl-runtime**
    + Se ha corregido el problema con el error que se producía si el intervalo de agregación no estaba presente en el conjunto de entrenamiento que aparecía en el conjunto de pruebas.
    + Se ha quitado el requisito y_query durante la puntuación en el servicio de previsión.
  + **azureml-contrib-mir**
    + Agrega funcionalidad en la clase MirWebservice para recuperar el token de acceso.
  + **azureml-core**
    + Los servicios web implementados de AzureML ahora tendrán como valor predeterminado el registro `INFO`. Esto se puede controlar estableciendo la variable de entorno `AZUREML_LOG_LEVEL` en el servicio implementado.
    + Se ha corregido la iteración en `Dataset.get_all` para devolver todos los conjuntos de datos registrados en el área de trabajo.
    + Se ha mejorado el mensaje de error cuando se pasa un tipo no válido al argumento `path` de las API de creación del conjunto de datos.
    + El SDK de Python usa el servicio de detección para usar el punto de conexión "API" en lugar de "canalizaciones".
    + Intercambio a las nuevas rutas en todas las llamadas del SDK.
    + Cambios en el enrutamiento de llamadas a ModelManagementService a una nueva estructura unificada.
      + El método de actualización de áreas de trabajo se ha puesto a disposición del público.
      + Se agregó el parámetro image_build_compute al método de actualización de áreas de trabajo para permitir que el usuario actualice el proceso para la compilación de imágenes.
    +  Se han agregado mensajes de desuso al flujo de trabajo de generación de perfiles antiguos. Se han corregido los límites de CPU y memoria de generación de perfiles.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret a interpret-community 0.6*.
  + **azureml-mlflow**
    + Se ha agregado compatibilidad para nubes soberanas a azureml.mlflow.
  + **azureml-pipeline-steps**
    + `AutoMLStep` se ha movido a `azureml-pipeline-steps package`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-train-automl-client**
    + Se ha corregido un problema por el que algunos paquetes se instalaban en versiones incorrectas en ejecuciones remotas.
  + **azureml-train-automl-runtime**
    + Se ha corregido el problema con la detección de frecuencia en las ejecuciones remotas.
    + `AutoMLStep` se ha movido a `azureml-pipeline-steps package`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-train-core**
    + `AutoMLStep` se ha movido a `azureml-pipeline-steps package`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.

## <a name="2020-02-18"></a>18-02-2020

### <a name="azure-machine-learning-sdk-for-python-v111rc0-pre-release"></a>SDK de Azure Machine Learning para Python v1.1.1rc0 (versión preliminar)

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
  + **azureml-automl-core**
    + El registro de errores se ha mejorado.
  + **azureml-automl-runtime**
    + Se ha corregido el problema con la previsión que se producía cuando el conjunto de datos contenía períodos cortos de agregación en intervalos de tiempo prolongados.
    + Se ha corregido el problema que se producía cuando se activaba el horizonte máximo automático y la columna de fecha contenía fechas en forma de cadenas. Hemos agregado una conversión adecuada y la opción de que aparezca un error si la conversión a fecha no es posible
    + Uso de NumPy y SciPy nativos para serializar y deserializar los datos intermedios para FileCacheStore (se usa para las ejecuciones de AutoML locales)
    + Se ha corregido un problema por el que las ejecuciones secundarias con error se quedaban bloqueadas en el estado "En ejecución".
  + **azureml-cli-common**
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
  + **azureml-core**
    + Se ha agregado `--grant-workspace-msi-access` como parámetro adicional para la CLI del almacén de datos para registrar el contenedor de blobs de Azure, lo que le permitirá registrar el contenedor de blobs que está detrás de una red virtual.
    + La generación de perfiles de instancia única se ha corregido para generar una recomendación y se ha puesto a disposición en el SDK de .NET Core.
    + Se ha corregido el problema en aks.py _deploy
    + Valida la integridad de los modelos que se cargan para evitar errores de almacenamiento silenciosos.
  + **azureml-interpret**
    + Se han agregado excepciones de azureml-style a azureml-interpret
    + Se ha corregido la serialización de DeepScoringExplainer para los modelos de Keras
  + **azureml-pipeline-core**
    + El cuaderno de puntuación por lotes de la canalización ahora usa ParallelRunStep
  + **azureml-pipeline-steps**
    + Se ha movido `AutoMLStep` del paquete `azureml-pipeline-steps`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-contrib-pipeline-steps**
    + Se ha agregado el parámetro opcional side_inputs a ParallelRunStep. Este parámetro se puede utilizar para montar la carpeta en el contenedor. Los tipos que se admiten actualmente son DataReference y PipelineData.
  + **azureml-tensorboard**
    + Se ha actualizado azureml-tensorboard para que admita TensorFlow 2.0.
  + **azureml-train-automl-client**
    + Se ha corregido el problema de invalidación de FeaturizationConfig que filtra la configuración de características personalizada.
  + **azureml-train-automl-runtime**
    + Se ha movido `AutoMLStep` del paquete `azureml-pipeline-steps`. Se ha puesto en desuso `AutoMLStep` en `azureml-train-automl-runtime`.
  + **azureml-train-core**
    + Compatibilidad con la versión 1.4 de PyTorch en el estimador de PyTorch
  
## <a name="2020-02-04"></a>04-02-2020

### <a name="azure-machine-learning-sdk-for-python-v110rc0-pre-release"></a>SDK de Azure Machine Learning para Python v1.1.0rc0 (versión preliminar)

+ **Cambios importantes**
  + **Versionamiento Semántico 2.0.0**
    + A partir de la versión 1.1, el SDK de Python para Azure Machine Learning adopta Versionamiento Semántico 2.0.0. [Obtenga más información aquí.](https://semver.org/) Todas las versiones posteriores seguirán un nuevo esquema de numeración y un contrato de Versionamiento Semántico. 
  
+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + Mayor velocidad de caracterización.
    + Se corrigió la comprobación de frecuencia durante la puntuación. Ahora, en las tareas de previsión, no se requiere una equivalencia de frecuencia estricta entre el conjunto de entrenamiento y de prueba.
  + **azureml-core**
    + Ahora, el usuario puede especificar un valor para la clave de autenticación al volver a generar las claves para los servicios web.
  + **azureml-interpret**
    + Se ha actualizado azureml-interpret para que dependa de interpret-community 0.5.0.
  + **azureml-pipeline-core**
    + Se ha corregido un error que hacía que los resultados de PythonScriptStep se reutilizaran incorrectamente a pesar de cambiar la lista de argumentos.
  + **azureml-pipeline-steps**
    + Se ha agregado un ejemplo de documentación para el conjunto de datos como entrada PythonScriptStep.
  + **azureml-contrib-pipeline-steps**
    + Los parámetros pasados en ParallelRunConfig ahora se pueden sobrescribir pasando parámetros de canalización. Se admiten los nuevos parámetros de canalización siguientes: aml_mini_batch_size, aml_error_threshold, aml_logging_level y aml_run_invocation_timeout (aml_node_count y aml_process_count_per_node ya formaban parte de una versión anterior).
  
## <a name="2020-01-21"></a>2020-01-21

### <a name="azure-machine-learning-sdk-for-python-v1085"></a>SDK de Azure Machine Learning para Python v1.0.85

+ **Nuevas características:**
  + **azureml-core**
    + Obtenga el uso principal actual y la limitación de cuota para los recursos de AmlCompute en un área de trabajo y una suscripción determinadas
  
  + **azureml-contrib-pipeline-steps**
    + Permita al usuario pasar un conjunto de datos tabulares como resultado intermedio del paso anterior a parallelrunstep

+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + Se ha quitado el requisito de la columna y_query de la solicitud al servicio de previsión implementado. 
    + "y_query" se ha quitado de la sección de solicitud de servicio del cuaderno Orange Juice de Dominick.
    + Se ha corregido el error que impide la previsión en los modelos implementados y que funciona en conjuntos de datos con columnas de fecha y hora.
    + Se ha agregado el coeficiente de correlación de Matthews como una métrica de clasificación, tanto para la clasificación binaria como la multiclase.
  + **azureml-contrib-interpret**
    + Se han eliminado las explicaciones de texto de azureml-contrib-interpret, ya que la explicación del texto se ha movido al repositorio interpret-text que se publicará pronto.
  + **azureml-core**
    + Conjunto de datos: los usos del conjunto de datos de archivo ya no dependen de que Numpy y Pandas estén instalados en el entorno de Python.
    + Se ha cambiado LocalWebservice.wait_for_deployment () para comprobar el estado del contenedor de Docker local antes de intentar hacer ping a su punto de conexión de mantenimiento, lo que reduce considerablemente el tiempo que se tarda en informar de un error de implementación.
    + Se ha corregido la inicialización de una propiedad interna usada en LocalWebservice.reload() cuando el objeto de servicio se crea a partir de una implementación existente mediante el constructor LocalWebservice ().
    + Mensaje de error editado para clarificación.
    + Se ha agregado un nuevo método llamado get_access_token () a AksWebservice que devolverá el objeto AksServiceAccessToken, que contiene el token de acceso, la actualización después de la marca de tiempo, la expiración en la marca de tiempo y el tipo de token. 
    + El método get_token () existente está en desuso en AksWebservice, ya que el nuevo método devuelve toda la información que devuelve este método.
    + Se ha modificado la salida del comando get-access-token de az ml service. Se ha cambiado el nombre del token a accessToken y de refreshBy a refreshAfter. Se han agregado las propiedades expiryOn y tokenType.
    + Se ha reparado get_active_runs
  + **azureml-explain-model**
    + se ha actualizado shap a la versión 0.33.0 e interpret-community a la 0.4.*
  + **azureml-interpret**
    + se ha actualizado shap a la versión 0.33.0 e interpret-community a la 0.4.*
  + **azureml-train-automl-runtime**
    + Se ha agregado el coeficiente de correlación de Matthews como una métrica de clasificación, tanto para la clasificación binaria como la multiclase.
    + Se ha dejado en desuso la marca de preprocesamiento del código y se ha reemplazado por características-características está activada de forma predeterminada

## <a name="2020-01-06"></a>2020-01-06

### <a name="azure-machine-learning-sdk-for-python-v1083"></a>SDK de Azure Machine Learning para Python v 1.0.83

+ **Nuevas características:**
  + Conjunto de datos: Agregue dos opciones `on_error` y `out_of_range_datetime` para que se produzca un error en `to_pandas_dataframe` cuando los datos tengan valores de error en lugar de llenarlos con `None`.
  + Área de trabajo: Se ha agregado la marca `hbi_workspace` para las áreas de trabajo con información confidencial que habilita un cifrado adicional y deshabilita el diagnóstico avanzado en áreas de trabajo. También hemos agregado compatibilidad con la posibilidad de traer sus propias claves para la instancia de Cosmos DB asociada, mediante la especificación de los parámetros `cmk_keyvault` y `resource_cmk_uri` al crear un área de trabajo, lo que crea una instancia de Cosmos DB en su suscripción mientras aprovisiona el área de trabajo. [Obtenga más información aquí.](./concept-data-encryption.md#azure-cosmos-db)

+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + Se corrigió una regresión que provocaba un TypeError cuando se ejecutaba AutoML en las versiones de Python inferiores a 3.5.4.
  + **azureml-core**
    + Se corrigió un error en `datastore.upload_files`, donde la ruta de acceso relativa que no se inició con `./` no se podía usar.
    + Se han agregado mensajes de desuso para toda la clase de imagen rutas.
    + Se ha corregido la construcción de URL de Administración de modelos para la región Azure China 21Vianet.
    + Se corrigió un problema en el que los modelos que usan source_dir no se podían empaquetar para Azure Functions.    
    + Se ha agregado una opción a [Environment.build_local ()](/python/api/azureml-core/azureml.core.environment.environment) para introducir una imagen en el registro de contenedor del área de trabajo de AzureML
    + Se actualizó el SDK para usar la nueva biblioteca de tokens en Azure Synapse de forma que sea compatible con versiones anteriores.
  + **azureml-interpret**
    + Se corrigió un error en el que no se devolvió ninguno cuando no había ninguna explicación disponible para su descarga. Ahora genera una excepción, que coincide con otro lugar.
  + **azureml-pipeline-steps**
    + No se permite pasar `DatasetConsumptionConfig`s al parámetro `Estimator` de `inputs` cuando se utiliza el `Estimator` en una `EstimatorStep`.
  + **azureml-sdk**
    + Se ha agregado el cliente AutoML al paquete azureml-SDK, lo que permite que se envíen los AutoML remotos sin necesidad de instalar el paquete completo de AutoML.
  + **azureml-train-automl-client**
    + Se corrigió la alineación en la salida de la consola para ejecuciones de AutoML.
    + Se corrigió un error que hacía que la versión incorrecta de pandas se instalara en amlcompute remoto.

## <a name="2019-12-23"></a>2019-12-23

### <a name="azure-machine-learning-sdk-for-python-v1081"></a>SDK de Azure Machine Learning para Python v 1.0.81

+ **Mejoras y correcciones de errores**
  + **azureml-contrib-interpret**
    + aplazar la dependencia de la forma a interpretar-comunidad de azureml-interpretar
  + **azureml-core**
    + El destino de proceso ahora se puede especificar como un parámetro para los objetos de configuración de implementación correspondientes. Es específicamente el nombre del destino de proceso en el que se va a realizar la implementación, no el objeto SDK.
    + Se agregó información de CreatedBy al modelo y a los objetos de servicio. Se puede obtener acceso a través de .created_by.
    + Se corrigió ContainerImage.run(), que no configuraba correctamente el puerto HTTP del contenedor de Docker.
    + Se ha hecho que `azureml-dataprep` sea opcional para el comando de la CLI `az ml dataset register`.
    + Se corrigió un error por el cual `TabularDataset.to_pandas_dataframe` volvía incorrectamente a un lector alternativo e imprimía una advertencia.
  + **azureml-explain-model**
    + aplazar la dependencia de la forma a interpretar-comunidad de azureml-interpretar
  + **azureml-pipeline-core**
    + Se ha agregado un nuevo paso de canalización `NotebookRunnerStep` para ejecutar un cuaderno local como un paso de la canalización.
    + Se quitaron funciones desusadas get_all para PublishedPipelines, programaciones y PipelineEndpoints
  + **azureml-train-automl-client**
    + Se inició el desuso de data_script como entrada para AutoML.


## <a name="2019-12-09"></a>2019-12-09

### <a name="azure-machine-learning-sdk-for-python-v1079"></a>SDK de Azure Machine Learning para Python v1.0.79

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se quitó el registro de featurizationConfig
      + Registro actualizado para registrar solo "auto"/"off"/"customized".
  + **azureml-automl-runtime**
    + Se agregó compatibilidad con pandas. Series y pandas. Categoría para detectar el tipo de datos de la columna. Anteriormente solo se admitía numpy.ndarray.
      + Se agregaron cambios de código relacionados para administrar dtype categórico correctamente.
    + Se ha mejorado la interfaz de la función de pronóstico: el parámetro y_pred se hizo opcional. -Se mejoraron las propiedades docstring.
  + **azureml-contrib-dataset**
    + Se corrigió un error donde los conjuntos de datos con etiqueta no se podían montar.
  + **azureml-core**
    + Corrección de errores de `Environment.from_existing_conda_environment(name, conda_environment_name)`. El usuario puede crear una instancia del entorno que sea una réplica exacta del entorno local.
    + Se cambiaron los métodos de conjuntos de datos relacionados con series temporales por `include_boundary=True` de forma predeterminada.
  + **azureml-train-automl-client**
    + Se corrigió un problema donde los resultados de validación no se imprimían cuando show output se establecía en false.


## <a name="2019-11-25"></a>2019-11-25

### <a name="azure-machine-learning-sdk-for-python-v1076"></a>SDK de Azure Machine Learning para Python v1.0.76

+ **Cambios importantes**
  + Problemas de actualización de Azureml-Train-AutoML
    + La actualización a una versión superior o igual a azureml-train-automl 1.0.76 desde una versión inferior puede generar instalaciones parciales, lo que provocaría el error de algunas importaciones de AutoML. Para solucionar este problema, puede ejecutar el script de configuración que se encuentra en https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/automl_setup.cmd. O bien, si usa PIP directamente, puede usar lo siguiente:
      + "pip install --upgrade azureml-train-automl"
      + "pip install --ignore-installed azureml-train-automl-client"
    + También puede desinstalar la versión anterior antes de actualizar:
      + "pip uninstall azureml-train-automl"
      + "pip install azureml-train-automl"

+ **Mejoras y correcciones de errores**
  + **azureml-automl-runtime**
    + AutoML ahora tendrá en cuenta las clases true y false al calcular las métricas escalares medias para tareas de clasificación binaria.
    + Se ha trasladado el código de entrenamiento y el aprendizaje automático en AzureML-AutoML-Core a un nuevo paquete, AzureML-AutoML-Runtime.
  + **azureml-contrib-dataset**
    + Cuando llama a `to_pandas_dataframe` en un conjunto de datos con etiqueta con la opción de descarga, ahora ya puede especificar si desea sobrescribir los archivos existentes o no.
    + Cuando se llama a `keep_columns` o `drop_columns` y el resultado es la eliminación de una columna de serie temporal, etiqueta o imagen, también se eliminan las funcionalidades correspondientes del conjunto de datos.
    + Se ha corregido un problema con el cargador de PyTorch para la tarea de detección de objetos.
  + **azureml-contrib-interpret**
    + Se ha quitado el widget de panel de explicación de azureml-contrib-interpretate; se ha cambiado el paquete para hacer referencia al nuevo en interpret_community.
    + Se ha actualizado la versión de interpret-community a 0.2.0.
  + **azureml-core**
    + Mejora del rendimiento de `workspace.datasets`.
    + Se ha agregado la capacidad de registrar el almacén de datos de Azure SQL Database mediante la autenticación de nombre de usuario y contraseña.
    + Corrección para cargar RunConfigurations desde rutas de acceso relativas.
    + Cuando se llama a `keep_columns` o `drop_columns` y el resultado es la eliminación de una columna de serie temporal, también se eliminan las funcionalidades correspondientes del conjunto de datos.
  + **azureml-interpret**
    + Se ha actualizado la versión de interpret-community a 0.2.0
  + **azureml-pipeline-steps**
    + Se han documentado los valores admitidos para `runconfig_pipeline_params` para los pasos de canalización de Azure Machine Learning.
  + **azureml-pipeline-core**
    + Se ha agregado la opción de CLI para descargar la salida en formato JSON para comandos de canalización.
  + **azureml-train-automl**
    + Se ha dividido AzureML-Train-AutoML en dos paquetes, un paquete de cliente AzureML-Train-AutoML-Client y un paquete de entrenamiento de Machine Learning AzureML-Train-AutoML-Runtime.
  + **azureml-train-automl-client**
    + Se ha agregado un cliente ligero para enviar experimentos de AutoML sin necesidad de instalar localmente dependencias de aprendizaje automático.
    + Se ha corregido el registro de retardos detectados automáticamente, tamaños de ventanas con desplazamiento y horizontes máximos en ejecuciones remotas.
  + **azureml-train-automl-runtime**
    + Se ha agregado un nuevo paquete AutoML para aislar los componentes de aprendizaje automático y tiempo de ejecución del cliente.
  + **azureml-contrib-train-rl**
    + Se ha agregado compatibilidad con aprendizaje de refuerzo en SDK.
    + Se ha agregado compatibilidad con AmlWindowsCompute en el SDK de RL.


## <a name="2019-11-11"></a>2019-11-11

### <a name="azure-machine-learning-sdk-for-python-v1074"></a>SDK de Azure Machine Learning para Python v1.0.74

  + **Características en versión preliminar**
    + **azureml-contrib-dataset**
      + Después de importar azureml-contrib-dataset, puede llamar a `Dataset.Labeled.from_json_lines` en lugar de `._Labeled` para crear un conjunto de datos con etiqueta.
      + Cuando llama a `to_pandas_dataframe` en un conjunto de datos con etiqueta con la opción de descarga, ahora ya puede especificar si desea sobrescribir los archivos existentes o no.
      + Cuando se llama a `keep_columns` o `drop_columns` y el resultado es la eliminación de una columna de serie temporal, etiqueta o imagen, también se eliminan las funcionalidades correspondientes del conjunto de datos.
      + Se han solucionado los problemas con el cargador de PyTorch cuando se llama a `dataset.to_torchvision()`.

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Se ha agregado la generación de perfiles de modelos a la versión preliminar de la CLI.
    + Soluciona un cambio importante en Azure Storage que provocaba que la CLI de Azure ML tuviera un error.
    + Se ha agregado un tipo de Load Balancer a MLC para tipos de AKS
  + **azureml-automl-core**
    + Se ha solucionado el problema con la detección del horizonte máximo de la serie temporal por el que faltaban valores y varios intervalos.
    + Se ha corregido el problema de los errores que se producían durante la generación de divisiones de validación cruzada.
    + Reemplace esta sección por un mensaje en formato Markdown para que aparezca en las notas de la versión: control mejorado de los intervalos cortos de los conjuntos de datos de previsión.
    + Se ha solucionado el problema con el enmascaramiento de parte de la información de usuario durante el registro. Se ha mejorado el registro de errores durante la ejecución de previsiones.
    + Se ha agregado Psutil como dependencia de Conda al archivo de implementación yml generado automáticamente.
  + **azureml-contrib-mir**
    + Soluciona un cambio importante en Azure Storage que provocaba que la CLI de Azure ML tuviera un error.
  + **azureml-core**
    + Se ha solucionado un error que provocaba que los modelos implementados en Azure Functions produjeran errores 500.
    + Se ha solucionado un problema que hacía que el archivo amlignore no se aplicara a las instantáneas.
    + Se ha agregado una nueva API amlcompute.get_active_runs que devuelve un generador para la ejecución y las ejecuciones en cola en un amlcompute determinado.
    + Se ha agregado un tipo de Load Balancer a MLC para tipos de AKS.
    + Se ha agregado un parámetro booleano append_prefix a download_files en run.py y a download_artifacts_from_prefix en artifacts_client. Esta marca se usa para simplificar selectivamente la ruta de acceso al archivo original, con lo que solo se agrega el nombre de archivo o carpeta a output_directory
    + Se ha solucionado el problema de deserialización de `run_config.yml` con el uso del conjunto de datos.
    + Cuando se llama a `keep_columns` o `drop_columns` y el resultado es la eliminación de una columna de serie temporal, también se eliminan las funcionalidades correspondientes del conjunto de datos.
  + **azureml-interpret**
    + Se ha actualizado interpretar la versión de interpret-community a 0.1.0.3
  + **azureml-train-automl**
    + Se ha solucionado un problema por el que en automl_step no se imprimían los problemas de validación.
    + Se ha solucionado un problema de register_model para que pueda completarse correctamente incluso si faltan dependencias localmente en el entorno del modelo.
    + Se ha solucionado un problema que hacía que algunas ejecuciones remotas no estuvieran habilitadas para Docker.
    + Agregue el registro de la excepción que está causando un error en una ejecución local prematuramente.
  + **azureml-train-core**
    + Tenga en cuenta las ejecuciones de resume_from en el cálculo de las mejores ejecuciones secundarias de optimización de hiperparámetros automatizada.
  + **azureml-pipeline-core**
    + Se ha corregido el control de parámetros en la construcción del argumento de una canalización.
    + Se ha agregado una descripción de la canalización y un parámetro yaml de tipo de paso.
    + Hay un nuevo formato yaml para el paso de canalización y se ha agregado una advertencia de desuso para el formato anterior.



## <a name="2019-11-04"></a>2019-11-04

### <a name="web-experience"></a>Experiencia web

La página de aterrizaje del área de trabajo de colaboración en [https://ml.azure.com](https://ml.azure.com) se ha mejorado y renombrado como Azure Machine Learning Studio.

Desde Studio, puede entrenar, probar, implementar y administrar recursos de Azure Machine Learning, como conjuntos de datos, canalizaciones, modelos, puntos de conexión y mucho más.

Acceda a las siguientes herramientas de creación basadas en web desde Studio:

| Herramienta basada en web | Descripción | 
|-|-|-|
| Máquina virtual de Notebook (versión preliminar) | Estación de trabajo totalmente administrada basada en la nube | 
| [Aprendizaje automático automatizado](tutorial-first-experiment-automated-ml.md) (versión preliminar) | Experiencia sin código para automatizar el desarrollo de modelos de aprendizaje automático | 
| [Designer](concept-designer.md) | Herramienta de modelado de aprendizaje automático de arrastrar y colocar conocida anteriormente como la interfaz visual | 


### <a name="azure-machine-learning-designer-enhancements"></a>Mejoras del diseñador de Azure Machine Learning

+ Anteriormente conocido como la interfaz visual 
+    11 nuevos [módulos](algorithm-module-reference/module-reference.md) que incluyen utilidades de recomendación, clasificación y entrenamiento, como la ingeniería de características, la validación cruzada y la transformación de datos.

### <a name="r-sdk"></a>SDK de R 
 
Los científicos de datos y los desarrolladores de inteligencia artificial usan el [SDK de Azure Machine Learning para R](https://github.com/Azure/azureml-sdk-for-r) para crear y ejecutar flujos de trabajo de aprendizaje automático con Azure Machine Learning.

El SDK de Azure Machine Learning para R usa el paquete de `reticulate` para enlazar con el SDK de Python. Al enlazar directamente a Python, el SDK para R permite acceder a los objetos y métodos principales implementados en el SDK de Python desde cualquier entorno de R que elija.

Entre las principales capacidades del SDK se incluyen:

+    Administre los recursos en la nube para la supervisión, registro y organización de los experimentos de aprendizaje automático.
+    Entrenar modelos mediante recursos en la nube, incluido el entrenamiento del modelo de aceleración por GPU.
+    Implementar sus modelos como servicios web en Azure Container Instances (ACI) y Azure Kubernetes Service (AKS).

Para obtener la documentación completa, vaya al [sitio web del paquete](https://azure.github.io/azureml-sdk-for-r).

### <a name="azure-machine-learning-integration-with-event-grid"></a>Integración de Azure Machine Learning con Event Grid 

Azure Machine Learning es ahora un proveedor de recursos para Event Grid, puede configurar eventos de aprendizaje automático mediante el Azure Portal o el CLI de Azure. Los usuarios pueden crear eventos para la finalización de la ejecución, el registro del modelo, la implementación de modelos y el desfase de datos detectado. Estos eventos se pueden enrutar a los controladores de eventos admitidos por Event Grid para su consumo. Consulte los artículos del [esquema](../event-grid/event-schema-machine-learning.md) y el [tutorial](how-to-use-event-grid.md) del evento de aprendizaje automático para obtener más detalles.

## <a name="2019-10-31"></a>2019-10-31

### <a name="azure-machine-learning-sdk-for-python-v1072"></a>SDK de Azure Machine Learning para Python v1.0.72

+ **Nuevas características:**
  + Se han agregado supervisores de conjuntos de datos mediante el paquete [**azureml-datadrift**](/python/api/azureml-datadrift), que permite supervisar a lo largo del tiempo el desfase de datos u otros cambios estadísticos en los conjuntos de datos de serie temporal. Se pueden desencadenar alertas y eventos si se detecta un desfase o se cumplen otras condiciones en los datos. Consulte [nuestra documentación](how-to-monitor-datasets.md) para más información.
  + Anuncio de dos nuevas ediciones (también denominadas SKU indistintamente) en Azure Machine Learning. Con esta versión, puede crear un área de trabajo con el nivel Básico o Enterprise de Azure Machine Learning. Todas las áreas de trabajo existentes se establecerán de forma predeterminada en la edición Basic y puede ir a Azure Portal o Studio para actualizar el área de trabajo en cualquier momento. Puede crear un área de trabajo Basic o Enterprise desde Azure Portal. Lea [nuestra documentación](./how-to-manage-workspace.md) para obtener más información. Desde el SDK, se puede determinar la edición del área de trabajo mediante la propiedad "sku" del objeto de área de trabajo.
  + También hemos mejorado el proceso de Azure Machine Learning: ahora se pueden ver las métricas de los clústeres (como el total de nodos, los nodos en ejecución o la cuota total del núcleo) en Azure Monitor, además de ver los registros de diagnóstico para la depuración. Además, también puede ver las ejecuciones activas o en cola actualmente en el clúster y los detalles, como las direcciones IP de los distintos nodos del clúster. Puede verlos en el portal o mediante las funciones correspondientes en el SDK o la CLI.

  + **Características en versión preliminar**
    + Estamos lanzando soporte técnico para la versión preliminar del cifrado de disco de la SSD local en el proceso de Azure Machine Learning. Genere una incidencia de soporte técnico para que su suscripción se incluya en la lista de permitidos para usar esta característica.
    + Versión preliminar pública de la inferencia por lotes de Azure Machine Learning. La inferencia por lotes de Azure Machine Learning tiene como destino trabajos de inferencia grandes que no dependen del tiempo. La inferencia por lotes proporciona un escalado del proceso de inferencia rentable, con un rendimiento sin precedentes para aplicaciones asincrónicas. Está optimizada para la inferencia de alto rendimiento, de tipo "fire-and-forget" (envíelo y olvídese), en colecciones de datos de gran tamaño.
    + [**azureml-contrib-dataset**](/python/api/azureml-contrib-dataset)
        + Habilitación de funcionalidades para el conjunto de datos etiquetado
        ```Python
        import azureml.core
        from azureml.core import Workspace, Datastore, Dataset
        import azureml.contrib.dataset
        from azureml.contrib.dataset import FileHandlingOption, LabeledDatasetTask

        # create a labeled dataset by passing in your JSON lines file
        dataset = Dataset._Labeled.from_json_lines(datastore.path('path/to/file.jsonl'), LabeledDatasetTask.IMAGE_CLASSIFICATION)

        # download or mount the files in the `image_url` column
        dataset.download()
        dataset.mount()

        # get a pandas dataframe
        from azureml.data.dataset_type_definitions import FileHandlingOption
        dataset.to_pandas_dataframe(FileHandlingOption.DOWNLOAD)
        dataset.to_pandas_dataframe(FileHandlingOption.MOUNT)

        # get a Torchvision dataset
        dataset.to_torchvision()
        ```

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + La CLI ahora admite el empaquetado de modelos.
    + Agregación de una CLI de conjunto de datos. Para obtener más información: `az ml dataset --help`
    + Agregación de compatibilidad para la implementación y el empaquetado de modelos admitidos (ONNX, scikit-learn y TensorFlow) sin una instancia de InferenceConfig.
    + Agregación de una marca de sobrescritura para la implementación del servicio (ACI y AKS) en el SDK y la CLI. Si se proporciona, sobrescribirá el servicio existente si ya existe un servicio con ese nombre. Si el servicio no existe, creará un servicio nuevo.
    + Los modelos se pueden registrar con dos nuevos marcos, Onnx y TensorFlow. El registro del modelo acepta los datos de entrada de ejemplo, los datos de salida de ejemplo y la configuración de recursos para el modelo.
  + **azureml-automl-core**
    + El entrenamiento de una iteración se ejecutará en un proceso secundario solo cuando se establecen restricciones en tiempo de ejecución.
    + Agregación de una medida de protección para las tareas de previsión, para comprobar si un elemento max_horizon especificado producirá o no un problema de memoria en el equipo correspondiente. Si es así, se mostrará un mensaje de medida de protección.
    + Se ha agregado compatibilidad con frecuencias complejas como, por ejemplo, dos años y un mes. Se ha agregado un mensaje de error comprensible si no se puede determinar la frecuencia.
    + Agregación de un elemento azureml-defaults al entorno Conda generado automáticamente para resolver el error de implementación de modelo.
    + Se permite que los datos intermedios de la canalización de Azure Machine Learning se conviertan en un conjunto de datos tabular y se usen en `AutoMLStep`.
    + Implementación de la actualización del propósito de columna para streaming.
    + Implementación de la actualización de parámetros de transformador para Imputer y HashOneHotEncoder para streaming.
    + Agregación del tamaño de datos actual y el tamaño de datos mínimo necesario para los mensajes de error de validación.
    + Actualización del tamaño de datos mínimo necesario para la validación cruzada a fin de garantizar un mínimo de dos muestras en cada iteración de validación.
  + **azureml-cli-common**
    + La CLI ahora admite el empaquetado de modelos.
    + Los modelos se pueden registrar con dos nuevos marcos, Onnx y TensorFlow.
    + El registro del modelo acepta los datos de entrada de ejemplo, los datos de salida de ejemplo y la configuración de recursos para el modelo.
  + **azureml-contrib-gbdt**
    + Corrección del canal de versión del cuaderno
    + Se ha agregado una advertencia para destinos de proceso que no son de AmlCompute que no se admiten.
    + Agregación del estimador LightGMB al paquete azureml-contrib-gbdt
  + [**azureml-core**](/python/api/azureml-core)
    + La CLI ahora admite el empaquetado de modelos.
    + Agregación de una advertencia de desuso para las API de conjunto de los conjuntos de datos. Consulte el aviso de cambio de API de conjunto de datos en https://aka.ms/tabular-dataset.
    + Cambie [`Dataset.get_by_id`](/python/api/azureml-core/azureml.core.dataset%28class%29#get-by-id-workspace--id-) para devolver el nombre y la versión del registro si el conjunto de datos está registrado.
    + Corrección del error por el que ScriptRunConfig con el conjunto de datos como argumento no se puede usar repetidamente para enviar la ejecución del experimento.
    + Se realizará un seguimiento de los conjuntos de datos recuperados durante una ejecución y estos se pueden ver en la página de detalles de la ejecución, o bien mediante una llamada a [`run.get_details()`](/python/api/azureml-core/azureml.core.run%28class%29#get-details--) una vez completada la ejecución.
    + Permita que los datos intermedios de la canalización de Azure Machine Learning se conviertan en un conjunto de datos tabular y que se usen en [`AutoMLStep`](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlstep).
    + Agregación de compatibilidad para la implementación y el empaquetado de modelos admitidos (ONNX, scikit-learn y TensorFlow) sin una instancia de InferenceConfig.
    + Agregación de una marca de sobrescritura para la implementación del servicio (ACI y AKS) en el SDK y la CLI. Si se proporciona, sobrescribirá el servicio existente si ya existe un servicio con ese nombre. Si el servicio no existe, creará un servicio nuevo.
    +  Los modelos se pueden registrar con dos nuevos marcos, Onnx y TensorFlow. El registro del modelo acepta los datos de entrada de ejemplo, los datos de salida de ejemplo y la configuración de recursos para el modelo.
    + Agregación de un nuevo almacén de datos para Azure Database for MySQL. Agregación de un ejemplo para usar Azure Database for MySQL en DataTransferStep en las canalizaciones de Azure Machine Learning.
    + Agregación de la funcionalidad para agregar y quitar etiquetas de experimentos. Agregación de la funcionalidad para quitar etiquetas de las ejecuciones
    + Agregación de una marca de sobrescritura para la implementación del servicio (ACI y AKS) en el SDK y la CLI. Si se proporciona, sobrescribirá el servicio existente si ya existe un servicio con ese nombre. Si el servicio no existe, creará un servicio nuevo.
  + [**azureml-datadrift**](/python/api/azureml-datadrift)
    + Se ha movido de `azureml-contrib-datadrift` a `azureml-datadrift`
    + Se ha agregado compatibilidad con la supervisión de desfases y otras medidas estadísticas en conjuntos de datos de serie temporal.
    + Los nuevos métodos `create_from_model()` y `create_from_dataset()` de la clase [`DataDriftDetector`](/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector%28class%29). Método `create()` en desuso.
    + Ajustes en las visualizaciones en Python y en la interfaz de usuario de Azure Machine Learning Studio.
    + Compatibilidad de la programación de supervisión semanal y mensual, además de la diaria para los supervisores de conjunto de datos.
    + Compatibilidad de la reposición de las métricas de supervisor de datos para analizar los datos históricos de los supervisores de conjunto de datos.
    + Varias correcciones de errores
  + [**azureml-pipeline-core**](/python/api/azureml-pipeline-core)
    + azureml-dataprep ya no es necesario para enviar una ejecución de canalización de Azure Machine Learning desde el archivo de canalización `yaml`.
  + [**azureml-train-automl**](/python/api/azureml-train-automl-runtime/)
    + Agregación de un elemento azureml-defaults al entorno Conda generado automáticamente para resolver el error de implementación de modelo.
    + El aprendizaje remoto de AutoML ahora incluye azureml-defaults para permitir la reutilización del entorno de aprendizaje para la inferencia.
  + **azureml-train-core**
    + Agregación de compatibilidad con PyTorch 1.3 en el calculador de [`PyTorch`](/python/api/azureml-train-core/azureml.train.dnn.pytorch)

## <a name="2019-10-21"></a>2019-10-21

### <a name="visual-interface-preview"></a>Interfaz visual (versión preliminar)

+ La interfaz visual de Azure Machine Learning (versión preliminar) se ha revisado para ejecutarse en [canalizaciones de Azure Machine Learning](concept-ml-pipelines.md). Las canalizaciones (anteriormente conocidas como experimentos) creadas en la interfaz visual ahora están totalmente integradas con la experiencia principal de Azure Machine Learning.
  + Experiencia de administración unificada con recursos de SDK
  + Control de versiones y seguimiento de modelos de interfaz visual, canalizaciones y puntos de conexión
  + Interfaz de usuario rediseñada
  + Agregación de implementación de inferencia por lotes
  + Agregación de compatibilidad con Azure Kubernetes Service (AKS) para los destinos de proceso de inferencia
  + Nuevo flujo de trabajo de creación de canalizaciones de paso de Python
  + Nueva [página de aterrizaje](https://ml.azure.com) para herramientas de creación visual

+ **Nuevos módulos**
  + Aplicación de operación matemática
  + Aplicación de transformaciones de SQL
  + Recorte de valores
  + Resumen de datos
  + Importar desde SQL Database

## <a name="2019-10-14"></a>2019-10-14

### <a name="azure-machine-learning-sdk-for-python-v1069"></a>SDK de Azure Machine Learning para Python v1.0.69

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Limitación de las explicaciones de modelo a la mejor ejecución en vez de calcular las explicaciones para cada ejecución. Cambio de este comportamiento para el modo local, remoto y ADB.
    + Se ha agregado compatibilidad con las explicaciones del modelo a petición para la interfaz de usuario.
    + Agregación de psutil como dependencia de `automl` e inclusión de psutil como una dependencia de Conda en amlcompute.
    + Se corrigió el problema con intervalos heurísticos y los tamaños de ventanas graduales en los conjuntos de datos de previsión de los cuales algunas series pueden provocar errores de álgebra lineal.
      + Se ha agregado la impresión para los parámetros que se han determinado heurísticamente en las ejecuciones de previsión.
  + **azureml-contrib-datadrift**
    + Se ha agregado protección al crear métricas de salida si el desfase del nivel de conjunto de resultados no está en la primera sección.
  + **azureml-contrib-interpret**
    + Se ha cambiado el nombre del paquete azureml-contrib-explain a azureml-contrib-interpret.
  + **azureml-core**
    + Se ha agregado una API para cancelar el registro de conjuntos de registros. `dataset.unregister_all_versions()`
    + Se ha cambiado el nombre del paquete azureml-contrib-explain-model a azureml-contrib-interpret.
  + **[azureml-core](/python/api/azureml-core)**
    + Se ha agregado una API para cancelar el registro de conjuntos de registros. dataset.[unregister_all_versions()](/python/api/azureml-core/azureml.data.abstract_datastore.abstractdatastore#unregister--).
    + Se ha agregado una API de conjunto de datos para comprobar la hora del cambio de los datos. `dataset.data_changed_time`.
    + Capacidad de consumir `FileDataset` y `TabularDataset` como entradas para `PythonScriptStep`, `EstimatorStep` y `HyperDriveStep` en la canalización de Azure Machine Learning.
    + Se ha mejorado el rendimiento de `FileDataset.mount` para las carpetas con un gran número de archivos.
    + Posibilidad de usar [FileDataset](/python/api/azureml-core/azureml.data.filedataset) y [TabularDataset](/python/api/azureml-core/azureml.data.tabulardataset) como entradas para [PythonScriptStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep), [EstimatorStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep) y [HyperDriveStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyperdrivestep) en la canalización de Azure Machine Learning.
    + Se ha mejorado el rendimiento de FileDataset.[mount()](/python/api/azureml-core/azureml.data.filedataset#mount-mount-point-none----kwargs-) para las carpetas con un gran número de archivos.
    + Se ha agregado la dirección URL a las recomendaciones de errores conocidas en los detalles de ejecución.
    + Se ha corregido un error en la ejecución run.get_metrics por el cual las solicitudes darían error si una ejecución tuviera demasiados elementos secundarios.
    + Se ha corregido un error en [run.get_metrics](/python/api/azureml-core/azureml.core.run.run#get-metrics-name-none--recursive-false--run-type-none--populate-false-) por el cual las solicitudes darían error si una ejecución tuviera demasiados elementos secundarios.
    + Se ha agregado compatibilidad para la autenticación en el clúster de Arcadia.
    + La creación de un objeto de experimento obtiene o crea el experimento en el área de trabajo Azure Machine Learning para el seguimiento del historial de ejecución. El id. de experimento y el tiempo de archivado se rellenan en el objeto de experimento al crearse. Ejemplo: experiment = Experiment(workspace, "New Experiment") experiment_id = experiment.id archive() y reactivate() son funciones a las que se puede llamar en un experimento para ocultar y volver a mostrar el experimento en la experiencia de usuario o para mostrarlo de forma predeterminada en una llamada para enumerar experimentos. Si se crea un nuevo experimento con el mismo nombre que un experimento archivado, puede cambiar el nombre del experimento archivado al reactivarlo proporcionando un nuevo nombre. Solo puede haber un experimento activo con un nombre determinado. Ejemplo: experiment1 = Experiment(workspace, "Active Experiment") experiment1.archive() # Crear un nuevo experimento activo con el mismo nombre que el archivado. experiment2. = Experiment(workspace, "Active Experiment") experiment1.reactivate(new_name="Previous Active Experiment") La lista de métodos estáticos () del experimento puede tomar un filtro de nombre y un filtro ViewType. Los valores ViewType son "ACTIVE_ONLY", "ARCHIVED_ONLY" y "ALL" Ejemplo: archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY") all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
    + Compatibilidad con el uso del entorno para la implementación de modelos y la actualización del servicio
  + **azureml-datadrift**
    + El atributo show de la clase DataDriftDector ha dejado de ser compatible con el argumento opcional "with_details". El atributo show solo presentará el coeficiente de desfase de datos y la contribución del desfase de datos de las columnas de características.
    + Cambios de comportamiento del atributo "get_output" de DataDriftDetector:
      + Los parámetros de entrada start_time, end_time son opcionales en lugar de obligatorios;
      + La especificación de un valor start_time o end_time particular con un run_id determinado en la misma invocación provocará una excepción de error de valor porque son mutualmente excluyentes.
      + Al indicar un valor específico de start_time o end_time, solo se devolverán los resultados de las ejecuciones programadas.
      + El parámetro "daily_latest_only" está en desuso.
    + Compatibilidad con la recuperación de salidas de desfase de datos basadas en conjuntos de datos.
  + **azureml-explain-model**
    + Cambia el nombre del paquete AzureML-explain-model a AzureML-interpret, manteniendo por ahora el paquete anterior por compatibilidad con versiones anteriores.
    + Se corrige el error `automl` consistente en la presencia de explicaciones sin procesar en la tarea de clasificación en lugar de regresión de forma predeterminada en la descarga de ExplanationClient.
    + Se agregó compatibilidad con `ScoringExplainer` para que se cree directamente con `MimicWrapper`.
  + **azureml-pipeline-core**
    + Se mejoró el rendimiento para la creación de canalizaciones de gran tamaño.
  + **azureml-train-core**
    + Se agregó compatibilidad con TensorFlow 2.0 en un estimador de TensorFlow.
  + **azureml-train-automl**
    + La creación de un objeto [Experimento](/python/api/azureml-core/azureml.core.experiment.experiment) obtiene o crea el experimento en el área de trabajo Azure Machine Learning para el seguimiento del historial de ejecución. El id. de experimento y el tiempo de archivado se rellenan en el objeto de experimento al crearse. Ejemplo:

        ```python
        experiment = Experiment(workspace, "New Experiment")
        experiment_id = experiment.id
        ```
        [archive()](/python/api/azureml-core/azureml.core.experiment.experiment#archive--) y [reactivate()](/python/api/azureml-core/azureml.core.experiment.experiment#reactivate-new-name-none-) son funciones a las que se puede llamar en un experimento para ocultar y volver a mostrar el experimento en la experiencia de usuario o para devolverlo de forma predeterminada en una llamada para enumerar experimentos. Si se crea un nuevo experimento con el mismo nombre que un experimento archivado, puede cambiar el nombre del experimento archivado al reactivarlo proporcionando un nuevo nombre. Solo puede haber un experimento activo con un nombre determinado. Ejemplo:

        ```python
        experiment1 = Experiment(workspace, "Active Experiment")
        experiment1.archive()
        # Create new active experiment with the same name as the archived.
        experiment2 = Experiment(workspace, "Active Experiment")
        experiment1.reactivate(new_name="Previous Active Experiment")
        ```
        El método estático [list()](/python/api/azureml-core/azureml.core.experiment.experiment#list-workspace--experiment-name-none--view-type--activeonly---tags-none-) en el experimento puede tomar un filtro de nombre y un filtro ViewType. Los valores ViewType son "ACTIVE_ONLY", "ARCHIVED_ONLY" y "ALL". Ejemplo:

        ```python
        archived_experiments = Experiment.list(workspace, view_type="ARCHIVED_ONLY")
        all_first_experiments = Experiment.list(workspace, name="First Experiment", view_type="ALL")
        ```
    + Compatibilidad con el uso del entorno para la implementación de modelos y la actualización del servicio.
  + **[azureml-datadrift](/python/api/azureml-datadrift)**
    + El atributo show de la clase [DataDriftDetector](/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector) ha dejado de ser compatible con el argumento opcional "with_details". El atributo show solo presentará el coeficiente de desfase de datos y la contribución del desfase de datos de las columnas de características.
    + Cambios de comportamiento de la función DataDriftDetector [get_output]python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#get-output-start-time-none--end-time-none--run-id-none-):
      + Los parámetros de entrada start_time, end_time son opcionales en lugar de obligatorios;
      + La especificación de un valor start_time o end_time particular con un run_id determinado en la misma invocación provocará una excepción de error de valor porque son mutualmente excluyentes.
      + Al indicar un valor específico de start_time o end_time, solo se devolverán los resultados de las ejecuciones programadas.
      + El parámetro "daily_latest_only" está en desuso.
    + Compatibilidad con la recuperación de salidas de desfase de datos basadas en conjuntos de datos.
  + **azureml-explain-model**
    + Agregación de compatibilidad con [ScoringExplainer](/python/api/azureml-interpret/azureml.interpret.scoring.scoring_explainer.scoringexplainer) que se creará directamente con MimicWrapper
  + **[azureml-pipeline-core](/python/api/azureml-pipeline-core)**
    + Mejora del rendimiento para la creación de canalizaciones de gran tamaño.
  + **[azureml-train-core](/python/api/azureml-train-core)**
    + Agregación de compatibilidad con TensorFlow 2.0 en un estimador de [TensorFlow](/python/api/azureml-train-core/azureml.train.dnn.tensorflow).
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + Ya no se producirá un error en la ejecución primaria cuando se produzca un error en la iteración de instalación, ya que la orquestación se encarga de ello.
    + Se agregó compatibilidad con local-docker y local-conda para los experimentos de AutoML.
    + Agregación de compatibilidad con local-docker y local-conda para los experimentos de AutoML.


## <a name="2019-10-08"></a>2019-10-08

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Nueva experiencia web (versión preliminar) para áreas de trabajo de Azure Machine Learning

La pestaña Experimento del [nuevo portal de áreas de trabajo](https://ml.azure.com) se ha actualizado para que los científicos de datos puedan supervisar los experimentos de forma más eficaz. Puede explorar las características siguientes:
+ Los metadatos de experimentos para filtrar y ordenar fácilmente la lista de experimentos
+ Se han simplificado y mejorado las páginas de detalles de los experimentos que le permiten visualizar y comparar las ejecuciones.
+ Nuevo diseño para ejecutar páginas de detalles para comprender y supervisar las ejecuciones de entrenamiento

## <a name="2019-09-30"></a>2019-09-30

### <a name="azure-machine-learning-sdk-for-python-v1065"></a>SDK de Azure Machine Learning para Python v1.0.65

  + **Nuevas características:**
    + Incorporación de entornos mantenidos. Estos entornos se han configurado previamente con bibliotecas para tareas comunes de aprendizaje automático, y se han creado previamente y almacenado en caché como imágenes de Docker para que su ejecución sea más rápida. Aparecen de forma predeterminada en la lista de entornos del área de trabajo, con el prefijo "AzureML".
    + Incorporación de entornos mantenidos. Estos entornos se han configurado previamente con bibliotecas para tareas comunes de aprendizaje automático, y se han creado previamente y almacenado en caché como imágenes de Docker para que su ejecución sea más rápida. Aparecen de forma predeterminada en la lista de [Área de trabajo](/python/api/azureml-core/azureml.core.workspace%28class%29), con el prefijo "AzureML".

  + **azureml-train-automl**
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + Incorporación de la compatibilidad con la conversión de ONNX para ADB y HDI

+ **Características en versión preliminar**
  + **azureml-train-automl**
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + BERT y BiLSTM admitidos como texto caracterizador (solo versión preliminar)
    + Personalización de características admitida para los parámetros de propósito de columna y transformador (solo versión preliminar)
    + Admisión de las explicaciones sin procesar cuando el usuario habilita la explicación del modelo durante el entrenamiento (solo versión preliminar)
    + Incorporación de Prophet para la previsión de `timeseries` como una canalización que se pueda entrenar (solo versión preliminar)

  + **azureml-contrib-datadrift**
    + Paquetes reubicados de azureml-contrib-datadrift a azureml-datadrift; el paquete `contrib` se quitará en una versión futura

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Introducción de FeaturizationConfig a AutoMLConfig y AutoMLBaseSettings
    + Introducción de FeaturizationConfig en [AutoMLConfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig) y AutoMLBaseSettings
      + Invalidación del propósito de columna para características con la columna y el tipo de característica dados
      + Invalidación de los parámetros de transformador
    + Incorporación del mensaje de desuso para explain_model() y retrieve_model_explanations()
    + Incorporación de Prophet como una canalización que se puede entrenar (solo versión preliminar)
    + Incorporación del mensaje de desuso para explain_model() y retrieve_model_explanations().
    + Incorporación de Prophet como una canalización que se puede entrenar (solo versión preliminar).
    + Incorporación de la compatibilidad para la detección automática de los intervalos de destino, el tamaño de la ventana acumulada y el horizonte máximo. Si uno de los valores de target_lags, target_rolling_window_size o max_horizon está establecido como "automático", se aplicará la heurística para calcular el valor del parámetro correspondiente en base a los datos de entrenamiento.
    + Previsión fija en el caso de que el conjunto de datos contenga una columna de grano, este grano es de un tipo numérico y hay un intervalo entre el conjunto de entrenamiento y el de pruebas
    + Corrección del mensaje de error sobre el índice duplicado en la ejecución remota en las tareas de previsión
    + Previsión fija en el caso de que el conjunto de datos contenga una columna de grano, este grano es de un tipo numérico y hay un intervalo entre el conjunto de entrenamiento y el de pruebas.
    + Corrección del mensaje de error sobre el índice duplicado en la ejecución remota en las tareas de previsión.
    + Incorporación de una barrera protectora para comprobar si un conjunto de datos está desequilibrado o no. Si es así, se escribirá un mensaje de barrera protectora en la consola.
  + **azureml-core**
    + Incorporación de la capacidad de recuperar la dirección URL de SAS para el modelo en el almacenamiento mediante el objeto de modelo. Ejemplo: model.get_sas_url()
    + Introducción de `run.get_details()['datasets']` para obtener los conjuntos de datos asociados a la ejecución enviada
    + Incorporación de la API `Dataset.Tabular.from_json_lines_files` para crear un TabularDataset a partir de archivos de líneas JSON. Para obtener más información sobre estos datos tabulares en archivos de líneas JSON en TabularDataset, consulte la documentación de [este artículo](how-to-create-register-datasets.md).
    + Incorporación de campos de tamaño de máquina virtual adicionales (disco del sistema operativo, número de los GPU) a la función supported_vmsizes ()
    + Incorporación de campos adicionales a la función list_nodes () para mostrar la ejecución, las direcciones IP privada y pública, el puerto, etc.
    + Posibilidad de especificar un campo nuevo durante el aprovisionamiento de clústeres: remotelogin_port_public_access, que se puede establecer en habilitado o deshabilitado,dependiendo de si desea dejar el puerto SSH abierto o cerrado en el momento de crear el clúster. Si no lo especifica, el servicio abrirá o cerrará el puerto de forma inteligente, en función de si va a implementar el clúster dentro de una red virtual.
  + **azureml-explain-model**
  + **[azureml-core](/python/api/azureml-core/azureml.core)**
    + Incorporación de la capacidad de recuperar la dirección URL de SAS para el modelo en el almacenamiento mediante el objeto de modelo. Ex: model.[get_sas_url()](/python/api/azureml-core/azureml.core.model.model#get-sas-urls--)
    + Introducción de run.[get_details](/python/api/azureml-core/azureml.core.run%28class%29#get-details--)['datasets'] para obtener los conjuntos de datos asociados a la ejecución enviada
    + Incorporación de la API `Dataset.Tabular`.[from_json_lines_files()](/python/api/azureml-core/azureml.data.dataset_factory.tabulardatasetfactory#from-json-lines-files-path--validate-true--include-path-false--set-column-types-none--partition-format-none-) para crear un TabularDataset a partir de archivos de líneas JSON. Para obtener más información sobre estos datos tabulares en archivos de líneas JSON en TabularDataset, acuda a https://aka.ms/azureml-data para obtener documentación.
    + Incorporación de campos de tamaño de máquina virtual adicionales (disco del sistema operativo, número de GPU) a la función [supported_vmsizes()](/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-).
    + Incorporación de campos adicionales a la función [list_nodes()](/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#list-nodes--) para mostrar la ejecución, las direcciones IP privada y pública, el puerto, etc.
    + Posibilidad de especificar un campo nuevo durante el [aprovisionamiento](/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--) que se puede establecer en habilitado o deshabilitado, en función de si desea dejar el puerto SSH abierto o cerrado en el momento de crear el clúster. Si no lo especifica, el servicio abrirá o cerrará el puerto de forma inteligente, en función de si va a implementar el clúster dentro de una red virtual.
  + **azureml-explain-model**
    + Documentación mejorada para las salidas de explicación en el escenario de clasificación.
    + Incorporación de la capacidad de cargar los valores y previstos en la explicación de los ejemplos de evaluación. Desbloquea visualizaciones más útiles.
    + Incorporación de la propiedad de explicación a MimicWrapper para habilitar la obtención de MimicExplainer subyacente.
  + **azureml-pipeline-core**
    + Se agregó un cuaderno para describir Module, ModuleVersion y ModuleStep.
  + **azureml-pipeline-steps**
    + Se ha agregado RScriptStep para admitir la ejecución de scripts de R mediante la canalización de AML.
    + Se han corregido los parámetros de metadatos que se analizan en AzureBatchStep que provocaban el mensaje de error "assignment for parameter SubscriptionId is not specified" (no se ha especificado la asignación para el parámetro SubscriptionId).
  + **azureml-train-automl**
    + Compatibilidad de training_data, validation_data, label_column_name, weight_column_name como formato de entrada de datos
    + Incorporación del mensaje de desuso para explain_model() y retrieve_model_explanations()
  + **[azureml-pipeline-core](/python/api/azureml-pipeline-core)**
    + Se ha agregado un [cuaderno](https://aka.ms/pl-modulestep) para describir [Module](/python/api/azureml-pipeline-core/azureml.pipeline.core.module%28class%29), ModuleVersion y [ModuleStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep).
  + **[azureml-pipeline-steps](/python/api/azureml-pipeline-steps)**
    + Incorporación de [RScriptStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.rscriptstep) para admitir la ejecución del script R mediante la canalización de AML.
    + Se han corregido los parámetros de metadatos que se analizan en AzureBatchStep que provocaban el mensaje de error "assignment for parameter SubscriptionId is not specified" (no se ha especificado la asignación para el parámetro SubscriptionId).
  + **[azureml-train-automl](/python/api/azureml-train-automl-runtime/)**
    + Compatibilidad de training_data, validation_data, label_column_name, weight_column_name como formato de entrada de datos.
    + Incorporación del mensaje de desuso para [explain_model()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#explain-model-fitted-model--x-train--x-test--best-run-none--features-none--y-train-none----kwargs-) y [retrieve_model_explanations()](/python/api/azureml-train-automl-runtime/azureml.train.automl.runtime.automlexplainer#retrieve-model-explanation-child-run-).


## <a name="2019-09-16"></a>2019-09-16

### <a name="azure-machine-learning-sdk-for-python-v1062"></a>SDK de Azure Machine Learning para Python v1.0.62

+ **Nuevas características:**
  + Introducción del rasgo de `timeseries` en TabularDataset. Este rasgo permite filtrar fácilmente los datos de un objeto TabularDataset por las marcas de tiempo; por ejemplo, tomar todos los datos de un intervalo de tiempo o los datos más recientes.  https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/work-with-data/datasets-tutorial/timeseries-datasets/tabular-timeseries-dataset-filtering.ipynb para un cuaderno de ejemplo.
  + Se ha habilitado el entrenamiento con TabularDataset y FileDataset. 

  + **azureml-train-core**
      + Agregación de compatibilidad con `Nccl` y `Gloo` en el estimador de PyTorch

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Se ha dejado de usar el valor "lag_length" de AutoML y LaggingTransformer.
    + Se ha corregido la validación correcta de los datos de entrada si se especifican en formato de Dataflow.
    + Se ha modificado fit_pipeline.py para generar el JSON de grafo y cargarlo en los artefactos.
    + Representación del gráfico en `userrun` mediante `Cytoscape`.
  + **azureml-core**
    + Se ha revisado el control de excepciones en el código ADB y se han realizado cambios según el nuevo control de errores.
    + Se ha agregado la autenticación automática de MSI para máquinas virtuales de cuaderno.
    + Corrige el error en el que los modelos dañados o vacíos se podían cargar debido a reintentos con error.
    + Se ha corregido el error donde el nombre `DataReference` cambia cuando cambia el modo `DataReference` (por ejemplo, al llamar a `as_upload`, `as_download` o `as_mount`).
    + Puede permitir que `mount_point` y `target_path` sean opcionales para `FileDataset.mount` y `FileDataset.download`.
    + Se producirá la excepción de que no se encuentra la columna de marca de tiempo si se llama a la API relacionada con las series temporales sin la columna de marca de tiempo específica asignada o se quitan las columnas de marca de tiempo asignadas.
    + Las columnas de serie temporal se deben asignar con columnas cuyo tipo sea Fecha, si no, se espera una excepción.
    + Las columnas de serie temporal que asignan la API "with_timestamp_columns" pueden tomar el nombre de columna de marca de tiempo específica o general con un valor de Ninguno, que borrará las columnas de marca de tiempo asignadas previamente.
    + Se produce una excepción cuando la columna de marca de tiempo específica o general se elimina con la indicación al usuario de que la eliminación se puede realizar tras excluir la columna de marca de tiempo en la lista de eliminación o llamar a with_time_stamp con el valor Ninguno para liberar las columnas de marca de tiempo.
    + Se produce una excepción cuando la columna de marca de tiempo específica o general no se incluye en la lista de mantenimiento de columnas con la indicación al usuario de que el mantenimiento se puede realizar tras incluir la columna de marca de tiempo en la lista de mantenimiento o llamar a with_time_stamp con el valor Ninguno para liberar las columnas de marca de tiempo.
    + Se ha agregado el registro para el tamaño de un modelo registrado.
  + **azureml-explain-model**
    + Se ha corregido una advertencia impresa cuando el paquete de Python "packaging" no está instalado: "Using older than supported version of lightgbm, please upgrade to version greater than 2.2.1" (Se está usando una versión anterior a la admitida de LightGBM. Actualice a una versión superior a 2.2.1).
    + Se ha corregido la explicación del modelo de descarga con particionamiento en el caso de explicaciones globales con muchas características.
    + Se han corregido ejemplos de inicialización que faltan de Mimic Explainer en la explicación de salida.
    + Se ha corregido un error inmutable en las propiedades establecidas al cargar con el cliente de explicación mediante dos tipos diferentes de modelos.
    + Se ha agregado un parámetro get_raw a la puntuación explainer.explain(), de modo que una explicación de puntuación puede devolver valores sin formato y con ingeniería.
  + **azureml-train-automl**
    + Se han introducido API públicas de AutoML para la compatibilidad con explicaciones del SDK de explicación de `automl`; existe una nueva manera de admitir explicaciones de AutoML desacoplando las características de AutoML y el SDK de explicación; también se ha incluido la compatibilidad con explicaciones sin procesar integradas del SDK de explicación de Azure ML para los modelos de AutoML.
    + Se va a quitar azureml-defaults de los entornos de entrenamiento remoto.
    + Se ha cambiado la ubicación predeterminada del almacén de caché de una basada en FileCacheStore a una de AzureFileCacheStore para AutoML en la ruta de acceso del código de Azure Databricks.
    + Se ha corregido la validación correcta de los datos de entrada si se especifican en formato de Dataflow.
  + **azureml-train-core**
    + Se ha revertido source_directory_data_store que estaba en desuso.
    + Se ha agregado la capacidad para invalidar las versiones de paquetes instaladas de azureml.
    + Se ha agregado compatibilidad con dockerfile en el parámetro `environment_definition` en los estimadores.
    + Se han simplificado los parámetros de entrenamiento distribuido en los estimadores.

         ```python
        from azureml.train.dnn import TensorFlow, Mpi, ParameterServer
        ```

## <a name="2019-09-09"></a>2019-09-09

### <a name="new-web-experience-preview-for-azure-machine-learning-workspaces"></a>Nueva experiencia web (versión preliminar) para áreas de trabajo de Azure Machine Learning
La nueva experiencia web permite a los científicos de datos y a los ingenieros de datos completar su ciclo de vida completo de aprendizaje automático a partir de la preparación y la visualización de los datos para entrenar e implementar modelos en una sola ubicación.

![Interfaz de usuario del área de trabajo de Azure Machine Learning (versión preliminar)](./media/azure-machine-learning-release-notes/new-ui-for-workspaces.jpg)

**Características principales:**

Con esta nueva interfaz de Azure Machine Learning, ahora puede:
+ Administrar los cuadernos o vincular a Jupyter.
+ [Ejecutar experimentos de ML automatizados](tutorial-first-experiment-automated-ml.md).
+ [Crear conjuntos de datos a partir de archivos locales, almacenes de datos y archivos web](how-to-create-register-datasets.md).
+ Explorar y preparar conjuntos de datos para la creación de modelos.
+ Supervisar el desfase de datos de los modelos.
+ Ver recursos recientes desde un panel.

En el momento en que se publica este artículo, se admiten los siguientes exploradores: Chrome, Firefox, Safari y la versión preliminar de Microsoft Edge.

**Problemas conocidos:**

1. Actualice el explorador si ve el mensaje "Se ha producido un error al cargar los archivos de fragmentos" cuando la implementación está en curso.

1. No se puede eliminar ni cambiar el nombre de un archivo en Notebooks y Files. Durante la versión preliminar pública, puede usar la interfaz de usuario de Jupyter o la terminal de la máquina virtual de Notebook para realizar operaciones de actualización de archivos. Al ser un sistema de archivos de red montado, todos los cambios que realice en una máquina virtual de Notebook se reflejan de inmediato en el área de trabajo de Notebook.

1. Para SSH en la VM de Notebook:
   1. Busque las claves SSH que se crearon durante la configuración de la máquina virtual. También puede buscar las claves en el área de trabajo de Azure Machine Learning; para ello, abra la pestaña Compute, busque la máquina virtual de Notebook en la lista, abra sus propiedades y copie las claves del cuadro de diálogo.
   1. Importe esas claves SSH públicas y privadas en el equipo local.
   1. Úselas para SSH en la VM de Notebook.

## <a name="2019-09-03"></a>2019-09-03
### <a name="azure-machine-learning-sdk-for-python-v1060"></a>SDK de Azure Machine Learning para Python v1.0.60

+ **Nuevas características:**
  + Incorporó FileDataset, que hace referencia a uno o varios archivos de sus almacenes de archivos o direcciones URL públicas. Los archivos pueden tener cualquier formato. FileDataset proporciona la capacidad de descargar o montar los archivos en el proceso. 
  + Se agregó compatibilidad de canalización con YAML para PythonScript Step, Adla Step, Databricks Step, DataTransferStep y AzureBatch Step.

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + AutoArima es ahora una canalización sugerida solo para la vista previa.
    + Informe de errores mejorado para pronósticos.
    + Se mejoró el registro mediante el uso de excepciones personalizadas en lugar de genéricas en las tareas de previsión.
    + Se quito la comprobación de max_concurrent_iterations para que sea menor que el número total de iteraciones.
    + Los modelos AutoML ahora devuelven las excepciones AutoMLE
    + Esta versión mejora el rendimiento de ejecución de las ejecuciones locales automatizadas de aprendizaje automático.
  + **azureml-core**
    + Se incorporó Dataset.get_all(workspace), que devuelve un diccionario de objetos `TabularDataset` y `FileDataset` con clave por su nombre de registro.

    ```python
    workspace = Workspace.from_config()
    all_datasets = Dataset.get_all(workspace)
    mydata = all_datasets['my-data']
    ```

    + Introduzca `parition_format` como argumento para `Dataset.Tabular.from_delimited_files` y `Dataset.Tabular.from_parquet.files`. La información de partición de cada ruta de acceso de datos se extraerá en columnas según el formato especificado. "{column_name}" crea una columna de cadena y "{column_name:yyyy/MM/dd/HH/mm/ss}" crea una columna DateTime, donde "yyyy", "MM", "dd", "HH", "mm" y "ss" se usan para extraer año, mes, día, hora, minuto y segundo para el tipo datetime. El partition_format debe empezar por la posición de la primera clave de partición hasta el final de la ruta de acceso del archivo. Por ejemplo, dada la ruta de acceso “ .. /USA/2019/01/01/data.csv” donde la partición es por país y hora, partition_format = “ /{Country}/{PartitionDate: YYYY/MM/DD}/Data. csv ” crea la columna de cadena “Country” con el valor “EE. UU. ” y la columna de fecha y hora “PartitionDate ” con el valor “2019-01-01 ”.
        ```python
        workspace = Workspace.from_config()
        all_datasets = Dataset.get_all(workspace)
        mydata = all_datasets['my-data']
        ```

    + Introduzca `partition_format` como argumento para `Dataset.Tabular.from_delimited_files` y `Dataset.Tabular.from_parquet.files`. La información de partición de cada ruta de acceso de datos se extraerá en columnas según el formato especificado. "{column_name}" crea una columna de cadena y "{column_name:yyyy/MM/dd/HH/mm/ss}" crea una columna DateTime, donde "yyyy", "MM", "dd", "HH", "mm" y "ss" se usan para extraer año, mes, día, hora, minuto y segundo para el tipo datetime. El partition_format debe empezar por la posición de la primera clave de partición hasta el final de la ruta de acceso del archivo. Por ejemplo, dada la ruta de acceso “ .. /USA/2019/01/01/data.csv” donde la partición es por país y hora, partition_format = “ /{Country}/{PartitionDate: YYYY/MM/DD}/Data. csv ” crea la columna de cadena “Country” con el valor “EE. UU. ” y la columna de fecha y hora “PartitionDate ” con el valor “2019-01-01 ”.
    + `to_csv_files` y los `to_parquet_files` métodos y se han agregado a `TabularDataset`. Estos métodos permiten la conversión entre un `TabularDataset` y un `FileDataset` al convertir los datos a archivos del formato especificado.
    + Inicie sesión automáticamente en el registro de imágenes base al guardar un Dockerfile generado por Model.package ().
    + "gpu_support" ya no es necesario; AML ahora detecta y usa automáticamente la extensión de Docker de Nvidia cuando está disponible. Se eliminará en una próxima versión.
    + Se agregó compatibilidad para crear, actualizar y usar PipelineDrafts.
    + Esta versión mejora el rendimiento de ejecución de las ejecuciones locales automatizadas de aprendizaje automático.
    + Los usuarios pueden consultar las métricas del historial de ejecución por nombre.
    + Se mejoró el registro mediante el uso de excepciones personalizadas en lugar de genéricas en las tareas de previsión.
  + **azureml-explain-model**
    + Se agregó el parámetro feature_maps al nuevo MimicWrapper, lo que permite a los usuarios obtener explicaciones de características sin procesar.
    + Las cargas de conjunto de los conjuntos de datos ahora están desactivadas de forma predeterminada para la carga explicativa y se pueden volver a habilitar con upload_datasets = True
    + Se agregaron parámetros de filtrado "is_law" a la lista de explicaciones y funciones de descarga.
    + Agrega el método `get_raw_explanation(feature_maps)` a los objetos de explicación global y local.
    + Comprobación de versión agregada a lightgbm con advertencia impresa si está por debajo de la versión compatible
    + Uso optimizado de memoria al explicar el procesamiento por lotes
    + Los modelos AutoML ahora devuelven las excepciones AutoMLE
  + **azureml-pipeline-core**
    + Se agregó compatibilidad para crear, actualizar y usar PipelineDrafts; se puede usar para mantener las definiciones de canalización mutable y usarlas de forma interactiva para ejecutar.
  + **azureml-train-automl**
    + Se creó la característica para instalar versiones específicas de pytorch v1.1.0 compatible con gpu, :::no-loc text="cuda"::: toolkit 9.0, pytorch-transformers, que se requiere para habilitar BERT o XLNet en el entorno remoto de tiempo de ejecución de Python.
  + **azureml-train-core**
    + Error temprano de algunos errores de definición de espacio de hiperparámetros directamente en el SDK en lugar del lado del servidor.

### <a name="azure-machine-learning-data-prep-sdk-v1114"></a>SDK de preparación de datos de Azure Machine Learning v1.1.14
+ **Mejoras y correcciones de errores**
  + Se ha habilitado la escritura en ADLS/ADLSGen2 mediante la ruta de acceso sin formato y las credenciales.
  + Se ha corregido un error que provocaba que `include_path=True` no funcionara para `read_parquet`.
  + Se ha corregido el error `to_pandas_dataframe()` causado por la excepción "Valor de propiedad no válido: hostSecret".
  + Se ha corregido un error por el que no se podían leer los archivos en DBFS en modo de Spark.

## <a name="2019-08-19"></a>2019-08-19

### <a name="azure-machine-learning-sdk-for-python-v1057"></a>SDK de Azure Machine Learning para Python v1.0.57
+ **Nuevas características:**
  + Se ha habilitado `TabularDataset` para su uso en AutomatedML. Para obtener más información sobre `TabularDataset`, visite https://aka.ms/azureml/howto/createdatasets.

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Ahora puede actualizar el certificado TLS/SSL del punto de conexión de puntuación implementado en el clúster de AKS para los certificados de cliente y generados por Microsoft.
  + **azureml-automl-core**
    + Se ha corregido un problema en AutoML en que las filas donde faltaban etiquetas no se quitaban correctamente.
    + Se ha mejorado el registro de errores en AutoML; ahora los mensajes de error completos siempre se escribirán en el archivo de registro.
    + AutoML ha actualizado el anclaje de paquetes para incluir `azureml-defaults`, `azureml-explain-model` y `azureml-dataprep`. AutoML dejará de advertir sobre las discrepancias de paquetes (excepto del paquete `azureml-train-automl`).
    + Se ha corregido un problema de `timeseries` en que las divisiones de validación cruzada tenían un tamaño distinto, lo que provocaba un error en el cálculo de rangos.
    + Al ejecutar una iteración de conjunto para el tipo de entrenamiento de validación cruzada, si acabábamos teniendo problemas al descargar los modelos entrenados en todo el conjunto de datos, había una incoherencia entre los pesos del modelo y los modelos que se estaban introduciendo en el conjunto de votación.
    + Se ha corregido el error que se generaba al proporcionar las etiquetas de entrenamiento y/o validación (y e y_valid) en forma de dataframe pandas, pero no como matriz numpy.
    + Se ha corregido un problema con las tareas de previsión cuando se encontraba Ninguno en las columnas booleanas de las tablas de entrada.
    + Se permite a los usuarios de AutoML quitar series de entrenamiento que no son lo suficientemente largas al realizar la previsión. Se permite a los usuarios de AutoML quitar granos del conjunto de pruebas que no existen en el conjunto de entrenamiento al realizar la previsión.
  + **azureml-core**
    + Se ha corregido un problema con el orden de los parámetros blob_cache_timeout.
    + Se han agregado los tipos de excepción fit y transform externos a los errores del sistema.
    + Se ha agregado compatibilidad con los secretos de Key Vault para las ejecuciones remotas. Agregue una clase azureml.core.keyvault.Keyvault para agregar, obtener y enumerar los secretos del objeto keyvault asociado con el área de trabajo. Las operaciones compatibles son:
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.keyvault.Keyvault.set_secret(name, value)
      + azureml.core.keyvault.Keyvault.set_secrets(secrets_dict)
      + azureml.core.keyvault.Keyvault.get_secret(name)
      + azureml.core.keyvault.Keyvault.get_secrets(secrets_list)
      + azureml.core.keyvault.Keyvault.list_secrets()
    + Métodos adicionales para obtener el objeto keyvault predeterminado y los secretos durante la ejecución remota:
      + azureml.core.workspace.Workspace.get_default_keyvault()
      + azureml.core.run.Run.get_secret(name)
      + azureml.core.run.Run.get_secrets(secrets_list)
    + Se han agregado parámetros de reemplazo adicionales al comando submit-hyperdrive de la CLI.
    + Se ha mejorado la confiabilidad de las llamadas a la API mediante la ampliación de los reintentos a las excepciones comunes de la biblioteca de solicitudes.
    + Se ha agregado compatibilidad para el envío de ejecuciones desde una ejecución enviada.
    + Se ha corregido un problema en que el token de SAS expiraba en FileWatcher, lo que provocaba que los archivos dejaran de cargarse después de expirar su token inicial.
    + Ahora se admite la importación de archivos CSV/TSV de HTTP en el SDK de Python del conjunto de datos.
    + Se ha dejado de usar el método Workspace.setup(). El mensaje de advertencia que se muestra a los usuarios sugiere usar create() o get()/from_config() en su lugar.
    + Se ha agregado Environment.add_private_pip_wheel(), que permite cargar paquetes `whl` de Python personalizados privados en el área de trabajo y usarlos con seguridad para compilar o materializar el entorno.
    + Ahora puede actualizar el certificado TLS/SSL del punto de conexión de puntuación implementado en el clúster de AKS para los certificados de cliente y generados por Microsoft.
  + **azureml-explain-model**
    + Se ha agregado un parámetro para agregar un identificador de modelo a las explicaciones en la carga.
    + Se ha agregado el etiquetado `is_raw` a las explicaciones en la memoria y la carga.
    + Se han agregado compatibilidad con PyTorch y pruebas para el paquete azureml-explain-model.
  + **azureml-opendatasets**
    + Compatibilidad con la detección y el registro del entorno de pruebas automáticas.
    + Se han agregado clases para obtener la población de EE. UU. por condado y código postal.
  + **azureml-pipeline-core**
    + Se ha agregado la propiedad label a las definiciones de puertos de entrada y salida.
  + **azureml-telemetry**
    + Se ha corregido una configuración de telemetría incorrecta.
  + **azureml-train-automl**
    + Se ha corregido un error de instalación. El error no se registraba en el campo "errores" de la ejecución del programa de instalación y, por tanto, no se almacenaba en los "errores" de la ejecución principal.
    + Se ha corregido un problema en AutoML en que las filas donde faltaban etiquetas no se quitaban correctamente.
    + Se permite a los usuarios de AutoML quitar series de entrenamiento que no son lo suficientemente largas al realizar la previsión.
    + Se permite a los usuarios de AutoML quitar granos del conjunto de pruebas que no existen en el conjunto de entrenamiento al realizar la previsión.
    + Ahora, AutoMLStep pasa a través de la configuración de `automl` para evitar problemas en los cambios o las adiciones de nuevos parámetros de configuración.
    + AutoML Data Guardrail está ahora en versión preliminar pública. El usuario verá un informe de Data Guardrail (para tareas de clasificación o regresión) después del entrenamiento y también podrá acceder a él a través del API de SDK.
  + **azureml-train-core**
    + Se ha agregado compatibilidad con Torch 1.2 en el estimador de PyTorch.
  + **azureml-widgets**
    + Los gráficos de matriz de confusión se han mejorado para el entrenamiento de la clasificación.

### <a name="azure-machine-learning-data-prep-sdk-v1112"></a>SDK de preparación de datos de Azure Machine Learning v1.1.12
+ **Nuevas características:**
  + Ahora, las listas de cadenas se pueden pasar como entrada a los métodos `read_*`.

+ **Mejoras y correcciones de errores**
  + El rendimiento de `read_parquet` ha mejorado cuando se ejecuta en Spark.
  + Se ha corregido un problema en que se producía un error `column_type_builder` en el caso de una sola columna con formatos de fecha ambiguos.

### <a name="azure-portal"></a>Azure portal
+ **Característica en vista previa**
  + El streaming de archivos de registro y de salida ahora está disponible para las páginas de detalles de la ejecución. Los archivos transmitirán las actualizaciones en tiempo real cuando se active la alternancia de la vista previa.
  + Se ofrece en versión preliminar la capacidad de establecer la cuota en el nivel de área de trabajo. Las cuotas de AmlCompute se asignan en el nivel de suscripción, pero ahora puede distribuir esa cuota entre las áreas de trabajo y asignarla con el fin de conseguir un uso compartido y una gobernanza equitativos. Solo tiene que hacer clic en la hoja **Usages+Quotas** (Usos y cuotas) de la barra de navegación izquierda del área de trabajo y seleccionar la pestaña **Configure Quotas** (Configurar cuotas). Debe ser administrador de la suscripción para poder establecer cuotas en el nivel de área de trabajo, ya que se trata de una operación entre áreas de trabajo.

## <a name="2019-08-05"></a>2019-08-05

### <a name="azure-machine-learning-sdk-for-python-v1055"></a>SDK de Azure Machine Learning para Python v1.0.55

+ **Nuevas características:**
  + Ahora se admite la autenticación basada en token para las llamadas realizadas al punto de conexión de puntuación implementado en AKS. Se seguirá admitiendo la actual autenticación basada en clave y los usuarios podrán cualquiera de los dos mecanismos.
  + Capacidad para registrar cualquier almacenamiento de blobs que esté detrás de la red virtual (VNet) como un almacén de datos.

+ **Mejoras y correcciones de errores**
  + **azureml-automl-core**
    + Corrige un error por el que el tamaño de validación de las divisiones de CV es pequeño y, como resultado, los gráficos de regresión y predicción con datos reales y previstos son incorrectos.
    + Se ha mejorado el registro de las tareas de previsión de las ejecuciones remotas, ahora el usuario recibe un completo mensaje de error si la operación no se ha podido realizar.
    + Se han corregido errores de `Timeseries` si la marca de preprocesamiento es True.
    + Se ha hecho que se puedan realizar más acciones tras algunos mensajes de error de validación de datos de previsión.
    + Reducción del consumo de memoria de las ejecuciones de AutoML mediante la eliminación o la carga diferida de conjuntos de valores, especialmente entre las generaciones de procesos
  + **azureml-contrib-explain-model**
    + Se ha agregado la marca model_task a las explicaciones para que el usuario pueda anular la lógica de inferencia automática predeterminada para el tipo de modelo
    + Cambios en los widgets: se instala automáticamente con `contrib`, ya no es necesario instalar ni habilitar `nbextension`. Permite explicaciones con la importancia global de la característica (por ejemplo, permutativa).
    + Cambios en el panel: trazados de cuadros y trazados de violín, además de trazado de `beeswarm` en la página de resumen. Representación mucho más rápida del trazado de `beeswarm` en el cambio del control deslizante "Top-k". Mensaje útil que explica cómo se calcula top-k. Mensajes personalizables útiles, en lugar de gráficos, cuando no se proporcionan datos.
  + **azureml-core**
    + Se ha agregado el método Model.package() para crear imágenes de Docker y Dockerfiles que encapsulan modelos y sus dependencias.
    + Se han actualizado los servicios web locales para aceptar instancias de InferenceConfig que contengan objetos de entorno.
    + Se ha solucionado el problema de que Model.register() generaba modelos no válidos cuando "." (en el directorio actual) se pasa como parámetro model_path.
    + Se ha agregado Run. submit_child, la funcionalidad refleja Experiment.submit al especificar la ejecución como elemento primario de la ejecución secundaria enviada.
    + Compatibilidad con las opciones de configuración de Model.register en Run.register_model.
    + Capacidad para ejecutar trabajos JAR en el clúster existente.
    + Ahora se admiten los parámetros instance_pool_id y cluster_log_dbfs_path.
    + Se ha agregado compatibilidad con el uso de objetos de entorno al implementar modelos en servicios web. Ahora se puede proporcionar el objeto de entorno como parte del objeto InferenceConfig.
    + Se ha agregado la asignación de appinsifht a las nuevas regiones - centralus - westus - northcentralus
    + Se ha agregado documentación de todos los atributos de todas las clases del almacén de datos.
    + Se ha agregado el parámetro blob_cache_timeout a `Datastore.register_azure_blob_container`.
    + Se han agregado los métodos save_to_directory y load_from_directory a azureml.core.environment.Environment.
    + Se han agregado los comandos "az ml environment download" y "az ml environment register" a la CLI.
    + Se ha agregado el método Environment.add_private_pip_wheel.
  + **azureml-explain-model**
    + Se ha agregado el seguimiento de conjuntos de cambios a las explicaciones mediante el servicio Dataset (versión preliminar).
    + Se ha reducido el tamaño, de lote predeterminado, de 10 000 a 100, al transmitir en secuencias explicaciones globales.
    + Se ha agregado la marca model_task a las explicaciones para que el usuario pueda anular la lógica de inferencia automática predeterminada para el tipo de modelo.
  + **azureml-mlflow**
    + Se ha corregido en mlflow.azureml.build_image el error de que se ignoraban los directorios anidados.
  + **azureml-pipeline-steps**
    + Se ha agregado la capacidad para ejecutar trabajos JAR en clústeres de Azure Databricks existentes.
    + Se han agregado compatibilidad con los parámetros instance_pool_id y cluster_log_dbfs_path para el paso DatabricksStep.
    + Se ha agregado compatibilidad con los parámetros de canalización en el paso de DatabricksStep.
  + **azureml-train-automl**
    + Se ha agregado `docstrings` para los archivos relacionados con Ensemble.
    + Se han actualizado los documentos a un idioma más adecuado en `max_cores_per_iteration` y `max_concurrent_iterations`
    + Se ha mejorado el registro de las tareas de previsión de las ejecuciones remotas, ahora el usuario recibe un completo mensaje de error si la operación no se ha podido realizar.
    + Se ha quitado get_data del cuaderno de la canalización `automlstep`.
    + Se inició la compatibilidad con `dataprep` en `automlstep`.

### <a name="azure-machine-learning-data-prep-sdk-v1110"></a>SDK de preparación de datos de Azure Machine Learning v1.1.10

+ **Nuevas características:**
  + Ahora puede solicitar la ejecución de inspectores concretos (por ejemplo, histograma, gráfico de dispersión, etc.) en columnas específicas.
  + Se ha agregado un argumento parallelize a `append_columns`. Si es True, los datos se cargan en la memoria, pero la ejecución se ejecutará en paralelo; si es False, la ejecución se transmitirá en secuencias, pero tendrá un solo subproceso.

## <a name="2019-07-23"></a>2019-07-23

### <a name="azure-machine-learning-sdk-for-python-v1053"></a>SDK de Azure Machine Learning para Python v1.0.53

+ **Nuevas características:**
  + Automated Machine Learning ahora admite modelos de ONNX de entrenamiento en el destino de proceso remoto
  + Ahora Azure Machine Learning proporciona la posibilidad de reanudar el entrenamiento desde una ejecución, punto de comprobación o archivos de modelo anteriores.
    + Obtenga información sobre [cómo usar los estimadores para reanudar el entrenamiento desde una ejecución anterior](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/tensorflow/train-tensorflow-resume-training/train-tensorflow-resume-training.ipynb)

+ **Mejoras y correcciones de errores**
  + **azure-cli-ml**
    + Los comandos de la CLI de implementación de modelos y de actualización de servicio ahora aceptan parámetros, archivos de configuración o una combinación de ambos. Los parámetros tienen prioridad sobre los atributos en los archivos.
    + La descripción del modelo ahora se puede actualizar después del registro.
  + **azureml-automl-core**
    + Actualice la dependencia de NimbusML a la versión 1.2.0 (la más reciente actualmente).
    + Se agrega compatibilidad con los estimadores y canalizaciones de NimbusML que se van a usar dentro de los estimadores de AutoML.
    + Se ha corregido el error en el procedimiento de selección de conjuntos que hacía aumentar innecesariamente el conjunto resultante aunque las puntuaciones permanecieran constantes.
    + Se ha habilitado la reutilización de algunas características en las divisiones de CV para las tareas de previsión. De este modo, se acelera la ejecución del programa de instalación aproximadamente en un factor de n_cross_validations para las características caras, como las ventanas con desplazamiento y retraso.
    + Se soluciona un problema si el tiempo está fuera del intervalo de tiempo admitido de pandas. Ahora se genera una excepción DataException si el tiempo es menor que pd.Timestamp.min o mayor que pd.Timestamp.max.
    + La previsión permite ahora frecuencias diferentes en los conjuntos de entrenamiento y de prueba si se pueden alinear. Por ejemplo, "quarterly starting in January" y "quarterly starting in October" se pueden alinear.
    + La propiedad "parameters" se ha agregado a TimeSeriesTransformer.
    + Se quitan las clases de excepción antiguas.
    + En las tareas de previsión, el parámetro `target_lags` ahora acepta un valor entero único o una lista de enteros. Si se proporcionó el entero, solo se creará un retardo. Si se proporciona una lista, se tomarán los valores únicos de los retardos. target_lags=[1, 2, 2, 4] creará retardos de uno, dos y cuatro períodos.
    + Se corrige el error sobre la pérdida de tipos de columnas después de la transformación (error vinculado).
    + En `model.forecast(X, y_query)` se permite que y_query sea un tipo de objeto que contiene None(s) al principio (#459519).
    + Se agregan los valores esperados a la salida de `automl`.
  + **azureml-contrib-datadrift**
    +  Se incluyen mejoras en el cuaderno de ejemplo, incluido el cambio a azureml-opendatasets en lugar de azureml-contrib-opendatasets y mejoras de rendimiento al enriquecer datos.
  + **azureml-contrib-explain-model**
    + Se corrige el argumento de transformaciones para el explicador LIME con respecto a la importancia de la característica sin formato en el paquete azureml-contrib-explain-model.
    + Se han agregado segmentaciones a las explicaciones de imágenes en el explicador de imágenes del paquete AzureML-contrib-explain-model.
    + Se ha agregado compatibilidad dispersa con SciPy para LimeExplainer.
    + Se ha agregado `batch_size` para imitar el explicador cuando `include_local=False` para las explicaciones globales de streaming en lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel.
  + **azureml-contrib-featureengineering**
    + Se corrige para llamar a set_featurizer_timeseries_params(): el cambio de tipo de valor dict y null check. Se agrega un cuaderno para el caracterizador `timeseries`.
    + Actualice la dependencia de NimbusML a la versión 1.2.0 (la más reciente actualmente).
  + **azureml-core**
    + Se agrega la capacidad de asociar almacenes de datos DBFS en la CLI de AzureML.
    + Se corrige el error con la carga del almacén de datos en que se creaba una carpeta vacía si `target_path` se iniciaba con `/`.
    + Se corrige el problema de `deepcopy` en ServicePrincipalAuthentication.
    + Se han agregado los comandos "az ml environment show" y "az ml environment list" a la CLI.
    + Los entornos ahora admiten la especificación de un base_dockerfile como alternativa a una base_image ya creada.
    + La configuración de RunConfiguration sin usar en auto_prepare_environment se ha marcado como en desuso.
    + La descripción del modelo ahora se puede actualizar después del registro.
    + Corrección de los errores: Ahora, la eliminación del modelo y la imagen proporciona más información sobre la recuperación de objetos ascendentes que dependen de ellos si la eliminación produce un error debido a una dependencia ascendente.
    + Se corrige un error que imprimía la duración en blanco para las implementaciones que se producen al crear un área de trabajo para algunos entornos.
    + Se han mejorado las excepciones de error en la creación de áreas de trabajo. Así, los usuarios no verán el mensaje "Unable to create workspace. Unable to find... " (No se puede crear el área de trabajo. No se puede encontrar.) y, en su lugar, verán el error de creación real.
    + Se agrega compatibilidad con la autenticación de token en los servicios web de AKS.
    + Agregue el método `get_token()` a los objetos `Webservice`.
    + Se ha agregado compatibilidad con la CLI para administrar conjuntos de datos de aprendizaje automático.
    + `Datastore.register_azure_blob_container` ahora, opcionalmente, acepta un valor `blob_cache_timeout` (en segundos) que configura los parámetros de montaje de blobfuse para habilitar la expiración de la memoria caché para este almacén de datos. El valor predeterminado es sin tiempo de espera, es decir, cuando se lea un blob, permanecerá en la memoria caché local hasta que el trabajo finalice. La mayoría de los trabajos preferirán esta configuración, pero algunos trabajos necesitan leer más datos de un conjunto de datos grande que quepa en sus nodos. Para estos trabajos, el ajuste de este parámetro les ayudará a funcionar correctamente. Tenga cuidado al optimizar este parámetro: si establece un valor demasiado bajo, puede producirse un rendimiento deficiente, ya que los datos que se usan en una época pueden expirar antes de volver a usarse. Todas las lecturas se realizarán desde el almacenamiento de blobs o de la red, en lugar de la memoria caché local, lo que afecta desfavorablemente a los tiempos de entrenamiento.
    + La descripción del modelo ahora se puede actualizar correctamente después del registro.
    + La eliminación de modelos e imágenes ahora proporciona más información sobre los objetos de nivel superior que dependen de ellos, lo que provoca un error en la eliminación.
    + Se mejora la utilización de recursos de las ejecuciones remotas que usan azureml.mlflow.
  + **azureml-explain-model**
    + Se corrige el argumento de transformaciones para el explicador LIME con respecto a la importancia de la característica sin formato en el paquete azureml-contrib-explain-model.
    + Se agrega compatibilidad dispersa con SciPy para LimeExplainer.
    + Se agrega el contenedor de explicación lineal, así como otro nivel de explicación tabular para explicar los modelos lineales.
    + Para imitar la explicación en la biblioteca de modelos de explicación, se corrige el error cuando include_local=False para la entrada de datos dispersos.
    + Se agregan los valores esperados a la salida de `automl`.
    + Se corrige la importancia de la característica de permutación cuando se proporcionaba el argumento de transformaciones para obtener la importancia de la característica sin procesar.
    + Se agrega `batch_size` para imitar el explicador cuando `include_local=False` para las explicaciones globales de streaming en los lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel.
    + Para la biblioteca de explicación de modelos, se corrigieron las explicaciones de la caja negra en los que la entrada de trama de datos de Pandas es necesaria para la predicción.
    + Se corrige el error por el que `explanation.expected_values` a veces devolvería un valor float en lugar de una lista con dicho valor incluido.
  + **azureml-mlflow**
    + Se mejora el rendimiento de mlflow.set_experiment(experiment_name).
    + Se corrige el error en el uso de InteractiveLoginAuthentication para tracking_uri de mlflow.
    + Se mejora la utilización de recursos de las ejecuciones remotas que usan azureml.mlflow.
    + Se mejora la documentación del paquete azureml-mlflow.
    + Se ha corregido el error que hacía que mlflow.log_artifacts("my_dir") guardara artefactos en `my_dir/<artifact-paths>` en lugar de `<artifact-paths>`.
  + **azureml-opendatasets**
    + Se ancla `pyarrow` de `opendatasets` para las versiones anteriores (<0.14.0) debido al problema de memoria reciente.
    + Se mueve azureml-contrib-opendatasets a azureml-opendatasets.
    + Se permite que las clases del conjunto de datos abierto se registren en el área de trabajo de Azure Machine Learning y aprovechen sin problemas las funcionalidades del conjunto de datos de AML.
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.
  + **azureml-pipeline-steps**
    + El almacén de datos DBFS se admite ahora en las entradas y salidas de DatabricksStep.
    + Se ha actualizado la documentación para Azure Batch Step con respecto a las entradas y salidas.
    + En AzureBatchStep, se cambió el valor predeterminado de *delete_batch_job_after_finish* a *true*.
  + **azureml-telemetry**
    +  Se mueve azureml-contrib-opendatasets a azureml-opendatasets.
    + Se permite que las clases del conjunto de datos abierto se registren en el área de trabajo de Azure Machine Learning y aprovechen sin problemas las funcionalidades del conjunto de datos de AML.
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.
  + **azureml-train-automl**
    + Se actualiza la documentación sobre get_output para reflejar el tipo de valor devuelto real y proporcionar notas adicionales sobre cómo recuperar las propiedades de clave.
    + Actualice la dependencia de NimbusML a la versión 1.2.0 (la más reciente actualmente).
    + Se agregan los valores esperados a la salida de `automl`.
  + **azureml-train-core**
    + Ahora se aceptan cadenas como destino de proceso para la optimización automática de hiperparámetros.
    + La configuración de RunConfiguration sin usar en auto_prepare_environment se ha marcado como en desuso.

### <a name="azure-machine-learning-data-prep-sdk-v119"></a>SDK de preparación de datos de Azure Machine Learning v1.1.9

+ **Nuevas características:**
  + Se ha agregado compatibilidad para leer un archivo directamente desde una dirección URL HTTP o HTTPS.

+ **Mejoras y correcciones de errores**
  + Se mejora el mensaje de error al intentar leer un conjunto de datos Parquet de un origen remoto (que no se admite actualmente).
  + Se corrige un error al escribir en el formato de archivo Parquet en ADLS Gen 2 y al actualizar el nombre del contenedor ADLS Gen 2 en la ruta de acceso.

## <a name="2019-07-09"></a>09/07/2019

### <a name="visual-interface"></a>Interfaz visual
+ **Características en versión preliminar**
  + Se ha agregado el módulo "Ekecutar script R" en la interfaz visual.

### <a name="azure-machine-learning-sdk-for-python-v1048"></a>SDK de Azure Machine Learning para Python v1.0.48

+ **Nuevas características:**
  + **azureml-opendatasets**
    + **azureml-contrib-opendatasets** ahora está disponible como **azureml-opendatasets**. El paquete antiguo puede seguir funcionando, pero le recomendamos que use **azureml-opendatasets** de ahora en adelante para obtener mejoras y funcionalidades más avanzadas.
    + Este nuevo paquete le permite registrar conjuntos de datos abiertos como un conjunto de datos en el área de trabajo de Azure Machine Learning y aprovechar completamente las funcionalidades que ofrece dicho conjunto de datos.
    + También incluye funcionalidades existentes, por ejemplo, el consumo de conjuntos de datos abiertos como dataframes de SPARK o Pandas y combinaciones de ubicaciones para algún conjunto de datos como el tiempo.

+ **Características en versión preliminar**
    + Ahora, HyperDriveConfig puede aceptar el objeto de canalización como un parámetro a fin de admitir la optimización de hiperparámetros mediante una canalización.

+ **Mejoras y correcciones de errores**
  + **azureml-train-automl**
    + Se corrigió el error sobre la pérdida de tipos de columnas después de la transformación.
    + Se corrigió el error para permitir que y_query sea un tipo de objeto que contiene None(s) al principio.
    + Se corrigió el problema en el que el procedimiento de selección de conjuntos hacía aumentar innecesariamente el conjunto resultante aunque las puntuaciones permanecieran constantes.
    + Se corrigió el problema con la configuración para permitir y bloquear list_models en AutoMLStep.
    + Se corrigió el problema que impedía el uso del preprocesamiento cuando AutoML se habría usado en el contexto de las canalizaciones de Azure ML.
  + **azureml-opendatasets**
    + Se movió azureml-contrib-opendatasets a azureml-opendatasets.
    + Se permitió que las clases del conjunto de datos abierto se registraran en el área de trabajo de Azure Machine Learning y aprovecharan completamente las funcionalidades del conjunto de datos de AML.
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.
  + **azureml-explain-model**
    + Se actualizó la documentación en línea sobre los objetos de interpretabilidad.
    + Se agregó `batch_size` para imitar la explicación cuando `include_local=False` para las explicaciones globales de streaming de los lotes a fin de mejorar el tiempo de ejecución de DecisionTreeExplainableModel para la biblioteca de explicación del modelo.
    + Se corrigió el problema por el que `explanation.expected_values` a veces devolvería un valor float en lugar de una lista con dicho valor incluido.
    + Se agregaron los valores esperados en la salida de `automl` para la explicación de imitación en la biblioteca de modelos de explicación.
    + Se corrigió la importancia de la característica de permutación cuando se proporcionaba el argumento de transformaciones para obtener la importancia de la característica sin procesar.
  + **azureml-core**
    + Se agregó la capacidad de asociar almacenes de datos DBFS en la CLI de AzureML.
    + Se corrigió el problema con la carga del almacén de datos en que se creaba una carpeta vacía si `target_path` se iniciaba con `/`.
    + Se habilitó la comparación de dos conjuntos de datos:
    + Ahora, la eliminación del modelo y la imagen proporciona más información sobre la recuperación de objetos ascendentes que dependen de ellos si la eliminación produce un error debido a una dependencia ascendente.
    + Se dejó de usar la configuración de RunConfiguration sin usar en auto_prepare_environment.
  + **azureml-mlflow**
    + Se mejoró la utilización de recursos de las ejecuciones remotas que usan azureml.mlflow.
    + Se mejoró la documentación del paquete azureml-mlflow.
    + Se corrigió el problema que hacía que mlflow.log_artifacts("my_dir") guardara artefactos en "my_dir/artifact-paths" en lugar de "artifact-paths".
  + **azureml-pipeline-core**
    + El parámetro hash_paths de todos los pasos de la canalización está en desuso y se quitará en el futuro. De manera predeterminada, se aplica hash al contenido de source_directory (excepto en los archivos que se enumeran en `.amlignore` o `.gitignore`).
    + Se ha continuado la mejora de los valores Module y ModuleStep para que admitan módulos específicos de tipo de proceso, para preparar la integración de RunConfiguration y otros cambios para desbloquear el uso del módulo específico del proceso en canalizaciones.
  + **azureml-pipeline-steps**
    + AzureBatchStep: Se mejoró la documentación con respecto a las entradas y salidas.
    + AzureBatchStep: Se cambió el valor predeterminado de delete_batch_job_after_finish a true.
  + **azureml-train-core**
    + Ahora se aceptan cadenas como destino de proceso para la optimización automática de hiperparámetros.
    + Se dejó de usar la configuración de RunConfiguration sin usar en auto_prepare_environment.
    + Se dejaron de usar los parámetros `conda_dependencies_file_path` y `pip_requirements_file_path` para usar `conda_dependencies_file` y `pip_requirements_file`, respectivamente.
  + **azureml-opendatasets**
    + Se mejoró significativamente el rendimiento de enriquecimiento de NoaaIsdWeather en una versión que no es de SPARK.

## <a name="2019-04-26"></a>2019-04-26

### <a name="azure-machine-learning-sdk-for-python-v1033-released"></a>SDK de Azure Machine Learning para Python v1.0.33.

+ Los modelos acelerados mediante hardware de Azure Machine Learning de la [matriz de puertas programables](how-to-deploy-fpga-web-service.md) están disponible con carácter general.
  + Ya se puede [usar el paquete de modelos de azureml-accel](how-to-deploy-fpga-web-service.md) para:
    + Entrenar las ponderaciones de una red neuronal profunda compatible (ResNet 50, ResNet 152, DenseNet 121, VGG-16 y SSD VGG)
    + Usar el aprendizaje de transferencia con la DNN compatible
    + Registrar el modelo con el servicio Administración de modelos e incluir el modelo en un contenedor
    + Implementar el modelo en una máquina virtual de Azure con una matriz de puertas programables en un clúster de Azure Kubernetes Service (AKS)
  + Implementar el contenedor en un dispositivo servidor de [Azure Data Box Edge](../databox-online/azure-stack-edge-overview.md)
  + Puntuar los datos con el punto de conexión gRPC con este [ejemplo](https://github.com/Azure-Samples/aml-hardware-accelerated-models)

### <a name="automated-machine-learning"></a>Automated Machine Learning

+ Barrido de características para habilitar la incorporación dinámica de :::no-loc text="featurizers"::: para la optimización del rendimiento. Nuevos :::no-loc text="featurizers":::: incrustaciones de trabajos, peso de la evidencia, codificaciones de destino, codificación de destino de texto, distancia del clúster
+ CV inteligente para controlar el entrenamiento o las divisiones válidas en Machine Learning automatizado
+ Algunos cambios en la optimización de la memoria y mejora del rendimiento del runtime
+ Mejora del rendimiento en la explicación de modelos
+ Conversión del modelo de ONNX para la ejecución local
+ Se ha agregado compatibilidad con submuestreos
+ Detención inteligente cuando haya criterios de salida definidos
+ Conjuntos apilados

+ Previsión de series temporales
  + Nueva función de previsión de predicciones
  + Ahora puede usar la validación cruzada de origen variable en los datos de series temporales
  + Se ha agregado una nueva funcionalidad para configurar los retardos de las series temporales
  + Se ha agregado una nueva funcionalidad que admita las características agregadas del periodo acumulado
  + Nueva detección de festivos y administrador de características cuando el código de país se define en la configuración del experimento

+ Azure Databricks
  + Se han habilitado la previsión de series temporales y la funcionalidad de explicabilidad e interpretabilidad de modelos
  + Ya se pueden cancelar y reanudar (continuar) experimentos de Machine Learning automatizado
  + Se ha agregado compatibilidad con el procesamiento con múltiples núcleos

### <a name="mlops"></a>MLOps
+ **Implementación y depuración locales para la puntuación de contenedores**<br/> Ya se puede implementar un modelo de Machine Learning localmente y realizar iteraciones rápidas en un archivo de puntuación y en las dependencias para asegurarse de que se comportan según lo previsto.

+ **Se han introducido InferenceConfig y Model.deploy()**<br/> La implementación de modelos ahora admite la especificación de una carpeta de origen con un script de entrada, igual que en RunConfig.  Además, se ha simplificado la implementación de modelos a un solo comando.

+ **Seguimiento de referencias de Git**<br/> Los clientes llevan tiempo solicitando funcionalidades de integración de Git básicas, ya que son una ayuda para mantener un registro de auditoría completo. Hemos implementado el seguimiento por las entidades principales de Azure Machine Learning de los metadatos relacionados con Git (repo, commit, clean state). Esta información la recopilarán automáticamente el SDK y la CLI.

+ **Servicio de validación y generación de perfiles de modelos**<br/> Los clientes se quejan con frecuencia de la dificultad que conlleva ajustar correctamente el tamaño del proceso asociado a su servicio de inferencia. Con nuestro servicio de generación de perfiles de modelos, el cliente puede proporcionar entradas de ejemplo y generaremos perfiles en 16 configuraciones de memoria/CPU diferentes para determinar el tamaño óptimo para la implementación.

+ **Traiga su propia imagen base para la inferencia**<br/> Otra queja habitual era la dificultad para moverse de las dependencias de experimentación al uso compartido de RE de inferencia. Con la nueva funcionalidad de uso compartido de imágenes base, ahora es posible reutilizar las imágenes base de la experimentación, las dependencias y todo en general, para la inferencia. Esto debería acelerar las implementaciones y reducir la brecha entre el bucle interno y el externo.

+ **Mejora en la generación de esquemas de Swagger**<br/> El anterior método de generación de Swagger era imposible de automatizar y propenso a errores. Hay una nueva forma en línea de generar esquemas de Swagger desde cualquier función de Python mediante los decoradores. Ahora este es un código abierto y el protocolo de generación de esquema no está emparejado a la plataforma de Azure Machine Learning.

+ **La CLI de Azure ML está disponible con carácter general**<br/> Ahora se pueden implementar modelos con un solo comando de la CLI. Recibimos comentarios de clientes comunes que informan de que nadie implementa un modelo de Machine Learning desde un cuaderno de Jupyter Notebook. La [**documentación de referencia de la CLI**](./reference-azure-machine-learning-cli.md) se ha actualizado.


## <a name="2019-04-22"></a>2019-04-22

SDK de Azure Machine Learning para Python v1.0.30.

[`PipelineEndpoint`](/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline_endpoint.pipelineendpoint) se ha introducido para agregar una nueva versión de una canalización publicada sin perder el mismo punto de conexión.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure portal
  + Ahora puede volver a enviar un ciclo de script existente de un clúster de proceso remoto existente.
  + Ahora puede ejecutar una canalización publicada con parámetros nuevos en la pestaña Canalizaciones.
  + Los detalles de la ejecución ahora admiten un nuevo visor de archivos de instantánea. Puede ver una instantánea del directorio cuando envió una ejecución concreta. También puede descargar el cuaderno que se envió para iniciar la ejecución.
  + Ahora puede cancelar las ejecuciones primarias desde Azure Portal.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>SDK de Azure Machine Learning para Python v1.0.23

+ **Nuevas características:**
  + El SDK de Azure Machine Learning ahora admite Python 3.7.
  + Ahora, los estimadores de DNN de Azure Machine Learning proporcionan compatibilidad con múltiples versiones integrada. Por ejemplo, el estimador `TensorFlow` ahora acepta un parámetro `framework_version` y los usuarios pueden especificar las versiones "1.10" o "1.12". Para obtener una lista de las versiones compatibles con la versión actual del SDK, llame a `get_supported_versions()` en la clase de marco deseada (por ejemplo, `TensorFlow.get_supported_versions()`).
  Para obtener una lista de las versiones compatibles con la versión más reciente del SDK, consulte la [documentación del estimador de DNN](/python/api/azureml-train-core/azureml.train.dnn).

## <a name="2019-03-25"></a>25-03-2019

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>SDK de Azure Machine Learning para Python v1.0.21

+ **Nuevas características:**
  + El método *azureml.core.Run.create_children* permite la creación de baja latencia de varias ejecuciones secundarias con una sola llamada.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>SDK de Azure Machine Learning para Python v1.0.18

 + **Cambios**
   + El paquete azureml tensorboard reemplaza azureml-contrib-tensorboard.
   + Con esta versión, puede configurar una cuenta de usuario en el clúster de proceso administrado (amlcompute) al crearlo. Esto puede realizarse usando estas propiedades en la configuración de aprovisionamiento. Encontrará más información en la [documentación de referencia del SDK](/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none--remote-login-port-public-access--notspecified--).

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>SDK de preparación de datos de Azure Machine Learning v1.0.17

+ **Nuevas características:**
  + Ahora admite la adición de dos columnas numéricas para generar una columna resultante mediante el lenguaje de expresiones.

+ **Mejoras y correcciones de errores**
  + Ha mejorado la documentación y la comprobación de parámetros de random_split.

## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>SDK de preparación de datos de Azure Machine Learning v1.0.16

+ **Corrección de error**
  + Se ha corregido un problema de autenticación de una entidad de servicio que se produjo por un cambio de API.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>SDK de Azure Machine Learning para Python v1.0.17

+ **Nuevas características:**
  + Ahora, Azure Machine Learning proporciona compatibilidad de primera clase con Chainer del marco DNN popular. Mediante [`Chainer`](/python/api/azureml-train-core/azureml.train.dnn.chainer) los usuarios de la clase pueden entrenar e implementar fácilmente modelos de Chainer.
    + Aprenda a [ejecutar el entrenamiento distribuido con ChainerMN](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/ml-frameworks/chainer/distributed-chainer/distributed-chainer.ipynb)
    + Aprenda a [ejecutar el ajuste de hiperparámetros con Chainer mediante HyperDrive](https://github.com/Azure/MachineLearningNotebooks/blob/b881f78e4658b4e102a72b78dbd2129c24506980/how-to-use-azureml/ml-frameworks/chainer/deployment/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)
  + Se ha agregado a las canalizaciones de Azure Machine Learning la posibilidad de desencadenar una ejecución de una canalización según las modificaciones del almacén de datos. El [cuaderno de programación](https://aka.ms/pl-schedule) de la canalización se actualice para presentar esta característica.

+ **Mejoras y correcciones de errores**
  + Se ha agregado compatibilidad en las canalizaciones de Azure Machine Learning con el establecimiento de la propiedad source_directory_data_store en un almacén de datos deseado (como un almacenamiento de blobs) en [RunConfigurations](/python/api/azureml-core/azureml.core.runconfig.runconfiguration) que se suministran a [PythonScriptStep](/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep). De forma predeterminada los pasos utilizan el almacén de Azure File como el almacén de datos de respaldo, lo que provocar problemas de limitación cuando un gran número de pasos se ejecutan simultáneamente.

### <a name="azure-portal"></a>Azure portal

+ **Nuevas características:**
  + Nueva experiencia de arrastrar y soltar el editor de tablas en informes. Los usuarios pueden arrastrar una columna desde el cuadro al área de la tabla en la que se mostrará una vista previa de la tabla. Las columnas se pueden reorganizar.
  + Nuevo visor de archivos de registros
  + Vínculos a ejecuciones de experimentos, proceso, modelos, imágenes e implementaciones desde la pestaña de actividades

## <a name="next-steps"></a>Pasos siguientes

Consulte la introducción de [Azure Machine Learning](overview-what-is-azure-machine-learning.md).
