---
title: Variables del sistema
titleSuffix: Azure Data Factory & Azure Synapse
description: En este artículo se describen las variables del sistema compatibles con Azure Data Factory y Azure Synapse Analytics. Puede usar estas variables en expresiones al definir entidades en cualquiera de los dos servicios.
author: chez-charlie
ms.author: chez
ms.reviewer: jburchel
ms.service: data-factory
ms.subservice: orchestration
ms.custom: synapse
ms.topic: conceptual
ms.date: 08/24/2021
ms.openlocfilehash: f0fa5503b52481afafe2a0a6be8e28f8a964464b
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122821625"
---
# <a name="system-variables-supported-by-azure-data-factory-and-azure-synapse-analytics"></a>Variables del sistema compatibles con Azure Data Factory y Azure Synapse Analytics

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describen las variables del sistema compatibles con Azure Data Factory y Azure Synapse. Puede usar estas variables en expresiones al definir entidades en cualquiera de los dos servicios.

## <a name="pipeline-scope"></a>Ámbito de canalización

Se puede hacer referencia a estas variables del sistema en cualquier JSON de la canalización.

| Nombre de la variable | Descripción |
| --- | --- |
| @pipeline().DataFactory |Nombre de los datos o el área de trabajo de Synapse donde se realiza la ejecución de la canalización |
| @pipeline().Pipeline |Nombre de la canalización |
| @pipeline().RunId |Id. de la ejecución de canalización específica |
| @pipeline().TriggerType |Tipo de desencadenador que ha invocado la canalización (por ejemplo, `ScheduleTrigger`, `BlobEventsTrigger`). Para obtener una lista de los tipos de desencadenadores compatibles, vea [Ejecución y desencadenadores de canalización](concepts-pipeline-execution-triggers.md). Un tipo de desencadenador `Manual` indica que la canalización se desencadenó manualmente. |
| @pipeline().TriggerId|Id. del desencadenador que ha invocado la canalización. |
| @pipeline().TriggerName|Nombre del desencadenador que ha invocado la canalización. |
| @pipeline().TriggerTime|Hora de la ejecución de desencadenador que ha invocado la canalización. Esta es la hora a la que el desencadenador **realmente** se activó para invocar la ejecución de la canalización y puede diferir ligeramente de la hora programada del desencadenador.  |
| @pipeline().GroupId | Identificador del grupo al que pertenece la ejecución de canalización. |
| @pipeline()?TriggeredByPipelineName | Nombre de la canalización que desencadena la ejecución de canalización. Se puede aplicar cuando una actividad ExecutePipeline desencadena la ejecución de canalización. Se evalúa como _NULL_ cuando se usa en otras circunstancias. Observe el signo de interrogación después de @pipeline() |
| @pipeline()?TriggeredByPipelineRunId | Identificador de ejecución de la canalización que desencadena la ejecución de canalización. Se puede aplicar cuando una actividad ExecutePipeline desencadena la ejecución de canalización. Se evalúa como _NULL_ cuando se usa en otras circunstancias. Observe el signo de interrogación después de @pipeline() |

>[!NOTE]
>Las variables del sistema de fecha y hora relacionadas con el desencadenador (en los ámbitos de la canalización y el desencadenador) devuelven las fechas locales en formato ISO 8601; por ejemplo, `2017-06-01T22:20:00.4061448Z`.

## <a name="schedule-trigger-scope"></a>Ámbito del desencadenador de programación

Se puede hacer referencia a estas variables del sistema en cualquier parte del JSON de desencadenador para los desencadenadores de tipo [ScheduleTrigger](concepts-pipeline-execution-triggers.md#schedule-trigger).

| Nombre de la variable | Descripción |
| --- | --- |
| @trigger().scheduledTime |Hora a la que se programó el desencadenador para invocar la ejecución de la canalización. |
| @trigger().startTime |Hora a la que **realmente** se activó el desencadenador para invocar la ejecución de la canalización. Este valor puede diferir ligeramente de la hora programada del desencadenador. |

## <a name="tumbling-window-trigger-scope"></a>Ámbito de desencadenador periódico

Se puede hacer referencia a estas variables del sistema en cualquier parte del JSON del desencadenador para los desencadenadores de tipo [TumblingWindowTrigger](concepts-pipeline-execution-triggers.md#tumbling-window-trigger).

| Nombre de la variable | Descripción |
| --- | --- |
| @trigger().outputs.windowStartTime |Inicio de la ventana asociada a la ejecución del desencadenador. |
| @trigger().outputs.windowEndTime |Finalización de la ventana asociada a la ejecución del desencadenador. |
| @trigger().scheduledTime |Hora a la que se programó el desencadenador para invocar la ejecución de la canalización. |
| @trigger().startTime |Hora a la que **realmente** se activó el desencadenador para invocar la ejecución de la canalización. Este valor puede diferir ligeramente de la hora programada del desencadenador. |

## <a name="storage-event-trigger-scope"></a>Ámbito de desencadenador de eventos de almacenamiento

Se puede hacer referencia a estas variables del sistema en cualquier parte del JSON de desencadenador para los desencadenadores de tipo [BlobEventsTrigger](concepts-pipeline-execution-triggers.md#event-based-trigger).

| Nombre de la variable | Descripción |
| --- | --- |
| @triggerBody().fileName  |Nombre del archivo cuya creación o eliminación hizo que se activara el desencadenador.   |
| @triggerBody().folderPath  |Ruta de acceso a la carpeta que contiene el archivo especificado en `@triggerBody().fileName`. El primer segmento de la ruta de acceso de la carpeta es el nombre del contenedor de Azure Blob Storage.  |
| @trigger().startTime |Hora a la que se activó el desencadenador para invocar la ejecución de la canalización. |

## <a name="custom-event-trigger-scope"></a>Ámbito de desencadenador de eventos de personalización

Se puede hacer referencia a estas variables del sistema en cualquier parte del JSON de desencadenador para los desencadenadores de tipo [CustomEventsTrigger](concepts-pipeline-execution-triggers.md#event-based-trigger).

>[!NOTE]
>El servicio espera que los eventos personalizados tengan el formato del [Esquema de eventos de Azure Event Grid](../event-grid/event-schema.md).

| Nombre de la variable | Descripción
| --- | --- |
| @triggerBody().event.eventType | Tipo de eventos que desencadenaron la ejecución del desencadenador de eventos personalizados. El tipo de evento es el campo definido por el cliente y toma cualquier valor de tipo de cadena. |
| @triggerBody().event.subject | Asunto del evento personalizado que activó el desencadenador. |
| @triggerBody().event.data._keyName_ | El campo de datos de evento personalizado es un blob de JSON gratuito, que el cliente puede usar para enviar mensajes y datos. Use data._keyName_ para hacer referencia a cada campo. Por ejemplo, @triggerBody().event.data.callback devuelve el valor del campo de _devolución de llamada_ almacenado en _datos_. |
| @trigger().startTime | Hora a la que se activó el desencadenador para invocar la ejecución de la canalización. |

## <a name="next-steps"></a>Pasos siguientes

* Para información sobre cómo se usan estas variables en las expresiones, vea [Expression language & functions](control-flow-expression-language-functions.md) (Lenguaje de expresión y funciones).
* Para usar variables del sistema de ámbito de desencadenador en la canalización, consulte [Referencia de metadatos de desencadenador en canalización](how-to-use-trigger-parameterization.md).
