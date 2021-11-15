---
title: Matriz de soporte para la recuperación ante desastres de máquinas virtuales de Azure con Azure Site Recovery
description: Resume la compatibilidad con la recuperación ante desastres de máquinas virtuales de Azure en una región secundaria con Azure Site Recovery.
ms.topic: article
ms.date: 11/29/2020
author: Sharmistha-Rai
ms.author: sharrai
ms.openlocfilehash: 64197f1ef9c2797f6d021ab7ca8dea6e7bc05995
ms.sourcegitcommit: 1a0fe16ad7befc51c6a8dc5ea1fe9987f33611a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131867273"
---
# <a name="support-matrix-for-azure-vm-disaster-recovery-between-azure-regions"></a>Matriz de soporte para la recuperación ante desastres de máquinas virtuales de Azure entre regiones de Azure

En este artículo se resumen los requisitos previos y la compatibilidad para la recuperación ante desastres de máquinas virtuales de Azure de una región de Azure a otra mediante el servicio [Azure Site Recovery](site-recovery-overview.md).


## <a name="deployment-method-support"></a>Compatibilidad de método de implementación

**Implementación** |  **Soporte técnico**
--- | ---
**Azure Portal** | Compatible.
**PowerShell** | Compatible. [Más información](azure-to-azure-powershell.md)
**REST API** | Compatible.
**CLI** | No se admite actualmente.


## <a name="resource-support"></a>Compatibilidad de recursos

**Acción de recursos** | **Detalles**
--- | ---
**Mover almacenes entre grupos de recursos** | No compatible
**Mover recursos de proceso, almacenamiento y red entre grupos de recursos** | No compatible.<br/><br/> Si mueve una máquina virtual o los componentes asociados, como el almacenamiento o la red, después de la replicación de esta, tendrá que deshabilitar la replicación y volver a habilitarla para la máquina virtual.
**Replicar máquinas virtuales de Azure de una suscripción a otra para la recuperación ante desastres** | Se admite en el mismo inquilino de Azure Active Directory.
**Migración de máquinas virtuales entre regiones dentro de los clústeres geográficos admitidos (dentro y entre suscripciones)** | Se admite en el mismo inquilino de Azure Active Directory.
**Migrar máquinas virtuales dentro de la misma región** | No compatible.
**Hosts dedicados de Azure** | No compatible.

## <a name="region-support"></a>Regiones admitidas

Azure Site Recovery permite realizar la recuperación ante desastres global. Puede replicar y recuperar las VM entre dos regiones de Azure cualesquiera del mundo. Si le preocupa la soberanía de los datos, puede optar por limitar la replicación en su clúster geográfico específico. Los distintos clústeres geográficos son los siguientes:

**Clúster geográfico** | **Regiones de Azure**
-- | --
America | Este de Canadá, Centro de Canadá, Centro-sur de EE. UU., Centro-oeste de EE. UU., Este de EE. UU., Este de EE. UU. 2, Oeste de EE. UU., Oeste de EE. UU. 2, Oeste de EE. UU. 3, Centro de EE. UU., Centro-norte de EE. UU.
Europa | Oeste de Reino Unido, Sur de Reino Unido, Norte de Europa, Oeste de Europa, Oeste de Sudáfrica, Norte de Sudáfrica, Este de Noruega, Centro de Francia, Norte de Suiza, Centro-oeste de Alemania, Norte de Emiratos Árabes Unidos, Centro de Emiratos Árabes Unidos (Emiratos Árabes Unidos se trata como parte del clúster geográfico de Europa)
Asia | Sur de la India, Centro de la India, India occidental, Sudeste de Asia, Este de Asia, Este de Japón, Oeste de Japón, Centro de Corea del Sur, Sur de Corea del Sur
JIO | JIO del Oeste de la India
Australia    | Este de Australia, Sudeste de Australia, Centro de Australia, Centro de Australia 2
Azure Government    | Virginia Gob. EE. UU., Iowa Gob. EE. UU., US Gov de Arizona, US Gov de Texas, US DOD este, US DOD central
Alemania    | Centro de Alemania y Nordeste de Alemania
China | Este de China, Norte de China, Norte de China 2 y Este de China 2
Brasil | Sur de Brasil
Regiones restringidas reservadas para la recuperación ante desastres dentro de un país |Oeste de Suiza reservado para el Norte de Suiza, Sur de Francia reservado para el Centro de Francia, Oeste de Noruega para clientes del Este de Noruega; JIO del Centro de la India para clientes de JIO del Oeste de la India, Sudeste de Brasil para clientes del Sur de Brasil, Oeste de Sudáfrica para clientes del Norte de Sudáfrica, Norte de Alemania para clientes de Centro-oeste de Alemania.

>[!NOTE]
>
> - En la región **Sur de Brasil**, puede replicar y conmutar por error en las siguientes regiones: Sudeste de Brasil, Centro-sur de EE. UU., Centro-oeste de EE. UU., Este de EE. UU., Este de EE. UU. 2, Oeste de EE. UU., Oeste de EE. UU. 2 y Centro-norte de EE. UU.
> - La región Sur de Brasil solo puede utilizarse como la región de origen desde la que las máquinas virtuales replican mediante Site Recovery. No puede ser una región de destino. Tenga en cuenta que si realiza la conmutación por error desde Sur de Brasil como región de origen a un destino, se admite la conmutación por recuperación a Sur de Brasil desde la región de destino. El Sudeste de Brasil solo se puede usar como región de destino.
> - Si la región donde quiere crear un almacén no aparece, asegúrese de que su suscripción tiene acceso para crear recursos en esa región.
> - Si no ve una región dentro de un clúster geográfico al habilitar la replicación, asegúrese de que su suscripción tiene permisos para crear máquinas virtuales en dicha región.

## <a name="cache-storage"></a>Almacenamiento en caché

Esta tabla resume la compatibilidad con la cuenta de almacenamiento de la memoria caché que usa Site Recovery durante la replicación.

