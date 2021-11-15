---
title: Enrutamiento de mensajes de Azure IoT Hub | Microsoft Docs
description: 'Guía del desarrollador: cómo usar el enrutamiento de mensajes para enviar mensajes del dispositivo a la nube. Incluye información sobre el envío de datos de telemetría y datos que no son de telemetría.'
author: nehsin
manager: mehmet.kucukgoz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/14/2021
ms.author: nehsin
ms.custom:
- 'Role: Cloud Development'
- devx-track-csharp
ms.openlocfilehash: 5a405c50c94a16394a92ba7c4830ab3f3aee072f
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131449723"
---
# <a name="use-iot-hub-message-routing-to-send-device-to-cloud-messages-to-different-endpoints"></a>Uso del enrutamiento de mensajes de IoT Hub para enviar mensajes del dispositivo a la nube a distintos puntos de conexión

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

El enrutamiento de mensajes le permite enviar mensajes desde los dispositivos a los servicios en la nube de forma automatizada, escalable y confiable. El enrutamiento de mensajes se puede usar para: 

* **Enviar mensajes de telemetría y eventos del dispositivo**, es decir, eventos del ciclo de vida del dispositivo, eventos de cambio de dispositivo gemelo, eventos de cambio de gemelo digital y eventos de estado de conexión del dispositivo al punto de conexión integrado y los puntos de conexión personalizados. Obtenga información sobre los [puntos de conexión de enrutamiento](#routing-endpoints). Para más información acerca de los eventos enviados desde los dispositivos de IoT Plug and Play, consulte [Información de Digital Twins de IoT Plug and Play](../iot-develop/concepts-digital-twin.md).

* **Filtrar los datos antes de enrutarlos a los diferentes puntos de conexión** mediante la aplicación de consultas enriquecidas. El enrutamiento de mensajes le permite realizar consultas sobre las propiedades de los mensajes y el cuerpo del mensaje, así como las etiquetas del dispositivo gemelo y las propiedades del dispositivo gemelo. Más información sobre el uso de [consultas en el enrutamiento de mensajes](iot-hub-devguide-routing-query-syntax.md).

IoT Hub necesita acceso de escritura a estos puntos de conexión de servicio para que el enrutamiento de mensajes funcione. Si configura los puntos de conexión a través de Azure Portal, se agregan los permisos necesarios automáticamente. Asegúrese de configurar los servicios para admitir el rendimiento esperado. Por ejemplo, si usa Event Hubs como punto de conexión personalizado, debe configurar las **unidades de procesamiento** para ese centro de eventos de forma que pueda controlar la entrada de eventos que planea enviar a través del enrutamiento de mensajes de IoT Hub. Del mismo modo, cuando se usa una cola de Service Bus como punto de conexión, debe configurar el **tamaño máximo** para asegurarse de que la cola puede contener todos los datos ingresados, hasta que los consumidores los extraigan. Al configurar la solución de IoT por primera vez, es posible que deba supervisar los demás puntos de conexión y realizar los ajustes necesarios para la carga real.

IoT Hub define un [formato común](iot-hub-devguide-messages-construct.md) para todos los mensajes del dispositivo a la nube a fin de generar interoperabilidad entre protocolos. Si un mensaje coincide con varias rutas que señalan al mismo punto de conexión, IoT Hub entrega el mensaje a ese punto de conexión solo una vez. Por lo tanto, no es necesario configurar la desduplicación en la cola o el tema de Service Bus. Use este tutorial para aprender a [configurar el enrutamiento de mensajes](tutorial-routing.md).

## <a name="routing-endpoints"></a>Puntos de conexión de enrutamiento

Un centro de IoT tiene un punto de conexión integrado predeterminado (**mensajes y eventos**) que es compatible con Event Hubs. Puede crear [puntos de conexión personalizados](iot-hub-devguide-endpoints.md#custom-endpoints) a los que enrutar mensajes vinculando otros servicios de la suscripción al centro de IoT. 

Cada mensaje se enruta a todos los puntos de conexión cuyas consultas se correspondan con el mensaje. En otras palabras, un mensaje se puede enrutar a varios puntos de conexión.

Si el punto de conexión personalizado tiene configuraciones de firewall, considere la posibilidad de usar [excepciones propias de confianza de Microsoft](./virtual-network-support.md#egress-connectivity-from-iot-hub-to-other-azure-resources).

IoT Hub admite actualmente los siguientes puntos de conexión:

 - Punto de conexión integrado
 - Azure Storage
 - Colas de Service Bus y temas de Service Bus
 - Event Hubs

## <a name="built-in-endpoint-as-a-routing-endpoint"></a>Punto de conexión integrado como punto de conexión de enrutamiento

Puede usar la [integración y los SDK de Event Hubs](iot-hub-devguide-messages-read-builtin.md) estándar para recibir mensajes del dispositivo a la nube desde el punto de conexión integrado (**mensajes y eventos**). Una vez que se crea una ruta, los datos dejan de fluir al punto de conexión integrado, a menos que se cree una ruta a ese punto de conexión. Aunque no se cree ninguna ruta, se debe habilitar una ruta de reserva para enrutar los mensajes al punto de conexión integrado. La reserva está habilitada de forma predeterminada si crea el centro mediante el portal o la CLI.

## <a name="azure-storage-as-a-routing-endpoint"></a>Azure Storage como punto de conexión de enrutamiento

Hay dos servicios de almacenamiento a los que IoT Hub puede enrutar mensajes: a cuentas de [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md) y [Azure Data Lake Storage Gen2 ](../storage/blobs/data-lake-storage-introduction.md) (ADLS Gen2). Las cuentas de Azure Data Lake Storage son cuentas de almacenamiento habilitadas para [espacios de nombres jerárquicos](../storage/blobs/data-lake-storage-namespace.md) que se basan en Blob Storage. Ambas usan blobs para su almacenamiento.

IoT Hub admite la escritura de datos en Azure Storage con los formatos [Apache Avro](https://avro.apache.org/) y JSON. El valor predeterminado es AVRO. Cuando se usa la codificación JSON, debe establecer contentType en **application/json** y contentEncoding en **UTF-8** en las [propiedades del sistema](iot-hub-devguide-routing-query-syntax.md#system-properties) del mensaje. Ambos valores no distinguen mayúsculas de minúsculas. Si no está establecida la codificación del contenido, IoT Hub escribirá los mensajes en formato codificado de base 64.

El formato de codificación solo se puede establecer cuando se configura el punto de conexión de Blob Storage. No se puede editar desde un punto de conexión existente. Para cambiar los formatos de codificación de un punto de conexión existente, debe eliminar y volver a crear el punto de conexión con el formato que quiera. Una estrategia útil podría ser crear un nuevo punto de conexión personalizado con el formato de codificación deseado y agregar una ruta paralela a ese punto de conexión. De esta manera, puede comprobar los datos antes de eliminar el punto de conexión existente.

Puede seleccionar el formato de codificación mediante la API REST Crear o actualizar de IoT Hub, específicamente [RoutingStorageContainerProperties](/rest/api/iothub/iothubresource/createorupdate#routingstoragecontainerproperties), [Azure Portal](https://portal.azure.com), la [CLI de Azure](/cli/azure/iot/hub/routing-endpoint) o [Azure PowerShell](/powershell/module/az.iothub/add-aziothubroutingendpoint). En la siguiente imagen se muestra cómo seleccionar el formato de codificación en Azure Portal.

![Codificación de puntos de conexión de Blob Storage](./media/iot-hub-devguide-messages-d2c/blobencoding.png)

IoT Hub agrupa los mensajes por lotes y escribe los datos en un almacenamiento cuando el lote llega a cierto tamaño o después de transcurrir cierta cantidad de tiempo. IoT Hub asume como valor predeterminado la convención de nomenclatura de archivos siguiente:

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

Puede usar cualquier convención de nomenclatura de archivos, aunque debe usar todos los tokens de la lista. IoT Hub escribirá en un blob vacío si no hay datos que escribir.

Se recomienda enumerar los blobs o los archivos e iterar sobre ellos para garantizar que se leen todos sin pasar por alto ninguna partición. El intervalo de partición podría cambiar durante una [conmutación por error iniciada por Microsoft](iot-hub-ha-dr.md#microsoft-initiated-failover) o una [conmutación por error manual](iot-hub-ha-dr.md#manual-failover) de IoT Hub. Puede usar la [API List Blobs](/rest/api/storageservices/list-blobs) para la lista de blobs o la [API List ADLS Gen2](/rest/api/storageservices/datalakestoragegen2/path) para la lista de archivos. Vea el siguiente ejemplo como guía.

```csharp
public void ListBlobsInContainer(string containerName, string iothub)
{
    var storageAccount = CloudStorageAccount.Parse(this.blobConnectionString);
    var cloudBlobContainer = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
    if (cloudBlobContainer.Exists())
    {
        var results = cloudBlobContainer.ListBlobs(prefix: $"{iothub}/");
        foreach (IListBlobItem item in results)
        {
            Console.WriteLine(item.Uri);
        }
    }
}
```

Para crear una cuenta de almacenamiento compatible con Azure Data Lake Gen2, cree una nueva cuenta de almacenamiento V2 y seleccione *habilitado* en el campo *Espacio de nombres jerárquico* en la pestaña **Avanzado**, como se muestra en la siguiente imagen:

![Seleccione el almacenamiento de Azure Date Lake Gen2.](./media/iot-hub-devguide-messages-d2c/selectadls2storage.png)

## <a name="service-bus-queues-and-service-bus-topics-as-a-routing-endpoint"></a>Colas y temas de Service Bus como punto de conexión de enrutamiento

Las colas y los temas de Service Bus usados como puntos de conexión de IoT Hub no deben tener habilitadas las opciones **Sesiones** o **Detección de duplicados**. Si cualquiera de estas opciones está habilitada, el punto de conexión aparece como **Inaccesible** en Azure Portal.

## <a name="event-hubs-as-a-routing-endpoint"></a>Event Hubs como punto de conexión de enrutamiento

Aparte del punto de conexión compatible con Event Hubs integrado, también puede enrutar los datos a puntos de conexión personalizados de tipo Event Hubs. 

## <a name="reading-data-that-has-been-routed"></a>Lectura de datos que se han enrutado

Para configurar una ruta, siga este [tutorial](tutorial-routing.md).

Use los siguientes tutoriales para obtener información sobre cómo leer mensajes de un punto de conexión.

* Lectura desde el [punto de conexión integrado](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-nodejs)

* Lectura desde [Blob Storage](../storage/blobs/storage-blob-event-quickstart.md)

* Lectura desde [Event Hubs](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

* Lectura desde [colas de Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)

* Lectura desde [temas de Service Bus](../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md)


## <a name="fallback-route"></a>Ruta de reserva

La ruta de reserva envía todos los mensajes que no cumplen las condiciones de la consulta en cualquiera de las rutas existentes al punto de conexión de Event Hubs integrado (**mensajes y eventos**), que es compatible con [Event Hubs](../event-hubs/index.yml). Si el enrutamiento de mensajes está activado, puede habilitar la funcionalidad de ruta de reserva. Una vez que se crea una ruta, los datos dejan de fluir al punto de conexión integrado, a menos que se cree una ruta a ese punto de conexión. Si no hay ninguna ruta al punto de conexión integrado y está habilitada una ruta de reserva, solo se enviarán al punto de conexión integrado los mensajes que no coinciden con las condiciones de la consulta sobre rutas. Además, si se eliminan todas las rutas existentes, se debe habilitar la ruta de reserva para recibir todos los datos en el punto de conexión integrado. 

Puede habilitar o deshabilitar la ruta de reserva en Azure Portal -> hoja Enrutamiento de mensajes. También puede usar Azure Resource Manager para [FallbackRouteProperties](/rest/api/iothub/iothubresource/createorupdate#fallbackrouteproperties) para usar un punto de conexión personalizado para la ruta de reserva.

## <a name="non-telemetry-events"></a>Eventos que no son de telemetría

Además de los datos de telemetría del dispositivo, el enrutamiento de mensajes también permite enviar eventos de cambio de dispositivo gemelo, eventos del ciclo de vida del dispositivo, eventos de cambio de gemelo digital y eventos de estado de conexión del dispositivo. Por ejemplo, si se crea una ruta con el origen de datos establecido en **eventos de cambio de dispositivo gemelo**, IoT Hub envía mensajes al punto de conexión que contienen el cambio en el dispositivo gemelo. De forma similar, si se crea una ruta con el origen de datos establecido en **eventos del ciclo de vida del dispositivo**, IoT Hub enviará un mensaje que indica si el dispositivo se ha eliminado o se ha creado. Como parte de [Azure IoT Plug and Play](../iot-develop/overview-iot-plug-and-play.md), un desarrollador puede crear rutas con el origen de datos establecido en **eventos de cambio de gemelo digital** e IoT Hub enviará mensajes siempre que se establezca o cambie una propiedad de gemelo digital, se sustituya un gemelo digital o se produzca un evento de cambio para el dispositivo gemelo subyacente. Finalmente, si se crea una ruta con el origen de datos establecido en **eventos de estado de conexión del dispositivo**, IoT Hub enviará un mensaje que indica si el dispositivo se ha conectado o desconectado.


[IoT Hub también se integra con Azure Event Grid](iot-hub-event-grid.md) para publicar los eventos de dispositivo con el fin de admitir integraciones en tiempo real y la automatización de flujos de trabajo basados en estos eventos. Consulte las [diferencias principales entre el enrutamiento de mensajes y Event Grid](iot-hub-event-grid-routing-comparison.md) para obtener información sobre la mejor opción para su escenario.

## <a name="limitations-for-device-connection-state-events"></a>Limitaciones de los eventos de estado de conexión del dispositivo

Para recibir eventos de estado de conexión de dispositivo, el dispositivo debe llamar a una operación de *envío de telemetría de dispositivo a nube* o de *recepción de mensajes de nube a dispositivo* con IoT Hub. Sin embargo, si un dispositivo usa el protocolo AMQP para conectarse con IoT Hub, se recomienda que el dispositivo llame a la operación de *recepción de mensajes de nube a dispositivo*; de lo contrario, sus notificaciones de estado de conexión pueden retrasarse unos minutos. Si el dispositivo se conecta con el protocolo MQTT, IoT Hub mantiene abierto el vínculo de nube a dispositivo. Para abrir el vínculo de nube a dispositivo para AMQP, llame a [Receive Async API](/rest/api/iothub/device/receivedeviceboundnotification).

El vínculo de dispositivo a nube permanece abierto siempre que el dispositivo envíe datos de telemetría.

Si el estado de conexión del dispositivo cambia continuamente, lo que significa que el dispositivo se conecta y desconecta con frecuencia, IoT Hub no envía todos los estados de conexión, sino que publica el estado de conexión actual tomado en una instantánea periódica de 60 segundos hasta que se queda fijo. Si se recibe el mismo evento de estado de conexión con diferentes números de secuencia, o eventos de estado de conexión distintos, quiere decir que se ha producido un cambio en el estado de conexión del dispositivo.

## <a name="testing-routes"></a>Prueba de las rutas

Al crear una ruta nueva o al modificar una ruta existente, debe probar la consulta de ruta con un mensaje de ejemplo. Puede probar rutas individuales o probar todas las rutas a la vez y no se enruta ningún mensaje a los puntos de conexión durante la prueba. Puede usar Azure Portal, Azure Resource Manager, Azure PowerShell y la CLI de Azure para realizar las pruebas. Los resultados ayudan a identificar si el mensaje de ejemplo coincide con la consulta, el mensaje no coincide con la consulta o no se pudo ejecutar la prueba porque la sintaxis del mensaje de ejemplo o de la consulta son incorrectas. Para más información, consulte [Prueba de una ruta](/rest/api/iothub/iothubresource/testroute) y [Prueba de todas las rutas](/rest/api/iothub/iothubresource/testallroutes).

## <a name="latency"></a>Latencia

Al enrutar mensajes de telemetría del dispositivo a la nube mediante puntos de conexión integrados, aumenta ligeramente la latencia de un extremo a otro tras las creación de la primera ruta.

En la mayoría de los casos, el aumento medio de la latencia es inferior a 500 ms. Puede supervisar la latencia con **Enrutamiento: latencia de mensaje para mensajes y eventos** o con la métrica de IoT Hub **d2c.endpoints.latency.builtIn.events**. La creación o eliminación de una ruta tras la primera no afecta a la latencia de un extremo a otro.

## <a name="monitoring-and-troubleshooting"></a>Supervisión y solución de problemas

IoT Hub proporciona varias métricas relacionadas con el enrutamiento y los puntos de conexión para ofrecerle una visión general del mantenimiento del centro y los mensajes enviados. Puede encontrar una lista de todas las métricas de IoT Hub desglosadas por categoría funcional en [Métricas de la referencia de datos de supervisión](monitor-iot-hub-reference.md#metrics). Puede realizar el seguimiento de los errores que se producen durante la evaluación de una consulta de enrutamiento y del estado del punto de conexión que percibe IoT Hub con la categoría [**rutas** de los registros de recursos de IoT Hub](monitor-iot-hub-reference.md#routes). Para más información sobre el uso de métricas y registros de recursos con IoT Hub, consulte [Supervisión de IoT Hub](monitor-iot-hub.md).

Puede usar la API REST [Get Endpoint Health](/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) para obtener el [estado de mantenimiento](iot-hub-devguide-endpoints.md#custom-endpoints) de los puntos de conexión.

Use la[ guía de solución de problemas del enrutamiento](troubleshoot-message-routing.md) para obtener una información más detallada y soporte técnico para solucionar problemas de enrutamiento.

## <a name="next-steps"></a>Pasos siguientes

* Para aprender a crear rutas de mensajes, consulte [Procesamiento de mensajes del dispositivo a la nube de IoT Hub mediante rutas](tutorial-routing.md).

* [Envío de mensajes de dispositivo a nube](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-nodejs).

* Para obtener información sobre los SDK que puede utilizar para enviar mensajes del dispositivo a la nube, consulte [SDK de Azure IoT](iot-hub-devguide-sdks.md).