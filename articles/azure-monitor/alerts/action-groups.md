---
title: Creación y administración de grupos de acciones en Azure Portal
description: Obtenga información acerca de cómo crear y administrar grupos de acciones en Azure Portal.
author: dkamstra
ms.topic: conceptual
ms.date: 05/28/2021
ms.author: dukek
ms.openlocfilehash: cc5d5aa589b56fb6e6fda1845e50606ff492fbdd
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129217890"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Creación y administración de grupos de acciones en Azure Portal
Un grupo de acciones es una colección de las preferencias de notificación que el propietario de una suscripción de Azure define. Las alertas de Azure Monitor, Service Health y Azure Advisor usan grupos de acciones para notificar a los usuarios que se ha desencadenado una alerta. Varias alertas pueden usar el mismo grupo de acciones o distintos grupos de acciones en función de los requisitos del usuario. 

En este artículo se muestra cómo crear y administrar grupos de acciones en el portal de Azure.

Cada acción se compone de las siguientes propiedades:

* **Tipo**: La notificación o acción realizada. El envío de llamadas de voz, mensajes de texto o correo electrónico o el desencadenamiento de varios tipos de acciones automatizadas son algunos ejemplos. Consulte los tipos más adelante en este artículo.
* **Name**: un identificador único dentro del grupo de acciones.
* **Detalles**: detalles correspondientes que varían según el *tipo*.

Para más información sobre el uso de plantillas de Azure Resource Manager para configurar grupos de acciones, consulte [Plantillas de Resource Manager para grupos de acciones](./action-groups-create-resource-manager-template.md).

El grupo de acciones es un servicio **Globa**, por lo que no hay ninguna dependencia de una región específica de Azure. Las solicitudes del cliente se pueden procesar mediante el servicio del grupo de acciones en cualquier región, lo que significa que, si una región del servicio no funciona, el tráfico se enruta y procesa automáticamente por otras regiones. Que sea un *servicio global* ayuda al cliente a no preocuparse por la **recuperación ante desastres**. 

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Creación de un grupo de acciones con Azure Portal

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **Monitor**. El panel **Monitor** consolida todas las opciones de configuración y todos los datos de supervisión en una vista.

1. Seleccione **Alertas** y **Administrar acciones**.

    ![Botón Administrar acciones](./media/action-groups/manage-action-groups.png)
    
1. Seleccione **Agregar grupo de acciones** y rellene los campos correspondientes en la experiencia del asistente.

    ![Comando "Agregar grupo de acciones"](./media/action-groups/add-action-group.PNG)

### <a name="configure-basic-action-group-settings"></a>Configuración básica del grupo de acciones

En **Detalles del proyecto**:

Seleccione la **Suscripción** y el **Grupo de recursos** donde está guardado el grupo de acciones.

En **Detalles de instancia**:

1. Escriba el **Nombre del grupo de acciones**.

1. Escriba un **Nombre para mostrar**. El nombre para mostrar se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.

      ![Cuadro de diálogo "Agregar grupo de acciones"](./media/action-groups/action-group-1-basics.png)


### <a name="configure-notifications"></a>Configuración de notificaciones

1. Haga clic en el botón **Siguiente: Notificaciones >** para ir a la pestaña **Notificaciones**, o bien seleccione la pestaña **Notificaciones** en la parte superior de la pantalla.

1. Defina una lista de notificaciones que se enviarán cuando se desencadene una alerta. Proporcione lo siguiente para cada notificación:

    a. **Tipo de notificación**: seleccione el tipo de notificación que desea enviar. Las opciones disponibles son:
      * Rol de Azure Resource Manager de correo electrónico: envíe un correo electrónico a los usuarios asignados a determinados roles de ARM en el nivel de suscripción.
      * Correo electrónico/SMS/Inserción/Voz: envíe estos tipos de notificación a destinatarios específicos.
    
    b. **Name**: especifique un nombre único para la notificación.

    c. **Detalles**: en función del tipo de notificación seleccionado, escriba una dirección de correo electrónico, un número de teléfono, etc.
    
    d. **Esquema de alerta común**: puede optar por habilitar el [esquema de alerta común](./alerts-common-schema.md), que proporciona la ventaja de tener una sola carga de alertas, extensible y unificada, para todos los servicios de alerta de Azure Monitor.

    ![Pestaña Notificaciones](./media/action-groups/action-group-2-notifications.png)
    
