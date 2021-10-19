---
title: Solución de problemas de estado de invitado de VM Insights (versión preliminar)
description: Se describen los pasos para solucionar problemas que puede seguir cuando tenga problemas con el estado de las instancias de VM Insights.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/25/2021
ms.openlocfilehash: 13de7b055820ac680e8b513198224bcfd6421e11
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129708863"
---
# <a name="troubleshoot-vm-insights-guest-health-preview"></a>Solución de problemas de estado de invitado de VM Insights (versión preliminar)
En este artículo se describen los pasos para solucionar problemas que puede seguir cuando tenga problemas con el estado de las instancias de VM Insights.

## <a name="installation-errors"></a>Errores de instalación
Si alguna de las soluciones siguientes no resuelve el problema de instalación, recopile el registro del agente de estado de la máquina virtual ubicado en `/var/log/azure/Microsoft.Azure.Monitor.VirtualMachines.GuestHealthLinuxAgent/*.log` y póngase en contacto con Microsoft para una investigación más exhaustiva.

### <a name="error-message-showing-db5-error"></a>Mensaje de error con el error db5
La instalación no se ha realizado correctamente y el mensaje de error de instalación es similar al siguiente:

```
script execution exit with error: error: db5 error(5) from dbenv->open: Input/output error
error: cannot open Packages index using db5 - Input/output error (5)
error: cannot open Packages database in /var/lib/rpm
error: db5 error(5) from dbenv->open: Input/output error
error: cannot open Packages database in /var/lib/rpm
```
Esto se debe a que la base de datos rpm del administrador de paquetes está dañada; pruebe las instrucciones de [Recuperación de base de datos RPM](https://rpm.org/user_doc/db_recovery.html) para recuperarla. Una vez que recupere la base de datos rpm, intente instalarla de nuevo.

### <a name="init-file-already-exist-error"></a>Error de archivo de inicialización ya existente
La instalación no se ha realizado correctamente y el mensaje de error de instalación es similar al siguiente:

```
Exiting with the following error: "Failed to install VM Guest Health Agent: Init already exists: /etc/systemd/system/vmGuestHealthAgent.service"install vmGuestHealthAgent service execution failed with exit code 37
```

El agente de estado de la máquina virtual desinstalará primero el servicio existente antes de instalar la versión actual. El motivo de este error es probablemente que el archivo del servicio anterior no se ha limpiado por alguna razón. Inicie sesión en la máquina virtual y ejecute el comando siguiente para realizar una copia de seguridad del archivo de servicio existente e intente volver a instalarlo.

```
sudo mv /etc/systemd/system/vmGuestHealthAgent.service  /etc/systemd/system/vmGuestHealthAgent.service.bak
```

Si la instalación se realiza correctamente, ejecute el comando siguiente para quitar el archivo de copia de seguridad.

```
sudo rm /etc/systemd/system/vmGuestHealthAgent.service.bak
```

### <a name="installation-failed-to-exit-code-37"></a>Error de instalación en código de salida 37
La instalación no se ha realizado correctamente y el mensaje de error de instalación es similar al siguiente: 

```
Exiting with the following error: "Failed to install VM Guest Health Agent: exit status 1"install vmGuestHealthAgent service execution failed with exit code 37
```
Probablemente se deba a que el agente invitado de máquina virtual no ha podido adquirir el bloqueo del archivo del servicio. Pruebe a reiniciar la máquina virtual, de esta forma se liberará el bloqueo.


## <a name="upgrade-errors"></a>Errores de actualización

### <a name="upgrade-available-message-is-still-displayed-after-upgrading-guest-health"></a>El mensaje de actualización disponible sigue mostrándose después de actualizar el estado del invitado 

- Compruebe que la máquina virtual se ejecuta en Azure global. Aún no se admiten servidores habilitados para Azure Arc.
- Compruebe que la región y la versión del sistema operativo de la máquina virtual se admiten tal y como se describe en [Habilitación del estado de invitado de Azure Monitor para VM (versión preliminar)](vminsights-health-enable.md).
- Compruebe que la extensión de estado del invitado se ha instalado correctamente con el código de salida 0.
- Compruebe que la extensión del agente de Azure Monitor se ha instalado correctamente.
- Compruebe que la identidad administrada asignada por el sistema está habilitada para la máquina virtual.
- Compruebe que no se ha especificado ninguna identidad administrada asignada por el usuario para la máquina virtual.
- Compruebe que la configuración regional de las máquinas virtuales Windows es *Inglés (EE. UU.)* . La localización no es compatible actualmente con el agente de Azure Monitor.
- Compruebe que la máquina virtual no esté usando el proxy de red. El agente de Azure Monitor no admite actualmente servidores proxy.
- Compruebe que el agente de extensión de estado se ha iniciado sin errores. Si el agente no puede iniciarse, puede que el estado del agente esté dañado. Elimine el contenido de la carpeta de estado del agente y reinicie el agente.
  - En Linux: el demonio es *vmGuestHealthAgent*. La carpeta de estado es */var/opt/vmGuestHealthAgent/* *.
  - En Windows: el servicio es *VM Guest Health Agent*. La carpeta de estado es _%ProgramData%\Microsoft\VMGuestHealthAgent \\*_ .
- Compruebe que el agente de Azure Monitor tenga conectividad de red. 
  - Desde la máquina virtual, intente hacer ping a _\<region\>.handler.control.monitor.azure.com_. Por ejemplo, si la máquina virtual está en westeurope, intente hacer ping a _westeurope.handler.control.monitor.azure.com:443_.
- Compruebe que la máquina virtual tenga una asociación con una regla de recopilación de datos de la misma región que el área de trabajo de Log Analytics.
  -  Consulte **Creación de una regla de recopilación de datos (DCR)** en [Habilitación del estado de invitado de Azure Monitor para VM (versión preliminar)](vminsights-health-enable.md) para asegurarse de que la estructura de la DCR es correcta. Preste especial atención a la presencia de la sección de origen de datos *performanceCounters* configurada para tres contadores de ejemplo y la presencia de la sección *inputDataSources* en la configuración de la extensión de estado para enviar contadores a la extensión.
-  Compruebe si hay errores de la extensión de estado del invitado en la máquina virtual.
   -  En Linux: compruebe los registros en _/var/log/azure/Microsoft.Azure.Monitor.VirtualMachines.GuestHealthLinuxAgent/*.log_.
   -  En Windows: compruebe los registros en _C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Monitor.VirtualMachines.GuestHealthWindowsAgent\{versión de la extensión}\*.log_.
-  Compruebe si hay errores del agente de Azure Monitor en la máquina virtual.
   -  En Linux: compruebe los registros en _/var/log/mdsd.*_ .
   -  En Windows: compruebe los registros en _C:\WindowsAzure\Resources\*{vmName}.AMADataStore_.
 

## <a name="usage-errors"></a>Errores de utilización

### <a name="error-message-that-no-data-is-available"></a>Mensaje de error que indica que no hay datos disponibles 

![Sin datos](media/vminsights-health-troubleshoot/no-data.png)


#### <a name="verify-that-the-virtual-machine-meets-configuration-requirements"></a>Compruebe que la máquina virtual cumple estos requisitos de configuración.

1. Compruebe que la máquina virtual sea una máquina virtual de Azure. Azure Arc para servidores no se admite de momento.
2. Compruebe que la máquina virtual usa un [sistema operativo](vminsights-health-enable.md?current-limitations.md) compatible.
3. Compruebe que la máquina virtual está instalada en una [región compatible](vminsights-health-enable.md?current-limitations.md).
4. Compruebe que el área de trabajo de Log Analytics está instalada en una [región compatible](vminsights-health-enable.md?current-limitations.md).

#### <a name="verify-that-the-vm-is-properly-onboarded"></a>Comprobación de que la máquina virtual está incorporada correctamente
Compruebe que la extensión del agente de Azure Monitor y el agente de estado de la máquina virtual invitada se aprovisionaron correctamente en la máquina virtual. Seleccione **Extensiones** en el menú de la máquina virtual en Azure Portal y asegúrese de que se muestran los dos agentes.

![Extensiones de máquina virtual](media/vminsights-health-troubleshoot/extensions.png)

#### <a name="verify-the-system-assigned-identity-is-enabled-on-the-virtual-machine"></a>Comprobación de que la identidad asignada por el sistema está habilitada en la máquina virtual
Compruebe que la identidad asignada por el sistema está habilitada en la máquina virtual. Seleccione **Identidad** en el menú de la máquina virtual en Azure Portal. Si la identidad administrada por el usuario está habilitada, independientemente del estado de la identidad administrada por el sistema, el agente de Azure Monitor no podrá comunicarse con el servicio de configuración, y la extensión de estado de invitado no funcionará.

![Identidad asignada por el sistema](media/vminsights-health-troubleshoot/system-identity.png)

#### <a name="verify-data-collection-rule"></a>Comprobación de la regla de recopilación de datos
Compruebe que la regla de recopilación de datos que especifica la extensión de mantenimiento como un origen de datos está asociada a la máquina virtual.

### <a name="error-message-for-bad-request-due-to-insufficient-permissions"></a>Mensaje de error debido a una solicitud incorrecta por permisos insuficientes
Este error indica que el proveedor de recursos **Microsoft.WorkloadMonitor** no se ha registrado en la suscripción. Consulte [Tipos y proveedores de recursos de Azure](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider) para obtener información sobre registrar a este proveedor de recursos. 

![Solicitud incorrecta](media/vminsights-health-troubleshoot/bad-request.png)

### <a name="health-shows-as-unknown-after-guest-health-is-enabled"></a>El estado se muestra como "desconocido" después de habilitar el estado del invitado.

#### <a name="verify-that-performance-counters-on-windows-nodes-are-working-correctly"></a>Comprobación de que los contadores de rendimiento de los nodos de Windows funcionan correctamente 
El estado del invitado se basa en que el agente pueda recopilar contadores de rendimiento del nodo. Es posible que el conjunto base de bibliotecas de contadores de rendimiento esté dañado y tenga que recompilarse. Para recompilar los contadores de rendimiento, siga las instrucciones que se indican en [Recompilar manualmente los valores de la biblioteca de contadores de rendimiento](/troubleshoot/windows-server/performance/rebuild-performance-counter-library-values).





## <a name="next-steps"></a>Pasos siguientes

- [Obtenga información general sobre la característica de estado de invitado de VM Insights](vminsights-health-overview.md).
