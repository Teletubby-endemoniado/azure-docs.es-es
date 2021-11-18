---
title: Diagnósticos de arranque de Azure
description: Información general sobre diagnósticos de arranque de Azure y diagnósticos de arranque administrados
services: virtual-machines
ms.service: virtual-machines
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.date: 11/06/2020
ms.openlocfilehash: fdb7b3bcaac2825e64111bdba4ad98554bfc02ae
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132339264"
---
# <a name="azure-boot-diagnostics"></a>Diagnósticos de arranque de Azure

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

Diagnósticos de arranque es una característica de depuración para máquinas virtuales (VM) de Azure que permite el diagnóstico de errores de arranque de la máquina virtual. Los diagnósticos de arranque permiten a un usuario observar el estado de la máquina virtual cuando está arrancando mediante la recopilación de información de registro serie y capturas de pantallas.

## <a name="boot-diagnostics-storage-account"></a>Cuenta de almacenamiento de diagnóstico de arranque
Al crear una máquina virtual en Azure Portal, los diagnósticos de arranque están habilitados de forma predeterminada. La experiencia de los diagnósticos de arranque recomendada consiste en usar una cuenta de almacenamiento administrada, ya que proporciona importantes mejoras de rendimiento en el momento de crear una máquina virtual de Azure. Esto se debe a que se usará una cuenta de almacenamiento administrada de Azure y se eliminará el tiempo necesario para crear una nueva cuenta de almacenamiento de usuario para almacenar los datos de diagnóstico de arranque.

> [!IMPORTANT]
> Los blobs de datos de diagnósticos de arranque (que constan de registros e imágenes de instantáneas) se almacenan en una cuenta de almacenamiento administrada. A los clientes solo se les cobrarán los GiB utilizados por los blobs, no por el tamaño aprovisionado del disco. Los medidores de las instantáneas se usarán para la facturación de la cuenta de almacenamiento administrada. Dado que las cuentas administradas se crean en ZRS estándar o LRS estándar, a los clientes se les cobrará a 0,05 USD/GB al mes solo para el tamaño de los blobs de datos de diagnóstico. Para obtener más información sobre los precios, consulte [Precios de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/). Los clientes verán este cargo asociado al URI de recurso de la VM. 

Una experiencia alternativa de diagnóstico de arranque consiste en usar una cuenta de almacenamiento administrada por el usuario. Un usuario puede crear una cuenta de almacenamiento o usar una existente.
> [!NOTE]
> Las cuentas de almacenamiento administradas por el usuario asociadas con los diagnósticos de arranque requieren que la cuenta de almacenamiento y las máquinas virtuales asociadas residan en la misma región y suscripción. 



## <a name="boot-diagnostics-view"></a>Vista de diagnósticos de arranque
Situada en la hoja de la máquina virtual, la opción Diagnósticos de arranque se encuentra en la sección *Soporte técnico y solución de problemas* de Azure Portal. Al seleccionar los diagnósticos de arranque se mostrará una captura de pantalla e información de registro serie. El registro serie contiene la mensajería del kernel y la captura de pantalla es una instantánea del estado actual de las máquinas virtuales. El aspecto de la captura de pantalla esperada, depende de si la máquina virtual se ejecuta en Windows o Linux. En Windows, los usuarios verán un fondo de escritorio y en Linux, los usuarios verán un mensaje de inicio de sesión.

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-linux.png" alt-text="Captura de pantalla de diagnósticos de arranque de Linux":::
:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-windows.png" alt-text="Captura de pantalla de diagnósticos de arranque de Windows":::

## <a name="enable-managed-boot-diagnostics"></a>Habilitación de diagnósticos de arranque administrados 
Los diagnósticos de arranque administrados se pueden habilitar a través de Azure Portal, la CLI y plantillas de ARM. Todavía no se admite la habilitación a través de PowerShell. 

### <a name="enable-managed-boot-diagnostics-using-the-azure-portal"></a>Habilitación de diagnósticos de arranque administrados mediante Azure Portal
Al crear una máquina virtual en Azure Portal, el valor predeterminado es tener habilitados los diagnósticos de arranque mediante una cuenta de almacenamiento administrada. Para verlo, vaya a la pestaña *Administración* durante la creación de la máquina virtual. 

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-enable-portal.png" alt-text="Captura de pantalla de la habilitación del diagnóstico de arranque administrado durante la creación de la máquina virtual":::.

