---
title: Desencadenador de Azure Cosmos DB para Functions 2.x y versiones posteriores
description: Aprenda a usar el desencadenador de Azure Cosmos DB en Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 09/01/2021
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 0bccf556f48cea52c4458ec6afc882c3c6035a71
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128635192"
---
# <a name="azure-cosmos-db-trigger-for-azure-functions-2x-and-higher"></a>Desencadenador de Azure Cosmos DB para Azure Functions 2.x y versiones posteriores

El desencadenador de Azure Cosmos DB utiliza la [fuente de cambios de Azure Cosmos DB](../cosmos-db/change-feed.md) para estar atento a las inserciones y actualizaciones de las particiones. La fuente de cambios publica inserciones y actualizaciones, no eliminaciones.

Para obtener información sobre los detalles de instalación y configuración, vea la [información general](./functions-bindings-cosmosdb-v2.md).

<a id="example" name="example"></a>

# <a name="c"></a>[C#](#tab/csharp)

En el ejemplo siguiente se muestra un [función de C#](functions-dotnet-class-library.md) que se invoca cuando hay inserciones y actualizaciones en la base de datos y colección especificadas.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents,
            ILogger log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.LogInformation($"Documents modified: {documents.Count}");
                log.LogInformation($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

Las aplicaciones que usan la [versión 4.x o posterior de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher) de Cosmos DB tendrán propiedades de atributo diferentes, que se muestran a continuación. En este ejemplo se hace referencia a un tipo simple `ToDoItem`.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

```cs
using System.Collections.Generic;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "databaseName",
            containerName: "containerName",
            Connection = "CosmosDBConnectionSetting",
            LeaseContainerName = "leases",
            CreateLeaseContainerIfNotExists = true)]IReadOnlyList<ToDoItem> input, ILogger log)
        {
            if (input != null && input.Count > 0)
            {
                log.LogInformation("Documents modified " + input.Count);
                log.LogInformation("First document Id " + input[0].Id);
            }
        }
    }
}
```

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

En el ejemplo siguiente se muestra un enlace de desencadenador de Cosmos DB en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función escribe mensajes de registro cuando se agregan o modifican los registros de Cosmos DB.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Este es el código de script de C#:

```cs
    #r "Microsoft.Azure.DocumentDB.Core"

    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using Microsoft.Extensions.Logging;

    public static void Run(IReadOnlyList<Document> documents, ILogger log)
    {
      log.LogInformation("Documents modified " + documents.Count);
      log.LogInformation("First document Id " + documents[0].Id);
    }
```

# <a name="java"></a>[Java](#tab/java)

Esta función se invoca cuando hay inserciones y actualizaciones en la base de datos y la colección especificadas.

```java
    @FunctionName("cosmosDBMonitor")
    public void cosmosDbProcessor(
        @CosmosDBTrigger(name = "items",
            databaseName = "ToDoList",
            collectionName = "Items",
            leaseCollectionName = "leases",
            createLeaseCollectionIfNotExists = true,
            connectionStringSetting = "AzureCosmosDBConnection") String[] items,
            final ExecutionContext context ) {
                context.getLogger().info(items.length + "item(s) is/are changed.");
            }
```


En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@CosmosDBTrigger` en los parámetros cuyo valor provendría de Cosmos DB.  Esta anotación se puede usar con tipos nativos de Java, POJO o valores que aceptan valores NULL mediante `Optional<T>`.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

En el ejemplo siguiente se muestra un enlace de desencadenador de Cosmos DB en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función escribe mensajes de registro cuando se agregan o modifican los registros de Cosmos DB.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Este es el código de JavaScript:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

En el ejemplo siguiente se muestra cómo ejecutar una función a medida que los datos cambian en Cosmos DB.

```json
{
  "type": "cosmosDBTrigger",
  "name": "Documents",
  "direction": "in",
  "leaseCollectionName": "leases",
  "connectionStringSetting": "MyStorageConnectionAppSetting",
  "databaseName": "Tasks",
  "collectionName": "Items",
  "createLeaseCollectionIfNotExists": true
}
```

En el archivo _run.ps1_, tiene acceso al documento que desencadena la función a través del parámetro `$Documents`.

```powershell
param($Documents, $TriggerMetadata) 

Write-Host "First document Id modified : $($Documents[0].id)" 
```

# <a name="python"></a>[Python](#tab/python)

En el ejemplo siguiente se muestra un enlace del desencadenador de Cosmos DB en un archivo *function.json* y una [función de Python](functions-reference-python.md) que usa dicho enlace. La función escribe mensajes de registro cuando se modifican los registros de Cosmos DB.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDBTrigger",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Este es el código de Python:

```python
    import logging
    import azure.functions as func


    def main(documents: func.DocumentList) -> str:
        if documents:
            logging.info('First document Id modified: %s', documents[0]['id'])
```

---

## <a name="attributes-and-annotations"></a>Atributos y anotaciones

# <a name="c"></a>[C#](#tab/csharp)

En las [bibliotecas de clases de C#](functions-dotnet-class-library.md), use el atributo [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs).

El constructor del atributo toma el nombre de la base de datos y el nombre de la colección. Para información sobre esos valores y otras propiedades que puede configurar, consulte [Desencadenador: configuración](#configuration). A continuación, se muestra un ejemplo del atributo `CosmosDBTrigger` en una signatura de método:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run([CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
        IReadOnlyList<Document> documents,
        ILogger log)
    {
        ...
    }
```

En la [versión 4.x de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher), algunos valores y propiedades se han quitado o se han cambiado de nombre. Para obtener más información sobre estos cambios, consulte [Desencadenador: configuración](#configuration). Aquí hay un ejemplo de atributo `CosmosDBTrigger` en una firma de método que hace referencia a un tipo simple `ToDoItem`:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run([CosmosDBTrigger("database", "container", Connection = "CosmosDBConnectionSetting")]
        IReadOnlyList<ToDoItem> documents,  
        ILogger log)
    {
        ...
    }
```

Para ver un ejemplo completo de cualquiera de las versiones de la extensión, consulte [Desencadenar](#example).

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="java"></a>[Java](#tab/java)

En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@CosmosDBInput` en los parámetros que leen los datos de Cosmos DB.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell no admite atributos.

# <a name="python"></a>[Python](#tab/python)

Python no admite atributos.

---

## <a name="configuration"></a>Configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `CosmosDBTrigger`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `cosmosDBTrigger`. |
|**direction** | N/D | Se debe establecer en `in`. Este parámetro se establece automáticamente cuando se crea el desencadenador en Azure Portal. |
|**name** | N/D | Nombre de la variable que se utiliza en el código de función y que representa la lista de documentos con los cambios. |
|**connectionStringSetting** <br> o <br> **connection**|**ConnectionStringSetting** <br> o <br> **Connection**| Nombre de una configuración de aplicación que contiene la cadena de conexión utilizada para conectarse a la cuenta de Azure Cosmos DB que se está supervisando. <br><br> En la [versión 4.x de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher), esta propiedad se denomina `Connection`. El valor es el nombre de una configuración de aplicación que contiene la cadena de conexión que se usa para conectarse a la cuenta de Azure Cosmos DB que se está supervisando, o que contiene una sección o prefijo de configuración que define la conexión. Consulte [Conexiones](./functions-reference.md#connections). |
|**databaseName**|**DatabaseName**  | Nombre de la base de datos de Azure Cosmos DB con la colección que se está supervisando. |
|**collectionName** <br> o <br> **containerName** |**CollectionName** <br> o <br> **ContainerName** | Nombre de la colección que se está supervisando. <br><br> En la [versión 4.x de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher), esta propiedad se denomina `ContainerName`. |
|**leaseConnectionStringSetting** <br> o <br> **leaseConnection** | **LeaseConnectionStringSetting** <br> o <br> **LeaseConnection** | (Opcional) Nombre de una configuración de aplicación que contiene la cadena de conexión a la cuenta de Azure Cosmos DB que incluye la colección de concesiones. Si no se establece, se usa el valor `connectionStringSetting`. Este parámetro se establece automáticamente cuando se crea el enlace en el portal. La cadena de conexión para la colección de concesiones debe tener permisos de escritura. <br><br> En la [versión 4.x de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher), esta propiedad se denomina `LeaseConnection` y, si no se establece, se usará el valor `Connection`. El valor es el nombre de una configuración de aplicación que contiene la cadena de conexión usada para conectarse a la cuenta de Azure Cosmos DB con el contenedor de concesión, o que contiene una sección o prefijo de configuración que define la conexión. Consulte [Conexiones](./functions-reference.md#connections).|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Opcional) Nombre de la base de datos que contiene la colección que se usa para almacenar las concesiones. Si no se establece, se usa el valor de la configuración `databaseName`. Este parámetro se establece automáticamente cuando se crea el enlace en el portal. |
|**leaseCollectionName** <br> o <br> **leaseContainerName** | **LeaseCollectionName** <br> o <br> **LeaseContainerName** | (Opcional) Nombre de la colección que se usa para almacenar concesiones. Si no se establece, se usa el valor `leases`. <br><br> En la [versión 4.x de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher), esta propiedad se denomina `LeaseContainerName`. |
|**createLeaseCollectionIfNotExists** <br> o <br> **createLeaseContainerIfNotExists** | **CreateLeaseCollectionIfNotExists** <br> o <br> **CreateLeaseContainerIfNotExists** | (Opcional) Cuando se establece en `true`, la colección de concesiones se crea automáticamente cuando todavía no existe. El valor predeterminado es `false`. <br><br> En la [versión 4.x de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher), esta propiedad se denomina `CreateLeaseContainerIfNotExists`. |
|**leasesCollectionThroughput** <br> o <br> **leasesContainerThroughput**| **LeasesCollectionThroughput** <br> o <br> **LeasesContainerThroughput**| (Opcional) Define la cantidad de unidades de solicitud que se asignan cuando se crea la colección de concesiones. Esta configuración solo se usa cuando `createLeaseCollectionIfNotExists` se establece en `true`. Este parámetro se establece automáticamente cuando el enlace se crea con el portal. <br><br> En la [versión 4.x de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher), esta propiedad se denomina `LeasesContainerThroughput`. |
|**leaseCollectionPrefix** <br> o <br> **leaseContainerPrefix**| **LeaseCollectionPrefix** <br> o <br> **leaseContainerPrefix** | (Opcional) Cuando se establece, el valor se agrega como prefijo a las concesiones creadas en la colección de concesiones para esta función. El uso de un prefijo permite que dos funciones de Azure Functions independientes compartan la misma colección de concesiones con prefijos diferentes. <br><br> En la [versión 4.x de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher), esta propiedad se denomina `LeaseContainerPrefix`. |
|**feedPollDelay**| **FeedPollDelay**| (Opcional) El tiempo (en milisegundos) del retraso entre sondeos de una partición en busca de nuevos cambios en la fuente, después de que todos los cambios actuales se purguen. El valor predeterminado es de 5 000 milisegundos (o 5 segundos).
|**leaseAcquireInterval**| **LeaseAcquireInterval**| (Opcional) Al establecerse, define, en milisegundos, el intervalo para iniciar una tarea para calcular si las particiones se distribuyen uniformemente entre las instancias de host conocidas. El valor predeterminado es 13 000 (13 segundos).
|**leaseExpirationInterval**| **LeaseExpirationInterval**| (Opcional) Al establecerse, define, en milisegundos, el intervalo para el que se toma la concesión en una concesión que representa una partición. Si la concesión no se renueva dentro de este intervalo, expirará y la propiedad de la partición se moverá a otra instancia. El valor predeterminado es 60 000 (60 segundos).
|**leaseRenewInterval**| **LeaseRenewInterval**| (Opcional) Al establecerse, define, en milisegundos, el intervalo de renovación para todas las concesiones para las particiones que mantiene una instancia actualmente. El valor predeterminado es 17 000 (17 segundos).
|**checkpointInterval**| **CheckpointInterval**| (Opcional) Al establecerse, define, en milisegundos, el intervalo entre los puntos de comprobación de las concesiones. El valor predeterminado siempre se encuentra después de una llamada de función. <br><br> En la [versión 4.x de la extensión](./functions-bindings-cosmosdb-v2.md#cosmos-db-extension-4x-and-higher), esta propiedad no está disponible. |
|**maxItemsPerInvocation**| **MaxItemsPerInvocation**| (Opcional) Al establecerse, esta propiedad establece la cantidad máxima de elementos recibidos por llamada de función. Si las operaciones de la colección supervisada se realizan mediante procedimientos almacenados, el [ámbito de transacción](../cosmos-db/stored-procedures-triggers-udfs.md#transactions) se conserva al leer los elementos de la fuente de cambios. Como resultado, es posible que el número de elementos recibidos sea mayor que el valor especificado, de modo que los elementos que cambian en la misma transacción se devuelvan como parte de un lote atómico.
|**startFromBeginning**| **StartFromBeginning**| (Opcional) Esta opción indica al desencadenador que lea los cambios desde el principio del historial de cambios de la colección en lugar de comenzar en la hora actual. La lectura desde el principio solo funciona la primera vez que se inicia el desencadenador, ya que en las ejecuciones posteriores, los puntos de control ya están almacenados. Si esta opción se establece en `true` cuando ya hay concesiones creadas no tiene ningún efecto. |
|**preferredLocations**| **PreferredLocations**| (Opcional) Defina las ubicaciones preferidas (regiones) para las cuentas de base de datos con replicación geográfica en el servicio de Azure Cosmos DB. Los valores deben estar separados por comas. Por ejemplo, "Este de EE. UU., Centro-sur de EE. UU. Norte de Europa". |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Uso

El desencadenador requiere una segunda colección que utiliza para almacenar _concesiones_ en las particiones. La colección que se está supervisando y la colección que contiene las concesiones deben estar disponibles para que el desencadenador funcione.

>[!IMPORTANT]
> Si hay varias funciones configuradas para usar un desencadenador de Cosmos DB para la misma colección, cada una de ellas debe usar una colección de concesión dedicada, o bien se debe especificar un valor de `LeaseCollectionPrefix` para cada función. En caso contrario, se desencadenará solo una de las funciones. Para obtener información acerca del prefijo, consulte la [sección de configuración](#configuration).

El desencadenador no indica si un documento se actualizó o se insertó; solo proporciona el propio documento. Si tiene que administrar las actualizaciones e inserciones de manera diferente, puede hacerlo mediante la implementación de campos de marca de tiempo para las inserciones o actualizaciones.

## <a name="next-steps"></a>Pasos siguientes

- [Lectura de un documento de Azure Cosmos DB (enlace de entrada)](./functions-bindings-cosmosdb-v2-input.md)
- [Guardar los cambios en un documento de Azure Cosmos DB (enlace de salida)](./functions-bindings-cosmosdb-v2-output.md)