### <a name="configure-actions"></a>Configuración de acciones

1. Haga clic en el botón **Siguiente: Acciones >** para ir a la pestaña **Acciones**, o bien seleccione la pestaña **Acciones** en la parte superior de la pantalla.

1. Defina una lista de acciones que se ejecutarán cuando se desencadene una alerta. Proporcione lo siguiente para cada acción:

    a. **Tipo de acción**: seleccione Runbook de Automation, Función de Azure, ITSM, Aplicación lógica, Webhook seguro y webhook.
    
    b. **Name**: escriba un nombre único para la acción.

    c. **Detalles**: según el tipo de acción, proporcione un identificador URI de webhook, una aplicación de Azure, una conexión de ITSM o un runbook de Automation. Para la acción de ITSM, especifique además **Elemento de trabajo** y otros campos que requiera la herramienta ITSM.
    
    d. **Esquema de alerta común**: puede optar por habilitar el [esquema de alerta común](./alerts-common-schema.md), que proporciona la ventaja de tener una sola carga de alertas, extensible y unificada, para todos los servicios de alerta de Azure Monitor.
    
    ![Pestaña Acciones](./media/action-groups/action-group-3-actions.png)

### <a name="create-the-action-group"></a>Creación del grupo de acciones

1. Puede explorar la configuración de **Etiquetas** si lo desea. Esto le permite asociar pares clave-valor al grupo de acciones para su categorización, y es una característica disponible para cualquier recurso de Azure.

    ![Pestaña Etiquetas](./media/action-groups/action-group-4-tags.png)
    
1. Haga clic en **Revisar y crear** para revisar la configuración. Se realizará una validación rápida de las entradas para asegurarse de que se seleccionan todos los campos obligatorios. Si hay algún problema, se le indicará aquí. Una vez revisada la configuración, haga clic en **Crear** para aprovisionar el grupo de acciones.
    
    ![Pestaña Revisar y crear](./media/action-groups/action-group-5-review.png)

> [!NOTE]
> Al configurar una acción para notificar a una persona por correo electrónico o SMS, la persona recibe una confirmación que le indica que se le ha agregado al grupo de acciones.

## <a name="manage-your-action-groups"></a>Administración de los grupos de acciones

Después de crear un grupo de acciones, puede ver los **grupos de acciones** seleccionando **Administrar acciones** en la página de aterrizaje **Alertas** del panel **Supervisión**. Seleccione el grupo de acciones que desea administrar para:

* Agregar, editar o quitar acciones.
* Eliminar el grupo de acciones.

## <a name="action-specific-information"></a>Información específica de la acción

> [!NOTE]
> Consulte los [límites de servicio de suscripción para la supervisión](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-monitor-limits) para los límites numéricos de cada uno de los siguientes elementos.  

### <a name="automation-runbook"></a>Runbook de automatización
Consulte los [límites de servicio de suscripción de Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md) para los límites relacionados con las cargas de runbook.

En un grupo de acciones puede tener un número limitado de acciones de runbook. 

