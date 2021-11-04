---
title: Permisos de Azure Sentinel | Microsoft Docs
description: En este artículo se explica cómo Azure Sentinel usa el control de acceso basado en rol de Azure para asignar permisos a los usuarios e identifica las acciones permitidas para cada rol.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/11/2021
ms.author: yelevin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 2f70dc3f062adb89977e103721b01e78ab638092
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131022975"
---
# <a name="permissions-in-azure-sentinel"></a>Permisos de Azure Sentinel

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Azure Sentinel usa el [control de acceso basado en rol (RBAC)](../role-based-access-control/role-assignments-portal.md) para proporcionar [roles integrados](../role-based-access-control/built-in-roles.md) que se pueden asignar a usuarios, grupos y servicios en Azure.

Utilice RBAC de Azure para crear y asignar roles dentro del equipo de operaciones de seguridad para conceder el acceso adecuado a Azure Sentinel. Los diferentes roles proporcionan un control exhaustivo sobre lo que los usuarios de Azure Sentinel pueden ver y hacer. Los roles de Azure se pueden asignar directamente en el área de trabajo de Azure Sentinel (vea la nota siguiente), o bien en una suscripción o un grupo de recursos al que pertenece el área de trabajo, y que Azure Sentinel heredará.

## <a name="roles-for-working-in-azure-sentinel"></a>Roles para trabajar en Azure Sentinel

### <a name="azure-sentinel-specific-roles"></a>Roles específicos de Azure Sentinel

**Todos los roles integrados de Azure Sentinel conceden acceso de lectura a los datos en el área de trabajo de Azure Sentinel.**

- El rol [Lector de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-reader) puede ver datos, incidentes, libros y otros recursos de Azure Sentinel.

- El rol [Respondedor de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-responder) puede, además de lo anterior, administrar incidentes (asignarlos, descartarlos, etc.).

- El rol [Colaborador de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-contributor) puede, además de lo anterior, crear y editar libros, reglas de análisis y otros recursos de Azure Sentinel.

- [Colaborador de automatización de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-contributor) permite que Azure Sentinel agregue cuadernos de estrategias a las reglas de automatización. No está diseñado para las cuentas de usuario.

> [!NOTE]
>
> - Para obtener los mejores resultados, estos roles deben asignarse en el **grupo de recursos** que contiene el área de trabajo de Azure Sentinel. De esta manera, los roles se aplicarán a todos los recursos implementados para dar soporte a Azure Sentinel, ya que esos recursos también deben colocarse en ese mismo grupo de recursos.
>
> - Otra opción consiste en asignar los roles directamente en la propia **área de trabajo** de Azure Sentinel. Si lo hace, también debe asignar los mismos roles en el **recurso de la solución** de SecurityInsights en esa área de trabajo. También puede que tenga que asignarlos en otros recursos, y deberá administrar constantemente las asignaciones de roles en los recursos.

### <a name="additional-roles-and-permissions"></a>Roles y permisos adicionales

Es posible que sea necesario asignar roles adicionales o permisos específicos a los usuarios con requisitos de trabajo concretos para que puedan realizar sus tareas.

