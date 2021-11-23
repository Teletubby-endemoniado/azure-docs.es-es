---
title: 'Qué es Apache Spark: Azure HDInsight'
description: En este artículo se proporciona una introducción a Spark en HDInsight y los diferentes escenarios en los que puede usar un clúster de Spark en HDInsight.
ms.service: hdinsight
ms.custom: contperf-fy21q1
ms.topic: overview
ms.date: 11/17/2020
ms.openlocfilehash: ed6f7f30fde528d5829dd52d24043d33a0fc913a
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132708435"
---
# <a name="what-is-apache-spark-in-azure-hdinsight"></a>Qué es Apache Spark en Azure HDInsight

Apache Spark es una plataforma de procesamiento paralelo que admite el procesamiento en memoria para mejorar el rendimiento de aplicaciones de análisis de macrodatos. Apache Spark en Azure HDInsight es la implementación de Microsoft de Apache Spark en la nube, y es una de las varias ofertas de Spark en Azure.

* Apache Spark en Azure HDInsight facilita la creación y configuración de clústeres de Spark, lo que le permite personalizar y usar un entorno completo de Spark en Azure.

* Los [grupos de Spark Azure Synapse Analytics](../../synapse-analytics/quickstart-create-apache-spark-pool-portal.md) usan grupos de Spark administrados para permitir que los datos se carguen, modelen, procesen y distribuyen para obtener información analítica en Azure.

* [Apache Spark en Azure Databricks](/databricks/getting-started/spark.md) usa clústeres de Spark para proporcionar un área de trabajo interactiva que permita la colaboración entre los usuarios para leer datos de varios orígenes de datos y convertirlos en conclusiones innovadoras.

* Las [actividades de Spark Azure Data Factory](../../data-factory/transform-data-using-spark.md) permiten usar análisis de Spark en la canalización de datos, mediante clústeres de Spark a petición o existentes previamente.


Con Apache Spark en Azure HDInsight, puede almacenar y procesar los datos dentro de Azure. Los clústeres de Spark en HDInsight son compatibles con [Azure Blob Storage](../../storage/common/storage-introduction.md), [Azure Data Lake Storage Gen1](../../data-lake-store/data-lake-store-overview.md) o [Azure Data Lake Storage Gen2](../../storage/blobs/data-lake-storage-introduction.md), lo que le permite aplicar el procesamiento de Spark en los almacenes de datos existentes.

:::image type="content" source="./media/apache-spark-overview/hdinsight-spark-overview.svg" alt-text="Spark: un marco unificado" lightbox="./media/apache-spark-overview/hdinsight-spark-overview.svg":::

Para empezar a trabajar con Apache Spark en Azure HDInsight, siga nuestro [tutorial para crear clústeres de HDInsight Spark](apache-spark-jupyter-spark-sql-use-portal.md).

Para obtener información sobre Apache Spark y cómo interactúa con Azure, siga leyendo el artículo siguiente.

Para conocer los componentes y la información de versiones, consulte el artículo [¿Cuáles son los componentes y versiones de Apache Hadoop disponibles con HDInsight?](../hdinsight-component-versioning.md).

## <a name="what-is-apache-spark"></a>¿Qué es Apache Spark?

Spark proporciona primitivas de computación de clúster en memoria. Un trabajo de Spark puede cargar y almacenar en la memoria caché datos, y repetir consultas sobre ellos. La informática en memoria es mucho más rápida que las aplicaciones basadas en disco, como Hadoop, que comparten datos mediante el sistema de archivos distribuido de Hadoop. Spark también se integra en el lenguaje de programación de Scala para que pueda manipular conjuntos de datos distribuidos como colecciones locales. No se necesita estructurar todo como operaciones de asignación y reducción.

:::image type="content" source="./media/apache-spark-overview/map-reduce-vs-spark.svg" alt-text="Comparación del tradicional MapReduce y Spark" lightbox="./media/apache-spark-overview/map-reduce-vs-spark.svg":::

Los clústeres de Spark en HDInsight ofrecen un servicio de Spark completamente administrado. Aquí se enumeran las ventajas de crear un clúster de Spark en HDInsight.

