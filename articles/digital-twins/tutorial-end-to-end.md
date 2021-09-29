---
title: 'Tutorial: Conexión de una solución de un extremo a otro.'
titleSuffix: Azure Digital Twins
description: Tutorial para crear soluciones de Azure Digital Twins de un extremo a otro controladas por los datos de los dispositivos.
author: baanders
ms.author: baanders
ms.date: 8/23/2021
ms.topic: tutorial
ms.service: digital-twins
ms.openlocfilehash: 9d19a74dc7bacc996fe328679d9c3e12766bfadf
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128626197"
---
# <a name="tutorial-build-out-an-end-to-end-solution"></a>Tutorial: Creación de soluciones de un extremo a otro

Para configurar una solución de un extremo a otro controlada por los datos en directo de su entorno, puede conectar su instancia de Azure Digital Twins a otros servicios de Azure para la administración tanto de los dispositivos como de los datos.

En este tutorial:
> [!div class="checklist"]
> * Configurará una instancia de Azure Digital Twins.
> * Obtendrá información acerca del escenario del edificio de ejemplo y creará instancias de los componentes que se han escrito previamente.
> * Usará una aplicación de [Azure Functions](../azure-functions/functions-overview.md) para enrutar los datos de telemetría simulados de un dispositivo de [IoT Hub](../iot-hub/about-iot-hub.md) en las propiedades de gemelos digitales.
> * Propagará los cambios con el **grafo de gemelos**, mediante el procesamiento de las notificaciones de los gemelos digitales con Azure Functions, puntos de conexión y rutas.

[!INCLUDE [Azure Digital Twins tutorial: sample prerequisites](../../includes/digital-twins-tutorial-sample-prereqs.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment-h3.md)]

