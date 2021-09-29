---
title: 'Creación de entornos y grupos de hosts de Azure Virtual Desktop: Azure'
description: Cómo solucionar problemas de grupos de inquilinos y de hosts durante la instalación de un entorno de Azure Virtual Desktop.
author: Heidilohr
ms.topic: troubleshooting
ms.custom: references_regions
ms.date: 02/17/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 7e2c4c6a68bb8db4434a233e7b7c658074ed015a
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124804120"
---
# <a name="host-pool-creation"></a>Creación de grupos de hosts

>[!IMPORTANT]
>Este contenido se aplica a Azure Virtual Desktop con objetos de Azure Resource Manager. Si usa Azure Virtual Desktop (clásico) sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/troubleshoot-set-up-issues-2019.md).

En este artículo se tratan problemas que se producen durante la instalación inicial del inquilino de Azure Virtual Desktop y la infraestructura del grupo de hosts de la sesión relacionada.

## <a name="provide-feedback"></a>Envío de comentarios

Visite la [comunidad técnica de Azure Virtual Desktop](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/bd-p/AzureVirtualDesktopForum) para analizar el servicio Azure Virtual Desktop con el equipo de producto y los miembros activos de la comunidad.

## <a name="acquiring-the-windows-10-enterprise-multi-session-image"></a>Adquisición de la imagen de sesión múltiple de Windows 10 Enterprise

Para usar la imagen de sesión múltiple de Windows 10 Enterprise, vaya a Azure Marketplace, seleccione **Comenzar** > **Microsoft Windows 10** > y [Sesión múltiple de Windows 10 Enterprise, versión 1809](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsdesktop.windows-10?tab=PlansAndPrice).

## <a name="issues-with-using-the-azure-portal-to-create-host-pools"></a>Problemas con el uso de Azure Portal para crear grupos de hosts

### <a name="error-create-a-free-account-appears-when-accessing-the-service"></a>Error: El mensaje "Crear una cuenta gratuita" aparece al acceder al servicio

> [!div class="mx-imgBorder"]
> ![Imagen que muestra Azure Portal con el mensaje "Crear una cuenta gratuita"](media/create-new-account.png)

**Causa**: No existen suscripciones activas en la cuenta con la que inició sesión en Azure, o bien la cuenta no tiene permisos para ver las suscripciones.

**Corrección**: Inicie sesión en la suscripción donde va a implementar las máquinas virtuales del host de sesión con una cuenta que tenga al menos acceso de nivel de colaborador.

### <a name="error-exceeding-quota-limit"></a>Error: "Se está excediendo el límite de cuota"

Si la operación supera el límite de cuota, puede realizar una de las siguientes acciones:

- Crear un nuevo grupo de hosts con los mismos parámetros pero menos máquinas virtuales y núcleos de máquinas virtuales.

- Abrir el vínculo que aparece en el campo statusMessage en un explorador para enviar una solicitud a fin de aumentar la cuota de su suscripción a Azure para la SKU de la máquina virtual especificada.

### <a name="error-cant-see-user-assignments-in-app-groups"></a>Error: Can't see user assignments in app groups. (No se pueden ver las asignaciones de usuario en los grupos de aplicaciones).

**Causa**: Este error suele producirse después de haber migrado la suscripción de un inquilino de Azure Active Directory (AD) a otro. Si las asignaciones antiguas todavía están vinculadas al inquilino antiguo de Azure AD, Azure Portal perderá el seguimiento de ellas.

**Solución**: Deberá volver a asignar los usuarios a los grupos de aplicaciones.

### <a name="i-only-see-us-when-setting-the-location-for-my-service-objects"></a>Solo veo "US" (EE. UU.) al establecer la ubicación de los objetos de servicio

**Causa**: Azure actualmente no admite esa región para el servicio Azure Virtual Desktop. Para más información sobre las zonas geográficas admitidas, consulte [Ubicaciones de datos](data-locations.md). Si Azure Virtual Desktop admite la ubicación pero todavía no aparece cuando está intentando seleccionar una ubicación, significa que el proveedor de recursos todavía no se ha actualizado.

**Corrección**: para obtener la lista más reciente de regiones, vuelva a registrar el proveedor de recursos:

