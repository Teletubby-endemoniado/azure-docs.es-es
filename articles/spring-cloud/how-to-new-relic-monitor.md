---
title: Supervisión de aplicaciones de Spring Boot con el agente de Java de New Relic
titleSuffix: Azure Spring Cloud
description: Aprenda a supervisar las aplicaciones de Spring Boot con el agente de Java de New Relic.
author: karlerickson
ms.author: karler
ms.service: spring-cloud
ms.topic: how-to
ms.date: 04/07/2021
ms.custom: devx-track-java
ms.openlocfilehash: 740193a9526bf19efb0e98f937c6f77c30b50b61
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130258304"
---
# <a name="how-to-monitor-spring-boot-apps-using-new-relic-java-agent-preview"></a>Supervisión de aplicaciones de Spring Boot con el agente de Java de New Relic (versión preliminar)

Esta característica permite supervisar las aplicaciones de Spring Boot que se ejecutan en Azure Spring Cloud con el agente de Java de New Relic.

Con el agente de Java de New Relic, puede hacer lo siguiente:
* Consumir el agente de Java de New Relic.
* Configurar el agente de Java de New Relic mediante variables de entorno.
* Comprobar todos los datos de supervisión del panel de New Relic.

En el vídeo siguiente se describe cómo activar y supervisar aplicaciones de Spring Boot que se ejecutan en Azure Spring Cloud mediante New Relic One.

<br>

> [!VIDEO https://www.youtube.com/embed/4GQPwJSP3ys?list=PLPeZXlCR7ew8LlhnSH63KcM0XhMKxT1k_]

## <a name="prerequisites"></a>Prerrequisitos

* Una cuenta de [New Relic](https://newrelic.com/).
* [CLI de Azure, versión 2.0.67 o posterior](/cli/azure/install-azure-cli).

## <a name="activate-the-new-relic-java-in-process-agent"></a>Activación del agente de Java de New Relic en proceso

Use este procedimiento para acceder al agente:

1. Cree una instancia de Azure Spring Cloud.

2. Cree una aplicación.

    ```azurecli
      az spring-cloud app create --name "appName" --is-public true \
      -s "resourceName" -g "resourceGroupName"
    ```

3. Cree una implementación con el agente de New Relic y variables de entorno.

    ```azurecli
    az spring-cloud app deploy --name "appName" --jar-path app.jar \
       -s "resourceName" -g "resourceGroupName" \
       --jvm-options="-javaagent:/opt/agents/newrelic/java/newrelic-agent.jar" \
       --env NEW_RELIC_APP_NAME=appName NEW_RELIC_LICENSE_KEY=newRelicLicenseKey
    ```

Azure Spring Cloud instala previamente el agente de New Relic Java en *opt/agents/newrelic/java/newrelic-agent.jar\* . Los clientes pueden activar el agente en las **opciones de JVM** de las aplicaciones, así como configurar el agente mediante las [variables de entorno del agente de Java de New Relic](https://docs.newrelic.com/docs/agents/java-agent/configuration/java-agent-configuration-config-file/#Environment_Variables).

## <a name="portal"></a>Portal

También puede activar este agente desde el portal con el procedimiento siguiente.

1. Busque la aplicación en **Configuración**/**Aplicaciones** en el panel de navegación.

   [ ![Búsqueda de aplicación que se va a supervisar](media/new-relic-monitoring/find-app.png) ](media/new-relic-monitoring/find-app.png)

2. Seleccione la aplicación para ir a la página de **información general**.

   [ ![Página de información general](media/new-relic-monitoring/overview-page.png) ](media/new-relic-monitoring/overview-page.png)

3. Seleccione **Configuración** en el panel de navegación de la izquierda para agregar, actualizar o eliminar las **variables de entorno** de la aplicación.

   [ ![Actualización de entorno](media/new-relic-monitoring/configurations-update-environment.png) ](media/new-relic-monitoring/configurations-update-environment.png)

4. Haga clic en **Configuración general** para agregar, actualizar o eliminar las **opciones de JVM** de la aplicación.

   [ ![Actualización de la opción de JVM](media/new-relic-monitoring/update-jvm-option.png) ](media/new-relic-monitoring/update-jvm-option.png)

5. Consulte la página de **resumen** de la puerta de enlace o API de aplicación en el panel New Relic.

   [ ![Página de resumen de aplicación](media/new-relic-monitoring/app-summary-page.png) ](media/new-relic-monitoring/app-summary-page.png)

6. Vea la página de **resumen** del servicio a los clientes de la aplicación en el panel de New Relic.
 
   [ ![Página customers-service](media/new-relic-monitoring/customers-service.png) ](media/new-relic-monitoring/customers-service.png)  

7. Vea la página **Service Map** en el panel de New Relic.  

   [ ![Página Service Map](media/new-relic-monitoring/service-map.png) ](media/new-relic-monitoring/service-map.png)

8. Consulte la página de los **JVM** de la aplicación en el panel de New Relic.

   [ ![Página JVM](media/new-relic-monitoring/jvm-page.png) ](media/new-relic-monitoring/jvm-page.png)

9. Vea el perfil de la aplicación en el panel de New Relic.

   [ ![Perfil de aplicación](media/new-relic-monitoring/profile-app.png) ](media/new-relic-monitoring/profile-app.png)

## <a name="automate-provisioning"></a>Aprovisionamiento automatizado

También puede ejecutar una canalización de automatización de aprovisionamiento mediante Terraform o una plantilla de Azure Resource Manager (plantilla de ARM). Esta canalización puede proporcionar una experiencia práctica completa para instrumentar y supervisar las nuevas aplicaciones que cree e implemente.

### <a name="automate-provisioning-using-terraform"></a>Aprovisionamiento automatizado mediante Terraform

Para configurar las variables de entorno en una plantilla de Terraform, agregue el código que se muestra a continuación a la plantilla y reemplace los marcadores de posición *\<...>* por sus propios valores. Para obtener más información, consulte el tema sobre la [administración de una implementación activa de Azure Spring Cloud](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/spring_cloud_active_deployment).

```terraform
resource "azurerm_spring_cloud_java_deployment" "example" {
  ...
  jvm_options = "-javaagent:/opt/agents/newrelic/java/newrelic-agent.jar"
  ...
    environment_variables = {
      "NEW_RELIC_APP_NAME": "<app-name>",
      "NEW_RELIC_LICENSE_KEY": "<new-relic-license-key>"
  }
}
```

### <a name="automate-provisioning-using-an-arm-template"></a>Aprovisionamiento automatizado mediante una plantilla de ARM

Para configurar las variables de entorno en una plantilla de ARM, agregue el código a la plantilla que se muestra a continuación y reemplace los marcadores de posición *\<...>* por sus propios valores. Para obtener más información, consulte [Microsoft.AppPlatform Spring/apps/deployments](/azure/templates/microsoft.appplatform/spring/apps/deployments?tabs=json).

```ARM template
"deploymentSettings": {
  "environmentVariables": {
    "NEW_RELIC_APP_NAME" : "<app-name>",
    "NEW_RELIC_LICENSE_KEY" : "<new-relic-license-key>"
  },
  "jvmOptions": "-javaagent:/opt/agents/newrelic/java/newrelic-agent.jar",
  ...
}
```

## <a name="view-new-relic-java-agent-logs"></a>Visualización de los registros del agente de Java de New Relic

De manera predeterminada, Azure Spring Cloud imprimirá los registros del agente de Java de New Relic en `STDOUT`. Los registros se mezclarán con los registros de aplicaciones. Puede encontrar la versión explícita del agente en los registros de aplicaciones.

También puede obtener los registros del agente de New Relic desde las ubicaciones siguientes:

* Registros de Azure Spring Cloud
* Azure Spring Cloud Application Insights
* LogStream de Azure Spring Cloud

Puede aprovechar algunas de las variables de entorno que proporciona New Relic para configurar el registro del agente nuevo, como `NEW_RELIC_LOG_LEVEL` para controlar el nivel de los registros. Para más información, consulte las [variables de entorno de New Relic](https://docs.newrelic.com/docs/agents/java-agent/configuration/java-agent-configuration-config-file/#Environment_Variables).

> [!CAUTION]
> Se recomienda encarecidamente que *no* invalide el comportamiento de registro personalizado que proporciona Azure Spring Cloud para New Relic. Si lo hace, se bloquearán los escenarios de registro de los escenarios anteriores y es posible que se pierda el archivo de registro. Por ejemplo, no debe pasar estas variables de entorno a las aplicaciones. Es posible que los archivos de registro se pierdan después de reiniciar o volver a implementar las aplicaciones.
>
> * NEW_RELIC_LOG
> * NEW_RELIC_LOG_FILE_PATH

## <a name="new-relic-java-agent-updateupgrade"></a>Actualización del agente de Java de New Relic

El agente de Java de New Relic actualizará el JDK de manera periódica. La actualización del agente puede afectar los escenarios siguientes.

* Las aplicaciones existentes que utilizan el agente de Java de New Relic antes de la actualización no se modificarán.
* Es necesario reiniciar o volver a implementar las aplicaciones existentes que utilizan el agente de Java de New Relic antes de la actualización a fin de interactuar con la versión nueva del agente de Java de New Relic.
* Las aplicaciones nuevas que se creen después de la actualización usarán la versión nueva del agente de Java de New Relic.

## <a name="vnet-injection-instance-outbound-traffic-configuration"></a>Configuración del tráfico de salida de la instancia de inyección de red virtual

En el caso de una instancia de inyección de red virtual de Azure Spring Cloud, debe asegurarse de que el tráfico de salida esté configurado correctamente para el agente de Java de New Relic. Para más información, consulte [Redes de New Relic](https://docs.newrelic.com/docs/using-new-relic/cross-product-functions/install-configure/networks/#agents).

## <a name="next-steps"></a>Pasos siguientes

* [Seguimiento distribuido y Application Insights](how-to-distributed-tracing.md)
