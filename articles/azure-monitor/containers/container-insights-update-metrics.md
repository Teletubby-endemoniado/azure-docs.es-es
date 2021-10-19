---
title: Procedimiento para actualizar Container Insights para las métricas | Microsoft Docs
description: En este artículo se describe cómo actualizar Container Insights para habilitar la característica de métricas personalizadas, que permite explorar métricas y recibir alertas de métricas agregadas.
ms.topic: conceptual
ms.date: 10/09/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 0d48ae48c667422b68c39570eb0003ff2e648267
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129706872"
---
# <a name="how-to-update-container-insights-to-enable-metrics"></a>Procedimiento para actualizar Container Insights para habilitar métricas

Container Insights presenta compatibilidad para recopilar métricas de pods y nodos de clústeres de Kubernetes habilitados para Azure Arc y Azure Kubernetes Services (AKS), así como para escribirlos en el almacén de métricas de Azure Monitor. La finalidad de este cambio es presentar los cálculos de agregado (Avg, Count, Max, Min, Sum) de mejor forma y más oportuna en los gráficos de rendimiento, así como ofrecer compatibilidad con el anclaje de gráficos de rendimiento en los paneles de Azure Portal y compatibilidad con alertas de métricas.

>[!NOTE]
>Esta característica no admite actualmente clústeres de Red Hat OpenShift en Azure.
>

Las siguientes métricas están habilitadas como parte de esta característica:

| Espacio de nombres de métricas | Métrica | Descripción |
|------------------|--------|-------------|
| Insights.container/nodes | cpuUsageMillicores, cpuUsagePercentage, memoryRssBytes, memoryRssPercentage, memoryWorkingSetBytes, memoryWorkingSetPercentage, **cpuUsageAllocatablePercentage**, **memoryWorkingSetAllocatablePercentage**, **memoryRssAllocatablePercentage**, nodesCount, diskUsedPercentage, | Como métricas de *nodo* incluyen *host* como dimensión. También incluyen el<br> nombre del nodo como valor de la dimensión *host*. |
| Insights.container/pods | podCount, completedJobsCount, restartingContainerCount, oomKilledContainerCount, podReadyPercentage | Como métricas de *pod*, incluyen las siguientes dimensiones: ControllerName, espacio de nombres de Kubernetes, nombre y fase. |
| Insights.container/containers | cpuExceededPercentage, memoryRssExceededPercentage, memoryWorkingSetExceededPercentage, **cpuThresholdViolated**, **memoryRssThresholdViolated**, **memoryWorkingSetThresholdViolated** | |
| Insights.container/persistentvolumes | pvUsageExceededPercentage, **pvUsageThresholdViolated** | |

Para admitir estas nuevas capacidades, en la versión se incluye un nuevo agente en contenedores, versión **microsoft/oms:ciprod05262020** para AKS y versión **microsoft/oms:ciprod09252020** para clústeres de Kubernetes habilitados para Azure Arc. Las nuevas implementaciones de AKS incluyen este cambio de configuración y estas funcionalidades de forma automática. La actualización del clúster para admitir esta funcionalidad se puede realizar desde Azure Portal o Azure PowerShell, o con la CLI de Azure. Con Azure PowerShell y la CLI. Se puede habilitar en cada clúster o en todos los clústeres de la suscripción.

Cada proceso asigna el rol **Publicador de métricas de supervisión** al MSI asignado del usuario o entidad de servicio del clúster para el complemento de supervisión de modo que los datos recopilados por el agente se puedan publicar en el recurso de los clústeres. El rol Publicador de métricas de supervisión tiene permiso únicamente para insertar métricas en el recurso; no puede modificar ningún estado, actualizar el recurso ni leer datos. Para más información sobre este rol, vea el [rol del publicador de métricas de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-metrics-publisher). El requisito de rol Publicador de métricas de supervisión no es aplicable a clústeres de Kubernetes habilitados para Azure Arc.

> [!IMPORTANT]
> La actualización no es necesaria para clústeres de Kubernetes habilitados para Azure Arc, ya que estos ya tienen la versión mínima necesaria del agente. La asignación del rol **Publicador de métricas de supervisión** a la entidad de servicio del clúster o a la MSI asignada por el usuario para el complemento de supervisión se realiza automáticamente al usar Azure Portal, Azure PowerShell o CLI de Azure.

## <a name="prerequisites"></a>Requisitos previos

Antes de actualizar el clúster, confirme lo siguiente:

* Las métricas personalizadas solo están disponibles en un subconjunto de regiones de Azure. [Aquí](../essentials/metrics-custom-overview.md#supported-regions) se documenta una lista de regiones admitidas.

* Es miembro del rol **[Propietario](../../role-based-access-control/built-in-roles.md#owner)** en el recurso de clúster de AKS para habilitar la recopilación de métricas de rendimiento personalizadas de nodos y pods. Este requisito no se aplica a clústeres de Kubernetes habilitados para Azure Arc.

Si decide usar la CLI de Azure, primero debe instalar y usar la CLI localmente. Debe ejecutar la versión 2.0.59 de la CLI de Azure, o cualquier versión posterior. Para identificar la versión, ejecute `az --version`. Si necesita instalar o actualizar la CLI de Azure, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="upgrade-a-cluster-from-the-azure-portal"></a>Actualizar un clúster desde Azure Portal

En el caso de los clústeres de AKS existentes supervisados por Container Insights, tras seleccionar el clúster para ver su estado en la vista de varios clústeres de Azure Monitor (o bien directamente desde el clúster si se selecciona **Información** en el panel de la izquierda) verá un banner en la parte superior del portal.

![Actualizar banner de clúster de AKS en Azure Portal](./media/container-insights-update-metrics/portal-banner-enable-01.png)

Si se hace clic en **Habilitar**, iniciará el proceso para actualizar el clúster. Este proceso puede tardar varios segundos en finalizar, y se puede realizar un seguimiento del progreso en Notificaciones en el menú.

## <a name="upgrade-all-clusters-using-bash-in-azure-command-shell"></a>Actualizar todos los clústeres con Bash en el Shell de comandos de Azure

Haga lo siguiente para actualizar todos los clústeres de la suscripción con Bash en el Shell de comandos de Azure.

1. Ejecute el siguiente comando mediante la CLI de Azure.  Edite el valor de **subscriptionId** utilizando el valor de la página **AKS Overview** (Introducción a AKS) del clúster de AKS.

    ```azurecli
    az login
    az account set --subscription "Subscription Name"
    curl -sL https://aka.ms/ci-md-onboard-atscale | bash -s subscriptionId   
    ```

    El cambio de configuración puede tardar unos segundos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

    ```azurecli
    completed role assignments for all AKS clusters in subscription: <subscriptionId>
    ```

## <a name="upgrade-per-cluster-using-azure-cli"></a>Actualizar cada clúster mediante la CLI de Azure

Haga lo siguiente para actualizar un clúster concreto de la suscripción usando la CLI de Azure.

1. Ejecute el siguiente comando mediante la CLI de Azure. Edite los valores de **subscriptionId**, **resourceGroupName** y **clusterName** utilizando los valores de la página **AKS Overview** (Introducción a AKS) del clúster de AKS.  Para obtener el valor de **clientIdOfSPN**, se devuelve al ejecutar el comando `az aks show`, como se muestra en el siguiente ejemplo.


    ```azurecli
    az login
    az account set --subscription "<subscriptionName>"
    az aks show -g <resourceGroupName> -n <clusterName> 
    az role assignment create --assignee <clientIdOfSPN> --scope <clusterResourceId> --role "Monitoring Metrics Publisher" 
    ```


    Para obtener el valor de **clientIdOfSPNOrMsi**, se puede ejecutar el comando `az aks show`, como se muestra en el siguiente ejemplo. Si el objeto **servicePrincipalProfile** tiene un valor de *clientid* válido, puede usarlo. De lo contrario, si se establece en *msi*, deberá pasar el clientid desde `addonProfiles.omsagent.identity.clientId`.


    ```azurecli
    az login
    az account set --subscription "<subscriptionName>"
    az aks show -g <resourceGroupName> -n <clusterName> 
    az role assignment create --assignee <clientIdOfSPNOrMsi> --scope <clusterResourceId> --role "Monitoring Metrics Publisher"
    ```



>[!NOTE]
>Si usa su cuenta de usuario y quiere realizar la asignación de roles, use el parámetro --assignee tal como se muestra en el ejemplo siguiente. De lo contrario, si inicia sesión con SPN y quiere realizar la asignación de roles, use los parámetros --assignee-object-id --assignee-principal-type en lugar del parámetro --assignee.

## <a name="upgrade-all-clusters-using-azure-powershell"></a>Actualizar todos los clústeres con Azure PowerShell

Haga lo siguiente para actualizar todos los clústeres de la suscripción con Azure PowerShell.

1. [Descargue](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/docs/aks/mdmonboarding/mdm_onboarding_atscale.ps1) el script **mdm_onboarding_atscale.ps1** de nuestro repositorio de GitHub y guárdelo en una carpeta local.
2. Ejecute el siguiente comando mediante Azure PowerShell.  Edite el valor de **subscriptionId** utilizando el valor de la página **AKS Overview** (Introducción a AKS) del clúster de AKS.

    ```powershell
    .\mdm_onboarding_atscale.ps1 subscriptionId
    ```
    El cambio de configuración puede tardar unos segundos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

    ```powershell
    Completed adding role assignment for the aks clusters in subscriptionId :<subscriptionId>
    ```

## <a name="upgrade-per-cluster-using-azure-powershell"></a>Actualizar cada clúster mediante Azure PowerShell

Haga lo siguiente para actualizar un clúster concreto con mediante Azure PowerShell.

1. [Descargue](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/docs/aks/mdmonboarding/mdm_onboarding.ps1) el script **mdm_onboarding.ps1** de nuestro repositorio de GitHub y guárdelo en una carpeta local.

2. Ejecute el siguiente comando mediante Azure PowerShell. Edite los valores de **subscriptionId**, **resourceGroupName** y **clusterName** utilizando los valores de la página **AKS Overview** (Introducción a AKS) del clúster de AKS.

    ```powershell
    .\mdm_onboarding.ps1 subscriptionId <subscriptionId> resourceGroupName <resourceGroupName> clusterName <clusterName>
    ```

    El cambio de configuración puede tardar unos segundos en completarse. Cuando se completa, se muestra un mensaje que incluye el resultado y que es similar al siguiente:

    ```powershell
    Successfully added Monitoring Metrics Publisher role assignment to cluster : <clusterName>
    ```

## <a name="verify-update"></a>Comprobar la actualización

Después de iniciar la actualización a través de uno de los métodos descritos anteriormente, puede usar el Explorador de métricas de Azure Monitor y comprobar en el **Espacio de nombres de métrica** si aparece **insights**. Si es así, puede continuar y empezar a configurar [alertas de métricas](../alerts/alerts-metric.md) o a anclar gráficos a [paneles](../../azure-portal/azure-portal-dashboards.md).  
