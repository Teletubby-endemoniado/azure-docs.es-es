---
title: Enlace de entrada de Azure Blob Storage para Azure Functions
description: Obtenga información sobre cómo proporcionar datos de enlace de entrada de Azure Blob Storage a una instancia de Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 24db395c33f50fc7638ee00e9406db3709db5366
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "130004015"
---
# <a name="azure-blob-storage-input-binding-for-azure-functions"></a>Enlace de entrada de Azure Blob Storage para Azure Functions

El enlace de entrada permite leer datos de Blob Storage como entrada para una instancia de Azure Functions.

Para obtener información sobre los detalles de instalación y configuración, consulte [Introducción](./functions-bindings-storage-blob.md).

## <a name="example"></a>Ejemplo

# <a name="c"></a>[C#](#tab/csharp)

El ejemplo siguiente es una [función de C#](functions-dotnet-class-library.md) que utiliza un desencadenador de cola y un enlaces de blobs de entrada. El mensaje de cola contiene el nombre del blob y la función registra el tamaño del blob.

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

<!--Same example for input and output. -->

En el ejemplo siguiente se muestran enlaces de entrada y salida de blobs en un archivo *function.json* y código de [script de C# (.csx)](functions-reference-csharp.md) que usa los enlaces. La función realiza una copia de un blob de texto. La función se activa mediante un mensaje de cola que contiene el nombre del blob que se va a copiar. El nuevo blob se denomina *{nombreoriginaldelblob}-Copy*.

En el archivo *function.json*, la propiedad de metadatos `queueTrigger` se utiliza para especificar el nombre del blob en las propiedades de `path`:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

Este es el código de script de C#:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

# <a name="java"></a>[Java](#tab/java)

En esta sección se incluyen los ejemplos siguientes:

* [Desencadenador de HTTP, búsqueda de nombre de blob en la cadena de consulta](#http-trigger-look-up-blob-name-from-query-string)
* [Desencadenador de cola, recepción del nombre del blob de la cola de mensajes](#queue-trigger-receive-blob-name-from-queue-message)

#### <a name="http-trigger-look-up-blob-name-from-query-string"></a>Desencadenador de HTTP, búsqueda de nombre de blob en la cadena de consulta

 El ejemplo siguiente muestra una función de Java que usa la anotación `HttpTrigger` para recibir un parámetro que contiene el nombre de un archivo en un contenedor de Blob Storage. Posteriormente, la anotación `BlobInput` lee el archivo y pasa el contenido a la función como `byte[]`.

```java
  @FunctionName("getBlobSizeHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage blobSize(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    final ExecutionContext context) {
      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-receive-blob-name-from-queue-message"></a>Desencadenador de cola, recepción del nombre del blob de la cola de mensajes

 El ejemplo siguiente muestra una función de Java que usa la anotación `QueueTrigger` para recibir un mensaje que contiene el nombre de un archivo en un contenedor de Blob Storage. Posteriormente, la anotación `BlobInput` lee el archivo y pasa el contenido a la función como `byte[]`.

```java
  @FunctionName("getBlobSize")
  @StorageAccount("Storage_Account_Connection_String")
  public void blobSize(
    @QueueTrigger(
      name = "filename", 
      queueName = "myqueue-items-sample") 
    String filename,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{queueTrigger}") 
    byte[] content,
    final ExecutionContext context) {
      context.getLogger().info("The size of \"" + filename + "\" is: " + content.length + " bytes");
  }
```

En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@BlobInput` en los parámetros cuyo valor provendría de un blob.  Esta anotación se puede usar con tipos nativos de Java, POJO o valores que aceptan valores NULL mediante `Optional<T>`.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

<!--Same example for input and output. -->

En el ejemplo siguiente se muestran enlaces de entrada y salida de blobs en un archivo *function.json* y código de [script de JavaScript](functions-reference-node.md) que usa los enlaces. La función realiza una copia de un blob. La función se activa mediante un mensaje de cola que contiene el nombre del blob que se va a copiar. El nuevo blob se denomina *{nombreoriginaldelblob}-Copy*.

En el archivo *function.json*, la propiedad de metadatos `queueTrigger` se utiliza para especificar el nombre del blob en las propiedades de `path`:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

En el ejemplo siguiente se muestra un enlace de entrada de blob, definido en el archivo _function.json_, que hace que los datos de blobs entrantes estén disponibles para la función de [PowerShell](functions-reference-powershell.md).

Esta es la configuración json:

```json
{
  "bindings": [
    {
      "name": "InputBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "source/{name}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

Este es el código de la función:

```powershell
# Input bindings are passed in via param block.
param([byte[]] $InputBlob, $TriggerMetadata)

Write-Host "PowerShell Blob trigger: Name: $($TriggerMetadata.Name) Size: $($InputBlob.Length) bytes"
```

# <a name="python"></a>[Python](#tab/python)

<!--Same example for input and output. -->

En el ejemplo siguiente se muestran enlaces de entrada y salida de blobs en un archivo *function.json* y el [código de Python](functions-reference-python.md) que usa los enlaces. La función realiza una copia de un blob. La función se activa mediante un mensaje de cola que contiene el nombre del blob que se va a copiar. El nuevo blob se denomina *{nombreoriginaldelblob}-Copy*.

En el archivo *function.json*, la propiedad de metadatos `queueTrigger` se utiliza para especificar el nombre del blob en las propiedades de `path`:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "queuemsg",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "inputblob",
      "type": "blob",
      "dataType": "binary",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

La propiedad `dataType` determina qué enlace se usa. Los siguientes valores están disponibles para admitir diferentes estrategias de enlace:

| Valor de enlace | Valor predeterminado | Descripción | Ejemplo |
| --- | --- | --- | --- |
| `string` | N | Usa enlaces genéricos y convierte el tipo de entrada en `string`. | `def main(input: str)` |
| `binary` | N | Usa enlaces genéricos y convierte el blob de entrada en el objeto `bytes` de Python. | `def main(input: bytes)` |

Si la propiedad `dataType` no está definida en function.json, el valor predeterminado es `string`.

Este es el código de Python:

```python
import logging
import azure.functions as func


# The input binding field inputblob can either be 'bytes' or 'str' depends
# on dataType in function.json, 'binary' or 'string'.
def main(queuemsg: func.QueueMessage, inputblob: bytes) -> bytes:
    logging.info(f'Python Queue trigger function processed {len(inputblob)} bytes')
    return inputblob
```

---

## <a name="attributes-and-annotations"></a>Atributos y anotaciones

# <a name="c"></a>[C#](#tab/csharp)

En las [bibliotecas de clase C#](functions-dotnet-class-library.md), use [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs).

El constructor del atributo toma la ruta de acceso al blob y un parámetro `FileAccess` que indica lectura o escritura, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}

```

Puede establecer la propiedad `Connection` para especificar la cuenta de almacenamiento que se usará, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read, Connection = "StorageConnectionAppSetting")] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

Puede usar el atributo `StorageAccount` para especificar la cuenta de almacenamiento en el nivel de clase, método o parámetro. Para más información, consulte [Desencadenador: atributos y anotaciones](./functions-bindings-storage-blob-trigger.md#attributes-and-annotations).

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobInput` da acceso al blob que desencadenó la función. Si utiliza una matriz de bytes con el atributo, establezca `dataType` en `binary`. Vea el [ejemplo de entrada](#example) para más información.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell no admite atributos.

# <a name="python"></a>[Python](#tab/python)

Python no admite atributos.

---

## <a name="configuration"></a>Configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `Blob`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `blob`. |
|**direction** | N/D | Se debe establecer en `in`. Las excepciones se indican en la sección [uso](#usage). |
|**name** | N/D | Nombre de la variable que representa el blob en el código de la función.|
|**path** |**BlobPath** | Ruta de acceso al blob. |
|**connection** |**Connection**| Nombre de una configuración de aplicación o de una colección de configuraciones de aplicación que especifica cómo conectarse a los blobs de Azure. Consulte [Conexiones](#connections).|
|**dataType**| N/D | En el caso de los lenguajes con tipos dinámicos, especifica el tipo de datos subyacente. Los valores posibles son `string`, `binary` o `stream`. Para obtener más información, consulte los [conceptos de desencadenadores y enlaces](functions-triggers-bindings.md?tabs=python#trigger-and-binding-definitions). |
|N/D | **Acceder** | Indica si va a leer o escribir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

[!INCLUDE [functions-storage-blob-connections](../../includes/functions-storage-blob-connections.md)]

## <a name="usage"></a>Uso

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-input-usage.md](../../includes/functions-bindings-blob-storage-input-usage.md)]

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-input-usage.md](../../includes/functions-bindings-blob-storage-input-usage.md)]

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobInput` da acceso al blob que desencadenó la función. Si utiliza una matriz de bytes con el atributo, establezca `dataType` en `binary`. Vea el [ejemplo de entrada](#example) para más información.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Acceda a los datos de BLOB mediante `context.bindings.<NAME>` donde `<NAME>` coincide con el valor definido en *function.json*.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Acceda a los datos de blob a través de un parámetro que coincida con el nombre designado por el parámetro del nombre del enlace en el archivo _function.json_.

# <a name="python"></a>[Python](#tab/python)

Acceda a los datos de blob a través del parámetro con el tipo [InputStream](/python/api/azure-functions/azure.functions.inputstream). Vea el [ejemplo de entrada](#example) para más información.

---

## <a name="next-steps"></a>Pasos siguientes

- [Ejecución de una función cuando cambian los datos de Blob Storage](./functions-bindings-storage-blob-trigger.md)
- [Escritura de datos de Blob Storage desde una función](./functions-bindings-storage-blob-output.md)
