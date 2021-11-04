---
title: Administración de los scripts previos y posteriores en la implementación de Update Management en Azure
description: En este artículo se describe cómo configurar y administrar los scripts previos y posteriores a las implementaciones de actualizaciones.
services: automation
ms.subservice: update-management
ms.date: 09/16/2021
ms.topic: conceptual
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 19101cea8f435f09cb37aa2340f0bc02788442f3
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131459071"
---
# <a name="manage-pre-scripts-and-post-scripts"></a>Administración de scripts previos y posteriores

Los scripts previos y posteriores son runbooks para ejecutar en la cuenta de Azure Automation antes (antes de la tarea) y después (después de la tarea) de la implementación de una actualización. Los scripts previos y posteriores se ejecutan en el contexto de Azure, no de forma local. Los scripts previos se ejecutan al principio de la implementación de actualizaciones. En Windows, los scripts posteriores se ejecutan al final de la implementación y después de todos los reinicios que estén configurados. En el caso de Linux, los scripts posteriores se ejecutan tras finalizar la implementación, no después de que se reinicie la máquina. 

## <a name="pre-script-and-post-script-requirements"></a>Requisitos de scripts previos y posteriores

Para que un runbook se utilice como script previo o posterior, debe importarlo en su cuenta de Automation y [publicarlo](../manage-runbooks.md#publish-a-runbook).

Actualmente solo se admiten los runbooks de PowerShell y Python 2 como scripts anteriores o posteriores. Otros tipos de runbook, como los de Python 3, gráficos, flujo de trabajo de PowerShell o el flujo de trabajo gráfico de PowerShell no se admiten actualmente como scripts anteriores o posteriores.

## <a name="pre-script-and-post-script-parameters"></a>Parámetros de scripts previos y posteriores

Cuando se configuran los scripts previos y posteriores, se pueden pasar parámetros como cuando se programa un runbook. Los parámetros se definen en el momento de la creación de la implementación de actualización. Los scripts previos y posteriores admiten los siguientes tipos:

* [char]
* [byte]
* [int]
* [long]
* [decimal]
* [single]
* [double]
* [DateTime]
* [string]

Los parámetros de runbook de scripts previos y posteriores no admiten tipos booleanos, de objeto ni de matriz. Estos valores hacen que se produzca un error en los runbooks. 

Si necesita otro tipo de objeto, puede convertirlo a otro tipo mediante su propia lógica en el runbook.

Además de los parámetros estándar de runbook, se proporciona el parámetro `SoftwareUpdateConfigurationRunContext` (cadena de tipo JSON). Si define el parámetro en el runbook del script previo o posterior, la implementación de actualizaciones lo pasará automáticamente. El parámetro contiene información sobre la implementación de la actualizaciones, que es un subconjunto de la información que devuelve [SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration). En las secciones siguientes se definen las propiedades asociadas.

### <a name="softwareupdateconfigurationruncontext-properties"></a>Propiedades de SoftwareUpdateConfigurationRunContext

|Propiedad  |Tipo |Descripción  |
|---------|---------|---------|
|SoftwareUpdateConfigurationName     |String | Nombre de la configuración de actualizaciones de software.        |
|SoftwareUpdateConfigurationRunId     |GUID | El identificador único de la ejecución.        |
|SoftwareUpdateConfigurationSettings     || Colección de propiedades relacionadas con la configuración de actualizaciones de software.         |
|SoftwareUpdateConfigurationSettings.OperatingSystem     |Int | Sistemas operativos a los que se dirige la implementación de actualizaciones. `1` = Windows y `2` = Linux        |
|SoftwareUpdateConfigurationSettings.Duration     |Intervalo de tiempo (HH:MM:SS) | La duración máxima de la ejecución de la implementación de actualizaciones es de `PT[n]H[n]M[n]S`, según ISO8601; también se conoce como ventana de mantenimiento.<br> Ejemplo: 02:00:00         |
|SoftwareUpdateConfigurationSettings.WindowsConfiguration     || Colección de propiedades relacionadas con los equipos Windows.         |
|SoftwareUpdateConfigurationSettings.WindowsConfiguration.excludedKbNumbers     |String | Una lista separada por espacios de artículos de KB que se excluyen de la implementación de actualizaciones.        |
|SoftwareUpdateConfigurationSettings.WindowsConfiguration.includedKbNumbers     |String | Una lista separada por espacios de artículos de KB que se incluyen en la implementación de actualizaciones.        |
|SoftwareUpdateConfigurationSettings.WindowsConfiguration.UpdateCategories     |Entero | 1 = "Critical";<br> 2 = "Security"<br> 4 = "UpdateRollUp"<br> 8 = "FeaturePack"<br> 16 = "ServicePack"<br> 32 = "Definition"<br> 64 = "Tools"<br> 128 = "Updates"        |
|SoftwareUpdateConfigurationSettings.WindowsConfiguration.rebootSetting     |String | Configuración de reinicio para la implementación de actualizaciones. Los valores son `IfRequired`, `Never` y `Always`      |
|SoftwareUpdateConfigurationSettings.LinuxConfiguration     || Colección de propiedades relacionadas con los equipos Linux.         |
|SoftwareUpdateConfigurationSettings.LinuxConfiguration.IncludedPackageClassifications |Entero |0 = "Unclassified"<br> 1 = "Critical"<br> 2 = "Security"<br> 4 = "Other"|
|SoftwareUpdateConfigurationSettings.LinuxConfiguration.IncludedPackageNameMasks |String | Una lista separada por espacios de nombres de paquetes que se incluyen en la implementación de actualizaciones. |
|SoftwareUpdateConfigurationSettings.LinuxConfiguration.ExcludedPackageNameMasks |String |Una lista separada por espacios de nombres de paquetes que se excluyen de la implementación de actualizaciones. |
|SoftwareUpdateConfigurationSettings.LinuxConfiguration.RebootSetting |String |Configuración de reinicio para la implementación de actualizaciones. Los valores son `IfRequired`, `Never` y `Always`      |
|SoftwareUpdateConfiguationSettings.AzureVirtualMachines     |Matriz de cadena | Lista de identificadores resourceId para las VM de Azure en la implementación de actualizaciones.        |
|SoftwareUpdateConfigurationSettings.NonAzureComputerNames|Matriz de cadena |Una lista de FQDN de los equipos que no son de Azure en la implementación de actualizaciones.|

En el ejemplo siguiente se muestra una cadena JSON que se pasa a la propiedad **SoftwareUpdateConfigurationSettings** para un equipo Linux:

```json
"SoftwareUpdateConfigurationSettings": {
     "OperatingSystem": 2,
     "WindowsConfiguration": null,
     "LinuxConfiguration": {
         "IncludedPackageClassifications": 7,
         "ExcludedPackageNameMasks": "fgh xyz",
         "IncludedPackageNameMasks": "abc bin*",
         "RebootSetting": "IfRequired"
     },
     "Targets": {
         "azureQueries": null,
         "nonAzureQueries": ""
     },
     "NonAzureComputerNames": [
        "box1.contoso.com",
        "box2.contoso.com"
     ],
     "AzureVirtualMachines": [
        "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Compute/virtualMachines/vm-01"
     ],
     "Duration": "02:00:00",
     "PSComputerName": "localhost",
     "PSShowComputerName": true,
     "PSSourceJobInstanceId": "2477a37b-5262-4f4f-b636-3a70152901e9"
 }
```

En el ejemplo siguiente se muestra una cadena JSON que se pasa a la propiedad **SoftwareUpdateConfigurationSettings** para un equipo Windows:

```json
"SoftwareUpdateConfigurationRunContext": {
    "SoftwareUpdateConfigurationName": "sampleConfiguration",
    "SoftwareUpdateConfigurationRunId": "00000000-0000-0000-0000-000000000000",
    "SoftwareUpdateConfigurationSettings": {
      "operatingSystem": "Windows",
      "duration": "02:00:00",
      "windows": {
        "excludedKbNumbers": [
          "168934",
          "168973"
        ],
        "includedUpdateClassifications": "Critical",
        "rebootSetting": "IfRequired"
      },
      "azureVirtualMachines": [
        "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/vm-01",
        "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/vm-02",
        "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/vm-03"
      ],
      "nonAzureComputerNames": [
        "box1.contoso.com",
        "box2.contoso.com"
      ]
    }
  }
```

Se puede encontrar un ejemplo completo con todas las propiedades en: [Get software update configuration by name](/rest/api/automation/softwareupdateconfigurations/getbyname#examples) (Obtener la configuración de actualizaciones de software por nombre).

> [!NOTE]
> El objeto `SoftwareUpdateConfigurationRunContext` puede contener entradas duplicadas para las máquinas. Esto puede hacer que los scripts previos y posteriores se ejecuten varias veces en la misma máquina. Para solucionar este comportamiento, use `Sort-Object -Unique` para seleccionar solo nombres de VM únicos.

## <a name="use-a-pre-script-or-post-script-in-a-deployment"></a>Uso de un script previo o posterior en una implementación

Para usar un script previo o posterior en una implementación de actualizaciones, solo tiene que empezar por crear una implementación de actualizaciones. Seleccione **Scripts previos y scripts posteriores**. Se abrirá la página **Seleccionar scripts previos + scripts posteriores**.

![Selección de scripts](./media/pre-post-scripts/select-scripts.png)

Seleccione el script que quiere utilizar. En este ejemplo, usamos el runbook **UpdateManagement-TurnOnVms**. Al seleccionar el runbook, se abre la página **Configurar script**. Seleccione **Script previo** y, a continuación, elija **Aceptar**.

Repita este proceso para el script **UpdateManagement TurnOffVms**. No obstante, al elegir la opción de **Tipo de script**, seleccione **Script posterior**.

Ahora la sección **Elementos seleccionados** muestra ambos scripts seleccionados. Uno es un script previo y el otro es un script posterior:

![Elementos seleccionados](./media/pre-post-scripts/selected-items.png)

Finalice la configuración de su implementación de actualizaciones.

Cuando se complete la implementación de actualizaciones, puede ir a **Implementaciones de actualizaciones** para ver los resultados. Como se puede ver, se proporciona el estado de los scripts previo y posterior:

![Resultados de actualización](./media/pre-post-scripts/update-results.png)

Al seleccionar la ejecución de la implementación de actualizaciones, se muestran detalles adicionales de los scripts previos y posteriores. Se proporciona un vínculo al origen del script en el momento de la ejecución.

![Resultados de la ejecución de la implementación](./media/pre-post-scripts/deployment-run.png)

## <a name="stop-a-deployment"></a>Detención de una implementación

Si quiere detener una implementación basada en un script previo, debe [generar](../automation-runbook-execution.md#throw) una excepción. Si no lo hace, la implementación y el script posterior se seguirán ejecutando. En el fragmento de código siguiente se muestra cómo generar una excepción mediante PowerShell.

```powershell
#In this case, we want to terminate the patch job if any run fails.
#This logic might not hold for all cases - you might want to allow success as long as at least 1 run succeeds
foreach($summary in $finalStatus)
{
    if ($summary.Type -eq "Error")
    {
        #We must throw in order to fail the patch deployment.
        throw $summary.Summary
    }
}
```

En Python 2, el control de excepciones se administra en un bloque [try](https://www.python-course.eu/exception_handling.php).

## <a name="interact-with-machines"></a>Interacción con máquinas

Los scripts previos y los scripts posteriores se ejecutan como runbooks en la cuenta de Automation y no directamente en las máquinas de la implementación. Las tareas previas y posteriores también se ejecutan en el contexto de Azure y no tienen acceso a máquinas que no son de Azure. En las secciones siguientes se muestra cómo interactuar directamente con las máquinas, tanto si se trata de VM de Azure como de máquinas que no son de Azure.

### <a name="interact-with-azure-machines"></a>Interacción con máquinas de Azure

Las tareas previas y posteriores se ejecutan como runbooks y no se ejecutan de manera nativa en las VM de Azure de la implementación. Para interactuar con las máquinas virtuales de Azure, debe tener estos elementos:

* Una [identidad administrada](../automation-security-overview.md#managed-identities) o una cuenta de ejecución
* Un runbook que quiera ejecutar

Para interactuar con máquinas de Azure, debe usar el cmdlet [Invoke-AzVMRunCommand](/powershell/module/az.compute/invoke-azvmruncommand) para interactuar con las VM de Azure. Para un ejemplo de cómo hacerlo, consulte el ejemplo de runbook [Update Management: ejecutar script con el comando de ejecución](https://github.com/azureautomation/update-management-run-script-with-run-command).

### <a name="interact-with-non-azure-machines"></a>Interacción con máquinas que no son de Azure

Las tareas previas y posteriores se ejecutan en el contexto de Azure y no tienen acceso a máquinas que no son de Azure. Para poder interactuar con máquinas que no son de Azure, debe tener los siguientes elementos:

* Una [identidad administrada](../automation-security-overview.md#managed-identities) o una cuenta de ejecución
* Hybrid Runbook Worker instalado en la máquina
* Un runbook que desea ejecutar localmente
* Un runbook principal

Para interactuar con máquinas que no son de Azure, se ejecuta un runbook principal en el contexto de Azure. Este runbook llama a un runbook secundario con el cmdlet [Start-AzAutomationRunbook](/powershell/module/Az.Automation/Start-AzAutomationRunbook). Debe especificar el parámetro `RunOn` y proporcionar el nombre de la instancia de Hybrid Runbook Worker para que el script se ejecute. Consulte el ejemplo de runbook [Update Management: ejecutar script de forma local](https://github.com/azureautomation/update-management-run-script-locally).

## <a name="abort-patch-deployment"></a>Acerca de la implementación de revisiones

Si el script previo devuelve un error, es posible que quiera anular la implementación. Para ello, debe [generar](/powershell/module/microsoft.powershell.core/about/about_throw) un error en el script en cualquier lógica que implique un error.

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.
    throw "There was an error, abort deployment"
}
```

En Python 2, si quiere generar un error cuando se produzca una condición determinada, use una instrucción [raise](https://docs.python.org/2.7/reference/simple_stmts.html#the-raise-statement).

```python
If (<My custom error logic>)
   raise Exception('Something happened.')
```

## <a name="samples"></a>Ejemplos

Se pueden encontrar ejemplos de scripts previos y posteriores en la [organización GitHub de Azure Automation](https://github.com/azureautomation) y en la [Galería de PowerShell](https://www.powershellgallery.com/packages?q=Tags%3A%22UpdateManagement%22+Tags%3A%22Automation%22), o bien se pueden importar mediante Azure Portal. Para hacerlo, en la cuenta de Automation, en **Automatización de procesos**, seleccione **Galería de runbooks**. Utilice **Update Management** para el filtro.

![Lista de la galería](./media/pre-post-scripts/runbook-gallery.png)

O bien puede buscarlos por su nombre de script, tal y como se muestra en la siguiente lista:

* Update Management: activar máquinas virtuales
* Update Management: desactivar máquinas virtuales
* Update Management: ejecución de scripts de forma local
* Update Management: plantilla para scripts previos y posteriores
* Update Management: ejecutar script con el comando de ejecución

> [!IMPORTANT]
> Después de importar los runbooks, debe publicarlos para poder usarlos. Para ello, busque el runbook en su cuenta de Automation, seleccione **Editar** y elija **Publicar**.

Todos los ejemplos se basan en la plantilla básica que se define en el ejemplo siguiente. Esta plantilla se puede usar para crear un runbook propio para usarlo con los scripts previos y posteriores. Se incluye la lógica necesaria para realizar una autenticación con Azure y para controlar el parámetro `SoftwareUpdateConfigurationRunContext`.

```powershell
<#
.SYNOPSIS
 Barebones script for Update Management Pre/Post

.DESCRIPTION
  This script is intended to be run as a part of Update Management pre/post-scripts.
  It requires the Automation account's system-assigned managed identity.

.PARAMETER SoftwareUpdateConfigurationRunContext
  This is a system variable which is automatically passed in by Update Management during a deployment.
#>

param(
    [string]$SoftwareUpdateConfigurationRunContext
)

#region BoilerplateAuthentication
# Ensures you do not inherit an AzContext in your runbook
Disable-AzContextAutosave -Scope Process

# Connect to Azure with system-assigned managed identity
$AzureContext = (Connect-AzAccount -Identity).context

# set and store context
$AzureContext = Set-AzContext -SubscriptionName $AzureContext.Subscription -DefaultProfile $AzureContext
#endregion BoilerplateAuthentication

#If you wish to use the run context, it must be converted from JSON
$context = ConvertFrom-Json $SoftwareUpdateConfigurationRunContext
#Access the properties of the SoftwareUpdateConfigurationRunContext
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines | Sort-Object -Unique
$runId = $context.SoftwareUpdateConfigurationRunId

Write-Output $context

#Example: How to create and write to a variable using the pre-script:
<#
#Create variable named after this run so it can be retrieved
New-AzAutomationVariable -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccount -Name $runId -Value "" -Encrypted $false
#Set value of variable
Set-AutomationVariable -Name $runId -Value $vmIds
#>

#Example: How to retrieve information from a variable set during the pre-script
<#
$variable = Get-AutomationVariable -Name $runId
#>
```

Si desea que el runbook se ejecute con la identidad administrada asignada por el sistema, deje el código tal y como está. Si prefiere usar una identidad administrada asignada por el usuario, haga lo siguiente:
1. Desde la línea 22, quite `$AzureContext = (Connect-AzAccount -Identity).context`.
1. Reemplace el valor por `$AzureContext = (Connect-AzAccount -Identity -AccountId <ClientId>).context`.
1. Escriba el id. de cliente.

> [!NOTE]
> En el caso de los runbooks de PowerShell no gráficos, `Add-AzAccount` y `Add-AzureRMAccount` son alias de [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount). Puede usar estos cmdlets o bien [actualizar los módulos](../automation-update-azure-modules.md) de la cuenta de Automation a las versiones más recientes. Es posible que deba actualizar los módulos incluso si acaba de crear una nueva cuenta de Automation.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la administración de actualizaciones, consulte [Administración de actualizaciones y revisiones para las máquinas virtuales](manage-updates-for-vm.md).
