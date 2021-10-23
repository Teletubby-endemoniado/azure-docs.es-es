---
title: Cohortes de uso de Application Insights | Microsoft Docs
description: Análisis de conjuntos diferentes o usuarios, sesiones, eventos u operaciones que tienen algo en común
ms.topic: conceptual
ms.date: 07/30/2021
ms.openlocfilehash: ba51e407a7e9d72926c4ed7c480e0c1ba71225e1
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2021
ms.locfileid: "130129517"
---
# <a name="application-insights-cohorts"></a>Cohortes de Application Insights

Una cohorte es un conjuntos de usuarios, sesiones, eventos u operaciones que tienen algo en común. En Application Insights, las cohortes se definen mediante una consulta de análisis. En los casos en que tiene que analizar un conjunto específico de usuarios o eventos de forma repetida, las cohortes pueden proporcionarle una mayor flexibilidad para expresar exactamente el conjunto que le interesa.

## <a name="cohorts-versus-basic-filters"></a>Cohortes frente a filtros básicos

Las cohortes se usan de manera similar a los filtros. Pero las definiciones de las cohortes se crean a partir de consultas de análisis personalizadas, por lo que son mucho más adaptables y complejas. A diferencia de los filtros, puede guardar las cohortes para que otros miembros del equipo puedan volver a usarlas.

Puede definir una cohorte de usuarios en la que todos hayan probado una nueva característica de una aplicación. Puede guardar esta cohorte en un recurso de Application Insights. En el futuro, resultará sencillo analizar este grupo de usuarios específicos guardado.

> [!NOTE]
> Una vez creadas, las cohortes están disponibles en las herramientas Usuarios, Sesiones, Eventos y Flujos de usuario.

## <a name="example-engaged-users"></a>Ejemplo: usuarios dedicados

El equipo define como usuario dedicado a cualquier persona que use la aplicación cinco o más veces en un mes determinado. En esta sección, definirá una cohorte de estos usuarios dedicados.

1. Seleccione **Crear una cohorte**.

2. Seleccione la pestaña **Galería de plantillas**. Verá una colección de plantillas para varias cohortes.

3. Seleccione **Engaged Users – by Days Used** (Usuarios dedicados: por días de uso).

    Hay tres parámetros para esta cohorte:
    * **Actividades**, donde elige qué eventos y vistas de página contar como "uso".
    * **Período**, la definición de un mes.
    * **UsedAtleastCustom**, el número de veces que los usuarios necesitan usar algo de un período para contar como usuario dedicado.

4. Cambie **UsedAtleastCustom** a **5+ días** y deje **Período** con el valor predeterminado de 28 días.

  
    Esta cohorte representa todos los identificadores de usuarios que se enviaron con cualquier vista de página o evento personalizado en 5 días distintos en los últimos 28 días.

5. Seleccione **Guardar**.

   > [!TIP]
   > Asigne un nombre a la cohorte, como "Usuarios dedicados (5 + días)". Guárdela en "Mis informes" o "Informes compartidos" en función de si desea que otras personas con acceso a este recurso de Application Insights vean esta cohorte.

6. Seleccione **Back to Gallery** (Volver a la galería).

### <a name="what-can-you-do-by-using-this-cohort"></a>¿Qué puede hacer con esta cohorte?

Abra la herramienta Users (Usuarios). En la lista desplegable **Show** (Mostrar), elija la cohorte que creó en **Users who belong to** (Usuarios que pertenecen a).


:::image type="content" source="./media/usage-cohorts/cohort-2.png" alt-text="Captura de pantalla de la lista desplegable Mostrar, en la que se muestra una cohorte.":::

Hay varios aspectos importantes que se deben tener en cuenta:

* No puede crear este conjunto a través de los filtros normales. La lógica de fecha es más avanzada.
* Puede filtrar aún más esta cohorte con los filtros normales de la herramienta de usuarios. De ese modo, si bien la cohorte se define en ventanas de 28 días, aún se puede ajustar el intervalo de tiempo en la herramienta de usuario para que sea 30, 60 o 90 días.

