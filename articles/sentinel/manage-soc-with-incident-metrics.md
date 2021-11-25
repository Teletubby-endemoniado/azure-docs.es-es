---
title: Mejora de la administración de SOC con las métricas de incidentes de Microsoft Sentinel | Microsoft Docs
description: Use la información de la pantalla de métricas de incidentes de Microsoft Sentinel y el libro como ayuda para administrar el centro de operaciones de seguridad (SOC).
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: how-to
ms.custom: mvc, ignite-fall-2021
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2021
ms.author: yelevin
ms.openlocfilehash: 14188ad0738b0e817bb10fcff7b05441022efac5
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132720803"
---
# <a name="manage-your-soc-better-with-incident-metrics"></a>Mejora en la administración de SOC con métricas de incidentes

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Los administradores del Centro de operaciones de seguridad (SOC) deben tener medidas y métricas de eficiencia generales a su alcance para calibrar el rendimiento de su equipo. Querrá ver las operaciones de los incidentes a lo largo del tiempo clasificadas por muchos criterios diferentes, como la gravedad, las tácticas de MITRE, el tiempo medio para la evaluación de prioridades, el tiempo medio de resolución, etc. Ahora, Microsoft Sentinel pone todos estos datos a su disposición con la nueva tabla **SecurityIncident** y el esquema de Log Analytics y el libro **Security operations efficiency** (Eficiencia de las operaciones de seguridad) que se incluye. Podrá visualizar el rendimiento del equipo a lo largo del tiempo y usar esta información para mejorar su eficacia. También puede escribir y usar sus propias consultas de KQL en la tabla de incidentes para crear libros personalizados que se ajusten a sus necesidades específicas de auditoría y a los indicadores clave de rendimiento.

## <a name="use-the-security-incidents-table"></a>Uso de la tabla de incidentes de seguridad

La tabla **SecurityIncident** está integrada en Microsoft Sentinel. La encontrará con las demás tablas de la colección **SecurityInsights** en **Registros**. Puede consultarla como cualquier otra tabla de Log Analytics.

:::image type="content" source="./media/manage-soc-with-incident-metrics/security-incident-table.png" alt-text="Tabla de incidentes de seguridad":::

Cada vez que se crea o se actualiza un incidente, se agregará una nueva entrada del registro a la tabla. Esto le permite realizar un seguimiento de los cambios realizados en incidentes y permite usar métricas de SOC aún más eficaces, pero debe ser consciente de ello al construir consultas para esta tabla, ya que puede que tenga que quitar entradas duplicadas de un incidente (en función de la consulta exacta que esté ejecutando). 

Por ejemplo, si deseara devolver una lista de todos los incidentes ordenados por número de incidente, pero solo deseara devolver el registro más reciente por incidente, podría hacerlo mediante el [operador summarize](/azure/data-explorer/kusto/query/summarizeoperator) de KQL con la `arg_max()` [función aggregation](/azure/data-explorer/kusto/query/arg-max-aggfunction):

```Kusto
SecurityIncident
| summarize arg_max(LastModifiedTime, *) by IncidentNumber
```
### <a name="more-sample-queries"></a>Más consultas de ejemplo

Estado del incidente: todos los incidentes por estado y gravedad en un periodo de tiempo determinado:

```Kusto
let startTime = ago(14d);
let endTime = now();
SecurityIncident
| where TimeGenerated >= startTime
| summarize arg_max(TimeGenerated, *) by IncidentNumber
| where LastModifiedTime  between (startTime .. endTime)
| where Status in  ('New', 'Active', 'Closed')
| where Severity in ('High','Medium','Low', 'Informational')
```

Tiempo medio para la clausura:
```Kusto
SecurityIncident
| summarize arg_max(TimeGenerated,*) by IncidentNumber 
| extend TimeToClosure =  (ClosedTime - CreatedTime)/1h
| summarize 5th_Percentile=percentile(TimeToClosure, 5),50th_Percentile=percentile(TimeToClosure, 50), 
  90th_Percentile=percentile(TimeToClosure, 90),99th_Percentile=percentile(TimeToClosure, 99)
```

Tiempo medio para la evaluación de prioridades:
```Kusto
SecurityIncident
| summarize arg_max(TimeGenerated,*) by IncidentNumber 
| extend TimeToTriage =  (FirstModifiedTime - CreatedTime)/1h
| summarize 5th_Percentile=max_of(percentile(TimeToTriage, 5),0),50th_Percentile=percentile(TimeToTriage, 50), 
  90th_Percentile=percentile(TimeToTriage, 90),99th_Percentile=percentile(TimeToTriage, 99) 
```

## <a name="security-operations-efficiency-workbook"></a>Libro Security operations efficiency (Eficiencia de las operaciones de seguridad)

Para complementar la tabla **SecurityIncidents**, se ha proporcionado una plantilla del libro **security operations efficiency** (eficiencia de las operaciones de seguridad) que se puede usar para supervisar las operaciones de SOC. El libro contiene las siguientes métricas: 
- Incidentes creados a lo largo del tiempo 
- Incidentes creados por clasificación de cierre, gravedad, propietario y estado 
- Tiempo medio para la evaluación de prioridades 
- Tiempo medio para la clausura 
- Incidentes creados por gravedad, propietario, estado, producto y tácticas a lo largo del tiempo 
- Percentiles de tiempo para la evaluación de prioridades 
- Percentiles de tiempo para la clausura 
- Tiempo medio para la evaluación de prioridades por propietario 
- Actividades recientes 
- Clasificaciones de cierre recientes  

Para encontrar esta nueva plantilla de libro, elija **Libros en** el menú de navegación de Microsoft Sentinel y seleccione la pestaña **Plantillas**. Elija **Eficiencia de las operaciones de seguridad** en la galería y haga clic en uno de los botones **Ver libro guardado** y **Ver plantilla**.

:::image type="content" source="./media/manage-soc-with-incident-metrics/security-incidents-workbooks-gallery.png" alt-text="Galería de Security incidents workbook"::: (Libro de incidentes de seguridad)

:::image type="content" source="./media/manage-soc-with-incident-metrics/security-operations-workbook-1.png" alt-text="Security incidents workbook (Libro de incidentes de seguridad) completado":::

Puede usar la plantilla para crear sus propios libros personalizados adaptados a sus necesidades específicas.

## <a name="securityincidents-schema"></a>Esquema de SecurityIncidents

[!INCLUDE [SecurityIncidents schema](../../includes/sentinel-schema-security-incident.md)]

## <a name="next-steps"></a>Pasos siguientes

- Para empezar a trabajar con Microsoft Azure Sentinel, necesita una suscripción a Microsoft Azure. Si no tiene una suscripción, puede registrarse para obtener una [evaluación gratuita](https://azure.microsoft.com/free/).
- Aprenda a [incorporar los datos en Microsoft Azure Sentinel](quickstart-onboard.md), [obtenga visibilidad sobre ellos y aprenda a defenderse de posibles amenazas](get-visibility.md).
