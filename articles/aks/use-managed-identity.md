---
title: Uso de identidades administradas en Azure Kubernetes Service
description: Aprenda a utilizar identidades administradas en Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 05/12/2021
ms.openlocfilehash: d3d479730b88c80c627c3e6dad2ab8f80eb3aee6
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123431737"
---
# <a name="use-managed-identities-in-azure-kubernetes-service"></a>Uso de identidades administradas en Azure Kubernetes Service

Actualmente, un clúster de Azure Kubernetes Service (AKS) (específicamente, el proveedor de nube Kubernetes) requiere una identidad para crear recursos adicionales, como equilibradores de carga y discos administrados en Azure. Esta identidad puede ser una *identidad administrada* o una *entidad de servicio*. Si usa una [entidad de servicio](kubernetes-service-principal.md), debe indicarla, o bien AKS la creará automáticamente. Si usa la identidad administrada, AKS la creará automáticamente. Llega un momento en que los clústeres que usan entidades de servicio alcanzan un estado en el que se debe renovar la entidad de servicio para mantener el clúster en funcionamiento. La administración de entidades de servicio agrega complejidad, por lo que es más fácil usar identidades administradas. Se aplican los mismos requisitos de permisos tanto en las entidades de servicio como en las identidades administradas.

Las *identidades administradas* son básicamente un contenedor relacionado con las entidades de servicio y facilitan su administración. La rotación de credenciales para MI se produce automáticamente cada 46 días según el valor predeterminado de Azure Active Directory. AKS usa tipos de identidad administrados asignados por el sistema y asignados por el usuario. Estas identidades son inmutables actualmente. Para más información, consulte el tema sobre [identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="before-you-begin"></a>Antes de empezar

Debe tener instalado el siguiente recurso:

- La CLI de Azure, versión 2.23.0 o posterior

## <a name="limitations"></a>Limitaciones

* No se admite que los inquilinos trasladen o migren los clústeres habilitados para identidades administradas.
* Si el clúster tiene `aad-pod-identity` habilitado, los pods de Identidad administrada del nodo (NMI) modifican las tablas de IP de los nodos para interceptar las llamadas que se realizan en el punto de conexión de Azure Instance Metadata. Esta configuración hace que NMI intercepte toda solicitud realizada al punto de conexión de Metadata, incluso aunque el pod no utilice `aad-pod-identity`. La CRD de AzurePodIdentityException se puede configurar para informar a `aad-pod-identity` de que las solicitudes dirigidas al punto de conexión de Metadata que se originen en un pod que coincida con las etiquetas definidas en la CRD deben pasar por el servidor proxy sin que se procesen en NMI. Los pods del sistema con la etiqueta `kubernetes.azure.com/managedby: aks` del espacio de nombres _kube-system_ deben excluirse en `aad-pod-identity` configurando la CRD de AzurePodIdentityException. Para obtener más información, consulte este artículo acerca de [cómo deshabilitar aad-pod-identity en una aplicación o pod específicos](https://azure.github.io/aad-pod-identity/docs/configure/application_exception).
  Para configurar una excepción, instale [mic-exception.yaml](https://github.com/Azure/aad-pod-identity/blob/master/deploy/infra/mic-exception.yaml).

## <a name="summary-of-managed-identities"></a>Resumen de identidades administradas

AKS usa varias identidades administradas para servicios integrados y complementos.

| Identidad                       | Nombre    | Caso de uso | Permisos predeterminados | Traiga su propia identidad
|----------------------------|-----------|----------|
| Plano de control | Nombre del clúster de AKS | La usan los componentes del plano de control de AKS para administrar los recursos del clúster, incluidos los equilibradores de carga de entrada y las IP públicas administradas de AKS, Cluster Autoscaler y los controladores CSI de discos y archivos de Azure. | Rol de colaborador para un grupo de recursos de nodo | Compatible
| Kubelet | Nombre de clúster de AKS-agentpool | Autenticación con Azure Container Registry (ACR) | N/D (para kubernetes 1.15 y versiones posteriores) | Compatible
| Complemento | AzureNPM | No se requiere ninguna identidad | N/D | No
| Complemento | Supervisión de red AzureCNI | No se requiere ninguna identidad | N/D | No
| Complemento | azure-policy (gatekeeper) | No se requiere ninguna identidad | N/D | No
| Complemento | azure-policy | No se requiere ninguna identidad | N/D | No
| Complemento | Calico | No se requiere ninguna identidad | N/D | No
| Complemento | Panel | No se requiere ninguna identidad | N/D | No
| Complemento | HTTPApplicationRouting | Administra los recursos de red necesarios | Rol de lector para grupo de recursos de nodo, rol colaborador para zona DNS | No
| Complemento | Ingresar a la puerta de enlace de aplicaciones | Administra los recursos de red necesarios| Rol de colaborador para grupo de recursos de nodo | No
| Complemento | omsagent | Se usa para enviar métricas de AKS a Azure Monitor | Supervisión del rol del publicador de métricas | No
| Complemento | Nodo virtual (ACIConnector) | Administra los recursos de red necesarios para Azure Container Instances (ACI) | Rol de colaborador para grupo de recursos de nodo | No
| Proyecto de software de código abierto | aad-pod-identity | Permite que las aplicaciones puedan acceder de manera segura a los recursos en la nube con Azure Active Directory (AAD) | N/D | Pasos para conceder el permiso en https://github.com/Azure/aad-pod-identity#role-assignment.

## <a name="create-an-aks-cluster-with-managed-identities"></a>Creación de un clúster de AKS con identidades administradas

Ahora puede crear un clúster de AKS con identidades administradas utilizando los siguientes comandos de la CLI.

En primer lugar, cree un grupo de recursos de Azure:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Después, cree un clúster de AKS:

```azurecli-interactive
az aks create -g myResourceGroup -n myManagedCluster --enable-managed-identity
```

Una vez creado el clúster, puede implementar las cargas de trabajo de la aplicación en el nuevo clúster e interactuar con él del mismo modo que con los clústeres de AKS basados en una entidad de servicio.

Por último, obtenga credenciales para acceder al clúster:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myManagedCluster
```

## <a name="update-an-aks-cluster-to-managed-identities"></a>Actualización de un clúster de AKS a identidades administradas

Ahora puede actualizar un clúster de AKS que trabaja actualmente con entidades de servicio para trabajar con identidades administradas mediante los siguientes comandos de la CLI.

```azurecli-interactive
az aks update -g <RGName> -n <AKSName> --enable-managed-identity
```
> [!NOTE]
> Después de la actualización, el plano de control del clúster y los pods de complemento cambiarán para usar la identidad administrada, pero kubelet SEGUIRÁ USANDO LA ENTIDAD DE SERVICIO hasta que se actualice el grupo de agentes. Utilice `az aks nodepool upgrade --node-image-only` en los nodos para completar la actualización de la identidad administrada. 


> Si el clúster usaba --attach-acr para extraer de la imagen de ACR, después de actualizar el clúster a Identidad administrada, debe volver a ejecutar "az aks update --attach-acr <ACR Resource ID>" para permitir que el kubelet recién creado que se usa para la identidad administrada obtenga el permiso para la extracción desde ACR. De lo contrario, no podrá realizar la extracción desde ACR después de la actualización.


## <a name="obtain-and-use-the-system-assigned-managed-identity-for-your-aks-cluster"></a>Obtención y uso de la identidad administrada asignada por el sistema para el clúster de AKS

Confirme que el clúster de AKS usa la identidad administrada con el siguiente comando de la CLI:

```azurecli-interactive
az aks show -g <RGName> -n <ClusterName> --query "servicePrincipalProfile"
```

Si el clúster usa identidades administradas, verá un valor `clientId` de "msi". Un clúster que use una entidad de servicio mostrará en su lugar el identificador de objeto. Por ejemplo: 

```output
{
  "clientId": "msi"
}
```

Después de comprobar que el clúster usa identidades administradas, puede encontrar el identificador de objeto de la identidad asignada por el sistema del plano de control con el siguiente comando:

```azurecli-interactive
az aks show -g <RGName> -n <ClusterName> --query "identity"
```

```output
{
    "principalId": "<object-id>",
    "tenantId": "<tenant-id>",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
},
```

> [!NOTE]
> Para crear y usar su propia red virtual, una dirección IP estática o un disco de Azure conectado en el que los recursos estén fuera del grupo de recursos del nodo de trabajo, use el PrincipalID del sistema de clúster asignado a la identidad administrada para realizar una asignación de roles. Para obtener más información sobre la asignación, consulte [Delegación del acceso a otros recursos de Azure](kubernetes-service-principal.md#delegate-access-to-other-azure-resources).
>
> Las concesiones de permisos a la identidad administrada de clúster que el proveedor en la nube de Azure utiliza pueden tardar hasta 60 minutos en rellenarse.


## <a name="bring-your-own-control-plane-mi"></a>Traer su propia instancia administrada de plano de control
Una identidad de plano de control personalizado permite que se conceda acceso a la identidad existente antes de la creación del clúster. Esta característica permite escenarios como el uso de una red virtual personalizada o outboundType de UDR con una identidad administrada previamente creada.

Debe tener instalada la versión 2.15.1 de la CLI de Azure o una versión posterior.

### <a name="limitations"></a>Limitaciones
* Actualmente no se admiten Departamento de Defensa de centro de EE. UU., Departamento de Defensa de este de EE. UU. y USGov Iowa en Azure Government.

Si aún no tiene una identidad administrada, debería continuar y crear una, por ejemplo, mediante [az identity CLI][az-identity-create].

```azurecli-interactive
az identity create --name myIdentity --resource-group myResourceGroup
```
El resultado debería tener este aspecto:

```output
{                                                                                                                                                                                 
  "clientId": "<client-id>",
  "clientSecretUrl": "<clientSecretUrl>",
  "id": "/subscriptions/<subscriptionid>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myIdentity", 
  "location": "westus2",
  "name": "myIdentity",
  "principalId": "<principalId>",
  "resourceGroup": "myResourceGroup",                       
  "tags": {},
  "tenantId": "<tenant-id>>",
  "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

Si la identidad administrada forma parte de su suscripción, puede usar el [comando az identity CLI][az-identity-list] para consultarla.  

```azurecli-interactive
az identity list --query "[].{Name:name, Id:id, Location:location}" -o table
```

Ahora puede usar el siguiente comando para crear el clúster con la identidad existente:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myManagedCluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --enable-managed-identity \
    --assign-identity <identity-id>
```

La creación correcta de un clúster con sus propias identidades administradas contiene esta información de perfil de userAssignedIdentities:

```output
 "identity": {
   "principalId": null,
   "tenantId": null,
   "type": "UserAssigned",
   "userAssignedIdentities": {
     "/subscriptions/<subscriptionid>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myIdentity": {
       "clientId": "<client-id>",
       "principalId": "<principal-id>"
     }
   }
 },
```

## <a name="bring-your-own-kubelet-mi"></a>Identidad administrada de Bring your own kubelet

Una identidad de kubelet permite que se conceda acceso a la identidad existente antes de la creación del clúster. Esta característica permite escenarios como la conexión a ACR con una identidad administrada creada previamente.

### <a name="prerequisites"></a>Requisitos previos

- Debe tener instalada la versión 2.26.0 de la CLI de Azure, o cualquier versión posterior.

### <a name="limitations"></a>Limitaciones

- Solo funciona con un clúster administrado asignado por el usuario.
- Actualmente no se admiten Este de China ni Norte de China en Azure China 21Vianet.

### <a name="create-or-obtain-managed-identities"></a>Creación u obtención de identidades administradas

Si aún no tiene una identidad administrada de plano de control, debería continuar y crear una. En el ejemplo siguiente se usa el comando [az identity create][az-identity-create]:

```azurecli-interactive
az identity create --name myIdentity --resource-group myResourceGroup
```

El resultado debería tener este aspecto:

```output
{                                  
  "clientId": "<client-id>",
  "clientSecretUrl": "<clientSecretUrl>",
  "id": "/subscriptions/<subscriptionid>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myIdentity", 
  "location": "westus2",
  "name": "myIdentity",
  "principalId": "<principalId>",
  "resourceGroup": "myResourceGroup",                       
  "tags": {},
  "tenantId": "<tenant-id>",
  "type&quot;: &quot;Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

Si aún no tiene una identidad administrada de kubelet, debería continuar y crear una. En el ejemplo siguiente se usa el comando [az identity create][az-identity-create]:

```azurecli-interactive
az identity create --name myKubeletIdentity --resource-group myResourceGroup
```

El resultado debería tener este aspecto:

```output
{
  "clientId": "<client-id>",
  "clientSecretUrl": "<clientSecretUrl>",
  "id": "/subscriptions/<subscriptionid>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myKubeletIdentity", 
  "location": "westus2",
  "name": "myKubeletIdentity",
  "principalId": "<principalId>",
  "resourceGroup": "myResourceGroup",                       
  "tags": {},
  "tenantId": "<tenant-id>",
  "type&quot;: &quot;Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

Si la identidad administrada existente forma parte de su suscripción, puede usar el comando [az identity list][az-identity-list] para consultarla:

```azurecli-interactive
az identity list --query "[].{Name:name, Id:id, Location:location}" -o table
```

### <a name="create-a-cluster-using-kubelet-identity"></a>Creación de un clúster mediante la identidad de kubelet

Ahora puede usar el siguiente comando para crear el clúster con sus identidades existentes. Proporcione el identificador de la identidad del plano de control a través de `assign-identity` y la identidad administrada de kubelet a través de `assign-kublet-identity`:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myManagedCluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --enable-managed-identity \
    --assign-identity <identity-id> \
    --assign-kubelet-identity <kubelet-identity-id>
```

Una creación correcta del clúster mediante su propia identidad administrada de kubelet contiene la siguiente salida:

```output
  "identity": {
    "principalId": null,
    "tenantId": null,
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/<subscriptionid>/resourcegroups/resourcegroups/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myIdentity": {
        "clientId": "<client-id>",
        "principalId&quot;: &quot;<principal-id>"
      }
    }
  },
  "identityProfile": {
    "kubeletidentity": {
      "clientId": "<client-id>",
      "objectId": "<object-id>",
      "resourceId&quot;: &quot;/subscriptions/<subscriptionid>/resourcegroups/resourcegroups/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myKubeletIdentity"
    }
  },
```

## <a name="next-steps"></a>Pasos siguientes
* Use las [plantillas de Azure Resource Manager][aks-arm-template] para crear clústeres habilitados para Managed Identity.

<!-- LINKS - external -->
[aks-arm-template]: /azure/templates/microsoft.containerservice/managedclusters

<!-- LINKS - internal -->
[az-identity-create]: /cli/azure/identity#az_identity_create
[az-identity-list]: /cli/azure/identity#az_identity_list
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
