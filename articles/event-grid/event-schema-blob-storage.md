---
title: Azure Blob Storage como origen de Event Grid
description: Describe las propiedades que se proporcionan para los eventos de Blob Storage con Azure Event Grid.
ms.topic: conceptual
ms.date: 09/08/2021
ms.openlocfilehash: fcd8ff40e8a31c3329f6526a488a6a6a5261383e
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124797636"
---
# <a name="azure-blob-storage-as-an-event-grid-source"></a>Azure Blob Storage como origen de Event Grid

En este artículo se proporcionan las propiedades y los esquemas de los eventos de Blob Storage.  Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](event-schema.md). También proporciona una lista de inicios rápidos y tutoriales para usar Azure Blob Storage como un origen de eventos.


>[!NOTE]
> Solo las cuentas de almacenamiento de tipo **StorageV2 (uso general v2)** , **BlockBlobStorage** y **BlobStorage** admiten la integración de eventos. **Storage (uso general v1)** *no* admite la integración con Event Grid.

## <a name="available-event-types"></a>Tipos de eventos disponibles

### <a name="list-of-events-for-blob-rest-apis"></a>Lista de eventos para las API REST de Blob

Estos eventos se desencadenan cuando un cliente crea, reemplaza o elimina un blob mediante una llamada a las API REST de Blob.

> [!NOTE]
> Los contenedores `$logs` y `$blobchangefeed` no están integrados con Event Grid, por lo que la actividad en estos contenedores no generará eventos. Además, el uso del punto de conexión DFS *`(abfss://URI) `* para las cuentas habilitadas para el espacio de nombres no jerárquico no generará eventos, pero el punto de conexión de blob *`(wasb:// URI)`* sí que generará eventos.

 |Nombre del evento |Descripción|
 |----------|-----------|
 |**Microsoft.Storage.BlobCreated** |Se desencadena cuando se crea o se sustituye un blob. <br>En concreto, este evento se desencadena cuando los clientes usan las operaciones `PutBlob`, `PutBlockList` o `CopyBlob`, que están disponibles en la API REST del blob **y** cuando el blob en bloques se confirma por completo. <br>Si los clientes usan la operación `CopyBlob` en cuentas que tienen habilitada la característica de **espacio de nombres jerárquico**, la operación `CopyBlob` funciona de forma algo distinta. En ese caso, el evento **Microsoft.Storage.BlobCreated** se desencadena cuando se **inicia** la operación `CopyBlob` y no cuando el blob en bloques se confirma por completo.   |
 |**Microsoft.Storage.BlobDeleted** |Se desencadena cuando se elimina un blob. <br>En concreto, este evento se desencadena cuando los clientes usan la operación `DeleteBlob` que está disponibles en la API REST de Blob. |
 |**Microsoft.Storage.BlobTierChanged** |Se desencadena cuando se cambia el nivel de acceso de blob. En concreto, cuando los clientes llaman a la operación `Set Blob Tier` que está disponible en la API REST de blob, este evento se desencadena una vez que se completa el cambio de nivel. |
|**Microsoft.Storage.AsyncOperationInitiated** |Se desencadena cuando se inicia una operación que supone mover o copiar datos desde el nivele de acceso de archivo al nivel de acceso frecuente o esporádico. En concreto, este evento se desencadena cuando los clientes llaman a la API `Set Blob Tier` para mover un blob del nivel de archivo al nivel de acceso frecuente o esporádico, o cuando los clientes llaman a la API `Copy Blob` para copiar datos de un blob en el nivel de acceso de archivo a un blob en el nivel de acceso frecuente o esporádico.|

### <a name="list-of-the-events-for-azure-data-lake-storage-gen-2-rest-apis"></a>Lista de los eventos para las API REST de Azure Data Lake Storage Gen 2

Estos eventos se desencadenan si habilita un espacio de nombres jerárquico en la cuenta de almacenamiento y los clientes usan las API REST de Azure Data Lake Storage Gen2. Para más información sobre Azure Data Lake Storage Gen2, consulte [Introducción a Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).

