---
title: Recopilación de registros de invitado de VM en una GPU de Azure Stack Edge Pro
description: Describe cómo crear un paquete de soporte técnico con registros de invitado para VM en un dispositivo GPU de Azure Stack Edge Pro.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 07/30/2021
ms.author: alkohli
ms.openlocfilehash: 1c25ea8c35b81169119b0f10025b36319d4dc2c9
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121744614"
---
# <a name="collect-vm-guest-logs-on-an-azure-stack-edge-pro-gpu-device"></a>Recopilación de registros de invitado de VM en un dispositivo GPU de Azure Stack Edge Pro

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Para diagnosticar los errores de aprovisionamiento de VM en el dispositivo GPU de Azure Stack Edge Pro, revisará los registros de invitado de la máquina virtual con errores. En este artículo se describe cómo recopilar registros de invitado para las VM en un paquete de soporte técnico.

> [!NOTE]
> También puede supervisar los registros de actividad de las máquinas virtuales en Azure Portal. Para obtener más información, vea [Supervisión de la actividad de la máquina virtual en el dispositivo](azure-stack-edge-gpu-monitor-virtual-machine-activity.md).


## <a name="collect-vm-guest-logs-in-support-package"></a>Recopilación de registros de invitado de máquina virtual en el paquete de soporte técnico

Para recopilar registros de invitado para máquinas virtuales con errores en un dispositivo GPU de Azure Stack Edge Pro, siga estos pasos:

1. [Conéctese a la interfaz de PowerShell del dispositivo](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface).

2. Recopile los registros de invitado para las VM con errores e inclúyalos en un paquete de soporte técnico mediante la ejecución de los siguientes comandos:

   ```powershell
   Get-VMInGuestLogs -FailedVM
   Get-HcsNodeSupportPackage -Path “\\<network path>” -Include InGuestVMLogFiles -Credential “domain_name\user”
   ```

   Encontrará los registros en la carpeta `hcslogs\VmGuestLogs`.

3. Para obtener los detalles del historial de aprovisionamiento de VM, revise los registros siguientes:

   **VM de Linux:**
   - /var/log/cloud-init-output.log
   - /var/log/cloud-init.log
   - /var/log/waagent.log

   **VM de Windows**:
   - C:\Windows\Azure\Panther\WaSetup.xml

## <a name="next-steps"></a>Pasos siguientes

- [Supervisión del registro de actividad de la máquina virtual](azure-stack-edge-gpu-monitor-virtual-machine-activity.md).
- [Solución de problemas de aprovisionamiento de VM en un dispositivo GPU de Azure Stack Edge Pro](azure-stack-edge-gpu-troubleshoot-virtual-machine-provisioning.md).