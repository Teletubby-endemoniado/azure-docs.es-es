---
title: Introducción a las colas de Azure Service Bus (Azure.Messaging.ServiceBus)
description: En este tutorial, creará una aplicación de consola de C# de .NET Core para enviar mensajes a una cola de Service Bus y recibir mensajes desde ella.
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.date: 10/11/2021
ms.custom: contperf-fy22q2
ms.openlocfilehash: c7e0ddee8e42f76c034ce79c7200e48fcb8bae04
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132555914"
---
# <a name="get-started-with-azure-service-bus-queues-net"></a>Introducción a las colas de Azure Service Bus (.NET)
En este inicio rápido, hará lo siguiente:

1. Creación de un espacio de nombres de Service Bus mediante Azure Portal
2. Creación de una cola de Service Bus mediante Azure Portal.
3. Escriba una aplicación de consola de .NET Core para enviar un conjunto de mensajes a la cola.
4. Escriba una aplicación de consola de .NET Core para recibir esos mensajes de la cola.

> [!NOTE]
> En este inicio rápido se proporcionan instrucciones paso a paso para un escenario sencillo de envío de un lote de mensajes a una cola de Service Bus y la recepción de estos. Puede encontrar más ejemplos pregenerados de .NET para Azure Service Bus en el [repositorio del SDK de Azure para .NET en GitHub](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/servicebus/Azure.Messaging.ServiceBus/samples). 

## <a name="prerequisites"></a>Prerrequisitos
Si no está familiarizado con el servicio, consulte la [información general sobre Service Bus](service-bus-messaging-overview.md) antes de seguir este artículo de inicio rápido. 

- **Suscripción de Azure**. Para usar los servicios de Azure, entre los que se incluye Azure Service Bus, se necesita una suscripción. Si no tiene una cuenta de Azure existente, puede registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/).
- **Microsoft Visual Studio 2019**. La biblioteca cliente de Azure Service Bus usa las nuevas características que se introdujeron en C# 8.0.  Aunque puede seguir usando la biblioteca con versiones anteriores de C#, la nueva sintaxis no estará disponible. Para usar la sintaxis completa, se recomienda realizar la compilación con el SDK para .NET Core 3.0 o posterior y la versión de lenguaje establecida en `latest`. Si usa Visual Studio, las versiones anteriores a Visual Studio 2019 no son compatibles con las herramientas necesarias para compilar proyectos de C# 8.0.


[!INCLUDE [service-bus-create-namespace-portal](./includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](./includes/service-bus-create-queue-portal.md)]


## <a name="send-messages-to-the-queue"></a>Envío de mensajes a la cola
En esta sección se muestra cómo crear una aplicación de consola de .NET Core para mensajes a una cola de Service Bus. 

### <a name="create-a-console-application"></a>Creación de una aplicación de consola

1. Inicie Visual Studio 2019. 
1. Seleccione **Crear un proyecto**. 
1. En el cuadro de diálogo **Crear un nuevo proyecto**, siga estos pasos: Si no ve este cuadro de diálogo, seleccione **Archivo** en el menú, seleccione **Nuevo** y, después, seleccione **Proyecto**. 
    1. Seleccione **C#** como lenguaje de programación.
    1. Seleccione **Consola** como tipo de aplicación. 
    1. Seleccione **Aplicación de consola** en la lista de resultados. 
    1. Después, seleccione **Siguiente**. 

        :::image type="content" source="./media/service-bus-dotnet-get-started-with-queues/new-send-project.png" alt-text="Imagen que muestra el cuadro de diálogo Crear un proyecto con C# y Consola seleccionados":::
1. Escriba **QueueSender** como nombre del proyecto, **ServiceBusQueueQuickStart** como nombre de la solución y, a continuación, seleccione **Siguiente**. 

    :::image type="content" source="./media/service-bus-dotnet-get-started-with-queues/project-solution-names.png" alt-text="Imagen que muestra los nombres de solución y proyecto en el cuadro de diálogo Configurar el nuevo proyecto ":::
1. En la página **Información adicional**, seleccione **Crear** para crear la solución y el proyecto. 

### <a name="add-the-service-bus-nuget-package"></a>Agregar el paquete NuGet de Service Bus

1. Seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes** en el menú. 
1. Ejecute el siguiente comando para instalar el paquete NuGet **Azure.Messaging.ServiceBus**:

    ```cmd
    Install-Package Azure.Messaging.ServiceBus
    ```