|Nombre del evento|Descripción|
|----------|-----------|
|**Microsoft.Storage.BlobCreated** | Se desencadena cuando se crea o se sustituye un blob. <br>En concreto, este evento se desencadena cuando los clientes usan las operaciones `CreateFile` y `FlushWithClose` que están disponibles en la API REST de Azure Data Lake Storage Gen2. |
|**Microsoft.Storage.BlobDeleted** |Se desencadena cuando se elimina un blob. <br>En concreto, este evento también se desencadena cuando los clientes llaman a la operación `DeleteFile` que está disponible en la API REST de Azure Data Lake Storage Gen2. |
|**Microsoft.Storage.BlobRenamed**|Se desencadena cuando se cambia el nombre de un blob. <br>En concreto, este evento se desencadena cuando los clientes usan la operación `RenameFile` que está disponible en la API REST de Azure Data Lake Storage Gen2.|
|**Microsoft.Storage.DirectoryCreated**|Se desencadena cuando se crea un directorio. <br>En concreto, este evento se desencadena cuando los clientes usan la operación `CreateDirectory` que está disponible en la API REST de Azure Data Lake Storage Gen2.|
|**Microsoft.Storage.DirectoryRenamed**|Se desencadena cuando se cambia el nombre de un directorio. <br>En concreto, este evento se desencadena cuando los clientes usan la operación `RenameDirectory` que está disponible en la API REST de Azure Data Lake Storage Gen2.|
|**Microsoft.Storage.DirectoryDeleted**|Se desencadena cuando se elimina un directorio. <br>En concreto, este evento se desencadena cuando los clientes usan la operación `DeleteDirectory` que está disponible en la API REST de Azure Data Lake Storage Gen2.|

> [!NOTE]
> En el caso de **Azure Data Lake Storage Gen2**, si quiere asegurarse de que el evento **Microsoft.Storage.BlobCreated** se desencadena únicamente cuando un blob en bloques está completamente confirmado, filtre el evento para la llamada de API REST `FlushWithClose`. Esta llamada API desencadena el evento **Microsoft.Storage.BlobCreated** únicamente después de que los datos se hayan confirmado en un blob en bloques. Para información sobre cómo crear un filtro, consulte [Filtrado de eventos para Event Grid](./how-to-filter-events.md).

## <a name="example-event"></a>Evento de ejemplo
Cuando se desencadena un evento, el servicio Event Grid envía datos sobre ese evento al punto de conexión correspondiente. Esta sección contiene un ejemplo del aspecto que deben tener los datos para cada evento de blob de almacenamiento.

# <a name="event-grid-event-schema"></a>[Esquema de eventos de Event Grid](#tab/event-grid-event-schema)

### <a name="microsoftstorageblobcreated-event"></a>Evento Microsoft.Storage.BlobCreated

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/test-container/blobs/new-file.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "\"0x8D4BCC2E4835CD0\"",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/new-file.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobcreated-event-data-lake-storage-gen2"></a>Evento Microsoft.Storage.BlobCreated (Data Lake Storage Gen2)

Si la cuenta de almacenamiento de blobs tiene un espacio de nombres jerárquico, los datos tendrán un aspecto similar al ejemplo anterior con la excepción de estos cambios:

* La clave `dataVersion` tiene un valor de `2`.

* La clave `data.api` está establecida en la cadena `CreateFile` o `FlushWithClose`.

* La clave `contentOffset` está incluida en el conjunto de datos.

> [!NOTE]
> Si las aplicaciones utilizan la operación `PutBlockList` para cargar un blob nuevo a la cuenta, los datos no contendrán estos cambios.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/new-file.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "\"0x8D4BCC2E4835CD0\"",
    "contentType": "text/plain",
    "contentLength": 0,
    "contentOffset": 0,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/new-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event"></a>Evento Microsoft.Storage.BlobDeleted

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/file-to-delete.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event-data-lake-storage-gen2"></a>Evento Microsoft.Storage.BlobDeleted (Data Lake Storage Gen2)

Si la cuenta de almacenamiento de blobs tiene un espacio de nombres jerárquico, los datos tendrán un aspecto similar al ejemplo anterior con la excepción de estos cambios:

* La clave `dataVersion` tiene un valor de `2`.

* La clave `data.api` está establecida en la cadena `DeleteFile`.

* La clave `url` contiene la ruta de acceso `dfs.core.windows.net`.

> [!NOTE]
> Si las aplicaciones utilizan la operación `DeleteBlob` para eliminar un blob de la cuenta, los datos no contendrán estos cambios.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
    "api": "DeleteFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/file-to-delete.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobtierchanged-event"></a>Evento Microsoft.Storage.BlobTierChanged

