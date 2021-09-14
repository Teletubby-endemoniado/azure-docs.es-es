---
title: Evaluación de servidores habilitados para Azure Arc con una máquina virtual de Azure
description: Aprenda a evaluar servidores habilitados para Azure Arc mediante una máquina virtual de Azure.
ms.date: 09/02/2021
ms.topic: conceptual
ms.openlocfilehash: 81c82ad05b715e7d0243585d3bce0abd44f00670
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123424391"
---
# <a name="evaluate-arc-enabled-servers-on-an-azure-virtual-machine"></a>Evaluación de servidores habilitados para Arc en una máquina virtual de Azure

Los servidores habilitados para Azure Arc están diseñados para ayudarle a conectar a Azure los servidores que se ejecutan en el entorno local o en otras nubes. Lo habitual es no usar servidores habilitados para Azure Arc en máquinas virtuales de Azure porque las mismas funcionalidades están disponibles de forma nativa para estas máquinas virtuales, incluida una representación de la máquina virtual en Azure Resource Manager, extensiones de máquina virtual, identidades administradas y Azure Policy. Si intenta instalar servidores habilitados para Azure Arc en una máquina virtual de Azure, recibirá un mensaje de error que indica que no se admite y se cancelará la instalación del agente.

Aunque no puede instalar servidores habilitados para Azure Arc en una máquina virtual de Azure en escenarios de producción, es posible configurar servidores habilitados para Azure Arc que se ejecuten en una máquina virtual de Azure *solo con fines de evaluación y prueba*. Este artículo le ayudará a configurar una máquina virtual de Azure para poder usar en ella servidores habilitados para Azure Arc.

## <a name="prerequisites"></a>Requisitos previos

