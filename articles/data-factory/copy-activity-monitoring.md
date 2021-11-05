---
title: Supervisión de la actividad de copia
titleSuffix: Azure Data Factory & Azure Synapse
description: Conozca cómo supervisar la ejecución de la actividad de copia en Azure Data Factory y Azure Synapse Analytics.
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.author: jianleishen
ms.openlocfilehash: 14607231fd68cf78f33a21abdb26dd68e2ef6871
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131062195"
---
# <a name="monitor-copy-activity"></a>Supervisión de actividad de copia

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe cómo supervisar la ejecución de la actividad de copia en las canalizaciones de Azure Data Factory y Synapse. El documento se basa en el artículo de [introducción a la actividad de copia](copy-activity-overview.md) que describe información general de la actividad de copia.  También puede supervisar las actividades de copia generadas con la [Herramienta de copia de datos](copy-data-tool.md), así como [eliminar actividades](delete-activity.md) de la misma forma.

## <a name="monitor-visually"></a>Supervisión visual

Una vez que haya creado y publicado una canalización, puede asociarla a un desencadenador o iniciar manualmente una ejecución ad hoc. Puede supervisar todas las ejecuciones de la canalización de forma nativa en la experiencia de usuario. Obtenga información general sobre la supervisión en [Supervisión visual de canalizaciones de Azure Data Factory y Synapse](monitor-visually.md).

Para supervisar la ejecución de la actividad de copia, vaya a la interfaz de usuario de **Data Factory Studio** o **Azure Synapse Studio** para su instancia de servicio. En la pestaña **Monitor** (Supervisión), verá una lista de ejecuciones de canalización; haga clic en el vínculo del **nombre de canalización** para acceder a la lista de ejecuciones de actividad en la ejecución de canalización.

# <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

:::image type="content" source="./media/copy-activity-overview/monitor-pipeline-run.png" alt-text="Supervisar ejecución de canalización":::

# <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

:::image type="content" source="./media/copy-activity-overview/monitor-pipeline-run-synapse.png" alt-text="Supervisar ejecución de canalización":::

---

En este nivel, puede ver vínculos a la entrada, la salida y los errores (si se produce un error en la ejecución de la actividad de copia) de la actividad de copia, así como estadísticas tales como la duración y el estado. Al hacer clic en el botón de **detalles** (gafas) junto al nombre de la actividad de copia, obtendrá información más detallada sobre la ejecución de la actividad de copia. 

:::image type="content" source="./media/copy-activity-overview/monitor-copy-activity-run.png" alt-text="Supervisión de la ejecución de la actividad de copia":::

