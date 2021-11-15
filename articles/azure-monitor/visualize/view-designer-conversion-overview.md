---
title: Guía de transición del diseñador de vistas de Azure Monitor en Workbooks
description: Transición de vistas a libros en Azure Monitor.
author: shijatsu
ms.author: shijain
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: 1c371a155f36574f7a443506c0b9090b6b3bd544
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131447082"
---
# <a name="azure-monitor-view-designer-to-workbooks-transition-guide"></a>Guía de transición del diseñador de vistas de Azure Monitor en Workbooks
El [diseñador de vistas](view-designer.md) es una característica de Azure Monitor que permite crear vistas personalizadas para ayudar a visualizar datos en el área de trabajo de Log Analytics, con gráficos, listas y escalas de tiempo. Se ha realizado su transición a Libros para proporcionar un lienzo flexible para el análisis de datos y la creación de informes visuales completos en Azure Portal. Este artículo le ayuda a realizar la transición desde el diseñador de vistas a los libros. 


## <a name="workbooks-overview"></a>Información general sobre Workbooks
Los [libros](../vm/vminsights-workbooks.md) combinan texto,  [consultas de registros](/azure/data-explorer/kusto/query/), métricas y parámetros en informes interactivos avanzados. Los miembros del equipo con el mismo acceso a los recursos de Azure también pueden editar los libros.

Los libros son útiles en escenarios como los siguientes:

-   Explorar la utilización de la máquina virtual cuando no se conocen de antemano las métricas de interés: utilización de CPU, espacio en disco, memoria, dependencias de red, etc. A diferencia de otras herramientas de análisis de uso, los libros le permiten combinar varios tipos de visualizaciones y análisis, lo que los hace idóneos para este tipo de exploración de forma libre.
-   Explicar a su equipo cómo funciona una máquina virtual aprovisionada recientemente, mostrando las métricas para contadores clave y otros eventos de registro.
-   Compartir los resultados de un experimento de cambio de tamaño de la máquina virtual con otros miembros del equipo. Puede explicar los objetivos del experimento con texto y luego mostrar cada métrica de uso y consulta de análisis empleada para evaluar el experimento, junto con indicadores claros sobre si cada métrica está por encima o por debajo del objetivo.
-   Notificar el impacto de una interrupción del servicio en la utilización de la máquina virtual, con la combinación de datos, explicación del texto y análisis de los pasos siguientes para evitar más interrupciones en el futuro.


## <a name="why-convert-view-designer-dashboards-to-workbooks"></a>¿Por qué convertir los paneles del diseñador de vistas en libros?

El diseñador de vistas ofrece la posibilidad de generar diferentes vistas y visualizaciones basadas en consultas. Sin embargo, las personalizaciones generales son limitadas, como el formato de las cuadrículas y los diseños de los iconos, o la posibilidad de seleccionar gráficos alternativos para representar los datos. El diseñador de vistas está restringido a un total de nueve iconos distintos para representar los datos.

Workbooks es una plataforma que permite aprovechar todo el potencial que los datos ofrecen. Los libros no solo conservan todas las capacidades, sino que también admiten funcionalidad adicional mediante texto, métricas, parámetros, etc. Por ejemplo, los libros permiten a los usuarios consolidar cuadrículas densas y agregar barras de búsqueda para filtrar y analizar los datos con facilidad. 

### <a name="advantages-of-using-workbooks-over-view-designer"></a>Ventajas del uso de Workbooks con respecto al diseñador de vistas

* Admite registros y métricas.
* Permite vistas personales para el control de acceso individual y vistas de libros compartidas.
* Opciones de diseño personalizadas con pestañas y controles de tamaño y escalado.
* Compatibilidad con consultas en varias áreas de trabajo de Log Analytics, aplicaciones de Application Insights y suscripciones.
* Habilita parámetros personalizados que actualizan dinámicamente los gráficos y las visualizaciones asociados.
* Compatibilidad con la galería de plantillas desde GitHub.

Aunque esta guía ofrece pasos sencillos para recrear directamente varias de las vistas del diseñador de vistas usadas habitualmente, los libros otorgan a los usuarios la libertad de crear y diseñar cualquiera de sus propias visualizaciones y métricas personalizadas. La captura de pantalla siguiente procede de la [plantilla de uso del área de trabajo](https://go.microsoft.com/fwlink/?linkid=874159&resourceId=Azure%20Monitor&featureName=Workbooks&itemId=community-Workbooks%2FAzure%20Monitor%20-%20Workspaces%2FWorkspace%20Usage&workbookTemplateName=Workspace%20Usage&func=NavigateToPortalFeature&type=workbook) y muestra un ejemplo de lo que los libros son capaces de crear:


![Ejemplo de aplicación Workbooks](media/view-designer-conversion-overview/workbook-template-example.jpg)


## <a name="how-to-start-using-workbooks"></a>Cómo empezar a usar Workbooks
Abra libros desde el icono Libros en el área de trabajo de Log Analytics.

![Navegación en Workbooks](media/view-designer-conversion-overview/workbooks-nav.png)

Una vez seleccionado, se muestra una galería con todos los libros y las plantillas guardados para el área de trabajo.

![Galería de Workbooks](media/view-designer-conversion-overview/workbooks-gallery.png)

Para iniciar un nuevo libro, puede seleccionar la plantilla **Vacío** en **Inicio rápido** o el icono **Nuevo** de la barra de navegación superior. Para ver las plantillas o volver a los libros guardados, seleccione el elemento en la galería o busque por nombre en la barra de búsqueda.

Para guardar un libro, tiene que guardar el informe con un título, una suscripción, un grupo de recursos y una ubicación específicos.
El libro se rellena con la misma configuración que el área de trabajo de LA, con la misma suscripción y grupo de recursos, aunque los usuarios pueden cambiar esta configuración de informe. Los libros son recursos compartidos que requieren acceso de escritura al grupo de recursos primario que se va a guardar.

## <a name="next-steps"></a>Pasos siguientes

- [Opciones de conversión](view-designer-conversion-options.md)
