---
title: Configuración de la replicación geográfica para las instancias de Azure Cache for Redis prémium
description: Obtenga información sobre cómo replicar las instancias de Azure Cache for Redis entre regiones de Azure.
author: curib
ms.service: cache
ms.topic: conceptual
ms.date: 02/08/2021
ms.author: cauribeg
ms.openlocfilehash: 989284bd10fc5d452a738d027c693a15f7871b9b
ms.sourcegitcommit: 216b6c593baa354b36b6f20a67b87956d2231c4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2021
ms.locfileid: "129730005"
---
# <a name="configure-geo-replication-for-premium-azure-cache-for-redis-instances"></a>Configuración de la replicación geográfica para las instancias de Azure Cache for Redis prémium

En este artículo, aprenderá a configurar una instancia de Azure Cache con replicación geográfica mediante Azure Portal.

La replicación geográfica une dos instancias de Azure Cache for Redis prémium y crea una relación de replicación de datos. Estas instancias de caché se encuentran normalmente en diferentes regiones de Azure, aunque ello no es necesario. Una instancia actúa como la principal y la otra como la secundario. La principal controla las solicitudes de lectura y escritura y propaga los cambios a la secundaria. Este proceso continúa hasta que se quita el vínculo entre las dos instancias.

> [!NOTE]
> La replicación geográfica está diseñada como una solución de recuperación ante desastres.
>
>

## <a name="geo-replication-prerequisites"></a>Requisitos previos de la replicación geográfica

Para configurar la replicación geográfica entre dos cachés, se deben cumplir los siguientes requisitos previos:

