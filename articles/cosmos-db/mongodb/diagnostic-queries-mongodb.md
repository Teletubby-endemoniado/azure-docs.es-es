---
title: Solución de problemas con consultas de diagnóstico avanzadas para Mongo API
titleSuffix: Azure Cosmos DB
description: Obtenga información sobre cómo consultar registros de diagnóstico para solucionar problemas de los datos almacenados en Azure Cosmos DB para MongoDB API.
services: cosmos-db
ms.service: cosmos-db
ms.topic: how-to
ms.date: 06/12/2021
ms.author: esarroyo
author: StefArroyo
ms.openlocfilehash: 73dba970aea4171a99ff499300d15c63c3bbb380
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131430617"
---
# <a name="troubleshoot-issues-with-advanced-diagnostics-queries-for-the-mongodb-api"></a>Solución de problemas con consultas de diagnóstico avanzadas para MongoDB API

[!INCLUDE[appliesto-all-apis-except-table](../includes/appliesto-all-apis-except-table.md)]

> [!div class="op_single_selector"]
> * [SQL API (Core)](../cosmos-db-advanced-queries.md)
> * [MongoDB API](diagnostic-queries-mongodb.md)
> * [Cassandra API](../cassandra/diagnostic-queries-cassandra.md)
> * [Gremlin API](../queries-gremlin.md)
>

En este artículo, se explicará cómo escribir consultas más avanzadas para ayudar a solucionar problemas relacionados con la cuenta de Azure Cosmos DB mediante registros de diagnóstico enviados a las tablas de **Azure Diagnostics (heredado)** y **específicas del recurso (versión preliminar)** .

