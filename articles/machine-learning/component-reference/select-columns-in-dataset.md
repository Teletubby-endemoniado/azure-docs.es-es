---
title: 'Seleccionar columnas de conjunto de datos: referencia de componente'
titleSuffix: Azure Machine Learning
description: Aprenda a usar el componente Seleccionar columnas de conjunto de datos de Azure Machine Learning para seleccionar un subconjunto de columnas a fin de usarlas en operaciones de nivel inferior.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: 724a40cbe711e5bb06174533aaf477f7c01d6ac9
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131566043"
---
# <a name="select-columns-in-dataset-component"></a>Componente Seleccionar columnas de conjunto de datos

En este artículo se describe un componente del diseñador de Azure Machine Learning.

Use este componente para seleccionar un subconjunto de columnas a fin de usarlas en operaciones de nivel inferior. El componente no elimina físicamente las columnas del conjunto de datos de origen, sino que crea un subconjunto de columnas, de forma muy similar a una *vista* o *proyección* de una base de datos.

Este componente es útil si se necesita limitar las columnas disponibles para una operación de nivel inferior o si se quiere reducir el tamaño del conjunto de datos mediante la eliminación de columnas innecesarias.

Las columnas del conjunto de datos mantienen el mismo orden que en los datos originales, incluso si las especifica en un orden diferente.

## <a name="how-to-use"></a>Cómo se usa

Este componente no tiene parámetros. Utiliza el selector de columnas para elegir las columnas que desea incluir o excluir.

### <a name="choose-columns-by-name"></a>Elegir columnas por nombre

El componente cuenta con varias opciones para seleccionar columnas por nombre: 

+ Filtrar y buscar

    Haga clic en la opción **POR NOMBRE**.

    Si ha conectado un conjunto de datos que ya está propagado, se mostrará una lista de columnas disponibles. Si no aparece ninguna columna, es posible que tenga que ejecutar componentes de nivel superior para ver la lista de columnas.

    Para filtrar la lista, escriba en el cuadro de búsqueda. Por ejemplo, si escribe la letra `w` en el cuadro de búsqueda, la lista se filtrará para mostrar los nombres de columna que contengan la letra `w`.

    Seleccione las columnas y haga clic en el botón de flecha derecha para mover las columnas seleccionadas a la lista del panel derecho.

    + Para seleccionar un intervalo continuo de nombres de columna, presione **Mayús + clic**.
    + Para agregar columnas individuales a la selección, presione **Ctrl + clic**.

    Haga clic en el botón de marca de verificación para guardar y cerrar.

+ Usar nombres en combinación con otras reglas

    Haga clic en la opción **CON REGLAS**.
    
    Elija una regla, por ejemplo, que se muestren las columnas de un tipo de datos específico.

    A continuación, haga clic en las columnas individuales de ese tipo por nombre para agregarlas a la lista de la selección.

+ Escribir o pegar una lista de nombres de columna separados por comas

    Si el conjunto de datos es grande, puede que sea más fácil usar índices o generar listas de nombres en lugar de seleccionar columnas individualmente. Suponiendo que ha preparado la lista de antemano:

    1. Haga clic en la opción **CON REGLAS**. 
    2. Seleccione **Ninguna columna**, seleccione **Incluir** y, a continuación, haga clic en el cuadro de texto con el signo de exclamación rojo. 
    3. Pegue o escriba una lista de nombres de columna validados previamente separados por comas. No se puede guardar el componente si alguna columna tiene un nombre no válido, así que asegúrese de comprobar los nombres de antemano.
    
    También puede usar este método para especificar una lista de columnas utilizando sus valores de índice. 

### <a name="choose-by-type"></a>Elegir por tipo

Si usa la opción **CON REGLAS**, puede aplicar varias condiciones a las selecciones de columnas. Por ejemplo, es posible que necesite obtener solo las columnas de característica de un tipo de datos numérico.

La opción **EMPIEZA POR** determina el punto inicial y es importante para entender los resultados. 

+ Si selecciona la opción **TODAS LAS COLUMNAS**, se agregarán todas las columnas a la lista. A continuación, debe usar la opción **Excluir** para *eliminar* las columnas que cumplan ciertas condiciones. 

    Por ejemplo, puede comenzar con todas las columnas y, a continuación, eliminar columnas por nombre o por tipo.

+ Si selecciona la opción **NINGUNA COLUMNA**, la lista de columnas empieza vacía. A continuación, deberá especificar las condiciones para *agregar* columnas a la lista. 

    Si aplica varias reglas, cada condición es **aditiva**. Por ejemplo, supongamos que comienza sin ninguna columna y, a continuación, agrega una regla para obtener todas las columnas numéricas. En el conjunto de datos de precios de automóviles, que obtiene como resultado 16 columnas. A continuación, deberá hacer clic en el signo **+** para agregar una nueva condición y seleccionar **Incluir todas las características**. El conjunto de datos resultante incluye todas las columnas numéricas, además de todas las columnas de característica, incluidas algunas columnas de característica de cadena.

### <a name="choose-by-column-index"></a>Elegir por índice de columna

El índice de columna hace referencia al orden de la columna dentro del conjunto de datos original.

+ Las columnas se numeran secuencialmente, comenzando en el 1.  
+ Para obtener un intervalo de columnas, use un guion. 
+ No se permiten especificaciones abiertas como `1-` o `-3`.
+ No se permiten valores de índice (o nombres de columna) duplicados, que podrían producir un error.

Por ejemplo, suponiendo que el conjunto de datos tiene al menos ocho columnas, puede pegar cualquiera de los ejemplos siguientes para devolver varias columnas no contiguas: 

+ `8,1-4,6`
+ `1,3-8`
+ `1,3-6,4` 

el último ejemplo no produce un error; sin embargo, devuelve una única instancia de la columna `4`.



### <a name="change-order-of-columns"></a>Cambiar el orden de las columnas

La opción **Permitir duplicados y conservar el orden de las columnas en la selección** comienza con una lista vacía y agrega las columnas que especifica por nombre o por índice. A diferencia de otras opciones, que siempre devuelven las columnas en su "orden natural", esta opción muestra las columnas en el orden en que las nombre o enumere. 

Por ejemplo, en un conjunto de datos con las columnas Col1, Col2, Col3 y Col4, podría invertir el orden de las columnas y omitir la columna 2 especificando las listas siguientes:

+ `Col4, Col3, Col1`
+ `4,3,1`


## <a name="next-steps"></a>Pasos siguientes

Vea el [conjunto de componentes disponibles](component-reference.md) para Azure Machine Learning. 