### <a name="azure-app-push-notifications"></a>Notificaciones push de aplicaciones de Azure
Habilite las notificaciones push en [Azure Mobile App](https://azure.microsoft.com/features/azure-portal/mobile-app/); para ello, proporcione la dirección de correo electrónico que usa como identificador de cuenta al configurar Azure Mobile App.

En un grupo de acciones puede tener un número limitado de acciones de aplicación de Azure.

### <a name="email"></a>Email
Se enviarán mensajes de correo electrónico desde las direcciones de correo electrónico siguientes. Asegúrese de que el filtrado de correo electrónico esté configurado correctamente.
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

En un grupo de acciones puede tener un número limitado de acciones de correo electrónico. Consulte el artículo de [información sobre las limitaciones](./alerts-rate-limiting.md).

### <a name="email-azure-resource-manager-role"></a>Rol de Azure Resource Manager de correo electrónico
Envíe un correo electrónico a los miembros del rol de la suscripción. El correo electrónico solo se enviará a los miembros **usuarios de Azure AD** del rol. No se enviará correo electrónico a grupos de Azure AD ni entidades de servicio.

Solo se envía un correo electrónico de notificación a la dirección de *correo electrónico principal*.

Si no recibe notificaciones en la *dirección correo electrónico principal*, intente seguir estos pasos:

1. En Azure Portal, vaya a *Active Directory*.
2. Haga clic en Todos los usuarios (en el panel de la izquierda); verá una lista de usuarios (en el panel de la derecha).
3. Seleccione el usuario para el que quiera revisar la información de la *dirección de correo electrónico principal*.

  :::image type="content" source="media/action-groups/active-directory-user-profile.png" alt-text="Ejemplo de cómo revisar el perfil de usuario." border="true":::

4. En Perfil de usuario, en Información de contacto, si la pestaña "Dirección de correo electrónico" está en blanco, haga clic en el botón *Editar* de la parte superior, agregue la *dirección de correo electrónico principal* y presione el botón *Guardar* de la parte superior.

  :::image type="content" source="media/action-groups/active-directory-add-primary-email.png" alt-text="Ejemplo de cómo agregar la dirección de correo electrónico principal." border="true":::

En un grupo de acciones puede tener un número limitado de acciones de correo electrónico. Consulte el artículo de [información sobre las limitaciones](./alerts-rate-limiting.md).

Al configurar el *Rol de ARM de correo electrónico*, debe asegurarse de que se cumplen las siguientes tres condiciones:

1. El tipo de entidad que se asignará al rol debe ser **"User"** .
2. La asignación debe realizarse en el nivel de **suscripción**.
3. El usuario debe tener un correo electrónico configurado en su **perfil de AAD**. 

> [!NOTE]
> El cliente puede tardar hasta **24 horas** en empezar a recibir notificaciones después de agregar un nuevo rol de ARM a su suscripción.

### <a name="function"></a>Función
Llama a un punto de conexión del desencadenador HTTP existente en [Azure Functions](../../azure-functions/functions-get-started.md). Para controlar una solicitud, el punto de conexión debe controlar el verbo HTTP POST.

Al definir la acción de la función, el punto de conexión httptrigger y la clave de acceso de la función se guardan en la definición de la acción. Por ejemplo: `https://azfunctionurl.azurewebsites.net/api/httptrigger?code=this_is_access_key`. Si cambia la clave de acceso de la función, deberá quitar y volver a crear la acción de la función del grupo de acciones.

En un grupo de acciones puede tener un número limitado de acciones de función.

### <a name="itsm"></a>ITSM
La acción de ITSM requiere una conexión de ITSM. Aprenda cómo crear una [conexión de ITSM](./itsmc-overview.md).

En un grupo de acciones puede tener un número limitado de acciones de ITSM. 

### <a name="logic-app"></a>Aplicación lógica
En un grupo de acciones puede tener un número limitado de acciones de aplicación lógica.

### <a name="secure-webhook"></a>Webhook seguro
Con la acción de webhook seguro de grupos de acciones, puede aprovechar Azure Active Directory para proteger la conexión entre su grupo de acciones y su API web protegida (punto de conexión de webhook). A continuación se describe el flujo de trabajo general para aprovechar esta funcionalidad. Para una introducción a las entidades de servicio y aplicaciones de Azure AD, consulte [Introducción a la Plataforma de identidad de Microsoft (versión 2.0)](../../active-directory/develop/v2-overview.md).

> [!NOTE]
> El uso de la acción de webhook requiere que el punto de conexión del webhook de destino no necesite detalles de la alerta para funcionar correctamente, o que sea capaz de analizar la información de contexto de la alerta que se proporciona como parte de la operación POST. Si el punto de conexión de webhook no puede controlar la información de contexto de la alerta por sí mismo, puede usar una solución como una [acción de aplicación lógica](./action-groups-logic-app.md) para manipular de manera personalizada la información de contexto de la alerta con el fin de que coincida con el formato de datos esperado del webhook.

1. Cree una aplicación Azure AD para la API web. Consulte [API web protegida: registro de aplicación](../../active-directory/develop/scenario-protected-web-api-app-registration.md).
    - Configure la API protegida para que [la llame una aplicación de demonio](../../active-directory/develop/scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app).
    
2. Habilite Grupos de acciones para usar la aplicación de Azure AD.

    > [!NOTE]
    > Debe ser miembro del [rol Administrador de aplicaciones de Azure AD](../../active-directory/roles/permissions-reference.md#all-roles) para ejecutar este script.
    
    - Modifique la llamada Connect-AzureAD del script de PowerShell para usar el identificador de inquilino de Azure AD.
    - Modifique la variable del script de PowerShell $myAzureADApplicationObjectId para usar el identificador de objeto de la aplicación de Azure AD.
    - Ejecute el script modificado.
    
3. Configure la acción de webhook seguro del grupo de acciones.
    - Copie el valor $myApp.ObjectId del script e introdúzcalo en el campo Id. de objeto de aplicación en la definición de acción de webhook.
    
    ![Acción de webhook seguro](./media/action-groups/action-groups-secure-webhook.png)

#### <a name="secure-webhook-powershell-script"></a>Script de PowerShell de webhook seguro

```PowerShell
Connect-AzureAD -TenantId "<provide your Azure AD tenant ID here>"
    
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object ID of your Azure AD Application>"
    
# This is the Action Groups Azure AD AppId
$actionGroupsAppId = "461e8683-5575-4561-ac7f-899cc907d62a"
    
# This is the name of the new role we will add to your Azure AD Application
$actionGroupRoleName = "ActionGroupsSecureWebhook"
    
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
    
# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles
$actionGroupsSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $actionGroupsAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match "ActionGroupsSecureWebhook")
{
    Write-Host "The Action Groups role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $actionGroupRoleName -Description "This is a role for Action Groups to join"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Create the service principal if it doesn't exist
if ($actionGroupsSP -match "AzNS AAD Webhook")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the Action Groups Azure AD Application and add it to the role
    $actionGroupsSP = New-AzureADServicePrincipal -AppId $actionGroupsAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $actionGroupsSP.ObjectId -PrincipalId $actionGroupsSP.ObjectId
    
Write-Host "My Azure AD Application (ObjectId): " + $myApp.ObjectId
Write-Host "My Azure AD Application's Roles"
Write-Host $myApp.AppRoles
```

### <a name="sms"></a>sms
Consulte también la [información sobre las limitaciones](./alerts-rate-limiting.md) y el [comportamiento de las alertas por SMS](./alerts-sms-behavior.md) para información adicional. 

En un grupo de acciones puede tener un número limitado de acciones de SMS.

> [!NOTE]
> Si la interfaz de usuario del grupo de acciones de Azure Portal no permite seleccionar el código de país o región, no se admite SMS para el país o región.  Si el código de país o región no está disponible, puede votar en [UserVoice](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice) para que el país o región se agregue. Mientras tanto, como solución alternativa, haga que el grupo de acciones llame a un webhook de un proveedor de SMS de terceros con soporte en el país o región.  

Los precios de los países o regiones admitidos se muestran en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

**Lista de países en los que se admite la notificación por SMS**

| Código de país | Nombre de país |
|:---|:---|
| 61 | Australia |
| 43 | Austria |
| 32 | Bélgica |
| 55 | Brasil |
| 1 |Canadá |
| 56 | Chile |
| 86 | China |
| 420 | República Checa |
| 45 | Dinamarca |
| 372 | Estonia |
| 358 | Finlandia |
| 33 | Francia |
| 49 | Alemania |
| 852 | RAE de Hong Kong |
| 91 | India |
| 353 | Irlanda |
| 972 | Israel |
| 39 | Italia |
| 81 | Japón |
| 352 | Luxemburgo |
| 60 | Malasia |
| 52 | México |
| 31 | Países Bajos |
| 64 | Nueva Zelanda |
| 47 | Noruega |
| 351 | Portugal |
| 1 | Puerto Rico |
| 40 | Rumanía |
| 7  | Rusia  |
| 65 | Singapur |
| 27 | Sudáfrica |
| 82 | Corea del Sur |
| 34 | España |
| 41 | Suiza |
| 886 | Taiwán |
| 971 | Emiratos Árabes Unidos    |
| 44 | Reino Unido |
| 1 | Estados Unidos |

### <a name="voice"></a>Voz
Consulte el artículo [Información sobre las limitaciones](./alerts-rate-limiting.md) para comportamientos importantes adicionales.

En un grupo de acciones puede tener un número limitado de acciones de voz.

> [!NOTE]
> Si la interfaz de usuario del grupo de acciones de Azure Portal no permite seleccionar el código de país o región, no se admiten llamadas de voz en el país o región. Si el código de país o región no está disponible, puede votar en [UserVoice](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice) para que el país o región se agregue.  Mientras tanto, como solución alternativa, haga que el grupo de acciones llame a un webhook de un proveedor de llamadas de voz de terceros con soporte en el país o región.  
> El único código de país que se admite actualmente en el grupo de acciones de Azure Portal para las notificaciones de voz es +1 (Estados Unidos). 

Los precios de los países o regiones admitidos se muestran en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

### <a name="webhook"></a>webhook

> [!NOTE]
> El uso de la acción de webhook requiere que el punto de conexión del webhook de destino no necesite detalles de la alerta para funcionar correctamente, o que sea capaz de analizar la información de contexto de la alerta que se proporciona como parte de la operación POST. Si el punto de conexión de webhook no puede controlar la información de contexto de la alerta por sí mismo, puede usar una solución como una [acción de aplicación lógica](./action-groups-logic-app.md) para manipular de manera personalizada la información de contexto de la alerta con el fin de que coincida con el formato de datos esperado del webhook.

Los webhooks se procesan utilizando las siguientes reglas:
- Una llamada de webhook se intenta un máximo de 3 veces.
- Se volverá a intentar la llamada si no se recibe una respuesta dentro del período de tiempo de espera o se devuelve uno de los siguientes códigos de estado HTTP: 408, 429, 503 o 504.
- La primera llamada esperará diez segundos para obtener una respuesta.
- El segundo y el tercer intento esperarán treinta segundos para obtener una respuesta.
- Después de que los tres intentos de llamada al webhook no se hayan realizado, ningún grupo de acciones llamará al punto de conexión durante quince minutos.

Consulte las [direcciones IP del grupo de acciones](../app/ip-addresses.md) para conocer los intervalos de direcciones IP de origen.


## <a name="next-steps"></a>Pasos siguientes
* Más información sobre el [comportamiento de las alertas por SMS](./alerts-sms-behavior.md).  
* [Comprender el esquema de webhook de alertas del registro de actividad](./activity-log-alerts-webhook.md).  
* Obtenga más información sobre el [conector de ITSM](./itsmc-overview.md).
* Más información sobre la [limitación de velocidad](./alerts-rate-limiting.md) en las alertas.
* Consulte la [introducción a las alertas del registro de actividad](./alerts-overview.md) y aprenda cómo puede recibir alertas.  
* Aprenda a [configurar alertas siempre que se publique una notificación de mantenimiento de un servicio](../../service-health/alerts-activity-log-service-notifications-portal.md).
