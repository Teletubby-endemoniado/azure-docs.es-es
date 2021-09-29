---
title: Uso de Azure AD en Automation para autenticarse en Azure
description: En este artículo se muestra cómo usar Azure AD en Azure Automation como proveedor de autenticación para Azure.
services: automation
ms.date: 09/23/2021
ms.topic: conceptual
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: a5bb9b4d8235d48f47b34613c78d68f8bd3a2435
ms.sourcegitcommit: 10029520c69258ad4be29146ffc139ae62ccddc7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2021
ms.locfileid: "129080005"
---
# <a name="use-azure-ad-to-authenticate-to-azure"></a>Uso de Azure AD para autenticarse en Azure

El servicio [Azure Active Directory (AD)](../active-directory/fundamentals/active-directory-whatis.md) permite realizar una serie de tareas administrativas, como la administración de usuarios, la administración de dominios y la configuración del inicio de sesión único. En este artículo se describe cómo usar Azure AD en Azure Automation como proveedor de autenticación para Azure. 

## <a name="install-azure-ad-modules"></a>Instalación de módulos de Azure AD

Puede habilitar Azure AD a través de los siguientes módulos de PowerShell:

* Azure Active Directory PowerShell para Graph (módulos AzureRM y Az). Azure Automation incluye el módulo AzureRM y su actualización reciente, el módulo Az. Entre sus funcionalidades de incluye la autenticación no interactiva en Azure mediante la autenticación basada en credenciales del usuario (OrgId) de Azure AD. Consulte [Azure AD 2.0.2.76](https://www.powershellgallery.com/packages/AzureAD/2.0.2.76).

* Microsoft Azure Active Directory para Windows PowerShell versión (módulo MSOnline). Este módulo habilita las interacciones con Microsoft Online, incluido Microsoft 365.

>[!NOTE]
>PowerShell Core no es compatible con el módulo MSOnline. Para usar los cmdlets de módulo, debe ejecutarlos desde Windows PowerShell. Se recomienda usar la versión más reciente de los módulos de Azure Active Directory PowerShell para Graph en lugar del módulo MSOnline. 

### <a name="preinstallation"></a>Preinstalación

Antes de instalar los módulos de Azure AD en el equipo, haga lo siguiente:

* Desinstale todas las versiones anteriores del módulo AzureRM/Az y del módulo MSOnline. 

* Desinstale el Ayudante para el inicio de sesión de Microsoft Online Services para garantizar el correcto funcionamiento de los nuevos módulos de PowerShell.  

### <a name="install-the-azurerm-and-az-modules"></a>Instalación de los módulos AzureRM y Az

>[!NOTE]
>Para trabajar con estos módulos, debe usar la versión de PowerShell 5.1 o posterior con una versión de Windows de 64 bits. 

1. Instale Windows Management Framework (WMF) 5.1. Consulte [Instalación y configuración de WMF 5.1](/powershell/scripting/wmf/setup/install-configure).

2. Instale AzureRM o Az. Para ello, siga las instrucciones del artículo [Instalación de Azure PowerShell en Windows con PowerShellGet](/powershell/azure/azurerm/install-azurerm-ps).

### <a name="install-the-msonline-module"></a>Instalación del módulo MSOnline

>[!NOTE]
>Para instalar el módulo MSOnline, debe ser miembro de un rol de administrador. Consulte [Acerca de los roles de administrador](/microsoft-365/admin/add-users/about-admin-roles).

1. Asegúrese de que la característica Microsoft .NET Framework 3.5.x está habilitada en el equipo. Probablemente el equipo tiene instalada una versión más reciente, pero se puede habilitar o deshabilitar la compatibilidad con versiones anteriores de .NET Framework. 

2. Instale la versión de 64 bits del [Ayudante para el inicio de sesión de Microsoft Online Services](/microsoft-365/enterprise/connect-to-microsoft-365-powershell?view=o365-worldwide&preserve-view=true#step-1-install-the-required-software-1).

3. Ejecute Windows PowerShell como administrador para crear un símbolo del sistema de Windows PowerShell con privilegios elevados.

4. Implemente Azure Active Directory desde [MSOnline 1.0](http://www.powershellgallery.com/packages/MSOnline/1.0).

5. Si se le pide que instale el proveedor de NuGet, escriba Y (Sí) y presione Entrar.

6. Si se le pide que instale el módulo de [PSGallery](https://www.powershellgallery.com/), escriba Y (Sí) y presione Entrar.

### <a name="install-support-for-pscredential"></a>Instalación de la compatibilidad con PSCredential

Azure Automation usa la clase [PSCredential](/dotnet/api/system.management.automation.pscredential) para representar un recurso de credencial. Los scripts recuperan los objetos `PSCredential` mediante el cmdlet `Get-AutomationPSCredential`. Para obtener más información, consulte [Recursos de credenciales en Azure Automation](shared-resources/credentials.md).

## <a name="assign-a-subscription-administrator"></a>Asignación de un administrador de suscripción

Debe asignar un administrador para la suscripción a Azure. Esta persona tiene el rol de propietario para el ámbito de la suscripción. Consulte [Control de acceso basado en rol en Azure Automation](automation-role-based-access-control.md). 

## <a name="change-the-azure-ad-users-password"></a>Cambio de la contraseña del usuario de Azure AD

Para cambiar la contraseña del usuario de Azure AD, haga lo siguiente:

1. Cierre la sesión de Azure.

2. Haga que el administrador inicie sesión en Azure con el usuario de Azure AD que acaba de crear, mediante el nombre de usuario completo (incluido el dominio) y una contraseña temporal. 

3. Pida al administrador que cambie la contraseña cuando se le solicite.

## <a name="configure-azure-automation-to-manage-the-azure-subscription"></a>Configuración de Azure Automation para administrar la suscripción a Azure

Para que Azure Automation se comunique con Azure AD, debe recuperar las credenciales asociadas con la conexión entre Azure y Azure AD. Algunos ejemplos de estas credenciales son el id. de inquilino, el id. de suscripción y otras similares. Para obtener más información sobre la conexión entre Azure y Azure AD, consulte [Conexión de la organización a Azure Active Directory](/azure/devops/organizations/accounts/connect-organization-to-azure-ad).

## <a name="create-a-credential-asset"></a>Creación de un recurso de credencial

Ahora que las credenciales de Azure para Azure AD están disponibles, es momento de crear un recurso de credencial de Azure Automation que almacene de forma segura las credenciales de Azure AD. De este modo, los runbooks y los scripts de Desired State Configuration (DSC) podrán tener acceso a ellas. Para ello, pueden usar Azure Portal o los cmdlets de PowerShell.

### <a name="create-the-credential-asset-in-azure-portal"></a>Creación del recurso de credencial en Azure Portal

Puede usar Azure Portal para crear el recurso de credencial. Realice esta operación desde la cuenta de Automation. Para ello, seleccione **Credenciales** en **Recursos compartidos**. Consulte [Recursos de credenciales en Azure Automation](shared-resources/credentials.md).

### <a name="create-the-credential-asset-with-windows-powershell"></a>Creación de un recurso de credencial con Windows PowerShell

Para preparar un nuevo recurso de credencial en Windows PowerShell, el script debe crear primero un objeto `PSCredential` con el nombre de usuario y la contraseña asignados. Después, el script usa este objeto para crear el recurso a través de una llamada al cmdlet [New-AzureAutomationCredential](/powershell/module/servicemanagement/azure.service/new-azureautomationcredential). El script también podría llamar al cmdlet [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential) para solicitar al usuario que escriba un nombre y una contraseña. Consulte [Recursos de credenciales en Azure Automation](shared-resources/credentials.md). 

## <a name="manage-azure-resources-from-an-azure-automation-runbook"></a>Administración de recursos de Azure desde un runbook de Azure Automation

Puede administrar los recursos de Azure desde los runbooks de Azure Automation mediante el recurso de credencial. A continuación se muestra un ejemplo de runbook de PowerShell que recopila al recurso de credencial que se usará para detener e iniciar máquinas virtuales en una suscripción a Azure. En primer lugar, este runbook usa `Get-AutomationPSCredential` para recuperar la credencial que se usará para autenticarse en Azure. A continuación, llama al cmdlet [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) para conectarse a Azure mediante la credencial. 

```powershell
Workflow Workflow
{ 
    Param 
    (    
        [Parameter(Mandatory=$true)][ValidateNotNullOrEmpty()] 
        [String] 
        $AzureSubscriptionId, 
        [Parameter(Mandatory=$true)][ValidateNotNullOrEmpty()] 
        [String] 
        $AzureVMList="All", 
        [Parameter(Mandatory=$true)][ValidateSet("Start","Stop")] 
        [String] 
        $Action 
    ) 
     
    # Ensures you do not inherit an AzContext in your runbook
    Disable-AzContextAutosave -Scope Process

    # Connect to Azure with system-assigned managed identity
    $AzureContext = (Connect-AzAccount -Identity).context

    # set and store context
    $AzureContext = Set-AzContext -SubscriptionName $AzureContext.Subscription -DefaultProfile $AzureContext 

    # get credential
    $credential = Get-AutomationPSCredential -Name "AzureCredential"

    # Connect to Azure with credential
    $AzureContext = (Connect-AzAccount -Credential $credential -TenantId $AzureContext.Subscription.TenantId).context 

    # set and store context
    $AzureContext = Set-AzContext -SubscriptionName $AzureContext.Subscription `
        -TenantId $AzureContext.Subscription.TenantId `
        -DefaultProfile $AzureContext
 
    if($AzureVMList -ne "All") 
    { 
        $AzureVMs = $AzureVMList.Split(",") 
        [System.Collections.ArrayList]$AzureVMsToHandle = $AzureVMs 
    } 
    else 
    { 
        $AzureVMs = (Get-AzVM -DefaultProfile $AzureContext).Name 
        [System.Collections.ArrayList]$AzureVMsToHandle = $AzureVMs 
    } 
 
    foreach($AzureVM in $AzureVMsToHandle) 
    { 
        if(!(Get-AzVM -DefaultProfile $AzureContext | ? {$_.Name -eq $AzureVM})) 
        { 
            throw " AzureVM : [$AzureVM] - Does not exist! - Check your inputs " 
        } 
    } 
 
    if($Action -eq "Stop") 
    { 
        Write-Output "Stopping VMs"; 
        foreach -parallel ($AzureVM in $AzureVMsToHandle) 
        { 
            Get-AzVM -DefaultProfile $AzureContext | ? {$_.Name -eq $AzureVM} | Stop-AzVM -DefaultProfile $AzureContext -Force 
        } 
    } 
    else 
    { 
        Write-Output "Starting VMs"; 
        foreach -parallel ($AzureVM in $AzureVMsToHandle) 
        { 
            Get-AzVM -DefaultProfile $AzureContext | ? {$_.Name -eq $AzureVM} | Start-AzVM -DefaultProfile $AzureContext
        } 
    } 
}
```  

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre el uso de credenciales, vea [Administración de credenciales en Azure Automation](shared-resources/credentials.md).
* Para obtener más información sobre los módulos, vea [Administración de módulos en Azure Automation](shared-resources/modules.md).
* Si necesita iniciar un runbook, vea [Inicio de un runbook en Azure Automation](start-runbooks.md).
* Para obtener información sobre PowerShell, vea [Documentación de PowerShell](/powershell/scripting/overview).
