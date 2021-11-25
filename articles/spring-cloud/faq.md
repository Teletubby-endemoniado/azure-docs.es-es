---
title: Preguntas frecuentes acerca de Azure Spring Cloud | Microsoft Docs
description: En este artículo se responden las preguntas más frecuentes sobre Azure Spring Cloud.
author: karlerickson
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: karler
ms.custom: devx-track-java
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: ecc563cfc400fcf0e90320cb5c2436fe84cfa0d0
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132704009"
---
# <a name="azure-spring-cloud-faq"></a>Preguntas frecuentes de Azure Spring Cloud

En este artículo se responden las preguntas más frecuentes sobre Azure Spring Cloud.

## <a name="general"></a>General

### <a name="why-azure-spring-cloud"></a>¿Por qué Azure Spring Cloud?

Azure Spring Cloud proporciona una plataforma como servicio (PaaS) para los desarrolladores de Spring Cloud. Azure Spring Cloud administra la infraestructura de la aplicación, de modo que el usuario puede centrarse en el código de la aplicación y en la lógica de negocios. Las características principales integradas en Azure Spring Cloud incluyen Eureka, Config Server, Service Registry Server, Pivotal Build Service e implementación Blue-Green, entre otras. Este servicio también permite a los desarrolladores enlazar sus aplicaciones con otros servicios de Azure, como Azure Cosmos DB, Azure Database for MySQL y Azure Cache for Redis.

Azure Spring Cloud mejora la experiencia de diagnóstico de aplicaciones para los desarrolladores y operadores mediante la integración de Azure Monitor, Application Insights y Log Analytics.

### <a name="how-secure-is-azure-spring-cloud"></a>¿Es seguro Azure Spring Cloud?

La seguridad y la privacidad se encuentran entre las principales prioridades para los clientes de Azure y de Azure Spring Cloud. Azure permite garantizar que solo los clientes tienen acceso a los datos, registros o configuraciones de la aplicación mediante el cifrado seguro de todos estos datos.

* Las instancias de servicio de Azure Spring Cloud están aisladas unas de otras.
* Azure Spring Cloud proporciona administración completa de certificados y TLS/SSL.
* Las revisiones de seguridad críticas de los runtimes de OpenJDK y Spring Cloud se aplicarán a Azure Spring Cloud lo antes posible.

### <a name="how-does-azure-spring-cloud-host-my-applications"></a>¿Cómo hospeda Azure Spring Cloud mis aplicaciones?

Cada instancia de servicio de Azure Spring Cloud cuenta con el respaldo de un clúster de Kubernetes totalmente dedicado con varios nodos de trabajo. Azure Spring Cloud administra el clúster de Kubernetes subyacente de forma automática, lo que incluye la alta disponibilidad, la escalabilidad, la actualización de la versión de Kubernetes, etc.

Azure Spring Cloud programa de forma inteligente las aplicaciones en los nodos de trabajo de Kubernetes subyacentes. Para proporcionar alta disponibilidad, Azure Spring Cloud aplicaciones con dos o más instancias en nodos diferentes.

### <a name="in-which-regions-is-azure-spring-cloud-available"></a>¿En qué regiones está disponible Azure Spring Cloud?

