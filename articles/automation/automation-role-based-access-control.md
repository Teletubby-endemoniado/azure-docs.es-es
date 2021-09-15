---
title: Administración de seguridad y permisos de roles en Azure Automation
description: En este artículo se explica cómo usar el control de acceso basado en rol de Azure (RBAC de Azure), que permite la administración del acceso a los recursos de Azure.
keywords: automatización de rbac, control de acceso basado en roles, rbac de azure
services: automation
ms.subservice: shared-capabilities
ms.date: 08/26/2021
ms.topic: conceptual
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 30bc4a306eecf8be3177fb045f9904d775cab9bd
ms.sourcegitcommit: f53f0b98031cd936b2cd509e2322b9ee1acba5d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2021
ms.locfileid: "123215012"
---
# <a name="manage-role-permissions-and-security"></a>Administración de seguridad y permisos de roles

El control de acceso basado en rol de Azure (RBAC de Azure) permite la administración del acceso a los recursos de Azure. Con [RBAC de Azure](../role-based-access-control/overview.md) se pueden separar las tareas dentro del equipo y conceder a los usuarios, los grupos y las aplicaciones únicamente el nivel de acceso que necesitan para realizar su trabajo. El acceso basado en rol se puede conceder a los usuarios mediante Azure Portal, las herramientas de la línea de comandos de Azure o las API de administración de Azure.

## <a name="roles-in-automation-accounts"></a>Roles en cuentas de Automation

En Azure Automation, el acceso se concede mediante la asignación del rol de Azure adecuado a los usuarios, grupos y aplicaciones en el ámbito de las cuentas de Automation. Estos son los roles integrados compatibles que admiten las cuentas de Automation:

| **Rol** | **Descripción** |
|:--- |:--- |
| Propietario |El rol Propietario permite el acceso a todos los recursos y las acciones de una cuenta de Automation, lo que incluye proporcionar acceso a otros usuarios, grupos y aplicaciones para que administren la cuenta de Automation. |
| Colaborador |El rol Colaborador permite administrar todo, excepto la modificación de los permisos de acceso de otros usuarios a una cuenta de Automation. |
| Lector |El rol Lector permite ver todos los recursos de una cuenta de Automation, pero no realizar cambios. |
| Colaborador de Automation | El rol Colaborador de Automation permite administrar todos los recursos de la cuenta de Automation, excepto modificar los permisos de acceso de otros usuarios a una cuenta de Automation. |
| Operador de Automation |El rol de función de Operador de Automation permite ver las propiedades y el nombre del runbook, y crear y administrar trabajos para todos los runbooks de una cuenta de Automation. Este rol es útil si se desea proteger los recursos de la cuenta de Automation, como los recursos de credenciales y los runbooks, para que no puedan verse ni modificarse, pero permitir a los miembros de la organización ejecutar los runbooks. |
|Operador de trabajos de Automation|El rol de Operador de trabajos de Automation permite crear y administrar trabajos para todos los runbooks de una cuenta de Automation.|
|Operador de runbooks de Automation|El rol de Operador de runbooks de Automation permite leer el nombre y las propiedades de los runbooks.|
| Colaborador de Log Analytics | El rol de colaborador de Log Analytics permite leer todos los datos de supervisión y editar la configuración de supervisión. La edición de la configuración de supervisión incluye la posibilidad de agregar la extensión de máquina virtual a las máquinas virtuales, leer las claves de las cuentas de almacenamiento para poder configurar la recopilación de registros de Azure Storage, crear y configurar cuentas de Automation, agregar características de Azure Automation y configurar Azure Diagnostics en todos los recursos de Azure.|
| Lector de Log Analytics | El rol de lector de Log Analytics permite ver y buscar todos los datos de supervisión, así como ver la configuración de supervisión. Esto incluye la visualización de la configuración de Azure Diagnostics en todos los recursos de Azure. |
| Colaborador de supervisión | El rol de colaborador de supervisión permite leer todos los datos de supervisión y actualizar la configuración de supervisión.|
| Lector de supervisión | El rol de lector de supervisión permite leer todos los datos de supervisión. |
| Administrador de acceso de usuario |El rol Administrador de acceso de usuario permite administrar el acceso de los usuarios a las cuentas de Azure Automation. |

## <a name="role-permissions"></a>Permisos de los roles

Las tablas siguientes describen los permisos específicos concedidos a cada rol. En ellas, se incluyen elementos correspondientes a las categorías Actions (para conceder permisos) y NotActions (para restringirlos).

### <a name="owner"></a>Propietario

Un propietario puede administrar todo, incluido el acceso. La siguiente tabla muestra los permisos concedidos para el rol:

|Acciones|Descripción|
|---|---|
|Microsoft.Automation/automationAccounts/|Crear y administrar recursos de todos los tipos.|

