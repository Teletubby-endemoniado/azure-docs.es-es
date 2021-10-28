---
title: Uso de grupos de varios nodos de Azure Kubernetes Service (AKS)
description: Aprenda a crear y administrar grupos de varios nodos para un clúster de Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 02/11/2021
ms.openlocfilehash: 6d378b9380f2aa70b88bca0c1f0aaa58f03c26ff
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130236701"
---
# <a name="create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>Creación y administración de varios grupos de nodos para un clúster de Azure Kubernetes Service (AKS)

En Azure Kubernetes Service, los nodos de la misma configuración se agrupan en *grupos de nodos*. Estos grupos de nodos contienen las máquinas virtuales subyacentes que ejecutan las aplicaciones. El número de nodos y su tamaño (SKU) inicial se definen al crear un clúster de AKS, lo cual crea un [grupo de nodos del sistema][use-system-pool]. Para admitir aplicaciones con diferentes necesidades de proceso o almacenamiento, puede crear *grupos de nodos de usuario*. Los grupos de nodos del sistema tienen el propósito principal de hospedar los pods críticos del sistema, como CoreDNS y tunnelfront. Los grupos de nodos de usuario tienen el propósito principal de hospedar los pods de aplicación. Sin embargo, se pueden programar pods de aplicación en grupos de nodos del sistema si quiere tener solo un grupo en el clúster de AKS. En los grupos de nodos del usuario es donde se colocan los pods específicos de la aplicación. Por ejemplo, puede usar estos grupos de nodos de usuario adicionales para proporcionar GPU para aplicaciones de proceso intensivo o acceso a almacenamiento SSD de alto rendimiento.

> [!NOTE]
> Esta característica le permite obtener un mayor control sobre cómo crear y administrar varios grupos de nodos. Como resultado, se requieren comandos separados para crear, actualizar y eliminar elementos. Anteriormente, las operaciones de clúster a través de `az aks create` o `az aks update` usaban la API managedCluster y eran la única opción para cambiar el plano de control y un grupo de nodo único. Esta característica expone un conjunto de operaciones independiente para los grupos de agentes a través de la API agentPool y requiere el uso del conjunto de comandos `az aks nodepool` para ejecutar operaciones en un grupo de nodos individual.

Este artículo le muestra cómo crear y administrar grupos de varios nodos en un clúster de AKS.

## <a name="before-you-begin"></a>Antes de empezar

Es preciso que esté instalada y configurada la versión 2.2.0 de la CLI de Azure, o cualquier otra posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][install-azure-cli].

## <a name="limitations"></a>Limitaciones

Se aplican las siguientes limitaciones cuando crea y administra clústeres de AKS que admiten varios grupos de nodos:

* Consulte [Cuotas, restricciones de tamaño de máquinas virtuales y disponibilidad de regiones en Azure Kubernetes Service (AKS)][quotas-skus-regions].
* Puede eliminar grupos de nodos del sistema, siempre que disponga de otro grupo de nodos del sistema que ocupe su lugar en el clúster de AKS.
* Los grupos del sistema deben contener al menos un nodo mientras que los grupos de nodos del usuario pueden contener varios nodos o ninguno.
* El clúster de AKS debe usar el equilibrador de carga de SKU estándar para usar varios grupos de nodos; la característica no es compatible con los equilibradores de carga de SKU básica.
* El clúster de AKS debe usar conjuntos de escalado de máquinas virtuales para los nodos.
* El nombre de un grupo de nodos solo puede contener caracteres alfanuméricos en minúsculas y debe comenzar con una letra minúscula. En el caso de los grupos de nodos de Linux, la longitud debe estar comprendida entre 1 y 12 caracteres. Para los grupos de nodos de Windows, la longitud debe estar comprendida entre 1 y 6 caracteres.
* Todos los grupos de nodos deben residir en la misma red virtual.
* Al crear varios grupos de nodos durante la creación del clúster, todas las versiones de Kubernetes que se usen en los grupos de nodos deben coincidir con la versión establecida para el plano de control. Se puede actualizar después de aprovisionar el clúster mediante operaciones en función del grupo de nodos.

## <a name="create-an-aks-cluster"></a>Creación de un clúster de AKS

> [!Important]
> Si ejecuta un único grupo de nodos del sistema para el clúster de AKS en un entorno de producción, se recomienda usar al menos tres nodos para el grupo de nodos.

Para empezar, cree un clúster de AKS con un grupo de nodo único. El ejemplo siguiente usa el comando [az group create][az-group-create] para crear un grupo de recursos denominado *myResourceGroup* en la región *eastus*. Se crea un clúster de AKS denominado *myAKSCluster* mediante el comando [az aks create][az-aks-create].

> [!NOTE]
> **No se admite** la SKU *Básico* del equilibrador de carga cuando se usan varios grupos de nodos. De forma predeterminada, los clústeres de AKS se crean con el SKU de equilibrador de carga *Estándar* de la CLI de Azure y Azure Portal.

```azurecli-interactive
# Create a resource group in East US
az group create --name myResourceGroup --location eastus

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --vm-set-type VirtualMachineScaleSets \
    --node-count 2 \
    --generate-ssh-keys \
    --load-balancer-sku standard
```

La operación de creación del clúster tarda unos minutos.

> [!NOTE]
> Para asegurarse de que el clúster funciona de forma confiable, debe ejecutar al menos dos nodos en el grupo de nodos predeterminado, ya que los servicios esenciales del sistema se ejecutan en este grupo de nodos.

Cuando el clúster esté listo, utilice el comando [az aks get-credentials][az-aks-get-credentials] para obtener las credenciales del clúster para su uso con `kubectl`:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>Adición de un grupo de nodos

