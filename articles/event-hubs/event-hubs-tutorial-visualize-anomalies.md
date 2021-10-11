---
title: 'Azure Event Hubs: Visualización de anomalías de datos de eventos en tiempo real'
description: 'Tutorial: Visualización de anomalías de datos de eventos en tiempo real enviados a Microsoft Azure Event Hubs'
ms.topic: tutorial
ms.date: 09/29/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e89cf8f501576b18144e28b8b042948cdcb408ee
ms.sourcegitcommit: 613789059b275cfae44f2a983906cca06a8706ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129276495"
---
# <a name="tutorial-visualize-data-anomalies-in-real-time-events-sent-to-azure-event-hubs"></a>Tutorial: Visualización de anomalías de datos de eventos en tiempo real enviados a Azure Event Hubs

Con Azure Event Hubs, puede usar Azure Stream Analytics para comprobar los datos entrantes y extraer las anomalías, que luego puede visualizar en Power BI. Supongamos que tiene miles de dispositivos que envían constantemente datos en tiempo real a un centro de eventos, lo que suma millones de eventos por segundo. ¿Cómo comprueba errores y anomalías en tantos datos? Por ejemplo, ¿y si los dispositivos envían transacciones de tarjeta de crédito y necesita capturarlas en cualquier lugar donde tenga varias transacciones en varios países o regiones dentro de un intervalo de tiempo de 5 segundos? Esto podría ocurrir si alguien roba tarjetas de crédito y luego las usa para comprar artículos en todo el mundo a la misma hora. 

En este tutorial, se simula este ejemplo. Ejecutará una aplicación que crea y envía transacciones de tarjeta de crédito a un centro de eventos. A continuación, leerá el flujo de datos en tiempo real con Azure Stream Analytics, que separa las transacciones válidas de las que no lo son y, luego, usará Power BI para identificar visualmente las transacciones etiquetadas como no válidas.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Creación de un espacio de nombres de Event Hubs
> * Creación de un centro de eventos
> * Ejecutar la aplicación que envía transacciones de tarjeta de crédito
> * Configurar un trabajo de Stream Analytics para procesar esas transacciones
> * Configurar una visualización de Power BI para mostrar los resultados

## <a name="prerequisites"></a>Prerrequisitos
Antes de empezar, asegúrese de seguir estos pasos:

- Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- [Crear un espacio de nombres de Event Hubs y un centro de eventos en el espacio de nombres](event-hubs-create.md).
- Siga las instrucciones de [Obtención de una cadena de conexión de Event Hubs](event-hubs-get-connection-string.md). Anote la cadena de conexión en el espacio de nombres de Event Hubs y el nombre del centro de eventos. 
- Instale [Visual Studio](https://www.visualstudio.com/). Use una solución de Visual Studio para ejecutar una aplicación para generar y enviar datos de eventos de prueba al centro de eventos. 
- Necesitará una cuenta de Power BI para analizar la salida de un trabajo de Stream Analytics. Puede [probar Power BI de forma gratuita](https://app.powerbi.com/signupredirect?pbi_source=web).

## <a name="run-app-to-produce-test-event-data"></a>Ejecución de la aplicación para generar datos de eventos de prueba

Los [ejemplos de Event Hubs en GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet) incluyen una aplicación de Anomaly Detector que genera datos de prueba automáticamente. Para simular el uso de tarjetas de crédito, escribe transacciones de tarjeta de crédito en el centro de eventos, y ocasionalmente escribe varias transacciones para la misma tarjeta de crédito en varias ubicaciones para que se etiqueten como anomalías. Para ejecutar esta aplicación, siga estos pasos: 

1. Descargue los [ejemplos de Azure Event Hubs](https://github.com/Azure/azure-event-hubs/archive/master.zip) desde GitHub y descomprímalos localmente.
2. Vaya a la carpeta **\azure-event-hubs-master\samples\DotNet\\** . 
3. Vaya a la carpeta **Azure.Messaging.EventHubs\AnomalyDetector\\** y haga doble clic en **AnomalyDetector.sln** para abrir la solución en Visual Studio. 

    Para usar la versión anterior del ejemplo que usa el paquete antiguo Microsoft.Azure.EventHubs, abra la solución desde la carpeta **Microsoft.Azure.EventHubs\AnomalyDetector**. 
3. Abra Program.cs y reemplace la **cadena de conexión de Event Hubs** por la que guardó al ejecutar el script. 
4. Reemplace el **nombre del centro de eventos** por el nombre de su centro de eventos. Presione F5 para ejecutar la aplicación. La aplicación comienza a enviar eventos al centro de eventos y continúa hasta que ha enviado 1000 eventos. Hay algunos casos en los que la aplicación debe estar en ejecución para recuperar datos. Estos casos se señalan en las instrucciones siguientes, donde sea necesario.

## <a name="set-up-azure-stream-analytics"></a>Configuración de Azure Stream Analytics

Ahora puede transmitir los datos al centro de eventos. Para usar esos datos en una visualización de Power BI, primero configure un trabajo de Stream Analytics para recuperar los datos que luego se introducen en la visualización de Power BI.

### <a name="create-the-stream-analytics-job"></a>Creación del trabajo de Stream Analytics

1. En Azure Portal, haga clic en **Crear un recurso**. Escriba **stream analytics** en el cuadro de búsqueda y presione **Entrar**. Seleccione **Trabajo de Stream Analytics**. Haga clic en **Crear** en el panel del trabajo de Stream Analytics. 

2. Escriba la siguiente información del trabajo:

   **Nombre del trabajo**: use **contosoEHjob**. Este campo es el nombre del trabajo y debe ser globalmente único.

   **Suscripción**: Seleccione su suscripción.

   **Grupo de recursos**: use el mismo grupo de recursos utilizado por el centro de eventos (**ContosoResourcesEH**).

   **Ubicación:** use la misma región de Azure que usó antes.

   ![Captura de pantalla que muestra cómo crear un trabajo de Azure Stream Analytics.](./media/event-hubs-tutorial-visualize-anomalies/stream-analytics-add-job.png)

    En el resto de los campos, acepte los valores predeterminados. Haga clic en **Crear**. 

### <a name="add-an-input-to-the-stream-analytics-job"></a>Adición de una entrada al trabajo de Stream Analytics

Si no está en el portal en el panel **Trabajo de Stream Analytics**, puede volver al trabajo de Stream Analytics; para ello, haga clic en **Grupos de recursos** en el portal y seleccione el grupo de recursos ( **ContosoResourcesEH**). Esta acción muestra todos los recursos del grupo y ahí puede seleccionar el trabajo de Stream Analytics. 

Las entradas del trabajo de Steam Analytics son las transacciones de tarjeta de crédito del centro de eventos.


1. En el menú de la izquierda, seleccione **Entradas** en la sección **Topología del trabajo**.
2. En el panel **Entradas**, haga clic en **Add stream input** (Agregar entrada de flujo) y seleccione **Event Hubs**. En la pantalla que aparece, rellene los campos siguientes:

   **Alias de entrada**: use **contosoinputs**. Este campo es el nombre del flujo de entrada usado al definir la consulta de los datos.

   **Suscripción**: Seleccione su suscripción a Azure.

   **Espacio de nombres de Event Hubs**: seleccione su espacio de nombres de Event Hubs. 

   **Nombre del centro de eventos**: haga clic en **Usar existente** y seleccione el centro de eventos.

   **Grupo de consumidores de Event Hubs**: deje este campo en blanco para usar el grupo de consumidores predeterminado.

   En el resto de los campos, acepte los valores predeterminados.

   ![Captura de pantalla que muestra cómo agregar un flujo de entrada al trabajo de Stream Analytics.](./media/event-hubs-tutorial-visualize-anomalies/stream-analytics-inputs.png)
5. Haga clic en **Save**(Guardar).

### <a name="add-an-output-to-the-stream-analytics-job"></a>Adición de una salida al trabajo de Stream Analytics

1. En el menú de la izquierda, seleccione **Salidas** en la sección **Topología del trabajo**. Este campo es el nombre del flujo de salida, que se usa al definir la consulta de los datos.
2. En el panel **Salidas**, haga clic en **Agregar** y, después, seleccione **Power BI**. En la pantalla que aparece, rellene los campos siguientes:

   **Alias de salida**: use **contosooutputs**. Este campo es el alias único de la salida. 

   **Nombre del conjunto de datos**: use **contosoehdataset**. Este campo es el nombre del conjunto de datos que se va a usar en Power BI. 

   **Nombre de la tabla**: use **contosoehtable**. Este campo es el nombre de la tabla que se va a usar en Power BI. 

   En el resto de los campos, acepte los valores predeterminados.

   ![Captura de pantalla que muestra cómo configurar la salida de un trabajo de Stream Analytics.](./media/event-hubs-tutorial-visualize-anomalies/stream-analytics-outputs.png)
3. Haga clic en **Autorizar** e inicie sesión en su cuenta de Power BI.
4. En el resto de los campos, acepte los valores predeterminados.
5. Haga clic en **Save**(Guardar).

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Configuración de la consulta del trabajo de Stream Analytics

Esta consulta se usa para recuperar los datos que finalmente se envían a la visualización de Power BI. Usa **contosoinputs** y **contosooutputs**, que anteriormente definió al configurar el trabajo. Esta consulta recupera las transacciones de tarjeta de crédito que parecen fraudulentas, que son las transacciones en las que el mismo número de tarjeta de crédito tiene varias transacciones en diferentes ubicaciones en el mismo intervalo de cinco segundos.

1. En **Topología de trabajo**, haga clic en **Consulta**.

2. Reemplace la consulta por la siguiente: 

   ```SQL
   /* criteria for fraud:
      credit card purchases with the same card
      in different locations within 5 seconds
   */
   SELECT System.Timestamp AS WindowEnd, 
     COUNT(*) as FraudulentUses      
   INTO contosooutputs
   FROM contosoinputs CS1 TIMESTAMP BY [Timestamp]
       JOIN contosoinputs CS2 TIMESTAMP BY [Timestamp]
       /* where the credit card # is the same */
       ON CS1.CreditCardId = CS2.CreditCardId
       /* and time between the two is between 0 and 5 seconds */
       AND DATEDIFF(second, CS1, CS2) BETWEEN 0 AND 5
       /* where the location is different */
   WHERE CS1.Location != CS2.Location
   GROUP BY TumblingWindow(Duration(second, 1))
   ```

4. Haga clic en **Save**(Guardar).

### <a name="test-the-query-for-the-stream-analytics-job"></a>Prueba de la consulta en el trabajo de Stream Analytics 

1. Ejecute la aplicación de detección de anomalías para enviar datos al centro de eventos mientras se configura y ejecuta la prueba. 

2. En el panel Consulta, haga clic en los puntos que aparecen junto a la entrada **contosoinputs** y seleccione **Sample data from input** (Muestrear datos de entrada).

3. Especifique que quiere datos correspondientes a tres minutos y haga clic en **Aceptar**. Espere a que se le notifique que se ha realizado un muestreo de los datos.

4. Haga clic en **Probar** y asegúrese de que obtiene resultados. Los resultados se muestran en la sección **Resultados** del panel inferior a la derecha en la consulta.

5. Cierre el panel Consulta.

### <a name="run-the-stream-analytics-job"></a>Ejecución del trabajo de Stream Analytics

En el trabajo de Stream Analytics, haga clic en **Iniciar**, luego en **Ahora** y, finalmente, en **Iniciar**. Una vez que el trabajo se inicia correctamente, su estado cambia de **Detenido** a **En ejecución**.

## <a name="set-up-the-power-bi-visualizations"></a>Configuración de las visualizaciones de Power BI

1. Ejecute la aplicación de detección de anomalías para enviar datos al centro de eventos mientras se configura y ejecuta la visualización de Power BI. Es posible que deba ejecutarla varias veces, puesto que solo se generan 1000 transacciones con cada ejecución.

2. Inicie sesión en su cuenta de [Power BI](https://powerbi.microsoft.com/).

3. Vaya a **Mi área de trabajo**.

4. Ahora haga clic en **Conjuntos de datos**.

   Verá el conjunto de datos que especificó en el momento de crear la salida para el trabajo de Stream Analytics (**contosoehdataset**). La primera vez, el conjunto de datos puede tardar en mostrarse entre 5 y 10 minutos.

5. Haga clic en **Paneles**, en **Crear** y seleccione **Panel**.

   ![Captura de pantalla de los botones Paneles y Crear.](./media/event-hubs-tutorial-visualize-anomalies/power-bi-add-dashboard.png)

6. Especifique el nombre del panel y, luego, haga clic en **Crear**. Use **Credit Card Anomalies**.

   ![Captura de pantalla de especificación del nombre del panel.](./media/event-hubs-tutorial-visualize-anomalies/power-bi-dashboard-name.png)

7. En la página Panel, haga clic en **Agregar icono**, seleccione **Datos de streaming personalizados** en la sección **DATOS EN TIEMPO REAL** y, después, haga clic en **Siguiente**.

   ![Captura de pantalla de especificación del origen del icono.](./media/event-hubs-tutorial-visualize-anomalies/power-bi-add-card-real-time-data.png)

8. Seleccione el conjunto de datos (**contosoehdataset**) y haga clic en **Siguiente**.

   ![Captura de pantalla de especificación del conjunto de datos.](./media/event-hubs-tutorial-visualize-anomalies/power-bi-dashboard-select-dataset.png)

9. Seleccione **Tarjeta** como tipo de visualización. En **Campos**, haga clic en **Agregar valor** y, luego, seleccione `fraudulentuses`.

   ![Captura de pantalla de especificación de campos y tipo de visualización.](./media/event-hubs-tutorial-visualize-anomalies/power-bi-add-card-tile.png)

   Haga clic en **Next**.

10. Establezca el título en **Fraudulent uses** (Usos fraudulentos) y el subtítulo en **Sum in last few minutes** (Suma en los últimos minutos). Haga clic en **Aplicar**. El icono se guarda en el panel.

    ![Captura de pantalla de especificación de título y subtítulo de icono del panel.](./media/event-hubs-tutorial-visualize-anomalies/power-bi-tile-details.png)

    > [!IMPORTANT]
    > Al ejecutar la aplicación de ejemplo y transmitir datos al centro de eventos, el número en este icono cambia rápidamente (cada segundo). El motivo es que la consulta de Stream Analytics actualiza realmente el valor **cada segundo**. Actualice la consulta a una ventana de saltos de tamaño constante de 3 minutos para ver la suma en los últimos minutos. 
11. Agregue otra visualización. Vuelva a repetir los primeros pasos:

    * Haga clic en **Agregar icono**.
    * Seleccione **Datos de streaming personalizados**. 
    * Haga clic en **Next**.
    * Seleccione el conjunto de datos y, a continuación, haga clic en **Siguiente**. 

12. En **Tipo de visualización**, seleccione **Gráfico de líneas**.

13. En **Eje**, haga clic en **Agregar valor** y seleccione `windowend`. 

14. En **Valores**, haga clic en **Agregar valor** y seleccione `fraudulentuses`.

15. En **Time window to display** (Período de tiempo para mostrar), seleccione los últimos 10 minutos. Haga clic en **Next**.

16. Especifique **Show fraudulent uses over time** (Mostrar usos fraudulentos con el tiempo) como título y deje el subtítulo del icono en blanco; a continuación, haga clic en **Aplicar**. Se le redirigirá a su panel.

17. Ejecute de nuevo la aplicación de detección de anomalías para enviar algunos datos al centro de eventos. Verá que el icono **Fraudulent uses** (Usos fraudulentos) cambia a medida que se analizan los datos y que el gráfico de líneas muestra datos. 

    ![Captura de pantalla que muestra los resultados de Power BI](./media/event-hubs-tutorial-visualize-anomalies/power-bi-results.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere quitar todos los recursos que ha creado, quite los datos de visualización de Power BI y, luego, elimine el grupo de recursos. Al eliminar un grupo de recursos se eliminan todos los recursos contenidos en él. En este caso, se quita el centro de eventos, el espacio de nombres de Event Hubs, el trabajo de Stream Analytics y el grupo de recursos. 

### <a name="clean-up-resources-in-the-power-bi-visualization"></a>Limpieza de recursos en la visualización de Power BI

Inicie sesión en su cuenta de Power BI. Vaya a **Mi área de trabajo**. En la línea con el nombre del panel, haga clic en el icono de la papelera. A continuación, vaya a **Conjuntos de datos** y haga clic en el icono de papelera para eliminar el conjunto de datos (**contosoehdataset**).

### <a name="clean-up-resources"></a>Limpieza de recursos
Elimine el grupo de recursos que contiene todos los recursos que creó como parte de este tutorial. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:
> [!div class="checklist"]
> * Creación de un espacio de nombres de Event Hubs
> * Creación de un centro de eventos
> * Ejecutar la aplicación que simula eventos y los envía al centro de eventos
> * Configurar un trabajo de Stream Analytics para procesar los eventos enviados al centro
> * Configurar una visualización de Power BI para mostrar los resultados

Avance al siguiente artículo para más información sobre Azure Event Hubs.

> [!div class="nextstepaction"]
> [Introducción al envío de mensajes a Azure Event Hubs en .NET Standard](event-hubs-dotnet-standard-getstarted-send.md)

[create a free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
