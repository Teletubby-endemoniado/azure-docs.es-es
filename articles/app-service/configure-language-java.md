---
title: Configuración de aplicaciones Java
description: Aprenda a configurar aplicaciones Java para que funcionen en Azure App Service. En este artículo se muestran las tareas de configuración más comunes.
keywords: azure app service, web app, windows, oss, java, tomcat, jboss
author: jasonfreeberg
ms.devlang: java
ms.topic: article
ms.date: 04/12/2019
ms.author: jafreebe
ms.reviewer: cephalin
ms.custom: seodec18, devx-track-java, devx-track-azurecli
zone_pivot_groups: app-service-platform-windows-linux
adobe-target: true
ms.openlocfilehash: bd98cd6a0317400bcae932d9f08f719cb8c7fc0f
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131558922"
---
# <a name="configure-a-java-app-for-azure-app-service"></a>Configuración de una aplicación Java para Azure App Service

Azure App Service permite a los desarrolladores de Java compilar, implementar y escalar rápidamente sus aplicaciones web de Java SE, Tomcat y JBoss EAP en un servicio totalmente administrado. Implemente aplicaciones con complementos de Maven desde la línea de comandos o en editores, como IntelliJ, Eclipse o Visual Studio Code.

Esta guía incluye conceptos clave e instrucciones para los desarrolladores de Java que usan App Service. Si nunca ha usado Azure App Service, debe leer primero la [Guía de inicio rápido de Java](quickstart-java.md). Encontrará respuestas a las preguntas generales sobre el uso de App Service que no son específicas del desarrollo de Java en [Preguntas más frecuentes sobre Azure App Service](faq-configuration-and-management.yml).

## <a name="show-java-version"></a>Visualización de la versión de Java

::: zone pivot="platform-windows"  

Para mostrar la versión actual de Java, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --name <app-name> --resource-group <resource-group-name> --query "[javaVersion, javaContainer, javaContainerVersion]"
```

Para mostrar todas las versiones compatibles de Java, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes | grep java
```

::: zone-end

::: zone pivot="platform-linux"

Para mostrar la versión actual de Java, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Para mostrar todas las versiones compatibles de Java, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep "JAVA\|TOMCAT\|JBOSSEAP"
```

::: zone-end

## <a name="deploying-your-app"></a>Implementación de la aplicación

### <a name="build-tools"></a>Build Tools

#### <a name="maven"></a>Maven
Con el [complemento Maven para Azure Web Apps](https://github.com/microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin), puede preparar fácilmente el proyecto de Java de Maven con un comando en la raíz del proyecto:

```shell
mvn com.microsoft.azure:azure-webapp-maven-plugin:2.2.0:config
```

Este comando agrega un complemento `azure-webapp-maven-plugin` y una configuración relacionada al pedirle que seleccione una aplicación web de Azure existente o que cree una nueva. Luego, puede implementar la aplicación Java en Azure mediante el siguiente comando:
```shell
mvn package azure-webapp:deploy
```

Esta es una configuración de ejemplo de `pom.xml`:
```xml
<plugin> 
  <groupId>com.microsoft.azure</groupId>  
  <artifactId>azure-webapp-maven-plugin</artifactId>  
  <version>2.2.0</version>  
  <configuration>
    <subscriptionId>111111-11111-11111-1111111</subscriptionId>
    <resourceGroup>spring-boot-xxxxxxxxxx-rg</resourceGroup>
    <appName>spring-boot-xxxxxxxxxx</appName>
    <pricingTier>B2</pricingTier>
    <region>westus</region>
    <runtime>
      <os>Linux</os>      
      <webContainer>Java SE</webContainer>
      <javaVersion>Java 11</javaVersion>
    </runtime>
    <deployment>
      <resources>
        <resource>
          <type>jar</type>
          <directory>${project.basedir}/target</directory>
          <includes>
            <include>*.jar</include>
          </includes>
        </resource>
      </resources>
    </deployment>
  </configuration>
</plugin> 
```

#### <a name="gradle"></a>Gradle
1. Configure el [complemento Gradle para Azure Web Apps](https://github.com/microsoft/azure-gradle-plugins/tree/master/azure-webapp-gradle-plugin) mediante su incorporación a `build.gradle`:
    ```groovy
    plugins {
      id "com.microsoft.azure.azurewebapp" version "1.2.0"
    }
    ```

1. Configure los detalles de la aplicación web; si no existe, se crean los recursos de Azure correspondientes.
Esta es una configuración de ejemplo. Para obtener detalles, vea este [documento](https://github.com/microsoft/azure-gradle-plugins/wiki/Webapp-Configuration).
    ```groovy
    azurewebapp {
        subscription = '<your subscription id>'
        resourceGroup = '<your resource group>'
        appName = '<your app name>'
        pricingTier = '<price tier like 'P1v2'>'
        region = '<region like 'westus'>'
        runtime {
          os = 'Linux'
          webContainer = 'Tomcat 9.0' // or 'Java SE' if you want to run an executable jar
          javaVersion = 'Java 8'
        }
        appSettings {
            <key> = <value>
        }
        auth {
            type = 'azure_cli' // support azure_cli, oauth2, device_code and service_principal
        }
    }
    ```

1. Implemente con un comando.
    ```shell
    gradle azureWebAppDeploy
    ```
    
### <a name="ides"></a>IDE
Azure proporciona una experiencia de desarrollo de Java App Service eficiente en los entornos de desarrollo de Java más populares, incluidos:
- *VS Code*: [Java Web Apps con Visual Studio Code](https://code.visualstudio.com/docs/java/java-webapp#_deploy-web-apps-to-the-cloud)
- *IntelliJ IDEA*: [Creación de una aplicación web Hola mundo para Azure App Service mediante IntelliJ](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app)
- *Eclipse*: [Creación de una aplicación web Hola mundo para Azure App Service mediante Eclipse](/azure/developer/java/toolkit-for-eclipse/create-hello-world-web-app)

### <a name="kudu-api"></a>API de Kudu
#### <a name="java-se"></a>Java SE

Para implementar archivos .jar en Java SE, use el punto de conexión `/api/publish/` del sitio de Kudu. Para más información sobre esta API, consulte [esta documentación](./deploy-zip.md#deploy-warjarear-packages). 

> [!NOTE]
>  La aplicación .jar debe tener el nombre `app.jar` para App Service y así poder identificar y ejecutar la aplicación. El complemento Maven (mencionado anteriormente) cambiará automáticamente el nombre de la aplicación durante la implementación. Si no quiere cambiar el nombre de archivo JAR a *app.jar*, puede cargar un script de shell con el comando para ejecutar la aplicación .jar. Pegue la ruta de acceso absoluta a este script en el cuadro de texto[Archivo de inicio](./faq-app-service-linux.yml) de la sección Configuración del portal. El script de inicio no se ejecuta desde el directorio en el que se encuentra. Por lo tanto, use siempre rutas de acceso absolutas para hacer referencia a los archivos del script de inicio (por ejemplo: `java -jar /home/myapp/myapp.jar`).

#### <a name="tomcat"></a>Tomcat

Para implementar archivos .war en Tomcat, utilice el punto de conexión `/api/wardeploy/` para realizar el conjunto de rutinas POST en el archivo. Para más información sobre esta API, consulte [esta documentación](./deploy-zip.md#deploy-warjarear-packages).

::: zone pivot="platform-linux"

#### <a name="jboss-eap"></a>JBoss EAP

Para implementar archivos .war en JBoss, use el punto de conexión `/api/wardeploy/` para realizar la instrucción POST en el archivo. Para más información sobre esta API, consulte [esta documentación](./deploy-zip.md#deploy-warjarear-packages).

Para implementar archivos .ear, [use FTP](deploy-ftp.md). La aplicación .ear se implementará en la raíz de contexto definida en la configuración de la aplicación. Por ejemplo, si la raíz de contexto de la aplicación es `<context-root>myapp</context-root>`, puede examinar el sitio en la ruta de acceso `/myapp`: `http://my-app-name.azurewebsites.net/myapp`. Si desea que se atienda a la aplicación web en la ruta de acceso raíz, asegúrese de que la aplicación establece la raíz del contexto en la ruta de acceso raíz: `<context-root>/</context-root>`. Para obtener más información, vea [Establecimiento del contexto raíz de una aplicación web](https://docs.jboss.org/jbossas/guides/webguide/r2/en/html/ch06.html).

