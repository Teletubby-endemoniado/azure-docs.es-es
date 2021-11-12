---
title: Uso de una instancia pública de Load Balancer
titleSuffix: Azure Kubernetes Service
description: Aprenda a usar una instancia pública de Load Balancer con una SKU estándar para exponer los servicios con Azure Kubernetes Service (AKS).
services: container-service
ms.topic: article
ms.date: 11/14/2020
ms.author: jpalma
author: palma21
ms.openlocfilehash: 41d98bfa2fddc6575d53c2770e9411609acb68c1
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131845844"
---
# <a name="use-a-public-standard-load-balancer-in-azure-kubernetes-service-aks"></a>Uso de Standard Load Balancer en Azure Kubernetes Service (AKS)

Azure Load Balancer es una instancia L4 del modelo de interconexión de sistemas abiertos (OSI) que admite escenarios de entrada y salida. Distribuye flujos de entrada que llegan al front-end del equilibrador de carga a las instancias del grupo de servidores back-end.

Una instancia **pública** de Load Balancer, cuando se integra con AKS, tiene dos propósitos:

1. Proporcionar conexiones salientes a los nodos de clúster dentro de la red virtual AKS. Logra este objetivo al traducir la dirección IP privada de los nodos a una dirección IP pública que forma parte de su *Grupo de salida*.
2. Para proporcionar acceso a las aplicaciones mediante instancias de Kubernetes Services del tipo `LoadBalancer`. En ese caso, puede escalar las aplicaciones y crear servicios con alta disponibilidad fácilmente.

Un equilibrador de carga **interno (o privado)** se usa cuando solo se admiten direcciones IP privadas como front-end. Los equilibradores de carga internos se usan para equilibrar la carga del tráfico dentro de una red virtual. También se puede acceder a un servidor front-end de equilibrador de carga desde una red local en un escenario híbrido.

En este documento se describe la integración con un equilibrador de carga público. Para la integración interna de Load Balancer, consulte la [documentación del equilibrador de carga interno de AKS](internal-lb.md).

## <a name="before-you-begin"></a>Antes de empezar

Azure Load Balancer está disponible en dos SKU: *Básica* y *Estándar*. De forma predeterminada, se usa la SKU *estándar* al crear un clúster de AKS. Use la SKU *estándar* para tener acceso a funcionalidad agregada, como un grupo mayor de back-end, [**varios grupos de nodos**](use-multiple-node-pools.md), y [**Availability Zones**](availability-zones.md). Se trata de la SKU de Load Balancer recomendada para AKS.

Para más información sobre las SKU *básicas* y *estándar*, consulte [Comparación de las SKU de Load Balancer][azure-lb-comparison].

En este artículo se da por sentado que tiene un clúster de AKS con Azure Load Balancer de la SKU *estándar* y se le guía en el uso y la configuración de algunas de las funcionalidades y características del equilibrador de carga. Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

> [!IMPORTANT]
> Si prefiere no aprovechar Azure Load Balancer para proporcionar una conexión de salida y prefiere tener su propia puerta de enlace, firewall o proxy para ese propósito, puede omitir la creación del grupo de salida del equilibrador de carga y el IP del front-end respectivo si usa el [**tipo de salida como UserDefinedRouting (UDR)**](egress-outboundtype.md). El tipo de salida define el método de salida para un clúster y su valor predeterminado es el tipo de equilibrador de carga.

## <a name="use-the-public-standard-load-balancer"></a>Uso de la instancia pública de Standard Load Balancer.

Después de crear un clúster de AKS con el tipo de salida de Load Balancer (valor predeterminado), el clúster está listo para usar el equilibrador de carga para exponer también los servicios.

Para ello, puede crear un servicio público de tipo `LoadBalancer` como se muestra en el ejemplo siguiente. Para empezar, cree un manifiesto de servicio denominado `public-svc.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: public-svc
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: public-app
```

Para implementar el manifiesto de servicio público, use [kubectl apply][kubectl-apply] y especifique el nombre del manifiesto de YAML:

```azurecli-interactive
kubectl apply -f public-svc.yaml
```

Azure Load Balancer se configurará con una nueva dirección IP pública de entrada al nuevo servicio. Dado que Azure Load Balancer puede tener varias direcciones IP de front-end, cada nuevo servicio implementado obtendrá una nueva dirección IP de front-end dedicada a la que podrá acceder de forma única.

Por ejemplo, para confirmar que se ha creado el servicio y que Load Balancer está configurado, ejecute:

```azurecli-interactive
kubectl get service public-svc
```

```console
NAMESPACE     NAME          TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)         AGE
default       public-svc    LoadBalancer   10.0.39.110    52.156.88.187   80:32068/TCP    52s
```

Cuando visualiza los detalles del servicio, la dirección IP pública creada para este servicio en Load Balancer se muestra en la columna *EXTERNAL-IP*. La dirección IP puede tardar uno o dos minutos en cambiar de *\<pending\>* a una dirección IP pública real, como se muestra en el ejemplo anterior.

## <a name="configure-the-public-standard-load-balancer"></a>Configuración de la instancia pública de Standard Load Balancer

Cuando se usa la instancia pública de Load Balancer de la SKU estándar, hay un conjunto de opciones que se pueden personalizar en el momento de la creación o al actualizar el clúster. Estas opciones le permiten personalizar Load Balancer para satisfacer sus necesidades de cargas de trabajo y debe revisarse en consecuencia. Con Standard Load Balance, puede hacer lo siguiente:

* Definir o escalar el número de direcciones IP de salida administradas.
* Traer su propio [prefijo de IP de salida](#provide-your-own-outbound-public-ips-or-prefixes).
* Personalizar el número de puertos de salida asignados a cada nodo del clúster.
* Configurar el valor de tiempo de espera para las conexiones inactivas.

> [!IMPORTANT]
> En un momento dado, solo se puede usar una opción de dirección IP de salida (direcciones IP administradas, traer sus propias direcciones IP o prefijos de IP).

### <a name="scale-the-number-of-managed-outbound-public-ips"></a>Escalar el número de direcciones IP públicas de salida administradas.

Azure Load Balancer proporciona conectividad de salida desde una red virtual, además de conectividad de entrada. Las reglas de salida simplifican la configuración de la traducción de direcciones de red públicas de salida de Standard Load Balancer.

Al igual que el resto de reglas de Load Balancer, las reglas de salida siguen la misma sintaxis conocida que las reglas de NAT de entrada y el equilibrio de carga:

***IP de front-end + parámetros + grupo de back-end***

Una regla de salida configura una NAT de salida para todas las máquinas virtuales identificadas por el grupo de back-end que se deben traducir para el front-end. Y los parámetros proporcionan un control más preciso sobre el algoritmo de NAT de salida.

Mientras una regla de salida se puede usar con una única dirección IP pública, las reglas de salida alivian la carga de configuración para escalar la NAT de salida. Puede usar varias direcciones IP para planear escenarios de gran escala y puede usar reglas de salida para mitigar los patrones con tendencia a agotamiento de SNAT. Cada dirección IP adicional que proporciona un front-end ofrece 64 000 puertos efímeros que Load Balancer puede usar como puertos SNAT. 

Cuando se usa un equilibrador de carga de SKU *estándar* con direcciones IP públicas de salida administradas, que se crean de forma predeterminada, se puede escalar el número de direcciones IP públicas de salida administradas mediante el parámetro **`load-balancer-managed-ip-count`** .

Para actualizar un clúster existente, ejecute el siguiente comando. Este parámetro también se puede establecer en el momento de creación del clúster para tener varias direcciones IP públicas de salida administradas.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 2
```

En el ejemplo anterior se establece el número de direcciones IP públicas de salida administradas en *2* para el clúster de *myAKSCluster* en *myResourceGroup*. 

También puede usar el parámetro **`load-balancer-managed-ip-count`** para establecer el número inicial de direcciones IP públicas de salida administradas al crear el clúster anexando el parámetro **`--load-balancer-managed-outbound-ip-count`** y estableciéndolo en el valor deseado. El número predeterminado de direcciones IP públicas de salida administradas es 1.

### <a name="provide-your-own-outbound-public-ips-or-prefixes"></a>Proporcionar prefijos o direcciones IP públicas de salida

Cuando se usa un equilibrador de carga de SKU *estándar*, de forma predeterminada, el clúster de AKS crea automáticamente una dirección IP pública en el grupo de recursos de infraestructura administrado por AKS y la asigna al grupo de salida del equilibrador de carga.

Una dirección IP pública creada por AKS se considera un recurso administrado de AKS. Esto significa que el ciclo de vida de la dirección IP pública está pensado para ser administrado por AKS y no requiere ninguna acción del usuario directamente en el recurso de IP pública. Como alternativa, puede asignar su propio prefijo de dirección IP pública personalizada o IP pública en el momento de la creación del clúster. Las direcciones IP personalizadas también se pueden actualizar en las propiedades del equilibrador de carga de un clúster existente.

Requisitos para usar su propio prefijo o dirección IP pública:

- Las direcciones IP públicas personalizadas deben crearse y ser propiedad del usuario. Las direcciones IP públicas administradas creadas por AKS no se pueden volver a usar como una dirección IP personalizada, ya que pueden provocar conflictos de administración.
- Debe asegurarse de que la identidad de clúster de AKS (entidad de servicio o identidad administrada) tiene permisos para acceder a la dirección IP de salida. Según la [lista de permisos de dirección IP pública requeridos](kubernetes-service-principal.md#networking).
- Asegúrese de que cumple [los requisitos previos y las restricciones](../virtual-network/ip-services/public-ip-address-prefix.md#limitations) necesarios para configurar direcciones IP de salida o prefijos IP de salida.

#### <a name="update-the-cluster-with-your-own-outbound-public-ip"></a>Actualización del clúster con su propia dirección IP pública de salida

Use el comando [az network public-ip show][az-network-public-ip-show] para enumerar los id. de las direcciones IP públicas.

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIP --query id -o tsv
```

El comando anterior muestra el id. de la dirección IP pública *myPublicIP* en el grupo de recursos *myResourceGroup*.

Use el comando `az aks update` con el parámetro **`load-balancer-outbound-ips`** para actualizar el clúster con las direcciones IP públicas.

En el ejemplo siguiente se usa el parámetro `load-balancer-outbound-ips` con los identificadores del comando anterior.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

#### <a name="update-the-cluster-with-your-own-outbound-public-ip-prefix"></a>Actualización del clúster con su propio prefijo de dirección IP pública de salida

También puede usar prefijos de dirección IP pública para la salida con el equilibrador de carga de SKU *estándar*. En el ejemplo siguiente se usa el comando [az network public-ip prefix show][az-network-public-ip-prefix-show] para enumerar los id. de los prefijos de dirección IP pública:

```azurecli-interactive
az network public-ip prefix show --resource-group myResourceGroup --name myPublicIPPrefix --query id -o tsv
```

El comando anterior muestra el id. del prefijo de dirección IP pública *myPublicIPPrefix* en el grupo de recursos *myResourceGroup*.

En el ejemplo siguiente se usa el parámetro *load-balancer-outbound-ip-prefixes* con los id. del comando anterior.

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

#### <a name="create-the-cluster-with-your-own-public-ip-or-prefixes"></a>Creación del clúster con su propio prefijo o dirección IP pública

Es posible que quiera traer sus propias direcciones IP o prefijos IP para la salida en el momento de la creación del clúster para admitir escenarios como la incorporación de puntos de conexión de salida a una lista de permitidos. Anexe los mismos parámetros mostrados anteriormente al paso de creación del clúster para definir sus propios prefijos IP y direcciones IP públicas al inicio del ciclo de vida de un clúster.

Use el comando *az aks create* con el parámetro *load-balancer-outbound-ips* para crear un clúster con las IP públicas en el inicio.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-outbound-ips <publicIpId1>,<publicIpId2>
```

Use el comando *az aks create* con el parámetro *load-balancer-outbound-ip-prefixes* para crear un clúster con los prefijos de direcciones IP públicas en el inicio.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --load-balancer-outbound-ip-prefixes <publicIpPrefixId1>,<publicIpPrefixId2>
```

### <a name="configure-the-allocated-outbound-ports"></a>Configuración de los puertos de salida asignados

> [!IMPORTANT]
> Si tiene aplicaciones en el clúster que se espera que establezcan una gran cantidad de conexiones en un pequeño conjunto de destinos; por ejemplo, muchas instancias de front-end que se conectan a una base de datos de SQL, tiene un escenario muy susceptible de encontrar agotamiento de puertos SNAT (que se está quedando sin puertos desde los que conectarse). En estos casos, se recomienda aumentar los puertos de salida asignados y las direcciones IP de front-end salientes en el equilibrador de carga. Consulte a continuación el procedimiento para calcular correctamente estos valores.

A menos que se especifique lo contrario, AKS usará el valor predeterminado de los puertos de salida asignados que defina Standard Load Balancer al configurarlo. Este valor es **null** en la API de AKS o **0** en la API de SLB, tal como se muestra en el comando siguiente:

```azurecli-interactive
NODE_RG=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
az network lb outbound-rule list --resource-group $NODE_RG --lb-name kubernetes -o table
```

Los comandos anteriores mostrarán la regla de salida del equilibrador de carga, por ejemplo:

```console
AllocatedOutboundPorts    EnableTcpReset    IdleTimeoutInMinutes    Name             Protocol    ProvisioningState    ResourceGroup
------------------------  ----------------  ----------------------  ---------------  ----------  -------------------  -------------
0                         True              30                      aksOutboundRule  All         Succeeded            MC_myResourceGroup_myAKSCluster_eastus  
```

Esta salida no significa que el clúster tenga 0 puertos, sino que usa la [asignación automática de puertos de salida en función del tamaño del grupo de back-end][azure-lb-outbound-preallocatedports]. Por ejemplo, si un clúster tiene 50 nodos o menos, se asignan 1024 puertos a cada nodo. A medida que aumenta el número de nodos del clúster, habrá menos puertos disponibles por nodo.

Para definir o aumentar el número de puertos de salida asignados, se deben calcular los valores adecuados para el número de puertos de salida y el número de direcciones IP. El número de puertos de salida se fija por instancia en el valor especificado aquí. El valor de los puertos de salida debe ser un múltiplo de 8.

Si se agregan más direcciones IP no se agregan más puertos a ningún nodo; en su lugar, se proporciona capacidad para más nodos del clúster. Al realizar este cálculo, asegúrese de tener en cuenta los nodos que se pueden agregar como parte de las actualizaciones, incluido el recuento de los nodos especificados a través de los [valores maxSurge](upgrade-cluster.md#customize-node-surge-upgrade). El cálculo del número de direcciones IP necesarias es `(<maximum number of nodes in the cluster> * <outbound ports per node>) / 64000`, redondeado al entero más cercano.

Ejemplos:
- Si no se proporciona ningún valor y el clúster tiene 48 nodos, cada nodo tendrá 1024 puertos disponibles.
- Si no se proporciona ningún valor y el clúster aumenta a 52 nodos, cada nodo ahora tendrá 512 puertos disponibles.
- Si los puertos de salida se establecen en 1000 y el número de direcciones IP salientes se establece en 2, el clúster puede admitir un máximo de 128 nodos (64 000 puertos por IP/1000 puertos por nodo x 2 direcciones IP = 128 nodos).
- Si los puertos de salida se establecen en 4000 y el número de direcciones IP salientes se establece en 7, el clúster puede admitir un máximo de 112 nodos (64 000 puertos por IP/4000 puertos por nodo x 7 direcciones IP = 112 nodos).

Una vez calculados los valores, se puede usar el siguiente comando para aplicarlos al clúster:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-managed-outbound-ip-count 7 \
    --load-balancer-outbound-ports 4000
```

Para comprobar estos valores, suponga que el clúster tiene un tamaño máximo de 100 nodos y calcule el número de puertos necesarios (400 000) frente al número de puertos disponibles (448 000). Esta configuración proporcionaría puertos suficientes para un clúster de 100 nodos con espacio para la sobrecarga de nodos durante las actualizaciones.

- 100 nodos x 4000 puertos por nodo = 400 000 puertos necesarios
- 7 direcciones IP x 64 000 puertos por IP = 448 000 puertos disponibles.

> [!IMPORTANT]
> Debe [calcular la cuota necesaria y comprobar los requisitos][requisitos] antes de personalizar *allocatedOutboundPorts* para evitar problemas de conectividad o escalado. Es fundamental asignar suficientes puertos para los nodos adicionales necesarios para la actualización y otras operaciones. AKS se establece de forma predeterminada en un nodo de búfer para la actualización. Si usa [valores maxSurge](upgrade-cluster.md#customize-node-surge-upgrade), multiplique los puertos de salida por nodo por el valor maxSurge para determinar el número de puertos necesarios.

También puede usar los parámetros **`load-balancer-outbound-ports`** al crear un clúster, pero debe especificar **`load-balancer-managed-outbound-ip-count`** , **`load-balancer-outbound-ips`** o **`load-balancer-outbound-ip-prefixes`** .  Por ejemplo:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-sku standard \
    --load-balancer-managed-outbound-ip-count 2 \
    --load-balancer-outbound-ports 1024 
```

### <a name="configure-the-load-balancer-idle-timeout"></a>Configuración del tiempo de espera de inactividad del equilibrador de carga

Cuando se agotan los recursos de los puertos SNAT, los flujos de salida generan errores hasta que los flujos ya existentes liberan puertos SNAT. Load Balancer reclama puertos SNAT cuando el flujo se cierra y el equilibrador de carga configurado con AKS usa un tiempo de espera de inactividad de 30 minutos para reclamar puertos SNAT de los flujos inactivos.
También puede usar transporte (por ejemplo, **`TCP keepalives`** ) o **`application-layer keepalives`** para actualizar un flujo de inactividad y restablecer este tiempo de expiración en caso necesario. Puede configurar este tiempo de expiración como en el ejemplo siguiente: 


```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --load-balancer-idle-timeout 4
```

Si espera tener numerosas conexiones de corta duración, no hay ninguna conexión de larga duración y puede tener tiempos de inactividad prolongados, como el uso de `kubectl proxy` o `kubectl port-forward`, considere el uso de un valor de tiempo de expiración bajo, como 4 minutos. Además, al usar conexiones persistentes de TCP, es suficiente habilitarlas en un lado de la conexión. Por ejemplo, es suficiente habilitarlas solo en el servidor para restablecer el temporizador de inactividad del flujo y no se necesita que ambos lados inicien conexiones TCP persistentes. Existen conceptos similares existen para la capa de aplicación, incluidas las configuraciones de cliente/servidor de base de datos. Compruebe el lado del servidor para ver qué opciones existen para las conexiones persistentes específicas de la aplicación.

> [!IMPORTANT]
> AKS habilita el restablecimiento de TCP en modo inactivo de forma predeterminada y recomienda que mantenga esta configuración y la aproveche para obtener un comportamiento de la aplicación más predecible en sus escenarios.
> TCP RST solo se envía durante la conexión TCP en el estado ESTABLECIDO. [Aquí encontrará más información](../load-balancer/load-balancer-tcp-reset.md).

Si establece *IdleTimeoutInMinutes* en un valor distinto del predeterminado de 30 minutos, tenga en cuenta el tiempo que las cargas de trabajo necesitarán una conexión de salida. Tenga en cuenta también que el valor de tiempo de espera predeterminado para un equilibrador de carga de SKU *estándar* usado fuera de AKS es de 4 minutos. Un valor de *IdleTimeoutInMinutes* que refleje de forma más precisa su carga de trabajo de AKS específica puede ayudar a reducir el agotamiento de SNAT causado por la vinculación de las conexiones ya no se usan.

> [!WARNING]
> La modificación de los valores de *AllocatedOutboundPorts* e *IdleTimeoutInMinutes* puede cambiar significativamente el comportamiento de la regla de salida para el equilibrador de carga y no debe realizarse a la ligera sin comprender los inconvenientes y los patrones de conexión de la aplicación. Consulte la sección [Solución de problemas de SNAT que aparece a continuación][troubleshoot-snat] y revise las [reglas de salida de Load Balancer][azure-lb-outbound-rules-overview] y las [conexiones salientes en Azure][azure-lb-outbound-connections] antes de actualizar estos valores para comprender totalmente el impacto de los cambios.

## <a name="restrict-inbound-traffic-to-specific-ip-ranges"></a>Restricción del tráfico entrante a intervalos IP específicos

En el siguiente manifiesto se usa *loadBalancerSourceRanges* para especificar un nuevo intervalo IP para el tráfico externo entrante:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
  loadBalancerSourceRanges:
  - MY_EXTERNAL_IP_RANGE
```

En este ejemplo se actualiza la regla para permitir el tráfico externo entrante solo desde el intervalo `MY_EXTERNAL_IP_RANGE`. Si reemplaza `MY_EXTERNAL_IP_RANGE` por la dirección IP de la subred interna, el tráfico se restringe solo a las direcciones IP internas del clúster. Si el tráfico está restringido a las direcciones IP internas del clúster, los clientes fuera del clúster de Kubernetes no podrán acceder al equilibrador de carga.

> [!NOTE]
> Los flujos de tráfico externos entrantes del equilibrador de carga a la red virtual para el clúster de AKS. La red virtual tiene un grupo de seguridad de red (NSG) que permite todo el tráfico entrante desde el equilibrador de carga. Este NSG usa una [etiqueta de servicio][service-tags] de tipo *LoadBalancer* para permitir el tráfico desde el equilibrador de carga.

## <a name="maintain-the-clients-ip-on-inbound-connections"></a>Mantenimiento de la dirección IP del cliente en las conexiones entrantes

De forma predeterminada, un servicio de tipo `LoadBalancer` [en Kubernetes](https://kubernetes.io/docs/tutorials/services/source-ip/#source-ip-for-services-with-type-loadbalancer) y en AKS no conservará la dirección IP del cliente en la conexión con el pod. La dirección IP de origen del paquete que se proporciona al pod será la dirección IP privada del nodo. Para mantener la dirección IP del cliente, debe establecer `service.spec.externalTrafficPolicy` en `local` en la definición del servicio. En el siguiente manifiesto se muestra un ejemplo:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  externalTrafficPolicy: Local
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="additional-customizations-via-kubernetes-annotations"></a>Personalizaciones adicionales mediante anotaciones de Kubernetes

A continuación, se muestra una lista de las anotaciones admitidas para los servicios de Kubernetes con el tipo `LoadBalancer`. Estas anotaciones solo se aplican a flujos **DE ENTRADA**:

| Anotación | Value | Descripción
| ----------------------------------------------------------------- | ------------------------------------- | ------------------------------------------------------------ 
| `service.beta.kubernetes.io/azure-load-balancer-internal`         | `true` o `false`                     | Especifique si el equilibrador de carga debe ser interno. Si no se establece, el valor predeterminado es público.
| `service.beta.kubernetes.io/azure-load-balancer-internal-subnet`  | Nombre de la subred                    | Especifique a qué subred se debe enlazar el equilibrador de carga interno. Si no se establece, el valor predeterminado es la subred configurada en el archivo de configuración de la nube.
| `service.beta.kubernetes.io/azure-dns-label-name`                 | Nombre de la etiqueta DNS en direcciones IP públicas   | Especifique el nombre de la etiqueta DNS para el servicio **público**. Si se establece en una cadena vacía, no se usará la entrada DNS de la dirección IP pública.
| `service.beta.kubernetes.io/azure-shared-securityrule`            | `true` o `false`                     | Especifique que el servicio debe exponerse mediante una regla de seguridad de Azure que se pueda compartir con otro servicio, con el fin de definir la especificidad de las reglas para un aumento en el número de servicios que se pueden exponer. Esta anotación depende de las característica [reglas de seguridad ampliadas](../virtual-network/network-security-groups-overview.md#augmented-security-rules) de los grupos de seguridad de red. 
| `service.beta.kubernetes.io/azure-load-balancer-resource-group`   | Nombre del grupo de recursos            | Especifique el grupo de recursos de direcciones IP públicas del equilibrador de carga que no están en el mismo grupo de recursos que la infraestructura de clúster (grupo de recursos de nodo).
| `service.beta.kubernetes.io/azure-allowed-service-tags`           | Lista de etiquetas de servicio permitidas          | Especifique una lista de [etiquetas de servicio ][service-tags] permitidas separadas por comas.
| `service.beta.kubernetes.io/azure-load-balancer-tcp-idle-timeout` | Tiempo de expiración de inactividad de TCP en minutos          | Especifique el tiempo, en minutos, para la expiración de inactividad de conexión TCP en el equilibrador de carga. El valor predeterminado y el mínimo es 4. El valor máximo es 30. Debe ser un entero.
|`service.beta.kubernetes.io/azure-load-balancer-disable-tcp-reset` | `true`                                | Deshabilite `enableTcpReset` para SLB. En desuso en Kubernetes 1.18 y eliminado en la versión 1.20. 


## <a name="troubleshooting-snat"></a>Solución de problemas de SNAT

Si sabe que se van a iniciar muchas conexiones TCP o UDP salientes a la misma dirección IP y puerto de destino, y observa errores en las conexiones salientes, o el soporte técnico está avisando del agotamiento de los puertos SNAT (puertos efímeros asignados previamente usados por PAT), dispone de varias opciones generales para mitigar este problema. Revise estas opciones y vea cuál está disponible y resulta mejor para su escenario. Es posible que una o varias de ellas puedan ayudar a administrar este escenario. Para obtener información detallada, revise la [guía de solución de problemas de conexiones salientes](../load-balancer/troubleshoot-outbound-connection.md).

Con frecuencia, la causa principal del agotamiento de SNAT es un antipatrón de cómo se establece y administra la conectividad de salida o cómo se cambian los temporizadores configurables de sus valores predeterminados. Consulte detenidamente esta sección.

### <a name="steps"></a>Pasos
1. Compruebe si las conexiones permanecen inactivas durante mucho tiempo y confíe en el tiempo de expiración de inactividad predeterminado para liberar ese puerto. En tal caso, puede que el tiempo de expiración predeterminado de 30 minutos se deba reducir para su escenario.
2. Investigue la forma en que la aplicación crea conectividad saliente (por ejemplo la revisión del código o la captura de paquetes).
3. Determine si esta actividad es el comportamiento esperado o si la aplicación no se comporta correctamente. Use las [métricas](../load-balancer/load-balancer-standard-diagnostics.md) y los [registros](../load-balancer/monitor-load-balancer.md) de Azure Monitor para apoyar sus conclusiones. Use la categoría "Error" para la métrica de conexiones SNAT como ejemplo.
4. Evalúe si se siguen los [patrones](#design-patterns) adecuados.
5. Evalúe si el agotamiento de puertos SNAT debe mitigarse con [direcciones IP de salida + puertos de salida asignados adicionales](#configure-the-allocated-outbound-ports) .

### <a name="design-patterns"></a>Patrones de diseño
Aproveche la reutilización de las conexiones y la agrupación de conexiones siempre que sea posible. Estos patrones evitarán problemas de agotamiento de los recursos y el resultado será un comportamiento predecible. Las primitivas de estos patrones se pueden encontrar en muchas bibliotecas y marcos de desarrollo.

- Las solicitudes atómicas (una solicitud por conexión) no suelen ser una buena decisión de diseño. Estos límites de antipatrón escalan, reducen el rendimiento y disminuyen la confiabilidad. En su lugar, reutilice las conexiones HTTP/S para reducir el número de conexiones y los puertos SNAT asociados. El escalado de la aplicación aumentará y mejorará el rendimiento debido a la reducción de los protocolos de enlace, a la sobrecarga y al costo de la operación criptográfica al usar TLS.
- Si usa DNS personalizado o fuera del clúster, o los servidores ascendentes personalizados de coreDNS tienen en cuenta que DNS puede introducir muchos flujos individuales en el volumen cuando el cliente no almacena en caché el resultado de la resolución DNS. Asegúrese de personalizar coreDNS primero, en lugar de usar servidores DNS personalizados y defina un buen valor de almacenamiento en caché.
- Los flujos UDP (por ejemplo, las búsquedas de DNS) asignan puertos SNAT mientras dure el tiempo de espera de inactividad. Cuanto mayor sea el tiempo de espera de inactividad, mayor será la presión sobre los puertos SNAT. Use un tiempo de espera de inactividad corto (por ejemplo, 4 minutos).
Use los grupos de conexiones para dar forma al volumen de la conexión.
- Nunca abandone de forma silenciosa un flujo TCP y confíe en temporizadores TCP para limpiar el flujo. Si no permite que TCP cierre explícitamente la conexión, el estado permanecerá asignado en los sistemas y puntos de conexión intermedios y hará que los puertos de SNAT no estén disponibles para otras conexiones. Este patrón puede desencadenar errores en la aplicación y el agotamiento de SNAT.
- No cambie los valores del temporizador relacionado con el cierre de TCP de nivel de sistema operativo sin el conocimiento experto de impacto. Aunque la pila de TCP se recuperará, el rendimiento de la aplicación puede verse afectado negativamente si los puntos de conexión de una conexión tienen expectativas no coincidentes. El deseo de cambiar los temporizadores suele ser un signo de un problema de diseño subyacente. Revise las siguientes recomendaciones.

## <a name="moving-from-a-basic-sku-load-balancer-to-standard-sku"></a>Traslado de un equilibrador de carga de la SKU básica a la SKU estándar

Si tiene un clúster existente con el equilibrador de carga de SKU básica, existen diferencias de comportamiento importantes que se deben tener en cuenta al migrar para usar un clúster con la SKU estándar.

Por ejemplo, la realización de implementaciones Blue-Green para migrar clústeres es una práctica común, dado que el tipo `load-balancer-sku` de un clúster solo se puede definir en el momento de la creación del clúster. Sin embargo, los equilibradores de carga de *SKU básica* usan direcciones IP de *SKU básica* que no son compatibles con los equilibradores de carga de *SKU estándar*, que requieren direcciones IP de *SKU estándar*. Al migrar clústeres para actualizar las SKU del equilibrador de carga, se necesitará una nueva dirección IP con una SKU de dirección IP compatible.

Para conocer más detalles sobre cómo migrar clústeres, consulte [nuestra documentación con consideraciones sobre la migración](aks-migration.md) para ver una lista de temas importantes que se deben tener en cuenta al realizar una migración. Las siguientes limitaciones son también diferencias de comportamiento importantes que se deben tener en cuenta al usar equilibradores de carga de SKU estándar en AKS.

## <a name="limitations"></a>Limitaciones

Las siguientes limitaciones se aplican al crear y administrar clústeres de AKS que admiten un equilibrador de carga con la SKU *estándar*:

* Se requiere al menos una dirección IP pública o un prefijo de dirección IP pública para permitir el tráfico de salida del clúster de AKS. Esta dirección IP o el prefijo de dirección IP pública son necesarios para mantener la conectividad entre el plano de control y los nodos de agente, así como para mantener la compatibilidad con versiones anteriores de AKS. Tiene las siguientes opciones para especificar direcciones IP o prefijos de dirección IP pública con un equilibrador de carga de SKU *estándar*:
    * Proporcione sus propias IP públicas.
    * Proporcione sus propios prefijos de dirección IP pública.
    * Especifique un número hasta 100 para permitir que el clúster de AKS cree esa cantidad de direcciones IP públicas de SKU *estándar* en el mismo grupo de recursos que se creó como el clúster de AKS, que normalmente se denomina con *MC_* al principio. AKS asigna la dirección IP pública al equilibrador de carga de SKU *estándar*. De forma predeterminada, se creará automáticamente una IP pública en el mismo grupo de recursos que el clúster de AKS, si no se especifica ninguna dirección IP pública, prefijo de dirección IP pública o número de direcciones IP. Asimismo, debe permitir las direcciones públicas y evitar la creación de cualquier directiva de Azure Policy que prohíba la creación de direcciones IP.
* Una dirección IP pública creada con AKS no se puede volver a usar como dirección IP pública propia personalizada. El usuario debe crear y administrar todas las direcciones IP públicas personalizadas.
* La definición de la SKU del equilibrador de carga solo puede realizarse cuando se crea un clúster de AKS. No se puede cambiar la SKU del equilibrador de carga una vez creado un clúster de AKS.
* Solo se puede usar un tipo de SKU de equilibrador de carga (básica o estándar) en un único clúster.
* Los equilibradores de carga de SKU *estándar* solo admiten direcciones IP de SKU *estándar*.


## <a name="next-steps"></a>Pasos siguientes

Más información sobre los servicios de Kubernetes en la [documentación de servicios de Kubernetes][kubernetes-services].

Obtenga más información sobre el uso de una instancia de Load Balancer interna para el tráfico de entrada en la [documentación de la instancia de Load Balancer interna de AKS](internal-lb.md).

<!-- LINKS - External -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/
[aks-engine]: https://github.com/Azure/aks-engine

<!-- LINKS - Internal -->
[advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-sp]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[az-aks-show]: /cli/azure/aks#az_aks_show
[az-aks-create]: /cli/azure/aks#az_aks_create
[az-aks-get-credentials]: /cli/azure/aks#az_aks_get_credentials
[az-aks-install-cli]: /cli/azure/aks#az_aks_install_cli
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-group-create]: /cli/azure/group#az_group_create
[az-provider-register]: /cli/azure/provider#az_provider_register
[az-network-lb-outbound-rule-list]: /cli/azure/network/lb/outbound-rule#az_network_lb_outbound_rule_list
[az-network-public-ip-show]: /cli/azure/network/public-ip#az_network_public_ip_show
[az-network-public-ip-prefix-show]: /cli/azure/network/public-ip/prefix#az_network_public_ip_prefix_show
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
[azure-lb]: ../load-balancer/load-balancer-overview.md
[azure-lb-comparison]: ../load-balancer/skus.md
[azure-lb-outbound-rules]: ../load-balancer/load-balancer-outbound-connections.md#outboundrules
[azure-lb-outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md
[azure-lb-outbound-preallocatedports]: ../load-balancer/load-balancer-outbound-connections.md#preallocatedports
[azure-lb-outbound-rules-overview]: ../load-balancer/load-balancer-outbound-connections.md#outboundrules
[install-azure-cli]: /cli/azure/install-azure-cli
[internal-lb-yaml]: internal-lb.md#create-an-internal-load-balancer
[kubernetes-concepts]: concepts-clusters-workloads.md
[use-kubenet]: configure-kubenet.md
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[use-multiple-node-pools]: use-multiple-node-pools.md
[troubleshoot-snat]: #troubleshooting-snat
[service-tags]: ../virtual-network/network-security-groups-overview.md#service-tags