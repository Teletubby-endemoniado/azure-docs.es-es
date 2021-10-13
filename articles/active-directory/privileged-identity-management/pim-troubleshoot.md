---
title: 'Solución de problemas de acceso denegado a recursos en Privileged Identity Management: Azure Active Directory | Microsoft Docs'
description: Aprenda a solucionar errores del sistema con roles de Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: KarenH444
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 10/07/2021
ms.author: curtand
ms.reviewer: shaunliu
ms.collection: M365-identity-device-management
ms.openlocfilehash: f357c41edf30870207ffde0f7d29ef9aadbfef72
ms.sourcegitcommit: bee590555f671df96179665ecf9380c624c3a072
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2021
ms.locfileid: "129667181"
---
# <a name="troubleshoot-access-to-azure-resources-denied-in-privileged-identity-management"></a>Solución de problemas de acceso denegado a recursos de Azure en Privileged Identity Management

¿Tiene un problema con Privileged Identity Management (PIM) de Azure Active Directory (Azure AD)? La siguiente información puede ayudarle a conseguir que funcione de nuevo.

## <a name="access-to-azure-resources-denied"></a>Denegación de acceso a recursos de Azure

### <a name="problem"></a>Problema

Como administrador de acceso de usuario o propietario activo de un recurso de Azure, puede ver el recurso dentro de Privileged Identity Management pero no realizar acciones tales como crear una asignación válida o ver una lista de asignaciones de roles en la página de información general sobre recursos. Cualquiera de estas acciones produce un error de autorización.

### <a name="cause"></a>Causa

Este problema puede ocurrir cuando el rol de administrador de acceso de usuario de la entidad de servicio de PIM se quitó accidentalmente de la suscripción. Para que el servicio Privileged Identity Management pueda acceder a los recursos de Azure, la entidad de servicio MS-PIM debe tener siempre asignado el [rol de administrador de acceso de usuario](../../role-based-access-control/built-in-roles.md#user-access-administrator) a través de la suscripción de Azure.

### <a name="resolution"></a>Solución

Asigne el rol de administrador de acceso de usuario al nombre de entidad de seguridad de servicio de Privileged Identity Management (MS–PIM) en el nivel de suscripción. Esta asignación debe permitir que el servicio Privileged Identity Management acceda a los recursos de Azure. El rol puede asignarse en el nivel de grupo de administración o el nivel de suscripción, según sus requisitos. Para obtener más información sobre las entidades de servicio, consulte el artículo sobre [asignación de una aplicación a un rol](../develop/howto-create-service-principal-portal.md#assign-a-role-to-the-application).

## <a name="next-steps"></a>Pasos siguientes

- [Requisitos de licencia para usar Privileged Identity Management](subscription-requirements.md)
- [Protección del acceso con privilegios para las implementaciones híbridas y en la nube en Azure AD](../roles/security-planning.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
- [Implementación de Privileged Identity Management](pim-deployment-plan.md)