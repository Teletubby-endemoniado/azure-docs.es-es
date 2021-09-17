---
title: 'Tutorial: Configuración del enrutamiento de mensajes para Azure IoT Hub mediante la CLI de Azure'
description: 'Tutorial: Configuración del enrutamiento de mensajes para Azure IoT Hub mediante la CLI de Azure y Azure Portal'
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 08/16/2021
ms.author: robinsh
ms.custom:
- mvc
- 'Role: Cloud Development'
- 'Role: Data Analytics'
- devx-track-azurecli
ms.openlocfilehash: 1805213d64a7d6feb47033940c3c479713acd688
ms.sourcegitcommit: ddac53ddc870643585f4a1f6dc24e13db25a6ed6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2021
ms.locfileid: "122397256"
---
# <a name="tutorial-use-the-azure-cli-and-azure-portal-to-configure-iot-hub-message-routing"></a>Tutorial: Uso de la CLI de Azure y Azure Portal para configurar el enrutamiento de mensajes de IoT Hub

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="use-the-azure-cli-to-create-the-base-resources"></a>Uso de la CLI de Azure para crear recursos de base

En este tutorial se usa la CLI de Azure para crear los recursos de base y, después, [Azure Portal](https://portal.azure.com), para mostrar cómo se configura el enrutamiento de mensajes y se instala el dispositivo virtual para las pruebas.

Copie y pegue el siguiente script en Cloud Shell y presione Entrar. El script se ejecuta de línea en línea. Con esto creará los recursos de base para este tutorial, incluida la cuenta de almacenamiento, la instancia de IoT Hub, el espacio de nombres de Service Bus y la cola de Service Bus.

Hay varios nombres de recurso que deben ser únicos globalmente, como el nombre de IoT Hub y el nombre de la cuenta de almacenamiento. Para facilitar esta tarea, se anexan los nombres de los recursos con un valor alfanumérico aleatorio denominado *randomValue*. El valor RandomValue se genera una vez en la parte superior del script y se anexa a los nombres de los recursos según sea necesario en el script. Si no desea que sea aleatorio, puede establecerlo en una cadena vacía o en un valor específico.

> [!TIP]
> Una sugerencia de depuración: este script usa el símbolo de continuación (la barra diagonal inversa, `\`) para que el script se lea mejor. Si tiene problemas al ejecutar el script, asegúrese de que la sesión de Cloud Shell esté ejecutando `bash` y de que no hay ningún espacio después de cualquiera de las barras diagonales inversas.
>

```azurecli-interactive
# This retrieves the subscription id of the account 
#   in which you're logged in.
# This field is used to set up the routing queries.
subscriptionID=$(az account show --query id)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# Set the values for the resource names that 
#   don't have to be globally unique.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, 
#   so add a random value to the end.
iotHubName=ContosoTestHub$randomValue 
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub for the 'events' endpoint.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

# The storage account name must be globally unique, 
#   so add a random value to the end.
storageAccountName=contosostorage$randomValue
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key. 
#    You need this to create the container.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"') 

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the container in the storage account. 
az storage container create --name $containerName \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# The Service Bus namespace must be globally unique, 
#   so add a random value to the end.
sbNamespace=ContosoSBNamespace$randomValue
echo "Service Bus namespace = " $sbNamespace

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNamespace \
    --location $location

# The Service Bus queue name must be globally unique, 
#   so add a random value to the end.
sbQueueName=ContosoSBQueue$randomValue
echo "Service Bus queue name = " $sbQueueName

# Create the Service Bus queue to be used as a routing destination.
az servicebus queue create --name $sbQueueName \
    --namespace-name $sbNamespace \
    --resource-group $resourceGroup

```

Ahora que los recursos de base están configurados, puede continuar por el enrutamiento de mensajes en [Azure Portal](https://portal.azure.com).

## <a name="set-up-message-routing"></a>Configuración del enrutamiento de mensajes

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

### <a name="route-to-a-storage-account"></a>Enrutamiento a una cuenta de almacenamiento

Ahora, configure el enrutamiento de la cuenta de almacenamiento. Vaya al panel de enrutamiento de mensajes y agregue una ruta. Al agregar la ruta, defina un nuevo punto de conexión para la misma. Una vez que el enrutamiento esté configurado, los mensajes en los que la propiedad **level** esté establecida en **storage** se escribirán automáticamente en una cuenta de almacenamiento. 

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

Ahora, configure la configuración del enrutamiento de mensajes para Azure Storage.

1. En [Azure Portal](https://portal.azure.com), seleccione **Grupos de recursos** y el grupo de recursos. En este tutorial se usa **ContosoResources**.

2. Seleccione la instancia de IoT Hub en la lista de recursos. En este tutorial se usa **ContosoTestHub**.

3. Seleccione **Enrutamiento de mensajes** en la columna central que dice **_Mensajería_*. Seleccione +** Agregar** para ver el panel **Agregar una ruta**. Seleccione +**Agregar punto de conexión** junto al campo Punto de conexión y, a continuación, seleccione **Storage**. Verá el panel **Agregar un punto de conexión de almacenamiento**.

   ![Empezar a agregar un punto de conexión para una ruta](./media/tutorial-routing/01-add-a-route-to-storage.png)

4. Escriba el nombre del punto de conexión. En este tutorial se usa **ContosoStorageEndpoint**.

   ![Nombre del punto de conexión](./media/tutorial-routing/02-add-a-storage-endpoint.png)

5. Seleccione **Seleccionar un contenedor**. Esto le llevará a una lista donde podrá ver las cuentas de almacenamiento. Seleccione la que ha configurado en los pasos de preparación. En este tutorial, se usa **contosostorage**. Muestra una lista de contenedores en esa cuenta de almacenamiento. **Seleccione** el contenedor que configuró en los pasos de preparación. En este tutorial se usa **contosoresults**. A continuación, haga clic en **Seleccionar** en la parte inferior de la pantalla. Se le dirige a otro panel **Agregar un punto de conexión de almacenamiento**. Verá la dirección URL del contenedor seleccionado. 

6. Establezca la codificación en JSON o AVRO. Para este tutorial, use los valores predeterminados para el resto de los campos. Este campo se atenuará si la región seleccionada no es compatible con la codificación JSON. Establezca el formato de nombre de archivo. 

   > [!NOTE]
   > Establezca el formato del nombre de blob mediante la opción **Formato de nombre de archivo del blob**. El valor predeterminado es `{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}`. El formato debe contener {iothub}, {partición}, {AAAA}, {MM}, {DD}, {HH} y {mm} en cualquier orden.
   >
   > Por ejemplo, con el formato de nombre de archivo del blob predeterminado, si el nombre del centro es ContosoTestHub y la fecha y hora es el 30 de octubre de 2018 a las 10:56 a.m., el nombre del blob tendrá este aspecto: `ContosoTestHub/0/2018/10/30/10/56`.
   > 
   > De manera predeterminada, los blobs se escriben en el formato AVRO.
   >

7. Seleccione **Crear** en la parte inferior de la página para crear el punto de conexión de almacenamiento y agregarlo a la ruta. Volverá al panel **Agregar una ruta**. 

8. Complete el resto de la información de la consulta de enrutamiento. En esta consulta se especifican los criterios para enviar mensajes al contenedor de almacenamiento que acaba de agregar como punto de conexión. Rellene los campos de la pantalla.

9. Rellene el resto de los campos.

   - **Nombre**: escriba el nombre de la ruta. En este tutorial se usa **ContosoStorageRoute**. A continuación, especifique el punto de conexión para el almacenamiento. En este tutorial se usa ContosoStorageEndpoint.
   
   - Especificar **Origen de datos**: seleccione **Mensajes de telemetría del dispositivo** en la lista desplegable.   

   - Seleccione **Habilitar ruta**: asegúrese de que este campo esté establecido en `enabled`.

   - **Consulta de enrutamiento**: escriba `level="storage"` como cadena de consulta.

   ![Guardar la información de la consulta de enrutamiento](./media/tutorial-routing/04-save-storage-route.png)
  
10.  Seleccione **Guardar**. Al terminar, vuelve al panel Enrutamiento de mensajes, donde podrá ver la nueva consulta de enrutamiento del almacenamiento. Cierre el panel Enrutamiento de mensajes, lo que le devolverá al panel Grupo de recursos.


### <a name="route-to-a-service-bus-queue"></a>Ruta a una cola de Service Bus

Ahora, configure el enrutamiento de la cola de Service Bus. Vaya al panel de enrutamiento de mensajes y agregue una ruta. Al agregar la ruta, defina una cola de Service Bus como punto de conexión para la ruta. Después de realizar la configuración de la ruta, los mensajes en los que la propiedad **level** está establecida en **critical** se escriben en la cola de Service Bus, lo que desencadena una aplicación lógica, que, posteriormente, envía un correo electrónico con la información.

1. En la página Grupo de recursos, seleccione IoT Hub y **Enrutamiento de mensajes**.

2. En el panel **Enrutamiento de mensajes**, seleccione **+ Agregar**.

3. En el panel **Agregar una ruta**, seleccione **+ Agregar** junto a **+Punto de conexión**. Seleccione la **cola de Service Bus**. Verá el panel **Add Service Bus Endpoint**  (Agregar punto de conexión de Service Bus).

   ![Incorporación del primer punto de conexión de Service Bus](./media/tutorial-routing/05-setup-sbq-endpoint.png)

4. Rellene el resto de los campos:

   **Nombre del punto de conexión**: Escriba el nombre del punto de conexión. En este tutorial, se usa **ContosoSBQEndpoint**.
   
   **Espacio de nombres de Service Bus**: Use la lista desplegable para seleccionar el espacio de nombres de Service Bus que configuró en los pasos de preparación. En este tutorial se usa **ContosoSBNamespace**.

   **Cola de Service Bus**: use la lista desplegable para seleccionar la cola de Service Bus. En este tutorial se usa **contososbqueue**.

5. Seleccione **Crear** para agregar el primer punto de conexión de cola de Service Bus. Volverá al panel **Agregar una ruta**.

   ![Incorporación del segundo punto de conexión de Service Bus](./media/tutorial-routing/06-save-sbq-endpoint.png)

6. Ahora, complete el resto de la información de la consulta de enrutamiento. En esta consulta, se especifican los criterios para enviar mensajes a la cola de Service Bus que acaba de agregar como punto de conexión. Rellene los campos de la pantalla. 

   **Nombre**: escriba el nombre de la ruta. En este tutorial se usa **ContosoSBQueueRoute**. 

   **Punto de conexión**: seleccione el punto de conexión que acaba de configurar.

   **Origen de datos**: seleccione **Mensajes de telemetría del dispositivo** en la lista desplegable.

   **Habilitar ruta**: establezca este campo en `enable`.

   **Consulta de enrutamiento**: escriba `level="critical"` como la consulta de enrutamiento. 

   ![Creación de una consulta de enrutamiento para la cola de Service Bus](./media/tutorial-routing/07-save-servicebusqueue-route.png)

7. Seleccione **Guardar**. Al regresar al panel Rutas, se ven las dos nuevas rutas.

   ![Rutas recién configuradas](./media/tutorial-routing/08-show-both-routes.png)

8. Para ver los puntos de conexión personalizados que configuró, seleccione la pestaña **Puntos de conexión personalizados**.

   ![Puntos de conexión recién configurados](./media/tutorial-routing/09-show-custom-endpoints.png)

9. Cierre el panel Enrutamiento de mensajes, lo que le devolverá al panel Grupo de recursos.

## <a name="create-a-simulated-device"></a>Cree un dispositivo simulado:

[!INCLUDE [iot-hub-include-create-simulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene las rutas de mensajes y los recursos configurados, pase al siguiente tutorial para aprender a enviar mensajes a IoT Hub y verlos enrutados a los distintos destinos. 

> [!div class="nextstepaction"]
> [Parte 2: Visualización de los resultados del enrutamiento de mensajes](tutorial-routing-view-message-routing-results.md)
