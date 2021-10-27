---
title: Habilitación de la compatibilidad con disco Ultra en Azure Kubernetes Service (AKS)
description: Aprenda a habilitar y configurar discos Ultra en un clúster de Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 10/12/2021
ms.openlocfilehash: 07d43fc72d10f1739fb67124feadacc9632206be
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129984865"
---
# <a name="use-azure-ultra-disks-on-azure-kubernetes-service"></a>Uso de discos Ultra de Azure en Azure Kubernetes Service

Los [discos Ultra de Azure](../virtual-machines/disks-enable-ultra-ssd.md) ofrecen un alto rendimiento, IOPS elevadas y un almacenamiento en disco coherente y de baja latencia para las aplicaciones con estado. Una ventaja importante de los discos Ultra es la posibilidad de cambiar dinámicamente el rendimiento del disco SSD junto con sus cargas de trabajo sin tener que reiniciar los nodos de agente. Los discos Ultra son adecuados para cargas de trabajo con un uso intensivo de datos.

## <a name="before-you-begin"></a>Antes de empezar

Esta característica solo se puede establecer durante la creación del clúster o en el momento de creación del grupo de nodos.

> [!IMPORTANT]
> Los discos Ultra de Azure requieren grupos de nodos implementados en zonas de disponibilidad y regiones que admiten estos discos, así como solo series de máquinas virtuales específicas. Consulte el [**ámbito y las limitaciones de la disponibilidad general de los discos Ultra**](../virtual-machines/disks-enable-ultra-ssd.md#ga-scope-and-limitations).

### <a name="limitations"></a>Limitaciones
- Consulte el [**ámbito y las limitaciones de la disponibilidad general de los discos Ultra**](../virtual-machines/disks-enable-ultra-ssd.md#ga-scope-and-limitations)
- El intervalo de tamaño admitido para los discos Ultra está comprendido entre 100 y 1500

## <a name="create-a-new-cluster-that-can-use-ultra-disks"></a>Creación de un nuevo clúster que pueda usar discos Ultra

Cree un clúster de AKS que pueda aprovechar los discos Ultra mediante los siguientes comandos de la CLI. Use la marca `--enable-ultra-ssd` para establecer la característica `EnableUltraSSD`.

Crear un grupo de recursos de Azure:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Cree el clúster de AKS con compatibilidad con Ultra Disks.

```azurecli-interactive
# Create an AKS-managed Azure AD cluster
az aks create -g MyResourceGroup -n MyManagedCluster -l westus2 --node-vm-size Standard_D2s_v3 --zones 1 2 --node-count 2 --enable-ultra-ssd
```

Si desea crear clústeres sin la compatibilidad con disco Ultra, puede omitir el parámetro `--enable-ultra-ssd` para hacerlo.

## <a name="enable-ultra-disks-on-an-existing-cluster"></a>Habilitación de discos Ultra en un clúster existente

Puede habilitar los discos Ultra en clústeres existentes agregando un nuevo grupo de nodos al clúster que admitan discos Ultra. Configure un nuevo grupo de nodos para usar Ultra Disks mediante la marca `--enable-ultra-ssd`.

```azurecli
az aks nodepool add --name ultradisk --cluster-name myAKSCluster --resource-group myResourceGroup --node-vm-size Standard_D2s_v3 --zones 1 2 --node-count 2 --enable-ultra-ssd
```

Si desea crear grupos de nodos nuevos sin compatibilidad con los discos Ultra, puede hacerlo omitiendo el parámetro `--enable-ultra-ssd`.

## <a name="use-ultra-disks-dynamically-with-a-storage-class"></a>Uso de discos Ultra dinámicamente con una clase de almacenamiento

Para usar discos Ultra en nuestras implementaciones o conjuntos con estado, puede usar una [clase de almacenamiento para el aprovisionamiento dinámico](azure-disks-dynamic-pv.md).

### <a name="create-the-storage-class"></a>Creación de la clase de almacenamiento

Una clase de almacenamiento se usa para definir cómo se crea dinámicamente una unidad de almacenamiento con un volumen persistente. Para más información sobre las clases de almacenamiento de Kubernetes, consulte las [clases de almacenamiento de Kubernetes][kubernetes-storage-classes].

En este caso, vamos a crear una clase de almacenamiento que haga referencia a discos Ultra. Cree un archivo denominado `azure-ultra-disk-sc.yaml` y cópielo en el siguiente código manifiesto.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: ultra-disk-sc
provisioner: kubernetes.io/azure-disk
volumeBindingMode: WaitForFirstConsumer # optional, but recommended if you want to wait until the pod that will use this disk is created 
parameters:
  skuname: UltraSSD_LRS
  kind: managed
  cachingmode: None
  diskIopsReadWrite: "2000"  # minimum value: 2 IOPS/GiB 
  diskMbpsReadWrite: "320"   # minimum value: 0.032/GiB
```

Cree la clase de almacenamiento con el comando [kubectl apply][kubectl-apply] y especifique su archivo *azure-ultra-disk-sc.yaml*:

```console
$ kubectl apply -f azure-ultra-disk-sc.yaml


storageclass.storage.k8s.io/ultra-disk-sc created
```

## <a name="create-a-persistent-volume-claim"></a>Creación de una notificación de volumen persistente

Una notificación de volumen persistente (PVC) se usa para aprovisionar automáticamente el almacenamiento en función de una clase de almacenamiento. En este caso, un PVC puede usar la clase de almacenamiento creada anteriormente para crear un disco Ultra.

Cree un archivo denominado `azure-ultra-disk-pvc.yaml` y cópielo en el siguiente código manifiesto. La notificación solicita un disco llamado `ultra-disk` que tiene un tamaño de *1000 GB* con acceso *ReadWriteOnce*. La clase de almacenamiento *ultra-disk-sc* se especifica como la clase de almacenamiento.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ultra-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: ultra-disk-sc
  resources:
    requests:
      storage: 1000Gi
```

Cree la notificación del volumen persistente con el comando [kubectl apply][kubectl-apply] y especifique su archivo *azure-ultra-disk-pvc.yaml*:

```console
$ kubectl apply -f azure-ultra-disk-pvc.yaml

persistentvolumeclaim/ultra-disk created
```

## <a name="use-the-persistent-volume"></a>Uso del volumen persistente

Una vez que se haya creado la notificación de volumen persistente y tras el aprovisionamiento correcto del disco, se puede crear un pod con acceso al disco. El siguiente manifiesto crea un pod NGINX básico que utiliza la notificación de volumen persistente denominada *ultra-disk* para montar el disco de Azure en la ruta de acceso `/mnt/azure`.

Cree un archivo denominado `nginx-ultra.yaml` y cópielo en el siguiente código manifiesto.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx-ultra
spec:
  containers:
  - name: nginx-ultra
    image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: ultra-disk
```

Cree el pod con el comando [kubectl apply][kubectl-apply], tal como se muestra en el ejemplo siguiente:

```console
$ kubectl apply -f nginx-ultra.yaml

pod/nginx-ultra created
```

Ahora tiene un pod en ejecución con el disco de Azure montado en el directorio `/mnt/azure`. Esta configuración puede verse al examinar el pod mediante `kubectl describe pod nginx-ultra`, tal y como se muestra en el siguiente ejemplo reducido:

```console
$ kubectl describe pod nginx-ultra

[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azure-managed-disk
    ReadOnly:   false
  default-token-smm2n:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-smm2n
    Optional:    false
[...]
Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              2m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  2m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-smm2n"
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "pvc-faf0f176-8b8d-11e8-923b-deb28c58d242"
[...]
```


## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los discos ultra, consulte [Uso de discos Ultra de Azure](../virtual-machines/disks-enable-ultra-ssd.md).
- Para más información sobre los procedimientos recomendados, consulte [Procedimientos recomendados para el almacenamiento y las copias de seguridad en Azure Kubernetes Service (AKS)][operator-best-practices-storage].

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/disks-types.md
[az-disk-list]: /cli/azure/disk#az_disk_list
[az-snapshot-create]: /cli/azure/snapshot#az_snapshot_create
[az-disk-create]: /cli/azure/disk#az_disk_create
[az-disk-show]: /cli/azure/disk#az_disk_show
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
[storage-class-concepts]: concepts-storage.md#storage-classes
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-provider-register]: /cli/azure/provider#az_provider_register
