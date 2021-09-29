---
title: Extensión Guest Configuration
description: Obtenga información sobre la extensión que se usa para auditar o configurar valores dentro de las máquinas virtuales.
ms.topic: article
ms.service: virtual-machines
ms.subservice: extensions
author: mgreenegit
ms.author: migreene
ms.date: 04/15/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: ea70d25bae9b30e1170046a7a7d470b4fea1a08d
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128626634"
---
# <a name="overview-of-the-guest-configuration-extension"></a>Información general de la extensión Guest Configuration

La extensión Guest Configuration es un componente de Azure Policy que realiza operaciones de auditoría y configuración dentro de las máquinas virtuales.
Las directivas, como las definiciones de línea de base de seguridad para [Linux](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffc9b3da7-8347-4380-8e70-0a0361d8dedd) y [Windows](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F72650e9f-97bc-4b2a-ab5f-9781a9fcecbc), no pueden comprobar la configuración dentro de las máquinas hasta que se instale la extensión.

## <a name="prerequisites"></a>Requisitos previos

Para que la máquina se autentique en el servicio de configuración de invitado, esta debe tener una [identidad administrada asignada por el sistema](../../active-directory/managed-identities-azure-resources/overview.md).
El requisito de la identidad en una máquina virtual se cumple si se establece la propiedad siguiente.

  ```json
  "identity": {
    "type": "SystemAssigned"
  }
  ```

### <a name="operating-systems"></a>Sistemas operativos

La compatibilidad con la extensión Guest Configuration es la misma que con el sistema operativo [documentado para la solución de un extremo a otro.](../../governance/policy/concepts/guest-configuration.md#supported-client-types)

### <a name="internet-connectivity"></a>Conectividad de Internet

El agente instalado por la extensión Guest Configuration debe poder acceder a los paquetes de contenido que enumeran las asignaciones de Guest Configuration y notificar el estado al servicio de configuración de invitado.
La máquina puede conectarse mediante HTTPS saliente a través del puerto TCP 443 o mediante una conexión a través de redes privadas.
Para obtener más información sobre las redes privadas, consulte los artículos siguientes:

- [Guest Configuration, Comunicación a través de un vínculo privado en Azure](../../governance/policy/concepts/guest-configuration.md#communicate-over-private-link-in-azure)
- [Uso de puntos de conexión privados para Azure Storage](../../storage/common/storage-private-endpoints.md)

## <a name="how-can-i-install-the-extension"></a>¿Cómo puedo instalar la extensión?

El nombre de instancia de la extensión debe establecerse en "AzurePolicyforWindows" o "AzurePolicyforLinux", ya que las directivas a las que se hace referencia anteriormente requieren estas cadenas específicas.

De forma predeterminada, todas las implementaciones se actualizan a la última versión. El valor predeterminado de la propiedad _autoUpgradeMinorVersion_ es "true", a menos que se especifique lo contrario. No es necesario preocuparse por actualizar el código cuando se publiquen nuevas versiones de la extensión.

### <a name="azure-policy"></a>Azure Policy

Para implementar la versión más reciente de la extensión a gran escala, incluidos los requisitos de identidad, [asigne](../../governance/policy/assign-policy-portal.md) la instancia de Azure Policy:

[Implemente los requisitos previos para habilitar directivas de configuración de invitado en máquinas virtuales](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Guest%20Configuration/GuestConfiguration_Prerequisites.json).

### <a name="azure-cli"></a>Azure CLI

Para implementar la extensión en Linux:


```azurecli
az vm extension set  --publisher Microsoft.GuestConfiguration --name ConfigurationforLinux --extension-instance-name AzurePolicyforLinux --resource-group myResourceGroup --vm-name myVM
```

Para implementar la extensión en Windows:

```azurecli
az vm extension set  --publisher Microsoft.GuestConfiguration --name ConfigurationforWindows --extension-instance-name AzurePolicyforWindows --resource-group myResourceGroup --vm-name myVM
```

### <a name="powershell"></a>PowerShell

Para implementar la extensión en Linux:

```powershell
Set-AzVMExtension -Publisher 'Microsoft.GuestConfiguration' -Type 'ConfigurationforLinux' -Name 'AzurePolicyforLinux' -TypeHandlerVersion 1.0 -ResourceGroupName 'myResourceGroup' -Location 'myLocation' -VMName 'myVM' -EnableAutomaticUpgrade $true
```

Para implementar la extensión en Windows:

```powershell
Set-AzVMExtension -Publisher 'Microsoft.GuestConfiguration' -Type 'ConfigurationforWindows' -Name 'AzurePolicyforWindows' -TypeHandlerVersion 1.0 -ResourceGroupName 'myResourceGroup' -Location 'myLocation' -VMName 'myVM' --enable-auto-upgrade
```

### <a name="resource-manager-template"></a>Plantilla de Resource Manager

Para implementar la extensión en Linux:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(parameters('VMName'), '/AzurePolicyforLinux')]",
  "apiVersion": "2020-12-01",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.GuestConfiguration",
    "type": "ConfigurationforLinux",
    "typeHandlerVersion": "1.0",
    "autoUpgradeMinorVersion": true,
    "enableAutomaticUpgrade": true, 
    "settings": {},
    "protectedSettings": {}
  }
}
```

Para implementar la extensión en Windows:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(parameters('VMName'), '/AzurePolicyforWindows')]",
  "apiVersion": "2020-12-01",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.GuestConfiguration",
    "type": "ConfigurationforWindows",
    "typeHandlerVersion": "1.0",
    "autoUpgradeMinorVersion": true,
    "enableAutomaticUpgrade": true, 
    "settings": {},
    "protectedSettings": {}
  }
}
```

