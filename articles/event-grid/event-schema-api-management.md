---
title: API Management como origen de Event Grid
description: En este artículo se describe cómo usar Azure API Management como origen de eventos de Event Grid. Se proporciona el esquema y vínculos a artículos paso a paso.
ms.topic: conceptual
author: dlepow
ms.author: danlep
ms.date: 07/12/2021
ms.openlocfilehash: e2f56f8886a387158c148edaf9ae557deac3783f
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128659160"
---
# <a name="azure-api-management-as-an-event-grid-source-preview"></a>Azure API Management como origen de Event Grid (versión preliminar)

En este artículo se proporcionan las propiedades y el esquema de los eventos de [Azure API Management](../api-management/index.yml).  Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](./event-schema.md). También se proporcionan vínculos a artículos para usar API Management como origen de eventos.

## <a name="available-event-types"></a>Tipos de eventos disponibles

API Management emite los siguientes tipos de eventos:

| Tipo de evento | Descripción |
| ---------- | ----------- |
| Microsoft.ApiManagement.UserCreated | Se genera cuando se crea un usuario. |
| Microsoft.ApiManagement.UserUpdated | Se genera cuando se actualiza un usuario. |
| Microsoft.ApiManagement.UserDeleted | Se genera cuando se elimina un usuario. |
| Microsoft.ApiManagement.APICreated | Se genera cuando se crea una API. |
| Microsoft.ApiManagement.APIUpdated | Se genera cuando se actualiza una API. |
| Microsoft.ApiManagement.APIDeleted | Se genera cuando se elimina una API. |
| Microsoft.ApiManagement.ProductCreated | Se genera cuando se crea un producto. |
| Microsoft.ApiManagement.ProductUpdated | Se genera cuando se actualiza un producto. |
| Microsoft.ApiManagement.ProductDeleted | Se genera cuando se elimina un producto. |
| Microsoft.ApiManagement.ReleaseCreated | Se genera cuando se crea una versión de API. |
| Microsoft.ApiManagement.ReleaseUpdated | Se genera cuando se actualiza una versión de API. |
| Microsoft.ApiManagement.ReleaseDeleted | Se genera cuando se elimina una versión de API. |
| Microsoft.ApiManagement.SubscriptionCreated | Se genera cuando se crea una suscripción. |
| Microsoft.ApiManagement.SubscriptionUpdated | Se genera cuando se actualiza una suscripción. |
| Microsoft.ApiManagement.SubscriptionDeleted | Se genera cuando se elimina una suscripción. |

## <a name="example-event"></a>Evento de ejemplo

# <a name="event-grid-event-schema"></a>[Esquema de eventos de Event Grid](#tab/event-grid-event-schema)
En el ejemplo siguiente se muestra el esquema de un evento de producto creado. El esquema de otros eventos de recurso creado de API Management es similar.

```json
[{
  "id": "92c502f2-a966-42a7-a428-d3b319844544",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/products/myproduct",
  "data": {
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/products/myproduct"
  },
  "eventType": "Microsoft.ApiManagement.ProductCreated",
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2021-07-02T00:47:47.8536532Z"
}]
```

# <a name="cloud-event-schema"></a>[Esquema de eventos en la nube](#tab/cloud-event-schema)

En el ejemplo siguiente se muestra el esquema de un evento de producto creado. El esquema de otros eventos de recurso creado de API Management es similar. 

```json
[{
  "id": "81dac958-49cf-487e-8805-d0baf0ee485a",
  "source": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/products/myproduct",
  "data": {
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/products/myproduct"
  },
  "Type": "Microsoft.ApiManagement.ProductCreated",
  "Time": "2021-07-02T00:38:44.3978295Z",
  "specversion":"1.0"
}]
```
---

# <a name="event-grid-event-schema"></a>[Esquema de eventos de Event Grid](#tab/event-grid-event-schema)
En el ejemplo siguiente se muestra el esquema de un evento de usuario eliminado. El esquema de otros eventos de recurso eliminado de API Management es similar.

