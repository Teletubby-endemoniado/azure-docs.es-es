---
title: Configuración de las clasificaciones de los permisos
titleSuffix: Azure AD
description: Obtenga información acerca de cómo administrar las clasificaciones de permisos delegados.
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 10/23/2021
ms.author: davidmu
ms.reviewer: arvindh, luleon, phsignor
ms.custom: contperf-fy21q2
ms.openlocfilehash: c061dcbbf21c67e0b14768f5918e76f8bcb43e0d
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131471238"
---
# <a name="configure-permission-classifications-in-azure-active-directory"></a>Configuración de clasificaciones de permisos en Azure Active Directory

En este artículo va a aprender a configurar clasificaciones de permisos en Azure Active Directory (Azure AD). Las clasificaciones de permisos permiten identificar el impacto que tienen los distintos permisos según las evaluaciones de riesgo y las directivas de la organización. Por ejemplo, puede usar las clasificaciones de permisos en las directivas de consentimiento para identificar el conjunto de permisos a los que los usuarios pueden dar su consentimiento.

Actualmente, solo se admite la clasificación de permisos de "bajo impacto". Solo los permisos delegados que no requieren el consentimiento del administrador se pueden clasificar como "bajo impacto".

Los permisos mínimos necesarios para realizar el inicio de sesión básico son `openid`, `profile`, `email`, `User.Read` y `offline_access`, puesto que son todos los permisos delegados en Microsoft Graph. Con estos permisos, una aplicación puede leer los detalles completos del perfil del usuario que ha iniciado sesión y puede mantener este acceso incluso cuando el usuario ya no usa la aplicación.

## <a name="prerequisites"></a>Prerrequisitos

Para configurar clasificaciones de permisos necesita:

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Uno de los siguientes roles: Administrador global, Administrador de aplicaciones en la nube, Administrador de aplicaciones o Propietario de la entidad de servicio.

## <a name="manage-permission-classifications"></a>Administración de las clasificaciones de los permisos

# <a name="portal"></a>[Portal](#tab/azure-portal)

Siga estos pasos para clasificar los permisos mediante Azure Portal:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como [Administrador global](../roles/permissions-reference.md#global-administrator), [Administrador de aplicaciones](../roles/permissions-reference.md#application-administrator) o [Administrador de aplicaciones en la nube](../roles/permissions-reference.md#cloud-application-administrator).
1. Seleccione **Azure Active Directory** > **Aplicaciones empresariales** > **Consentimiento y permisos** > **Clasificaciones de permisos**.
1. Elija **Agregar permisos** para clasificar otro permiso como "bajo impacto".
1. Seleccione la API y, después, seleccione los permisos delegados.

En este ejemplo, hemos clasificado el conjunto mínimo de permisos necesarios para el inicio de sesión único:

:::image type="content" source="media/configure-permission-classifications/permission-classifications.png" alt-text="Clasificaciones de permisos":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Puede usar el módulo de versión preliminar de Azure AD PowerShell más reciente, [AzureADPreview](/powershell/module/azuread/?preserve-view=true&view=azureadps-2.0-preview), para clasificar los permisos. Las clasificaciones de permisos se configuran en el objeto **ServicePrincipal** de la API que publica los permisos.

#### <a name="list-the-current-permission-classifications-for-an-api"></a>Enumeración de las clasificaciones de permisos actuales para una API

1. Recupere el objeto **ServicePrincipal** para la API. Aquí se recupera el objeto ServicePrincipal para Microsoft Graph API:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Lea las clasificaciones de permisos delegados para la API:

   ```powershell
   Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId | Format-Table Id, PermissionName, Classification
   ```

#### <a name="classify-a-permission-as-low-impact"></a>Clasificación de un permiso como de "bajo impacto"

1. Recupere el objeto **ServicePrincipal** para la API. Aquí se recupera el objeto ServicePrincipal para Microsoft Graph API:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Busque el permiso delegado que desea clasificar:

   ```powershell
   $delegatedPermission = $api.OAuth2Permissions | Where-Object { $_.Value -eq "User.ReadBasic.All" }
   ```

1. Establezca la clasificación de permisos mediante el nombre y el identificador del permiso:

   ```powershell
   Add-AzureADMSServicePrincipalDelegatedPermissionClassification `
      -ServicePrincipalId $api.ObjectId `
      -PermissionId $delegatedPermission.Id `
      -PermissionName $delegatedPermission.Value `
      -Classification "low"
   ```

#### <a name="remove-a-delegated-permission-classification"></a>Opción para quitar una clasificación de permisos delegados

1. Recupere el objeto **ServicePrincipal** para la API. Aquí se recupera el objeto ServicePrincipal para Microsoft Graph API:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Busque la clasificación de permisos delegados que desea quitar:

   ```powershell
   $classifications = Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId
   $classificationToRemove = $classifications | Where-Object {$_.PermissionName -eq "User.ReadBasic.All"}
   ```

1. Elimine la clasificación de permisos:

   ```powershell
   Remove-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId `
       -Id $classificationToRemove.Id
   ```

---

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información:

- Vaya a [Permisos y consentimiento en Plataforma de identidad de Microsoft](../develop/v2-permissions-and-consent.md)
