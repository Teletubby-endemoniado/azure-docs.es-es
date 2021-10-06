---
title: Compatibilidad con la migración de VMware en Azure Migrate
description: Aprenda sobre la compatibilidad con la migración de máquinas virtuales de VMware en Azure Migrate.
author: anvar-ms
ms.author: anvar
ms.manager: bsiva
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 1217acb5911366b8e3943d414aeeeaf8a9e5e1e1
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129357241"
---
# <a name="support-matrix-for-vmware-migration"></a>Matriz de compatibilidad para la migración de VMware

En este artículo se resumen los valores de compatibilidad y las limitaciones para migrar VM de VMware con [Azure Migrate: Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool). Si busca información sobre cómo evaluar máquinas virtuales de VMware para migrar a Azure, revise la [matriz de compatibilidad de evaluación](migrate-support-matrix-vmware.md).


## <a name="migration-options"></a>Opciones de migración

Puede migrar máquinas virtuales de VMware de dos maneras:

- **Uso de la migración sin agente**: Las máquinas virtuales se migran sin necesidad de instalar nada en ellas. Se implementa el [dispositivo de Azure Migrate](migrate-appliance.md) para la migración sin agente.
- **Uso de la migración basada en agente**: Instale un agente en la máquina virtual para su replicación. En el caso de la migración basada en agente, implemente un [dispositivo de replicación](migrate-replication-appliance.md).

Revise [este artículo](server-migrate-overview.md) para averiguar qué método desea usar.

## <a name="agentless-migration"></a>Migración sin agentes

En esta sección se resumen los requisitos de la migración de máquinas virtuales de VMware a Azure sin agente.

### <a name="vmware-requirements-agentless"></a>Requisitos de VMware (sin agente)

En la tabla se resumen los requisitos del hipervisor de VMware.

**VMware** | **Detalles**
--- | ---
**VMware vCenter Server** | Versión 5.5, 6.0, 6.5, 6.7, 7.0.
**Host ESXI de VMware vSphere** | Versión 5.5, 6.0, 6.5, 6.7, 7.0.
**Permisos de vCenter Server** | La migración sin agentes utiliza el [dispositivo de Azure Migrate](migrate-appliance.md). El dispositivo necesita estos permisos en vCenter Server:<br/><br/> - **Datastore.Browse** (Datastore -> Browse datastore): Permite examinar los archivos de registro de máquina virtual para solucionar problemas de creación y eliminación de instantáneas.<br/><br/> - **Datastore.FileManagement** (Datastore -> Low level file operations): Permite operaciones de lectura, escritura, eliminación y cambio de nombre en el explorador del almacén de datos para solucionar problemas de creación y eliminación de instantáneas.<br/><br/> - **VirtualMachine.Config.ChangeTracking** (Virtual machine -> Disk change tracking): Permite habilitar o deshabilitar el seguimiento de cambios de los discos de máquina virtual para extraer los bloques de datos cambiados entre instantáneas.<br/><br/> - **VirtualMachine.Config.DiskLease** (Virtual machine -> Disk lease): Permite operaciones de concesión de discos para una máquina virtual, para leer el disco mediante el kit de desarrollo de discos virtuales de VMware vSphere (VDDK).<br/><br/> - **VirtualMachine.Provisioning.DiskRandomRead** (Virtual machine -> Provisioning -> Allow read-only disk access): Permite abrir un disco en una máquina virtual para leerlo mediante VDDK.<br/><br/> - **VirtualMachine.Provisioning.DiskRandomAccess** (Virtual machine -> Provisioning -> Allow disk access): Permite abrir un disco en una máquina virtual para leerlo mediante VDDK.<br/><br/> - **VirtualMachine.Provisioning.GetVmFiles** (Virtual machine -> Provisioning -> Allow virtual machine download): Permite operaciones de lectura en los archivos asociados con una máquina virtual, para descargar los registros y solucionar problemas si se produce un error.<br/><br/> - **VirtualMachine.State.\* *_ (Virtual machine -> Snapshot management): permite la creación y administración de instantáneas de máquinas virtuales para la replicación.<br/><br/> - _* VirtualMachine.Interact.PowerOff** (Virtual machine -> Interaction -> Power off): Permite apagar la máquina virtual durante la migración a Azure.



### <a name="vm-requirements-agentless"></a>Requisitos de VM (sin agente)

En la tabla se resumen los requisitos de migración sin agente para las VM de VMware.