| Característica | Descripción |
| --- | --- |
| Creación sencilla |Puede crear un clúster de Spark en HDInsight en cuestión de minutos mediante Azure Portal, Azure PowerShell o el SDK de .NET de HDInsight. Consulte [Introducción al clúster de Apache Spark en HDInsight](apache-spark-jupyter-spark-sql-use-portal.md). |
| Facilidad de uso |El clúster de Spark en HDInsight incluye cuadernos de Jupyter Notebook y cuadernos de Apache Zeppelin. Puede usar estos notebooks para el procesamiento y la visualización de datos interactivos. Consulte [Uso de cuadernos de Apache Zeppelin con un clúster de Apache Spark](apache-spark-zeppelin-notebook.md) y [Carga de datos y ejecución de consultas en un clúster de Apache Spark](apache-spark-load-data-run-query.md).|
| API de REST |Los clústeres de Spark en HDInsight incluyen [Apache Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), un servidor de trabajos de Spark basado en una API REST para enviar y supervisar trabajos de forma remota. Consulte [Uso de la API REST de Apache Spark para enviar trabajos remotos a un clúster de HDInsight Spark](apache-spark-livy-rest-interface.md).|
| Compatibilidad con Azure Storage | Los clústeres de Spark en HDInsight pueden usar Azure Data Lake Storage Gen1/Gen2 como almacenamiento principal o almacenamiento adicional. Para más información sobre Data Lake Storage Gen1, consulte [Azure Data Lake Storage Gen1](../../data-lake-store/data-lake-store-overview.md). Para más información sobre Data Lake Storage Gen2, consulte [Azure Data Lake Storage Gen2](../../storage/blobs/data-lake-storage-introduction.md).|
| Integración con servicios de Azure |Un clúster de Spark en HDInsight incluye un conector a Azure Event Hubs. Las aplicaciones de streaming se pueden compilar mediante Event Hubs, lo que incluye Apache Kafka, que ya está disponible como parte de Spark. |
| Integración con IDE de terceros | HDInsight ofrece varios complementos de IDE que son útiles para crear y enviar aplicaciones a un clúster de Spark en HDInsight. Para más información, consulte [Uso de Azure Toolkit for IntelliJ IDEA](apache-spark-intellij-tool-plugin.md), [Uso de las herramientas de Spark y Hive para VSCode](../hdinsight-for-vscode.md) y [Uso de Azure Toolkit for Eclipse](apache-spark-eclipse-tool-plugin.md).|
| Consultas simultáneas |Los clústeres de Spark en HDInsight admiten consultas simultáneas. Esta funcionalidad permite que varias consultas de un usuario o varias consultas de diversos usuarios y aplicaciones compartan los mismos recursos de clúster. |
| Almacenamiento en caché en SSD |Puede almacenar datos en caché, ya sea en memoria o en SSD conectadas a los nodos del clúster. El almacenamiento en memoria caché ofrece el mejor rendimiento de las consultas, pero también podría ser costoso. El almacenamiento en memoria caché en SSD ofrece una opción excelente para mejorar el rendimiento de las consultas sin necesidad de crear un clúster de un tamaño que admita todo el conjunto de datos en memoria. Consulte [Mejora del rendimiento de las cargas de trabajo de Apache Spark con la memoria caché de E/S de Azure HDInsight](apache-spark-improve-performance-iocache.md). |
| Integración con herramientas de BI |Los clústeres de Spark en HDInsight ofrecen conectores para herramientas de BI, como Power BI, para el análisis de datos. |
| Bibliotecas de Anaconda precargadas |Los clústeres de Spark en HDInsight incluyen bibliotecas de Anaconda preinstaladas. [Anaconda](https://docs.continuum.io/anaconda/) ofrece casi 200 bibliotecas para el aprendizaje automático, el análisis de datos, la visualización, etc. |
| Adaptabilidad | HDInsight permite cambiar el número de nodos de clúster de forma dinámica con la característica Escalabilidad automática. Consulte [Escalado automático de clústeres de Azure HDInsight](../hdinsight-autoscale-clusters.md). Además, los clústeres de Spark se pueden quitar sin que se pierdan datos, ya que todos los datos se almacenan en Azure Blob Storage, [Azure Data Lake Storage Gen1](../../data-lake-store/data-lake-store-overview.md) o [Azure Data Lake Storage Gen2](../../storage/blobs/data-lake-storage-introduction.md). |
| Contrato de nivel de servicio |Los clústeres de Spark en HDInsight incluyen soporte técnico ininterrumpido y un SLA del 99,9 % de tiempo de actividad. |

Los clústeres de Apache Spark en HDInsight incluyen los siguientes componentes que están disponibles en los clústeres de manera predeterminada.

* [Spark Core](https://spark.apache.org/docs/latest/). Incluye Spark Core, Spark SQL, API Spark de streaming, GraphX y MLlib.
* [Anaconda](https://docs.continuum.io/anaconda/)
* [Apache Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
* [Jupyter Notebook](https://jupyter.org)
* [Apache Zeppelin Notebook](http://zeppelin-project.org/)

HDInsight Spark agrupa en clústeres un [controlador ODBC](/sql/connect/odbc/download-odbc-driver-for-sql-server) para obtener conectividad de herramientas de BI como Microsoft Power BI.

## <a name="spark-cluster-architecture"></a>Arquitectura de clúster de Spark

:::image type="content" source="./media/apache-spark-overview/hdi-spark-architecture.svg" alt-text="La arquitectura de HDInsight Spark" lightbox="./media/apache-spark-overview/hdi-spark-architecture.svg":::

Los componentes de Spark son fáciles de entender si se conoce la manera en que Spark se ejecuta en clústeres de HDInsight.

Las aplicaciones de Spark se ejecutan como conjuntos de procesos independientes en un clúster. Coordinado por el objeto SparkContext en el programa principal (denominado programa de controladores).

SparkContext puede conectarse a varios tipos de administradores de clúster, lo que da recursos entre las aplicaciones. Estos administradores de clúster incluyen Apache Mesos, Apache Hadoop YARN o el administrador de clústeres de Spark. En HDInsight, Spark se ejecuta mediante el Administrador de clústeres YARN. Una vez conectado, Spark adquiere los ejecutores en nodos de trabajo en el clúster, que son procesos que ejecutan cálculos y almacenan los datos de la aplicación. A continuación, envía el código de la aplicación (definido por archivos JAR o Python que se pasan a SparkContext) a los ejecutores. Por último, SparkContext envía las tareas a los ejecutores para la ejecución.

SparkContext ejecuta la función principal del usuario y las distintas operaciones paralelas en los nodos de trabajo. A continuación, SparkContext recopila los resultados de las operaciones. Los nodos de trabajo leen y escriben datos del sistema de archivos distribuido de Hadoop y también en él. Además, los nodos de trabajo almacenan en caché datos transformados en memoria como conjuntos de datos distribuidos resistentes (RDD).

SparkContext se conecta al maestro de Spark y es responsable de convertir una aplicación en un grafo acíclico dirigido (DAG) de tareas individuales. Tareas que se ejecutan en un proceso ejecutor en los nodos de trabajo. Cada aplicación obtiene sus propios procesos de ejecutor. Los que permanecen durante toda la aplicación y ejecutan tareas en varios subprocesos.

## <a name="spark-in-hdinsight-use-cases"></a>Casos de uso de Spark en HDInsight

Los clústeres de Spark en HDInsight hacen posibles los siguientes escenarios clave:

### <a name="interactive-data-analysis-and-bi"></a>Análisis interactivo de datos y BI

Apache Spark en HDInsight almacena los datos en Azure Blob Storage, Azure Data Lake Gen1 o Azure Data Lake Storage Gen2. Los expertos empresariales y responsables de la toma de decisiones clave pueden analizar y crear informes en base a esos datos. Y utilizan Microsoft Power BI para crear informes interactivos a partir de los datos analizados. Los analistas pueden comenzar a partir de datos no estructurados y semiestructurados presentes en el almacenamiento de clúster, definir un esquema de los datos mediante notebooks y luego generar modelos de datos mediante Microsoft Power BI. Los clústeres de Spark en HDInsight también admiten varias herramientas de BI de terceros. Una de ellas es Tableau, que facilita a los analistas de datos, expertos empresariales y responsables de la toma de decisiones clave.

* [Tutorial: Visualización de datos de Spark mediante Power BI](apache-spark-use-bi-tools.md)

### <a name="spark-machine-learning"></a>Machine Learning con Spark

Apache Spark incluye [MLlib](https://spark.apache.org/mllib/). MLlib es una biblioteca de aprendizaje automático basada en Spark que puede usar desde un clúster de Spark en HDInsight. El clúster de Spark en HDInsight también incluye Anaconda, una distribución de Python con diversos tipos de paquetes para el aprendizaje automático. Y con la compatibilidad integrada con cuadernos de Jupyter Notebook y Zeppelin, dispone de un entorno para la creación de aplicaciones de aprendizaje automático.

* [Tutorial: Predicción de las temperaturas de edificios con datos HVAC](apache-spark-ipython-notebook-machine-learning.md)  
* [Tutorial: Predicción de resultados de inspección alimentaria](apache-spark-machine-learning-mllib-ipython.md)

### <a name="spark-streaming-and-real-time-data-analysis"></a>Streaming y análisis de datos en tiempo real con Spark

Los clústeres de Spark en HDInsight ofrecen amplia compatibilidad para crear soluciones de análisis en tiempo real. Spark ya tiene conectores para ingerir datos de muchos orígenes, como Kafka, Flume, Twitter, ZeroMQ o sockets TCP. Spark en HDInsight agrega compatibilidad de primera clase para la ingesta de datos de Azure Event Hubs. Event Hubs son el servicio de cola más usado de Azure. La disponibilidad de compatibilidad completa con Event Hubs convierte a los clústeres de Spark en HDInsight en una plataforma ideal para crear una canalización de análisis en tiempo real.

* [Introducción a Apache Spark Streaming](apache-spark-streaming-overview.md)
* [Información general acerca de Apache Spark Structured Streaming](apache-spark-structured-streaming-overview.md)

## <a name="next-steps"></a>Pasos siguientes

En esta introducción, se le ha proporcionado un conocimiento básico de Apache Spark en Azure HDInsight.  Puede usar los siguientes artículos para más información sobre Apache Spark en HDInsight y puede crear un clúster de HDInsight Spark y ejecutar algunas consultas de Spark de ejemplo:

* [Inicio rápido: Creación de un clúster de Apache Spark en Azure HDInsight](./apache-spark-jupyter-spark-sql-use-portal.md)
* [Tutorial: Carga de datos y ejecución de consultas en un trabajo de Apache Spark mediante Jupyter](./apache-spark-load-data-run-query.md)
* [Tutorial: Visualización de datos de Spark mediante Power BI](apache-spark-use-bi-tools.md)
* [Tutorial: Predicción de las temperaturas de edificios con datos HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Optimización de trabajos de Spark para mejorar el rendimiento](apache-spark-perf.md)
