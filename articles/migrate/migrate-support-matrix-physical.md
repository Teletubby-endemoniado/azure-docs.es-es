---
title: Compatibilidad con la detección y la evaluación físicas en Azure Migrate
description: 'Aprenda sobre la compatibilidad con la detección y la evaluación físicas con Azure Migrate: Discovery and assessment.'
author: Vikram1988
ms.author: vibansa
ms.manager: abhemraj
ms.topic: conceptual
ms.date: 03/18/2021
ms.openlocfilehash: 808f7a23a0be389703f2a805407b6ce725ce920a
ms.sourcegitcommit: 851b75d0936bc7c2f8ada72834cb2d15779aeb69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2021
ms.locfileid: "123315559"
---
# <a name="support-matrix-for-physical-server-discovery-and-assessment"></a>Matriz de compatibilidad con la detección y la evaluación físicas de servidores 

En este artículo se resumen los requisitos previos y los requisitos de soporte técnico al evaluar servidores físicos para migrarlos a Azure mediante la herramienta [Azure Migrate: Discovery and assessment](migrate-services-overview.md#azure-migrate-discovery-and-assessment-tool). Si quiere migrar servidores físicos a Azure, revise la [matriz de compatibilidad para la migración](migrate-support-matrix-physical-migration.md).

Para evaluar los servidores físicos, creará un proyecto y le agregará la herramienta Azure Migrate: Discovery and assessment. Después de agregarla, implemente el [dispositivo de Azure Migrate](migrate-appliance.md). El dispositivo detecta continuamente los servidores locales y envía sus metadatos y datos de rendimiento a Azure. Una vez completada la detección, recopile los servidores detectados en grupos y ejecute una evaluación de un grupo.

## <a name="limitations"></a>Limitaciones

**Soporte técnico** | **Detalles**
--- | ---
**Límites de evaluación** | Puede detectar y evaluar hasta 35 000 servidores físicos en un solo [proyecto](migrate-support-matrix.md#project).
**Límites del proyecto** | Puede crear varios proyectos en una suscripción a Azure. Además de los servidores físicos, un proyecto puede incluir servidores de VMware y de Hyper-V, hasta sus límites de evaluación.
**Detección** | El dispositivo de Azure Migrate puede detectar hasta 1000 servidores físicos.
**Valoración** | Puede agregar hasta 35 000 servidores en un solo grupo.<br/><br/> Puede evaluar hasta 35 000 máquinas virtuales en una única evaluación.

[Más información](concepts-assessment-calculation.md) sobre las evaluaciones.

## <a name="physical-server-requirements"></a>Requisitos del servidor físico

**Implementación del servidor físico:** El servidor físico puede ser independiente o implementarse en un clúster.

**Tipo de servidores:** Servidores sin sistema operativo, servidores virtualizados que se ejecutan en el entorno local on en entornos de nube, como AWS, GCP, Xen, etc.
>[!Note]
> Actualmente, Azure Migrate no admite la detección de servidores paravirtualizados. 

**Sistema operativo:** Todos los sistemas operativos Windows y Linux se pueden evaluar para la migración.

**Permisos:**

Configure una cuenta que el dispositivo pueda usar para acceder a los servidores físicos.

**Servidores Windows**

- En el caso de servidores Windows, use una cuenta de dominio para los servidores que se hayan unido a un dominio y una cuenta local para los que no. 
- Debe agregar la cuenta de usuario a estos grupos: Usuarios de administración remota, Usuarios de Monitor de rendimiento y Usuarios del registro de rendimiento. 
- Si el grupo Usuarios de administración remota no está presente, agregue la cuenta de usuario al grupo: **WinRMRemoteWMIUsers_** .
- La cuenta necesita estos permisos para que el dispositivo cree una conexión CIM con el servidor y extraiga los metadatos de configuración y rendimiento necesarios de las clases WMI enumeradas [aquí](migrate-appliance.md#collected-data---physical).
- En algunos casos, es posible que agregar la cuenta a estos grupos no devuelva los datos necesarios de las clases WMI, ya que la cuenta podría estar filtrada por [UAC](/windows/win32/wmisdk/user-account-control-and-wmi). Para superar el filtrado de UAC, la cuenta de usuario debe tener los permisos necesarios en el espacio de nombres CIMV2 y en los subespacios de nombres en el servidor de destino. Puede seguir [estos](troubleshoot-appliance.md#access-is-denied-error-occurs-when-you-connect-to-physical-servers-during-validation) pasos para habilitar los permisos obligatorios.

    > [!Note]
    > En el caso de Windows Server 2008 y 2008 R2, asegúrese de que WMF 3.0 esté instalado en los servidores.

**Servidores Linux**

- Necesita una cuenta raíz en los servidores que desee detectar. Como alternativa, puede proporcionar permisos sudo a una cuenta de usuario.
- La compatibilidad para agregar una cuenta de usuario con acceso sudo se proporciona de manera predeterminada con el nuevo script del instalador del dispositivo descargado del portal después del 20 de julio de 2021.
- En el caso de dispositivos anteriores, puede seguir estos pasos para habilitar la funcionalidad:
    1. En el servidor que ejecuta el dispositivo, abra el Editor del Registro.
    1. Vaya a HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureAppliance.
    1. Cree una clave del Registro "isSudo" con un valor de DWORD de 1.

    :::image type="content" source="./media/tutorial-discover-physical/issudo-reg-key.png" alt-text="Captura de pantalla que muestra cómo habilitar la compatibilidad con sudo.":::

- Para detectar los metadatos de configuración y rendimiento del servidor de destino, debe habilitar el acceso sudo para los comandos enumerados [aquí](migrate-appliance.md#linux-server-metadata). Asegúrese de que ha habilitado "NOPASSWD" para que la cuenta ejecute los comandos necesarios sin solicitar una contraseña cada vez que se invoque el comando sudo.
- Las siguientes distribuciones del sistema operativo Linux son compatibles con la detección de Azure Migrate mediante una cuenta con acceso sudo:

    Sistema operativo | Versiones 
    --- | ---
    Red Hat Enterprise Linux | 6,7,8
    Cent OS | 6.6, 8.2
    Ubuntu | 14.04,16.04,18.04
    SUSE Linux | 11.4, 12.4
    Debian | 7, 10
    Amazon Linux | 2.0.2021
    CoreOS Container | 2345.3.0

- Si no puede proporcionar acceso sudo a la cuenta raíz o a la cuenta de usuario, puede establecer la clave del Registro "isSudo" en el valor "0" en el Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureAppliance y proporcionar una cuenta no raíz con las funcionalidades necesarias mediante los siguientes comandos:

**Comando** | **Propósito**
--- | --- |
setcap CAP_DAC_READ_SEARCH+eip /usr/sbin/fdisk <br></br> setcap CAP_DAC_READ_SEARCH+eip /sbin/fdisk _(si /usr/sbin/fdisk no está presente)_ | Recopilar datos de configuración del disco
setcap "cap_dac_override,cap_dac_read_search,cap_fowner,cap_fsetid,cap_setuid,<br>cap_setpcap,cap_net_bind_service,cap_net_admin,cap_sys_chroot,cap_sys_admin,<br>cap_sys_resource,cap_audit_control,cap_setfcap=+eip" /sbin/lvm | Recopilar datos de rendimiento del disco
setcap CAP_DAC_READ_SEARCH+eip /usr/sbin/dmidecode | Recopilar el número de serie del BIOS
chmod a+r /sys/class/dmi/id/product_uuid | Recopilar el GUID del BIOS

## <a name="azure-migrate-appliance-requirements"></a>Requisitos del dispositivo de Azure Migrate

Azure Migrate usa el [dispositivo de Azure Migrate](migrate-appliance.md) para la detección y la evaluación. El dispositivo para servidores físicos se puede ejecutar en una máquina virtual o en un servidor físico.

- Obtenga información sobre los [requisitos del dispositivo](migrate-appliance.md#appliance---physical) para los servidores físicos.
- Obtenga información sobre las direcciones URL a las que el dispositivo necesita acceder en nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).
- Este dispositivo se configura mediante un [script de PowerShell](how-to-set-up-appliance-physical.md) que se descarga de Azure Portal.
En Azure Government, debe implementar el dispositivo [mediante este script](deploy-appliance-script-government.md).

## <a name="port-access"></a>Acceso a puertos

En la tabla siguiente se resumen los requisitos de los puertos para la evaluación.

**Dispositivo** | **Connection**
--- | ---
**Dispositivo** | Conexiones entrantes en el puerto TCP 3389 para permitir las conexiones del Escritorio remoto al dispositivo.<br/><br/> Conexiones entrantes en el puerto 44368 para tener acceso de forma remota a la aplicación de administración del dispositivo mediante la dirección URL: ``` https://<appliance-ip-or-name>:44368 ```.<br/><br/> Conexiones salientes en el puerto 443 (HTTPS) para enviar metadatos de detección y rendimiento a Azure Migrate.
**Servidores físicos** | **Windows:** conexiones entrantes en el puerto 5985 (HTTP) de WinRM para extraer los metadatos de configuración y rendimiento de los servidores Windows. <br/><br/> **Linux:**  conexiones entrantes en el puerto 22 (TCP) para extraer los metadatos de configuración y rendimiento de los servidores Linux. |

## <a name="agent-based-dependency-analysis-requirements"></a>Requisitos para el análisis de dependencia con agentes

El [análisis de dependencias](concepts-dependency-visualization.md) le ayuda a identificar las dependencias entre los servidores locales que quiere evaluar y migrar a Azure. En esta tabla se resumen los requisitos para configurar el análisis de dependencia con agentes. Actualmente solo se admite el análisis de dependencias basadas en agentes para los servidores físicos.

**Requisito** | **Detalles**
--- | ---
**Antes de la implementación** | Debe tener un proyecto al que se le haya agregado la herramienta Azure Migrate: Discovery and assessment.<br/><br/>  La visualización de dependencias se implementa después de configurar un dispositivo de Azure Migrate para detectar los servidores locales.<br/><br/> [Aprenda](create-manage-projects.md) a crear un proyecto por primera vez.<br/> [Aprenda](how-to-assess.md) a agregar una herramienta de evaluación a un proyecto existente.<br/> Aprenda a configurar un dispositivo de Azure Migrate para la evaluación de [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md) o de servidores físicos.
**Azure Government** | La visualización de dependencias no está disponible en Azure Government.
**Log Analytics** | Azure Migrate utiliza la solución [Service Map](../azure-monitor/vm/service-map.md) de los [registros de Azure Monitor](../azure-monitor/logs/log-query-overview.md) para la visualización de dependencias.<br/><br/> Puede asociar a un proyecto un área de trabajo de Log Analytics nueva o existente. El área de trabajo de un proyecto no se puede modificar una vez que se ha agregado. <br/><br/> El área de trabajo debe estar en la misma suscripción que el proyecto.<br/><br/> El área de trabajo debe residir en las regiones Este de EE. UU., Sudeste Asiático u Oeste de Europa. Las áreas de trabajo de otras regiones no se pueden asociar a un proyecto.<br/><br/> El área de trabajo debe estar en una región en la que [se admita Service Map](../azure-monitor/vm/vminsights-configure-workspace.md#supported-regions).<br/><br/> En Log Analytics, el área de trabajo asociada con Azure Migrate se etiqueta con la clave del proyecto de migración y el nombre del proyecto.
**Agentes necesarios** | En cada servidor que quiera analizar, instale los siguientes agentes:<br/><br/> [Microsoft Monitoring agent (MMA)](../azure-monitor/agents/agent-windows.md).<br/> [Dependency Agent](../azure-monitor/agents/agents-overview.md#dependency-agent).<br/><br/> Si los servidores locales no están conectados a Internet, tiene que descargar e instalar la puerta de enlace de Log Analytics en ellas.<br/><br/> Conozca más información sobre la instalación de [Dependency Agent](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) y [MMA](how-to-create-group-machine-dependencies.md#install-the-mma).
**Área de trabajo de Log Analytics** | El área de trabajo debe estar en la misma suscripción que el proyecto.<br/><br/> Azure Migrate admite áreas de trabajo que residen en las regiones Este de EE. UU., Sudeste Asiático y Oeste de Europa.<br/><br/>  El área de trabajo debe estar en una región en la que [se admita Service Map](../azure-monitor/vm/vminsights-configure-workspace.md#supported-regions).<br/><br/> El área de trabajo de un proyecto no se puede modificar una vez que se ha agregado.
**Costos** | La solución Service Map no genera ningún gasto durante los primeros 180 días (a partir del día que asocie el área de trabajo de Log Analytics al proyecto).<br/><br/> Transcurridos los 180 días, se aplicarán las tarifas normales de Log Analytics.<br/><br/> Si se usa una solución que no sea Service Map en el área de trabajo de Log Analytics asociada generará los [gastos estándar](https://azure.microsoft.com/pricing/details/log-analytics/) de Log Analytics.<br/><br/> Cuando se elimina el proyecto, el área de trabajo no se elimina con él. Tras la eliminación del proyecto, el uso de Service Map no será gratuito y cada nodo se facturará conforme al nivel de pago del área de trabajo de Log Analytics.<br/><br/>Si tiene proyectos creados antes de que Azure Migrate estuviera disponible con carácter general (el 28 de febrero de 2018), puede que haya incurrido en cargos de Service Map adicionales. Para garantizar el pago solo después de 180 días, es recomendable que cree un nuevo proyecto, ya que las áreas de trabajo existentes antes de la disponibilidad general siguen siendo facturables.
**Administración** | Al registrar agentes en el área de trabajo, use el identificador y la clave proporcionados por el proyecto.<br/><br/> Puede usar el área de trabajo de Log Analytics fuera de Azure Migrate.<br/><br/> Si elimina el proyecto asociado, el área de trabajo no se elimina automáticamente. [Elimínela manualmente](../azure-monitor/logs/manage-access.md).<br/><br/> No elimine el área de trabajo creada por Azure Migrate, a menos que elimine el proyecto. Si lo hace, la característica de visualización de dependencias no funcionará según lo previsto.
**Conectividad de Internet** | Si los servidores no están conectados a Internet, debe instalar la puerta de enlace de Log Analytics en ellos.
**Azure Government** | El análisis de dependencias basado en agente no se admite.

## <a name="next-steps"></a>Pasos siguientes

[Preparación de la detección y la evaluación físicas](./tutorial-discover-physical.md).