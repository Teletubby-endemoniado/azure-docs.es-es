---
title: 'Optimización del rendimiento: Storm con Azure Data Lake Storage Gen1'
description: Descripción de los factores que se deben tener en cuenta al optimizar el rendimiento de una topología de Azure Storm, incluida la solución de problemas comunes.
author: normesta
ms.service: data-lake-store
ms.topic: how-to
ms.date: 12/19/2016
ms.author: normesta
ms.openlocfilehash: 8b90b6e6ca6867ea397ac6fca5abe22dd5c0c86b
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128615916"
---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-storage-gen1"></a>Guía para la optimización del rendimiento de Storm en HDInsight y Azure Data Lake Storage Gen1

Esta guía le ayuda a comprender los factores que se deben tener en cuenta al optimizar el rendimiento de una topología de Azure Storm. Por ejemplo, es importante comprender las características del trabajo realizado por los spouts y los bolts (si el trabajo hace un uso intensivo de memoria o E/S). En este artículo se describen varias directrices de optimización del rendimiento y se incluyen soluciones a problemas comunes.

## <a name="prerequisites"></a>Prerrequisitos

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
* **Una cuenta de Azure Data Lake Storage Gen1**. Para instrucciones sobre cómo crear una, consulte la [introducción a Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md).
* **Clúster de Azure HDInsight** con acceso a una cuenta de Data Lake Storage Gen1. Consulte [Creación de un clúster de HDInsight con Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md). Asegúrese de habilitar el Escritorio remoto para el clúster.
* **Ejecución de un clúster de Storm en Data Lake Storage Gen1**. Para más información, consulte [Storm en HDInsight](../hdinsight/storm/apache-storm-overview.md).
* **Guía para la optimización del rendimiento en Data Lake Storage Gen1**.  Para conocer los conceptos generales sobre rendimiento, consulte [Guía para la optimización del rendimiento de Data Lake Storage Gen1](./data-lake-store-performance-tuning-guidance.md).  

## <a name="tune-the-parallelism-of-the-topology"></a>Ajuste del paralelismo de la topología

Es posible que pueda mejorar el rendimiento si aumenta la simultaneidad de E/S hacia y desde Data Lake Storage Gen1. Una topología de Storm tiene un conjunto de configuraciones que determinan el paralelismo:
* Número de procesos de trabajo: los trabajos se distribuyen uniformemente entre las máquinas virtuales
* Número de instancias de ejecutor de spout
* Número de instancias de ejecutor de bolt
* Número de tareas de spout
* Número de tareas de bolt

Por ejemplo, en un clúster con 4 máquinas virtuales y 4 procesos de trabajo, 32 ejecutores de spout, 32 tareas de spout, 256 ejecutores de bolt y 512 tareas de bolt, tenga en cuenta lo siguiente:

Cada supervisor, que es un nodo de trabajo, tiene un solo proceso de trabajo de máquina virtual Java (JVM). Este proceso de JVM administra 4 subprocesos de spout y 64 subprocesos de bolt. Dentro de cada subproceso, las tareas se ejecutan secuencialmente. Con la configuración anterior, cada subproceso de spout tiene una tarea y cada subproceso de bolt tiene dos tareas.

En Storm, estos son los diversos componentes implicados y así es cómo afectan al nivel de paralelismo que tiene:
* El nodo principal (denominado Nimbus en Storm) se utiliza para enviar y administrar los trabajos. Estos nodos afectan al grado de paralelismo.
* Nodos de supervisor. En HDInsight, corresponde a un nodo de trabajo de una máquina virtual de Azure.
* Las tareas de trabajo son procesos de Storm que se ejecutan en las VM. Cada tarea de trabajo corresponde a una instancia de JVM. Storm distribuye el número de procesos de trabajo que especifique a los nodos de trabajo lo más uniformemente posible.
* Instancias de ejecutor de bolt y spout. Cada instancia de ejecutor corresponde a un subproceso que se ejecuta dentro de los trabajos (máquinas virtuales Java).
* Tareas de Storm. Son tareas lógicas que ejecuta cada uno de estos subprocesos. Esto no cambia el nivel de paralelismo, por lo que se debe evaluar si necesita o no varias tareas por ejecutor.

