---
title: 'Inicio rápido: Creación de un perfil y un punto de conexión: plantilla de Azure Resource Manager'
titleSuffix: Azure Content Delivery Network
description: En esta guía de inicio rápido aprenderá a crear un perfil y un punto de conexión de Azure Content Delivery Network con una plantilla de Resource Manager.
services: cdn
author: duongau
manager: KumudD
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: subject-armqs, devx-track-azurepowershell
ms.date: 05/10/2021
ms.author: duau
ms.openlocfilehash: d0f160a068e8f9205a9659e34de6033b6690f520
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131426361"
---
# <a name="quickstart-create-an-azure-cdn-profile-and-endpoint---arm-template"></a>Inicio rápido: Creación de un perfil y un punto de conexión de Azure CDN: plantilla de ARM

Introducción a Azure Content Delivery Network (CDN) mediante una plantilla de Azure Resource Manager (plantilla de ARM). La plantilla implementa un perfil y un punto de conexión.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Si su entorno cumple los requisitos previos y está familiarizado con el uso de plantillas de Resource Manager, seleccione el botón **Implementar en Azure**. La plantilla se abrirá en Azure Portal.

[![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.cdn%2Fcdn-with-custom-origin%2Fazuredeploy.json)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/cdn-with-custom-origin/).

Esta plantilla está configurada para crear lo siguiente:

* Perfil
* Punto de conexión

:::code language="json" source="~/quickstart-templates/quickstarts/microsoft.cdn/cdn-with-custom-origin/azuredeploy.json":::

En la plantilla, se define un recurso de Azure:

* **[Microsoft.Cdn/profiles](/azure/templates/microsoft.cdn/profiles)**

## <a name="deploy-the-template"></a>Implementación de la plantilla

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
read -p "Enter the location (i.e. eastus): " location
resourceGroupName="myResourceGroupCDN"
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.cdn/cdn-with-custom-origin/azuredeploy.json"

az group create \
--name $resourceGroupName \
--location $location

az deployment group create \
--resource-group $resourceGroupName \
--template-uri  $templateUri
```

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
$location = Read-Host -Prompt "Enter the location (i.e. eastus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.cdn/cdn-with-custom-origin/azuredeploy.json"

$resourceGroupName = "myResourceGroupCDN"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri
```

### <a name="portal"></a>Portal

[![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.cdn%2Fcdn-with-custom-origin%2Fazuredeploy.json)

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. Seleccione **Grupos de recursos** en el panel izquierdo.

3. Seleccione el grupo de recursos que creó en la sección anterior. El nombre predeterminado del grupo de recursos es **myResourceGroupNAT**.

4. Compruebe que los recursos siguientes se han creado en el grupo de recursos:

    :::image type="content" source="media/create-profile-endpoint-template/cdn-profile-template-rg.png" alt-text="Grupo de recursos de Azure CDN" border="true":::

## <a name="clean-up-resources"></a>Limpieza de recursos

### <a name="azure-cli"></a>Azure CLI

Cuando ya no lo necesite, puede usar el comando [az group delete](/cli/azure/group#az_group_delete) para quitar el grupo de recursos y todos los recursos que contiene.

```azurecli-interactive
  az group delete \
    --name myResourceGroupCDN
```

### <a name="powershell"></a>PowerShell

Cuando ya no lo necesite, puede usar el comando [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupCDN
```

### <a name="portal"></a>Portal

Cuando ya no los necesite, elimine el grupo de recursos, el perfil de red de CDN y todos los recursos relacionados. Seleccione el grupo de recursos **myResourceGroupCDN** que contiene el perfil y el punto de conexión de la red CDN y, después, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado lo siguiente:

* Perfil de CDN
* Punto de conexión

Para más información sobre Azure CDN y Azure Resource Manager, consulte los siguientes artículos.

> [!div class="nextstepaction"]
> [Tutorial: Uso de la red CDN para el contenido estático del servidor desde una aplicación web](cdn-add-to-web-app.md)
