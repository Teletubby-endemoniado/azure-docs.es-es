---
title: Introducción a la autenticación de cuentas de Azure Automation
description: En este artículo se ofrece información general sobre la autenticación de cuentas de Azure Automation.
keywords: seguridad de automatización, automatización segura; autenticación de automatización
services: automation
ms.subservice: process-automation
ms.date: 10/26/2021
ms.topic: conceptual
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 08f265d4e2af8fe985db3ceab78b535db2f73924
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131470877"
---
# <a name="azure-automation-account-authentication-overview"></a>Introducción a la autenticación de cuentas de Azure Automation

Azure Automation le permite automatizar tareas en recursos de Azure, locales y de otros proveedores de servicios en la nube como Amazon Web Services (AWS). Puede usar runbooks para automatizar las tareas, o bien una instancia de Hybrid Runbook Worker si tiene que administrar procesos empresariales u operativos fuera de Azure. Para trabajar en cualquiera de estos entornos, se requieren permisos para proteger el acceso a los recursos con los derechos mínimos necesarios.

En este artículo se tratan los distintos escenarios de autenticación que se admiten en Azure Automation y se indica cómo empezar a trabajar según los entornos que haya que administrar.

## <a name="automation-account"></a>Cuenta de Automation

Cuando inicia Azure Automation por primera vez, debe crear al menos una cuenta de Automation. Las cuentas de Automation le permiten aislar sus recursos, runbooks, activos y configuraciones de Automation de los recursos de otras cuentas. Puede usar cuentas de Automation para separar los recursos en entornos lógicos independientes o responsabilidades delegadas. Por ejemplo, puede usar una cuenta para desarrollo, otra para producción y otra para su entorno local. O bien, puede dedicar una cuenta de Automation para administrar las actualizaciones del sistema operativo en todas las máquinas con [Update Management](update-management/overview.md).

Una cuenta de Azure Automation es diferente de su cuenta Microsoft o de las cuentas creadas en su suscripción de Azure. Para obtener una introducción a la creación de una cuenta de Automation, consulte [Creación de una cuenta independiente de Azure Automation](./quickstarts/create-account-portal.md).

## <a name="automation-resources"></a>Recursos de Automation

Los recursos de Automation de cada cuenta de Automation están asociados a una sola región de Azure, pero la cuenta puede administrar todos los recursos de la suscripción de Azure. El principal motivo para crear cuentas de Automation en distintas regiones es si tiene directivas que requieren que los datos y recursos se aíslen en una región específica.

Todas las tareas que se crean en los recursos con Azure Resource Manager y los cmdlets de PowerShell en Azure Automation deben autenticarse en Azure mediante la autenticación basada en credenciales de la identidad organizativa de Azure Active Directory (Azure AD).

## <a name="managed-identities"></a>Identidades administradas

Una identidad administrada de Azure Active Directory (Azure AD) permite al runbook acceder fácilmente a otros recursos protegidos por Azure AD. La plataforma Azure administra la identidad y no es necesario que lleve a cabo el aprovisionamiento ni la rotación de los secretos. Para más información acerca de las identidades administradas en Azure AD, consulte [Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md).

Las identidades administradas son la manera recomendada de autenticarse en los runbooks y es el método de autenticación predeterminado para la cuenta de Automation.

Estas son algunas de las ventajas de usar las identidades administradas:

- El uso de una identidad administrada en lugar de la cuenta de ejecución de Automation simplifica la administración. No es necesario renovar el certificado que usa la cuenta de ejecución.

- Las identidades administradas se pueden usar sin ningún costo adicional.

- No tiene que especificar el objeto de conexión de ejecución en el código del runbook. Puede acceder a los recursos con la identidad administrada de la cuenta de Automation desde un runbook sin crear certificados, conexiones, cuentas de ejecución, etc.

Una cuenta de Automation puede autenticarse mediante dos tipos de identidades administradas:

- Una identidad asignada por el sistema está asociada a la aplicación y se elimina si se elimina la aplicación. Una aplicación solo puede tener una identidad asignada por el sistema.

- Una identidad asignada por el usuario es un recurso de Azure independiente que puede asignarse a la aplicación. Una aplicación puede tener varias identidades asignadas por el usuario.

> [!NOTE]
> Las identidades asignadas por el usuario solo se admiten para trabajos en la nube. Para más información sobre las distintas identidades administradas, vea [Administración de tipos de identidad](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types).

