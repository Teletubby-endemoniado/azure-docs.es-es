---
title: 'Tutorial: Detección de anomalías con Cognitive Services'
description: Aprenda a usar Cognitive Services para detectar anomalías en Azure Synapse Analytics.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 07/01/2021
author: nelgson
ms.author: negust
ms.custom: ignite-fall-2021
ms.openlocfilehash: 6afec907fb648e80034f22a4dfdb6fb94e0844a6
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132715822"
---
# <a name="tutorial-anomaly-detection-with-cognitive-services"></a>Tutorial: Detección de anomalías con Cognitive Services

En este tutorial, aprenderá a enriquecer fácilmente los datos de Azure Synapse Analytics con [Azure Cognitive Services](../../cognitive-services/index.yml). Además, utilizará [Anomaly Detector](../../cognitive-services/anomaly-detector/index.yml) para buscar anomalías. Un usuario de Azure Synapse puede seleccionar simplemente una tabla para enriquecer la detección de anomalías.

Esta tutorial abarca lo siguiente:

> [!div class="checklist"]
> - Pasos para obtener un conjunto de datos de una tabla de Spark que contiene datos de series temporales.
> - Uso de un asistente de Azure Synapse para enriquecer los datos con el servicio Anomaly Detector de Cognitive Services.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- Necesitará un [área de trabajo de Azure Synapse Analytics](../get-started-create-workspace.md) con una cuenta de almacenamiento de Azure Data Lake Storage Gen2 que esté configurada como almacenamiento predeterminado. Asegúrese de que es el *colaborador de datos de Storage Blob* en el sistema de archivos de Data Lake Storage Gen2 con el que trabaja.
- Grupo de Spark en el área de trabajo de Azure Synapse Analytics. Para más información, consulte el artículo sobre [creación de un grupo de Spark en Azure Synapse](../quickstart-create-sql-pool-studio.md).
- Haber completado los pasos de configuración previos que se detallan en este tutorial sobre la [configuración de Cognitive Services en Azure Synapse](tutorial-configure-cognitive-services-synapse.md).

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="create-a-spark-table"></a>Creación de una tabla de Spark

En este tutorial, es necesario tener una tabla de Spark.

1. Descargue el siguiente cuaderno, que contiene el código para generar una tabla de Spark: [prepare_anomaly_detector_data.ipynb](https://go.microsoft.com/fwlink/?linkid=2149577).

1. Cargue el archivo en el área de trabajo de Azure Synapse.

   ![Captura de pantalla en la que se muestran las opciones que deben seleccionarse para cargar un cuaderno.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00a.png)

1. Abra el archivo del cuaderno y seleccione **Run all** (Ejecutar todas) para ejecutar todas las celdas.

   ![Captura de pantalla en la que se muestran las opciones que deben seleccionarse para crear una tabla de Spark.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00b.png)

1. Ahora debe aparecer una tabla de Spark denominada **anomaly_detector_testing_data** en la base de datos de Spark predeterminada.

## <a name="open-the-cognitive-services-wizard"></a>Inicio del asistente de Cognitive Services

1. Haga clic con el botón derecho en la tabla de Spark que creó en el paso anterior. Seleccione **Machine Learning** > **Predict with a model** (Predecir con un modelo) para abrir el asistente.

   ![Captura de pantalla en la que se muestran las opciones que abren el Asistente para puntuación.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00g.png)

2. Aparecerá un panel de configuración y se le pedirá que seleccione un modelo de Cognitive Services. Seleccione **Anomaly Detector**.

   ![Captura de pantalla en la que se muestra que el modelo seleccionado es Anomaly Detector.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-00c.png)

## <a name="configure-anomaly-detector"></a>Configuración de Anomaly Detector

Especifique los datos siguientes para configurar Anomaly Detector:

- **Servicio vinculado de Azure Cognitive Services**: como parte de los pasos de requisitos previos, ha creado un servicio vinculado a [Cognitive Services](tutorial-configure-cognitive-services-synapse.md). Selecciónelo aquí.

- **Granularity** (Granularidad): velocidad a la que se muestrean los datos. Elija **monthly** (mensualmente). 

- **Timestamp column** (Columna de marca de tiempo): columna que contiene los datos temporales de la serie. Elija **timestamp (string)** .

- **Columna de valor de serie temporal**: columna que representa el valor de la serie en el momento especificado en la columna de marca de tiempo. Elija **value (double)** [valor (double)].

- **Columna de agrupación**: columna que agrupa la serie. Es decir, todas las filas que tienen el mismo valor en esta columna deben formar una serie temporal. Elija **group (string)** [grupo (string)].

Cuando haya terminado, seleccione **Open notebook** (Abrir cuaderno). De este modo, se generará automáticamente un cuaderno con el código de PySpark que realiza la detección de anomalías mediante Azure Cognitive Services.

![Captura de pantalla en la que se muestran los datos de configuración de Anomaly Detector.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-config.png)

## <a name="run-the-notebook"></a>Ejecución del cuaderno

El cuaderno que acaba de abrir utiliza la [biblioteca SynapseML](https://github.com/microsoft/SynapseML) para conectarse a Cognitive Services. El servicio vinculado de Azure Cognitive Services que proporcionó, le permite hacer referencia de forma segura a su servicio cognitivo desde esta experiencia sin revelar secretos.

Ahora puede ejecutar todas las celdas para realizar la detección de anomalías. Seleccione **Run all** (Ejecutar todas). [Descubra más información sobre el servicio Anomaly Detector de Cognitive Services](../../cognitive-services/anomaly-detector/index.yml).

![Captura de pantalla en la que se muestra la detección de anomalías.](media/tutorial-cognitive-services/tutorial-cognitive-services-anomaly-notebook.png)

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Análisis de sentimiento con Azure Cognitive Services](tutorial-cognitive-services-sentiment.md)
- [Tutorial: Asistente para puntuación de modelos de Machine Learning en grupos de SQL dedicados de Azure Synapse](tutorial-sql-pool-model-scoring-wizard.md)
- [Funcionalidades de aprendizaje automático en Azure Synapse Analytics](what-is-machine-learning.md)
