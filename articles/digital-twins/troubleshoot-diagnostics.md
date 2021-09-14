---
title: Habilitación y consulta de registros de diagnóstico
titleSuffix: Azure Digital Twins
description: Vea cómo habilitar el registro con la configuración de diagnóstico y cómo consultar los registros para verlos de forma inmediata.
author: baanders
ms.author: baanders
ms.date: 8/24/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 1479e2b6b715e8f80ea9e02b0b57a3995da2bfd9
ms.sourcegitcommit: 40866facf800a09574f97cc486b5f64fced67eb2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2021
ms.locfileid: "123219705"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logging"></a>Solución de problemas de Azure Digital Twins: Registro de diagnóstico

Azure Digital Twins puede recopilar los registros de una instancia de servicio para supervisar su rendimiento, acceso y otros datos. Estos registros se pueden usar para hacerse una idea de lo que ocurre en una instancia de Azure Digital Twins y analizar las causas principales de los problemas sin necesidad de ponerse en contacto con el soporte técnico de Azure.

En este artículo se muestra cómo [configurar los valores de diagnóstico](#turn-on-diagnostic-settings) en [Azure Portal](https://portal.azure.com) para empezar a recopilar registros de una instancia de Azure Digital Twins. También se puede especificar la ubicación en que se deben almacenar los registros (como Log Analytics o la cuenta de almacenamiento que se prefiera).

Este artículo también contiene una lista de todas las [categorías de registro](#log-categories) y los [esquemas de registro](#log-schemas) que recopila Azure Digital Twins.

Después de configurar los registros, también puede [consultar los registros](#view-and-query-logs) para recopilar rápidamente información personalizada.

## <a name="turn-on-diagnostic-settings"></a>Activación de la configuración de diagnóstico 

Active la configuración de diagnóstico para empezar a recopilar registros en una instancia de Azure Digital Twins. También puede elegir el destino en el que se deben almacenar los registros exportados. Aquí se describe cómo habilitar la configuración de diagnóstico de una instancia de Azure Digital Twins.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la instancia de Azure Digital Twins. Escriba el nombre en la barra de búsqueda para encontrarla. 

2. En el menú, seleccione **Configuración de diagnóstico** y **Agregar configuración de diagnóstico**.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Captura de pantalla que muestra la página de configuración de diagnóstico en Azure Portal y el botón para agregar." lightbox="media/troubleshoot-diagnostics/diagnostic-settings.png":::

3. En la página siguiente, rellene los valores a continuación:
     * **Diagnostic setting name** (Nombre de la configuración de diagnóstico): Asigne un nombre a la configuración de diagnóstico.
     * **Category details** (Detalles de categoría): Elija qué operaciones desea supervisar y marque las casillas para habilitar el diagnóstico para ellas. Las operaciones sobre las que puede informar la configuración de diagnóstico son:
        - DigitalTwinsOperation
        - EventRoutesOperation
        - ModelsOperation
        - QueryOperation
        - AllMetrics
        
        Para más información sobre estas categorías y la información que contienen, consulte la sección [Categorías de registro](#log-categories), que encontrará a continuación.
     * **Detalles de destino**: Elija dónde desea enviar los registros. También puede seleccionar cualquier combinación de las tres opciones:
        - Enviar a Log Analytics
        - Archivar en una cuenta de almacenamiento
        - Transmitir a un centro de eventos

        Si es necesario para el destino seleccionado, se le pedirá que rellene detalles adicionales.  
    
4. Guarde la configuración nueva. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Captura de pantalla que muestra la página de configuración de diagnóstico en Azure Portal en la que el usuario ha rellenado información de configuración de diagnóstico." lightbox="media/troubleshoot-diagnostics/diagnostic-settings-details.png":::

La nueva configuración surte efecto en unos 10 minutos. Después, los registros aparecen en el destino configurado en la página **Configuración de diagnóstico** de la instancia. 

Para obtener información más detallada sobre la configuración de diagnóstico y sus opciones, puede visitar [Creación de una configuración de diagnóstico para enviar los registros y las métricas de la plataforma a diferentes destinos](../azure-monitor/essentials/diagnostic-settings.md).

## <a name="log-categories"></a>Categorías de registro

Aquí encontrará más detalles sobre las categorías de registros que recopila Azure Digital Twins.

| Categoría de registro | Descripción |
| --- | --- |
| ADTModelsOperation | Registra todas las llamadas API relativas a modelos. |
| ADTQueryOperation | Registra todas las llamadas API relativas a consultas. |
| ADTEventRoutesOperation | Registra todas las llamadas API relativas a rutas de eventos, así como la salida de eventos de Azure Digital Twins a un servicio de punto de conexión como Event Grid, Event Hubs y Service Bus. |
| ADTDigitalTwinsOperation | Registra todas las llamadas API que pertenecen a gemelos individuales. |

Cada categoría de registro consta de operaciones de escritura, lectura, eliminación y acción. Estas se asignan a las llamadas API REST de la siguiente manera:

| Tipo de evento | Operaciones de API REST |
| --- | --- |
| Escritura | PUT y PATCH |
| Lectura | GET |
| Eliminar | Delete |
| Acción | POST |

Esta es una lista completa de las operaciones y las [llamadas API REST de Azure Digital Twins](/rest/api/azure-digitaltwins/) correspondientes que se registran en cada categoría. 

>[!NOTE]
> Cada categoría de registro contiene varias operaciones y llamadas API REST. En la tabla siguiente, cada categoría de registro se asigna a todas las operaciones y llamadas API REST que hay debajo hasta que se muestra la siguiente categoría de registro. 

| Categoría de registro | Operación | Llamadas API REST y otros eventos |
| --- | --- | --- |
| ADTModelsOperation | Microsoft.DigitalTwins/models/write | Digital Twin Models Update API |
|  | Microsoft.DigitalTwins/models/read | Digital Twin Models Get By ID API y List API |
|  | Microsoft.DigitalTwins/models/delete | Digital Twin Models Delete API |
|  | Microsoft.DigitalTwins/models/action | Digital Twin Models Add API |
| ADTQueryOperation | Microsoft.DigitalTwins/query/action | Query Twins API |
| ADTEventRoutesOperation | Microsoft.DigitalTwins/eventroutes/write | Event Routes Add API |
|  | Microsoft.DigitalTwins/eventroutes/read | Event Routes Get By ID API y List API |
|  | Microsoft.DigitalTwins/eventroutes/delete | Event Routes Delete API |
|  | Microsoft.DigitalTwins/eventroutes/action | Error al intentar publicar eventos en un servicio de punto de conexión (no una llamada API) |
| ADTDigitalTwinsOperation | Microsoft.DigitalTwins/digitaltwins/write | Agregar, agregar relaciones, actualizar, actualizar componente de Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/read | Obtener por identificador, obtener componente, obtener relación por identificador, enumerar relaciones entrantes, enumerar relaciones de Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/delete | Eliminar, eliminar relación de Digital Twins |
|  | Microsoft.DigitalTwins/digitaltwins/action | Enviar telemetría de componentes, enviar telemetría de Digital Twins |

## <a name="log-schemas"></a>Esquemas de registro 

Cada categoría de registro tiene un esquema que define cómo se notifican los eventos de la categoría de que se trate. Cada entrada de registro individual se almacena como texto y se le da formato como un blob JSON. Los campos del registro y los cuerpos JSON de ejemplo se proporcionan para cada tipo de registro siguiente. 

`ADTDigitalTwinsOperation`, `ADTModelsOperation` y `ADTQueryOperation` usan un esquema de registro de API coherente. `ADTEventRoutesOperation` extiende el esquema para que contenga un campo `endpointName` en las propiedades.

### <a name="api-log-schemas"></a>Esquemas de registro de API

Este esquema de registro es coherente para `ADTDigitalTwinsOperation`, `ADTModelsOperation` y `ADTQueryOperation`. También se usa el mismo esquema para `ADTEventRoutesOperation`, **excepto** el nombre de operación `Microsoft.DigitalTwins/eventroutes/action` (para obtener más información sobre el esquema, consulte la sección siguiente, [Esquemas de registro de salida](#egress-log-schemas)).

El esquema contiene información pertinente para las llamadas a API para una instancia de Azure Digital Twins.

Estas son las descripciones de campo y propiedad de los registros de API.

| Nombre del campo | Tipo de datos | Descripción |
|-----|------|-------------|
| `Time` | DateTime | La fecha y la hora en que ocurrió el evento de auditoría, en UTC |
| `ResourceId` | String | El identificador de recurso Azure Resource Manager para el recurso en el que tuvo lugar el evento |
| `OperationName` | String  | El tipo de acción que se realiza durante el evento |
| `OperationVersion` | String | La versión de API utilizada durante el evento |
| `Category` | String | El tipo de recurso que se está emitiendo |
| `ResultType` | String | El resultado del evento |
| `ResultSignature` | String | El código de estado HTTP para el evento |
| `ResultDescription` | String | Detalles adicionales acerca del evento |
| `DurationMs` | String | Cuánto tiempo se tardó en realizar el evento en milisegundos |
| `CallerIpAddress` | String | Una dirección IP de origen enmascarada para el evento |
| `CorrelationId` | Guid | Identificador único proporcionado por el cliente para el evento |
| `ApplicationId` | Guid | Identificador de aplicación usado en la autorización del portador |
| `Level` | Int | La gravedad del registro del evento |
| `Location` | String | La región en la que tuvo lugar el evento |
| `RequestUri` | Identificador URI | El punto de conexión utilizado durante el evento |
| `TraceId` | String | `TraceId`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Identificador del seguimiento completo usado para identificar de forma única un seguimiento distribuido entre los sistemas. |
| `SpanId` | String | `SpanId`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Identificador de esta solicitud en el seguimiento. |
| `ParentId` | String | `ParentId`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Una solicitud sin un identificador primario es la raíz del seguimiento. |
| `TraceFlags` | String | `TraceFlags`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Controla las marcas de seguimiento, como el muestreo, el nivel de seguimiento, etc. |
| `TraceState` | String | `TraceState`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Información adicional de identificación de seguimiento específica del proveedor que abarca varios sistemas diferentes de seguimiento distribuido. |

A continuación se muestran ejemplos de cuerpos JSON para estos tipos de registros.

#### <a name="adtdigitaltwinsoperation"></a>ADTDigitalTwinsOperation

```json
{
  "time": "2020-03-14T21:11:14.9918922Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/digitaltwins/write",
  "operationVersion": "2020-10-31",
  "category": "DigitalTwinOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": 8,
  "callerIpAddress": "13.68.244.*",
  "correlationId": "2f6a8e64-94aa-492a-bc31-16b9f0b16ab3",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/digitaltwins/factory-58d81613-2e54-4faa-a930-d980e6e2a884?api-version=2020-10-31",
  "properties": {},
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
}
```

#### <a name="adtmodelsoperation"></a>ADTModelsOperation

```json
{
  "time": "2020-10-29T21:12:24.2337302Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/models/write",
  "operationVersion": "2020-10-31",
  "category": "ModelsOperation",
  "resultType": "Success",
  "resultSignature": "201",
  "resultDescription": "",
  "durationMs": "80",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "9dcb71ea-bb6f-46f2-ab70-78b80db76882",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/Models?api-version=2020-10-31",
  "properties": {},
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
}
```

#### <a name="adtqueryoperation"></a>ADTQueryOperation

```json
{
  "time": "2020-12-04T21:11:44.1690031Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/query/action",
  "operationVersion": "2020-10-31",
  "category": "QueryOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": "314",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "1ee2b6e9-3af4-4873-8c7c-1a698b9ac334",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/query?api-version=2020-10-31",
  "properties": {},
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
}
```

#### <a name="adteventroutesoperation"></a>ADTEventRoutesOperation

Este es un ejemplo de cuerpo JSON de un objeto `ADTEventRoutesOperation` que **no** es de tipo `Microsoft.DigitalTwins/eventroutes/action` (para obtener más información sobre el esquema, consulte la sección siguiente, [Esquemas de registro de salida](#egress-log-schemas)).

```json
  {
    "time": "2020-10-30T22:18:38.0708705Z",
    "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
    "operationName": "Microsoft.DigitalTwins/eventroutes/write",
    "operationVersion": "2020-10-31",
    "category": "EventRoutesOperation",
    "resultType": "Success",
    "resultSignature": "204",
    "resultDescription": "",
    "durationMs": 42,
    "callerIpAddress": "212.100.32.*",
    "correlationId": "7f73ab45-14c0-491f-a834-0827dbbf7f8e",
    "identity": {
      "claims": {
        "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
      }
    },
    "level": "4",
    "location": "southcentralus",
    "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/EventRoutes/egressRouteForEventHub?api-version=2020-10-31",
    "properties": {},
    "traceContext": {
      "traceId": "95ff77cfb300b04f80d83e64d13831e7",
      "spanId": "b630da57026dd046",
      "parentId": "9f0de6dadae85945",
      "traceFlags": "01",
      "tracestate": "k1=v1,k2=v2"
    }
  },
```

### <a name="egress-log-schemas"></a>Esquemas de registro de salida

Este es el esquema para los registros `ADTEventRoutesOperation` específicos del nombre de operación `Microsoft.DigitalTwins/eventroutes/action`. Contienen detalles relacionados con las excepciones y las operaciones de API en torno a los puntos de conexión de salida conectados a una instancia de Azure Digital Twins.

|Nombre del campo | Tipo de datos | Descripción |
|-----|------|-------------|
| `Time` | DateTime | La fecha y la hora en que ocurrió el evento de auditoría, en UTC |
| `ResourceId` | String | El identificador de recurso Azure Resource Manager para el recurso en el que tuvo lugar el evento |
| `OperationName` | String  | El tipo de acción que se realiza durante el evento |
| `Category` | String | El tipo de recurso que se está emitiendo |
| `ResultDescription` | String | Detalles adicionales acerca del evento |
| `CorrelationId` | Guid | Identificador único proporcionado por el cliente para el evento |
| `ApplicationId` | Guid | Identificador de aplicación usado en la autorización del portador |
| `Level` | Int | La gravedad del registro del evento |
| `Location` | String | La región en la que tuvo lugar el evento |
| `TraceId` | String | `TraceId`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Identificador del seguimiento completo usado para identificar de forma única un seguimiento distribuido entre los sistemas. |
| `SpanId` | String | `SpanId`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Identificador de esta solicitud en el seguimiento. |
| `ParentId` | String | `ParentId`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Una solicitud sin un identificador primario es la raíz del seguimiento. |
| `TraceFlags` | String | `TraceFlags`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Controla las marcas de seguimiento, como el muestreo, el nivel de seguimiento, etc. |
| `TraceState` | String | `TraceState`, como parte del [contexto de seguimiento de W3C](https://www.w3.org/TR/trace-context/). Información adicional de identificación de seguimiento específica del proveedor que abarca varios sistemas diferentes de seguimiento distribuido. |
| `EndpointName` | String | El nombre del punto de conexión de salida creado en Azure Digital Twins |

A continuación se muestran ejemplos de cuerpos JSON para estos tipos de registros.

#### <a name="adteventroutesoperation-for-microsoftdigitaltwinseventroutesaction"></a>ADTEventRoutesOperation para Microsoft.DigitalTwins/eventroutes/action

Este es un ejemplo de cuerpo JSON para un `ADTEventRoutesOperation` de tipo `Microsoft.DigitalTwins/eventroutes/action`.

```json
{
  "time": "2020-11-05T22:18:38.0708705Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/eventroutes/action",
  "operationVersion": "",
  "category": "EventRoutesOperation",
  "resultType": "",
  "resultSignature": "",
  "resultDescription": "Unable to send EventHub message to [myPath] for event Id [f6f45831-55d0-408b-8366-058e81ca6089].",
  "durationMs": -1,
  "callerIpAddress": "",
  "correlationId": "7f73ab45-14c0-491f-a834-0827dbbf7f8e",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "",
  "properties": {
    "endpointName": "myEventHub"
  },
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
},
```

## <a name="view-and-query-logs"></a>Visualización y consulta de registro

En este artículo, ha configurado los tipos de registros que se almacenan y ha especificado su ubicación de almacenamiento.

Para solucionar el problema y generar información de estos registros, puede generar **consultas personalizadas**. Para empezar, también puede aprovechar algunas de las consultas de ejemplo que proporciona el servicio, que aborda preguntas comunes que los clientes pueden tener sobre su instancia.

Así es como se realizan consultas en los registros de una instancia.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la instancia de Azure Digital Twins. Escriba el nombre en la barra de búsqueda para encontrarla. 

2. Seleccione **Registros** en el menú para abrir la página de consulta del registro. La página se abre en una ventana llamada *Consultas*.

    :::image type="content" source="media/troubleshoot-diagnostics/logs.png" alt-text="Captura de pantalla que muestra la página Registros de una instancia de Azure Digital Twins en Azure Portal con la ventana Consultas superpuesta, en la que se muestran las consultas creadas previamente." lightbox="media/troubleshoot-diagnostics/logs.png":::

    Se trata de consultas de ejemplo creadas previamente escritas para varios registros. Puede seleccionar una de las consultas para cargarla en el editor de consultas y ejecutarla para ver estos registros de su instancia.

    También puede cerrar la ventana *Consultas* sin ejecutar nada para ir directamente a la página del editor de consultas, donde puede escribir o editar el código de consulta personalizado.

3. Después de salir de la ventana *Consultas* verá la página principal del editor de consultas. Aquí puede ver y editar el texto de las consultas de ejemplo, o escribir sus propias consultas desde cero.
    :::image type="content" source="media/troubleshoot-diagnostics/logs-query.png" alt-text="Captura de pantalla que muestra la página Registros de una instancia de Azure Digital Twins en Azure Portal. Incluye una lista de registros, código de consulta e historial de consultas." lightbox="media/troubleshoot-diagnostics/logs-query.png":::

    En el panel izquierdo: 
    - La pestaña *Tables* (Tablas) muestran las distintas [categorías de registro](#log-categories) de Azure Digital Twins que están disponibles para su uso en las consultas. 
    - La pestaña *Queries* (Consultas) contiene las consultas de ejemplo que se pueden cargar en el editor.
    - La pestaña *Filter* (Filtro) permite personalizar una vista filtrada de los datos que devuelve la consulta.

Para más información sobre consultas del registro y cómo escribirlas, puede visitar [Introducción a las consultas de registro en Azure Monitor](../azure-monitor/logs/log-query-overview.md).

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la configuración de diagnósticos, consulte [Recopilación y uso de los datos de registro provenientes de los recursos de Azure](../azure-monitor/essentials/platform-logs-overview.md).
* Para información sobre las métricas de Azure Digital Twins, consulte el artículo [Solución de problemas: visualización de las métricas con Azure Monitor](troubleshoot-metrics.md).
* Para ver cómo habilitar las alertas de las métricas, consulte [Solución de problemas: Configuración de alertas](troubleshoot-alerts.md).