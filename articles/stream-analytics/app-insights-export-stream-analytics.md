---
title: Exportación mediante Stream Analytics desde Azure Application Insights | Microsoft Docs
description: Stream Analytics puede transformar, filtrar y enrutar continuamente los datos de que exportan desde Application Insights.
ms.topic: conceptual
ms.date: 01/08/2019
ms.service: stream-analytics
ms.openlocfilehash: 56ce47f232c78637b82c16501e47f2bff8707430
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128594641"
---
# <a name="use-stream-analytics-to-process-exported-data-from-application-insights"></a>Uso de Stream Analytics para procesar datos exportados de Application Insights

[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) es la herramienta ideal para procesar los datos [exportados desde Application Insights](../azure-monitor/app/export-telemetry.md). Stream Analytics puede extraer datos de una variedad de orígenes. Puede transformar y filtrar los datos y, después, enrutarlos a una variedad de receptores.

En este ejemplo, vamos a crear un adaptador que toma datos de Application Insights mediante el uso de la exportación continua, cambia su nombre y procesa algunos de los campos, y después los canaliza a Power BI.

> [!WARNING]
> Hay formas recomendadas mucho mejores [y sencillas de mostrar datos de Application Insights en Power BI](../azure-monitor/app/export-power-bi.md). La ruta de acceso que se muestra aquí es solo un ejemplo para mostrar cómo se procesan los datos exportados.

> [!IMPORTANT]
> La exportación continua está en desuso y solo se admite para los recursos clásicos de Application Insights. [Migre a un recurso de Application Insights basado en áreas de trabajo](../azure-monitor/app/convert-classic-resource.md) para usar la [configuración de diagnóstico](../azure-monitor/app/export-telemetry.md#diagnostic-settings-based-export) para la exportación de telemetría.


![Diagrama de bloques para exportar Stream Analytics a Power BI.](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a>Creación de almacenamiento en Azure
La exportación continua siempre envía los datos a una cuenta de Azure Storage, por lo que necesitará crear primero el almacenamiento.

1. Cree una cuenta de almacenamiento "clásica" en su suscripción en el [Portal de Azure](https://portal.azure.com).
   
   ![Captura de pantalla de Azure Portal, selección de Nuevo, Datos, Almacenamiento.](./media/app-insights-export-stream-analytics/030.png)
2. Creación de un contenedor
   
    ![Captura de pantalla del nuevo almacenamiento, seleccione Contenedores, haga clic en el icono Contenedores y luego, en Agregar.](./media/app-insights-export-stream-analytics/040.png)
3. Copie la clave de acceso de almacenamiento.
   
    La necesitará en seguida para configurar la entrada para el servicio de análisis de transmisiones.
   
    ![Captura de pantalla del almacenamiento, abra Configuración, Claves y realice una copia de la clave de acceso principal.](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-to-azure-storage"></a>Inicio de la exportación continua al almacenamiento de Azure

[exportación continua](../azure-monitor/app/export-telemetry.md) transfiere los datos de Application Insights al almacenamiento de Azure. 

1. En el portal de Azure, busque el recurso de Application Insights que ha creado para la aplicación.
   
    ![Captura de pantalla de todos los recursos de Azure Portal, elija Application Insights y, a continuación, la aplicación.](./media/app-insights-export-stream-analytics/050.png)
2. Cree una exportación continua.
   
    ![Captura de pantalla de Application Insights y elija Configuración, Exportación continua y, a continuación, Agregar.](./media/app-insights-export-stream-analytics/060.png)

    Seleccione la cuenta de almacenamiento que creó anteriormente:

    ![Captura de pantalla de Exportación continua, seleccione Agregar y, a continuación, establezca el destino de exportación.](./media/app-insights-export-stream-analytics/070.png)

    Configure los tipos de eventos que desea ver:

    ![Captura de pantalla de la adición de la exportación continua y elija los tipos de eventos.](./media/app-insights-export-stream-analytics/080.png)

1. Permita que se acumulen algunos datos. Póngase cómo y deje que los usuarios usen su aplicación durante un tiempo. Así, aparecerá la telemetría y verá gráficos estadísticos en el [explorador de métricas](../azure-monitor/essentials/metrics-charts.md) y eventos individuales en la [búsqueda de diagnóstico](../azure-monitor/app/diagnostic-search.md).
   
    Y, además, exportará los datos en el almacenamiento. 
2. Inspeccione los datos exportados. En Visual Studio, elija **Ver/Cloud Explorer** y abra Azure/Almacenamiento. (Si no tiene esta opción de menú, deberá instalar Azure SDK: abra el cuadro de diálogo Nuevo proyecto y Visual C#/Nube/Obtener Microsoft Azure SDK para. NET).
   
    ![Captura de pantalla que muestra cómo establecer los tipos de evento que desea ver.](./media/app-insights-export-stream-analytics/04-data.png)
   
    Tome nota de la parte común del nombre de la ruta de acceso, que se deriva del nombre de la aplicación y de la clave de instrumentación.

Los eventos se escriben en archivos de blob en formato JSON. Cada archivo puede contener uno o varios eventos. Así, es probable que queramos leer los datos de eventos y filtrar por los campos que deseemos. Se pueden realizar multitud de acciones con los datos, pero nuestro plan de hoy consiste en usar Stream Analytics para canalizar los datos a Power BI.

## <a name="create-an-azure-stream-analytics-instance"></a>Creación de una instancia de Azure Stream Analytics

Desde [Azure Portal](https://portal.azure.com/), seleccione el servicio Azure Stream Analytics y cree un nuevo trabajo de Stream Analytics:

![Captura de pantalla que muestra la página principal para crear un trabajo de Stream Analytics en Azure Portal.](./media/app-insights-export-stream-analytics/001.png)

![Captura de pantalla que muestra los detalles necesarios al crear un nuevo trabajo de Stream Analytics.](./media/app-insights-export-stream-analytics/002.png)

Cuando se cree el nuevo trabajo, seleccione **Ir al recurso**.

![Captura de pantalla que muestra el mensaje recibido cuando la nueva implementación de trabajo de Stream Analytics se realiza correctamente.](./media/app-insights-export-stream-analytics/003.png)

### <a name="add-a-new-input"></a>Incorporación de una nueva entrada

![Captura de pantalla que muestra cómo agregar entradas al trabajo de Stream Analytics.](./media/app-insights-export-stream-analytics/004.png)

Configúrelo de manera que tome los datos del blob de Exportación continua:

![Captura de pantalla que muestra la configuración del trabajo de Stream Analytics para tomar la entrada de un blob de exportación continua.](./media/app-insights-export-stream-analytics/0005.png)

Ahora, necesitará la clave de acceso principal de la cuenta de almacenamiento, que anotó anteriormente. Establézcala como la clave de cuenta de almacenamiento.

### <a name="set-path-prefix-pattern"></a>Establecimiento del patrón del prefijo de la ruta de acceso

**Asegúrese de establecer el formato de fecha como AAAA-MM-DD (con guiones).**

El patrón del prefijo de la ruta de acceso especifica dónde busca el Stream Analytics los archivos de entrada en el almacenamiento. Deberá establecerlo para que coincida con el modo en que la Exportación continua almacena los datos. Configúrelo como este caso que se muestra a continuación:

`webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}`

En este ejemplo:

* `webapplication27` es el nombre del recurso de Application Insights, **todo en minúsculas**.
* `1234...` es la clave de instrumentación que copió del recurso de Application Insights, **omitiendo guiones**. 
* `PageViews` es el tipo de datos que quiere analizar. Los tipos disponibles dependen del filtro definido en la Exportación continua. Examine los datos exportados para ver los demás tipos disponibles y vea el [modelo de exportación de datos](../azure-monitor/app/export-data-model.md).
* `/{date}/{time}` es un patrón escrito literalmente.

> [!NOTE]
> Inspeccione el almacenamiento para asegurarse de que obtiene la ruta de acceso correcta.

## <a name="add-new-output"></a>Incorporación de una nueva salida

Ahora seleccione el trabajo > **Salidas** > **Agregar**.

![Captura de pantalla que muestra la selección del trabajo de Stream Analytics para agregar una nueva salida.](./media/app-insights-export-stream-analytics/006.png)


![Captura de pantalla de la nueva salida, seleccione el nuevo canal, Salidas, Agregar y, a continuación, Power BI.](./media/app-insights-export-stream-analytics/010.png)

Ofrezca su **cuenta profesional o educativa** para autorizar el Stream Analytics a tener acceso al recurso de Power BI. Luego invente un nombre para la salida y para la tabla y el conjunto de datos de Power BI de destino.

## <a name="set-the-query"></a>Definición de la consulta

La consulta controla la traducción de la entrada en la salida.

Use la función de prueba para comprobar que obtiene la salida correcta. Proporciónele los datos de ejemplo que tomó de la página de entradas.

### <a name="query-to-display-counts-of-events"></a>Consulta para mostrar recuentos de eventos

Pegue esta consulta:

```SQL
SELECT
  flat.ArrayValue.name,
  count(*)
INTO
  [pbi-output]
FROM
  [export-input] A
OUTER APPLY GetElements(A.[event]) as flat
GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* export-input es el alias que damos a la entrada de transmisiones
* pbi-output es el alias que hemos definido para la salida
* Usamos [OUTER APPLY GetElements](/stream-analytics-query/apply-azure-stream-analytics) porque el nombre del evento se encuentra en una matriz JSON anidada. A continuación, la opción Select selecciona el nombre de evento, junto con el recuento del número de instancias con dicho nombre en el período de tiempo. La cláusula [Group By](/stream-analytics-query/group-by-azure-stream-analytics) agrupa los elementos en períodos de tiempo de un minuto.

### <a name="query-to-display-metric-values"></a>Consulta para mostrar valores de métrica

```SQL
SELECT
  A.context.data.eventtime,
  avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
INTO
  [pbi-output]
FROM
  [export-input] A
OUTER APPLY GetElements(A.context.custom.metrics) as flat
GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime
```

* Esta consulta explora la telemetría de métricas para obtener la hora del evento y el valor de métrica. Los valores de métrica están dentro de una matriz, por eso usamos el patrón OUTER APPLY GetElements para extraer las filas. "myMetric" es el nombre de la métrica en este caso.

### <a name="query-to-include-values-of-dimension-properties"></a>Consulta para incluir los valores de propiedades de dimensión

```SQL
WITH flat AS (
SELECT
  MySource.context.data.eventTime as eventTime,
  InstanceId = MyDimension.ArrayValue.InstanceId.value,
  BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
FROM MySource
OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
)
SELECT
  eventTime,
  InstanceId,
  BusinessUnitId
INTO AIOutput
FROM flat
```

* Esta consulta incluye valores de las propiedades de dimensión sin depender de la fijación de una dimensión determinada en un índice fijo en la matriz de dimensión.

## <a name="run-the-job"></a>Ejecutar el trabajo

Puede seleccionar una fecha en el pasado desde la que iniciar el trabajo.

![Captura de pantalla de Stream Analyics, seleccione el trabajo y haga clic en Consulta. Pegue el siguiente ejemplo.](./media/app-insights-export-stream-analytics/008.png)

Espere hasta que el trabajo esté en ejecución.

## <a name="see-results-in-power-bi"></a>Visualización de resultados en Power BI

> [!WARNING]
> Hay formas recomendadas mucho mejores [y sencillas de mostrar datos de Application Insights en Power BI](../azure-monitor/app/export-power-bi.md). La ruta de acceso que se muestra aquí es solo un ejemplo para mostrar cómo se procesan los datos exportados.


Abra Power BI con su cuenta profesional o educativa y seleccione el conjunto de datos y la tabla que ha definido como la salida del trabajo del Stream Analytics.

![Captura de pantalla de Power BI, seleccione el conjunto de datos y los campos.](./media/app-insights-export-stream-analytics/200.png)

Ahora puede usar este conjunto de datos en informes y paneles de [Power BI](https://powerbi.microsoft.com).

![Captura de pantalla que muestra un ejemplo de informe procedente de un conjunto de datos de Power BI.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a>¿No hay datos?

* Compruebe que [establece el formato de fecha](#set-path-prefix-pattern) correctamente en AAAA-MM-DD (con guiones).

## <a name="next-steps"></a>Pasos siguientes
* [Exportación continua](../azure-monitor/app/export-telemetry.md)
* [Referencia detallada del modelo de datos para los tipos y valores de propiedad.](../azure-monitor/app/export-data-model.md)
* [Application Insights](../azure-monitor/app/app-insights-overview.md)
