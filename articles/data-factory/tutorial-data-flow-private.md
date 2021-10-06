---
title: Transformación de datos con un flujo de datos de asignación de red virtual administrado por Azure Data Factory
description: En este tutorial se proporcionan instrucciones detalladas para usar Azure Data Factory con el objetivo de transformar los datos con flujos de datos de asignación.
author: ssabat
ms.author: susabat
ms.reviewer: makromer
ms.service: data-factory
ms.subservice: tutorials
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/04/2021
ms.openlocfilehash: fb7dfc93048d888eb83088ca2a3f458219fdc393
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128617894"
---
# <a name="transform-data-securely-by-using-mapping-data-flow"></a>Transformación de datos de forma segura mediante flujos de datos de asignación

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Si no está familiarizado con Azure Data Factory, consulte [Introducción a Azure Data Factory](./introduction.md).

En este tutorial, se usará la interfaz de usuario de Data Factory (IU) para crear una canalización que copie y transforme los datos *desde un origen de Azure Data Lake Storage Gen2 hasta un receptor de Data Lake Storage Gen2 (los cuales, permiten el acceso solo a las redes seleccionadas)* mediante un flujo de datos de asignación en una [red virtual administrada por Data Factory](managed-virtual-network-private-endpoint.md). Puede expandir el patrón de configuración de este tutorial al transformar los datos mediante el flujo de datos de asignación.

En este tutorial, realizará los siguientes pasos:

> [!div class="checklist"]
>
> * Creación de una factoría de datos.
> * Creación de una canalización con una actividad de flujo de datos.
> * Cree un flujo de datos de asignación con cuatro transformaciones.
> * Realización de la serie de pruebas de la canalización.
> * Supervisión de una actividad de flujo de datos.

## <a name="prerequisites"></a>Requisitos previos

* **Suscripción de Azure**. Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* **Cuenta de Azure Storage**. Se usará Data Lake Storage como almacén de datos de *origen* y *receptor*. Si no tiene una cuenta de almacenamiento, consulte [Crear una cuenta de almacenamiento](../storage/common/storage-account-create.md?tabs=azure-portal) para crear una. *Asegúrese de que la cuenta de almacenamiento solo permita el acceso desde las redes seleccionadas.* 