### <a name="get-the-best-performance-from-data-lake-storage-gen1"></a>Obtener el mejor rendimiento de Data Lake Storage Gen1

Cuando trabaje con Data Lake Storage Gen1, obtendrá el mejor rendimiento con estas sugerencias:
* Fusione sus anexos más pequeños en tamaños más grandes (lo ideal son 4 MB).
* Realice tantas solicitudes simultáneas como pueda. Como cada subproceso de bolt realiza lecturas de bloqueo, querrá tener entre 8 y 12 subprocesos por núcleo. De esta manera el nivel de utilización de la NIC y la CPU es adecuado. Una máquina virtual más grande permite más solicitudes simultáneas.  

### <a name="example-topology"></a>Topología de ejemplo

Supongamos que tiene un clúster de ocho nodos de trabajo con una VM de Azure D13v2. Esta VM tiene ocho núcleos, así que entre los ocho nodos de trabajo, tiene un total de 64 núcleos.

Digamos que tenemos ocho subprocesos de bolt por núcleo. Como hay 64 núcleos, significa que queremos 512 instancias de ejecutor de bolt (es decir, subprocesos) en total. En este caso, supongamos que empezamos con una JVM por máquina virtual y usamos principalmente la simultaneidad de subprocesos dentro de la JVM para conseguir simultaneidad. Esto significa que necesitamos ocho tareas de trabajo (una por cada VM de Azure) y 512 ejecutores de bolt. Dada esta configuración, Storm intenta distribuir los trabajos uniformemente entre los nodos de trabajo (también conocidos como nodos de supervisor), lo que da a cada nodo de trabajo una JVM. Ahora, dentro de los supervisores, Storm intenta distribuir los ejecutores uniformemente entre los supervisores, lo que da a cada supervisor (es decir, a cada JVM) ocho subprocesos.

## <a name="tune-additional-parameters"></a>Ajuste de parámetros adicionales
Una vez que tenga la topología básica, puede valorar si desea modificar alguno de los parámetros:
* **Number of JVMs per worker node** (Número total de JVM por nodo de trabajo). Si tiene una estructura de datos de gran tamaño (por ejemplo, una tabla de búsqueda) que se hospeda en memoria, cada JVM requiere una copia independiente. Como alternativa, puede usar la estructura de datos en muchos subprocesos si tiene menos JVM. Para la E/S del bolt, el número de JVM no supone una gran diferencia con respecto al número de subprocesos agregados en esas JVM. Para simplificar, es una buena idea tener una JVM por trabajo. No obstante, dependiendo de lo que haga el bolt o del procesamiento de aplicaciones que requiera, puede que necesite cambiar este número.
* **Number of spout executors** (Número de ejecutores de spout) Como en el ejemplo anterior se usan bolts para escribir en Data Lake Storage Gen1, el número de spouts no está relacionado directamente con el rendimiento del bolt. Sin embargo, según la cantidad de procesamiento o E/S que se realice en el spout, es una buena idea ajustar los spouts para mejorar el rendimiento. Asegúrese de tener spouts suficientes para que los bolts se mantengan ocupados. Las velocidades de salida de los spouts deben coincidir con el rendimiento de los bolts. La configuración real depende del spout.
* **Number of tasks** (Número de tareas) Cada bolt se ejecuta como un único subproceso. Las tareas adicionales por bolt no proporcionan ninguna simultaneidad adicional. La única vez que resultan útiles es cuando el proceso de confirmación de la tupla usa una gran proporción del tiempo de ejecución de los bolts. Es una buena idea agrupar muchas tuplas en un anexo más grande antes de enviar una confirmación del bolt. Por lo tanto, en la mayoría de los casos, varias tareas no proporcionan ninguna ventaja adicional.
* **Local or shuffle grouping** (Agrupación local o aleatoria) Cuando esta opción está habilitada, las tuplas se envían a bolts dentro del mismo proceso de trabajo. Esta opción reduce las llamadas de red y la comunicación entre procesos. Se recomienda su uso para la mayoría de las topologías.

Este escenario básico es un buen punto de partida. Pruebe con sus propios datos para ajustar los parámetros anteriores con el fin de alcanzar un rendimiento óptimo.

