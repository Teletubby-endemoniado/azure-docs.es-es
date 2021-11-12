---
title: Creación y carga de un VHD de Red Hat Enterprise Linux para su uso en Azure
description: Aprenda a crear y cargar un disco duro virtual (VHD) de Azure que contiene un sistema operativo Red Hat Linux.
author: srijang
ms.service: virtual-machines
ms.subservice: redhat
ms.collection: linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: how-to
ms.date: 11/10/2021
ms.author: srijangupta
ms.openlocfilehash: 11a7931126d451b2fbeff301a337f15fd6aecbf3
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132301236"
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Preparación de una máquina virtual basada en Red Hat para Azure

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes 

En este artículo, aprenderá a preparar una máquina virtual de Red Hat Enterprise Linux (RHEL) para usarla en Azure. Las versiones de RHEL que se tratan en este artículo son 6.7 y 7.1. Los hipervisores de preparación que se tratan en este artículo son Hyper-V, máquina virtual basada en kernel (KVM) y VMware. Para más información sobre los requisitos para poder participar en el programa de acceso a la nube de Red Hat, visite el sitio [web de acceso a la nube de Red Hat](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) y [Ejecución de RHEL en Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure). Para ver cómo automatizar la creación de imágenes de RHEL, consulte [Azure Image Builder](../image-builder-overview.md).

## <a name="hyper-v-manager"></a>Administrador de Hyper-V

En esta sección se muestra cómo preparar una máquina virtual [RHEL 6](#rhel-6-using-hyper-v-manager), [RHEL 7](#rhel-7-using-hyper-v-manager) o [RHEL 8](#rhel-8-using-hyper-v-manager) mediante el administrador de Hyper-V.

### <a name="prerequisites"></a>Requisitos previos
En esta sección, se supone que ya obtuvo un archivo ISO en el sitio web de Red Hat y que ha instalado la imagen RHEL en un disco duro virtual (VHD). Para más información acerca de cómo usar el Administrador de Hyper-V para instalar una imagen de sistema operativo, vea [Instalar Hyper-V y crear una máquina virtual](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh846766(v=ws.11)).

**Notas de instalación de RHEL**

* Azure no admite el formato VHDX. Azure solo admite VHD fijo. Puede usar el Administrador de Hyper-V para convertir el disco al formato VHD, o puede usar el cmdlet convert-vhd. Si usa VirtualBox, seleccione **Tamaño fijo** a diferencia de la opción predeterminada asignada dinámicamente al crear el disco.
* Azure admite máquinas virtuales de Gen1 (arranque del BIOS) y Gen2 (arranque UEFI).
* El tamaño máximo permitido para los discos duros virtuales es de 1023 GB.
* El Administrador de volúmenes lógicos (LVM) se admite y puede usarse en el disco del sistema operativo o discos de datos en máquinas virtuales de Azure. Sin embargo, en general se recomienda usar las particiones estándar en el disco del sistema operativo en lugar de LVM. Esta práctica evitará los conflictos de nombres LVM con máquinas virtuales clonadas, especialmente si alguna vez necesita conectar un disco de sistema operativo a otra máquina virtual idéntica para solucionar el problema. Consulte también la documentación de [LVM](/previous-versions/azure/virtual-machines/linux/configure-lvm) y [RAID](/previous-versions/azure/virtual-machines/linux/configure-raid).
* **Se requiere la compatibilidad de kernel para el montaje de sistemas de archivos de formato de disco universal (UDF)** . Al arrancar Azure la primera vez, los medios con formato UDF conectados al invitado pasan la configuración de aprovisionamiento a la máquina virtual Linux. El agente Linux de Azure debe poder montar el sistema de archivos UDF para leer su configuración y aprovisionar la máquina virtual ya que, sin esto, se producirá un error en el aprovisionamiento.
* No configure una partición de intercambio en el disco del sistema operativo. Puede encontrar más información al respecto en los pasos siguientes.

* En Azure, todos los discos duros virtuales deben tener un tamaño virtual alineado con 1 MB. Al convertir un disco sin procesar en un disco duro virtual, tiene que asegurarse de que su tamaño es un múltiplo de 1 MB antes de la conversión. Se puede encontrar más información en los siguientes pasos. Para más información, consulte también [Notas sobre la instalación de Linux](create-upload-generic.md#general-linux-installation-notes).

### <a name="rhel-6-using-hyper-v-manager"></a>Uso del administrador de Hyper-V en RHEL 6

1. En el Administrador de Hyper-V, seleccione la máquina virtual.

1. Haga clic en **Conectar** para abrir una ventana de consola de la máquina virtual.

1. En RHEL 6, NetworkManager puede interferir en el Agente de Linux de Azure. Desinstale el paquete ejecutando el comando siguiente:

    ```console
    # sudo rpm -e --nodeps NetworkManager
    ```

1. Cree o edite el archivo `/etc/sysconfig/network` y agregue el siguiente texto:

    ```config
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Cree o edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` y agregue el siguiente texto:

    ```config
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    ```

1. Mueva (o elimine) las reglas udev para impedir que se generen reglas estáticas para la interfaz Ethernet. Estas reglas pueden causar problemas cuando clone una máquina virtual en Microsoft Azure o Hyper-V:

    ```console
    # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

    # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
    ```

1. Asegúrese de que el servicio de red se inicia en el arranque ejecutando el comando siguiente:

    ```console
    # sudo chkconfig network on
    ```

1. Registre la suscripción de Red Hat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```console
    # sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. El paquete WALinuxAgent `WALinuxAgent-<version>` se ha insertado en el repositorio de extras de Red Hat. Habilite el repositorio de extras ejecutando el comando siguiente:

    ```console
    # subscription-manager repos --enable=rhel-6-server-extras-rpms
    ```

1. Modifique la línea de arranque de kernel de su configuración grub para que incluya parámetros de kernel adicionales para Azure. Para realizar esta modificación, abra `/boot/grub/menu.lst` en un editor de texto y asegúrese de que el kernel predeterminado incluye los parámetros siguientes:

    ```config-grub
    console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    ```

    Así también se asegurará de que todos los mensajes de la consola se envían al primer puerto serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración de errores.
    
    Además, se recomienda quitar los parámetros siguientes:

    ```config-grub
    rhgb quiet crashkernel=auto
    ```
    
    Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde queremos que todos los registros se envíen al puerto serie.  Puede dejar la opción `crashkernel` configurada si lo desea. Tenga en cuenta que este parámetro reduce la cantidad de memoria disponible en la máquina virtual unos 128 MB o más. Esta configuración podría resultar problemática en tamaños de máquinas virtuales más pequeños.


1. Asegúrese de que el servidor Secure Shell (SSH) está instalado y configurado para iniciarse en el tiempo de arranque, que suele ser el predeterminado. Modifique /etc/ssh/sshd_config para que incluya la siguiente línea:

    ```config
    ClientAliveInterval 180
    ```

1. Instale el Agente de Linux de Azure ejecutando el comando siguiente:

    ```console
    # sudo yum install WALinuxAgent

    # sudo chkconfig waagent on
    ```

    La instalación del paquete WALinuxAgent elimina los paquetes NetworkManager y NetworkManager-gnome, si es que aún no se han quitado en el paso 3.

1. No cree espacio de intercambio en el disco del sistema operativo.

    El Agente de Linux de Azure puede configurar automáticamente un espacio de intercambio usando el disco de recursos local que se adjunta a la máquina virtual, después de que la máquina virtual se aprovisione en Azure. Tenga en cuenta que el disco de recursos local es un disco temporal que debe vaciarse si la máquina virtual se desaprovisiona. Después de instalar el agente de Linux de Azure en el paso anterior, modifique apropiadamente los parámetros siguientes en /etc/waagent.conf:

    ```config-cong
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
    ```

1. Para anular el registro de la suscripción (si es necesario), ejecute el siguiente comando:

    ```console
    # sudo subscription-manager unregister
    ```

1. Ejecute los comandos siguientes para desaprovisionar la máquina virtual y prepararla para aprovisionarse en Azure:

    ```console
    # Note: if you are migrating a specific virtual machine and do not wish to create a generalized image,
    # skip the deprovision step
    # sudo waagent -force -deprovision

    # export HISTSIZE=0

    # logout
    ```

1. Haga clic en **Acción**  >  **Apagar** en el Administrador de Hyper-V. El VHD de Linux ya está listo para cargarse en Azure.


### <a name="rhel-7-using-hyper-v-manager"></a>Uso del administrador de Hyper-V en RHEL 7

1. En el Administrador de Hyper-V, seleccione la máquina virtual.

1. Haga clic en **Conectar** para abrir una ventana de consola de la máquina virtual.

1. Cree o edite el archivo `/etc/sysconfig/network` y agregue el siguiente texto:

    ```config
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Cree o edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` y agregue el siguiente texto:

    ```config
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    PERSISTENT_DHCLIENT=yes
    NM_CONTROLLED=yes
    ```

1. Asegúrese de que el servicio de red se inicia en el arranque ejecutando el comando siguiente:

    ```console
    # sudo systemctl enable network
    ```

1. Registre la suscripción de Red Hat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```console
    # sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Modifique la línea de arranque de kernel de su configuración grub para que incluya parámetros de kernel adicionales para Azure. Para realizar esta modificación, abra `/etc/default/grub` en un editor de texto y edite el parámetro `GRUB_CMDLINE_LINUX`. Por ejemplo:

    
    ```config-grub
    GRUB_CMDLINE_LINUX="rootdelay=300 console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 earlyprintk=ttyS0 net.ifnames=0"
    GRUB_TERMINAL_OUTPUT="serial console"
    GRUB_SERIAL_COMMAND="serial --speed=115200 --unit=0 --word=8 --parity=no --stop=1
    ```
   
    Esta acción también garantiza que todos los mensajes de la consola se envíen al primer puerto serie y se permita la interacción con la consola serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración. Esta configuración también desactiva las nuevas convenciones de nomenclatura de RHEL 7 para NIC.

    ```config
    rhgb quiet crashkernel=auto
    ```
   
    Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde queremos que todos los registros se envíen al puerto serie. Puede dejar la opción `crashkernel` configurada si lo desea. Tenga en cuenta que este parámetro reduce la cantidad de memoria disponible en la máquina virtual mediante 128 MB o más, lo que podría resultar problemática en tamaños de máquina virtual más pequeños.

1. Una vez que termine de editar `/etc/default/grub`, ejecute el comando siguiente para recompilar la configuración de GRUB:

    ```console
    # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```
    > [!NOTE]
    > Si va a cargar una máquina virtual habilitada para UEFI, el comando para actualizar grub es `grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg`.

1. Asegúrese de que el servidor SSH se haya instalado y configurado para iniciarse en el tiempo de arranque, que suele ser el predeterminado. Modifique `/etc/ssh/sshd_config` para que incluya la siguiente línea:

    ```config
    ClientAliveInterval 180
    ```

1. El paquete WALinuxAgent `WALinuxAgent-<version>` se ha insertado en el repositorio de extras de Red Hat. Habilite el repositorio de extras ejecutando el comando siguiente:

    ```console
    # subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Ejecute el comando siguiente para instalar el agente de Linux de Azure, cloud-init y otras utilidades necesarias:

    ```console
    # sudo yum install -y WALinuxAgent cloud-init cloud-utils-growpart gdisk hyperv-daemons

    # sudo systemctl enable waagent.service
    # sudo systemctl enable cloud-init.service
    ```

1. Configure cloud-init para administrar el aprovisionamiento:

    1. Configure waagent para cloud-init:

    ```console
    sed -i 's/Provisioning.Agent=auto/Provisioning.Agent=cloud-init/g' /etc/waagent.conf
    sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
    sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
    ```
    > [!NOTE]
    > Si va a migrar una máquina virtual concreta y no quiere crear una imagen generalizada, establezca `Provisioning.Agent=disabled` en la configuración de `/etc/waagent.conf`.
    
    1. Configure montajes:

    ```console
    echo "Adding mounts and disk_setup to init stage"
    sed -i '/ - mounts/d' /etc/cloud/cloud.cfg
    sed -i '/ - disk_setup/d' /etc/cloud/cloud.cfg
    sed -i '/cloud_init_modules/a\\ - mounts' /etc/cloud/cloud.cfg
    sed -i '/cloud_init_modules/a\\ - disk_setup' /etc/cloud/cloud.cfg
    ```
    
    1. Configure el origen de datos de Azure:

    ```console
    echo "Allow only Azure datasource, disable fetching network setting via IMDS"
    cat > /etc/cloud/cloud.cfg.d/91-azure_datasource.cfg <<EOF
    datasource_list: [ Azure ]
    datasource:
        Azure:
            apply_network_config: False
    EOF
    ```

    1. Si está configurado, quite el archivo de intercambio existente:

    ```console
    if [[ -f /mnt/resource/swapfile ]]; then
    echo "Removing swapfile" #RHEL uses a swapfile by defaul
    swapoff /mnt/resource/swapfile
    rm /mnt/resource/swapfile -f
    fi
    ```
    1. Configure el registro de cloud-init:
    ```console
    echo "Add console log file"
    cat >> /etc/cloud/cloud.cfg.d/05_logging.cfg <<EOF

    # This tells cloud-init to redirect its stdout and stderr to
    # 'tee -a /var/log/cloud-init-output.log' so the user can see output
    # there without needing to look on the console.
    output: {all: '| tee -a /var/log/cloud-init-output.log'}
    EOF

    ```

1. Configuración de intercambio No cree espacio de intercambio en el disco del sistema operativo.

    Anteriormente, el agente de Linux de Azure se usaba para configurar automáticamente el espacio de intercambio mediante el disco de recursos local que se asociaba a la máquina virtual después de que esta se aprovisionara en Azure. Ahora, en cambio, esto se administra mediante cloud-init; **no tiene** que usar el agente de Linux para formatear el disco de recursos, crear el archivo de intercambio y modificar los parámetros siguientes en `/etc/waagent.conf` adecuadamente:

    ```console
    ResourceDisk.Format=n
    ResourceDisk.EnableSwap=n
    ```

    Si quiere montar, formatear y crear un intercambio, puede:
    * Pasarlo como una configuración de cloud-init cada vez que cree una máquina virtual con customdata. Éste es el método recomendado.
    * Usar una directiva de cloud-init preparada en la imagen que hará esto cada vez que se cree la máquina virtual.

        ```console
        echo 'DefaultEnvironment="CLOUD_CFG=/etc/cloud/cloud.cfg.d/00-azure-swap.cfg"' >> /etc/systemd/system.conf
        cat > /etc/cloud/cloud.cfg.d/00-azure-swap.cfg << EOF
        #cloud-config
        # Generated by Azure cloud image build
        disk_setup:
          ephemeral0:
            table_type: mbr
            layout: [66, [33, 82]]
            overwrite: True
        fs_setup:
          - device: ephemeral0.1
            filesystem: ext4
          - device: ephemeral0.2
            filesystem: swap
        mounts:
          - ["ephemeral0.1", "/mnt"]
          - ["ephemeral0.2", "none", "swap", "sw,nofail,x-systemd.requires=cloud-init.service", "0", "0"]
        EOF
        ```
1. Si quiere anular el registro de la suscripción, ejecute el siguiente comando:

    ```console
    # sudo subscription-manager unregister
    ```

1. Desaprovisionamiento

    Ejecute los comandos siguientes para desaprovisionar la máquina virtual y prepararla para aprovisionarse en Azure:

    > [!CAUTION]
    > Si va a migrar una máquina virtual concreta y no desea crear una imagen generalizada, omita el paso de desaprovisionamiento. Al ejecutar el comando `waagent -force -deprovision+user`, la máquina de origen no se podrá usar; este paso está pensado únicamente para crear una imagen generalizada.
    ```console
    # sudo rm -f /var/log/waagent.log
    # sudo cloud-init clean
    # waagent -force -deprovision+user
    # rm -f ~/.bash_history
    # export HISTSIZE=0
    # logout
    ```
    

1. Haga clic en **Acción**  >  **Apagar** en el Administrador de Hyper-V. El VHD de Linux ya está listo para cargarse en Azure.

### <a name="rhel-8-using-hyper-v-manager"></a>Administrador de Hyper-V en RHEL 8

1. En el Administrador de Hyper-V, seleccione la máquina virtual.

1. Haga clic en **Conectar** para abrir una ventana de consola de la máquina virtual.

1. Ejecute el siguiente comando para asegurarse de que el servicio Administrador de red se inicia en el arranque:

    ```console
    # sudo systemctl enable NetworkManager.service
    ```

1. Configure la interfaz de red para que se inicie automáticamente en el arranque y use DHCP:

    ```console
    # nmcli con mod eth0 connection.autoconnect yes ipv4.method auto
    ```


1. Registre la suscripción de Red Hat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```console
    # sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Modifique la línea de arranque del kernel de su configuración de grub para que incluya parámetros del kernel adicionales para Azure y habilite la consola serie. 

    1. Quitar los parámetros actuales de GRUB:
    ```console
    # grub2-editenv - unset kernelopts
    ```

    1. Edite `/etc/default/grub` en un editor de texto y agregue los parámetros siguientes:

    ```config-grub
    GRUB_CMDLINE_LINUX="rootdelay=300 console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 earlyprintk=ttyS0 net.ifnames=0"
    GRUB_TERMINAL_OUTPUT="serial console"
    GRUB_SERIAL_COMMAND="serial --speed=115200 --unit=0 --word=8 --parity=no --stop=1"
    ```
   
   Esta acción también garantiza que todos los mensajes de la consola se envíen al primer puerto serie y se permita la interacción con la consola serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración. Esta configuración también desactiva las nuevas convenciones de nomenclatura para NIC.
   
   1. Además, se recomienda quitar los parámetros siguientes:

    ```config
    rhgb quiet crashkernel=auto
    ```
   
    Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde queremos que todos los registros se envíen al puerto serie. Puede dejar la opción `crashkernel` configurada si lo desea. Tenga en cuenta que este parámetro reduce la cantidad de memoria disponible en la máquina virtual mediante 128 MB o más, lo que podría resultar problemática en tamaños de máquina virtual más pequeños.

1. Una vez que termine de editar `/etc/default/grub`, ejecute el comando siguiente para recompilar la configuración de GRUB:

    ```console
    # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```
    Y, para una máquina virtual habilitada para UEFI, ejecute el siguiente comando:

    ```console
    # sudo grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg
    ```

1. Asegúrese de que el servidor SSH se haya instalado y configurado para iniciarse en el tiempo de arranque, que suele ser el predeterminado. Modifique `/etc/ssh/sshd_config` para que incluya la siguiente línea:

    ```config
    ClientAliveInterval 180
    ```

1. Ejecute el comando siguiente para instalar el agente de Linux de Azure, cloud-init y otras utilidades necesarias:

    ```console
    # sudo yum install -y WALinuxAgent cloud-init cloud-utils-growpart gdisk hyperv-daemons

    # sudo systemctl enable waagent.service
    # sudo systemctl enable cloud-init.service
    ```

1. Configure cloud-init para administrar el aprovisionamiento:

    1. Configure waagent para cloud-init:

    ```console
    sed -i 's/Provisioning.Agent=auto/Provisioning.Agent=cloud-init/g' /etc/waagent.conf
    sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
    sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
    ```
    > [!NOTE]
    > Si va a migrar una máquina virtual concreta y no quiere crear una imagen generalizada, establezca `Provisioning.Agent=disabled` en la configuración de `/etc/waagent.conf`.
    
    1. Configure montajes:

    ```console
    echo "Adding mounts and disk_setup to init stage"
    sed -i '/ - mounts/d' /etc/cloud/cloud.cfg
    sed -i '/ - disk_setup/d' /etc/cloud/cloud.cfg
    sed -i '/cloud_init_modules/a\\ - mounts' /etc/cloud/cloud.cfg
    sed -i '/cloud_init_modules/a\\ - disk_setup' /etc/cloud/cloud.cfg
    ```
    
    1. Configure el origen de datos de Azure:

    ```console
    echo "Allow only Azure datasource, disable fetching network setting via IMDS"
    cat > /etc/cloud/cloud.cfg.d/91-azure_datasource.cfg <<EOF
    datasource_list: [ Azure ]
    datasource:
        Azure:
            apply_network_config: False
    EOF
    ```

    1. Si está configurado, quite el archivo de intercambio existente:

    ```console
    if [[ -f /mnt/resource/swapfile ]]; then
    echo "Removing swapfile" #RHEL uses a swapfile by defaul
    swapoff /mnt/resource/swapfile
    rm /mnt/resource/swapfile -f
    fi
    ```
    1. Configure el registro de cloud-init:
    ```console
    echo "Add console log file"
    cat >> /etc/cloud/cloud.cfg.d/05_logging.cfg <<EOF

    # This tells cloud-init to redirect its stdout and stderr to
    # 'tee -a /var/log/cloud-init-output.log' so the user can see output
    # there without needing to look on the console.
    output: {all: '| tee -a /var/log/cloud-init-output.log'}
    EOF

    ```

1. Configuración de intercambio No cree espacio de intercambio en el disco del sistema operativo.

    Anteriormente, el agente de Linux de Azure se usaba para configurar automáticamente el espacio de intercambio mediante el disco de recursos local que se asociaba a la máquina virtual después de que esta se aprovisionara en Azure. Ahora, en cambio, esto se administra mediante cloud-init; **no tiene** que usar el agente de Linux para formatear el disco de recursos, crear el archivo de intercambio y modificar los parámetros siguientes en `/etc/waagent.conf` adecuadamente:

    ```console
    ResourceDisk.Format=n
    ResourceDisk.EnableSwap=n
    ```

    * Pasarlo como una configuración de cloud-init cada vez que cree una máquina virtual con customdata. Éste es el método recomendado.
    * Usar una directiva de cloud-init preparada en la imagen que hará esto cada vez que se cree la máquina virtual.

        ```console
        echo 'DefaultEnvironment="CLOUD_CFG=/etc/cloud/cloud.cfg.d/00-azure-swap.cfg"' >> /etc/systemd/system.conf
        cat > /etc/cloud/cloud.cfg.d/00-azure-swap.cfg << EOF
        #cloud-config
        # Generated by Azure cloud image build
        disk_setup:
          ephemeral0:
            table_type: mbr
            layout: [66, [33, 82]]
            overwrite: True
        fs_setup:
          - device: ephemeral0.1
            filesystem: ext4
          - device: ephemeral0.2
            filesystem: swap
        mounts:
          - ["ephemeral0.1", "/mnt"]
          - ["ephemeral0.2", "none", "swap", "sw,nofail,x-systemd.requires=cloud-init.service", "0", "0"]
        EOF
        ```
1. Si quiere anular el registro de la suscripción, ejecute el siguiente comando:

    ```console
    # sudo subscription-manager unregister
    ```

1. Desaprovisionamiento

    Ejecute los comandos siguientes para desaprovisionar la máquina virtual y prepararla para aprovisionarse en Azure:

    ```console
    # sudo cloud-init clean
    # waagent -force -deprovision+user
    # rm -f ~/.bash_history
    # sudo rm -f /var/log/waagent.log
    # export HISTSIZE=0
    # logout
    ```
    > [!CAUTION]
    > Si va a migrar una máquina virtual concreta y no desea crear una imagen generalizada, omita el paso de desaprovisionamiento. Al ejecutar el comando `waagent -force -deprovision+user`, la máquina de origen no se podrá usar; este paso está pensado únicamente para crear una imagen generalizada.


1. Haga clic en **Acción**  >  **Apagar** en el Administrador de Hyper-V. El VHD de Linux ya está listo para cargarse en Azure.


## <a name="kvm"></a>KVM

En esta sección se muestra cómo usar KVM para preparar una distribución de [RHEL 6](#rhel-6-using-kvm) o [RHEL 7](#rhel-7-using-kvm) para cargar en Azure. 

### <a name="rhel-6-using-kvm"></a>Uso de KVM en RHEL 6

1. Descargue la imagen KVM de RHEL 6 desde el sitio web de Red Hat.

1. Establezca una contraseña raíz.

    Genere una contraseña cifrada y copie el resultado del comando:

    ```console
    # openssl passwd -1 changeme
    ```

    Establezca una contraseña raíz con guestfish:

    ```console
    # guestfish --rw -a <image-name>
    > <fs> run
    > <fs> list-filesystems
    > <fs> mount /dev/sda1 /
    > <fs> vi /etc/shadow
    > <fs> exit
    ```

   Cambie el segundo campo del usuario raíz de "!!" a la contraseña cifrada.

1. Cree una máquina virtual en KVM a partir de la imagen qcow2. Establezca el tipo de disco en **qcow2** y el modelo de dispositivo de interfaz de red virtual en **virtio**. Después, inicie la máquina virtual e inicie sesión como raíz.

1. Cree o edite el archivo `/etc/sysconfig/network` y agregue el siguiente texto:

    ```config
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Cree o edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` y agregue el siguiente texto:

    ```config
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    ```

1. Mueva (o elimine) las reglas udev para impedir que se generen reglas estáticas para la interfaz Ethernet. Estas reglas pueden causar problemas cuando clone una máquina virtual en Azure o Hyper-V:

    ```console
    # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

    # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
    ```

1. Asegúrese de que el servicio de red se inicia en el arranque ejecutando el comando siguiente:

    ```console
    # chkconfig network on
    ```

1. Registre la suscripción de Red Hat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```console
    # subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Modifique la línea de arranque de kernel de su configuración grub para que incluya parámetros de kernel adicionales para Azure. Para realizar esta configuración, abra `/boot/grub/menu.lst` en un editor de texto y asegúrese de que el kernel predeterminado incluye los parámetros siguientes:

    ```config-grub
    console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    ```

    Así también se asegurará de que todos los mensajes de la consola se envían al primer puerto serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración de errores.
    
    Además, se recomienda quitar los parámetros siguientes:

    ```config-grub
    rhgb quiet crashkernel=auto
    ```

    Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde queremos que todos los registros se envíen al puerto serie. Puede dejar la opción `crashkernel` configurada si lo desea. Tenga en cuenta que este parámetro reduce la cantidad de memoria disponible en la máquina virtual mediante 128 MB o más, lo que podría resultar problemática en tamaños de máquina virtual más pequeños.


1. Agregue módulos de Hyper-V a initramfs:  

    Edite `/etc/dracut.conf` y agregue el siguiente contenido:

    ```config-conf
    add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "
    ```

    Recompile initramfs:

    ```config-conf
    # dracut -f -v
    ```

1. Desinstale cloud-init:

    ```console
    # yum remove cloud-init
    ```

1. Asegúrese de que el servidor SSH esté instalado y configurado para iniciarse en el tiempo de arranque:

    ```console
    # chkconfig sshd on
    ```

    Modifique /etc/ssh/sshd_config para que incluya las siguientes líneas:

    ```config
    PasswordAuthentication yes
    ClientAliveInterval 180
    ```

1. El paquete WALinuxAgent `WALinuxAgent-<version>` se ha insertado en el repositorio de extras de Red Hat. Habilite el repositorio de extras ejecutando el comando siguiente:

    ```console
    # subscription-manager repos --enable=rhel-6-server-extras-rpms
    ```

1. Instale el Agente de Linux de Azure ejecutando el comando siguiente:

    ```console
    # yum install WALinuxAgent

    # chkconfig waagent on
    ```

1. El Agente de Linux de Azure puede configurar automáticamente un espacio de intercambio usando el disco de recursos local que se adjunta a la máquina virtual, después de que la máquina virtual se aprovisione en Azure. Tenga en cuenta que el disco de recursos local es un disco temporal que debe vaciarse si la máquina virtual se desaprovisiona. Después de instalar el agente de Linux de Azure en el paso anterior, modifique apropiadamente los parámetros siguientes en **/etc/waagent.conf**:

    ```config-conf
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
    ```

1. Para anular el registro de la suscripción (si es necesario), ejecute el siguiente comando:

    ```console-conf
    # subscription-manager unregister
    ```

1. Ejecute los comandos siguientes para desaprovisionar la máquina virtual y prepararla para aprovisionarse en Azure:

    ```console
    # Note: if you are migrating a specific virtual machine and do not wish to create a generalized image,
    # skip the deprovision step
    # sudo rm -rf /var/lib/waagent/
    # sudo rm -f /var/log/waagent.log

    # waagent -force -deprovision+user
    # rm -f ~/.bash_history
    

    # export HISTSIZE=0

    # logout
    ```

1. Apague la máquina virtual en KVM.

1. Convierta la imagen qcow2 al formato VHD.

    > [!NOTE]
    > Hay un problema conocido en versiones de qemu-img >= 2.2.1 que da como resultado un VHD con formato incorrecto. El problema se corrigió en QEMU 2.6. Se recomienda usar qemu-img 2.2.0 o anterior, o bien actualice a la versión 2.6 o posterior. Referencia: https://bugs.launchpad.net/qemu/+bug/1490611.
    >

    Primero convierta la imagen al formato raw:

    ```console
    # qemu-img convert -f qcow2 -O raw rhel-6.9.qcow2 rhel-6.9.raw
    ```

    Asegúrese de que el tamaño de la imagen sin procesar está alineado con 1 MB. De lo contrario, redondee hacia arriba el tamaño para alinear con 1 MB:

    ```console
    # MB=$((1024*1024))
    # size=$(qemu-img info -f raw --output json "rhel-6.9.raw" | \
      gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

    # rounded_size=$((($size/$MB + 1)*$MB))
    # qemu-img resize rhel-6.9.raw $rounded_size
    ```

    Convierta el disco sin procesar en un disco duro virtual (VHD) de tamaño fijo:

    ```console
    # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.9.raw rhel-6.9.vhd
    ```

    O bien, con la versión de qemu **2.6 o posterior**, incluya la opción `force_size`:

    ```console
    # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-6.9.raw rhel-6.9.vhd
    ```

        
### <a name="rhel-7-using-kvm"></a>RHEL 7 mediante KVM

1. Descargue la imagen KVM de RHEL 7 desde el sitio web de Red Hat. Este procedimiento utiliza RHEL 7 como el ejemplo.

1. Establezca una contraseña raíz.

    Genere una contraseña cifrada y copie el resultado del comando:

    ```console
    # openssl passwd -1 changeme
    ```

    Establezca una contraseña raíz con guestfish:

    ```console
    # guestfish --rw -a <image-name>
    > <fs> run
    > <fs> list-filesystems
    > <fs> mount /dev/sda1 /
    > <fs> vi /etc/shadow
    > <fs> exit
    ```

   Cambie el segundo campo de usuario raíz de "!!" a la contraseña cifrada.

1. Cree una máquina virtual en KVM a partir de la imagen qcow2. Establezca el tipo de disco en **qcow2** y el modelo de dispositivo de interfaz de red virtual en **virtio**. Después, inicie la máquina virtual e inicie sesión como raíz.

1. Cree o edite el archivo `/etc/sysconfig/network` y agregue el siguiente texto:

    ```config
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Cree o edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` y agregue el siguiente texto:

    ```config
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    PERSISTENT_DHCLIENT=yes
    NM_CONTROLLED=yes
    ```

1. Asegúrese de que el servicio de red se inicia en el arranque ejecutando el comando siguiente:

    ```console
    # sudo systemctl enable network
    ```

1. Registre la suscripción de RedHat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```console
    # subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Modifique la línea de arranque de kernel de su configuración grub para que incluya parámetros de kernel adicionales para Azure. Para realizar esta configuración, abra `/etc/default/grub` en un editor de texto y edite el parámetro `GRUB_CMDLINE_LINUX`. Por ejemplo:

    ```config-grub
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Este comando también asegura que todos los mensajes de la consola se envían al primer puerto serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración de errores. El comando también desactiva las nuevas convenciones de nomenclatura de RHEL 7 para NIC. Además, se recomienda quitar los parámetros siguientes:

    ```config-grub
    rhgb quiet crashkernel=auto
    ```

    Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde queremos que todos los registros se envíen al puerto serie. Puede dejar la opción `crashkernel` configurada si lo desea. Tenga en cuenta que este parámetro reduce la cantidad de memoria disponible en la máquina virtual mediante 128 MB o más, lo que podría resultar problemática en tamaños de máquina virtual más pequeños.

1. Una vez que termine de editar `/etc/default/grub`, ejecute el comando siguiente para recompilar la configuración de GRUB:

    ```console
    # grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Agregue módulos de Hyper-V a initramfs.

    Edite `/etc/dracut.conf` y agregue contenido:

    ```config-conf
    add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "
    ```

    Recompile initramfs:

    ```config-conf
    # dracut -f -v
    ```

1. Desinstale cloud-init:

    ```console
    # yum remove cloud-init
    ```

1. Asegúrese de que el servidor SSH esté instalado y configurado para iniciarse en el tiempo de arranque:

    ```console
    # systemctl enable sshd
    ```

    Modifique /etc/ssh/sshd_config para que incluya las siguientes líneas:

    ```config
    PasswordAuthentication yes
    ClientAliveInterval 180
    ```

1. El paquete WALinuxAgent `WALinuxAgent-<version>` se ha insertado en el repositorio de extras de Red Hat. Habilite el repositorio de extras ejecutando el comando siguiente:

    ```config
    # subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Instale el Agente de Linux de Azure ejecutando el comando siguiente:

    ```console
    # yum install WALinuxAgent
    ```

    Habilite el servicio waagent:

    ```console
    # systemctl enable waagent.service
    ```

1. Instale cloud-init. Siga los pasos descritos en "Preparar una máquina virtual RHEL 7 desde el Administrador de Hyper-V", en el paso 12, "Instalación de cloud-init para controlar el aprovisionamiento".

1. Configuración de intercambio 

    No cree espacio de intercambio en el disco del sistema operativo.
    Siga los pasos descritos en "Preparar una máquina virtual RHEL 7 desde el Administrador de Hyper-V", paso 13, "Configuración de intercambio".


1. Para anular el registro de la suscripción (si es necesario), ejecute el siguiente comando:

    ```console
    # subscription-manager unregister
    ```


1. Desaprovisionamiento

    Siga los pasos descritos en "Preparar una máquina virtual RHEL 7 desde el Administrador de Hyper-V", paso 15, "Desaprovisionamiento".

1. Apague la máquina virtual en KVM.

1. Convierta la imagen qcow2 al formato VHD.

    > [!NOTE]
    > Hay un problema conocido en versiones de qemu-img >= 2.2.1 que da como resultado un VHD con formato incorrecto. El problema se corrigió en QEMU 2.6. Se recomienda usar qemu-img 2.2.0 o anterior, o bien actualice a la versión 2.6 o posterior. Referencia: https://bugs.launchpad.net/qemu/+bug/1490611.
    >

    Primero convierta la imagen al formato raw:

    ```console
    # qemu-img convert -f qcow2 -O raw rhel-7.4.qcow2 rhel-7.4.raw
    ```

    Asegúrese de que el tamaño de la imagen sin procesar está alineado con 1 MB. De lo contrario, redondee hacia arriba el tamaño para alinear con 1 MB:

    ```console
    # MB=$((1024*1024))
    # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
      gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

    # rounded_size=$((($size/$MB + 1)*$MB))
    # qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Convierta el disco sin procesar en un disco duro virtual (VHD) de tamaño fijo:

    ```console
    # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    O bien, con la versión de qemu **2.6 o posterior**, incluya la opción `force_size`:

    ```console
    # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="vmware"></a>VMware

En esta sección se muestra cómo preparar una distribución de [RHEL 6](#rhel-6-using-vmware) o [RHEL 7](#rhel-6-using-vmware) desde VMware.

### <a name="prerequisites"></a>Requisitos previos
En esta sección se supone que ya instaló una máquina virtual RHEL en VMware. Para más información acerca de cómo instalar un sistema operativo en VMware, consulte [VMware Guest Operating System Installation Guide](https://partnerweb.vmware.com/GOSIG/home.html)(Guía de instalación de sistema operativo invitado de VMware).

* Al instalar el sistema Linux se recomienda usar las particiones estándar en lugar de un LVM, que a menudo viene de forma predeterminada en muchas instalaciones. De este modo se impedirá que el nombre del LVM entre en conflicto con las máquinas virtuales clonadas, especialmente si en algún momento hace falta adjuntar un disco de sistema operativo a otra máquina virtual para solucionar problemas. Para los discos de datos se puede utilizar LVM o RAID si así se prefiere.
* No configure una partición de intercambio en el disco del sistema operativo. Es posible configurar el agente de Linux para crear un archivo de intercambio en el disco de recursos temporal. Puede encontrar más información al respecto en los pasos a continuación.
* Cuando cree el disco duro virtual, seleccione **Almacenar disco virtual como un solo archivo**.

### <a name="rhel-6-using-vmware"></a>Uso de VMware en RHEL 6
1. En RHEL 6, NetworkManager puede interferir en el Agente de Linux de Azure. Desinstale el paquete ejecutando el comando siguiente:

    ```console
    # sudo rpm -e --nodeps NetworkManager
    ```

1. Cree un archivo llamado **network** en el directorio /etc/sysconfig/ que contenga el texto siguiente:

    ```console
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Cree o edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` y agregue el siguiente texto:

    ```config
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    ```

1. Mueva (o elimine) las reglas udev para impedir que se generen reglas estáticas para la interfaz Ethernet. Estas reglas pueden causar problemas cuando clone una máquina virtual en Azure o Hyper-V:

    ```console
    # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

    # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
    ```

1. Asegúrese de que el servicio de red se inicia en el arranque ejecutando el comando siguiente:

    ```console
    # sudo chkconfig network on
    ```

1. Registre la suscripción de Red Hat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```console
    # sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. El paquete WALinuxAgent `WALinuxAgent-<version>` se ha insertado en el repositorio de extras de Red Hat. Habilite el repositorio de extras ejecutando el comando siguiente:

    ```console
    # subscription-manager repos --enable=rhel-6-server-extras-rpms
    ```

1. Modifique la línea de arranque de kernel de su configuración grub para que incluya parámetros de kernel adicionales para Azure. Para ello, abra `/etc/default/grub` en un editor de texto y edite el parámetro `GRUB_CMDLINE_LINUX`. Por ejemplo:

    ```config-grub
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
    ```

   Así también se asegurará de que todos los mensajes de la consola se envían al primer puerto serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración de errores. Además, se recomienda quitar los parámetros siguientes:

    ```config-grub
    rhgb quiet crashkernel=auto
    ```
   
    Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde queremos que todos los registros se envíen al puerto serie. Puede dejar la opción `crashkernel` configurada si lo desea. Tenga en cuenta que este parámetro reduce la cantidad de memoria disponible en la máquina virtual mediante 128 MB o más, lo que podría resultar problemática en tamaños de máquina virtual más pequeños.

1. Agregue módulos de Hyper-V a initramfs:

    Edite `/etc/dracut.conf` y agregue el siguiente contenido:

    ```config-conf
    add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "
    ```

    Recompile initramfs:

    ```config-cong
    # dracut -f -v
    ```

1. Asegúrese de que el servidor SSH se haya instalado y configurado para iniciarse en el tiempo de arranque, que suele ser el predeterminado. Modifique `/etc/ssh/sshd_config` para que incluya la siguiente línea:

    ```config
    ClientAliveInterval 180
    ```

1. Instale el Agente de Linux de Azure ejecutando el comando siguiente:

    ```console
    # sudo yum install WALinuxAgent

    # sudo chkconfig waagent on
    ```

1. No cree espacio de intercambio en el disco del sistema operativo.

    El Agente de Linux de Azure puede configurar automáticamente un espacio de intercambio usando el disco de recursos local que se adjunta a la máquina virtual, después de que la máquina virtual se aprovisione en Azure. Tenga en cuenta que el disco de recursos local es un disco temporal que debe vaciarse si la máquina virtual se desaprovisiona. Después de instalar el agente de Linux de Azure en el paso anterior, modifique apropiadamente los parámetros siguientes en `/etc/waagent.conf`:

    ```config-conf
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.EnableSwap=y
    ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
    ```

1. Para anular el registro de la suscripción (si es necesario), ejecute el siguiente comando:

    ```console
    # sudo subscription-manager unregister
    ```

1. Ejecute los comandos siguientes para desaprovisionar la máquina virtual y prepararla para aprovisionarse en Azure:

    ```console
    # Note: if you are migrating a specific virtual machine and do not wish to create a generalized image,
    # skip the deprovision step
    # sudo rm -rf /var/lib/waagent/
    # sudo rm -f /var/log/waagent.log

    # waagent -force -deprovision+user
    # rm -f ~/.bash_history
    

    # export HISTSIZE=0

    # logout
    ```

1. Apague la máquina virtual y convierta el archivo VMDK en un archivo .vhd.

    > [!NOTE]
    > Hay un problema conocido en versiones de qemu-img >= 2.2.1 que da como resultado un VHD con formato incorrecto. El problema se corrigió en QEMU 2.6. Se recomienda usar qemu-img 2.2.0 o anterior, o bien actualice a la versión 2.6 o posterior. Referencia: https://bugs.launchpad.net/qemu/+bug/1490611.
    >

    Primero convierta la imagen al formato raw:

    ```console
    # qemu-img convert -f vmdk -O raw rhel-6.9.vmdk rhel-6.9.raw
    ```

    Asegúrese de que el tamaño de la imagen sin procesar está alineado con 1 MB. De lo contrario, redondee hacia arriba el tamaño para alinear con 1 MB:

    ```console
    # MB=$((1024*1024))
    # size=$(qemu-img info -f raw --output json "rhel-6.9.raw" | \
      gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

    # rounded_size=$((($size/$MB + 1)*$MB))
    # qemu-img resize rhel-6.9.raw $rounded_size
    ```

    Convierta el disco sin procesar en un disco duro virtual (VHD) de tamaño fijo:

    ```console
    # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.9.raw rhel-6.9.vhd
    ```

    O bien, con la versión de qemu **2.6 o posterior**, incluya la opción `force_size`:

    ```console
    # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-6.9.raw rhel-6.9.vhd
    ```

### <a name="rhel-7-using-vmware"></a>Uso de VMware en RHEL 7
1. Cree o edite el archivo `/etc/sysconfig/network` y agregue el siguiente texto:

    ```config
    NETWORKING=yes
    HOSTNAME=localhost.localdomain
    ```

1. Cree o edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` y agregue el siguiente texto:

    ```config
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    PERSISTENT_DHCLIENT=yes
    NM_CONTROLLED=yes
    ```

1. Asegúrese de que el servicio de red se inicia en el arranque ejecutando el comando siguiente:

    ```console
    # sudo systemctl enable network
    ```

1. Registre la suscripción de Red Hat para habilitar la instalación de paquetes desde el repositorio RHEL ejecutando el siguiente comando:

    ```console
    # sudo subscription-manager register --auto-attach --username=XXX --password=XXX
    ```

1. Modifique la línea de arranque de kernel de su configuración grub para que incluya parámetros de kernel adicionales para Azure. Para realizar esta modificación, abra `/etc/default/grub` en un editor de texto y edite el parámetro `GRUB_CMDLINE_LINUX`. Por ejemplo:

    ```config-grub
    GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
    ```

   Esta configuración también asegura que todos los mensajes de la consola se envían al primer puerto serie, lo que puede ayudar al soporte técnico de Azure con los problemas de depuración de errores. Esto también desactiva las nuevas convenciones de nomenclatura de RHEL 7 para NIC. Además, se recomienda quitar los parámetros siguientes:

    ```config-grub
    rhgb quiet crashkernel=auto
    ```

    Los arranques gráfico y silencioso no resultan útiles en un entorno de nube, donde queremos que todos los registros se envíen al puerto serie. Puede dejar la opción `crashkernel` configurada si lo desea. Tenga en cuenta que este parámetro reduce la cantidad de memoria disponible en la máquina virtual mediante 128 MB o más, lo que podría resultar problemática en tamaños de máquina virtual más pequeños.

1. Una vez que termine de editar `/etc/default/grub`, ejecute el comando siguiente para recompilar la configuración de GRUB:

    ```console
    # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
    ```

1. Agregue módulos de Hyper-V a initramfs.

    Edite `/etc/dracut.conf`y agregue contenido:

    ```config-conf
    add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "
    ```

    Recompile initramfs:

    ```console
    # dracut -f -v
    ```

1. Asegúrese de que el servidor SSH se haya instalado y configurado para iniciarse en el tiempo de arranque. Esta configuración es normalmente el valor predeterminado. Modifique `/etc/ssh/sshd_config` para que incluya la siguiente línea:

    ```config
    ClientAliveInterval 180
    ```

1. El paquete WALinuxAgent `WALinuxAgent-<version>` se ha insertado en el repositorio de extras de Red Hat. Habilite el repositorio de extras ejecutando el comando siguiente:

    ```console
    # subscription-manager repos --enable=rhel-7-server-extras-rpms
    ```

1. Instale el Agente de Linux de Azure ejecutando el comando siguiente:

    ```console
    # sudo yum install WALinuxAgent

    # sudo systemctl enable waagent.service
    ```

1. Instale cloud-init.

    Siga los pasos descritos en "Preparar una máquina virtual RHEL 7 desde el Administrador de Hyper-V", en el paso 12, "Instalación de cloud-init para controlar el aprovisionamiento".

1. Configuración de intercambio

    No cree espacio de intercambio en el disco del sistema operativo.
    Siga los pasos descritos en "Preparar una máquina virtual RHEL 7 desde el Administrador de Hyper-V", paso 13, "Configuración de intercambio".

1. Si quiere anular el registro de la suscripción, ejecute el siguiente comando:

    ```console
    # sudo subscription-manager unregister
    ```

1. Desaprovisionamiento

    Siga los pasos descritos en "Preparar una máquina virtual RHEL 7 desde el Administrador de Hyper-V", paso 15, "Desaprovisionamiento".


1. Apague la máquina virtual y convierta el archivo VMDK al formato VHD.

    > [!NOTE]
    > Hay un problema conocido en versiones de qemu-img >= 2.2.1 que da como resultado un VHD con formato incorrecto. El problema se corrigió en QEMU 2.6. Se recomienda usar qemu-img 2.2.0 o anterior, o bien actualice a la versión 2.6 o posterior. Referencia: https://bugs.launchpad.net/qemu/+bug/1490611.
    >

    Primero convierta la imagen al formato raw:

    ```console
    # qemu-img convert -f vmdk -O raw rhel-7.4.vmdk rhel-7.4.raw
    ```

    Asegúrese de que el tamaño de la imagen sin procesar está alineado con 1 MB. De lo contrario, redondee hacia arriba el tamaño para alinear con 1 MB:

    ```console
    # MB=$((1024*1024))
    # size=$(qemu-img info -f raw --output json "rhel-7.4.raw" | \
      gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

    # rounded_size=$((($size/$MB + 1)*$MB))
    # qemu-img resize rhel-7.4.raw $rounded_size
    ```

    Convierta el disco sin procesar en un disco duro virtual (VHD) de tamaño fijo:

    ```console
    # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

    O bien, con la versión de qemu **2.6 o posterior**, incluya la opción `force_size`:

    ```console
    # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc rhel-7.4.raw rhel-7.4.vhd
    ```

## <a name="kickstart-file"></a>Archivo kickstart

En esta sección se muestra cómo preparar una distribución de RHEL 7 a partir de una imagen ISO mediante un archivo kickstart.

### <a name="rhel-7-from-a-kickstart-file"></a>RHEL 7 desde un archivo kickstart

1.  Cree un archivo kickstart que incluye el contenido siguiente y guarde el archivo. Para obtener información más detallada sobre la instalación de Kickstart, consulte la [guía de instalación de Kickstart](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/chap-kickstart-installations).

    ```text
    # Kickstart for provisioning a RHEL 7 Azure VM

    # System authorization information
      auth --enableshadow --passalgo=sha512

    # Use graphical install
    text

    # Do not run the Setup Agent on first boot
    firstboot --disable

    # Keyboard layouts
    keyboard --vckeymap=us --xlayouts='us'

    # System language
    lang en_US.UTF-8

    # Network information
    network  --bootproto=dhcp

    # Root password
    rootpw --plaintext "to_be_disabled"

    # System services
    services --enabled="sshd,waagent,NetworkManager"

    # System timezone
    timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

    # Partition clearing information
    clearpart --all --initlabel

    # Clear the MBR
    zerombr

    # Disk partitioning information
    part /boot --fstype="xfs" --size=500
    part / --fstyp="xfs" --size=1 --grow --asprimary

    # System bootloader configuration
    bootloader --location=mbr

    # Firewall configuration
    firewall --disabled

    # Enable SELinux
    selinux --enforcing

    # Don't configure X
    skipx

    # Power down the machine after install
    poweroff

    %packages
    @base
    @console-internet
    chrony
    sudo
    parted
    -dracut-config-rescue

    %end

    %post --log=/var/log/anaconda/post-install.log

    #!/bin/bash

    # Register Red Hat Subscription
    subscription-manager register --username=XXX --password=XXX --auto-attach --force

    # Install latest repo update
    yum update -y

    # Enable extras repo
    subscription-manager repos --enable=rhel-7-server-extras-rpms

    # Install WALinuxAgent
    yum install -y WALinuxAgent

    # Unregister Red Hat subscription
    subscription-manager unregister

    # Enable waaagent at boot-up
    systemctl enable waagent

    # Install cloud-init
    yum install -y cloud-init cloud-utils-growpart gdisk hyperv-daemons

    # Configure waagent for cloud-init
    sed -i 's/Provisioning.UseCloudInit=n/Provisioning.UseCloudInit=y/g' /etc/waagent.conf
    sed -i 's/Provisioning.Enabled=y/Provisioning.Enabled=n/g' /etc/waagent.conf
    sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
    sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf

    echo "Adding mounts and disk_setup to init stage"
    sed -i '/ - mounts/d' /etc/cloud/cloud.cfg
    sed -i '/ - disk_setup/d' /etc/cloud/cloud.cfg
    sed -i '/cloud_init_modules/a\\ - mounts' /etc/cloud/cloud.cfg
    sed -i '/cloud_init_modules/a\\ - disk_setup' /etc/cloud/cloud.cfg

    # Disable the root account
    usermod root -p '!!'

    # Disabke swap in WALinuxAgent
    ResourceDisk.Format=n
    ResourceDisk.EnableSwap=n

    # Configure swap using cloud-init
    cat > /etc/cloud/cloud.cfg.d/00-azure-swap.cfg << EOF
    #cloud-config
    # Generated by Azure cloud image build
    disk_setup:
    ephemeral0:
        table_type: mbr
        layout: [66, [33, 82]]
        overwrite: True
    fs_setup:
    - device: ephemeral0.1
        filesystem: ext4
    - device: ephemeral0.2
        filesystem: swap
    mounts:
    - ["ephemeral0.1", "/mnt"]
    - ["ephemeral0.2", "none", "swap", "sw,nofail,x-systemd.requires=cloud-init.service", "0", "0"]
    EOF

    # Set the cmdline
    sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

    # Enable SSH keepalive
    sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

    # Build the grub cfg
    grub2-mkconfig -o /boot/grub2/grub.cfg

    # Configure network
    cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
    DEVICE=eth0
    ONBOOT=yes
    BOOTPROTO=dhcp
    TYPE=Ethernet
    USERCTL=no
    PEERDNS=yes
    IPV6INIT=no
    PERSISTENT_DHCLIENT=yes
    NM_CONTROLLED=yes
    EOF

    # Deprovision and prepare for Azure if you are creating a generalized image
    sudo cloud-init clean --logs --seed
    sudo rm -rf /var/lib/cloud/
    sudo rm -rf /var/lib/waagent/
    sudo rm -f /var/log/waagent.log

    sudo waagent -force -deprovision+user
    rm -f ~/.bash_history
    export HISTSIZE=0

    %end
    ```

1. Coloque el archivo kickstart donde el sistema de la instalación puede acceder a él.

1. En el Administrador de Hyper-V, cree una nueva máquina virtual. En la página **Conectar disco duro virtual**, seleccione **Exponer un disco duro virtual más adelante** y complete el Asistente para crear nueva máquina virtual.

1. Abra la configuración de la máquina virtual:

    a.  Cargue un nuevo disco duro virtual en la máquina virtual. Asegúrese de seleccionar **Formato VHD** y **Tamaño fijo**.

    b.  Adjunte la imagen ISO de instalación a la unidad de DVD.

    c.  Configure el BIOS para que arranque desde el CD.

1. Inicie la máquina virtual. Cuando aparezca la guía de instalación, presione la tecla **Tab** para configurar las opciones de arranque.

1. Escriba `inst.ks=<the location of the kickstart file>` al final de las opciones de arranque y presione **Entrar**.

1. Espere hasta que la instalación se complete. Cuando haya finalizado, la máquina virtual se apagará automáticamente. El VHD de Linux ya está listo para cargarse en Azure.

## <a name="known-issues"></a>Problemas conocidos
### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>El controlador de Hyper-V no se pudo incluir en el disco RAM inicial al usar un hipervisor de Hyper-V

En algunos casos, los instaladores de Linux pueden no incluir los controladores de Hyper-V en el disco RAM inicial (initrd o initramfs) a menos que Linux detecte que se está ejecutando un entorno de Hyper-V.

Cuando se usa otro sistema de virtualización (es decir, VirtualBox, KVM, etc.) para preparar la imagen de Linux, es posible que tenga que recompilar initrd para asegurarse de que al menos los módulos de kernel hv_vmbus y hv_storvsc están disponibles en el disco RAM inicial. Esto es un problema conocido al menos en sistemas basados en el nivel superior de distribución de Red Hat.

Para resolver este problema, agregue los módulos de Hyper-V en initramfs y recompilarlo:

Edite `/etc/dracut.conf` y agregue el siguiente contenido:

```config-conf
add_drivers+=" hv_vmbus hv_netvsc hv_storvsc "
```

Recompile initramfs:

```console
# dracut -f -v
```

Para obtener más detalles, consulte la información sobre cómo [recompilar initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Pasos siguientes
* Ya está listo para usar el disco duro virtual de Red Hat Enterprise Linux para crear nuevas máquinas virtuales de Azure. Si es la primera vez que carga el archivo .vhd en Azure, vea [Crear una VM Linux a partir de un disco personalizado](upload-vhd.md#option-1-upload-a-vhd).
* Para obtener información más detallada sobre los hipervisores certificados para ejecutar Red Hat Enterprise Linux, visite el [sitio web de Red Hat](https://access.redhat.com/certified-hypervisors).
* Para obtener más información sobre el uso de imágenes BYOS de RHEL listas para producción, vaya a la página de documentación de [BYOS](../workloads/redhat/byos.md).
