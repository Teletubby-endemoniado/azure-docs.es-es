---
title: 'Protección de zonas y registros DNS privados: Azure DNS'
description: En esta ruta de aprendizaje, comenzará a proteger las zonas y los conjuntos de registros DNS privados en Microsoft Azure DNS.
services: dns
ms.service: dns
author: duongau
ms.author: duau
ms.topic: how-to
ms.date: 05/07/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: b64d20e07ca78d18662f3e7aa7fe6ef1f822a159
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128659804"
---
# <a name="how-to-protect-private-dns-zones-and-records"></a>Protección de zonas y registros DNS privados

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Las zonas y los registros DNS privados son recursos críticos. La eliminación de una zona DNS o incluso de un solo un registro DNS puede dar lugar a una interrupción total del servicio. Es importante proteger las zonas y los registros DNS contra cambios accidentales o no autorizados.

En este artículo se explica cómo Azure DNS le permite proteger las zonas y los registros DNS privados de dichos cambios.  Se aplican dos características de seguridad eficaces que proporciona Azure Resource Manager: [control de acceso basado en rol de Azure (Azure RBAC)](../role-based-access-control/overview.md) y [bloqueos de recursos](../azure-resource-manager/management/lock-resources.md).

## <a name="azure-role-based-access-control"></a>Control de acceso basado en roles de Azure

El control de acceso basado en rol de Azure (Azure RBAC) permite realizar una administración detallada del acceso de usuarios, grupos y recursos de Azure. Con RBAC de Azure, puede conceder el nivel de acceso que necesitan los usuarios. Para más información sobre cómo Azure RBAC ayuda a administrar el acceso, consulte [¿Qué es el control de acceso basado en rol de Azure (RBAC)?](../role-based-access-control/overview.md)

### <a name="the-private-dns-zone-contributor-role"></a>Rol de colaborador de zona DNS privada

El rol de colaborador de zona DNS privada es un rol integrado para administrar recursos DNS privados. Este rol aplicado a un usuario o un grupo les permite administrar recursos DNS privados.

El grupo de recursos *myPrivateDNS* contiene cinco zonas de Contoso Corporation. Cuando se conceden permisos de colaborador de zona DNS privada de administrador de DNS a ese grupo de recursos, se permite el control total sobre esas zonas DNS. Se evita la concesión de permisos innecesarios. El administrador de DNS no puede crear ni detener máquinas virtuales.

La manera más sencilla de asignar permisos de Azure RBAC es [a través de Azure Portal](../role-based-access-control/role-assignments-portal.md).  

Abra **Control de acceso (IAM)** del grupo de recursos, seleccione **Agregar** y, luego, seleccione el rol **Colaborador de zona DNS privada**. Seleccione los usuarios o grupos necesarios para conceder permisos.

:::image type="content" source="./media/dns-protect-private-zones-recordsets/resource-group-rbac.png" alt-text="Captura de pantalla de RBAC para el grupo de recursos DNS privado.":::

