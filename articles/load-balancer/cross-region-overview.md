---
title: Equilibrador de carga entre regiones (versión preliminar)
titleSuffix: Azure Load Balancer
description: Información general del nivel del equilibrador de carga entre regiones para Azure Load Balancer.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2020
ms.author: allensu
ms.custom: references_regions
ms.openlocfilehash: cf094664fab07e9a75c890899dff9cd0118d12fc
ms.sourcegitcommit: 1d56a3ff255f1f72c6315a0588422842dbcbe502
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2021
ms.locfileid: "129614410"
---
# <a name="cross-region-load-balancer-preview"></a>Equilibrador de carga entre regiones (versión preliminar)

Azure Standard Load Balancer admite el equilibrio de carga entre regiones que habilita escenarios de alta disponibilidad con redundancia geográfica como, por ejemplo:

* Tráfico entrante que se origina desde varias regiones.
* [Conmutación por error global instantánea](#regional-redundancy) a la siguiente implementación regional óptima.
* Distribución de la carga entre regiones a la región de Azure más cercana con [latencia ultrabaja](#ultra-low-latency).
* Capacidad de [escalar verticalmente o reducir verticalmente](#ability-to-scale-updown-behind-a-single-endpoint) detrás de un solo punto de conexión.
* [Dirección IP estática](#static-ip)
* [Conservación de la dirección IP de cliente](#client-ip-preservation)
* [Compilación en la solución del equilibrador de carga existente](#build-cross-region-solution-on-existing-azure-load-balancer) sin curva de aprendizaje

> [!IMPORTANT]
> El equilibrador de carga entre regiones se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

La configuración de direcciones IP de front-end del equilibrador de carga entre regiones es estática y se anuncia a través de la [mayoría de las regiones de Azure](#participating-regions).

:::image type="content" source="./media/cross-region-overview/cross-region-load-balancer.png" alt-text="Diagrama del equilibrador de carga entre regiones." border="true":::

> [!NOTE]
> El puerto de back-end de la regla de equilibrio de carga del equilibrador de carga entre regiones debe coincidir con el puerto de front-end de la regla de equilibrio de carga o la regla NAT de entrada de la instancia de Standard Load Balancer regional. 

### <a name="regional-redundancy"></a>Redundancia regional

Configure la redundancia regional agregando una dirección IP pública de front-end global a los equilibradores de carga existentes. 

Si se produce un error en una región, el tráfico se enruta al siguiente equilibrador de carga regional correcto más cercano.  

El sondeo de estado del equilibrador de carga entre regiones recopila información sobre la disponibilidad cada 20 segundos. Si la disponibilidad de un equilibrador de carga regional baja a 0, el equilibrador de carga entre regiones detectará el error. A continuación, el equilibrador de carga regional se excluye de la rotación. 

:::image type="content" source="./media/cross-region-overview/global-region-view.png" alt-text="Diagrama de la vista del tráfico de la región global." border="true":::

### <a name="ultra-low-latency"></a>Latencia ultrabaja

El algoritmo de equilibrio de carga de proximidad geográfica se basa en la ubicación geográfica de los usuarios y las implementaciones regionales. 

El tráfico iniciado desde un cliente alcanzará la región participante más cercana y circulará por la red troncal global de Microsoft para llegar a la implementación regional más cercana. 

Por ejemplo, tiene un equilibrador de carga entre regiones con instancias de Standard Load Balancer en las regiones de Azure:

* Oeste de EE. UU.
* Norte de Europa

Si se inicia un flujo desde Seattle, el tráfico entra en Oeste de EE. UU. Esta región es la región participante más cercana de Seattle. El tráfico se enruta al equilibrador de carga de la región más cercana, que es Oeste de EE. UU.

El equilibrador de carga entre regiones de Azure usa el algoritmo de equilibrio de carga de proximidad geográfica para la decisión de enrutamiento. 

El modo de distribución de carga configurado de los equilibradores de carga regionales se usa para tomar la decisión de enrutamiento final al usarse varios equilibradores de carga regionales para la proximidad geográfica.

Para más información, consulte [Configurar el modo de distribución para Azure Load Balancer](./load-balancer-distribution-mode.md).


### <a name="ability-to-scale-updown-behind-a-single-endpoint"></a>Capacidad de escalar verticalmente o reducir verticalmente detrás de un solo punto de conexión

Cuando expone el punto de conexión global de un equilibrador de carga entre regiones a los clientes, puede agregar o quitar implementaciones regionales detrás del punto de conexión global sin que se produzca ninguna interrupción. 

<!---To learn about how to add or remove a regional deployment from the backend, read more [here](TODO: Insert CLI doc here).--->

### <a name="static-ip"></a>IP estática
El equilibrador de carga entre regiones viene con una dirección pública estática, que garantiza que la dirección IP siga siendo la misma. Para obtener más información sobre la dirección IP estática, lea más [aquí](../virtual-network/public-ip-addresses.md#ip-address-assignment).

### <a name="client-ip-preservation"></a>Conservación de la dirección IP de cliente
El equilibrador de carga entre regiones es un equilibrador de carga de red de tránsito de nivel 4. Este tránsito conserva la dirección IP original del paquete.  La dirección IP original está disponible para el código que se ejecuta en la máquina virtual. Esta conservación permite aplicar lógica específica de una dirección IP.

## <a name="build-cross-region-solution-on-existing-azure-load-balancer"></a>Compilación de la solución entre regiones en la instancia de Azure Load Balancer existente
El grupo de back-end del equilibrador de carga entre regiones contiene uno o varios equilibradores de carga regionales. 

Agregue las implementaciones del equilibrador de carga existentes a un equilibrador de carga entre regiones para una implementación entre regiones altamente disponible.

La **región principal** es donde se implementa el equilibrador de carga entre regiones o la dirección IP pública del nivel global. Esta región no afecta al modo en que se enrutará el tráfico. Si una región principal deja de funcionar, el flujo de tráfico no se ve afectado.

### <a name="home-regions"></a>Regiones principales
* Este de EE. UU. 2
* Oeste de EE. UU.
* Oeste de Europa
* Sudeste de Asia
* Centro de EE. UU.
* Norte de Europa
* Este de Asia

> [!NOTE]
> Solo puede implementar el equilibrador de carga entre regiones o la dirección IP pública de nivel global en una de las siete regiones anteriores.

Una **región participante** es aquella en la que la dirección IP pública global del equilibrador de carga se anuncia.

El tráfico iniciado por el usuario se dirigirá a la región participante más cercana a través de la red principal de Microsoft. 

El equilibrador de carga entre regiones enruta el tráfico al equilibrador de carga regional adecuado.

### <a name="participating-regions"></a>Regiones participantes
* Este de EE. UU. 
* Oeste de Europa 
* Centro de EE. UU. 
* Este de EE. UU. 2 
* Oeste de EE. UU. 
* Norte de Europa 
* Centro-sur de EE. UU. 
* Oeste de EE. UU. 2 
* Sur de Reino Unido 2 
* Sudeste de Asia 
* Centro-Norte de EE. UU 
* Japón Oriental 
* Este de Asia 
* Centro-Oeste de EE. UU. 
* Sudeste de Australia 
* Este de Australia 
* Centro de la India 

## <a name="limitations"></a>Limitaciones

* Las configuraciones de direcciones IP de front-end entre regiones son solo públicas. Actualmente no se admite un front-end interno.

* No se puede agregar un equilibrador de carga interno o privado al grupo de back-end del equilibrador de carga entre regiones. 

* Las configuraciones de direcciones IP de front-end IPv6 entre regiones no se admiten. 

* No se admite el tráfico UDP en el equilibrador de carga entre regiones 

* Un sondeo de estado no se puede configurar actualmente. Un sondeo de estado predeterminado recopila de forma automática información de disponibilidad sobre el equilibrador de carga regional cada 20 segundos. 

* Actualmente no existe integración con Azure Kubernetes Service (AKS). Se produce una pérdida de conectividad al implementar un equilibrador de carga entre regiones con el equilibrador de carga estándar cuando hay un clúster de AKS implementado en el back-end.

## <a name="pricing-and-sla"></a>Precios y contrato de nivel de servicio
El equilibrador de carga entre regiones comparte el [Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/load-balancer/v1_0/ ) de la instancia de Standard Load Balancer.

 
## <a name="next-steps"></a>Pasos siguientes

- Consulte [Tutorial: Creación de un equilibrador de carga entre regiones mediante Azure Portal](tutorial-cross-region-portal.md) para crear un equilibrador de carga entre regiones.
- Obtenga más información sobre el [equilibrador de carga entre regiones.](https://www.youtube.com/watch?v=3awUwUIv950)
- Más información sobre [Azure Load Balancer](load-balancer-overview.md).
