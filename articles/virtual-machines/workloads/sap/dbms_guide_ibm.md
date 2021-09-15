---
title: Implementación de DBMS de Azure Virtual Machines de IBM Db2 para la carga de trabajo de SAP | Microsoft Docs
description: Implementación de DBMS de Azure Virtual Machines de IBM Db2 para carga de trabajo de SAP
services: virtual-machines-linux,virtual-machines-windows
author: msjuergent
manager: bburns
tags: azure-resource-manager
keywords: Azure, Db2, SAP, IBM
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/17/2021
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4e63e2603b6625c7eaee602b107c2d01c40a8ac8
ms.sourcegitcommit: 16e25fb3a5fa8fc054e16f30dc925a7276f2a4cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122830671"
---
# <a name="ibm-db2-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Implementación de DBMS de Azure Virtual Machines de IBM Db2 para carga de trabajo de SAP

Con Microsoft Azure, puede migrar la aplicación SAP existente que se ejecuta en IBM Db2 para Linux, UNIX y Windows (LUW) a Azure Virtual Machines. Con SAP en IBM Db2 para LUW, los administradores y los desarrolladores pueden seguir usando las mismas herramientas de desarrollo y administración, disponibles de forma local.
Se puede encontrar información general sobre la ejecución de SAP Business Suite en IBM Db2 para LUW en SAP Community Network (SCN) en <https://www.sap.com/community/topic/db2-for-linux-unix-and-windows.html>.

Para obtener más información y actualizaciones de SAP en Db2 para LUW en Azure, vea la nota de SAP [2233094]. 

Hay varios artículos publicados sobre la carga de trabajo de SAP en Azure.  Se recomienda empezar por la [Introducción a la carga de trabajo de SAP en Azure](./get-started.md) y después elegir el área de interés.

Las notas de SAP siguientes están relacionadas con SAP en Azure con respecto al área que se describe en este documento:

| Número de nota |Título |
| --- |--- |
| [1928533] |SAP Applications on Azure: Supported Products and Azure VM Types (Aplicaciones de SAP en Azure: productos y tipos de máquina virtual de Azure compatibles) |
| [2015553] |SAP on Microsoft Azure: Support Prerequisites (Requisitos previos de soporte técnico de SAP en Microsoft Azure) |
| [1999351] |Troubleshooting Enhanced Azure Monitoring for SAP (Solución de problemas de la supervisión mejorada de Azure para SAP) |
| [2178632] |Key Monitoring Metrics for SAP on Microsoft Azure (Métricas de supervisión clave para SAP en Microsoft Azure) |
| [1409604] |Virtualization on Windows: Enhanced Monitoring (Virtualización en Windows: supervisión mejorada) |
| [2191498] |SAP on Linux with Azure: Enhanced Monitoring (Virtualización en Windows: supervisión mejorada) |
| [2233094] |DB6: SAP Applications on Azure Using IBM DB2 for Linux, UNIX, and Windows - Additional Information (DB6: aplicaciones de SAP en Azure con IBM DB2 para Linux, UNIX y Windows: información adicional) |
| [2243692] |Linux on Microsoft Azure (IaaS) VM: SAP license issues (Linux y máquinas virtuales de Microsoft Azure (IaaS): problemas de licencia de SAP) |
| [1984787] |SUSE LINUX Enterprise Server 12: Notas de instalación |
| [2002167] |Red Hat Enterprise Linux 7.x: Instalación y actualización |
| [1597355] |Recomendación de espacio de intercambio para Linux |

Como lectura previa a este documento, debe haber leído el documento [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md), así como otras guías de la [documentación de carga de trabajo de SAP en Azure](./get-started.md). 


## <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM Db2 para Linux, UNIX y compatibilidad de versiones de Windows
SAP en IBM Db2 para LUW en los servicios de Microsoft Azure Virtual Machines es compatible a partir de la versión 10.5 de Db2.

Para más información de los tipos de máquina virtual de Azure y los productos de SAP compatibles, consulte la nota de SAP [1928533].

## <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Instrucciones de configuración de IBM Db2 para Linux, UNIX y Windows para instalaciones de SAP en máquinas virtuales de Azure
### <a name="storage-configuration"></a>Configuración de almacenamiento
Para obtener información general sobre los tipos de Azure Storage para la carga de trabajo de SAP, consulte el artículo [Tipos de Azure Storage para una carga de trabajo de SAP](./planning-guide-storage.md). Todos los archivos de base de datos deben almacenarse en discos montados de almacenamiento en bloque de Azure (Windows: NFFS; Linux: xfs o ext3). Los volúmenes compartidos remotos como los servicios de Azure en los escenarios enumerados **NO** son compatibles con los archivos de base de datos db2: 

