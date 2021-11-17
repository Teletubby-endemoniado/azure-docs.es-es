---
title: Administración de Azure RBAC en Kubernetes desde Azure
titleSuffix: Azure Kubernetes Service
description: Aprenda a usar Azure RBAC para la autorización de Kubernetes con Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 02/09/2021
ms.author: jpalma
author: palma21
ms.openlocfilehash: b6cd1bce4528d35cf0f7f897e6032079cae6cdea
ms.sourcegitcommit: 96deccc7988fca3218378a92b3ab685a5123fb73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131579243"
---
# <a name="use-azure-rbac-for-kubernetes-authorization"></a>Uso de la autorización de Azure RBAC para Kubernetes

En la actualidad, ya puede aprovechar la autenticación integrada entre [Azure Active Directory (Azure AD) y AKS](managed-aad.md). Cuando está habilitada, esta integración permite a los clientes usar usuarios, grupos o entidades de servicio de Azure AD como asuntos en RBAC de Kubernetes, más información [aquí](azure-ad-rbac.md).
Esta característica evita tener que administrar por separado las identidades de usuario y las credenciales de Kubernetes. Sin embargo, todavía tiene que configurar y administrar Azure RBAC y Kubernetes por separado. Para más información sobre la autenticación y la autorización con RBAC en AKS, haga clic [aquí](concepts-identity.md).

En este documento se describe un nuevo enfoque que permite la administración unificada y el control de acceso en los recursos de Azure, AKS y Kubernetes.

## <a name="before-you-begin"></a>Antes de empezar