```json
[{
  "id": "92c502f2-a966-42a7-a428-d3b319844544",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/users/apimuser-contoso-com",
  "data": {
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/users/apimuser-contoso-com"
  },
  "eventType": "Microsoft.ApiManagement.UserDeleted",
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2021-07-02T00:47:47.8536532Z"
}]
```

# <a name="cloud-event-schema"></a>[Esquema de eventos en la nube](#tab/cloud-event-schema)

En el ejemplo siguiente se muestra el esquema de un evento de usuario eliminado. El esquema de otros eventos de recurso eliminado de API Management es similar. 

```json
[{
  "id": "81dac958-49cf-487e-8805-d0baf0ee485a",
  "source": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/users/apimuser-contoso-com",
  "data": {
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/users/apimuser-contoso-com"
  },
  "Type": "Microsoft.ApiManagement.UserDeleted",
  "Time": "2021-07-02T00:38:44.3978295Z",
  "specversion":"1.0"
}]
```
---

# <a name="event-grid-event-schema"></a>[Esquema de eventos de Event Grid](#tab/event-grid-event-schema)

En el ejemplo siguiente se muestra el esquema de un evento de API actualizada. El esquema de otros eventos de recurso actualizado de API Management es similar. 
```json
[{
  "id": "95015754-aa51-4eb6-98d9-9ee322b82ad7",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/apis/myapi;Rev=1",
  "data": {
    "resourceUri": "/subscriptions/subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/apis/myapi;Rev=1"
  },
  "eventType": "Microsoft.ApiManagement.APIUpdated",
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2021-07-12T23:13:44.9048323Z"
}]
```

# <a name="cloud-event-schema"></a>[Esquema de eventos en la nube](#tab/cloud-event-schema)

En el ejemplo siguiente se muestra el esquema de un evento de API actualizada. El esquema de otros eventos de recurso actualizado de API Management es similar. 

```json
[{
  "id": "95015754-aa51-4eb6-98d9-9ee322b82ad7",
  "source": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}",
  "subject": "/apis/myapi;Rev=1",
  "data": {
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{your-rg}/providers/Microsoft.ApiManagement/service/{your-APIM-instance}/apis/myapi;Rev=1"
  },
  "Type": "Microsoft.ApiManagement.APIUpdated",
  "Time": "2021-07-12T23:13:44.9048323Z",
  "specversion":1.0
}]
```
---

## <a name="event-properties"></a>Propiedades de evento

# <a name="event-grid-event-schema"></a>[Esquema de eventos de Event Grid](#tab/event-grid-event-schema)

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| `topic` | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| `subject` | string | Ruta al asunto del evento definida por el anunciante. |
| `eventType` | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| `eventTime` | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| `id` | string | Identificador único para el evento |
| `data` | object | Datos de eventos de API Management. |
| `dataVersion` | string | Versión del esquema del objeto de datos. El publicador define la versión del esquema. |
| `metadataVersion` | string | Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor. |

# <a name="cloud-event-schema"></a>[Esquema de eventos en la nube](#tab/cloud-event-schema)

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| `source` | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| `subject` | string | Ruta al asunto del evento definida por el anunciante. |
| `type` | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| `time` | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| `id` | string | Identificador único para el evento |
| `data` | object | Datos de eventos de API Management. |
| `specversion` | string | Versión de especificación del esquema CloudEvents. |

---

El objeto data tiene las siguientes propiedades:

| Propiedad | Tipo | Descripción |
| -------- | ---- | ----------- |
| `resourceUri` | string | Id. completo del recurso para el que se cambia el estado de cumplimiento, incluido el nombre del recurso y el tipo de recurso. Usa el formato `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroup>/Microsoft.ApiManagement/service/<ServiceName>/<ResourceType>/<ResourceName>` |

## <a name="tutorials-and-how-tos"></a>Tutoriales y procedimientos

|Título  |Descripción  |
|---------|---------|
| [Envío de eventos de API Management a Event Grid](../api-management/how-to-event-grid.md)| Procedimiento para suscribirse a eventos de API Management mediante Event Grid. |

## <a name="next-steps"></a>Pasos siguientes

- Para una introducción a Azure Event Grid, consulte [Introducción a Azure Event Grid](./overview.md).
- Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](./subscription-creation-schema.md).


