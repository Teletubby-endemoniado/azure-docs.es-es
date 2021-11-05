---
title: Esquemas para las alertas de Microsoft Defender for Cloud
description: En este artículo se describen los diferentes esquemas usados por Microsoft Defender for Cloud para las alertas de seguridad.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 10/18/2021
ms.author: memildin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 335f20150490d9f792f9ba552b13b01e64e95673
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131010619"
---
# <a name="security-alerts-schemas"></a>Esquemas de las alertas de seguridad

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Si la suscripción tiene habilitadas las características de seguridad mejorada, recibirá alertas de seguridad cuando Defender for Cloud detecte amenazas para sus recursos.

Puede ver estas alertas de seguridad en las páginas de Microsoft Defender for Cloud ([panel de información general](overview-page.md), [alertas](tutorial-security-incident.md), [páginas de estado de recursos](investigate-resource-health.md) o [panel de protección de cargas de trabajo](workload-protections-dashboard.md)) y a través de herramientas externas como:

- [Microsoft Sentinel](../sentinel/index.yml): SIEM nativo de la nube de Microsoft. El conector de Sentinel obtiene alertas de Microsoft Defender for Cloud y las envía al [área de trabajo de Log Analytics](../azure-monitor/logs/quick-create-workspace.md) para Microsoft Sentinel.
- SIEM de terceros: enviar datos a [Azure Event Hubs](../event-hubs/index.yml). Luego, integre los datos de Event Hubs con un SIEM de terceros. Puede encontrar más información en [Transmisión de alertas a una solución de administración de servicios de TI, SIEM o SOAR](export-to-siem.md).
- [La API REST](/rest/api/securitycenter/): si usa la API REST para acceder a las alertas, consulte la [documentación de Alerts API en línea](/rest/api/securitycenter/alerts).

Si utiliza cualquier método de programación para consumir las alertas, necesitará el esquema correcto para encontrar los campos pertinentes. De igual modo, si va a realizar una exportación a un centro de eventos o a intentar desencadenar Automatización de flujo de trabajo con conectores HTTP genéricos, utilice los esquemas para analizar correctamente los objetos JSON.

>[!IMPORTANT]
> El esquema es ligeramente diferente en cada uno de estos escenarios, así que asegúrese de que selecciona la pestaña pertinente a continuación.


## <a name="the-schemas"></a>Los esquemas 


### <a name="microsoft-sentinel"></a>[Microsoft Sentinel](#tab/schema-sentinel)

El conector de Sentinel obtiene alertas de Microsoft Defender for Cloud y las envía al área de trabajo de Log Analytics para Microsoft Sentinel.

Para crear un caso o incidente de Microsoft Sentinel mediante alertas de Defender for Cloud, necesitará el esquema para esas alertas que se muestran a continuación. 

Para más información acerca de Microsoft Sentinel, consulte [la documentación](../sentinel/index.yml).

[!INCLUDE [Sentinel and workspace schema](../../includes/security-center-alerts-schema-log-analytics-workspace.md)]


### <a name="azure-activity-log"></a>[Registro de actividad de Azure](#tab/schema-activitylog)

Las auditorías de Microsoft Defender for Cloud generaron alertas de seguridad como eventos en el registro de actividad de Azure.

Para ver los eventos de las alertas de seguridad en el registro de actividad, busque el evento Activar alerta como se muestra:

[![Búsqueda del registro de actividad para el evento activar alerta.](media/alerts-schemas/sample-activity-log-alert.png)](media/alerts-schemas/sample-activity-log-alert.png#lightbox)


### <a name="sample-json-for-alerts-sent-to-azure-activity-log"></a>JSON de ejemplo para alertas enviadas al registro de actividad de Azure

```json
{
    "channels": "Operation",
    "correlationId": "2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff",
    "description": "PREVIEW - Role binding to the cluster-admin role detected. Kubernetes audit log analysis detected a new binding to the cluster-admin role which gives administrator privileges.\r\nUnnecessary administrator privileges might cause privilege escalation in the cluster.",
    "eventDataId": "2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff",
    "eventName": {
        "value": "PREVIEW - Role binding to the cluster-admin role detected",
        "localizedValue": "PREVIEW - Role binding to the cluster-admin role detected"
    },
    "category": {
        "value": "Security",
        "localizedValue": "Security"
    },
    "eventTimestamp": "2019-12-25T18:52:36.801035Z",
    "id": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RESOURCE_GROUP_NAME/providers/Microsoft.Security/locations/centralus/alerts/2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff/events/2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff/ticks/637128967568010350",
    "level": "Informational",
    "operationId": "2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff",
    "operationName": {
        "value": "Microsoft.Security/locations/alerts/activate/action",
        "localizedValue": "Activate Alert"
    },
    "resourceGroupName": "RESOURCE_GROUP_NAME",
    "resourceProviderName": {
        "value": "Microsoft.Security",
        "localizedValue": "Microsoft.Security"
    },
    "resourceType": {
        "value": "Microsoft.Security/locations/alerts",
        "localizedValue": "Microsoft.Security/locations/alerts"
    },
    "resourceId": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RESOURCE_GROUP_NAME/providers/Microsoft.Security/locations/centralus/alerts/2518250008431989649_e7313e05-edf4-466d-adfd-35974921aeff",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-12-25T19:14:03.5507487Z",
    "subscriptionId": "SUBSCRIPTION_ID",
    "properties": {
        "clusterRoleBindingName": "cluster-admin-binding",
        "subjectName": "for-binding-test",
        "subjectKind": "ServiceAccount",
        "username": "masterclient",
        "actionTaken": "Detected",
        "resourceType": "Kubernetes Service",
        "severity": "Low",
        "intent": "[\"Persistence\"]",
        "compromisedEntity": "ASC-IGNITE-DEMO",
        "remediationSteps": "[\"Review the user in the alert details. If cluster-admin is unnecessary for this user, consider granting lower privileges to the user.\"]",
        "attackedResourceType": "Kubernetes Service"
    },
    "relatedEvents": []
}
```

### <a name="the-data-model-of-the-schema"></a>El modelo de datos del esquema

|Campo|Descripción|
|----|----|
|**channels**|Constante, "Operation"|
|**correlationId**|El identificador de alerta de Microsoft Defender for Cloud|
|**description**|Descripción de la alerta|
|**eventDataId**|Consulte correlationId|
|**eventName**|Los subcampos value y localizedValue contienen el nombre para mostrar de la alerta|
|**category**|Los subcampos Value y localizedValue son constantes: "Security"|
|**eventTimestamp**|La marca de tiempo UTC de cuando se generó la alerta|
|**id**|Identificador completo de la alerta|
|**level**|Constante, "Informational"|
|**operationId**|Consulte correlationId|
|**operationName**|El campo value es constante: "Microsoft.Security/locations/alerts/activate/action" y el valor localizado será "Activar alerta" (puede que esté posiblemente localizado en función de la configuración regional del usuario)|
|**resourceGroupName**|Incluirá el nombre del grupo de recursos|
|**resourceProviderName**|Los subcampos value y localizedValue son constantes: "Microsoft.Security"|
|**resourceType**|Los subcampos value y localizedValue son constantes: "Microsoft.Security/locations/alerts"|
|**resourceId**|El identificador de recursos de Azure completo|
|**status**|Los subcampos value y localizedValue son constantes: "Active"|
|**subStatus**|Los subcampos value y localizedValue están vacíos|
|**submissionTimestamp**|Marca de tiempo UTC del envío de eventos al registro de actividad|
|**subscriptionId**|Identificador de suscripción del recurso en peligro|
|**properties**|Una bolsa JSON de propiedades adicionales que pertenecen a la alerta. Pueden cambiar de una alerta a la otra; sin embargo, los siguientes campos aparecerán en todas ellas:<br>- severity: la gravedad del ataque<br>- compromisedEntity: el nombre del recurso en peligro<br>- remediationSteps: matriz de pasos de corrección que se deben realizar<br>- intent: La intención de la cadena de eliminación de la alerta. Las posibles intenciones se documentan en la [tabla de intenciones](alerts-reference.md#intentions)|
|**relatedEvents**|Constante: matriz vacía|
|||


### <a name="workflow-automation"></a>[Automatización de flujos de trabajo (versión preliminar)](#tab/schema-workflow-automation)

Para obtener el esquema de alertas cuando se usa la automatización del flujo de trabajo, consulte la [documentación de los conectores](/connectors/ascalert/).


### <a name="continuous-export"></a>[Exportación continua](#tab/schema-continuousexport)

La característica de exportación continua de Defender for Cloud pasa los datos de alerta a:

- Azure Event Hubs con el mismo esquema que la [API de alertas](/rest/api/securitycenter/alerts).
- Áreas de trabajo de Log Analytics según el [esquema SecurityAlert](/azure/azure-monitor/reference/tables/SecurityAlert) de la documentación de referencia de datos de Azure Monitor.




### <a name="ms-graph-api"></a>[MS Graph API](#tab/schema-graphapi)

Microsoft Graph es la puerta de enlace a los datos y la inteligencia en Microsoft 365. Proporciona un modelo de programación unificado que puede usar para acceder a la ingente cantidad de datos en Microsoft 365, Windows 10 y Enterprise Mobility + Security. Use la riqueza de datos de Microsoft Graph para crear aplicaciones para organizaciones y consumidores que interactúen con millones de usuarios.

El esquema y una representación JSON de las alertas de seguridad enviadas a MS Graph, están disponibles en [la documentación de Microsoft Graph](/graph/api/resources/alert).

---


## <a name="next-steps"></a>Pasos siguientes

En este artículo se han descrito los esquemas que usan las herramientas de protección contra amenazas de Microsoft Defender for Cloud al enviar información de alertas de seguridad.

Para más información sobre las distintas formas de acceder a las alertas de seguridad desde fuera de Defender for Cloud, consulte las siguientes páginas:

- [Microsoft Sentinel](../sentinel/index.yml): SIEM nativo de la nube de Microsoft
- [Azure Event Hubs](../event-hubs/index.yml): servicio de ingesta de datos en tiempo real y totalmente administrado de Microsoft
- [Exportación continua de datos de Defender for Cloud](continuous-export.md)
- [Áreas de trabajo de Log Analytics](../azure-monitor/logs/quick-create-workspace.md): Azure Monitor almacena los datos de registro en un área de trabajo de Log Analytics, un contenedor que incluye información de configuración y datos