En esta vista gráfica de supervisión, el servicio presenta la información sobre la ejecución de la actividad de copia, entre la que se incluye el volumen de datos leídos y escritos, el número de archivos y filas de datos copiados del origen al receptor, el rendimiento, las configuraciones aplicadas para el escenario de copia, los pasos de la actividad de copia con las duraciones y los detalles correspondientes, etc. Consulte en [esta tabla](#monitor-programmatically) cada métrica posible y su descripción detallada. 

En algunos casos, cuando se ejecuta una actividad de copia, se ve el mensaje **"Performance tuning tips"** (Sugerencias para la optimización del rendimiento) en la parte superior de la vista de supervisión de la actividad de copia, como se muestra en el ejemplo siguiente. Las sugerencias indican el cuello de botella identificado por el servicio para la ejecución específica de la copia, junto con recomendaciones sobre aquellos elementos que puede cambiar para aumentar el rendimiento de la copia. Obtenga más información sobre [sugerencias de optimización automática del rendimiento](copy-activity-performance-troubleshooting.md#performance-tuning-tips).

En la parte inferior, los **detalles y duraciones de la ejecución** describen los pasos clave por los que pasa la actividad de copia, algo que resulta especialmente útil para solucionar problemas de rendimiento de la copia. El cuello de botella de la ejecución de copia corresponde a la que tiene mayor duración. Consulte en [Solución de problemas de rendimiento de la actividad de copia](copy-activity-performance-troubleshooting.md) qué representa cada fase y la guía detallada de solución de problemas.

**Ejemplo: Copia de Amazon S3 a Azure Data Lake Storage Gen2**

:::image type="content" source="./media/copy-activity-overview/monitor-copy-activity-run-details.png" alt-text="Detalles de la supervisión de la ejecución de la actividad de copia":::

## <a name="monitor-programmatically"></a>Supervisión mediante programación

Los detalles de la ejecución de la actividad de copia y las características de rendimiento también se devuelven en la sección **Copy Activity run result** > **Output** (Resultado de la ejecución de la actividad de copia -> Salida), que se utiliza para representar la vista de supervisión en la interfaz de usuario. A continuación se muestra una lista completa de las propiedades que pueden devolverse. Solo verá las propiedades que se aplican a su escenario de copia. Para información sobre cómo supervisar las ejecuciones de la actividad mediante programación en general, consulte [Supervisión mediante programación de una canalización de Azure Data Factory o Synapse](monitor-programmatically.md).

| Nombre de propiedad  | Descripción | Unidad en la salida |
|:--- |:--- |:--- |
| dataRead | La cantidad real de datos leídos del origen. | Valor Int64 en bytes |
| dataWritten | La cantidad real de datos escritos o confirmados en el receptor. El tamaño puede ser diferente del tamaño de la propiedad `dataRead`, ya que tiene que ver con el modo en que cada almacén de datos almacena los datos. | Valor Int64 en bytes |
| filesRead | El número de archivos leídos del origen basado en archivos. | Valor Int64 (sin unidad) |
| filesWritten | El número de archivos escritos o confirmados en el receptor basado en archivos. | Valor Int64 (sin unidad) |
| filesSkipped | El número de archivos omitidos del origen basado en archivos. | Valor Int64 (sin unidad) |
| dataConsistencyVerification | Detalles de la verificación de coherencia de datos, donde puede ver si se ha verificado que los datos copiados son coherentes entre el almacén de origen y el de destino. Más información en [este artículo](copy-activity-data-consistency.md#monitoring). | Array |
| sourcePeakConnections | Número máximo de conexiones simultáneas establecidas en el almacén de datos de origen durante la ejecución de la actividad de copia. | Valor Int64 (sin unidad) |
| sinkPeakConnections | Número máximo de conexiones simultáneas establecidas en el almacén de datos receptor durante la ejecución de la actividad de copia. | Valor Int64 (sin unidad) |
| rowsRead | Número de filas leídas del origen. Esta métrica no se aplica cuando se copian archivos tal cual sin analizarlos, por ejemplo, cuando los conjuntos de datos de origen y receptor tienen un tipo de formato binario u otro tipo de formato con configuraciones idénticas. | Valor Int64 (sin unidad) |
| rowsCopied | Número de filas copiadas al receptor. Esta métrica no se aplica cuando se copian archivos tal cual sin analizarlos, por ejemplo, cuando los conjuntos de datos de origen y receptor tienen un tipo de formato binario u otro tipo de formato con configuraciones idénticas.  | Valor Int64 (sin unidad) |
| rowsSkipped | Número de filas incompatibles que se han omitido. Puede permitir que se omitan las filas incompatibles; para ello, establezca `enableSkipIncompatibleRow` en true. | Valor Int64 (sin unidad) |
| copyDuration | Duración de la ejecución de copia. | Valor Int32 en segundos |
| throughput | Velocidad de transferencia de datos, calculada por `dataRead` dividido por `copyDuration`. | Número de punto flotante en KBps |
| sourcePeakConnections | Número máximo de conexiones simultáneas establecidas en el almacén de datos de origen durante la ejecución de la actividad de copia. | Valor Int32 (sin unidad) |
| sinkPeakConnections| Número máximo de conexiones simultáneas establecidas en el almacén de datos receptor durante la ejecución de la actividad de copia.| Valor Int32 (sin unidad) |
| sqlDwPolyBase | Si se usa PolyBase cuando se copian datos en Azure Synapse Analytics. | Boolean |
| redshiftUnload | Si se usará UNLOAD cuando se copian datos de Redshift. | Boolean |
| hdfsDistcp | Si se usará DistCp cuando se copian datos de HDFS. | Boolean |
| effectiveIntegrationRuntime | El entorno de ejecución de integración (IR) o los tiempos de ejecución que se usan para aumentar la potencia de la ejecución de la actividad, en el formato `<IR name> (<region if it's Azure IR>)`. | Texto (cadena) |
| usedDataIntegrationUnits | Unidades de integración de datos vigentes durante la copia. | Valor Int32 |
| usedParallelCopies | El número de parallelCopies efectivo durante la copia. | Valor Int32 |
| logPath | Ruta de acceso al registro de sesión de los datos omitidos en el almacenamiento de blobs. Consulte [Tolerancia a errores](copy-activity-overview.md#fault-tolerance). | Texto (cadena) |
| executionDetails | Más información sobre las fases por las que pasa la actividad de copia y los pasos, la duración, las configuraciones usadas, etc. correspondientes. No se recomienda analizar esta sección porque podría cambiar. Para comprender mejor cómo ayuda a entender y solucionar problemas de rendimiento de las copias, consulte la sección [Supervisión visual](#monitor-visually). | Array |
| perfRecommendation | Sugerencias de optimización del rendimiento de la copia. Consulte [Sugerencias de optimización del rendimiento](copy-activity-performance-troubleshooting.md#performance-tuning-tips) para obtener más información. | Array |
| billingReference | El consumo de facturación para la ejecución especificada. Obtenga más información en [Supervisión del consumo en el nivel de ejecución de actividad](plan-manage-costs.md#monitor-consumption-at-activity-run-level). | Object |
| durationInQueue | Duración de la cola en segundos antes de que se inicie la ejecución de la actividad de copia. | Object |

**Ejemplo**:

```json
"output": {
    "dataRead": 1180089300500,
    "dataWritten": 1180089300500,
    "filesRead": 110,
    "filesWritten": 110,
    "filesSkipped": 0,
    "sourcePeakConnections": 640,
    "sinkPeakConnections": 1024,
    "copyDuration": 388,
    "throughput": 2970183,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)",
    "usedDataIntegrationUnits": 128,
    "billingReference": "{\"activityType\":\"DataMovement\",\"billableDuration\":[{\"Managed\":11.733333333333336}]}",
    "usedParallelCopies": 64,
    "dataConsistencyVerification": 
    { 
        "VerificationResult": "Verified", 
        "InconsistentData": "None" 
    },
    "executionDetails": [
        {
            "source": {
                "type": "AmazonS3"
            },
            "sink": {
                "type": "AzureBlobFS",
                "region": "East US",
                "throttlingErrors": 6
            },
            "status": "Succeeded",
            "start": "2020-03-04T02:13:25.1454206Z",
            "duration": 388,
            "usedDataIntegrationUnits": 128,
            "usedParallelCopies": 64,
            "profile": {
                "queue": {
                    "status": "Completed",
                    "duration": 2
                },
                "transfer": {
                    "status": "Completed",
                    "duration": 386,
                    "details": {
                        "listingSource": {
                            "type": "AmazonS3",
                            "workingDuration": 0
                        },
                        "readingFromSource": {
                            "type": "AmazonS3",
                            "workingDuration": 301
                        },
                        "writingToSink": {
                            "type": "AzureBlobFS",
                            "workingDuration": 335
                        }
                    }
                }
            },
            "detailedDurations": {
                "queuingDuration": 2,
                "transferDuration": 386
            }
        }
    ],
    "perfRecommendation": [
        {
            "Tip": "6 write operations were throttled by the sink data store. To achieve better performance, you are suggested to check and increase the allowed request rate for Azure Data Lake Storage Gen2, or reduce the number of concurrent copy runs and other data access, or reduce the DIU or parallel copy.",
            "ReferUrl": "https://go.microsoft.com/fwlink/?linkid=2102534 ",
            "RuleName": "ReduceThrottlingErrorPerfRecommendationRule"
        }
    ],
    "durationInQueue": {
        "integrationRuntimeQueue": 0
    }
}
```

## <a name="next-steps"></a>Pasos siguientes
Consulte los otros artículos de la actividad de copia:

\- [Introducción a la actividad de copia](copy-activity-overview.md)

\- [Rendimiento de la actividad de copia](copy-activity-performance.md)
