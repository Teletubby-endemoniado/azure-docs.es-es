---
title: Creación, visualización y administración de alertas de métricas mediante Azure Monitor
description: Aprenda a usar Azure Portal o la CLI para crear, ver y administrar las reglas de alerta de métricas.
author: harelbr
ms.author: harelbr
ms.topic: conceptual
ms.date: 09/09/2021
ms.openlocfilehash: 7a76b4b37760e5e320ab62c2660e97c9fcabdbb0
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124815689"
---
# <a name="create-view-and-manage-metric-alerts-using-azure-monitor"></a>Creación, visualización y administración de alertas de métricas mediante Azure Monitor

Las alertas de métricas en Azure Monitor proporcionan una forma de recibir notificaciones cuando una de sus métricas cruza un umbral. Las alertas de métricas funcionan en una amplia variedad de métricas de plataforma multidimensionales, métricas personalizadas y métricas personalizadas y estándar de Application Insights. En este artículo describiremos cómo crear, ver y administrar las reglas de alertas de métricas a través de Azure Portal y la CLI de Azure. También puede crear reglas de alertas de métricas mediante plantillas de Azure Resource Manager, que se describen en [otro artículo](./alerts-metric-create-templates.md).

Puede obtener más información acerca del funcionamiento de las alertas de métricas en el artículo sobre [información general de las alertas de métricas](./alerts-metric-overview.md).

## <a name="create-with-azure-portal"></a>Creación con Azure Portal

En el siguiente procedimiento se describe cómo crear una regla de alertas de métricas en Azure Portal:

