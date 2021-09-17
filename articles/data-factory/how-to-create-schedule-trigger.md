---
title: Creación de desencadenadores programados
titleSuffix: Azure Data Factory & Azure Synapse
description: Aprenda a crear un desencadenador de Azure Data Factory o Azure Synapse Analytics que ejecute una canalización conforme a una programación.
author: chez-charlie
ms.author: chez
ms.reviewer: jburchel
ms.service: data-factory
ms.subservice: orchestration
ms.topic: conceptual
ms.date: 08/24/2021
ms.custom: devx-track-python, devx-track-azurepowershell, synapse
ms.openlocfilehash: 833800da17302d2f28619cd1f66acfc476175a7f
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122824619"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Creación de un desencadenador que ejecuta una canalización en una programación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se proporciona información sobre el desencadenador de programación y los pasos para crear, iniciar y supervisar un desencadenador de programación. Para otros tipos de desencadenadores, consulte [Ejecución y desencadenadores de canalización](concepts-pipeline-execution-triggers.md).

Al crear un desencadenador de programación, especifique una programación (fecha de inicio, periodicidad, fecha de finalización, etc.) para el desencadenador y asócielo con una canalización. Las canalizaciones y los desencadenadores tienen una relación de varios a varios. Varios desencadenadores pueden comenzar una única canalización. Un único desencadenador puede iniciar varias canalizaciones.

En las secciones siguientes se proporcionan los pasos necesarios para crear un desencadenador de programación de diferentes maneras. 

## <a name="ui-experience"></a>Experiencia de UI

Puede crear un **programador de desencadenador** para programar la ejecución de una canalización periódicamente (cada hora, diariamente, etc.). 

> [!NOTE]
> Para obtener un tutorial completo acerca de cómo crear una canalización y un desencadenador de programación, que asocia el desencadenador a la canalización y ejecuta y supervisa la canalización, consulte [Inicio rápido: Creación de una factoría de datos con la interfaz de usuario de Azure Data Factory](quickstart-create-data-factory-portal.md).

1. Vaya a la pestaña **Editar** de Data Factory o a la pestaña Integrar de Azure Synapse. 

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)
    ![Cambio a la pestaña Edit (Editar)](./media/how-to-create-schedule-trigger/switch-edit-tab.png)

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)
    ![Cambio a la pestaña Edit (Editar)](./media/how-to-create-schedule-trigger/switch-edit-tab-synapse.png)

---
    
2. Seleccione **Trigger** (Desencadenador) en el menú y, después, seleccione **New/Edit** (Nuevo/Editar). 

    ![Menú Nuevo desencadenador](./media/how-to-create-schedule-trigger/new-trigger-menu.png)

1. En la página **Add Triggers** (Agregar desencadenadores), seleccione **Choose trigger...** (Elegir desencadenador) y, después, seleccione **+New** (+Nuevo). 

    ![Add Triggers (Agregar desencadenadores): nuevo desencadenador](./media/how-to-create-schedule-trigger/add-trigger-new-button.png)

1. En la página **New Trigger** (Nuevo desencadenador), lleve a cabo los siguientes pasos: 

    1. Asegúrese de que **Programación** está seleccionado para **Tipo**.
    1. Especifique la fecha y hora de inicio del desencadenador para **Fecha de inicio**. De forma predeterminada, se establece la fecha y hora actuales en la hora universal coordinada (UTC).
    1. Especifique la zona horaria en la que se creará el desencadenador. La configuración de zona horaria se aplicará a **Fecha de inicio**, **Fecha de finalización** y **Programar tiempos de ejecución** en las opciones avanzadas de periodicidad. Al cambiar la configuración de la zona horaria no se cambiará automáticamente la fecha de inicio. Asegúrese de que la fecha de inicio es correcta en la zona horaria especificada. Tenga en cuenta que el tiempo de ejecución programado del desencadenador se considerará posterior a la fecha de inicio (asegúrese de que la fecha de inicio es de al menos 1 minuto menos que el tiempo de ejecución; de lo contrario, se desencadenará la canalización en la siguiente periodicidad). 

        > [!NOTE]
        > En el caso de las zonas horarias que se adaptan a la hora de verano, la hora del desencadenador se ajustará automáticamente en los dos cambios que se producen al año. Para no cambiar en función del horario de verano, seleccione una zona horaria que no se adapte al horario de verano, como UTC.

    1. Especifique **Periodicidad** para el desencadenador. Seleccione uno de los valores de la lista desplegable (cada minuto, cada hora, diariamente, semanalmente y mensualmente). Introduzca el multiplicador en el cuadro de texto. Por ejemplo, si desea que el desencadenador se ejecute una vez para cada 15 minutos, seleccione **Cada minuto** y escriba **15** en el cuadro de texto. 
    1. En **Periodicidad**, si elige "Días, Semanas o Meses" en la lista desplegable, puede encontrar "Opciones avanzadas de periodicidad".
    :::image type="content" source="./media/how-to-create-schedule-trigger/advanced.png" alt-text="Opciones avanzadas de periodicidad de Días, Semanas o Meses":::
    1. Para especificar una fecha y hora de finalización, seleccione **Especificar una fecha de finalización**, especifique _Termina el_ y luego seleccione **Aceptar**. Hay un costo asociado a cada ejecución de canalización. Si está realizando pruebas, es posible que quiera asegurarse de que la canalización se desencadena solo un par de veces. No obstante, asegúrese de que hay tiempo suficiente para que la canalización se ejecute entre la hora de publicación y la hora de finalización. El desencadenador se aplica después de publicar la solución, no cuando se guarda en la interfaz de usuario.

        ![Configuración del desencadenador](./media/how-to-create-schedule-trigger/trigger-settings-01.png)

        ![Configuración del desencadenador para la fecha de finalización](./media/how-to-create-schedule-trigger/trigger-settings-02.png)

1. En la ventana **New Trigger** (Nuevo desencadenador), seleccione **Yes** (Sí) en la opción **Activated** (Activado) y, a continuación, seleccione **OK** (Aceptar). Puede utilizar esta casilla para desactivar el desencadenador más adelante. 

    ![Configuración del desencadenador: botón Siguiente](./media/how-to-create-schedule-trigger/trigger-settings-next.png)

1. En la página **New Trigger** (Nuevo desencadenador), revise el mensaje de advertencia y seleccione **Finish** (Finalizar).

    ![Configuración del desencadenador: botón Finalizar](./media/how-to-create-schedule-trigger/new-trigger-finish.png)

1. Seleccione **Publicar todo** para publicar los cambios. El desencadenador no inicia las ejecuciones de canalización hasta que se publican los cambios. 

    ![Botón Publicar](./media/how-to-create-schedule-trigger/publish-2.png)

1. Cambie a la pestaña **Pipeline runs** (Ejecuciones de canalización) de la izquierda y seleccione **Refresh** (Actualizar) para actualizar la lista. Verá que el desencadenador programado ejecuta las ejecuciones de la canalización. Observe los valores de la columna **Triggered By** (Desencadenado por). Si usa la opción **Trigger Now** (Desencadenar ahora), verá la ejecución del desencadenador manual en la lista. 

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    ![Supervisión de las ejecuciones desencadenadas](./media/how-to-create-schedule-trigger/monitor-triggered-runs.png)

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)
    ![Supervisión de las ejecuciones desencadenadas](./media/how-to-create-schedule-trigger/monitor-triggered-runs-synapse.png)
    
---

9. Cambie a la vista **Trigger Runs** (Ejecuciones de desencadenador) \ **Programación**. 

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    ![Supervisión de las ejecuciones del desencadenador](./media/how-to-create-schedule-trigger/monitor-trigger-runs.png)

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)
    ![Supervisión de las ejecuciones del desencadenador](./media/how-to-create-schedule-trigger/monitor-trigger-runs-synapse.png)
    
---

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

En esta sección se muestra cómo usar Azure PowerShell para crear, iniciar y supervisar un desencadenador de la programación. Para ver este ejemplo en funcionamiento, primero debe seguir la [Guía de inicio rápido: Creación de una factoría de datos con Azure PowerShell](quickstart-create-data-factory-powershell.md). A continuación, agregue el código siguiente al método principal, de modo que se crea e inicia un desencadenador de programación que se ejecuta cada 15 minutos. El desencadenador está asociado a una canalización llamada **Adfv2QuickStartPipeline** que se crea como parte de la guía de inicio rápido.

1. Cree un archivo JSON llamado **MyTrigger.json** en la carpeta C:\ADFv2QuickStartPSH\ con el siguiente contenido:

    > [!IMPORTANT]
    > Antes de guardar el archivo JSON, establezca el valor del elemento **startTime** en la hora UTC actual. Establezca el valor del elemento **endTime** en una hora anterior a la hora UTC actual.

    ```json
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 15,
                    "startTime": "2017-12-08T00:00:00Z",
                    "endTime": "2017-12-08T01:00:00Z",
                    "timeZone": "UTC"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "Adfv2QuickStartPipeline"
                    },
                    "parameters": {
                        "inputPath": "adftutorial/input",
                        "outputPath": "adftutorial/output"
                    }
                }
            ]
        }
    }
    ```

    En el fragmento de código de JSON:
    - El elemento **type** del desencadenador se establece en "ScheduleTrigger".
    - El elemento **frequency** se establece en "Minute" y el elemento **interval** se establece en 15. Por lo tanto, el desencadenador ejecuta la canalización cada 15 minutos entre las horas de inicio y finalización.
    - El elemento **timeZone** especifica la zona horaria en la que se crea el desencadenador. Esta configuración afecta a **startTime** y **endTime**.
    - El elemento **endTime** es una hora después del valor del elemento **startTime**. Por lo tanto, el desencadenador ejecuta la canalización 15 minutos, 30 minutos y 45 minutos después de la hora de inicio. No se olvide de actualizar el valor de start_time a la hora UTC actual y end_time a una hora posterior a la hora de inicio. 

        > [!IMPORTANT]
        > En el caso de la zona horaria UTC, los valores de startTime y endTime deben seguir el formato "aaaa-MM-ddTHH:mm:SS **Z**", mientras que para otras zonas horarias, startTime y endTime siguen "aaaa-MM-ddTHH:mm:ss". 
        > 
        > De acuerdo con la norma ISO 8601, el sufijo _Z_ añadido a la marca de tiempo señala la fecha y hora en la zona horaria UTC y hace innecesario el campo de zona horaria. Si falta el sufijo _Z_ para la zona horaria UTC, se producirá un error en la _activación_ del desencadenador.

    - El desencadenador está asociado con la canalización **Adfv2QuickStartPipeline**. Para asociar varias canalizaciones con un desencadenador, agregue más secciones **pipelineReference**.
    - La canalización de la guía de inicio rápido toma dos valores de **parámetros**: **inputPath** y **outputPath**. Y debe pasar valores para esos parámetros desde el desencadenador.

1. Cree un desencadenador mediante el cmdlet **Set-AzDataFactoryV2Trigger**:

    ```powershell
    Set-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```

1. Confirme que el estado del desencadenador es **Detenido** mediante el cmdlet **Get-AzDataFactoryV2Trigger**:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

1. Inicie el desencadenador mediante el cmdlet **Start-AzDataFactoryV2Trigger**:

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

1. Confirme que el estado del desencadenador es **Iniciado** mediante el cmdlet **Get-AzDataFactoryV2Trigger**:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

1.  Haga que el desencadenador se ejecute en Azure PowerShell mediante el cmdlet **Get-AzDataFactoryV2TriggerRun**. Para obtener información acerca de cómo se ejecuta el desencadenador, ejecute el siguiente comando periódicamente. Actualice los valores **TriggerRunStartedAfter** y **TriggerRunStartedBefore** para que coincidan con los valores de la definición del desencadenador:

    ```powershell
    Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
    > [!NOTE]
    > La hora del desencadenador de los desencadenadores de programación se especifica en la marca de tiempo UTC. _TriggerRunStartedAfter_ y _TriggerRunStartedBefore_ esperan también una marca de tiempo UTC.

    Para supervisar las ejecuciones del desencadenador y de la canalización en Azure Portal, consulte la sección acerca de la [supervisión de ejecuciones de canalización](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="net-sdk"></a>.NET SDK

En esta sección se muestra cómo usar el SDK de.NET para crear, iniciar y supervisar un desencadenador. Para ver este ejemplo en funcionamiento, primero debe seguir los pasos que se incluyen en [Creación de una factoría de datos y una canalización con SDK de .NET](quickstart-create-data-factory-dot-net.md). A continuación, agregue el código siguiente al método principal, de modo que se crea e inicia un desencadenador de programación que se ejecuta cada 15 minutos. El desencadenador está asociado a una canalización llamada **Adfv2QuickStartPipeline** que se crea como parte de la guía de inicio rápido.

Para crear e iniciar un desencadenador de programación que se ejecuta cada 15 minutos, agregue el código siguiente al método principal:

```csharp
            // Create the trigger
            Console.WriteLine("Creating the trigger");

            // Set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // Specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // Create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // Associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // Set the start time to the current UTC time and the end time to one hour after the start time
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // Now, create the trigger by invoking the CreateOrUpdate method
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            // Start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);
```

Para crear desencadenadores en una zona horaria distinta a la hora UTC, se requiere la siguiente configuración:
```csharp
<<ClientInstance>>.SerializationSettings.DateFormatHandling = Newtonsoft.Json.DateFormatHandling.IsoDateFormat;
<<ClientInstance>>.SerializationSettings.DateTimeZoneHandling = Newtonsoft.Json.DateTimeZoneHandling.Unspecified;
<<ClientInstance>>.SerializationSettings.DateParseHandling = DateParseHandling.None;
<<ClientInstance>>.DeserializationSettings.DateParseHandling = DateParseHandling.None;
<<ClientInstance>>.DeserializationSettings.DateFormatHandling = Newtonsoft.Json.DateFormatHandling.IsoDateFormat;
<<ClientInstance>>.DeserializationSettings.DateTimeZoneHandling = Newtonsoft.Json.DateTimeZoneHandling.Unspecified;
```

Para supervisar una ejecución de desencadenador, agregue el código siguiente a la última instrucción `Console.WriteLine` del ejemplo:

```csharp
            // Check that the trigger runs every 15 minutes
            Console.WriteLine("Trigger runs. You see the output every 15 minutes");

            for (int i = 0; i < 3; i++)
            {
                System.Threading.Thread.Sleep(TimeSpan.FromMinutes(15));
                List<TriggerRun> triggerRuns = client.Triggers.ListRuns(resourceGroup, dataFactoryName, triggerName, DateTime.UtcNow.AddMinutes(-15 * (i + 1)), DateTime.UtcNow.AddMinutes(2)).ToList();
                Console.WriteLine("{0} trigger runs found", triggerRuns.Count);

                foreach (TriggerRun run in triggerRuns)
                {
                    foreach (KeyValuePair<string, string> triggeredPipeline in run.TriggeredPipelines)
                    {
                        PipelineRun triggeredPipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, triggeredPipeline.Value);
                        Console.WriteLine("Pipeline run ID: {0}, Status: {1}", triggeredPipelineRun.RunId, triggeredPipelineRun.Status);
                        List<ActivityRun> runs = client.ActivityRuns.ListByPipelineRun(resourceGroup, dataFactoryName, triggeredPipelineRun.RunId, run.TriggerRunTimestamp.Value, run.TriggerRunTimestamp.Value.AddMinutes(20)).ToList();
                    }
                }
            }
```

Para supervisar las ejecuciones del desencadenador y de la canalización en Azure Portal, consulte la sección acerca de la [supervisión de ejecuciones de canalización](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="python-sdk"></a>SDK de Python

En esta sección se muestra cómo usar el SDK de Python para crear, iniciar y supervisar un desencadenador. Para ver este ejemplo en funcionamiento, primero debe seguir la [Creación de una factoría de datos y una canalización con SDK de Python](quickstart-create-data-factory-python.md). A continuación, agregue el siguiente bloque de código después del bloque de código para "supervisar la ejecución de canalización" en el script de Python. Este código crea un desencadenador de programación que se ejecuta cada 15 minutos entre las horas de inicio y finalización especificadas. Actualice la variable **start_time** a la hora UTC actual y la variable **end_time** a una hora posterior a la hora UTC actual.

```python
    # Create a trigger
    tr_name = 'mytrigger'
    scheduler_recurrence = ScheduleTriggerRecurrence(frequency='Minute', interval='15',start_time='2017-12-12T04:00:00Z', end_time='2017-12-12T05:00:00Z', time_zone='UTC')
    pipeline_parameters = {'inputPath':'adftutorial/input', 'outputPath':'adftutorial/output'}
    pipelines_to_run = []
    pipeline_reference = PipelineReference('copyPipeline')
    pipelines_to_run.append(TriggerPipelineReference(pipeline_reference, pipeline_parameters))
    tr_properties = ScheduleTrigger(description='My scheduler trigger', pipelines = pipelines_to_run, recurrence=scheduler_recurrence)    
    adf_client.triggers.create_or_update(rg_name, df_name, tr_name, tr_properties)

    # Start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```

Para supervisar las ejecuciones del desencadenador y de la canalización en Azure Portal, consulte la sección acerca de la [supervisión de ejecuciones de canalización](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="azure-resource-manager-template"></a>Plantilla del Administrador de recursos de Azure

Puede usar una plantilla de Azure Resource Manager para crear un desencadenador. Para obtener instrucciones detalladas, consulte [Creación de una instancia de Azure Data Factory mediante una plantilla de Resource Manager](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Transmisión de la hora de inicio del desencadenador a una canalización

La versión 1 de Azure Data Factory admite la lectura y la escritura de datos con particiones por medio de las variables del sistema **SliceStart**, **SliceEnd**, **WindowStart** y **WindowEnd**. En la versión actual de canalizaciones de Azure Data Factory y Synapse, puede lograr este comportamiento con un parámetro de canalización. La hora de inicio y la hora programada para el desencadenador se establecen como el valor del parámetro de la canalización. En el siguiente ejemplo, la hora programada del desencadenador se pasa como un valor al parámetro de canalización **scheduledRunTime**.

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```

## <a name="json-schema"></a>Esquema JSON

La siguiente definición de JSON le muestra cómo crear un desencadenador de programación con la programación y periodicidad:

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // Optional, specifies how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
          "hours": [<<0-23>>],
          "weekDays": [<<Monday-Sunday>>],
          "minutes": [<<0-59>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurrences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ]
        }
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
>  La propiedad **parameters** es una propiedad obligatoria del elemento **pipelines**. Si la canalización no toma ningún parámetro, incluya una definición de JSON vacía para la propiedad **parameters**.


### <a name="schema-overview"></a>Información general del esquema

En la tabla siguiente se muestra una descripción general de los elementos del esquema más importantes relacionados con la periodicidad y la programación de un desencadenador:

| Propiedad JSON | Descripción |
|:--- |:--- |
| **startTime** | Valor de fecha y hora. Para las programaciones simples, se aplica el valor de la propiedad **startTime** al primer caso. Para las programaciones complejas, el desencadenador no se inicia antes del valor de **startTime** especificado. <br> En el caso de la zona horaria UTC, el formato es `'yyyy-MM-ddTHH:mm:ssZ'`, para otra zona horaria, el formato es `'yyyy-MM-ddTHH:mm:ss'`. |
| **endTime** | Fecha y hora de finalización para el desencadenador. El desencadenador no se ejecuta después de la fecha y hora de finalización especificadas. El valor de la propiedad no puede estar en el pasado. Esta propiedad es opcional.  <br> En el caso de la zona horaria UTC, el formato es `'yyyy-MM-ddTHH:mm:ssZ'`, para otra zona horaria, el formato es `'yyyy-MM-ddTHH:mm:ss'`. |
| **timeZone** | Zona horaria en la que se crea el desencadenador. Esta configuración afecta a **startTime**, **endTime** y **schedule**. Vea la [lista de zonas horarias admitidas](#time-zone-option). |
| **recurrence** | Objeto que especifica las reglas de periodicidad para el desencadenador. El objeto recurrence admite los elementos **frequency**, **interval**, **endTime**, **count** y **schedule**. Cuando se define un objeto recurrence, es necesario el elemento **frequency**. Los demás elementos del objeto recurrence son opcionales. |
| **frequency** | Unidad de frecuencia a la que se repite el desencadenador. Los valores admitidos son "minute", "hour", "day", "week" y "month". |
| **interval** | Un entero positivo que indica el intervalo para el valor **frequency**, que determina la frecuencia con la que se ejecuta el desencadenador. Por ejemplo, si **interval** es 3 y **frequency** es “week”, el desencadenador se repite cada tres semanas. |
| **schedule** | La programación de periodicidad para el desencadenador. Un desencadenador con valor de **frequency** especificado modifica su periodicidad según una programación periódica. La propiedad **schedule** contiene modificaciones de la periodicidad basadas en minutos, horas, días de la semana, días del mes y número de semana.

> [!IMPORTANT]
> En el caso de la zona horaria UTC, los valores de startTime y endTime deben seguir el formato "aaaa-MM-ddTHH:mm:SS **Z**", mientras que para otras zonas horarias, startTime y endTime siguen "aaaa-MM-ddTHH:mm:ss". 
> 
> De acuerdo con la norma ISO 8601, el sufijo _Z_ añadido a la marca de tiempo señala la fecha y hora en la zona horaria UTC y hace innecesario el campo de zona horaria. Si falta el sufijo _Z_ para la zona horaria UTC, se producirá un error en la _activación_ del desencadenador.

### <a name="schema-defaults-limits-and-examples"></a>Valores predeterminados del esquema, límites y ejemplos

| Propiedad JSON | Tipo | Obligatorio | Valor predeterminado | Valores válidos | Ejemplo |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | String | Sí | None | Fechas-horas ISO-8601 | para la zona horaria UTC `"startTime" : "2013-01-09T09:30:00-08:00Z"` <br> para otra zona horaria `"2013-01-09T09:30:00-08:00"` |
| **timeZone** | String | Sí | None | [Valores de zona horaria](#time-zone-option)  | `"UTC"` |
| **recurrence** | Object | Sí | None | Objeto de periodicidad | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Number | No | 1 | De 1 a 1000 | `"interval":10` |
| **endTime** | String | Sí | None | Valor de fecha y hora que representa un período de tiempo en el futuro. | para la zona horaria UTC `"endTime" : "2013-02-09T09:30:00-08:00Z"` <br> para otra zona horaria `"endTime" : "2013-02-09T09:30:00-08:00"`|
| **schedule** | Object | No | None | Objeto de programación | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="time-zone-option"></a>Opción de zona horaria

Estas son algunas de las zonas horarias que se admiten para los desencadenadores de programación:

| Zona horaria | Diferencia horaria con UTC (sin horario de verano) | Valor de zona horaria | Adaptación al horario de verano | Formato de marca de hora |
| :--- | :--- | :--- | :--- | :--- |
| Hora universal coordinada | 0 | `UTC` | No | `'yyyy-MM-ddTHH:mm:ssZ'`|
| Hora del Pacífico (PT) | -8 | `Pacific Standard Time` | Sí | `'yyyy-MM-ddTHH:mm:ss'` |
| Hora central (CT) | -6 | `Central Standard Time` | Sí | `'yyyy-MM-ddTHH:mm:ss'` |
| Hora oriental (ET) | -5 | `Eastern Standard Time` | Sí | `'yyyy-MM-ddTHH:mm:ss'` |
| Hora del meridiano de Greenwich (GMT) | 0 | `GMT Standard Time` | Sí | `'yyyy-MM-ddTHH:mm:ss'` |
| Hora estándar centroeuropea | +1 | `W. Europe Standard Time` | Sí | `'yyyy-MM-ddTHH:mm:ss'` |
| Hora estándar de la India (IST) | +5:30 | `India Standard Time` | No | `'yyyy-MM-ddTHH:mm:ss'` |
| Hora estándar de China | +8 | `China Standard Time` | No | `'yyyy-MM-ddTHH:mm:ss'` |

Esta lista está incompleta. Para obtener una lista completa de las opciones de zona horaria, vea la [página de creación de desencadenadores](#ui-experience) del portal

### <a name="starttime-property"></a>Propiedad startTime
En la tabla siguiente se muestra cómo la propiedad **startTime** controla una ejecución de desencadenador:

| Valor de startTime | Periodicidad sin programación | Periodicidad con programación |
|:--- |:--- |:--- |
| Hora de inicio en el pasado | Calcula la primera hora de ejecución futura después de la hora de inicio y se ejecuta a esa hora.<br/><br/>Realiza ejecuciones posteriores según el cálculo desde la última hora de ejecución.<br/><br/>Consulte el ejemplo que sigue a esta tabla. | El desencadenador se inicia _no antes que_ la hora de inicio especificada. La primera repetición se basa en la programación que se calcula a partir de la hora de inicio.<br/><br/>Realiza las sucesivas ejecuciones según la programación de periodicidad. |
| Hora de inicio en el futuro o en el presente | Se ejecuta una vez a la hora de inicio especificada.<br/><br/>Realiza ejecuciones posteriores según el cálculo desde la última hora de ejecución. | El desencadenador se inicia _no antes_ que la hora de inicio especificada. La primera repetición se basa en la programación que se calcula a partir de la hora de inicio.<br/><br/>Realiza las sucesivas ejecuciones según la programación de periodicidad. |

Veamos un ejemplo de lo que sucede cuando startTime se encuentra en el pasado, con periodicidad, pero sin programación. Suponga que la hora actual es `2017-04-08 13:00`, la hora de inicio es `2017-04-07 14:00`, y la periodicidad es cada dos días. (El valor de **recurrence** se define estableciendo la propiedad **frequency** en "day" y la propiedad **interval** en 2). Tenga en cuenta que el valor de **startTime** se encuentra en el pasado y tiene lugar antes de la hora actual.

En estas condiciones, la primera ejecución será el `2017-04-09` a las `14:00`. El motor de Scheduler calcula las repeticiones de la ejecución desde la hora de inicio. Se descartan las instancias en el pasado. El motor utiliza la instancia siguiente que tiene lugar en el futuro. En este escenario, la hora de inicio será el `2017-04-07` a las `2:00pm`, así que la siguiente instancia es dos días a partir de ese momento, que es el `2017-04-09` a las `2:00pm`.

La hora de la primera ejecución es la misma incluso si el valor de **startTime** es `2017-04-05 14:00` o `2017-04-01 14:00`. Después de la primera ejecución, las ejecuciones posteriores se calculan mediante la programación. Por lo tanto, las ejecuciones posteriores se realizan el `2017-04-11` a las `2:00pm`, después el `2017-04-13` a las `2:00pm`, después el `2017-04-15` a las `2:00pm` y así sucesivamente.

Finalmente, cuando las horas o los minutos no se establecen en el programa para un desencadenador, se utilizan las horas o minutos de la primera ejecución como valores predeterminados.

### <a name="schedule-property"></a>Propiedad schedule

Por un lado, el uso de una programación puede limitar el número de ejecuciones de desencadenadores. Por ejemplo, si un desencadenador con frecuencia mensual tiene un valor programado para ejecutarse solo el día 31, el desencadenador se ejecuta solo en los meses que tienen 31 días.

En cambio, una programación también puede ampliar el número de ejecuciones de desencadenadores. Por ejemplo, un desencadenador con una frecuencia mensual programado para ejecutarse en los días 1 y 2 del mes, se ejecuta el primer y segundo día del mes, en lugar de una vez al mes.

Si se especifican varios elementos de **schedule**, el orden de evaluación es de la configuración de programación mayor a menor. La evaluación empieza por el número de semana y, después, el día del mes, el día de la semana, la hora y, finalmente, los minutos.

En la siguiente tabla se describen los elementos de **schedule** con detalle:

| Elemento JSON | Descripción | Valores válidos |
|:--- |:--- |:--- |
| **minutes** | Minutos de la hora en la que se ejecuta el desencadenador. | <ul><li>Entero</li><li>Matriz de enteros</li></ul>
| **hours** | Horas del día en la que se ejecuta el desencadenador. | <ul><li>Entero</li><li>Matriz de enteros</li></ul> |
| **weekDays** | Días de la semana en los que se ejecuta el desencadenador. El valor solo se puede especificar con una frecuencia semanal. | <ul><li>Lunes, martes, miércoles, jueves, viernes, sábado, domingo</li><li>Matriz de valores de día (el tamaño máximo de la matriz es 7)</li><li>Los valores de día no distinguen mayúsculas de minúsculas.</li></ul> |
| **monthlyOccurrences** | Días del mes en los que se ejecuta el desencadenador. El valor solo se puede especificar con una frecuencia mensual. | <ul><li>Matriz de objetos de **monthlyOccurrence**: `{ "day": day,  "occurrence": occurrence }`.</li><li>El atributo **day** es el día de la semana en el que se ejecuta el desencadenador. Por ejemplo, una propiedad **monthlyOccurrences** con un valor de **day** de `{Sunday}` significa todos los domingos del mes. Se necesita un atributo **day**.</li><li>El atributo **occurrence** es la repetición del elemento **day** especificado durante el mes. Por ejemplo, una propiedad **monthlyOccurrences** valores de **day** y **occurrence** de `{Sunday, -1}` implica el último domingo del mes. El atributo **occurrence** es opcional.</li></ul> |
| **monthDays** | Día del mes en el que se ejecuta el desencadenador. El valor solo se puede especificar con una frecuencia mensual. | <ul><li>Cualquier valor <= -1 y >= -31</li><li>Cualquier valor >= 1 y <= 31</li><li>Matriz de valores</li></ul> |

## <a name="examples-of-trigger-recurrence-schedules"></a>Ejemplos de programaciones de periodicidad del desencadenador

En esta sección se muestran ejemplos de programaciones de periodicidad, y se centra en el objeto **schedule** y sus elementos.

Los ejemplos asumen que el valor de **interval** es 1 y que el valor de **frequency** es correcto según la definición de la programación. Por ejemplo, no puede tener un valor de **frequency** de "day" y tener también una modificación de "monthDays" en el objeto **schedule**. Restricciones como estas se mencionan en la tabla de la sección anterior.

| Ejemplo | Descripción |
|:--- |:--- |
| `{"hours":[5]}` | Se ejecuta a las 5:00 a. m. todos los días. |
| `{"minutes":[15], "hours":[5]}` | Se ejecuta a las 5:15 a. m. todos los días. |
| `{"minutes":[15], "hours":[5,17]}` | Se ejecuta a las 5:15 a. m. y 5:15 p. m. todos los días. |
| `{"minutes":[15,45], "hours":[5,17]}` | Se ejecuta a las 5:15 a. m., 5:45 a. m., 5:15 p. m. y 5:45 p. m. todos los días. |
| `{"minutes":[0,15,30,45]}` | Se ejecuta cada 15 minutos. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Se ejecuta cada hora. Este desencadenador se ejecuta cada hora. Los minutos se controlan mediante el valor de **startTime**, cuando se especifica un valor. Si no se especifica un valor, los minutos los controla el tiempo de creación. Por ejemplo, si la hora de inicio o la hora de creación (lo que corresponda) es 12:25 p. m., el desencadenador se ejecuta a las 00:25, 01:25, 02:25… y 23:25.<br/><br/>Su programación equivale a tener un desencadenador con un valor de **frequency** de "hour", un valor de **interval** de 1 y ningún valor de **schedule**.  Esta programación puede usarse con diferentes valores en **frequency** e **interval** para crear otros desencadenadores. Por ejemplo, cuando el valor de **frequency** es "month", la ejecución de la programación se ejecuta solo una vez al mes, en lugar de cada día, cuando el valor de **frequency** es "day". |
| `{"minutes":[0]}` | Se ejecuta cada hora durante la hora. Este desencadenador se ejecuta cada hora a partir de las 12:00 a. m., 1:00 a. m., 2:00 a. m., y así sucesivamente.<br/><br/>Esta programación es equivalente a un desencadenador con un valor de **frequency** de "hour" y un valor de **startTime** de cero minutos, o sin valor de **schedule** pero con un valor de **frequency** de "day". Si el valor de **frequency** es "week" o "month", la programación ejecuta únicamente un día a la semana o un día al mes, respectivamente. |
| `{"minutes":[15]}` | Se ejecuta 15 minutos después de cada hora en punto. Este desencadenador se ejecuta cada hora 15 minutos después de la hora en punto desde las 00:15 a. m., 1:15 a. m., 2:15 a. m. y así sucesivamente, y finalizando a las 11:15 p. m. |
| `{"hours":[17], "weekDays":["saturday"]}` | Se ejecuta a las 5:00 p. m. los sábados de cada semana. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Se ejecuta a las 5:00 p. m. los lunes, miércoles y viernes de cada semana. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Se ejecuta a las 5:15 p. m. y 5:45 p. m. los lunes, miércoles y viernes de cada semana. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Se ejecuta cada 15 minutos los días laborables. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Se ejecuta cada 15 minutos los días laborables entre las 9:00 a. m. y las 4:45 p. m. |
| `{"weekDays":["tuesday", "thursday"]}` | Se ejecuta los martes y jueves a la hora de inicio especificada. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Se ejecuta a las 6:00 a. m. del día 28 de cada mes (suponiendo un valor de **frequency** de mes). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Se ejecuta a las 6:00 a. m. el último día del mes. Para ejecutar un desencadenador el último día del mes, use -1 en lugar del día 28, 29, 30 o 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Se ejecuta a las 6:00 a. m. el primer y el último día del mes. |
| `{monthDays":[1,14]}` | Se ejecuta el primer día y el 14 de cada mes a la hora de inicio especificada. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Se ejecuta el primer viernes de cada mes a las 5:00 a. m. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Se ejecuta el primer viernes de cada mes a la hora de inicio especificada. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Se ejecuta el tercer viernes desde el final del mes, todos los meses, a la hora de inicio especificada. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Se ejecuta el primer viernes de cada mes a las 5:15 a. m. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Se ejecuta el primer y el último viernes de cada mes a la hora de inicio especificada. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Se ejecuta el quinto viernes de cada mes a la hora de inicio especificada. Si no hay ningún quinto viernes en un mes, la canalización no se ejecuta, ya que se ha programado para ejecutarse solo el quinto viernes. Considere usar -1 en lugar de 5 para el valor **occurrence** para ejecutar el desencadenador en el último viernes del mes. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Se ejecuta cada 15 minutos el último viernes del mes. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Se ejecuta a las 5:15 a. m., 5:45 a. m., 5:15 a. m. y las 5:45 a. m. el tercer miércoles de cada mes. |

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información detallada acerca de los desencadenadores, consulte el artículo [Pipeline execution and triggers](concepts-pipeline-execution-triggers.md#trigger-execution) (Ejecución de canalizaciones y desencadenadores).
- Aprenda a hacer referencia a los metadatos de desencadenador en la canalización; para ello, consulte [Referencia a los metadatos de desencadenador en las ejecuciones de canalización](how-to-use-trigger-parameterization.md).