* [Microsoft Azure File Service](/archive/blogs/windowsazurestorage/introducing-microsoft-azure-file-service) para todos los sistemas operativos invitados

* [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) para db2 que se ejecuta en el sistema operativo invitado Windows. 

Los volúmenes compartidos remotos como los servicios de Azure en los escenarios enumerados son compatibles con los archivos de base de datos db2: 
 
* Se admite el hospedaje de archivos de datos y de registro db2 basados en el sistema operativo invitado Linux en recursos compartidos de NFS hospedados Azure NetApp Files.

Si se usan discos basados en Azure Page BLOB Storage o Managed Disks, las afirmaciones realizadas en [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md) también se aplican a las implementaciones con el DBMS de Db2.

Como se explicó anteriormente en la parte general del documento, existen cuotas en el rendimiento IOPS para discos de Azure. Las cuotas exactas varían según el tipo de máquina virtual que se utilice. Puede encontrar una lista de tipos de VM con sus cuotas [aquí (Linux)][virtual-machines-sizes-linux] y [aquí (Windows)][virtual-machines-sizes-windows].

Mientras la cuota actual de IOPS por disco sea suficiente, se pueden almacenar todos los archivos de base de datos en un solo disco montado. Mientras que siempre debe separar los archivos de datos y los de registro de transacciones en otros discos o discos duros virtuales.

Para obtener las consideraciones de rendimiento, consulte también el capítulo "Data Safety and Performance Considerations for Database Directories" (Consideraciones de seguridad y rendimiento de directorios de bases de datos) en las guías de instalación de SAP.

También puede usar grupos de almacenamiento de Windows (solo disponibles en Windows Server 2012 y versiones posteriores), como se describe en [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md), o LVM o mdadm en Linux para crear un dispositivo lógico de gran tamaño en varios discos.

<!-- log_dir, sapdata and saptmp are terms in the SAP and DB2 world and now spelling errors -->

Para la máquina virtual de Azure de la serie M, se puede reducir la latencia de escritura en los registros de transacciones mediante factores, en comparación con el rendimiento de Azure Premium Storage, cuando se usa el Acelerador de escritura de Azure. Por tanto, se debe implementar el Acelerador de escritura de Azure para los discos duros virtuales que forman el volumen para los registros de transacciones de Db2. En el documento [Acelerador de escritura](../../how-to-enable-write-accelerator.md) se pueden leer los detalles.