El clúster creado en el paso anterior tiene un grupo de nodo único. Vamos a agregar un segundo grupo de nodos mediante el comando [az aks node pool add][az-aks-nodepool-add]. En el ejemplo siguiente se crea un grupo de nodos denominado *mynodepool* que ejecuta *3* nodos:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3
```

> [!NOTE]
> El nombre de un grupo de nodos debe empezar con una letra minúscula y solo puede contener caracteres alfanuméricos. En el caso de los grupos de nodos de Linux, la longitud debe estar comprendida entre 1 y 12 caracteres. Para los grupos de nodos de Windows, la longitud debe estar comprendida entre 1 y 6 caracteres.

Para ver el estado de los grupos de nodos, use el comando [az aks node pool list][az-aks-nodepool-list] y especifique el nombre del clúster y del grupo de recursos:

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster
```

En la siguiente salida de ejemplo se puede ver que *mynodepool* se ha creado correctamente con tres nodos en el grupo de nodos. Cuando se creó el clúster de AKS en el paso anterior, también se creó un grupo de nodos predeterminado denominado *nodepool1* con *2* nodos.

```output
[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.15.7",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.15.7",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

> [!TIP]
> Si no se especifica *VmSize* al agregar un grupo de nodos, el tamaño predeterminado será *Standard_D2s_v3* para los grupos de nodos de Windows y *Standard_DS2_v2* para los grupos de nodos de Linux. Si no se especifica *OrchestratorVersion*, se establecerá de forma predeterminada en la misma versión que el plano de control.

### <a name="add-a-node-pool-with-a-unique-subnet-preview"></a>Adición de un grupo de nodos con una subred única (versión preliminar)

Una carga de trabajo puede requerir la división de los nodos de un clúster en grupos independientes para el aislamiento lógico. Este aislamiento se puede admitir con subredes independientes dedicadas a cada grupo de nodos del clúster. Esto puede satisfacer requisitos tales como tener un espacio de direcciones de red virtual no contiguo para dividirlo en grupos de nodos.

#### <a name="limitations"></a>Limitaciones

* Todas las subredes asignadas a grupos de nodos deben pertenecer a la misma red virtual.
* Los pods del sistema deben tener acceso a todos los nodos o pods del clúster para proporcionar funcionalidad crítica, como la resolución de DNS y la tunelización de logs/exec/port-forward proxy de kubectl.
* Si expande la red virtual después de crear el clúster, debe actualizarlo (es decir, debe realizar cualquier operación de clúster administrado, aunque no cuentan las operaciones del grupo de nodos) antes de agregar una subred fuera del CIDR original. AKS generará un error con la opción Agregar ahora en el grupo de agentes aunque originalmente se hubiera permitido. Si no sabe cómo conciliar el clúster, abra una incidencia de soporte técnico. 
* No se admite la directiva de red de Calico. 
* No se admite la directiva de red de Azure.
* Kube-proxy espera un único CIDR contiguo y lo usa para tres optimizaciones. Consulte esta [propuesta de mejora de Kubernetes](https://github.com/kubernetes/enhancements/tree/master/keps/sig-network/2450-Remove-knowledge-of-pod-cluster-CIDR-from-iptables-rules) y -cluster-cidr [aquí](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/) para obtener más información. En Azure CNI, la subred del primer grupo de nodos se asignará a kube-proxy. 

Para crear un grupo de nodos con una subred dedicada, pase el identificador de recurso de subred como parámetro adicional al crear un grupo de nodos.

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3 \
    --vnet-subnet-id <YOUR_SUBNET_RESOURCE_ID>
```

## <a name="upgrade-a-node-pool"></a>Actualización de un grupo de nodos

