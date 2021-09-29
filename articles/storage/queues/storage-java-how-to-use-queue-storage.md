---
title: Uso de Queue Storage de Java - Azure Storage
description: Obtenga información acerca de cómo usar Queue Storage para crear y eliminar colas. Aprenda a insertar, leer, obtener y eliminar mensajes con la biblioteca de cliente de Azure Storage para Java.
author: normesta
ms.author: normesta
ms.reviewer: dineshm
ms.date: 08/19/2020
ms.topic: how-to
ms.service: storage
ms.subservice: queues
ms.custom: devx-track-java
ms.openlocfilehash: 25734b7f4073895da832b3d020679c6f7b36a080
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128665860"
---
# <a name="how-to-use-queue-storage-from-java"></a>Uso de Queue Storage de Java

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

## <a name="overview"></a>Información general

Esta guía muestra cómo codificar algunos escenarios comunes a través del servicio Azure Queue Storage. Los ejemplos están escritos en Java y utilizan el [SDK de Azure Storage para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage). Entre los escenarios descritos se incluyen **insertar**, **inspeccionar**, **obtener** y **eliminar** mensajes de la cola. También se incluye código para **crear** y **eliminar** colas. Para obtener más información sobre las colas, consulte la sección [Pasos siguientes](#next-steps) .

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Creación de una aplicación Java

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

En primer lugar, compruebe que el sistema de desarrollo cumple los requisitos previos enumerados en [Biblioteca cliente de Azure Queue Storage v12 para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-queue).

Para crear una aplicación Java denominada `queues-how-to-v12`:

1. En una ventana de consola (por ejemplo, cmd, PowerShell o Bash), use Maven para crear una nueva aplicación de consola con el nombre `queues-how-to-v12`. Escriba el siguiente comando `mvn` para crear un proyecto "Hola mundo" en Java.

   ```bash
    mvn archetype:generate \
        --define interactiveMode=n \
        --define groupId=com.queues.howto \
        --define artifactId=queues-howto-v12 \
        --define archetypeArtifactId=maven-archetype-quickstart \
        --define archetypeVersion=1.4
   ```

   ```powershell
   mvn archetype:generate `
       --define interactiveMode=n `
       --define groupId=com.queues.howto `
       --define artifactId=queues-howto-v12 `
       --define archetypeArtifactId=maven-archetype-quickstart `
       --define archetypeVersion=1.4
   ```

1. La salida a partir de la generación del proyecto debe ser similar a la siguiente:

   ```console
   [INFO] Scanning for projects...
   [INFO]
   [INFO] ------------------< org.apache.maven:standalone-pom >-------------------
   [INFO] Building Maven Stub Project (No POM) 1
   [INFO] --------------------------------[ pom ]---------------------------------
   [INFO]
   [INFO] >>> maven-archetype-plugin:3.1.2:generate (default-cli) > generate-sources @ standalone-pom >>>
   [INFO]
   [INFO] <<< maven-archetype-plugin:3.1.2:generate (default-cli) < generate-sources @ standalone-pom <<<
   [INFO]
   [INFO]
   [INFO] --- maven-archetype-plugin:3.1.2:generate (default-cli) @ standalone-pom ---
   [INFO] Generating project in Batch mode
   [INFO] ----------------------------------------------------------------------------
   [INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
   [INFO] ----------------------------------------------------------------------------
   [INFO] Parameter: groupId, Value: com.queues.howto
   [INFO] Parameter: artifactId, Value: queues-howto-v12
   [INFO] Parameter: version, Value: 1.0-SNAPSHOT
   [INFO] Parameter: package, Value: com.queues.howto
   [INFO] Parameter: packageInPathFormat, Value: com/queues/howto
   [INFO] Parameter: version, Value: 1.0-SNAPSHOT
   [INFO] Parameter: package, Value: com.queues.howto
   [INFO] Parameter: groupId, Value: com.queues.howto
   [INFO] Parameter: artifactId, Value: queues-howto-v12
   [INFO] Project created from Archetype in dir: C:\queues\queues-howto-v12
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  6.775 s
   [INFO] Finished at: 2020-08-17T15:27:31-07:00
   [INFO] ------------------------------------------------------------------------
   ```

1. Cambie al directorio `queues-howto-v12` recién creado.

   ```console
   cd queues-howto-v12
   ```

### <a name="install-the-package"></a>Instalar el paquete

Abra el archivo `pom.xml` en el editor de texto. Agregue el siguiente elemento de dependencia al grupo de dependencias.

```xml
<dependency>
  <groupId>com.azure</groupId>
  <artifactId>azure-storage-queue</artifactId>
  <version>12.6.0</version>
</dependency>
```

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

En primer lugar, compruebe que el sistema de desarrollo cumple los requisitos previos enumerados en el [SDK de Azure Storage para Java v8](https://github.com/azure/azure-storage-java). Siga las instrucciones para descargar e instalar las bibliotecas de Azure Storage para Java. Después, puede crear una aplicación de Java con los ejemplos de este artículo.

---

## <a name="configure-your-application-to-access-queue-storage"></a>Configuración de la aplicación para obtener acceso a Queue Storage

Agregue las siguientes instrucciones de importación en la parte superior del archivo Java en el que desea utilizar las API de Azure Storage para obtener acceso a las colas:

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_ImportStatements":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

---

## <a name="set-up-an-azure-storage-connection-string"></a>Configuración de una cadena de conexión de Azure Storage

Un cliente de Azure Storage usa una cadena de conexión de almacenamiento para tener acceso a los servicios de administración de datos. Obtenga el nombre y la clave de acceso principal de la cuenta de almacenamiento que aparecen en [Azure Portal](https://portal.azure.com). Úselos como los valores `AccountName` y `AccountKey` en la cadena de conexión. En este ejemplo se muestra cómo puede declarar un campo estático para mantener la cadena de conexión:

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_ConnectionString":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

```java
// Define the connection-string with your values.
final String storageConnectionString =
    "DefaultEndpointsProtocol=https;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Puede almacenar esta cadena en el archivo de configuración de servicio denominado `ServiceConfiguration.cscfg`. En el caso de las aplicaciones que se ejecutan en un rol de Microsoft Azure, llame a `RoleEnvironment.getConfigurationSettings` para acceder a la cadena de conexión. A continuación se muestra un ejemplo de cómo obtener la cadena de conexión desde un elemento de `Setting` denominado `StorageConnectionString`:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

---

En los ejemplos siguientes se supone que tiene un objeto `String` que contiene la cadena de conexión de almacenamiento.

## <a name="how-to-create-a-queue"></a>Procedimientos: Creación de una cola

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

Un objeto `QueueClient` contiene las operaciones para interactuar con una cola. El código siguiente crea un objeto `QueueClient`. Use el objeto `QueueClient` para crear la cola que desea utilizar.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_CreateQueue":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

Los objetos `CloudQueueClient` le permiten obtener objetos de referencia para las colas. En el código siguiente se crea un objeto `CloudQueueClient` que proporciona una referencia a la cola que se desea utilizar. En caso de que la cola no exista todavía, es posible crearla.

> [!NOTE]
> Hay otras maneras de crear objetos `CloudStorageAccount`. Para obtener más información, consulte `CloudStorageAccount` en la [referencia del SDK de cliente de Azure Storage](https://azure.github.io/azure-sdk-for-java/storage.html)).

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create the queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference to a queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create the queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

## <a name="how-to-add-a-message-to-a-queue"></a>Procedimientos: un mensaje a una cola

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

Para insertar un mensaje en una cola existente, llame al método `sendMessage`. Un mensaje puede ser una cadena (en formato UTF-8) o una matriz de bytes. Este es el código que envía un mensaje de cadena a la cola.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_AddMessage":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

Para insertar un mensaje en una cola existente, cree en primer lugar un nuevo `CloudQueueMessage`. A continuación, llame al método `addMessage`. Se puede crear un objeto `CloudQueueMessage` a partir de una cadena (en formato UTF-8) o de una matriz de bytes. A continuación se muestra el código con el que se crea una cola (si no existe) y se inserta el mensaje `Hello, World`.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

## <a name="how-to-peek-at-the-next-message"></a>Procedimientos: siguiente mensaje

Puede inspeccionar el mensaje situado en la parte delantera de una cola, sin quitarlo de la cola, mediante una llamada a `peekMessage`:

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_PeekMessage":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at the next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output the message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedimientos: contenido de un mensaje en cola

Puede cambiar el contenido de un mensaje local en la cola. Si el mensaje representa una tarea de trabajo, puede usar esta característica para actualizar el estado. El siguiente código actualiza el mensaje de la cola con contenido nuevo y amplía el tiempo de espera de la visibilidad en 30 segundos más. La extensión del tiempo de espera de visibilidad proporciona al cliente otros 30 segundos para seguir trabajando en el mensaje. También puede mantener un número de reintentos. Si el mensaje se vuelve a intentar más de *n* veces, lo eliminaría. Este escenario proporciona protección frente a un mensaje que produce un error en la aplicación cada vez que se procesa.

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

El siguiente código de ejemplo busca en la cola de mensajes, encuentra el primer mensaje cuyo contenido coincide con una cadena de búsqueda, modifica el contenido del mensaje y se cierra.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_UpdateSearchMessage":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

El siguiente código de ejemplo busca en la cola de mensajes, encuentra el primer mensaje cuyo contenido coincide con `Hello, world`, modifica el contenido del mensaje y se cierra.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // The maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through the messages in the queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify the content of the first matching message.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

El siguiente código de ejemplo actualiza únicamente el primer mensaje visible de la cola.

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_UpdateFirstMessage":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify the message content.
        message.setMessageContent("Updated contents.");
        // Set it to be visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update the message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

## <a name="how-to-get-the-queue-length"></a>Procedimientos: la longitud de la cola

Puede obtener una estimación del número de mensajes existentes en una cola.

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

El método `getProperties` devuelve varios valores, incluido el número de mensajes que hay actualmente en una cola. El recuento solo es aproximado, ya que se pueden agregar o borrar mensajes después de que el servicio de cola haya respondido su solicitud. El método `getApproximateMessageCount` devuelve el último valor recuperado por la llamada a `getProperties`, sin llamar a Queue Storage.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_GetQueueLength":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

El método `downloadAttributes` recupera varios valores, incluido el número de mensajes que hay actualmente en una cola. El recuento solo es aproximado, ya que se pueden agregar o borrar mensajes después de que el servicio de cola haya respondido su solicitud. El método `getApproximateMessageCount` devuelve el último valor recuperado por la llamada a `downloadAttributes`, sin llamar a Queue Storage.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download the approximate message count from the server.
    queue.downloadAttributes();

    // Retrieve the newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display the queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

## <a name="how-to-dequeue-the-next-message"></a>Procedimientos: Extracción del siguiente mensaje de la cola

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

El código extrae un mensaje de una cola en dos pasos. Al llamar a `receiveMessage`, obtiene el siguiente mensaje de una cola. Un mensaje devuelto por `receiveMessage` se hace invisible a cualquier otro código de lectura de mensajes de esta cola. De forma predeterminada, este mensaje permanece invisible durante 30 segundos. Para terminar quitando el mensaje de la cola, también debe llamar a `deleteMessage`. Si el código no puede procesar un mensaje, este proceso de dos pasos garantiza que pueda obtener el mismo mensaje e intentarlo de nuevo. El código siguiente llama a `deleteMessage` justo después de haberse procesado el mensaje.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_DequeueMessage":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

El código extrae un mensaje de una cola en dos pasos. Al llamar a `retrieveMessage`, obtiene el siguiente mensaje de una cola. Un mensaje devuelto por `retrieveMessage` se hace invisible a cualquier otro código de lectura de mensajes de esta cola. De forma predeterminada, este mensaje permanece invisible durante 30 segundos. Para terminar quitando el mensaje de la cola, también debe llamar a `deleteMessage`. Si el código no puede procesar un mensaje, este proceso de dos pasos garantiza que pueda obtener el mismo mensaje e intentarlo de nuevo. El código siguiente llama a `deleteMessage` justo después de haberse procesado el mensaje.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process the message in less than 30 seconds, and then delete the message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

## <a name="additional-options-for-dequeuing-messages"></a>Opciones adicionales para quitar mensajes de la cola

Hay dos formas de personalizar la recuperación de mensajes de una cola. En primer lugar, obtenga un lote de mensajes (hasta 32). En segundo lugar, establezca r un tiempo de espera de la invisibilidad más largo o más corto para que el código disponga de más o menos tiempo para procesar cada mensaje.

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

El siguiente ejemplo de código utiliza el método `receiveMessages` para obtener veinte mensajes en una llamada. A continuación, procesa cada mensaje con un bucle `for`. También establece el tiempo de espera de la invisibilidad en cinco minutos (300 segundos) para cada mensaje. El tiempo de espera se inicia para todos los mensajes al mismo tiempo. Cuando hayan transcurrido cinco minutos desde la llamada a `receiveMessages`, todos los mensajes que no se eliminen volverán a estar visibles.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_DequeueMessages":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

El siguiente ejemplo de código utiliza el método `retrieveMessages` para obtener veinte mensajes en una llamada. A continuación, procesa cada mensaje con un bucle `for`. También establece el tiempo de espera de la invisibilidad en cinco minutos (300 segundos) para cada mensaje. El tiempo de espera se inicia para todos los mensajes al mismo tiempo. Cuando hayan transcurrido cinco minutos desde la llamada a `retrieveMessages`, todos los mensajes que no se eliminen volverán a estar visibles.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

## <a name="how-to-list-the-queues"></a>Procedimientos: Enumeración de las colas

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

Para obtener una lista de las colas actuales, llame al método `QueueServiceClient.listQueues()` , el cual devolverá una colección de objetos `QueueItem`.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_ListQueues":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

Para obtener una lista de las colas actuales, llame al método `CloudQueueClient.listQueues()` , el cual devolverá una colección de objetos `CloudQueue`.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through the collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

## <a name="how-to-delete-a-queue"></a>Procedimientos: Eliminación de una cola

# <a name="java-v12-sdk"></a>[SDK de Java v12](#tab/java)

Para eliminar una cola y todos los mensajes contenidos en ella, llame al método `delete` en el objeto de cola `QueueClient`.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_DeleteMessageQueue":::

# <a name="java-v8-sdk"></a>[SDK de Java v8](#tab/java8)

Para eliminar una cola y todos los mensajes contenidos en ella, llame al método `deleteIfExists` en el objeto de cola `CloudQueue`.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete the queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

---

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que está familiarizado con los aspectos básicos de Queue Storage, utilice estos vínculos para obtener más información acerca de tareas de almacenamiento más complejas.

- [SDK de Azure Storage para Java](https://github.com/Azure/Azure-SDK-for-Java)
- [Referencia del SDK del cliente de Azure Storage](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage)
- [API REST de servicios de Azure Storage](/rest/api/storageservices/)
- [Blog del equipo de Azure Storage](https://techcommunity.Microsoft.com/t5/Azure-storage/bg-p/azurestorageblog)
