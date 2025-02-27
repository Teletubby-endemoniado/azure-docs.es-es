---
title: Tipos de runbooks de Azure Automation
description: En este artículo se describen los tipos de runbooks que puede usar en Azure Automation y las consideraciones de determinar qué tipo usar.
services: automation
ms.subservice: process-automation
ms.date: 10/28/2021
ms.topic: conceptual
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: f0fe647d586887bcb2d0a897f57c75a5f0b141b5
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131423949"
---
# <a name="azure-automation-runbook-types"></a>Tipos de runbooks de Azure Automation

La característica de automatización de procesos de Azure Automation admite varios tipos de runbooks, tal como se define en la tabla siguiente. Para más información sobre el entorno de automatización de procesos, consulte [Ejecución de un runbook en Azure Automation](automation-runbook-execution.md).

| Tipo | Descripción |
|:--- |:--- |
| [Gráfico](#graphical-runbooks)|Runbook gráfico basado en Windows PowerShell, y creado y editado completamente en el editor gráfico de Azure Portal. |
| [Flujo de trabajo gráfico de PowerShell](#graphical-runbooks)|Runbook gráfico basado en flujo de trabajo de Windows PowerShell, y creado y editado completamente en el editor gráfico de Azure Portal. |
| [PowerShell](#powershell-runbooks) |Runbook de texto basado en scripting de Windows PowerShell. |
| [Flujo de trabajo de PowerShell](#powershell-workflow-runbooks)|Runbook de texto basado en scripting de flujo de trabajo de Windows PowerShell. |
| [Python](#python-runbooks) |Runbook de texto basado en scripting de Python. |

Tenga en cuenta las siguientes consideraciones al determinar qué tipo usar para un runbook concreto.

* No es posible convertir runbooks de tipo gráfico a texto o viceversa.
* Existen limitaciones al utilizar runbooks de diferentes tipos como runbooks secundarios. Para más información, consulte [Child runbooks in Azure Automation](automation-child-runbooks.md) (Runbooks secundarios en Azure Automation).

## <a name="graphical-runbooks"></a>Runbooks gráficos

Puede crear y editar runbooks gráficos y runbooks gráficos de flujo de trabajo de PowerShell mediante el editor gráfico de Azure Portal. Sin embargo, no puede crear o editar este tipo de runbooks con otra herramienta. Principales características de los runbooks gráficos:

* Se exportan a archivos de la cuenta de Automation y se importan a otra cuenta de Automation.
* Generan código de PowerShell.
* Se pueden convertir a y desde runbooks gráficos de flujo de trabajo de PowerShell durante la importación.

### <a name="advantages"></a>Ventajas

* Utilice modelos de creación visual para insertar, vincular y configurar.
* Céntrese en cómo fluyen los datos por el proceso.
* Represente visualmente los procesos de administración.
* Incluya otros runbooks como runbooks secundarios para crear flujos de trabajo de nivel alto.
* Anime a usar la programación modular.

### <a name="limitations"></a>Limitaciones

* No puede crear o editar fuera de Azure Portal.
* Puede requerir una actividad de código que contenga código de PowerShell para ejecutar una lógica compleja.
* No puede convertir a uno de los [formatos de texto](automation-runbook-types.md), ni puede convertir un runbook de texto a un formato gráfico. 
* No puede ver ni modificar directamente el código de PowerShell que el flujo de trabajo gráfico crea. Puede ver el código que crea en las actividades de código.
* No puede ejecutar runbooks en Linux Runbook Worker. Consulte [Automatización de recursos en los centros de datos o nube con Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).
* Los runbooks gráficos no se pueden firmar digitalmente.

## <a name="powershell-runbooks"></a>Runbooks de PowerShell

Los runbooks de PowerShell están basados en Windows PowerShell. Puede modificar directamente el código del runbook con el editor de texto en el Portal de Azure. También puede usar cualquier editor de texto sin conexión e [importar el runbook](manage-runbooks.md) en Azure Automation.


La versión de PowerShell viene determinada por la **versión del entorno de ejecución** especificada (es decir, la versión preliminar 7.1 o 5.1). El servicio Azure Automation admite el entorno de ejecución de PowerShell más reciente.
 
El mismo espacio aislado de Azure y la instancia de Hybrid Runbook Worker pueden ejecutar runbooks de **PowerShell 5.1** y **PowerShell 7.1** en paralelo.   

> [!NOTE]
>  En el momento de la ejecución del runbook, si selecciona **Versión del entorno de ejecución** como **7.1 (versión preliminar)** , se usan los módulos de PowerShell destinados a la versión del entorno de ejecución 7.1; igualmente, si selecciona **Versión del entorno de ejecución** como **5.1**, se usan los módulos de PowerShell que tienen como destino la versión del entorno de ejecución 5.1.
 
Asegúrese de seleccionar la versión del entorno de ejecución adecuada para los módulos.

Por ejemplo: si está ejecutando un runbook para un escenario de automatización de SharePoint en la **versión del entorno de ejecución** *7.1 (versión preliminar)* , importe el módulo en la **versión del entorno de ejecución** **7.1 (versión preliminar)** ; si por el contrario está ejecutando un runbook para un escenario de automatización de SharePoint en la **versión del entorno de ejecución** **5.1**, importe el módulo en la **versión del entorno de ejecución** *5.1*. En este caso, verá dos entradas para el módulo, una para la **versión del entorno de ejecución** **7.1 (versión preliminar)** y otra para la versión **5.1**.

:::image type="content" source="./media/automation-runbook-types/runbook-types.png" alt-text="Tipos de runbook.":::


Actualmente, se admiten PowerShell 5.1 y 7.1 (versión preliminar).


### <a name="advantages"></a>Ventajas

* Implemente toda la lógica compleja con código de PowerShell sin otras complejidades del flujo de trabajo de PowerShell.
* Se inician con más rapidez que los runbooks de flujo de trabajo de PowerShell, ya que no necesitan compilarse antes de la ejecución.
* Ejecute en Azure y en Hybrid Runbook Worker para Windows y Linux.

### <a name="limitations---version-51"></a>Limitaciones - versión 5.1

* Debe estar familiarizado con el scripting de PowerShell.
* Los runbooks no pueden usar el [procesamiento paralelo](automation-powershell-workflow.md#use-parallel-processing) para ejecutar varias acciones en paralelo.
* Los runbooks no pueden usar los [puntos de control](automation-powershell-workflow.md#use-checkpoints-in-a-workflow) para reanudar un runbook si se produce un error.
* Solo puede incluir runbooks gráficos, de PowerShell y de flujo de trabajo de PowerShell como runbooks secundarios mediante el cmdlet [Start-AzAutomationRunbook](/powershell/module/az.automation/start-azautomationrunbook), que crea un trabajo.
* Los runbooks no pueden usar la instrucción [#Requires](/powershell/module/microsoft.powershell.core/about/about_requires) de PowerShell, no se admite en el espacio aislado de Azure ni en instancias de Hybrid Runbook Worker y provocarán un error en el trabajo.

### <a name="known-issues---version-51"></a>Problemas conocidos: versión 5.1

A continuación se describen los problemas conocidos actuales con los runbooks de PowerShell:

* Los runbooks de PowerShell no pueden recuperar un [recurso de variable](./shared-resources/variables.md) sin cifrar con un valor null.
* Los runbooks de PowerShell no pueden recuperar un recurso de variable con `*~*` en el nombre.
* Una operación [Get-Process](/powershell/module/microsoft.powershell.management/get-process) en un bucle de un runbook de PowerShell puede bloquearse después de más de 80 iteraciones.
* Un runbook de PowerShell puede producir un error si intenta escribir una gran cantidad de datos en el flujo de salida a la vez. Puede evitar este problema si hace que el runbook genere únicamente la información necesaria para trabajar con objetos grandes. Por ejemplo, en lugar de usar `Get-Process` sin limitaciones, puede hacer que el cmdlet genere solo los parámetros necesarios como en `Get-Process | Select ProcessName, CPU`.

### <a name="limitations---71-preview"></a>Limitaciones: 7.1 (versión preliminar)

-  Los cmdlets internos de PowerShell correspondientes a Azure Automation no se admiten en una instancia de Hybrid Runbook Worker de Linux. Debe importar el módulo `automationassets` al comienzo de su runbook de Python para acceder a las funciones de recursos compartidos (recursos) de la cuenta de Automation. 
-  Para la versión del entorno de ejecución de PowerShell 7, las actividades del módulo no se extraen para los módulos importados.
-  El tipo de parámetro de runbook *PSCredential* no se admite en la versión del entorno de ejecución de PowerShell 7.
-  PowerShell 7.x no admite flujos de trabajo. Para obtener más información, consulte [esto](/powershell/scripting/whats-new/differences-from-windows-powershell?view=powershell-7.1#powershell-workflow&preserve-view=true).
-  PowerShell 7.x no admite actualmente runbooks firmados.

### <a name="known-issues---71-preview"></a>Problemas conocidos: 7.1 (versión preliminar)

-  La ejecución de scripts secundarios mediante `.\child-runbook.ps1` no se admite en esta versión preliminar. 
  **Solución alternativa**: use `Start-AutomationRunbook` (cmdlet interno) o `Start-AzAutomationRunbook` (desde el módulo *Az.Automation*) para iniciar otro runbook desde el runbook primario.
-  Las propiedades de runbook que definen la preferencia de registro no se admiten en el entorno de ejecución de PowerShell 7.  
  **Solución alternativa**: establezca explícitamente la preferencia al principio del runbook, tal como se indica a continuación:

      `$VerbosePreference = "Continue"`

      `$ProgressPreference = "Continue"`

-   Evite importar el módulo `Az.Accounts` a la versión 2.4.0 para el entorno de ejecución de PowerShell 7, ya que puede haber un comportamiento inesperado al usar esta versión en Azure Automation. 
-    Es posible que encuentre problemas de formato con los flujos de salida de error para el trabajo que se ejecuta en el entorno de ejecución de PowerShell 7.

## <a name="powershell-workflow-runbooks"></a>Runbooks del flujo de trabajo de PowerShell

Los runbooks del flujo de trabajo de PowerShell son runbooks de texto basados en el [flujo de trabajo de Windows PowerShell](automation-powershell-workflow.md). Puede modificar directamente el código del runbook con el editor de texto en el Portal de Azure. También puede usar cualquier editor de texto sin conexión e [importar el runbook](manage-runbooks.md) en Azure Automation. 

>[!NOTE]
> PowerShell 7.1 no admite runbooks de flujo de trabajo.

### <a name="advantages"></a>Ventajas

* Implemente toda la lógica compleja con código del flujo de trabajo de PowerShell.
* Use los [puntos de control](automation-powershell-workflow.md#use-checkpoints-in-a-workflow) para reanudar la operación si se produce un error.
* Utilice el [procesamiento paralelo](automation-powershell-workflow.md#use-parallel-processing) para realizar varias acciones en paralelo.
* Se pueden incluir otros runbooks gráficos o de flujo de trabajo de PowerShell como runbooks secundarios para crear flujos de trabajo de alto nivel.

### <a name="limitations"></a>Limitaciones

* Debe estar familiarizado con el flujo de trabajo de PowerShell.
* Los runbooks deben tratar la complejidad adicional del flujo de trabajo de PowerShell como [objetos deserializados](automation-powershell-workflow.md#deserialized-objects).
* Los runbooks tardan más en iniciarse que los runbooks de PowerShell, ya que deben compilarse antes de su ejecución.
* Solo puede incluir runbooks de PowerShell como runbooks secundarios mediante el cmdlet `Start-AzAutomationRunbook`.
* Los runbooks no se pueden ejecutar en una instancia de Hybrid Runbook Worker de Linux.

## <a name="python-runbooks"></a>Runbooks de Python

Los runbooks de Python utilizan Python 2 y Python 3. Actualmente, los runbooks de Python 3 están en versión preliminar. Puede modificar directamente el código del runbook con el editor de texto en Azure Portal. También puede usar cualquier editor de texto sin conexión e [importar el runbook](manage-runbooks.md) en Azure Automation.

Los runbooks de Python 3 se admiten en las siguientes infraestructuras globales de Azure:

* Azure Global
* Azure Government

### <a name="advantages"></a>Ventajas

* Use las sólidas bibliotecas de Python.
* Se pueden ejecutar en Azure o en instancias de Hybrid Runbook Worker.
* En el caso de Python 2, las instancias de Hybrid Runbook Worker de Windows son compatibles si [Python2.7](https://www.python.org/downloads/release/latest/python2) está instalado.
* Para los trabajos en la nube de Python 3, se admite la versión 3.8 de Python. Es posible que los scripts y paquetes de cualquier versión 3.x funcionen si el código es compatible con distintas versiones.  
* En el caso de los trabajos híbridos de Python 3 en máquinas Windows, puede instalar cualquier versión 3. x que desee usar.  
* Para los trabajos híbridos de Python 3 para Linux, depende de la versión de Python 3 instalada en el equipo para ejecutar DSC OMSConfig y Hybrid Worker para Linux. Se recomienda instalar la versión 3.6 en máquinas Linux. Sin embargo, otras versiones también deberían funcionar si no hay cambios importantes en los contratos o firmas de método entre versiones de Python 3.

### <a name="limitations"></a>Limitaciones

* Debe estar familiarizado con el scripting de Python.
* Para utilizar bibliotecas de terceros, debe [importar los paquetes](python-packages.md) a la cuenta de Automation.
* Usar el cmdlet **Start-AutomationRunbook** en PowerShell o en el flujo de trabajo de PowerShell para iniciar un runbook de Python 3 (versión preliminar) no funciona. Puede usar el cmdlet  **Start-AzAutomationRunbook** desde el nódulo Az.Automation o el cmdlet  **Start-AzureRmAutomationRunbook** del módulo AzureRm.Automation para solucionar esta limitación.  
* Azure Automation no admite  **sys.stderr**.

### <a name="multiple-python-versions"></a>Varias versiones de Python

En el caso de Runbook Worker de Windows, al ejecutar un runbook de Python 2, este busca primero la variable de entorno `PYTHON_2_PATH` y valida si apunta a un archivo ejecutable válido. Por ejemplo, si la carpeta de instalación es `C:\Python2`, comprobaría si `C:\Python2\python.exe` es una ruta de acceso válida. Si no se encuentra, busca la variable de entorno `PATH` para realizar una comprobación similar.

Para Python 3, primero busca la variable env `PYTHON_3_PATH` y, a continuación, recurre a la variable de entorno `PATH`.

Cuando se usa solo una versión de Python, puede agregar la ruta de instalación a la variable `PATH`. Si quiere usar ambas versiones en Runbook Worker, establezca `PYTHON_2_PATH` y `PYTHON_3_PATH` en la ubicación del módulo para esas versiones.

### <a name="known-issues"></a>Problemas conocidos

En el caso de los trabajos en la nube, a veces se produce un error en los trabajos de Python 3 con un mensaje de excepción `invalid interpreter executable path`. Podría ver esta excepción si el trabajo se retrasa y empieza más de 10 minutos tarde o usa **Start-AutomationRunbook** para iniciar runbooks de Python 3. Si el trabajo se retrasa, reiniciar el runbook debería ser suficiente. Los trabajos híbridos deben funcionar sin ningún problema si se usan los pasos siguientes:

1. Cree una nueva variable de entorno denominada `PYTHON_3_PATH` y especifique la carpeta de instalación. Por ejemplo, si la carpeta de instalación es `C:\Python3`, debe agregarse esta ruta de acceso a la variable.
1. Reinicie la máquina después de establecer la variable de entorno.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los runbooks de PowerShell, consulte [Tutorial: Creación de un runbook de PowerShell](./learn/powershell-runbook-managed-identity.md).
* Para mas información sobre los runbooks de flujo de trabajo de PowerShell, consulte [Tutorial: Creación de un runbook de flujo de trabajo de PowerShell](learn/automation-tutorial-runbook-textual.md).
* Para información sobre los runbooks gráficos, consulte [Tutorial: Creación de un runbook gráfico](./learn/powershell-runbook-managed-identity.md).
* Para información sobre los runbooks de Python, consulte [Tutorial: Creación de un runbook de Python](./learn/automation-tutorial-runbook-textual-python-3.md).