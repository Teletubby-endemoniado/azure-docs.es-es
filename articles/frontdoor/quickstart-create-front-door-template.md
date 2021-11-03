---
title: 'Inicio rápido: Creación de una instancia de Azure Front Door Service mediante una plantilla de Azure Resource Manager (plantilla de ARM)'
description: En esta guía de inicio rápido se describe cómo usar una plantilla de Azure Resource Manager (plantilla de ARM) para crear una instancia de Azure Front Door Service.
services: front-door
documentationcenter: ''
author: duongau
ms.author: duau
editor: ''
ms.date: 09/14/2020
ms.topic: quickstart
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: devx-track-azurepowershell, subject-armqs, mode-arm
ms.openlocfilehash: 6e2b5137096e407a86011ed0ee08ed7bf45c4f21
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131073099"
---
# <a name="quickstart-create-a-front-door-using-an-arm-template"></a>Inicio rápido: Creación de una instancia de Front Door mediante una plantilla de ARM

En esta guía de inicio rápido se describe cómo usar una plantilla de Azure Resource Manager (plantilla de ARM) para crear una instancia de Front Door para configurar la alta disponibilidad para un punto de conexión web.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Si su entorno cumple los requisitos previos y está familiarizado con el uso de plantillas de Resource Manager, seleccione el botón **Implementar en Azure**. La plantilla se abrirá en Azure Portal.

[![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.network%2Ffront-door-create-basic%2Fazuredeploy.json)

## <a name="prerequisites"></a>Prerrequisitos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
* Dirección IP o FQDN de un sitio web o una aplicación web.

## <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/front-door-create-basic).

En este inicio rápido, creará una configuración de Front Door con un solo back-end y una única ruta de acceso predeterminada que coincide con `/*`.

:::code language="json" source="~/quickstart-templates/quickstarts/microsoft.network/front-door-create-basic/azuredeploy.json":::

En la plantilla, se define un recurso de Azure:

* [**Microsoft.Network/frontDoors**](/azure/templates/microsoft.network/frontDoors)

## <a name="deploy-the-template"></a>Implementación de la plantilla

1. Seleccione **Try It** (Probarlo) en el bloque de código siguiente para abrir Azure Cloud Shell y siga las instrucciones para iniciar sesión en Azure.

    ```azurepowershell-interactive
    $projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.network/front-door-create-basic/azuredeploy.json"

    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

    Read-Host -Prompt "Press [ENTER] to continue ..."
    ```

    Espere hasta que vea el aviso de la consola.

1. Seleccione **Copiar** en el bloque de código anterior para copiar el script de PowerShell.

1. Haga clic con el botón derecho en el panel de consola del shell y, a continuación, seleccione **Pegar**.

1. Escriba los valores.

    La implementación de la plantilla crea una instancia de Front Door con un solo back-end. En este ejemplo `microsoft.com` se usa como valor de **backendAddress**.

    El nombre del grupo de recursos es el nombre del proyecto con **rg** anexado.

    > [!NOTE]
    > **frontDoorName** debe ser un nombre globalmente único para que la plantilla se implemente correctamente. Si se produce un error en la implementación, vuelva al paso 1.

    La plantilla tarda unos minutos en implementarse. Al finalizar, la salida es parecida a esta:

    :::image type="content" source="./media/quickstart-create-front-door-template/front-door-template-deployment-powershell-output.png" alt-text="Salida de la implementación de PowerShell de la plantilla de Resource Manager de Front Door":::

Azure PowerShell se usa para implementar la plantilla. Además de Azure PowerShell, también puede usar Azure Portal, la CLI de Azure y API REST. Para obtener información sobre otros métodos de implementación, consulte [Implementación de plantillas](../azure-resource-manager/templates/deploy-portal.md).

## <a name="validate-the-deployment"></a>Validación de la implementación

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Grupos de recursos** en el panel izquierdo.

1. Seleccione el grupo de recursos que creó en la sección anterior. El nombre del grupo de recursos predeterminado es el nombre del proyecto con **rg** anexado.

1. Seleccione la instancia de Front Door que creó previamente y haga clic en el vínculo **Host de front-end**. El vínculo abrirá un explorador web que le redirigirá al FQDN de back-end que definió durante la creación.

    :::image type="content" source="./media/quickstart-create-front-door-template/front-door-overview.png" alt-text="Información general del portal de Front Door":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite la instancia de Front Door Service, elimine el grupo de recursos. Esta acción elimina la instancia de Front Door y todos los recursos relacionados.

Para eliminar el grupo de recursos, llame al cmdlet `Remove-AzResourceGroup`:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado una instancia de Front Door.

Para obtener información sobre cómo agregar un dominio personalizado a Front Door, continúe con los tutoriales de Front Door.

> [!div class="nextstepaction"]
> [Tutoriales de Front Door](front-door-custom-domain.md)