### <a name="add-code-to-send-messages-to-the-queue"></a>Incorporación de código para enviar mensajes a la cola

1. En **Program.cs**, agregue las siguientes instrucciones `using` en la parte superior de la definición del espacio de nombres, antes de la declaración de clase.

    ```csharp
    using System.Threading.Tasks;
    using Azure.Messaging.ServiceBus;
    ```
2. Dentro de la clase `Program`, declare las siguientes propiedades, justo antes del método `Main`. 

    Reemplace `<NAMESPACE CONNECTION STRING>` por la cadena de conexión principal del espacio de nombres de Service Bus. Además, reemplace `<QUEUE NAME>` por el nombre de la cola.

    ```csharp

    // connection string to your Service Bus namespace
    static string connectionString = "<NAMESPACE CONNECTION STRING>";

    // name of your Service Bus queue
    static string queueName = "<QUEUE NAME>";

    // the client that owns the connection and can be used to create senders and receivers
    static ServiceBusClient client;

    // the sender used to publish messages to the queue
    static ServiceBusSender sender;

    // number of messages to be sent to the queue
    private const int numOfMessages = 3;

    ```
3. Reemplace el código del método `Main` por el código siguiente. Consulte los comentarios del código para obtener más información sobre este. Estos son los pasos importantes del código.  
    1. Crea un objeto [ServiceBusClient](/dotnet/api/azure.messaging.servicebus.servicebusclient) mediante la cadena de conexión principal al espacio de nombres. 
    1. Invoca al método [CreateSender](/dotnet/api/azure.messaging.servicebus.servicebusclient.createsender) en el objeto [ServiceBusClient](/dotnet/api/azure.messaging.servicebus.servicebusclient) para crear un objeto [ServiceBusSender](/dotnet/api/azure.messaging.servicebus.servicebussender) para la cola específica de Service Bus.     
    1. Crea un objeto [ServiceBusMessageBatch](/dotnet/api/azure.messaging.servicebus.servicebusmessagebatch) mediante el método [ServiceBusSender.CreateMessageBatchAsync](/dotnet/api/azure.messaging.servicebus.servicebussender.createmessagebatchasync).
    1. Agrega mensajes al lote mediante [ServiceBusMessageBatch.TryAddMessage](/dotnet/api/azure.messaging.servicebus.servicebusmessagebatch.tryaddmessage). 
    1. Envía el lote de mensajes a la cola de Service Bus mediante el método [ServiceBusSender.SendMessagesAsync](/dotnet/api/azure.messaging.servicebus.servicebussender.sendmessagesasync).

        ```csharp
        static async Task Main()
        {
            // The Service Bus client types are safe to cache and use as a singleton for the lifetime
            // of the application, which is best practice when messages are being published or read
            // regularly.
            //
            // Create the clients that we'll use for sending and processing messages.
            client = new ServiceBusClient(connectionString);
            sender = client.CreateSender(queueName);
    
            // create a batch 
            using ServiceBusMessageBatch messageBatch = await sender.CreateMessageBatchAsync();
    
            for (int i = 1; i <= numOfMessages; i++)
            {
                // try adding a message to the batch
                if (!messageBatch.TryAddMessage(new ServiceBusMessage($"Message {i}")))
                {
                    // if it is too large for the batch
                    throw new Exception($"The message {i} is too large to fit in the batch.");
                }
            }
    
            try
            {
                // Use the producer client to send the batch of messages to the Service Bus queue
                await sender.SendMessagesAsync(messageBatch);
                Console.WriteLine($"A batch of {numOfMessages} messages has been published to the queue.");
            }
            finally
            {
                // Calling DisposeAsync on client types is required to ensure that network
                // resources and other unmanaged objects are properly cleaned up.
                await sender.DisposeAsync();
                await client.DisposeAsync();
            }
    
            Console.WriteLine("Press any key to end the application");
            Console.ReadKey();
        }    
        ```    