> [!NOTE]
> Las operaciones de actualización y escalado en un grupo de clústeres o nodos no se pueden realizar simultáneamente. Si se intenta, se devuelve un error. En su lugar, cada tipo de operación debe completarse en el recurso de destino antes de la siguiente solicitud en ese mismo recurso. Obtenga más información al respecto en nuestra [guía de solución de problemas](./troubleshooting.md#im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-upgraded-or-has-failed-upgrade).

Los comandos de esta sección explican cómo actualizar un único grupo de nodos específico. La relación entre actualizar la versión de Kubernetes del plano de control y el grupo de nodos se explica en la [sección que tiene a continuación](#upgrade-a-cluster-control-plane-with-multiple-node-pools).

> [!NOTE]
> La versión de la imagen del sistema operativo del grupo de nodos está vinculada a la versión de Kubernetes del clúster. Solo obtendrá actualizaciones de la imagen del sistema operativo cuando se haya realizado una actualización del clúster.

Como en este ejemplo hay dos grupos de nodos, debemos usar [az aks nodepool upgrade][az-aks-nodepool-upgrade] para actualizar un grupo de nodos. Para ver las actualizaciones disponibles, use [az aks get-upgrades][az-aks-get-upgrades]

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

Vamos a actualizar *mynodepool*. Use el comando [az aks nodepool upgrade][az-aks-nodepool-upgrade] para actualizar el grupo de nodos tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version KUBERNETES_VERSION \
    --no-wait
```

Muestre el estado de los grupos de nodos de nuevo mediante el comando [az aks node pool list][az-aks-nodepool-list]. En el ejemplo siguiente, se muestra que *mynodepool* se encuentra en el estado *Actualizando* a *KUBERNETES_VERSION*:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 3,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "KUBERNETES_VERSION",
    ...
    "provisioningState": "Upgrading",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Tarda unos minutos en actualizar los nodos a la versión especificada.

Se recomienda que actualice todos los grupos de nodos de un clúster de AKS a la misma versión de Kubernetes. El comportamiento predeterminado de `az aks upgrade` es actualizar todos los grupos de nodos junto con el plano de control para lograr esta alineación. La posibilidad de actualizar grupos de nodos individuales le permite realizar una actualización gradual y programar pods entre grupos de nodos para mantener el tiempo de actividad de las aplicaciones dentro de las restricciones antes mencionadas.

## <a name="upgrade-a-cluster-control-plane-with-multiple-node-pools"></a>Actualización del plano de control de un clúster con varios grupos de nodos

> [!NOTE]
> Kubernetes usa el esquema de versiones estándar de [Versionamiento Semántico](https://semver.org/). El número de versión se expresa como *x.y.z*, donde *x* es la versión principal, *y* es la versión secundaria y *z* es la versión de revisión. Por ejemplo, en la versión *1.12.6*, 1 es la versión principal, 12 es la versión secundaria y 6 es la versión de revisión. Las versiones de Kubernetes del plano de control y del grupo de nodos inicial se establecen durante la creación del clúster. Todos los grupos de nodos adicionales tienen establecida la versión de Kubernetes cuando se agregan al clúster. Las versiones de Kubernetes pueden diferir entre los grupos de nodos, así como entre un grupo de nodos y el plano de control.

Un clúster de AKS tiene dos objetos de recursos de clúster con las versiones de Kubernetes asociadas.

1. Un plano de control del clúster con una versión de Kubernetes.
2. Un grupo de nodos con una versión de Kubernetes.

Un plano de control se asigna a uno o varios grupos de nodos. El comportamiento de una operación de actualización depende del comando de la CLI de Azure que se use.

La actualización de un plano de control de AKS requiere el uso de `az aks upgrade`. Este comando actualiza la versión del plano de control y todos los grupos de nodos del clúster.

Si se emite el comando `az aks upgrade` con la marca `--control-plane-only`, solo se actualiza el plano de control del clúster. No se cambia ninguno de los grupos de nodos asociados del clúster.

La actualización de los grupos de nodos individuales requiere el uso de `az aks nodepool upgrade`. Este comando actualiza solo el grupo de nodos de destino con la versión de Kubernetes especificada.

### <a name="validation-rules-for-upgrades"></a>Reglas de validación para actualizaciones

Las actualizaciones válidas de Kubernetes del plano de control o de los grupos de nodos de un clúster se validan mediante los siguientes conjuntos de reglas.

* Reglas de versiones válidas para actualizar grupos de nodos:
   * La versión del grupo de nodos debe tener la misma versión *principal* que el plano de control.
   * La versión del grupo de nodos *secundaria* debe estar dentro de dos versiones *secundarias* de la versión del plano de control.
   * La versión del grupo de nodos no puede ser mayor que la versión `major.minor.patch` de control.

* Reglas para enviar una operación de actualización:
   * No se puede cambiar a la versión anterior de Kubernetes en el plano de control ni en el grupo de nodos.
   * Si no se especifica una versión de Kubernetes del grupo de nodos, el comportamiento depende del cliente que se use. La declaración en las plantillas de Resource Manager revierte a versión existente definida para el grupo de nodos. Si no se establece ninguna, se usa la versión del plano de control para la reversión.
   * Puede actualizar o escalar un plano de control o un grupo de nodos en un momento dado, pero no puede enviar simultáneamente varias operaciones en un único recurso de plano de control o de grupo de nodos.

## <a name="scale-a-node-pool-manually"></a>Escalado manual de un grupo de nodos

A medida que la carga de trabajo de las aplicaciones cambia, puede que tenga que escalar el número de nodos de un grupo de nodos. El número de nodos se puede escalar o reducir verticalmente.

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

Para escalar el número de nodos de un grupo de nodos, use el comando [az aks node pool scale][az-aks-nodepool-scale]. En el ejemplo siguiente se escala el número de nodos de *mynodepool* a *5*:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

Muestre el estado de los grupos de nodos de nuevo mediante el comando [az aks node pool list][az-aks-nodepool-list]. El ejemplo siguiente muestra que *mynodepool* está en el estado *Escalando* con un nuevo recuento de *5* nodos:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Scaling",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

La operación de escalado tarda unos minutos en completarse.

## <a name="scale-a-specific-node-pool-automatically-by-enabling-the-cluster-autoscaler"></a>Escalar un grupo de nodos específico automáticamente habilitando para ello la escalabilidad automática del clúster

AKS ofrece una característica independiente para escalar automáticamente grupos de nodos con una característica denominada [cluster autocaler](cluster-autoscaler.md). Esta característica se puede habilitar en función del grupo de nodos con recuentos de escala mínimos y máximos únicos por grupo de nodos. Aprenda a [ usar la escalabilidad automática del clúster en función del grupo de nodos](cluster-autoscaler.md#use-the-cluster-autoscaler-with-multiple-node-pools-enabled).

## <a name="delete-a-node-pool"></a>Eliminación de un grupo de nodos

Si ya no necesita un grupo, puede eliminarlo y quitar los nodos de máquinas virtuales subyacentes. Para eliminar un grupo de nodos, use el comando [az aks node pool delete][az-aks-nodepool-delete] y especifique el nombre del grupo de nodos. El ejemplo siguiente elimina el grupo *mynoodepool* que se creó en los pasos anteriores:

> [!CAUTION]
> No hay ninguna opción de recuperación para la pérdida de datos que se puede producir cuando se elimina un grupo de nodos. Si no se pueden programar pods en otros grupos de nodos, esas aplicaciones dejarán de estar disponibles. Asegúrese de que no elimina un grupo de nodos si las aplicaciones en uso no disponen de copias de seguridad de los datos o de la posibilidad de ejecutarse en otros grupos de nodos del clúster.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

En la siguiente salida de ejemplo del comando [az aks node pool list][az-aks-nodepool-list] se puede ver que *mynodepool* se encuentra en el estado *Eliminando*:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 5,
    ...
    "name": "mynodepool",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Deleting",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Tarda unos minutos en eliminar los nodos y el grupo de nodos.

## <a name="specify-a-vm-size-for-a-node-pool"></a>Especificación de un tamaño de máquina virtual para un grupo de nodos

En los ejemplos anteriores para crear un grupo de nodos, se usó un tamaño de máquina virtual predeterminado para los nodos creados en el clúster. Un escenario habitual consiste en crear grupos de nodos con diferentes tamaños y funcionalidades de máquina virtual. Por ejemplo, puede crear un grupo de nodos que contenga nodos con grandes cantidades de memoria o CPU, o un grupo de nodos que proporcione compatibilidad con GPU. En el paso siguiente, se [usan taints y tolerations](#setting-nodepool-taints) para indicar al programador de Kubernetes cómo limitar el acceso a los pods que se pueden ejecutar en esos nodos.

En el ejemplo siguiente, cree un grupo de nodos basado en GPU que use el tamaño de máquina virtual *Standard_NC6*. Estas máquinas virtuales disponen de una tarjeta Tesla K80 de NVIDIA. Para más información sobre los tamaños de máquina virtual disponibles, consulte [Tamaños de las máquinas virtuales Linux en Azure][vm-sizes].

Cree un grupo de nodos mediante el comando [az aks node pool add][az-aks-nodepool-add] de nuevo. Esta vez, especifique el nombre *gpunodepool* y use el parámetro `--node-vm-size` para especificar el tamaño *Standard_NC6*:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

En la siguiente salida de ejemplo del comando [az aks node pool list][az-aks-nodepool-list] se puede ver que *gpunodepool* está *Creando* nodos con el *VmSize* especificado:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 1,
    ...
    "name": "gpunodepool",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Creating",
    ...
    "vmSize": "Standard_NC6",
    ...
  },
  {
    ...
    "count": 2,
    ...
    "name": "nodepool1",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Succeeded",
    ...
    "vmSize": "Standard_DS2_v2",
    ...
  }
]
```

Tarda unos minutos en crear correctamente *gpunodepool*.

## <a name="specify-a-taint-label-or-tag-for-a-node-pool"></a>Especificación de un valor taint o una etiqueta para un grupo de nodos

Al crear un grupo de nodos, puede agregar valores taint o etiquetas a ese grupo de nodos. Al agregar un valor taint o una etiqueta, todos los nodos de ese grupo de nodos también obtienen ese valor taint o etiqueta.

> [!IMPORTANT]
> Se deben agregar valores taint o etiquetas a los nodos para todo el grupo de nodos mediante `az aks nodepool`. No se recomienda aplicar valores taint o etiquetas a nodos individuales de un grupo de nodos mediante `kubectl`.  

### <a name="setting-nodepool-taints"></a>Configuración de las intolerancias de un grupo de nodos

Para crear un grupo de nodos con un valor taint, use [az aks nodepool add][az-aks-nodepool-add]. Especifique el nombre *taintnp* y use el parámetro `--node-taints` para especificar *sku=gpu:NoSchedule* para el valor taint.

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name taintnp \
    --node-count 1 \
    --node-taints sku=gpu:NoSchedule \
    --no-wait
```

> [!NOTE]
> Solo pueden crearse intolerancias para los grupos de nodos cuando estos se crean.

En la siguiente salida de ejemplo del comando [az aks nodepool list][az-aks-nodepool-list] se puede ver que *taintnp* está creando (*Creating*) nodos con el valor *nodeTaints* especificado:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 1,
    ...
    "name": "taintnp",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Creating",
    ...
    "nodeTaints":  [
      "sku=gpu:NoSchedule"
    ],
    ...
  },
 ...
]
```

La información del valor taint está visible en Kubernetes para controlar las reglas de programación de los nodos. El programador de Kubernetes puede utilizar taints y tolerations para limitar las cargas de trabajo que se pueden ejecutar en los nodos.

* Un valor **taint** se aplica a un nodo que indica que solo se pueden programar pods específicos en él.
* Luego se aplica un valor **toleration** a un pod que le permite *tolerar* el valor taint de un nodo.

Para más información sobre cómo usar las características avanzadas programadas de Kubernetes, consulte [Best practices for advanced scheduler features in AKS][taints-tolerations] (Procedimientos recomendados para las características avanzadas del programador en AKS).

En el paso anterior, aplicó el valor taint *sku=gpu:NoSchedule* cuando creó el grupo de nodos. El siguiente manifiesto YAML básico de ejemplo usa un valor toleration para permitir al programador de Kubernetes ejecutar un pod NGINX en un nodo de ese grupo de nodos.

Cree un archivo denominado `nginx-toleration.yaml` y cópielo en el ejemplo siguiente de YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: mcr.microsoft.com/oss/nginx/nginx:1.15.9-alpine
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Programe el pod mediante el comando `kubectl apply -f nginx-toleration.yaml`:

```console
kubectl apply -f nginx-toleration.yaml
```

Se tarda unos segundos en programar el pod y extraer la imagen NGINX. Use el comando [kubectl describe pod][kubectl-describe] para ver el estado del pod. En la siguiente salida de ejemplo reducida se puede ver que se aplica el valor *sku=gpu:NoSchedule* a toleration. En la sección de eventos, el programador ha asignado el pod al nodo *aks-taintnp-28993262-vmss000000*:

```console
kubectl describe pod mypod
```

```output
[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                Message
  ----    ------     ----   ----                -------
  Normal  Scheduled  4m48s  default-scheduler   Successfully assigned default/mypod to aks-taintnp-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet             pulling image "mcr.microsoft.com/oss/nginx/nginx:1.15.9-alpine"
  Normal  Pulled     4m43s  kubelet             Successfully pulled image "mcr.microsoft.com/oss/nginx/nginx:1.15.9-alpine"
  Normal  Created    4m40s  kubelet             Created container
  Normal  Started    4m40s  kubelet             Started container
```

Solo los pods a los que se ha aplicado este valor toleration se pueden programar en los nodos de *taintnp*. Cualquier otro pod se tendría que programar en el grupo de nodos *nodepool1*. Si crea grupos de nodos adicionales, puede usar valores taint y toleration adicionales para limitar los pods que se pueden programar en esos recursos del nodo.

### <a name="setting-nodepool-labels"></a>Configuración de etiquetas de grupos de nodos

También puede agregar etiquetas a un grupo de nodos durante la creación del mismo. Las etiquetas establecidas en el grupo de nodos se agregan a cada nodo del grupo de nodos. Estas [etiquetas están visibles en Kubernetes][kubernetes-labels] para controlar las reglas de programación de los nodos.

Para crear un grupo de nodos con una etiqueta, use [az aks nodepool add][az-aks-nodepool-add]. Especifique el nombre *labelnp* y use el parámetro `--labels` para especificar *dept=IT* y *costcenter=9999* para las etiquetas.

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name labelnp \
    --node-count 1 \
    --labels dept=IT costcenter=9999 \
    --no-wait
```

> [!NOTE]
> La etiqueta solo se puede establecer para los grupos de nodos durante la creación de estos. Las etiquetas también deben ser un par clave-valor y tener una [sintaxis válida][kubernetes-label-syntax].

En la siguiente salida de ejemplo del comando [az aks nodepool list][az-aks-nodepool-list] se puede ver que *labelnp* está creando (*Creating*) nodos con el valor *nodeLabels* especificado:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster

[
  {
    ...
    "count": 1,
    ...
    "name": "labelnp",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Creating",
    ...
    "nodeLabels":  {
      "dept": "IT",
      "costcenter": "9999"
    },
    ...
  },
 ...
]
```

### <a name="setting-nodepool-azure-tags"></a>Configuración de etiquetas de Azure para grupos de nodos

Puede aplicar una etiqueta de Azure a grupos de nodos en su clúster de AKS. Las etiquetas aplicadas a un grupo de nodos se aplican a cada nodo del grupo y se conservan de una actualización a otra. También se aplican etiquetas a los nuevos nodos que se agregan a un grupo durante las operaciones de escalado horizontal. Agregar una etiqueta puede ayudar con tareas como el seguimiento de directivas o la estimación de costos.

Las etiquetas de Azure tienen claves que no distinguen mayúsculas y minúsculas en las operaciones; por ejemplo, cuando se recupera una etiqueta buscando la clave. En este caso, una etiqueta con la clave especificada se actualizará o recuperará independientemente del uso de mayúsculas y minúsculas. Los valores de etiqueta distinguen mayúsculas de minúsculas.

En AKS, si varias etiquetas están configuradas con claves que son idénticas salvo por el uso de mayúsculas y minúsculas, la etiqueta que se utilizará será la primera en orden alfabético. Por ejemplo, si se utiliza `{"Key1": "val1", "kEy1": "val2", "key1": "val3"}`, se configurará `Key1` y `val1`.

Cree un grupo de nodos mediante el comando [az aks nodepool add][az-aks-nodepool-add]. Especifique el nombre *tagnodepool* y use el parámetro `--tag` para especificar *dept=IT* y *costcenter=9999* para las etiquetas.

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name tagnodepool \
    --node-count 1 \
    --tags dept=IT costcenter=9999 \
    --no-wait
```

> [!NOTE]
> También puede usar el parámetro `--tags` cuando use el comando [az aks nodepool update][az-aks-nodepool-update], y también durante la creación del clúster. Durante la creación del clúster, el parámetro `--tags` aplica la etiqueta al grupo de nodos inicial que se crea con el clúster. Todos los nombres de etiqueta deben cumplir las limitaciones de [Uso de etiquetas para organizar los recursos de Azure][tag-limitation]. Al actualizar un grupo de nodos con el parámetro `--tags`, se actualizan los valores de etiqueta existentes y se anexan las etiquetas nuevas. Por ejemplo, si el grupo de nodos tuviera *dept=IT* y *costcenter=9999* en las etiquetas y lo actualizara a *team=dev* y *costcenter=111*, el grupo de nodos tendría *dept=IT*, *costcenter=111* y *team=dev* como etiquetas.

En la siguiente salida de ejemplo del comando [az aks nodepool list][az-aks-nodepool-list] se puede ver que *tagnodepool* está creando (*Creating*) nodos con el valor de *tag* especificado:

```azurecli
az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster
```

```output
[
  {
    ...
    "count": 1,
    ...
    "name": "tagnodepool",
    "orchestratorVersion": "1.15.7",
    ...
    "provisioningState": "Creating",
    ...
    "tags": {
      "dept": "IT",
      "costcenter": "9999"
    },
    ...
  },
 ...
]
```

## <a name="add-a-fips-enabled-node-pool-preview"></a>Adición de un grupo de nodos habilitado para FIPS (versión preliminar)

El Estándar federal de procesamiento de información (FIPS) 140-2 es un estándar del gobierno de EE. UU. que define los requisitos mínimos de seguridad para los módulos criptográficos en sistemas y productos de tecnologías de la información. AKS permite crear grupos de nodos basados en Linux con FIPS 140-2 habilitado. Las implementaciones que se ejecutan en grupos de nodos habilitados para FIPS pueden usar esos módulos criptográficos para proporcionar mayor seguridad y ayudar a cumplir los controles de seguridad como parte del cumplimiento de FedRAMP. Para más información sobre FIPS 140-2, consulte [Estándar federal de procesamiento de información (FIPS) 140-2][fips].

Los grupos de nodos con el estándar FIPS habilitado están actualmente en versión preliminar.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

Necesitará la versión *0.5.11* de la extensión *aks-preview* de la CLI de Azure. Instale la extensión de la CLI de Azure *aks-preview* mediante el comando [az extension add][az-extension-add]. También puede instalar las actualizaciones disponibles mediante el comando [az extension update][az-extension-update].

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

Para usar la característica, también debe habilitar la marca de característica `FIPSPreview` en la suscripción.

Registre la marca de la característica `FIPSPreview` con el comando [az feature register][az-feature-register], como se muestra en el siguiente ejemplo:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "FIPSPreview"
```

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado de registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/FIPSPreview')].{Name:name,State:properties.state}"
```

Cuando haya terminado, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

Los grupos de nodos con FIPS habilitado tienen las siguientes limitaciones:

* Actualmente, solo puede tener grupos de nodos basados en Linux con FIPS habilitado que se ejecutan en Ubuntu 18.04.
* Los grupos de nodos con FIPS habilitado requieren la versión 1.19 de Kubernetes, y cualquier versión posterior.
* Para actualizar los paquetes o módulos subyacentes que se usan para FIPS, debe utilizar [Node Image Upgrade][node-image-upgrade].

> [!IMPORTANT]
> La imagen de Linux con FIPS habilitado es una imagen diferente de la imagen de Linux predeterminada que se usa para los grupos de nodos basados en Linux. Para habilitar FIPS en un grupo de nodos, debe crear un grupo de nodos basado en Linux. FIPS no se puede habilitar en grupos de nodos existentes.
> 
> Las imágenes de nodos con FIPS habilitado pueden tener números de versión diferentes, como la versión del kernel, que las imágenes que no están habilitadas para FIPS. Además, el ciclo de actualización de los grupos de nodos con FIPS habilitado y las imágenes de nodo pueden diferir de los grupos de nodos y las imágenes que no tengan FIPS habilitado.

Para crear un grupo de nodos con FIPS habilitado, use [az aks nodepool add][az-aks-nodepool-add] con el parámetro *--enable-fips-image* al crear un grupo de nodos.

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name fipsnp \
    --enable-fips-image
```

> [!NOTE]
> También puede usar el *parámetro --enable-fips-image* con [az aks create][az-aks-create] al crear un clúster para habilitar FIPS en el grupo de nodos predeterminado. Al agregar grupos de nodos a un clúster creado de esta forma, debe seguir utilizando el parámetro *--enable-fips-image* al agregar grupos de nodos para crear un grupo de nodos con FIPS habilitado.

Para comprobar que el grupo de nodos tiene FIPS habilitado, use [az aks show][az-aks-show] para comprobar el valor *enableFIPS* en *agentPoolProfiles*.

```azurecli-interactive
az aks show --resource-group myResourceGroup --cluster-name myAKSCluster --query="agentPoolProfiles[].{Name:name enableFips:enableFips}" -o table
```

La siguiente salida de ejemplo muestra que el grupo de nodos *fipsnp* tiene FIPS habilitado, pero *nodepool1* no.

```output
Name       enableFips
---------  ------------
fipsnp     True
nodepool1  False  
```

También puede comprobar que las implementaciones tienen acceso a las bibliotecas criptográficas de FIPS mediante `kubectl debug` en un nodo del grupo de nodos con FIPS habilitado. Use `kubectl get nodes` para enumerar los nodos:

```output
$ kubectl get nodes
NAME                                STATUS   ROLES   AGE     VERSION
aks-fipsnp-12345678-vmss000000      Ready    agent   6m4s    v1.19.9
aks-fipsnp-12345678-vmss000001      Ready    agent   5m21s   v1.19.9
aks-fipsnp-12345678-vmss000002      Ready    agent   6m8s    v1.19.9
aks-nodepool1-12345678-vmss000000   Ready    agent   34m     v1.19.9
```

En el ejemplo anterior, los nodos que empiezan por `aks-fipsnp` forman parte del grupo de nodos con FIPS habilitado. Use `kubectl debug` para ejecutar una implementación con una sesión interactiva en uno de los nodos del grupo de nodos con FIPS habilitado.

```azurecli-interactive
kubectl debug node/aks-fipsnp-12345678-vmss000000 -it --image=mcr.microsoft.com/aks/fundamental/base-ubuntu:v0.0.11
```

Desde la sesión interactiva, puede comprobar que las bibliotecas criptográficas de FIPS están habilitadas:

```output
root@aks-fipsnp-12345678-vmss000000:/# cat /proc/sys/crypto/fips_enabled
1
```

Los grupos de nodos con FIPS habilitado también tienen la etiqueta *kubernetes.azure.com/fips_enabled=true*, que las implementaciones pueden usar para dirigirse a esos grupos de nodos.

## <a name="manage-node-pools-using-a-resource-manager-template"></a>Administración de grupos de nodos con una plantilla de Resource Manager

Cuando usa una plantilla de Azure Resource Manager para crear y administrar recursos, normalmente puede actualizar la configuración de la plantilla y volver a implementarla para actualizar el recurso. Con grupos de nodos de AKS, el perfil del grupo de nodos inicial no se podrá actualizar una vez que se haya creado el clúster de AKS. Este comportamiento significa que no se puede actualizar una plantilla de Resource Manager existente, realizar un cambio en los grupos de nodos y volver a implementar. En su lugar, debe crear una plantilla de Resource Manager independiente que actualice solo los grupos de nodos de un clúster de AKS existente.

Cree una plantilla como `aks-agentpools.json` y pegue el siguiente ejemplo de manifiesto. Esta plantilla de ejemplo configura los valores siguientes:

* Actualiza el grupo de nodos de *Linux* denominado *myagentpool* para que ejecute tres nodos.
* Establece los nodos del grupo para ejecutar la versión de Kubernetes *1.15.7*.
* Define el tamaño del nodo como *Standard_DS2_v2*.

Edite estos valores para actualizar, agregar o eliminar grupos de nodos según sea necesario:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description&quot;: &quot;The name of your existing AKS cluster."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description&quot;: &quot;The location of your existing AKS cluster."
            }
        },
        "agentPoolName": {
            "type": "string",
            "defaultValue": "myagentpool",
            "metadata": {
                "description&quot;: &quot;The name of the agent pool to create or update."
            }
        },
        "vnetSubnetId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description&quot;: &quot;The Vnet subnet resource ID for your existing AKS cluster."
            }
        }
    },
    "variables": {
        "apiVersion": {
            "aks&quot;: &quot;2020-01-01"
        },
        "agentPoolProfiles": {
            "maxPods": 30,
            "osDiskSizeGB": 0,
            "agentCount": 3,
            "agentVmSize": "Standard_DS2_v2",
            "osType": "Linux",
            "vnetSubnetId&quot;: &quot;[parameters('vnetSubnetId')]"
        }
    },
    "resources": [
        {
            "apiVersion": "2020-01-01",
            "type": "Microsoft.ContainerService/managedClusters/agentPools",
            "name": "[concat(parameters('clusterName'),'/', parameters('agentPoolName'))]",
            "location": "[parameters('location')]",
            "properties": {
                "maxPods": "[variables('agentPoolProfiles').maxPods]",
                "osDiskSizeGB": "[variables('agentPoolProfiles').osDiskSizeGB]",
                "count": "[variables('agentPoolProfiles').agentCount]",
                "vmSize": "[variables('agentPoolProfiles').agentVmSize]",
                "osType": "[variables('agentPoolProfiles').osType]",
                "storageProfile": "ManagedDisks",
                "type": "VirtualMachineScaleSets",
                "vnetSubnetID": "[variables('agentPoolProfiles').vnetSubnetId]",
                "orchestratorVersion": "1.15.7"
            }
        }
    ]
}
```

Implemente esta plantilla mediante el comando [az deployment group create][az-deployment-group-create], tal como se muestra en el ejemplo siguiente. Se le solicitará el nombre y la ubicación del clúster de AKS existente:

```azurecli-interactive
az deployment group create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

