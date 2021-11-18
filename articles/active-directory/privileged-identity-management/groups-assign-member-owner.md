---
title: 'Asignación de propietarios y miembros elegibles para grupos de acceso con privilegios: Azure Active Directory'
description: Obtenga información sobre cómo asignar propietarios y miembros elegibles de un grupo al que se pueden asignar roles en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: KarenH444
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/09/2021
ms.author: curtand
ms.reviewer: shaunliu
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 62012b910ad1e08291760a2aea0747bf5d4c024d
ms.sourcegitcommit: 512e6048e9c5a8c9648be6cffe1f3482d6895f24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2021
ms.locfileid: "132157587"
---
# <a name="assign-eligibility-for-a-privileged-access-group-preview-in-privileged-identity-management"></a>Asignación de la elegibilidad para un grupo de acceso con privilegios (versión preliminar) en Privileged Identity Management

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) puede ayudarle a administrar la elegibilidad y la activación de las asignaciones a grupos de acceso con privilegios en Azure AD. Puede asignar la elegibilidad a los miembros o propietarios del grupo.

>[!NOTE]
>Todos los usuarios que son elegibles por pertenencia o por propiedad de un grupo de acceso con privilegios deben tener una licencia Premium P2 de Azure AD. Para más información, consulte [Requisitos de licencia para usar Privileged Identity Management](subscription-requirements.md).

## <a name="assign-an-owner-or-member-of-a-group"></a>Asignación de un propietario o un miembro de un grupo

Siga estos pasos para hacer que un usuario sea elegible para ser miembro o propietario de un grupo de acceso con privilegios.

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com/) con un usuario que tenga el rol [Administrador global](../roles/permissions-reference.md#global-administrator), el rol Administrador de roles con privilegios o el rol Propietario del grupo.

1. Seleccione **Grupos** y, a continuación, seleccione el grupo al que se pueden asignar roles que quiera administrar. Puede buscar o filtrar la lista.

    ![buscar un grupo al que se pueden asignar roles para administrarlo en PIM](./media/groups-assign-member-owner/groups-list-in-azure-ad.png)

1. Abra el grupo y seleccione **Acceso con privilegios (versión preliminar)** .

    ![Abrir la experiencia de Privileged Identity Management](./media/groups-assign-member-owner/groups-discover-groups.png)

1. Seleccione **Agregar asignaciones**.

    ![Panel Nueva asignación](./media/groups-assign-member-owner/groups-add-assignment.png)

1. Seleccione los miembros o propietarios que desea que sean elegibles para el grupo de acceso con privilegios.

    ![Captura de pantalla que muestra la página "Agregar asignaciones" con el panel "Seleccionar un miembro o grupo" abierto y el botón "Seleccionar" resaltado.](./media/groups-assign-member-owner/add-assignments.png)

1. Seleccione **Siguiente** para establecer la duración de la pertenencia o la propiedad.

    ![Panel Seleccionar un miembro o grupo](./media/groups-assign-member-owner/assignment-duration.png)

1. En la lista **Tipo de asignación**, seleccione **Apto** o **Activo**. Los grupos de acceso con privilegios proporcionan dos tipos de asignación diferentes:

    - Las asignaciones tipo **Apto** requieren que el miembro del rol realice una acción para usar el rol. Entre las acciones se puede incluir realizar una comprobación de autenticación multifactor (MFA), proporcionar una justificación de negocios o solicitar la aprobación de los aprobadores designados. 

      > [!Important]
      > En el caso de los grupos de acceso con privilegios que se usan para elevar a roles de Azure AD, Microsoft recomienda requerir un proceso de aprobación para las asignaciones de miembros elegibles. Las asignaciones que se pueden activar sin aprobación pueden plantearle un riesgo de seguridad por parte de otro administrador con permiso para restablecer las contraseñas de un usuario apto.

    - Las asignaciones tipo **Activo** no requieren que el miembro realice ninguna acción para usar el rol. Los miembros asignados como activos tienen siempre los privilegios asignados al rol.

1. Si la asignación debe ser permanente (siempre apta o asignada de forma permanente), active la casilla **Permanentemente**. Según la configuración de la organización, es posible que la casilla no aparezca o que no se pueda editar.

1. Cuando termine, seleccione **Asignar**.

1. Para crear la nueva asignación de roles, seleccione **Agregar**. Se muestra una notificación del estado.

    ![Nueva asignación: notificación](./media/groups-assign-member-owner/groups-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Actualizar o quitar una asignación de roles existente

Siga estos pasos para actualizar o quiotar una asignación de roles existente.

1. [Inicie sesión en Azure AD](https://aad.portal.azure.com) con permisos de administrador global o de propietario de grupo.
1. Seleccione **Grupos** y, a continuación, seleccione el grupo al que se pueden asignar roles que quiera administrar. Puede buscar o filtrar la lista.

    ![buscar un grupo al que se pueden asignar roles para administrarlo en PIM](./media/groups-assign-member-owner/groups-list-in-azure-ad.png)

1. Abra el grupo y seleccione **Acceso con privilegios (versión preliminar)** .

    ![Abrir la experiencia de Privileged Identity Management](./media/groups-assign-member-owner/groups-discover-groups.png)

1. Seleccione el rol que quiera actualizar o quitar.

1. Busque la asignación de roles en las pestañas **Roles elegibles** o **Roles activos**.

    ![Actualizar o quitar la asignación de roles](./media/groups-assign-member-owner/groups-bring-under-management.png)

1. Seleccione **Actualizar** o **Quitar** para actualizar o quitar la asignación de roles.

    Para obtener información sobre cómo ampliar una asignación de roles, consulte [Ampliación o renovación de roles de recursos de Azure en Privileged Identity Management](pim-resource-roles-renew-extend.md).

## <a name="next-steps"></a>Pasos siguientes

- [Ampliación o renovación de roles de recursos de Azure en Privileged Identity Management](pim-resource-roles-renew-extend.md)
- [Configuración de las opciones de rol de recursos de Azure en Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
- [Asignación de roles de Azure AD en Privileged Identity Management](pim-how-to-add-role-to-user.md)
