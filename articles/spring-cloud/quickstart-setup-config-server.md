---
title: 'Inicio rápido: Configuración de Config Server de Azure Spring Cloud'
description: Se describe la configuración de Config Server de Azure Spring Cloud para la implementación de aplicaciones.
author: karlerickson
ms.author: karler
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/12/2021
ms.custom: devx-track-java, fasttrack-edit
zone_pivot_groups: programming-languages-spring-cloud
ms.openlocfilehash: f4b4eeaac6e32335937f42e1b35abe3929f6e972
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "130005268"
---
# <a name="quickstart-set-up-azure-spring-cloud-config-server"></a>Inicio rápido: Configuración del servidor de Config Server de Azure Spring Cloud

Config Server de Azure Spring Cloud es un servicio de configuración centralizado para sistemas distribuidos. Usa una capa de repositorio conectable que actualmente admite el almacenamiento local, Git y Subversion. En este inicio rápido, configurará Config Server para obtener datos de un repositorio de Git.

::: zone pivot="programming-language-csharp"

## <a name="prerequisites"></a>Requisitos previos

* Complete la guía de inicio rápido anterior de esta serie: [Aprovisionamiento del servicio Azure Spring Cloud](./quickstart-provision-service-instance.md).

## <a name="azure-spring-cloud-config-server-procedures"></a>Procedimientos de Config Server de Azure Spring Cloud

Ejecute el siguiente comando para configurar Config Server con la ubicación del repositorio de Git del proyecto. Reemplace *\<service instance name>* por el nombre del servicio que creó anteriormente. El valor predeterminado para el nombre de instancia de servicio que estableció en la guía de inicio rápido anterior no funciona con este comando.

```azurecli
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples --search-paths steeltoe-sample/config
```

Este comando indica a Config Server que busque los datos de configuración en la carpeta [steeltoe-sample/config](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/tree/master/steeltoe-sample/config) del repositorio de aplicaciones de ejemplo. Dado que el nombre de la aplicación que va a obtener los datos de configuración es `planet-weather-provider`, el archivo que se usará es [planet-weather-provider.yml](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples/blob/master/steeltoe-sample/config/planet-weather-provider.yml).

::: zone-end

::: zone pivot="programming-language-java"
Config Server de Azure Spring Cloud es un servicio de configuración centralizado para sistemas distribuidos. Usa una capa de repositorio conectable que actualmente admite el almacenamiento local, Git y Subversion.  Configure Config Server para implementar aplicaciones de microservicios en Azure Spring Cloud.

## <a name="prerequisites"></a>Requisitos previos

* [Instale JDK 8 u 11](/azure/developer/java/fundamentals/java-jdk-install).
* [Registro para obtener una suscripción a Azure](https://azure.microsoft.com/free/)
* (Opcional) [Instale la versión 2.0.67 de la CLI de Azure, o cualquier versión superior](/cli/azure/install-azure-cli), e instale la extensión de Azure Spring Cloud con el comando `az extension add --name spring-cloud`
* (Opcional) [Instale Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053-azure-toolkit-for-intellij/) e [inicie sesión](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in)

## <a name="azure-spring-cloud-config-server-procedures"></a>Procedimientos de Config Server de Azure Spring Cloud

#### <a name="portal"></a>[Portal](#tab/Azure-portal)

En el procedimiento siguiente se configura Config Server mediante Azure Portal para implementar el [ejemplo de PetClinic](https://github.com/azure-samples/spring-petclinic-microservices).

1. Vaya a la página **Información general** del servicio y seleccione **Config Server**.

2. En la sección **Default repository** (Repositorio predeterminado), en **URI** seleccione "https://github.com/azure-samples/spring-petclinic-microservices-config".

3. Seleccione **Validar**.

    ![Ir a Config Server](media/spring-cloud-quickstart-launch-app-portal/portal-config.png)

4. Cuando finalice la validación, seleccione **Aplicar** para guardar los cambios.

    ![Validación de Config Server](media/spring-cloud-quickstart-launch-app-portal/validate-complete.png)

5. La actualización de la configuración puede tardar unos minutos.

    ![Actualización de Config Server](media/spring-cloud-quickstart-launch-app-portal/updating-config.png)

6. Cuando se haya completado la configuración, debería recibir una notificación.

#### <a name="cli"></a>[CLI](#tab/Azure-CLI)

En el procedimiento siguiente se usa la CLI de Azure para configurar Config Server para implementar el [ejemplo de PetClinic](https://github.com/azure-samples/spring-petclinic-microservices).

Ejecute el siguiente comando para establecer el repositorio predeterminado.

```azurecli
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/azure-samples/spring-petclinic-microservices-config
```

::: zone-end

> [!TIP]
> Si usa un repositorio privado para Config Server, consulte el [tutorial sobre la configuración de la autenticación](./how-to-config-server.md).

## <a name="troubleshooting-of-azure-spring-cloud-config-server"></a>Solución de problemas de Config Server de Azure Spring Cloud

En el procedimiento siguiente se explica cómo solucionar los problemas de configuración de Config Server.

1. En Azure Portal, vaya a la página **Información general** del servicio y seleccione **Registros**.
1. Seleccione **Consultas** y **Mostrar los registros de aplicación que contienen los términos "error" o "excepción"** .
1. Seleccione **Run** (Ejecutar).
1. Si s encuentra el error **java. lang. illegalStateException** en los registros, esto indica que el servicio Spring Cloud no puede encontrar propiedades de Config Server.

    [ ![Ejecución de la consulta del portal de ASC](media/spring-cloud-quickstart-setup-config-server/setup-config-server-query.png) ](media/spring-cloud-quickstart-setup-config-server/setup-config-server-query.png)

1. Vaya a la página **Información general** del servicio.
1. Seleccione **Diagnóstico y solución de problemas**.
1. Seleccione la palabra clave **Config Server**.

    [ ![Problemas de diagnóstico del portal de ASC](media/spring-cloud-quickstart-setup-config-server/setup-config-server-diagnose.png) ](media/spring-cloud-quickstart-setup-config-server/setup-config-server-diagnose.png)

1. Seleccione **Comprobación de estado de Config Server**.

    [ ![Asistente del portal de ASC](media/spring-cloud-quickstart-setup-config-server/setup-config-server-genie.png) ](media/spring-cloud-quickstart-setup-config-server/setup-config-server-genie.png)

1. Seleccione **Config Server Status** (Estado de Config Server) para ver más detalles del detector.

    [ ![Estado de mantenimiento del portal de ASC](media/spring-cloud-quickstart-setup-config-server/setup-config-server-health-status.png) ](media/spring-cloud-quickstart-setup-config-server/setup-config-server-health-status.png)

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado recursos de Azure que seguirán acumulando cargos si permanecen en la suscripción. Si no tiene intención de pasar al siguiente inicio rápido, consulte [Limpieza de recursos](./quickstart-logs-metrics-tracing.md#clean-up-resources). De lo contrario, pase al siguiente inicio rápido:

> [!div class="nextstepaction"]
> [Compilación e implementación de aplicaciones](./quickstart-deploy-apps.md)
