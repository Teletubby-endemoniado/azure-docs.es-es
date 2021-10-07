---
title: Supervisión de los cambios en la delegación en el inquilino de administración
description: Aprenda a supervisar toda la actividad de delegación de Azure Lighthouse en el inquilino de administración.
ms.date: 09/08/2021
ms.topic: how-to
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 7eb38ac8ac7f86fd179663fe7bfb3aa1fb4e8830
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124819582"
---
# <a name="monitor-delegation-changes-in-your-managing-tenant"></a>Supervisión de los cambios en la delegación en el inquilino de administración

Como proveedor de servicios, es posible que desee conocer cuándo se delegan las suscripciones o grupos de recursos del cliente al inquilino mediante [Azure Lighthouse](../overview.md), o cuándo se eliminan los recursos delegados anteriormente.

En el inquilino de administración, el [registro de actividad de Azure](../../azure-monitor/essentials/platform-logs-overview.md) realiza un seguimiento de la actividad de delegación en el nivel del inquilino. Esta actividad registrada incluye cualquier delegación agregada o eliminada de los inquilinos del cliente.

En este tema se explican los permisos necesarios para supervisar la actividad de delegación en el inquilino (en todos los clientes). También incluye un script de ejemplo que muestra un método para realizar consultas e informes sobre estos datos.

> [!IMPORTANT]
> Todos estos pasos se deben realizar en el inquilino de administración, en lugar de en los inquilinos de clientes.
>
> Aunque en este tema hacemos referencia a los proveedores de servicios y clientes, las [empresas que administran varios inquilinos](../concepts/enterprise.md) pueden usar los mismos procesos.

## <a name="enable-access-to-tenant-level-data"></a>Habilitar el acceso a los datos en el nivel del inquilino

Para acceder a los datos del registro de actividad en el nivel del inquilino, se tiene que asignar a una cuenta el rol integrado de Azure [Lector de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-reader) en el ámbito raíz (/). Esta asignación la debe realizar un usuario que tenga el rol Administrador global con privilegios de acceso elevados adicionales.

### <a name="elevate-access-for-a-global-administrator-account"></a>Elevación de los privilegios de acceso de una cuenta de administrador global

Para asignar un rol en el ámbito raíz (/), deberá disponer del rol Administrador global con privilegios de acceso elevados. Estos privilegios de acceso elevados solo se deben agregar cuando tenga que realizar la asignación de roles y, a continuación, eliminarlos cuando haya terminado.

Para obtener instrucciones detalladas sobre cómo agregar y eliminar la elevación, consulte [Elevación de los privilegios de acceso para administrar todas las suscripciones y los grupos de administración de Azure](../../role-based-access-control/elevate-access-global-admin.md).

Después de elevar los privilegios de acceso, la cuenta tendrá el rol Administrador de acceso de usuarios en Azure en el ámbito raíz. Esta asignación de roles le permite ver todos los recursos y asignar acceso en cualquier suscripción o grupo de administración en el directorio, así como realizar asignaciones de roles en el ámbito raíz.

### <a name="assign-the-monitoring-reader-role-at-root-scope"></a>Asignación del rol Lector de supervisión en el ámbito raíz

Una vez que haya elevado el acceso, puede asignar los permisos adecuados a una cuenta para que pueda consultar los datos del registro de actividad en el nivel de inquilino. Será necesario asignar el rol integrado de Azure [Lector de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-reader) a esta cuenta en el ámbito raíz del inquilino administrador.

