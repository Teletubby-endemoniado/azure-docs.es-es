---
title: Integración con Azure SignalR Service
titleSuffix: Azure Digital Twins
description: Obtenga información acerca de cómo transmitir telemetría de Azure Digital Twins a clientes mediante Azure SignalR.
author: dejimarquis
ms.author: aymarqui
ms.date: 02/12/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 8690f69bace02e73f144b563a5541073cb976f22
ms.sourcegitcommit: 2da83b54b4adce2f9aeeed9f485bb3dbec6b8023
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2021
ms.locfileid: "122770613"
---
# <a name="integrate-azure-digital-twins-with-azure-signalr-service"></a>Integración de Azure Digital Twins con Azure SignalR Service

En este artículo, aprenderá a integrar Azure Digital Twins con [Azure SignalR Service](../azure-signalr/signalr-overview.md).

La solución que se describe en este artículo le permite insertar datos de telemetría de gemelos digitales en clientes conectados, como una única página web o una aplicación móvil. Como resultado, los clientes se actualizan con métricas y estados en tiempo real de dispositivos IoT sin necesidad de sondear el servidor ni de enviar nuevas solicitudes HTTP para las actualizaciones.

## <a name="prerequisites"></a>Requisitos previos

Estos son los requisitos previos que debe completar antes de continuar:

* Antes de integrar la solución con Azure SignalR Service en este artículo, debe completar el módulo [Conexión de una solución de un extremo a otro](tutorial-end-to-end.md) de Azure Digital Twins, ya que este artículo sobre procedimientos se basa en él. El tutorial le guiará a través de la configuración de una instancia de Azure Digital Twins que funciona con un dispositivo IoT virtual para desencadenar las actualizaciones de gemelos digitales. En este artículo de procedimientos se conectarán esas actualizaciones a una aplicación web de ejemplo mediante Azure SignalR Service.

* Necesitará los siguientes valores del tutorial:
  - Tema de Event Grid
  - Resource group
  - Nombre de la instancia de App Service y de la aplicación de funciones
    
