---
title: Introducción
description: Obtenga información sobre las características y las ventajas de Azure VMware Solution para implementar y administrar las cargas de trabajo basadas en VMware en Azure. El Acuerdo de Nivel de Servicio de Azure VMware Solution garantiza la disponibilidad de las herramientas de administración de Azure VMware (vCenter Server y NSX Manager) al menos el 99,9 % del tiempo.
ms.topic: overview
ms.date: 04/20/2021
ms.openlocfilehash: 10362ef88f572a40771fa6ab3e61b04220c79249
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "122070737"
---
# <a name="what-is-azure-vmware-solution"></a>¿Qué es Azure VMware Solution?

Azure VMware Solution le proporciona nubes privadas que contienen clústeres de vSphere, creados a partir de una infraestructura dedicada de Azure sin sistema operativo. La implementación mínima inicial es de tres hosts, pero se pueden agregar más hosts de uno en uno, hasta un máximo de 16 hosts por clúster. Todas las nubes privadas aprovisionadas tienen vCenter Server, vSAN, vSphere y NSX-T. Como resultado, puede migrar cargas de trabajo de sus entornos locales, implementar nuevas máquinas virtuales y usar servicios de Azure desde sus nubes privadas. Además, las herramientas de administración de Azure VMware Solution (vCenter Server y NSX Manager) están disponibles al menos el 99,9 % del tiempo. Para más información, consulte el [Acuerdo de Nivel de Servicio de Azure VMware Solution](https://aka.ms/avs/sla).

Azure VMware Solution es una solución validada de VMware con validación y pruebas continuas de sus mejoras y actualizaciones. Microsoft administra y mantiene el software y la infraestructura en la nube privada. Esto le permite centrarse en el desarrollo y la ejecución de cargas de trabajo en las nubes privadas.

En este diagrama se muestra la adyacencia entre las nubes privadas y las redes virtuales en Azure, los servicios de Azure y los entornos locales. El acceso a la red desde nubes privadas a servicios o redes virtuales de Azure proporciona una integración controlada mediante Acuerdo de Nivel de Servicio de los puntos de conexión de servicio de Azure. Global Reach de ExpressRoute conecta su entorno local y la nubes privada de Azure VMware Solution.
 

:::image type="content" source="media/adjacency-overview-drawing-final.png" alt-text="Diagrama de la proximidad de la nube privada de Azure VMware Solution a Azure y al entorno local." border="false":::

## <a name="hosts-clusters-and-private-clouds"></a>Hosts, clústeres y nubes privadas

Las nubes privadas y los clústeres de Azure VMware Solution se crean a partir de un host de infraestructura de Azure hiperconvergida completa. Los hosts de gama alta tienen 576 GB de RAM y dos procesadores Intel de 18 núcleos a 2,3 GHz. Además, los hosts de gama alta tienen dos grupos de discos vSAN con un nivel de capacidad de vSAN sin procesar de 15,36 TB (SSD) y un nivel de caché de vSAN de 3,2 TB (NVMe).

Puede implementar nubes privadas nuevas mediante Azure Portal o la CLI de Azure.


## <a name="networking"></a>Redes

[!INCLUDE [avs-networking-description](includes/azure-vmware-solution-networking-description.md)]

Para más información, consulte [Conceptos de redes](concepts-networking.md).

## <a name="access-and-security"></a>Acceso y seguridad

Para mejorar la seguridad, las nubes privadas de Azure VMware Solution usan el control de acceso basado en rol de vSphere. Puede integrar las funcionalidades LDAP del inicio de sesión único de vSphere con Azure Active Directory. Para más información, consulte [Conceptos de acceso y de identidad](concepts-identity.md).  

El cifrado de datos en reposo de vSAN está habilitado de forma predeterminada y se usa para proporcionar seguridad al almacén de datos de vSAN. Para más información, consulte [Conceptos sobre Storage](concepts-storage.md).

## <a name="host-and-software-lifecycle-maintenance"></a>Mantenimiento del ciclo de vida del host y del software

Las actualizaciones periódicas del software de VMware y de la nube privada de Azure VMware Solution garantizan que las nubes privadas disfruten de la seguridad, estabilidad y los conjuntos de características más recientes. Para más información, consulte [Mantenimiento del host y administración del ciclo de vida](concepts-private-clouds-clusters.md#host-maintenance-and-lifecycle-management).

## <a name="monitoring-your-private-cloud"></a>Supervisión de una nube privada

Una vez que se ha implementado Azure VMware Solution en la suscripción, se generan [registros de Azure Monitor](../azure-monitor/overview.md) automáticamente. 

En la nube privada, puede:
- Recopilar registros en cada una de las máquinas virtuales.
- [Descargar e instalar el agente MMA](../azure-monitor/agents/log-analytics-agent.md#installation-options) en máquinas virtuales Linux y Windows.
- Habilite la [extensión de Azure Diagnostics](../azure-monitor/agents/diagnostics-extension-overview.md).
- [Creación y ejecución de nuevas consultas](../azure-monitor/logs/data-platform-logs.md#log-queries).
- Ejecute las mismas consultas que normalmente ejecuta en las máquinas virtuales.

Los patrones de supervisión dentro de Azure VMware Solution son similares a los de Azure Virtual Machines en la plataforma IaaS. Para obtener información adicional y de procedimientos, consulte [Supervisión de máquinas virtuales de Azure con Azure Monitor](../azure-monitor/vm/monitor-vm-azure.md).

## <a name="customer-communication"></a>Comunicación al cliente
[!INCLUDE [customer-communications](includes/customer-communications.md)]

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso es obtener información sobre los [conceptos clave clústeres y nubes privadas](concepts-private-clouds-clusters.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-private-clouds-clusters]: ./concepts-private-clouds-clusters.md

