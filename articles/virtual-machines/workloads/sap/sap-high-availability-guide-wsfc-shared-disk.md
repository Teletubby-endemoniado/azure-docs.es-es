---
title: Agrupación de instancias de ASCS/SCS de SAP en WSFC con discos compartidos en Azure | Microsoft Docs
description: Aprenda a agrupar una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un disco compartido de clúster.
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: f6fb85f8-c77a-4af1-bde8-1de7e4425d2e
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/29/2021
ms.author: radeltch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dc233e3aac51255be4b6b7befde322216ae7f053
ms.sourcegitcommit: af303268d0396c0887a21ec34c9f49106bb0c9c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2021
ms.locfileid: "129754565"
---
# <a name="cluster-an-sap-ascsscs-instance-on-a-windows-failover-cluster-by-using-a-cluster-shared-disk-in-azure"></a>Agrupación de una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un disco compartido de clúster en Azure

> ![SO Windows][Logo_Windows] Windows
>

Los clústeres de conmutación por error de Windows Server son la base de una instalación de ASCS/SCS de SAP de alta disponibilidad y DBMS en Windows.

Un clúster de conmutación por error es un grupo de 1+n servidores independientes (nodos) que colaboran para aumentar la disponibilidad de aplicaciones y servicios. Si se produce un error de nodo, los clústeres de conmutación por error de Windows Server calculan el número de errores que se pueden producir y mantiene un clúster en buen estado para proporcionar aplicaciones y servicios. Para conseguir clústeres de conmutación por error, puede elegir entre distintos modos de cuórum.

## <a name="prerequisites"></a>Prerrequisitos
Antes de comenzar las tareas de este artículo, consulte el siguiente artículo:

* [Escenarios y arquitectura de alta disponibilidad de Azure Virtual Machines para SAP NetWeaver][sap-high-availability-architecture-scenarios]


## <a name="windows-server-failover-clustering-in-azure"></a>Clústeres de conmutación por error de Windows Server en Azure

La agrupación en clústeres de conmutación por error con Azure Virtual Machines requiere pasos adicionales de configuración. Al crear un clúster, debe establecer varias direcciones IP y nombres de host virtual para la instancia de ASCS/SCS de SAP.

### <a name="name-resolution-in-azure-and-the-cluster-virtual-host-name"></a>Resolución de nombres en Azure y el nombre de host virtual del clúster

La plataforma en la nube de Azure no ofrece la opción de configurar direcciones IP virtuales, como direcciones IP flotantes. Necesita una solución alternativa para configurar una dirección IP virtual que se comunique con el recurso de clúster en la nube. 

El servicio Azure Load Balancer proporciona un *equilibrador de carga interno* para Azure. Con el equilibrador de carga interno, los clientes se comunican con el clúster a través de la dirección IP virtual del clúster. 

Implemente el equilibrador de carga interno en el grupo de recursos que contiene los nodos del clúster. A continuación, configure todas las reglas de reenvío de puertos necesarias utilizando los puertos de sondeo del equilibrador de carga interno. Los clientes se pueden conectar por medio del nombre de host virtual. El servidor DNS resuelve la dirección IP del clúster y el equilibrador de carga interno controla el enrutamiento de puerto al nodo activo del clúster.