* Asegúrese de tener [Node.js](https://nodejs.org/) instalado en la máquina.

Asegúrese de iniciar sesión en [Azure Portal](https://portal.azure.com/) con su cuenta de Azure, ya que deberá usarla en esta guía.

## <a name="solution-architecture"></a>Arquitectura de la solución

Va a asociar Azure SignalR Service a Azure Digital Twins a través de la ruta de acceso siguiente. Las secciones A, B y C del diagrama se toman del diagrama de arquitectura del [requisito previo del tutorial de un extremo a otro](tutorial-end-to-end.md). En este artículo de procedimientos creará la sección D en la arquitectura existente, que incluye dos nuevas funciones de Azure que se comunican tanto con SignalR como con las aplicaciones cliente.

:::image type="content" source="media/how-to-integrate-azure-signalr/signalr-integration-topology.png" alt-text="Diagrama de los servicios de Azure en un escenario de un extremo a otro que muestra los datos que entran y salen de Azure Digital Twins." lightbox="media/how-to-integrate-azure-signalr/signalr-integration-topology.png":::

## <a name="download-the-sample-applications"></a>Descarga de aplicaciones de ejemplo

En primer lugar, descargue las aplicaciones de ejemplo necesarias. Necesitará los dos ejemplos siguientes:
* [Ejemplos de Azure Digital Twins de un extremo a otro](/samples/azure-samples/digital-twins-samples/digital-twins-samples/): este ejemplo incluye la aplicación *AdtSampleApp*, que contiene dos funciones de Azure para mover datos por una instancia de Azure Digital Twins (puede obtener información detallada sobre este escenario en [Conexión de una solución de un extremo a otro](tutorial-end-to-end.md)). También contiene una aplicación de ejemplo *DeviceSimulator* que simula un dispositivo IoT y genera un nuevo valor de temperatura cada segundo.
    - Si aún no ha descargado el ejemplo como parte del tutorial en [Requisitos previos](#prerequisites), [vaya al ejemplo](/samples/azure-samples/digital-twins-samples/digital-twins-samples/) y seleccione el botón *Browse code* (Examinar código) situado debajo del título. Esto lleva al repositorio de GitHub de los ejemplos, que se pueden descargar como archivo .zip si se selecciona el botón *Código* y *Descargar archivo ZIP*.

        :::image type="content" source="media/includes/download-repo-zip.png" alt-text="Captura de pantalla del repositorio digital-twins-samples en GitHub y los pasos para descargarlo como un archivo ZIP." lightbox="media/includes/download-repo-zip.png":::

    Con este botón se descargará una copia del repositorio de ejemplo en la máquina, como **digital-twins-samples-master.zip**. Descomprima la carpeta.
* [Ejemplo de aplicación web de integración de SignalR](/samples/azure-samples/digitaltwins-signalr-webapp-sample/digital-twins-samples/): esta aplicación web de React de ejemplo consumirá datos de telemetría de Azure Digital Twins desde una instancia de Azure SignalR Service.
    -  Vaya al vínculo de ejemplos y use el mismo proceso para descargar una copia del ejemplo en la máquina como, por ejemplo, _**digitaltwins-signalr-webapp-sample-main.zip**_. Descomprima la carpeta.

[!INCLUDE [Create instance](../azure-signalr/includes/signalr-quickstart-create-instance.md)]

Deje abierto Azure Portal en la ventana del explorador, ya que lo volverá a usar en la sección siguiente.

## <a name="publish-and-configure-the-azure-functions-app"></a>Publicación y configuración de la aplicación de Azure Functions

En esta sección configurará dos funciones de Azure:
* **negotiate**: una función de desencadenador de HTTP. Esta función usa el enlace de entrada *SignalRConnectionInfo* para generar y devolver información de conexión válida.
* **broadcast**: una función de desencadenador de [Event Grid](../event-grid/overview.md). Recibe datos de telemetría de Azure Digital Twins a través de Event Grid y usa el enlace de salida de la instancia de *SignalR* que creó en el paso anterior para transmitir el mensaje a todas las aplicaciones cliente conectadas.

Inicie Visual Studio (u otro editor de código de su elección) y abra la solución de código en la carpeta *digital-twins-samples-master > ADTSampleApp*. A continuación, siga estos pasos para crear las funciones:

1. En el proyecto *SampleFunctionsApp*, cree una clase de C# llamada **SignalRFunctions.cs**. Para instrucciones sobre cómo crear una clase, consulte [Desarrollo de Azure Functions con Visual Studio](../azure-functions/functions-develop-vs.md#add-a-function-to-your-project).

1. Reemplace el contenido del archivo de clase por el código siguiente:
    
    :::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/signalRFunction.cs":::

1. En la ventana *Consola del Administrador de paquetes* de Visual Studio o en cualquier ventana de comandos de la máquina, vaya a la carpeta *digital-twins-samples-master\AdtSampleApp\SampleFunctionsApp* y ejecute el siguiente comando para instalar el paquete NuGet `SignalRService` en el proyecto:
    ```cmd
    dotnet add package Microsoft.Azure.WebJobs.Extensions.SignalRService --version 1.2.0
    ```

    La ejecución de este comando debería resolver cualquier problema de dependencia de la clase.

1. Publique la función en Azure. Puede publicarla en la misma aplicación de funciones o servicio de aplicación que usó en el [requisito previo](#prerequisites) del tutorial de un extremo a otro, o bien crear una, si bien se recomienda usar la misma para reducir el número de duplicados. Para obtener instrucciones sobre cómo publicar una función mediante Visual Studio, vea [Desarrollo de Azure Functions con Visual Studio](../azure-functions/functions-develop-vs.md#publish-to-azure).

### <a name="configure-the-function"></a>Configuración de la función

Posteriormente, configure la función para que se comuniquen con la instancia de Azure SignalR. Comenzará recopilando la **cadena de conexión** de la instancia de SignalR y, a continuación, la agregará a la configuración de la aplicación de funciones.

1. Vaya a [Azure Portal](https://portal.azure.com/) y busque el nombre de la instancia de SignalR en la barra de búsqueda de la parte superior del portal. Seleccione la instancia para abrirla.
1. Seleccione **Claves** en el menú de la instancia para ver las cadenas de conexión de la instancia de servicio de SignalR.
1. Seleccione el icono *Copiar* para copiar la cadena de conexión principal.

    :::image type="content" source="media/how-to-integrate-azure-signalr/signalr-keys.png" alt-text="Captura de pantalla de Azure Portal que muestra la página Claves de la instancia de SignalR. Se está copiando la cadena de conexión." lightbox="media/how-to-integrate-azure-signalr/signalr-keys.png":::

1. Por último, agregue la **cadena de conexión** de Azure SignalR a la configuración de la aplicación de funciones con el siguiente comando de la CLI de Azure. Además, reemplace los marcadores de posición por el grupo de recursos y el nombre de App Service o de la aplicación de funciones del [requisito previo del tutorial](how-to-integrate-azure-signalr.md#prerequisites). El comando se puede ejecutar en [Azure Cloud Shell](https://shell.azure.com), o bien localmente si la [CLI de Azure está instalada en la máquina](/cli/azure/install-azure-cli):
 
    ```azurecli-interactive
    az functionapp config appsettings set --resource-group <your-resource-group> --name <your-function-app-name> --settings "AzureSignalRConnectionString=<your-Azure-SignalR-ConnectionString>"
    ```

    La salida de este comando imprime todas las configuraciones de aplicación configuradas para la función de Azure. Busque `AzureSignalRConnectionString` en la parte inferior de la lista para comprobar que se ha agregado.

    :::image type="content" source="media/how-to-integrate-azure-signalr/output-app-setting.png" alt-text="Captura de pantalla de la salida en una ventana de comandos que muestra un elemento de lista denominado &quot;AzureSignalRConnectionString&quot;.":::

## <a name="connect-the-function-to-event-grid"></a>Conexión de la función a Event Grid

A continuación, suscriba la función de Azure *broadcast* al **tema de Event Grid** que creó durante el [requisito previo del tutorial](how-to-integrate-azure-signalr.md#prerequisites). Esta acción permitirá que los datos de telemetría fluyan desde el gemelo thermostat67 por el tema de Event Grid hasta la función. Desde aquí, la función puede transmitir los datos a todos los clientes.

Para retransmitir los datos, creará una **suscripción de eventos** desde el tema de Event Grid a la función de Azure *broadcast* como punto de conexión.

En [Azure Portal](https://portal.azure.com/), busque el nombre de su tema de Event Grid en la barra de búsqueda superior para ir a él. Seleccione *+ Suscripción de eventos*.

:::image type="content" source="media/how-to-integrate-azure-signalr/event-subscription-1b.png" alt-text="Captura de pantalla de cómo crear una suscripción de eventos en Azure Portal.":::

En la página *Crear suscripción de eventos*, rellene los campos como se indica a continuación (no se mencionan los campos rellenados de forma predeterminada):
* *DETALLES DE SUSCRIPCIONES DE EVENTOS* > **Nombre**: asigne un nombre a su suscripción de eventos.
* *DETALLES DE PUNTO DE CONEXIÓN* > **Tipo de punto de conexión**: Seleccione *Función de Azure* en las opciones del menú.
* *DETALLES DE PUNTO DE CONEXIÓN* > **Punto de conexión**: seleccione el vínculo *Seleccionar un extremo* que abrirá una ventana *Seleccionar la función de Azure*:
    - Rellene los campos **Suscripción**, **Grupo de recursos**, **Aplicación de función** y **Función** (*broadcast*). Es posible que algunos de estos campos se rellenen automáticamente después de seleccionar la suscripción.
    - Seleccione **Confirm Selection** (Confirmar selección).

:::image type="content" source="media/how-to-integrate-azure-signalr/create-event-subscription.png" alt-text="Captura de pantalla del formulario para crear una suscripción de eventos en Azure Portal.":::

De nuevo en la página *Crear suscripción de eventos*, seleccione **Crear**.

En este punto, debería ver dos suscripciones de eventos en la página *Tema de Event Grid*.

:::image type="content" source="media/how-to-integrate-azure-signalr/view-event-subscriptions.png" alt-text="Captura de pantalla de Azure Portal que muestra dos suscripciones de eventos en la página del tema de Event Grid." lightbox="media/how-to-integrate-azure-signalr/view-event-subscriptions.png":::

## <a name="configure-and-run-the-web-app"></a>Configuración y ejecución de la aplicación web

En esta sección verá el resultado en acción. Primero, configurará la **aplicación web cliente de ejemplo** para conectarse al flujo de Azure SignalR que ha configurado. Luego, iniciará la **aplicación de ejemplo del dispositivo simulado** que envía datos de telemetría mediante la instancia de Azure Digital Twins. Después de eso, observará cómo los datos del dispositivo simulado actualizan la aplicación web de ejemplo en tiempo real en la aplicación web de ejemplo.

### <a name="configure-the-sample-client-web-app"></a>Configuración de la aplicación web cliente de ejemplo

A continuación, configurará la aplicación web cliente de ejemplo. Para ello, empiece por recopilar la **dirección URL del punto de conexión HTTP** de la función *negotiate* y, a continuación, úsela para configurar el código de la aplicación en la máquina.

1. Vaya a la página [Instancias de Function App](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites/kind/functionapp) de Azure Portal y seleccione su aplicación de funciones de la lista. En el menú de la aplicación, seleccione *Funciones* y elija la función *negotiate*.

    :::image type="content" source="media/how-to-integrate-azure-signalr/functions-negotiate.png" alt-text="Captura de pantalla de las aplicaciones de funciones de Azure Portal con &quot;Funciones&quot; resaltado en el menú y &quot;negotiate&quot; resaltado en la lista de funciones.":::

1. Seleccione *Obtener la dirección URL de la función* y copie el valor **hasta _/api_ (no incluya la última instancia de _/negotiate?_ )** . Usará este valor en el paso siguiente.

    :::image type="content" source="media/how-to-integrate-azure-signalr/get-function-url.png" alt-text="Captura de pantalla de Azure Portal que muestra la función &quot;negotiate&quot; con el botón &quot;Obtener la dirección URL de la función&quot; y la dirección URL de la función resaltada.":::

1. Con Visual Studio o cualquier editor de código de su elección, abra la carpeta _**digitaltwins-signalr-webapp-sample-main**_ descomprimida que descargó en la sección [Descarga de aplicaciones de ejemplo](#download-the-sample-applications).

1. Abra el archivo *src/App.js* y reemplace la dirección URL de función de `HubConnectionBuilder` por la dirección URL del punto de conexión HTTP de la función **negotiate** que guardó anteriormente:

    ```javascript
        const hubConnection = new HubConnectionBuilder()
            .withUrl('<Function-URL>')
            .build();
    ```
1. En el *símbolo del sistema para desarrolladores* de Visual Studio o en cualquier ventana de comandos de la máquina, vaya a la carpeta *digitaltwins-signalr-webapp-sample-main\src*. Ejecute el siguiente comando para instalar los paquetes de nodos dependientes:

    ```cmd
    npm install
    ```

A continuación, defina permisos en la aplicación de función en Azure Portal:
1. En la página [Aplicación de funciones](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites/kind/functionapp) de Azure Portal, seleccione la instancia de su aplicación de función.

1. Desplácese hacia abajo en el menú de la instancia y seleccione *CORS*. En la página CORS, escriba `http://localhost:3000` en el cuadro vacío para agregarlo como origen permitido. Active la casilla *Habilitar Access-Control-Allow-Credentials* y seleccione *Guardar*.

    :::image type="content" source="media/how-to-integrate-azure-signalr/cors-setting-azure-function.png" alt-text="Captura de pantalla de Azure Portal que muestra la configuración de CORS en Azure Functions.":::

### <a name="run-the-device-simulator"></a>Ejecución del simulador de dispositivos

Durante el requisito previo del tutorial de un extremo a otro, [ha configurado el simulador de dispositivos](tutorial-end-to-end.md#configure-and-run-the-simulation) para enviar datos mediante una instancia de IoT Hub a la instancia de Azure Digital Twins.

Ahora, lo único que tiene que hacer es iniciar el proyecto del simulador, que se encuentra en *digital-twins-samples-master > DeviceSimulator > DeviceSimulator.sln*. Si utiliza Visual Studio, puede abrir el proyecto y ejecutarlo con este botón en la barra de herramientas:

:::image type="content" source="media/how-to-integrate-azure-signalr/start-button-simulator.png" alt-text="Captura de pantalla del botón de inicio de Visual Studio con el proyecto DeviceSimulator abierto.":::

Se abrirá una ventana de la consola y se mostrarán los mensajes de los datos de telemetría de temperatura simulados. Estos mensajes se envían mediante la instancia de Azure Digital Twins, donde las funciones de Azure y SignalR los recopilan.

En esta consola no es preciso hacer nada más, solo dejar que se ejecute mientras se completa el paso siguiente.

### <a name="see-the-results"></a>Ver los resultados

Para ver los resultados en acción, inicie la **aplicación web de integración de SignalR de ejemplo**. Puede hacerlo desde cualquier ventana de la consola en la ubicación *digitaltwins-signalr-webapp-sample-main\src*, mediante la ejecución de este comando:

```cmd
npm start
```

Al ejecutar este comando se abrirá una ventana del explorador en la que se ejecutará la aplicación de ejemplo, que muestra un medidor de temperatura visual. Una vez que se ejecute la aplicación, debe empezar a ver los valores de telemetría de temperatura del simulador de dispositivos que se propagan mediante Azure Digital Twins y que refleja la aplicación web en tiempo real.

:::image type="content" source="media/how-to-integrate-azure-signalr/signalr-webapp-output.png" alt-text="Captura de pantalla de la aplicación web cliente de ejemplo que muestra un medidor de temperatura visual. La temperatura reflejada es 67,52.":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite los recursos creados en este artículo, siga estos pasos para eliminarlos. 

Con Azure Cloud Shell o la CLI de Azure local, puede eliminar todos los recursos de Azure de un grupo de recursos mediante el comando [az group delete](/cli/azure/group?view=azure-cli-latest&preserve-view=true#az_group_delete). Al eliminar el grupo de recursos, también se eliminará lo siguiente:
* La instancia de Azure Digital Twins (del tutorial de un extremo a otro).
* La instancia de IoT Hub y el registro del dispositivo central (del tutorial de un extremo a otro).
* El tema de Event Grid y las suscripciones asociadas.
* La aplicación Azure Functions, incluidas las tres funciones y los recursos asociados, como el almacenamiento.
* La instancia de Azure SignalR.

> [!IMPORTANT]
> La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. 

```azurecli-interactive
az group delete --name <your-resource-group>
```

Finalmente, elimine las carpetas de ejemplo del proyecto que descargó en la máquina local (*digital-twins-samples-master.zip* y *digitaltwins-signalr-webapp-sample-main.zip*, y las correspondientes carpetas descomprimidas).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha configurado funciones de Azure con SignalR para transmitir eventos de telemetría de Azure Digital Twins a una aplicación cliente de ejemplo.

A continuación, puede obtener más información acerca de Azure SignalR Service:
* [¿Qué es Azure SignalR Service?](../azure-signalr/signalr-overview.md)

También puede obtener información acerca de la autenticación de Azure SignalR Service con Azure Functions:
* [Autenticación de Azure SignalR Service](../azure-signalr/signalr-tutorial-authenticate-azure-functions.md)