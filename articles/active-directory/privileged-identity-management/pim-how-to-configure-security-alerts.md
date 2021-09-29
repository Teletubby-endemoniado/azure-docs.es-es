---
title: Alertas de seguridad para roles de Azure AD en PIM-Azure AD | Microsoft Docs
description: Configure alertas de seguridad para los roles de Azure AD Privileged Identity Management en Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 06/30/2021
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 761995f33d5688e5864640a0e6e2f864f5aa44a3
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124766053"
---
# <a name="configure-security-alerts-for-azure-ad-roles-in-privileged-identity-management"></a>Configurar alertas de seguridad para roles de Azure AD en Privileged Identity Management

Privileged Identity Management (PIM) genera alertas cuando existen actividades sospechosas o no seguras en su entorno de organización Azure Active Directory (Azure AD). Cuando se desencadena una alerta, se muestra en el panel de Privileged Identity Management. Seleccione la alerta para ver un informe en el que se enumeren los usuarios o roles que activaron la alerta.

![Captura de pantalla que muestra la página "Alertas" con una lista de alertas y su gravedad.](./media/pim-how-to-configure-security-alerts/view-alerts.png)

## <a name="security-alerts"></a>Alertas de seguridad

En esta sección se enumeran todas las alertas de seguridad para roles de Azure AD, y cómo resolverlas y evitarlas. La gravedad tiene el significado siguiente:

- **Alta**: requiere acción inmediata debido a la infracción de una directiva.
- **Media**: no requiere acción inmediata, pero indica una posible infracción de una directiva.
- **Baja**: no requiere acción inmediata, pero sugiere un cambio de directiva preferible.

### <a name="administrators-arent-using-their-privileged-roles"></a>Los administradores no están usando sus roles con privilegios

Gravedad: **Baja**

| | Descripción |
| --- | --- |
| **¿Por qué se recibe esta alerta?** | El hecho de que los usuarios tengan asignados roles con privilegios que no necesitan aumenta la probabilidad de un ataque. También es más fácil para los atacantes permanecer desapercibidos en cuentas que no se usan activamente. |
| **Solución** | Revise los usuarios de la lista y quítelos de los roles con privilegios que no necesitan. |
| **Prevención** | Asigne roles con privilegios solo a los usuarios que tengan una justificación empresarial. </br>Programe [revisiones de acceso](./pim-create-azure-ad-roles-and-resource-roles-review.md) periódicas para comprobar que los usuarios todavía necesitan el acceso. |
| **Acción de mitigación en el portal** | Quítele a la cuenta su rol con privilegios. |
| **Desencadenador** | Se desencadena si un usuario pasa por un número especificado de días sin activar un rol. |
| **Número de días** | Esta configuración especifica el número máximo de días, de 0 a 100, que un usuario puede pasar sin activar un rol.|

### <a name="roles-dont-require-multi-factor-authentication-for-activation"></a>No se necesita la autenticación multifactor para la activación de los roles

Gravedad: **Baja**

| | Descripción |
| --- | --- |
| **¿Por qué se recibe esta alerta?** | Sin autenticación multifactor, los usuarios en peligro pueden activar roles con privilegios. |
| **Solución** | Revise la lista de roles y [pida autenticación multifactor](pim-how-to-change-default-settings.md) para cada rol. |
| **Prevención** | [Exija MFA](pim-how-to-change-default-settings.md) para cada rol.  |
| **Acción de mitigación en el portal** | Hace que la autenticación multifactor sea necesaria para la activación del rol con privilegios. |

### <a name="the-organization-doesnt-have-azure-ad-premium-p2"></a>La organización no tiene Azure AD Premium P2

Gravedad: **Baja**

| | Descripción |
| --- | --- |
| **¿Por qué se recibe esta alerta?** | La organización de Azure AD actual no tiene Azure AD Premium P2. |
| **Solución** | Revise la información sobre las [ediciones de Azure AD](../fundamentals/active-directory-whatis.md). Actualice a Azure AD Premium P2. |

### <a name="potential-stale-accounts-in-a-privileged-role"></a>Posibles cuentas obsoletas en un rol con privilegios

Gravedad: **Media**

| | Descripción |
| --- | --- |
| **¿Por qué se recibe esta alerta?** | Las cuentas de un rol con privilegios no han cambiado su contraseña en los últimos 90 días. Estas cuentas podrían ser cuentas de servicio o compartidas que no se mantienen y son vulnerables a los atacantes. |
| **Solución** | Revise las cuentas de la lista. Si ya no necesitan acceso, quíteles sus roles con privilegios. |
| **Prevención** | Asegúrese de que las cuentas que se comparten tengan contraseñas seguras que rotan cuando se produce un cambio en los usuarios que conocen la contraseña. </br>Revise con regularidad las cuentas con roles con privilegios mediante [revisiones de acceso](./pim-create-azure-ad-roles-and-resource-roles-review.md) y quite las asignaciones de roles que ya no sean necesarias. |
| **Acción de mitigación en el portal** | Quítele a la cuenta su rol con privilegios. |
| **procedimientos recomendados** | Las cuentas de acceso compartido, de servicio y de emergencia que se autentican mediante una contraseña y que se asignan a roles administrativos con privilegios elevados, como administrador global o administrador de seguridad, deben rotar sus contraseñas para los siguientes casos:<ul><li>Después de un incidente de seguridad que implique usos indebidos o riesgos de los derechos de acceso administrativos.</li><li>Después de cambiar los privilegios del usuario de modo que deje de ser un administrador (por ejemplo, cuando un empleado que era administrador deja el departamento de TI o abandona la organización).</li><li>A intervalos regulares (por ejemplo, trimestral o anualmente), incluso si no se ha producido ninguna infracción conocida o cambio en el personal de TI.</li></ul>Dado que varias personas tienen acceso a las credenciales de estas cuentas, se deben girar las credenciales para garantizar que las personas que dejan sus roles no puedan seguir accediendo a las cuentas. [Más información sobre la protección de cuentas](../roles/security-planning.md) |