1. Este es el aspecto que debería tener el archivo Program.cs: 
    
    ```csharp
    using System;
    using System.Threading.Tasks;
    using Azure.Messaging.ServiceBus;
    
    namespace QueueSender
    {
        class Program
        {
            // connection string to your Service Bus namespace
            static string connectionString = "<NAMESPACE CONNECTION STRING>";

            // name of your Service Bus queue
            static string queueName = "<QUEUE NAME>";
    
            // the client that owns the connection and can be used to create senders and receivers
            static ServiceBusClient client;
    
            // the sender used to publish messages to the queue
            static ServiceBusSender sender;
    
            // number of messages to be sent to the queue
            private const int numOfMessages = 3;
    
            static async Task Main()
            {
                // The Service Bus client types are safe to cache and use as a singleton for the lifetime
                // of the application, which is best practice when messages are being published or read
                // regularly.
                //
                // Create the clients that we'll use for sending and processing messages.
                client = new ServiceBusClient(connectionString);
                sender = client.CreateSender(queueName);
    
                // create a batch 
                using ServiceBusMessageBatch messageBatch = await sender.CreateMessageBatchAsync();
    
                for (int i = 1; i <= numOfMessages; i++)
                {
                    // try adding a message to the batch
                    if (!messageBatch.TryAddMessage(new ServiceBusMessage($"Message {i}")))
                    {
                        // if it is too large for the batch
                        throw new Exception($"The message {i} is too large to fit in the batch.");
                    }
                }
    
                try
                {
                    // Use the producer client to send the batch of messages to the Service Bus queue
                    await sender.SendMessagesAsync(messageBatch);
                    Console.WriteLine($"A batch of {numOfMessages} messages has been published to the queue.");
                }
                finally
                {
                    // Calling DisposeAsync on client types is required to ensure that network
                    // resources and other unmanaged objects are properly cleaned up.
                    await sender.DisposeAsync();
                    await client.DisposeAsync();
                }
    
                Console.WriteLine("Press any key to end the application");
                Console.ReadKey();
            }
        }
    }   
    ``` 
1. Reemplace `<NAMESPACE CONNECTION STRING>` por la cadena de conexión principal del espacio de nombres de Service Bus. Además, reemplace `<QUEUE NAME>` por el nombre de la cola.
1. Compile el proyecto y asegúrese de que no hay errores. 
1. Ejecute el programa y espere el mensaje de confirmación.
    
    ```bash
    A batch of 3 messages has been published to the queue
    ```
1. En Azure Portal, haga lo siguiente:
    1. Vaya al espacio de nombres de Service Bus. 
    1. En la página **Información general**, seleccione la cola en el panel inferior central. 
    
        :::image type="content" source="./media/service-bus-dotnet-get-started-with-queues/select-queue.png" alt-text="Imagen en la que se muestra Espacio de nombres de Service Bus en Azure Portal con la cola seleccionada" lightbox="./media/service-bus-dotnet-get-started-with-queues/select-queue.png":::.
    1. Observe los valores en la sección **Información esencial**.

    :::image type="content" source="./media/service-bus-dotnet-get-started-with-queues/sent-messages-essentials.png" alt-text="Imagen que muestra el número de mensajes recibidos y el tamaño de la cola" lightbox="./media/service-bus-dotnet-get-started-with-queues/sent-messages-essentials.png":::.

    Observe los valores siguientes:
    - El valor del recuento de mensajes **Activos** de la cola ahora es **3**. Cada vez que se ejecuta la aplicación de remitente sin recuperar los mensajes, este valor aumenta en 3.
    - El **tamaño actual** de la cola aumenta cada vez que la aplicación agrega mensajes a la misma.
    - En el gráfico **Mensajes** de la sección **Métricas** inferior, puede ver que hay tres mensajes entrantes para la cola. 

## <a name="receive-messages-from-the-queue"></a>Recepción de mensajes de la cola
En esta sección, creará una aplicación de consola de .NET Core que recibe mensajes de la cola. 

### <a name="create-a-project-for-the-receiver"></a>Creación de un proyecto para el destinatario

1. En la ventana del Explorador de soluciones, haga clic con el botón derecho en la solución **ServiceBusQueueQuickStart**, haga clic en **Agregar** y seleccione **Nuevo proyecto**. 
1. Seleccione **Aplicación de consola** y elija **Siguiente**. 
1. Escriba **QueueReceiver** en **Nombre de proyecto** y seleccione **Crear**. 
1. En la ventana del **Explorador de soluciones**, haga clic con el botón derecho en **QueueReceiver** y seleccione **Set as a Startup Project** (Establecer como proyecto de inicio). 

### <a name="add-the-service-bus-nuget-package"></a>Agregar el paquete NuGet de Service Bus

1. Seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes** en el menú. 
1. En la ventana de la **Consola del Administrador de paquetes**, confirme que **QueueReceiver** está seleccionado como **Proyecto predeterminado**. Si no es así, use la lista desplegable para seleccionar **QueueReceiver**.

    :::image type="content" source="./media/service-bus-dotnet-get-started-with-queues/package-manager-console.png" alt-text="Captura de pantalla que muestra el proyecto QueueReceiver seleccionado en la consola del administrador de paquetes":::