Este de EE. UU., Este de EE. UU. 2, Centro de EE. UU., Centro-sur de EE. UU., Centro-norte de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2, Oeste de Europa, Norte de Europa, Sur de Reino Unido, Sudeste Asiático, Este de Australia, Centro de Canadá, Norte de Emiratos Árabes Unidos, Centro de la India, Centro de Corea del Sur, Este de Asia, Japón Oriental, Norte de Sudáfrica, Sur de Brasil, Centro de Francia y Este de China 2 (Mooncake). [Más información](https://azure.microsoft.com/global-infrastructure/services/?products=spring-cloud)

### <a name="is-any-customer-data-stored-outside-of-the-specified-region"></a>¿Los datos de clientes se almacenan fuera de la región especificada?

Azure Spring Cloud es un servicio regional. Todos los datos de cliente de Azure Spring Cloud se almacenan en una única región especificada. Para más información sobre la geoárea y la región, consulte [Residencia de datos en Azure](https://azure.microsoft.com/global-infrastructure/data-residency/).

### <a name="what-are-the-known-limitations-of-azure-spring-cloud"></a>¿Cuáles son las limitaciones conocidas de Azure Spring Cloud?

Azure Spring Cloud tiene las limitaciones conocidas siguientes:

* `spring.application.name` se sustituirá por el nombre de la aplicación que se usó para crear cada aplicación.
* El valor predeterminado de `server.port` es el puerto 1025. Si se aplica cualquier otro valor, se invalidará. También debe respetar esta configuración y no especificar el puerto del servidor en el código.
* Azure Portal, las plantillas de Azure Resource Manager y Terraform no admiten la carga de paquetes de aplicación. Para cargar paquetes de aplicación, implemente la aplicación mediante la CLI de Azure, Azure DevOps, el complemento Maven para Azure Spring Cloud, Azure Toolkit for IntelliJ y la extensión Visual Studio Code para Azure Spring Cloud.

### <a name="what-pricing-tiers-are-available"></a>¿Qué planes de tarifa están disponibles?

¿Cuál debo usar y cuáles son los límites dentro de cada plan?

* Azure Spring Cloud ofrece dos planes de tarifa: Básico y Estándar. El plan Básico está destinado a desarrollo y pruebas y a probar Azure Spring Cloud. El plan Estándar está optimizado para ejecutar el tráfico de producción de uso general. Consulte [Detalle de precios de Azure Spring Cloud](https://azure.microsoft.com/pricing/details/spring-cloud/) para obtener información sobre los límites y la comparación de niveles de las características.

### <a name="whats-the-difference-between-service-binding-and-service-connector"></a>¿En qué se diferencian Service Binding y Service Connector?
No estamos desarrollando activamente más funcionalidades para Service Binding en favor de la nueva solución de Azure denominada [Service Connector](../service-connector/overview.md). Por un lado, la nueva solución ofrece una experiencia de integración coherente entre los servicios de hospedaje de aplicaciones en Azure, como App Service. Por otro lado, cubre mejor sus necesidades empezando por admitir los 10 servicios de Azure de destino más usados, entre los que se incluyen MySQL, SQL DB, Cosmos DB, Postgres DB, Redis, Storage, etc. Service Connector se encuentra actualmente en versión preliminar pública y le invitamos a probar la nueva experiencia.

### <a name="how-can-i-provide-feedback-and-report-issues"></a>¿Cómo puedo realizar comentarios y notificar incidencias?

Si encuentra algún problema con Azure Spring Cloud, cree una [solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md). Para enviar una solicitud de característica o proporcionar comentarios, vaya a [Comentarios de Azure](https://feedback.azure.com/d365community/forum/79b1327d-d925-ec11-b6e6-000d3a4f06a4).

## <a name="development"></a>Desarrollo

### <a name="i-am-a-spring-cloud-developer-but-new-to-azure-what-is-the-quickest-way-for-me-to-learn-how-to-develop-an-application-in-azure-spring-cloud"></a>Soy un desarrollador de Spring Cloud, pero nunca he usado Azure. ¿Cuál es la forma más rápida de aprender a desarrollar una aplicación de Azure Spring Cloud?

Para conocer la forma más rápida de comenzar con Azure Spring Cloud, siga las instrucciones del [inicio rápido en el que se explica cómo iniciar una aplicación de Azure Spring Cloud mediante Azure Portal](./quickstart.md).

::: zone pivot="programming-language-java"
### <a name="what-java-runtime-does-azure-spring-cloud-support"></a>¿Qué runtime de Java es compatible con Azure Spring Cloud?

Azure Spring Cloud es compatible con Java 8 y 11. Consulte las [versiones del sistema operativo y el runtime de Java](#java-runtime-and-os-versions).

### <a name="is-spring-boot-24x-supported"></a>¿Se admite Spring Boot 2.4.x?
Se ha identificado un problema con Spring Boot 2.4 y se está trabajando con la comunidad de Spring para resolverlo. Mientras tanto, incluya estas dos dependencias para habilitar la autenticación TLS entre las aplicaciones y Eureka.

```xml
<dependency>
    <groupId>com.sun.jersey</groupId>
    <artifactId>jersey-client</artifactId>
    <version>1.19.4</version>
</dependency>
<dependency>
    <groupId>com.sun.jersey.contribs</groupId>
    <artifactId>jersey-apache-client4</artifactId>
    <version>1.19.4</version>
</dependency>
```

::: zone-end

### <a name="where-can-i-view-my-spring-cloud-application-logs-and-metrics"></a>¿Dónde puedo ver mis métricas y registros de aplicaciones de Spring Cloud?

Busque las métricas en la pestaña Información general de la aplicación y en la pestaña [Azure Monitor](../azure-monitor/essentials/data-platform-metrics.md#metrics-explorer).

Azure Spring Cloud admite la exportación de las métricas y los registros de aplicaciones de Spring Cloud a Azure Storage, EventHub y [Log Analytics](../azure-monitor/logs/data-platform-logs.md). El nombre de la tabla en Log Analytics es *AppPlatformLogsforSpring*. Para obtener más información sobre cómo habilitarlo, consulte [Servicios de diagnóstico](diagnostic-services.md).

### <a name="does-azure-spring-cloud-support-distributed-tracing"></a>¿Admite Azure Spring Cloud el seguimiento distribuido?

Sí. Para más información, consulte el [Tutorial: uso del seguimiento distribuido con Azure Spring Cloud](./how-to-distributed-tracing.md).

::: zone pivot="programming-language-java"
### <a name="what-resource-types-does-service-binding-support"></a>¿Qué tipos de recursos admiten el enlace de servicios?

Actualmente se admiten tres servicios:

* Azure Cosmos DB
* Azure Database for MySQL
* Azure Cache for Redis.
::: zone-end

### <a name="can-i-view-add-or-move-persistent-volumes-from-inside-my-applications"></a>¿Puedo ver, agregar o mover volúmenes persistentes desde dentro de mis aplicaciones?

Sí.

### <a name="how-many-outbound-public-ip-addresses-does-an-azure-spring-cloud-instance-have"></a>¿Cuántas IP publicas de salida tiene una instancia de Azure Spring Cloud?

El número de IP públicas de salida puede variar en función de los niveles y otros factores.

| Tipo de instancia de Azure Spring Cloud | Número predeterminado de IP públicas de salida |
| -------------------------------- | ---------------------------------------------- |
| Instancias de nivel Básico             | 1                                              |
| Instancias de nivel Estándar          | 2                                              |
| Instancias de inserción de red virtual         | 1                                              |

### <a name="can-i-increase-the-number-of-outbound-public-ip-addresses"></a>¿Puedo aumentar el número de IP públicas de salida?

Sí, puede abrir una [incidencia de soporte técnico](https://azure.microsoft.com/support/faq/) para solicitar más IP públicas de salida.

### <a name="when-i-deletemove-an-azure-spring-cloud-service-instance-will-its-extension-resources-be-deletedmoved-as-well"></a>Cuando elimino o traslado una instancia del servicio Azure Spring Cloud, ¿también se eliminarán o trasladarán sus recursos de extensión?

Depende de la lógica de los proveedores de recursos a los que pertenecen los recursos de extensión. Los recursos de extensión de una instancia de `Microsoft.AppPlatform` no pertenecen al mismo espacio de nombres, por lo que el comportamiento varía en función del proveedor de recursos. Por ejemplo, la operación de eliminación o traslado no se aplicará en cascada a los recursos de la **configuración de diagnósticos**. Si se aprovisiona una nueva instancia de Azure Spring Cloud con el mismo id. de recurso que la eliminada o si la instancia de Azure Spring Cloud anterior se vuelve a trasladar, los recursos de la **configuración de diagnósticos** anteriores siguen extendiéndola.

Puede eliminar la configuración de diagnóstico de Spring Cloud mediante la CLI de Azure:

```azurecli
 az monitor diagnostic-settings delete --name $diagnosticSettingName --resource $azureSpringCloudResourceId
```

::: zone pivot="programming-language-java"
## <a name="java-runtime-and-os-versions"></a>Runtime de Java y versiones del sistema operativo

### <a name="which-versions-of-java-runtime-are-supported-in-azure-spring-cloud"></a>¿Qué versiones del runtime de Java se admiten en Azure Spring Cloud?

Azure Spring Cloud es compatible con las versiones LTS de Java con las compilaciones más recientes; actualmente, en junio de 2020, se admiten Java 8 y Java 11. Consulte [Instalación del JDK para Azure y Azure Stack](/azure/developer/java/fundamentals/java-jdk-install).

### <a name="who-built-these-java-runtimes"></a>¿Quién ha compilado estos runtimes de Java?

Azul Systems. Las compilaciones del JDK Azul Zulu for Azure - Enterprise Edition son una distribución gratuita, multiplataforma y lista para producción del OpenJDK para Azure y Azure Stack, con la tecnología de Microsoft y Azul Systems. Contienen todos los componentes para la creación y ejecución de aplicaciones de Java SE.

### <a name="how-often-will-java-runtimes-get-updated"></a>¿Con qué frecuencia se actualizan los runtimes de Java?

Las versiones LTS y MTS del JDK tienen actualizaciones de seguridad trimestrales, correcciones de errores, así como actualizaciones críticas y revisiones puntuales según sea necesario. Este soporte técnico incluye entregas de actualizaciones de seguridad y correcciones de errores para Java 7 y 8 que figuran en las versiones más recientes de Java, como Java 11.

### <a name="how-long-will-java-8-and-java-11-lts-versions-be-supported"></a>¿Cuánto tiempo se admitirán las versiones LTS de Java 8 y Java 11?

Consulte [Soporte técnico de Java a largo plazo para Azure y Azure Stack](/azure/developer/java/fundamentals/java-support-on-azure).

* Java 8 LTS tiene soporte técnico hasta diciembre de 2030.
* Java 11 LTS tiene soporte técnico hasta septiembre de 2027.

### <a name="how-can-i-download-a-supported-java-runtime-for-local-development"></a>¿Cómo puedo descargar un runtime de Java compatible para el desarrollo local?

Consulte [Instalación del JDK para Azure y Azure Stack](/azure/developer/java/fundamentals/java-jdk-install).

### <a name="what-is-the-retire-policy-for-older-java-runtimes"></a>¿Cuál es la directiva de retirada de los runtimes de Java más antiguos?

Se realizará un anuncio público 12 meses antes de retirar cualquier versión anterior del runtime. Tendrá 12 meses para migrar a una versión posterior.

* Los administradores de la suscripción recibirán una notificación por correo electrónico acerca de cuándo se retirará una versión de Java.
* La información de retirada se publicará en la documentación.

### <a name="how-can-i-get-support-for-issues-at-the-java-runtime-level"></a>¿Cómo puedo obtener soporte técnico para problemas en el nivel de runtime de Java?

Puede abrir una incidencia de soporte técnico con el departamento de Soporte técnico de Azure.  Consulte [Creación de una solicitud de soporte técnico de Azure](../azure-portal/supportability/how-to-create-azure-support-request.md).

### <a name="what-is-the-operation-system-to-run-my-apps"></a>¿En qué sistema operativo se ejecutan mis aplicaciones?

Se utiliza la versión LTS de Ubuntu más reciente; actualmente, el sistema operativo predeterminado es [Ubuntu 20.04 LTS (Focal Fossa)](https://releases.ubuntu.com/focal/).

### <a name="how-often-are-os-security-patches-applied"></a>¿Con qué frecuencia se aplican las actualizaciones de seguridad del sistema operativo?

Las actualizaciones de seguridad aplicables a Azure Spring Cloud se implementarán en producción mensualmente.
Las actualizaciones de seguridad críticas (puntuación CVE >= 9) aplicables a Azure Spring Cloud se implementarán lo antes posible.
::: zone-end

## <a name="deployment"></a>Implementación

### <a name="does-azure-spring-cloud-support-blue-green-deployment"></a>¿Admite Azure Spring Cloud la implementación blue-green?

Sí. Para obtener más información, consulte [Configuración de un entorno de ensayo](./how-to-staging-environment.md).

### <a name="can-i-access-kubernetes-to-manipulate-my-application-containers"></a>¿Puedo acceder a Kubernetes para manipular mis contenedores de aplicaciones?

No.  Azure Spring Cloud ayuda a desviar la atención del desarrollador de la arquitectura subyacente, lo que le permite concentrarse en el código de la aplicación y en la lógica de negocios.

### <a name="does-azure-spring-cloud-support-building-containers-from-source"></a>¿Admite Azure Spring Cloud la creación de contenedores desde el origen?

Sí. Para obtener más información, consulte [Inicio de la aplicación Spring Cloud desde el código fuente](./quickstart.md).

### <a name="does-azure-spring-cloud-support-autoscaling-in-app-instances"></a>¿Admite Azure Spring Cloud la escalabilidad automática en instancias de aplicaciones?

Sí. Para más información, consulte [Configuración de la escalabilidad automática para aplicaciones de microservicios](./how-to-setup-autoscale.md).

### <a name="how-does-azure-spring-cloud-monitor-the-health-status-of-my-application"></a>¿Cómo supervisa Azure Spring Cloud el estado de mantenimiento de mi aplicación?

Azure Spring Cloud sondea continuamente el puerto 1025 en busca de las aplicaciones del cliente. Estos sondeos determinan si el contenedor de aplicaciones está listo para empezar a aceptar tráfico y Azure Spring Cloud necesita reiniciar el contenedor de aplicaciones. Internamente, Azure Spring Cloud sondeos de disponibilidad y disponibilidad de Kubernetes para lograr la supervisión del estado.

>[!NOTE]
> Debido a estos sondeos, actualmente no se pueden iniciar aplicaciones en Azure Spring Cloud sin exponer el puerto 1025.

### <a name="whether-and-when-will-my-application-be-restarted"></a>¿Se va a reiniciar mi aplicación?, ¿cuándo?

Sí. Para más información, consulte [Supervisión de eventos del ciclo de vida de la aplicación mediante el registro de actividad de Azure y Azure Service Health](./monitor-app-lifecycle-events.md).

::: zone pivot="programming-language-java"
### <a name="what-are-the-best-practices-for-migrating-existing-spring-cloud-microservices-to-azure-spring-cloud"></a>¿Cuáles son los procedimientos recomendados para migrar los microservicios de Spring Cloud existentes a Azure Spring Cloud?

Para más información, consulte [Migración de aplicaciones de Spring Cloud a Azure Spring Cloud](/azure/developer/java/migration/migrate-spring-cloud-to-azure-spring-cloud).
::: zone-end

::: zone pivot="programming-language-csharp"
## <a name="net-core-versions"></a>Versiones de .NET Core

### <a name="which-net-core-versions-are-supported"></a>¿Qué versiones de .NET Core son compatibles?

.NET Core 3.1 y versiones posteriores.

### <a name="how-long-will-net-core-31-be-supported"></a>¿Cuánto tiempo se admitirá .NET Core 3.1?

Hasta el 3 de diciembre de 2022. Consulte [Visualización de la directiva de compatibilidad de .NET Core](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).
::: zone-end

## <a name="troubleshooting"></a>Solución de problemas

### <a name="what-are-the-impacts-of-service-registry-rarely-unavailable"></a>¿Cuáles son las repercusiones del registro de servicio que raramente no está disponible?

En algunos escenarios que rara vez se han producido, es posible que vea algunos errores como el siguiente en los registros de la aplicación:

```output
RetryableEurekaHttpClient: Request execution failure with status code 401; retrying on another server if available
```

Este es un problema introducido por el marco de Spring con una tasa muy baja debido a la inestabilidad de la red o a otros problemas de la red.

No debe haber ninguna repercusión en la experiencia del usuario, ya que el cliente Eureka tiene una directiva de reintentos y latido para poder encargarse de ello. Puede considerarlo como un error transitorio y omitirlo de forma segura.

Mejoraremos esta parte y evitaremos este error de las aplicaciones de los usuarios en breve.

## <a name="next-steps"></a>Pasos siguientes

Si tiene más preguntas, consulte la [guía de solución de problemas de Azure Spring Cloud](./troubleshoot.md).