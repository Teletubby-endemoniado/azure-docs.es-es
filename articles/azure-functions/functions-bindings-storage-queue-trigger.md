---
title: Desencadenador de Azure Queue Storage para Azure Functions
description: Aprenda a ejecutar una instancia de Azure Functions a medida que cambian los datos de Azure Queue Storage.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2020
ms.author: cshoe
ms.custom: devx-track-csharp, cc996988-fb4f-47, devx-track-python
ms.openlocfilehash: 716026480ec2b6266edfc69e55d71d0bc1c303cf
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129997903"
---
# <a name="azure-queue-storage-trigger-for-azure-functions"></a>Desencadenador de Azure Queue Storage para Azure Functions

El desencadenador de Queue Storage ejecuta una función a medida que se agregan mensajes a Azure Queue Storage.

## <a name="encoding"></a>Encoding

Las funciones esperan una cadena codificada en *base64*. Cualquier ajuste al tipo de codificación (para preparar los datos como una cadena codificada en *base64*) debe implementarse en el servicio de llamada.

## <a name="example"></a>Ejemplo

Use el desencadenador de cola para iniciar una función cuando se reciba un nuevo elemento en una cola. El mensaje de la cola se proporciona a modo de entrada para la función.

# <a name="c"></a>[C#](#tab/csharp)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que sondea la cola `myqueue-items` y escribe un registro cada vez que se procesa un elemento de cola.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
}
```

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

En el ejemplo siguiente se muestra un enlace de desencadenador de cola de un archivo *function.json* y código de [script de C# (.csx)](functions-reference-csharp.md) que usa el enlace. La función sondea la cola `myqueue-items` y escribe un registro cada vez que se procesa un elemento de cola.

Este es el archivo *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

Este es el código de script de C#:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

En la sección acerca del [uso](#usage) se explica `myQueueItem`, que recibe el nombre de la propiedad `name` de function.json.  En la [sección de metadatos del mensaje](#message-metadata) se explican el resto de variables que se muestran.

# <a name="java"></a>[Java](#tab/java)

En el siguiente ejemplo de Java se muestra una función de desencadenador de cola de almacenamiento que registra el mensaje desencadenado que se pone en la cola `myqueuename`.

```java
@FunctionName("queueprocessor")
public void run(
    @QueueTrigger(name = "msg",
                queueName = "myqueuename",
                connection = "myconnvarname") String message,
    final ExecutionContext context
) {
    context.getLogger().info(message);
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

En el ejemplo siguiente se muestra un enlace de desencadenador de cola de un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función sondea la cola `myqueue-items` y escribe un registro cada vez que se procesa un elemento de cola.

Este es el archivo *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

> [!NOTE]
> El nombre del parámetro se muestra como `context.bindings.<name>` en el código JavaScript que contiene la carga del elemento de cola. Esta carga también se pasa como segundo parámetro a la función.

Este es el código de JavaScript:

```javascript
module.exports = async function (context, message) {
    context.log('Node.js queue trigger function processed work item', message);
    // OR access using context.bindings.<name>
    // context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

En la sección acerca del [uso](#usage) se explica `myQueueItem`, que recibe el nombre de la propiedad `name` de function.json.  En la [sección de metadatos del mensaje](#message-metadata) se explican el resto de variables que se muestran.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

En el ejemplo siguiente se muestra cómo leer un mensaje en cola pasado a una función a través de un desencadenador.

Un desencadenador de cola de almacenamiento se define en el archivo *function.json*, donde `type` se establece en `queueTrigger`.

```json
{
  "bindings": [
    {
      "name": "QueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "messages",
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

El código del archivo *Run.ps1* declara un parámetro como `$QueueItem`, que permite leer el mensaje de la cola en la función.

```powershell
# Input bindings are passed in via param block.
param([string] $QueueItem, $TriggerMetadata)

# Write out the queue message and metadata to the information log.
Write-Host "PowerShell queue trigger function processed work item: $QueueItem"
Write-Host "Queue item expiration time: $($TriggerMetadata.ExpirationTime)"
Write-Host "Queue item insertion time: $($TriggerMetadata.InsertionTime)"
Write-Host "Queue item next visible time: $($TriggerMetadata.NextVisibleTime)"
Write-Host "ID: $($TriggerMetadata.Id)"
Write-Host "Pop receipt: $($TriggerMetadata.PopReceipt)"
Write-Host "Dequeue count: $($TriggerMetadata.DequeueCount)"
```

# <a name="python"></a>[Python](#tab/python)

En el ejemplo siguiente se muestra cómo leer un mensaje en cola pasado a una función a través de un desencadenador.

Un desencadenador de cola de almacenamiento se define en *function.json*, donde *type* está establecido en `queueTrigger`.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "msg",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "messages",
      "connection": "AzureStorageQueuesConnectionString"
    }
  ]
}
```

El código *_\_init_\_.py* declara un parámetro como `func.QueueMessage`, lo que permite leer el mensaje de la cola en la función.

```python
import logging
import json

import azure.functions as func

def main(msg: func.QueueMessage):
    logging.info('Python queue trigger function processed a queue item.')

    result = json.dumps({
        'id': msg.id,
        'body': msg.get_body().decode('utf-8'),
        'expiration_time': (msg.expiration_time.isoformat()
                            if msg.expiration_time else None),
        'insertion_time': (msg.insertion_time.isoformat()
                           if msg.insertion_time else None),
        'time_next_visible': (msg.time_next_visible.isoformat()
                              if msg.time_next_visible else None),
        'pop_receipt': msg.pop_receipt,
        'dequeue_count': msg.dequeue_count
    })

    logging.info(result)
```

 ---

## <a name="attributes-and-annotations"></a>Atributos y anotaciones

# <a name="c"></a>[C#](#tab/csharp)

Para [bibliotecas de clases de C#](functions-dotnet-class-library.md), use los siguientes atributos para configurar un desencadenador de cola:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues/QueueTriggerAttribute.cs)

  El constructor del atributo toma el nombre de la cola que debe supervisar, tal como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      ILogger log)
  {
      ...
  }
  ```

  Puede establecer la propiedad `Connection` para especificar la configuración de aplicación que contiene la cadena de conexión de la cuenta de almacenamiento, tal como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      ILogger log)
  {
      ....
  }
  ```

  Para ver un ejemplo completo, consulte el [ejemplo](#example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Proporciona otra manera de especificar la cuenta de almacenamiento que se debe usar. El constructor toma el nombre de una configuración de aplicación que contiene una cadena de conexión de almacenamiento. El atributo se puede aplicar en el nivel de clase, método o parámetro. En el ejemplo siguiente se muestran el nivel de clase y de método:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

La cuenta de almacenamiento que se debe usar se determina en el orden siguiente:

* La propiedad `Connection` del atributo `QueueTrigger`.
* El atributo `StorageAccount` aplicado al mismo parámetro que el atributo `QueueTrigger`.
* El atributo `StorageAccount` aplicado a la función.
* El atributo `StorageAccount` aplicado a la clase.
* La configuración de aplicación "AzureWebJobsStorage".

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="java"></a>[Java](#tab/java)

La anotación `QueueTrigger` proporciona acceso a la cola que desencadena la función. En el ejemplo siguiente, el mensaje de la cola se hace disponible para la función mediante el parámetro `message`.

```java
package com.function;
import com.microsoft.azure.functions.annotation.*;
import java.util.Queue;
import com.microsoft.azure.functions.*;