1. Vaya a **Suscripciones** y seleccione la suscripción adecuada.
2. Seleccione **Proveedor de recursos**.
3. Seleccione **Microsoft.DesktopVirtualization** y, a continuación, seleccione **Volver a registrar** en el menú de acciones.

Al volver a registrar el proveedor de recursos, no verá ningún comentario o estado de actualización específico de la interfaz de usuario. El proceso de volver a registrar tampoco interferirá con los entornos existentes.

## <a name="azure-resource-manager-template-errors"></a>Errores de plantilla de Azure Resource Manager

Siga estas instrucciones para solucionar problemas de implementaciones incorrectas de DSC de PowerShell y plantillas de Azure Resource Manager.

1. Revise los errores en la implementación mediante [Ver operaciones de implementación con Azure Resource Manager](../azure-resource-manager/templates/deployment-history.md).
2. Si no hay ningún error en la implementación, revise los errores en el registro de actividad mediante [Ver registros de actividad para auditar las acciones sobre los recursos](../azure-monitor/essentials/activity-log.md).
3. Una vez identificado el error, utilice el mensaje de error y los recursos de [Solucionar errores comunes de implementación de Azure con Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md) para solucionar el problema.
4. Elimine los recursos creados durante la implementación anterior y vuelva a intentar implementar la plantilla.

### <a name="error-your-deployment-failedhostnamejoindomain"></a>Error: La implementación no se pudo…\<hostname>/joindomain

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de error de implementación.](media/failure-joindomain.png)

Ejemplo de error no procesado:

```Error
 {"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details.
 Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\": \"Failed\",\r\n \"error\":
 {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.
 \",\r\n \"details\": [\r\n {\r\n \"code\": \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has reported a failure when processing
 extension 'joindomain'. Error message: \\\"Exception(s) occurred while joining Domain 'diamondsg.onmicrosoft.com'\\\".\"\r\n }\r\n ]\r\n }\r\n}"}]}
```

**Causa 1:** Las credenciales proporcionadas para unir las máquinas virtuales al dominio son incorrectas.

**Corrección 1:** Vea el error "Credenciales incorrectas" cuando las máquinas virtuales no están unidas al dominio en [Configuración de máquina virtual del host de sesión](troubleshoot-vm-configuration.md).

**Causa 2:** No se resuelve el nombre de dominio.

