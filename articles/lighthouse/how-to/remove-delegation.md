---
title: Quitar el acceso a una delegación
description: Obtenga información sobre cómo quitar el acceso a los recursos que se han delegado a un proveedor de Azure Lighthouse.
ms.date: 09/08/2021
ms.topic: how-to
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 16d92c58e08e06832781ff7d5095039cedbb344f
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124736597"
---
# <a name="remove-access-to-a-delegation"></a>Quitar el acceso a una delegación

Una vez delegada la suscripción o el grupo de recursos de un cliente a un proveedor de servicios para [Azure Lighthouse](../overview.md), la delegación se puede quitar si es necesario. Una vez que se quita una delegación, el acceso a la [Administración de recursos delegados de Azure](../concepts/architecture.md) que había sido concedido previamente a los usuarios del inquilino del proveedor de servicios ya no será aplicado.

La eliminación de una delegación puede realizarla un usuario en el inquilino del cliente o el inquilino del proveedor de servicios, siempre y cuando el usuario tenga los permisos adecuados.

> [!TIP]
> Aunque en este tema hacemos referencia a los proveedores de servicios y clientes, las [empresas que administran varios inquilinos](../concepts/enterprise.md) pueden usar los mismos procesos.

## <a name="customers"></a>Clientes

Los usuarios del inquilino del cliente que tienen un rol con el permiso `Microsoft.Authorization/roleAssignments/write`, como [Propietario](../../role-based-access-control/built-in-roles.md#owner), pueden quitar el acceso del proveedor de servicios a esa suscripción (o a los grupos de recursos de esa suscripción). Para ello, el usuario puede ir a la [página Proveedores de servicios](view-manage-service-providers.md#remove-service-provider-offers) de Azure Portal, buscar la oferta en la pantalla **Ofertas del proveedor de servicios** y seleccionar el icono de la papelera en la fila de esa oferta.

Tras confirmar la eliminación, ningún usuario en el inquilino del proveedor de servicios podrá acceder a los recursos que se hayan delegado previamente.

## <a name="service-providers"></a>Proveedores de servicios

Los usuarios de un inquilino de administración pueden quitar el acceso a los recursos delegados si se les ha concedido el [rol para eliminar la asignación de registros de servicios administrados](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) para los recursos del cliente. Si este rol no se ha asignado a ningún usuario del proveedor de servicios, la delegación solo la puede quitar un usuario del inquilino del cliente.

En el ejemplo siguiente se muestra una asignación que concede el **rol para eliminar la asignación del registro de servicios administrados** que se puede incluir en un archivo de parámetros durante el [proceso de incorporación](onboard-customer.md):

```json
    "authorizations": [ 
        { 
            "principalId": "cfa7496e-a619-4a14-a740-85c5ad2063bb", 
            "principalIdDisplayName": "MSP Operators", 
            "roleDefinitionId": "91c1777a-f3dc-4fae-b103-61d183457e46" 
        } 
    ] 
```

Este rol también se puede seleccionar en una **autorización** durante la [creación de una oferta de servicio administrado](../../marketplace/plan-managed-service-offer.md) para publicar en Azure Marketplace.

Un usuario con este permiso puede quitar una delegación de una de las siguientes maneras.

### <a name="azure-portal"></a>Azure portal

1. Vaya a la página [Mis clientes](view-manage-customers.md).
2. Seleccione **Delegaciones**.
3. Busque la delegación que desea quitar y seleccione el icono de la papelera que aparece en su fila.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory 

Login-AzAccount

# Select the subscription that is delegated - or contains the delegated resource group(s)

Select-AzSubscription -SubscriptionName "<subscriptionName>"

# Get the registration assignment

Get-AzManagedServicesAssignment -Scope "/subscriptions/{delegatedSubscriptionId}"

# Delete the registration assignment

Remove-AzManagedServicesAssignment -ResourceId "/subscriptions/{delegatedSubscriptionId}/providers/Microsoft.ManagedServices/registrationAssignments/{assignmentGuid}"
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory

az login

# Select the subscription that is delegated – or contains the delegated resource group(s)

az account set -s <subscriptionId/name>

# List registration assignments

az managedservices assignment list

# Delete the registration assignment

az managedservices assignment delete --assignment <id or full resourceId>
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre la [arquitectura de Azure Lighthouse](../concepts/architecture.md).
- Puede [ver y administrar clientes](view-manage-customers.md) desde **Mis clientes**, en Azure Portal.
- Obtenga información acerca de cómo [actualizar una delegación anterior](update-delegation.md).
