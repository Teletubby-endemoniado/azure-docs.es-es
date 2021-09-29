---
title: Actualización automática de extensiones para máquinas virtuales y conjuntos de escalado en Azure
description: Aprenda a habilitar la actualización automática de extensiones para las máquinas virtuales y los conjuntos de escalado de máquinas virtuales de Azure.
author: mayanknayar
ms.service: virtual-machines
ms.subservice: extensions
ms.workload: infrastructure
ms.topic: how-to
ms.date: 08/10/2021
ms.author: manayar
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: a7368ee52808dc56e3532f2edfb41e00c7737a7e
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129216479"
---
# <a name="automatic-extension-upgrade-for-vms-and-scale-sets-in-azure"></a>Actualización automática de extensiones para máquinas virtuales y conjuntos de escalado en Azure

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

La actualización automática de extensiones está disponible para las máquinas virtuales de Azure y Azure Virtual Machine Scale Sets. Cuando la actualización automática de extensiones está habilitada en una máquina virtual o en un conjunto de escalado, la extensión se actualiza automáticamente cada vez que el editor de la extensión publica una nueva versión de esta.

 La actualización automática de extensiones tiene las siguientes características:
- Es compatible con máquinas virtuales de Azure y Azure Virtual Machine Scale Sets.
- Las actualizaciones se aplican según un modelo de implementación de orden de disponibilidad (se detalla a continuación).
- En el caso de un conjunto de escalado de máquinas virtuales, no se actualizará más del 20 % de las máquinas virtuales del conjunto de escalado en un único lote. El tamaño mínimo de lote es una única máquina virtual.
- Funciona con todos los tamaños de máquina virtual y con extensiones de Windows y Linux.
- Puede optar a las actualizaciones automáticas en cualquier momento.
- La actualización automática de extensiones se puede habilitar en una instancia de Virtual Machine Scale Sets de cualquier tamaño.
- Cada extensión admitida se inscribe individualmente, y puede elegir qué extensiones se van a actualizar automáticamente.
- Se admite en todas las regiones de la nube pública.

## <a name="how-does-automatic-extension-upgrade-work"></a>¿Cómo funciona la actualización automática de extensiones?
El proceso de actualización de extensiones reemplaza la versión de la extensión existente en una máquina virtual por una nueva versión de la misma extensión que ha publicado el editor de la extensión. El estado de la máquina virtual se supervisa después de instalar la nueva extensión. Si el estado de la máquina virtual no es correcto a los cinco minutos de finalizar la actualización, la versión de la extensión se revierte a la versión anterior.

Las actualizaciones de extensiones con errores se reintentan automáticamente. Los reintentos se realizan cada pocos días automáticamente sin la intervención del usuario.

### <a name="availability-first-updates"></a>Actualizaciones por orden de disponibilidad
El modelo de orden de disponibilidad de las actualizaciones orquestadas de la plataforma garantiza que se respeten las configuraciones de disponibilidad en Azure de varios niveles.

En un grupo de máquinas virtuales que se vayan a actualizar, la plataforma Azure orquestará las actualizaciones:

**Entre regiones:**
- una actualización se moverá en Azure globalmente por fases para evitar errores de implementación en esta plataforma.
- Una "fase" puede tener una o más regiones, y una actualización solo cambia de fase si las máquinas virtuales válidas de la fase anterior se actualizan correctamente.
- Las regiones emparejadas geográficamente no se actualizan simultáneamente y no pueden estar en la misma fase regional.
- El éxito de una actualización se mide realizando un seguimiento del estado de la máquina virtual después de la actualización. El seguimiento del estado de la máquina virtual se realiza a través de los indicadores de estado de la plataforma de la máquina virtual. En el caso de Virtual Machine Scale Sets, se realiza un seguimiento del estado de la máquina virtual a través de sondeos de estado de la aplicación o de la extensión Estado de la aplicación, si se aplica al conjunto de escalado.

**Dentro de una región:**
- Las máquinas virtuales de diferentes Availability Zones no se actualizan simultáneamente con la misma actualización.
- Las máquinas virtuales que no forman parte de un conjunto de disponibilidad se procesan por lotes, según la mejor opción, para evitar las actualizaciones simultáneas de todas las máquinas virtuales de una suscripción.  

