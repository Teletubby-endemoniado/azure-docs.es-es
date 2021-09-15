---
title: Integración con Azure Maps
titleSuffix: Azure Digital Twins
description: Vea cómo usar Azure Functions para crear una función que pueda usar las notificaciones de Azure Digital Twins y el grafo de gemelos para actualizar un mapa de interiores de Azure Maps.
author: baanders
ms.author: baanders
ms.date: 8/27/2021
ms.topic: how-to
ms.service: digital-twins
ms.reviewer: baanders
ms.openlocfilehash: 494e4b048fc33a0cd7fcfc6c354a7f757298ebef
ms.sourcegitcommit: 40866facf800a09574f97cc486b5f64fced67eb2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2021
ms.locfileid: "123223468"
---
# <a name="use-azure-digital-twins-to-update-an-azure-maps-indoor-map"></a>Uso de Azure Digital Twins para actualizar un plano interior de Azure Maps

En este artículo se explican los pasos necesarios para usar los datos de Azure Digital Twins a fin de actualizar la información que se muestra en un *plano interior* mediante [Azure Maps](../azure-maps/about-azure-maps.md). Azure Digital Twins almacena un grafo de las relaciones de los dispositivos IoT y enruta la telemetría a distintos puntos de conexión, lo que lo convierte en el servicio perfecto para actualizar las superposiciones de información en los planos.

En esta guía se tratarán los siguientes temas:

1. Configurar la instancia de Azure Digital Twins para enviar eventos de actualización de gemelos a una función en [Azure Functions](../azure-functions/functions-overview.md).
2. Crear una función para actualizar un conjunto de estados de características de planos interiores de Azure Maps.
3. Procedimientos para almacenar id. de planos y de conjuntos de estados de características en el grafo de Azure Digital Twins.

### <a name="prerequisites"></a>Requisitos previos

* Siga Azure Digital Twins en [Conexión de una solución de un extremo a otro](./tutorial-end-to-end.md).
    * En él ampliará este gemelo con un punto de conexión y una ruta adicionales. En dicho tutorial también se incorporará otra función a la aplicación de funciones. 
