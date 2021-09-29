---
title: Procedimiento para usar Azure Queue Storage desde Python
description: Aprenda a usar Azure Queue Storage desde Python para crear y eliminar colas, e insertar, obtener y eliminar mensajes.
author: normesta
ms.author: normesta
ms.reviewer: dineshm
ms.date: 02/16/2021
ms.topic: how-to
ms.service: storage
ms.subservice: queues
ms.custom: seo-javascript-october2019, devx-track-python
ms.openlocfilehash: c7210ea6c0930f77620254a09b46ea37055aa8ef
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128579560"
---
# <a name="how-to-use-azure-queue-storage-from-python"></a>Procedimiento para usar Azure Queue Storage desde Python

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

## <a name="overview"></a>Información general

En este artículo se muestran escenarios comunes con el servicio Azure Queue Storage. Entre los escenarios descritos se incluyen la inserción, inspección, obtención y eliminación de mensajes de la cola. También se incluye código para crear y eliminar colas.

Los ejemplos de este artículo están escritos en Python y usan la [biblioteca cliente de Azure Queue Storage para Python](https://github.com/azure/azure-sdk-for-python/tree/master/sdk/storage/azure-storage-queue). Para obtener más información sobre las colas, consulte la sección [Pasos siguientes](#next-steps) .

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Descarga e instalación del SDK de Azure Storage para Python

El [SDK de Azure Storage para Python](https://github.com/azure/azure-storage-python) necesita Python 2.7, 3.3 o una versión posterior.

### <a name="install-via-pypi"></a>Instalación mediante PyPI

Para realizar la instalación mediante el índice de paquetes de Python (PyPI), escriba:

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

```console
pip install azure-storage-queue
```

# <a name="python-v2"></a>[Python v2](#tab/python2)

```console
pip install azure-storage-queue==2.1.0
```

---

> [!NOTE]
> Si va a actualizar desde el SDK de Azure Storage para Python 0.36 o una versión anterior, desinstale el SDK anterior mediante `pip uninstall azure-storage` antes de instalar el paquete más reciente.

Para métodos de instalación alternativos, consulte [SDK de Azure para Python](https://github.com/Azure/Azure-SDK-for-Python).

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="configure-your-application-to-access-queue-storage"></a>Configuración de la aplicación para obtener acceso a Queue Storage

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

El objeto [`QueueClient`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient) permite trabajar con una cola. Agregue el siguiente código cerca de la parte superior de cualquier archivo Python en el que desee obtener acceso a una cola de Azure mediante programación:

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_ImportStatements":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

El objeto [`QueueService`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2&preserve-view=true) permite trabajar con colas. El código siguiente crea un objeto `QueueService`. Agregue el siguiente código cerca de la parte superior de cualquier archivo Python en el que desee obtener acceso a Azure Storage mediante programación:

```python
from azure.storage.queue import (
        QueueService,
        QueueMessageFormat
)

import os, uuid
```

---

El paquete `os` proporciona compatibilidad para recuperar una variable de entorno. El paquete `uuid` proporciona compatibilidad para generar un identificador único para un nombre de cola.

## <a name="create-a-queue"></a>Creación de una cola

La cadena de conexión se recupera de la variable de entorno `AZURE_STORAGE_CONNECTION_STRING` establecida anteriormente.

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

El código siguiente crea un objeto `QueueClient` con la cadena de conexión de almacenamiento.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_CreateQueue":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

El código siguiente crea un objeto `QueueService` con la cadena de conexión de almacenamiento.

```python
# Retrieve the connection string from an environment
# variable named AZURE_STORAGE_CONNECTION_STRING
connect_str = os.getenv("AZURE_STORAGE_CONNECTION_STRING")

# Create a unique name for the queue
queue_name = "queue-" + str(uuid.uuid4())

# Create a QueueService object which will
# be used to create and manipulate the queue
print("Creating queue: " + queue_name)
queue_service = QueueService(connection_string=connect_str)

# Create the queue
queue_service.create_queue(queue_name)
```

---

Los mensajes de la cola de Azure se almacenan como texto. Si quiere almacenar datos binarios, configure las funciones de codificación y descodificación de Base64 antes de colocar un mensaje en la cola.

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

Configure las funciones de codificación y descodificación de Base64 durante la creación del objeto de cliente.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_EncodeMessage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Configure las funciones de codificación y descodificación de Base64 en el objeto de Queue Storage.

```python
# Setup Base64 encoding and decoding functions
queue_service.encode_function = QueueMessageFormat.binary_base64encode
queue_service.decode_function = QueueMessageFormat.binary_base64decode
```

---

## <a name="insert-a-message-into-a-queue"></a>un mensaje en una cola

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

Para insertar un mensaje en una cola, use el método [`send_message`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#send-message-content----kwargs-).

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_AddMessage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Para insertar un mensaje en una cola, use el método [`put_message`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2&preserve-view=true#put-message-queue-name--content--visibility-timeout-none--time-to-live-none--timeout-none-) para crear un mensaje y agregarlo a la cola.

```python
message = u"Hello, World"
print("Adding message: " + message)
queue_service.put_message(queue_name, message)
```

---

## <a name="peek-at-messages"></a>Inspección de mensajes

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

Puede ver el código sin salir de los mensajes sin tener que quitarlos de la cola, mediante una llamada al método [`peek_messages`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#peek-messages-max-messages-none----kwargs-). De forma predeterminada, este método ve el código sin salir en un único mensaje.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_PeekMessage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Puede ver el código sin salir de los mensajes sin tener que quitarlos de la cola, mediante una llamada al método [`peek_messages`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2&preserve-view=true#peek-messages-queue-name--num-messages-none--timeout-none-). De forma predeterminada, este método ve el código sin salir en un único mensaje.

```python
messages = queue_service.peek_messages(queue_name)

for peeked_message in messages:
    print("Peeked message: " + peeked_message.content)
```

---

## <a name="change-the-contents-of-a-queued-message"></a>contenido de un mensaje en cola

Puede cambiar el contenido de un mensaje local en la cola. Si el mensaje representa una tarea, puede usar esta característica para actualizar el estado de la tarea.

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

En el código siguiente se usa el método [`update_message`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#update-message-message--pop-receipt-none--content-none----kwargs-) para actualizar un mensaje. El tiempo de espera de visibilidad se establece en 0, lo que significa que el mensaje aparece inmediatamente y se actualiza el contenido.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_ChangeMessage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

En el código siguiente se usa el método [`update_message`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2&preserve-view=true#update-message-queue-name--message-id--pop-receipt--visibility-timeout--content-none--timeout-none-) para actualizar un mensaje. El tiempo de espera de visibilidad se establece en 0, lo que significa que el mensaje aparece inmediatamente y se actualiza el contenido.

```python
messages = queue_service.get_messages(queue_name)

for message in messages:
    queue_service.update_message(
        queue_name, message.id, message.pop_receipt, 0, u"Hello, World Again")
```

---

## <a name="get-the-queue-length"></a>la longitud de la cola

Puede obtener una estimación del número de mensajes existentes en una cola.

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

El método [get_queue_properties](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#get-queue-properties---kwargs-) devuelve propiedades de cola, incluida `approximate_message_count`.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_GetQueueLength":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

El método [`get_queue_metadata`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2&preserve-view=true#get-queue-metadata-queue-name--timeout-none-) devuelve propiedades de cola, incluido `approximate_message_count`.

```python
metadata = queue_service.get_queue_metadata(queue_name)
count = metadata.approximate_message_count
print("Message count: " + str(count))
```

---

El resultado solo es aproximado, ya que se pueden agregar o borrar mensajes después de que el servicio responda la solicitud.

## <a name="dequeue-messages"></a>Retirar mensajes de la cola

Quite un mensaje de una cola en dos pasos. Si el código no puede procesar un mensaje, este proceso de dos pasos garantiza que pueda obtener el mismo mensaje e intentarlo de nuevo. Llame a `delete_message` después de que el mensaje se haya procesado correctamente.

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

Si llama a [receive_messages](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-), obtiene, de forma predeterminada, el siguiente mensaje de una cola. Un mensaje devuelto por `receive_messages` se hace invisible a cualquier otro código de lectura de mensajes de esta cola. De forma predeterminada, este mensaje permanece invisible durante 30 segundos. Para terminar quitando el mensaje de la cola, también debe llamar a [delete_message](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#delete-message-message--pop-receipt-none----kwargs-).

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_DequeueMessages":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Si llama a [get_messages](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2&preserve-view=true#get-messages-queue-name--num-messages-none--visibility-timeout-none--timeout-none-), obtiene, de forma predeterminada, el siguiente mensaje de una cola. Un mensaje devuelto por `get_messages` se hace invisible a cualquier otro código de lectura de mensajes de esta cola. De forma predeterminada, este mensaje permanece invisible durante 30 segundos. Para terminar quitando el mensaje de la cola, también debe llamar a [delete_message](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2&preserve-view=true#delete-message-queue-name--message-id--pop-receipt--timeout-none-).

```python
messages = queue_service.get_messages(queue_name)

for message in messages:
    print("Deleting message: " + message.content)
    queue_service.delete_message(queue_name, message.id, message.pop_receipt)
```

---

Hay dos formas de personalizar la recuperación de mensajes de una cola. En primer lugar, puede obtener un lote de mensajes (hasta 32). En segundo lugar, puede establecer un tiempo de espera de la invisibilidad más largo o más corto para que el código disponga de más o menos tiempo para procesar cada mensaje.

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

En el ejemplo de código siguiente se usa el método [`receive_messages`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#receive-messages---kwargs-) para obtener mensajes en lotes. A continuación, procesa cada mensaje dentro de cada lote mediante un bucle `for` anidado. También establece el tiempo de espera de la invisibilidad en cinco minutos para cada mensaje.

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_DequeueByPage":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

En el ejemplo de código siguiente se usa el método [`get_messages`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2&preserve-view=true#get-messages-queue-name--num-messages-none--visibility-timeout-none--timeout-none-) para obtener 16 mensajes en una llamada. A continuación, procesa cada mensaje con un bucle `for`. También establece el tiempo de espera de la invisibilidad en cinco minutos para cada mensaje.

```python
messages = queue_service.get_messages(queue_name, num_messages=16, visibility_timeout=5*60)

for message in messages:
    print("Deleting message: " + message.content)
    queue_service.delete_message(queue_name, message.id, message.pop_receipt)
```

---

## <a name="delete-a-queue"></a>Eliminación de una cola

# <a name="python-v12-sdk"></a>[SDK para Python v12](#tab/python)

Para eliminar una cola y todos los mensajes que contiene, llame al método [`delete_queue`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueclient#delete-queue---kwargs-).

:::code language="python" source="~/azure-storage-snippets/queues/howto/python/python-v12/python-howto-v12.py" id="Snippet_DeleteQueue":::

# <a name="python-v2"></a>[Python v2](#tab/python2)

Para eliminar una cola y todos los mensajes que contiene, llame al método [`delete_queue`](/azure/developer/python/sdk/storage/azure-storage-queue/azure.storage.queue.queueservice.queueservice?view=storage-py-v2&preserve-view=true#delete-queue-queue-name--fail-not-exist-false--timeout-none-).

```python
print("Deleting queue: " + queue_name)
queue_service.delete_queue(queue_name)
```

---

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya conoce los aspectos básicos de Queue Storage, siga estos vínculos para obtener más información.

- [Referencia de la API de Python de Azure Queue Storage](/python/api/azure-storage-queue)
- [Centro para desarrolladores de Python](https://azure.microsoft.com/develop/python/)
- [Referencia de API REST de Azure Storage](/rest/api/storageservices/)
