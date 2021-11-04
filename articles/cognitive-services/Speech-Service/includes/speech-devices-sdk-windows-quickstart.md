---
author: eric-urban
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 02/20/2020
ms.author: eur
ms.openlocfilehash: dc39dd5f63b0e4ce1526ac93c8c416eb567852b0
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131509761"
---
En este inicio rápido, aprenderá a usar Speech Devices SDK para Windows para crear un producto habilitado para voz o para utilizarlo como un dispositivo de [transcripción de conversaciones](../conversation-transcription.md). Para la transcripción de conversaciones, solo se admite el [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/). En el caso de otros tipos de voz, se admiten matrices de micrófonos lineales que proporcionen una geometría de matriz de micrófonos.

La aplicación se crea con el paquete del SDK de Voz y el entorno de desarrollo integrado de Java de Eclipse (v4) en Windows de 64 bits. Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

Para esta guía se requiere una cuenta de [Azure Cognitive Services](../overview.md#try-the-speech-service-for-free) con un recurso del servicio de voz.

El código fuente de la [aplicación de ejemplo](https://aka.ms/sdsdk-download-JRE) se incluye con Speech Devices SDK. También está [disponible en GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Prerrequisitos

Esta guía de inicio rápido requiere:

* Sistema operativo: Windows de 64 bits
* Una matriz de micrófonos, como [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)
* [IDE de Java de Eclipse](https://www.eclipse.org/downloads/)
* Solo [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) o [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Microsoft Visual C++ Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](../overview.md#try-the-speech-service-for-free).
* Descargue la versión más reciente de [Speech Devices SDK](https://aka.ms/sdsdk-download-JRE) para Java y extraiga el archivo .zip en el directorio de trabajo.
   > [!NOTE]
   > En esta guía de inicio rápido se supone que la aplicación se extrae en C:\SDSDK\JRE-Sample-Release

Si tiene previsto usar intenciones necesitará una suscripción a [Language Understanding Service (LUIS)](../../luis/luis-how-to-azure-subscription.md). Para más información acerca de LUIS y el reconocimiento de intenciones, consulte [Reconocimiento de las intenciones de voz con LUIS, C#](../how-to-recognize-intents-from-speech-csharp.md). Hay un [modelo de ejemplo de LUIS](https://aka.ms/sdsdk-luis) disponible para esta aplicación.

## <a name="create-and-configure-the-project"></a>Creación y configuración del proyecto

1. Inicie Eclipse.

1. En el **selector de IDE de Eclipse**, en el campo **Workspace** (Área de trabajo), escriba el nombre de un nuevo directorio de área de trabajo. Luego, seleccione **Launch** (Iniciar).

   ![Captura de pantalla que muestra el selector de Eclipse donde se escribe el nombre del directorio del área de trabajo.](../media/speech-devices-sdk/eclipse-launcher.png)

1. Al cabo de unos segundos, aparece la ventana principal del IDE de Eclipse. Cierre la pantalla de bienvenida si hay alguna.

1. En la barra de menús de Eclipse, cree un nuevo proyecto eligiendo **File (Archivo)**  > **New (Nuevo)**  > **Java Project (Proyecto de Java)** . Si no está disponible seleccione **Project** (Proyecto) y, a continuación, **Java Project** (Proyecto de Java).

1. Se inicia el asistente para **nuevo proyecto de Java**. **Busque** la ubicación del proyecto de ejemplo. Seleccione **Finalizar**.

   ![Captura de pantalla que muestra el asistente para nuevo proyecto de Java.](../media/speech-devices-sdk/eclipse-new-java-project.png)

1. En el **Explorador de paquetes**, haga clic con el botón derecho en el proyecto. Elija **Configure (Configurar)**  > **Convert to Maven Project (Convertir en proyecto de Maven)** en el menú contextual. Seleccione **Finalizar**.

   ![Captura de pantalla del Explorador de paquetes](../media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. Abra el archivo pom.xml y edítelo.

    Al final del archivo, antes de la etiqueta de cierre `</project>`, cree los elementos `repositories` y `dependencies`, como se muestra aquí, y asegúrese de que `version` coincida con la versión actual:
    ```xml
    <repositories>
         <repository>
             <id>maven-cognitiveservices-speech</id>
             <name>Microsoft Cognitive Services Speech Maven Repository</name>
             <url>https://csspeechstorage.blob.core.windows.net/maven/</url>
         </repository>
    </repositories>
 
    <dependencies>
        <dependency>
             <groupId>com.microsoft.cognitiveservices.speech</groupId>
             <artifactId>client-sdk</artifactId>
             <version>1.15.0</version>
        </dependency>
    </dependencies>
   ```

1. Copie el contenido de **Windows-x64** en la ubicación del proyecto de Java, por ejemplo **C:\SDSDK\JRE-Sample-Release**.

1. Copie `kws.table`, `participants.properties` y `Microsoft.CognitiveServices.Speech.extension.pma.dll` en la carpeta del proyecto **target\classes**

## <a name="configure-the-sample-application"></a>Configurar la aplicación de ejemplo

1. Agregue la clave de suscripción de Voz al código fuente. Si quiere probar el reconocimiento de intenciones, agregue también la clave de suscripción y el identificador de aplicación del [servicio Language Understanding](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

   Para Voz y LUIS, la información se trasladará a `FunctionsList.java`:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

   Si usa la transcripción de conversaciones, la información de la clave y la región de Voz también se necesitará en `Cts.java`:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
   ```

1. La palabra clave predeterminada (palabra clave) es "Equipo". Además, puede probar una de las otras palabras clave proporcionadas, como "Máquina" o "Asistente". Los archivos de recursos de estas palabras clave alternativas pueden encontrarse en Speech Devices SDK en la carpeta de palabras clave. Por ejemplo, `C:\SDSDK\JRE-Sample-Release\keyword\Computer` contiene los archivos utilizados para la palabra clave "Equipo".

    > [!TIP]
    > También puede [crear una palabra clave personalizada](../custom-keyword-basics.md).

    Para usar una nueva palabra clave, actualice la línea siguiente en `FunctionsList.java`, y copie la palabra clave en la aplicación. Por ejemplo, para usar la palabra clave "Máquina" desde el paquete de palabras clave `machine.zip`:

   * Copie el archivo `kws.table` del paquete ZIP en la carpeta de proyecto **target/classes**.
   * Actualice `FunctionsList.java` con el nombre de la palabra clave:

     ```java
     private static final String Keyword = "Machine";
     ```

## <a name="run-the-sample-application-from-eclipse"></a>Ejecución de la aplicación de ejemplo de Eclipse

1. En la barra de menús de Eclipse seleccione, **Run (Ejecutar)**  > **Run As (Ejecutar como)**  > **Java Application (Aplicación de Java)** . A continuación, seleccione **FunctionsList** y **OK** (Aceptar).

   ![Captura de pantalla de la aplicación "Select" de Java](../media/speech-devices-sdk/eclipse-run-sample.png)

1. Se inicia la aplicación de ejemplo del SDK de dispositivos de voz y muestra las siguientes opciones:

   ![Captura de pantalla de opciones y aplicación de ejemplo del SDK de dispositivos de Voz de ejemplo.](../media/speech-devices-sdk/java-sample-app-windows.png)

1. Pruebe la nueva demostración de **Transcripción de conversaciones**. Empiece a transcribir con **Sesión** > **Iniciar**. De forma predeterminada, todos los usuarios son invitados. Sin embargo, si dispone de las firmas de voz del participante, se pueden colocar en un archivo `participants.properties` en la carpeta del proyecto **target/classes**. Para generar la firma de voz, consulte [Transcripción de conversaciones (SDK)](../how-to-use-conversation-transcription.md).

   ![Captura de pantalla de una aplicación de demostración de transcripción de conversaciones.](../media/speech-devices-sdk/cts-sample-app-windows.png)

## <a name="create-and-run-a-standalone-application"></a>Creación y ejecución de una aplicación independiente

1. En el **Explorador de paquetes**, haga clic con el botón derecho en el proyecto. Seleccione **Exportar**.

1. Aparecerá la ventana **Exportar**. Expanda **Java** y seleccione **Runnable JAR file** (Archivo JAR ejecutable) y, a continuación, seleccione **Siguiente**.

   ![Captura de pantalla que muestra la ventana Exportar donde se selecciona el archivo JAR ejecutable.](../media/speech-devices-sdk/eclipse-export-windows.png)

1. Aparece la ventana **Runnable JAR File Export** (Exportación de archivo JAR ejecutable). Elija un **Destino de exportación** para la aplicación y, a continuación, seleccione **Finalizar**.

   ![Captura de pantalla que muestra la ventana Runnable JAR File Export (Exportación de archivo JAR ejecutable) donde se selecciona el destino para la exportación.](../media/speech-devices-sdk/eclipse-export-jar-windows.png)

1. Coloque `kws.table`, `participants.properties`, `unimic_runtime.dll`, `pma.dll` y `Microsoft.CognitiveServices.Speech.extension.pma.dll` en la carpeta de destino elegida anteriormente, ya que la aplicación necesita estos archivos.

1. Ejecución de la aplicación independiente

   ```powershell
   java -jar SpeechDemo.jar
   ```