1. En [Azure Portal](https://portal.azure.com), haga clic en **Monitor**. La hoja {1}Monitor{2} consolida todas las opciones de configuración y todos los datos de supervisión en una vista.

2. Haga clic en **Alertas** y, a continuación, en **+ Nueva regla de alertas**.

    > [!TIP]
    > La mayoría de las hojas de recursos también tienen la opción **Alertas** en el menú de recursos de la sección **Supervisión**, de modo que también podría crear alertas desde allí.

3. Haga clic en **Seleccionar destino**, en el panel de contexto que se carga, y seleccione un recurso de destino sobre el que quiera alertar. Use los menús desplegables **Suscripción** y **Tipo de recurso** para buscar el recurso que quiere supervisar. También puede utilizar la barra de búsqueda para buscar su recurso.

4. Si el recurso seleccionado tiene métricas para las que puede crear alertas, la sección **Available signals** (Señales disponibles) de la parte inferior derecha incluirá métricas. Puede ver la lista completa de tipos de recursos compatibles con las alertas de métricas en este [artículo](./alerts-metric-near-real-time.md#metrics-and-dimensions-supported).

5. Una vez haya seleccionado un recurso de destino, haga clic en **Agregar condición**.

6. Verá una lista de señales que se admiten para el recurso. Seleccione la métrica para la que quiera crear una alerta.

7. Verá un gráfico para la métrica de las últimas seis horas. Use la lista desplegable **Período del gráfico** para ver un historial más largo de la métrica.

8. Si la métrica tiene dimensiones, podrá ver una tabla de dimensiones. Seleccione uno o varios valores por dimensión.
    - Los valores de dimensión mostrados se basan en los datos de métricas del último día.
    - Si no se muestra el valor de dimensión que está buscando, haga clic en "Agregar valor personalizado" para agregar un valor de dimensión personalizado.
    - También puede **Seleccionar todos los valores actuales y futuros** para cualquiera de las dimensiones. Esto escala dinámicamente la selección a todos los valores actuales y futuros de una dimensión.

    La regla de alertas de métricas evaluará la condición para todas las combinaciones de valores seleccionados. [Obtenga más información sobre cómo funciona la creación de alertas en las métricas multidimensionales](./alerts-metric-overview.md).
    
    > [!NOTE]
    > El uso de "All" como valor de dimensión equivale a seleccionar " Todos los valores actuales y futuros".

9. Seleccione el tipo de **Umbral**, **Operador** y **Tipo de agregación**. Esto determinará la lógica que evaluará la regla de alertas de métricas.
    - Si usa un umbral **Estático**, siga para definir un **Valor de umbral**. El gráfico de métricas puede ayudar a determinar cuál podría ser un umbral razonable.
    - Si usa un umbral **Dinámico**, siga para definir la **Sensibilidad del umbral**. El gráfico de métricas mostrará los umbrales calculados según los datos recientes. [Más información sobre las opciones de tipo y sensibilidad de la condición de umbrales dinámicos](../alerts/alerts-dynamic-thresholds.md).

10. Opcionalmente, refine la condición ajustando las opciones **Granularidad de agregación** y **Frecuencia de evaluación**. 

11. Haga clic en **Done**(Listo).

12. Opcionalmente, puede agregar otro criterio si quiere supervisar una regla de alertas compleja. Actualmente los usuarios pueden tener reglas de alertas con criterios de umbrales dinámicos como único criterio.

13. Rellene los **Detalles de alertas** como **Nombre de la regla de alertas**, **Descripción** y **Gravedad**.

14. Agregue un grupo de acciones a la alerta, ya sea seleccionando un grupo de acciones existente o creando uno nuevo.

15. Haga clic en **Listo** para guardar la regla de alertas de métrica.


## <a name="view-and-manage-with-azure-portal"></a>Visualización y administración con Azure Portal

Puede ver y administrar las reglas de alertas de métricas mediante la hoja Administrar reglas de Alertas. En el siguiente procedimiento se muestra cómo puede ver las reglas de alertas de métricas y editar una de ellas.

1. En Azure Portal, vaya a **Monitor**.

2. Haga clic en **Alertas** y **Administrar reglas**

3. En la hoja **Administrar reglas**, puede ver todas las reglas de alertas de las suscripciones. Puede filtrar aún más las reglas mediante las opciones **Grupo de recursos**, **Tipo de recurso** y **Recurso**. Si solo quiere ver las alertas de métricas, seleccione Métricas en **Tipo de señal**.

    > [!TIP]
    > En la hoja **Administrar reglas**, puede seleccionar varias reglas de alertas y habilitarlas o deshabilitarlas. Esto puede resultarle útil cuando es necesario realizar un mantenimiento de determinados recursos de destino.

4. Haga clic en el nombre de la regla de alertas de métricas que quiera editar.

5. En la regla Editar, haga clic en los **criterios de alerta** que quiera editar. Puede cambiar la métrica, la condición de umbral y otros campos según sea necesario

    > [!NOTE]
    > No puede editar el **nombre de la regla de alertas** después de crear la regla de alertas de métrica.

6. Haga clic en **Listo** para guardar los cambios.


## <a name="with-azure-cli"></a>Con la CLI de Azure

En las secciones anteriores se describía cómo crear, ver y administrar las reglas de alertas de métricas mediante Azure Portal. En esta sección se describe cómo hacer lo mismo con la multiplataforma [CLI de Azure](/cli/azure/get-started-with-azure-cli). La forma más rápida de comenzar a utilizar la CLI de Azure es a través de [Azure Cloud Shell](../../cloud-shell/overview.md). En este artículo, usamos Cloud Shell.

1. Vaya a Azure Portal y haga clic en **Cloud Shell**.

2. En el símbolo del sistema, puede usar los comandos con la opción ``--help`` para obtener más información sobre el comando y cómo usarlo. Por ejemplo, el comando siguiente muestra la lista de comandos disponibles para crear, ver y administrar alertas de métricas.

    ```azurecli
    az monitor metrics alert --help
    ```

3. Puede crear una regla de alertas de métricas sencilla que supervise si el porcentaje medio de la CPU en una máquina virtual es mayor que 90.

    ```azurecli
    az monitor metrics alert create -n {nameofthealert} -g {ResourceGroup} --scopes {VirtualMachineResourceID} --condition "avg Percentage CPU > 90" --description {descriptionofthealert}
    ```

4. Puede ver todas las alertas de métricas en un grupo de recursos con el siguiente comando.

    ```azurecli
    az monitor metrics alert list  -g {ResourceGroup}
    ```

5. Puede ver los detalles de una regla de alertas métricas determinada mediante el nombre o el identificador del recurso de la regla.

    ```azurecli
    az monitor metrics alert show -g {ResourceGroup} -n {AlertRuleName}
    ```

    ```azurecli
    az monitor metrics alert show --ids {RuleResourceId}
    ```

6. Puede deshabilitar una regla de alertas de métricas con el comando siguiente.

    ```azurecli
    az monitor metrics alert update -g {ResourceGroup} -n {AlertRuleName} --enabled false
    ```

7. Puede eliminar una regla de alertas de métricas con el comando siguiente.

    ```azurecli
    az monitor metrics alert delete -g {ResourceGroup} -n {AlertRuleName}
    ```

## <a name="with-powershell"></a>Con PowerShell

Las reglas de alertas de métricas disponen de cmdlets dedicados de PowerShell:

- [Add-AzMetricAlertRuleV2](/powershell/module/az.monitor/add-azmetricalertrulev2): cree una nueva regla de alerta de métrica o actualice una existente.
- [Get-AzMetricAlertRuleV2](/powershell/module/az.monitor/get-azmetricalertrulev2): obtenga una o varias reglas de alerta de métricas.
- [Remove-AzMetricAlertRuleV2](/powershell/module/az.monitor/remove-azmetricalertrulev2): elimine una regla de alerta de métrica.

## <a name="with-rest-api"></a>Con API REST

- [Crear o actualizar](/rest/api/monitor/metricalerts/createorupdate): cree una nueva regla de alerta de métrica o actualice una existente.
- [Obtener](/rest/api/monitor/metricalerts/get): obtenga una regla de alerta de métrica específica.
- [Enumerar por grupo de recursos](/rest/api/monitor/metricalerts/listbyresourcegroup): obtenga una lista de reglas de alertas de métricas en un grupo de recursos específico.
- [Lista por suscripción](/rest/api/monitor/metricalerts/listbysubscription): obtenga una lista de reglas de alertas de métricas en una suscripción específica.
- [Actualizar](/rest/api/monitor/metricalerts/update): actualice una regla de alerta de métrica.
- [Eliminar](/rest/api/monitor/metricalerts/delete): elimine una regla de alerta de métrica.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una alerta de métrica con una plantilla de Resource Manager](./alerts-metric-create-templates.md)
- [Comprender cómo funcionan las alertas de métricas](./alerts-metric-overview.md)
- [Alertas de métricas con umbrales dinámicos en Azure Monitor](../alerts/alerts-dynamic-thresholds.md)
- [Comprender el esquema de webhook para las alertas de métricas](./alerts-metric-near-real-time.md#payload-schema)
- [Solución de problemas en las alertas de métricas](./alerts-troubleshoot-metric.md)