::: zone-end

No implemente el archivo .war o .jar con el FTP. La herramienta FTP está diseñada para cargar los scripts de inicio, dependencias u otros archivos en tiempo de ejecución. Tenga en cuenta que no es la opción óptima para realizar la implementación de aplicaciones web.

## <a name="logging-and-debugging-apps"></a>Registro y depuración de aplicaciones

Encontrará informes de rendimiento, visualizaciones de tráfico y comprobaciones de mantenimiento de cada aplicación a través de Azure Portal. Para más información, consulte [Introducción a los diagnósticos de Azure App Service](overview-diagnostics.md).

### <a name="stream-diagnostic-logs"></a>Transmisión de registros de diagnóstico

::: zone pivot="platform-windows"

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

::: zone-end
::: zone pivot="platform-linux"

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-linux-no-h.md)]

::: zone-end

Para más información, consulte [Registros de secuencias en Cloud Shell](troubleshoot-diagnostic-logs.md#in-cloud-shell).

::: zone pivot="platform-linux"

### <a name="ssh-console-access"></a>Acceso a la consola SSH

[!INCLUDE [Open SSH session in browser](../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="troubleshooting-tools"></a>Herramienta para la solución de problemas

Las imágenes de Java integradas usan el sistema operativo [Alpine Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html). Use el administrador de paquetes `apk` para instalar todas las herramientas o comandos para la solución de problemas.

::: zone-end

### <a name="flight-recorder"></a>Caja negra SQL

Todos los entornos de ejecución de Java en App Service que usan JVM de Azul incluyen Zulu Flight Recorder. Puede usar esta aplicación para grabar eventos JVM, de aplicación y del sistema, así como para solucionar los problemas de las aplicaciones Java.

::: zone pivot="platform-windows"

#### <a name="timed-recording"></a>Grabación temporizada

Para realizar una grabación temporizada, necesitará el PID (id. de proceso) de la aplicación Java. Para buscar el PID, abra un explorador en el sitio de SCM de la aplicación web en `https://<your-site-name>.scm.azurewebsites.net/ProcessExplorer/`. En esta página se muestran los procesos en ejecución en la aplicación web. Busque el proceso denominado "Java" en la tabla y copie el PID (identificador de proceso) correspondiente.

A continuación, abra la **Consola de depuración** en la barra de herramientas superior del sitio de SCM y ejecute el siguiente comando. Reemplace `<pid>` por el identificador de proceso que ha copiado anteriormente. Este comando iniciará una grabación de 30 segundos del generador de perfiles de su aplicación Java y generará un archivo denominado `timed_recording_example.jfr` en el directorio `D:\home`.

```
jcmd <pid> JFR.start name=TimedRecording settings=profile duration=30s filename="D:\home\timed_recording_example.JFR"
```

::: zone-end
::: zone pivot="platform-linux"

Conéctese a App Service mediante SSH y ejecute el comando `jcmd` para ver una lista de todos los procesos de Java en ejecución. Además del propio jcmd, debería ver la aplicación de Java que se ejecuta con un número de Id. de proceso (pid).

```shell
078990bbcd11:/home# jcmd
Picked up JAVA_TOOL_OPTIONS: -Djava.net.preferIPv4Stack=true
147 sun.tools.jcmd.JCmd
116 /home/site/wwwroot/app.jar
```

Ejecute el siguiente comando para iniciar una grabación de 30 segundos de la máquina virtual Java. Así se generará el perfil de la máquina virtual Java y se creará un archivo JFR denominado *jfr_example.jfr* en el directorio principal. (reemplace 116 por el pid de la aplicación de Java).

```shell
jcmd 116 JFR.start name=MyRecording settings=profile duration=30s filename="/home/jfr_example.jfr"
```

Durante el intervalo de 30 segundos puede validar que la grabación se lleva a cabo mediante la ejecución de `jcmd 116 JFR.check`. Esto mostrará todas las grabaciones del proceso de Java dado.

#### <a name="continuous-recording"></a>Grabación continua

Puede usar Zulu Flight Recorder para generar un perfil continuamente de una aplicación Java con un impacto mínimo en el rendimiento del entorno de ejecución. Para ello, ejecute el siguiente comando de la CLI de Azure para crear una configuración de aplicación denominada JAVA_OPTS con la configuración necesaria. El contenido de la configuración de la aplicación de JAVA_OPTS se pasa al comando `java` cuando se inicia la aplicación.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

Cuando se haya iniciado la grabación, puede volcar los datos de grabación actuales en cualquier momento mediante el comando `JFR.dump`.

```shell
jcmd <pid> JFR.dump name=continuous_recording filename="/home/recording1.jfr"
```

::: zone-end

#### <a name="analyze-jfr-files"></a>Análisis de archivos `.jfr`

Use [FTPS](deploy-ftp.md) para descargar el archivo JFR en el equipo local. Para analizar el archivo JFR, descargue e instale [Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/). Para obtener instrucciones acerca de Zulu Mission Control, consulte la [documentación de Azul](https://docs.azul.com/zmc/) y [las instrucciones de instalación](/java/azure/jdk/java-jdk-flight-recorder-and-mission-control).

### <a name="app-logging"></a>Registro de aplicaciones

::: zone pivot="platform-windows"

Habilite el [registro de aplicaciones](troubleshoot-diagnostic-logs.md#enable-application-logging-windows) a través de Azure Portal o la [CLI de Azure](/cli/azure/webapp/log#az_webapp_log_config) para configurar App Service para que escriba los flujos de salida y de error de la consola estándar en el sistema de archivos local o en Azure Blob Storage. El registro en el sistema de archivos local de App Service se deshabilita 12 horas después de su configuración. Si necesita una retención más prolongada, configure la aplicación para escribir la salida en un contenedor de Blob Storage. Los registros de aplicación de Java y Tomcat pueden encontrarse en el directorio */home/LogFiles/Application/* .

::: zone-end
::: zone pivot="platform-linux"

Habilite el [registro de aplicaciones](troubleshoot-diagnostic-logs.md#enable-application-logging-linuxcontainer) a través de Azure Portal o la [CLI de Azure](/cli/azure/webapp/log#az_webapp_log_config) para configurar App Service para que escriba los flujos de salida y de error de la consola estándar en el sistema de archivos local o en Azure Blob Storage. Si necesita una retención más prolongada, configure la aplicación para escribir la salida en un contenedor de Blob Storage. Los registros de aplicación de Java y Tomcat pueden encontrarse en el directorio */home/LogFiles/Application/* .

El registro de Azure Blob Storage para instancias de App Services basadas en Linux solo se puede configurar mediante [Azure Monitor](./troubleshoot-diagnostic-logs.md#send-logs-to-azure-monitor) 

::: zone-end

Si la aplicación usa [Logback](https://logback.qos.ch/) o [Log4j](https://logging.apache.org/log4j) para el seguimiento, puede reenviar estos seguimientos para su revisión en Azure Application Insights mediante las instrucciones de configuración del marco de registro en [Exploración de los registros de seguimiento de Java en Application Insights](../azure-monitor/app/java-2x-trace-logs.md).

## <a name="customization-and-tuning"></a>Personalización y optimización

Azure App Service admite la optimización y la personalización de serie a través de Azure Portal y de la CLI de Azure. Consulte los artículos siguientes para conocer la configuración de aplicaciones web específicas que no son de Java:

- [Configuración de aplicaciones](configure-common.md#configure-app-settings)
- [Configuración de un dominio personalizado](app-service-web-tutorial-custom-domain.md)
- [Configuración de enlaces TLS/SSL](configure-ssl-bindings.md)
- [Adición de una red CDN](../cdn/cdn-add-to-web-app.md)
- [Configuración del sitio de Kudu](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Definición de las opciones de Java Runtime

Para establecer la memoria asignada u otras opciones de runtime de JVM, cree una [configuración de la aplicación](configure-common.md#configure-app-settings) denominada `JAVA_OPTS` con las opciones. App Service pasa esta configuración como variable de entorno para Java Runtime cuando se inicia.

En Azure Portal, en **Configuración de la aplicación** para la aplicación web, cree un valor de la aplicación denominado `JAVA_OPTS` para Java SE o `CATALINA_OPTS` para Tomcat que incluya otros valores de configuración, como `-Xms512m -Xmx1204m`.

Para definir la configuración de la aplicación desde el complemento MavenLinux, agregue las etiquetas setting/value a la sección de complementos de Azure. En el ejemplo siguiente se establece un tamaño del montón de Java mínimo y máximo específico:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

::: zone pivot="platform-windows"

> [!NOTE]
> No es necesario crear un archivo web.config al usar Tomcat en Windows App Service. 

::: zone-end

Los desarrolladores que ejecutan una sola aplicación con una ranura de implementación en su plan de App Service pueden usar las siguientes opciones:

- Instancias de S1 y B1: `-Xms1024m -Xmx1024m`
- Instancias de S2 y B2: `-Xms3072m -Xmx3072m`
- Instancias de S3 y B3: `-Xms6144m -Xmx6144m`
- Instancias de P1v2: `-Xms3072m -Xmx3072m`
- Instancias de P2v2: `-Xms6144m -Xmx6144m`
- Instancias de P3v2: `-Xms12800m -Xmx12800m`
- Instancias de P1v3: `-Xms6656m -Xmx6656m`
- Instancias de P2v3: `-Xms14848m -Xmx14848m`
- Instancias de P3v3: `-Xms30720m -Xmx30720m`
- Instancias de I1: `-Xms3072m -Xmx3072m`
- Instancias de I2: `-Xms6144m -Xmx6144m`
- Instancias de I3: `-Xms12800m -Xmx12800m`
- Instancias de I1v2: `-Xms6656m -Xmx6656m`
- Instancias de I2v2: `-Xms14848m -Xmx14848m`
- Instancias de I3v2: `-Xms30720m -Xmx30720m`

Cuando optimice la configuración del montón de la aplicación, revise los detalles de su plan de App Service y tenga en cuenta distintas necesidades de aplicaciones y ranuras de implementación para encontrar la asignación óptima de memoria.

### <a name="turn-on-web-sockets"></a>Activación de sockets web

Active la compatibilidad con sockets web en Azure Portal en **Configuración de la aplicación** para la aplicación. Deberá reiniciar la aplicación para que la configuración surta efecto.

Active la compatibilidad con los sockets web mediante la CLI de Azure con el comando siguiente:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

A continuación, reinicie su aplicación:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Definición de la codificación de caracteres predeterminada

En Azure Portal, en **Configuración de la aplicación** para la aplicación web, cree un nuevo valor de la aplicación denominado `JAVA_OPTS` con el valor `-Dfile.encoding=UTF-8`.

También puede configurar el valor de la aplicación mediante el complemento de Maven de App Service. Agregue las etiquetas setting y value del valor en la configuración del complemento:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="pre-compile-jsp-files"></a>Precompilación de archivos JSP

Para mejorar el rendimiento de las aplicaciones Tomcat, puede compilar los archivos JSP antes de realizar la implementación en App Service. Puede usar el [complemento Maven](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) proporcionado por Apache Sling o este [archivo de compilación Ant](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation).

## <a name="secure-applications"></a>Aplicaciones seguras

Las aplicaciones de Java que se ejecutan en App Service presentan el mismo conjunto de [procedimientos recomendados de seguridad](../security/fundamentals/paas-applications-using-app-services.md) que otras aplicaciones.

### <a name="authenticate-users-easy-auth"></a>Autenticación de usuarios (autenticación sencilla)

Configure la autenticación de la aplicación en Azure Portal con la opción **Autenticación y autorización**. Desde allí, puede habilitar la autenticación con Azure Active Directory o con inicios de sesión en redes sociales como Facebook, Google o GitHub. La configuración de Azure Portal solo funciona al configurar un proveedor de autenticación único. Para obtener más información, consulte [Configuración de una aplicación de App Service para usar el inicio de sesión de Azure Active Directory](configure-authentication-provider-aad.md) y los artículos relacionados de otros proveedores de identidades. Si tiene que habilitar varios proveedores de inicio de sesión, siga las instrucciones del artículo sobre la [personalización de inicios y cierres de sesión](configure-authentication-customize-sign-in-out.md).

#### <a name="java-se"></a>Java SE

Los desarrolladores de Spring Boot pueden usar el [iniciador de Spring Boot para Azure Active Directory](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory) para proteger las aplicaciones mediante las anotaciones y las API conocidas de Spring Security. Asegúrese de aumentar el tamaño máximo del encabezado en el archivo *application.properties*. Se recomienda un valor de `16384`.

#### <a name="tomcat"></a>Tomcat

La aplicación Tomcat puede acceder a las reclamaciones del usuario directamente desde el servlet mediante la conversión del objeto Principal a un objeto Map. Este último objeto asignará ca tipo de reclamación a una colección de las notificaciones de dicho tipo. En el código siguiente, `request` es una instancia de `HttpServletRequest`.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Ahora puede inspeccionar el objeto `Map` de cualquier notificación específica. Por ejemplo, el fragmento de código siguiente recorre en iteración todos los tipos de notificación e imprime el contenido de cada colección.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Para cerrar la sesión de los usuarios, utilice la ruta de acceso `/.auth/ext/logout`. Para realizar otras acciones, consulte la documentación sobre la [personalización de inicios y cierres de sesión](configure-authentication-customize-sign-in-out.md). También hay documentación oficial acerca de la [interfaz HttpServletRequest](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) y sus métodos. Los siguientes métodos de servlet también se hidrata métodos según la configuración de App Service:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Para deshabilitar esta característica, cree una configuración de aplicación denominada `WEBSITE_AUTH_SKIP_PRINCIPAL` con un valor de `1`. Para deshabilitar todos los filtros de servlet que ha agregado App Service, cree una configuración denominada `WEBSITE_SKIP_FILTERS` con un valor de `1`.

### <a name="configure-tlsssl"></a>Configuración de TLS/SSL

Siga las instrucciones de [Protección de un nombre DNS personalizado con un enlace TLS/SSL en Azure App Service](configure-ssl-bindings.md) para cargar un certificado TLS/SSL y enlazarlo al nombre de dominio de la aplicación. De forma predeterminada, la aplicación seguirá permitiendo que las conexiones HTTP sigan los pasos específicos del tutorial para aplicar TLS/SSL.

### <a name="use-keyvault-references"></a>Uso de referencias de KeyVault

[Azure KeyVault](../key-vault/general/overview.md) proporciona administración de secretos centralizada con directivas de acceso un historial de directivas. En KeyVault puede almacenar secretos (como contraseñas o cadenas de conexión) y acceder a ellos en la aplicación través de las variables de entorno.

En primer lugar, siga las instrucciones para [conceder a su aplicación acceso a Key Vault](app-service-key-vault-references.md#granting-your-app-access-to-key-vault) y [hace una referencia de KeyVault a su secreto en una configuración de aplicación](app-service-key-vault-references.md#reference-syntax). Para validar que la referencia se resuelve en el secreto, imprima la variable de entorno mie3ntras acceder de forma remota al terminal de App Service.

Para insertar estos secretos en el archivo de configuración de Spring o Tomcat, use la sintaxis de inserción de variables de entorno (`${MY_ENV_VAR}`). En el caso de los archivos de configuración de Spring, consulte esta documentación acerca de las [configuraciones externalizadas](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

::: zone pivot="platform-linux"

### <a name="use-the-java-key-store"></a>Uso del almacén de claves de Java

De forma predeterminada, los certificados públicos o privados [cargados en App Service Linux](configure-ssl-certificate.md) se cargarán en los almacenes de claves de Java respectivos cuando se inicie el contenedor. Después de cargar el certificado, deberá reiniciar la instancia de App Service para que se cargue en el almacén de claves de Java. Los certificados públicos se cargan en el almacén de claves en `$JAVA_HOME/jre/lib/security/cacerts`, y los certificados privados se almacenan en `$JAVA_HOME/lib/security/client.jks`.

Puede ser necesaria más configuración para el cifrado de la conexión de JDBC con certificados en el almacén de claves de Java. Consulte la documentación del controlador JDBC elegido.

- [PostgreSQL](https://jdbc.postgresql.org/documentation/head/ssl-client.html)
- [SQL Server](/sql/connect/jdbc/connecting-with-ssl-encryption)
- [MySQL](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-using-ssl.html)
- [MongoDB](https://mongodb.github.io/mongo-java-driver/3.4/driver/tutorials/ssl/)
- [Cassandra](https://docs.datastax.com/en/developer/java-driver/4.3/)

#### <a name="initialize-the-java-key-store"></a>Inicialización del almacén de claves de Java

Para inicializar el objeto `import java.security.KeyStore`, cargue el archivo de almacén de claves con la contraseña. La contraseña predeterminada para ambos almacenes de claves es `changeit`.

```java
KeyStore keyStore = KeyStore.getInstance("jks");
keyStore.load(
    new FileInputStream(System.getenv("JAVA_HOME")+"/lib/security/cacets"),
    "changeit".toCharArray());

KeyStore keyStore = KeyStore.getInstance("pkcs12");
keyStore.load(
    new FileInputStream(System.getenv("JAVA_HOME")+"/lib/security/client.jks"),
    "changeit".toCharArray());
```

#### <a name="manually-load-the-key-store"></a>Cargar manualmente el almacén de claves

Puede cargar los certificados manualmente en el almacén de claves. Cree una configuración de aplicación, `SKIP_JAVA_KEYSTORE_LOAD`, con un valor de `1` para deshabilitar en App Service la carga automática de los certificados en el almacén de claves. Todos los certificados públicos cargados en App Service a través de Azure Portal se almacenan en `/var/ssl/certs/`. Los certificados privados se almacenan en `/var/ssl/private/`.

Puede depurar o interactuar con la herramienta de claves de Java si [abre una conexión SSH](configure-linux-open-ssh-session.md) a la instancia de App Service y ejecuta el comando `keytool`. Consulte la [documentación de la herramienta de claves](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) para obtener una lista de comandos. Para más información sobre la API KeyStore, consulte [la documentación oficial](https://docs.oracle.com/javase/8/docs/api/java/security/KeyStore.html).

::: zone-end

## <a name="configure-apm-platforms"></a>Configuración de plataformas APM

En esta sección se muestra cómo conectar aplicaciones Java implementadas en Azure App Service con las plataformas de supervisión de aplicaciones (APM) Azure Monitor Application Insights, NewRelic y AppDynamics.

### <a name="configure-application-insights"></a>Configuración de Application Insights

Azure Monitor Application Insights es un servicio de supervisión de aplicaciones nativo de la nube que permite a los clientes observar errores, cuellos de botella y patrones de uso para mejorar el rendimiento de la aplicación y reducir el tiempo medio de resolución (MTTR). Con unos pocos clics o comandos de la CLI, puede habilitar la supervisión de las aplicaciones Node.js o Java y recopilar automáticamente registros, métricas y seguimientos distribuidos, lo que elimina la necesidad de incluir un SDK en la aplicación.

#### <a name="azure-portal"></a>Azure portal

Para habilitar Application Insights desde Azure Portal, vaya a **Application Insights** en el menú de la izquierda y seleccione **Activar Application Insights**. De forma predeterminada, se usará un nuevo recurso de Application Insights con el mismo nombre que la aplicación web. Puede optar por usar un recurso de Application Insights existente o cambiar el nombre. Haga clic en **Aplicar** en la parte inferior.

#### <a name="azure-cli"></a>Azure CLI

Para habilitarlo a través de la CLI de Azure, deberá crear un recurso de Application Insights y establecer un par de configuraciones de aplicación en Azure Portal para conectar Application Insights a la aplicación web.

1. Habilitación de la extensión de Applications Insights

    ```bash
    az extension add -n application-insights
    ```

2. Cree un recurso de Application Insights mediante el siguiente comando de la CLI. Reemplace los marcadores de posición por el nombre de recurso y el grupo deseados.

    ```bash
    az monitor app-insights component create --app <resource-name> -g <resource-group> --location westus2  --kind web --application-type web
    ```

    Anote los valores de `connectionString` y `instrumentationKey`, los necesitará en el siguiente paso.

    > Para recuperar una lista de otras ubicaciones, ejecute `az account list-locations`.

::: zone pivot="platform-windows"
    
3. Establezca la clave de instrumentación, la cadena de conexión y la versión del agente de supervisión como configuración de la aplicación en la aplicación web. Sustituya `<instrumentationKey>` y `<connectionString>` por los valores del paso anterior.

    ```bash
    az webapp config appsettings set -n <webapp-name> -g <resource-group> --settings "APPINSIGHTS_INSTRUMENTATIONKEY=<instrumentationKey>" "APPLICATIONINSIGHTS_CONNECTION_STRING=<connectionString>" "ApplicationInsightsAgent_EXTENSION_VERSION=~3" "XDT_MicrosoftApplicationInsights_Mode=default" "XDT_MicrosoftApplicationInsights_Java=1"
    ```

::: zone-end
::: zone pivot="platform-linux"
    
3. Establezca la clave de instrumentación, la cadena de conexión y la versión del agente de supervisión como configuración de la aplicación en la aplicación web. Sustituya `<instrumentationKey>` y `<connectionString>` por los valores del paso anterior.

    ```bash
    az webapp config appsettings set -n <webapp-name> -g <resource-group> --settings "APPINSIGHTS_INSTRUMENTATIONKEY=<instrumentationKey>" "APPLICATIONINSIGHTS_CONNECTION_STRING=<connectionString>" "ApplicationInsightsAgent_EXTENSION_VERSION=~3" "XDT_MicrosoftApplicationInsights_Mode=default"
    ```

::: zone-end

### <a name="configure-new-relic"></a>Configuración de New Relic

::: zone pivot="platform-windows"

1. Cree una cuenta de NewRelic en [NewRelic.com](https://newrelic.com/signup).
2. Descargue el agente de Java desde NewRelic; tendrá un nombre de archivo similar a *newrelic-java-x.x.x.zip*.
3. Copie la clave de licencia, ya que la necesitará más tarde para configurar el agente.
4. [Conéctese mediante SSH a su instancia de App Service](configure-linux-open-ssh-session.md) y cree un nuevo directorio */home/site/wwwroot/apm*.
5. Cargue los archivos desempaquetados del agente de Java de NewRelic en un directorio de */home/site/wwwroot/apm*. Los archivos del agente deben estar en */home/site/wwwroot/apm/newrelic*.
6. Modifique el archivo YAML en */home/site/wwwroot/apm/newrelic/newrelic.yml* y reemplace el marcador de posición de valor de la licencia por su propia clave de licencia.
7. En Azure Portal, vaya a la aplicación en App Service y cree una nueva configuración de la aplicación.

    - Para las aplicaciones de **Java SE**, cree una variable de entorno llamada `JAVA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Para las aplicaciones de **Tomcat**, cree una variable de entorno llamada `CATALINA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.

::: zone-end
::: zone pivot="platform-linux"

1. Cree una cuenta de NewRelic en [NewRelic.com](https://newrelic.com/signup).
2. Descargue el agente de Java desde NewRelic; tendrá un nombre de archivo similar a *newrelic-java-x.x.x.zip*.
3. Copie la clave de licencia, ya que la necesitará más tarde para configurar el agente.
4. [Conéctese mediante SSH a su instancia de App Service](configure-linux-open-ssh-session.md) y cree un nuevo directorio */home/site/wwwroot/apm*.
5. Cargue los archivos desempaquetados del agente de Java de NewRelic en un directorio de */home/site/wwwroot/apm*. Los archivos del agente deben estar en */home/site/wwwroot/apm/newrelic*.
6. Modifique el archivo YAML en */home/site/wwwroot/apm/newrelic/newrelic.yml* y reemplace el marcador de posición de valor de la licencia por su propia clave de licencia.
7. En Azure Portal, vaya a la aplicación en App Service y cree una nueva configuración de la aplicación.
   
    - Para las aplicaciones de **Java SE**, cree una variable de entorno llamada `JAVA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.
    - Para las aplicaciones de **Tomcat**, cree una variable de entorno llamada `CATALINA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar`.

::: zone-end

>  Si ya tiene una variable de entorno para `JAVA_OPTS` o `CATALINA_OPTS`, anexe la opción `-javaagent:/...` al final del valor actual.

### <a name="configure-appdynamics"></a>Configuración de AppDynamics

::: zone pivot="platform-windows"

1. Cree una cuenta de AppDynamics en [AppDynamics.com](https://www.appdynamics.com/community/register/).
2. Descargue el agente de Java desde el sitio web de AppDynamics; el nombre de archivo será similar a *AppServerAgent-x.x.x.xxxxx.zip*
3. Use la [consola de Kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console) para crear un directorio */home/site/wwwroot/apm*.
4. Cargue los archivos del agente de Java en un directorio de */home/site/wwwroot/apm*. Los archivos del agente deben estar en */home/site/wwwroot/apm/appdynamics*.
5. En Azure Portal, vaya a la aplicación en App Service y cree una nueva configuración de la aplicación.

   - Para las aplicaciones de **Java SE**, cree una variable de entorno llamada `JAVA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, donde `<app-name>` es el nombre de su instancia de App Service.
   - Para las aplicaciones de **Tomcat**, cree una variable de entorno llamada `CATALINA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, donde `<app-name>` es el nombre de su instancia de App Service.

::: zone-end
::: zone pivot="platform-linux"

1. Cree una cuenta de AppDynamics en [AppDynamics.com](https://www.appdynamics.com/community/register/).
2. Descargue el agente de Java desde el sitio web de AppDynamics; el nombre de archivo será similar a *AppServerAgent-x.x.x.xxxxx.zip*
3. [Conéctese mediante SSH a su instancia de App Service](configure-linux-open-ssh-session.md) y cree un nuevo directorio */home/site/wwwroot/apm*.
4. Cargue los archivos del agente de Java en un directorio de */home/site/wwwroot/apm*. Los archivos del agente deben estar en */home/site/wwwroot/apm/appdynamics*.
5. En Azure Portal, vaya a la aplicación en App Service y cree una nueva configuración de la aplicación.

   - Para las aplicaciones de **Java SE**, cree una variable de entorno llamada `JAVA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, donde `<app-name>` es el nombre de su instancia de App Service.
   - Para las aplicaciones de **Tomcat**, cree una variable de entorno llamada `CATALINA_OPTS` con el valor `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, donde `<app-name>` es el nombre de su instancia de App Service.

::: zone-end

> [!NOTE]
>  Si ya tiene una variable de entorno para `JAVA_OPTS` o `CATALINA_OPTS`, anexe la opción `-javaagent:/...` al final del valor actual.

## <a name="configure-data-sources"></a>Configuración de orígenes de datos

### <a name="java-se"></a>Java SE

Para conectarse a orígenes de datos en aplicaciones de Spring Boot, se recomienda crear cadenas de conexión e insertarlas en su archivo *application.properties*.

1. En la sección "Configuración" de la página de App Service, establezca un nombre para la cadena, pegue la cadena de conexión de JDBC en el campo de valor y establezca el tipo en "Custom" (Personalizado). Opcionalmente, puede establecer esta cadena de conexión como configuración de ranura.

    Nuestra aplicación puede acceder a cadena de conexión como una variable de entorno denominada `CUSTOMCONNSTR_<your-string-name>`. Por ejemplo, la cadena de conexión que hemos creado anteriormente se denominará `CUSTOMCONNSTR_exampledb`.

2. En su archivo *application.properties*, haga referencia a esta cadena de conexión con el nombre de variable de entorno. En nuestro ejemplo, se usaría lo siguiente.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Consulte la [documentación de Spring Boot relativa al acceso a datos](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) y [configuraciones externalizadas ](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) para más información acerca de este tema.

::: zone pivot="platform-windows"

### <a name="tomcat"></a>Tomcat

Estas instrucciones se aplican a todas las conexiones de base de datos. Deberá rellenar los marcadores de posición con el nombre de clase de controlador de la base de datos elegido y con el archivo JAR. Se proporciona una tabla con los nombres de clase y las descargas de controladores de las bases de datos más habituales.

| Base de datos   | Nombre de clase de controlador                             | Controlador JDBC                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [Descargar](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [Descargar](https://dev.mysql.com/downloads/connector/j/) (seleccione "Platform Independent") |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [Descargar](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server#download)                                                           |

Para configurar Tomcat para que use Java Database Connectivity (JDBC) o Java Persistence API (JPA), personalice primero la variable de entorno `CATALINA_OPTS` que lee Tomcat al iniciarse. Establezca estos valores a través de un valor de la aplicación en el [complemento de Maven de App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md):

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

O bien establezca las variables de entorno en la página **Configuración** > **Configuración de la aplicación** de Azure Portal.

A continuación, determine si el origen de datos debe estar disponible para una aplicación o para todas las aplicaciones que se ejecutan en el servlet de Tomcat.

#### <a name="application-level-data-sources"></a>Orígenes de datos de nivel de aplicación

1. Cree un archivo *context.xml* en el directorio *META-INF /* del proyecto. Si el directorio *META-INF/* no existe, créelo.

2. En *, context.xml*, agregue un elemento `Context` para vincular el origen de datos a una dirección JNDI. Reemplace el marcador de posición `driverClassName` por el nombre de clase de su controlador de la tabla anterior.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Actualice el archivo *web.xml* de la aplicación para que use el origen de datos de su aplicación.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Recursos de nivel de servidor compartidos

Las instalaciones de Tomcat en App Service en Windows existen en el espacio compartido del plan de App Service. No se puede modificar directamente una instalación de Tomcat para la configuración de todo el servidor. Para realizar modificaciones en la configuración de nivel de servidor en la instalación de Tomcat, debe copiar Tomcat en una carpeta local, en la que podrá modificar dicha configuración. 

##### <a name="automate-creating-custom-tomcat-on-app-start"></a>Automatización de la creación de una instancia personalizada de Tomcat al iniciar la aplicación

Puede usar un script de inicio para realizar acciones antes de que se inicie una aplicación web. El script de inicio para personalizar Tomcat debe completar los siguientes pasos:

1. Compruebe si Tomcat ya se ha copiado y configurado localmente. De ser así, el script de inicio puede terminar aquí.
2. Copie Tomcat localmente.
3. Realice los cambios de configuración necesarios.
4. Indique que la configuración se completó correctamente.

Para sitios de Windows, cree un archivo denominado `startup.cmd` o `startup.ps1` en el directorio `wwwroot`. Se ejecutará automáticamente antes de que se inicie el servidor de Tomcat.

Este es un script de PowerShell que completa estos pasos:

```powershell
    # Check for marker file indicating that config has already been done
    if(Test-Path "$Env:LOCAL_EXPANDED\tomcat\config_done_marker"){
        return 0
    }

    # Delete previous Tomcat directory if it exists
    # In case previous config could not be completed or a new config should be forcefully installed
    if(Test-Path "$Env:LOCAL_EXPANDED\tomcat"){
        Remove-Item "$Env:LOCAL_EXPANDED\tomcat" --recurse
    }

    # Copy Tomcat to local
    # Using the environment variable $AZURE_TOMCAT90_HOME uses the 'default' version of Tomcat
    Copy-Item -Path "$Env:AZURE_TOMCAT90_HOME\*" -Destination "$Env:LOCAL_EXPANDED\tomcat" -Recurse

    # Perform the required customization of Tomcat
    {... customization ...}

    # Mark that the operation was a success
    New-Item -Path "$Env:LOCAL_EXPANDED\tomcat\config_done_marker" -ItemType File
```

##### <a name="transforms"></a>Transformaciones

Un caso de uso común para personalizar una versión de Tomcat consiste en modificar los archivos de configuración de Tomcat `server.xml`, `context.xml` o `web.xml`. App Service ya modifica estos archivos para proporcionar características de la plataforma. Para seguir usando estas características, es importante conservar el contenido de estos archivos al realizar cambios en ellos. Para ello, se recomienda usar una [transformación XSL (XSLT)](https://www.w3schools.com/xml/xsl_intro.asp). Use una transformación XSL para realizar cambios en los archivos XML a la vez que se conserva el contenido original del archivo.

###### <a name="example-xslt-file"></a>Archivo XSLT de ejemplo

Esta transformación de ejemplo agrega un nuevo nodo de conector a `server.xml`. Observe la *Transformación de identidad*, que conserva el contenido original del archivo.

```xml
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method="xml" indent="yes"/>
  
    <!-- Identity transform: this ensures that the original contents of the file are included in the new file -->
    <!-- Ensure that your transform files include this block -->
    <xsl:template match="@* | node()" name="Copy">
      <xsl:copy>
        <xsl:apply-templates select="@* | node()"/>
      </xsl:copy>
    </xsl:template>
  
    <xsl:template match="@* | node()" mode="insertConnector">
      <xsl:call-template name="Copy" />
    </xsl:template>
  
    <xsl:template match="comment()[not(../Connector[@scheme = 'https']) and
                                   contains(., '&lt;Connector') and
                                   (contains(., 'scheme=&quot;https&quot;') or
                                    contains(., &quot;scheme='https'&quot;))]">
      <xsl:value-of select="." disable-output-escaping="yes" />
    </xsl:template>
  
    <xsl:template match="Service[not(Connector[@scheme = 'https'] or
                                     comment()[contains(., '&lt;Connector') and
                                               (contains(., 'scheme=&quot;https&quot;') or
                                                contains(., &quot;scheme='https'&quot;))]
                                    )]
                        ">
      <xsl:copy>
        <xsl:apply-templates select="@* | node()" mode="insertConnector" />
      </xsl:copy>
    </xsl:template>
  
    <!-- Add the new connector after the last existing Connnector if there is one -->
    <xsl:template match="Connector[last()]" mode="insertConnector">
      <xsl:call-template name="Copy" />
  
      <xsl:call-template name="AddConnector" />
    </xsl:template>
  
    <!-- ... or before the first Engine if there is no existing Connector -->
    <xsl:template match="Engine[1][not(preceding-sibling::Connector)]"
                  mode="insertConnector">
      <xsl:call-template name="AddConnector" />
  
      <xsl:call-template name="Copy" />
    </xsl:template>
  
    <xsl:template name="AddConnector">
      <!-- Add new line -->
      <xsl:text>&#xa;</xsl:text>
      <!-- This is the new connector -->
      <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true" 
                 maxThreads="150" scheme="https" secure="true" 
                 keystroreFile="${{user.home}}/.keystore" keystorePass="changeit"
                 clientAuth="false" sslProtocol="TLS" />
    </xsl:template>

    </xsl:stylesheet>
```

###### <a name="function-for-xsl-transform"></a>Función para la transformación XSL

PowerShell tiene herramientas integradas para transformar archivos XML mediante transformaciones XSL. El siguiente script es una función de ejemplo que puede usar en `startup.ps1` para realizar la transformación:

```powershell
    function TransformXML{
        param ($xml, $xsl, $output)

        if (-not $xml -or -not $xsl -or -not $output)
        {
            return 0
        }

        Try
        {
            $xslt_settings = New-Object System.Xml.Xsl.XsltSettings;
            $XmlUrlResolver = New-Object System.Xml.XmlUrlResolver;
            $xslt_settings.EnableScript = 1;

            $xslt = New-Object System.Xml.Xsl.XslCompiledTransform;
            $xslt.Load($xsl,$xslt_settings,$XmlUrlResolver);
            $xslt.Transform($xml, $output);

        }

        Catch
        {
            $ErrorMessage = $_.Exception.Message
            $FailedItem = $_.Exception.ItemName
            Write-Host  'Error'$ErrorMessage':'$FailedItem':' $_.Exception;
            return 0
        }
        return 1
    }
```

##### <a name="app-settings"></a>Configuración de la aplicación

La plataforma también debe saber dónde está instalada la versión personalizada de Tomcat. Puede establecer la ubicación de la instalación en el valor de configuración `CATALINA_BASE` de la aplicación.

Puede usar la CLI de Azure para cambiar este valor:

```powershell
    az webapp config appsettings set -g $MyResourceGroup -n $MyUniqueApp --settings CATALINA_BASE="%LOCAL_EXPANDED%\tomcat"
```

O bien, puede cambiar manualmente la configuración en Azure Portal:

1. Vaya a **Configuración** > **Configuración** > **Configuración de la aplicación**.
1. Seleccione **Nueva configuración de la aplicación**.
1. Use estos valores para crear la configuración:
   1. **Nombre**: `CATALINA_BASE`
   1. **Valor**: `"%LOCAL_EXPANDED%\tomcat"`

##### <a name="example-startupps1"></a>Ejemplo de startup.ps1

El siguiente script de ejemplo copia una instancia de Tomcat personalizada en una carpeta local, realiza una transformación XSL e indica que la transformación se ha realizado correctamente:

```powershell
    # Locations of xml and xsl files
    $target_xml="$Env:LOCAL_EXPANDED\tomcat\conf\server.xml"
    $target_xsl="$Env:HOME\site\server.xsl"
    
    # Define the transform function
    # Useful if transforming multiple files
    function TransformXML{
        param ($xml, $xsl, $output)
    
        if (-not $xml -or -not $xsl -or -not $output)
        {
            return 0
        }
    
        Try
        {
            $xslt_settings = New-Object System.Xml.Xsl.XsltSettings;
            $XmlUrlResolver = New-Object System.Xml.XmlUrlResolver;
            $xslt_settings.EnableScript = 1;
    
            $xslt = New-Object System.Xml.Xsl.XslCompiledTransform;
            $xslt.Load($xsl,$xslt_settings,$XmlUrlResolver);
            $xslt.Transform($xml, $output);
        }
    
        Catch
        {
            $ErrorMessage = $_.Exception.Message
            $FailedItem = $_.Exception.ItemName
            echo  'Error'$ErrorMessage':'$FailedItem':' $_.Exception;
            return 0
        }
        return 1
    }
    
    $success = TransformXML -xml $target_xml -xsl $target_xsl -output $target_xml
    
    # Check for marker file indicating that config has already been done
    if(Test-Path "$Env:LOCAL_EXPANDED\tomcat\config_done_marker"){
        return 0
    }
    
    # Delete previous Tomcat directory if it exists
    # In case previous config could not be completed or a new config should be forcefully installed
    if(Test-Path "$Env:LOCAL_EXPANDED\tomcat"){
        Remove-Item "$Env:LOCAL_EXPANDED\tomcat" --recurse
    }
    
    md -Path "$Env:LOCAL_EXPANDED\tomcat"
    
    # Copy Tomcat to local
    # Using the environment variable $AZURE_TOMCAT90_HOME uses the 'default' version of Tomcat
    Copy-Item -Path "$Env:AZURE_TOMCAT90_HOME\*" "$Env:LOCAL_EXPANDED\tomcat" -Recurse
    
    # Perform the required customization of Tomcat
    $success = TransformXML -xml $target_xml -xsl $target_xsl -output $target_xml
    
    # Mark that the operation was a success if successful
    if($success){
        New-Item -Path "$Env:LOCAL_EXPANDED\tomcat\config_done_marker" -ItemType File
    }
```

#### <a name="finalize-configuration"></a>Finalización de la configuración

Por último, coloque los archivos JAR del controlador en la classpath de Tomcat y reinicie App Service. Asegúrese de que los archivos del controlador JDBC estén disponibles para el cargador de clases de Tomcat. Para ello, colóquelos en el directorio */home/tomcat/lib*. (cree el directorio si no existe). Para cargar estos archivos en su instancia de App Service, realice los pasos siguientes:

1. En el [Cloud Shell](https://shell.azure.com), instale la extensión webapp:

    ```azurecli-interactive
    az extension add -–name webapp
    ```

2. Ejecute el siguiente comando de la CLI para crear un túnel SSH desde el sistema local a App Service:

    ```azurecli-interactive
    az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
    ```

3. Establezca la conexión al puerto de tunelización local con el cliente SFTP y cargue los archivos en la carpeta */home/tomcat/lib*.

Como alternativa, puede usar un cliente FTP para cargar el controlador JDBC. Siga estas [instrucciones para obtener las credenciales de FTP](deploy-configure-credentials.md).

---

::: zone-end
::: zone pivot="platform-linux"

### <a name="tomcat"></a>Tomcat

Estas instrucciones se aplican a todas las conexiones de base de datos. Deberá rellenar los marcadores de posición con el nombre de clase de controlador de la base de datos elegido y con el archivo JAR. Se proporciona una tabla con los nombres de clase y las descargas de controladores de las bases de datos más habituales.

| Base de datos   | Nombre de clase de controlador                             | Controlador JDBC                                                                              |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [Descargar](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [Descargar](https://dev.mysql.com/downloads/connector/j/) (seleccione "Platform Independent") |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [Descargar](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server#download)     |

Para configurar Tomcat para que use Java Database Connectivity (JDBC) o Java Persistence API (JPA), personalice primero la variable de entorno `CATALINA_OPTS` que lee Tomcat al iniciarse. Establezca estos valores a través de un valor de la aplicación en el [complemento de Maven de App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md):

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

O bien establezca las variables de entorno en la página **Configuración** > **Configuración de la aplicación** de Azure Portal.

A continuación, determine si el origen de datos debe estar disponible para una aplicación o para todas las aplicaciones que se ejecutan en el servlet de Tomcat.

#### <a name="application-level-data-sources"></a>Orígenes de datos de nivel de aplicación

1. Cree un archivo *context.xml* en el directorio *META-INF /* del proyecto. Si el directorio *META-INF/* no existe, créelo.

2. En *, context.xml*, agregue un elemento `Context` para vincular el origen de datos a una dirección JNDI. Reemplace el marcador de posición `driverClassName` por el nombre de clase de su controlador de la tabla anterior.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Actualice el archivo *web.xml* de la aplicación para que use el origen de datos de su aplicación.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Recursos de nivel de servidor compartidos

Para agregar un origen de datos compartido de nivel de servidor, será necesario editar el archivo server.xml de Tomcat. En primer lugar, cargue un [script de inicio](./faq-app-service-linux.yml) y establezca la ruta de acceso al script en **Configuración** > **Comando de inicio**. Puede cargar el script de inicio mediante [FTP](deploy-ftp.md).

El script de inicio realizará una [transformación XSL](https://www.w3schools.com/xml/xsl_intro.asp) al archivo server.xml y generará el archivo XML resultante en `/usr/local/tomcat/conf/server.xml`. El script de inicio debe instalar libxslt a través de APK. El archivo XSL y el script de inicio se pueden cargar a través de FTP. A continuación se muestra un script de inicio de ejemplo.

```sh
# Install libxslt. Also copy the transform file to /home/tomcat/conf/
apk add --update libxslt

# Usage: xsltproc --output output.xml style.xsl input.xml
xsltproc --output /home/tomcat/conf/server.xml /home/tomcat/conf/transform.xsl /usr/local/tomcat/conf/server.xml
```

A continuación se puede ver un archivo XSL de ejemplo. Este archivo agrega un nuevo nodo de conector al archivo server.xml de Tomcat.

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:output method="xml" indent="yes"/>

  <xsl:template match="@* | node()" name="Copy">
    <xsl:copy>
      <xsl:apply-templates select="@* | node()"/>
    </xsl:copy>
  </xsl:template>

  <xsl:template match="@* | node()" mode="insertConnector">
    <xsl:call-template name="Copy" />
  </xsl:template>

  <xsl:template match="comment()[not(../Connector[@scheme = 'https']) and
                                 contains(., '&lt;Connector') and
                                 (contains(., 'scheme=&quot;https&quot;') or
                                  contains(., &quot;scheme='https'&quot;))]">
    <xsl:value-of select="." disable-output-escaping="yes" />
  </xsl:template>

  <xsl:template match="Service[not(Connector[@scheme = 'https'] or
                                   comment()[contains(., '&lt;Connector') and
                                             (contains(., 'scheme=&quot;https&quot;') or
                                              contains(., &quot;scheme='https'&quot;))]
                                  )]
                      ">
    <xsl:copy>
      <xsl:apply-templates select="@* | node()" mode="insertConnector" />
    </xsl:copy>
  </xsl:template>

  <!-- Add the new connector after the last existing Connnector if there is one -->
  <xsl:template match="Connector[last()]" mode="insertConnector">
    <xsl:call-template name="Copy" />

    <xsl:call-template name="AddConnector" />
  </xsl:template>

  <!-- ... or before the first Engine if there is no existing Connector -->
  <xsl:template match="Engine[1][not(preceding-sibling::Connector)]"
                mode="insertConnector">
    <xsl:call-template name="AddConnector" />

    <xsl:call-template name="Copy" />
  </xsl:template>

  <xsl:template name="AddConnector">
    <!-- Add new line -->
    <xsl:text>&#xa;</xsl:text>
    <!-- This is the new connector -->
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true" 
               maxThreads="150" scheme="https" secure="true" 
               keystroreFile="${{user.home}}/.keystore" keystorePass="changeit"
               clientAuth="false" sslProtocol="TLS" />
  </xsl:template>
  
</xsl:stylesheet>
```

#### <a name="finalize-configuration"></a>Finalización de la configuración

Por último, coloque los archivos JAR del controlador en la classpath de Tomcat y reinicie App Service.

1. Asegúrese de que los archivos del controlador JDBC estén disponibles para el cargador de clases de Tomcat. Para ello, colóquelos en el directorio */home/tomcat/lib*. (cree el directorio si no existe). Para cargar estos archivos en su instancia de App Service, realice los pasos siguientes:

    1. En el [Cloud Shell](https://shell.azure.com), instale la extensión webapp:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. Ejecute el siguiente comando de la CLI para crear un túnel SSH desde el sistema local a App Service:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. Establezca la conexión al puerto de tunelización local con el cliente SFTP y cargue los archivos en la carpeta */home/tomcat/lib*.

    Como alternativa, puede usar un cliente FTP para cargar el controlador JDBC. Siga estas [instrucciones para obtener las credenciales de FTP](deploy-configure-credentials.md).

2. Si ha creado un origen de datos de nivel de servidor, reinicie la aplicación App Service de Linux. Tomcat restablecerá `CATALINA_BASE` a `/home/tomcat` y usará la configuración actualizadas.

### <a name="jboss-eap"></a>JBoss EAP

Al [registrar un origen de datos en JBoss EAP](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.0/html/configuration_guide/datasource_management), hay tres pasos fundamentales: cargar el controlador JDBC, agregarlo como un módulo y registrar el módulo. App Service es un servicio de hospedaje sin estado, por lo que los comandos de configuración para agregar y registrar el módulo de origen de datos deben incluirse en un script y aplicarse cuando se inicie el contenedor.

1. Obtenga el controlador JDBC de la base de datos. 
2. Cree un archivo de definición de módulo XML para el controlador JDBC. El ejemplo que se muestra a continuación es una definición de módulo para PostgreSQL.

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="org.postgres">
        <resources>
        <!-- ***** IMPORTANT : REPLACE THIS PLACEHOLDER *******-->
        <resource-root path="/home/site/deployments/tools/postgresql-42.2.12.jar" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

1. Coloque los comandos de la CLI de JBoss en un archivo denominado `jboss-cli-commands.cli`. Los comandos de JBoss deben agregar el módulo y registrarlo como un origen de datos. En el ejemplo siguiente se muestran los comandos de la CLI de JBoss para PostgreSQL.

    ```bash
    #!/usr/bin/env bash
    module add --name=org.postgres --resources=/home/site/deployments/tools/postgresql-42.2.12.jar --module-xml=/home/site/deployments/tools/postgres-module.xml

    /subsystem=datasources/jdbc-driver=postgres:add(driver-name="postgres",driver-module-name="org.postgres",driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)

    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=${POSTGRES_CONNECTION_URL,env.POSTGRES_CONNECTION_URL:jdbc:postgresql://db:5432/postgres} --user-name=${POSTGRES_SERVER_ADMIN_FULL_NAME,env.POSTGRES_SERVER_ADMIN_FULL_NAME:postgres} --password=${POSTGRES_SERVER_ADMIN_PASSWORD,env.POSTGRES_SERVER_ADMIN_PASSWORD:example} --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker
    ```

1. Cree un script de inicio, `startup_script.sh`, que llame a los comandos de la CLI de JBoss. En el ejemplo siguiente se muestra cómo llamar a `jboss-cli-commands.cli`. Más adelante, configurará App Service para ejecutar este script cuando se inicie el contenedor. 

    ```bash
    $JBOSS_HOME/bin/jboss-cli.sh --connect --file=/home/site/deployments/tools/jboss-cli-commands.cli
    ```

1. Mediante el cliente FTP de su elección, cargue el controlador JDBC, `jboss-cli-commands.cli`, `startup_script.sh` y la definición del módulo en `/site/deployments/tools/`.
2. Configure el sitio para que ejecute `startup_script.sh` cuando se inicie el contenedor. En Azure Portal, vaya a **Configuración** > **Configuración general** > **Comando de inicio**. Establezca el campo de comando de inicio en `/home/site/deployments/tools/startup_script.sh`. Guarde los cambios mediante **Guardar**.

Para confirmar que el origen de datos se agregó al servidor JBoss, conéctese a su aplicación web mediante SSH y ejecute `$JBOSS_HOME/bin/jboss-cli.sh --connect`. Cuando esté conectado a JBoss, ejecute `/subsystem=datasources:read-resource` para imprimir una lista de los orígenes de datos.

::: zone-end

[!INCLUDE [robots933456](../../includes/app-service-web-configure-robots933456.md)]

## <a name="choosing-a-java-runtime-version"></a>Selección de la versión del entorno de ejecución de Java

App Service permite a los usuarios elegir la versión principal de JVM (por ejemplo, Java 8 o Java 11) y la versión secundaria (por ejemplo, 1.8.0_232 o 11.0.5). También puede elegir que la versión secundaria se actualice automáticamente a medida que estén disponibles otras nuevas. En la mayoría de los casos, los sitios de producción deben usar versiones secundarias de JVM ancladas. De esta forma, se evitan interrupciones imprevistas durante la actualización automática de una versión secundaria. Todas las aplicaciones web de Java usan JMS de 64 bits, lo que no es configurable.

Si elige anclar la versión secundaria, tendrá que actualizar periódicamente la versión secundaria de JVM en el sitio. Para asegurarse de que la aplicación se ejecuta en la versión secundaria más reciente, cree un espacio de ensayo e incremente en él la versión secundaria. Cuando haya confirmado que la aplicación se ejecuta correctamente en la nueva versión secundaria, puede intercambiar los espacios de ensayo y de producción.

::: zone pivot="platform-linux"

## <a name="jboss-eap-app-service-plans"></a>Planes de App Service de JBoss EAP
<a id="jboss-eap-hardware-options"></a>

JBoss EAP solo está disponible en los tipos de plan Premium v3 y Aislado v2 de App Service. Los clientes que crearon un sitio de JBoss EAP en un nivel diferente durante la versión preliminar pública deben escalar verticalmente a un nivel de hardware Premium o Aislado para evitar un comportamiento inesperado.

::: zone-end

## <a name="java-runtime-statement-of-support"></a>Instrucción de compatibilidad de Java Runtime

### <a name="jdk-versions-and-maintenance"></a>Mantenimiento y versiones de JDK

El JDK (Java Development Kit) compatible con Azure es [Zulu](https://www.azul.com/downloads/azure-only/zulu/) y se distribuye a través de [Azul Systems](https://www.azul.com/). Las compilaciones del OpenJDK de Azul Zulu Enterprise son una distribución gratuita, multiplataforma y lista para producción del OpenJDK para Azure y Azure Stack respaldado por Microsoft y Azul Systems. Contienen todos los componentes para la creación y ejecución de aplicaciones de Java SE. El JDK se puede instalar desde [Instalación del JDK de Java](/azure/developer/java/fundamentals/java-support-on-azure).

Las actualizaciones de versión principal se proporcionarán por medio de nuevas opciones de entorno de ejecución en Azure App Service. Para actualizar a estas versiones más recientes, los clientes deben configurar su implementación de App Service. Asimismo, son responsables de probar y garantizar que la actualización principal satisface sus necesidades.

Los JDK compatibles se revisarán trimestralmente de manera automática en enero, abril, julio y octubre de cada año. Para más información sobre Java en Azure, consulte [este documento de soporte técnico](/azure/developer/java/fundamentals/java-support-on-azure).

### <a name="security-updates"></a>Actualizaciones de seguridad

Las revisiones y correcciones de las vulnerabilidades de seguridad principales se publicarán en cuanto estén disponibles a través de Azul Systems. Una vulnerabilidad "principal" se define con una puntuación total de 9.0 o superior en el [CVSS (Common Vulnerability Scoring System), versión 2 del NIST](https://nvd.nist.gov/vuln-metrics/cvss).

Tomcat 8.0 alcanzó el [fin de la vida útil el 30 de septiembre de 2018](https://tomcat.apache.org/tomcat-80-eol.html). Aunque el entorno en tiempo de ejecución sigue disponible en Azure App Service, Azure no aplicará actualizaciones de seguridad a Tomcat 8.0. Si es posible, migre las aplicaciones a Tomcat 8.5 o 9.0. Tanto Tomcat 8.5 como 9.0 están disponibles en Azure App Service. Para más información, consulte el [sitio oficial de Tomcat](https://tomcat.apache.org/whichversion.html). 

### <a name="deprecation-and-retirement"></a>Desuso y retirada

Si un entorno compatible de Java Runtime se va a retirar, los desarrolladores de Azure que lo usen recibirán un aviso de desuso al menos seis meses antes de su retirada.


### <a name="local-development"></a>Desarrollo local

Los desarrolladores pueden descargar el JDK de Azul Zulu Enterprise Production Edition para el desarrollo local desde el [sitio de descarga de Azul](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Soporte para el desarrollo

El soporte técnico para el JDK de [Azul Zulu compatible con Azure](https://www.azul.com/downloads/azure-only/zulu/) está disponible mediante Microsoft al desarrollar para Azure o [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) con un [plan de soporte técnico de Azure cualificado](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Pasos siguientes

Visite el centro de [Azure para desarrolladores de Java](/java/azure/) para encontrar guías de inicio rápido de Azure, tutoriales y documentación de referencia de Java.

- [P+F sobre App Service en Linux](faq-app-service-linux.yml)
- [Variables de entorno y configuración de la aplicación en Azure App Service](reference-app-settings.md)
