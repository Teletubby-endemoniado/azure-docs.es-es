---
title: Métricas compatibles de Azure Monitor por tipo de recurso
description: Lista de métricas disponibles para cada tipo de recurso con Azure Monitor.
author: rboucher
services: azure-monitor
ms.topic: reference
ms.date: 10/05/2021
ms.author: robb
ms.openlocfilehash: e0843f9e25b3059bbdd0102e4a830f151b0e87b0
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129996919"
---
# <a name="supported-metrics-with-azure-monitor"></a>Métricas compatibles con Azure Monitor

> [!NOTE]
> Esta lista se genera automáticamente. Cualquier modificación realizada en esta lista a través de GitHub puede sobrescribirse sin previo aviso. Póngase en contacto con el autor de este artículo para obtener más información sobre cómo hacer actualizaciones permanentes.

Azure Monitor proporciona varias maneras de interactuar con las métricas, como su representación en gráficos en Azure Portal, el acceso a ellas por medio de la API REST o su consulta con PowerShell o la CLI de Azure. 

Este artículo es una lista completa de todas las métricas de plataforma (es decir, recopiladas automáticamente) que actualmente están disponibles con la canalización de métricas consolidada de Azure Monitor. Es posible que las métricas modificadas o agregadas después de la fecha que se indica en la parte superior de este artículo no aparezcan en la lista. Para consultar la lista de métricas y acceder a ella mediante programación, use [2018-01-01 api-version](/rest/api/monitor/metricdefinitions). Otras métricas que no aparecen en esta lista pueden estar disponibles en el portal o mediante API heredadas.

Las métricas están organizadas por proveedor de recursos y tipo de recurso. Para obtener una lista de los servicios y los proveedores de recursos y tipos que pertenecen a las métricas, consulte [Proveedores de recursos para servicios de Azure](../../azure-resource-manager/management/azure-services-resource-providers.md).  

## <a name="exporting-platform-metrics-to-other-locations"></a>Exportación de métricas de plataforma a otras ubicaciones

Existen dos formas de exportar las métricas de plataforma de la canalización de Azure Monitor a otras ubicaciones:

- Con la [API REST de métricas](/rest/api/monitor/metrics/list).
- Con la [configuración de diagnóstico](../essentials/diagnostic-settings.md) para enrutar las métricas de plataforma a: 
    - Azure Storage.
    - Registros de Azure Monitor (y, por lo tanto, Log Analytics).
    - Centros de eventos, que es como se envían a sistemas que no son de Microsoft. 

El uso de la configuración de diagnóstico es la forma más sencilla de enrutar las métricas, pero existen algunas limitaciones: 

- **Capacidad de exportación**. Todas las métricas se pueden exportar por medio de la API REST, pero algunas no se pueden exportar mediante la configuración de diagnóstico debido a los entresijos del back-end de Azure Monitor. La columna "¿Se puede exportar con la configuración de diagnóstico?" de las tablas siguientes indica las métricas que se pueden exportar de esta manera.  

- **Métricas multidimensionales**. Actualmente no se admite el envío de métricas de varias dimensiones a otras ubicaciones a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas en valores de dimensión. 

  Por ejemplo, la métrica *Mensajes entrantes* de un centro de eventos se puede explorar y representar gráficamente por colas. Pero cuando se exporta por medio de la configuración de diagnóstico, se representa como todos los mensajes entrantes de todas las colas del centro de eventos.

## <a name="guest-os-and-host-os-metrics"></a>Métricas de SO invitado y SO host

Las métricas del sistema operativo invitado (SO invitado) que se ejecuta en Azure Virtual Machines, Service Fabric y Cloud Services *no* aparecen aquí. Las métricas del SO invitado se deben recopilar mediante uno o varios agentes que se ejecuten en el sistema operativo invitado o como parte de él. Las métricas del sistema operativo invitado incluyen los contadores de rendimiento que realizan el seguimiento del porcentaje de las CPU invitadas o el uso de la memoria, ya que se usan con frecuencia para el realizar el escalado automático o las alertas. 

Las métricas del sistema SO host *están* disponibles y se indican en las tablas. Las métricas del SO host se refieren a la sesión de Hyper-V que hospeda la sesión del SO invitado. 

> [!TIP]
> Un procedimiento recomendado es usar y configurar el agente de Azure Monitor para enviar métricas de rendimiento del SO invitado a la misma base de datos de métricas de Azure Monitor donde se almacenan las métricas de plataforma. El agente enruta las métricas del SO invitado a través de la API de [métricas personalizadas](../essentials/metrics-custom-overview.md). Después, puede crear un gráfico, generar alertas y usar las métricas del SO invitado como métricas de plataforma. 
>
> Además, o como alternativa, puede enviar las métricas del SO invitado a los registros de Azure Monitor mediante el mismo agente. Allí puede consultar las métricas en combinación con datos no pertenecientes a métricas mediante Log Analytics. 

El agente de Azure Monitor reemplaza la extensión Azure Diagnostics y el agente de Log Analytics, que se usaban anteriormente para el enrutamiento del SO invitado. Para obtener más información importante, vea [Información general sobre los agentes de Azure Monitor](../agents/agents-overview.md).

## <a name="table-formatting"></a>Formato de las tablas

La actualización más reciente agrega una nueva columna y reordena las métricas en orden alfabético. La información adicional significa que las tablas pueden tener una barra de desplazamiento horizontal en la parte inferior, en función del ancho de la ventana del explorador. Si cree que falta información, use la barra de desplazamiento para ver la totalidad de la tabla.

## <a name="microsoftaadiamazureadmetrics"></a>microsoft.aadiam/azureADMetrics

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ThrottledRequests|No|ThrottledRequests|Count|Average|Métrica del tipo azureADMetrics|Sin dimensiones|


## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|CleanerCurrentPrice|Sí|Memoria: precio actual de limpieza|Count|Average|Precio actual de memoria, USD/byte/tiempo, normalizado a 1000.|ServerResourceType|
|CleanerMemoryNonshrinkable|Sí|Memoria: memoria de limpieza no reducible|Bytes|Average|Cantidad de memoria, en bytes, que no purgará el limpiador en segundo plano.|ServerResourceType|
|CleanerMemoryShrinkable|Sí|Memoria: memoria de limpieza reducible|Bytes|Average|Cantidad de memoria, en bytes, que purgará el limpiador en segundo plano.|ServerResourceType|
|CommandPoolBusyThreads|Sí|Subprocesos: subprocesos ocupados del grupo de comandos|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de comandos.|ServerResourceType|
|CommandPoolIdleThreads|Sí|Subprocesos: subprocesos inactivos del grupo de comandos|Count|Average|Número de subprocesos inactivos del grupo de subprocesos de comandos.|ServerResourceType|
|CommandPoolJobQueueLength|Sí|Longitud de cola de trabajos de grupo de comandos|Count|Average|Número de trabajos en la cola del grupo de subprocesos de comandos.|ServerResourceType|
|CurrentConnections|Sí|Conexión: Conexiones actuales|Count|Average|Número actual de conexiones de cliente establecidas.|ServerResourceType|
|CurrentUserSessions|Sí|Sesiones de usuario actuales|Count|Average|Número actual de sesiones de usuario establecidas.|ServerResourceType|
|LongParsingBusyThreads|Sí|Subprocesos: subprocesos ocupados en análisis largos|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de análisis largos.|ServerResourceType|
|LongParsingIdleThreads|Sí|Subprocesos: subprocesos inactivos en análisis largos|Count|Average|Número de subprocesos inactivos del grupo de subprocesos en análisis largos.|ServerResourceType|
|LongParsingJobQueueLength|Sí|Subprocesos: longitud de cola de trabajos en análisis largos|Count|Average|Número de trabajos en la cola del grupo de subprocesos en análisis largos.|ServerResourceType|
|mashup_engine_memory_metric|Sí|Memoria del motor M|Bytes|Average|Uso de memoria por los procesos del motor de mashup|ServerResourceType|
|mashup_engine_private_bytes_metric|Sí|Bytes privados del motor M|Bytes|Average|Uso de bytes privados en procesos de motor de mashup.|ServerResourceType|
|mashup_engine_qpu_metric|Sí|QPU de motor M|Count|Average|Uso de QPU por los procesos del motor de mashup|ServerResourceType|
|mashup_engine_virtual_bytes_metric|Sí|Bytes virtuales del motor M|Bytes|Average|Uso de bytes virtuales en procesos de motor de mashup.|ServerResourceType|
|memory_metric|Sí|Memoria|Bytes|Average|Memoria. Intervalo de 0-25 GB para S1, 0-50 GB para S2 y 0-100 GB para S4|ServerResourceType|
|memory_thrashing_metric|Sí|Paginación excesiva de memoria|Percent|Average|Paginación excesiva media de memoria.|ServerResourceType|
|MemoryLimitHard|Sí|Memoria: límite de memoria física|Bytes|Average|Límite de memoria física del archivo de configuración.|ServerResourceType|
|MemoryLimitHigh|Sí|Memoria: límite alto de memoria|Bytes|Average|Límite alto de memoria del archivo de configuración.|ServerResourceType|
|MemoryLimitLow|Sí|Memoria: límite bajo de memoria|Bytes|Average|Límite bajo de memoria del archivo de configuración.|ServerResourceType|
|MemoryLimitVertiPaq|Sí|Memoria: VertiPaq de límite de memoria|Bytes|Average|Límite en memoria del archivo de configuración.|ServerResourceType|
|MemoryUsage|Sí|Memoria: Uso de la memoria|Bytes|Average|Uso de memoria del proceso de servidor tal como se usa para calcular el precio de la memoria del limpiador. Es igual al contador Proceso\Bytes privados más el tamaño de los datos asignados a la memoria; se ignora la memoria que ha asignado VertiPaq superior al límite de memoria de VertiPaq.|ServerResourceType|
|private_bytes_metric|Sí|Bytes privados|Bytes|Average|Bytes privados.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Sí|Subprocesos: subprocesos de trabajo de E/S ocupados del grupo de procesamiento|Count|Average|Número de subprocesos que ejecutan trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Sí|Subprocesos: subprocesos de trabajo ocupados que no son de E/S del grupo de procesamiento|Count|Average|Número de subprocesos que ejecutan trabajos que no son de E/S del grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Sí|Subprocesos: subprocesos de trabajo de E/S inactivos del grupo de procesamiento|Count|Average|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Sí|Subprocesos: subprocesos de trabajo inactivos que no son de E/S del grupo de procesamiento|Count|Average|Número de subprocesos inactivos del grupo de subprocesos de procesamiento dedicado a trabajos que no son de E/S.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Sí|Subprocesos: Longitud de cola de trabajos de E/S de grupo de procesamiento|Count|Average|Número de trabajos de E/S en la cola del grupo de subprocesos de procesamiento.|ServerResourceType|
|ProcessingPoolJobQueueLength|Sí|Longitud de cola de trabajos de grupo de procesamiento|Count|Average|Número de trabajos que no son de E/S en la cola del grupo de subprocesos de procesamiento.|ServerResourceType|
|qpu_metric|Sí|QPU|Count|Average|QPU. Intervalo de 0-100 para S1, 0-200 para S2 y 0-400 para S4|ServerResourceType|
|QueryPoolBusyThreads|Sí|Subprocesos ocupados de grupo de consultas|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de consulta.|ServerResourceType|
|QueryPoolIdleThreads|Sí|Subprocesos: subprocesos inactivos del grupo de consultas|Count|Average|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|ServerResourceType|
|QueryPoolJobQueueLength|Sí|Subprocesos: longitud de cola de trabajos del grupo de consultas|Count|Average|Número de trabajos en la cola del grupo de subprocesos de consultas.|ServerResourceType|
|Quota|Sí|Memoria: Quota|Bytes|Average|Cuota de memoria actual, en bytes. La cuota de memoria también se denomina concesión de memoria o reserva de memoria.|ServerResourceType|
|QuotaBlocked|Sí|Memoria: cuota bloqueada|Count|Average|Número actual de solicitudes de cuota que están bloqueadas hasta que se liberen otras cuotas de memoria.|ServerResourceType|
|RowsConvertedPerSec|Sí|Procesamiento: filas convertidas por segundo|CountPerSecond|Average|Velocidad de filas convertidas durante el procesamiento.|ServerResourceType|
|RowsReadPerSec|Sí|Procesamiento: filas leídas por segundo|CountPerSecond|Average|Velocidad de filas leídas de todas las bases de datos relacionales.|ServerResourceType|
|RowsWrittenPerSec|Sí|Procesamiento: filas escritas por segundo|CountPerSecond|Average|Velocidad de filas escritas durante el procesamiento.|ServerResourceType|
|ShortParsingBusyThreads|Sí|Subprocesos: subprocesos ocupados en análisis cortos|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de análisis cortos.|ServerResourceType|
|ShortParsingIdleThreads|Sí|Subprocesos: subprocesos inactivos en análisis cortos|Count|Average|Número de subprocesos inactivos del grupo de subprocesos de análisis cortos.|ServerResourceType|
|ShortParsingJobQueueLength|Sí|Subprocesos: longitud de cola de trabajos en análisis cortos|Count|Average|Número de trabajos en la cola del grupo de subprocesos en análisis cortos.|ServerResourceType|
|SuccessfullConnectionsPerSec|Sí|Conexiones correctas por segundo|CountPerSecond|Average|Tasa de finalizaciones de conexión correctas.|ServerResourceType|
|TotalConnectionFailures|Sí|Número total de errores de conexión|Count|Average|Número total de intentos de conexión con error.|ServerResourceType|
|TotalConnectionRequests|Sí|Número total de solicitudes de conexión|Count|Average|Número total de solicitudes de conexión. Se trata de llegadas.|ServerResourceType|
|VertiPaqNonpaged|Sí|Memoria: VertiPaq no paginado|Bytes|Average|Bytes de memoria bloqueados en el espacio de trabajo para que los use el motor en memoria.|ServerResourceType|
|VertiPaqPaged|Sí|Memoria: VertiPaq paginado|Bytes|Average|Bytes de memoria paginada en uso para datos en memoria.|ServerResourceType|
|virtual_bytes_metric|Sí|Bytes virtuales|Bytes|Average|Bytes virtuales.|ServerResourceType|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BackendDuration|Sí|Duration of Backend Requests (Duración de las solicitudes de back-end)|MilliSeconds|Average|Duración de las solicitudes de back-end en milisegundos|Ubicación, Nombre de host|
|Capacity|Sí|Capacity|Percent|Average|Métrica de uso para el servicio ApiManagement. Nota: En el caso de skus que no Premium, la agregación "Max" mostrará el valor como 0.|Location|
|ConnectionAttempts|Sí|WebSocket Connection Attempts (Preview) [Intentos de conexión de WebSocket (versión preliminar)]|Count|Total|Recuento de intentos de conexión de WebSocket basados en el origen y el destino seleccionados.|Ubicación, origen, destino, estado|
|Duration|Sí|Duración total de las solicitudes de puerta de enlace|MilliSeconds|Average|Duración total de las solicitudes de puerta de enlace en milisegundos|Ubicación, Nombre de host|
|EventHubDroppedEvents|Sí|Eventos EventHub quitados|Count|Total|Número de eventos omitidos por haber alcanzado el límite del tamaño de la cola|Location|
|EventHubRejectedEvents|Sí|Eventos EventHub rechazados|Count|Total|Número de eventos EventHub rechazados (configuración incorrecta o no autorizados)|Location|
|EventHubSuccessfulEvents|Sí|Eventos EventHub correctos|Count|Total|Número de eventos EventHub correctos|Location|
|EventHubThrottledEvents|Sí|Eventos EventHub limitados|Count|Total|Número de eventos EventHub limitados|Location|
|EventHubTimedoutEvents|Sí|Eventos EventHub con tiempo de espera agotado|Count|Total|Número de eventos EventHub con tiempo de espera agotado|Location|
|EventHubTotalBytesSent|Sí|Tamaño de los eventos EventHub|Bytes|Total|Tamaño total de los eventos EventHub en bytes|Location|
|EventHubTotalEvents|Sí|Eventos totales de EventHub|Count|Total|Número de eventos enviados a EventHub|Location|
|EventHubTotalFailedEvents|Sí|Eventos EventHub con errores|Count|Total|Número de eventos EventHub con errores|Location|
|FailedRequests|Sí|Failed Gateway Requests (Deprecated) [Solicitudes de puerta de enlace erróneas (en desuso)]|Count|Total|Número de errores en las solicitudes de puerta de enlace: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Ubicación, Nombre de host|
|NetworkConnectivity|Sí|Estado de conectividad de red de los recursos (versión preliminar)|Count|Average|Estado de conectividad de red de los tipos de recursos dependientes del servicio de API Management|Location, ResourceType|
|OtherRequests|Sí|Other Gateway Requests (Deprecated) [Otras solicitudes de puerta de enlace (en desuso)]|Count|Total|Número de solicitudes de puerta de enlace de otro tipo: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Ubicación, Nombre de host|
|Requests|Sí|Requests|Count|Total|Métricas de solicitud de puerta de enlace con varias dimensiones|Location, Hostname, LastErrorReason, BackendResponseCode, GatewayResponseCode, BackendResponseCodeCategory, GatewayResponseCodeCategory|
|SuccessfulRequests|Sí|Successful Gateway Requests (Deprecated) [Solicitudes de puerta de enlace correctas (en desuso)]|Count|Total|Número de solicitudes de puerta de enlace correctas: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Ubicación, Nombre de host|
|TotalRequests|Sí|Total Gateway Requests (Deprecated) [Solicitudes de puerta de enlace en total (en desuso)]|Count|Total|Número de solicitudes de puerta de enlace: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Ubicación, Nombre de host|
|UnauthorizedRequests|Sí|Unauthorized Gateway Requests (Deprecated) [Solicitudes de puerta de enlace no autorizadas (en desuso)]|Count|Total|Número de solicitudes de puerta de enlace no autorizadas: use la métrica de solicitud de varias dimensiones con la dimensión GatewayResponseCodeCategory en su lugar.|Ubicación, Nombre de host|
|WebSocketMessages|Sí|WebSocket Messages (Preview) [Mensajes de WebSocket (versión preliminar)]|Count|Total|Recuento de mensajes de WebSocket basados en el origen y el destino seleccionados.|Ubicación, origen, destino|


## <a name="microsoftappconfigurationconfigurationstores"></a>Microsoft.AppConfiguration/configurationStores

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|HttpIncomingRequestCount|Sí|HttpIncomingRequestCount|Count|Count|Número total de solicitudes HTTP entrantes.|StatusCode, Authentication|
|HttpIncomingRequestDuration|Sí|HttpIncomingRequestDuration|Count|Average|Latencia de una solicitud HTTP.|StatusCode, Authentication|
|ThrottledHttpRequestCount|Sí|ThrottledHttpRequestCount|Count|Count|Throttled http requests.|Sin dimensiones|


## <a name="microsoftappplatformspring"></a>Microsoft.AppPlatform/Spring

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|active-timer-count|Sí|active-timer-count|Count|Average|Número de temporizadores que están actualmente activos|Deployment, AppName, Pod|
|alloc-rate|Sí|alloc-rate|Bytes|Average|Número de bytes asignados en el montón administrado|Deployment, AppName, Pod|
|AppCpuUsage|Sí|Uso de CPU de la aplicación |Percent|Average|Uso de CPU reciente de la aplicación|Deployment, AppName, Pod|
|assembly-count|Sí|assembly-count|Count|Average|Número de ensamblados cargados|Deployment, AppName, Pod|
|cpu-usage|Sí|cpu-usage|Percent|Average|Porcentaje de tiempo que el proceso ha utilizado la CPU|Deployment, AppName, Pod|
|current-requests|Sí|current-requests|Count|Average|Número total de solicitudes en procesamiento durante el proceso|Deployment, AppName, Pod|
|exception-count|Sí|exception-count|Count|Total|Número de excepciones|Deployment, AppName, Pod|
|failed-requests|Sí|failed-requests|Count|Average|Número total de solicitudes con error durante el proceso|Deployment, AppName, Pod|
|gc-heap-size|Sí|gc-heap-size|Count|Average|Tamaño total del montón notificado por la recolección de elementos no utilizados (MB)|Deployment, AppName, Pod|
|gen-0-gc-count|Sí|gen-0-gc-count|Count|Average|Número de GC de la generación 0|Deployment, AppName, Pod|
|gen-0-size|Sí|gen-0-size|Bytes|Average|Tamaño del montón de gen. 0|Deployment, AppName, Pod|
|gen-1-gc-count|Sí|gen-1-gc-count|Count|Average|Número de GC de la generación 1|Deployment, AppName, Pod|
|gen-1-size|Sí|gen-1-size|Bytes|Average|Tamaño del montón de gen. 1|Deployment, AppName, Pod|
|gen-2-gc-count|Sí|gen-2-gc-count|Count|Average|Número de GC de la generación 2|Deployment, AppName, Pod|
|gen-2-size|Sí|gen-2-size|Bytes|Average|Tamaño del montón de gen. 2|Deployment, AppName, Pod|
|IngressBytesReceived|Sí|Bytes recibidos|Bytes|Average|Recuento de bytes de los clientes recibidos por Azure Spring Cloud.|Nombre de host, HttpStatus|
|IngressBytesReceivedRate|Sí|Throughput In (bytes/s) [Rendimiento de entrada (bytes/s)]|BytesPerSecond|Average|Bytes de los clientes recibidos por segundo por Azure Spring Cloud.|Nombre de host, HttpStatus|
|IngressBytesSent|Sí|Bytes enviados|Bytes|Average|Recuento de bytes enviados por Azure Spring Cloud a los clientes.|Nombre de host, HttpStatus|
|IngressBytesSentRate|Sí|Throughput Out (bytes/s) [Rendimiento de salida (bytes/s)]|BytesPerSecond|Average|Bytes enviados por segundo por Azure Spring Cloud a los clientes.|Nombre de host, HttpStatus|
|IngressFailedRequests|Sí|Solicitudes con error|Count|Average|Recuento de solicitudes con error de los clientes realizado por Azure Spring Cloud.|Nombre de host, HttpStatus|
|IngressRequests|Sí|Requests|Count|Average|Recuento de solicitudes de los clientes realizado por Azure Spring Cloud.|Nombre de host, HttpStatus|
|IngressResponseStatus|Sí|Estado de respuesta|Count|Average|Estado de la respuesta HTTP devuelta por Azure Spring Cloud. La distribución del código de estado de respuesta se puede categorizar aún más para mostrar las respuestas en las categorías 2xx, 3xx, 4xx y 5xx.|Nombre de host, HttpStatus|
|IngressResponseTime|Sí|Tiempo de respuesta|Segundos|Average|Tiempo de respuesta HTTP devuelto por Azure Spring Cloud.|Nombre de host, HttpStatus|
|jvm.gc.live.data.size|Sí|jvm.gc.live.data.size|Bytes|Average|Tamaño máximo del grupo de memoria de generación anterior después de una GC|Deployment, AppName, Pod|
|jvm.gc.max.data.size|Sí|jvm.gc.max.data.size|Bytes|Average|Tamaño máximo del grupo de memoria de generación anterior|Deployment, AppName, Pod|
|jvm.gc.memory.allocated|Sí|jvm.gc.memory.allocated|Bytes|Máxima|Se incrementa por un aumento del tamaño del grupo de memoria de generación nueva después de una GC y antes de la siguiente.|Deployment, AppName, Pod|
|jvm.gc.memory.promoted|Sí|jvm.gc.memory.promoted|Bytes|Máxima|Recuento de aumentos positivos en el tamaño del grupo de memoria de generación anterior entre antes y después de una GC|Deployment, AppName, Pod|
|jvm.gc.pause.total.count|Sí|jvm.gc.pause.total.count|Count|Total|Recuento de pausas de la GC|Deployment, AppName, Pod|
|jvm.gc.pause.total.time|Sí|jvm.gc.pause.total.time|Milisegundos|Total|Tiempo total de pausas de la GC|Deployment, AppName, Pod|
|jvm.memory.committed|Sí|jvm.memory.committed|Bytes|Average|Memoria asignada a la JVM en bytes|Deployment, AppName, Pod|
|jvm.memory.max|Sí|jvm.memory.max|Bytes|Máxima|Cantidad máxima de memoria en bytes que se puede usar para la administración de memoria.|Deployment, AppName, Pod|
|jvm.memory.used|Sí|jvm.memory.used|Bytes|Average|Memoria de la aplicación utilizada en bytes|Deployment, AppName, Pod|
|loh-size|Sí|loh-size|Bytes|Average|Tamaño del montón de LOH|Deployment, AppName, Pod|
|monitor-lock-contention-count|Sí|monitor-lock-contention-count|Count|Average|Número de veces que hubo contención al intentar realizar el bloqueo de supervisión|Deployment, AppName, Pod|
|PodCpuUsage|Sí|Uso de CPU de la aplicación|Percent|Average|Uso de CPU reciente de la aplicación|Deployment, AppName, Pod|
|PodMemoryUsage|Sí|Uso de memoria de la aplicación|Percent|Average|Uso de memoria reciente de la aplicación|Deployment, AppName, Pod|
|process.cpu.usage|Sí|process.cpu.usage|Percent|Average|Uso de CPU reciente para el proceso de JVM|Deployment, AppName, Pod|
|requests-per-second|Sí|requests-rate|Count|Average|Velocidad de solicitudes|Deployment, AppName, Pod|
|system.cpu.usage|Sí|system.cpu.usage|Percent|Average|Uso de CPU reciente de todo el sistema|Deployment, AppName, Pod|
|threadpool-completed-items-count|Sí|threadpool-completed-items-count|Count|Average|Recuento de elementos de trabajo completados del grupo de subprocesos|Deployment, AppName, Pod|
|threadpool-queue-length|Sí|threadpool-queue-length|Count|Average|Longitud de la cola de elementos de trabajo del grupo de subprocesos|Deployment, AppName, Pod|
|threadpool-thread-count|Sí|threadpool-thread-count|Count|Average|Número de subprocesos del grupo de subprocesos|Deployment, AppName, Pod|
|time-in-gc|Sí|time-in-gc|Percent|Average|% de tiempo de GC desde la última GC|Deployment, AppName, Pod|
|tomcat.global.error|Sí|tomcat.global.error|Count|Total|Error global de Tomcat|Deployment, AppName, Pod|
|tomcat.global.received|Sí|tomcat.global.received|Bytes|Total|Total de bytes recibidos de Tomcat|Deployment, AppName, Pod|
|tomcat.global.request.avg.time|Sí|tomcat.global.request.avg.time|Milisegundos|Average|Tiempo medio de las solicitudes de Tomcat|Deployment, AppName, Pod|
|tomcat.global.request.max|Sí|tomcat.global.request.max|Milisegundos|Máxima|Tiempo máximo de las solicitudes de Tomcat|Deployment, AppName, Pod|
|tomcat.global.request.total.count|Sí|tomcat.global.request.total.count|Count|Total|Recuento total de las solicitudes de Tomcat|Deployment, AppName, Pod|
|tomcat.global.request.total.time|Sí|tomcat.global.request.total.time|Milisegundos|Total|Tomcat Request Total Time (Tiempo total de la solicitud de Tomcat)|Deployment, AppName, Pod|
|tomcat.global.sent|Sí|tomcat.global.sent|Bytes|Total|Total de bytes enviados de Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.active.current|Sí|tomcat.sessions.active.current|Count|Total|Recuento de sesiones activas de Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.active.max|Sí|tomcat.sessions.active.max|Count|Total|Recuento activo máximo de las sesiones de Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.alive.max|Sí|tomcat.sessions.alive.max|Milisegundos|Máxima|Tempo activo máximo de las sesiones de Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.created|Sí|tomcat.sessions.created|Count|Total|Recuento de sesiones creadas de Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.expired|Sí|tomcat.sessions.expired|Count|Total|Recuento de sesiones expiradas de Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.rejected|Sí|tomcat.sessions.rejected|Count|Total|Recuento de sesiones rechazadas de Tomcat|Deployment, AppName, Pod|
|tomcat.threads.config.max|Sí|tomcat.threads.config.max|Count|Total|Número máximo de subprocesos de configuración de Tomcat|Deployment, AppName, Pod|
|tomcat.threads.current|Sí|tomcat.threads.current|Count|Total|Número de subprocesos actuales de Tomcat|Deployment, AppName, Pod|
|total-requests|Sí|total-requests|Count|Average|Número total de solicitudes durante el proceso|Deployment, AppName, Pod|
|working-set|Sí|working-set|Count|Average|Cantidad de memoria del espacio de trabajo usada por el proceso (MB)|Deployment, AppName, Pod|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|TotalJob|Sí|Número total de trabajos|Count|Total|El número total de trabajos (jobs)|Runbook, Estado|
|TotalUpdateDeploymentMachineRuns|Sí|Ejecuciones de máquina de implementación de actualizaciones totales|Count|Total|Ejecuciones de máquina de implementación de actualizaciones de software totales en una ejecución de implementación de actualizaciones de software|SoftwareUpdateConfigurationName, Status, TargetComputer, SoftwareUpdateConfigurationRunId|
|TotalUpdateDeploymentRuns|Sí|Ejecuciones de implementación de actualizaciones totales|Count|Total|Ejecuciones de implementación de actualizaciones de software totales|SoftwareUpdateConfigurationName, Status|


## <a name="microsoftavsprivateclouds"></a>microsoft.avs/privateClouds

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|CapacityLatest|Sí|Capacidad total del disco de almacén de datos|Bytes|Average|La capacidad total del disco de almacén de datos|dsname|
|DiskUsedPercentage|Sí| Porcentaje de disco del almacén de datos empleado|Percent|Average|Porcentaje de disco disponible usado en el almacén de datos|dsname|
|EffectiveCpuAverage|Sí|Porcentaje de CPU|Percent|Average|Porcentaje de recursos de CPU usados en el clúster|clustername|
|EffectiveMemAverage|Sí|Promedio de memoria efectiva|Bytes|Average|Cantidad total de memoria de la máquina disponible en el clúster|clustername|
|OverheadAverage|Sí|Promedio de sobrecarga de la memoria|Bytes|Average|Memoria física de host utilizada por la infraestructura de virtualización|clustername|
|TotalMbAverage|Sí|Promedio de memoria total|Bytes|Average|Memoria total del clúster|clustername|
|UsageAverage|Sí|Promedio de uso de memoria|Percent|Average|Uso de memoria como porcentaje del total de memoria configurada o disponible|clustername|
|UsedLatest|Sí|Disco de almacén de datos empleado|Bytes|Average|La cantidad total de disco que se usa en el almacén de datos|dsname|


## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|CoreCount|No|Recuento de núcleos dedicados|Count|Total|Número total de núcleos dedicados de la cuenta de Batch|Sin dimensiones|
|CreatingNodeCount|No|Recuento de nodos creados|Count|Total|Número de nodos que se van a crear|Sin dimensiones|
|IdleNodeCount|No|Recuento de nodos inactivos|Count|Total|Número de nodos inactivos|Sin dimensiones|
|JobDeleteCompleteEvent|Sí|Eventos de finalización de eliminaciones de trabajo|Count|Total|Número total de trabajos que se han eliminado correctamente.|jobId|
|JobDeleteStartEvent|Sí|Eventos de inicio de eliminaciones de trabajo|Count|Total|Número total de trabajos cuya eliminación se ha solicitado.|jobId|
|JobDisableCompleteEvent|Sí|Eventos de finalización de deshabilitación de trabajo|Count|Total|Número total de trabajos que se han deshabilitado correctamente.|jobId|
|JobDisableStartEvent|Sí|Eventos de inicio de deshabilitación de trabajo|Count|Total|Número total de trabajos cuya deshabilitación se ha solicitado.|jobId|
|JobStartEvent|Sí|Eventos de inicio de trabajo|Count|Total|Número total de trabajos que se han iniciado correctamente.|jobId|
|JobTerminateCompleteEvent|Sí|Eventos de finalización de terminaciones de trabajo|Count|Total|Número total de trabajos que se han finalizado correctamente.|jobId|
|JobTerminateStartEvent|Sí|Eventos de inicio de terminaciones de trabajo|Count|Total|Número total de trabajos cuya terminación se ha solicitado.|jobId|
|LeavingPoolNodeCount|No|Recuento de nodos que salen del grupo|Count|Total|Número de nodos que abandonan el grupo|Sin dimensiones|
|LowPriorityCoreCount|No|Recuento de núcleos de baja prioridad|Count|Total|Número total de núcleos de baja prioridad de la cuenta de Batch|Sin dimensiones|
|OfflineNodeCount|No|Recuento de nodos sin conexión|Count|Total|Número de nodos sin conexión|Sin dimensiones|
|PoolCreateEvent|Sí|Eventos de creación de grupo|Count|Total|Número total de grupos que se han creado|poolId|
|PoolDeleteCompleteEvent|Sí|Eventos de finalización de eliminaciones de grupo|Count|Total|Número total de eliminaciones de grupo que se han completado|poolId|
|PoolDeleteStartEvent|Sí|Eventos de inicio de eliminación de grupo|Count|Total|Número total de eliminaciones de grupo que se han iniciado|poolId|
|PoolResizeCompleteEvent|Sí|Eventos de finalización de cambio de tamaño de grupo|Count|Total|Número total de cambios de tamaño de grupo completados|poolId|
|PoolResizeStartEvent|Sí|Eventos de inicio de cambio de tamaño de grupo|Count|Total|Número total de cambios de tamaño de grupo que se han iniciado|poolId|
|PreemptedNodeCount|No|Recuento de nodos con prioridad|Count|Total|Número de nodos con prioridad|Sin dimensiones|
|RebootingNodeCount|No|Recuento de nodos de reinicio|Count|Total|Número de nodos de reinicio|Sin dimensiones|
|ReimagingNodeCount|No|Recuento de nodos de restablecimiento de imagen inicial|Count|Total|Número de nodos de restablecimiento de imagen inicial|Sin dimensiones|
|RunningNodeCount|No|Recuento de nodos en ejecución|Count|Total|Número de nodos en ejecución|Sin dimensiones|
|StartingNodeCount|No|Recuento de nodos de inicio|Count|Total|Número de nodos que se van a iniciar|Sin dimensiones|
|StartTaskFailedNodeCount|No|Recuento de nodos con error de la tarea de inicio|Count|Total|Número de nodos con error en la tarea de inicio|Sin dimensiones|
|TaskCompleteEvent|Sí|Eventos de tarea completada|Count|Total|Número total de tareas que se han completado|poolId, jobId|
|TaskFailEvent|Sí|Eventos de error en tareas|Count|Total|Número total de tareas que se han completado con errores|poolId, jobId|
|TaskStartEvent|Sí|Eventos de inicio de tarea|Count|Total|Número total de tareas que se han iniciado|poolId, jobId|
|TotalLowPriorityNodeCount|No|Recuento de nodos de baja prioridad|Count|Total|Número total de nodos de baja prioridad de la cuenta de Batch|Sin dimensiones|
|TotalNodeCount|No|Recuento de nodos dedicados|Count|Total|Número total de nodos dedicados de la cuenta de Batch|Sin dimensiones|
|UnusableNodeCount|No|Recuento de nodos no utilizables|Count|Total|Número de nodos inutilizables|Sin dimensiones|
|WaitingForStartTaskNodeCount|No|Recuento de nodos esperando a la tarea de inicio|Count|Total|Número de nodos que esperan a que se complete la tarea de inicio|Sin dimensiones|

## <a name="microsoftbatchaiworkspaces"></a>Microsoft.BatchAI/workspaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Active Cores (Núcleos activos)|Sí|Active Cores (Núcleos activos)|Count|Average|Número de núcleos activos|Scenario, ClusterName|
|Active Nodes (Nodos activos)|Sí|Active Nodes (Nodos activos)|Count|Average|Número de nodos en ejecución|Scenario, ClusterName|
|Idle Cores (Núcleos inactivos)|Sí|Idle Cores (Núcleos inactivos)|Count|Average|Número de núcleos inactivos|Scenario, ClusterName|
|Idle Nodes (Nodos inactivos)|Sí|Idle Nodes (Nodos inactivos)|Count|Average|Número de nodos inactivos|Scenario, ClusterName|
|Job Completed (Trabajo completado)|Sí|Job Completed (Trabajo completado)|Count|Total|Trabajo completado|Scenario, ClusterName, ResultType|
|Job Submitted (Trabajo enviado)|Sí|Job Submitted (Trabajo enviado)|Count|Total|Número de trabajos enviados|Scenario, ClusterName|
|Leaving Cores (Núcleos de salida)|Sí|Leaving Cores (Núcleos de salida)|Count|Average|Número de núcleos de salida|Scenario, ClusterName|
|Leaving Nodes (Nodos de salida)|Sí|Leaving Nodes (Nodos de salida)|Count|Average|Número de nodos de salida|Scenario, ClusterName|
|Preempted Cores (Núcleos con prioridad)|Sí|Preempted Cores (Núcleos con prioridad)|Count|Average|Número de núcleos con prioridad|Scenario, ClusterName|
|Preempted Nodes (Nodos con prioridad)|Sí|Preempted Nodes (Nodos con prioridad)|Count|Average|Número de nodos con prioridad|Scenario, ClusterName|
|Porcentaje de uso de la cuota|Sí|Porcentaje de uso de la cuota|Count|Average|Porcentaje de cuota utilizada|Scenario, ClusterName, VmFamilyName, VmPriority|
|Total Cores (Núcleos totales)|Sí|Total Cores (Núcleos totales)|Count|Average|Número de núcleos totales|Scenario, ClusterName|
|Total Nodes|Sí|Total Nodes (Nodos totales)|Count|Average|Número de nodos totales|Scenario, ClusterName|
|Unusable Cores|Sí|Unusable Cores (Núcleos no utilizables)|Count|Average|Número de núcleos no utilizables|Scenario, ClusterName|
|Nodos no utilizables|Sí|Nodos no utilizables|Count|Average|Número de nodos inutilizables|Scenario, ClusterName|

## <a name="microsoftbingaccounts"></a>microsoft.bing/accounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BlockedCalls|Sí|Llamadas bloqueadas|Count|Total|Número de llamadas que han superado la tasa o el límite de cuota|ApiName, ServingRegion, StatusCode|
|ClientErrors|Sí|Errores de cliente|Count|Total|Número de llamadas con cualquier error de cliente (código de estado HTTP 4xx)|ApiName, ServingRegion, StatusCode|
|DataIn|Sí|Entrada de datos|Bytes|Total|Longitud del contenido de la solicitud entrante en bytes|ApiName, ServingRegion, StatusCode|
|DataOut|Sí|Salida de datos|Bytes|Total|Longitud del contenido de la respuesta saliente en bytes|ApiName, ServingRegion, StatusCode|
|Latencia|Sí|Latencia|Milisegundos|Average|Latencia en milisegundos|ApiName, ServingRegion, StatusCode|
|ServerErrors|Sí|Errores del servidor|Count|Total|Número de llamadas con cualquier error de servidor (código de estado HTTP 5xx)|ApiName, ServingRegion, StatusCode|
|SuccessfulCalls|Sí|Llamadas correctas|Count|Total|Número de llamadas correctas (código de estado HTTP 2xx)|ApiName, ServingRegion, StatusCode|
|TotalCalls|Sí|Total de llamadas|Count|Total|Número total de llamadas|ApiName, ServingRegion, StatusCode|
|TotalErrors|Sí|Total de errores|Count|Total|Número de llamadas con cualquier error (código de estado HTTP 4xx o 5xx)|ApiName, ServingRegion, StatusCode|


## <a name="microsoftblockchainblockchainmembers"></a>Microsoft.Blockchain/blockchainMembers

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BroadcastProcessedCount|Sí|BroadcastProcessedCountDisplayName|Count|Average|Número de transacciones procesadas.|Nodo, canal, tipo, estado|
|ChaincodeExecuteTimeouts|Sí|ChaincodeExecuteTimeoutsDisplayName|Count|Average|Número de ejecuciones de código de cadena (Init o Invoke) que han agotado el tiempo de espera.|Nodo, código de cadena|
|ChaincodeLaunchFailures|Sí|ChaincodeLaunchFailuresDisplayName|Count|Average|Número de inicios de código de cadena con errores.|Nodo, código de cadena|
|ChaincodeLaunchTimeouts|Sí|ChaincodeLaunchTimeoutsDisplayName|Count|Average|Número de inicios de código de cadena que han agotado el tiempo de espera.|Nodo, código de cadena|
|ChaincodeShimRequestsCompleted|Sí|ChaincodeShimRequestsCompletedDisplayName|Count|Average|El número de solicitudes de corrección de compatibilidad (shim) de código de cadena completadas.|Nodo, tipo, canal, código de cadena, éxito|
|ChaincodeShimRequestsReceived|Sí|ChaincodeShimRequestsReceivedDisplayName|Count|Average|El número de solicitudes de corrección de compatibilidad (shim) de código de cadena recibidas.|Nodo, tipo, canal, código de cadena|
|ClusterCommEgressQueueCapacity|Sí|ClusterCommEgressQueueCapacityDisplayName|Count|Average|Capacidad de la cola de salida.|Nodo, host, msg_type, canal|
|ClusterCommEgressQueueLength|Sí|ClusterCommEgressQueueLengthDisplayName|Count|Average|Longitud de la cola de salida.|Nodo, host, msg_type, canal|
|ClusterCommEgressQueueWorkers|Sí|ClusterCommEgressQueueWorkersDisplayName|Count|Average|Recuento de los trabajos de la cola de salida.|Nodo, canal|
|ClusterCommEgressStreamCount|Sí|ClusterCommEgressStreamCountDisplayName|Count|Average|Recuento de secuencias a otros nodos.|Nodo, canal|
|ClusterCommEgressTlsConnectionCount|Sí|ClusterCommEgressTlsConnectionCountDisplayName|Count|Average|Recuento de conexiones TLS a otros nodos.|Nodo|
|ClusterCommIngressStreamCount|Sí|ClusterCommIngressStreamCountDisplayName|Count|Average|Recuento de secuencias desde otros nodos.|Nodo|
|ClusterCommMsgDroppedCount|Sí|ClusterCommMsgDroppedCountDisplayName|Count|Average|Recuento de mensajes eliminados.|Nodo, host, canal|
|ConnectionAccepted|Sí|Accepted Connections (Conexiones aceptadas)|Count|Total|Conexiones aceptadas|Nodo|
|ConnectionActive|Sí|Conexiones activas|Count|Average|Conexiones activas|Nodo|
|ConnectionHandled|Sí|Handled Connections (Conexiones controladas)|Count|Total|Conexiones controladas|Nodo|
|ConsensusEtcdraftActiveNodes|Sí|ConsensusEtcdraftActiveNodesDisplayName|Count|Average|Número de nodos activos en este canal.|Nodo, canal|
|ConsensusEtcdraftClusterSize|Sí|ConsensusEtcdraftClusterSizeDisplayName|Count|Average|Número de nodos de este canal.|Nodo, canal|
|ConsensusEtcdraftCommittedBlockNumber|Sí|ConsensusEtcdraftCommittedBlockNumberDisplayName|Count|Average|Número de bloque del último bloque confirmado.|Nodo, canal|
|ConsensusEtcdraftConfigProposalsReceived|Sí|ConsensusEtcdraftConfigProposalsReceivedDisplayName|Count|Average|Número total de propuestas recibidas de transacciones de tipo de configuración.|Nodo, canal|
|ConsensusEtcdraftIsLeader|Sí|ConsensusEtcdraftIsLeaderDisplayName|Count|Average|El estado de liderazgo del nodo actual: 1 si es el líder, 0 en caso contrario.|Nodo, canal|
|ConsensusEtcdraftLeaderChanges|Sí|ConsensusEtcdraftLeaderChangesDisplayName|Count|Average|Número de cambios de líder desde el inicio del proceso.|Nodo, canal|
|ConsensusEtcdraftNormalProposalsReceived|Sí|ConsensusEtcdraftNormalProposalsReceivedDisplayName|Count|Average|Número total de propuestas recibidas de transacciones de tipo normal.|Nodo, canal|
|ConsensusEtcdraftProposalFailures|Sí|ConsensusEtcdraftProposalFailuresDisplayName|Count|Average|Número de errores en las propuestas.|Nodo, canal|
|ConsensusEtcdraftSnapshotBlockNumber|Sí|ConsensusEtcdraftSnapshotBlockNumberDisplayName|Count|Average|Número de bloque de la última instantánea.|Nodo, canal|
|ConsensusKafkaBatchSize|Sí|ConsensusKafkaBatchSizeDisplayName|Count|Average|Tamaño medio del lote, en bytes, que se envía a los temas.|Nodo, tema|
|ConsensusKafkaCompressionRatio|Sí|ConsensusKafkaCompressionRatioDisplayName|Count|Average|La proporción de compresión media (en porcentaje) para los temas.|Nodo, tema|
|ConsensusKafkaIncomingByteRate|Sí|ConsensusKafkaIncomingByteRateDisplayName|Count|Average|Bytes por segundo leídos de los agentes.|Nodo, broker_id|
|ConsensusKafkaLastOffsetPersisted|Sí|ConsensusKafkaLastOffsetPersistedDisplayName|Count|Average|Desplazamiento especificado en los metadatos de bloque del bloque confirmado más recientemente.|Nodo, canal|
|ConsensusKafkaOutgoingByteRate|Sí|ConsensusKafkaOutgoingByteRateDisplayName|Count|Average|Bytes por segundo escritos en los agentes.|Nodo, broker_id|
|ConsensusKafkaRecordSendRate|Sí|ConsensusKafkaRecordSendRateDisplayName|Count|Average|Número de registros por segundo enviados a los temas.|Nodo, tema|
|ConsensusKafkaRecordsPerRequest|Sí|ConsensusKafkaRecordsPerRequestDisplayName|Count|Average|El número medio de registros enviados por solicitud a los temas.|Nodo, tema|
|ConsensusKafkaRequestLatency|Sí|ConsensusKafkaRequestLatencyDisplayName|Count|Average|La latencia media en milisegundos de las solicitudes a los agentes.|Nodo, broker_id|
|ConsensusKafkaRequestRate|Sí|ConsensusKafkaRequestRateDisplayName|Count|Average|Solicitudes por segundo enviadas a los agentes.|Nodo, broker_id|
|ConsensusKafkaRequestSize|Sí|ConsensusKafkaRequestSizeDisplayName|Count|Average|Tamaño medio de la solicitud en bytes a los agentes.|Nodo, broker_id|
|ConsensusKafkaResponseRate|Sí|ConsensusKafkaResponseRateDisplayName|Count|Average|Solicitudes por segundo enviadas a los agentes.|Nodo, broker_id|
|ConsensusKafkaResponseSize|Sí|ConsensusKafkaResponseSizeDisplayName|Count|Average|Tamaño medio de respuesta en bytes de los agentes.|Nodo, broker_id|
|CpuUsagePercentageInDouble|Sí|CPU Usage Percentage (Porcentaje de uso de CPU)|Percent|Máxima|Porcentaje de uso de CPU|Nodo|
|DeliverBlocksSent|Sí|DeliverBlocksSentDisplayName|Count|Average|El número de bloques que envía el servicio de entrega.|Nodo, canal, filtrado, data_type|
|DeliverRequestsCompleted|Sí|DeliverRequestsCompletedDisplayName|Count|Average|Número de solicitudes de entrega que se han completado.|Nodo, canal, filtrado, data_type, éxito|
|DeliverRequestsReceived|Sí|DeliverRequestsReceivedDisplayName|Count|Average|Número de solicitudes de entrega que se han recibido.|Nodo, canal, filtrado, data_type|
|DeliverStreamsClosed|Sí|DeliverStreamsClosedDisplayName|Count|Average|El número de secuencias GRPC que se han cerrado para el servicio de entrega.|Nodo|
|DeliverStreamsOpened|Sí|DeliverStreamsOpenedDisplayName|Count|Average|El número de secuencias GRPC que se han abierto para el servicio de entrega.|Nodo|
|EndorserChaincodeInstantiationFailures|Sí|EndorserChaincodeInstantiationFailuresDisplayName|Count|Average|El número de creaciones de instancias de código de cadena o de actualizaciones con errores.|Nodo, canal, código de cadena|
|EndorserDuplicateTransactionFailures|Sí|EndorserDuplicateTransactionFailuresDisplayName|Count|Average|Número de propuestas con error debido a identificadores de transacción duplicados.|Nodo, canal, código de cadena|
|EndorserEndorsementFailures|Sí|EndorserEndorsementFailuresDisplayName|Count|Average|Número de aprobaciones con error.|Node, channel, chaincode, chaincodeerror|
|EndorserProposalAclFailures|Sí|EndorserProposalAclFailuresDisplayName|Count|Average|Número de propuestas con errores en las comprobaciones de ACL.|Nodo, canal, código de cadena|
|EndorserProposalSimulationFailures|Sí|EndorserProposalSimulationFailuresDisplayName|Count|Average|Número de simulaciones de propuestas con error.|Nodo, canal, código de cadena|
|EndorserProposalsReceived|Sí|EndorserProposalsReceivedDisplayName|Count|Average|Número de propuestas recibidas.|Nodo|
|EndorserProposalValidationFailures|Sí|EndorserProposalValidationFailuresDisplayName|Count|Average|Número de propuestas en las que se ha producido un error en la validación inicial.|Nodo|
|EndorserSuccessfulProposals|Sí|EndorserSuccessfulProposalsDisplayName|Count|Average|El número de propuestas completadas correctamente.|Nodo|
|FabricVersion|Sí|FabricVersionDisplayName|Count|Average|Versión activa de Fabric.|Node, versión|
|GossipCommMessagesReceived|Sí|GossipCommMessagesReceivedDisplayName|Count|Average|Número de mensajes recibidos.|Nodo|
|GossipCommMessagesSent|Sí|GossipCommMessagesSentDisplayName|Count|Average|Número de mensajes enviados.|Nodo|
|GossipCommOverflowCount|Sí|GossipCommOverflowCountDisplayName|Count|Average|Número de desbordamientos del búfer de la cola de salida.|Nodo|
|GossipLeaderElectionLeader|Sí|GossipLeaderElectionLeaderDisplayName|Count|Average|El par es el responsable (1) o el seguidor (0).|Nodo, canal|
|GossipMembershipTotalPeersKnown|Sí|GossipMembershipTotalPeersKnownDisplayName|Count|Average|Pares totales conocidos.|Nodo, canal|
|GossipPayloadBufferSize|Sí|GossipPayloadBufferSizeDisplayName|Count|Average|Tamaño del búfer de carga.|Nodo, canal|
|GossipStateHeight|Sí|GossipStateHeightDisplayName|Count|Average|Alto de libro de contabilidad actual.|Nodo, canal|
|GrpcCommConnClosed|Sí|GrpcCommConnClosedDisplayName|Count|Average|Conexiones cerradas de gRPC. El número de conexiones abiertas menos el de cerradas es el número activo de conexiones.|Nodo|
|GrpcCommConnOpened|Sí|GrpcCommConnOpenedDisplayName|Count|Average|Conexiones abiertas de gRPC. El número de conexiones abiertas menos el de cerradas es el número activo de conexiones.|Nodo|
|GrpcServerStreamMessagesReceived|Sí|GrpcServerStreamMessagesReceivedDisplayName|Count|Average|Número de mensajes de secuencia recibidos.|Nodo, servicio, método|
|GrpcServerStreamMessagesSent|Sí|GrpcServerStreamMessagesSentDisplayName|Count|Average|Número de mensajes de secuencia enviados.|Nodo, servicio, método|
|GrpcServerStreamRequestsCompleted|Sí|GrpcServerStreamRequestsCompletedDisplayName|Count|Average|Número de solicitudes de secuencia completadas.|Nodo, servicio, método, código|
|GrpcServerUnaryRequestsReceived|Sí|GrpcServerUnaryRequestsReceivedDisplayName|Count|Average|Número de solicitudes unarias recibidas.|Nodo, servicio, método|
|IOReadBytes|Sí|IO Read Bytes (Bytes de lectura de E/S)|Bytes|Total|Bytes de lectura de E/S|Nodo|
|IOWriteBytes|Sí|IO Write Bytes (Bytes de escritura de E/S)|Bytes|Total|Bytes de escritura de E/S|Nodo|
|LedgerBlockchainHeight|Sí|LedgerBlockchainHeightDisplayName|Count|Average|Altura de la cadena en bloques.|Nodo, canal|
|LedgerTransactionCount|Sí|LedgerTransactionCountDisplayName|Count|Average|Número de transacciones procesadas.|Node, channel, transaction_type, chaincode, validation_code|
|LoggingEntriesChecked|Sí|LoggingEntriesCheckedDisplayName|Count|Average|Número de entradas de registro comprobadas en comparación con el nivel de registro activo.|Nodo, nivel|
|LoggingEntriesWritten|Sí|LoggingEntriesWrittenDisplayName|Count|Average|Número de entradas de registro que se escriben.|Nodo, nivel|
|MemoryLimit|Sí|Memory Limit (Límite de memoria)|Bytes|Average|Límite de memoria|Nodo|
|MemoryUsage|Sí|Uso de la memoria|Bytes|Average|Uso de la memoria|Nodo|
|MemoryUsagePercentageInDouble|Sí|Memory Usage Percentage (Porcentaje de uso de memoria)|Percent|Average|Porcentaje de uso de memoria|Nodo|
|PendingTransactions|Sí|Pending Transactions (Transacciones pendientes)|Count|Average|Transacciones pendientes|Nodo|
|ProcessedBlocks|Sí|Processed Blocks (Bloques procesados)|Count|Total|Bloques procesados|Nodo|
|ProcessedTransactions|Sí|Processed Transactions (Transacciones procesadas)|Count|Total|Transacciones procesadas|Nodo|
|QueuedTransactions|Sí|Queued Transactions (Transacciones en cola)|Count|Average|Transacciones en cola|Nodo|
|RequestHandled|Sí|Handled Requests (Solicitudes controladas)|Count|Total|Solicitudes controladas|Nodo|
|StorageUsage|Sí|Storage Usage (Uso del almacenamiento)|Bytes|Average|Uso del almacenamiento|Nodo|


## <a name="microsoftbotservicebotservices"></a>microsoft.botservice/botservices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|RequestLatency|Sí|Latencia de las solicitudes|Milisegundos|Total|Tiempo que tarda el servidor en procesar la solicitud|Operación, autenticación, protocolo, centro de datos|
|RequestsTraffic|Sí|Tráfico de solicitudes|Percent|Count|Número de solicitudes realizadas|Operación, autenticación, protocolo, StatusCode, StatusCodeClass, centro de datos|


## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|allcachehits|Sí|Aciertos de caché (basada en instancias)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allcachemisses|Sí|Errores de caché (basada en instancias)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allcacheRead|Sí|Lectura de caché (basada en instancias)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allcacheWrite|Sí|Escritura de caché (basada en instancias)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allconnectedclients|Sí|Clientes conectados (basada en instancias)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allevictedkeys|Sí|Claves expulsadas (basada en instancias)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allexpiredkeys|Sí|Claves expiradas (basada en instancias)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allgetcommands|Sí|Comandos get (basada en instancias)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|alloperationsPerSecond|Sí|Operaciones por segundo (basada en instancias)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allpercentprocessortime|Sí|CPU (basada en instancia)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allserverLoad|Sí|Carga del servidor (basada en instancias)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allsetcommands|Sí|Comandos set (basada en instancias)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|alltotalcommandsprocessed|Sí|Total de operaciones (basada en instancias)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|alltotalkeys|Sí|Claves totales (basada en instancias)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allusedmemory|Sí|Memoria usada (basada en instancias)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allusedmemorypercentage|Sí|Porcentaje de memoria usada (basada en instancias)|Percent|Máxima|El porcentaje de memoria caché usada para pares clave-valor. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allusedmemoryRss|Sí|RSS de memoria usada (basada en instancias)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|cachehits|Sí|Aciertos de caché|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|cachehits0|Sí|Aciertos de caché (partición 0)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachehits1|Sí|Aciertos de caché (partición 1)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachehits2|Sí|Aciertos de caché (partición 2)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachehits3|Sí|Aciertos de caché (partición 3)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachehits4|Sí|Aciertos de caché (partición 4)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachehits5|Sí|Aciertos de caché (partición 5)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachehits6|Sí|Aciertos de caché (partición 6)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachehits7|Sí|Aciertos de caché (partición 7)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachehits8|Sí|Aciertos de caché (partición 8)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachehits9|Sí|Aciertos de caché (partición 9)|Count|Total|El número de búsquedas de claves correctas. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheLatency|Sí|Microsegundos de latencia de la memoria caché (versión preliminar)|Count|Average|La latencia de la caché en microsegundos. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|cachemisses|Sí|Errores de caché|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|cachemisses0|Sí|Errores de caché (partición 0)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemisses1|Sí|Errores de caché (partición 1)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemisses2|Sí|Errores de caché (partición 2)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemisses3|Sí|Errores de caché (partición 3)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemisses4|Sí|Errores de caché (partición 4)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemisses5|Sí|Errores de caché (partición 5)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemisses6|Sí|Errores de caché (partición 6)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemisses7|Sí|Errores de caché (partición 7)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemisses8|Sí|Errores de caché (partición 8)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemisses9|Sí|Errores de caché (partición 9)|Count|Total|El número de búsquedas de claves con error. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cachemissrate|Sí|Tasa de errores de caché|Percent|Total|El porcentaje de solicitudes Get que no se ejecutan. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|cacheRead|Sí|Lectura de caché|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|cacheRead0|Sí|Lectura de caché (partición 0)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheRead1|Sí|Lectura de caché (partición 1)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheRead2|Sí|Lectura de caché (partición 2)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheRead3|Sí|Lectura de caché (partición 3)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheRead4|Sí|Lectura de caché (partición 4)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheRead5|Sí|Lectura de caché (partición 5)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheRead6|Sí|Lectura de caché (partición 6)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheRead7|Sí|Lectura de caché (partición 7)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheRead8|Sí|Lectura de caché (partición 8)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheRead9|Sí|Lectura de caché (partición 9)|BytesPerSecond|Máxima|La cantidad de datos que se leen de la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite|Sí|Escritura de caché|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|cacheWrite0|Sí|Escritura de caché (partición 0)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite1|Sí|Escritura de caché (partición 1)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite2|Sí|Escritura de caché (partición 2)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite3|Sí|Escritura de caché (partición 3)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite4|Sí|Escritura de caché (partición 4)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite5|Sí|Escritura de caché (partición 5)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite6|Sí|Escritura de caché (partición 6)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite7|Sí|Escritura de caché (partición 7)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite8|Sí|Escritura de caché (partición 8)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|cacheWrite9|Sí|Escritura de caché (partición 9)|BytesPerSecond|Máxima|La cantidad de datos que se escriben en la caché en megabytes por segundo (MB/s). Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients|Sí|Clientes conectados|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|connectedclients0|Sí|Clientes conectados (partición 0)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients1|Sí|Clientes conectados (partición 1)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients2|Sí|Clientes conectados (partición 2)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients3|Sí|Clientes conectados (partición 3)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients4|Sí|Clientes conectados (partición 4)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients5|Sí|Clientes conectados (partición 5)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients6|Sí|Clientes conectados (partición 6)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients7|Sí|Clientes conectados (partición 7)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients8|Sí|Clientes conectados (partición 8)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|connectedclients9|Sí|Clientes conectados (partición 9)|Count|Máxima|El número de conexiones de cliente a la memoria caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|errors|Sí|Errors|Count|Máxima|El número de errores que se produjeron en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId, ErrorType|
|evictedkeys|Sí|Claves expulsadas|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|evictedkeys0|Sí|Claves expulsadas (partición 0)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|evictedkeys1|Sí|Claves expulsadas (partición 1)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|evictedkeys2|Sí|Claves expulsadas (partición 2)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|evictedkeys3|Sí|Claves expulsadas (partición 3)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|evictedkeys4|Sí|Claves expulsadas (partición 4)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|evictedkeys5|Sí|Claves expulsadas (partición 5)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|evictedkeys6|Sí|Claves expulsadas (partición 6)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|evictedkeys7|Sí|Claves expulsadas (partición 7)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|evictedkeys8|Sí|Claves expulsadas (partición 8)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|evictedkeys9|Sí|Claves expulsadas (partición 9)|Count|Total|El número de elementos expulsados de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys|Sí|Claves expiradas|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|expiredkeys0|Sí|Claves expiradas (partición 0)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys1|Sí|Claves expiradas (partición 1)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys2|Sí|Claves expiradas (partición 2)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys3|Sí|Claves expiradas (partición 3)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys4|Sí|Claves expiradas (partición 4)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys5|Sí|Claves expiradas (partición 5)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys6|Sí|Claves expiradas (partición 6)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys7|Sí|Claves expiradas (partición 7)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys8|Sí|Claves expiradas (partición 8)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|expiredkeys9|Sí|Claves expiradas (partición 9)|Count|Total|El número de elementos expirados en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands|Sí|Gets|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|getcommands0|Sí|Obtenciones (partición 0)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands1|Sí|Obtenciones (partición 1)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands2|Sí|Obtenciones (partición 2)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands3|Sí|Obtenciones (partición 3)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands4|Sí|Obtenciones (partición 4)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands5|Sí|Obtenciones (partición 5)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands6|Sí|Obtenciones (partición 6)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands7|Sí|Obtenciones (partición 7)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands8|Sí|Obtenciones (partición 8)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|getcommands9|Sí|Obtenciones (partición 9)|Count|Total|El número de operaciones Get de la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond|Sí|Operaciones por segundo|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|operationsPerSecond0|Sí|Operaciones por segundo (partición 0)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond1|Sí|Operaciones por segundo (partición 1)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond2|Sí|Operaciones por segundo (partición 2)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond3|Sí|Operaciones por segundo (partición 3)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond4|Sí|Operaciones por segundo (partición 4)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond5|Sí|Operaciones por segundo (partición 5)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond6|Sí|Operaciones por segundo (partición 6)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond7|Sí|Operaciones por segundo (partición 7)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond8|Sí|Operaciones por segundo (partición 8)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|operationsPerSecond9|Sí|Operaciones por segundo (partición 9)|Count|Máxima|El número de operaciones instantáneas por segundo ejecutadas en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime|Sí|CPU|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|percentProcessorTime0|Sí|CPU (partición 0)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime1|Sí|CPU (partición 1)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime2|Sí|CPU (partición 2)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime3|Sí|CPU (partición 3)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime4|Sí|CPU (partición 4)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime5|Sí|CPU (partición 5)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime6|Sí|CPU (partición 6)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime7|Sí|CPU (partición 7)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime8|Sí|CPU (partición 8)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|percentProcessorTime9|Sí|CPU (partición 9)|Percent|Máxima|El uso de CPU del servidor de Azure Cache for Redis como porcentaje. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad|Sí|Carga de servidor|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|serverLoad0|Sí|Carga de servidor (partición 0)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad1|Sí|Carga de servidor (partición 1)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad2|Sí|Carga de servidor (partición 2)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad3|Sí|Carga de servidor (partición 3)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad4|Sí|Carga de servidor (partición 4)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad5|Sí|Carga de servidor (partición 5)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad6|Sí|Carga de servidor (partición 6)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad7|Sí|Carga de servidor (partición 7)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad8|Sí|Carga de servidor (partición 8)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|serverLoad9|Sí|Carga de servidor (partición 9)|Percent|Máxima|El porcentaje de ciclos en el que el servidor de Redis está ocupado procesando y no esperando a los mensajes inactivo. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands|Sí|Conjuntos|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|setcommands0|Sí|Definiciones (partición 0)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands1|Sí|Definiciones (partición 1)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands2|Sí|Definiciones (partición 2)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands3|Sí|Definiciones (partición 3)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands4|Sí|Definiciones (partición 4)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands5|Sí|Definiciones (partición 5)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands6|Sí|Definiciones (partición 6)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands7|Sí|Definiciones (partición 7)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands8|Sí|Definiciones (partición 8)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|setcommands9|Sí|Definiciones (partición 9)|Count|Total|El número de operaciones Set a la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed|Sí|Total de operaciones|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|totalcommandsprocessed0|Sí|Total de operaciones (partición 0)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed1|Sí|Total de operaciones (partición 1)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed2|Sí|Total de operaciones (partición 2)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed3|Sí|Total de operaciones (partición 3)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed4|Sí|Total de operaciones (partición 4)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed5|Sí|Total de operaciones (partición 5)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed6|Sí|Total de operaciones (partición 6)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed7|Sí|Total de operaciones (partición 7)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed8|Sí|Total de operaciones (partición 8)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalcommandsprocessed9|Sí|Total de operaciones (partición 9)|Count|Total|El número total de comandos procesados por el servidor de caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys|Sí|Total de claves|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|totalkeys0|Sí|Total de claves (partición 0)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys1|Sí|Total de claves (partición 1)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys2|Sí|Total de claves (partición 2)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys3|Sí|Total de claves (partición 3)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys4|Sí|Total de claves (partición 4)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys5|Sí|Total de claves (partición 5)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys6|Sí|Total de claves (partición 6)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys7|Sí|Total de claves (partición 7)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys8|Sí|Total de claves (partición 8)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|totalkeys9|Sí|Total de claves (partición 9)|Count|Máxima|El número total de elementos en la caché. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory|Sí|Memoria usada|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|usedmemory0|Sí|Memoria usada (partición 0)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory1|Sí|Memoria usada (partición 1)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory2|Sí|Memoria usada (partición 2)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory3|Sí|Memoria usada (partición 3)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory4|Sí|Memoria usada (partición 4)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory5|Sí|Memoria usada (partición 5)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory6|Sí|Memoria usada (partición 6)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory7|Sí|Memoria usada (partición 7)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory8|Sí|Memoria usada (partición 8)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemory9|Sí|Memoria usada (partición 9)|Bytes|Máxima|La cantidad de memoria caché usada para pares clave-valor en la caché en MB. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemorypercentage|Sí|Porcentaje de memoria usado|Percent|Máxima|El porcentaje de memoria caché usada para pares clave-valor. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|usedmemoryRss|Sí|Memoria RSS usada|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|ShardId|
|usedmemoryRss0|Sí|Memoria RSS usada (partición 0)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemoryRss1|Sí|Memoria RSS usada (partición 1)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemoryRss2|Sí|Memoria RSS usada (partición 2)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemoryRss3|Sí|Memoria RSS usada (partición 3)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemoryRss4|Sí|Memoria RSS usada (partición 4)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemoryRss5|Sí|Memoria RSS usada (partición 5)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemoryRss6|Sí|Memoria RSS usada (partición 6)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemoryRss7|Sí|Memoria RSS usada (partición 7)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemoryRss8|Sí|Memoria RSS usada (partición 8)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|
|usedmemoryRss9|Sí|Memoria RSS usada (partición 9)|Bytes|Máxima|La cantidad de memoria caché usada en MB, incluida la fragmentación y los metadatos. Para obtener más información, vea https://aka.ms/redis/metrics.|Sin dimensiones|


## <a name="microsoftcacheredisenterprise"></a>Microsoft.Cache/redisEnterprise

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|cachehits|Sí|Aciertos de caché|Count|Total||Sin dimensiones|
|cacheLatency|Sí|Microsegundos de latencia de la memoria caché (versión preliminar)|Count|Average||InstanceId|
|cachemisses|Sí|Errores de caché|Count|Total||InstanceId|
|cacheRead|Sí|Lectura de caché|BytesPerSecond|Máxima||InstanceId|
|cacheWrite|Sí|Escritura de caché|BytesPerSecond|Máxima||InstanceId|
|connectedclients|Sí|Clientes conectados|Count|Máxima||InstanceId|
|errors|Sí|Errors|Count|Máxima||InstanceId, ErrorType|
|evictedkeys|Sí|Claves expulsadas|Count|Total||Sin dimensiones|
|expiredkeys|Sí|Claves expiradas|Count|Total||Sin dimensiones|
|getcommands|Sí|Gets|Count|Total||Sin dimensiones|
|operationsPerSecond|Sí|Operaciones por segundo|Count|Máxima||Sin dimensiones|
|percentProcessorTime|Sí|CPU|Percent|Máxima||InstanceId|
|serverLoad|Sí|Carga de servidor|Percent|Máxima||Sin dimensiones|
|setcommands|Sí|Conjuntos|Count|Total||Sin dimensiones|
|totalcommandsprocessed|Sí|Total de operaciones|Count|Total||Sin dimensiones|
|totalkeys|Sí|Total de claves|Count|Máxima||Sin dimensiones|
|usedmemory|Sí|Memoria usada|Bytes|Máxima||Sin dimensiones|
|usedmemorypercentage|Sí|Porcentaje de memoria usado|Percent|Máxima||InstanceId|


## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft.Cdn/cdnwebapplicationfirewallpolicies

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|WebApplicationFirewallRequestCount|Sí|Recuento de solicitudes del firewall de aplicaciones web|Count|Total|Número de solicitudes de cliente procesadas por el firewall de aplicaciones web|PolicyName, RuleName, Action|


## <a name="microsoftcdnprofiles"></a>Microsoft.Cdn/profiles

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ByteHitRatio|Sí|Proporción de aciertos de bytes|Percent|Average|Esta es la proporción del total de bytes enviados desde la memoria caché en comparación con el total de bytes de respuesta|Punto de conexión|
|OriginHealthPercentage|Sí|Porcentaje de estado origen|Percent|Average|El porcentaje de sondeos de estado correctos de AFDX a los servidores back-ends.|Origen, OriginGroup|
|OriginLatency|Sí|Latencia de origen|MilliSeconds|Average|Tiempo calculado desde que el perímetro de AFDX envía la solicitud al servidor back-end hasta que AFDX recibe el último byte de respuesta del servidor back-end.|Origen, punto de conexión|
|OriginRequestCount|Sí|Recuento de solicitudes de origen|Count|Total|Número de solicitudes enviadas desde AFDX al origen.|HttpStatus, HttpStatusGroup, origen, punto de conexión|
|Percentage4XX|Sí|Porcentaje de 4XX|Percent|Average|El porcentaje de todas las solicitudes de cliente para las que el código de estado de respuesta es 4XX|Punto de conexión, ClientRegion, ClientCountry|
|Percentage5XX|Sí|Porcentaje de 5XX|Percent|Average|El porcentaje de todas las solicitudes de cliente para las que el código de estado de respuesta es 5XX|Punto de conexión, ClientRegion, ClientCountry|
|RequestCount|Sí|Recuento de solicitudes|Count|Total|Número de solicitudes de cliente que atiende el proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry, punto de conexión|
|RequestSize|Sí|Tamaño de la solicitud|Bytes|Total|Número de bytes enviados como solicitudes de clientes a AFDX.|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry, punto de conexión|
|ResponseSize|Sí|Tamaño de la respuesta|Bytes|Total|Número de bytes enviados como respuestas del proxy HTTP/S a los clientes|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry, punto de conexión|
|TotalLatency|Sí|Latencia total|MilliSeconds|Average|Tiempo calculado desde que el proxy HTTP/S ha recibido la solicitud de cliente hasta que el cliente ha reconocido el último byte de respuesta del proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry, punto de conexión|
|WebApplicationFirewallRequestCount|Sí|Recuento de solicitudes del firewall de aplicaciones web|Count|Total|Número de solicitudes de cliente procesadas por el firewall de aplicaciones web|PolicyName, RuleName, Action|


## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disk Read Bytes/Sec|No|Lectura de disco|BytesPerSecond|Average|Promedio de bytes que se leen desde el disco durante el período de supervisión.|RoleInstanceId|
|Operaciones de lectura de disco por segundo|Sí|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco.|RoleInstanceId|
|Disk Write Bytes/Sec|No|Escritura de disco|BytesPerSecond|Average|Promedio de bytes que se escriben en el disco durante el período de supervisión.|RoleInstanceId|
|Operaciones de escritura por segundo en disco|Sí|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco.|RoleInstanceId|
|Red interna|Sí|Red interna|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante).|RoleInstanceId|
|Red interna|Sí|Red interna|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente).|RoleInstanceId|
|Porcentaje de CPU|Sí|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente.|RoleInstanceId|


## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disk Read Bytes/Sec|No|Lectura de disco|BytesPerSecond|Average|Promedio de bytes que se leen desde el disco durante el período de supervisión.|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Sí|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco.|Sin dimensiones|
|Disk Write Bytes/Sec|No|Escritura de disco|BytesPerSecond|Average|Promedio de bytes que se escriben en el disco durante el período de supervisión.|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Sí|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco.|Sin dimensiones|
|Red interna|Sí|Red interna|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante).|Sin dimensiones|
|Red interna|Sí|Red interna|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente).|Sin dimensiones|
|Porcentaje de CPU|Sí|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente.|Sin dimensiones|


## <a name="microsoftclassicstoragestorageaccounts"></a>Microsoft.ClassicStorage/storageAccounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Sí|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|UsedCapacity|Sí|Capacidad usada|Bytes|Average|Capacidad usada de la cuenta|Sin dimensiones|


## <a name="microsoftclassicstoragestorageaccountsblobservices"></a>Microsoft.ClassicStorage/storageAccounts/blobServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|
|BlobCapacity|No|Capacidad de Blob|Bytes|Average|Cantidad de almacenamiento que ha usado la instancia de Blob service de la cuenta de almacenamiento, en bytes.|BlobType, Tier|
|BlobCount|No|Recuento de blobs|Count|Average|Número de blobs en la instancia de Blob service de la cuenta de almacenamiento.|BlobType, Tier|
|ContainerCount|Sí|Recuento de contenedores de blobs|Count|Average|Número de contenedores en la instancia de Blob service de la cuenta de almacenamiento.|Sin dimensiones|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|IndexCapacity|No|Capacidad de índice|Bytes|Average|Cantidad de almacenamiento que usa el índice (jerárquico) de ADLS Gen2 en bytes.|Sin dimensiones|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Sí|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftclassicstoragestorageaccountsfileservices"></a>Microsoft.ClassicStorage/storageAccounts/fileServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication, FileShare|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication, FileShare|
|FileCapacity|No|Capacidad de File|Bytes|Average|Cantidad de almacenamiento que ha utilizado el servicio File de la cuenta de almacenamiento, en bytes.|FileShare|
|FileCount|No|Recuento de archivos|Count|Average|Número de archivos en el servicio File de la cuenta de almacenamiento.|FileShare|
|FileShareCount|No|Recuento de recursos compartidos de archivos|Count|Average|Número de recursos compartidos de archivos en el servicio File de la cuenta de almacenamiento.|Sin dimensiones|
|FileShareQuota|No|File share quota size (Tamaño de cuota del recurso compartido de archivos)|Bytes|Average|Límite superior de la cantidad de almacenamiento que puede usar el servicio Azure Files en bytes.|FileShare|
|FileShareSnapshotCount|No|File Share Snapshot Count (Recuento de instantáneas del recurso compartido de archivos)|Count|Average|Número de instantáneas presentes en el recurso compartido en el servicio Files de la cuenta de almacenamiento.|FileShare|
|FileShareSnapshotSize|No|File Share Snapshot Size (Tamaño de instantánea del recurso compartido de archivos)|Bytes|Average|Cantidad de almacenamiento que usan las instantáneas del servicio Files de la cuenta de almacenamiento en bytes.|FileShare|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication, FileShare|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication, FileShare|
|SuccessServerLatency|Sí|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication, FileShare|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication, FileShare|


## <a name="microsoftclassicstoragestorageaccountsqueueservices"></a>Microsoft.ClassicStorage/storageAccounts/queueServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|QueueCapacity|Sí|Capacidad de Queue|Bytes|Average|Cantidad de almacenamiento que ha utilizado la instancia de Queue service de la cuenta de almacenamiento, en bytes.|Sin dimensiones|
|QueueCount|Sí|Recuento de colas|Count|Average|Número de colas en la instancia de Queue service de la cuenta de almacenamiento.|Sin dimensiones|
|QueueMessageCount|Sí|Recuento de mensajes de Queue|Count|Average|Número aproximado de mensajes de cola en la instancia de Queue service de la cuenta de almacenamiento.|Sin dimensiones|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Sí|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftclassicstoragestorageaccountstableservices"></a>Microsoft.ClassicStorage/storageAccounts/tableServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida, en bytes. Este número incluye la salida de un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|Milisegundos|Average|Latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Sí|Latencia del servidor correcta|Milisegundos|Average|Latencia utilizada por Azure Storage para procesar una solicitud correcta, en milisegundos. Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication|
|TableCapacity|Sí|Capacidad de Table|Bytes|Average|Cantidad de almacenamiento que ha utilizado la instancia de Table service de la cuenta de almacenamiento, en bytes.|Sin dimensiones|
|TableCount|Sí|Recuento de tablas|Count|Average|Número de tablas en la instancia de Table service de la cuenta de almacenamiento.|Sin dimensiones|
|TableEntityCount|Sí|Recuento de entidades de Table|Count|Average|Número de entidades de tabla en la instancia de Table service de la cuenta de almacenamiento.|Sin dimensiones|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftcloudtesthostedpools"></a>Microsoft.Cloudtest/hostedpools

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Allocated|Sí|Allocated|Count|Average|Recursos asignados|PoolId, SKU, Images, ProviderName|
|AllocationDurationMs|Sí|AllocationDurationMs|Milisegundos|Average|Tiempo medio para asignar solicitudes (ms)|PoolId, Type, ResourceRequestType, Image|
|Count|Sí|Count|Count|Count|Número de solicitudes en el último volcado|RequestType, Status, PoolId, Type, ErrorCode, FailureStage|
|NotReady|Sí|NotReady|Count|Average|Recursos que no están listos para usarse|PoolId, SKU, Images, ProviderName|
|PendingReimage|Sí|PendingReimage|Count|Average|Recursos pendientes de restablecer la imagen inicial|PoolId, SKU, Images, ProviderName|
|PendingReturn|Sí|PendingReturn|Count|Average|Recursos pendientes de devolución|PoolId, SKU, Images, ProviderName|
|aprovisionado|Sí|aprovisionado|Count|Average|Recursos aprovisionados|PoolId, SKU, Images, ProviderName|
|Ready|Sí|Ready|Count|Average|Recursos que están listos para usarse|PoolId, SKU, Images, ProviderName|
|Iniciando|Sí|Iniciando|Count|Average|Recursos que se inician|PoolId, SKU, Images, ProviderName|
|Total|Sí|Total|Count|Average|Número total de recursos|PoolId, SKU, Images, ProviderName|


## <a name="microsoftcloudtestpools"></a>Microsoft.Cloudtest/pools

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Allocated|Sí|Allocated|Count|Average|Recursos asignados|PoolId, SKU, Images, ProviderName|
|AllocationDurationMs|Sí|AllocationDurationMs|Milisegundos|Average|Tiempo medio para asignar solicitudes (ms)|PoolId, Type, ResourceRequestType, Image|
|Count|Sí|Count|Count|Count|Número de solicitudes en el último volcado|RequestType, Status, PoolId, Type, ErrorCode, FailureStage|
|NotReady|Sí|NotReady|Count|Average|Recursos que no están listos para usarse|PoolId, SKU, Images, ProviderName|
|PendingReimage|Sí|PendingReimage|Count|Average|Recursos pendientes de restablecer la imagen inicial|PoolId, SKU, Images, ProviderName|
|PendingReturn|Sí|PendingReturn|Count|Average|Recursos pendientes de devolución|PoolId, SKU, Images, ProviderName|
|aprovisionado|Sí|aprovisionado|Count|Average|Recursos aprovisionados|PoolId, SKU, Images, ProviderName|
|Ready|Sí|Ready|Count|Average|Recursos que están listos para usarse|PoolId, SKU, Images, ProviderName|
|Iniciando|Sí|Iniciando|Count|Average|Recursos que se inician|PoolId, SKU, Images, ProviderName|
|Total|Sí|Total|Count|Average|Número total de recursos|PoolId, SKU, Images, ProviderName|


## <a name="microsoftclusterstornodes"></a>Microsoft.ClusterStor/nodes

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|TotalCapacityAvailable|No|TotalCapacityAvailable|Bytes|Average|La capacidad total disponible en el sistema de archivos de Lustre|filesystem_name, category, system|
|TotalCapacityUsed|No|TotalCapacityUsed|Bytes|Average|La capacidad total usada en el sistema de archivos de Lustre|filesystem_name, category, system|
|TotalRead|No|TotalRead|BytesPerSecond|Average|Total de lectura del sistema de archivos de Lustre por segundo|filesystem_name, category, system|
|TotalWrite|No|TotalWrite|BytesPerSecond|Average|Total de escritura del sistema de archivos de Lustre por segundo|filesystem_name, category, system|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AudioSecondsTranscribed|Sí|Audio Seconds Transcribed (Segundos de audio transcritos)|Count|Total|Número de segundos transcritos.|ApiName, FeatureName, UsageChannel, Region|
|AudioSecondsTranslated|Sí|Audio Seconds Translated (Segundos de audio traducidos)|Count|Total|Número de segundos traducidos.|ApiName, FeatureName, UsageChannel, Region|
|BlockedCalls|Sí|Llamadas bloqueadas|Count|Total|Número de llamadas que han superado la tasa o el límite de cuota.|ApiName, OperationName, Region|
|CharactersTrained|Sí|Caracteres entrenados (en desuso)|Count|Total|Número total de caracteres entrenados.|ApiName, OperationName, Region|
|CharactersTranslated|Sí|Caracteres traducidos (en desuso)|Count|Total|Número total de caracteres de la solicitud entrante de texto.|ApiName, OperationName, Region|
|ClientErrors|Sí|Errores de cliente|Count|Total|Número de llamadas con error interno del lado cliente (código de respuesta HTTP 4xx).|ApiName, OperationName, Region|
|ComputerVisionTransactions|Sí|Computer Vision Transactions (Transacciones de Computer Vision)|Count|Total|Número de transacciones de Computer Vision.|ApiName, FeatureName, UsageChannel, Region|
|CustomVisionTrainingTime|Sí|Custom Vision Training Time (Tiempo de entrenamiento de Custom Vision)|Segundos|Total|Tiempo de entrenamiento de Custom Vision.|ApiName, FeatureName, UsageChannel, Region|
|CustomVisionTransactions|Sí|Custom Vision Transactions (Transacciones de Custom Vision)|Count|Total|Número de transacciones de predicción de Custom Vision.|ApiName, FeatureName, UsageChannel, Region|
|DataIn|Sí|Entrada de datos|Bytes|Total|Tamaño de los datos de entrada en bytes.|ApiName, OperationName, Region|
|DataOut|Sí|Salida de datos|Bytes|Total|Tamaño de los datos de salida en bytes.|ApiName, OperationName, Region|
|DocumentCharactersTranslated|Sí|Document Characters Translated (Caracteres de documento traducidos)|Count|Total|Número de caracteres en la solicitud de traducción de documentos.|ApiName, FeatureName, UsageChannel, Region|
|DocumentCustomCharactersTranslated|Sí|Document Custom Characters Translated (Caracteres personalizados de documento traducidos)|Count|Total|Número de caracteres en la solicitud de traducción de documentos personalizada.|ApiName, FeatureName, UsageChannel, Region|
|FaceImagesTrained|Sí|Face Images Trained (Imágenes de caras entrenadas)|Count|Total|Número de imágenes entrenadas. 1000 imágenes entrenadas por transacción.|ApiName, FeatureName, UsageChannel, Region|
|FacesStored|Sí|Faces Stored (Caras almacenadas)|Count|Total|Número de caras almacenadas, prorrateadas diariamente. El número de caras almacenadas se notifica a diario.|ApiName, FeatureName, UsageChannel, Region|
|FaceTransactions|Sí|Face Transactions (Transacciones de Face)|Count|Total|Número de llamadas API realizadas al servicio Face.|ApiName, FeatureName, UsageChannel, Region|
|ImagesStored|Sí|Images Stored (Imágenes almacenadas)|Count|Total|Número de imágenes de Custom Vision almacenadas.|ApiName, FeatureName, UsageChannel, Region|
|Latencia|Sí|Latencia|MilliSeconds|Average|Latencia en milisegundos.|ApiName, OperationName, Region|
|LearnedEvents|Sí|Eventos aprendidos|Count|Total|Número de eventos aprendidos.|IsMatchBaseline, modo, RunId|
|LUISSpeechRequests|Sí|LUIS Speech Requests (Solicitudes de voz de LUIS)|Count|Total|Número de solicitudes de conversión de voz en intención de LUIS.|ApiName, FeatureName, UsageChannel, Region|
|LUISTextRequests|Sí|LUIS Text Requests (Solicitudes de texto de LUIS)|Count|Total|Número de solicitudes de texto de LUIS.|ApiName, FeatureName, UsageChannel, Region|
|MatchedRewards|Sí|Recompensas coincidentes|Count|Total|Número de recompensas coincidentes.|Mode, RunId|
|NumberofSpeakerProfiles|Sí|Number of Speaker Profiles (Número de perfiles de hablante)|Count|Total|Número de perfiles de hablante inscritos. Se prorratea cada hora.|ApiName, FeatureName, UsageChannel, Region|
|ObservedRewards|Sí|Recompensas observadas|Count|Total|Número de recompensas observadas.|Mode, RunId|
|ProcessedCharacters|Sí|Caracteres procesados|Count|Total|Número de caracteres procesados por Immersive Reader.|ApiName, FeatureName, UsageChannel, Region|
|ProcessedHealthTextRecords|Sí|Processed Health Text Records (Registros de texto del estado procesados)|Count|Total|Número de registros de texto del estado procesados.|ApiName, FeatureName, UsageChannel, Region|
|ProcessedImages|Sí|Imágenes procesadas|Count|Total|Número de imágenes procesadas.|ApiName, FeatureName, UsageChannel, Region|
|ProcessedPages|Sí|Processed Pages (Páginas procesadas)|Count|Total|Número de páginas procesadas.|ApiName, FeatureName, UsageChannel, Region|
|ProcessedTextRecords|Sí|Registros de texto procesados|Count|Total|Número de registros de texto.|ApiName, FeatureName, UsageChannel, Region|
|ServerErrors|Sí|Errores del servidor|Count|Total|Número de llamadas con error interno del servicio (código de respuesta HTTP 5xx).|ApiName, OperationName, Region|
|SpeakerRecognitionTransactions|Sí|Speaker Recognition Transactions (Transacciones de Speaker Recognition)|Count|Total|Número de transacciones de reconocimiento del hablante.|ApiName, FeatureName, UsageChannel, Region|
|SpeechModelHostingHours|Sí|Speech Model Hosting Hours (Horas de hospedaje del modelo de voz)|Count|Total|Número de horas de hospedaje del modelo de voz.|ApiName, FeatureName, UsageChannel, Region|
|SpeechSessionDuration|Sí|Duración de la sesión de voz (en desuso)|Segundos|Total|Duración total de la sesión de voz en segundos.|ApiName, OperationName, Region|
|SuccessfulCalls|Sí|Llamadas correctas|Count|Total|Número de llamadas correctas.|ApiName, OperationName, Region|
|SynthesizedCharacters|Sí|Caracteres sintetizados|Count|Total|Número de caracteres.|ApiName, FeatureName, UsageChannel, Region|
|TextCharactersTranslated|Sí|Text Characters Translated (Caracteres de texto traducidos)|Count|Total|Número de caracteres en la solicitud entrante de traducción de texto.|ApiName, FeatureName, UsageChannel, Region|
|TextCustomCharactersTranslated|Sí|Text Custom Characters Translated (Caracteres de texto personalizados traducidos)|Count|Total|Número de caracteres en la solicitud entrante personalizada de traducción de texto.|ApiName, FeatureName, UsageChannel, Region|
|TextTrainedCharacters|Sí|Text Trained Characters (Caracteres de texto entrenados)|Count|Total|Número de caracteres entrenados mediante la traducción de texto.|ApiName, FeatureName, UsageChannel, Region|
|TotalCalls|Sí|Total de llamadas|Count|Total|Número total de llamadas.|ApiName, OperationName, Region|
|TotalErrors|Sí|Total de errores|Count|Total|Número total de llamadas con respuesta de error (código de respuesta HTTP 4xx o 5xx).|ApiName, OperationName, Region|
|TotalTokenCalls|Sí|Llamadas de token totales|Count|Total|Número total de llamadas de token.|ApiName, OperationName, Region|
|TotalTransactions|Sí|Transacciones totales (en desuso)|Count|Total|Número total de transacciones.|Sin dimensiones|
|VoiceModelHostingHours|Sí|Horas de hospedaje del modelo de voz|Count|Total|Número de horas.|ApiName, FeatureName, UsageChannel, Region|
|VoiceModelTrainingMinutes|Sí|Minutos de entrenamiento del modelo de voz|Count|Total|Número de minutos.|ApiName, FeatureName, UsageChannel, Region|


## <a name="microsoftcommunicationcommunicationservices"></a>Microsoft.Communication/CommunicationServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|APIRequestAuthentication|No|Solicitudes de API de autenticación|Count|Count|Recuento de todas las solicitudes en el punto de conexión de autenticación de Communication Services.|Operación, StatusCode, StatusCodeClass|
|APIRequestChat|Sí|Solicitudes de API de chat|Count|Count|Recuento de todas las solicitudes en el punto de conexión de chat de Communication Services.|Operación, StatusCode, StatusCodeClass|
|APIRequestSMS|Sí|Solicitudes de API de SMS|Count|Count|Recuento de todas las solicitudes en el punto de conexión de SMS de Communication Services.|Operación, StatusCode, StatusCodeClass, ErrorCode|


## <a name="microsoftcomputecloudservices"></a>Microsoft.Compute/cloudServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Bytes de memoria disponible|Sí|Bytes de memoria disponible (versión preliminar)|Bytes|Average|Cantidad de memoria física, en bytes, disponible inmediatamente para su asignación a un proceso o para su uso por parte del sistema en la máquina virtual|RoleInstanceId, RoleId|
|Bytes de lectura de disco|Sí|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|RoleInstanceId, RoleId|
|Operaciones de lectura de disco por segundo|Sí|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco|RoleInstanceId, RoleId|
|Bytes de escritura de disco|Sí|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|RoleInstanceId, RoleId|
|Operaciones de escritura por segundo en disco|Sí|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco|RoleInstanceId, RoleId|
|Red entrante total|Sí|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|RoleInstanceId, RoleId|
|Red saliente total|Sí|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|RoleInstanceId, RoleId|
|Porcentaje de CPU|Sí|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|RoleInstanceId, RoleId|


## <a name="microsoftcomputecloudservicesroles"></a>Microsoft.Compute/cloudServices/roles

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Bytes de memoria disponible|Sí|Bytes de memoria disponible (versión preliminar)|Bytes|Average|Cantidad de memoria física, en bytes, disponible inmediatamente para su asignación a un proceso o para su uso por parte del sistema en la máquina virtual|RoleInstanceId, RoleId|
|Bytes de lectura de disco|Sí|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|RoleInstanceId, RoleId|
|Operaciones de lectura de disco por segundo|Sí|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco|RoleInstanceId, RoleId|
|Bytes de escritura de disco|Sí|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|RoleInstanceId, RoleId|
|Operaciones de escritura por segundo en disco|Sí|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco|RoleInstanceId, RoleId|
|Red entrante total|Sí|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|RoleInstanceId, RoleId|
|Red saliente total|Sí|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|RoleInstanceId, RoleId|
|Porcentaje de CPU|Sí|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|RoleInstanceId, RoleId|


## <a name="microsoftcomputedisks"></a>microsoft.compute/disks

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Bytes por segundo de lectura de discos compuestos|No|Bytes por segundo de lectura de discos (versión preliminar)|Bytes|Average|Bytes por segundo de lectura del disco durante el período de supervisión. Tenga en cuenta que esta métrica está en versión preliminar y está sujeta a cambios antes de que esté disponible con carácter general|Sin dimensiones|
|Operaciones de lectura de disco compuesto por segundo|No|Operaciones de lectura de discos por segundo (versión preliminar)|Bytes|Average|Número de operaciones de E/S de lectura durante el período de supervisión. Tenga en cuenta que esta métrica está en versión preliminar y está sujeta a cambios antes de que esté disponible con carácter general|Sin dimensiones|
|Bytes por segundo de escritura en discos compuestos|No|Bytes por segundo de escritura en discos (versión preliminar)|Bytes|Average|Bytes por segundo de escritura en el disco durante el período de supervisión. Tenga en cuenta que esta métrica está en versión preliminar y está sujeta a cambios antes de que esté disponible con carácter general|Sin dimensiones|
|Operaciones de escritura en discos compuestos por segundo|No|Operaciones de escritura en discos por segundo (versión preliminar)|Bytes|Average|Número de operaciones de E/S de escritura durante el período de supervisión. Tenga en cuenta que esta métrica está en versión preliminar y está sujeta a cambios antes de que esté disponible con carácter general|Sin dimensiones|


## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Bytes de memoria disponible|Sí|Bytes de memoria disponible (versión preliminar)|Bytes|Average|Cantidad de memoria física, en bytes, disponible inmediatamente para su asignación a un proceso o para su uso por parte del sistema en la máquina virtual|Sin dimensiones|
|Créditos de CPU consumidos|Sí|Créditos de CPU consumidos|Count|Average|Número total de créditos consumidos por la máquina virtual. Solo disponible en máquinas virtuales ampliables de la serie B|Sin dimensiones|
|Créditos de CPU restantes|Sí|Créditos de CPU restantes|Count|Average|Número total de créditos disponibles para ampliar. Solo disponible en máquinas virtuales ampliables de la serie B|Sin dimensiones|
|Porcentaje de ancho de banda consumido del disco de datos|Sí|Porcentaje de ancho de banda consumido del disco de datos|Percent|Average|Porcentaje de ancho de banda de disco de datos consumido por minuto|LUN|
|Porcentaje de consumo de IOPS de disco de datos|Sí|Porcentaje de consumo de IOPS de disco de datos|Percent|Average|Porcentaje de operaciones de E/S de disco de datos consumido por minuto|LUN|
|Ancho de banda máximo de expansión de disco de datos|Sí|Ancho de banda máximo de expansión de disco de datos|Count|Average|Rendimiento máximo en bytes por segundos que el disco de datos puede conseguir mediante la expansión|LUN|
|IOPS máximo de expansión del disco de datos|Sí|IOPS máximo de expansión del disco de datos|Count|Average|IOPS máximo que el disco de datos puede lograr mediante la expansión|LUN|
|Profundidad de cola de discos de datos|Sí|Profundidad de cola de discos de datos|Count|Average|Profundidad de la cola del disco de datos (o longitud de la cola)|LUN|
|Bytes de lectura de discos de datos por segundo|Sí|Bytes de lectura de discos de datos por segundo|BytesPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión|LUN|
|Operaciones de lectura de discos de datos por segundo|Sí|Operaciones de lectura de discos de datos por segundo|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión|LUN|
|Ancho de banda de destino del disco de datos|Sí|Ancho de banda de destino del disco de datos|Count|Average|Rendimiento de línea de base en bytes por segundos que el disco de datos puede conseguir sin la expansión|LUN|
|IOPS de destino del disco de datos|Sí|IOPS de destino del disco de datos|Count|Average|IOPS de línea de base que el disco de datos puede lograr sin la expansión|LUN|
|Porcentaje de créditos de BPS de expansión usados del disco de datos|Sí|Porcentaje de créditos de BPS de expansión usados del disco de datos|Percent|Average|Porcentaje de créditos de ancho de banda de expansión del disco de datos usados hasta ahora|LUN|
|Porcentaje de créditos de E/S de expansión usados del disco de datos|Sí|Porcentaje de créditos de E/S de expansión usados del disco de datos|Percent|Average|Porcentaje de créditos de E/S de expansión del disco de datos usados hasta ahora|LUN|
|Bytes de escritura de discos de datos por segundo|Sí|Bytes de escritura en disco de datos por segundo|BytesPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión|LUN|
|Operaciones de escritura de discos de datos por segundo|Sí|Operaciones de escritura de discos de datos por segundo|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión|LUN|
|Bytes de lectura de disco|Sí|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Sí|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco|Sin dimensiones|
|Bytes de escritura de disco|Sí|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Sí|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco|Sin dimensiones|
|Flujos de entrada|Sí|Flujos de entrada|Count|Average|Los flujos de entrada son el número de flujos actuales en la dirección entrante (el tráfico que va a la máquina virtual)|Sin dimensiones|
|Tasa de creación máxima de flujos de entrada|Sí|Tasa de creación máxima de flujos de entrada|CountPerSecond|Average|La tasa de creación máxima de flujos de entrada (el tráfico que va a la máquina virtual)|Sin dimensiones|
|Red interna|Sí|Network In Billable (Deprecated) [Entrada de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales recibieron en todas las interfaces de red (tráfico entrante) (en desuso).|Sin dimensiones|
|Red entrante total|Sí|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|Sin dimensiones|
|Red interna|Sí|Network Out Billable (Deprecated) [Salida de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales enviaron en todas las interfaces de red (tráfico saliente) (en desuso)|Sin dimensiones|
|Red saliente total|Sí|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|Sin dimensiones|
|Porcentaje de ancho de banda consumido del sistema operativo|Sí|Porcentaje de ancho de banda consumido del sistema operativo|Percent|Average|Porcentaje de ancho de banda de disco del sistema operativo consumido por minuto|LUN|
|Porcentaje de consumo de IOPS de disco del sistema operativo|Sí|Porcentaje de consumo de IOPS de disco del sistema operativo|Percent|Average|Porcentaje de E/S de disco del sistema operativo consumido por minuto|LUN|
|Ancho de banda máximo de expansión de disco del sistema operativo|Sí|Ancho de banda máximo de expansión de disco del sistema operativo|Count|Average|Rendimiento máximo en bytes por segundo que el disco del sistema operativo puede conseguir mediante la expansión|LUN|
|IOPS máximo de expansión del disco del sistema operativo|Sí|IOPS máximo de expansión del disco del sistema operativo|Count|Average|IOPS máximo que el disco del sistema operativo puede lograr mediante la expansión|LUN|
|Profundidad de cola de discos de SO|Sí|Profundidad de cola de discos de SO|Count|Average|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|Sin dimensiones|
|Bytes de lectura de discos del sistema operativo por segundo|Sí|Bytes de lectura de discos del sistema operativo por segundo|BytesPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Operaciones de lectura de discos de SO por segundo|Sí|Operaciones de lectura de discos de SO por segundo|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Ancho de banda de destino del disco del sistema operativo|Sí|Ancho de banda de destino del disco del sistema operativo|Count|Average|Rendimiento de línea de base en bytes por segundos que el disco del sistema operativo puede conseguir sin la expansión|LUN|
|IOPS de destino del disco del sistema operativo|Sí|IOPS de destino del disco del sistema operativo|Count|Average|IOPS de línea de base que el disco del sistema operativo puede lograr sin la expansión|LUN|
|Porcentaje de créditos de BPS de expansión usados del disco del sistema operativo|Sí|Porcentaje de créditos de BPS de expansión usados del disco del sistema operativo|Percent|Average|Porcentaje de créditos de ancho de banda de expansión del disco del sistema operativo usados hasta ahora|LUN|
|Porcentaje de créditos de E/S de expansión usados del disco del sistema operativo|Sí|Porcentaje de créditos de E/S de expansión usados del disco del sistema operativo|Percent|Average|Porcentaje de créditos de E/S de expansión del disco del sistema operativo usados hasta ahora|LUN|
|Bytes de escritura en discos del SO por segundo|Sí|Bytes de escritura en disco del SO por segundo|BytesPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Operaciones de escritura en discos de SO por segundo|Sí|Operaciones de escritura en discos de SO por segundo|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Flujos de salida|Sí|Flujos de salida|Count|Average|Los flujos de salida son el número de flujos actuales en la dirección saliente (el tráfico que sale de la máquina virtual)|Sin dimensiones|
|Tasa de creación máxima de flujos de salida|Sí|Tasa de creación máxima de flujos de salida|CountPerSecond|Average|La tasa de creación máxima de flujos de salida (el tráfico que sale de la máquina virtual)|Sin dimensiones|
|Porcentaje de CPU|Sí|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|Sin dimensiones|
|Acierto de lectura de la caché de discos de datos premium|Sí|Acierto de lectura de la caché de discos de datos premium|Percent|Average|Acierto de lectura de la caché de discos de datos premium|LUN|
|Error de lectura de la caché de discos de datos premium|Sí|Error de lectura de la caché de discos de datos premium|Percent|Average|Error de lectura de la caché de discos de datos premium|LUN|
|Acierto de lectura de la caché de discos de SO premium|Sí|Acierto de lectura de la caché de discos de SO premium|Percent|Average|Acierto de lectura de la caché de discos de SO premium|Sin dimensiones|
|Error de lectura de la caché de discos de SO premium|Sí|Error de lectura de la caché de discos de SO premium|Percent|Average|Error de lectura de la caché de discos de SO premium|Sin dimensiones|
|Porcentaje de consumo de ancho de banda en caché de máquinas virtuales|Sí|Porcentaje de consumo de ancho de banda en caché de máquinas virtuales|Percent|Average|Porcentaje de ancho de banda de disco en caché consumido por la máquina virtual|Sin dimensiones|
|Porcentaje de consumo de IOPS en caché de la máquina virtual|Sí|Porcentaje de consumo de IOPS en caché de la máquina virtual|Percent|Average|Porcentaje de IOPS de disco en caché consumido por la máquina virtual|Sin dimensiones|
|Porcentaje de consumo de ancho de banda que no está almacenado en caché de máquinas virtuales|Sí|Porcentaje de consumo de ancho de banda que no está almacenado en caché de máquinas virtuales|Percent|Average|Porcentaje de ancho de banda de disco no almacenado en caché consumido por la máquina virtual|Sin dimensiones|
|Porcentaje de consumo de IOPS que no están almacenados en el caché de la máquina virtual|Sí|Porcentaje de consumo de IOPS que no están almacenados en el caché de la máquina virtual|Percent|Average|Porcentaje de IOPS de disco no almacenado en caché consumido por la máquina virtual|Sin dimensiones|
|VmAvailabilityMetric|Sí|VM Availability Metric (Preview) [Métrica de disponibilidad de VM (versión preliminar)]|Count|Average|Medida de la disponibilidad de las máquinas virtuales a lo largo del tiempo. Nota: Esta métrica está en versión preliminar solo para un pequeño conjunto de clientes en estos momentos, ya que priorizamos la mejora de la calidad y la coherencia de los datos. A medida que ampliemos nuestro estándar de datos, implementaremos esta característica a nivel general de forma escalonada.|Sin dimensiones|


## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualmachineScaleSets

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Bytes de memoria disponible|Sí|Bytes de memoria disponible (versión preliminar)|Bytes|Average|Cantidad de memoria física, en bytes, disponible inmediatamente para su asignación a un proceso o para su uso por parte del sistema en la máquina virtual|VMName|
|Créditos de CPU consumidos|Sí|Créditos de CPU consumidos|Count|Average|Número total de créditos consumidos por la máquina virtual. Solo disponible en máquinas virtuales ampliables de la serie B|Sin dimensiones|
|Créditos de CPU restantes|Sí|Créditos de CPU restantes|Count|Average|Número total de créditos disponibles para ampliar. Solo disponible en máquinas virtuales ampliables de la serie B|Sin dimensiones|
|Porcentaje de ancho de banda consumido del disco de datos|Sí|Porcentaje de ancho de banda consumido del disco de datos|Percent|Average|Porcentaje de ancho de banda de disco de datos consumido por minuto|LUN, VMName|
|Porcentaje de consumo de IOPS de disco de datos|Sí|Porcentaje de consumo de IOPS de disco de datos|Percent|Average|Porcentaje de operaciones de E/S de disco de datos consumido por minuto|LUN, VMName|
|Ancho de banda máximo de expansión de disco de datos|Sí|Ancho de banda máximo de expansión de disco de datos|Count|Average|Rendimiento máximo en bytes por segundos que el disco de datos puede conseguir mediante la expansión|LUN, VMName|
|IOPS máximo de expansión del disco de datos|Sí|IOPS máximo de expansión del disco de datos|Count|Average|IOPS máximo que el disco de datos puede lograr mediante la expansión|LUN, VMName|
|Profundidad de cola de discos de datos|Sí|Profundidad de cola de discos de datos|Count|Average|Profundidad de la cola del disco de datos (o longitud de la cola)|LUN, VMName|
|Bytes de lectura de discos de datos por segundo|Sí|Bytes de lectura de discos de datos por segundo|BytesPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión|LUN, VMName|
|Operaciones de lectura de discos de datos por segundo|Sí|Operaciones de lectura de discos de datos por segundo|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión|LUN, VMName|
|Ancho de banda de destino del disco de datos|Sí|Ancho de banda de destino del disco de datos|Count|Average|Rendimiento de línea de base en bytes por segundos que el disco de datos puede conseguir sin la expansión|LUN, VMName|
|IOPS de destino del disco de datos|Sí|IOPS de destino del disco de datos|Count|Average|IOPS de línea de base que el disco de datos puede lograr sin la expansión|LUN, VMName|
|Porcentaje de créditos de BPS de expansión usados del disco de datos|Sí|Porcentaje de créditos de BPS de expansión usados del disco de datos|Percent|Average|Porcentaje de créditos de ancho de banda de expansión del disco de datos usados hasta ahora|LUN, VMName|
|Porcentaje de créditos de E/S de expansión usados del disco de datos|Sí|Porcentaje de créditos de E/S de expansión usados del disco de datos|Percent|Average|Porcentaje de créditos de E/S de expansión del disco de datos usados hasta ahora|LUN, VMName|
|Bytes de escritura de discos de datos por segundo|Sí|Bytes de escritura en disco de datos por segundo|BytesPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión|LUN, VMName|
|Operaciones de escritura de discos de datos por segundo|Sí|Operaciones de escritura de discos de datos por segundo|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión|LUN, VMName|
|Bytes de lectura de disco|Sí|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|VMName|
|Operaciones de lectura de disco por segundo|Sí|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco|VMName|
|Bytes de escritura de disco|Sí|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|VMName|
|Operaciones de escritura por segundo en disco|Sí|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco|VMName|
|Flujos de entrada|Sí|Flujos de entrada|Count|Average|Los flujos de entrada son el número de flujos actuales en la dirección entrante (el tráfico que va a la máquina virtual)|VMName|
|Tasa de creación máxima de flujos de entrada|Sí|Tasa de creación máxima de flujos de entrada|CountPerSecond|Average|La tasa de creación máxima de flujos de entrada (el tráfico que va a la máquina virtual)|VMName|
|Red interna|Sí|Network In Billable (Deprecated) [Entrada de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales recibieron en todas las interfaces de red (tráfico entrante) (en desuso).|VMName|
|Red entrante total|Sí|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|VMName|
|Red interna|Sí|Network Out Billable (Deprecated) [Salida de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales enviaron en todas las interfaces de red (tráfico saliente) (en desuso)|VMName|
|Red saliente total|Sí|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|VMName|
|Porcentaje de ancho de banda consumido del sistema operativo|Sí|Porcentaje de ancho de banda consumido del sistema operativo|Percent|Average|Porcentaje de ancho de banda de disco del sistema operativo consumido por minuto|LUN, VMName|
|Porcentaje de consumo de IOPS de disco del sistema operativo|Sí|Porcentaje de consumo de IOPS de disco del sistema operativo|Percent|Average|Porcentaje de E/S de disco del sistema operativo consumido por minuto|LUN, VMName|
|Ancho de banda máximo de expansión de disco del sistema operativo|Sí|Ancho de banda máximo de expansión de disco del sistema operativo|Count|Average|Rendimiento máximo en bytes por segundo que el disco del sistema operativo puede conseguir mediante la expansión|LUN, VMName|
|IOPS máximo de expansión del disco del sistema operativo|Sí|IOPS máximo de expansión del disco del sistema operativo|Count|Average|IOPS máximo que el disco del sistema operativo puede lograr mediante la expansión|LUN, VMName|
|Profundidad de cola de discos de SO|Sí|Profundidad de cola de discos de SO|Count|Average|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|VMName|
|Bytes de lectura de discos del sistema operativo por segundo|Sí|Bytes de lectura de discos del sistema operativo por segundo|BytesPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|VMName|
|Operaciones de lectura de discos de SO por segundo|Sí|Operaciones de lectura de discos de SO por segundo|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|VMName|
|Ancho de banda de destino del disco del sistema operativo|Sí|Ancho de banda de destino del disco del sistema operativo|Count|Average|Rendimiento de línea de base en bytes por segundos que el disco del sistema operativo puede conseguir sin la expansión|LUN, VMName|
|IOPS de destino del disco del sistema operativo|Sí|IOPS de destino del disco del sistema operativo|Count|Average|IOPS de línea de base que el disco del sistema operativo puede lograr sin la expansión|LUN, VMName|
|Porcentaje de créditos de BPS de expansión usados del disco del sistema operativo|Sí|Porcentaje de créditos de BPS de expansión usados del disco del sistema operativo|Percent|Average|Porcentaje de créditos de ancho de banda de expansión del disco del sistema operativo usados hasta ahora|LUN, VMName|
|Porcentaje de créditos de E/S de expansión usados del disco del sistema operativo|Sí|Porcentaje de créditos de E/S de expansión usados del disco del sistema operativo|Percent|Average|Porcentaje de créditos de E/S de expansión del disco del sistema operativo usados hasta ahora|LUN, VMName|
|Bytes de escritura en discos del SO por segundo|Sí|Bytes de escritura en disco del SO por segundo|BytesPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|VMName|
|Operaciones de escritura en discos de SO por segundo|Sí|Operaciones de escritura en discos de SO por segundo|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|VMName|
|Flujos de salida|Sí|Flujos de salida|Count|Average|Los flujos de salida son el número de flujos actuales en la dirección saliente (el tráfico que sale de la máquina virtual)|VMName|
|Tasa de creación máxima de flujos de salida|Sí|Tasa de creación máxima de flujos de salida|CountPerSecond|Average|La tasa de creación máxima de flujos de salida (el tráfico que sale de la máquina virtual)|VMName|
|Porcentaje de CPU|Sí|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|VMName|
|Acierto de lectura de la caché de discos de datos premium|Sí|Acierto de lectura de la caché de discos de datos premium|Percent|Average|Acierto de lectura de la caché de discos de datos premium|LUN, VMName|
|Error de lectura de la caché de discos de datos premium|Sí|Error de lectura de la caché de discos de datos premium|Percent|Average|Error de lectura de la caché de discos de datos premium|LUN, VMName|
|Acierto de lectura de la caché de discos de SO premium|Sí|Acierto de lectura de la caché de discos de SO premium|Percent|Average|Acierto de lectura de la caché de discos de SO premium|VMName|
|Error de lectura de la caché de discos de SO premium|Sí|Error de lectura de la caché de discos de SO premium|Percent|Average|Error de lectura de la caché de discos de SO premium|VMName|
|Porcentaje de consumo de ancho de banda en caché de máquinas virtuales|Sí|Porcentaje de consumo de ancho de banda en caché de máquinas virtuales|Percent|Average|Porcentaje de ancho de banda de disco en caché consumido por la máquina virtual|VMName|
|Porcentaje de consumo de IOPS en caché de la máquina virtual|Sí|Porcentaje de consumo de IOPS en caché de la máquina virtual|Percent|Average|Porcentaje de IOPS de disco en caché consumido por la máquina virtual|VMName|
|Porcentaje de consumo de ancho de banda que no está almacenado en caché de máquinas virtuales|Sí|Porcentaje de consumo de ancho de banda que no está almacenado en caché de máquinas virtuales|Percent|Average|Porcentaje de ancho de banda de disco no almacenado en caché consumido por la máquina virtual|VMName|
|Porcentaje de consumo de IOPS que no están almacenados en el caché de la máquina virtual|Sí|Porcentaje de consumo de IOPS que no están almacenados en el caché de la máquina virtual|Percent|Average|Porcentaje de IOPS de disco no almacenado en caché consumido por la máquina virtual|VMName|
|VmAvailabilityMetric|Sí|VM Availability Metric (Preview) [Métrica de disponibilidad de VM (versión preliminar)]|Count|Average|Medida de la disponibilidad de las máquinas virtuales a lo largo del tiempo. Nota: Esta métrica está en versión preliminar solo para un pequeño conjunto de clientes en estos momentos, ya que priorizamos la mejora de la calidad y la coherencia de los datos. A medida que ampliemos nuestro estándar de datos, implementaremos esta característica a nivel general de forma escalonada.|VMName|


## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Bytes de memoria disponible|Sí|Bytes de memoria disponible (versión preliminar)|Bytes|Average|Cantidad de memoria física, en bytes, disponible inmediatamente para su asignación a un proceso o para su uso por parte del sistema en la máquina virtual|Sin dimensiones|
|Créditos de CPU consumidos|Sí|Créditos de CPU consumidos|Count|Average|Número total de créditos consumidos por la máquina virtual. Solo disponible en máquinas virtuales ampliables de la serie B|Sin dimensiones|
|Créditos de CPU restantes|Sí|Créditos de CPU restantes|Count|Average|Número total de créditos disponibles para ampliar. Solo disponible en máquinas virtuales ampliables de la serie B|Sin dimensiones|
|Porcentaje de ancho de banda consumido del disco de datos|Sí|Porcentaje de ancho de banda consumido del disco de datos|Percent|Average|Porcentaje de ancho de banda de disco de datos consumido por minuto|LUN|
|Porcentaje de consumo de IOPS de disco de datos|Sí|Porcentaje de consumo de IOPS de disco de datos|Percent|Average|Porcentaje de operaciones de E/S de disco de datos consumido por minuto|LUN|
|Ancho de banda máximo de expansión de disco de datos|Sí|Ancho de banda máximo de expansión de disco de datos|Count|Average|Rendimiento máximo en bytes por segundos que el disco de datos puede conseguir mediante la expansión|LUN|
|IOPS máximo de expansión del disco de datos|Sí|IOPS máximo de expansión del disco de datos|Count|Average|IOPS máximo que el disco de datos puede lograr mediante la expansión|LUN|
|Profundidad de cola de discos de datos|Sí|Profundidad de cola de discos de datos|Count|Average|Profundidad de la cola del disco de datos (o longitud de la cola)|LUN|
|Bytes de lectura de discos de datos por segundo|Sí|Bytes de lectura de discos de datos por segundo|BytesPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión|LUN|
|Operaciones de lectura de discos de datos por segundo|Sí|Operaciones de lectura de discos de datos por segundo|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión|LUN|
|Ancho de banda de destino del disco de datos|Sí|Ancho de banda de destino del disco de datos|Count|Average|Rendimiento de línea de base en bytes por segundos que el disco de datos puede conseguir sin la expansión|LUN|
|IOPS de destino del disco de datos|Sí|IOPS de destino del disco de datos|Count|Average|IOPS de línea de base que el disco de datos puede lograr sin la expansión|LUN|
|Porcentaje de créditos de BPS de expansión usados del disco de datos|Sí|Porcentaje de créditos de BPS de expansión usados del disco de datos|Percent|Average|Porcentaje de créditos de ancho de banda de expansión del disco de datos usados hasta ahora|LUN|
|Porcentaje de créditos de E/S de expansión usados del disco de datos|Sí|Porcentaje de créditos de E/S de expansión usados del disco de datos|Percent|Average|Porcentaje de créditos de E/S de expansión del disco de datos usados hasta ahora|LUN|
|Bytes de escritura de discos de datos por segundo|Sí|Bytes de escritura en disco de datos por segundo|BytesPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión|LUN|
|Operaciones de escritura de discos de datos por segundo|Sí|Operaciones de escritura de discos de datos por segundo|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión|LUN|
|Bytes de lectura de disco|Sí|Bytes de lectura de disco|Bytes|Total|Bytes que se leen desde el disco durante el período de supervisión|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Sí|Operaciones de lectura de disco por segundo|CountPerSecond|Average|E/S por segundo de lectura de disco|Sin dimensiones|
|Bytes de escritura de disco|Sí|Bytes de escritura de disco|Bytes|Total|Bytes que se escriben en el disco durante el período de supervisión|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Sí|Operaciones de escritura por segundo en disco|CountPerSecond|Average|E/S por segundo de escritura en disco|Sin dimensiones|
|Flujos de entrada|Sí|Flujos de entrada|Count|Average|Los flujos de entrada son el número de flujos actuales en la dirección entrante (el tráfico que va a la máquina virtual)|Sin dimensiones|
|Tasa de creación máxima de flujos de entrada|Sí|Tasa de creación máxima de flujos de entrada|CountPerSecond|Average|La tasa de creación máxima de flujos de entrada (el tráfico que va a la máquina virtual)|Sin dimensiones|
|Red interna|Sí|Network In Billable (Deprecated) [Entrada de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales recibieron en todas las interfaces de red (tráfico entrante) (en desuso).|Sin dimensiones|
|Red entrante total|Sí|Red entrante total|Bytes|Total|El número de bytes recibidos en todas las interfaces de red por las máquinas virtuales (tráfico entrante)|Sin dimensiones|
|Red interna|Sí|Network Out Billable (Deprecated) [Salida de red facturable (en desuso)]|Bytes|Total|Número de bytes facturables que las máquinas virtuales enviaron en todas las interfaces de red (tráfico saliente) (en desuso)|Sin dimensiones|
|Red saliente total|Sí|Red saliente total|Bytes|Total|El número de bytes enviados en todas las interfaces de red por las máquinas virtuales (tráfico saliente)|Sin dimensiones|
|Porcentaje de ancho de banda consumido del sistema operativo|Sí|Porcentaje de ancho de banda consumido del sistema operativo|Percent|Average|Porcentaje de ancho de banda de disco del sistema operativo consumido por minuto|LUN|
|Porcentaje de consumo de IOPS de disco del sistema operativo|Sí|Porcentaje de consumo de IOPS de disco del sistema operativo|Percent|Average|Porcentaje de E/S de disco del sistema operativo consumido por minuto|LUN|
|Ancho de banda máximo de expansión de disco del sistema operativo|Sí|Ancho de banda máximo de expansión de disco del sistema operativo|Count|Average|Rendimiento máximo en bytes por segundo que el disco del sistema operativo puede conseguir mediante la expansión|LUN|
|IOPS máximo de expansión del disco del sistema operativo|Sí|IOPS máximo de expansión del disco del sistema operativo|Count|Average|IOPS máximo que el disco del sistema operativo puede lograr mediante la expansión|LUN|
|Profundidad de cola de discos de SO|Sí|Profundidad de cola de discos de SO|Count|Average|Profundidad de la cola de discos del sistema operativo (o longitud de la cola)|Sin dimensiones|
|Bytes de lectura de discos del sistema operativo por segundo|Sí|Bytes de lectura de discos del sistema operativo por segundo|BytesPerSecond|Average|Bytes por segundo de lectura de un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Operaciones de lectura de discos de SO por segundo|Sí|Operaciones de lectura de discos de SO por segundo|CountPerSecond|Average|IOPS de lectura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Ancho de banda de destino del disco del sistema operativo|Sí|Ancho de banda de destino del disco del sistema operativo|Count|Average|Rendimiento de línea de base en bytes por segundos que el disco del sistema operativo puede conseguir sin la expansión|LUN|
|IOPS de destino del disco del sistema operativo|Sí|IOPS de destino del disco del sistema operativo|Count|Average|IOPS de línea de base que el disco del sistema operativo puede lograr sin la expansión|LUN|
|Porcentaje de créditos de BPS de expansión usados del disco del sistema operativo|Sí|Porcentaje de créditos de BPS de expansión usados del disco del sistema operativo|Percent|Average|Porcentaje de créditos de ancho de banda de expansión del disco del sistema operativo usados hasta ahora|LUN|
|Porcentaje de créditos de E/S de expansión usados del disco del sistema operativo|Sí|Porcentaje de créditos de E/S de expansión usados del disco del sistema operativo|Percent|Average|Porcentaje de créditos de E/S de expansión del disco del sistema operativo usados hasta ahora|LUN|
|Bytes de escritura en discos del SO por segundo|Sí|Bytes de escritura en disco del SO por segundo|BytesPerSecond|Average|Bytes por segundo escritos en un único disco durante el período de supervisión del disco del sistema operativo|Sin dimensiones|
|Operaciones de escritura en discos de SO por segundo|Sí|Operaciones de escritura en discos de SO por segundo|CountPerSecond|Average|IOPS de escritura de un único disco durante el período de supervisión para el disco de SO|Sin dimensiones|
|Flujos de salida|Sí|Flujos de salida|Count|Average|Los flujos de salida son el número de flujos actuales en la dirección saliente (el tráfico que sale de la máquina virtual)|Sin dimensiones|
|Tasa de creación máxima de flujos de salida|Sí|Tasa de creación máxima de flujos de salida|CountPerSecond|Average|La tasa de creación máxima de flujos de salida (el tráfico que sale de la máquina virtual)|Sin dimensiones|
|Porcentaje de CPU|Sí|Porcentaje de CPU|Percent|Average|El porcentaje de unidades de proceso asignadas que las máquinas virtuales usan actualmente|Sin dimensiones|
|Acierto de lectura de la caché de discos de datos premium|Sí|Acierto de lectura de la caché de discos de datos premium|Percent|Average|Acierto de lectura de la caché de discos de datos premium|LUN|
|Error de lectura de la caché de discos de datos premium|Sí|Error de lectura de la caché de discos de datos premium|Percent|Average|Error de lectura de la caché de discos de datos premium|LUN|
|Acierto de lectura de la caché de discos de SO premium|Sí|Acierto de lectura de la caché de discos de SO premium|Percent|Average|Acierto de lectura de la caché de discos de SO premium|Sin dimensiones|
|Error de lectura de la caché de discos de SO premium|Sí|Error de lectura de la caché de discos de SO premium|Percent|Average|Error de lectura de la caché de discos de SO premium|Sin dimensiones|
|Porcentaje de consumo de ancho de banda en caché de máquinas virtuales|Sí|Porcentaje de consumo de ancho de banda en caché de máquinas virtuales|Percent|Average|Porcentaje de ancho de banda de disco en caché consumido por la máquina virtual|Sin dimensiones|
|Porcentaje de consumo de IOPS en caché de la máquina virtual|Sí|Porcentaje de consumo de IOPS en caché de la máquina virtual|Percent|Average|Porcentaje de IOPS de disco en caché consumido por la máquina virtual|Sin dimensiones|
|Porcentaje de consumo de ancho de banda que no está almacenado en caché de máquinas virtuales|Sí|Porcentaje de consumo de ancho de banda que no está almacenado en caché de máquinas virtuales|Percent|Average|Porcentaje de ancho de banda de disco no almacenado en caché consumido por la máquina virtual|Sin dimensiones|
|Porcentaje de consumo de IOPS que no están almacenados en el caché de la máquina virtual|Sí|Porcentaje de consumo de IOPS que no están almacenados en el caché de la máquina virtual|Percent|Average|Porcentaje de IOPS de disco no almacenado en caché consumido por la máquina virtual|Sin dimensiones|


## <a name="microsoftconnectedvehicleplatformaccounts"></a>Microsoft.ConnectedVehicle/platformAccounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ClaimsProviderRequestLatency|Sí|Tiempo de ejecución de solicitud HTTP|Milisegundos|Average|Tiempo medio de ejecución de las solicitudes al punto de conexión del proveedor de notificaciones del cliente en milisegundos.|VehicleId, DeviceName, IsSuccessful, FailureCategory|
|ClaimsProviderRequests|Sí|Solicitudes del proveedor de notificaciones|Count|Total|Número de solicitudes al proveedor de notificaciones.|VehicleId, DeviceName, IsSuccessful, FailureCategory|
|ConnectionServiceRequestRuntime|Sí|Tiempo de ejecución de la solicitud de servicio de conexión del vehículo|Milisegundos|Average|Tiempo medio de ejecución de solicitudes de conexión del vehículo en milisegundos.|VehicleId, DeviceName, IsSuccessful, FailureCategory|
|ConnectionServiceRequests|Sí|Solicitudes de servicio de conexión del vehículo|Count|Total|Número total de solicitudes de conexión del vehículo.|VehicleId, DeviceName, IsSuccessful, FailureCategory|
|DataPipelineMessageCount|Sí|Recuento de mensajes de canalización de datos|Count|Total|Número total de mensajes enviados a la canalización de datos MCVP para su almacenamiento.|VehicleId, DeviceName, IsSuccessful, FailureCategory|
|ExtensionInvocationCount|Sí|Recuento de invocaciones de extensión|Count|Total|Número total de veces que se ha llamado a una extensión.|VehicleId, DeviceName, ExtensionName, IsSuccessful, FailureCategory|
|ExtensionInvocationRuntime|Sí|Tiempo de ejecución de invocación de extensión|Milisegundos|Average|Tiempo medio de ejecución empleado en una extensión en milisegundos.|VehicleId, DeviceName, ExtensionName, IsSuccessful, FailureCategory|
|ProvisionerServiceRequestRuntime|Sí|Tiempo de ejecución de aprovisionamiento del vehículo|Milisegundos|Average|Tiempo medio de ejecución de las solicitudes de aprovisionamiento del vehículo en milisegundos.|VehicleId, DeviceName, IsSuccessful, FailureCategory|
|ProvisionerServiceRequests|Sí|Solicitudes de servicio de aprovisionamiento del vehículo|Count|Total|Número total de solicitudes de aprovisionamiento del vehículo.|VehicleId, DeviceName, IsSuccessful, FailureCategory|
|StateStoreReadRequestLatency|Sí|Tiempo de ejecución de lectura del almacén de estado|Milisegundos|Average|Tiempo medio de ejecución de solicitudes de lectura del almacén de estado en milisegundos.|VehicleId, DeviceName, ExtensionName, IsSuccessful, FailureCategory|
|StateStoreReadRequests|Sí|Solicitudes de lectura del almacén de estado|Count|Total|Número de solicitudes de lectura al almacén de estado.|VehicleId, DeviceName, ExtensionName, IsSuccessful, FailureCategory|
|StateStoreWriteRequestLatency|Sí|Tiempo de ejecución de escritura del almacén de estado|Milisegundos|Average|Tiempo medio de ejecución de la solicitud de escritura del almacén de estado en milisegundos.|VehicleId, DeviceName, ExtensionName, IsSuccessful, FailureCategory|
|StateStoreWriteRequests|Sí|Solicitudes de escritura del almacén de estado.|Count|Total|Número de solicitudes de escritura al almacén de estado.|VehicleId, DeviceName, ExtensionName, IsSuccessful, FailureCategory|


## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|CpuUsage|Sí|Uso de CPU|Count|Average|Uso de la CPU en todos los núcleos en millares de núcleos.|containerName|
|MemoryUsage|Sí|Uso de la memoria|Bytes|Average|Uso de memoria total en bytes.|containerName|
|NetworkBytesReceivedPerSecond|Sí|Bytes de red recibidos por segundo|Bytes|Average|Bytes de red recibidos por segundo.|Sin dimensiones|
|NetworkBytesTransmittedPerSecond|Sí|Bytes de red transmitidos por segundo|Bytes|Average|Bytes de red transmitidos por segundo.|Sin dimensiones|


## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AgentPoolCPUTime|Sí|Tiempo de CPU del grupo de agentes|Segundos|Total|Tiempo de CPU del grupo de agentes en segundos|Sin dimensiones|
|RunDuration|Sí|Duración de la ejecución|MilliSeconds|Total|Duración de la ejecución en milisegundos|Sin dimensiones|
|StorageUsed|Sí|Almacenamiento utilizado|Bytes|Average|La cantidad de almacenamiento que utiliza el registro de contenedor. Para una cuenta del registro, es la suma de la capacidad que usan todos los repositorios dentro de un registro. Es la suma de la capacidad que usan las capas compartidas, los archivos de manifiesto y las copias de réplica en cada uno de sus repositorios.|Geolocalización|
|SuccessfulPullCount|Sí|Recuento de extracciones correctas|Count|Total|Número de extracciones de imágenes correctas|Sin dimensiones|
|SuccessfulPushCount|Sí|Recuento de inserciones correctas|Count|Total|Número de inserciones de imágenes correctas|Sin dimensiones|
|TotalPullCount|Sí|Recuento total de extracciones|Count|Total|Número de extracciones de imágenes en total|Sin dimensiones|
|TotalPushCount|Sí|Recuento total de inserciones|Count|Total|Número de inserciones de imágenes en total|Sin dimensiones|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|apiserver_current_inflight_requests|No|Solicitudes en proceso|Count|Average|Número máximo de solicitudes en proceso usadas actualmente en apiserver por tipo de solicitud en el último segundo|requestKind|
|cluster_autoscaler_cluster_safe_to_autoscale|No|Estado del clúster|Count|Average|Determina si la escalabilidad automática de clústeres realizará alguna acción en el clúster.|Sin dimensiones|
|cluster_autoscaler_scale_down_in_cooldown|No|Reducción vertical de recuperación|Count|Average|Determina si la reducción vertical está en recuperación: no se quitarán nodos durante este período de tiempo.|Sin dimensiones|
|cluster_autoscaler_unneeded_nodes_count|No|Nodos innecesarios|Count|Average|La escalabilidad automática del clúster marca esos nodos como candidatos para su eliminación y al final se eliminan.|Sin dimensiones|
|cluster_autoscaler_unschedulable_pods_count|No|Pods no programables|Count|Average|Número de pods que actualmente no se pueden programar en el clúster.|Sin dimensiones|
|kube_node_status_allocatable_cpu_cores|No|Número total de núcleos de CPU disponibles en un clúster administrado|Count|Average|Número total de núcleos de CPU disponibles en un clúster administrado|Sin dimensiones|
|kube_node_status_allocatable_memory_bytes|No|Cantidad total de memoria disponible en un clúster administrado|Bytes|Average|Cantidad total de memoria disponible en un clúster administrado|Sin dimensiones|
|kube_node_status_condition|No|Estados de diversas condiciones de nodo|Count|Average|Estados de diversas condiciones de nodo|condition, status, status2, node|
|kube_pod_status_phase|No|Número de pods por fase|Count|Average|Número de pods por fase|fase, espacio de nombres, pod|
|kube_pod_status_ready|No|Número de pods con estado Listo|Count|Average|Número de pods con estado Listo|espacio de nombres, pod, condición|
|node_cpu_usage_millicores|Sí|Milinúcleos de uso de CPU|Milinúcleos|Average|Medida agregada del uso de CPU en milinúcleos en el clúster.|nodo, grupo de nodos|
|node_cpu_usage_percentage|Sí|CPU Usage Percentage (Porcentaje de uso de CPU)|Percent|Average|Uso medio agregado de la CPU medido como porcentaje en el clúster|nodo, grupo de nodos|
|node_disk_usage_bytes|Sí|Bytes usados de disco|Bytes|Average|Espacio en disco usado en bytes por dispositivo|nodo, grupo de nodos, dispositivo|
|node_disk_usage_percentage|Sí|Porcentaje usado de disco|Percent|Average|Espacio en disco usado en porcentaje por dispositivo|nodo, grupo de nodos, dispositivo|
|node_memory_rss_bytes|Sí|Bytes de RSS de memoria|Bytes|Average|Memoria RSS de contenedor usada, en bytes|nodo, grupo de nodos|
|node_memory_rss_percentage|Sí|Porcentaje de memoria RSS|Percent|Average|Memoria RSS del contenedor usada, en porcentaje|nodo, grupo de nodos|
|node_memory_working_set_bytes|Sí|Bytes de memoria del conjunto de trabajo|Bytes|Average|Memoria del conjunto de trabajo de contenedor usada, en bytes|nodo, grupo de nodos|
|node_memory_working_set_percentage|Sí|Porcentaje de memoria de conjunto de trabajo|Percent|Average|Memoria del conjunto de trabajo de contenedor usada, en porcentaje|nodo, grupo de nodos|
|node_network_in_bytes|Sí|Red en bytes|Bytes|Average|Bytes recibidos de la red|nodo, grupo de nodos|
|node_network_out_bytes|Sí|Bytes de salida de la red|Bytes|Average|Bytes transmitidos de la red|nodo, grupo de nodos|


## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft.CustomProviders/resourceproviders

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|FailedRequests|Sí|Solicitudes con error|Count|Total|Obtiene los registros disponibles para proveedores de recursos personalizados|HttpMethod, CallPath, StatusCode|
|SuccessfullRequests|Sí|Solicitudes correctas|Count|Total|Solicitudes correctas realizadas por el proveedor personalizado|HttpMethod, CallPath, StatusCode|


## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AvailableCapacity|Sí|Capacidad disponible|Bytes|Average|La capacidad disponible en bytes durante el período de informe.|Sin dimensiones|
|BytesUploadedToCloud|Sí|Bytes cargados en la nube (dispositivo)|Bytes|Average|El número total de bytes que se carga en Azure desde un dispositivo durante el período de informe.|Sin dimensiones|
|BytesUploadedToCloudPerShare|Sí|Bytes cargados en la nube (recurso compartido)|Bytes|Average|El número total de bytes que se carga en Azure desde un recurso compartido durante el período de informe.|Compartir|
|CloudReadThroughput|Sí|Rendimiento de descarga en la nube|BytesPerSecond|Average|El rendimiento de descarga en la nube en Azure durante el período de informe.|Sin dimensiones|
|CloudReadThroughputPerShare|Sí|Rendimiento de descarga en la nube (recurso compartido)|BytesPerSecond|Average|El rendimiento de descarga en Azure desde un recurso compartido durante el período de informe.|Compartir|
|CloudUploadThroughput|Sí|Rendimiento de carga en la nube|BytesPerSecond|Average|El rendimiento de carga en la nube en Azure durante el período de informe.|Sin dimensiones|
|CloudUploadThroughputPerShare|Sí|Rendimiento de carga en la nube (recurso compartido)|BytesPerSecond|Average|El rendimiento de carga en Azure desde un recurso compartido durante el período de informe.|Compartir|
|HyperVMemoryUtilization|Sí|Proceso perimetral: uso de memoria|Percent|Average|Cantidad de RAM en uso|InstanceName|
|HyperVVirtualProcessorUtilization|Sí|Proceso perimetral: porcentaje de CPU|Percent|Average|Porcentaje de uso de CPU|InstanceName|
|NICReadThroughput|Sí|Rendimiento de lectura (red)|BytesPerSecond|Average|El rendimiento de lectura de la interfaz de red en el dispositivo en el período de informe para todos los volúmenes en la puerta de enlace.|InstanceName|
|NICWriteThroughput|Sí|Rendimiento de escritura (red)|BytesPerSecond|Average|El rendimiento de escritura de la interfaz de red en el dispositivo en el período de informe para todos los volúmenes en la puerta de enlace.|InstanceName|
|TotalCapacity|Sí|Capacidad total|Bytes|Average|La capacidad total del dispositivo en bytes durante el período de informes.|Sin dimensiones|


## <a name="microsoftdatacollaborationworkspaces"></a>Microsoft.DataCollaboration/workspaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|DataAssetCount|Sí|Recursos de datos creados|Count|Máxima|Número de recursos de datos creados|DataAssetName|
|PipelineCount|Sí|Canalizaciones creadas|Count|Máxima|Número de canalizaciones creadas|PipelineName|
|ProposalCount|Sí|Propuestas creadas|Count|Máxima|Número de propuestas creadas|ProposalName|
|ScriptCount|Sí|Scripts creados|Count|Máxima|Número de scripts creados|ScriptName|


## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|FailedRuns|Sí|Ejecuciones con error|Count|Total||pipelineName, activityName|
|SuccessfulRuns|Sí|Ejecuciones correctas|Count|Total||pipelineName, activityName|


## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActivityCancelledRuns|Sí|Cancelled activity runs metrics (Métricas de ejecuciones de actividad canceladas)|Count|Total||ActivityType, PipelineName, FailureType, Name|
|ActivityFailedRuns|Sí|Métricas de ejecuciones de actividad erróneas|Count|Total||ActivityType, PipelineName, FailureType, Name|
|ActivitySucceededRuns|Sí|Métricas de ejecución de actividad realizadas correctamente|Count|Total||ActivityType, PipelineName, FailureType, Name|
|FactorySizeInGbUnits|Sí|Tamaño total de fábrica (unidad de GB)|Count|Máxima||Sin dimensiones|
|IntegrationRuntimeAvailableMemory|Sí|Memoria disponible de entorno de ejecución de integración|Bytes|Average||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableNodeNumber|Sí|Recuento de nodos disponibles de Integration Runtime|Count|Average||IntegrationRuntimeName|
|IntegrationRuntimeAverageTaskPickupDelay|Sí|Integration runtime queue duration (Duración de cola de entorno de ejecución de integración)|Segundos|Average||IntegrationRuntimeName|
|IntegrationRuntimeCpuPercentage|Sí|Uso de la CPU de entorno de ejecución de integración|Percent|Average||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeQueueLength|Sí|Integration runtime queue length (Longitud de cola de entorno de ejecución de integración)|Count|Average||IntegrationRuntimeName|
|MaxAllowedFactorySizeInGbUnits|Sí|Tamaño de fábrica máximo permitido (unidad de GB)|Count|Máxima||Sin dimensiones|
|MaxAllowedResourceCount|Sí|Recuento del máximo de entidades permitidas|Count|Máxima||Sin dimensiones|
|PipelineCancelledRuns|Sí|Cancelled pipeline runs metrics (Métricas de ejecuciones de canalización canceladas)|Count|Total||FailureType, Name|
|PipelineElapsedTimeRuns|Sí|Métricas de ejecuciones de canalización con tiempo transcurrido|Count|Total||RunId, nombre|
|PipelineFailedRuns|Sí|Métricas de ejecuciones de canalización erróneas|Count|Total||FailureType, Name|
|PipelineSucceededRuns|Sí|Las métricas de ejecuciones de canalización se realizaron correctamente|Count|Total||FailureType, Name|
|ResourceCount|Sí|Recuento total de entidades|Count|Máxima||Sin dimensiones|
|SSISIntegrationRuntimeStartCancel|Sí|Métricas de inicio de SSIS Integration Runtime cancelado|Count|Total||IntegrationRuntimeName|
|SSISIntegrationRuntimeStartFailed|Sí|Métricas de inicio de SSIS Integration Runtime con errores|Count|Total||IntegrationRuntimeName|
|SSISIntegrationRuntimeStartSucceeded|Sí|Métricas de inicio de SSIS Integration Runtime correcto|Count|Total||IntegrationRuntimeName|
|SSISIntegrationRuntimeStopStuck|Sí|Métricas de detención de SSIS Integration Runtime bloqueada|Count|Total||IntegrationRuntimeName|
|SSISIntegrationRuntimeStopSucceeded|Sí|Métricas de detención de SSIS Integration Runtime correcta|Count|Total||IntegrationRuntimeName|
|SSISPackageExecutionCancel|Sí|Métricas de ejecuciones de paquetes SSIS canceladas|Count|Total||IntegrationRuntimeName|
|SSISPackageExecutionFailed|Sí|Métricas de ejecución de paquetes SSIS con errores|Count|Total||IntegrationRuntimeName|
|SSISPackageExecutionSucceeded|Sí|Métricas de ejecución de paquetes SSIS correcta|Count|Total||IntegrationRuntimeName|
|TriggerCancelledRuns|Sí|Cancelled trigger runs metrics (Métricas de ejecuciones de desencadenador canceladas)|Count|Total||Name, FailureType|
|TriggerFailedRuns|Sí|Métricas de ejecuciones de desencadenador erróneas|Count|Total||Name, FailureType|
|TriggerSucceededRuns|Sí|Métricas de ejecuciones de desencadenador realizadas correctamente|Count|Total||Name, FailureType|


## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|JobAUEndedCancelled|Sí|Tiempo AU para cancelados|Segundos|Total|Tiempo total AU para trabajos cancelados|Sin dimensiones|
|JobAUEndedFailure|Sí|Error de tiempo AU|Segundos|Total|Tiempo total AU para trabajos con error|Sin dimensiones|
|JobAUEndedSuccess|Sí|Tiempo AU correcto|Segundos|Total|Tiempo total AU para trabajos realizados correctamente|Sin dimensiones|
|JobEndedCancelled|Sí|Trabajos cancelados|Count|Total|Recuento de trabajos cancelados|Sin dimensiones|
|JobEndedFailure|Sí|Trabajos con error|Count|Total|Recuento de trabajos con error|Sin dimensiones|
|JobEndedSuccess|Sí|Trabajos correctos|Count|Total|Recuento de trabajos realizados correctamente|Sin dimensiones|
|JobStage|Sí|Trabajos en fase|Count|Total|Número de trabajos en cada fase.|Sin dimensiones|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|DataRead|Sí|Datos leídos|Bytes|Total|Cantidad total de datos leídos de la cuenta.|Sin dimensiones|
|DataWritten|Sí|Datos escritos|Bytes|Total|Cantidad total de datos escritos en la cuenta.|Sin dimensiones|
|ReadRequests|Sí|Solicitudes de lectura|Count|Total|Número de solicitudes de lectura de datos de la cuenta.|Sin dimensiones|
|TotalStorage|Sí|Almacenamiento total|Bytes|Máxima|Cantidad total de datos almacenados en la cuenta.|Sin dimensiones|
|WriteRequests|Sí|Solicitudes de escritura|Count|Total|Número de solicitudes de escritura de datos en la cuenta.|Sin dimensiones|


## <a name="microsoftdatashareaccounts"></a>Microsoft.DataShare/accounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|FailedShareSubscriptionSynchronizations|Sí|Instantáneas de recurso compartido recibido con error|Count|Count|Número de instantáneas de recurso compartido recibido con error en la cuenta|Sin dimensiones|
|FailedShareSynchronizations|Sí|Instantáneas de recurso compartido enviado con error|Count|Count|Número de instantáneas de recurso compartido enviado con error en la cuenta|Sin dimensiones|
|ShareCount|Sí|Recursos compartidos enviados|Count|Máxima|Número de recursos compartidos enviados en la cuenta|ShareName|
|ShareSubscriptionCount|Sí|Recursos compartidos recibidos|Count|Máxima|Número de recursos compartidos recibidos en la cuenta|ShareSubscriptionName|
|SucceededShareSubscriptionSynchronizations|Sí|Instantáneas de recurso compartido recibido correctamente|Count|Count|Número de instantáneas de recurso compartido recibido correctamente en la cuenta|Sin dimensiones|
|SucceededShareSynchronizations|Sí|Instantáneas de recurso compartido enviado correctamente|Count|Count|Número de instantáneas de recurso compartido enviado correctamente en la cuenta|Sin dimensiones|


## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Sí|Conexiones activas|Count|Average|Conexiones activas|Sin dimensiones|
|backup_storage_used|Sí|Almacenamiento de copia de seguridad utilizado|Bytes|Average|Almacenamiento de copia de seguridad utilizado|Sin dimensiones|
|connections_failed|Sí|Conexiones con errores|Count|Total|Conexiones con errores|Sin dimensiones|
|cpu_percent|Sí|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Sin dimensiones|
|io_consumption_percent|Sí|Porcentaje de E/S|Percent|Average|Porcentaje de E/S|Sin dimensiones|
|memory_percent|Sí|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|Sin dimensiones|
|network_bytes_egress|Sí|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Sí|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|
|seconds_behind_master|Sí|Intervalo de replicación en segundos|Count|Máxima|Intervalo de replicación en segundos|Sin dimensiones|
|serverlog_storage_limit|Sí|Límite de almacenamiento del registro del servidor|Bytes|Máxima|Límite de almacenamiento del registro del servidor|Sin dimensiones|
|serverlog_storage_percent|Sí|Porcentaje de almacenamiento del registro del servidor|Percent|Average|Porcentaje de almacenamiento del registro del servidor|Sin dimensiones|
|serverlog_storage_usage|Sí|Almacenamiento del registro del servidor usado|Bytes|Average|Almacenamiento del registro del servidor usado|Sin dimensiones|
|storage_limit|Sí|Límite de almacenamiento|Bytes|Máxima|Límite de almacenamiento|Sin dimensiones|
|storage_percent|Sí|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Sí|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|Sin dimensiones|


## <a name="microsoftdbformysqlflexibleservers"></a>Microsoft.DBforMySQL/flexibleServers

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|aborted_connections|Sí|Conexiones anuladas|Count|Total|Conexiones anuladas|Sin dimensiones|
|active_connections|Sí|Conexiones activas|Count|Máxima|Conexiones activas|Sin dimensiones|
|backup_storage_used|Sí|Almacenamiento de copia de seguridad utilizado|Bytes|Máxima|Almacenamiento de copia de seguridad utilizado|Sin dimensiones|
|cpu_credits_consumed|Sí|Créditos de CPU consumidos|Count|Máxima|Créditos de CPU consumidos|Sin dimensiones|
|cpu_credits_remaining|Sí|Créditos de CPU restantes|Count|Máxima|Créditos de CPU restantes|Sin dimensiones|
|cpu_percent|Sí|Porcentaje de CPU de host|Percent|Máxima|Porcentaje de CPU de host|Sin dimensiones|
|io_consumption_percent|Sí|Porcentaje de E/S|Percent|Máxima|Porcentaje de E/S|Sin dimensiones|
|memory_percent|Sí|Porcentaje de memoria de host|Percent|Máxima|Porcentaje de memoria de host|Sin dimensiones|
|network_bytes_egress|Sí|Salida de red de host|Bytes|Total|Salida de red del host en bytes|Sin dimensiones|
|network_bytes_ingress|Sí|Entrada de red de host|Bytes|Total|Entrada de red del host en bytes|Sin dimensiones|
|Consultas|Sí|Consultas|Count|Total|Consultas|Sin dimensiones|
|replication_lag|Sí|Intervalo de replicación en segundos|Segundos|Máxima|Intervalo de replicación en segundos|Sin dimensiones|
|storage_limit|Sí|Límite de almacenamiento|Bytes|Máxima|Límite de almacenamiento|Sin dimensiones|
|storage_percent|Sí|Porcentaje de almacenamiento|Percent|Máxima|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Sí|Almacenamiento utilizado|Bytes|Máxima|Almacenamiento utilizado|Sin dimensiones|
|total_connections|Sí|Conexiones totales|Count|Total|Conexiones totales|Sin dimensiones|


## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Sí|Conexiones activas|Count|Average|Conexiones activas|Sin dimensiones|
|backup_storage_used|Sí|Almacenamiento de copia de seguridad utilizado|Bytes|Average|Almacenamiento de copia de seguridad utilizado|Sin dimensiones|
|connections_failed|Sí|Conexiones con errores|Count|Total|Conexiones con errores|Sin dimensiones|
|cpu_percent|Sí|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Sin dimensiones|
|io_consumption_percent|Sí|Porcentaje de E/S|Percent|Average|Porcentaje de E/S|Sin dimensiones|
|memory_percent|Sí|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|Sin dimensiones|
|network_bytes_egress|Sí|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Sí|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|
|seconds_behind_master|Sí|Intervalo de replicación en segundos|Count|Máxima|Intervalo de replicación en segundos|Sin dimensiones|
|serverlog_storage_limit|Sí|Límite de almacenamiento del registro del servidor|Bytes|Máxima|Límite de almacenamiento del registro del servidor|Sin dimensiones|
|serverlog_storage_percent|Sí|Porcentaje de almacenamiento del registro del servidor|Percent|Average|Porcentaje de almacenamiento del registro del servidor|Sin dimensiones|
|serverlog_storage_usage|Sí|Almacenamiento del registro del servidor usado|Bytes|Average|Almacenamiento del registro del servidor usado|Sin dimensiones|
|storage_limit|Sí|Límite de almacenamiento|Bytes|Máxima|Límite de almacenamiento|Sin dimensiones|
|storage_percent|Sí|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Sí|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|Sin dimensiones|


## <a name="microsoftdbforpostgresqlflexibleservers"></a>Microsoft.DBforPostgreSQL/flexibleServers

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Sí|Conexiones activas|Count|Average|Conexiones activas|Sin dimensiones|
|backup_storage_used|Sí|Almacenamiento de copia de seguridad utilizado|Bytes|Average|Almacenamiento de copia de seguridad utilizado|Sin dimensiones|
|connections_failed|Sí|Conexiones con errores|Count|Total|Conexiones con errores|Sin dimensiones|
|connections_succeeded|Sí|Conexiones correctas|Count|Total|Conexiones correctas|Sin dimensiones|
|cpu_credits_consumed|Sí|Créditos de CPU consumidos|Count|Average|Número total de créditos consumidos por el servidor de bases de datos|Sin dimensiones|
|cpu_credits_remaining|Sí|Créditos de CPU restantes|Count|Average|Número total de créditos disponibles para irrumpir|Sin dimensiones|
|cpu_percent|Sí|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Sin dimensiones|
|disk_queue_depth|Sí|Profundidad de la cola de discos|Count|Average|Número de operaciones de E/S pendiente en el disco de datos|Sin dimensiones|
|iops|Sí|E/S|Count|Average|Operaciones de E/S por segundo|Sin dimensiones|
|maximum_used_transactionIDs|Sí|Máximo de identificadores de transacción usados|Count|Average|Máximo de identificadores de transacción usados|Sin dimensiones|
|memory_percent|Sí|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|Sin dimensiones|
|network_bytes_egress|Sí|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Sí|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|
|read_iops|Sí|IOPS de lectura|Count|Average|Número de operaciones de lectura de E/S de disco de datos por segundo|Sin dimensiones|
|read_throughput|Sí|Rendimiento de lectura en bytes/seg.|Count|Average|Bytes leídos por segundo del disco de datos durante el período de supervisión|Sin dimensiones|
|storage_free|Sí|Almacenamiento libre|Bytes|Average|Almacenamiento libre|Sin dimensiones|
|storage_percent|Sí|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Sí|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|Sin dimensiones|
|txlogs_storage_used|Sí|Almacenamiento del registro de transacciones usado|Bytes|Average|Almacenamiento del registro de transacciones usado|Sin dimensiones|
|write_iops|Sí|IOPS de escritura|Count|Average|Número de operaciones de escritura de E/S de disco de datos por segundo|Sin dimensiones|
|write_throughput|Sí|Rendimiento de escritura en bytes/seg.|Count|Average|Bytes escritos por segundo en el disco de datos durante el período de supervisión|Sin dimensiones|


## <a name="microsoftdbforpostgresqlservergroupsv2"></a>Microsoft.DBForPostgreSQL/serverGroupsv2

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Sí|Conexiones activas|Count|Average|Conexiones activas|nombreDeServidor|
|cpu_percent|Sí|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|nombreDeServidor|
|iops|Sí|E/S|Count|Average|Operaciones de E/S por segundo|nombreDeServidor|
|memory_percent|Sí|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|nombreDeServidor|
|network_bytes_egress|Sí|Red interna|Bytes|Total|Red externa a través de conexiones activas|nombreDeServidor|
|network_bytes_ingress|Sí|Red interna|Bytes|Total|Red interna a través de conexiones activas|nombreDeServidor|
|storage_percent|Sí|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|nombreDeServidor|
|storage_used|Sí|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|nombreDeServidor|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Sí|Conexiones activas|Count|Average|Conexiones activas|Sin dimensiones|
|backup_storage_used|Sí|Almacenamiento de copia de seguridad utilizado|Bytes|Average|Almacenamiento de copia de seguridad utilizado|Sin dimensiones|
|connections_failed|Sí|Conexiones con errores|Count|Total|Conexiones con errores|Sin dimensiones|
|cpu_percent|Sí|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Sin dimensiones|
|io_consumption_percent|Sí|Porcentaje de E/S|Percent|Average|Porcentaje de E/S|Sin dimensiones|
|memory_percent|Sí|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|Sin dimensiones|
|network_bytes_egress|Sí|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Sí|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|
|pg_replica_log_delay_in_bytes|Sí|Retraso máximo entre réplicas|Bytes|Máxima|Retardo en bytes de la réplica con más retraso|Sin dimensiones|
|pg_replica_log_delay_in_seconds|Sí|Retraso entre réplicas|Segundos|Máxima|Intervalo de replicación en segundos|Sin dimensiones|
|serverlog_storage_limit|Sí|Límite de almacenamiento del registro del servidor|Bytes|Máxima|Límite de almacenamiento del registro del servidor|Sin dimensiones|
|serverlog_storage_percent|Sí|Porcentaje de almacenamiento del registro del servidor|Percent|Average|Porcentaje de almacenamiento del registro del servidor|Sin dimensiones|
|serverlog_storage_usage|Sí|Almacenamiento del registro del servidor usado|Bytes|Average|Almacenamiento del registro del servidor usado|Sin dimensiones|
|storage_limit|Sí|Límite de almacenamiento|Bytes|Máxima|Límite de almacenamiento|Sin dimensiones|
|storage_percent|Sí|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Sí|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|Sin dimensiones|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Sí|Conexiones activas|Count|Average|Conexiones activas|Sin dimensiones|
|cpu_percent|Sí|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Sin dimensiones|
|iops|Sí|E/S|Count|Average|Operaciones de E/S por segundo|Sin dimensiones|
|memory_percent|Sí|Porcentaje de memoria|Percent|Average|Porcentaje de memoria|Sin dimensiones|
|network_bytes_egress|Sí|Red interna|Bytes|Total|Red externa a través de conexiones activas|Sin dimensiones|
|network_bytes_ingress|Sí|Red interna|Bytes|Total|Red interna a través de conexiones activas|Sin dimensiones|
|storage_percent|Sí|Porcentaje de almacenamiento|Percent|Average|Porcentaje de almacenamiento|Sin dimensiones|
|storage_used|Sí|Almacenamiento utilizado|Bytes|Average|Almacenamiento utilizado|Sin dimensiones|


## <a name="microsoftdeviceselasticpools"></a>Microsoft.Devices/ElasticPools

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|elasticPool.requestedUsageRate|Sí|tasa de uso solicitado|Percent|Average|tasa de uso solicitado|Sin dimensiones|


## <a name="microsoftdeviceselasticpoolsiothubtenants"></a>Microsoft.Devices/ElasticPools/IotHubTenants

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|c2d.commands.egress.abandon.success|Sí|C2D messages abandoned (Mensajes de C2D abandonados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha abandonado.|Sin dimensiones|
|c2d.commands.egress.complete.success|Sí|C2D message deliveries completed (Entregas de mensajes de C2D completadas)|Count|Total|Número de entregas de mensajes de la nube al dispositivo que el dispositivo ha completado correctamente.|Sin dimensiones|
|c2d.commands.egress.reject.success|Sí|C2D messages rejected (Mensajes de C2D rechazados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha rechazado.|Sin dimensiones|
|c2d.methods.failure|Sí|Invocaciones al método directo con error|Count|Total|El número de todas las llamadas al método directo con error.|Sin dimensiones|
|c2d.methods.requestSize|Sí|Tamaño de la solicitud de las invocaciones a métodos directos|Bytes|Average|El valor medio, mínimo y máximo de todas las solicitudes correctas de método directo.|Sin dimensiones|
|c2d.methods.responseSize|Sí|Tamaño de la respuesta de las invocaciones a métodos directos|Bytes|Average|El valor medio, mínimo y máximo de todas las respuestas correctas de método directo.|Sin dimensiones|
|c2d.methods.success|Sí|Invocaciones correctas al método directo|Count|Total|El número de todas las llamadas correctas al método directo.|Sin dimensiones|
|c2d.twin.read.failure|Sí|Lecturas gemelas con error del back-end|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.read.size|Sí|Tamaño de la respuesta de las lecturas gemelas del back-end|Bytes|Average|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.read.success|Sí|Lecturas gemelas correctas del back-end|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.failure|Sí|Actualizaciones gemelas con error del back-end|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.size|Sí|Tamaño de las actualizaciones gemelas del back-end|Bytes|Average|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.success|Sí|Actualizaciones gemelas correctas del back-end|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|C2DMessagesExpired|Sí|Mensajes de C2D expirados|Count|Total|Número de mensajes de la nube al dispositivo expirados|Sin dimensiones|
|configuraciones|Sí|Métricas de configuración|Count|Total|Métricas de las operaciones de configuración|Sin dimensiones|
|connectedDeviceCount|Sí|Dispositivos conectados|Count|Average|Número de dispositivos conectados a IoT Hub|Sin dimensiones|
|d2c.endpoints.egress.builtIn.events|Sí|Enrutamiento: mensajes entregados a mensajes/eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente al punto de conexión integrado (mensajes/eventos).|Sin dimensiones|
|d2c.endpoints.egress.eventHubs|Sí|Enrutamiento: mensajes entregados al centro de eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del centro de eventos.|Sin dimensiones|
|d2c.endpoints.egress.serviceBusQueues|Sí|Enrutamiento: mensajes entregados a la cola de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de la cola de Service Bus.|Sin dimensiones|
|d2c.endpoints.egress.serviceBusTopics|Sí|Enrutamiento: mensajes entregados al tema de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del tema de Service Bus.|Sin dimensiones|
|d2c.endpoints.egress.storage|Sí|Enrutamiento: mensajes entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.egress.storage.blobs|Sí|Enrutamiento: blobs entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado blobs a los puntos de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.egress.storage.bytes|Sí|Enrutamiento: datos entregados al almacenamiento|Bytes|Total|Cantidad de datos (bytes) que el enrutamiento de IoT Hub ha entregado a los punto de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.latency.builtIn.events|Sí|Enrutamiento: latencia de mensajes para mensajes/eventos|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría al punto de conexión integrado (mensajes/eventos).|Sin dimensiones|
|d2c.endpoints.latency.eventHubs|Sí|Enrutamiento: latencia de mensajes para el centro de eventos|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a un punto de conexión del centro de eventos.|Sin dimensiones|
|d2c.endpoints.latency.serviceBusQueues|Sí|Enrutamiento: latencia de mensajes de la cola de Service Bus|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de la cola de Service Bus.|Sin dimensiones|
|d2c.endpoints.latency.serviceBusTopics|Sí|Enrutamiento: latencia de mensajes del tema de Service Bus|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión del tema de Service Bus.|Sin dimensiones|
|d2c.endpoints.latency.storage|Sí|Enrutamiento: latencia de mensajes para almacenamiento|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de almacenamiento.|Sin dimensiones|
|d2c.telemetry.egress.dropped|Sí|Enrutamiento: mensajes de telemetría quitados |Count|Total|Número de veces que se han quitado los mensajes con el enrutamiento de IoT Hub a causa de puntos de conexión inactivos. Este valor no cuenta los mensajes entregados a la ruta de reserva porque los mensajes quitados no se entregan allí.|Sin dimensiones|
|d2c.telemetry.egress.fallback|Sí|Enrutamiento: mensajes entregados a la reserva|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes al punto de conexión asociado con la ruta de reserva.|Sin dimensiones|
|d2c.telemetry.egress.invalid|Sí|Enrutamiento: mensajes de telemetría incompatibles|Count|Total|Número de veces que el enrutamiento de IoT Hub no ha podido entregar mensajes debido a una incompatibilidad con el punto de conexión. Este valor no incluye los reintentos.|Sin dimensiones|
|d2c.telemetry.egress.orphaned|Sí|Enrutamiento: mensajes de telemetría huérfanos |Count|Total|Número de veces que los mensajes eran huérfanos a causa del enrutamiento de IoT Hub porque no coincidían con las reglas de enrutamiento (incluida la regla de reserva). |Sin dimensiones|
|d2c.telemetry.egress.success|Sí|Enrutamiento: mensajes de telemetría entregados|Count|Total|Número de veces que los mensajes se han entregado correctamente a todos los puntos de conexión mediante el enrutamiento de IoT Hub. Si un mensaje se enruta a varios puntos de conexión, este valor aumenta en uno por cada entrega correcta. Si un mensaje se enruta al mismo punto de conexión varias veces, este valor aumenta en uno por cada entrega correcta.|Sin dimensiones|
|d2c.telemetry.ingress.allProtocol|Sí|Intentos de envío de mensajes de telemetría|Count|Total|Número de mensajes de telemetría de dispositivo a la nube para enviar a IoT Hub|Sin dimensiones|
|d2c.telemetry.ingress.sendThrottle|Sí|Número de errores de limitación|Count|Total|Número de errores de limitación debido a las limitaciones de rendimiento del dispositivo|Sin dimensiones|
|d2c.telemetry.ingress.success|Sí|Mensajes de telemetría enviados|Count|Total|Número de mensajes de telemetría de dispositivo a la nube enviados correctamente a IoT Hub|Sin dimensiones|
|d2c.twin.read.failure|Sí|Lecturas gemelas con error de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.read.size|Sí|Tamaño de la respuesta de las lecturas gemelas de dispositivos|Bytes|Average|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.read.success|Sí|Lecturas gemelas correctas de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.failure|Sí|Actualizaciones gemelas con error de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.size|Sí|Tamaño de las actualizaciones gemelas de los dispositivos|Bytes|Average|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.success|Sí|Actualizaciones gemelas correctas de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|dailyMessageQuotaUsed|Sí|Número total de mensajes usados|Count|Máxima|Número de mensajes totales usados hoy|Sin dimensiones|
|deviceDataUsage|Sí|Uso total de datos del dispositivo|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|Sin dimensiones|
|deviceDataUsageV2|Sí|Uso total de datos del dispositivo (versión preliminar)|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|Sin dimensiones|
|devices.connectedDevices.allProtocol|Sí|Dispositivos conectados (en desuso) |Count|Total|Número de dispositivos conectados a IoT Hub|Sin dimensiones|
|devices.totalDevices|Sí|Total de dispositivos (en desuso)|Count|Total|Número de dispositivos registrados en IoT Hub|Sin dimensiones|
|EventGridDeliveries|Sí|Entregas de Event Grid|Count|Total|El número de eventos de IoT Hub publicados en Event Grid. Use la dimensión de resultado para ver el número de solicitudes correctas e incorrectas. La dimensión EventType muestra el tipo de evento (https://aka.ms/ioteventgrid).|Result, EventType|
|EventGridLatency|Sí|Latencia de Event Grid|Milisegundos|Average|La latencia media (milisegundos) desde el momento en que se generó el evento de IoT Hub hasta el momento en que se publicó el evento en Event Grid. Este número es un promedio entre todos los tipos de evento. Use la dimensión EventType para ver la latencia de un tipo específico de evento.|EventType|
|jobs.cancelJob.failure|Sí|Cancelaciones de trabajos con error|Count|Total|El recuento de todas las llamadas con error para cancelar un trabajo.|Sin dimensiones|
|jobs.cancelJob.success|Sí|Cancelaciones de trabajos correctas|Count|Total|El recuento de todas las llamadas correctas para cancelar un trabajo.|Sin dimensiones|
|jobs.completed|Sí|Trabajos completados|Count|Total|El recuento de todos los trabajos completados.|Sin dimensiones|
|jobs.createDirectMethodJob.failure|Sí|Creaciones con error de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones con error de los trabajos de invocación de método directos.|Sin dimensiones|
|jobs.createDirectMethodJob.success|Sí|Creaciones correctas de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones correctas de los trabajos de invocación de método directos.|Sin dimensiones|
|jobs.createTwinUpdateJob.failure|Sí|Creaciones con error de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones con error de trabajos de actualización gemelos.|Sin dimensiones|
|jobs.createTwinUpdateJob.success|Sí|Creaciones correctas de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones correctas de trabajos de actualización gemelos.|Sin dimensiones|
|jobs.failed|Sí|Trabajos con error|Count|Total|El recuento de todos los trabajos con error.|Sin dimensiones|
|jobs.listJobs.failure|Sí|Llamadas con error para enumerar trabajos|Count|Total|El recuento de todas las llamadas con error para enumerar trabajos.|Sin dimensiones|
|jobs.listJobs.success|Sí|Llamadas correctas para enumerar trabajos|Count|Total|El recuento de todas las llamadas correctas para enumerar trabajos.|Sin dimensiones|
|jobs.queryJobs.failure|Sí|Consultas de trabajo con error|Count|Total|El recuento de todas las llamadas con error para consultar trabajos.|Sin dimensiones|
|jobs.queryJobs.success|Sí|Consultas de trabajo correctas|Count|Total|El recuento de todas las llamadas correctas para consultar trabajos.|Sin dimensiones|
|RoutingDataSizeInBytesDelivered|Sí|Tamaño del enrutamiento de los mensajes entregados en bytes (versión preliminar)|Bytes|Total|Tamaño total en bytes de los mensajes entregados por un centro de IoT a un punto de conexión. También puede usar las dimensiones EndpointName y EndpointType para ver el tamaño de los mensajes en bytes entregados a los distintos puntos de conexión. El valor de la métrica se incrementa por cada mensaje entregado, incluido si el mensaje se entrega a varios puntos de conexión o si el mensaje se entrega varias veces en el mismo punto de conexión.|EndpointType, EndpointName, RoutingSource|
|RoutingDeliveries|Sí|Entregas de enrutamiento (versión preliminar)|Count|Total|Número de veces que IoT Hub intentó la entrega de mensajes a todos los puntos de conexión mediante el enrutamiento. Para ver el número de intentos completados correctamente o con error, use la dimensión Result. Para ver el motivo del error, como no válido, quitado o huérfano, use la dimensión FailureReasonCategory. También puede usar las dimensiones EndpointName y EndpointType para entender cuántos mensajes se entregaron a los distintos puntos de conexión. El valor de la métrica se incrementa en uno por cada intento de entrega, incluido si el mensaje se entrega a varios puntos de conexión o si el mensaje se entrega varias veces en el mismo punto de conexión.|EndpointType, EndpointName, FailureReasonCategory, Result, RoutingSource|
|RoutingDeliveryLatency|Sí|Latencia de entrega de enrutamiento (versión preliminar)|Milisegundos|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión. Puede usar las dimensiones EndpointName y EndpointType para comprender la latencia de los distintos puntos de conexión.|EndpointType, EndpointName, RoutingSource|
|tenantHub.requestedUsageRate|Sí|tasa de uso solicitado|Percent|Average|tasa de uso solicitado|Sin dimensiones|
|totalDeviceCount|Sí|Número total de dispositivos|Count|Average|Número de dispositivos registrados en IoT Hub|Sin dimensiones|
|twinQueries.failure|Sí|Consultas gemelas con error|Count|Total|El recuento de todas las consultas gemelas con error.|Sin dimensiones|
|twinQueries.resultSize|Sí|Tamaño de resultado de consultas gemelas|Bytes|Average|El valor medio, mínimo y máximo del tamaño del resultado de todas las consultas gemelas correctas.|Sin dimensiones|
|twinQueries.success|Sí|Consultas gemelas correctas|Count|Total|El recuento de todas las consultas gemelas correctas.|Sin dimensiones|


## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|c2d.commands.egress.abandon.success|Sí|C2D messages abandoned (Mensajes de C2D abandonados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha abandonado.|Sin dimensiones|
|c2d.commands.egress.complete.success|Sí|C2D message deliveries completed (Entregas de mensajes de C2D completadas)|Count|Total|Número de entregas de mensajes de la nube al dispositivo que el dispositivo ha completado correctamente.|Sin dimensiones|
|c2d.commands.egress.reject.success|Sí|C2D messages rejected (Mensajes de C2D rechazados)|Count|Total|Número de mensajes de la nube al dispositivo que el dispositivo ha rechazado.|Sin dimensiones|
|c2d.methods.failure|Sí|Invocaciones al método directo con error|Count|Total|El número de todas las llamadas al método directo con error.|Sin dimensiones|
|c2d.methods.requestSize|Sí|Tamaño de la solicitud de las invocaciones a métodos directos|Bytes|Average|El valor medio, mínimo y máximo de todas las solicitudes correctas de método directo.|Sin dimensiones|
|c2d.methods.responseSize|Sí|Tamaño de la respuesta de las invocaciones a métodos directos|Bytes|Average|El valor medio, mínimo y máximo de todas las respuestas correctas de método directo.|Sin dimensiones|
|c2d.methods.success|Sí|Invocaciones correctas al método directo|Count|Total|El número de todas las llamadas correctas al método directo.|Sin dimensiones|
|c2d.twin.read.failure|Sí|Lecturas gemelas con error del back-end|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.read.size|Sí|Tamaño de la respuesta de las lecturas gemelas del back-end|Bytes|Average|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.read.success|Sí|Lecturas gemelas correctas del back-end|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.failure|Sí|Actualizaciones gemelas con error del back-end|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.size|Sí|Tamaño de las actualizaciones gemelas del back-end|Bytes|Average|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|c2d.twin.update.success|Sí|Actualizaciones gemelas correctas del back-end|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el back-end.|Sin dimensiones|
|C2DMessagesExpired|Sí|Mensajes de C2D expirados|Count|Total|Número de mensajes de la nube al dispositivo expirados|Sin dimensiones|
|configuraciones|Sí|Métricas de configuración|Count|Total|Métricas de las operaciones de configuración|Sin dimensiones|
|connectedDeviceCount|No|Dispositivos conectados|Count|Average|Número de dispositivos conectados a IoT Hub|Sin dimensiones|
|d2c.endpoints.egress.builtIn.events|Sí|Enrutamiento: mensajes entregados a mensajes/eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente al punto de conexión integrado (mensajes/eventos).|Sin dimensiones|
|d2c.endpoints.egress.eventHubs|Sí|Enrutamiento: mensajes entregados al centro de eventos|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del centro de eventos.|Sin dimensiones|
|d2c.endpoints.egress.serviceBusQueues|Sí|Enrutamiento: mensajes entregados a la cola de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de la cola de Service Bus.|Sin dimensiones|
|d2c.endpoints.egress.serviceBusTopics|Sí|Enrutamiento: mensajes entregados al tema de Service Bus|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión del tema de Service Bus.|Sin dimensiones|
|d2c.endpoints.egress.storage|Sí|Enrutamiento: mensajes entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes correctamente a los puntos de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.egress.storage.blobs|Sí|Enrutamiento: blobs entregados al almacenamiento|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado blobs a los puntos de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.egress.storage.bytes|Sí|Enrutamiento: datos entregados al almacenamiento|Bytes|Total|Cantidad de datos (bytes) que el enrutamiento de IoT Hub ha entregado a los punto de conexión de almacenamiento.|Sin dimensiones|
|d2c.endpoints.latency.builtIn.events|Sí|Enrutamiento: latencia de mensajes para mensajes/eventos|MilliSeconds|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría al punto de conexión integrado (mensajes/eventos).|Sin dimensiones|
|d2c.endpoints.latency.eventHubs|Sí|Enrutamiento: latencia de mensajes para el centro de eventos|MilliSeconds|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes a un punto de conexión del centro de eventos.|Sin dimensiones|
|d2c.endpoints.latency.serviceBusQueues|Sí|Enrutamiento: latencia de mensajes de la cola de Service Bus|MilliSeconds|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de la cola de Service Bus.|Sin dimensiones|
|d2c.endpoints.latency.serviceBusTopics|Sí|Enrutamiento: latencia de mensajes del tema de Service Bus|MilliSeconds|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión del tema de Service Bus.|Sin dimensiones|
|d2c.endpoints.latency.storage|Sí|Enrutamiento: latencia de mensajes para almacenamiento|MilliSeconds|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión de almacenamiento.|Sin dimensiones|
|d2c.telemetry.egress.dropped|Sí|Enrutamiento: mensajes de telemetría quitados |Count|Total|Número de veces que se han quitado los mensajes con el enrutamiento de IoT Hub a causa de puntos de conexión inactivos. Este valor no cuenta los mensajes entregados a la ruta de reserva porque los mensajes quitados no se entregan allí.|Sin dimensiones|
|d2c.telemetry.egress.fallback|Sí|Enrutamiento: mensajes entregados a la reserva|Count|Total|Número de veces que el enrutamiento de IoT Hub ha entregado mensajes al punto de conexión asociado con la ruta de reserva.|Sin dimensiones|
|d2c.telemetry.egress.invalid|Sí|Enrutamiento: mensajes de telemetría incompatibles|Count|Total|Número de veces que el enrutamiento de IoT Hub no ha podido entregar mensajes debido a una incompatibilidad con el punto de conexión. Este valor no incluye los reintentos.|Sin dimensiones|
|d2c.telemetry.egress.orphaned|Sí|Enrutamiento: mensajes de telemetría huérfanos |Count|Total|Número de veces que los mensajes eran huérfanos a causa del enrutamiento de IoT Hub porque no coincidían con las reglas de enrutamiento (incluida la regla de reserva). |Sin dimensiones|
|d2c.telemetry.egress.success|Sí|Enrutamiento: mensajes de telemetría entregados|Count|Total|Número de veces que los mensajes se han entregado correctamente a todos los puntos de conexión mediante el enrutamiento de IoT Hub. Si un mensaje se enruta a varios puntos de conexión, este valor aumenta en uno por cada entrega correcta. Si un mensaje se enruta al mismo punto de conexión varias veces, este valor aumenta en uno por cada entrega correcta.|Sin dimensiones|
|d2c.telemetry.ingress.allProtocol|Sí|Intentos de envío de mensajes de telemetría|Count|Total|Número de mensajes de telemetría de dispositivo a la nube para enviar a IoT Hub|Sin dimensiones|
|d2c.telemetry.ingress.sendThrottle|Sí|Número de errores de limitación|Count|Total|Número de errores de limitación debido a las limitaciones de rendimiento del dispositivo|Sin dimensiones|
|d2c.telemetry.ingress.success|Sí|Mensajes de telemetría enviados|Count|Total|Número de mensajes de telemetría de dispositivo a la nube enviados correctamente a IoT Hub|Sin dimensiones|
|d2c.twin.read.failure|Sí|Lecturas gemelas con error de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas con error iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.read.size|Sí|Tamaño de la respuesta de las lecturas gemelas de dispositivos|Bytes|Average|El valor medio, mínimo y máximo de todas las lecturas gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.read.success|Sí|Lecturas gemelas correctas de los dispositivos|Count|Total|El recuento de todas las lecturas gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.failure|Sí|Actualizaciones gemelas con error de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas con error iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.size|Sí|Tamaño de las actualizaciones gemelas de los dispositivos|Bytes|Average|El tamaño medio, mínimo y máximo de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|d2c.twin.update.success|Sí|Actualizaciones gemelas correctas de los dispositivos|Count|Total|El recuento de todas las actualizaciones gemelas correctas iniciadas por el dispositivo.|Sin dimensiones|
|dailyMessageQuotaUsed|Sí|Número total de mensajes usados|Count|Máxima|Número de mensajes totales usados hoy|Sin dimensiones|
|deviceDataUsage|Sí|Uso total de datos del dispositivo|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|Sin dimensiones|
|deviceDataUsageV2|Sí|Uso total de datos del dispositivo (versión preliminar)|Bytes|Total|Bytes transferidos hacia y desde cualquier dispositivo conectado al Centro de IOT|Sin dimensiones|
|devices.connectedDevices.allProtocol|Sí|Dispositivos conectados (en desuso) |Count|Total|Número de dispositivos conectados a IoT Hub|Sin dimensiones|
|devices.totalDevices|Sí|Total de dispositivos (en desuso)|Count|Total|Número de dispositivos registrados en IoT Hub|Sin dimensiones|
|EventGridDeliveries|Sí|Entregas de Event Grid|Count|Total|El número de eventos de IoT Hub publicados en Event Grid. Use la dimensión de resultado para ver el número de solicitudes correctas e incorrectas. La dimensión EventType muestra el tipo de evento (https://aka.ms/ioteventgrid).|Result, EventType|
|EventGridLatency|Sí|Latencia de Event Grid|MilliSeconds|Average|La latencia media (milisegundos) desde el momento en que se generó el evento de IoT Hub hasta el momento en que se publicó el evento en Event Grid. Este número es un promedio entre todos los tipos de evento. Use la dimensión EventType para ver la latencia de un tipo específico de evento.|EventType|
|jobs.cancelJob.failure|Sí|Cancelaciones de trabajos con error|Count|Total|El recuento de todas las llamadas con error para cancelar un trabajo.|Sin dimensiones|
|jobs.cancelJob.success|Sí|Cancelaciones de trabajos correctas|Count|Total|El recuento de todas las llamadas correctas para cancelar un trabajo.|Sin dimensiones|
|jobs.completed|Sí|Trabajos completados|Count|Total|El recuento de todos los trabajos completados.|Sin dimensiones|
|jobs.createDirectMethodJob.failure|Sí|Creaciones con error de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones con error de los trabajos de invocación de método directos.|Sin dimensiones|
|jobs.createDirectMethodJob.success|Sí|Creaciones correctas de trabajos de invocación de método|Count|Total|El recuento de todas las creaciones correctas de los trabajos de invocación de método directos.|Sin dimensiones|
|jobs.createTwinUpdateJob.failure|Sí|Creaciones con error de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones con error de trabajos de actualización gemelos.|Sin dimensiones|
|jobs.createTwinUpdateJob.success|Sí|Creaciones correctas de trabajos de actualización gemelos|Count|Total|El recuento de todas las creaciones correctas de trabajos de actualización gemelos.|Sin dimensiones|
|jobs.failed|Sí|Trabajos con error|Count|Total|El recuento de todos los trabajos con error.|Sin dimensiones|
|jobs.listJobs.failure|Sí|Llamadas con error para enumerar trabajos|Count|Total|El recuento de todas las llamadas con error para enumerar trabajos.|Sin dimensiones|
|jobs.listJobs.success|Sí|Llamadas correctas para enumerar trabajos|Count|Total|El recuento de todas las llamadas correctas para enumerar trabajos.|Sin dimensiones|
|jobs.queryJobs.failure|Sí|Consultas de trabajo con error|Count|Total|El recuento de todas las llamadas con error para consultar trabajos.|Sin dimensiones|
|jobs.queryJobs.success|Sí|Consultas de trabajo correctas|Count|Total|El recuento de todas las llamadas correctas para consultar trabajos.|Sin dimensiones|
|RoutingDataSizeInBytesDelivered|Sí|Tamaño del enrutamiento de los mensajes entregados en bytes (versión preliminar)|Bytes|Total|Tamaño total en bytes de los mensajes entregados por un centro de IoT a un punto de conexión. También puede usar las dimensiones EndpointName y EndpointType para ver el tamaño de los mensajes en bytes entregados a los distintos puntos de conexión. El valor de la métrica se incrementa por cada mensaje entregado, incluido si el mensaje se entrega a varios puntos de conexión o si el mensaje se entrega varias veces en el mismo punto de conexión.|EndpointType, EndpointName, RoutingSource|
|RoutingDeliveries|Sí|Entregas de enrutamiento (versión preliminar)|Count|Total|Número de veces que IoT Hub intentó la entrega de mensajes a todos los puntos de conexión mediante el enrutamiento. Para ver el número de intentos completados correctamente o con error, use la dimensión Result. Para ver el motivo del error, como no válido, quitado o huérfano, use la dimensión FailureReasonCategory. También puede usar las dimensiones EndpointName y EndpointType para entender cuántos mensajes se entregaron a los distintos puntos de conexión. El valor de la métrica se incrementa en uno por cada intento de entrega, incluido si el mensaje se entrega a varios puntos de conexión o si el mensaje se entrega varias veces en el mismo punto de conexión.|EndpointType, EndpointName, FailureReasonCategory, Result, RoutingSource|
|RoutingDeliveryLatency|Sí|Latencia de entrega de enrutamiento (versión preliminar)|MilliSeconds|Average|Latencia media (milisegundos) entre la entrada de mensajes a IoT Hub y la entrada de mensajes de telemetría a un punto de conexión. Puede usar las dimensiones EndpointName y EndpointType para comprender la latencia de los distintos puntos de conexión.|EndpointType, EndpointName, RoutingSource|
|totalDeviceCount|No|Número total de dispositivos|Count|Average|Número de dispositivos registrados en IoT Hub|Sin dimensiones|
|twinQueries.failure|Sí|Consultas gemelas con error|Count|Total|El recuento de todas las consultas gemelas con error.|Sin dimensiones|
|twinQueries.resultSize|Sí|Tamaño de resultado de consultas gemelas|Bytes|Average|El valor medio, mínimo y máximo del tamaño del resultado de todas las consultas gemelas correctas.|Sin dimensiones|
|twinQueries.success|Sí|Consultas gemelas correctas|Count|Total|El recuento de todas las consultas gemelas correctas.|Sin dimensiones|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AttestationAttempts|Sí|Intentos de atestación|Count|Total|Número de intentos de atestación de dispositivos|ProvisioningServiceName, Status, Protocol|
|DeviceAssignments|Sí|Dispositivos asignados|Count|Total|Número de dispositivos asignados a un IoT Hub|ProvisioningServiceName, IotHubName|
|RegistrationAttempts|Sí|Intentos de registro|Count|Total|Número de intentos de registro de dispositivos|ProvisioningServiceName, IotHubName, Status|


## <a name="microsoftdigitaltwinsdigitaltwinsinstances"></a>Microsoft.DigitalTwins/digitalTwinsInstances

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ApiRequests|Sí|Solicitudes de API|Count|Total|Número de solicitudes de API realizadas para las operaciones de lectura, escritura, eliminación y consulta de Digital Twins.|Operación, autenticación, protocolo, StatusCode, StatusCodeClass, StatusText|
|ApiRequestsFailureRate|Sí|Tasa de errores de solicitudes de API|Percent|Average|Porcentaje de solicitudes de API que el servicio recibe para la instancia que devuelve un código de respuesta de error interno (500) para las operaciones de lectura, escritura, eliminación y consulta de Digital Twins.|Operación, autenticación, protocolo|
|ApiRequestsLatency|Sí|Latencia de solicitudes de API|Milisegundos|Average|El tiempo de respuesta de las solicitudes de API, es decir, desde que Azure Digital Twins recibe la solicitud hasta que el servicio envía un resultado de éxito o error para las operaciones de lectura, escritura, eliminación y consulta Digital Twins.|Operación, autenticación, protocolo, StatusCode, StatusCodeClass, StatusText|
|BillingApiOperations|Sí|Operaciones de la API de facturación|Count|Total|Métrica de facturación para el recuento de todas las solicitudes de API realizadas en el servicio Azure Digital Twins.|Id. del medidor|
|BillingMessagesProcessed|Sí|Mensajes de facturación procesados|Count|Total|Métrica de facturación para el número de mensajes enviados desde Azure Digital Twins a puntos de conexión externos.|Id. del medidor|
|BillingQueryUnits|Sí|Facturación de unidades de consulta|Count|Total|Número de unidades de consulta, una medida del uso de recursos de servicio calculada internamente, que se consume para ejecutar consultas.|Id. del medidor|
|IngressEvents|Sí|Eventos de entrada|Count|Total|Número de eventos de telemetría entrantes en Azure Digital Twins.|Resultado|
|IngressEventsFailureRate|Sí|Tasa de errores de los eventos de entrada|Percent|Average|Porcentaje de eventos de telemetría entrantes para los que el servicio devuelve un código de respuesta de error interno (500).|Sin dimensiones|
|IngressEventsLatency|Sí|Latencia de eventos de entrada|Milisegundos|Average|Hora a partir de la cual llega un evento cuando está listo para que Azure Digital Twins lo haga salir, momento en el que el servicio envía un resultado correcto o erróneo.|Resultado|
|ModelCount|Sí|Recuento de modelos|Count|Total|Número total de modelos de una instancia de Azure Digital Twins. Use esta métrica para determinar si se está aproximando al límite del servicio para el número máximo de modelos permitido por instancia.|Sin dimensiones|
|Enrutamiento|Sí|Mensajes enrutados|Count|Total|Número de mensajes que se enrutan a un servicio de Azure de punto de conexión, como Event Hubs, Service Bus o Event Grid.|EndpointType, resultado|
|RoutingFailureRate|Sí|Tasa de errores de enrutamiento|Percent|Average|Porcentaje de eventos que producen un error a medida que se enrutan desde Azure Digital Twins a un servicio de Azure de punto de conexión, como Event Hubs, Service Bus o Event Grid.|EndpointType|
|RoutingLatency|Sí|Latencia de enrutamiento|Milisegundos|Average|Tiempo transcurrido entre el enrutamiento de un evento desde Azure Digital Twins hasta su publicación en el servicio de Azure de punto de conexión, como Event Hubs, Service Bus o Event Grid.|EndpointType, resultado|
|TwinCount|Sí|Recuento de gemelos|Count|Total|Número total de gemelos de una instancia de Azure Digital Twins. Use esta métrica para determinar si se está aproximando al límite del servicio para el número máximo de gemelos permitido por instancia.|Sin dimensiones|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/DatabaseAccounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AddRegion|Sí|Región agregada|Count|Count|Región agregada|Region|
|AutoscaleMaxThroughput|No|Rendimiento máximo del autoescalado|Count|Máxima|Rendimiento máximo del autoescalado|DatabaseName, CollectionName|
|AvailableStorage|No|(En desuso) Almacenamiento disponible|Bytes|Total|"Almacenamiento disponible" se quitará de Azure Monitor a finales de septiembre de 2023. El tamaño de almacenamiento de la colección de Cosmos DB ahora es ilimitado. La única restricción es que el tamaño de almacenamiento de cada clave de partición lógica sea de 20 GB. Puede habilitar PartitionKeyStatistics en el registro de diagnóstico para conocer el consumo de almacenamiento de las claves de partición principales. Para obtener más información sobre la cuota de almacenamiento de Cosmos DB, consulte esta documentación: [https://docs.microsoft.com/azure/cosmos-db/concepts-limits](/azure/cosmos-db/concepts-limits). Después de que se aplique el desuso, las reglas de alerta restantes aún definidas en la métrica en desuso se deshabilitarán automáticamente después de la fecha de desuso.|CollectionName, DatabaseName, Region|
|CassandraConnectionClosures|No|Cierres de conexión de Cassandra|Count|Total|Número de conexiones de Cassandra que se han cerrado, notificadas en una granularidad de 1 minuto|APIType, Region, ClosureReason|
|CassandraConnectorAvgReplicationLatency|No|Cassandra Connector Average ReplicationLatency|MilliSeconds|Average|Promedio de latencia de replicación del conector de Cassandra|Sin dimensiones|
|CassandraConnectorReplicationHealthStatus|No|Estado de mantenimiento de replicación del conector de Cassandra|Count|Count|Estado de mantenimiento de replicación del conector de Cassandra|NotStarted, ReplicationInProgress, Error|
|CassandraKeyspaceCreate|No|Espacio de claves de Cassandra creado|Count|Count|Espacio de claves de Cassandra creado|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|CassandraKeyspaceDelete|No|Espacio de claves de Cassandra eliminado|Count|Count|Espacio de claves de Cassandra eliminado|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|CassandraKeyspaceThroughputUpdate|No|Rendimiento de un espacio de claves de Cassandra actualizado|Count|Count|Rendimiento de un espacio de claves de Cassandra actualizado|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|CassandraKeyspaceUpdate|No|Espacio de claves de Cassandra actualizado|Count|Count|Espacio de claves de Cassandra actualizado|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|CassandraRequestCharges|No|Cargos de solicitud de Cassandra|Count|Total|Unidades de solicitud consumidas para las solicitudes de Cassandra realizadas|APIType, DatabaseName, CollectionName, Region, OperationType, ResourceType|
|CassandraRequests|No|Solicitudes de Cassandra|Count|Count|Número de solicitudes de Cassandra realizadas|APIType, DatabaseName, CollectionName, Region, OperationType, ResourceType, ErrorCode|
|CassandraTableCreate|No|Tabla de Cassandra creada|Count|Count|Tabla de Cassandra creada|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|CassandraTableDelete|No|Tabla de Cassandra eliminada|Count|Count|Tabla de Cassandra eliminada|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, OperationType|
|CassandraTableThroughputUpdate|No|Rendimiento de una tabla de Cassandra actualizado|Count|Count|Rendimiento de una tabla de Cassandra actualizado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|CassandraTableUpdate|No|Tabla de Cassandra actualizada|Count|Count|Tabla de Cassandra actualizada|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|CreateAccount|Sí|Cuenta creada|Count|Count|Cuenta creada|Sin dimensiones|
|DataUsage|No|Uso de datos|Bytes|Total|Uso total de datos notificado a una granularidad de cinco minutos|CollectionName, DatabaseName, Region|
|DedicatedGatewayAverageCPUUsage|No|DedicatedGatewayAverageCPUUsage|Percent|Average|Uso medio de CPU en las instancias de puerta de enlace dedicadas.|Region, MetricType|
|DedicatedGatewayAverageMemoryUsage|No|DedicatedGatewayAverageMemoryUsage|Bytes|Average|Uso medio de memoria en las instancias de puerta de enlace dedicadas, que se usa tanto para las solicitudes de enrutamiento como para los datos de almacenamiento en caché.|Region|
|DedicatedGatewayMaximumCPUUsage|No|DedicatedGatewayMaximumCPUUsage|Percent|Average|Uso medio máximo de CPU en las instancias de puerta de enlace dedicadas|Region, MetricType|
|DedicatedGatewayRequests|Sí|DedicatedGatewayRequests|Count|Count|Solicitudes en la puerta de enlace dedicada|DatabaseName, CollectionName, CacheExercised, OperationName, región|
|DeleteAccount|Sí|Cuenta eliminada|Count|Count|Cuenta eliminada|Sin dimensiones|
|DocumentCount|No|Recuento de documentos|Count|Total|Recuento total de documentos notificado a una granularidad de cinco minutos, una hora y un día|CollectionName, DatabaseName, Region|
|DocumentQuota|No|Cuota de documentos|Bytes|Total|Cuota total de almacenamiento notificada a una granularidad de cinco minutos|CollectionName, DatabaseName, Region|
|GremlinDatabaseCreate|No|Base de datos de Gremlin creada|Count|Count|Base de datos de Gremlin creada|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|GremlinDatabaseDelete|No|Base de datos de Gremlin eliminada|Count|Count|Base de datos de Gremlin eliminada|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|GremlinDatabaseThroughputUpdate|No|Rendimiento de una base de datos de Gremlin actualizado|Count|Count|Rendimiento de una base de datos de Gremlin actualizado|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|GremlinDatabaseUpdate|No|Base de datos de Gremlin actualizada|Count|Count|Base de datos de Gremlin actualizada|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|GremlinGraphCreate|No|Grafo de Gremlin creado|Count|Count|Grafo de Gremlin creado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|GremlinGraphDelete|No|Gráfico de Gremlin eliminado|Count|Count|Gráfico de Gremlin eliminado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, OperationType|
|GremlinGraphThroughputUpdate|No|Rendimiento de un grafo de Gremlin actualizado|Count|Count|Rendimiento de un grafo de Gremlin actualizado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|GremlinGraphUpdate|No|Grafo de Gremlin actualizado|Count|Count|Grafo de Gremlin actualizado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|IndexUsage|No|Uso de índice|Bytes|Total|Uso total del índice notificado a una granularidad de cinco minutos|CollectionName, DatabaseName, Region|
|IntegratedCacheEvictedEntriesSize|No|IntegratedCacheEvictedEntriesSize|Bytes|Average|Tamaño de las entradas expulsadas de la caché integrada|Region|
|IntegratedCacheItemExpirationCount|No|IntegratedCacheItemExpirationCount|Count|Average|Número de entradas expulsadas de la caché integrada debido a la expiración del TTL.|Region, CacheEntryType|
|IntegratedCacheItemHitRate|No|IntegratedCacheItemHitRate|Percent|Average|Número de lecturas de punto que usaban la caché integrada dividida por el número de lecturas de punto enrutadas a través de la puerta de enlace dedicada con coherencia final.|Region, CacheEntryType|
|IntegratedCacheQueryExpirationCount|No|IntegratedCacheQueryExpirationCount|Count|Average|Número de entradas eliminadas de la caché integrada debido a la expiración del TTL.|Region, CacheEntryType|
|IntegratedCacheQueryHitRate|No|IntegratedCacheQueryHitRate|Percent|Average|Número de consultas que usaban la caché integrada dividida por el número de consultas enrutadas a través de la puerta de enlace dedicada con coherencia final.|Region, CacheEntryType|
|MaterializedViewCatchupGapInMinutes|No|Brecha de puesta al día de vista materializada en minutos|Count|Máxima|Diferencia de tiempo máxima en minutos entre los datos del contenedor de origen y los datos propagados a la vista materializada|Region, TargetContainerName, BuildType|
|MaterializedViewsBuilderAverageCPUUsage|No|Uso medio de CPU del generador de vistas materializadas|Percent|Average|Uso medio de CPU en las instancias del generador de vistas materializadas, que se usan para rellenar datos en vistas materializadas|Region, MetricType|
|MaterializedViewsBuilderAverageMemoryUsage|No|Uso medio de memoria del generador de vistas materializadas|Bytes|Average|Uso medio de memoria en las instancias del generador de vistas materializadas, que se usan para rellenar datos en vistas materializadas|Region|
|MaterializedViewsBuilderMaximumCPUUsage|No|Uso máximo de CPU del generador de vistas materializadas|Percent|Average|Uso máximo de CPU en las instancias del generador de vistas materializadas, que se usan para rellenar datos en vistas materializadas|Region, MetricType|
|MetadataRequests|No|Solicitudes de metadatos|Count|Count|Recuento de las solicitudes de metadatos. Cosmos DB mantiene la colección de metadatos del sistema para cada cuenta, lo que le permite enumerar las colecciones, las bases de datos y sus configuraciones de forma gratuita.|DatabaseName, CollectionName, Region, StatusCode, Role|
|MongoCollectionCreate|No|Colección de Mongo creada|Count|Count|Colección de Mongo creada|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|MongoCollectionDelete|No|Colección de Mongo eliminada|Count|Count|Colección de Mongo eliminada|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, OperationType|
|MongoCollectionThroughputUpdate|No|Rendimiento de la colección de Mongo actualizado|Count|Count|Rendimiento de la colección de Mongo actualizado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|MongoCollectionUpdate|No|Colección de Mongo actualizada|Count|Count|Colección de Mongo actualizada|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|MongoDatabaseDelete|No|Base de datos de Mongo eliminada|Count|Count|Base de datos de Mongo eliminada|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|MongoDatabaseThroughputUpdate|No|Rendimiento de la base de datos de Mongo actualizado|Count|Count|Rendimiento de la base de datos de Mongo actualizado|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|MongoDBDatabaseCreate|No|Base de datos de Mongo creada|Count|Count|Base de datos de Mongo creada|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|MongoDBDatabaseUpdate|No|Base de datos de Mongo actualizada|Count|Count|Base de datos de Mongo actualizada|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|MongoRequestCharge|Sí|Cargo de la solicitud de Mongo|Count|Total|Unidades de la solicitud de Mongo consumidas|DatabaseName, CollectionName, Region, CommandName, ErrorCode, Status|
|MongoRequests|Sí|Solicitudes de Mongo|Count|Count|Número de solicitudes de Mongo realizadas|DatabaseName, CollectionName, Region, CommandName, ErrorCode, Status|
|NormalizedRUConsumption|No|Consumo de RU normalizado|Percent|Máxima|Porcentaje máximo de consumo de RU por minuto|CollectionName, DatabaseName, Region, PartitionKeyRangeId|
|ProvisionedThroughput|No|Rendimiento aprovisionado|Count|Máxima|Rendimiento aprovisionado|DatabaseName, CollectionName|
|RegionFailover|Sí|Región conmutada por error|Count|Count|Región conmutada por error|Sin dimensiones|
|RemoveRegion|Sí|Región quitada|Count|Count|Región quitada|Region|
|ReplicationLatency|Sí|Latencia de replicación P99|MilliSeconds|Average|Latencia de replicación P99 entre regiones de origen y destino para la cuenta habilitada geográficamente|SourceRegion, TargetRegion|
|ServerSideLatency|No|Latencia del servidor|MilliSeconds|Average|Latencia del servidor|DatabaseName, CollectionName, Region, ConnectionMode, OperationType, PublicAPIType|
|ServiceAvailability|No|Disponibilidad del servicio|Percent|Average|Disponibilidad de solicitudes de cuenta en una granularidad por hora, día o mes|Sin dimensiones|
|SqlContainerCreate|No|Contenedor SQL creado|Count|Count|Contenedor SQL creado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|SqlContainerDelete|No|Contenedor SQL eliminado|Count|Count|Contenedor SQL eliminado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, OperationType|
|SqlContainerThroughputUpdate|No|Rendimiento de un contenedor de SQL actualizado|Count|Count|Rendimiento de un contenedor de SQL actualizado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|SqlContainerUpdate|No|Contenedor SQL actualizado|Count|Count|Contenedor SQL actualizado|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|SqlDatabaseCreate|No|Sql Database Created|Count|Count|Base de datos SQL creada|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|SqlDatabaseDelete|No|Base de datos SQL eliminada|Count|Count|Base de datos SQL eliminada|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|SqlDatabaseThroughputUpdate|No|Rendimiento de una base de datos SQL actualizado|Count|Count|Rendimiento de una base de datos SQL actualizado|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|SqlDatabaseUpdate|No|Base de datos SQL actualizada|Count|Count|Base de datos SQL actualizada|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|TableTableCreate|No|Tabla de AzureTable creada|Count|Count|Tabla de AzureTable creada|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|TableTableDelete|No|Tabla AzureTable eliminada|Count|Count|Tabla AzureTable eliminada|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|TableTableThroughputUpdate|No|Rendimiento de la tabla AzureTable actualizado|Count|Count|Rendimiento de la tabla AzureTable actualizado|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|TableTableUpdate|No|Tabla AzureTable actualizada|Count|Count|Tabla AzureTable actualizada|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|TotalRequests|Sí|Total de solicitudes|Count|Count|Número de solicitudes realizadas|DatabaseName, CollectionName, Region, StatusCode, OperationType, Status|
|TotalRequestUnits|Sí|Unidades de solicitud totales|Count|Total|Unidades de solicitud consumidas|DatabaseName, CollectionName, Region, StatusCode, OperationType, Status|
|UpdateAccountKeys|Sí|Claves de cuenta actualizadas|Count|Count|Claves de cuenta actualizadas|KeyType|
|UpdateAccountNetworkSettings|Sí|Configuración de red de la cuenta actualizada|Count|Count|Configuración de red de la cuenta actualizada|Sin dimensiones|
|UpdateAccountReplicationSettings|Sí|Configuración de replicación de cuenta actualizada|Count|Count|Configuración de replicación de cuenta actualizada|Sin dimensiones|
|UpdateDiagnosticsSettings|No|Configuración de diagnóstico de cuenta actualizada|Count|Count|Configuración de diagnóstico de cuenta actualizada|DiagnosticSettingsName, ResourceGroupName|


## <a name="microsofteventgriddomains"></a>Microsoft.EventGrid/domains

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Sí|Evaluaciones de filtros avanzados|Count|Total|Total de filtros avanzados evaluados en las suscripciones de eventos de este tema.|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName, Error, ErrorType|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|Topic, ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Tema|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|


## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|Sin dimensiones|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|Sin dimensiones|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|Sin dimensiones|


## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|


## <a name="microsofteventgridpartnernamespaces"></a>Microsoft.EventGrid/partnerNamespaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|


## <a name="microsofteventgridpartnertopics"></a>Microsoft.EventGrid/partnerTopics

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Sí|Evaluaciones de filtros avanzados|Count|Total|Total de filtros avanzados evaluados en las suscripciones de eventos de este tema.|EventSubscriptionName|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|


## <a name="microsofteventgridsystemtopics"></a>Microsoft.EventGrid/systemTopics

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Sí|Evaluaciones de filtros avanzados|Count|Total|Total de filtros avanzados evaluados en las suscripciones de eventos de este tema.|EventSubscriptionName|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|


## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Sí|Evaluaciones de filtros avanzados|Count|Total|Total de filtros avanzados evaluados en las suscripciones de eventos de este tema.|EventSubscriptionName|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|


## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActiveConnections|No|ActiveConnections|Count|Average|Total de conexiones activas de Microsoft.EventHub.|Sin dimensiones|
|AvailableMemory|No|Memoria disponible|Percent|Máxima|Memoria disponible para el clúster del centro de eventos como un porcentaje de la memoria total.|Role|
|CaptureBacklog|No|Capturar el trabajo pendiente.|Count|Total|Capturar el trabajo pendiente de Microsoft.EventHub.|Sin dimensiones|
|CapturedBytes|No|Bytes capturados.|Bytes|Total|Bytes capturados de Microsoft.EventHub.|Sin dimensiones|
|CapturedMessages|No|Mensajes capturados.|Count|Total|Mensajes capturados de Microsoft.EventHub.|Sin dimensiones|
|ConnectionsClosed|No|Conexiones cerradas.|Count|Average|Conexiones cerradas de Microsoft.EventHub.|Sin dimensiones|
|ConnectionsOpened|No|Conexiones abiertas.|Count|Average|Conexiones abiertas de Microsoft.EventHub.|Sin dimensiones|
|CPU|No|CPU|Percent|Máxima|Uso de la CPU para el clúster del centro de eventos como porcentaje|Role|
|IncomingBytes|Sí|Bytes entrantes.|Bytes|Total|Bytes entrantes de Microsoft.EventHub.|Sin dimensiones|
|IncomingMessages|Sí|Mensajes entrantes|Count|Total|Mensajes entrantes de Microsoft.EventHub.|Sin dimensiones|
|IncomingRequests|Sí|Solicitudes entrantes|Count|Total|Solicitudes entrantes de Microsoft.EventHub.|Sin dimensiones|
|OutgoingBytes|Sí|Bytes salientes.|Bytes|Total|Bytes salientes de Microsoft.EventHub.|Sin dimensiones|
|OutgoingMessages|Sí|Mensajes salientes|Count|Total|Mensajes salientes de Microsoft.EventHub.|Sin dimensiones|
|QuotaExceededErrors|No|Cuota de errores superada.|Count|Total|Cuota de errores superada de Microsoft.EventHub.|OperationResult|
|ServerErrors|No|Errores del servidor.|Count|Total|Errores del servidor de Microsoft.EventHub.|OperationResult|
|Size|No|Size|Bytes|Average|Tamaño de EventHub en bytes.|Role|
|SuccessfulRequests|No|Solicitudes correctas|Count|Total|Solicitudes correctas de Microsoft.EventHub.|OperationResult|
|ThrottledRequests|No|Solicitudes limitadas.|Count|Total|Solicitudes limitadas de Microsoft.EventHub.|OperationResult|
|UserErrors|No|Errores de usuario.|Count|Total|Errores de usuario de Microsoft.EventHub.|OperationResult|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActiveConnections|No|ActiveConnections|Count|Average|Total de conexiones activas de Microsoft.EventHub.|Sin dimensiones|
|CaptureBacklog|No|Capturar el trabajo pendiente.|Count|Total|Capturar el trabajo pendiente de Microsoft.EventHub.|EntityName|
|CapturedBytes|No|Bytes capturados.|Bytes|Total|Bytes capturados de Microsoft.EventHub.|EntityName|
|CapturedMessages|No|Mensajes capturados.|Count|Total|Mensajes capturados de Microsoft.EventHub.|EntityName|
|ConnectionsClosed|No|Conexiones cerradas.|Count|Average|Conexiones cerradas de Microsoft.EventHub.|EntityName|
|ConnectionsOpened|No|Conexiones abiertas.|Count|Average|Conexiones abiertas de Microsoft.EventHub.|EntityName|
|EHABL|Sí|Mensajes de trabajo pendiente de archivado (en desuso)|Count|Total|Mensajes de archivado del centro de eventos en el trabajo pendiente para un espacio de nombres (en desuso)|Sin dimensiones|
|EHAMBS|Sí|Rendimiento de mensajes de archivado (en desuso)|Bytes|Total|Rendimiento de mensajes de archivado del centro de eventos en un espacio de nombres (en desuso)|Sin dimensiones|
|EHAMSGS|Sí|Mensajes de archivado (en desuso)|Count|Total|Mensajes de archivado del centro de eventos en un espacio de nombres (en desuso)|Sin dimensiones|
|EHINBYTES|Sí|Bytes de entrada (en desuso)|Bytes|Total|Rendimiento de mensajes entrantes del centro de eventos para un espacio de nombres (en desuso)|Sin dimensiones|
|EHINMBS|Sí|Incoming bytes (obsolete) (Deprecated) [Bytes entrantes (obsoletos) (en desuso)]|Bytes|Total|Rendimiento de mensajes entrantes del centro de eventos para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Bytes de entrada (en desuso)|Sin dimensiones|
|EHINMSGS|Sí|Mensajes entrantes (en desuso)|Count|Total|Total de mensajes entrantes para un espacio de nombres (en desuso)|Sin dimensiones|
|EHOUTBYTES|Sí|Bytes de salida (en desuso)|Bytes|Total|Rendimiento de mensajes salientes del centro de eventos para un espacio de nombres (en desuso)|Sin dimensiones|
|EHOUTMBS|Sí|Outgoing bytes (obsolete) (Deprecated) [Bytes salientes (obsoletos) (en desuso)]|Bytes|Total|Rendimiento de mensajes salientes del centro de eventos para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Bytes de salida (en desuso)|Sin dimensiones|
|EHOUTMSGS|Sí|Mensajes salientes (en desuso)|Count|Total|Total de mensajes salientes para un espacio de nombres (en desuso)|Sin dimensiones|
|FAILREQ|Sí|Solicitudes con error (en desuso)|Count|Total|Total de solicitudes con error para un espacio de nombres (en desuso)|Sin dimensiones|
|IncomingBytes|Sí|Bytes entrantes.|Bytes|Total|Bytes entrantes de Microsoft.EventHub.|EntityName|
|IncomingMessages|Sí|Mensajes entrantes|Count|Total|Mensajes entrantes de Microsoft.EventHub.|EntityName|
|IncomingRequests|Sí|Solicitudes entrantes|Count|Total|Solicitudes entrantes de Microsoft.EventHub.|EntityName|
|INMSGS|Sí|Incoming Messages (obsolete) (Deprecated) [Mensajes entrantes (obsoletos) (en desuso)]|Count|Total|Total de mensajes entrantes para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Mensajes entrantes (en desuso)|Sin dimensiones|
|INREQS|Sí|Solicitudes entrantes (en desuso)|Count|Total|Total de solicitudes de envío entrantes de un espacio de nombres (en desuso)|Sin dimensiones|
|INTERR|Sí|Errores internos del servidor (en desuso)|Count|Total|Errores totales de servidor interno para un espacio de nombres (en desuso)|Sin dimensiones|
|MISCERR|Sí|Otros errores (en desuso)|Count|Total|Total de solicitudes con error para un espacio de nombres (en desuso)|Sin dimensiones|
|OutgoingBytes|Sí|Bytes salientes.|Bytes|Total|Bytes salientes de Microsoft.EventHub.|EntityName|
|OutgoingMessages|Sí|Mensajes salientes|Count|Total|Mensajes salientes de Microsoft.EventHub.|EntityName|
|OUTMSGS|Sí|Outgoing Messages (obsolete) (Deprecated) [Mensajes salientes (obsoletos) (en desuso)]|Count|Total|Total de mensajes salientes para un espacio de nombres. Esta métrica está en desuso. En su lugar, use la métrica Mensajes salientes (en desuso)|Sin dimensiones|
|QuotaExceededErrors|No|Cuota de errores superada.|Count|Total|Cuota de errores superada de Microsoft.EventHub.|EntityName, OperationResult|
|ServerErrors|No|Errores del servidor.|Count|Total|Errores del servidor de Microsoft.EventHub.|EntityName, OperationResult|
|Size|No|Size|Bytes|Average|Tamaño de EventHub en bytes.|EntityName|
|SuccessfulRequests|No|Solicitudes correctas|Count|Total|Solicitudes correctas de Microsoft.EventHub.|EntityName, OperationResult|
|SUCCREQ|Sí|Solicitudes correctas (en desuso)|Count|Total|Total de solicitudes correctas para un espacio de nombres (en desuso)|Sin dimensiones|
|SVRBSY|Sí|Errores de servidor ocupado (en desuso)|Count|Total|Errores totales de servidor ocupado para un espacio de nombres (en desuso)|Sin dimensiones|
|ThrottledRequests|No|Solicitudes limitadas.|Count|Total|Solicitudes limitadas de Microsoft.EventHub.|EntityName, OperationResult|
|UserErrors|No|Errores de usuario.|Count|Total|Errores de usuario de Microsoft.EventHub.|EntityName, OperationResult|


## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|CategorizedGatewayRequests|Sí|Solicitudes de puerta de enlace categorizadas|Count|Total|Número de solicitudes de puerta de enlace por categorías (1xx/2xx/3xx/4xx/5xx)|HttpStatus|
|GatewayRequests|Sí|Solicitudes de puerta de enlace|Count|Total|Número de solicitudes de puerta de enlace|HttpStatus|
|KafkaRestProxy.ConsumerRequest.m1_delta|Sí|Rendimiento de las solicitudes de consumidor de proxy de REST|CountPerSecond|Total|Número de solicitudes de consumidor al proxy de REST de Kafka|Máquina, tema|
|KafkaRestProxy.ConsumerRequestFail.m1_delta|Sí|Solicitudes incorrectas del consumidor del proxy de REST|CountPerSecond|Total|Excepciones de solicitud de consumidor|Máquina, tema|
|KafkaRestProxy.ConsumerRequestTime.p95|Sí|Latencia de las solicitudes de consumidor del proxy de REST|Milisegundos|Average|Latencia de mensajes en una solicitud de consumidor a través del proxy de REST de Kafka|Máquina, tema|
|KafkaRestProxy.ConsumerRequestWaitingInQueueTime.p95|Sí|Trabajo pendiente de las solicitudes de consumidor del proxy de REST|Milisegundos|Average|Longitud de cola de proxy de REST de consumidor|Máquina, tema|
|KafkaRestProxy.MessagesIn.m1_delta|Sí|Rendimiento del mensaje del productor del proxy de REST|CountPerSecond|Total|Número de mensajes del productor a través del proxy de REST de Kafka|Máquina, tema|
|KafkaRestProxy.MessagesOut.m1_delta|Sí|Rendimiento del mensaje del consumidor del proxy de REST|CountPerSecond|Total|Número de mensajes del consumidor a través del proxy de REST de Kafka|Máquina, tema|
|KafkaRestProxy.OpenConnections|Sí|Conexiones simultáneas del proxy de REST|Count|Total|Número de conexiones simultáneas a través del proxy de REST de Kafka|Máquina, tema|
|KafkaRestProxy.ProducerRequest.m1_delta|Sí|Rendimiento de las solicitudes del productor del proxy de REST|CountPerSecond|Total|Número de solicitudes del productor al proxy de REST de Kafka|Máquina, tema|
|KafkaRestProxy.ProducerRequestFail.m1_delta|Sí|Solicitudes incorrectas del productor del proxy de REST|CountPerSecond|Total|Excepciones de solicitud de productor|Máquina, tema|
|KafkaRestProxy.ProducerRequestTime.p95|Sí|Latencia de las solicitudes del productor del proxy de REST|Milisegundos|Average|Latencia de mensajes en una solicitud de productor a través del proxy de REST de Kafka|Máquina, tema|
|KafkaRestProxy.ProducerRequestWaitingInQueueTime.p95|Sí|Trabajo pendiente de las solicitudes del productor del proxy de REST|Milisegundos|Average|Longitud de cola del proxy de REST del consumidor|Máquina, tema|
|NumActiveWorkers|Sí|Número de trabajos activos|Count|Máxima|Número de trabajos activos|MetricName|
|PendingCPU|Sí|CPU pendiente|Count|Máxima|Solicitudes de CPU pendientes en YARN|Sin dimensiones|
|PendingMemory|Sí|Memoria pendiente|Count|Máxima|Solicitudes de memoria pendientes en YARN|Sin dimensiones|


## <a name="microsofthealthcareapisservices"></a>Microsoft.HealthcareApis/services

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|La tasa de disponibilidad del servicio.|Sin dimensiones|
|CosmosDbCollectionSize|Sí|El tamaño de una colección de Cosmos DB|Bytes|Total|El tamaño de la colección de Cosmos DB de respaldo, en bytes.|Sin dimensiones|
|CosmosDbIndexSize|Sí|Tamaño del índice de Cosmos DB|Bytes|Total|El tamaño del índice de la colección de Cosmos DB de respaldo, en bytes.|Sin dimensiones|
|CosmosDbRequestCharge|Sí|Uso de unidades de solicitud de Cosmos DB|Count|Total|El uso de unidades de solicitud de las solicitudes a la instancia de Cosmos DB de respaldo del servicio.|Operación, ResourceType|
|CosmosDbRequests|Sí|Solicitudes de Cosmos DB de servicio|Count|Sum|El número total de solicitudes realizadas a la instancia de Cosmos DB de respaldo de un servicio.|Operación, ResourceType|
|CosmosDbThrottleRate|Sí|Velocidad de limitación de la instancia de Cosmos DB de servicio|Count|Sum|El número total de respuestas 429 de la instancia de Cosmos DB de respaldo de un servicio.|Operación, ResourceType|
|IoTConnectorDeviceEvent|Sí|Número de mensajes entrantes|Count|Sum|El número total de mensajes recibidos por el conector de Azure IoT para FHIR antes de cualquier normalización.|Operación, ConnectorName|
|IoTConnectorDeviceEventProcessingLatencyMs|Sí|Latencia de la fase de normalización promedio|Milisegundos|Average|Promedio de tiempo entre el momento de ingesta de un evento y el momento en el que se procesa el evento para la normalización.|Operación, ConnectorName|
|IoTConnectorMeasurement|Sí|Número de medidas|Count|Sum|El número de lecturas de valores normalizados que recibe la fase de conversión de FHIR del conector de Azure IoT para FHIR.|Operación, ConnectorName|
|IoTConnectorMeasurementGroup|Sí|Número de grupos de mensajes|Count|Sum|El número total de agrupaciones únicas de medidas en los tipos, dispositivos, pacientes y períodos de tiempo configurados que genera la fase de conversión de FHIR.|Operación, ConnectorName|
|IoTConnectorMeasurementIngestionLatencyMs|Sí|Latencia de la fase de grupo promedio|Milisegundos|Average|El período de tiempo transcurrido entre el momento en que el conector de IoT recibió los datos del dispositivo y el momento en que la fase de conversión de FHIR procesa los datos.|Operación, ConnectorName|
|IoTConnectorNormalizedEvent|Sí|Número de mensajes normalizados|Count|Sum|El número total de valores normalizados asignados que genera la fase de normalización del conector de Azure IoT para FHIR.|Operación, ConnectorName|
|IoTConnectorTotalErrors|Sí|Recuento total de errores|Count|Sum|El número total de errores registrados por el conector de Azure IoT para FHIR|Nombre, operación, ErrorType, ErrorSeverity, ConnectorName|
|TotalErrors|Sí|Total de errores|Count|Sum|Número total de errores internos del servidor encontrados por el servicio.|Protocolo, StatusCode, StatusCodeClass, StatusCodeText|
|TotalLatency|Sí|Latencia total|Milisegundos|Average|Latencia de respuesta del servicio.|Protocolo|
|TotalRequests|Sí|Total de solicitudes|Count|Sum|Número total de solicitudes recibidas por el servicio.|Protocolo|


## <a name="microsofthealthcareapisworkspacesiotconnectors"></a>Microsoft.HealthcareApis/workspaces/iotconnectors

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|DeviceEvent|Sí|Número de mensajes entrantes|Count|Sum|El número total de mensajes recibidos por el conector de Azure IoT para FHIR antes de cualquier normalización.|Operation, ResourceName|
|DeviceEventProcessingLatencyMs|Sí|Latencia de la fase de normalización promedio|Milisegundos|Average|Promedio de tiempo entre el momento de ingesta de un evento y el momento en el que se procesa el evento para la normalización.|Operation, ResourceName|
|Medición|Sí|Número de medidas|Count|Sum|El número de lecturas de valores normalizados que recibe la fase de conversión de FHIR del conector de Azure IoT para FHIR.|Operation, ResourceName|
|MeasurementGroup|Sí|Número de grupos de mensajes|Count|Sum|El número total de agrupaciones únicas de medidas en los tipos, dispositivos, pacientes y períodos de tiempo configurados que genera la fase de conversión de FHIR.|Operation, ResourceName|
|MeasurementIngestionLatencyMs|Sí|Latencia de la fase de grupo promedio|Milisegundos|Average|El período de tiempo transcurrido entre el momento en que el conector de IoT recibió los datos del dispositivo y el momento en que la fase de conversión de FHIR procesa los datos.|Operation, ResourceName|
|NormalizedEvent|Sí|Número de mensajes normalizados|Count|Sum|El número total de valores normalizados asignados que genera la fase de normalización del conector de Azure IoT para FHIR.|Operation, ResourceName|
|TotalErrors|Sí|Recuento total de errores|Count|Sum|El número total de errores registrados por el conector de Azure IoT para FHIR|Name, Operation, ErrorType, ErrorSeverity, ResourceName|


## <a name="microsofthybridnetworknetworkfunctions"></a>microsoft.hybridnetwork/networkfunctions

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|HyperVVirtualProcessorUtilization|Sí|Uso medio de la CPU|Percent|Average|Porcentaje medio total de uso de la CPU virtual en un intervalo de un minuto. El número total de CPU virtuales se basa en el valor configurado por el usuario en la definición de SKU. Se puede aplicar un filtro adicional en función del valor de RoleName definido en el SKU.|InstanceName|


## <a name="microsofthybridnetworkvirtualnetworkfunctions"></a>microsoft.hybridnetwork/virtualnetworkfunctions

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|HyperVVirtualProcessorUtilization|Sí|Uso medio de la CPU|Percent|Average|Porcentaje medio total de uso de la CPU virtual en un intervalo de un minuto. El número total de CPU virtuales se basa en el valor configurado por el usuario en la definición de SKU. Se puede aplicar un filtro adicional en función del valor de RoleName definido en el SKU.|InstanceName|


## <a name="microsoftinsightsautoscalesettings"></a>microsoft.insights/autoscalesettings

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|MetricThreshold|Sí|Umbral de métrica|Count|Average|El umbral configurado de escalado automático cuando se ejecutó el escalado automático.|MetricTriggerRule|
|ObservedCapacity|Sí|Capacidad observada|Count|Average|La capacidad informada al escalado automático cuando se ejecutó.|Sin dimensiones|
|ObservedMetricValue|Sí|Valor de métrica observado|Count|Average|El valor calculado por el escalado automático cuando se ejecuta|MetricTriggerSource|
|ScaleActionsInitiated|Sí|Acciones de escalado iniciadas|Count|Total|La dirección de la operación de escalado.|ScaleDirection|


## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Sí|Disponibilidad|Percent|Average|Porcentaje de pruebas de disponibilidad completadas correctamente|availabilityResult/name, availabilityResult/location|
|availabilityResults/count|No|Pruebas de disponibilidad|Count|Count|Recuento de pruebas de disponibilidad|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|availabilityResults/duration|Sí|Duración de la prueba de disponibilidad|MilliSeconds|Average|Duración de la prueba de disponibilidad|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|browserTimings/networkDuration|Sí|Tiempo de conexión de red de carga de página|MilliSeconds|Average|Tiempo transcurrido entre la solicitud del usuario y la conexión de red. Incluye la búsqueda de DNS y la conexión de transporte.|Sin dimensiones|
|browserTimings/processingDuration|Sí|Tiempo de procesamiento del cliente|MilliSeconds|Average|Tiempo transcurrido entre la recepción del último byte de un documento hasta que se carga el DOM. Todavía se pueden procesar solicitudes asincrónicas.|Sin dimensiones|
|browserTimings/receiveDuration|Sí|Tiempo de recepción de respuesta|MilliSeconds|Average|Tiempo transcurrido entre el primer y el último byte, o hasta la desconexión.|Sin dimensiones|
|browserTimings/sendDuration|Sí|Tiempo de solicitud de envío|MilliSeconds|Average|Tiempo transcurrido entre la conexión de red y la recepción del primer byte.|Sin dimensiones|
|browserTimings/totalDuration|Sí|Tiempo de carga de página del explorador|MilliSeconds|Average|Tiempo que transcurre entre la solicitud del usuario hasta que se cargan el DOM, las hojas de estilo, los scripts y las imágenes.|Sin dimensiones|
|dependencies/count|No|Llamadas de dependencia|Count|Count|Número de llamadas realizadas por la aplicación a recursos externos.|dependency/type, dependency/performanceBucket, dependency/success, dependency/target, dependency/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/duration|Sí|Duración de la dependencia|MilliSeconds|Average|Duración de las llamadas realizadas por la aplicación a recursos externos.|dependency/type, dependency/performanceBucket, dependency/success, dependency/target, dependency/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/failed|No|Errores de llamadas de dependencia|Count|Count|Número de llamadas de dependencia erróneas realizadas por la aplicación a recursos externos.|dependency/type, dependency/performanceBucket, dependency/target, dependency/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|exceptions/browser|No|Excepciones de explorador|Count|Count|Recuento de excepciones no detectadas en el explorador.|cloud/roleName|
|exceptions/count|Sí|Excepciones|Count|Count|Recuento combinado de todas las excepciones no detectadas.|cloud/roleName, cloud/roleInstance, client/type|
|exceptions/server|No|Excepciones de servidor|Count|Count|Número de excepciones no detectadas producidas en la aplicación de servidor.|cloud/roleName, cloud/roleInstance|
|pageViews/count|Sí|Vistas de página|Count|Count|Número de vistas de página.|operation/synthetic, cloud/roleName|
|pageViews/duration|Sí|Tiempo de carga de la vista de página|MilliSeconds|Average|Tiempo de carga de la vista de página|operation/synthetic, cloud/roleName|
|performanceCounters/exceptionsPerSecond|Sí|Velocidad de excepciones|CountPerSecond|Average|Recuento de excepciones controladas y no controladas de las cuales se informó a Windows, incluidas las excepciones de .NET y las excepciones no administradas que se convierten en excepciones de .NET.|cloud/roleInstance|
|performanceCounters/memoryAvailableBytes|Sí|Memoria disponible|Bytes|Average|Memoria física disponible de inmediato para su asignación a un proceso o para uso del sistema.|cloud/roleInstance|
|performanceCounters/processCpuPercentage|Sí|CPU de procesos|Percent|Average|Porcentaje de tiempo transcurrido que todos los subprocesos del proceso han usado el procesador para ejecutar instrucciones. Puede variar entre 0 y 100. Esta métrica indica el rendimiento solo del proceso w3wp.|cloud/roleInstance|
|performanceCounters/processIOBytesPerSecond|Sí|Velocidad de E/S del proceso|BytesPerSecond|Average|Número total de bytes por segundo leídos y escritos en los archivos, la red y los dispositivos.|cloud/roleInstance|
|performanceCounters/processorCpuPercentage|Sí|Tiempo de procesador|Percent|Average|Porcentaje de tiempo que invierte el procesador en subprocesos no inactivos.|cloud/roleInstance|
|performanceCounters/processPrivateBytes|Sí|Bytes privados del proceso|Bytes|Average|Memoria asignada exclusivamente a los procesos de la aplicación supervisada.|cloud/roleInstance|
|performanceCounters/requestExecutionTime|Sí|Tiempo de ejecución de solicitud HTTP|MilliSeconds|Average|Tiempo de ejecución de la solicitud más reciente.|cloud/roleInstance|
|performanceCounters/requestsInQueue|Sí|Solicitudes HTTP en la cola de la aplicación|Count|Average|Longitud de la cola de solicitudes de aplicación.|cloud/roleInstance|
|performanceCounters/requestsPerSecond|Sí|Tasa de solicitudes HTTP|CountPerSecond|Average|Velocidad de todas las solicitudes a la aplicación por segundo desde ASP.NET.|cloud/roleInstance|
|requests/count|No|Solicitudes de servidor|Count|Count|Número de solicitudes HTTP completadas.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/duration|Sí|Tiempo de respuesta del servidor|MilliSeconds|Average|Tiempo entre que se recibe una solicitud HTTP y se termina de enviar la respuesta.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/failed|No|Error en las solicitudes|Count|Count|Número de solicitudes HTTP marcadas como erróneas. En la mayoría de los casos, se trata de solicitudes con un código de respuesta > = 400 y no igual a 401.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|requests/rate|No|Tasa de solicitudes del servidor|CountPerSecond|Average|Tasa de solicitudes del servidor por segundo|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|traces/count|Sí|Traces|Count|Count|Número de documentos de seguimiento|trace/severityLevel, operation/synthetic, cloud/roleName, cloud/roleInstance|


## <a name="microsoftiotcentraliotapps"></a>Microsoft.IoTCentral/IoTApps

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|c2d.commands.failure|Sí|Invocaciones de comandos con error|Count|Total|Recuento de todas las solicitudes de comandos con error iniciadas desde IoT Central|Sin dimensiones|
|c2d.commands.requestSize|Sí|Tamaño de solicitud de las invocaciones de comandos|Bytes|Total|Tamaño de solicitud de todas las solicitudes de comandos iniciadas desde IoT Central|Sin dimensiones|
|c2d.commands.responseSize|Sí|Tamaño de respuesta de las invocaciones de comandos|Bytes|Total|Tamaño de respuesta de todas las respuestas de comandos iniciadas desde IoT Central|Sin dimensiones|
|c2d.commands.success|Sí|Invocaciones de comandos correctas|Count|Total|Recuento de todas las solicitudes de comandos correctas iniciadas desde IoT Central|Sin dimensiones|
|c2d.property.read.failure|Sí|Lecturas de propiedades de dispositivo con error de IoT Central|Count|Total|Recuento de todas las lecturas de propiedades con error iniciadas desde IoT Central|Sin dimensiones|
|c2d.property.read.success|Sí|Lecturas de propiedades de dispositivo correctas de IoT Central|Count|Total|Recuento de todas las lecturas de propiedades correctas iniciadas desde IoT Central|Sin dimensiones|
|c2d.property.update.failure|Sí|Actualizaciones de propiedades de dispositivo con error de IoT Central|Count|Total|Recuento de todas las actualizaciones de propiedades con error iniciadas desde IoT Central|Sin dimensiones|
|c2d.property.update.success|Sí|Actualizaciones de propiedades de dispositivo correctas de IoT Central|Count|Total|Recuento de todas las actualizaciones de propiedades correctas iniciadas desde IoT Central|Sin dimensiones|
|connectedDeviceCount|No|Dispositivos conectados totales|Count|Average|Número de dispositivos conectados a IoT Central|Sin dimensiones|
|d2c.property.read.failure|Sí|Lecturas de propiedades de dispositivo con error de los dispositivos|Count|Total|Recuento de todas las lecturas de propiedades con error iniciadas desde dispositivos|Sin dimensiones|
|d2c.property.read.success|Sí|Lecturas de propiedades de dispositivo correctas de los dispositivos|Count|Total|Recuento de todas las lecturas de propiedades correctas iniciadas desde dispositivos|Sin dimensiones|
|d2c.property.update.failure|Sí|Actualizaciones de propiedades de dispositivo con errores de los dispositivos|Count|Total|Recuento de todas las actualizaciones de propiedades con error iniciadas desde dispositivos|Sin dimensiones|
|d2c.property.update.success|Sí|Actualizaciones de propiedades de dispositivo correctas de los dispositivos|Count|Total|Recuento de todas las actualizaciones de propiedades correctas iniciadas desde los dispositivos|Sin dimensiones|
|d2c.telemetry.ingress.allProtocol|Sí|Número total de intentos de envío de mensajes de telemetría|Count|Total|Número de mensajes de telemetría de dispositivo a nube que se intentaron enviar a la aplicación de IoT Central|Sin dimensiones|
|d2c.telemetry.ingress.success|Sí|Total de mensajes de telemetría enviados|Count|Total|Número de mensajes de telemetría de dispositivo a nube enviados correctamente a la aplicación de IoT Central|Sin dimensiones|
|dataExport.error|Sí|Errores de exportación de datos|Count|Total|Número máximo de errores detectados en la exportación de errores|exportId, exportDisplayName, destinationId, destinationDisplayName|
|dataExport.messages.filtered|Sí|Mensajes de exportación de datos filtrados|Count|Total|Número de mensajes que han pasado a través de filtros en la exportación de datos|exportId, exportDisplayName, destinationId, destinationDisplayName|
|dataExport.messages.received|Sí|Mensajes de exportación de datos recibidos|Count|Total|Número de mensajes entrantes en la exportación de datos, antes del filtrado y el procesamiento del enriquecimiento|exportId, exportDisplayName, destinationId, destinationDisplayName|
|dataExport.messages.written|Sí|Mensajes de exportación de datos escritos|Count|Total|Número de mensajes escritos en un destino|exportId, exportDisplayName, destinationId, destinationDisplayName|
|dataExport.statusChange|Sí|Cambio del estado de exportación de datos|Count|Total|Número de cambios de estado|exportId, exportDisplayName, destinationId, destinationDisplayName, status|
|deviceDataUsage|Sí|Uso total de datos del dispositivo|Bytes|Total|Bytes transferidos a cualquier dispositivo conectado a una aplicación de IoT Central o desde este.|Sin dimensiones|
|provisionedDeviceCount|No|Total de dispositivos aprovisionados|Count|Average|Número de dispositivos aprovisionados en la aplicación de IoT Central|Sin dimensiones|


## <a name="microsoftkeyvaultmanagedhsms"></a>microsoft.keyvault/managedhsms

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|No|Disponibilidad general del servicio|Percent|Average|Disponibilidad de solicitudes del servicio|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|ServiceApiHit|Sí|Número total de visitas de la API de servicio|Count|Count|Número total de visitas de la API de servicio|ActivityType, ActivityName|
|ServiceApiLatency|No|Latencia general de la API de servicio|Milisegundos|Average|Latencia general de las solicitudes de la API de servicio|ActivityType, ActivityName, StatusCode, StatusCodeClass|


## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad total del almacén|Percent|Average|Disponibilidad de solicitudes del almacén|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|SaturationShoebox|No|Saturación general del almacén|Percent|Average|Capacidad usada del almacén|ActivityType, ActivityName, TransactionType|
|ServiceApiHit|Sí|Número total de visitas de la API de servicio|Count|Count|Número total de visitas de la API de servicio|ActivityType, ActivityName|
|ServiceApiLatency|Sí|Latencia general de la API de servicio|MilliSeconds|Average|Latencia general de las solicitudes de la API de servicio|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|ServiceApiResult|Sí|Número total de resultados de la API de servicio|Count|Count|Número total de resultados de la API de servicio|ActivityType, ActivityName, StatusCode, StatusCodeClass|


## <a name="microsoftkubernetesconnectedclusters"></a>microsoft.kubernetes/connectedClusters

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|capacity_cpu_cores|Sí|Número total de núcleos de CPU en un clúster conectado|Count|Total|Número total de núcleos de CPU en un clúster conectado|Sin dimensiones|


## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BatchBlobCount|Sí|Recuento de blobs de lote|Count|Average|Número de orígenes de datos de un lote agregado para la ingesta.|Base de datos|
|BatchDuration|Sí|Duración del lote|Segundos|Average|La duración de la fase de agregación en el flujo de la ingesta.|Base de datos|
|BatchesProcessed|Sí|Lotes procesados|Count|Total|Número de lotes agregados para la ingesta. Tipo de procesamiento por lotes: si el lote alcanzó el tiempo de procesamiento por lotes, el tamaño de los datos o el límite de archivos establecido por la directiva de procesamiento por lotes|Database, SealReason|
|BatchSize|Sí|Tamaño de lote|Bytes|Average|Tamaño de datos esperado sin comprimir en un lote agregado para la ingesta.|Base de datos|
|BlobsDropped|Sí|Blobs eliminados|Count|Total|Número de blobs rechazados permanentemente por un componente.|Database, ComponentType, ComponentName|
|BlobsProcessed|Sí|Blobs procesados|Count|Total|Número de blobs procesados por un componente.|Database, ComponentType, ComponentName|
|BlobsReceived|Sí|Blobs recibidos|Count|Total|Número de blobs recibidos del flujo de entrada por un componente.|Database, ComponentType, ComponentName|
|CacheUtilization|Sí|Uso de la caché|Percent|Average|Nivel de uso en el ámbito de clúster|Sin dimensiones|
|CacheUtilizationFactor|Sí|Factor de uso de la caché|Percent|Average|Diferencia de porcentaje entre el número actual de instancias y el número óptimo de instancias (por uso de caché)|Sin dimensiones|
|ContinuousExportMaxLatenessMinutes|Sí|Máx. de demora en la exportación continua|Count|Máxima|Demora (en minutos) indicada por los trabajos de exportación continua del clúster|Sin dimensiones|
|ContinuousExportNumOfRecordsExported|Sí|Exportación continua: número de registros exportados|Count|Total|Número de registros exportados que se han activado para cada artefacto de almacenamiento escrito durante la operación de exportación.|ContinuousExportName, Database|
|ContinuousExportPendingCount|Sí|Recuento pendiente de exportación continua|Count|Máxima|El número de trabajos de exportación continua pendientes listos para su ejecución|Sin dimensiones|
|ContinuousExportResult|Sí|Continuous Export Result (Resultado de la exportación continua)|Count|Count|Indica si la exportación continua se realizó correctamente o no.|ContinuousExportName, Result, Database|
|CPU|Sí|CPU|Percent|Average|Nivel de uso de CPU|Sin dimensiones|
|DiscoveryLatency|Sí|Latencia de detección|Segundos|Average|Notificada por las conexiones de datos (si existen). Tiempo en segundos desde el momento en que se pone en cola un mensaje o se crea un evento, hasta que lo detecta la conexión de datos. Este tiempo no se incluye en la duración total de la ingesta de Azure Data Explorer.|ComponentType, ComponentName|
|EventsDropped|Sí|Eventos eliminados|Count|Total|Número de eventos eliminados de forma permanente por la conexión de datos. Se enviará una métrica de resultado de ingesta con un motivo de error.|ComponentType, ComponentName|
|EventsProcessed|Sí|Eventos procesados|Count|Total|Número de eventos que procesa el clúster|ComponentType, ComponentName|
|EventsProcessedForEventHubs|Sí|Eventos procesados (para instancias de Event Hubs o IoT Hub)|Count|Total|Número de eventos que ha procesado el clúster en la ingesta desde el centro de eventos o Iot Hub.|EventStatus|
|EventsReceived|Sí|Eventos recibidos|Count|Total|Número de eventos recibidos por la conexión de datos.|ComponentType, ComponentName|
|ExportUtilization|Sí|Utilización de la exportación|Percent|Máxima|Utilización de la exportación|Sin dimensiones|
|IngestionLatencyInSeconds|Sí|Latencia de la ingesta|Segundos|Average|La latencia de datos ingeridos desde el momento en que se recibieron los datos en el clúster hasta que está preparado para la consulta. El periodo de latencia de ingesta depende del escenario de ingesta.|Sin dimensiones|
|IngestionResult|Sí|Resultado de la ingesta|Count|Total|Número total de orígenes que se lograron ingerir o no. Al dividir la métrica por estado, puede obtener información detallada sobre el estado de las operaciones de ingesta.|IngestionResultDetails, FailureKind|
|IngestionUtilization|Sí|Uso de la ingesta|Percent|Average|Proporción de ranuras de ingesta usadas en el clúster|Sin dimensiones|
|IngestionVolumeInMB|Sí|Volumen de ingesta|Bytes|Total|Volumen global de la ingesta de datos del clúster|Base de datos|
|InstanceCount|Sí|Recuento de instancias|Count|Average|Recuento total de instancias|Sin dimensiones|
|KeepAlive|Sí|Mantener conexión|Count|Average|La comprobación de integridad indica las respuestas del clúster a las consultas|Sin dimensiones|
|MaterializedViewAgeMinutes|Sí|Antigüedad de la vista materializada|Count|Average|Antigüedad de la vista materializada en minutos|Database, MaterializedViewName|
|MaterializedViewAgeSeconds|Sí|Antigüedad de la vista materializada|Segundos|Average|Antigüedad de la vista materializada en minutos.|Database, MaterializedViewName|
|MaterializedViewDataLoss|Sí|Pérdida de datos de la vista materializada|Count|Máxima|Indica una posible pérdida de datos en la vista materializada|Database, MaterializedViewName, Kind|
|MaterializedViewExtentsRebuild|Sí|Recompilaciones de extensiones de la vista materializada|Count|Average|Número de recompilaciones de extensiones|Database, MaterializedViewName|
|MaterializedViewHealth|Sí|Estado de la vista materializada|Count|Average|El estado de la vista materializada (1 para correcto, 0 para incorrecto)|Database, MaterializedViewName|
|MaterializedViewRecordsInDelta|Sí|Diferencial de registros de la vista materializada|Count|Average|El número de registros de la parte no materializada de la vista|Database, MaterializedViewName|
|MaterializedViewResult|Sí|Resultado de la vista materializada|Count|Average|Resultado del proceso de materialización|Database, MaterializedViewName, Result|
|QueryDuration|Sí|Duración de la consulta|Milisegundos|Average|Duración de las consultas en segundos|QueryStatus|
|QueryResult|No|Resultado de la consulta|Count|Count|Número total de consultas.|QueryStatus|
|QueueLength|Sí|Longitud de la cola|Count|Average|Número de mensajes pendientes en la cola de un componente.|ComponentType|
|QueueOldestMessage|Sí|Mensaje de cola más antiguo|Count|Average|Tiempo en segundos desde el momento en el que se insertó el mensaje más antiguo de la cola.|ComponentType|
|ReceivedDataSizeBytes|Sí|Bytes de tamaño de datos recibidos|Bytes|Average|Tamaño de los datos recibidos por la conexión de datos. Es el tamaño del flujo de datos o el tamaño de los datos sin procesar, si se proporciona.|ComponentType, ComponentName|
|StageLatency|Sí|Latencia de la fase|Segundos|Average|Tiempo acumulado desde el momento en que se detecta un mensaje hasta que lo recibe el componente de informes para su procesamiento (el tiempo de detección se establece en el momento en que el mensaje se pone en cola para la cola de ingesta o cuando lo detecta la conexión de datos).|Database, ComponentType|
|SteamingIngestRequestRate|Sí|Streaming Ingest Request Rate [Velocidad de solicitudes de ingesta de streaming]|Count|RateRequestsPerSecond|Tasa de solicitudes de la ingesta de streaming (solicitudes por segundo)|Sin dimensiones|
|StreamingIngestDataRate|Sí|Streaming Ingest Data Rate (Velocidad de datos de la ingesta de streaming)|Count|Average|Velocidad de datos de la ingesta de streaming (MB por segundo)|Sin dimensiones|
|StreamingIngestDuration|Sí|Streaming Ingest Duration (Duración de la ingesta de streaming)|Milisegundos|Average|Duración de la ingesta de streaming en milisegundos|Sin dimensiones|
|StreamingIngestResults|Sí|Streaming Ingest Result (Resultado de la ingesta de streaming)|Count|Average|Resultado de la ingesta de streaming|Resultado|
|TotalNumberOfConcurrentQueries|Sí|Número total de consultas simultáneas|Count|Máxima|Número total de consultas simultáneas|Sin dimensiones|
|TotalNumberOfExtents|Sí|Número total de extensiones|Count|Total|Número total de extensiones de datos|Sin dimensiones|
|TotalNumberOfThrottledCommands|Sí|Número total de comandos limitados|Count|Total|Número total de comandos limitados|CommandType|
|TotalNumberOfThrottledQueries|Sí|Número total de consultas limitadas|Count|Máxima|Número total de consultas limitadas|Sin dimensiones|
|WeakConsistencyLatency|Sí|Latencia de coherencia débil|Segundos|Average|Latencia máxima entre la sincronización de metadatos anterior y la siguiente (en el ámbito de base de datos o nodo)|Database, RoleInstance|


## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/IntegrationServiceEnvironments

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActionLatency|Sí|Latencia de acciones |Segundos|Average|Latencia de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionsCompleted|Sí|Acciones completadas |Count|Total|Número de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionsFailed|Sí|Acciones con errores |Count|Total|Número de acciones de flujo de trabajo con errores|Sin dimensiones|
|ActionsSkipped|Sí|Acciones omitidas |Count|Total|Número de acciones de flujo de trabajo omitidas|Sin dimensiones|
|ActionsStarted|Sí|Acciones iniciadas |Count|Total|Número de acciones de flujo de trabajo iniciadas|Sin dimensiones|
|ActionsSucceeded|Sí|Acciones correctas |Count|Total|Número de acciones de flujo de trabajo correctas|Sin dimensiones|
|ActionSuccessLatency|Sí|Latencia de acciones correctas |Segundos|Average|Latencia de acciones de flujo de trabajo correctas|Sin dimensiones|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Sí|Uso de la memoria del conector para el entorno del servicio de integración|Percent|Average|Uso de la memoria del conector para el entorno del servicio de integración.|Sin dimensiones|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Sí|Uso del procesador del conector para el entorno del servicio de integración|Percent|Average|Uso del procesador del conector para el entorno del servicio de integración.|Sin dimensiones|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|Sí|Uso del memoria de flujo de trabajo para el entorno del servicio de integración|Percent|Average|Uso del memoria de flujo de trabajo para el entorno del servicio de integración.|Sin dimensiones|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|Sí|Uso del procesador de flujo de trabajo para el entorno del servicio de integración|Percent|Average|Uso del procesador de flujo de trabajo para el entorno de servicio de integración.|Sin dimensiones|
|RunLatency|Sí|Latencia de ejecución|Segundos|Average|Latencia de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunsCancelled|Sí|Ejecuciones canceladas|Count|Total|Número de ejecuciones de flujo de trabajo canceladas|Sin dimensiones|
|RunsCompleted|Sí|Ejecuciones completadas|Count|Total|Número de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunsFailed|Sí|Ejecuciones con errores|Count|Total|Número de ejecuciones de flujo de trabajo con errores|Sin dimensiones|
|RunsStarted|Sí|Ejecuciones iniciadas|Count|Total|Número de ejecuciones de flujo de trabajo iniciadas|Sin dimensiones|
|RunsSucceeded|Sí|Ejecuciones correctas|Count|Total|Número de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|RunSuccessLatency|Sí|Latencia de ejecuciones correctas|Segundos|Average|Latencia de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|TriggerFireLatency|Sí|Latencia de desencadenador activado |Segundos|Average|Latencia de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggerLatency|Sí|Latencia de desencadenador |Segundos|Average|Latencia de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggersCompleted|Sí|Desencadenadores completados |Count|Total|Número de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggersFailed|Sí|Desencadenadores con errores |Count|Total|Número de desencadenadores de flujo de trabajo con errores|Sin dimensiones|
|TriggersFired|Sí|Desencadenadores activados |Count|Total|Número de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggersSkipped|Sí|Desencadenadores omitidos|Count|Total|Número de desencadenadores de flujo de trabajo omitidos|Sin dimensiones|
|TriggersStarted|Sí|Desencadenadores iniciados |Count|Total|Número de desencadenadores de flujo de trabajo iniciados|Sin dimensiones|
|TriggersSucceeded|Sí|Desencadenadores correctos |Count|Total|Número de desencadenadores de flujo de trabajo correctos|Sin dimensiones|
|TriggerSuccessLatency|Sí|Latencia de desencadenadores correctos |Segundos|Average|Latencia de desencadenadores de flujo de trabajo correctos|Sin dimensiones|


## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/Workflows

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActionLatency|Sí|Latencia de acciones |Segundos|Average|Latencia de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionsCompleted|Sí|Acciones completadas |Count|Total|Número de acciones de flujo de trabajo completadas|Sin dimensiones|
|ActionsFailed|Sí|Acciones con errores |Count|Total|Número de acciones de flujo de trabajo con errores|Sin dimensiones|
|ActionsSkipped|Sí|Acciones omitidas |Count|Total|Número de acciones de flujo de trabajo omitidas|Sin dimensiones|
|ActionsStarted|Sí|Acciones iniciadas |Count|Total|Número de acciones de flujo de trabajo iniciadas|Sin dimensiones|
|ActionsSucceeded|Sí|Acciones correctas |Count|Total|Número de acciones de flujo de trabajo correctas|Sin dimensiones|
|ActionSuccessLatency|Sí|Latencia de acciones correctas |Segundos|Average|Latencia de acciones de flujo de trabajo correctas|Sin dimensiones|
|ActionThrottledEvents|Sí|Eventos limitados de acciones|Count|Total|Número de eventos limitados de acciones de flujo de trabajo|Sin dimensiones|
|BillableActionExecutions|Sí|Ejecuciones de acciones facturables|Count|Total|Número de ejecuciones de acciones de flujo de trabajo que se facturan.|Sin dimensiones|
|BillableTriggerExecutions|Sí|Ejecuciones del desencadenador facturable|Count|Total|Número de ejecuciones del desencadenador de flujo de trabajo que se factura.|Sin dimensiones|
|BillingUsageNativeOperation|Sí|Uso de facturación para las ejecuciones de la operación nativa|Count|Total|Número de ejecuciones de la operación nativa que se facturan.|Sin dimensiones|
|BillingUsageStandardConnector|Sí|Uso de facturación para las ejecuciones del conector estándar|Count|Total|Número de ejecuciones del conector estándar que se facturan.|Sin dimensiones|
|BillingUsageStorageConsumption|Sí|Uso de facturación para las ejecuciones de consumo de almacenamiento|Count|Total|Número de ejecuciones del consumo de almacenamiento que se facturan.|Sin dimensiones|
|RunFailurePercentage|Sí|Porcentaje de errores de ejecución|Percent|Total|Porcentaje de ejecuciones de flujo de trabajo con errores.|Sin dimensiones|
|RunLatency|Sí|Latencia de ejecución|Segundos|Average|Latencia de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunsCancelled|Sí|Ejecuciones canceladas|Count|Total|Número de ejecuciones de flujo de trabajo canceladas|Sin dimensiones|
|RunsCompleted|Sí|Ejecuciones completadas|Count|Total|Número de ejecuciones de flujo de trabajo completadas|Sin dimensiones|
|RunsFailed|Sí|Ejecuciones con errores|Count|Total|Número de ejecuciones de flujo de trabajo con errores|Sin dimensiones|
|RunsStarted|Sí|Ejecuciones iniciadas|Count|Total|Número de ejecuciones de flujo de trabajo iniciadas|Sin dimensiones|
|RunsSucceeded|Sí|Ejecuciones correctas|Count|Total|Número de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|RunStartThrottledEvents|Sí|Ejecución de eventos limitados de inicio|Count|Total|Número de eventos limitados de inicio de ejecución de flujo de trabajo.|Sin dimensiones|
|RunSuccessLatency|Sí|Latencia de ejecuciones correctas|Segundos|Average|Latencia de ejecuciones de flujo de trabajo correctas|Sin dimensiones|
|RunThrottledEvents|Sí|Ejecución de eventos limitados|Count|Total|Número de eventos limitados de desencadenador o acciones de flujo de trabajo|Sin dimensiones|
|TotalBillableExecutions|Sí|Total de ejecuciones facturables|Count|Total|Número de ejecuciones de flujo de trabajo que se factura.|Sin dimensiones|
|TriggerFireLatency|Sí|Latencia de desencadenador activado |Segundos|Average|Latencia de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggerLatency|Sí|Latencia de desencadenador |Segundos|Average|Latencia de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggersCompleted|Sí|Desencadenadores completados |Count|Total|Número de desencadenadores de flujo de trabajo completados|Sin dimensiones|
|TriggersFailed|Sí|Desencadenadores con errores |Count|Total|Número de desencadenadores de flujo de trabajo con errores|Sin dimensiones|
|TriggersFired|Sí|Desencadenadores activados |Count|Total|Número de desencadenadores de flujo de trabajo activados|Sin dimensiones|
|TriggersSkipped|Sí|Desencadenadores omitidos|Count|Total|Número de desencadenadores de flujo de trabajo omitidos|Sin dimensiones|
|TriggersStarted|Sí|Desencadenadores iniciados |Count|Total|Número de desencadenadores de flujo de trabajo iniciados|Sin dimensiones|
|TriggersSucceeded|Sí|Desencadenadores correctos |Count|Total|Número de desencadenadores de flujo de trabajo correctos|Sin dimensiones|
|TriggerSuccessLatency|Sí|Latencia de desencadenadores correctos |Segundos|Average|Latencia de desencadenadores de flujo de trabajo correctos|Sin dimensiones|
|TriggerThrottledEvents|Sí|Eventos limitados de desencadenadores|Count|Total|Número de eventos limitados de desencadenador de flujo de trabajo|Sin dimensiones|


## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Active Cores (Núcleos activos)|Sí|Active Cores (Núcleos activos)|Count|Average|Número de núcleos activos|Scenario, ClusterName|
|Active Nodes (Nodos activos)|Sí|Active Nodes (Nodos activos)|Count|Average|Número de nodos activos. Son los nodos que ejecutan activamente un trabajo.|Scenario, ClusterName|
|Cancelar ejecuciones solicitadas|Sí|Cancelar ejecuciones solicitadas|Count|Total|Número de ejecuciones en las que se solicitó la cancelación para esta área de trabajo. El recuento se actualiza cuando se ha recibido la solicitud de cancelación de una ejecución.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Ejecuciones canceladas|Sí|Ejecuciones canceladas|Count|Total|Número de ejecuciones canceladas para esta área de trabajo. El recuento se actualiza cuando una ejecución se cancela correctamente.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Ejecuciones finalizadas|Sí|Ejecuciones finalizadas|Count|Total|Número de ejecuciones finalizadas correctamente para esta área de trabajo. El recuento se actualiza cuando se ha completado una ejecución y se ha recopilado la salida.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|CpuCapacityMillicores|Sí|CpuCapacityMillicores|Count|Average|Capacidad máxima de un nodo de CPU en milinúcleos. La capacidad se agrega en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|CpuMemoryCapacityMegabytes|Sí|CpuMemoryCapacityMegabytes|Count|Average|Uso máximo de memoria de un nodo de CPU en megabytes. El uso se agrega en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|CpuMemoryUtilizationMegabytes|Sí|CpuMemoryUtilizationMegabytes|Count|Average|Uso máximo de memoria de un nodo de CPU en megabytes. El uso se agrega en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|CpuMemoryUtilizationPercentage|Sí|CpuMemoryUtilizationPercentage|Count|Average|Porcentaje de uso de memoria de un nodo de CPU. El uso se agrega en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|CpuUtilization|Sí|CpuUtilization|Count|Average|Porcentaje de uso en un nodo de CPU. El uso se notifica en intervalos de un minuto.|Scenario, runId, NodeId, ClusterName|
|CpuUtilizationMillicores|Sí|CpuUtilizationMillicores|Count|Average|Uso de un nodo de CPU en milinúcleos. El uso se agrega en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|CpuUtilizationPercentage|Sí|CpuUtilizationPercentage|Count|Average|Porcentaje de uso de un nodo de CPU. El uso se agrega en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|Errors|Sí|Errors|Count|Total|Número de errores de ejecución en esta área de trabajo. El recuento se actualiza cada vez que la ejecución encuentra un error.|Escenario|
|Ejecuciones con error|Sí|Ejecuciones con error|Count|Total|Número de ejecuciones con error para esta área de trabajo. El recuento se actualiza cuando se produce un error en una ejecución.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Finalizando ejecuciones|Sí|Finalizando ejecuciones|Count|Total|Número de ejecuciones especificadas para finalizar el estado de esta área de trabajo. El recuento se actualiza cuando se ha completado una ejecución pero la recopilación de la salida todavía está en curso.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|GpuCapacityMilliGPUs|Sí|GpuCapacityMilliGPUs|Count|Average|Capacidad máxima de un dispositivo GPU en mili-GPU. La capacidad se agrega en intervalos de un minuto.|RunId, InstanceId, DeviceId, ComputeName|
|GpuEnergyJoules|Sí|GpuEnergyJoules|Count|Total|Energía en intervalos en Joules en un nodo de GPU. La energía se notifica en intervalos de un minuto.|Scenario, runId, rootRunId, InstanceId, DeviceId, ComputeName|
|GpuMemoryCapacityMegabytes|Sí|GpuMemoryCapacityMegabytes|Count|Average|Capacidad máxima de memoria de un dispositivo GPU en megabytes. La capacidad se agrega en intervalos de un minuto.|RunId, InstanceId, DeviceId, ComputeName|
|GpuMemoryUtilization|Sí|GpuMemoryUtilization|Count|Average|Porcentaje de uso de memoria en un nodo de GPU. El uso se notifica en intervalos de un minuto.|Scenario, runId, NodeId, DeviceId, ClusterName|
|GpuMemoryUtilizationMegabytes|Sí|GpuMemoryUtilizationMegabytes|Count|Average|Uso de memoria de un dispositivo GPU en megabytes. La utilización se agrega en intervalos de un minuto.|RunId, InstanceId, DeviceId, ComputeName|
|GpuMemoryUtilizationPercentage|Sí|GpuMemoryUtilizationPercentage|Count|Average|Porcentaje de uso de memoria de un dispositivo GPU. La utilización se agrega en intervalos de un minuto.|RunId, InstanceId, DeviceId, ComputeName|
|GpuUtilization|Sí|GpuUtilization|Count|Average|Porcentaje de uso en un nodo de GPU. El uso se notifica en intervalos de un minuto.|Scenario, runId, NodeId, DeviceId, ClusterName|
|GpuUtilizationMilliGPUs|Sí|GpuUtilizationMilliGPUs|Count|Average|Uso de un dispositivo GPU en mili-GPU. El uso se agrega en intervalos de un minuto.|RunId, InstanceId, DeviceId, ComputeName|
|GpuUtilizationPercentage|Sí|GpuUtilizationPercentage|Count|Average|Porcentaje de uso de un dispositivo GPU. El uso se agrega en intervalos de un minuto.|RunId, InstanceId, DeviceId, ComputeName|
|IBReceiveMegabytes|Sí|IBReceiveMegabytes|Count|Average|Datos de red recibidos a través de InfiniBand en megabytes. Las métricas se agregan en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|IBTransmitMegabytes|Sí|IBTransmitMegabytes|Count|Average|Datos de red enviados a través de InfiniBand en megabytes. Las métricas se agregan en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|Idle Cores (Núcleos inactivos)|Sí|Idle Cores (Núcleos inactivos)|Count|Average|Número de núcleos inactivos|Scenario, ClusterName|
|Idle Nodes (Nodos inactivos)|Sí|Idle Nodes (Nodos inactivos)|Count|Average|Número de nodos inactivos. Los nodos inactivos son los nodos que no ejecutan ningún trabajo, pero que pueden aceptar un nuevo trabajo si está disponible.|Scenario, ClusterName|
|Leaving Cores (Núcleos de salida)|Sí|Leaving Cores (Núcleos de salida)|Count|Average|Número de núcleos de salida|Scenario, ClusterName|
|Leaving Nodes (Nodos de salida)|Sí|Leaving Nodes (Nodos de salida)|Count|Average|Número de nodos de salida. Los nodos de salida son los nodos que acaban de procesar un trabajo y pasan al estado inactivo.|Scenario, ClusterName|
|Error al realizar la implementación de modelo|Sí|Error al realizar la implementación de modelo|Count|Total|Número de implementaciones de modelo erróneas de este espacio de trabajo.|Scenario, StatusCode|
|Model Deploy Started (Implementación de modelo iniciada)|Sí|Model Deploy Started (Implementación de modelo iniciada)|Count|Total|Número de implementaciones de modelos que se iniciaron en este espacio de trabajo.|Escenario|
|Model Deploy Succeeded|Sí|Model Deploy Succeeded (Implementación de modelo correcta)|Count|Total|Número de implementaciones de modelos que se realizaron correctamente en este espacio de trabajo.|Escenario|
|Model Register Failed|Sí|Model Register Failed (Error en el registro de modelo)|Count|Total|Número de registros de modelo erróneos de este espacio de trabajo.|Scenario, StatusCode|
|Model Register Succeeded (Registro de modelo realizado correctamente)|Sí|Model Register Succeeded (Registro de modelo realizado correctamente)|Count|Total|Número de registros de modelo que se realizaron correctamente en este espacio de trabajo.|Escenario|
|NetworkInputMegabytes|Sí|NetworkInputMegabytes|Count|Average|Datos de red recibidos en megabytes. Las métricas se agregan en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|NetworkOutputMegabytes|Sí|NetworkOutputMegabytes|Count|Average|Datos de red enviados en megabytes. Las métricas se agregan en intervalos de un minuto.|RunId, InstanceId, ComputeName|
|Ejecuciones que no responden|Sí|Ejecuciones que no responden|Count|Total|Número de ejecuciones que no responden para esta área de trabajo. El recuento se actualiza cuando una ejecución entra en el estado No responde.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Ejecuciones no iniciadas|Sí|Ejecuciones no iniciadas|Count|Total|Número de ejecuciones en estado No iniciada para esta área de trabajo. El recuento se actualiza cuando se recibe una solicitud para crear una ejecución, pero aún no se ha rellenado la información de la ejecución. |Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Preempted Cores (Núcleos con prioridad)|Sí|Preempted Cores (Núcleos con prioridad)|Count|Average|Número de núcleos con prioridad|Scenario, ClusterName|
|Preempted Nodes (Nodos con prioridad)|Sí|Preempted Nodes (Nodos con prioridad)|Count|Average|Número de nodos con prioridad. Estos nodos son los nodos de prioridad baja que se quitan del grupo de nodos disponible.|Scenario, ClusterName|
|Preparando ejecuciones|Sí|Preparando ejecuciones|Count|Total|Número de ejecuciones que se están preparando para esta área de trabajo. El recuento se actualiza cuando una ejecución entra en el estado Preparándose mientras se prepara el entorno de ejecución.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Aprovisionamiento de ejecuciones|Sí|Aprovisionamiento de ejecuciones|Count|Total|Número de ejecuciones que se están aprovisionando para esta área de trabajo. El recuento se actualiza cuando una ejecución está esperando la creación o el aprovisionamiento del destino de proceso.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Ejecuciones en cola|Sí|Ejecuciones en cola|Count|Total|Número de ejecuciones en cola para esta área de trabajo. El recuento se actualiza cuando una ejecución se pone en cola en el destino de proceso. Puede producirse cuando se espera a que los nodos de proceso necesarios estén listos.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Porcentaje de uso de la cuota|Sí|Porcentaje de uso de la cuota|Count|Average|Porcentaje de cuota utilizada|Scenario, ClusterName, VmFamilyName, VmPriority|
|Ejecuciones iniciadas|Sí|Ejecuciones iniciadas|Count|Total|Número de ejecuciones en curso para esta área de trabajo. El recuento se actualiza cuando la ejecución se inicia en los recursos necesarios.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Iniciando ejecuciones|Sí|Iniciando ejecuciones|Count|Total|Número de ejecuciones iniciadas para esta área de trabajo. El recuento se actualiza después de que se hayan rellenado la solicitud para crear la ejecución y la información de ejecución, como el identificador de ejecución|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Total Cores (Núcleos totales)|Sí|Total Cores (Núcleos totales)|Count|Average|Número de núcleos totales|Scenario, ClusterName|
|Total Nodes|Sí|Total Nodes (Nodos totales)|Count|Average|Número de nodos totales. Este total incluye algunos de los nodos activos, nodos inactivos, nodos inutilizables, nodos con prioridad, nodos de salida.|Scenario, ClusterName|
|Unusable Cores|Sí|Unusable Cores (Núcleos no utilizables)|Count|Average|Número de núcleos no utilizables|Scenario, ClusterName|
|Nodos no utilizables|Sí|Nodos no utilizables|Count|Average|Número de nodos inutilizables. Los nodos inutilizables no funcionan debido a algún problema que no se puede resolver. Azure reciclará estos nodos.|Scenario, ClusterName|
|Advertencias|Sí|Advertencias|Count|Total|Número de advertencias de ejecución en esta área de trabajo. El recuento se actualiza cada vez que una ejecución encuentra una advertencia.|Escenario|


## <a name="microsoftmapsaccounts"></a>Microsoft.Maps/accounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|Disponibilidad de las API|ApiCategory, ApiName|
|CreatorUsage|No|Uso de Creator|Bytes|Average|Estadísticas de uso de Creator en Azure Maps|ServiceName|
|Uso|No|Uso|Count|Count|Recuento de llamadas a la API|ApiCategory, ApiName, ResultType, ResponseCode|


## <a name="microsoftmediamediaservices"></a>Microsoft.Media/mediaservices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AssetCount|Sí|Número de recursos|Count|Average|Cantidad de recursos creados en la cuenta de servicio multimedia actual|Sin dimensiones|
|AssetQuota|Sí|Cuota de recursos|Count|Average|Cantidad de recursos permitidos para la cuenta de servicio multimedia actual|Sin dimensiones|
|AssetQuotaUsedPercentage|Sí|Porcentaje de cuota de recursos usado|Percent|Average|Porcentaje de recursos usado en la cuenta de servicio multimedia actual|Sin dimensiones|
|ChannelsAndLiveEventsCount|Sí|Número de eventos en directo|Count|Average|El número total de eventos en directo de la cuenta de Media Services actual|Sin dimensiones|
|ContentKeyPolicyCount|Sí|Número de directivas de clave de contenido|Count|Average|Cantidad de directivas de clave de contenido creadas en la cuenta de servicio multimedia actual|Sin dimensiones|
|ContentKeyPolicyCount|Sí|Cuota de directiva de clave de contenido|Count|Average|Cantidad de directivas de clave de contenido permitida para la cuenta de servicio multimedia actual|Sin dimensiones|
|ContentKeyPolicyQuotaUsedPercentage|Sí|Porcentaje utilizado de la cuota de directivas de clave de contenido|Percent|Average|Porcentaje de directivas de clave de contenido usado en la cuenta de servicio multimedia actual|Sin dimensiones|
|JobQuota|Sí|Cuota de trabajos|Count|Average|Cuota de trabajos de la cuenta de servicio multimedia actual.|Sin dimensiones|
|JobsScheduled|Sí|Trabajos programados|Count|Average|Número de trabajos en estado programado. Los recuentos de esta métrica solo reflejan los trabajos enviados a través de la API v3. Los trabajos enviados a través de la API v2 (heredada) no se cuentan.|Sin dimensiones|
|MaxChannelsAndLiveEventsCount|Sí|Cuota máxima de eventos en directo|Count|Average|El número máximo de eventos en directo permitidos en la cuenta de Media Services actual|Sin dimensiones|
|MaxRunningChannelsAndLiveEventsCount|Sí|Cuota máxima de eventos en directo en ejecución|Count|Average|El número máximo de eventos en directo en ejecución permitidos en la cuenta de Media Services actual|Sin dimensiones|
|RunningChannelsAndLiveEventsCount|Sí|Número de eventos en directo en ejecución|Count|Average|El número total de eventos en directo en ejecución de la cuenta de Media Services actual|Sin dimensiones|
|StreamingPolicyCount|Sí|Número de directivas de streaming|Count|Average|Cantidad de directivas de streaming creadas en la cuenta de servicio multimedia actual|Sin dimensiones|
|StreamingPolicyQuota|Sí|Cuota de directivas de streaming|Count|Average|Cantidad de directivas de streaming permitidas para la cuenta de servicio multimedia actual|Sin dimensiones|
|StreamingPolicyQuotaUsedPercentage|Sí|Porcentaje usado de la cuota de directiva de streaming|Percent|Average|Porcentaje de directivas de streaming usado en la cuenta de servicio multimedia actual|Sin dimensiones|
|TransformQuota|Sí|Cuota de transformación|Count|Average|Cuota de transformación de la cuenta de servicio multimedia actual.|Sin dimensiones|


## <a name="microsoftmediamediaservicesliveevents"></a>Microsoft.Media/mediaservices/liveEvents

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|IngestBitrate|Sí|Velocidad de bits de ingesta de eventos en directo|BitsPerSecond|Average|La velocidad de bits entrante ingerida para un evento en directo, en bits por segundo.|TrackName|
|IngestDriftValue|Sí|Valor de desfase de ingesta de eventos en directo|Segundos|Máxima|Desfase entre la marca de tiempo del contenido ingerido y el reloj del sistema, medido en segundos por minuto. Un valor distinto de cero indica que el contenido ingerido va más lento que el tiempo del reloj del sistema.|TrackName|
|IngestLastTimestamp|Sí|Última marca de tiempo de la ingesta de eventos en directo|Milisegundos|Máxima|Última marca de tiempo de la ingesta para un evento en directo.|TrackName|
|LiveOutputLastTimestamp|Sí|Marca de tiempo de la última salida|Milisegundos|Máxima|Marca de tiempo del último fragmento cargado en el almacenamiento para una salida de evento en directo.|TrackName|


## <a name="microsoftmediamediaservicesstreamingendpoints"></a>Microsoft.Media/mediaservices/streamingEndpoints

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|CPU|Sí|Uso de CPU|Percent|Average|Uso de CPU para puntos de conexión de streaming prémium. Estos datos no están disponibles para los puntos de conexión de streaming estándar.|Sin dimensiones|
|Salida|Sí|Salida|Bytes|Total|Cantidad de datos de salida, en bytes.|OutputFormat|
|EgressBandwidth|No|Ancho de banda de salida|BitsPerSecond|Average|Ancho de banda de salida en bits por segundo.|Sin dimensiones|
|Requests|Sí|Requests|Count|Total|Solicitudes a un punto de conexión de streaming.|OutputFormat, HttpStatusCode, ErrorCode|
|SuccessE2ELatency|Sí|Latencia correcta de extremo a extremo|Milisegundos|Average|Latencia media de las solicitudes correctas en milisegundos.|OutputFormat|


## <a name="microsoftmediavideoanalyzers"></a>Microsoft.Media/videoanalyzers

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|IngressBytes|Sí|Bytes de entrada|Bytes|Total|Número de bytes introducidos por el nodo de canalización.|PipelineTopology, canalización, nodo|


## <a name="microsoftmixedrealityremoterenderingaccounts"></a>Microsoft.MixedReality/remoteRenderingAccounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActiveRenderingSessions|Sí|Sesiones de representación activa|Count|Average|Número total de sesiones de representación activas|SessionType, SDKVersion|
|AssetsConverted|Sí|Activos convertidos|Count|Total|Número total de activos convertidos|SDKVersion|


## <a name="microsoftmixedrealityspatialanchorsaccounts"></a>Microsoft.MixedReality/spatialAnchorsAccounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AnchorsCreated|Sí|Anclajes creados|Count|Total|Número de anclajes creados|DeviceFamily, SDKVersion|
|AnchorsDeleted|Sí|Anclajes eliminados|Count|Total|Número de anclajes eliminados|DeviceFamily, SDKVersion|
|AnchorsQueried|Sí|Anclajes consultados|Count|Total|Número de anclajes espaciales consultados|DeviceFamily, SDKVersion|
|AnchorsUpdated|Sí|Anclajes actualizados|Count|Total|Número de anclajes actualizados|DeviceFamily, SDKVersion|
|PosesFound|Sí|Posturas encontradas|Count|Total|Número de posturas devueltas|DeviceFamily, SDKVersion|
|TotalDailyAnchors|Sí|Total de anclajes diarios|Count|Average|Número total de anclajes: diario|DeviceFamily, SDKVersion|


## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Sí|Tamaño asignado del grupo|Bytes|Average|Tamaño aprovisionado de este grupo|Sin dimensiones|
|VolumePoolAllocatedToVolumeThroughput|Sí|Rendimiento asignado al grupo|BytesPerSecond|Average|Suma del rendimiento de todos los volúmenes que pertenecen al grupo|Sin dimensiones|
|VolumePoolAllocatedUsed|Sí|Grupo asignado al tamaño del volumen|Bytes|Average|Tamaño usado asignado del grupo|Sin dimensiones|
|VolumePoolProvisionedThroughput|Sí|Rendimiento aprovisionado para el grupo|BytesPerSecond|Average|Rendimiento aprovisionado de este grupo|Sin dimensiones|
|VolumePoolTotalLogicalSize|Sí|Tamaño consumido del grupo|Bytes|Average|Suma del tamaño lógico de todos los volúmenes que pertenecen al grupo|Sin dimensiones|
|VolumePoolTotalSnapshotSize|Sí|Tamaño de instantánea total del grupo|Bytes|Average|Suma del tamaño de la instantánea de todos los volúmenes de este grupo|Sin dimensiones|


## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/volumes

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AverageReadLatency|Sí|Latencia de lectura media|MilliSeconds|Average|Latencia de lectura media en milisegundos por operación|Sin dimensiones|
|AverageWriteLatency|Sí|Latencia de escritura media|MilliSeconds|Average|Latencia de escritura media en milisegundos por operación|Sin dimensiones|
|CbsVolumeBackupActive|Sí|¿Se ha suspendido la copia de seguridad del volumen?|Count|Average|¿Está la directiva de copia de seguridad suspendida para el volumen? 0 si es así, 1 si no lo es.|Sin dimensiones|
|CbsVolumeLogicalBackupBytes|Sí|Bytes de copia de seguridad del volumen|Bytes|Average|Total de bytes de copia de seguridad para este volumen.|Sin dimensiones|
|CbsVolumeOperationComplete|Sí|¿Se ha completado la operación de copia de seguridad del volumen?|Count|Average|¿Se completó correctamente la última operación de copia de seguridad o restauración del volumen? 1 si es así, 0 si no lo es.|Sin dimensiones|
|CbsVolumeOperationTransferredBytes|Sí|Últimos bytes transferidos de la copia de seguridad del volumen|Bytes|Average|Bytes totales transferidos de la última operación de copia de seguridad o restauración.|Sin dimensiones|
|CbsVolumeProtected|Sí|¿Está habilitada la copia de seguridad del volumen?|Count|Average|¿Está habilitada la copia de seguridad del volumen? 1 si es así, 0 si no lo es.|Sin dimensiones|
|OtherThroughput|Sí|Otro rendimiento|BytesPerSecond|Average|Otro rendimiento (que no sea de lectura ni escritura) en bytes por segundo|Sin dimensiones|
|ReadIops|Sí|IOPS de lectura|CountPerSecond|Average|Operaciones de entrada/salida de lectura por segundo|Sin dimensiones|
|ReadThroughput|Sí|Rendimiento de lectura|BytesPerSecond|Average|Rendimiento de lectura en bytes por segundo|Sin dimensiones|
|TotalThroughput|Sí|Rendimiento total|BytesPerSecond|Average|Suma de todo el rendimiento en bytes por segundo|Sin dimensiones|
|VolumeAllocatedSize|Sí|Tamaño asignado del volumen|Bytes|Average|Tamaño aprovisionado de un volumen|Sin dimensiones|
|VolumeConsumedSizePercentage|Sí|Porcentaje de tamaño consumido del volumen|Percent|Average|Porcentaje del volumen consumido, incluidas las instantáneas.|Sin dimensiones|
|VolumeCoolTierDataReadSize|Sí|Tamaño de lectura de datos de nivel de acceso esporádico del volumen|Bytes|Average|Datos leídos usando GET por volumen|Sin dimensiones|
|VolumeCoolTierDataWriteSize|Sí|Tamaño de escritura de datos del nivel de acceso esporádico del volumen|Bytes|Average|Datos en capas usando PUT por volumen|Sin dimensiones|
|VolumeCoolTierSize|Sí|Tamaño del nivel de acceso esporádico del volumen|Bytes|Average|Superficie de volumen para el nivel de acceso esporádico|Sin dimensiones|
|VolumeLogicalSize|Sí|Tamaño consumido del volumen|Bytes|Average|Tamaño lógico del volumen (bytes usados)|Sin dimensiones|
|VolumeSnapshotSize|Sí|Tamaño de instantánea de volumen|Bytes|Average|Tamaño de todas las instantáneas del volumen|Sin dimensiones|
|WriteIops|Sí|IOPS de escritura|CountPerSecond|Average|Operaciones de entrada/salida de escritura por segundo|Sin dimensiones|
|WriteThroughput|Sí|Rendimiento de escritura|BytesPerSecond|Average|Rendimiento de escritura en bytes por segundo|Sin dimensiones|
|XregionReplicationHealthy|Sí|¿Es correcto el estado de replicación del volumen?|Count|Average|Condición de la relación, 1 o 0.|Sin dimensiones|
|XregionReplicationLagTime|Sí|Tiempo de retardo de replicación del volumen|Segundos|Average|Cantidad de tiempo en segundos que los datos del reflejo se retrasan detrás del origen.|Sin dimensiones|
|XregionReplicationLastTransferDuration|Sí|Duración de la última transferencia de replicación de volumen|Segundos|Average|Cantidad de tiempo en segundos que tardó en completarse la última transferencia.|Sin dimensiones|
|XregionReplicationLastTransferSize|Sí|Tamaño de última transferencia de replicación de volumen|Bytes|Average|Número total de bytes transferidos como parte de la última transferencia.|Sin dimensiones|
|XregionReplicationRelationshipProgress|Sí|Progreso de la replicación del volumen|Bytes|Average|Cantidad total de datos transferidos para la operación de transferencia actual.|Sin dimensiones|
|XregionReplicationRelationshipTransferring|Sí|¿Se está transfiriendo la replicación del volumen?|Count|Average|Indica si el estado de la replicación del volumen es "transfiriéndose".|Sin dimensiones|
|XregionReplicationTotalTransferBytes|Sí|Transferencia total de la replicación del volumen|Bytes|Average|Bytes acumulados transferidos para la relación.|Sin dimensiones|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationgateways

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ApplicationGatewayTotalTime|No|Application Gateway Total Time (Tiempo total de Application Gateway)|MilliSeconds|Average|Promedio de tiempo que se tarda en procesar una solicitud y en enviar la respuesta. Esto se calcula como promedio del intervalo desde el momento en que Application Gateway recibe el primer byte de una solicitud HTTP hasta el momento en que termina la operación de envío de la respuesta. Es importante tener en cuenta que esto normalmente incluye el tiempo de procesamiento de Application Gateway, el tiempo que los paquetes de solicitud y respuesta viajan por la red y el tiempo que el servidor back-end tardó en responder.|Agente de escucha|
|AvgRequestCountPerHealthyHost|No|Requests per minute per Healthy Host (Solicitudes por minuto y host con estado correcto)|Count|Average|Recuento medio de solicitudes por minuto por host de back-end con estado correcto de un grupo|BackendSettingsPool|
|BackendConnectTime|No|Backend Connect Time (Tiempo de conexión de back-end)|MilliSeconds|Average|Tiempo empleado en establecer una conexión con un servidor back-end.|Listener, BackendServer, BackendPool, BackendHttpSetting|
|BackendFirstByteResponseTime|No|Backend First Byte Response Time (Tiempo de respuesta del primer byte de back-end)|MilliSeconds|Average|Intervalo de tiempo entre el inicio del establecimiento de una conexión con el servidor back-end y la recepción del primer byte del encabezado de respuesta, el tiempo de procesamiento aproximado del servidor back-end|Listener, BackendServer, BackendPool, BackendHttpSetting|
|BackendLastByteResponseTime|No|Backend Last Byte Response Time (Tiempo de respuesta del último byte de back-end)|MilliSeconds|Average|Intervalo de tiempo entre el inicio del establecimiento de una conexión con el servidor back-end y la recepción del último byte del cuerpo de la respuesta|Listener, BackendServer, BackendPool, BackendHttpSetting|
|BackendResponseStatus|Sí|Backend Response Status (Estado de respuesta de back-end)|Count|Total|Número de códigos de respuesta HTTP generados por los miembros de back-end No se incluyen los códigos de respuesta generados por Application Gateway.|BackendServer, BackendPool, BackendHttpSetting, HttpStatusGroup|
|BackendTlsNegotiationError|Sí|Errores de conexión TLS de back-end|Count|Total|Errores de conexión TLS para back-end de Application Gateway|BackendHttpSetting, BackendPool, ErrorType|
|BlockedCount|Sí|Web Application Firewall Blocked Requests Rule Distribution (Distribución de reglas de solicitudes bloqueadas del firewall de aplicaciones web)|Count|Total|Distribución de reglas de solicitudes bloqueadas del firewall de aplicaciones web|RuleGroup, RuleId|
|BlockedReqCount|Sí|Web Application Firewall Blocked Requests Count (Recuento de solicitudes bloqueadas del firewall de aplicaciones web)|Count|Total|Recuento de solicitudes bloqueadas del firewall de aplicaciones web|Sin dimensiones|
|BytesReceived|Sí|Bytes recibidos|Bytes|Total|Número total de bytes recibidos por Application Gateway de los clientes|Agente de escucha|
|BytesSent|Sí|Bytes enviados|Bytes|Total|Número total de bytes enviados por Application Gateway a los clientes|Agente de escucha|
|CapacityUnits|No|Unidades de capacidad actuales|Count|Average|Unidades de capacidad consumidas|Sin dimensiones|
|ClientRtt|No|Client RTT (RTT del cliente)|MilliSeconds|Average|Tiempo medio de ida y vuelta entre clientes y Application Gateway. Esta métrica indica cuánto tiempo se tarda en establecer conexiones y devolver confirmaciones.|Agente de escucha|
|ComputeUnits|No|Current Compute Units (Unidades de proceso actuales)|Count|Average|Unidades de proceso consumidas|Sin dimensiones|
|CpuUtilization|No|Uso de CPU|Percent|Average|Uso actual de la CPU de Application Gateway|Sin dimensiones|
|CurrentConnections|Sí|Conexiones actuales|Count|Total|Recuento de conexiones actuales establecidas con Application Gateway|Sin dimensiones|
|EstimatedBilledCapacityUnits|No|Unidades de capacidad facturadas estimadas|Count|Average|Unidades de capacidad estimadas que se cobrarán|Sin dimensiones|
|FailedRequests|Sí|Solicitudes con error|Count|Total|Recuento de solicitudes con error que ha servido Application Gateway|BackendSettingsPool|
|FixedBillableCapacityUnits|No|Unidades de capacidad facturables fijas|Count|Average|Unidades de capacidad mínima que se cobrarán|Sin dimensiones|
|HealthyHostCount|Sí|Recuento de hosts con estado correcto|Count|Average|Número de hosts de back-end con estado correcto|BackendSettingsPool|
|MatchedCount|Sí|Web Application Firewall Total Rule Distribution (Distribución de reglas totales del firewall de aplicaciones web)|Count|Total|Distribución de reglas totales del firewall de aplicaciones web para el tráfico entrante|RuleGroup, RuleId|
|NewConnectionsPerSecond|No|Nuevas conexiones por segundo|CountPerSecond|Average|Nuevas conexiones por segundo establecidas con Application Gateway|Sin dimensiones|
|RejectedConnections|Sí|Conexiones rechazadas|Count|Total|Recuento de conexiones rechazadas para el front-end de Application Gateway|Sin dimensiones|
|ResponseStatus|Sí|Estado de respuesta|Count|Total|Estado de respuesta HTTP devuelto por Application Gateway|HttpStatusGroup|
|Throughput|No|Throughput|BytesPerSecond|Average|Número de bytes por segundo que ha ofrecido Application Gateway|Sin dimensiones|
|TlsProtocol|Sí|Client TLS Protocol (Protocolo TLS de cliente)|Count|Total|Número de solicitudes TLS y no TLS iniciadas por el cliente que establecieron la conexión con Application Gateway. Para ver la distribución del protocolo TLS, filtre por el protocolo TLS de la dimensión.|Listener, TlsProtocol|
|TotalRequests|Sí|Total de solicitudes|Count|Total|Recuento de solicitudes correctas que ha servido Application Gateway|BackendSettingsPool|
|UnhealthyHostCount|Sí|Recuento de hosts con estado incorrecto|Count|Average|Número de hosts de back-end con estado incorrecto|BackendSettingsPool|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ApplicationRuleHit|Sí|Recuento de llamadas de las reglas de aplicación|Count|Total|Número de veces que se llamó a las reglas de aplicación|Status, Reason, Protocol|
|DataProcessed|Sí|Datos procesados|Bytes|Total|Cantidad total de datos procesados por este firewall|Sin dimensiones|
|FirewallHealth|Sí|Firewall health state (Estado de mantenimiento del firewall)|Percent|Average|Indica el mantenimiento general de este firewall|Status, Reason|
|NetworkRuleHit|Sí|Número de llamadas de reglas de red|Count|Total|Número de veces que se llamó a las reglas de red|Status, Reason, Protocol|
|SNATPortUtilization|Sí|SNAT port utilization (Uso de puertos SNAT)|Percent|Average|Porcentaje de puertos SNAT salientes actualmente en uso|Protocolo|
|Throughput|No|Throughput|BitsPerSecond|Average|Rendimiento procesado por este firewall|Sin dimensiones|


## <a name="microsoftnetworkbastionhosts"></a>microsoft.network/bastionHosts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Pingmesh|No|Estado de la comunicación de Bastion|Count|Average|El estado de la comunicación muestra 1 si toda la comunicación es correcta y 0 si es incorrecta.|Sin dimensiones|
|sesiones|No|Recuento de sesiones|Count|Total|Recuento de sesiones de Bastion. Ver en suma y por instancia.|host|
|total|Sí|Memoria total|Count|Average|Estadísticas de memoria total.|host|
|usage_user|No|Uso de CPU|Count|Average|Estadísticas de uso de CPU.|CPU, host|
|usados|Sí|Uso de la memoria|Count|Average|Estadísticas de uso de memoria.|host|


## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BitsInPerSecond|Sí|BitsInPerSecond|BitsPerSecond|Average|Bits que ingresan a Azure por segundo|Sin dimensiones|
|BitsOutPerSecond|Sí|BitsOutPerSecond|BitsPerSecond|Average|Bits que egresan de Azure por segundo|Sin dimensiones|


## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|QueryVolume|No|Volumen de consulta|Count|Total|Número de consultas que se sirven para una zona DNS|Sin dimensiones|
|RecordSetCapacityUtilization|No|Uso de capacidad de conjunto de registros|Percent|Máxima|Porcentaje de capacidad de conjunto de registros que usa una zona DNS|Sin dimensiones|
|RecordSetCount|No|Recuento de conjunto de registros|Count|Máxima|Número de conjuntos de registros de una zona DNS|Sin dimensiones|


## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ArpAvailability|Sí|Disponibilidad de ARP|Percent|Average|Disponibilidad de ARP de MSEE a todos los elementos del mismo nivel.|PeeringType, Peer|
|BgpAvailability|Sí|Disponibilidad de BGP|Percent|Average|Disponibilidad de BGP de MSEE a todos los elementos del mismo nivel.|PeeringType, Peer|
|BitsInPerSecond|Sí|BitsInPerSecond|BitsPerSecond|Average|Bits que ingresan a Azure por segundo|PeeringType, DeviceRole|
|BitsOutPerSecond|Sí|BitsOutPerSecond|BitsPerSecond|Average|Bits que egresan de Azure por segundo|PeeringType, DeviceRole|
|GlobalReachBitsInPerSecond|No|GlobalReachBitsInPerSecond|BitsPerSecond|Average|Bits que ingresan a Azure por segundo|PeeredCircuitSKey|
|GlobalReachBitsOutPerSecond|No|GlobalReachBitsOutPerSecond|BitsPerSecond|Average|Bits que egresan de Azure por segundo|PeeredCircuitSKey|
|QosDropBitsInPerSecond|Sí|DroppedInBitsPerSecond|BitsPerSecond|Average|Bits de entrada de datos descartados por segundo|Sin dimensiones|
|QosDropBitsOutPerSecond|Sí|DroppedOutBitsPerSecond|BitsPerSecond|Average|Bits de salida de datos descartados por segundo|Sin dimensiones|


## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BitsInPerSecond|Sí|BitsInPerSecond|BitsPerSecond|Average|Bits que ingresan a Azure por segundo|Sin dimensiones|
|BitsOutPerSecond|Sí|BitsOutPerSecond|BitsPerSecond|Average|Bits que egresan de Azure por segundo|Sin dimensiones|


## <a name="microsoftnetworkexpressroutegateways"></a>Microsoft.Network/expressRouteGateways

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ErGatewayConnectionBitsInPerSecond|No|BitsInPerSecond|BitsPerSecond|Average|Bits que ingresan a Azure por segundo|ConnectionName|
|ErGatewayConnectionBitsOutPerSecond|No|BitsOutPerSecond|BitsPerSecond|Average|Bits que egresan de Azure por segundo|ConnectionName|
|ExpressRouteGatewayCountOfRoutesAdvertisedToPeer|Sí|Recuento de rutas anunciadas al par (versión preliminar)|Count|Máxima|Recuento de rutas anunciadas al par por ExpressRouteGateway|roleInstance|
|ExpressRouteGatewayCountOfRoutesLearnedFromPeer|Sí|Recuento de rutas aprendidas del par (versión preliminar)|Count|Máxima|Recuento de rutas aprendidas del par por ExpressRouteGateway|roleInstance|
|ExpressRouteGatewayCpuUtilization|Sí|Uso de CPU|Percent|Average|Uso de CPU de la puerta de enlace de ExpressRoute|roleInstance|
|ExpressRouteGatewayFrequencyOfRoutesChanged|No|Frecuencia de cambio de rutas (versión preliminar)|Count|Total|Frecuencia de cambio de rutas en la puerta de enlace de ExpressRoute|roleInstance|
|ExpressRouteGatewayNumberOfVmInVnet|No|Número de máquinas virtuales en la red virtual (versión preliminar)|Count|Máxima|Número de máquinas virtuales en la red virtual|Sin dimensiones|
|ExpressRouteGatewayPacketsPerSecond|No|Paquetes por segundo|CountPerSecond|Average|Recuento de paquetes de puerta de enlace de ExpressRoute|roleInstance|


## <a name="microsoftnetworkexpressrouteports"></a>Microsoft.Network/expressRoutePorts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AdminState|Sí|AdminState|Count|Average|Estado de administración del puerto|Vínculo|
|LineProtocol|Sí|LineProtocol|Count|Average|Estado del protocolo de línea del puerto|Vínculo|
|PortBitsInPerSecond|No|BitsInPerSecond|BitsPerSecond|Average|Bits que ingresan a Azure por segundo|Vínculo|
|PortBitsOutPerSecond|No|BitsOutPerSecond|BitsPerSecond|Average|Bits que egresan de Azure por segundo|Vínculo|
|RxLightLevel|Sí|RxLightLevel|Count|Average|Nivel de iluminación de recepción en dBm|Link, Lane|
|TxLightLevel|Sí|TxLightLevel|Count|Average|Nivel de iluminación de transmisión en dBm|Link, Lane|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BackendHealthPercentage|Sí|Porcentaje de estado del back-end|Percent|Average|El porcentaje de sondeos de estado correctos del proxy HTTP/S a los back-ends|Backend, BackendPool|
|BackendRequestCount|Sí|Recuento de solicitudes de back-end|Count|Total|Número de solicitudes enviadas del proxy HTTP/S a los back-end|HttpStatus, HttpStatusGroup, Backend|
|BackendRequestLatency|Sí|Latencia de las solicitudes de back-end|MilliSeconds|Average|Tiempo calculado desde que el proxy HTTP/S ha enviado la solicitud al back-end hasta que el proxy HTTP/S ha recibido el último byte de respuesta del back-end|Back-end|
|BillableResponseSize|Sí|Billable Response Size (Tamaño de respuesta facturable)|Bytes|Total|Número de bytes facturables (mínimo de 2 KB por solicitud) enviados como respuestas del proxy HTTP/S a los clientes.|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestCount|Sí|Recuento de solicitudes|Count|Total|Número de solicitudes de cliente que atiende el proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Sí|Tamaño de la solicitud|Bytes|Total|Número de bytes enviados como solicitudes de clientes al proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Sí|Tamaño de la respuesta|Bytes|Total|Número de bytes enviados como respuestas del proxy HTTP/S a los clientes|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|TotalLatency|Sí|Latencia total|MilliSeconds|Average|Tiempo calculado desde que el proxy HTTP/S ha recibido la solicitud de cliente hasta que el cliente ha reconocido el último byte de respuesta del proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|WebApplicationFirewallRequestCount|Sí|Recuento de solicitudes del firewall de aplicaciones web|Count|Total|Número de solicitudes de cliente procesadas por el firewall de aplicaciones web|PolicyName, RuleName, Action|


## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AllocatedSnatPorts|No|Puertos SNAT asignados|Count|Average|Número total de puertos SNAT asignados dentro del período de tiempo|FrontendIPAddress, BackendIPAddress, ProtocolType, IsAwaitingRemoval|
|ByteCount|Sí|Recuento de bytes|Bytes|Total|Número total de bytes transmitidos en un período|FrontendIPAddress, FrontendPort, Direction|
|DipAvailability|Sí|Estado del sondeo de mantenimiento|Count|Average|Estado del sondeo de mantenimiento medio de Load Balancer por duración|ProtocolType, BackendPort, FrontendIPAddress, FrontendPort, BackendIPAddress|
|PacketCount|Sí|Recuento de paquetes|Count|Total|Número total de paquetes transmitidos en un período|FrontendIPAddress, FrontendPort, Direction|
|SnatConnectionCount|Sí|Recuento de conexiones SNAT|Count|Total|Número total de conexiones SNAT nuevas creadas en un período|FrontendIPAddress, BackendIPAddress, ConnectionState|
|SYNCount|Sí|Recuento de SYN|Count|Total|Número total de paquetes SYN transmitidos en un período|FrontendIPAddress, FrontendPort, Direction|
|UsedSnatPorts|No|Puertos SNAT usados|Count|Average|Número total de puertos SNAT usados dentro del período de tiempo|FrontendIPAddress, BackendIPAddress, ProtocolType, IsAwaitingRemoval|
|VipAvailability|Sí|Disponibilidad de la ruta de acceso de datos|Count|Average|Disponibilidad media de la ruta de acceso de datos de Load Balancer por duración|FrontendIPAddress, FrontendPort|


## <a name="microsoftnetworknatgateways"></a>Microsoft.Network/natGateways

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ByteCount|Sí|Bytes|Bytes|Total|Número total de bytes transmitidos en un período|Protocolo, dirección|
|DatapathAvailability|Sí|Disponibilidad de la ruta de acceso (versión preliminar)|Count|Average|Disponibilidad de la ruta de acceso de NAT Gateway|Sin dimensiones|
|PacketCount|Sí|Paquetes|Count|Total|Número total de paquetes transmitidos en un período|Protocolo, dirección|
|PacketDropCount|Sí|Paquetes descartados|Count|Total|Número de paquetes descartados|Sin dimensiones|
|SNATConnectionCount|Sí|Recuento de conexiones SNAT|Count|Total|Total de conexiones activas simultáneas|Protocol, ConnectionState|
|TotalConnectionCount|Sí|Recuento de conexiones SNAT totales|Count|Total|Número total de conexiones SNAT activas|Protocolo|


## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BytesReceivedRate|Sí|Bytes recibidos|Bytes|Total|Número de bytes de la interfaz de red recibidos|Sin dimensiones|
|BytesSentRate|Sí|Bytes enviados|Bytes|Total|Número de bytes de la interfaz de red enviados|Sin dimensiones|
|PacketsReceivedRate|Sí|Paquetes recibidos|Count|Total|Número de paquetes de la interfaz de red recibidos|Sin dimensiones|
|PacketsSentRate|Sí|Paquetes enviados|Count|Total|Número de paquetes de la interfaz de red enviados|Sin dimensiones|


## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AverageRoundtripMs|Sí|Prom. Tiempo de ida y vuelta (ms) (clásico)|MilliSeconds|Average|Promedio de tiempo de ida y vuelta de red (ms) para los sondeos de supervisión de conectividad que se envían entre el origen y el destino|Sin dimensiones|
|ChecksFailedPercent|Sí|Porcentaje de comprobaciones erróneas|Percent|Average|% de comprobaciones de supervisión de conectividad con error|SourceAddress, SourceName, SourceResourceId, SourceType, Protocol, DestinationAddress, DestinationName, DestinationResourceId, DestinationType, DestinationPort, TestGroupName, TestConfigurationName, SourceIP, DestinationIP, SourceSubnet, DestinationSubnet|
|ProbesFailedPercent|Sí|% de sondeos con error (clásico)|Percent|Average|% de sondeos de supervisión de conectividad con error|Sin dimensiones|
|RoundTripTimeMs|Sí|Tiempo de ida y vuelta (ms)|MilliSeconds|Average|Tiempo de ida y vuelta en milisegundos para las comprobaciones de supervisión de conectividad|SourceAddress, SourceName, SourceResourceId, SourceType, Protocol, DestinationAddress, DestinationName, DestinationResourceId, DestinationType, DestinationPort, TestGroupName, TestConfigurationName, SourceIP, DestinationIP, SourceSubnet, DestinationSubnet|
|TestResult|Sí|Resultado de pruebas|Count|Average|Recurso de la prueba de Connection Monitor|SourceAddress, SourceName, SourceResourceId, SourceType, Protocol, DestinationAddress, DestinationName, DestinationResourceId, DestinationType, DestinationPort, TestGroupName, TestConfigurationName, TestResultCriterion, SourceIP, DestinationIP, SourceSubnet, DestinationSubnet|


## <a name="microsoftnetworkp2svpngateways"></a>microsoft.network/p2svpngateways

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|P2SBandwidth|Sí|Ancho de banda P2S de puerta de enlace|BytesPerSecond|Average|Ancho de banda de punto a sitio de una puerta de enlace en bytes por segundo|Instancia|
|P2SConnectionCount|Sí|Recuento de conexiones P2S|Count|Total|Recuento de conexiones de punto a sitio de una puerta de enlace|Protocol, Instance|
|UserVpnRouteCount|No|Recuento de rutas de VPN de usuario|Count|Total|Recuento de rutas de VPN de punto a sitio de usuario aprendidas por puerta de enlace|RouteType, Instance|


## <a name="microsoftnetworkprivatednszones"></a>Microsoft.Network/privateDnsZones

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|QueryVolume|Sí|Volumen de consulta|Count|Total|Número de consultas que se sirven para una zona de DNS privado|Sin dimensiones|
|RecordSetCapacityUtilization|No|Uso de capacidad de conjunto de registros|Percent|Máxima|Porcentaje de capacidad de conjunto de registros que usa una zona de DNS privado|Sin dimensiones|
|RecordSetCount|Sí|Recuento de conjunto de registros|Count|Máxima|Número de conjuntos de registros de una zona de DNS privado|Sin dimensiones|
|VirtualNetworkLinkCapacityUtilization|No|Uso de la capacidad del vínculo de red virtual|Percent|Máxima|Porcentaje de capacidad del vínculo de red virtual que usa una zona de DNS privado|Sin dimensiones|
|VirtualNetworkLinkCount|Sí|Recuento de vínculos de red virtual|Count|Máxima|Número de redes virtuales vinculadas a una zona de DNS privado|Sin dimensiones|
|VirtualNetworkWithRegistrationCapacityUtilization|No|Uso de la capacidad del vínculo de registro de la red virtual|Percent|Máxima|Porcentaje de capacidad del vínculo de red virtual con registro automático que usa una zona de DNS privado|Sin dimensiones|
|VirtualNetworkWithRegistrationLinkCount|Sí|Recuento de vínculo de registro de red virtual|Count|Máxima|Número de redes virtuales vinculadas a una zona de DNS privado con el registro automático habilitado|Sin dimensiones|


## <a name="microsoftnetworkprivateendpoints"></a>Microsoft.Network/privateEndpoints

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|PEBytesIn|Sí|Bytes de entrada|Count|Total|Número total de bytes de salida|Sin dimensiones|
|PEBytesOut|Sí|Bytes de salida|Count|Total|Número total de bytes de salida|Sin dimensiones|


## <a name="microsoftnetworkprivatelinkservices"></a>Microsoft.Network/privateLinkServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|PLSBytesIn|Sí|Bytes de entrada|Count|Total|Número total de bytes de salida|PrivateLinkServiceId|
|PLSBytesOut|Sí|Bytes de salida|Count|Total|Número total de bytes de salida|PrivateLinkServiceId|
|PLSNatPortsUsage|Sí|Uso de puertos NAT|Percent|Average|Uso de puertos NAT|PrivateLinkServiceId, PrivateLinkServiceIPAddress|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ByteCount|Sí|Recuento de bytes|Bytes|Total|Número total de bytes transmitidos en un período|Port, Direction|
|BytesDroppedDDoS|Sí|Bytes de entrada de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada de DDoS descartados|Sin dimensiones|
|BytesForwardedDDoS|Sí|Bytes de entrada de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada de DDoS reenviados|Sin dimensiones|
|BytesInDDoS|Sí|Bytes de entrada de DDoS|BytesPerSecond|Máxima|Bytes de entrada de DDoS|Sin dimensiones|
|DDoSTriggerSYNPackets|Sí|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|Sin dimensiones|
|DDoSTriggerTCPPackets|Sí|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|Sin dimensiones|
|DDoSTriggerUDPPackets|Sí|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|Sin dimensiones|
|IfUnderDDoSAttack|Sí|Bajo ataque de DDoS o no|Count|Máxima|Bajo ataque de DDoS o no|Sin dimensiones|
|PacketCount|Sí|Recuento de paquetes|Count|Total|Número total de paquetes transmitidos en un período|Port, Direction|
|PacketsDroppedDDoS|Sí|Paquetes entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes entrantes de DDoS descartados|Sin dimensiones|
|PacketsForwardedDDoS|Sí|Paquetes entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes entrantes de DDoS reenviados|Sin dimensiones|
|PacketsInDDoS|Sí|Paquetes entrantes de DDoS|CountPerSecond|Máxima|Paquetes entrantes de DDoS|Sin dimensiones|
|SynCount|Sí|Recuento de SYN|Count|Total|Número total de paquetes SYN transmitidos en un período|Port, Direction|
|TCPBytesDroppedDDoS|Sí|Bytes de entrada TCP de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS descartados|Sin dimensiones|
|TCPBytesForwardedDDoS|Sí|Bytes de entrada TCP de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS reenviados|Sin dimensiones|
|TCPBytesInDDoS|Sí|Bytes de entrada TCP de DDoS|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS|Sin dimensiones|
|TCPPacketsDroppedDDoS|Sí|Paquetes TCP entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS descartados|Sin dimensiones|
|TCPPacketsForwardedDDoS|Sí|Paquetes TCP entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS reenviados|Sin dimensiones|
|TCPPacketsInDDoS|Sí|Paquetes TCP entrantes de DDoS|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS|Sin dimensiones|
|UDPBytesDroppedDDoS|Sí|Bytes de entrada UDP de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS descartados|Sin dimensiones|
|UDPBytesDroppedDDoS|Sí|Bytes de entrada UDP de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS reenviados|Sin dimensiones|
|UDPBytesInDDoS|Sí|Bytes de entrada UDP de DDoS|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS|Sin dimensiones|
|UDPPacketsDroppedDDoS|Sí|Paquetes UDP entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS descartados|Sin dimensiones|
|UDPPacketsForwardedDDoS|Sí|Paquetes UDP entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS reenviados|Sin dimensiones|
|UDPPacketsInDDoS|Sí|Paquetes UDP entrantes de DDoS|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS|Sin dimensiones|
|VipAvailability|Sí|Disponibilidad de la ruta de acceso de datos|Count|Average|Disponibilidad media de la dirección IP por duración|Port|


## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Sí|Estado de punto de conexión por punto de conexión|Count|Máxima|1 si el estado de sondeo de un punto de conexión es “Habilitado”, 0 en caso contrario.|EndpointName|
|QpsByEndpoint|Sí|Consultas por punto de conexión devueltas|Count|Total|Número de veces que se devolvió un punto de conexión de Traffic Manager en el período dado|EndpointName|


## <a name="microsoftnetworkvirtualhubs"></a>Microsoft.Network/virtualHubs

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BgpPeerStatus|No|Estado del par de BGP|Count|Máxima|1 - Conectado, 0 - No conectado|routeserviceinstance, bgppeerip, bgppeertype|
|CountOfRoutesAdvertisedToPeer|No|Recuento de rutas anunciadas al par|Count|Máxima|Número total de rutas anunciadas al par|routeserviceinstance, bgppeerip, bgppeertype|
|CountOfRoutesLearnedFromPeer|No|Recuento de rutas aprendidas del par|Count|Máxima|Número total de rutas aprendidas del par|routeserviceinstance, bgppeerip, bgppeertype|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>microsoft.network/virtualnetworkgateways

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AverageBandwidth|Sí|Ancho de banda S2S de puerta de enlace|BytesPerSecond|Average|Ancho de banda de sitio a sitio de una puerta de enlace en bytes por segundo|Instancia|
|BgpPeerStatus|No|Estado del par de BGP|Count|Average|Estado del par de BGP|BgpPeerAddress, Instance|
|BgpRoutesAdvertised|Sí|Rutas de BGP anunciadas|Count|Total|Recuento de rutas de BGP anunciadas a través del túnel|BgpPeerAddress, Instance|
|BgpRoutesLearned|Sí|Rutas de BGP aprendidas|Count|Total|Recuento de rutas de BGP aprendidas a través del túnel|BgpPeerAddress, Instance|
|ExpressRouteGatewayCountOfRoutesAdvertisedToPeer|Sí|Recuento de rutas anunciadas al par (versión preliminar)|Count|Máxima|Recuento de rutas anunciadas al par por ExpressRouteGateway|roleInstance|
|ExpressRouteGatewayCountOfRoutesLearnedFromPeer|Sí|Recuento de rutas aprendidas del par (versión preliminar)|Count|Máxima|Recuento de rutas aprendidas del par por ExpressRouteGateway|roleInstance|
|ExpressRouteGatewayCpuUtilization|Sí|Uso de CPU|Percent|Average|Uso de CPU de la puerta de enlace de ExpressRoute|roleInstance|
|ExpressRouteGatewayFrequencyOfRoutesChanged|No|Frecuencia de cambio de rutas (versión preliminar)|Count|Total|Frecuencia de cambio de rutas en la puerta de enlace de ExpressRoute|roleInstance|
|ExpressRouteGatewayNumberOfVmInVnet|No|Número de máquinas virtuales en la red virtual (versión preliminar)|Count|Máxima|Número de máquinas virtuales en la red virtual|roleInstance|
|ExpressRouteGatewayPacketsPerSecond|No|Paquetes por segundo|CountPerSecond|Average|Recuento de paquetes de puerta de enlace de ExpressRoute|roleInstance|
|MmsaCount|Sí|Recuento de MMSA del túnel|Count|Total|Recuento de MMSA|ConnectionName, RemoteIP, Instance|
|P2SBandwidth|Sí|Ancho de banda P2S de puerta de enlace|BytesPerSecond|Average|Ancho de banda de punto a sitio de una puerta de enlace en bytes por segundo|Instancia|
|P2SConnectionCount|Sí|Recuento de conexiones P2S|Count|Total|Recuento de conexiones de punto a sitio de una puerta de enlace|Protocol, Instance|
|QmsaCount|Sí|Recuento de QMSA del túnel|Count|Total|Recuento de QMSA|ConnectionName, RemoteIP, Instance|
|TunnelAverageBandwidth|Sí|Ancho de banda de túnel|BytesPerSecond|Average|Media de ancho de banda de un túnel en bytes por segundo|ConnectionName, RemoteIP, Instance|
|TunnelEgressBytes|Sí|Bytes de salida de túnel|Bytes|Total|Bytes de salida de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPacketDropCount|Sí|Recuento de eliminación de paquetes de salida del túnel|Count|Total|Recuento de paquetes salientes descartados por túnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPacketDropTSMismatch|Sí|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de salida|Count|Total|Recuento de paquetes de salida con error de coincidencia del selector de tráfico de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPackets|Sí|Paquetes de salida de túnel|Count|Total|Recuento de paquetes salientes de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressBytes|Sí|Bytes de salida de túnel|Bytes|Total|Bytes de entrada de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPacketDropCount|Sí|Recuento de eliminación de paquetes de entrada del túnel|Count|Total|Recuento de paquetes entrantes descartados por túnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPacketDropTSMismatch|Sí|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de entrada|Count|Total|Recuento de paquetes entrantes con error de coincidencia del selector de tráfico de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPackets|Sí|Paquetes de entrada de túnel|Count|Total|Recuento de paquetes entrantes de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelNatAllocations|No|Asignaciones de NAT de túnel|Count|Total|Recuento de asignaciones para una regla NAT en un túnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatedBytes|No|Bytes de túnel con NAT|Bytes|Total|Número de bytes a los que una regla NAT ha aplicado NAT en un túnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatedPackets|No|Paquetes de túnel con NAT|Count|Total|Número de paquetes a los que una regla NAT ha aplicado NAT en un túnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatFlowCount|No|Flujos NAT de túnel|Count|Total|Número de flujos NAT en un túnel por tipo de flujo y regla NAT|NatRule, FlowType, ConnectionName, RemoteIP, Instance|
|TunnelNatPacketDrop|No|Paquetes NAT de túnel descartados|Count|Total|Número de paquetes con NAT en un túnel descartados por tipo de eliminación y regla NAT|NatRule, DropType, ConnectionName, RemoteIP, Instance|
|TunnelPeakPackets|Sí|PPS máximo del túnel|Count|Máxima|Máximo de paquetes por segundo en el túnel|ConnectionName, RemoteIP, Instance|
|TunnelReverseNatedBytes|No|Bytes con NAT inverso de túnel|Bytes|Total|Número de bytes a los que una regla NAT ha aplicado NAT inverso en un túnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelReverseNatedPackets|No|Paquetes con NAT inverso de túnel|Count|Total|Número de paquetes de un túnel a los que una regla NAT ha aplicado NAT inverso|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelTotalFlowCount|Sí|Recuento total de flujos del túnel|Count|Total|Recuento total de flujos en un túnel|ConnectionName, RemoteIP, Instance|
|UserVpnRouteCount|No|Recuento de rutas de VPN de usuario|Count|Total|Recuento de rutas de VPN de punto a sitio de usuario aprendidas por puerta de enlace|RouteType, Instance|
|VnetAddressPrefixCount|Sí|Recuento de prefijos de dirección de la red virtual|Count|Total|Recuento de prefijos de dirección de red virtual tras la puerta de enlace|Instancia|


## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BytesDroppedDDoS|Sí|Bytes de entrada de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada de DDoS descartados|ProtectedIPAddress|
|BytesForwardedDDoS|Sí|Bytes de entrada de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada de DDoS reenviados|ProtectedIPAddress|
|BytesInDDoS|Sí|Bytes de entrada de DDoS|BytesPerSecond|Máxima|Bytes de entrada de DDoS|ProtectedIPAddress|
|DDoSTriggerSYNPackets|Sí|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes SYN para desencadenar la mitigación de DDoS|ProtectedIPAddress|
|DDoSTriggerTCPPackets|Sí|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes TCP para desencadenar la mitigación de DDoS|ProtectedIPAddress|
|DDoSTriggerUDPPackets|Sí|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|CountPerSecond|Máxima|Paquetes entrantes UDP para desencadenar la mitigación de DDoS|ProtectedIPAddress|
|IfUnderDDoSAttack|Sí|Bajo ataque de DDoS o no|Count|Máxima|Bajo ataque de DDoS o no|ProtectedIPAddress|
|PacketsDroppedDDoS|Sí|Paquetes entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes entrantes de DDoS descartados|ProtectedIPAddress|
|PacketsForwardedDDoS|Sí|Paquetes entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes entrantes de DDoS reenviados|ProtectedIPAddress|
|PacketsInDDoS|Sí|Paquetes entrantes de DDoS|CountPerSecond|Máxima|Paquetes entrantes de DDoS|ProtectedIPAddress|
|PingMeshAverageRoundtripMs|Sí|Round trip time for Pings to a VM [Tiempo de recorrido de ida y vuelta de los pings a una VM]|MilliSeconds|Average|Tiempo de ida y vuelta de los pings enviados a una VM de destino|SourceCustomerAddress, DestinationCustomerAddress|
|PingMeshProbesFailedPercent|Sí|Failed Pings to a VM (Pings a una VM erróneos)|Percent|Average|Porcentaje de pings erróneos del total de pings enviados de una VM de destino|SourceCustomerAddress, DestinationCustomerAddress|
|TCPBytesDroppedDDoS|Sí|Bytes de entrada TCP de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS descartados|ProtectedIPAddress|
|TCPBytesForwardedDDoS|Sí|Bytes de entrada TCP de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS reenviados|ProtectedIPAddress|
|TCPBytesInDDoS|Sí|Bytes de entrada TCP de DDoS|BytesPerSecond|Máxima|Bytes de entrada TCP de DDoS|ProtectedIPAddress|
|TCPPacketsDroppedDDoS|Sí|Paquetes TCP entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS descartados|ProtectedIPAddress|
|TCPPacketsForwardedDDoS|Sí|Paquetes TCP entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS reenviados|ProtectedIPAddress|
|TCPPacketsInDDoS|Sí|Paquetes TCP entrantes de DDoS|CountPerSecond|Máxima|Paquetes TCP entrantes de DDoS|ProtectedIPAddress|
|UDPBytesDroppedDDoS|Sí|Bytes de entrada UDP de DDoS descartados|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS descartados|ProtectedIPAddress|
|UDPBytesDroppedDDoS|Sí|Bytes de entrada UDP de DDoS reenviados|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS reenviados|ProtectedIPAddress|
|UDPBytesInDDoS|Sí|Bytes de entrada UDP de DDoS|BytesPerSecond|Máxima|Bytes de entrada UDP de DDoS|ProtectedIPAddress|
|UDPPacketsDroppedDDoS|Sí|Paquetes UDP entrantes de DDoS descartados|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS descartados|ProtectedIPAddress|
|UDPPacketsForwardedDDoS|Sí|Paquetes UDP entrantes de DDoS reenviados|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS reenviados|ProtectedIPAddress|
|UDPPacketsInDDoS|Sí|Paquetes UDP entrantes de DDoS|CountPerSecond|Máxima|Paquetes UDP entrantes de DDoS|ProtectedIPAddress|


## <a name="microsoftnetworkvirtualrouters"></a>Microsoft.Network/virtualRouters

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|PeeringAvailability|Sí|Disponibilidad de BGP|Percent|Average|Disponibilidad de BGP entre VirtualRouter y los elementos remotos del mismo nivel|Del mismo nivel|


## <a name="microsoftnetworkvpngateways"></a>microsoft.network/vpngateways

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AverageBandwidth|Sí|Ancho de banda S2S de puerta de enlace|BytesPerSecond|Average|Ancho de banda de sitio a sitio de una puerta de enlace en bytes por segundo|Instancia|
|BgpPeerStatus|No|Estado del par de BGP|Count|Average|Estado del par de BGP|BgpPeerAddress, Instance|
|BgpRoutesAdvertised|Sí|Rutas de BGP anunciadas|Count|Total|Recuento de rutas de BGP anunciadas a través del túnel|BgpPeerAddress, Instance|
|BgpRoutesLearned|Sí|Rutas de BGP aprendidas|Count|Total|Recuento de rutas de BGP aprendidas a través del túnel|BgpPeerAddress, Instance|
|MmsaCount|Sí|Recuento de MMSA del túnel|Count|Total|Recuento de MMSA|ConnectionName, RemoteIP, Instance|
|QmsaCount|Sí|Recuento de QMSA del túnel|Count|Total|Recuento de QMSA|ConnectionName, RemoteIP, Instance|
|TunnelAverageBandwidth|Sí|Ancho de banda de túnel|BytesPerSecond|Average|Media de ancho de banda de un túnel en bytes por segundo|ConnectionName, RemoteIP, Instance|
|TunnelEgressBytes|Sí|Bytes de salida de túnel|Bytes|Total|Bytes de salida de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPacketDropCount|Sí|Recuento de eliminación de paquetes de salida del túnel|Count|Total|Recuento de paquetes salientes descartados por túnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPacketDropTSMismatch|Sí|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de salida|Count|Total|Recuento de paquetes de salida con error de coincidencia del selector de tráfico de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPackets|Sí|Paquetes de salida de túnel|Count|Total|Recuento de paquetes salientes de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressBytes|Sí|Bytes de salida de túnel|Bytes|Total|Bytes de entrada de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPacketDropCount|Sí|Recuento de eliminación de paquetes de entrada del túnel|Count|Total|Recuento de paquetes entrantes descartados por túnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPacketDropTSMismatch|Sí|Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de entrada|Count|Total|Recuento de paquetes entrantes con error de coincidencia del selector de tráfico de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPackets|Sí|Paquetes de entrada de túnel|Count|Total|Recuento de paquetes entrantes de un túnel|ConnectionName, RemoteIP, Instance|
|TunnelNatAllocations|No|Asignaciones de NAT de túnel|Count|Total|Recuento de asignaciones para una regla NAT en un túnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatedBytes|No|Bytes de túnel con NAT|Bytes|Total|Número de bytes a los que una regla NAT ha aplicado NAT en un túnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatedPackets|No|Paquetes de túnel con NAT|Count|Total|Número de paquetes a los que una regla NAT ha aplicado NAT en un túnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatFlowCount|No|Flujos NAT de túnel|Count|Total|Número de flujos NAT en un túnel por tipo de flujo y regla NAT|NatRule, FlowType, ConnectionName, RemoteIP, Instance|
|TunnelNatPacketDrop|No|Paquetes NAT de túnel descartados|Count|Total|Número de paquetes con NAT en un túnel descartados por tipo de eliminación y regla NAT|NatRule, DropType, ConnectionName, RemoteIP, Instance|
|TunnelPeakPackets|Sí|PPS máximo del túnel|Count|Máxima|Máximo de paquetes por segundo en el túnel|ConnectionName, RemoteIP, Instance|
|TunnelReverseNatedBytes|No|Bytes con NAT inverso de túnel|Bytes|Total|Número de bytes a los que una regla NAT ha aplicado NAT inverso en un túnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelReverseNatedPackets|No|Paquetes con NAT inverso de túnel|Count|Total|Número de paquetes de un túnel a los que una regla NAT ha aplicado NAT inverso|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelTotalFlowCount|Sí|Recuento total de flujos del túnel|Count|Total|Recuento total de flujos en un túnel|ConnectionName, RemoteIP, Instance|
|VnetAddressPrefixCount|Sí|Recuento de prefijos de dirección de la red virtual|Count|Total|Recuento de prefijos de dirección de red virtual tras la puerta de enlace|Instancia|


## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|incoming|Sí|Mensajes entrantes|Count|Total|El recuento de todas las llamadas a la API de envío. |Sin dimensiones|
|incoming.all.failedrequests|Sí|Todas las solicitudes entrantes con error|Count|Total|Solicitudes entrantes totales con error de un centro de notificaciones|Sin dimensiones|
|incoming.all.requests|Sí|Todas las solicitudes entrantes|Count|Total|Solicitudes entrantes totales de un centro de notificaciones|Sin dimensiones|
|incoming.scheduled|Sí|Notificaciones push programadas enviadas|Count|Total|Notificaciones push programadas canceladas|Sin dimensiones|
|incoming.scheduled.cancel|Sí|Notificaciones push programadas canceladas|Count|Total|Notificaciones push programadas canceladas|Sin dimensiones|
|installation.all|Sí|Operaciones de administración de instalaciones|Count|Total|Operaciones de administración de instalaciones|Sin dimensiones|
|installation.delete|Sí|Eliminar operaciones de instalación|Count|Total|Eliminar operaciones de instalación|Sin dimensiones|
|installation.get|Sí|Obtener operaciones de instalación|Count|Total|Obtener operaciones de instalación|Sin dimensiones|
|installation.patch|Sí|Aplicar revisión a operaciones de instalación|Count|Total|Aplicar revisión a operaciones de instalación|Sin dimensiones|
|installation.upsert|Sí|Crear o actualizar operaciones de instalación|Count|Total|Crear o actualizar operaciones de instalación|Sin dimensiones|
|notificationhub.pushes|Sí|Todas las notificaciones salientes|Count|Total|Todas las notificaciones salientes del Centro de notificaciones|Sin dimensiones|
|outgoing.allpns.badorexpiredchannel|Sí|Errores de canal incorrecto o expirado|Count|Total|El recuento de inserciones que provocaron un error porque el canal/token/registrationId del registro ha expirado o no es válido.|Sin dimensiones|
|outgoing.allpns.channelerror|Sí|Errores de canal|Count|Total|El recuento de inserciones que provocaron un error porque el canal no era válido no está asociado a la aplicación correcta limitada o expirada.|Sin dimensiones|
|outgoing.allpns.invalidpayload|Sí|Errores de carga útil|Count|Total|El recuento de las inserciones que provocaron un error porque el PNS devolvió un error de carga útil incorrecta.|Sin dimensiones|
|outgoing.allpns.pnserror|Sí|Errores del sistema de notificación externo|Count|Total|El recuento de inserciones que provocaron un error porque se produjo un problema al comunicarse con el PNS (excluye los problemas de autenticación).|Sin dimensiones|
|outgoing.allpns.success|Sí|Notificaciones correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.apns.badchannel|Sí|Error de canal incorrecto de APNS|Count|Total|El recuento de inserciones que provocaron un error porque el token no es válido (código de estado de APNS: 8).|Sin dimensiones|
|outgoing.apns.expiredchannel|Sí|Error de canal expirado de APNS|Count|Total|El recuento de tokens invalidados por el canal de comentarios de APNS.|Sin dimensiones|
|outgoing.apns.invalidcredentials|Sí|Errores de autorización de APNS|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.apns.invalidnotificationsize|Sí|Error de tamaño de notificación no válido de APNS|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil era demasiado grande (código de estado de APNS: 7).|Sin dimensiones|
|outgoing.apns.pnserror|Sí|Errores de APNS|Count|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con APNS.|Sin dimensiones|
|outgoing.apns.success|Sí|Notificaciones de APNS correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.gcm.authenticationerror|Sí|Errores de autenticación de GCM|Count|Total|El recuento de inserciones que provocaron un error debido a que el PNS no aceptó las credenciales proporcionadas, las credenciales están bloqueadas o el Id. del remitente no está configurado correctamente en la aplicación (resultado de GCM: MismatchedSenderId).|Sin dimensiones|
|outgoing.gcm.badchannel|Sí|Error de canal incorrecto de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro no se ha reconocido (resultado de GCM: registro no válido).|Sin dimensiones|
|outgoing.gcm.expiredchannel|Sí|Error de canal expirado de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro ha caducado (resultado de GCM: NotRegistered).|Sin dimensiones|
|outgoing.gcm.invalidcredentials|Sí|Errores de autorización de GCM (credenciales no válidas)|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.gcm.invalidnotificationformat|Sí|Formato de notificación no válido de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil no tenía el formato correcto (resultado de GCM: InvalidDataKey o InvalidTtl).|Sin dimensiones|
|outgoing.gcm.invalidnotificationsize|Sí|Error de tamaño de notificación no válido de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil era demasiado grande (resultado de GCM: MessageTooBig).|Sin dimensiones|
|outgoing.gcm.pnserror|Sí|Errores de GCM|Count|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con GCM.|Sin dimensiones|
|outgoing.gcm.success|Sí|Notificaciones de GCM correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.gcm.throttled|Sí|Notificaciones limitadas de GCM|Count|Total|El recuento de inserciones que provocaron un error porque GCM limitó esta aplicación (código de estado de GCM: 501-599 o resultado: no disponible).|Sin dimensiones|
|outgoing.gcm.wrongchannel|Sí|Error de canal incorrecto de GCM|Count|Total|El recuento de inserciones que provocaron un error porque la instancia de registrationId del registro no está asociado a la aplicación actual (resultado de GCM: InvalidPackageName).|Sin dimensiones|
|outgoing.mpns.authenticationerror|Sí|Errores de autenticación de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.mpns.badchannel|Sí|Error de canal incorrecto de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque no se reconoció ChannelURI en el registro (estado de MPNS: 404 No encontrado).|Sin dimensiones|
|outgoing.mpns.channeldisconnected|Sí|Canal de MPNS desconectado|Count|Total|El recuento de inserciones que provocaron un error porque se desconectó la instancia ChannelURI en el registro (estado de MPNS: 412 No encontrado).|Sin dimensiones|
|outgoing.mpns.dropped|Sí|Notificaciones descartadas de MPNS|Count|Total|El recuento de inserciones que eliminó MPNS (encabezado de respuesta MPNS: X-NotificationStatus: QueueFull o Suprimido).|Sin dimensiones|
|outgoing.mpns.invalidcredentials|Sí|Credenciales no válidas de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas.|Sin dimensiones|
|outgoing.mpns.invalidnotificationformat|Sí|Formato de notificación no válido de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque la carga útil de la notificación era demasiado grande.|Sin dimensiones|
|outgoing.mpns.pnserror|Sí|Errores de MPNS|Count|Total|El recuento de inserciones que provocaron un error debido a errores de comunicación con MPNS.|Sin dimensiones|
|outgoing.mpns.success|Sí|Notificaciones de MPNS correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.mpns.throttled|Sí|Notificaciones limitadas de MPNS|Count|Total|El recuento de inserciones que provocaron un error porque MPNS limita esta aplicación (MPNS de WNS: 406 No aceptable).|Sin dimensiones|
|outgoing.wns.authenticationerror|Sí|Errores de autenticación de WNS|Count|Total|La notificación no se entregó debido a errores de comunicación con Windows Live, credenciales no válidas o token erróneo.|Sin dimensiones|
|outgoing.wns.badchannel|Sí|Error de canal incorrecto de WNS|Count|Total|El recuento de inserciones que provocaron un error porque no se reconoció ChannelURI en el registro (estado de WNS: 404 No encontrado).|Sin dimensiones|
|outgoing.wns.channeldisconnected|Sí|Canal de WNS desconectado|Count|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (encabezado de respuesta WNS: X-WNS-DeviceConnectionStatus: desconectado).|Sin dimensiones|
|outgoing.wns.channelthrottled|Sí|Canal de WNS limitado|Count|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (encabezado de respuesta WNS: X-WNS-NotificationStatus:channelThrottled).|Sin dimensiones|
|outgoing.wns.dropped|Sí|Notificaciones descartadas de WNS|Count|Total|La notificación se descartó porque la instancia de ChannelURI del registro está limitada (X-WNS-NotificationStatus: descartado pero no X-WNS-DeviceConnectionStatus: desconectado).|Sin dimensiones|
|outgoing.wns.expiredchannel|Sí|Error de canal expirado de WNS|Count|Total|El recuento de inserciones que provocaron un error porque ChannelURI expiró (estado de WNS: 410 Ya no existe).|Sin dimensiones|
|outgoing.wns.invalidcredentials|Sí|Errores de autorización de WNS (credenciales no válidas)|Count|Total|El recuento de inserciones que provocaron un error porque el PNS no aceptó las credenciales proporcionadas o las credenciales están bloqueadas. (Windows Live no reconoce las credenciales).|Sin dimensiones|
|outgoing.wns.invalidnotificationformat|Sí|Formato de notificación no válido de WNS|Count|Total|El formato de la notificación no es válido (estado de WNS: 400). Tenga en cuenta que WNS no rechaza todas las cargas útiles no válidas.|Sin dimensiones|
|outgoing.wns.invalidnotificationsize|Sí|Error de tamaño de notificación no válido de WNS|Count|Total|La carga útil de la notificación es demasiado grande (estado de WNS: 413).|Sin dimensiones|
|outgoing.wns.invalidtoken|Sí|Errores de autorización de WNS (token no válido)|Count|Total|El token que se proporciona a WNS no es válido (estado de WNS: 401 No autorizado).|Sin dimensiones|
|outgoing.wns.pnserror|Sí|Errores de WNS|Count|Total|La notificación no se entregó debido a errores de comunicación con WNS.|Sin dimensiones|
|outgoing.wns.success|Sí|Notificaciones de WNS correctas|Count|Total|El recuento de todas las notificaciones correctas.|Sin dimensiones|
|outgoing.wns.throttled|Sí|Notificaciones limitadas de WNS|Count|Total|El recuento de inserciones que provocaron un error porque WNS limita esta aplicación (estado de WNS: 406 No aceptable).|Sin dimensiones|
|outgoing.wns.tokenproviderunreachable|Sí|Errores de autorización de WNS (inaccesible)|Count|Total|No se puede acceder a Windows Live.|Sin dimensiones|
|outgoing.wns.wrongtoken|Sí|Errores de autorización de WNS (token erróneo)|Count|Total|El token que se proporciona a WNS es válido, pero para otra aplicación (estado de WNS: 403 Prohibido). Esto puede ocurrir si la instancia de ChannelURI en el registro está asociada a otra aplicación. Compruebe que la aplicación cliente está asociada a la misma aplicación cuyas credenciales están en el centro de notificaciones.|Sin dimensiones|
|registration.all|Sí|Operaciones de registro|Count|Total|El recuento de todas las operaciones de registro correctas (creaciones, actualizaciones, consultas y eliminaciones). |Sin dimensiones|
|registration.create|Sí|Operaciones de creación de registros|Count|Total|El recuento de todas las creaciones de registros correctas.|Sin dimensiones|
|registration.delete|Sí|Operaciones de eliminación de registros|Count|Total|El recuento de todas las eliminaciones de registros correctas.|Sin dimensiones|
|registration.get|Sí|Operaciones de lectura de registros|Count|Total|El recuento de todas las consultas de registros correctas.|Sin dimensiones|
|registration.update|Sí|Operaciones de actualización de registros|Count|Total|El recuento de todas las actualizaciones de registros correctas.|Sin dimensiones|
|scheduled.pending|Sí|Notificaciones programadas pendientes|Count|Total|Notificaciones programadas pendientes|Sin dimensiones|


## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Average_% Available Memory|Sí|% de memoria disponible|Count|Average|Average_% Available Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Swap Space|Sí|% de espacio de intercambio disponible|Count|Average|Average_% Available Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Committed Bytes In Use|Sí|% de bytes confirmados en uso|Count|Average|Average_% Committed Bytes In Use|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% DPC Time|Sí|% de tiempo de DPC|Count|Average|Average_% DPC Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Inodes|Sí|% de Inodes libres|Count|Average|Average_% Free Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|Sí|% de espacio libre|Count|Average|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Idle Time|Sí|% de tiempo de inactividad|Count|Average|Average_% Idle Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Interrupt Time|Sí|% de tiempo de interrupción|Count|Average|Average_% Interrupt Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% IO Wait Time|Sí|% de tiempo de espera de E/S|Count|Average|Average_% IO Wait Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Nice Time|Sí|% de tiempo bueno|Count|Average|Average_% Nice Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Privileged Time|Sí|% de tiempo con privilegios|Count|Average|Average_% Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|Sí|% de tiempo de procesador|Count|Average|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Inodes|Sí|% de Inodes usados|Count|Average|Average_% Used Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Memory|Sí|% de memoria usada|Count|Average|Average_% Used Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Space|Sí|% espacio usado|Count|Average|Average_% Used Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Swap Space|Sí|% de espacio de intercambio usado|Count|Average|Average_% Used Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% User Time|Sí|% de tiempo de usuario|Count|Average|Average_% User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes|Sí|MB disponibles|Count|Average|Average_Available MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Memory|Sí|MB de memoria disponibles|Count|Average|Average_Available MBytes Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Swap|Sí|Intercambio de MB disponibles|Count|Average|Average_Available MBytes Swap|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/lecturas|Sí|Prom. Segundos de disco/lecturas|Count|Average|Average_Avg. Segundos de disco/lecturas|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/transferencias|Sí|Prom. Segundos de disco/transferencias|Count|Average|Average_Avg. Segundos de disco/transferencias|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Segundos de disco/escrituras|Sí|Prom. Segundos de disco/escrituras|Count|Average|Average_Avg. Segundos de disco/escrituras|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Received/sec|Sí|Bytes recibidos por segundo|Count|Average|Average_Bytes Received/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Sent/sec|Sí|Bytes enviados por segundo|Count|Average|Average_Bytes Sent/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Total/sec|Sí|Bytes totales por segundo|Count|Average|Average_Bytes Total/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Current Disk Queue Length|Sí|Current Disk Queue Length|Count|Average|Average_Current Disk Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Read Bytes/sec|Sí|Bytes de lectura de disco/s|Count|Average|Average_Disk Read Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Sí|Lecturas de disco/s|Count|Average|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Sí|Transferencias de disco/s|Count|Average|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Write Bytes/sec|Sí|Bytes de escritura en disco/s|Count|Average|Average_Disk Write Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Sí|Escrituras en disco/s|Count|Average|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Sí|Megabytes libres|Count|Average|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Physical Memory|Sí|Memoria física libre|Count|Average|Average_Free Physical Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Space in Paging Files|Sí|Espacio libre en archivos de paginación|Count|Average|Average_Free Space in Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Virtual Memory|Sí|Memoria virtual libre|Count|Average|Average_Free Virtual Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Logical Disk Bytes/sec|Sí|Bytes de disco lógico/s|Count|Average|Average_Logical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Reads/sec|Sí|Lecturas de página/s|Count|Average|Average_Page Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Writes/sec|Sí|Escrituras de página/s|Count|Average|Average_Page Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pages/sec|Sí|Páginas/s|Count|Average|Average_Pages/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct Privileged Time|Sí|Porcentaje de tiempo con privilegios|Count|Average|Average_Pct Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct User Time|Sí|Porcentaje de tiempo de usuario|Count|Average|Average_Pct User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Physical Disk Bytes/sec|Sí|Bytes de disco físico/s|Count|Average|Average_Physical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processes|Sí|Procesos|Count|Average|Average_Processes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processor Queue Length|Sí|Longitud de cola del procesador|Count|Average|Average_Processor Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Size Stored In Paging Files|Sí|Tamaño almacenado en archivos de paginación|Count|Average|Average_Size Stored In Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes|Sí|Número total de bytes|Count|Average|Average_Total Bytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Received|Sí|Número total de bytes recibidos|Count|Average|Average_Total Bytes Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Transmitted|Sí|Número total de bytes transmitidos|Count|Average|Average_Total Bytes Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Collisions|Sí|Colisiones totales|Count|Average|Average_Total Collisions|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Received|Sí|Número total de paquetes recibidos|Count|Average|Average_Total Packets Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Transmitted|Sí|Número total de paquetes transmitidos|Count|Average|Average_Total Packets Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Rx Errors|Sí|Errores de Rx totales|Count|Average|Average_Total Rx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Tx Errors|Sí|Errores de Tx totales|Count|Average|Average_Total Tx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Uptime|Sí|Tiempo de actividad|Count|Average|Average_Uptime|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used MBytes Swap Space|Sí|Espacio de intercambio de MB usado|Count|Average|Average_Used MBytes Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory kBytes|Sí|Kilobytes de memoria usados|Count|Average|Average_Used Memory kBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory MBytes|Sí|MB de memoria usados|Count|Average|Average_Used Memory MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Users|Sí|Usuarios|Count|Average|Average_Users|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Virtual Shared Memory|Sí|Memoria compartida virtual|Count|Average|Average_Virtual Shared Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Evento|Sí|Evento|Count|Average|Evento|Source, EventLog, Computer, EventCategory, EventLevel, EventLevelName, EventID|
|Latido|Sí|Latido|Count|Total|Latido|Computer, OSType, Version, SourceComputerId|
|Actualizar|Sí|Actualizar|Count|Average|Actualizar|Computer, Product, Classification, UpdateState, Optional, Approved|


## <a name="microsoftpeeringpeerings"></a>Microsoft.Peering/peerings

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|EgressTrafficRate|Sí|Velocidad de tráfico de salida|BitsPerSecond|Average|Velocidad de tráfico de salida en bits por segundo|ConnectionId, SessionIp, TrafficClass|
|IngressTrafficRate|Sí|Velocidad de tráfico de entrada|BitsPerSecond|Average|Velocidad de tráfico de entrada en bits por segundo|ConnectionId, SessionIp, TrafficClass|
|SessionAvailability|Sí|Disponibilidad de la sesión|Count|Average|Disponibilidad de la sesión de emparejamiento|ConnectionId, SessionIp|


## <a name="microsoftpeeringpeeringservices"></a>Microsoft.Peering/peeringServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|PrefixLatency|Sí|Latencia de prefijo|Milisegundos|Average|Latencia de prefijo media|PrefixName|
|RoundTripTime|Sí|Tiempo de recorrido de ida y vuelta|Milisegundos|Average|Tiempo medio de recorrido de ida y vuelta|ConnectionMonitorTestName|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|CleanerCurrentPrice|Sí|Memoria: precio actual de limpieza|Count|Average|Precio actual de memoria, USD/byte/tiempo, normalizado a 1000.|Sin dimensiones|
|CleanerMemoryNonshrinkable|Sí|Memoria: memoria de limpieza no reducible|Bytes|Average|Cantidad de memoria, en bytes, que no purgará el limpiador en segundo plano.|Sin dimensiones|
|CleanerMemoryShrinkable|Sí|Memoria: memoria de limpieza reducible|Bytes|Average|Cantidad de memoria, en bytes, que purgará el limpiador en segundo plano.|Sin dimensiones|
|CommandPoolBusyThreads|Sí|Subprocesos: subprocesos ocupados del grupo de comandos|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de comandos.|Sin dimensiones|
|CommandPoolIdleThreads|Sí|Subprocesos: subprocesos inactivos del grupo de comandos|Count|Average|Número de subprocesos inactivos del grupo de subprocesos de comandos.|Sin dimensiones|
|CommandPoolJobQueueLength|Sí|Longitud de cola de trabajos de grupo de comandos|Count|Average|Número de trabajos en la cola del grupo de subprocesos de comandos.|Sin dimensiones|
|cpu_metric|Sí|CPU (Gen2)|Percent|Average|Uso de CPU. Solo se admite para recursos de Power BI Embedded Generation 2.|Sin dimensiones|
|cpu_workload_metric|Sí|CPU por carga de trabajo (Gen2)|Percent|Average|Uso de CPU por carga de trabajo. Solo se admite para recursos de Power BI Embedded Generation 2.|Carga de trabajo|
|CurrentConnections|Sí|Conexión: Conexiones actuales|Count|Average|Número actual de conexiones de cliente establecidas.|Sin dimensiones|
|CurrentUserSessions|Sí|Sesiones de usuario actuales|Count|Average|Número actual de sesiones de usuario establecidas.|Sin dimensiones|
|LongParsingBusyThreads|Sí|Subprocesos: subprocesos ocupados en análisis largos|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de análisis largos.|Sin dimensiones|
|LongParsingIdleThreads|Sí|Subprocesos: subprocesos inactivos en análisis largos|Count|Average|Número de subprocesos inactivos del grupo de subprocesos en análisis largos.|Sin dimensiones|
|LongParsingJobQueueLength|Sí|Subprocesos: longitud de cola de trabajos en análisis largos|Count|Average|Número de trabajos en la cola del grupo de subprocesos en análisis largos.|Sin dimensiones|
|memory_metric|Sí|Memoria (Gen1)|Bytes|Average|Memoria. Intervalo de 0-3 GB para A1, 0-5 GB para A2, 0-10 GB para A3, 0-25 GB para A4, 0-50 GB para A5 y 0-100 GB para A6. Solo se admite para recursos de Power BI Embedded Generation 1.|Sin dimensiones|
|memory_thrashing_metric|Sí|Hiperpaginación de memoria (conjuntos de datos) (Gen1)|Percent|Average|Paginación excesiva media de memoria. Solo se admite para recursos de Power BI Embedded Generation 1.|Sin dimensiones|
|MemoryLimitHard|Sí|Memoria: límite de memoria física|Bytes|Average|Límite de memoria física del archivo de configuración.|Sin dimensiones|
|MemoryLimitHigh|Sí|Memoria: límite alto de memoria|Bytes|Average|Límite alto de memoria del archivo de configuración.|Sin dimensiones|
|MemoryLimitLow|Sí|Memoria: límite bajo de memoria|Bytes|Average|Límite bajo de memoria del archivo de configuración.|Sin dimensiones|
|MemoryLimitVertiPaq|Sí|Memoria: VertiPaq de límite de memoria|Bytes|Average|Límite en memoria del archivo de configuración.|Sin dimensiones|
|MemoryUsage|Sí|Memoria: Uso de la memoria|Bytes|Average|Uso de memoria del proceso de servidor tal como se usa para calcular el precio de la memoria del limpiador. Es igual al contador Proceso\Bytes privados más el tamaño de los datos asignados a la memoria; se ignora la memoria que ha asignado VertiPaq superior al límite de memoria de VertiPaq.|Sin dimensiones|
|overload_metric|Sí|Sobrecarga (Gen2)|Count|Average|Sobrecarga de recursos, 1 si el recurso está sobrecargado; de lo contrario, 0. Solo se admite para recursos de Power BI Embedded Generation 2.|Sin dimensiones|
|ProcessingPoolBusyIOJobThreads|Sí|Subprocesos: subprocesos de trabajo de E/S ocupados del grupo de procesamiento|Count|Average|Número de subprocesos que ejecutan trabajos de E/S en el grupo de subprocesos de procesamiento.|Sin dimensiones|
|ProcessingPoolBusyNonIOThreads|Sí|Subprocesos: subprocesos de trabajo ocupados que no son de E/S del grupo de procesamiento|Count|Average|Número de subprocesos que ejecutan trabajos que no son de E/S del grupo de subprocesos de procesamiento.|Sin dimensiones|
|ProcessingPoolIdleIOJobThreads|Sí|Subprocesos: subprocesos de trabajo de E/S inactivos del grupo de procesamiento|Count|Average|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|Sin dimensiones|
|ProcessingPoolIdleNonIOThreads|Sí|Subprocesos: subprocesos de trabajo inactivos que no son de E/S del grupo de procesamiento|Count|Average|Número de subprocesos inactivos del grupo de subprocesos de procesamiento dedicado a trabajos que no son de E/S.|Sin dimensiones|
|ProcessingPoolIOJobQueueLength|Sí|Subprocesos: Longitud de cola de trabajos de E/S de grupo de procesamiento|Count|Average|Número de trabajos de E/S en la cola del grupo de subprocesos de procesamiento.|Sin dimensiones|
|ProcessingPoolJobQueueLength|Sí|Longitud de cola de trabajos de grupo de procesamiento|Count|Average|Número de trabajos que no son de E/S en la cola del grupo de subprocesos de procesamiento.|Sin dimensiones|
|qpu_high_utilization_metric|Sí|Uso alto de QPU (Gen1)|Count|Total|Uso alto de QPU en el último minuto, 1 para un uso elevado de QPU; de lo contrario, 0. Solo se admite para recursos de Power BI Embedded Generation 1.|Sin dimensiones|
|qpu_metric|Sí|QPU (Gen1)|Count|Average|QPU. El intervalo para A1 es 0 - 20, para A2 es 0 - 40, para A3 es 0 - 40, para A4 es 0 - 80, para A5 es 0 - 160, para A6 es 0 - 320. Solo se admite para recursos de Power BI Embedded Generation 1.|Sin dimensiones|
|QueryDuration|Sí|Duración de consulta (conjuntos de datos) (Gen1)|Milisegundos|Average|Duración de la consulta DAX en el último intervalo. Solo se admite para recursos de Power BI Embedded Generation 1.|Sin dimensiones|
|QueryPoolBusyThreads|Sí|Subprocesos ocupados de grupo de consultas|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de consulta.|Sin dimensiones|
|QueryPoolIdleThreads|Sí|Subprocesos: subprocesos inactivos del grupo de consultas|Count|Average|Número de subprocesos inactivos para trabajos de E/S en el grupo de subprocesos de procesamiento.|Sin dimensiones|
|QueryPoolJobQueueLength|Sí|Longitud de la cola de trabajos del grupo de consultas (conjuntos de datos) (Gen1)|Count|Average|Número de trabajos en la cola del grupo de subprocesos de consultas. Solo se admite para recursos de Power BI Embedded Generation 1.|Sin dimensiones|
|Quota|Sí|Memoria: Quota|Bytes|Average|Cuota de memoria actual, en bytes. La cuota de memoria también se denomina concesión de memoria o reserva de memoria.|Sin dimensiones|
|QuotaBlocked|Sí|Memoria: cuota bloqueada|Count|Average|Número actual de solicitudes de cuota que están bloqueadas hasta que se liberen otras cuotas de memoria.|Sin dimensiones|
|RowsConvertedPerSec|Sí|Procesamiento: filas convertidas por segundo|CountPerSecond|Average|Velocidad de filas convertidas durante el procesamiento.|Sin dimensiones|
|RowsReadPerSec|Sí|Procesamiento: filas leídas por segundo|CountPerSecond|Average|Velocidad de filas leídas de todas las bases de datos relacionales.|Sin dimensiones|
|RowsWrittenPerSec|Sí|Procesamiento: filas escritas por segundo|CountPerSecond|Average|Velocidad de filas escritas durante el procesamiento.|Sin dimensiones|
|ShortParsingBusyThreads|Sí|Subprocesos: subprocesos ocupados en análisis cortos|Count|Average|Número de subprocesos ocupados del grupo de subprocesos de análisis cortos.|Sin dimensiones|
|ShortParsingIdleThreads|Sí|Subprocesos: subprocesos inactivos en análisis cortos|Count|Average|Número de subprocesos inactivos del grupo de subprocesos de análisis cortos.|Sin dimensiones|
|ShortParsingJobQueueLength|Sí|Subprocesos: longitud de cola de trabajos en análisis cortos|Count|Average|Número de trabajos en la cola del grupo de subprocesos en análisis cortos.|Sin dimensiones|
|SuccessfullConnectionsPerSec|Sí|Conexiones correctas por segundo|CountPerSecond|Average|Tasa de finalizaciones de conexión correctas.|Sin dimensiones|
|TotalConnectionFailures|Sí|Número total de errores de conexión|Count|Average|Número total de intentos de conexión con error.|Sin dimensiones|
|TotalConnectionRequests|Sí|Número total de solicitudes de conexión|Count|Average|Número total de solicitudes de conexión. Se trata de llegadas.|Sin dimensiones|
|VertiPaqNonpaged|Sí|Memoria: VertiPaq no paginado|Bytes|Average|Bytes de memoria bloqueados en el espacio de trabajo para que los use el motor en memoria.|Sin dimensiones|
|VertiPaqPaged|Sí|Memoria: VertiPaq paginado|Bytes|Average|Bytes de memoria paginada en uso para datos en memoria.|Sin dimensiones|
|workload_memory_metric|Sí|Memoria por carga de trabajo (Gen1)|Bytes|Average|Memoria por carga de trabajo. Solo se admite para recursos de Power BI Embedded Generation 1.|Carga de trabajo|
|workload_qpu_metric|Sí|QPU por carga de trabajo (Gen1)|Count|Average|QPU por carga de trabajo. El intervalo para A1 es 0 - 20, para A2 es 0 - 40, para A3 es 0 - 40, para A4 es 0 - 80, para A5 es 0 - 160, para A6 es 0 - 320. Solo se admite para recursos de Power BI Embedded Generation 1.|Carga de trabajo|


## <a name="microsoftpurviewaccounts"></a>microsoft.purview/accounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ScanCancelled|Sí|Examen cancelado|Count|Total|Indica el número de exámenes cancelados.|Sin dimensiones|
|ScanCompleted|Sí|Examen completado|Count|Total|Indica el número de exámenes completados correctamente.|Sin dimensiones|
|ScanFailed|Sí|Examen con errores|Count|Total|Indica el número de exámenes con errores.|Sin dimensiones|
|ScanTimeTaken|Sí|Tiempo invertido en el examen|Segundos|Total|Indica el tiempo de examen total en segundos.|Sin dimensiones|


## <a name="microsoftrecoveryservicesvaults"></a>Microsoft.RecoveryServices/Vaults

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BackupHealthEvent|Sí|Backup Health Events (preview) [Eventos de estado de copia de seguridad (versión preliminar)]|Count|Count|Recuento de eventos de estado relacionados con el estado del trabajo de copia de seguridad.|dataSourceURL, backupInstanceUrl, dataSourceType, healthStatus, backupInstanceName|
|RestoreHealthEvent|Sí|Restore Health Events (preview) [Eventos de estado de restauración (versión preliminar)]|Count|Count|Recuento de eventos de estado relacionados con el estado del trabajo de restauración.|dataSourceURL, backupInstanceUrl, dataSourceType, healthStatus, backupInstanceName|


## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActiveConnections|No|ActiveConnections|Count|Total|Valor total de ActiveConnections para Microsoft.Relay.|EntityName|
|ActiveListeners|No|ActiveListeners|Count|Total|Valor total de ActiveListeners para Microsoft.Relay.|EntityName|
|BytesTransferred|Sí|BytesTransferred|Bytes|Total|Valor total de BytesTransferred para Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|No|ListenerConnections-ClientError|Count|Total|ClientError en ListenerConnections para Microsoft.Relay.|EntityName, OperationResult|
|ListenerConnections-ServerError|No|ListenerConnections-ServerError|Count|Total|ServerError en ListenerConnections para Microsoft.Relay.|EntityName, OperationResult|
|ListenerConnections-Success|No|ListenerConnections-Success|Count|Total|Valor correcto de ListenerConnections de Microsoft.Relay.|EntityName, OperationResult|
|ListenerConnections-TotalRequests|No|ListenerConnections-TotalRequests|Count|Total|Valor total de ListenerConnections para Microsoft.Relay.|EntityName|
|ListenerDisconnects|No|ListenerDisconnects|Count|Total|Valor total de ListenerDisconnects para Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|No|SenderConnections-ClientError|Count|Total|ClientError en SenderConnections para Microsoft.Relay.|EntityName, OperationResult|
|SenderConnections-ServerError|No|SenderConnections-ServerError|Count|Total|ServerError en SenderConnections para Microsoft.Relay.|EntityName, OperationResult|
|SenderConnections-Success|No|SenderConnections-Success|Count|Total|Valor correcto de SenderConnections para Microsoft.Relay.|EntityName, OperationResult|
|SenderConnections-TotalRequests|No|SenderConnections-TotalRequests|Count|Total|Solicitudes totales de SenderConnections para Microsoft.Relay.|EntityName|
|SenderDisconnects|No|SenderDisconnects|Count|Total|Valor total de SenderDisconnects para Microsoft.Relay.|EntityName|


## <a name="microsoftresourcessubscriptions"></a>microsoft.resources/subscriptions

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Latencia|No|Latencia|Segundos|Average|Datos de latencia de todas las solicitudes a Azure Resource Manager.|IsCustomerOriginated, método, Namespace, RequestRegion, ResourceType, StatusCode, StatusCodeClass, Microsoft.SubscriptionId|
|Tráfico|No|Tráfico|Count|Count|Datos de tráfico de todas las solicitudes a Azure Resource Manager.|IsCustomerOriginated, método, Namespace, RequestRegion, ResourceType, StatusCode, StatusCodeClass, Microsoft.SubscriptionId|


## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|DocumentsProcessedCount|Sí|Recuento de documentos procesados|Count|Total|Número de documentos procesados|DataSourceName, Failed, IndexerName, IndexName, SkillsetName|
|SearchLatency|Sí|Latencia de búsqueda|Segundos|Average|Promedio de latencia de búsqueda para el servicio de búsqueda|Sin dimensiones|
|SearchQueriesPerSecond|Sí|Consultas de búsqueda por segundo|CountPerSecond|Average|Consultas de búsqueda por segundo para el servicio de búsqueda|Sin dimensiones|
|SkillExecutionCount|Sí|Recuento de invocación de ejecución de aptitudes|Count|Total|Número de ejecuciones de aptitudes|DataSourceName, Failed, IndexerName, SkillName, SkillsetName, SkillType|
|ThrottledSearchQueriesPercentage|Sí|Porcentaje de consultas de búsqueda limitadas|Percent|Average|Porcentaje de consultas de búsqueda limitadas para el servicio de búsqueda|Sin dimensiones|


## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActiveConnections|No|ActiveConnections|Count|Total|Total de conexiones activas para Microsoft.ServiceBus.|Sin dimensiones|
|ActiveMessages|No|Recuento de mensajes activos de una cola/tema.|Count|Average|Recuento de mensajes activos de una cola/tema.|EntityName|
|ConnectionsClosed|No|Conexiones cerradas.|Count|Average|Conexiones cerradas para Microsoft.ServiceBus.|EntityName|
|ConnectionsOpened|No|Conexiones abiertas.|Count|Average|Conexiones abiertas para Microsoft.ServiceBus.|EntityName|
|CPUXNS|No|CPU (Deprecated) [CPU (en desuso)]|Percent|Máxima|Métrica de uso de CPU de espacio de nombres prémium de Service Bus. Esta métrica está en desuso. Use la métrica de CPU (NamespaceCpuUsage) en su lugar.|Réplica|
|DeadletteredMessages|No|Recuento de mensajes fallidos de una cola/tema.|Count|Average|Recuento de mensajes fallidos de una cola/tema.|EntityName|
|IncomingMessages|Sí|Mensajes entrantes|Count|Total|Mensajes entrantes para Microsoft.ServiceBus.|EntityName|
|IncomingRequests|Sí|Solicitudes entrantes|Count|Total|Solicitudes entrantes para Microsoft.ServiceBus.|EntityName|
|error de Hadoop|No|Recuento de mensajes de una cola/tema.|Count|Average|Recuento de mensajes de una cola/tema.|EntityName|
|NamespaceCpuUsage|No|CPU|Percent|Máxima|Métrica de uso de CPU de espacio de nombres prémium de Service Bus.|Réplica|
|NamespaceMemoryUsage|No|Uso de la memoria|Percent|Máxima|Métrica de uso de memoria de espacio de nombres prémium de Service Bus.|Réplica|
|OutgoingMessages|Sí|Mensajes salientes|Count|Total|Mensajes salientes para Microsoft.ServiceBus.|EntityName|
|ScheduledMessages|No|Recuento de mensajes programados de una cola/tema.|Count|Average|Recuento de mensajes programados de una cola/tema.|EntityName|
|ServerErrors|No|Errores del servidor.|Count|Total|Errores del servidor para Microsoft.ServiceBus.|EntityName, OperationResult|
|Size|No|Size|Bytes|Average|Tamaño de cola/tema en bytes.|EntityName|
|SuccessfulRequests|No|Solicitudes correctas|Count|Total|Número total de solicitudes correctas para un espacio de nombres|EntityName, OperationResult|
|ThrottledRequests|No|Solicitudes limitadas.|Count|Total|Solicitudes limitadas para Microsoft.ServiceBus.|EntityName, OperationResult|
|UserErrors|No|Errores de usuario.|Count|Total|Errores de usuario para Microsoft.ServiceBus.|EntityName, OperationResult|
|WSXNS|No|Memory Usage (Deprecated) [Uso de memoria (en desuso)]|Percent|Máxima|Métrica de uso de memoria de espacio de nombres prémium de Service Bus. Esta métrica está en desuso. Utilice la métrica de uso de memoria (NamespaceMemoryUsage) en su lugar.|Réplica|


## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ConnectionCount|Sí|Número de conexiones|Count|Máxima|Cantidad de conexiones de usuario.|Punto de conexión|
|InboundTraffic|Sí|Tráfico entrante|Bytes|Total|Tráfico entrante del servicio|Sin dimensiones|
|MessageCount|Sí|Número de mensajes|Count|Total|Cantidad total de mensajes.|Sin dimensiones|
|OutboundTraffic|Sí|Tráfico saliente|Bytes|Total|Tráfico saliente del servicio|Sin dimensiones|
|SystemErrors|Sí|Errores de sistema|Percent|Máxima|Porcentaje de errores del sistema|Sin dimensiones|
|UserErrors|Sí|Errores de usuario|Percent|Máxima|Porcentaje de errores de usuario|Sin dimensiones|


## <a name="microsoftsignalrservicewebpubsub"></a>Microsoft.SignalRService/WebPubSub

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|InboundTraffic|Sí|Tráfico entrante|Bytes|Total|El tráfico que se origina desde fuera hacia dentro del servicio. Se calcula agregando todos los bytes del tráfico.|Sin dimensiones|
|OutboundTraffic|Sí|Tráfico saliente|Bytes|Total|El tráfico que se origina desde dentro hacia fuera del servicio. Se calcula agregando todos los bytes del tráfico.|Sin dimensiones|
|TotalConnectionCount|Sí|Número de conexiones|Count|Máxima|El número de conexiones de usuario establecidas al servicio. Se calcula agregando todas las conexiones en línea.|Sin dimensiones|


## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|avg_cpu_percent|Sí|Porcentaje de CPU medio|Percent|Average|Porcentaje de CPU medio|Sin dimensiones|
|io_bytes_read|Sí|Bytes de E/S leídos|Bytes|Average|Bytes de E/S leídos|Sin dimensiones|
|io_bytes_written|Sí|Bytes de E/S escritos|Bytes|Average|Bytes de E/S escritos|Sin dimensiones|
|io_requests|Sí|Recuento de solicitudes de E/S|Count|Average|Recuento de solicitudes de E/S|Sin dimensiones|
|reserved_storage_mb|Sí|Espacio de almacenamiento reservado|Count|Average|Espacio de almacenamiento reservado|Sin dimensiones|
|storage_space_used_mb|Sí|Espacio de almacenamiento usado|Count|Average|Espacio de almacenamiento usado|Sin dimensiones|
|virtual_core_count|Sí|Recuento de núcleos virtuales|Count|Average|Recuento de núcleos virtuales|Sin dimensiones|


## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|active_queries|Sí|Consultas activas|Count|Total|Consultas activas en todos los grupos de cargas de trabajo. Solo se aplica a los almacenes de datos.|Sin dimensiones|
|allocated_data_storage|Sí|Espacio de datos asignado|Bytes|Average|Almacenamiento de datos asignado. No es aplicable a los almacenes de datos.|Sin dimensiones|
|app_cpu_billed|Sí|CPU de aplicación facturada|Count|Total|CPU de aplicación facturada. Se aplica a las bases de datos sin servidor.|Sin dimensiones|
|app_cpu_percent|Sí|Porcentaje de CPU de la aplicación|Percent|Average|Porcentaje de CPU de la aplicación. Se aplica a las bases de datos sin servidor.|Sin dimensiones|
|app_memory_percent|Sí|App memory percentage (Porcentaje de memoria de la aplicación)|Percent|Average|Porcentaje de memoria de la aplicación. Se aplica a las bases de datos sin servidor.|Sin dimensiones|
|base_blob_size_bytes|Sí|Tamaño de almacenamiento de blobs de base|Bytes|Máxima|Tamaño de almacenamiento de blobs de base. Se aplica a las bases de datos de hiperescala.|Sin dimensiones|
|blocked_by_firewall|Sí|Bloqueado por el firewall|Count|Total|Bloqueado por el firewall|Sin dimensiones|
|cache_hit_percent|Sí|Porcentaje de aciertos de caché|Percent|Máxima|Porcentaje de aciertos de caché. Solo se aplica a los almacenes de datos.|Sin dimensiones|
|cache_used_percent|Sí|Porcentaje de caché usada|Percent|Máxima|Porcentaje de caché usada. Solo se aplica a los almacenes de datos.|Sin dimensiones|
|connection_failed|Sí|Conexiones con errores|Count|Total|Conexiones con errores|Sin dimensiones|
|connection_successful|Sí|Conexiones correctas|Count|Total|Conexiones correctas|Sin dimensiones|
|cpu_limit|Sí|Límite de CPU|Count|Average|Límite de CPU. Se aplica a las bases de datos basadas en núcleo virtual.|Sin dimensiones|
|cpu_percent|Sí|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Sin dimensiones|
|cpu_used|Sí|CPU utilizada|Count|Average|CPU utilizada. Se aplica a las bases de datos basadas en núcleo virtual.|Sin dimensiones|
|deadlock|Sí|Interbloqueos|Count|Total|Interbloqueos. No es aplicable a los almacenes de datos.|Sin dimensiones|
|delta_num_of_bytes_read|Sí|Lecturas de datos remotos|Bytes|Total|Lecturas de datos remotos en bytes.|Sin dimensiones|
|delta_num_of_bytes_total|Sí|Total remote bytes read and written (Total de bytes remotos leídos y escritos)|Bytes|Total|Total de bytes remotos leídos y escritos por proceso.|Sin dimensiones|
|delta_num_of_bytes_written|Sí|Escrituras remotas de registros.|Bytes|Total|Escrituras de registros remotos en bytes.|Sin dimensiones|
|diff_backup_size_bytes|Sí|Tamaño de almacenamiento de copia de seguridad diferencial|Bytes|Máxima|Tamaño de almacenamiento de copia de seguridad diferencial acumulativo. Se aplica a las bases de datos basadas en núcleo virtual. No aplicable a bases de datos de hiperescala.|Sin dimensiones|
|dtu_consumption_percent|Sí|Porcentaje de DTU|Percent|Average|Porcentaje de DTU. Se aplica a las bases de datos basadas en DTU.|Sin dimensiones|
|dtu_limit|Sí|Límite de DTU|Count|Average|Límite de DTU. Se aplica a las bases de datos basadas en DTU.|Sin dimensiones|
|dtu_used|Sí|DTU utilizada|Count|Average|DTU utilizada. Se aplica a las bases de datos basadas en DTU.|Sin dimensiones|
|dwu_consumption_percent|Sí|Porcentaje de DWU|Percent|Máxima|Porcentaje de DWU. Solo se aplica a los almacenes de datos.|Sin dimensiones|
|dwu_limit|Sí|Límite de DWU|Count|Máxima|Límite de DWU. Solo se aplica a los almacenes de datos.|Sin dimensiones|
|dwu_used|Sí|DWU utilizada|Count|Máxima|DWU utilizada. Solo se aplica a los almacenes de datos.|Sin dimensiones|
|full_backup_size_bytes|Sí|Tamaño de almacenamiento de copia de seguridad completa|Bytes|Máxima|Tamaño de almacenamiento de copia de seguridad completa acumulativa. Se aplica a las bases de datos basadas en núcleo virtual. No aplicable a bases de datos de hiperescala.|Sin dimensiones|
|local_tempdb_usage_percent|Sí|Porcentaje de tempdb local|Percent|Average|Porcentaje de tempdb local. Solo se aplica a los almacenes de datos.|Sin dimensiones|
|log_backup_size_bytes|Sí|Tamaño de almacenamiento de copia de seguridad de registros|Bytes|Máxima|Tamaño de almacenamiento de copia de seguridad de registros acumulativa. Se aplica a las bases de datos de hiperescala y basadas en núcleo virtual.|Sin dimensiones|
|log_write_percent|Sí|Porcentaje de E/S de registro|Percent|Average|Porcentaje de E/S de registro. No es aplicable a los almacenes de datos.|Sin dimensiones|
|memory_usage_percent|Sí|Porcentaje de memoria|Percent|Máxima|Porcentaje de memoria. Solo se aplica a los almacenes de datos.|Sin dimensiones|
|physical_data_read_percent|Sí|Porcentaje de E/S de datos|Percent|Average|Porcentaje de E/S de datos|Sin dimensiones|
|queued_queries|Sí|Consultas en cola|Count|Total|Consultas en cola en todos los grupos de cargas de trabajo. Solo se aplica a los almacenes de datos.|Sin dimensiones|
|sessions_percent|Sí|Porcentaje de sesiones|Percent|Average|Porcentaje de sesiones. No es aplicable a los almacenes de datos.|Sin dimensiones|
|snapshot_backup_size_bytes|Sí|Tamaño de almacenamiento de copia de seguridad de instantánea|Bytes|Máxima|Tamaño de almacenamiento de copia de seguridad de instantánea acumulativa. Se aplica a las bases de datos de hiperescala.|Sin dimensiones|
|sqlserver_process_core_percent|Sí|Porcentaje de núcleos de proceso de SQL Server|Percent|Máxima|Uso de CPU como porcentaje del proceso de SQL DB. No es aplicable a los almacenes de datos.|Sin dimensiones|
|sqlserver_process_memory_percent|Sí|Porcentaje de memoria de proceso de SQL Server|Percent|Máxima|Uso de memoria como porcentaje del proceso de SQL DB. No es aplicable a los almacenes de datos.|Sin dimensiones|
|storage|Sí|Espacio de datos usado|Bytes|Máxima|Espacio de datos usado. No es aplicable a los almacenes de datos.|Sin dimensiones|
|storage_percent|Sí|Porcentaje de espacio de datos usado|Percent|Máxima|Porcentaje de espacio de datos usado. No se aplica a los almacenamientos de datos ni a las bases de datos de hiperescala.|Sin dimensiones|
|tempdb_data_size|Sí|Kilobytes de tamaño de archivo de datos Tempdb|Count|Máxima|Espacio utilizado en los archivos de datos de tempdb en kilobytes. No es aplicable a los almacenes de datos.|Sin dimensiones|
|tempdb_log_size|Sí|Kilobytes de tamaño de archivo de registro Tempdb|Count|Máxima|Espacio utilizado en el archivo de registro de transacciones de tempdb en kilobytes. No es aplicable a los almacenes de datos.|Sin dimensiones|
|tempdb_log_used_percent|Sí|Porcentaje de registro de tempdb usado|Percent|Máxima|Porcentaje de espacio utilizado en el archivo de registro de transacciones de tempdb. No es aplicable a los almacenes de datos.|Sin dimensiones|
|wlg_active_queries|Sí|Consultas activas de grupo de cargas de trabajo|Count|Total|Consultas activas dentro del grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName, IsUserDefined|
|wlg_active_queries_timeouts|Sí|Tiempos de espera de consultas de grupo de cargas de trabajo|Count|Total|Consultas que han agotado el tiempo de espera para el grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName, IsUserDefined|
|wlg_allocation_relative_to_system_percent|Sí|Asignación de grupos de cargas de trabajo por porcentaje del sistema|Percent|Máxima|Porcentaje asignado de recursos en relación con todo el sistema por grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName, IsUserDefined|
|wlg_allocation_relative_to_wlg_effective_cap_percent|Sí|\- Asignación de grupos de cargas de trabajo por porcentaje máximo de recursos|Percent|Máxima|Porcentaje asignado de recursos en relación con los recursos máximos especificados por grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName, IsUserDefined|
|wlg_effective_cap_resource_percent|Sí|Porcentaje máximo de recursos efectivo|Percent|Máxima|Límite máximo en el porcentaje de recursos permitidos para el grupo de cargas de trabajo, teniendo en cuenta el porcentaje de recursos mínimo efectivo asignado a otros grupos de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName, IsUserDefined|
|wlg_effective_min_resource_percent|Sí|Porcentaje mínimo de recursos efectivo|Percent|Máxima|Porcentaje mínimo de recursos reservados y aislados para el grupo de cargas de trabajo teniendo en cuenta el nivel de servicio mínimo. Solo se aplica a los almacenes de datos.|WorkloadGroupName, IsUserDefined|
|wlg_queued_queries|Sí|Consultas en cola del grupo de cargas de trabajo|Count|Total|Consultas en cola dentro del grupo de cargas de trabajo. Solo se aplica a los almacenes de datos.|WorkloadGroupName, IsUserDefined|
|workers_percent|Sí|Porcentaje de trabajos|Percent|Average|Porcentaje de trabajos. No es aplicable a los almacenes de datos.|Sin dimensiones|
|xtp_storage_percent|Sí|Porcentaje de almacenamiento de OLTP en memoria|Percent|Average|Porcentaje de almacenamiento de OLTP en memoria. No es aplicable a los almacenes de datos.|Sin dimensiones|


## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|allocated_data_storage|Sí|Espacio de datos asignado|Bytes|Average|Espacio de datos asignado|Sin dimensiones|
|allocated_data_storage_percent|Sí|Porcentaje asignado de espacio de datos|Percent|Máxima|Porcentaje asignado de espacio de datos|Sin dimensiones|
|cpu_limit|Sí|Límite de CPU|Count|Average|Límite de CPU. Se aplica a los grupos elásticos basados en núcleo virtual.|Sin dimensiones|
|cpu_percent|Sí|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|Sin dimensiones|
|cpu_used|Sí|CPU utilizada|Count|Average|CPU utilizada. Se aplica a los grupos elásticos basados en núcleo virtual.|Sin dimensiones|
|database_allocated_data_storage|No|Espacio de datos asignado|Bytes|Average|Espacio de datos asignado|DatabaseResourceId|
|database_cpu_limit|No|Límite de CPU|Count|Average|Límite de CPU|DatabaseResourceId|
|database_cpu_percent|No|Porcentaje de CPU|Percent|Average|Porcentaje de CPU|DatabaseResourceId|
|database_cpu_used|No|CPU utilizada|Count|Average|CPU utilizada|DatabaseResourceId|
|database_dtu_consumption_percent|No|Porcentaje de DTU|Percent|Average|Porcentaje de DTU|DatabaseResourceId|
|database_eDTU_used|No|eDTU utilizada|Count|Average|eDTU utilizada|DatabaseResourceId|
|database_log_write_percent|No|Porcentaje de E/S de registro|Percent|Average|Porcentaje de E/S de registro|DatabaseResourceId|
|database_physical_data_read_percent|No|Porcentaje de E/S de datos|Percent|Average|Porcentaje de E/S de datos|DatabaseResourceId|
|database_sessions_percent|No|Porcentaje de sesiones|Percent|Average|Porcentaje de sesiones|DatabaseResourceId|
|database_storage_used|No|Espacio de datos usado|Bytes|Average|Espacio de datos usado|DatabaseResourceId|
|database_workers_percent|No|Porcentaje de trabajos|Percent|Average|Porcentaje de trabajos|DatabaseResourceId|
|dtu_consumption_percent|Sí|Porcentaje de DTU|Percent|Average|Porcentaje de DTU. Se aplica a los grupos elásticos basados en DTU.|Sin dimensiones|
|eDTU_limit|Sí|Límite de eDTU|Count|Average|Límite de eDTU. Se aplica a los grupos elásticos basados en DTU.|Sin dimensiones|
|eDTU_used|Sí|eDTU utilizada|Count|Average|eDTU utilizada. Se aplica a los grupos elásticos basados en DTU.|Sin dimensiones|
|log_write_percent|Sí|Porcentaje de E/S de registro|Percent|Average|Porcentaje de E/S de registro|Sin dimensiones|
|physical_data_read_percent|Sí|Porcentaje de E/S de datos|Percent|Average|Porcentaje de E/S de datos|Sin dimensiones|
|sessions_percent|Sí|Porcentaje de sesiones|Percent|Average|Porcentaje de sesiones|Sin dimensiones|
|sqlserver_process_core_percent|Sí|Porcentaje de núcleos de proceso de SQL Server|Percent|Máxima|Uso de CPU como porcentaje del proceso de SQL DB. Se aplica a los grupos elásticos.|Sin dimensiones|
|sqlserver_process_memory_percent|Sí|Porcentaje de memoria de proceso de SQL Server|Percent|Máxima|Uso de memoria como porcentaje del proceso de SQL DB. Se aplica a los grupos elásticos.|Sin dimensiones|
|storage_limit|Sí|Tamaño máximo de datos|Bytes|Average|Tamaño máximo de datos|Sin dimensiones|
|storage_percent|Sí|Porcentaje de espacio de datos usado|Percent|Average|Porcentaje de espacio de datos usado|Sin dimensiones|
|storage_used|Sí|Espacio de datos usado|Bytes|Average|Espacio de datos usado|Sin dimensiones|
|tempdb_data_size|Sí|Kilobytes de tamaño de archivo de datos Tempdb|Count|Máxima|Espacio utilizado en los archivos de datos de tempdb en kilobytes.|Sin dimensiones|
|tempdb_log_size|Sí|Kilobytes de tamaño de archivo de registro Tempdb|Count|Máxima|Espacio utilizado en el archivo de registro de transacciones de tempdb en kilobytes.|Sin dimensiones|
|tempdb_log_used_percent|Sí|Porcentaje de registro de tempdb usado|Percent|Máxima|Porcentaje de espacio utilizado en el archivo de registro de transacciones de tempdb|Sin dimensiones|
|workers_percent|Sí|Porcentaje de trabajos|Percent|Average|Porcentaje de trabajos|Sin dimensiones|
|xtp_storage_percent|Sí|Porcentaje de almacenamiento de OLTP en memoria|Percent|Average|Porcentaje de almacenamiento de OLTP en memoria|Sin dimensiones|


## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida. Este número incluye la salida a un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|MilliSeconds|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Sí|Latencia del servidor correcta|MilliSeconds|Average|El tiempo medio que se usa para que Azure Storage procese una solicitud correcta . Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|
|UsedCapacity|Sí|Capacidad usada|Bytes|Average|La cantidad de almacenamiento que utiliza la cuenta de almacenamiento. En las cuentas de almacenamiento estándar, es la suma de la capacidad usada por un blob, una tabla, un archivo y una cola. Tanto en las cuentas de almacenamiento Premium como en las cuentas de Blob Storage, coincide con BlobCapacity o FileCapacity.|Sin dimensiones|


## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|
|BlobCapacity|No|Capacidad de Blob|Bytes|Average|Cantidad de almacenamiento que ha usado la instancia de Blob service de la cuenta de almacenamiento, en bytes.|BlobType, Tier|
|BlobCount|No|Recuento de blobs|Count|Average|El número de objetos BLOB almacenados en la cuenta de almacenamiento.|BlobType, Tier|
|BlobProvisionedSize|No|Tamaño de aprovisionamiento de blob|Bytes|Average|La cantidad de almacenamiento aprovisionado en Blob service en la cuenta de almacenamiento, en bytes.|BlobType, Tier|
|ContainerCount|Sí|Recuento de contenedores de blobs|Count|Average|El número de contenedores que hay en la cuenta de almacenamiento.|Sin dimensiones|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida. Este número incluye la salida a un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|IndexCapacity|No|Capacidad de índice|Bytes|Average|Capacidad de almacenamiento que usa el índice jerárquico de Azure Data Lake Storage Gen2.|Sin dimensiones|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|Milisegundos|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Sí|Latencia del servidor correcta|Milisegundos|Average|El tiempo medio que se usa para que Azure Storage procese una solicitud correcta . Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication, FileShare|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida. Este número incluye la salida a un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication, FileShare|
|FileCapacity|No|Capacidad de File|Bytes|Average|La cantidad de almacenamiento de archivos que utiliza la cuenta de almacenamiento.|FileShare|
|FileCount|No|Recuento de archivos|Count|Average|El número de archivos que hay en la cuenta de almacenamiento.|FileShare|
|FileShareCapacityQuota|No|Cuota de capacidad de recurso compartido de archivos|Bytes|Average|Límite superior de la cantidad de almacenamiento que puede usar el servicio Azure Files en bytes.|FileShare|
|FileShareCount|No|Recuento de recursos compartidos de archivos|Count|Average|El número de recursos compartidos de archivo que hay en la cuenta de almacenamiento.|Sin dimensiones|
|FileShareProvisionedIOPS|No|IOPS aprovisionado del recurso compartido de archivos|CountPerSecond|Average|Número de línea base de IOPS aprovisionadas para el recurso compartido de archivos prémium en la cuenta de almacenamiento de archivos prémium. Este número se calcula en función del tamaño aprovisionado (cuota) de la capacidad del recurso compartido.|FileShare|
|FileShareSnapshotCount|No|File Share Snapshot Count (Recuento de instantáneas del recurso compartido de archivos)|Count|Average|Número de instantáneas presentes en el recurso compartido en el servicio Files de la cuenta de almacenamiento.|FileShare|
|FileShareSnapshotSize|No|File Share Snapshot Size (Tamaño de instantánea del recurso compartido de archivos)|Bytes|Average|Cantidad de almacenamiento que usan las instantáneas del servicio Files de la cuenta de almacenamiento en bytes.|FileShare|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication, FileShare|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|Milisegundos|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication, FileShare|
|SuccessServerLatency|Sí|Latencia del servidor correcta|Milisegundos|Average|El tiempo medio que se usa para que Azure Storage procese una solicitud correcta . Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication, FileShare|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication, FileShare|


## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida. Este número incluye la salida a un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|QueueCapacity|Sí|Capacidad de Queue|Bytes|Average|La cantidad de almacenamiento de colas que utiliza la cuenta de almacenamiento.|Sin dimensiones|
|QueueCount|Sí|Recuento de colas|Count|Average|El número de colas que hay en la cuenta de almacenamiento.|Sin dimensiones|
|QueueMessageCount|Sí|Recuento de mensajes de Queue|Count|Average|El número de mensajes de la colas no expirados que hay en la cuenta de almacenamiento.|Sin dimensiones|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|Milisegundos|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Sí|Latencia del servidor correcta|Milisegundos|Average|El tiempo medio que se usa para que Azure Storage procese una solicitud correcta . Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilidad|Sí|Disponibilidad|Percent|Average|El porcentaje de disponibilidad para el servicio de almacenamiento o la operación de API especificada. Para calcular la disponibilidad hay que tomar el valor deTotalBillableRequests y dividirlo por el número de solicitudes aplicables, incluidas aquellas que generaron errores inesperados. Todos los errores inesperados reducen la disponibilidad del servicio de almacenamiento o de la operación de API especificada.|GeoType, ApiName, Authentication|
|Salida|Sí|Salida|Bytes|Total|La cantidad de datos de salida. Este número incluye la salida a un cliente externo en Azure Storage, así como la salida dentro de Azure. En consecuencia, este número no refleja las salidas facturables.|GeoType, ApiName, Authentication|
|Entrada|Sí|Entrada|Bytes|Total|La cantidad de datos de entrada, en bytes. Este número incluye la entradas desde un cliente externo en Azure Storage, así como la entrada dentro de Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Sí|Latencia de E2E correcta|Milisegundos|Average|La latencia media de un extremo a otro de las solicitudes correctas realizadas a un servicio de almacenamiento o a la operación de API especificada, en milisegundos. Este valor incluye el tiempo de procesamiento requerido dentro de Azure Storage para leer la solicitud, enviar la respuesta y recibir la confirmación de la respuesta.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Sí|Latencia del servidor correcta|Milisegundos|Average|El tiempo medio que se usa para que Azure Storage procese una solicitud correcta . Este valor no incluye la latencia de red especificada en SuccessE2ELatency.|GeoType, ApiName, Authentication|
|TableCapacity|Sí|Capacidad de Table|Bytes|Average|La cantidad de almacenamiento de tablas que utiliza la cuenta de almacenamiento.|Sin dimensiones|
|TableCount|Sí|Recuento de tablas|Count|Average|El número de tablas de la cuenta de almacenamiento.|Sin dimensiones|
|TableEntityCount|Sí|Recuento de entidades de Table|Count|Average|El número de entidades de tabla que hay en la cuenta de almacenamiento.|Sin dimensiones|
|Transacciones|Sí|Transacciones|Count|Total|El número de solicitudes realizadas a un servicio de almacenamiento o la operación de API especificada. Este número incluye solicitudes correctas y con errores, así como las solicitudes que generaron errores. Use la dimensión ResponseType para el número de diferentes tipos de respuesta.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftstoragecachecaches"></a>Microsoft.StorageCache/caches

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ClientIOPS|Sí|Total Client IOPS (Total de IOPS de cliente)|Count|Average|Tasa de operaciones de archivos de cliente que procesa la caché.|Sin dimensiones|
|ClientLatency|Sí|Average Client Latency (Latencia media de cliente)|MilliSeconds|Average|Latencia media de las operaciones de archivos de cliente en la caché.|Sin dimensiones|
|ClientLockIOPS|Sí|Client Lock IOPS (IOPS de bloqueo de cliente)|CountPerSecond|Average|Operaciones de bloqueo de archivos de cliente por segundo.|Sin dimensiones|
|ClientMetadataReadIOPS|Sí|Client Metadata Read IOPS (IOPS de lectura de metadatos de cliente)|CountPerSecond|Average|Tasa de operaciones de archivos de cliente enviadas a la memoria caché, sin incluir las lecturas de datos, que no modifican el estado persistente.|Sin dimensiones|
|ClientMetadataWriteIOPS|Sí|Client Metadata Write IOPS (IOPS de escritura de metadatos de cliente)|CountPerSecond|Average|Tasa de operaciones de archivos de cliente enviadas a la caché, sin incluir las escrituras de datos, que modifican el estado persistente.|Sin dimensiones|
|ClientReadIOPS|Sí|Client Read IOPS (IOPS de lectura de cliente)|CountPerSecond|Average|Operaciones de lectura de cliente por segundo.|Sin dimensiones|
|ClientReadThroughput|Sí|Average Cache Read Throughput (Rendimiento medio de lectura de caché)|BytesPerSecond|Average|Velocidad de transferencia de los datos de lectura del cliente.|Sin dimensiones|
|ClientStatus|Sí|Estado de cliente|Count|Total|Información sobre la conexión del cliente.|ClientSource, CacheAddress, ClientAddress, Protocol, ConnectionType|
|ClientWriteIOPS|Sí|Client Write IOPS (IOPS de escritura de cliente)|CountPerSecond|Average|Operaciones de escritura de cliente por segundo.|Sin dimensiones|
|ClientWriteThroughput|Sí|Average Cache Write Throughput (Rendimiento medio de escritura de caché)|BytesPerSecond|Average|Velocidad de transferencia de los datos de escritura del cliente.|Sin dimensiones|
|FileOps|Sí|Operaciones de archivos|CountPerSecond|Average|Número de operaciones de archivo por segundo.|SourceFile, Rank, FileType|
|FileReads|Sí|Lecturas de archivos|BytesPerSecond|Average|Número de bytes por segundo leídos de un archivo.|SourceFile, Rank, FileType|
|FileUpdates|Sí|Actualizaciones de archivos|CountPerSecond|Average|Número de actualizaciones de directorios y operaciones de metadatos por segundo.|SourceFile, Rank, FileType|
|FileWrites|Sí|Escrituras de archivos|BytesPerSecond|Average|Número de bytes por segundo escritos en un archivo.|SourceFile, Rank, FileType|
|StorageTargetAsyncWriteThroughput|Sí|StorageTarget Asynchronous Write Throughput (Rendimiento de escritura asincrónica de StorageTarget)|BytesPerSecond|Average|Velocidad con la que la caché escribe datos de forma asincrónica en un elemento StorageTarget determinado. Se trata de escrituras oportunistas que no hacen que los clientes se bloqueen.|StorageTarget|
|StorageTargetBlocksRecycled|Sí|Bloques de destino de almacenamiento reciclados|Count|Average|Número total de bloques de caché de 16k reciclados (liberados) por destino de almacenamiento.|StorageTarget|
|StorageTargetFillThroughput|Sí|StorageTarget Fill Throughput (Rendimiento de relleno de StorageTarget)|BytesPerSecond|Average|Velocidad con la que la caché lee datos de StorageTarget para controlar un error de caché.|StorageTarget|
|StorageTargetFreeReadSpace|Sí|Espacio de lectura libre en destino de almacenamiento|Bytes|Average|Espacio de lectura disponible para almacenar en caché los archivos asociados a un destino de almacenamiento.|StorageTarget|
|StorageTargetFreeWriteSpace|Sí|Espacio de escritura libre en destino de almacenamiento|Bytes|Average|Espacio de escritura disponible para los datos sucios asociados a un destino de almacenamiento.|StorageTarget|
|StorageTargetHealth|Sí|Storage Target Health (Estado del destino de almacenamiento)|Count|Average|Resultados booleanos de la prueba de conectividad entre la caché y los destinos de almacenamiento.|Sin dimensiones|
|StorageTargetIOPS|Sí|Total StorageTarget IOPS (IOPS total de StorageTarget)|Count|Average|Tasa de todas las operaciones de archivos que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetLatency|Sí|StorageTarget Latency (Latencia de StorageTarget)|MilliSeconds|Average|Latencia de ida y vuelta media de todas las operaciones de archivos que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetMetadataReadIOPS|Sí|StorageTarget Metadata Read IOPS (IOPS de lectura de metadatos de StorageTarget)|CountPerSecond|Average|Tasa de operaciones de archivos que no modifican el estado persistente y excluyen la operación de lectura, y que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetMetadataWriteIOPS|Sí|StorageTarget Metadata Write IOPS (IOPS de escritura de metadatos de StorageTarget)|CountPerSecond|Average|Tasa de operaciones de archivos que modifican el estado persistente y excluyen la operación de escritura, y que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetReadAheadThroughput|Sí|StorageTarget Read Ahead Throughput (Rendimiento de lectura anticipada de StorageTarget)|BytesPerSecond|Average|Velocidad con la que la caché lee datos de forma oportunista desde el elemento StorageTarget.|StorageTarget|
|StorageTargetReadIOPS|Sí|StorageTarget Read IOPS (IOPS de lectura de StorageTarget)|CountPerSecond|Average|Tasa de operaciones de lectura de archivos que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetRecycleRate|Sí|Velocidad de reciclaje de destino de almacenamiento|BytesPerSecond|Average|Velocidad de reciclaje del espacio en caché asociada a un destino de almacenamiento en HPC Cache. Esta es la velocidad a la que se borran los datos existentes de la caché para dar espacio a los nuevos datos.|StorageTarget|
|StorageTargetSyncWriteThroughput|Sí|StorageTarget Synchronous Write Throughput (Rendimiento de escritura sincrónica de StorageTarget)|BytesPerSecond|Average|Velocidad con la que la caché escribe datos de forma sincrónica en un elemento StorageTarget determinado. Se trata de escrituras que hacen que los clientes se bloqueen.|StorageTarget|
|StorageTargetTotalReadThroughput|Sí|StorageTarget Total Read Throughput (Rendimiento de lectura total de StorageTarget)|BytesPerSecond|Average|Velocidad total con la que la caché lee datos de un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetTotalWriteThroughput|Sí|StorageTarget Total Write Throughput (Rendimiento de escritura total de StorageTarget)|BytesPerSecond|Average|Velocidad total con la que la caché escribe datos en un elemento StorageTarget determinado.|StorageTarget|
|StorageTargetUsedReadSpace|Sí|Espacio de lectura usado en destino de almacenamiento|Bytes|Average|Espacio de lectura utilizado por los archivos almacenados en caché asociados a un destino de almacenamiento.|StorageTarget|
|StorageTargetUsedWriteSpace|Sí|Espacio de escritura usado en destino de almacenamiento|Bytes|Average|Espacio de escritura usado por los datos sucios asociados a un destino de almacenamiento.|StorageTarget|
|StorageTargetWriteIOPS|Sí|StorageTarget Write IOPS (IOPS de escritura de StorageTarget)|Count|Average|Tasa de operaciones de escritura de archivos que la caché envía a un elemento StorageTarget determinado.|StorageTarget|
|TotalBlocksRecycled|Sí|Total de bloques reciclados|Count|Average|Número total de bloques de caché de 16k reciclados (liberados) para HPC Cache.|Sin dimensiones|
|TotalFreeReadSpace|Sí|Espacio de lectura libre|Bytes|Average|Espacio total disponible para almacenar en caché los archivos de lectura.|Sin dimensiones|
|TotalFreeWriteSpace|Sí|Espacio de lectura y escritura libre|Bytes|Average|Espacio total de escritura disponible para almacenar los datos modificados en la caché.|Sin dimensiones|
|TotalRecycleRate|Sí|Velocidad de reciclaje|BytesPerSecond|Average|Velocidad total de reciclaje del espacio en caché en HPC Cache. Esta es la velocidad a la que se borran los datos existentes de la caché para dar espacio a los nuevos datos.|Sin dimensiones|
|TotalUsedReadSpace|Sí|Espacio de lectura usado|Bytes|Average|Espacio total de lectura utilizado por los datos sucios para HPC Cache.|Sin dimensiones|
|TotalUsedWriteSpace|Sí|Espacio de escritura usado|Bytes|Average|Espacio total de escritura utilizado por los datos sucios para HPC Cache.|Sin dimensiones|
|Tiempo de actividad|Sí|Tiempo de actividad|Count|Average|Resultados booleanos de la prueba de conectividad entre la caché y el sistema de supervisión.|Sin dimensiones|


## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ServerSyncSessionResult|Sí|Resultado de la sesión de sincronización|Count|Average|Métrica que registra un valor de 1 cada vez que el punto de conexión del servidor completa correctamente una sesión de sincronización con el punto de conexión en la nube|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Sí|Bytes sincronizados|Bytes|Total|Tamaño total del archivo transferido para las sesiones de sincronización|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncComputedCacheHitRate|Sí|Tasa de aciertos de caché de nube por niveles|Percent|Average|Porcentaje de bytes que se atendieron desde la caché|SyncGroupName, ServerName, ServerEndpointName|
|StorageSyncRecallComputedSuccessRate|Sí|Tasa de éxito de recuperación de nube por niveles|Percent|Average|Porcentaje de todas las recuperaciones correctas|SyncGroupName, ServerName, ServerEndpointName|
|StorageSyncRecalledNetworkBytesByApplication|Sí|Tamaño de recuperación de nube por niveles por aplicación|Bytes|Total|Tamaño de los datos recuperados por aplicación|SyncGroupName, ServerName, ApplicationName|
|StorageSyncRecalledTotalNetworkBytes|Sí|Tamaño de recuperación de nube por niveles|Bytes|Total|El tamaño de los datos que se recuperan|SyncGroupName, ServerName, ServerEndpointName|
|StorageSyncRecallThroughputBytesPerSecond|Sí|Rendimiento de recuperación de nube por niveles|BytesPerSecond|Average|Rendimiento del tamaño de recuperación de datos|SyncGroupName, ServerName, ServerEndpointName|
|StorageSyncServerHeartbeat|Sí|Estado en línea del servidor|Count|Máxima|Métrica que registra un valor de 1 cada vez que el servidor registrado registra correctamente un latido con el punto de conexión en la nube.|nombreDeServidor|
|StorageSyncSyncSessionAppliedFilesCount|Sí|Archivos sincronizados|Count|Total|Número de archivos sincronizados|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Sí|Archivos que no se están sincronizando|Count|Average|No se pudo sincronizar el recuento de archivos|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncTieringCacheSizeBytes|Sí|Tamaño de caché del servidor|Bytes|Average|Tamaño de los datos almacenados en caché en el servidor|SyncGroupName, ServerName, ServerEndpointName|


## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AMLCalloutFailedRequests|Sí|Solicitudes de función con errores|Count|Total|Solicitudes de función con errores|LogicalName, PartitionId, ProcessorInstance|
|AMLCalloutInputEvents|Sí|Eventos de función|Count|Total|Eventos de función|LogicalName, PartitionId, ProcessorInstance|
|AMLCalloutRequests|Sí|Solicitudes de función|Count|Total|Solicitudes de función|LogicalName, PartitionId, ProcessorInstance|
|ConversionErrors|Sí|Errores de conversión de datos|Count|Total|Errores de conversión de datos|LogicalName, PartitionId, ProcessorInstance|
|DeserializationError|Sí|Errores de deserialización de entrada|Count|Total|Errores de deserialización de entrada|LogicalName, PartitionId, ProcessorInstance|
|DroppedOrAdjustedEvents|Sí|Eventos de fuera de orden|Count|Total|Eventos de fuera de orden|LogicalName, PartitionId, ProcessorInstance|
|EarlyInputEvents|Sí|Primeros eventos de entrada|Count|Total|Primeros eventos de entrada|LogicalName, PartitionId, ProcessorInstance|
|Errors|Sí|Errores de tiempo de ejecución|Count|Total|Errores de tiempo de ejecución|LogicalName, PartitionId, ProcessorInstance|
|InputEventBytes|Sí|Bytes del evento de entrada|Bytes|Total|Bytes del evento de entrada|LogicalName, PartitionId, ProcessorInstance|
|InputEvents|Sí|Eventos de entrada|Count|Total|Eventos de entrada|LogicalName, PartitionId, ProcessorInstance|
|InputEventsSourcesBacklogged|Sí|Eventos de entrada pendientes|Count|Máxima|Eventos de entrada pendientes|LogicalName, PartitionId, ProcessorInstance|
|InputEventsSourcesPerSecond|Sí|Orígenes de entrada recibidos|Count|Total|Orígenes de entrada recibidos|LogicalName, PartitionId, ProcessorInstance|
|LateInputEvents|Sí|Eventos de entrada retrasada|Count|Total|Eventos de entrada retrasada|LogicalName, PartitionId, ProcessorInstance|
|OutputEvents|Sí|Eventos de salida|Count|Total|Eventos de salida|LogicalName, PartitionId, ProcessorInstance|
|OutputWatermarkDelaySeconds|Sí|Retraso de la marca de agua|Segundos|Máxima|Retraso de la marca de agua|LogicalName, PartitionId, ProcessorInstance|
|ProcessCPUUsagePercentage|Sí|Porcentaje de uso de CPU (versión preliminar)|Percent|Máxima|Porcentaje de uso de CPU (versión preliminar)|LogicalName, PartitionId, ProcessorInstance|
|ResourceUtilization|Sí|SU % uso|Percent|Máxima|SU % uso|LogicalName, PartitionId, ProcessorInstance|


## <a name="microsoftsynapseworkspaces"></a>Microsoft.Synapse/workspaces

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BuiltinSqlPoolDataProcessedBytes|No|Datos procesados (bytes)|Bytes|Total|Cantidad de datos procesados por consultas|Sin dimensiones|
|BuiltinSqlPoolLoginAttempts|No|Intentos de inicio de sesión|Count|Total|Recuento de intentos de inicio de sesión correctos o con error|Resultado|
|BuiltinSqlPoolRequestsEnded|No|Solicitudes finalizadas|Count|Total|Recuento de solicitudes correctas, con error o canceladas|Resultado|
|IntegrationActivityRunsEnded|No|Ejecuciones de actividad finalizadas|Count|Total|Recuento de actividades de integración correctas, con error o canceladas|Result, FailureType, Activity, ActivityType, Pipeline|
|IntegrationPipelineRunsEnded|No|Ejecuciones de canalización finalizadas|Count|Total|Recuento de ejecuciones de canalización de integración correctas, con error o canceladas|Result, FailureType, Pipeline|
|IntegrationTriggerRunsEnded|No|Ejecuciones de desencadenador finalizadas|Count|Total|Recuento de desencadenadores de integración correctos, con error o cancelados|Result, FailureType, Trigger|
|SQLStreamingBackloggedInputEventSources|No|Eventos de entrada pendientes (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Número de orígenes de eventos de entrada que están pendientes.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingConversionErrors|No|Errores de conversión de datos (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Número de eventos de salida que no se pudieron convertir al esquema de salida previsto. La directiva de errores puede cambiarse a "Drop" para quitar los eventos que se producen en este escenario.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingDeserializationError|No|Errores de deserialización de entrada (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Número de eventos de entrada que no se pudieron deserializar.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingEarlyInputEvents|No|Eventos de entrada tempranos (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Número de eventos de entrada cuya hora de aplicación se considera anticipada en comparación con la hora de llegada, según la directiva de llegada temprana.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingInputEventBytes|No|Bytes de eventos de entrada (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Cantidad de datos recibidos por el trabajo de transmisión, en bytes. Puede usarse para validar que los eventos que se envían al origen de entrada.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingInputEvents|No|Eventos de entrada (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Número de eventos de entrada.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingInputEventsSourcesPerSecond|No|Orígenes de entrada recibidos (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Número de orígenes de eventos de entrada por segundo.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingLateInputEvents|No|Eventos de entrada con retraso (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Número de eventos de entrada cuya hora de aplicación se considera retrasada en comparación con la hora de llegada, según la directiva de llegada con retraso.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingOutOfOrderEvents|No|Eventos desordenados (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Número de eventos del centro de eventos (mensajes serializados) recibidos por el adaptador de entrada del centro de eventos, que se han recibido desordenados y se han eliminado o se les ha dado una marca de tiempo ajustada, según la directiva de ordenación de eventos.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingOutputEvents|No|Eventos de salida (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Número de eventos de salida.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingOutputWatermarkDelaySeconds|No|Retraso de la marca de agua (versión preliminar)|Count|Máxima|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. Retraso de la marca de agua de salida en segundos.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingResourceUtilization|No|Porcentaje de uso de recursos (versión preliminar)|Percent|Máxima|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa.
 Uso de recursos expresado como porcentaje. Una alta utilización indica que el trabajo está usando casi el máximo de recursos asignados.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingRuntimeErrors|No|Errores del entorno de ejecución (versión preliminar)|Count|Total|Se trata de una métrica en versión preliminar disponible en Este de EE. UU., Oeste de Europa. El número total de errores relacionados con el procesamiento de consultas (sin incluir los errores encontrados durante la ingesta de eventos o los resultados de salida).|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|


## <a name="microsoftsynapseworkspacesbigdatapools"></a>Microsoft.Synapse/workspaces/bigDataPools

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BigDataPoolAllocatedCores|No|Núcleos virtuales asignados|Count|Máxima|Núcleos virtuales asignados para un grupo de Apache Spark|SubmitterId|
|BigDataPoolAllocatedMemory|No|Memoria asignada (GB)|Count|Máxima|Memoria asignada para el grupo de Apache Spark (GB)|SubmitterId|
|BigDataPoolApplicationsActive|No|Aplicaciones de Apache Spark activas|Count|Máxima|Total de aplicaciones de grupo de Apache Spark activas|JobState|
|BigDataPoolApplicationsEnded|No|Aplicaciones de Apache Spark finalizadas|Count|Total|Número de aplicaciones de grupo de Apache Spark finalizadas|JobType, JobResult|


## <a name="microsoftsynapseworkspacessqlpools"></a>Microsoft.Synapse/workspaces/sqlPools

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActiveQueries|No|Consultas activas|Count|Total|Las consultas activas. El uso de esta métrica sin filtrar y sin dividir muestra todas las consultas activas que se ejecutan en el sistema.|IsUserDefined|
|AdaptiveCacheHitPercent|No|Porcentaje de aciertos de caché adaptable|Percent|Máxima|Mide en qué grado las cargas de trabajo usan la caché adaptable. Use esta métrica con la métrica del porcentaje de aciertos de caché para determinar si se debe escalar para aumentar la capacidad o volver a ejecutar cargas de trabajo para hidratar la memoria caché.|Sin dimensiones|
|AdaptiveCacheUsedPercent|No|Porcentaje usado de caché adaptable|Percent|Máxima|Mide en qué grado las cargas de trabajo usan la caché adaptable. Use esta métrica con la métrica del porcentaje usado de caché para determinar si se debe escalar para aumentar la capacidad o volver a ejecutar cargas de trabajo para hidratar la memoria caché.|Sin dimensiones|
|Conexiones|Sí|Conexiones|Count|Total|Recuento total de inicios de sesión en el grupo de SQL|Resultado|
|ConnectionsBlockedByFirewall|No|Conexiones bloqueadas por el firewall|Count|Total|Recuento de conexiones bloqueadas por las reglas de firewall. Vuelva a visitar las directivas de control de acceso para el grupo de SQL y supervise estas conexiones si el recuento es alto.|Sin dimensiones|
|CPUPercent|No|Porcentaje usado de CPU|Percent|Máxima|Uso de CPU en todos los nodos del grupo de SQL|Sin dimensiones|
|DWULimit|No|Límite de DWU|Count|Máxima|Objetivo de nivel de servicio del grupo de SQL|Sin dimensiones|
|DWUUsed|No|DWU utilizada|Count|Máxima|Representa una representación de alto nivel del uso en el grupo de SQL. Medido por el límite de DWU * porcentaje de DWU|Sin dimensiones|
|DWUUsedPercent|No|Porcentaje usado de DWU|Percent|Máxima|Representa una representación de alto nivel del uso en el grupo de SQL. Medido tomando el máximo entre el porcentaje de CPU y el porcentaje de E/S de datos|Sin dimensiones|
|LocalTempDBUsedPercent|No|Porcentaje usado de tempdb local|Percent|Máxima|El uso de tempdb local en todos los nodos de ejecución. Los valores se emiten cada cinco minutos.|Sin dimensiones|
|MemoryUsedPercent|No|Porcentaje utilizado de memoria.|Percent|Máxima|Uso de memoria en todos los nodos del grupo de SQL|Sin dimensiones|
|QueuedQueries|No|Consultas en cola|Count|Total|El recuento acumulado de solicitudes en cola una vez alcanzado el límite de simultaneidad máximo.|IsUserDefined|
|WLGActiveQueries|No|Consultas activas de grupo de cargas de trabajo|Count|Total|Consultas activas dentro del grupo de cargas de trabajo. El uso de esta métrica sin filtrar y sin dividir muestra todas las consultas activas que se ejecutan en el sistema.|IsUserDefined, WorkloadGroup|
|WLGActiveQueriesTimeouts|No|Tiempos de espera de consultas de grupo de cargas de trabajo|Count|Total|Consultas para el grupo de cargas de trabajo que han agotado el tiempo de espera. Los tiempos de espera de consulta que indica esta métrica son solo una vez que se ha iniciado la ejecución de la consulta (no incluye el tiempo de espera debido a las esperas de bloqueo o de recursos).|IsUserDefined, WorkloadGroup|
|WLGAllocationByEffectiveCapResourcePercent|No|Asignación de grupos de cargas de trabajo por porcentaje máximo de recursos|Percent|Máxima|Muestra el porcentaje de asignación de recursos en relación con la métrica Porcentaje máximo de recursos efectivo por grupo de cargas de trabajo. Esta métrica proporciona la utilización efectiva del grupo de cargas de trabajo.|IsUserDefined, WorkloadGroup|
|WLGAllocationBySystemPercent|No|Asignación de grupos de cargas de trabajo por porcentaje del sistema|Percent|Máxima|El porcentaje de asignación de recursos en relación con todo el sistema.|IsUserDefined, WorkloadGroup|
|WLGEffectiveCapResourcePercent|No|Porcentaje máximo de recursos efectivo|Percent|Máxima|Porcentaje máximo de recursos efectivo para el grupo de cargas de trabajo. Si hay otros grupos de cargas de trabajo con min_percentage_resource > 0, effective_cap_percentage_resource se reduce proporcionalmente|IsUserDefined, WorkloadGroup|
|WLGEffectiveMinResourcePercent|No|Porcentaje mínimo de recursos efectivo|Percent|Máxima|La configuración de porcentaje de recursos mínimos efectivos permitida teniendo en cuenta el nivel de servicio y la configuración del grupo de cargas de trabajo. El valor min_percentage_resource efectivo se puede ajustar en niveles de servicio superiores o inferiores.|IsUserDefined, WorkloadGroup|
|WLGQueuedQueries|No|Consultas en cola del grupo de cargas de trabajo|Count|Total|El recuento acumulado de solicitudes en cola una vez alcanzado el límite de simultaneidad máximo.|IsUserDefined, WorkloadGroup|


## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|IngressReceivedBytes|Sí|Bytes de entrada recibidos|Bytes|Total|Número de bytes leídos desde todos los orígenes de eventos|Sin dimensiones|
|IngressReceivedInvalidMessages|Sí|Mensajes no válidos de entrada recibidos|Count|Total|Número de mensajes no válidos leídos desde todos los orígenes de eventos de Event Hubs o IoT Hub|Sin dimensiones|
|IngressReceivedMessages|Sí|Mensajes de entrada recibidos|Count|Total|Número de mensajes leídos desde todos los orígenes de eventos de Event Hubs o IoT Hub|Sin dimensiones|
|IngressReceivedMessagesCountLag|Sí|Intervalo de recuento de los mensajes de entrada recibidos|Count|Average|Diferencia entre el número de secuencia del último mensaje en cola en la partición del origen del evento y el del mensaje que se está procesando en la entrada.|Sin dimensiones|
|IngressReceivedMessagesTimeLag|Sí|Tiempo de retardo de los mensajes de entrada recibidos|Segundos|Máxima|Diferencia entre el momento en el que el mensaje se pone en la cola en el origen del evento y el momento en que se procesa en la entrada|Sin dimensiones|
|IngressStoredBytes|Sí|Bytes de entrada almacenados|Bytes|Total|Tamaño total de los eventos almacenados correctamente y disponibles para su consulta|Sin dimensiones|
|IngressStoredEvents|Sí|Eventos de entrada almacenados|Count|Total|Número de eventos acoplados que se procesaron correctamente y que están disponibles para su consulta|Sin dimensiones|
|WarmStorageMaxProperties|Sí|Propiedades máximas del almacenamiento intermedio|Count|Máxima|Número máximo de propiedades utilizadas que permite el entorno para la SKU S1/S2 y número máximo de propiedades admitido por el almacenamiento intermedio para la SKU de pago por uso|Sin dimensiones|
|WarmStorageUsedProperties|Sí|Propiedades utilizadas de almacenamiento intermedio |Count|Máxima|Número de propiedades utilizadas por el entorno para la SKU S1/S2 y número de propiedades que utiliza el almacenamiento intermedio para la SKU de pago por uso|Sin dimensiones|


## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|IngressReceivedBytes|Sí|Bytes de entrada recibidos|Bytes|Total|Número de bytes leídos desde el origen del evento|Sin dimensiones|
|IngressReceivedInvalidMessages|Sí|Mensajes no válidos de entrada recibidos|Count|Total|Número de mensajes leídos no válidos desde el origen del evento|Sin dimensiones|
|IngressReceivedMessages|Sí|Mensajes de entrada recibidos|Count|Total|Número de mensajes leídos desde el origen del evento|Sin dimensiones|
|IngressReceivedMessagesCountLag|Sí|Intervalo de recuento de los mensajes de entrada recibidos|Count|Average|Diferencia entre el número de secuencia del último mensaje en cola en la partición del origen del evento y el del mensaje que se está procesando en la entrada.|Sin dimensiones|
|IngressReceivedMessagesTimeLag|Sí|Tiempo de retardo de los mensajes de entrada recibidos|Segundos|Máxima|Diferencia entre el momento en el que el mensaje se pone en la cola en el origen del evento y el momento en que se procesa en la entrada|Sin dimensiones|
|IngressStoredBytes|Sí|Bytes de entrada almacenados|Bytes|Total|Tamaño total de los eventos almacenados correctamente y disponibles para su consulta|Sin dimensiones|
|IngressStoredEvents|Sí|Eventos de entrada almacenados|Count|Total|Número de eventos acoplados que se procesaron correctamente y que están disponibles para su consulta|Sin dimensiones|
|WarmStorageMaxProperties|Sí|Propiedades máximas del almacenamiento intermedio|Count|Máxima|Número máximo de propiedades utilizadas que permite el entorno para la SKU S1/S2 y número máximo de propiedades admitido por el almacenamiento intermedio para la SKU de pago por uso|Sin dimensiones|
|WarmStorageUsedProperties|Sí|Propiedades utilizadas de almacenamiento intermedio |Count|Máxima|Número de propiedades utilizadas por el entorno para la SKU S1/S2 y número de propiedades que utiliza el almacenamiento intermedio para la SKU de pago por uso|Sin dimensiones|


## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudSimple/virtualMachines

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Bytes de lectura de disco|Sí|Bytes de lectura de disco|Bytes|Total|Rendimiento de disco total debido a las operaciones de lectura durante el período de ejemplo.|Sin dimensiones|
|Operaciones de lectura de disco por segundo|Sí|Operaciones de lectura de disco por segundo|CountPerSecond|Average|El número promedio de operaciones de lectura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|Sin dimensiones|
|Bytes de escritura de disco|Sí|Bytes de escritura de disco|Bytes|Total|Rendimiento de disco total debido a las operaciones de escritura durante el período de ejemplo.|Sin dimensiones|
|Operaciones de escritura por segundo en disco|Sí|Operaciones de escritura por segundo en disco|CountPerSecond|Average|El número promedio de operaciones de escritura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|Sin dimensiones|
|DiskReadBytesPerSecond|Sí|Disk Read Bytes/Sec|BytesPerSecond|Average|Rendimiento de disco promedio debido a las operaciones de lectura durante el período de ejemplo.|Sin dimensiones|
|DiskReadLatency|Sí|Latencia de lectura en disco|Milisegundos|Average|Latencia de lectura total. La suma de las latencias de lectura del kernel y el dispositivo.|Sin dimensiones|
|DiskReadOperations|Sí|Operaciones de lectura en disco|Count|Total|El número de operaciones de lectura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|Sin dimensiones|
|DiskWriteBytesPerSecond|Sí|Disk Write Bytes/Sec|BytesPerSecond|Average|Rendimiento de disco promedio debido a las operaciones de escritura durante el período de ejemplo.|Sin dimensiones|
|DiskWriteLatency|Sí|Latencia de escritura en disco|Milisegundos|Average|Latencia de escritura total. La suma de las latencias de escritura del kernel y el dispositivo.|Sin dimensiones|
|DiskWriteOperations|Sí|Operaciones de escritura en disco|Count|Total|El número de operaciones de escritura de E/S en el período de ejemplo anterior. Tenga en cuenta que estas operaciones pueden ser de tamaño variable.|Sin dimensiones|
|MemoryActive|Sí|Memoria activa|Bytes|Average|La cantidad de memoria utilizada por la máquina virtual en el último período de tiempo reducido. Esta es la cantidad de memoria "verdadera" que requiere la máquina virtual. La memoria adicional que no se utiliza se puede intercambiar o aumentar sin afectar al rendimiento del invitado.|Sin dimensiones|
|MemoryGranted|Sí|Memoria concedida|Bytes|Average|La cantidad de memoria que concedió el host a la máquina virtual. No se concede memoria al host hasta que se toque una vez, y la memoria concedida se puede intercambiar o aumentar en caso de que el kernel de la VM necesite la memoria.|Sin dimensiones|
|MemoryUsed|Sí|Memoria usada|Bytes|Average|La cantidad de memoria de la máquina que está usando la máquina virtual.|Sin dimensiones|
|Red interna|Sí|Red interna|Bytes|Total|Rendimiento de red total para el tráfico recibido.|Sin dimensiones|
|Red interna|Sí|Red interna|Bytes|Total|Rendimiento de red total para el tráfico transmitido.|Sin dimensiones|
|NetworkInBytesPerSecond|Sí|Red en bytes/seg|BytesPerSecond|Average|Rendimiento de red promedio para el tráfico recibido.|Sin dimensiones|
|NetworkOutBytesPerSecond|Sí|Bytes/seg de red externa|BytesPerSecond|Average|Rendimiento de red promedio para el tráfico transmitido.|Sin dimensiones|
|Porcentaje de CPU|Sí|Porcentaje de CPU|Percent|Average|El uso de la CPU. Este valor se notifica con el 100 %, que representa todos los núcleos de procesador del sistema. Por ejemplo, una máquina virtual bidireccional que use el 50 % de un sistema de cuatro núcleos estará usando por completo dos núcleos.|Sin dimensiones|
|PercentageCpuReady|Sí|Preparación de la CPU en porcentaje|Milisegundos|Total|El tiempo de preparación es el tiempo que se tuvo que esperar a que las CPU estuvieran disponibles en el último intervalo de actualización.|Sin dimensiones|


## <a name="microsoftwebconnections"></a>Microsoft.Web/connections

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|Requests|No|Requests|Count|Total|Solicitudes de conexión de API|HttpStatusCode, ClientIPAddress|


## <a name="microsoftwebhostingenvironments"></a>Microsoft.Web/hostingEnvironments

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActiveRequests|Sí|Solicitudes activas (en desuso)|Count|Total|ActiveRequests|Instancia|
|AverageResponseTime|Sí|Tiempo medio de respuesta (en desuso)|Segundos|Average|AverageResponseTime|Instancia|
|BytesReceived|Sí|Entrada de datos|Bytes|Total|BytesReceived|Instancia|
|BytesSent|Sí|Salida de datos|Bytes|Total|BytesSent|Instancia|
|CpuPercentage|Sí|Porcentaje de CPU|Percent|Average|CpuPercentage|Instancia|
|DiskQueueLength|Sí|Longitud de la cola de disco|Count|Average|DiskQueueLength|Instancia|
|Http101|Sí|Http 101|Count|Total|Http101|Instancia|
|Http2xx|Sí|Http 2xx|Count|Total|Http2xx|Instancia|
|Http3xx|Sí|Http 3xx|Count|Total|Http3xx|Instancia|
|Http401|Sí|Http 401|Count|Total|Http401|Instancia|
|Http403|Sí|Http 403|Count|Total|Http403|Instancia|
|Http404|Sí|Http 404|Count|Total|Http404|Instancia|
|Http406|Sí|Http 406|Count|Total|Http406|Instancia|
|Http4xx|Sí|Http 4xx|Count|Total|Http4xx|Instancia|
|Http5xx|Sí|Errores de servidor HTTP|Count|Total|Http5xx|Instancia|
|HttpQueueLength|Sí|Longitud de la cola HTTP|Count|Average|HttpQueueLength|Instancia|
|HttpResponseTime|Sí|Tiempo de respuesta|Segundos|Average|HttpResponseTime|Instancia|
|LargeAppServicePlanInstances|Sí|Trabajos grandes del plan de App Service|Count|Average|Trabajos grandes del plan de App Service|Sin dimensiones|
|MediumAppServicePlanInstances|Sí|Trabajos medianos del plan de App Service|Count|Average|Trabajos medianos del plan de App Service|Sin dimensiones|
|MemoryPercentage|Sí|Porcentaje de memoria|Percent|Average|MemoryPercentage|Instancia|
|Requests|Sí|Requests|Count|Total|Requests|Instancia|
|SmallAppServicePlanInstances|Sí|Trabajos pequeños del plan de App Service|Count|Average|Trabajos pequeños del plan de App Service|Sin dimensiones|
|TotalFrontEnds|Sí|Número total de front-ends|Count|Average|Número total de front-ends|Sin dimensiones|


## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|ActiveRequests|Sí|Solicitudes activas (en desuso)|Count|Total|Solicitudes activas|Instancia|
|AverageResponseTime|Sí|Tiempo medio de respuesta (en desuso)|Segundos|Average|Tiempo medio que tarda el front-end en atender las solicitudes, en segundos.|Instancia|
|BytesReceived|Sí|Entrada de datos|Bytes|Total|Uso medio del ancho de banda de entrada en todos los front-end, en MiB.|Instancia|
|BytesSent|Sí|Salida de datos|Bytes|Total|Uso medio del ancho de banda de entrada en todo el front-end, en MiB.|Instancia|
|CpuPercentage|Sí|Porcentaje de CPU|Percent|Average|Uso medio de la CPU en todas las instancias de front-end.|Instancia|
|DiskQueueLength|Sí|Longitud de la cola de disco|Count|Average|Cantidad media de solicitudes de lectura y escritura en cola en Storage. Una longitud de cola de disco alta es una indicación de que una aplicación podría ralentizarse debido a una excesiva E/S de disco.|Instancia|
|Http101|Sí|Http 101|Count|Total|Número de solicitudes que dan como resultado un código de estado HTTP 101.|Instancia|
|Http2xx|Sí|Http 2xx|Count|Total|Número de solicitudes que devuelven un código de estado HTTP = 200, pero < 300.|Instancia|
|Http3xx|Sí|Http 3xx|Count|Total|Número total de solicitudes que devuelven un código de estado HTTP = 300, pero < 400.|Instancia|
|Http401|Sí|Http 401|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 401.|Instancia|
|Http403|Sí|Http 403|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 403.|Instancia|
|Http404|Sí|Http 404|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 404.|Instancia|
|Http406|Sí|Http 406|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 406.|Instancia|
|Http4xx|Sí|Http 4xx|Count|Total|Número de solicitudes que devuelven un código de estado HTTP = 400, pero < 500.|Instancia|
|Http5xx|Sí|Errores de servidor HTTP|Count|Total|Número de solicitudes que devuelven un código de estado HTTP = 500, pero < 600.|Instancia|
|HttpQueueLength|Sí|Longitud de la cola HTTP|Count|Average|Promedio de solicitudes HTTP que estuvieron en cola antes de realizarse. Una longitud de la cola HTTP elevada o creciente indica que un plan está sobrecargado.|Instancia|
|HttpResponseTime|Sí|Tiempo de respuesta|Segundos|Average|Tiempo que tarda el front-end en atender las solicitudes, en segundos.|Instancia|
|LargeAppServicePlanInstances|Sí|Trabajos grandes del plan de App Service|Count|Average|Trabajos grandes del plan de App Service|Sin dimensiones|
|MediumAppServicePlanInstances|Sí|Trabajos medianos del plan de App Service|Count|Average|Trabajos medianos del plan de App Service|Sin dimensiones|
|MemoryPercentage|Sí|Porcentaje de memoria|Percent|Average|Uso medio de la memoria en todas las instancias del front-end.|Instancia|
|Requests|Sí|Requests|Count|Total|Número total de solicitudes, independientemente de su código de estado HTTP resultante.|Instancia|
|SmallAppServicePlanInstances|Sí|Trabajos pequeños del plan de App Service|Count|Average|Trabajos pequeños del plan de App Service|Sin dimensiones|
|TotalFrontEnds|Sí|Número total de front-ends|Count|Average|Número total de front-ends|Sin dimensiones|


## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|CpuPercentage|Sí|Porcentaje de CPU|Percent|Average|Uso promedio de CPU de todas las instancias del grupo de trabajo.|Instancia|
|MemoryPercentage|Sí|Porcentaje de memoria|Percent|Average|Uso medio de la memoria en todas las instancias del grupo de trabajo.|Instancia|
|WorkersAvailable|Sí|Trabajos disponibles|Count|Average|Trabajos disponibles|Sin dimensiones|
|WorkersTotal|Sí|Número total de trabajos|Count|Average|Número total de trabajos|Sin dimensiones|
|WorkersUsed|Sí|Trabajos usados|Count|Average|Trabajos usados|Sin dimensiones|


## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BytesReceived|Sí|Entrada de datos|Bytes|Total|Uso promedio de ancho de banda entrante entre todas las instancias del plan.|Instancia|
|BytesSent|Sí|Salida de datos|Bytes|Total|Uso promedio de ancho de banda saliente entre todas las instancias del plan.|Instancia|
|CpuPercentage|Sí|Porcentaje de CPU|Percent|Average|Uso promedio de CPU de todas las instancias del plan.|Instancia|
|DiskQueueLength|Sí|Longitud de la cola de disco|Count|Average|Cantidad media de solicitudes de lectura y escritura en cola en Storage. Una longitud de cola de disco alta es una indicación de que una aplicación podría ralentizarse debido a una excesiva E/S de disco.|Instancia|
|HttpQueueLength|Sí|Longitud de la cola HTTP|Count|Average|Promedio de solicitudes HTTP que estuvieron en cola antes de realizarse. Una longitud de la cola HTTP elevada o creciente indica que un plan está sobrecargado.|Instancia|
|MemoryPercentage|Sí|Porcentaje de memoria|Percent|Average|Uso promedio de memoria entre todas las instancias del plan.|Instancia|
|SocketInboundAll|Sí|Recuento de sockets para solicitudes entrantes|Count|Average|Número medio de sockets usados para las solicitudes HTTP entrantes en todas las instancias del plan.|Instancia|
|SocketLoopback|Sí|Recuento de sockets para conexiones de bucles invertidos|Count|Average|Número medio de sockets usados para las conexiones de bucle invertido en todas las instancias del plan.|Instancia|
|SocketOutboundAll|Sí|Recuento de sockets de solicitudes salientes|Count|Average|El número medio de sockets usados para las conexiones salientes en todas las instancias del plan, independientemente de sus estados TCP. Tener demasiadas conexiones salientes puede provocar errores de conectividad.|Instancia|
|SocketOutboundEstablished|Sí|Recuento de sockets establecido para las solicitudes salientes|Count|Average|Número medio de sockets con el estado ESTABLISHED usados para las conexiones salientes en todas las instancias del plan.|Instancia|
|SocketOutboundTimeWait|Sí|Recuento de sockets de Time Wait para solicitudes salientes|Count|Average|Número medio de sockets con el estado TIME_WAIT usados para las conexiones salientes en todas las instancias del plan. Un número elevado o creciente de sockets de salida en estado TIME_WAIT puede provocar errores de conectividad.|Instancia|
|TcpCloseWait|Sí|TCP Close Wait|Count|Average|Número medio de sockets con el estado CLOSE_WAIT en todas las instancias del plan.|Instancia|
|TcpClosing|Sí|TCP Closing (Cierre de TCP)|Count|Average|Número medio de sockets con el estado CLOSING en todas las instancias del plan.|Instancia|
|TcpEstablished|Sí|TCP Established|Count|Average|Número medio de sockets con el estado ESTABLISHED en todas las instancias del plan.|Instancia|
|TcpFinWait1|Sí|TCP Fin Wait 1|Count|Average|Número medio de sockets con el estado FIN_WAIT_1 en todas las instancias del plan.|Instancia|
|TcpFinWait2|Sí|TCP Fin Wait 2|Count|Average|Número medio de sockets con el estado FIN_WAIT_2 en todas las instancias del plan.|Instancia|
|TcpLastAck|Sí|TCP Last Ack|Count|Average|Número medio de sockets con el estado LAST_ACK en todas las instancias del plan.|Instancia|
|TcpSynReceived|Sí|TCP Syn Received|Count|Average|Número medio de sockets con el estado SYN_RCVD en todas las instancias del plan.|Instancia|
|TcpSynSent|Sí|TCP Syn Sent|Count|Average|Número medio de sockets con el estado SYN_SENT en todas las instancias del plan.|Instancia|
|TcpTimeWait|Sí|TCP Time Wait|Count|Average|Número medio de sockets con el estado TIME_WAIT en todas las instancias del plan.|Instancia|


## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AppConnections|Sí|Conexiones|Count|Average|Número de sockets enlazados existente en el espacio aislado (w3wp.exe y sus procesos secundarios). Un socket enlazado se crea mediante una llamada a las API bind()/connect() y permanece hasta que el socket se cierra con CloseHandle()/closesocket(). Para WebApps y FunctionApps.|Instancia|
|AverageMemoryWorkingSet|Sí|Espacio de trabajo de memoria promedio|Bytes|Average|Cantidad media de memoria que utiliza la aplicación, en megabytes (MiB). Para WebApps y FunctionApps.|Instancia|
|AverageResponseTime|Sí|Tiempo medio de respuesta (en desuso)|Segundos|Average|Tiempo promedio en segundos necesario para que la aplicación atienda solicitudes. Para WebApps y FunctionApps.|Instancia|
|BytesReceived|Sí|Entrada de datos|Bytes|Total|Cantidad de ancho de banda entrante consumido por la aplicación, en MiB. Para WebApps y FunctionApps.|Instancia|
|BytesSent|Sí|Salida de datos|Bytes|Total|Cantidad de ancho de banda saliente consumido por la aplicación, en MiB. Para WebApps y FunctionApps.|Instancia|
|CpuTime|Sí|Tiempo de CPU|Segundos|Total|Cantidad de CPU consumida por la aplicación, en segundos. Para más información acerca de esta métrica, consulte https://aka.ms/website-monitor-cpu-time-vs-cpu-percentage (Tiempo de CPU y porcentaje de CPU). Solo para Web Apps.|Instancia|
|CurrentAssemblies|Sí|Ensamblados actuales|Count|Average|Número actual de los ensamblados cargados en todos los dominios de aplicación de esta aplicación. Para WebApps y FunctionApps.|Instancia|
|FileSystemUsage|Sí|Uso del sistema de archivos|Bytes|Average|Porcentaje de la cuota de sistema de archivos consumida por la aplicación. Para WebApps y FunctionApps.|Sin dimensiones|
|FunctionExecutionCount|Sí|Recuento de ejecución de funciones|Count|Total|Recuento de ejecución de funciones. Solo para FunctionApps.|Instancia|
|FunctionExecutionUnits|Sí|Unidades de ejecución de función|Count|Total|Unidades de ejecución de función. Solo para FunctionApps.|Instancia|
|Gen0Collections|Sí|Recolección de elementos no utilizados de Gen 0|Count|Total|Número de veces que los objetos de generación 0 son elementos no utilizados recopilados desde el inicio del proceso de la aplicación. Los elementos no utilizados de última generación incluyen los de generaciones anteriores. Para WebApps y FunctionApps.|Instancia|
|Gen1Collections|Sí|Recolección de elementos no utilizados de Gen 1|Count|Total|Número de veces que los objetos de generación 1 son elementos no utilizados recopilados desde el inicio del proceso de la aplicación. Los elementos no utilizados de última generación incluyen los de generaciones anteriores. Para WebApps y FunctionApps.|Instancia|
|Gen2Collections|Sí|Recolección de elementos no utilizados de Gen 2|Count|Total|Número de veces que los objetos de generación 2 son elementos no utilizados recopilados desde el inicio del proceso de la aplicación. Para WebApps y FunctionApps.|Instancia|
|Asas|Sí|Recuento de identificadores|Count|Average|Total de identificadores abiertos actualmente por el proceso de la aplicación. Para WebApps y FunctionApps.|Instancia|
|HealthCheckStatus|Sí|Estado de comprobación de estado|Count|Average|Estado de la comprobación de estado. Para WebApps y FunctionApps.|Instancia|
|Http101|Sí|Http 101|Count|Total|Número de solicitudes que dan como resultado un código de estado HTTP 101. Para WebApps y FunctionApps.|Instancia|
|Http2xx|Sí|Http 2xx|Count|Total|Número de solicitudes que devuelven un código de estado HTTP = 200, pero < 300. Para WebApps y FunctionApps.|Instancia|
|Http3xx|Sí|Http 3xx|Count|Total|Número total de solicitudes que devuelven un código de estado HTTP = 300, pero < 400. Para WebApps y FunctionApps.|Instancia|
|Http401|Sí|Http 401|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 401. Para WebApps y FunctionApps.|Instancia|
|Http403|Sí|Http 403|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 403. Para WebApps y FunctionApps.|Instancia|
|Http404|Sí|Http 404|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 404. Para WebApps y FunctionApps.|Instancia|
|Http406|Sí|Http 406|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 406. Para WebApps y FunctionApps.|Instancia|
|Http4xx|Sí|Http 4xx|Count|Total|Número de solicitudes que devuelven un código de estado HTTP = 400, pero < 500. Para WebApps y FunctionApps.|Instancia|
|Http5xx|Sí|Errores de servidor HTTP|Count|Total|Número de solicitudes que devuelven un código de estado HTTP = 500, pero < 600. Para WebApps y FunctionApps.|Instancia|
|HttpResponseTime|Sí|Tiempo de respuesta|Segundos|Average|Tiempo en segundos necesario para que la aplicación atienda solicitudes. Para WebApps y FunctionApps.|Instancia|
|IoOtherBytesPerSecond|Sí|Otros bytes de E/S por segundo|BytesPerSecond|Total|La velocidad a la que el proceso de la aplicación emite bytes en las operaciones de E/S que no implican datos, como las operaciones de control. Para WebApps y FunctionApps.|Instancia|
|IoOtherOperationsPerSecond|Sí|Otras operaciones de E/S por segundo|BytesPerSecond|Total|La velocidad a la que el proceso de la aplicación emite operaciones de E/S que no son operaciones de lectura o escritura. Para WebApps y FunctionApps.|Instancia|
|IoReadBytesPerSecond|Sí|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Velocidad a la que el proceso de la aplicación está leyendo bytes de las operaciones de E/S. Para WebApps y FunctionApps.|Instancia|
|IoReadOperationsPerSecond|Sí|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Velocidad a la que el proceso de la aplicación está realizando operaciones de lectura de E/S. Para WebApps y FunctionApps.|Instancia|
|IoReadBytesPerSecond|Sí|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Velocidad a la que el proceso de la aplicación está escribiendo bytes de las operaciones de E/S. Para WebApps y FunctionApps.|Instancia|
|IoWriteOperationsPerSecond|Sí|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Velocidad a la que el proceso de la aplicación está realizando operaciones de escritura de E/S. Para WebApps y FunctionApps.|Instancia|
|MemoryWorkingSet|Sí|Espacio de trabajo de memoria|Bytes|Average|Cantidad actual de memoria utilizada por la aplicación, en MiB. Para WebApps y FunctionApps.|Instancia|
|PrivateBytes|Sí|Bytes privados|Bytes|Average|Bytes privados es el tamaño actual, en bytes, de la memoria asignada por el proceso de la aplicación que no se puede compartir con otros procesos. Para WebApps y FunctionApps.|Instancia|
|Requests|Sí|Requests|Count|Total|Número total de solicitudes, independientemente de su código de estado HTTP resultante. Para WebApps y FunctionApps.|Instancia|
|RequestsInApplicationQueue|Sí|Solicitudes en cola de la aplicación|Count|Average|Número de solicitudes en la cola de solicitudes de la aplicación. Para WebApps y FunctionApps.|Instancia|
|Subprocesos|Sí|Número de subprocesos|Count|Average|Número de subprocesos activos actualmente en el proceso de la aplicación. Para WebApps y FunctionApps.|Instancia|
|TotalAppDomains|Sí|Dominios de aplicación totales|Count|Average|Número actual de dominios de aplicación cargados en esta aplicación. Para WebApps y FunctionApps.|Instancia|
|TotalAppDomainsUnloaded|Sí|Dominios de aplicación totales descargados|Count|Average|Número total de dominios de aplicación descargados desde el inicio de la aplicación. Para WebApps y FunctionApps.|Instancia|


## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AppConnections|Sí|Conexiones|Count|Average|Número de sockets enlazados existente en el espacio aislado (w3wp.exe y sus procesos secundarios). Un socket enlazado se crea mediante una llamada a las API bind()/connect() y permanece hasta que el socket se cierra con CloseHandle()/closesocket().|Instancia|
|AverageMemoryWorkingSet|Sí|Espacio de trabajo de memoria promedio|Bytes|Average|Cantidad media de memoria que utiliza la aplicación, en megabytes (MiB).|Instancia|
|AverageResponseTime|Sí|Tiempo medio de respuesta (en desuso)|Segundos|Average|Tiempo promedio en segundos necesario para que la aplicación atienda solicitudes.|Instancia|
|BytesReceived|Sí|Entrada de datos|Bytes|Total|Cantidad de ancho de banda entrante consumido por la aplicación, en MiB.|Instancia|
|BytesSent|Sí|Salida de datos|Bytes|Total|Cantidad de ancho de banda saliente consumido por la aplicación, en MiB.|Instancia|
|CpuTime|Sí|Tiempo de CPU|Segundos|Total|Cantidad de CPU consumida por la aplicación, en segundos. Para más información acerca de esta métrica, consulte https://aka.ms/website-monitor-cpu-time-vs-cpu-percentage (Tiempo de CPU y porcentaje de CPU).|Instancia|
|CurrentAssemblies|Sí|Ensamblados actuales|Count|Average|Número actual de los ensamblados cargados en todos los dominios de aplicación de esta aplicación.|Instancia|
|FileSystemUsage|Sí|Uso del sistema de archivos|Bytes|Average|Porcentaje de la cuota de sistema de archivos consumida por la aplicación.|Sin dimensiones|
|FunctionExecutionCount|Sí|Recuento de ejecución de funciones|Count|Total|Recuento de ejecución de funciones|Instancia|
|FunctionExecutionUnits|Sí|Unidades de ejecución de función|Count|Total|Unidades de ejecución de función|Instancia|
|Gen0Collections|Sí|Recolección de elementos no utilizados de Gen 0|Count|Total|Número de veces que los objetos de generación 0 son elementos no utilizados recopilados desde el inicio del proceso de la aplicación. Los elementos no utilizados de última generación incluyen los de generaciones anteriores.|Instancia|
|Gen1Collections|Sí|Recolección de elementos no utilizados de Gen 1|Count|Total|Número de veces que los objetos de generación 1 son elementos no utilizados recopilados desde el inicio del proceso de la aplicación. Los elementos no utilizados de última generación incluyen los de generaciones anteriores.|Instancia|
|Gen2Collections|Sí|Recolección de elementos no utilizados de Gen 2|Count|Total|Número de veces que los objetos de generación 2 son elementos no utilizados recopilados desde el inicio del proceso de la aplicación.|Instancia|
|Asas|Sí|Recuento de identificadores|Count|Average|Total de identificadores abiertos actualmente por el proceso de la aplicación.|Instancia|
|HealthCheckStatus|Sí|Estado de comprobación de estado|Count|Average|Estado de comprobación de estado|Instancia|
|Http101|Sí|Http 101|Count|Total|Número de solicitudes que dan como resultado un código de estado HTTP 101.|Instancia|
|Http2xx|Sí|Http 2xx|Count|Total|Número de solicitudes que devuelven un código de estado HTTP = 200, pero < 300.|Instancia|
|Http3xx|Sí|Http 3xx|Count|Total|Número total de solicitudes que devuelven un código de estado HTTP = 300, pero < 400.|Instancia|
|Http401|Sí|Http 401|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 401.|Instancia|
|Http403|Sí|Http 403|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 403.|Instancia|
|Http404|Sí|Http 404|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 404.|Instancia|
|Http406|Sí|Http 406|Count|Total|Cantidad total de solicitudes que devuelven el código de estado HTTP 406.|Instancia|
|Http4xx|Sí|Http 4xx|Count|Total|Número de solicitudes que devuelven un código de estado HTTP = 400, pero < 500.|Instancia|
|Http5xx|Sí|Errores de servidor HTTP|Count|Total|Número de solicitudes que devuelven un código de estado HTTP = 500, pero < 600.|Instancia|
|HttpResponseTime|Sí|Tiempo de respuesta|Segundos|Average|Tiempo en segundos necesario para que la aplicación atienda solicitudes.|Instancia|
|IoOtherBytesPerSecond|Sí|Otros bytes de E/S por segundo|BytesPerSecond|Total|La velocidad a la que el proceso de la aplicación emite bytes en las operaciones de E/S que no implican datos, como las operaciones de control.|Instancia|
|IoOtherOperationsPerSecond|Sí|Otras operaciones de E/S por segundo|BytesPerSecond|Total|La velocidad a la que el proceso de la aplicación emite operaciones de E/S que no son operaciones de lectura o escritura.|Instancia|
|IoReadBytesPerSecond|Sí|Bytes de lectura de E/S por segundo|BytesPerSecond|Total|Velocidad a la que el proceso de la aplicación está leyendo bytes de las operaciones de E/S.|Instancia|
|IoReadOperationsPerSecond|Sí|Operaciones de lectura de E/S por segundo|BytesPerSecond|Total|Velocidad a la que el proceso de la aplicación está realizando operaciones de lectura de E/S.|Instancia|
|IoReadBytesPerSecond|Sí|Bytes de escritura de E/S por segundo|BytesPerSecond|Total|Velocidad a la que el proceso de la aplicación está escribiendo bytes de las operaciones de E/S.|Instancia|
|IoWriteOperationsPerSecond|Sí|Operaciones de escritura de E/S por segundo|BytesPerSecond|Total|Velocidad a la que el proceso de la aplicación está realizando operaciones de escritura de E/S.|Instancia|
|MemoryWorkingSet|Sí|Espacio de trabajo de memoria|Bytes|Average|Cantidad actual de memoria utilizada por la aplicación, en MiB.|Instancia|
|PrivateBytes|Sí|Bytes privados|Bytes|Average|Bytes privados es el tamaño actual, en bytes, de la memoria asignada por el proceso de la aplicación que no se puede compartir con otros procesos.|Instancia|
|Requests|Sí|Requests|Count|Total|Número total de solicitudes, independientemente de su código de estado HTTP resultante.|Instancia|
|RequestsInApplicationQueue|Sí|Solicitudes en cola de la aplicación|Count|Average|Número de solicitudes en la cola de solicitudes de la aplicación.|Instancia|
|Subprocesos|Sí|Número de subprocesos|Count|Average|Número de subprocesos activos actualmente en el proceso de la aplicación.|Instancia|
|TotalAppDomains|Sí|Dominios de aplicación totales|Count|Average|Número actual de dominios de aplicación cargados en esta aplicación.|Instancia|
|TotalAppDomainsUnloaded|Sí|Dominios de aplicación totales descargados|Count|Average|Número total de dominios de aplicación descargados desde el inicio de la aplicación.|Instancia|


## <a name="microsoftwebstaticsites"></a>Microsoft.Web/staticSites

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BytesSent|Sí|Salida de datos|Bytes|Total|BytesSent|Instancia|
|FunctionErrors|Sí|FunctionErrors|Count|Total|FunctionErrors|Instancia|
|FunctionHits|Sí|FunctionHits|Count|Total|FunctionHits|Instancia|
|SiteErrors|Sí|SiteErrors|Count|Total|SiteErrors|Instancia|
|SiteHits|Sí|SiteHits|Count|Total|SiteHits|Instancia|


## <a name="wandiscofusionmigrators"></a>Wandisco.Fusion/migrators

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|BytesPerSecond|Sí|Bytes por segundo|BytesPerSecond|Average|Velocidad de rendimiento en bytes por segundo que usa un migrador.|Sin dimensiones|
|DirectoriesCreatedCount|Sí|Directories Created Count (Recuento de directorios creados)|Count|Total|Proporciona una vista acumulada de cuántos directorios se han creado como parte de una migración.|Sin dimensiones|
|FileMigrationCount|Sí|Files Migration Count (Recuento de archivos migrados)|Count|Total|Proporciona un total acumulado de cuántos archivos se han migrado.|Sin dimensiones|
|InitialScanDataMigratedInBytes|Sí|Initial Scan Data Migrated in Bytes (Datos de examen inicial migrados en bytes)|Bytes|Total|Proporciona la vista del total de bytes que se han transferido en un migrador nuevo como resultado del examen inicial del sistema de archivos local. Los datos que se agregan a la migración después de la migración de examen inicial no se incluyen en esta métrica.|Sin dimensiones|
|LiveDataMigratedInBytes|Sí|Live Data Migrated in Bytes (Datos dinámicos migrados en bytes)|Count|Total|Proporciona un total acumulado de datos dinámicos que han cambiado debido a la actividad del cliente desde que se inició la migración.|Sin dimensiones|
|MigratorCPULoad|Sí|Migrator CPU Load (Carga de CPU del migrador)|Percent|Average|Consumo de CPU del proceso del migrador.|Sin dimensiones|
|NumberOfExcludedPaths|Sí|Number of Excluded Paths (Número de rutas de acceso excluidas)|Count|Total|Proporciona un recuento acumulado de las rutas de acceso que se han excluido de la migración debido a las reglas de exclusión.|Sin dimensiones|
|NumberOfFailedPaths|Sí|Number of Failed Paths (Número de rutas de acceso con error)|Count|Total|Recuento de las rutas de acceso que no se han podido migrar.|Sin dimensiones|
|SystemCPULoad|Sí|System CPU Load (Carga de CPU del sistema)|Percent|Average|Consumo total de CPU.|Sin dimensiones|
|TotalMigratedDataInBytes|Sí|Total Migrated Data in Bytes (Total de datos migrados en bytes)|Bytes|Total|Proporciona una vista de los bytes migrados correctamente de un migrador determinado.|Sin dimensiones|
|TotalTransactions|Sí|Transacciones totales|Count|Total|Proporciona un total acumulado de las transacciones de datos por las que se podría facturar al usuario.|Sin dimensiones|

## <a name="next-steps"></a>Pasos siguientes

- [Lea información sobre las métricas en Azure Monitor](../data-platform.md)
- [Creación de alertas basadas en métricas](../alerts/alerts-overview.md)
- [Exportación de métricas a cuentas de almacenamiento, Event Hubs o Log Analytics](../essentials/platform-logs-overview.md)
