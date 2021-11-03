---
title: Desencadenador de Azure Blob Storage para Azure Functions
description: Aprenda a ejecutar una instancia de Azure Functions a medida que cambian los datos de Azure Blob Storage.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 58167006a1925ee1110f9d7468a66d580545f336
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131039304"
---
# <a name="azure-blob-storage-trigger-for-azure-functions"></a>Desencadenador de Azure Blob Storage para Azure Functions

El desencadenador de Blob Storage inicia una función cuando se detecta un blob nuevo o actualizado. El contenido del blob se proporciona a modo de [entrada para la función](./functions-bindings-storage-blob-input.md).

El desencadenador de Azure Blob Storage necesita una cuenta de almacenamiento de uso general. También se admiten las cuentas de almacenamiento v2 con [espacios de nombres jerárquicos](../storage/blobs/data-lake-storage-namespace.md). Para usar una cuenta solo para blobs, o si la aplicación tiene necesidades especializadas, revise las alternativas al uso de este desencadenador.

Para obtener información sobre los detalles de instalación y configuración, consulte la [información general](./functions-bindings-storage-blob.md).

## <a name="polling"></a>Sondeo

El sondeo funciona como un híbrido entre la inspección de registros y la ejecución de exámenes periódicos de contenedores. Los blobs se examinan en grupos de 10 000 a la vez con un token de continuación que se usa entre intervalos.

> [!WARNING]
> Además, [se crean registros de almacenamiento basados en el principio del "mejor esfuerzo"](/rest/api/storageservices/About-Storage-Analytics-Logging). No hay ninguna garantía de que se capturen todos los eventos. En algunos casos, podrían faltar registros.
> 
> Si necesita un procesamiento de blobs más rápido o confiable, considere crear un [mensaje de cola](../storage/queues/storage-dotnet-how-to-use-queues.md) al crear el blob. A continuación, use un [desencadenador de cola](functions-bindings-storage-queue.md), en lugar de un desencadenador de blob, para procesar el blob. Otra opción consiste en usar Event Grid; consulte el tutorial [Automatizar el cambio de tamaño de imágenes cargadas mediante Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md).
>

## <a name="alternatives"></a>Alternativas

### <a name="event-grid-trigger"></a>Desencadenador de Event Grid

