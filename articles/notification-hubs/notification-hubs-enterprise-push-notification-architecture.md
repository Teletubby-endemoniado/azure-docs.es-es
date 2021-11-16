---
title: Arquitectura de inserción empresarial de Notification Hubs
description: Aprenda a usar Azure Notification Hubs en un entorno de empresa
services: notification-hubs
documentationcenter: ''
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 9b521427b5fc3fd0eb3f0e91e8122d3da543296f
ms.sourcegitcommit: 4cd97e7c960f34cb3f248a0f384956174cdaf19f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "132027667"
---
# <a name="enterprise-push-architectural-guidance"></a>Instrucciones sobre arquitectura de inserción empresarial

En la actualidad, las empresas están pasando cada vez más hacia la creación de aplicaciones móviles para sus usuarios finales (externos) o para los empleados (internos). Disponen de sistemas back-end como grandes sistemas (mainframes) o algunas aplicaciones de línea de negocio (LoB) que deben estar integradas en la arquitectura de aplicaciones móviles. En esta guía se habla acerca de la mejor manera de realizar esta integración y recomendaremos soluciones posibles a escenarios comunes.

Un requisito frecuente es el envío de notificaciones de inserción a los usuarios a través de su aplicación móvil cuando se produce un evento de interés en los sistemas back-end. Por ejemplo, el cliente de un banco que tiene la aplicación del banco en un iPhone quiere recibir una notificación cuando se efectúe un cargo por encima de un importe determinado desde su cuenta, o un escenario de intranet en el que un empleado del departamento financiero que tiene una aplicación de aprobación de presupuestos en un Windows Phone quiere recibir una notificación cuando obtenga una solicitud de aprobación.

El procesamiento de la cuenta bancaria o de la solicitud de aprobación se realiza probablemente en algún sistema back-end que debe iniciar una inserción al usuario. Podría haber muchos sistemas back-end de este tipo que deben crear todos el mismo tipo de lógica para realizar la inserción cuando un evento desencadena una notificación. Aquí la complejidad reside en la integración de varios back-ends juntos con un único sistema de inserción donde los usuarios finales podrían haberse suscrito a diferentes notificaciones e incluso podría haber varias aplicaciones móviles. Por ejemplo, en el caso de aplicaciones móviles de intranet donde una aplicación móvil puede querer recibir notificaciones de varios de estos sistemas back-end. Los sistemas back-end no conocen o no necesitan conocer la semántica/tecnología de inserción, de modo que una solución común aquí ha sido tradicionalmente introducir un componente que sondea los sistemas back-end en busca de eventos de interés y que es responsable de enviar los mensajes de inserción al cliente.

Una solución mejor es usar el modelo de temas y suscripciones de Azure Service Bus, que reduce la complejidad y hará que la solución sea escalable.

Esta es la arquitectura general de la solución (generalizada con varias aplicaciones móviles pero igualmente aplicable cuando solo hay una aplicación móvil).

## <a name="architecture"></a>Architecture

![Diagrama de la arquitectura empresarial que muestra el flujo a través de eventos, suscripciones y mensajes de inserción.][1]

La pieza clave en este diagrama arquitectónico es Azure Service Bus que proporciona un modelo de programación de temas y suscripciones (se puede obtener más información al respecto en [Programación Pub/Sub de Service Bus]). El receptor, en este caso el back-end móvil (normalmente el [servicio móvil de Azure], que inicia una inserción en las aplicaciones móviles) no recibe los mensajes directamente de los sistemas back-end, sino de una capa de abstracción intermedia que proporciona [Azure Service Bus] que permite que el back-end móvil reciba mensajes de uno o varios sistemas back-end. Se debe crear un tema de Service Bus para cada uno de los sistemas back-end, por ejemplo, Cuentas, RR.HH., Finanzas, que son básicamente "temas" de interés que iniciarán mensajes para enviarse como notificaciones de inserción. Los sistemas back-end envían mensajes a estos temas. Un back-end móvil puede suscribirse a uno o varios de estos temas mediante la creación de una suscripción a Service Bus. Esto permite al back-end móvil recibir una notificación del sistema back-end correspondiente. El back-end móvil sigue escuchando mensajes en sus suscripciones y, en cuanto llega uno, da la vuelta y lo envía como notificación a su centro de notificaciones. Los centros de notificaciones entregan finalmente el mensaje a la aplicación móvil. Esta es la lista de componentes clave:

1. Sistema back-end (sistema LoB o heredado)
   * Crea el tema de Service Bus
   * Envía el mensaje
1. Back-end móvil
   * Crea la suscripción al servicio
   * Recibe el mensaje (del sistema back-end)
   * Envía una notificación a los clientes (a través del Centro de notificaciones de Azure)
1. Aplicación móvil
   * Recibe y muestra notificaciones

### <a name="benefits"></a>Ventajas

1. La separación entre el receptor (aplicación/servicio móvil a través del Centro de notificaciones) y el emisor (sistemas back-end) permite la integración de sistemas back-end adicionales con cambios mínimos.
1. Esto hace también que en el escenario de varias aplicaciones móviles, sea posible recibir eventos de uno o varios sistemas back-end.  

## <a name="sample"></a>Muestra

### <a name="prerequisites"></a>Requisitos previos

Para familiarizarse con los conceptos, así como con los pasos comunes de creación y configuración, realice los siguientes tutoriales:

1. [Programación Pub/Sub de Service Bus]: en este tutorial se explican los detalles de trabajar con temas y suscripciones de Service Bus, cómo crear un espacio de nombres que contenga temas o suscripciones y cómo enviar y recibir mensajes de ellos.
2. [Tutorial sobre Notification Hubs: Windows Universal]: en este tutorial se explica cómo configurar una aplicación de la Tienda Windows y usar Notification Hubs para registrar y, después, recibir notificaciones.

### <a name="sample-code"></a>Código de ejemplo

El código de ejemplo completo está disponible en [Ejemplos de centro de notificaciones]. Se divide en tres componentes:

1. **EnterprisePushBackendSystem**

    a. Este proyecto usa el paquete NuGet **Azure.Messaging.ServiceBus** y se basa en la [programación Pub/Sub de Service Bus].

    b. Se trata de una aplicación de consola C# sencilla para simular un sistema LoB que inicia el mensaje que se entrega a la aplicación móvil.

    ```csharp
    static async Task Main(string[] args)
    {
        string connectionString =
            ConfigurationManager.AppSettings.Get("Azure.ServiceBus.ConnectionString");

        // Create the topic
        await CreateTopicAsync(connectionString);

        // Send message
        await SendMessageAsync(connectionString);
    }
    ```

    c. `CreateTopicAsync` se usa para crear un tema de Service Bus.

    ```csharp
    public static async Task CreateTopicAsync(string connectionString)
    {
        // Create the topic if it does not exist already
        ServiceBusAdministrationClient client = new ServiceBusAdministrationClient(connectionString);

        if (!await client.TopicExistsAsync(topicName))
        {
            await client.CreateTopicAsync(topicName);
        }
    }
    ```

    d. `SendMessageAsync` se usa para enviar los mensajes a este tema de Service Bus. En el ejemplo, este código simplemente envía un conjunto de mensajes aleatorios al tema de manera periódica. Normalmente, hay un sistema back-end que envía los mensajes cuando se produce un evento.

    ```csharp
    public static sync Task SendMessageAsync(string connectionString)
    {
        await using var client = new ServiceBusClient(connectionString);
        ServiceBusSender sender = client.CreateSender(topicName);

        // Sends random messages every 10 seconds to the topic
        string[] messages =
        {
            "Employee Id '{0}' has joined.",
            "Employee Id '{0}' has left.",
            "Employee Id '{0}' has switched to a different team."
        };

        while (true)
        {
            Random rnd = new Random();
            string employeeId = rnd.Next(10000, 99999).ToString();
            string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

            // Send Notification
            ServiceBusMessage message = new ServiceBusMessage(notification);
            await sender.SendMessageAsync(message);

            Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

            System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
        }
    }
    ```

2. **ReceiveAndSendNotification**

    a. Este proyecto usa los paquetes NuGet *Azure.Messaging.ServiceBus* y **Microsoft.Web.WebJobs.Publish** y se basa en la [programación Pub/Sub de Service Bus].

    b. La siguiente aplicación de consola se ejecuta como un [trabajo web de Azure] dado que se tiene que ejecutar continuamente para escuchar mensajes de los sistemas LoB/back-end. Esta aplicación forma parte de su back-end móvil.

    ```csharp
    static async Task Main(string[] args)
    {
        string connectionString =
                 ConfigurationManager.AppSettings.Get("Azure.ServiceBus.ConnectionString");

        // Create the subscription that receives messages
        await CreateSubscriptionAsync(connectionString);

        // Receive message
        await ReceiveMessageAndSendNotificationAsync(connectionString);
    }
    ```

    c. `CreateSubscriptionAsync` se usa para crear una suscripción de Service Bus para el tema donde el sistema back-end envía mensajes. Según el escenario empresarial, este componente crea una o varias suscripciones a temas correspondientes (por ejemplo, puede que algunos reciban mensajes del sistema de RR. PP., otros del sistema de Finanzas, etc.)

    ```csharp
    static async Task CreateSubscriptionAsync(string connectionString)
    {
        // Create the subscription if it does not exist already
        ServiceBusAdministrationClient client = new ServiceBusAdministrationClient(connectionString);

        if (!await client.SubscriptionExistsAsync(topicName, subscriptionName))
        {
            await client.CreateSubscriptionAsync(topicName, subscriptionName);
        }
    }
    ```

    d. `ReceiveMessageAndSendNotificationAsync` se usa para leer el mensaje del tema mediante su suscripción y, si la lectura se realiza correctamente, entonces se crea una notificación (en el escenario de ejemplo, una notificación del sistema nativa de Windows) para su envío a la aplicación móvil mediante Azure Notification Hubs.

    ```csharp
    static async Task ReceiveMessageAndSendNotificationAsync(string connectionString)
    {
        // Initialize the Notification Hub
        string hubConnectionString = ConfigurationManager.AppSettings.Get
                ("Microsoft.NotificationHub.ConnectionString");
        hub = NotificationHubClient.CreateClientFromConnectionString
                (hubConnectionString, "enterprisepushservicehub");

        ServiceBusClient Client = new ServiceBusClient(connectionString);
        ServiceBusReceiver receiver = Client.CreateReceiver(topicName, subscriptionName);

        // Continuously process messages received from the subscription
        while (true)
        {
            ServiceBusReceivedMessage message = await receiver.ReceiveMessageAsync();
            var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

            if (message != null)
            {
                try
                {
                    Console.WriteLine(message.MessageId);
                    Console.WriteLine(message.SequenceNumber);
                    string messageBody = message.Body.ToString();
                    Console.WriteLine("Body: " + messageBody + "\n");

                    toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                    SendNotificationAsync(toastMessage);

                    // Remove message from subscription
                    await receiver.CompleteMessageAsync(message);
                }
                catch (Exception)
                {
                    // Indicate a problem, unlock message in subscription
                    await receiver.AbandonMessageAsync(message);
                }
            }
        }
    }
    static async void SendNotificationAsync(string message)
    {
        await hub.SendWindowsNativeNotificationAsync(message);
    }
    ```

    e. Para publicar esta aplicación como un **trabajo web**, haga clic con el botón derecho en la solución en Visual Studio y seleccione **Publicar como trabajo web**.

    ![Captura de pantalla de las opciones de clic con el botón derecho que se muestran con la opción Publish as Azure WebJob (Publicar como WebJob de Azure) enmarcada en rojo.][2]

    f. Seleccione su perfil de publicación y cree un sitio web de Azure, si aún no existe, que hospede este trabajo web. Cuando tenga el sitio web, haga clic en **Publicar**.

    :::image type="complex" source="./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png" alt-text="Captura de pantalla que muestra el flujo de trabajo para crear un sitio en Azure.":::
    Captura de pantalla del cuadro de diálogo Publicación web con la opción Sitios web de Microsoft Azure seleccionada, una flecha verde que apunta al cuadro de diálogo Seleccionar sitio web existente con la opción Nuevo enmarcada en rojo, y una flecha verde que apunta al cuadro de diálogo Crear sitio en Microsoft Azure con las opciones Nombre del sitio y Crear enmarcadas en rojo.
    :::image-end:::

    g. Configure el trabajo como "Ejecutar continuamente", de modo que cuando inicie sesión en [Azure Portal] vea algo parecido a esto:

    ![Captura de pantalla de Azure Portal con los trabajos web de back-end de inserción empresarial mostrados y los valores de Nombre, Programación y Registros enmarcados en rojo.][4]

3. **EnterprisePushMobileApp**

    a. Se trata de una aplicación de la Tienda Windows que recibe notificaciones del sistema del trabajo web que se ejecuta como parte de su back-end móvil y las mostrará. Este código se basa en el [tutorial sobre Notification Hubs: Windows Universal].  

    b. Asegúrese de que su aplicación está habilitada para recibir notificaciones del sistema.

    c. Asegúrese de que se llama al siguiente código de registro de Notification Hubs al inicio de la aplicación (después de sustituir los valores de `HubName` y `DefaultListenSharedAccessSignature`:

    ```csharp
    private async void InitNotificationsAsync()
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
        var result = await hub.RegisterNativeAsync(channel.Uri);

        // Displays the registration ID so you know it was successful
        if (result.RegistrationId != null)
        {
            var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

### <a name="running-the-sample"></a>Ejecución del ejemplo

1. Asegúrese de que su trabajo web se ejecuta correctamente y está programado para ejecutarse continuamente.
2. Ejecute **EnterprisePushMobileApp**, que inicia la aplicación de la Tienda Windows.
3. Ejecute la aplicación de consola **EnterprisePushBackendSystem** que simula el back-end LoB e inicia el envío de mensajes, de modo que verá que aparecen notificaciones del sistema como la siguiente imagen:

    ![Captura de pantalla de una consola que ejecuta la aplicación Enterprise Push Backend System y el mensaje enviado por ella.][5]

4. Los mensajes se enviaron originalmente a temas de Service Bus, los cuales son supervisadas por las suscripciones de Service Bus en el trabajo web. Cuando se recibe un mensaje, se crea una notificación y se envía a la aplicación móvil. Puede examinar los registros del trabajo web para confirmar el procesamiento en el vínculo Registros en [Azure Portal] correspondiente a su trabajo web:

    ![Captura de pantalla del cuadro de diálogo Continuous WebJob Details (Detalles de WebJobs continuos) con el mensaje que se envía enmarcado en rojo.][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Ejemplos de centro de notificaciones]: https://github.com/Azure/azure-notificationhubs-samples
[Servicio móvil de Azure]: https://azure.microsoft.com/services/app-service/mobile/
[Azure Service Bus]: ../service-bus-messaging/service-bus-messaging-overview.md
[Programación Pub/Sub de Service Bus]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
[trabajo web de Azure]: ../app-service/webjobs-create.md
[Tutorial sobre Notification Hubs: Windows Universal]: ./notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Azure Portal]: https://portal.azure.com/
