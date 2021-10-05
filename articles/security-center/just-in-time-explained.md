---
title: Descripción del acceso a la máquina virtual Just-In-Time en Azure Security Center
description: En este documento se explica la forma en que el acceso a la máquina virtual Just-In-Time en Azure Security Center permite controlar el acceso a máquinas virtuales de Azure
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 07/12/2020
ms.author: memildin
ms.openlocfilehash: 8a6fe163ade61df65f6ff0d9ba4f2862866094b7
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128631692"
---
# <a name="understanding-just-in-time-jit-vm-access"></a>Descripción del acceso a la máquina virtual Just-in-Time (JIT)

En esta página se explican los principios en los que se basa la característica de acceso a la máquina virtual Just-in-Time (JIT) de Azure Security Center y la lógica que hay detrás de la recomendación.

Para información sobre cómo aplicar JIT a las máquinas virtuales mediante Azure Portal (ya sea Security Center o Azure Virtual Machines) o mediante programación, consulte [Procedimientos para proteger los puertos de administración con acceso Just-In-Time](security-center-just-in-time.md).


## <a name="the-risk-of-open-management-ports-on-a-virtual-machine"></a>Riesgos de abrir los puertos de administración abiertos en una máquina virtual

Los actores de amenazas buscan activamente máquinas accesibles con puertos de administración abiertos, como RDP o SSH. Todas las máquinas virtuales son objetivos potenciales para un ataque. Cuando se consigue poner en peligro a una máquina virtual, se usa como punto de entrada para atacar más recursos dentro de su entorno.



## <a name="why-jit-vm-access-is-the-solution"></a>¿Por qué el acceso a la máquina virtual JIT es la solución? 

Al igual que con todas las técnicas de prevención de ciberseguridad, el objetivo debería ser reducir la superficie expuesta a ataques. En este caso, eso significa tener menos puertos abiertos, especialmente puertos de administración.

Los usuarios legítimos también usan estos puertos, por lo que no resulta práctico mantenerlos cerrados.

Para solucionar este dilema, Azure Security Center ofrece JIT. Gracias a JIT, se puede bloquear el tráfico entrante a las máquinas virtuales. Para ello, se reduce la exposición a ataques al mismo tiempo que se proporciona un acceso sencillo para conectarse a las máquinas virtuales cuando sea necesario.



## <a name="how-jit-operates-with-network-security-groups-and-azure-firewall"></a>Funcionamiento de JIT con grupos de seguridad de red y Azure Firewall

