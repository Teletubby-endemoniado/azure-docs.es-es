---
title: Creación de tareas de automatización para administrar y supervisar recursos de Azure
description: Configure tareas automatizadas que lo ayuden a administrar los recursos de Azure y a supervisar los costos mediante la creación de flujos de trabajo que se ejecuten en Azure Logic Apps.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, azla
ms.topic: how-to
ms.date: 11/02/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: 436a9422adcc9f0e564d34c68d3ed0d5b9185151
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131079823"
---
# <a name="manage-azure-resources-and-monitor-costs-by-creating-automation-tasks-preview"></a>Administración de los recursos de Azure y supervisión de los costos mediante la creación de tareas de automatización (versión preliminar)

> [!IMPORTANT]
> Esta funcionalidad está en versión preliminar y está sujeta a las [Condiciones de uso complementarias para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Para que pueda administrar los [recursos de Azure](../azure-resource-manager/management/overview.md#terminology) de manera más sencilla, puede crear tareas de administración automatizadas para un recurso o un grupo de recursos específico mediante el uso de plantillas de tareas de automatización, cuya disponibilidad varía en función del tipo de recurso. Por ejemplo, para una [cuenta de Azure Storage](../storage/common/storage-account-overview.md), puede configurar una tarea de automatización que le envíe el costo mensual de esa cuenta de almacenamiento. En el caso de una [máquina virtual de Azure](https://azure.microsoft.com/services/virtual-machines/), puede crear una tarea de automatización que active o desactive esa máquina virtual según una programación predefinida.

A continuación se muestran las plantillas de tareas actualmente disponibles en esta versión preliminar:

| Tipo de recurso | Plantillas de tareas de automatización |
|---------------|---------------------------|
| Grupos de recursos de Azure | **Cuando se elimina el recurso** |
| Todos los recursos de Azure | **Enviar el costo mensual del recurso** |
| Azure Virtual Machines | Además: <p>- **Apagar la máquina virtual** <br>- **Iniciar la máquina virtual** |
| Cuentas de Azure Storage | Además: <p>- **Eliminar blobs antiguos** |
| Azure Cosmos DB | Además: <p>- **Enviar el resultado de la consulta por correo electrónico** |
|||

En este artículo se muestra cómo completar las tareas siguientes:

* [Crear una tarea de automatización](#create-automation-task) para un recurso específico de Azure.

* [Revisar el historial de una tarea](#review-task-history), que incluye el estado de la ejecución, las entradas, las salidas y otra información histórica.

* [Editar la tarea](#edit-task), para que pueda actualizarla o personalizar su flujo de trabajo subyacente en el diseñador de flujos de trabajo.

<a name="differences"></a>

## <a name="how-do-automation-tasks-differ-from-azure-automation"></a>¿Qué diferencias hay entre las tareas de automatización y Azure Automation?

Las tareas de automatización son más básicas y ligeras que [Azure Automation](../automation/automation-intro.md). Actualmente, las tareas de automatización solo se pueden en el nivel de recursos de Azure. En segundo plano, una tarea de automatización es realmente un recurso de aplicación lógica que ejecuta un flujo de trabajo y cuenta con la tecnología del [*servicio Azure Logic Apps* multiinquilino](logic-apps-overview.md). Después de crear la tarea de automatización, puede ver y editar el flujo de trabajo subyacente abriendo la tarea en el diseñador de flujos de trabajo. Cuando una tarea finaliza al menos una ejecución, se puede revisar el estado de la tarea, el historial de la ejecución del flujo de trabajo, las entradas y las salidas de cada ejecución.

En comparación, Azure Automation es un servicio de automatización y configuración basado en la nube que facilita una administración coherente en los entornos que se encuentran dentro y fuera de Azure. El servicio consta de la [automatización de procesos para organizarlos](../automation/automation-intro.md#process-automation) a través del uso de [runbooks](../automation/automation-runbook-execution.md), administración de configuración con [Change Tracking e Inventario](../automation/change-tracking/overview.md), administración de actualizaciones, funcionalidades compartidas y características heterogéneas. Azure proporciona un control completo durante la implementación, las operaciones y la retirada de las cargas de trabajo y recursos.

<a name="pricing"></a>

## <a name="pricing"></a>Precios

Al crear una tarea de automatización, los cargos no se inician automáticamente. De forma subyacente, una tarea de automatización está basada en un flujo de trabajo de un recurso de aplicación lógica que se hospeda en una aplicación multiinquilino de Azure Logic Apps. Por lo tanto, se aplica el [modelo de precios de consumo](logic-apps-pricing.md) a las tareas de automatización. La medición y facturación se basan en las ejecuciones del desencadenador y de la acción en el flujo de trabajo de la aplicación lógica subyacente.

Las ejecuciones se miden y facturan, independientemente de si el flujo de trabajo se ejecuta correctamente o de si se crea una instancia del flujo de trabajo. Por ejemplo, suponga que la tarea de automatización usa un desencadenador de sondeo que realiza regularmente una llamada saliente a un punto de conexión. Esta solicitud de salida se mide y factura como una ejecución, independientemente de si el desencadenador se activa o se omite, lo que afecta a si se crea una instancia de flujo de trabajo.

Tanto los desencadenadores como las acciones utilizan las [tarifas del plan de consumo](https://azure.microsoft.com/pricing/details/logic-apps/), que difieren en función de si estas operaciones son ["integradas"](../connectors/built-in.md) o ["administradas" (Estándar o Enterprise).](../connectors/managed.md) Los desencadenadores y las acciones también realizan transacciones de almacenamiento, que usan la [tarifa de datos del plan de consumo](https://azure.microsoft.com/pricing/details/logic-apps/).

> [!TIP]
> Como bonificación mensual, el plan de consumo incluye *varios miles* de ejecuciones integradas gratuitas. Para obtener información específica, consulte las [tarifas del plan de consumo](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="prerequisites"></a>Prerrequisitos

* Una cuenta y una suscripción de Azure. Si aún no tiene una, [regístrese para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* El recurso de Azure que quiere administrar. En este artículo se usa una cuenta de Azure Storage como ejemplo.

* Una cuenta de Office 365 si quiere seguir con el ejemplo, que le envía correo electrónico mediante Office 365 Outlook.

<a name="create-automation-task"></a>

## <a name="create-an-automation-task"></a>Creación de una tarea de automatización

1. En [Azure Portal](https://portal.azure.com), busque el recurso que quiere administrar.

1. En el menú de navegación del recurso, desplácese a la sección **Automatización** y seleccione **Tareas (versión preliminar)** .

   ![Captura de pantalla que muestra Azure Portal y el menú del recurso de cuenta de almacenamiento con la opción "Tareas (versión preliminar)" seleccionada.](./media/create-automation-tasks-azure-resources/storage-account-menu-automation-section.png)

1. En el panel **Tareas**, seleccione **Agregar una tarea** para que pueda seleccionar una plantilla de tarea.

   ![Captura de pantalla que muestra el panel "Tareas (versión preliminar)" con la opción "Agregar una tarea" seleccionada.](./media/create-automation-tasks-azure-resources/add-automation-task.png)

1. En el panel **Agregar una tarea**, en **Seleccionar una plantilla**, en la plantilla de la tarea de replicación que quiere crear, seleccione **Seleccionar**. Si no aparece la página siguiente, seleccione **Siguiente: Autenticar**.

   En este ejemplo se sigue seleccionando la plantilla de tarea **Enviar el costo mensual del recurso**.

   ![Captura de pantalla que muestra el panel "Agregar una tarea" con la plantilla "Enviar costo mensual del recurso" seleccionada.](./media/create-automation-tasks-azure-resources/select-task-template.png)

1. En **Autenticar**, en la sección **Conexiones**, seleccione **Crear** para cada conexión que aparece en la tarea a fin de poder proporcionar credenciales de autenticación para todas las conexiones. Los tipos de conexiones de cada tarea varían según la tarea.

   En este ejemplo solo se muestra una de las conexiones que requiere esta tarea.

   ![Captura de pantalla que muestra seleccionada la opción "Crear" para la conexión de Azure Resource Manager.](./media/create-automation-tasks-azure-resources/create-authenticate-connections.png)

1. Cuando se indique, inicie sesión con las credenciales de su cuenta de Azure.

   ![Captura de pantalla que muestra la selección "Iniciar sesión".](./media/create-automation-tasks-azure-resources/create-connection-sign-in.png)

   Cada conexión correctamente autenticada tiene un aspecto similar al ejemplo siguiente:

   ![Captura de pantalla que muestra una conexión correctamente creada.](./media/create-automation-tasks-azure-resources/create-connection-success.png)

1. Después de autenticar todas las conexiones, seleccione **Siguiente: Configurar** si no aparece la página siguiente.

1. En **Configurar**, proporcione un nombre y cualquier otra información necesaria para la tarea. Seleccione **Revisar y crear** cuando haya terminado.

   > [!NOTE]
   > No puede cambiar el nombre de la tarea después de crearla, por lo que debe considerar un nombre que se siga aplicando si [edita el flujo de trabajo subyacente](#edit-task-workflow). Los cambios que haga en el flujo de trabajo subyacente solo se aplican a la tarea que creó y no a la plantilla de tarea.
   >
   > Por ejemplo, si le asigna el nombre `SendMonthlyCost` a la tarea, pero después edita el flujo de trabajo subyacente para que se ejecute de manera semanal, no podrá cambiar el nombre de la tarea a `SendWeeklyCost`.

   Las tareas que envían notificaciones por correo electrónico requieren una dirección de correo electrónico.

   ![Captura de pantalla que muestra la información necesaria para la tarea seleccionada.](./media/create-automation-tasks-azure-resources/provide-task-information.png)

   La tarea que ha creado, que se activa y ejecuta de manera automática, ahora aparece en la lista **Tareas**.

   ![Captura de pantalla que muestra la lista de tareas de automatización.](./media/create-automation-tasks-azure-resources/automation-tasks-list.png)

   > [!TIP]
   > Si la tarea no aparece de inmediato, intente actualizar la tarea de listas o espere un poco antes de actualizar. En la barra de herramientas, seleccione **Actualizar**.

   Una vez que se ejecute la tarea seleccionada, recibirá un correo electrónico similar al siguiente:

   ![Captura de pantalla que muestra la notificación de correo electrónico enviada por la tarea.](./media/create-automation-tasks-azure-resources/email-notification-received.png)

<a name="review-task-history"></a>

## <a name="review-task-history"></a>Revisión del historial de una tarea

Para ver el historial de ejecuciones de una tarea junto con sus estados, entradas, salidas y otro tipo de información, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), busque el recurso que tiene el historial de la tarea que quiere revisar.

1. En el menú del recurso, en **Configuración**, en la sección **Automatización**, seleccione **Tareas (versión preliminar)** .

1. En la lista de tareas, busque la tarea que quiere revisar. En la columna **Ejecuciones** de la tarea, seleccione **Ver**.

   ![Captura de pantalla que muestra una tarea y la opción "Ver" seleccionada.](./media/create-automation-tasks-azure-resources/view-runs-for-task.png)

   En el panel **Historial de ejecuciones** se muestran todas las ejecuciones de la tarea junto con sus estados, horas de inicio, identificadores y las duraciones de cada ejecución.

   ![Captura de pantalla que muestra las ejecuciones de una tarea, sus estados y otro tipo de información.](./media/create-automation-tasks-azure-resources/view-runs-history.png)

   Estos son los estados posibles de una ejecución:

   | Estado | Descripción |
   |--------|-------------|
   | **Cancelado** | La tarea se canceló durante la ejecución. |
   | **Erróneo** | La tarea tiene al menos una acción con errores, pero no hubo acciones posteriores para resolver el error. |
   | **Ejecución** | La tarea está en ejecución. |
   | **Correcto** | Todas las acciones correctas. Una tarea puede finalizar correctamente de todos modos si se produjo un error en una acción, pero hubo una acción posterior para resolver dicho error. |
   | **En espera** | La ejecución todavía no se inicia y está en pausa porque una instancia anterior de la tarea todavía está en ejecución. |
   |||

   Para más información, consulte [Revisión del historial de ejecuciones en la vista de supervisión](monitor-logic-apps.md#review-runs-history).

1. Para ver los estados y otro tipo de información de cada paso de una ejecución, seleccione dicha ejecución.

   Se abre el panel **Ejecución de aplicación lógica** y muestra el flujo de trabajo subyacente que se ejecutó.

   * Un flujo de trabajo siempre se inicia con un [*desencadenador*](../connectors/apis-list.md#triggers). En esta tarea, el flujo de trabajo se inicia con el desencadenador [**Periodicidad**](../connectors/connectors-native-recurrence.md).

   * Cada paso muestra su estado y la duración de la ejecución. Los pasos que tienen duraciones de 0 segundos tardan menos 1 segundo en ejecutarse.

   ![Captura de pantalla que muestra cada paso de la ejecución, el estado y la duración de la ejecución.](./media/create-automation-tasks-azure-resources/runs-history-details.png)

1. Para revisar las entradas y salidas de cada paso, selecciónelo y se expandirá.

   En este ejemplo se muestran las entradas para el desencadenador de periodicidad, que no tiene ninguna salida, porque el desencadenador solo especifica cuándo se ejecuta el flujo de trabajo y no proporciona salidas para las acciones posteriores que se van a procesar.

   ![Captura de pantalla que muestra el desencadenador expandido y las entradas.](./media/create-automation-tasks-azure-resources/view-trigger-inputs.png)

   Por el contrario, la acción **Enviar un correo electrónico** tiene entradas provenientes de acciones anteriores en el flujo de trabajo, así como salidas.

   ![Captura de pantalla que muestra una acción expandida, las entradas y las salidas.](./media/create-automation-tasks-azure-resources/view-action-inputs-outputs.png)

Para obtener información sobre cómo puede crear sus propios flujos de trabajo a fin de poder integrar aplicaciones, datos, servicios y sistemas fuera del contexto de las tareas de automatización de los recursos de Azure, consulte [Inicio rápido: Creación del primer flujo de trabajo de integración automatizado con Azure Logic Apps: Azure Portal](quickstart-create-first-logic-app-workflow.md).

<a name="edit-task"></a>

## <a name="edit-the-task"></a>Edición de una tarea

Existen estas opciones si quiere modificar una tarea:

* [Editar la tarea "en línea"](#edit-task-inline) para que pueda cambiar las propiedades de la tarea, como la información de conexión o de configuración; por ejemplo, la dirección de correo electrónico.

* [Editar el flujo de trabajo subyacente de la tarea](#edit-task-workflow) en el diseñador de flujos de trabajo.

<a name="edit-task-inline"></a>

### <a name="edit-the-task-inline"></a>Edición de la tarea en línea

1. En [Azure Portal](https://portal.azure.com), busque el recurso que tiene la tarea que quiere actualizar.

1. En el menú de navegación del recurso, en la sección **Automatización**, seleccione **Tareas (versión preliminar)** .

1. En la lista de tareas, busque la tarea que quiere actualizar. Abra el menú de puntos suspensivos ( **…** ) de la tarea y seleccione **Edit in-line** (Editar en línea).

   ![Captura de pantalla que muestra el menú de puntos suspensivos abierto y la opción seleccionada, "Editar en línea".](./media/create-automation-tasks-azure-resources/edit-task-in-line.png)

   De manera predeterminada, aparece la pestaña **Autenticar** y muestra las conexiones existentes.

1. Para agregar credenciales de autenticación nuevas o seleccionar otras existentes para una conexión, abra el menú de puntos suspensivos ( **…** ) de la conexión y seleccione **Agregar nueva conexión** o bien, si es posible, otras credenciales de autenticación.

   ![Captura de pantalla que muestra la pestaña Autenticación, las conexiones existentes y el menú de puntos suspensivos seleccionado.](./media/create-automation-tasks-azure-resources/edit-connections.png)

1. Para actualizar otras propiedades de la tarea, seleccione **Siguiente: Configurar**.

   En la tarea de este ejemplo, la única propiedad disponible para editar es la dirección de correo electrónico.

   ![Captura de pantalla que muestra la pestaña "Configurar".](./media/create-automation-tasks-azure-resources/edit-task-configuration.png)

1. Cuando finalice, seleccione **Guardar**.

<a name="edit-task-workflow"></a>

### <a name="edit-the-tasks-underlying-workflow"></a>Edición del flujo de trabajo subyacente de una tarea

Al cambiar el flujo de trabajo subyacente de una tarea de automatización, los cambios solo afectan la instancia de la tarea que ha creado y no la plantilla que crea la tarea. Después de hacer los cambios y guardarlos, es posible que el nombre que proporcionó para la tarea original no describa exactamente la tarea, por lo que quizás tenga que volver a crear la tarea con otro nombre.

> [!TIP]
> Como procedimiento recomendado, clone el flujo de trabajo subyacente para que pueda editar en su lugar la versión copiada. De este modo, puede hacer los cambios y probarlos en la copia mientras la tarea de automatización original sigue trabajando y ejecutándose sin riesgo de interrupción de la funcionalidad existente. Cuando termine de hacer los cambios y esté seguro de que la versión nueva se ejecuta correctamente, podrá deshabilitar o eliminar la tarea de automatización original y usar la versión clonada. En los pasos siguientes se incluye información sobre cómo clonar el flujo de trabajo.

1. En [Azure Portal](https://portal.azure.com), busque el recurso que tiene la tarea que quiere actualizar.

1. En el menú de navegación del recurso, en la sección **Automatización**, seleccione **Tareas**.

1. En la lista de tareas, busque la tarea que quiere actualizar. Abra el menú de puntos suspensivos ( **…** ) de la tarea y seleccione **Open in Logic Apps** (Abrir en Logic Apps).

   ![Captura de pantalla que muestra el menú de puntos suspensivos abierto y la opción seleccionada, "Abrir en Logic Apps".](./media/create-automation-tasks-azure-resources/edit-task-logic-app-designer.png)

   El flujo de trabajo subyacente de la tarea se abre en el servicio Azure Logic Apps y se muestra el panel **Información general** en el que puede ver el mismo historial de ejecuciones disponible para la tarea.

   ![Captura de pantalla que muestra la tarea en la vista de Azure Logic Apps con el panel Información general seleccionado.](./media/create-automation-tasks-azure-resources/task-logic-apps-view.png)

1. Para abrir el flujo de trabajo subyacente en el diseñador, seleccione **Diseñador de aplicación lógica** en el menú de la aplicación lógica.

   ![Captura de pantalla que muestra la opción de menú "Diseñador de aplicación lógica" seleccionada y la superficie del diseñador con el flujo de trabajo subyacente.](./media/create-automation-tasks-azure-resources/view-task-workflow-logic-app-designer.png)

   Ahora puede editar las propiedades de las acciones y el desencadenador del flujo de trabajo, así como editar el desencadenador y las acciones que definen el flujo de trabajo en sí. Sin embargo, como procedimiento recomendado, siga los pasos para clonar el flujo de trabajo de manera que pueda hacer los cambios en una copia mientras el flujo de trabajo original sigue funcionando y en ejecución.

1. Para clonar el flujo de trabajo y editar la versión copiada, siga estos pasos:

   1. En el menú del flujo de trabajo de la aplicación lógica, seleccione **Información general**.

   1. En la barra de herramientas del panel de información general, seleccione **Clonar**.

   1. En el panel de creación de la aplicación lógica, en **Nombre**, escriba un nombre nuevo para el flujo de trabajo de la aplicación lógica que copió.

      A excepción de **Estado de aplicación lógica**, no es posible editar las demás propiedades.

   1. En **Estado de aplicación lógica**, seleccione **Deshabilitado** para que el flujo de trabajo clonado no se ejecute mientras hace los cambios. Puede habilitar el flujo de trabajo cuando esté listo para probar los cambios.

   1. Una vez que Azure termine de aprovisionar el flujo de trabajo clonado, búsquelo y ábralo en el diseñador.

1. Para ver las propiedades del desencadenador o una acción, expanda ese desencadenador o esa acción.

   Por ejemplo, puede cambiar el desencadenador de periodicidad para que se ejecute semanalmente, en lugar de una vez al mes.

   ![Captura de pantalla que muestra el desencadenador de periodicidad expandido con la lista de frecuencias abierta para mostrar las opciones de frecuencia disponibles.](./media/create-automation-tasks-azure-resources/edit-recurrence-trigger.png)

   Para más información sobre el desencadenador de periodicidad, consulte [Creación, programación y ejecución de tareas y flujos de trabajo periódicos con el desencadenador de periodicidad](../connectors/connectors-native-recurrence.md). Para más información sobre otros desencadenadores y acciones que puede usar, consulte [Conectores para Azure Logic Apps](../connectors/apis-list.md).

1. Para guardar los cambios, en la barra de herramientas del diseñador, seleccione **Guardar**.

   ![Captura de pantalla que muestra la barra de herramientas del diseñador y el comando "Guardar" seleccionado.](./media/create-automation-tasks-azure-resources/save-updated-workflow.png)

1. Para probar y ejecutar el flujo de trabajo actualizado, en la barra de herramientas del diseñador, seleccione **Ejecutar**.

   Al finalizar la ejecución, el diseñador muestra los detalles de ejecución del flujo de trabajo.

   ![Captura de pantalla que muestra los detalles de ejecución del flujo de trabajo en el diseñador.](./media/create-automation-tasks-azure-resources/view-run-details-designer.png)

1. Para deshabilitar el flujo de trabajo para que la tarea no se siga ejecutando, consulte [Administración de aplicaciones lógicas en Azure Portal](../logic-apps/manage-logic-apps-with-azure-portal.md).

## <a name="provide-feedback"></a>Proporciona comentarios

Esperamos sus comentarios. Para informar errores, proporcionar comentarios o si tiene preguntas sobre esta funcionalidad en versión preliminar, [póngase en contacto con el equipo de Azure Logic Apps](mailto:logicappspm@microsoft.com).

## <a name="next-steps"></a>Pasos siguientes

* [Administración de aplicaciones lógicas en Azure Portal](manage-logic-apps-with-azure-portal.md)
