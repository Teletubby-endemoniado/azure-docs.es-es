---
title: 'Conversión a conjunto de datos: referencia del componente'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el componente de conversión a conjunto de datos en el diseñador de Azure Machine Learning para convertir la entrada de datos al formato del conjunto de datos interno.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: e86c7c6f6a5eb3c4b3a388dab4680d2856a80b43
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131565903"
---
# <a name="convert-to-dataset"></a>Convertir conjunto de datos

En este artículo se describe cómo usar el componente Conversión a conjunto de datos en el diseñador Azure Machine Learning para convertir cualquier dato de una canalización al formato interno del diseñador.
  
La conversión no es necesaria en la mayoría de los casos. Azure Machine Learning convierte implícitamente los datos a su formato de conjunto de datos nativo cuando se realiza cualquier operación en los datos. 

Le recomendamos guardar los datos en el formato del conjunto de datos si ha realizado algún tipo de normalización o limpieza en un conjunto de datos y desea asegurarse de que los cambios se usan en otras canalizaciones.  
  
> [!NOTE]
> Convert to Dataset (Convertir conjunto de datos) solo cambia el formato de los datos. No guarda una nueva copia de los datos en el área de trabajo. Para guardar el conjunto de datos, haga doble clic en el puerto de salida, seleccione **Guardar como conjunto de datos** y escriba un nuevo nombre.  
  
## <a name="how-to-use-convert-to-dataset"></a>Cómo usar Convertir conjunto de datos  

Le recomendamos usar el componente [Editar metadatos](edit-metadata.md) para preparar el conjunto de datos antes de usar Conversión a conjunto de datos. Puede agregar o cambiar los nombres de columna, ajustar los tipos de datos y realizar otros cambios que sean necesarios.

1.  Agregue a la canalización el componente Conversión a conjunto de datos. Puede encontrar el componente en la categoría **Transformación de datos** del diseñador. 

2. Conéctelo a cualquier componente que genere un conjunto de datos.   

    Siempre y cuando los datos sean [tabulares](/python/api/azureml-core/azureml.data.tabulardataset), puede convertirlos en un conjunto de datos. Esto incluye los datos cargados con [Importar datos](import-data.md), los datos creados con la [Introducción manual de datos](enter-data-manually.md), o los conjuntos de datos transformados con [Aplicar transformación](apply-transformation.md).

3.  En la lista desplegable **Acción**, indique si desea realizar alguna limpieza en los datos antes de guardar el conjunto de datos:  
  
    - **Ninguna**:  Use los datos tal como están.  
  
    - **SetMissingValue**: Establezca un valor específico en el valor que falta del conjunto de datos. El marcador de posición predeterminado es el carácter de signo de interrogación (?), pero puede usar la opción **Custom missing value** (Falta un valor personalizado) para escribir un valor diferente. Por ejemplo, si escribe **Taxi** para **Custom missing value** (Falta un valor personalizado), todas las instancias de **Taxi** del conjunto de datos se cambiarán al valor que falta.
  
    - **ReplaceValues**: Use esta opción para especificar valor único exacto que se va a reemplazar por cualquier otro valor exacto. Puede reemplazar los valores que faltan o los valores personalizados con la configuración del método **Reemplazar**:

      - **No se encuentra**: Seleccione esta opción para reemplazar los valores que faltan en el conjunto de datos de entrada. En **Nuevo valor**, escriba el valor que reemplace los valores que faltan.
      - **Personalizado**: Elija esta opción para reemplazar los valores personalizados en el conjunto de datos de entrada. En **Valor personalizado**, escriba el valor que desea encontrar. Por ejemplo, si los datos contienen la cadena `obs` usada como marcador de posición para los valores que faltan, tendría que escribir `obs`. En **Nuevo valor**, escriba el nuevo valor con el que va a reemplazar la cadena original.
  
    Tenga en cuenta que la operación **ReplaceValues** solo se aplica a las coincidencias exactas. Por ejemplo, estas cadenas no se verían afectadas: `obs.`,`obsolete`.  
 
  
5.  Envíe la canalización.  

## <a name="results"></a>Results

+  Para guardar el conjunto de datos resultante con otro nombre, seleccione el icono **Registrar conjunto de datos** en la pestaña **Salidas** del panel derecho del módulo.  
  
## <a name="technical-notes"></a>Notas técnicas  

-   Cualquier componente que toma un conjunto de datos como entrada también puede tomar datos en el archivo CSV o el archivo TSV. Antes de que se ejecute cualquier código de componente, las entradas se preprocesan. El preprocesamiento es equivalente a ejecutar el componente Conversión a conjunto de datos en la entrada.  
  
-   No se puede convertir del formato SVMLight a un conjunto de datos.  
  
-   Al especificar una operación de reemplazo personalizada, la operación de búsqueda y reemplazo se aplica a los valores completos. No se permiten coincidencias parciales. Por ejemplo, puede reemplazar un 3 por un-1 o por 33, pero no puede reemplazar un 3 por un número de dos dígitos, como 35.  
  
-   En el caso de las operaciones de reemplazo personalizadas, el reemplazo producirá un error en modo silencioso si usa como reemplazo cualquier carácter que no cumpla el tipo de datos actual de la columna.  

  
## <a name="next-steps"></a>Pasos siguientes

Vea el [conjunto de componentes disponibles](component-reference.md) para Azure Machine Learning.