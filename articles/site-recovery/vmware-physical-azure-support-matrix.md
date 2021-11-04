---
title: Matriz de compatibilidad para la recuperación ante desastres física o de VMware en Azure Site Recovery
description: Resume la compatibilidad de la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure mediante Azure Site Recovery.
ms.topic: conceptual
ms.date: 08/02/2021
ms.openlocfilehash: 084e0b9b0b42bbe2ba3632d54811e738197899f3
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131441483"
---
# <a name="support-matrix-for-disaster-recovery--of-vmware-vms-and-physical-servers-to-azure"></a>Matriz de compatibilidad para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure.

En este artículo se resumen los valores y los componentes admitidos para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure mediante [Azure Site Recovery](site-recovery-overview.md).

- [Más información](vmware-azure-architecture.md) sobre la arquitectura de recuperación ante desastres de una máquina virtual de VMware o un servidor físico.
- Siga nuestros [tutoriales](tutorial-prepare-azure.md) para probar la recuperación ante desastres.

> [!NOTE]
> Site Recovery no mueve ni almacena los datos de los clientes fuera de la región de destino, en la que se ha configurado la recuperación ante desastres para las máquinas de origen. Los clientes pueden seleccionar un almacén de Recovery Services de otra región si así lo deciden. El almacén de Recovery Services contiene metadatos, pero ningún dato real de los clientes.

## <a name="deployment-scenarios"></a>Escenarios de implementación