1. Ejecute el siguiente comando para instalar el paquete NuGet **Azure.Messaging.ServiceBus**:

    ```cmd
    Install-Package Azure.Messaging.ServiceBus
    ```

### <a name="add-the-code-to-receive-messages-from-the-queue"></a>Adición del código para recibir mensajes de la cola
En esta sección agregará código para recuperar mensajes de la cola.

1. En **Program.cs**, agregue las siguientes instrucciones `using` en la parte superior de la definición del espacio de nombres, antes de la declaración de clase.

    ```csharp
    using System.Threading.Tasks;
    using Azure.Messaging.ServiceBus;
    ```
2. Dentro de la clase `Program`, declare las siguientes propiedades, justo antes del método `Main`. 

    Reemplace `<NAMESPACE CONNECTION STRING>` por la cadena de conexión principal del espacio de nombres de Service Bus. Además, reemplace `<QUEUE NAME>` por el nombre de la cola.

    ```csharp
    // connection string to your Service Bus namespace
    static string connectionString = "<NAMESPACE CONNECTION STRING>";

    // name of your Service Bus queue
    static string queueName = "<QUEUE NAME>";

    // the client that owns the connection and can be used to create senders and receivers
    static ServiceBusClient client;

    // the processor that reads and processes messages from the queue
    static ServiceBusProcessor processor;
    ```
3. Agregue los métodos siguientes a la clase `Program` para controlar los mensajes y los errores recibidos. 

    ```csharp
    // handle received messages
    static async Task MessageHandler(ProcessMessageEventArgs args)
    {
        string body = args.Message.Body.ToString();
        Console.WriteLine($"Received: {body}");

        // complete the message. messages is deleted from the queue. 
        await args.CompleteMessageAsync(args.Message);
    }

    // handle any errors when receiving messages
    static Task ErrorHandler(ProcessErrorEventArgs args)
    {
        Console.WriteLine(args.Exception.ToString());
        return Task.CompletedTask;
    }
    ```
4. Reemplace el código del método `Main` por el código siguiente. Consulte los comentarios del código para obtener más información sobre este. Estos son los pasos importantes del código. 
    1. Crea un objeto [ServiceBusClient](/dotnet/api/azure.messaging.servicebus.servicebusclient) mediante la cadena de conexión principal al espacio de nombres. 
    1. Invoca al método [CreateProcessor](/dotnet/api/azure.messaging.servicebus.servicebusclient.createprocessor) en el objeto [ServiceBusClient](/dotnet/api/azure.messaging.servicebus.servicebusclient) para crear un objeto [ServiceBusProcessor](/dotnet/api/azure.messaging.servicebus.servicebusprocessor) para la cola de Service Bus especificada. 
    1. Especifica los controladores para los eventos [ProcessMessageAsync](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.processmessageasync) y [ProcessErrorAsync](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.processerrorasync) del objeto [ServiceBusProcessor](/dotnet/api/azure.messaging.servicebus.servicebusprocessor). 
    1. Inicia el procesamiento de mensajes; para ello, invoca el método [StartProcessingAsync](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.startprocessingasync) en el objeto [ServiceBusProcessor](/dotnet/api/azure.messaging.servicebus.servicebusprocessor). 
    1. Cuando el usuario presiona una tecla para finalizar el procesamiento, invoca el método [StopProcessingAsync](/dotnet/api/azure.messaging.servicebus.servicebusprocessor.stopprocessingasync) en el objeto [ServiceBusProcessor](/dotnet/api/azure.messaging.servicebus.servicebusprocessor). 

        Para más información, consulte los comentarios del código.

        ```csharp
                static async Task Main()
                {
                    // The Service Bus client types are safe to cache and use as a singleton for the lifetime
                    // of the application, which is best practice when messages are being published or read
                    // regularly.
                    //
        
                    // Create the client object that will be used to create sender and receiver objects
                    client = new ServiceBusClient(connectionString);
        
                    // create a processor that we can use to process the messages
                    processor = client.CreateProcessor(queueName, new ServiceBusProcessorOptions());
        
                    try
                    {
                        // add handler to process messages
                        processor.ProcessMessageAsync += MessageHandler;
        
                        // add handler to process any errors
                        processor.ProcessErrorAsync += ErrorHandler;
        
                        // start processing 
                        await processor.StartProcessingAsync();
        
                        Console.WriteLine("Wait for a minute and then press any key to end the processing");
                        Console.ReadKey();
        
                        // stop processing 
                        Console.WriteLine("\nStopping the receiver...");
                        await processor.StopProcessingAsync();
                        Console.WriteLine("Stopped receiving messages");
                    }
                    finally
                    {
                        // Calling DisposeAsync on client types is required to ensure that network
                        // resources and other unmanaged objects are properly cleaned up.
                        await processor.DisposeAsync();
                        await client.DisposeAsync();
                    }
                }
        ```
4. Este es el aspecto que debería tener `Program.cs`:  

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Azure.Messaging.ServiceBus;
    
    namespace QueueReceiver
    {
        class Program
        {
            // connection string to your Service Bus namespace
            static string connectionString = "<NAMESPACE CONNECTION STRING>";
    
            // name of your Service Bus queue
            static string queueName = "<QUEUE NAME>";
    
    
            // the client that owns the connection and can be used to create senders and receivers
            static ServiceBusClient client;
    
            // the processor that reads and processes messages from the queue
            static ServiceBusProcessor processor;
    
            // handle received messages
            static async Task MessageHandler(ProcessMessageEventArgs args)
            {
                string body = args.Message.Body.ToString();
                Console.WriteLine($"Received: {body}");
    
                // complete the message. messages is deleted from the queue. 
                await args.CompleteMessageAsync(args.Message);
            }
    
            // handle any errors when receiving messages
            static Task ErrorHandler(ProcessErrorEventArgs args)
            {
                Console.WriteLine(args.Exception.ToString());
                return Task.CompletedTask;
            }
    
            static async Task Main()
            {
                // The Service Bus client types are safe to cache and use as a singleton for the lifetime
                // of the application, which is best practice when messages are being published or read
                // regularly.
                //
    
                // Create the client object that will be used to create sender and receiver objects
                client = new ServiceBusClient(connectionString);
    
                // create a processor that we can use to process the messages
                processor = client.CreateProcessor(queueName, new ServiceBusProcessorOptions());
    
                try
                {
                    // add handler to process messages
                    processor.ProcessMessageAsync += MessageHandler;
    
                    // add handler to process any errors
                    processor.ProcessErrorAsync += ErrorHandler;
    
                    // start processing 
                    await processor.StartProcessingAsync();
    
                    Console.WriteLine("Wait for a minute and then press any key to end the processing");
                    Console.ReadKey();
    
                    // stop processing 
                    Console.WriteLine("\nStopping the receiver...");
                    await processor.StopProcessingAsync();
                    Console.WriteLine("Stopped receiving messages");
                }
                finally
                {
                    // Calling DisposeAsync on client types is required to ensure that network
                    // resources and other unmanaged objects are properly cleaned up.
                    await processor.DisposeAsync();
                    await client.DisposeAsync();
                }
            }
        }
    }
    ```
1. Reemplace `<NAMESPACE CONNECTION STRING>` por la cadena de conexión principal del espacio de nombres de Service Bus. Además, reemplace `<QUEUE NAME>` por el nombre de la cola. 
1. Compile el proyecto y asegúrese de que no hay errores.
1. Ejecute la aplicación del destinatario. Debería ver los mensajes recibidos. Presione cualquier tecla para detener el receptor y la aplicación. 

    ```console
    Wait for a minute and then press any key to end the processing
    Received: Message 1
    Received: Message 2
    Received: Message 3
    
    Stopping the receiver...
    Stopped receiving messages
    ```
1. Vuelva a consultar el portal. Espere unos minutos y actualice la página si no ve `0` para los mensajes **Activos**. 

    - El recuento de mensajes **Activos** y los valores de **Tamaño actual** ahora son **0**.
    - En el gráfico **Mensajes** de la sección **Métricas** inferior, puede ver que hay tres mensajes entrantes y otros tres salientes para la cola. 
    
        :::image type="content" source="./media/service-bus-dotnet-get-started-with-queues/queue-messages-size-final.png" alt-text="Mensajes activos y tamaño después de la recepción" lightbox="./media/service-bus-dotnet-get-started-with-queues/queue-messages-size-final.png":::


## <a name="next-steps"></a>Pasos siguientes
Consulte la documentación y los ejemplos siguientes:

- [Biblioteca cliente de Azure Service Bus para .NET: léame](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/servicebus/Azure.Messaging.ServiceBus)
- [Ejemplos en GitHub](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/servicebus/Azure.Messaging.ServiceBus/samples)
- [Referencia de API de .NET](/dotnet/api/azure.messaging.servicebus)
