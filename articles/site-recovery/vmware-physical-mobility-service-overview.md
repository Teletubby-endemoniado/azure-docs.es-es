---
title: Acerca del servicio Mobility para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: Aprenda acerca del agente del servicio Mobility para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con el servicio Azure Site Recovery.
author: Sharmistha-Rai
manager: gaggupta
ms.service: site-recovery
ms.topic: how-to
ms.author: sharrai
ms.date: 08/19/2021
ms.openlocfilehash: 1bf251d6aa45aaf0306d3e595a674280214dc64a
ms.sourcegitcommit: 1a0fe16ad7befc51c6a8dc5ea1fe9987f33611a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131867311"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>Acerca de Mobility Service para máquinas virtuales VMware y servidores físicos

Al configurar la recuperación ante desastres para máquinas virtuales de VMware y servidores físicos con [Azure Site Recovery](site-recovery-overview.md), instala el servicio Mobility de Site Recovery en cada máquina virtual de VMware y servidor físico local. Mobility Service captura las escrituras de datos en la máquina y las reenvía al servidor de procesos de Site Recovery. El servicio Mobility se instala mediante el software del agente correspondiente que puede implementar mediante los siguientes métodos:

- [Instalación de inserción](#push-installation): cuando se habilita la protección mediante Azure Portal, Site Recovery instala el servicio Mobility en el servidor.
- Instalación manual: puede instalar el servicio Mobility manualmente en cada máquina mediante la [interfaz de usuario](#install-the-mobility-service-using-ui-classic) o del [símbolo del sistema](#install-the-mobility-service-using-command-prompt-classic).
- [Implementación automatizada](vmware-azure-mobility-install-configuration-mgr.md): puede automatizar la instalación del servicio Mobility con herramientas de implementación de software como Configuration Manager.

> [!NOTE]
> El servicio Mobility usa aproximadamente entre un 6 y un 10 % de memoria en máquinas de origen para máquinas físicas o máquinas virtuales de VMware.

## <a name="antivirus-on-replicated-machines"></a>Antivirus en máquinas replicadas

Si las máquinas que desea replicar están ejecutando un software antivirus, excluya la carpeta de instalación del servicio Mobility, _C:\ProgramData\ASR\agent_, de las operaciones del antivirus. Esta exclusión garantiza que la replicación funcionará según lo esperado.

## <a name="push-installation"></a>Instalación de inserción

La instalación de inserción es una parte integral del trabajo que se ejecuta desde Azure Portal para [habilitar la replicación](vmware-azure-enable-replication.md#enable-replication). Después de elegir el conjunto de máquinas virtuales que desea proteger y de habilitar la replicación, el servidor de configuración inserta el agente del servicio Mobility en los servidores, lo instala y completa su registro con el servidor de configuración.

### <a name="prerequisites"></a>Prerrequisitos

- Asegúrese de que se cumplen todos los [requisitos previos](vmware-azure-install-mobility-service.md).
- Asegúrese de que todas las configuraciones de servidor cumplen los criterios que se indican en [Matriz de compatibilidad para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure](vmware-physical-azure-support-matrix.md).
- A partir de la versión 9.36, en el caso de SUSE Linux Enterprise Server 11 SP3, RHEL 5, CentOS 5 y Debian 7, asegúrese de que el instalador más reciente está [disponible en el servidor de configuración y que se realiza la escalabilidad horizontal del servidor de procesos](#download-latest-mobility-agent-installer-for-suse-11-sp3-rhel-5-debian-7-server).

El flujo de trabajo de instalación de inserción se describe en las siguientes secciones:

### <a name="mobility-service-agent-version-923-and-higher"></a>Versión 9.23 y posteriores del agente del servicio Mobility

Para más información acerca de la versión 9.23, consulte el [paquete acumulativo de actualizaciones 35 para Azure Site Recovery](https://support.microsoft.com/help/4494485/update-rollup-35-for-azure-site-recovery).

Durante una instalación de inserción del servicio Mobility, se realizan los siguientes pasos:

1. El agente se inserta en la máquina de origen. Es posible que no se pueda copiar el agente en la máquina de origen debido a varios errores de entorno. Visite [nuestra guía](vmware-azure-troubleshoot-push-install.md) para solucionar los errores de instalación de inserción.
1. Tras copiarse correctamente el agente en el servidor, se realiza una comprobación de requisitos previos en este último.
   - Si se cumplen todos los requisitos previos, empezará la instalación.
   - Si uno o varios de los [requisitos previos](vmware-physical-azure-support-matrix.md) no se cumplen, se producirá un error en la instalación.
1. Como parte de la instalación del agente, se instala el proveedor del Servicio de instantáneas de volumen (VSS) para Azure Site Recovery. Este proveedor se usa para generar puntos de recuperación coherentes con la aplicación. Si se produce un error en la instalación de VSS, este paso se omitirá y la instalación del agente continuará.
1. Si la instalación del agente se realiza correctamente, pero se produce un error en la del proveedor de VSS, el estado del trabajo se marca como **Advertencia**. Esto no afecta a la generación de puntos de recuperación coherentes frente a bloqueos.

    - Para generar puntos coherentes con la aplicación, consulte [nuestra guía](vmware-physical-manage-mobility-service.md#install-site-recovery-vss-provider-on-source-machine) para completar una instalación manual del proveedor de VSS de Site Recovery.
    - Si no desea que se generen puntos de recuperación coherentes con la aplicación, [modifique la directiva de replicación](vmware-azure-set-up-replication.md#create-a-policy) para desactivarlos.

### <a name="mobility-service-agent-version-922-and-below"></a>Versión 9.22 y anteriores del agente del servicio Mobility

1. El agente se inserta en la máquina de origen. Es posible que no se pueda copiar el agente en la máquina de origen debido a varios errores de entorno. Consulte [nuestra guía](vmware-azure-troubleshoot-push-install.md) para solucionar los errores de instalación de inserción.
1. Tras copiarse correctamente el agente en el servidor, se realiza una comprobación de requisitos previos en este último.
   - Si se cumplen todos los requisitos previos, empezará la instalación.
   - Si uno o varios de los [requisitos previos](vmware-physical-azure-support-matrix.md) no se cumplen, se producirá un error en la instalación.

1. Como parte de la instalación del agente, se instala el proveedor del Servicio de instantáneas de volumen (VSS) para Azure Site Recovery. Este proveedor se usa para generar puntos de recuperación coherentes con la aplicación.
   - Si se produce un error en la instalación del proveedor de VSS, no se podrá realizar la instalación del agente. Para evitar errores en la instalación del agente, use la [versión 9.23](https://support.microsoft.com/help/4494485/update-rollup-35-for-azure-site-recovery) o versiones posteriores para generar puntos de recuperación coherentes frente a bloqueos e instalar el proveedor de VSS manualmente.

## <a name="install-the-mobility-service-using-ui-classic"></a>Instalación del servicio Mobility mediante una interfaz de usuario (clásico)

>[!NOTE]
> Esta sección es aplicable a Azure Site Recovery: clásico. [Estas son las instrucciones de instalación de la versión preliminar](#install-the-mobility-service-using-ui-preview)
### <a name="prerequisites"></a>Prerrequisitos

- Asegúrese de que todas las configuraciones de servidor cumplen los criterios que se indican en [Matriz de compatibilidad para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure](vmware-physical-azure-support-matrix.md).
- [Busque el instalador](#locate-installer-files) correspondiente según el sistema operativo del servidor.

>[!IMPORTANT]
> No use el método de instalación con interfaz de usuario si va a replicar una máquina virtual de infraestructura como servicio (IaaS) de Azure de una región de Azure a otra. Use la instalación desde el [símbolo del sistema](#install-the-mobility-service-using-command-prompt-classic).

1. Copie el archivo de instalación en el equipo y ejecútelo.
1. En **Opción de instalación**, seleccione **Instalar Mobility Service**.
1. Elija la ubicación de instalación y seleccione **Instalar**.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility1.png" alt-text="Página Opción de instalación del servicio Mobility":::.

1. Supervise la instalación en **Progreso de la instalación**. Una vez completada la instalación, seleccione **Continuar con la configuración** para registrar el servicio en el servidor de configuración.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility3.png" alt-text="Captura de pantalla que muestra el progreso de la instalación y el botón Proceed to Configuration (Continuar con la configuración) activo cuando finaliza esta.":::

1. En **Configuration Server Details** (Detalles del servidor de configuración), especifique la dirección IP y la frase de contraseña que ha configurado.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility4.png" alt-text="Página de registro del servicio Mobility":::.

1. Seleccione **Registrar** para finalizar el registro.

    :::image type="content" source="./media/vmware-physical-mobility-service-install-manual/mobility5.png" alt-text="Página de registro final del servicio Mobility":::.

## <a name="install-the-mobility-service-using-command-prompt-classic"></a>Instalación del servicio Mobility mediante el símbolo del sistema (clásico)

>[!NOTE]
> Esta sección es aplicable a Azure Site Recovery: clásico. [Estas son las instrucciones de instalación de la versión preliminar](#install-the-mobility-service-using-command-prompt-preview).

### <a name="prerequisites"></a>Prerrequisitos

- Asegúrese de que todas las configuraciones de servidor cumplen los criterios que se indican en [Matriz de compatibilidad para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure](vmware-physical-azure-support-matrix.md).
- [Busque el instalador](#locate-installer-files) correspondiente según el sistema operativo del servidor.

### <a name="windows-machine"></a>Un equipo Windows

- En un símbolo del sistema, ejecute los siguientes comandos para copiar el instalador en una carpeta local como, por ejemplo, _C:\Temp_, en el servidor que desea proteger. Reemplace el nombre de archivo del instalador por el nombre de archivo real.

  ```cmd
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted
  ```

- Ejecute este comando para instalar el agente.

  ```cmd
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```

- Ejecute estos comandos para registrar el agente en el servidor de configuración.

  ```cmd
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="installation-settings"></a>Configuración de la instalación

Configuración | Detalles
--- | ---
Sintaxis | `UnifiedAgent.exe /Role \<MS/MT> /InstallLocation \<Install Location> /Platform "VmWare" /Silent`
Registros de configuración | `%ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log`
`/Role` | Parámetro de instalación obligatorio. Especifica si se debe instalar el servicio Mobility (Agent) o el destino maestro (MasterTarget).  Nota: En versiones anteriores, los modificadores correctos eran Mobility Service (MS) o destino maestro (MT)
`/InstallLocation`| Parámetro opcional. Especifique la ubicación (cualquier carpeta) de la instalación de Mobility Service.
`/Platform` | Mandatory. Especifica la plataforma en la que se instala el servicio Mobility: <br/> **VMware** para las máquinas virtuales y los servidores físicos de VMware. <br/> **Azure** para las máquinas virtuales de Azure.<br/><br/> Si está tratando máquinas virtuales de Azure como máquinas físicas, especifique **VMware**.
`/Silent`| Opcional. Especifica si se debe ejecutar el instalador en modo silencioso.

#### <a name="registration-settings"></a>Configuración de registro
Configuración | Detalles
--- | ---
Sintaxis | `UnifiedAgentConfigurator.exe  /CSEndPoint \<CSIP> /PassphraseFilePath \<PassphraseFilePath>`
Registros de configuración del agente | `%ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log`
`/CSEndPoint` | Parámetro obligatorio. `<CSIP>` especifica la dirección IP del servidor de configuración. Use una dirección IP válida.
`/PassphraseFilePath` |  Mandatory. Ubicación de la frase de contraseña. Use cualquier ruta de acceso de archivo local o UNC válida.

### <a name="linux-machine"></a>En un equipo Linux

1. Desde una sesión terminal, copie el instalador en una carpeta local (por ejemplo, _/tmp_) del servidor que desea proteger. Reemplace el nombre de archivo del instalador por el nombre de archivo real de la distribución de Linux y, a continuación, ejecute los comandos.

   ```shell
   cd /tmp ;
   tar -xvf Microsoft-ASR_UA_version_LinuxVersion_GA_date_release.tar.gz
   ```

2. Realice la instalación de la siguiente manera (no se requiere una cuenta raíz, pero sí permisos de raíz):

   ```shell
   sudo ./install -d <Install Location> -r Agent -v VmWare -q
   ```

3. Una vez completada la instalación, el servicio Mobility debe registrarse en el servidor de configuración. Ejecute el siguiente comando para registrar el servicio Mobility en el servidor de configuración.

   ```shell
   /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
   ```

#### <a name="installation-settings"></a>Configuración de la instalación

Configuración | Detalles
--- | ---
Sintaxis | `./install -d \<Install Location> -r \<MS/MT> -v VmWare -q`
`-r` | Parámetro de instalación obligatorio. Especifica si se debe instalar Mobility Service (MS) o el destino maestro (MT).
`-d` | Parámetro opcional. Especifica la ubicación de la instalación del servicio Mobility: `/usr/local/ASR`.
`-v` | Mandatory. Especifica la plataforma en la que se instala el servicio Mobility. <br/> **VMware** para las máquinas virtuales y los servidores físicos de VMware. <br/> **Azure** para las máquinas virtuales de Azure.
`-q` | Opcional. Especifica si se debe ejecutar el instalador en modo silencioso.

#### <a name="registration-settings"></a>Configuración de registro

Configuración | Detalles
--- | ---
Sintaxis | `cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i \<CSIP> -P \<PassphraseFilePath>`
`-i` | Parámetro obligatorio. `<CSIP>` especifica la dirección IP del servidor de configuración. Use una dirección IP válida.
`-P` |  Mandatory. Ruta de acceso completa al archivo en que se guarda la frase de contraseña. Uso de cualquier carpeta válida.

## <a name="azure-virtual-machine-agent"></a>Agente de máquina virtual de Azure

- **Máquinas virtuales Windows**: desde la versión 9.7.0.0 de Mobility Service, el instalador de Mobility Service instala el [agente de máquina virtual de Azure](../virtual-machines/extensions/features-windows.md#azure-vm-agent). De este forma se garantiza que, cuando un equipo conmuta por error a Azure, la máquina virtual de Azure cumple el requisito previo de instalación del agente para usar cualquier extensión de máquina virtual.
- **Máquinas virtuales Linux**: [WALinuxAgent](../virtual-machines/extensions/update-linux-agent.md) se instala automáticamente en la máquina virtual de Azure después de la conmutación por error.

## <a name="locate-installer-files"></a>Búsqueda de archivos del instalador

En el servidor de configuración vaya a la carpeta _%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository_. Compruebe qué instalador necesita en función del sistema operativo. En la siguiente tabla se resumen los archivos del instalador para cada máquina virtual VMware y el sistema operativo del servidor físico. Antes de empezar, puede revisar los [sistemas operativos admitidos](vmware-physical-azure-support-matrix.md#replicated-machines).

> [!NOTE]
> Los nombres de archivo utilizan la sintaxis que se muestra en la tabla siguiente con _version_ y _date_ como marcadores de posición de valores reales. Los nombres de archivo reales tendrán un aspecto similar al de estos ejemplos:
> - `Microsoft-ASR_UA_9.30.0.0_Windows_GA_22Oct2019_release.exe`
> - `Microsoft-ASR_UA_9.30.0.0_UBUNTU-16.04-64_GA_22Oct2019_release.tar.gz`

Archivo de instalador | Sistema operativo (solo de 64 bits)
--- | ---
`Microsoft-ASR_UA_version_Windows_GA_date_release.exe` | Windows Server 2016 </br> Windows Server 2012 R2 </br> Windows Server 2012 </br> Windows Server 2008 R2 SP1
[Para descargarlo y colocarlo en esta carpeta de forma manual](#rhel-5-or-centos-5-server) | Red Hat Enterprise Linux (RHEL) 5 </br> CentOS 5
`Microsoft-ASR_UA_version_RHEL6-64_GA_date_release.tar.gz` | Red Hat Enterprise Linux (RHEL) 6 </br> CentOS 6
`Microsoft-ASR_UA_version_RHEL7-64_GA_date_release.tar.gz` | Red Hat Enterprise Linux (RHEL) 7 </br> CentOS 7
`Microsoft-ASR_UA_version_RHEL8-64_GA_date_release.tar.gz` | Red Hat Enterprise Linux (RHEL) 8 </br> CentOS 8
`Microsoft-ASR_UA_version_SLES12-64_GA_date_release.tar.gz` | SUSE Linux Enterprise Server 12 SP1 </br> Incluye SP2 y SP3.
[Para descargarlo y colocarlo en esta carpeta de forma manual](#suse-11-sp3-server) | SUSE Linux Enterprise Server 11 SP3
`Microsoft-ASR_UA_version_SLES11-SP4-64_GA_date_release.tar.gz` | SUSE Linux Enterprise Server 11 SP4
`Microsoft-ASR_UA_version_SLES15-64_GA_date_release.tar.gz` | SUSE Linux Enterprise Server 15
`Microsoft-ASR_UA_version_OL6-64_GA_date_release.tar.gz` | Oracle Enterprise Linux 6.4 </br> Oracle Enterprise Linux 6.5
`Microsoft-ASR_UA_version_OL7-64_GA_date_release.tar.gz` | Oracle Enterprise Linux 7
`Microsoft-ASR_UA_version_OL8-64_GA_date_release.tar.gz` | Oracle Enterprise Linux 8
`Microsoft-ASR_UA_version_UBUNTU-14.04-64_GA_date_release.tar.gz` | Ubuntu Linux 14.04
`Microsoft-ASR_UA_version_UBUNTU-16.04-64_GA_date_release.tar.gz` | Servidor Ubuntu Linux 16.04 LTS
`Microsoft-ASR_UA_version_UBUNTU-18.04-64_GA_date_release.tar.gz` | Servidor Ubuntu Linux 18.04 LTS
`Microsoft-ASR_UA_version_UBUNTU-20.04-64_GA_date_release.tar.gz` | Servidor Ubuntu Linux 20.04 LTS
[Para descargarlo y colocarlo en esta carpeta de forma manual](#debian-7-server) | Debian 7
`Microsoft-ASR_UA_version_DEBIAN8-64_GA_date_release.tar.gz` | Debian 8
`Microsoft-ASR_UA_version_DEBIAN9-64_GA_date_release.tar.gz` | Debian 9

## <a name="download-latest-mobility-agent-installer-for-suse-11-sp3-rhel-5-debian-7-server"></a>Descarga del instalador del agente de movilidad más reciente para el servidor de SUSE 11 SP3, RHEL 5, Debian 7

### <a name="suse-11-sp3-server"></a>Servidor SUSE 11 SP3

Como **requisito previo para actualizar o proteger las máquinas con SUSE Linux Enterprise Server 11 SP3**  a partir de la versión 9.36:

1. Asegúrese de que el instalador del agente de movilidad más reciente se descarga desde el Centro de descarga de Microsoft y se coloca en el repositorio del instalador de inserciones del servidor de configuración y en todos los servidores de procesos de escalado horizontal
2. [Descargue](site-recovery-whats-new.md) el instalador del agente más reciente para SUSE Linux Enterprise Server 11 SP3.
3. Vaya al servidor de configuración y copie el instalador del agente para SUSE Linux Enterprise Server 11 SP3 en la ruta de acceso: INSTALL_DIR\home\svsystems\pushinstallsvc\repository.
1. Después de copiar el instalador más reciente, reinicie el servicio InMage PushInstall.
1. Ahora, vaya a los servidores de proceso de escalabilidad horizontal asociados y repita los pasos 3 y 4.
1. **Por ejemplo**, si la ruta de acceso de instalación es C:\Archivos de programa (x86)\Microsoft Azure Site Recovery, los directorios mencionados anteriormente serán
    1. C:\Archivos de programa (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository

### <a name="rhel-5-or-centos-5-server"></a>Servidor RHEL 5 o CentOS 5

Como **requisito previo para actualizar o proteger las máquinas con RHEL 5** a partir de la versión 9.36:

1. Asegúrese de que el instalador del agente de movilidad más reciente se descarga desde el Centro de descarga de Microsoft y se coloca en el repositorio del instalador de inserciones del servidor de configuración y en todos los servidores de procesos de escalado horizontal
2. [Descargue](site-recovery-whats-new.md) el instalador del agente más reciente para RHEL 5 o CentOS 5.
3. Vaya al servidor de configuración y copie el instalador del agente para RHEL 5 o CentOS 5 en la ruta de acceso: INSTALL_DIR\home\svsystems\pushinstallsvc\repository.
1. Después de copiar el instalador más reciente, reinicie el servicio InMage PushInstall.
1. Ahora, vaya a los servidores de proceso de escalabilidad horizontal asociados y repita los pasos 3 y 4.
1. **Por ejemplo**, si la ruta de acceso de instalación es C:\Archivos de programa (x86)\Microsoft Azure Site Recovery, los directorios mencionados anteriormente serán
    1. C:\Archivos de programa (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository

## <a name="debian-7-server"></a>Servidor Debian 7

Como **requisito previo para actualizar o proteger las máquinas con Debian 7** a partir de la versión 9.36:

1. Asegúrese de que el instalador del agente de movilidad más reciente se descarga desde el Centro de descarga de Microsoft y se coloca en el repositorio del instalador de inserciones del servidor de configuración y en todos los servidores de procesos de escalado horizontal
2. [Descargue](site-recovery-whats-new.md) el instalador del agente más reciente para Debian 7.
3. Vaya al servidor de configuración y copie el instalador del agente para Debian 7 en la ruta de acceso: INSTALL_DIR\home\svsystems\pushinstallsvc\repository.
1. Después de copiar el instalador más reciente, reinicie el servicio InMage PushInstall.
1. Ahora, vaya a los servidores de proceso de escalabilidad horizontal asociados y repita los pasos 3 y 4.
1. **Por ejemplo**, si la ruta de acceso de instalación es C:\Archivos de programa (x86)\Microsoft Azure Site Recovery, los directorios mencionados anteriormente serán
    1. C:\Archivos de programa (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository

## <a name="install-the-mobility-service-using-ui-preview"></a>Instalación del servicio Mobility mediante una interfaz de usuario (versión preliminar)

>[!NOTE]
> Esta sección es aplicable a Azure Site Recovery: versión preliminar. [Estas son las instrucciones de instalación de la versión clásica](#install-the-mobility-service-using-ui-classic).

### <a name="prerequisites"></a>Requisitos previos

Realice los pasos siguientes para buscar los archivos del instalador del sistema operativo del servidor:  
- En el dispositivo, vaya a la carpeta *E:\Software\Agents*.
- Copie el instalador correspondiente al sistema operativo de la máquina de origen y colóquelo en la máquina de origen, en una carpeta local, como *C:\Azure Site Recovery\Agent*.

**Siga estos pasos para instalar el servicio Mobility:**

1. Abra el símbolo del sistema y vaya a la carpeta en la que se ha colocado el archivo del instalador.

   ```cmd
    cd C:\Azure Site Recovery\Agent*
   ```

2. Ejecute el comando siguiente para extraer el archivo del instalador:

   ```cmd
   .\Microsoft-ASR_UA*Windows*release.exe /q /x:C:\Azure Site Recovery\Agent
   ```

3. Ejecute el comando siguiente para continuar con la instalación. De esta forma, se iniciará la interfaz de usuario del instalador:

   ```cmd
   .\UnifiedAgentInstaller.exe /Platform vmware /Role MS /CSType CSPrime /InstallLocation "C:\Azure Site Recovery\Agent"
   ```

   >[!NOTE]
   >La ubicación de instalación mencionada en la interfaz de usuario es la misma que se pasó en el comando.

4. Haga clic en **Instalar**.

   Esto iniciará la instalación del servicio Mobility.

   Espere hasta que se haya completado. Una vez hecho esto, llegará al paso de registro; puede registrar la máquina de origen en el dispositivo que prefiera.

   ![Imagen que muestra la opción para instalar la interfaz de usuario del servicio Mobility](./media/vmware-physical-mobility-service-overview-preview/mobility-service-install.png)

   ![Imagen que muestra el progreso de la instalación el servicio Mobility](./media/vmware-physical-mobility-service-overview-preview/installation-progress.png)

5. Copie la cadena presente en el campo **Detalles de la máquina**.

   Este campo incluye información única de la máquina de origen. Esta información es necesaria para [generar el archivo de configuración del servicio Mobility](#generate-mobility-service-configuration-file).

   ![Imagen que muestra la cadena de la máquina de origen](./media/vmware-physical-mobility-service-overview-preview/source-machine-string.png)

6.  Proporcione la ruta de acceso del **archivo de configuración del servicio Mobility** en el configurador de Unified Agent.
7.  Haga clic en **Registrar**.

    De esta forma la máquina de origen se registrará correctamente en el dispositivo.

## <a name="install-the-mobility-service-using-command-prompt-preview"></a>Instalación del servicio Mobility mediante el símbolo del sistema (versión preliminar)

>[!NOTE]
> Esta sección es aplicable a Azure Site Recovery: versión preliminar. [Estas son las instrucciones de instalación de la versión clásica](#install-the-mobility-service-using-command-prompt-classic).

### <a name="windows-machine"></a>Un equipo Windows
1. Abra el símbolo del sistema y vaya a la carpeta en la que se ha colocado el archivo del instalador.

   ```cmd
   cd C:\Azure Site Recovery\Agent
   ```
2. Ejecute el comando siguiente para extraer el archivo del instalador:
   ```cmd
       .\Microsoft-ASR_UA*Windows*release.exe /q /x:C:\Azure Site Recovery\Agent
    ```
3. Ejecute el comando siguiente para continuar con la instalación:

   ```cmd

    .\UnifiedAgentInstaller.exe /Platform vmware /Silent /Role MS /CSType CSPrime /InstallLocation "C:\Azure Site Recovery\Agent"
   ```
    Una vez completada la instalación, copie la cadena que se genera junto con el parámetro *Agent Config Input*. Esta cadena es necesaria para [generar el archivo de configuración del servicio Mobility](#generate-mobility-service-configuration-file).

    ![cadena de ejemplo para descargar el archivo de configuración ](./media/vmware-physical-mobility-service-overview-preview/configuration-string.png)

4. Después de realizar la instalación correctamente, registre la máquina de origen en el dispositivo anterior con el comando siguiente:

   ```cmd
   "C:\Azure Site Recovery\Agent\agent\UnifiedAgentConfigurator.exe" /SourceConfigFilePath "config.json" /CSType CSPrime
   ```

#### <a name="installation-settings"></a>Configuración de la instalación

Configuración | Detalles
--- | ---
Sintaxis | `.\UnifiedAgentInstaller.exe /Platform vmware /Role MS /CSType CSPrime /InstallLocation <Install Location>`
`/Role` | Parámetro de instalación obligatorio. Especifica si se instalará el servicio Mobility (MS).
`/InstallLocation`| Opcional. Especifique la ubicación (cualquier carpeta) de la instalación de Mobility Service.
`/Platform` | Mandatory. Especifica la plataforma en la que se instala el servicio Mobility: <br/> **VMware** para las máquinas virtuales y los servidores físicos de VMware. <br/> **Azure** para las máquinas virtuales de Azure.<br/><br/> Si está tratando máquinas virtuales de Azure como máquinas físicas, especifique **VMware**.
`/Silent`| Opcional. Especifica si se debe ejecutar el instalador en modo silencioso.
`/CSType`| Mandatory. Se usa para definir la arquitectura en versión preliminar o heredada. (CSPrime o CSLegacy)

#### <a name="registration-settings"></a>Configuración de registro

Configuración | Detalles
--- | ---
Sintaxis | `"<InstallLocation>\UnifiedAgentConfigurator.exe" /SourceConfigFilePath "config.json" /CSType CSPrime >`
`/SourceConfigFilePath` | Mandatory. Ruta de acceso completa del archivo de configuración del servicio Mobility. Uso de cualquier carpeta válida.
`/CSType` |  Mandatory. Se usa para definir la arquitectura en versión preliminar o heredada. (CSPrime o CSLegacy).


### <a name="linux-machine"></a>En un equipo Linux

1. Desde una sesión terminal, copie el instalador en una carpeta local (por ejemplo, **/tmp**) del servidor que desea proteger. A continuación, ejecute el comando siguiente:

   ```shell
       cd /tmp ;
       tar -xvf Microsoft-ASR_UA_version_LinuxVersion_GA_date_release.tar.gz
   ```

2. Para instalar, use el comando siguiente:
   ```shell
        ./install -q -r MS -v VmWare -c CSPrime
    ```

    Una vez completada la instalación, copie la cadena que se genera junto con el parámetro *Agent Config Input*. Esta cadena es necesaria para [generar el archivo de configuración del servicio Mobility](#generate-mobility-service-configuration-file).

3. Después de realizar la instalación correctamente, registre la máquina de origen en el dispositivo anterior con el comando siguiente:

   ```shell
        <InstallLocation>/Vx/bin/UnifiedAgentConfigurator.sh -c CSPrime -S config.json -q
    ```
#### <a name="installation-settings"></a>Configuración de la instalación

  Configuración | Detalles
  --- | ---
    Sintaxis | `./install -q -r MS -v VmWare -c CSPrime`
    `-r` | Mandatory. Parámetro de instalación. Especifica si debe instalarse el servicio Mobility (MS).
    `-d` | Opcional. Especifica la ubicación de la instalación del servicio Mobility: `/usr/local/ASR`.
    `-v` | Mandatory. Especifica la plataforma en la que se instala el servicio Mobility. <br/> **VMware** para las máquinas virtuales y los servidores físicos de VMware. <br/> **Azure** para las máquinas virtuales de Azure.
    `-q` | Opcional. Especifica si se debe ejecutar el instalador en modo silencioso.
    `-c` | Mandatory. Se usa para definir la arquitectura en versión preliminar o heredada. (CSPrime o CSLegacy).

#### <a name="registration-settings"></a>Configuración de registro

  Configuración | Detalles
  --- | ---
    Sintaxis | `cd <InstallLocation>/Vx/bin UnifiedAgentConfigurator.sh -c CSPrime -S -q`  
    `-s` | Mandatory. Ruta de acceso completa del archivo de configuración del servicio Mobility. Uso de cualquier carpeta válida.
    `-c` |  Mandatory. Se usa para definir la arquitectura en versión preliminar o heredada. (CSPrime o CSLegacy).
    `-q` |  Opcional. Especifica si se debe ejecutar el instalador en modo silencioso.

## <a name="generate-mobility-service-configuration-file"></a>Generación de un archivo de configuración del servicio Mobility

  Realice los pasos siguientes para generar el archivo de configuración del servicio Mobility:

  1. Vaya al dispositivo en el que quiere registrar la máquina de origen. Abra Microsoft Azure Appliance Configuration Manager y navegue a la sección de **detalles de configuración del servicio Mobility**.
  2. Pegue en el campo de entrada la cadena de detalles de la máquina que copió del servicio Mobility.
  3. Haga clic en **Descargar el archivo de configuración**.

  ![Imagen que muestra la opción de descargar el archivo de configuración para el servicio Mobility](./media/vmware-physical-mobility-service-overview-preview/download-configuration-file.png)

Al hacerlo, se descargará el archivo de configuración del servicio Mobility. Copie este archivo en una carpeta local de la máquina de origen. Puede colocarlo en la misma carpeta que el instalador del servicio Mobility.

Consulte la información sobre cómo [actualizar el servicio Mobility](upgrade-mobility-service-preview.md).

## <a name="next-steps"></a>Pasos siguientes

[Configurar la instalación de Mobility Service](vmware-azure-install-mobility-service.md).
