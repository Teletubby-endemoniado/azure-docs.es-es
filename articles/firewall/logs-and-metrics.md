---
title: Introducción a la supervisión de métricas y registros de Azure Firewall
description: Puede supervisar Azure Firewall mediante los registros del firewall. También puede usar los registros de actividad para auditar las operaciones de los recursos de Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 04/02/2021
ms.author: victorh
ms.openlocfilehash: 504ed485115f48b252027431a5b69e2a6b5f33f3
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124763583"
---
# <a name="azure-firewall-logs-and-metrics"></a>Métricas y registros de Azure Firewall

Puede supervisar Azure Firewall mediante los registros del firewall. También puede usar los registros de actividad para auditar las operaciones de los recursos de Azure Firewall.

Se puede acceder a algunos de estos registros mediante el portal. Se pueden enviar registros a los [registros de Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md), a Storage y a Event Hubs, y se pueden analizar en los registros de Azure Monitor o mediante otras herramientas como Excel y Power BI.

Las métricas son ligeras y pueden admitir escenarios casi en tiempo real, lo que las hace útiles para alertas y detección rápida de problemas.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

 Los siguientes registros de diagnóstico están disponibles para Azure Firewall:

* **Registro de regla de aplicación**

   El registro de la regla de aplicación se guarda en una cuenta de almacenamiento, se transmite a Event Hubs o se envía a los registros de Azure Monitor solo si se ha habilitado para cada instancia de Azure Firewall. Cada nueva conexión que coincida con una de las reglas de la aplicación configuradas dará como resultado un registro de la conexión aceptada o rechazada. Los datos se registran en formato JSON, tal y como se muestra en los ejemplos siguientes:

   ```
   Category: application rule logs.
   Time: log timestamp.
   Properties: currently contains the full message.
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

   ```json
   {
     "category": "AzureFirewallApplicationRule",
     "time": "2018-04-16T23:45:04.8295030Z",
     "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
     "operationName": "AzureFirewallApplicationRuleLog",
     "properties": {
         "msg": "HTTPS request from 10.11.2.4:53344 to www.bing.com:443. Action: Allow. Rule Collection: ExampleRuleCollection. Rule: ExampleRule. Web Category: SearchEnginesAndPortals"
     }
   }
   ```

* **Registro de regla de red**

   El registro de la regla de red se guarda en una cuenta de almacenamiento, se transmite a Event Hubs o se envía a los registros de Azure Monitor solo si se ha habilitado para cada instancia de Azure Firewall. Cada nueva conexión que coincida con una de las reglas de red configuradas dará como resultado un registro de la conexión aceptada o rechazada. Los datos se registran en formato JSON, tal y como se muestra en el ejemplo siguiente:

   ```
   Category: network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message.
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

