---
title: 'Supervisión de máquinas virtuales con Azure Monitor: seguridad'
description: Obtenga información sobre los servicios para supervisar la seguridad de las máquinas virtuales y cómo se relacionan con Azure Monitor.
ms.service: azure-monitor
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/21/2021
ms.openlocfilehash: c34a860f7b949a387af8c4905a948e1f47f68057
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132310307"
---
# <a name="monitor-virtual-machines-with-azure-monitor-security-monitoring"></a>Supervisión de máquinas virtuales con Azure Monitor: supervisión de la seguridad
Este artículo forma parte del escenario [Supervisión de máquinas virtuales y sus cargas de trabajo en Azure Monitor](monitor-virtual-machine.md). Se describen los servicios de Azure para supervisar la seguridad de las máquinas virtuales y cómo se relacionan con Azure Monitor. Azure Monitor se ha diseñado para supervisar la disponibilidad y el rendimiento de las máquinas virtuales y otros recursos en la nube. Aunque los datos operativos almacenados en Azure Monitor pueden ser útiles para investigar incidentes de seguridad, se han diseñado otros servicios de Azure para supervisar la seguridad. 

> [!IMPORTANT]
> Los servicios de seguridad tienen su propio costo independiente al de Azure Monitor. Antes de configurar estos servicios, consulte su información de precios para determinar la inversión adecuada en su uso.

## <a name="azure-services-for-security-monitoring"></a>Servicios de Azure para la supervisión de seguridad
Azure Monitor se centra en datos operativos, como los registros de actividad, las métricas y los orígenes admitidos de Log Analytics, incluidos los eventos de Windows (excepto los de seguridad), los contadores de rendimiento, los registros y Syslog. Microsoft Defender para la nube y Microsoft Sentinel realizan la supervisión de seguridad en Azure. Cada uno de estos servicios tiene un costo adicional, por lo que debe determinar su valor para el entorno antes de implementarlos.


## <a name="integration-with-azure-monitor"></a>Integración con Azure Monitor
En la tabla siguiente se enumeran los puntos de integración de Azure Monitor con los servicios de seguridad. Todos los servicios usan el mismo agente de Log Analytics, lo que reduce la complejidad porque no se implementan otros componentes en las máquinas virtuales. Defender para la nube y Microsoft Sentinel almacenan sus datos en un área de trabajo de Log Analytics para que pueda usar consultas de registro a fin de poner en correlación los datos recopilados por los distintos servicios. O bien, puede crear un libro personalizado que combine datos de seguridad y datos de disponibilidad y rendimiento en una sola vista.

| Punto de integración       | Azure Monitor | Microsoft Defender for Cloud | Microsoft Sentinel | Defender para punto de conexión |
|:---|:---|:---|:---|:---|
| Recopila eventos de seguridad     |   | X | X | X |
| Almacena los datos en el área de trabajo de Log Analytics | X | X | X |   | 
| Usa el agente de Log Analytics     | X | X | X | X | 



## <a name="agent-deployment"></a>Implementación de agentes
Puede configurar Defender para la nube para que implemente de forma automática el agente de Log Analytics en máquinas virtuales de Azure. Aunque esto podría parecer redundante con la implementación del mismo agente por parte de Azure Monitor, lo más probable es que quiera habilitar los dos, ya que cada uno de ellos realizará su propia configuración. Por ejemplo, si Defender para la nube intenta aprovisionar una máquina que ya supervisa Azure Monitor, usará el agente que ya está instalado y agregará la configuración para el área de trabajo de Defender para la nube.

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión de máquinas virtuales con Azure Monitor: análisis de los datos de supervisión](monitor-virtual-machine-analyze.md)
* [Supervisión de máquinas virtuales con Azure Monitor: alertas](monitor-virtual-machine-alerts.md)