```json
{
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
    "subject": "/blobServices/default/containers/testcontainer/blobs/Auto.jpg",
    "eventType": "Microsoft.Storage.BlobTierChanged",
    "id": "0fdefc06-b01e-0034-39f6-4016610696f6",
    "data": {
        "api": "SetBlobTier",
        "clientRequestId": "68be434c-1a0d-432f-9cd7-1db90bff83d7",
        "requestId": "0fdefc06-b01e-0034-39f6-401661000000",
        "contentType": "image/jpeg",
        "contentLength": 105891,
        "blobType": "BlockBlob",
        "url": "https://my-storage-account.blob.core.windows.net/testcontainer/Auto.jpg",
        "sequencer": "000000000000000000000000000089A4000000000018d6ea",
        "storageDiagnostics": {
            "batchId": "3418f7a9-7006-0014-00f6-406dc6000000"
        }
    },
    "dataVersion": "",
    "metadataVersion": "1",
    "eventTime": "2021-05-04T15:00:00.8350154Z"
}
```

### <a name="microsoftstorageasyncoperationinitiated-event"></a>Evento Microsoft.Storage.AsyncOperationInitiated

```json
{
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
    "subject": "/blobServices/default/containers/testcontainer/blobs/00000.avro",
    "eventType": "Microsoft.Storage.AsyncOperationInitiated",
    "id": "8ea4e3f2-101e-003d-5ff4-4053b2061016",
    "data": {
        "api": "SetBlobTier",
        "clientRequestId": "777fb4cd-f890-4c5b-b024-fb47300bae62",
        "requestId": "8ea4e3f2-101e-003d-5ff4-4053b2000000",
        "contentType": "application/octet-stream",
        "contentLength": 3660,
        "blobType": "BlockBlob",
        "url": "https://my-storage-account.blob.core.windows.net/testcontainer/00000.avro",
        "sequencer": "000000000000000000000000000089A4000000000018c6d7",
        "storageDiagnostics": {
            "batchId": "34128c8a-7006-0014-00f4-406dc6000000"
        }
    },
    "dataVersion": "",
    "metadataVersion": "1",
    "eventTime": "2021-05-04T14:44:59.3204652Z"
}
```


### <a name="microsoftstorageblobrenamed-event"></a>Evento Microsoft.Storage.BlobRenamed

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-file.txt",
  "eventType": "Microsoft.Storage.BlobRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-file.txt",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorycreated-event"></a>Evento Microsoft.Storage.DirectoryCreated

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-new-directory",
  "eventType": "Microsoft.Storage.DirectoryCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-new-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectoryrenamed-event"></a>Evento Microsoft.Storage.DirectoryRenamed

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-directory",
  "eventType": "Microsoft.Storage.DirectoryRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-directory",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorydeleted-event"></a>Evento Microsoft.Storage.DirectoryDeleted

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/directory-to-delete",
  "eventType": "Microsoft.Storage.DirectoryDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "DeleteDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/directory-to-delete",
    "recursive": "true", 
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

# <a name="cloud-event-schema"></a>[Esquema de eventos en la nube](#tab/cloud-event-schema)

### <a name="microsoftstorageblobcreated-event"></a>Evento Microsoft.Storage.BlobCreated

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/test-container/blobs/new-file.txt",
  "type": "Microsoft.Storage.BlobCreated",
  "time": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "\"0x8D4BCC2E4835CD0\"",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/new-file.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "specversion": "1.0"
}]
```

### <a name="microsoftstorageblobcreated-event-data-lake-storage-gen2"></a>Evento Microsoft.Storage.BlobCreated (Data Lake Storage Gen2)

Si la cuenta de almacenamiento de blobs tiene un espacio de nombres jerárquico, los datos tendrán un aspecto similar al ejemplo anterior con la excepción de estos cambios:

* La clave `data.api` está establecida en la cadena `CreateFile` o `FlushWithClose`.
* La clave `contentOffset` está incluida en el conjunto de datos.

> [!NOTE]
> Si las aplicaciones utilizan la operación `PutBlockList` para cargar un blob nuevo a la cuenta, los datos no contendrán estos cambios.

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/new-file.txt",
  "type": "Microsoft.Storage.BlobCreated",
  "time": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "\"0x8D4BCC2E4835CD0\"",
    "contentType": "text/plain",
    "contentLength": 0,
    "contentOffset": 0,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/new-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "specversion": "1.0"
}]
```