La capacidad de administrar RBAC para los recursos de Kubernetes de Azure le ofrece la opción de administrar RBAC para los recursos del clúster mediante mecanismos de Kubernetes nativos o de Azure. Cuando está habilitada, las entidades de seguridad de Azure AD se validarán exclusivamente con Azure RBAC, mientras que las cuentas de servicio y los usuarios de Kubernetes normales se validan exclusivamente mediante el RBAC de Kubernetes. Para más información sobre la autenticación y autorización con RBAC en AKS, haga clic [aquí](concepts-identity.md#azure-rbac-for-kubernetes-authorization).

### <a name="prerequisites"></a>Requisitos previos

- Asegúrese de que tiene la versión 2.24.0 de la CLI de Azure o una posterior.
- Asegúrese de que ha instalado [kubectl v 1.18.3 +][az-aks-install-cli].

### <a name="limitations"></a>Limitaciones

- Requiere [integración administrada de Azure AD](managed-aad.md).
- Use [kubectl v1.18.3+][az-aks-install-cli].
- Si tiene CRDs y está realizando definiciones de roles personalizados, la única manera de abarcar CRDs hoy es proporcionar `Microsoft.ContainerService/managedClusters/*/read`. AKS trabaja para proporcionar permisos más granulares para CRDs. Para el resto de objetos, puede usar los grupos de API específicos, por ejemplo: `Microsoft.ContainerService/apps/deployments/read`.
- Las nuevas asignaciones de roles pueden tardar hasta 5 minutos en propagarse y actualizarse mediante el servidor de autorización.
- Es necesario que el inquilino de Azure AD configurado para la autenticación sea el mismo que el de la suscripción que contiene el clúster de AKS. 

## <a name="create-a-new-cluster-using-azure-rbac-and-managed-azure-ad-integration"></a>Crear un nuevo clúster con Azure RBAC y la integración administrada de Azure AD

Crear un clúster de AKS con los siguientes comandos de la CLI.

Crear un grupo de recursos de Azure:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Cree el clúster de AKS con la integración administrada de Azure AD y Azure RBAC para la autorización de Kubernetes.

```azurecli-interactive
# Create an AKS-managed Azure AD cluster
az aks create -g MyResourceGroup -n MyManagedCluster --enable-aad --enable-azure-rbac
```

Una creación correcta de un clúster con la integración de Azure AD y Azure RBAC para la autorización de Kubernetes tiene la siguiente sección en el cuerpo de respuesta:

```json
"AADProfile": {
    "adminGroupObjectIds": null,
    "clientAppId": null,
    "enableAzureRbac": true,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "****-****-****-****-****"
  }
```

## <a name="integrate-azure-rbac-into-an-existing-cluster"></a>Integración de Azure RBAC en un clúster existente

> [!NOTE]
> Para usar la autorización de Azure RBAC para Kubernetes, debe estar habilitada la integración de Azure Active Directory en el clúster. Para más información, consulte [Integración de Azure Active Directory][managed-aad].

Para agregar la autorización de Azure RBAC para Kubernetes a un clúster de AKS existente, use el comando [az aks update][az-aks-update] con la marca `enable-azure-rbac`.

```azurecli-interactive
az aks update -g myResourceGroup -n myAKSCluster --enable-azure-rbac
```
Para quitar la autorización de Azure RBAC para Kubernetes de un clúster de AKS existente, use el comando [az aks update][az-aks-update] con la marca `disable-azure-rbac`.

```azurecli-interactive
az aks update -g myResourceGroup -n myAKSCluster --disable-azure-rbac
```

## <a name="create-role-assignments-for-users-to-access-cluster"></a>Crear asignaciones de roles para que los usuarios accedan al clúster.

AKS proporciona los siguientes cuatro roles integrados:


| Role                                | Descripción  |
|-------------------------------------|--------------|
| Lector de RBAC de Azure Kubernetes Service  | Permite el acceso de solo lectura para ver la mayoría de los objetos en un espacio de nombres. No permite la visualización de roles o enlaces de roles. Este rol no permite visualización `Secrets`, ya que leer el contenido de los secretos permite el acceso a las credenciales de ServiceAccount en el espacio de nombres, que permitiría el acceso a la API como cualquier ServiceAccount en el espacio de nombres (una forma de elevación de privilegios).  |
| Escritura de Azure Kubernetes Service RBAC | Permite el acceso de lectura y escritura para ver la mayoría de los objetos en un espacio de nombres. Este rol no permite la visualización o modificación de roles o enlaces de roles. Sin embargo, este rol permite acceder a `Secrets` y ejecutar pods como cualquier ServiceAccount en el espacio de nombres, por lo que se puede usar para obtener los niveles de acceso de la API de cualquier ServiceAccount en el espacio de nombres. |
| Administrador de Azure Kubernetes Service RBAC  | Permite el acceso de administrador, diseñado para su concesión dentro de un espacio de nombres. Permite el acceso de lectura y escritura a la mayoría de los recursos de un espacio de nombres (o ámbito de clúster), incluida la capacidad de crear roles y enlaces de roles dentro del espacio de nombres. Este rol no permite el acceso de escritura a la cuota de recursos o al espacio de nombres en sí. |
| Administrador de clúster de Azure Kubernetes Service RBAC  | Permite el acceso de superusuario para realizar cualquier acción en cualquier recurso. Proporciona control total sobre todos los recursos del clúster y en todos los espacios de nombres. |


Las asignaciones de roles que tienen como ámbito el **clúster de AKS completo** se pueden realizar en la hoja Access Control (IAM) del recurso de clúster en Azure Portal o mediante comandos de CLI de Azure, como se muestra a continuación:

```bash
# Get your AKS Resource ID
AKS_ID=$(az aks show -g MyResourceGroup -n MyManagedCluster --query id -o tsv)
```

```azurecli-interactive
az role assignment create --role "Azure Kubernetes Service RBAC Admin" --assignee <AAD-ENTITY-ID> --scope $AKS_ID
```

donde `<AAD-ENTITY-ID>` podría ser un nombre de usuario (por ejemplo, user@contoso.com) o incluso el ClientID de una entidad de servicio.

También puede crear asignaciones de roles en el ámbito de un **espacio de nombres** específico dentro del clúster:

```azurecli-interactive
az role assignment create --role "Azure Kubernetes Service RBAC Reader" --assignee <AAD-ENTITY-ID> --scope $AKS_ID/namespaces/<namespace-name>
```

En la actualidad, las asignaciones de roles cuyo ámbito son los espacios de nombres deben configurarse mediante CLI de Azure.


### <a name="create-custom-roles-definitions"></a>Creación de definición de roles personalizados

Opcionalmente, puede optar por crear su propia definición de rol y, a continuación, asignarla como se indicó anteriormente.

A continuación se muestra un ejemplo de una definición de roles que permite a un usuario leer solo las implementaciones y nada más. Puede consultar la lista completa de acciones posibles [aquí](../role-based-access-control/resource-provider-operations.md#microsoftcontainerservice).


Copie el siguiente JSON en un archivo denominado `deploy-view.json`.

```json
{
    "Name": "AKS Deployment Reader",
    "Description": "Lets you view all deployments in cluster/namespace.",
    "Actions": [],
    "NotActions": [],
    "DataActions": [
        "Microsoft.ContainerService/managedClusters/apps/deployments/read"
    ],
    "NotDataActions": [],
    "assignableScopes": [
        "/subscriptions/<YOUR SUBSCRIPTION ID>"
    ]
}
```

Reemplace `<YOUR SUBSCRIPTION ID>` por el id. de la suscripción, que puede obtener al ejecutar:

```azurecli-interactive
az account show --query id -o tsv
```

Ahora se puede crear la definición de roles al ejecutar el comando siguiente desde la carpeta donde se guardó `deploy-view.json`:

```azurecli-interactive
az role definition create --role-definition @deploy-view.json 
```

Ahora que tiene la definición de rol, puede asignarla a un usuario u otra identidad mediante la ejecución de:

```azurecli-interactive
az role assignment create --role "AKS Deployment Reader" --assignee <AAD-ENTITY-ID> --scope $AKS_ID
```

## <a name="use-azure-rbac-for-kubernetes-authorization-with-kubectl"></a>Uso de Azure RBAC para la autorización de Kubernetes con `kubectl`.

> [!NOTE]
> Asegúrese de que dispone de la versión más reciente de kubectl al ejecutar el comando siguiente:
>
> ```azurecli-interactive
> az aks install-cli
> ```
>
> Es posible que tenga que ejecutarlo con los privilegios de `sudo`.

Ahora que ha asignado el rol y los permisos deseados. Puede empezar a llamar a la API de Kubernetes, por ejemplo, desde `kubectl`.

Para este propósito, primero vamos a obtener el kubeconfig del clúster con el siguiente comando:

```azurecli-interactive
az aks get-credentials -g MyResourceGroup -n MyManagedCluster
```

> [!IMPORTANT]
> Necesitará el rol integrado del [usuario del clúster de Azure Kubernetes Service](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role) para realizar el paso anterior.

Ahora puede usar kubectl para, por ejemplo, enumerar los nodos del clúster. La primera vez que la ejecute, deberá iniciar sesión y los comandos subsiguientes usarán el token de acceso correspondiente.

```azurecli-interactive
kubectl get nodes
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code AAAAAAAAA to authenticate.

NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-93451573-vmss000000   Ready    agent   3h6m   v1.15.11
aks-nodepool1-93451573-vmss000001   Ready    agent   3h6m   v1.15.11
aks-nodepool1-93451573-vmss000002   Ready    agent   3h6m   v1.15.11
```


## <a name="use-azure-rbac-for-kubernetes-authorization-with-kubelogin"></a>Uso de Azure RBAC para la autorización de Kubernetes con `kubelogin`.

Para desbloquear escenarios adicionales, como los inicios de sesión no interactivos, las versiones de `kubectl` anteriores o el uso de SSO en varios clústeres sin necesidad de iniciar sesión en el nuevo clúster, admitiendo que el token siga siendo válido, AKS creó un complemento exec denominado [`kubelogin`](https://github.com/Azure/kubelogin).

Puede usarlo mediante la ejecución de:

```bash
export KUBECONFIG=/path/to/kubeconfig
kubelogin convert-kubeconfig
``` 

La primera vez, tendrá que iniciar sesión de forma interactiva como con el kubectl regular, pero después ya no tendrá que hacerlo, incluso para los nuevos clústeres de Azure AD (siempre que el token siga siendo válido).

```bash
kubectl get nodes
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code AAAAAAAAA to authenticate.

NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-93451573-vmss000000   Ready    agent   3h6m   v1.15.11
aks-nodepool1-93451573-vmss000001   Ready    agent   3h6m   v1.15.11
aks-nodepool1-93451573-vmss000002   Ready    agent   3h6m   v1.15.11
```

## <a name="clean-up"></a>Limpieza

### <a name="clean-role-assignment"></a>Limpieza de asignación de roles

```azurecli-interactive
az role assignment list --scope $AKS_ID --query [].id -o tsv
```

Copie el identificador o los identificadores de todas las asignaciones que ha realizado y, a continuación,.

```azurecli-interactive
az role assignment delete --ids <LIST OF ASSIGNMENT IDS>
```

### <a name="clean-up-role-definition"></a>Limpieza de la definición de roles.

```azurecli-interactive
az role definition delete -n "AKS Deployment Reader"
```

### <a name="delete-cluster-and-resource-group"></a>Eliminar clúster y grupo de recursos.

```azurecli-interactive
az group delete -n MyResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información [aquí](concepts-identity.md) sobre la autenticación de AKS, la autorización, RBAC de Kubernetes y Azure RBAC.
- Obtenga más información sobre Azure RBAC [aquí](../role-based-access-control/overview.md).
- Obtenga más información sobre todas las acciones que puede usar para definir de un modo pormenorizado roles de Azure personalizados para la autorización de Kubernetes [aquí](../role-based-access-control/resource-provider-operations.md#microsoftcontainerservice).


<!-- LINKS - Internal -->
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-aks-install-cli]: /cli/azure/aks#az_aks_install_cli
[az-provider-register]: /cli/azure/provider#az_provider_register
[az-aks-update]: /cli/azure/aks#az_aks_update
[managed-aad]: ./managed-aad.md
