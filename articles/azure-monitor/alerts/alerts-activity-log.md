---
title: Crear, ver y administrar las alertas del registro de actividad en Azure Monitor
description: Crear alertas del registro de actividad mediante Azure Portal, una plantilla de Azure Resource Manager y Azure PowerShell.
ms.topic: conceptual
ms.subservice: alerts
ms.date: 08/12/2021
ms.openlocfilehash: 5cd113692cfe0c024980cb95252acf9f7763a2eb
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129360131"
---
# <a name="create-view-and-manage-activity-log-alerts-by-using-azure-monitor"></a>Crear, ver y administrar las alertas del registro de actividad mediante Azure Monitor  

Las *alertas del registro de actividad* son alertas que se activan cuando un nuevo evento del registro de actividad cumple las condiciones especificadas en la alerta. Estas alertas se crean para recursos de Azure con una plantilla de Azure Resource Manager. También se pueden crear, actualizar o eliminar estas alertas en Azure Portal.

Normalmente es necesario crear alertas del registro de actividad para recibir una notificación si se producen cambios específicos en los recursos de la suscripción de Azure. Las alertas a menudo se limitan a grupos de recursos o recursos determinados. Por ejemplo, es posible que quiera recibir una notificación cuando se elimine cualquier máquina virtual del grupo de recursos de ejemplo `myProductionResourceGroup`. O bien, podría querer recibir una notificación si se asigna algún rol nuevo a un usuario de la suscripción.

> [!IMPORTANT]
> No se pueden crear alertas sobre las notificaciones de estado del servicio mediante la interfaz para crear alertas del registro de actividad. Para obtener más información acerca de cómo crear y usar las notificaciones de Service Health, consulte [Receive activity log alerts on service health notifications](../../service-health/alerts-activity-log-service-notifications-portal.md) (Recibir alertas del registro de actividad con las notificaciones de Service Health).

Al crear reglas de alertas, asegúrese de que:

- La suscripción del ámbito no es diferente de la suscripción donde se creó la alerta.
- La alerta se configurará según los siguientes tipos de criterios: nivel, estado, autor de la llamada, grupo de recursos, id. del recurso o categoría de eventos del tipo de recurso.
- No hay condición `anyOf` o condiciones anidadas en la configuración de alertas JSON. Solo se permite una condición `allOf`, sin más condiciones `allOf` o `anyOf`.
- Cuando la categoría es `administrative`, debe especificar al menos uno de los criterios anteriores en la alerta. No se puede crear una alerta que se active cada vez que se crea un evento en los registros de actividad.
- No se pueden crear alertas para eventos en la categoría `alert` del registro de actividad.

## <a name="azure-portal"></a>Azure Portal

Puede usar Azure Portal para crear y modificar las reglas de alertas del registro de actividad. Asimismo, se integra la experiencia con el registro de actividad de Azure para garantizar la creación de alertas sin problemas para eventos específicos de interés. En Azure Portal puede crear una nueva regla de alerta de registro de actividad, ya sea desde el panel de alertas de Azure Monitor o desde el panel de registro de actividad de Azure Monitor. 

### <a name="create-an-alert-rule-from-the-azure-monitor-alerts-pane"></a>Creación de una regla de alertas desde el panel de alertas de Azure Monitor

Aquí se muestra cómo crear una regla de alertas del registro de actividad en Azure Portal:

1. En [Azure Portal](https://portal.azure.com), seleccione **Supervisión**. El panel Monitor consolida todas las opciones de configuración y todos los datos de supervisión en una vista.

2. Seleccione **Alertas** >  **+Nueva regla de alertas**.

    :::image type="content" source="media/alerts-activity-log/create-alert-rule-button-new.png" alt-text="Captura de pantalla que muestra el botón de nueva regla de alertas.":::
    > [!TIP]
    > La mayoría de los paneles de recursos también tienen **Alertas** en su menú de recursos, bajo **Supervisión**. También puede crear alertas desde allí.

3. Elija **Seleccionar destino** y, a continuación, seleccione un recurso de destino sobre el que quiera alertar. Use las listas **Suscripción** y **Tipo de recurso** para buscar el recurso que quiere supervisar. También puede utilizar la barra de búsqueda para buscar su recurso.
    
    > [!NOTE]
    > Como destino, puede seleccionar una suscripción completa, un grupo de recursos o un recurso específico. Si eligió una suscripción o un grupo de recursos como destino y también selecciona un tipo de recurso, la regla se aplicará a todos los recursos de ese tipo dentro de la suscripción seleccionada o un grupo de recursos. Si elige un recurso de destino específico, la regla solo se aplicará a ese recurso. No se pueden seleccionar explícitamente varias suscripciones, grupos de recursos o recursos mediante el selector de destino. 

4. Si el recurso seleccionado tiene operaciones de registro de actividad sobre las que se pueden crear alertas, verá que en **Tipos de señal disponibles** aparece **Registro de actividad**. Puede ver la lista completa de tipos de recursos compatibles para las alertas de registro de actividad en [Operaciones del proveedor de recursos de Azure](../../role-based-access-control/resource-provider-operations.md).

    :::image type="content" source="media/alerts-activity-log/select-target-new.png" alt-text="Captura de pantalla del panel de selección de destino." lightbox="media/alerts-activity-log/select-target-new.png":::

5. Una vez haya seleccionado un recurso de destino, seleccione **Agregar condición**.

6. Verá una lista de las señales admitidas para el recurso, que incluye las de varias categorías del **registro de actividad**. Seleccione la señal o la operación del registro de actividad sobre la que desee crear una alerta.

7. Verá un gráfico para la operación del registro de actividad de las últimas seis horas. Use la lista desplegable **Período del gráfico** para ver un historial más largo de la operación.

8. En **Lógica de alerta**, puede definir opcionalmente más criterios de filtrado:

    - **Nivel de evento**: Nivel de gravedad del evento: _Detallado_, _Informativo_, _Advertencia_, _Error_ o _Crítico_.
    - **Estado**: Estado del evento: _Iniciado_ o _Erróneo_ o _Correcto_.
    - **Evento iniciado por**: También se denomina "autor de la llamada". La dirección de correo electrónico o el identificador de Azure Active Directory del usuario que realizó la operación.

    > [!NOTE]
    > La definición de al menos uno de estos criterios le ayuda a lograr reglas más eficaces. Por ejemplo, si el ámbito de alerta es una suscripción completa y la señal seleccionada es `All Administrative Operations`, la regla será más específica si proporciona el nivel de evento, el estado o la información de iniciación.
        
9. Seleccione **Listo**.

    :::image type="content" source="media/alerts-activity-log/condition-selected-new.png" alt-text="Captura de pantalla del panel de selección de condición." lightbox="media/alerts-activity-log/condition-selected-new.png":::

10. Rellene los detalles de las aalertas como **Nombre de la regla de alertas**, **Descripción** y **Gravedad**.

    > [!NOTE]
    > La gravedad de la alerta para las alertas del registro de actividad no se puede configurar actualmente por el usuario. El nivel de gravedad predeterminado siempre es **Sev4**.

11. Agregue un grupo de acciones a la alerta, ya sea seleccionando un grupo de acciones existente o creando uno nuevo.

12. Seleccione **Listo** para guardar la alerta del registro de actividad.
     
     
### <a name="create-an-alert-rule-from-the-azure-monitor-activity-log-pane"></a>Creación de una regla de alertas desde el panel del registro de actividad de Azure Monitor

Otra forma de crear una alerta del registro de actividad es comenzar con un evento de registro de actividad que ya se produjo, a través del [registro de actividad de Azure Portal](../essentials/activity-log.md#view-the-activity-log). 

1. En el panel **Azure Monitor: registro de actividad** se puede filtrar o buscar un evento que quiera y crear una alerta para eventos similares futuros seleccionando **Agregar alerta del registro de actividad**. 

    :::image type="content" source="media/alerts-activity-log/create-alert-rule-from-activity-log-event-new.png" alt-text="Captura de pantalla de la creación de reglas de alertas a partir de un evento de registro de actividad." lightbox="media/alerts-activity-log/create-alert-rule-from-activity-log-event-new.png":::

2. Se abre el panel de **creación de regla sde alertas** con el ámbito y la condición de la regla de alertas ya rellenados según el evento de registro de actividad seleccionado previamente. Puede editar y modificar el ámbito y la condición en esta fase si es necesario. Tenga en cuenta que, de forma predeterminada, el ámbito y la condición exactos de la nueva regla se copian de los atributos de evento originales. Por ejemplo, el recurso exacto en el que se produjo el evento y el nombre de usuario o servicio específico que inició el evento se incluyen de forma predeterminada en la nueva regla de alertas. Si desea que la regla de alertas sea más general, modifique el ámbito y la condición en consecuencia (consulte los pasos 3 a 9 de la sección "Creación de una regla de alertas desde el panel de alertas de Azure Monitor"). 

3. A continuación, siga los pasos del 10 al 12 de la sección "Creación de una regla de alertas desde el panel de alertas de Azure Monitor".
    
### <a name="view-and-manage-in-the-azure-portal"></a>Visualización y administración en Azure Portal

1. En Azure Portal, seleccione **Supervisar** > **Alertas**. Luego, seleccione **Administrar reglas de alertas**.

    :::image type="content" source="media/alerts-activity-log/manage-alert-rules-button-new.png" alt-text="Captura de pantalla del botón para administrar reglas de alertas.":::
    
    Aparece la lista de reglas disponibles.

2. Filtro o busque la regla del registro de actividad que quiera modificar.

    :::image type="content" source="media/alerts-activity-log/manage-alert-rules-new.png" alt-text="Captura de pantalla del panel de administración de reglas de alertas." lightbox="media/alerts-activity-log/manage-alert-rules-new.png":::

    Puede usar los filtros disponibles: _Suscripción_, _Grupo de recursos_, _Recursos_, _Tipo de señal_ o _Estado_ para buscar la regla de actividad que quiera editar.
 
3. Seleccione la regla y haga doble clic para editar las opciones de la misma. Realice los cambios necesarios y seleccione **Guardar**. 

## <a name="azure-resource-manager-template"></a>Plantilla del Administrador de recursos de Azure
Para crear una regla de alerta del registro de actividad mediante una plantilla de Azure Resource Manager, cree un recurso del tipo `microsoft.insights/activityLogAlerts`. A continuación, rellene todas las propiedades relacionadas. A continuación, se muestra una plantilla que crea una regla de alerta del registro de actividad:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
El ejemplo JSON anterior puede guardarse como, por ejemplo, *sampleActivityLogAlert.json*. Puede implementar el ejemplo mediante [Azure Resource Manager en Azure Portal](../../azure-resource-manager/templates/deploy-portal.md).

> [!NOTE]
> Tenga en cuenta que el nivel más alto en que se pueden definir las alertas del registro de actividad es el de suscripción. No hay ninguna opción para definir una alerta en dos suscripciones. La definición debe ser alertar por suscripción.

Los campos siguientes son las opciones que puede usar en la plantilla de Azure Resource Manager para los campos de condiciones. (Observe que **Resource Health**, **Advisor** y **Service Health** tienen campos de propiedades adicionales para sus campos especiales). 

1. `resourceId`: identificador del recurso afectado en el evento del registro de actividad en el que se debe generar la alerta.
1. `category`: categoría del evento del registro de actividad. Por ejemplo: `Administrative`, `ServiceHealth`, `ResourceHealth`, `Autoscale`, `Security`, `Recommendation` o `Policy`.
1. `caller`: dirección de correo electrónico o el identificador de Azure Active Directory del usuario que realizó la operación del evento del registro de actividad.
1. `level`: nivel de la actividad del evento del registro de actividad en el que se debe generar la alerta. Por ejemplo, `Critical`, `Error`, `Warning`, `Informational` o `Verbose`.
1. `operationName`: nombre de la operación en el evento del registro de actividad. Por ejemplo: `Microsoft.Resources/deployments/write`.
1. `resourceGroup`: nombre del grupo de recursos del recurso afectado en el evento del registro de actividad.
1. `resourceProvider` Para más información, consulte [Tipos y proveedores de recursos de Azure](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-resource-manager%2Fmanagement%2Fresource-providers-and-types&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373543634&sdata=4RjpTkO5jsdOgPdt%2F%2FDOlYjIFE2%2B%2BuoHq5%2F7lHpCwQw%3D&reserved=0). Para obtener una lista que asigna proveedores de recursos con servicios de Azure, consulte [Proveedores de recursos para servicios de Azure](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-resource-manager%2Fmanagement%2Fazure-services-resource-providers&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373553639&sdata=0ZgJPK7BYuJsRifBKFytqphMOxMrkfkEwDqgVH1g8lw%3D&reserved=0).
1. `status`: cadena que describe el estado de la operación en el evento de actividad. Por ejemplo: `Started`, `In Progress`, `Succeeded`, `Failed`, `Active` o `Resolved`.
1. `subStatus`: normalmente, es campo es el código de estado HTTP de la llamada de REST correspondiente. Pero también puede incluir otras cadenas que describen un subestado. Algunos ejemplos de códigos de estado HTTP son `OK`(código de estado HTTP: 200), `No Content` (código de estado HTTP: 204) y `Service Unavailable` (código de estado HTTP: 503), entre muchos otros.
1. `resourceType`: tipo de recurso que se vio afectado por el evento. Por ejemplo: `Microsoft.Resources/deployments`.

Por ejemplo:

```json
"condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        }

```

Para más información sobre los campos del registro de actividad, consulte [Esquema de eventos del registro de actividad de Azure](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-monitor%2Fplatform%2Factivity-log-schema&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373563632&sdata=6QXLswwZgUHFXCuF%2FgOSowLzA8iOALVgvL3GMVhkYJY%3D&reserved=0).

> [!NOTE]
> Una regla de alertas nueva del registro de actividad puede tardar hasta 5 minutos en activarse.

## <a name="rest-api"></a>API DE REST 
La API de alertas del registro de actividad de Azure Monitor es una API de REST. Es totalmente compatible con la API de REST de Azure Resource Manager. Se puede usar con PowerShell mediante el cmdlet de Resource Manager o la CLI de Azure.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="deploy-the-resource-manager-template-with-powershell"></a>Implementación de la plantilla de Resource Manager con PowerShell
Para usar PowerShell para implementar la plantilla de ejemplo de Resource Manager que se muestra en la sección anterior [Plantilla de Azure Resource Manager](#azure-resource-manager-template), use el siguiente comando:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```

El archivo *sampleActivityLogAlert.parameters.json* tiene los valores proporcionados para los parámetros que se necesitan para crear las reglas de alertas.

### <a name="use-activity-log-powershell-cmdlets"></a>Usar los cmdlets de PowerShell del registro de actividad

Las alertas del registro de actividad tienen cmdlets dedicados de PowerShell disponibles:

- [Set-AzActivityLogAlert](/powershell/module/az.monitor/set-azactivitylogalert): crea una nueva alerta de registro de actividad o actualiza una alerta de registro de actividad existente.
- [Get-AzActivityLogAlert](/powershell/module/az.monitor/get-azactivitylogalert): obtiene uno o más recursos de alerta del registro de actividad.
- [Enable-AzActivityLogAlert](/powershell/module/az.monitor/enable-azactivitylogalert): habilita una alerta de registro de actividad existente y establece sus etiquetas.
- [Disable-AzActivityLogAlert](/powershell/module/az.monitor/disable-azactivitylogalert): deshabilita una alerta de registro de actividad existente y establece sus etiquetas.
- [Remove-AzActivityLogAlert](/powershell/module/az.monitor/remove-azactivitylogalert): quita una alerta de registro de actividad.

### <a name="azure-cli"></a>Azure CLI

Se pueden administrar las reglas de alertas del registro de actividad mediante comandos dedicados de la CLI de Azure bajo el conjunto [az monitor activity-log alert](/cli/azure/monitor/activity-log/alert).

Para crear una nueva regla de alertas del registro de actividad, use los siguientes comandos:

1. [az monitor activity-log alert create](/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-create): para crear un nuevo recurso de regla de alertas del registro de actividad.
2. [az monitor activity-log alert scope](/cli/azure/monitor/activity-log/alert/scope): para agregar el ámbito de la regla de alertas creada del registro de actividad.
3. [az monitor activity-log alert action-group](/cli/azure/monitor/activity-log/alert/action-group): para agregar un grupo de acciones a la regla de alertas del registro de actividad.

Para recuperar un recurso de regla de alertas del registro de actividad, puede usar el comando [az monitor activity-log alert show](/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-show
) de la CLI de Azure. Asimismo, para ver todos los recursos de regla de alertas del registro de actividad en un grupo de recursos, use [az monitor activity-log alert list](/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list).
Se pueden quitar los recursos de regla de alertas del registro de actividad con el comando [az monitor activity-log alert delete](/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-delete) de la CLI de Azure.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información acerca del [esquema de webhook para los registros de actividad](./activity-log-alerts-webhook.md).
- Lea la [información general sobre los registros de actividad](./activity-log-alerts.md).
- Más información sobre los [grupos de acciones](./action-groups.md).  
- Más información acerca de las [Notificaciones del estado del servicio](../../service-health/service-notifications.md).