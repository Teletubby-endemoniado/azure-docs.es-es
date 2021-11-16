---
title: Detección, evaluación y migración de instancias de máquina virtual de Google Cloud Platform (GCP) a Azure
description: En este artículo se describe cómo migrar máquinas virtuales de GCP a Azure con Azure Migrate.
author: deseelam
ms.author: deseelam
ms.manager: bsiva
ms.topic: tutorial
ms.date: 08/19/2020
ms.custom: MVC
ms.openlocfilehash: 7b826bc1d127f38e716bfcae302a055b941d9b4d
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132331212"
---
# <a name="discover-assess-and-migrate-google-cloud-platform-gcp-vms-to-azure"></a>Detección, evaluación y migración de máquinas virtuales de Google Cloud Platform (GCP) a Azure

En este tutorial se muestra cómo detectar, evaluar y migrar máquinas virtuales de Google Cloud Platform (GCP) a máquinas virtuales de Azure mediante Azure Migrate: Server Assessment y Azure Migrate: Migración del servidor.

En este tutorial, aprenderá a:
> [!div class="checklist"]
>
> * Comprobar los requisitos previos para la migración.
> * Preparar los recursos de Azure con Azure Migrate: Server Migration. Configure los permisos de la cuenta y los recursos de Azure para poder usar Azure Migrate.
> * Preparar las instancias de máquinas virtuales de GCP para la migración.
> * Agregar la herramienta Azure Migrate: Server Migration en el centro de Azure Migrate.
> * Configurar el dispositivo de replicación e implementar el servidor de configuración.
> * Instalar el servicio Mobility en las máquinas virtuales de GCP que desee migrar.
> * Habilite la replicación para máquinas virtuales.
> * Realizar un seguimiento y supervisar el estado de la replicación.
> * Ejecute una migración de prueba para asegurarse de que todo funciona de la forma esperada.
> * Ejecutar una migración completa a Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.

## <a name="discover-and-assess"></a>Detección y evaluación

Antes de migrar a Azure, se recomienda realizar una evaluación de la detección y migración de máquinas virtuales. Esta evaluación ayuda a ajustar el tamaño adecuado de las máquinas virtuales de GCP para su migración a Azure y a calcular los posibles costos de proceso de Azure.

Configure una evaluación como se indica a continuación:

1. Siga el [tutorial](./tutorial-discover-gcp.md) para configurar Azure y preparar las máquinas virtuales de GCP para una evaluación. Observe lo siguiente:

    - Azure Migrate usa la autenticación de contraseña al detectar instancias de máquinas virtuales de GCP. Las instancias de GCP no admiten la autenticación de contraseña de forma predeterminada. Antes de poder detectar, debe habilitar la autenticación de contraseña.
        - En el caso de máquinas Windows, permita el puerto WinRM 5985 (HTTP). Esto permite las llamadas remotas de Instrumental de administración de Windows.
        - Para máquinas Linux:
            1. Inicie sesión en cada máquina Linux.
            2. Abra el archivo sshd_config: vi/etc/ssh/sshd_config
            3. En el archivo, busque la línea **PasswordAuthentication** y cambie el valor a **yes** (sí).
            4. Guarde el archivo y ciérrelo. Reinicie el servicio ssh.
    - Si usa un usuario raíz para detectar las máquinas virtuales Linux, asegúrese de que se permite el inicio de sesión raíz en las máquinas virtuales.
        1. Inicie sesión en cada máquina Linux.
        2. Abra el archivo sshd_config: vi/etc/ssh/sshd_config
        3. En el archivo, busque la línea **PermitRootLogin** y cambie el valor a **yes** (sí).
        4. Guarde el archivo y ciérrelo. Reinicie el servicio ssh.

2. A continuación, siga este [tutorial](./tutorial-assess-gcp.md) para configurar un proyecto y un dispositivo de Azure Migrate para detectar y evaluar las máquinas virtuales de GCP.

Aunque se recomienda probar una evaluación, no es obligatorio llevarla a cabo para poder migrar máquinas virtuales.



## <a name="prerequisites"></a>Prerrequisitos

