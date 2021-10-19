---
title: Administración del uso y los costos con los registros de Azure Monitor
description: Aprenda cómo cambiar el plan de precios y cómo administrar el volumen de datos y la directiva de retención para el área de trabajo de Log Analytics en Azure Monitor.
services: azure-monitor
documentationcenter: azure-monitor
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/23/2021
ms.author: bwren
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: febbfc5a1a3381affac50a75a29cb502c7872d69
ms.sourcegitcommit: e82ce0be68dabf98aa33052afb12f205a203d12d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2021
ms.locfileid: "129657226"
---
# <a name="manage-usage-and-costs-with-azure-monitor-logs"></a>Administrar el uso y los costos con los registros de Azure Monitor    

> [!NOTE]
> En este artículo se describe cómo entender y controlar los costos de los registros de Azure Monitor. En un artículo relacionado, [Supervisión del uso y costos estimados](../usage-estimated-costs.md), se describe cómo ver el uso y los costos estimados mediante varias características de supervisión de Azure para los distintos modelos de precios. Todos los precios y costos de este artículo se muestran solo con fines de ejemplo. 

Los registros de Azure Monitor están diseñados para escalar y admitir la recopilación, indexación y almacenamiento de grandes cantidades de datos por día provenientes de cualquier origen dentro de su empresa o implementados en Azure.  Aunque esto puede ser un factor clave principal para su organización, la rentabilidad es en última instancia el factor clave subyacente. Con este fin, es importante comprender que el costo de un área de trabajo de Log Analytics no se basa simplemente en el volumen de datos recopilados; también depende del plan seleccionado y de cuánto tiempo se almacenen los datos generados a partir de los orígenes conectados.  

En este artículo se revisa cómo puede supervisar de forma proactiva el crecimiento del almacenamiento y el volumen de datos ingeridos. También se describe cómo definir límites para controlar los costos asociados. 

## <a name="pricing-model"></a>Modelo de precios

Los precios predeterminados de Log Analytics corresponden a un modelo de **Pago por uso**, que se basa en el volumen de datos ingeridos y, opcionalmente, permite prolongar la retención de datos. El volumen de datos se mide como el tamaño de los datos que se almacenarán, en GB (10^9 bytes). Cada área de trabajo de Log Analytics se cobra como un servicio independiente y contribuye a la factura de la suscripción a Azure. La cantidad de ingesta de datos puede ser considerable dependiendo de los factores siguientes: 

  - Conjunto de soluciones de administración habilitadas y su configuración
  - Número y tipo de recursos supervisados
  - Tipo de datos recopilados de cada recurso supervisado
  
