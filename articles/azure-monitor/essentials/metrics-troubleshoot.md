---
title: Solución de problemas de gráficos de métricas en Azure Monitor
description: Solucione los problemas con la creación, la personalización o la interpretación de los gráficos de métricas
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: vitalyg
ms.openlocfilehash: f8ff057f818999a9d170fe6f58101c99cd931f7e
ms.sourcegitcommit: 54e7b2e036f4732276adcace73e6261b02f96343
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2021
ms.locfileid: "129809682"
---
# <a name="troubleshooting-metrics-charts"></a>Solución de problemas de gráficos de métricas

Use este artículo si tienen problemas al crear, personalizar o interpretar los gráficos en el Explorador de métricas de Azure. Si no está familiarizado con las métricas, obtenga información sobre la [introducción al explorador de métricas](metrics-getting-started.md) y las [características avanzadas del explorador de métricas](../essentials/metrics-charts.md). También puede ver [ejemplos](../essentials/metric-chart-samples.md) de los gráficos de métricas configurados.

## <a name="chart-shows-no-data"></a>El gráfico no muestra ningún dato

En ocasiones, puede que los gráficos no muestren ningún dato después de seleccionar las métricas y los recursos correctos. Este comportamiento puede deberse a varias de las siguientes razones:

### <a name="microsoftinsights-resource-provider-isnt-registered-for-your-subscription"></a>El proveedor de recursos Microsoft.Insights no se ha registrado para la suscripción

La exploración de métricas requiere que el proveedor de recursos *Microsoft.Insights* esté registrado en su suscripción. En muchos casos, se registra automáticamente (es decir, después de configurar una regla de alerta, personalizar la configuración de diagnóstico para cualquier recurso o configurar una regla de escalado automático). Si el proveedor de recursos Microsoft.Insights no está registrado, debe registrarlo manualmente siguiendo los pasos descritos en [Tipos y proveedores de recursos de Azure](../../azure-resource-manager/management/resource-providers-and-types.md).

**Solución:** Abra **Suscripciones**, pestaña **Proveedores de recursos** y verifique que *Microsoft.Insights* esté registrado para su suscripción.

### <a name="you-dont-have-sufficient-access-rights-to-your-resource"></a>No tiene suficientes derechos de acceso al recurso