> [!IMPORTANT]
> La dirección IP flotante no se admite en una configuración de IP secundaria de NIC para los escenarios de equilibrio de carga. Para ver detalles, consulte [Limitaciones de Azure Load Balancer](../../../load-balancer/load-balancer-multivip-overview.md#limitations). Si necesita una dirección IP adicional para la VM, implemente una segunda NIC.  

![Figura 1: Configuración de clústeres de conmutación por error de Windows en Azure sin un disco compartido][sap-ha-guide-figure-1001]

_Configuración de clústeres de conmutación por error de Windows Server en Azure sin disco compartido_

### <a name="sap-ascsscs-ha-with-cluster-shared-disks"></a>Alta disponibilidad de ASCS/SCS de SAP con discos compartidos de clúster
En Windows, una instancia de ASCS/SCS de SAP contiene servicios centrales de SAP, el servidor de mensajes de SAP, procesos del servidor de colas y archivos de host global de SAP. Los archivos de host global de SAP almacenan archivos centrales para todo el sistema SAP.

Una instancia de ASCS/SCS de SAP tiene los siguientes componentes:

* Servicios centrales de SAP:
    * Dos procesos, un servidor de mensajes y colas y un \<ASCS/SCS virtual host name> que se usa para acceder a estos dos procesos.
    * Estructura de archivos: S:\usr\sap\\&lt;SID&gt;\ASCS/SCS\<instance number\>


* Archivos de host global de SAP :
  * Estructura de archivos: S:\usr\sap\\&lt;SID&gt;\SYS\..
  * El recurso compartido de archivos sapmnt, que permite el acceso a estos archivos S:\usr\sap\\&lt;SID&gt;\SYS\... globales utilizando la siguiente ruta de acceso UNC:

    \\\\<nombre de host virtual ASCS/SCS\>\sapmnt\\&lt;SID&gt;\SYS\..


![Ilustración 2: Procesos, estructura de archivos y recurso compartido de archivos sapmnt de host global de una instancia de ASCS/SCS de SAP][sap-ha-guide-figure-8001]

_Procesos, estructura de archivos y recurso compartido de archivos sapmnt de host global de una instancia de ASCS/SCS de SAP_

En una configuración de alta disponibilidad, se agrupan instancias de ASCS/SCS de SAP. Usamos *discos compartidos de clúster* (unidad S en nuestro ejemplo) para colocar los archivos de ASCS/SCS de SAP y los archivos de host global de SAP.

![Ilustración 3: Arquitectura de alta disponibilidad de ASCS/SCS de SAP con disco compartido][sap-ha-guide-figure-8002]

_Arquitectura de alta disponibilidad de ASCS/SCS de SAP con disco compartido_


Con arquitectura de Servidor de replicación de colas 1:
* Se usa el mismo \<ASCS/SCS virtual host name> para acceder a los procesos del servidor de mensajes y colas de SAP y los archivos de host globales de SAP mediante el recurso compartido de archivos sapmnt.
* Comparten la misma unidad de disco compartido de clúster S.  

Con arquitectura de Servidor de replicación de colas 2: 
* Se usa el mismo \<ASCS/SCS virtual host name> para acceder al proceso del servidor de mensajes de SAP y los archivos de host global de SAP mediante el recurso compartido de archivos sapmnt.
* Comparten la misma unidad de disco compartido de clúster S.
* Hay un \<ERS virtual host name> distinto para acceder al proceso del servidor de colas.  

![Ilustración 4: Arquitectura de alta disponibilidad de ASCS/SCS de SAP con disco compartido][sap-ha-guide-figure-8003]

_Arquitectura de alta disponibilidad de ASCS/SCS de SAP con disco compartido_

#### <a name="shared-disk-and-enqueue-replication-server"></a>Disco compartido y Servidor de replicación de colas 

1. El disco compartido es compatible con la arquitectura de Servidor de replicación de colas 1, donde la instancia de Servidor de replicación de colas (ERS):   

   - no está agrupada en clústeres
   - usa el nombre `localhost`
   - está implementada en discos locales en cada uno de los nodos del clúster

2. El disco compartido también es compatible con la arquitectura de Servidor de replicación de colas 2, donde la instancia de Servidor de replicación de colas 2 (ERS2):  

   - está agrupada en clústeres
   - usa un nombre de host virtual o de red dedicado
   - necesita la dirección IP del nombre de host virtual de ERS que se va a configurar en el equilibrador de carga interno de Azure, además de la dirección IP de (A)SCS
   - está implementada en **discos locales** en cada uno de los nodos del clúster y, por lo tanto, no hay necesidad de disco compartido

   > [!TIP]
   > Puede encontrar más información sobre Servidor de replicación de colas 1 y 2 (ERS1 y ERS2) aquí:  
   > [Servidor de replicación de colas en un clúster de conmutación por error de Microsoft](https://help.sap.com/viewer/3741bfe0345f4892ae190ee7cfc53d4c/CURRENT_VERSION_SWPM20/en-US/8abd4b52902d4b17a105c2fabdf5c0cf.html)  
   > [Nuevo replicador de colas en entornos de clúster de conmutación por error](https://blogs.sap.com/2019/03/19/new-enqueue-replicator-in-failover-cluster-environments/)  

#### <a name="options-for-shared-disk-in-azure-for-sap-workloads"></a>Opciones de disco compartido en Azure para cargas de trabajo de SAP

Hay dos opciones de disco compartido en un clúster de conmutación por error de Azure:

- [Discos compartidos de Azure](../../disks-shared.md): característica que permite asociar un disco administrado de Azure a varias máquinas virtuales de forma simultánea. 
- Uso del software de terceros [SIOS DataKeeper Cluster Edition](https://us.sios.com/products/datakeeper-cluster) para crear un almacenamiento reflejado que simule almacenamiento compartido de clúster. 

Al seleccionar la tecnología de disco compartido, tenga en cuenta las siguientes consideraciones:

**Disco compartido de Azure para cargas de trabajo de SAP**

- Permite asociar un disco administrado de Azure a varias máquinas virtuales simultáneamente sin necesidad de software adicional que mantener y operar.
- Un [disco compartido de Azure](../../disks-shared.md) con discos [SSD prémium](../../disks-types.md#premium-ssd) es compatible con la implementación de SAP en el conjunto y las zonas de disponibilidad.
- Un [disco Ultra de Azure](../../disks-types.md#ultra-disk) y los [discos estándar de Azure](../../disks-types.md#standard-ssd) no se admiten como discos compartidos de Azure para cargas de trabajo de SAP.
- Asegúrese de aprovisionar el disco prémium de Azure con un tamaño de disco mínimo según lo especificado en [Rangos de discos SSD prémium](../../disks-shared.md#disk-sizes) para poder asociarlo al número requerido de máquinas virtuales simultáneamente (normalmente dos por clúster de conmutación por error de Windows de ASCS de SAP).
 
**SIOS**
- La solución SIOS proporciona replicación de datos sincrónica en tiempo real entre dos discos.
- Con la solución SIOS, se opera con dos discos administrados y, si se usan conjuntos o zonas de disponibilidad, los discos administrados terminan en distintos clústeres de almacenamiento. 
- Se admite la implementación en zonas de disponibilidad.
- Requiere instalación y manipulación de software de terceros que se tiene que comprar por separado.

### <a name="shared-disk-using-azure-shared-disk"></a>Disco compartido con disco compartido de Azure

Microsoft ofrece [discos compartidos de Azure](../../disks-shared.md), que se pueden usar para implementar alta disponibilidad de ASCS/SCS de SAP con una opción de disco compartido.

#### <a name="prerequisites-and-limitations"></a>Requisitos previos y limitaciones

De momento, puede usar discos SSD Premium de Azure como un disco compartido de Azure para la instancia de ASCS/SCS de SAP. Actualmente están en vigor las siguientes limitaciones:

-  Un [disco Ultra de Azure](../../disks-types.md#ultra-disk) y los [discos SSD estándar](../../disks-types.md#standard-ssd) no se admiten como discos compartidos de Azure para cargas de trabajo de SAP.
-  Un [disco compartido de Azure](../../disks-shared.md) con [discos SSD prémium](../../disks-types.md#premium-ssd) es compatible con la implementación de SAP en el conjunto y las zonas de disponibilidad.
-  Un disco compartido de Azure con discos SSD prémium incluye dos SKU de almacenamiento.
   - El almacenamiento con redundancia local (LRS) para el disco compartido prémium (skuName: Premium_LRS) es compatible con la implementación en el conjunto de disponibilidad de Azure.
   - El almacenamiento con redundancia de zona (ZRS) para el disco compartido prémium (skuName: Premium_ZRS) es compatible con la implementación en las zonas de disponibilidad de Azure.
-  El valor de disco compartido de Azure [maxShares](../../disks-shared-enable.md?tabs=azure-cli#disk-sizes) determina cuántos nodos de clúster pueden usar el disco compartido. Normalmente, para la instancia de ASCS/SCS de SAP, se configuran dos nodos en el clúster de conmutación por error de Windows, por lo que el valor de `maxShares` debe establecerse en dos.
-  Al usar [grupos con ubicación por proximidad de Azure](../../windows/proximity-placement-groups.md) para el sistema SAP, todas las máquinas virtuales que comparten un disco deben formar parte del mismo PPG.

Para obtener más detalles sobre las limitaciones del disco compartido de Azure, consulte con detenimiento la sección [Limitaciones](../../disks-shared.md#limitations) de la documentación sobre discos compartidos de Azure.

#### <a name="important-consideration-for-premium-shared-disk"></a>Consideraciones importantes del disco compartido prémium

A continuación se incluyen algunos de los puntos importantes que se deben tener en cuenta para el disco compartido prémium de Azure:

- LRS para discos compartidos prémium
  - La implementación de SAP con LRS para discos compartidos prémium funcionará con un único disco compartido de Azure en un clúster de almacenamiento. La instancia de ASCS/SCS de SAP se vería afectada en caso de problemas con el clúster de almacenamiento donde se ha implementado el disco compartido de Azure.

- ZRS para discos compartidos prémium
  - La latencia de escritura de ZRS es mayor que la de LRS debido a la copia de datos entre zonas.
  - La distancia entre las zonas de disponibilidad en una región diferente varía y, con ella, también la latencia de discos ZRS entre zonas de disponibilidad. [Realice pruebas comparativas en los discos](../../disks-benchmarks.md) para identificar la latencia del disco ZRS en su región.
  - ZRS para discos compartidos prémium replica de forma sincrónica los datos en tres zonas de disponibilidad de la región. En caso de que se produzca alguna incidencia en uno de los clústeres de almacenamiento, ASCS/SCS de SAP seguirá funcionando, ya que la conmutación por error del almacenamiento es transparente en el nivel de aplicación.
  - Revise la sección de [limitaciones](../../disks-redundancy.md#limitations) de ZRS para discos administrados a fin de obtener más detalles.

> [!TIP]
> Vea la [Guía de planeación de SAP Netweaver en Azure](./planning-guide.md) y la [Guía de Azure Storage para cargas de trabajo de SAP](./planning-guide-storage.md) para conocer importantes consideraciones a la hora de planear la implementación de SAP.

### <a name="supported-os-versions"></a>Versiones de SO admitidas

Se admiten servidores Windows 2016 y 2019 (use las imágenes más recientes del centro de datos).

Se recomienda encarecidamente usar **Windows Server 2019 Datacenter**, ya que:
- El servicio de clúster de conmutación por error de Windows 2019 reconoce a Azure.
- Se ha agregado la integración y el reconocimiento del mantenimiento del host de Azure y se ha mejorado la experiencia mediante la supervisión de eventos de programación de Azure.
- Es posible usar el nombre de red distribuida (es la opción predeterminada). Por lo tanto, no es necesario tener una dirección IP dedicada para el nombre de red del clúster. Tampoco existe la necesidad de configurar esta dirección IP en el equilibrador de carga interno de Azure. 

### <a name="shared-disks-in-azure-with-sios-datakeeper"></a>Discos compartidos en Azure con SIOS DataKeeper

Otra opción de disco compartido es usar el software de terceros SIOS DataKeeper Cluster Edition para crear un almacenamiento reflejado que simule el almacenamiento compartido de clúster. La solución SIOS proporciona replicación sincrónica de datos en tiempo real.

Para crear un recurso de disco compartido para un clúster:

1. Conecte un disco adicional a cada una de las máquinas virtuales de una configuración de clúster de Windows.
2. Ejecute SIOS DataKeeper Cluster Edition en ambos nodos de la máquina virtual.
3. Configure SIOS DataKeeper Cluster Edition de forma que refleje el contenido del volumen del disco adicional asociado desde la máquina virtual de origen al volumen del disco adicional asociado de la máquina virtual de destino. SIOS DataKeeper abstrae los volúmenes locales de origen y de destino y los presenta a los clústeres de conmutación por error de Windows Server como un disco compartido.

Obtenga más información sobre [SIOS DataKeeper](https://us.sios.com/products/datakeeper-cluster/).

![Ilustración 5: Configuración de clústeres de conmutación por error de Windows Server en Azure con SIOS DataKeeper][sap-ha-guide-figure-1002]

_Configuración de clústeres de conmutación por error de Windows en Azure con SIOS DataKeeper_

> [!NOTE]
> No necesita discos compartidos para obtener alta disponibilidad con algunos productos de DBMS, como SQL Server. SQL Server Always On replica archivos de registro y datos de DBMS desde el disco local de un nodo del clúster hasta el disco local de otro nodo del clúster. En este caso, la configuración de clúster de Windows no necesita un disco compartido.
>
## <a name="optional-configurations"></a>Configuraciones opcionales

En los diagramas siguientes se muestran varias instancias de SAP en máquinas virtuales de Azure que ejecutan el clúster de conmutación por error de Microsoft Windows para reducir el número total de máquinas virtuales.

Se puede tratar de servidores de aplicaciones SAP locales en un clúster de ASCS/SCS de SAP o un rol de clúster de ASCS/SCS de SAP en nodos Always On de Microsoft SQL Server.

> [!IMPORTANT]
> No se admite la instalación de un servidor de aplicaciones de SAP local en un nodo Always On de SQL Server.
>

Tanto ASCS/SCS de SAP como la base de datos de Microsoft SQL Server, son puntos de error únicos (SPOF). Para proteger estos SPOF en un entorno Windows se usa WSFC.

Aunque el consumo de recursos de ASCS/SCS de SAP es bastante pequeño, se recomienda una reducción de la configuración de memoria de 2 GB para SQL Server o para el servidor de aplicaciones SAP.

### <a name="sap-application-servers-on-wsfc-nodes-using-sios-datakeeper"></a>Servidores de aplicaciones SAP en nodos WSFC mediante SIOS DataKeeper

![Figura 6: configuración de clústeres de conmutación por error de Windows Server en Azure con SIOS DataKeeper y el servidor de aplicaciones SAP instalado localmente][sap-ha-guide-figure-1003]

> [!NOTE]
> Puesto que los servidores de aplicaciones SAP se instalan localmente, no es necesario configurar ninguna sincronización como se muestra en la imagen.
>
### <a name="sap-ascsscs-on-sql-server-always-on-nodes-using-sios-datakeeper"></a>ASCS/SCS de SAP en nodos Always On de SQL Server con SIOS DataKeeper

![Figura 7: ASCS/SCS de SAP en nodos Always On de SQL Server con SIOS DataKeeper][sap-ha-guide-figure-1005]

[Configuración opcional para servidores de aplicaciones SAP en nodos WSFC mediante Windows SOFS][optional-fileshare]

[Configuración opcional para servidores de aplicaciones SAP en nodos WSFC mediante SMB de NetApp Files][optional-smb]

[Configuración opcional para ASCS/SCS de SAP en nodos Always On de SQL Server mediante Windows SOFS][optional-fileshare-sql]

[Configuración opcional para ASCS/SCS de SAP en nodos Always On de SQL Server mediante SMB de NetApp Files][optional-smb-sql]

## <a name="next-steps"></a>Pasos siguientes

* [Preparación de la infraestructura de Azure para alta disponibilidad de SAP con un clúster de conmutación por error de Windows y un disco compartido para una instancia de ASCS/SCS de SAP][sap-high-availability-infrastructure-wsfc-shared-disk]

* [Instalación de alta disponibilidad para SAP NetWeaver en un clúster de conmutación por error de Windows y un disco compartido para una instancia de ASCS/SCS de SAP][sap-high-availability-installation-wsfc-shared-disk]


[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2243692]:https://launchpad.support.sap.com/#/notes/2243692

[sap-installation-guides]:http://service.sap.com/instguides

[azure-resource-manager/management/azure-subscription-service-limits]:../../../azure-resource-manager/management/azure-subscription-service-limits.md
[azure-resource-manager/management/azure-subscription-service-limits-subscription]:../../../azure-resource-manager/management/azure-subscription-service-limits.md

[dbms-guide]:../../virtual-machines-windows-sap-dbms-guide.md

[deployment-guide]:deployment-guide.md

[dr-guide-classic]:https://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:get-started.md
[ha-guide]:sap-high-availability-guide.md
[sap-high-availability-architecture-scenarios]:sap-high-availability-architecture-scenarios.md
[sap-high-availability-infrastructure-wsfc-shared-disk]:sap-high-availability-infrastructure-wsfc-shared-disk.md
[sap-high-availability-installation-wsfc-shared-disk]:sap-high-availability-installation-wsfc-shared-disk.md

[planning-guide]:planning-guide.md
[planning-guide-11]:planning-guide.md
[planning-guide-2.1]:planning-guide.md#1625df66-4cc6-4d60-9202-de8a0b77f803
[planning-guide-2.2]:planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10

[planning-guide-microsoft-azure-networking]:planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f

[sap-ha-guide]:sap-high-availability-guide.md
[sap-ha-guide-2]:#42b8f600-7ba3-4606-b8a5-53c4f026da08
[sap-ha-guide-4]:#8ecf3ba0-67c0-4495-9c14-feec1a2255b7
[sap-ha-guide-8]:#78092dbe-165b-454c-92f5-4972bdbef9bf
[sap-ha-guide-8.1]:#c87a8d3f-b1dc-4d2f-b23c-da4b72977489
[sap-ha-guide-8.9]:#fe0bd8b5-2b43-45e3-8295-80bee5415716
[sap-ha-guide-8.11]:#661035b2-4d0f-4d31-86f8-dc0a50d78158
[sap-ha-guide-8.12]:#0d67f090-7928-43e0-8772-5ccbf8f59aab
[sap-ha-guide-8.12.1]:#5eecb071-c703-4ccc-ba6d-fe9c6ded9d79
[sap-ha-guide-8.12.3]:#5c8e5482-841e-45e1-a89d-a05c0907c868
[sap-ha-guide-8.12.3.1]:#1c2788c3-3648-4e82-9e0d-e058e475e2a3
[sap-ha-guide-8.12.3.2]:#dd41d5a2-8083-415b-9878-839652812102
[sap-ha-guide-8.12.3.3]:#d9c1fc8e-8710-4dff-bec2-1f535db7b006
[sap-ha-guide-9]:#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:#31c6bd4f-51df-4057-9fdf-3fcbc619c170
[sap-ha-guide-9.1.1]:#a97ad604-9094-44fe-a364-f89cb39bf097

[sap-ha-multi-sid-guide]:sap-high-availability-multi-sid.md (Configuración de alta disponibilidad de varios SID de SAP)

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png

[sap-ha-guide-figure-1000]:./media/virtual-machines-shared-sap-high-availability-guide/1000-wsfc-for-sap-ascs-on-azure.png
[sap-ha-guide-figure-1001]:./media/virtual-machines-shared-sap-high-availability-guide/1001-wsfc-on-azure-ilb.png
[sap-ha-guide-figure-1003]:./media/virtual-machines-shared-sap-high-availability-guide/ha-sios-as.png
[sap-ha-guide-figure-1005]:./media/virtual-machines-shared-sap-high-availability-guide/ha-sql-ascs-sios.png
[sap-ha-guide-figure-1002]:./media/virtual-machines-shared-sap-high-availability-guide/ha-sios.png
[sap-ha-guide-figure-2000]:./media/virtual-machines-shared-sap-high-availability-guide/2000-wsfc-sap-as-ha-on-azure.png
[sap-ha-guide-figure-2001]:./media/virtual-machines-shared-sap-high-availability-guide/2001-wsfc-sap-ascs-ha-on-azure.png
[sap-ha-guide-figure-2003]:./media/virtual-machines-shared-sap-high-availability-guide/2003-wsfc-sap-dbms-ha-on-azure.png
[sap-ha-guide-figure-2004]:./media/virtual-machines-shared-sap-high-availability-guide/2004-wsfc-sap-ha-e2e-archit-template1-on-azure.png
[sap-ha-guide-figure-2005]:./media/virtual-machines-shared-sap-high-availability-guide/2005-wsfc-sap-ha-e2e-arch-template2-on-azure.png

[sap-ha-guide-figure-3000]:./media/virtual-machines-shared-sap-high-availability-guide/3000-template-parameters-sap-ha-arm-on-azure.png
[sap-ha-guide-figure-3001]:./media/virtual-machines-shared-sap-high-availability-guide/3001-configuring-dns-servers-for-Azure-vnet.png
[sap-ha-guide-figure-3002]:./media/virtual-machines-shared-sap-high-availability-guide/3002-configuring-static-IP-address-for-network-card-of-each-vm.png
[sap-ha-guide-figure-3003]:./media/virtual-machines-shared-sap-high-availability-guide/3003-setup-static-ip-address-ilb-for-ascs-instance.png
[sap-ha-guide-figure-3004]:./media/virtual-machines-shared-sap-high-availability-guide/3004-default-ascs-scs-ilb-balancing-rules-for-azure-ilb.png
[sap-ha-guide-figure-3005]:./media/virtual-machines-shared-sap-high-availability-guide/3005-changing-ascs-scs-default-ilb-rules-for-azure-ilb.png
[sap-ha-guide-figure-3006]:./media/virtual-machines-shared-sap-high-availability-guide/3006-adding-vm-to-domain.png
[sap-ha-guide-figure-3007]:./media/virtual-machines-shared-sap-high-availability-guide/3007-config-wsfc-1.png
[sap-ha-guide-figure-3008]:./media/virtual-machines-shared-sap-high-availability-guide/3008-config-wsfc-2.png
[sap-ha-guide-figure-3009]:./media/virtual-machines-shared-sap-high-availability-guide/3009-config-wsfc-3.png
[sap-ha-guide-figure-3010]:./media/virtual-machines-shared-sap-high-availability-guide/3010-config-wsfc-4.png
[sap-ha-guide-figure-3011]:./media/virtual-machines-shared-sap-high-availability-guide/3011-config-wsfc-5.png
[sap-ha-guide-figure-3012]:./media/virtual-machines-shared-sap-high-availability-guide/3012-config-wsfc-6.png
[sap-ha-guide-figure-3013]:./media/virtual-machines-shared-sap-high-availability-guide/3013-config-wsfc-7.png
[sap-ha-guide-figure-3014]:./media/virtual-machines-shared-sap-high-availability-guide/3014-config-wsfc-8.png
[sap-ha-guide-figure-3015]:./media/virtual-machines-shared-sap-high-availability-guide/3015-config-wsfc-9.png
[sap-ha-guide-figure-3016]:./media/virtual-machines-shared-sap-high-availability-guide/3016-config-wsfc-10.png
[sap-ha-guide-figure-3017]:./media/virtual-machines-shared-sap-high-availability-guide/3017-config-wsfc-11.png
[sap-ha-guide-figure-3018]:./media/virtual-machines-shared-sap-high-availability-guide/3018-config-wsfc-12.png
[sap-ha-guide-figure-3019]:./media/virtual-machines-shared-sap-high-availability-guide/3019-assign-permissions-on-share-for-cluster-name-object.png
[sap-ha-guide-figure-3020]:./media/virtual-machines-shared-sap-high-availability-guide/3020-change-object-type-include-computer-objects.png
[sap-ha-guide-figure-3021]:./media/virtual-machines-shared-sap-high-availability-guide/3021-check-box-for-computer-objects.png
[sap-ha-guide-figure-3022]:./media/virtual-machines-shared-sap-high-availability-guide/3022-set-security-attributes-for-cluster-name-object-on-file-share-quorum.png
[sap-ha-guide-figure-3023]:./media/virtual-machines-shared-sap-high-availability-guide/3023-call-configure-cluster-quorum-setting-wizard.png
[sap-ha-guide-figure-3024]:./media/virtual-machines-shared-sap-high-availability-guide/3024-selection-screen-different-quorum-configurations.png
[sap-ha-guide-figure-3025]:./media/virtual-machines-shared-sap-high-availability-guide/3025-selection-screen-file-share-witness.png
[sap-ha-guide-figure-3026]:./media/virtual-machines-shared-sap-high-availability-guide/3026-define-file-share-location-for-witness-share.png
[sap-ha-guide-figure-3027]:./media/virtual-machines-shared-sap-high-availability-guide/3027-successful-reconfiguration-cluster-file-share-witness.png
[sap-ha-guide-figure-3028]:./media/virtual-machines-shared-sap-high-availability-guide/3028-install-dot-net-framework-35.png
[sap-ha-guide-figure-3029]:./media/virtual-machines-shared-sap-high-availability-guide/3029-install-dot-net-framework-35-progress.png
[sap-ha-guide-figure-3030]:./media/virtual-machines-shared-sap-high-availability-guide/3030-sios-installer.png
[sap-ha-guide-figure-3031]:./media/virtual-machines-shared-sap-high-availability-guide/3031-first-screen-sios-data-keeper-installation.png
[sap-ha-guide-figure-3032]:./media/virtual-machines-shared-sap-high-availability-guide/3032-data-keeper-informs-service-be-disabled.png
[sap-ha-guide-figure-3033]:./media/virtual-machines-shared-sap-high-availability-guide/3033-user-selection-sios-data-keeper.png
[sap-ha-guide-figure-3034]:./media/virtual-machines-shared-sap-high-availability-guide/3034-domain-user-sios-data-keeper.png
[sap-ha-guide-figure-3035]:./media/virtual-machines-shared-sap-high-availability-guide/3035-provide-sios-data-keeper-license.png
[sap-ha-guide-figure-3036]:./media/virtual-machines-shared-sap-high-availability-guide/3036-data-keeper-management-config-tool.png
[sap-ha-guide-figure-3037]:./media/virtual-machines-shared-sap-high-availability-guide/3037-tcp-ip-address-first-node-data-keeper.png
[sap-ha-guide-figure-3038]:./media/virtual-machines-shared-sap-high-availability-guide/3038-create-replication-sios-job.png
[sap-ha-guide-figure-3039]:./media/virtual-machines-shared-sap-high-availability-guide/3039-define-sios-replication-job-name.png
[sap-ha-guide-figure-3040]:./media/virtual-machines-shared-sap-high-availability-guide/3040-define-sios-source-node.png
[sap-ha-guide-figure-3041]:./media/virtual-machines-shared-sap-high-availability-guide/3041-define-sios-target-node.png
[sap-ha-guide-figure-3042]:./media/virtual-machines-shared-sap-high-availability-guide/3042-define-sios-synchronous-replication.png
[sap-ha-guide-figure-3043]:./media/virtual-machines-shared-sap-high-availability-guide/3043-enable-sios-replicated-volume-as-cluster-volume.png
[sap-ha-guide-figure-3044]:./media/virtual-machines-shared-sap-high-availability-guide/3044-data-keeper-synchronous-mirroring-for-SAP-gui.png
[sap-ha-guide-figure-3045]:./media/virtual-machines-shared-sap-high-availability-guide/3045-replicated-disk-by-data-keeper-in-wsfc.png
[sap-ha-guide-figure-3046]:./media/virtual-machines-shared-sap-high-availability-guide/3046-dns-entry-sap-ascs-virtual-name-ip.png
[sap-ha-guide-figure-3047]:./media/virtual-machines-shared-sap-high-availability-guide/3047-dns-manager.png
[sap-ha-guide-figure-3048]:./media/virtual-machines-shared-sap-high-availability-guide/3048-default-cluster-probe-port.png
[sap-ha-guide-figure-3049]:./media/virtual-machines-shared-sap-high-availability-guide/3049-cluster-probe-port-after.png
[sap-ha-guide-figure-3050]:./media/virtual-machines-shared-sap-high-availability-guide/3050-service-type-ers-delayed-automatic.png
[sap-ha-guide-figure-5000]:./media/virtual-machines-shared-sap-high-availability-guide/5000-wsfc-sap-sid-node-a.png
[sap-ha-guide-figure-5001]:./media/virtual-machines-shared-sap-high-availability-guide/5001-sios-replicating-local-volume.png
[sap-ha-guide-figure-5002]:./media/virtual-machines-shared-sap-high-availability-guide/5002-wsfc-sap-sid-node-b.png
[sap-ha-guide-figure-5003]:./media/virtual-machines-shared-sap-high-availability-guide/5003-sios-replicating-local-volume-b-to-a.png

[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png


[sap-ha-guide-figure-8001]:./media/virtual-machines-shared-sap-high-availability-guide/8001.png
[sap-ha-guide-figure-8002]:./media/virtual-machines-shared-sap-high-availability-guide/8002.png
[sap-ha-guide-figure-8003]:./media/virtual-machines-shared-sap-high-availability-guide/8003.png
[sap-ha-guide-figure-8004]:./media/virtual-machines-shared-sap-high-availability-guide/8004.png
[sap-ha-guide-figure-8005]:./media/virtual-machines-shared-sap-high-availability-guide/8005.png
[sap-ha-guide-figure-8006]:./media/virtual-machines-shared-sap-high-availability-guide/8006.png
[sap-ha-guide-figure-8007]:./media/virtual-machines-shared-sap-high-availability-guide/8007.png
[sap-ha-guide-figure-8008]:./media/virtual-machines-shared-sap-high-availability-guide/8008.png
[sap-ha-guide-figure-8009]:./media/virtual-machines-shared-sap-high-availability-guide/8009.png
[sap-ha-guide-figure-8010]:./media/virtual-machines-shared-sap-high-availability-guide/8010.png
[sap-ha-guide-figure-8011]:./media/virtual-machines-shared-sap-high-availability-guide/8011.png
[sap-ha-guide-figure-8012]:./media/virtual-machines-shared-sap-high-availability-guide/8012.png
[sap-ha-guide-figure-8013]:./media/virtual-machines-shared-sap-high-availability-guide/8013.png
[sap-ha-guide-figure-8014]:./media/virtual-machines-shared-sap-high-availability-guide/8014.png
[sap-ha-guide-figure-8015]:./media/virtual-machines-shared-sap-high-availability-guide/8015.png
[sap-ha-guide-figure-8016]:./media/virtual-machines-shared-sap-high-availability-guide/8016.png
[sap-ha-guide-figure-8017]:./media/virtual-machines-shared-sap-high-availability-guide/8017.png
[sap-ha-guide-figure-8018]:./media/virtual-machines-shared-sap-high-availability-guide/8018.png
[sap-ha-guide-figure-8019]:./media/virtual-machines-shared-sap-high-availability-guide/8019.png
[sap-ha-guide-figure-8020]:./media/virtual-machines-shared-sap-high-availability-guide/8020.png
[sap-ha-guide-figure-8021]:./media/virtual-machines-shared-sap-high-availability-guide/8021.png
[sap-ha-guide-figure-8022]:./media/virtual-machines-shared-sap-high-availability-guide/8022.png
[sap-ha-guide-figure-8023]:./media/virtual-machines-shared-sap-high-availability-guide/8023.png
[sap-ha-guide-figure-8024]:./media/virtual-machines-shared-sap-high-availability-guide/8024.png
[sap-ha-guide-figure-8025]:./media/virtual-machines-shared-sap-high-availability-guide/8025.png


[sap-templates-3-tier-multisid-xscs-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs%2Fazuredeploy.json
[sap-templates-3-tier-multisid-xscs-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fapplication-workloads%2Fsap%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[sap-templates-3-tier-multisid-db-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fapplication-workloads%2Fsap%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-apps%2Fazuredeploy.json
[sap-templates-3-tier-multisid-apps-marketplace-image-md]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fapplication-workloads%2Fsap%2Fsap-3-tier-marketplace-image-multi-sid-apps-md%2Fazuredeploy.json

[virtual-machines-azure-resource-manager-architecture-benefits-arm]:../../../azure-resource-manager/management/overview.md#the-benefits-of-using-resource-manager

[virtual-machines-manage-availability]:../../virtual-machines-windows-manage-availability.md
[optional-smb]:high-availability-guide-windows-netapp-files-smb.md#5121771a-7618-4f36-ae14-ccf9ee5f2031 (Configuración opcional para servidores de aplicaciones SAP en nodos WSFC mediante SMB de NetApp Files)
[optional-fileshare]:sap-high-availability-guide-wsfc-file-share.md#86cb3ee0-2091-4b74-be77-64c2e6424f50 (Configuración opcional para servidores de aplicaciones SAP en nodos WSFC mediante Windows SOFS)
[optional-smb-sql]:high-availability-guide-windows-netapp-files-smb.md#01541cf2-0a03-48e3-971e-e03575fa7b4f (Configuración opcional para ASCS/SCS de SAP en nodos Always On de SQL Server mediante SMB de NetApp Files)
[optional-fileshare-sql]:sap-high-availability-guide-wsfc-file-share.md#db335e0d-09b4-416b-b240-afa18505f503 (Configuración opcional para ASCS/SCS de SAP en nodos Always On de SQL Server mediante Windows SOFS)