---
title: Ventana de mantenimiento
description: Aprenda a configurar la ventana de mantenimiento de Azure SQL Database y Managed Instance.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service-overview
ms.topic: conceptual
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: mathoma
ms.custom: references_regions
ms.date: 10/15/2021
ms.openlocfilehash: 123ea592e46c270830bccef1dc06f9caa3e8fd6e
ms.sourcegitcommit: 37cc33d25f2daea40b6158a8a56b08641bca0a43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130071898"
---
# <a name="maintenance-window-preview"></a>Ventana de mantenimiento (versión preliminar)
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

La característica de ventana de mantenimiento le permite configurar la programación de mantenimiento para los recursos de [Azure SQL Database](sql-database-paas-overview.md) y [Azure SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md) que hacen que los eventos de mantenimiento impactantes sean predecibles y menos disruptivos para la carga de trabajo. 

> [!Note]
> La característica de ventana de mantenimiento solo protege contra el impacto planeado de las actualizaciones o el mantenimiento programado. No protege frente a todas las causas de conmutación por error; las excepciones que pueden provocar interrupciones breves de conexión fuera de una ventana de mantenimiento incluyen errores de hardware, equilibrio de carga de clústeres y reconfiguraciones de bases de datos debido a eventos como un cambio en el objetivo de nivel de servicio de una base de datos. 

## <a name="overview"></a>Información general