El archivo que se va a transformar en este tutorial es moviesDB.csv, que se puede encontrar en este [sitio de contenido de GitHub](https://raw.githubusercontent.com/djpmsft/adf-ready-demo/master/moviesDB.csv). Para recuperar el archivo de GitHub, copie el contenido en un editor de texto de su elección para guardarlo localmente como un archivo .csv. Para cargar el archivo en la cuenta de almacenamiento, vea [Carga de blobs con Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md). En los ejemplos se hará referencia a un contenedor llamado **sample-data**.

## <a name="create-a-data-factory"></a>Crear una factoría de datos

En este paso, creará una factoría de datos y abrirá la interfaz de usuario de Data Factory para crear una canalización en la factoría de datos.

1. Abra Microsoft Edge o Google Chrome. Actualmente, solo los exploradores web Microsoft Edge y Google Chrome admiten la interfaz de usuario de Data Factory.
1. En el menú de la izquierda, seleccione **Crear un recurso** > **Analytics** > **Data Factory**.
1. En la página **Nueva factoría de datos**, en **Nombre**, escriba **ADFTutorialDataFactory**.

   El nombre de la factoría de datos tiene que ser *único a nivel global*. Si recibe un mensaje de error sobre el valor de nombre, escriba un nombre diferente para la factoría de datos (por ejemplo, yournameADFTutorialDataFactory). Para conocer las reglas de nomenclatura de los artefactos de Data Factory, consulte [Azure Data Factory: reglas de nomenclatura](naming-rules.md).

1. Seleccione la **suscripción** de Azure en la que quiere crear la factoría de datos.
1. Para **Grupo de recursos**, realice uno de los siguientes pasos:

    * Seleccione en primer lugar **Usar existente** y después un grupo de recursos de la lista desplegable.
    * Seleccione **Crear nuevo** y escriba el nombre de un grupo de recursos. 
         
    Para más información sobre los grupos de recursos, consulte [Uso de grupos de recursos para administrar los recursos de Azure](../azure-resource-manager/management/overview.md). 
1. En **Versión**, seleccione **V2**.
1. En **Ubicación**, seleccione la ubicación de la factoría de datos. En la lista desplegable solo aparecen las ubicaciones que se admiten. Los almacenes de datos (por ejemplo, Azure Storage y Azure SQL Database) y los procesos (por ejemplo, Azure HDInsight) que la factoría de datos usa pueden estar en otras regiones.

1. Seleccione **Crear**.
1. Una vez finalizada la creación, verá el aviso en el centro de notificaciones. Seleccione **Ir al recurso** para ir a la página de **Data Factory**.
1. Haga clic en **Author & Monitor** (Creación y supervisión) para iniciar la interfaz de usuario de Data Factory en una pestaña independiente.

## <a name="create-an-azure-ir-in-data-factory-managed-virtual-network"></a>Creación de una instancia de Azure IR en una red virtual administrada por Data Factory

En este paso se crea una instancia de Azure IR y se habilita una red virtual administrada por Data Factory.

1. En el portal de Data Factory, vaya a **Administrar** y seleccione **Nuevo** para crear una instancia de Azure IR.

   :::image type="content" source="./media/tutorial-copy-data-portal-private/create-new-azure-ir.png" alt-text="Captura de pantalla que muestra la creación de una instancia de Azure IR.":::
1. En la página **Integration runtime setup** (Configuración de Integration Runtime), elija qué entorno de ejecución de integración va a crear según las funcionalidades necesarias. En este tutorial, seleccione **Azure, Self-Hosted** (Azure, autohospedado) y, luego, haga clic en **Continuar**. 
1. Seleccione **Azure** y, luego, haga clic en **Continuar** para crear un entorno de ejecución de integración de Azure.

   :::image type="content" source="./media/tutorial-copy-data-portal-private/azure-ir.png" alt-text="Captura de pantalla que muestra una nueva instancia de Azure IR.":::

1. En **Configuración de red virtual (versión preliminar)** , seleccione **Habilitar**.

   :::image type="content" source="./media/tutorial-copy-data-portal-private/enable-managed-vnet.png" alt-text="Captura de pantalla que muestra la habilitación de una nueva instancia de Azure IR.":::

1. Seleccione **Crear**.

## <a name="create-a-pipeline-with-a-data-flow-activity"></a>Creación de una canalización con una actividad de flujo de datos

En este paso, creará una canalización que contiene una actividad de flujo de datos.

1. En la página principal de Azure Data Factory, seleccione **Orchestrate** (Organizar).

   :::image type="content" source="./media/doc-common-process/get-started-page.png" alt-text="Captura de pantalla que muestra la creación de una canalización.":::

1. En el panel de propiedades de la canalización, escriba **TransformMovies** como nombre de la canalización.
1. En el panel **Actividades**, expanda **Move and Transform** (Mover y transformar). Arrastre la actividad **Flujo de datos** desde el panel y colóquela en el lienzo de la canalización.

1. En el menú emergente **Adding data flow** (Agregar flujo de datos), seleccione **Create new Data Flow** (Crear nuevo flujo de datos) y, luego, **Flujo de datos de asignación**. Cuando haya finalizado, seleccione **Aceptar**.

    :::image type="content" source="media/tutorial-data-flow-private/mapping-dataflow.png" alt-text="Captura de pantalla que muestra el flujo de datos de asignación.":::

1. Asigne al flujo de datos el nombre **TransformMovies** en el panel de propiedades.
1. En la barra superior del lienzo de la canalización, mueva el control deslizante **Depuración de flujo de datos** a la posición de activado. El modo de depuración permite realizar pruebas interactivas de la lógica de transformación en un clúster de Spark activo. Los clústeres de Data Flow tardan de 5 a 7 minutos en prepararse y se recomienda que los usuarios activen primero la depuración si planean realizar el desarrollo de Data Flow. Para más información, consulte [Modo de depuración](concepts-data-flow-debug-mode.md).

    :::image type="content" source="media/tutorial-data-flow-private/dataflow-debug.png" alt-text="Captura de pantalla que muestra el control deslizante Data flow debug (Depuración de flujo de datos).":::

## <a name="build-transformation-logic-in-the-data-flow-canvas"></a>Generación de la lógica de transformación en el lienzo de flujo de datos

Una vez creado el flujo de datos, se le enviará automáticamente al lienzo del flujo de datos. En este paso, creará un flujo de datos que toma el archivo moviesDB.csv en Data Lake Storage y agrega el promedio de clasificación de comedias de 1910 a 2000. Después, volverá a escribir este archivo en Data Lake Storage.

### <a name="add-the-source-transformation"></a>Incorporación de la transformación de origen

En este paso, se configura Data Lake Storage Gen2 como origen.

1. En el lienzo del flujo de datos, seleccione el cuadro **Agregar origen** para agregar un origen.

1. Asigne un nombre al origen **MoviesDB**. Seleccione **Nuevo** para crear un conjunto de datos de origen.

1. Seleccione **Azure Data Lake Storage Gen2** y, luego, **Continuar**.

1. Seleccione **DelimitedText** y, después, **Continuar**.

1. Asigne un nombre al conjunto de datos **MoviesDB**. En la lista desplegable de servicios vinculados, seleccione **Nuevo**.

1. En la pantalla de creación de un servicio vinculado, asigne el nombre **ADLSGen2** al servicio vinculado y especifique el método de autenticación. A continuación, escriba las credenciales de conexión. En este tutorial, se va a usar la **clave de cuenta** para conectarse a nuestra cuenta de almacenamiento. 

1. Asegúrese de habilitar **Creación interactiva**. Puede tardar aproximadamente un minuto en habilitarse.

    :::image type="content" source="./media/tutorial-data-flow-private/interactive-authoring.png" alt-text="Captura de pantalla que muestra la creación interactiva.":::

1. Seleccione **Test connection** (Probar conexión). Se producirá un error, ya que la cuenta de almacenamiento no permite el acceso a ella sin la creación y aprobación de un punto de conexión privado. En el mensaje de error, debería ver un vínculo para crear un punto de conexión privado que puede usar para crear un punto de conexión privado administrado. Una alternativa es ir directamente a la pestaña **Administrar** y seguir las instrucciones de [esta sección](#create-a-managed-private-endpoint) para crear un punto de conexión privado administrado.

1. Mantenga abierto el cuadro de diálogo y, después, vaya a la cuenta de almacenamiento.

1. Siga las instrucciones de [esta sección](#approval-of-a-private-link-in-a-storage-account) para aprobar el vínculo privado.

1. Vuelva al cuadro de diálogo. Seleccione de nuevo **Probar conexión** y elija **Crear** para implementar el servicio vinculado.

1. Cuando vuelva a la pantalla de creación del conjunto de datos, escriba la ubicación del archivo en el campo **Ruta de acceso de archivo**. En este tutorial, el archivo moviesDB.csv se encuentra en el contenedor **sample-data**. Como el archivo tiene encabezados, active la casilla **Primera fila como encabezado**. Seleccione **From Connection/Store** (Desde la conexión o almacén) para importar el esquema de encabezado directamente desde el archivo en el almacenamiento. Cuando haya finalizado, seleccione **Aceptar**.

    :::image type="content" source="media/tutorial-data-flow-private/source-file-path.png" alt-text="Captura de pantalla que muestra la ruta de acceso de origen.":::

1. Si se ha iniciado el clúster de depuración, vaya a la pestaña **Data Preview** (Vista previa de los datos) de la transformación de origen y seleccione **Actualizar** para obtener una instantánea de los datos. Puede usar la vista previa de los datos para comprobar que la transformación está configurada correctamente.

    :::image type="content" source="media/tutorial-data-flow-private/data-preview.png" alt-text="Captura de pantalla que muestra la pestaña Data Preview (Vista previa de los datos).":::

#### <a name="create-a-managed-private-endpoint"></a>Creación de un punto de conexión privado administrado

Si no usó el hipervínculo al probar la conexión anterior, siga la ruta de acceso. Ahora debe crear un punto de conexión privado administrado que conectará al servicio vinculado que creó anteriormente.

1. Vaya a la pestaña **Administrar**.

   > [!NOTE]
   > Es posible que la pestaña **Administrar** no esté disponible para todas las instancias de Data Factory. Si no se muestra, puede tener acceso a los puntos de conexión privados al seleccionar **Autor** > **Conexiones** > **Punto de conexión privado**.

1. Vaya a la sección **Puntos de conexión privados administrados**.
1. Seleccione **+ Nuevo** debajo de **Puntos de conexión privados administrados**.

    :::image type="content" source="./media/tutorial-data-flow-private/new-managed-private-endpoint.png" alt-text="Captura de pantalla que muestra el botón Nuevo de los puntos de conexión privados administrados."::: 

1. Seleccione el icono de **Azure Data Lake Storage Gen2** de la lista y elija **Continuar**.
1. Escriba el nombre de la cuenta de almacenamiento que ha creado.
1. Seleccione **Crear**.
1. Después de unos segundos, debería ver que el vínculo privado creado necesita aprobación.
1. Seleccione el punto de conexión privado que creó. Puede ver un hipervínculo que le llevará a la página para aprobar el punto de conexión privado en el nivel de cuenta de almacenamiento.

    :::image type="content" source="./media/tutorial-data-flow-private/manage-private-endpoint.png" alt-text="Captura de pantalla que muestra el panel Manage private endpoint (Administrar punto de conexión privado)."::: 

#### <a name="approval-of-a-private-link-in-a-storage-account"></a>Aprobación de un vínculo privado en una cuenta de almacenamiento

1. En la cuenta de almacenamiento, vaya a **Conexiones de punto de conexión privado** en la sección **Configuración**.

1. Active la casilla del punto de conexión privado que creó y seleccione **Aprobar**.

    :::image type="content" source="./media/tutorial-data-flow-private/approve-private-endpoint.png" alt-text="Captura de pantalla que muestra el botón Aprobar del punto de conexión privado.":::

1. Agregue una descripción y seleccione **Sí**.
1. Vuelva a la sección **Managed private endpoints** (Puntos de conexión privados administrados) de la pestaña **Administrar** de Data Factory.
1. Al cabo de unos minutos, verá que aparece la aprobación para el punto de conexión privado.

### <a name="add-the-filter-transformation"></a>Incorporación de la transformación filtrar

1. Junto al nodo de origen en el lienzo del flujo de datos, seleccione el icono con el signo más para agregar una nueva transformación. La primera transformación que va a agregar es un **filtro**.

    :::image type="content" source="media/tutorial-data-flow-private/add-filter.png" alt-text="Captura de pantalla que muestra la adición de un nuevo filtro.":::
1. Denomine **FilterYears** a la transformación de filtro. Seleccione el cuadro de expresión junto a **Filtro activado** para abrir el generador de expresiones. Aquí especificará la condición de filtrado.

    :::image type="content" source="media/tutorial-data-flow-private/filter-years.png" alt-text="Captura de pantalla que muestra FilterYears.":::
1. El generador de expresiones de flujo de datos le permite compilar de forma interactiva expresiones para utilizarlas en varias transformaciones. Las expresiones pueden incluir funciones integradas, columnas del esquema de entrada y parámetros definidos por el usuario. Para más información sobre cómo compilar expresiones, consulte [Generador de expresiones de Data Flow](./concepts-data-flow-expression-builder.md).

    * En este tutorial, quiere filtrar las películas del género comedia que se estrenaron entre los años 1910 y 2000. Dado que el año es actualmente una cadena, debe convertirlo en un entero mediante la función `toInteger()`. Use los operadores mayor o igual que (> =) y menor o igual que (< =) para realizar una comparación con los valores de año literales 1910 y 2000. Una estas expresiones junto con el operador and (&&). La expresión aparece como:

        `toInteger(year) >= 1910 && toInteger(year) <= 2000`

    * Para averiguar qué películas son comedias, puede usar la función `rlike()` para buscar el patrón "Comedy" en la columna de géneros. Una la expresión `rlike` con la comparación de año para obtener:

        `toInteger(year) >= 1910 && toInteger(year) <= 2000 && rlike(genres, 'Comedy')`

    * Si tiene un clúster de depuración activo, puede comprobar la lógica; para ello, seleccione **Actualizar** para ver la salida de la expresión en comparación con las entradas usadas. Hay más de una respuesta correcta sobre cómo puede realizar esta lógica mediante el lenguaje de expresiones de flujo de datos.

        :::image type="content" source="media/tutorial-data-flow-private/filter-expression.png" alt-text="Captura de pantalla que muestra la expresión de filtro.":::

    * Cuando haya terminado con la expresión, seleccione **Guardar y finalizar**.

1. Capture una **Vista previa de datos** para comprobar que el filtro funciona correctamente.

    :::image type="content" source="media/tutorial-data-flow-private/filter-data.png" alt-text="Captura de pantalla que muestra la vista previa de los datos filtrados.":::

### <a name="add-the-aggregate-transformation"></a>Incorporación de la transformación de agregado

1. La transformación siguiente que se va a agregar es una transformación de **agregado** en **Schema Modifier** (Modificador de esquema).

    :::image type="content" source="media/tutorial-data-flow-private/add-aggregate.png" alt-text="Captura de pantalla que muestra cómo agregar el agregado.":::
1. Denomine **AggregateComedyRatings** a la transformación de agregado. En la pestaña **Agrupar por**, seleccione **año** en el cuadro de lista desplegable para agrupar las agregaciones por el año en que apareció la película.

    :::image type="content" source="media/tutorial-data-flow-private/group-by-year.png" alt-text="Captura de pantalla que muestra el grupo agregado.":::
1. Vaya a la pestaña **Agregados**. En el cuadro de texto de la izquierda, asigne a la columna agregada el nombre **AverageComedyRating**. Seleccione el cuadro de expresión derecho para especificar la expresión de agregado a través del generador de expresiones.

    :::image type="content" source="media/tutorial-data-flow-private/name-column.png" alt-text="Captura de pantalla que muestra el nombre de la columna del agregado.":::
1. Para obtener el promedio de la columna **Rating** (Clasificación), use la función de agregado ```avg()```. Como **Clasificación** es una cadena y ```avg()``` toma una entrada numérica, debemos convertir el valor en un número por medio de la función ```toInteger()```. Se trata de una expresión similar a la siguiente:

    `avg(toInteger(Rating))`

1. Cuando haya terminado, seleccione **Guardar y finalizar**.

    :::image type="content" source="media/tutorial-data-flow-private/save-aggregate.png" alt-text="Captura de pantalla que muestra cómo guardar el agregado.":::
1. Vaya a la pestaña **Vista previa de datos** para ver la salida de la transformación. Observe que solo hay dos columnas, **year** y **AverageComedyRating**.

### <a name="add-the-sink-transformation"></a>Incorporación de la transformación de receptor

1. A continuación, desea agregar una transformación de **receptor** en **Destino**.

    :::image type="content" source="media/tutorial-data-flow-private/add-sink.png" alt-text="Captura de pantalla que muestra la adición de un receptor.":::
1. Asigne un nombre al receptor **Sink** (Receptor). Seleccione **Nuevo** para crear el conjunto de datos del receptor.

    :::image type="content" source="media/tutorial-data-flow-private/create-sink.png" alt-text="Captura de pantalla que muestra la creación de un receptor.":::
1. En la página **Nuevo conjunto de datos**, seleccione **Azure Data Lake Storage Gen2** y, luego, **Continuar**.

1. En la página **Seleccionar formato**, elija **DelimitedText** y, luego, **Continuar**.

1. Asigne el nombre **MoviesSink** al conjunto de datos de receptor. Para el servicio vinculado, elija **ADLSGen2**, que es el mismo que se ha creado para la transformación del origen. Escriba una carpeta de salida en la que escribir los datos. En este tutorial, vamos a escribir en la carpeta **output** del contenedor **sample-data**. No es necesario que la carpeta exista de antemano y se puede crear dinámicamente. Active la casilla **Primera fila como encabezado** y seleccione **Ninguno** en **Import schema** (Importar esquema). Seleccione **Aceptar**.

    :::image type="content" source="media/tutorial-data-flow-private/sink-file-path.png" alt-text="Captura de pantalla que muestra la ruta de acceso del receptor.":::

Ahora ha terminado de crear el flujo de datos. Está preparado para ejecutarlo en la canalización.

## <a name="run-and-monitor-the-data-flow"></a>Ejecución y supervisión del flujo de datos

Puede depurar una canalización antes de publicarla. En este paso, va a desencadenar una ejecución de depuración de la canalización de flujo de datos. Aunque en la vista previa de los datos no se escriben datos, una ejecución de depuración los escribirá en el destino del receptor.

1. Vaya al lienzo de la canalización. Seleccione **Depurar** para desencadenar una ejecución de depuración.

1. La depuración de la canalización de actividades de flujo de datos usa el clúster de depuración activo, pero sigue tardando al menos un minuto en inicializarse. Puede realizar un seguimiento del progreso a través de la pestaña **Salida**. Después de que la ejecución se realice correctamente, seleccione el icono de anteojos para ver los detalles.

1. En el panel Detalles, puede ver el número de filas y el tiempo invertido en cada paso de transformación.

    :::image type="content" source="media/tutorial-data-flow-private/run-details.png" alt-text="Captura de pantalla que muestra una ejecución de supervisión.":::
1. Seleccione una transformación para obtener información detallada sobre las columnas y las particiones de los datos.

Si siguió correctamente este tutorial, debe haber escrito 83 filas y 2 columnas en la carpeta del receptor. Puede comprobar el almacenamiento de blobs para confirmar que los datos son correctos.

## <a name="summary"></a>Resumen

En este tutorial, ha usado la interfaz de usuario de Data Factory para crear una canalización que copia y transforma los datos desde un origen de Data Lake Storage Gen2 hasta un receptor de Data Lake Storage Gen2 (los cuales, permiten el acceso solo a las redes seleccionadas) mediante un flujo de datos de asignación en una [red virtual administrada por Data Factory](managed-virtual-network-private-endpoint.md).
