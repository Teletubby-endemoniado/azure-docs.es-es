---
title: Introducción a Desired State Configuration de Azure
description: Aprenda a usar el controlador de extensiones de Microsoft Azure para Desired State Configuration (DSC) de PowerShell. El artículo incluye los requisitos previos, la arquitectura y los cmdlets.
services: virtual-machines
documentationcenter: ''
author: mgoedtel
manager: evansma
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines
ms.subservice: extensions
ms.collection: windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 07/13/2020
ms.author: magoedte
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 463b806994364083c22dcfc40fd8b78dae87d8da
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132488779"
---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Introducción al controlador de extensiones de configuración de estado deseado de Azure

El agente de máquina virtual de Azure y las extensiones asociadas forman parte de los servicios de infraestructura de Microsoft Azure. Las extensiones de máquina virtual son componentes de software que amplían la funcionalidad de dicha máquina virtual y simplifican diversas operaciones de administración de la máquina virtual.

> [!NOTE]
> Antes de habilitar la extensión DSC, debe saber que ahora hay una versión más reciente de DSC en versión preliminar, administrada mediante una característica de Azure Policy de nombre [configuración de invitado](../../governance/policy/concepts/guest-configuration.md). La característica de configuración de invitado combina características de la extensión Desired State Configuration (DSC), Azure Automation State Configuration y las características que más solicitan los clientes en sus comentarios. La configuración de invitado también incluye la compatibilidad con máquinas híbridas a través de [servidores habilitados para Arc](../../azure-arc/servers/overview.md).

