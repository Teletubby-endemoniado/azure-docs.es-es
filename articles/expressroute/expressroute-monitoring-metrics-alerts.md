---
title: 'Azure ExpressRoute: Supervisión, métricas y alertas'
description: Obtenga información sobre la supervisión, las métricas y las alertas de Azure ExpressRoute con Azure Monitor, la ventanilla única para todas las métricas, alertas y registros de diagnóstico en Azure.
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 09/14/2021
ms.author: duau
ms.openlocfilehash: ebb661500fdf14d19218704906d24f391389bec8
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128667224"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>Supervisión, métricas y alertas de ExpressRoute

Este artículo le servirá para comprender la supervisión, las métricas y las alertas de ExpressRoute con Azure Monitor. Azure Monitor es un centro único para todas las métricas, alertas y registros de diagnóstico en todo Azure.
 
>[!NOTE]
>No se recomienda el uso de **métricas clásicas**.
>

## <a name="expressroute-metrics"></a>Métricas de ExpressRoute

Para ver **Métricas**, vaya a la página *Azure Monitor* y seleccione *Métricas*. Para ver las métricas de **ExpressRoute**, filtre por el tipo de recurso *Circuitos de ExpressRoute*. Para ver las métricas de **Global Reach**, filtre por el tipo de recurso *Circuitos de ExpressRoute* y seleccione un recurso de circuito de ExpressRoute que tenga Global Reach habilitado. Para ver las métricas **ExpressRoute Direct** , filtre el Tipo de recurso por *Puertos ExpressRoute*. 

Una vez seleccionada una métrica, se aplicará la agregación predeterminada. Opcionalmente, puede aplicar la división, que mostrará la métrica con diferentes dimensiones.

> [!IMPORTANT]
> Cuando consulte las métricas de ExpressRoute en Azure Portal, seleccione una granularidad temporal de **5 minutos o superior** para obtener los mejores resultados posibles.
> 
> :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/metric-granularity.png" alt-text="Instantánea de las opciones de granularidad temporal.":::

### <a name="aggregation-types"></a>Tipos de agregación:

El Explorador de métricas admite SUM, MAX, MIN, AVG y COUNT como [tipos de agregación](../azure-monitor/essentials/metrics-charts.md#aggregation). Debe usar el tipo de agregación recomendado al revisar la información detallada de cada métrica de ExpressRoute.

* Suma: la suma de todos los valores capturados durante el intervalo de agregación. 
* Recuento: el número de medidas capturadas durante el intervalo de agregación. 
* Average: el promedio de los valores de la métrica capturados durante el intervalo de agregación. 
* Mínimo: el menor valor capturado durante el intervalo de agregación. 
* Máximo: el mayor valor capturado durante el intervalo de agregación. 

### <a name="expressroute-circuit"></a>Circuito ExpressRoute

| Métrica | Category | Unidad | Tipo de agregación | Descripción | Dimensions |  ¿Se puede exportar con la configuración de diagnóstico? | 
| --- | --- | --- | --- | --- | --- | --- | 
| [Disponibilidad de ARP](#arp) | Disponibilidad | Percent | Average | Disponibilidad de ARP de MSEE a todos los elementos del mismo nivel. | PeeringType, Peer |  Yes | 
| [Disponibilidad de BGP](#bgp) | Disponibilidad | Percent | Average | Disponibilidad de BGP de MSEE a todos los elementos del mismo nivel. | PeeringType, Peer |  Yes | 
| [BitsInPerSecond](#circuitbandwidth) | Tráfico | BitsPerSecond | Average | Bits que ingresan a Azure por segundo | PeeringType | No | 
| [BitsOutPerSecond](#circuitbandwidth) | Tráfico | BitsPerSecond | Average | Bits que egresan de Azure por segundo | PeeringType | No | 
| DroppedInBitsPerSecond | Tráfico | BitsPerSecond | Average | Bits de entrada de datos descartados por segundo | Tipo de emparejamiento | Sí | 
| DroppedOutBitsPerSecond | Tráfico | BitPerSecond | Average | Bits de salida de datos descartados por segundo | Tipo de emparejamiento | Yes | 
| GlobalReachBitsInPerSecond | Tráfico | BitsPerSecond | Average | Bits que ingresan a Azure por segundo | PeeredCircuitSKey | No | 
| GlobalReachBitsOutPerSecond | Tráfico | BitsPerSecond | Average | Bits que egresan de Azure por segundo | PeeredCircuitSKey | No | 

>[!NOTE]
>El uso de *GlobalGlobalReachBitsInPerSecond* y *GlobalGlobalReachBitsOutPerSecond* solo será visible si se ha establecido al menos una conexión Global Reach.
>

### <a name="expressroute-gateways"></a>Puertas de enlace de ExpressRoute

| Métrica | Category | Unidad | Tipo de agregación | Descripción | Dimensions | ¿Se puede exportar con la configuración de diagnóstico? | 
| --- | --- | --- | --- | --- | --- | --- | 
| [Uso de CPU](#cpu) | Rendimiento | Count | Average | Uso de CPU de la puerta de enlace de ExpressRoute | roleInstance | Yes | 
| [Paquetes por segundo](#packets) | Rendimiento | CountPerSecond | Average | Recuento de paquetes de puerta de enlace de ExpressRoute | roleInstance | No | 
| [Número de rutas anunciadas al nodo del mismo nivel](#advertisedroutes) | Disponibilidad | Count | Máxima | Recuento de rutas anunciadas al par por ExpressRouteGateway | roleInstance | Yes | 
| [Número de rutas aprendidas del nodo del mismo nivel](#learnedroutes)| Disponibilidad | Count | Máxima | Recuento de rutas aprendidas del par por ExpressRouteGateway | roleInstance | Yes | 
| [Frecuencia de cambio de rutas](#frequency) | Disponibilidad | Count | Total | Frecuencia de cambio de rutas en la puerta de enlace de ExpressRoute | roleInstance | No | 
| [Número de máquinas virtuales en la red virtual](#vm) | Disponibilidad | Count | Máxima | Número de máquinas virtuales en la red virtual | Sin dimensiones | No | 

### <a name="expressroute-gateway-connections"></a>Conexiones de puerta de enlace de ExpressRoute

| Métrica | Category | Unidad | Tipo de agregación | Descripción | Dimensions | ¿Se puede exportar con la configuración de diagnóstico? | 
| --- | --- | --- | --- | --- | --- | --- | 
| [BitsInPerSecond](#connectionbandwidth) | Tráfico | BitsPerSecond | Average | Bits que ingresan a Azure por segundo | ConnectionName | No | 
| [BitsOutPerSecond](#connectionbandwidth) | Tráfico | BitsPerSecond | Average | Bits que egresan de Azure por segundo | ConnectionName | No | 
| DroppedInBitsPerSecond | Tráfico | BitsPerSecond | Average | Bits de entrada de datos descartados por segundo | ConnectionName | Sí | 
| DroppedOutBitsPerSecond | Tráfico | BitPerSecond | Average | Bits de salida de datos descartados por segundo | ConnectionName | Yes | 

### <a name="expressroute-direct"></a>ExpressRoute Direct

| Métrica | Category | Unidad | Tipo de agregación | Descripción | Dimensions | ¿Se puede exportar con la configuración de diagnóstico? | 
| --- | --- | --- | --- | --- | --- | --- | 
| [BitsInPerSecond](#directin) | Tráfico | BitsPerSecond | Average | Bits que ingresan a Azure por segundo | Vínculo | Yes | 
| [BitsOutPerSecond](#directout) | Tráfico | BitsPerSecond | Average | Bits que egresan de Azure por segundo | Vínculo | Sí | 
| DroppedInBitsPerSecond | Tráfico | BitsPerSecond | Average | Bits de entrada de datos descartados por segundo | Link | Sí | 
| DroppedOutBitsPerSecond | Tráfico | BitPerSecond | Average | Bits de salida de datos descartados por segundo | Link  | Yes | 
| [AdminState](#admin) | Conectividad física | Count | Average | Estado de administración del puerto | Vínculo | Yes | 
| [LineProtocol](#line) | Conectividad física | Count | Average | Estado del protocolo de línea del puerto | Vínculo | Yes | 
| [RxLightLevel](#rxlight) | Conectividad física | Count | Average | Nivel de iluminación de recepción en dBm | Link, Lane | Yes | 
| [TxLightLevel](#txlight) | Conectividad física | Count | Average | Nivel de iluminación de transmisión en dBm | Link, Lane | Yes |

## <a name="circuits-metrics"></a>Métricas de circuito

### <a name="bits-in-and-out---metrics-across-all-peerings"></a><a name = "circuitbandwidth"></a>Bits dentro y fuera: métricas de todos los emparejamientos

Tipo de agregación: *Avg*

Puede ver las métricas de todos los emparejamientos de un circuito ExpressRoute determinado.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg" alt-text="métricas de circuito":::

### <a name="bits-in-and-out---metrics-per-peering"></a>Bits dentro y fuera: métricas por emparejamiento

Tipo de agregación: *Avg*

Puede ver las métricas de emparejamiento público, privado y de Microsoft en bits por segundo.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg" alt-text="métricas por emparejamiento":::

### <a name="bgp-availability---split-by-peer"></a><a name = "bgp"></a>Disponibilidad de BGP: dividido por nodo del mismo nivel  

Tipo de agregación: *Avg*

Puede ver la disponibilidad casi en tiempo real de BGP (conectividad de nivel 3) entre emparejamientos y pares (enrutadores de ExpressRoute principales y secundarios). En este panel se muestra el estado de la sesión de BGP principal como activo para el emparejamiento privado y el estado de la segunda sesión BGP como inactivo para el emparejamiento privado. 

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erBgpAvailabilityMetrics.jpg" alt-text="Disponibilidad de BGP por par":::

### <a name="arp-availability---split-by-peering"></a><a name = "arp"></a>Disponibilidad de ARP: dividido por emparejamiento  

Tipo de agregación: *Avg*

Puede ver la disponibilidad casi en tiempo real de [ARP](./expressroute-troubleshooting-arp-resource-manager.md) (conectividad de nivel 3) entre emparejamientos y pares (enrutadores de ExpressRoute principales y secundarios). En este panel se muestra el estado de la sesión de ARP del emparejamiento privado como activo entre ambos pares, pero inactivo para el emparejamiento de Microsoft para ambos pares. La agregación predeterminada (promedio) se usó en ambos pares.  

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erArpAvailabilityMetrics.jpg" alt-text="Disponibilidad de ARP por par":::

## <a name="expressroute-direct-metrics"></a>Métricas directas de ExpressRoute

### <a name="admin-state---split-by-link"></a><a name = "admin"></a>Estado de administración: dividido por vínculo

Tipo de agregación: *Avg*

Puede ver el estado del administrador para cada vínculo del par de puertos de ExpressRoute Direct. El estado del administrador representa si el puerto físico está activo o inactivo. Este estado es necesario para pasar el tráfico a través de la conexión de ExpressRoute Direct.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/adminstate-per-link.jpg" alt-text="Estado de administrador ER directo":::

### <a name="bits-in-per-second---split-by-link"></a><a name = "directin"></a>Bits de entrada por segundo: dividido por vínculo

Tipo de agregación: *Avg*

Puede ver los bits por segundo en ambos enlaces del par de puertos directos de ExpressRoute. Supervise este panel para comparar el ancho de banda de entrada de ambos vínculos.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/bits-in-per-second-per-link.jpg" alt-text="ER bits directos por segundo":::

### <a name="bits-out-per-second---split-by-link"></a><a name = "directout"></a>Bits de salida por segundo: dividido por vínculo

Tipo de agregación: *Avg*

También puede ver los bits por segundo en ambos enlaces del par de puertos directos de ExpressRoute. Supervise este panel para comparar el ancho de banda de salida de ambos vínculos.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/bits-out-per-second-per-link.jpg" alt-text="ER bits directos por segundo":::

### <a name="line-protocol---split-by-link"></a><a name = "line"></a>Protocolo de línea: dividido por vínculo

Tipo de agregación: *Avg*

Puede ver el protocolo de línea en cada enlace del par de puertos directos de ExpressRoute. El protocolo de línea indica si el vínculo físico está en funcionamiento a través de ExpressRoute Direct. Supervise este panel y establezca alertas para saber cuándo se ha interrumpido la conexión física.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/line-protocol-per-link.jpg" alt-text="Protocolo ER de línea directa":::

### <a name="rx-light-level---split-by-link"></a><a name = "rxlight"></a>Nivel de luz de Rx: dividido por vínculo

Tipo de agregación: *Avg*

Puede ver el nivel de luz de Rx (el nivel de luz que el puerto directo de ExpressRoute está **recibiendo**) para cada puerto. Un nivel de luz de Rx correcto suele situarse en un intervalo de -10 dBm a 0 dBm. Establezca alertas para recibir una notificación si el nivel de luz de Rx está fuera del intervalo correcto.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/rxlight-level-per-link.jpg" alt-text="Nivel de luz de recepción de línea directa de ER":::

### <a name="tx-light-level---split-by-link"></a><a name = "txlight"></a>Nivel de luz de Tx: dividido por vínculo

Tipo de agregación: *Avg*

Puede ver el nivel de luz Tx (el nivel de luz que el puerto directo de ExpressRoute **transmite**) para cada puerto. Un nivel de luz de Tx correcto suele situarse en un intervalo de -10 dBm a 0 dBm. Establezca alertas para recibir una notificación si el nivel de luz Tx está fuera del intervalo correcto.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/txlight-level-per-link.jpg" alt-text="Nivel de luz de recepción de línea directa de ER":::

## <a name="expressroute-virtual-network-gateway-metrics"></a>Métricas de puerta de enlace de red virtual de ExpressRoute

Tipo de agregación: *Avg*

Al implementar una puerta de enlace de ExpressRoute, Azure administra el proceso y las funciones de la puerta de enlace. Hay seis métricas de puerta de enlace disponibles para comprender mejor el rendimiento de la puerta de enlace:

* Uso de CPU
* Paquetes por segundo
* Recuento de rutas anunciadas al par
* Recuento de rutas aprendidas del par
* Frecuencia de cambio de rutas
* Número de máquinas virtuales en la red virtual  

Se recomienda encarecidamente establecer alertas para cada una de estas métricas para saber cuándo la puerta de enlace puede estar experimentando problemas de rendimiento.

### <a name="cpu-utilization---split-instance"></a><a name = "cpu"></a>Uso de CPU: división de instancia

Tipo de agregación: *Avg*

Puede ver el uso de CPU de cada una de las instancias de la puerta de enlace. El uso de CPU puede repuntar brevemente durante el mantenimiento rutinario del host, pero un elevado uso de CPU prolongado podría indicar que la puerta de enlace está alcanzando un cuello de botella de rendimiento. Este problema puede resolverse aumentando el tamaño de la puerta de enlace de ExpressRoute. Establezca una alerta sobre la frecuencia con la que el uso de CPU supera un umbral determinado.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/cpu-split.jpg" alt-text="Captura de pantalla del uso de CPU: métricas de división.":::

### <a name="packets-per-second---split-by-instance"></a><a name = "packets"></a>Paquetes por segundo: dividido por instancia

Tipo de agregación: *Avg*

Esta métrica captura el número de paquetes entrantes que atraviesan la puerta de enlace de ExpressRoute. Aquí debería mostrarse un flujo coherente de datos si la puerta de enlace recibe tráfico de la red local. Establezca una alerta para cuando el número de paquetes por segundo caiga por debajo de un umbral, lo que indicará que la puerta de enlace ya no recibe tráfico.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/pps-split.jpg" alt-text="Captura de pantalla de paquetes por segundo: métricas de división.":::

### <a name="count-of-routes-advertised-to-peer---split-by-instance"></a><a name = "advertisedroutes"></a>Número de rutas anunciadas al nodo del mismo nivel: dividido por instancia

Tipo de agregación: *Count*

Esta métrica es el recuento del número de rutas que la puerta de enlace de ExpressRoute anuncia al circuito. Los espacios de direcciones pueden incluir redes virtuales que están conectadas mediante el emparejamiento de VNet y usan la puerta de enlace de ExpressRoute remota. El número de rutas debe ser coherente a menos que se produzcan cambios frecuentes en los espacios de direcciones de la red virtual. Establezca una alerta para cuando el número de rutas anunciadas caiga por debajo del umbral para el número de espacios de direcciones de red virtual que conozca.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/count-of-routes-advertised-to-peer.png" alt-text="Captura de pantalla del recuento de rutas anunciadas al par.":::

### <a name="count-of-routes-learned-from-peer---split-by-instance"></a><a name = "learnedroutes"></a>Número de rutas aprendidas del nodo del mismo nivel: dividido por instancia

Tipo de agregación: *Max*

Esta métrica muestra el número de rutas que la puerta de enlace de ExpressRoute está aprendiendo de los pares conectados al circuito de ExpressRoute. Estas rutas pueden ser de otra red virtual conectada al mismo circuito o aprenderse del entorno local. Establezca una alerta para cuando el número de rutas aprendidas caiga por debajo de un umbral determinado. Esto podría indicar que la puerta de enlace está experimentando un problema de rendimiento o que los pares remotos ya no anuncian rutas al circuito de ExpressRoute. 

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/count-of-routes-learned-from-peer.png" alt-text="Captura de pantalla del recuento de rutas aprendidas del par.":::

### <a name="frequency-of-routes-change---split-by-instance"></a><a name = "frequency"></a>Frecuencia de cambio de rutas: dividido por instancia

Tipo de agregación: *Sum*

Esta métrica muestra la frecuencia de las rutas que se aprenden de pares remotos o se anuncian a pares remotos. En primer lugar, debe investigar los dispositivos locales para comprender por qué la red cambia con tanta frecuencia. Si las rutas cambian con mucha frecuencia podría existir un problema de rendimiento en la puerta de enlace de ExpressRoute, que podría resolverse escalando verticalmente la SKU de la puerta de enlace. Establezca una alerta para un umbral de frecuencia para saber cuándo la puerta de enlace de ExpressRoute experimenta cambios de ruta anómalos.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/frequency-of-routes-changed.png" alt-text="Captura de pantalla de la métrica de frecuencia de cambios en las rutas.":::

### <a name="number-of-vms-in-the-virtual-network"></a><a name = "vm"></a>Número de máquinas virtuales en la red virtual

Tipo de agregación: *Max*

Esta métrica muestra el número de máquinas virtuales que usan la puerta de enlace de ExpressRoute. El número de máquinas virtuales puede incluir máquinas virtuales de redes virtuales emparejadas que usan la misma puerta de enlace de ExpressRoute. Establezca una alerta para esta métrica si el número de máquinas virtuales supera un umbral determinado que podría afectar al rendimiento de la puerta de enlace. 

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/number-of-virtual-machines-virtual-network.png" alt-text="Captura de pantalla de la métrica del número de máquinas virtuales en la red virtual.":::

## <a name="expressroute-gateway-connections-in-bitsseconds"></a><a name = "connectionbandwidth"></a>Conexiones de puerta de enlace de ExpressRoute en bits por segundo

Tipo de agregación: *Avg*

Esta métrica muestra el uso de ancho de banda para una conexión específica a un circuito de ExpressRoute.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erconnections.jpg" alt-text="Captura de pantalla de la métrica de uso del ancho de banda de conexión de la puerta de enlace.":::

## <a name="alerts-for-expressroute-gateway-connections"></a>Alertas para las conexiones de puerta de enlace de ExpressRoute

1. Para configurar las alertas, vaya a **Azure Monitor** y, a continuación, seleccione **Alertas**.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg" alt-text="alerts":::
2. Seleccione **+Seleccionar destino** y elija el recurso de conexión de puerta de enlace de ExpressRoute.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg" alt-text="Destino":::
3. Defina los detalles de la alerta.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg" alt-text="grupo de acciones":::
4. Defina y agregue el grupo de acciones.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/actiongroup.png" alt-text="adición de grupo de acciones":::

## <a name="alerts-based-on-each-peering"></a>Alertas basadas en cada emparejamiento

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg" alt-text="cada emparejamiento":::

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Configuración de alertas para los registros de actividad en circuitos

En **Criterios de alerta**, puede seleccionar **Registro de actividad** para el tipo de señal y seleccionar la señal.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg" alt-text="registros de actividad":::

## <a name="more-metrics-in-log-analytics"></a>Más métricas en Log Analytics

También puede ver las métricas de ExpressRoute navegando hasta el recurso del circuito de ExpressRoute y seleccionando la pestaña *Registros*. Para cualquier métrica que consulte, la salida contendrá las columnas siguientes.

| **Columna** | **Tipo** | **Descripción** | 
|  ---  |  ---  |  ---  | 
| TimeGrain | string | PT1M (los valores de métricas se insertan cada minuto) | 
| Count | real | Normalmente es igual a 2 (cada MSEE inserta un solo valor de métrica cada minuto) | 
| Mínima | real | El mínimo de los dos valores de métrica insertados por los dos MSEE | 
| Máxima | real | El máximo de los dos valores de métrica insertados por los dos MSEE | 
| Average | real | Igual a (mínimo + máximo)/2 | 
| Total | real | Suma de los dos valores de métrica de ambos MSEE (el valor principal en el que se centrará para la métrica consultada) | 
  
## <a name="next-steps"></a>Pasos siguientes

Configure su conexión ExpressRoute.
  
* [Creación y modificación de un circuito](expressroute-howto-circuit-arm.md)
* [Creación y modificación de la configuración de emparejamiento](expressroute-howto-routing-arm.md)
* [Vinculación de redes virtuales a circuitos ExpressRoute](expressroute-howto-linkvnet-arm.md)
