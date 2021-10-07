---
title: Introducción a la administración de dispositivos de Azure IoT Hub (Java) | Microsoft Docs
description: Describe cómo usar la administración de dispositivos de IoT Hub de Azure para iniciar un reinicio del dispositivo remoto. Usará el SDK de dispositivo IoT de Azure para Java con el fin de implementar una aplicación de dispositivo simulado que incluye un método directo, además del SDK de servicio IoT de Azure para Java con el objetivo de implementar una aplicación de servicio que invoque el método directo.
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 08/20/2019
ms.custom: mqtt, devx-track-java, devx-track-azurecli
ms.openlocfilehash: c11c15c5dd206a201c9f535bb49b571c5807514e
ms.sourcegitcommit: 557ed4e74f0629b6d2a543e1228f65a3e01bf3ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2021
ms.locfileid: "129458631"
---
# <a name="get-started-with-device-management-java"></a>Introducción a la administración de dispositivos (Java)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

En este tutorial se muestra cómo realizar las siguientes acciones:

* Usar Azure Portal para un IoT Hub y una identidad de dispositivo en este.

* Cree una aplicación de dispositivo simulado que implemente un método directo para reiniciar el dispositivo. Los métodos directos se invocan desde la nube.

* Cree una aplicación que invoque al método directo de reinicio en la aplicación de dispositivo simulado mediante su instancia de IoT Hub. Luego, esta aplicación supervisa las propiedades notificadas desde el dispositivo para ver cuándo se completa la operación de reinicio.

Al final de este tutorial, tendrá dos aplicaciones de consola de Java:

**simulated-device**. Esta aplicación:

* Se conecta con la instancia de IoT Hub con la identidad de dispositivo que se creó anteriormente.

* Recibe una llamada de método directo de reinicio.

* Simula un reinicio físico.

* Informa la hora de último reinicio a través de una propiedad notificada.

**trigger-reboot**. Esta aplicación:

* Llama a un método directo en la aplicación de dispositivo simulado.

* Muestra la respuesta a la llamada al método directo que envía el dispositivo simulado.

* Muestra las propiedades notificadas actualizadas.

> [!NOTE]
> Para más información sobre los SDK que puede usar para compilar aplicaciones de modo que se ejecuten en dispositivos y en el back-end de la solución, consulte [SDK de IoT de Azure](iot-hub-devguide-sdks.md).

## <a name="prerequisites"></a>Requisitos previos

* [Java SE Development Kit 8](/java/azure/jdk/). Asegúrese de seleccionar **Java 8** en **Long-term support** (Soporte técnico a largo plazo) para obtener descargas de JDK 8.

* [Maven 3](https://maven.apache.org/download.cgi)

* Una cuenta de Azure activa. (En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) en tan solo unos minutos).

