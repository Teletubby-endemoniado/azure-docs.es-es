---
title: 'Ejemplo de PowerShell: enumeración de usuarios y grupos de una aplicación de Application Proxy de Azure Active Directory'
description: Ejemplo de PowerShell en el que se enumeran todos los usuarios y grupos asignados a una aplicación específica de Azure Active Directory (Azure AD) Application Proxy.
services: active-directory
author: kenwith
manager: karenh444
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: sample
ms.date: 04/29/2021
ms.author: kenwith
ms.reviewer: ashishj
ms.openlocfilehash: 8b07e9673c9305826a1e02e03eac1d17b519772c
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129988323"
---
# <a name="display-users-and-groups-assigned-to-an-application-proxy-application"></a>Visualización de usuarios y grupos asignados a una aplicación de Application Proxy

En este script de ejemplo de PowerShell en el que se enumeran todos los usuarios y grupos asignados a una aplicación específica de Azure Active Directory (Azure AD) Application Proxy.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

El ejemplo requiere el [módulo Azure AD V2 de PowerShell para Graph](/powershell/azure/active-directory/install-adv2) (Azure AD) o la [versión preliminar del módulo Azure AD V2 de PowerShell para Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (versión preliminar de Azure AD).

## <a name="sample-script"></a>Script de ejemplo

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/display-users-group-of-an-app.ps1 "Display users and groups assigned to an Application Proxy application")]

## <a name="script-explanation"></a>Explicación del script

| Get-Help | Notas |
|---|---|
| [Get-AzureADUser](/powershell/module/AzureAD/get-azureaduser)| Obtiene un usuario. |
| [Get-AzureADGroup](/powershell/module/AzureAD/get-azureadgroup)| Obtiene un grupo. |
| [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Obtiene una entidad de servicio. |
| [Get-AzureADUserAppRoleAssignment](/powershell/module/AzureAD/get-azureaduserapproleassignment) | Obtiene una asignación de roles de aplicación de usuario. |
| [Get-AzureADGroupAppRoleAssignment](/powershell/module/AzureAD/get-azureadgroupapproleassignment) | Obtiene una asignación de roles de aplicación de grupo. |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure AD PowerShell, consulte [Información general del módulo de Azure AD PowerShell](/powershell/azure/active-directory/overview).

Para obtener otros ejemplos de PowerShell para Application Proxy, consulte [Ejemplos de Azure AD PowerShell para Azure AD Application Proxy](../application-proxy-powershell-samples.md).
