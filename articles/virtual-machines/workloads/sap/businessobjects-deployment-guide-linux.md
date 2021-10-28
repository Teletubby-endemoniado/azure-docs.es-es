---
title: Implementación de la plataforma de inteligencia empresarial SAP BusinessObjects en Azure para Linux | Microsoft Docs
description: Implementación y configuración de la plataforma de inteligencia empresarial SAP BusinessObjects en Azure para Linux
services: virtual-machines-windows,virtual-network,storage,azure-netapp-files,azure-files,mysql
documentationcenter: saponazure
author: dennispadia
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/05/2020
ms.author: depadia
ms.openlocfilehash: e3cc7420a976812d462b3f5c5e60878ba67641ff
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130249160"
---
# <a name="sap-businessobjects-bi-platform-deployment-guide-for-linux-on-azure"></a>Guía de implementación de la plataforma de inteligencia empresarial SAP BusinessObjects para Linux en Azure

En este artículo se describe la estrategia para implementar la plataforma de inteligencia empresarial SAP BusinessObjects (BOBI) en Azure para Linux. En este ejemplo, configurará dos máquinas virtuales con discos administrados de unidad de estado sólido (SSD) prémium como directorio de instalación. Puede usar Azure Database for MySQL para la base de datos CMS y compartir Azure NetApp Files para el servidor de repositorio de archivos entre ambos servidores. En ambas máquinas virtuales, la aplicación web de Java de Tomcat predeterminada y la plataforma de inteligencia empresarial se instalan juntas. Para equilibrar la carga de las solicitudes de los usuarios, se usa Azure Application Gateway con capacidades de descarga nativas de TLS/SSL.

Este tipo de arquitectura es eficaz para implementaciones pequeñas o en entornos que no son de producción. Para implementaciones grandes o en entornos de producción, puede tener hosts independientes para la aplicación web. También puede tener varios hosts de aplicación BOBI, lo que permitirá al servidor procesar más información.

![Diagrama de la implementación de SAP BOBI en Azure para Linux](media/businessobjects-deployment-guide/businessobjects-deployment-linux.png)

Esta es la versión del producto y el diseño del sistema de archivos para este ejemplo:

- Plataforma de SAP BusinessObjects 4.3
- SUSE Linux Enterprise Server 12 SP5
- Azure Database for MySQL (Versión: 8.0.15)
- Conector de MySQL C API: libmysqlclient (Versión: 6.1.11)

| Sistema de archivos        | Descripción                                                                                                               | Tamaño (GB)             | Propietario  | Grupo  | Storage                    |
|--------------------|---------------------------------------------------------------------------------------------------------------------------|-----------------------|--------|--------|----------------------------|
| /usr/sap           | El sistema de archivos para la instalación de la instancia de SAP BOBI, la aplicación web de Tomcat predeterminada y los controladores de base de datos (según proceda). | Directrices de dimensionamiento de SAP | bl1adm | sapsys | Disco administrado prémium: SSD |
| /usr/sap/frsinput  | El directorio de montaje es para los archivos compartidos en todos los hosts de BOBI que se usarán como directorio de repositorios de archivos de entrada.  | Necesidad empresarial         | bl1adm | sapsys | Azure NetApp Files         |
| /usr/sap/frsoutput | El directorio de montaje es para los archivos compartidos entre todos los hosts de BOBI que se usarán como directorio de repositorios de archivos de salida. | Necesidad empresarial         | bl1adm | sapsys | Azure NetApp Files         |

## <a name="deploy-linux-virtual-machine-via-azure-portal"></a>Implementación de máquinas virtuales Linux en Azure Portal

En esta sección, se crean dos máquinas virtuales con la imagen del sistema operativo Linux para la plataforma SAP BOBI. A continuación, se indican los pasos de alto nivel para crear máquinas virtuales:

1. Cree un [grupo de recursos](../../../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups).

2. Cree una [red virtual](../../../virtual-network/quick-create-portal.md#create-a-virtual-network).

   - No use una única subred para todos los servicios de Azure en la implementación de la plataforma de inteligencia artificial SAP. En función de la arquitectura de la plataforma de inteligencia empresarial SAP, debe crear varias subredes. En esta implementación, se crean tres subredes: una para la aplicación, el almacén del repositorio de archivos y Application Gateway.
   - En Azure, Application Gateway y Azure NetApp Files deben estar siempre en una subred independiente. Para más información, consulte [Azure Application Gateway](../../../application-gateway/configuration-overview.md) y [Directrices para el planeamiento de red de Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-network-topologies.md).

3. Cree un conjunto de disponibilidad. Para lograr redundancia para cada nivel en la implementación de varias instancias, coloque las máquinas virtuales de cada nivel en un conjunto de disponibilidad. Asegúrese de separar los conjuntos de disponibilidad para cada nivel en función de su arquitectura.

4. Cree la máquina virtual 1, denominada **(azusbosl1)** .

   - Puede usar una imagen personalizada o elegir una imagen de Azure Marketplace. Para más información, consulte [Implementación de una máquina virtual desde Azure Marketplace para SAP](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/virtual-machines/workloads/sap/deployment-guide.md#scenario-1-deploying-a-vm-from-the-azure-marketplace-for-sap) o [Implementación de una máquina virtual con una imagen personalizada para SAP](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/virtual-machines/workloads/sap/deployment-guide.md#scenario-2-deploying-a-vm-with-a-custom-image-for-sap).

5. Cree la máquina virtual 2, denominada **(azusbosl2)** .
6. Agregue un disco SSD prémium. Lo usará como directorio de instalación de SAP BOBI.

## <a name="provision-azure-netapp-files"></a>Aprovisionamiento de Azure NetApp Files

Antes de continuar con la configuración de Azure NetApp Files, familiarícese con la [documentación de Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-introduction.md).

Azure NetApp Files está disponible en varias [regiones de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=netapp). Compruebe si la región de Azure seleccionada ofrece Azure NetApp Files.

Para comprobar la disponibilidad de Azure NetApp Files por región, consulte [Disponibilidad de Azure NetApp Files por región de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all).

### <a name="deploy-azure-netapp-files-resources"></a>Implementación de recursos de Azure NetApp Files

En las siguientes instrucciones se supone que ya ha implementado la [red virtual de Azure](../../../virtual-network/virtual-networks-overview.md). Los recursos de Azure NetApp Files y las máquinas virtuales en las que esos recursos se montarán se deben implementar en la misma red virtual de Azure o en redes virtuales emparejadas.

1. [Cree una cuenta de Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-create-netapp-account.md) en la región de Azure seleccionada.

2. [Configure un grupo de capacidad de Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-set-up-capacity-pool.md). La arquitectura de la plataforma de inteligencia empresarial SAP que se presenta en este artículo utiliza un único grupo de capacidad de Azure NetApp Files, el nivel de servicio Prémium. En el caso del servidor de repositorio de archivos de inteligencia empresarial SAP en Azure, se recomienda usar un [nivel de servicio](../../../azure-netapp-files/azure-netapp-files-service-levels.md) Prémium o Ultra de Azure NetApp Files.

3. [Delegación de una subred en Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-delegate-subnet.md).

5. Implemente los volúmenes de Azure NetApp Files según las instrucciones de [Creación de un volumen de NFS para Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-create-volumes.md).

   Puede implementar los volúmenes como NFSv3 y NFSv4.1, ya que ambos protocolos son compatibles con la plataforma SAP BOBI. Implemente los volúmenes en las subredes de Azure NetApp Files correspondientes. Las direcciones IP de los volúmenes de Azure NetApp Files se asignan automáticamente.

Tenga en cuenta que los recursos de Azure NetApp Files y las VM de Azure deben estar en la misma red virtual de Azure o en redes virtuales de Azure emparejadas. Por ejemplo, *azusbobi-frsinput* y *azusbobi-frsoutput* son los nombres de los volúmenes y *nfs://10.31.2.4/azusbobi-frsinput* y *nfs://10.31.2.4/azusbobi-frsoutput* son las rutas de acceso de archivo para los volúmenes de Azure NetApp Files.

- Volumen azusbobi-frsinput (nfs://10.31.2.4/azusbobi-frsinput) azusbobi-frsinput (nfs://10.31.2.4/azusbobi-frsinput)
- Volumen azusbobi-frsoutput (nfs://10.31.2.4/azusbobi-frsoutput)

### <a name="important-considerations"></a>Consideraciones importantes

A medida que se crea la instancia de Azure NetApp Files para el servidor de repositorio de archivos de la plataforma SAP BOBI, tenga en cuenta lo siguiente:

- El grupo de capacidad mínima es 4 tebibytes (TiB).
- El tamaño del volumen mínimo es de 100 gigabytes (GiB).
- Azure NetApp Files y todas las máquinas virtuales en las que los volúmenes de Azure NetApp Files se montarán se deben implementar en la misma red virtual de Azure o en [redes virtuales emparejadas](../../../virtual-network/virtual-network-peering-overview.md) de la misma región. Se admite el acceso de Azure NetApp Files mediante emparejamiento de red virtual en la misma región. Actualmente no se admite el acceso de Azure NetApp Files a través del emparejamiento global.
- La red virtual seleccionada debe tener una subred delegada en Azure NetApp Files.
- Con la [directiva de exportación](../../../azure-netapp-files/azure-netapp-files-configure-export-policy.md) de Azure NetApp Files, puede controlar los clientes permitidos y el tipo de acceso (por ejemplo, lectura y escritura o solo lectura).
- La característica Azure NetApp Files no depende aún de la zona. En la actualidad, la característica no se implementa en todas las zonas de disponibilidad de una región de Azure. Tenga en cuenta las posibles implicaciones de latencia en algunas regiones de Azure.
- Los volúmenes de Azure NetApp Files se pueden implementar como volúmenes NFSv3 o NFSv4.1. Ambos protocolos son compatibles con las aplicaciones de la plataforma de inteligencia empresarial SAP.

## <a name="configure-file-systems-on-linux-servers"></a>Configuración de sistemas de archivos en servidores Linux

En los pasos de esta sección se usan los siguientes prefijos:

**[A]** : El paso se aplica a todos los hosts.

### <a name="format-and-mount-the-sap-file-system"></a>Formato y montaje del sistema de archivos de SAP

1. **[A]** Enumerar todos los discos asociados.

    ```bash
    sudo lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda      8:0    0   30G  0 disk
    ├─sda1   8:1    0    2M  0 part
    ├─sda2   8:2    0  512M  0 part /boot/efi
    ├─sda3   8:3    0    1G  0 part /boot
    └─sda4   8:4    0 28.5G  0 part /
    sdb      8:16   0   32G  0 disk
    └─sdb1   8:17   0   32G  0 part /mnt
    sdc      8:32   0  128G  0 disk
    sr0     11:0    1  628K  0 rom  
    # Premium SSD of 128 GB is attached to virtual machine, whose device name is sdc
    ```

2. **[A]** Formatear el dispositivo de bloque para /usr/sap.

    ```bash
    sudo mkfs.xfs /dev/sdc
    ```

3. **[A]** Crear el directorio de montaje.

    ```bash
    sudo mkdir -p /usr/sap
    ```

4. **[A]** Obtener el UUID del dispositivo de bloque.

    ```bash
    sudo blkid

    # It will display information about block device. Copy UUID of the formatted block device

    /dev/sdc: UUID="0eb5f6f8-fa77-42a6-b22d-7a9472b4dd1b" TYPE="xfs"
    ```

5. **[A]** Mantener la entrada de montaje del sistema de archivos en /etc/fstab.

    ```bash
    sudo echo "UUID=0eb5f6f8-fa77-42a6-b22d-7a9472b4dd1b /usr/sap xfs defaults,nofail 0 2" >> /etc/fstab
    ```

6. **[A]** Montar el sistema de archivos.

    ```bash
    sudo mount -a

    sudo df -h

    Filesystem                     Size  Used Avail Use% Mounted on
    devtmpfs                       7.9G  8.0K  7.9G   1% /dev
    tmpfs                          7.9G   82M  7.8G   2% /run
    tmpfs                          7.9G     0  7.9G   0% /sys/fs/cgroup
    /dev/sda4                       29G  1.8G   27G   6% /
    tmpfs                          1.6G     0  1.6G   0% /run/user/1000
    /dev/sda3                     1014M   87M  928M   9% /boot
    /dev/sda2                      512M  1.1M  511M   1% /boot/efi
    /dev/sdb1                       32G   49M   30G   1% /mnt
    /dev/sdc                       128G   29G  100G  23% /usr/sap
    ```

### <a name="mount-the-azure-netapp-files-volume"></a>Montaje de los volúmenes de Azure NetApp Files

1. **[A]** Crear los directorios de montaje.

   ```bash
   sudo mkdir -p /usr/sap/frsinput
   sudo mkdir -p /usr/sap/frsoutput
   ```

2. **[A]** Configurar el sistema operativo cliente para que admita el montaje de NFSv4.1 (solo es aplicable si se usa NFSv4.1).

   Si usa volúmenes de Azure NetApp Files con el protocolo NFSv4.1, ejecute la siguiente configuración en todas las máquinas virtuales, donde se deben montar volúmenes de Azure NetApp Files NFSv4.1.

   En este paso, debe comprobar la configuración del dominio NFS. Asegúrese de que el dominio esté configurado como dominio predeterminado de Azure NetApp Files (`defaultv4iddomain.com`), y que la asignación se haya establecido en `nobody`.

   ```bash
   sudo cat /etc/idmapd.conf
   # Example
   [General]
   Domain = defaultv4iddomain.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```

   > [!Important]
   > Asegúrese de establecer el dominio NFS en /etc/idmapd.conf en la máquina virtual para que coincida con la configuración de dominio predeterminada en Azure NetApp Files (`defaultv4iddomain.com`). Si hay un error de coincidencia, los permisos para los archivos en volúmenes de Azure NetApp Files montados en las máquinas virtuales se mostrarán como `nobody`.

   Comprobar `nfs4_disable_idmapping` Se debe establecer en `Y`. Para crear la estructura de directorio en la que se encuentra `nfs4_disable_idmapping`, ejecute el comando mount. No podrá crear manualmente el directorio en /sys/modules, ya que el acceso está reservado para el kernel o los controladores.

   ```bash
   # Check nfs4_disable_idmapping
   cat /sys/module/nfs/parameters/nfs4_disable_idmapping

   # If you need to set nfs4_disable_idmapping to Y
   mkdir /mnt/tmp
   mount -t nfs -o sec=sys,vers=4.1 10.31.2.4:/azusbobi-frsinput /mnt/tmp
   umount /mnt/tmp

   echo "Y" > /sys/module/nfs/parameters/nfs4_disable_idmapping

   # Make the configuration permanent
   echo "options nfs nfs4_disable_idmapping=Y" >> /etc/modprobe.d/nfs.conf
   ```

3. **[A]** Agregar entradas de montaje.

   Si usa NFSv3:

   ```bash
   sudo echo "10.31.2.4:/azusbobi-frsinput /usr/sap/frsinput  nfs   rw,hard,rsize=65536,wsize=65536,vers=3" >> /etc/fstab
   sudo echo "10.31.2.4:/azusbobi-frsoutput /usr/sap/frsoutput  nfs   rw,hard,rsize=65536,wsize=65536,vers=3" >> /etc/fstab
   ```

   Si usa NFSv4.1:

   ```bash
   sudo echo "10.31.2.4:/azusbobi-frsinput /usr/sap/frsinput  nfs   rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys" >> /etc/fstab
   sudo echo "10.31.2.4:/azusbobi-frsoutput /usr/sap/frsoutput  nfs   rw,hard,rsize=65536,wsize=65536,vers=4.1,sec=sys" >> /etc/fstab
   ```

4. **[A]** Montar volúmenes de NFS.

   ```bash
   sudo mount -a

   sudo df -h

   Filesystem                     Size  Used Avail Use% Mounted on
   devtmpfs                       7.9G  8.0K  7.9G   1% /dev
   tmpfs                          7.9G   82M  7.8G   2% /run
   tmpfs                          7.9G     0  7.9G   0% /sys/fs/cgroup
   /dev/sda4                       29G  1.8G   27G   6% /
   tmpfs                          1.6G     0  1.6G   0% /run/user/1000
   /dev/sda3                     1014M   87M  928M   9% /boot
   /dev/sda2                      512M  1.1M  511M   1% /boot/efi
   /dev/sdb1                       32G   49M   30G   1% /mnt
   /dev/sdc                       128G   29G  100G  23% /usr/sap
   10.31.2.4:/azusbobi-frsinput   101T   18G  100T   1% /usr/sap/frsinput
   10.31.2.4:/azusbobi-frsoutput  100T  512K  100T   1% /usr/sap/frsoutput
   ```

## <a name="configure-azure-database-for-mysql"></a>Configuración de Azure Database for MySQL

En esta sección se proporcionan detalles sobre cómo aprovisionar Azure Database for MySQL mediante Azure Portal. También se proporcionan instrucciones sobre cómo crear bases de datos CMS y de auditoría para la plataforma SAP BOBI y una cuenta de usuario para tener acceso a la base de datos.

Las instrucciones solo se aplican si usa Azure Database for MySQL. En el caso de otras bases de datos, consulte la documentación específica de SAP o de la base de datos para obtener instrucciones.

### <a name="create-a-database"></a>Crear una base de datos

Inicie sesión en el Azure Portal y siga los pasos descritos en [Inicio rápido: Creación de un servidor Azure Database for MySQL mediante el Azure Portal](../../../mysql/quickstart-create-mysql-server-database-using-azure-portal.md). Los siguientes puntos se deben tener en cuenta durante el aprovisionamiento de Azure Database for MySQL:

- Seleccione la misma región para Azure Database for MySQL donde se ejecutan los servidores de aplicaciones de la plataforma de inteligencia empresarial SAP.

- Elija una versión de base de datos compatible en función de la [matriz de disponibilidad de productos (PAM) para SAP BI](https://support.sap.com/pam) específica de la versión de SAP BOBI.

- En **Proceso y almacenamiento**, seleccione **Configurar servidor** y seleccione el plan de tarifa adecuado en función del tamaño de la salida.

- El **crecimiento automático de almacenamiento** está habilitado de manera predeterminada. Recuerde que el [almacenamiento](../../../mysql/concepts-pricing-tiers.md#storage) solo se puede escalar verticalmente, no reducir.

- De manera predeterminada, el **período de retención de copia de seguridad** es de siete días. [Opcionalmente, puede configurarlo](../../../mysql/howto-restore-server-portal.md#set-backup-configuration) hasta 35 días.

- Las copias de seguridad de Azure Database for MySQL son redundantes localmente de forma predeterminada. Si quiere copias de seguridad del servidor en el almacenamiento con redundancia geográfica, seleccione **Redundancia geográfica** en **Opciones de redundancia de copia de seguridad**.

>[!Important]
>No se admite el cambio de [Opciones de redundancia de copia de seguridad](../../../mysql/concepts-backup.md#backup-redundancy-options) después de la creación del servidor.

>[!Note]
>La característica de vínculo privado solo está disponible para servidores de Azure Database for MySQL en los planes de tarifa De uso general u Optimizado para memoria. Asegúrese de que el servidor de bases de datos esté incluido en uno de estos planes de tarifa.

### <a name="configure-azure-private-link"></a>Configuración de Azure Private Link

En esta sección, se crea un vínculo privado que permite que las máquinas virtuales de SAP BOBI se conecten a Azure Database for MySQL a través de un punto de conexión privado. Azure Private Link incorpora los servicios de Azure dentro de su red virtual privada.

1. Seleccione la base de datos creada en la sección anterior.
2. Vaya a **Seguridad** > **Conexiones de punto de conexión privado**.
3. En **Conexiones de punto de conexión privado**, seleccione **Punto de conexión privado**.
4. Seleccione **Suscripción** > **Grupo de recursos** > **Ubicación**.
5. Escriba el **Nombre** del punto de conexión privado.
6. En la sección **Recurso**, especifique lo siguientes:
   - Tipo de recurso: Microsoft.DBforMySQL/servers
   - Recurso: base de datos MySQL creada en la sección anterior
   - Subrecurso de destino: mysqlServer
7. En la sección **Redes**, seleccione la **Red virtual** y la **Subred** donde se ha implementado la aplicación SAP BOBI.
   >[!NOTE]
   >Si tiene un grupo de seguridad de red (NSG) habilitado para la subred, se deshabilitará para los puntos de conexión privados solo en esta subred. En el resto de recursos de la subred seguirá vigente el grupo de seguridad de red.
8. En **Integrar con la zona DNS privada**, acepte el **valor predeterminado (sí)** .
9.  Seleccione la **Zona DNS privada** en la lista desplegable.
10. Seleccione **Revisar y crear**, y cree un punto de conexión privado.

Para más información, consulte [Private Link para Azure Database for MySQL](../../../mysql/concepts-data-access-security-private-link.md).

### <a name="create-the-cms-and-audit-databases"></a>Creación de las bases de datos CMS y de auditoría

1. Descargue e instale MySQL Workbench desde el [sitio web de MySQL](https://dev.mysql.com/downloads/workbench/). Asegúrese de instalar MySQL Workbench en el servidor que puede tener acceso a Azure Database for MySQL.

2. Conéctese al servidor con MySQL Workbench. Siga las instrucciones de [Obtención de información de conexión](../../../mysql/connect-workbench.md#get-connection-information). Si la prueba de la conexión es correcta, aparece el mensaje siguiente:

   ![Captura de pantalla del mensaje en MySQL Workbench.](media/businessobjects-deployment-guide/businessobjects-sql-workbench.png)

3. En la pestaña de la consulta SQL, ejecute la siguiente consulta para crear un esquema para las bases de datos CMS y de auditoría.

   ```sql
   # Here cmsbl1 is the database name of CMS database. You can provide the name you want for CMS database.
   CREATE SCHEMA `cmsbl1` DEFAULT CHARACTER SET utf8;

   # auditbl1 is the database name of Audit database. You can provide the name you want for CMS database.
   CREATE SCHEMA `auditbl1` DEFAULT CHARACTER SET utf8;
   ```

4. Cree una cuenta de usuario para conectarse al esquema.

   ```sql
   # Create a user that can connect from any host, use the '%' wildcard as a host part
   CREATE USER 'cmsadmin'@'%' IDENTIFIED BY 'password';
   CREATE USER 'auditadmin'@'%' IDENTIFIED BY 'password';

   # Grant all privileges to a user account over a specific database:
   GRANT ALL PRIVILEGES ON cmsbl1.* TO 'cmsadmin'@'%' WITH GRANT OPTION;
   GRANT ALL PRIVILEGES ON auditbl1.* TO 'auditadmin'@'%' WITH GRANT OPTION;

   # Following any updates to the user privileges, be sure to save the changes by issuing the FLUSH PRIVILEGES
   FLUSH PRIVILEGES;
   ```

5. Para comprobar los privilegios y roles de la cuenta de usuario de MySQL:

   ```sql
   USE sys;
   SHOW GRANTS for 'cmsadmin'@'%';
   +------------------------------------------------------------------------+
   | Grants for cmsadmin@%                                                  |
   +------------------------------------------------------------------------+
   | GRANT USAGE ON *.* TO `cmsadmin`@`%`                                   |
   | GRANT ALL PRIVILEGES ON `cmsbl1`.* TO `cmsadmin`@`%` WITH GRANT OPTION |
   +------------------------------------------------------------------------+

   USE sys;
   SHOW GRANTS FOR 'auditadmin'@'%';
   +----------------------------------------------------------------------------+
   | Grants for auditadmin@%                                                    |
   +----------------------------------------------------------------------------+
   | GRANT USAGE ON *.* TO `auditadmin`@`%`                                     |
   | GRANT ALL PRIVILEGES ON `auditbl1`.* TO `auditadmin`@`%` WITH GRANT OPTION |
   +----------------------------------------------------------------------------+
   ```

### <a name="install-mysql-c-api-connector-on-a-linux-server"></a>Instalación del conector de MySQL C API en un servidor Linux

Para que el servidor de aplicaciones de SAP BOBI tenga acceso a la base de datos, requiere controladores de cliente de base de datos. Para acceder a las bases de datos CMS y de auditoría, debe usar el conector de MySQL C API para Linux. No se admite la conexión ODBC con la base de datos CMS. En esta sección se proporcionan instrucciones sobre cómo configurar el conector de MySQL C API en Linux.

1. Consulte [Herramientas de administración y controladores de MySQL compatibles con Azure Database for MySQL](../../../mysql/concepts-compatibility.md). Busque el controlador **Conector de MySQL/C (libmysqlclient)** en el artículo.

2. Para descargar controladores, consulte [Archivos de productos de MySQL](https://downloads.mysql.com/archives/c-c/).

3. Seleccione el sistema operativo y descargue el paquete RPM del componente compartido del conector MySQL. En este ejemplo, se usa la versión del conector mysql-connector-c-shared-6.1.11.

4. Instale el conector en todas las instancias de la aplicación de SAP BOBI.

   ```bash
   # Install rpm package
   SLES: sudo zypper install <package>.rpm
   RHEL: sudo yum install <package>.rpm
   ```

5. Compruebe la ruta de acceso de libmysqlclient.so.

   ```bash
   # Find the location of libmysqlclient.so file
   whereis libmysqlclient

   # sample output
   libmysqlclient: /usr/lib64/libmysqlclient.so
   ```

6. Establezca `LD_LIBRARY_PATH` para que apunte al directorio `/usr/lib64` de la cuenta de usuario que se utilizará para la instalación.

   ```bash
   # This configuration is for bash shell. If you are using any other shell for sidadm, kindly set environment variable accordingly.
   vi /home/bl1adm/.bashrc

   export LD_LIBRARY_PATH=/usr/lib64
   ```

## <a name="server-preparation"></a>Preparación del servidor

En los pasos de esta sección se usan los siguientes prefijos:

**[A]** : El paso se aplica a todos los hosts.

1. **[A]** En función del tipo de Linux (SLES o RHEL), debe establecer los parámetros de kernel e instalar las bibliotecas necesarias. Consulte la sección "Requisitos del sistema" del [Manual de instalación de la plataforma de Business Intelligence para UNIX](https://help.sap.com/viewer/65018c09dbe04052b082e6fc4ab60030/4.3).

2. **[A]** Asegúrese de que la zona horaria de la máquina está configurada correctamente. En el manual de instalación, consulte [Requisitos adicionales de UNIX y Linux](https://help.sap.com/viewer/65018c09dbe04052b082e6fc4ab60030/4.3/46b143336e041014910aba7db0e91070.html).

3. **[A]** Cree una cuenta de usuario (**bl1** adm) y un grupo (sapsys) en el que se puedan ejecutar los procesos en segundo plano del software. Use esta cuenta para ejecutar la instalación y el software. La cuenta no requiere privilegios de raíz.

4. **[A]** Establezca el entorno de cuenta de usuario (**bl1** adm) para usar una configuración regional UTF-8 compatible y asegúrese de que el software de la consola admite juegos de caracteres UTF-8. Para asegurarse de que el sistema operativo usa la configuración regional correcta, establezca las variables de entorno `LC_ALL` y `LANG` en su configuración regional preferida en el entorno de usuario (**bl1** adm).

   ```bash
   # This configuration is for bash shell. If you are using any other shell for sidadm, kindly set environment variable accordingly.
   vi /home/bl1adm/.bashrc

   export LANG=en_US.utf8
   export LC_ALL=en_US.utf8
   ```

5. **[A]** Configure la cuenta de usuario (**bl1** adm).

   ```bash
   # Set ulimit for bl1adm to unlimited
   root@azusbosl1:~> ulimit -f unlimited bl1adm
   root@azusbosl1:~> ulimit -u unlimited bl1adm

   root@azusbosl1:~> su - bl1adm
   bl1adm@azusbosl1:~> ulimit -a

   core file size          (blocks, -c) unlimited
   data seg size           (kbytes, -d) unlimited
   scheduling priority             (-e) 0
   file size               (blocks, -f) unlimited
   pending signals                 (-i) 63936
   max locked memory       (kbytes, -l) 64
   max memory size         (kbytes, -m) unlimited
   open files                      (-n) 1024
   pipe size            (512 bytes, -p) 8
   POSIX message queues     (bytes, -q) 819200
   real-time priority              (-r) 0
   stack size              (kbytes, -s) 8192
   cpu time               (seconds, -t) unlimited
   max user processes              (-u) unlimited
   virtual memory          (kbytes, -v) unlimited
   file locks                      (-x) unlimited
   ```

6. Descargue y extraiga los medios para la plataforma de inteligencia empresarial SAP BusinessObjects desde el marketplace de servicios de SAP.

## <a name="installation"></a>Instalación

Compruebe la configuración regional de la cuenta de usuario **bl1** adm en el servidor:

```bash
bl1adm@azusbosl1:~> locale
LANG=en_US.utf8
LC_ALL=en_US.utf8
```

Vaya al medio de la plataforma SAP BOBI y ejecute el siguiente comando con el usuario **bl1** adm:

```bash
./setup.sh -InstallDir /usr/sap/BL1
```

Siga el manual de instalación de la [plataforma SAP BOBI](https://help.sap.com/viewer/product/SAP_BUSINESSOBJECTS_BUSINESS_INTELLIGENCE_PLATFORM) para UNIX, específico de su versión. Estos son algunos puntos a tener en cuenta al instalar la plataforma SAP BOBI:

- En **Configure Product Registration** (Configurar el registro del producto), puede usar una clave de licencia temporal para las soluciones de SAP BusinessObjects de la nota de SAP [1288121](https://launchpad.support.sap.com/#/notes/1288121), o puede generar la clave de licencia en el marketplace de servicios de SAP.

- En **Select Install Type** (Seleccionar tipo de instalación), seleccione la instalación **Completa** en el primer servidor (`azusbosl1`). Para el otro servidor (`azusbosl2`), seleccione **Custom / Expand** (Personalizar/Expandir), que expandirá la configuración de BOBI existente.

- En **Seleccionar base de datos predeterminada o existente**, seleccione **configure an existing database** (Configurar una base de datos existente), que le solicitará que seleccione base de datos CMS y de auditoría. Seleccione **MySQL** para estas bases de datos.

  También puede seleccionar **No auditing database** (Ninguna base de datos de auditoría) si no quiere configurar la auditoría durante la instalación.

- En la **pantalla de selección del servidor de aplicaciones web de Java**, seleccione las opciones adecuadas según la arquitectura de SAP BOBI. En este ejemplo, se ha seleccionado la opción 1, que instala el servidor de Tomcat en la misma plataforma SAP BOBI.

- Escriba la información de la base de datos CMS en **Configurar la base de datos del repositorio de CMS: MySQL**. En el ejemplo siguiente se muestra la entrada para la información de la base de datos CMS para una instalación de Linux. Azure Database for MySQL se usa en el puerto 3306 predeterminado.
  
  ![Captura de pantalla que muestra la implementación de SAP BOBI en Linux: base de datos CMS.](media/businessobjects-deployment-guide/businessobjects-deployment-linux-sql-cms.png)

- (Opcional) Escriba la información de la base de datos de auditoría en **Configurar la base de datos del repositorio de auditoría: MySQL**. En el ejemplo siguiente se muestra la entrada para la información de la base de datos de auditoría para una instalación de Linux.

  ![Captura de pantalla que muestra la implementación de SAP BOBI en Linux: base de datos de auditoría.](media/businessobjects-deployment-guide/businessobjects-deployment-linux-sql-audit.png)

- Siga las instrucciones e introduzca las entradas necesarias para completar la instalación.

Para la implementación de varias instancias, ejecute la configuración de instalación en el segundo host (`azusbosl2`). En **Select Install Type** (Seleccionar tipo de instalación), seleccione **Custom / Expand** (Personalizar/Expandir), que expandirá la configuración de BOBI existente.

En Azure Database for MySQL, una puerta de enlace para redirigir las conexiones a las instancias de servidor. Una vez establecida la conexión, el cliente de MySQL muestra la versión de MySQL establecida en la puerta de enlace, no la versión real que se ejecuta en la instancia del servidor MySQL. Para determinar la versión de la instancia del servidor MySQL, use el comando `SELECT VERSION();` en el símbolo del sistema de MySQL. Para más detalles, consulte [Versiones admitidas de servidores de Azure Database for MySQL](../../../mysql/concepts-supported-versions.md).

![Captura de pantalla que muestra la implementación de SAP BOBI en Linux: configuración de CMS.](media/businessobjects-deployment-guide/businessobjects-deployment-linux-sql-cmc.png)

```sql
# Run direct query to the database using MySQL Workbench

select version();

+-----------+
| version() |
+-----------+
| 8.0.15    |
+-----------+
```

## <a name="post-installation"></a>Posterior a la instalación

Después de la instalación de varias instancias de la plataforma SAP BOBI, deben seguirse pasos adicionales posteriores a la configuración para admitir la alta disponibilidad de la aplicación.

### <a name="configure-the-cluster-name"></a>Configuración del nombre de clúster

En la implementación de varias instancias de la plataforma de SAP BOBI, se quiere ejecutar varios servidores CMS juntos en un clúster. Un clúster consta de dos o más servidores CMS que funcionan conjuntamente en una base de datos común del sistema CMS. Si se produce un error en un nodo que se ejecuta en CMS, un nodo con otro CMS seguirá prestando servicio a las solicitudes de la plataforma de inteligencia empresarial. De manera predeterminada, en la plataforma SAP BOBI, un nombre de clúster refleja el nombre de host del primer CMS que instale.

Para configurar el nombre del clúster en Linux, siga las instrucciones de la [Guía del administrador de la plataforma de SAP Business Intelligence](https://help.sap.com/viewer/2e167338c1b24da9b2a94e68efd79c42/4.3). Después de configurar el nombre del clúster, siga la nota de SAP [1660440](https://launchpad.support.sap.com/#/notes/1660440) para establecer la entrada predeterminada del sistema en la página de inicio de sesión de Launchpad para CMC o BI.

### <a name="configure-input-and-output-filestore-location-to-azure-netapp-files"></a>Configuración de la ubicación del almacén de archivos de entrada y salida en Azure NetApp Files

El almacén de archivos hace referencia a los directorios de disco en los que se encuentran los archivos reales de SAP BusinessObjects. La ubicación predeterminada del servidor de repositorios de archivos para la plataforma de SAP BOBI se encuentra en el directorio de instalación local. En una implementación de varias instancias, es importante configurar el almacén de archivos en un almacenamiento compartido, como Azure NetApp Files. Esto permite el acceso al almacén de archivos desde todos los servidores de nivel de almacenamiento.

1. Si aún no ha creado volúmenes NFS, créelos en Azure NetApp Files. (Siga las instrucciones de la sección anterior "Aprovisionamiento de Azure NetApp Files").

2. Monte los volúmenes NFS. (Siga las instrucciones de la sección anterior "Montaje de los volúmenes de Azure NetApp Files").

3. Siga la nota de SAP [2512660](https://launchpad.support.sap.com/#/notes/0002512660) para cambiar la ruta de acceso del repositorio de archivos (tanto de entrada como de salida).

### <a name="session-replication-in-tomcat-clustering"></a>Replicación de sesión de la agrupación en clústeres de Tomcat

Tomcat admite la agrupación en clústeres de dos o más servidores de aplicaciones para la replicación de sesión y la conmutación por error. Las sesiones de la plataforma de SAP BOBI están serializadas, por lo que una sesión de usuario puede conmutar por error a otra instancia de Tomcat, incluso cuando se produce un error en un servidor de aplicaciones.

Por ejemplo, suponga que un usuario está conectado a un servidor web que produce un error mientras el usuario navega por una jerarquía de carpetas en la aplicación de inteligencia empresarial SAP. Con un clúster configurado correctamente, el usuario puede seguir navegando por la jerarquía de carpetas sin que se le redirija a la página de inicio de sesión.

Consulte en la nota de SAP [2808640](https://launchpad.support.sap.com/#/notes/2808640) los pasos para configurar la agrupación en clústeres de Tomcat mediante multidifusión. Sin embargo, tenga en cuenta que Azure no admite la multidifusión. Para que el clúster de Tomcat funcione en Azure, debe usar [StaticMembershipInterceptor](https://tomcat.apache.org/tomcat-8.0-doc/config/cluster-interceptor.html#Static_Membership) (nota de SAP [2764907](https://launchpad.support.sap.com/#/notes/2764907)). Para obtener más información, vea la entrada de blog [Agrupación en clústeres de Tomcat mediante la pertenencia estática de la plataforma de inteligencia empresarial de SAP BusinessObjects](https://blogs.sap.com/2020/09/04/sap-on-azure-tomcat-clustering-using-static-membership-for-sap-businessobjects-bi-platform/).

### <a name="load-balancing-web-tier-of-sap-bi-platform"></a>Nivel web de equilibrio de carga de la plataforma de inteligencia empresarial SAP

En la implementación de varias instancias de SAP BOBI, los servidores de aplicaciones web de Java (nivel web) se ejecutan en dos o más hosts. Para distribuir la carga de usuarios uniformemente entre los servidores web, puede usar un equilibrador de carga entre los usuarios finales y los servidores web. En Azure, puede usar Azure Load Balancer o Azure Application Gateway para administrar el tráfico a los servidores de aplicaciones web. En la sección siguiente se explican los detalles de cada oferta.

#### <a name="azure-load-balancer"></a>Azure Load Balancer

[Azure Load Balancer](../../../load-balancer/load-balancer-overview.md) es un equilibrador de carga de nivel 4 (TCP, UDP) de alto rendimiento y baja latencia. Distribuye el tráfico entre máquinas virtuales (VM) correctas. Un sondeo de estado de equilibrador de carga supervisa un puerto especificado en cada máquina virtual y solo distribuye tráfico a una máquina virtual operativa. Puede elegir un equilibrador de carga público o un equilibrador de carga interno, en función de si quiere que la plataforma de inteligencia empresarial SAP sea accesible desde Internet o no. Tiene redundancia de zona, lo que garantiza una alta disponibilidad en zonas de disponibilidad.

En el diagrama siguiente, consulte la sección sobre el equilibrador de carga interno. El servidor de aplicaciones web se ejecuta en el puerto 8080, el puerto HTTP predeterminado de Tomcat, que el sondeo de estado supervisará. Cualquier solicitud entrante procedente de los usuarios finales se redirige a los servidores de aplicaciones web (`azusbosl1` o `azusbosl2`). El equilibrador de carga no admite la terminación TLS/SSL (también conocida como descarga TLS/SSL). Si usa el equilibrador de carga para distribuir el tráfico entre servidores web, se recomienda usar Standard Load Balancer.

> [!NOTE]
> Cuando las máquinas virtuales sin direcciones IP públicas se colocan en el grupo de Standard Load Balancer interno (sin dirección IP pública), no habrá conectividad saliente de Internet, a menos que se realice una configuración adicional para permitir el enrutamiento a puntos de conexión públicos. Para más información, consulte [Conectividad del punto de conexión público para las máquinas virtuales que usan Azure Standard Load Balancer en escenarios de alta disponibilidad de SAP](high-availability-guide-standard-load-balancer-outbound-connections.md).

![Diagrama en el que se muestra Azure Load Balancer que equilibra el tráfico entre servidores web.](media/businessobjects-deployment-guide/businessobjects-deployment-load-balancer.png)

#### <a name="azure-application-gateway"></a>Azure Application Gateway

[Azure Application Gateway](../../../application-gateway/overview.md) proporciona un controlador de entrega de aplicaciones (ADC) como servicio. Este servicio se usa para ayudar a la aplicación a dirigir el tráfico de usuario a uno o varios servidores de aplicaciones web. Ofrece diversas funcionalidades de equilibrio de carga de capa 7, como la descarga SSL/TLS, firewall de aplicaciones web (WAF) y afinidad de sesión basada en cookies.

En una plataforma de inteligencia empresarial de SAP, Application Gateway dirige el tráfico web de las aplicaciones a los recursos especificados, `azusbosl1` o `azusbos2`. Se asignan escuchas a los puertos, se crean reglas y se agregan recursos a un grupo. En el diagrama siguiente, Application Gateway tiene una dirección IP privada (10.31.3.20) que actúa como punto de entrada para los usuarios. También controla las conexiones TLS/SSL (HTTPS: TCP/443) entrantes, descifra TLS/SSL y pasa la solicitud sin cifrar (HTTP: TCP/8080) a los servidores. Simplifica las operaciones para mantener solo un certificado TLS/SSL en Application Gateway.

![Diagrama en el que se muestra Application Gateway que equilibra el tráfico entre servidores web.](media/businessobjects-deployment-guide/businessobjects-deployment-application-gateway.png)

Para configurar Application Gateway para el servidor web de SAP BOBI, consulte la entrada de blog [Servidores web de SAP BOBI de equilibrio de carga mediante Azure Application Gateway](https://blogs.sap.com/2020/09/17/sap-on-azure-load-balancing-web-application-servers-for-sap-bobi-using-azure-application-gateway/).

> [!NOTE]
> Azure Application Gateway es preferible equilibrar la carga del tráfico en un servidor web. Proporciona características útiles, como la descarga de SSL, la administración centralizada de SSL para reducir la sobrecarga del cifrado y descifrado en el servidor, un algoritmo round robin para distribuir el tráfico, las funcionalidades de WAF y la alta disponibilidad.

## <a name="sap-bobi-platform-reliability-on-azure"></a>Confiabilidad de la plataforma de SAP BOBI en Azure

La de SAP BOBI incluye diferentes niveles, que están optimizados para operaciones y tareas específicas. Cuando un componente de cualquier nivel deja de estar disponible, una aplicación SAP BOBI deja de ser accesible o está limitada en su funcionalidad. Asegúrese de que cada nivel esté diseñado para ser confiable y mantener la aplicación operativa sin interrupción empresarial.

En esta guía se explora cómo las características nativas de Azure, en combinación con la configuración de la plataforma SAP BOBI, mejora la disponibilidad de la implementación de SAP. Esta sección se centra en las siguientes opciones:

- **Copia de seguridad y restauración:** es un proceso de creación de copias periódicas de datos y aplicaciones en una ubicación independiente. Se puede restaurar o recuperar al estado anterior si se pierden o dañan los datos o las aplicaciones originales.

- **Alta disponibilidad:** una plataforma de alta disponibilidad tiene al menos dos de todo en la región de Azure para mantener la aplicación operativa si uno de los servidores deja de estar disponible.
- **Recuperación ante desastres:** se trata de un proceso de restauración de la funcionalidad de la aplicación si hay alguna pérdida catastrófica, como cuando toda la región de Azure deja de estar disponible debido a algún desastre natural.

La implementación de esta solución varía en función de la naturaleza de la configuración del sistema en Azure. Adapte las soluciones de copia de seguridad y restauración, alta disponibilidad y recuperación ante desastres según sus requisitos empresariales.

## <a name="backup-and-restore"></a>Copia de seguridad y restauración

La copia de seguridad y restauración es un componente esencial de cualquier estrategia de recuperación ante desastres empresarial. Para desarrollar una estrategia completa para la plataforma SAP BOBI, identifique los componentes que provocan el tiempo de inactividad del sistema o la interrupción de la aplicación. En la plataforma de SAP BOBI, la copia de seguridad de los componentes siguientes es fundamental para proteger la aplicación:

- Directorio de instalación de SAP BOBI (Managed Disks Premium)
- Servidor de repositorio de archivos (Azure NetApp Files o archivos prémium de Azure)
- Base de datos CMS (Azure Database for MySQL o una base de datos en Azure Virtual Machines)

En la siguiente sección se describe cómo implementar una estrategia de copia de seguridad y restauración para cada uno de estos componentes.

### <a name="backup-and-restore-for-sap-bobi-installation-directory"></a>Copia de seguridad y restauración para un directorio de instalación de SAP BOBI

En Azure, la manera más sencilla de realizar copias de seguridad de los servidores de aplicaciones y de todos los discos conectados es mediante [Azure Backup](../../../backup/backup-overview.md). Proporciona copias de seguridad independientes y aisladas para impedir la destrucción accidental de los datos en las máquinas virtuales. Las copias de seguridad se almacenan en un almacén de servicios de recuperación, con administración integrada de puntos de recuperación. La configuración y el escalado son sencillos, las copias de seguridad están optimizadas y puede restaurarlas fácilmente cuando sea necesario.

Como parte del proceso de copia de seguridad, se toma una instantánea y los datos se transfieren al almacén sin que ello afecte a las cargas de trabajo de producción. Para más información, consulte [Coherencias de instantáneas](../../../backup/backup-azure-vms-introduction.md#snapshot-consistency). También puede optar por realizar una copia de seguridad de un subconjunto de los discos de datos de una máquina virtual, mediante el uso de la funcionalidad de copia de seguridad y restauración de discos selectivos. Para más información, consulte [Copia de seguridad de máquinas virtuales de Azure](../../../backup/backup-azure-vms-introduction.md) y [Preguntas más frecuentes sobre la copia de seguridad de máquinas virtuales de Azure](../../../backup/backup-azure-vm-backup-faq.yml).

### <a name="backup-and-restore-for-file-repository-server"></a>Copia de seguridad y restauración del servidor de repositorio de archivos

En función de la implementación de SAP BOBI en Linux, puede usar Azure NetApp Files como almacén de archivos de la plataforma de SAP BOBI. Elija entre las siguientes opciones de copia de seguridad y restauración en función del almacenamiento que use para el almacén de archivos.

- **Azure NetApp Files:** puede crear instantáneas a petición y programar la creación automática de instantáneas mediante directivas de instantáneas. Las copias de instantáneas proporcionan una copia de un momento dado del volumen. Para obtener más información, consulte [Administración de instantáneas mediante Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-manage-snapshots.md).

- Si ha creado un servidor NFS independiente, asegúrese de implementar la estrategia de copia de seguridad y restauración para el mismo servidor.

### <a name="backup-and-restore-for-cms-and-audit-databases"></a>Copia de seguridad y restauración de la base de datos CMS y de auditoría

En máquinas virtuales de Linux, las bases de datos CMS y de auditoría pueden ejecutarse en cualquiera de las bases de datos admitidas. Para obtener más información, consulte la [matriz de compatibilidad](businessobjects-deployment-guide.md#support-matrix). Es importante que adopte la estrategia de copia de seguridad y restauración en función de la base de datos usada para el almacén de datos CMS y de auditoría.

- Azure Database for MySQL crea automáticamente copias de seguridad del servidor y las guarda en un almacenamiento con redundancia local o con redundancia geográfica configurado por el usuario. Azure Database for MySQL hace copias de seguridad de los archivos de datos y del registro de transacciones. En función del tamaño de almacenamiento máximo admitido, se realizan copias de seguridad completas y diferenciales (servidores de almacenamiento de hasta 4 TB) o copias de seguridad de instantáneas (servidores de almacenamiento de hasta 16 TB). Estas copias de seguridad permiten restaurar un servidor a un momento dado dentro del período de retención de copias de seguridad configurado. De manera predeterminada, el período de retención de la copia de seguridad es de siete días, pero se puede [configurar opcionalmente](../../../mysql/howto-restore-server-portal.md#set-backup-configuration) hasta tres días. Todas las copias de seguridad se cifran mediante cifrado AES de 256 bits. Estos archivos de copia de seguridad no están expuestos al usuario y no se pueden exportar. Estas copias de seguridad solo se pueden usar en operaciones de restauración de Azure Database for MySQL. Puede usar [mysqldump](../../../mysql/concepts-migrate-dump-restore.md) para copiar una base de datos. Para más información, consulte [Copia de seguridad y restauración en Azure Database for MySQL](../../../mysql/concepts-backup.md).

- Para una base de datos instalada en una máquina virtual de Azure, puede usar herramientas de copia de seguridad estándar o [Azure Backup](../../../backup/sap-hana-db-about.md) para las bases de datos admitidas. Puede usar herramientas de copia de seguridad compatibles de terceros que proporcionen un agente para la copia de seguridad y recuperación de todos los componentes de la plataforma SAP BOBI.

## <a name="high-availability"></a>Alta disponibilidad

La *alta disponibilidad* hace referencia a un conjunto de tecnologías que pueden minimizar las interrupciones de IT al proporcionar continuidad empresarial de aplicaciones y servicios. Lo hace a través de componentes redundantes, tolerantes a errores o protegidos por conmutación por error dentro del mismo centro de datos. En nuestro caso, el centro de datos se encuentra en una región de Azure. Para más información, consulte [Escenarios y arquitectura de alta disponibilidad para SAP](sap-high-availability-architecture-scenarios.md).

En función del resultado del tamaño de la plataforma SAP BOBI, debe diseñar el panorama y determinar la distribución de los componentes de inteligencia empresarial en Azure Virtual Machines y las subredes. El nivel de redundancia en la arquitectura distribuida depende del objetivo de tiempo de recuperación (RTO) y del objetivo de punto de recuperación (RPO) necesarios para la empresa. La plataforma SAP BOBI incluye diferentes niveles y componentes en cada nivel que se deben diseñar para lograr redundancia. Por ejemplo:

- Servidores de aplicaciones redundantes, como servidores de aplicaciones de inteligencia empresarial y servidores web.
- Componentes únicos, como la base de datos CMS, el servidor de repositorio de archivos y Load Balancer.

En las siguientes secciones se describe cómo lograr una alta disponibilidad en cada componente de la plataforma SAP BOBI.

### <a name="high-availability-for-application-servers"></a>Alta disponibilidad en los servidores de aplicaciones

Puede lograr una alta disponibilidad para los servidores de aplicaciones mediante el empleo de redundancia. Para ello, configure varias instancias de inteligencia empresarial y servidores web en varias máquinas virtuales de Azure.

Para reducir el impacto del tiempo de inactividad debido a uno o varios eventos, es una buena idea:

- Usar zonas de disponibilidad para protegerse frente a errores en el centro de datos.
- Configurar varias máquinas virtuales en un conjunto de disponibilidad para la redundancia.
- Usar discos administrados para las máquinas virtuales de un conjunto de disponibilidad.
- Configure cada capa de aplicación en conjuntos de disponibilidad independientes.

Para más información, consulte [Administración de la disponibilidad de las máquinas virtuales de Linux](../../availability.md).

>[!Important]
>Los conceptos de zonas de disponibilidad de Azure y conjuntos de disponibilidad de Azure son mutuamente excluyentes. Puede implementar dos o varias máquinas virtuales en una zona de disponibilidad específica o en un conjunto de disponibilidad, pero no en ambos.

### <a name="high-availability-for-a-cms-database"></a>Alta disponibilidad de una base de datos CMS

Si usa Azure Database for MySQL para las bases de datos CMS y de auditoría, tiene un marco de alta disponibilidad con redundancia local de forma predeterminada. Solo tiene que seleccionar las capacidades de alta disponibilidad, redundancia y resistencia inherentes a la región y el servicio, sin necesidad de configurar ningún componente adicional. Si la estrategia de implementación de la plataforma SAP BOBI se aplica entre zonas de disponibilidad, debe asegurarse de lograr la redundancia de zona para las bases de datos CMS y de auditoría. Para más información, consulte [Alta disponibilidad en Azure Database for MySQL](../../../mysql/concepts-high-availability.md) y [Alta disponibilidad para Azure SQL Database](../../../azure-sql/database/high-availability-sla.md).

Para otras implementaciones de la base de datos CMS, consulte la información de alta disponibilidad en las [guías de implementación de DBMS para la carga de trabajo de SAP](dbms_guide_general.md).

### <a name="high-availability-for-filestore"></a>Alta disponibilidad para el almacén de archivos

El almacén de archivos hace referencia a los directorios del disco donde se almacena contenido como informes, universos y conexiones. Se comparte entre todos los servidores de aplicaciones de ese sistema. Por lo tanto, debe asegurarse de que tenga alta disponibilidad, junto con otros componentes de la plataforma SAP BOBI.

Para la plataforma SAP BOBI que se ejecuta en Linux, puede elegir [Azure Premium Files](../../../storage/files/storage-files-introduction.md) o [Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-introduction.md) para los recursos compartidos de archivos que por naturaleza están diseñados para tener alta disponibilidad y alta durabilidad. Para más información, consulte [Redundancia](../../../storage/files/storage-files-planning.md#redundancy) para Azure Files.

> [!Important]
> El protocolo SMB para Azure Files está disponible con carácter general, pero la compatibilidad con el protocolo NFS para Azure Files se encuentra actualmente en versión preliminar. Para más información, consulte [La compatibilidad de NFS 4.1 con Azure Files ya se encuentra en versión preliminar](https://azure.microsoft.com/blog/nfs-41-support-for-azure-files-is-now-in-preview/).

Tenga en cuenta que este servicio de recursos compartidos de archivos no está disponible en todas las regiones. Consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/) para buscar información actualizada. Si el servicio no está disponible en su región, puede crear un servidor NFS desde el que pueda compartir el sistema de archivos con la aplicación SAP BOBI. Sin embargo, también debe tener en cuenta su alta disponibilidad.

### <a name="high-availability-for-load-balancer"></a>Alta disponibilidad para Load Balancer

Para distribuir el tráfico a través de un servidor web, puede usar Azure Load Balancer o Azure Application Gateway. La redundancia para cualquiera de ellos se puede lograr en función de la SKU que elija para la implementación.

- Para Azure Load Balancer, la redundancia se puede lograr mediante la configuración de Standard Load Balancer como con redundancia de zona. Para más información, consulte [Standard Load Balancer y Availability Zones](../../../load-balancer/load-balancer-standard-availability-zones.md).

- Para Application Gateway, se puede lograr alta disponibilidad en función del tipo de nivel seleccionado durante la implementación.
   -  La SKU v1 admite escenarios de alta disponibilidad cuando se han implementado dos o más instancias. Azure distribuye estas instancias entre dominios de actualización y de errores para asegurarse de que las instancias no produzcan un error todas al mismo tiempo. Se consigue redundancia dentro de la zona.
   -  La SKU v2 garantiza automáticamente que las nuevas instancias se distribuyan entre dominios de error y dominios de actualización. Si elige la redundancia de zona, las instancias más recientes también se distribuyen entre las zonas de disponibilidad para ofrecer resistencia ante errores de zona. Para más información, vea [Escalabilidad automática y Application Gateway v2 con redundancia de zona](../../../application-gateway/application-gateway-autoscaling-zone-redundant.md).

### <a name="reference-high-availability-architecture-for-sap-bobi-platform"></a>Arquitectura de alta disponibilidad de referencia para la plataforma SAP BOBI

En el diagrama siguiente se muestra la configuración de la plataforma SAP BOBI cuando se usa un conjunto de disponibilidad que se ejecuta en el servidor Linux. La arquitectura muestra el uso de diferentes servicios, como Azure Application Gateway, Azure NetApp Files y Azure Database for MySQL. Estos servicios ofrecen redundancia integrada, lo que reduce la complejidad de administrar diferentes soluciones de alta disponibilidad.

Observe que en el tráfico entrante (HTTPS: TCP/443) se equilibra la carga mediante la SKU de Azure Application Gateway v1, que es de alta disponibilidad cuando se implementa en dos o más instancias. Se implementan varias instancias del servidor web, los servidores de administración y los servidores de procesamiento en instancias de Virtual Machines independientes para lograr la redundancia y cada nivel se implementa en conjuntos de disponibilidad independientes. Azure NetApp Files tiene redundancia integrada dentro del centro de datos, por lo que los volúmenes de Azure NetApp Files para el servidor de repositorio de archivos tendrán una alta disponibilidad. La base de datos CMS se aprovisiona en Azure Database for MySQL, que tiene una alta disponibilidad inherente. Para más información, consulte [Alta disponibilidad en Azure Database for MySQL](../../../mysql/concepts-high-availability.md).

![Diagrama que muestra la redundancia de la plataforma de inteligencia empresarial de SAP BusinessObjects con conjuntos de disponibilidad.](media/businessobjects-deployment-guide/businessobjects-deployment-high-availability.png)

En la arquitectura anterior se proporciona información sobre cómo se puede realizar la implementación de SAP BOBI en Azure. Sin embargo, no cubre todas las posibles opciones de configuración. Puede adaptar la implementación en función de sus requisitos empresariales.

En varias regiones de Azure, puede usar zonas de disponibilidad. Esto significa que puede aprovechar una fuente de alimentación, refrigeración y red independientes. Permite implementar la aplicación entre dos o tres zonas de disponibilidad. Si quiere lograr una alta disponibilidad entre zonas de disponibilidad, puede implementar la plataforma SAP BOBI en todas esas zonas, asegurándose de que todos los componentes de la aplicación tengan redundancia de zona.

## <a name="disaster-recovery"></a>Recuperación ante desastres

En esta sección se explica la estrategia para proporcionar la protección de recuperación ante desastres para la plataforma SAP BOBI que se ejecuta en Linux. Complementa el documento [Recuperación ante desastres para SAP](../../../site-recovery/site-recovery-sap.md), que representa los recursos principales para el enfoque general de la recuperación ante desastres de SAP. Para SAP BOBI, consulte la nota de SAP [2056228](https://launchpad.support.sap.com/#/notes/2056228), en la que se describen los métodos siguientes para implementar un entorno de recuperación ante desastres de forma segura.

- Uso total o selectivo de la administración del ciclo de vida o de la federación para promover y distribuir el contenido del sistema principal.
- Copia periódica del contenido del servidor CMS y del repositorio de archivos.

Esta guía se centra en la segunda opción. No abarca todas las opciones de configuración posibles para la recuperación ante desastres, sino que trata una solución que incluye los servicios nativos de Azure en combinación con la configuración de la plataforma SAP BOBI.

>[!Important]
>La disponibilidad de cada componente en la plataforma de inteligencia empresarial SAP BOBI debe tenerse en cuenta en la región secundaria, y toda la estrategia de recuperación ante desastres debe probarse exhaustivamente.

### <a name="reference-disaster-recovery-architecture-for-sap-bobi-platform"></a>Arquitectura de recuperación ante desastres de referencia para la plataforma SAP BOBI

Esta arquitectura de referencia ejecuta una implementación de varias instancias de la plataforma SAP BOBI con servidores de aplicaciones redundantes. Para la recuperación ante desastres, debe conmutar por error todos los componentes de la plataforma SAP BOBI a una región secundaria. En el diagrama siguiente, Azure NetApp Files se usa como almacén de archivos, Azure Database for MySQL como repositorio CMS y de auditoría, y Azure Application Gateway como equilibrador de carga. La estrategia para lograr la protección de la recuperación ante desastres para cada componente es diferente, y estas diferencias se describen más detalladamente en las secciones siguientes.

![Diagrama que muestra la recuperación ante desastres de la plataforma de inteligencia empresarial de SAP BusinessObjects.](media/businessobjects-deployment-guide/businessobjects-deployment-disaster-recovery.png)

### <a name="load-balancer"></a>Equilibrador de carga

Un equilibrador de carga se usa para distribuir el tráfico entre servidores de aplicaciones web de la plataforma SAP BOBI. En Azure, puede usar Azure Load Balancer o Azure Application Gateway para este fin. Para lograr la recuperación ante desastres para los servicios del equilibrador de carga, debe implementar otra instancia de Azure Load Balancer o Azure Application Gateway en la región secundaria. Para mantener la misma dirección URL después de la conmutación por error de recuperación ante desastres, debe cambiar la entrada en DNS, que apunte al servicio de equilibrio de carga que se ejecuta en la región secundaria.

### <a name="vms-running-web-and-bi-application-servers"></a>Máquinas virtuales que ejecutan servidores de aplicaciones web y de inteligencia empresarial

Use [Azure Site Recovery](../../../site-recovery/site-recovery-overview.md) para replicar máquinas virtuales que ejecutan servidores de aplicaciones web y de inteligencia empresarial en la región secundaria. Replica los servidores y todos sus discos administrados conectados en la región secundaria para que, cuando se produzcan desastres o interrupciones, pueda conmutar por error fácilmente a su entorno replicado y seguir trabajando. Para comenzar a replicar todas las máquinas virtuales de la aplicación SAP en el centro de datos de recuperación ante desastres de Azure, siga las instrucciones de [Replicación de una máquina virtual en Azure](../../../site-recovery/azure-to-azure-tutorial-enable-replication.md).

### <a name="file-repository-servers"></a>Servidores de repositorio de archivos

El almacén de archivos es un directorio de discos donde se almacenan los archivos reales, como los informes y los documentos de BI. Es importante que todos los archivos del almacén de archivos estén sincronizados con la región de recuperación ante desastres. En función del tipo de servicio de recurso compartido de archivos que use para la plataforma SAP BOBI que se ejecuta en Linux, es necesario adoptar la estrategia adecuada de recuperación ante desastres para sincronizar el contenido.

- **Azure NetApp Files** proporciona volúmenes NFS y SMB, por lo que puede usar cualquier herramienta de copia basada en archivos para replicar datos entre regiones de Azure. Para más información sobre cómo copiar un volumen en otra región, consulte [Preguntas más frecuentes acerca de Azure NetApp Files](../../../azure-netapp-files/faq-data-migration-protection.md#how-do-i-create-a-copy-of-an-azure-netapp-files-volume-in-another-azure-region).

  Puede usar la replicación entre regiones de Azure NetApp Files, actualmente en [versión preliminar](https://azure.microsoft.com/blog/azure-netapp-files-cross-region-replication-and-new-enhancements-in-preview/). Así que solo los bloques modificados se envían a través de la red en un formato comprimido y eficaz. Esto reduce la cantidad de datos necesarios para replicar entre regiones, lo que ahorra costes de transferencia de datos. También acorta el tiempo de replicación, por lo que puede lograr un RPO menor. Para obtener más información, consulte [Requisitos y consideraciones del uso de la replicación entre regiones](../../../azure-netapp-files/cross-region-replication-requirements-considerations.md).

- **Los archivos de Azure Premium** solo admiten el almacenamiento con redundancia local (LRS) y con redundancia de zona (ZRS). Para la estrategia de recuperación ante desastres, puede usar [AzCopy](../../../storage/common/storage-use-azcopy-v10.md) o [Azure PowerShell](/powershell/module/az.storage/) para copiar los archivos en otra cuenta de almacenamiento en una región distinta. Para más información, consulte [Recuperación ante desastres y conmutación por error de la cuenta de almacenamiento](../../../storage/common/storage-disaster-recovery-guidance.md).

   > [!Important]
   > El protocolo SMB para Azure Files está disponible con carácter general, pero la compatibilidad con el protocolo NFS para Azure Files se encuentra actualmente en versión preliminar. Para más información, consulte [La compatibilidad de NFS 4.1 con Azure Files ya se encuentra en versión preliminar](https://azure.microsoft.com/blog/nfs-41-support-for-azure-files-is-now-in-preview/).

### <a name="cms-database"></a>Base de datos CMS

Las bases de datos CMS y de auditoría de la región de recuperación ante desastres deben ser una copia de las bases de datos que se ejecutan en la región primaria. En función del tipo de base de datos, es importante copiar la base de datos en la región de recuperación ante desastres en función del RTO y el RPO que requiera su empresa.

#### <a name="azure-database-for-mysql"></a>Azure Database for MySQL

Azure Database for MySQL proporciona varias opciones para recuperar una base de datos si se produce algún desastre. Elija una opción adecuada que se adapte a su empresa.

- Habilite las réplicas de lectura entre regiones para mejorar el planeamiento de la continuidad empresarial y recuperación ante desastres. Puede crear hasta cinco réplicas desde el servidor de origen. Las réplicas de lectura se actualizan de manera asincrónica mediante la tecnología de replicación de registros binarios de MySQL. Las réplicas son nuevos servidores que se administran de forma similar a los servidores normales de Azure Database for MySQL. Para más información, vea [Réplicas de lectura en Azure Database for MySQL](../../../mysql/concepts-read-replicas.md).

- Use la característica de restauración geográfica para restaurar el servidor mediante copias de seguridad con redundancia geográfica. Se puede acceder a estas copias de seguridad incluso cuando la región en la que se hospeda el servidor está sin conexión. Puede realizar la restauración a partir de estas copias de seguridad en cualquier otra región y volver a poner en línea el servidor.

  > [!NOTE]
  > La restauración geográfica solo es posible si se ha aprovisionado el servidor con almacenamiento de copia de seguridad con redundancia geográfica. No se admite el cambio de **Opciones de redundancia de copia de seguridad** después de la creación del servidor. Para más información, vea [Redundancia de copia de seguridad](../../../mysql/concepts-backup.md#backup-redundancy-options).

En la tabla siguiente se muestra la recomendación para la recuperación ante desastres de cada nivel usado en este ejemplo.

| Niveles de la plataforma de SAP BOBI   | Recomendación                                                                                           |
|---------------------------|----------------------------------------------------------------------------------------------------------|
| Azure Application Gateway | Configuración paralela de Application Gateway en una región secundaria.                                             |
| Servidores de aplicaciones web   | Replicación con Azure Site Recovery.                                                                  |
| Servidores de aplicaciones de BI    | Replicación mediante Site Recovery.                                                                        |
| Azure NetApp Files        | Herramienta de copia basada en archivos para replicar datos en una región secundaria o mediante la replicación entre regiones.      |
| Azure Database for MySQL  | Réplicas de lectura entre regiones o restauración de la copia de seguridad de copias de seguridad con redundancia geográfica.                                |

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de la recuperación ante desastres para la implementación de una aplicación de SAP de niveles múltiples](../../../site-recovery/site-recovery-sap.md)
- [Planeamiento e implementación de Azure Virtual Machines para SAP](planning-guide.md)
- [Implementación de Azure Virtual Machines para SAP](deployment-guide.md)
- [Implementación de DBMS de Azure Virtual Machines para SAP](./dbms_guide_general.md)
