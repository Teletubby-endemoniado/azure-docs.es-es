---
title: Creación de una máquina virtual Windows a partir de una plantilla en Azure
description: Use una plantilla de Resource Manager y PowerShell para crear fácilmente una nueva máquina virtual de Windows.
author: cynthn
ms.service: virtual-machines
ms.topic: how-to
ms.date: 03/22/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017, devx-track-azurepowershell
ms.openlocfilehash: 1be03bee5005cbcc4b2eec3d469631d504d3c504
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122690158"
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Creación de una máquina virtual Windows con una plantilla de Resource Manager

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Windows 

Aprenda a crear una máquina virtual Windows mediante una plantilla de Azure Resource Manager y Azure PowerShell desde Azure Cloud Shell. La plantilla usada en este artículo implementa una sola máquina virtual que ejecuta Windows Server en una nueva red virtual con una sola subred. Para la creación de una máquina virtual Linux, consulte [Procedimiento para crear una máquina virtual Linux con plantillas de Azure Resource Manager](../linux/create-ssh-secured-vm-from-template.md).

Una alternativa consiste en implementar la plantilla desde Azure Portal. Para abrir la plantilla en Azure Portal, haga clic en el botón **Implementar en Azure**.

[![Implementación en Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.compute%2Fvm-simple-windows%2Fazuredeploy.json)

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

La creación de una máquina virtual de Azure normalmente incluye dos pasos:

- Cree un grupo de recursos. Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Se debe crear un grupo de recursos antes de una máquina virtual.
- Cree una máquina virtual.

En el siguiente ejemplo se crea una máquina virtual a partir de una [plantilla de inicio rápido de Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.compute/vm-simple-windows/azuredeploy.json). Esta es una copia de la plantilla:

[!code-json[create-windows-vm](~/quickstart-templates/quickstarts/microsoft.compute/vm-simple-windows/azuredeploy.json)]

Para ejecutar el script de PowerShell, seleccione **Pruébelo** para abrir el shell de Azure Cloud. Para pegar el script, haga clic con el botón derecho en el shell y luego seleccione **Pegar**:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the administrator username"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter an unique DNS name for the public IP"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.compute/vm-simple-windows/azuredeploy.json" `
    -adminUsername $adminUsername `
    -adminPassword $adminPassword `
    -dnsLabelPrefix $dnsLabelPrefix

 (Get-AzVm -ResourceGroupName $resourceGroupName).name

```

Si decide instalar y usar PowerShell de forma local en lugar de en el shell de Azure Cloud, en este tutorial necesitará el módulo de Azure PowerShell. Ejecute `Get-Module -ListAvailable Az` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

En el ejemplo anterior, especificó una plantilla almacenada en GitHub. También puede descargar o crear una plantilla y especificar la ruta de acceso local con el parámetro `--template-file`.

Estos son algunos recursos adicionales:

- Para aprender a desarrollar plantillas de Resource Manager, consulte la [documentación de Azure Resource Manager](../../azure-resource-manager/index.yml).
- Para ver los esquemas de la máquina virtual de Azure, consulte [Referencia sobre las plantillas de Azure](/azure/templates/microsoft.compute/allversions).
- Para ver más ejemplos de plantilla de máquina virtual, consulte [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

## <a name="connect-to-the-virtual-machine"></a>Conexión a la máquina virtual

El último comando de PowerShell del script anterior muestra el nombre de la máquina virtual. Para conectarse a la máquina virtual, consulte [Conexión a una máquina virtual de Azure donde se ejecuta Windows e inicio de sesión en esta](./connect-logon.md).

## <a name="next-steps"></a>Pasos siguientes

- Si se produjeron problemas con la implementación, consulte [Solución de errores comunes de implementación de Azure con Azure Resource Manager](../../azure-resource-manager/templates/common-deployment-errors.md).
- Aprenda a crear y administrar una máquina virtual en [Creación y administración de máquinas virtuales Windows con el módulo de Azure PowerShell](tutorial-manage-vm.md).

Para más información sobre cómo crear plantillas, vea las propiedades y la sintaxis de JSON para los tipos de recursos que ha implementado:

- [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
