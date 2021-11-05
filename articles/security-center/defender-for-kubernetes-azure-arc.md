---
title: Protección de implementaciones de Kubernetes híbridas y de varias nubes con Microsoft Defender para Kubernetes
description: Uso de Microsoft Defender para Kubernetes con los clústeres de Kubernetes locales y de varias nubes
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 11/02/2021
ms.author: memildin
ms.custom: ignite-fall-2021
ms.openlocfilehash: c3d328732f35a35c663f04c7bfed9209adbcb7f7
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131009936"
---
# <a name="defend-azure-arc-enabled-kubernetes-clusters-running-in-on-premises-and-multi-cloud-environments"></a>Protección de clústeres de Kubernetes habilitados para Azure Arc que se ejecutan en entornos locales y de varias nubes

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

La **extensión de clústeres de Microsoft Defender para Kubernetes** puede proteger los clústeres locales con las mismas funcionalidades de detección de amenazas que se ofrecen para los clústeres de Azure Kubernetes Service. Habilite [Kubernetes habilitado para Azure Arc](../azure-arc/kubernetes/overview.md) en los clústeres e implemente la extensión como se describe en esta página. 

La extensión puede proteger también los clústeres de Kubernetes en otros proveedores de nube, aunque no los que están en los servicios de Kubernetes administrado.