**Soporte técnico** | **Detalles**
--- | ---
**Sistemas operativos compatibles** | Puede migrar los sistemas operativos [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) y [Linux](../virtual-machines/linux/endorsed-distros.md) que admite Azure.
**VM Windows en Azure** | Es posible que tenga que [realizar algunos cambios](prepare-for-migration.md#verify-required-changes-before-migrating) en las VM antes de la migración.
**VM Linux en Azure** | Es posible que algunas máquinas virtuales requieran cambios para poder ejecutarse en Azure.<br/><br/> En el caso de Linux, Azure Migrate realiza los cambios automáticamente para estos sistemas operativos:<br/> - Red Hat Enterprise Linux 8, 7.9, 7.8, 7.7, 7.6, 7.5, 7.4, 7.0, 6.x <br/> - Cent OS 8, 7.7, 7.6, 7.5, 7.4, 6.x</br> - SUSE Linux Enterprise Server 11, 12, 15 SP0, 15 SP1 <br/>- Ubuntu 20.04, 19.04, 19.04LTS, 10.04LTS,  14.04LTS, 16.04LTS y 18.04LTS<br/> - Debian 7, 8, 9 <br/> Oracle Linux 6, 7.7, 7.7-CI y 7.8<br/> En el caso de otros sistemas operativos, realice [los cambios necesarios](prepare-for-migration.md#verify-required-changes-before-migrating) manualmente.
**Requisitos de arranque** | Si/boot está en una partición dedicada, debe residir en el disco del sistema operativo y no distribuirse en varios discos.<br/> Si /boot forma parte de la partición raíz (/), la partición "/" debe estar en el disco del sistema operativo y no abarcar otros discos.
**Arranque UEFI** | Compatible. Las máquinas virtuales basadas en UEFI se migrarán a máquinas virtuales de generación 2 de Azure.
**Tamaño del disco** | Disco de sistema operativo de hasta 2 TB para las VM de Gen 1 y Gen 2; 32 TB para discos de datos.
**Límites del disco** |  Hasta 60 discos por máquina virtual.
**Discos/volúmenes cifrados** | Las máquinas virtuales con volúmenes o discos cifrados no se admiten para la migración.
**Clúster de discos compartido** | No compatible.
**Discos independientes** | No compatible.
**Discos RDM/de acceso directo** | Si las máquinas virtuales tienen discos RDM o de acceso directo, estos discos no se replicarán en Azure.
**NFS** | Los volúmenes NFS montados como volúmenes en las máquinas virtuales no se replicarán.
**Destinos iSCSI** | Las máquinas virtuales con destinos iSCSI no se admiten para la migración sin agente.
**E/S de varias rutas** | No compatible.
**Storage vMotion** | No compatible. La replicación no funcionará si una máquina virtual usa Storage vMotion.
**NIC en equipo** | No compatible.
**IPv6** | No compatible.
**Disco de destino** | Las VM solo se pueden migrar a discos administrados (HDD Estándar, SSD estándar, SSD Premium) en Azure.
**Replicación simultánea** | Hasta 300 replicaciones simultáneas de máquinas virtuales por cada instancia de vCenter Server con 1 dispositivo. Hasta 500 replicaciones simultáneas de máquinas virtuales por cada instancia de vCenter Server cuando se implementa un [dispositivo de escalado horizontal](./how-to-scale-out-for-migration.md) adicional.
**Instalación automática del agente de máquina virtual de Azure (agente de Windows y Linux)** | Compatible con Windows Server 2008 R2 en adelante <br/> Compatible con RHEL6, RHEL7, CentOS7, Ubuntu 14.04, Ubuntu 16.04, Ubuntu 18.04, Ubuntu 19.04, Ubuntu 19.10, Ubuntu 20.04.

> [!Note]
> Además de la conectividad a Internet, en el caso de las máquinas virtuales de Linux, asegúrese de que los siguientes paquetes están instalados para una instalación correcta del agente de Linux de Microsoft Azure (waagent):
>- Python 2.6
>- OpenSSL 1.0+
>- OpenSSH 5.3+
>- Utilidades del sistema de archivos: sfdisk, fdisk, mkfs, parted
>- Herramientas de contraseña: chpasswd, sudo
>- Herramientas de procesamiento de texto: sed, grep
>- Herramientas de red: ip-route

> [!TIP]
>  Con Azure Portal, podrá seleccionar hasta 10 máquinas virtuales a la vez para configurar la replicación. Para replicar más máquinas virtuales, puede usar el portal y agregar las máquinas virtuales que se van a replicar en varios lotes de 10 máquinas virtuales, o bien usar la interfaz de PowerShell de Azure Migrate para configurar la replicación. Asegúrese de no configurar la replicación simultánea en más del número máximo de máquinas virtuales admitidas para replicaciones simultáneas.

### <a name="appliance-requirements-agentless"></a>Requisitos de dispositivo (sin agente)

La migración sin agentes utiliza el [dispositivo de Azure Migrate](migrate-appliance.md). Puede implementar el dispositivo como una máquina virtual de VMware mediante una plantilla OVA, importada en vCenter Server, o bien mediante un [script de PowerShell](deploy-appliance-script.md).

- Obtenga más información sobre los [requisitos del dispositivo](migrate-appliance.md#appliance---vmware) para VMware.
- Obtenga información sobre las direcciones URL a las que el dispositivo necesita acceder en nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).
- En Azure Government, debe implementar el dispositivo [mediante el script](deploy-appliance-script-government.md).

### <a name="port-requirements-agentless"></a>Requisitos de puerto (sin agente)

**Dispositivo** | **Connection**
--- | ---
Dispositivo | Las conexiones salientes del puerto 443 para cargar los datos replicados en Azure y comunicarse con los servicios ce Azure Migrate que organizan la replicación y la migración.
Servidor vCenter | Conexiones salientes del puerto 443 que permiten al dispositivo organizar la replicación (crear instantáneas, copiar datos y liberar instantáneas).
Host vSphere/ESXI | Conexiones entrantes en el puerto TCP 902 para que el dispositivo replique datos de las instantáneas. Puerto de salida 902 del host ESXi.

## <a name="agent-based-migration"></a>Migración basada en agente


En esta sección se resumen los requisitos de la migración basada en agente.


### <a name="vmware-requirements-agent-based"></a>Requisitos de VMware (basado en agente)

En esta tabla se resume la compatibilidad con la evaluación y las limitaciones de los servidores de virtualización de VMware.

**Requisitos de VMware** | **Detalles**
--- | ---
**VMware vCenter Server** | Versión 5.5, 6.0, 6.5 o 6.7.
**Host ESXI de VMware vSphere** | Versión 5.5, 6.0, 6.5 o 6.7.
**Permisos de vCenter Server** | Una cuenta de solo lectura para vCenter Server.

### <a name="vm-requirements-agent-based"></a>Requisitos de VM (basada en agente)

En la tabla se resume la compatibilidad de las máquinas virtuales de VMware con las máquinas virtuales de VMware que desea migrar mediante la migración basada en agente.

**Soporte técnico** | **Detalles**
--- | ---
**Carga de trabajo de la máquina** | Azure Migrate admite la migración de cualquier carga de trabajo (por ejemplo, Active Directory, SQL Server, etc.) que se ejecute en una máquina compatible.
**Sistemas operativos** | Para conocer la información más reciente, revise la [compatibilidad del sistema operativo](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) de Site Recovery. Azure Migrate proporciona compatibilidad idéntica con el sistema operativo de máquina virtual.
**Sistema de archivos Linux/almacenamiento de invitados** | Para conocer la información más reciente, revise la [compatibilidad del sistema de archivos Linux](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) de Site Recovery. Azure Migrate tiene compatibilidad idéntica con el sistema de archivos Linux.
**Red y almacenamiento** | Para conocer la información más reciente, revise los requisitos previos de [red](../site-recovery/vmware-physical-azure-support-matrix.md#network) y [almacenamiento](../site-recovery/vmware-physical-azure-support-matrix.md#storage) de Site Recovery. Azure Migrate proporciona requisitos idénticos de red y almacenamiento.
**Requisitos de Azure** | Para conocer la información más reciente, revise los requisitos de [red](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [almacenamiento](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage) y [proceso](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) de Azure para Site Recovery. Azure Migrate tiene requisitos idénticos para la migración de VMware.
**Servicio de movilidad** | El agente del servicio de movilidad se debe instalar en cada máquina virtual que quiera migrar.
**Arranque UEFI** | Compatible. Las máquinas virtuales basadas en UEFI se migrarán a máquinas virtuales de segunda generación de Azure.
**UEFI: arranque seguro**         | No se admiten para la migración.
**Disco de destino** | Las VM solo se pueden migrar a discos administrados (HDD Estándar, SSD estándar, SSD Premium) en Azure.
**Tamaño del disco** | Disco de sistema operativo de hasta 2 TB para la VM de generación 1; disco de sistema operativo de hasta 4 TB para la VM de generación 2; 32 TB para discos de datos.
**Límites del disco** |  Hasta 63 discos por máquina virtual.
**Discos/volúmenes cifrados** | Las máquinas virtuales con volúmenes o discos cifrados no se admiten para la migración.
**Clúster de discos compartido** | No compatible.
**Discos independientes** | Compatible.
**Discos de acceso directo** | Compatible.
**NFS** | Los volúmenes NFS montados como volúmenes en las máquinas virtuales no se replicarán.
**Destinos iSCSI** | Compatible.
**E/S de varias rutas** | No compatible.
**Storage vMotion** | Compatible
**NIC en equipo** | No compatible.
**IPv6** | No compatible.




### <a name="appliance-requirements-agent-based"></a>Requisitos de dispositivo (basado en agente)

Al configurar el dispositivo de replicación con la plantilla OVA proporcionada en el concentrador de Azure Migrate, el dispositivo ejecuta Windows Server 2016 y cumple los requisitos de compatibilidad. Si configura el dispositivo de replicación manualmente en un servidor físico, asegúrese de que cumple los requisitos.

- Obtenga más información sobre los [requisitos del dispositivo de replicación](migrate-replication-appliance.md#appliance-requirements) para VMware.
- MySQL debe estar instalado en el dispositivo. Obtenga más información sobre las [opciones de instalación](migrate-replication-appliance.md#mysql-installation).
- Obtenga información sobre las direcciones URL a las que el dispositivo de replicación necesita acceder en nubes [públicas](migrate-replication-appliance.md#url-access) y [gubernamentales](migrate-replication-appliance.md#azure-government-url-access).
- Revise los [puertos](migrate-replication-appliance.md#port-access) a los que tiene que acceder el dispositivo de replicación.

### <a name="port-requirements-agent-based"></a>Requisitos de puerto (basado en agente)

**Dispositivo** | **Connection**
--- | ---
Máquinas virtuales | El servicio de movilidad que se ejecuta en las máquinas virtuales se comunica con el dispositivo de replicación local (servidor de configuración) en el puerto HTTPS 443 entrante para la administración de la replicación.<br/><br/> Las máquinas virtuales envían datos de replicación al servidor de procesos (que se ejecuta en la máquina del servidor de configuración) en el puerto HTTPS 9443 entrante. Este puerto se puede modificar.
Dispositivo de replicación | El dispositivo de replicación organiza la replicación con Azure a través del puerto HTTPS 443 saliente.
Servidor de proceso | El servidor de procesos recibe los datos de la replicación, los optimiza, los cifra y los envía a Azure Storage a través del puerto 443 de salida.<br/> De forma predeterminada, el servidor de procesos se ejecuta en el dispositivo de replicación.

## <a name="azure-vm-requirements"></a>Requisitos de VM de Azure

Todas las VM locales que se replican en Azure (con migración basada en agente o sin agente) deben cumplir los requisitos de VM de Azure que se resumen en esta tabla.

**Componente** | **Requisitos**
--- | --- | ---
Sistema operativo invitado | Comprueba los sistemas operativos de máquinas virtuales de VMware compatibles con la migración.<br/> Puede migrar cualquier carga de trabajo que se ejecute en un sistema operativo compatible.
Arquitectura del sistema operativo invitado | 64 bits
Tamaño del disco del sistema operativo | Hasta 2048 GB
Número de discos del sistema operativo | 1
Número de discos de datos | 64 o menos
Tamaño del disco de datos | Hasta 32 TB
Adaptadores de red | Se admiten varios adaptadores.
VHD compartido | No compatible.
Disco FC | No compatible.
BitLocker | No compatible.<br/><br/> Se debe deshabilitar BitLocker antes de migrar la máquina.
Nombre de la máquina virtual | Entre 1 y 63 caracteres.<br/><br/> Restringido a letras, números y guiones.<br/><br/> El nombre de la máquina debe empezar y terminar con una letra o un número.
Conexión después de la migración: Windows | Para conectarse a máquinas virtuales de Azure que se ejecutan en Windows después de la migración, siga estos pasos:<br/><br/> -Antes de la migración, habilite RDP en la máquina virtual local.<br/><br/> Asegúrese de que se hayan agregado las reglas de TCP y UDP para el perfil **Público**, y que RDP se permite en **Firewall de Windows** > **Aplicaciones permitidas** para todos los perfiles.<br/><br/> Para el acceso a VPN de sitio a sitio, habilite RDP y permítalo en **Firewall de Windows** -> **Aplicaciones y características permitidas** para redes de **dominio y privadas**.<br/><br/> Además, compruebe que la directiva SAN del sistema operativo está establecida en **OnlineAll**. [Más información](prepare-for-migration.md).
Conexión después de la migración: Linux | Para conectarse a máquinas virtuales de Azure después de la migración mediante SSH, siga estos pasos:<br/><br/> Antes de la migración, en la máquina local, compruebe que el servicio Secure Shell está establecido en Iniciar y que las reglas de firewall permiten una conexión SSH.<br/><br/> Tras la conmutación por error, en la máquina virtual de Azure, permita conexiones entrantes al puerto SSH para las reglas del grupo de seguridad de red de la máquina virtual conmutada por error y para la subred de Azure a la que esta se conecta.<br/><br/> Además, agregue una dirección IP pública para la máquina virtual.  


## <a name="next-steps"></a>Pasos siguientes

[Selección](server-migrate-overview.md) de una opción de migración de VMware.
