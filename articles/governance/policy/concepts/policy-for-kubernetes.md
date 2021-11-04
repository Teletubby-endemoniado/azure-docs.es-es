---
title: Información sobre Azure Policy para Kubernetes
description: Obtenga información sobre cómo Azure Policy usa Rego y Open Policy Agent para administrar clústeres que ejecutan Kubernetes en Azure o en el entorno local.
ms.date: 09/13/2021
ms.topic: conceptual
ms.custom: devx-track-azurecli
ms.openlocfilehash: 75f93320e3cb051ec05008146fec08ada4b3a3cb
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131460970"
---
# <a name="understand-azure-policy-for-kubernetes-clusters"></a>Descripción de Azure Policy para clústeres de Kubernetes (versión preliminar)

Azure Policy extiende [Gatekeeper](https://github.com/open-policy-agent/gatekeeper) v3, un _webhook de controlador de admisión_ de [Open Policy Agent](https://www.openpolicyagent.org/) (OPA), a fin de aplicar medidas de seguridad y cumplimiento a escala en los clústeres de una manera centralizada y coherente. Azure Policy permite administrar e informar sobre el estado de cumplimiento de los clústeres de Kubernetes desde un único lugar. El complemento realiza las funciones siguientes:

- Comprueba con el servicio Azure Policy las asignaciones de directivas al clúster.
- Implementa definiciones de directiva en el clúster como recursos de [plantilla de restricción](https://open-policy-agent.github.io/gatekeeper/website/docs/howto/#constraint-templates) y [restricción](https://github.com/open-policy-agent/gatekeeper#constraints).
- Realiza informes de auditoría y cumplimiento y los devuelve al servicio Azure Policy.

Azure Policy para Kubernetes admite los entornos de clúster siguientes:

- [Azure Kubernetes Service (AKS)](../../../aks/intro-kubernetes.md)
- [Kubernetes habilitado para Azure Arc](../../../azure-arc/kubernetes/overview.md)
- [AKS Engine](https://github.com/Azure/aks-engine/blob/master/docs/README.md)

> [!IMPORTANT]
> Los complementos para el motor de AKS y los Kubernetes habilitados para Arc están en **versión preliminar**. Azure Policy para Kubernetes solo admite grupos de nodos de Linux y definiciones de directivas integradas (las definiciones de directivas personalizadas son una característica en _versión preliminar pública)_ . Las definiciones de directivas integradas se encuentran en la categoría **Kubernetes**. Las definiciones de directiva de versión preliminar limitadas con efecto **EnforceOPAConstraint** y **EnforceRegoPolicy** y la categoría **servicio Kubernetes** relacionada están en _desuso_.
> En su lugar, use los efectos _audit_ y _deny_ con el modo de proveedor de recursos `Microsoft.Kubernetes.Data`.

## <a name="overview"></a>Información general

Para habilitar y usar Azure Policy con el clúster de Kubernetes, realice las acciones siguientes:

1. Configure el clúster de Kubernetes e instale el complemento:
   - [Azure Kubernetes Service (AKS)](#install-azure-policy-add-on-for-aks)
   - [Kubernetes habilitado para Azure Arc](#install-azure-policy-add-on-for-azure-arc-enabled-kubernetes)
   - [AKS Engine](#install-azure-policy-add-on-for-aks-engine)

   > [!NOTE]
   > Para resolver problemas de instalación comunes, consulte el tema de [solución de problemas del complemento de Azure Policy](../troubleshoot/general.md#add-on-for-kubernetes-installation-errors).

1. [Descripción del lenguaje de Azure Policy para Kubernetes](#policy-language)

1. [Asignación de una definición a un clúster de Kubernetes](#assign-a-policy-definition)

1. [Espere la validación](#policy-evaluation)

## <a name="limitations"></a>Limitaciones

Las siguientes limitaciones generales se aplican al complemento de Azure Policy en los clústeres de Kubernetes:

- El complemento Azure Policy para Kubernetes es compatible con la versión **1.14** o posterior de Kubernetes.
- El complemento Azure Policy para Kubernetes solo se puede implementar en grupos de nodos de Linux.
- Solo se admiten las definiciones de directivas integradas. Las definiciones de directivas personalizadas son una característica en _versión preliminar pública_.
- Número máximo de pods admitidos por el complemento de Azure Policy: **10 000**
- Número máximo de registros no compatibles por directiva por clúster: **500**
- Número máximo de registros no compatibles por suscripción: **1 millón**
- No se admiten las instalaciones de Gatekeeper fuera del complemento de Azure Policy. Desinstale los componentes instalados por una instalación anterior de Gatekeeper antes de habilitar el complemento de Azure Policy.
- [Los motivos de la no compatibilidad](../how-to/determine-non-compliance.md#compliance-reasons) no están disponibles para el `Microsoft.Kubernetes.Data`
  [modo de proveedor de recursos](./definition-structure.md#resource-provider-modes). Use [Detalles del componente](../how-to/determine-non-compliance.md#component-details-for-resource-provider-modes).
- No se admiten [exenciones](./exemption-structure.md) para los [modos de proveedor de recursos](./definition-structure.md#resource-provider-modes).

Las siguientes limitaciones generales solo se aplican al complemento de Azure Policy para AKS:

- [La directiva de seguridad de pod de AKS](../../../aks/use-pod-security-policies.md) y el complemento de Azure Policy para AKS no se pueden habilitar simultáneamente. Para obtener más información, consulte [Limitación de seguridad de pod de AKS](../../../aks/use-azure-policy.md).
- Los espacios de nombres se excluyen automáticamente mediante el complemento de Azure Policy para la evaluación: _kube-system_, _gatekeeper-system_ y _aks-periscope_.

## <a name="recommendations"></a>Recomendaciones

A continuación se muestran recomendaciones generales para usar el complemento de Azure Policy:

- El complemento de Azure Policy requiere tres componentes de Gatekeeper para ejecutarse: 1 pod de auditoría y 2 réplicas de pods de webhook. Estos componentes consumen más recursos, ya que el recuento de los recursos de Kubernetes y las asignaciones de directivas aumentan en el clúster, lo que requiere operaciones de auditoría y cumplimiento.

  - Para menos de 500 pods en un solo clúster con un máximo de 20 restricciones: 2 vCPU y 350 MB de memoria por componente.
  - Para más de 500 pods en un solo clúster con un máximo de 40 restricciones: 3 vCPU y 600 MB de memoria por componente.

- Los pods de Windows [no admiten contextos de seguridad](https://kubernetes.io/docs/concepts/security/pod-security-standards/#what-profiles-should-i-apply-to-my-windows-pods).
  Por lo tanto, algunas de las definiciones de Azure Policy, como no permitir privilegios raíz, no se pueden escalar en pods de Windows y solo se aplican a los pods de Linux.

La siguiente recomendación solo se aplica a AKS y al complemento de Azure Policy:

- Use el grupo de nodos del sistema con el valor taint `CriticalAddonsOnly` para programar pods de Gatekeeper. Para obtener más información, consulte [Uso de pods de nodos del sistema](../../../aks/use-system-pools.md#system-and-user-node-pools).
- Proteja el tráfico saliente de sus clústeres de AKS. Para obtener más información, consulte [Control del tráfico de salida de los nodos de clúster](../../../aks/limit-egress-traffic.md).
- Si el clúster tiene `aad-pod-identity` habilitado, los pods de Identidad administrada del nodo (NMI) modifican las tablas de IP de los nodos para interceptar las llamadas que se realizan en el punto de conexión de Azure Instance Metadata. Esta configuración hace que NMI intercepte toda solicitud realizada al punto de conexión de Metadata, incluso aunque el pod no utilice `aad-pod-identity`. La CRD de AzurePodIdentityException se puede configurar para informar a `aad-pod-identity` de que las solicitudes dirigidas al punto de conexión de Metadata que se originen en un pod que coincida con las etiquetas definidas en la CRD deben pasar por el servidor proxy sin que se procesen en NMI. Los pods del sistema con la etiqueta `kubernetes.azure.com/managedby: aks` del espacio de nombres _kube-system_ deben excluirse en `aad-pod-identity` configurando la CRD de AzurePodIdentityException. Para obtener más información, consulte este artículo acerca de [cómo deshabilitar aad-pod-identity en una aplicación o pod específicos](https://azure.github.io/aad-pod-identity/docs/configure/application_exception).
  Para configurar una excepción, instale [mic-exception.YAML](https://github.com/Azure/aad-pod-identity/blob/master/deploy/infra/mic-exception.yaml).

## <a name="install-azure-policy-add-on-for-aks"></a>Instalación del complemento de Azure Policy para AKS

Antes de instalar el complemento de Azure Policy o habilitar cualquiera de las características del servicio, la suscripción debe habilitar los proveedores de recursos **Microsoft.PolicyInsights**.

1. Es preciso que esté instalada y configurada la versión 2.12.0 de la CLI de Azure, o cualquier otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

1. Registre los proveedores de recursos y las características en vista previa.

   - Azure Portal:

     Registre el proveedor de recursos **Microsoft.PolicyInsights**. Para conocer los pasos, consulte [Proveedores de recursos y sus tipos](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

   - CLI de Azure:

     ```azurecli-interactive
     # Log in first with az login if you're not using Cloud Shell

     # Provider register: Register the Azure Policy provider
     az provider register --namespace Microsoft.PolicyInsights
     ```

1. Si hay definiciones de directiva de versión preliminar limitadas instaladas, quite el complemento con el botón **Deshabilitar** en el clúster de AKS en la página **Directivas**.

1. Se debe usar la versión del clúster de AKS _1.14_ o posterior. Use el siguiente script para validar la versión del clúster de AKS:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Look for the value in kubernetesVersion
   az aks list
   ```

1. Instale la versión _2.12.0_ o superior de la CLI de Azure. Para más información, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

Una vez que se han completado los pasos anteriores de requisitos previos, instale el complemento de Azure Policy en el clúster de AKS que quiera administrar.

- Azure portal

  1. Inicie el servicio AKS en Azure Portal al seleccionar **Todos los servicios**; a continuación, busque y seleccione **Servicios de Kubernetes**.

  1. Seleccione uno de sus clústeres de AKS.

  1. Seleccione **Directivas** en el lado izquierdo de la página de servicio de Kubernetes.

  1. En la página principal, seleccione el botón **Enable add-on** (Habilitar complemento).

- Azure CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks enable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

Para comprobar que la instalación del complemento se ha realizado correctamente y que los pods _azure-policy_ y _gatekeeper_ están en ejecución, ejecute el comando siguiente:

```bash
# azure-policy pod is installed in kube-system namespace
kubectl get pods -n kube-system

# gatekeeper pod is installed in gatekeeper-system namespace
kubectl get pods -n gatekeeper-system
```

Por último, para comprobar que el complemento más reciente está instalado, ejecute este comando de la CLI de Azure y reemplace `<rg>` por el nombre del grupo de recursos y `<cluster-name>` por el nombre del clúster de AKS: `az aks show --query addonProfiles.azurePolicy -g <rg> -n <cluster-name>`. La resultado tiene que ser similar a la siguiente salida:

```output
{
        "config": null,
        "enabled": true,
        "identity": null
}
```

## <a name="install-azure-policy-add-on-for-azure-arc-enabled-kubernetes-preview"></a><a name="install-azure-policy-add-on-for-azure-arc-enabled-kubernetes"></a>Instalación del complemento de Azure Policy de Kubernetes habilitado para Azure Arc (versión preliminar)

Antes de instalar el complemento de Azure Policy o habilitar cualquiera de las características del servicio, su suscripción debe tener habilitados el proveedor de recursos **Microsoft.PolicyInsights** y crear una asignación de roles para la entidad de servicio del clúster.

1. Es preciso que esté instalada y configurada la versión 2.12.0 de la CLI de Azure, o cualquier otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

1. Para habilitar el proveedor de recursos, siga los pasos descritos en [Tipos y proveedores de recursos](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal) o ejecute el comando de PowerShell o la CLI de Azure:

   - Azure CLI

     ```azurecli-interactive
     # Log in first with az login if you're not using Cloud Shell

     # Provider register: Register the Azure Policy provider
     az provider register --namespace 'Microsoft.PolicyInsights'
     ```

   - Azure PowerShell

     ```azurepowershell-interactive
     # Log in first with Connect-AzAccount if you're not using Cloud Shell

     # Provider register: Register the Azure Policy provider
     Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
     ```

1. Se debe usar la versión del clúster de Kubernetes _1.14_ o posterior.

1. Instale [Helm 3](https://v3.helm.sh/docs/intro/install/).

1. El clúster de Kubernetes habilitado para Azure Arc. Para obtener más información, vea [Incorporación de un clúster de Kubernetes a Azure Arc](../../../azure-arc/kubernetes/quickstart-connect-cluster.md).

1. Tener el identificador de recurso completo de Azure del clúster de Kubernetes habilitado para Azure Arc.

1. Abra los puertos para el complemento. El complemento de Azure Policy usa estos dominios y puertos para obtener las definiciones de directiva y las asignaciones, y notificar a Azure Policy el cumplimiento del clúster.

   |Domain |Port |
   |---|---|
   |`data.policy.core.windows.net` |`443` |
   |`store.policy.core.windows.net` |`443` |
   |`login.windows.net` |`443` |
   |`dc.services.visualstudio.com` |`443` |

1. Asigne la asignación de rol "Escritor de datos de Policy Insights (versión preliminar)" al clúster de Kubernetes habilitado para Azure Arc. Reemplace `<subscriptionId>` por el identificador de la suscripción, `<rg>` por el grupo de recursos del clúster de Kubernetes habilitado para Azure Arc y `<clusterName>` por el nombre del clúster de Kubernetes habilitado para Azure Arc. Realice el seguimiento de los valores devueltos para _appId_, _password_ y _tenant_ para los pasos de instalación.

   - Azure CLI

     ```azurecli-interactive
     az ad sp create-for-rbac --role "Policy Insights Data Writer (Preview)" --scopes "/subscriptions/<subscriptionId>/resourceGroups/<rg>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
     ```

   - Azure PowerShell

     ```azurepowershell-interactive
     $sp = New-AzADServicePrincipal -Role "Policy Insights Data Writer (Preview)" -Scope "/subscriptions/<subscriptionId>/resourceGroups/<rg>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"

     @{ appId=$sp.ApplicationId;password=[System.Runtime.InteropServices.Marshal]::PtrToStringAuto([System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($sp.Secret));tenant=(Get-AzContext).Tenant.Id } | ConvertTo-Json
     ```

   Ejemplo de salida de los comandos anteriores:

   ```json
   {
       "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
       "password": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
       "tenant": "cccccccc-cccc-cccc-cccc-cccccccccccc"
   }
   ```

Una vez que se han completado los pasos anteriores de requisitos previos, instale el complemento de Azure Policy en el clúster de Kubernetes habilitado para Azure Arc:

1. Agregue el repositorio del complemento de Azure Policy a Helm:

   ```bash
   helm repo add azure-policy https://raw.githubusercontent.com/Azure/azure-policy/master/extensions/policy-addon-kubernetes/helm-charts
   ```

1. Instale el complemento de Azure Policy con el gráfico de Helm:

   ```bash
   # In below command, replace the following values with those gathered above.
   #    <AzureArcClusterResourceId> with your Azure Arc enabled Kubernetes cluster resource Id. For example: /subscriptions/<subscriptionId>/resourceGroups/<rg>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>
   #    <ServicePrincipalAppId> with app Id of the service principal created during prerequisites.
   #    <ServicePrincipalPassword> with password of the service principal created during prerequisites.
   #    <ServicePrincipalTenantId> with tenant of the service principal created during prerequisites.
   helm install azure-policy-addon azure-policy/azure-policy-addon-arc-clusters \
       --set azurepolicy.env.resourceid=<AzureArcClusterResourceId> \
       --set azurepolicy.env.clientid=<ServicePrincipalAppId> \
       --set azurepolicy.env.clientsecret=<ServicePrincipalPassword> \
       --set azurepolicy.env.tenantid=<ServicePrincipalTenantId>
   ```

   Para obtener más información sobre lo que instala el gráfico de Helm de complemento, consulte la [definición del gráfico de Helm de complemento de Azure Policy](https://github.com/Azure/azure-policy/tree/master/extensions/policy-addon-kubernetes/helm-charts/azure-policy-addon-arc-clusters) en GitHub.

Para comprobar que la instalación del complemento se ha realizado correctamente y que los pods _azure-policy_ y _gatekeeper_ están en ejecución, ejecute el comando siguiente:

```bash
# azure-policy pod is installed in kube-system namespace
kubectl get pods -n kube-system

# gatekeeper pod is installed in gatekeeper-system namespace
kubectl get pods -n gatekeeper-system
```

## <a name="install-azure-policy-add-on-for-aks-engine-preview"></a><a name="install-azure-policy-add-on-for-aks-engine"></a>Instalación del complemento de Azure Policy para AKS Engine (versión preliminar)

Antes de instalar el complemento de Azure Policy o habilitar cualquiera de las características del servicio, su suscripción debe tener habilitados el proveedor de recursos **Microsoft.PolicyInsights** y crear una asignación de roles para la entidad de servicio del clúster.

1. Es preciso que esté instalada y configurada la versión 2.0.62 de la CLI de Azure, o cualquier otra versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

1. Para habilitar el proveedor de recursos, siga los pasos descritos en [Tipos y proveedores de recursos](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal) o ejecute el comando de PowerShell o la CLI de Azure:

   - Azure CLI

     ```azurecli-interactive
     # Log in first with az login if you're not using Cloud Shell

     # Provider register: Register the Azure Policy provider
     az provider register --namespace 'Microsoft.PolicyInsights'
     ```

   - Azure PowerShell

     ```azurepowershell-interactive
     # Log in first with Connect-AzAccount if you're not using Cloud Shell

     # Provider register: Register the Azure Policy provider
     Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
     ```

1. Cree una asignación de roles para la entidad de servicio del clúster.

   - Si no conoce el identificador de la aplicación de la entidad de servicio del clúster, búsquelo con el siguiente comando.

     ```bash
     # Get the kube-apiserver pod name
     kubectl get pods -n kube-system

     # Find the aadClientID value
     kubectl exec <kube-apiserver pod name> -n kube-system cat /etc/kubernetes/azure.json
     ```

   - Consigne la asignación de roles "Policy Insights Data Writer (Preview)" al identificador de la aplicación de la entidad de servicio del clúster (valor _aadClientID_ del paso anterior) con la CLI de Azure. Reemplace `<subscriptionId>` por el identificador de su suscripción y `<aks engine cluster resource group>` por el grupo de recursos en el que se encuentra el clúster de Kubernetes autoadministrado de AKS Engine.

     ```azurecli-interactive
     az role assignment create --assignee <cluster service principal app ID> --scope "/subscriptions/<subscriptionId>/resourceGroups/<aks engine cluster resource group>" --role "Policy Insights Data Writer (Preview)"
     ```

Una vez que se han completado los pasos anteriores de requisitos previos, instale el complemento de Azure Policy. La instalación puede realizarse durante la creación o el ciclo de actualización de una instancia de AKS Engine o como una acción independiente en un clúster existente.

- Instalación durante la creación o el ciclo de actualización

  Para habilitar el complemento Azure Policy durante la creación de un nuevo clúster autoadministrado o como una actualización de un clúster existente, incluya la definición del clúster de la propiedad [addons](https://github.com/Azure/aks-engine/tree/master/examples/addons/azure-policy) para AKS Engine.

  ```json
  "addons": [{
      "name": "azure-policy",
      "enabled": true
  }]
  ```

  Para obtener más información al respecto, consulte la guía externa de [definición del clúster de AKS Engine](https://github.com/Azure/aks-engine/blob/master/docs/topics/clusterdefinitions.md).

- Instalación en un clúster existente con gráficos de Helm

  Siga estos pasos para preparar el clúster e instalar el complemento:

  1. Instale [Helm 3](https://v3.helm.sh/docs/intro/install/).

  1. Agregue el repositorio Azure Policy a Helm.

     ```bash
     helm repo add azure-policy https://raw.githubusercontent.com/Azure/azure-policy/master/extensions/policy-addon-kubernetes/helm-charts
     ```

     Para obtener más información, consulte la [guía de inicio rápido del gráfico de Helm](https://helm.sh/docs/using_helm/#quickstart-guide).

  1. Instale el complemento con un gráfico de Helm. Reemplace `<subscriptionId>` por el identificador de su suscripción y `<aks engine cluster resource group>` por el grupo de recursos en el que se encuentra el clúster de Kubernetes autoadministrado de AKS Engine.

     ```bash
     helm install azure-policy-addon azure-policy/azure-policy-addon-aks-engine --set azurepolicy.env.resourceid="/subscriptions/<subscriptionId>/resourceGroups/<aks engine cluster resource group>"
     ```

     Para obtener más información sobre lo que instala el gráfico de Helm de complemento, consulte la [definición del gráfico de Helm de complemento de Azure Policy](https://github.com/Azure/azure-policy/tree/master/extensions/policy-addon-kubernetes/helm-charts/azure-policy-addon-aks-engine) en GitHub.

     > [!NOTE]
     > Debido a la relación entre el complemento de Azure Policy y el identificador del grupo de recursos, Azure Policy solo admite un clúster de AKS Engine para cada grupo de recursos.

Para comprobar que la instalación del complemento se ha realizado correctamente y que los pods _azure-policy_ y _gatekeeper_ están en ejecución, ejecute el comando siguiente:

```bash
# azure-policy pod is installed in kube-system namespace
kubectl get pods -n kube-system

# gatekeeper pod is installed in gatekeeper-system namespace
kubectl get pods -n gatekeeper-system
```

## <a name="policy-language"></a>Idioma de directiva

La estructura del lenguaje de Azure Policy para administrar Kubernetes sigue la de las definiciones de directiva existentes. Con un [modo de proveedor de recursos](./definition-structure.md#resource-provider-modes) de `Microsoft.Kubernetes.Data`, se usan los efectos [audit](./effects.md#audit) y [deny](./effects.md#deny) para administrar los clústeres de Kubernetes. _Audit_ y _deny_ deben proporcionar propiedades **details** específicas para trabajar con [OPA Constraint Framework](https://github.com/open-policy-agent/frameworks/tree/master/constraint) y Gatekeeper v3.

Como parte de las propiedades _details.templateInfo_, _details.constraint_ o _details.constraintTemplate_ de la definición de directiva, Azure Policy pasa los URI o valores codificados en Base64 de estas [CustomResourceDefinitions](https://open-policy-agent.github.io/gatekeeper/website/docs/howto/#constraint-templates) (CRD) al complemento. Rego es el lenguaje que OPA y el equipo selector admiten para validar una solicitud al clúster de Kubernetes. Al admitir un estándar existente para la administración de Kubernetes, Azure Policy le permite reutilizar las reglas existentes y vincularlas con Azure Policy para obtener una experiencia de informes de cumplimiento unificada en la nube. Para obtener más información, consulte [¿Qué es Rego?](https://www.openpolicyagent.org/docs/latest/policy-language/#what-is-rego)

## <a name="assign-a-policy-definition"></a>Asignación de una definición de directiva

Para asignar una definición de directiva al clúster de Kubernetes, debe tener asignadas las operaciones de asignación de directivas de control de acceso basado en rol de Azure (RBAC de AZure) adecuadas. Los roles de Azure integrados **Colaborador de la directiva de recursos** y **Propietario** tienen estas operaciones. Para obtener más información, vea [Permisos de RBAC de Azure en Azure Policy](../overview.md#azure-rbac-permissions-in-azure-policy).

> [!NOTE]
> Las definiciones de directivas personalizadas son una característica en _versión preliminar pública_.

Siga estos pasos para buscar las directivas integradas para administrar el clúster mediante Azure Portal. Si usa una definición de directiva personalizada, busque por nombre o por la categoría con la que la creó.

1. Inicie el servicio Azure Policy en Azure Portal. Seleccione **All services** (Todos los servicios) en el panel izquierdo y busque y seleccione la opción **Policy** (Directiva).

1. En el panel izquierdo de la página de Azure Policy, seleccione **Definitions** (Definiciones).

1. En el cuadro de lista desplegable Categoría, use **Seleccionar todo** para borrar el filtro y seleccione **Kubernetes**.

1. Seleccione la definición de directiva y pulse el botón **Assign** (Asignar).

1. Establezca el **Ámbito** en el grupo de administración, la suscripción o el grupo de recursos del clúster de Kubernetes al que se aplicará la asignación de directiva.

   > [!NOTE]
   > Al asignar Azure Policy para la definición de Kubernetes, el **Ámbito** debe incluir el recurso de clúster. En el caso de un clúster de AKS Engine, el **ámbito** debe ser el grupo de recursos del clúster.

1. Asigne un **Nombre** y una **Descripción** a la asignación de directiva que pueda usar para identificarla con facilidad.

1. Establezca [Cumplimiento de directivas](./assignment-structure.md#enforcement-mode) en uno de los valores siguientes.

   - **Habilitado**: aplicar la directiva en el clúster. Se deniegan las solicitudes de admisión de Kubernetes con infracciones.

   - **Deshabilitado**: no aplicar la directiva en el clúster. No se deniegan las solicitudes de admisión de Kubernetes con infracciones. Los resultados de la evaluación de cumplimiento siguen estando disponibles. Al implementar nuevas definiciones de directiva para ejecutar clústeres, la opción _Deshabilitado_ resulta útil para probar la definición de directiva, ya que las solicitudes de admisión con infracciones no se deniegan.

1. Seleccione **Next** (Siguiente).

1. Establecer **Valores de parámetros**

   - Para excluir los espacios de nombres de Kubernetes de la evaluación de directivas, especifique la lista de espacios de nombres en el parámetro **Exclusiones de los espacios de nombres**. Se recomienda excluir: _kube-system_, _gatekeeper-system_ y _azure-arc_.

1. Seleccione **Revisar + crear**.

Como alternativa, use el inicio rápido [Asignación de una directiva: Portal](../assign-policy-portal.md) para buscar y asignar una directiva de Kubernetes. Busque una definición de directiva de Kubernetes en lugar del ejemplo "audit vms".

> [!IMPORTANT]
> Las definiciones de directiva integradas están disponibles para los clústeres de Kubernetes en la categoría **Kubernetes**. Para obtener una lista de las definiciones de directiva integradas, vea [Ejemplos de Kubernetes](../samples/built-in-policies.md#kubernetes).

## <a name="policy-evaluation"></a>Evaluación de directiva

El complemento se registra con el servicio Azure Policy para detectar cambios en las asignaciones de directivas cada 15 minutos.
Durante este ciclo de actualización, el complemento comprueba si hay cambios. Estos cambios desencadenan la creación, actualización o eliminación de restricciones y plantillas de restricciones.

En un clúster de Kubernetes, si un espacio de nombres tiene la etiqueta adecuada para el clúster, no se deniegan las solicitudes de admisión con infracciones. Los resultados de la evaluación de cumplimiento siguen estando disponibles.

- Clúster de Kubernetes habilitado para Azure Arc: `admission.policy.azure.com/ignore`
- Clúster de Azure Kubernetes Service: `control-plane`

> [!NOTE]
> Aunque es posible que un administrador de clústeres tenga permiso para crear y actualizar los recursos de plantillas y restricciones instalados por el complemento Azure Policy, estos no son escenarios admitidos, ya que las actualizaciones manuales se sobrescriben. Gatekeeper sigue evaluando las directivas que existían antes de instalar el complemento y de asignar las definiciones de directiva de Azure Policy.

Cada 15 minutos, el complemento solicita un examen completo del clúster. Después de recopilar los detalles del examen completo y las evaluaciones en tiempo real que realiza Gatekeeper sobre los intentos de cambios en el clúster, el complemento proporciona los resultados a Azure Policy para los agregue en los [detalles de cumplimiento](../how-to/get-compliance-data.md) como cualquier otra asignación de Azure Policy. Solo los resultados de las asignaciones de directivas activas se devuelven durante el ciclo de auditoría. Los resultados de la auditoría también pueden verse como [infracciones](https://github.com/open-policy-agent/gatekeeper#audit) enumeradas en el campo de estado de la restricción errónea. Para obtener más información sobre recursos _no compatibles_, consulte [Detalles de los componentes de los modos de proveedor de recursos](../how-to/determine-non-compliance.md#component-details-for-resource-provider-modes).

> [!NOTE]
> En cada informe de cumplimiento de Azure Policy para los clústeres de Kubernetes se incluyen todas las infracciones de los últimos 45 minutos. La marca de tiempo indica cuándo se ha producido una infracción.

Otras consideraciones:

- Si la suscripción del clúster se registra con Azure Security Center, las directivas de Kubernetes de Azure Security Center se aplicarán automáticamente en el clúster.

- Cuando se aplica una directiva de denegación en un clúster que ya tiene recursos de Kubernetes, aunque estos recursos no sean compatibles con la nueva directiva, seguirán ejecutándose. Cuando un recurso que no es compatible vuelve a programarse en un nodo diferente, Gatekeeper bloquea la creación de recursos.

- Si un clúster tiene una directiva de denegación que valida los recursos, no se mostrará ningún mensaje de rechazo al usuario cuando cree una implementación. Por ejemplo, imagine que tiene una implementación de Kubernetes con pods y conjuntos de réplicas. Cuando un usuario ejecute `kubectl describe deployment $MY_DEPLOYMENT`, no se devolverá un mensaje de rechazo durante los eventos. Sin embargo, `kubectl describe replicasets.apps $MY_DEPLOYMENT` sí devolverá los eventos relacionados con el rechazo.

> [!NOTE]
> Los contenedores de inicialización se pueden incluir durante la evaluación de directivas. Para ver si los contenedores de inicialización están incluidos, revise el valor de CRD para la declaración siguiente u otra similar:
>
> ```rego
> input_containers[c] { 
>    c := input.review.object.spec.initContainers[_] 
> }
> ```

### <a name="constraint-template-conflicts"></a>Conflictos de plantilla de restricciones

Si las plantillas de restricciones tienen el mismo nombre de los metadatos de recursos, pero la definición de directiva hace referencia al origen en ubicaciones diferentes, se considera que las definiciones de directiva están en conflicto. Ejemplo: dos definiciones de directiva hacen referencia al mismo archivo `template.yaml` almacenado en diferentes ubicaciones de origen como, por ejemplo, el almacén de plantillas de Azure Policy (`store.policy.core.windows.net`) y GitHub.

Cuando se asignan las definiciones de directiva y sus plantillas de restricciones, pero aún no están instaladas en el clúster y están en conflicto, se notifican como un conflicto y no se instalarán en el clúster hasta que este se resuelva. Del mismo modo, las definiciones de directiva existentes y sus plantillas de restricciones que ya están en el clúster que entren en conflicto con las definiciones de directiva recién asignadas seguirán funcionando con normalidad. Si se actualiza una asignación existente y se produce un error al sincronizar la plantilla de restricciones, el clúster también se marca como un conflicto. Para ver todos los mensajes de conflicto, consulte [Motivos de cumplimiento del modo del proveedor de recursos de AKS](../how-to/determine-non-compliance.md#aks-resource-provider-mode-compliance-reasons).

## <a name="logging"></a>Registro

Como controlador o contenedor de Kubernetes, los pods _azure-policy_ y _gatekeeper_ mantienen registros en el clúster de Kubernetes. Los registros se pueden exponer en la página **Insights** (Detalles) del clúster de Kubernetes. Para obtener más información, vea [Supervisión del rendimiento del clúster de Kubernetes con Azure Monitor para contenedores](../../../azure-monitor/containers/container-insights-analyze.md).

Para ver los registros del complemento, use `kubectl`:

```bash
# Get the azure-policy pod name installed in kube-system namespace
kubectl logs <azure-policy pod name> -n kube-system

# Get the gatekeeper pod name installed in gatekeeper-system namespace
kubectl logs <gatekeeper pod name> -n gatekeeper-system
```

Para obtener más información, vea [Depuración de Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/docs/debug/) en la documentación de Gatekeeper.

## <a name="view-gatekeeper-artifacts"></a>Visualización de artefactos de Gatekeeper

Después de que el complemento descarga las asignaciones de directiva e instala las plantillas de restricción y las restricciones en el clúster, anota ambos con información de Azure Policy, como el identificador de asignación de directiva y el identificador de definición de directiva. Para configurar el cliente para ver los artefactos relacionados con el complemento, siga estos pasos:

1. Configure `kubeconfig` para el clúster.

   En el caso de un nuevo clúster de Azure Kubernetes Service, use la siguiente CLI de Azure:

   ```azurecli-interactive
   # Set context to the subscription
   az account set --subscription <YOUR-SUBSCRIPTION>

   # Save credentials for kubeconfig into .kube in your home folder
   az aks get-credentials --resource-group <RESOURCE-GROUP> --name <CLUSTER-NAME>
   ```

1. Pruebe la conexión del clúster.

   Ejecute el comando `kubectl cluster-info`. Una ejecución correcta hace que cada servicio responda con una dirección URL de dónde se ejecuta.

### <a name="view-the-add-on-constraint-templates"></a>Visualización de las plantillas de restricción del complemento

Para ver las plantillas de restricción descargadas por el complemento, ejecute `kubectl get constrainttemplates`.
Las plantillas de restricción que comienzan por `k8sazure` son las instaladas por el complemento.

### <a name="get-azure-policy-mappings"></a>Obtención de las asignaciones de Azure Policy

Para identificar la asignación entre una plantilla de restricción descargada en el clúster y la definición de directiva, use `kubectl get constrainttemplates <TEMPLATE> -o yaml`. Los resultados se parecen a la siguiente salida:

```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
    annotations:
    azure-policy-definition-id: /subscriptions/<SUBID>/providers/Microsoft.Authorization/policyDefinitions/<GUID>
    constraint-template-installed-by: azure-policy-addon
    constraint-template: <URL-OF-YAML>
    creationTimestamp: "2021-09-01T13:20:55Z"
    generation: 1
    managedFields:
    - apiVersion: templates.gatekeeper.sh/v1beta1
    fieldsType: FieldsV1
...
```

`<SUBID>` es el identificador de suscripción y `<GUID>` es el identificador de la definición de directiva asignada.
`<URL-OF-YAML>` es la ubicación de origen de la plantilla de restricción que descargó el complemento para instalar en el clúster.

### <a name="view-constraints-related-to-a-constraint-template"></a>Visualización de las restricciones relacionadas con una plantilla de restricción

Una vez que tenga los nombres de las [plantillas de restricción descargadas del complemento](#view-the-add-on-constraint-templates), puede usar el nombre para ver las restricciones relacionadas. Use `kubectl get <constraintTemplateName>` para obtener la lista.
Las restricciones instaladas por el complemento comienzan por `azurepolicy-`.

### <a name="view-constraint-details"></a>Visualización de los detalles de la restricción

La restricción tiene detalles sobre las infracciones y asignaciones a la definición y asignación de directiva. Para ver los detalles, use `kubectl get <CONSTRAINT-TEMPLATE> <CONSTRAINT> -o yaml`. Los resultados se parecen a la siguiente salida:

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sAzureContainerAllowedImages
metadata:
  annotations:
    azure-policy-assignment-id: /subscriptions/<SUB-ID>/resourceGroups/<RG-NAME>/providers/Microsoft.Authorization/policyAssignments/<ASSIGNMENT-GUID>
    azure-policy-definition-id: /providers/Microsoft.Authorization/policyDefinitions/<DEFINITION-GUID>
    azure-policy-definition-reference-id: ""
    azure-policy-setdefinition-id: ""
    constraint-installed-by: azure-policy-addon
    constraint-url: <URL-OF-YAML>
  creationTimestamp: "2021-09-01T13:20:55Z"
spec:
  enforcementAction: deny
  match:
    excludedNamespaces:
    - kube-system
    - gatekeeper-system
    - azure-arc
  parameters:
    imageRegex: ^.+azurecr.io/.+$
status:
  auditTimestamp: "2021-09-01T13:48:16Z"
  totalViolations: 32
  violations:
  - enforcementAction: deny
    kind: Pod
    message: Container image nginx for container hello-world has not been allowed.
    name: hello-world-78f7bfd5b8-lmc5b
    namespace: default
  - enforcementAction: deny
    kind: Pod
    message: Container image nginx for container hello-world has not been allowed.
    name: hellow-world-89f8bfd6b9-zkggg
```

## <a name="troubleshooting-the-add-on"></a>Solución de problemas del complemento

Para más información acerca de cómo solucionar los problemas del complemento de Kubernetes, consulte la sección [Kubernetes](../troubleshoot/general.md#add-on-for-kubernetes-general-errors) en el artículo de solución de problemas de Azure Policy.

## <a name="remove-the-add-on"></a>Eliminar el complemento

### <a name="remove-the-add-on-from-aks"></a>Eliminación del complemento de AKS

Para eliminar el complemento de Azure Policy del clúster de AKS, use Azure Portal o la CLI de Azure:

- Azure portal

  1. Inicie el servicio AKS en Azure Portal al seleccionar **Todos los servicios**; a continuación, busque y seleccione **Servicios de Kubernetes**.

  1. Seleccione el clúster de AKS en el que quiera deshabilitar el complemento de Azure Policy.

  1. Seleccione **Directivas** en el lado izquierdo de la página de servicio de Kubernetes.

  1. En la página principal, pulse **Disable add-on** (Deshabilitar complemento).

- Azure CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

### <a name="remove-the-add-on-from-azure-arc-enabled-kubernetes"></a>Eliminación del complemento desde Kubernetes habilitado para Azure Arc

Para quitar el complemento de Azure Policy y Gatekeeper desde el clúster de Kubernetes habilitado para Azure Arc, ejecute el siguiente comando de Helm:

```bash
helm uninstall azure-policy-addon
```

### <a name="remove-the-add-on-from-aks-engine"></a>Eliminación del complemento desde AKS Engine

Para quitar el complemento de Azure Policy y Gatekeeper del clúster de AKS Engine, use el método en línea con la manera en que se ha instalado el complemento:

- Si se ha instalado definiendo la propiedad **addons** en la definición del clúster para AKS Engine:

  Vuelva a implementar la definición del clúster en AKS Engine después de cambiar la propiedad **addons** de _azure-policy_ a false:

  ```json
  "addons": [{
      "name": "azure-policy",
      "enabled": false
  }]
  ```

  Para obtener más información, vea [AKS Engine: deshabilitación del complemento de Azure Policy](https://github.com/Azure/aks-engine/blob/master/examples/addons/azure-policy/README.md#disable-azure-policy-add-on).

- Si se ha instalado con gráficos de Helm, ejecute el siguiente comando de Helm:

  ```bash
  helm uninstall azure-policy-addon
  ```

## <a name="diagnostic-data-collected-by-azure-policy-add-on"></a>Datos de diagnóstico recopilados por el complemento de Azure Policy

El complemento de Azure Policy para Kubernetes recopila datos de diagnóstico de clúster limitados. Estos datos de diagnóstico son datos técnicos esenciales relacionados con el software y el rendimiento. Se utiliza para lo siguiente:

- Mantener actualizado el complemento de Azure Policy
- Mantener la seguridad, la confiabilidad y el rendimiento del complemento de Azure Policy
- Mejorar el complemento de Azure Policy mediante el análisis agregado del uso del complemento

La información recopilada por el complemento no son datos personales. Actualmente se recopilan los detalles siguientes:

- Versión del agente del complemento de Azure Policy
- Tipo de clúster
- Región del clúster
- Grupo de recursos del clúster
- Identificador de recurso del clúster
- Identificador de suscripción del clúster
- Sistema operativo del clúster (ejemplo: Linux)
- Ciudad del clúster (ejemplo: Seattle)
- Estado o provincia del clúster (ejemplo: Washington)
- País o región del clúster (ejemplo: Estados Unidos)
- Excepciones o errores encontrados por el complemento de Azure Policy durante la instalación del agente en la evaluación de directiva
- Número de definiciones de directiva de Gatekeeper no instaladas por el complemento de Azure Policy

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md).
- Vea la [Estructura de definición de Azure Policy](definition-structure.md).
- Vea la [Descripción de los efectos de directivas](effects.md).
- Obtenga información acerca de cómo se pueden [crear directivas mediante programación](../how-to/programmatically-create.md).
- Obtenga información sobre cómo [obtener datos de cumplimiento](../how-to/get-compliance-data.md).
- Obtenga información sobre cómo [corregir recursos no compatibles](../how-to/remediate-resources.md).
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.