Azure realiza periódicamente el [mantenimiento planeado](planned-maintenance.md) de los recursos de SQL Database y SQL Managed Instance. Durante el evento de mantenimiento de Azure SQL, las bases de datos están totalmente disponibles, pero pueden estar sujetas a breves reconfiguraciones dentro de los SLA de disponibilidad respectivos para [SQL Database](https://azure.microsoft.com/support/legal/sla/azure-sql-database) y [SQL Managed Instance](https://azure.microsoft.com/support/legal/sla/azure-sql-sql-managed-instance).

La ventana de mantenimiento está pensada para cargas de trabajo de producción que no son resistentes a las reconfiguraciones de base de datos o de instancia y no pueden absorber interrupciones de conexión breves causadas por eventos de mantenimiento planeado. Al elegir una ventana de mantenimiento preferida, puede minimizar el impacto del mantenimiento planeado, ya que se producirá fuera del horario comercial de máxima actividad. Las cargas de trabajo resistentes y las cargas de trabajo que no son de producción pueden depender de la directiva de mantenimiento predeterminada de Azure SQL.

La ventana de mantenimiento se puede configurar en la creación o para los recursos de Azure SQL existentes. Se puede configurar mediante Azure Portal, PowerShell, la CLI o la API de Azure.

> [!Important]
> La configuración de la ventana de mantenimiento es una operación asincrónica de larga duración, similar a la modificación del nivel de servicio del recurso de Azure SQL. El recurso está disponible durante la operación, excepto una breve reconfiguración que se produce al final de la operación y que normalmente dura hasta 8 segundos, incluso en el caso de transacciones de larga duración interrumpidas. Para minimizar el impacto de la reconfiguración, debe realizar la operación fuera de las horas punta.

### <a name="gain-more-predictability-with-maintenance-window"></a>Ganar en previsibilidad con la ventana de mantenimiento

De forma predeterminada, la directiva de mantenimiento de Azure SQL bloquea las actualizaciones impactantes durante el período de **8.00 a 17.00, hora local, todos los días** para evitar interrupciones durante el horario comercial habitual de máxima actividad. La hora local viene determinada por la ubicación de la [región de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) que hospeda el recurso y puede observar el horario de verano según la definición de la zona horaria local. 

Para ajustar aún más el horario de las actualizaciones de mantenimiento a sus recursos de Azure SQL. Para ello, elija entre otras dos franjas de tiempo de la ventana de mantenimiento:
 
* Ventana de día de la semana: de 22:00 a 6:00 (hora local) de lunes a jueves.
* Ventana de fin de semana: de 22:00 a 6:00 (hora local) de viernes a domingo.

Una vez seleccionada la ventana de mantenimiento y completada la configuración del servicio, el mantenimiento planeado solo se producirá durante la ventana que haya elegido. Aunque los eventos de mantenimiento normalmente se completan dentro de una sola ventana, algunos de ellos pueden abarcar dos o más ventanas adyacentes.   

> [!Important]
> En circunstancias muy poco habituales en las que cualquier aplazamiento de acción podría tener un impacto grave, como aplicar una revisión de seguridad crítica, es posible que la ventana de mantenimiento configurada se invalide temporalmente. 

### <a name="cost-and-eligibility"></a>Costo y elegibilidad

La configuración y el uso de una ventana de mantenimiento no tiene ningún costo para todos los [tipos de oferta](https://azure.microsoft.com/support/legal/offer-details/) elegibles: pago por uso, proveedor de soluciones en la nube (CSP), Contrato Enterprise o Contrato de cliente de Microsoft.

> [!Note]
> Una oferta de Azure es el tipo de la suscripción a Azure que tiene. Algunos ejemplos de ofertas de Azure son una suscripción con [tarifas de pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/), [Azure bajo licencia Open](https://azure.microsoft.com/offers/ms-azr-0111p/) y [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/). Cada uno de los planes u ofertas tienen diferentes términos y ventajas. La oferta o el plan se muestra en la información general de la suscripción. Para obtener más información sobre cómo cambiar su suscripción a una oferta distinta, consulte [Cambio de la suscripción de Azure a una oferta distinta](../../cost-management-billing/manage/switch-azure-offer.md).

## <a name="advance-notifications"></a>Notificaciones anticipadas

Las notificaciones de mantenimiento se pueden configurar para que le avisen de los próximos eventos de mantenimiento planeado con 24 horas de antelación, en el momento del mantenimiento y cuando se complete la ventana de mantenimiento. Para más información, consulte [Notificaciones anticipadas](advance-notifications.md).

## <a name="availability"></a>Disponibilidad

### <a name="supported-service-level-objectives"></a>Objetivos de nivel de servicio admitidos

La elección de una ventana de mantenimiento que no sea la predeterminada está disponible para todos los objetivos de nivel de servicio, **excepto por**:
* Grupos de instancias
* Núcleo virtual Gen4 heredado
* Básico, S0 y S1 
* DC, Fsv2, serie M

### <a name="azure-region-support"></a>Compatibilidad con regiones de Azure

La elección de una ventana de mantenimiento que no sea la predeterminada está disponible actualmente en las siguientes regiones:

| Región de Azure | Instancia administrada de SQL | SQL Database | SQL Database en una [zona de disponibilidad de Azure](high-availability-sla.md) | 
|:---|:---|:---|:---|
| Centro de Australia 1 | Yes | | |
| Centro de Australia 2 | Yes | | |
| Este de Australia | Sí | Sí | Sí |
| Sudeste de Australia | Sí | Sí | |
| Sur de Brasil | Sí | Sí |  |
| Centro de Canadá | Sí | Sí | Sí |
| Este de Canadá | Sí | Sí | |
| India central | Sí | Sí | |
| Centro de EE. UU. | Sí | Sí | Sí |
| Este de China 2 |Sí | Sí ||
| Norte de China 2 |Sí|Sí ||
| Este de EE. UU. | Sí | Sí | Sí |
| Este de EE. UU. 2 | Sí | Sí | Sí |
| Este de Asia | Sí | Sí | |
| Centro de Francia | Sí | Sí | |
| Sur de Francia | Sí | Sí | |
| Centro-oeste de Alemania | Sí | Sí |  |
| Norte de Alemania | Sí |  |  |
| Japón Oriental | Sí | Sí | Sí |
| Japón Occidental | Sí | Sí | |
| Centro de Corea del Sur | Sí | | |
| Corea del Sur | Sí | | |
| Centro-Norte de EE. UU | Sí | Sí | |
| Norte de Europa | Sí | Sí | Sí |
| Norte de Sudáfrica | Sí | | | 
| Oeste de Sudáfrica | Sí | | | 
| Centro-sur de EE. UU. | Sí | Sí | Sí |
| Sur de la India | Sí | Sí | |
| Sudeste de Asia | Sí | Sí | Sí |
| Norte de Suiza | Sí | Sí | |
| Oeste de Suiza | Yes | | |
| Centro de Emiratos Árabes Unidos | Yes | | |
| Norte de Emiratos Árabes Unidos | Sí | | |
| Sur de Reino Unido 2 | Sí | Sí | Sí |
| Oeste de Reino Unido | Sí | Sí | |
| Centro-Oeste de EE. UU. | Sí | Sí | |
| Oeste de Europa | Sí | Sí | Sí |
| Oeste de la India | Sí | | |
| Oeste de EE. UU. | Sí | Sí |  |
| Oeste de EE. UU. 2 | Sí | Sí | Sí |
| | | | | 

## <a name="gateway-maintenance-for-azure-sql-database"></a>Mantenimiento de la puerta de enlace para Azure SQL Database

Para aprovechar al máximo la ventaja de las ventanas de mantenimiento, asegúrese de que las aplicaciones cliente usen la directiva de conexión de redireccionamiento. Redirigir es la directiva de conexión recomendada, mediante la que los clientes establecen conexiones directamente con el nodo que hospeda la base de datos. De este modo, se reduce la latencia y se mejora el rendimiento.  

* En Azure SQL Database, cualquier conexión que use la directiva de conexión de proxy podría verse afectada por la ventana de mantenimiento elegida y por una ventana de mantenimiento del nodo de puerta de enlace. Sin embargo, las conexiones de cliente que usan la directiva de conexión de redireccionamiento recomendada no se ven afectadas por la reconfiguración de mantenimiento del nodo de puerta de enlace. 

* En Azure SQL Managed Instance, los nodos de puerta de enlace se hospedan [en el clúster virtual](../../azure-sql/managed-instance/connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture) y tienen la misma ventana de mantenimiento que la instancia administrada, pero aún se recomienda usar la directiva de conexión de redireccionamiento para minimizar el número de interrupciones durante el evento de mantenimiento.

Para obtener más información sobre la directiva de conexión de cliente en Azure SQL Database consulte la [directiva de conexión de Azure SQL Database](../database/connectivity-architecture.md#connection-policy). 

Para más información sobre la directiva de conexión de cliente en Azure SQL Managed Instance, consulte [Tipos de conexión de Azure SQL Managed Instance](../../azure-sql/managed-instance/connection-types-overview.md).

## <a name="considerations-for-azure-sql-managed-instance"></a>Consideraciones sobre Azure SQL Managed Instance

Azure SQL Managed Instance está formado por componentes de servicio hospedados en un conjunto dedicado de máquinas virtuales aisladas que se ejecutan dentro de la subred de la red virtual del cliente. Estas máquinas virtuales forman [clústeres virtuales](../managed-instance/connectivity-architecture-overview.md#high-level-connectivity-architecture) que pueden hospedar varias instancias administradas. La ventana de mantenimiento configurada en instancias de una subred puede influir en el número de clústeres virtuales dentro de la subred, en la distribución de instancias entre clústeres virtuales y en las operaciones de administración de estos clústeres. Esto puede requerir la consideración de algunos efectos.

### <a name="maintenance-window-configuration-is-long-running-operation"></a>La configuración de la ventana de mantenimiento es una operación de larga duración 
Todas las instancias hospedadas en un clúster virtual comparten la ventana de mantenimiento. De forma predeterminada, todas las instancias administradas se hospedan en el clúster virtual con la ventana de mantenimiento predeterminada. Especificar otra ventana de mantenimiento para la instancia administrada durante su creación o después significa que debe colocarse en un clúster virtual con la ventana de mantenimiento correspondiente. Si no hay ningún clúster virtual en la subred, primero se debe crear uno para acomodar la instancia. Para acomodar una instancia adicional en el clúster virtual existente, es posible que se deba cambiar el tamaño del clúster. Ambas operaciones contribuyen a la duración de la configuración de la ventana de mantenimiento para una instancia administrada.
La duración esperada de la configuración de la ventana de mantenimiento en una instancia administrada se puede calcular mediante la [duración estimada de las operaciones de administración de instancias](../managed-instance/management-operations-overview.md#duration).

> [!Important]
> Se produce una breve reconfiguración al final de la operación de mantenimiento, que suele durar un máximo de 8 segundos, incluso en el caso de transacciones de larga duración interrumpidas. Para minimizar el impacto de la reconfiguración, debe programar la operación fuera de las horas punta.

### <a name="ip-address-space-requirements"></a>Requisitos de espacio de direcciones IP
Cada nuevo clúster virtual de la subred requiere direcciones IP adicionales según la [asignación de la dirección IP del clúster virtual](../managed-instance/vnet-subnet-determine-size.md#determine-subnet-size). El cambio de la ventana de mantenimiento de la instancia administrada existente también requiere una [capacidad de IP adicional temporal](../managed-instance/vnet-subnet-determine-size.md#update-scenarios), como en el escenario de escalado de núcleos virtuales para el nivel de servicio correspondiente.

### <a name="ip-address-change"></a>Cambio de direcciones IP
La configuración y el cambio de la ventana de mantenimiento provoca un cambio de la dirección IP de la instancia, dentro del intervalo de direcciones IP de la subred.

> [!Important]
>  Asegúrese de que el grupo de seguridad de red y las reglas de firewall no bloquearán el tráfico de datos después de cambiar la dirección IP. 

### <a name="serialization-of-virtual-cluster-management-operations"></a>Serialización de operaciones de administración de clústeres virtuales

Las operaciones que afectan a los clústeres virtuales, como las actualizaciones de servicio y el cambio de tamaño de los clústeres (mediante la incorporación de nodos de proceso nuevos o la eliminación de los nodos de proceso innecesarios) se serializan. En otras palabras, una nueva operación de administración de clústeres virtuales no se puede iniciar hasta que se completa la anterior. En caso de que la ventana de mantenimiento se cierre antes de que se complete la operación de actualización o mantenimiento del servicio en curso, cualquier otra operación de administración de clúster virtual enviada mientras tanto se pondrá en espera hasta que se abra la siguiente ventana de mantenimiento y se complete la operación de actualización o mantenimiento del servicio. No es habitual que una operación de mantenimiento lleve más tiempo que el que dura una sola ventana por clúster virtual, pero puede ocurrir en el caso de operaciones de mantenimiento muy complejas.

La serialización de las operaciones de administración de clústeres virtuales es un comportamiento general que también se aplica a la directiva de mantenimiento predeterminada. Con una programación de ventana de mantenimiento configurada, el período entre dos ventanas adyacentes puede ser de unos días. Las operaciones enviadas también pueden estar en espera durante unos días si la operación de mantenimiento abarca dos ventanas. Este es un caso muy poco frecuente, pero la creación de nuevas instancias o el cambio de tamaño de las instancias existentes (si se necesitan nodos de proceso adicionales) se puede bloquear durante este período.

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de ventanas de mantenimiento](maintenance-window-configure.md)
* [Notificaciones anticipadas](advance-notifications.md)

## <a name="learn-more"></a>Más información

* [P+F sobre la ventana de mantenimiento](maintenance-window-faq.yml)
* [Azure SQL Database](sql-database-paas-overview.md) 
* [SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md)
* [Planeación de eventos de mantenimiento de Azure en Azure SQL Database e Instancia administrada de Azure SQL](planned-maintenance.md)