### <a name="contributor"></a>Colaborador

Un colaborador puede administrar todo, excepto el acceso. La siguiente tabla muestra los permisos concedidos y denegados para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Crear y administrar recursos de todos los tipos|
|**No acciones**||
|Microsoft.Authorization/*/Delete| Eliminar roles y asignaciones de roles.       |
|Microsoft.Authorization/*/Write     |  Crear roles y asignaciones de roles.       |
|Microsoft.Authorization/elevateAccess/Action    | Deniega la capacidad de crear un administrador de acceso de usuario.       |

### <a name="reader"></a>Lector

Un lector puede ver todos los recursos de una cuenta de Automation, pero no realizar cambios.

|**Acciones**  |**Descripción**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Ver todos los recursos en una cuenta de Automation. |

### <a name="automation-contributor"></a>Colaborador de Automation

Un Colaborador de Automation puede administrar todos los recursos de la cuenta de Automation, excepto el acceso. La siguiente tabla muestra los permisos concedidos para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/*|Crear y administrar recursos de todos los tipos en la cuenta de Automation.|
|Microsoft.Authorization/*/read|Roles de lectura y asignaciones de roles.|
|Microsoft.Resources/deployments/*|Crear y administrar implementaciones de grupos de recursos.|
|Microsoft.Resources/subscriptions/resourceGroups/read|Leer implementaciones de grupos de recursos.|
|Microsoft.Support/*|Crear y administrar incidencias de soporte técnico.|

> [!NOTE]
> El rol Colaborador de Automation se puede usar para acceder a cualquier recurso mediante la identidad administrada, si se establecen los permisos adecuados en el recurso de destino, o mediante una cuenta de ejecución. De manera predeterminada, está configurada una cuenta de ejecución de Automation con derechos de colaborador en la suscripción. Siga el principio de privilegios mínimos y asigne cuidadosamente solo los permisos necesarios para ejecutar el runbook. Por ejemplo, si la cuenta de Automation solo es necesaria para iniciar o detener una VM de Azure, los permisos asignados a la cuenta de ejecución o a la identidad administrada solo deben ser para iniciar o detener la VM. De manera similar, si un runbook lee del almacenamiento de blobs, asigne permisos de solo lectura.
> 
> Al asignar permisos, se recomienda usar el control de acceso basado en rol (RBAC) de Azure asignado a una identidad administrada. Revise nuestras recomendaciones sobre el [mejor enfoque](../active-directory/managed-identities-azure-resources/managed-identity-best-practice-recommendations.md) para usar una identidad administrada asignada por el sistema o por el usuario, incluida la administración y la gobernanza durante su vigencia.

### <a name="automation-operator"></a>Operador de Automation

El rol de Operador de Automation permite crear y administrar trabajos, y leer las propiedades y el nombre del runbook de todos los runbooks de una cuenta de Automation.

>[!NOTE]
>Si desea controlar el acceso de operador a runbooks individuales, no establezca este rol. En su lugar, use los roles **operador de trabajo de Automation** y el **operador de runbook de Automation** en combinación.

La siguiente tabla muestra los permisos concedidos para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|Microsoft.Authorization/*/read|Autorización de lectura.|
|Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read|Lee los recursos de Hybrid Runbook Worker.|
|Microsoft.Automation/automationAccounts/jobs/read|Enumerar trabajos del runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Reanudar un trabajo que está en pausa.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Cancelar un trabajo en curso.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Leer los flujos de trabajo y los resultados.|
|Microsoft.Automation/automationAccounts/jobs/output/read|Obtiene la salida de un trabajo.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Pausar un trabajo en curso.|
|Microsoft.Automation/automationAccounts/jobs/write|Crear trabajos.|
|Microsoft.Automation/automationAccounts/jobSchedules/read|Obtiene una programación de trabajos de Azure Automation.|
|Microsoft.Automation/automationAccounts/jobSchedules/write|Crea una programación de trabajos de Azure Automation.|
|Microsoft.Automation/automationAccounts/linkedWorkspace/read|Obtiene el área de trabajo vinculada a la cuenta de Automation.|
|Microsoft.Automation/automationAccounts/read|Obtiene una cuenta de Azure Automation.|
|Microsoft.Automation/automationAccounts/runbooks/read|Obtiene un runbook de Azure Automation.|
|Microsoft.Automation/automationAccounts/schedules/read|Obtiene un recurso de programación de Azure Automation.|
|Microsoft.Automation/automationAccounts/schedules/write|Crea o actualiza un recurso de programación de Azure Automation.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Roles de lectura y asignaciones de roles.         |
|Microsoft.Resources/deployments/*      |Crear y administrar implementaciones de grupos de recursos.         |
|Microsoft.Insights/alertRules/*      | Crear y administrar reglas de alertas.        |
|Microsoft.Support/* |Crear y administrar incidencias de soporte técnico.|

### <a name="automation-job-operator"></a>Operador de trabajos de Automation

El rol de Operador de trabajos de Automation se asigna en el ámbito de la cuenta de Automation. Esto permite a los roles con permiso de Operador crear y administrar trabajos para todos los runbooks de la cuenta. Si a la función de operador de trabajo se le conceden permisos de lectura en el grupo de recursos que contiene la cuenta de Automation, los miembros del rol tendrán la capacidad de iniciar runbooks. Pero no tienen la capacidad de crearlos, modificarlos ni eliminarlos.

La siguiente tabla muestra los permisos concedidos para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|Microsoft.Authorization/*/read|Autorización de lectura.|
|Microsoft.Automation/automationAccounts/jobs/read|Enumerar trabajos del runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Reanudar un trabajo que está en pausa.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Cancelar un trabajo en curso.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Leer los flujos de trabajo y los resultados.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Pausar un trabajo en curso.|
|Microsoft.Automation/automationAccounts/jobs/write|Crear trabajos.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Roles de lectura y asignaciones de roles.       |
|Microsoft.Resources/deployments/*      |Crear y administrar implementaciones de grupos de recursos.         |
|Microsoft.Insights/alertRules/*      | Crear y administrar reglas de alertas.        |
|Microsoft.Support/* |Crear y administrar incidencias de soporte técnico.|

### <a name="automation-runbook-operator"></a>Operador de runbooks de Automation

El rol de operador de runbooks de Automation se concede en el ámbito del runbook. Un Operador de runbooks de Automation puede ver las propiedades y el nombre del runbook. Este rol, combinado con el de **operador de trabajos de Automation**, permite también al operador crear y administrar trabajos para el runbook. La siguiente tabla muestra los permisos concedidos para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | Enumerar los runbooks.        |
|Microsoft.Authorization/*/read      | Autorización de lectura.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Roles de lectura y asignaciones de roles.         |
|Microsoft.Resources/deployments/*      | Crear y administrar implementaciones de grupos de recursos.         |
|Microsoft.Insights/alertRules/*      | Crear y administrar reglas de alertas.        |
|Microsoft.Support/*      | Crear y administrar incidencias de soporte técnico.        |

### <a name="log-analytics-contributor"></a>Colaborador de Log Analytics

Un colaborador de Log Analytics puede leer todos los datos de supervisión y editar la configuración de supervisión. La edición de la configuración de supervisión incluye la posibilidad de agregar la extensión de máquina virtual a las máquinas virtuales, leer las claves de las cuentas de almacenamiento para poder configurar la recopilación de registros de Azure Storage, crear y configurar cuentas de Automation, agregar características y configurar Azure Diagnostics en todos los recursos de Azure. La siguiente tabla muestra los permisos concedidos para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|*/read|Leer recursos de todos los tipos, excepto secretos.|
|Microsoft.Automation/automationAccounts/*|Administrar cuentas de Automation.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Crear y administrar extensiones de máquinas virtuales.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Enumerar claves de cuentas de almacenamiento clásicas.|
|Microsoft.Compute/virtualMachines/extensions/*|Crear y administrar extensiones de máquinas virtuales clásicas.|
|Microsoft.Insights/alertRules/*|Reglas de alerta de lectura, escritura y eliminación.|
|Microsoft.Insights/diagnosticSettings/*|Configuración de diagnóstico de lectura, escritura y eliminación.|
|Microsoft.OperationalInsights/*|Administrar registros de Azure Monitor.|
|Microsoft.OperationsManagement/*|Administración de características de Azure Automation en áreas de trabajo.|
|Microsoft.Resources/deployments/*|Crear y administrar implementaciones de grupos de recursos.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Crear y administrar implementaciones de grupos de recursos.|
|Microsoft.Storage/storageAccounts/listKeys/action|Enumerar claves de cuentas de almacenamiento.|
|Microsoft.Support/*|Crear y administrar incidencias de soporte técnico.|

### <a name="log-analytics-reader"></a>Lector de Log Analytics

Un lector de Log Analytics puede ver y buscar los datos de supervisión, así como consultar la configuración, incluida la de Azure Diagnostics en todos los recursos de Azure. La siguiente tabla muestra los permisos concedidos o denegados para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|*/read|Leer recursos de todos los tipos, excepto secretos.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|Administrar consultas de búsqueda en registros de Azure Monitor.|
|Microsoft.OperationalInsights/workspaces/search/action|Buscar datos de registros de Azure Monitor.|
|Microsoft.Support/*|Crear y administrar incidencias de soporte técnico.|
|**No acciones**| |
|Microsoft.OperationalInsights/workspaces/sharedKeys/read|No se pueden leer las claves de acceso compartido.|

### <a name="monitoring-contributor"></a>Colaborador de supervisión

Un colaborador de supervisión puede leer todos los datos de supervisión y actualizar la configuración de supervisión. La siguiente tabla muestra los permisos concedidos para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|*/read|Leer recursos de todos los tipos, excepto secretos.|
|Microsoft.AlertsManagement/alerts/*|Administrar alertas.|
|Microsoft.AlertsManagement/alertsSummary/*|Administrar el panel de alertas.|
|Microsoft.Insights/AlertRules/*|Administrar reglas de alertas.|
|Microsoft.Insights/components/*|Administrar componentes de Application Insights.|
|Microsoft.Insights/DiagnosticSettings/*|Administrar la configuración de diagnóstico.|
|Microsoft.Insights/eventtypes/*|Enumerar eventos del registro de actividades (eventos de administración) de una suscripción. Este permiso es aplicable para el acceso mediante programación y mediante el portal al registro de actividades.|
|Microsoft.Insights/LogDefinitions/*|Este permiso es necesario para los usuarios que necesitan acceder a registros de actividades a través del portal. Enumere las categorías de registro del registro de actividad.|
|Microsoft.Insights/MetricDefinitions/*|Leer definiciones de métrica (lista de tipos de métricas disponibles para un recurso).|
|Microsoft.Insights/Metrics/*|Leer las métricas de un recurso.|
|Microsoft.Insights/Register/Action|Registrar el proveedor de Microsoft.Insights.|
|Microsoft.Insights/webtests/*|Administrar pruebas web de Application Insights.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|Administrar paquetes de soluciones de registros de Azure Monitor.|
|Microsoft.OperationalInsights/workspaces/savedSearches/*|Administrar búsquedas guardadas de registros de Azure Monitor.|
|Microsoft.OperationalInsights/workspaces/search/action|Buscar áreas de trabajo de Log Analytics.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|Enumerar las claves de un área de trabajo de Log Analytics.|
|Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*|Administrar configuraciones de visión de almacenamiento de registros de Azure Monitor.|
|Microsoft.Support/*|Crear y administrar incidencias de soporte técnico.|
|Microsoft.WorkloadMonitor/workloads/*|Administrar cargas de trabajo.|

### <a name="monitoring-reader"></a>Lector de supervisión

Un lector de supervisión puede leer todos los datos de supervisión. La siguiente tabla muestra los permisos concedidos para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|*/read|Leer recursos de todos los tipos, excepto secretos.|
|Microsoft.OperationalInsights/workspaces/search/action|Buscar áreas de trabajo de Log Analytics.|
|Microsoft.Support/*|Crear y administrar incidencias de soporte técnico|

### <a name="user-access-administrator"></a>Administrador de acceso de usuario

Un administrador de acceso de usuario puede administrar el acceso de los usuarios a los recursos de Azure. La siguiente tabla muestra los permisos concedidos para el rol:

|**Acciones**  |**Descripción**  |
|---------|---------|
|*/read|Leer todos los recursos.|
|Microsoft.Authorization/*|Administrar la autorización|
|Microsoft.Support/*|Crear y administrar incidencias de soporte técnico|

## <a name="feature-setup-permissions"></a>Permisos de configuración de características

En las secciones siguientes se describen los permisos mínimos necesarios para habilitar las características Update Management y Change Tracking e Inventario.

### <a name="permissions-for-enabling-update-management-and-change-tracking-and-inventory-from-a-vm"></a>Permisos para habilitar Update Management y Change Tracking e Inventario en una máquina virtual

|**Acción**  |**Permiso**  |**Ámbito mínimo**  |
|---------|---------|---------|
|Escribir nueva implementación      | Microsoft.Resources/deployments/*          |Subscription          |
|Escribir nuevo grupo de recursos      | Microsoft.Resources/subscriptions/resourceGroups/write        | Subscription          |
|Crear nueva área de trabajo predeterminada      | Microsoft.OperationalInsights/workspaces/write         | Resource group         |
|Crear nueva cuenta      |  Microsoft.Automation/automationAccounts/write        |Resource group         |
|Vincular área de trabajo y cuenta      |Microsoft.OperationalInsights/workspaces/write</br>Microsoft.Automation/automationAccounts/read|Área de trabajo</br>Cuenta de Automation
|Crear extensión MMA      | Microsoft.Compute/virtualMachines/write         | Máquina virtual         |
|Crear búsqueda guardada      | Microsoft.OperationalInsights/workspaces/write          | Área de trabajo         |
|Crear configuración de ámbito      | Microsoft.OperationalInsights/workspaces/write          | Área de trabajo         |
|Comprobación de estado de incorporación: leer área trabajo      | Microsoft.OperationalInsights/workspaces/read         | Área de trabajo         |
|Comprobación de estado de incorporación: leer propiedad de área de trabajo vinculada de la cuenta     | Microsoft.Automation/automationAccounts/read      | Cuenta de Automation        |
|Comprobación de estado de incorporación: leer solución      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Solución         |
|Comprobación de estado de incorporación: leer máquina virtual      | Microsoft.Compute/virtualMachines/read         | Máquina virtual         |
|Comprobación de estado de incorporación: leer cuenta      | Microsoft.Automation/automationAccounts/read  |  Cuenta de Automation   |
| Incorporación de comprobación de área de trabajo para VM<sup>1</sup>       | Microsoft.OperationalInsights/workspaces/read         | Subscription         |
| Registro del proveedor de Log Analytics |Microsoft.Insights/register/action | Subscription|

<sup>1</sup> Este permiso es necesario para habilitar características a través de la experiencia de máquina virtual del portal.

### <a name="permissions-for-enabling-update-management-and-change-tracking-and-inventory-from-an-automation-account"></a>Permisos para habilitar Update Management y Change Tracking e inventario en una cuenta de Automation

|**Acción**  |**Permiso** |**Ámbito mínimo**  |
|---------|---------|---------|
|Crear una nueva implementación     | Microsoft.Resources/deployments/*        | Subscription         |
|Crear un grupo de recursos     | Microsoft.Resources/subscriptions/resourceGroups/write         | Subscription        |
|Hoja AutomationOnboarding: crear nueva área de trabajo     |Microsoft.OperationalInsights/workspaces/write           | Resource group        |
|Hoja AutomationOnboarding: leer área de trabajo vinculada     | Microsoft.Automation/automationAccounts/read        | Cuenta de Automation       |
|Hoja AutomationOnboarding: leer solución     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Solución        |
|Hoja AutomationOnboarding: leer área de trabajo     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Área de trabajo        |
|Crear vínculo para área de trabajo y cuenta     | Microsoft.OperationalInsights/workspaces/write        | Área de trabajo        |
|Escribir cuenta para Shoebox      | Microsoft.Automation/automationAccounts/write        | Cuenta        |
|Crear o editar búsqueda guardada     | Microsoft.OperationalInsights/workspaces/write        | Área de trabajo        |
|Crear o editar la configuración de ámbito     | Microsoft.OperationalInsights/workspaces/write        | Área de trabajo        |
| Registro del proveedor de Log Analytics |Microsoft.Insights/register/action | Subscription|
|**Paso 2: Habilitación de varias máquinas virtuales**     |         |         |
|Hoja VMOnboarding: crear extensión MMA     | Microsoft.Compute/virtualMachines/write           | Máquina virtual        |
|Crear o editar búsqueda guardada     | Microsoft.OperationalInsights/workspaces/write           | Área de trabajo        |
|Crear o editar la configuración de ámbito  | Microsoft.OperationalInsights/workspaces/write   | Área de trabajo|

## <a name="custom-azure-automation-contributor-role"></a>Rol Colaborador de Azure Automation personalizado

Microsoft quiere quitar los derechos de la cuenta de Automation del rol Colaborador de Log Analytics. Actualmente, el rol integrado [Colaborador de Log Analytics](#log-analytics-contributor) descrito antes puede escalar los privilegios al rol [Colaborador](./../role-based-access-control/built-in-roles.md#contributor) de la suscripción. Como las cuentas de ejecución de la cuenta de Automation se configuran inicialmente con derechos de Colaborador en la suscripción, un atacante puede usarla para crear runbooks y ejecutar código como Colaborador en la suscripción.

Debido a este riesgo de seguridad, se recomienda no utilizar el rol Colaborador de Log Analytics para ejecutar trabajos de Automation. En su lugar, cree el rol personalizado Colaborador de Azure Automation y utilícelo para las acciones relacionadas con la cuenta de Automation. Siga estos pasos para crear este rol personalizado.

### <a name="create-using-the-azure-portal"></a>Creación mediante Azure Portal

Siga estos pasos para crear un rol personalizado de Azure Automation en Azure Portal. Si quiere obtener más información, vea [Roles personalizados de Azure](./../role-based-access-control/custom-roles.md).

1. Copie y pegue la siguiente sintaxis JSON en un archivo. Guarde el archivo en la máquina local o en una cuenta de almacenamiento de Azure. En el archivo JSON, reemplace el valor de la propiedad **assignableScopes** por el GUID de la suscripción.

   ```json
   {
    "properties": {
        "roleName": "Automation Account Contributor (Custom)",
        "description": "Allows access to manage Azure Automation and its resources",
        "assignableScopes": [
            "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX"
        ],
        "permissions": [
            {
                "actions": [
                    "Microsoft.Authorization/*/read",
                    "Microsoft.Insights/alertRules/*",
                    "Microsoft.Insights/metrics/read",
                    "Microsoft.Insights/diagnosticSettings/*",
                    "Microsoft.Resources/deployments/*",
                    "Microsoft.Resources/subscriptions/resourceGroups/read",
                    "Microsoft.Automation/automationAccounts/*",
                    "Microsoft.Support/*"
                ],
                "notActions": [],
                "dataActions": [],
                "notDataActions": []
            }
        ]
      }
   }
   ```

1. Complete los pasos restantes como se describe en [Creación o actualización de roles personalizados de Azure mediante Azure Portal](../role-based-access-control/custom-roles-portal.md#start-from-json). En [Paso 3: Datos básicos](../role-based-access-control/custom-roles-portal.md#step-3-basics), tenga en cuenta lo siguiente:

    -  En el campo **Nombre del rol personalizado**, escriba **Colaborador de la cuenta de Automation (personalizado)** o un nombre que coincida con los estándares de nomenclatura.
    - En **Permisos de línea base**, seleccione **Iniciar desde JSON**. Después, seleccione el archivo JSON personalizado que ha guardado antes.

1. Complete los pasos restantes y, después, revise y cree el rol personalizado. El rol personalizado puede tardar unos minutos en aparecer en todas partes.

### <a name="create-using-powershell"></a>Creación mediante PowerShell

Siga estos pasos para crear el rol personalizado de Azure Automation con PowerShell. Si quiere obtener más información, vea [Roles personalizados de Azure](./../role-based-access-control/custom-roles.md).

1. Copie y pegue la siguiente sintaxis JSON en un archivo. Guarde el archivo en la máquina local o en una cuenta de almacenamiento de Azure. En el archivo JSON, reemplace el valor de la propiedad **AssignableScopes** por el GUID de la suscripción.

    ```json
    { 
        "Name": "Automation account Contributor (custom)",
        "Id": "",
        "IsCustom": true,
        "Description": "Allows access to manage Azure Automation and its resources",
        "Actions": [
            "Microsoft.Authorization/*/read",
            "Microsoft.Insights/alertRules/*",
            "Microsoft.Insights/metrics/read",
            "Microsoft.Insights/diagnosticSettings/*",
            "Microsoft.Resources/deployments/*",
            "Microsoft.Resources/subscriptions/resourceGroups/read",
            "Microsoft.Automation/automationAccounts/*",
            "Microsoft.Support/*"
        ],
        "NotActions": [],
        "AssignableScopes": [
            "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX"
        ] 
    } 
    ```

1. Complete los pasos restantes como se describe en [Creación o actualización de roles personalizados de Azure mediante Azure PowerShell](./../role-based-access-control/custom-roles-powershell.md#create-a-custom-role-with-json-template). El rol personalizado puede tardar unos minutos en aparecer en todas partes.

## <a name="update-management-permissions"></a>Permisos de Update Management

Update Management se puede usar para evaluar y programar implementaciones de actualizaciones en máquinas de varias suscripciones en el mismo inquilino de Azure Active Directory (Azure AD) o distintos inquilinos mediante Azure Lighthouse. En la tabla siguiente se muestran los permisos necesarios para administrar las implementaciones de actualizaciones.

|**Recurso** |**Rol** |**Ámbito** |
|---------|---------|---------|
|Cuenta de Automation |[Rol Colaborador de Azure Automation personalizado](#custom-azure-automation-contributor-role) |Cuenta de Automation |
|Cuenta de Automation |Colaborador de la máquina virtual  |Grupo de recursos para la cuenta  |
|Colaborador de Log Analytics en el área de trabajo de Log Analytics|Área de trabajo de Log Analytics |
|Área de trabajo de Log Analytics |Lector de Log Analytics|Subscription|
|Solución |Colaborador de Log Analytics |Solución|
|Máquina virtual |Colaborador de la máquina virtual |Máquina virtual |
|**Acciones en la máquina virtual** | | |
|Ver el historial de ejecución de la programación de actualizaciones ([se ejecuta la máquina de configuración de actualizaciones de software](/rest/api/automation/softwareupdateconfigurationmachineruns)) |Lector |Cuenta de Automation |
|**Acciones en la máquina virtual** |**Permiso** | |
|Crear programación de actualizaciones ([configuraciones de actualizaciones de software](/rest/api/automation/softwareupdateconfigurations)) |Microsoft.Compute/virtualMachines/write |Para grupos de recursos y listas de máquinas virtuales estáticas |
|Crear programación de actualizaciones ([configuraciones de actualizaciones de software](/rest/api/automation/softwareupdateconfigurations)) |Microsoft.OperationalInsights/workspaces/analytics/query/action |Para el identificador de recurso del área de trabajo cuando se usa una lista dinámica que no es de Azure.|

## <a name="configure-azure-rbac-for-your-automation-account"></a>Configuración de RBAC de Azure para la cuenta de Automation

En la sección siguiente se muestra cómo configurar RBAC de Azure en la cuenta de Automation mediante [Azure Portal](#configure-azure-rbac-using-the-azure-portal) y [PowerShell](#configure-azure-rbac-using-powershell).

### <a name="configure-azure-rbac-using-the-azure-portal"></a>Configuración de RBAC de Azure mediante Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y abra su cuenta de Automation en la página Cuentas de Automation.
2. Haga clic en **Control de acceso (IAM)** para abrir la página Control de acceso (IAM). Puede usar esta página para agregar nuevos usuarios, grupos y aplicaciones para administrar la cuenta de Automation y ver los roles existentes que se pueden configurar para la cuenta de Automation.
3. Haga clic en la pestaña **Asignaciones de roles**.

   ![Botón de acceso](media/automation-role-based-access-control/automation-01-access-button.png)

#### <a name="add-a-new-user-and-assign-a-role"></a>Adición de usuarios nuevos y asignación de roles

1. En la página Control de acceso (IAM), haga clic en **+ Agregar asignación de roles**. Esta acción abre la página Agregar asignación de roles, donde puede agregar un usuario, un grupo o una aplicación y asignar el rol correspondiente.

2. Seleccione en rol en la lista de roles disponibles. Puede elegir cualquiera de los roles integrados disponibles que admitan las cuentas de Automation o cualquier rol personalizado que haya definido.

3. Escriba el nombre del usuario al que desee conceder permisos en el campo **Seleccionar**. Elija el usuario de la lista y haga clic en **Guardar**.

   ![Agregar usuarios](media/automation-role-based-access-control/automation-04-add-users.png)

   Ahora debería ver el usuario agregado en la página Usuarios con el rol seleccionado asignado.

   ![Enumerar usuarios](media/automation-role-based-access-control/automation-05-list-users.png)

   También se puede asignar un rol al usuario desde la página Roles.

4. Haga clic en **Roles** en la página Control de acceso (IAM) para abrir la página Roles. Puede ver el nombre del rol y el número de usuarios y grupos asignados a dicho rol.

    ![Asignar rol en la página de usuarios](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)

   > [!NOTE]
   > El control de acceso basado en roles solo se puede establecer en el ámbito de la cuenta de Automation, no en los recursos por debajo de la cuenta de Automation.

#### <a name="remove-a-user"></a>Eliminación de un usuario

Puede quitar el permiso de usuario de cualquier usuario que no administre la cuenta de Automation o que haya dejado de trabajar en la organización. Estos son los pasos que deben seguirse para eliminar un usuario:

1. En la página Control de acceso (IAM), seleccione el usuario que desea eliminar y haga clic en **Eliminar**.
2. Haga clic en el botón **Quitar** del panel de detalles de asignación.
3. Haga clic en **Sí** para confirmar la eliminación.

   ![Quitar usuarios](media/automation-role-based-access-control/automation-08-remove-users.png)

### <a name="configure-azure-rbac-using-powershell"></a>Configuración de RBAC de Azure mediante PowerShell

El acceso basado en rol también se puede configurar en una cuenta de Automation mediante los siguientes [cmdlets de Azure PowerShell](../role-based-access-control/role-assignments-powershell.md):

[Get-AzRoleDefinition](/powershell/module/Az.Resources/Get-AzRoleDefinition) enumera todos los roles de Azure disponibles en Azure Active Directory. Puede usar este cmdlet con el parámetro `Name` para enumerar todas las acciones que puede realizar un rol específico.

```azurepowershell-interactive
Get-AzRoleDefinition -Name 'Automation Operator'
```

A continuación se incluye el resultado de ejemplo:

```azurepowershell
Name             : Automation Operator
Id               : d3881f73-407a-4167-8283-e981cbba0404
IsCustom         : False
Description      : Automation Operators are able to start, stop, suspend, and resume jobs
Actions          : {Microsoft.Authorization/*/read, Microsoft.Automation/automationAccounts/jobs/read, Microsoft.Automation/automationAccounts/jobs/resume/action,
                   Microsoft.Automation/automationAccounts/jobs/stop/action...}
NotActions       : {}
AssignableScopes : {/}
```

[Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) enumera las asignaciones de roles de Azure en el ámbito especificado. Sin parámetros, este cmdlet devuelve todas las asignaciones de roles realizadas en la suscripción. Use el parámetro `ExpandPrincipalGroups` para enumerar las asignaciones de acceso del usuario especificado, así como los grupos a los que pertenezca.

**Ejemplo**: use el siguiente cmdlet para enumerar todos los usuarios de una cuenta de Automation y sus roles.

```azurepowershell-interactive
Get-AzRoleAssignment -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

A continuación se incluye el resultado de ejemplo:

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/cc594d39-ac10-46c4-9505-f182a355c41f
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : 15f26a47-812d-489a-8197-3d4853558347
ObjectType         : User
```

Use [New-AzRoleAssignment](/powershell/module/Az.Resources/New-AzRoleAssignment) para asignar acceso a usuarios, grupos y aplicaciones en un ámbito determinado.

**Ejemplo**: use el siguiente comando para asignar el rol "Operador de Automation" para un usuario en el ámbito de la cuenta de Automation.

```azurepowershell-interactive
New-AzRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

A continuación se incluye el resultado de ejemplo:

```azurepowershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/25377770-561e-4496-8b4f-7cba1d6fa346
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : f5ecbe87-1181-43d2-88d5-a8f5e9d8014e
ObjectType         : User
```

Use [Remove-AzRoleAssignment](/powershell/module/Az.Resources/Remove-AzRoleAssignment) para quitar el acceso a un usuario, grupo o aplicación concretos de un ámbito determinado.

**Ejemplo**: Use el siguiente comando para quitar el usuario del rol Operador de Automation en el ámbito de la cuenta de Automation.

```azurepowershell-interactive
Remove-AzRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

En el ejemplo anterior, reemplace `sign-in ID of a user you wish to remove`, `SubscriptionID`, `Resource Group Name` y `Automation account name` por los detalles de su cuenta. Elija **Sí** cuando se le pida que confirme antes de proceder a la eliminación de las asignaciones de roles de usuario.

### <a name="user-experience-for-automation-operator-role---automation-account"></a>Experiencia del usuario en el rol Operador de Automation: cuenta de Automation

Cuando un usuario asignado al rol Operador de Automation en el ámbito de la cuenta de Automation vea la cuenta de Automation a la que está asignado, solo verá la lista de runbooks, los trabajos de runbook y las programaciones creados en la cuenta de Automation. Este usuario no puede ver las definiciones de estos elementos. El usuario puede iniciar, detener, suspender, reanudar o programar el trabajo de runbook. Pero el usuario no tiene acceso a otros recursos de Automation como configuraciones, grupos de Hybrid Runbook Worker o nodos de DSC.

![Sin acceso a los recursos](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

## <a name="configure-azure-rbac-for-runbooks"></a>Configuración de RBAC de Azure para runbooks

Azure Automation permite asignar roles de Azure a runbooks específicos. Para ello, ejecute el siguiente script para agregar un usuario a un runbook específico. Un administrador de la cuenta de Automation o un administrador de inquilinos pueden ejecutar este script.

```azurepowershell-interactive
$rgName = "<Resource Group Name>" # Resource Group name for the Automation account
$automationAccountName ="<Automation account name>" # Name of the Automation account
$rbName = "<Name of Runbook>" # Name of the runbook
$userId = "<User ObjectId>" # Azure Active Directory (AAD) user's ObjectId from the directory

# Gets the Automation account resource
$aa = Get-AzResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts" -ResourceName $automationAccountName

# Get the Runbook resource
$rb = Get-AzResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts/runbooks" -ResourceName "$rbName"

# The Automation Job Operator role only needs to be run once per user.
New-AzRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Job Operator" -Scope $aa.ResourceId

# Adds the user to the Automation Runbook Operator role to the Runbook scope
New-AzRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Runbook Operator" -Scope $rb.ResourceId
```

Una vez ejecutado el script, los usuarios deberán iniciar sesión en Azure Portal y seleccionar **Todos los recursos**. En la lista, el usuario puede ver el runbook para el que se ha agregado como Operador de runbook de Automation.

![RBAC de Azure de runbook en el portal](./media/automation-role-based-access-control/runbook-rbac.png)

### <a name="user-experience-for-automation-operator-role---runbook"></a>Experiencia del usuario en el rol Operador de Automation: runbook

Cuando un usuario asignado al rol Operador de Automation en el ámbito del runbook vea un runbook al que esté asignado, solo podrá iniciar el runbook y ver los trabajos de runbook.

![Acceso solo a la página de inicio](media/automation-role-based-access-control/automation-only-start.png)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre RBAC de Azure con PowerShell, vea [Incorporación o eliminación de asignaciones de roles de Azure con Azure PowerShell](../role-based-access-control/role-assignments-powershell.md).
* Para más información sobre los tipos de runbooks, consulte [Tipos de runbooks de Azure Automation](automation-runbook-types.md).
* Para iniciar un runbook, consulte [Inicio de un runbook en Azure Automation](start-runbooks.md).