> [!TIP]
> Para agregar una etiqueta al grupo de nodos, utilice la propiedad *tag* en la plantilla, tal y como se muestra en el ejemplo siguiente.
> 
> ```json
> ...
> "resources": [
> {
>   ...
>   "properties": {
>     ...
>     "tags": {
>       "name1": "val1"
>     },
>     ...
>   }
> }
> ...
> ```

Puede que tarde unos minutos en actualizarse el clúster de AKS según la configuración del grupo de nodos y las operaciones que defina en la plantilla de Resource Manager.

## <a name="assign-a-public-ip-per-node-for-your-node-pools"></a>Asignación de una dirección IP pública por cada nodo para el grupo de nodos

Los nodos de AKS no necesitan sus propias direcciones IP públicas para la comunicación. Sin embargo, los escenarios pueden requerir que los nodos de un grupo de nodos reciban sus propias direcciones IP públicas dedicadas. Un escenario común es para las cargas de trabajo de juegos, en las que se necesita una consola para tener una conexión directa a una máquina virtual en la nube para minimizar los saltos. Este escenario se puede conseguir en AKS mediante el uso de la dirección IP pública del nodo.

En primer lugar, cree un nuevo grupo de recursos.

```azurecli-interactive
az group create --name myResourceGroup2 --location eastus
```

