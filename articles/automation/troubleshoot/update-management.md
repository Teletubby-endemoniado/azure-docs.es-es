---
title: Resolución de problemas de Update Management de Azure Automation
description: En este artículo se describe cómo solucionar y resolver problemas con Update Management de Azure Automation.
services: automation
ms.subservice: update-management
ms.date: 06/10/2021
ms.topic: troubleshooting
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: bf9804b0881e02b1a4f58e5923c33840d06e37e2
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129706470"
---
# <a name="troubleshoot-update-management-issues"></a>Solución de problemas de Update Management

En este artículo se describen los problemas que puede experimentar al usar la característica Update Management para evaluar y administrar actualizaciones en sus máquinas. Existe un agente solucionador de problemas para que el agente de Hybrid Runbook Worker determine el problema subyacente. Para obtener más información sobre el solucionador de problemas, consulte [Solución de problemas del agente de actualización de Windows](update-agent-issues.md) y [Solución de problemas del agente de actualización de Linux](update-agent-issues-linux.md). Si tiene otros problemas de implementación de características, consulte [Solucionar problemas de implementación de características](onboarding.md).

>[!NOTE]
>Si encuentra problemas al implementar Update Management en una máquina Windows, abra el Visor de eventos de Windows y compruebe el registro de eventos de **Operations Manager** en los **registros de aplicaciones y servicios** en la máquina local. Busque eventos con el id. de evento 4502 y detalles del evento que contengan `Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent`.

## <a name="scenario-windows-defender-update-always-show-as-missing"></a><a name="windows-defender-update-missing-status"></a>Ejemplo: la actualización de Windows Defender siempre se muestra como ausente.

### <a name="issue"></a>Incidencia

La actualización de definiciones de Windows Defender (**KB2267602**) siempre se muestra como ausente en una evaluación cuando se instala y se muestra como actualizada en el momento de comprobarla en el historial de Windows Update.

### <a name="cause"></a>Causa

Las actualizaciones de definiciones se publican varias veces en un solo día. Como resultado, puede ver varias versiones de KB2267602 publicadas en un solo día, pero con un id. de actualización y una versión diferentes.

La evaluación de Update Management se ejecuta una vez cada 11 horas. En este ejemplo, a las 10:00 se ejecutó una evaluación y la versión 1.237.316.0 estaba disponible en ese momento. Al buscar la tabla **Actualización** en el área de trabajo de Log Analytics, la actualización de la definición 1.237.316.0 se muestra con un valor de **updateState** establecido en **Necesario**. Si una implementación programada se ejecuta unas horas más tarde (supongamos que a las 13:00) y la versión 1.237.316.0 o una versión más reciente sigue estando disponible, se instalará la versión más reciente; así pues, este proceso se reflejará en el registro escrito en la tabla **UpdateRunProgress**. Sin embargo, en la tabla **Actualización** todavía se mostrará la versión 1.237.316.0 como **Necesaria** hasta que se ejecute la siguiente evaluación. Cuando la evaluación se ejecuta de nuevo, es posible que no haya una actualización de definición más reciente disponible, por lo que la tabla **Actualización** no mostrará la versión de actualización de definición 1.237.316.0 como ausente o una versión más reciente disponible según sea necesario. Debido a la frecuencia de las actualizaciones de definiciones, se podrían devolver varias versiones en la búsqueda de registros. 

### <a name="resolution"></a>Solución

Ejecute la siguiente consulta de registro para confirmar que las actualizaciones de definiciones instaladas se notifican correctamente. Esta consulta devuelve el tiempo generado, la versión y el id. de actualización de KB2267602 en la tabla **Actualizaciones**. Reemplace el valor de *Equipo* por el nombre completo de la máquina.

```kusto
Update
| where TimeGenerated > ago(14h) and OSType != "Linux" and (Optional == false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((
    Heartbeat
    | where TimeGenerated > ago(12h) and OSType =~ "Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by Computer, SourceComputerId, UpdateID
| where UpdateState =~ "Needed" and Approved != false and Computer == "<computerName>"
| render table
```

Los resultados de la consulta deben devolver un valor similar al siguiente:

:::image type="content" source="./media/update-management/example-query-updates-table.png" alt-text="Ejemplo que muestra los resultados de la consulta de registro de la tabla Actualizaciones.":::

Ejecute la siguiente consulta de registro para obtener el tiempo generado, la versión y el id. de actualización de KB2267602 en la tabla **UpdatesRunProgress**. Esta consulta le permitirá saber si se instaló desde Update Management o si se instaló automáticamente en la máquina desde Microsoft Update. Debe reemplazar el valor de *CorrelationId* por el GUID del trabajo de runbook (es decir, el valor de la propiedad **MasterJOBID** del trabajo de runbook **Patch-MicrosoftOMSComputer**) para la actualización, y *SourceComputerId* por el GUID de la máquina.

```kusto
UpdateRunProgress
| where OSType!="Linux" and CorrelationId=="<master job id>" and SourceComputerId=="<source computer id>"
| summarize arg_max(TimeGenerated, Title, InstallationStatus) by UpdateId
| project TimeGenerated, id=UpdateId, displayName=Title, InstallationStatus
```

Los resultados de la consulta deben devolver un valor similar al siguiente:

:::image type="content" source="./media/update-management/example-query-updaterunprogress-table.png" alt-text="Ejemplo que muestra los resultados de la consulta de registro de la tabla UpdatesRunProgress.":::

Si el valor **TimeGenerated** de los resultados de la consulta de registro de la tabla **Actualizaciones** es anterior a la marca de tiempo (es decir, el valor de **TimeGenerated**) de la instalación de la actualización en el equipo o de los resultados de la consulta de registro de la tabla **UpdateRunProgress**, espere a la siguiente valoración. A continuación, vuelva a ejecutar la consulta de registro en la tabla **Actualizaciones**. No aparecerá una actualización de KB2267602 o aparecerá con una versión más reciente. Sin embargo, si después de realizar la evaluación más reciente la misma versión aparece como **Necesaria** en la tabla **Actualizaciones** pero resulta que ya está instalada, debe abrir un incidente de soporte técnico de Azure.

