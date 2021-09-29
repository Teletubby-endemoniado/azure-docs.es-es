---
title: 'Introducción a los centros de eventos dedicados: Azure Event Hubs | Microsoft Docs'
description: En este artículo, se proporciona información general sobre Azure Event Hubs dedicado, que permite realizar implementaciones de centros de eventos con un único inquilino.
ms.topic: article
ms.date: 09/23/2021
ms.openlocfilehash: 696df96c9ac833d937fb89c52842b09ace4c4ec5
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129210730"
---
# <a name="overview-of-event-hubs-dedicated"></a>Introducción a Event Hubs dedicado

Los *clústeres de Event Hubs* disponen de implementaciones con un único inquilino, ideales para aquellos clientes con las necesidades de streaming más exigentes. Esta oferta de inquilino único tiene un acuerdo de nivel de servicio garantizado del 99,99 % y solo está disponible en el plan de tarifa Dedicado. Un clúster de Event Hubs puede incorporar millones de eventos por segundo con capacidad garantizada y una latencia inferior a un segundo. Los espacios de nombres y los centros de eventos creados en el clúster Dedicado incluyen todas las características de la oferta Premium y muchas otras, pero sin imponer límites de entrada. También incluye la conocida característica [Captura de Event Hubs](event-hubs-capture-overview.md) sin costo adicional. Esta característica permite procesar por lotes y registrar flujos de datos automáticamente en Azure Storage o Azure Data Lake. 

Los clústeres se aprovisionan y facturan por **unidades de capacidad (CU)** , una cantidad de recursos de CPU y memoria asignada previamente. Puede adquirir 1, 2, 4, 8, 12, 16 o 20 CU para cada clúster. El grado de ingesta y transmisión por CU depende de diversos factores, como los siguientes: 

- Número de productores y consumidores
- Forma de la carga
- Velocidad de salida

> [!NOTE]
> De forma predeterminada, todos los clústeres de Event Hubs están habilitados para Kafka y admiten puntos de conexión de Kafka que se pueden usar en las aplicaciones existentes basadas en esta plataforma. El hecho de tener habilitado Kafka en el clúster no afectará a otros casos de uso que no estén relacionados con Kafka, por lo que no existe la opción ni la necesidad de deshabilitar Kafka en un clúster.

## <a name="why-dedicated"></a>¿Por qué el plan de tarifa Dedicado?

Event Hubs dedicado brinda tres atractivas ventajas para los clientes que necesitan capacidad de nivel empresarial:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Al haber un único inquilino, se asegura de que hay capacidad suficiente para mejorar el rendimiento.

Un clúster dedicado garantiza la capacidad con el máximo nivel de escalado. Puede admitir hasta gigabytes de datos de streaming de entrada con un almacenamiento de máxima duración y una latencia inferior a un segundo para admitir ráfagas de tráfico de todo tipo. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Acceso incluido y exclusivo a características

La oferta dedicada incluye características, como Capture, sin ningún costo adicional y proporciona acceso exclusivo a características como Bring Your Own Key (BYOK). El servicio también administra el equilibrio de carga, las actualizaciones del sistema operativo, las revisiones de seguridad y la creación de particiones. Por lo tanto, puede dedicar menos tiempo al mantenimiento de la infraestructura y más tiempo a la creación de características del lado cliente.  

## <a name="event-hubs-dedicated-quotas-and-limits"></a>Cuotas y límites de Event Hubs dedicado
La oferta de Event Hubs dedicado se factura aplicando una tarifa mensual fija con un uso mínimo de 4 horas. El nivel Dedicado ofrece todas las características del plan Premium, pero con una capacidad y unos límites de clase empresarial para aquellos clientes que tienen cargas de trabajo muy exigentes. 

Para obtener más información sobre cuotas y límites, consulte [Cuotas y límites de Azure Event Hubs](event-hubs-quotas.md).

## <a name="how-to-onboard"></a>¿Cómo incorporarlo?

La experiencia de autoservicio para [crear un clúster de Event Hubs](event-hubs-dedicated-cluster-create-portal.md) en [Azure Portal](https://aka.ms/eventhubsclusterquickstart) está ahora en versión preliminar. Si tiene alguna pregunta o necesita ayuda para empezar a usar Event Hubs dedicado, póngase en contacto con el [equipo de Event Hubs](mailto:askeventhubs@microsoft.com).

## <a name="faqs"></a>Preguntas más frecuentes

[!INCLUDE [event-hubs-dedicated-clusters-faq](./includes/event-hubs-dedicated-clusters-faq.md)]

## <a name="next-steps"></a>Pasos siguientes

Póngase en contacto con un representante de ventas o con el Soporte técnico de Microsoft para obtener detalles adicionales sobre Event Hubs dedicado. También puede crear un clúster u obtener más información sobre los planes de tarifa de Event Hubs en los vínculos siguientes:

- [Creación de un clúster de Event Hubs con Azure Portal](https://aka.ms/eventhubsclusterquickstart) 
- [Precios de Event Hubs dedicado](https://azure.microsoft.com/pricing/details/event-hubs/). También puede ponerse en contacto con su representante de ventas de Microsoft o el Servicio de soporte técnico de Microsoft para obtener detalles adicionales sobre la capacidad dedicada de Event Hubs.
- En [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.yml) se proporciona información sobre los precios, así como respuestas a algunas preguntas frecuentes acerca de Event Hubs.
