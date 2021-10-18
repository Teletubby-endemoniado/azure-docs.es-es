---
title: Archivo de inclusión
description: archivo de inclusión
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 10/07/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 058ce3899e5857c136859426f0972efc9fd0b92f
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129716006"
---
En la siguiente tabla se muestra la información de cuotas específica de la mensajería de Azure Service Bus. Para obtener información sobre los precios y otras cuotas de Service Bus, vea [Precios de Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

| Nombre de cuota | Ámbito | Valor | Notas | 
| --- | --- | --- | --- |
| Número máximo de espacios de nombres por suscripción de Azure |Espacio de nombres |  1000 (valor predeterminado y máximo) |Se rechazan las solicitudes posteriores de espacios de nombres adicionales. |
| Tamaño de cola o tema |Entidad | <p>1, 2, 3, 4 o 5 GB</p><p>En la SKU Premium y en la SKU Estándar con [particiones](../articles/service-bus-messaging/service-bus-partitioning.md) habilitadas, el tamaño máximo de cola o tema es de 80 GB.</p><p>El límite de tamaño total de un espacio de nombres Premium es de 1 TB por [unidad de mensajería](../articles/service-bus-messaging/service-bus-premium-messaging.md). El tamaño total de todas las entidades de un espacio de nombres no puede superar este límite.</p> | Se define tras la creación o la actualización de la cola o tema. <br/><br/> Los sucesivos mensajes entrantes se rechazan y el código que llama recibe una excepción. |
| Número de conexiones simultáneas en un espacio de nombres |Espacio de nombres |Número neto de mensajes: 1000.<br /><br />AMQP: 5000. | Las solicitudes posteriores de conexiones adicionales se rechazan y el código que llama recibe una excepción. Las operaciones REST no cuentan para las conexiones de TCP simultáneas. |
| Número de solicitudes de recepción simultáneas en una cola, un tema o una entidad de suscripción |Entidad | 5\.000 |Las solicitudes de recepción posteriores se rechazan y el código que llama recibe una excepción. Esta cuota se aplica a un número combinado de operaciones de recepción simultáneas en todas las suscripciones de un tema. |
| Número de temas o colas por espacio de nombres |Espacio de nombres | 10 000 para el nivel Básico o Estándar. El número total de temas y colas de un espacio de nombres debe ser menor o igual a 10 000. <br/><br/>En el nivel Premium, 1000 por unidad de mensajería (MU). | Se rechazan las posteriores solicitudes de creación colas o temas nuevos en el espacio de nombres. Como resultado, si se configuran mediante [Azure Portal][Azure portal], se genera un mensaje de error. Si se realiza una llamada desde la API de administración, el código de llamada recibe una excepción. |
| Número de [temas o colas con particiones](../articles/service-bus-messaging/service-bus-partitioning.md) por espacio de nombres |Espacio de nombres | Niveles Básico y Estándar: 100.<br/><br/>No se admiten entidades con particiones en el nivel [Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md).<br/><br />Cada cola o tema con particiones cuenta para la cuota de 1000 entidades por espacio de nombres. | Se rechazan las posteriores solicitudes de creación colas o temas con particiones nuevos en el espacio de nombres. Como resultado, si se configuran mediante [Azure Portal][Azure portal], se genera un mensaje de error. Si se realiza una llamada desde la API de administración, el código que llama recibe una excepción **QuotaExceededException**. <p>Si desea tener más entidades con particiones en un espacio de nombres de nivel básico o estándar, cree espacios de nombres adicionales. </p>|
| Tamaño máximo de cualquier ruta de entidad de mensajería: cola o tema |Entidad |- |260 caracteres. |
| Tamaño máximo de cualquier nombre de entidad de mensajería: espacio de nombres, suscripción o regla de suscripción |Entidad |- |50 caracteres. |
| Tamaño máximo de un identificador de mensaje | Entidad |- | 128 |
| Tamaño máximo de un identificador de sesión de mensaje | Entidad |- | 128 |
| Tamaño de mensaje de una cola, un tema o una entidad de suscripción |Entidad |Los mensajes entrantes que superan estas cuotas se rechazan y el código que llama recibe una excepción. | 256 KB para el [nivel Estándar](../articles/service-bus-messaging/service-bus-premium-messaging.md)<br/> 1 MB para el [nivel Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md). <br /><br />El tamaño del mensaje incluye el tamaño de las propiedades (sistema y usuario) y el tamaño de la carga útil. El tamaño de las propiedades del sistema varía en función de su escenario. |
| Tamaño de propiedad de mensaje para una cola, un tema o una entidad de suscripción |Entidad | Se genera la excepción `SerializationException`. | <p>El tamaño máximo de propiedad de mensaje para cada propiedad es 32 KB.</p><p>El tamaño acumulado de todas las propiedades no puede superar 64 KB. Este límite se aplica a todo el encabezado del mensaje asincrónico, que contiene tanto las propiedades de usuario como las propiedades del sistema, como el número de secuencia, la etiqueta y el identificador del mensaje.</p><p>Número máximo de propiedades de encabezado en el contenedor de propiedades: **byte/int.MaxValue**.</p> |
| Número de suscripciones por tema |Entidad |Se rechazan las posteriores solicitudes de creación de suscripciones adicionales para el tema. Como resultado, si se configura a través del portal, se muestra un mensaje de error. Si se realiza una llamada desde la API de administración, el código de llamada recibe una excepción. |2000 por tema para el nivel Estándar y el nivel Premium. |
| Número de filtros SQL por tema |Entidad |Se rechazan las posteriores solicitudes de creación de filtros adicionales en el tema y el código que realiza la llamada recibe una excepción. |2\.000 |
| Número de filtros de correlación por tema |Entidad |Se rechazan las posteriores solicitudes de creación de filtros adicionales en el tema y el código que realiza la llamada recibe una excepción. |100 000 |
| Tamaño de filtros o acciones SQL |Espacio de nombres |Se rechazan las posteriores solicitudes de creación de filtros adicionales y el código que realiza la llamada recibe una excepción. |Longitud máxima de la cadena de condición de filtro: 1024 (1 K).<br /><br />Longitud máxima de la cadena de acción de regla: 1024 (1 K).<br /><br />Número máximo de expresiones por acción de regla: 32. |
| Número de reglas de autorización de acceso compartido por espacio de nombres, cola o tema |Entidad, espacio de nombres |Se rechazan las posteriores solicitudes de creación de reglas adicionales en el tema y el código que llama recibe una excepción. |Número máximo de reglas por tipo de entidad: 12. <br /><br /> Las reglas que se configuran en un espacio de nombres de Service Bus se aplican a todos los tipos: colas, temas. |
| Número de mensajes por transacción | Transacción | Los mensajes entrantes adicionales se rechazan y el código que llama recibe una excepción que indica "Can't send more than 100 messages in a single transaction" (No se pueden enviar más de 100 mensajes en una sola transacción). | 100 <br /><br /> Para ambas operaciones **Send()** y **SendAsync()** . |
| Número de reglas de filtro de dirección IP y red virtual | Espacio de nombres | &nbsp; | 128 |

[Azure portal]: https://portal.azure.com