Cuando se habilita el acceso a la máquina virtual Just-in-Time, se pueden seleccionar los puertos en la máquina virtual en los que se bloqueará el tráfico entrante. Security Center garantiza que existen reglas para "denegar todo el tráfico entrante" de los puertos seleccionados en el [grupo de seguridad de red](../virtual-network/network-security-groups-overview.md#security-rules) (NSG) y las [reglas de firewall de Azure](../firewall/rule-processing.md). Estas reglas restringen el acceso a los puertos de administración de las máquinas virtuales de Azure y los defienden frente a ataques. 

En caso de que ya existan otras reglas relativas a los puertos seleccionados, las reglas existentes tendrán prioridad sobre las nuevas reglas para "denegar todo el tráfico entrante". Si no hay ninguna regla existente en los puertos seleccionados, las nuevas reglas tendrán prioridad principal en los grupos de seguridad de red y Azure Firewall.

Cuando un usuario solicita acceso a una máquina virtual, Security Center comprueba que este tenga permisos de [control de acceso basado en rol (RBAC de Azure)](../role-based-access-control/role-assignments-portal.md) para ella. Si la solicitud se aprueba, Security Center configura los grupos de seguridad de red y Azure Firewall para permitir el tráfico entrante a los puertos seleccionados desde las direcciones (o rangos) IP relevantes durante el periodo especificado. Una vez transcurrido ese tiempo, Security Center restaura los NSG a su estado anterior. Las conexiones que ya están establecidas no se interrumpen.

> [!NOTE]
> JIT no admite las máquinas virtuales que protegen los firewalls de Azure controlados mediante [Azure Firewall Manager](../firewall-manager/overview.md).  Azure Firewall debe configurarse con Reglas (clásico) y no puede usar directivas de firewall.




## <a name="how-security-center-identifies-which-vms-should-have-jit-applied"></a>Cómo identifica Security Center qué máquinas virtuales deben tener JIT aplicado

En el diagrama siguiente se muestra la lógica que Security Center aplica al decidir cómo clasificar las máquinas virtuales compatibles: 

[![Flujo de lógica de máquina virtual (VM) Just-in-Time (JIT)](media/just-in-time-explained/jit-logic-flow.png)](media/just-in-time-explained/jit-logic-flow.png#lightbox).

Cuando Security Center encuentra una máquina que puede beneficiarse de JIT, agrega esa máquina a la pestaña **Recursos incorrectos** de la recomendación. 

![Recomendación de acceso a la máquina virtual (VM) Just-in-Time (JIT).](./media/just-in-time-explained/unhealthy-resources.png)


## <a name="faq---just-in-time-virtual-machine-access"></a>Preguntas frecuentes: Acceso a máquinas virtuales Just-In-Time

### <a name="what-permissions-are-needed-to-configure-and-use-jit"></a>¿Cuáles son los permisos necesarios para configurar y usar Just-In-Time?

JIT necesita que [Azure Defender para servidores](defender-for-servers-introduction.md) esté habilitado en la suscripción. 

Los roles **Lector** y **SecurityReader** pueden ver el estado y los parámetros de JIT.

Si quiere crear roles personalizados que puedan funcionar con JIT, necesita los detalles de la tabla siguiente.

> [!TIP]
> Para crear un rol con privilegios mínimos para los usuarios que necesiten solicitar acceso JIT a una máquina virtual y no realizar ninguna otra operación JIT, use el [script Set-JitLeastPrivilegedRole](https://github.com/Azure/Azure-Security-Center/tree/main/Powershell%20scripts/JIT%20Scripts/JIT%20Custom%20Role) de las páginas de la comunidad de GitHub de Security Center.

| Para permitir a los usuarios: | Permisos que se deben establecer|
| --- | --- |
|Configurar o editar una directiva JIT para una VM | *Asigne estas acciones al rol.*  <ul><li>En el ámbito de una suscripción o un grupo de recursos asociados a la máquina virtual:<br/> `Microsoft.Security/locations/jitNetworkAccessPolicies/write` </li><li> En el ámbito de una suscripción o grupo de recursos de una máquina virtual: <br/>`Microsoft.Compute/virtualMachines/write`</li></ul> | 
|Solicitud de acceso Just-In-Time a una máquina virtual | *Asigne estas acciones al usuario.*  <ul><li>En el ámbito de una suscripción o un grupo de recursos asociados a la máquina virtual:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action` </li><li>En el ámbito de una suscripción o un grupo de recursos asociados a la máquina virtual:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/*/read` </li><li>  En el ámbito de una suscripción, un grupo de recursos o una máquina virtual:<br/> `Microsoft.Compute/virtualMachines/read` </li><li>  En el ámbito de una suscripción, un grupo de recursos o una máquina virtual:<br/> `Microsoft.Network/networkInterfaces/*/read` </li></ul>|
|Leer directivas JIT| *Asigne estas acciones al usuario.*  <ul><li>`Microsoft.Security/locations/jitNetworkAccessPolicies/read`</li><li>`Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action`</li><li>`Microsoft.Security/policies/read`</li><li>`Microsoft.Security/pricings/read`</li><li>`Microsoft.Compute/virtualMachines/read`</li><li>`Microsoft.Network/*/read`</li>|
|||





## <a name="next-steps"></a>Pasos siguientes

En esta página se ha explicado _por qué_ se debe usar el acceso a la máquina virtual (VM) Just-in-Time (JIT). Para más información sobre _cómo_ para habilitar JIT y solicitar acceso a las máquinas virtuales habilitadas para JIT, consulte lo siguiente:

> [!div class="nextstepaction"]
> [Procedimientos para proteger los puertos de administración con acceso Just-in-Time](security-center-just-in-time.md)
