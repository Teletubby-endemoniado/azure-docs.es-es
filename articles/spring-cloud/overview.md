---
title: Introducción a Azure Spring Cloud
description: Aprenda las características y ventajas de Azure Spring Cloud para implementar y administrar aplicaciones Java Spring en Azure.
author: karlerickson
ms.service: spring-cloud
ms.topic: overview
ms.date: 03/09/2021
ms.author: karler
ms.custom: devx-track-java, contperf-fy21q2
customer intent: As an Azure Cloud user, I want to deploy, run, and monitor Spring Boot microservices.
ms.openlocfilehash: 8720a24e975ce7a73ed49f92460de301b5c8dfd8
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132485687"
---
# <a name="what-is-azure-spring-cloud"></a>¿Qué es Azure Spring Cloud?

Azure Spring Cloud facilita la implementación de aplicaciones de microservicio de Spring Boot en Azure sin necesidad de realizar cambios en el código.  El servicio administra la infraestructura de las aplicaciones de Spring Cloud, con el fin de que los desarrolladores puedan centrarse en el código.  Azure Spring Cloud proporciona administración del ciclo de vida mediante el uso de una supervisión y un diagnóstico completos, administración de la configuración, detección de servicios, integración de CI/CD e implementaciones blue-green, entre otros.

En el vídeo siguiente se muestra una aplicación compuesta de aplicaciones de Spring Boot que se ejecutan en Azure mediante Azure Spring Cloud.

<br>

> [!VIDEO https://www.youtube.com/embed/1jOXMFc1oRg]

## <a name="why-use-azure-spring-cloud"></a>Razones para usar Azure Spring Cloud

La implementación de aplicaciones en Azure Spring Cloud tiene muchas ventajas. Puede:

* Migrar de forma eficaz las aplicaciones Spring existentes y administrar los costos y el escalado en la nube.
* Modernizar las aplicaciones con patrones de Spring Cloud para mejorar la agilidad y la velocidad de entrega.
* Ejecutar Java a escala en la nube e impulsar un mayor uso sin una infraestructura complicada.
* Desarrollar e implementar rápidamente sin dependencias de la contenedorización.
* Supervisar las cargas de trabajo de producción de forma eficaz y sin esfuerzo.

Azure Spring Cloud es compatible con aplicaciones de [Spring Boot](https://spring.io/projects/spring-boot) para Java y de [Steeltoe](https://steeltoe.io/) para ASP.NET Core. Steeltoe se ofrece actualmente como versión preliminar pública. Las ofertas de versión preliminar pública le permiten experimentar con nuevas características antes de su publicación oficial.

## <a name="service-overview"></a>Introducción al servicio

Al formar parte del ecosistema de Azure, Azure Spring Cloud facilita el enlace con otros servicios de Azure, como almacenamiento, bases de datos, supervisión, etc.

![Introducción a Azure Spring Cloud](media/spring-cloud-principles/azure-spring-cloud-overview.png)

* Azure Spring Cloud es un servicio totalmente administrado para aplicaciones de Spring Boot que le permite centrarse en la creación y ejecución de aplicaciones sin la molestia de administrar la infraestructura.

* No tiene más que implementar los archivos JAR o el código de la aplicación Spring Boot o Zip for your Steeltoe, y Azure Spring Cloud conectará automáticamente sus aplicaciones con el runtime del servicio Spring y el ciclo de vida de la aplicación integrada.

* La supervisión es simple. Después de la implementación puede supervisar el rendimiento de la aplicación, corregir los errores y mejorar rápidamente las aplicaciones.

* Integración completa con los servicios y ecosistemas de Azure.

* Azure Spring Cloud está preparado para la empresa y tiene una infraestructura totalmente administrada, administración del ciclo de vida integrada y es fácil de supervisar.

## <a name="documentation-overview"></a>Introducción a la documentación

En esta documentación se incluyen secciones en las que se explica no solo cómo empezar a usar los servicios de Azure Spring Cloud, sino también cómo sacar provecho de ellos.

* Primeros pasos

    * [Inicio de la primera aplicación](./quickstart.md)
    * [Aprovisionamiento de un servicio de Azure Spring Cloud](./quickstart-provision-service-instance.md)
    * [Configuración del servidor de configuración]()
    * [Compilación e implementación de aplicaciones](./quickstart-deploy-apps.md)
    * [Uso de registros, métricas y seguimiento](./quickstart-logs-metrics-tracing.md)

* Instrucciones

    * [Desarrollo](how-to-prepare-app-deployment.md): Prepare una aplicación de Java Spring existente para su implementación en Azure Spring Cloud. Si se configura correctamente, Azure Spring Cloud proporciona servicios sólidos para supervisar, escalar y actualizar aplicaciones de Java Spring Cloud.
    * [Implementación](./how-to-staging-environment.md): Cómo configurar una implementación de almacenamiento provisional con el patrón de implementación blue-green en Azure Spring Cloud. La implementación azul/verde es un patrón de entrega continua de Azure DevOps que se basa en mantener una versión existente (azul) activa mientras se implementa una nueva (verde).
    * [Configurar aplicaciones](./how-to-start-stop-delete.md): inicie, detenga y elimine una aplicación en Azure Spring. Cambie el estado de una aplicación en Azure Spring Cloud mediante Azure Portal o la CLI de Azure.
    * [Escala](./how-to-scale-manual.md): escale cualquier aplicación de microservicios desde el panel de Azure Spring Cloud de Azure Portal o mediante la configuración de la escalabilidad automática. Hay direcciones IP públicas disponibles para comunicarse con recursos externos, como las bases de datos, el almacenamiento y los almacenes de claves.
    * [Supervisión de aplicaciones](./how-to-distributed-tracing.md): herramientas de seguimiento distribuido para depurar y supervisar fácilmente problemas complejos. Azure Spring Cloud integra Spring Cloud Sleuth con Application Insights de Azure. Esta integración proporciona una funcionalidad eficaz de seguimiento distribuido desde Azure Portal.
    * [Protección de aplicaciones](./how-to-enable-system-assigned-managed-identity.md): los recursos de Azure proporcionan una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código.
    * [Integración con otros servicios de Azure](./how-to-bind-cosmos.md): en lugar de configurar manualmente las aplicaciones de Spring Boot, puede enlazar automáticamente determinados servicios de Azure a sus aplicaciones, por ejemplo, puede enlazar una aplicación a una base de datos de Azure Cosmos DB.
    * [Automatización](./how-to-cicd.md): Las herramientas de integración continua y entrega continua permiten implementar rápidamente las actualizaciones en las aplicaciones existentes con el mínimo esfuerzo y riesgo. Azure DevOps ayuda a organizar y controlar estas tareas clave.
    * [Solución de problemas](./how-to-self-diagnose-solve.md): los diagnósticos de Azure Spring Cloud proporcionan una experiencia interactiva que sirve de ayuda para solucionar problemas de las aplicaciones. No se requiere ninguna configuración. Cuando encuentre problemas, el diagnóstico de Azure Spring Cloud los identifica y le guía por la información que ayuda a solucionarlos.
    * [Migración](/azure/developer/java/migration/migrate-spring-boot-to-azure-spring-cloud): Cómo migrar una aplicación de Spring Cloud o de Spring Boot existente para ejecutarse en Azure Spring Cloud.

Para empezar, consulte:

> [!div class="nextstepaction"]
> [Inicio rápido de Spring Cloud](./quickstart.md)

Los ejemplos están disponibles en GitHub: [Ejemplos de Azure Spring Cloud](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/).