Cree un clúster de AKS y conecte una dirección IP pública para los nodos. Cada uno de los nodos del grupo de nodos recibe una dirección IP pública única. Puede comprobarlo si examina las instancias del conjunto de escalado de máquinas virtuales.

```azurecli-interactive
az aks create -g MyResourceGroup2 -n MyManagedCluster -l eastus  --enable-node-public-ip
```

En el caso de los clústeres de AKS que ya existan, también puede agregar un nuevo grupo de nodos y asociar una dirección IP pública para los nodos.

```azurecli-interactive
az aks nodepool add -g MyResourceGroup2 --cluster-name MyManagedCluster -n nodepool2 --enable-node-public-ip
```

### <a name="use-a-public-ip-prefix"></a>Uso de un prefijo de dirección IP pública

Existen varias [ventajas al usar un prefijo de dirección IP pública][public-ip-prefix-benefits]. AKS admite el uso de direcciones de un prefijo de dirección IP pública existente para los nodos pasando el identificador de recurso con la marca `node-public-ip-prefix` al crear un nuevo clúster o agregar un grupo de nodos.

En primer lugar, cree un prefijo de dirección IP pública mediante [az network public-ip prefix create][az-public-ip-prefix-create]:

```azurecli-interactive
az network public-ip prefix create --length 28 --location eastus --name MyPublicIPPrefix --resource-group MyResourceGroup3
```