* Asegúrese de que el puerto 8883 está abierto en el firewall. En el ejemplo de dispositivo de este artículo se usa el protocolo MQTT, que se comunica mediante el puerto 8883. Este puerto puede estar bloqueado en algunos entornos de red corporativos y educativos. Para más información y para saber cómo solucionar este problema, consulte el artículo sobre la [conexión a IoT Hub (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registro de un nuevo dispositivo en el centro de IoT

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="get-the-iot-hub-connection-string"></a>Obtención de la cadena de conexión de IoT Hub

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Desencadenar un reinicio remoto en el dispositivo con un método directo

En esta sección, creará una aplicación de consola de Java que permite:

1. Invocar un método directo de reinicio en la aplicación de dispositivo simulado.

2. Mostrar la respuesta.

3. Sondear las propiedades notificadas enviadas desde el dispositivo para determinar cuándo se completa el reinicio.

Esta aplicación de consola se conecta a la instancia de IoT Hub para invocar al método directo y leer las propiedades notificadas.

1. Cree una carpeta vacía llamada **dm-get-started**.

2. En la carpeta **dm-get-started**, cree un proyecto de Maven denominado **trigger-reboot** mediante el comando siguiente en el símbolo del sistema:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

3. En el símbolo del sistema, vaya a la carpeta **trigger-reboot**.

4. Con un editor de texto, abra el archivo **pom.xml** en la carpeta **trigger-reboot** y agregue la dependencia siguiente al nodo **dependencies**. Esta dependencia le permite usar el paquete iot-service-client en la aplicación para comunicarse con la instancia de IoT Hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.17.1</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Puede comprobar la versión más reciente de **iot-service-client** mediante la [búsqueda de Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

5. Agregue el nodo **build** después del nodo **dependencies**. Esta configuración indica a Maven que use Java 1.8 para compilar la aplicación:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

6. Guarde y cierre el archivo **pom.xml**.

7. Con un editor de texto, abra el archivo de origen **trigger-reboot\src\main\java\com\mycompany\app\App.java**.

8. Agregue las siguientes instrucciones **import** al archivo:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

9. Agregue las siguientes variables de nivel de clase a la clase **App** . Reemplace `{youriothubconnectionstring}` por la cadena de conexión de IoT Hub que copió anteriormente en [Obtención de la cadena de conexión de IoT Hub](#get-the-iot-hub-connection-string):

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

10. Para implementar un subproceso que lee las propiedades notificadas desde el dispositivo gemelo cada 10 segundos, agregue la clase anidada siguiente a la clase **App**:

    ```java
    private static class ShowReportedProperties implements Runnable {
      public void run() {
        try {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          while (true) {
            System.out.println("Get reported properties from device twin");
            deviceTwins.getTwin(twinDevice);
            System.out.println(twinDevice.reportedPropertiesToString());
            Thread.sleep(10000);
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

11. Modifique la firma del método **main** para generar la siguiente excepción:

    ```java
    public static void main(String[] args) throws IOException
    ```

12. Para invocar al método directo de reinicio en el dispositivo simulado, reemplace el código en el método **main** por el código siguiente:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      System.out.println("Invoke reboot direct method");
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, null);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }
      System.out.println("Invoked reboot on device");
      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }
    ```

13. Para iniciar el subproceso que sondea las propiedades notificadas desde el dispositivo simulado, agregue el código siguiente al método **main**:

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

14. Para poder detener la aplicación, agregue el código siguiente al método **main**:

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

15. Guarde y cierre el archivo **trigger-reboot\src\main\java\com\mycompany\app\App.java**.

16. Compile la aplicación de back-end **trigger-reboot** y corrija los errores. En el símbolo del sistema, vaya a la carpeta **trigger-reboot** y ejecute el comando siguiente:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Creación de una aplicación de dispositivo simulado

En esta sección, creará una aplicación de consola de Java que simula un dispositivo. La aplicación escucha la llamada al método directo de reinicio desde IoT Hub y la responder inmediatamente. Luego, la aplicación se suspende unos instantes para simular el proceso de reinicio antes de usar una propiedad notificada para notificar a la aplicación back-end **trigger-reboot** que se completó el reinicio.

1. En la carpeta **dm-get-started**, cree un proyecto Maven denominado **simulated-device** con el comando siguiente en el símbolo del sistema:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. En el símbolo del sistema, vaya a la nueva carpeta **simulated-device**.

3. Con un editor de texto, abra el archivo **pom.xml** de la carpeta **simulated-device** y agregue la dependencia siguiente al nodo **dependencies**. Esta dependencia le permite usar el paquete iot-service-client en la aplicación para comunicarse con la instancia de IoT Hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.17.5</version>
    </dependency>
    ```

    > [!NOTE]
    > Puede comprobar la versión más reciente de **iot-device-client** mediante la [búsqueda de Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

4. Agregue la siguiente dependencia al nodo **dependencies**. Esta dependencia configura una instrucción NOP para la fachada de registro de Apache [SLF4J](https://www.slf4j.org/), que usa el SDK de cliente de dispositivo para implementar el registro. Esta configuración es opcional, pero si la omite, es posible que vea una advertencia en la consola al ejecutar la aplicación. Para más información sobre el registro en el SDK de cliente de dispositivo, consulte [Registro](https://github.com/Azure/azure-iot-sdk-java/blob/main/device/iot-device-samples/readme.md#logging) en el archivo Léame *Ejemplos para el SDK de dispositivo IoT de Azure para Java*.

    ```xml
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-nop</artifactId>
      <version>1.7.28</version>
    </dependency>
    ```

5. Agregue el nodo **build** después del nodo **dependencies**. Esta configuración indica a Maven que use Java 1.8 para compilar la aplicación:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

6. Guarde y cierre el archivo **pom.xml**.

7. Con un editor de texto, abra el archivo de origen **simulated-device\src\main\java\com\mycompany\app\App.java**.

8. Agregue las siguientes instrucciones **import** al archivo:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    ```

9. Agregue las siguientes variables de nivel de clase a la clase **App** . Reemplace `{yourdeviceconnectionstring}` por la cadena de conexión de dispositivo que anotó en la sección [Registro de un nuevo dispositivo en el centro de IoT](#register-a-new-device-in-the-iot-hub):

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

10. Para implementar un controlador de devolución de llamada para los eventos de estado de método directo, agregue la clase anidada siguiente a la clase **App**:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

11. Para implementar un controlador de devolución de llamada para los eventos de estado de dispositivo gemelo, agregue la clase anidada siguiente a la clase **App**:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

12. Para implementar un controlador de devolución de llamada para los eventos de propiedad, agregue la clase anidada siguiente a la clase **App**:

    ```java
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

13. Para implementar un subproceso que simule el reinicio del dispositivo, agregue la clase anidada siguiente a la clase **App**. El subproceso se suspende durante cinco segundos y luego establece la propiedad notificada **lastReboot**:

    ```java
    protected static class RebootDeviceThread implements Runnable {
      public void run() {
        try {
          System.out.println("Rebooting...");
          Thread.sleep(5000);
          Property property = new Property("lastReboot", LocalDateTime.now());
          Set<Property> properties = new HashSet<Property>();
          properties.add(property);
          client.sendReportedProperties(properties);
          System.out.println("Rebooted");
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

14. Para implementar el método directo en el dispositivo, agregue la clase anidada siguiente a la clase **App**. Cuando la aplicación simulada recibe una llamada al método directo de **reinicio**, devuelve una confirmación al autor de la llamada y luego inicia un subproceso para procesar el reinicio:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "reboot" :
          {
            int status = METHOD_SUCCESS;
            System.out.println("Received reboot request");
            deviceMethodData = new DeviceMethodData(status, "Started reboot");
            RebootDeviceThread rebootThread = new RebootDeviceThread();
            Thread t = new Thread(rebootThread);
            t.start();
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

15. Modifique la signatura del método **main** para generar las excepciones siguientes:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

16. Para crear una instancia de **DeviceClient**, reemplace el código del método **main** por el código siguiente:

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

17. Agregue el código siguiente al método **main** para empezar a escuchar las llamadas al método directo:

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed to direct methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

18. Agregue el código siguiente al método **main** para apagar el simulador de dispositivos:

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

19. Guarde y cierre el archivo simulated-device\src\main\java\com\mycompany\app\App.java.

20. Compile la aplicación **simulated-device** y corrija los errores. En el símbolo del sistema, vaya a la carpeta **simulated-device** y ejecute el comando siguiente:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-apps"></a>Ejecución de las aplicaciones

Ya está preparado para ejecutar las aplicaciones.

1. En un símbolo del sistema en la carpeta **simulated-device**, ejecute el comando siguiente para empezar a escuchar las llamadas al método de reinicio desde el centro de IoT:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Aplicación de dispositivo simulado IoT Hub de Java para escuchar las llamadas al método directo de reinicio](./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png)

2. En un símbolo del sistema en la carpeta **trigger-reboot**, ejecute el comando siguiente para llamar al método de reinicio en el dispositivo simulado desde el centro de IoT:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Aplicación de servicio IoT Hub de Java para llamar al método directo de reinicio](./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png)

3. El dispositivo simulado responde a la llamada al método directo de reinicio:

    ![La aplicación de dispositivo simulado IoT Hub de Java responde a la llamada al método directo](./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png)

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]
