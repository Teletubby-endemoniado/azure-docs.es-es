---
title: 'Extensión de controlador de GPU NVIDIA: máquinas virtuales Windows de Azure'
description: Extensión de Microsoft Azure para instalar controladores de GPU de NVIDIA en VM de proceso de la serie N que ejecutan Windows.
services: virtual-machines
documentationcenter: ''
author: vermagit
ms.service: virtual-machines
ms.subservice: hpc
ms.collection: windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/14/2021
ms.author: amverma
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 8e164fad73fd04ea2f9093e99b454f43aa441088
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130038315"
---
# <a name="nvidia-gpu-driver-extension-for-windows"></a>Extensión del controlador de GPU de NVIDIA para Windows

## <a name="overview"></a>Información general

Esta extensión instala los controladores de GPU de NVIDIA en VM de la serie N de Windows. En función de la familia de VM, la extensión instala los controladores CUDA o GRID. Al instalar controladores de NVIDIA mediante esta extensión, acepta y está de acuerdo con los términos del [contrato de licencia de usuario final de NVIDIA](https://go.microsoft.com/fwlink/?linkid=874330). Durante el proceso de instalación, es posible que la máquina virtual se reinicie para completar la instalación del controlador.

Hay instrucciones disponibles sobre la instalación manual de los controladores y las versiones que son compatibles actualmente. Para más información, consulte [Instalación de controladores de GPU de la serie N de NVIDIA de Azure para Windows](../windows/n-series-driver-setup.md).
También hay disponible una extensión para instalar controladores de GPU de NVIDIA en [VM de la serie N para Linux](hpccompute-gpu-linux.md).

## <a name="prerequisites"></a>Requisitos previos

### <a name="operating-system"></a>Sistema operativo

Esta extensión es compatible con los siguientes sistemas operativos:

| Distribución | Versión |
|---|---|
| Windows 10 | Core |
| Windows Server 2019 | Core |
| Windows Server 2016 | Core |
| Windows Server 2012 R2 | Core |

### <a name="internet-connectivity"></a>Conectividad de Internet

La extensión de Microsoft Azure para los controladores de GPU de NVIDIA requiere que la máquina virtual de destino esté conectada a Internet y tenga acceso.

## <a name="extension-schema"></a>Esquema de extensión

En el siguiente JSON, se muestra el esquema para la extensión.

```json
{
  "name": "<myExtensionName>",
  "type": "extensions",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <myVM>)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverWindows",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="properties"></a>Propiedades

| Nombre | Valor / ejemplo | Tipo de datos |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.HpcCompute | string |
| type | NvidiaGpuDriverWindows | string |
| typeHandlerVersion | 1.4 | int |


## <a name="deployment"></a>Implementación

### <a name="azure-portal"></a>Portal de Azure

Puede implementar extensiones de máquina virtual Nvidia de Azure en Azure Portal.

1. En el explorador, vaya al [Portal de Azure](https://portal.azure.com).

2. Vaya a la máquina virtual en la que desea instalar el controlador.

3. En el menú izquierdo, seleccione **Extensiones**.

    :::image type="content" source="./media/nvidia-ext-portal/extensions-menu.png" alt-text="Captura de pantalla que muestra la selección de Extensiones en el menú de Azure Portal.":::

4. Seleccione **Agregar**.

    :::image type="content" source="./media/nvidia-ext-portal/add-extension.png" alt-text="Captura de pantalla que muestra cómo agregar una extensión de máquina virtual para la instancia de máquina virtual seleccionada.":::

5. Desplácese para buscar y seleccionar **Extensión del controlador de GPU de NVIDIA** y, a continuación, seleccione **Siguiente**.

    :::image type="content" source="./media/nvidia-ext-portal/select-nvidia-extension.png" alt-text="Captura de pantalla que muestra la selección del controlador de GPU de NVIDIA.":::

6. Seleccione **Revisar y crear** y espere unos minutos a que se implemente el controlador.

    :::image type="content" source="./media/nvidia-ext-portal/create-nvidia-extension.png" alt-text="Captura de pantalla que muestra la selección del botón Revisar y crear.":::
  
7. Compruebe que la extensión se ha agregado a la lista de extensiones instaladas.

    :::image type="content" source="./media/nvidia-ext-portal/verify-extension.png" alt-text="Captura de pantalla que muestra la nueva extensión en la lista de extensiones para la máquina virtual.":::

### <a name="azure-resource-manager-template"></a>Plantilla de Azure Resource Manager 

Las extensiones de VM de Azure pueden implementarse con plantillas de Azure Resource Manager. Las plantillas resultan ideales al implementar una o varias máquinas virtuales que requieren configurarse tras la implementación.

La configuración JSON de una extensión de máquina virtual puede estar anidada en el recurso de máquina virtual o colocada en la raíz o nivel superior de una plantilla JSON de Resource Manager. La colocación de la configuración JSON afecta al valor del nombre y tipo del recurso. Para obtener más información, consulte el artículo sobre cómo [establecer el nombre y el tipo de recursos secundarios](../../azure-resource-manager/templates/child-resource-name-type.md). 

En el siguiente ejemplo se da por supuesto que la extensión está anidada dentro del recurso de máquina virtual. Cuando se anidan los recursos de extensión, la plantilla JSON se coloca en el objeto `"resources": []` de la máquina virtual.

```json
{
  "name": "myExtensionName",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', myVM)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverWindows",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverWindows" `
    -ExtensionType "NvidiaGpuDriverWindows" `
    -TypeHandlerVersion 1.4 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name NvidiaGpuDriverWindows \
  --publisher Microsoft.HpcCompute \
  --version 1.4 \
  --settings '{ \
  }'
```

## <a name="troubleshoot-and-support"></a>Solución de problemas y asistencia

### <a name="troubleshoot"></a>Solución de problemas

Los datos sobre el estado de las implementaciones de extensiones pueden recuperarse desde Azure Portal y mediante Azure PowerShell y la CLI de Azure. Para ver el estado de implementación de las extensiones de una VM determinada, ejecute el comando siguiente.

```powershell
Get-AzVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

El resultado de la ejecución de las extensiones se registra en el directorio siguiente:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.HpcCompute.NvidiaGpuDriverWindows\
```

### <a name="error-codes"></a>Códigos de error

| Código de error | Significado | Acción posible |
| :---: | --- | --- |
| 0 | Operación correcta |
| 1 | Operación correcta. Se requiere reiniciar. |
| 100 | La operación no es compatible o no se pudo completar. | Causas posibles: No se admite la versión de PowerShell, el tamaño de la VM no corresponde con una VM de la serie N, no se pudieron descargar los datos. Compruebe los archivos de registro para determinar la causa del error. |
| 240, 840 | Tiempo de espera de la operación. | Reintentar operación. |
| -1 | Se produjo una excepción. | Comprobar los archivos de registro para determinar la causa de la excepción. |
| -5x | La operación se interrumpió debido a un reinicio pendiente. | Reinicie la VM. La instalación continuará después del reinicio. La desinstalación se debe invocar manualmente. |


### <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/community/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las extensiones, consulte el artículo [Características y extensiones de las máquinas virtuales para Windows](features-windows.md).

Para obtener más información acerca de las VM de serie N, consulte el artículo [Tamaños de máquinas virtuales optimizadas para GPU](../sizes-gpu.md).
