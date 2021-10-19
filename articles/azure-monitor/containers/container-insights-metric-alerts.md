---
title: Alertas de métricas de Container Insights
description: En este artículo se revisan las alertas de métricas recomendadas disponibles en Container Insights en la versión preliminar pública.
ms.topic: conceptual
ms.date: 10/28/2020
ms.openlocfilehash: 7036bc7a0f161044312687d6b22171df99821e6a
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129714426"
---
# <a name="recommended-metric-alerts-preview-from-container-insights"></a>Alertas de métricas recomendadas (versión preliminar) de Container Insights

Para alertar sobre problemas de los recursos del sistema cuando estos experimentan un pico de demanda y utilizan prácticamente toda su capacidad, puede usar Container Insights para crear una alerta de registro en función de los datos de rendimiento almacenados en los registros de Azure Monitor. Ahora Container Insights incluye reglas de alerta de métricas preconfiguradas para el clúster de AKS y de Kubernetes habilitado para Azure Arc, que se encuentra en versión preliminar pública.

En este artículo se revisa la experiencia y se proporcionan instrucciones sobre la configuración y administración de estas reglas de alertas.

Si no está familiarizado con las alertas en Azure Monitor, consulte [Información general sobre las alertas en Microsoft Azure](../alerts/alerts-overview.md) antes de empezar. Para más información acerca de las alertas de métrica, consulte [Alertas de métricas en Azure Monitor](../alerts/alerts-metric-overview.md).

> [!NOTE]
> A partir del 8 de octubre de 2021, se actualizaron tres alertas para calcular correctamente la condición de alerta: **Porcentaje de la CPU del contenedor**, **Porcentaje de memoria del espacio de trabajo del contenedor** y **Porcentaje de uso del volumen persistente**. Estas nuevas alertas tienen los mismos nombres que las alertas correspondientes disponibles previamente, pero usan métricas nuevas y actualizadas. Se recomienda deshabilitar las alertas que usen las métricas "Antiguas", descritas en este artículo, y habilitar las métricas "Nuevas". Las métricas "Antiguas" ya no estarán disponibles en las alertas recomendadas una vez deshabilitadas, pero puede volver a habilitarlas manualmente.

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar, confirme lo siguiente:

