---
title: 'Tutorial: Uso del panel de Circuit Breaker con Azure Spring Cloud'
description: Aprenda cómo utilizar el panel de Circuit Breaker con Azure Spring Cloud.
author: karlerickson
ms.author: karler
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 04/06/2020
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 6fce05bf1f853edd527df920f579689e23f080c8
ms.sourcegitcommit: 7f3ed8b29e63dbe7065afa8597347887a3b866b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "122015668"
---
# <a name="tutorial-use-circuit-breaker-dashboard-with-azure-spring-cloud"></a>Tutorial: Uso del panel de Circuit Breaker con Azure Spring Cloud

**Este artículo se aplica a:** ✔️ Java

Spring [Cloud Netflix Turbine](https://github.com/Netflix/Turbine) se usa con frecuencia para agregar varias secuencias de métricas [Hystrix](https://github.com/Netflix/Hystrix), de modo que las secuencias se puedan supervisar en una sola vista mediante el panel de Hystrix. En este tutorial se muestra cómo usarlas en Azure Spring Cloud.

> [!NOTE]
> Netflix Hystrix se usa ampliamente en muchas aplicaciones de Spring Cloud existentes, pero ya no está en desarrollo activo. Si está desarrollando un proyecto nuevo, utilice en su lugar implementaciones de Spring Cloud Circuit Breaker como [resilience4j](https://github.com/resilience4j/resilience4j). A diferencia de Turbine, que se muestra en este tutorial, el nuevo marco de Spring Cloud Circuit Breaker unifica todas las implementaciones de su canalización de datos de métricas en Micrometer, que también se admite en Azure Spring Cloud. [Más información](./how-to-circuit-breaker-metrics.md).

## <a name="prepare-your-sample-applications"></a>Preparación de las aplicaciones de ejemplo

El ejemplo se bifurca desde este [repositorio](https://github.com/StackAbuse/spring-cloud/tree/master/spring-turbine).

Clone el repositorio de ejemplo en el entorno de desarrollo:

```bash
git clone https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples.git
cd Azure-Spring-Cloud-Samples/hystrix-turbine-sample
```

Compile las 3 aplicaciones que se usarán en este tutorial:

* user-service: Un servicio REST sencillo que tiene un único punto de conexión de /personalized/{id}
* recommendation-service: Un servicio REST sencillo que tiene un único punto de conexión de /recommendations, al que llamará user-service.
* hystrix-turbine: Un servicio de panel de Hystrix para mostrar secuencias de Hystrix y un servicio de Turbine que agrega una secuencia de métricas de Hystrix de otros servicios.

```bash
mvn clean package -D skipTests -f user-service/pom.xml
mvn clean package -D skipTests -f recommendation-service/pom.xml
mvn clean package -D skipTests -f hystrix-turbine/pom.xml
```

## <a name="provision-your-azure-spring-cloud-instance"></a>Aprovisionamiento de la instancia de Azure Spring Cloud

Siga el procedimiento [Aprovisionamiento de una instancia de servicio en la CLI de Azure](./quickstart.md#provision-an-instance-of-azure-spring-cloud).

## <a name="deploy-your-applications-to-azure-spring-cloud"></a>Implementación de las aplicaciones en Azure Spring Cloud

Estas aplicaciones no usan **Config Server**, por lo que no es necesario configurar **Config Server** para Azure Spring Cloud.  Cree e implemente de la manera siguiente:

```azurecli
az spring-cloud app create -n user-service --assign-endpoint
az spring-cloud app create -n recommendation-service
az spring-cloud app create -n hystrix-turbine --assign-endpoint

az spring-cloud app deploy -n user-service --jar-path user-service/target/user-service.jar
az spring-cloud app deploy -n recommendation-service --jar-path recommendation-service/target/recommendation-service.jar
az spring-cloud app deploy -n hystrix-turbine --jar-path hystrix-turbine/target/hystrix-turbine.jar
```

## <a name="verify-your-apps"></a>Comprobación de las aplicaciones

Una vez que todas las aplicaciones se estén ejecutando y sean reconocibles, acceda a `user-service` con la ruta de acceso `https://<username>-user-service.azuremicroservices.io/personalized/1` desde el explorador. Si user-service puede acceder a `recommendation-service`, debería obtener la siguiente salida. Actualice la página web varias veces si no funciona.

```json
[{"name":"Product1","description":"Description1","detailsLink":"link1"},{"name":"Product2","description":"Description2","detailsLink":"link3"},{"name":"Product3","description":"Description3","detailsLink":"link3"}]
```

## <a name="access-your-hystrix-dashboard-and-metrics-stream"></a>Acceso al panel de Hystrix y a la secuencia de métricas

Compruebe el uso de puntos de conexión públicos o puntos de conexión de prueba privados.

### <a name="using-public-endpoints"></a>Uso de puntos de conexión públicos

Acceda a Hystrix-Turbine con la ruta de acceso `https://<SERVICE-NAME>-hystrix-turbine.azuremicroservices.io/hystrix` desde el explorador.  En la siguiente ilustración se muestra el panel de Hystrix que se ejecuta en esta aplicación.

![Panel de Hystrix](media/spring-cloud-circuit-breaker/hystrix-dashboard.png)

Copie la dirección URL de la secuencia de Turbine `https://<SERVICE-NAME>-hystrix-turbine.azuremicroservices.io/turbine.stream?cluster=default` en el cuadro de texto y seleccione **Monitor Stream** (Supervisar secuencia).  Esta acción mostrará el panel. Si no se muestra nada en el visor, ejecute los puntos de conexión `user-service` para generar secuencias.

![La secuencia de Hystrix](media/spring-cloud-circuit-breaker/hystrix-stream.png) Ahora puede experimentar con el panel de Circuit Breaker.

> [!NOTE]
> En un entorno de producción, el panel de Hystrix y la secuencia de métricas no se deben exponer a Internet.

### <a name="using-private-test-endpoints"></a>Uso de puntos de conexión privados

Las secuencias de métricas de Hystrix también son accesibles desde `test-endpoint`. Como servicio back-end, no asignamos un punto de conexión público para `recommendation-service`, pero podemos mostrar sus métricas con test-endpoint en `https://primary:<KEY>@<SERVICE-NAME>.test.azuremicroservices.io/recommendation-service/default/actuator/hystrix.stream`

![Secuencia test-endpoint de Hystrix](media/spring-cloud-circuit-breaker/hystrix-test-endpoint-stream.png)

Como aplicación web, el panel de Hystrix debe estar funcionando en `test-endpoint`. Si no funciona correctamente, puede haber dos motivos: en primer lugar, el uso de `test-endpoint` ha cambiado la dirección URL base de `/` a `/<APP-NAME>/<DEPLOYMENT-NAME>`, o, en segundo lugar, la aplicación web usa la ruta de acceso absoluta para el recurso estático. Para que funcione en `test-endpoint`, es posible que tenga que editar manualmente `<base>` en los archivos de front-end.

## <a name="next-steps"></a>Pasos siguientes

* [Aprovisionamiento de una instancia de servicio en la CLI de Azure](./quickstart.md#provision-an-instance-of-azure-spring-cloud)
* [Preparación de una aplicación Java Spring para su implementación en Azure Spring Cloud](how-to-prepare-app-deployment.md)
