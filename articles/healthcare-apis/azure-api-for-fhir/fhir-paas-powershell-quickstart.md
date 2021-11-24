---
title: 'Inicio rápido: Implementación de Azure API for FHIR mediante PowerShell'
description: En este inicio rápido va a aprender a implementar Azure API for FHIR mediante PowerShell.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: zxue
ms.custom: devx-track-azurepowershell, mode-api
ms.openlocfilehash: 4c0e0467d2a5381f9cb562a93c9e709f6c845101
ms.sourcegitcommit: 56235f8694cc5f88db3afcc8c27ce769ecf455b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2021
ms.locfileid: "133071810"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-powershell"></a>Inicio rápido: Implementación de Azure API for FHIR mediante PowerShell

En este inicio rápido va a aprender a implementar Azure API for FHIR mediante PowerShell.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="register-the-azure-api-for-fhir-resource-provider"></a>Registro del proveedor de recursos de Azure API for FHIR

Si el proveedor de recursos `Microsoft.HealthcareApis` no se ha registrado en la suscripción, puede registrarlo con:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

## <a name="create-azure-resource-group"></a>Creación del grupo de recursos de Azure

```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroupName" -Location westus2
```

## <a name="deploy-azure-api-for-fhir"></a>Implementación de Azure API for FHIR

```azurepowershell-interactive
New-AzHealthcareApisService -Name nameoffhirservice -ResourceGroupName myResourceGroupName -Location westus2 -Kind fhir-R4
```

> [!NOTE]
> En función del módulo `Az` de PowerShell que haya instalado, el servidor de FHIR aprovisionado puede configurarse para que use el [control de acceso basado en roles local](configure-local-rbac.md) y tenga el usuario de PowerShell con la sesión iniciada actualmente establecido en la lista de identificadores de objetos de identidad permitidos para el servicio FHIR implementado. En adelante, se recomienda [usar el control de acceso basado en roles de Azure](configure-azure-rbac.md) para asignar roles de plano de datos y es posible que tenga que eliminar este identificador de objeto de usuarios después de la implementación para habilitar el modo RBAC de Azure.


## <a name="fetch-capability-statement"></a>Captura de una instrucción de funcionalidad

Podrá comprobar que la cuenta de Azure API for FHIR se está ejecutando mediante la captura de una instrucción de funcionalidad de FHIR:

```azurepowershell-interactive
$metadata = Invoke-WebRequest -Uri "https://nameoffhirservice.azurehealthcareapis.com/metadata"
$metadata.RawContent
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, elimine el grupo de recursos mediante los siguientes pasos:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupName
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha implementado Azure API for FHIR en su suscripción. Para establecer opciones de configuración adicionales en Azure API for FHIR, continúe con la guía paso a paso de configuración adicional. Si está listo para empezar a usar Azure API for FHIR, lea más información sobre cómo registrar aplicaciones.

>[!div class="nextstepaction"]
>[Configuración adicional en Azure API for FHIR](azure-api-for-fhir-additional-settings.md)

>[!div class="nextstepaction"]
>[Introducción al registro de aplicaciones](fhir-app-registration.md)
