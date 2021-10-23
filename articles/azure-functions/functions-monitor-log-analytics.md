---
title: Supervisión de Azure Functions con registros de Azure Monitor
description: Obtenga información sobre cómo usar los registros de Azure Monitor con Azure Functions para supervisar las ejecuciones de funciones.
author: craigshoemaker
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 8323f3a917cfe51fdc358d28b865db3055f28172
ms.sourcegitcommit: 92889674b93087ab7d573622e9587d0937233aa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130178227"
---
# <a name="monitoring-azure-functions-with-azure-monitor-logs"></a>Supervisión de Azure Functions con registros de Azure Monitor

Azure Functions ofrece integración con los [registros de Azure Monitor](../azure-monitor/logs/data-platform-logs.md) para supervisar las funciones. En este artículo se muestra cómo configurar Azure Functions para enviar los registros generados por el sistema y generados por el usuario a los registros de Azure Monitor.

Los registros de Azure Monitor le ofrecen la posibilidad de consolidar registros de distintos recursos en la misma área de trabajo, donde se pueden analizar con [consultas](../azure-monitor/logs/log-query-overview.md) para recuperar, consolidar y analizar rápidamente los datos recopilados.  Puede crear y probar consultas mediante [Log Analytics](../azure-monitor/logs/log-query-overview.md) en Azure Portal y después analizar los datos directamente mediante estas herramientas o guardar las consultas para usarlas con las [visualizaciones](../azure-monitor/best-practices-analysis.md) o las [reglas de alertas](../azure-monitor/alerts/alerts-overview.md).

Azure Monitor utiliza una versión del [lenguaje de consulta Kusto](/azure/kusto/query/) utilizado por Azure Data Explorer, que es adecuado para realizar búsquedas de registros simples, pero también dispone de funciones avanzadas, como agregaciones, combinaciones y análisis inteligentes. Puede aprender rápidamente el lenguaje de consulta con [diversas lecciones](../azure-monitor/logs/get-started-queries.md).

> [!NOTE]
> La integración con registros de Azure Monitor está actualmente en versión preliminar pública. No es compatible para aplicaciones de funciones que se ejecuten en la [versión 1.x](functions-versions.md).

## <a name="setting-up"></a>Instalación

1. En la sección **Supervisión** de la aplicación de funciones en [Azure Portal](https://portal.azure.com), seleccione **Configuración de diagnóstico** y, a continuación, seleccione **Agregar configuración de diagnóstico**.

   :::image type="content" source="media/functions-monitor-log-analytics/diagnostic-settings-add.png" alt-text="Selección de configuración de diagnóstico":::

1. En la página **Configuración de diagnóstico**, en **Detalles de categoría** y **Registro**, elija **FunctionAppLogs**.

   La tabla **FunctionAppLogs** contiene los registros deseados.

1. En **Detalles de destino**, elija **Enviar a Log Analytics** y, a continuación, seleccione el **área de trabajo de Log Analytics**. 

1. Escriba un **Nombre de configuración de diagnóstico** y, después, seleccione **Guardar**.

   :::image type="content" source="media/functions-monitor-log-analytics/choose-table.png" alt-text="Adición de una configuración de diagnóstico":::

## <a name="user-generated-logs"></a>Registros generados por el usuario

Para generar registros personalizados, use la instrucción de registro específica de su lenguaje. Estos son los fragmentos de código de ejemplo:


# <a name="c"></a>[C#](#tab/csharp)

```csharp
log.LogInformation("My app logs here.");
```

# <a name="java"></a>[Java](#tab/java)

```java
context.getLogger().info("My app logs here.");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
context.log('My app logs here.');
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Write-Host "My app logs here."
```

# <a name="python"></a>[Python](#tab/python)

```python
logging.info('My app logs here.')
```

---

## <a name="querying-the-logs"></a>Consultas de registros

Para consultar los registros generados:
 
1. En la aplicación de funciones, seleccione **Configuración de diagnóstico**. 

1. En la lista de **Configuración de diagnóstico**, seleccione el área de trabajo de Log Analytics que configuró para enviar los registros de la función. 

1. En la página **Área de trabajo de Log Analytics**, seleccione **Registros**.

   Azure Functions escribe todos los registros en la tabla **FunctionAppLogs** en **LogManagement**. 

   :::image type="content" source="media/functions-monitor-log-analytics/querying.png" alt-text="Ventana de consulta en el área de trabajo de Log Analytics":::

A continuación se muestran algunas consultas de ejemplo:

### <a name="all-logs"></a>Todos los registros

```

FunctionAppLogs
| order by TimeGenerated desc

```

### <a name="specific-function-logs"></a>Registros de una función específica

```

FunctionAppLogs
| where FunctionName == "<Function name>" 

```

### <a name="exceptions"></a>Excepciones

```

FunctionAppLogs
| where ExceptionDetails != ""  
| order by TimeGenerated asc

```

## <a name="next-steps"></a>Pasos siguientes

- Revise la [introducción a Azure Functions](functions-overview.md).
- Obtenga más información sobre los [registros de Azure Monitor](../azure-monitor/logs/data-platform-logs.md).
- Obtenga más información sobre el [lenguaje de consulta](../azure-monitor/logs/get-started-queries.md).
