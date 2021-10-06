---
title: Preparación de una aplicación para su implementación en Azure Spring Cloud
description: Obtenga información sobre cómo preparar una aplicación Java Spring para su implementación en Azure Spring Cloud.
author: karlerickson
ms.service: spring-cloud
ms.topic: how-to
ms.date: 07/06/2021
ms.author: karler
ms.custom: devx-track-java
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: 9def9f39e28851498c7bf87d5b6b2e7d0a26f2c2
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128599418"
---
# <a name="prepare-an-application-for-deployment-in-azure-spring-cloud"></a>Preparación de una aplicación Java Spring para su implementación en Azure Spring Cloud

::: zone pivot="programming-language-csharp"
Azure Spring Cloud proporciona servicios sólidos para hospedar, supervisar, escalar y actualizar una aplicación de Steeltoe. En este artículo se muestra cómo preparar una aplicación Steeltoe existente para su implementación en Azure Spring Cloud.

En este artículo se explican las dependencias, la configuración y el código necesarios para ejecutar una aplicación Steeltoe de .NET Core en Azure Spring Cloud. Para obtener información sobre cómo implementar una aplicación en Azure Spring Cloud, consulte [implementación de la primera aplicación de Azure Spring Cloud](./quickstart.md).

>[!Note]
> Compatibilidad de Steeltoe con Azure Spring Cloud se ofrece actualmente como versión preliminar pública. Las ofertas de versión preliminar pública permiten a los clientes experimentar con nuevas características antes de su publicación oficial.  Los servicios y las características en versión preliminar pública no están diseñados para su uso en producción.  Para más información sobre el soporte técnico durante las versiones preliminares, revise las [preguntas frecuentes](https://azure.microsoft.com/support/faq/) o envíe una [solicitud de soporte técnico](../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="supported-versions"></a>Versiones compatibles

Azure Spring Cloud es compatible con:

* .NET Core 3.1
* Steeltoe 2.4 y 3.0

## <a name="dependencies"></a>Dependencias

Para Steeltoe 2.4, agregue el paquete [Microsoft.Azure.SpringCloud.Client 1.x.x](https://www.nuget.org/packages/Microsoft.Azure.SpringCloud.Client/) más reciente al archivo del proyecto:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Azure.SpringCloud.Client" Version="1.0.0-preview.1" />
  <PackageReference Include="Steeltoe.Discovery.ClientCore" Version="2.4.4" />
  <PackageReference Include="Steeltoe.Extensions.Configuration.ConfigServerCore" Version="2.4.4" />
  <PackageReference Include="Steeltoe.Management.TracingCore" Version="2.4.4" />
  <PackageReference Include="Steeltoe.Management.ExporterCore" Version="2.4.4" />
</ItemGroup>
```

Para Steeltoe 3.0, agregue el paquete [Microsoft.Azure.SpringCloud.Client 2.x.x](https://www.nuget.org/packages/Microsoft.Azure.SpringCloud.Client/) más reciente al archivo del proyecto:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Azure.SpringCloud.Client" Version="2.0.0-preview.1" />
  <PackageReference Include="Steeltoe.Discovery.ClientCore" Version="3.0.0" />
  <PackageReference Include="Steeltoe.Extensions.Configuration.ConfigServerCore" Version="3.0.0" />
  <PackageReference Include="Steeltoe.Management.TracingCore" Version="3.0.0" />
</ItemGroup>
```

## <a name="update-programcs"></a>Actualizar Program.cs

En el método `Program.Main`, llame al método `UseAzureSpringCloudService`.

Para Steeltoe 2.4.4, llame a `UseAzureSpringCloudService` después de `ConfigureWebHostDefaults` y de `AddConfigServer`, si se ha llamado:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        })
        .AddConfigServer()
        .UseAzureSpringCloudService();
```

Para Steeltoe 3.0.0, llame a `UseAzureSpringCloudService` antes de `ConfigureWebHostDefaults` y antes de cualquier código de configuración de Steeltoe:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .UseAzureSpringCloudService()
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        })
        .AddConfigServer();
```

## <a name="enable-eureka-server-service-discovery"></a>Habilitación de la detección de servicios del servidor de Eureka

