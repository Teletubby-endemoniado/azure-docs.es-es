---
title: 'Plantillas de ejemplo de Azure Resource Manager: Event Grid | Microsoft Docs'
description: En este artículo se proporciona una lista de ejemplos de plantillas de Azure Resource Manager para Azure Event Grid en GitHub.
ms.topic: sample
ms.date: 09/28/2021
ms.openlocfilehash: 3c2188e99e62a1db3f71b4064a09535bc24c0c68
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129210834"
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Plantillas de Azure Resource Manager para Event Grid

Para la sintaxis y las propiedades de JSON que se usan en una plantilla, consulte [Tipos de recursos de Microsoft.EventGrid](/azure/templates/microsoft.eventgrid/allversions). En la tabla siguiente se incluyen vínculos a plantillas de Azure Resource Manager para Event Grid.

## <a name="event-grid-subscriptions"></a>Suscripciones de Event Grid
- [Tema personalizado y suscripción con el punto de conexión de WebHook](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.eventgrid/event-grid): implementa un tema personalizado de Event Grid. Crea una suscripción a ese tema personalizado que usa un punto de conexión de WebHook. 
- [Suscripción a un tema personalizado con el punto de conexión de EventHub](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.eventgrid/event-grid-event-hubs-handler): crea una suscripción de Event Grid a un tema personalizado. La suscripción usa un centro de eventos para el punto de conexión. 
- [Suscripción a Azure o suscripción al grupo de recursos](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.eventgrid/event-grid-resource-events-to-webhook): se suscribe a los eventos para una suscripción al grupo de recursos o a Azure. El grupo de recursos que especifique como destino durante la implementación es el origen de los eventos. La suscripción usa Webhook para el punto de conexión. 
- [Cuenta y suscripción de Blob Storage](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.eventgrid/event-grid-subscription-and-storage): implementa una cuenta de Azure Blob Storage y crea una suscripción a eventos de dicha cuenta. 

## <a name="next-steps"></a>Pasos siguientes
Consulte los ejemplos siguientes:

- [Ejemplos de PowerShell](powershell-samples.md)
- [Ejemplos de CLI](cli-samples.md)
