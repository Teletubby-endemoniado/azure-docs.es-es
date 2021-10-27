---
title: Archivo de inclusión
description: archivo de inclusión
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 08/30/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 64ce44dc96571447c8d6baa4afd4b60ed4c68932
ms.sourcegitcommit: 5361d9fe40d5c00f19409649e5e8fed660ba4800
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2021
ms.locfileid: "130143902"
---
Event Hubs organiza las secuencias de eventos que se envían a un centro de eventos en una o más particiones. A medida que llegan eventos más recientes, se agregan al final de esta secuencia. 

![Event Hubs](./media/event-hubs-partitions/multiple-partitions.png)

Una partición puede considerarse como un "registro de confirmación". Las particiones almacenan datos de eventos que contienen el cuerpo del evento, un contenedor de propiedades definido por el usuario que describe el evento y metadatos, como su desplazamiento en la partición, su número en la secuencia de la transmisión y la marca de tiempo del servicio en el momento que se aceptó.

![Diagrama que muestra la secuencia de eventos, de más antiguo o más reciente.](./media/event-hubs-partitions/partition.png)

### <a name="advantages-of-using-partitions"></a>Ventajas de usar particiones
Event Hubs está diseñado para ayudar en el procesamiento de grandes volúmenes de eventos y la creación de particiones ayuda a hacerlo de dos maneras:

- Aunque Event Hubs sea un servicio PaaS, hay una realidad física subyacente, y el mantenimiento de un registro que conserva el orden de los eventos requiere que estos eventos se mantengan juntos en el almacenamiento subyacente y sus réplicas, y esto crea un límite máximo de rendimiento para este tipo de registro. La creación de particiones permite que se usen varios registros paralelos para el mismo centro de eventos y, por lo tanto, multiplicar la capacidad de rendimiento de E/S sin procesar disponible.
- Sus propias aplicaciones deben poder mantener el procesamiento del volumen de eventos que se envía a un centro de eventos. Esto puede ser complejo y requiere una capacidad de procesamiento en paralelo importante con escalabilidad horizontal. La capacidad de un único proceso para controlar eventos es limitada, por lo que se necesitan varios procesos. Las particiones son el modo en que la solución alimenta esos procesos y, además, garantiza que cada evento tenga un propietario del proceso claro. 

### <a name="number-of-partitions"></a>Número de particiones
El número de particiones se especifica en el momento de crear un centro de eventos. Debe tener entre 1 y el número máximo de particiones admitido para cada plan de tarifa. Para ver el límite del número de particiones para cada plan, consulte [este artículo](../event-hubs-quotas.md#basic-vs-standard-vs-premium-vs-dedicated-tiers). 

Se recomienda elegir al menos tantas particiones como espere necesitar durante la carga máxima de la aplicación para ese centro de eventos específico. No se puede cambiar el número de particiones de un centro de eventos después de su creación, excepto en el caso del centro de eventos de un clúster dedicado y el nivel prémium. El número de particiones de un centro de eventos de un [clúster de Event Hubs dedicado](../event-hubs-dedicated-overview.md) se puede [aumentar](../dynamically-add-partitions.md) tras su creación, pero la distribución de flujos entre las particiones cambiará cuando se realice como la asignación de claves de partición a cambios de las particiones, por lo que es preciso evitar a toda costa ese tipo de cambios si el orden relativo de los es importante en la aplicación.

Establecer el número de particiones en el valor máximo permitido es tentador, pero siempre debe tener en cuenta que los flujos de eventos deben estar estructurados de manera que se puedan aprovechar las ventajas de tener varias particiones. Si necesita conservar el orden absoluto en todos los eventos o solo en un puñado de subsecuencias, es posible que no pueda aprovechar las ventajas de tener muchas particiones. Además, muchas particiones hacen que el procesamiento sea más complejo. 

No importa cuántas particiones haya en un centro de eventos en lo que respecta a los precios. Depende del número de unidades de precios ([unidades de procesamiento (TU)](../event-hubs-scalability.md#throughput-units) para el nivel estándar, [unidades de procesamiento (PU)](../event-hubs-scalability.md#processing-units) para el nivel prémium y [unidades de capacidad (CU)](../event-hubs-dedicated-overview.md) para el nivel dedicado) para el espacio de nombres o el clúster dedicado. Por ejemplo, un centro de eventos de nivel estándar con 32 particiones o con 1 partición generan exactamente el mismo costo cuando el espacio de nombres se establece en una capacidad de 1 TU. También puede escalar las TU o PU del espacio de nombres o las CU del clúster dedicado independientemente del número de particiones. 

### <a name="mapping-of-events-to-partitions"></a>Asignación de eventos a particiones
Puede usar una clave de partición para asignar datos de eventos entrantes a particiones concretas con fines de organización de los datos. La clave de partición es un valor proporcionado por el remitente que se pasa a un centro de eventos. Se procesa a través de una función hash estática que crea la asignación de la partición. Si no especifica una clave de partición cuando se publica un evento, se usa una asignación de tipo round robin.

El publicador de eventos solo conoce su clave de partición, no la partición en la que se publican los eventos. Este desacoplamiento de la clave y la partición evita al remitente la necesidad de conocer demasiado sobre el procesamiento de bajada. Una identidad única por cada dispositivo o usuario es una buena clave de partición, pero otros atributos como la geografía también pueden usarse para agrupar eventos relacionados en una única partición.

La especificación de una clave de partición permite mantener los eventos relacionados en la misma partición y en el orden exacto en el que han llegado. La clave de partición es una cadena que se deriva del contexto de la aplicación e identifica la interrelación de los eventos. Una secuencia de eventos identificados por una clave de partición es un *flujo*. Una partición es un almacén de registros multiplexado para muchos de estos flujos. 

> [!NOTE]
> Aunque puede enviar eventos directamente a las particiones, no se recomienda, sobre todo si le da una gran importancia a la alta disponibilidad. Esto degrada la disponibilidad de un centro de eventos en el nivel de partición. Para más información, consulte [Disponibilidad y coherencia](../event-hubs-availability-and-consistency.md).