**Configuración** | **Soporte técnico** | **Detalles**
--- | --- | ---
Cuentas de almacenamiento de uso general V2 (capas de acceso frecuente y esporádico) | Compatible | Se desaconseja usar GPv2, ya que los costes de las transacciones de V2 son significativamente más altos que los de las cuentas de almacenamiento de V1.
Premium Storage | No compatible | Las cuentas de almacenamiento estándar se usan para el almacenamiento en caché, para ayudar a optimizar los costos.
Region |  Misma región que la máquina virtual  | La cuenta de almacenamiento en caché debe estar en la misma región que la máquina virtual que se protege.
Subscription  | Puede ser diferente de las máquinas virtuales de origen | La cuenta de almacenamiento en caché no debe estar en la misma suscripción que las máquinas virtuales de origen.
Firewalls de Azure Storage para redes virtuales  | Compatible | Si usa una cuenta de almacenamiento en caché o de destino con firewall habilitado, asegúrese de seleccionar ["Permitir servicios de Microsoft de confianza"](../storage/common/storage-network-security.md#exceptions).<br></br>Además, asegúrese de permitir el acceso a al menos una subred de Vnet de origen.<br></br>Nota: No restrinja el acceso de red virtual a las cuentas de almacenamiento que usa para Site Recovery. Debe permitir el acceso desde "Todas las redes".
Eliminación temporal | No compatible | No se admite la eliminación temporal porque, una vez habilitada en la cuenta de almacenamiento en caché, aumenta el costo. ASR realiza creaciones o eliminaciones muy frecuentes de archivos de registro mientras se replican, lo que provoca un aumento de los costos.

En la tabla siguiente se muestran los límites en el número de discos que se pueden replicar en una sola cuenta de almacenamiento.

**Tipo de cuenta de almacenamiento**    |    **Renovación = 4 MBps por disco**    |    **Renovación = 8 MBps por disco**
---    |    ---    |    ---
Cuenta de almacenamiento V1    |    300 discos    |    150 discos
Cuenta de almacenamiento V2    |    750 discos    |    375 discos

A medida que aumenta el promedio de renovación en los discos, se reduce el número de discos que una cuenta de almacenamiento puede admitir. La tabla anterior puede usarse como guía para tomar decisiones sobre el número de cuentas de almacenamiento que se deben aprovisionar.

Tenga en cuenta que los límites anteriores son específicos de escenarios de recuperación ante desastres de Azure a Azure y de zona a zona.

## <a name="replicated-machine-operating-systems"></a>Sistemas operativos de máquinas replicadas

Site Recovery admite la replicación de máquinas virtuales de Azure que ejecutan los sistemas operativos enumerados en esta sección. Tenga en cuenta que si una máquina que ya se está replicando se actualiza posteriormente (o se cambia a una versión anterior) a un kernel principal diferente, deberá deshabilitar la replicación y volverla a habilitar después de la actualización.

### <a name="windows"></a>Windows


**Sistema operativo** | **Detalles**
--- | ---
Windows Server 2019 | Se admite para Server Core y Server con Experiencia de escritorio.
Windows Server 2016  | Se admite para Server Core y Server con Experiencia de escritorio
Windows Server 2012 R2 | Compatible.
Windows Server 2012 | Compatible.
Windows Server 2008 R2 con SP1/SP2 | Compatible.<br/><br/> Desde la versión [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) de la extensión de servicio de Mobility Service para máquinas virtuales de Azure, necesita instalar una [actualización de la pila de servicio (SSU)](https://support.microsoft.com/help/4490628) de Windows y la [actualización de SHA-2](https://support.microsoft.com/help/4474419) en las máquinas que ejecutan Windows Server 2008 R2 SP1/SP2.  SHA-1 no se admite desde septiembre de 2019 y, si la firma de código SHA-2 no está habilitada, la extensión del agente no se instalará ni actualizará según lo previsto. Más información sobre los [requisitos y la actualización de SHA-2](https://aka.ms/SHA-2KB).
Windows 10 (x64) | Compatible.
Windows 8.1 (x64) | Compatible.
Windows 8 (x64) | Compatible.
Windows 7 (x64) con SP1 en adelante | Desde la versión [9.30](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) de la extensión de servicio de Mobility Service para máquinas virtuales de Azure, necesita instalar una [actualización de la pila de servicio (SSU)](https://support.microsoft.com/help/4490628) de Windows y la [actualización de SHA-2](https://support.microsoft.com/help/4474419) en las máquinas que ejecutan Windows 7 con SP1.  SHA-1 no se admite desde septiembre de 2019 y, si la firma de código SHA-2 no está habilitada, la extensión del agente no se instalará ni actualizará según lo previsto. Más información sobre los [requisitos y la actualización de SHA-2](https://aka.ms/SHA-2KB).



#### <a name="linux"></a>Linux

**Sistema operativo** | **Detalles**
--- | ---
Red Hat Enterprise Linux | 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6,[7.7](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery), [7.8](https://support.microsoft.com/help/4564347/), [7.9](https://support.microsoft.com/help/4578241/), [8.0](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery), 8.1, [8.2](https://support.microsoft.com/help/4570609/), [8.3](https://support.microsoft.com/help/4597409/)
CentOS | 6.5, 6.6, 6.7, 6.8, 6.9, 6.10 </br> 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7, [7.8](https://support.microsoft.com/help/4564347/), [versión 7.9 anterior a la disponibilidad general](https://support.microsoft.com/help/4578241/), la versión 7.9 anterior a la disponibilidad general se admite a partir de la revisión urgente 9.37** </br> 8.0, 8.1, [8.2](https://support.microsoft.com/help/4570609), [8.3](https://support.microsoft.com/help/4597409/)
Servidor Ubuntu 14.04 LTS | Incluye compatibilidad con todas las versiones 14.04.*x*; [versiones de kernel compatibles](#supported-ubuntu-kernel-versions-for-azure-virtual-machines).
Servidor Ubuntu 16.04 LTS | Incluye compatibilidad con todas las versiones 16.04.*x*; [versión de kernel compatible](#supported-ubuntu-kernel-versions-for-azure-virtual-machines).<br/><br/> Los servidores Ubuntu que usan un inicio de sesión y una autenticación mediante contraseña y que usan el paquete cloud-init para configurar máquinas virtuales en la nube pueden tener el inicio de sesión mediante contraseña deshabilitado tras una conmutación por error (en función de la configuración de cloudinit). Para volver a habilitar el inicio de sesión mediante contraseña en la máquina virtual, restablezca la contraseña desde el menú Soporte > Solución de problemas > Configuración (de la máquina virtual conmutada por error en Azure Portal).
Servidor Ubuntu 18.04 LTS | Incluye compatibilidad con todas las versiones 18.04.*x*; [versión de kernel compatible](#supported-ubuntu-kernel-versions-for-azure-virtual-machines).<br/><br/> Los servidores Ubuntu que usan un inicio de sesión y una autenticación mediante contraseña y que usan el paquete cloud-init para configurar máquinas virtuales en la nube pueden tener el inicio de sesión mediante contraseña deshabilitado tras una conmutación por error (en función de la configuración de cloudinit). Para volver a habilitar el inicio de sesión mediante contraseña en la máquina virtual, restablezca la contraseña desde el menú Soporte > Solución de problemas > Configuración (de la máquina virtual conmutada por error en Azure Portal).
Servidor Ubuntu 20.04 LTS | Incluye compatibilidad con todas las versiones 20.04.*x*; [versión de kernel compatible](#supported-ubuntu-kernel-versions-for-azure-virtual-machines).
Debian 7 | Incluye compatibilidad para todas las versiones 7. *x*). [Versiones de kernel admitidas](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | Incluye compatibilidad para todas las versiones 8. *x*). [Versiones de kernel admitidas](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 9 | Incluye compatibilidad con las versiones 9.1 a 9.13. Debian 9.0 es incompatible. [Versiones de kernel admitidas](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 10 | [Versiones de kernel admitidas](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1, SP2, SP3, SP4, SP5 [(versiones de kernel admitidas)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 15 | 15, SP1, SP2[(versiones kernel admitidas)](#supported-suse-linux-enterprise-server-15-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> No se admite la actualización de máquinas de replicación de SP3 a SP4. Si se ha actualizado una máquina replicada, deberá deshabilitar la replicación y volverla a habilitar después de la actualización.
SUSE Linux Enterprise Server 11 | SP4
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, [7.7](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery), [7.8](https://support.microsoft.com/help/4573888/), [7.9](https://support.microsoft.com/help/4597409), [8.0](https://support.microsoft.com/help/4573888/), [8.1](https://support.microsoft.com/help/4573888/), [8.2](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8), [8.3](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) (ejecución del kernel compatible de Red Hat o Unbreakable Enterprise Kernel Release 3, 4, 5 y 6 (UEK3, UEK4, UEK5 y UEK6)<br/><br/>8.1 (se admite su ejecución en todos los kernels de UEK y en el kernel de RedHat<= 3.10.0-1062.* en la versión [9.35](https://support.microsoft.com/help/4573888/). La compatibilidad con el resto de los kernels de RedHat está disponible en la versión [9.36](https://support.microsoft.com/help/4578241/)).

> [!NOTE]
> En las versiones de Linux, Azure Site Recovery no admite kernels de SO personalizados. Solo se admiten kernels de stock que forman parte del lanzamiento o la actualización de la versión secundaria de la distribución.

**Nota: Para admitir los kernels de Linux más recientes en un plazo de 15 días desde su publicación, Azure Site Recovery implementa la revisión de corrección urgente sobre la versión más reciente del agente de movilidad. Esta corrección se implementa entre dos versiones principales. Para actualizar a la versión más reciente del agente de movilidad (incluida la revisión de corrección urgente), siga los pasos que se mencionan en [este artículo](service-updates-how-to.md#azure-vm-disaster-recovery-to-azure). Esta revisión está implementada actualmente para los agentes de movilidad que se usan en Azure para el escenario de recuperación ante desastres de Azure.

#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Versiones de kernel de Ubuntu admitidas para máquinas virtuales de Azure

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
14.04 LTS | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a), [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533), [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8), [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6), [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 3.13.0-24-generic a 3.13.0-170-generic,<br/>3.16.0-25-generic a 3.16.0-77-generic,<br/>3.19.0-18-generic a 3.19.0-80-generic,<br/>4.2.0-18-generic a 4.2.0-42-generic,<br/>4.4.0-21-generic a 4.4.0-148-generic,<br/>4.15.0-1023-azure a 4.15.0-1045-azure |
|||
16.04 LTS | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 4.4.0-21-generic a 4.4.0-206-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-140-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1111-azure|
16.04 LTS | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 4.4.0-21-generic a 4.4.0-206-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-140-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1111-azure|
16.04 LTS | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | 4.4.0-21-generic a 4.4.0-206-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-140-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1111-azure|
16.04 LTS | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | 4.4.0-21-generic a 4.4.0-201-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-133-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1106-azure <br/> 4.4.0-203-generic, 4.4.0-204-generic, 4.4.0-206-generic, 4.15.0-136-generic, 4.15.0-137-generic, 4.15.0-139-generic, 4.15.0-140-generic, 4.15.0-1108-azure, 4.15.0-1109-azure, 4.15.0-1110-azure, 4.15.0-1111-azure mediante la revisión urgente 9.41**|
16.04 LTS | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a) | 4.4.0-21-generic a 4.4.0-197-generic,<br/>4.8.0-34-generic a 4.8.0-58-generic,<br/>4.10.0-14-generic a 4.10.0-42-generic,<br/>4.11.0-13-generic to 4.11.0-14-generic,<br/>4.13.0-16-generic a 4.13.0-45-generic,<br/>4.15.0-13-generic a 4.15.0-128-generic<br/>4.11.0-1009-azure to 4.11.0-1016-azure,<br/>4.13.0-1005-azure a 4.13.0-1018-azure <br/>4.15.0-1012-azure a 4.15.0-1102-azure </br> 4.15.0-132-generic, 4.4.0-200-generic, 4.15.0-1106-azure, 4.15.0-133-generic, 4.4.0-201-generic mediante la revisión urgente 9.40**|
|||
18.04 LTS |[9.45](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) | 4.15.0-1123-azure </br> 5.4.0-1058-azure </br> 4.15.0-156-generic </br> 4.15.0-1125-azure </br> 4.15.0-161-generic </br> 5.4.0-1061-azure </br> 5.4.0-1062-azure </br> 5.4.0-89-generic |
18.04 LTS | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 4.15.0-20-generic a 4.15.0-140-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-65-generic </br> 5.3.0-19-generic a 5.3.0-72-generic </br> 5.4.0-37-generic a 5.4.0-70-generic </br> 4.15.0-1009-azure a 4.15.0-1111-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1043-azure </br> 4.15.0-1114-azure </br> 4.15.0-143-generic </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 4.15.0-1115-azure </br> 4.15.0-144-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic </br> 4.15.0-1121-azure </br> 4.15.0-151-generic </br> 4.15.0-153-generic </br> 5.3.0-76-generic </br> 5.4.0-1055-azure </br> 5.4.0-80-generic </br> 4.15.0-147-generic </br> 4.15.0-153-generic </br> 5.4.0-1056-azure </br> 5.4.0-81-generic </br> 4.15.0-1122-azure </br> 4.15.0-154-generic |
18.04 LTS | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 4.15.0-20-generic a 4.15.0-140-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-65-generic </br> 5.3.0-19-generic a 5.3.0-72-generic </br> 5.4.0-37-generic a 5.4.0-70-generic </br> 4.15.0-1009-azure a 4.15.0-1111-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1043-azure </br> 4.15.0-1114-azure </br> 4.15.0-143-generic </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 4.15.0-1115-azure </br> 4.15.0-144-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic </br> 4.15.0-1121-azure </br> 4.15.0-151-generic </br> 4.15.0-153-generic </br> 5.3.0-76-generic </br> 5.4.0-1055-azure </br> 5.4.0-80-generic </br> 4.15.0-147-generic |
18.04 LTS |[9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | 4.15.0-20-generic a 4.15.0-140-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-65-generic </br> 5.3.0-19-generic a 5.3.0-72-generic </br> 5.4.0-37-generic a 5.4.0-70-generic </br> 4.15.0-1009-azure a 4.15.0-1111-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1043-azure </br> 4.15.0-1114-azure </br> 4.15.0-143-generic </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 4.15.0-1115-azure </br> 4.15.0-144-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic |
18.04 LTS | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | 4.15.0-20-generic a 4.15.0-135-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-65-generic </br> 5.3.0-19-generic a 5.3.0-70-generic </br> 5.4.0-37-generic a 5.4.0-59-generic</br> 5.4.0-60-generic a 5.4.0-65-generic </br> 4.15.0-1009-azure a 4.15.0-1106-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1039-azure </br> 4.15.0-136-generic, 4.15.0-137-generic, 4.15.0-139-generic, 4.15.0-140-generic, 5.3.0-72-generic, 5.4.0-66-generic, 5.4.0-67-generic, 5.4.0-70-generic, 4.15.0-1108-azure, 4.15.0-1111-azure, 5.4.0-1040-azure, 5.4.0-1041-azure, 5.4.0-1043-azure, 4.15.0-1109-azure, 4.15.0-1110-azure mediante la revisión urgente 9.41**|
18.04 LTS | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a) | 4.15.0-20-generic a 4.15.0-129-generic </br> De 4.18.0-13-generic a 4.18.0-25-generic </br> 5.0.0-15-generic a 5.0.0-63-generic, </br> 5.3.0-19-generic a 5.3.0-69-generic, </br> 5.4.0-37-generic a 5.4.0-59-generic</br> 4.15.0-1009-azure a 4.15.0-1103-azure </br> De 4.18.0-1006-azure a 4.18.0-1025-azure </br> 5.0.0-1012-azure a 5.0.0-1036-azure </br> 5.3.0-1007-azure a 5.3.0-1035-azure </br> 5.4.0-1020-azure a 5.4.0-1035-azure </br> 4.15.0-1104-azure, 4.15.0-130-generic, 4.15.0-132-generic, 5.4.0-1036-azure, 5.4.0-60-generic, 5.4.0-62-generic, 4.15.0-1106-azure, 4.15.0-134-generic, 4.15.0-135-generic, 5.4.0-1039-azure, 5.4.0-64-generic, 5.4.0-65-generic mediante la revisión urgente 9.40**|
|||
20.04 LTS |[9.45](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) | 5.4.0-1058-azure </br> 5.4.0-84-generic </br> 5.4.0-1061-azure </br> 5.4.0-1062-azure </br> 5.4.0-89-generic |
20.04 LTS |[9.44](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 5.4.0-26-generic a 5.4.0-60-generic </br> 5.4.0-1010-azure a 5.4.0-1043-azure </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic </br> 5.4.0-81-generic </br> 5.4.0-1056-azure |
20.04 LTS |[9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 5.4.0-26-generic a 5.4.0-60-generic </br> 5.4.0-1010-azure a 5.4.0-1043-azure </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic |
20.04 LTS |[9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8)| 5.4.0-26-generic a 5.4.0-60-generic </br> 5.4.0-1010-azure a 5.4.0-1043-azure </br> 5.4.0-1047-azure </br> 5.4.0-73-generic </br> 5.4.0-1048-azure </br> 5.4.0-74-generic |
20.04 LTS |[9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533)| 5.4.0-26-generic a 5.4.0-65-generic </br> 5.4.0-1010-azure a 5.4.0-1039-azure </br> 5.4.0-66-generic, 5.4.0-67-generic, 5.4.0-70-generic, 5.4.0-1040-azure, 5.4.0-1041-azure, 5.4.0-1043-azure mediante la revisión urgente 9.41**|
20.04 LTS |[9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a)| 5.4.0-26-generic a 5.4.0-59-generic </br> 5.4.0-1010-azure a 5.4.0-1035-azure </br> 5.4.0-1036-azure, 5.4.0-60-generic, 5.4.0-62-generic, 5.4.0-1039-azure, 5.4.0-64-generic, 5.4.0-65-generic |

**Nota: Para admitir los kernels de Linux más recientes en un plazo de 15 días desde su publicación, Azure Site Recovery implementa la revisión de corrección urgente sobre la versión más reciente del agente de movilidad. Esta corrección se implementa entre dos versiones principales. Para actualizar a la versión más reciente del agente de movilidad (incluida la revisión de corrección urgente), siga los pasos que se mencionan en [este artículo](service-updates-how-to.md#azure-vm-disaster-recovery-to-azure). Esta revisión está implementada actualmente para los agentes de movilidad que se usan en Azure para el escenario de recuperación ante desastres de Azure.

**Nota: Para Ubuntu 20.04, inicialmente se había implementado la compatibilidad con los kernels 5.8.* , pero desde entonces hemos detectado problemas con dicha compatibilidad, por lo que hemos eliminado estos kernels de nuestra declaración de compatibilidad por el momento.

#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Versiones de kernel de Debian admitidas para máquinas virtuales de Azure

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
Debian 7 | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a), [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533), [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8), [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6), [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 3.2.0-4-amd64 a 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a), [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533), [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8), [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6), [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 3.16.0-4-amd64 a 3.16.0-11-amd64, 4.9.0-0.bpo.4-amd64 a 4.9.0-0.bpo.11-amd64 |
|||
Debian 9.1 | [9.45](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) | 4.19.0-0.bpo.18-amd64 </br> 4.19.0-0.bpo.18-cloud-amd64
Debian 9.1 | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 4.9.0-1-amd64 a 4.9.0-15-amd64 </br> 4.19.0-0.bpo.1-amd64 a 4.19.0-0.bpo.16-amd64 </br> 4.19.0-0.bpo.1-cloud-amd64 a 4.19.0-0.bpo.16-cloud-amd64
Debian 9.1 | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 4.9.0-1-amd64 a 4.9.0-15-amd64 </br> 4.19.0-0.bpo.1-amd64 a 4.19.0-0.bpo.16-amd64 </br> 4.19.0-0.bpo.1-cloud-amd64 a 4.19.0-0.bpo.16-cloud-amd64
Debian 9.1 | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | 4.9.0-1-amd64 a 4.9.0-15-amd64 </br> 4.19.0-0.bpo.1-amd64 a 4.19.0-0.bpo.16-amd64 </br> 4.19.0-0.bpo.1-cloud-amd64 a 4.19.0-0.bpo.16-cloud-amd64
Debian 9.1 | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | 4.9.0-1-amd64 a 4.9.0-14-amd64, </br> 4.19.0-0.bpo.1-amd64 a 4.19.0-0.bpo.14-amd64 </br> 4.19.0-0.bpo.1-cloud-amd64 a 4.19.0-0.bpo.14-cloud-amd64 </br> 4.9.0-15-amd64, 4.19.0-0.bpo.16-amd64, 4.19.0-0.bpo.16-cloud-amd64 mediante la revisión urgente 9.41**
|||
Debian 10 | [9.45](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) | 4.19.0-18-amd64 </br> 4.19.0-18-cloud-amd64
Debian 10 | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 4.19.0-6-amd64 a 4.19.0-16-amd64 </br> 4.19.0-6-cloud-amd64 a 4.19.0-16-cloud-amd64 </br> 5.8.0-0.bpo.2-amd64 </br> 5.8.0-0.bpo.2-cloud-amd64
Debian 10 | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | 4.19.0-6-amd64 a 4.19.0-16-amd64 </br> 4.19.0-6-cloud-amd64 a 4.19.0-16-cloud-amd64 </br> 5.8.0-0.bpo.2-amd64 </br> 5.8.0-0.bpo.2-cloud-amd64
Debian 10 | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | 4.19.0-6-amd64 a 4.19.0-16-amd64 </br> 4.19.0-6-cloud-amd64 a 4.19.0-16-cloud-amd64 </br> 5.8.0-0.bpo.2-amd64 </br> 5.8.0-0.bpo.2-cloud-amd64
Debian 10 | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | 4.19.0-6-amd64 a 4.19.0-14-amd64 </br> 4.19.0-6-cloud-amd64 a 4.19.0-14-cloud-amd64 </br> 5.8.0-0.bpo.2-amd64 </br> 5.8.0-0.bpo.2-cloud-amd64 </br> 4.19.0-10-cloud-amd64, 4.19.0-16-amd64, 4.19.0-16-cloud-amd64 mediante la revisión urgente 9.41**

**Nota: Para admitir los kernels de Linux más recientes en un plazo de 15 días desde su publicación, Azure Site Recovery implementa la revisión de corrección urgente sobre la versión más reciente del agente de movilidad. Esta corrección se implementa entre dos versiones principales. Para actualizar a la versión más reciente del agente de movilidad (incluida la revisión de corrección urgente), siga los pasos que se mencionan en [este artículo](service-updates-how-to.md#azure-vm-disaster-recovery-to-azure). Esta revisión está implementada actualmente para los agentes de movilidad que se usan en Azure para el escenario de recuperación ante desastres de Azure.

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Versiones de kernel admitidas de SUSE Linux Enterprise Server 12 para máquinas virtuales de Azure

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.45](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.56-azure </br> 4.12.14-16.65-azure </br> 4.12.14-16.68-azure </br> 4.12.14-16.76-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.56-azure </br> 4.12.14-16.65-azure </br> 4.12.14-16.68-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.56-azure </br> 4.12.14-16.65-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.56-azure |
SUSE Linux Enterprise Server 12 (SP1, SP2, SP3, SP4, SP5) | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) | Se admiten todos los [kernels de SUSE 12 SP1, SP2, SP3, SP4, SP5 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.4.138-4.7-azure a 4.4.180-4.31-azure,</br>4.12.14-6.3-azure a 4.12.14-6.43-azure </br> 4.12.14-16.7-azure a 4.12.14-16.44-azure </br> 4.12.14-16.47-azure mediante la revisión urgente 9.41**|

#### <a name="supported-suse-linux-enterprise-server-15-kernel-versions-for-azure-virtual-machines"></a>Versiones de kernel admitidas de SUSE Linux Enterprise Server 15 para máquinas virtuales de Azure

**Versión** | **Versión de Mobility service** | **Versión de kernel** |
--- | --- | --- |
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.45](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2](https://www.suse.com/support/kb/doc/?id=000019587) en stock.</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> De 5.3.18-18.5-azure a 5.3.18-18.58-azure </br> 5.3.18-18.69-azure
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.44](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2](https://www.suse.com/support/kb/doc/?id=000019587) en stock.</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> De 5.3.18-18.5-azure a 5.3.18-18.58-azure
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.43](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2](https://www.suse.com/support/kb/doc/?id=000019587) en stock.</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> De 5.3.18-18.5-azure a 5.3.18-18.58-azure
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.42](https://support.microsoft.com/topic/update-rollup-55-for-azure-site-recovery-kb5003408-b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> 5.3.18-18.5-azure a 5.3.18-18.47-azure
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.41](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2 en stock](https://www.suse.com/support/kb/doc/?id=000019587).</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.55-azure </br> 5.3.18-16-azure </br> 5.3.18-18.5-azure a 5.3.18-18.35-azure </br> 5.3.18-18.38-azure mediante la revisión urgente 9.41**
SUSE Linux Enterprise Server 15, SP1, SP2 | [9.40](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a)  | De forma predeterminada, se admiten todos los [kernels de SUSE 15, SP1, SP2](https://www.suse.com/support/kb/doc/?id=000019587) en stock.</br></br> 4.12.14-5.5-azure a 4.12.14-5.47-azure </br></br> 4.12.14-8.5-azure a 4.12.14-8.58-azure </br> 5.3.18-16-azure </br> 5.3.18-18.5-azure a 5.3.18-18.29-azure </br> 5.3.18-18.32-azure, 4.12.14-8.58-azure mediante la revisión urgente 9.40**

**Nota: Para admitir los kernels de Linux más recientes en un plazo de 15 días desde su publicación, Azure Site Recovery implementa la revisión de corrección urgente sobre la versión más reciente del agente de movilidad. Esta corrección se implementa entre dos versiones principales. Para actualizar a la versión más reciente del agente de movilidad (incluida la revisión de corrección urgente), siga los pasos que se mencionan en [este artículo](service-updates-how-to.md#azure-vm-disaster-recovery-to-azure). Esta revisión está implementada actualmente para los agentes de movilidad que se usan en Azure para el escenario de recuperación ante desastres de Azure.

## <a name="replicated-machines---linux-file-systemguest-storage"></a>Máquinas replicadas: Sistema de archivos o almacenamiento de invitado de Linux

* Sistemas de archivos: ext3, ext4, XFS y BTRFS
* Administrador de volúmenes: LVM2

> [!NOTE]
> No se admite software de múltiples rutas.


## <a name="replicated-machines---compute-settings"></a>Máquinas replicadas: Configuración de proceso

**Configuración** | **Soporte técnico** | **Detalles**
--- | --- | ---
Size | Cualquier tamaño de máquina virtual de Azure con al menos 2 núcleos de CPU y 1 GB de RAM | Compruebe los [tamaños de máquina virtual de Azure](../virtual-machines/sizes.md).
RAM | El controlador de Azure Site Recovery consume el 6 % de la memoria RAM.
Conjuntos de disponibilidad | Compatible | Si habilita la replicación de una máquina virtual de Azure con las opciones predeterminadas, se creará automáticamente un conjunto de disponibilidad en función de la configuración de la región de origen. Puede modificar estos valores.
Zonas de disponibilidad | Compatible |
Ventaja de uso híbrido (HUB) | Compatible | Si la máquina virtual de origen tiene habilitada una licencia HUB, en la conmutación por error de prueba o la máquina virtual de conmutación por error también se usa la licencia HUB.
Conjuntos de escalado de máquinas virtuales | No compatible |
Imágenes de la galería de Azure (publicadas por Microsoft) | Compatible | Se admiten si la máquina virtual se ejecuta en un sistema operativo compatible.
Imágenes de la Galería de Azure (publicadas por terceros) | Compatible | Se admiten si la máquina virtual se ejecuta en un sistema operativo compatible.
Imágenes personalizadas (publicadas de terceros) | Compatible | Se admiten si la máquina virtual se ejecuta en un sistema operativo compatible.
Máquinas virtuales migradas con Site Recovery | Compatible | Si se migró una máquina virtual de VMware o una física a Azure mediante Site Recovery, deberá desinstalar la versión antigua de Mobility Service que se ejecuta en la máquina y reiniciarla antes de replicarla en otra región de Azure.
Directivas de RBAC de Azure | No compatible | Las directivas de control de acceso basado en rol de Azure (RBAC de Azure) de las máquinas virtuales no se replican en la máquina virtual de conmutación por error de la región de destino.
Extensiones | No compatible | Las extensiones no se replican en la máquina virtual de conmutación por error en la región de destino. Deben instalarse manualmente después de la conmutación por error.
Grupos con ubicación por proximidad | Compatible | Las máquinas virtuales que se encuentran dentro de un grupo con ubicación por proximidad se pueden proteger mediante Site Recovery.
Etiquetas  | Compatible | Las etiquetas generadas por el usuario aplicadas en máquinas virtuales de origen se trasladan a máquinas virtuales de destino después de la conmutación por error o conmutación por error de prueba. Las etiquetas de las máquinas virtuales se replican una vez cada 24 horas siempre que las máquinas virtuales estén presentes en la región de destino.


## <a name="replicated-machines---disk-actions"></a>Máquinas replicadas: acciones de disco

**Acción** | **Detalles**
-- | ---
Cambiar el tamaño de disco en una máquina virtual replicada | Se admite el aumento de tamaño de la máquina virtual de origen. No se admite la reducción de tamaño de la máquina virtual de origen. El redimensionamiento debe realizarse antes de la conmutación por error. No es necesario deshabilitar o volver a habilitar la replicación.<br/><br/> Si cambia la máquina virtual de origen después de la conmutación por error, los cambios no se capturan.<br/><br/> Si cambia el tamaño del disco en la máquina virtual de Azure después de la conmutación por error, Site Recovery no capturará los cambios y la conmutación por recuperación se realizará al tamaño original de la máquina virtual.<br/><br/> Si el redimensionamiento es mayor o igual que 4 TB, tenga en cuenta las instrucciones de Azure sobre el almacenamiento en caché de disco [aquí](../virtual-machines/premium-storage-performance.md). 
Agregar un disco a una máquina virtual replicada | Compatible
Cambios sin conexión en discos protegidos | Para desconectar discos y realizar modificaciones sin conexión en ellos debe desencadenar una resincronización completa.
Almacenamiento en caché de disco | No se admite el almacenamiento en caché de disco para discos de 4 TiB o más. Si varios discos están conectados a la máquina virtual, cada disco de menos de 4 TiB será compatible con el almacenamiento en caché. Al cambiar la configuración de caché de un disco de Azure, se desconecta y se vuelve a conectar el disco de destino. Si se trata del disco del sistema operativo, se reinicia la máquina virtual. Detenga todas las aplicaciones y todos los servicios que podrían verse afectados por esta interrupción antes de cambiar la configuración de caché de disco. El incumplimiento de estas recomendaciones podría provocar datos dañados.

## <a name="replicated-machines---storage"></a>Máquinas replicadas: almacenamiento

Esta tabla resume la compatibilidad con el disco del sistema operativo, el disco de datos y el disco temporal de Azure VM.

- Es importante respetar los límites y destinos de los discos de VM de los [discos administrados](../virtual-machines/disks-scalability-targets.md) para evitar cualquier problema de rendimiento.
- Si implementa con la configuración predeterminada, Site Recovery creará automáticamente las cuentas de almacenamiento y los discos necesarios en función de la configuración de origen.
- Si desea personalizar, asegúrese de que sigue las directrices.

**Componente** | **Soporte técnico** | **Detalles**
--- | --- | ---
Tamaño máximo del disco de sistema operativo | 2048 GB | [Más información](../virtual-machines/managed-disks-overview.md) sobre discos de máquina virtual.
Disco temporal | No compatible | El disco temporal se excluye de la replicación siempre.<br/><br/> No almacene los datos persistentes en el disco temporal. [Más información](../virtual-machines/managed-disks-overview.md).
Tamaño máximo del disco de datos | 32 TB para discos administrados<br></br>4 TB para discos no administrados|
Tamaño mínimo del disco de datos | Sin restricción de discos no administrados. 2 GB en discos administrados |
Número máximo de discos de datos | Hasta 64, según la compatibilidad con un tamaño específico de máquina virtual de Azure | [Más información](../virtual-machines/sizes.md) sobre tamaños de máquina virtual.
Tasa de cambio de disco de datos | Máximo de 20 MBps por disco para el almacenamiento Premium. Máximo de 2 MBps por disco para el almacenamiento Estándar. | Si la tasa media de cambio de los datos en el disco sobrepasa el máximo continuamente, la replicación no mantendrá el ritmo.<br/><br/>  Sin embargo, si se supera el máximo esporádicamente, la replicación podrá mantenerlo, aunque podría ver puntos de recuperación ligeramente retrasados.
Disco de datos: cuenta de almacenamiento Estándar | Compatible |
Disco de datos: cuenta de almacenamiento Premium | Compatible | Si una máquina virtual tiene discos repartidos entre cuentas de almacenamiento Estándar y Premium, puede seleccionar una cuenta de almacenamiento de destino diferente para cada disco a fin de garantizar que tenga la misma configuración de almacenamiento en la región de destino.
Disco administrado: estándar | Se admite en regiones de Azure en las que se pueda usar Azure Site Recovery. |
Disk administrado: premium | Se admite en regiones de Azure en las que se pueda usar Azure Site Recovery. |
Límites de suscripción de discos | Hasta 3000 discos protegidos por suscripción | Asegúrese de que la suscripción de origen o destino no tenga más de 3000 discos protegidos con Azure Site Recovery (de datos y del sistema operativo).
SSD estándar | Compatible |
Redundancia | Se admiten LRS y GRS.<br/><br/> No se admite ZRS.
Almacenamiento esporádico y almacenamiento frecuente | No compatible | Los discos de máquina virtual no admiten el almacenamiento esporádico ni el frecuente
Espacios de almacenamiento | Compatible |
Interfaz de almacenamiento de NVMe | No compatible
Cifrado en el host | Compatible | [Vea información detallada](../virtual-machines/disks-enable-host-based-encryption-portal.md) para crear una máquina virtual con cifrado de un extremo a otro mediante el cifrado en el host.
Cifrado en reposo (SSE) | Compatible | SSE es la configuración predeterminada en las cuentas de almacenamiento.
Cifrado en reposo (CMK) | Compatible | Para los discos administrados se admiten tanto claves de software como HSM
Cifrado en reposo doble | Compatible | Más información sobre las regiones admitidas para [Windows](../virtual-machines/disk-encryption.md) y [Linux](../virtual-machines/disk-encryption.md)
Cifrado FIPS | No compatible
Azure Disk Encryption (ADE) para sistemas operativos Windows | Compatible con máquinas virtuales con discos administrados. | No se admiten las máquinas virtuales que usan discos no administrados. <br/><br/> No se admiten las claves protegidas con HSM. <br/><br/> No se admite el cifrado de volúmenes individuales en un solo disco. |
Azure Disk Encryption (ADE) para sistemas operativos Linux | Compatible con máquinas virtuales con discos administrados. | No se admiten las máquinas virtuales que usan discos no administrados. <br/><br/> No se admiten las claves protegidas con HSM. <br/><br/> No se admite el cifrado de volúmenes individuales en un solo disco. <br><br> Problema conocido en la habilitación de la replicación. [Más información.](./azure-to-azure-troubleshoot-errors.md#enable-protection-failed-as-the-installer-is-unable-to-find-the-root-disk-error-code-151137) |
Rotación de claves SAS | No compatible | Si se gira la clave SAS de las cuentas de almacenamiento, el cliente debe deshabilitar y volver a habilitar la replicación. |
Almacenamiento en caché del host | Compatible
Adición en caliente    | Compatible | La replicación de un disco de datos que se agrega a una máquina virtual de Azure replicada se puede habilitar en máquinas virtuales que usan discos administrados. <br/><br/> Solo se puede agregar un disco a una máquina virtual de Azure a la vez. No se admite la adición paralela de varios discos. |
Eliminación de disco en caliente    | No compatible | Si quita un disco de datos en la máquina virtual, deberá deshabilitar la replicación y habilitarla de nuevo en la máquina virtual.
Excluir el disco | Compatible. Debe usar [PowerShell](azure-to-azure-exclude-disks.md) para configurarlo. |    Los discos temporales se excluyen de forma predeterminada.
Espacios de almacenamiento directo  | Compatible con puntos de recuperación coherentes con los bloqueos. Los puntos de recuperación coherentes de la aplicación no son compatibles. |
Servidor de archivos de escalado horizontal  | Compatible con puntos de recuperación coherentes con los bloqueos. Los puntos de recuperación coherentes de la aplicación no son compatibles. |
DRBD | No se admiten los discos que forman parte de una instalación de DRBD. |
LRS | Compatible |
GRS | Compatible |
RA-GRS | Compatible |
ZRS | No compatible |
Almacenamiento en frío y en caliente | No compatible | Los discos de máquina virtual no admiten el almacenamiento temporal y permanente.
Firewalls de Azure Storage para redes virtuales  | Compatible | Si restringe el acceso de red virtual a las cuentas de almacenamiento, habilite [Permitir servicios de Microsoft de confianza](../storage/common/storage-network-security.md#exceptions).
Cuentas de almacenamiento de uso general V2 (capas de acceso frecuente y esporádico) | Compatible | Los costos de transacción aumentan considerablemente en comparación con las cuentas de almacenamiento de uso general V1
Generación 2 (Arranque UEFI) | Compatible
Discos NVMe | No compatible
Discos compartidos de Azure | No compatible
Opción de transferencia segura | Compatible
Discos con el acelerador de escritura habilitado | No compatible
Etiquetas  | Compatible | Las etiquetas generadas por el usuario se replican cada 24 horas.
Eliminación temporal | No compatible | No se admite la eliminación temporal porque, una vez habilitada en una cuenta de almacenamiento, aumenta el costo. ASR realiza creaciones o eliminaciones muy frecuentes de archivos de registro mientras se replican, lo que provoca un aumento de los costos.

>[!IMPORTANT]
> Para evitar cualquier problema de rendimiento, asegúrese de respetar los objetivos de escalabilidad y rendimiento del disco de la VM de los [discos administrados](../virtual-machines/disks-scalability-targets.md). Si usa la configuración predeterminada, Site Recovery crea las cuentas de almacenamiento y los discos necesarios en función de la configuración de origen. Si personaliza y selecciona su propia configuración, respete los objetivos de escalabilidad y rendimiento del disco de las máquinas virtuales de origen.

## <a name="limits-and-data-change-rates"></a>Límites y tasas de cambio de datos

En la tabla siguiente se resumen los límites de Site Recovery.

- Estos límites se basan en nuestras pruebas, pero evidentemente no pueden cubrir todas las combinaciones de E/S posibles de la aplicación.
- Los resultados reales pueden variar en función de la combinación de E/S de la aplicación.
- Hay dos límites que deben tenerse en cuenta: renovación de datos por disco y por máquina virtual.
- El límite actual para la actividad de datos por máquina virtual es de 54 MB/s, independientemente del tamaño.

**Destino de almacenamiento** | **Promedio de E/S de disco de origen** |**Actividad de datos media de disco de origen** | **Actividad de datos de disco de origen total por día**
---|---|---|---
Standard Storage | 8 KB    | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 8 KB    | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 16 KB | 4 MB/s |    336 GB por disco
Disco Premium P10 o P15 | 32 KB, o más | 8 MB/s | 672 GB por disco
Disco Premium P20, P30, P40 o P50 | 8 KB    | 5 MB/s | 421 GB por disco
Disco Premium P20, P30, P40 o P50 | 16 KB, o más |20 MB/s | 1684 GB por disco

## <a name="replicated-machines---networking"></a>Máquinas replicadas: redes
**Configuración** | **Soporte técnico** | **Detalles**
--- | --- | ---
NIC | Número máximo admitido para un tamaño específico de máquina virtual de Azure | Las NIC se crean cuando se crea la máquina virtual durante la conmutación por error.<br/><br/> El número de tarjetas NIC en la máquina virtual de conmutación por error viene determinado por el número de tarjetas NIC que haya en la máquina virtual de origen en el momento de habilitar la replicación. Si agrega o quita una tarjeta NIC después de habilitar la replicación, esto no influirá en el número de NIC de la máquina virtual replicada después de la conmutación por error. <br/><br/> No existe garantía de que el orden de lo adaptadores de red después de la conmutación sea el mismo que el orden original. <br/><br/> Puede cambiar el nombre de los adaptadores de red en la región de destino en función de las convenciones de nomenclatura de su organización. El cambio de nombre de adaptador de red es compatible con PowerShell.
Equilibrador de carga de Internet | No compatible | Puede configurar equilibradores de carga públicos y de Internet en la región primaria. Aun así, Azure Site Recovery no admite los equilibradores de carga públicos o de Internet en la región de recuperación ante desastres.
Equilibrador de carga interno | Compatible | Asocie el equilibrador de carga configurado previamente con un script de Azure Automation de un plan de recuperación.
Dirección IP pública | Compatible | Asocie una dirección IP pública existente a la NIC. O bien, cree una dirección IP pública y asóciela con la NIC mediante un script de Azure Automation de un plan de recuperación.
Grupo de seguridad de red en una tarjeta NIC | Compatible | Asocie el grupo de seguridad de red a la NIC con un script de Azure Automation de un plan de recuperación.
Grupo de seguridad de red en la subred | Compatible | Asocie el grupo de seguridad de red a la subred con un script de Azure Automation de un plan de recuperación.
Dirección IP (estática) reservada | Compatible | Si la NIC de la máquina virtual de origen tiene una dirección IP estática y la subred de destino tiene la misma dirección IP disponible, esta se asigna a la máquina virtual de conmutación por error.<br/><br/> Si la subred de destino no tiene la misma dirección IP disponible, una de las direcciones IP disponibles de la subred se reserva para esta máquina virtual.<br/><br/> También puede especificar una dirección IP fija y una subred en **Elementos replicados** > **Configuración** > **Red** > **Interfaces de red**.
Dirección IP dinámica | Compatible | Si la NIC de origen tiene una dirección IP dinámica, la NIC de la máquina virtual de conmutación por error será también dinámica de forma predeterminada.<br/><br/> Puede modificar esta a una dirección IP fija si es necesario.
Varias direcciones IP | Compatible | Cuando se conmuta por error una máquina virtual que tiene una NIC con varias direcciones IP, solamente se mantiene la dirección IP principal de la NIC en la región de origen de forma predeterminada. Para conmutar por error configuraciones de IP secundarias, vaya a la hoja **Red** y configúrelas.
Traffic Manager     | Compatible | Puede configurar previamente Traffic Manager de forma que el tráfico se dirija regularmente al punto de conexión de la región de origen y al punto de conexión de la región de destino en caso de conmutación por error.
Azure DNS | Compatible |
DNS personalizado    | Compatible |
Proxy no autenticado | Compatible | [Más información](./azure-to-azure-about-networking.md)
Proxy autenticado | No compatible | Si la máquina virtual usa un proxy autenticado para la conectividad saliente, no se puede replicar mediante Azure Site Recovery.
Conexión VPN de sitio a sitio en el entorno local<br/><br/>(con o sin ExpressRoute)| Compatible | Asegúrese de que los UDR y NSG estén configurados de manera que el tráfico de Site Recovery no se dirija al entorno local. [Más información](./azure-to-azure-about-networking.md)
Conexión de red virtual a red virtual    | Compatible | [Más información](./azure-to-azure-about-networking.md)
Puntos de conexión de servicio de red virtual | Compatible | Si está restringiendo el acceso de red virtual a las cuentas de almacenamiento, asegúrese de que los servicios de Microsoft de confianza pueden acceder a la cuenta de almacenamiento.
Redes aceleradas | Compatible | Se deben habilitar las redes aceleradas en una máquina virtual de origen. [Más información](azure-vm-disaster-recovery-with-accelerated-networking.md).
Dispositivo de Palo Alto Networks | No compatible | Con los dispositivos de terceros, a menudo hay restricciones impuestas por el proveedor dentro de la máquina virtual. Azure Site Recovery necesita que el agente, las extensiones y la conectividad saliente estén disponibles. Pero el dispositivo no permite que se configure ninguna actividad saliente dentro de la máquina virtual.
IPv6  | No compatible | Tampoco se admiten configuraciones mixtas que incluyan IPv4 e IPv6. Libere la subred del intervalo IPv6 antes de cualquier operación de Site Recovery.
Acceso de vínculo privado al servicio Site Recovery | Compatible | [Más información](azure-to-azure-how-to-enable-replication-private-endpoints.md)
Etiquetas  | Compatible | Las etiquetas generadas por el usuario en NIC se replican cada 24 horas.



## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información en las [instrucciones sobre redes](./azure-to-azure-about-networking.md) para replicar máquinas virtuales de Azure.
- Implemente la recuperación ante desastres mediante [la replicación de máquinas virtuales de Azure](./azure-to-azure-quickstart.md).
