---
title: Importación de datos en el diseñador
titleSuffix: Azure Machine Learning
description: Aprenda a importar datos en el diseñador de Azure Machine Learning por medio de conjuntos de datos de Azure Machine Learning y el componente Importar datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: mldata
author: likebupt
ms.author: keli19
ms.date: 10/21/2021
ms.topic: how-to
ms.custom: designer
ms.openlocfilehash: d08d691b7ef990ac351984798efd1562171a023a
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131553425"
---
# <a name="import-data-into-azure-machine-learning-designer"></a>Importación de datos en el diseñador de Azure Machine Learning

En este artículo, aprenderá a importar sus propios datos en el diseñador para crear soluciones personalizadas. Hay dos formas de importar datos en el diseñador: 

* **Conjuntos de datos de Azure Machine Learning**: registre [conjuntos de datos](concept-data.md#datasets) en Azure Machine Learning para habilitar características avanzadas que le ayuden a administrar sus datos.
* **Componente Importar datos**: use el componente [Importar datos](algorithm-module-reference/import-data.md) para acceder directamente a datos de orígenes de datos en línea.

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="use-azure-machine-learning-datasets"></a>Uso de conjuntos de datos de Azure Machine Learning

Se recomienda usar [conjuntos de datos](concept-data.md#datasets) para importar datos en el diseñador. Al registrar un conjunto de datos, puede aprovechar al máximo las características de datos avanzadas como el [control de versiones y el seguimiento](how-to-version-track-datasets.md), y la [supervisión de datos](how-to-monitor-datasets.md).

### <a name="register-a-dataset"></a>Registro de un conjunto de datos

Puede registrar los conjuntos de datos existentes [mediante programación con el SDK](how-to-create-register-datasets.md#datasets-sdk) o [visualmente en Azure Machine Learning Studio](how-to-connect-data-ui.md#create-datasets).

También puede registrar la salida de cualquier componente del diseñador como un conjunto de datos.

1. Seleccione el componente que genera los datos que quiere registrar.

1. En el panel de propiedades, seleccione **Outputs + logs** (Salidas y registros) > **Register dataset** (Registrar conjunto de datos).

    ![Captura de pantalla que muestra cómo ir a la opción Register Dataset (Registrar conjunto de datos)](media/how-to-designer-import-data/register-dataset-designer.png)

Si los datos de salida del componente están en formato de tabla, debe optar por registrar la salida como un **conjunto de datos de archivo** o un **conjunto de datos tabular**.

 - El **conjunto de datos de archivo** registra la carpeta de salida del componente como un conjunto de datos de archivo. La carpeta de salida contiene un archivo de datos y metarchivos que el diseñador usa internamente. Seleccione esta opción si quiere seguir usando el conjunto de los conjuntos registrado en el diseñador. 

 - El **conjunto de datos tabular** solo registra el archivo de datos de salida del componente como un conjunto de datos tabular. Este formato se usa fácilmente en otras herramientas, como las de aprendizaje automático automatizado o el SDK de Python. Seleccione esta opción si tiene previsto utilizar el conjunto de datos registrado fuera del diseñador.  
 

### <a name="use-a-dataset"></a>Uso de un conjunto de datos

Los conjuntos de datos registrados se pueden encontrar en la paleta del componente, en **Conjuntos de datos**. Para usar un conjunto de datos, arrástrelo y suéltelo en el lienzo de la canalización. Luego conecte el puerto de salida del conjunto de datos a otros componentes del lienzo. 

Si registra un conjunto de datos de archivo, el tipo de puerto de salida del conjunto de datos es **AnyDirectory**. Si registra un conjunto de datos tabulares, el tipo de puerto de salida del conjunto de datos es **DataFrameDirectory**. Tenga en cuenta que si conecta el puerto de salida del conjunto de datos a otros componentes del diseñador, debe alinear el tipo de puerto de los conjuntos de datos y los componentes.

![Captura de pantalla que muestra la ubicación de los conjuntos de datos guardados en la paleta del diseñador](media/how-to-designer-import-data/use-datasets-designer.png)


> [!NOTE]
> El diseñador admite el [control de versiones del conjunto de datos](how-to-version-track-datasets.md). Especifique la versión del conjunto de datos en el panel de propiedades del componente del conjunto de datos.

### <a name="limitations"></a>Limitaciones 

- Actualmente solo se puede visualizar el conjunto de datos tabular en el diseñador. Si registra un conjunto de datos de archivo fuera del diseñador, no podrá visualizarlo en el lienzo del diseñador.
- Actualmente, el diseñador solo admite salidas en vista previa que se almacenan en **Azure Blob Storage**. Puede comprobar y cambiar el almacén de datos de salida en **Configuración de salida**, en la pestaña **Parámetros** del panel derecho del componente.
- Si los datos se almacenan en una red virtual (VNet) y quiere obtener una vista previa, tendrá que habilitar la identidad administrada del área de trabajo del almacén de datos.
    1. Vaya al almacén de datos relacionado y haga clic en **Actualizar autenticación**
    :::image type="content" source="./media/resource-known-issues/datastore-update-credential.png" alt-text="Actualizar credenciales":::.
    1. Seleccione **Sí** para habilitar la identidad administrada del área de trabajo.
    :::image type="content" source="./media/resource-known-issues/enable-workspace-managed-identity.png" alt-text="Habilitación de la identidad administrada del área de trabajo":::

## <a name="import-data-using-the-import-data-component"></a>Importación de datos mediante el componente Importar datos

Aunque se recomienda usar conjuntos de datos para importar datos, también se puede usar el componente [Importar datos](algorithm-module-reference/import-data.md). El componente Importar datos omite el registro del conjunto de datos en Azure Machine Learning e importa los datos directamente de un [almacén de datos](concept-data.md#datastores) o una dirección URL HTTP.

Para obtener información detallada sobre cómo usar el componente Importar datos, vea la [página de referencia de Importar datos](algorithm-module-reference/import-data.md).

> [!NOTE]
> Si el conjunto de datos tiene demasiadas columnas, puede encontrar el siguiente error: "Validation failed due to size limitation" (Error de validación debido a un límite de tamaño). Para evitar esto, [registre el conjunto de datos en la interfaz de conjuntos de datos](how-to-connect-data-ui.md#create-datasets).

## <a name="supported-sources"></a>Orígenes compatibles

En esta sección se enumeran los orígenes de datos que admite el diseñador. Los datos llegan al diseñador desde un almacén de datos o un [conjunto de datos tabular](how-to-create-register-datasets.md#dataset-types).

### <a name="datastore-sources"></a>Orígenes del almacén de datos
Para ver una lista de los orígenes del almacén de datos compatibles, consulte [Acceso a los datos en los servicios de almacenamiento de Azure](how-to-access-data.md#supported-data-storage-service-types).

### <a name="tabular-dataset-sources"></a>Orígenes de conjuntos de datos tabulares

El diseñador admite los conjuntos de datos tabulares creados en los siguientes orígenes:
 * Archivos delimitados
 * Archivos JSON
 * Archivos de Parquet
 * Consultas SQL

## <a name="data-types"></a>Tipos de datos

Internamente, el diseñador reconoce los siguientes tipos de datos:

* String
* Entero
* Decimal
* Boolean
* Date

El diseñador usa un tipo de datos interno para pasar datos entre componentes. Puede convertir explícitamente los datos en formato de tabla de datos con el componente [Convertir en conjunto de datos](algorithm-module-reference/convert-to-dataset.md). Todos los componentes que aceptan formatos distintos al interno convierten los datos de manera silenciosa antes de pasarlos al componente siguiente.

## <a name="data-constraints"></a>Restricciones de datos

Los módulos del diseñador están limitados por el tamaño del destino de proceso. Para conjuntos de datos mayores, debe usar un recurso de proceso de Azure Machine Learning mayor. Para más información sobre el proceso de Azure Machine Learning, consulte [¿Qué son los destinos de proceso en Azure Machine Learning?](concept-compute-target.md#azure-machine-learning-compute-managed)

## <a name="access-data-in-a-virtual-network"></a>Acceso a los datos de una red virtual

Si el área de trabajo está en una red virtual, debe realizar pasos de configuración adicionales para visualizar los datos en el diseñador. Para más información sobre cómo usar los almacenes de datos y los conjuntos de datos en una red virtual, consulte [Uso de Azure Machine Learning Studio en una instancia de Azure Virtual Network](how-to-enable-studio-virtual-network.md).

## <a name="next-steps"></a>Pasos siguientes

Conozca los aspectos básicos del diseñador con el [Tutorial: Predecir el precio del automóvil con el diseñador](tutorial-designer-automobile-price-train-score.md).
