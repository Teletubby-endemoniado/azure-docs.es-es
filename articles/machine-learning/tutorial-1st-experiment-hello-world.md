---
title: 'Tutorial: Introducción a un script de Python'
titleSuffix: Azure Machine Learning
description: Introducción al primer script de Python en Azure Machine Learning. Esta es la primera parte de una serie introductoria de tres partes.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 04/27/2021
ms.custom: devx-track-python, FY21Q4-aml-seo-hack, contperf-fy21q4
ms.openlocfilehash: 472fcf4c7a1cc486db5aded40c87ffec2a9e796a
ms.sourcegitcommit: 9f1a35d4b90d159235015200607917913afe2d1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2021
ms.locfileid: "122634698"
---
# <a name="tutorial-get-started-with-a-python-script-in-azure-machine-learning-part-1-of-3"></a>Tutorial: Introducción a un script de Python en Azure Machine Learning (parte 1 de 3)

En este tutorial, ejecutará su primer script de Python en la nube con Azure Machine Learning. Esta es la *parte 1 de una serie de tutoriales de tres partes*.

En este tutorial se evita la complejidad del entrenamiento de un modelo de Machine Learning. Ejecutará un script de Python "Hola mundo" en la nube. Aprenderá cómo se usa un script de control para configurar y crear una ejecución en Azure Machine Learning.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Cómo crear y ejecutar de un script "Hola mundo" script de Python.
> * Crear un script de control de Python para enviar "Hola mundo" a Azure Machine Learning.
> * Comprender los conceptos de Azure Machine Learning en el script de control.
> * Enviar y ejecutar el script "Hola mundo".
> * Ver la salida del código en la nube.

## <a name="prerequisites"></a>Requisitos previos

- Complete los pasos que se describen en [Inicio rápido: Configure el área de trabajo para empezar a trabajar con Azure Machine Learning](quickstart-create-resources.md) para crear un área de trabajo, una instancia de proceso y un clúster de proceso para usarlos en esta serie de tutoriales.

## <a name="create-and-run-a-python-script"></a>Creación y ejecución de un script de Python

En este tutorial se usará la instancia de proceso como equipo de desarrollo.  En primer lugar, cree algunas carpetas y el script:

1. Inicie sesión en [Estudio de Azure Machine Learning](https://ml.azure.com) y seleccione el área de trabajo si se le solicita.
1. Seleccione **Notebooks** en la parte izquierda.
1. En la barra de herramientas de **Archivos**, seleccione **+** y, a continuación, seleccione **Crear nueva carpeta**.
  :::image type="content" source="media/tutorial-1st-experiment-hello-world/create-folder.png" alt-text="Instantánea que muestra cómo crear la herramienta de carpeta nueva en la barra de herramientas.":::
1. Asigne a la carpeta el nombre **Empezar**.
1. A la derecha del nombre de la carpeta, use **...** para crear otra carpeta dentro de la carpeta **Empezar**.
  :::image type="content" source="media/tutorial-1st-experiment-hello-world/create-sub-folder.png" alt-text="Instantánea que muestra cómo crear un menú de subcarpetas.":::
1. Asigne a la nueva carpeta el nombre **src**.  Use el vínculo **Editar ubicación** si la ubicación del archivo no es correcta.
1. A la derecha de la carpeta **src**, use **...** para crear un nuevo archivo en la carpeta **src**. 
1. Asigne al archivo el nombre *hello.py*.  Cambie el **tipo de archivo** a *Python (* .py)*.

Copie este código en el archivo:

```python
# src/hello.py
print("Hello world!")
```

Ahora la estructura de carpetas del proyecto tendrá el siguiente aspecto: 

:::image type="content" source="media/tutorial-1st-experiment-hello-world/directory-structure.png" alt-text="La estructura de carpetas muestra hello.py en la subcarpeta src.":::


### <a name="test-your-script"></a><a name="test"></a>Prueba del script

Puede ejecutar el código de forma local, lo que en este caso significa en la instancia de proceso. Ejecutar el código localmente tiene la ventaja de la depuración interactiva del código.  

Si ha detenido previamente la instancia de proceso, iníciela ahora con la herramienta **Iniciar proceso** a la derecha de la lista desplegable de procesos. Espere aproximadamente un minuto para que el estado cambie a *En ejecución*.

:::image type="content" source="media/tutorial-1st-experiment-hello-world/start-compute.png" alt-text="Captura de pantalla que muestra el inicio de la instancia de proceso si se detiene":::

Seleccione **Guardar y ejecutar script en el terminal** para ejecutar el script.

:::image type="content" source="media/tutorial-1st-experiment-hello-world/save-run-in-terminal.png" alt-text="Captura de pantalla que muestra cómo guardar y ejecutar el script en la herramienta de terminal en la barra de herramientas":::

Verá la salida del script en la ventana de terminal que se abre. Cierre la pestaña y seleccione **Finalizar** para cerrar la sesión.

## <a name="create-a-control-script"></a><a name="control-script"></a> Creación de un script de control

Un *script de control* le permite ejecutar el script `hello.py` en distintos recursos de proceso. El script de control se usa para controlar cómo y dónde se ejecuta el código de aprendizaje automático.  

Seleccione **...** al final de la carpeta **Empezar** para crear un archivo nuevo.  Cree un archivo de Python denominado *run-hello.py*, y copie y pegue el código siguiente en este archivo:

```python
# get-started/run-hello.py
from azureml.core import Workspace, Experiment, Environment, ScriptRunConfig

ws = Workspace.from_config()
experiment = Experiment(workspace=ws, name='day1-experiment-hello')

config = ScriptRunConfig(source_directory='./src', script='hello.py', compute_target='cpu-cluster')

run = experiment.submit(config)
aml_url = run.get_portal_url()
print(aml_url)
```

> [!TIP]
> Si usó un nombre diferente al crear el clúster de proceso, asegúrese de ajustar también el nombre en el código `compute_target='cpu-cluster'`.

### <a name="understand-the-code"></a>Comprendiendo el código

Esta es una descripción de cómo funciona el script de control:

:::row:::
   :::column span="":::
      `ws = Workspace.from_config()`
   :::column-end:::
   :::column span="2":::
      El [área de trabajo](/python/api/azureml-core/azureml.core.workspace.workspace) se conecta al área de trabajo de Azure Machine Learning para que pueda comunicarse con los recursos de Azure Machine Learning.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `experiment =  Experiment( ... )`
   :::column-end:::
   :::column span="2":::
      El [experimento](/python/api/azureml-core/azureml.core.experiment.experiment) proporciona una manera sencilla de organizar varias ejecuciones con un solo nombre. Más adelante, podrá ver cómo los experimentos facilitan la comparación de las métricas entre docenas de ejecuciones.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config = ScriptRunConfig( ... )` 
   :::column-end:::
   :::column span="2":::
      [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig) ajusta el código `hello.py` y lo pasa al área de trabajo. Como sugiere su nombre, puede usar esta clase para _configurar_ cómo quiera el _script_ para su _ejecución_ en Azure Machine Learning. También especifica el destino de proceso en el que se ejecutará el script. En este código, el destino es el clúster de proceso que creó en el [tutorial de configuración](./quickstart-create-resources.md).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `run = experiment.submit(config)`
   :::column-end:::
   :::column span="2":::
       Envía el script. Este envío se denomina [ejecución](/python/api/azureml-core/azureml.core.run%28class%29). Una ejecución encapsula una ejecución única del código. Use una ejecución para supervisar el progreso del script, capturar la salida, analizar los resultados, visualizar las métricas y mucho más.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `aml_url = run.get_portal_url()` 
   :::column-end:::
   :::column span="2":::
        El objeto `run` proporciona un identificador en la ejecución del código. Supervise el progreso de Azure Machine Learning Studio con la dirección URL que se imprime desde el script de Python.  
   :::column-end:::
:::row-end:::


## <a name="submit-and-run-your-code-in-the-cloud"></a><a name="submit"></a> Envío y ejecución del código en la nube

1. Seleccione **Guardar y ejecutar el script en el terminal** para ejecutar el script de control, que a su vez ejecuta `hello.py` en el clúster de proceso que creó en el [tutorial de configuración](quickstart-create-resources.md).

1. En el terminal, es posible que se le pida que inicie sesión para autenticarse.  Copie el código y siga el vínculo para completar este paso.

1. Una vez que se haya autenticado, verá un vínculo en el terminal. Seleccione el vínculo para ver la ejecución.

    [!INCLUDE [amlinclude-info](../../includes/machine-learning-py38-ignore.md)]

## <a name="view-the-output"></a>Visualización de la salida

1. En la página que se abre, verá el estado de la ejecución.
1. Cuando dicho estado sea **Completado**, seleccione **Resultados y registros** en la parte superior de la página.
1. Seleccione **70_driver_log.txt** para ver la salida de la ejecución.

## <a name="monitor-your-code-in-the-cloud-in-the-studio"></a><a name="monitor"></a>Supervisión del código en la nube en Studio

La salida del script contendrá un vínculo a Studio que tiene un aspecto similar al siguiente: `https://ml.azure.com/experiments/hello-world/runs/<run-id>?wsid=/subscriptions/<subscription-id>/resourcegroups/<resource-group>/workspaces/<workspace-name>`.

Siga el vínculo.  En primer lugar, verá el estado **En cola** o **En preparación**.  La primera ejecución tardará entre 5 y 10 minutos en completarse. Esto se debe a lo siguiente:

* Se crea una imagen de Docker en la nube.
* Se cambia el tamaño del clúster de proceso de 0 a 1 nodo.
* La imagen de Docker se descarga en el proceso. 

Las ejecuciones posteriores son mucho más rápidas (unos 15 segundos), ya que la imagen de Docker se almacena en caché en el proceso. Para probar esto, reenvíe el código siguiente después de que se haya completado la primera ejecución.

Espere unos 10 minutos.  Verá un mensaje que indica que la ejecución se ha completado. A continuación, use **Actualizar** para ver el cambio de estado a *Completado*.  Una vez completado el trabajo, vaya a la pestaña **Salidas y registros**. Allí puede ver un archivo `70_driver_log.txt` similar al siguiente:

```txt
 1: [2020-08-04T22:15:44.407305] Entering context manager injector.
 2: [context_manager_injector.py] Command line Options: Namespace(inject=['ProjectPythonPath:context_managers.ProjectPythonPath', 'RunHistory:context_managers.RunHistory', 'TrackUserError:context_managers.TrackUserError', 'UserExceptions:context_managers.UserExceptions'], invocation=['hello.py'])
 3: Starting the daemon thread to refresh tokens in background for process with pid = 31263
 4: Entering Run History Context Manager.
 5: Preparing to call script [ hello.py ] with arguments: []
 6: After variable expansion, calling script [ hello.py ] with arguments: []
 7:
 8: Hello world!
 9: Starting the daemon thread to refresh tokens in background for process with pid = 31263
10:
11:
12: The experiment completed successfully. Finalizing run...
13: Logging experiment finalizing status in history service.
14: [2020-08-04T22:15:46.541334] TimeoutHandler __init__
15: [2020-08-04T22:15:46.541396] TimeoutHandler __enter__
16: Cleaning up all outstanding Run operations, waiting 300.0 seconds
17: 1 items cleaning up...
18: Cleanup took 0.1812913417816162 seconds
19: [2020-08-04T22:15:47.040203] TimeoutHandler __exit__
```

En la línea 8, verá el resultado "Hola mundo" .

El archivo de `70_driver_log.txt` contiene la salida estándar de una ejecución. Este archivo puede ser útil al depurar ejecuciones remotas en la nube.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, tomó un script "Hola mundo" sencillo y lo ejecutó en Azure. Vio cómo conectarse al área de trabajo de Azure Machine Learning, crear un experimento y enviar el código `hello.py` a la nube.

En el siguiente tutorial, se basará en estos aprendizajes para ejecutar algo más interesante que `print("Hello world!")`.

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento de un modelo](tutorial-1st-experiment-sdk-train.md)

>[!NOTE] 
> Si quiere finalizar la serie de tutoriales aquí y no avanzar al paso siguiente, recuerde [limpiar los recursos](tutorial-1st-experiment-bring-data.md#clean-up-resources).