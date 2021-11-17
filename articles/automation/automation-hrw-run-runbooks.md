---
title: Ejecución de runbooks de Azure Automation en una instancia de Hybrid Runbook Worker
description: En este artículo se describe cómo ejecutar runbooks en máquinas del centro de datos local o en otro proveedor de nube con la instancia de Hybrid Runbook Worker.
services: automation
ms.subservice: process-automation
ms.date: 11/11/2021
ms.topic: conceptual
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 368622d7f0ea914541ce1385405a40e28ca2576b
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132282203"
---
# <a name="run-automation-runbooks-on-a-hybrid-runbook-worker"></a>Ejecución de runbooks de Automation en una instancia de Hybrid Runbook Worker

Los runbooks que se ejecutan en una instancia de [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) normalmente administran recursos en el equipo local o en recursos del entorno local en el que se implementa el rol de trabajo. Los runbooks en Azure Automation normalmente administran recursos en la nube de Azure. Aunque se usan de manera diferente, los runbooks que se ejecutan en Azure Automation y los runbooks que se ejecutan en una instancia de Hybrid Runbook Worker son idénticos en estructura.

Cuando cree un runbook para que se ejecute en una instancia de Hybrid Runbook Worker, debe editarlo y probarlo en la máquina que hospeda el rol de trabajo. La máquina host tiene todos los módulos de PowerShell y el acceso de red necesarios para administrar los recursos locales. Una vez que un runbook se ha probado en la máquina de Hybrid Runbook Worker, puede cargarlo en el entorno de Azure Automation, donde se puede ejecutar en el rol de trabajo.

## <a name="plan-for-azure-services-protected-by-firewall"></a>Planeación de servicios de Azure protegidos por firewall

