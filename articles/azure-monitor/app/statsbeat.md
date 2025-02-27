---
title: Statsbeat en Azure Application Insights | Microsoft Docs
description: Estadísticas sobre la instrumentación automática y los SDK de Application Insights
ms.topic: conceptual
ms.date: 09/20/2021
ms.openlocfilehash: 55e727bc05007c69f0144c7f95e17e38f907742e
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131426893"
---
# <a name="statsbeat-in-azure-application-insights"></a>Statsbeat en Azure Application Insights

Statsbeat recopila [métricas personalizadas](../essentials/metrics-custom-overview.md) esenciales y no esenciales sobre la instrumentación automática y los SDK de Application Insights. Statsbeat ofrece tres ventajas para los clientes de Azure Monitor Application Insights:
-   Service Health y confiabilidad (supervisión de afuera hacia adentro de la conectividad con el punto de conexión de ingesta)
-   Diagnóstico de soporte técnico (información de autoayuda e información de CSS)
-   Mejora del producto (información para optimizaciones de diseño)

Los datos de Statsbeat se almacenan en un almacén de datos de Microsoft.  No afectan al volumen ni al costo de supervisión general de los clientes. 

## <a name="what-data-does-statsbeat-collect"></a>¿Qué datos recopila Statsbeat?

Statsbeat recopila métricas esenciales y no esenciales.

## <a name="supported-languages"></a>Idiomas compatibles

| C#                        | Java            | JavaScript                | Node.js         | Python          |
|---------------------------|-----------------|---------------------------|-----------------|-----------------|
| No se admite actualmente   | Compatible       | No se admite actualmente   | Compatible       | Compatible       |


### <a name="essential-statsbeat"></a>Statsbeat esencial

#### <a name="network-statsbeat"></a>Statsbeat de red

|Nombre de la métrica|Unidad|Dimensiones compatibles|
|-----|-----|-----|
|Recuento de éxitos de solicitud|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Recuento de errores de solicitud|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Duración de la solicitud|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Número de reintentos|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Recuento de limitaciones|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|
|Recuento de excepciones|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`, `Endpoint`, `Host`|

#### <a name="attach-statsbeat"></a>Statsbeat de asociación

|Nombre de la métrica|Unidad|Dimensiones compatibles|
|-----|-----|-----|
|Attach|Count| `Resource Provider`, `Resource Provider Identifier`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`|

#### <a name="feature-statsbeat"></a>Statsbeat de características

|Nombre de la métrica|Unidad|Dimensiones compatibles|
|-----|-----|-----|
|Característica|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Feature`, `Type`, `Operating System`, `Language`, `Version`|

### <a name="non-essential-statsbeat"></a>Statsbeat no esencial

Seguimiento del error de E/S de disco al usar la persistencia de disco para la telemetría recuperable

|Nombre de la métrica|Unidad|Dimensiones compatibles|
|-----|-----|-----|
|Recuento de errores de lectura|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`|
|Recuento de errores de escritura|Count| `Resource Provider`, `Attach Type`, `Instrumentation Key`, `Runtime Version`, `Operating System`, `Language`, `Version`|

### <a name="configure-statsbeat"></a>Statsbeat de configuración

#### <a name="java"></a>[Java](#tab/java)

Para deshabilitar Statsbeat no esencial, agregue la siguiente configuración al archivo de configuración.

```json
{
  "preview": {
    "statsbeat": {
        "disabled": "true"
    }
  }
}
```

También puede deshabilitar esta característica estableciendo la variable de entorno `APPLICATIONINSIGHTS_STATSBEAT_DISABLED` en true (que tendrá prioridad sobre lo que se deshabilite en función de la configuración de json).

#### <a name="node"></a>[Node](#tab/node)

N/D

#### <a name="python"></a>[Python](#tab/python)

N/D

---