* Siga Azure Maps en [Uso de Creator para crear mapas de interiores](../azure-maps/tutorial-creator-indoor-maps.md) a fin de crear un mapa de interiores de Azure Maps con un *conjunto de estados de características*.
    * Los [conjuntos de estados de características](../azure-maps/creator-indoor-maps.md#feature-statesets) son colecciones de propiedades dinámicas (estados) asignadas a las características del conjunto de datos, como salas o equipamiento. En el tutorial anterior de Azure Maps, el conjunto de estados de características almacena el estado de la sala que se mostrará en un plano.
    * Necesitará el *id. de conjunto de estados* de características y la *clave de suscripción* de Azure Maps.

### <a name="topology"></a>Topología

En la imagen siguiente se muestra dónde encajan los elementos de integración de planos interiores en este tutorial en un escenario de mayor tamaño de Azure Digital Twins de un extremo a otro.

:::image type="content" source="media/how-to-integrate-maps/maps-integration-topology.png" alt-text="Diagrama de los servicios de Azure en un escenario de un extremo a otro en el que se resalta la parte Integración de Indoor Maps." lightbox="media/how-to-integrate-maps/maps-integration-topology.png":::

## <a name="create-a-function-to-update-a-map-when-twins-update"></a>Creación de una función para actualizar un plano al actualizar los gemelos

En primer lugar, creará una ruta en Azure Digital Twins para reenviar todos los eventos de actualización de gemelos a un tema de Event Grid. Después, usará una función para leer los mensajes de la actualización y actualizar un conjunto de estados de características en Azure Maps. 

## <a name="create-a-route-and-filter-to-twin-update-notifications"></a>Creación de una ruta y un filtro para las notificaciones de actualización de gemelos

Las instancias de Azure Digital Twins pueden emitir eventos de actualización de gemelos cada vez que se actualiza el estado de uno de estos elementos. El tutorial de Azure Digital Twins [Conexión de una solución de un extremo a otro](./tutorial-end-to-end.md) vinculado anteriormente le guiará a través de un escenario en el que se usa un termómetro para actualizar un atributo de temperatura conectado al gemelo de una sala. Ampliará esa solución mediante la suscripción a las notificaciones de actualización de gemelos y el uso de dicha información para actualizar los planos.

Este patrón realiza la lectura directamente desde el gemelo de la sala, en lugar de desde el dispositivo IoT, lo que ofrece la flexibilidad de cambiar el origen de datos subyacente para la temperatura sin necesidad de actualizar la lógica de asignación. Por ejemplo, puede agregar varios termómetros o establecer que esta sala comparta un termómetro con otra, todo ello sin necesidad de actualizar la lógica de asignación.

1. Cree un tema de Event Grid, que recibirá eventos de la instancia de Azure Digital Twins.
    ```azurecli-interactive
    az eventgrid topic create --resource-group <your-resource-group-name> --name <your-topic-name> --location <region>
    ```

2. Cree un punto de conexión para vincular el tema de Event Grid a Azure Digital Twins.
    ```azurecli-interactive
    az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> --dt-name <your-Azure-Digital-Twins-instance-name>
    ```

3. Cree una ruta en Azure Digital Twins para enviar eventos de actualización de gemelos al punto de conexión.

    >[!NOTE]
    >Actualmente hay un **problema conocido** en Cloud Shell que afecta a estos grupos de comandos: `az dt route`, `az dt model` y `az dt twin`.
    >
    >Para resolverlo, ejecute `az login` en Cloud Shell antes de ejecutar el comando, o bien use la [CLI local](/cli/azure/install-azure-cli) en lugar de Cloud Shell. Para obtener más información, vea [Solución de problemas: Problemas conocidos en Azure Digital Twins](troubleshoot-known-issues.md#400-client-error-bad-request-in-cloud-shell).

    ```azurecli-interactive
    az dt route create --dt-name <your-Azure-Digital-Twins-instance-name> --endpoint-name <Event-Grid-endpoint-name> --route-name <my-route> --filter "type = 'Microsoft.DigitalTwins.Twin.Update'"
    ```

## <a name="create-a-function-to-update-maps"></a>Creación de una función para actualizar planos

Va a crear una **función desencadenada por Event Grid** en la aplicación de funciones desde el tutorial de un extremo a otro ([Conexión de una solución de un extremo a otro](./tutorial-end-to-end.md)). Esta función desempaquetará esas notificaciones y enviará actualizaciones a un conjunto de estados de características de Azure Maps para actualizar la temperatura de una sala.

Vea el documento siguiente para obtener información de referencia: [Desencadenador de Azure Event Grid para Azure Functions](../azure-functions/functions-bindings-event-grid-trigger.md).

Reemplace el código de la función por el siguiente. Solo filtrará las actualizaciones de los gemelos de los espacios, leerá la temperatura actualizada y enviará esa información a Azure Maps.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/updateMaps.cs":::

Tendrá que establecer dos variables de entorno en la aplicación de funciones. Una es la [clave de suscripción principal de Azure Maps](../azure-maps/quick-demo-map-app.md#get-the-primary-key-for-your-account), y la otra, el [id. del conjunto de estados de Azure Maps](../azure-maps/tutorial-creator-indoor-maps.md#create-a-feature-stateset).

```azurecli-interactive
az functionapp config appsettings set --name <your-function-app-name> --resource-group <your-resource-group> --settings "subscription-key=<your-Azure-Maps-primary-subscription-key>"
az functionapp config appsettings set --name <your-function-app-name>  --resource-group <your-resource-group> --settings "statesetID=<your-Azure-Maps-stateset-ID>"
```

### <a name="view-live-updates-on-your-map"></a>Consulta de actualizaciones directas en el plano

Para ver las actualizaciones directas de la temperatura, siga estos pasos:

1. Comience a enviar datos de IoT simulados ejecutando el proyecto **DeviceSimulator** desde el tutorial de Azure Digital Twins [Conexión de una solución de un extremo a otro](tutorial-end-to-end.md). Las instrucciones de este proceso están disponibles en la sección [Configuración y ejecución de la simulación](././tutorial-end-to-end.md#configure-and-run-the-simulation).
2. Use [el módulo Indoor de Azure Maps](../azure-maps/how-to-use-indoor-module.md) para representar los mapas de interiores creados en Azure Maps Creator.
    1. Copie el HTML de la sección [Ejemplo: Uso del módulo Indoor Maps](../azure-maps/how-to-use-indoor-module.md#example-use-the-indoor-maps-module) del mapa de interiores en [Uso del módulo de mapas de Azure Maps Indoor](../azure-maps/how-to-use-indoor-module.md) en un archivo local.
    1. Reemplace la *clave de suscripción* y los elementos *tilesetId* y *statesetID* en el archivo HTML local por sus valores.
    1. Abra ese archivo en el explorador.

Ambos ejemplos envían la temperatura en un rango compatible, por lo que debería ver el color de la actualización de la sala 121 en el plano aproximadamente cada 30 segundos.

:::image type="content" source="media/how-to-integrate-maps/maps-temperature-update.png" alt-text="Captura de pantalla de un plano de una oficina en la que se muestra la sala 121 de color naranja.":::

## <a name="store-your-maps-information-in-azure-digital-twins"></a>Almacenamiento de la información de los planos en Azure Digital Twins

Ahora que tiene una solución codificada de forma rígida para actualizar la información de los planos, puede usar el grafo de Azure Digital Twins para almacenar toda la información necesaria a fin de actualizar el plano interior. Esta información incluiría el id. de conjuntos de estados, el id. de suscripción de los planos y el id. de característica de cada plano y ubicación, respectivamente. 

Una solución para este ejemplo específico implicaría actualizar cada espacio de nivel superior para que tuviera un id. de conjunto de estados y un atributo de id. de suscripción de planos, así como actualizar cada sala para que tuviera un id. de característica. Tendría que establecer estos valores una vez al inicializar el grafo de gemelos y, después, consultar esos valores para cada evento de actualización de gemelos.

En función de la configuración de su topología, podrá almacenar estos tres atributos en diferentes niveles según la granularidad del plano.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo administrar, actualizar y recuperar información del grafo de gemelos, vea las referencias siguientes:

* [Administración de Digital Twins](./how-to-manage-twin.md)
* [Consulta del grafo de gemelos](./how-to-query-graph.md)