- Ambas deben ser cachés de [nivel Premium](cache-overview.md#service-tiers).
- Ambas cachés deben estar en la misma suscripción de Azure.
- La caché vinculada secundaria debe tener el mismo tamaño de caché o un tamaño de caché mayor que la caché vinculada principal.
- Ambas cachés deben estar creadas y en ejecución.

> [!NOTE]
> La transferencia de datos entre las regiones de Azure se cobrará según las [tarifas de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/) estándar.

Algunas características no son compatibles con la replicación geográfica:

- La redundancia de zona no es compatible con la replicación geográfica.
- La persistencia no es compatible con la replicación geográfica.
- Se admite la agrupación en clústeres si ambas cachés tienen la agrupación en clústeres habilitada y tienen el mismo número de particiones.
- Se admiten las memorias caché red virtual (VNet).
- Las memorias caché de distintas redes virtuales son compatibles con advertencias. Consulte [¿Puedo usar la replicación geográfica con cachés en una VNet?](#can-i-use-geo-replication-with-my-caches-in-a-vnet) para obtener más información.

Una vez que se configura la replicación geográfica, se aplican las siguientes restricciones al par de cachés vinculadas:

- La caché vinculada secundaria es de solo lectura, porque puede leer de ella, pero no puede escribir datos. Si decide leer desde la instancia de replicación geográfica secundaria, es importante tener en cuenta que, cada vez que se produce una sincronización de datos completa entre las instancias principal y secundaria de replicación geográfica (se produce cuando se actualiza alguna de las dos y también en algunos escenarios de reinicio), la instancia secundaria de replicación geográfica generará errores (indicando que hay una sincronización de datos completa en curso) en cualquier operación de Redis que realice en ella hasta que se complete la sincronización de datos completa entre las instancias principal y secundaria de replicación geográfica. Las aplicaciones que leen desde la instancia de replicación geográfica secundaria deben crearse para revertir a la instancia de replicación geográfica principal cada vez que la secundaria genere tales errores.
- Se quitarán los datos existentes en la caché vinculada secundaria antes de la vinculación. Sin embargo, si la replicación geográfica se quita posteriormente, los datos replicados permanecen en la caché vinculada secundaria.
- No puede [escalar](cache-how-to-scale.md) ninguna de las cachés mientras están vinculadas.
- No puede [cambiar el número de particiones](cache-how-to-premium-clustering.md) si la caché tiene la agrupación en clústeres habilitada.
- No puede habilitar la persistencia en ninguna de las cachés.
- Puede [exportar](cache-how-to-import-export-data.md#export) desde cualquier caché.
- No puede [importar](cache-how-to-import-export-data.md#import) en la caché vinculada secundaria.
- No puede eliminar la caché vinculada, ni el grupo de recursos que las contiene, hasta que se desvinculen las cachés. Para más información, vea [¿Por qué no se pudo realizar la operación al intentar eliminar la memoria caché vinculada?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Si las memorias caché se encuentran en regiones diferentes, los costes de salida de red se aplican a los datos que se mueven entre regiones. Para más información, consulte el artículo sobre [cuánto cuesta replicar datos entre regiones de Azure](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions).
- La conmutación automática por error no se produce entre la caché vinculada principal y secundaria. Para obtener más información y conocer cómo realizar la conmutación por error de una aplicación cliente, consulte [¿Cómo funciona la conmutación por error a la caché vinculada secundaria?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Agregar un vínculo de replicación geográfica

1. Para vincular dos cachés para la replicación geográfica, primero haga clic en **Replicación geográfica** en el menú Recursos de la caché que vaya a ser la caché vinculada principal. A continuación, haga clic en **Agregar vínculo de replicación de caché** desde **Replicación geográfica** en la izquierda.

    :::image type="content" source="media/cache-how-to-geo-replication/cache-geo-location-menu.png" alt-text="Menú de replicación geográfica de caché":::

1. Seleccione el nombre de la caché secundaria deseada en la lista **Cachés compatibles**. Si la caché secundaria no aparece en la lista, compruebe que se cumplen los [requisitos previos de replicación geográfica](#geo-replication-prerequisites) para la caché secundaria. Para filtrar las cachés por región, seleccione la región en el mapa para mostrar solo esas cachés en la lista de **cachés disponibles**.

    :::image type="content" source="media/cache-how-to-geo-replication/cache-geo-location-select-link.png" alt-text="Selección de la memoria caché compatible":::

    También puede iniciar el proceso de vinculación o ver detalles sobre la caché secundaria mediante el menú contextual.

    :::image type="content" source="media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png" alt-text="Menú contextual de replicación geográfica":::

1. Seleccione **Vincular** para vincular las dos cachés y comenzar el proceso de replicación.

    :::image type="content" source="media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png" alt-text="Vincular cachés":::

1. Puede ver el progreso del proceso de replicación por medio de **Replicación geográfica** en la parte izquierda.

    :::image type="content" source="media/cache-how-to-geo-replication/cache-geo-location-linking.png" alt-text="Estado de la vinculación":::

    También puede consultar el estado de la vinculación en la parte izquierda, desde **Información general**, para las dos cachés, la principal y la secundaria.

    :::image type="content" source="media/cache-how-to-geo-replication/cache-geo-location-link-status.png" alt-text="Captura de pantalla que destaca cómo ver el estado de vinculación de las memorias caché principales y secundarias.":::

    Una vez que se completa el proceso de replicación, el **estado del vínculo** cambia a **Correcto**.

    :::image type="content" source="media/cache-how-to-geo-replication/cache-geo-location-link-successful.png" alt-text="Estado de cachés":::

    La caché vinculada principal sigue estando disponible para su uso durante el proceso de vinculación. La caché vinculada secundaria no está disponible hasta que se complete el proceso de vinculación.

## <a name="remove-a-geo-replication-link"></a>Quitar un vínculo de replicación geográfica

1. Para quitar el vínculo entre dos cachés y detener la replicación geográfica, haga clic en **Desvincular cachés** desde **Replicación geográfica** en la parte izquierda.

    :::image type="content" source="media/cache-how-to-geo-replication/cache-geo-location-unlink.png" alt-text="Desvincular cachés":::

    Una vez que se completa el proceso de desvinculación, la caché secundaria queda disponible tanto para lecturas como para escrituras.

>[!NOTE]
>Cuando se quita el vínculo de replicación geográfica, los datos replicados de la caché vinculada principal se mantienen en la caché secundaria.
>
>

## <a name="geo-replication-faq"></a>Preguntas más frecuentes sobre la replicación geográfica

- [¿Puedo usar la replicación geográfica con una caché de nivel Estándar o Básico?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [¿La caché está disponible para usarla durante los procesos de vinculación o desvinculación?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [¿Puedo vincular más de dos cachés?](#can-i-link-more-than-two-caches-together)
- [¿Puedo vincular dos cachés de distintas suscripciones de Azure?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [¿Puedo vincular dos cachés de tamaños distintos?](#can-i-link-two-caches-with-different-sizes)
- [¿Puedo usar la replicación geográfica con la agrupación en clústeres habilitada?](#can-i-use-geo-replication-with-clustering-enabled)
- [¿Puedo usar la replicación geográfica con mis memorias caché en una VNet?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [¿Qué es la programación de replicación para la replicación geográfica de Redis?](#what-is-the-replication-schedule-for-redis-geo-replication)
- [¿Cuánto tiempo tarda la replicación geográfica?](#how-long-does-geo-replication-replication-take)
- [¿Se garantiza el punto de recuperación de replicación?](#is-the-replication-recovery-point-guaranteed)
- [¿Puedo usar PowerShell o la CLI de Azure para administrar la replicación geográfica?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [¿Cuánto cuesta replicar datos entre regiones de Azure?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [¿Por qué no se pudo realizar la operación al intentar eliminar la memoria caché vinculada?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [¿Qué región se debe usar para la caché vinculada secundaria?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [¿Cómo funciona la conmutación por error a la caché vinculada secundaria?](#how-does-failing-over-to-the-secondary-linked-cache-work)
- [¿Puedo configurar el firewall con la replicación geográfica?](#can-i-configure-a-firewall-with-geo-replication)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>¿Puedo usar la replicación geográfica con una caché de nivel Estándar o Básico?

No, la replicación geográfica solo está disponible para las cachés de nivel Premium.

### <a name="is-my-cache-available-for-use-during-the-linking-or-unlinking-process"></a>¿La caché está disponible para usarla durante los procesos de vinculación o desvinculación?

- Cuando se realiza el proceso de vinculación, la caché vinculada principal sigue estando disponible mientras se completa dicho proceso.
- En el proceso de vinculación, la caché vinculada secundaria no está disponible hasta que se complete dicho proceso.
- Cuando se realiza el proceso de desvinculación, ambas cachés siguen estando disponibles mientras se completa dicho proceso.

### <a name="can-i-link-more-than-two-caches-together"></a>¿Puedo vincular más de dos cachés?

No, solo puede vincular dos cachés juntas.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>¿Puedo vincular dos cachés de distintas suscripciones de Azure?

No, ambas cachés deben estar en la misma suscripción de Azure.

### <a name="can-i-link-two-caches-with-different-sizes"></a>¿Puedo vincular dos cachés de tamaños distintos?

Sí, siempre que la caché vinculada secundaria sea mayor que la caché vinculada principal.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>¿Puedo usar la replicación geográfica con la agrupación en clústeres habilitada?

Sí, siempre que ambas cachés tengan la misma cantidad de particiones.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>¿Puedo usar la replicación geográfica con mis memorias caché en una VNet?

Sí, la replicación geográfica de memorias caché en redes virtuales es compatible con advertencias:

- Se admite la replicación geográfica entre memorias cachés de la misma VNet.
- También se admite la replicación geográfica entre memorias cachés en distintas redes virtuales.
  - Si las VNet están en la misma región, puede conectarlas mediante un [emparejamiento de la red virtual](../virtual-network/virtual-network-peering-overview.md) o una [conexión de red virtual a red virtual de VPN Gateway](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
  - Si las redes virtuales están en diferentes regiones, se admite la replicación geográfica mediante el emparejamiento de VNet, pero una VM cliente de la red virtual 1 (región 1) no puede acceder a la memoria caché de la red virtual 2 (región 2) mediante su nombre DNS debido a una restricción de los equilibradores de carga internos básicos. Para obtener más información sobre las restricciones de emparejamiento de redes virtuales, consulte [Red virtual - Emparejamiento - Requisitos y restricciones](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints). Se recomienda usar una conexión de red virtual a red virtual de VPN Gateway.
  
Puede usar [esta plantilla de Azure](https://azure.microsoft.com/resources/templates/redis-vnet-geo-replication/) para implementar rápidamente dos cachés con replicación geográfica en una red virtual conectada con una conexión de red virtual a red virtual de VPN Gateway.

### <a name="what-is-the-replication-schedule-for-redis-geo-replication"></a>¿Qué es la programación de replicación para la replicación geográfica de Redis?

La replicación es continua y asincrónica. No ocurre según una programación específica. Todas las escrituras realizadas en el servidor principal se replican asincrónicamente y al instante en el servidor secundario.

### <a name="how-long-does-geo-replication-replication-take"></a>¿Cuánto tiempo tarda la replicación geográfica?

La replicación es incremental, asincrónica y continua, y el tiempo empleado no es muy diferente de la latencia entre regiones. En determinadas circunstancias, la caché secundaria puede requerir la realización de una sincronización completa de los datos del servidor principal. El tiempo de replicación en este caso depende de varios factores, como la carga en la caché principal, el ancho de banda de red disponible y la latencia interregional. Hemos detectado que el tiempo de replicación para un par de replicación geográfica de 53 GB completa puede estar entre 5 y 10 minutos.

### <a name="is-the-replication-recovery-point-guaranteed"></a>¿Se garantiza el punto de recuperación de replicación?

Para las cachés en un modo de replicación geográfica, la persistencia está deshabilitada. Si se ha desvinculado un par de replicación geográfica, como una conmutación por error iniciada por el cliente, la caché vinculada secundaria mantiene sus datos sincronizados hasta ese momento. En esas situaciones no se garantiza ningún punto de recuperación.

Para obtener un punto de recuperación, [exporte](cache-how-to-import-export-data.md#export) desde cualquier caché. Más adelante, puede [importar](cache-how-to-import-export-data.md#import) en la caché vinculada principal.

### <a name="can-i-use-powershell-or-azure-cli-to-manage-geo-replication"></a>¿Puedo usar PowerShell o la CLI de Azure para administrar la replicación geográfica?

Sí, se puede administrar la replicación geográfica mediante Azure Portal, PowerShell o la CLI de Azure. Para obtener más información, consulte los [documentos de PowerShell](/powershell/module/az.rediscache/#redis_cache) o [los documentos de la CLI de Azure](/cli/azure/redis/server-link).

### <a name="how-much-does-it-cost-to-replicate-my-data-across-azure-regions"></a>¿Cuánto cuesta replicar datos entre regiones de Azure?

Cuando usa la replicación geográfica, los datos de la caché vinculada principal se replican a la caché vinculada secundaria. Si las dos cachés vinculadas están en la misma región, no hay ningún cargo por la transferencia de datos. Si las dos cachés vinculadas se encuentran en regiones diferentes, el cargo por transferencia de datos es el coste de salida de red de los datos que se transfieren entre cualquier región. Para más información, consulte [Detalles de precios de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache"></a>¿Por qué no se pudo realizar la operación al intentar eliminar la memoria caché vinculada?

Las cachés con replicación geográfica y sus grupos de recursos no se pueden eliminar si están vinculados hasta que quite el vínculo de replicación geográfica. Si intenta eliminar el grupo de recursos que contiene una o las dos cachés vinculadas, se eliminan los otros recursos del grupo de recursos, pero el grupo de recursos permanece en el estado `deleting` y las cachés en el grupo de recursos permanecen en el estado `running`. Para completar la eliminación del grupo de recursos y las cachés vinculadas dentro de él, desvincule las cachés como se describe en [Quitar un vínculo de replicación geográfica](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>¿Qué región se debe usar para la caché vinculada secundaria?

En general, se recomienda que la caché esté en la misma región de Azure que la aplicación que accede a ella. Si las aplicaciones tienen regiones principales y de reserva, se recomiendan que las cachés principal y secundaria estén en esas mismas regiones. Para más información sobre regiones emparejadas, consulte el artículo sobre [procedimientos recomendados: regiones emparejadas de Azure](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-to-the-secondary-linked-cache-work"></a>¿Cómo funciona la conmutación por error a la caché vinculada secundaria?

No se admite la conmutación automática por error entre regiones de Azure para cachés de replicación geográfica. En un escenario de recuperación ante desastres, los clientes deben mostrar la pila de toda la aplicación de manera coordinada en su región de copia de seguridad. Permitir a los componentes de aplicación individuales cuándo cambiar a sus copias de seguridad por sí mismos puede afectar negativamente al rendimiento. 

Uno de los beneficios clave de Redis es que se trata de un almacén de latencia muy baja. Si la aplicación principal del cliente está en una región distinta a su caché, el tiempo de ida y vuelta agregado tendría un impacto apreciable en el rendimiento. Por este motivo, evitamos que se realice automáticamente la conmutación por error debido a problemas transitorios de disponibilidad.

Para ejecutar una conmutación por error iniciada por el cliente, desvincule primero las cachés. A continuación, cambie el cliente de Redis para utilizar el punto de conexión de la caché secundaria (anteriormente vinculada). Cuando se desvinculan las dos cachés, la réplica secundaria se vuelve a convertir en una caché normal de lectura y escritura, y acepta solicitudes directamente de los clientes de Redis.

### <a name="can-i-configure-a-firewall-with-geo-replication"></a>¿Puedo configurar un firewall con la replicación geográfica?

Sí, puede configurar un [firewall](./cache-configure.md#firewall) con la replicación geográfica. Para que la replicación geográfica funcione junto con un firewall, asegúrese de que la dirección IP de la caché secundaria está agregada a las reglas de firewall de la caché principal.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las características de Azure Cache for Redis.

- [Niveles de servicio de Azure Cache for Redis](cache-overview.md#service-tiers)
- [Alta disponibilidad en Azure Cache for Redis](cache-high-availability.md)
