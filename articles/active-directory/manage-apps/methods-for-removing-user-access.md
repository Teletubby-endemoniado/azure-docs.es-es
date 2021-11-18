---
title: Cómo quitar el acceso de un usuario a una aplicación
description: Aprenda a quitar el acceso de un usuario a una aplicación en Azure Active Directory
titleSuffix: Azure AD
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 11/02/2020
ms.author: davidmu
ms.reviewer: phsignor
ms.openlocfilehash: b0b3e1074df0a1067736900dbce7d7521414c1bc
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132549762"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Cómo quitar el acceso de un usuario a una aplicación

En este artículo se le ayudará a entender cómo quitar el acceso de un usuario a una aplicación.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Deseo quitar la asignación de un usuario o grupo específicos a una aplicación

Para quitar la asignación de un usuario o grupo a una aplicación, siga los pasos indicados en el artículo [Eliminación de asignaciones de usuario o grupo de una aplicación empresarial en Azure Active Directory](./assign-user-or-group-access-portal.md).

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Quiero deshabilitar el acceso a una aplicación para todos los usuarios

Para deshabilitar todos los inicios de sesión de usuarios a una aplicación, siga los pasos indicados en el artículo [Deshabilitación de los inicios de sesión de usuario de una aplicación empresarial en Azure Active Directory](./disable-user-sign-in-portal.md).

## <a name="i-want-to-delete-an-application-entirely"></a>Quiero eliminar una aplicación por completo

La [serie de guías de inicio rápido sobre administración de aplicaciones](delete-application-portal.md) incluye instrucciones sobre cómo eliminar una aplicación del inquilino de Azure Active Directory.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Quiero deshabilitar todas las operaciones de consentimiento de usuario futuras para todas las aplicaciones

Deshabilitar el consentimiento del usuario para todo el directorio impide que los usuarios finales den consentimiento a cualquier aplicación. A pesar de ello, los administradores podrán seguir dando el consentimiento en nombre de los usuarios. Para más información acerca del consentimiento de aplicación y los motivos por los que eso se puede o no puede hacer, lea el artículo [Descripción del consentimiento de usuario y administrador](../develop/howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent). Consulte también [Tipos de permisos y consentimiento](../develop/v2-permissions-and-consent.md)

Para **deshabilitar todas las operaciones de consentimiento de usuario futuras en todo el directorio**, siga estas instrucciones:

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como **administrador global.**

2. Abra la **Extensión de Azure Active Directory**

3. Haga clic en **Aplicaciones empresariales** en el menú de navegación.

4. Haga clic en **Configuración de usuario**.

5. En **Users can allow apps to access company data on their behalf** (Los usuarios pueden permitir a las aplicaciones acceder a los datos de la empresa en su nombre), seleccione **No** y haga clic en el botón Guardar.

## <a name="next-steps"></a>Pasos siguientes

[Administración del acceso a las aplicaciones](what-is-access-management.md)