> [!TIP]
> Hemos preparado algunos archivos de ejemplo para ayudarle con el proceso de instalación en los [ejemplos de instalación de GitHub](https://aka.ms/kubernetes-extension-installation-examples).

## <a name="availability"></a>Disponibilidad

| Aspecto | Detalles |
|--------|---------|
| Estado de la versión | **Versión preliminar**<br>[!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)]|
| Roles y permisos necesarios | El [administrador de seguridad](../role-based-access-control/built-in-roles.md#security-admin) puede descartar las alertas.<br>El [Lector de seguridad](../role-based-access-control/built-in-roles.md#security-reader) puede ver los resultados. |
| Precios | Gratis (durante la versión preliminar) |
| Distribuciones de Kubernetes admitidas | [Azure Kubernetes Service en Azure Stack HCl](/azure-stack/aks-hci/overview)<br>[Kubernetes](https://kubernetes.io/docs/home/)<br> [AKS Engine](https://github.com/Azure/aks-engine)<br> [Red Hat OpenShift en Azure](https://azure.microsoft.com/services/openshift/)<br> [Red Hat OpenShift](https://www.openshift.com/learn/topics/kubernetes/) (versión 4.6 o posterior)<br> [VMware Tanzu Kubernetes Grid](https://tanzu.vmware.com/kubernetes-grid)<br> [Rancher Kubernetes Engine](https://rancher.com/docs/rke/latest/en/) |
| Limitaciones | Kubernetes habilitado para Azure Arc y la extensión de Defender **no admiten** las ofertas de Kubernetes administrado, como Google Kubernetes Engine y Elastic Kubernetes Service. [Defender for Cloud está disponible de forma nativa para Azure Kubernetes Service (AKS)](defender-for-kubernetes-introduction.md) y no requiere conectar el clúster a Azure Arc. |
| Entornos y regiones | La disponibilidad de esta extensión es la misma que para [Kubernetes habilitado para Azure Arc](../azure-arc/kubernetes/overview.md).|

## <a name="architecture-overview"></a>Información general sobre la arquitectura

Para todos los clústeres de Kubernetes que no sean de AKS, deberá conectar el clúster a Azure Arc. Una vez conectado, Microsoft Defender para Kubernetes se puede implementar en los recursos de [Kubernetes habilitado para Azure Arc](../azure-arc/kubernetes/overview.md) como una [extensión de clúster](../azure-arc/kubernetes/extensions.md).

Los componentes de la extensión recopilan datos de los registros de auditoría de Kubernetes de todos los nodos del plano de control y los envían al back-end de Microsoft Defender para Kubernetes en la nube para su posterior análisis. La extensión se registra con un área de trabajo de Log Analytics que se usa como una canalización de datos, pero los datos del registro de auditoría no se almacenan en el área de trabajo de Log Analytics.

En este diagrama se muestra la interacción entre Microsoft Defender para Kubernetes y el clúster de Kubernetes habilitado para Azure Arc:

:::image type="content" source="media/defender-for-kubernetes-azure-arc/defender-for-kubernetes-architecture-overview.png" alt-text="Diagrama general de la arquitectura que describe la interacción entre Microsoft Defender para Kubernetes y un clúster de Kubernetes habilitado para Azure Arc." lightbox="media/defender-for-kubernetes-azure-arc/defender-for-kubernetes-architecture-overview.png":::

## <a name="prerequisites"></a>Requisitos previos

Antes de implementar la extensión, asegúrese de que:
- [Conecta el clúster de Kubernetes a Azure Arc](../azure-arc/kubernetes/quickstart-connect-cluster.md).
- Cumple los [requisitos previos incluidos en la documentación acerca de las extensiones de clúster genéricas](../azure-arc/kubernetes/extensions.md#prerequisites).
- Configura el **puerto 443** en los siguientes puntos de conexión para el acceso saliente:
    - En cuanto a los clústeres en la nube de Azure Government:
        - *.ods.opinsights.azure.us
        - *.oms.opinsights.azure.us
        - :::no-loc text="login.microsoftonline.us":::
    - En el caso de los clústeres en otras implementaciones en la nube de Azure:
        - *.ods.opinsights.azure.com
        - *.oms.opinsights.azure.com
        - :::no-loc text="login.microsoftonline.com":::

## <a name="deploy-the-defender-extension"></a>Implementación de la extensión de Defender

Puede implementar la extensión de Defender con varios métodos. Para obtener instrucciones detalladas, seleccione la pestaña correspondiente.

### <a name="azure-portal"></a>[**Azure Portal**](#tab/k8s-deploy-asc)

### <a name="use-the-fix-button-from-the-defender-for-cloud-recommendation"></a>Uso del botón Corregir de la recomendación de Defender for Cloud

Una recomendación dedicada en Microsoft Defender for Cloud proporciona lo siguiente:

- **Visibilidad** sobre cuál de los clústeres tiene implementada la extensión Defender para Kubernetes
- Botón **Corregir** para implementarlo en esos clústeres sin la extensión

1. En la página de recomendaciones de Microsoft Defender for Cloud, abra el control de seguridad **Habilitar seguridad mejorada**.

1. Use el filtro para buscar la recomendación denominada **Los clústeres de Kubernetes habilitados para Azure Arc deben tener la extensión de Defender instalada**.

    :::image type="content" source="media/defender-for-kubernetes-azure-arc/extension-recommendation.png" alt-text="Recomendación de Microsoft Defender for Cloud para implementar la extensión de Defender para clústeres de Kubernetes habilitados para Azure Arc." lightbox="media/defender-for-kubernetes-azure-arc/extension-recommendation.png":::

    > [!TIP]
    > Observe el icono **Corregir** en la columna Acciones.

1. Seleccione la extensión para ver los detalles de los recursos correctos e incorrectos; es decir, clústeres con y sin la extensión.

1. En la lista de recursos incorrectos, seleccione un clúster y seleccione **Corregir** para abrir el panel con las opciones de corrección.

1. Seleccione el área de trabajo de Log Analytics correspondiente y seleccione **Corregir recurso x**.

    :::image type="content" source="media/defender-for-kubernetes-azure-arc/security-center-deploy-extension.gif" alt-text="Implemente la extensión de Defender para Azure Arc con la opción de corrección de Defender for Cloud.":::


### <a name="azure-cli"></a>[**CLI de Azure**](#tab/k8s-deploy-cli)

### <a name="use-azure-cli-to-deploy-the-defender-extension"></a>Uso de la CLI de Azure para implementar la extensión de Defender

1. Inicie sesión en Azure:

    ```azurecli
    az login
    az account set --subscription <your-subscription-id>
    ```

    > [!IMPORTANT]
    > Asegúrese de reemplazar ``<your-subscription-id>`` por el mismo identificador de suscripción que usó al conectar el clúster a Azure Arc.

1. Ejecute el siguiente comando para implementar la extensión en el clúster de Kubernetes habilitado para Azure Arc:

    ```azurecli
    az k8s-extension create --name microsoft.azuredefender.kubernetes --cluster-type connectedClusters --cluster-name <cluster-name> --resource-group <resource-group> --extension-type microsoft.azuredefender.kubernetes
    ```

    La siguiente es una descripción de todas las opciones de configuración que admite el tipo de extensión de Defender:

    | Propiedad | Descripción |
    |----------|-------------|
    | logAnalyticsWorkspaceResourceID | **Opcional**. Identificador de recurso completo del área de trabajo de Log Analytics.<br>Cuando no se especifique, se usará el área de trabajo predeterminada de la región.<br><br>Para obtener el identificador de recurso completo, ejecute el siguiente comando para mostrar la lista de áreas de trabajo de sus suscripciones en el formato JSON predeterminado:<br>```az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json```<br><br>El identificador de recurso del área de trabajo de Log Analytics tiene la siguiente sintaxis:<br>/subscriptions/{id-de-suscripción}/resourceGroups/{grupo-de-recursos}/providers/Microsoft.OperationalInsights/workspaces/{nombre-del-área-de-trabajo}. <br>Obtenga más información en [Áreas de trabajo de Log Analytics](../azure-monitor/logs/data-platform-logs.md#log-analytics-and-workspaces). |
    | auditLogPath |**Opcional**. Ruta de acceso completa a los archivos de registro de auditoría.<br>Cuando no se especifique, se usará la ruta de acceso predeterminada ``/var/log/kube-apiserver/audit.log``.<br>En el caso del motor de AKS, la ruta de acceso estándar es ``/var/log/kubeaudit/audit.log``. |

    El siguiente comando muestra un ejemplo de uso de todos los campos opcionales:

    ```azurecli
    az k8s-extension create --name microsoft.azuredefender.kubernetes --cluster-type connectedClusters --cluster-name <your-connected-cluster-name> --resource-group <your-rg> --extension-type microsoft.azuredefender.kubernetes --configuration-settings logAnalyticsWorkspaceResourceID=<log-analytics-workspace-resource-id> auditLogPath=<your-auditlog-path>
    ```

### <a name="resource-manager"></a>[**Resource Manager**](#tab/k8s-deploy-resource-manager)

### <a name="use-azure-resource-manager-to-deploy-the-defender-extension"></a>Uso de Azure Resource Manager para implementar la extensión de Defender

Si quiere usar Azure Resource Manager para implementar la extensión de Defender, necesitará un área de trabajo de Log Analytics en su suscripción. Obtenga más información en [Áreas de trabajo de Log Analytics](../azure-monitor/logs/data-platform-logs.md#log-analytics-and-workspaces).

Puede usar la plantilla de Resource Manager **azure-defender-extension-arm-template.json** de los [ejemplos de instalación](https://aka.ms/kubernetes-extension-installation-examples) de Defender for Cloud.

> [!TIP]
> Si no está familiarizado con las plantillas de Resource Manager, empiece aquí: [¿Qué son las plantillas de Azure Resource Manager?](../azure-resource-manager/templates/overview.md)

### <a name="rest-api"></a>[**API DE REST**](#tab/k8s-deploy-api)

### <a name="use-rest-api-to-deploy-the-defender-extension"></a>Uso de la API de REST para implementar la extensión de Defender 

Si quiere usar la API de REST para implementar la extensión de Defender, necesitará un área de trabajo de Log Analytics en su suscripción. Obtenga más información en [Áreas de trabajo de Log Analytics](../azure-monitor/logs/data-platform-logs.md#log-analytics-and-workspaces).

> [!TIP]
> La manera más sencilla de usar la API para implementar la extensión de Defender es con el **JSON de la colección de Postman** de ejemplo que está incluida en los [ejemplos de instalación](https://aka.ms/kubernetes-extension-installation-examples) de Defender for Cloud.
- Para modificar el JSON de la colección de Postman o implementar manualmente la extensión con la API REST, ejecute el siguiente comando PUT:

    ```rest
    PUT https://management.azure.com/subscriptions/{{Subscription Id}}/resourcegroups/{{Resource Group}}/providers/Microsoft.Kubernetes/connectedClusters/{{Cluster Name}}/providers/Microsoft.KubernetesConfiguration/extensions/microsoft.azuredefender.kubernetes?api-version=2020-07-01-preview
    ```

    Donde:

    | Nombre            | En   | Obligatorio | Tipo   | Descripción                                  |
    |-----------------|------|----------|--------|----------------------------------------------|
    | Id. de suscripción | path | True     | string | Identificador de suscripción del recurso de Kubernetes habilitado para Azure Arc |
    | Grupo de recursos  | path | True     | string | Nombre del grupo de recursos que contiene el recurso de Kubernetes habilitado para Azure Arc |
    | Cluster Name    | path | True     | string | Nombre del recurso de Kubernetes habilitado para Azure Arc  |


    Para la **autenticación**, el encabezado debe tener un token de portador (como sucede con otras API de Azure). Para obtener un token de portador, ejecute el siguiente comando:

    `az account get-access-token --subscription <your-subscription-id>` Use la siguiente estructura para el cuerpo del mensaje:
    ```json
    { 
    "properties": { 
        "extensionType": "microsoft.azuredefender.kubernetes",
    "con    figurationSettings": { 
            "logAnalytics.workspaceId":"YOUR-WORKSPACE-ID"
        // ,    "auditLogPath":"PATH/TO/AUDITLOG"
        },
        "configurationProtectedSettings": {
            "logAnalytics.key":"YOUR-WORKSPACE-KEY"
        }
        }
    } 
    ```

    A continuación se proporciona una descripción de las propiedades:

    | Propiedad | Descripción | 
    | -------- | ----------- |
    | logAnalytics.workspaceId | Identificador de área de trabajo del recurso de Log Analytics. |
    | logAnalytics.key         | Clave del recurso de Log Analytics. | 
    | auditLogPath             | **Opcional**. Ruta de acceso completa a los archivos de registro de auditoría. El valor predeterminado es ``/var/log/kube-apiserver/audit.log`` |

---

## <a name="verify-the-deployment"></a>Comprobación de la implementación

Para comprobar que el clúster tiene instalada la extensión de Defender, siga los pasos que se describen en cualquiera de las siguientes pestañas:

### <a name="azure-portal---defender-for-cloud"></a>[**Azure Portal: Defender for Cloud**](#tab/k8s-verify-asc)

### <a name="use-defender-for-cloud-recommendation-to-verify-the-status-of-your-extension"></a>Uso de la recomendación de Defender for Cloud para comprobar el estado de la extensión

1. En la página de recomendaciones de Microsoft Defender for Cloud, abra el control de seguridad **Habilitar Azure Defender**.

1. Seleccione la recomendación **Los clústeres de Kubernetes habilitados para Azure Arc deben tener la extensión de Azure Defender instalada**.

    :::image type="content" source="media/defender-for-kubernetes-azure-arc/extension-recommendation.png" alt-text="Recomendación de Microsoft Defender for Cloud para implementar la extensión de Defender para clústeres de Kubernetes habilitados para Azure Arc." lightbox="media/defender-for-kubernetes-azure-arc/extension-recommendation.png":::

1. Compruebe que el clúster en el que implementó la extensión aparezca como **Correcto**.


### <a name="azure-portal---azure-arc"></a>[**Azure Portal: Azure Arc**](#tab/k8s-verify-arc)

### <a name="use-the-azure-arc-pages-to-verify-the-status-of-your-extension"></a>Uso de las páginas de Azure Arc para comprobar el estado de la extensión

1. Desde Azure Portal, abra **Azure Arc**.
1. En la lista Infraestructura, seleccione **clústeres de Kubernetes** y, a continuación, seleccione el clúster específico.
1. Abra la página de extensiones. Se muestra una lista de las extensiones del clúster. Para confirmar que la extensión de Defender se instaló correctamente, compruebe la columna **Estado de instalación**.

    :::image type="content" source="media/defender-for-kubernetes-azure-arc/extension-installed-clusters-page.png" alt-text="Página de Azure Arc para comprobar el estado de todas las extensiones instaladas en un clúster de Kubernetes." lightbox="media/defender-for-kubernetes-azure-arc/extension-installed-clusters-page.png":::

1. Para obtener más información, seleccione la extensión.

    :::image type="content" source="media/defender-for-kubernetes-azure-arc/extension-details-page.png" alt-text="Detalles completos de una extensión de Azure Arc en un clúster de Kubernetes.":::


### <a name="azure-cli"></a>[**CLI de Azure**](#tab/k8s-verify-cli)

### <a name="use-azure-cli-to-verify-that-the-extension-is-deployed"></a>Uso de la CLI de Azure para comprobar que la extensión se haya implementado

1. Ejecute el siguiente comando en la CLI de Azure:

    ```azurecli
    az k8s-extension show --cluster-type connectedClusters --cluster-name <your-connected-cluster-name> --resource-group <your-rg> --name microsoft.azuredefender.kubernetes
    ```

1. En la respuesta, busque "extensionType": "microsoft.azuredefender.kubernetes" e "installState": "Installed".

    > [!NOTE]
    > Podría mostrarse "installState": "Pending" durante los primeros minutos.
    
1. Si el estado aparece como **Installed**, ejecute el siguiente comando en el equipo con el archivo `kubeconfig` que apunta al clúster para comprobar que un pod llamado "azuredefender-XXXXX" se encuentra en estado "Running":
    
    ```console
    kubectl get pods -n azuredefender
    ```

### <a name="rest-api"></a>[**API DE REST**](#tab/k8s-verify-api)

### <a name="use-the-rest-api-to-verify-that-the-extension-is-deployed"></a>Uso de la API REST para comprobar que la extensión se haya implementado

Para confirmar una implementación correcta o validar el estado de la extensión en cualquier momento, haga lo siguiente:

1. Ejecute el siguiente comando GET:

    ```rest
    GET https://management.azure.com/subscriptions/{{Subscription Id}}/resourcegroups/{{Resource Group}}/providers/Microsoft.Kubernetes/connectedClusters/{{Cluster Name}}/providers/Microsoft.KubernetesConfiguration/extensions/microsoft.azuredefender.kubernetes?api-version=2020-07-01-preview
    ```

1. En la respuesta, busque en "extensionType": "microsoft.azuredefender.kubernetes" para "installState": "Installed".

    > [!TIP]
    > Podría mostrarse "installState": "Pending" durante los primeros minutos.

1. Si el estado aparece como **Installed**, ejecute el siguiente comando en el equipo con el archivo `kubeconfig` que apunta al clúster para comprobar que un pod llamado "azuredefender-XXXXX" se encuentra en estado "Running":

    ```console
    kubectl get pods -n azuredefender
    ```

---

## <a name="simulate-security-alerts-from-microsoft-defender-for-kubernetes"></a>Simulación de alertas de seguridad de Microsoft Defender para Kubernetes

Hay disponible una lista completa de las alertas compatibles en la [tabla de referencia de todas las alertas de seguridad de Microsoft Defender for Cloud](alerts-reference.md#alerts-k8scluster).

1. Para simular una alerta de seguridad, ejecute el comando siguiente:

    ```console
    kubectl get pods --namespace=asc-alerttest-662jfi039n
    ```

    La respuesta esperada es "No se encontró ningún recurso".

    En un plazo de 30 minutos, Defender for Cloud detectará esta actividad y desencadenará una alerta de seguridad.

1. En Azure Portal, abra la página de alertas de seguridad de Microsoft Defender for Cloud y busque la alerta en el recurso correspondiente:

    :::image type="content" source="media/defender-for-kubernetes-azure-arc/sample-kubernetes-security-alert.png" alt-text="Alerta de ejemplo de Microsoft Defender para Kubernetes." lightbox="media/defender-for-kubernetes-azure-arc/sample-kubernetes-security-alert.png":::

## <a name="removing-the-defender-extension"></a>Eliminación de la extensión de Defender

Puede quitar la extensión mediante Azure Portal, la CLI de Azure o la API REST, como se explica en las pestañas siguientes.

### <a name="azure-portal---arc"></a>[**Azure Portal: Arc**](#tab/k8s-remove-arc)

### <a name="use-azure-portal-to-remove-the-extension"></a>Uso de Azure Portal para quitar la extensión

1. En Azure Portal, abra Azure Arc.
1. En la lista Infraestructura, seleccione **clústeres de Kubernetes** y, a continuación, seleccione el clúster específico.
1. Abra la página de extensiones. Se muestra una lista de las extensiones del clúster.
1. Seleccione el clúster y, después, **Desinstalar**.

    :::image type="content" source="media/defender-for-kubernetes-azure-arc/extension-uninstall-clusters-page.png" alt-text="Eliminación de una extensión del clúster de Kubernetes habilitado para Arc." lightbox="media/defender-for-kubernetes-azure-arc/extension-uninstall-clusters-page.png":::

### <a name="azure-cli"></a>[**CLI de Azure**](#tab/k8s-remove-cli)

### <a name="use-azure-cli-to-remove-the-defender-extension"></a>Uso de la CLI de Azure para quitar la extensión de Defender

1. Quite la extensión para Arc de Microsoft Defender para Kubernetes con los siguientes comandos:

    ```azurecli
    az login
    az account set --subscription <subscription-id>
    az k8s-extension delete --cluster-type connectedClusters --cluster-name <your-connected-cluster-name> --resource-group <your-rg> --name microsoft.azuredefender.kubernetes --yes
    ```

    La eliminación de la extensión puede tardar unos minutos. Se recomienda que espere antes de comprobar que se ha realizado correctamente.

1. Para comprobar que la extensión se ha eliminado correctamente, ejecute los siguientes comandos:

    ```azurecli
    az k8s-extension show --cluster-type connectedClusters --cluster-name <your-connected-cluster-name> --resource-group <your-rg> --name microsoft.azuredefender.kubernetes
    ```

    No debería haber ningún retraso al eliminar el recurso de extensión de Azure Resource Manager. Después, compruebe que no haya ningún pod denominado "azuredefender-XXXXX" en el clúster. Para ello, ejecute el siguiente comando con el archivo `kubeconfig` que apunta al clúster: 

    ```console
    kubectl get pods -n azuredefender
    ```

    Los pods pueden tardar unos minutos en eliminarse.

### <a name="rest-api"></a>[**API DE REST**](#tab/k8s-remove-api)

### <a name="use-rest-api-to-remove-the-defender-extension"></a>Uso de la API de REST para quitar la extensión de Defender 

Para quitar la extensión mediante la API REST, ejecute el siguiente comando DELETE:

```rest
DELETE https://management.azure.com/subscriptions/{{Subscription Id}}/resourcegroups/{{Resource Group}}/providers/Microsoft.Kubernetes/connectedClusters/{{Cluster Name}}/providers/Microsoft.KubernetesConfiguration/extensions/microsoft.azuredefender.kubernetes?api-version=2020-07-01-preview
```

| Nombre            | En   | Obligatorio | Tipo   | Descripción                                           |
|-----------------|------|----------|--------|-------------------------------------------------------|
| Id. de suscripción | path | True     | string | Identificador de suscripción del clúster de Kubernetes habilitado para Azure Arc |
| Grupo de recursos  | path | True     | string | Grupo de recursos del clúster de Kubernetes habilitado para Azure Arc  |
| Cluster Name    | path | True     | string | Nombre del clúster de Kubernetes habilitado para Azure Arc            |

Para la **autenticación**, el encabezado debe tener un token de portador (como sucede con otras API de Azure). Para obtener un token de portador, ejecute el siguiente comando:

```azurecli
az account get-access-token --subscription <your-subscription-id>
```

La solicitud puede tardar varios minutos en completarse.

---

## <a name="next-steps"></a>Pasos siguientes

En esta página se explica cómo implementar la extensión de Defender para clústeres de Kubernetes habilitados para Azure Arc. Obtenga más información sobre las características de seguridad de contenedores de Microsoft Defender for Cloud en las páginas siguientes:

- [Seguridad de contenedores en Defender for Cloud](container-security.md)
- [Introducción a Microsoft Defender para Kubernetes](defender-for-kubernetes-introduction.md)
- [Protección de las cargas de trabajo de Kubernetes](kubernetes-workload-protections.md)