public class QueueTriggerDemo {
    @FunctionName("QueueTriggerDemo")
    public void run(
        @QueueTrigger(name = "message", queueName = "messages", connection = "MyStorageConnectionAppSetting") String message,
        final ExecutionContext context
    ) {
        context.getLogger().info("Queue message: " + message);
    }
}
```

| Propiedad    | Descripción |
|-------------|-----------------------------|
|`name`       | Declara el nombre del parámetro en la firma de la función. Cuando se desencadena la función, el valor de este parámetro tiene el contenido del mensaje de la cola. |
|`queueName`  | Declara el nombre de la cola en la cuenta de almacenamiento. |
|`connection` | Apunta a la cadena de conexión de la cuenta de almacenamiento. |

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell no admite atributos.

# <a name="python"></a>[Python](#tab/python)

Python no admite atributos.

---

## <a name="configuration"></a>Configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `QueueTrigger`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D| Se debe establecer en `queueTrigger`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal.|
|**direction**| N/D | Solo en el archivo *function.json*. Se debe establecer en `in`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal. |
|**name** | N/D |El nombre de la variable que contiene la carga del elemento de cola en el código de función.  |
|**queueName** | **QueueName**| Nombre de la cola que se sondea. |
|**connection** | **Connection** |Nombre de una configuración de aplicación o colección de configuraciones que especifica cómo conectarse a las colas de Azure. Consulte [Conexiones](#connections).|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

[!INCLUDE [functions-storage-queue-connections](../../includes/functions-storage-queue-connections.md)]

## <a name="usage"></a>Uso

# <a name="c"></a>[C#](#tab/csharp)

### <a name="default"></a>Valor predeterminado

Acceda a los datos de mensaje mediante un parámetro de método, como `string paramName`. Puede enlazar a cualquiera de los siguientes tipos:

* Objeto: el entorno de tiempo de ejecución de Functions deserializa una carga JSON en una instancia de una clase arbitraria definida en el código.
* `string`
* `byte[]`
* [CloudQueueMessage]

Si intenta enlazar a `CloudQueueMessage` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x).

### <a name="additional-types"></a>Tipos adicionales

Las aplicaciones que usan la [versión 5.0.0 o posterior de la extensión de Azure Storage](./functions-bindings-storage-queue.md#storage-extension-5x-and-higher) también pueden usar tipos de [Azure SDK para .NET](/dotnet/api/overview/azure/storage.queues-readme). En esta versión se elimina la compatibilidad con el tipo `CloudQueueMessage` heredado en favor de los siguientes tipos:

- [QueueMessage](/dotnet/api/azure.storage.queues.models.queuemessage)

Para obtener ejemplos de uso de estos tipos, consulte el [repositorio de GitHub de la extensión](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Microsoft.Azure.WebJobs.Extensions.Storage.Queues#examples).

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

### <a name="default"></a>Valor predeterminado

Acceda a los datos de mensaje mediante un parámetro de método, como `string paramName`. `paramName` es el valor especificado en la propiedad `name` de *function.json*. Puede enlazar a cualquiera de los siguientes tipos:

* Objeto: el entorno de tiempo de ejecución de Functions deserializa una carga JSON en una instancia de una clase arbitraria definida en el código.
* `string`
* `byte[]`
* [CloudQueueMessage]

Si intenta enlazar a `CloudQueueMessage` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](functions-bindings-storage-queue.md#azure-storage-sdk-version-in-functions-1x).

### <a name="additional-types"></a>Tipos adicionales

Las aplicaciones que usan la [versión 5.0.0 o posterior de la extensión de Azure Storage](./functions-bindings-storage-queue.md#storage-extension-5x-and-higher) también pueden usar tipos de [Azure SDK para .NET](/dotnet/api/overview/azure/storage.queues-readme). En esta versión se elimina la compatibilidad con el tipo `CloudQueueMessage` heredado en favor de los siguientes tipos:

- [QueueMessage](/dotnet/api/azure.storage.queues.models.queuemessage)

Para obtener ejemplos de uso de estos tipos, consulte el [repositorio de GitHub de la extensión](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Microsoft.Azure.WebJobs.Extensions.Storage.Queues#examples).

# <a name="java"></a>[Java](#tab/java)

La anotación [QueueTrigger](/java/api/com.microsoft.azure.functions.annotation.queuetrigger) proporciona acceso al mensaje de la cola que desencadenó la función.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

La carga del elemento de cola está disponible mediante `context.bindings.<NAME>`, donde `<NAME>` coincide con el nombre definido en *function.json*. Si la carga es JSON, el valor se deserializa en un objeto.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Acceda al mensaje de la cola a través de un parámetro de cadena que coincida con el nombre designado por el parámetro `name` del enlace en el archivo *function.json*.

# <a name="python"></a>[Python](#tab/python)

Puede acceder al mensaje de la cola mediante un parámetro de tipo [QueueMessage](/python/api/azure-functions/azure.functions.queuemessage).

---

## <a name="message-metadata"></a>Metadatos del mensaje

El desencadenador de cola proporciona varias [propiedades de metadatos](./functions-bindings-expressions-patterns.md#trigger-metadata). Estas propiedades pueden usarse como parte de expresiones de enlace en otros enlaces o como parámetros del código. Las propiedades forman parte de la clase [CloudQueueMessage](/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage).

|Propiedad|Tipo|Descripción|
|--------|----|-----------|
|`QueueTrigger`|`string`|Carga de cola (si hay una cadena válida). Si la carga del mensaje de la cola es una cadena, `QueueTrigger` tiene el mismo valor que la variable denominada por la propiedad `name` en *function.json*.|
|`DequeueCount`|`int`|Es el número de veces que se ha quitado de la cola este mensaje.|
|`ExpirationTime`|`DateTimeOffset`|Es la hora de expiración del mensaje.|
|`Id`|`string`|Es el identificador del mensaje de cola.|
|`InsertionTime`|`DateTimeOffset`|Es la hora en la que el mensaje se agregó a la cola.|
|`NextVisibleTime`|`DateTimeOffset`|Es la siguiente hora a la que será visible el mensaje.|
|`PopReceipt`|`string`|Es la confirmación de extracción del mensaje.|

## <a name="poison-messages"></a>Mensajes dudosos

Si se produce un error en una función del desencadenador de cola, Azure Functions volverá a intentar esa función hasta cinco veces para un determinado mensaje en la cola, incluido el primer intento. Si los cinco intentos son infructuosos, Functions agregará un mensaje a la cola denominada *&lt;originalqueuename&gt;-poison*. Puede escribir una función para procesar los mensajes desde la cola de mensajes dudosos registrándolos o enviando una notificación indicando que se necesita atención manual.

Para controlar manualmente los mensajes dudosos, compruebe el elemento [dequeueCount](#message-metadata) del mensaje de la cola.


## <a name="peek-lock"></a>Inspección y bloqueo
El patrón de inspección y bloqueo se produce automáticamente en los desencadenadores de cola. A medida que los mensajes se van quitando de la cola, se marcan como invisibles y se asocian a un tiempo de espera administrado por el servicio de Storage.

Cuando la función se inicia, comienza a procesar un mensaje bajo las siguientes condiciones.

- Si la función es correcta, termina de ejecutarse y el mensaje se elimina.
- Si la función no es correcta, se restablece la visibilidad del mensaje y, tras ello, el mensaje vuelve a procesarse la próxima vez que la función solicite un nuevo mensaje.
- Si la función nunca se completa debido a un bloqueo, la visibilidad del mensaje expira y el mensaje vuelve a aparecer en la cola.

Todas las mecánicas de visibilidad se controlan mediante el servicio de Storage, no el runtime de Functions.

## <a name="polling-algorithm"></a>Algoritmo de sondeo

El desencadenador de cola implementa un algoritmo de interrupción exponencial aleatorio para reducir el efecto del sondeo de cola inactiva en los costos de transacción de almacenamiento.

El algoritmo utiliza la siguiente lógica:

- Cuando se encuentra un mensaje, el runtime espera 100 milisegundos y, después, busca otro mensaje.
- Si no encuentra ninguno, espera unos 200 milisegundos antes de volver a intentarlo.
- Después de varios intentos fallidos para obtener un mensaje de la cola, el tiempo de espera sigue aumentando hasta que alcanza el tiempo de espera máximo, predeterminado en un minuto.
- El tiempo de espera máximo se configura mediante la propiedad `maxPollingInterval` en el [archivo host.json](functions-host-json-v1.md#queues).

En el desarrollo local, el intervalo máximo de sondeo predeterminado es de dos segundos.

> [!NOTE]
> Respecto a la facturación al hospedar aplicaciones de función en el plan de consumo, no se le cobrará por el tiempo empleado en el sondeo del runtime.

## <a name="concurrency"></a>Simultaneidad

Cuando hay varios mensajes en cola en espera, el desencadenador de cola recupera un lote de mensajes e invoca instancias de función de manera simultánea para procesarlas. De manera predeterminada, el tamaño de lote es 16. Cuando el número que se está procesando llega a 8, el entorno en tiempo de ejecución obtiene otro lote y empieza a procesar esos mensajes. Por lo tanto, el número máximo de mensajes simultáneos que se procesan por función en una máquina virtual es 24. Este límite se aplica por separado a cada función desencadenada por la cola en cada máquina virtual. Si la aplicación de función se escala horizontalmente a varias máquinas virtuales, cada máquina virtual esperará los desencadenadores e intentará ejecutar las funciones. Por ejemplo, si una aplicación de función se escala horizontalmente a 3 máquinas virtuales, el número máximo predeterminado de instancias simultáneas de una función desencadenada por la cola es 72.

El tamaño de lote y el umbral para obtener un lote nuevo se configuran en el [archivo host.json](functions-host-json.md#queues). Si quiere minimizar la ejecución en paralelo para las funciones desencadenadas por la cola en una aplicación de función, puede establecer el tamaño de lote en 1. Este valor solo elimina la simultaneidad siempre y cuando la aplicación de función se ejecute en una única máquina virtual (VM).

El desencadenador de cola impide automáticamente que una función procese un mensaje de cola varias veces al mismo tiempo.

## <a name="hostjson-properties"></a>Propiedades de host.json

El archivo [host.json](functions-host-json.md#queues) contiene opciones de configuración que controlan el comportamiento de desencadenador de cola. Consulte la sección de [configuración de host.json](functions-bindings-storage-queue.md#hostjson-settings) para más información sobre las opciones de configuración disponibles.

## <a name="next-steps"></a>Pasos siguientes

- [Escritura de mensajes de Queue Storage (enlace de salida)](./functions-bindings-storage-queue-output.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage
