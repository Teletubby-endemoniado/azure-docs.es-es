---
title: Información sobre el punto de conexión integrado de Azure IoT Hub | Microsoft Docs
description: En esta guía del desarrollador se describe cómo usar el punto de conexión integrado compatible con Event Hubs para leer los mensajes del dispositivo a la nube.
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/16/2021
ms.custom:
- amqp
- 'Role: Cloud Development'
ms.openlocfilehash: 1a3bae398a5991bbaf79e362e5ebb3942fd2f53e
ms.sourcegitcommit: 557ed4e74f0629b6d2a543e1228f65a3e01bf3ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2021
ms.locfileid: "129458612"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Leer mensajes del dispositivo a la nube desde el punto de conexión integrado

De forma predeterminada, los mensajes se enrutan al punto de conexión orientado al servicio integrado ( **/messages/events**), que es compatible con [Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Actualmente, este punto de conexión solo se expone mediante el protocolo [AMQP](https://www.amqp.org/) en el puerto 5671 y AMQP a través de [WebSockets](http://docs.oasis-open.org/amqp-bindmap/amqp-wsb/v1.0/cs01/amqp-wsb-v1.0-cs01.html) en el puerto 443. IoT Hub muestra las propiedades siguientes para permitirle controlar el punto de conexión de mensajería integrado compatible con Event Hubs **messages/events**.

| Propiedad            | Descripción |
| ------------------- | ----------- |
| **Número de particiones** | Establezca esta propiedad durante la creación para definir el número de [particiones](../event-hubs/event-hubs-features.md#partitions) para ingesta de eventos del dispositivo a la nube. |
| **Tiempo de retención**  | Esta propiedad especifica cuánto tiempo, en días, IoT Hub conserva los mensajes. El valor predeterminado es un día, pero se puede aumentar a siete días. |

IoT Hub permite la retención de datos en el servicio Event Hubs integrado durante un máximo de 7 días. Puede establecer el tiempo de retención durante la creación del servicio IoT Hub. El tiempo de retención de datos en IoT Hub depende del nivel y el tipo de unidad de su centro de IoT. En términos de tamaño, el servicio Event Hubs integrado puede conservar los mensajes del tamaño máximo hasta al menos 24 horas de cuota. Por ejemplo, para una unidad S1, IoT Hub proporciona almacenamiento suficiente para conservar al menos 400 000 mensajes con un tamaño de 4k cada uno. Si los dispositivos envían mensajes más pequeños, pueden conservarse durante más tiempo (hasta 7 días) según la cantidad de almacenamiento que se consuma. Garantizamos la conservación de los datos durante el tiempo de retención especificado como mínimo. Los mensajes expirarán y no podrá acceder a ellos una vez transcurrido el tiempo de retención. 

IoT Hub también le permite administrar los grupos de consumidores en el punto de conexión de recepción de dispositivo a nube integrado. Puede tener hasta 20 grupos de consumidores para cada IoT Hub.

Si usa el [enrutamiento de mensajes](iot-hub-devguide-messages-d2c.md) y está habilitada la [ruta de reserva](iot-hub-devguide-messages-d2c.md#fallback-route), todos los mensajes que no coincidan con una consulta en cualquier ruta se dirigirán al punto de conexión integrado. Si deshabilita esta ruta de reserva, los mensajes que no coincidan con ninguna consulta se quitarán.

Puede modificar el tiempo de retención mediante programación con las [API REST del proveedor de recursos de IoT Hub](/rest/api/iothub/iothubresource) o con [Azure Portal](https://portal.azure.com).

IoT Hub expone el punto de conexión integrado **messages/events** para los servicios de back-end con el fin de leer los mensajes de dispositivo a nube recibidos por el centro. Este punto de conexión es compatible con Event Hubs, lo que permite usar cualquiera de los mecanismos del servicio Event Hubs para leer mensajes.

## <a name="read-from-the-built-in-endpoint"></a>Leer desde el punto de conexión integrado

Algunas integraciones del producto y algunos SDK de Event Hubs tienen en cuenta el servicio IoT Hub y permiten usar la cadena de conexión de servicio del centro de IoT para conectarse al punto de conexión integrado.

Si usa SDK o integraciones del productos de Event Hubs que no tienen en cuenta IoT Hub, necesita un punto de conexión y un nombre compatibles con Event Hubs. Puede recuperar estos valores del portal como se indica a continuación:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.

2. Haga clic en **Puntos de conexión integrados**.

3. La sección **Eventos** contiene los valores siguientes: **Particiones**, **Nombre compatible con Event Hub**, **Extremo compatible con Event Hub**, **Tiempo de retención** y **Grupos de consumidores**.

    ![Configuración de dispositivo a nube](./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png)

En el portal, el campo Extremo compatible con Event Hub contiene una cadena de conexión de Event Hubs completa similar a la siguiente: **Endpoint=sb://abcd1234namespace.servicebus.windows.net/;SharedAccessKeyName=iothubowner;SharedAccessKey=keykeykeykeykeykey=;EntityPath=iothub-ehub-abcd-1234-123456**. Si el SDK que utiliza requiere otros valores, serían los siguientes:

| Nombre | Value |
| ---- | ----- |
| Punto de conexión | sb://abcd1234namespace.servicebus.windows.net/ |
| Hostname | abcd1234namespace.servicebus.windows.net |
| Espacio de nombres | abcd1234namespace |

Tras ello, puede elegir cualquier directiva de acceso compartido en la lista desplegable, como se muestra en la captura de pantalla anterior. Solamente aparecerán las directivas que tengan permisos **ServiceConnect** para conectarse a la instancia de Event Hub especificada.

Los SDK que puede usar para conectarse al punto de conexión compatible con Event Hubs integrado que muestra IoT Hub incluyen:

| Idioma | SDK | Ejemplo |
| -------- | --- | ------ |
| .NET | https://www.nuget.org/packages/Azure.Messaging.EventHubs | [Guía de inicio rápido](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-csharp) |
| Java | https://mvnrepository.com/artifact/com.azure/azure-messaging-eventhubs | [Guía de inicio rápido](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-java) |
| Node.js | https://www.npmjs.com/package/@azure/event-hubs | [Guía de inicio rápido](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-nodejs) |
| Python | https://pypi.org/project/azure-eventhub/ | [Guía de inicio rápido](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-python) |

Las integraciones del producto que puede usar con el punto de conexión compatible con Event Hubs integrado que muestra IoT Hub incluyen:

* [Azure Functions](../azure-functions/index.yml). Consulte [Enlaces de Azure IoT Hub para Azure Functions](../azure-functions/functions-bindings-event-iot.md).
* [Azure Stream Analytics](../stream-analytics/index.yml). Consulte [Datos de flujo como entrada en Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-iot-hub).
* [Time Series Insights](../time-series-insights/index.yml). Consulte [Adición de un origen de eventos de IoT Hub al entorno de Time Series Insights](../time-series-insights/how-to-ingest-data-iot-hub.md).
* [Spout de Apache Storm](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). Puede ver el [origen de spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) en GitHub.
* [Integración de Apache Spark](../hdinsight/spark/apache-spark-ipython-notebook-machine-learning.md).
* [Azure Databricks](/azure/azure-databricks/).

## <a name="use-amqp-ws-or-a-proxy-with-event-hubs-sdks"></a>Uso de AMQP-WS o un proxy con los SDK de Event Hubs

Puede usar los SDK de Event Hubs para leer desde el punto de conexión integrado en entornos en los que se requiere AMQP a través de WebSockets o la lectura a través de un proxy. Para obtener más información, consulte los ejemplos siguientes.

| Lenguaje | Muestra |
| -------- | ------ |
| .NET | [ReadD2cMessages .NET](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/main/iot-hub/Quickstarts/ReadD2cMessages) |
| Java | [read-d2c-messages Java](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/iot-hub/Quickstarts/read-d2c-messages) |
| Node.js | [read-d2c-messages Node.js](https://github.com/Azure-Samples/azure-iot-samples-node/tree/master/iot-hub/Quickstarts/read-d2c-messages) |
| Python | [read-dec-messages Python](https://github.com/Azure-Samples/azure-iot-samples-python/tree/master/iot-hub/Quickstarts/read-d2c-messages) |

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los puntos de conexión de IoT Hub, vea [Puntos de conexión de IoT Hub](iot-hub-devguide-endpoints.md).

* En las [guías de inicio rápidos](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-nodejs) se muestra cómo enviar mensajes del dispositivo a la nube desde dispositivos simulados y cómo leer los mensajes desde el punto de conexión integrado. 

Para más información, consulte el tutorial [Procesamiento de mensajes de dispositivo a nube de IoT Hub mediante rutas](tutorial-routing.md).

* Si quiere enrutar los mensajes del dispositivo a la nube a puntos de conexión personalizados, consulte [Uso de rutas de mensajes y de puntos de conexión personalizados para mensajes de dispositivo a nube](iot-hub-devguide-messages-read-custom.md).