### <a name="bicep"></a>Bicep

Para implementar la extensión en Linux:

```bicep
resource virtualMachine 'Microsoft.Compute/virtualMachines@2021-03-01' existing = {
  name: 'VMName'
}
resource windowsVMGuestConfigExtension 'Microsoft.Compute/virtualMachines/extensions@2020-12-01' = {
  parent: virtualMachine
  name: 'AzurePolicyforLinux'
  location: resourceGroup().location
  properties: {
    publisher: 'Microsoft.GuestConfiguration'
    type: 'ConfigurationforLinux'
    typeHandlerVersion: '1.0'
    autoUpgradeMinorVersion: true
    enableAutomaticUpgrade: true
    settings: {}
    protectedSettings: {}
  }
}
```

Para implementar la extensión en Windows:

```bicep
resource virtualMachine 'Microsoft.Compute/virtualMachines@2021-03-01' existing = {
  name: 'VMName'
}
resource windowsVMGuestConfigExtension 'Microsoft.Compute/virtualMachines/extensions@2020-12-01' = {
  parent: virtualMachine
  name: 'AzurePolicyforWindows'
  location: resourceGroup().location
  properties: {
    publisher: 'Microsoft.GuestConfiguration'
    type: 'ConfigurationforWindows'
    typeHandlerVersion: '1.0'
    autoUpgradeMinorVersion: true
    enableAutomaticUpgrade: true
    settings: {}
    protectedSettings: {}
  }
}
```

### <a name="terraform"></a>Terraform

Para implementar la extensión en Linux:

```terraform
resource "azurerm_virtual_machine_extension" "gc" {
  name                       = "AzurePolicyforLinux"
  virtual_machine_id         = "myVMID"
  publisher                  = "Microsoft.GuestConfiguration"
  type                       = "ConfigurationforLinux"
  type_handler_version       = "1.0"
  auto_upgrade_minor_version = "true"
}
```

Para implementar la extensión en Windows:

```terraform
resource "azurerm_virtual_machine_extension" "gc" {
  name                       = "AzurePolicyforWindows"
  virtual_machine_id         = "myVMID"
  publisher                  = "Microsoft.GuestConfiguration"
  type                       = "ConfigurationforWindows"
  type_handler_version       = "1.0"
  auto_upgrade_minor_version = "true"
}
```

## <a name="settings"></a>Configuración

No es necesario incluir ninguna configuración ni propiedades de configuración protegida en la extensión.
El agente recupera toda esta información de los recursos de [asignación de configuración de invitado](/rest/api/guestconfiguration/guestconfigurationassignments). Por ejemplo, las propiedades [ConfigurationUri](/rest/api/guestconfiguration/guestconfigurationassignments/createorupdate#guestconfigurationnavigation), [Mode](/rest/api/guestconfiguration/guestconfigurationassignments/createorupdate#configurationmode) y [ConfigurationSetting](/rest/api/guestconfiguration/guestconfigurationassignments/createorupdate#configurationsetting) se administran por configuración, en lugar de en la extensión de VM.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre Guest Configuration en Azure Policy, consulte [Información sobre Guest Configuration de Azure Policy](../../governance/policy/concepts/guest-configuration.md).
* Para obtener más información sobre cómo funcionan las extensiones y el agente de Linux, vea [Características y extensiones de VM de Azure para Linux](features-linux.md).
* Para más información sobre cómo funcionan las extensiones de Windows y Windows Guest Agent, consulte [Características y extensiones de VM de Azure para Windows](features-windows.md).  
* Para instalar Windows Guest Agent, vea [Información general del agente de máquina virtual de Azure](agent-windows.md).  
* Para instalar el agente de Linux, vea [Información y uso del agente de Linux de Azure](agent-linux.md).  