Además del modelo de Pago por uso, Log Analytics tiene **niveles de compromiso** que le permiten ahorrar hasta un 30 % en comparación con el precio de Pago por uso. Con los precios del nivel de compromiso se puede comprometer a comprar ingesta de datos a partir de 100 GB al día por un precio inferior al de Pago por uso. Cualquier uso que supere el nivel de compromiso (uso por encima del límite) se facturará a ese mismo precio por GB según lo provisto por el nivel de compromiso actual. Los niveles de compromiso tienen un período de compromiso de 31 días. Durante el período de compromiso, puede cambiar a un nivel de compromiso de nivel superior (lo que reinicia el período de compromiso de 31 días), pero no podrá volver a la versión de Pago por uso o a un nivel de compromiso inferior hasta que el período de compromiso finalice. La facturación de los niveles de compromiso se realiza a diario. [Más información](https://azure.microsoft.com/pricing/details/monitor/) sobre los precios de Pago por uso y de nivel de compromiso de Log Analytics. 

> [!NOTE]
> A partir del 2 de junio de 2021, las **reservas de capacidad** se denominan **niveles de compromiso**. Ahora, los datos recopilados por encima del nivel de compromiso (uso por encima del límite) se facturan con el mismo precio por GB que el nivel de compromiso actual, lo que disminuye los costos en comparación con el antiguo método de facturación con la tarifa de Pago por uso y reduce la necesidad de los usuarios con grandes volúmenes de datos de ajustar su nivel de compromiso. También se agregaron tres nuevos niveles de compromiso: 1000, 2000 y 5000 GB/día. 

En todos los planes de tarifa, el tamaño de los datos de un evento se calcula a partir de una representación de cadena de las propiedades que se almacenan en Log Analytics para ese evento, independientemente de si los datos se envían desde un agente o si se agregan durante el proceso de ingesta. Esto incluye cualquier [campo personalizado](custom-fields.md) que se agregue a medida que se recopilan datos y luego se almacenan en Log Analytics. Varias propiedades comunes a todos los tipos de datos, incluidas algunas [propiedades estándar de Log Analytics](./log-standard-columns.md), se excluyen del cálculo del tamaño del evento. Esto incluye `_ResourceId`, `_SubscriptionId`, `_ItemId`, `_IsBillable`, `_BilledSize` y `Type`. Todas las demás propiedades almacenadas en Log Analytics se incluyen en el cálculo del tamaño del evento. Algunos tipos de datos están libres de los cargos de ingesta de datos; por ejemplo, los tipos [AzureActivity](/azure/azure-monitor/reference/tables/azureactivity), [Latido](/azure/azure-monitor/reference/tables/heartbeat), [Uso](/azure/azure-monitor/reference/tables/usage) y [Operación](/azure/azure-monitor/reference/tables/operation). Algunas soluciones tienen más directivas específicas de la solución sobre la ingesta gratuita de datos; por ejemplo, [Azure Migrate](https://azure.microsoft.com/pricing/details/azure-migrate/) libra de cargos los datos de visualización de dependencias durante los 180 primeros días de Server Assessment. Para determinar si un evento se ha excluido de la facturación relacionada con la ingesta de datos, puede usar la propiedad [_IsBillable](log-standard-columns.md#_isbillable), como se muestra [más adelante](#data-volume-for-specific-events). El uso se notifica en GB (10^9 bytes). 

Además, algunas soluciones como [Azure Defender (Security Center)](https://azure.microsoft.com/pricing/details/azure-defender/), [Azure Sentinel](https://azure.microsoft.com/pricing/details/azure-sentinel/) y [Administración de configuración](https://azure.microsoft.com/pricing/details/automation/) tienen sus propios modelos de precios. 

### <a name="log-analytics-dedicated-clusters"></a>Clústeres dedicados de Log Analytics

Los [clústeres dedicados de Log Analytics](logs-dedicated-clusters.md) son colecciones de áreas de trabajo en un único clúster administrado de Azure Data Explorer para permitir escenarios avanzados, como las [claves administradas por el cliente](customer-managed-keys.md). Los clústeres dedicados de Log Analytics usan el mismo modelo de precios de nivel de compromiso que las áreas de trabajo, salvo que un clúster debe tener un nivel de compromiso de al menos 500 GB/día. No hay ninguna opción Pago por uso para los clústeres. El nivel de compromiso del clúster tiene un período de compromiso de 31 días después de aumentar el nivel de compromiso. Durante el período de compromiso, no se puede reducir el nivel de compromiso, pero sí se puede aumentar en cualquier momento. Cuando las áreas de trabajo están asociadas a un clúster, la facturación de ingesta de datos para esas áreas de trabajo se realiza en el nivel de clúster con el nivel de compromiso configurado. Más información sobre la [creación de clústeres de Log Analytics](customer-managed-keys.md#create-cluster) y la [asociación de áreas de trabajo a ellos](customer-managed-keys.md#link-workspace-to-cluster). Para obtener información sobre los precios de los niveles de compromiso, consulte la [página de precios de Azure Monitor]( https://azure.microsoft.com/pricing/details/monitor/).

El nivel de compromiso del clúster se configura mediante programación con Azure Resource Manager usando el parámetro `Capacity` en `Sku`. `Capacity` se especifica en unidades de GB y puede tener valores de 500, 1000, 2000 o 5000 GB/día. Cualquier uso que supere el nivel de compromiso (uso por encima del límite) se facturará a ese mismo precio por GB según lo provisto por el nivel de compromiso actual.  Para más información, consulte [Clave administrada por el cliente de Azure Monitor](customer-managed-keys.md).

Hay dos modos de facturación para el uso en un clúster. El parámetro `billingType` puede especificarlos cuando [se crea un clúster](logs-dedicated-clusters.md#create-a-dedicated-cluster) o pueden establecerse después de la creación. Los dos modos son: 

- **Clúster**: en este caso (que es el modo predeterminado), la facturación de los datos ingeridos se realiza en el nivel de clúster. Las cantidades de datos ingeridas desde cada área de trabajo asociada a un clúster se suman para calcular la factura diaria del clúster. Las asignaciones por nodo de [Azure Defender (Security Center)](../../security-center/index.yml) se aplican en el nivel de área de trabajo antes de esta agregación de datos agregados en todas las áreas de trabajo del clúster. 

- **Áreas de trabajo**: los costos del nivel de compromiso para el clúster se atribuyen proporcionalmente a las áreas de trabajo del clúster, por el volumen de ingesta de datos de cada área de trabajo [después de tener en cuenta las asignaciones por nodo de [Azure Defender (Security Center)](../../security-center/index.yml) para cada área de trabajo]. Si el volumen total de datos ingeridos en un clúster durante un día es menor que el nivel de compromiso, cada área de trabajo se factura por sus datos ingeridos con la tasa de nivel de compromiso por GB efectiva y la facturación de una fracción del nivel de compromiso. La parte sin usar del nivel de compromiso se factura al recurso de clúster. Si el volumen total de datos ingeridos en un clúster durante un día es superior al del nivel de compromiso, se factura a cada área de trabajo una fracción del nivel de compromiso en función de su fracción de la cantidad de datos ingeridos ese día, y también una fracción de los datos ingeridos por encima del nivel de compromiso. Si el volumen total de datos ingeridos en un área de trabajo durante un día supera el nivel de compromiso, no se factura nada al recurso de clúster.

En las opciones de facturación del clúster, la retención de datos se factura por cada área de trabajo. La facturación del clúster comienza cuando este se crea, independientemente de si las áreas de trabajo se han asociado al clúster. Las áreas de trabajo asociadas a un clúster ya no tienen su propio plan de tarifa.

## <a name="estimating-the-costs-to-manage-your-environment"></a>Estimación de los costos de administración del entorno 

Si aún no usa los registros de Azure Monitor, puede usar la [calculadora de precios de Azure Monitor](https://azure.microsoft.com/pricing/calculator/?service=monitor) para calcular el costo del uso de Log Analytics. Comience por escribir "Azure Monitor" en el cuadro **Buscar** y haga clic en el icono de Azure Monitor resultante. Desplácese hacia abajo en la página hasta **Azure Monitor** y expanda la sección **Log Analytics**. Aquí puede especificar los GB de datos que espera recopilar. Si ya está evaluando los registros de Azure Monitor, puede usar las estadísticas de datos de su propio entorno. Consulte a continuación cómo determinar el [número de máquinas virtuales supervisadas](#understanding-nodes-sending-data) y el [volumen de los datos que ingiere el área de trabajo](#understanding-ingested-data-volume). Si aún no está ejecutando Log Analytics, estas son algunas instrucciones para calcular los volúmenes de datos:

1. **Supervisión de VM:** con la supervisión típica habilitada, se ingieren de 1 GB a 3 GB de datos al mes por VM supervisada. 
2. **Supervisión de clústeres de Azure Kubernetes Service (AKS):** los detalles sobre los volúmenes de datos esperados para supervisar un clúster de AKS típico están disponibles [aquí](../containers/container-insights-cost.md#estimating-costs-to-monitor-your-aks-cluster). Siga estos [procedimientos recomendados](../containers/container-insights-cost.md#controlling-ingestion-to-reduce-cost) para controlar los costos de supervisión del clúster de AKS. 
3. **Supervisión de aplicaciones:** la calculadora de precios de Azure Monitor incluye un estimador de volumen de datos que emplea el uso de la aplicación y se basa en un análisis estadístico de los volúmenes de datos de Application Insights. En la sección Application Insights de la calculadora de precios, utilice el conmutador situado junto a "Estimar el volumen de datos en función de la actividad de la aplicación". 

## <a name="understand-your-usage-and-estimate-costs"></a>Información útil del uso y los costos estimados

Si ya usa los registros de Azure Monitor, es fácil comprender cuáles serán probablemente los costos según los patrones de uso recientes. Para ello, utilice **Uso y costos estimados de Log Analytics** a fin de revisar y analizar el uso de datos. Muestra la cantidad de datos que recopila cada solución, la cantidad de datos que se retienen y una estimación de los costos según la cantidad de datos ingeridos y cualquier retención adicional más allá de la cantidad incluida.

:::image type="content" source="media/manage-cost-storage/usage-estimated-cost-dashboard-01.png" alt-text="Información útil del uso y los costos estimados":::

Para explorar los datos más detalladamente, seleccione el icono situado en la esquina superior derecha de cualquiera de los gráficos de la página **Uso y costos estimados**. Ahora puede trabajar con esta consulta para explorar más detalles sobre su uso.  

:::image type="content" source="media/manage-cost-storage/logs.png" alt-text="Visualización de registros":::

En la página **Uso y costos estimados** puede revisar el volumen de datos del mes. Esto incluye todos los datos recibidos y retenidos facturables en el área de trabajo de Log Analytics.  
 
Los cargos de Log Analytics se agregarán a la factura de Azure. Puede consultar los detalles de su factura de Azure en la sección **Facturación** de Azure Portal o en el [Portal de facturación de Azure](https://account.windowsazure.com/Subscriptions).  

## <a name="viewing-log-analytics-usage-on-your-azure-bill"></a>Visualización del uso de Log Analytics en la factura de Azure 

Azure proporciona una gran cantidad de funcionalidades útiles en el centro [Azure Cost Management + Facturación](../../cost-management-billing/costs/quick-acm-cost-analysis.md?toc=%2fazure%2fbilling%2fTOC.json). Por ejemplo, puede usar la funcionalidad "Análisis de costos" para ver los gastos de los recursos de Azure. Para realizar el seguimiento de los gastos de Log Analytics, puede agregar un filtro por "tipo de recurso" (a microsoft.operationalinsights/workspace para Log Analytics y microsoft.operationalinsights/cluster para los clústeres de Log Analytics). En **Agrupar por**, seleccione **Categoría del medidor** o **Medidor**. Otros servicios, como Azure Defender (Security Center) y Azure Sentinel, también facturan su uso a los recursos del área de trabajo de Log Analytics. Para ver la asignación al nombre del servicio, puede seleccionar la vista de tabla en lugar de un gráfico. 

Para obtener más información sobre el uso, puede [descargar sus datos de uso desde Azure Portal](../../cost-management-billing/understand/download-azure-daily-usage.md). En la hoja de cálculo descargada, puede ver el uso por recurso de Azure (por ejemplo, área de trabajo de Log Analytics) al día. En esta hoja de cálculo de Excel, para encontrar el uso de las áreas de trabajo de Log Analytics, puede filtrar primero por la columna "Categoría del medidor" para mostrar "Log Analytics", "Insight and Analytics" (que utilizan algunos planes de tarifa heredados) y "Azure Monitor" (que utilizan los planes de tarifa de nivel de compromiso) y, a continuación, agregar un filtro en la columna "Id. de instancia", como "contiene área de trabajo" o "contiene clúster" (este último para incluir el uso de clústeres de Log Analytics). El uso se muestra en la columna "Cantidad consumida" y la unidad de cada entrada se muestra en la columna "Unidad de medida". Para comprender su factura, consulte [Examen de la factura de una suscripción individual a Azure](../../cost-management-billing/understand/review-individual-bill.md). 

## <a name="changing-pricing-tier"></a>Actualización del plan de tarifa

Para cambiar el plan de tarifa de Log Analytics del área de trabajo:

1. En Azure Portal, abra **Uso y costos estimados** en el área de trabajo, donde verá una lista de todos los planes de tarifa disponibles para esta área de trabajo.

2. Revise los costos estimados para cada uno de los planes de tarifa. Esta estimación se basa en los últimos 31 días de uso, por lo que esta estimación de costos se basa en los últimos 31 días lo que es representativo de su uso habitual. En el ejemplo siguiente puede ver cómo, en función de los patrones de datos de los últimos 31 días, esta área de trabajo costaría menos en el nivel de Pago por uso (n. 1) en comparación con el nivel de compromiso de 100 GB al día (n. 2).  

:::image type="content" source="media/manage-cost-storage/pricing-tier-estimated-costs.png" alt-text="Planes de tarifa":::
    
3. Después de revisar los costos estimados en función de los últimos 31 días de uso, si decide cambiar el plan de tarifa, elija **Seleccionar**.  

### <a name="changing-pricing-tier-via-arm"></a>Cambio del plan de tarifa vía ARM

También puede [establecer el plan de tarifa mediante Azure Resource Manager](./resource-manager-workspace.md) con el uso del objeto `sku` para establecer el plan de tarifa y el parámetro `capacityReservationLevel` si el plan de tarifa es `capacityresrvation`. (Obtenga más información sobre cómo [establecer las propiedades del área de trabajo a través de ARM](/azure/templates/microsoft.operationalinsights/2020-08-01/workspaces?tabs=json#workspacesku-object)). Esta es una plantilla de Azure Resource Manager de muestra para establecer el área de trabajo en un nivel de compromiso de 300 GB al día (en Resource Manager se denomina `capacityreservation`). 

```
{
  "$schema": https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#,
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "YourWorkspaceName",
      "type": "Microsoft.OperationalInsights/workspaces",
      "apiVersion": "2020-08-01",
      "location": "yourWorkspaceRegion",
      "properties": {
                    "sku": {
                      "name": "capacityreservation",
                      "capacityReservationLevel": 300
                    }
      }
    }
  ]
}
```

Para usar esta plantilla en PowerShell, después de [instalar el módulo de Azure Az PowerShell](/powershell/azure/install-az-ps), inicie sesión en Azure mediante `Connect-AzAccount`, seleccione la suscripción que contiene el área de trabajo mediante `Select-AzSubscription -SubscriptionId YourSubscriptionId` y aplique la plantilla (guardada en un archivo denominado template.json):

```
New-AzResourceGroupDeployment -ResourceGroupName "YourResourceGroupName" -TemplateFile "template.json"
```

Para establecer el plan de tarifa en otros valores, como Pago por uso (denominado `pergb2018` para la SKU), omita la propiedad `capacityReservationLevel`. Obtenga más información sobre [la creación de plantillas de ARM](../../azure-resource-manager/templates/template-tutorial-create-first-template.md), al [añadir un recurso a la plantilla](../../azure-resource-manager/templates/template-tutorial-add-resource.md) y al [aplicar plantillas](../resource-manager-samples.md). 

## <a name="legacy-pricing-tiers"></a>Planes de tarifa heredados

Las suscripciones que contenían un área de trabajo de Log Analytics o un recurso de Application Insights el 2 de abril de 2018, o que están vinculadas a un Contrato Enterprise anterior al 1 de febrero de 2019 y que sigue activo, seguirán teniendo acceso para usar los planes de tarifa heredados: **Evaluación gratuita**, **Independiente (por GB)** y **Por nodo (OMS)** . Las áreas de trabajo en el plan de tarifa Evaluación gratuita tendrán una ingesta diaria de datos limitada a 500 MB (excepto los tipos de datos de seguridad que recopile [Azure Defender (Security Center)](../../security-center/index.yml)) y la retención de datos se limitará a 7 días. El plan de tarifa de evaluación gratuita está destinado solo para fines de evaluación. No se proporciona ningún Acuerdo de Nivel de Servicio para el nivel Gratis.  Las áreas de trabajo en los planes de tarifa independientes o por nodo tienen una retención configurable para el usuario de 30 a 730 días.

El uso en el plan de tarifa independiente se factura por el volumen de datos ingerido. Se indica en el servicio **Log Analytics** y el medidor se denomina "Datos analizados". 

Los cargos del plan de tarifa por nodo y por VM supervisada (nodo) en una granularidad de hora. En cada nodo supervisado, se asignan al área de trabajo 500 MB de datos al día que no se facturan. Esta asignación se calcula con granularidad por hora y se agrega en el nivel de área de trabajo cada día. Los datos ingeridos por encima de la asignación de datos diaria agregada se facturan por GB como datos con un uso por encima del límite. En su factura, el servicio será **Insight and Analytics** para el uso de Log Analytics si el área de trabajo se encuentra en el plan de tarifa Por nodo. El uso se muestra en tres medidores:

- **Nodo**: uso del número de nodos supervisados (VM) en unidades de meses de nodo.
- **Data Overage per Node** (Datos por encima del límite por nodo): número de GB de datos ingeridos en exceso de la asignación de datos agregados.
- **Data Included per Node** (Datos incluidos por nodo): cantidad de datos ingeridos incluidos en la asignación de datos agregados. Este medidor también se usa cuando el área de trabajo está en todos los planes de tarifa para mostrar la cantidad de datos incluidos en Azure Defender (Security Center).

> [!TIP]
> Si el área de trabajo tiene acceso al plan de tarifa **Por nodo**, pero se pregunta si sería menos costoso un plan de tarifa de Pago por uso, puede [usar la siguiente consulta](#evaluating-the-legacy-per-node-pricing-tier) para obtener fácilmente una recomendación. 

Las áreas de trabajo creadas antes de abril de 2016 pueden seguir usando los planes de tarifa **Estándar** y **Premium** originales, que tienen una retención de datos fija de 30 y 365 días, respectivamente. No se pueden crear áreas de trabajo en los planes de tarifa **Estándar** o **Premium** y, si un área de trabajo se saca de estos niveles, no puede regresar a ellos. Los medidores de ingesta de datos en la factura de Azure para estos niveles heredados se denominan "Datos analizados".

### <a name="legacy-pricing-tiers-and-azure-defender-security-center"></a>Planes de tarifa heredados y Azure Defender (Security Center)

También hay algunos comportamientos entre el uso de los niveles de Log Analytics heredados y cómo se factura el uso en [Azure Defender (Security Center)](../../security-center/index.yml). 

- Si el área de trabajo se encuentra en el nivel heredado Estándar o Premium, Azure Defender se factura solo por la ingesta de datos de Log Analytics, no por nodo.
- Si el área de trabajo se encuentra en el nivel heredado Por nodo, Azure Defender se factura según el [modelo actual de precios basado en nodos de Azure Defender](https://azure.microsoft.com/pricing/details/security-center/). 
- En otros planes de tarifa (incluidos los niveles de compromiso), si se habilitó antes del 19 de junio de 2017, Azure Defender se factura solo por la ingesta de datos de Log Analytics. De lo contrario, Azure Defender se factura según el modelo actual de precios basado en nodos de Azure Defender.

Puede encontrar más detalles sobre las limitaciones de los planes de tarifa en [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md#log-analytics-workspaces).

Ninguno de los planes de tarifa heredados tiene precios basados en la región.  

> [!NOTE]
> Para usar los derechos que proceden de la adquisición de OMS E1 Suite, OMS E2 Suite o un complemento de OMS para System Center, elija el plan de tarifa *Por nodo* de Log Analytics.

## <a name="log-analytics-and-azure-defender-security-center"></a>Log Analytics y Azure Defender (Security Center)

La facturación de [Azure Defender para servidores (Security Center)](../../security-center/index.yml) está estrechamente vinculada a la facturación de Log Analytics. Azure Defender [factura por el número de servicios supervisados](https://azure.microsoft.com/pricing/details/azure-defender/) y proporciona una asignación de 500 MB por servidor al día que se aplica al subconjunto siguiente de [tipos de datos de seguridad](/azure/azure-monitor/reference/tables/tables-category#security) (WindowsEvent, SecurityAlert, SecurityBaseline, SecurityBaselineSummary, SecurityDetection, SecurityEvent, WindowsFirewall, MaliciousIPCommunication, LinuxAuditLog, SysmonEvent, ProtectionStatus) y los tipos de datos Update y UpdateSummary cuando la solución Update Management no se está ejecutando en el área de trabajo o el destino de la solución está habilitado ([más información](../../security-center/security-center-pricing.md#what-data-types-are-included-in-the-500-mb-data-daily-allowance)). El recuento de servidores supervisados se calcula según una granularidad por hora. Las contribuciones diarias de asignación de datos de cada servidor supervisado se agregan en el nivel de área de trabajo. Si el área de trabajo está en el plan de tarifa heredado por nodo, las asignaciones de Azure Defender y Log Analytics se combinan y se aplican conjuntamente a todos los datos ingeridos facturables.  

## <a name="change-the-data-retention-period"></a>Cambio del período de retención de datos

Los pasos siguientes describen cómo configurar cuánto tiempo se conservan los datos de registro en el área de trabajo. La retención de datos en el nivel de área de trabajo se puede configurar entre 30 y 730 días (2 años) para todas las áreas de trabajo a menos que usen el plan de tarifa heredado Evaluación gratuita. La retención de tipos de datos individuales se puede establecer en un valor mínimo de 4 días. [Obtenga más información](https://azure.microsoft.com/pricing/details/monitor/) sobre los precios de la retención de datos de mayor duración. Para conservar los datos de más de 730 días, considere la posibilidad de usar la [exportación de datos del área de trabajo de Log Analytics](logs-data-export.md).

### <a name="workspace-level-default-retention"></a>Retención predeterminada de nivel de área de trabajo

Para establecer la retención predeterminada del área de trabajo:
 
1. En Azure Portal, en el área de trabajo, seleccione **Uso y costos estimados** en el panel izquierdo.
2. En la página **Uso y costos estimados**, seleccione **Retención de datos** en la parte superior de la página.
3. En el panel, mueva el control deslizante para aumentar o disminuir el número de días y, después, haga clic en **Aceptar**.  Si utiliza el nivel *gratuito*, no puede modificar el período de retención de datos y debe actualizar al nivel de pago para controlar esta configuración.

:::image type="content" source="media/manage-cost-storage/manage-cost-change-retention-01.png" alt-text="Cambio de la configuración de retención de datos del área de trabajo":::

Cuando se reduce la retención, hay un período de gracia de varios días antes de que se quiten los datos anteriores a la nueva configuración de retención. 

La página **Retención de datos** permite la configuración de una retención de 30, 31, 60, 90, 120, 180, 270, 365, 550 y 730 días. Si se requiere otra configuración, se puede configurar mediante [Azure Resource Manager](./resource-manager-workspace.md) con el parámetro `retentionInDays`. Al configurar la retención de datos en 30 días, puede desencadenar una purga inmediata de los datos más antiguos mediante el parámetro `immediatePurgeDataOn30Days`, que elimina el período de gracia. Esto puede ser útil para escenarios relacionados con el cumplimiento en los que la eliminación de datos inmediata es imperativa. Esta funcionalidad de purga inmediata solo está expuesta a través de Azure Resource Manager. 

Las áreas de trabajo con una retención de 30 días en realidad pueden conservar los datos durante 31 días. Si es imperativo que los datos se conserven solo durante 30 días, use Azure Resource Manager para establecer la retención en 30 días y con el parámetro `immediatePurgeDataOn30Days`.  

De manera predeterminada, dos tipos de datos (`Usage` y `AzureActivity`) se conservan durante un mínimo de 90 días sin ningún cargo. Si la retención del área de trabajo supera los 90 días, también aumentará la retención de estos tipos de datos. Estos tipos de datos también están libres de los cargos de ingesta de datos. 

Los tipos de datos de los recursos de Application Insights basados en áreas de trabajo (`AppAvailabilityResults`, `AppBrowserTimings`, `AppDependencies`, `AppExceptions`, `AppEvents`, `AppMetrics`, `AppPageViews`, `AppPerformanceCounters`, `AppRequests`, `AppSystemEvents` y `AppTraces`) también se conservan durante 90 días sin ningún cargo de manera predeterminada. Su retención se puede ajustar mediante la funcionalidad de retención por tipo de datos. 

La [API de purga](/rest/api/loganalytics/workspacepurge/purge) de Log Analytics no afecta a la facturación de la retención y está pensada para usarse en casos muy limitados. Para reducir la factura de retención, el período de retención se debe reducir para el área de trabajo o para tipos de datos específicos. 

### <a name="retention-by-data-type"></a>Retención por tipo de datos

También es posible especificar diferentes configuraciones de retención para tipos de datos individuales de 4 a 730 días (excepto para áreas de trabajo del plan de tarifa heredado Evaluación gratuita), que sobrescriben la retención predeterminada del nivel de área de trabajo. Cada tipo de datos es un subrecurso del área de trabajo. Por ejemplo, la tabla SecurityEvent se puede tratar en [Azure Resource Manager](../../azure-resource-manager/management/overview.md) como:

```
/subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent
```

Tenga en cuenta que el tipo de datos (tabla) distingue mayúsculas de minúsculas. Para obtener la configuración actual de retención por tipo de datos de un tipo de datos determinado (en este ejemplo, SecurityEvent), use:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
```

> [!NOTE]
> Solo se devuelve la retención de un tipo de datos si está establecida explícitamente para dicho tipo de datos. Los tipos de datos que no tiene la retención establecida explícitamente (y que, por tanto, heredan la retención del área de trabajo) no devuelven nada en esta llamada. 

Para obtener la configuración actual de retención por tipo de datos para todos los tipos de datos del área de trabajo que tienen establecida esta configuración, solo tiene que omitir el tipo de datos específico, por ejemplo:

```JSON
    GET /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables?api-version=2017-04-26-preview
```

Para establecer la retención de un tipo de datos determinado (en este ejemplo, SecurityEvent) en 730 días, use lo siguiente:

```JSON
    PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview
    {
        "properties": 
        {
            "retentionInDays": 730
        }
    }
```

Los valores válidos para `retentionInDays` oscilan entre 4 y 730.

Los tipos de datos `Usage` y `AzureActivity` no se pueden establecer con la retención personalizada. Adoptan el máximo de la retención predeterminada del área de trabajo o 90 días. 

Una herramienta excelente para conectarse directamente a Azure Resource Manager para establecer la retención por tipo de datos es la herramienta OSS [ARMclient](https://github.com/projectkudu/ARMClient).  Obtenga más información sobre ARMClient en los artículos de [David Ebbo](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) y Daniel Bowbyes.  A continuación, se muestra un ejemplo de uso de ARMClient, estableciendo los datos de SecurityEvent con una retención de 730 días:

```
armclient PUT /subscriptions/00000000-0000-0000-0000-00000000000/resourceGroups/MyResourceGroupName/providers/Microsoft.OperationalInsights/workspaces/MyWorkspaceName/Tables/SecurityEvent?api-version=2017-04-26-preview "{properties: {retentionInDays: 730}}"
```

> [!TIP]
> El establecimiento de la retención de tipos de datos individuales se puede usar para reducir los costos de la retención de datos. Para los datos recopilados a partir de octubre de 2019 (fecha de publicación de esta característica), la reducción de la retención para algunos tipos de datos puede reducir el costo de retención con el tiempo. Para los datos recopilados anteriormente, el establecimiento de una retención más baja para un tipo individual no afectará a los costos de retención.  

## <a name="manage-your-maximum-daily-data-volume"></a>Administración del volumen de datos diario máximo

Puede configurar un límite diario y limitar la ingesta diaria para el área de trabajo, pero tenga cuidado, ya que su objetivo no debe ser alcanzar el límite diario. En caso contrario, perderá los datos para el resto del día, lo que puede afectar a otros servicios y soluciones de Azure cuya funcionalidad puede depender de la disponibilidad de datos actualizados en el área de trabajo. Como resultado, esto afecta a la capacidad de observar y recibir alertas cuando cambian las condiciones de mantenimiento de los recursos que respaldan los servicios de TI. El límite diario está pensado para usarse como una manera de administrar un **aumento inesperado** del volumen de datos de los recursos administrados y permanecer dentro de su límite, o simplemente cuando quiera limitar los cargos no planeados para el área de trabajo. No es adecuado para establecer un límite diario para que se cumpla cada día en un área de trabajo.

A cada área de trabajo se le aplica su límite diario en una hora diferente del día. La hora de restablecimiento se muestra en la página **Límite diario** (consulte más adelante). Esta hora de restablecimiento no se puede configurar. 

Poco después de alcanzar el límite diario, la recopilación de tipos de datos facturables se detiene durante el resto del día. La latencia inherente a la aplicación del límite diario significa que el límite no se aplica con precisión al nivel de límite diario especificado. Un mensaje emergente de advertencia aparece en la parte superior de la página del área de trabajo de Log Analytics seleccionada, y se envía un evento de operación a la tabla *Operación* en la categoría **LogManagement**. La recopilación de datos se reanuda después de que se restablezca el tiempo definido en *Daily limit will be set at* (El límite diario se establecerá en). Se recomienda definir una regla de alerta basada en este evento de operación que esté configurada para notificar cuando se alcanza el límite diario de datos. Para más información, consulte la sección [Alerta cuando se alcanza el límite diario](#alert-when-daily-cap-is-reached). 

> [!NOTE]
> El límite diario no puede detener la recopilación de datos con precisión en el nivel de límite especificado y pueden esperarse datos sobrantes, especialmente si el área de trabajo recibe grandes volúmenes de datos. Si se recopilan datos por encima del límite, se siguen facturando. Para obtener una consulta útil para estudiar el comportamiento del límite diario, consulte la sección [Visualización del efecto del límite diario](#view-the-effect-of-the-daily-cap) de este artículo. 

> [!WARNING]
> El límite diario no detiene la recopilación de los tipos de datos **WindowsEvent**, **SecurityAlert**, **SecurityBaseline**, **SecurityBaselineSummary**, **SecurityDetection**, **SecurityEvent**, **WindowsFirewall**, **MaliciousIPCommunication**, **LinuxAuditLog**, **SysmonEvent**, **ProtectionStatus**, **Update** y **UpdateSummary**, excepto en el caso de las áreas de trabajo en las que se instaló Azure Defender (Security Center) antes del 19 de junio de 2017. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identificación del límite diario de datos para definir

Revise [Uso y costos estimados de Log Analytics](../usage-estimated-costs.md) para comprender la tendencia de ingesta de datos y el límite de volumen diario que se debe definir. Revíselo con cuidado, ya que no puede supervisar los recursos una vez alcanzado el límite. 

### <a name="set-the-daily-cap"></a>Establecimiento del límite diario

Los pasos siguientes describen cómo configurar un límite para administrar el volumen de datos que el área de trabajo de Log Analytics ingiere por día.  

1. Desde el área de trabajo, seleccione **Uso y costos estimados** en el panel izquierdo.
2. En la página **Uso y costos estimados** del área de trabajo seleccionada, seleccione **Límite diario** en la parte superior de la página. 
3. De forma predeterminada, la opción **Límite diario** está establecida en **OFF** (Desactivado). Para habilitarla, seleccione **ON** (Activado) y, luego, establezca el límite de volumen datos en GB/día.

:::image type="content" source="media/manage-cost-storage/set-daily-volume-cap-01.png" alt-text="Configuración del límite de datos con Log Analytics":::
    
Puede usar Azure Resource Manager para configurar el límite diario. Para configurarlo, establezca el parámetro `dailyQuotaGb` en `WorkspaceCapping`, como se describe en [Áreas de trabajo: creación o actualización](/rest/api/loganalytics/workspaces/createorupdate#workspacecapping). 

Puede realizar un seguimiento de los cambios realizados en el límite diario mediante esta consulta:

```kusto
_LogOperation | where Operation == "Workspace Configuration" | where Detail contains "Daily quota"
```

Obtenga más información sobre la función [_LogOperation](./monitor-workspace.md). 

### <a name="view-the-effect-of-the-daily-cap"></a>Visualización del efecto del límite diario

Para ver el efecto del límite diario, es importante tener en cuenta los tipos de datos de seguridad no incluidos en el límite diario y la hora de restablecimiento del área de trabajo. La hora de restablecimiento del límite diario se muestra en la página **Límite diario**. La siguiente consulta se puede usar para realizar el seguimiento de los volúmenes de datos sujetos al límite diario entre restablecimientos del límite diario. En este ejemplo, la hora de restablecimiento del área de trabajo es las 14:00 horas. Deberá actualizarla para el área de trabajo.

```kusto
let DailyCapResetHour=14;
Usage
| where Type !in ("SecurityAlert", "SecurityBaseline", "SecurityBaselineSummary", "SecurityDetection", "SecurityEvent", "WindowsFirewall", "MaliciousIPCommunication", "LinuxAuditLog", "SysmonEvent", "ProtectionStatus", "WindowsEvent")
| extend TimeGenerated=datetime_add("hour",-1*DailyCapResetHour,TimeGenerated)
| where TimeGenerated > startofday(ago(31d))
| where IsBillable
| summarize IngestedGbBetweenDailyCapResets=sum(Quantity)/1000. by day=bin(TimeGenerated, 1d) | render areachart  
```

(En el tipo de datos de uso, las unidades de `Quantity` están en MB).

### <a name="alert-when-daily-cap-is-reached"></a>Alerta cuando se alcanza el límite diario

Aunque Azure Portal presenta una indicación visual cuando se alcanza el umbral de límite de datos, este comportamiento no tiene que alinearse necesariamente con la forma en que el usuario administra los problemas de funcionamiento que requieren atención inmediata. Para recibir una notificación de alerta, puede crear una nueva regla de alerta en Azure Monitor. Para obtener más información, consulte [Cómo crear, ver y administrar alertas](../alerts/alerts-metric.md).

Para empezar, esta es la configuración recomendada para la alerta al consultar la tabla `Operation` con la función `_LogOperation` ([obtenga más información](./monitor-workspace.md)). 

- Destino: seleccione el recurso de Log Analytics
- Criterios: 
   - Nombre de señal: Búsqueda de registros personalizada
   - Consulta de búsqueda: `_LogOperation | where Operation =~ "Data collection stopped" | where Detail contains "OverQuota"`
   - Basado en: Número de resultados
   - Condición: Mayor que
   - Umbral: 0
   - Período: 5 (minutos)
   - Frecuencia: 5 (minutos)
- Nombre de regla de alertas: Límite de datos diario alcanzado
- Gravedad: advertencia (gravedad 1)

Una vez que se define la alerta y se alcanza el límite, se desencadena una alerta que realiza la respuesta definida en el grupo de acciones. Puede notificar al equipo de las siguientes maneras:

- Mensajes de texto y de correo electrónico
- Acciones automatizadas mediante webhooks
- Runbooks de Azure Automation
- [Integrado con una solución ITSM externa](../alerts/itsmc-definition.md#create-itsm-work-items-from-azure-alerts). 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Solución del problema de un uso mayor de lo esperado

Un mayor uso está provocado por una de estas causas o por ambas:
- Más nodos de lo previsto que envían datos al área de trabajo de Log Analytics. Para obtener información, consulte la sección [Descripción de los nodos que envían datos](#understanding-nodes-sending-data) de este artículo.
- Se enviaron más datos de los esperados al área de trabajo de Log Analytics (quizás porque se empezó a usar una solución nueva o debido a un cambio de configuración en una solución existente). Para obtener información, consulte la sección [Descripción del volumen de datos ingeridos](#understanding-ingested-data-volume) de este artículo.

Si observa que se notifica una ingesta de datos elevada mediante los registros `Usage` (consulte [Volumen de datos por solución](#data-volume-by-solution) a continuación), pero no observa los mismos resultados al agregar `_BilledSize` directamente en el [tipo de datos](#data-volume-for-specific-events), es posible que datos importantes estén llegando tarde. Para obtener información sobre cómo realizar este diagnóstico, consulte la sección [Datos de llegada tardía](#late-arriving-data) de este artículo. 

### <a name="log-analytics-workspace-insights"></a>Conclusiones del área de trabajo de Log Analytics

Empiece a comprender los volúmenes de datos en la pestaña **Uso** del [libro Conclusiones del área de trabajo de Log Analytics](log-analytics-workspace-insights-overview.md). En el **panel de uso** puede ver fácilmente lo siguiente:
- ¿Qué tablas de datos ingieren el mayor volumen de datos de la tabla principal?  
- ¿Cuáles son los principales recursos que contribuyen a los datos? 
- ¿Cuál es la tendencia de ingesta de datos?

Puede cambiar a **consultas adicionales** para ejecutar fácilmente más consultas útiles para comprender los patrones de datos. 

Obtenga más información sobre las [funcionalidades de la pestaña Uso](log-analytics-workspace-insights-overview.md#usage-tab). 

Aunque este libro puede responder a muchas de las preguntas sin necesidad de ejecutar una consulta, para responder a preguntas más específicas o realizar análisis más profundos, las consultas de las dos secciones siguientes le ayudarán a empezar. 

## <a name="understanding-nodes-sending-data"></a>Descripción de nodos que envían datos

Para conocer el número de nodos que notificaron latidos del agente todos los días del último mes, use esta consulta:

```kusto
Heartbeat 
| where TimeGenerated > startofday(ago(31d))
| summarize nodes = dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart
```
Para obtener un recuento de los nodos que han enviado datos en las últimas 24 horas, use esta consulta: 

```kusto
find where TimeGenerated > ago(24h) project Computer
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodes = dcount(computerName)
```

Para obtener una lista de los nodos que envían datos (y la cantidad de datos que envía cada uno), use esta consulta:

```kusto
find where TimeGenerated > ago(24h) project _BilledSize, Computer
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

### <a name="nodes-billed-by-the-legacy-per-node-pricing-tier"></a>Nodos facturados según el plan de tarifa heredado por nodo

El [plan de tarifa heredado Por nodo](#legacy-pricing-tiers) factura por los nodos con granularidad por hora y tampoco cuenta los nodos que solo envían un conjunto de tipos de datos de seguridad. Su recuento diario de nodos se aproximará a la siguiente consulta:

```kusto
find where TimeGenerated >= startofday(ago(7d)) and TimeGenerated < startofday(now()) project Computer, _IsBillable, Type, TimeGenerated
| where Type !in ("SecurityAlert", "SecurityBaseline", "SecurityBaselineSummary", "SecurityDetection", "SecurityEvent", "WindowsFirewall", "MaliciousIPCommunication", "LinuxAuditLog", "SysmonEvent", "ProtectionStatus", "WindowsEvent")
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| where _IsBillable == true
| summarize billableNodesPerHour=dcount(computerName) by bin(TimeGenerated, 1h)
| summarize billableNodesPerDay = sum(billableNodesPerHour)/24., billableNodeMonthsPerDay = sum(billableNodesPerHour)/24./31.  by day=bin(TimeGenerated, 1d)
| sort by day asc
```

El número de unidades de la factura está en unidades de meses de nodo, que se representa mediante `billableNodeMonthsPerDay` en la consulta. Si el área de trabajo tiene instalada la solución Update Management, agregue los tipos de datos **Update** y **UpdateSummary** a la lista en la cláusula WHERE de la consulta anterior. Por último, hay cierta complejidad adicional en el algoritmo de facturación real cuando se usa un destino de la solución que no está representado en la consulta anterior. 


> [!TIP]
> Use estas consultas `find` con moderación, ya que la ejecución de exámenes entre tipos de datos [consume muchos recursos](./query-optimization.md#query-performance-pane). Si no necesita resultados **por equipo**, consulte el tipo de datos **Uso** (vea a continuación).

## <a name="understanding-ingested-data-volume"></a>Descripción del volumen de datos ingeridos

En la página **Uso y costos estimados**, el gráfico *Ingesta de datos por solución*  muestra el volumen total de los datos enviados y la cantidad que envía cada solución. Esto le permite determinar tendencias tales como si el uso global de los datos (o el uso de una solución en particular) crece, permanece constante o disminuye. 

### <a name="data-volume-for-specific-events"></a>Volumen de datos para eventos específicos

Para ver el tamaño de los datos ingeridos de un conjunto determinado de eventos, puede consultar la tabla específica (en este ejemplo, `Event`) y, a continuación, restringir la consulta a los eventos de interés (en este ejemplo, identificador de evento 5145 o 5156):

```kusto
Event
| where TimeGenerated > startofday(ago(31d)) and TimeGenerated < startofday(now()) 
| where EventID == 5145 or EventID == 5156
| where _IsBillable == true
| summarize count(), Bytes=sum(_BilledSize) by EventID, bin(TimeGenerated, 1d)
``` 

Tenga en cuenta que la cláusula `where _IsBillable = true` filtra los tipos de datos de determinadas soluciones para las que no hay ningún cargo de ingesta. [Más información](./log-standard-columns.md#_isbillable) sobre `_IsBillable`.

### <a name="data-volume-by-solution"></a>Data volume by solution (Volumen de datos por solución)

La consulta que se usa para ver el volumen de datos facturable por solución en el último mes (excepto el último día parcial) se puede compilar mediante el tipo de datos [Uso](/azure/azure-monitor/reference/tables/usage), como se indica a continuación:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), Solution 
| render columnchart
```

La cláusula con `TimeGenerated` solo se utiliza para garantizar que la experiencia de consulta en Azure Portal examina más allá del período predeterminado de 24 horas. Al utilizar el tipo de datos **Uso**, `StartTime` y `EndTime` representan los períodos de tiempo de los que se presentan resultados. 

### <a name="data-volume-by-type"></a>Volumen de datos por tipo

Puede profundizar más para ver las tendencias de datos por tipo de datos:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), DataType 
| render columnchart
```

O bien, para ver una tabla por solución y tipo durante el último mes,

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000 by Solution, DataType
| sort by Solution asc, DataType asc
```

### <a name="data-volume-by-computer"></a>Volumen de datos por equipo

El tipo de datos **Uso** no incluye información en el nivel de equipo. Para ver el **tamaño** de los datos facturables ingeridos por equipo, use la [propiedad](./log-standard-columns.md#_billedsize) **_BilledSize**, que proporciona el tamaño en bytes:

```kusto
find where TimeGenerated > ago(24h) project _BilledSize, _IsBillable, Computer, Type
| where _IsBillable == true and Type != "Usage"
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize BillableDataBytes = sum(_BilledSize) by  computerName 
| sort by BillableDataBytes desc nulls last
```

La [propiedad](./log-standard-columns.md#_isbillable) **_IsBillable** especifica si los datos ingeridos generarán cargos. El tipo **Uso** se omite, ya que es solo para el análisis de tendencias de datos. 

Para ver el **recuento** de eventos facturables que ingirió cada equipo, use: 

```kusto
find where TimeGenerated > ago(24h) project _IsBillable, Computer
| where _IsBillable == true and Type != "Usage"
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| summarize eventCount = count() by computerName  
| sort by eventCount desc nulls last
```

> [!TIP]
> Use estas consultas `find` con moderación, ya que la ejecución de exámenes entre tipos de datos [consume muchos recursos](./query-optimization.md#query-performance-pane). Si no necesita resultados **por equipo**, consulte el tipo de datos **Uso**.

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Volumen de datos por recurso de Azure, grupo de recursos o suscripción

Para los datos de los nodos hospedados en Azure, puede obtener el **tamaño** de los eventos ingeridos __por equipo__; para ello, use la [propiedad _ResourceId](./log-standard-columns.md#_resourceid), que proporciona la ruta completa al recurso:

```kusto
find where TimeGenerated > ago(24h) project _ResourceId, _BilledSize, _IsBillable
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId | sort by BillableDataBytes nulls last
```

Para los datos de los nodos hospedados en Azure, puede obtener el **tamaño** de los datos ingeridos __por suscripción de Azure__; para ello, use la propiedad **_SubscriptionId** de la siguiente manera:

```kusto
find where TimeGenerated > ago(24h) project _BilledSize, _IsBillable, _SubscriptionId
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _SubscriptionId | sort by BillableDataBytes nulls last
```

Para obtener el volumen de datos por grupo de recursos, puede analizar la propiedad **_ResourceId**:

```kusto
find where TimeGenerated > ago(24h) project _ResourceId, _BilledSize, _IsBillable
| where _IsBillable == true 
| summarize BillableDataBytes = sum(_BilledSize) by _ResourceId
| extend resourceGroup = tostring(split(_ResourceId, "/")[4] )
| summarize BillableDataBytes = sum(BillableDataBytes) by resourceGroup | sort by BillableDataBytes nulls last
```

Si es necesario, también puede analizar la propiedad **_ResourceId** de manera más completa:

```Kusto
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
```

> [!TIP]
> Use estas consultas `find` con moderación, ya que la ejecución de exámenes entre tipos de datos [consume muchos recursos](./query-optimization.md#query-performance-pane). Si no necesita resultados por suscripción, grupo de recursos o nombre de recurso, consulte el tipo de datos **Uso**.

> [!WARNING]
> Algunos de los campos del tipo de datos **Uso**, aunque siguen en el esquema, han quedado en desuso y ya no se rellenan sus valores. Estos son **Computer**, además de los campos relacionados con la ingesta (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** y **AverageProcessingTimeMs**).

## <a name="late-arriving-data"></a>Datos de llegada tardía   

Pueden surgir situaciones en las que los datos se ingieren con marcas de tiempo antiguas. Por ejemplo, si un agente no puede comunicarse con Log Analytics debido a una incidencia de conectividad o cuando un host tiene una fecha u hora incorrecta. Esto puede manifestarse a través de una discrepancia aparente entre los datos ingeridos notificados por el tipo de datos **Uso** y una consulta que agrega la propiedad **_BilledSize** con los datos sin procesar de un día determinado especificado por **TimeGenerated**, la marca de tiempo cuando se generó el evento.

Para diagnosticar las incidencias de datos que llegan tarde, use la columna **_TimeReceived** ([más información](./log-standard-columns.md#_timereceived)), además de la columna **TimeGenerated**. **_TimeReceived** es la hora en que el punto de ingesta de Azure Monitor recibió el registro en la nube de Azure. Por ejemplo, al usar los registros de **Uso**, ha observado grandes volúmenes de datos **W3CIISLog** ingeridos el 2 de mayo de 2021. A continuación, se incluye una consulta que identifica las marcas de tiempo de estos datos ingeridos: 

```Kusto
W3CIISLog
| where TimeGenerated > datetime(1970-01-01)
| where _TimeReceived >= datetime(2021-05-02) and _TimeReceived < datetime(2021-05-03) 
| where _IsBillable == true
| summarize BillableDataMB = sum(_BilledSize)/1.E6 by bin(TimeGenerated, 1d)
| sort by TimeGenerated asc 
```

La instrucción `where TimeGenerated > datetime(1970-01-01)` está presente solo para proporcionar la pista a la interfaz de usuario de Log Analytics para buscar entre todos los datos.  

## <a name="querying-for-common-data-types"></a>Consulta de los tipos de datos comunes

Para profundizar en el origen de datos de un tipo de datos concreto, estas son algunas consultas de ejemplo útiles:

+ Recursos de **Application Insights basados en áreas de trabajo**
  - Obtenga más información en [Administración del uso y los costos de Application Insights](../app/pricing.md#data-volume-for-workspace-based-application-insights-resources).
+ Solución **Security**
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ Solución **Log Management**
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ Tipo de datos **Perf**
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ Tipo de datos **Event**
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ Tipo de datos **Syslog**
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ Tipo de datos de **AzureDiagnostics**
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

## <a name="tips-for-reducing-data-volume"></a>Sugerencias para reducir el volumen de datos

En esta tabla se incluyen algunas sugerencias para reducir el volumen de registros recopilados.

| Origen del mayor volumen de datos | Cómo reducir el volumen de datos |
| -------------------------- | ------------------------- |
| Reglas de recopilación de datos      | El [agente de Azure Monitor](../agents/azure-monitor-agent-overview.md) usa reglas de recopilación de datos para administrar la recopilación de datos. Puede [limitar la recopilación de datos](../agents/data-collection-rule-azure-monitor-agent.md#limit-data-collection-with-custom-xpath-queries) mediante consultas XPath personalizadas. | 
| Container Insights         | [Configure Container Insights](../containers/container-insights-cost.md#controlling-ingestion-to-reduce-cost) para recopilar solo los datos necesarios. |
| Azure Sentinel | Revise los [orígenes de datos de Sentinel](../../sentinel/connect-data-sources.md) que ha habilitado recientemente como orígenes del volumen de datos adicionales. [Obtenga más información](../../sentinel/azure-sentinel-billing.md) sobre los costos y la facturación de Sentinel. |
| Eventos de seguridad            | Seleccione los [eventos de seguridad común o mínima](../../security-center/security-center-enable-data-collection.md#data-collection-tier). <br> Cambie la directiva de auditoría de seguridad para recopilar únicamente los eventos necesarios. En particular, revise la necesidad de recopilar eventos para: <br> - [Auditar la plataforma de filtrado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772749(v=ws.10)). <br> - [Auditar el Registro](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10)). <br> - [Auditar el sistema de archivos](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10)). <br> - [Auditar el objeto de kernel](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10)). <br> - [Auditar la manipulación de identificadores](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10)). <br> - Auditar el almacenamiento extraíble. |
| Contadores de rendimiento       | Cambie la [configuración de los contadores de rendimiento](../agents/data-sources-performance-counters.md) para: <br> - Reducir la frecuencia de recopilación. <br> - Reducir el número de contadores de rendimiento. |
| Registros de eventos                 | Cambie la [configuración del registro de eventos](../agents/data-sources-windows-events.md) para: <br> - Reducir el número de registros de eventos recopilados. <br> - Recopilar solo los niveles de eventos necesarios Por ejemplo, no recopile eventos de nivel de *información*. |
| syslog                     | Cambie la [configuración de syslog](../agents/data-sources-syslog.md) para: <br> - Reducir el número de instalaciones recopiladas. <br> - Recopilar solo los niveles de eventos necesarios Por ejemplo, no recopile eventos de nivel de *información* y *depuración*. |
| AzureDiagnostics           | Cambie la [recopilación de registros de recursos](../essentials/diagnostic-settings.md#create-in-azure-portal) para: <br> - Reducir el número de recursos que envían registros a Log Analytics. <br> - Recopilar solo los registros necesarios. |
| Datos de la solución procedentes de equipos que no necesitan la solución | Use la [selección de destino de solución](../insights/solution-targeting.md) para recopilar datos solo de los grupos de equipos necesarios. |
| Application Insights | Revise las opciones para [administrar el volumen de datos de Application Insights](../app/pricing.md#managing-your-data-volume). |
| [SQL Analytics](../insights/azure-sql.md) | Use [Set-AzSqlServerAudit](/powershell/module/az.sql/set-azsqlserveraudit) para ajustar la configuración de auditoría. |

### <a name="getting-nodes-as-billed-in-the-per-node-pricing-tier"></a>Obtención de nodos facturados en el plan de tarifa Por nodo

Para obtener una lista de los equipos que se facturarán como nodos si el área de trabajo se encuentra en el plan de tarifa heredado Por nodo, busque los nodos que envían **tipos de datos facturados** (algunos tipos de datos son gratuitos). Para hacerlo, use la [propiedad _IsBillable](./log-standard-columns.md#_isbillable) y el campo situado más a la izquierda del nombre de dominio completo. De este modo se devuelve el recuento de equipos con datos facturados por hora (que es la granularidad con la que se contabilizan y se facturan los nodos):

```kusto
find where TimeGenerated > ago(24h) project Computer, TimeGenerated
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize billableNodes=dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

### <a name="getting-security-and-automation-node-counts"></a>Obtener recuentos de nodos de seguridad y automatización

Para ver el número de nodos de seguridad distintos, puede utilizar la consulta:

```kusto
union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count
```

Para ver el número de nodos de Automation distintos, utilice la consulta:

```kusto
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="evaluating-the-legacy-per-node-pricing-tier"></a>Evaluación del plan de tarifa por nodo heredado

La decisión de si las áreas de trabajo con acceso a los planes de tarifas **Por nodo** heredados están mejor en ese plan o en otro de **Pago por uso** o de **nivel de compromiso** suele resultar difícil de evaluar para los clientes.  Implica conocer el equilibrio entre el costo fijo por nodo supervisado en el plan de tarifa por nodo y su asignación de datos incluida de 500 MB/nodo/día, y el costo que supone pagar solo por los datos ingeridos en la tarifa de pago por uso (por GB). 

Para facilitar esta valoración, se puede usar la consulta siguiente a fin de realizar una recomendación para el plan de tarifa óptimo en función de los patrones de uso de un área de trabajo. Esta consulta examina los nodos supervisados y los datos ingeridos en un área de trabajo los últimos siete días y evalúa para cada día qué plan de tarifa habría sido el óptimo. Para usar la consulta, debe hacer lo siguiente:

- Especificar si el área de trabajo usa Azure Defender (Security Center). Para ello, establezca **workspaceHasSecurityCenter** en **true** o **false**. 
- Actualizar los precios si tiene descuentos específicos.
- Especificar el número de días anteriores que se van a buscar y analizar. Para ello, establezca **daysToEvaluate**. Esto resulta útil si la consulta tarda demasiado en buscar datos de 7 días. 

Esta es la consulta de recomendación del nivel de precios:

```kusto
// Set these parameters before running query
// Pricing details available at https://azure.microsoft.com/pricing/details/monitor/
let daysToEvaluate = 7; // Enter number of previous days to analyze (reduce if the query is taking too long)
let workspaceHasSecurityCenter = false;  // Specify if the workspace has Azure Security Center
let PerNodePrice = 15.; // Enter your montly price per monitored nodes
let PerNodeOveragePrice = 2.30; // Enter your price per GB for data overage in the Per Node pricing tier
let PerGBPrice = 2.30; // Enter your price per GB in the Pay-as-you-go pricing tier
let CommitmentTier100Price = 196.; // Enter your price for the 100 GB/day commitment tier
let CommitmentTier200Price = 368.; // Enter your price for the 200 GB/day commitment tier
let CommitmentTier300Price = 540.; // Enter your price for the 300 GB/day commitment tier
let CommitmentTier400Price = 704.; // Enter your price for the 400 GB/day commitment tier
let CommitmentTier500Price = 865.; // Enter your price for the 500 GB/day commitment tier
let CommitmentTier1000Price = 1700.; // Enter your price for the 1000 GB/day commitment tier
let CommitmentTier2000Price = 3320.; // Enter your price for the 2000 GB/day commitment tier
let CommitmentTier5000Price = 8050.; // Enter your price for the 5000 GB/day commitment tier
// ---------------------------------------
let SecurityDataTypes=dynamic(["SecurityAlert", "SecurityBaseline", "SecurityBaselineSummary", "SecurityDetection", "SecurityEvent", "WindowsFirewall", "MaliciousIPCommunication", "LinuxAuditLog", "SysmonEvent", "ProtectionStatus", "WindowsEvent", "Update", "UpdateSummary"]);
let StartDate = startofday(datetime_add("Day",-1*daysToEvaluate,now()));
let EndDate = startofday(now());
union * 
| where TimeGenerated >= StartDate and TimeGenerated < EndDate
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize nodesPerHour = dcount(computerName) by bin(TimeGenerated, 1h)  
| summarize nodesPerDay = sum(nodesPerHour)/24.  by day=bin(TimeGenerated, 1d)  
| join kind=leftouter (
    Heartbeat 
    | where TimeGenerated >= StartDate and TimeGenerated < EndDate
    | where Computer != ""
    | summarize ASCnodesPerHour = dcount(Computer) by bin(TimeGenerated, 1h) 
    | extend ASCnodesPerHour = iff(workspaceHasSecurityCenter, ASCnodesPerHour, 0)
    | summarize ASCnodesPerDay = sum(ASCnodesPerHour)/24.  by day=bin(TimeGenerated, 1d)   
) on day
| join (
    Usage 
    | where TimeGenerated >= StartDate and TimeGenerated < EndDate
    | where IsBillable == true
    | extend NonSecurityData = iff(DataType !in (SecurityDataTypes), Quantity, 0.)
    | extend SecurityData = iff(DataType in (SecurityDataTypes), Quantity, 0.)
    | summarize DataGB=sum(Quantity)/1000., NonSecurityDataGB=sum(NonSecurityData)/1000., SecurityDataGB=sum(SecurityData)/1000. by day=bin(StartTime, 1d)  
) on day
| extend AvgGbPerNode =  NonSecurityDataGB / nodesPerDay
| extend OverageGB = iff(workspaceHasSecurityCenter, 
             max_of(DataGB - 0.5*nodesPerDay - 0.5*ASCnodesPerDay, 0.), 
             max_of(DataGB - 0.5*nodesPerDay, 0.))
| extend PerNodeDailyCost = nodesPerDay * PerNodePrice / 31. + OverageGB * PerNodeOveragePrice
| extend billableGB = iff(workspaceHasSecurityCenter,
             (NonSecurityDataGB + max_of(SecurityDataGB - 0.5*ASCnodesPerDay, 0.)), DataGB )
| extend PerGBDailyCost = billableGB * PerGBPrice
| extend CommitmentTier100DailyCost = CommitmentTier100Price + max_of(billableGB - 100, 0.)* CommitmentTier100Price/100.
| extend CommitmentTier200DailyCost = CommitmentTier200Price + max_of(billableGB - 200, 0.)* CommitmentTier200Price/200.
| extend CommitmentTier300DailyCost = CommitmentTier300Price + max_of(billableGB - 300, 0.)* CommitmentTier300Price/300.
| extend CommitmentTier400DailyCost = CommitmentTier400Price + max_of(billableGB - 400, 0.)* CommitmentTier400Price/400.
| extend CommitmentTier500DailyCost = CommitmentTier500Price + max_of(billableGB - 500, 0.)* CommitmentTier500Price/500.
| extend CommitmentTier1000DailyCost = CommitmentTier1000Price + max_of(billableGB - 1000, 0.)* CommitmentTier1000Price/1000.
| extend CommitmentTier2000DailyCost = CommitmentTier2000Price + max_of(billableGB - 2000, 0.)* CommitmentTier2000Price/2000.
| extend CommitmentTier5000DailyCost = CommitmentTier5000Price + max_of(billableGB - 5000, 0.)* CommitmentTier5000Price/5000.
| extend MinCost = min_of(
    PerNodeDailyCost,PerGBDailyCost,CommitmentTier100DailyCost,CommitmentTier200DailyCost,
    CommitmentTier300DailyCost, CommitmentTier400DailyCost, CommitmentTier500DailyCost, CommitmentTier1000DailyCost, CommitmentTier2000DailyCost, CommitmentTier5000DailyCost)
| extend Recommendation = case(
    MinCost == PerNodeDailyCost, "Per node tier",
    MinCost == PerGBDailyCost, "Pay-as-you-go tier",
    MinCost == CommitmentTier100DailyCost, "Commitment tier (100 GB/day)",
    MinCost == CommitmentTier200DailyCost, "Commitment tier (200 GB/day)",
    MinCost == CommitmentTier300DailyCost, "Commitment tier (300 GB/day)",
    MinCost == CommitmentTier400DailyCost, "Commitment tier (400 GB/day)",
    MinCost == CommitmentTier500DailyCost, "Commitment tier (500 GB/day)",
    MinCost == CommitmentTier1000DailyCost, "Commitment tier (1000 GB/day)",
    MinCost == CommitmentTier2000DailyCost, "Commitment tier (2000 GB/day)",
    MinCost == CommitmentTier5000DailyCost, "Commitment tier (5000 GB/day)",
    "Error"
)
| project day, nodesPerDay, ASCnodesPerDay, NonSecurityDataGB, SecurityDataGB, OverageGB, AvgGbPerNode, PerGBDailyCost, PerNodeDailyCost, 
    CommitmentTier100DailyCost, CommitmentTier200DailyCost, CommitmentTier300DailyCost, CommitmentTier400DailyCost, CommitmentTier500DailyCost, CommitmentTier1000DailyCost, CommitmentTier2000DailyCost, CommitmentTier5000DailyCost, Recommendation 
| sort by day asc
//| project day, Recommendation // Comment this line to see details
| sort by day asc
```

Esta consulta no es una replicación exacta de cómo se calcula el uso, pero proporciona recomendaciones de plan de tarifa en la mayoría de los casos.  

> [!NOTE]
> Para usar los derechos que proceden de la adquisición de OMS E1 Suite, OMS E2 Suite o un complemento de OMS para System Center, elija el plan de tarifa *Por nodo* de Log Analytics.

## <a name="create-an-alert-when-data-collection-is-high"></a>Creación de una alerta cuando la colección de datos es mayor

En esta sección se describe cómo crear una alerta mediante [Alertas de registro](../alerts/alerts-unified-log.md) de Azure Monitor si el volumen de datos de las últimas 24 horas ha superado una cantidad especificada. 

Para generar una alerta si el volumen de datos facturable ingerido en las últimas 24 horas fue superior a 50 GB: 

- **Definición de la condición de alerta**: especifique el área de trabajo de Log Analytics como el destino del recurso.
- **Criterios de alerta**: especifique lo siguiente:
   - **Nombre de señal**: seleccione **Custom log search** (Búsqueda de registros personalizada)
   - **Consulta de búsqueda** en `Usage | where IsBillable | summarize DataGB = sum(Quantity / 1000.) | where DataGB > 50`.  
   - El valor de **Lógica de alerta** está **Basado en** el *número de resultados*, mientras que el valor de **Condición** es *Mayor que* un **Umbral**  de *0*.
   - **Período de tiempo** de *1440* minutos y **Frecuencia de alerta** cada *1440* minutos para ejecutarse una vez al día.
- **Definición de los detalles de la alerta**: especifique lo siguiente:
   - **Nombre** en *Billable data volume greater than 50 GB in 24 hours* (Volumen de datos facturable mayor que 50 GB en 24 horas)
   - **Gravedad** en *Advertencia*

Especifique un [grupo de acciones](../alerts/action-groups.md) existente o cree uno nuevo para enviarle una notificación cuando la alerta de registro coincida con los criterios especificados.

Cuando se recibe una alerta, siga los pasos de las secciones siguientes sobre cómo solucionar el problema del uso mayor de lo esperado.

## <a name="data-transfer-charges-using-log-analytics"></a>Cargos por transferencia de datos mediante Log Analytics

Al enviar datos a Log Analytics se pueden aplicar ciertos cargos debido al ancho de banda de datos. No obstante, esta opción está limitada a las máquinas virtuales en las que está instalado un agente de Log Analytics y no se aplica cuando se usa la configuración de diagnóstico o con otros conectores integrados en Azure Sentinel. Tal como se describe en la [página de precios de Azure Bandwidth](https://azure.microsoft.com/pricing/details/bandwidth/), la transferencia de datos entre los servicios de Azure ubicados en dos regiones se cobra como transferencia de datos salientes a precio normal. La transferencia de datos entrantes es gratuita. Sin embargo, este cargo es muy reducido en comparación con los costos de la ingesta de datos de Log Analytics. Por lo tanto, el control de los costos de Log Analytics tiene que centrarse en el [volumen de datos ingerido](#understanding-ingested-data-volume). 

## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Solucionar que Log Analytics ya no recopile datos

Si tiene el plan de tarifa heredado Gratis y ha enviado más de 500 MB de datos en un día, la recopilación de datos se detiene durante el resto del día. Alcanzar el límite diario es un motivo frecuente de que Log Analytics deje de recopilar datos o de que parezca que faltan datos. Log Analytics crea un evento de tipo **Operación** cuando la recopilación de datos se inicia y se detiene. Ejecute la siguiente consulta en la búsqueda para comprobar si está alcanzando el límite diario y faltan datos: 

```kusto
Operation | where OperationCategory == 'Data Collection Status'
```

Cuando se detiene la recopilación de datos, **OperationStatus** es **Advertencia**. Cuando se inicia la recopilación de datos, **OperationStatus** es **Correcto**. En la tabla siguiente se enumeran los motivos por los que se detiene la recopilación de datos y una acción recomendada para reanudarla.

|Motivo por el que se detiene la recopilación| Solución| 
|-----------------------|---------|
|Se alcanzó el límite diario del área de trabajo|Espere para que se reinicie automáticamente la recopilación o aumente el límite diario de volumen de datos que se describe en Administración del volumen de datos diario máximo El tiempo de restablecimiento del límite diario se muestra en la página **Límite diario**. |
| El área de trabajo ha alcanzado la [tasa de volumen de ingesta de datos](../service-limits.md#log-analytics-workspaces). | El límite de velocidad de volumen de ingesta predeterminado para los datos enviados desde los recursos de Azure mediante la configuración del diagnóstico es de aproximadamente 6 GB/min por área de trabajo. Este es un valor aproximado, ya que el tamaño real puede variar entre los tipos de datos en función de la longitud del registro y su razón de compresión. Este límite no se aplica a los datos que se envían desde agentes o desde Data Collector API. Si envía datos a una velocidad superior a una sola área de trabajo, se quitan algunos datos y se envía un evento a la tabla Operación del área de trabajo cada 6 horas mientras se siga superando el umbral. Si el volumen de ingesta sigue superando el límite de velocidad o prevé que pronto lo va a alcanzar, puede solicitar un aumento en el área de trabajo mediante el envío de un correo electrónico a LAIngestionRate@microsoft.com o mediante la apertura de una solicitud de soporte técnico. El evento que se va a buscar que indica un límite de tasa de ingesta de datos se puede encontrar mediante la consulta `Operation | where OperationCategory == "Ingestion" | where Detail startswith "The rate of data crossed the threshold"`. |
|Se alcanzó el límite diario del plan de tarifa Gratis heredado |Espere hasta el día siguiente para que la recopilación se reinicie automáticamente, o cambie a un plan de tarifa de pago.|
|La suscripción de Azure está en estado suspendido debido a:<br> Prueba gratuita finalizada<br> Pase para Azure expirado<br> Se ha alcanzado el límite de gasto mensual (por ejemplo, en una suscripción de MSDN o Visual Studio)|Cambie a una suscripción de pago<br> Quite el límite o espere a que se restablezca|

Para recibir una notificación cuando se detenga la recopilación de datos, siga los pasos descritos en la sección [Alerta cuando se alcanza el límite diario](#alert-when-daily-cap-is-reached). Siga los pasos descritos en [Crear un grupo de acciones](../alerts/action-groups.md) para configurar una acción de correo electrónico, de webhook o de runbook para la regla de alerta. 

## <a name="limits-summary"></a>Resumen de límites

Existen limitaciones adicionales de Log Analytics, algunas de las cuales dependen del plan de tarifa de Log Analytics. Se documentan en [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md#log-analytics-workspaces).


## <a name="next-steps"></a>Pasos siguientes

- Consulte [Log searches in Azure Monitor Logs](../logs/log-query-overview.md) (Búsquedas de registros en el registro de Azure Monitor) para obtener información sobre cómo usar el lenguaje de búsqueda. Puede utilizar las consultas de búsqueda para realizar análisis adicionales sobre los datos de uso.
- Siga los pasos explicados en [Crear una nueva alerta de registro](../alerts/alerts-metric.md) para recibir una notificación cuando se cumplan los criterios de búsqueda.
- Use la [selección de destino de solución](../insights/solution-targeting.md) para recopilar datos solo de los grupos de equipos necesarios.
- Para configurar una directiva eficaz de recopilación de eventos, revise la [Directiva de filtrado de Azure Defender (Security Center)](../../security-center/security-center-enable-data-collection.md).
- Cambie la [configuración de los contadores de rendimiento](../agents/data-sources-performance-counters.md).
- Para modificar la configuración de recopilación de eventos, revise la [configuración de registros de eventos](../agents/data-sources-windows-events.md).
- Para modificar la configuración de la recopilación de syslog, revise la [configuración de syslog](../agents/data-sources-syslog.md).
- Para modificar la configuración de la recopilación de syslog, revise la [configuración de syslog](../agents/data-sources-syslog.md).