### <a name="microsoftstorageblobdeleted-event"></a>Evento Microsoft.Storage.BlobDeleted

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file-to-delete.txt",
  "type": "Microsoft.Storage.BlobDeleted",
  "time": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/file-to-delete.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "specversion": "1.0"
}]
```

### <a name="microsoftstorageblobdeleted-event-data-lake-storage-gen2"></a>Evento Microsoft.Storage.BlobDeleted (Data Lake Storage Gen2)

Si la cuenta de almacenamiento de blobs tiene un espacio de nombres jerárquico, los datos tendrán un aspecto similar al ejemplo anterior con la excepción de estos cambios:


* La clave `data.api` está establecida en la cadena `DeleteFile`.
* La clave `url` contiene la ruta de acceso `dfs.core.windows.net`.

> [!NOTE]
> Si las aplicaciones utilizan la operación `DeleteBlob` para eliminar un blob de la cuenta, los datos no contendrán estos cambios.

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/file-to-delete.txt",
  "type": "Microsoft.Storage.BlobDeleted",
  "time": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
    "api": "DeleteFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/file-to-delete.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "specversion": "1.0"
}]
```

### <a name="microsoftstorageblobrenamed-event"></a>Evento Microsoft.Storage.BlobRenamed

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-file.txt",
  "type": "Microsoft.Storage.BlobRenamed",
  "time": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-file.txt",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "specversion": "1.0"
}]
```

### <a name="microsoftstoragedirectorycreated-event"></a>Evento Microsoft.Storage.DirectoryCreated

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-new-directory",
  "type": "Microsoft.Storage.DirectoryCreated",
  "time": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-new-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "specversion": "1.0"
}]
```

### <a name="microsoftstoragedirectoryrenamed-event"></a>Evento Microsoft.Storage.DirectoryRenamed

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-directory",
  "type": "Microsoft.Storage.DirectoryRenamed",
  "time": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-directory",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "specversion": "1.0"
}]
```

### <a name="microsoftstoragedirectorydeleted-event"></a>Evento Microsoft.Storage.DirectoryDeleted

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/directory-to-delete",
  "type": "Microsoft.Storage.DirectoryDeleted",
  "time": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "DeleteDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/directory-to-delete",
    "recursive": "true", 
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "specversion": "1.0"
}]
```

---


## <a name="event-properties"></a>Propiedades de evento

# <a name="event-grid-event-schema"></a>[Esquema de eventos de Event Grid](#tab/event-grid-event-schema)

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| `topic` | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| `subject` | string | Ruta al asunto del evento definida por el anunciante. |
| `eventType` | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| `eventTime` | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| `id` | string | Identificador único para el evento |
| `data` | object | Datos de eventos de Blob Storage. |
| `dataVersion` | string | Versión del esquema del objeto de datos. El publicador define la versión del esquema. |
| `metadataVersion` | string | Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor. |

# <a name="cloud-event-schema"></a>[Esquema de eventos en la nube](#tab/cloud-event-schema)

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| `source` | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| `subject` | string | Ruta al asunto del evento definida por el anunciante. |
| `type` | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| `time` | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| `id` | string | Identificador único para el evento |
| `data` | object | Datos de eventos de Blob Storage. |
| `specversion` | string | Versión de especificación del esquema CloudEvents. |

---

