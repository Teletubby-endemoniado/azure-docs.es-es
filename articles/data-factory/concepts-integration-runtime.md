---
title: Tiempo de ejecución de integración
titleSuffix: Azure Data Factory & Azure Synapse
description: Obtenga información sobre el entorno de ejecución de integración en Azure Data Factory y Azure Synapse Analytics.
ms.author: lle
author: lrtoyou1223
ms.service: data-factory
ms.subservice: integration-runtime
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 6081e63a9a2fbc4598bac0d368d43c31121fa448
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124787933"
---
# <a name="integration-runtime-in-azure-data-factory"></a>Integration Runtime en Azure Data Factory 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Integration Runtime (IR) es la infraestructura de proceso que usan las canalizaciones de Azure Data Factory y Azure Synapse para proporcionar las siguientes funcionalidades de integración de datos en distintos entornos de red:

- **Data Flow**: ejecute una instancia de [Data Flow](concepts-data-flow-overview.md) en el entorno de proceso de Azure administrado.  
- **Movimiento de datos**: Copie los datos entre almacenes de datos en redes públicas y almacenes de datos en redes privadas (red privada local o virtual). Proporciona compatibilidad para conectores integrados, conversión de formato, asignación de columnas y transferencia de datos escalable y de rendimiento superior.
- **Distribución de actividades**: distribuya y supervise actividades de transformación que se ejecuten en diferentes servicios de proceso, como Azure Databricks, Azure HDInsight, ML Studio (clásico), Azure SQL Database, SQL Server, etc.
- **Ejecución de paquetes SSIS**: ejecute de forma nativa paquetes de SQL Server Integration Services (SSIS) en un entorno de proceso de Azure administrado.

En las canalizaciones de Data Factory y Synapse, una actividad define la acción que se realizará. Un servicio vinculado define un almacén de datos o un servicio de proceso de destino. Una instancia de Integration Runtime proporciona el puente entre la actividad y los servicios vinculados.  La actividad o el servicio vinculado hace referencia a él, y proporciona el entorno de proceso donde se ejecuta la actividad o desde donde se distribuye. De esta manera, la actividad puede realizarse en la región más cercana posible al almacén de datos o servicio de proceso de destino de la manera con mayor rendimiento, a la vez que se satisfacen las necesidades de seguridad y cumplimiento.

Los entornos de ejecución de integración se pueden crear en las interfaces de usuario de Azure Data Factory y Azure Synapse por medio del [centro de administración](author-management-hub.md) y de cualquier actividad, conjunto de datos o flujo de datos que haga referencia a ellos.

## <a name="integration-runtime-types"></a>Tipos de instancias de Integration Runtime

Data Factory ofrece tres tipos de instancias de Integration Runtime (IR), y debe elegir el tipo que atienda mejor las funcionalidades de integración de datos y las necesidades del entorno de red que está buscando.  Estos tres tipos son:

- Azure
- Autohospedado
- SSIS de Azure

> [!NOTE]
> Actualmente, las canalizaciones de Synapse solo admiten entornos de ejecución de integración de Azure o autohospedados.

En la tabla siguiente se describen las funcionalidades y la compatibilidad de red para cada uno de los tipos de instancias de Integration Runtime:

Tipo de IR | Red pública | Red privada 
------- | -------------- | ---------------
Azure | Data Flow<br/>Movimiento de datos<br/>Distribución de actividades | Data Flow<br/>Movimiento de datos<br/>Distribución de actividades
Autohospedado | Movimiento de datos<br/>Distribución de actividades | Movimiento de datos<br/>Distribución de actividades
SSIS de Azure | Ejecución de paquetes SSIS | Ejecución de paquetes SSIS


## <a name="azure-integration-runtime"></a>Tiempo de ejecución de integración de Azure

Una instancia de Azure Integration Runtime puede:

- Ejecución de flujos de datos en Azure 
- Ejecución de la actividad de copia entre almacenes de datos en la nube
- Distribuir las siguientes actividades de transformación en la red pública: actividad Notebook, Jar o Python de Databricks, actividad Hive de HDInsight, actividad Pig de HDInsight, actividad MapReduce de HDInsight, actividad Spark de HDInsight, actividad Streaming de HDInsight, actividad de ejecución de lotes de ML Studio (clásico), actividad de recurso de actualización de ML Studio (clásico), actividad de procedimiento almacenado, actividad de U-SQL de Data Lake Analytics, actividad personalizada de .NET, actividad web, actividad de búsqueda y actividad de obtención de metadatos.

### <a name="azure-ir-network-environment"></a>Entorno de red de Azure Integration Runtime

Azure Integration Runtime admite la conexión con almacenes de datos y servicios de proceso con puntos de conexión de acceso público. Al habilitar Virtual Network administrado y Azure Integration Runtime se admite la conexión a almacenes de datos mediante el servicio de Private Link en el entorno de red privada.

### <a name="azure-ir-compute-resource-and-scaling"></a>Recursos de proceso y escalado de Azure Integration Runtime
Integration Runtime de Azure proporciona un proceso completamente administrado y sin servidor en Azure.  No tiene que preocuparse del aprovisionamiento de la infraestructura, la instalación de software, la aplicación de revisiones ni el escalado de la capacidad.  Además, solo se paga por la duración de la utilización real.

Integration Runtime de Azure proporciona el proceso nativo para mover datos entre almacenes de datos en la nube de forma segura, confiable y con alto rendimiento.  Puede establecer cuántas unidades de integración de datos se usarán en la actividad de copia, y el tamaño de proceso de Integration Runtime de Azure se escala verticalmente de manera elástica en consecuencia sin tener que ajustar de manera explícita el tamaño de Integration Runtime de Azure. 

La distribución de actividades es una operación ligera para enrutar la actividad al servicio de proceso de destino, por lo que no hay necesidad de escalar verticalmente el tamaño de proceso para este escenario.

Para obtener información sobre cómo crear y configurar una instancia de Azure IR, consulte [Cómo crear y configurar una instancia de Azure Integration Runtime](create-azure-integration-runtime.md). 

> [!NOTE] 
> Azure Integration Runtime tiene propiedades relacionadas con el flujo de datos en el entorno de ejecución de Data Flow, que define la infraestructura de proceso subyacente que se usaría para ejecutar los flujos de datos. 

## <a name="self-hosted-integration-runtime"></a>Integration Runtime autohospedado

Una instancia de Integration Runtime autohospedado es capaz de:

- Ejecutar la actividad de copia entre los almacenes de datos en la nube y un almacén de datos en la red privada.
- Distribuir las siguientes actividades de transformación a los recursos de proceso del entorno local o Azure Virtual Network: actividad Hive de HDInsight (BYOC, traiga su propio clúster), actividad Pig de HDInsight (BYOC), actividad MapReduce de HDInsight (BYOC), actividad Spark de HDInsight (BYOC), actividad Streaming de HDInsight (BYOC), actividad de ejecución de lotes de ML Studio (clásico), actividades de recurso de actualización de ML Studio (clásico), actividad de procedimiento almacenado, actividad de U-SQL de Data Lake Analytics, actividad personalizada (se ejecuta en Azure Batch), actividad de búsqueda y actividad de obtención de metadatos.

