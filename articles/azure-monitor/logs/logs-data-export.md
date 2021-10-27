---
title: Exportación de datos del área de trabajo de Log Analytics en Azure Monitor (versión preliminar)
description: La exportación de datos de Log Analytics permite exportar continuamente los datos de las tablas seleccionadas del área de trabajo de Log Analytics en una cuenta de Azure Storage o Azure Event Hubs a medida que se recopilan.
ms.topic: conceptual
ms.custom: references_regions, devx-track-azurecli, devx-track-azurepowershell
author: yossi-y
ms.author: yossiy
ms.date: 10/17/2021
ms.openlocfilehash: 25d1d07edabdc8ee3d46175a51d8a20c5d9cc9eb
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2021
ms.locfileid: "130133180"
---
# <a name="log-analytics-workspace-data-export-in-azure-monitor-preview"></a>Exportación de datos del área de trabajo de Log Analytics en Azure Monitor (versión preliminar)
La exportación de datos del área de trabajo de Log Analytics en Azure Monitor permite exportar continuamente los datos de las tablas seleccionadas del área de trabajo de Log Analytics en una cuenta de Azure Storage o Azure Event Hubs a medida que se recopilan. En este artículo se ofrecen detalles sobre esta característica y pasos para configurar la exportación de datos en las áreas de trabajo.

## <a name="overview"></a>Introducción
Una vez configurada la exportación de datos del área de trabajo de Log Analytics, los nuevos datos enviados a las tablas seleccionadas del área de trabajo se exportan automáticamente a la cuenta de almacenamiento en blobs en anexos cada hora o al centro de eventos prácticamente en tiempo real.

![Información general sobre la exportación de datos](media/logs-data-export/data-export-overview.png)

Todos los datos de las tablas incluidas se exportan sin ningún filtro. Por ejemplo, al configurar una regla de exportación de datos para la tabla *SecurityEvent*, todos los datos enviados a la tabla *SecurityEvent* se exportan a partir de la hora de configuración.


## <a name="other-export-options"></a>Otras opciones de exportación
La exportación de datos del área de trabajo de Log Analytics permite exportar datos continuamente de un área de trabajo de Log Analytics. Otras opciones para exportar datos en determinados escenarios son las siguientes:

- Exportación programada desde una consulta de registro con una aplicación lógica. Es similar a la característica de exportación de datos, pero permite enviar datos filtrados o agregados a Azure Storage. Este método está sujeto a los [límites de las consultas de registro](../service-limits.md#log-analytics-workspaces); consulte [Archivado de datos de un área de trabajo de Log Analytics a Azure Storage mediante Logic Apps](logs-export-logic-app.md).
- Exportación única a la máquina local mediante el script de PowerShell. Consulte [Invoke-AzOperationalInsightsQueryExport](https://www.powershellgallery.com/packages/Invoke-AzOperationalInsightsQueryExport).

## <a name="limitations"></a>Limitaciones

- Actualmente, la configuración solo se puede realizar mediante solicitudes REST o la CLI. Todavía no se admiten Azure Portal o PowerShell.
- La opción `--export-all-tables` de la CLI y REST no se admite y se quitará. Debe proporcionar la lista de tablas en las reglas de exportación de manera explícita.
- Las tablas admitidas se limitan a las especificadas en la sección [Tablas admitidas](#supported-tables) que tiene más adelante. 
- Las tablas de registro personalizadas existentes no se admiten en la exportación. Se admite una nueva versión de registro personalizada disponible en marzo de 2022.
- Si la regla de exportación de datos incluye una tabla no admitida, la operación se realizará correctamente, pero no se exportará ningún dato de esa tabla hasta que se admita. 
- Si la regla de exportación de datos incluye una tabla que no existe, se producirá un error `Table <tableName> does not exist in the workspace`.
- Puede definir hasta 10 reglas habilitadas en el área de trabajo. Se permiten reglas adicionales cuando se deshabilitan. 
- El destino debe ser único en todas las reglas de exportación del área de trabajo.
- Los destinos deben estar en la misma región que el área de trabajo de Log Analytics.
- Los nombres de tablas no pueden tener más de 60 caracteres cuando se exporta a la cuenta de almacenamiento y 47 caracteres cuando se exporta al centro de eventos. Las tablas con nombres más largos no se exportarán.
- La exportación de datos estará disponible en todas las regiones, pero actualmente se admite en: 
    - Centro de Australia
    - Este de Australia
    - Sudeste de Australia
    - Sur de Brasil
    - Centro de Canadá
    - Centro de la India
    - Centro de EE. UU.
    - Este de Asia
    - Este de EE. UU.
    - Este de EE. UU. 2
    - Centro de Francia
    - Centro-oeste de Alemania
    - Japón Oriental
    - Centro de Corea del Sur
    - Centro-Norte de EE. UU
    - Norte de Europa
    - Norte de Sudáfrica
    - Centro-sur de EE. UU.
    - Sudeste de Asia
    - Norte de Suiza
    - Oeste de Suiza
    - Norte de Emiratos Árabes Unidos
    - Sur de Reino Unido
    - Oeste de Reino Unido
    - Centro-Oeste de EE. UU.
    - Oeste de Europa
    - Oeste de EE. UU.
    - Oeste de EE. UU. 2

## <a name="data-completeness"></a>Integridad de los datos
La exportación de datos continuará reintentando el envío de datos durante un máximo de 30 minutos, en el caso de que el destino no esté disponible. Si sigue sin estar disponible después de 30 minutos, los datos se descartarán hasta que el destino esté disponible.

## <a name="cost"></a>Coste
Actualmente no hay cargos adicionales por la característica de exportación de datos. Los precios de la exportación de datos se anunciarán en el futuro y habrá un periodo de aviso antes del inicio de la facturación. Si decide seguir usando la exportación de datos después del período de aviso, se le facturará según la tarifa aplicable.

## <a name="export-destinations"></a>Destinos de la exportación

El destino de exportación de datos debe crearse antes de crear la regla de exportación en el área de trabajo. El destino no tiene que estar en la misma suscripción que el área de trabajo. Al usar Azure Lighthouse, también es posible que los datos se envíen a un destino en otro inquilino de Azure Active Directory.

### <a name="storage-account"></a>Cuenta de almacenamiento

Debe tener permisos de "escritura" en el área de trabajo y el destino para poder configurar la regla de exportación de datos. Recuerde que no debe usar una cuenta de almacenamiento existente que tenga otros datos que no sean de supervisión almacenados en ella, para poder controlar mejor el acceso a esos datos y evitar alcanzar el límite de la tasa de ingestión de almacenamiento. 

Para enviar los datos al almacenamiento inmutable, establezca la directiva de inmutabilidad para la cuenta de almacenamiento, tal como se escribe en [Establecimiento y administración de directivas de inmutabilidad para el almacenamiento de blobs](../../storage/blobs/immutable-policy-configure-version-scope.md). Debe seguir todos los pasos que aparecen en este artículo, incluida la habilitación de las escrituras de blobs en anexos protegidos.

La cuenta de almacenamiento debe tener la versión StorageV1 u otra superior y estar en la misma región que el área de trabajo. Si tiene que replicar los datos en otras cuentas de almacenamiento de otras regiones, puede usar cualquiera de las [opciones de redundancia de Azure Storage](../../storage/common/storage-redundancy.md#redundancy-in-a-secondary-region), incluidos GRS y GZRS.

Los datos se envían a las cuentas de almacenamiento cuando llegan a Azure Monitor y se almacenan en blobs en anexos cada hora. Se crea un contenedor para cada tabla en la cuenta de almacenamiento denominado *am-* seguido del nombre de la tabla. Por ejemplo, la tabla *SecurityEvent* se enviaría a un contenedor denominado *am-SecurityEvent*.

> [!NOTE]
> Se recomienda usar una cuenta de almacenamiento independiente para asignar la velocidad de entrada de forma adecuada y reducir la limitación, los errores y los eventos de latencia.

A partir del 15 de octubre de 2021, los blobs se almacenan en carpetas de 5 minutos en la siguiente estructura de ruta de acceso: *WorkspaceResourceId=/subscriptions/subscription-id/resourcegroups/\<resource-group\>/providers/microsoft.operationalinsights/workspaces/\<workspace\>/y=\<four-digit numeric year\>/m=\<two-digit numeric month\>/d=\<two-digit numeric day\>/h=\<two-digit 24-hour clock hour\>/m=\<two-digit 60-minute clock minute\>/PT05M.json*. Dado que los blobs en anexos están limitados a 50 000 escrituras en el almacenamiento, el número de blobs exportados puede ampliarse si el número de anexos es elevado. El patrón de nomenclatura de los blobs es PT05M_#.json en este caso, donde # corresponde al incremento del número de blobs.

El formato de los datos de la cuenta de almacenamiento está en [líneas JSON](../essentials/resource-logs-blob-format.md). Esto significa que cada registro se delimita mediante una nueva línea, sin matrices de registros exteriores y sin comas entre los registros JSON. 

[![Datos de ejemplo de almacenamiento](media/logs-data-export/storage-data.png)](media/logs-data-export/storage-data.png#lightbox)

### <a name="event-hub"></a>Centro de eventos

Debe tener permisos de "escritura" en el área de trabajo y el destino para poder configurar la regla de exportación de datos. La directiva de acceso compartido del espacio de nombres del centro de eventos, define los permisos que tiene el mecanismo de transmisión. Para realizar una transmisión a un centro de eventos, necesita tener permisos de administración, envío y escucha. Para actualizar la regla de exportación, debe tener el permiso ListKey en esa regla de autorización de Event Hubs.

El espacio de nombres del centro de eventos debe estar en la misma región que su área de trabajo.

Los datos se envían al centro de eventos a medida que llegan a Azure Monitor. Se crea un centro de eventos para cada tipo de datos que se exporta con el nombre *am-* seguido del nombre de la tabla. Por ejemplo, la tabla *SecurityEvent* se enviaría a un centro de eventos denominado *am-SecurityEvent*. Si quiere que los datos exportados lleguen a un centro de eventos específico, o si tiene una tabla con un nombre que supere el límite de 47 caracteres, puede proporcionar el nombre de su propio centro de eventos y exportar todos los datos para las tablas definidas en él.

> [!NOTE]
> - El nivel del centro de eventos "básico" admite un [límite](../../event-hubs/event-hubs-quotas.md#basic-vs-standard-vs-premium-vs-dedicated-tiers) de tamaño de evento inferior; tenga en cuenta que algunos registros del área de trabajo pueden superarlo y anularse. Use los niveles "Estándar", "Premium" o "Dedicado" para el destino de exportación.
> - El volumen de datos exportados aumenta con el tiempo y, en consecuencia, se necesita el escalado para alcanzar mayores velocidades de entrada. Use la característica de **inflado automático** para escalar verticalmente y aumentar el número de unidades de procesamiento de forma automática y, de este modo, satisfacer las necesidades de uso. Consulte [Escalado vertical y automático de las unidades de procesamiento de Azure Event Hubs](../../event-hubs/event-hubs-auto-inflate.md).
> - Use un espacio de nombres de centro de eventos independiente para asignar la velocidad de entrada de forma adecuada y reducir la limitación, los errores y los eventos de latencia.
> - La exportación de datos no puede acceder a los recursos del centro de eventos cuando las redes virtuales están habilitadas. Debe habilitar la opción **Habilitación de los servicios de Microsoft de confianza** a fin de omitir esta configuración del firewall en el centro de eventos para conceder acceso a los recursos de Event Hubs.

> [!IMPORTANT]
> El [número de centros de eventos admitidos por los niveles de espacios de nombres "Básico" y "Estándar"](../../event-hubs/event-hubs-quotas.md#common-limits-for-all-tiers) es 10. Si exporta más de 10 tablas, divida las tablas entre varias reglas de exportación en distintos espacios de nombres del centro de eventos o proporcione el nombre del centro de eventos en la regla de exportación y exporte todas las tablas a ese centro de eventos.

## <a name="enable-data-export"></a>Habilitación de la exportación de datos
Para habilitar la exportación de datos de Log Analytics, hay que seguir los pasos que se detallan a continuación. Consulte las siguientes secciones para obtener más información sobre cada uno de ellos.

- Registre el proveedor de recursos.
- Habilite los servicios de Microsoft de confianza.
- Cree una o varias reglas de exportación de datos que definan las tablas que se van a exportar y su destino.

### <a name="register-resource-provider"></a>Registro del proveedor de recursos
El siguiente proveedor de recursos de Azure debe registrarse en su suscripción para habilitar la exportación de datos de Log Analytics. 

- Microsoft.Insights

Probablemente, este proveedor de recursos ya esté registrado para la mayoría de los usuarios Azure Monitor. Para comprobarlo, vaya a **Suscripciones** en Azure Portal. Seleccione su suscripción y, luego, haga clic en **Proveedores de recursos**, en la sección **Configuración** del menú. Busque **Microsoft.Insights**. Si su estado es **Registrado**, entonces ya está registrado. Si no es así, haga clic en **Registrar** para registrarlo.

También puede usar cualquiera de los métodos disponibles para registrar un proveedor de recursos, tal y como se describe en [Tipos y proveedores de recursos de Azure](../../azure-resource-manager/management/resource-providers-and-types.md). El siguiente es un comando de ejemplo con PowerShell:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.insights
```

### <a name="allow-trusted-microsoft-services"></a>Habilitación de los servicios de Microsoft de confianza
Si ha configurado la cuenta de almacenamiento para permitir el acceso desde las redes seleccionadas, tiene que agregar una excepción para permitir que Azure Monitor escriba en la cuenta. En la opción **Firewalls y redes virtuales** de la cuenta de almacenamiento, seleccione **Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento**.

[![Firewalls y redes virtuales de la cuenta de almacenamiento](media/logs-data-export/storage-account-vnet.png)](media/logs-data-export/storage-account-vnet.png#lightbox)

### <a name="create-or-update-data-export-rule"></a>Creación o actualización de una regla de exportación de datos
La regla de exportación de datos define las tablas cuyos datos se exportarán y el destino. Puede tener 10 reglas habilitadas en el área de trabajo. Se pueden agregar más reglas, pero con el estado "deshabilitado". Los destinos deben ser únicos en todas las reglas de exportación del área de trabajo.

Los destinos de exportación de datos tienen límites y se deben supervisar para minimizar la limitación, los errores y la latencia de la exportación. Consulte los temas sobre [escalabilidad de las cuentas de almacenamiento](../../storage/common/scalability-targets-standard-account.md#scale-targets-for-standard-storage-accounts) y [cuotas de espacio de nombres del centro de eventos](../../event-hubs/event-hubs-quotas.md).

#### <a name="recommendations-for-storage-account"></a>Recomendaciones para la cuenta de almacenamiento 

1. Use una cuenta de almacenamiento independiente para la exportación.
1. Configure la alerta en la métrica siguiente con la siguiente configuración: 

    | Ámbito | Espacio de nombres de métrica | Métrica | Agregación | Umbral |
    |:---|:---|:---|:---|:---|
    | storage-name | Cuenta | Entrada | Sum | 80 % de la tasa máxima de entrada de almacenamiento. Por ejemplo: 60 Gbps para la versión 2 de uso general en la región Oeste de EE. UU. |
  
1. Acciones de corrección de alertas
    - Use una cuenta de almacenamiento independiente para la exportación.
    - Las cuentas estándar de Azure Storage admiten límites de entrada más altos por solicitud. Para solicitar un aumento, póngase en contacto con [Soporte técnico de Azure](https://azure.microsoft.com/support/faq/).
    - Dividir tablas entre cuentas de almacenamiento adicionales.

#### <a name="recommendations-for-event-hub"></a>Recomendaciones para el centro de eventos

1. Configuración de [alertas de métricas](../../event-hubs/monitor-event-hubs-reference.md):
  
    | Ámbito | Espacio de nombres de métrica | Métrica | Agregación | Umbral |
    |:---|:---|:---|:---|:---|
    | namespaces-name | Métricas estándar del centro de eventos | Bytes de entrada | Sum | 80 % de entrada máxima por 5 minutos. Por ejemplo, es 1 MB/s por unidad (TU o PU) |
    | namespaces-name | Métricas estándar del centro de eventos | Solicitudes entrantes | Count | 80 % de eventos como máximo por 5 minutos. Por ejemplo, es 1000/s por unidad (TU o PU) |
    | namespaces-name | Métricas estándar del centro de eventos | Cuota de errores superada | Count | Entre el 1 % y el 5 % de la solicitud |

1. Acciones de corrección de alertas
   - Aumentar el número de unidades (TU o PU)
   - Dividir tablas entre espacios de nombres adicionales
   - Usar los niveles "Premium" o "Dedicado" para obtener un mayor rendimiento

La regla de exportación debe incluir las tablas que tenga en el área de trabajo. Ejecute esta consulta para obtener una lista de tablas disponibles en el área de trabajo.

```kusto
find where TimeGenerated > ago(24h) | distinct Type
```

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

En el menú **Área de trabajo de Log Analytics** de Azure Portal, seleccione **Exportación de datos** en la sección **Configuración** y haga clic en **Nueva regla de exportación** en la parte superior del panel central.

![creación de exportación](media/logs-data-export/export-create-1.png)

Siga los pasos y, a continuación, haga clic en **Crear**. 

<img src="media/logs-data-export/export-create-2.png" alt="export rule configuration" title="configuración de reglas de exportación" width="80%"/>


# <a name="powershell"></a>[PowerShell](#tab/powershell)

N/D

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Use el siguiente comando para crear una regla de exportación de datos en una cuenta de almacenamiento mediante la CLI.

```azurecli
$storageAccountResourceId = '/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name'
az monitor log-analytics workspace data-export create --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --tables SecurityEvent Heartbeat --destination $storageAccountResourceId
```

Use el siguiente comando para crear una regla de exportación de datos en un centro de eventos mediante la CLI. Se crea un centro de eventos independiente para cada tabla.

```azurecli
$eventHubsNamespacesResourceId = '/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.EventHub/namespaces/namespaces-name'
az monitor log-analytics workspace data-export create --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --tables SecurityEvent Heartbeat --destination $eventHubsNamespacesResourceId
```

Use el siguiente comando para crear una regla de exportación de datos en un centro de eventos específico mediante la CLI. Todas las tablas se exportan al nombre del centro de eventos proporcionado. 

```azurecli
$eventHubResourceId = '/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.EventHub/namespaces/namespaces-name/eventhubs/eventhub-name'
az monitor log-analytics workspace data-export create --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --tables SecurityEvent Heartbeat --destination $eventHubResourceId
```

# <a name="rest"></a>[REST](#tab/rest)

Use la siguiente solicitud para crear una regla de exportación de datos mediante la API REST. La solicitud debe usar la autorización del token de portador y el tipo de contenido aplicación/json.

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
```

El cuerpo de la solicitud especifica el destino de las tablas. El siguiente es un cuerpo de ejemplo para la solicitud REST para una cuenta de almacenamiento.

```json
{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name"
        },
        "tablenames": [
            "table1",
            "table2" 
        ],
        "enable": true
    }
}
```

El siguiente es un cuerpo de ejemplo para la solicitud REST para un centro de eventos.

```json
{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.EventHub/namespaces/eventhub-namespaces-name"
        },
        "tablenames": [
            "table1",
            "table2"
        ],
        "enable": true
    }
}
```

El siguiente es un cuerpo de ejemplo para la solicitud REST para un centro de eventos, donde se proporciona el nombre del centro de eventos. En este caso, todos los datos exportados se envían a este centro de eventos.

```json
{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.EventHub/namespaces/eventhub-namespaces-name",
            "metaData": {
                "EventHubName": "eventhub-name"
        },
        "tablenames": [
            "table1",
            "table2"
        ],
        "enable": true
    }
  }
}
```

# <a name="template"></a>[Plantilla](#tab/json)

Use el siguiente comando para crear una regla de exportación de datos en una cuenta de almacenamiento mediante una plantilla.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "defaultValue": "workspace-name",
            "type": "String"
        },
        "workspaceLocation": {
            "defaultValue": "workspace-region",
            "type": "string"
        },
        "storageAccountRuleName": {
            "defaultValue": "storage-account-rule-name",
            "type": "string"
        },
        "storageAccountResourceId": {
            "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "microsoft.operationalinsights/workspaces",
            "apiVersion": "2020-08-01",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceLocation')]",
            "resources": [
                {
                  "type": "microsoft.operationalinsights/workspaces/dataexports",
                  "apiVersion": "2020-08-01",
                  "name": "[concat(parameters('workspaceName'), '/' , parameters('storageAccountRuleName'))]",
                  "dependsOn": [
                      "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]"
                  ],
                  "properties": {
                      "destination": {
                          "resourceId": "[parameters('storageAccountResourceId')]"
                      },
                      "tableNames": [
                          "Heartbeat",
                          "InsightsMetrics",
                          "VMConnection",
                          "Usage"
                      ],
                      "enable": true
                  }
              }
            ]
        }
    ]
}
```

Use el siguiente comando para crear una regla de exportación de datos en un centro de eventos mediante una plantilla. Se crea un centro de eventos independiente para cada tabla.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "defaultValue": "workspace-name",
            "type": "String"
        },
        "workspaceLocation": {
            "defaultValue": "workspace-region",
            "type": "string"
        },
        "eventhubRuleName": {
            "defaultValue": "event-hub-rule-name",
            "type": "string"
        },
        "namespacesResourceId": {
            "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/microsoft.eventhub/namespaces/namespaces-name",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "microsoft.operationalinsights/workspaces",
            "apiVersion": "2020-08-01",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceLocation')]",
            "resources": [
              {
                  "type": "microsoft.operationalinsights/workspaces/dataexports",
                  "apiVersion": "2020-08-01",
                  "name": "[concat(parameters('workspaceName'), '/', parameters('eventhubRuleName'))]",
                  "dependsOn": [
                      "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]"
                  ],
                  "properties": {
                      "destination": {
                          "resourceId": "[parameters('namespacesResourceId')]"
                      },
                      "tableNames": [
                          "Usage",
                          "Heartbeat"
                      ],
                      "enable": true
                  }
              }
            ]
        }
    ]
}
```

Use el siguiente comando para crear una regla de exportación de datos en un centro de eventos específico mediante una plantilla. Todas las tablas se exportan al nombre del centro de eventos proporcionado.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "defaultValue": "workspace-name",
            "type": "String"
        },
        "workspaceLocation": {
            "defaultValue": "workspace-region",
            "type": "string"
        },
        "eventhubRuleName": {
            "defaultValue": "event-hub-rule-name",
            "type": "string"
        },
        "namespacesResourceId": {
            "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group-name/providers/microsoft.eventhub/namespaces/namespaces-name",
            "type": "String"
        },
        "eventhubName": {
            "defaultValue": "event-hub-name",
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "microsoft.operationalinsights/workspaces",
            "apiVersion": "2020-08-01",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceLocation')]",
            "resources": [
              {
                  "type": "microsoft.operationalinsights/workspaces/dataexports",
                  "apiVersion": "2020-08-01",
                  "name": "[concat(parameters('workspaceName'), '/', parameters('eventhubRuleName'))]",
                  "dependsOn": [
                      "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]"
                  ],
                  "properties": {
                      "destination": {
                          "resourceId": "[parameters('namespacesResourceId')]",
                          "metaData": {
                              "eventHubName": "[parameters('eventhubName')]"
                          }
                      },
                      "tableNames": [
                          "Usage",
                          "Heartbeat"
                      ],
                      "enable": true
                  }
              }
            ]
        }
    ]
}
```

---

## <a name="view-data-export-rule-configuration"></a>Consulta de la configuración de la regla de exportación de datos

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

En el menú **Área de trabajo de Log Analytics** de Azure Portal, seleccione **Exportación de datos** en la sección **Configuración**.

![vista de las reglas de exportación](media/logs-data-export/export-view-1.png)

Haga clic en una regla para acceder a la vista de configuración.

<img src="media/logs-data-export/export-view-2.png" alt="export rule settings" title= "configuración de las reglas de exportación" width="65%"/>


# <a name="powershell"></a>[PowerShell](#tab/powershell)

N/D

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Use el siguiente comando para ver la configuración de una regla de exportación de datos mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export show --resource-group resourceGroupName --workspace-name workspaceName --name ruleName
```

# <a name="rest"></a>[REST](#tab/rest)

Use la siguiente solicitud para ver la configuración de una regla de exportación de datos mediante la API REST. La solicitud debe utilizar la autorización del token de portador.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
```

# <a name="template"></a>[Plantilla](#tab/json)

N/D

---

## <a name="disable-an-export-rule"></a>Deshabilitación de una regla de exportación

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Las reglas de exportación se pueden deshabilitar para permitir que se detenga la exportación cuando no sea necesario conservar los datos durante un período determinado, como al realizar pruebas. En el menú **Área de trabajo de Log Analytics** de Azure Portal, seleccione **Exportación de datos** en la sección **Configuración** y haga clic en el botón de alternancia de estado para habilitar o deshabilitar la regla de exportación.

![deshabilitación de la regla de exportación](media/logs-data-export/export-disable.png)


# <a name="powershell"></a>[PowerShell](#tab/powershell)

N/D

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Las reglas de exportación se pueden deshabilitar para permitir que se detenga la exportación cuando no sea necesario conservar los datos durante un período determinado, como al realizar pruebas. Use el siguiente comando para deshabilitar una regla de exportación de datos mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export update --resource-group resourceGroupName --workspace-name workspaceName --name ruleName --tables SecurityEvent Heartbeat --enable false
```

# <a name="rest"></a>[REST](#tab/rest)

Las reglas de exportación se pueden deshabilitar para permitir que se detenga la exportación cuando no sea necesario conservar los datos durante un período determinado, como al realizar pruebas. Use la siguiente solicitud para deshabilitar una regla de exportación de datos mediante la API REST. La solicitud debe utilizar la autorización del token de portador.

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
Authorization: Bearer <token>
Content-type: application/json

{
    "properties": {
        "destination": {
            "resourceId": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/Microsoft.Storage/storageAccounts/storage-account-name"
        },
        "tablenames": [
"table1",
    "table2" 
        ],
        "enable": false
    }
}
```

# <a name="template"></a>[Plantilla](#tab/json)

Las reglas de exportación se pueden deshabilitar para permitir que se detenga la exportación cuando no sea necesario conservar los datos durante un período determinado, como al realizar pruebas. Establezca ```"enable": false``` en la plantilla para deshabilitar una exportación de datos.

---

## <a name="delete-an-export-rule"></a>Eliminación de una regla de exportación

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

En el menú **Área de trabajo de Log Analytics** de Azure Portal, seleccione *Exportación de datos* en la sección **Configuración** y, a continuación, haga clic en los puntos suspensivos de la derecha de la regla y seleccione **Eliminar**. 

![eliminación de una regla de exportación](media/logs-data-export/export-delete.png)


# <a name="powershell"></a>[PowerShell](#tab/powershell)

N/D

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Use el siguiente comando para eliminar una regla de exportación de datos mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export delete --resource-group resourceGroupName --workspace-name workspaceName --name ruleName
```

# <a name="rest"></a>[REST](#tab/rest)

Use la siguiente solicitud para eliminar una regla de exportación de datos mediante la API REST. La solicitud debe utilizar la autorización del token de portador.

```rest
DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports/<data-export-name>?api-version=2020-08-01
```

# <a name="template"></a>[Plantilla](#tab/json)

N/D

---


## <a name="view-all-data-export-rules-in-a-workspace"></a>Consulta de todas las reglas de exportación de datos en un área de trabajo

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

En el menú **Área de trabajo de Log Analytics** de Azure Portal, seleccione **Exportación de datos** en la sección **Configuración** para ver todas las reglas de exportación del área de trabajo.

![reglas de exportación](media/logs-data-export/export-view.png)


# <a name="powershell"></a>[PowerShell](#tab/powershell)

N/D

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Use el siguiente comando para ver todas las reglas de exportación de datos en un área de trabajo mediante la CLI.

```azurecli
az monitor log-analytics workspace data-export list --resource-group resourceGroupName --workspace-name workspaceName
```

# <a name="rest"></a>[REST](#tab/rest)

Use la siguiente solicitud para ver todas las reglas de exportación de datos en un área de trabajo mediante la API REST. La solicitud debe utilizar la autorización del token de portador.

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.operationalInsights/workspaces/<workspace-name>/dataexports?api-version=2020-08-01
```

# <a name="template"></a>[Plantilla](#tab/json)

N/D

---


## <a name="unsupported-tables"></a>Tablas no admitidas
Si la regla de exportación de datos incluye una tabla no admitida, la configuración se realizará correctamente, pero no se exportará ningún dato de esa tabla. Si la tabla se admite posteriormente, sus datos se exportarán en ese momento.

Si la regla de exportación de datos incluye una tabla que no existe, se producirá un error que indicará que la "tabla \<tableName\> no existe en el área de trabajo".


## <a name="supported-tables"></a>Tablas admitidas
Las tablas admitidas se limitan actualmente a las que se especifican a continuación. Todos los datos de la tabla se exportarán a menos que se especifiquen limitaciones. Esta lista se actualiza a medida que se agrega compatibilidad con tablas adicionales.

| Tabla | Limitaciones |
|:---|:---|
| AACAudit |  |
| AACHttpRequest |  |
| AADDomainServicesAccountLogon |  |
| AADDomainServicesAccountManagement |  |
| AADDomainServicesDirectoryServiceAccess |  |
| AADDomainServicesLogonLogoff |  |
| AADDomainServicesPolicyChange |  |
| AADDomainServicesPrivilegeUse |  |
| AADManagedIdentitySignInLogs |  |
| AADNonInteractiveUserSignInLogs |  |
| AADProvisioningLogs |  |
| AADRiskyUsers |  |
| AADServicePrincipalSignInLogs |  |
| AADUserRiskEvents |  |
| ABSBotRequests |  |
| ACSAuthIncomingOperations |  |
| ACSBillingUsage |  |
| ACSChatIncomingOperations |  |
| ACSSMSIncomingOperations |  |
| ADAssessmentRecommendation |  |
| ADFActivityRun |  |
| ADFPipelineRun |  |
| ADFSSignInLogs |  |
| ADFTriggerRun |  |
| ADPAudit |  |
| ADPDiagnostics |  |
| ADPRequests |  |
| ADReplicationResult |  |
| ADSecurityAssessmentRecommendation |  |
| ADTDigitalTwinsOperation |  |
| ADTEventRoutesOperation |  |
| ADTModelsOperation |  |
| ADTQueryOperation |  |
| ADXCommand |  |
| ADXQuery |  |
| AegDeliveryFailureLogs |  |
| AegPublishFailureLogs |  |
| AEWAuditLogs |  |
| Alerta |  |
| AmlOnlineEndpointConsoleLog |  |
| ApiManagementGatewayLogs |  |
| AppCenterError |  |
| AppPlatformSystemLogs |  |
| AppServiceAppLogs |  |
| AppServiceAuditLogs |  |
| AppServiceConsoleLogs |  |
| AppServiceFileAuditLogs |  |
| AppServiceHTTPLogs |  |
| AppServicePlatformLogs |  |
| AuditLogs |  |
| AutoscaleEvaluationsLog |  |
| AutoscaleScaleActionsLog |  |
| AWSCloudTrail |  |
| AWSGuardDuty |  |
| AWSVPCFlow |  |
| AzureAssessmentRecommendation |  |
| AzureDevOpsAuditing |  |
| BehaviorAnalytics |  |
| BlockchainApplicationLog |  |
| BlockchainProxyLog |  |
| CDBCassandraRequests |  |
| CDBControlPlaneRequests |  |
| CDBDataPlaneRequests |  |
| CDBGremlinRequests |  |
| CDBMongoRequests |  |
| CDBPartitionKeyRUConsumption |  |
| CDBPartitionKeyStatistics |  |
| CDBQueryRuntimeStatistics |  |
| CommonSecurityLog |  |
| ComputerGroup |  |
| ConfigurationData | Compatibilidad parcial: algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Esta parte no está disponible en la exportación actualmente. |
| ContainerImageInventory |  |
| ContainerInventory |  |
| ContainerLog |  |
| ContainerLogV2 |  |
| ContainerNodeInventory |  |
| ContainerServiceLog |  |
| CoreAzureBackup |  |
| DatabricksAccounts |  |
| DatabricksClusters |  |
| DatabricksDBFS |  |
| DatabricksInstancePools |  |
| DatabricksJobs |  |
| DatabricksNotebook |  |
| DatabricksSecrets |  |
| DatabricksSQLPermissions |  |
| DatabricksSSH |  |
| DatabricksWorkspace |  |
| DeviceNetworkInfo |  |
| DnsEvents |  |
| DnsInventory |  |
| DummyHydrationFact |  |
| Dynamics365Activity |  |
| EmailAttachmentInfo |  |
| EmailEvents |  |
| EmailPostDeliveryEvents |  |
| EmailUrlInfo |  |
| Evento | Compatibilidad parcial: los datos que llegan del agente de Log Analytics (MMA) o el agente de Azure Monitor (AMA) son totalmente compatibles con la exportación. Los datos que llegan a través del agente de Diagnostics Extension se recopilan a través del almacenamiento, mientras que esta ruta de acceso no se admite en la exportación.2 |
| ExchangeAssessmentRecommendation |  |
| FailedIngestion |  |
| FunctionAppLogs |  |
| HDInsightAmbariClusterAlerts |  |
| HDInsightAmbariSystemMetrics |  |
| HDInsightHadoopAndYarnLogs |  |
| HDInsightHadoopAndYarnMetrics |  |
| HDInsightHBaseLogs |  |
| HDInsightHBaseMetrics |  |
| HDInsightHiveAndLLAPLogs |  |
| HDInsightHiveAndLLAPMetrics |  |
| HDInsightHiveTezAppStats |  |
| HDInsightJupyterNotebookEvents |  |
| HDInsightKafkaLogs |  |
| HDInsightKafkaMetrics |  |
| HDInsightOozieLogs |  |
| HDInsightSecurityLogs |  |
| HDInsightSparkApplicationEvents |  |
| HDInsightSparkBlockManagerEvents |  |
| HDInsightSparkEnvironmentEvents |  |
| HDInsightSparkExecutorEvents |  |
| HDInsightSparkJobEvents |  |
| HDInsightSparkLogs |  |
| HDInsightSparkSQLExecutionEvents |  |
| HDInsightSparkStageEvents |  |
| HDInsightSparkStageTaskAccumulables |  |
| HDInsightSparkTaskEvents |  |
| Latido |  |
| HuntingBookmark |  |
| InsightsMetrics | Compatibilidad parcial: algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Esta parte no está disponible en la exportación actualmente. |
| IntuneAuditLogs |  |
| IntuneDevices |  |
| IntuneOperationalLogs |  |
| KubeEvents |  |
| KubeHealth |  |
| KubeMonAgentEvents |  |
| KubeNodeInventory |  |
| KubePodInventory |  |
| KubeServices |  |
| LAQueryLogs |  |
| McasShadowItReporting |  |
| MCCEventLogs |  |
| MicrosoftAzureBastionAuditLogs |  |
| MicrosoftDataShareReceivedSnapshotLog |  |
| MicrosoftDataShareSentSnapshotLog |  |
| MicrosoftDataShareShareLog |  |
| MicrosoftHealthcareApisAuditLogs |  |
| NWConnectionMonitorPathResult |  |
| NWConnectionMonitorTestResult |  |
| OfficeActivity | Compatibilidad parcial en nubes de Administración Pública: algunos de los datos se ingieren mediante webhooks de O365 en LA. Esta parte no está disponible en la exportación actualmente. |
| Operación | Compatibilidad parcial: algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Esta parte no está disponible en la exportación actualmente. |
| Perf | Compatibilidad parcial: actualmente solo se admiten los datos de rendimiento de Windows. En este momento, faltan los datos de rendimiento de Linux en la exportación. |
| PowerBIDatasetsWorkspace |  |
| PurviewScanStatusLogs |  |
| SCCMAssessmentRecommendation |  |
| SCOMAssessmentRecommendation |  |
| SecurityAlert |  |
| SecurityBaseline |  |
| SecurityBaselineSummary |  |
| SecurityCef |  |
| SecurityDetection |  |
| SecurityEvent | Compatibilidad parcial: los datos que llegan del agente de Log Analytics (MMA) o el agente de Azure Monitor (AMA) son totalmente compatibles con la exportación. Los datos que llegan a través del agente de Diagnostics Extension se recopilan a través del almacenamiento, mientras que esta ruta de acceso no se admite en la exportación.2 |
| SecurityIncident |  |
| SecurityIoTRawEvent |  |
| SecurityNestedRecommendation |  |
| SecurityRecommendation |  |
| SentinelHealth |  |
| SfBAssessmentRecommendation |  |
| SfBOnlineAssessmentRecommendation |  |
| SharePointOnlineAssessmentRecommendation |  |
| SignalRServiceDiagnosticLogs |  |
| SigninLogs |  |
| SPAssessmentRecommendation |  |
| SQLAssessmentRecommendation |  |
| SQLSecurityAuditEvents |  |
| SucceededIngestion |  |
| SynapseBigDataPoolApplicationsEnded |  |
| SynapseBuiltinSqlPoolRequestsEnded |  |
| SynapseGatewayApiRequests |  |
| SynapseIntegrationActivityRuns |  |
| SynapseIntegrationPipelineRuns |  |
| SynapseIntegrationTriggerRuns |  |
| SynapseRbacOperations |  |
| SynapseSqlPoolDmsWorkers |  |
| SynapseSqlPoolExecRequests |  |
| SynapseSqlPoolRequestSteps |  |
| SynapseSqlPoolSqlRequests |  |
| SynapseSqlPoolWaits |  |
| syslog | Compatibilidad parcial: los datos que llegan del agente de Log Analytics (MMA) o el agente de Azure Monitor (AMA) son totalmente compatibles con la exportación. Los datos que llegan a través del agente de Diagnostics Extension se recopilan a través del almacenamiento, mientras que esta ruta de acceso no se admite en la exportación.2 |
| ThreatIntelligenceIndicator |  |
| Actualizar | Compatibilidad parcial: algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Esta parte no está disponible en la exportación actualmente. |
| UpdateRunProgress |  |
| UpdateSummary |  |
| Uso |  |
| UserAccessAnalytics |  |
| UserPeerAnalytics |  |
| Watchlist |  |
| WindowsEvent |  |
| WindowsFirewall |  |
| WireData | Compatibilidad parcial: algunos de los datos se ingieren por medio de servicios internos que no se admiten para la exportación. Esta parte no está disponible en la exportación actualmente. |
| WorkloadDiagnosticLogs |  |
| WVDAgentHealthStatus |  |
| WVDCheckpoints |  |
| WVDConnections |  |
| WVDErrors |  |
| WVDFeeds |  |
| WVDManagement |  |

## <a name="next-steps"></a>Pasos siguientes

- [Consulta de datos exportados desde Azure Monitor mediante Azure Data Explorer (versión preliminar)](../logs/azure-data-explorer-query-storage.md)