## <a name="scenario-linux-updates-shown-as-pending-and-those-installed-vary"></a><a name="updates-linux-installed-different"></a>Escenario: Las actualizaciones de Linux se muestran como pendientes y las que se instalan varían

### <a name="issue"></a>Incidencia

En el caso de la máquina Linux, Update Management muestra actualizaciones específicas disponibles bajo la clasificación **Seguridad** y **Otras**. Pero cuando se ejecuta una programación de actualización en el equipo, por ejemplo, para instalar solo las actualizaciones que coincidan con la clasificación **Seguridad**, las actualizaciones instaladas son diferentes de un subconjunto de actualizaciones que se muestran anteriormente que coinciden con esa clasificación.

### <a name="cause"></a>Causa

Cuando se realiza una evaluación de las actualizaciones del sistema operativo pendientes para la máquina Linux, Update Management utiliza los archivos [Open Vulnerability and Assessment Language ](https://oval.mitre.org/) (OVAL) proporcionados por el proveedor de distribución de Linux para la clasificación. La categorización se realiza para las actualizaciones de Linux como **Seguridad** u **Otras**, en función de los archivos OVAL, que indica que las actualizaciones tratan problemas de seguridad o vulnerabilidades. Sin embargo, cuando se ejecuta la programación de actualización, lo hace en la máquina Linux mediante el administrador de paquetes adecuado, como YUM, APT o ZYPPER, para instalarlas. El administrador de paquetes para la distribución de Linux puede tener un mecanismo diferente para clasificar las actualizaciones, donde los resultados pueden diferir de los que se obtienen de los archivos OVAL mediante Update Management.

### <a name="resolution"></a>Solución

Puede comprobar manualmente el equipo Linux, las actualizaciones aplicables y su clasificación según el administrador de paquetes de la distribución. Para saber qué actualizaciones clasifica el administrador de paquetes como **Seguridad**, ejecute los siguientes comandos.

En el caso de YUM, el comando siguiente devuelve una lista de actualizaciones distinta de cero categorizadas como **Seguridad** por Red Hat. Tenga en cuenta que en el caso de CentOS, siempre devuelve una lista vacía y no se produce ninguna clasificación de seguridad.

```bash
sudo yum -q --security check-update
```

En el caso de ZYPPER, el siguiente comando devuelve una lista de actualizaciones distinta de cero categorizadas como **Seguridad** por SUSE.

```bash
sudo LANG=en_US.UTF8 zypper --non-interactive patch --category security --dry-run
```

En el caso de APT, el siguiente comando devuelve una lista de actualizaciones distinta de cero categorizadas como **Seguridad** por las distribuciones Canonical y Ubuntu Linux.

```bash
sudo grep security /etc/apt/sources.list > /tmp/oms-update-security.list LANG=en_US.UTF8 sudo apt-get -s dist-upgrade -oDir::Etc::Sourcelist=/tmp/oms-update-security.list
```

A partir de esta lista, después puede ejecutar el comando `grep ^Inst` para obtener todas las actualizaciones de seguridad pendientes.

## <a name="scenario-you-receive-the-error-failed-to-enable-the-update-solution"></a><a name="failed-to-enable-error"></a>Escenario: Recibe el error "No se pudo habilitar la solución de actualización"

### <a name="issue"></a>Incidencia

Al intentar habilitar la Update Management en su cuenta de Automation, recibe el error siguiente:

```error
Error details: Failed to enable the Update solution
```

### <a name="cause"></a>Causa

Este error puede ocurrir debido a uno de los siguientes motivos:

* Es posible que los requisitos del firewall de red para el agente de Log Analytics no estén configurados correctamente. Esta situación puede provocar que el agente no pueda resolver las direcciones URL de DNS.

* El destino de Update Management no está configurado correctamente y la máquina no recibe las actualizaciones según lo esperado.

* Es posible que también note que la máquina muestra un estado de `Non-compliant` en **Cumplimiento**. Al mismo tiempo, **Análisis de escritorio del agente** informa el agente como `Disconnected`.

### <a name="resolution"></a>Solución

* Ejecute el solucionador de problemas para [Windows](update-agent-issues.md#troubleshoot-offline) o [Linux](update-agent-issues-linux.md#troubleshoot-offline), según el sistema operativo.

* Vaya a [Configuración de red](../automation-hybrid-runbook-worker.md#network-planning) para obtener información acerca de qué direcciones y puertos deben permitirse para que Update Management funcione.  

* Compruebe si hay problemas de configuración de ámbito. La [configuración de ámbito](../update-management/scope-configuration.md) determina qué máquinas se configuran para Update Management. Si la máquina aparece en el área de trabajo, pero no se muestra en Update Management, debe establecer la configuración del ámbito para dirigirse a las máquinas. Para obtener información sobre la configuración de ámbito, consulte [Habilitación de máquinas en el área de trabajo](../update-management/enable-from-automation-account.md#enable-machines-in-the-workspace).

* Para quitar la configuración de trabajo, siga los pasos descritos en [Eliminación de la instancia de Hybrid Runbook Worker de un equipo Windows local](../automation-windows-hrw-install.md#remove-windows-hybrid-runbook-worker) o [Eliminación de la instancia de Hybrid Runbook Worker de un equipo Linux local](../automation-linux-hrw-install.md#remove-linux-hybrid-runbook-worker).

## <a name="scenario-superseded-update-indicated-as-missing-in-update-management"></a>Escenario: En Update Management se indica que falta una actualización reemplazada

### <a name="issue"></a>Problema

Se indica que faltan actualizaciones antiguas en la cuenta de Automation, aunque se han reemplazado. Una actualización reemplazada es aquella que no es necesario instalar, puesto que hay una posterior disponible que corrige la misma vulnerabilidad. Update Management omite la actualización reemplazada y no la convierte en aplicable en favor de la actualización que la reemplaza. Para obtener información sobre un problema relacionado, vea [Actualización reemplazada](/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer).

### <a name="cause"></a>Causa

Las actualizaciones reemplazadas no se rechazan en Windows Server Update Services (WSUS), de modo que se puedan considerar no aplicables.

### <a name="resolution"></a>Resolución

Cuando una actualización reemplazada sea no aplicable al cien por cien, debe cambiar su estado de aprobación a `Declined` en WSUS. Para cambiar el estado de aprobación de todas las actualizaciones:

1. En la cuenta de Automation, seleccione **Update Management** para ver el estado de las máquinas. Vea [Visualización de la evaluación de la actualización](../update-management/view-update-assessments.md).

2. Compruebe la actualización reemplazada para asegurarse de que no es aplicable al cien por cien.

3. En el servidor WSUS del que dependen las máquinas, [rechace la actualización](/windows-server/administration/windows-server-update-services/manage/updates-operations#declining-updates).

4. Seleccione **Equipos** y, en la columna **Cumplimiento**, fuerce un nuevo examen de cumplimiento. Consulte [Administración de actualizaciones para máquinas virtuales](../update-management/manage-updates-for-vm.md).

5. Repita los pasos anteriores para otras actualizaciones reemplazadas.

6. En Windows Server Update Services (WSUS), limpie manualmente todas las actualizaciones reemplazadas a fin de actualizar la infraestructura mediante el [Asistente para liberar espacio del servidor](/windows-server/administration/windows-server-update-services/manage/the-server-cleanup-wizard) de WSUS.

7. Repita este procedimiento con regularidad para corregir el problema de visualización y minimizar la cantidad de espacio en disco que se usa para la administración de actualizaciones.

## <a name="scenario-machines-dont-show-up-in-the-portal-under-update-management"></a><a name="nologs"></a>Escenario: Las máquinas no se muestran en el portal en Update Management

### <a name="issue"></a>Problema

Los equipos presentan estos síntomas:

* La máquina muestra `Not configured` en la vista de Update Management de una máquina virtual.

* Faltan las máquinas en la vista de Update Management de su cuenta de Azure Automation.

* Tiene máquinas que se muestran como `Not assessed` en **Cumplimiento**. Sin embargo, los datos de latido se ven en registros de Azure Monitor para la Hybrid Runbook Worker, pero no para Update Management.

### <a name="cause"></a>Causa

Este problema puede deberse a problemas de configuración local o a que la configuración de ámbito no se ha realizado correctamente. Las posibles causas específicas son:

* Es posible que tenga que volver a registrar y reinstalar Hybrid Runbook Worker.

* Es posible que se haya alcanzado una cuota definida en el área de trabajo y que impida el almacenamiento de datos adicional.

### <a name="resolution"></a>Solución

1. Ejecute el solucionador de problemas para [Windows](update-agent-issues.md#troubleshoot-offline) o [Linux](update-agent-issues-linux.md#troubleshoot-offline), según el sistema operativo.

2. Asegúrese de que las notificaciones de la máquina se envían al área de trabajo correcta. Para obtener instrucciones sobre cómo comprobar este aspecto, consulte [Comprobación de la conectividad del agente a Azure Monitor](../../azure-monitor/agents/agent-windows.md#verify-agent-connectivity-to-azure-monitor). Asegúrese también de que esta área de trabajo esté vinculada a su cuenta de Azure Automation. Para ello, vaya a la cuenta de Automation y seleccione **Área de trabajo vinculada** en **Recursos relacionados**.

3. Asegúrese de que la máquina se muestre en el área de trabajo de Log Analytics vinculada a la cuenta de Automation. Ejecute la siguiente consulta en el área de trabajo de Log Analytics.

   ```kusto
   Heartbeat
   | summarize by Computer, Solutions
   ```

    Si no ve la máquina en los resultados de la consulta, significa que no se ha registrado recientemente. Probablemente haya un problema de configuración local y debe [volver a instalar el agente](../../azure-monitor/agents/agent-windows.md).

    Si el equipo aparece en los resultados de la consulta, compruebe en la propiedad **Solutions** que aparezca **updates**. Así se comprueba si está registrado en Update Management. Si no es así, compruebe si hay problemas de configuración de ámbito. La [configuración de ámbito](../update-management/scope-configuration.md) determina qué máquinas se configuran para Update Management. Para configurar el ámbito para el equipo de destino, vea [Habilitación de máquinas en el área de trabajo](../update-management/enable-from-automation-account.md#enable-machines-in-the-workspace).

4. En el área de trabajo, ejecute esta consulta.

   ```kusto
   Operation
   | where OperationCategory == 'Data Collection Status'
   | sort by TimeGenerated desc
   ```

   Si recibe el resultado `Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota`, significa que la cuota definida en el área de trabajo se ha alcanzado, lo que ha impedido que se guarden los datos. En el área de trabajo, vaya a **Administración del volumen de datos** en **Usos y costos estimados** y cambie o elimine su cuota.

5. Si el problema no se resuelve, siga los pasos descritos en [Implementación de Hybrid Runbook Worker en Windows](../automation-windows-hrw-install.md) para volver a instalar Hybrid Worker para Windows. Para Linux, siga los pasos que aparecen en [Implementación de Hybrid Runbook Worker en Linux](../automation-linux-hrw-install.md).

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>Escenario: No se puede registrar el proveedor de recursos de Automation para las suscripciones

### <a name="issue"></a>Problema

Al trabajar con implementaciones de características en la cuenta de Automation, se produce el siguiente error:

```error
Error details: Unable to register Automation Resource Provider for subscriptions
```

### <a name="cause"></a>Causa

El proveedor de recursos de Automation no está registrado en la suscripción.

### <a name="resolution"></a>Solución

Para registrar el proveedor de recursos de Automation, realice los pasos siguientes en Azure Portal.

1. En la lista de servicios de Azure de la parte inferior del portal, seleccione **Todos los servicios** y, a continuación, seleccione **Suscripciones** en el grupo de servicios General.

2. Seleccione su suscripción.

3. En **Configuración**, seleccione **Proveedores de recursos**.

4. En la lista de proveedores de recursos, compruebe que el proveedor de recursos de Microsoft.Automation esté registrado.

5. Si no aparece, registre el proveedor de Microsoft.Automation siguiendo los pasos descritos en [Resolución de errores del registro del proveedor de recursos](../../azure-resource-manager/templates/error-register-resource-provider.md).

## <a name="scenario-scheduled-update-did-not-patch-some-machines"></a><a name="scheduled-update-missed-machines"></a>Escenario: La actualización programada no ha actualizado algunas máquinas

### <a name="issue"></a>Problema

No todas las máquinas incluidas en una versión preliminar de actualización aparecen en la lista de máquinas a las que se ha hecho una revisión durante una ejecución programada, o las máquinas virtuales para los ámbitos seleccionados de un grupo dinámico no aparecen en la lista de versión preliminar de actualización en el portal.

La lista de versión preliminar de actualización consta de todas las máquinas recuperadas por una consulta de [Azure Resource Graph](../../governance/resource-graph/overview.md) para los ámbitos seleccionados. Los ámbitos se filtran para las máquinas que tienen instalado Hybrid Runbook Worker y para las que tiene permisos de acceso.

### <a name="cause"></a>Causa

Este problema puede tener una de las siguientes causas:

* Las suscripciones definidas en el ámbito de una consulta dinámica no están configuradas para el proveedor de recursos de Automation registrado.

* Las máquinas no estaban disponibles o no tenían etiquetas adecuadas cuando se ejecutó la programación.

* No tiene el acceso correcto en los ámbitos seleccionados.

* La consulta de Azure Resource Graph no recupera las máquinas esperadas.

* El sistema Hybrid Runbook Worker no está instalado en las máquinas.

### <a name="resolution"></a>Solución

#### <a name="subscriptions-not-configured-for-registered-automation-resource-provider"></a>Suscripciones no configuradas para el proveedor de recursos de Automation registrado

Si la suscripción no está configurada para el proveedor de recursos de Automation, no podrá consultar ni capturar información en las máquinas de esa suscripción. Siga estos pasos para comprobar el registro de la suscripción.

1. En [Azure Portal](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), acceda a la lista de servicios de Azure.

2. Seleccione **Todos los servicios** y, después, **Suscripciones** en el grupo de servicios generales.

3. Busque la suscripción definida en el ámbito de la implementación.

4. En **Configuración**, elija **Proveedores de recursos**.

5. Compruebe que el proveedor de recursos de Microsoft.Automation esté registrado.

6. Si no aparece, registre el proveedor de Microsoft.Automation siguiendo los pasos descritos en [Resolución de errores del registro del proveedor de recursos](../../azure-resource-manager/templates/error-register-resource-provider.md).

#### <a name="machines-not-available-or-not-tagged-correctly-when-schedule-executed"></a>Máquinas no disponibles o no etiquetadas correctamente cuando se ejecutó la programación

Utilice el procedimiento siguiente si la suscripción está configurada para el proveedor de recursos de Automation, pero la ejecución de la programación de actualización con los [grupos dinámicos](../update-management/configure-groups.md) especificados perdió algunas máquinas.

1. En Azure Portal, abra la cuenta de Automation y seleccione **Update Management**.

2. Compruebe el [historial de Update Management](../update-management/deploy-updates.md#view-results-of-a-completed-update-deployment) para determinar la hora exacta en la que se ejecutó la implementación de actualizaciones.

3. En el caso de las máquinas en las que sospecha que ha perdido Update Management, use Azure Resource Graph (ARG) para [localizar los cambios en la máquina](../../governance/resource-graph/how-to/get-resource-changes.md#find-detected-change-events-and-view-change-details).

4. Busque cambios durante un período considerable, como un día, antes de ejecutar la implementación de la actualización.

5. Compruebe los resultados de la búsqueda para constatar si hay cambios sistémicos, como cambios de eliminación o actualización, en las máquinas en este período. Estos cambios pueden modificar el estado o las etiquetas de la máquina, de forma que las máquinas no se seleccionan en la lista de máquinas cuando se implementan las actualizaciones.

6. Ajuste la configuración de los recursos y las máquinas según sea necesario para corregir los problemas de estado o etiquetas de la máquina.

7. Vuelva a ejecutar la programación de actualización para asegurarse de que la implementación con los grupos dinámicos especificados incluya todas las máquinas.

#### <a name="incorrect-access-on-selected-scopes"></a>Acceso incorrecto en los ámbitos seleccionados

En Azure Portal solo se muestran las máquinas para las que tiene acceso de escritura en un ámbito determinado. Si no tiene el acceso correcto a un ámbito, consulte [Tutorial: Concesión de acceso de usuario a los recursos de Azure mediante Azure Portal](../../role-based-access-control/quickstart-assign-role-user-portal.md).

#### <a name="resource-graph-query-doesnt-return-expected-machines"></a>La consulta de Azure Resource Graph no devuelve las máquinas esperadas

Siga los pasos que se indican a continuación para averiguar si las consultas funcionan correctamente.

1. Ejecute una consulta de Azure Resource Graph con el formato que se muestra a continuación en la hoja de Resource Graph Explorer en Azure Portal. Si no lleva mucho tiempo usando Azure Resource Graph, consulte este [inicio rápido](../../governance/resource-graph/first-query-portal.md) para aprender a trabajar con el explorador de Resource Graph. Esta consulta imita los filtros seleccionados al crear el grupo dinámico en Update Management. Consulte [Uso de grupos dinámicos con Update Management](../update-management/configure-groups.md).

    ```kusto
    where (subscriptionId in~ ("<subscriptionId1>", "<subscriptionId2>") and type =~ "microsoft.compute/virtualmachines" and properties.storageProfile.osDisk.osType == "<Windows/Linux>" and resourceGroup in~ ("<resourceGroupName1>","<resourceGroupName2>") and location in~ ("<location1>","<location2>") )
    | project id, location, name, tags = todynamic(tolower(tostring(tags)))
    | where  (tags[tolower("<tagKey1>")] =~ "<tagValue1>" and tags[tolower("<tagKey2>")] =~ "<tagValue2>") // use this if "All" option selected for tags
    | where  (tags[tolower("<tagKey1>")] =~ "<tagValue1>" or tags[tolower("<tagKey2>")] =~ "<tagValue2>") // use this if "Any" option selected for tags
    | project id, location, name, tags
    ```

   Este es un ejemplo:

    ```kusto
    where (subscriptionId in~ ("20780d0a-b422-4213-979b-6c919c91ace1", "af52d412-a347-4bc6-8cb7-4780fbb00490") and type =~ "microsoft.compute/virtualmachines" and properties.storageProfile.osDisk.osType == "Windows" and resourceGroup in~ ("testRG","withinvnet-2020-01-06-10-global-resources-southindia") and location in~ ("australiacentral","australiacentral2","brazilsouth") )
    | project id, location, name, tags = todynamic(tolower(tostring(tags)))
    | where  (tags[tolower("ms-resource-usage")] =~ "azure-cloud-shell" and tags[tolower("temp")] =~ "temp")
    | project id, location, name, tags
    ```

2. Compruebe si las máquinas que está buscando aparecen en los resultados de la consulta.

3. Si las máquinas no aparecen en la lista, es probable que haya un problema con el filtro seleccionado en el grupo dinámico. Ajuste la configuración del grupo según sea necesario.

#### <a name="hybrid-runbook-worker-not-installed-on-machines"></a>Hybrid Runbook Worker no está instalado en las máquinas

Las máquinas aparecen en los resultados de la consulta de azure Resource Graph, pero todavía no se muestran en la versión preliminar del grupo dinámico. En este caso, es posible que las máquinas no estén designadas como instancias de Hybrid Worker del sistema y, por tanto, no puedan ejecutar trabajos de Azure Automation y Update Management. Para asegurarse de que las máquinas que espera ver estén configuradas como instancias de Hybrid Runbook Worker del sistema:

1. En Azure Portal, vaya a la cuenta de Automation de una máquina que no aparezca correctamente.

2. Seleccione **Grupos de Hybrid Worker** en **Automatización de procesos**.

3. Seleccione la pestaña **Grupos de Hybrid Worker del sistema**.

4. Compruebe que la instancia de Hybrid Worker está presente para esa máquina.

5. Si la máquina no está configurada como Hybrid Runbook Worker del sistema, revise los siguientes métodos para habilitar la máquina usando uno de ellos:

   - Desde su [cuenta de Automation](../update-management/enable-from-automation-account.md) para una o más máquinas de Azure o que no sean de Azure, incluidos servidores habilitados para Azure Arc.

   - Use el [runbook](../update-management/enable-from-runbook.md) **Enable-AutomationSolution** para automatizar la incorporación de VM de Azure.

   - Para una [VM de Azure concreta](../update-management/enable-from-vm.md), desde la página **Máquinas virtuales** en Azure Portal. Este escenario está disponible para las VM Linux y Windows.

   - Para [varias máquinas virtuales de Azure](../update-management/enable-from-portal.md), selecciónelas en la página **Máquinas virtuales** de Azure Portal.

   El método que se va a habilitar se basa en el entorno en el que se ejecuta la máquina.

6. Repita los pasos anteriores para todas las máquinas que no se han mostrado en la versión preliminar.

## <a name="scenario-update-management-components-enabled-while-vm-continues-to-show-as-being-configured"></a><a name="components-enabled-not-working"></a>Escenario: Componentes de Update Management habilitados, mientras la máquina virtual se sigue mostrando como configurada

### <a name="issue"></a>Problema

Continúa recibiendo el mensaje siguiente en una máquina virtual 15 minutos después de la implementación:

```error
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

### <a name="cause"></a>Causa

Este error puede ocurrir debido a uno de los siguientes motivos:

* Está bloqueada la comunicación con la cuenta de Automation.

* Hay un nombre de equipo duplicado con identificadores de equipo de origen diferentes. Este escenario se produce cuando se crea una máquina virtual con un nombre de equipo determinado en distintos grupos de recursos y envía notificaciones a la misma área de trabajo del agente de logística en la suscripción.

* La imagen de máquina virtual que se implementará puede provenir de una máquina clonada que no se haya preparado mediante la preparación del sistema (sysprep) con el agente de Log Analytics para Windows instalado.

### <a name="resolution"></a>Solución

Para ayudar a determinar el problema exacto con la VM, ejecute la consulta siguiente en el área de trabajo de Log Analytics que está vinculada a su cuenta de Automation.

```
Update
| where Computer contains "fillInMachineName"
| project TimeGenerated, Computer, SourceComputerId, Title, UpdateState 
```

#### <a name="communication-with-automation-account-blocked"></a>Comunicación con la cuenta de Automation bloqueada

Vaya a [Network planning](../update-management/plan-deployment.md#ports) (Planeamiento de red) para obtener información acerca de qué direcciones y puertos deben permitirse para que Update Management funcione.

#### <a name="duplicate-computer-name"></a>Nombre de equipo duplicado

Cambie el nombre de las máquinas virtuales para asegurarse de que los nombres sean únicos en el entorno.

#### <a name="deployed-image-from-cloned-machine"></a>Imagen implementada desde una máquina clonada

Si usa una imagen clonada, los distintos nombres de equipo tienen el mismo identificador de equipo de origen. En este caso:

1. En el área de trabajo de Log Analytics, quite la máquina virtual de la búsqueda guardada en la configuración de ámbito `MicrosoftDefaultScopeConfig-Updates` si se muestra. Las búsquedas guardadas se pueden encontrar en la sección **General** del área de trabajo.

2. Ejecute el siguiente cmdlet.

    ```azurepowershell-interactive
    Remove-Item -Path "HKLM:\software\microsoft\hybridrunbookworker" -Recurse -Force
    ```

3. Ejecute `Restart-Service HealthService` para reiniciar el servicio de mantenimiento. Esta operación vuelve a crear la clave y genera un nuevo UUID.

4. Si este enfoque no funciona, primero ejecute sysprep en la imagen y, a continuación, instale el agente de Log Analytics para Windows.

## <a name="scenario-you-receive-a-linked-subscription-error-when-you-create-an-update-deployment-for-machines-in-another-azure-tenant"></a><a name="multi-tenant"></a>Escenario: Recibe un error de la suscripción vinculada al crear una implementación de actualización para las máquinas en otro inquilino de Azure

### <a name="issue"></a>Problema

Encuentra el error siguiente al intentar crear una implementación de actualización para las máquinas en otro inquilino de Azure:

```error
The client has permission to perform action 'Microsoft.Compute/virtualMachines/write' on scope '/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Automation/automationAccounts/automationAccountName/softwareUpdateConfigurations/updateDeploymentName', however the current tenant '00000000-0000-0000-0000-000000000000' is not authorized to access linked subscription '00000000-0000-0000-0000-000000000000'.
```

### <a name="cause"></a>Causa

Este error se produce cuando se crea una implementación de actualización que tiene máquinas virtuales de Azure en otro inquilino que se incluye en una implementación de actualización.

### <a name="resolution"></a>Solución

Use la solución alternativa siguiente para programar estos elementos. Puede usar el cmdlet [New-AzAutomationSchedule](/powershell/module/az.automation/new-azautomationschedule) con el parámetro `ForUpdateConfiguration` para crear una programación. Después, use el cmdlet [New-AzAutomationSoftwareUpdateConfiguration](/powershell/module/Az.Automation/New-AzAutomationSoftwareUpdateConfiguration) y pase las máquinas del otro inquilino al parámetro `NonAzureComputer`. El ejemplo siguiente muestra cómo hacerlo:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$s = New-AzAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdateConfiguration

New-AzAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName $aa -Schedule $s -Windows -AzureVMResourceId $azureVMIdsW -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="scenario-unexplained-reboots"></a><a name="node-reboots"></a>Escenario: Reinicios inexplicables

### <a name="issue"></a>Problema

Aunque haya establecido la opción **Reboot Control** (Control de reinicio) en **No reiniciar nunca**, las máquinas todavía se reinician después de instalar las actualizaciones.

### <a name="cause"></a>Causa

Windows Update se puede modificar mediante varias claves del Registro, cualquiera de ellas puede modificar el comportamiento del reinicio.

### <a name="resolution"></a>Solución

Revise las claves del Registro enumeradas en [Configuración de actualizaciones automáticas mediante la edición del Registro](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) y [Claves del Registro usadas para administrar reinicios](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) para asegurarse de que las máquinas estén configuradas correctamente.

## <a name="scenario-machine-shows-failed-to-start-in-an-update-deployment"></a><a name="failed-to-start"></a>Escenario: Una máquina muestra "No se pudo iniciar" en una implementación de actualizaciones

### <a name="issue"></a>Problema

Una máquina muestra un estado `Failed to start` o `Failed`. Al ver los detalles específicos de la máquina, ve el siguiente error:

```error
For one or more machines in schedule, UM job run resulted in either Failed or Failed to start state. Guide available at https://aka.ms/UMSucrFailed.
```

### <a name="cause"></a>Causa

Este problema puede ocurrir debido a uno de los siguientes motivos:

* La máquina ya no existe.
* La máquina está desactivada y no se posible establecer comunicación con ella.
* La máquina tiene un problema de conectividad de red y, por lo tanto, no se puede alcanzar la instancia de Hybrid Worker de la máquina.
* Hubo una actualización del agente de Log Analytics que hizo cambiar el identificador del equipo de origen.
* La ejecución de actualizaciones se ha regulado si se ha alcanzado el límite de 200 trabajos simultáneos en una cuenta de Automation. Cada implementación se considera un trabajo y cada máquina de una implementación de actualizaciones se cuenta como un trabajo. Cualquier otro trabajo de automatización o implementación de actualizaciones actualmente en ejecución en la cuenta de Automation cuenta para el límite de trabajos simultáneos.

### <a name="resolution"></a>Solución

Puede recuperar más detalles mediante programación con las API REST. Consulte [Ejecuciones de la máquina de configuración de actualizaciones de software](/rest/api/automation/softwareupdateconfigurationmachineruns) para obtener información sobre cómo recuperar una lista de ejecuciones de máquina de configuración de actualizaciones, o una única ejecución de máquina de configuración de actualizaciones de software por identificador.

Cuando proceda, use [grupos dinámicos](../update-management/configure-groups.md) para las implementaciones de actualizaciones. Además, puede llevar a cabo los pasos siguientes.

1. Compruebe si la máquina o el servidor cumplen los [requisitos](../update-management/operating-system-requirements.md).
2. Compruebe la conectividad con el Hybrid Runbook Worker mediante el solucionador de problemas del agente de Hybrid Runbook Worker. Para más información sobre el solucionador de problemas, consulte el artículo sobre [cómo solucionar problemas con el agente de actualización](update-agent-issues.md).

## <a name="scenario-updates-are-installed-without-a-deployment"></a><a name="updates-nodeployment"></a>Escenario: Las actualizaciones se instalan sin una implementación

### <a name="issue"></a>Problema

Al inscribir una máquina Windows en Update Management, puede ver las actualizaciones instaladas sin una implementación.

### <a name="cause"></a>Causa

En Windows, las actualizaciones se instalan automáticamente en cuanto están disponibles. Este comportamiento puede producir confusión si no ha programado que una actualización se implemente en la máquina.

### <a name="resolution"></a>Solución

La clave del Registro `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU` se establece de manera predeterminada en un valor de 4: `auto download and install`.

En el caso de los clientes de Update Management, se recomienda establecer esta clave en 3: `auto download but do not auto install`.

Para más información, consulte [Configuración de actualizaciones automáticas](/windows/deployment/update/waas-wu-settings#configure-automatic-updates).

## <a name="scenario-machine-is-already-registered-to-a-different-account"></a><a name="machine-already-registered"></a>Escenario: La máquina ya está registrada en otra cuenta

### <a name="issue"></a>Problema

Aparece el siguiente mensaje de error:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

### <a name="cause"></a>Causa

La máquina ya se ha implementado en otra área de trabajo para Update Management.

### <a name="resolution"></a>Solución

1. Siga los pasos de [Las máquinas no se muestran en el portal en Update Management](#nologs) para asegurarse de que la máquina envía notificaciones al área de trabajo adecuada.
2. Limpie los artefactos en la máquina mediante la [eliminación del grupo Hybrid Runbook](../automation-windows-hrw-install.md#remove-a-hybrid-worker-group) y vuelva a intentarlo.

## <a name="scenario-machine-cant-communicate-with-the-service"></a><a name="machine-unable-to-communicate"></a>Escenario: La máquina no se puede comunicar con el servicio

### <a name="issue"></a>Problema

Aparece uno de los siguientes mensajes de error:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```error
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

```error
Access is denied. (Exception form HRESULT: 0x80070005(E_ACCESSDENIED))
```

### <a name="cause"></a>Causa

Un proxy, una puerta de enlace o un firewall pueden estar bloqueando la comunicación de red.

### <a name="resolution"></a>Solución

Revise la red y asegúrese de que están permitidas las direcciones y los puertos adecuados. Consulte los [requisitos de red](../automation-hybrid-runbook-worker.md#network-planning) para obtener una lista de puertos y direcciones que Update Management necesita y las instancias de Hybrid Runbook Worker.

## <a name="scenario-unable-to-create-self-signed-certificate"></a><a name="unable-to-create-selfsigned-cert"></a>Escenario: Error al crear el certificado autofirmado

### <a name="issue"></a>Problema

Aparece uno de los siguientes mensajes de error:

```error
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

### <a name="cause"></a>Causa

Hybrid Runbook Worker no pudo generar un certificado autofirmado.

### <a name="resolution"></a>Solución

Verifique que la cuenta del sistema tiene acceso de lectura a la carpeta **C:\ProgramData\Microsoft\Crypto\RSA** e inténtelo de nuevo.

## <a name="scenario-the-scheduled-update-failed-with-a-maintenancewindowexceeded-error"></a><a name="mw-exceeded"></a>Escenario: Error en la actualización programada con un error MaintenanceWindowExceeded

### <a name="issue"></a>Problema

La ventana de mantenimiento predeterminada para las actualizaciones es de 120 minutos. Puede aumentar la ventana de mantenimiento a un máximo de seis 6 horas o 360 minutos. Es posible que reciba el mensaje de error `For one or more machines in schedule, UM job run resulted in Maintenance Window Exceeded state. Guide available at https://aka.ms/UMSucrMwExceeded.`

### <a name="resolution"></a>Resolución

Para comprender el motivo de esta incidencia durante la ejecución de una actualización después de haberse iniciado correctamente, [compruebe la salida del trabajo](../update-management/deploy-updates.md#view-results-of-a-completed-update-deployment) desde la máquina afectada en la ejecución. Puede encontrar mensajes de error específicos procedentes de las máquinas que puede investigar e intentar solucionar.  

Puede recuperar más detalles mediante programación con las API REST. Consulte [Ejecuciones de la máquina de configuración de actualizaciones de software](/rest/api/automation/softwareupdateconfigurationmachineruns) para obtener información sobre cómo recuperar una lista de ejecuciones de máquina de configuración de actualizaciones, o una única ejecución de máquina de configuración de actualizaciones de software por identificador.

Edite las implementaciones de actualizaciones programadas con errores y aumente la ventana de mantenimiento.

Para más información sobre las ventanas de mantenimiento, consulte la [instalación de actualizaciones](../update-management/deploy-updates.md#schedule-an-update-deployment).

## <a name="scenario-machine-shows-as-not-assessed-and-shows-an-hresult-exception"></a><a name="hresult"></a>Escenario: La máquina aparece como "No evaluado" y se muestra una excepción HRESULT

### <a name="issue"></a>Problema

* Tiene máquinas que aparecen como `Not assessed` en **Cumplimiento** y verá un mensaje de excepción debajo de él.
* Se muestra un código de error HRESULT en el portal.

### <a name="cause"></a>Causa

El agente de actualización (Agente de Windows Update en Windows, el administrador de paquetes para la distribución de Linux) no está configurado correctamente. Update Management se basa en el agente de actualización de la máquina para proporcionar las actualizaciones necesarias, el estado de la revisión y los resultados de las revisiones implementadas. Sin esta información, Update Management no puede informar correctamente de las revisiones que son necesarias o que están instaladas.

### <a name="resolution"></a>Solución

Intente realizar actualizaciones de forma local en la máquina. Si esta operación falla, suele significar que hay un error de configuración con el agente de actualización.

Este problema suele deberse a problemas de firewall y de configuración de red. Use las siguientes comprobaciones para corregir el problema.

* Para Linux, consulte la documentación correspondiente para asegurarse de que puede acceder al punto de conexión de red del repositorio de paquetes.

* Para Windows, compruebe la configuración del agente como se indica en [Las actualizaciones no se descargan desde el punto de conexión de la intranet (WSUS/SCCM)](/windows/deployment/update/windows-update-troubleshooting#updates-arent-downloading-from-the-intranet-endpoint-wsussccm).

  * Si las máquinas están configuradas para Windows Update, asegúrese de que puede acceder a los puntos de conexión descritos en [Problemas relacionados con HTTP/proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy).
  * Si las máquinas están configuradas para Windows Server Update Services (WSUS), asegúrese de que puede acceder al servidor WSUS configurado por la [clave del Registro WUServer ](/windows/deployment/update/waas-wu-settings).

Si se muestra un código de error HRESULT, haga doble clic en la excepción que se muestra en rojo para ver el mensaje de excepción completo. Revise la siguiente tabla para ver posibles resoluciones o acciones recomendadas.

|Excepción  |Acción o resolución  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Busque el código de error correspondiente en la [lista de códigos de error de Windows Update](https://support.microsoft.com/help/938205/windows-update-error-code-list) para buscar detalles adicionales sobre la causa de la excepción.        |
|`0x8024402C`</br>`0x8024401C`</br>`0x8024402F`      | Estos errores indican problemas de conectividad de red. Asegúrese de que la máquina tenga conectividad de red con Update Management. Consulte la sección sobre el [planeamiento de red](../update-management/plan-deployment.md#ports) para obtener una lista de puertos y direcciones necesarios.        |
|`0x8024001E`| No se completó la operación de actualización porque se estaba cerrando el servicio o el sistema.|
|`0x8024002E`| El servicio de Windows Update está deshabilitado.|
|`0x8024402C`     | Si usa un servidor WSUS, asegúrese de que los valores del Registro para `WUServer` y `WUStatusServer` en la clave del Registro `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` especifican el servidor WSUS correcto.        |
|`0x80072EE2`|Existe un problema de conectividad de red o un problema al comunicarse con un servidor WSUS configurado. Compruebe la configuración de WSUS y asegúrese de que se puede acceder al servicio desde el cliente.|
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Asegúrese de que el servicio Windows Update (wuauserv) se está ejecutando y no está deshabilitado.        |
|`0x80070005`| Un error de acceso denegado puede deberse a cualquiera de las siguientes razones:<br> Equipo infectado<br> Windows Update no está configurado correctamente.<br> Error de permiso de archivo en la carpeta %WinDir%\SoftwareDistribution<br> Espacio en disco insuficiente en la unidad del sistema (C:).
|Cualquier otra excepción genérica     | Ejecute una búsqueda en Internet para ver las resoluciones posibles y colabore con el equipo de soporte técnico de TI local.         |

Revisar el archivo **%Windir%\Windowsupdate.log** también puede ayudar a determinar los posibles motivos. Para más información sobre cómo leer el registro, consulte [Cómo leer el archivo Windowsupdate.log](https://support.microsoft.com/help/902093/how-to-read-the-windowsupdate-log-file).

También puede descargar y ejecutar el [solucionador de problemas de Windows Update](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) para comprobar si hay algún problema con Windows Update en el equipo.

> [!NOTE]
> La documentación del [solucionador de problemas de Windows Update](https://support.microsoft.com/help/4027322/windows-update-troubleshooter) indica que es para su uso en clientes de Windows, pero también funciona en Windows Server.

## <a name="scenario-update-run-returns-failed-status-linux"></a>Escenario: La ejecución de la actualización devuelve un estado Error (Linux)

### <a name="issue"></a>Problema

Se inicia una ejecución de actualización, pero encuentra errores durante la ejecución.

### <a name="cause"></a>Causa

Causas posibles:

* El estado del administrador de paquetes no es correcto.
* El agente de actualización (WUA para Windows, el administrador de paquetes específico de la distribución para Linux) no está configurado correctamente.
* Paquetes específicos interfieren con la aplicación de revisiones basada en la nube.
* La máquina es inaccesible.
* Las actualizaciones tenían dependencias que no se resolvieron.

### <a name="resolution"></a>Solución

Si se producen errores durante una ejecución de actualizaciones después de que se haya iniciado correctamente, [compruebe el trabajo de salida](../update-management/deploy-updates.md#view-results-of-a-completed-update-deployment) desde la máquina afectada en la ejecución. Puede encontrar mensajes de error específicos procedentes de las máquinas que puede investigar e intentar solucionar. Update Management requiere que el administrador de paquetes tenga un estado correcto para que las implementaciones de actualizaciones se realicen con éxito.

Si se observan revisiones, paquetes o actualizaciones específicos inmediatamente antes de que se produzca un error en el trabajo, puede intentar [excluirlos](../update-management/deploy-updates.md#schedule-an-update-deployment) de la siguiente implementación de actualizaciones. Para recopilar información de registro de Windows Update, consulte [Archivos de registro de Windows Update](/windows/deployment/update/windows-update-logs).

Si no puede resolver un problema de aplicación de revisiones, realice una copia del archivo **/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log** y consérvela para solucionar los problemas antes de que se inicie la siguiente implementación de actualizaciones.

## <a name="patches-arent-installed"></a>Las revisiones no se instalan

### <a name="machines-dont-install-updates"></a>Las máquinas no instalan las actualizaciones

Intente ejecutar las actualizaciones directamente en la máquina. Si la máquina no se puede aplicar las actualizaciones, consulte la [lista de posibles errores en la guía de solución de problemas](#hresult).

Si las actualizaciones se ejecutan localmente, intente quitar y volver a instalar el agente en la máquina siguiendo las instrucciones para [quitar una máquina virtual de Update Management](../update-management/remove-vms.md).

### <a name="i-know-updates-are-available-but-they-dont-show-as-available-on-my-machines"></a>Sé que hay actualizaciones disponibles, pero no se muestran como disponibles en mis máquinas

Esto sucede a menudo si las máquinas están configuradas para obtener actualizaciones de WSUS o Microsoft Endpoint Configuration Manager pero WSUS y Configuration Manager no han aprobado las actualizaciones.

Puede comprobar si las máquinas están configuradas para WSUS y SSCM mediante la referencia cruzada de la clave del Registro `UseWUServer` con las claves del Registro de la sección [Configuración de actualizaciones automáticas mediante la edición del Registro de este artículo](https://support.microsoft.com/help/328010/how-to-configure-automatic-updates-by-using-group-policy-or-registry-s).

Si las actualizaciones no se aprueban en WSUS, no se instalan. Puede comprobar si hay actualizaciones no aprobadas en Log Analytics ejecutando la consulta siguiente.

  ```kusto
  Update | where UpdateState == "Needed" and ApprovalSource == "WSUS" and Approved == "False" | summarize max(TimeGenerated) by Computer, KBID, Title
  ```

### <a name="updates-show-as-installed-but-i-cant-find-them-on-my-machine"></a>Las actualizaciones se muestran como instaladas, pero no las encuentro en mi máquina

Con frecuencia, unas actualizaciones tienen preferencia sobre otras. Para más información, consulte [La actualización se ha reemplazado](/windows/deployment/update/windows-update-troubleshooting#the-update-is-not-applicable-to-your-computer) en la guía de solución de problemas de Windows Update.

### <a name="installing-updates-by-classification-on-linux"></a>Instalación de actualizaciones mediante clasificación en Linux

La implementación de actualizaciones en Linux mediante clasificación ("actualizaciones críticas y de seguridad") tiene advertencias importantes, especialmente para CentOS. Estas limitaciones se documentan en la página de [información general de Update Management](../update-management/overview.md#update-classifications).

### <a name="kb2267602-is-consistently-missing"></a>KB2267602 falta constantemente

KB2267602 es la [actualización de la definición de Windows Defender](https://www.microsoft.com/wdsi/definitions). Se actualiza diariamente.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o no puede resolverlo, intente uno de los siguientes canales para obtener ayuda adicional.

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente.
* Registrar un incidente de soporte técnico de Azure. Vaya al [sitio de Soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
