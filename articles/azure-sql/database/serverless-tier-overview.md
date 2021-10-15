---
title: Nivel de servicio de informática sin servidor
description: En este artículo se describe el nuevo nivel de proceso sin servidor y se compara con el nivel de proceso aprovisionado existente para Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service-overview
ms.custom: test sqldbrb=1, devx-track-azurecli, devx-track-azurepowershell
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: mathoma, wiassaf
ms.date: 9/28/2021
ms.openlocfilehash: cc9c0f35be998e8ef3947946c84bc67a94f125cb
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129235637"
---
# <a name="azure-sql-database-serverless"></a>Azure SQL Database sin servidor
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Sin servidor es un nivel de proceso para las bases de datos únicas de Azure SQL Database que se escala automáticamente según la demanda de carga de trabajo y se factura según la cantidad de proceso usada por segundo. El nivel de proceso sin servidor también detiene automáticamente las bases de datos durante períodos inactivos cuando solo se factura el almacenamiento y reanuda automáticamente las bases de datos cuando finaliza la actividad.

## <a name="serverless-compute-tier"></a>Nivel de servicio de informática sin servidor

El nivel de proceso sin servidor para bases de datos únicas de Azure SQL Database se parametriza mediante un intervalo de escalado automático de proceso y un retraso de pausa automática. La configuración de estos parámetros da forma a la experiencia de rendimiento de la base de datos y el costo de proceso.

![facturación sin servidor](./media/serverless-tier-overview/serverless-billing.png)

### <a name="performance-configuration"></a>Configuración del rendimiento

- Los valores **mínimos de núcleos virtuales** y los valores **máximos de núcleos virtuales** son parámetros configurables que definen el intervalo de capacidad de proceso disponible para la base de datos. Los límites de memoria y E/S son proporcionales al intervalo de núcleos virtuales especificado.  
- La **demora de pausa automática** es un parámetro configurable que define el período de tiempo de que la base de datos debe estar inactiva antes de pausarse automáticamente. La base de datos se reanuda automáticamente cuando se produce el siguiente inicio de sesión u otra actividad.  Como alternativa, se puede deshabilitar la pausa automática.

### <a name="cost"></a>Coste

- El coste de una base de datos sin servidor es la suma del coste de proceso y el coste de almacenamiento.
- Cuando el uso de proceso es entre los límites mínimos y máximos configurados, el coste de proceso se basa en el núcleo virtual y la memoria utilizada.
- Cuando el uso de proceso está por debajo de los límites mínimos configurados, el coste de proceso se basa en los núcleos virtuales mínimos y la cantidad mínima de memoria configurada.
- Cuando se pausa la base de datos, el coste de proceso es cero y solo se incurre en costes de almacenamiento.
- El coste de almacenamiento se determina de la misma manera que el nivel de proceso aprovisionado.

