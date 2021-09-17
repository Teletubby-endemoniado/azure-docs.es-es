---
title: ¿Qué es Azure Machine Learning Studio?
description: Azure Machine Learning Studio es un portal web para las áreas de trabajo de Azure Machine Learning. Studio combina las experiencias de los tipos sin código y código primero para una plataforma de ciencia de datos inclusiva.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: peterclu
ms.author: peterlu
ms.date: 08/23/2021
adobe-target: true
ms.openlocfilehash: f52c27bbcd0f289028f51c6fcb1fa22ae321f8b4
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122695774"
---
# <a name="what-is-azure-machine-learning-studio"></a>¿Qué es Azure Machine Learning Studio?

En este artículo, obtendrá información sobre Azure Machine Learning Studio, el portal web para desarrolladores y científicos de datos de [Azure Machine Learning](overview-what-is-azure-machine-learning.md). Studio combina las experiencias de los tipos sin código y código primero para una plataforma de ciencia de datos inclusiva.

En este artículo, aprenderá lo siguiente:
>[!div class="checklist"]
> - Cómo [crear proyectos de aprendizaje automático](#author-machine-learning-projects) en Studio.
> - Cómo [administrar activos y recursos](#manage-assets-and-resources) en Studio.
> - Las diferencias entre [Azure Machine Learning Studio y Machine Learning Studio (clásico)](#ml-studio-classic-vs-azure-machine-learning-studio).

Se recomienda usar el explorador más actualizado compatible con el sistema operativo. Se admiten los siguientes exploradores:
  * Microsoft Edge (versión más recientes)
  * Safari (versión más reciente, solo Mac)
  * Chrome (versión más reciente)
  * Firefox (versión más reciente)

## <a name="author-machine-learning-projects"></a>Creación de proyectos de aprendizaje automático

Studio ofrece varias experiencias de creación en función del tipo de proyecto y el nivel de experiencia del usuario.

+ **Blocs de notas**

  Escriba y ejecute su propio código en servidores de [Jupyter Notebook](how-to-run-jupyter-notebooks.md) administrados que estén integrados directamente en Studio. 

:::image type="content" source="media/overview-what-is-azure-ml-studio/notebooks.gif" alt-text="Captura de pantalla: escritura y ejecución de código en un cuaderno":::

+ **Diseñador de Azure Machine Learning**

  Use el diseñador para entrenar e implementar modelos de Machine Learning sin necesidad de escribir código. Arrastre y coloque conjuntos de datos y módulos para crear canalizaciones de Machine Learning. Pruebe el [tutorial del diseñador](tutorial-designer-automobile-price-train-score.md).

    ![Ejemplo del diseñador de Azure Machine Learning](media/concept-designer/designer-drag-and-drop.gif)

+ **Interfaz de usuario de Automated Machine Learning**

  Aprenda a crear [experimentos de aprendizaje automático automatizado](tutorial-first-experiment-automated-ml.md) con una interfaz fácil de usar. 

  ![AutoML en el panel de navegación de Estudio de Azure Machine Learning](./media/overview-what-is-azure-ml-studio/azure-machine-learning-automated-ml-ui.jpg)

+ **Etiquetado de datos**

    Use el [etiquetado de datos de Azure Machine Learning](how-to-create-labeling-projects.md) para coordinar eficazmente los proyectos de etiquetado de datos.

## <a name="manage-assets-and-resources"></a>Administración de activos y recursos

Administre los recursos de aprendizaje automático directamente en el explorador. Los recursos se comparten en la misma área de trabajo entre el SDK y Studio para ofrecer una experiencia sin problemas. Use Studio para administrar:

- Modelos
- Conjuntos de datos
- Almacenes de datos
- Recursos de proceso
- Cuaderno
- Experimentos
- Registros de ejecución
- Procesos 
- Puntos de conexión de canalización

Studio puede simplificar el modo en que se administran los recursos del área de trabajo incluso para desarrolladores experimentados.

## <a name="ml-studio-classic-vs-azure-machine-learning-studio"></a>Machine Learning Studio (clásico) en comparación con Azure Machine Learning Studio

[!INCLUDE [ML Studio (classic) retirement](../../includes/machine-learning-studio-classic-deprecation.md)]

**Estudio de Machine Learning (clásico)** se publicó en 2015 y fue nuestro primer generador de modelos de aprendizaje automático que usó el sistema de arrastrar y colocar. **Machine Learning Studio (clásico)** es un servicio independiente que solo ofrece una experiencia visual. Studio (clásico) no interopera con Azure Machine Learning.

**Azure Machine Learning** es un servicio independiente y modernizado que ofrece una plataforma de ciencia de datos completa. Admite las experiencias de los tipos código primero y código bajo.

**Azure Machine Learning Studio** es un portal web *de* Azure Machine Learning que contiene las opciones de los tipos código bajo y sin código para la creación de proyectos y la administración de recursos. 

Si no está familiarizado, elija **Azure Machine Learning**, en lugar de Estudio de Machine Learning (clásico). Como plataforma de ML completa, Azure Machine Learning ofrece:

- Clústeres de proceso escalables para el aprendizaje a gran escala.
- Seguridad y gobernanza para empresas.
- Interoperabilidad con las herramientas de código abierto conocidas.
- MLOps integral.

### <a name="feature-comparison"></a>Comparación de características

[!INCLUDE [aml-compare-classic](../../includes/machine-learning-compare-classic-aml.md)]

## <a name="troubleshooting"></a>Solución de problemas

* **Faltan elementos de la interfaz de usuario en Studio** El control de acceso basado en rol de Azure se puede usar para limitar las acciones que se pueden realizar con Azure Machine Learning. Estas restricciones pueden impedir que aparezcan elementos de la interfaz de usuario en Estudio de Azure Machine Learning. Por ejemplo, si se le asigna un rol que no puede crear una instancia de proceso, la opción para esta tarea no aparecerá en Studio. Para más información, consulte [Administración de usuarios y roles](how-to-assign-roles.md).

## <a name="next-steps"></a>Pasos siguientes

Visite [Studio](https://ml.azure.com) o explore las diferentes opciones de creación con estos tutoriales:  

Empiece por la [Guía de inicio rápido: Introducción a Azure Machine Learning](quickstart-create-resources.md).  Después use estos recursos para crear su primer experimento con el método que prefiera:

  + [Ejecución de un script de Python "Hola mundo" (parte 1 de 3)](tutorial-1st-experiment-hello-world.md)
  + [Uso de un cuaderno de Jupyter para entrenar modelos de clasificación de imágenes](tutorial-train-models-with-aml.md)
  + [Uso del aprendizaje automático automatizado para entrenar e implementar modelos](tutorial-first-experiment-automated-ml.md)  
  + [Uso del diseñador para entrenar e implementar modelos](tutorial-designer-automobile-price-train-score.md)
  + [Uso de Studio en una red virtual protegida](how-to-enable-studio-virtual-network.md)