**Dentro de un "conjunto":**
- todas las máquinas virtuales de un conjunto de disponibilidad o de un conjunto de escalado común no se actualizan de forma simultánea.  
- Las máquinas virtuales de un conjunto de disponibilidad común se actualizan dentro de los límites del dominio de actualización y las máquinas virtuales de varios dominios de actualización no se actualizan simultáneamente.  
- Las máquinas virtuales de un conjunto de escalado de máquinas virtuales común se agrupan en lotes y se actualizan en los límites del dominio de actualización.

### <a name="upgrade-process-for-virtual-machine-scale-sets"></a>Proceso de actualización de Virtual Machine Scale Sets
1. Antes de comenzar el proceso de actualización, el orquestador se asegurará de que no haya más del 20 % de las máquinas virtuales de todo el conjunto de escalado en mal estado (por cualquier motivo).

2. El orquestrador de actualización identifica el lote de instancias de máquina virtual que se va a actualizar. Un lote de actualización puede tener como máximo el 20 % del número total de máquinas virtuales (sujeto a un tamaño de lote mínimo de una máquina virtual).

3. En conjuntos de escalado que tienen configurados sondeos de estado de la aplicación o la extensión Estado de la aplicación, la actualización espera hasta cinco minutos (o la configuración de sondeo de estado definida) a que la máquina virtual tenga un estado correcto antes de actualizar el siguiente lote. Si una máquina virtual no recupera su estado después de una actualización, de forma predeterminada se vuelve a instalar la versión anterior de la extensión de la máquina virtual.

4. El orquestador de la actualización también realiza un seguimiento del porcentaje de máquinas virtuales que tienen un estado incorrecto después de una actualización. La actualización se detendrá si más del 20 % de las instancias actualizadas pasan a tener un estado incorrecto durante el proceso de actualización.

El proceso anterior continúa hasta que se han actualizado todas las instancias del conjunto de escalado.

El orquestador de la actualización del conjunto de escalado comprueba el estado global del conjunto de escalado antes de actualizar cada lote. Al actualizar un lote, podría haber otras actividades de mantenimiento simultáneas planeadas o sin planear que podrían afectar al estado de las máquinas virtuales del conjunto de escalado. En tales casos, si más del 20 % de las instancias del conjunto de escalado tienen un estado incorrecto, la actualización del conjunto de escalado se detiene al final del lote actual.

## <a name="supported-extensions"></a>Extensiones admitidas
La actualización automática de extensiones admite las siguientes extensiones (y se agregan más periódicamente):
- Dependency Agent: [Linux](./extensions/agent-dependency-linux.md) y [Windows](./extensions/agent-dependency-windows.md)
- [Extensión Application Health](../virtual-machine-scale-sets/virtual-machine-scale-sets-health-extension.md): Linux y Windows
- [Extensión Guest Configuration](./extensions/guest-configuration.md): Linux y Windows
- Key Vault: [Linux](./extensions/key-vault-linux.md) y [Windows](./extensions/key-vault-windows.md)


## <a name="enabling-automatic-extension-upgrade"></a>Habilitación de la actualización automática de extensiones

Para habilitar la actualización automática de extensiones para una extensión, debe asegurarse de que la propiedad `enableAutomaticUpgrade` esté establecida en `true` y de que se haya agregado a cada definición de extensión de manera individual.

### <a name="rest-api-for-virtual-machines"></a>API REST para Virtual Machines
Para habilitar la actualización automática de extensiones para una extensión (en este ejemplo, la extensión Dependency Agent) en una máquina virtual de Azure, use lo siguiente:

```
PUT on `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/virtualMachines/<vmName>/extensions/<extensionName>?api-version=2019-12-01`
```

```json
{    
    "name": "extensionName",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "location": "<location>",
    "properties": {
        "autoUpgradeMinorVersion": true,
        "enableAutomaticUpgrade": true, 
        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
        "type": "DependencyAgentWindows",
        "typeHandlerVersion": "9.5"
        }
}
```

### <a name="rest-api-for-virtual-machine-scale-sets"></a>API REST para Virtual Machine Scale Sets
Use los pasos siguientes para agregar la extensión al modelo de conjunto de escalado:

```
PUT on `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/virtualMachineScaleSets/<vmssName>?api-version=2019-12-01`
```

```json
{
   "location": "<location>",
   "properties": {
        "virtualMachineProfile": {
            "extensionProfile": {
                "extensions": [
                {
                "name": "<extensionName>",
                  "properties": {
                        "autoUpgradeMinorVersion": true,
                        "enableAutomaticUpgrade": true,
                    "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
                    "type": "DependencyAgentWindows",
                    "typeHandlerVersion": "9.5"
                    }
                }
                ]
            }
        }
    }
}
```

### <a name="azure-powershell-for-virtual-machines"></a>Azure PowerShell para Virtual Machines
Use el cmdlet [Set-AzVMExtension](/powershell/module/az.compute/set-azvmextension):

```azurepowershell-interactive
Set-AzVMExtension -ExtensionName "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Monitoring.DependencyAgent" `
    -ExtensionType "DependencyAgentWindows" `
    -TypeHandlerVersion 9.5 `
    -Location WestUS `
    -EnableAutomaticUpgrade $true
```


### <a name="azure-powershell-for-virtual-machine-scale-sets"></a>Azure PowerShell para Virtual Machine Scale Sets
Use el cmdlet [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) para agregar la extensión al modelo de conjunto de escalado:

```azurepowershell-interactive
Add-AzVmssExtension -VirtualMachineScaleSet $vmss
    -Name "Microsoft.Azure.Monitoring.DependencyAgent" `
    -Publisher "Microsoft.Azure.Monitoring.DependencyAgent" `
    -Type "DependencyAgentWindows" `
    -TypeHandlerVersion 9.5 `
    -EnableAutomaticUpgrade $true
```

Después de agregar la extensión, actualice el conjunto de escalado mediante [Update-AzVmss](/powershell/module/az.compute/update-azvmss).


### <a name="azure-cli-for-virtual-machines"></a>CLI de Azure para Virtual Machines
Use el cmdlet [az vm extension set](/cli/azure/vm/extension#az_vm_extension_set):

```azurecli-interactive
az vm extension set \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name DependencyAgentLinux \
    --publisher Microsoft.Azure.Monitoring.DependencyAgent \
    --version 9.5 \
    --enable-auto-upgrade true
```

### <a name="azure-cli-for-virtual-machine-scale-sets"></a>CLI de Azure para Virtual Machine Scale Sets
Use el cmdlet [az vmss extension set](/cli/azure/vmss/extension#az_vmss_extension_set) para agregar la extensión al modelo del conjunto de escalado:

```azurecli-interactive
az vmss extension set \
    --resource-group myResourceGroup \
    --vmss-name myVMSS \
    --name DependencyAgentLinux \
    --publisher Microsoft.Azure.Monitoring.DependencyAgent \
    --version 9.5 \
    --enable-auto-upgrade true
```

## <a name="extension-upgrades-with-multiple-extensions"></a>Actualizaciones de extensiones con varias extensiones

Una máquina virtual o un conjunto de escalado de máquinas virtuales pueden tener varias extensiones y tener habilitada la actualización automática de extensiones. La misma máquina virtual o conjunto de escalado también puede tener otras extensiones sin tener habilitada la actualización automática de extensiones.  

Si hay varias actualizaciones de extensiones disponibles para una máquina virtual, es posible que se procesen juntas por lotes, pero cada una se aplica individualmente en una máquina virtual. Un error en una extensión no afecta a las otras extensiones que puedan estar actualizándose. Por ejemplo, si hay dos extensiones programadas para una actualización y se produce un error en la actualización de la primera, la actualización de la segunda extensión sigue su curso.

También se pueden aplicar actualizaciones automáticas de extensiones cuando una máquina virtual o un conjunto de escalado de máquinas virtuales tiene varias extensiones configuradas con la [secuenciación de extensiones](../virtual-machine-scale-sets/virtual-machine-scale-sets-extension-sequencing.md). La secuenciación de extensiones es aplicable a la primera implementación de la máquina virtual, y las posteriores actualizaciones de una extensión se aplican de forma independiente.


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Más información sobre la extensión Estado de la aplicación](../virtual-machine-scale-sets/virtual-machine-scale-sets-health-extension.md)