> [!IMPORTANT]
> La concesión de una asignación de roles en el ámbito raíz significa que los mismos permisos se aplicarán a todos los recursos del inquilino. Dado que se trata de un nivel de acceso amplio, se recomienda [asignar este rol a una cuenta de entidad de servicio y usar esa cuenta para consultar los datos](#use-a-service-principal-account-to-query-the-activity-log).
> 
> También puede asignar el rol Lector de supervisión en el ámbito raíz a usuarios individuales o a grupos de usuarios para que puedan [ver información de delegación directamente en Azure Portal](#view-delegation-changes-in-the-azure-portal). Si lo hace, tenga en cuenta que se trata de un amplio nivel de acceso que se debe limitar al menor número posible de usuarios.

Use uno de los métodos siguientes para realizar la asignación en el ámbito raíz.

#### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

New-AzRoleAssignment -SignInName <yourLoginName> -Scope "/" -RoleDefinitionName "Monitoring Reader"  -ObjectId <objectId> 
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az role assignment create --assignee 00000000-0000-0000-0000-000000000000 --role "Monitoring Reader" --scope "/"
```

### <a name="remove-elevated-access-for-the-global-administrator-account"></a>Eliminación de la elevación de los privilegios de acceso de la cuenta de administrador global

Una vez que asigne el rol Lector de supervisión en el ámbito raíz a la cuenta deseada, asegúrese de [quitar el acceso con privilegios elevados](../../role-based-access-control/elevate-access-global-admin.md#remove-elevated-access) para la cuenta Administrador global, ya que este nivel de acceso ya no será necesario.

## <a name="view-delegation-changes-in-the-azure-portal"></a>Visualización de cambios de delegación en Azure Portal

Los usuarios a los que se asignó el rol Lector de supervisión en el ámbito raíz pueden ver los cambios de delegación directamente en Azure Portal.

1. Vaya a la página **Mis clientes** y, después, seleccione **Registro de actividades** en el menú de navegación izquierdo.
1. Asegúrese de que **Actividad de directorio** está seleccionado en el filtro situado cerca de la parte superior de la pantalla.

Aparecerá una lista de cambios de delegación. Puede seleccionar **Editar columnas** para mostrar u ocultar los valores de **Estado**, **Categoría de evento**, **Hora**, **Marca de tiempo**, **Suscripción**, **Evento iniciado por**, **Grupo de recursos**, **Tipo de recurso** y **Recurso**.

:::image type="content" source="../media/delegation-activity-portal.jpg" alt-text="Captura de pantalla de cambios de delegación en Azure Portal.":::

## <a name="use-a-service-principal-account-to-query-the-activity-log"></a>Uso de una cuenta de entidad de servicio para consultar el registro de actividad

Como el rol Lector de supervisión en el ámbito raíz es un nivel de acceso muy amplio, quizás desee asignar el rol a una cuenta de entidad de servicio y utilizar esa cuenta para consultar los datos mediante el script siguiente.

> [!IMPORTANT]
> Actualmente, los inquilinos que tienen una gran cantidad de actividad de delegación pueden encontrar errores al consultar estos datos.

Al usar una cuenta de entidad de servicio para consultar el registro de actividad, se recomiendan los procedimientos recomendados siguientes:

- [Cree una nueva cuenta de entidad de servicio](../../active-directory/develop/howto-create-service-principal-portal.md) que se usará solo para esta función, en vez de asignar este rol a una entidad de servicio existente usada para otra automatización.
- Asegúrese de que esta entidad de servicio no tiene acceso a ningún recurso de cliente delegado.
- [Use un certificado para la autenticación](../../active-directory/develop/howto-create-service-principal-portal.md#authentication-two-options) y [almacénelo de forma segura en Azure Key Vault](../../key-vault/general/security-features.md).
- Limite los usuarios que tienen acceso para actuar en nombre de la entidad de servicio.

Una vez que haya creado una nueva cuenta de entidad de servicio con el acceso de Lector de supervisión en el ámbito raíz del inquilino de administración, puede usarla para consultar e informar sobre las actividades de delegación en el inquilino.

[Este script de Azure PowerShell](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/monitor-delegation-changes) se puede usar para consultar el último día de actividad y notifica sobre cualquier delegación agregada o eliminada (o intentos de delegaciones que no se realizaron correctamente). Consulta los datos del [registro de actividad del inquilino](/rest/api/monitor/TenantActivityLogs/List) y, a continuación, construye los valores siguientes para notificar las delegaciones que se han agregado o eliminado:

- **DelegatedResourceId**: El identificador de la suscripción o grupo de recursos delegados
- **CustomerTenantId**: El identificador de inquilino del cliente
- **CustomerSubscriptionId**: El identificador de la suscripción que se ha delegado o que contiene el grupo de recursos que se ha delegado
- **CustomerDelegationStatus**: El cambio de estado del recurso delegado (correcto o erróneo)
- **EventTimeStamp**: Fecha y hora en que se registró el cambio de delegación

Al consultar estos datos, tenga en cuenta lo siguiente:

- Si se delegan varios grupos de recursos en una sola implementación, se devolverán entradas independientes para cada grupo de recursos.
- Los cambios realizados en una delegación anterior (como la actualización de la estructura de permisos) se registrarán como una delegación agregada.
- Como se indicó anteriormente, una cuenta tiene que tener el rol integrado de Azure Lector de supervisión en el ámbito raíz (/) para poder acceder a estos datos en el nivel de inquilino.
- Puede usar estos datos en sus propios flujos de trabajo e informes. Por ejemplo, puede usar [HTTP Data Collector API (versión preliminar pública)](../../azure-monitor/logs/data-collector-api.md) para registrar datos en Azure Monitor desde un cliente de la API REST y, después, usar [grupos de acciones](../../azure-monitor/alerts/action-groups.md) para crear notificaciones o alertas.

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Azure Lighthouse: Query Tenant Activity Log for registered/unregistered delegations for the last 1 day

$GetDate = (Get-Date).AddDays((-1))

$dateFormatForQuery = $GetDate.ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ssZ")

# Getting Azure context for the API call
$currentContext = Get-AzContext

# Fetching new token
$azureRmProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = [Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient]::new($azureRmProfile)
$token = $profileClient.AcquireAccessToken($currentContext.Tenant.Id)

$listOperations = @{
    Uri     = "https://management.azure.com/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&`$filter=eventTimestamp ge '$($dateFormatForQuery)'"
    Headers = @{
        Authorization  = "Bearer $($token.AccessToken)"
        'Content-Type' = 'application/json'
    }
    Method  = 'GET'
}
$list = Invoke-RestMethod @listOperations

# First link can be empty - and point to a next link (or potentially multiple pages)
# While you get more data - continue fetching and add result
while($list.nextLink){
    $list2 = Invoke-RestMethod $list.nextLink -Headers $listOperations.Headers -Method Get
    $data+=$list2.value;
    $list.nextLink = $list2.nextlink;
}

$showOperations = $data;

if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/register/action") {
    $registerOutputs = $showOperations | Where-Object -FilterScript { $_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/register/action" }
    foreach ($registerOutput in $registerOutputs) {
        $eventDescription = $registerOutput.description | ConvertFrom-Json;
    $registerOutputdata = [pscustomobject]@{
        Event                    = "An Azure customer has registered delegated resources to your Azure tenant";
        DelegatedResourceId      = $eventDescription.delegationResourceId; 
        CustomerTenantId         = $eventDescription.subscriptionTenantId;
        CustomerSubscriptionId   = $eventDescription.subscriptionId;
        CustomerDelegationStatus = $registerOutput.status.value;
        EventTimeStamp           = $registerOutput.eventTimestamp;
        }
        $registerOutputdata | Format-List
    }
}
if ($showOperations.operationName.value -eq "Microsoft.Resources/tenants/unregister/action") {
    $unregisterOutputs = $showOperations | Where-Object -FilterScript { $_.eventName.value -eq "EndRequest" -and $_.resourceType.value -and $_.operationName.value -eq "Microsoft.Resources/tenants/unregister/action" }
    foreach ($unregisterOutput in $unregisterOutputs) {
        $eventDescription = $registerOutput.description | ConvertFrom-Json;
    $unregisterOutputdata = [pscustomobject]@{
        Event                    = "An Azure customer has unregistered delegated resources from your Azure tenant";
        DelegatedResourceId      = $eventDescription.delegationResourceId;
        CustomerTenantId         = $eventDescription.subscriptionTenantId;
        CustomerSubscriptionId   = $eventDescription.subscriptionId;
        CustomerDelegationStatus = $unregisterOutput.status.value;
        EventTimeStamp           = $unregisterOutput.eventTimestamp;
        }
        $unregisterOutputdata | Format-List
    }
}
else {
    Write-Output "No new delegation events for tenant: $($currentContext.Tenant.TenantId)"
}
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [incorporar clientes a Azure Lighthouse](onboard-customer.md).
- Más información sobre [Azure Monitor](../../azure-monitor/index.yml) y el [registro de actividad de Azure](../../azure-monitor/essentials/platform-logs-overview.md).
- Revise el libro de ejemplo [Registros de actividad por dominio](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/workbook-activitylogs-by-domain) para aprender a mostrar los registros de actividad de Azure entre suscripciones con una opción para filtrarlos por nombre de dominio.
