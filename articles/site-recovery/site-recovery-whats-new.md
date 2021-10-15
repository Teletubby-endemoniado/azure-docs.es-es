---
title: Novedades de Azure Site Recovery
description: Proporciona un resumen de las nuevas características y las actualizaciones más recientes en el servicio Azure Site Recovery.
ms.topic: conceptual
ms.date: 07/28/2021
ms.openlocfilehash: 1e5c74d34d85fed9fff86dba92398c8cd57287bd
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129356257"
---
# <a name="whats-new-in-site-recovery"></a>Novedades de Site Recovery

El servicio [Azure Site Recovery](site-recovery-overview.md) se actualiza y mejora de manera continua. Para ayudarlo a mantenerse actualizado, en este artículo encontrará información sobre las versiones más recientes, las características nuevas y el nuevo contenido. Esta página se actualiza de manera periódica.

Puede seguirla y suscribirse a las notificaciones de actualizaciones de Site Recovery en el canal de [actualizaciones de Azure](https://azure.microsoft.com/updates/?product=site-recovery).

## <a name="supported-updates"></a>Actualizaciones admitidas

En el caso de los componentes de Site Recovery, se admiten las versiones N-4, donde N es la versión de lanzamiento más reciente. Estos se resumen en la siguiente tabla.

**Actualizar** |  **Instalación unificada** | **Servidor de configuración OVA** | **Agente de Mobility Service** | **Proveedor de Site Recovery** | **Agente de Recovery Services**
--- | --- | --- | --- | --- | ---
[Paquete acumulativo 58](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) | 9.45.6096.1 | 5.1.6952.0 | 9.45.6096.1 | 5.1.6952.0 | 2.0.9237.0
[Paquete acumulativo 57](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) | 9.44.6068.1 | 5.1.6899.0 | 9.44.6068.1 | 5.1.6899.0 | 2.0.9236.0
[Rollup 56](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6)  | 9.43.6040.1  | 5.1.6853.0 | 9.43.6040.1| 5.1.6853.0 | 2.0.9226.0
[Paquete acumulativo 55](https://support.microsoft.com/topic/b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8)  | 9.42.5941.1 | 5.1.6692.0 | 9.42.5941.1 | 5.1.6692.0  | 2.0.9208.0
[Paquete acumulativo 54](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533)  | 9.41.5888.1 | 5.1.6620.0 | 9.41.5888.1 | 5.1.6620.0  | 2.0.9202.0
[Paquete acumulativo 53](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a)  | 9.40.5850.1 | 5.1.6537.0 | 9.40.5850.1 | 5.1.6537.0  | 2.0.9202.0
[Paquete acumulativo 52](https://support.microsoft.com/help/4597409/)  | 9.39.5796.1 | 5.1.6458.0 | 9.39.5796.1 | 5.1.6458.0  | 2.0.9196.0


[Más información](service-updates-how-to.md) sobre la instalación y el soporte técnico de las actualizaciones.

## <a name="updates-september-2021"></a>Actualizaciones (septiembre de 2021)

### <a name="update-rollup-58"></a>Paquete acumulativo de actualizaciones 58

El [paquete acumulativo de actualizaciones 58](https://support.microsoft.com/topic/update-rollup-58-for-azure-site-recovery-kb5007075-37ba21c3-47d9-4ea9-9130-a7d64f517d5d) ofrece las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el artículo de KB del paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el artículo de KB del paquete acumulativo).

## <a name="updates-august-2021"></a>Actualizaciones (agosto de 2021)

### <a name="update-rollup-57"></a>Paquete acumulativo de actualizaciones 57

El [paquete acumulativo de actualizaciones 57](https://support.microsoft.com/topic/update-rollup-57-for-azure-site-recovery-kb5006172-9fccc879-6e0c-4dc8-9fec-e0600cf94094) ofrece las siguientes actualizaciones:

> [!NOTE]
> El paquete acumulativo de actualizaciones solo proporciona actualizaciones para la versión preliminar pública de las protecciones de VMware a Azure. En esta versión no se abordan otras correcciones ni mejoras.
> Para configurar la experiencia de versión preliminar, tendrá que realizar una instalación nueva y usar un nuevo almacén de Recovery Services. No se admite la actualización de la arquitectura existente a la nueva arquitectura.

Esta versión preliminar pública cubre una revisión completa de la arquitectura actual para la protección de máquinas de VMware.
- Obtenga [información](./vmware-azure-architecture-preview.md) sobre la nueva arquitectura y los cambios introducidos.
- Consulte los requisitos previos y configure el dispositivo de replicación de ASR según [estos pasos](./deploy-vmware-azure-replication-appliance-preview.md).
- [Habilite la replicación](./vmware-azure-set-up-replication-tutorial-preview.md) de máquinas de VMware.
- Consulte las funcionalidades de [actualización automática](./upgrade-mobility-service-preview.md) y el [cambio](./switch-replication-appliance-preview.md) para el dispositivo de replicación de ASR.


### <a name="update-rollup-56"></a>Paquete acumulativo de actualizaciones 56

El [paquete acumulativo de actualizaciones 56](https://support.microsoft.com/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el artículo de KB del paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el artículo de KB del paquete acumulativo).

**Azure Site Recovery Service** | Se han realizado mejoras para que la habilitación de la replicación y la re-protección de las operaciones sean más rápidas en un 46 %.
**Azure Site Recovery portal** | La replicación ahora se puede habilitar entre dos regiones de Azure de todo el mundo. Ya no está limitado a habilitar la replicación dentro del continente.


## <a name="updates-july-2021"></a>Actualizaciones (julio 2021)

### <a name="update-rollup-56"></a>Paquete acumulativo de actualizaciones 56

El [paquete acumulativo de actualizaciones 56](https://support.microsoft.com/en-us/topic/update-rollup-56-for-azure-site-recovery-kb5005376-33f27950-1a07-43e5-bf40-4b380a270ef6) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el artículo de KB del paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el artículo de KB del paquete acumulativo).

**Azure Site Recovery Service** | Se han realizado mejoras para que la habilitación de la replicación y la re-protección de las operaciones sean más rápidas en un 46 %.
**Azure Site Recovery portal** | La replicación ahora se puede habilitar entre dos regiones de Azure de todo el mundo. Ya no está limitado a habilitar la replicación dentro del continente.


## <a name="updates-april-2021"></a>Actualizaciones (abril de 2021)

### <a name="update-rollup-55"></a>Paquete acumulativo de actualizaciones 55

El [paquete acumulativo de actualizaciones 55](https://support.microsoft.com/topic/b19c8190-5f88-43ea-85b1-d9e0cc5ca7e8) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).
**Recuperación ante desastres en VM de Azure** | Se ha agregado compatibilidad con la recuperación ante desastres intercontinental de máquinas virtuales de Azure.<br/><br/> Compatibilidad con la API REST para la protección de VMSS Flex.<br/><br/> Ahora se admite para las máquinas virtuales que ejecutan Oracle Linux 8.2 y 8.3.
**Recuperación ante desastres de un servidor físico o de una máquina virtual de VMware en Azure** | Se ha agregado compatibilidad con el uso de Ubuntu-20.04 al configurar el servidor de destino maestro.<br/><br/> Ahora se admite para las máquinas virtuales que ejecutan Oracle Linux 8.2 y 8.3.


## <a name="updates-february-2021"></a>Actualizaciones (febrero de 2021)

### <a name="update-rollup-54"></a>Paquete acumulativo de actualizaciones 54

El [paquete acumulativo de actualizaciones 54](https://support.microsoft.com/topic/update-rollup-54-for-azure-site-recovery-50873c7c-272c-4a7a-b9bb-8cd59c230533) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).
**Recuperación ante desastres en VM de Azure** | La recuperación ante desastres de zona a zona mediante Azure Site Recovery está ahora disponible con carácter general en 4 regiones más: Europa del Norte, Este de EE. UU., Centro de EE. UU. y Oeste de EE. UU. 2.<br/>
**Recuperación ante desastres de un servidor físico o de una máquina virtual de VMware en Azure** | La actualización incluye la posibilidad de seleccionar en el portal grupos con ubicación por proximidad para máquinas de VMware o físicas después de habilitar la replicación.<br/><br/> Ahora se admite la protección de máquinas de VMware con un tamaño de disco de datos de hasta 32 TB.
**Recuperación ante desastres de Hyper-V en Azure** | La actualización incluye la posibilidad de seleccionar en el portal grupos con ubicación por proximidad para máquinas de Hyper-V después de habilitar la replicación.


## <a name="updates-january-2021"></a>Actualizaciones (enero de 2021)

### <a name="update-rollup-53"></a>Paquete acumulativo de actualizaciones 53

El [paquete acumulativo de actualizaciones 53](https://support.microsoft.com/topic/update-rollup-53-for-azure-site-recovery-060268ef-5835-bb49-7cbc-e8c1e6c6e12a) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).
**Recuperación ante desastres en VM de Azure** | Ahora se admite la replicación de etiquetas. Cualquier etiqueta agregada a las máquinas virtuales de Azure, los discos y las NIC de la región de origen se replican en las máquinas de la región de destino.<br/><br/> Ahora las máquinas virtuales de Azure que ejecutan Debian 10 se admiten para la replicación.
**Recuperación ante desastres de un servidor físico o de una máquina virtual de VMware en Azure** | La actualización incluye mejoras del registro para la replicación de máquinas virtuales de VMware en Azure y una mensajería de errores mejorada.<br/><br/> Ahora las máquinas virtuales de VMware y las máquinas físicas que ejecutan Debian 10 se admiten para la replicación.


## <a name="updates-november-2020"></a>Actualizaciones (noviembre de 2020)

### <a name="update-rollup-52"></a>Paquete acumulativo de actualizaciones 52

El [paquete acumulativo de actualizaciones 52](https://support.microsoft.com/help/4597409/update-rollup-52-for-azure-site-recovery) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Varias correcciones y mejoras que se detallan en el paquete acumulativo, incluyendo la nueva compatibilidad con Linux del servicio Mobility.
**Recuperación ante desastres en VM de Azure** | Ahora se admite para las máquinas virtuales que ejecutan RHEL 8.3 y Oracle Linux 7.9.
**Recuperación ante desastres de un servidor físico o de una máquina virtual de VMware en Azure** | Ahora se admite para máquinas virtuales que ejecutan RHEL 8.3 y Oracle Linux 7.9.

## <a name="updates-october-2020"></a>Actualizaciones (octubre de 2020)

### <a name="update-rollup-51"></a>Paquete acumulativo de actualizaciones 51

El [paquete acumulativo de actualizaciones 51](https://support.microsoft.com/help/4590304/update-rollup-51-for-azure-site-recovery) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Varias correcciones y mejoras que se detallan en el paquete acumulativo, incluyendo la nueva compatibilidad con Linux del servicio Mobility.

## <a name="updates-september-2020"></a>Actualizaciones (septiembre de 2020)

### <a name="update-rollup-50"></a>Paquete acumulativo de actualizaciones 50

El [paquete acumulativo de actualizaciones 50](https://support.microsoft.com/help/4582666/update-rollup-50-for-azure-site-recovery) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

## <a name="updates-august-2020"></a>Actualizaciones (agosto de 2020)

### <a name="update-rollup-49"></a>Paquete acumulativo de actualizaciones 49

El [paquete acumulativo de actualizaciones 49](https://support.microsoft.com/help/4578241/update-rollup-49-for-azure-site-recovery) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Varias correcciones y mejoras que se detallan en el paquete acumulativo, incluyendo la nueva compatibilidad con Linux del servicio Mobility.

## <a name="updates-july-2020"></a>Actualizaciones (julio de 2020)

### <a name="update-rollup-48"></a>Paquete acumulativo de actualizaciones 48

El [paquete acumulativo de actualizaciones 48](https://support.microsoft.com/help/4573888/update-rollup-48-for-azure-site-recovery) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

> [!NOTE]
> El paquete acumulativo de actualizaciones 48 tiene un problema conocido con la habilitación de la replicación de máquinas Linux cifradas mediante ADE. [Más información](./azure-to-azure-troubleshoot-errors.md#enable-protection-failed-as-the-installer-is-unable-to-find-the-root-disk-error-code-151137).

### <a name="update-rollup-47"></a>Paquete acumulativo de actualizaciones 47

El [paquete acumulativo de actualizaciones 47](https://support.microsoft.com/help/4570609/update-rollup-47-for-azure-site-recovery) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

## <a name="updates-june-2020"></a>Actualizaciones (junio de 2020)

### <a name="update-rollup-46"></a>Paquete acumulativo de actualizaciones 46

El [paquete acumulativo de actualizaciones 46](https://support.microsoft.com/help/4564347/update-rollup-46-for-azure-site-recovery) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

## <a name="updates-march-2020"></a>Actualizaciones (marzo de 2020)

### <a name="update-rollup-45"></a>Paquete acumulativo de actualizaciones 45

El [paquete acumulativo de actualizaciones 45](https://support.microsoft.com/help/4550047/update-rollup-45-for-azure-site-recovery) proporciona las siguientes actualizaciones:

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery según se detalla en el paquete acumulativo de actualizaciones.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

## <a name="updates-january-2020"></a>Actualizaciones (enero de 2020)

### <a name="update-rollup-44"></a>Paquete acumulativo de actualizaciones 44

El [paquete acumulativo de actualizaciones 44](https://support.microsoft.com/help/4538187/update-rollup-44-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | No hubo actualizaciones para los proveedores y agentes de Site Recovery.
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="azure-vmware-disaster-recovery"></a>Recuperación ante desastres de VMware en Azure

Azure Virtual Machines ahora admiten que máquinas virtuales habiliten el cifrado en reposo con claves administradas por el cliente. [Más información](azure-to-azure-how-to-enable-replication-cmk-disks.md).


### <a name="update-rollup-43"></a>Paquete acumulativo de actualizaciones 43

El [paquete acumulativo de actualizaciones 43](https://support.microsoft.com/help/4537047/update-rollup-43-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)


## <a name="updates-november-2019"></a>Actualizaciones (noviembre de 2019)

### <a name="update-rollup-42"></a>Paquete acumulativo de actualizaciones 42

El [paquete acumulativo de actualizaciones 42](https://support.microsoft.com/help/4531426/update-rollup-42-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)


### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

En la tabla se resumen las nuevas características de recuperación ante desastres de las máquinas virtuales de Azure.

**Característica** | **Detalles**
--- | ---
**UEFI** | Site Recovery admite ahora la recuperación ante desastres para máquinas virtuales de Azure con la arquitectura de arranque basada en UEFI.
**Linux** | Site Recovery admite ahora máquinas virtuales de Azure que ejecutan Linux con Azure Disk Encryption (ADE).
**Generación 2** | Ahora se admiten todas las máquinas virtuales de Azure de generación 2 para la recuperación ante desastres.
**Regiones** | Ahora puede habilitar la recuperación ante desastres para máquinas virtuales de Azure en la geografía de Noruega.

### <a name="vmware-to-azure-disaster-recovery"></a>Recuperación ante desastres de VMware a Azure

En la tabla se resumen las nuevas características para la recuperación ante desastres de VMware en Azure.

**Característica** | **Detalles**
--- | ---
**UEFI** | Site Recovery admite ahora la recuperación ante desastres para máquinas virtuales de VMware con la arquitectura de arranque basada en UEFI.<br/><br/> Entre los sistemas operativos admitidos se incluyen Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, SLES 12 SP4, RHEL 8.

## <a name="update-to-servicing-stack-updatesha-2"></a>Actualización de la pila de servicio/SHA-2

Para la recuperación ante desastres de máquinas virtuales de Azure en una región secundaria o de máquinas virtuales de VMware locales o servidores físicos en Azure, tenga en cuenta lo siguiente:

- A partir de la versión 9.30.5407.1 de la extensión de Mobility Service (para máquinas virtuales de Azure) y del agente de Mobility Service (para máquinas físicas y de VMware), algunos sistemas operativos para máquinas deben ejecutar la actualización de la pila de servicio y SHA-2. En la tabla siguiente se muestran los detalles.
- Instale la actualización y SHA-2 de acuerdo con el KB vinculado. SHA-1 no se admite desde septiembre de 2019 y, si la firma de código SHA-2 no está habilitada, la extensión del agente no se instalará ni actualizará según lo previsto.
- Más información sobre los [requisitos y la actualización de SHA-2](https://aka.ms/SHA-2KB).

**Sistema operativo** | **MV de Azure** | **Máquina física/máquina virtual de VMware**
--- | --- | ---
**Windows 2008 R2 SP1** | [Actualización de la pila de servicio](https://support.microsoft.com/help/4490628)<br/> [SHA-2](https://support.microsoft.com/help/4474419)| [Actualización de la pila de servicio](https://support.microsoft.com/help/4490628)<br/> [SHA-2](https://support.microsoft.com/help/4474419)
**Windows 2008 SP2** | [Actualización de la pila de servicio](https://support.microsoft.com/help/4493730)<br/> [SHA-2](https://support.microsoft.com/help/4474419)| [Actualización de la pila de servicio](https://support.microsoft.com/help/4493730)<br/> [SHA-2](https://support.microsoft.com/help/4474419)
**Windows 7 SP1** | [Actualización de la pila de servicio](https://support.microsoft.com/help/4490628)<br/> [SHA-2](https://support.microsoft.com/help/4474419)| [Actualización de la pila de servicio](https://support.microsoft.com/help/4490628)<br/> [SHA-2](https://support.microsoft.com/help/4474419).



## <a name="updates-october-2019"></a>Actualizaciones (octubre de 2019)

### <a name="update-rollup-41"></a>Paquete acumulativo de actualizaciones 41

El [paquete acumulativo de actualizaciones 41](https://support.microsoft.com/help/4528026/update-rollup-41-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)



### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

En la tabla se resumen las nuevas características de recuperación ante desastres de las máquinas virtuales de Azure.

**Característica** | **Detalles**
--- | ---
**Configuración de conmutación por error de prueba** | Al configurar una conmutación por error de prueba, ahora puede configurar las opciones de la red y la máquina virtual de la conmutación por error de prueba, incluida la dirección IP, el grupo de seguridad de red, el equilibrador de carga interno y la dirección IP pública de la NIC de cada máquina. Esta configuración es opcional y no cambia el comportamiento actual. Si no configura estas opciones, puede elegir una red virtual de Azure en el momento de la conmutación por error de prueba. [Más información](https://azure.microsoft.com/blog/customize-networking-for-dr-drills-azure-site-recovery/).
**Planes de recuperación** | Los planes de recuperación ahora están limitados a 100 máquinas virtuales para garantizar la confiabilidad de la conmutación por error.

### <a name="vmware-to-azure-disaster-recovery"></a>Recuperación ante desastres de VMware a Azure

En la tabla se resumen las nuevas características para la recuperación ante desastres de VMware en Azure.

**Característica** | **Detalles**
--- | ---
**Planes de recuperación** | Los planes de recuperación ahora están limitados a 100 máquinas virtuales para garantizar la confiabilidad de la conmutación por error.


## <a name="updates-september-2019"></a>Actualizaciones (septiembre de 2019)

### <a name="update-rollup-40"></a>Paquete acumulativo de actualizaciones 40

El [Paquete acumulativo de actualizaciones 40](https://support.microsoft.com/help/4521530/update-rollup-40-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)




### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

En la tabla se resumen las nuevas características de recuperación ante desastres de las máquinas virtuales de Azure.

**Característica** | **Detalles**
--- | ---
**Limpieza después de la conmutación por recuperación** | Después de conmutar por error en la región secundaria de Azure y, a continuación, conmutar por recuperación a la región primaria, Site Recovery limpia automáticamente las máquinas de la región secundaria. No es necesario eliminar manualmente las máquinas virtuales y las tarjetas de red.
**La conmutación por error de prueba conserva la dirección IP** | Ahora puede conservar la dirección IP de la máquina virtual de origen durante un simulacro de recuperación ante desastres y seleccionar una dirección IP estática para una conmutación por error de prueba.

### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
Nuevas alertas del servidor de procesos | Hemos agregado nuevas alertas del servidor de procesos. [Más información](vmware-physical-azure-monitor-process-server.md).

### <a name="hyper-v-disaster-recovery"></a>Recuperación ante desastres de Hyper-V

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
Cuenta de almacenamiento | Site Recovery admite ahora el uso de cuentas de almacenamiento con el firewall habilitado para la recuperación ante desastres de Hyper-V en Azure.  Puede seleccionar cuentas de almacenamiento con firewall habilitado como una cuenta de destino o para almacenamiento en caché. Si usa la cuenta de almacenamiento con firewall habilitado, asegúrese de habilitar la opción de permitir servicios de Microsoft de confianza.<br/><br/> Esto se admite para las máquinas virtuales de Hyper-V con o sin System Center VMM.


## <a name="updates-august-2019"></a>Actualizaciones (agosto de 2019)

### <a name="update-rollup-39"></a>Paquete acumulativo de actualizaciones 39

El [Paquete acumulativo de actualizaciones 39](https://support.microsoft.com/help/4517283/update-rollup-39-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)


### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

En la tabla se resumen las nuevas características de recuperación ante desastres de las máquinas virtuales de Azure.

**Característica** | **Detalles**
--- | ---
**Cifrado sin Azure AD** | Ahora se admite el cifrado sin una aplicación Azure AD para la replicación de máquinas virtuales de Azure en discos administrados que ejecutan Windows.
**Recursos de red para conmutación por error** | Cuando se conmuta por error a otra región, ahora puede adjuntar la configuración de recursos de red (NSG, equilibrio de carga, dirección IP pública) a una máquina virtual.

## <a name="updates-july-2019"></a>Actualizaciones (julio de 2019)

### <a name="update-rollup-38"></a>Paquete acumulativo de actualizaciones 38

El [Paquete acumulativo de actualizaciones 38](https://support.microsoft.com/help/4513507/) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)


### <a name="general"></a>General

Site Recovery ahora admite la utilización de cuentas de almacenamiento de uso general versión 2 para el almacenamiento en caché o el almacenamiento de destino. Anteriormente solo se admitía la versión 1.

### <a name="vmware-to-azure-disaster-recovery"></a>Recuperación ante desastres de VMware a Azure

Ahora puede replicar discos de hasta 8 TB al replicar en una máquina virtual de Azure con discos administrados.


## <a name="updates-june-2019"></a>Actualizaciones (junio de 2019)

### <a name="update-rollup-37"></a>Paquete acumulativo de actualizaciones 37

El [Paquete acumulativo de actualizaciones 37](https://support.microsoft.com/help/4508614/) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualizaciones de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)


### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Particiones GPT** | A partir del Paquete acumulativo de actualizaciones 37 y versiones posteriores (versión 9.25.5241.1 de Mobility Service), se admiten hasta cinco particiones GPT en UEFI. Antes de esta actualización, se admitían cuatro.



## <a name="updates-may-2019"></a>Actualizaciones (mayo de 2019)

### <a name="update-rollup-36"></a>Paquete acumulativo de actualizaciones 36

El [Paquete acumulativo de actualizaciones 36](https://support.microsoft.com/help/4503156) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Replicar discos agregados** | La habilitación de la replicación para disco de datos se agregó a una instancia de Azure Virtual Machines que ya está habilitada para la recuperación ante desastres. [Más información](azure-to-azure-enable-replication-added-disk.md).
**Actualizaciones automáticas** | Ahora, cuando configure las actualizaciones automáticas de la extensión Mobility Service que se ejecute en instancias de Azure Virtual Machines habilitadas para la recuperación ante desastres, puede seleccionar una cuenta de automatización que ya tenga para usarla en lugar de usar la cuenta predeterminada que crea Site Recovery. [Más información](azure-to-azure-autoupdate.md).


### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Supervisión del servidor de procesos** | Ahora, para la recuperación ante desastres de máquinas virtuales VMware y servidores físicos en el entorno local, puede supervisar y solucionar problemas de los servidores de procesos con funcionalidad mejorada de alertas e informes sobre el estado de mantenimiento de los servidores. [Más información](vmware-physical-azure-monitor-process-server.md).





## <a name="updates-march-2019"></a>Actualizaciones (marzo de 2019)

### <a name="update-rollup-35"></a>Paquete acumulativo de actualizaciones 35

El [Paquete acumulativo de actualizaciones 35](https://support.microsoft.com/en-us/help/4494485/update-rollup-35-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones)
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo)

### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Discos administrados** | La replicación de servidores físicos y máquinas virtuales de VMware locales es ahora directamente en discos administrados en Azure. Los datos locales se envían a una cuenta de almacenamiento de caché de Azure y los puntos de recuperación se crean en discos administrados en la ubicación de destino. Esto garantiza que no es necesario administrar varias cuentas de almacenamiento de destino.
**Servidor de configuración** | Site Recovery ahora admite servidores de configuración con diferentes NIC. Agregue adaptadores adicionales a la máquina virtual del servidor de configuración antes de registrar este en el almacén. Si lo agrega más tarde, deberá volver a registrar el servidor en el almacén.


## <a name="updates-february-2019"></a>Actualizaciones (febrero de 2019)

### <a name="update-rollup-34"></a>Paquete acumulativo de actualizaciones 34

El [Paquete acumulativo de actualizaciones 34](https://support.microsoft.com/help/4490016/update-rollup-34-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).


### <a name="update-rollup-33"></a>Paquete acumulativo de actualizaciones 33

El [Paquete acumulativo de actualizaciones 33](https://support.microsoft.com/help/4489582/update-rollup-33-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).


### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Asignación de red** | Para la recuperación ante desastres de VM de Azure, puede usar cualquier red de destino disponible cuando se habilite la replicación.
**SSD estándar** | Ahora puede configurar la recuperación ante desastres para VM de Azure con [discos SSD estándar](../virtual-machines/disks-types.md#standard-ssd).
**Espacios de almacenamiento directo** | Puede configurar la recuperación ante desastres para aplicaciones que se ejecutan en aplicaciones de la VM de Azure mediante el uso de [Espacios de almacenamiento directo ](/windows-server/storage/storage-spaces/storage-spaces-direct-overview) para lograr alta disponibilidad.  Usar los Espacios de almacenamiento directo (S2D) junto con Site Recovery proporciona una protección exhaustiva de las cargas de trabajo de una VM de Azure. S2D le permite hospedar un clúster invitado en Azure. Esto es especialmente útil cuando una VM hospeda una aplicación crítica, como capa de ASCS de SAP, SQL Server o un servidor de archivos de escalabilidad horizontal.


### <a name="vmwarephysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Sistema de archivos BRTFS de Linux** | Site Recovery ahora admite la replicación de VM de VMware con el sistema de archivos BRTFS. No se admite la replicación si:<br/><br/>- El subvolumen del sistema de archivos BTRFS se cambia después de habilitar la replicación.<br/><br/>- El sistema de archivos está repartido entre varios discos.<br/><br/>- El sistema de archivos BTRFS es compatible con RAID.
**Windows Server 2019** | Compatibilidad agregada para máquinas que ejecutan Windows Server 2019.


## <a name="updates-january-2019"></a>Actualizaciones (enero de 2019)


### <a name="accelerated-networking-azure-vms"></a>Redes aceleradas (VM de Azure)

Redes aceleradas habilita la virtualización de E/S de raíz única (SR-IOV) en una máquina virtual, lo que mejora su rendimiento en la red. Al habilitar la replicación de una máquina virtual de Azure, Site Recovery detecta si las redes aceleradas están habilitadas. Si es así, después de la conmutación por error, Site Recovery configura automáticamente las redes aceleradas en la máquina virtual de Azure de réplica de destino, tanto para [Windows](../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) como para [Linux](../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms).

[Más información](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="update-rollup-32"></a>Paquete acumulativo de actualizaciones 32

El [Paquete acumulativo de actualizaciones 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó soporte para RedHat Workstation 6/7; versiones nuevas de kernel para Ubuntu, Debian y SUSE.
**Espacios de almacenamiento directo** | Site Recovery admite VM de Azure que usan Espacios de almacenamiento directo (S2D).

### <a name="vmware-vmsphysical-servers-disaster-recovery"></a>Recuperación ante desastres de servidores físicos/VM de VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó compatibilidad con RedHat Enterprise Linux 7.6; RedHat Workstation 6/7; Oracle Linux 6.10 y Oracle Linux 7.6, además de nuevas versiones de kernel para Ubuntu, Debian y SUSE.


### <a name="update-rollup-31"></a>Paquete acumulativo de actualizaciones 31

El [Paquete acumulativo de actualizaciones 31](https://support.microsoft.com/help/4478871/update-rollup-31-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="vmware-vmsphysical-servers-replication"></a>Replicación de servidores físicos/VM de VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** |  Se ha agregado compatibilidad con Oracle Linux 6.8, Oracle Linux 6.9 y Oracle Linux 7.0 mediante Red Hat Compatible Kernel y con la versión 5 de Unbreakable Enterprise Kernel (UEK).
**LVM** | Se agregó compatibilidad para los volúmenes LVM y LVM2.<br/><br/> Ahora se admite el directorio /boot, en una partición de disco y en volúmenes LVM.
**Directorios** | Se agregó compatibilidad para estos directorios configurados como particiones independientes o sistemas de archivos que no están en el mismo disco del sistema:<br/><br/> /(root), /boot, /usr, /usr/local, /var, /etc.
**Windows Server 2008** | Se agregó compatibilidad para discos dinámicos.
**Conmutación por error** | Tiempo de conmutación por error mejorado para las VM de VMware donde storvsc y vsbus no son controladores de arranque.
**Compatibilidad con UEFI** | Las VM de Azure no admiten el tipo de arranque UEFI. Ahora puede migrar servidores físicos locales con UEFI a Azure con Site Recovery. Site Recovery migra el servidor mediante la conversión del tipo de arranque a BIOS antes de la migración. Antes, Site Recovery solo admitía esta conversión para VM. La compatibilidad está disponible para los servidores físicos que ejecutan Windows Server 2012 o posterior.

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se ha agregado compatibilidad con Oracle Linux 6.8, Oracle Linux 6.9 y Oracle Linux 7.0 mediante Red Hat Compatible Kernel y con la versión 5 de Unbreakable Enterprise Kernel (UEK).
**Sistema de archivos BRTFS de Linux** | Compatibilidad con las máquinas virtuales de Azure.
**Máquinas virtuales de Azure en zonas de disponibilidad** | Puede habilitar la replicación en otra región para VM de Azure implementadas en zonas de disponibilidad. Ahora puede habilitar la replicación de una máquina virtual de Azure y establecer el destino de una conmutación por error en una instancia de máquina virtual única, una máquina virtual en un conjunto de disponibilidad o una máquina virtual en una zona de disponibilidad. La configuración no afecta la replicación. [Lea](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) el anuncio.
**Almacenamiento habilitado para firewall (portal/PowerShell)** | Se agregó compatibilidad para [cuentas de almacenamiento habilitadas para firewall](../storage/common/storage-network-security.md).<br/><br/> Puede replicar VM de Azure con discos no administrados en cuentas de almacenamiento habilitadas mediante firewall en otra región de Azure para recuperación ante desastres.<br/><br/> También puede usar cuentas de almacenamiento habilitadas mediante firewall como cuentas de almacenamiento de destino para discos no administrados.<br/><br/> Compatible con el portal y usando PowerShell.

## <a name="updates-december-2018"></a>Actualizaciones (diciembre de 2018)

### <a name="automatic-updates-for-the-mobility-service-azure-vms"></a>Actualizaciones automáticas de Mobility Service (VM de Azure)

Site Recovery agregó una opción para actualizaciones automáticas de la extensión de Mobility Service. La extensión de Mobility Service se instala en cada máquina virtual de Azure replicada por Site Recovery. Al habilitar la replicación, puede seleccionar si permitir que Site Recovery administre las actualizaciones de la extensión.

Las actualizaciones no requieren reiniciar la máquina virtual ni tampoco afectan la replicación. [Más información](azure-to-azure-autoupdate.md).

### <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Calculadora de precios para la recuperación ante desastres de máquinas virtuales de Azure

La recuperación ante desastres de VM de Azure incurre en costos de licencias de VM y en costos de red y almacenamiento. Azure brinda una [calculadora de precios](https://aka.ms/a2a-cost-estimator) para ayudarlo a establecer estos costos. Site Recovery proporciona ahora una [estimación de precios de ejemplo](https://aka.ms/a2a-cost-estimator) que valora una implementación de ejemplo en función de una aplicación de tres nivel que usa seis máquinas virtuales con 12 discos HDD estándar y 6 discos SSD Premium.

- En el ejemplo se supone un cambio de datos de 10 GB al día para un disco estándar y de 20 GB para Premium.
- En el caso de su implementación en particular, puede cambiar las variables para calcular los costos.
- Puede especificar el número de máquinas virtuales, el número y el tipo de discos administrados y la tasa de cambio de datos total esperada en todas las máquinas virtuales.
- Además, puede aplicar un factor de compresión para estimar los costos de ancho de banda.

[Lea](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) el anuncio.


## <a name="updates-october-2018"></a>Actualizaciones (octubre de 2018)

### <a name="update-rollup-30"></a>Paquete acumulativo de actualizaciones 30

El [Paquete acumulativo de actualizaciones 30](https://support.microsoft.com/help/4468181/azure-site-recovery-update-rollup-30) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure
Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Regiones admitidas** | Compatibilidad de Site Recovery agregada para Centro de Australia 1 y Centro de Australia 2.
**Compatibilidad con cifrado del disco** | Se ha agregado compatibilidad para recuperación ante desastres de VM de Azure cifradas con Azure Disk Encryption (ADE) con la aplicación de Azure AD. [Más información](azure-to-azure-how-to-enable-replication-ade-vms.md).
**Exclusión de disco** | Ahora, los discos no inicializados se excluyen automáticamente durante la replicación de VM de Azure.
**Almacenamiento habilitado para firewall (PowerShell)** | Se agregó compatibilidad para [cuentas de almacenamiento habilitadas para firewall](../storage/common/storage-network-security.md).<br/><br/> Puede replicar VM de Azure con discos no administrados en cuentas de almacenamiento habilitadas mediante firewall en otra región de Azure para recuperación ante desastres.<br/><br/> También puede usar cuentas de almacenamiento habilitadas mediante firewall como cuentas de almacenamiento de destino para discos no administrados.<br/><br/> Compatible solo con PowerShell.


### <a name="update-rollup-29"></a>Paquete acumulativo de actualizaciones 29

El [Paquete acumulativo de actualizaciones 29](https://support.microsoft.com/help/4466466/update-rollup-29-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).


## <a name="updates-august-2018"></a>Actualizaciones (agosto de 2018)

### <a name="update-rollup-28"></a>Paquete acumulativo de actualizaciones 28

El [Paquete acumulativo de actualizaciones 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure
Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó compatibilidad para RedHat Enterprise Linux 6.10; CentOS 6.10.<br/><br/>
**Soporte técnico en la nube** | Compatibilidad con recuperación ante desastres para VM de Azure en la nube de Alemania.
**Recuperación ante desastres entre suscripciones** | Se admite la replicación de VM de Azure de una región a otra de una suscripción diferente dentro del mismo inquilino de Azure Active Directory. [Más información](https://aka.ms/cross-sub-blog).

### <a name="vmware-vmphysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VM de VMware
Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se agregó compatibilidad para RedHat Enterprise Linux 6.10; CentOS 6.10.<br/><br/> Ahora se admiten las máquinas virtuales Linux que usan el estilo de partición de tabla de particiones de GUID (GPT) en el modo de compatibilidad BIOS heredado. Revise las [preguntas más frecuentes de VM de Azure](../virtual-machines/faq-for-disks.yml) para obtener más información.
**Recuperación ante desastres para VM después de la migración** | Compatibilidad para habilitar la recuperación ante desastres en una región secundaria para una VM de VMware local migrada a Azure, sin necesidad de desinstalar el servicio Mobility en la VM antes de habilitar la replicación.
**Windows Server 2008** | Compatibilidad para migrar máquinas que ejecutan Windows Server 2008 R2/2008 de 64 bits y 32 bits.<br/><br/> Solo migración (conmutación por error y replicación). No se admite la conmutación por recuperación.

## <a name="updates-july-2018"></a>Actualizaciones (julio de 2018)

### <a name="update-rollup-27-july-2018"></a>Paquete acumulativo de actualizaciones 27 (julio de 2018)

El [Paquete acumulativo de actualizaciones 27](https://support.microsoft.com/help/4055712/update-rollup-27-for-azure-site-recovery) proporciona las siguientes actualizaciones.

**Actualizar** | **Detalles**
--- | ---
**Proveedores y agentes** | Actualización de los proveedores y agentes de Site Recovery (como se detalla en el paquete acumulativo de actualizaciones).
**Mejoras y correcciones de problemas** | Un número de correcciones y mejoras (tal como se detalla en el paquete acumulativo).

### <a name="azure-vm-disaster-recovery"></a>Recuperación ante desastres en VM de Azure

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Compatibilidad agregada para Red Hat Enterprise Linux 7.5.

### <a name="vmware-vmphysical-server-disaster-recovery"></a>Recuperación ante desastres de un servidor físico/VM de VMware

Las características que se agregaron este mes se resumen en la tabla.

**Característica** | **Detalles**
--- | ---
**Soporte técnico de Linux** | Se ha agregado compatibilidad para Red Hat Enterprise Linux 7.5, SUSE Linux Enterprise Server 12.



## <a name="next-steps"></a>Pasos siguientes

Manténgase al día con las actualizaciones en la página [Actualizaciones de Azure](https://azure.microsoft.com/updates/?product=site-recovery).