El caso de uso principal de la extensión Desired State Configuration (DSC) de Azure es arrancar una máquina virtual en el [servicio State Configuration (DSC) de Azure Automation](../../automation/automation-dsc-overview.md).
El servicio proporciona [ventajas](/powershell/scripting/dsc/managing-nodes/metaConfig#pull-service) que incluyen la administración continua de la configuración de máquinas virtuales y la integración con otras herramientas operativas, como Supervisión de Azure.
El uso de la extensión para registrar las máquinas virtuales para el servicio proporciona una solución flexible que funciona incluso en las suscripciones de Azure.

Puede utilizar la extensión DSC independientemente del servicio DSC de Automatización.
Sin embargo, esto solo insertará una configuración en la máquina virtual.
No hay ningún informe disponible, excepto los locales en la máquina virtual.

Este artículo proporciona información sobre ambos escenarios de uso de la extensión DSC: para la incorporación de Automation y como herramienta para asignar configuraciones a las máquinas virtuales mediante Azure SDK.

## <a name="prerequisites"></a>Prerrequisitos

- **Máquina local**: Para interactuar con la extensión de VM de Azure, debe usar Azure Portal o el SDK de Azure PowerShell.
- **Agente invitado**: La VM de Azure que cuenta con la configuración de DSC debe tener un SO que admita Windows Management Framework (WMF) 4.0 o posterior. La lista completa de las versiones compatibles del SO puede encontrarse en el [historial de versiones de la extensión DSC](../../automation/automation-dsc-extension-history.md).

## <a name="terms-and-concepts"></a>Términos y conceptos

Esta guía asume que está familiarizado con los conceptos siguientes:

- **Configuración**: Un documento de configuración de DSC.
- **Nodo**: Un destino para una configuración de DSC. En este documento, *nodo* siempre hace referencia a una máquina virtual de Azure.
- **Datos de configuración**: Un archivo .psd1 que tiene datos del entorno de una configuración.

## <a name="architecture"></a>Architecture

La extensión DSC de Azure usa el marco del agente de máquina virtual de Azure para entregar y aplicar las configuraciones de DSC que se ejecutan en las máquinas virtuales de Azure, así como informar sobre estas. La extensión DSC acepta un documento de configuración y un conjunto de parámetros. Si no se proporciona ningún archivo, se inserta un [script de configuración predeterminada](#default-configuration-script) con la extensión. El script de configuración predeterminada se usa únicamente para establecer metadatos en el [administrador de configuración Local](/powershell/scripting/dsc/managing-nodes/metaConfig).

Cuando se llama a la extensión por primera vez, se instala una versión de WMF usando la siguiente lógica:

- Si el SO de la máquina virtual de Azure es Windows Server 2016, no se realiza ninguna acción. Windows Server 2016 ya tiene instalada la versión más reciente de PowerShell.
- Si está especificada la propiedad **wmfVersion**, se instala esta versión de WMF, salvo que dicha versión sea incompatible con el SO de la máquina virtual.
- Si no hay ninguna propiedad **wmfVersion** especificada, se instala la versión aplicable más reciente de WMF.

La instalación de WMF requiere un reinicio. Después del reinicio, la extensión descarga el archivo .zip que se especifica en la propiedad **modulesUrl**, si se proporciona. Si esta ubicación se encuentra en Azure Blob Storage, se puede especificar un token de SAS en la propiedad **sasToken** para acceder al archivo. Después de que el archivo .zip se descargue y descomprima, se ejecuta la función de configuración definida en **configurationFunction** para generar un archivo .mof ([Managed Object Format](/windows/win32/wmisdk/managed-object-format--mof-)). Luego, la extensión ejecuta `Start-DscConfiguration -Force` con el archivo .mof generado. La extensión captura la salida y la escribe en el canal de estado de Azure.

### <a name="default-configuration-script"></a>Script de configuración predeterminada

La extensión DSC de Azure incluye un script de configuración predeterminada diseñado para utilizarse al incorporar una máquina virtual al servicio DSC de Azure Automation. Los parámetros del script se alinean con las propiedades configurables del [administrador de configuración local](/powershell/scripting/dsc/managing-nodes/metaConfig). Para los parámetros de script, consulte [Script de configuración predeterminada](dsc-template.md#default-configuration-script) en [Extensión Desired State Configuration con plantillas de Azure Resource Manager](dsc-template.md). Para el script completo, consulte la [plantilla de inicio rápido de Azure en GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/demos/azmgmt-demo/nestedtemplates/scripts/UpdateLCMforAAPull.zip).

## <a name="information-for-registering-with-azure-automation-state-configuration-dsc-service"></a>Información para el registro con el servicio State Configuration (DSC) de Azure Automation

Al usar la extensión DSC para registrar un nodo con el servicio de State Configuration, deberán proporcionarse tres valores.

- RegistrationUrl: dirección https de la cuenta de Azure Automation
- RegistrationKey: secreto compartido que se usa para registrar los nodos con el servicio
- NodeConfigurationName: nombre de la configuración del nodo (MOF) para extraer desde el servicio y configurar el rol de servidor

Esta información se encuentra en Azure Portal, pero también se puede usar PowerShell.

```powershell
(Get-AzAutomationRegistrationInfo -ResourceGroupName <resourcegroupname> -AutomationAccountName <accountname>).Endpoint
(Get-AzAutomationRegistrationInfo -ResourceGroupName <resourcegroupname> -AutomationAccountName <accountname>).PrimaryKey
```

Para el nombre de la configuración del nodo, asegúrese de que la configuración del nodo existe en Azure State Configuration.  Si no es así, la implementación de extensión devolverá un error.  Además, asegúrese de que usa el nombre de la *Configuración del nodo* y no de Configuración.
La configuración se define en un script que se usa para [compilar la Configuración del nodo (archivo MOF)](../../automation/automation-dsc-compile.md).
El nombre será siempre la Configuración seguida por un punto `.` y, o bien, `localhost`, o bien, un nombre de equipo específico.

## <a name="dsc-extension-in-resource-manager-templates"></a>Extensión DSC en plantillas de Resource Manager

En la mayoría de los escenarios, las plantillas de implementación de Resource Manager son la manera esperada de trabajar con la extensión DSC. Para más información y ejemplos sobre cómo incluir la extensión DSC en las plantillas de implementación de Resource Manager, vea [Extensión Desired State Configuration con plantillas de Azure Resource Manager](dsc-template.md).

## <a name="dsc-extension-powershell-cmdlets"></a>Cmdlets de PowerShell de la extensión DSC

Los cmdlets de PowerShell que se usan para administrar la extensión DSC son útiles para solucionar problemas interactivos y escenarios de recopilación de información. Puede usar los cmdlets para empaquetar, publicar y supervisar las implementaciones de la extensión DSC. Los cmdlets para la extensión DSC aún no se han actualizado para trabajar con el [script de configuración predeterminada](#default-configuration-script).

El cmdlet **Publish-AzVMDscConfiguration** toma un archivo de configuración, busca los recursos de DSC dependientes y, luego, crea un archivo zip. El archivo zip contiene la configuración y recursos de DSC que son necesarios para aplicar la configuración. El cmdlet también puede crear el paquete localmente mediante el parámetro *-OutputArchivePath*. En caso contrario, el cmdlet publica el archivo .zip en Blob Storage y lo protege con un token de SAS.

El script de configuración .ps1 que el cmdlet crea se encuentra en el archivo .zip en la raíz de la carpeta de archivos. La carpeta del módulo se encuentra en la carpeta de archivos en los recursos.

El cmdlet **Set-AzVMDscExtension** inserta la configuración que la extensión DSC de PowerShell necesita en un objeto de configuración de VM.

El cmdlet **Get-AzVMDscExtension** recupera el estado de la extensión DSC de una VM específica.

El cmdlet **Get-AzVMDscExtensionStatus** recupera el estado de la configuración de DSC que el controlador de la extensión DSC aplica. Esta acción puede realizarse en una sola máquina virtual o en un grupo de máquinas virtuales.

El cmdlet **Remove-AzVMDscExtension** quita el controlador de extensiones de una VM específica. Este cmdlet *no* quita la configuración, ni desinstala WMF ni cambia la configuración aplicada en la máquina virtual. Solo quita el controlador de extensiones.

Información importante acerca de los cmdlets de la extensión DSC de Resource Manager:

- Los cmdlets de Azure Resource Manager son sincrónicos.
- *ResourceGroupName*, *VMName*, *ArchiveStorageAccountName*, *Version* y *Location* son todos parámetros obligatorios.
- *ArchiveResourceGroupName* es un parámetro opcional. Se puede especificar este parámetro cuando la cuenta de almacenamiento pertenezca a un grupo de recursos que no sea el mismo que en el que se crea la máquina virtual.
- Use el modificador **AutoUpdate** para actualizar automáticamente el controlador de extensiones a la versión más reciente cuando esté disponible. Este parámetro puede provocar que la máquina virtual se reinicie cuando se lance una nueva versión de WMF.

### <a name="get-started-with-cmdlets"></a>Introducción a los cmdlets

La extensión DSC de Azure puede usar documentos de configuración de DSC para configurar directamente las máquinas virtuales de Azure durante la implementación. Este paso no registra el nodo en Automation. El nodo *no* se administrada centralmente.

En el ejemplo siguiente se muestra un ejemplo sencillo de una configuración. Guarde la configuración de forma local como iisInstall.ps1.

```powershell
configuration IISInstall
{
    node "localhost"
    {
        WindowsFeature IIS
        {
            Ensure = "Present"
            Name = "Web-Server"
        }
    }
}
```

Los siguientes comandos colocan el script iisInstall.ps1 en la máquina virtual especificada. Los comandos también ejecutan la configuración y luego informan sobre el estado.

```powershell
$resourceGroup = 'dscVmDemo'
$vmName = 'myVM'
$storageName = 'demostorage'
#Publish the configuration script to user storage
Publish-AzVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzVMDscExtension -Version '2.76' -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName 'iisInstall.ps1.zip' -AutoUpdate -ConfigurationName 'IISInstall'
```

## <a name="azure-cli-deployment"></a>Implementación de la CLI de Azure

La CLI de Azure puede utilizarse para implementar la extensión DSC en una máquina virtual existente.

Para máquinas virtuales que ejecutan Windows:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name DSC \
  --publisher Microsoft.Powershell \
  --version 2.77 --protected-settings '{}' \
  --settings '{}'
```

Para máquinas virtuales que ejecutan Linux:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name DSCForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 2.7 --protected-settings '{}' \
  --settings '{}'
```

## <a name="azure-portal-functionality"></a>Funcionalidad del Portal de Azure

Para configurar DSC en el portal:

1. Vaya a la máquina virtual.
2. En **Configuración**, seleccione **Extensiones**.
3. En la página nueva que se creó, seleccione **+ Agregar** y, luego, seleccione **PowerShell Desired State Configuration** (Desired State Configuration de PowerShell).
4. Haga clic en **Crear** en la parte inferior de la página de información de la extensión.

El portal recopila la siguiente información:

- **Módulos o script de configuración**: Este campo es obligatorio (el formulario no se ha actualizado para el [script de configuración predeterminada](#default-configuration-script)). Los scripts y módulos de configuración requieren un archivo. ps1 que tiene un script de configuración o un archivo .zip con un script de configuración. ps1 en la raíz. Si utiliza un archivo .zip, todos los recursos dependientes se deben incluir en las carpetas de módulo en el archivo .zip. Puede crear el archivo ZIP mediante el cmdlet **Publish-AzureVMDscConfiguration -OutputArchivePath** que se incluye en el SDK de Azure PowerShell. El archivo .zip se carga en el almacenamiento de blob de usuario y se protege mediante un token de SAS.

- **Nombre completo del módulo de configuración**: Puede incluir varias funciones de configuración en un archivo .ps1. Escriba el nombre del script .ps1 de configuración seguido de \\ y el nombre de la función de configuración. Por ejemplo, si el script .ps1 tiene el nombre configuration.ps1 y la configuración es **IisInstall**, escriba **configuration.ps1\IisInstall**.

- **Argumentos de configuración**: Si la función de configuración adopta argumentos, escríbalos aquí con el formato **nombreArgumento1=valor1,nombreArgumento2=valor2**. Este formato es un formato distinto en el que se aceptan argumentos de configuración en cmdlets de PowerShell o las plantillas de Resource Manager.

- **Configuration Data PSD1 File** (Archivo PSD1 de datos de configuración): Si la configuración requiere un archivo de datos de configuración en `.psd1`, use este campo para seleccionar el archivo de datos y cargarlo en el almacenamiento de blobs de usuario. El archivo de datos de configuración se protege mediante un token SAS en el almacenamiento de blobs.

- **Versión de WMF**: Especifica la versión de Windows Management Framework (WMF) que debe instalarse en la máquina virtual. Si se establece esta propiedad en latest, se instala la versión más reciente de WMF. Actualmente, los únicos valores posibles para esta propiedad son 4.0, 5.0, 5.1 y latest. Estos valores posibles están sujetos a actualizaciones. El valor predeterminado es **latest**.

- **Recopilación de datos**: Determina si la extensión recopilará datos de telemetría. Para más información, consulte [Azure DSC extension data collection](https://devblogs.microsoft.com/powershell/azure-dsc-extension-data-collection-2/) (Colección de datos de la extensión DSC de Azure).

- **Versión**: Especifica la versión de la extensión DSC que se va a instalar. Para obtener información sobre las versiones, consulte [DSC extension version history](/powershell/scripting/dsc/getting-started/azuredscexthistory) (Historial de versiones de la extensión DSC).

- **Auto Upgrade Minor Version** (Actualización automática de versión secundaria): Este campo se asigna al modificador **AutoUpdate** en los cmdlets y permite que la extensión se actualice automáticamente a la versión más reciente durante la instalación. **Sí** indicará al controlador de la extensión que use la versión más reciente disponible, y **No** forzará la instalación de la **Versión** especificada. No seleccionar ni **Sí** ni **No** es igual que seleccionar **No**.

## <a name="logs"></a>Registros

Los registros de la extensión se almacenan en la siguiente ubicación: `C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\<version number>`

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre DSC de PowerShell, vaya al [centro de documentación de PowerShell](/powershell/scripting/dsc/overview/overview).
- Examine la [plantilla de Resource Manager para la extensión DSC](dsc-template.md).
- Para obtener más funcionalidades que pueda administrar con DSC de PowerShell y más recursos de DSC, examine la [Galería de PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
- Para detalles sobre cómo pasar parámetros confidenciales a configuraciones, consulte [Pasar credenciales al controlador de extensiones DSC de Azure](dsc-credentials.md).