Vea la salida y tome nota de `id` para el prefijo:

```output
{
  ...
  "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup3/providers/Microsoft.Network/publicIPPrefixes/MyPublicIPPrefix",
  ...
}
```

Por último, al crear un nuevo clúster o al agregar un nuevo grupo de nodos, use la marca `node-public-ip-prefix` y pase el identificador de recurso del prefijo:

```azurecli-interactive
az aks create -g MyResourceGroup3 -n MyManagedCluster -l eastus --enable-node-public-ip --node-public-ip-prefix /subscriptions/<subscription-id>/resourcegroups/MyResourceGroup3/providers/Microsoft.Network/publicIPPrefixes/MyPublicIPPrefix
```

### <a name="locate-public-ips-for-nodes"></a>Búsqueda de direcciones IP públicas para nodos

Puede buscar las direcciones IP públicas de los nodos de varias maneras:

* Use el comando [az vmss list-instance-public-ips][az-list-ips] de la CLI de Azure.
* Use [comandos de PowerShell o Bash][vmss-commands]. 
* También puede ver las direcciones IP públicas en Azure Portal visualizando las instancias del conjunto de escalado de máquinas virtuales.

> [!Important]
> El [grupo de recursos de nodo][node-resource-group] contiene los nodos y sus direcciones IP públicas. Use el grupo de recursos de nodo al ejecutar comandos para buscar las direcciones IP públicas de los nodos.

