---
title: Mejora de la confiabilidad de la aplicación con Advisor
description: Use Azure Advisor para garantizar y mejorar la confiabilidad en las implementaciones de Azure críticas para la empresa.
ms.topic: article
ms.date: 10/26/2021
ms.openlocfilehash: f7ea986424271315843af9557555aa82a708a12c
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131045869"
---
# <a name="improve-the-reliability-of-your-application-by-using-azure-advisor"></a>Mejora de la confiabilidad de la aplicación mediante el uso de Azure Advisor

Azure Advisor lo ayuda a garantizar y mejorar la continuidad de las aplicaciones empresariales críticas. Puede obtener recomendaciones de confiabilidad en la pestaña **Confiabilidad** de [Azure Advisor](https://aka.ms/azureadvisordashboard).

## <a name="check-the-version-of-your-check-point-network-virtual-appliance-image"></a>Compruebe la versión de la imagen de aplicación virtual de la red de Check Point.

Advisor puede identificar si es posible que la máquina virtual esté ejecutando una versión de la imagen de Check Point de la que se sabe que pierde la conectividad de red durante operaciones de mantenimiento de la plataforma. La recomendación de Advisor le ayudará a actualizar a una versión más reciente de la imagen que soluciona esta incidencia. Esta comprobación garantizará la continuidad empresarial a través de una mejor conectividad de red.

## <a name="ensure-application-gateway-fault-tolerance"></a>Garantía de la tolerancia a errores de Application Gateway

Esta recomendación garantiza la continuidad empresarial de las aplicaciones críticas con tecnología de Application Gateway. Advisor identifica las instancias de Application Gateway que no están configuradas para la tolerancia a errores. Después, sugiere las acciones correctivas que puede realizar. Advisor identifica instancias únicas de Application Gateway medianas o grandes y recomienda que agregue al menos una instancia más. También identifica instancias únicas y múltiples de Application Gateway pequeñas y recomienda migrar a SKU medianas o grandes. Advisor recomienda estas acciones para asegurarse de que las instancias de Application Gateway estén configuradas para satisfacer los requisitos actuales del Acuerdo de Nivel de Servicio para estos recursos.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Protección de los datos de máquinas virtuales contra la eliminación accidental

La configuración de la copia de seguridad de máquinas virtuales garantiza la disponibilidad de los datos críticos y ofrece protección contra la eliminación accidental o los daños. Advisor identifica las máquinas virtuales que no tienen la copia de seguridad habilitada y recomienda habilitarla. 

## <a name="ensure-you-have-access-to-azure-experts-when-you-need-it"></a>Asegúrese de tener acceso a los expertos de Azure cuando lo necesite

Cuando se ejecuta una carga de trabajo crítica para la empresa, es importante tener acceso al soporte técnico cuando sea necesario. Advisor identifica las posibles suscripciones críticas para la empresa que no tienen soporte técnico incluido en sus planes de soporte técnico. Recomienda actualizar a una opción que incluye soporte técnico.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-problems-affect-you"></a>Creación de alertas de Azure Service Health para recibir notificaciones cuando se produzcan problemas de Azure que le afecten

Es recomendable configurar alertas de Azure Service Health para recibir notificaciones cuando se produzcan problemas de Azure que le afecten. [Azure Service Health](https://azure.microsoft.com/features/service-health/) es un servicio gratuito que ofrece una guía personalizada y soporte técnico cuando experimenta un problema con algún servicio de Azure. Advisor identifica las suscripciones que no tienen alertas configuradas y recomienda su configuración.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Configuración de puntos de conexión de Traffic Manager para lograr resistencia

Los perfiles de Azure Traffic Manager con más de un punto de conexión experimentan una mayor disponibilidad si se produce un error en cualquier punto de conexión determinado. Colocar puntos de conexión en diferentes regiones mejora aún más la confiabilidad del servicio. Advisor identifica los perfiles de Traffic Manager donde hay solo un punto de conexión y recomienda agregar al menos uno más en otra región.

Si todos los puntos de conexión de un perfil de Traffic Manager configurado para el enrutamiento de proximidad están en la misma región, los usuarios de otras regiones pueden experimentar retrasos en la conexión. Si agrega o mueve un punto de conexión a otra región, mejorará el rendimiento global y proporcionará una mejor disponibilidad en caso de que se produzcan errores en todos los puntos de conexión de una región. Advisor identifica los perfiles de Traffic Manager configurados para el enrutamiento de proximidad en los que todos los puntos de conexión están en la misma región. También recomienda agregar o mover un punto de conexión a otra región de Azure.

Si un perfil de Traffic Manager está configurado para el enrutamiento geográfico, el tráfico se enrutará a puntos de conexión en función de las regiones definidas. Si se produce un error en una región, no hay ninguna conmutación por error predefinida. Tener un punto de conexión en el que la agrupación regional está configurada en **All (World)** evitará que se quite tráfico y mejorará la disponibilidad del servicio. Advisor identifica los perfiles de Traffic Manager configurados para enrutamiento geográfico, en los que no hay ningún punto de conexión que tenga la agrupación regional configurada como **All (World)** . Y recomienda cambiar la configuración para que el punto de conexión tenga esa configuración **Todo (Mundo)** .

## <a name="use-soft-delete-on-your-azure-storage-account-to-save-and-recover-data-after-accidental-overwrite-or-deletion"></a>Uso de la eliminación temporal en la cuenta de Azure Storage para guardar y recuperar datos después de una sobrescritura o eliminación accidentales

Habilite la [eliminación temporal](../storage/blobs/soft-delete-blob-overview.md) en la cuenta de almacenamiento para que los blobs eliminados pasen a un estado de eliminación temporal en lugar de eliminarse de forma definitiva. Cuando se sobrescriben datos, se genera una instantánea de la eliminación temporal para guardar el estado de los datos sobrescritos. El uso de la eliminación temporal le permite efectuar una recuperación en caso de que se produzca una eliminación o sobrescritura accidentales. Advisor identifica las cuentas de Azure Storage que no tienen habilitada la eliminación temporal y sugiere habilitarla.

## <a name="configure-your-vpn-gateway-to-active-active-for-connection-resiliency"></a>Configuración de la puerta de enlace VPN como activo-activo para lograr la resistencia de conexión

En la configuración activo-activo, ambas instancias de una puerta de enlace VPN establecen túneles VPN S2S al dispositivo VPN local. Cuando se produce un evento de mantenimiento planeado o no planeado para una instancia de la puerta de enlace, el tráfico cambiará automáticamente al otro túnel IPsec activo. Azure Advisor identifica las puertas de enlace VPN que no están configuradas como activo-activo y sugiere que las configure para alta disponibilidad.

## <a name="use-production-vpn-gateways-to-run-your-production-workloads"></a>Uso de puertas de enlace VPN de producción para ejecutar las cargas de trabajo de producción

Azure Advisor busca las puertas de enlace VPN que sean una SKU básica y le recomienda que use una SKU de producción en su lugar. La SKU básica se ha diseñado para usarse con fines de desarrollo y pruebas. Ofertas de SKU de producción:
- Más túneles. 
- Compatibilidad con BGP. 
- Opciones de configuración activo-activo. 
- Directiva IPSec/IKE personalizada. 
- Mayor estabilidad y disponibilidad.

## <a name="ensure-reliable-outbound-connectivity-with-vnet-nat"></a>Garantizar una conectividad de salida confiable con NAT de Virtual Network
No se recomienda usar la conectividad de salida predeterminada proporcionada por una instancia de Standard Load Balancer u otros recursos de Azure para cargas de trabajo de producción, ya que esto provoca errores de conexión (también se denomina agotamiento de puertos SNAT). El enfoque recomendado es usar una NAT de Virtual Network que evitará errores de conectividad en este sentido. NAT puede escalar sin problemas para asegurarse de que la aplicación nunca está fuera de los puertos. [Obtenga más información sobre NAT de Virtual Network](../virtual-network/nat-gateway/nat-overview.md).

## <a name="ensure-virtual-machine-fault-tolerance-temporarily-disabled"></a>Garantía de la tolerancia a errores en máquinas virtuales (deshabilitada temporalmente)

Para proporcionar redundancia a la aplicación, recomendamos que agrupe dos máquinas virtuales o más en un conjunto de disponibilidad. Advisor identifica las máquinas virtuales que no forman parte de un conjunto de disponibilidad y recomienda moverlas a uno. Esta configuración garantiza que, durante un evento de mantenimiento planeado o no planeado, al menos una máquina virtual esté disponible y cumpla el Acuerdo de Nivel de Servicio de máquina virtual de Azure. Puede elegir crear un conjunto de disponibilidad de la máquina virtual o agregar la máquina virtual a uno existente.

> [!NOTE]
> Si decide crear un conjunto de disponibilidad, debe agregar al menos una máquina virtual más a él. Se recomienda que agrupe dos o más máquinas virtuales en un conjunto de disponibilidad para garantizar que al menos una de ellas esté disponible durante una interrupción.

## <a name="ensure-availability-set-fault-tolerance-temporarily-disabled"></a>Garantía de la tolerancia a errores del conjunto de disponibilidad (deshabilitado temporalmente)

Para proporcionar redundancia a la aplicación, recomendamos que agrupe dos máquinas virtuales o más en un conjunto de disponibilidad. Advisor identifica conjuntos de disponibilidad que contienen una sola máquina virtual y recomienda que se agregue una o más máquinas virtuales a él.  Esta configuración garantiza que, durante un evento de mantenimiento planeado o no planeado, al menos una máquina virtual esté disponible y cumpla el Acuerdo de Nivel de Servicio de máquina virtual de Azure.  Puede elegir crear una máquina virtual o agregar una existente al conjunto de disponibilidad.  

## <a name="use-managed-disks-to-improve-data-reliability"></a>Uso de discos administrados para mejorar la confiabilidad de los datos

Las máquinas virtuales de un conjunto de disponibilidad con discos que comparten las cuentas de almacenamiento o las unidades de escalado de almacenamiento no son resistentes a errores de unidad de escalado de almacenamiento único durante las interrupciones. Migre a Azure Managed Disks para garantizar que los discos de las distintas máquinas virtuales del conjunto de disponibilidad estén lo suficientemente aislados entre sí como para evitar un único punto de error.

## <a name="repair-invalid-log-alert-rules"></a>Reparación de las reglas de alertas de registro no válidas

Azure Advisor detecta las reglas de alertas de registro que tienen consultas no válidas especificadas en la sección de condiciones. Las reglas de alerta de registro de Azure Monitor ejecutan consultas a la frecuencia especificada y activan alertas en función de los resultados. Las consultas pueden convertirse en no válidas con el paso del tiempo debido a cambios en los comandos, las tablas o los recursos a los que se hace referencia. Advisor recomienda correcciones para las consultas de alertas para impedir que las reglas se deshabiliten automáticamente y para garantizar la cobertura de supervisión. Para obtener más información, vea [Solución de problemas de reglas de alertas](../azure-monitor/alerts/alerts-troubleshoot-log.md#query-used-in-a-log-alert-isnt-valid).

## <a name="configure-consistent-indexing-mode-on-your-azure-cosmos-db-collection"></a>Configuración del modo de indexación coherente en la colección de Azure Cosmos DB

La configuración de los contenedores de Azure Cosmos DB con el modo de indexación diferido pueden afectar a la actualización de los resultados de la consulta. Advisor detecta los contenedores configurados de esta forma y le recomienda que cambie al modo coherente. [Más información sobre las directivas de indexación de Azure Cosmos DB](../cosmos-db/how-to-manage-indexing-policy.md)

## <a name="configure-your-azure-cosmos-db-containers-with-a-partition-key"></a>Configuración de los contenedores de Azure Cosmos DB con una clave de partición

Azure Advisor identifica las colecciones sin particiones de Azure Cosmos DB que se estén aproximando a su cuota de almacenamiento aprovisionado. Recomienda que migre estas colecciones a otras nuevas con una definición de clave de partición para que el servicio pueda escalarlas automáticamente. [Más información sobre la elección de una clave de partición.](../cosmos-db/partitioning-overview.md)

## <a name="upgrade-your-azure-cosmos-db-net-sdk-to-the-latest-version-from-nuget"></a>Actualización del SDK para NET de Azure Cosmos DB a la versión más reciente desde NuGet.

Azure Advisor identifica cuentas de Azure Cosmos DB que usan versiones anteriores del SDK de .NET. Recomienda actualizar a la versión más reciente de NuGet para disfrutar de las correcciones más recientes, mejoras de rendimiento y funcionalidades de características. [Obtenga más información sobre el SDK de .NET de Azure Cosmos DB](../cosmos-db/sql-api-sdk-dotnet-standard.md).

## <a name="upgrade-your-azure-cosmos-db-java-sdk-to-the-latest-version-from-maven"></a>Actualización del SDK para Java de Azure Cosmos DB a la versión más reciente desde Maven

Azure Advisor identifica cuentas de Azure Cosmos DB que usan versiones anteriores del SDK de Java. Recomienda actualizar a la versión más reciente de Maven para disfrutar de las correcciones más recientes, mejoras de rendimiento y funcionalidades de características. [Obtenga más información sobre el SDK de Java de Azure Cosmos DB](../cosmos-db/sql-api-sdk-java-v4.md).

## <a name="upgrade-your-azure-cosmos-db-spark-connector-to-the-latest-version-from-maven"></a>Actualización del conector de Spark de Azure Cosmos DB a la versión más reciente desde Maven

Azure Advisor identifica cuentas de Azure Cosmos DB que usan versiones anteriores del conector de Spark de Azure Cosmos DB. Recomienda actualizar a la versión más reciente de Maven para disfrutar de las correcciones más recientes, mejoras de rendimiento y funcionalidades de características. [Más información sobre el conector de Spark de Azure Cosmos DB.](../cosmos-db/create-sql-api-spark.md)

## <a name="consider-moving-to-kafka-21-on-hdinsight-40"></a>Considere la posibilidad de cambiar a Kafka 2.1 en HDInsight 4.0.

A partir del 1 de julio de 2020, no podrá crear nuevos clústeres de Kafka mediante Kafka 1.1 en Azure HDInsight 4.0. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Considere la posibilidad de pasar a Kafka 2.1 en HDInight 4.0 a partir del 30 de junio de 2020 para evitar la posible interrupción del sistema o del soporte técnico.

## <a name="consider-upgrading-older-spark-versions-in-hdinsight-spark-clusters"></a>Considere la posibilidad de actualizar las versiones anteriores de Spark en clústeres de HDInsight Spark.

A partir del 1 de julio de 2020, no podrá crear nuevos clústeres de Spark con Spark 2.1 o 2.2 en HDInsight 3.6. No podrá crear nuevos clústeres de Spark con Spark 2.3 en HDInsight 4.0. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. 

## <a name="enable-virtual-machine-replication"></a>Habilitar la replicación de máquinas virtuales
Las máquinas virtuales que no tienen habilitada la replicación en otra región no son resistentes a las interrupciones regionales. La replicación de las máquinas virtuales reduce cualquier impacto adverso en el negocio durante el transcurso de una interrupción regional de Azure. Advisor detecta las máquinas virtuales en las que la replicación no está habilitada y recomienda su habilitación. Al habilitar la replicación, si se produce una interrupción, puede abrir rápidamente las máquinas virtuales en una región de Azure remota. [Más información sobre la replicación de máquinas virtuales](../site-recovery/azure-to-azure-quickstart.md).

## <a name="upgrade-to-the-latest-version-of-the-azure-connected-machine-agent"></a>Actualización a la versión más reciente del agente de Azure Connected Machine
El [agente de Azure Connected Machine](../azure-arc/servers/manage-agent.md) se actualiza periódicamente con correcciones de errores, mejoras de estabilidad y nuevas funcionalidades. Hemos identificado recursos que no funcionan en la versión más reciente del agente de equipo y esta recomendación de Advisor le sugerirá que actualice el agente a la versión más reciente para obtener la mejor experiencia de Azure Arc.

## <a name="do-not-override-hostname-to-ensure-website-integrity"></a>No reemplace el nombre de host para garantizar la integridad del sitio web.
Advisor recomienda evitar reemplazar el nombre de host al configurar Application Gateway. Tener un dominio diferente en el front-end de Application Gateway que el que se usa para tener acceso al back-end puede provocar que se interrumpan las cookies o las direcciones URL de redireccionamiento. Tenga en cuenta que esto podría no ser el caso en todas las situaciones y que determinadas categorías de back-end (como las API REST) en general son menos sensibles a esto. Asegúrese de que el back-end puede tratar con esto o actualice la configuración de Application Gateway para que no sea necesario sobrescribir el nombre de host hacia el back-end. Cuando se usa con App Service, adjunte un nombre de dominio personalizado a la aplicación web y evite el uso del nombre de host `*.azurewebsites.net` en el back-end. [Obtenga más información sobre el dominio personalizado](../application-gateway/troubleshoot-app-service-redirection-app-service-url.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las recomendaciones de Advisor, consulte:
* [Introducción a Advisor](advisor-overview.md)
* [Introducción a Advisor](advisor-get-started.md)
* [Puntuación de Advisor](azure-advisor-score.md)
* [Recomendaciones sobre el costo de Advisor](advisor-cost-recommendations.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
* [Recomendaciones de excelencia operativa de Advisor](advisor-operational-excellence-recommendations.md)