El objeto data tiene las siguientes propiedades:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| `api` | string | Operación que desencadenó el evento. |
| `clientRequestId` | string | Id. de solicitud que proporciona el cliente para la operación de la API de Storage. Dicho id. se puede usar para establecer la correlación con los registros de diagnóstico de Azure Storage que usan el campo "client-request-id" en los registros y se puede proporcionar en las solicitudes de los clientes que usan el encabezado "x-ms-client-request-id". Consulte [Storage Analytics Log Format](/rest/api/storageservices/storage-analytics-log-format) (Formato de registro de Storage Analytics). |
| `requestId` | string | Id. de solicitud generado por el servicio para la operación de la API de Storage. Se puede usar para establecer la correlación con los registros de diagnóstico de Azure Storage que usan el campo "request-id-header" en los registros y se devuelve cuando se inicia la llamada API en el encabezado "x-ms-request-id". Consulte [Storage Analytics Log Format](/rest/api/storageservices/storage-analytics-log-format) (Formato de registro de Storage Analytics). |
| `eTag` | string | Valor que puede usar para ejecutar operaciones de manera condicional. |
| `contentType` | string | Tipo de contenido especificado para el blob. |
| `contentLength` | integer | Tamaño del blob en bytes. |
| `blobType` | string | El tipo de blob. Los valores válidos son "BlockBlob" o "PageBlob". |
| `contentOffset` | number | Desplazamiento en bytes de una operación de escritura realizada en el punto en el que la aplicación de desencadenamiento de eventos completa la escritura del archivo. <br>Solo aparece para los eventos desencadenados en las cuentas de almacenamiento de blobs que tienen un espacio de nombres jerárquico.|
| `destinationUrl` |string | Dirección URL del archivo que existirá una vez completada la operación. Por ejemplo, si se cambia el nombre de un archivo, la propiedad `destinationUrl` contiene la dirección URL del nuevo nombre de archivo. <br>Solo aparece para los eventos desencadenados en las cuentas de almacenamiento de blobs que tienen un espacio de nombres jerárquico.|
| `sourceUrl` |string | Dirección URL del archivo que existe antes de completarse la operación. Por ejemplo, si se cambia el nombre de un archivo, `sourceUrl` contiene la dirección URL del nombre de archivo original antes de la operación de cambio de nombre. <br>Solo aparece para los eventos desencadenados en las cuentas de almacenamiento de blobs que tienen un espacio de nombres jerárquico. |
| `url` | string | Ruta de acceso al blob. <br>Si el cliente usa una API REST de blobs, la dirección URL tiene esta estructura: `<storage-account-name>.blob.core.windows.net\<container-name>\<file-name>`. <br>Si el cliente usa una API REST de Data Lake Store, la dirección URL tiene esta estructura: `<storage-account-name>.dfs.core.windows.net/<file-system-name>/<file-name>`. |
| `recursive` | string | `True` para ejecutar la operación en todos los directorios secundarios; en caso contrario, `False`. <br>Solo aparece para los eventos desencadenados en las cuentas de almacenamiento de blobs que tienen un espacio de nombres jerárquico. |
| `sequencer` | string | Un valor de cadena opaco que representa la secuencia lógica de eventos para cualquier nombre de blob concreto.  Los usuarios pueden usar una comparación de cadenas estándar para conocer la secuencia relativa de dos eventos que estén en el mismo nombre de blob. |
| `storageDiagnostics` | object | Datos de diagnóstico que, en ocasiones, incluye el servicio Azure Storage. Cuando están presentes, los consumidores de eventos deben ignorarlos. |

## <a name="tutorials-and-how-tos"></a>Tutoriales y procedimientos
|Título  |Descripción  |
|---------|---------|
| [Guía de inicio rápido: enrutamiento de eventos de Blob Storage a un punto de conexión web personalizado con la CLI de Azure](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo utilizar la CLI de Azure para enviar eventos de Blob Storage a un webhook. |
| [Guía de inicio rápido: enrutamiento de eventos de Blob Storage a un punto de conexión web personalizado con PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Muestra cómo utilizar Azure PowerShell para enviar eventos de Blob Storage a un webhook. |
| [Guía de inicio rápido: creación y enrutamiento de eventos de Blob Storage con Azure Portal](blob-event-quickstart-portal.md) | Muestra cómo utilizar el portal para enviar eventos de Blob Storage a un webhook. |
| [CLI de Azure: suscripción a eventos de una cuenta de Blob Storage](./scripts/event-grid-cli-blob.md) | Script de ejemplo que se suscribe a un evento para una cuenta de Blob Storage. Envía el evento a un webhook. |
| [PowerShell: suscripción a eventos de una cuenta de Blob Storage](./scripts/event-grid-powershell-blob.md) | Script de ejemplo que se suscribe a un evento para una cuenta de Blob Storage. Envía el evento a un webhook. |
| [Plantilla de Resource Manager: Creación de almacenamiento y suscripción de blobs](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.eventgrid/event-grid-subscription-and-storage) | Implementa una cuenta de Azure Blob Storage y crea una suscripción a eventos de dicha cuenta. Envía eventos a un webhook. |
| [Introducción: reacción ante eventos de Blob Storage](../storage/blobs/storage-blob-event-overview.md) | Información general de la integración de Blob Storage con Event Grid. |

## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a Azure Event Grid, consulte [Introducción a Azure Event Grid](overview.md).
* Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).
* Para obtener una introducción acerca del trabajo con eventos de Blob Storage, consulte [Enrutamiento de eventos de Blob Storage: CLI de Azure](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