### <a name="enable-managed-boot-diagnostics-using-cli"></a>Habilitación de diagnósticos de arranque administrados mediante la CLI
Los diagnósticos de arranque con una cuenta de almacenamiento administrada se admiten en la CLI de Azure 2.12.0 y versiones posteriores. Si no especifica un nombre o un URI para una cuenta de almacenamiento, se usará una cuenta administrada. Para obtener más información y ejemplos de código, consulte la [documentación de la CLI para los diagnósticos de arranque](/cli/azure/vm/boot-diagnostics).

### <a name="enable-managed-boot-diagnostics-using-powershell"></a>Habilitación de diagnósticos de arranque administrados mediante PowerShell
Los diagnósticos de arranque con una cuenta de almacenamiento administrada se admiten en Azure PowerShell 6.6.0 y versiones posteriores. Si no especifica un nombre o un URI para una cuenta de almacenamiento, se usará una cuenta administrada. Para obtener más información y ejemplos de código, consulte la [documentación de PowerShell para los diagnósticos de arranque](https://docs.microsoft.com/powershell/module/az.compute/set-azvmbootdiagnostic?view=azps-6.6.0).

### <a name="enable-managed-boot-diagnostics-using-azure-resource-manager-arm-templates"></a>Habilitación de diagnósticos de arranque administrados mediante plantillas de Azure Resource Manager (ARM)
Todas las versiones de API posteriores a 2020-06-01 admiten los diagnósticos de arranque administrados. Para más información, consulte la [vista de instancia de diagnósticos de arranque](/rest/api/compute/virtualmachines/createorupdate#bootdiagnostics).

```ARM Template
            "name": "[parameters('virtualMachineName')]",
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2020-06-01",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"
            ],
            "properties": {
                "hardwareProfile": {
                    "vmSize": "[parameters('virtualMachineSize')]"
                },
                "storageProfile": {
                    "osDisk": {
                        "createOption": "fromImage",
                        "managedDisk": {
                            "storageAccountType": "[parameters('osDiskType')]"
                        }
                    },
                    "imageReference": {
                        "publisher": "Canonical",
                        "offer": "UbuntuServer",
                        "sku": "18.04-LTS",
                        "version": "latest"
                    }
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces', parameters('networkInterfaceName'))]"
                        }
                    ]
                },
                "osProfile": {
                    "computerName": "[parameters('virtualMachineComputerName')]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "linuxConfiguration": {
                        "disablePasswordAuthentication": true
                    }
                },
                "diagnosticsProfile": {
                    "bootDiagnostics": {
                        "enabled": true
                    }
                }
            }
        }
    ],

```

## <a name="limitations"></a>Limitaciones
- Los diagnósticos de arranque administrados solo están disponibles para máquinas virtuales de Azure Resource Manager. 
- Los diagnósticos de arranque administrados no admiten máquinas virtuales con discos de sistema operativo no administrados.
- Diagnósticos de arranque no es compatible con las cuentas de almacenamiento prémium ni los tipos de cuenta de almacenamiento con redundancia de zona. Si alguno de ellos se usa para los diagnósticos de arranque, los usuarios recibirán un error `StorageAccountTypeNotSupported` al iniciar la máquina virtual. 
- Las cuentas de almacenamiento administradas se admiten en la API de Resource Manager versión "2020-06-01" y posteriores.
- Actualmente, la consola serie de Azure no es compatible con una cuenta de almacenamiento administrada de los diagnósticos de arranque. Más información sobre la [consola serie de Azure](/troubleshoot/azure/virtual-machines/serial-console-overview).
- El portal solo admite el uso de diagnósticos de arranque con una cuenta de almacenamiento administrada para máquinas virtuales de instancia única.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la [consola serie de Azure](/troubleshoot/azure/virtual-machines/serial-console-overview) y cómo usar los diagnósticos de arranque para [solucionar problemas de máquinas virtuales en Azure](/troubleshoot/azure/virtual-machines/boot-diagnostics).
