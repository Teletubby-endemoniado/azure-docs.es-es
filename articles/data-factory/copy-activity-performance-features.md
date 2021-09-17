---
title: Características de optimización del rendimiento de la actividad de copia
titleSuffix: Azure Data Factory & Azure Synapse
description: Conozca las características clave que le ayudarán a optimizar el rendimiento de la actividad de copia en canalizaciones de Azure Data Factory y Azure Synapse Analytics.
ms.author: jianleishen
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 08/24/2021
ms.openlocfilehash: 9be8ef1772da6259441a8de4c85fa44d54945c7d
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122821820"
---
# <a name="copy-activity-performance-optimization-features"></a>Características de optimización del rendimiento de la actividad de copia

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describen las características de optimización del rendimiento de la actividad de copia que puede usar en canalizaciones de Azure Data Factory y Azure Synapse Analytics.

## <a name="data-integration-units"></a>Unidades de integración de datos

Una unidad de integración de datos es una medida que representa la potencia (una combinación de CPU, memoria y asignación de recursos de red) de una única unidad en el servicio. Una unidad de integración de datos solo se aplica a [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime), pero no a [Integration Runtime autohospedado](concepts-integration-runtime.md#self-hosted-integration-runtime).

El número permitido de DIU para impulsar la ejecución de una actividad de copia se encuentra **entre 2 y 256**. Si no se especifica o elige "automático" en la interfaz de usuario, el servicio aplicará dinámicamente el valor óptimo de DIU según el par origen-receptor y el patrón de datos. En la tabla siguiente se enumeran los intervalos de unidades de integración de datos (DIU) admitidos y el comportamiento predeterminado en diferentes escenarios de copia:

| Escenario de copia | Intervalo de DIU admitido | DIU predeterminadas que determina el servicio |
|:--- |:--- |---- |
| Entre almacenes de archivos |- **Copia desde o hacia un solo archivo**: entre 2 y 4 <br>- **Copia desde y hacia varios archivos**: entre 2 y 256, según el número y tamaño de los archivos <br><br>Por ejemplo, si copia datos de una carpeta con 4 archivos grandes y decide conservar la jerarquía, el número máximo efectivo de DIU es 16; si decide combinar archivos, el número máximo efectivo de DIU es 4. |Entre 4 y 32, según el número y tamaño de los archivos |
| Del almacén de archivos a un almacén que no es de archivos |- **Copia desde un solo archivo**: entre 2 y 4 <br/>- **Copia desde varios archivos**: entre 2 y 256, según el número y tamaño de los archivos <br/><br/>Por ejemplo, si copia datos de una carpeta con 4 archivos grandes, el número máximo efectivo de DIU es 16. |- **Copia en Azure SQL Database o Azure Cosmos DB**: entre 4 y 16, según el nivel de receptor (DTU o RU) y el patrón del archivo de origen<br>- **Copia en Azure Synapse Analytics** mediante PolyBase o la instrucción COPY: 2<br>- Otro escenario: 4 |
| De un almacén que no es de archivos al almacén de archivos |- **Copia desde almacenes de datos habilitados con la opción de partición** (incluidos [Azure SQL Database](connector-azure-sql-database.md#azure-sql-database-as-the-source), [Azure SQL Managed Instance](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source) y [Teradata](connector-teradata.md#teradata-as-source)): entre 2 y 256 cuando se escribe en una carpeta y entre 2 y 4 cuando se escribe en un solo archivo. Tenga en cuenta que se puede usar un máximo de 4 DIU por partición de datos de origen.<br>- **Otros escenarios**: entre 2 y 4 |- **Copia desde REST o HTTP**: 1<br/>- **Copia desde Amazon Redshift** mediante UNLOAD: 2<br>- **Otro escenario**: 4 |
| Entre almacenes que no son de archivos |- **Copia desde almacenes de datos habilitados con la opción de partición** (incluidos [Azure SQL Database](connector-azure-sql-database.md#azure-sql-database-as-the-source), [Azure SQL Managed Instance](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source) y [Teradata](connector-teradata.md#teradata-as-source)): entre 2 y 256 cuando se escribe en una carpeta y entre 2 y 4 cuando se escribe en un solo archivo. Tenga en cuenta que se puede usar un máximo de 4 DIU por partición de datos de origen.<br/>- **Otros escenarios**: entre 2 y 4 |- **Copia desde REST o HTTP**: 1<br>- **Otro escenario**: 4 |

Puede ver el número de unidades de integración de datos utilizadas en cada ejecución de copia en la vista de supervisión o en la salida de la actividad de copia. Para más información, consulte el artículo [Supervisión de la actividad de copia](copy-activity-monitoring.md). Para reemplazar esta configuración predeterminada, especifique un valor en la propiedad `dataIntegrationUnits` de la manera siguiente. El *número real de DIU* que usa la operación de copia en tiempo de ejecución es igual o inferior al valor configurado, según el patrón de datos.

Se le cobrará **el n.° de DIU usadas\* la duración de la copia \* el precio unitario/hora de DIU**. Consulte [aquí](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) los precios actuales. La moneda local y el descuento independiente pueden aplicarse según el tipo de suscripción.

**Ejemplo**:

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "dataIntegrationUnits": 128
        }
    }
]
```

## <a name="self-hosted-integration-runtime-scalability"></a>Escalabilidad del entorno de ejecución de integración autohospedado

Si quiere lograr una mayor capacidad de proceso, puede escalar vertical u horizontalmente el IR autohospedado:

- Si la memoria disponible y la CPU del nodo de IR autohospedado no se utilizan totalmente, pero la ejecución de trabajos simultáneos está alcanzando el límite, debe realizar un escalado vertical aumentando el número de trabajos simultáneos que se pueden ejecutar en un nodo.  Consulte [este enlace](create-self-hosted-integration-runtime.md#scale-up) para obtener instrucciones.
- Si, por otro lado, la CPU tiene un uso elevado en el nodo de IR autohospedado o la memoria disponible es baja, puede agregar un nuevo nodo para ayudar a escalar horizontalmente la carga en varios nodos.  Consulte [este enlace](create-self-hosted-integration-runtime.md#high-availability-and-scalability) para obtener instrucciones.

Tenga en cuenta que, en los escenarios siguientes, una sola ejecución de la actividad de copia puede aprovechar varios nodos de IR autohospedados:

- Copia de datos desde almacenes basados en archivos, en función del número y el tamaño de los archivos.
- Copia de datos desde almacenes de datos habilitados con la opción de partición (incluidos [Azure SQL Database](connector-azure-sql-database.md#azure-sql-database-as-the-source), [Azure SQL Managed Instance](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source) y [Teradata](connector-teradata.md#teradata-as-source)), en función del número de particiones de datos.

## <a name="parallel-copy"></a>Copia en paralelo

Puede establecer la copia en paralelo (propiedad `parallelCopies`) en la actividad de copia, para indicar el paralelismo que desea que utilice la actividad de copia. Esta propiedad se puede considerar como el número máximo de subprocesos dentro de la actividad de copia que se leen de los almacenes de datos de origen o se escriben en los almacenes de datos receptores en paralelo.

La copia en paralelo es ortogonal para [unidades de integración de datos](#data-integration-units) o [nodos de IR autohospedado](#self-hosted-integration-runtime-scalability); es decir, se cuenta para todas las unidades de integración de datos o todos los nodos de IR autohospedado.

En cada ejecución de actividad de copia y de forma predeterminada, el servicio aplica dinámicamente el valor óptimo de copia en paralelo según el par origen-receptor y el patrón de datos. 

> [!TIP]
> El comportamiento predeterminado de la copia en paralelo suele proporcionar el mejor rendimiento, que el servicio determina automáticamente en función del par origen-receptor, el patrón de datos y el número de unidades de integración de datos o el recuento de CPU/memoria/nodos del entorno de ejecución de integración autohospedado. Consulte [Solución de problemas de rendimiento de la actividad de copia](copy-activity-performance-troubleshooting.md) para conocer cuándo se debe ajustar la copia en paralelo.

En la tabla siguiente se muestra el comportamiento de la copia en paralelo:

| Escenario de copia | Comportamiento de la copia en paralelo |
| --- | --- |
| Entre almacenes de archivos | `parallelCopies` determina el paralelismo **en el nivel de archivo**. La fragmentación dentro de cada archivo se produce por debajo, de forma automática y transparente. Está diseñada con el objetivo de utilizar el tamaño de fragmento más adecuado para que un tipo de almacén de datos determinado cargue datos en paralelo. <br/><br/>El número real de copias en paralelo que usa la actividad de copia en tiempo de ejecución no es superior al número de archivos que se tiene. Si el comportamiento de copia tiene el valor **mergeFile** para combinar el archivo en un receptor, la actividad de copia no puede aprovechar las ventajas del paralelismo en el nivel de archivo. |
| Del almacén de archivos a un almacén que no es de archivos | - Cuando se copian datos en Azure SQL Database o Azure Cosmos DB, la copia en paralelo predeterminada también depende del nivel de receptor (número de DTU/RU).<br>- Cuando se copian datos en Azure Table, la copia en paralelo predeterminada es 4. |
| De un almacén que no es de archivos al almacén de archivos | - Cuando se copian datos desde almacenes de datos habilitados con la opción de partición (incluidos [Azure SQL Database](connector-azure-sql-database.md#azure-sql-database-as-the-source), [Azure SQL Managed Instance](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source) y [Teradata](connector-teradata.md#teradata-as-source)), la copia en paralelo predeterminada es 4. El número real de copias en paralelo que usa la actividad de copia en tiempo de ejecución no es superior al número de particiones de datos que se tiene. Si usa el entorno de ejecución de integración autohospedado y copia en Azure Blob/ADLS Gen2, tenga en cuenta que la copia en paralelo máxima efectiva es 4 o 5 por nodo de IR.<br>- En otros escenarios, la copia en paralelo no surte efecto. Aunque se especifique el paralelismo, no se aplica. |
| Entre almacenes que no son de archivos | - Cuando se copian datos en Azure SQL Database o Azure Cosmos DB, la copia en paralelo predeterminada también depende del nivel de receptor (número de DTU/RU).<br/>- Cuando se copian datos desde almacenes de datos habilitados con la opción de partición (incluidos [Azure SQL Database](connector-azure-sql-database.md#azure-sql-database-as-the-source), [Azure SQL Managed Instance](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [SAP Open Hub](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source), [SAP Table](connector-sap-table.md#sap-table-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source) y [Teradata](connector-teradata.md#teradata-as-source)), la copia en paralelo predeterminada es 4.<br>- Cuando se copian datos en Azure Table, la copia en paralelo predeterminada es 4. |

Para controlar la carga en las máquinas que hospedan los almacenes de datos o para ajustar el rendimiento de la copia, puede reemplazar el valor predeterminado y especificar un valor para la propiedad `parallelCopies`. El valor debe ser un entero mayor o igual que 1. En tiempo de ejecución, y para obtener el mejor rendimiento, la actividad de copia usa un valor inferior o igual al valor que ha establecido.

Cuando especifique un valor para la propiedad `parallelCopies`, tenga en cuenta el aumento de la carga en los almacenes de datos de origen y receptor. Tenga también en cuenta el aumento de carga en el entorno de ejecución de integración autohospedado si la actividad de copia está capacitada para él. Dicho aumento sucede especialmente si tiene varias actividades o ejecuciones simultáneas de las mismas actividades que se ejecutan en el mismo almacén de datos. Si observa que el almacén de datos o el entorno de ejecución de integración autohospedado están sobrecargados, disminuya el valor de `parallelCopies` para aliviar la carga.

**Ejemplo**:

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 32
        }
    }
]
```

## <a name="staged-copy"></a>copia almacenada provisionalmente

Al copiar datos de un almacén de datos de origen a un almacén de datos receptor, podría elegir usar Azure Blob Storage o Azure Data Lake Storage Gen2 como almacenamiento provisional. El almacenamiento provisional es especialmente útil en los siguientes casos:

- **Quiere ingerir datos de varios almacenes de datos en Azure Synapse Analytics a través de PolyBase, copiar datos con Snowflake como origen y destino o introducir datos desde Amazon Redshift/HDFS de manera eficaz.** Obtenga más información sobre lo siguiente:
  - [Uso de PolyBase para cargar datos en Azure Synapse Analytics](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-synapse-analytics)
  - [Conector de Snowflake](connector-snowflake.md)
  - [Conector de Amazon Redshift](connector-amazon-redshift.md)
  - [Conector de HDFS](connector-hdfs.md)
- **Solo desea abrir los puertos 80 y el 443 en el firewall, a causa de las directivas de TI corporativas**. Por ejemplo, al copiar datos de un almacén de datos local a Azure SQL Database o a una instancia de Azure Synapse Analytics, debe activar la comunicación TCP saliente en el puerto 1433 tanto para el firewall de Windows como para el firewall corporativo. En este escenario, la copia almacenada provisionalmente usa el entorno de ejecución de integración autohospedado para copiar primero los datos en un almacenamiento provisional a través de HTTP o HTTPS en el puerto 443 y, luego, cargar los datos desde el almacenamiento provisional en SQL Database o Azure Synapse Analytics. En este flujo, no es necesario habilitar el puerto 1433.
- **En ocasiones, se tarda un tiempo en realizar un movimiento de datos híbridos (es decir, copiar desde un almacén de datos local a un almacén de datos en la nube) a través de una conexión de red lenta.** Para mejorar el rendimiento se puede usar una copia almacenada provisionalmente para comprimir los datos de forma local, con el fin de que se tarde menos tiempo en mover datos al almacén de datos provisional en la nube. Luego, puede descomprimir los datos en dicho almacén antes de cargarlos en el almacén de datos de destino.

### <a name="how-staged-copy-works"></a>Funcionamiento de las copias almacenadas provisionalmente

Al activar la característica de almacenamiento provisional, primero se copian los datos desde el almacén de datos de origen al almacenamiento provisional (aporte su propio Azure Blob o Azure Data Lake Storage Gen2). A continuación, los datos se copian desde el almacén de datos provisional al almacén de datos receptor. La actividad de copia administra automáticamente el flujo de dos fases y también borra los datos temporales del almacenamiento provisional una vez completado el movimiento de datos.

![copia almacenada provisionalmente](media/copy-activity-performance/staged-copy.png)

Cuando activa el movimiento de datos mediante un almacén provisional, puede especificar si quiere que los datos se compriman antes de moverlos del almacén de datos de origen al almacenamiento provisional y luego se descompriman antes de moverlos desde este último al almacén de datos receptor.

Actualmente, no es posible copiar datos entre dos almacenes de datos que estén conectados a través de distintos IR autohospedados, ni con copia almacenada provisionalmente ni sin ella. Para dicho escenario, puede configurar dos actividades de copia explícitamente encadenadas, con el fin de copiar desde el origen al almacenamiento provisional y, después, desde este al receptor.

### <a name="configuration"></a>Configuración

Configure el valor **enableStaging** de la actividad de copia para especificar si desea que los datos se almacenen provisionalmente antes de cargarlos en un almacén de datos de destino. Cuando establezca **enableStaging** en `TRUE`, especifique las propiedades adicionales que se muestran en la tabla siguiente. 

| Propiedad | Descripción | Valor predeterminado | Obligatorio |
| --- | --- | --- | --- |
| enableStaging |Especifique si desea copiar los datos a través de un almacén provisional. |False |No |
| linkedServiceName |Especifique el nombre de un servicio vinculado [Azure Blob Storage](connector-azure-blob-storage.md#linked-service-properties) o [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#linked-service-properties) que haga referencia a la instancia de almacenamiento que se usa como almacenamiento provisional. |N/D |Sí, cuando el valor de **enableStaging** está establecido en True. |
| path |Especifique la ruta de acceso que quiere que contenga los datos almacenados provisionalmente. Si no se proporciona una ruta de acceso, el servicio crea un contenedor para almacenar los datos temporales. |N/D |No |
| enableCompression |Especifica si se deben comprimir los datos antes de copiarlos en el destino. Esta configuración reduce el volumen de datos que se va a transferir. |False |No |

>[!NOTE]
> Si usa una copia almacenada provisionalmente con la compresión habilitada, no se admite la autenticación de MSI o de la entidad de servicio para el almacenamiento provisional de un servicio vinculado de blob.

Este es un ejemplo de definición de una actividad de copia con las propiedades que se han descrito en la tabla anterior:

```json
"activities":[
    {
        "name": "CopyActivityWithStaging",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "OracleSource",
            },
            "sink": {
                "type": "SqlDWSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingStorage",
                    "type": "LinkedServiceReference"
                },
                "path": "stagingcontainer/path"
            }
        }
    }
]
```

### <a name="staged-copy-billing-impact"></a>Impacto en la facturación de copia almacenada provisionalmente

Los cargos que se le realizan se basan en dos pasos: duración de la copia y tipo de copia.

* Cuando use el almacenamiento provisional durante una copia en la nube, que copia de datos de un almacén de datos en la nube a otro, donde ambos almacenamientos provisionales usan Azure Integration Runtime, se le cobrará de la siguiente manera: [suma de la duración de la copia de los pasos 1 y 2] x [precio unitario de la copia en la nube].
* Cuando use el almacenamiento provisional durante una copia híbrida, que copia de datos de un almacén de datos local a uno en la nube, un almacenamiento provisional que usa Integration Runtime autohospedado, se le cobrará de la siguiente manera: [duración de la copia híbrida] x [precio unitario de la copia híbrida] + [duración de la copia de nube] x [precio unitario de la copia de nube].

## <a name="next-steps"></a>Pasos siguientes
Consulte los restantes artículos acerca de la actividad de copia:

- [Información general de la actividad de copia](copy-activity-overview.md)
- [Guía de escalabilidad y rendimiento de la actividad de copia](copy-activity-performance.md)
- [Solución de problemas de rendimiento de la actividad de copia](copy-activity-performance-troubleshooting.md)
- [Uso de Azure Data Factory para migrar datos del lago de datos y el almacenamiento de datos a Azure](data-migration-guidance-overview.md)
- [Migración de datos de AWS S3 a Azure Storage](data-migration-guidance-s3-azure-storage.md)