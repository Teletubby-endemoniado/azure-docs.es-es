---
title: 'Inicio rápido: Creación de recursos de área de trabajo'
titleSuffix: Azure Machine Learning
description: Cree un área de trabajo de Azure Machine Learning y recursos en la nube que se puedan usar para entrenar modelos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 10/21/2021
adobe-target: true
ms.custom: FY21Q4-aml-seo-hack, contperf-fy21q4
ms.openlocfilehash: 749400cc3f5c798b16dc08485f0cbff14cb276e7
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131556954"
---
# <a name="quickstart-create-workspace-resources-you-need-to-get-started-with-azure-machine-learning"></a>Inicio rápido: Creación de los recursos de área de trabajo necesarios para empezar a trabajar con Azure Machine Learning

En este inicio rápido, creará un área de trabajo a la que luego agregará recursos de proceso. Así tendrá todo lo necesario para empezar a trabajar con Azure Machine Learning.  

El área de trabajo es el recurso de nivel superior para las actividades de aprendizaje automático y proporciona un lugar centralizado para ver y administrar los artefactos que se crean al usar Azure Machine Learning. Los recursos de proceso proporcionan un entorno basado en la nube preconfigurado que permite entrenar, implementar, automatizar, administrar y realizar un seguimiento de los modelos de aprendizaje automático.


## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-the-workspace"></a>Creación del área de trabajo

Si ya tiene un área de trabajo, omita esta sección y vaya a [Creación de una instancia de proceso](#instance).

Si aún no tiene un área de trabajo, créela ahora:

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

## <a name="create-compute-instance"></a><a name="instance"></a>Creación de una instancia de proceso

Azure Machine Learning se puede instalar en el propio equipo.  Pero en este inicio rápido, creará un recurso de proceso en línea que ya tiene un entorno de desarrollo instalado y listo para usarse.  Esta máquina en línea, una *instancia de proceso,* la usará para que el entorno de desarrollo escriba y ejecute código en scripts de Python y cuadernos de Jupyter Notebook.

Cree una *instancia de proceso* para usar este entorno de desarrollo en el resto de los tutoriales e inicios rápidos.

1. Si no seleccionó **Ir a área de trabajo** en la sección anterior, inicie sesión en [Estudio de Azure Machine Learning](https://ml.azure.com) y seleccione el área de trabajo.
1. Seleccione **Proceso** en el lado izquierdo.
1. Seleccione **+Nuevo** para crear una nueva instancia de proceso.
1. Proporcione un valor y mantenga todos los valores predeterminados de la primera página.
1. Seleccione **Crear**.
 
En unos dos minutos, verá que el valor de **Estado** de la instancia de proceso cambia de *Creando* a *En ejecución*.  Ya está listo para empezar.  

## <a name="create-compute-clusters"></a><a name="cluster"></a>Creación de clústeres de proceso

A continuación, creará un clúster de proceso.  Los clústeres permiten distribuir un proceso de entrenamiento o inferencia de lotes en un clúster de nodos de proceso de CPU o GPU en la nube.

Cree un clúster de proceso que se escalará automáticamente entre cero y cuatro nodos:

1. Si salir de la sección **Proceso**, en la pestaña superior, seleccione **Clústeres de proceso**.
1. Seleccione **+Nuevo** para crear un nuevo clúster de proceso.
1. Mantenga todos los valores predeterminados de la primera página y seleccione **Siguiente**.
1. Asigne al clúster el nombre **cpu-cluster**.  Si este nombre ya existe, agréguele sus iniciales para que sea único.
1. Deje el valor de **Número mínimo de nodos** en 0.
1. Cambio el valor de **Número máximo de nodos** a 4, siempre que sea posible.  En función de la configuración, es posible que tenga un límite menor.
1. Cambie el valor de **Segundos de inactividad antes de la reducción vertical** a 2400.
1. Deje el resto de valores predeterminados y seleccione **Crear**.

En menos de un minuto, el valor del campo **Estado** del clúster cambiará de *Creando* a *Correcto*.  En la vista se muestra el clúster de proceso aprovisionado, junto con el número de nodos inactivos, de nodos ocupados y de nodos no aprovisionados.  Dado que aún no ha usado el clúster, los nodos no están aprovisionados actualmente. 

> [!NOTE]
> Cuando se cree el clúster, no tendrá ningún nodo aprovisionado. El clúster *no* genera costos hasta que envíe un trabajo. Este clúster se reducirá verticalmente cuando haya estado inactivo durante 2400 segundos (40 minutos).  Esto le dará el tiempo necesario para usarlo en algunos tutoriales, si así lo desea, sin tener esperar a que se vuelva a escalar verticalmente.

## <a name="quick-tour-of-the-studio"></a><a name="studio"></a> Un recorrido rápido por Studio

Studio es un portal web para Azure Machine Learning en el que se combinan las experiencias de trabajar si código y código primero para lograr una plataforma de ciencia de datos inclusiva.

Revise las partes de Studio en la barra de navegación izquierda:

* La sección **Autor** de Studio contiene varias maneras de empezar a crear modelos de aprendizaje automático.  Puede hacer lo siguiente:

    * La sección **Cuadernos** permite crear cuadernos de Jupyter Notebook, copiar cuadernos de ejemplo y ejecutar cuadernos y scripts de Python.
    * **ML automatizado** le permite recorrer un modelo de Machine Learning creado sin escribir código.
    * El **diseñador** proporciona una forma de crear modelos mediante componentes precompilados a través del método de arrastrar y colocar.

* La sección **Recursos** de Studio le ayuda a realizar un seguimiento de los recursos que crea al ejecutar los trabajos.  Si tiene un área de trabajo nueva, estas secciones no contendrán absolutamente nada.

* Ya ha usado la sección **Administrar** de Studio para crear los recursos de proceso.  Esta sección también le permite crear y administrar datos y servicios externos que vincular a su área de trabajo.  

### <a name="workspace-diagnostics"></a>Diagnóstico del área de trabajo

[!INCLUDE [machine-learning-workspace-diagnostics](../../includes/machine-learning-workspace-diagnostics.md)]

## <a name="clean-up-resources"></a><a name="clean-up"></a>Limpieza de recursos

Si planea continuar ahora con el siguiente tutorial, vaya a [Pasos siguientes](#next-steps).

### <a name="stop-compute-instance"></a>Detención de una instancia de proceso

Si no va a utilizar ahora la instancia de proceso, deténgala:

1. En Studio, a la izquierda, seleccione **Proceso**.
1. En las pestañas superiores, seleccione **Instancia de proceso**.
1. Seleccione la instancia de proceso en la lista.
1. En la barra de herramientas superior, seleccione **Detener**.

### <a name="delete-all-resources"></a>Eliminación de todos los recursos

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora tiene un área de trabajo de Azure Machine Learning que contiene:

- Una instancia de proceso que va a usar en el entorno de desarrollo.
- Un clúster de proceso que va a usar para enviar ejecuciones de entrenamiento.

Use estos recursos para más información sobre Azure Machine Learning y entrenar un modelo con scripts de Python.

> [!div class="nextstepaction"]
> [Más información con scripts de Python](tutorial-1st-experiment-hello-world.md)
>