Estos filtros admiten preguntas más sofisticadas que son imposibles de expresas mediante el generador de consultas. Por ejemplo, _las personas que estaban dedicadas en los últimos 28 días. ¿Cómo se han comportado esas mismas personas durante los últimos 60 días?_

## <a name="example-events-cohort"></a>Ejemplo: cohortes de eventos

También puede hacer cohortes de eventos. En esta sección, definirá una cohorte de los eventos y las vistas de página. Luego verá cómo usarlos desde las otras herramientas. Esta cohorte puede definir un conjunto de eventos que su equipo considere de _uso activo_ o un conjunto relacionado con una nueva característica específica.

1. Seleccione **Crear una cohorte**.

2. Seleccione la pestaña **Galería de plantillas**. Verá una colección de plantillas para varias cohortes.

3. Seleccione **Events Picker** (Selector de eventos).

4. En la lista desplegable **Actividades**, seleccione los eventos que quiere incluir en la cohorte.

5. Guarde la cohorte y asígnele un nombre.

## <a name="example-active-users-where-you-modify-a-query"></a>Ejemplo: usuarios activos en donde se modifica una consulta

Las dos cohortes anteriores se han definido mediante listas desplegables. Pero también se pueden definir las cohortes con consultas de análisis para una flexibilidad total. Para ver cómo hacerlo, cree una cohorte de usuarios del Reino Unido.


1. Abra la herramienta Cohortes, seleccione la pestaña **Galería de plantillas** y seleccione **Cohortes de usuarios en blanco**.

   :::image type="content" source="./media/usage-cohorts/cohort.png" alt-text="Captura de pantalla de la galería de plantillas para cohortes." lightbox="./media/usage-cohorts/cohort.png":::

    Hay tres opciones:
   * Una sección de texto Markdown donde se describe la cohorte en más detalle para otros usuarios del equipo.

   * Una sección de parámetros, donde crea sus propios parámetros, como **Actividades** y otras listas desplegables de los dos ejemplos anteriores.

   * Una sección de consulta que se usa para definir la cohorte con una consulta de análisis.

     En la sección de la consulta, [escriba una consulta de análisis](/azure/kusto/query). La consulta selecciona el conjunto específico de filas que describen la cohorte que desee definir. La herramienta Cohortes agrega implícitamente una cláusula "| Resumir por identificador de usuario" a la consulta. Se muestra una vista previa de estos datos debajo de la consulta en una tabla para que se asegure de que la consulta devuelve resultados.

     > [!NOTE]
     > Si no ve la consulta, pruebe a cambiar el tamaño de la sección para hacerla más alta y mostrar la consulta. 

2. Copie y pegue el texto siguiente en el editor de consultas:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Seleccione **Ejecutar consulta**. Si no ve identificadores de usuarios en la tabla, cambie a un país o región donde haya usuarios de la aplicación.

4. Guarde y dele un nombre a la cohorte.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

_He definido una cohorte de usuarios de un determinado país o región. Cuando comparo esta cohorte en la herramienta Usuarios con simplemente establecer un filtro en ese país o región, veo resultados diferentes. ¿Por qué?_

Las cohortes y los filtros son diferentes. Suponga que tiene una cohorte de usuarios del Reino Unido (definida como el ejemplo anterior) y compara sus resultados con configurar el filtro "Country or region = United Kingdom" (País o región = Reino Unido).

* La versión de cohorte muestra todos los eventos de los usuarios que enviaron uno o varios eventos desde el Reino Unido en el intervalo de tiempo actual. Si se divide según el país o región, probablemente vea muchos países y regiones.
* La versión de filtros solo muestra los eventos del Reino Unido. Pero si se divide según el país o región, solo verá el Reino Unido.

## <a name="learn-more"></a>Más información

* [Lenguaje de consulta de Analytics](../logs/log-analytics-tutorial.md?toc=%2fazure%2fazure-monitor%2ftoc.json)
* [Usuarios, sesiones, eventos](usage-segmentation.md)
* [Flujos de usuario](usage-flows.md)
* [Información general del uso](usage-overview.md)
