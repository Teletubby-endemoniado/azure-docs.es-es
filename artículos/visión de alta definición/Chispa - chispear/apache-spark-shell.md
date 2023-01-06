---librar = ENTERO
Registro ID del check-in a editar (OBLIGATORIO)
title: Uso de un Shell de Spark interactivo en Azure HDInsightct
description: Un Shell de Spark interactivo proporciona un proceso de impresión de lectura-ejecución para ejecutar comandos de Spark puntuales y ver los resultados.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 02/10/2020
ms.openlocfilehash: 324852a967b5de015a9b1e9b465d4b4703e573cb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2021
ms.locfileid: "98929688"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>Ejecución de Apache Spark desde el shell de Spark

Un shell interactivo de [Apache Spark](https://spark.apache.org/) proporciona un entorno REPL (bucle leer, ejecutar e imprimir) para ejecutar comandos Spark de uno en uno y ver los resultados. Este proceso es útil para tareas de desarrollo y depuración. Spark proporciona un shell para cada uno de los lenguajes que admite: Scala, Python y R.

## <a name="run-an-apache-spark-shell"></a>Ejecución del shell de Apache Spark

1. Use el [comando SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) para conectarse al clúster. Modifique el comando siguiente: reemplace CLUSTERNAME por el nombre del clúster y, luego, escriba el comando:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Spark proporciona shells para Scala (spark-shell) y Python (pyspark). En su sesión SSH, escriba *uno* de los siguientes comandos:

    ```bash
    spark-shell

    # Optional configurations
    # spark-shell --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4
    ```

    ```bash
    pyspark

    # Optional configurations
    # pyspark --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4
    ```

    Si tiene previsto usar una configuración opcional, asegúrese de revisar primero la [excepción OutOfMemoryError de Apache Spark](./apache-spark-troubleshoot-outofmemory.md).

1. Algunos comandos básicos de ejemplo. Elija el lenguaje pertinente:

    ```spark-shell
    val textFile = spark.read.textFile("/example/data/fruits.txt")
    textFile.first()
    textFile.filter(line => line.contains("apple")).show()
    ```

    ```pyspark
    textFile = spark.read.text("/example/data/fruits.txt")
    textFile.first()
    textFile.filter(textFile.value.contains("apple")).show()
    ```

1. Consulte un archivo CSV. Tenga en cuenta que el lenguaje siguiente funciona para `spark-shell` y `pyspark`.

    ```scala
    spark.read.csv("/HdiSamples/HdiSamples/SensorSampleData/building/building.csv").show()
    ```

1. Consulte un archivo CSV y almacene los resultados en una variable:

    ```spark-shell
    var data = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/HdiSamples/HdiSamples/SensorSampleData/building/building.csv")
    ```

    ```pyspark
    data = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/HdiSamples/HdiSamples/SensorSampleData/building/building.csv")
    ```

1. Visualización de resultados:

    ```spark-shell
    data.show()
    data.select($"BuildingID", $"Country").show(10)
    ```

    ```pyspark
    data.show()
    data.select("BuildingID", "Country").show(10)
    ```

1. Salir

    ```spark-shell
    :q
    ```

    ```pyspark
    exit()
    ```

## <a name="sparksession-and-sparkcontext-instances"></a>Instancias de objetos SparkSession y SparkContext

De forma predeterminada, cuando se ejecute el shell de Spark, se crea automáticamente las instancias de los objetos SparkSession y SparkContext.

Para acceder a la instancia de SparkSession, escriba `spark`. Para acceder a la instancia de SparkContext, escriba `sc`.

## <a name="important-shell-parameters"></a>Parámetros de shell importantes

El comando de shell de Spark (`spark-shell` o `pyspark`) admite muchos parámetros de línea de comandos. Para ver una lista completa de parámetros, inicie el shell de Spark con el modificador `--help`. Algunos de estos parámetros solo se pueden aplicar a `spark-submit`, que es el que incluye el shell de Spark.

| switch | description | ejemplo |
| --- | --- | --- |
| --master MASTER_URL | Especifica la URL principal. En HDInsight, este valor es siempre `yarn`. | `--master yarn`|
| --jars JAR_LIST | Lista separada por comas de archivos JAR locales que se incluirán en las rutas de clase del controlador y el ejecutor. En HDInsight, esta lista se compone de las rutas de acceso al sistema de archivos predeterminado en Azure Storage o Data Lake Storage. | `--jars /path/to/examples.jar` |
| --packages MAVEN_COORDS | Lista separada por comas de coordenadas de Maven de archivos JAR locales que se incluirán en las rutas de clase del controlador y el ejecutor. Busca en el repositorio de Maven local, luego, en el central y, después, en cualquier repositorio remoto adicional especificado con `--repositories`. El formato de las coordenadas es *idGrupo*:*idArtefacto*:*versión*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py-files LIST | Solo para Python; lista separada por comas de los archivos .zip, .egg o .py que se colocarán en PYTHONPATH. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [Introducción a Apache Spark en Azure HDInsight](apache-spark-overview.md).
- Si quiere trabajar con SparkSQL y los clústeres de Spark, lea [Creación de un clúster de Apache Spark en Azure HDInsight](apache-spark-jupyter-spark-sql.md).
- Para escribir aplicaciones que procesen datos de streaming con Spark, consulte el artículo que explica [qué es el streaming estructurado de Apache Spark](apache-spark-streaming-overview.md).