## <a name="tune-the-spout"></a>Optimización del spout

Puede modificar los valores de configuración siguientes para optimizar el spout.

- **Tuple timeout (Tiempo de espera de tupla): topology.message.timeout.secs**. Este valor determina la cantidad de tiempo que un mensaje tarda en completarse y en recibir una confirmación, antes de que se considere erróneo.

- **Max memory per worker process (Memoria máxima por proceso de trabajo): worker.childopts**. Este valor le permite especificar parámetros de línea de comandos adicionales para los trabajos de Java. El ajuste usado más frecuentemente aquí es XmX, que determina la memoria máxima asignada al montón de una JVM.

- **Max spout pending (Spouts máximos pendientes): topology.max.spout.pending**. Este valor determina el número de tuplas que pueden estar en vuelo (que aún no se han confirmado en todos los nodos de la topología) por subproceso de spout en un momento concreto.

  Un buen cálculo sería estimar el tamaño de cada una de sus tuplas. Después, averigüe cuánta memoria tiene un subproceso de spout. La memoria total asignada a un subproceso dividida por este valor debe proporcionarle el límite superior para el parámetro de spouts máximos pendientes.

## <a name="tune-the-bolt"></a>Optimización del bolt
Al escribir en Data Lake Storage Gen1, establezca una directiva de sincronización de tamaño (el búfer en el lado cliente) en 4 MB. Así, se realizará un vaciado o hsync() solamente cuando el tamaño del búfer sea el valor anterior. El controlador de Data Lake Storage Gen1 en la máquina virtual de trabajo realiza automáticamente este almacenamiento en búfer, a menos que ejecute explícitamente hsync().

El bolt predeterminado de Storm de Data Lake Storage Gen1 tiene un parámetro de directiva de sincronización de tamaño (fileBufferSize) que puede usarse para optimizar este parámetro.

En topologías con uso intensivo de E/S, se recomienda que cada subproceso de bolt escriba en su propio archivo y que establezca una directiva de rotación de archivos (fileRotationSize). Cuando el archivo alcanza un determinado tamaño, la transmisión se vacía automáticamente y se escribe en un nuevo archivo. El tamaño de archivo recomendado por rotación es de 1 GB.

### <a name="handle-tuple-data"></a>Administración de los datos de la tupla

En Storm, un spout se mantiene en una tupla hasta que el bolt lo confirma de forma explícita. Si el bolt ha leído una tupla pero no la ha confirmado aún, es posible que el spout no se haya guardado en el back-end de Data Lake Storage Gen1. Cuando se confirma una tupla, el bolt garantiza la persistencia del spout, por lo que puede eliminar los datos de origen de cualquier origen que esté leyendo.  

Para obtener el mejor rendimiento en Data Lake Storage Gen1, el búfer del bolt debe tener 4 MB de datos de tupla. Luego, escriba en el back-end de Data Lake Storage Gen1 como una escritura de 4 MB. Cuando los datos se hayan escrito correctamente en el almacén (mediante la llamada a hflush()), el bolt puede confirmar los datos al spout. Esto es lo que hace el bolt de ejemplo aquí indicado. También es aceptable conservar un mayor número de tuplas antes de realizar la llamada hflush() y de que se confirmen las tuplas. Sin embargo, esto aumenta el número de tuplas en vuelo que el spout debe conservar y, por lo tanto, aumenta la cantidad de memoria necesaria por cada JVM.

> [!NOTE]
> Puede que las aplicaciones tengan la necesidad de confirmar tuplas más a menudo (con tamaños de datos inferiores a 4 MB) por otros motivos no relacionados con el rendimiento. Sin embargo, eso podría afectar al rendimiento de E/S en el back-end de almacenamiento. Por lo tanto, sopese con cuidado este inconveniente con el rendimiento de E/S del bolt.

Si la velocidad de entrada de las tuplas no es alta, por lo que el búfer de 4 MB tarda mucho tiempo en llenarse, considere estas posibilidades para solucionarlo:
* Reducir el número de bolts para que haya menos búferes que llenar.
* Tener una directiva basada en recuento o en tiempo, donde se desencadene un hflush() cada X vaciados o cada y milisegundos y se confirmen las tuplas acumuladas hasta entonces.