En el origen de configuración que se usará cuando la aplicación se ejecute en Azure Spring Cloud, establezca `spring.application.name` en el mismo nombre que la aplicación Azure Spring Cloud en la que se implementará el proyecto.

Por ejemplo, si implementa un proyecto .NET denominado `EurekaDataProvider` en una aplicación de Azure Spring Cloud denominada `planet-weather-provider` el archivo *appSettings.json* debe incluir el siguiente JSON:

```json
"spring": {
  "application": {
    "name": "planet-weather-provider"
  }
}
```

## <a name="use-service-discovery"></a>Uso de la detección de servicios

Para llamar a un servicio mediante la detección de servicios del servidor de Eureka, realice solicitudes HTTP a `http://<app_name>`, donde `app_name` sea el valor de `spring.application.name` de la aplicación de destino. Por ejemplo, el código siguiente llama al servicio `planet-weather-provider`:

```csharp
using (var client = new HttpClient(discoveryHandler, false))
{
    var responses = await Task.WhenAll(
        client.GetAsync("http://planet-weather-provider/weatherforecast/mercury"),
        client.GetAsync("http://planet-weather-provider/weatherforecast/saturn"));
    var weathers = await Task.WhenAll(from res in responses select res.Content.ReadAsStringAsync());
    return new[]
    {
        new KeyValuePair<string, string>("Mercury", weathers[0]),
        new KeyValuePair<string, string>("Saturn", weathers[1]),
    };
}
```

::: zone-end

::: zone pivot="programming-language-java"
En este tema se muestra cómo preparar una aplicación Java Spring existente para su implementación en Azure Spring Cloud. Si se ha configurado correctamente, Azure Spring Cloud proporciona servicios sólidos para supervisar, escalar y actualizar cualquier aplicación Java Spring Cloud.

Antes de ejecutar este ejemplo, puede probar la [guía de inicio rápido básica](./quickstart.md).

En otros ejemplos se explica cómo implementar una aplicación en Azure Spring Cloud cuando se configura el archivo POM.

* [Inicio de la primera aplicación](./quickstart.md)
* [Compilación y ejecución de microservicios](./quickstart-sample-app-introduction.md)

En este artículo se explican las dependencias necesarias y cómo agregarlas al archivo POM.

## <a name="java-runtime-version"></a>Versión del entorno de ejecución de Java

Las aplicaciones de Spring/Java son las únicas que se pueden ejecutar en Azure Spring Cloud.

Azure Spring Cloud es compatible con Java 8 y Java 11. El entorno de hospedaje contiene la versión más reciente de Azul Zulu OpenJDK para Azure. Para más información sobre Azul Zulu OpenJDK para Azure, consulte [Instalación del JDK para Azure y Azure Stack](/azure/developer/java/fundamentals/java-jdk-install).

## <a name="spring-boot-and-spring-cloud-versions"></a>Versiones de Spring Boot y Spring Cloud

Para preparar una aplicación de Spring Boot existente para la implementación en Azure Spring Cloud, incluya las dependencias de Spring Boot y Spring Cloud en el archivo POM de la aplicación, como se muestra en las siguientes secciones.

