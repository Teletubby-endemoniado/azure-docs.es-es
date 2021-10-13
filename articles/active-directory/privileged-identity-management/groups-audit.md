---
title: Visualización del informe de auditoría para las asignaciones de grupos de acceso con privilegios en Privileged Identity Management (PIM) - Azure AD | Microsoft Docs
description: Consulte el historial de actividades y auditoría de las asignaciones de grupos de acceso con privilegios en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: KarenH444
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: pim
ms.date: 10/07/2021
ms.author: curtand
ms.reviewer: shaunliu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72a415ba766dd9a1149dfe1089fe96ff2924988b
ms.sourcegitcommit: bee590555f671df96179665ecf9380c624c3a072
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2021
ms.locfileid: "129668112"
---
# <a name="audit-activity-history-for-privileged-access-group-assignments-preview-in-privileged-identity-management"></a>Historial de actividades de auditoría para las asignaciones de grupos de acceso con privilegios (versión preliminar) en Privileged Identity Management

Con Privileged Identity Management (PIM), puede ver las actividades, las activaciones y el historial de auditoría de los miembros y propietarios del grupo de acceso con privilegios de Azure que se encuentra en la organización de Azure Active Directory (Azure AD).

> [!NOTE]
> Si su organización ha externalizado las funciones de administración a un proveedor de servicios que usa [Azure Lighthouse](../../lighthouse/overview.md), las asignaciones de roles autorizadas por ese proveedor de servicios no se mostrarán aquí.

Siga estos pasos para ver el historial de auditoría de los grupos de acceso con privilegios.

## <a name="view-resource-audit-history"></a>Visualización del historial de auditoría de recursos

La **auditoría de recursos** proporciona una vista de todas las actividades asociadas a los grupos de acceso con privilegios.

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Grupos de acceso con privilegios (versión preliminar)** .

1. Seleccione el grupo de acceso con privilegios para el que desea ver el historial de auditorías.

1. En **Actividad**, seleccione **Auditoría de recursos**.

1. Filtre el historial con una fecha predefinida o un intervalo personalizado.

    ![Lista de auditoría de recursos con filtros](media/groups-audit/groups-resource-audit.png)

## <a name="view-my-audit"></a>Visualización de mi auditoría

**Mi auditoría** permite ver la actividad de rol personal para un grupo de acceso con privilegios.

1. Abra **Azure AD Privileged Identity Management**.

1. Seleccione **Grupos de acceso con privilegios (versión preliminar)** .

1. Seleccione el grupo de acceso con privilegios para el que desea ver el historial de auditorías.

1. En **Actividad**, seleccione **Mi auditoría**.

1. Filtre el historial con una fecha predefinida o un intervalo personalizado.

    ![Lista de auditoría para el usuario actual](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de la pertenencia a grupos y al propiedad en Privileged Identity Management](groups-assign-member-owner.md)
- [Aprobación o denegación de solicitudes de grupos de acceso con privilegios en PIM](groups-approval-workflow.md)
- [Visualización del historial de auditorías de Azure AD en PIM](groups-audit.md)