Los permisos también se pueden [conceder mediante Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell-interactive
# Grant 'Private DNS Zone Contributor' permissions to all zones in a resource group

$rsg = "<resource group name>"
$usr = "<user email address>"
$rol = "Private DNS Zone Contributor"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -ResourceGroupName $rsg
```

El comando equivalente también está [disponible a través de la CLI de Azure](../role-based-access-control/role-assignments-cli.md):

```azurecli-interactive
# Grant 'Private DNS Zone Contributor' permissions to all zones in a resource group

az role assignment create \
--assignee "<user email address>" \
--role "Private DNS Zone Contributor" \
--resource-group "<resource group name>"
```

### <a name="private-zone-level-azure-rbac"></a>Permiso de Azure RBAC de nivel de zona privada

Las reglas de RBAC de Azure pueden aplicarse a una suscripción, a un grupo de recursos o a un recurso individual. Ese recurso puede ser una zona DNS individual o un conjunto de registros individual.

Por ejemplo, el grupo de recursos *myPrivateDNS* contiene la zona *private.contoso.com* y una subzona *customers.private.contoso.com*. Para cada cuenta de cliente se crean registros CNAME. La cuenta de administrador que se usa para administrar los registros CNAME recibe permisos para crear registros en la zona *customers.private.contoso.com*. La cuenta solo puede administrar *customers.private.contoso.com*.

Los permisos de Azure RBAC de nivel de zona se pueden conceder a través de Azure Portal.  Abra **Control de acceso (IAM)** de la zona, seleccione **Agregar** y, luego, seleccione el rol **Colaborador de zona DNS privada**. Seleccione los usuarios o grupos necesarios para conceder permisos.

:::image type="content" source="./media/dns-protect-private-zones-recordsets/zone-rbac.png" alt-text="Captura de pantalla de RBAC para la zona DNS privada.":::

Los permisos también se pueden [conceder mediante Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell-interactive
# Grant 'Private DNS Zone Contributor' permissions to a specific zone

$rsg = "<resource group name>"
$usr = "<user email address>"
$zon = "<zone name>"
$rol = "Private DNS Zone Contributor"
$rsc = "Microsoft.Network/privateDnsZones"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -ResourceGroupName $rsg -ResourceName $zon -ResourceType $rsc
```

El comando equivalente también está [disponible a través de la CLI de Azure](../role-based-access-control/role-assignments-cli.md):

```azurecli-interactive
# Grant 'Private DNS Zone Contributor' permissions to a specific zone

az role assignment create \
--assignee <user email address> \
--role "Private DNS Zone Contributor" \
--scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/privateDnsZones/<zone name>/"
```

### <a name="record-set-level-azure-rbac"></a>Permiso de Azure RBAC de nivel de conjunto de registros

Los permisos se aplican en el nivel de conjunto de registros.  Se concede al usuario control sobre las entradas que necesita y no puede realizar ningún otro cambio.

Los permisos de Azure RBAC de nivel de conjunto de registros se pueden configurar a través de Azure Portal. Para ello, utilice el botón **Control de acceso (IAM)** de la página del conjunto de registros:

:::image type="content" source="./media/dns-protect-private-zones-recordsets/record-set-rbac-1.png" alt-text="Captura de pantalla de RBAC para el grupo de recursos DNS privado.":::

:::image type="content" source="./media/dns-protect-private-zones-recordsets/record-set-rbac-2.png" alt-text="Captura de pantalla de asignación de roles para el grupo de recursos DNS privado.":::

Los permisos de Azure RBAC de nivel de conjunto de registros también se pueden [conceder mediante Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

```azurepowershell-interactive
# Grant permissions to a specific record set

$usr = "<user email address>"
$rol = "Private DNS Zone Contributor"
$sco = 
"/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/privateDnsZones/<zone name>/<record type>/<record name>"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -Scope $sco
```

El comando equivalente también está [disponible a través de la CLI de Azure](../role-based-access-control/role-assignments-cli.md):

```azurecli-interactive
# Grant permissions to a specific record set

az role assignment create \
--assignee "<user email address>" \
--role "Private DNS Zone Contributor" \
--scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/privateDnsZones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Roles personalizados

El rol Colaborador de zona DNS privada integrado permite control total sobre un recurso DNS. También es posible crear roles de Azure de cliente propios, para proporcionar un control más detallado.

La cuenta que se use para administrar CNAME solo debe tener permiso para administrar registros CNAME. La cuenta no puede modificar registros de otros tipos. La cuenta no puede realizar operaciones de nivel de zona, como la eliminación de zonas.

En el ejemplo siguiente se muestra una definición de rol personalizado para administrar únicamente registros CNAME:

```json
{
    "Name": "Private DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/privateDnsZones/CNAME/*",
        "Microsoft.Network/privateDNSZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

La propiedad Actions define los siguientes permisos específicos de DNS:

* `Microsoft.Network/privateDnsZones/CNAME/*` concede control total sobre registros CNAME
* `Microsoft.Network/privateDNSZones/read` concede permiso para leer zonas DNS privadas, pero no para modificarlas, lo que permite ver la zona en la que se crea el registro CNAME.

> [!NOTE]
> El uso de un rol personalizado de Azure para evitar que se eliminen conjuntos de registros mientras se permite modificarlos no es un control eficaz. Evita que los conjuntos de registros se eliminen, pero no que se modifiquen.  Entre las modificaciones permitidas figuran la adición y eliminación de registros desde el conjunto de registros, incluida la eliminación de todos los registros para dejar un conjunto de registros empty. Esto tiene el mismo efecto que eliminar el conjunto de registros desde un punto de vista de la resolución DNS.

Actualmente no pueden realizarse definiciones de roles personalizados a través de Azure Portal. Puede crearse un rol personalizado basado en esta definición de rol mediante Azure PowerShell:

```azurepowershell-interactive
# Create new role definition based on input file

New-AzRoleDefinition -InputFile <file path>
```

También puede crearse a través de la CLI de Azure:

```azurecli-interactive
# Create new role definition based on input file

az role create -inputfile <file path>
```

El rol se puede asignar después del mismo modo que los roles integrados, tal cual se describe anteriormente en este artículo.

Para obtener más información sobre cómo crear, administrar y asignar roles personalizados, consulte [Roles personalizados de Azure](../role-based-access-control/custom-roles.md).

## <a name="resource-locks"></a>Bloqueos de recursos

Azure Resource Manager admite otro tipo de control de seguridad: la posibilidad de bloquear recursos. Los bloqueos de recursos se aplican al recurso y están en vigor en todos los usuarios y roles. Para obtener más información, consulte [Bloqueo de recursos con el Administrador de recursos de Azure](../azure-resource-manager/management/lock-resources.md).

Existen dos tipos de bloqueos de recursos: **CanNotDelete** y **ReadOnly**. Estos tipos de bloqueos pueden aplicarse a una zona DNS privada o a un conjunto de registros individual.  En las secciones siguientes se describen varios escenarios comunes y cómo mantenerlos con bloqueos de recursos.

### <a name="protecting-against-all-changes"></a>Protección contra todos los cambios

Para evitar que se realicen cambios, aplique un bloqueo ReadOnly a la zona. Este bloqueo impide que se creen otros conjuntos de registros y que se modifiquen o eliminen conjuntos de registros existentes.

Pueden crearse bloqueos de recursos de nivel de zona a través de Azure Portal.  En la página de la zona DNS, seleccione **Bloqueos** y después seleccione **+ Agregar**:

:::image type="content" source="./media/dns-protect-private-zones-recordsets/zone-locks.png" alt-text="Captura de pantalla de bloqueos para la zona DNS privada.":::

También pueden crearse bloqueos de recursos de nivel de zona a través de [Azure PowerShell](/powershell/module/az.resources/new-azresourcelock):

```azurepowershell-interactive
# Lock a DNS zone

$lvl = "<lock level>"
$lnm = "<lock name>"
$rsc = "<zone name>"
$rty = "Microsoft.Network/privateDnsZones"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rsc -ResourceType $rty -ResourceGroupName $rsg
```

El comando equivalente también está [disponible a través de la CLI de Azure](/cli/azure/lock#az_lock_create):

```azurecli-interactive
# Lock a DNS zone

az lock create \
--lock-type "<lock level>" \
--name "<lock name>" \
--resource-name "<zone name>" \
--namespace "Microsoft.Network" \
--resource-type "privateDnsZones" \
--resource-group "<resource group name>"
```
### <a name="protecting-individual-records"></a>Protección de registros individuales

Para evitar que se modifique un conjunto de registros de DNS existente, aplique un bloqueo ReadOnly al conjunto de registros.

> [!NOTE]
> La aplicación de un bloqueo CanNotDelete a un conjunto de registros no es un control eficaz. Evita que el conjunto de registros se elimine, pero no que se modifique.  Entre las modificaciones permitidas figuran la adición y eliminación de registros desde el conjunto de registros, incluida la eliminación de todos los registros para dejar un conjunto de registros empty. Esto tiene el mismo efecto que eliminar el conjunto de registros desde un punto de vista de la resolución DNS.

Actualmente, los bloqueos de recursos de nivel de conjunto de recursos solo pueden configurarse mediante Azure PowerShell.  No se admiten en Azure Portal ni en la CLI de Azure.

Azure PowerShell

```azurepowershell-interactive
# Lock a DNS record set

$lvl = "<lock level>"
$lnm = "<lock name>"
$rnm = "<zone name>/<record set name>"
$rty = "Microsoft.Network/privateDnsZones"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rnm -ResourceType $rty -ResourceGroupName $rsg
```
### <a name="protecting-against-zone-deletion"></a>Protección contra la eliminación de zonas

Cuando se elimina una zona en Azure DNS, se eliminan también todos los conjuntos de registros de la zona.  Esta operación no se puede deshacer. La eliminación accidental de una zona crítica tiene la posibilidad de tener un significativo impacto de negocio.  Es importante protegerse contra la eliminación accidental de zonas.

La aplicación de un bloqueo CanNotDelete a una zona impide que se elimine la zona. Los recursos secundarios heredan los bloqueos. Un bloqueo impide que se eliminen los conjuntos de registros de la zona. Como se describe en la nota anterior, no resulta eficaz puesto que todavía se pueden quitar registros de los conjuntos de registros existentes.

Como alternativa, aplique un bloqueo CanNotDelete a un conjunto de registros de la zona, como el conjunto de registros SOA. La zona no se elimina sin eliminar también los conjuntos de registros. Este bloqueo protege contra la eliminación de la zona, a la vez que permite que se modifiquen libremente los conjuntos de registros dentro de la zona. Si se realiza un intento de eliminar la zona, Azure Resource Manager detecta esta eliminación. La eliminación también eliminaría el conjunto de registros SOA; Azure Resource Manager bloquea la llamada porque SOA está bloqueada.  No se elimina ningún conjunto de registros.

El comando de PowerShell siguiente crea un bloqueo CanNotDelete contra el registro SOA de la zona especificada:

```azurepowershell-interactive
# Protect against zone delete with CanNotDelete lock on the record set

$lvl = "CanNotDelete"
$lnm = "<lock name>"
$rnm = "<zone name>/@"
$rty = "Microsoft.Network/privateDnsZones/SOA"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rnm -ResourceType $rty -ResourceGroupName $rsg
```
Otra opción para evitar la eliminación accidental de la zona es usar un rol personalizado. Este rol garantiza que las cuentas que se usan para administrar las zonas no tienen permisos de eliminación de zonas. 

Cuando sea necesario eliminar una zona, puede aplicar una eliminación de dos pasos:

 - En primer lugar, conceda permisos de eliminación de zonas.
 - En segundo lugar, conceda permisos para eliminar la zona.

El rol personalizado funciona para todas las zonas a las que se accede con esas cuentas. Las cuentas con permisos de eliminación de zonas, como el propietario de la suscripción, todavía pueden eliminar una zona por accidente.

Es posible usar ambos enfoques (bloqueos de recursos y roles personalizados) al mismo tiempo, como un método de defensa en profundidad para la protección de zonas DNS.

## <a name="next-steps"></a>Pasos siguientes

* Para más información acerca de trabajar con Azure RBAC, consulte [¿Qué es el control de acceso basado en rol de Azure (Azure RBAC)?](../role-based-access-control/overview.md)
* Para más información sobre cómo trabajar con bloqueos de recursos, vea [Bloqueo de recursos con Azure Resource Manager](../azure-resource-manager/management/lock-resources.md).