**Corrección 2:** Consulte [Error: El nombre de dominio no se resuelve](troubleshoot-vm-configuration.md#error-domain-name-doesnt-resolve) en [Configuración de máquina virtual del host de sesión](troubleshoot-vm-configuration.md).

**Causa 3:** La configuración de DNS de la red virtual (VNET) está establecida en **Predeterminada**.

Para corregirlo, siga estos pasos:

1. Abra Azure Portal y vaya a la pestaña **Redes virtuales**.
2. Busque la red virtual y, después, seleccione **Servidores DNS**.
3. El menú Servidores DNS debe aparecer en el lado derecho de la pantalla. En ese menú, seleccione **Personalizado**.
4. Asegúrese de que los servidores DNS que aparecen en Personalizado coinciden con el controlador de dominio o el dominio de Active Directory. Si no ve el servidor DNS, escriba su valor en el campo **Agregar servidor DNS** para agregarlo.

### <a name="error-your-deployment-failedunauthorized"></a>Error: Error al realizar la implementación...\Unauthorized

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Unauthorized","message":"{\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Target\": null,\r\n \"Details\": [\r\n {\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n },\r\n {\r\n \"Code\": \"Unauthorized\"\r\n },\r\n {\r\n \"ErrorEntity\": {\r\n \"ExtendedCode\": \"52020\",\r\n \"MessageTemplate\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Parameters\": [\r\n \"default\"\r\n ],\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n }\r\n }\r\n ],\r\n \"Innererror\": null\r\n}"}]}
```

**Causa:** La suscripción que está usando es de un tipo que no puede acceder a las características necesarias en la región donde el cliente está intentando realizar la implementación. Por ejemplo, las suscripciones a MSDN, Free o Education pueden mostrar este error.

**Solución:** Cambie el tipo de suscripción o la región a uno que pueda acceder a las características necesarias.

### <a name="error-vmextensionprovisioningerror"></a>Error: VMExtensionProvisioningError

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de error de implementación con error de estado de aprovisionamiento terminal.](media/failure-vmextensionprovisioning.png)

**Causa 1:** Error transitorio del entorno de Azure Virtual Desktop.

**Causa 2:** Error transitorio de conexión.

**Corrección:** Inicie sesión con PowerShell para confirmar que el entorno de Azure Virtual Desktop es correcto. Finalice manualmente el registro de la máquina virtual en [Creación de un grupo de host con PowerShell](create-host-pools-powershell.md).

### <a name="error-the-admin-username-specified-isnt-allowed"></a>Error: El nombre de usuario de administrador especificado no está permitido

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de error de implementación en que un administrador especificado no está permitido.](media/failure-username.png)

Ejemplo de error no procesado:

```Error
 { …{ "provisioningOperation":
 "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T20:53:18.904917Z", "duration": "PT3.0574505S", "trackingId":
 "1f460af8-34dd-4c03-9359-9ab249a1a005", "statusCode": "BadRequest", "statusMessage": { "error": { "code": "InvalidParameter", "message":
 "The Admin Username specified is not allowed.", "target": "adminUsername" } … }
```

**Causa:** La contraseña proporcionada contiene subcadenas prohibidas (admin, administrador, raíz).

**Solución:** Actualice el nombre de usuario o utilice distintos usuarios.

### <a name="error-vm-has-reported-a-failure-when-processing-extension"></a>Error: La máquina virtual ha indicado un error al procesar la extensión

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de la operación de recursos completa con un estado de aprovisionamiento terminal de Error de implementación.](media/failure-processing.png)

Ejemplo de error no procesado:

```Error
{ … "code": "ResourceDeploymentFailure", "message":
 "The resource operation completed with terminal provisioning state 'Failed'.", "details": [ { "code":
 "VMExtensionProvisioningError", "message": "VM has reported a failure when processing extension 'dscextension'.
 Error message: \"DSC Configuration 'SessionHost' completed with error(s). Following are the first few:
 PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource functionality with error message:
 One or more errors occurred. The SendConfigurationApply function did not succeed.\"." } ] … }
```

**Causa:** La extensión DSC de PowerShell no ha podido obtener acceso de administrador a la máquina virtual.

**Solución:** Confirme que el nombre de usuario y la contraseña tienen acceso administrativo a la máquina virtual y vuelva a ejecutar la plantilla de Azure Resource Manager.

### <a name="error-deploymentfailed--powershell-dsc-configuration-firstsessionhost-completed-with-errors"></a>Error: DeploymentFailed: la configuración de DSC de PowerShell "FirstSessionHost" se ha completado con errores

> [!div class="mx-imgBorder"]
> ![Captura de pantalla de error de implementación con la configuración de DSC de PowerShell "FirstSessionHost" completada con errores.](media/failure-dsc.png)

Ejemplo de error no procesado:

```Error
{
    "code": "DeploymentFailed",
   "message": "At least one resource deployment operation failed. Please list
 deployment operations for details. 4 Please see https://aka.ms/arm-debug for usage details.",
 "details": [
         { "code": "Conflict",
         "message": "{\r\n \"status\": \"Failed\",\r\n \"error\": {\r\n \"code\":
         \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource
         operation completed with terminal provisioning state 'Failed'.\",\r\n
         \"details\": [\r\n {\r\n \"code\":
        \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has
              reported a failure when processing extension 'dscextension'.
              Error message: \\\"DSC Configuration 'FirstSessionHost'
              completed with error(s). Following are the first few:
              PowerShell DSC resource MSFT ScriptResource failed to
              execute Set-TargetResource functionality with error message:
              One or more errors occurred. The SendConfigurationApply
              function did not succeed.\\\".\"\r\n }\r\n ]\r\n }\r\n}"  }

```

**Causa:** La extensión DSC de PowerShell no ha podido obtener acceso de administrador a la máquina virtual.

**Solución:** Confirme que el nombre de usuario y la contraseña proporcionados tienen acceso administrativo a la máquina virtual y vuelva a ejecutar la plantilla de Azure Resource Manager.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Error: DeploymentFailed: InvalidResourceReference

Ejemplo de error no procesado:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/networkInterfaces/erd. Please make sure that
the referenced resource exists, and that both resources are in the same
region.\\\",\\r\\n\\\"details\\\": []\\r\\n }\\r\\n}\"\r\n }\r\n ]\r\n }\r\n ]\r\n }\r\n}"}]}
```

**Causa:** Parte del nombre del grupo de recursos se usa para determinados recursos que se crean mediante la plantilla. Debido a los recursos existentes con coincidencia de nombres, la plantilla puede seleccionar un recurso existente de un grupo diferente.

**Solución:** Al ejecutar la plantilla de Azure Resource Manager para implementar las máquinas virtuales del host de sesión, asegúrese de los dos primeros caracteres sean únicos para el nombre del grupo de recursos de suscripción.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Error: DeploymentFailed: InvalidResourceReference

Ejemplo de error no procesado:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/DEMO/providers/Microsoft.Network/networkInterfaces
/EXAMPLE was not found. Please make sure that the referenced resource exists, and that both
resources are in the same region.\\\",\\r\\n \\\"details\\\": []\\r\\n }\\r\\n}\"\r\n
}\r\n ]\r\n }\r\n ]\r\n }\r\n\
```

**Causa:** Este error se debe a que la NIC creada con la plantilla de Azure Resource Manager tiene el mismo nombre que otra NIC de la red virtual.

**Solución:** Utilice un prefijo de host diferente.

### <a name="error-deploymentfailed--error-downloading"></a>Error: DeploymentFailed: error al descargar

Ejemplo de error no procesado:

```Error
\\\"The DSC Extension failed to execute: Error downloading
https://catalogartifact.azureedge.net/publicartifacts/rds.wvd-provision-host-pool-
2dec7a4d-006c-4cc0-965a-02bbe438d6ff-prod
/Artifacts/DSC/Configuration.zip after 29 attempts: The remote name could not be
resolved: 'catalogartifact.azureedge.net'.\\nMore information about the failure can
be found in the logs located under
'C:\\\\WindowsAzure\\\\Logs\\\\Plugins\\\\Microsoft.Powershell.DSC\\\\2.77.0.0' on
the VM.\\\"
```

**Causa:** Este error se debe a que una ruta estática, una regla de firewall o un NSG bloquean la descarga del archivo zip vinculado a la plantilla de Azure Resource Manager.

**Solución:** Suprima la ruta estática de bloqueo, la regla de firewall o el NSG. De manera opcional, abra el archivo json de plantilla de Azure Resource Manager en un editor de texto, comprima el vínculo en un archivo zip y descargue el recurso en una ubicación permitida.

### <a name="error-cant-delete-a-session-host-from-the-host-pool-after-deleting-the-vm"></a>Error: No se puede eliminar un host de sesión del grupo de hosts después de eliminar la máquina virtual

**Causa:** Debe eliminar el host de sesión antes de eliminar la máquina virtual.

**Solución:** Ponga el host de sesión en modo de purga, cierre la sesión de todos los usuarios del host de sesión y elimine el host.

## <a name="next-steps"></a>Pasos siguientes

- Para información general sobre la solución de problemas de Azure Virtual Desktop y las pistas de escalación, consulte [Introducción a la solución de problemas, comentarios y soporte técnico](troubleshoot-set-up-overview.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en Azure Virtual Desktop, consulte [Configuración de la máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas relacionados con el agente de Azure Virtual Desktop o la conectividad de sesión, consulte [Solución de problemas comunes del agente de Azure Virtual Desktop](troubleshoot-agent.md).
- Para solucionar problemas con conexiones de cliente de Azure Virtual Desktop, consulte [Conexiones de servicios de Azure Virtual Desktop](troubleshoot-service-connection.md).
- Para solucionar problemas con los clientes de Escritorio remoto, consulte [Solución de problemas del cliente de Escritorio remoto](troubleshoot-client.md).
- Para solucionar problemas al usar PowerShell con Azure Virtual Desktop, consulte [PowerShell para Azure Virtual Desktop](troubleshoot-powershell.md).
- Para más información sobre el servicio, consulte [Entorno de Azure Virtual Desktop](environment-setup.md).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de las implementaciones de plantillas de Resource Manager](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
- Para más información sobre las acciones de auditoría, consulte [Operaciones de auditoría con Resource Manager](../azure-monitor/essentials/activity-log.md).
- Si desea conocer más detalles sobre las acciones que permiten determinar los errores durante la implementación, consulte [Visualización de operaciones de implementación con el Portal de Azure](../azure-resource-manager/templates/deployment-history.md).
