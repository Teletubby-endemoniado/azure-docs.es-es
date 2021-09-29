---
title: 'Uso de Azure Queue Storage de Node.js: Azure Storage'
description: Aprenda a usar Azure Queue Storage para crear y eliminar colas. Aprenda a insertar, obtener y eliminar mensajes mediante Node.js.
author: normesta
ms.author: normesta
ms.reviewer: dineshm
ms.date: 12/21/2020
ms.topic: how-to
ms.service: storage
ms.subservice: queues
ms.custom: seo-javascript-september2019, devx-track-js
ms.openlocfilehash: 29a621aa5221a0a4ee06c5c9a635af49320533c7
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128638280"
---
# <a name="how-to-use-azure-queue-storage-from-nodejs"></a>Uso de Azure Queue Storage desde Node.js

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

## <a name="overview"></a>Información general

Esta guía le indicará cómo actuar en escenarios habituales usando Azure Queue Storage. Los ejemplos están escritos usando la API Node.js. Entre los escenarios descritos se incluyen la inserción, inspección, obtención y eliminación de mensajes de la cola. También aprenderá a crear y eliminar colas.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Creación de una aplicación Node.js

Para crear una aplicación de Node.js en blanco, consulte [Creación de una aplicación web Node.js en Azure App Service](../../app-service/quickstart-nodejs.md), [Creación e implementación de una aplicación Node.js en Azure Cloud Services](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) con PowerShell o [Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="configure-your-application-to-access-storage"></a>Configuración de la aplicación para acceder al almacenamiento

La [biblioteca cliente de Azure Storage para JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage#azure-storage-client-library-for-javascript) incluye un conjunto de bibliotecas útiles que se comunican con los servicios REST de almacenamiento.

<!-- docutune:ignore Terminal -->

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Uso del Administrador de paquetes para Node (NPM) para obtener el paquete

1. Use una interfaz de línea de comandos, como PowerShell (Windows), Terminal (Mac) o Bash (Unix), y vaya a la carpeta donde ha creado la aplicación de ejemplo.

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

1. Escriba `npm install @azure/storage-queue` en la ventana de comandos.

1. Compruebe que se ha creado una carpeta `node_modules`. Dentro de dicha carpeta, encontrará el paquete `@azure/storage-queue`, que contiene la biblioteca cliente necesaria para acceder al almacenamiento.

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

1. Escriba `npm install azure-storage` en la ventana de comandos.

1. Compruebe que se ha creado una carpeta `node_modules`. Dentro de dicha carpeta, encontrará el paquete `azure-storage`, que contiene las bibliotecas necesarias para acceder al almacenamiento.

---

### <a name="import-the-package"></a>Importación del paquete

Con el editor de código, agregue lo siguiente al principio del archivo de JavaScript en el que pretenda utilizar el las colas.

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_ImportStatements":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

```javascript
var azure = require('azure-storage');
```

---

## <a name="how-to-create-a-queue"></a>Creación de una cola

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

En el código siguiente se obtiene el valor de una variable de entorno denominada `AZURE_STORAGE_CONNECTION_STRING` y se usa para crear un objeto [`QueueServiceClient`](/javascript/api/@azure/storage-queue/queueserviceclient). Este objeto se utiliza a continuación para crear un objeto [`QueueClient`](/javascript/api/@azure/storage-queue/queueclient) que permita trabajar con una cola específica.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_CreateQueue":::

Si la cola ya existe, se produce una excepción.

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

El módulo de Azure leerá las variables de entorno `AZURE_STORAGE_ACCOUNT` y `AZURE_STORAGE_ACCESS_KEY` o `AZURE_STORAGE_CONNECTION_STRING` para obtener la información necesaria para conectarse a la cuenta de Azure Storage. Si no se han configurado estas variables de entorno, debe especificar la información de la cuenta llamando a `createQueueService`.

El siguiente código crea un objeto `QueueService` , que le permite trabajar con colas.

```javascript
var queueSvc = azure.createQueueService();
```

Llame al método `createQueueIfNotExists` para crear una nueva cola con el nombre especificado o devolver la cola si ya existe.

```javascript
queueSvc.createQueueIfNotExists('myqueue', function(error, results, response){
  if(!error){
    // Queue created or exists
  }
});
```

Si la cola se crea, `result.created` es verdadero. Si la cola existe, `result.created` es falso.

---

## <a name="how-to-insert-a-message-into-a-queue"></a>Cómo insertar un mensaje en una cola

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

Para agregar un mensaje a una cola, llame al método [`sendMessage`](/javascript/api/@azure/storage-queue/queueclient#sendmessage-string--queuesendmessageoptions-).

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_AddMessage":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Para insertar un mensaje en una cola, llame al método `createMessage` para crear un nuevo mensaje y agregarlo a la cola.

```javascript
queueSvc.createMessage('myqueue', "Hello, World", function(error, results, response){
  if(!error){
    // Message inserted
  }
});
```

---

## <a name="how-to-peek-at-the-next-message"></a>Inspección del mensaje siguiente

Puede inspeccionar el mensaje de la cola sin tener que quitarlo de esta, mediante una llamada al método `peekMessages`.

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

De forma predeterminada, [`peekMessages`](/javascript/api/@azure/storage-queue/queueclient#peekmessages-queuepeekmessagesoptions-) inspecciona un único mensaje. En el ejemplo siguiente se inspeccionan los cinco primeros mensajes de la cola. Si hay menos de cinco mensajes visibles, solo se devuelven los mensajes visibles.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_PeekMessage":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

De forma predeterminada, `peekMessages` inspecciona un único mensaje.

```javascript
queueSvc.peekMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
  }
});
```

El `result` contiene el mensaje.

---

Si llama a `peekMessages` cuando no hay ningún mensaje en la cola, no se devolverá un error. Sin embargo, no se devolverá ningún mensaje.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Cambio del contenido de un mensaje en cola

En el ejemplo siguiente se actualiza el texto de un mensaje.

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

Cambie el contenido de un mensaje local en la cola llamando a [`updateMessage`](/javascript/api/@azure/storage-queue/queueclient#updatemessage-string--string--string--number--queueupdatemessageoptions-).

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_UpdateMessage":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Cambie el contenido de un mensaje local en la cola mediante una llamada a `updateMessage`.

```javascript
queueSvc.getMessages('myqueue', function(error, getResults, getResponse){
  if(!error){
    // Got the message
    var message = getResults[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, updateResults, updateResponse){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

---

## <a name="how-to-dequeue-a-message"></a>Cómo quitar un mensaje de la cola

El proceso para quitar un mensaje de la cola consta de dos etapas:

1. Obtención del mensaje.

1. Eliminación del mensaje.

En el siguiente ejemplo se obtiene un mensaje y luego se elimina.

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

Para obtener un mensaje, llame al método [`receiveMessages`](/javascript/api/@azure/storage-queue/queueclient#receivemessages-queuereceivemessageoptions-). Esta llamada hace que los mensajes sean invisibles en la cola, así que ningún otro cliente puede procesarlos. Después de que la aplicación haya procesado un mensaje, llame a [`deleteMessage`](/javascript/api/@azure/storage-queue/queueclient#deletemessage-string--string--queuedeletemessageoptions-) para eliminarlo de la cola.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_DequeueMessage":::

De manera predeterminada, un mensaje solo está oculto durante 30 segundos. Después de 30 segundos, es visible para otros clientes. Puede especificar un valor diferente estableciendo [`options.visibilityTimeout`](/javascript/api/@azure/storage-queue/queuereceivemessageoptions#visibilitytimeout) al llamar a `receiveMessages`.

Si llama a `receiveMessages` cuando no hay ningún mensaje en la cola, no se devolverá un error. Sin embargo, no se devolverá ningún mensaje.

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Para obtener un mensaje, llame al método `getMessages`. Esta llamada hace que los mensajes sean invisibles en la cola, así que ningún otro cliente puede procesarlos. Después de que la aplicación haya procesado un mensaje, llame a `deleteMessage` para eliminarlo de la cola.

```javascript
queueSvc.getMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
    var message = results[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

De manera predeterminada, un mensaje solo está oculto durante 30 segundos. Después de 30 segundos, es visible para otros clientes. Puede especificar un valor diferente usando `options.visibilityTimeout` con `getMessages`.

Si usa `getMessages` cuando no hay ningún mensaje en la cola, no se devolverá un error. Sin embargo, no se devolverá ningún mensaje.

---

## <a name="additional-options-for-dequeuing-messages"></a>Opciones adicionales para quitar mensajes de la cola

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

Hay dos formas de personalizar la recuperación de mensajes de una cola:

- [`options.numberOfMessages`](/javascript/api/@azure/storage-queue/queuereceivemessageoptions#numberofmessages): Recuperar un lote de mensajes (hasta 32).
- [`options.visibilityTimeout`](/javascript/api/@azure/storage-queue/queuereceivemessageoptions#visibilitytimeout): Establecer un tiempo de espera de invisibilidad más largo o más corto.

El siguiente ejemplo utiliza el método `receiveMessages` para obtener cinco mensajes en una llamada. A continuación, procesa cada mensaje con un bucle `for`. También se establece el tiempo de espera de invisibilidad en cinco minutos para todos los mensajes que devuelve este método.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_DequeueMessages":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Hay dos formas de personalizar la recuperación de mensajes de una cola:

- `options.numOfMessages`: Recuperar un lote de mensajes (hasta 32).
- `options.visibilityTimeout`: Establecer un tiempo de espera de invisibilidad más largo o más corto.

En el siguiente ejemplo se usa el método `getMessages` para obtener 15 mensajes en una llamada. A continuación, procesa cada mensaje con un bucle `for`. También se establece el tiempo de espera de invisibilidad en cinco minutos para todos los mensajes que devuelve este método.

```javascript
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, results, getResponse){
  if(!error){
    // Messages retrieved
    for(var index in results){
      // text is available in result[index].messageText
      var message = results[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, deleteResponse){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

---

## <a name="how-to-get-the-queue-length"></a>Obtención de la longitud de la cola

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

El método [`getProperties`](/javascript/api/@azure/storage-queue/queueclient#getproperties-queuegetpropertiesoptions-) devuelve metadatos sobre la cola, junto con el número aproximado de mensajes que esperan en la cola.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_QueueLength":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

El método `getQueueMetadata` devuelve metadatos sobre la cola, junto con el número aproximado de mensajes que esperan en la cola.

```javascript
queueSvc.getQueueMetadata('myqueue', function(error, results, response){
  if(!error){
    // Queue length is available in results.approximateMessageCount
  }
});
```

---

## <a name="how-to-list-queues"></a>Enumeración de las colas

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

Para recuperar una lista de colas, llame a [`QueueServiceClient.listQueues`](/javascript/api/@azure/storage-queue/servicelistqueuesoptions#prefix). Para recuperar una lista filtrada por un prefijo específico, establezca [options.prefix](/javascript/api/@azure/storage-queue/servicelistqueuesoptions#prefix) en la llamada a `listQueues`.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_ListQueues":::

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Para recuperar una lista de colas, use `listQueuesSegmented`. Para recuperar una lista filtrada por un prefijo determinado, use `listQueuesSegmentedWithPrefix`.

```javascript
queueSvc.listQueuesSegmented(null, function(error, results, response){
  if(!error){
    // results.entries contains the list of queues
  }
});
```

Si no se pueden devolver todas las colas, pase `result.continuationToken` como primer parámetro de `listQueuesSegmented` o como segundo parámetro de `listQueuesSegmentedWithPrefix` para recuperar más resultados.

---

## <a name="how-to-delete-a-queue"></a>Como eliminar una cola

# <a name="javascript-v12-sdk"></a>[SDK de JavaScript v12](#tab/javascript)

Para eliminar una cola y todos los mensajes contenidos en ella, llame al método [`DeleteQueue`](/javascript/api/@azure/storage-queue/queueclient#delete-queuedeleteoptions-) en el objeto `QueueClient`.

:::code language="javascript" source="~/azure-storage-snippets/queues/howto/JavaScript/JavaScript-v12/javascript-queues-v12.js" id="Snippet_DeleteQueue":::

Para borrar todos los mensajes de una cola sin eliminarla, llame a [`ClearMessages`](/javascript/api/@azure/storage-queue/queueclient#clearmessages-queueclearmessagesoptions-).

# <a name="javascript-v2"></a>[JavaScript v2](#tab/javascript2)

Para eliminar una cola y todos los mensajes contenidos en ella, llame al método `deleteQueue` en el objeto de cola.

```javascript
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

Para borrar todos los mensajes de una cola sin eliminarla, llame a `clearMessages`.

---

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con los aspectos básicos de Queue Storage, utilice estos vínculos para obtener más información acerca de tareas de almacenamiento más complejas.

- Visite el [blog del equipo de Azure Storage](https://techcommunity.Microsoft.com/t5/Azure-storage/bg-p/azurestorageblog) para conocer las novedades.
- Visite el repositorio [Biblioteca cliente de Azure Storage para JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage#Azure-storage-client-library-for-JavaScript) en GitHub