### <a name="roles-are-being-assigned-outside-of-privileged-identity-management"></a>Los roles se están asignando fuera de Privileged Identity Management

Gravedad: **Alta**

| | Descripción |
| --- | --- |
| **¿Por qué se recibe esta alerta?** | Las asignaciones de roles con privilegios realizadas fuera de Privileged Identity Management no se supervisan correctamente y pueden indicar un ataque activo. |
| **Solución** | Revise los usuarios de la lista y quítelos de los roles con privilegios asignados fuera de Privileged Identity Management. También puede habilitar o deshabilitar la alerta y su notificación por correo electrónico que la acompaña en la configuración de alertas. |
| **Prevención** | Investigue a qué usuarios se les asignan roles con privilegios fuera de Privileged Identity Management y que prohíben las asignaciones futuras desde allí. |
| **Acción de mitigación en el portal** | Quita el usuario de su rol con privilegios. |

### <a name="there-are-too-many-global-administrators"></a>Hay demasiados administradores globales

Gravedad: **Baja**

| | Descripción |
| --- | --- |
| **¿Por qué se recibe esta alerta?** | El administrador global es el rol con más privilegios. Si un administrador global está en peligro, el atacante obtiene acceso a todos sus permisos, lo que pone en riesgo todo el sistema. |
| **Solución** | Revise los usuarios de la lista y quite los que no necesite para nada el rol de administrador global. </br>En su lugar, asigne roles con privilegios inferiores a estos usuarios. |
| **Prevención** | Asigne a los usuarios el rol con privilegios mínimos que necesiten. |
| **Acción de mitigación en el portal** | Quítele a la cuenta su rol con privilegios. |
| **Desencadenador** | Se desencadena si se cumplen dos criterios diferentes, y puede configurar ambos. En primer lugar, debe alcanzar un umbral determinado de asignaciones de roles de administradores globales. En segundo lugar, un porcentaje determinado de las asignaciones de roles totales deben ser administradores globales. Si solo se cumple uno de estos criterios, la alerta no aparece. |
| **Número mínimo de administradores globales** | Esta configuración especifica el número de asignaciones de roles de administradores globales, de 2 a 100, que considera que son demasiado pocos para su organización de Azure AD. |
| **Porcentaje de Administradores globales** | Esta configuración especifica el porcentaje mínimo de administradores que son administradores globales, del 0 % al 100 %, por debajo del cual no desea que la organización de Azure AD DIP. |

### <a name="roles-are-being-activated-too-frequently"></a>Se activan roles con demasiada frecuencia

Gravedad: **Baja**

| | Descripción |
| --- | --- |
| **¿Por qué se recibe esta alerta?** | Varias activaciones del mismo rol con privilegios por el mismo usuario es un signo de un ataque. |
| **Solución** | Revise los usuarios de la lista y asegúrese de que la [duración de la activación](pim-how-to-change-default-settings.md) de su rol con privilegios se establezca con tiempo suficiente para realizar sus tareas. |
| **Prevención** | Asegúrese de que la [duración de la activación](pim-how-to-change-default-settings.md) de roles con privilegios se establezca con el tiempo suficiente para que los usuarios realicen sus tareas.</br>[Pida autenticación multifactor](pim-how-to-change-default-settings.md) para roles con privilegios que tengan cuentas compartidas por varios administradores. |
| **Acción de mitigación en el portal** | N/D |
| **Desencadenador** | Se desencadena si un usuario activa el mismo rol con privilegios varias veces dentro de un período especificado. Puede configurar el período de tiempo y el número de activaciones. |
| **Período de tiempo de renovación de activaciones** | Esta opción especifica el período de tiempo, en días, horas, minutos y segundos, que quiere usar para realizar el seguimiento de renovaciones sospechosas. |
| **Number of activation renewals** (Número de renovaciones de activación) | Esta configuración especifica el número de activaciones, de 2 a 100, en las que le gustaría recibir notificaciones, dentro del período de tiempo elegido. Puede cambiar este valor moviendo el control deslizante o escribiendo un número en el cuadro de texto. |

## <a name="customize-security-alert-settings"></a>Personalización de la configuración de alertas de seguridad

En la página **Alertas**, seleccione **Configuración**.

![Página de alertas con la opción Configuración resaltada](media/pim-how-to-configure-security-alerts/alert-settings.png)

Personalice la configuración de las diferentes alertas para que encajen con su entorno y con sus objetivos de seguridad.

![Página de configuración de una alerta para habilitar y configurar los valores](media/pim-how-to-configure-security-alerts/security-alert-settings.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración del rol de Azure AD en Privileged Identity Management](pim-how-to-change-default-settings.md)