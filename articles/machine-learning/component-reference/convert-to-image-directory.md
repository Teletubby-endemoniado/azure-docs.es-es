---
title: Conversión al directorio de imagen
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el componente Conversión al directorio de imagen para convertir el conjunto de datos al formato de directorio de imagen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 11/12/2020
ms.openlocfilehash: d6411f0744872daaf4ffaf2a03c2505d32c264e8
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131565947"
---
# <a name="convert-to-image-directory"></a>Conversión al directorio de imagen

En este artículo se describe cómo usar el componente Conversión al directorio de imagen para ayudar a convertir el conjunto de datos de imagen al tipo de datos *Directorio de imagen*, que es el formato de datos estandarizado en tareas relacionadas con imágenes como la clasificación de imágenes en el diseñador de Azure Machine Learning.

## <a name="how-to-use-convert-to-image-directory"></a>Cómo usar Conversión al directorio de imagen  

1. En primer lugar, prepare el conjunto de datos de imágenes. 

    Debe especificar la etiqueta del conjunto de datos de entrenamiento para el aprendizaje supervisado. El archivo de conjunto de archivos de imágenes debe estar en la siguiente estructura:
    
    ```
    Your_image_folder_name/Category_1/xxx.png
    Your_image_folder_name/Category_1/xxy.jpg
    Your_image_folder_name/Category_1/xxz.jpeg
    
    Your_image_folder_name/Category_2/123.png
    Your_image_folder_name/Category_2/nsdf3.png
    Your_image_folder_name/Category_2/asd932_.png
    ```
    
    En la carpeta Image DataSet, hay varias subcarpetas. Cada subcarpeta contiene imágenes de una categoría, respectivamente. Los nombres de las subcarpetas se consideran etiquetas para tareas como la clasificación de imágenes. Consulte el artículo sobre los [conjuntos de datos de Torchvision](https://pytorch.org/vision/stable/datasets.html#imagefolder) para obtener más información.

    > [!WARNING]
    > En el diseñador no se admiten los conjuntos de datos etiquetados actualmente exportados desde el etiquetado de datos.

    Se admiten las imágenes con estas extensiones (en minúsculas): .jpg, .jpeg, .png, .ppm, .bmp, .pgm, .tif, .tiff y .webp. También puede tener varios tipos de imágenes en una carpeta. No es necesario que se incluya el mismo recuento de imágenes en cada carpeta de categorías.

    Puede usar la carpeta o el archivo comprimido con la extensión .zip, .tar, .gz y .bz2. **Se recomiendan los archivos comprimidos para mejorar el rendimiento.** 
    
    ![Conjunto de datos de ejemplo de imágenes](./media/module/image-sample-dataset.png)

    > [!NOTE]
    > Por inferencia, la carpeta del conjunto de datos de imágenes solo debe contener imágenes sin clasificar.

1. [Registre el conjunto de datos de imágenes como un conjunto de datos de archivos](../how-to-create-register-datasets.md) en el área de trabajo, ya que la entrada del componente Conversión al directorio de imagen debe ser un **conjunto de datos de archivos**.

1. Agregue el conjunto de datos de imágenes registradas al lienzo. Puede encontrar el conjunto de datos registrados en la categoría **Conjuntos de datos** en la lista de componentes a la izquierda del lienzo. Actualmente, el diseñador no admite la visualización del conjunto de datos de imágenes.

    > [!WARNING]
    > **No puede** usar el componente **Importación de datos** para importar el conjunto de datos de imagen, porque el tipo de salida de **Importación de datos** es el directorio de DataFrame, que solo contiene la cadena de la ruta de acceso del archivo.

1. Agregue el componente **Conversión al directorio de imagen** al lienzo. Puede encontrar este componente en la categoría "Computer Vision/Image Data Transformation" de la lista de componentes. Conéctelo al conjunto de datos de imágenes.
    
3.  Envíe la canalización. Este componente puede ejecutarse en una GPU o una CPU.

## <a name="results"></a>Resultados

La salida del componente **Conversión al directorio de imagen** está en formato de **directorio de imagen** y se puede conectar a otros componentes relacionados con imágenes cuyo formato de puerto de entrada también es el del directorio de imágenes.

![Salida de Conversión al directorio de imagen](./media/module/convert-to-image-directory-output.png)

## <a name="technical-notes"></a>Notas técnicas 

###  <a name="expected-inputs"></a>Entradas esperadas  

| Nombre          | Tipo                  | Descripción   |
| ------------- | --------------------- | ------------- |
| Conjunto de datos de entrada | AnyDirectory, ZipFile | Conjunto de datos de entrada |

###  <a name="output"></a>Output  

| Nombre                   | Tipo           | Descripción            |
| ---------------------- | -------------- | ---------------------- |
| Directorio de imagen de salida | ImageDirectory | Directorio de imagen de salida |

## <a name="next-steps"></a>Pasos siguientes

Vea el [conjunto de componentes disponibles](component-reference.md) para Azure Machine Learning.