```azurecli
az vmss list-instance-public-ips -g MC_MyResourceGroup2_MyManagedCluster_eastus -n YourVirtualMachineScaleSetName
```

## <a name="clean-up-resources"></a>Limpieza de recursos

En este artículo, ha creado un clúster de AKS que incluye nodos basados en GPU. Para reducir costos innecesarios, puede que desee eliminar *gpunodepool* o todo el clúster de AKS.

Para eliminar el grupo de nodos basado en GPU, use el comando [az aks nodepool delete][az-aks-nodepool-delete] tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

Para eliminar el clúster propiamente dicho, use el comando [az group delete][az-group-delete] para eliminar el grupo de recursos de AKS:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

También puede eliminar el clúster adicional que creó para el escenario de IP pública de los grupos de nodos.

```azurecli-interactive
az group delete --name myResourceGroup2 --yes --no-wait
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los [grupos de nodos del sistema][use-system-pool].

En este artículo ha aprendido a crear y administrar grupos de varios nodos en un clúster de AKS. Para más información sobre cómo controlar pods en grupos de nodos, consulte [Best practices for advanced scheduler features in AKS][operator-best-practices-advanced-scheduler] (Procedimientos recomendados para las características avanzadas del programador en AKS).

Para crear y usar grupos de nodos de contenedores de Windows Server, consulte [Creación de un contenedor de Windows Server en AKS][aks-windows].

Use [grupos con ubicación por proximidad][reduce-latency-ppg] para disminuir la latencia de las aplicaciones de AKS.

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubernetes-labels]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
[kubernetes-label-syntax]: https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#syntax-and-character-set

<!-- INTERNAL LINKS -->
[aks-windows]: windows-container-cli.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest&preserve-view=true#az_aks_get_credentials
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest&preserve-view=true#az_aks_create
[az-aks-get-upgrades]: /cli/azure/aks?view=azure-cli-latest&preserve-view=true#az_aks_get_upgrades
[az-aks-nodepool-add]: /cli/azure/aks/nodepool?view=azure-cli-latest&preserve-view=true#az_aks_nodepool_add
[az-aks-nodepool-list]: /cli/azure/aks/nodepool?view=azure-cli-latest&preserve-view=true#az_aks_nodepool_list
[az-aks-nodepool-update]: /cli/azure/aks/nodepool?view=azure-cli-latest&preserve-view=true#az_aks_nodepool_update
[az-aks-nodepool-upgrade]: /cli/azure/aks/nodepool?view=azure-cli-latest&preserve-view=true#az_aks_nodepool_upgrade
[az-aks-nodepool-scale]: /cli/azure/aks/nodepool?view=azure-cli-latest&preserve-view=true#az_aks_nodepool_scale
[az-aks-nodepool-delete]: /cli/azure/aks/nodepool?view=azure-cli-latest&preserve-view=true#az_aks_nodepool_delete
[az-aks-show]: /cli/azure/aks#az_aks_show
[az-extension-add]: /cli/azure/extension?view=azure-cli-latest&preserve-view=true#az_extension_add
[az-extension-update]: /cli/azure/extension?view=azure-cli-latest&preserve-view=true#az_extension_update
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
[az-group-create]: /cli/azure/group?view=azure-cli-latest&preserve-view=true#az_group_create
[az-group-delete]: /cli/azure/group?view=azure-cli-latest&preserve-view=true#az_group_delete
[az-deployment-group-create]: /cli/azure/deployment/group?view=azure-cli-latest&preserve-view=true#az_deployment_group_create
[gpu-cluster]: gpu-cluster.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[quotas-skus-regions]: quotas-skus-regions.md
[supported-versions]: supported-kubernetes-versions.md
[tag-limitation]: ../azure-resource-manager/management/tag-resources.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[vm-sizes]: ../virtual-machines/sizes.md
[use-system-pool]: use-system-pools.md
[ip-limitations]: ../virtual-network/virtual-network-ip-addresses-overview-arm#standard
[node-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[vmss-commands]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine
[az-list-ips]: /cli/azure/vmss?view=azure-cli-latest&preserve-view=true#az_vmss_list_instance_public_ips
[reduce-latency-ppg]: reduce-latency-ppg.md
[public-ip-prefix-benefits]: ../virtual-network/ip-services/public-ip-address-prefix.md
[az-public-ip-prefix-create]: /cli/azure/network/public-ip/prefix?view=azure-cli-latest&preserve-view=true#az_network_public_ip_prefix_create
[node-image-upgrade]: node-image-upgrade.md
[fips]: /azure/compliance/offerings/offering-fips-140-2