**Escenario** | **Detalles**
--- | ---
Recuperación ante desastres de máquinas virtuales de VMware | Replicación de máquinas virtuales de VMware locales en Azure. Puede implementar este escenario en Azure Portal o mediante [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Recuperación ante desastres de servidores físicos | Replicación de servidores Windows/Linux físicos locales en Azure. Puede implementar este escenario en Azure Portal. (no se admite en arquitecturas en versión preliminar)

## <a name="on-premises-virtualization-servers"></a>Servidores de virtualización locales

**Server** | **Requisitos** | **Detalles**
--- | --- | ---
vCenter Server | Versión 7.0 y actualizaciones posteriores en esta versión, 6.7, 6.5, 6.0 o 5.5 | Se recomienda que utilice un servidor vCenter en su implementación de recuperación ante desastres.
Hosts de vSphere | Versión 7.0 y actualizaciones posteriores en esta versión, 6.7, 6.5, 6.0 o 5.5 | Se recomienda que los hosts de vSphere y los servidores vCenter se encuentren en la misma red que el servidor de procesos. De forma predeterminada, el servidor de procesos se ejecuta en el servidor de configuración. [Más información](vmware-physical-azure-config-process-server-overview.md).

## <a name="site-recovery-configuration-server"></a>Servidor de configuración de Site Recovery

El servidor de configuración es una máquina local que ejecuta componentes de Site Recovery locales, incluido el servidor de configuración, el servidor de procesos y el servidor de destino maestro.

- Para las máquinas virtuales de VMware puede establecer el servidor de configuración mediante la descarga de una plantilla de OVF para crear una máquina virtual de VMware.
- Para los servidores físicos, la máquina del servidor de configuración se configura manualmente.

**Componente** | **Requisitos**
--- |---
Núcleos de CPU | 8
RAM | 16 GB
Número de discos | 3 discos<br/><br/> Los discos incluyen el disco del sistema operativo, el disco de memoria caché del servidor de procesos y la unidad de retención para la conmutación por recuperación.
Espacio libre en el disco | 600 GB de espacio para la memoria caché del servidor de procesos.
Espacio libre en el disco | 600 GB de espacio para la unidad de retención.
Sistema operativo  | Windows Server 2012 R2 o Windows Server 2016 con experiencia de escritorio <br/><br> Si tiene previsto usar el destino maestro integrado de este dispositivo para la conmutación por recuperación, asegúrese de que la versión del sistema operativo sea igual o superior a la de los elementos replicados.|
Configuración regional del sistema operativo | Español (es-es)
[PowerCLI](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) | No es necesaria para la configuración la versión [9.14](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery) u otra posterior del servidor.
Roles de Windows Server | No habilite Active Directory Domain Services, Internet Information Services ni Hyper-V.
Directivas de grupo| - Impedir el acceso al símbolo del sistema. <br/> - Impedir el acceso a herramientas de edición del Registro. <br/> - Confiar en la lógica de datos adjuntos de archivos. <br/> - Activar la ejecución de scripts. <br/> - [Más información](/previous-versions/windows/it-pro/windows-7/gg176671(v=ws.10))|
IIS | Asegúrese de lo siguiente:<br/><br/> - No hay ningún sitio web preexistente predeterminado <br/> - Habilitar la [autenticación anónima](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731244(v=ws.10)) <br/> - Habilitar la configuración de [FastCGI](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753077(v=ws.10))  <br/> - No hay ningún sitio web o aplicación preexistente que escuche en el puerto 443<br/>
Tipo de NIC | VMXNET3 (cuando se implementa como una máquina virtual VMware)
Tipo de dirección IP | estática
Puertos | 443 se usa para la orquestación del canal de control<br/>9443 se usa para el transporte de datos

> [!NOTE]
> El sistema operativo debe instalarse con la configuración regional en inglés. La conversión de la configuración regional posterior a la instalación podría crear problemas.



## <a name="replicated-machines"></a>Máquinas replicadas

En versión preliminar, la replicación se realiza mediante el dispositivo de replicación de Azure Site Recovery. Para información detallada sobre el dispositivo de replicación, consulte [este artículo](deploy-vmware-azure-replication-appliance-preview.md).

Site Recovery admite la replicación de cualquier carga de trabajo que se ejecute en una máquina compatible.

**Componente** | **Detalles**
--- | ---
Configuración del equipo | Las máquinas que se replican en Azure deben cumplir con los [requisitos de Azure](#azure-vm-requirements).
Carga de trabajo del equipo | Site Recovery admite la replicación de cualquier carga de trabajo que se ejecute en una máquina compatible. [Más información](./site-recovery-workload.md).
Nombre de equipo | Asegúrese de que el nombre para mostrar de la máquina no pertenezca a [nombres de recursos reservados de Azure](../azure-resource-manager/templates/error-reserved-resource-name.md).<br/><br/> Los nombres de los volúmenes lógicos no distinguen mayúsculas de minúsculas. Asegúrese de que no haya dos volúmenes con el mismo nombre en un dispositivo. Por ejemplo: Los volúmenes con los nombres "voLUME1" o "volume1" no se pueden proteger con Azure Site Recovery.

### <a name="for-windows"></a>Para Windows

**Sistema operativo** | **Detalles**
--- | ---
Windows Server 2019 | Se admite desde el [paquete acumulativo de actualizaciones 34](https://support.microsoft.com/help/4490016) (versión 9.22 de Mobility Service) y versiones posteriores.
Windows Server 2016 de 64 bits | Se admite para Server Core y Server con Experiencia de escritorio.
Windows Server 2012 R2 / Windows Server 2012 | Compatible.
Windows Server 2008 R2 con SP1 y versiones posteriores. | Compatible.<br/><br/> Desde la versión [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) del agente de Mobility Service, necesita la [actualización de la pila de servicio (SSU)](https://support.microsoft.com/help/4490628) y la [actualización de SHA-2](https://support.microsoft.com/help/4474419) instaladas en las máquinas que ejecutan Windows 2008 R2 con SP1 o posterior. SHA-1 no se admite desde septiembre de 2019 y, si la firma de código SHA-2 no está habilitada, la extensión del agente no se instalará ni actualizará según lo previsto. Más información sobre los [requisitos y la actualización de SHA-2](https://support.microsoft.com/topic/sha-2-code-signing-support-update-for-windows-server-2008-r2-windows-7-and-windows-server-2008-september-23-2019-84a8aad5-d8d9-2d5c-6d78-34f9aa5f8339).
Windows Server 2008 con SP2 o posterior (64 bits/32 bits) |  Se admiten para la migración solo. [Más información](migrate-tutorial-windows-server-2008.md).<br/><br/> Desde la versión [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) del agente de Mobility Service, necesita la [actualización de la pila de servicio (SSU)](https://support.microsoft.com/help/4493730) y la [actualización de SHA-2](https://support.microsoft.com/help/4474419) instaladas en máquinas con Windows 2008 SP2. SHA-1 no se admite desde septiembre de 2019 y, si la firma de código SHA-2 no está habilitada, la extensión del agente no se instalará ni actualizará según lo previsto. Más información sobre los [requisitos y la actualización de SHA-2](https://support.microsoft.com/topic/sha-2-code-signing-support-update-for-windows-server-2008-r2-windows-7-and-windows-server-2008-september-23-2019-84a8aad5-d8d9-2d5c-6d78-34f9aa5f8339).
Windows 10, Windows 8.1, Windows 8 | Solo se admite un sistema de 64 bits. No se admite uno de 32 bits.
Windows 7 con SP1 de 64 bits | Se admite desde el [paquete acumulativo de actualizaciones 36](https://support.microsoft.com/help/4503156) (versión 9.22 de Mobility Service) y versiones posteriores. </br></br> Desde la versión [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) del agente de Mobility Service, necesita la [actualización de la pila de servicio (SSU)](https://support.microsoft.com/help/4490628) y la [actualización de SHA-2](https://support.microsoft.com/help/4474419) instaladas en máquinas con Windows 7 SP1.  SHA-1 no se admite desde septiembre de 2019 y, si la firma de código SHA-2 no está habilitada, la extensión del agente no se instalará ni actualizará según lo previsto. Más información sobre los [requisitos y la actualización de SHA-2](https://support.microsoft.com/topic/sha-2-code-signing-support-update-for-windows-server-2008-r2-windows-7-and-windows-server-2008-september-23-2019-84a8aad5-d8d9-2d5c-6d78-34f9aa5f8339).

### <a name="for-linux"></a>Para Linux

**Sistema operativo** | **Detalles**
--- | ---
Linux | Solo se admite un sistema de 64 bits. No se admite uno de 32 bits.<br/><br/>Todos los servidores de Linux deberían tener los [componentes de los servicios de integración de Linux (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) instalados. Es necesario para arrancar el servidor en Azure después de una conmutación por error o de una conmutación por error de prueba. Si faltan componentes integrados de LIS, asegúrese de instalar los [componentes](https://www.microsoft.com/download/details.aspx?id=55106) antes de habilitar la replicación para que las máquinas arranquen en Azure. <br/><br/> Site Recovery organiza la conmutación por error para ejecutar servidores Linux en Azure. Sin embargo, los proveedores de Linux podrían limitar la compatibilidad a solo las versiones de distribución que no hayan alcanzado el final del ciclo de vida.<br/><br/> En las distribuciones de Linux, solo se admiten kernels de stock que forman parte del lanzamiento o la actualización de la versión secundaria de la distribución.<br/><br/> La actualización de máquinas protegidas entre distribuciones de versiones principales de Linux no se admite. Para actualizar, deshabilite la replicación, actualice el sistema operativo y, a continuación, habilite la replicación de nuevo.<br/><br/> [Más información](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) sobre la compatibilidad con Linux y la tecnología de código abierto en Azure.
Linux Red Hat Enterprise | 5.2 a 5.11</b><br/> 6.1 a 6.10</b> </br> 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, [7.7](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery), [7.8](https://support.microsoft.com/help/4564347/), [versión beta 7.9](https://support.microsoft.com/help/4578241/), [7.9](https://support.microsoft.com/help/4590304/) </br> [8.0](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery), 8.1, [8.2](https://support.microsoft.com/help/4570609), [8.3](https://support.microsoft.com/help/4597409/) <br/> Algunos kernels antiguos de servidores que ejecutan Red Hat Enterprise Linux 5.2-5.11 y 6.1-6.10 no incluyen [componentes de los servicios de integración de Linux (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) preinstalados. Si faltan componentes integrados de LIS, asegúrese de instalar los [componentes](https://www.microsoft.com/download/details.aspx?id=55106) antes de habilitar la replicación para que las máquinas arranquen en Azure.
Linux: CentOS | 5.2 a 5.11</b><br/> 6.1 a 6.10</b><br/> </br> 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, [7.7](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery), [7.8](https://support.microsoft.com/help/4564347/), [7.9](https://support.microsoft.com/help/4578241/) </br> [8.0](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery), 8.1, [8.2](https://support.microsoft.com/help/4570609), [8.3](https://support.microsoft.com/help/4597409/) <br/><br/> Algunos kernels antiguos de servidores que ejecutan CentOS 5.2-5.11 y 6.1-6.10 no incluyen [componentes de los servicios de integración de Linux (LIS)](https://www.microsoft.com/download/details.aspx?id=55106) preinstalados. Si faltan componentes integrados de LIS, asegúrese de instalar los [componentes](https://www.microsoft.com/download/details.aspx?id=55106) antes de habilitar la replicación para que las máquinas arranquen en Azure.
Ubuntu | Servidor con Ubuntu 14.04* LTS [(consulte las versiones de kernel compatibles)](#ubuntu-kernel-versions)<br/>Servidor con Ubuntu 16.04* LTS [(consulte las versiones de kernel compatibles)](#ubuntu-kernel-versions) </br> Servidor con Ubuntu 18.04* LTS [(consulte las versiones de kernel compatibles)](#ubuntu-kernel-versions) </br> Servidor con Ubuntu 20.04* LTS [(consulte las versiones de kernel compatibles)](#ubuntu-kernel-versions) </br> (*incluye compatibilidad con todas las versiones 14.04.* x *, 16.04.* x *, 18.04.* x *y 20.04.* x*)
Debian | Debian 7/Debian 8 (incluye compatibilidad para todas las versiones 7. *x*, 8. *x*); Debian 9 (incluye compatibilidad con las versiones 9.1 a 9.13. Debian 9.0 no se admite). Debian 10 [(Revise las versiones de kernel admitidas)](#debian-kernel-versions).
SUSE Linux | SUSE Linux Enterprise Server 12 SP1, SP2, SP3, SP4, [SP5](https://support.microsoft.com/help/4570609) [(revise las versiones de kernel admitidas)](#suse-linux-enterprise-server-12-supported-kernel-versions) <br/> SUSE Linux Enterprise Server 15, 15 SP1 [(revise las versiones de kernel admitidas)](#suse-linux-enterprise-server-15-supported-kernel-versions) <br/> SUSE Linux Enterprise Server 11 SP3. [Asegúrese de descargar el instalador del agente de movilidad más reciente en el servidor de configuración](vmware-physical-mobility-service-overview.md#download-latest-mobility-agent-installer-for-suse-11-sp3-rhel-5-debian-7-server). </br> SUSE Linux Enterprise Server 11 SP4 </br> **Nota**: No se admite la actualización de máquinas replicadas de SUSE Linux Enterprise Server 11 SP3 a SP4. Para actualizar, deshabilite la replicación y habilítela de nuevo después de la actualización. <br/>|
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, [7.7](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery), [7.8](https://support.microsoft.com/help/4573888/), [7.9](https://support.microsoft.com/help/4597409/), [8.0](https://support.microsoft.com/help/4573888/), [8.1](https://support.microsoft.com/help/4573888/), [8.2](https://support.microsoft.com/topic/b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8), [8.3](https://support.microsoft.com/topic/b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8)  <br/> Ejecución del kernel compatible de Red Hat o Unbreakable Enterprise Kernel Release 3, 4 y 5 (UEK3, UEK4, UEK5)<br/><br/>8.1<br/>La ejecución en todos los kernels de UEK y RedHat kernel <= 3.10.0-1062.* se admiten en [9.35](https://support.microsoft.com/help/4573888/). La compatibilidad con el resto de los kernels de RedHat está disponible en [9.36](https://support.microsoft.com/help/4578241/).

> [!Note]
>- Para cada una de las versiones de Windows, Azure Site Recovery solo admite compilaciones del [canal de mantenimiento a largo plazo (LTSC)](/windows-server/get-started/servicing-channels-comparison#long-term-servicing-channel-ltsc).  Las versiones de [canal semianual](/windows-server/get-started/servicing-channels-comparison#semi-annual-channel) no se admiten en este momento.
>- Asegúrese de que, en el caso de las versiones de Linux, Azure Site Recovery no admita imágenes de SO personalizadas. Solo se admiten kernels de stock que forman parte del lanzamiento o la actualización de la versión secundaria de la distribución.

### <a name="ubuntu-kernel-versions"></a>Versiones de kernel de Ubuntu

**Versión compatible** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
14.04 LTS | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a), [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533), [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8), [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6), [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 3.13.0-24-generic a 3.13.0-170-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-148-generic,<br/>4.15.0-1023-azure a 4.15.0-1045-azure |
|||
16.04 LTS | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 4.4.0-21-generic a 4.4.0-206-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-140-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1111-azure|
16.04 LTS | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 4.4.0-21-generic a 4.4.0-206-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-140-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1111-azure|
16.04 LTS | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | 4.4.0-21-generic a 4.4.0-206-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-140-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1111-azure|
16.04 LTS | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | 4.4.0-21-generic a 4.4.0-201-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-133-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1106-azure|
16.04 LTS | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a) | 4.4.0-21-generic a 4.4.0-197-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-128-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1102-azure |
|||
18.04 LTS |[9.45](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) | 4.15.0-1123-azure </br> 5.4.0-1058-azure </br> 4.15.0-156-generic |
18.04 LTS | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 4.15.0-20-generic a 4.15.0-140-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-65-generic </br> 5.3.0-19-generic a 5.3.0-72-generic </br> 5.4.0-37-generic a 5.4.0-70-generic </br> 4.15.0-1009-azure a 4.15.0-1111-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1043-azure </br> 4.15.0-1114-azure </br> 4.15.0-143-generic </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 4.15.0-1115-azure </br> 4.15.0-144-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic </br> 4.15.0-1121-azure </br> 4.15.0-151-generic </br> 5.3.0-76-generic </br> 5.4.0-1055-azure </br> 5.4.0-80-generic </br> 4.15.0-147-generic </br> 4.15.0-153-generic </br> 5.4.0-1056-azure </br> 5.4.0-81-generic </br> 4.15.0-1122-azure </br> 4.15.0-154-generic |
18.04 LTS | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 4.15.0-20-generic a 4.15.0-140-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-65-generic </br> 5.3.0-19-generic a 5.3.0-72-generic </br> 5.4.0-37-generic a 5.4.0-70-generic </br> 4.15.0-1009-azure a 4.15.0-1111-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1043-azure </br> 4.15.0-1114-azure </br> 4.15.0-143-generic </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 4.15.0-1115-azure </br> 4.15.0-144-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic </br> 4.15.0-1121-azure </br> 4.15.0-151-generic </br> 5.3.0-76-generic </br> 5.4.0-1055-azure </br> 5.4.0-80-generic </br> 4.15.0-147-generic |
18.04 LTS |[9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | 4.15.0-20-generic a 4.15.0-140-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-65-generic </br> 5.3.0-19-generic a 5.3.0-72-generic </br> 5.4.0-37-generic a 5.4.0-70-generic </br> 4.15.0-1009-azure a 4.15.0-1111-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1043-azure </br> 4.15.0-1114-azure </br> 4.15.0-143-generic </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 4.15.0-1115-azure </br> 4.15.0-144-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic |
18.04 LTS | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | 4.15.0-20-generic a 4.15.0-135-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-65-generic </br> 5.3.0-19-generic a 5.3.0-70-generic </br> 5.4.0-37-generic a 5.4.0-59-generic</br> 5.4.0-60-generic a 5.4.0-65-generic </br> 4.15.0-1009-azure a 4.15.0-1106-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1039-azure|
18.04 LTS | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a) | 4.15.0-20-generic a 4.15.0-129-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-63-generic, </br> 5.3.0-19-generic a 5.3.0-69-generic, </br> 5.4.0-37-generic a 5.4.0-59-generic</br> 4.15.0-1009-azure a 4.15.0-1103-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1035-azure|
|||
20.04 LTS |[9.45](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) | 5.4.0-1058-azure </br> 5.4.0-84-generic |
20.04 LTS |[9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 5.4.0-26-generic a 5.4.0-80 </br> 5.4.0-1010-azure a 5.4.0-1048-azure </br> 5.4.0-81-generic </br> 5.4.0-1056-azure |
20.04 LTS |[9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 5.4.0-26-generic a 5.4.0-80 </br> 5.4.0-1010-azure a 5.4.0-1048-azure |
20.04 LTS |[9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8)| 5.4.0-26-generic a 5.4.0-60-generic </br> -generic 5.4.0-1010-azure a 5.4.0-1043-azure </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic |
20.04 LTS |[9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533)| 5.4.0-26-generic a 5.4.0-65 </br> -generic 5.4.0-1010-azure a 5.4.0-1039-azure |
20.04 LTS |[9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a)| 5.4.0-26-generic a 5.4.0-59 </br> -generic 5.4.0-1010-azure a 5.4.0-1035-azure |

**Nota: Para Ubuntu 20.04, inicialmente se había implementado la compatibilidad con los kernels 5.8.* , pero desde entonces hemos detectado problemas con dicha compatibilidad, por lo que hemos eliminado estos kernels de nuestra declaración de compatibilidad por el momento.

### <a name="debian-kernel-versions"></a>Versiones de kernel de Debian


**Versión compatible** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
Debian 7 | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a), [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533), [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8),[9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6), [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 3.2.0-4-amd64 a 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a), [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533), [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8), [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6), [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 3.16.0-4-amd64 a 3.16.0-11-amd64, 4.9.0-0.bpo.4-amd64 a 4.9.0-0.bpo.11-amd64 |
|||
Debian 9.1 | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 4.9.0-1-amd64 a 4.9.0-15-amd64 </br> 4.19.0-0.bpo.1-amd64 a 4.19.0-0.bpo.16-amd64 </br> 4.19.0-0.bpo.1-cloud-amd64 a 4.19.0-0.bpo.16-cloud-amd64 </br>
Debian 9.1 | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 4.9.0-1-amd64 a 4.9.0-15-amd64 </br> 4.19.0-0.bpo.1-amd64 a 4.19.0-0.bpo.16-amd64 </br> 4.19.0-0.bpo.1-cloud-amd64 a 4.19.0-0.bpo.16-cloud-amd64 </br>
Debian 9.1 | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | 4.9.0-1-amd64 a 4.9.0-15-amd64 </br> 4.19.0-0.bpo.1-amd64 a 4.19.0-0.bpo.16-amd64 </br> 4.19.0-0.bpo.1-cloud-amd64 a 4.19.0-0.bpo.16-cloud-amd64 </br>
Debian 9.1 | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | 4.9.0-1-amd64 a 4.9.0-14-amd64, </br> 4.19.0-0.bpo.1-amd64 a 4.19.0-0.bpo.14-amd64 </br> 4.19.0-0.bpo.1-cloud-amd64 a 4.19.0-0.bpo.14-cloud-amd64 </br>
Debian 9.1 | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a) | 4.9.0-1-amd64 a 4.9.0-14-amd64, </br> 4.19.0-0.bpo.1-amd64 a 4.19.0-0.bpo.13-amd64 </br> 4.19.0-0.bpo.1-cloud-amd64 a 4.19.0-0.bpo.13-cloud-amd64 </br>
|||
Debian 10 | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 4.19.0-6-amd64 a 4.19.0-16-amd64 </br> 4.19.0-6-cloud-amd64 a 4.19.0-16-cloud-amd64 </br> 5.8.0-0.bpo.2-amd64 </br> 5.8.0-0.bpo.2-cloud-amd64
Debian 10 | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 4.19.0-6-amd64 a 4.19.0-16-amd64 </br> 4.19.0-6-cloud-amd64 a 4.19.0-16-cloud-amd64 </br> 5.8.0-0.bpo.2-amd64 </br> 5.8.0-0.bpo.2-cloud-amd64
Debian 10 | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | 4.19.0-6-amd64 a 4.19.0-16-amd64 </br> 4.19.0-6-cloud-amd64 a 4.19.0-16-cloud-amd64 </br> 5.8.0-0.bpo.2-amd64 </br> 5.8.0-0.bpo.2-cloud-amd64
Debian 10 | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | 4.19.0-6-amd64 a 4.19.0-14-amd64 </br> 4.19.0-6-cloud-amd64 a 4.19.0-14-cloud-amd64 </br> 5.8.0-0.bpo.2-amd64 </br> 5.8.0-0.bpo.2-cloud-amd64
Debian 10 | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a) | 4.19.0-6-amd64 a 4.19.0-13-amd64 </br> 4.19.0-6-cloud-amd64 a 4.19.0-13-cloud-amd64 </br> 5.8.0-0.bpo.2-amd64 </br> 5.8.0-0.bpo.2-cloud-amd64

### <a name="suse-linux-enterprise-server-12-supported-kernel-versions"></a>Versiones de kernel admitidas de SUSE Linux Enterprise Server 12

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.65-azure </br> 4.12.14-16.68-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.65-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.56-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.44-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.38-azure|

### <a name="suse-linux-enterprise-server-15-supported-kernel-versions"></a>Versiones de kernel admitidas de SUSE Linux Enterprise Server 15

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2](https://www.suse.com/support/kb/doc/?id=000019587) en stock.</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> De 5.3.18-18.5-azure a 5.3.18-18.58-azure
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2](https://www.suse.com/support/kb/doc/?id=000019587) en stock.</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> De 5.3.18-18.5-azure a 5.3.18-18.58-azure
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> 5.3.18-18.5-azure a 5.3.18-18.47-azure
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> 5.3.18-18.5-azure a 5.3.18-18.35-azure
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2](https://www.suse.com/support/kb/doc/?id=000019587) en stock.</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> 5.3.18-18.5-azure a 5.3.18-18.29-azure

## <a name="linux-file-systemsguest-storage"></a>Sistemas de archivos Linux/almacenamiento de invitados

**Componente** | **Compatible**
--- | ---
Sistemas de archivos | ext3, ext4, XFS y BTRFS (condiciones aplicables según esta tabla)
Aprovisionamiento de administración de volúmenes lógicos (LVM)| Aprovisionamiento grueso: sí <br></br> Aprovisionamiento fino: no
Administrador de volúmenes | - Se admite LVM.<br/> - Se admite /boot en LVM desde el [paquete acumulativo de actualizaciones 31](https://support.microsoft.com/help/4478871/) (versión 9.20 de Mobility Service) y versiones posteriores. No se admite en versiones anteriores de Mobility Service.<br/> - No se admiten varios discos de sistema operativo.
Dispositivos de almacenamiento paravirtualizados | No se admiten dispositivos que se hayan exportado con controladores paravirtualizados.
Dispositivos de E/S de bloque multicola | No compatible.
Servidores físicos con controlador de almacenamiento HP CCISS | No compatible.
Convención de nomenclatura de punto de montaje/dispositivo | El nombre de dispositivo o el nombre de punto de montaje debe ser único.<br/> Asegúrese de que los nombres de dos dispositivos o puntos de montaje no solo se diferencien en las mayúsculas y minúsculas. Por ejemplo, no puede asignar a dispositivos de la misma máquina virtual los nombres *device1* y *Device1*.
Directorios | Si está ejecutando una versión de Mobility Service anterior a la versión 9.20 (publicada en el [paquete acumulativo de actualizaciones 31](https://support.microsoft.com/help/4478871/)) se aplicarán estas restricciones:<br/><br/> - Estos directorios (si se configuran como sistemas de archivos o particiones independientes) deben ubicarse en el mismo disco de sistema operativo del servidor de origen: /(root), /boot, /usr, /usr/local, /var, /etc.</br> - El directorio /boot debe estar en una partición de disco y no en un volumen de LVM.<br/><br/> De la versión 9.20 en adelante, estas restricciones no se aplican.
Directorio raíz | - Los discos de arranque no debe estar en formato de partición GPT. Se trata de una limitación de la arquitectura de Azure. Los discos GPT se admiten como discos de datos.<br/><br/> No se admiten varios discos de arranque en una VM.<br/><br/> - No se admite /boot en un volumen de LVM en más de un disco.<br/> - Una máquina sin un disco de arranque no se puede replicar.
Espacio en disco necesario| 2 GB en la partición /root <br/><br/> 250 MB en la carpeta de instalación
XFSv5 | Las características de XFSv5 en sistemas de archivos XFS, como la suma de comprobación de metadatos, se admiten a partir de la versión 9.10 de Mobility Service.<br/> Puede usar la utilidad xfs_info para comprobar el superbloque XFS de la partición. Si `ftype` está establecido en 1, entonces se usan las características de XFSv5.
BTRFS | Se admite BTRFS desde el [paquete acumulativo de actualizaciones 34](https://support.microsoft.com/help/4490016) (versión 9.22 de Mobility Service) y versiones posteriores. No se admite BTRFS si:<br/><br/> - El subvolumen del sistema de archivos BTRFS se cambia después de habilitar la protección.</br> - El sistema de archivos BTRFS está repartido entre varios discos.</br> - El sistema de archivos BTRFS es compatible con RAID.

## <a name="vmdisk-management"></a>Administración de máquinas virtuales o discos

**Acción** | **Detalles**
--- | ---
Cambio del tamaño del disco en la máquina virtual replicada (no se admite en arquitecturas en versión preliminar)| Se admite el aumento de tamaño de la máquina virtual de origen. No se admite la reducción de tamaño de la máquina virtual de origen. El cambio de tamaño debe realizarse antes de la conmutación por error, directamente en las propiedades de la máquina virtual. No es necesario deshabilitar o volver a habilitar la replicación.<br/><br/> Si cambia la máquina virtual de origen después de la conmutación por error, los cambios no se capturan.<br/><br/> Si cambia el tamaño del disco en la máquina virtual de Azure después de la conmutación por error, Site Recovery crea una nueva máquina virtual con las actualizaciones.
Agregar disco a una máquina virtual replicada | No compatible.<br/> Deshabilite la replicación para la máquina virtual, agregue el disco y vuelva a habilitar la replicación.

> [!NOTE]
> No se admite ningún cambio en la identidad del disco. Por ejemplo, si se ha cambiado la creación de particiones de disco de GPT a MBR, o viceversa, cambiará la identidad del disco. En este escenario, la replicación se interrumpirá y habrá que realizar una nueva configuración.
> En el caso de las máquinas Linux, no se admite el cambio de nombre de dispositivo, ya que afecta a la identidad del disco.
> En versión preliminar, no se admite cambiar el tamaño original del disco para reducirlo.

## <a name="network"></a>Red

**Componente** | **Compatible**
--- | ---
Formación de equipos de adaptador de red de la red de host | Se admite en máquinas virtuales de VMware. <br/><br/>No se admite en la replicación de máquinas físicas.
VLAN de la red de host | Sí.
IPv4 de la red de host | Sí.
IPv6 de la red de host | No.
Formación de equipos de adaptador de red de la red de invitado o de servidor | No.
IPv4 de la red de invitado o de servidor | Sí.
IPv6 de la red de invitado o de servidor | No.
IP estática de la red de invitado o de servidor (Windows) | Sí.
IP estática de la red de invitado o de servidor (Linux) | Sí. <br/><br/>Las máquinas virtuales se configuran para usar DHCP en la conmutación por recuperación.
Varios adaptadores de red de la red de invitado o de servidor | Sí.
Acceso de vínculo privado al servicio Site Recovery | Sí. [Más información](hybrid-how-to-enable-replication-private-endpoints.md). (no se admite en arquitecturas en versión preliminar)


## <a name="azure-vm-network-after-failover"></a>Red de máquinas virtuales de Azure (después de la conmutación por error)

**Componente** | **Compatible**
--- | ---
Azure ExpressRoute | Sí
ILB | Sí
ELB | Sí
Administrador de tráfico de Azure | Sí
Varias NIC | Sí
Dirección IP reservada | Sí
IPv4 | Sí
Conservar la dirección IP de origen | Sí
Punto de conexión de servicio de red virtual de Azure<br/> | Sí
Redes aceleradas | No

## <a name="storage"></a>Storage
**Componente** | **Compatible**
--- | ---
Disco dinámico | El disco del sistema operativo debe ser un disco básico. <br/><br/>Los discos de datos pueden ser discos dinámicos.
Configuración del disco de Docker | No
NFS de host | Sí para VMware<br/><br/> No para servidores físicos
SAN de host (ISCSI/FC) | Sí
vSAN de host | Sí para VMware<br/><br/> N/D para servidores físicos
Varias rutas de host (MPIO) | Sí, probado con DSM de Microsoft, EMC PowerPath 5.7 SP4, EMC PowerPath DSM para CLARiiON.
Hospedaje de volúmenes virtuales (VVols) | Sí para VMware<br/><br/> N/D para servidores físicos
VMDK de invitado/servidor | Sí
Disco de clúster compartido de invitado/servidor | No
Disco cifrado de invitado/servidor | No
Cifrado FIPS | No
NFS de invitado/servidor | No
Invitado/servidor iSCSI | Para migración: sí<br/>Para recuperación ante desastres: no, iSCSI realiza la conmutación por recuperación como un disco conectado a la máquina virtual
SMB 3.0 de invitado/servidor | No
RDM de invitado/servidor | Sí<br/><br/> N/D para servidores físicos
Disco de invitado/servidor > 1 TB | Sí, el disco debe ser de un tamaño superior a 1024 MB.<br/><br/>Hasta 32 767 GB al replicar en discos administrados (versión 9.41 en adelante)<br></br> Hasta 4095 GB al replicar en cuentas de almacenamiento
Disco de invitado/servidor con tamaño de sector físico de 4K y lógico de 4K | No
Disco de invitado/servidor con tamaño de sector físico de 512 bytes y lógico de 4K | No
Volumen de invitado/servidor con disco seccionado > 4 TB | Sí
Administración de volúmenes lógicos (LVM)| Aprovisionamiento grueso: sí <br></br> Aprovisionamiento fino: no
Invitado/servidor: espacios de almacenamiento | No
Invitado/servidor: interfaz NVMe | No
Invitado/servidor: adición/eliminación de disco en caliente | No
Invitado/servidor: disco de exclusión | Sí
Varias rutas (MPIO) de invitado/servidor | No
Particiones GPT de invitado/servidor | Se admiten cinco particiones desde el [paquete acumulativo de actualizaciones 37](https://support.microsoft.com/help/4508614/) (versión 9.25 de Mobility Service) y versiones posteriores. Antes se admitían cuatro.
ReFS | El sistema de archivos resistente es compatible con Mobility Service versión 9.23 o posterior
Arranque de EFI/UEFI de invitado/servidor | - Compatible con todos los [sistemas operativos de Azure Marketplace UEFI](../virtual-machines/generation-2.md#generation-2-vm-images-in-azure-marketplace) con la versión 9.30 del agente de movilidad de Site Recovery y posteriores. <br/> - No se admite el tipo de arranque seguro de UEFI. [Más información.](../virtual-machines/generation-2.md#on-premises-vs-azure-generation-2-vms)
Disco RAID| No

## <a name="replication-channels"></a>Canales de replicación

|**Tipos de replicación**   |**Compatible**  |
|---------|---------|
|Transferencias de datos descargados (ODX)    |       No  |
|Propagación sin conexión        |   No      |
| Azure Data Box | No

## <a name="azure-storage"></a>Almacenamiento de Azure

**Componente** | **Compatible**
--- | ---
Almacenamiento con redundancia local | Sí
Almacenamiento con redundancia geográfica | Sí
Almacenamiento con redundancia geográfica con acceso de lectura | Sí
Almacenamiento de acceso esporádico | No
Almacenamiento de acceso frecuente| No
Blobs en bloques | No
Cifrado en reposo (SSE)| Sí
Cifrado en reposo (CMK)| Sí (a través del módulo PowerShell Az 3.3.0 en adelante)
Cifrado de datos en reposo doble | Sí (con el módulo PowerShell Az 3.3.0 y versiones posteriores). Obtenga más información sobre las regiones admitidas para [Windows](../virtual-machines/disk-encryption.md) y [Linux](../virtual-machines/disk-encryption.md).
Premium Storage | Sí
Opción de transferencia segura | Sí
Servicio Import/Export | No
Firewalls de Azure Storage para redes virtuales | Sí.<br/> Configurados en la cuenta de almacenamiento o la cuenta de almacenamiento en caché de destino (se usa para almacenar los datos de replicación).
Cuentas de almacenamiento de uso general v2 (capas de acceso frecuente y esporádico) | Sí (los costos de transacción son sustancialmente más elevados para V2 en comparación con V1)

## <a name="azure-compute"></a>Azure Compute

**Característica** | **Compatible**
--- | ---
Conjuntos de disponibilidad | Sí. (no se admite en arquitecturas en versión preliminar)
Zonas de disponibilidad | No
CONCENTRADOR | Sí
Discos administrados | Sí

## <a name="azure-vm-requirements"></a>Requisitos de VM de Azure

Las máquinas virtuales locales que se replican en Azure deben cumplir con los requisitos de VM de Azure que se resumen en esta tabla. Cuando Site Recovery ejecuta una comprobación de requisitos previos para la replicación, se producirá un error si no se cumplen algunos de los requisitos.

**Componente** | **Requisitos** | **Detalles**
--- | --- | ---
Sistema operativo invitado | Compruebe los [sistemas operativos compatibles](#replicated-machines) con máquinas replicadas. | Se produce un error en la comprobación si no es compatible.
Arquitectura del sistema operativo invitado | 64 bits | Se produce un error en la comprobación si no es compatible.
Tamaño del disco del sistema operativo | Hasta 2048 GB para máquinas de generación 1. <br> Hasta 4095 GB para máquinas de generación 2. | Se produce un error en la comprobación si no es compatible.
Número de discos del sistema operativo | 1 </br> El arranque y la partición del sistema en discos diferentes no se admiten. | Se produce un error en la comprobación si no es compatible.
Número de discos de datos | 64 o menos | Se produce un error en la comprobación si no es compatible.
Tamaño del disco de datos | Hasta 32 767 GB al replicar en discos administrados (versión 9.41 en adelante)<br> Hasta 4095 GB al replicar en cuentas de almacenamiento </br> Requisito de tamaño mínimo de disco: al menos 1024 MB </br> La arquitectura en versión preliminar admite discos de hasta 8 TB.  | Se produce un error en la comprobación si no es compatible.
RAM | El controlador de ASR consume un 6 % de RAM.
Adaptadores de red | Se admiten varios adaptadores. |
VHD compartido | No compatible. | Se produce un error en la comprobación si no es compatible.
Disco FC | No compatible. | Se produce un error en la comprobación si no es compatible.
BitLocker | No compatible. | Debe deshabilitar BitLocker antes de habilitar la replicación de una máquina. |
Nombre de la máquina virtual | Entre 1 y 63 caracteres.<br/><br/> Restringido a letras, números y guiones.<br/><br/> El nombre de la máquina debe empezar y terminar con una letra o un número. |  Actualice el valor de las propiedades de la máquina en Site Recovery.

## <a name="resource-group-limits"></a>Límites de los grupos de recursos

Para comprender el número de máquinas virtuales que se pueden proteger en un único grupo de recursos, consulte el artículo sobre [los límites y las cuotas de la suscripción](../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits).

## <a name="churn-limits"></a>Límites de renovación

En la tabla siguiente se proporcionan los límites de Azure Site Recovery.
- Estos límites se basan en nuestras pruebas, pero no cubren todas las combinaciones de E/S posibles de la aplicación.
- Los resultados reales pueden variar en función de la combinación de E/S de la aplicación.
- Para obtener mejores resultados, es muy recomendable que ejecute la [herramienta Deployment Planner](site-recovery-deployment-planner.md) y realice pruebas de la aplicación de forma exhaustiva mediante una conmutación por error de prueba para obtener una imagen real del rendimiento de la aplicación.

**Destino de replicación** | **Tamaño medio de E/S de disco de origen** |**Actividad de datos media de disco de origen** | **Actividad de datos de disco de origen total por día**
---|---|---|---
Standard Storage | 8 KB    | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 8 KB    | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 16 KB | 4 MB/s |    336 GB por disco
Disco Premium P10 o P15 | 32 KB, o más | 8 MB/s | 672 GB por disco
Disco Premium P20, P30, P40 o P50 | 8 KB    | 5 MB/s | 421 GB por disco
Disco Premium P20, P30, P40 o P50 | 16 KB, o más |20 MB/s | 1684 GB por disco


**Actividad de datos de origen** | **Límite máximo**
---|---
Actividad de datos máxima entre todos los discos de una máquina virtual | 54 MB/s
Actividad de datos máxima por día admitida por un servidor de procesos | 2 TB

- Estos son los números promedio si la superposición de E/S es del 30 %.
- Site Recovery es capaz de controlar un mayor rendimiento en función de la relación de superposición, tamaños de escritura mayores y el comportamiento real de E/S de la carga de trabajo.
- Estos números asumen un trabajo pendiente típico de aproximadamente cinco minutos. Es decir, una vez que se cargan los datos, se procesan y se crea un punto de recuperación en menos de cinco minutos.

## <a name="storage-account-limits"></a>Límites de la cuenta de almacenamiento

A medida que aumenta el promedio de renovación en los discos, se reduce el número de discos que una cuenta de almacenamiento puede admitir. La tabla siguiente puede usarse como guía para tomar decisiones sobre el número de cuentas de almacenamiento que se deben aprovisionar.

**Tipo de cuenta de almacenamiento**    |    **Renovación = 4 MBps por disco**    |    **Renovación = 8 MBps por disco**
---    |    ---    |    ---
Cuenta de almacenamiento V1    |    600 discos    |    300 discos
Cuenta de almacenamiento V2    |    1500 discos    |    750 discos

Tenga en cuenta que los límites anteriores solo se aplican a escenarios híbridos de recuperación ante desastres.

## <a name="vault-tasks"></a>Tareas de almacén

**Acción** | **Compatible**
--- | ---
Mover el almacén entre grupos de recursos | No
Mueva el almacén dentro de las suscripciones y entre ellas | No
Mover el almacenamiento, la red y las máquinas virtuales de Azure entre grupos de recursos | No
Migre el almacenamiento, la red y las VM de Azure dentro de las suscripciones y entre ellas. | No


## <a name="obtain-latest-components"></a>Obtenga los componentes más recientes

**Nombre** | **Descripción** | **Detalles**
--- | --- | ---
Servidor de configuración | Instalado en el entorno local.<br/> Coordina las comunicaciones entre servidores o máquinas físicas de VMware locales y Azure. | - [Más información](vmware-physical-azure-config-process-server-overview.md) acerca del servidor de configuración.<br/> - [Más información](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) acerca de la actualización a la versión más reciente.<br/> - [Más información](vmware-azure-deploy-configuration-server.md) acerca de la configuración del servidor de configuración.
Servidor de proceso | Se instala de forma predeterminada en el servidor de configuración.<br/> Recibe datos de replicación, los optimiza con almacenamiento en caché, compresión y cifrado y los envía a Azure.<br/> A medida que crece la implementación, puede agregar más servidores de procesos para controlar mayores volúmenes de tráfico de replicación. | - [Más información](vmware-physical-azure-config-process-server-overview.md) acerca del servidor de procesos.<br/> - [Más información](vmware-azure-manage-process-server.md#upgrade-a-process-server) acerca de la actualización a la versión más reciente.<br/> - [Más información](vmware-physical-large-deployment.md#set-up-a-process-server) acerca de la configuración de servidores de procesos de escalado horizontal.
Mobility Service | Se instala en cada máquina virtual de VMware o servidor físico que se va a replicar.<br/> Coordina la replicación entre servidores de VMware locales o servidores físicos y Azure.| - [Más información](vmware-physical-mobility-service-overview.md) acerca de Mobility Service.<br/> - [Más información](vmware-physical-manage-mobility-service.md#update-mobility-service-from-azure-portal) acerca de la actualización a la versión más reciente.<br/>



## <a name="next-steps"></a>Pasos siguientes
[Aprenda](tutorial-prepare-azure.md) a preparar Azure para la recuperación ante desastres de las máquinas virtuales de VMware.

[9.32 UR]: https://support.microsoft.com/en-in/help/4538187/update-rollup-44-for-azure-site-recovery
[9.31 UR]: https://support.microsoft.com/en-in/help/4537047/update-rollup-43-for-azure-site-recovery
[9.30 UR]: https://support.microsoft.com/en-in/help/4531426/update-rollup-42-for-azure-site-recovery
[9.29 UR]: https://support.microsoft.com/en-in/help/4528026/update-rollup-41-for-azure-site-recovery
[9.28 UR]: https://support.microsoft.com/en-in/help/4521530/update-rollup-40-for-azure-site-recovery
[9.27 UR]: https://support.microsoft.com/en-in/help/4517283/update-rollup-39-for-azure-site-recovery
[9.26 UR]: https://support.microsoft.com/en-in/help/4513507/update-rollup-38-for-azure-site-recovery
[9.25 UR]: https://support.microsoft.com/en-in/help/4508614/update-rollup-37-for-azure-site-recovery
[9.24 UR]: https://support.microsoft.com/en-in/help/4503156
[9.23 UR]: https://support.microsoft.com/en-in/help/4494485/update-rollup-35-for-azure-site-recovery
[9.22 UR]: https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery
[9.21 UR]: https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery
[9.20 UR]: https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery
[9.19 UR]: https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30
[9.18 UR]: https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery
