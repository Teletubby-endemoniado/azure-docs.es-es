---
title: 'Trabajo con grupos de administración: Gobernanza en Azure'
description: Aprenda a visualizar, mantener, actualizar y eliminar la jerarquía de grupos de administración.
ms.date: 08/17/2021
ms.topic: conceptual
ms.openlocfilehash: 57c4af2d7c3d5d4978d500beaa6e6c8be5d12d4f
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131509399"
---
# <a name="manage-your-resources-with-management-groups"></a>Administración de los recursos con grupos de administración

Si su organización tiene varias suscripciones, puede que necesite una manera de administrar el acceso, las directivas y el cumplimiento de esas suscripciones de forma eficaz. Los grupos de administración de Azure proporcionan un nivel de ámbito por encima de las suscripciones. Las suscripciones se organizan en contenedores llamados "grupos de administración" y aplican sus condiciones de gobernanza a los grupos de administración. Todas las suscripciones dentro de un grupo de administración heredan automáticamente las condiciones que se aplican al grupo de administración.

Los grupos de administración proporcionan capacidad de administración de nivel empresarial a gran escala con independencia del tipo de suscripciones que tenga. Para más información acerca de los grupos de administración, consulte [Organización de los recursos con grupos de administración de Azure](./overview.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

> [!IMPORTANT]
> Los tokens de usuario y la memoria caché del grupo de administración de Azure Resource Manager duran 30 minutos antes de que se produzca una actualización obligatoria. Después de realizar cualquier acción como mover un grupo de administración o una suscripción, puede tardar hasta 30 minutos en mostrarse. Para ver las actualizaciones antes de que tenga que actualizar el token actualizando el explorador, inicie y cierre sesión, o solicite un nuevo token.

> [!IMPORTANT]
> Los cmdlets de Az PowerShell relacionados con AzManagementGroup mencionan que **-GroupId** es alias del parámetro **-GroupName**, por lo que podemos usar cualquiera de ellos para proporcionar el identificador del grupo de administración como valor de cadena. 

## <a name="change-the-name-of-a-management-group"></a>Cambio del nombre de un grupo de administración

Puede cambiar el nombre del grupo de administración mediante el portal, PowerShell o la CLI de Azure.

### <a name="change-the-name-in-the-portal"></a>Cambiar el nombre en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración cuyo nombre quiere cambiar.

1. Seleccione **Detalles**.

1. Seleccione la opción **Cambiar nombre de grupo** en la parte superior de la página.

   :::image type="content" source="./media/detail_action_small.png" alt-text="Captura de pantalla de la barra de acciones y el botón &quot;Cambiar nombre de grupo&quot; en la página grupo de administración." border="false":::

1. Cuando se abra el menú, escriba el nuevo nombre que desea mostrar.

   :::image type="content" source="./media/rename_context.png" alt-text="Captura de pantalla de la ventana Cambiar nombre de grupo y las opciones para cambiar el nombre de un grupo de administración." border="false":::

1. Seleccione **Guardar**.

### <a name="change-the-name-in-powershell"></a>Cambiar el nombre en PowerShell

Para actualizar el nombre para mostrar, use **Update-AzManagementGroup**. Por ejemplo, para cambiar el nombre para mostrar de un grupo de administración de "Contoso TI" a "Grupo de Contoso", ejecute el siguiente comando:

```azurepowershell-interactive
Update-AzManagementGroup -GroupId 'ContosoIt' -DisplayName 'Contoso Group'
```

### <a name="change-the-name-in-azure-cli"></a>Cambiar el nombre en la CLI de Azure

En la CLI de Azure, use el comando de actualización.

```azurecli-interactive
az account management-group update --name 'Contoso' --display-name 'Contoso Group'
```

## <a name="delete-a-management-group"></a>Eliminación de un grupo de administración

Para eliminar un grupo de administración, deben cumplirse los siguientes requisitos:

1. No deben existir grupos de administración secundarios ni suscripciones en el grupo de administración. Para mover una suscripción o un grupo de administración a otro grupo de administración, consulte [Movimiento de grupos de administración y suscripciones en la jerarquía](#moving-management-groups-and-subscriptions).

1. Necesita permisos de escritura sobre el grupo de administración (propietario, colaborador o colaborador de grupo de administración). Para ver qué permisos tiene, seleccione el grupo de administración y, a continuación, seleccione **IAM**. Para más información sobre los roles de Azure, consulte [¿Qué es el control de acceso basado en roles de Azure (RBAC)?](../../role-based-access-control/overview.md).

### <a name="delete-in-the-portal"></a>Eliminar en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración que desea eliminar.

1. Seleccione **Detalles**.

1. Seleccionar **Eliminar**

   :::image type="content" source="./media/delete.png" alt-text="Captura de pantalla de la página Grupo de administración con el botón &quot;Eliminar&quot; resaltado." border="false":::

   > [!TIP]
   > Si el icono está deshabilitado, al mantener el selector del mouse sobre el icono se muestra el motivo.

1. Se abre una ventana para que confirme si quiere eliminar el grupo de administración.

   :::image type="content" source="./media/delete_confirm.png" alt-text="Captura de pantalla del cuadro de diálogo de confirmación &quot;Eliminar grupo&quot; para eliminar un grupo de administración." border="false":::

1. Seleccione **Sí**.

### <a name="delete-in-powershell"></a>Eliminar en PowerShell

Use el comando **Remove-AzManagementGroup** de PowerShell para eliminar grupos de administración.

```azurepowershell-interactive
Remove-AzManagementGroup -GroupId 'Contoso'
```

### <a name="delete-in-azure-cli"></a>Eliminación en la CLI de Azure

Con la CLI de Azure, use el comando az account management-group delete.

```azurecli-interactive
az account management-group delete --name 'Contoso'
```

## <a name="view-management-groups"></a>Visualización de los grupos de administración

Puede ver cualquier grupo de administración sobre el que tenga un rol de Azure directo o heredado.

### <a name="view-in-the-portal"></a>Ver en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Se cargará la página de la jerarquía de grupos de administración. En esta página puede explorar todos los grupos de administración y las suscripciones a los que tiene acceso. Si selecciona el nombre del grupo, desciende a un nivel inferior de la jerarquía. La navegación funciona de la misma forma que un explorador de archivos.

1. Para ver los detalles del grupo de administración, seleccione el vínculo **(detalles)** situado junto al título de este. Si este vínculo no está disponible, no tiene permisos para ver ese grupo de administración.

   :::image type="content" source="./media/main.png" alt-text="Captura de pantalla de la página Grupos de administración que muestra las suscripciones y los grupos de administración secundarios." border="false":::

### <a name="view-in-powershell"></a>Ver en PowerShell

Use el comando Get-AzManagementGroup para recuperar todos los grupos. Consulte los módulos [Az.Resources](/powershell/module/az.resources/Get-AzManagementGroup) para ver la lista completa de comandos GET de PowerShell del grupo de administración.

```azurepowershell-interactive
Get-AzManagementGroup
```

Para obtener información de un único grupo de administración, use el parámetro -GroupId.

```azurepowershell-interactive
Get-AzManagementGroup -GroupId 'Contoso'
```

Para devolver un grupo de administración específico y todos los niveles de la jerarquía que hay debajo, use los parámetros **-Expand** y **-Recurse**.

```azurepowershell-interactive
PS C:\> $response = Get-AzManagementGroup -GroupId TestGroupParent -Expand -Recurse
PS C:\> $response

Id                : /providers/Microsoft.Management/managementGroups/TestGroupParent
Type              : /providers/Microsoft.Management/managementGroups
Name              : TestGroupParent
TenantId          : 00000000-0000-0000-0000-000000000000
DisplayName       : TestGroupParent
UpdatedTime       : 2/1/2018 11:15:46 AM
UpdatedBy         : 00000000-0000-0000-0000-000000000000
ParentId          : /providers/Microsoft.Management/managementGroups/00000000-0000-0000-0000-000000000000
ParentName        : 00000000-0000-0000-0000-000000000000
ParentDisplayName : 00000000-0000-0000-0000-000000000000
Children          : {TestGroup1DisplayName, TestGroup2DisplayName}

PS C:\> $response.Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestGroup1
Name        : TestGroup1
DisplayName : TestGroup1DisplayName
Children    : {TestRecurseChild}

PS C:\> $response.Children[0].Children[0]

Type        : /managementGroup
Id          : /providers/Microsoft.Management/managementGroups/TestRecurseChild
Name        : TestRecurseChild
DisplayName : TestRecurseChild
Children    :
```

### <a name="view-in-azure-cli"></a>Ver en la CLI de Azure

Use el comando list para recuperar todos los grupos.

```azurecli-interactive
az account management-group list
```

Para obtener información de un único grupo de administración, use el comando show.

```azurecli-interactive
az account management-group show --name 'Contoso'
```

Para devolver un grupo de administración específico y todos los niveles de la jerarquía que hay debajo, use los parámetros **-Expand** y **-Recurse**.

```azurecli-interactive
az account management-group show --name 'Contoso' -e -r
```

## <a name="moving-management-groups-and-subscriptions"></a>Movimiento de grupos de administración y suscripciones

Uno de los motivos de crear un grupo de administración es agrupar las suscripciones. Solo los grupos de administración y las suscripciones pueden convertirse en secundarios de otro grupo de administración. Una suscripción que se mueve a un grupo de administración hereda todas las directivas y accesos de usuario del grupo de administración primario.

Al mover un grupo de administración o una suscripción para ser secundarios de otro grupo de administración, es preciso evaluar tres reglas como verdaderas.

Si va a realizar un traslado, necesita permiso en cada una de las capas siguientes:

- Suscripción o grupo de administración secundarios
  - `Microsoft.management/managementgroups/write`
  - `Microsoft.management/managementgroups/subscription/write` (solo para suscripciones)
  - `Microsoft.Authorization/roleAssignments/write`
  - `Microsoft.Authorization/roleAssignments/delete`
  - `Microsoft.Management/register/action`
- Grupo de administración primario de destino
  - `Microsoft.management/managementgroups/write`
- Grupo de administración primario actual
  - `Microsoft.management/managementgroups/write`

**Excepción**: Si el grupo de administración primario existente o de destino es el grupo de administración raíz, no se aplican los requisitos de permisos. Puesto que el grupo de administración raíz es la zona de aterrizaje predeterminada de todos los nuevos grupos de administración y suscripciones, no necesita permisos sobre él para mover un elemento.

Si el rol de propietario de la suscripción se hereda del grupo de administración actual, los destinos de movimiento están limitados. Solo puede mover la suscripción a otro grupo de administración en el que tenga el rol de propietario. No puede moverla a un grupo de administración en el que solo sea colaborador porque perdería la propiedad de la suscripción. Si se le asigna directamente el rol de propietario de la suscripción, puede moverla a cualquier grupo de administración donde sea colaborador.

Para ver qué permisos tiene en Azure Portal, seleccione el grupo de administración y, luego, **IAM**. Para más información sobre los roles de Azure, consulte [¿Qué es el control de acceso basado en roles de Azure (RBAC)?](../../role-based-access-control/overview.md).

## <a name="move-subscriptions"></a>Movimiento de suscripciones

### <a name="add-an-existing-subscription-to-a-management-group-in-the-portal"></a>Adición una suscripción existente a un grupo de administración del portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración que tiene previsto que sea el primario.

1. En la parte superior de la página, haga clic en **Agregar suscripción**.

1. Seleccione la suscripción de la lista con el identificador correcto.

   :::image type="content" source="./media/add_context_sub.png" alt-text="Captura de pantalla de las opciones &quot;Agregar suscripción&quot; para seleccionar una suscripción existente para agregarla a un grupo de administración." border="false":::

1. Seleccione "Guardar".

### <a name="remove-a-subscription-from-a-management-group-in-the-portal"></a>Eliminación de una suscripción de un grupo de administración en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración que tiene previsto que sea el primario actual.

1. Seleccione los puntos suspensivos al final de la fila correspondiente a la suscripción de la lista que quiere mover.

   :::image type="content" source="./media/move_small.png" alt-text="Captura de pantalla del menú alternativo para una suscripción para seleccionar la opción &quot;Mover&quot;." border="false":::

1. Seleccione **Mover**.

1. En el menú que se abre, seleccione el **grupo de administración primario**.

   :::image type="content" source="./media/move_small_context.png" alt-text="Captura de pantalla de la ventana &quot;Mover&quot; y opciones para mover una suscripción a otro grupo de administración." border="false":::

1. Seleccione **Guardar**.

### <a name="move-subscriptions-in-powershell"></a>Mover las suscripciones en PowerShell

Para mover una suscripción en PowerShell, use el comando New-AzManagementGroupSubscription.

```azurepowershell-interactive
New-AzManagementGroupSubscription -GroupId 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

Para quitar el vínculo entre la suscripción y el grupo de administración, use el comando Remove-AzManagementGroupSubscription.

```azurepowershell-interactive
Remove-AzManagementGroupSubscription -GroupId 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-azure-cli"></a>Mover las suscripciones en la CLI de Azure

Para mover una suscripción en la CLI, utilice el comando add.

```azurecli-interactive
az account management-group subscription add --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

Para quitar la suscripción del grupo de administración, use el comando subscription remove.

```azurecli-interactive
az account management-group subscription remove --name 'Contoso' --subscription '12345678-1234-1234-1234-123456789012'
```

### <a name="move-subscriptions-in-arm-template"></a>Traslado de las suscripciones de la plantilla de ARM

Para trasladar una suscripción de una plantilla de Azure Resource Manager (plantilla de ARM), use la siguiente plantilla e impleméntela en el [nivel de inquilino](../../azure-resource-manager/templates/deploy-to-tenant.md).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "targetMgId": {
            "type": "string",
            "metadata": {
                "description": "Provide the ID of the management group that you want to move the subscription to."
            }
        },
        "subscriptionId": {
            "type": "string",
            "metadata": {
                "description": "Provide the ID of the existing subscription to move."
            }
        }
    },
    "resources": [
        {
            "scope": "/",
            "type": "Microsoft.Management/managementGroups/subscriptions",
            "apiVersion": "2020-05-01",
            "name": "[concat(parameters('targetMgId'), '/', parameters('subscriptionId'))]",
            "properties": {
            }
        }
    ],
    "outputs": {}
}
```

## <a name="move-management-groups"></a>Movimiento de grupos de administración

### <a name="move-management-groups-in-the-portal"></a>Mover grupos de administración en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Grupos de administración**.

1. Seleccione el grupo de administración que tiene previsto que sea el primario.

1. En la parte superior de la página, haga clic en **Agregar grupo de administración**.

1. En el menú que se abre, seleccione si quiere un grupo de administración nuevo o usar uno existente.

   - Al seleccionar Nuevo se crea un grupo de administración.
   - Al seleccionar un grupo existente se mostrará una lista desplegable de todos los grupos de administración que se pueden mover a este grupo de administración.

   :::image type="content" source="./media/add_context_MG.png" alt-text="Captura de pantalla de las opciones de &quot;Agregar grupo de administración&quot; para crear un nuevo grupo de administración." border="false":::

1. Seleccione **Guardar**.

### <a name="move-management-groups-in-powershell"></a>Mover grupos de administración en PowerShell

Use el comando Update-AzManagementGroup de PowerShell para mover un grupo de administración a un grupo diferente.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupId ContosoIT
Update-AzManagementGroup -GroupId 'Contoso' -ParentId $parentGroup.id
```

### <a name="move-management-groups-in-azure-cli"></a>Mover grupos de administración en la CLI de Azure

Use el comando update para mover un grupo de administración con CLI de Azure.

```azurecli-interactive
az account management-group update --name 'Contoso' --parent ContosoIT
```

## <a name="audit-management-groups-using-activity-logs"></a>Auditoría de los grupos de administración mediante registros de actividad

Se admiten grupos de administración en el [registro de actividad de Azure](../../azure-monitor/essentials/platform-logs-overview.md). Puede consultar todos los eventos que se producen en un grupo de administración en la misma ubicación central que otros recursos de Azure. Por ejemplo, puede ver todos los cambios de asignaciones de roles o de asignación de directiva efectuados en un grupo de administración concreto.

:::image type="content" source="./media/al-mg.png" alt-text="Captura de pantalla de los registros de actividad y las operaciones relacionadas con el grupo de administración seleccionado." border="false":::

Si observa las consultas en los grupos de administración fuera de Azure Portal, el ámbito de destino de los grupos de administración se parece a **"/providers/Microsoft.Management/managementGroups/{yourMgID}"** .

## <a name="referencing-management-groups-from-other-resource-providers"></a>Referencia a grupos de administración de otros proveedores de recursos

Al hacer referencia a grupos de administración desde las acciones de otro proveedor de recursos, use la siguiente ruta de acceso como ámbito. Esta ruta de acceso se usa cuando se utiliza PowerShell, la CLI de Azure y las API REST.

`/providers/Microsoft.Management/managementGroups/{yourMgID}`

Un ejemplo de uso de esta ruta de acceso es cuando se realiza una nueva asignación de roles a un grupo de administración en PowerShell:

```azurepowershell-interactive
New-AzRoleAssignment -Scope "/providers/Microsoft.Management/managementGroups/Contoso"
```

La misma ruta de acceso al ámbito se usa al recuperar una definición de directiva en un grupo de administración.

```http
GET https://management.azure.com/providers/Microsoft.Management/managementgroups/MyManagementGroup/providers/Microsoft.Authorization/policyDefinitions/ResourceNaming?api-version=2019-09-01
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los grupos de administración, consulte:

- [Creación de grupos de administración para organizar los recursos de Azure](./create-management-group-portal.md)
- [Cambio, eliminación y administración de los grupos de administración](./manage.md)
- [Revisión de grupos de administración en el módulo de recursos de Azure PowerShell](/powershell/module/az.resources#resources)
- [Revisión de grupos de administración en la API REST](/rest/api/managementgroups/managementgroups)
- [Revisión de grupos de administración en la CLI de Azure](/cli/azure/account/management-group)