> [!NOTE] 
> Use Integration Runtime autohospedado para admitir almacenes de datos que requieran bring-your-own driver (BYOD), como SAP Hana, MySQL, etc.  Para obtener más información, consulte los [almacenes de datos compatibles](copy-activity-overview.md#supported-data-stores-and-formats).

> [!NOTE] 
> Java Runtime Environment (JRE) es una dependencia de IR autohospedado. Asegúrese de que tiene instalado JRE en el mismo host.

### <a name="self-hosted-ir-network-environment"></a>Entorno de red de IR autohospedado

Si desea realizar la integración de datos de manera segura en un entorno de red privada, que no tiene una línea de vista directa desde el entorno de nube pública, puede instalar IR autohospedado en el entorno local detrás del firewall corporativo, o dentro de un red privada virtual.  Integration Runtime autohospedado establece conexiones salientes basadas en HTTP para abrir Internet.

### <a name="self-hosted-ir-compute-resource-and-scaling"></a>Recursos de proceso y escalado de IR autohospedado

Instale IR autohospedado en un equipo local o en una máquina virtual dentro de una red privada. Actualmente, solo se admite ejecutar instancias de Integration Runtime autohospedados en un sistema operativo Windows.  

Para una alta disponibilidad y escalabilidad, puede escalar horizontalmente IR autohospedado mediante la asociación de la instancia lógica con varias máquinas locales en modo activo-activo.  Para más información, consulte el artículo sobre cómo [crear y configurar un IR autohospedado](create-self-hosted-integration-runtime.md) en las guías de procedimientos.

## <a name="azure-ssis-integration-runtime"></a>Integration Runtime de SSIS de Azure

> [!NOTE]
> Los entornos de ejecución de integración de SSIS de Azure no se admiten actualmente en las canalizaciones de Synapse.

Para levantar y mover la carga de trabajo de SSIS existente, puede crear una instancia de Integration Runtime de SSIS de Azure para ejecutar paquetes SSIS de forma nativa.  

### <a name="azure-ssis-ir-network-environment"></a>Entorno de red de una instancia de Integration Runtime para la integración de SSIS en Azure

Integration Runtime de SSIS de Azure se puede aprovisionar en la red pública o la red privada.  El acceso a datos locales se admite mediante la combinación de Integration Runtime de SSIS de Azure a una red virtual que está conectada a la red local.  

### <a name="azure-ssis-ir-compute-resource-and-scaling"></a>Recursos de proceso y escalado de una instancia de Integration Runtime para la integración de SSIS en Azure

Integration Runtime de SSIS de Azure es un clúster totalmente administrado de máquinas virtuales de Azure dedicado a ejecutar los paquetes de SSIS. Puede traer su propia instancia de Azure SQL Database o Instancia administrada de SQL para el catálogo de proyectos y paquetes de SSIS (SSISDB). Puede escalar verticalmente la eficacia de los procesos especificando el tamaño de nodo y escalar horizontalmente especificando el número de nodos del clúster. Puede administrar el coste de funcionamiento de Integration Runtime de SSIS de Azure deteniéndolo e iniciándolo según lo considere oportuno.

Para más información, consulte el artículo sobre cómo crear y configurar un IR de SSIS de Azure en las guías de procedimientos.  Una vez creado, puede implementar y administrar los paquetes de SSIS existentes con poco o ningún cambio utilizando herramientas conocidas, como SQL Server Data Tools (SSDT) y SQL Server Management Studio (SSMS), de manera similar a usar SSIS de forma local.

Consulte los siguientes artículos para más información sobre Integration Runtime de SSIS de Azure: 

- [Tutorial: Implementación de paquetes SSIS en Azure](./tutorial-deploy-ssis-packages-azure.md). En este artículo se proporcionan instrucciones paso a paso para crear una instancia de Azure-SSIS IR y se usa una instancia de Azure SQL Database para hospedar el catálogo de SSIS. 
- [Cómo: Creación de una instancia de Integration Runtime de SSIS de Azure](create-azure-ssis-integration-runtime.md). En este artículo se amplía el tutorial y se proporcionan instrucciones sobre el uso de Instancia administrada de SQL y la unión de Integration Runtime a una red virtual. 
- [Monitor an Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime) (Supervisión de una instancia de Integration Runtime de SSIS de Azure). En este artículo se muestra cómo recuperar información sobre una instancia de IR de SSIS de Azure, junto con descripciones de los estados en la información devuelta. 
- [Administración de Integration Runtime de SSIS de Azure](manage-azure-ssis-integration-runtime.md). En este artículo se muestra cómo detener, iniciar o quitar una instancia de IR de SSIS de Azure. También se muestra cómo escalar horizontalmente IR de SSIS de Azure mediante la adición de más nodos a IR. 
- [Unión de una instancia de Integration Runtime para la integración de SSIS en Azure a una red virtual](join-azure-ssis-integration-runtime-virtual-network.md). En este artículo se proporciona información conceptual sobre cómo unir una instancia de Integration Runtime de SSIS de Azure a una red virtual de Azure. También se proporcionan los pasos para configurar la red virtual mediante Azure Portal para que se una la instancia de Integration Runtime de SSIS de Azure. 

## <a name="integration-runtime-location"></a>Ubicación de Integration Runtime

### <a name="relationship-between-factory-location-and-ir-location"></a>Relación entre la ubicación de la factoría y la del entorno de ejecución de integración

Cuando un cliente crea una instancia de Data Factory, debe especificar la ubicación del área de trabajo de Data Factory o de Synapse. En ella se guardan los metadatos del área de trabajo y desde ella se desencadena la canalización. Los metadatos solo se almacenan en la región que elija el cliente, no en otras regiones.

Sin embargo, una canalización de Azure Data Factory o de Azure Synapse puede acceder a almacenes de datos y a servicios de proceso de otras regiones de Azure para mover datos entre los almacenes de datos o para procesar datos mediante servicios de proceso. Este comportamiento se lleva a cabo a través de la instancia de [Integration Runtime disponible globalmente](https://azure.microsoft.com/global-infrastructure/services/) para garantizar el cumplimiento de los datos, la eficacia y los menores costes de salida de la red.

La ubicación de Integration Runtime define la ubicación de su proceso de back-end y, en esencia, la ubicación donde se realizan el movimiento de datos, la distribución de actividades y la ejecución de paquetes de SSIS. La ubicación de la instancia de Integration Runtime puede ser diferente de la ubicación de la factoría de datos a la que pertenece. 

### <a name="azure-ir-location"></a>Ubicación de Azure Integration Runtime

Puede establecer una ubicación determinada para una instancia de Azure Integration Runtime, en cuyo caso la ejecución o la distribución de la actividad se producirá en esa región específica.

Si opta por usar la resolución automática de Azure IR en redes públicas, que es la opción predeterminada,

- En la actividad de copia, se intentará detectar automáticamente la ubicación del almacén de datos del receptor y luego se usará IR en la misma región si está disponible, o en la más cercana de la misma ubicación geográfica; si la región del almacén de datos del receptor no se puede detectar, se usa la instancia de IR de la región de la factoría de datos como alternativa.

  Por ejemplo, supongamos que el área de trabajo de Data Factory o de Synapse se creó en Este de EE. UU.: 
  
  - Al copiar datos en un blob de Azure en la región Oeste de EE. UU., si se detecta que el blob está en esa región, la actividad de copia se ejecuta en el entorno de ejecución de integración de la misma región. Si se produce un error en la detección de la región, la actividad de copia se ejecuta en el entorno de ejecución de integración de la región Este de EE. UU.
  - Cuando copie datos en Salesforce cuya región no se puede detectar, la actividad de copia se ejecuta en IR en la región Este de EE. UU.

  >[!TIP] 
  >Si tiene requisitos estrictos de cumplimiento de datos y debe garantizar que los datos no salen de una determinada región geográfica, puede crear explícitamente una instancia de Azure IR en una determinada región y dirigir el servicio vinculado a esta instancia de IR con la propiedad ConnectVia. Por ejemplo, si quiere copiar datos de un blob de la región Sur de Reino Unido en Azure Synapse Analytics de esa misma región y quiere garantizar que los datos no salen del Reino Unido, cree una instancia de Azure IR en Reino Unido y vincule ambos servicios vinculados a ella.

- Para la ejecución de las actividades de búsqueda, obtención de metadatos o eliminación (también conocidas como actividades de canalización), la distribución de actividades de transformación (también conocidas como actividades externas) y las operaciones de creación (prueba de conexión, examen de lista de carpetas y lista de tablas, vista previa de datos), se usa el entorno de ejecución de integración de la misma región que el área de trabajo de Data Factory o Synapse.

- En el caso de Data Flow, se usa el entorno de ejecución de integración de la región del área de trabajo de Data Factory o de Synapse. 

  > [!TIP] 
  > Una buena práctica sería asegurarse de que Data Flow se ejecuta en la misma región que los almacenes de datos correspondiente (si es posible). Puede conseguirlo mediante la resolución automática de Azure IR (si la ubicación del almacén de datos coincide con la ubicación del área de trabajo de Data Factory o Synapse), o mediante la creación de una nueva instancia de Azure IR en la misma región que los almacenes de datos y la posterior ejecución del flujo de datos en ella. 

Si habilita una red virtual administrada para la resolución automática de Azure IR, se usa el entorno de ejecución de integración de la región del área de trabajo de Data Factory o de Synapse. 

Puede supervisar qué ubicación de IR se usa durante la ejecución de la actividad en la vista de supervisión de actividades de la canalización en la interfaz de usuario, o en la carga de supervisión de actividades.

### <a name="self-hosted-ir-location"></a>Ubicación de IR autohospedado

El IR autohospedado se registra lógicamente en el área de trabajo de Data Factory o de Synapse, y el usuario proporciona el proceso que se usa para admitir sus funcionalidades. Por lo tanto, no hay ninguna propiedad de ubicación explícita para la instancia de Integration Runtime autohospedado. 

Cuando se utiliza para realizar el movimiento de datos, IR autohospedado extrae datos del origen y los escribe en el destino.

### <a name="azure-ssis-ir-location"></a>Ubicación de la instancia de Integration Runtime para la integración de SSIS en Azure

> [!NOTE]
> Los entornos de ejecución de integración de SSIS de Azure no se admiten actualmente en las canalizaciones de Synapse.

Al seleccionar la ubicación adecuada para Integration Runtime de SSIS de Azure, es esencial lograr un alto rendimiento en los flujos de trabajo de extracción, transformación y carga (ETL).

- La ubicación de Azure-SSIS IR no tiene que ser igual que la de instancia de Data Factory, pero debe ser la misma que la ubicación de su propia instancia de Azure SQL Database o de SQL Managed Instance donde se hospedará SSISDB. De esta manera, Integration Runtime de SSIS de Azure puede acceder fácilmente a SSISDB sin incurrir en un tráfico excesivo entre las distintas ubicaciones.
- Si no tiene una instancia de SQL Database o de Instancia administrada de SQL, pero tiene orígenes y destinos de datos locales, debe crearlas en la misma ubicación de una red virtual conectada a la red local.  De esta manera, puede crear su instancia de Azure-SSIS IR mediante la nueva instancia de Azure SQL Database o Instancia administrada de SQL y unirse a esa red virtual, todo en la misma ubicación, con lo que se reduce de manera eficaz los movimientos de datos entre distintas ubicaciones.
- Si la ubicación de su instancia existente de Azure SQL Database o Instancia administrada de SQL no es la misma que la ubicación de una red virtual conectada a la red local, cree primero la instancia de Azure-SSIS IR con una instancia existente de Azure SQL Database o Instancia administrada de SQL, únase a otra red virtual de la misma ubicación y, luego, configure una conexión entre redes virtuales en distintas ubicaciones.

En el diagrama siguiente se muestra la configuración de la ubicación de Data Factory y sus instancias de Integration Runtime:

:::image type="content" source="media/concepts-integration-runtime/integration-runtime-location.png" alt-text="Ubicación de Integration Runtime":::

## <a name="determining-which-ir-to-use"></a>Determinar qué instancias de Integration Runtime usar
Si una actividad se asocia a más de un tipo de entorno de ejecución de integración, se resolverá en uno de ellos. El entorno de ejecución de integración autohospedado tiene prioridad sobre el entorno de ejecución de integración de las áreas de trabajo de Azure Data Factory o Synapse con una red virtual administrada. Y este último tiene prioridad sobre el entorno de ejecución de integración global de Azure.

Por ejemplo, se usa una actividad de copia para copiar datos del origen al receptor. El entorno de ejecución de integración global de Azure está asociado con el servicio vinculado del origen y un entorno de ejecución de integración de Azure de una red virtual administrada por Azure Data Factory se asocia con el servicio vinculado del receptor; el resultado es que tanto el servicio vinculado de origen y como el del receptor usan el entorno de ejecución de integración de las áreas de trabajo de Azure Data Factory o Synapse con una red virtual administrada. Sin embargo, si un entorno de ejecución de integración autohospedado asocia el servicio vinculado del origen, el servicio vinculado de origen y el servicio vinculado receptor usan el entorno de ejecución de integración autohospedado.

### <a name="copy-activity"></a>Actividad de copia

Para la actividad de copia, necesita servicios vinculados de origen y receptor para definir la dirección del flujo de datos. Se utiliza la lógica siguiente para determinar qué instancia de Integration Runtime se utiliza para realizar la copia: 

- **Copia entre dos orígenes de datos en la nube**: si los servicios vinculados del origen y receptor usan Azure IR, se usa la instancia regional de Azure IR, si se ha especificado, o se determina automáticamente una ubicación para Azure IR si decidió usar la opción de resolución automática de IR (la opción predeterminada) como se ha descrito en la sección [Ubicación de Integration Runtime](#integration-runtime-location).
- **Copia de datos entre un origen de datos en la nube y un origen de datos en una red privada**: si el servicio vinculado de origen o de receptor apunta a una instancia de IR autohospedado, la actividad de copia se ejecuta en esa instancia de Integration Runtime autohospedado.
- **Copia entre dos orígenes de datos en una red privada**: el servicio vinculado de origen y de receptor deben apuntar a la misma instancia de Integration Runtime y esa instancia se usa para ejecutar la actividad de copia.

### <a name="lookup-and-getmetadata-activity"></a>Actividad Lookup y GetMetadata

La actividad Lookup y GetMetadata se ejecuta en el entorno de ejecución de integración asociado al servicio vinculado de almacén de datos.

### <a name="external-transformation-activity"></a>Actividad de transformación externa

Cada actividad de transformación externa que usa un motor de proceso externo tiene un servicio vinculado de proceso de destino, que señala a un entorno de ejecución de integración. Esta instancia de entorno de ejecución de integración determina la ubicación desde la que se envía la actividad de transformación codificada externamente.

### <a name="data-flow-activity"></a>Actividad Data Flow

Las actividades de Data Flow se ejecutan en el entorno de ejecución de integración asociado a ella. El proceso de Spark que usan los flujos de datos viene determinado por las propiedades del flujo de datos de su instancia de Azure Integration Runtime y se administran por completo en ADF.

## <a name="next-steps"></a>Pasos siguientes

Vea los artículos siguientes:

- [Cómo crear y configurar una instancia de Azure Integration Runtime](create-azure-integration-runtime.md)
- [Crear Integration Runtime autohospedado](create-self-hosted-integration-runtime.md)
- [Creación de una instancia de Integration Runtime de SSIS de Azure](create-azure-ssis-integration-runtime.md). En este artículo se amplía el tutorial y se proporcionan instrucciones sobre el uso de Instancia administrada de SQL y la unión de Integration Runtime a una red virtual.