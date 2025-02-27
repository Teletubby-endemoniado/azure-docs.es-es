---
title: Información general del agente Connected Machine
description: En este artículo se proporciona una descripción detallada del agente de servidores habilitados para Azure Arc disponible, que admite la supervisión de máquinas virtuales hospedadas en entornos híbridos.
ms.date: 11/03/2021
ms.topic: conceptual
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 30c9a32cabf24e54dd41d45d40a8ff07920fd919
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132546925"
---
# <a name="overview-of-azure-arc-enabled-servers-agent"></a>Introducción al agente de servidores habilitados para Azure Arc

El agente Connected Machine de servidores habilitados para Azure Arc permite administrar las máquinas Windows y Linux hospedadas fuera de Azure en la red corporativa o en otros proveedores de nube. En este artículo se proporciona una descripción detallada del agente, de los requisitos del sistema y de la red, y de los diferentes métodos de implementación.

>[!NOTE]
> El [agente de Azure Monitor](../../azure-monitor/agents/azure-monitor-agent-overview.md) (AMA) no reemplaza al agente de Connected Machine. El agente de Azure Monitor reemplazará al agente de Log Analytics, a la extensión Diagnostics y al agente de Telegraf para máquinas Windows y Linux. Revise la documentación de Azure Monitor sobre el nuevo agente para obtener más detalles.

## <a name="agent-component-details"></a>Detalles del componente de agente

:::image type="content" source="media/agent-overview/connected-machine-agent.png" alt-text="Introducción al agente de servidores habilitado para Azure Arc." border="false":::

El paquete del agente Azure Connected Machine contiene varios componentes lógicos que se agrupan juntos.

* Hybrid Instance Metadata Service (HIMDS) administra la conexión a Azure y la identidad de Azure de la máquina conectada.

* El agente de configuración de invitado proporciona la funcionalidad de evaluación de si la máquina cumple con las directivas requeridas y las normas de cumplimiento aplicables.

    Tenga en cuenta el siguiente comportamiento con [Configuración de invitado](../../governance/policy/concepts/guest-configuration.md) de Azure Policy para una máquina desconectada:

  * Una asignación de Azure Policy que se dirige a las máquinas desconectadas no se ve afectada.
  * La asignación de invitado se almacena de forma local durante 14 días. En el período de 14 días, si el agente Connected Machine se vuelve a conectar al servicio, se vuelven a aplicar las asignaciones de directiva.
  * Las asignaciones se eliminan después de 14 días y no se reasignan a la máquina después de dicho período.

* El agente de extensión administra las extensiones de VM, incluida la instalación, desinstalación y actualización. Las extensiones se descargan de Azure y se copian en la carpeta `%SystemDrive%\%ProgramFiles%\AzureConnectedMachineAgent\ExtensionService\downloads` en Windows, y en `/opt/GC_Ext/downloads` en Linux. En Windows, la extensión se instala en la siguiente ruta de acceso `%SystemDrive%\Packages\Plugins\<extension>` y, en Linux, la extensión se instala en `/var/lib/waagent/<extension>`.

## <a name="instance-metadata"></a>Metadatos de instancia

La información de los metadatos sobre la máquina conectada se recopila después de que el agente de Connected Machine se registre con los servidores habilitados para Azure Arc. Concretamente:

* Nombre, tipo y versión del sistema operativo
* Nombre del equipo
* Modelo y el fabricante del equipo
* Nombre de dominio completo (FQDN) del equipo
* Nombre de dominio (si está unido a un dominio de Active Directory)
* Versión del agente Connected Machine
* Active Directory y nombre de dominio completo (FQDN) de DNS
* UUID (IDENTIFICADOR DE BIOS)
* Latido del agente de Connected Machine
* Versión del agente Connected Machine
* Clave pública para la identidad administrada
* Estado y detalles de cumplimiento de las directivas (si usa directivas de configuración de invitado)
* SQL Server está instalado (valor booleano)
* Identificador de recurso de clúster (para nodos de Azure Stack HCI)

El agente de Azure solicita la siguiente información de metadatos:

* Ubicación del recurso (región)
* Virtual machine ID (Identificador de máquina virtual)
* Etiquetas
* Certificado de identidad administrada de Azure Active Directory
* Asignaciones de la directiva de configuración de invitado
* Solicitudes de extensión: instalación, actualización y eliminación.

## <a name="download-agents"></a>Descarga de agentes

Puede descargar el paquete del agente de Azure Connected Machine para Windows y Linux desde las ubicaciones que se indican a continuación.

* [Paquete de Windows Installer con el agente](https://aka.ms/AzureConnectedMachineAgent) del centro de descarga de Microsoft.

* En el caso del paquete del agente de Linux, este se distribuye a través del [repositorio de paquetes](https://packages.microsoft.com/) de Microsoft con el formato preferido para la distribución (.RPM o .DEB).

El agente de Azure Connected Machine para Windows y Linux se puede actualizar a la versión más reciente de forma manual o automática según sus necesidades. Para más información, consulte [esta página](manage-agent.md).

## <a name="prerequisites"></a>Prerrequisitos

### <a name="supported-environments"></a>Entornos admitidos

Los servidores habilitados para Azure Arc admiten la instalación del agente de Connected Machine en cualquier servidor físico y máquina virtual hospedados *fuera* de Azure. Esto incluye compatibilidad con máquinas virtuales que se ejecutan en plataformas como las siguientes:

* VMware
* Azure Stack HCI
* Otro entornos de nube

Los servidores habilitados para Azure Arc *no* admiten la instalación del agente en máquinas virtuales que se ejecutan en Azure ni en las que se ejecutan en Azure Stack Hub o Azure Stack Edge, porque ya están modeladas como máquinas virtuales de Azure.

### <a name="supported-operating-systems"></a>Sistemas operativos admitidos

Las siguientes versiones de los sistemas operativos Windows y Linux son compatibles oficialmente con el agente de Azure Connected Machine:

* Windows Server 2008 R2 SP1, Windows Server 2012 R2, 2016, 2019 y 2022 (incluido Server Core)
* Ubuntu 16.04, 18.04 y 20.04 LTS (x64)
* CentOS Linux 7 y 8 (x64)
* SUSE Linux Enterprise Server (SLES) 12 y 15 (x64)
* Red Hat Enterprise Linux (RHEL) 7 y 8 (x64)
* Amazon Linux 2 (x64)
* Oracle Linux 7

> [!WARNING]
> El nombre de host de Linux o de equipo de Windows no puede usar una de las palabras reservadas o marcas en el nombre; de lo contrario, si intenta registrar la máquina conectada con Azure, se producirá un error. Consulte [Resolución de errores en los nombres de recursos reservados](../../azure-resource-manager/templates/error-reserved-resource-name.md) para obtener una lista de las palabras reservadas.

> [!NOTE]
> Aunque los servidores habilitados para Azure Arc admiten Amazon Linux, los siguientes no admiten esta distribución:
>
> * La instancia de Dependency Agent que usa VM Insights de Azure Monitor
> * Update Management en Azure Automation

### <a name="software-requirements"></a>Requisitos de software

* Se requiere .NET Framework 4.6 o posterior. [Descargue .NET Framework](/dotnet/framework/install/guide-for-developers).
* Se requiere Windows PowerShell 5.1. [Descargue Windows Management Framework 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

### <a name="required-permissions"></a>Permisos necesarios

* Para incorporar máquinas, debe ser miembro de la **Incorporación de Azure Connected Machine** o del [rol de Colaborador](../../role-based-access-control/built-in-roles.md#contributor) en el grupo de recursos.

* Para leer, modificar y eliminar una máquina, debe ser miembro del **rol Administrador de recursos de Azure Connected Machine** en el grupo de recursos.

* Para seleccionar un grupo de recursos de la lista desplegable al usar el método de **Generación de scripts**, como mínimo, debe ser miembro del rol [Lector](../../role-based-access-control/built-in-roles.md#reader) para ese grupo de recursos.

### <a name="azure-subscription-and-service-limits"></a>Límites del servicio y la suscripción de Azure

Antes de configurar las máquinas con servidores habilitados para Azure Arc, revise los [límites de suscripción](../../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits) y los [límites del grupo de recursos](../../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits) de Azure Resource Manager para planear el número de máquinas que se van a conectar.

Los servidores habilitados para Azure Arc admiten hasta 5000 instancias de máquina en un grupo de recursos.

### <a name="register-azure-resource-providers"></a>Registro de proveedores de recursos de Azure

Los servidores habilitados para Azure Arc dependen de los siguientes proveedores de recursos de Azure de la suscripción para poder usar este servicio:

* **Microsoft.HybridCompute**
* **Microsoft.GuestConfiguration**

Si no están registrados, puede registrarlos con los comandos siguientes:

Azure PowerShell:

```azurepowershell-interactive
Login-AzAccount
Set-AzContext -SubscriptionId [subscription you want to onboard]
Register-AzResourceProvider -ProviderNamespace Microsoft.HybridCompute
Register-AzResourceProvider -ProviderNamespace Microsoft.GuestConfiguration
```

CLI de Azure:

```azurecli-interactive
az account set --subscription "{Your Subscription Name}"
az provider register --namespace 'Microsoft.HybridCompute'
az provider register --namespace 'Microsoft.GuestConfiguration'
```

Para registrar también los proveedores de recursos en Azure Portal siga los pasos que se describen en [Azure Portal](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

### <a name="transport-layer-security-12-protocol"></a>Protocolo Seguridad de la capa de transporte 1.2

Para garantizar la seguridad de los datos en tránsito hacia Azure, se recomienda encarecidamente configurar la máquina para que use Seguridad de la capa de transporte (TLS) 1.2. Las versiones anteriores de TLS/Capa de sockets seguros (SSL) han demostrado ser vulnerables y, si bien todavía funcionan para permitir la compatibilidad con versiones anteriores, **no se recomiendan**.

|Plataforma/lenguaje | Soporte técnico | Más información |
| --- | --- | --- |
|Linux | Las distribuciones de Linux tienden a basarse en [OpenSSL](https://www.openssl.org) para la compatibilidad con TLS 1.2. | Compruebe el [registro de cambios de OpenSSL](https://www.openssl.org/news/changelog.html) para confirmar si su versión de OpenSSL es compatible.|
| Windows Server 2012 R2 y versiones posteriores | Compatible y habilitado de manera predeterminada. | Para confirmar que aún usa la [configuración predeterminada](/windows-server/security/tls/tls-registry-settings).|

## <a name="networking-configuration"></a>Configuración de red

El agente de Connected Machine para Linux y Windows se comunica de forma segura con la salida de Azure Arc a través del puerto TCP 443. Si la máquina necesita conectarse a través de un firewall o un servidor proxy para comunicarse por Internet, el agente se comunica de forma saliente mediante el protocolo HTTP. Los servidores proxy no hacen que el agente de Connected Machine sea más seguro, ya que el tráfico ya está cifrado.

Para proteger aún más la conectividad de red con Azure Arc, en lugar de usar redes públicas y servidores proxy, puede implementar un [ámbito de Private Link de Azure Arc](private-link-security.md) (versión preliminar).

> [!NOTE]
> Los servidores habilitados para Azure Arc no admiten el uso de una [puerta de enlace de Log Analytics](../../azure-monitor/agents/gateway.md) como proxy para el agente de Connected Machine.
>

Si la conectividad saliente está restringida por el firewall o el servidor proxy, asegúrese de que las direcciones URL que se muestran a continuación no estén bloqueadas. Si solo permite los intervalos IP o los nombres de dominio necesarios para que el agente se comunique con el servicio, también debe permitir el acceso a las siguientes etiquetas y direcciones URL del servicio.

Etiquetas de servicio:

* AzureActiveDirectory
* AzureTrafficManager
* AzureResourceManager
* AzureArcInfrastructure
* Storage

Direcciones URL:

| Recurso del agente | Descripción |
|---------|---------|
|`azgn*.servicebus.windows.net`|Plataforma de conectividad de Azure Arc|
|`management.azure.com`|Azure Resource Manager|
|`login.windows.net`|Azure Active Directory|
|`login.microsoftonline.com`|Azure Active Directory|
|`pas.windows.net`|Azure Active Directory|
|`*.guestconfiguration.azure.com` |Servicios de extensión y configuración de invitado|
|`*.his.arc.azure.com`|Metadatos y servicios de identidad híbridos|
|`*.blob.core.windows.net`|Origen de descarga para las extensiones de servidores habilitados para Azure Arc|
|`dc.services.visualstudio.com`|Telemetría del agente|

Para obtener una lista de direcciones IP para cada etiqueta o región de servicio, consulte el archivo JSON [Rangos de direcciones IP y etiquetas de servicio de Azure: nube pública](https://www.microsoft.com/download/details.aspx?id=56519). Microsoft publica actualizaciones semanales que incluyen cada uno de los servicios de Azure y los intervalos IP que usan. Esta información en el archivo JSON es la lista actual en un momento dado de los intervalos de direcciones IP que corresponden a cada etiqueta de servicio. Las direcciones IP están sujetas a cambios. Si se necesitan intervalos de direcciones IP para la configuración del firewall, se debe usar la etiqueta de servicio **AzureCloud** para permitir el acceso a todos los servicios de Azure. No deshabilite la supervisión de seguridad ni la inspección de estas direcciones URL, pero permítalas como haría con otro tráfico de Internet.

Para obtener más información, consulte [Información general sobre etiquetas de servicio](../../virtual-network/service-tags-overview.md).

## <a name="installation-and-configuration"></a>Instalación y configuración

La conexión de máquinas del entorno híbrido directamente con Azure se puede lograr mediante diferentes métodos según sus requisitos. En la tabla siguiente se resalta cada método para determinar cuál se adapta mejor a su organización.

> [!IMPORTANT]
> El agente de Connected Machine no se puede instalar en una máquina virtual Windows de Azure. Si lo intenta, la instalación lo detectará y revertirá la operación.

| Método | Descripción |
|--------|-------------|
| interactivamente, | Instale manualmente el agente en una máquina, o en un grupo reducido de estas, siguiendo los pasos que se indican en [Conexión de máquinas desde Azure Portal](onboard-portal.md).<br> En Azure Portal, puede generar un script y ejecutarlo en la máquina para automatizar los pasos de instalación y configuración del agente.|
| A escala | Instale y configure el agente para varias máquinas según lo que se indica en [Conexión de máquinas mediante una entidad de servicio](onboard-service-principal.md).<br> Este método crea una entidad de servicio para conectar máquinas de forma no interactiva.|
| A escala | A fin de instalar y configurar el agente para varias máquinas, siga el método [Conexión de máquinas híbridas a Azure desde Automation Update Management](onboard-update-management-machines.md).<br> Este método crea una entidad de servicio e instala y configura el agente para varias máquinas administradas con Azure Automation Update Management a fin de conectar las máquinas de forma no interactiva. |
| A escala | Instale y configure el agente para varias máquinas según el método [Uso de DSC de Windows PowerShell](onboard-dsc.md).<br> Este método usa una entidad de servicio para conectar máquinas de forma no interactiva con DSC de PowerShell. |

## <a name="connected-machine-agent-technical-overview"></a>Introducción técnica al agente de Connected Machine

### <a name="windows-agent-installation-details"></a>Detalles de instalación del agente de Windows

El agente Connected Machine para Windows se puede instalar con uno de los tres métodos siguientes:

* Haga doble clic en el archivo `AzureConnectedMachineAgent.msi`.
* Ejecute manualmente el paquete de Windows Installer `AzureConnectedMachineAgent.msi` desde el shell de comandos.
* Desde una sesión de PowerShell mediante un método con scripts.

Después de instalar el agente de Connected Machine para Windows, se aplican los siguientes cambios de configuración a todo el sistema.

* Durante la instalación se crean las carpetas de instalación siguientes.

    |Carpeta |Descripción |
    |-------|------------|
    |%ProgramFiles%\AzureConnectedMachineAgent |Archivos ejecutables del servicio de metadatos de instancia y la CLI de azcmagent.|
    |%ProgramFiles%\AzureConnectedMachineAgent\ExtensionService\GC | Archivos ejecutables del servicio de extensión.|
    |%ProgramFiles%\AzureConnectedMachineAgent\GuestConfig\GC | Archivos ejecutables del servicio de configuración de invitado (directiva).|
    |%ProgramData%\AzureConnectedMachineAgent |Archivos de token de configuración, registro e identidad para la CLI de azcmagent y el servicio de metadatos de instancia.|
    |%ProgramData%\GuestConfig |Descargas de paquetes de extensión, descargas de definiciones de configuración de invitado (directiva) y registros para los servicios de configuración de extensión y de invitado.|

* Los servicios de Windows siguientes se crean en el equipo de destino durante la instalación del agente.

    |Nombre del servicio |Nombre para mostrar |Nombre del proceso |Descripción |
    |-------------|-------------|-------------|------------|
    |himds |Instance Metadata Service de Azure híbrido |himds |Este servicio implementa el servicio de metadatos de instancia híbrida (IMDS) para administrar la conexión a Azure y la identidad de Azure de la máquina conectada.|
    |GCArcService |Servicio de Arc de configuración de invitado |gc_service |Supervisa la configuración del estado deseado de la máquina.|
    |ExtensionService |Servicio de extensión de la configuración de invitado | gc_service |Instala las extensiones necesarias que tienen como destino la máquina.|

* Las variables de entorno siguientes se crean durante la instalación del agente.

    |Nombre |Valor predeterminado |Descripción |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |<`http://localhost:40342/metadata/identity/oauth2/token`> ||
    |IMDS_ENDPOINT |<`http://localhost:40342`> ||

* Hay varios archivos de registro disponibles para la solución de problemas. Se describen en la tabla siguiente.

    |Log |Descripción |
    |----|------------|
    |%ProgramData%\AzureConnectedMachineAgent\Log\himds.log |Registra los detalles del componente de agente de identidad y latido.|
    |%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log |Contiene la salida de los comandos de la herramienta azcmagent.|
    |%ProgramData%\GuestConfig\arc_policy_logs\ |Registra los detalles del componente del agente de configuración de invitado (directiva).|
    |%ProgramData%\GuestConfig\ext_mgr_logs|Registra los detalles sobre el componente de agente de extensión.|
    |%ProgramData%\GuestConfig\extension_logs\\\<Extension>|Registra los detalles de la extensión instalada.|

* Se crea el grupo de seguridad local **Aplicaciones de extensión de agente híbrido**.

* Durante la desinstalación del agente, no se quitan los artefactos siguientes.

  * %ProgramData%\AzureConnectedMachineAgent\Log
  * %ProgramData%\AzureConnectedMachineAgent y subdirectorios
  * %ProgramData%\GuestConfig

### <a name="linux-agent-installation-details"></a>Detalles de instalación del agente de Linux

El agente de Connected Machine de Linux se proporciona en el formato de paquete preferido para la distribución (.RPM o .DEB) que se hospeda en el [repositorio de paquetes](https://packages.microsoft.com/) de Microsoft. El agente se instala y configura con el paquete de scripts de shell [Install_linux_azcmagent.sh](https://aka.ms/azcmagent).

Después de instalar el agente de Connected Machine para Linux, se aplican los siguientes cambios de configuración a todo el sistema.

* Durante la instalación se crean las carpetas de instalación siguientes.

    |Carpeta |Descripción |
    |-------|------------|
    |/opt/azcmagent/ |Archivos ejecutables del servicio de metadatos de instancia y la CLI de azcmagent.|
    |/opt/GC_Ext/ | Archivos ejecutables del servicio de extensión.|
    |/opt/GC_Service/ |Archivos ejecutables del servicio de configuración de invitado (directiva).|
    |/var/opt/azcmagent/ |Archivos de token de configuración, registro e identidad para la CLI de azcmagent y el servicio de metadatos de instancia.|
    |/var/lib/GuestConfig/ |Descargas de paquetes de extensión, descargas de definiciones de configuración de invitado (directiva) y registros para los servicios de configuración de extensión y de invitado.|

* Los demonios siguientes se crean en el equipo de destino durante la instalación del agente.

    |Nombre del servicio |Nombre para mostrar |Nombre del proceso |Descripción |
    |-------------|-------------|-------------|------------|
    |himdsd.service |Servicio del Agente de Azure Connected Machine |himds |Este servicio implementa el servicio de metadatos de instancia híbrida (IMDS) para administrar la conexión a Azure y la identidad de Azure de la máquina conectada.|
    |gcad.service |Servicio de Arc de GC |gc_linux_service |Supervisa la configuración del estado deseado de la máquina. |
    |extd.service |Servicio de extensión |gc_linux_service | Instala las extensiones necesarias que tienen como destino la máquina.|

* Hay varios archivos de registro disponibles para la solución de problemas. Se describen en la tabla siguiente.

    |Log |Descripción |
    |----|------------|
    |/var/opt/azcmagent/log/himds.log |Registra los detalles del componente de agente de identidad y latido.|
    |/var/opt/azcmagent/log/azcmagent.log |Contiene la salida de los comandos de la herramienta azcmagent.|
    |/var/lib/GuestConfig/arc_policy_logs |Registra los detalles del componente del agente de configuración de invitado (directiva).|
    |/var/lib/GuestConfig/ext_mgr_logs |Registra los detalles del componente de agente de extensión.|
    |/var/lib/GuestConfig/extension_logs|Registra los detalles de las operaciones de instalación, actualización o desinstalación de la extensión.|

* Las variables de entorno siguientes se crean durante la instalación del agente. Estas variables se establecen en `/lib/systemd/system.conf.d/azcmagent.conf`.

    |Nombre |Valor predeterminado |Descripción |
    |-----|--------------|------------|
    |IDENTITY_ENDPOINT |<`http://localhost:40342/metadata/identity/oauth2/token`> ||
    |IMDS_ENDPOINT |<`http://localhost:40342`> ||

* Durante la desinstalación del agente, no se quitan los artefactos siguientes.

  * /var/opt/azcmagent
  * /var/lib/GuestConfig

### <a name="agent-resource-governance"></a>Gobernanza de recursos de agente

El agente de Connected Machine en servidores habilitados para Azure Arc está diseñado para administrar el consumo de recursos del sistema y del agente. El agente se acerca a la gobernanza de recursos bajo las siguientes condiciones:

* El agente de configuración de invitado está limitado a usar hasta un 5 % de la CPU para evaluar las directivas.
* El agente del servicio de extensión está limitado a usar hasta un 5 % de la CPU para instalar y administrar extensiones.

  * Una vez instalada, cada extensión se limita a usar hasta un 5 % de la CPU mientras se ejecuta. Por ejemplo, si tiene dos extensiones instaladas, pueden usar un total combinado del 10 % de la CPU.
  * Se permite que el agente de Log Analytics y el agente de Azure Monitor usen hasta el 60 % de la CPU durante sus operaciones de instalación, actualización o desinstalación en Red Hat Linux, CentOS y otras variantes de Linux empresariales. El límite es mayor con esta combinación de extensiones y sistemas operativos para poder hacer frente al impacto en el rendimiento de [SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux) en estos sistemas.

## <a name="next-steps"></a>Pasos siguientes

* Para empezar a evaluar los servidores habilitados para Azure Arc, siga el artículo [Conexión de máquinas híbridas con servidores habilitados para Azure Arc](learn/quick-enable-hybrid-vm.md).

* Antes de implementar el agente de servidores habilitados para Azure Arc y realizar la integración con otros servicios de administración y supervisión de Azure, revise la [guía de planificación e implementación](plan-at-scale-deployment.md).

* Puede encontrar información sobre la solución de problemas en la guía [Solución de problemas de conexión del agente de Connected Machine](troubleshoot-agent-onboard.md).
