---
title: Límites de recursos
titleSuffix: Azure SQL Managed Instance
description: En este artículo se proporciona información general sobre los límites de recursos de Instancia administrada de Azure SQL.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: service-overview
ms.custom: references_regions, ignite-fall-2021
ms.devlang: ''
ms.topic: reference
author: vladai78
ms.author: vladiv
ms.reviewer: mathoma, vladiv, sachinp, wiassaf
ms.date: 10/18/2021
ms.openlocfilehash: 77548bc86c98815174b2319a58554d4beee894c3
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131072201"
---
# <a name="overview-of-azure-sql-managed-instance-resource-limits"></a>Introducción a los límites de recursos de Instancia administrada de Azure SQL
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

En este artículo se proporciona información general sobre las características técnicas y los límites de recursos de Instancia administrada de Azure SQL, además de información sobre cómo solicitar un aumento de estos límites.

> [!NOTE]
> Para conocer las diferencias en las características e instrucciones T-SQL admitidas, consulte las instrucciones [Diferencias entre las características](../database/features-comparison.md) y [Compatibilidad con instrucciones T-SQL](transact-sql-tsql-differences-sql-server.md). Para conocer las diferencias generales entre los niveles de servicio de Azure SQL Database e Instancia administrada de SQL, vea [Comparación de niveles de servicio](../database/service-tiers-general-purpose-business-critical.md#service-tier-comparison).

## <a name="hardware-generation-characteristics"></a>Características de la generación de hardware

Instancia administrada de SQL tiene características y límites de recursos que dependen de la infraestructura y la arquitectura subyacentes. SQL Managed Instance puede implementarse en varias generaciones de hardware. 

> [!NOTE]
> La generación de hardware Gen5 ahora se llama **serie estándar (Gen5)** y se presentan dos nuevas generaciones de hardware en versión preliminar limitada: la **serie prémium** y la **serie prémium optimizada para memoria**.

Para obtener información sobre las generaciones de hardware anteriores, consulte [Detalles de generaciones de hardware anteriores](#previous-generation-hardware) más adelante en este artículo. 

Las generaciones de hardware tienen diferentes características, que se describen en la tabla siguiente:

|    | **Serie estándar (Gen5)** | **Serie prémium (versión preliminar)** | **Serie prémium optimizada para memoria (versión preliminar)** | 
|:-- |:-- |:-- |:-- |
| **CPU** |  Procesadores Intel&reg; E5-2673 v4 (Broadwell) 2,3 GHz, Intel&reg; SP-8160 (Skylake) e Intel&reg; 8272CL (Cascade Lake) 2,5 GHz | Procesadores Intel&reg; 8370C (Ice Lake) 2,8 GHz | Procesadores Intel&reg; 8370C (Ice Lake) 2,8 GHz |
| **Número de núcleos virtuales** <BR>Núcleo virtual =1 LP (hiperproceso) | 4-80 núcleos virtuales | 4-80 núcleos virtuales | 4-64 núcleos virtuales |
| **Memoria máxima (relación memoria/núcleo virtual)** | 5,1 GB por núcleo virtual<br/>Agregue más núcleos virtuales para obtener más memoria. | 7 GB por núcleo virtual | 13,6 GB por núcleo virtual |
| **Memoria máxima de OLTP en memoria** |  Límite de instancia: 0,8-1,65 GB por núcleo virtual | Límite de instancias: 1,1 - 2,3 GB por núcleo virtual | Límite de instancias: 2,2 - 4,5 GB por núcleo virtual |
| **Tamaño máximo de almacenamiento reservado de instancia**\* | **Uso general:** hasta 16 TB<br/> **Crítico para la empresa:** hasta 4 TB | **Uso general:** hasta 16 TB<br/> **Crítico para la empresa:** hasta 5,5 TB | **Uso general:** hasta 16 TB <br/> **Crítico para la empresa:** hasta 16 TB |

\*Depende del [número de núcleos virtuales](#service-tier-characteristics).

### <a name="regional-support-for-premium-series-hardware-generations-preview"></a>Compatibilidad regional con generaciones de hardware de la serie prémium (versión preliminar)

Actualmente, la compatibilidad con las generaciones de hardware de la serie prémium (versión preliminar pública) solo está disponible en estas regiones específicas:


| Region | **Serie prémium** | **Serie prémium optimizada para memoria** | 
|:--- |:--- |:--- |
| Centro de EE. UU. | Sí | Sí | 
| Este de EE. UU. 2 | Sí | Sí | 
| Norte de Europa | Sí | Sí | 
| Oeste de Europa | Sí | Sí | 
| Oeste de EE. UU. | Sí | Sí | 
| Oeste de EE. UU. 2 | Sí | Sí | 


### <a name="in-memory-oltp-available-space"></a>Espacio disponible de OLTP en memoria 

La cantidad de espacio OLTP en memoria en el nivel de servicio [Crítico para la empresa](../database/service-tier-business-critical.md) depende del número de núcleos virtuales y de la generación de hardware. En la tabla siguiente se indican los límites de memoria que se pueden usar para los objetos OLTP en memoria.

| **Núcleos virtuales** | **Serie estándar (Gen5)** | **Serie prémium** | **Serie prémium optimizada para memoria** | 
|:--- |:--- |:--- |:--- |
| 4 núcleos virtuales    | 3,14 GB | 4,39 GB | 8,79 GB | 
| 8 núcleos virtuales    | 6,28 GB | 8,79 GB | 22,06 GB |  
| 16 núcleos virtuales | 15,77 GB | 22,06 GB | 57,58 GB |
| 24 núcleos virtuales | 25,25 GB | 35,34 GB | 93,09 GB |
| 32 núcleos virtuales | 37,94 GB | 53,09 GB | 128,61 GB |
| 40 núcleos virtuales | 52,23 GB | 73,09 GB | 164,13 GB |
| 64 núcleos virtuales | 99,9 GB | 139,82 GB | 288,61 GB |
| 80 núcleos virtuales | 131,68 GB| 184,30 GB | - |

## <a name="service-tier-characteristics"></a>Características del nivel de servicios

Instancia administrada de SQL tiene dos niveles de servicio: [De uso general](../database/service-tier-general-purpose.md) y [Crítico para la empresa](../database/service-tier-business-critical.md). Estos niveles proporcionan [funcionalidades diferentes](../database/service-tiers-general-purpose-business-critical.md), como se describe en la tabla siguiente.

> [!Important]
> El nivel de servicio Crítico para la empresa proporciona una copia integrada adicional de Instancia administrada de SQL (réplica secundaria) que se puede usar para la carga de trabajo de solo lectura. Si puede separar las consultas de lectura y escritura de las consultas de solo lectura, análisis e informes, obtiene el doble de núcleos virtuales y memoria por el mismo precio. La réplica secundaria puede retrasarse unos segundos con respecto a la instancia principal, por lo que está diseñada para descargar las cargas de trabajo de informes o análisis que no necesitan el estado exacto actual de los datos. En la tabla siguiente, las **consultas de solo lectura** son las consultas que se ejecutan en la réplica secundaria.

| **Característica** | **Uso general** | **Crítico para la empresa** |
| --- | --- | --- |
| Número de núcleos virtuales\* | 4, 8, 16, 24, 32, 40, 64 y 80 |  **Serie estándar (Gen5)** : 4, 8, 16, 24, 32, 40, 64, 80 <BR> **Serie prémium**: 4, 8, 16, 24, 32, 40, 64, 80 <BR> **Serie prémium optimizada para memoria**: 4, 8, 16, 24, 32, 40, 64<br/>\*Se dedica el mismo número de núcleos virtuales a consultas de solo lectura. |
| Memoria máxima | **Serie estándar (Gen5)** : 20,4 GB - 408 GB (5,1 GB/núcleo virtual)<BR> **Serie prémium**: 28 GB - 560 GB (7 GB/núcleo virtual)<BR> **Serie prémium optimizada para memoria**: 54,4 GB - 870,4 GB (13,6 GB/núcleo virtual) | **Serie estándar (Gen5)** : 20,4 GB - 408 GB (5,1 GB/núcleo virtual) en cada réplica<BR> **Serie prémium**: 28 GB - 560 GB (7 GB/núcleo virtual) en cada réplica<BR> **Serie prémium optimizada para memoria**: 54,4 GB - 870,4 GB (13,6 GB/núcleo virtual) en cada réplica |
| Tamaño máximo de almacenamiento de instancia (reservado) | - 2 TB para 4 núcleos virtuales<br/>- 8 TB para 8 núcleos virtuales<br/>- 16 TB para otros tamaños <BR> | **Serie estándar (Gen5)** : <br/>- 1 TB para 4, 8 y 16 núcleos virtuales<br/>- 2 TB para 24 núcleos virtuales<br/>- 4 TB para 32, 40, 64 y 80 núcleos virtuales <BR> **Serie prémium**: <BR>- 1 TB para 4, 8 núcleos virtuales<br/>- 2 TB para 16, 24 núcleos virtuales<br/>- 4 TB para 32 núcleos virtuales<br/>- 5,5 TB para 40, 64 y 80 núcleos virtuales<br/> **Serie prémium optimizada para memoria**: <BR>- 1 TB para 4, 8 núcleos virtuales<br/>- 2 TB para 16, 24 núcleos virtuales<br/>- 4 TB para 32 núcleos virtuales<br/>- 5.5 TB para 40 núcleos virtuales<br/>- 16 TB para 64 núcleos virtuales<br/> |
| Tamaño máximo de base de datos | Hasta el tamaño de instancia disponible actualmente (según el número de núcleos virtuales). | Hasta el tamaño de instancia disponible actualmente (según el número de núcleos virtuales). |
| Tamaño máximo de tempDB | Limitado a 24 GB/núcleo virtual (96 - 1,920 GB) y el tamaño de almacenamiento de instancia disponible actualmente.<br/>Agregue más núcleos virtuales para obtener más espacio para TempDB.<br/> El tamaño del archivo de registro está limitado a 120 GB.| Hasta el tamaño de almacenamiento de instancia disponible actualmente. |
| Número máximo de bases de datos por instancia | 100 bases de datos de usuario, a menos que se alcance el límite del tamaño de almacenamiento de la instancia. | 100 bases de datos de usuario, a menos que se alcance el límite del tamaño de almacenamiento de la instancia. |
| Número máximo de archivos de base de datos por instancia | Hasta 280, a menos que se alcance el límite de tamaño de almacenamiento de instancia o [espacio de almacenamiento de Azure Premium Disk Storage](doc-changes-updates-known-issues.md#exceeding-storage-space-with-small-database-files). | 32 767 archivos por base de datos, a menos que se alcance el límite del tamaño de almacenamiento de la instancia. |
| Tamaño máximo del archivo de datos | El tamaño máximo de cada archivo de datos es de 8 TB. Use al menos dos archivos de datos para bases de datos de más de 8 TB. | Hasta el tamaño de instancia disponible actualmente (según el número de núcleos virtuales). |
| Tamaño máximo del archivo de registro | Limitado a 2 TB y el tamaño de almacenamiento de instancias disponible actualmente. | Limitado a 2 TB y el tamaño de almacenamiento de instancias disponible actualmente. |
| Datos/IOPS de registro (aproximado) | Hasta 30 000 - 40 000 IOPS por instancia*, 500 - 7500 por archivo<br/>\*[Aumentar el tamaño del archivo para obtener más IOPS](#file-io-characteristics-in-general-purpose-tier)| 16 000 - 320 000 (4000 IOPS/núcleo virtual)<br/>Agregue más núcleos virtuales para obtener un mejor rendimiento de E/S. |
| Límite de rendimiento de escritura en el registro (por instancia) | 3 MB/s por núcleo virtual<br/>120 MB/s por instancia como máximo<br/>De 22 a 65 MB/s por base de datos (según el tamaño del archivo de registro)<br/>\*[Aumentar el tamaño del archivo para mejorar el rendimiento de E/S](#file-io-characteristics-in-general-purpose-tier) | 4 MB/s por núcleo virtual<br/>96 MB/s como máximo |
| Rendimiento de datos (aproximado) | 100 - 250 MB/s por archivo<br/>\*[Aumentar el tamaño del archivo para mejorar el rendimiento de E/S](#file-io-characteristics-in-general-purpose-tier) | Sin limitación. |
| Latencia de E/S de almacenamiento (aproximada) | 5-10 ms | 1-2 ms |
| OLTP en memoria (optimización en memoria | No compatible | Disponible, [el tamaño depende del número de núcleos virtuales](#in-memory-oltp-available-space) |
| Número máximo de sesiones | 30000 | 30000 |
| Cantidad máxima de trabajos (solicitudes) simultáneos | 105 * número de núcleos virtuales + 800 | 105 * número de núcleos virtuales + 800 |
| [Réplicas de solo lectura](../database/read-scale-out.md) | 0 | 1 (incluida en el precio) |
| Aislamiento de proceso | No se admite, ya que las instancias de uso general pueden compartir hardware físico con otras instancias| **Serie estándar (Gen5)** :<br/> Compatible con 40, 64, 80 núcleos virtuales<BR> **Serie prémium**: compatible con 64, 80 núcleos virtuales <BR> **Serie prémium optimizada para memoria**: compatible con 64 núcleos virtuales |


Algunas consideraciones adicionales: 

- El **tamaño de almacenamiento de instancias disponible actualmente** es la diferencia entre el tamaño de instancia reservada y el espacio de almacenamiento usado.
- Tanto el tamaño de datos como el de archivo de registro de las bases de datos del usuario y el sistema están incluidos en el tamaño de almacenamiento de instancia que se compara con el límite de tamaño de almacenamiento máximo. Use la vista del sistema [sys.master_files](/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql) para determinar el espacio total empleado por las bases de datos. Los registros de errores no se mantienen y no se incluyen en el tamaño. Las copias de seguridad no se incluyen en el tamaño del almacenamiento.
- El rendimiento e IOPS del nivel De uso general también dependen del [tamaño de archivo](#file-io-characteristics-in-general-purpose-tier), que no está limitado explícitamente por Instancia administrada de SQL.
  Puede crear otra réplica legible en otra región de Azure mediante [grupos de conmutación por error automática](../database/auto-failover-group-configure.md).
- El número máximo de IOPS por instancia depende del diseño del archivo y la distribución de la carga de trabajo. Por ejemplo, si crea siete archivos de 1 TB con un máximo de 5000 IOPS cada uno y siete archivos pequeños (menos de 128 GB) con 500 IOPS cada uno, puede obtener 38500 IOPS por instancia (7 x 5000 + 7 x 500) si la carga de trabajo puede usar todos los archivos. Observe que también se usan algunas IOPS para las copias de seguridad automáticas.

Encuentre más información sobre los [límites de recursos en grupos de Instancia administrada de SQL en este artículo](instance-pools-overview.md#resource-limitations).

### <a name="file-io-characteristics-in-general-purpose-tier"></a>Características de E/S de archivos en el nivel De uso general

En el nivel de servicio de uso general, cada archivo de base de datos obtiene IOPS y rendimiento dedicados que dependen del tamaño de archivo. Los archivos de datos de mayor tamaño obtienen más IOPS y rendimiento. En la tabla siguiente se muestran las características de E/S de los archivos de base de datos:

| **Tamaño de archivo** | **>=0 y <=128 GiB** | **>128 y <= 512 GiB** | **>0,5 y <=1 TiB**    | **>1 y <=2 TiB**    | **>2 y <=4 TiB** | **>4 y <=8 TiB** | **>8 y <=16 TiB** |
|:--|:--|:--|:--|:--|:--|:--|:--|
| IOPS por archivo       | 500   | 2300              | 5000  | 7500              | 7500              | 12 500   | |
| Rendimiento por archivo | 100 MiB/s | 150 MiB/s | 200 MiB/s | 250 MiB/s| 250 MiB/s | 480 MiB/s |  |

Si observa una latencia de E/S alta en algún archivo de base de datos o que se va a alcanzar el límite de IOPS/rendimiento, puede mejorar el rendimiento si [aumenta el tamaño de archivo](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Increase-data-file-size-to-improve-HammerDB-workload-performance/ba-p/823337).

También hay un límite de nivel de instancia en el rendimiento de escritura de registro máximo (consulte los valores más arriba, p. ej., 22 MB/s), por lo que es posible que no pueda llegar al archivo máximo en el archivo de registro porque se está alcanzando el límite de rendimiento de la instancia.

## <a name="supported-regions"></a>Regiones admitidas

Instancia administrada de SQL solo se puede crear en las [regiones admitidas](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Para crear Instancia administrada de SQL en una región que no se admita actualmente, puede [enviar una solicitud de soporte técnico a través de Azure Portal](../database/quota-increase-request.md).

## <a name="supported-subscription-types"></a>Tipos de suscripciones admitidos

Actualmente, Instancia administrada de SQL admite la implementación solo en los siguientes tipos de suscripciones:

- [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Proveedor de nube (CSP)](/partner-center/csp-documents-and-learning-resources)
- [Desarrollo/pruebas - Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Desarrollo/pruebas - Pago por uso](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Suscripciones con crédito mensual de Azure para suscriptores de Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)

## <a name="regional-resource-limitations"></a>Limitaciones de recursos regionales

> [!Note]
> Puede encontrar la información más reciente sobre la disponibilidad por región de las suscripciones en la sección [Seleccionar regiones](https://aka.ms/sqlcapacity).

Los tipos de suscripción compatibles pueden contener un número limitado de recursos por región. Instancia administrada de SQL tiene dos límites predeterminados por región de Azure (que se pueden aumentar a petición mediante la creación de una [solicitud de soporte técnico especial en Azure Portal](../database/quota-increase-request.md)), en función de un tipo de suscripción:

- **Límite de subred**: número máximo de subredes en que se implementan instancias de Instancia administrada de SQL en una sola región.
- **Límite de unidades de núcleos virtuales**: el número máximo de unidades de núcleo virtual que se pueden implementar en todas las instancias en una sola región. Un núcleo virtual de GP usa una unidad de núcleo virtual y un núcleo virtual de BC usa cuatro unidades de núcleo virtual. El número total de instancias no está limitado, siempre que se encuentre dentro del límite de unidades de núcleo virtual.

> [!Note]
> Estos límites son opciones de configuración predeterminadas y no limitaciones técnicas. Los límites se pueden aumentar a petición mediante la creación de una [solicitud de soporte técnico especial en Azure Portal](../database/quota-increase-request.md) si necesita más instancias en la región actual. Como alternativa, puede crear nuevas instancias de Instancia administrada de SQL en otra región de Azure sin necesidad de enviar solicitudes de soporte técnico.

En la tabla siguiente se muestran los **límites regionales predeterminados** de los tipos de suscripción admitidos (estos límites se pueden ampliar mediante la solicitud de soporte técnico que se describe a continuación):

|Tipo de suscripción| Número máximo de subredes de Instancia administrada de SQL | Número máximo de unidades de núcleo virtual* |
| :---| :--- | :--- |
|CSP |16 (30 en algunas regiones**)|960 (1440 en algunas regiones**)|
|EA|16 (30 en algunas regiones**)|960 (1440 en algunas regiones**)|
|Desarrollo/pruebas - Enterprise|6|320|
|Pago por uso|6|320|
|Desarrollo/pruebas - Pago por uso|6|320|
|Pase para Azure|3|64|
|BizSpark|3|64|
|BizSpark Plus|3|64|
|Patrocinio de Microsoft Azure|3|64|
|Microsoft Partner Network|3|64|
|Visual Studio Enterprise – MPN|3|64|
|Visual Studio Enterprise|3|32|
|Visual Studio Enterprise (BizSpark)|3|32|
|Visual Studio Professional|3|32|
|Plataformas de MSDN|3|32|

\* En el planeamiento de las implementaciones, tenga en cuenta que el nivel de servicio Crítico para la empresa (BC) requiere cuatro (4) veces más capacidad de núcleo virtual que el nivel de servicio De uso general (GP). Por ejemplo: 1 núcleo virtual de GP = 1 unidad de núcleo virtual y 1 núcleo virtual de BC = 4 núcleos virtuales. Para simplificar el análisis de consumo en los límites predeterminados, resuma las unidades de núcleo virtual de todas las subredes de la región en la que se implementa Instancia administrada de SQL y compare los resultados con los límites de la unidad de instancia del tipo de suscripción. El límite **Número máximo de unidades de núcleo virtual** se aplica a cada suscripción en una región. No hay ningún límite por subredes individuales, salvo que la suma de todos los núcleos virtuales implementados en varias subredes debe ser inferior o igual al **número máximo de unidades de núcleo virtual**.

\*\* En las regiones siguientes hay más límites de subred y núcleo virtual: Este de Australia, Este de EE. UU., Este de EE. UU. 2, Norte de Europa, Centro-sur de EE. UU., Sudeste de Asia, Sur de Reino Unido, Oeste de Europa, Oeste de EE. UU. 2.

> [!IMPORTANT]
> En caso de que el límite de núcleos virtuales y de subredes sea 0, significa que no se ha establecido ningún límite regional predeterminado para el tipo de suscripción. También puede usar la solicitud de aumento de cuota para tener acceso a las suscripciones en una región específica siguiendo el mismo procedimiento e indicando los valores de núcleos virtuales y subredes necesarios.

## <a name="request-a-quota-increase"></a>Solicitar un aumento de cuota

Si necesita más instancias en las regiones actuales, puede enviar una solicitud de soporte técnico para ampliar la cuota a través de Azure Portal. Para más información, consulte [Solicitud de aumentos de cuota para Azure SQL Database](../database/quota-increase-request.md).

## <a name="previous-generation-hardware"></a>Hardware de generación anterior

En esta sección se incluyen detalles sobre las generaciones de hardware anteriores. Considere la posibilidad de [migrar la instancia de SQL Managed Instance al hardware de serie estándar (Gen5)](../database/service-tiers-vcore.md) para obtener una gama más amplia de escalabilidad de almacenamiento y núcleos virtuales, redes aceleradas, un mejor rendimiento de E/S y una latencia mínima.

- Gen4 está en proceso de eliminación gradual y no está disponible para las nuevas implementaciones. 

### <a name="hardware-generation-characteristics"></a>Características de la generación de hardware

|   | **Gen4** | 
| --- | --- | 
| **Hardware** | Procesadores Intel&reg; E5-2673 v3 (Haswell) de 2,4 GHz; núcleo virtual SSD conectado equivalente a 1 PP (núcleo físico) |   
| **Número de núcleos virtuales** | 8, 16, 24 núcleos virtuales | 
| **Memoria máxima (relación memoria/núcleo)** | 7 GB por núcleo virtual<br/>Agregue más núcleos virtuales para obtener más memoria. |  
| **Memoria máxima de OLTP en memoria** |  Límite de instancia: 1-1,5 GB por núcleo virtual |
| **Tamaño máximo de almacenamiento reservado de instancia** |  Uso general: 8 TB <br/>Crítico para la empresa: 1 TB | 

### <a name="in-memory-oltp-available-space"></a>Espacio disponible de OLTP en memoria 

La cantidad de espacio OLTP en memoria en el nivel de servicio [Crítico para la empresa](../database/service-tier-business-critical.md) depende del número de núcleos virtuales y de la generación de hardware. En la tabla siguiente se indican los límites de memoria que se pueden usar para los objetos OLTP en memoria.

| Espacio OLTP en memoria    |  **Gen4** |
| --- |  --- |
| 8 núcleos virtuales    | 8 GB |
| 16 núcleos virtuales |  20 GB |
| 24 núcleos virtuales |  36 GB |


### <a name="service-tier-characteristics"></a>Características del nivel de servicios

| **Característica** | **Uso general** | **Crítico para la empresa** |
| --- | --- | --- |
| Número de núcleos virtuales\* | Gen4: 8, 16, 24 | Gen4: 8, 16, 24 <BR>\*Se dedica el mismo número de núcleos virtuales a consultas de solo lectura. |
| Memoria máxima | Gen4: 56 GB - 168 GB (7 GB/núcleo virtual)<br/>Agregue más núcleos virtuales para obtener más memoria. | Gen4: 56 GB - 168 GB (7 GB/núcleo virtual)<br/>+ 20,4 GB - 408 GB (5,1 GB/núcleo virtual) adicionales para consultas de solo lectura.<br/>Agregue más núcleos virtuales para obtener más memoria. |
| Tamaño máximo de almacenamiento de instancia (reservado) | Gen4: 8 TB | Gen4: 1 TB  |
| Tamaño máximo de base de datos | Gen4: hasta el tamaño de instancia disponible actualmente (máximo 2 TB - 8 TB, según el número de núcleos virtuales). | Gen4: hasta el tamaño de instancia disponible actualmente (máximo 1 TB - 4 TB, según el número de núcleos virtuales). |
| Tamaño máximo de tempDB | Gen4: limitado a 24 GB/núcleo virtual (96 - 1920 GB) y el tamaño de almacenamiento de instancia disponible actualmente.<br/>Agregue más núcleos virtuales para obtener más espacio para TempDB.<br/> El tamaño del archivo de registro está limitado a 120 GB.| Gen4: hasta el tamaño de almacenamiento de instancia disponible actualmente. |
| Número máximo de bases de datos por instancia | Gen4: 100 bases de datos de usuario, a menos que se alcance el límite del tamaño de almacenamiento de la instancia. | Gen4: 100 bases de datos de usuario, a menos que se alcance el límite del tamaño de almacenamiento de la instancia. |
| Número máximo de archivos de base de datos por instancia | Gen4: hasta 280, a menos que se alcance el límite de tamaño de almacenamiento de instancia o [espacio de asignación de Azure Premium Disk Storage](../database/doc-changes-updates-release-notes.md#exceeding-storage-space-with-small-database-files). | Gen4: 32 767 archivos por base de datos, a menos que se alcance el límite del tamaño de almacenamiento de la instancia. |
| Tamaño máximo del archivo de datos | Gen4: limitado al tamaño de almacenamiento de instancia disponible actualmente (máximo 2 TB - 8 TB) y el [espacio de asignación de Azure Premium Disk Storage](../database/doc-changes-updates-release-notes.md#exceeding-storage-space-with-small-database-files). Use al menos dos archivos de datos para bases de datos de más de 8 TB. | Gen4: limitado al tamaño de almacenamiento de instancia disponible actualmente (hasta 1 TB - 4 TB). |
| Tamaño máximo del archivo de registro | Gen4: limitado a 2 TB y el tamaño de almacenamiento de instancia disponible actualmente. | Gen4: limitado a 2 TB y el tamaño de almacenamiento de instancia disponible actualmente. |
| Datos/IOPS de registro (aproximado) | Gen4: hasta 30 000 - 40 000 IOPS por instancia*, 500 - 7500 por archivo<br/>\*[Aumentar el tamaño del archivo para obtener más IOPS](#file-io-characteristics-in-general-purpose-tier)| Gen4: 16 K - 320 K (4000 IOPS/núcleo virtual)<br/>Agregue más núcleos virtuales para obtener un mejor rendimiento de E/S. | 
| Límite de rendimiento de escritura en el registro (por instancia) | Gen4: 3 MB/s por núcleo virtual<br/>120 MB/s por instancia como máximo<br/>22 - 65 MB/s por base de datos<br/>\*[Aumentar el tamaño del archivo para mejorar el rendimiento de E/S](#file-io-characteristics-in-general-purpose-tier) | Gen4: 4 MB/s por núcleo virtual<br/>96 MB/s como máximo |
| Rendimiento de datos (aproximado) | Gen4: de 100 a 250 MB/s por archivo<br/>\*[Aumentar el tamaño del archivo para mejorar el rendimiento de E/S](#file-io-characteristics-in-general-purpose-tier) | Gen4: no limitado. |
| Latencia de E/S de almacenamiento (aproximada) | Gen4: 5-10 ms | Gen4: 1-2 ms |
| OLTP en memoria (optimización en memoria | Gen4: no admitido | Gen4: disponible, el [tamaño depende del número de núcleos virtuales](#in-memory-oltp-available-space) |
| Número máximo de sesiones | Gen4: 30 000 | Gen4: 30 000 |
| Cantidad máxima de trabajos (solicitudes) simultáneos | Gen4: 210 * número de núcleos virtuales + 800 | Gen4: 210 * número de núcleos virtuales + 800 |
| [Réplicas de solo lectura](../database/read-scale-out.md) | Gen4: 0 | Gen4: 1 (incluido en el precio) |
| Aislamiento de proceso | Gen4: no admitido | Gen4: no admitido |


## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre Instancia administrada de SQL, vea [¿Qué es Instancia administrada de SQL?](sql-managed-instance-paas-overview.md)
- Para obtener información de precios, vea [Precios de Instancia administrada de SQL](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- Para obtener información sobre cómo crear la primera instancia de Instancia administrada de SQL, vea la [guía de inicio rápido](instance-create-quickstart.md).