Para las tablas de Azure Diagnostics, todos los datos se escriben en una única tabla. Los usuarios especifican qué categoría quieren consultar. Si quiere ver la consulta de texto completo de la solicitud, vea [Supervisión de datos de Azure Cosmos DB mediante la configuración de diagnóstico en Azure](../cosmosdb-monitor-resource-logs.md#full-text-query) para obtener información sobre cómo habilitar esta característica.

En el caso de las [tablas específicas del recurso](../cosmosdb-monitor-resource-logs.md#create-setting-portal), los datos se escriben en tablas individuales para cada categoría del recurso. Se recomienda este modo porque:

- Facilita considerablemente el trabajo con los datos. 
- Proporciona una mejor detectabilidad de los esquemas.
- Mejora el rendimiento tanto de la latencia de ingesta como de los tiempos de consulta.

## <a name="common-queries"></a>Consultas comunes
Las consultas comunes se muestran en las tablas específicas del recurso y de Azure Diagnostics.

### <a name="top-n10-request-unit-ru-consuming-requests-or-queries-in-a-specific-time-frame"></a>Principales N(10) unidades de solicitud (RU) que consumen solicitudes o consultas en un período de tiempo específico

# <a name="resource-specific"></a>[Específico del recurso](#tab/resource-specific)
   ```Kusto
   //Enable full-text query to view entire query text
   CDBMongoRequests
   | where TimeGenerated > ago(24h)
   | project PIICommandText, ActivityId, DatabaseName , CollectionName, RequestCharge
   | order by RequestCharge desc
   | take 10
   ```

# <a name="azure-diagnostics"></a>[Diagnóstico de Azure](#tab/azure-diagnostics)
   ```Kusto
   AzureDiagnostics
   | where Category == "MongoRequests"
   | where TimeGenerated > ago(24h)
   | project piiCommandText_s, activityId_g, databaseName_s , collectionName_s, requestCharge_s
   | order by requestCharge_s desc
   | take 10
   ```    
---

### <a name="requests-throttled-statuscode--429-or-16500-in-a-specific-time-window"></a>Solicitudes limitadas (statusCode = 429 o 16500) en un período de tiempo determinado 

# <a name="resource-specific"></a>[Específico del recurso](#tab/resource-specific)
   ```Kusto
   CDBMongoRequests
   | where TimeGenerated > ago(24h)
   | where ErrorCode == "429" or ErrorCode == "16500"
   | project DatabaseName, CollectionName, PIICommandText, OperationName, TimeGenerated
   ```

# <a name="azure-diagnostics"></a>[Diagnóstico de Azure](#tab/azure-diagnostics)
   ```Kusto
   AzureDiagnostics
   | where Category == "MongoRequests" and TimeGenerated > ago(24h)
   | where ErrorCode == "429" or ErrorCode == "16500"
   | project databaseName_s , collectionName_s , piiCommandText_s , OperationName, TimeGenerated
   ```    
---

### <a name="timed-out-requests-statuscode--50-in-a-specific-time-window"></a>Solicitudes con tiempo de espera agotado (statusCode = 50) en un período de tiempo determinado 

# <a name="resource-specific"></a>[Específico del recurso](#tab/resource-specific)
   ```Kusto
   CDBMongoRequests
   | where TimeGenerated > ago(24h)
   | where ErrorCode == "50"
   | project DatabaseName, CollectionName, PIICommandText, OperationName, TimeGenerated
   ```
# <a name="azure-diagnostics"></a>[Diagnóstico de Azure](#tab/azure-diagnostics)
   ```Kusto
   AzureDiagnostics
   | where Category == "MongoRequests" and TimeGenerated > ago(24h)
   | where ErrorCode == "50"
   | project databaseName_s , collectionName_s , piiCommandText_s , OperationName, TimeGenerated
   ```    
---

### <a name="queries-with-large-response-lengths-payload-size-of-the-server-response"></a>Consultas con longitudes de respuesta largas (tamaño de la carga de la respuesta del servidor)

# <a name="resource-specific"></a>[Específico del recurso](#tab/resource-specific)
   ```Kusto
   CDBMongoRequests
   //specify collection and database
   //| where DatabaseName == "DBNAME" and CollectionName == "COLLECTIONNAME"
   | summarize max(ResponseLength) by PIICommandText, RequestCharge, DurationMs, OperationName, TimeGenerated
   | order by max_ResponseLength desc
   ```
# <a name="azure-diagnostics"></a>[Diagnóstico de Azure](#tab/azure-diagnostics)
   ```Kusto
   AzureDiagnostics
   | where Category == "MongoRequests"
   //specify collection and database
   //| where databaseName_s == "DBNAME" and collectionName_s == "COLLECTIONNAME"
   | summarize max(responseLength_s) by piiCommandText_s, OperationName, duration_s, requestCharge_s
   | order by max_responseLength_s desc
   ```    
---

### <a name="ru-consumption-by-physical-partition-across-all-replicas-in-the-replica-set"></a>Consumo de RU por partición física (en todas las réplicas del conjunto de réplicas)

# <a name="resource-specific"></a>[Específico del recurso](#tab/resource-specific)
   ```Kusto
   CDBPartitionKeyRUConsumption
   | where TimeGenerated >= now(-1d)
   //specify collection and database
   //| where DatabaseName == "DBNAME" and CollectionName == "COLLECTIONNAME"
   // filter by operation type
   //| where operationType_s == 'Create'
   | summarize sum(todouble(RequestCharge)) by toint(PartitionKeyRangeId)
   | render columnchart
   ```

# <a name="azure-diagnostics"></a>[Diagnóstico de Azure](#tab/azure-diagnostics)
   ```Kusto
   AzureDiagnostics
   | where TimeGenerated >= now(-1d)
   | where Category == 'PartitionKeyRUConsumption'
   //specify collection and database
   //| where databaseName_s == "DBNAME" and collectionName_s == "COLLECTIONNAME"
   // filter by operation type
   //| where operationType_s == 'Create'
   | summarize sum(todouble(requestCharge_s)) by toint(partitionKeyRangeId_s)
   | render columnchart  
   ```    
---

### <a name="ru-consumption-by-logical-partition-across-all-replicas-in-the-replica-set"></a>Consumo de RU por partición lógica (en todas las réplicas del conjunto de réplicas)

# <a name="resource-specific"></a>[Específico del recurso](#tab/resource-specific)
   ```Kusto
   CDBPartitionKeyRUConsumption
   | where TimeGenerated >= now(-1d)
   //specify collection and database
   //| where DatabaseName == "DBNAME" and CollectionName == "COLLECTIONNAME"
   // filter by operation type
   //| where operationType_s == 'Create'
   | summarize sum(todouble(RequestCharge)) by PartitionKey, PartitionKeyRangeId
   | render columnchart  
   ```
# <a name="azure-diagnostics"></a>[Diagnóstico de Azure](#tab/azure-diagnostics)
   ```Kusto
   AzureDiagnostics
   | where TimeGenerated >= now(-1d)
   | where Category == 'PartitionKeyRUConsumption'
   //specify collection and database
   //| where databaseName_s == "DBNAME" and collectionName_s == "COLLECTIONNAME"
   // filter by operation type
   //| where operationType_s == 'Create'
   | summarize sum(todouble(requestCharge_s)) by partitionKey_s, partitionKeyRangeId_s
   | render columnchart  
   ```
---

## <a name="next-steps"></a>Pasos siguientes 
* Para obtener más información sobre cómo crear una configuración de diagnóstico para Azure Cosmos DB, vea [Creación de una configuración de diagnóstico](../cosmosdb-monitor-resource-logs.md).
* Para obtener más información sobre cómo crear una configuración de diagnóstico mediante Azure Portal, la CLI de Azure o PowerShell, vea [Creación de una configuración de diagnóstico para recopilar los registros y las métricas de la plataforma en Azure](../../azure-monitor/essentials/diagnostic-settings.md).