Azure Spring Cloud será compatible con la versión Spring Boot o Spring Cloud más reciente en un plazo de un mes después de su publicación. Puede obtener versiones compatibles en las páginas de [versiones de Spring Boot](https://github.com/spring-projects/spring-boot/wiki/Supported-Versions#releases) y [versiones de Spring Cloud](https://github.com/spring-projects/spring-boot/wiki/Supported-Versions#releases), respectivamente. 

En la tabla siguiente se enumeran las combinaciones admitidas de Spring Boot y Spring Cloud:

Versión de Spring Boot | Versión de Spring Cloud
---|---
2.3.x | Hoxton.SR8+
2.4.x, 2.5.x | 2020.0 también llamado Ilford +

> [!NOTE]
> - Actualice Spring Boot a la versión 2.5.2 o 2.4.8 para abordar el siguiente informe CVE [CVE-2021-22119: ataque por denegación de servicio con spring-security-oauth2-client](https://tanzu.vmware.com/security/cve-2021-22119). Si usa Spring Security, actualícelo a la versión 5.5.1, 5.4.7, 5.3.10 o 5.2.11.
> - Se ha identificado un problema con Spring Boot 2.4.0 en la autenticación TLS entre las aplicaciones y el registro de Spring Cloud Service. Use la versión 2.4.1 o superior. Consulte las [Preguntas frecuentes](./faq.md?pivots=programming-language-java#development) para obtener una solución alternativa si insiste en usar la versión 2.4.0.

### <a name="dependencies-for-spring-boot-version-23"></a>Dependencias de Spring Boot, versión 2.3

En Spring Boot 2.3, agregue las siguientes dependencias al archivo POM de la aplicación.

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Hoxton.SR8</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

### <a name="dependencies-for-spring-boot-version-2425"></a>Dependencias de Spring Boot, versión 2.4/2.5

Para la versión 2.4/2.5 de Spring Boot, agregue las dependencias siguientes al archivo POM de la aplicación.

```xml
    <!-- Spring Boot dependencies -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.8</version>
    </parent>

    <!-- Spring Cloud dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>2020.0.2</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```

> [!WARNING]
> No especifique `server.port` en la configuración. Azure Spring Cloud invalidará esta configuración en un número de puerto fijo. También debe respetar esta configuración y no especificar el puerto del servidor en el código.

## <a name="other-recommended-dependencies-to-enable-azure-spring-cloud-features"></a>Otras dependencias recomendadas para habilitar las características de Azure Spring Cloud

Para habilitar las características integradas de Azure Spring Cloud desde el registro de servicio a la traza distribuida, debe incluir también las dependencias siguientes en la aplicación. Puede quitar algunas de estas dependencias si no necesita las características correspondientes para las aplicaciones específicas.

### <a name="service-registry"></a>Registro del servicio

Para usar el servicio Azure Service Registry administrado, incluya la dependencia `spring-cloud-starter-netflix-eureka-client` en el archivo pom.xml, como se muestra aquí:

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
```

El punto de conexión del servidor de Service Registry se inserta automáticamente como variables de entorno con la aplicación. Así, las aplicaciones se pueden registrar por sí solas en el servidor de Service Registry y detectar otros microservicios dependientes.

#### <a name="enablediscoveryclient-annotation"></a>Anotación de EnableDiscoveryClient

Agregue la anotación siguiente al código fuente de la aplicación.

```java
@EnableDiscoveryClient
```

Por ejemplo, vea la aplicación piggymetrics de ejemplos anteriores:

```java
package com.piggymetrics.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.zuul.EnableZuulProxy;

@SpringBootApplication
@EnableDiscoveryClient
@EnableZuulProxy

public class GatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(GatewayApplication.class, args);
    }
}
```

### <a name="distributed-configuration"></a>Configuración distribuida

Para habilitar la configuración distribuida, incluya la siguiente dependencia `spring-cloud-config-client` en la sección de dependencias del archivo pom.xml:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-client</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
</dependency>
```

> [!WARNING]
> No especifique `spring.cloud.config.enabled=false` en la configuración de arranque. De lo contrario, la aplicación dejará de funcionar con Config Server.

### <a name="metrics"></a>Métricas

Incluya la dependencia `spring-boot-starter-actuator` en la sección de dependencias del archivo pom.xml, como se muestra aquí:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

 Se extraen métricas periódicamente de los puntos de conexión JMX. Estas métricas se pueden visualizar mediante Azure Portal.

 > [!WARNING]
 > Especifique `spring.jmx.enabled=true` en la propiedad de configuración. De lo contrario, las métricas no se pueden visualizar en Azure Portal.

## <a name="see-also"></a>Consulte también

* [Análisis de los registros y las métricas de la aplicación](./diagnostic-services.md)
* [Configuración de un servidor de configuración](./how-to-config-server.md)
* [Inicio rápido de Spring](https://spring.io/quickstart)
* [Documentación de Spring Boot](https://spring.io/projects/spring-boot)

## <a name="next-steps"></a>Pasos siguientes

En este tema ha aprendido a configurar una aplicación Java Spring para su implementación en Azure Spring Cloud. Para aprender a configurar una instancia de Config Server, consulte el artículo [Configuración de una instancia de Config Server](./how-to-config-server.md).

Hay más ejemplos disponibles en GitHub: [Ejemplos de Azure Spring Cloud](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples).
::: zone-end