IBM Db2 LUW 11.5 publicó compatibilidad con el tamaño del sector de 4 KB. Para las versiones anteriores de Db2, se debe usar un tamaño de sector de 512 bytes. Los discos SSD prémium son nativos de 4 KB y tienen una emulación de 512 bytes. El disco Ultra usa un tamaño de sector de 4 KB de forma predeterminada. Puede habilitar el tamaño del sector de 512 bytes durante la creación del disco Ultra. Los detalles están disponibles en [Uso de los discos Ultra de Azure](../../disks-enable-ultra-ssd.md#deploy-an-ultra-disk---512-byte-sector-size). Este tamaño de sector de 512 bytes es un requisito previo para las versiones de IBM Db2 LUW inferiores a 11.5.

En Windows, si se usan bloques de almacenamiento para las rutas de acceso de almacenamiento de Db2 para los directorios `log_dir`, `sapdata` y `saptmp`, debe especificar un tamaño de sector de disco físico de 512 bytes. Si usa bloques de almacenamiento de Windows, debe crearlos manualmente en la interfaz de la línea de comandos con el parámetro `-LogicalSectorSizeDefault`. Para más información, consulte <https://technet.microsoft.com/itpro/powershell/windows/storage/new-storagepool>.


## <a name="recommendation-on-vm-and-disk-structure-for-ibm-db2-deployment"></a>Recomendación sobre la estructura de disco y las máquinas virtuales para la implementación de IBM Db2

Las aplicaciones de IBM Db2 para SAP NetWeaver se admiten en cualquier tipo de máquina virtual que aparezca en la nota de soporte de SAP [1928533].  Las familias de máquinas virtuales recomendadas para ejecutar la base de datos IBM Db2 son las series Esd_v4/Eas_v4/Es_v3 y M/M_v2 para grandes bases de datos de varios terabytes. El rendimiento de escritura en disco del registro de transacciones de IBM Db2 se puede mejorar si se habilita el Acelerador de escritura de la serie M. 

A continuación, se facilita una configuración de línea de base para varios tamaños y usos de las implementaciones de SAP en DB2, que van de pequeña a grande. La lista se basa en Azure Premium Storage. Sin embargo, el disco Ultra de Azure también es totalmente compatible con DB2 y también se puede usar. Use los valores de capacidad, rendimiento de ráfaga e IOPS de ráfaga para definir la configuración del disco Ultra. Puede limitar la IOPS para /db2/```<SID>```/log_dir en torno a 5000 IOPS. 

#### <a name="extra-small-sap-system-database-size-50---200-gb-example-solution-manager"></a>Sistema SAP extra pequeño: tamaño de la base de datos de 50 a 200 GB: ejemplo de administrador de soluciones
| Nombre/tamaño de la máquina virtual |Punto de montaje de Db2 |Disco Premium de Azure |Número de discos |E/S |Rendimiento [MB/s] |Tamaño [GB] |IOPS de ráfaga |Rendimiento de ráfaga [GB] | Stripe size (Tamaño de las franjas) | Almacenamiento en memoria caché |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|E4ds_v4 |/db2 |P6 |1 |240  |50  |64  |3500  |170  ||  |
|vCPU: 4 |/db2/```<SID>```/sapdata |P10 |2 |1,000  |200  |256  |7000  |340  |256 KB |ReadOnly |
|RAM: 32 GiB |/db2/```<SID>```/saptmp |P6 |1 |240  |50  |128  |3500  |170  | ||
| |/db2/```<SID>```/log_dir |P6 |2 |480  |100  |128  |7000  |340  |64 KB ||
| |/db2/```<SID>```/offline_log_dir |P10 |1 |500  |100  |128  |3500  |170  || |

#### <a name="small-sap-system-database-size-200---750-gb-small-business-suite"></a>Sistema SAP pequeño: tamaño de la base de datos de 200 a 750 GB: Small Business Suite
| Nombre/tamaño de la máquina virtual |Punto de montaje de Db2 |Disco Premium de Azure |Número de discos |E/S |Rendimiento [MB/s] |Tamaño [GB] |IOPS de ráfaga |Rendimiento de ráfaga [GB] | Stripe size (Tamaño de las franjas) | Almacenamiento en memoria caché |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|E16ds_v4 |/db2 |P6 |1 |240  |50  |64  |3500  |170  || |
|vCPU: 16 |/db2/```<SID>```/sapdata |P15 |4 |4400  |500  |1024  |14 000  |680  |256 KB |ReadOnly |
|RAM: 128 GB |/db2/```<SID>```/saptmp |P6 |2 |480  |100  |128  |7000  |340  |128 KB ||
| |/db2/```<SID>```/log_dir |P15 |2 |2200  |250  |512  |7000  |340  |64 KB ||
| |/db2/```<SID>```/offline_log_dir |P10 |1 |500  |100  |128  |3500  |170  ||| 

#### <a name="medium-sap-system-database-size-500---1000-gb-small-business-suite"></a>Sistema SAP medio: tamaño de base de datos de 500 a 1000 GB: Small Business Suite
| Nombre/tamaño de la máquina virtual |Punto de montaje de Db2 |Disco Premium de Azure |Número de discos |E/S |Rendimiento [MB/s] |Tamaño [GB] |IOPS de ráfaga |Rendimiento de ráfaga [GB] | Stripe size (Tamaño de las franjas) | Almacenamiento en memoria caché |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|E32ds_v4 |/db2 |P6 |1 |240  |50  |64  |3500  |170  || |
|vCPU: 32 |/db2/```<SID>```/sapdata |P30 |2 |10 000  |400  |2048  |10 000  |400  |256 KB |ReadOnly |
|RAM: 256 GiB |/db2/```<SID>```/saptmp |P10 |2 |1,000  |200  |256  |7000  |340  |128 KB ||
| |/db2/```<SID>```/log_dir |P20 |2 |4600  |300  |1024  |7000  |340  |64 KB ||
| |/db2/```<SID>```/offline_log_dir |P15 |1 |1 100  |125  |256  |3500  |170  ||| 

#### <a name="large-sap-system-database-size-750---2000-gb-business-suite"></a>Sistema SAP grande: tamaño de la base de datos de 750 a 2000 GB: Business Suite
| Nombre/tamaño de la máquina virtual |Punto de montaje de Db2 |Disco Premium de Azure |Número de discos |E/S |Rendimiento [MB/s] |Tamaño [GB] |IOPS de ráfaga |Rendimiento de ráfaga [GB] | Stripe size (Tamaño de las franjas) | Almacenamiento en memoria caché |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|E64ds_v4 |/db2 |P6 |1 |240  |50  |64  |3500  |170  || |
|vCPU: 64 |/db2/```<SID>```/sapdata |P30 |4 |20 000  |800  |4.096  |20 000  |800  |256 KB |ReadOnly |
|RAM: 504 GiB |/db2/```<SID>```/saptmp |P15 |2 |2200  |250  |512  |7000  |340  |128 KB ||
| |/db2/```<SID>```/log_dir |P20 |4 |9200  |600  |2048  |14 000  |680  |64 KB ||
| |/db2/```<SID>```/offline_log_dir |P20 |1 |2,300  |150  |512  |3500  |170  || |

#### <a name="large-multi-terabyte-sap-system-database-size-2-tb-global-business-suite-system"></a>Sistema SAP grande de varios terabytes: tamaño de base de datos de 2 TB o más: Sistema Global Business Suite
| Nombre/tamaño de la máquina virtual |Punto de montaje de Db2 |Disco Premium de Azure |Número de discos |E/S |Rendimiento [MB/s] |Tamaño [GB] |IOPS de ráfaga |Rendimiento de ráfaga [GB] | Stripe size (Tamaño de las franjas) | Almacenamiento en memoria caché |
| --- | --- | --- | :---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
|M128s |/db2 |P10 |1 |500  |100  |128  |3500  |170  || |
|vCPU: 128 |/db2/```<SID>```/sapdata |P40 |4 |30,000  |1000  |8192  |30,000  |1000  |256 KB |ReadOnly |
|RAM:  2048 GiB |/db2/```<SID>```/saptmp |P20 |2 |4600  |300  |1024  |7000  |340  |128 KB ||
| |/db2/```<SID>```/log_dir |P30 |4 |20 000  |800  |4.096  |20 000  |800  |64 KB |WriteAccelerator |
| |/db2/```<SID>```/offline_log_dir |P30 |1 |5\.000  |200  |1024  |5\.000  |200  || |


### <a name="using-azure-netapp-files"></a>Uso de Azure NetApp Files
El uso de volúmenes NFS v4.1 basados en Azure NetApp Files (ANF) se admite con IBM Db2, hospedado en el sistema operativo invitado SUSE o Red Hat Linux. Debe crear al menos cuatro volúmenes diferentes como los siguientes:

- Un volumen compartido para saptmp1, sapmnt, usr_sap, ```<sid>```_home, db2```<sid>```_home, db2_software
- Un volumen de datos para los directorios sapdata del 1 al 4
- Un volumen de registro para el directorio de registro de la fase de puesta al día
- Un volumen para los archivos de registro y las copias de seguridad

Un quinto volumen posible podría ser un volumen de ANF que se usa para copias de seguridad a más largo plazo usadas para crear instantáneas y almacenarlas en el almacén de blobs de Azure.

La configuración podría ser similar a la siguiente:

![Ejemplo de una configuración de Db2 mediante ANF](./media/dbms_guide_ibm/anf-configuration-example.png)


El nivel de rendimiento y el tamaño de los volúmenes hospedados en ANF deben elegirse en función de los requisitos de rendimiento. Pero se recomienda usar el nivel de rendimiento Ultra para los datos y el volumen de registro. No se admite la combinación de tipos de almacenamiento compartido y en bloque para los datos y el volumen de registro.

En cuanto a las opciones de montaje, el montaje de esos volúmenes podría parecerse al siguiente (debe reemplazar ```<SID>``` y ```<sid>``` por el SID de su sistema SAP):

```
vi /etc/idmapd.conf   
 # Example
 [General]
 Domain = defaultv4iddomain.com
 [Mapping]
 Nobody-User = nobody
 Nobody-Group = nobody

mount -t nfs -o rw,hard,sync,rsize=1048576,wsize=1048576,sec=sys,vers=4.1,tcp 172.17.10.4:/db2shared /mnt 
mkdir -p /db2/Software /db2/AN1/saptmp /usr/sap/<SID> /sapmnt/<SID> /home/<sid>adm /db2/db2<sid> /db2/<SID>/db2_software
mkdir -p /mnt/Software /mnt/saptmp  /mnt/usr_sap /mnt/sapmnt /mnt/<sid>_home /mnt/db2_software /mnt/db2<sid>
umount /mnt

mount -t nfs -o rw,hard,sync,rsize=1048576,wsize=1048576,sec=sys,vers=4.1,tcp 172.17.10.4:/db2data /mnt
mkdir -p /db2/AN1/sapdata/sapdata1 /db2/AN1/sapdata/sapdata2 /db2/AN1/sapdata/sapdata3 /db2/AN1/sapdata/sapdata4
mkdir -p /mnt/sapdata1 /mnt/sapdata2 /mnt/sapdata3 /mnt/sapdata4
umount /mnt

mount -t nfs -o rw,hard,sync,rsize=1048576,wsize=1048576,sec=sys,vers=4.1,tcp 172.17.10.4:/db2log /mnt 
mkdir /db2/AN1/log_dir
mkdir /mnt/log_dir
umount /mnt

mount -t nfs -o rw,hard,sync,rsize=1048576,wsize=1048576,sec=sys,vers=4.1,tcp 172.17.10.4:/db2backup /mnt
mkdir /db2/AN1/backup
mkdir /mnt/backup
mkdir /db2/AN1/offline_log_dir /db2/AN1/db2dump
mkdir /mnt/offline_log_dir /mnt/db2dump
umount /mnt
```

>[!NOTE]
> Las opciones de montaje "hard" y "sync" son necesarias.


### <a name="backuprestore"></a>Copia de seguridad y restauración
La funcionalidad de copia de seguridad y restauración para IBM Db2 para LUW es compatible del mismo modo que en los sistemas operativos Windows Server y Hyper-V estándares.

Asegúrese de seguir una estrategia establecida de copias de seguridad de bases de datos válida. 

Como en las implementaciones sin sistema operativo, el rendimiento de las copias de seguridad y las restauraciones depende de cuántos volúmenes puedan leerse en paralelo y el rendimiento que estos volúmenes puedan tener. Además, el consumo de CPU que se emplea en la compresión de copias de seguridad podría desempeñar un papel importante en las máquinas virtuales con ocho subprocesos de CPU como máximo. Por lo tanto, se pueden asumir los siguientes puntos:

* Cuantos menos discos se utilicen para almacenar los dispositivos de las bases de datos, menor será el rendimiento general de lectura
* Cuantos menos subprocesos de CPU haya en la máquina virtual, mayor será el impacto en la compresión de copias de seguridad.
* Cuantos menos destinos (directorios de seccionamiento, discos) en los que escribir la copia de seguridad haya, menor será el rendimiento

Para aumentar la cantidad de destinos en los que escribir, existen dos opciones que se pueden usar o combinar según sus necesidades:

* Seccionamiento del volumen de destino de la copia de seguridad en varios discos para mejorar el rendimiento de IOPS en ese volumen seccionado
* Uso de más de un directorio de destino en el que escribir copias de seguridad

>[!NOTE]
>Db2 en Windows no es compatible con la tecnología de Windows VSS. Como resultado, no se puede aprovechar la copia de seguridad de máquina virtual coherente con la aplicación del servicio Azure Backup para las máquinas virtuales en las que se implementa el DBMS de Db2.

### <a name="high-availability-and-disaster-recovery"></a>Alta disponibilidad y recuperación ante desastres

#### <a name="linux-pacemaker"></a>Linux Pacemaker

La alta disponibilidad y la recuperación ante desastres (HADR) de Db2 con Pacemaker sí son compatibles. Se admiten los sistemas operativos SLES y RHEL. Esta configuración habilita la alta disponibilidad de IBM Db2 para SAP. Guías de implementación:
* SLES: [Alta disponibilidad de IBM Db2 LUW en máquinas virtuales de Azure en SUSE Linux Enterprise Server con Pacemaker](dbms-guide-ha-ibm.md) 
* RHEL: [Alta disponibilidad de IBM Db2 LUW en máquinas virtuales de Azure en Red Hat Enterprise Linux Server](high-availability-guide-rhel-ibm-db2-luw.md)

#### <a name="windows-cluster-server"></a>Windows Cluster Server

Microsoft Cluster Server (MSCS) no es compatible.

La alta disponibilidad y la recuperación ante desastres (HADR) de Db2 sí son compatibles. Si las máquinas virtuales de la configuración de alta disponibilidad tienen una resolución de nombres correcta, la configuración de Azure no será diferente a cualquier otra realizada de forma local. No se recomienda depender exclusivamente de la resolución de direcciones IP.

No utilice la replicación geográfica para las cuentas de almacenamiento donde se almacenan los discos de base de datos. Para obtener más información, consulte el documento [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md). 

### <a name="accelerated-networking"></a>Redes aceleradas
Para las implementaciones de Db2 en Windows, se recomienda usar la funcionalidad Redes aceleradas de Azure, como se describe en el documento sobre [redes aceleradas de Azure](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/). Tenga en cuenta también las recomendaciones realizadas en [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md). 


### <a name="specifics-for-linux-deployments"></a>Características específicas para las implementaciones de Linux
Mientras la cuota actual de IOPS por disco sea suficiente, se pueden almacenar todos los archivos de base de datos en un solo disco. Mientras que siempre debe separar los archivos de datos y los de registro de transacciones en otros discos.

Si el rendimiento de IOPS o E/S de un solo disco duro virtual de Azure no es suficiente, puede usar LVM (Administrador de discos lógicos) o MDADM como se describe en el documento [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md) para crear un dispositivo lógico de gran tamaño en varios discos.
Para los discos que contienen rutas de acceso de almacenamiento de Db2 para directorios sapdata y saptmp, debe especificar un tamaño de sector de disco físico de 512 KB.

<!-- sapdata and saptmp are terms in the SAP and DB2 world and now spelling errors -->


### <a name="other"></a>Otros
Todas las demás áreas generales, como los conjuntos de disponibilidad de Azure o la supervisión de SAP que se describen en el documento [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md) también se aplican a las implementaciones de máquinas virtuales con la base de datos de IBM.

## <a name="next-steps"></a>Pasos siguientes
Lea el artículo 

- [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](dbms_guide_general.md)


[767598]:https://launchpad.support.sap.com/#/notes/767598
[773830]:https://launchpad.support.sap.com/#/notes/773830
[826037]:https://launchpad.support.sap.com/#/notes/826037
[965908]:https://launchpad.support.sap.com/#/notes/965908
[1031096]:https://launchpad.support.sap.com/#/notes/1031096
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1139904]:https://launchpad.support.sap.com/#/notes/1139904
[1173395]:https://launchpad.support.sap.com/#/notes/1173395
[1245200]:https://launchpad.support.sap.com/#/notes/1245200
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1558958]:https://launchpad.support.sap.com/#/notes/1558958
[1585981]:https://launchpad.support.sap.com/#/notes/1585981
[1588316]:https://launchpad.support.sap.com/#/notes/1588316
[1590719]:https://launchpad.support.sap.com/#/notes/1590719
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1605680]:https://launchpad.support.sap.com/#/notes/1605680
[1619720]:https://launchpad.support.sap.com/#/notes/1619720
[1619726]:https://launchpad.support.sap.com/#/notes/1619726
[1619967]:https://launchpad.support.sap.com/#/notes/1619967
[1750510]:https://launchpad.support.sap.com/#/notes/1750510
[1752266]:https://launchpad.support.sap.com/#/notes/1752266
[1757924]:https://launchpad.support.sap.com/#/notes/1757924
[1757928]:https://launchpad.support.sap.com/#/notes/1757928
[1758182]:https://launchpad.support.sap.com/#/notes/1758182
[1758496]:https://launchpad.support.sap.com/#/notes/1758496
[1772688]:https://launchpad.support.sap.com/#/notes/1772688
[1814258]:https://launchpad.support.sap.com/#/notes/1814258
[1882376]:https://launchpad.support.sap.com/#/notes/1882376
[1909114]:https://launchpad.support.sap.com/#/notes/1909114
[1922555]:https://launchpad.support.sap.com/#/notes/1922555
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1941500]:https://launchpad.support.sap.com/#/notes/1941500
[1956005]:https://launchpad.support.sap.com/#/notes/1956005
[1973241]:https://launchpad.support.sap.com/#/notes/1973241
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2121797]:https://launchpad.support.sap.com/#/notes/2121797
[2134316]:https://launchpad.support.sap.com/#/notes/2134316
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[dbms-guide]:dbms-guide.md 
[dbms-guide-2.1]:dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f 
[dbms-guide-2.2]:dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 
[dbms-guide-2.3]:dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 
[dbms-guide-2]:dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 
[dbms-guide-3]:dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 
[dbms-guide-5.5.1]:dbms-guide.md#0fef0e79-d3fe-4ae2-85af-73666a6f7268 
[dbms-guide-5.5.2]:dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b 
[dbms-guide-5.6]:dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 
[dbms-guide-5.8]:dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 
[dbms-guide-5]:dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 
[dbms-guide-8.4.1]:dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 
[dbms-guide-8.4.2]:dbms-guide.md#23c78d3b-ca5a-4e72-8a24-645d141a3f5d 
[dbms-guide-8.4.3]:dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c 
[dbms-guide-8.4.4]:dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 
[dbms-guide-900-sap-cache-server-on-premises]:dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8
[dbms-guide-managed-disks]:dbms-guide.md#f42c6cb5-d563-484d-9667-b07ae51bce29