Para más detalles sobre costes, consulte [Facturación](serverless-tier-overview.md#billing).

## <a name="scenarios"></a>Escenarios

Este nivel de proceso sin servidor ofrece una relación entre precio y rendimiento optimizada para bases de datos únicas con patrones de uso intermitentes e impredecibles, que pueden permitirse alguna demora en el calentamiento de los recursos de proceso después de períodos de inactividad. En cambio, el nivel de proceso aprovisionado ofrece una relación entre precio y rendimiento optimizada para bases de datos únicas o varias bases de datos en grupos elásticos con mayor uso medio que no pueden permitirse ninguna demora en el calentamiento de los recursos de proceso.

### <a name="scenarios-well-suited-for-serverless-compute"></a>Escenarios adecuados para el proceso sin servidor

- Bases de datos únicas con patrones de uso impredecibles e intermitentes, intercalados con períodos de inactividad y menor uso promedio de proceso a lo largo del tiempo.
- Bases de datos únicas en el nivel de proceso aprovisionado con frecuentes cambios de escala y clientes que prefieren delegar el cambio de escala del proceso en el servicio.
- Nuevas bases de datos únicas sin historial de uso donde el tamaño de proceso es difícil (o incluso imposible) de estimar antes de la implementación en SQL Database.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Escenarios adecuados para el proceso aprovisionado

- Bases de datos únicas con patrones de uso más regular y predecible y mayor uso promedio de proceso a lo largo del tiempo.
- Bases de datos que no pueden tolerar compensaciones de rendimiento resultantes de recortes de memoria más frecuentes o de demoras en la reanudación automática desde un estado de pausa.
- Varias bases de datos con patrones de uso impredecibles e intermitentes que se pueden consolidar en grupos elásticos para una mejor optimización de la relación precio-rendimiento.

## <a name="comparison-with-provisioned-compute-tier"></a>Comparación con el nivel de proceso aprovisionado

La tabla siguiente resume las diferencias entre el nivel de proceso sin servidor y el nivel de proceso aprovisionado:

| | **Proceso sin servidor** | **Proceso aprovisionado** |
|:---|:---|:---|
|**Patrones de uso de bases de datos**| Uso intermitente e impredecible con menor uso promedio de proceso a lo largo del tiempo. | Patrones de uso más regulares con mayor uso promedio de proceso a lo largo del tiempo, o varias bases de datos que usen grupos elásticos.|
| **Trabajo de administración del rendimiento** |Inferior|Superior|
|**Escalado de proceso**|Automático|Manual|
|**Capacidad de respuesta del proceso**|Menor después de períodos de inactividad|Inmediata|
|**Granularidad de facturación**|Por segundo|Por hora|

## <a name="purchasing-model-and-service-tier"></a>Modelo de compra y nivel de servicio

SQL Database sin servidor solo se admite actualmente en el nivel de uso general en hardware de generación 5 en el modelo de compra de núcleos virtuales.

## <a name="autoscaling"></a>Escalado automático

### <a name="scaling-responsiveness"></a>Escalado de la capacidad de respuesta

En general, las bases de datos sin servidor se ejecutan en una máquina con capacidad suficiente para satisfacer la demanda de recursos sin interrupciones para cualquier cantidad de proceso solicitada dentro de los límites establecidos por el valor máximo de núcleos virtuales. En ocasiones, se produce automáticamente un equilibrio de carga si la máquina no puede satisfacer la demanda de recursos en cuestión de minutos. Por ejemplo, si la demanda de recursos es de 4 núcleos virtuales, pero solo hay 2 disponibles, puede que se tarde unos minutos en equilibrar la carga antes de que se proporcionen los 4 núcleos virtuales. La base de datos permanece en línea durante el equilibrio de carga excepto durante un breve período al final de la operación cuando se deshabilitan las conexiones.

### <a name="memory-management"></a>Administración de memoria

La memoria para bases de datos sin servidor se reclama con mayor frecuencia que la de las bases de datos de proceso aprovisionadas. Este comportamiento es importante para controlar costos en el nivel de proceso sin servidor y puede afectar al rendimiento.

#### <a name="cache-reclamation"></a>Reclamación de memoria caché

A diferencia de las bases de datos de proceso aprovisionadas, la memoria de la caché de SQL se reclama desde una base de datos sin servidor cuando el uso de CPU o de la memoria caché activa es bajo.

- La utilización de la memoria caché activa se considera baja cuando el tamaño total de las entradas de caché más recientes está por debajo de un determinado umbral durante un período de tiempo.
- Cuando se desencadena la reclamación de la memoria caché, el tamaño de la caché de destino se reduce de forma incremental a una fracción del tamaño anterior y la reclamación solo continúa si el uso sigue siendo bajo.
- Cuando se produce la reclamación de memoria caché, la directiva para seleccionar las entradas de esta que se van a expulsar es la misma directiva de selección que para las bases de datos de proceso aprovisionadas cuando la presión de memoria es elevada.
- El tamaño de la memoria caché no se reduce nunca por debajo del límite de memoria mínimo definido por el número mínimo de núcleos virtuales, cuyo valor se puede configurar.

En el caso de las bases de datos sin servidor y las de proceso aprovisionadas, se pueden expulsar entradas de la caché si se usa toda la memoria disponible.

Cuando el uso de la CPU es bajo, la utilización de la caché activa puede ser alto en función del patrón de uso e impedir la reclamación de memoria.  Además, puede haber otros retrasos después de que la actividad del usuario se detenga antes de que se produzca la reclamación de la memoria debido a los procesos en segundo plano periódicos que responden a la actividad anterior del usuario.  Por ejemplo, las operaciones de eliminación y las tareas de limpieza del Almacén de consultas generan registros fantasma que se marcan para su eliminación, pero no se eliminan físicamente hasta que se ejecuta el proceso de limpieza de registros fantasma. La limpieza de registros fantasma puede suponer la lectura de páginas de datos adicionales en la caché.

#### <a name="cache-hydration"></a>Hidratación de la memoria caché

La memoria caché de SQL crece a medida que se capturan datos del disco de la misma manera y a la misma velocidad que para las bases de datos aprovisionadas. Cuando la base de datos está ocupada, la memoria caché puede crecer sin restricciones hasta el límite máximo de memoria.

## <a name="auto-pausing-and-auto-resuming"></a>Pausa y reanudación automáticas

### <a name="auto-pausing"></a>Pausa automática

La pausa automática se desencadena si todas las condiciones siguientes se cumplen durante la demora de pausa automática:

- Número de sesiones = 0
- CPU = 0 para la carga de trabajo de usuario que se ejecuta en el grupo de usuarios

Hay disponible una opción para deshabilitar la pausa automática si se desea.

Las características siguientes no admiten la pausa automática, pero admiten el escalado automático. Si se utiliza cualquiera de las siguientes características, la pausa automática debe estar desactivada y la base de datos permanecerá en línea, independientemente de la duración de la inactividad de la base de datos:

- Replicación geográfica ([replicación geográfica activa](active-geo-replication-overview.md) y [grupos de conmutación por error automática](auto-failover-group-overview.md)).
- [Retención de copia de seguridad a largo plazo](long-term-retention-overview.md) (LTR).
- La base de datos de sincronización utilizada en [SQL Data Sync](sql-data-sync-data-sql-server-sql-database.md). A diferencia de las bases de datos de sincronización, las bases de datos centrales y miembro admiten la pausa automática.
- [Alias DNS](dns-alias-overview.md) creado para el servidor lógico que contiene una base de datos sin servidor.
- [Trabajos elásticos (versión preliminar)](elastic-jobs-overview.md), cuando la base de datos de trabajos es una base de datos sin servidor. Las bases de datos destinadas a trabajos elásticos admiten la pausa automática y se reanudarán mediante las conexiones del trabajo.

Se impide temporalmente la pausa automática durante la implementación de algunas actualizaciones de servicio que requieren que la base de datos esté en línea.  En tales casos, se vuelve a permitir la pausa automática una vez finalizada la actualización del servicio.

#### <a name="auto-pause-troubleshooting"></a>Solución de problemas de la pausa automática

Si la pausa automática está habilitada, pero una base de datos no se pausa automáticamente después del período de retraso, y no se utilizan las características mencionadas anteriormente, la aplicación o las sesiones de usuario pueden estar impidiendo esta característica. Para ver si hay alguna aplicación o sesión de usuario conectada actualmente a la base de datos, conéctese a la base de datos mediante cualquier herramienta de cliente y ejecute la consulta siguiente:

```sql
SELECT session_id,
       host_name,
       program_name,
       client_interface_name,
       login_name,
       status,
       login_time,
       last_request_start_time,
       last_request_end_time
FROM sys.dm_exec_sessions AS s
INNER JOIN sys.dm_resource_governor_workload_groups AS wg
ON s.group_id = wg.group_id
WHERE s.session_id <> @@SPID
      AND
      (
      (
      wg.name like 'UserPrimaryGroup.DB%'
      AND
      TRY_CAST(RIGHT(wg.name, LEN(wg.name) - LEN('UserPrimaryGroup.DB') - 2) AS int) = DB_ID()
      )
      OR
      wg.name = 'DACGroup'
      );
```

> [!TIP]
> Después de ejecutar la consulta, asegúrese de desconectarse de la base de datos. De lo contrario, la sesión abierta usada por la consulta impedirá la pausa automática.

Si el conjunto de resultados no está vacío, significa que hay sesiones que impiden la pausa automática. 

Si el conjunto de resultados está vacío, todavía es posible que las sesiones estuvieran abiertas, posiblemente durante un corto periodo de tiempo, en algún momento anterior durante el periodo de retraso de la pausa automática. Para ver si dicha actividad se ha producido durante el período de retraso, puede usar [Auditoría de Azure SQL](auditing-overview.md) y examinar los datos de auditoría del período pertinente.

La presencia de sesiones abiertas, con o sin uso simultáneo de CPU en el grupo de recursos de usuario, es la razón más común de que una base de datos sin servidor no se pause automáticamente según lo previsto.

### <a name="auto-resuming"></a>Reanudación automática

La reanudación automática se desencadena si se cumple cualquiera de las siguientes condiciones en cualquier momento:

|Característica|Desencadenador de reanudación automática|
|---|---|
|Autenticación y autorización|Inicio de sesión|
|Detección de amenazas|Habilitación o deshabilitación de la configuración de detección de amenazas en el nivel de base de datos o servidor.<br>Modificación de la configuración de detección de amenazas en el nivel de base de datos o servidor.|
|Clasificación y detección de datos|Adición, modificación, eliminación o visualización de las etiquetas de confidencialidad|
|Auditoría|Visualización de registros de auditoría<br>Actualización o visualización de la directiva de auditoría.|
|Enmascaramiento de datos|Adición, modificación, eliminación o visualización de reglas de enmascaramiento de datos|
|Cifrado de datos transparente|Visualización del estado del cifrado de datos transparente|
|Evaluación de vulnerabilidades|Exámenes ad hoc y exámenes periódicos si están habilitados|
|Almacén de datos de consulta (rendimiento)|Modificación o visualización de la configuración del almacén de consulta|
|Recomendaciones de rendimiento|Visualización o aplicación de recomendaciones de rendimiento|
|Ajuste automático|Aplicación y comprobación de recomendaciones de ajuste automático, como la indexación automática|
|Copia de base de datos|Creación de base de datos como copia.<br>Exportación a un archivo BACPAC.|
|Sincronización de datos SQL|Sincronización entre la base de datos central y las bases de datos miembro que se ejecutan según una programación configurable o bien de forma manual|
|Modificación de algunos metadatos de base de datos|Adición de nuevas etiquetas de base de datos.<br>Cambio del número máximo de núcleos virtuales, el número mínimo de núcleos virtuales y el retraso de la pausa automática.|
|SQL Server Management Studio (SSMS)|Al usar las versiones de SSMS anteriores a 18.1 y abrir una nueva ventana de consulta para cualquier base de datos en el servidor, se reanudará cualquier base de datos en pausa automática en el mismo servidor. Este comportamiento no se produce si se usa SSMS versión 18.1 o posterior.|

La supervisión, la administración u otras soluciones que realicen cualquiera de las operaciones indicadas anteriormente desencadenarán la reanudación automática.

También se desencadena la reanudación automática durante la implementación de algunas actualizaciones de servicio que requieren que la base de datos esté en línea.

### <a name="connectivity"></a>Conectividad

Si una base de datos sin servidor está en pausa, la primera vez que se inicie sesión se reanudará la base de datos y se devolverá un error con el código 40613 que indica que la base de datos no está disponible. Una vez que se reanude la base de datos, será necesario intentar iniciar sesión de nuevo para establecer la conectividad. No es necesario modificar los clientes de la base de datos con lógica de reintento de conexión.  Para ver las opciones de lógica de reintento de conexión integradas en el controlador SqlClient, consulte [Lógica de reintento configurable en SqlClient](/sql/connect/ado-net/configurable-retry-logic).

### <a name="latency"></a>Latencia

La latencia de la reanudación y la pausa automáticas de una base de datos sin servidor es, por lo general, de 1 minuto para la reanudación automática y de 1 a 10 minutos para la pausa automática.

### <a name="customer-managed-transparent-data-encryption-byok"></a>Cifrado de datos transparente administrado por el cliente (BYOK)

Si se usa el [cifrado de datos transparente administrado por el cliente](transparent-data-encryption-byok-overview.md) (BYOK) y la base de datos sin servidor se pausa automáticamente cuando se produce la eliminación o la revocación de claves, la base de datos permanece en estado de pausa automática.  En este caso, después de que la base de datos se reanude la próxima vez, esta dejará de estar accesible en aproximadamente 10 minutos. Si la base de datos pasa a ser inaccesible, el proceso de recuperación es el mismo que para las bases de datos de proceso aprovisionadas. Si la base de datos sin servidor está en línea cuando se produce la eliminación o la revocación de claves, la base de datos también pasa a ser inaccesible en el plazo de unos 10 minutos, como sucede con las bases de datos de proceso aprovisionadas.

## <a name="onboarding-into-serverless-compute-tier"></a>Incorporación en un nivel de proceso sin servidor

La creación de una nueva base de datos o el cambio de una base de datos existente a un nivel de proceso sin servidor siguen el mismo patrón que la creación de una nueva base de datos en el nivel de proceso aprovisionado y constan de los dos pasos siguientes.

1. Especifique el objetivo de servicio. El objetivo de servicio preceptúa el nivel de servicio, la generación de hardware y el máximo de núcleos virtuales. Para ver las opciones de objetivo de servicio, consulte [Límites de los recursos sin servidor](resource-limits-vcore-single-databases.md#general-purpose---serverless-compute---gen5)


2. Opcionalmente, especifique el número mínimo de núcleos virtuales y el retraso de la pausa automática para cambiar sus valores predeterminados. En la siguiente tabla se muestran los valores disponibles para estos parámetros.

   |Parámetro|Opciones de valores|Valor predeterminado|
   |---|---|---|---|
   |Número mínimo de núcleos virtuales|Depende de la cantidad máxima de núcleos virtuales configurada; consulte [Límites de los recursos](resource-limits-vcore-single-databases.md#general-purpose---serverless-compute---gen5).|0,5 núcleos virtuales|
   |Demora de pausa automática|Mínimos: 60 minutos (1 hora)<br>Máximo: 10 080 minutos (7 días)<br>Incrementos: 10 minutos<br>Deshabilitar pausa automática: -1|60 minutos|


### <a name="create-a-new-database-in-the-serverless-compute-tier"></a>Creación de una nueva base de datos en el nivel de proceso sin servidor

En el siguiente ejemplo se crea una base de datos en el nivel de proceso sin servidor.

#### <a name="use-azure-portal"></a>Usar Azure Portal

Consulte [Quickstart: Creación de una base de datos única en Azure SQL Database con Azure Portal](single-database-create-quickstart.md).


#### <a name="use-powershell"></a>Uso de PowerShell

```powershell
New-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
  -ComputeModel Serverless -Edition GeneralPurpose -ComputeGeneration Gen5 `
  -MinVcore 0.5 -MaxVcore 2 -AutoPauseDelayInMinutes 720
```
#### <a name="use-azure-cli"></a>Uso de CLI de Azure

```azurecli
az sql db create -g $resourceGroupName -s $serverName -n $databaseName `
  -e GeneralPurpose -f Gen5 --min-capacity 0.5 -c 2 --compute-model Serverless --auto-pause-delay 720
```


#### <a name="use-transact-sql-t-sql"></a>Uso de Transact-SQL (T-SQL)

Cuando se usa T-SQL, se aplican los valores predeterminados para el número mínimo de núcleos virtuales y la demora de pausa automática. Más adelante se pueden cambiar desde el portal o a través de otras API de administración (PowerShell, CLI de Azure, API REST).

```sql
CREATE DATABASE testdb
( EDITION = 'GeneralPurpose', SERVICE_OBJECTIVE = 'GP_S_Gen5_1' ) ;
```

Para más información, consulte [CREATE DATABASE](/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current&preserve-view=true).  

### <a name="move-a-database-from-the-provisioned-compute-tier-into-the-serverless-compute-tier"></a>Traslado de una base de datos del nivel de proceso aprovisionado al nivel de proceso sin servidor

En el siguiente ejemplo se mueve una base de datos del nivel de proceso aprovisionado al nivel de proceso sin servidor.

#### <a name="use-powershell"></a>Uso de PowerShell

```powershell
Set-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
  -Edition GeneralPurpose -ComputeModel Serverless -ComputeGeneration Gen5 `
  -MinVcore 1 -MaxVcore 4 -AutoPauseDelayInMinutes 1440
```

#### <a name="use-azure-cli"></a>Uso de CLI de Azure

```azurecli
az sql db update -g $resourceGroupName -s $serverName -n $databaseName `
  --edition GeneralPurpose --min-capacity 1 --capacity 4 --family Gen5 --compute-model Serverless --auto-pause-delay 1440
```

#### <a name="use-transact-sql-t-sql"></a>Uso de Transact-SQL (T-SQL)

Cuando se usa T-SQL, se aplican los valores predeterminados para el número mínimo de núcleos virtuales y la demora de pausa automática. Más adelante se pueden cambiar desde el portal o a través de otras API de administración (PowerShell, CLI de Azure, API REST).

```sql
ALTER DATABASE testdb 
MODIFY ( SERVICE_OBJECTIVE = 'GP_S_Gen5_1') ;
```

Para más información, consulte [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current&preserve-view=true).

### <a name="move-a-database-from-the-serverless-compute-tier-into-the-provisioned-compute-tier"></a>Traslado de una base de datos del nivel de proceso sin servidor al nivel de proceso aprovisionado

Una base de datos sin servidor se puede mover a un nivel de proceso aprovisionado de la misma manera que se mueve una base de datos de proceso aprovisionado a un nivel de proceso sin servidor.

## <a name="modifying-serverless-configuration"></a>Modificación de la configuración sin servidor

### <a name="use-powershell"></a>Uso de PowerShell

Puede usar el comando [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) en PowerShell con los argumentos `MaxVcore`, `MinVcore` y `AutoPauseDelayInMinutes` para modificar el número máximo o mínimo de núcleos virtuales, así como la demora de la pausa automática.

### <a name="use-azure-cli"></a>Uso de CLI de Azure

Puede usar el comando [az sql db update](/cli/azure/sql/db#az_sql_db_update) en la CLI de Azure con los argumentos `capacity`, `min-capacity` y `auto-pause-delay` para modificar el número máximo o mínimo de núcleos virtuales, así como la demora de la pausa automática.

## <a name="monitoring"></a>Supervisión

### <a name="resources-used-and-billed"></a>Recursos utilizados y facturados

Los recursos de una base de datos sin servidor se encapsulan por paquete de la aplicación, instancia de SQL y entidades de grupo de recursos del usuario.

#### <a name="app-package"></a>Paquete de aplicaciones

El paquete de aplicaciones es el límite de administración de recursos más externo de una base de datos, independientemente de si la base de datos se encuentra en un nivel de proceso sin servidor o aprovisionado. El paquete de la aplicación contiene la instancia de SQL y servicios externos (como la búsqueda de texto completo) que, en conjunto, abarcan todos los recursos de usuario y del sistema que utiliza una base de datos en SQL Database. Generalmente, la instancia de SQL domina el uso de recursos global en el paquete de aplicaciones.

#### <a name="user-resource-pool"></a>Grupo de recursos de usuario

El grupo de recursos de usuario es un límite interno de administración de recursos para una base de datos, independientemente de si la base de datos está en un nivel de proceso sin servidor o aprovisionado. El grupo de recursos de usuario abarca la CPU y la E/S de las cargas de trabajo de usuario generadas por consultas de DDL, como CREATE y ALTER, consultas de DML, como INSERT, UPDATE, DELETE y consultas MERGE y SELECT. Por lo general, estas consultas representan la proporción de uso dentro del paquete de aplicaciones más importante.

### <a name="metrics"></a>Métricas

Las métricas para supervisar el uso de recursos del paquete de la aplicación y el grupo de recursos de usuario de una base de datos sin servidor se muestran en la tabla siguiente:

|Entidad|Métrica|Descripción|Unidades|
|---|---|---|---|
|Paquete de aplicaciones|app_cpu_percent|Porcentaje de núcleos virtuales utilizado por la aplicación con respecto al máximo de núcleos virtuales permitido para la aplicación.|Porcentaje|
|Paquete de aplicaciones|app_cpu_billed|La cantidad de procesos que se facturan para la aplicación durante el período de informe. El importe pagado durante este período es el producto de esta métrica por el precio de la unidad de núcleo virtual. <br><br>Los valores de esta métrica se determinan al agregar en el tiempo el máximo de CPU utilizada y la memoria usada por segundo. Si la cantidad utilizada es menor que la cantidad mínima aprovisionada definida por el mínimo de núcleos virtuales y la memoria mínima, se factura la cantidad mínima aprovisionada. Para comparar la CPU y la memoria con fines de facturación, la memoria se normaliza en unidades de núcleos virtuales cambiando la escala de la cantidad de GB de memoria en 3 GB por núcleo virtual.|Segundos de núcleo virtual|
|Paquete de aplicaciones|app_memory_percent|Porcentaje de memoria utilizada por la aplicación con respecto a la memoria máxima permitida para la aplicación.|Porcentaje|
|Grupo de recursos de usuario|cpu_percent|Porcentaje de núcleos virtuales utilizado por la carga de trabajo de usuario con respecto al máximo de núcleos virtuales permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de recursos de usuario|data_IO_percent|Porcentaje de IOPS de datos utilizado por la carga de trabajo de usuario con respecto al máximo de IOPS de datos permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de recursos de usuario|log_IO_percent|Porcentaje de MB/s de registro utilizado por la carga de trabajo de usuario con respecto al máximo de MB/s de registro permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de recursos de usuario|workers_percent|Porcentaje de trabajos utilizado por la carga de trabajo de usuario con respecto al máximo de trabajos permitido para la carga de trabajo de usuario.|Porcentaje|
|Grupo de recursos de usuario|sessions_percent|Porcentaje de sesiones utilizado por la carga de trabajo de usuario con respecto al máximo de sesiones permitido para la carga de trabajo de usuario.|Porcentaje|

### <a name="pause-and-resume-status"></a>Estado de pausa y reanudación

En Azure Portal, se muestra el estado de la base de datos en el panel de información general del servidor que enumera las bases de datos que contiene. El estado de la base de datos también se muestra en el panel de información general de la base de datos.

Con los siguientes comandos para consultar el estado de pausa y reanudación de una base de datos:

#### <a name="use-powershell"></a>Uso de PowerShell

```powershell
Get-AzSqlDatabase -ResourceGroupName $resourcegroupname -ServerName $servername -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

#### <a name="use-azure-cli"></a>Uso de CLI de Azure

```azurecli
az sql db show --name $databasename --resource-group $resourcegroupname --server $servername --query 'status' -o json
```

## <a name="resource-limits"></a>Límites de recursos

Para ver los límites de recursos, consulte [Nivel de proceso sin servidor](resource-limits-vcore-single-databases.md#general-purpose---serverless-compute---gen5).

## <a name="billing"></a>Facturación

La cantidad de proceso que se factura es el máximo de CPU y memoria usado en cada segundo. Si la cantidad de CPU y memoria usadas es inferior a la cantidad mínima aprovisionada para cada una, se factura la cantidad aprovisionada. Para comparar la CPU y la memoria con fines de facturación, la memoria se normaliza en unidades de núcleos virtuales cambiando la escala de la cantidad de GB de memoria en 3 GB por núcleo virtual.

- **Recurso facturado**: CPU y memoria
- **Importe facturado**: precio de la unidad de núcleo virtual * máx. (mínimo de núcleos virtuales, núcleos virtuales usados, GB de memoria mínima * 1/3, GB de memoria usada * 1/3) 
- **Frecuencia de facturación**: Por segundo

El precio de unidad de núcleo virtual es el costo por núcleo virtual por segundo. Consulte la [página de precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/) para conocer los precios de unidad específicos de una región determinada.

La cantidad de proceso facturada se expone mediante la métrica siguiente:

- **Métrica**: app_cpu_billed (segundos de núcleo virtual)
- **Definición**: máx. (mínimo de núcleos virtuales, núcleos virtuales usados, GB de memoria mínima * 1/3, GB de memoria usada * 1/3)
- **Frecuencia de informes**: Por minuto

Esta cantidad se calcula cada segundo y se agrega en un intervalo de 1 minuto.

### <a name="minimum-compute-bill"></a>Factura de proceso mínimo

Si una base de datos sin servidor está en pausa, la factura de proceso es cero.  Si una base de datos sin servidor no está en pausa, la factura de proceso mínimo no es inferior a la cantidad de núcleos virtuales basada en el número máximo (número mínimo de núcleos virtuales, GB de memoria mínima * 1/3).

Ejemplos:

- Supongamos que una base de datos sin servidor no está en pausa y está configurada con 8 núcleos virtuales como máximo y 1 núcleo virtual como mínimo, lo que corresponde a 3,0 GB de memoria mínima.  La factura de proceso mínimo se basa entonces en el número máximo (1 núcleo virtual, 3,0 GB * 1 núcleo virtual/3 GB) = 1 núcleo virtual.
- Supongamos que una base de datos sin servidor no está en pausa y está configurada con 4 núcleos virtuales como máximo y 0,5 núcleos virtuales como mínimo, lo que corresponde a 2,1 GB de memoria mínima.  La factura de proceso mínimo se basa entonces en el número máximo (0,5 núcleos virtuales, 2,1 GB * 1 núcleo virtual/3 GB) = 0,7 núcleos virtuales.

La [calculadora de precios de Azure SQL Database](https://azure.microsoft.com/pricing/calculator/?service=sql-database) para escenarios sin servidor se puede usar para determinar la cantidad mínima de memoria configurable en función del número de núcleos virtuales máximos y mínimos configurados.  Por norma general, si el número mínimo de núcleos virtuales configurado es superior a 0,5 núcleos virtuales, la factura de proceso mínimo es independiente de la memoria mínima configurada y solo se basa en el número mínimo de núcleos virtuales configurado.

### <a name="example-scenario"></a>Escenario de ejemplo

Considere la posibilidad de una base de datos sin servidor configurada con 1 núcleo virtual como mínimo y 4 como máximo.  Esta configuración equivale aproximadamente a 3 GB de memoria como mínimo y a 12 GB de memoria como máximo.  Supongamos que la demora de la pausa automática se establece en 6 horas y la carga de trabajo de la base de datos está activa durante las primeras 2 horas de un período de 24 horas e inactiva el resto del tiempo.    

En este caso, la base de datos se facturará por proceso y almacenamiento durante las primeras 8 horas.  Aunque la base de datos está inactiva después de la segunda hora, se le seguirá facturando por el proceso de las 6 horas siguientes en función del proceso mínimo aprovisionado mientras la base de datos está en línea.  Solo se facturará por el almacenamiento el resto del período de 24 horas mientras la base de datos está en pausa.

Más concretamente, la facturación del proceso en este ejemplo se calcula como sigue:

|Intervalo de tiempo|Núcleos virtuales que se usan cada segundo|GB que se usan cada segundo|Dimensión del proceso facturado|Segundos de núcleo virtual facturados a lo largo de un intervalo de tiempo|
|---|---|---|---|---|
|0:00-1:00|4|9|Núcleos virtuales usados|4 núcleos virtuales * 3600 segundos = 14 400 segundos de núcleo virtual|
|1:00-2:00|1|12|Memoria usada|12 GB * 1/3 * 3600 segundos = 14 400 segundos de núcleo virtual|
|2:00-8:00|0|0|Memoria mínima aprovisionada|3 GB * 1/3 * 21 600 segundos = 21 600 segundos de núcleo virtual|
|8:00-24:00|0|0|No se factura ningún proceso mientras la base de datos está en pausa|0 segundos de núcleo virtual|
|Número total de segundos de núcleo virtual facturados durante 24 horas||||50 400 segundos de núcleo virtual|

Suponga que el precio de la unidad de proceso es 0,000145 $/núcleo virtual/segundo.  El proceso que se factura en este período de 24 horas es igual al producto del precio por unidad de proceso por los segundos de núcleo virtual facturados: 0,000145 USD/núcleo virtual/segundo * 50400 segundos de núcleo virtual = aproximadamente 7,31 USD.

### <a name="azure-hybrid-benefit-and-reserved-capacity"></a>Ventaja híbrida de Azure y capacidad reservada

Los descuentos de Ventaja híbrida de Azure (AHB) y capacidad reservada no se aplican al nivel de proceso sin servidor.

## <a name="available-regions"></a>Regiones disponibles

El nivel de proceso sin servidor está disponible en todas las regiones excepto las siguientes: Este de China, Norte de China, Centro de Alemania, Nordeste de Alemania y US Gov Central (Iowa).

## <a name="next-steps"></a>Pasos siguientes

- Para comenzar, consulte [Inicio rápido: Creación de una base de datos única en Azure SQL Database con Azure Portal](single-database-create-quickstart.md).
- Para ver los límites de recursos, consulte [Límites de recursos del nivel de proceso sin servidor](resource-limits-vcore-single-databases.md#general-purpose---serverless-compute---gen5).
