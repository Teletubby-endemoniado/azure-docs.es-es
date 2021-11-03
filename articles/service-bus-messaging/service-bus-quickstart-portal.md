---
title: Uso de Azure Portal para crear una cola de Service Bus
description: En este inicio rápido aprenderá a crear un espacio de nombres de Service Bus y una cola en ese mismo espacio mediante Azure Portal.
author: spelluru
ms.author: spelluru
ms.date: 09/10/2021
ms.topic: quickstart
ms.custom: mode-portal
ms.openlocfilehash: c6726e3717029b427c2cf15a974edf4d4871d588
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131083676"
---
# <a name="use-azure-portal-to-create-a-service-bus-namespace-and-a-queue"></a>Uso de Azure Portal para crear un espacio de nombres de Service Bus y una cola
En este inicio rápido se muestra cómo crear un espacio de nombres de Service Bus y una cola mediante [Azure Portal][Azure portal]. También se muestra cómo obtener credenciales de autorización que una aplicación cliente puede usar para enviar mensajes a la cola, o recibirlos de ella. 

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido, asegúrese de que dispone de una suscripción de Azure. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita][] antes de empezar.

[!INCLUDE [service-bus-create-namespace-portal](./includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](./includes/service-bus-create-queue-portal.md)]

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha creado un espacio de nombres de Service Bus y una cola en dicho espacio de nombres. Para aprender a enviar mensajes a la cola y recibirlos de ella, consulte uno de los siguientes inicios rápido en la sección **Envío y recepción de mensajes**. 

- [.NET](service-bus-dotnet-get-started-with-queues.md)
- [Java](service-bus-java-how-to-use-queues.md)
- [JavaScript](service-bus-nodejs-how-to-use-queues.md)
- [Python](service-bus-python-how-to-use-queues.md)
- [PHP](service-bus-php-how-to-use-queues.md)

[cuenta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Azure portal]: https://portal.azure.com/

[service-bus-flow]: ./media/service-bus-quickstart-portal/service-bus-flow.png