* La cuenta debe tener asignado el rol [Colaborador de la máquina virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor).
* La máquina virtual de Azure ejecuta un [sistema operativo compatible con servidores habilitados para Arc](agent-overview.md#supported-operating-systems). Si no tiene una máquina virtual de Azure, puede implementar una [máquina virtual Windows simple](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2fquickstarts%2fmicrosoft.compute%2fvm-simple-windows%2fazuredeploy.json) o una [máquina virtual Ubuntu Linux 18.04 LTS](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2fquickstarts%2fmicrosoft.compute%2fvm-simple-windows%2fazuredeploy.json).
* La máquina virtual de Azure puede comunicarse de forma externa para descargar el paquete del agente Azure Connected Machine para Windows desde el [Centro de descarga de Microsoft](https://aka.ms/AzureConnectedMachineAgent) y para Linux desde el [repositorio de paquetes](https://packages.microsoft.com/) de Microsoft. Si la conectividad saliente a Internet está restringida siguiendo la directiva de seguridad de TI, deberá descargar manualmente el paquete del agente y copiarlo en una carpeta de la máquina virtual de Azure.
* Una cuenta con privilegios elevados (es decir, un administrador o como raíz) en la máquina virtual y acceso RDP o SSH a la máquina virtual.
* Para registrar y administrar la máquina virtual de Azure con servidores habilitados para Arc, debe ser miembro del rol [Administrador de recursos de Azure Connected Machine](../../role-based-access-control/built-in-roles.md#azure-connected-machine-resource-administrator) o [Colaborador](../../role-based-access-control/built-in-roles.md#contributor) del grupo de recursos.

## <a name="plan"></a>Plan

Para empezar a administrar la máquina virtual de Azure como un servidor habilitado para Arc, debe realizar los siguientes cambios en la máquina virtual de Azure para poder instalar y configurar servidores habilitados para Arc.

1. Quite las extensiones de máquina virtual implementadas en la máquina virtual de Azure, como el agente de Log Analytics. Aunque los servidores habilitados para Arc admiten muchas de las mismas extensiones que las máquinas virtuales de Azure, el agente de servidores habilitados para Arc no puede administrar las extensiones de máquina virtual ya implementadas en la máquina virtual.

2. Deshabilite el agente invitado de Windows o Linux de Azure. El agente invitado de máquina virtual de Azure tiene un propósito similar al del agente de maquina conectada de servidores habilitados para Azure Arc. Para evitar conflictos entre los dos, es necesario deshabilitar el agente de máquina virtual de Azure. Una vez deshabilitado, no puede usar extensiones de máquina virtual ni algunos servicios de Azure.

3. Cree una regla de seguridad para denegar el acceso a Azure Instance Metadata Service (IMDS). IMDS es una API REST a la que las aplicaciones pueden llamar para obtener información sobre la representación de la máquina virtual en Azure, lo que incluye su ubicación e identificador de recurso. IMDS también proporciona acceso a las identidades administradas asignadas a la máquina. Los servidores habilitados para Azure Arc proporcionan su propia implementación de IMDS y devuelven información sobre la representación de Azure Arc de la máquina virtual. Para evitar situaciones en las que ambos puntos de conexión de IMDS están disponibles y las aplicaciones tienen que elegir entre los dos, debe bloquear el acceso al IMDS de la máquina virtual de Azure para que la implementación de IMDS del servidor habilitado para Azure Arc sea la única disponible.

Después de realizar estos cambios, la máquina virtual de Azure se comporta como cualquier máquina o servidor fuera de Azure y es el punto de partida necesario para instalar y evaluar los servidores habilitados para Azure Arc.

Cuando los servidores habilitados para Arc estén configurados en la máquina virtual, verá dos representaciones de ella en Azure. Una es el recurso de máquina virtual de Azure, con un tipo de recurso `Microsoft.Compute/virtualMachines`, y la otra es un recurso de Azure Arc, con un tipo de recurso `Microsoft.HybridCompute/machines`. Dado que se impide la administración del sistema operativo invitado desde el servidor host físico compartido, la mejor manera de considerar los dos recursos es que el recurso de máquina virtual de Azure es el hardware virtual de la máquina virtual y vamos a controlar el estado de energía y ver información sobre sus configuraciones de SKU, red y almacenamiento. El recurso de Azure Arc administra el sistema operativo invitado de esa máquina virtual y se puede usar para instalar extensiones, ver los datos de cumplimiento de Azure Policy y completar cualquier otra tarea compatible con servidores habilitados para Arc.

## <a name="reconfigure-azure-vm"></a>Reconfiguración de la máquina virtual de Azure

1. Quite las extensiones de máquina virtual de la máquina virtual de Azure.

   En Azure Portal, vaya al recurso de máquina virtual de Azure y, en el panel izquierdo, seleccione **Extensiones**. Si hay extensiones instaladas en la máquina virtual, seleccione cada extensión individualmente y, luego, elija **Desinstalar**. Espere a que todas las extensiones terminen de desinstalarse antes de continuar con el paso 2.

2. Deshabilite el agente invitado de máquina virtual de Azure.

   Para deshabilitar el agente invitado de máquina virtual de Azure, deberá conectarse a la máquina virtual mediante Conexión a Escritorio remoto (Windows) o SSH (Linux). Una vez conectado, ejecute los siguientes comandos para deshabilitar el agente invitado.

   Puede ejecutar los siguientes comandos de PowerShell:

   ```powershell
   Set-Service WindowsAzureGuestAgent -StartupType Disabled -Verbose
   Stop-Service WindowsAzureGuestAgent -Force -Verbose
   ```

   En Linux, ejecute los siguientes comandos:

   ```bash
   sudo service walinuxagent stop
   sudo waagent -deprovision -force
   sudo rm -rf /var/lib/waagent
   ```

3. Bloquee el acceso al punto de conexión IMDS de Azure.

   Mientras sigue conectado al servidor, ejecute los siguientes comandos para bloquear el acceso al punto de conexión IMDS de Azure. En Windows, ejecute el siguiente comando de PowerShell:

   ```powershell
   New-NetFirewallRule -Name BlockAzureIMDS -DisplayName "Block access to Azure IMDS" -Enabled True -Profile Any -Direction Outbound -Action Block -RemoteAddress 169.254.169.254
   ```

   En Linux, consulte la documentación de su distribución para saber la mejor manera de bloquear el acceso saliente a `169.254.169.254/32` a través del puerto TCP 80. Normalmente bloqueará el acceso saliente con el firewall integrado, pero también puede hacerlo temporalmente con **iptables** o **nftables**.

   Si la máquina virtual de Azure ejecuta Ubuntu, realice los pasos siguientes para configurar su firewall sin complicaciones (UFW):

   ```bash
   sudo ufw --force enable
   sudo ufw deny out from any to 169.254.169.254
   sudo ufw default allow incoming
   ```
   Para realizar una configuración genérica de iptables, ejecute el siguiente comando:

   ```bash
   iptables -A OUTPUT -d 169.254.169.254 -j DROP
   ```

   > [!NOTE]
   > Esta configuración debe establecerse después de cada reinicio, a menos que se utilice una solución iptables persistente.

   Si la máquina virtual de Azure ejecuta CentOS, Red Hat o SUSE Linux Enterprise Server (SLES), realice los pasos siguientes para configurar firewalld:

   ```bash
   firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -d 169.254.169.254 -j DROP
   firewall-cmd --reload
   ```

4. Instale y configure el agente de servidores habilitados para Azure Arc.

   La máquina virtual ya está lista para empezar a evaluar los servidores habilitados para Arc. Para instalar y configurar el agente de servidores habilitados para Arc, consulte [Conexión de máquinas híbridas mediante Azure Portal](onboard-portal.md) y siga los pasos para generar un script de instalación e instalarlo mediante el método generado por script.

   > [!NOTE]
   > Si la conectividad saliente a Internet está restringida desde la máquina virtual de Azure, deberá descargar el paquete del agente manualmente. Copie el paquete del agente en la máquina virtual de Azure y modifique el script de instalación de servidores habilitados para Arc para hacer referencia a la carpeta de origen.

Si se perdió uno de los pasos, el script de instalación detecta que se está ejecutando en una máquina virtual de Azure y finaliza con un error. Compruebe que ha completado los pasos 1 a 3 y, luego, vuelva a ejecutar el script.

## <a name="verify-the-connection-with-azure-arc"></a>Comprobación de la conexión con Azure Arc

Después de instalar y configurar el agente para que se registre en los servidores habilitados para Azure Arc, vaya a Azure Portal a fin de comprobar que el servidor se ha conectado correctamente. Vea la máquina en [Azure Portal](https://portal.azure.com).

![Una conexión de servidor correcta](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [planear y habilitar un gran número de máquinas en servidores habilitados para Azure Arc](plan-at-scale-deployment.md) para simplificar la configuración de las funcionalidades esenciales de seguridad, administración y supervisión de Azure.

* Aprenda sobre las [extensiones de máquina virtual de Azure compatibles](manage-vm-extensions.md) disponibles para simplificar la implementación con otros servicios de Azure, como Automation, KeyVault y otros para su máquina Windows o Linux.

* Cuando haya terminado de realizar las pruebas, consulte [Eliminación del agente de servidores habilitados para Arc](manage-agent.md#remove-the-agent).
