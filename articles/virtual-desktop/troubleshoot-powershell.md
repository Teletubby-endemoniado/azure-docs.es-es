---
title: 'PowerShell para Azure Virtual Desktop: Azure'
description: Cómo solucionar problemas de PowerShell al configurar un entorno de Azure Virtual Desktop.
author: Heidilohr
ms.topic: troubleshooting
ms.date: 06/05/2020
ms.author: helohr
ms.custom: devx-track-azurepowershell
manager: femila
ms.openlocfilehash: 3426ff64bbaf1d3d4801a8fcc069cb1767091181
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124804115"
---
# <a name="azure-virtual-desktop-powershell"></a>PowerShell para Azure Virtual Desktop

>[!IMPORTANT]
>Este contenido se aplica a Azure Virtual Desktop con objetos de Azure Resource Manager. Si usa Azure Virtual Desktop (clásico) sin objetos de Azure Resource Manager, consulte [este artículo](./virtual-desktop-fall-2019/troubleshoot-powershell-2019.md).

Use este artículo para resolver los problemas y errores al usar PowerShell con Azure Virtual Desktop. Para más información sobre PowerShell para Servicios de Escritorio remoto, consulte [PowerShell para Azure Virtual Desktop](/powershell/windows-virtual-desktop/overview).

## <a name="provide-feedback"></a>Envío de comentarios

Visite la [comunidad técnica de Azure Virtual Desktop](https://techcommunity.microsoft.com/t5/azure-virtual-desktop/bd-p/AzureVirtualDesktopForum) para analizar el servicio Azure Virtual Desktop con el equipo de producto y los miembros activos de la comunidad.

## <a name="powershell-commands-used-during-azure-virtual-desktop-setup"></a>Comandos de PowerShell usados durante la configuración de Azure Virtual Desktop

En esta sección se enumeran los comandos de PowerShell que se usan normalmente durante la configuración de Azure Virtual Desktop, y proporciona maneras de resolver los problemas que pueden surgir al usarlos.

### <a name="error-new-azroleassignment-the-provided-information-does-not-map-to-an-ad-object-id"></a>Error: New-AzRoleAssignment: "The provided information does not map to an AD object ID" (New-AzRoleAssignment: la información proporcionada no está asignada a ningún id. de objeto de AD).

```powershell
New-AzRoleAssignment -SignInName "admins@contoso.com" -RoleDefinitionName "Desktop Virtualization User" -ResourceName "0301HP-DAG" -ResourceGroupName 0301RG -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

**Causa:** El usuario especificado con el parámetro *-SignInName* no se encuentra en la instancia de Azure Active Directory vinculada al entorno de Azure Virtual Desktop.

**Solución:** Realice las siguientes comprobaciones:

- El usuario debe estar sincronizado con Azure Active Directory.
- El usuario no debe estar vinculado con comercios de negocio a consumidor (B2C) ni de negocio a negocio (B2B).
- El entorno de Azure Virtual Desktop debe estar vinculado a la instancia correcta de Azure Active Directory.

### <a name="error-new-azroleassignment-the-client-with-object-id-does-not-have-authorization-to-perform-action-over-scope-code-authorizationfailed"></a>Error: New-AzRoleAssignment: "El cliente con el id. de objeto no está autorizado para realizar la acción sobre el ámbito (código: AuthorizationFailed)"

**Causa 1:** la cuenta usada no tiene permisos de propietario en la suscripción.

**Corrección 1:** un usuario con permisos de propietario debe ejecutar la asignación de roles. Como alternativa, el usuario debe tener asignado el rol de administrador de acceso de usuario para asignar un usuario a un grupo de aplicaciones.

**Causa 2:** La cuenta usada tiene permisos de propietario, pero no forma parte de la instancia de Azure Active Directory del entorno o no tiene permisos para realizar consultas en la instancia de Azure Active Directory donde se encuentra el usuario.

**Corrección 2:** Un usuario con permisos de Active Directory debe ejecutar la asignación de roles.

### <a name="error-new-azwvdhostpool----the-location-is-not-available-for-resource-type"></a>Error: New-AzWvdHostPool: la ubicación no está disponible para el tipo de recurso

```powershell
New-AzWvdHostPool_CreateExpanded: The provided location 'southeastasia' is not available for resource type 'Microsoft.DesktopVirtualization/hostpools'. List of available regions for the resource type is 'eastus,eastus2,westus,westus2,northcentralus,southcentralus,westcentralus,centralus'.
```

Causa: Azure Virtual Desktop permite seleccionar la ubicación de grupos de hosts, grupos de aplicaciones y áreas de trabajo para almacenar metadatos de servicio en determinadas ubicaciones. Las opciones están restringidas en función de dónde esté disponible la característica. Este error significa que la característica no está disponible en la ubicación que ha elegido.

Solución: se publicará una lista de las regiones admitidas en el mensaje de error. Use alguna de las regiones admitidas.

### <a name="error-new-azwvdapplicationgroup-must-be-in-same-location-as-host-pool"></a>Error: New-AzWvdApplicationGroup debe estar en la misma ubicación que el grupo de hosts.

```powershell
New-AzWvdApplicationGroup_CreateExpanded: ActivityId: e5fe6c1d-5f2c-4db9-817d-e423b8b7d168 Error: ApplicationGroup must be in same location as associated HostPool
```

**Causa:** Hay un error de coincidencia de ubicación. Todos los grupos de hosts, grupos de aplicaciones y áreas de trabajo tienen una ubicación para almacenar los metadatos de servicio. Los objetos que cree que estén asociados entre sí deben estar en la misma ubicación. Por ejemplo, si un grupo de hosts está en `eastus`, también debe crear los grupos de aplicaciones en `eastus`. Si crea un área de trabajo para registrar estos grupos de aplicaciones, esta también debe estar en `eastus`.

**Solución:** Recupere la ubicación en la que se creó el grupo de hosts y, a continuación, asigne el grupo de aplicaciones que está creando a esa misma ubicación.

## <a name="next-steps"></a>Pasos siguientes

- Para información general sobre la solución de problemas de Azure Virtual Desktop y las pistas de escalación, consulte [Introducción a la solución de problemas, comentarios y soporte técnico](troubleshoot-set-up-overview.md).
- Para solucionar problemas durante la configuración de grupos de hosts y el entorno de Azure Virtual Desktop, consulte [Creación de entornos y grupos de hosts](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en Azure Virtual Desktop, consulte [Configuración de la máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas con conexiones de cliente de Azure Virtual Desktop, consulte [Conexiones de servicios de Azure Virtual Desktop](troubleshoot-service-connection.md).
- Para solucionar problemas con los clientes de Escritorio remoto, consulte [Solución de problemas del cliente de Escritorio remoto](troubleshoot-client.md).
- Para más información sobre el servicio, consulte [Entorno de Azure Virtual Desktop](environment-setup.md).
- Para más información sobre las acciones de auditoría, consulte [Operaciones de auditoría con Resource Manager](../azure-monitor/essentials/activity-log.md).
- Si desea conocer más detalles sobre las acciones que permiten determinar los errores durante la implementación, consulte [Visualización de operaciones de implementación con el Portal de Azure](../azure-resource-manager/templates/deployment-history.md).