### <a name="set-up-cloud-shell-session"></a>Configuración de una sesión de Cloud Shell
[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

[!INCLUDE [Azure Digital Twins tutorial: configure the sample project](../../includes/digital-twins-tutorial-sample-configure.md)]

## <a name="get-started-with-the-building-scenario"></a>Primeros pasos con el escenario de compilación

El proyecto de ejemplo que se usa en este tutorial representa un **escenario de un edificio** real, que contiene una planta, una habitación y un dispositivo de termostato. Estos componentes se representarán digitalmente en una instancia de Azure Digital Twins, que posteriormente se conectará a [IoT Hub](../iot-hub/about-iot-hub.md), [Event Grid](../event-grid/overview.md) y dos instancias de [Azure Functions](../azure-functions/functions-overview.md) para permitir el movimiento de los datos.

El siguiente diagrama representa todo el escenario. 

En primer lugar, creará la instancia de Azure Digital Twins (la **sección A** del diagrama) y configurará el flujo de datos de telemetría en los gemelos digitales (**flecha B**); después, configurará la propagación de los datos con el grafo de gemelos (**flecha C**).

:::image type="content" source="media/tutorial-end-to-end/building-scenario.png" alt-text="Diagrama del escenario de creación completo, que muestra el flujo de datos desde un dispositivo hacia el servicio Azure Digital Twins y fuera de este a través de varios servicios de Azure.":::

Para recorrer el escenario, interactuará con los componentes de la aplicación de ejemplo previamente escrita que descargó antes.

Estos son los complementos que implementa la aplicación de ejemplo *AdtSampleApp* del escenario del edificio:
* Autenticación de dispositivos 
* Ejemplos de uso del [SDK de .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true) (se encuentran en *CommandLoop.cs*).
* Interfaz de consola para llamar a la API de Azure Digital Twins.
* *SampleClientApp*: una solución de Azure Digital Twins de ejemplo.
* *SampleFunctionsApp*: una aplicación de Azure Functions que actualiza su grafo de Azure Digital Twins con los datos de telemetría de los eventos de IoT Hub y Azure Digital Twins.

### <a name="instantiate-the-pre-created-twin-graph"></a>Instanciación del grafo de gemelos creado previamente

En primer lugar, usará la solución *AdtSampleApp* del proyecto de ejemplo para crear la parte de Azure Digital Twins del escenario de un extremo a otro (**sección A**):

:::image type="content" source="media/tutorial-end-to-end/building-scenario-a.png" alt-text="Diagrama que muestra un extracto del diagrama del escenario de creación completo. Se muestra resaltada la sección de la instancia de Azure Digital Twins.":::

En la ventana de Visual Studio en la que está abierto el proyecto _**AdtE2ESample**_, ejecute el proyecto con este botón de la barra de herramientas:

:::image type="content" source="media/tutorial-end-to-end/start-button-sample.png" alt-text="Captura de pantalla del botón de inicio de Visual Studio con el proyecto SampleClientApp abierto.":::

Se abre una ventana de la consola, se lleva a cabo la autenticación y se espera un comando. En esta consola, ejecute el siguiente comando para crear una instancia de la solución de Azure Digital Twins de ejemplo.

> [!IMPORTANT]
> Si ya tiene gemelos digitales y relaciones en su instancia de Azure Digital Twins, la ejecución de este comando los eliminará y los sustituirá por los gemelos y relaciones del escenario de ejemplo.

```cmd/sh
SetupBuildingScenario
```

La salida de este comando es una serie de mensajes de confirmación cuando se crean y conectan tres [gemelos digitales](concepts-twins-graph.md) en su instancia de Azure Digital Twins: una planta llamada floor1, una habitación llamada room21 y un sensor de temperatura llamado thermostat67. Estos gemelos digitales representan las entidades que existirían en un entorno real.

Se conectan mediante relaciones en el siguiente [grafo de gemelos](concepts-twins-graph.md). El grafo de gemelos representa el entorno como un todo, incluida la forma en que las entidades interactúan entre ellas y se relacionan entre sí.

:::image type="content" source="media/tutorial-end-to-end/building-scenario-graph.png" alt-text="Diagrama que muestra que floor1 contiene room21, y room21 contiene thermostat67." border="false":::

Para comprobar los gemelos que se crearon, ejecute el siguiente comando, que consulta la instancia de Azure Digital Twins conectada en todos los gemelos digitales que contiene:

```cmd/sh
Query
```

[!INCLUDE [digital-twins-query-latency-note.md](../../includes/digital-twins-query-latency-note.md)]

Ahora puede dejar de ejecutar el proyecto. No obstante, mantenga la solución abierta en Visual Studio, ya que la usará más veces en el tutorial.

## <a name="set-up-the-sample-function-app"></a>Configuración de la aplicación de funciones de ejemplo

El siguiente paso es configurar una [aplicación de Azure Functions](../azure-functions/functions-overview.md) que se usará en este tutorial para procesar los datos. La aplicación de funciones, *SampleFunctionsApp*, contiene dos funciones:
* *ProcessHubToDTEvents*: procesa los datos entrantes de IoT Hub y actualiza Azure Digital Twins.
* *ProcessDTRoutedData*: procesa los datos de gemelos digitales y actualiza los gemelos principales de Azure Digital Twins.

En esta sección, publicará la aplicación de funciones previamente escrita y se asegurará de que esta pueda acceder a Azure Digital Twins, asignándole una identidad de Azure Active Directory (Azure AD). Si se realizan estos pasos, el resto del tutorial podrá usar las funciones dentro de la aplicación de funciones. 

De vuelta en la ventana de Visual Studio en la que está abierto el proyecto _**AdtE2ESample**_ la aplicación de funciones se encuentra en el archivo del proyecto _**SampleFunctionsApp**_. Puede verla en el panel *Explorador de soluciones*.

### <a name="update-dependencies"></a>Actualización de las dependencias

Antes de publicar la aplicación, se recomienda asegurarse de que las dependencias están actualizadas y de que tiene la versión más reciente de todos los paquetes incluidos.

En el panel *Explorador de soluciones*, expanda _**SampleFunctionsApp** > Dependencias_. Haga clic con el botón derecho en *Paquetes* y elija *Administrar paquetes NuGet...* .

:::image type="content" source="media/tutorial-end-to-end/update-dependencies-1.png" alt-text="Captura de pantalla de Visual Studio que muestra el botón de menú &quot;Administrar paquetes NuGet&quot; para el proyecto SampleFunctionsApp." border="false":::

Al hacerlo, se abrirá el administrador de paquetes NuGet. Seleccione la pestaña *Actualizaciones* y, si hay paquetes que actualizar, active la casilla *Seleccionar todos los paquetes*. Después, seleccione *Actualizar*.

:::image type="content" source="media/tutorial-end-to-end/update-dependencies-2.png" alt-text="Captura de pantalla de Visual Studio que muestra cómo seleccionar las opciones para actualizar todos los paquetes en el Administrador de paquetes NuGet.":::

### <a name="publish-the-app"></a>Publicación de la aplicación

Para publicar la aplicación de funciones en Azure, primero debe crear una cuenta de almacenamiento, después crear la aplicación de funciones en Azure y, por último, publicar las funciones en la aplicación de funciones de Azure. En esta sección se completan estas acciones mediante la CLI de Azure.

1. Cree una **cuenta de Azure Storage** mediante la ejecución del comando siguiente:

    ```azurecli-interactive
    az storage account create --name <name-for-new-storage-account> --location <location> --resource-group <resource-group> --sku Standard_LRS
    ```

1. Cree una **aplicación de funciones de Azure** mediante la ejecución del comando siguiente:

    ```azurecli-interactive
    az functionapp create --name <name-for-new-function-app> --storage-account <name-of-storage-account-from-previous-step> --consumption-plan-location <location> --runtime dotnet --resource-group <resource-group>
    ```

1. A continuación, **comprimirá** las funciones y las **publicará** en la nueva aplicación de funciones de Azure.

    1. Abra un terminal como PowerShell en el equipo local y vaya al [repositorio de ejemplos de Digital Twins](https://github.com/azure-samples/digital-twins-samples/tree/master/) que descargó anteriormente en el tutorial. Dentro de la carpeta del repositorio descargado, vaya a *digital-twins-samples-master\AdtSampleApp\SampleFunctionsApp*.
    
    1. En el terminal, ejecute el siguiente comando para publicar el proyecto:

        ```powershell
        dotnet publish -c Release
        ```

        Este comando publica el proyecto en el directorio *digital-twins-samples-master\AdtSampleApp\SampleFunctionsApp\bin\Release\netcoreapp3.1\publish*.

    1. Cree un archivo .zip de los archivos publicados que se encuentran en el directorio *digital-twins-samples-master\AdtSampleApp\SampleFunctionsApp\bin\Release\netcoreapp3.1\publish*. 
        
        Si usa PowerShell, puede crear el archivo zip copiando la ruta de acceso completa a ese directorio *\publish* y pegándola en el siguiente comando:
    
        ```powershell
        Compress-Archive -Path <full-path-to-publish-directory>\* -DestinationPath .\publish.zip
        ```

        El cmdlet creará un archivo **publish.zip** en la ubicación del directorio del terminal que incluye un archivo *host.json*, así como los directorios *bin*, *ProcessDTRoutedData* y *ProcessHubToDTEvents*.

        Si no usa PowerShell y no tiene acceso al cmdlet `Compress-Archive`, deberá comprimir los archivos mediante el Explorador de archivos u otro método.

1. En la CLI de Azure, ejecute el siguiente comando para implementar las funciones publicadas y comprimidas en la aplicación de funciones de Azure:

    ```azurecli-interactive
    az functionapp deployment source config-zip --resource-group <resource-group> --name <name-of-your-function-app> --src "<full-path-to-publish.zip>"
    ```

    > [!NOTE]
    > Si usa la CLI de Azure localmente, puede acceder al archivo ZIP en el equipo directamente mediante su ruta de acceso de la máquina.
    > 
    >Si usa el archivo Azure Cloud Shell, cargue el archivo ZIP en Cloud Shell con este botón antes de ejecutar el comando:
    >
    > :::image type="content" source="media/tutorial-end-to-end/azure-cloud-shell-upload.png" alt-text="Captura de pantalla de Azure Cloud Shell que resalta cómo cargar archivos.":::
    >
    > En este caso, el archivo se cargará en el directorio raíz del almacenamiento de Cloud Shell, por lo que puede hacer referencia al archivo directamente por su nombre para el parámetro `--src` del comando (como en `--src publish.zip`).

    Una implementación correcta responderá con el código de estado 202 y generará un objeto JSON que contiene los detalles de la nueva función. Puede confirmar que la implementación es correcta buscando este campo en el resultado:

    ```json
    {
      ...
      "provisioningState": "Succeeded",
      ...
    }
    ```

Ahora ha publicado las funciones en una aplicación de funciones en Azure.

A continuación, la aplicación de funciones deberá tener el permiso correcto para acceder a la instancia de Azure Digital Twins. Configurará este acceso en la sección siguiente.

### <a name="configure-permissions-for-the-function-app"></a>Configuración de los permisos de la aplicación de funciones

Hay dos configuraciones que deben establecerse para que la aplicación de funciones acceda a la instancia de Azure Digital Twins, ambas se pueden realizar mediante la CLI de Azure. 

#### <a name="assign-access-role"></a>Asignación de roles de acceso

La primera configuración proporciona a la aplicación de funciones el rol de **Propietario de datos de Azure Digital Twins** en la instancia de Azure Digital Twins. Este rol es necesario para cualquier usuario o función que desee realizar muchas actividades en el plano de datos en la instancia. Puede leer más sobre la seguridad y las asignaciones de roles en [Seguridad para las soluciones de Azure Digital Twins](concepts-security.md). 

1. Use el siguiente comando para ver los detalles de la identidad administrada por el sistema de la función. Anote el valor del campo **principalId** de la salida.

    ```azurecli-interactive 
    az functionapp identity show --resource-group <your-resource-group> --name <your-function-app-name> 
    ```

    >[!NOTE]
    > Si el resultado está vacío en lugar de mostrar los detalles de una identidad, cree una nueva identidad administrada por el sistema para la función mediante este comando:
    > 
    >```azurecli-interactive    
    >az functionapp identity assign --resource-group <your-resource-group> --name <your-function-app-name>  
    >```
    >
    > La salida mostrará los detalles de la identidad, incluido el valor de **principalId** requerido para el siguiente paso. 

1. Use el valor de **principalId** en el siguiente comando para asignar la identidad de la aplicación de funciones al rol **Propietario de datos de Azure Digital Twins** en la instancia de Azure Digital Twins.

    ```azurecli-interactive 
    az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Data Owner"
    ```

El resultado de este comando es la información de salida acerca de la asignación de roles que ha creado. Ahora, la aplicación de funciones tiene los permisos necesarios para acceder a los datos de la instancia de Azure Digital Twins.

#### <a name="configure-application-settings"></a>Configuración de la aplicación

El segundo valor crea una **variable de entorno** para la función con la dirección URL de la instancia de Azure Digital Twins. El código de la función usará el valor de esta variable para hacer referencia a la instancia. Para más información sobre las variables de entorno, consulte [Administración de la aplicación de funciones](../azure-functions/functions-how-to-use-azure-function-app-settings.md?tabs=portal). 

Para ejecutar el siguiente comando, rellene los marcadores de posición con los detalles de los recursos.

```azurecli-interactive
az functionapp config appsettings set --resource-group <your-resource-group> --name <your-function-app-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-host-name>"
```

La salida es la lista de valores de la función de Azure, que ahora debe contener una entrada denominada **ADT_SERVICE_URL**.


## <a name="process-simulated-telemetry-from-an-iot-hub-device"></a>Procesamiento de datos de telemetría simulados de un dispositivo de IoT Hub

Los grafos de Azure Digital Twins los controlan los datos de telemetría de los dispositivos reales. 

En este paso, conectará un dispositivo termostato simulado registrado en [IoT Hub](../iot-hub/about-iot-hub.md) al gemelo digital que lo representa en Azure Digital Twins. Cuando el dispositivo simulado emita datos de telemetría, los datos pasarán por la función de Azure *ProcessHubToDTEvents*, que desencadenará la correspondiente actualización en el gemelo digital. De esta forma, el gemelo digital permanece actualizado con los datos del dispositivo real. En Azure Digital Twins, el proceso de dirigir datos de eventos de un lugar a otro se denomina [enrutamiento de eventos](concepts-route-events.md).

El proceso de la telemetría simulada se produce en esta parte del escenario completo (**flecha B**):

:::image type="content" source="media/tutorial-end-to-end/building-scenario-b.png" alt-text="Diagrama que muestra un extracto del diagrama del escenario de creación completo. Se muestra resaltada la sección que ilustra los elementos antes de Azure Digital Twins.":::

Estas son las acciones que se deben realizar para configurar la conexión de este dispositivo:
1. Crear un centro de IoT que administrará el dispositivo simulado.
2. Conectar el centro de IoT a la función de Azure apropiada mediante la configuración de una suscripción al evento.
3. Registrar el dispositivo simulado en el centro de IoT.
4. Ejecutar el dispositivo simulado y generar datos de telemetría.
5. Consultar Azure Digital Twins para ver los resultados en directo

### <a name="create-an-iot-hub-instance"></a>Creación de una instancia de IoT Hub

Azure Digital Twins está diseñado para trabajar con [IoT Hub](../iot-hub/about-iot-hub.md), un servicio de Azure para administrar dispositivos y sus datos. En este paso, configurará un centro de IoT que administrará el dispositivo de ejemplo en este tutorial.

En Azure Cloud Shell, use este comando para crear un centro de IoT:

```azurecli-interactive
az iot hub create --name <name-for-your-IoT-hub> --resource-group <your-resource-group> --sku S1
```

La salida de este comando es información sobre el centro de IoT que se ha creado.

Guarde el **nombre** que asignó al centro de IoT. La usará más adelante.

### <a name="connect-the-iot-hub-to-the-azure-function"></a>Conexión del centro de IoT a la función de Azure

A continuación, conecte su centro de IoT a la función de Azure *ProcessHubToDTEvents* en la aplicación de funciones que publicó antes, con el fin de que los datos puedan fluir desde el dispositivo de IoT Hub a través de la función, que actualiza Azure Digital Twins.

Para ello, creara una **suscripción de eventos** en su centro de IoT, con la función de Azure como punto de conexión. Así se "subscribe" la función a los eventos que suceden en IoT Hub.

En [Azure Portal](https://portal.azure.com/), vaya al centro de IoT recién creado, para lo que debe buscar su nombre en la barra de búsqueda superior. Seleccione *Eventos* en el menú del centro y seleccione *+ Suscripción de eventos*.

:::image type="content" source="media/tutorial-end-to-end/event-subscription-1.png" alt-text="Captura de pantalla de Azure Portal que muestra la suscripción de eventos de IoT Hub.":::

Al seleccionar esta opción, aparecerá la página *Crear suscripción de eventos*.

:::image type="content" source="media/tutorial-end-to-end/event-subscription-2.png" alt-text="Captura de pantalla de Azure Portal que muestra cómo crear una suscripción de eventos.":::

Rellene los campos como se indica a continuación (no se mencionan los campos rellenados de forma predeterminada):
* *DETALLES DE SUSCRIPCIONES DE EVENTOS* > **Nombre**: asigne un nombre a su suscripción de eventos.
* *DETALLES DEL TEMA* > **Nombre del tema del sistema**: asigne un nombre que se utilizará para el tema del sistema. 
* *TIPOS DE EVENTO* > **Filtro para tipos de evento**: Seleccione *Telemetría de dispositivo* en las opciones de menú.
* *DETALLES DE PUNTO DE CONEXIÓN* > **Tipo de punto de conexión**: Seleccione *Función de Azure* en las opciones del menú.
* *DETALLES DE PUNTO DE CONEXIÓN* > **Punto de conexión**: seleccione el vínculo *Seleccionar un extremo* que abrirá una ventana *Seleccionar la función de Azure*: :::image type="content" source="media/tutorial-end-to-end/event-subscription-3.png" alt-text="Captura de pantalla de la suscripción de eventos de Azure Portal que muestra la ventana para seleccionar una función de Azure." border="false":::
    - Rellene los campos **Suscripción**, **Grupo de recursos**, **Aplicación de funciones** y **Función** (*ProcessHubToDTEvents*). Es posible que algunos de estos valores se rellenen automáticamente después de seleccionar la suscripción.
    - Seleccione **Confirm Selection** (Confirmar selección).

De nuevo en la página *Crear suscripción de eventos*, seleccione **Crear**.

### <a name="register-the-simulated-device-with-iot-hub"></a>Registro del dispositivo simulado en el centro de IoT 

En esta sección se crea una representación de un dispositivo en IoT Hub con el identificador thermostat67. El dispositivo simulado se conectará a esta representación, que es la forma en que los eventos de telemetría pasarán del dispositivo al IoT Hub. El centro de IoT es donde la función de Azure suscrita del paso anterior escucha, lista para seleccionar los eventos y continuar el procesamiento.

En Azure Cloud Shell, cree un dispositivo en IoT Hub con el siguiente comando:

```azurecli-interactive
az iot hub device-identity create --device-id thermostat67 --hub-name <your-IoT-hub-name> --resource-group <your-resource-group>
```

La salida es información acerca del dispositivo creado.

### <a name="configure-and-run-the-simulation"></a>Configuración y ejecución de la simulación

A continuación, configure el simulador de dispositivos para enviar datos a su instancia de IoT Hub.

Para empezar, obtenga la *cadena de conexión de IoT Hub* con este comando:

```azurecli-interactive
az iot hub connection-string show --hub-name <your-IoT-hub-name>
```

Luego, obtenga la *cadena de conexión del dispositivo* con este comando:

```azurecli-interactive
az iot hub device-identity connection-string show --device-id thermostat67 --hub-name <your-IoT-hub-name>
```

Conectará estos valores al código de simulador de dispositivo en el proyecto local para conectar el simulador a este centro de IoT y este dispositivo de IoT Hub.

En una nueva ventana de Visual Studio, abra (desde la carpeta de la solución descargada) _Device Simulator (Simulador de dispositivos) > **DeviceSimulator.sln**_.

>[!NOTE]
> Ahora debería tener dos ventanas de Visual Studio, una con _**DeviceSimulator.sln**_ y otra anterior con _**AdtE2ESample.sln**_.

En el panel del *Explorador de soluciones* de esta nueva ventana de Visual Studio, seleccione _DeviceSimulator/**AzureIoTHub.cs**_ para abrirlo en la ventana de edición. Cambie los siguientes valores de la cadena de conexión por los valores que recopiló anteriormente:

```csharp
iotHubConnectionString = <your-hub-connection-string>
deviceConnectionString = <your-device-connection-string>
```

Guarde el archivo.

Ahora, para ver los resultados de la simulación de datos que ha configurado, ejecute el proyecto **DeviceSimulator** con este botón en la barra de herramientas:

:::image type="content" source="media/tutorial-end-to-end/start-button-simulator.png" alt-text="Captura de pantalla del botón de inicio de Visual Studio con el proyecto DeviceSimulator abierto.":::

Se abrirá una ventana de la consola y se mostrarán los mensajes de los datos de telemetría de temperatura simulados. Estos mensajes se envían a IoT Hub, donde la función de Azure los recoge y procesa.

:::image type="content" source="media/tutorial-end-to-end/console-simulator-telemetry.png" alt-text="Captura de pantalla de la salida de la consola del simulador de dispositivos que muestra los datos de telemetría de temperatura que se enviarán.":::

En esta consola no es preciso hacer nada más, solo dejar que se ejecute mientras se completan los pasos siguientes.

### <a name="see-the-results-in-azure-digital-twins"></a>Visualización de los resultados en Azure Digital Twins

La función *ProcessHubToDTEvents* que publicó anteriormente escucha los datos de IoT Hub y llama a una API de Azure Digital Twins para actualizar la propiedad *Temperature* en el gemelo thermostat67.

Para ver los datos de Azure Digital Twins, vaya a la ventana de Visual Studio donde está abierto el proyecto _**AdtE2ESample**_ y ejecútelo.

En la ventana de la consola del proyecto que se abre, ejecute el siguiente comando para obtener las temperaturas que se indican en el gemelo digital thermostat67:

```cmd
ObserveProperties thermostat67 Temperature
```

Verá que las temperaturas se actualizan en directo *desde la instancia de Azure Digital Twins* y se registran en la consola cada 2 segundos.

>[!NOTE]
> Es posible que los datos del dispositivo tarden unos segundos en propagarse en el gemelo. Las primeras lecturas de temperatura pueden aparecer como 0 antes de que los datos comiencen a llegar.

:::image type="content" source="media/tutorial-end-to-end/console-digital-twins-telemetry.png" alt-text="Captura de pantalla de la salida de la consola que muestra el registro de mensajes de temperatura del gemelo digital thermostat67.":::

Una vez que haya comprobado el correcto funcionamiento del registro de temperaturas activo, puede dejar de ejecutar ambos proyectos. Mantenga abierta la ventana de Visual Studio, ya que se utilizará durante el resto del tutorial.

## <a name="propagate-azure-digital-twins-events-through-the-graph"></a>Propagación de eventos de Azure Digital Twins a través del grafo

Hasta ahora, en este tutorial ha visto cómo se puede actualizar Azure Digital Twins a partir de datos de dispositivos externos. A continuación, verá cómo se pueden propagar los cambios que se realicen en un gemelo digital mediante el grafo de Azure Digital Twins (es decir, cómo actualizar los gemelos a partir de los datos internos del servicio).

Para hacerlo, usará la función de Azure *ProcessDTRoutedData* para actualizar un gemelo Room cuando el gemelo Thermostat conectado se actualice. La funcionalidad de actualización se produce en esta parte del escenario completo (**flecha C**):

:::image type="content" source="media/tutorial-end-to-end/building-scenario-c.png" alt-text="Diagrama que muestra un extracto del diagrama del escenario de creación completo. Se muestra resaltada la sección que ilustra los elementos después de Azure Digital Twins.":::

Estas son las acciones que realizará para configurar este flujo de datos:
1. [Creación de un tema de Event Grid](#create-the-event-grid-topic) para permitir el movimiento de datos entre servicios de Azure
1. [Creación de un punto de conexión](#create-the-endpoint) en Azure Digital Twins que conecte la instancia al tema de Event Grid.
1. [Configuración de una ruta](#create-the-route) en Azure Digital Twins que envíe los eventos de cambio de propiedades de los gemelos al punto de conexión.
1. [Configuración de una función de Azure](#connect-the-azure-function) que escuche en el tema de Event Grid en el punto de conexión, reciba los eventos de cambio de propiedad de los gemelos que se envían allí y actualice otros gemelos del gráfico en consecuencia.

[!INCLUDE [digital-twins-twin-to-twin-resources.md](../../includes/digital-twins-twin-to-twin-resources.md)]

### <a name="connect-the-azure-function"></a>Conexión a la función de Azure

A continuación, suscriba la función de Azure *ProcessDTRoutedData* al tema de Event Grid que creó anteriormente, con el fin de que los datos de telemetría puedan fluir desde el gemelo thermostat67 a través del tema de Event Grid hasta la función, que vuelve a Azure Digital Twins y actualiza el gemelo room21 en consecuencia.

Para hacerlo, creará una **suscripción a Event Grid** que envíe datos del **tema de Event Grid** que creó anteriormente a la función *ProcessDTRoutedData* de Azure.

En [Azure Portal](https://portal.azure.com/), busque el nombre de su tema de Event Grid en la barra de búsqueda superior para ir a él. Seleccione *+ Suscripción de eventos*.

:::image type="content" source="media/tutorial-end-to-end/event-subscription-1b.png" alt-text="Captura de pantalla de Azure Portal que muestra cómo crear una suscripción de eventos de Event Grid.":::

Los pasos para crear esta suscripción de eventos son similares a los que dio cuando suscribió la primera función de Azure a IoT Hub en este mismo tutorial. Esta vez no es preciso especificar *Telemetría del dispositivo* como el tipo de evento que hay que escuchar y que se conectará a otra función de Azure.

En la página *Crear suscripción de eventos*, rellene los campos como se indica a continuación (no se mencionan los campos rellenados de forma predeterminada):
* *DETALLES DE SUSCRIPCIONES DE EVENTOS* > **Nombre**: asigne un nombre a su suscripción de eventos.
* *DETALLES DE PUNTO DE CONEXIÓN* > **Tipo de punto de conexión**: Seleccione *Función de Azure* en las opciones del menú.
* *DETALLES DE PUNTO DE CONEXIÓN* > **Punto de conexión**: seleccione el vínculo *Seleccionar un extremo* que abrirá una ventana *Seleccionar la función de Azure*:
    - Rellene los campos **Suscripción**, **Grupo de recursos**, **Aplicación de funciones** y **Función** (*ProcessDTRoutedData*). Es posible que algunos de estos valores se rellenen automáticamente después de seleccionar la suscripción.
    - Seleccione **Confirm Selection** (Confirmar selección).

De nuevo en la página *Crear suscripción de eventos*, seleccione **Crear**.

## <a name="run-the-simulation-and-see-the-results"></a>Ejecución de la simulación y visualización de los resultados

Ahora, los eventos deben poder fluir desde el dispositivo simulado a Azure Digital Twins y a través del grafo de Azure Digital Twins para actualizar los gemelos según corresponda. En esta sección, volverá a ejecutar el simulador de dispositivos para iniciar el flujo de eventos completo que ha configurado y consultará Azure Digital Twins para ver los resultados en directo.

Vaya a la ventana de Visual Studio en que esté abierto el proyecto _**DeviceSimulator**_ y ejecútelo.

Igual que pasó cuando ejecutó el simulador de dispositivo, se abrirá una ventana de la consola y se mostrarán los mensajes de los datos de telemetría de temperatura simulados. Estos eventos atraviesan el flujo que configuró anteriormente para actualizar el gemelo thermostat67 y, después, atraviesan el flujo configurado recientemente para actualizar el gemelo room21 para que coincidan.

:::image type="content" source="media/tutorial-end-to-end/console-simulator-telemetry.png" alt-text="Captura de pantalla de la salida de la consola del simulador de dispositivos que muestra los datos de telemetría de temperatura que se enviarán.":::

En esta consola no es preciso hacer nada más, solo dejar que se ejecute mientras se completan los pasos siguientes.

Para ver los datos de Azure Digital Twins, vaya a la ventana de Visual Studio donde está abierto el proyecto _**AdtE2ESample**_ y ejecútelo.

En la ventana de la consola del proyecto que se abre, ejecute el siguiente comando para obtener las temperaturas que se indican en **ambos** gemelos digitales, thermostat67 y room21.

```cmd
ObserveProperties thermostat67 Temperature room21 Temperature
```

Verá que las temperaturas se actualizan en directo *desde la instancia de Azure Digital Twins* y se registran en la consola cada 2 segundos. Tenga en cuenta que la temperatura de room21 se está actualizando para que coincida con las actualizaciones de thermostat67.

:::image type="content" source="media/tutorial-end-to-end/console-digital-twins-telemetry-b.png" alt-text="Captura de pantalla de la salida de la consola que muestra el registro de mensajes de temperatura de un termostato y una habitación.":::

Una vez que haya comprobado el correcto funcionamiento del registro de temperaturas activo de la instancia, puede dejar de ejecutar ambos proyectos. También puede cerrar las ventanas de Visual Studio, ya que ahora se ha completado el tutorial.

## <a name="review"></a>Revisar

Esta es una revisión del escenario que se ha creado en este tutorial.

1. Una instancia de Azure Digital Twins representa de forma digital una planta, una habitación y un termostato (representado por **sección A** en el diagrama siguiente)
2. Los datos de telemetría del dispositivo simulados se envían a IoT Hub, donde la función de Azure *ProcessHubToDTEvents* escucha los eventos de telemetría. La función de Azure *ProcessHubToDTEvents* usa la información de estos eventos para establecer la propiedad *Temperature* en thermostat67 (**flecha B** en el diagrama).
3. Los eventos de cambio de propiedad de Azure Digital Twins se enrutan a un tema de Event Grid, donde la función de Azure *ProcessDTRoutedData* escucha los eventos. La función de Azure *ProcessDTRoutedData* usa la información de estos eventos para establecer la propiedad *Temperature* en room21 (**flecha C** en el diagrama).

:::image type="content" source="media/tutorial-end-to-end/building-scenario.png" alt-text="Diagrama del escenario de creación completo, que muestra el flujo de datos desde un dispositivo hacia el servicio Azure Digital Twins y fuera de este a través de varios servicios de Azure.":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Después de completar este tutorial, puede elegir los recursos que quiere quitar en función de lo que quiera hacer a continuación.

[!INCLUDE [digital-twins-cleanup-basic.md](../../includes/digital-twins-cleanup-basic.md)]

* **Si quiere seguir usando la instancia de Azure Digital Twins que ha configurado en este artículo, pero quiere borrar algunos o todos sus modelos, gemelos y relaciones**, puede usar los comandos [az dt](/cli/azure/dt?view=azure-cli-latest&preserve-view=true) de la CLI en una ventana de [Azure Cloud Shell](https://shell.azure.com) para eliminar los elementos que quiera quitar.

    Esta opción no quitará ninguno de los otros recursos de Azure creados en este tutorial (IoT Hub, aplicación de Azure Functions, etc.). Puede eliminarlos individualmente mediante los [comandos dt](/cli/azure/reference-index?view=azure-cli-latest&preserve-view=true) adecuados para cada tipo de recurso.

También puede que desee eliminar la carpeta del proyecto de la máquina local.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un escenario de un extremo a otro que muestra la forma en que los datos de dispositivos activos controlan Azure Digital Twins.

A continuación, consulte la documentación sobre conceptos para más información sobre los elementos con los que ha trabajado en el tutorial:

> [!div class="nextstepaction"]
> [Modelos personalizados](concepts-models.md)