---
title: Tipos de Azure Storage para una carga de trabajo de SAP
description: Tipos de Azure Storage para una carga de trabajo de SAP
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: d7c59cc1-b2d0-4d90-9126-628f9c7a5538
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/02/2021
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: eb1e315d0fce2b43ed15c2808ddaf37c605c79dd
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131432802"
---
# <a name="azure-storage-types-for-sap-workload"></a>Tipos de Azure Storage para una carga de trabajo de SAP
Azure tiene numerosos tipos de almacenamiento que difieren en gran medida en términos de funcionalidad, rendimiento, latencia y precio. Algunos de los tipos de almacenamiento no son para escenarios de SAP, o bien su uso está limitado en ellos. Sin embargo, hay varios tipos de almacenamiento de Azure que están bien adaptados u optimizados para escenarios específicos de carga de trabajo de SAP. Especialmente en el caso de SAP HANA, algunos tipos de almacenamiento de Azure han recibido certificación para su uso con SAP HANA. En este documento, vamos a repasar los diferentes tipos de almacenamiento y describir su capacidad y uso con las cargas de trabajo de SAP y los componentes de SAP.

Comentario sobre las unidades que se usan en este artículo. Los proveedores de nube pública se han migrado para usar GiB ([gibibyte](https://en.wikipedia.org/wiki/Gibibyte)) o TiB ([tebibyte](https://en.wikipedia.org/wiki/Tebibyte) como unidades de tamaño, en lugar del gigabyte o terabyte. Por lo tanto, toda la documentación y los precios de Azure utilizan esas unidades.  A lo largo de todo el documento nos referiremos exclusivamente a estas unidades de tamaño MiB, GiB y TiB. Es posible que sus planes se basen en MB, GB y TB. Por lo tanto, tenga en cuenta algunas pequeñas diferencias en los cálculos si necesita dimensionar un rendimiento de 400 MiB/seg, en lugar de un rendimiento de 250 MiB/seg.

## <a name="microsoft-azure-storage-resiliency"></a>Resistencia de Microsoft Azure Storage

El almacenamiento en Microsoft Azure HDD estándar, SSD estándar, Azure Premium Storage y en disco Ultra mantiene el VHD base (con SO) y los discos de datos conectados a la máquina virtual o los VHD en tres copias en tres nodos de almacenamiento diferentes. La conmutación por error a otra réplica y la inicialización de una nueva réplica en caso de un error de nodo de almacenamiento es transparente. Como resultado de esta redundancia, **NO** es necesario usar ningún tipo de capa de redundancia de almacenamiento entre varios discos de Azure. Este hecho se denomina Almacenamiento con redundancia local (LRS). LRS es la opción predeterminada para todos estos tipos de almacenamiento en Azure. [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) proporciona redundancia suficiente para lograr los mismos Acuerdos de Nivel de Servicio que otro almacenamiento nativo de Azure.

Existen más métodos de redundancia, que se describen en el artículo [Redundancia de Azure Storage](../../../storage/common/storage-redundancy.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) y se aplican a algunos de los diferentes tipos de almacenamiento que ofrece Azure. 

Tenga en cuenta también que los distintos tipos de almacenamiento de Azure afectan a los SLA de disponibilidad de una sola máquina virtual, tal como se publica en [SLA para Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines).


### <a name="azure-managed-disks"></a>Azure Managed Disks

Los discos Managed Disks son un tipo de recurso de Azure Resource Manager que puede usarse en lugar de discos duros virtuales almacenados en cuentas de Azure Storage. Los discos Managed Disks se alinean automáticamente con el [conjunto de disponibilidad][virtual-machines-manage-availability] de la máquina virtual a la que están conectados y, por tanto, aumentan la disponibilidad de la máquina virtual y los servicios que se ejecutan en la máquina virtual. Para más información, lea [este artículo de información general](../../managed-disks-overview.md).

En relación con la resistencia, en este ejemplo se muestra la ventaja de estos discos administrados:

- Está implementando sus dos máquinas virtuales de DBMS para su sistema SAP en un conjunto de disponibilidad de Azure. 
- A medida que Azure implementa las máquinas virtuales, el disco con la imagen de sistema operativo se colocará en un clúster de almacenamiento diferente. Esto evita que ambas máquinas virtuales se vean afectadas por un problema de un solo clúster de almacenamiento de Azure.
- A medida que crea nuevos discos administrados que se asignan a estas máquinas virtuales para almacenar los archivos de datos y de registro de la base de datos, estos discos nuevos para las dos máquinas virtuales también se implementan en clústeres de almacenamiento independientes, por lo que ninguno de los discos de la primera máquina virtual comparte los clústeres de almacenamiento con los discos de la segunda máquina virtual.

Al implementar sin discos administrados en cuentas de almacenamiento definidas por el cliente, la asignación de disco es arbitraria y no tiene en cuenta el hecho de que las máquinas virtuales se implementen en un conjunto de disponibilidad con fines de resistencia.

> [!NOTE]
> Pero aparte de este motivo y otras mejoras que están exclusivamente disponibles a través de discos administrados, es necesario que las nuevas implementaciones de máquinas virtuales que usan almacenamiento en bloque de Azure para sus discos (todo Azure Storage excepto Azure NetApp Files) utilicen discos administrados de Azure para los discos VHD/SO de base, discos de datos que contienen archivos de base de datos de SAP. Independientemente de si implementa las máquinas virtuales a través del conjunto de disponibilidad, en Availability Zones o aparte de conjuntos y zonas. No es necesario que los discos que se usan con el propósito de almacenar copias de seguridad sean discos administrados.

> [!NOTE]
> Los discos administrados de Azure proporcionan solo redundancia local (LRS). 


## <a name="storage-scenarios-with-sap-workloads"></a>Escenarios de almacenamiento con cargas de trabajo de SAP
Se necesita almacenamiento persistente en la carga de trabajo de SAP en varios componentes de la pila que implementa en Azure. Estos escenarios son al menos como estos:

- Mantienen el VHD de base de su VM que contiene el sistema operativo y otro software que instale en ese disco. Este disco o VHD es la raíz de la máquina virtual. Los cambios que se realicen en él deben mantenerse. Por lo tanto, la próxima vez que detenga y reinicie la máquina virtual, todos los cambios realizados antes siguen existiendo. Especialmente en los casos en los que Azure implementa la máquina virtual en un host diferente de donde se estaba ejecutando originalmente.
- Discos de datos persistentes. Estos discos son los VHD que se adjuntan para almacenar los datos de la aplicación. Estos datos de aplicación podrían ser archivos de datos y registro/rehacer de una base de datos, archivos de copia de seguridad o instalaciones de software. Es decir, cualquier disco más allá del VHD de base que contiene el sistema operativo.
- Recursos compartidos de archivos o discos compartidos que contienen el directorio de transporte global para NetWeaver o S/4HANA. El contenido de esos recursos compartidos lo consume el software que se ejecuta en varias máquinas virtuales o se usa para crear escenarios de clúster de conmutación por error de alta disponibilidad.
- El directorio /sapmnt o los recursos compartidos de archivos comunes para procesos EDI o similares. El contenido de esos recursos compartidos lo consume el software que se ejecuta en varias máquinas virtuales o se usa para crear escenarios de clúster de conmutación por error de alta disponibilidad.

En las siguientes secciones, se explican los diferentes tipos de almacenamiento de Azure y su uso para la carga de trabajo de SAP que se aplican a los cuatro escenarios anteriores. En el artículo [¿Qué tipos de disco están disponibles en Azure?](../../disks-types.md) se documenta una categorización general de cómo se deben usar los distintos tipos de almacenamiento de Azure. Las recomendaciones para usar los diferentes tipos de almacenamiento de Azure para una carga de trabajo de SAP no van a ser muy diferentes.

Para conocer las restricciones de compatibilidad con los tipos de almacenamiento de Azure para SAP NetWeaver/nivel de aplicación de S/4HANA, lea la [nota de soporte 2015553 de SAP](https://launchpad.support.sap.com/#/notes/2015553). Para obtener más información sobre los tipos de almacenamiento de Azure certificados y compatibles, lea el artículo [Configuraciones de almacenamiento de máquinas virtuales de Azure en SAP HANA](./hana-vm-operations-storage.md).

En las secciones que describen los diferentes tipos de almacenamiento de Azure se proporcionará más información sobre las restricciones y las posibilidades que ofrece el almacenamiento compatible con SAP. 

### <a name="storage-choices-when-using-dbms-replication"></a>Opciones de almacenamiento al usar la replicación de DBMS
Nuestras arquitecturas de referencia prevén el uso de funcionalidades de DBMS como SQL Server Always On, la replicación del sistema de HANA, DB2 HADR u Oracle Data Guard. En caso de que use estas tecnologías entre dos o varias máquinas virtuales de Azure, los tipos de almacenamiento elegidos para cada una de las VM deben ser los mismos. Esto significa que si el almacenamiento elegido para el volumen de registro para rehacer un sistema DBMS es Azure Premium Storage en una VM, es necesario que el mismo volumen se base en Azure Premium Storage con todas las demás VM que se encuentran en la misma configuración de sincronización de alta disponibilidad. Lo mismo sucede con los volúmenes de datos que se usan para los archivos de base de datos.
  

## <a name="storage-recommendations-for-sap-storage-scenarios"></a>Recomendaciones de almacenamiento para escenarios de almacenamiento de SAP
Antes de entrar en los detalles, presentamos el resumen y las recomendaciones que ya están al principio del documento. Por su parte, los detalles de los tipos concretos de Azure Storage se presentan en esta sección del documento. El resumen de las recomendaciones de almacenamiento para los escenarios de almacenamiento de SAP en una tabla tiene el siguiente aspecto:

| Escenario de uso | HDD estándar | SSD estándar | Premium Storage | Disco Ultra | Azure NetApp Files |
| --- | --- | --- | --- | --- | --- |
| Disco del sistema operativo | No adecuado |  Adecuación restringida (no en prod.) | Recomendado | No es posible | No es posible |
| Directorio de transporte global | No compatible | No compatible | Recomendado | Recomendado | Recomendado |
| /sapmnt | No adecuado | Adecuación restringida (no en prod.) | Recomendado | Recomendado | Recomendado |
| Familias de máquinas virtuales M/MV2 de SAP HANA en volumen de datos de DBMS | No compatible | No compatible | Recomendado | Recomendado | Recomendado<sup>2</sup> |
| Familias de máquinas virtuales M/MV2 de SAP HANA en volumen de registro de DBMS | No compatible | No compatible | Recomendado<sup>1</sup> | Recomendado | Recomendado<sup>2</sup> | 
| Familias de máquinas virtuales Esv3/Edsv4 de SAP HANA en volumen de datos de DBMS | No compatible | No compatible | Recomendado | Recomendado | Recomendado<sup>2</sup> |
| Familias de máquinas virtuales Esv3/Edsv4 de SAP HANA en volumen de registro de DBMS | No compatible | No compatible | No compatible | Recomendado | Recomendado<sup>2</sup> | 
| Volumen de datos DBMS no HANA | No compatible | Adecuación restringida (no en prod.) | Recomendado | Recomendado | Solo para versiones específicas de Oracle en Oracle Linux, Db2 y SAP ASE en SLES/RHEL Linux |
| Familias de máquinas virtuales M/MV2 no de HANA en volumen de registro de DBMS | No compatible | Adecuación restringida (no en prod.) | Recomendado<sup>1</sup> | Recomendado | Solo para versiones específicas de Oracle en Oracle Linux, Db2 y SAP ASE en SLES/RHEL Linux |
| Familias de máquinas virtuales no M/MV2 no de HANA en volumen de registro de DBMS | No compatible | adecuación restringida (no en prod.) | Adecuado para cargas de trabajo hasta de tamaño medio | Recomendado | Solo para versiones específicas de Oracle en Oracle Linux, Db2 y SAP ASE en SLES/RHEL Linux |


<sup>1</sup> Con el uso del [Acelerador de escritura de Azure](../../how-to-enable-write-accelerator.md) para las familias de máquinas virtuales M/MV2 para los volúmenes de registros de registro/rehacer. <sup>2</sup> El uso de UNF requiere que /hana/data y /hana/log estén en ANF. 

Características que puede esperar de la lista de tipos de almacenamiento diferentes, como:

| Escenario de uso | HDD estándar | SSD estándar | Premium Storage | Disco Ultra | Azure NetApp Files |
| --- | --- | --- | --- | --- | --- |
| Acuerdo de Nivel de Servicio rendimiento/IOPS | No | No | Sí | Sí | Sí |
| Lecturas de latencia | Alto | Medio a alto | Bajo | submilisegundo | submilisegundo |
| Escrituras de latencia | Alto | Medio a alto  | Bajo (submilisegundo<sup>1</sup>) | submilisegundo | submilisegundo |
| HANA compatible | No | No | sí<sup>1</sup> | Sí | Sí |
| Instantáneas de disco posibles | Sí | Sí | Sí | No | Sí |
| Asignación de discos en diferentes clústeres de almacenamiento cuando se usan conjuntos de disponibilidad | Mediante discos administrados | Mediante discos administrados | Mediante discos administrados | Tipo de disco no compatible con máquinas virtuales implementadas mediante conjuntos de disponibilidad | No<sup>3</sup> |
| Alineación con Availability Zones | Sí | Sí | Sí | Sí | Requiere la participación de Microsoft |
| Redundancia de zona | No para discos administrados | No para discos administrados | No para discos administrados | No | No |
| Redundancia geográfica | No para discos administrados | No para discos administrados | No | No | No |


<sup>1</sup> Con el uso del [Acelerador de escritura de Azure](../../how-to-enable-write-accelerator.md) para las familias de máquinas virtuales M/MV2 para los volúmenes de registro/rehacer.

<sup>2</sup> Los costos dependen del IOPS aprovisionado y el rendimiento.

<sup>3</sup> La creación de diferentes grupos de capacidad de ANF no garantiza la implementación de grupos de capacidad en diferentes unidades de almacenamiento.


> [!IMPORTANT]
> Para lograr una latencia de E/S de menos de 1 milisegundo mediante Azure NetApp Files (ANF), debe trabajar con Microsoft para organizar la ubicación correcta entre las máquinas virtuales y los recursos compartidos de NFS basados en ANF. Hasta ahora no hay ningún mecanismo que proporcione una proximidad automática entre una máquina virtual implementada y los volúmenes de NFS hospedados en ANF. Dada la configuración diferente de las distintas regiones de Azure, la latencia de red agregada podría aumentar la latencia de E/S más allá de 1 milisegundo si la máquina virtual y el recurso compartido de NFS no están asignados en proximidad.


> [!IMPORTANT]
> Ninguno de los discos administrados basados en almacenamiento en bloque de Azure que se ofrecen actualmente, o Azure NetApp Files, ofrecen redundancia geográfica o zonal. Como resultado, debe asegurarse de que las arquitecturas de alta disponibilidad y recuperación ante desastres no se basen en ningún tipo de replicación de almacenamiento nativo de Azure para estos discos administrados, NFS o recursos compartidos de SMB.


## <a name="azure-premium-storage"></a>Azure Premium Storage
El almacenamiento SSD Premium de Azure se introdujo con el objetivo de proporcionar lo siguiente:

* Baja latencia de E/S
* Acuerdo de Nivel de Servicio para IOPS y rendimiento
* Menor variabilidad de la latencia de E/S

Este tipo de almacenamiento se dirige a las cargas de trabajo de DBMS, el tráfico de almacenamiento que requiere una latencia baja de milisegundos de un solo dígito. Por su parte, los Acuerdos de Nivel de Servicio sobre el costo de IOPS y rendimiento en el caso de Azure Premium Storage no se refieren al volumen de datos real almacenado en dichos discos, sino a la categoría de tamaño de ese disco, independientemente de la cantidad de datos que contengan. También se pueden crear discos en Premium Storage sin correspondencia directa con las categorías de tamaño mostradas en el artículo [SSD Premium](../../disks-types.md#premium-ssds). Las conclusiones de este artículo son las siguientes:

- El almacenamiento se organiza en intervalos. Por ejemplo, un disco en el intervalo de capacidad de 513 GiB a 1024 GiB comparte las mismas funcionalidades y los mismos costos mensuales.
- La IOPS por GiB no realiza un seguimiento lineal de las categorías de tamaño. Los discos más pequeños por debajo de 32 GiB tienen tasas de IOPS mayores por GiB. En el caso de los discos de más de 32 GiB a 1024 GiB, la tasa de IOPS por GiB se encuentra entre 4-5 IOPS por GiB. En el caso de discos más grandes, de hasta 32 767 GiB, la tasa de IOPS por GiB está por debajo de 1.
- El rendimiento de E/S de este almacenamiento no es lineal con el tamaño de la categoría de disco. En el caso de los discos más pequeños, como la categoría con capacidad entre 65 GiB y 128 GiB, el rendimiento se encuentra en torno a 780 KB/GiB. Mientras que para los discos de gran tamaño, como un disco de 32 767 GiB, el rendimiento está alrededor de 28 KB/GiB.
- Los Acuerdos de Nivel de Servicio de IOPS y rendimiento no se pueden modificar sin cambiar la capacidad del disco.


La matriz de funcionalidad para la carga de trabajo de SAP tiene el siguiente aspecto:

| Capacidad| Comentario| Notas y vínculos | 
| --- | --- | --- | 
| VHD de base de SO | Adecuado | Todos los sistemas |
| Disco de datos | Adecuado | Todos los sistemas: [especialmente para SAP HANA](../../how-to-enable-write-accelerator.md) |
| Directorio de transporte global de SAP | Sí | [Compatible](https://launchpad.support.sap.com/#/notes/2015553) |
| SAP sapmnt | Adecuado | Todos los sistemas |
| Almacenamiento de copia de seguridad | Adecuado | Para el almacenamiento a corto plazo de copias de seguridad |
| Recursos compartidos/disco compartido | No disponible | Necesita Azure Premium Files o de terceros |
| Resistencia | LRS | No hay almacenamiento con redundancia geográfica o almacenamiento con redundancia de zona disponible para los discos |
| Latencia | Baja a media | - |
| SLA DE IOPS | Sí | - |
| IOPS lineal a capacidad | semilineal entre corchetes  | [Precios de Managed Disk](https://azure.microsoft.com/pricing/details/managed-disks/) |
| Número máximo de IOPS por disco | 20 000 [en función del tamaño del disco](https://azure.microsoft.com/pricing/details/managed-disks/) | Tenga también en cuenta los [límites de la máquina virtual](../../sizes.md). |
| SLA de rendimiento | Sí | - |
| Rendimiento lineal de la capacidad | Semilineal entre corchetes | [Precios de Managed Disk](https://azure.microsoft.com/pricing/details/managed-disks/) |
| Certificación de HANA | Sí | [especialmente para SAP HANA](../../how-to-enable-write-accelerator.md) |
| Instantáneas de disco posibles | Sí | - |
| Instantáneas de máquina virtual de Azure Backup posibles | Sí | A excepción de los discos de caché del [Acelerador de escritura](../../how-to-enable-write-accelerator.md)  |
| Costes | Media| - |

Azure Premium Storage no cumple los KPI de latencia de almacenamiento de SAP HANA de latencia de almacenamiento con los tipos comunes de almacenamiento en caché que se ofrecen con Azure Premium Storage. Para cumplir los KPI de latencia de almacenamiento para escrituras de registro de SAP HANA, debe usar el almacenamiento en caché del Acelerador de escritura de Azure como se describe en el artículo [Habilitar el acelerador de escritura](../../how-to-enable-write-accelerator.md). El Acelerador de escritura de Azure beneficia a todos los demás sistemas DBMS para sus escrituras de registro de transacciones y de rehacer. Por lo tanto, se recomienda utilizarlo en todas las implementaciones de DBMS de SAP. Para SAP HANA, es obligatorio el uso del Acelerador de escritura de Azure junto con Azure Premium Storage.



**Resumen:** Azure Premium Storage es uno de los tipos de almacenamiento de Azure recomendados para la carga de trabajo de SAP. Esta recomendación se aplica a los sistemas de producción y de no producción. Azure Premium Storage es adecuado para controlar las cargas de trabajo de base de datos. El uso del Acelerador de escritura de Azure va a mejorar considerablemente la latencia de escritura en discos Premium de Azure. Sin embargo, en el caso de los sistemas DBMS con altas tasas de IOPS y rendimiento, tiene que aprovisionar en exceso la capacidad de almacenamiento o usar funciones como Espacios de almacenamiento de Windows o los administradores de volúmenes lógicos de Linux para crear conjuntos seccionados que proporcionen la capacidad deseada en un lado, pero también la IOPS o el rendimiento necesarios con la mejor rentabilidad.


### <a name="azure-burst-functionality-for-premium-storage"></a>Funcionalidad de ráfaga de Azure para Premium Storage
En el caso de los discos de Azure Premium Storage con una capacidad de 512 GiB o inferior, se ofrece funcionalidad de ráfaga. La forma exacta en que funciona la ráfaga de disco se describe en el artículo [Seguridad de disco](../../disk-bursting.md). Al leer el artículo, comprenderá el concepto de acumulación de IOPS y rendimiento en los casos en que la carga de trabajo de E/S esté por debajo del valor de IOPS y de rendimiento de los discos nominal (para obtener más información sobre el rendimiento nominal, consulte [Precios del disco administrado](https://azure.microsoft.com/pricing/details/managed-disks/)). Acumulará la diferencia de IOPS y rendimiento entre el uso actual y los valores nominales del disco. Las ráfagas están limitadas a un máximo de 30 minutos.

Los casos ideales en los que se puede planear esta funcionalidad de ráfaga serán, probablemente, los volúmenes o discos que contengan archivos de datos para distintos DBMS. Se espera que la carga de trabajo de E/S para esos volúmenes, especialmente con sistemas de gama pequeña o mediana, tenga el aspecto siguiente:

- Carga de trabajo de lectura baja a moderada, ya que los datos, idealmente, se almacenan en la memoria caché, o, en el caso de HANA, deben estar completamente en la memoria.
- Ráfagas de escritura desencadenadas por puntos de control o puntos de retorno de la base de datos que se emiten de forma regular
- Carga de trabajo de copia de seguridad que se lee en un flujo continuo en los casos en que las copias de seguridad no se ejecutan mediante instantáneas de almacenamiento
- En el caso de SAP HANA, carga de los datos en memoria después de reiniciar una instancia

Especialmente en sistemas DBMS más pequeños en los que la carga de trabajo controla solo unos cientos de transacciones por segundo, esta funcionalidad de ráfaga puede tener sentido para los discos o volúmenes que almacenan la transacción o el registro de fase de puesta al día. La carga de trabajo esperada en este disco o volúmenes tiene el siguiente aspecto:

- Las escrituras regulares en el disco que dependen de la carga de trabajo y la naturaleza de la carga de trabajo, ya que es probable que todas las confirmaciones emitidas por la aplicación desencadenen una operación de E/S
- Mayor carga de trabajo en el rendimiento para los casos de tareas operativas, como crear o recompilar índices
- Ráfagas de lectura al realizar copias de seguridad de registros de transacciones o fase de puesta al día


## <a name="azure-ultra-disk"></a>Disco Ultra de Azure
Los discos Ultra de Azure ofrecen un alto rendimiento, un número elevado de IOPS y un almacenamiento en disco coherente y de baja latencia para máquinas virtuales IaaS de Azure. Algunas ventajas adicionales de los discos Ultra incluyen la capacidad de cambiar dinámicamente la IOPS y el rendimiento del disco junto con sus cargas de trabajo sin tener que reiniciar las máquinas virtuales. Los discos Ultra son adecuados para cargas de trabajo con un uso intensivo de datos, como la carga de trabajo DBMS de SAP. Los discos Ultra solo se pueden usar como discos de datos, y no se pueden usar como disco VHD de base que almacena el sistema operativo. Se recomienda el uso de Azure Premium Storage como disco VHD de base.

Al crear un disco Ultra, puede definir tres dimensiones:

- La capacidad del disco. Los intervalos van de 4 GiB a 65 536 GiB.
- IOPS aprovisionada para el disco. Se aplican valores máximos diferentes a la capacidad del disco. Lea el artículo [Disco Ultra](../../disks-types.md#ultra-disks) para obtener más detalles
- Ancho de banda de almacenamiento aprovisionado. Se aplica un ancho de banda máximo diferente en función de la capacidad del disco. Lea el artículo [Disco Ultra](../../disks-types.md#ultra-disks) para obtener más detalles

El costo de un solo disco lo determinan las tres dimensiones que se pueden definir para los discos concretos por separado. 


La matriz de funcionalidad para la carga de trabajo de SAP tiene el siguiente aspecto:

| Capacidad| Comentario| Notas y vínculos | 
| --- | --- | --- | 
| VHD de base de SO | No funciona | - |
| Disco de datos | Adecuado | Todos los sistemas  |
| Directorio de transporte global de SAP | Sí | [Compatible](https://launchpad.support.sap.com/#/notes/2015553) |
| SAP sapmnt | Adecuado | Todos los sistemas |
| Almacenamiento de copia de seguridad | Adecuado | Para el almacenamiento a corto plazo de copias de seguridad |
| Recursos compartidos/disco compartido | No disponible | Necesita terceros |
| Resistencia | LRS | No hay almacenamiento con redundancia geográfica o almacenamiento con redundancia de zona disponible para los discos |
| Latencia | Muy baja | - |
| SLA DE IOPS | Sí | - |
| IOPS lineal a capacidad | Semilineal entre corchetes  | [Precios de Managed Disk](https://azure.microsoft.com/pricing/details/managed-disks/) |
| Número máximo de IOPS por disco | 1200 a 160 000 | depende de la capacidad del disco |
| SLA de rendimiento | Sí | - |
| Rendimiento lineal de la capacidad | Semilineal entre corchetes | [Precios de Managed Disk](https://azure.microsoft.com/pricing/details/managed-disks/) |
| Certificación de HANA | Sí | - |
| Instantáneas de disco posibles | No | - |
| Instantáneas de máquina virtual de Azure Backup posibles | No | - |
| Costos | Mayor que Premium Storage | - |



**Resumen:** Los discos Ultra de Azure son un almacenamiento adecuado con baja latencia para todo tipo de carga de trabajo de SAP. Hasta ahora, los discos Ultra solo se pueden usar en combinaciones con máquinas virtuales que se han implementado a través de Availability Zones (implementación de zonas). Los discos Ultra no admiten instantáneas de almacenamiento en este momento. En contraposición al resto del almacenamiento, no se puede usar un disco Ultra para el disco VHD de base. El disco Ultra es idóneo para los casos en los que la carga de trabajo de E/S fluctúa mucho y desea adaptar el rendimiento de almacenamiento implementado o IOPS a patrones de carga de trabajo de almacenamiento en lugar de cambiar el tamaño para el uso máximo de ancho de banda e IOPS.


## <a name="azure-netapp-files-anf"></a>Azure NetApp files (ANF)
[Azure NetApp Files](https://azure.microsoft.com/services/netapp/) es el resultado de una cooperación entre Microsoft y NetApp establecida con el objetivo de proporcionar recursos compartidos de SMB y NFS nativos de Azure de alto rendimiento. El énfasis se pone en proporcionar un alto ancho de banda y un almacenamiento de baja latencia que permita escenarios de implementación de DBMS y, a lo largo del tiempo, habilite la funcionalidad típica operativa del almacenamiento de NetApp a través también de Azure. Los recursos compartidos de NFS/SMB se ofrecen en tres niveles de servicio diferentes en términos de rendimiento del almacenamiento y precio. Los niveles de servicio se documentan en el artículo [Niveles de servicio para Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-service-levels.md). En el caso de los diferentes tipos de carga de trabajo de SAP, se recomiendan los siguientes niveles de servicio:

- Carga de trabajo de DBMS de SAP:    rendimiento, idealmente Ultra
- Recurso compartido SAPMNT:         rendimiento, idealmente Ultra
- Directorio de transporte global: rendimiento, idealmente Ultra

> [!NOTE]
> El tamaño mínimo de aprovisionamiento es una unidad de 4 TiB denominada grupo de capacidad. A continuación, crea volúmenes fuera de este grupo de capacidad. El volumen más pequeño que se puede compilar es 100 GiB. Puede expandir un grupo de capacidad en pasos de TiB. Para información sobre precios, consulte el artículo [Precios de Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/)

El almacenamiento de ANF se admite actualmente en varios escenarios de carga de trabajo de SAP:

- Provisión de recursos compartidos de SMB o NFS para el directorio de transporte global de SAP
- El recurso compartido sapmnt en escenarios de alta disponibilidad, como se documenta en:
    - [Alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure en Windows con Azure NetApp Files (SMB) para aplicaciones SAP](./high-availability-guide-windows-netapp-files-smb.md)
    - [Alta disponibilidad de SAP NetWeaver en VM de Azure en SUSE Linux Enterprise Server con Azure NetApp Files para las aplicaciones de SAP](./high-availability-guide-suse-netapp-files.md)
    - [Alta disponibilidad de Azure Virtual Machines para SAP NetWeaver en Red Hat Enterprise Linux con Azure NetApp Files para aplicaciones SAP](./high-availability-guide-rhel-netapp-files.md)
- Las implementaciones de SAP HANA que usan recursos compartidos de NFS v.4.1 para volúmenes /hana/data y /hana/log o volúmenes de NFS v4.1 o NFS v3 para volúmenes de /hana/shared, como se documenta en el artículo [Configuraciones de almacenamiento de máquinas virtuales de Azure en SAP HANA](./hana-vm-operations-storage.md).
- IBM Db2 en un sistema operativo invitado Suse o Red Hat Linux
- Las implementaciones de Oracle en un sistema operativo invitado Oracle Linux mediante [dNFS](https://docs.oracle.com/en/database/oracle/oracle-database/19/ntdbi/creating-an-oracle-database-on-direct-nfs.html#GUID-2A0CCBAB-9335-45A8-B8E3-7E8C4B889DEA) para volúmenes registros de fase de puesta al día y datos de Oracle. Puede encontrar más detalles en el artículo [Implementación de DBMS de Oracle de Azure Virtual Machines para la carga de trabajo de SAP](./dbms_guide_oracle.md)
- SAP ASE en un sistema operativo invitado Suse o Red Hat Linux

> [!NOTE]
> De momento no se admiten cargas de trabajo DBMS en SMB basado en Azure NetApp Files.

Como ya se ha hecho con Azure Premium Storage, un tamaño de rendimiento fijo o lineal por GB puede suponer un problema cuando tiene que cumplir algunos números mínimos para el rendimiento. Este es el caso de SAP HANA. Con ANF, este problema puede ser más pronunciado que con el disco Premium de Azure. En el caso del disco Premium de Azure, puede tomar varios discos más pequeños con un rendimiento relativamente alto por GiB y seccionarlos para que sean rentables y tengan un rendimiento mayor a menor capacidad. Este tipo de seccionamiento no funciona para los recursos compartidos de NFS o SMB hospedados en ANF. Esta restricción dio lugar a la implementación de un exceso de capacidad como el siguiente:

- Para lograr, por ejemplo, un rendimiento de 250 MiB/s en un volumen de NFS hospedado en ANF, debe implementar la capacidad de 1,95 TiB del nivel de servicio Ultra. 
- Para lograr 400 MiB/s, tendría que implementar la capacidad de 3,125 TiB. Pero es posible que necesite un aprovisionamiento en exceso de la capacidad para lograr el rendimiento que requiere del volumen. Este aprovisionamiento en exceso de la capacidad afecta a los precios de las instancias de HANA más pequeñas. 
- En el espacio de uso de NFS sobre ANF para el directorio /sapmnt de SAP, generalmente se supera la capacidad mínima de 100 GiB a 150 GiB que aplica Azure NetApp Files. Sin embargo, la experiencia de los clientes ha demostrado que el rendimiento relacionado de 12,8 MiB/s (con el nivel de servicio Ultra) puede no ser suficiente y puede incidir negativamente en la estabilidad del sistema SAP. En estos casos, los clientes podrían evitar problemas aumentando el volumen de /sapmnt, por lo que se proporciona más rendimiento a ese volumen.

La matriz de funcionalidad para la carga de trabajo de SAP tiene el siguiente aspecto:

| Capacidad| Comentario| Notas y vínculos | 
| --- | --- | --- | 
| VHD de base de SO | No funciona | - |
| Disco de datos | Adecuado | SAP HANA, Oracle en Oracle Linux, Db2 y SAP ASE en SLES/RHEL  |
| Directorio de transporte global de SAP | Sí | SMB y NFS |
| SAP sapmnt | Adecuado | Todos los sistemas SMB (solo Windows) o NFS (solo Linux) |
| Almacenamiento de copia de seguridad | Adecuado | - |
| Recursos compartidos/disco compartido | Sí | SMB 3.0, NFS v3 y NFS v4.1 |
| Resistencia | LRS | No hay almacenamiento con redundancia geográfica o almacenamiento con redundancia de zona disponible para los discos |
| Latencia | Muy baja | - |
| SLA DE IOPS | Sí | - |
| IOPS lineal a capacidad | estrictamente lineal  | Dependiente del [nivel de servicio](../../../azure-netapp-files/azure-netapp-files-service-levels.md) |
| SLA de rendimiento | Sí | - |
| Rendimiento lineal de la capacidad | Semilineal entre corchetes | Dependiente del [nivel de servicio](../../../azure-netapp-files/azure-netapp-files-service-levels.md) |
| Certificación de HANA | Sí | - |
| Instantáneas de disco posibles | Sí | - |
| Instantáneas de máquina virtual de Azure Backup posibles | No | - |
| Costos | Mayor que Premium Storage | - |


Funcionalidad integrada adicional de almacenamiento de ANF:

- Capacidad para realizar instantáneas de volumen
- Clonación de volúmenes de ANF desde instantáneas
- Restauración de volúmenes a partir de instantáneas (reversión de instantáneas)

**Resumen**: Azure NetApp Files es un almacenamiento de baja latencia certificado de HANA que permite implementar volúmenes o recursos compartidos de NFS y SMB. El almacenamiento incluye tres niveles de servicio diferentes que proporcionan rendimiento e IOPS diferentes de forma lineal por la capacidad de GiB del volumen. El almacenamiento de ANF está habilitado para implementar escenarios de escalado horizontal de SAP HANA con un nodo en espera. El almacenamiento es adecuado para proporcionar recursos compartidos de archivos según sea necesario para /sapmnt o el directorio de transporte global de SAP. El almacenamiento de ANF incluye la disponibilidad de funcionalidad que está disponible como funcionalidad nativa de NetApp.  



## <a name="azure-standard-ssd-storage"></a>Almacenamiento SSD estándar de Azure
En comparación con el almacenamiento HDD estándar de Azure, el almacenamiento SSD estándar de Azure ofrece una mejor disponibilidad, coherencia, confiabilidad y latencia. Está optimizado para cargas de trabajo que necesitan un rendimiento coherente a niveles inferiores de IOPS. Esta opción es el almacenamiento mínimo que se usa para sistemas SAP que no son de producción y que tienen bajas demandas de IOPS y rendimiento. La matriz de funcionalidad para la carga de trabajo de SAP tiene el siguiente aspecto:

| Capacidad| Comentario| Notas y vínculos | 
| --- | --- | --- | 
| VHD de base de SO | Adecuación restringida | Sistemas que no son de producción |
| Disco de datos | Adecuación restringida | Algunos sistemas que no son de producción con bajas exigencias de IOPS y latencia |
| Directorio de transporte global de SAP | No | [No compatible](https://launchpad.support.sap.com/#/notes/2015553) |
| SAP sapmnt | Adecuación restringida | Sistemas que no son de producción |
| Almacenamiento de copia de seguridad | Adecuado | - |
| Recursos compartidos/disco compartido | No disponible | Necesita terceros |
| Resistencia | LRS y GRS | No hay almacenamiento con redundancia de zona disponible para los discos |
| Latencia | high | Demasiado alta para el directorio de transporte global de SAP o sistemas de producción |
| SLA DE IOPS | No | - |
| Número máximo de IOPS por disco | 500 | Independiente del tamaño del disco |
| SLA de rendimiento | No | - |
| Certificación de HANA | No | - |
| Instantáneas de disco posibles | Sí | - |
| Instantáneas de máquina virtual de Azure Backup posibles | Sí | - |
| Costos | LOW | - |



**Resumen:** El almacenamiento de SSD estándar de Azure es la recomendación mínima para las máquinas virtuales que no son de producción para VHD de base, implementaciones de DBMS eventuales con insensibilidad relativa a la latencia o bajas tasas de IOPS y de rendimiento. Este tipo de almacenamiento de Azure ya no se admite para hospedar el directorio de transporte global de SAP. 



## <a name="azure-standard-hdd-storage"></a>Almacenamiento HDD estándar de Azure
El almacenamiento HDD estándar de Azure era el único tipo de almacenamiento cuando la infraestructura de Azure se certificó para la carga de trabajo de SAP NetWeaver en el año 2014. En el año 2014, las máquinas virtuales de Azure eran pequeñas y bajas en términos de rendimiento del almacenamiento. Por lo tanto, este tipo de almacenamiento era capaz de satisfacer las demandas. El almacenamiento es ideal para cargas de trabajo independientes de la latencia, lo que apenas se produce en el espacio de SAP. Con el creciente rendimiento de las máquinas virtuales de Azure y la mayor carga de trabajo que generan estas máquinas virtuales, este tipo de almacenamiento ya no se tiene en cuenta para el uso con escenarios de SAP. La matriz de funcionalidad para la carga de trabajo de SAP tiene el siguiente aspecto:

| Capacidad| Comentario| Notas y vínculos | 
| --- | --- | --- | 
| VHD de base de SO | No adecuado | - |
| Disco de datos | No adecuado | - |
| Directorio de transporte global de SAP | No | [No compatible](https://launchpad.support.sap.com/#/notes/2015553) |
| SAP sapmnt | No | No compatible |
| Almacenamiento de copia de seguridad | Adecuado | - |
| Recursos compartidos/disco compartido | No disponible | Necesita Azure Files o de terceros |
| Resistencia | LRS y GRS | No hay almacenamiento con redundancia de zona disponible para los discos |
| Latencia | high | Demasiado alta para el uso de DBMS, el directorio de transporte global de SAP o sapmnt/saploc |
| SLA DE IOPS | No | - |
| Número máximo de IOPS por disco | 500 | Independiente del tamaño del disco |
| SLA de rendimiento | No | - |
| Certificación de HANA | No | - |
| Instantáneas de disco posibles | Sí | - |
| Instantáneas de máquina virtual de Azure Backup posibles | Sí | - |
| Costes | Bajo | - |



**Resumen:** HDD estándar es un tipo de almacenamiento de Azure que solo se debe usar para almacenar copias de seguridad de SAP. Solo se debe usar como VHD de base para sistemas más bien inactivos, como sistemas retirados que se utilizan para buscar datos aquí y allá. Sin embargo, las máquinas virtuales de producción, de control de calidad o de desarrollo activo no se deben basar en ese almacenamiento. Tampoco deben hospedarse archivos de base de datos en ese almacenamiento.


## <a name="azure-vm-limits-in-storage-traffic"></a>Límites de máquinas virtuales de Azure en el tráfico de almacenamiento
En lo opuesto a los escenarios locales, el tipo de máquina virtual individual que está seleccionando desempeña un papel fundamental en el ancho de banda de almacenamiento que puede lograr. En el caso de los diferentes tipos de almacenamiento, debe tener en cuenta lo siguiente:

| Tipo de almacenamiento| Linux | Windows | Comentarios |
| --- | --- | --- | --- |
| HDD estándar | [Tamaños de las máquinas virtuales Linux en Azure](../../sizes.md) | [Tamaños de las máquinas virtuales Windows en Azure](../../sizes.md) | Probablemente difícil alcanzar los límites de almacenamiento de máquinas virtuales medianas o grandes |
| SSD estándar | [Tamaños de las máquinas virtuales Linux en Azure](../../sizes.md) | [Tamaños de las máquinas virtuales Windows en Azure](../../sizes.md) | Probablemente difícil alcanzar los límites de almacenamiento de máquinas virtuales medianas o grandes |
| Premium Storage | [Tamaños de las máquinas virtuales Linux en Azure](../../sizes.md) | [Tamaños de las máquinas virtuales Windows en Azure](../../sizes.md) | Fácil de alcanzar los límites de las máquinas virtuales de rendimiento de almacenamiento o IOPS con la configuración de almacenamiento |
| Almacenamiento en disco Ultra | [Tamaños de las máquinas virtuales Linux en Azure](../../sizes.md) | [Tamaños de las máquinas virtuales Windows en Azure](../../sizes.md) | Fácil de alcanzar los límites de las máquinas virtuales de rendimiento de almacenamiento o IOPS con la configuración de almacenamiento |
| Azure NetApp Files | [Tamaños de las máquinas virtuales Linux en Azure](../../sizes.md) | [Tamaños de las máquinas virtuales Windows en Azure](../../sizes.md) | El tráfico de almacenamiento usa ancho de banda de rendimiento de red y no ancho de banda de almacenamiento. |

Como limitación, puede tener en cuenta lo siguiente:

- Cuanto menor es la máquina virtual, menos discos puede adjuntar. Esto no se aplica a ANF. Dado que monta recursos compartidos de NFS o SMB, no encuentra un límite en el número de volúmenes compartidos que se van a adjuntar.
- Las máquinas virtuales tienen límites de rendimiento de E/S e IOPS que se podrían superar fácilmente con discos de Premium Storage y discos Ultra.
- Con ANF, el tráfico a los volúmenes compartidos está consumiendo el ancho de banda de red de la máquina virtual y no el ancho de banda de almacenamiento.
- Con grandes volúmenes de NFS en el espacio de capacidad de TiB de dos dígitos, el rendimiento que accede a dicho volumen desde una sola VM se estabilizará en función de los límites de Linux para una sola sesión que interactúa con el volumen compartido. 

A medida que incrementa el tamaño de las máquinas virtuales de Azure en el ciclo de vida de un sistema SAP, debe evaluar los límites de IOPS y rendimiento de almacenamiento del tipo de máquina virtual nuevo y mayor. En algunos casos, también podría convenir ajustar la configuración del almacenamiento a las nuevas funcionalidades de la máquina virtual de Azure. 


## <a name="striping-or-not-striping"></a>Posibilidad de seccionar o no
La creación de un conjunto seccionado de varios discos de Azure en un volumen más grande permite acumular la IOPS y el rendimiento de los discos individuales en un solo volumen. Se usa solo para Azure Standard Storage y Azure Premium Storage. El disco Ultra de Azure, donde puede configurar el rendimiento e IOPS independientemente de la capacidad de un disco, no requiere el uso de conjuntos seccionados. Los volúmenes compartidos basados en NFS o SMB no se pueden seccionar. Debido a la naturaleza no lineal del rendimiento e IOPS de Azure Premium Storage, puede aprovisionar una capacidad menor con los mismos valores de IOPS y rendimiento que los discos individuales de Azure Premium Storage grandes. Este es el método para lograr un mayor rendimiento o IOPS a un costo más bajo con Azure Premium Storage. Por ejemplo, con el seccionamiento en dos discos de almacenamiento premium P15 se consigue un rendimiento de: 

- 250 MiB/s. Dicho volumen va a tener una capacidad de 512 GiB. Si desea tener un único disco que proporcione un rendimiento de 250 MiB por segundo, deberá elegir un disco P40 con 2 TiB de capacidad. 
- 400 MiB/s mediante el seccionamiento de cuatro discos de almacenamiento premium P10 con una capacidad total de 512 GiB por medio del seccionamiento. Si desea tener un solo disco con un rendimiento mínimo de 500 MiB por segundo, debe elegir un disco de Premium Storage de P60 con 8 TiB. Dado que el costo del almacenamiento premium es casi lineal con la capacidad, puede detectar el ahorro en los costos mediante el uso del seccionamiento.

Algunas reglas deben seguirse en el seccionamiento:

- No se debe usar ningún almacenamiento configurado en la máquina virtual, ya que Azure Storage mantiene los datos redundantes.
- Los discos a los que se aplica el conjunto de bandas deben tener el mismo tamaño.

La división en varios discos más pequeños es la mejor manera de lograr una buena relación precio/rendimiento con Azure Premium Storage. Se entiende que el seccionamiento tiene una sobrecarga adicional de implementación y administración.

En el caso de recomendaciones específicas de tamaño de seccionamiento, lea la documentación de los distintos DBMS, como [Configuraciones de almacenamiento de máquinas virtuales de Azure en SAP HANA](./hana-vm-operations-storage.md).




## <a name="next-steps"></a>Pasos siguientes
Lea los artículos:

- [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](./dbms_guide_general.md)
- [Configuraciones de almacenamiento de máquinas virtuales de Azure en SAP HANA](./hana-vm-operations-storage.md)