* Las métricas personalizadas solo están disponibles en un subconjunto de regiones de Azure. Una lista de regiones admitidas se documenta en [Regiones admitidas](../essentials/metrics-custom-overview.md#supported-regions).

* Para admitir las alertas de métricas y la introducción de métricas adicionales, la versión mínima necesaria del agente es **mcr.microsoft.com/azuremonitor/containerinsights/ciprod:ciprod05262020** para el clúster de AKS y **mcr.microsoft.com/azuremonitor/containerinsights/ciprod:ciprod09252020** para el de Kubernetes habilitado para Azure Arc.

    Para comprobar que el clúster ejecuta la versión más reciente del agente, puede realizar alguna de las acciones siguientes:

    * Ejecute el comando: `kubectl describe <omsagent-pod-name> --namespace=kube-system`. En el estado devuelto, observe el valor de **Imagen** para omsagent en la sección *Contenedores* de la salida. 
    * En la pestaña **Nodos**, seleccione el nodo de clúster y, en el panel **Propiedades** de la derecha, observe el valor de **Etiqueta de imagen del agente**.

    El valor que se muestra para AKS debe ser la versión **ciprod05262020** o posterior. El valor que se muestra para el clúster de Kubernetes habilitado para Azure Arc debe ser la versión **ciprod09252020** o posterior. Si el clúster tiene una versión anterior, vea [Actualización del agente de Azure Monitor para contenedores](container-insights-manage-agent.md#upgrade-agent-on-aks-cluster) para conocer los pasos necesarios para obtener la versión más reciente.

    Para obtener más información relacionada con la versión del agente, consulte el [historial de versiones del agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). Para comprobar que se están recopilando métricas, puede usar el explorador de métricas de Azure Monitor y comprobar que, en **Espacio de nombres de métrica**, se muestra **Conclusiones**. Si es así, puede empezar a configurar las alertas. Si no ve ninguna métrica recopilada, significa que la entidad de servicio de clúster o MSI no tiene los permisos necesarios. Para comprobar que SPN o MSI es miembro del rol de **publicador de métricas de supervisión**, siga los pasos descritos en la sección [Actualización por clúster con la CLI de Azure](container-insights-update-metrics.md#upgrade-per-cluster-using-azure-cli) para confirmar y establecer la asignación de roles.

## <a name="alert-rules-overview"></a>Introducción a las reglas de alertas

Para enviar alertas sobre lo que importa, Container Insights incluye las siguientes alertas de métricas para los clústeres de AKS y de Kubernetes habilitado para Azure Arc:

|Nombre| Descripción |Umbral predeterminado |
|----|-------------|------------------|
|**(Nueva) Porcentaje medio de la CPU del contenedor** |Calcula el promedio de CPU usado por contenedor.|Cuando el uso medio de la CPU por contenedor es superior al 95 %.| 
|**(Nueva) Porcentaje medio de memoria del espacio de trabajo del contenedor** |Calcula el promedio de memoria del espacio de trabajo usado por contenedor.|Cuando el uso medio de memoria del espacio de trabajo por contenedor es superior al 95 %. |
|% uso medio de CPU |Calcula el promedio de CPU usado por nodo. |Cuando el uso medio de CPU de nodo es superior al 80 %. |
|Average Disk Usage % (% uso medio de disco) |Calcula el uso medio de disco de un nodo.|Cuando el uso de disco de un nodo es superior al 80 %. |
|**(Nueva) Porcentaje medio de uso de volumen persistente** |Calcula el uso medio de volumen persistente por pod. |Cuando el uso medio de volumen por pod es superior al 80 %.|
|Average Working set memory % (% medio de memoria del espacio de trabajo) |Calcula el promedio de memoria del espacio de trabajo de un nodo. |Cuando el promedio de memoria del espacio de trabajo de un nodo es superior al 80 %. |
|Restarting container count (Reiniciando recuento de contenedores) |Calcula el número de contenedores restantes. | Cuando los reinicios de contenedor son más de 0. |
|Recuentos de pod con errores |Calcula si algún pod tiene un estado de error.|Cuando el número de pods con estado de error es mayor que 0. |
|Node NotReady status (Estado de nodo NotReady) |Calcula si algún nodo está en estado NotReady.|Cuando el número de nodos con el estado NotReady es mayor que 0. |
|OOM Killed Containers (Contenedores eliminados con memoria insuficiente) |Calcula el número de contenedores eliminados con memoria insuficiente. |Cuando el número de contenedores eliminados con memoria insuficiente es mayor que 0. |
|Pods ready % (% de pods listos) |Calcula el promedio de estados listos de los pods. |Cuando el estado listo de los pods es inferior al 80 %.|
|Completed job count (Recuento de trabajos completados) |Calcula el número de trabajos completados hace más de seis horas. |Cuando el número de trabajos obsoletos de más de seis horas es mayor que 0.|

Existen propiedades comunes en todas estas reglas de alertas:

* Todas las reglas de alertas se basan en métricas.

* Todas las reglas de alertas están deshabilitadas de manera predeterminada.

* Todas las reglas de alerta se evalúan una vez por minuto y examinan los últimos 5 minutos de datos.

* Las reglas de alertas no tienen un grupo de acciones asignado de forma predeterminada. Puede agregar un [grupo de acciones](../alerts/action-groups.md) a la alerta, ya sea mediante la selección de un grupo de acciones existente o la creación de uno nuevo al editar la regla de alertas.

* Para modificar el umbral de las reglas de alertas puede editarlas directamente. Sin embargo, consulte las instrucciones que proporciona cada regla de alertas antes de modificar su umbral.

Las siguientes métricas basadas en alertas tienen características de comportamiento únicas en comparación con las otras métricas:

* La métrica *completedJobsCount* solo se envía cuando hay trabajos que se han completado hace más de seis horas.

* La métrica *containerRestartCount* solo se envía cuando se reinician los contenedores.

* La métrica *oomKilledContainerCount* solo se envía cuando hay contenedores de eliminados con memoria insuficiente.

* Las métricas *cpuExceededPercentage*, *memoryRssExceededPercentage* y *memoryWorkingSetExceededPercentage* se envían cuando los valores de CPU, memoria RSS y espacio de trabajo de memoria superan el umbral configurado (el umbral predeterminado es 95 %). Estos umbrales son exclusivos del umbral de condición de alerta especificado para la regla de alertas correspondiente. Es decir, si quiere recopilar estas métricas y analizarlas desde el [Explorador de métricas](../essentials/metrics-getting-started.md), se recomienda que configure el umbral en un valor inferior al umbral de alerta. La configuración relacionada con la configuración de recopilación para los umbrales de uso de recursos de contenedor se puede invalidar en el archivo ConfigMaps en la sección `[alertable_metrics_configuration_settings.container_resource_utilization_thresholds]`. Vea la sección [Configuración de métricas que generan alertas en ConfigMaps](#configure-alertable-metrics-in-configmaps) para obtener más información relacionada con la configuración del archivo de configuración ConfigMaps.

* La métrica *pvUsageExceededPercentage* se envía cuando el porcentaje de uso del volumen persistente supera el umbral configurado (el umbral predeterminado es 60 %). Este umbral es exclusivo del umbral de condición de alerta especificado para la regla de alertas correspondiente. Es decir, si quiere recopilar estas métricas y analizarlas desde el [Explorador de métricas](../essentials/metrics-getting-started.md), se recomienda que configure el umbral en un valor inferior al umbral de alerta. La configuración relacionada con la configuración de recopilación para los umbrales de uso de volúmenes persistentes se puede invalidar en el archivo ConfigMaps, en la sección `[alertable_metrics_configuration_settings.pv_utilization_thresholds]`. Vea la sección [Configuración de métricas que generan alertas en ConfigMaps](#configure-alertable-metrics-in-configmaps) para obtener más información relacionada con la configuración del archivo de configuración ConfigMaps. La colección de métricas de volumen persistentes con notificaciones en el espacio de nombres *kube-system* se excluye de forma predeterminada. Para habilitar la recopilación en este espacio de nombres, use la sección `[metric_collection_settings.collect_kube_system_pv_metrics]` en el archivo ConfigMap. Consulte [Configuración de la colección de métricas](./container-insights-agent-config.md#metric-collection-settings) para obtener más información.

## <a name="metrics-collected"></a>Métricas recopiladas

Las siguientes métricas se habilitan y recopilan, a menos que se especifique lo contrario, como parte de esta característica. Las métricas en **negrita** con la etiqueta "Antigua" son las que reemplazan las métricas con la etiqueta "Nueva" recopiladas para la evaluación de alertas correcta.

|Espacio de nombres de métricas |Métrica |Descripción |
|---------|----|------------|
|Insights.container/nodes |cpuUsageMillicores |Uso de CPU en milinúcleos por host.|
|Insights.container/nodes |cpuUsagePercentage |Porcentaje de uso de CPU por nodo.|
|Insights.container/nodes |memoryRssBytes |Uso de RSS de memoria en bytes por host.|
|Insights.container/nodes |memoryRssPercentage |Porcentaje de uso de RSS de memoria por host.|
|Insights.container/nodes |memoryWorkingSetBytes |Uso del espacio de trabajo de memoria en bytes por host.|
|Insights.container/nodes |memoryWorkingSetPercentage |Porcentaje de uso del espacio de trabajo de memoria por host.|
|Insights.container/nodes |nodesCount |Recuento de nodos por estado.|
|Insights.container/nodes |diskUsedPercentage |Porcentaje de disco usado en el nodo por dispositivo.|
|Insights.container/pods |podCount |Recuento de pods por controlador, espacio de nombres, nodo y fase.|
|Insights.container/pods |completedJobsCount |Umbral configurable por el usuario anterior al recuento de trabajos completados (el valor predeterminado es de seis horas) por controlador, espacio de nombres de Kubernetes. |
|Insights.container/pods |restartingContainerCount |El recuento de contenedores se reinicia por controlador, espacio de nombres Kubernetes.|
|Insights.container/pods |oomKilledContainerCount |Recuento de contenedores OOMkilled por controlador, espacio de nombres Kubernetes.|
|Insights.container/pods |podReadyPercentage |Porcentaje de pods en estado listo por controlador, espacio de nombres Kubernetes.|
|Insights.container/containers |**(Old)cpuExceededPercentage** |Porcentaje de uso de CPU para contenedores que superan el umbral configurable por el usuario (el valor predeterminado es 95,0) por nombre de contenedor, nombre de controlador, espacio de nombres de Kubernetes, nombre del pod.<br> Recopilados  |
|Insights.container/containers |**(New)cpuThresholdViolated** |Métrica desencadenada cuando el porcentaje de uso de CPU para contenedores supera el umbral configurable por el usuario (el valor predeterminado es 95,0) por nombre de contenedor, nombre de controlador, espacio de nombres de Kubernetes, nombre del pod.<br> Recopilados  |
|Insights.container/containers |**(Antigua)memoryRssExceededPercentage** |Porcentaje de RSS de memoria para contenedores que superan el umbral configurable por el usuario (el valor predeterminado es 95,0) por nombre de contenedor, nombre de controlador, espacio de nombres de Kubernetes, nombre del Pod.|
|Insights.container/containers |**(Nueva)memoryRssThresholdViolated** |Métrica desencadenada cuando el porcentaje de RSS de memoria para contenedores supera el umbral configurable por el usuario (el valor predeterminado es 95,0) por nombre de contenedor, nombre de controlador, espacio de nombres de Kubernetes, nombre del pod.|
|Insights.container/containers |**(Antigua)memoryWorkingSetExceededPercentage** |Porcentaje de espacio de trabajo de memoria para contenedores que superan el umbral configurable por el usuario (el valor predeterminado es 95,0) por nombre de contenedor, nombre de controlador, espacio de nombres Kubernetes, nombre del pod.|
|Insights.container/containers |**(Nueva)memoryWorkingSetThresholdViolated** |Métrica desencadenada cuando el porcentaje de espacio de trabajo de memoria para contenedores supera el umbral configurable por el usuario (el valor predeterminado es 95,0) por nombre de contenedor, nombre de controlador, espacio de nombres de Kubernetes, nombre del pod.|
|Insights.container/persistentvolumes |**(Antigua)pvUsageExceededPercentage** |Porcentaje de uso de volumen persistente para volúmenes persistentes que superan el umbral configurable por el usuario (el valor predeterminado es 60,0) por nombre de notificación, espacio de nombres de Kubernetes, nombre de volumen, nombre de pod y nombre de nodo.|
|Insights.container/persistentvolumes |**(Nueva)pvUsageThresholdViolated** |Métrica desencadenada cuando el porcentaje de uso de volumen persistente para volúmenes persistentes supera el umbral configurable por el usuario (el valor predeterminado es 60,0) por nombre de notificación, espacio de nombres de Kubernetes, nombre de volumen, nombre de pod y nombre de nodo.

## <a name="enable-alert-rules"></a>Habilitar reglas de alertas

Siga estos pasos para habilitar las alertas de métricas en Azure Monitor desde Azure Portal. Para habilitar el uso de una plantilla de Resource Manager, consulte [Habilitación con una plantilla de Resource Manager](#enable-with-a-resource-manager-template).

### <a name="from-the-azure-portal"></a>Desde Azure Portal

En esta sección se explica cómo habilitar la alerta de métricas de Container Insights (versión preliminar) desde Azure Portal.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. El acceso a la característica Alerta de métricas de Container Insights (versión preliminar) está disponible directamente desde un clúster de AKS al seleccionar la opción **Información** en el panel izquierdo de Azure Portal.

3. En la barra de comandos, seleccione **Alertas recomendadas**.

    ![Opción de alertas recomendadas en Container Insights](./media/container-insights-metric-alerts/command-bar-recommended-alerts.png)

4. El panel de propiedades de **Alertas recomendadas** se muestra automáticamente en el lado derecho de la página. De forma predeterminada, todas las reglas de alertas de la lista están deshabilitadas. Después de seleccionar **Habilitar**, se crea la regla de alertas y el nombre de la regla se actualiza para incluir un vínculo al recurso de alerta.

    ![Panel de propiedades de Alertas recomendadas](./media/container-insights-metric-alerts/recommended-alerts-pane.png)

    Después de seleccionar el botón de alternancia **Habilitar/Deshabilitar** para habilitar la alerta, se crea una regla de alerta y el nombre de la regla se actualiza para incluir un vínculo al recurso de alerta real.

    ![Habilitar regla de alertas](./media/container-insights-metric-alerts/recommended-alerts-pane-enable.png)

5. Las alertas no están asociadas a un [grupo de acciones](../alerts/action-groups.md) para notificar a los usuarios que se ha desencadenado una alerta. Seleccione **No se asignó ningún grupo de acciones** y, en la página **Grupos de acciones**, especifique un grupo de acciones o especifique uno existente mediante **Agregar** o **Crear**.

    ![Seleccionar un grupo de acciones](./media/container-insights-metric-alerts/select-action-group.png)

### <a name="enable-with-a-resource-manager-template"></a>Habilitación con una plantilla de Resource Manager

Puede utilizar una plantilla de Azure Resource Manager y un archivo de parámetros para crear las alertas de métricas incluidas en Azure Monitor.

Los pasos básicos son los siguientes:

1. Descargue todas o alguna de las plantillas disponibles que describen cómo crear la alerta desde [GitHub](https://github.com/microsoft/Docker-Provider/tree/ci_dev/alerts/recommended_alerts_ARM).

2. Cree y use un [archivo de parámetros](../../azure-resource-manager/templates/parameter-files.md) como JSON para establecer los valores necesarios para crear la regla de alertas.

3. Implemente la plantilla desde Azure Portal, PowerShell o la CLI de Azure.

#### <a name="deploy-through-azure-portal"></a>Implementación a través de Azure Portal

1. Descargue y guarde la plantilla de Azure Resource Manager y el archivo de parámetros en una carpeta local para crear la regla de alertas con los siguientes comandos:

2. Para implementar una plantilla personalizada mediante el portal, seleccione **Crear un recurso** en [Azure Portal](https://portal.azure.com).

3. Busque **plantilla** y, después, seleccione **Template Deployment**.

4. Seleccione **Crear**.

5. Verá varias opciones para crear una plantilla; seleccione **Cree su propia plantilla en el editor**.

6. En **Editar plantilla de página**, seleccione **Cargar archivo** y, a continuación, seleccione el archivo de plantilla.

7. En la página **Editar plantilla**, seleccione **Guardar**.

8. En la página **Implementación personalizada**, especifique lo siguiente y, después, cuando finalice, seleccione **Comprar** para implementar la plantilla y crear la regla de alertas.

    * Resource group
    * Location
    * Nombre de la alerta
    * Identificador de recurso del clúster

#### <a name="deploy-with-azure-powershell-or-cli"></a>Implementación con Azure PowerShell o la CLI de Azure

1. Descargue y guarde la plantilla de Azure Resource Manager y el archivo de parámetros en una carpeta local para crear la regla de alertas con los siguientes comandos:

2. Puede crear la alerta de métrica con la plantilla y el archivo de parámetros mediante PowerShell o la CLI de Azure.

    Uso de Azure PowerShell

    ```powershell
    Connect-AzAccount

    Select-AzSubscription -SubscriptionName <yourSubscriptionName>
    New-AzResourceGroupDeployment -Name CIMetricAlertDeployment -ResourceGroupName ResourceGroupofTargetResource `
    -TemplateFile templateFilename.json -TemplateParameterFile templateParameterFilename.parameters.json
    ```

    Uso de la CLI de Azure

    ```azurecli
    az login

    az deployment group create \
    --name AlertDeployment \
    --resource-group ResourceGroupofTargetResource \
    --template-file templateFileName.json \
    --parameters @templateParameterFilename.parameters.json
    ```

    >[!NOTE]
    >Aunque la alerta de métrica se puede crear en un grupo de recursos distinto al recurso de destino, se recomienda usar el mismo grupo de recursos que el recurso de destino.

## <a name="edit-alert-rules"></a>Edición de reglas de alertas

Puede ver y administrar reglas de alertas de Container Insights, editar su umbral o configurar un [grupo de acciones](../alerts/action-groups.md) para el clúster de AKS. Aunque puede realizar estas acciones desde Azure Portal y la CLI de Azure, también lo puede hacer directamente desde el clúster de AKS en Container Insights.

1. En la barra de comandos, seleccione **Alertas recomendadas**.

2. Para modificar el umbral, en el panel **Alertas recomendadas**, seleccione la alerta habilitada. En **Editar regla**, seleccione la opción de **Criterios de alerta** que quiera editar.

    * Para modificar el umbral de la regla de alertas, seleccione la opción de **Condición**.
    * Para crear un grupo de acciones o especificar uno existente, seleccione **Agregar** o **Crear** en **Grupo de acciones**.

Para ver las alertas creadas para las reglas habilitadas, en el panel **Alertas recomendadas**, seleccione **Ver en las alertas**. Se le redirigirá al menú de alertas del clúster de AKS, donde podrá ver todas las alertas creadas actualmente para el clúster.

## <a name="configure-alertable-metrics-in-configmaps"></a>Configuración de métricas que generan alertas en ConfigMaps

Realice los pasos siguientes para configurar el archivo de configuración ConfigMap para invalidar los umbrales de uso predeterminados. Estos pasos solo se aplican a las siguientes métricas que generan alertas:

* *cpuExceededPercentage*
* *memoryRssExceededPercentage*
* *memoryWorkingSetExceededPercentage*
* *pvUsageExceededPercentage*

1. Edite el archivo YAML de ConfigMap en la sección `[alertable_metrics_configuration_settings.container_resource_utilization_thresholds]` o `[alertable_metrics_configuration_settings.pv_utilization_thresholds]`.

   - Para modificar el umbral de *cpuExceededPercentage* al 90 % y comenzar la recopilación de esta métrica cuando se alcance y se supere ese umbral, configure el archivo ConfigMap mediante el ejemplo siguiente:

     ```
     [alertable_metrics_configuration_settings.container_resource_utilization_thresholds]
         # Threshold for container cpu, metric will be sent only when cpu utilization exceeds or becomes equal to the following percentage
         container_cpu_threshold_percentage = 90.0
         # Threshold for container memoryRss, metric will be sent only when memory rss exceeds or becomes equal to the following percentage
         container_memory_rss_threshold_percentage = 95.0
         # Threshold for container memoryWorkingSet, metric will be sent only when memory working set exceeds or becomes equal to the following percentage
         container_memory_working_set_threshold_percentage = 95.0
     ```

   - Para modificar el umbral de *pvUsageExceededPercentage* al 80 % y comenzar la recopilación de esta métrica cuando se alcance y se supere ese umbral, configure el archivo ConfigMap mediante el ejemplo siguiente:

     ```
     [alertable_metrics_configuration_settings.pv_utilization_thresholds]
         # Threshold for persistent volume usage bytes, metric will be sent only when persistent volume utilization exceeds or becomes equal to the following percentage
         pv_usage_threshold_percentage = 80.0
     ```

2. Ejecute el siguiente comando de kubectl: `kubectl apply -f <configmap_yaml_file.yaml>`.

    Ejemplo: `kubectl apply -f container-azm-ms-agentconfig.yaml`.

El cambio de configuración puede tardar unos minutos en finalizar antes de que surta efecto, y todos los pods de omsagent del clúster se reiniciarán. El reinicio es un reinicio gradual para todos los pods de omsagent; no todos se reinician al mismo tiempo. Cuando los reinicios finalizan, se muestra un mensaje similar al siguiente ejemplo e incluye el resultado: `configmap "container-azm-ms-agentconfig" created`.

## <a name="next-steps"></a>Pasos siguientes

- En los [ejemplos de consultas de registro](container-insights-log-query.md) encontrará consultas predefinidas y ejemplos para evaluar o personalizar las alertas, la visualización o el análisis de los clústeres.

- Para más información sobre Azure Monitor y cómo supervisar otros aspectos del clúster de Kubernetes, consulte [Visualización del rendimiento del clúster de Kubernetes](container-insights-analyze.md).
