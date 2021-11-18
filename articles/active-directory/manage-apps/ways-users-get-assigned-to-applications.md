---
title: Explicación del proceso de asignación de usuarios a aplicaciones
description: Comprenda cómo se asignan los usuarios a una aplicación que usa Azure Active Directory para la administración de identidades.
titleSuffix: Azure AD
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: reference
ms.date: 01/07/2021
ms.author: davidmu
ms.reviewer: alamaral
ms.openlocfilehash: 2df43ef2a3bc70398d1d73702e1402e439538d15
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132548299"
---
# <a name="understand-how-users-are-assigned-to-apps"></a>Explicación del proceso de asignación de usuarios a aplicaciones

Este artículo le ayudará a comprender cómo se asignan los usuarios a una aplicación del inquilino.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>¿Cómo se asignan los usuarios a una aplicación de Azure AD?

Para que un usuario tenga acceso a una aplicación, se le debe primero asignar a ella de alguna forma. Un administrador, un delegado de la empresa o, en ocasiones, el usuario mismo puede realizar esta asignación. A continuación se describen las maneras en las que se pueden asignar los usuarios a las aplicaciones:

* Un administrador [asigna un usuario](./assign-user-or-group-access-portal.md) a la aplicación directamente
* Un administrador [asigna un grupo](./assign-user-or-group-access-portal.md), del que el usuario forma parte, a la aplicación, incluidos:

  * Un grupo que se ha sincronizado de forma local
  * Un grupo de seguridad estático creado en la nube
  * Un [grupo de seguridad dinámico](../enterprise-users/groups-dynamic-membership.md) creado en la nube
  * Un grupo de Microsoft 365 creado en la nube
  * El grupo [Todos los usuarios](../fundamentals/active-directory-groups-create-azure-portal.md)
* Un administrador habilita [Acceso de autoservicio a las aplicaciones](./manage-self-service-access.md) para permitir que un usuario agregue una aplicación mediante la característica **Agregar aplicación** de la página [Aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510) **sin aprobación de la empresa**.
* Un administrador habilita [Acceso de autoservicio a las aplicaciones](./manage-self-service-access.md) para permitir que un usuario agregue una aplicación mediante la característica **Agregar aplicación** de la página [Mis aplicaciones](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510), pero solo **con la aprobación previa de un conjunto seleccionado de aprobadores de la empresa**
* Un administrador habilita [Administración de grupos de autoservicio](../enterprise-users/groups-self-service-management.md) para permitir que un usuario se una a un grupo al que se ha asignado una aplicación **sin la aprobación de la empresa**
* Un administrador habilita [Administración de grupos de autoservicio](../enterprise-users/groups-self-service-management.md) para permitir que un usuario se una a un grupo al que se ha asignado una aplicación pero solo **con la aprobación anterior de un conjunto seleccionado de aprobadores de la empresa**
* Un administrador asigna una licencia a un usuario directamente para una aplicación original de Microsoft, como [Microsoft 365](https://products.office.com/)
* Un administrador asigna una licencia a un grupo del que el usuario forma parte para una aplicación original de Microsoft, como [Microsoft 365](https://products.office.com/)
* Un [administrador da su consentimiento a una aplicación](../develop/howto-convert-app-to-be-multi-tenant.md) para que la usen todos los usuarios y, a continuación, un usuario inicia sesión en ella
* Un usuario [da su consentimiento a una aplicación](../develop/howto-convert-app-to-be-multi-tenant.md) por sí mismo iniciando sesión en ella directamente

## <a name="next-steps"></a>Pasos siguientes

* [Serie de guías de inicio rápido sobre la administración de aplicaciones](view-applications-portal.md)
* [¿Qué es la administración de aplicaciones?](what-is-application-management.md)
* [¿Qué es el inicio de sesión único?](what-is-single-sign-on.md)