Tenga en cuenta que el rendimiento en este caso es menor, aunque con una tasa de eventos lenta, el objetivo principal no es obtener un rendimiento máximo de todos modos. Estas soluciones le ayudan a reducir el tiempo total que tarda una tupla en atravesar el almacén. Esto podría ser importante si quiere una canalización en tiempo real incluso con una tasa baja de eventos. Tenga en cuenta también que si la tasa de tuplas entrantes es baja, debe ajustar el parámetro topology.message.timeout_secs, de modo que el tiempo de espera de las tuplas no se agote mientras se estén almacenando en búfer o procesando.

## <a name="monitor-your-topology-in-storm"></a>Supervisión de la topología en Storm  
Mientras se ejecuta la topología, puede supervisarla en la interfaz de usuario de Storm. Estos son los principales parámetros que debe examinar:

* **Total process execution latency (Latencia total de ejecución de procesos).** Es el tiempo medio que una tupla tarda en que la emita el spout, la procese el bolt y se confirme.

* **Total bolt process latency (Latencia total de proceso del bolt).** Es el tiempo medio que la tupla pasa en el bolt hasta que recibe una confirmación.

* **Total bolt execute latency (Latencia total de ejecución del bolt).** Es el tiempo medio empleado por el bolt en el método de ejecución.

* **Number of failures (Número de errores).** Se refiere al número de tuplas que no se pudieron procesar por completo antes de que se agotara el tiempo de espera.

* **Capacity (Capacidad).** Se trata de una medida de lo ocupado que está su sistema. Si este número es 1, los bolts trabajan tan rápido como pueden. Si es menor que 1, aumente el paralelismo. Si es mayor que 1, reduzca el paralelismo.

## <a name="troubleshoot-common-problems"></a>Solución de problemas comunes
Estos son algunos escenarios comunes de solución de problemas.
* **El tiempo de espera de muchas tuplas se está agotando.** Examine cada nodo de la topología para determinar dónde está el cuello de botella. El motivo más habitual de ello es que los bolts no puedan seguir el ritmo de los spouts. Como consecuencia, las tuplas quedan atascadas en los búferes internos a la espera de ser procesadas. Considere la posibilidad de aumentar el valor de tiempo de espera o de reducir el número máximo de spouts pendientes.

* **Existe una latencia alta total en la ejecución de los procesos, pero una latencia baja en los procesos de bolt.** En este caso, es posible que las tuplas no se reconozcan lo suficientemente rápido. Compruebe que hay un número suficiente de elementos de reconocimiento. Otra posibilidad es que esperen en la cola demasiado tiempo hasta que los bolts empiezan a procesarlas. Reduzca el máximo de spouts pendientes.

* **Existe una latencia alta en la ejecución de bolts.** Esto significa que el método execute() de su bolt está tardando demasiado. Optimice el código o examine el comportamiento de vaciado y los tamaños de escritura.

### <a name="data-lake-storage-gen1-throttling"></a>Límite de Data Lake Storage Gen1
Si se alcanzan los límites de ancho de banda proporcionados por Data Lake Storage Gen1, podría experimentar errores en las tareas. Compruebe los registros de la tarea para ver los errores de limitación. Puede reducir el paralelismo mediante un aumento del tamaño del contenedor.    

Para comprobar si le están aplicando limitaciones, habilite el registro de depuración en el lado cliente:

1. En **Ambari** > **Storm** > **Config** > **Advanced storm-worker-log4j**, cambie **&lt;nivel de raíz="info"&gt;** por **&lt;nivel de raíz=”debug”&gt;** . Reinicie todos los nodos/servicios para que la configuración surta efecto.
2. Supervise los registros de topología de Storm en los nodos de trabajo (en /var/log/storm/worker-artifacts/&lt;NombreTopología&gt;/&lt;puerto&gt;/worker.log) para controlar las excepciones de limitación de Data Lake Storage Gen1.

## <a name="next-steps"></a>Pasos siguientes
Puede consultar este [blog](/archive/blogs/shanyu/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs) para saber más sobre la optimización del rendimiento adicional de Storm.

Para ejecutar un ejemplo adicional, consulte [este en GitHub](https://github.com/hdinsight/storm-performance-automation).