- **Trabajar con cuadernos de estrategias para automatizar respuestas a amenazas**

    Azure Sentinel usa **cuadernos de estrategias** para una respuesta automatizada ante amenazas. Los cuadernos de estrategias se basan en **Azure Logic Apps** y son recursos independientes de Azure. Es posible que desee asignar a miembros específicos del equipo de operaciones de seguridad la posibilidad de usar Logic Apps para las operaciones de orquestación de seguridad, automatización y respuesta (SOAR). Puede usar el rol de [Colaborador de Logic App](../role-based-access-control/built-in-roles.md#logic-app-contributor) para asignar permisos explícitos para el uso de cuadernos de estrategias.

- **Conexión de orígenes de datos a Azure Sentinel**

    Para que un usuario pueda agregar **conectores de datos**, debe asignar permisos de escritura de usuario en el área de trabajo de Azure Sentinel. Además, tenga en cuenta los permisos adicionales necesarios para cada conector, tal como se muestra en la página del conector correspondiente.

- **Los usuarios invitados asignan incidentes**

    Si un usuario invitado necesita poder asignar incidentes, además del rol Respondedor de Azure Sentinel, deberá tener asignado el rol [Lector de directorios](../active-directory/roles/permissions-reference.md#directory-readers). Tenga en cuenta que este *no* es un rol de Azure, sino de **Azure Active Directory** y que los usuarios normales (no invitados) tienen este rol asignado de forma predeterminada.

- **Creación y eliminación de libros**

    Para que un usuario cree y elimine un libro de Azure Sentinel, también deberá tener asignado el rol [Colaborador de supervisión](../role-based-access-control/built-in-roles.md#monitoring-contributor) de Azure Monitor. Este rol no es necesario para *usar* los libros, solo para crearlos y eliminarlos.

### <a name="other-roles-you-might-see-assigned"></a>Otros roles que podría ver asignados

En la asignación de roles de Azure específicos de Azure Sentinel, puede encontrar otros roles de Azure y Log Analytics que se pueden haber asignado a los usuarios para otros fines. Debe tener en cuenta que estos roles conceden un conjunto más amplio de permisos que incluye el acceso al área de trabajo de Azure Sentinel y a otros recursos:

- **Roles de Azure:** [Propietario](../role-based-access-control/built-in-roles.md#owner), [colaborador](../role-based-access-control/built-in-roles.md#contributor) y [lector](../role-based-access-control/built-in-roles.md#reader). Los roles de Azure conceden acceso a todos los recursos de Azure, incluidas las áreas de trabajo de Log Analytics y los recursos de Azure Sentinel.

- **Roles de Log Analytics:** [Colaborador de Log Analytics](../role-based-access-control/built-in-roles.md#log-analytics-contributor) y [Lector de Log Analytics](../role-based-access-control/built-in-roles.md#log-analytics-reader). Los roles de Log Analytics conceden acceso a las áreas de trabajo de Log Analytics.

Por ejemplo, un usuario al que se haya asignado el rol de **Lector de Azure Sentinel**, pero no el rol de **Colaborador de Azure Sentinel**, todavía podrá editar elementos en Azure Sentinel si se le asigna el rol de **Colaborador** de nivel de Azure. Por tanto, si desea conceder permisos solo para un usuario en Azure Sentinel, elimine con cuidado los permisos anteriores de este usuario, y asegúrese de que no interrumpe ningún acceso necesario para otro recurso.

## <a name="azure-sentinel-roles-and-allowed-actions"></a>Roles de Azure Sentinel y acciones permitidas

En la siguiente tabla se resumen los roles de Azure Sentinel y sus acciones permitidas en Azure Sentinel.

| Role | Creación y ejecución de cuadernos de estrategias| Creación y edición de reglas analíticas y otros recursos de Azure Sentinel [*](#workbooks) | Administración de incidentes (descarte, asignación, etc.) | Visualización de datos, incidentes, libros y otros recursos de Azure Sentinel |
|---|---|---|---|---|
| Lector de Azure Sentinel | -- | -- | -- | &#10003; |
| Respondedor de Azure Sentinel | -- | -- | &#10003; | &#10003; |
| Colaborador de Azure Sentinel | -- | &#10003; | &#10003; | &#10003; |
| Colaborador de Azure Sentinel y Colaborador de Logic Apps | &#10003; | &#10003; | &#10003; | &#10003; |
| | | | | |

<a name=workbooks></a>* La creación y eliminación de libros requiere el rol adicional [Colaborador de supervisión](../role-based-access-control/built-in-roles.md#monitoring-contributor). Para más información, vea [Roles y permisos adicionales.](#additional-roles-and-permissions)
## <a name="custom-roles-and-advanced-azure-rbac"></a>Roles personalizados y Azure RBAC avanzado

- **Roles personalizados**. Además de usar roles integrados de Azure (o en vez de ello), puede crear roles personalizados de Azure para Azure Sentinel. Los roles personalizados de Azure para Azure Sentinel se crean de la misma manera que se crean otros [roles personalizados de Azure](../role-based-access-control/custom-roles-rest.md#create-a-custom-role) basados en [permisos específicos para los recursos de Azure Sentinel](../role-based-access-control/resource-provider-operations.md#microsoftsecurityinsights) y [Azure Log Analytics](../role-based-access-control/resource-provider-operations.md#microsoftoperationalinsights).

- **RBAC de Log Analytics**. Puede usar el control de acceso basado en rol de Azure avanzado de Log Analytics en los datos del área de trabajo de Azure Sentinel. Incluye RBAC de Azure basado en el tipo de datos y RBAC de Azure de contexto de recursos. Para más información, consulte:

    - [Administración de los datos de registro y las áreas de trabajo en Azure Monitor](../azure-monitor/logs/manage-access.md#manage-access-using-workspace-permissions)

    - [RBAC de contexto de recursos para Azure Sentinel](resource-context-rbac.md)
    - [RBAC de nivel de tabla](https://techcommunity.microsoft.com/t5/azure-sentinel/table-level-rbac-in-azure-sentinel/ba-p/965043)

    RBAC de contexto de recursos y de nivel de tabla son dos métodos de proporcionar acceso a datos específicos en el área de trabajo de Azure Sentinel sin permitir el acceso a toda la experiencia de Azure Sentinel.

## <a name="role-recommendations"></a>Recomendaciones de roles

Después de comprender cómo funcionan los roles y permisos en Azure Sentinel, puede usar la siguiente guía de procedimientos recomendados para aplicar roles a los usuarios:

|Tipo de usuario  |Rol |Grupo de recursos  |Descripción  |
|---------|---------|---------|---------|
|**Analistas de seguridad**     | [Respondedor de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-responder)        | Grupo de recursos de Azure Sentinel        | Visualización de datos, incidentes, libros y otros recursos de Azure Sentinel. <br><br>Administración de incidentes, como los de asignación o descarte.        |
|     | [Colaborador de Logic Apps](../role-based-access-control/built-in-roles.md#logic-app-contributor)        | Grupo de recursos de Azure Sentinel o grupo de recursos donde se almacenan los cuadernos de estrategias        | Adjunte cuadernos de estrategias a reglas de análisis y automatización, y ejecute cuadernos de estrategias. <br><br>**Nota**: Este rol también permite a los usuarios modificar cuadernos de estrategias.         |
|**Ingenieros de seguridad**     | [Colaborador de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-contributor)       |Grupo de recursos de Azure Sentinel         |   Visualización de datos, incidentes, libros y otros recursos de Azure Sentinel. <br><br>Administración de incidentes, como los de asignación o descarte. <br><br>Creación y edición de libros, reglas de análisis y otros recursos de Azure Sentinel.      |
|     | [Colaborador de Logic Apps](../role-based-access-control/built-in-roles.md#logic-app-contributor)        | Grupo de recursos de Azure Sentinel o grupo de recursos donde se almacenan los cuadernos de estrategias        | Adjunte cuadernos de estrategias a reglas de análisis y automatización, y ejecute cuadernos de estrategias. <br><br>**Nota**: Este rol también permite a los usuarios modificar cuadernos de estrategias.         |
|  **Entidad de seguridad de servicio**   | [Colaborador de Azure Sentinel](../role-based-access-control/built-in-roles.md#azure-sentinel-contributor)      |  Grupo de recursos de Azure Sentinel       | Configuración automatizada de las tareas de administración |
|     |         |        | |

> [!TIP]
> Es posible que se requieran roles adicionales en función de los datos que se ingieren o supervisan. Por ejemplo, se pueden requerir roles de Azure AD, como los de administrador global o administrador de seguridad, para configurar conectores de datos para servicios de otros portales de Microsoft.
>

## <a name="next-steps"></a>Pasos siguientes

En este documento, ha aprendido a trabajar con roles para usuarios de Azure Sentinel y lo que cada rol permite realizar a los usuarios.

Encontrará entradas de blog sobre la seguridad y el cumplimiento de Azure en el [blog de Azure Sentinel](https://aka.ms/azuresentinelblog).