En Azure, el acceso a las métricas se controla mediante el [control de acceso basado en rol (RBAC de Azure)](../../role-based-access-control/overview.md). Debe ser miembro del [lector de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-reader), [colaborador de supervisión](../../role-based-access-control/built-in-roles.md#monitoring-contributor) o [colaborador](../../role-based-access-control/built-in-roles.md#contributor) para explorar las métricas de cualquier recurso.

**Solución:** Asegúrese de que tiene permisos suficientes para el recurso desde el que explora las métricas.

### <a name="your-resource-didnt-emit-metrics-during-the-selected-time-range"></a>El recurso no emite métricas durante el intervalo de tiempo seleccionado

Algunos recursos no emiten constantemente sus métricas. Por ejemplo, Azure no recopilará las métricas de máquinas virtuales detenidas. Otros recursos pueden emitir sus métricas solo cuando se produce alguna condición. Por ejemplo, una métrica que muestra el tiempo de procesamiento de una transacción requiere al menos una transacción. Si no hubiera ninguna transacción en el intervalo de tiempo seleccionado, el gráfico naturalmente estará vacío. Además, si bien la mayoría de las métricas en Azure se recopilan cada minuto, hay algunas que se recopilan con menos frecuencia. Consulte la documentación de las métricas para obtener más detalles acerca de la métrica que quiere explorar.

**Solución:** Cambie la hora del gráfico a un intervalo más amplio. Puede comenzar desde "Últimos 30 días" con una mayor granularidad de tiempo (o basarse en la opción "Automatic time granularity" [Granularidad de tiempo automática]).

### <a name="you-picked-a-time-range-greater-than-30-days"></a>Ha elegido un intervalo de tiempo mayor que 30 días

[La mayoría de las métricas en Azure se almacenan durante 93 días](../essentials/data-platform-metrics.md#retention-of-metrics). Sin embargo, solo puede consultar datos para no más de 30 días en un único gráfico. Esta limitación no se aplica a las [métricas basadas en registros](../app/pre-aggregated-metrics-log-metrics.md#log-based-metrics).

**Solución:** Si ve un gráfico en blanco o el gráfico solo muestra parte de los datos de métricas, verifique que la diferencia entre las fechas de inicio y final en el selector de tiempo no supere el intervalo de 30 días. Una vez que haya seleccionado un intervalo de 30 días, puede [desplazarse de forma lateral](metrics-charts.md#pan) por el gráfico para ver el período de retención completo.

### <a name="all-metric-values-were-outside-of-the-locked-y-axis-range"></a>Todos los valores de métricas estaban fuera del intervalo del eje Y bloqueado

Al [bloquear los límites del eje Y del gráfico](../essentials/metrics-charts.md#locking-the-range-of-the-y-axis), puede hacer que por accidente el área de visualización del gráfico no muestre la línea del gráfico. Por ejemplo, si el eje Y está bloqueado en un intervalo entre el 0 y el 50 %, y la métrica tiene un valor constante del 100 %, la línea se representa siempre fuera del área visible, lo que hace que el gráfico parezca estar en blanco.

**Solución:** Verifique que los límites del eje Y del gráfico no queden bloqueados fuera del intervalo de los valores de las métricas. Si los límites del eje Y se bloquean, puede querer restablecerlos temporalmente para asegurarse de que los valores de las métricas no se encuentren fuera del intervalo del gráfico. No se recomienda bloquear el intervalo del eje Y con granularidad automática para los gráficos con agregación **sum**, **min** y **max**, ya que sus valores cambiarán con la granularidad al cambiar el tamaño de la ventana del explorador o al pasar de una resolución de pantalla a otra. Cambiar la granularidad puede hacer que el área de visualización del gráfico quede vacía.

### <a name="you-are-looking-at-a-guest-classic-metric-but-didnt-enable-azure-diagnostic-extension"></a>Ve una métrica de invitado (clásico), pero no ha habilitado la extensión de Azure Diagnostics

La colección de métricas de **invitado (clásico)** requiere la configuración de la extensión de Azure Diagnostics o habilitarla mediante el panel **Configuración de diagnóstico** para el recurso.

**Solución:** Si la extensión de Azure Diagnostics está habilitada, pero todavía no puede ver las métricas, siga los pasos descritos en la [guía de solución de problemas de extensión de Azure Diagnostics](../agents/diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal). Consulte también los pasos de solución de problemas para [No se pueden elegir las métricas y el espacio de nombres de invitado (clásico)](#cannot-pick-guest-namespace-and-metrics).

## <a name="error-retrieving-data-message-on-dashboard"></a>Mensaje "Error al recuperar datos" en el panel

Este problema puede producirse si se creó el panel con una métrica que más tarde dejó de utilizarse y acabó quitándose de Azure. Para verificar que sea así, abra la pestaña **Métricas** del recurso y compruebe las métricas disponibles en el selector de métricas. Si no se muestra la métrica, se quitó de Azure. Normalmente, cuando una métrica está en desuso, hay una nueva métrica mejor que brinda una perspectiva similar sobre el estado del recurso.

**Solución:** Actualice el elemento con error; para ello, elija una métrica alternativa para el gráfico en el panel. Puede [revisar una lista de métricas disponibles para servicios de Azure](./metrics-supported.md).

## <a name="chart-shows-dashed-line"></a>El gráfico muestra una línea discontinua

Los gráficos de métricas de Azure usan el estilo de línea discontinua para indicar que hay un valor que falta (también conocido como "valor null") entre dos puntos de datos de intervalo de agregación conocidos. Por ejemplo, si en el selector de tiempo ha elegido la granularidad de tiempo "1 minuto", pero la métrica se notificó a las 07:26, 07:27, 07:29 y 07:30 (observe la brecha de un minuto entre el segundo y el tercer punto de datos), entonces una línea discontinua conectará 07:27 con 07:29 y una línea continua conectará todos los demás puntos de datos. La línea discontinua cae a cero cuando la métrica usa la agregación **count** y **sum**. Para las agregaciones **avg**, **min** o **max**, la línea discontinua conecta dos puntos de datos conocidos más cercanos. Además, cuando faltan los datos en el lado más a la derecha o más a la izquierda del gráfico, la línea discontinua se expande hacia la dirección del punto de datos faltante.
  ![Captura de pantalla que muestra cómo, cuando faltan los datos en el lado más a la derecha o más a la izquierda del gráfico, la línea discontinua se expande hacia la dirección del punto de datos faltante.](./media/metrics-troubleshoot/dashed-line.png)

**Solución:** Este comportamiento es así por diseño. Resulta útil para identificar los puntos de datos que faltan. El gráfico de líneas es una opción superior para visualizar las tendencias de las métricas de alta densidad, pero puede ser difícil de interpretar para las métricas con valores dispersos, especialmente cuando es importante la correlación de valores con intervalo de agregación. La línea discontinua facilita la lectura de estos gráficos, pero si el gráfico sigue no siendo claro, considere la posibilidad de ver las métricas con otro tipo de gráfico. Por ejemplo, un gráfico de trazado disperso de la misma métrica muestra claramente cada intervalo de agregación al mostrar solo un punto cuando hay un valor y omite los datos de punto por completo cuando falta el valor: ![Captura de pantalla que resalta la opción de menú Gráfico de dispersión.](./media/metrics-troubleshoot/scatter-plot.png)

   > [!NOTE]
   > Si aún prefiere un gráfico de líneas para la métrica, mover el mouse sobre el gráfico puede ayudarle a evaluar la granularidad de tiempo al resaltar el punto de datos en la ubicación del puntero del mouse.

## <a name="chart-shows-unexpected-drop-in-values"></a>El gráfico muestra una caída inesperada en los valores

En muchos casos, la caída percibida en los valores de métrica es una falta de comprensión de los datos que se muestran en el gráfico. Puede verse confundido por un descenso en las sumas o los recuentos cuando el gráfico muestra los minutos más recientes porque los puntos de datos más recientes de las métricas no se han recibido o porque Azure no los ha procesado. Según el servicio, la latencia del procesamiento de métricas puede ubicarse dentro del intervalo un par de minutos. Para los gráficos que muestran un intervalo de tiempo reciente con una granularidad de 1 a 5 minutos, la caída del valor a lo largo de los últimos minutos se vuelve más evidente: ![Captura de pantalla que muestra una caída del valor en los últimos minutos.](./media/metrics-troubleshoot/unexpected-dip.png)

**Solución:** Este comportamiento es así por diseño. Creemos que es beneficioso mostrar los datos tan pronto como se reciben, incluso cuando los datos son *parciales* o *incompletos*. Esto le permite llegar a conclusiones importantes antes y comenzar a investigar de inmediato. Por ejemplo, para una métrica que muestra el número de errores, ver un valor parcial X le indica que se produjeron al menos X errores en un minuto dado. Puede comenzar a investigar el problema de inmediato, en lugar de esperar a ver el recuento exacto de los errores que se produjeron en este minuto, lo que podría no ser tan importante. El gráfico se actualizará una vez que se reciba todo el conjunto de datos, pero en ese momento puede que se muestren los nuevos puntos de datos incompletos de los minutos más recientes.

## <a name="cannot-pick-guest-namespace-and-metrics"></a>No se pueden elegir las métricas y el espacio de nombres de invitado

Las máquinas virtuales y los conjuntos de escalado de máquinas virtuales tienen dos categorías de métricas: las métricas del **host de máquina virtual** recopiladas por el entorno de hospedaje de Azure y las métricas de **invitado (clásico)** recopiladas por el [agente de supervisión](../agents/agents-overview.md) que se ejecuta en las máquinas virtuales. El agente de supervisión se instala al habilitar la [extensión de Azure Diagnostics](../agents/diagnostics-extension-overview.md).

De forma predeterminada, las métricas de invitado (clásico) se almacenan en la cuenta de Azure Storage, que elige en la pestaña **Configuración de diagnóstico** del recurso. Si no se recopilan métricas de invitado o el explorador de métricas no puede acceder a ellas, solo verá el espacio de nombres de las métricas del **host de máquina virtual**:

![imagen de métrica](./media/metrics-troubleshoot/vm-metrics.png)

**Solución:** Si no ve el espacio de nombres y las métricas de **invitado (clásico)** en el explorador de métricas:

1. Confirme que la [extensión de Azure Diagnostics](../agents/diagnostics-extension-overview.md) esté habilitada y configurada para recopilar métricas.
    > [!WARNING]
    > No puede usar el [agente de Log Analytics](../agents/agents-overview.md#log-analytics-agent) (también denominado Microsoft Monitoring Agent o "MMA") para enviar el **invitado (clásico)** a una cuenta de almacenamiento.

1. Asegúrese de que el proveedor de recursos **Microsoft.Insights** se ha [registrado para la suscripción](#microsoftinsights-resource-provider-isnt-registered-for-your-subscription).

1. Verifique que la cuenta de almacenamiento no esté protegida por el firewall. Azure Portal necesita acceder a la cuenta de almacenamiento para recuperar los datos de métricas y trazar los gráficos.

1. Use el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) para validar que las métricas fluyan hacia la cuenta de almacenamiento. Si no se recopilan métricas, siga la [guía de solución de problemas de la extensión de Azure Diagnostics](../agents/diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal).

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre la introducción al Explorador de métricas](metrics-getting-started.md)
* [Más información sobre las características avanzadas del Explorador de métricas](../essentials/metrics-charts.md)
* [Vea una lista de métricas disponibles para servicios de Azure](./metrics-supported.md)
* [Vea ejemplos de gráficos configurados](../essentials/metric-chart-samples.md)