[dbms-guide-figure-100]:media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:deployment-guide.md 
[deployment-guide-2.2]:deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 
[deployment-guide-3.1.2]:deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab 
[deployment-guide-3.2]:deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 
[deployment-guide-3.3]:deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 
[deployment-guide-3.4]:deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e 
[deployment-guide-4.1]:deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 
[deployment-guide-4.2]:deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e 
[deployment-guide-4.3]:deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc 
[deployment-guide-4.4.2]:deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 
[deployment-guide-4.4]:deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d 
[deployment-guide-4.5.1]:deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 
[deployment-guide-4.5.2]:deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f 
[deployment-guide-4.5]:deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca 
[deployment-guide-5.1]:deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 
[deployment-guide-5.2]:deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1
[deployment-guide-5.3]:deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 

[deployment-guide-configure-monitoring-scenario-1]:deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b 
[deployment-guide-configure-proxy]:deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d 
[deployment-guide-figure-100]:media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:deployment-guide.md#figure-11
[deployment-guide-figure-1100]:media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:deployment-guide.md#figure-14
[deployment-guide-figure-1400]:media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:deployment-guide.md#figure-5
[deployment-guide-figure-50]:media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:deployment-guide.md#figure-6
[deployment-guide-figure-600]:media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:deployment-guide.md#figure-7
[deployment-guide-figure-700]:media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b

