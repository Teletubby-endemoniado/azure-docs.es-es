---
title: Configuración del acceso a un registro público
description: Configure reglas de IP para permitir el acceso a un registro de contenedor de Azure desde intervalos de direcciones o direcciones IP públicas seleccionadas.
ms.topic: article
ms.date: 07/30/2021
ms.openlocfilehash: 3a4a4a28dfbcd859cf97be6799e24a8110add436
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128627014"
---
# <a name="configure-public-ip-network-rules"></a>Configuración de reglas de red de dirección IP pública

De manera predeterminada los registros de contenedor de aceptan las conexiones a través de Internet de hosts de cualquier red. En este artículo se muestra cómo configurar el registro de contenedor para permitir el acceso solo desde intervalos de direcciones o direcciones IP públicas específicas. Se proporcionan los pasos equivalentes si se usa mediante la CLI de Azure y Azure Portal.

Las reglas de red IP se configuran en el punto de conexión del registro público. Las reglas de red IP no se aplican a los puntos de conexión privados configurados con [Private Link](container-registry-private-link.md).

La configuración de reglas de acceso IP está disponible en el nivel de servicio de un registro de contenedor **Premium**. Para obtener información sobre los límites y niveles del servicio de registro, vea [Niveles de Azure Container Registry](container-registry-skus.md).

Cada registro admite un máximo de 100 reglas de acceso de IP.

[!INCLUDE [container-registry-scanning-limitation](../../includes/container-registry-scanning-limitation.md)]

## <a name="access-from-selected-public-network---cli"></a>Acceso desde una red pública seleccionada: CLI

### <a name="change-default-network-access-to-registry"></a>Cambio del acceso de red predeterminado al registro

Para limitar el acceso a una red pública seleccionada, primero cambie la acción predeterminada para denegar el acceso. Sustituya el nombre del registro en el siguiente comando [az acr update][az-acr-update]:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

### <a name="add-network-rule-to-registry"></a>Incorporación de una regla de red al registro

Use el comando [az acr network-rule add][az-acr-network-rule-add] para agregar una regla de red al registro que permita el acceso desde una dirección IP pública o un intervalo. Por ejemplo, sustituya el nombre del registro de contenedor y la dirección IP pública de una máquina virtual de una red virtual.

```azurecli
az acr network-rule add \
  --name mycontainerregistry \
  --ip-address <public-IP-address>
```

> [!NOTE]
> Después de agregar una regla, esta tarda unos minutos en aplicarse.

## <a name="access-from-selected-public-network---portal"></a>Acceso desde una red pública seleccionada: portal

1. En el portal, vaya al registro de contenedor.
1. En **Configuración**, seleccione **Redes**.
1. En la pestaña **Acceso público**, permita el acceso público desde **Redes seleccionadas**.
1. En **Firewall**, escriba una dirección IP pública, como la dirección IP pública de una máquina virtual de una red virtual. O bien, escriba un rango de direcciones en notación CIDR que contenga la dirección IP de la máquina virtual.
1. Seleccione **Guardar**.

![Configurar una regla de firewall para el registro de contenedor][acr-access-selected-networks]

> [!NOTE]
> Después de agregar una regla, esta tarda unos minutos en aplicarse.

> [!TIP]
> De forma opcional, habilite el acceso al registro desde un equipo cliente local o un intervalo de direcciones IP. Para permitir este acceso, necesita la dirección IPv4 pública del equipo. Para encontrar esta dirección, busque "cuál es mi dirección IP" en un explorador de Internet. La dirección IPv4 del cliente actual también aparece automáticamente cuando se establece la configuración del firewall en la página **Redes** del portal.

## <a name="disable-public-network-access"></a>Deshabilitación del acceso a una red pública

De forma opcional, deshabilite el punto de conexión público en el registro. Al deshabilitar el punto de conexión público, se invalidan todas las configuraciones del firewall. Por ejemplo, puede que quiera deshabilitar el acceso público a un registro protegido de una red virtual mediante [Private Link](container-registry-private-link.md).

> [!NOTE]
> Si el registro se configura en una red virtual con un [punto de conexión de servicio](container-registry-vnet.md), al deshabilitar el acceso al punto de conexión público del registro también se deshabilita el acceso al registro dentro de la red virtual.

### <a name="disable-public-access---cli"></a>Deshabilitación del acceso público: CLI

Para deshabilitar el acceso público mediante la CLI de Azure, ejecute [az acr update][az-acr-update] y establezca `--public-network-enabled` en `false`. El argumento `public-network-enabled` requiere una versión 2.6.0 o posterior de la CLI de Azure. 

```azurecli
az acr update --name myContainerRegistry --public-network-enabled false
```

### <a name="disable-public-access---portal"></a>Deshabilitación del acceso público: portal

1. En el portal, vaya al registro de contenedor y seleccione **Configuración > Redes**.
1. En la pestaña **Acceso público**, en **Permitir el acceso de red público**, seleccione **Deshabilitado**. Después, seleccione **Guardar**.

![Deshabilitación del acceso público][acr-access-disabled]


## <a name="restore-public-network-access"></a>Restauración del acceso a una red pública

Para volver a habilitar el punto de conexión público, actualice la configuración de redes para permitir el acceso público. Al habilitar el punto de conexión público, se invalidan todas las configuraciones del firewall. 

### <a name="restore-public-access---cli"></a>Restauración del acceso público: CLI

Ejecute [az acr update][az-acr-update] y establezca `--public-network-enabled` en `true`. 

> [!NOTE]
> El argumento `public-network-enabled` requiere una versión 2.6.0 o posterior de la CLI de Azure. 

```azurecli
az acr update --name myContainerRegistry --public-network-enabled true
```

### <a name="restore-public-access---portal"></a>Restauración del acceso público: portal

1. En el portal, vaya al registro de contenedor y seleccione **Configuración > Redes**.
1. En la pestaña **Acceso público**, en **Permitir el acceso de red público**, seleccione **Todas las redes**. Después, seleccione **Guardar**.

![Acceso público desde todas las redes][acr-access-all-networks]

## <a name="troubleshoot"></a>Solución de problemas

### <a name="access-behind-https-proxy"></a>Acceso detrás de un proxy HTTPS

Si se establece una regla de red pública o se deniega el acceso público al registro, se generará un error al intentar iniciar sesión en el registro desde una red pública no permitida. También se generará un error en el acceso del cliente detrás de un proxy HTTPS si no se establece una regla de acceso para el proxy. Verá un mensaje de error similar a `Error response from daemon: login attempt failed with status: 403 Forbidden` o `Looks like you don't have access to registry`.

Estos errores también pueden producirse si usa un proxy HTTPS permitido por una regla de acceso a la red, pero el proxy no está configurado correctamente en el entorno del cliente. Compruebe que tanto el cliente como el demonio de Docker estén configurados para el comportamiento del proxy. Para obtener más información, consulte el artículo sobre [el proxy HTTP/HTTPS](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy) en la documentación de Docker.

### <a name="access-from-azure-pipelines"></a>Acceso desde Azure Pipelines

Si usa Azure Pipelines con un registro de contenedor de Azure que limite el acceso a direcciones IP concretas, es posible que la canalización no pueda acceder al registro, porque la dirección IP saliente de la canalización no es fija. De forma predeterminada, la canalización ejecuta trabajos mediante un [agente](/azure/devops/pipelines/agents/agents) hospedado por Microsoft en un grupo de máquinas virtuales con un conjunto de direcciones IP cambiante.

Una solución alternativa consiste en cambiar el agente que se usa para ejecutar la canalización de hospedado por Microsoft a auto-hospedado. Con un agente auto-hospedado que se ejecuta en una máquina [Windows](/azure/devops/pipelines/agents/v2-windows) o [Linux](/azure/devops/pipelines/agents/v2-linux) que administra, controla la dirección IP saliente de la canalización y puede agregar esta dirección en una regla de acceso IP del Registro.

### <a name="access-from-aks"></a>Acceso desde AKS

Si usa Azure Kubernetes Service (AKS) con un registro de contenedor de Azure que limita el acceso a direcciones IP concretas, no puede configurar una dirección IP fija de AKS de forma predeterminada. La dirección IP de salida del clúster de AKS se asigna aleatoriamente.

Para permitir que el clúster de AKS acceda al Registro, tiene estas opciones:

* Si usa Azure Basic Load Balancer, configure una [dirección IP estática](../aks/egress.md) para el clúster de AKS. 
* Si usa Azure Standard Load Balancer, consulte las guía para [controlar el tráfico de salida](../aks/limit-egress-traffic.md) desde el clúster.

## <a name="next-steps"></a>Pasos siguientes

* Para restringir el acceso a un registro mediante un punto de conexión privado de una red virtual, consulte [Configuración de Azure Private Link para un registro de contenedor de Azure](container-registry-private-link.md).
* Si necesita configurar reglas de acceso a un registro desde detrás de un firewall cliente, consulte [Configuración de reglas para acceder a un registro de contenedor de Azure desde detrás de un firewall](container-registry-firewall-access-rules.md).
* Para más instrucciones para solucionar problemas, consulte [Solución de problemas de red con el Registro](container-registry-troubleshoot-access.md).

[az-acr-login]: /cli/azure/acr#az_acr_login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az_acr_network_rule_add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az_acr_network_rule_remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az_acr_network_rule_list
[az-acr-run]: /cli/azure/acr#az_acr_run
[az-acr-update]: /cli/azure/acr#az_acr_update
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com

[acr-access-selected-networks]: ./media/container-registry-access-selected-networks/acr-access-selected-networks.png
[acr-access-disabled]: ./media/container-registry-access-selected-networks/acr-access-disabled.png
[acr-access-all-networks]: ./media/container-registry-access-selected-networks/acr-access-all-networks.png