> [!NOTE]
> Cuando se usa la extensión de Storage 5.x y superior, el desencadenador de blobs tiene compatibilidad integrada para un desencadenador de blobs basado en Event Grid. Para obtener más información, consulte la sección [Extensión de Storage 5.x y superior](#storage-extension-5x-and-higher) a continuación.

El [desencadenador de Event Grid](functions-bindings-event-grid.md) también tiene compatibilidad integrada con [eventos de blob](../storage/blobs/storage-blob-event-overview.md). Utilice Event Grid en lugar del desencadenador de Blob Storage en los escenarios siguientes:

- **Cuentas de almacenamiento solo para blobs**: [Las cuentas de almacenamiento solo para blobs](../storage/common/storage-account-overview.md#types-of-storage-accounts) se admiten para enlaces de entrada y salida de blobs pero no para desencadenadores de blobs.

- **Gran escala**: Gran escala se aplica generalmente a contenedores que tienen más de 100 000 blobs en ellos o a cuentas de almacenamiento que tienen más de 100 actualizaciones de blob por segundo.

- **Blobs existentes**: el desencadenador de blobs procesará todos los blobs existentes en el contenedor al configurar el desencadenador. Si tiene un contenedor con muchos blobs existentes y solo desea que se desencadene para los nuevos blobs, use el desencadenador de Event Grid.

- **Minimización de latencia**: Si la aplicación de función está en un plan de consumo, puede haber un retraso de hasta 10 minutos en el procesamiento de nuevos blobs si una aplicación de función ha quedado inactiva. Para evitar esta latencia, puede cambiar a un plan de App Service con Always On habilitado. También puede usar un [desencadenador de Event Grid](functions-bindings-event-grid.md) con su cuenta de almacenamiento de blobs. Para ver un ejemplo, consulte el [tutorial de Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2Fazure%2Fazure-functions%2Ftoc.json).

Consulte el tutorial [Cambio de tamaño de una imagen con Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md) para ver un ejemplo de Event Grid.

#### <a name="storage-extension-5x-and-higher"></a>Extensión de Storage 5.x y superior

Al usar la extensión de Storage en versión preliminar, hay compatibilidad integrada con Event Grid en el desencadenador de blobs, que requiere establecer el parámetro `source` en Event Grid en el desencadenador de blobs existente. 

Para más información sobre cómo usar el desencadenador de blobs basado en Event Grid, consulte la [guía del desencadenador de blobs de Event Grid](./functions-event-grid-blob-trigger.md).

### <a name="queue-storage-trigger"></a>Desencadenador de Queue Storage

Otro enfoque para el procesamiento de blobs es escribir mensajes de cola que correspondan a los blobs que se están creando o modificando y, a continuación, usar un [desencadenador de Queue Storage](./functions-bindings-storage-queue.md) para comenzar el procesamiento.

## <a name="example"></a>Ejemplo

# <a name="c"></a>[C#](#tab/csharp)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que escribe un registro cuando se agrega o se actualiza un blob en el contenedor `samples-workitems`.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

La cadena `{name}` en la ruta de acceso del desencadenador de blobs `samples-workitems/{name}` crea una [expresión de enlace](./functions-bindings-expressions-patterns.md) que puede usar en el código de función para acceder al nombre de archivo del blob de desencadenamiento. Para más información, consulte [Patrones de nombre de blob](#blob-name-patterns) que aparece más adelante en este artículo.

Para obtener más información sobre el atributo `BlobTrigger`, consulte [atributos y anotaciones](#attributes-and-annotations).

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

En el siguiente ejemplo se muestra un enlace de desencadenador de blob en un archivo *function.json* y el código que usa el enlace. La función escribe un registro cuando se agrega o actualiza un blob en el [contenedor](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources) `samples-workitems`.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

La cadena `{name}` en la ruta de acceso del desencadenador de blobs `samples-workitems/{name}` crea una [expresión de enlace](./functions-bindings-expressions-patterns.md) que puede usar en el código de función para acceder al nombre de archivo del blob de desencadenamiento. Para más información, consulte [Patrones de nombre de blob](#blob-name-patterns) que aparece más adelante en este artículo.

Para más información sobre las propiedades del archivo *function.json*, consulte la sección [Configuración](#configuration) donde se explican estas propiedades.

Este es el código de script de C# que se enlaza a `Stream`:

```cs
public static void Run(Stream myBlob, string name, ILogger log)
{
   log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Este es el código de script de C# que se enlaza a `CloudBlockBlob`:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

# <a name="java"></a>[Java](#tab/java)

Esta función escribe un registro cuando se agrega o actualiza un blob en el contenedor `myblob`.

```java
@FunctionName("blobprocessor")
public void run(
  @BlobTrigger(name = "file",
               dataType = "binary",
               path = "myblob/{name}",
               connection = "MyStorageAccountAppSetting") byte[] content,
  @BindingName("name") String filename,
  final ExecutionContext context
) {
  context.getLogger().info("Name: " + filename + " Size: " + content.length + " bytes");
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

En el ejemplo siguiente se muestra un enlace de desencadenador de blob en un archivo *function.json* y el [código de JavaScript](functions-reference-node.md) que usa el enlace. La función escribe un registro cuando se agrega o actualiza un blob en el contenedor `samples-workitems`.

Este es el archivo *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

La cadena `{name}` en la ruta de acceso del desencadenador de blobs `samples-workitems/{name}` crea una [expresión de enlace](./functions-bindings-expressions-patterns.md) que puede usar en el código de función para acceder al nombre de archivo del blob de desencadenamiento. Para más información, consulte [Patrones de nombre de blob](#blob-name-patterns) que aparece más adelante en este artículo.

Para más información sobre las propiedades del archivo *function.json*, consulte la sección [Configuración](#configuration) donde se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

En el ejemplo siguiente se muestra cómo crear una función que se ejecuta cuando se agrega un archivo al contenedor de almacenamiento de blobs `source`.

El archivo de configuración de la función (_function.json_) incluye un enlace con `type` de `blobTrigger` y `direction` está establecido en `in`.

```json
{
  "bindings": [
    {
      "name": "InputBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "source/{name}",
      "connection": "MyStorageAccountConnectionString"
    }
  ]
}
```

Este es el código asociado del archivo _run.ps1_.

```powershell
param([byte[]] $InputBlob, $TriggerMetadata)

Write-Host "PowerShell Blob trigger: Name: $($TriggerMetadata.Name) Size: $($InputBlob.Length) bytes"
```

# <a name="python"></a>[Python](#tab/python)

En el ejemplo siguiente se muestra un enlace de desencadenador de blobs en un archivo *function.json* y el [código de Python](functions-reference-python.md) que usa el enlace. La función escribe un registro cuando se agrega o actualiza un blob en el [contenedor](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources) `samples-workitems`.

Este es el archivo *function.json*:

```json
{
    "scriptFile": "__init__.py",
    "disabled": false,
    "bindings": [
        {
            "name": "myblob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

La cadena `{name}` en la ruta de acceso del desencadenador de blobs `samples-workitems/{name}` crea una [expresión de enlace](./functions-bindings-expressions-patterns.md) que puede usar en el código de función para acceder al nombre de archivo del blob de desencadenamiento. Para más información, consulte [Patrones de nombre de blob](#blob-name-patterns) que aparece más adelante en este artículo.

Para más información sobre las propiedades del archivo *function.json*, consulte la sección [Configuración](#configuration) donde se explican estas propiedades.

Este es el código de Python:

```python
import logging
import azure.functions as func


def main(myblob: func.InputStream):
    logging.info('Python Blob trigger function processed %s', myblob.name)
```

---

## <a name="attributes-and-annotations"></a>Atributos y anotaciones

# <a name="c"></a>[C#](#tab/csharp)

Para [bibliotecas de clases de C#](functions-dotnet-class-library.md), utilice los siguientes atributos para configurar un desencadenador de blob:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobTriggerAttribute.cs)

  El constructor del atributo toma una cadena de ruta de acceso que indica al contenedor que inspeccione y, opcionalmente, un [patrón de nombre de blob](#blob-name-patterns). Este es un ejemplo:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  Puede establecer la propiedad `Connection` para especificar la cuenta de almacenamiento que se usará, tal como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  Para obtener un ejemplo completo, vea el [ejemplo de desencadenador](#example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Proporciona otra manera de especificar la cuenta de almacenamiento que se debe usar. El constructor toma el nombre de una configuración de aplicación que contiene una cadena de conexión de almacenamiento. El atributo se puede aplicar en el nivel de clase, método o parámetro. En el ejemplo siguiente se muestran el nivel de clase y de método:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("BlobTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ....
  }
  ```

La cuenta de almacenamiento que se debe usar se determina en el orden siguiente:

* La propiedad `Connection` del atributo `BlobTrigger`.
* El atributo `StorageAccount` aplicado al mismo parámetro que el atributo `BlobTrigger`.
* El atributo `StorageAccount` aplicado a la función.
* El atributo `StorageAccount` aplicado a la clase.
* La cuenta de almacenamiento predeterminada de la aplicación de función (configuración de aplicación "AzureWebJobsStorage").

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobTrigger` se usa para facilitar el acceso al blob que desencadenó la función. Vea el [ejemplo de desencadenador](#example) para más información.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

PowerShell no admite atributos.

# <a name="python"></a>[Python](#tab/python)

Python no admite atributos.

---

## <a name="configuration"></a>Configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `BlobTrigger`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `blobTrigger`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal.|
|**direction** | N/D | Se debe establecer en `in`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal. Las excepciones se indican en la sección [uso](#usage). |
|**name** | N/D | Nombre de la variable que representa el blob en el código de la función. |
|**path** | **BlobPath** |El [contenedor](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources) que se va a supervisar.  Puede ser un [patrón de nombre de blob](#blob-name-patterns). |
|**connection** | **Connection** | Nombre de una configuración de aplicación o de una colección de configuraciones de aplicación que especifica cómo conectarse a los blobs de Azure. Consulte [Conexiones](#connections).|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

[!INCLUDE [functions-storage-blob-connections](../../includes/functions-storage-blob-connections.md)]

## <a name="usage"></a>Uso

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-trigger.md)]

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-trigger.md)]

# <a name="java"></a>[Java](#tab/java)

El atributo `@BlobTrigger` se usa para facilitar el acceso al blob que desencadenó la función. Vea el [ejemplo de desencadenador](#example) para más información.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Acceda a los datos de BLOB mediante `context.bindings.<NAME>` donde `<NAME>` coincide con el valor definido en *function.json*.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Acceda a los datos de blob a través de un parámetro que coincida con el nombre designado por el parámetro del nombre del enlace en el archivo _function.json_.

# <a name="python"></a>[Python](#tab/python)

Acceda a los datos de blob a través del parámetro con el tipo [InputStream](/python/api/azure-functions/azure.functions.inputstream). Vea el [ejemplo de desencadenador](#example) para más información.

---

## <a name="blob-name-patterns"></a>Patrones de nombre de blobs

Puede especificar un patrón de nombre de blob en la propiedad `path` en *function.json* o en el constructor de atributos `BlobTrigger`. El patrón de nombre puede ser una [expresión de filtro o enlace](./functions-bindings-expressions-patterns.md). En las siguientes secciones se proporcionan ejemplos.

> [!TIP]
> Un nombre de contenedor no puede contener una resolución en el patrón de nombre.

### <a name="get-file-name-and-extension"></a>Obtener el nombre y la extensión de archivo

En el ejemplo siguiente se muestra cómo enlazar a nombre de archivo del blob y la extensión por separado:

```json
"path": "input/{blobname}.{blobextension}",
```

Si el blob se denomina *original-Blob1.txt*, los valores de las variables `blobname` y `blobextension` del código de la función son *original-Blob1* y *txt*.

### <a name="filter-on-blob-name"></a>Filtrar por nombre de blob

El ejemplo siguiente se desencadena solo en blobs del contenedor `input` que comienzan con la cadena "original-":

```json
"path": "input/original-{name}",
```

Si el nombre de blob es *original Blob1.txt*, el valor de la variable `name` en el código de la función es `Blob1.txt`.

### <a name="filter-on-file-type"></a>Filtrar por tipo de archivo

El siguiente ejemplo se desencadena solo en archivos *.png*:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Filtrar por llaves en nombres de archivo

Para buscar llaves en nombres de archivo, escape las llaves mediante dos llaves. En el ejemplo siguiente se filtran blobs que tienen llaves en el nombre:

```json
"path": "images/{{20140101}}-{name}",
```

Si el blob se denomina *{20140101}-soundfile.mp3*, el valor de variable `name` en el código de la función es *soundfile.mp3*.

## <a name="metadata"></a>Metadatos

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-metadata.md)]

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-trigger](../../includes/functions-bindings-blob-storage-metadata.md)]

# <a name="java"></a>[Java](#tab/java)

En Java no hay metadatos disponibles.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
module.exports = function (context, myBlob) {
    context.log("Full blob path:", context.bindingData.blobTrigger);
    context.done();
};
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Los metadatos están disponibles mediante el parámetro `$TriggerMetadata`.

# <a name="python"></a>[Python](#tab/python)

En Python no hay metadatos disponibles.

---

## <a name="blob-receipts"></a>Recepciones de blobs

El entorno en tiempo de ejecución de Azure Functions garantiza que no se llame más de una vez a ninguna función de desencadenador de blobs para un mismo blob, ya sea nuevo o actualizado. Mantiene *recepciones de blobs* para determinar si se ha procesado una determinada versión de blob.

Azure Functions almacena confirmaciones de blobs en un contenedor llamado *azure-webjobs-hosts* en la cuenta de almacenamiento de Azure de la aplicación de función (que se especifica mediante la configuración de la aplicación `AzureWebJobsStorage`). Una recepción de blobs tiene la información siguiente:

* La función desencadenada (`<FUNCTION_APP_NAME>.Functions.<FUNCTION_NAME>`, por ejemplo,`MyFunctionApp.Functions.CopyBlob`)
* El nombre del contenedor
* El tipo de blob (`BlockBlob` o `PageBlob`)
* El nombre del blob
* La etiqueta de entidad (un identificador de la versión del blob, por ejemplo, `0x8D1DC6E70A277EF`)

Si desea forzar el reprocesamiento de un blob, puede eliminar manualmente la recepción de ese blob desde el contenedor *azure-webjobs-hosts* . Al volver a procesar podría no producirse inmediatamente, pero se garantiza que se producirá más adelante en un momento dado. Para volver a procesarlo de inmediato, se puede actualizar el blob *scaninfo* en *azure-webjobs-hosts/blobscaninfo*. Cualquier blob con una marca de tiempo de última modificación después de la propiedad `LatestScan` se volverá a examinar.

## <a name="poison-blobs"></a>Blobs dudosos

Si se produce un error en una función de desencadenador de blob, Azure Functions vuelve a intentar ejecutar esa función hasta 5 veces de forma predeterminada.

Si se produce un error en los 5 intentos, Azure Functions agregará un mensaje a una cola de Storage llamada *webjobs-blobtrigger-poison*. Es posible configurar el número máximo de reintentos. Se usa la misma configuración de MaxDequeueCount para controlar los blobs dudosos y los mensajes de cola dudosos. El mensaje de cola para los blobs dudosos es un objeto JSON que contiene las siguientes propiedades:

* FunctionId (con formato `<FUNCTION_APP_NAME>.Functions.<FUNCTION_NAME>`)
* BlobType (`BlockBlob` o `PageBlob`)
* ContainerName
* BlobName
* ETag (un identificador de la versión del blob, por ejemplo, `0x8D1DC6E70A277EF`)

## <a name="concurrency-and-memory-usage"></a>Simultaneidad y uso de memoria

El desencadenador de blob utiliza una cola internamente, por lo que el número máximo de invocaciones de funciones simultáneas lo controla la [configuración de colas en host.json](functions-host-json.md#queues). La configuración predeterminada limita la simultaneidad a 24 invocaciones. Este límite se aplica por separado a cada función que usa un desencadenador de blob.

> [!NOTE]
> En el caso de las aplicaciones que usan la [versión 5.0.0 o posterior de la extensión de Storage](functions-bindings-storage-blob.md#storage-extension-5x-and-higher), la configuración de la cola de host.json solo se aplica a los desencadenadores de la cola. La simultaneidad del desencadenador de blob se controla en la [configuración de blobs en host.json](functions-host-json.md#blobs).

[El plan de consumo](event-driven-scaling.md) limita una aplicación de función de una máquina virtual (VM) a 1,5 GB de memoria. Tanto las instancias de función que se ejecutan de forma simultánea como el entorno de ejecución de Functions utilizan la memoria. Si una función desencadenada por un blob carga el blob entero a la memoria, la memoria máxima utilizada por esa función solo para blobs tiene un tamaño máximo de blob de 24 *. Por ejemplo, una aplicación de función con tres funciones desencadenadas por un blob y la configuración predeterminada tendrían una simultaneidad máxima por máquina virtual de 3*24 = 72 invocaciones de función.

Las funciones de JavaScript y Java cargan el blob entero en la memoria, mientras que las funciones de C# lo hacen si establece un enlace a `string` o `Byte[]`.

## <a name="hostjson-properties"></a>Propiedades de host.json

El archivo [host.json](functions-host-json.md#blobs) contiene opciones de configuración que controlan el comportamiento de desencadenador del blob. Consulte la sección de [configuración de host.json](functions-bindings-storage-blob.md#hostjson-settings) para más información sobre las opciones de configuración disponibles.

## <a name="next-steps"></a>Pasos siguientes

- [Lectura de datos de almacenamiento de blobs cuando se ejecuta una función](./functions-bindings-storage-blob-input.md)
- [Escritura de datos de Blob Storage desde una función](./functions-bindings-storage-blob-output.md)