La habilitación de Azure Firewall en [Azure Storage](../storage/common/storage-network-security.md), [Azure Key Vault](../key-vault/general/network-security.md) o [Azure SQL](../azure-sql/database/firewall-configure.md) bloquea el acceso desde runbooks de Azure Automation para esos servicios. El acceso se bloqueará incluso cuando la excepción de firewall para permitir servicios de Microsoft de confianza esté habilitada, ya que Automation no forma parte de la lista de servicios de confianza. Con un firewall habilitado, el acceso solo se puede realizar mediante una instancia de Hybrid Runbook Worker y un [punto de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="plan-runbook-job-behavior"></a>Planeación del comportamiento del trabajo de runbook

Azure Automation controla los trabajos en Hybrid Runbook Worker de forma diferente a los trabajos que se ejecutan en espacios aislados de Azure. Si tiene un runbook de larga ejecución, asegúrese de que sea resistente a posibles reinicios. Para obtener detalles del comportamiento del trabajo, consulte [Trabajos de Hybrid Runbook Worker](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-jobs).

## <a name="service-accounts"></a>Cuentas de servicio

### <a name="windows"></a>Windows 

Los trabajos de instancias de Hybrid Runbook Worker se ejecutan en la cuenta de **sistema** local.
>[!NOTE]
>  Para ejecutar PowerShell 7.x en una instancia de Hybrid Runbook Worker de Windows, consulte [Instalación de PowerShell en Windows](/powershell/scripting/install/installing-powershell-on-windows).
> Actualmente, solo se admite la incorporación basada en extensiones de Hybrid Worker, tal como se mencionó [aquí](/azure/automation/extension-based-hybrid-runbook-worker-install). 

Asegúrese de que conoce la ruta de acceso donde se encuentra el ejecutable *pwsh.exe* y que se agrega a la variable de entorno PATH. Reinicie la instancia de Hybrid Runbook Worker una vez completada la instalación.

### <a name="linux"></a>Linux

>[!NOTE]
> Para ejecutar PowerShell 7.x en una instancia de Hybrid Runbook Worker de Linux, consulte [Instalación de PowerShell en Linux](/powershell/scripting/install/installing-powershell-on-linux).
> Actualmente, solo se admite la incorporación basada en extensiones de Hybrid Worker, tal como se mencionó [aquí](/azure/automation/extension-based-hybrid-runbook-worker-install).


Se crean las cuentas de servicio **nxautomation** y **omsagent**. El script de creación y asignación de permisos se puede ver en [https://github.com/microsoft/OMS-Agent-for-Linux/blob/master/installer/datafiles/linux.data](https://github.com/microsoft/OMS-Agent-for-Linux/blob/master/installer/datafiles/linux.data). La cuentas, con los permisos sudo correspondientes, deben estar presentes durante la [instalación de la instancia de Hybrid Runbook Worker en Linux](automation-linux-hrw-install.md). Si intenta instalar el trabajo y la cuenta no está presente o no tiene los permisos adecuados, se producirá un error de la instalación. No cambie los permisos de la carpeta `sudoers.d` ni su propiedad. Se requiere permiso sudo para las cuentas y no se deben quitar los permisos. Su restricción a determinadas carpetas o comandos puede dar lugar a un cambio importante. El usuario **nxautomation** habilitado como parte de Update Management solo ejecuta runbooks firmados.

Para asegurarse de que las cuentas de servicio tienen acceso a los módulos de runbook almacenados:

- Cuando use `pip install`, `apt install` u otro método para instalar paquetes en Linux, asegúrese de que el paquete se instale para todos los usuarios. Por ejemplo, `sudo -H pip install <package_name>`.
- Si usa [PowerShell en Linux](/powershell/scripting/whats-new/what-s-new-in-powershell-70), cuando utilice el cmdlet [Install-Module](/powershell/module/powershellget/install-module), asegúrese de especificar `AllUsers` para el parámetro `Scope`.

El registro de trabajo de Automation se encuentra en `/var/opt/microsoft/omsagent/run/automationworker/worker.log`.

Las cuentas de servicio se quitan cuando se quita la máquina como Hybrid Runbook Worker.

## <a name="configure-runbook-permissions"></a>Configuración de permisos de runbooks

Defina los permisos para que el runbook se ejecute en la instancia de Hybrid Runbook de las siguientes formas:

* Haga que el runbook proporcione su propia autenticación a los recursos locales.
* Configure la autenticación mediante [identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager).
* Especifique una cuenta de ejecución para proporcionar un contexto de usuario para todos los runbooks.

### <a name="use-runbook-authentication-to-local-resources"></a>Uso de la autenticación de runbooks en los recursos locales

Si prepara un runbook que proporciona su propia autenticación a los recursos, use los recursos [Credencial](./shared-resources/credentials.md) y [Certificado](./shared-resources/certificates.md) en el runbook. Hay varios cmdlets que le permiten especificar las credenciales, para que el runbook pueda autenticarse en los distintos recursos. El ejemplo siguiente muestra una porción de un runbook que reinicia un equipo. Recupera las credenciales desde un recurso de credencial y el nombre del equipo desde un recurso de variable y, a continuación, usa estos valores con el cmdlet `Restart-Computer`.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

También puede usar una actividad [InlineScript](automation-powershell-workflow.md#use-inlinescript). `InlineScript` permite ejecutar bloques de código en otro equipo con credenciales.

### <a name="use-runbook-authentication-with-managed-identities"></a><a name="runbook-auth-managed-identities"></a>Uso de la autenticación de runbooks con identidades administradas

Las instancias de Hybrid Runbook Worker en máquinas virtuales de Azure pueden usar identidades administradas para autenticarse en los recursos de Azure. El uso de identidades administradas para los recursos de Azure en lugar de cuentas de ejecución ofrece ventajas porque no es necesario:

* Exportar el certificado de ejecución y después importarlo en Hybrid Runbook Worker.
* Renovar el certificado utilizado por la cuenta de ejecución.
* Administrar el objeto de conexión de ejecución en el código del runbook.

Realice los pasos siguientes para usar una identidad administrada para los recursos de Azure en una instancia de Hybrid Runbook Worker:

1. Cree una máquina virtual de Azure.
1. Configure identidades administradas para los recursos de Azure en la máquina virtual. Consulte [Configuración de identidades administradas para recursos de Azure en una máquina virtual mediante Azure Portal](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm).
1. Conceda acceso a la máquina virtual a un grupo de recursos en Resource Manager. Consulte [Uso de una identidad administrada asignada por el sistema de una máquina virtual Windows para acceder a Resource Manager](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager).
1. Instale Hybrid Runbook Worker en la máquina virtual. Consulte [Implementación de Hybrid Runbook Worker en Windows](automation-windows-hrw-install.md) o [Implementación de Hybrid Runbook Worker en Linux](automation-linux-hrw-install.md).
1. Actualice el runbook para que use el cmdlet [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) con el parámetro `Identity` para autenticarse en los recursos de Azure. Esta configuración reduce la necesidad de usar una cuenta de ejecución y realizar la administración de las cuentas asociadas.

    ```powershell
    # Ensures you do not inherit an AzContext in your runbook
    Disable-AzContextAutosave -Scope Process
    
    # Connect to Azure with system-assigned managed identity
    $AzureContext = (Connect-AzAccount -Identity).context
    
    # set and store context
    $AzureContext = Set-AzContext -SubscriptionName $AzureContext.Subscription -DefaultProfile $AzureContext

    # Get all VM names from the subscription
    Get-AzVM -DefaultProfile $AzureContext | Select Name
    ```

    Si desea que el runbook se ejecute con la identidad administrada asignada por el sistema, deje el código tal y como está. Si prefiere usar una identidad administrada asignada por el usuario, haga lo siguiente:
    1. En la línea 5, quite `$AzureContext = (Connect-AzAccount -Identity).context`.
    1. Reemplácelo por `$AzureContext = (Connect-AzAccount -Identity -AccountId <ClientId>).context`.
    1. Escriba el id. de cliente.

### <a name="use-runbook-authentication-with-run-as-account"></a>Uso de la autenticación de runbooks con la cuenta de ejecución

En lugar de hacer que el runbook proporcione su propia autenticación a los recursos locales, puede especificar una cuenta de ejecución para un grupo de Hybrid Runbook Worker. Para especificar una cuenta de ejecución, debe definir un [recurso de credencial](./shared-resources/credentials.md) que tenga acceso a los recursos locales. Estos recursos incluyen almacenes de certificados y todos los runbooks se ejecutan con estas credenciales en una instancia de Hybrid Runbook Worker del grupo.

- El nombre de usuario de la credencial debe tener uno de los siguientes formatos:

   * dominio\nombre de usuario
   * username@domain
   * nombre de usuario (para cuentas locales en el equipo local)

- Para usar el runbook **de PowerShell Export-RunAsCertificateToHybridWorker**, debe instalar AZ modules for Azure Automation en el equipo local.

#### <a name="use-a-credential-asset-to-specify-a-run-as-account"></a>Uso de un recurso de credencial para especificar una cuenta de ejecución

Utilice el procedimiento siguiente para especificar una cuenta de ejecución para un grupo de Hybrid Runbook Worker:

1. Cree un [recurso de credencial](./shared-resources/credentials.md) con acceso a los recursos locales.
1. Abra la cuenta de Automation en Azure Portal.
1. Seleccione **Grupos de Hybrid Worker** y elija el grupo concreto.
1. Seleccione **Toda la configuración** y luego **Configuración del grupo de Hybrid Worker**.
1. Cambie el valor de **Ejecutar como** de **Predeterminado** a **Personalizado**.
1. Seleccione la credencial y haga clic en **Guardar**.

## <a name="install-run-as-account-certificate"></a><a name="runas-script"></a>Instalación del certificado de cuentas de ejecución

Como parte del proceso de compilación automatizado para la implementación de recursos en Azure, es posible que necesite acceder a los sistemas locales para admitir una tarea o un conjunto de pasos de la secuencia de implementación. Para proporcionar la autenticación en Azure mediante la cuenta de ejecución, debe instalar el certificado de la cuenta de ejecución.

>[!NOTE]
>Actualmente, este runbook de PowerShell no se ejecuta en máquinas Linux. Se ejecuta solo en máquinas Windows.
>

El siguiente runbook de PowerShell, llamado **Export-RunAsCertificateToHybridWorker**, exporta el certificado de ejecución de la cuenta de Azure Automation. El runbook descarga e importa el certificado en el almacén de certificados del equipo local en una instancia de Hybrid Runbook Worker que está conectada a la misma cuenta. Una vez que completa dicho paso, el runbook comprueba que el rol de trabajo se puede autenticar correctamente en Azure con la cuenta de ejecución.

>[!NOTE]
>Este runbook de PowerShell no está diseñado o pensado para ejecutarse fuera de la cuenta de Automation como un script en el equipo de destino.
>

```azurepowershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account. Run this runbook on the hybrid worker where you want the certificate installed. This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running on the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword | Write-Verbose

Remove-Item -Path $CertPath -ErrorAction SilentlyContinue | Out-Null

# Test to see if authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzAccount `
    -ServicePrincipal `
    -Tenant $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzContext -Subscription $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm that Azure Resource Manager calls are working
Get-AzAutomationAccount | Select-Object AutomationAccountName
```

>[!NOTE]
>En el caso de los runbooks de PowerShell, `Add-AzAccount` y `Add-AzureRMAccount` son alias de `Connect-AzAccount`. Al buscar los elementos de biblioteca, si no ve `Connect-AzAccount`, puede usar `Add-AzAccount` o puede actualizar los módulos en la cuenta de Automation.

Para finalizar la preparación de la cuenta de ejecución:

1. Guarde el runbook **Export-RunAsCertificateToHybridWorker** en el equipo con la extensión **.ps1**.
1. Impórtelo en la cuenta de Automation.
1. Edite el runbook y cambie el valor de la variable `Password` por su propia contraseña.
1. Publique el runbook.
1. Ejecute el runbook y establezca como destino el grupo de Hybrid Runbook Worker que ejecuta y autentica los runbooks con la cuenta de ejecución. 
1. Examine el flujo de trabajo para ver si informa del intento de importar el certificado al almacén de la máquina local, seguido de varias líneas. Este comportamiento depende de cuántas cuentas de Automation se definan en la suscripción y del grado de éxito de la autenticación.

>[!NOTE]
>  En caso de acceso sin restricciones, un usuario con derechos de colaborador de máquina virtual o que tenga permisos para ejecutar comandos en la máquina de Hybrid Worker puede utilizar el certificado de ejecución de la cuenta de Automation de la máquina de Hybrid Worker, utilizando otros orígenes como los cmdlets de Azure que podrían permitir potencialmente el acceso de un usuario malintencionado como colaborador de la suscripción. Esto podría poner en peligro la seguridad del entorno de Azure. </br> </br>
>  Se recomienda dividir las tareas dentro del equipo y conceder los permisos o acceso necesarios a los usuarios según su trabajo. No proporcione permisos sin restricciones a la máquina que hospeda el rol de trabajo de runbook.


## <a name="work-with-signed-runbooks-on-a-windows-hybrid-runbook-worker"></a>Trabajo con runbooks firmados en Hybrid Runbook Worker en Windows

Puede configurar una instancia de Hybrid Runbook Worker en Windows para que solo ejecute runbooks firmados.

> [!IMPORTANT]
> Cuando haya configurado una instancia de Hybrid Runbook Worker para que solo ejecute runbooks firmados, los runbooks que no estén firmados no se ejecutarán en el rol de trabajo.

> [!NOTE]
>  PowerShell 7.x no admite runbooks firmados para las instancias de Hybrid Runbook Worker de Windows y Linux.  

### <a name="create-signing-certificate"></a>Creación de certificado de firma

En el ejemplo siguiente se crea un certificado autofirmado que se puede usar para firmar runbooks. Este código crea el certificado y lo exporta para que Hybrid Runbook Worker pueda importarlo más adelante. También se devuelve la huella digital para su uso posterior al hacer referencia al certificado.

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
    -Subject "CN=contoso.com" `
    -KeyAlgorithm RSA `
    -KeyLength 2048 `
    -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
    -KeyExportPolicy Exportable `
    -KeyUsage DigitalSignature `
    -Type CodeSigningCert

# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

### <a name="import-certificate-and-configure-workers-for-signature-validation"></a>Importación del certificado y configuración de los roles de trabajo para la validación de firmas

Copie el certificado que ha creado en cada instancia de Hybrid Runbook Worker de un grupo. Ejecute el script siguiente para importar el certificado y configurar los roles de trabajo para que usen la validación de firma en los runbooks.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

### <a name="sign-your-runbooks-using-the-certificate"></a>Firma de los runbooks con el certificado

Con las instancias de Hybrid Runbook Worker configuradas para usar solo runbooks firmados, debe firmar los runbooks que se van a utilizar en Hybrid Runbook Worker. Use el código de PowerShell de ejemplo siguiente para firmar estos runbooks.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Una vez firmado el runbook, debe importarlo en la cuenta de Automation y publicarlo con el bloque de firma. Para aprender a importar runbooks, consulte [Importación de un runbook](manage-runbooks.md#import-a-runbook).

>[!NOTE]
>Use solo caracteres de texto no cifrado en el código del runbook, incluidos los comentarios. El uso de caracteres con marcas diacríticas, como á o ñ, producirá un error. Cuando Azure Automation descargue el código, los caracteres se reemplazarán por un signo de interrogación y se producirá un error en la firma con un mensaje de "error de validación de hash de firma".

## <a name="work-with-signed-runbooks-on-a-linux-hybrid-runbook-worker"></a>Trabajo con runbooks firmados en Hybrid Runbook Worker en Linux

Para poder trabajar con runbooks firmados, una instancia de Hybrid Runbook Worker en Linux debe tener [GPG](https://gnupg.org/index.html) ejecutable en el equipo local.

> [!IMPORTANT]
> Cuando haya configurado una instancia de Hybrid Runbook Worker para que solo ejecute runbooks firmados, los runbooks que no estén firmados no se ejecutarán en el rol de trabajo.

Realizará los pasos siguientes para completar esta configuración:

* Creación un conjunto de claves y un par de claves de GPG
* Puesta a disposición del conjunto de claves para Hybrid Runbook Worker
* Verificación de que la validación de firmas está activada
* Ejecución de un runbook

> [!NOTE]
>  PowerShell 7.x no admite runbooks firmados para las instancias de Hybrid Runbook Worker de Windows y Linux.

### <a name="create-a-gpg-keyring-and-keypair"></a>Creación un conjunto de claves y un par de claves de GPG

Para crear el conjunto de claves y el par de claves de GPG, utilice la [cuenta nxautomation](automation-runbook-execution.md#log-analytics-agent-for-linux) de Hybrid Runbook Worker.

1. Use la aplicación sudo para iniciar sesión como la cuenta **nxautomation**.

    ```bash
    sudo su - nxautomation
    ```

1. Una vez que use **nxautomation**, genere el par de claves de GPG. GPG le guía por los pasos. Debe proporcionar el nombre, la dirección de correo electrónico, la hora de expiración y la frase de contraseña. A continuación, espere hasta que haya suficiente entropía en el equipo para que se genere la clave.

    ```bash
    sudo gpg --generate-key
    ```

1. Dado que el directorio de GPG se generó con sudo, debe cambiar el propietario a **nxautomation** con el comando siguiente.

    ```bash
    sudo chown -R nxautomation ~/.gnupg
    ```

### <a name="make-the-keyring-available-to-the-hybrid-runbook-worker"></a>Puesta a disposición del conjunto de claves para Hybrid Runbook Worker

Una vez creado el conjunto de claves, debe ponerlo a disposición de Hybrid Runbook Worker. Modifique el archivo de configuración **home/nxautomation/state/worker.conf** para que incluya el siguiente código de ejemplo en la sección del archivo `[worker-optional]`.

```bash
gpg_public_keyring_path = /home/nxautomation/run/.gnupg/pubring.kbx
```

### <a name="verify-that-signature-validation-is-on"></a>Verificación de que la validación de firmas está activada

Si se ha deshabilitado la validación de firmas en el equipo, debe activarla; para ello, ejecute el siguiente comando de sudo. Reemplace `<LogAnalyticsworkspaceId>` por el identificador del área de trabajo.

```bash
sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --true <LogAnalyticsworkspaceId>
```

### <a name="sign-a-runbook"></a>Ejecución de un runbook

Cuando haya configurado la validación de firmas, use el siguiente comando de GPG para firmar el runbook.

```bash
gpg --clear-sign <runbook name>
```

El runbook firmado se llama **\<runbook name>.asc**.

El runbook firmado ahora se puede cargar en Azure Automation y se puede ejecutar como un runbook normal.

## <a name="start-a-runbook-on-a-hybrid-runbook-worker"></a>Inicio de un runbook en una instancia de Hybrid Runbook Worker

[Inicio de un runbook en Azure Automation](start-runbooks.md) describe los distintos métodos para iniciar un runbook. El inicio de un runbook en una instancia de Hybrid Runbook Worker usa la opción **Ejecutar en**, que le permite especificar el nombre de un grupo de Hybrid Runbook Worker. Si se especifica un grupo, uno de los roles de trabajo del grupo recupera y ejecuta el runbook. Si el runbook no especifica esta opción, Azure Automation ejecuta el runbook como de costumbre.

Cuando inicie un runbook en Azure Portal, verá la opción **Ejecutar en**, para la que puede seleccionar **Azure** o **Hybrid Worker**. Si selecciona **Hybrid Worker**, puede elegir el grupo de Hybrid Runbook Worker en una lista desplegable.

Al iniciar un runbook con PowerShell, use el parámetro `RunOn` con el cmdlet [Start-AzAutomationRunbook](/powershell/module/Az.Automation/Start-AzAutomationRunbook). En el ejemplo siguiente se usa Windows PowerShell para iniciar un runbook llamado **Test-Runbook** en un grupo de Hybrid Runbook Worker llamado MyHybridGroup.

```azurepowershell-interactive
Start-AzAutomationRunbook -AutomationAccountName "MyAutomationAccount" -Name "Test-Runbook" -RunOn "MyHybridGroup"
```

## <a name="logging"></a>Registro

Para ayudar a solucionar problemas con los runbooks que se ejecutan en un trabajo de runbook híbrido, los registros se almacenan localmente en la siguiente ubicación:

* En Windows en `C:\ProgramData\Microsoft\System Center\Orchestrator\<version>\SMA\Sandboxes` para el registro detallado del proceso en tiempo de ejecución del trabajo. Los eventos de estado de trabajo de runbook de alto nivel se escriben en el registro de eventos **Registros de aplicaciones y servicios\Microsoft-Automation\Operations**.

* En Linux, los registros de trabajo híbridos de usuario y los registros de trabajo de runbook del sistema se pueden encontrar en `/home/nxautomation/run/worker.log` y `/var/opt/microsoft/omsagent/run/automationworker/worker.log`, respectivamente.

## <a name="next-steps"></a>Pasos siguientes

* Si los runbooks no finalizan correctamente, revise la guía de solución de problemas sobre [errores de ejecución de un runbook](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).
* Para obtener más información sobre PowerShell, incluidos los módulos de referencia de lenguaje y aprendizaje, consulte la [documentación de PowerShell](/powershell/scripting/overview).
* Obtenga información sobre el [uso de Azure Policy para administrar la ejecución de runbooks](enforce-job-execution-hybrid-worker.md) con Hybrid Runbook Worker.
* Para ver una referencia de los cmdlets de PowerShell, consulte [Az.Automation](/powershell/module/az.automation).
