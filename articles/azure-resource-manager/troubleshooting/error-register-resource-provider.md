---
title: Errores de registro del proveedor de recursos
description: Describe cómo resolver errores de registro del proveedor de recursos al implementar recursos con Azure Resource Manager.
ms.topic: troubleshooting
ms.date: 02/15/2019
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: f3e83fb460be9d62755e96e7a79f5f3b0f058e88
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131091445"
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Resolución de errores del registro del proveedor de recursos

En este artículo se describen los errores que pueden producirse al usar un proveedor de recursos que no ha usado anteriormente en la suscripción.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>Síntoma

Al implementar recursos, puede recibir el siguiente código y mensaje de error:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

O bien, puede recibir un mensaje similar que indica:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

El mensaje de error debería proporcionarle sugerencias con respecto a las versiones de API y a las ubicaciones admitidas. Puede cambiar la plantilla a uno de los valores sugeridos. La mayoría de los proveedores se registran automáticamente mediante Azure Portal o la interfaz de línea de comandos que esté usando; pero no ocurre con todos. Si no ha usado un proveedor de recursos determinado antes, debe registrar dicho proveedor.

O bien, al deshabilitar el apagado automático para máquinas virtuales, es posible que reciba un mensaje de error similar al siguiente:

```
Code: AuthorizationFailed
Message: The client '<identifier>' with object id '<identifier>' does not have authorization to perform action 'Microsoft.Compute/virtualMachines/read' over scope ...
```

## <a name="cause"></a>Causa

Recibirá estos errores por uno de estos motivos:

* El proveedor de recursos necesario no se ha registrado para la suscripción
* No se permite esta versión de API para el tipo de recurso
* No se permite esta ubicación para el tipo de recurso
* Para el apagado automático de máquinas virtuales, debe registrarse el proveedor de recursos Microsoft.DevTestLab.

## <a name="solution-1---powershell"></a>Solución 1: PowerShell

En PowerShell, utilice **Get-AzResourceProvider** para el estado del registro.

```powershell
Get-AzResourceProvider -ListAvailable
```

Para registrar un proveedor, use **Register-AzResourceProvider** e indique el nombre del proveedor de recursos que desea registrar.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Cdn
```

Para conocer las ubicaciones admitidas para un tipo determinado de recurso, use:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Para conocer las versiones de API admitidas para un tipo determinado de recurso, use:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

## <a name="solution-2---azure-cli"></a>Solución 2: CLI de Azure

Para ver si el proveedor está registrado, utilice el comando `az provider list` .

```azurecli-interactive
az provider list
```

Para registrar un proveedor de recursos, use el comando `az provider register` y especifique el *espacio de nombres* que desea registrar.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

Para ver las ubicaciones y las versiones de API admitidas para un tipo de recursos, use:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="solution-3---azure-portal"></a>Solución 3: Azure Portal

Puede ver el estado de registro y registrar un espacio de nombres de proveedor de recursos a través del portal.

1. En Azure Portal, seleccione **Todos los servicios**.

   ![Seleccionar todos los servicios](./media/error-register-resource-provider/select-all-services.png)

1. Seleccione **Suscripciones**.

   ![Selección de suscripciones](./media/error-register-resource-provider/select-subscriptions.png)

1. En la lista de suscripciones, seleccione la suscripción que quiere usar para registrar el proveedor de recursos.

   ![Selección de la suscripción para registrar el proveedor de recursos](./media/error-register-resource-provider/select-subscription-to-register.png)

1. Para la suscripción, seleccione **Proveedores de recursos**.

   ![Selección de proveedores de recursos](./media/error-register-resource-provider/select-resource-provider.png)

1. Examine la lista de proveedores de recursos y, si es necesario, seleccione el vínculo **Registrar** para registrar el proveedor de recursos del tipo que está intentando implementar.

   ![Enumeración de proveedores de recursos](./media/error-register-resource-provider/list-resource-providers.png)
