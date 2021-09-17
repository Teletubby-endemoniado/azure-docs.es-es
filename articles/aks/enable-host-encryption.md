---
title: Habilitación del cifrado basado en host en Azure Kubernetes Service (AKS)
description: Aprenda a configurar un cifrado basado en host en un clúster de Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 04/26/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 3eb2f0023cbd0bbe36b466ecf4a1380aa20a2c5c
ms.sourcegitcommit: 8000045c09d3b091314b4a73db20e99ddc825d91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2021
ms.locfileid: "122446311"
---
# <a name="host-based-encryption-on-azure-kubernetes-service-aks"></a>Cifrado basado en host en Azure Kubernetes Service (AKS)

Con el cifrado basado en host, los datos almacenados en el host de máquina virtual de las máquinas virtuales de los nodos de agente de AKS se cifran en reposo y se transmiten cifrados al servido Storage. Esto significa que los discos temporales se cifran en reposo con claves administradas por la plataforma. La memoria caché de los discos de datos y del sistema operativo se cifra en reposo con claves administradas por la plataforma o por el cliente, según el tipo de cifrado establecido en esos discos. De forma predeterminada, cuando se usa AKS, el sistema operativo y los discos de datos se cifran en reposo con claves administradas por la plataforma, lo que significa que las memorias caché de estos discos también se cifran de forma predeterminada en reposo con claves administradas por la plataforma.  Puede especificar sus propias claves administradas siguiendo [Traiga sus propias claves (BYOK) con discos de Azure en Azure Kubernetes Service (AKS)](azure-disk-customer-managed-keys.md). La memoria caché de estos discos también se cifrará con la clave que especifique en este paso.


## <a name="before-you-begin"></a>Antes de empezar

Esta característica solo se puede establecer durante la creación del clúster o en el momento de creación del grupo de nodos.

> [!NOTE]
> El cifrado basado en host está disponible en [regiones de Azure][supported-regions] que admiten el cifrado del lado servidor de discos administrados de Azure y solo con [tamaños de máquinas virtuales compatibles][supported-sizes] específicos.

### <a name="prerequisites"></a>Requisitos previos

- Asegúrese de que tiene instalada la extensión de la CLI v2.23 o posterior.
- Asegúrese de que tiene habilitada la marca de características `EncryptionAtHost` bajo `Microsoft.Compute`.

### <a name="register-encryptionathost--preview-features"></a>Registro de las características en versión preliminar de `EncryptionAtHost`

Para crear un clúster de AKS que usa el cifrado basado en host, debe habilitar las marcas de características `EncryptionAtHost` en su suscripción.

Registro de `EncryptionAtHost` la marca de característica con el comando de [característica de registro az][az-feature-register], tal como se muestra en el siguiente ejemplo:

```azurecli-interactive
az feature register --namespace "Microsoft.Compute" --name "EncryptionAtHost"
```

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado de registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.Compute/EncryptionAtHost')].{Name:name,State:properties.state}"
```

Cuando todo esté listo, actualice el registro de los proveedores de recursos `Microsoft.Compute` con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

### <a name="limitations"></a>Limitaciones

- Solo se puede habilitar en grupos de nodos nuevos.
- Solo se puede habilitar en [regiones de Azure][supported-regions] que admiten el cifrado del lado servidor de discos administrados de Azure y solo con [tamaños de máquinas virtuales compatibles][supported-sizes] específicos.
- Requiere un clúster de AKS y un grupo de nodos basado en Virtual Machine Scale Sets (VMSS) como *tipo de conjunto de máquinas virtuales*.

## <a name="use-host-based-encryption-on-new-clusters"></a>Uso del cifrado basado en host en clústeres nuevos

Configure los nodos de agente de clúster para usar el cifrado basado en host cuando se cree el clúster. 

```azurecli-interactive
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_DS2_v2 -l westus2 --enable-encryption-at-host
```

Si quiere crear clústeres sin el cifrado basado en host, puede omitir el parámetro `--enable-encryption-at-host` para hacerlo.

## <a name="use-host-based-encryption-on-existing-clusters"></a>Uso del cifrado basado en host en clústeres existentes

Puede habilitar el cifrado basado en host en clústeres existentes agregando un nuevo grupo de nodos al clúster. Configure un grupo de nodos nuevo para usar el cifrado basado en host mediante el parámetro `--enable-encryption-at-host`.

```azurecli
az aks nodepool add --name hostencrypt --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_DS2_v2 -l westus2 --enable-encryption-at-host
```

Si quiere crear grupos de nodos nuevos sin la característica de cifrado basado en host, puede hacerlo omitiendo el parámetro `--enable-encryption-at-host`.

## <a name="next-steps"></a>Pasos siguientes

Revise las [prácticas recomendadas para la seguridad del clúster de AKS][best-practices-security]. Lea más sobre el [cifrado basado en host](../virtual-machines/disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data).


<!-- LINKS - external -->

<!-- LINKS - internal -->
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[best-practices-security]: ./operator-best-practices-cluster-security.md
[supported-regions]: ../virtual-machines/disk-encryption.md#supported-regions
[supported-sizes]: ../virtual-machines/disk-encryption.md#supported-vm-sizes
[azure-cli-install]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