- Asegúrese de que las máquinas virtuales de GCP que quiere migrar ejecutan una versión del sistema operativo compatible. Las máquinas virtuales de GCP se tratan como máquinas físicas para el propósito de la migración. Revise los [sistemas operativos y versiones de kernel compatibles](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) para el flujo de trabajo de migración de servidores físicos. Puede usar comandos estándar como *hostnamectl* o *uname -a* para comprobar las versiones del sistema operativo y del kernel para las máquinas virtuales Linux.  Se recomienda realizar una migración de prueba para validar si la máquina virtual funciona según lo esperado antes de continuar con la migración real.
- Asegúrese de que las máquinas virtuales de GCP cumplan con las [configuraciones admitidas](./migrate-support-matrix-physical-migration.md#physical-server-requirements) para la migración a Azure.
- Compruebe que las máquinas virtuales de GCP que replique en Azure cumplan con los [requisitos de máquina virtual de Azure](./migrate-support-matrix-physical-migration.md#azure-vm-requirements).
- Hay que realizar algunos cambios necesarios en las máquinas virtuales antes de migrarlas a Azure.
    - En algunos sistemas operativos, Azure Migrate realiza estos cambios automáticamente.
    - Es importante realizar estos cambios antes de comenzar la migración. Si migra la máquina virtual antes de realizar el cambio, es posible que la máquina virtual no arranque en Azure.
Revise los cambios en [Windows](prepare-for-migration.md#windows-machines) y [Linux](prepare-for-migration.md#linux-machines) que debe realizar.

### <a name="prepare-azure-resources-for-migration"></a>Preparar los recursos de Azure para la migración

Prepare Azure para la migración con Azure Migrate: Herramienta de migración del servidor.

**Task** | **Detalles**
--- | ---
**Crear un proyecto de Azure Migrate** | La cuenta de Azure necesita permisos de colaborador o propietario para [crear un proyecto](./create-manage-projects.md).
**Comprobación de los permisos de la cuenta de Azure** | Su cuenta de Azure necesita permisos para crear una máquina virtual y escribir en un disco administrado de Azure.

### <a name="assign-permissions-to-create-project"></a>Asignación de permisos para crear un proyecto

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.

### <a name="assign-azure-account-permissions"></a>Asignación de los permisos de la cuenta de Azure

Asigne el rol de colaborador de la máquina virtual a la cuenta de Azure. Este rol proporciona permisos para:

- Crear una máquina virtual en el grupo de recursos seleccionado.
- Crear una máquina virtual en la red virtual seleccionada.
- Escribir en un disco administrado de Azure.

### <a name="create-an-azure-network"></a>Creación de una red de Azure

[Configure](../virtual-network/manage-virtual-network.md#create-a-virtual-network) una red virtual de Azure. Al realizar la replicación en Azure, las máquinas virtuales de Azure que se crean se unen a la red virtual de Azure que se especifica al configurar la migración.

## <a name="prepare-gcp-instances-for-migration"></a>Preparación de las instancias de GCP para la migración

Para prepararse para la migración de GCP a Azure, debe preparar e implementar un dispositivo de replicación para la migración.

### <a name="prepare-a-machine-for-the-replication-appliance"></a>Preparación de un equipo para el dispositivo de replicación

Azure Migrate: Server Migration usa un dispositivo de replicación para replicar máquinas en Azure. Este dispositivo ejecuta los siguientes componentes.

- **Servidor de configuración**: El servidor de configuración coordina la comunicación entre el entorno de las máquinas virtuales de GCP y Azure, además de administrar la replicación de datos.
- **Servidor de proceso**: El servidor de procesos actúa como puerta de enlace de replicación. Recibe los datos de la replicación; los optimiza mediante el almacenamiento en la caché, la compresión y el cifrado, y los envía a una cuenta de almacenamiento en caché en Azure.

Para prepararse para la implementación del dispositivo, siga estos pasos:

- Configure una máquina virtual de GCP independiente para hospedar el dispositivo de replicación. Esta instancia debe ejecutar Windows Server 2012 R2 o Windows Server 2016. [Revise](./migrate-replication-appliance.md#appliance-requirements) los requisitos de hardware, software y red para el dispositivo.
- El dispositivo no se debe instalar en una máquina virtual de origen que desee replicar ni en el dispositivo de detección y evaluación de Azure Migrate que haya instalado antes. se debe implementar en una máquina virtual diferente.
- Las máquinas virtuales de GCP de origen que se van a migrar deben tener línea de visión de red al dispositivo de replicación. Configure las reglas de firewall necesarias para conseguirlo. Se recomienda implementar el dispositivo de replicación en la misma red privada virtual (VPC) que las máquinas virtuales de origen que se van a migrar. Si el dispositivo de replicación debe estar en otra VPC, las redes privadas virtuales deben conectarse mediante el emparejamiento de VPC.
- Las máquinas virtuales de GCP de origen se comunican con el dispositivo de replicación en los puertos de entrada HTTPS 443 (orquestación del canal de control) y TCP 9443 (transporte de datos) para la administración de la replicación y la transferencia de datos de la replicación. A su vez, el dispositivo de replicación orquesta y envía los datos de replicación a Azure a través del puerto HTTPS 443 de salida. Para configurar estas reglas, edite las reglas de entrada y salida del grupo de seguridad con los puertos y la información de IP de origen adecuados.

   ![Reglas de firewall de GCP](./media/tutorial-migrate-gcp-virtual-machines/gcp-firewall-rules.png)


   ![Editar reglas de firewall](./media/tutorial-migrate-gcp-virtual-machines/gcp-edit-firewall-rule.png)

- El dispositivo de replicación usa MySQL. Revise las [opciones](migrate-replication-appliance.md#mysql-installation) para instalar MySQL en el dispositivo.
- Revise las direcciones URL de Azure necesarias para que el dispositivo de replicación acceda a las nubes [públicas](migrate-replication-appliance.md#url-access) y [gubernamentales](migrate-replication-appliance.md#azure-government-url-access).

## <a name="set-up-the-replication-appliance"></a>Configuración del dispositivo de replicación

El primer paso de la migración consiste en configurar el dispositivo de replicación. Para configurar el dispositivo para la migración de las máquinas virtuales de GCP, debe descargar el archivo del instalador del dispositivo y, luego, ejecutarlo en la [máquina virtual que ha preparado](#prepare-a-machine-for-the-replication-appliance).

### <a name="download-the-replication-appliance-installer"></a>Descarga del instalador del dispositivo de replicación

1. En el proyecto de Azure Migrate > **Servidores**, en **Azure Migrate: Migración del servidor**, haga clic en **Detectar**.

    ![Detectar máquinas virtuales](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

2. En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , haga clic en **No virtualizado/Otro**.
3. En **Región de destino**, seleccione la región de Azure a la que desea migrar las máquinas.
4. Seleccione **Confirmar que la región de destino de la migración es \<region-name\>** .
5. Haga clic en **Crear recursos**. Esto crea un almacén de Azure Site Recovery en segundo plano.
    - Si ya ha configurado la migración con Azure Migrate Server Migration, no se puede configurar la opción de destino, ya que los recursos se configuraron anteriormente.
    - Después de hacer clic en este botón ya no se puede cambiar la región de destino de este proyecto.
    - Para migrar las máquinas virtuales a otra región, debe crear un proyecto de Azure Migrate nuevo o diferente.
    > [!NOTE]
    > Si seleccionó el punto de conexión privado como método de conectividad para el proyecto Azure Migrate cuando se creó, el almacén de Recovery Services también se configurará para la conectividad de punto de conexión privado. Asegúrese de que los puntos de conexión privados sean accesibles desde el dispositivo de replicación: [**Más información**](troubleshoot-network-connectivity.md)

6. En **¿Quiere instalar un nuevo dispositivo de replicación?** , seleccione **Instalar un dispositivo de replicación**.
7. En **Descargue e instale el software del dispositivo de replicación**, descargue el instalador del dispositivo y la clave de registro. Necesitará la clave para registrar el dispositivo. La clave será válida durante cinco días a partir del momento en que se descarga.

    ![Descarga del proveedor](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

8. Copie el archivo de configuración del dispositivo y el archivo de clave a la máquina virtual de GCP para Windows Server 2016 o Windows Server 2012 que creó para el dispositivo de replicación.
9. Ejecute el archivo de configuración del dispositivo de replicación, tal como se describe en el procedimiento siguiente.  
    9.1. En **Antes de comenzar**, seleccione **Install the configuration server and process server** (Instalar el servidor de configuración y el servidor de procesos) y luego haga clic en **Siguiente**.   
    9.2 En **Third-Party Software License** (Licencia de software de terceros), haga clic en **I Accept the third party license agreement** (Acepto el contrato de licencia de terceros) y, a continuación, seleccione **Next** (Siguiente).   
    9.3 En **Registration** (Registro), seleccione **Browse** (Examinar) y vaya a donde colocó el archivo de clave de registro del almacén. Seleccione **Next** (Siguiente).  
    9.4 En **Internet Settings** (Configuración de Internet), seleccione **Connect directly to Azure Site Recovery without a proxy server** (Conectar directamente con Azure Site Recovery sin un servidor proxy) y, a continuación, seleccione **Next** (Siguiente).  
    9.5 En la página **Prerequisites Check** (Comprobación de requisitos previos) se ejecutan comprobaciones de varios elementos. Cuando haya finalizado, seleccione **Siguiente**.  
    9.6 En **MySQL Configuration** (Configuración de MySQL), proporcione una contraseña para la base de datos MySQL y, a continuación, seleccione **Next** (Siguiente).  
    9.7 En **Environment Details** (Detalles del entorno), seleccione **No**. No es necesario proteger las máquinas virtuales. Después, seleccione **Siguiente**.  
    9.8 En **Install Location** (Ubicación de instalación), seleccione **Next** (Siguiente) para aceptar el valor predeterminado.  
    9.9 En **Network Selection** (Selección de red), elija **Next** (Siguiente) para aceptar el valor predeterminado.  
    9.10 En **Summary** (Resumen), seleccione **Install** (Instalar).   
    9.11 **Installation Progress** (Progreso de la instalación) muestra información acerca del proceso de instalación. Cuando haya finalizado, seleccione **Finalizar**. Aparece una ventana que muestra un mensaje sobre un reinicio. Seleccione **Aceptar**.   
    9.12 A continuación, en una ventana se muestra un mensaje sobre la frase de contraseña de la conexión del servidor de configuración. Copie esa frase en el portapapeles y guárdela en un archivo de texto temporal en las máquinas virtuales de origen. Necesitará la frase de contraseña más adelante, durante el proceso de instalación del servicio Mobility.
10. Una vez finalizada la instalación, el asistente para la configuración de dispositivos se iniciará automáticamente (también puede iniciarlo manualmente mediante el acceso directo cspsconfigtool que se crea en el escritorio del dispositivo). En este tutorial, vamos a instalar manualmente el servicio Mobility en las máquinas virtuales de origen que se van a replicar, por lo que debe crear una cuenta ficticia en este paso y continuar. Puede especificar los siguientes datos para crear la cuenta ficticia: "guest" como nombre descriptivo, "username" como nombre de usuario y "password" como contraseña de la cuenta. Esta cuenta ficticia la usará en la fase de habilitación de la replicación.

    ![Finalizar el registro](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

## <a name="install-the-mobility-service"></a>Instalación de Mobility Service

Se debe instalar un agente del servicio Mobility en las máquinas virtuales de GCP de origen que se van a migrar. Los instaladores del agente están disponibles en el dispositivo de replicación. Debe encontrar el instalador correcto e instalar el agente en cada máquina que desee migrar. Haga lo siguiente:

1. Inicie sesión en el dispositivo de replicación.
2. Vaya a **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository**.
3. Busque el instalador correspondiente al sistema operativo y la versión de las máquinas virtuales de GCP de origen. Revise los [sistemas operativos compatibles](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines).
4. Copie el archivo de instalación en la máquina virtual de GCP de origen que desea migrar.
5. Asegúrese de que tiene el archivo de texto de frase de contraseña guardado que se creó al instalar el dispositivo de replicación.
    - Si olvidó guardar la frase de contraseña, puede verla en el dispositivo de replicación con este paso. Desde la línea de comandos, ejecute **C:\ProgramData\ASR\home\svsystems\bin\genpassphrase.exe -v** para ver la frase de contraseña actual.
    - Ahora, copie esta frase de contraseña en el portapapeles y guárdela en un archivo de texto temporal en las máquinas virtuales de origen.

### <a name="installation-guide-for-windows-gcp-vms"></a>Guía de instalación de máquinas virtuales de GCP para Windows

1. Extraiga el contenido del archivo de instalación en una carpeta local (por ejemplo, C:\Temp) de la máquina virtual de GCP, como se indica a continuación:

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```  

2. Ejecute el instalador de Mobility Service:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```  

3. Registre el agente en el dispositivo de replicación:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="installation-guide-for-linux-gcp-vms"></a>Guía de instalación para máquinas virtuales de GCP para Linux

1. Extraiga el contenido del tarball de instalación en una carpeta local (por ejemplo, /tmp/MobSvcInstaller) de la máquina virtual de GCP, como se indica a continuación:
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```  

2. Ejecute el script del instalador:
    ```
    sudo ./install -r MS -q
    ```  

3. Registre el agente en el dispositivo de replicación:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="enable-replication-for-gcp-vms"></a>Habilitar la replicación para máquinas virtuales de GCP

> [!NOTE]
> A través del portal, puede agregar hasta 10 máquinas virtuales para que se repliquen a la vez. Para replicar más máquinas virtuales simultáneamente, puede agregarlas en lotes de 10.

1. En el proyecto de Azure Migrate > **Servidores**, **Azure Migrate: Migración del servidor**, haga clic en **Replicar**.

    ![Replicación de máquinas virtuales](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. En **Replicar** > **Configuración de origen** >  **¿Las máquinas están virtualizadas?** , seleccione **Not virtualized/Other** (No virtualizadas/Otros).
3. En **Dispositivo local**, seleccione el nombre del dispositivo de Azure Migrate que configuró.
4. En **Servidor de procesos**, seleccione el nombre del dispositivo de replicación.
5. En **Credenciales de invitado**, seleccione la cuenta ficticia creada anteriormente en la [configuración del instalador de la replicación](#download-the-replication-appliance-installer) para instalar el servicio Mobility de forma manual (la instalación de inserción no se admite). A continuación, haga clic en **Siguiente: Máquinas virtuales**.   

    ![Configuración de replicación](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)
6. En **Máquinas virtuales**, en **¿Quiere importar la configuración de migración de una evaluación?** , deje la configuración predeterminada **No, especificaré la configuración de migración manualmente**.
7. Compruebe todas las máquinas virtuales que desea migrar. A continuación, haga clic en **Siguiente: Configuración de destino**.

    ![Selección de las máquinas virtuales](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)

8. En **Configuración de destino**, seleccione la suscripción y la región de destino a la que va a migrar, y especifique el grupo de recursos en el que residirán las máquinas virtuales de Azure después de la migración.
9. En **Red virtual**, seleccione la red virtual o la subred de Azure a la que se unirán las máquinas virtuales de Azure después de la migración.  
10. En **Cuenta de almacenamiento en caché**, mantenga la opción predeterminada para usar la cuenta de almacenamiento en caché que se crea automáticamente para el proyecto. Use la lista desplegable si desea especificar una cuenta de almacenamiento diferente para usarla como cuenta de almacenamiento en caché con fines de replicación. <br/>
    > [!NOTE]
    >
    > - Si seleccionó el punto de conexión privado como método de conectividad para el proyecto Azure Migrate, conceda al almacén de Recovery Services acceso a la cuenta de almacenamiento en caché. [**Más información**](how-to-use-azure-migrate-with-private-endpoints.md#grant-access-permissions-to-the-recovery-services-vault)
    > - Para la replicación mediante ExpressRoute con emparejamiento privado, cree un punto de conexión privado para la cuenta de almacenamiento en caché. [**Más información**](how-to-use-azure-migrate-with-private-endpoints.md#create-a-private-endpoint-for-the-storage-account-optional)
11. En **Opciones de disponibilidad**, seleccione:
    -  La zona de disponibilidad para anclar la máquina migrada a una zona de disponibilidad específica de la región. Use esta opción para distribuir los servidores que forman una capa de aplicación de varios nodos en Availability Zones. Si selecciona esta opción, deberá especificar la zona de disponibilidad que se va a usar en cada una de las máquinas seleccionadas en la pestaña Proceso. Esta opción solo está disponible si la región de destino seleccionada para la migración admite Availability Zones.
    -  El conjunto de disponibilidad para colocar la máquina migrada en un conjunto de disponibilidad. Para usar esta opción, el grupo de recursos de destino seleccionado debe tener uno o varios conjuntos de disponibilidad.
    - No se requiere ninguna opción de redundancia de infraestructura si no necesita ninguna de estas configuraciones de disponibilidad para las máquinas migradas.
12. En **Disk encryption type** (Tipo de cifrado de disco), seleccione:
    - Cifrado en reposo con clave administrada por la plataforma
    - Cifrado en reposo con clave administrada por el cliente
    - Cifrado doble con claves administradas por el cliente y por la plataforma

   > [!NOTE]
   > Para replicar máquinas virtuales con CMK, será necesario [crear un conjunto de cifrado de disco](../virtual-machines/disks-enable-customer-managed-keys-portal.md#set-up-your-disk-encryption-set) en el grupo de recursos de destino. Un objeto de conjunto de cifrado de disco asigna instancias de Managed Disks a una instancia de Key Vault que contiene las claves CMK que se van a usar para SSE.

13. En **Ventaja híbrida de Azure**:

    - Seleccione **No** si no desea aplicar la Ventaja híbrida de Azure. A continuación, haga clic en **Siguiente**.
    - Seleccione **Sí** si tiene equipos con Windows Server que están incluidos en suscripciones activas de Software Assurance o Windows Server y desea aplicar el beneficio a las máquinas que va a migrar. A continuación, haga clic en **Siguiente**.

    ![Configuración de destino](./media/tutorial-migrate-vmware/target-settings.png)

14. En **Proceso**, revise el nombre, el tamaño, el tipo de disco del sistema operativo y la configuración de disponibilidad (si se ha seleccionado en el paso anterior) de la máquina virtual. Las máquinas virtuales deben cumplir los [requisitos de Azure](migrate-support-matrix-physical-migration.md#azure-vm-requirements).

    - **Tamaño de VM**: si usa las recomendaciones de la evaluación, el menú desplegable de tamaño de máquina virtual muestra el tamaño recomendado. De lo contrario, Azure Migrate elige un tamaño en función de la coincidencia más cercana en la suscripción de Azure. También puede elegir un tamaño de manera manual en **Tamaño de la máquina virtual de Azure**.
    - **Disco del sistema operativo**: especifique el disco del sistema operativo (arranque) de la máquina virtual. Este es el disco que tiene el cargador de arranque y el instalador del sistema operativo.
    - **Zona de disponibilidad**: especifique la zona de disponibilidad que se va a usar.
    - **Conjunto de disponibilidad**: especifique el conjunto de disponibilidad que se va a usar.

![Configuración de Proceso](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

15. En **Discos**, especifique si los discos de máquina virtual se deben replicar en Azure y seleccione el tipo de disco (discos SSD o HDD estándar o bien discos administrados premium) en Azure. A continuación, haga clic en **Siguiente**.
    - Puede excluir discos de la replicación.
    - Si excluye discos, no estarán presentes en la máquina virtual de Azure después de la migración.

    ![Configuración de discos](./media/tutorial-migrate-physical-virtual-machines/disks.png)

16. En **Revisar e iniciar la replicación**, revise la configuración y haga clic en **Replicar** para iniciar la replicación inicial de los servidores.

> [!NOTE]
> Puede actualizar la configuración de replicación en cualquier momento antes de que esta comience; para ello, vaya a **Administrar** > **Replicación de máquinas**. Una vez iniciada la replicación, su configuración no se puede cambiar.

## <a name="track-and-monitor-replication-status"></a>Realizar un seguimiento y supervisar el estado de la replicación

- Al hacer clic en **Replicar**, comienza el trabajo de inicio de replicación.
- Cuando el trabajo de inicio de replicación finaliza correctamente, las máquinas virtuales comienzan su replicación inicial en Azure.
- Cuando finaliza esta replicación inicial, comienza la replicación diferencial. Los cambios incrementales de los discos de máquina virtual de GCP se replican periódicamente en los discos de réplica de Azure.

Puede realizar un seguimiento del estado del trabajo en las notificaciones del portal.

Para supervisar el estado de la replicación, haga clic en **Replicando servidores** en **Azure Migrate: Server Migration**.  

![Supervisión de la replicación](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Ejecutar una migración de prueba

Cuando comienza la replicación diferencial, puede ejecutar una migración de prueba para las máquinas virtuales antes de ejecutar una migración completa a Azure. La migración de prueba es muy recomendable, ya que supone una oportunidad para detectar cualquier posible problema y corregirlo antes de pasar a la migración real. Se recomienda hacerla al menos una vez para cada máquina virtual antes de migrarla.

- La ejecución de una migración de prueba comprueba que la migración funcionará según lo previsto, sin afectar a las máquinas virtuales de GCP, que seguirán estando operativas y continuarán realizando la replicación.
- Para simular la migración, la migración de prueba crea una máquina virtual de Azure usando datos replicados (normalmente, con una migración a una red virtual que no es de producción en la suscripción a Azure).
- Puede usar la máquina virtual de Azure de prueba replicada para validar la migración, realizar pruebas de aplicaciones y resolver los problemas antes de la migración completa.

Realice una migración de prueba como se indica a continuación:

1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Migration**, haga clic en **Probar servidores migrados**.

     ![Probar servidores migrados](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Haga clic con el botón derecho en la máquina virtual que va a probar y haga clic en **Migración de prueba**.

    :::image type="content" source="./media/tutorial-migrate-physical-virtual-machines/test-migrate-inline.png" alt-text="Captura de pantalla que muestra el resultado después de hacer clic en Probar migración." lightbox="./media/tutorial-migrate-physical-virtual-machines/test-migrate-expanded.png":::

3. En **Migración de prueba**, seleccione la red virtual de Azure en la que se ubicará la máquina virtual de Azure después de la migración. Se recomienda usar una red virtual que no sea de producción.
4. Comienza el trabajo de **Migración de prueba**. Supervise el trabajo en las notificaciones del portal.
5. Una vez finalizada la migración, la máquina virtual de Azure migrada se puede ver en **Máquinas virtuales** en Azure Portal. El nombre de la máquina tiene el sufijo **-Test**.
6. Una vez finalizada la prueba, haga clic con el botón derecho en la máquina virtual de Azure, en **Replicación de máquinas**, y haga clic en **Limpiar la migración de prueba**.

    :::image type="content" source="./media/tutorial-migrate-physical-virtual-machines/clean-up-inline.png" alt-text="Captura de pantalla que muestra el resultado después de la limpieza de la migración de prueba." lightbox="./media/tutorial-migrate-physical-virtual-machines/clean-up-expanded.png":::

    > [!NOTE]
    > Ahora puede registrar los servidores que ejecutan SQL Server con el punto de retención de SQL VM para aprovechar las ventajas de la aplicación automatizada de revisiones, la copia de seguridad automatizada y la administración simplificada de licencias mediante la extensión Agente de IaaS de SQL.
    >- Seleccione **Manage** > **Replicating servers** > **Machine containing SQL server** >  **Compute and Network** (Administrar > Servidores de replicación > Máquina que contiene servidor SQL Server > Proceso y red) y seleccione **yes** (Sí) para registrarse con el RP de máquina virtual de SQL.
    >- Seleccione Ventaja híbrida de Azure para SQL Server si tiene instancias de SQL Server que están incluidas en suscripciones activas de Software Assurance o SQL Server y quiere aplicar la ventaja a las máquinas que va a migrar.

## <a name="migrate-gcp-vms"></a>Migración de máquinas virtuales de GCP

Después de comprobar que la migración de prueba funciona según lo previsto, puede migrar las máquinas virtuales de GCP.

1. En el proyecto de Azure Migrate > **Servidores** > **Azure Migrate: Server Migration**, haga clic en **Replicando servidores**.

    ![Replicando servidores](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. En **Replicación de máquinas**, haga clic con el botón derecho en la máquina virtual > **Migrar**.
3. En **Migrar** >  **¿Quiere apagar las máquinas virtuales y realizar una migración planificada sin perder datos?** , seleccione **Sí** > **Aceptar**.
    - Si no desea apagar la VM, seleccione **No**.
4. Se inicia un trabajo de migración de la máquina virtual. Para ver el estado del trabajo, haga clic en el icono de la campana de notificación, en la parte superior derecha de la página del portal, o vaya a la página de trabajos de la herramienta Server Migration (en el icono de la herramienta, haga clic en Información general > Seleccionar trabajos en el menú izquierdo).
5. Una vez finalizado el trabajo, puede ver y administrar la máquina virtual desde la página Máquinas virtuales.

### <a name="complete-the-migration"></a>Completar la migración

1. Una vez finalizada la migración, haga clic con el botón derecho en la máquina virtual > **Detener migración**. Esto hace lo siguiente:
    - Detiene la replicación de la máquina virtual de GCP.
    - Quita la máquina virtual de GCP del recuento de **Replicando servidores** en Azure Migrate: Server Migration.
    - Limpia la información de estado de replicación de la máquina virtual.
1. Compruebe y [solucione los problemas de activación de Windows en la máquina virtual de Azure.](/troubleshoot/azure/virtual-machines/troubleshoot-activation-problems)
1. Realice los ajustes de la aplicación posteriores a la migración, como actualizar los nombres de host, las cadenas de conexión de la base de datos y las configuraciones del servidor web.
1. Realice las pruebas finales de la aplicación y la aceptación de la migración en la aplicación migrada que ahora se ejecuta en Azure.
1. Pase el tráfico a la instancia de máquina virtual de Azure migrada.
1. Actualice la documentación interna para mostrar la nueva ubicación y la dirección IP las máquinas virtuales de Azure.



## <a name="post-migration-best-practices"></a>Procedimientos recomendados después de la migración

- Para aumentar la resistencia:
    - Proteja los datos mediante la copia de seguridad de máquinas virtuales de Azure mediante el servicio Azure Backup. [Más información](../backup/quick-backup-vm-portal.md).
    - Mantenga las cargas de trabajo en ejecución y disponibles continuamente mediante la replicación de máquinas virtuales de Azure en una región secundaria con Site Recovery. [Más información](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Para aumentar la seguridad:
    - Bloquee y limite el acceso del tráfico entrante con [Microsoft Defender para la nube: administración Just-In-Time](../security-center/security-center-just-in-time.md).
    - Restrinja el tráfico de red a los puntos de conexión de administración con [grupos de seguridad de red](../virtual-network/network-security-groups-overview.md).
    - Implemente [Azure Disk Encryption](../security/fundamentals/azure-disk-encryption-vms-vmss.md) para ayudar a proteger discos y datos frente al robo y acceso no autorizado.
    - Obtenga más información sobre la [protección de recursos IaaS](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) y visite [Microsoft Defender para la nube](https://azure.microsoft.com/services/security-center/).
- Para supervisión y administración:
    - Considere la posibilidad de implementar [Azure Cost Management](../cost-management-billing/cost-management-billing-overview.md) para supervisar el gasto y el uso de recursos.



## <a name="troubleshooting--tips"></a>Solución de problemas y sugerencias

**Pregunta:** No puedo ver mi máquina virtual de GCP en la lista de servidores detectados para la migración   
**Respuesta:** Compruebe si el dispositivo de replicación cumple los requisitos. Asegúrese de que el agente de movilidad está instalado en la máquina virtual de origen que se va a migrar y que está registrado en el servidor de configuración. Compruebe las reglas de firewall para habilitar una ruta de acceso de red entre el dispositivo de replicación y las máquinas virtuales de GCP de origen.  

**Pregunta:** ¿Cómo puedo saber si la máquina virtual se migró correctamente?   
**Respuesta:** Tras la migración, puede ver y administrar la máquina virtual desde la página Máquinas virtuales. Conéctese a la máquina virtual migrada para realizar la validación.  

**Pregunta:** No puedo importar máquinas virtuales para la migración a partir de los resultados de Server Assessment creados anteriormente   
**Respuesta:** Actualmente, no se admite la importación de evaluaciones para este flujo de trabajo. Como solución alternativa, puede exportar la evaluación y seleccionar manualmente la recomendación de máquina virtual durante el paso de habilitación de replicación.

**Pregunta:** Recibo el error "Failed to fetch BIOS GUID" (No se pudo capturar el GUID del BIOS) al intentar detectar mis máquinas virtuales de GCP   
**Respuesta:** Use el inicio de sesión raíz para la autenticación y no otro usuario. Si no puede usar un usuario raíz, asegúrese de que las funcionalidades requeridas estén establecidas en el usuario, según las instrucciones que se indican en la [matriz de compatibilidad](migrate-support-matrix-physical.md#physical-server-requirements). Revise también los sistemas operativos compatibles para las máquinas virtuales de GCP.  

**Pregunta:** Mi estado de replicación no progresa   
**Respuesta:** Compruebe si el dispositivo de replicación cumple los requisitos. Asegúrese de que ha habilitado los puertos necesarios, TCP 9443 y HTTPS 443, en el dispositivo de replicación para el transporte de datos. Asegúrese de que no haya versiones duplicadas obsoletas del dispositivo de replicación conectadas al mismo proyecto.   

**Pregunta:** No puedo detectar instancias de GCP con Azure Migrate debido al código de estado HTTP 504 del servicio de administración remota de Windows    
**Respuesta:** Asegúrese de revisar los requisitos del dispositivo de Azure Migrate y las necesidades de acceso URL. Asegúrese de que ninguna configuración de proxy bloquea el registro del dispositivo.

**Pregunta:** ¿Tengo que realizar alguna modificación antes de migrar mis máquinas virtuales de GCP a Azure?   
**Respuesta:** quizá deba realizar estos cambios antes de migrar las máquinas virtuales de EC2 a Azure:

- Si usa cloud-init para el aprovisionamiento de la máquina virtual, quizá quiera deshabilitar cloud-init en la máquina virtual antes de replicarla en Azure. Los pasos de aprovisionamiento que realiza cloud-init en la máquina virtual quizás sean específicos de GCP y no serán válidos después de la migración a Azure. 
- Revise la sección [Requisitos previos](#prerequisites) para determinar si es necesario realizar algún cambio en el sistema operativo antes de la migración a Azure.
- Siempre se recomienda ejecutar una migración de prueba antes de la migración final.  

## <a name="next-steps"></a>Pasos siguientes

Investigue el [proceso de la migración en la nube](/azure/architecture/cloud-adoption/getting-started/migrate) en el marco de Cloud Adoption Framework para Azure.