[deploy-template-cli]:../../../resource-group-template-deploy-cli.md
[deploy-template-portal]:../../../resource-group-template-deploy-portal.md
[deploy-template-powershell]:../../../resource-group-template-deploy.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[getting-started-dbms]:get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:../../virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:../../virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:../../virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:../../virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:../../virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:../../virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:https://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:planning-guide.md 
[planning-guide-1.2]:planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff 
[planning-guide-11]:planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 
[planning-guide-11.4.1]:planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 
[planning-guide-11.5]:planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f 
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803 
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 
[planning-guide-3.1]:planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a 
[planning-guide-3.2.1]:planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 
[planning-guide-3.2.2]:planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 
[planning-guide-3.2.3]:planning-guide.md#18810088-f9be-4c97-958a-27996255c665 
[planning-guide-3.2]:planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 
[planning-guide-3.3.2]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 
[planning-guide-5.1.1]:planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 
[planning-guide-5.1.2]:planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c 
[planning-guide-5.2.1]:planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef 
[planning-guide-5.2.2]:planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 
[planning-guide-5.2]:planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 
[planning-guide-5.3.1]:planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 
[planning-guide-5.3.2]:planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a 
[planning-guide-5.4.2]:planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 
[planning-guide-5.5.1]:planning-guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 
[planning-guide-5.5.3]:planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d 
[planning-guide-7.1]:planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 
[planning-guide-7]:planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 
[planning-guide-9.1]:planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 
[planning-guide-azure-premium-storage]:planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 

[planning-guide-figure-100]:media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd 
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f 

[powershell-install-configure]:https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps
[resource-group-authoring-templates]:../../../resource-group-authoring-templates.md
[resource-group-overview]:../../../azure-resource-manager/management/overview.md
[resource-groups-networking]:../../../networking/networking-overview.md
[sap-pam]:https://support.sap.com/pam 
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fapplication-workloads%2Fsap%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../../../storage/common/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../../../storage/common/storage-azure-cli.md#copy-blobs
[storage-introduction]:../../../storage/common/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../../../storage/common/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../../disks-types.md