* **Registro de proxy DNS**

   El registro de proxy DNS se guarda en una cuenta de almacenamiento, se transmite a Event Hubs o se envía a los registros de Azure Monitor solo si se ha habilitado para cada instancia de Azure Firewall. Este registro realiza un seguimiento de los mensajes DNS a un servidor DNS configurado mediante el proxy DNS. Los datos se registran en formato JSON, tal y como se muestra en los ejemplos siguientes:


   ```
   Category: DNS proxy logs.
   Time: log timestamp.
   Properties: currently contains the full message.
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   Correcto:
   ```json
   {
     "category": "AzureFirewallDnsProxy",
     "time": "2020-09-02T19:12:33.751Z",
     "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
     "operationName": "AzureFirewallDnsProxyLog",
     "properties": {
         "msg": "DNS Request: 11.5.0.7:48197 – 15676 AAA IN md-l1l1pg5lcmkq.blob.core.windows.net. udp 55 false 512 NOERROR - 0 2.000301956s"
     }
   }
   ```

   Error:

   ```json
   {
     "category": "AzureFirewallDnsProxy",
     "time": "2020-09-02T19:12:33.751Z",
     "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
     "operationName": "AzureFirewallDnsProxyLog",
     "properties": {
         "msg": " Error: 2 time.windows.com.reddog.microsoft.com. A: read udp 10.0.1.5:49126->168.63.129.160:53: i/o timeout”
     }
   }
   ```

   Formato msg:

   `[client’s IP address]:[client’s port] – [query ID] [type of the request] [class of the request] [name of the request] [protocol used] [request size in bytes] [EDNS0 DO (DNSSEC OK) bit set in the query] [EDNS0 buffer size advertised in the query] [response CODE] [response flags] [response size] [response duration]`

Tiene tres opciones para almacenar los archivos de registro:

* **Cuenta de almacenamiento**: cuentas que resultan especialmente útiles para registros cuando estos se almacenan durante mucho tiempo y se revisan cuando es necesario.
* **Centros de eventos**: es una buena opción para la integración con otras herramientas de administración de eventos e información de seguridad (SIEM) para obtener alertas sobre los recursos.
* **Registros de Azure Monitor**: se usan para la supervisión general en tiempo real de la aplicación o para examinar las tendencias.

## <a name="activity-logs"></a>Registros de actividad

   Las entradas del registro de actividades se recopilan de forma predeterminada y se pueden ver en Azure Portal.

   Puede usar el [registro de actividades de Azure](../azure-monitor/essentials/activity-log.md) (anteriormente conocido como registros operativos y registros de auditoría) para ver todas las operaciones enviadas a sus suscripciones de Azure.

## <a name="metrics"></a>Métricas

Las métricas de Azure Monitor son valores numéricos que describen algunos aspectos de un sistema en un momento dado. Las métricas se recopilan cada minuto y son útiles para las alertas porque se pueden muestrear con frecuencia. Una alerta puede activarse rápidamente con una lógica relativamente simple.

Las siguientes métricas están disponibles para Azure Firewall:

- **Número de llamadas de reglas de aplicación**: el número de veces que se ha alcanzado una regla de aplicación.

    Unidad: número

- **Número de llamadas de reglas de red**: el número de veces que se ha alcanzado una regla de red.

    Unidad: número

- **Datos procesados**: suma de los datos que atraviesan el firewall en un período de tiempo determinado.

    Unidad: bytes

- **Rendimiento**: velocidad de los datos que atraviesan el firewall por segundo.

    Unidad: bits por segundo

- **Estado de mantenimiento del firewall**: indica el estado del firewall en base a la disponibilidad del puerto SNAT.

    Unit: porcentaje

   Esta métrica tiene dos dimensiones:
  - Estado: los valores posibles son *Correcto*, *Degradado* e *Incorrecto.*
  - Motivo: indica el motivo del estado correspondiente del firewall. 

     Si se usan puertos SNAT > 95 %, se consideran agotados y el mantenimiento tiene un 50 % con estado =**degradado** y razón =**puerto SNAT**. El firewall mantiene el procesamiento del tráfico y las conexiones existentes no se ven afectadas. Sin embargo, es posible que de forma intermitente no se establezcan las nuevas conexiones.

     Si se usan puertos SNAT < 95 %, el firewall se considera correcto y el mantenimiento se muestra como 100 %.

     Si no hay informe sobre el uso de puertos SNAT, el mantenimiento se muestra como 0 %. 

- **Uso de puertos SNAT**: el porcentaje de puertos SNAT que el firewall ha utilizado.

    Unit: porcentaje

   Al agregar más direcciones IP públicas al firewall, hay más puertos SNAT disponibles, lo que reduce el uso de estos puertos. Además, cuando el firewall se escala horizontalmente por distintos motivos (por ejemplo, CPU o rendimiento), los puertos SNAT adicionales también pasan a estar disponibles. De forma eficaz, un porcentaje determinado del uso de puertos SNAT puede reducirse sin agregar ninguna dirección IP pública, simplemente porque el servicio se ha escalado horizontalmente. Puede controlar directamente el número de direcciones IP públicas disponibles para aumentar los puertos disponibles en el firewall. Sin embargo, no puede controlar directamente el escalado del firewall.

   Si el firewall agota los puertos SNAT, debe agregar al menos cinco direcciones IP públicas. Así se aumenta el número de puertos SNAT disponibles. Para más información, consulte [Características de Azure Firewall](features.md#multiple-public-ip-addresses).


## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo supervisar las métricas y los registros de Azure Firewall, consulte [Tutorial: Supervisión de los registros de Azure Firewall](./firewall-diagnostics.md).

- Para más información sobre las métricas en Azure Monitor, consulte [Métricas en Azure monitor](../azure-monitor/essentials/data-platform-metrics.md).