Para más información acerca del uso de identidades administradas, consulte [Habilitación de la identidad administrada para Azure Automation](enable-managed-identity-for-automation.md).

## <a name="run-as-accounts"></a>Cuentas de ejecución

Las cuentas de ejecución de Azure Automation proporcionan autenticación para administrar recursos de Azure Resource Manager o recursos implementados en el modelo de implementación clásica. Hay dos tipos de cuentas de ejecución en Azure Automation:

Para crear o renovar una cuenta de ejecución, se necesitan permisos en tres niveles:

- Suscripción,
- Azure Active Directory (Azure AD), y
- Cuenta de Automation

> [!NOTE]
> Azure Automation no crea automáticamente la cuenta de ejecución. Se ha reemplazado por el uso de identidades administradas.

### <a name="subscription-permissions"></a>Permisos de suscripción

Debe tener el permiso `Microsoft.Authorization/*/Write`. Este permiso se obtiene mediante la pertenencia a uno de los siguientes roles integrados de Azure:

- [Propietario](../role-based-access-control/built-in-roles.md#owner)
- [Administrador de acceso de usuario](../role-based-access-control/built-in-roles.md#user-access-administrator)

Para configurar o renovar las cuentas de ejecución clásicas, debe tener el rol Coadministrador en el nivel de suscripción. Para más información sobre los permisos de suscripción clásicos, consulte [Administradores de la suscripción clásica de Azure](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

### <a name="azure-ad-permissions"></a>Permisos de Azure AD

Para poder crear o renovar la entidad de servicio, debe ser miembro de uno de los siguientes roles integrados de Azure AD:

- [Administrador de aplicaciones](../active-directory/roles/permissions-reference.md#application-administrator)
- [Desarrollador de aplicaciones](../active-directory/roles/permissions-reference.md#application-developer)

La pertenencia se puede asignar a **TODOS** los usuarios del inquilino en el nivel de directorio, que es el comportamiento predeterminado. Puede conceder pertenencia a cualquiera de los roles en el nivel de directorio. Para más información, vea [¿Quién tiene permiso para agregar aplicaciones a la instancia de Azure AD?](../active-directory/develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance)

### <a name="automation-account-permissions"></a>Permisos de la cuenta de Automation

Para poder crear o actualizar la cuenta de Automation, debe ser miembro de uno de los siguientes roles de la cuenta de Automation:

- [Propietario](./automation-role-based-access-control.md#owner)
- [Colaborador](./automation-role-based-access-control.md#contributor)
- [Colaborador de Azure Automation personalizado](./automation-role-based-access-control.md#custom-azure-automation-contributor-role)

Para obtener más información sobre los modelos de implementación clásico y de Azure Resource Manager, consulte [Implementación de Resource Manager e implementación clásica](../azure-resource-manager/management/deployment-models.md).

>[!NOTE]
>Las suscripciones del Proveedor de soluciones en la nube (CSP) de Azure solo admiten el modelo de Azure Resource Manager. Los servicios que no son de Azure Resource Manager no están disponibles en el programa. Cuando se usa una suscripción al programa CSP, no se crea la cuenta de ejecución de Azure clásico, sino la cuenta de ejecución de Azure. Para más información acerca de las suscripciones de CSP, consulte [Servicios disponibles en las suscripciones de CSP](/azure/cloud-solution-provider/overview/azure-csp-available-services).

Al crear una cuenta de Automation, la cuenta de ejecución se crea de manera predeterminada a la misma vez con un certificado autofirmado. Si decidió no crearla junto con la cuenta de Automation, se puede crear individualmente en otro momento. Una cuenta de ejecución de Azure clásico es opcional y se crea por separado si necesita administrar los recursos clásicos.

> [!NOTE]
> Azure Automation no crea automáticamente la cuenta de ejecución. Se ha reemplazado por el uso de identidades administradas.

Si quiere usar un certificado emitido por la entidad de certificación (CA) empresarial o de terceros en lugar del certificado autofirmado predeterminado, puede usar la opción [Script de PowerShell para crear una cuenta de ejecución](create-run-as-account.md#powershell-script-to-create-a-run-as-account) para las cuentas de ejecución y de ejecución clásicas.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWwtF3]

### <a name="run-as-account"></a>Cuenta de ejecución

Cuando crea una cuenta de ejecución, realiza las siguientes tareas:

* Crea una aplicación de Azure AD con un certificado autofirmado, crea una cuenta de entidad servicio para la aplicación en Azure AD y asigna el rol [Colaborador](../role-based-access-control/built-in-roles.md#contributor) para esta cuenta en la suscripción actual. Puede cambiar la configuración de certificado a [Lector](../role-based-access-control/built-in-roles.md#reader) o a cualquier otro rol. Para más información, consulte [Control de acceso basado en rol en Azure Automation](automation-role-based-access-control.md).

* Crea un recurso de certificado de Automation denominado `AzureRunAsCertificate` en la cuenta de Automation especificada. El recurso de certificado contiene la clave privada del certificado que se usa en la aplicación de Azure AD.

* Crea un recurso de conexión de Automation denominado `AzureRunAsConnection` en la cuenta de Automation especificada. El recurso de conexión contiene el identificador de la aplicación, el identificador del inquilino, el identificador de la suscripción y la huella digital del certificado.

### <a name="azure-classic-run-as-account"></a>Cuenta de ejecución de Azure clásico

Al crear una cuenta de ejecución de Azure clásico, realiza las siguientes tareas:

> [!NOTE]
> Debe ser coadministrador de la suscripción para crear o renovar este tipo de cuenta de ejecución.

* Crea un certificado de administración en la suscripción.

* Crea un recurso de certificado de Automation denominado `AzureClassicRunAsCertificate` en la cuenta de Automation especificada. El recurso de certificado contiene la clave privada del certificado que usa el certificado de administración.

* Crea un recurso de conexión de Automation denominado `AzureClassicRunAsConnection` en la cuenta de Automation especificada. El recurso de conexión contiene el nombre de la suscripción, el id. de suscripción y el nombre del recurso de certificado.

## <a name="service-principal-for-run-as-account"></a>Entidad de servicio para la cuenta de ejecución

La entidad de servicio para una cuenta de ejecución no tiene los permisos para leer Azure AD de manera predeterminada. Si quiere agregar permisos para leer o administrar Azure AD, tiene que conceder los permisos a la entidad de servicio en **Permisos de API**. Para más información, consulte [Adición de permisos para acceder a la API web](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-your-web-api).

## <a name="run-as-account-permissions"></a><a name="permissions"></a>Permisos de las cuentas de ejecución

En esta sección se definen los permisos para las cuentas de ejecución normales y las cuentas de ejecución clásicas.

* Para crear o actualizar una cuenta de ejecución, un Administrador de aplicaciones en Azure Active Directory y un Propietario en la suscripción pueden completar todas las tareas.
* Para configurar o renovar las cuentas de ejecución clásicas, debe tener el rol Coadministrador en el nivel de suscripción. Para más información sobre los permisos de suscripción clásicos, consulte [Administradores de la suscripción clásica de Azure](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

En una situación en la que tenga separación de funciones, la siguiente tabla muestra una lista de las tareas, el cmdlet equivalente y los permisos necesarios:

|Tarea|Cmdlet  |Permisos mínimos  |Donde se establecen los permisos|
|---|---------|---------|---|
|Crear una aplicación de Azure AD|[New-AzADApplication](/powershell/module/az.resources/new-azadapplication)     | Rol de desarrollador de aplicaciones<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)</br>Inicio > Azure AD > Registros de aplicaciones |
|Agregar una credencial a la aplicación.|[New-AzADAppCredential](/powershell/module/az.resources/new-azadappcredential)     | Administrador de la aplicación o administrador global<sup>1</sup>         |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)</br>Inicio > Azure AD > Registros de aplicaciones|
|Crear y obtener una entidad de servicio de Azure AD|[New-AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal)</br>[Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal)     | Administrador de la aplicación o administrador global<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app)</br>Inicio > Azure AD > Registros de aplicaciones|
|Asignar u obtener el rol de Azure para la entidad de seguridad especificada|[New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment)</br>[Get-AzRoleAssignment](/powershell/module/Az.Resources/Get-AzRoleAssignment)      | Administrador de acceso de usuario o Propietario, o bien tener los permisos siguientes:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br> | [Suscripción](../role-based-access-control/role-assignments-portal.md)</br>Inicio > Suscripciones > \<subscription name\> Access Control (IAM)|
|Crear o quitar un certificado de Automation|[New-AzAutomationCertificate](/powershell/module/Az.Automation/New-AzAutomationCertificate)</br>[Remove-AzAutomationCertificate](/powershell/module/az.automation/remove-azautomationcertificate)     | Colaborador en el grupo de recursos         |Grupo de recursos de la cuenta de Automation|
|Crear o quitar una conexión de Automation|[New-AzAutomationConnection](/powershell/module/az.automation/new-azautomationconnection)</br>[Remove-AzAutomationConnection](/powershell/module/az.automation/remove-azautomationconnection)|Colaborador en el grupo de recursos |Grupo de recursos de la cuenta de Automation|

<sup>1</sup> Los usuarios que no son administradores en el inquilino de Azure AD pueden [registrar aplicaciones de AD](../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app) si la opción **Los usuarios pueden registrar aplicaciones** del inquilino de Azure AD en la página **Configuración de usuario** está establecida en **Sí**. Si el registro de la aplicación está configurado en **No**, el usuario que realiza esta acción debe ser como se define en esta tabla.

Si no es miembro de la instancia de Active Directory de la suscripción antes de que se le agregue al rol Administrador global de la suscripción, se le agregará como invitado. En este caso, recibirá una advertencia `You do not have permissions to create…` en la página **Agregar cuenta de Automation**.

Para comprobar que se ha corregido la situación causante del mensaje de error:

1. En el panel de Azure Active Directory de Azure Portal, seleccione **Usuarios y grupos**.
2. Seleccione **Todos los usuarios**.
3. Elija su nombre y, a continuación, seleccione **Perfil**.
4. Asegúrese de que el valor del atributo **Tipo de usuario** en el perfil de usuario no esté establecido en **Invitado**.

## <a name="role-based-access-control"></a>Control de acceso basado en rol

El control de acceso basado en rol está disponible en Azure Resource Manager para conceder las acciones permitidas a una cuenta de usuario de Azure AD y a una cuenta de ejecución, y para autenticar la entidad de servicio. Para más información que le ayude a desarrollar su modelo de administración de permisos de Automation, consulte el [artículo Control de acceso basado en rol en Azure Automation](automation-role-based-access-control.md).

Si tiene controles de seguridad estrictos para la asignación de permisos en los grupos de recursos, debe asignar la pertenencia a la cuenta de ejecución al rol **Colaborador** en el grupo de recursos.

> [!NOTE]
> Se recomienda no utilizar el rol **Colaborador de Log Analytics** para ejecutar trabajos de Automation. En su lugar, cree el rol personalizado Colaborador de Azure Automation y utilícelo para las acciones relacionadas con la cuenta de Automation. Para más información, vea [Rol Colaborador de Azure Automation personalizado](./automation-role-based-access-control.md#custom-azure-automation-contributor-role).

## <a name="runbook-authentication-with-hybrid-runbook-worker"></a>Autenticación de Runbook con Hybrid Runbook Worker

Los runbooks que se ejecutan en una instancia de Hybrid Runbook Worker en su centro de datos o en los servicios de computación de otros entornos de nube, como AWS, no pueden usar el mismo método que se usa normalmente para los runbooks que se autentican en recursos de Azure. Esto se debe a que esos recursos se ejecutan fuera de Azure y, por lo tanto, requieren sus propias credenciales de seguridad definidas en Automatización para la autenticación en los recursos a los que tienen acceso localmente. Para más información sobre la autenticación de runbooks con trabajos de runbook, consulte [Ejecución de runbooks en Hybrid Runbook Worker](automation-hrw-run-runbooks.md).

Con los runbooks que utilizan instancias de Hybrid Runbook Worker en máquinas virtuales de Azure, puede utilizar la [autenticación de runbook con identidades administradas](automation-hrw-run-runbooks.md#runbook-auth-managed-identities) en lugar de cuentas de ejecución para autenticarse en sus recursos de Azure.

## <a name="next-steps"></a>Pasos siguientes

* Para crear una cuenta de Automation desde Azure Portal, consulte [Creación de una cuenta independiente de Azure Automation](automation-create-standalone-account.md).
* Si prefiere crear su cuenta mediante una plantilla, consulte [Creación de una cuenta de Automation con una plantilla de Azure Resource Manager](quickstart-create-automation-account-template.md).
* Para la autenticación con Amazon Web Services, consulte [Autenticación de runbooks de Azure Automation con Amazon Web Services](automation-config-aws-account.md).
* Para ver una lista de los servicios de Azure que admiten la característica Managed Identities for Azure Resources, consulte [Services that support managed identities for Azure resources](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) (Servicios que admiten la característica Managed Identities for Azure Resources).