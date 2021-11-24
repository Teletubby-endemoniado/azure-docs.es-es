---
title: Visualización de los datos mediante Workbooks de Azure Monitor en Microsoft Sentinel | Microsoft Docs
description: Aprenda a visualizar los datos mediante libros en Microsoft Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2021
ms.author: yelevin
ms.custom: ignite-fall-2021
ms.openlocfilehash: 95bbc0f4accb885ce177aa78f37f3a72bd7ebe4a
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132713105"
---
# <a name="use-azure-monitor-workbooks-to-visualize-and-monitor-your-data"></a>Uso de libros de Azure Monitor para visualizar y supervisar los datos

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Después de que haya [conectado los orígenes de datos](quickstart-onboard.md) a Microsoft Sentinel, puede visualizar y supervisar los datos mediante la adopción de Microsoft Sentinel de Workbooks de Azure Monitor, lo que proporciona versatilidad al crear paneles personalizados. Aunque los libros se muestran de manera diferente en Microsoft Sentinel, puede que le resulte útil ver cómo [crear informes interactivos con los libros de Azure Monitor](../azure-monitor/visualize/workbooks-overview.md). Microsoft Azure Sentinel permite crear libros personalizados en los datos y también incluye plantillas de libro integradas que permiten obtener información rápidamente en los datos en cuanto se conecta con un origen de datos.

En este artículo se describe cómo visualizar los datos en Microsoft Sentinel.

> [!div class="checklist"]
> * Uso de libros integrados
> * Creación de libros

## <a name="prerequisites"></a>Prerrequisitos

Debe tener al menos permisos de **lector de libros** o de **colaborador de libros** en el grupo de recursos del área de trabajo de Microsoft Sentinel.

> [!NOTE]
> Los libros que puede ver en Microsoft Sentinel se guardan en el grupo de recursos del área de trabajo de Microsoft Sentinel y se etiquetan según el área de trabajo en el que se crearon.

## <a name="use-built-in-workbooks"></a>Uso de libros integrados

1. Vaya a **Libros** y, a continuación, seleccione **Plantillas** para ver la lista completa de los libros integrados de Microsoft Sentinel. 

    Para ver cuáles son importantes para los tipos de datos que ha conectado, el campo **Tipos de datos requeridos** de cada libro mostrará el tipo de datos junto a una marca de verificación verde si ya ha transmitido los datos correspondientes a Microsoft Sentinel.

    [ ![Ir a Libros.](media/tutorial-monitor-data/access-workbooks.png) ](media/tutorial-monitor-data/access-workbooks.png#lightbox)

1. Seleccione **Ver plantilla** para ver la plantilla rellenada con los datos.

1. Para editar el libro, seleccione **Guardar** y, a continuación, seleccione la ubicación en la que quiere guardar el archivo JSON de la plantilla.

   > [!NOTE]
   > Esta acción crea un recurso de Azure basado en la plantilla correspondiente y guarda el archivo JSON del libro, pero no los datos.


1. Seleccione **Ver libro guardado**. 

    [ ![Ver libros. ](media/tutorial-monitor-data/workbook-graph.png) ](media/tutorial-monitor-data/workbook-graph.png#lightbox)

    Seleccione el botón **Editar** en la barra de herramientas del libro para personalizarlo según sus necesidades. Cuando haya terminado, haga clic en **Guardar** para guardar los cambios.

    Para más información, consulte cómo [Crear informes interactivos con libros de Azure Monitor](../azure-monitor/visualize/workbooks-overview.md).

> [!TIP]
> Para clonar el libro, seleccione **Editar** y, después, **Guardar como**, asegurándose de guardarlo con otro nombre, en la misma suscripción y el mismo grupo de recursos.
> Los libros clonados aparecerán en la pestaña **Mis libros**.
>
## <a name="create-new-workbook"></a>Creación de un libro

1. Vaya a **Libros** y, a continuación, seleccione **Agregar libro** para crear un nuevo libro desde el principio.

    [ ![Nuevo libro.](media/tutorial-monitor-data/create-workbook.png) ](media/tutorial-monitor-data/create-workbook.png#lightbox)

1. Para editar el libro, seleccione **Editar** y, a continuación, agregue texto, consultas y parámetros según sea necesario. Para más información sobre cómo personalizar el libro, consulte cómo [Crear informes interactivos con libros de Azure Monitor](../azure-monitor/visualize/workbooks-overview.md). 

1. Al compilar una consulta, asegúrese de que **Origen de datos** esté establecido en **Registros** y **Tipo de recurso** esté establecido en **Log Analytics**. A continuación, elija las áreas de trabajo apropiadas. 

1. Después de crear el libro, guárdelo. Asegúrese de que lo guarda en el grupo de recursos y la suscripción del área de trabajo de Microsoft Sentinel.

1. Si desea permitir que otros usuarios de su organización utilicen el libro, en **Guardar en** seleccione **Informes compartidos**. Si desea que este libro esté disponible solo para usted, seleccione **Mis informes**.

1. Para alternar entre los libros del área de trabajo, seleccione **Abrir** ![Icono para abrir un libro](./media/tutorial-monitor-data/switch.png). en la barra de herramientas de cualquier libro. La pantalla cambia a una lista de otros libros a los que puede cambiar.

    Seleccione el libro que desee abrir:

    [ ![Cambiar libros.](media/tutorial-monitor-data/switch-workbooks.png) ](media/tutorial-monitor-data/switch-workbooks.png#lightbox)

## <a name="refresh-your-workbook-data"></a>Actualización de los datos del libro

Actualice el libro para que muestre los datos actualizados. En la barra de herramientas, seleccione una de las siguientes opciones:

- :::image type="icon" source="media/whats-new/manual-refresh-button.png" border="false"::: **Actualizar**, para actualizar manualmente los datos del libro.

- :::image type="icon" source="media/whats-new/auto-refresh-workbook.png" border="false"::: **Actualización automática**, para establecer que el libro se actualice automáticamente en un intervalo configurado.

    - Los intervalos de actualización automática admitidos oscilan entre los **cinco minutos** y **un día**.

    - La actualización automática se pausa mientras edita un libro y los intervalos se reinician cada vez que pasa al modo de vista desde el modo de edición.

    - Los intervalos de actualización automática también se reinician si actualiza manualmente los datos.

    > [!TIP]
    > De manera predeterminada, la actualización automática está desactivada. Para optimizar el rendimiento, la actualización automática también se desactiva cada vez que se cierra un libro, y no se ejecuta en segundo plano. Vuelva a activar la actualización automática según sea necesario la próxima vez que abra el libro.
    >

## <a name="print-a-workbook-or-save-as-pdf"></a>Impresión de un libro o almacenamiento como PDF

Para imprimir un libro o guardarlo como un archivo PDF, use el menú de opciones a la derecha del título del libro.

1. Seleccione Opciones > :::image type="icon" source="media/whats-new/print-icon.png" border="false":::**Imprimir contenido**. 
2. En la pantalla de impresión, ajuste la configuración según sea necesario o seleccione **Guardar como PDF** para guardarlo localmente.

Por ejemplo:

[ ![Imprima el libro o guárdelo como PDF.](media/whats-new/print-workbook.png) ](media/whats-new/print-workbook.png#lightbox)

## <a name="how-to-delete-workbooks"></a>Eliminación de libros

Para eliminar un libro guardado (ya sea una plantilla guardada o un libro personalizado), en la página Libros, seleccione el libro guardado que quiere eliminar y elija **Eliminar**. Esto eliminará el libro guardado.

> [!NOTE]
> Esta acción elimina el recurso del libro y los cambios realizados en la plantilla. La plantilla original seguirá estando disponible.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a visualizar los datos en Microsoft Sentinel mediante libros de Azure.

Para aprender a automatizar las respuestas a las amenazas, consulte [Configuración de respuestas automatizadas frente a amenazas en Microsoft Sentinel](tutorial-respond-threats-playbook.md).

Para obtener información sobre los libros integrados más populares, consulte [Libros de Microsoft Sentinel más utilizados](top-workbooks.md). 
