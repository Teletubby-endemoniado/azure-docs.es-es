---
title: Copia masiva desde archivos a una base de datos
description: Aprenda a usar una plantilla de solución para copiar datos de forma masiva desde Azure Data Lake Storage Gen2 a Azure Synapse Analytics/Azure SQL Database.
titleSuffix: Azure Data Factory & Azure Synapse
author: jianleishen
ms.author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.date: 12/09/2020
ms.openlocfilehash: 59189f0c197294ca74e01d331663c51fa0e2bd7d
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124757902"
---
# <a name="bulk-copy-from-files-to-database"></a>Copia masiva desde archivos a una base de datos

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Este artículo describe una plantilla de solución para copiar datos de forma masiva desde Azure Data Lake Storage Gen2 a Azure Synapse Analytics/Azure SQL Database.

## <a name="about-this-solution-template"></a>Acerca de esta plantilla de solución

Esta plantilla recupera archivos de un origen de Azure Data Lake Storage Gen2. A continuación, recorre en iteración cada archivo de origen y copia el archivo al almacén de datos de destino. 

Actualmente, esta plantilla solo admite la copia de datos en formato **DelimitedText**. Los archivos de otros formatos de datos también se pueden recuperar del almacén de datos de origen, pero no se pueden copiar en el almacén de datos de destino.  

La plantilla contiene tres actividades:
- La actividad **Obtener metadatos** recupera archivos de Azure Data Lake Storage Gen2 y los envía a la actividad *ForEach* posterior.
- La actividad **ForEach** obtiene los archivos de la actividad *Obtener metadatos* e itera cada archivo en la actividad *Copiar*.
- La actividad **Copiar** reside en la actividad *ForEach* para copiar cada archivo del almacén de datos de origen al almacén de datos de destino.

La plantilla define los dos parámetros siguientes:
- *SourceContainer* es la ruta de acceso del contenedor raíz en el que se copian los datos en el Azure Data Lake Storage Gen2. 
- *SourceDirectory* es la ruta de acceso del directorio en el contenedor raíz del que se copian los datos en el Azure Data Lake Storage Gen2.

## <a name="how-to-use-this-solution-template"></a>Uso de esta plantilla de solución

1. Vaya a la plantilla **Copia masiva desde archivos a una base de datos**. Cree una **nueva** conexión al almacén Gen2 de origen. Tenga en cuenta que "GetMetadataDataset" y "SourceDataset" son referencias a la misma conexión del almacén de archivos de origen.

    :::image type="content" source="media/solution-template-bulk-copy-from-files-to-database/source-connection.png" alt-text="Creación de una nueva conexión al almacén de datos de origen":::

2. Cree una conexión **nueva** al almacén de datos receptor al que está copiando datos.

    :::image type="content" source="media/solution-template-bulk-copy-from-files-to-database/destination-connection.png" alt-text="Creación de una nueva conexión al almacén de datos receptor":::
    
3. Seleccione **Usar esta plantilla**.

    :::image type="content" source="media/solution-template-bulk-copy-from-files-to-database/use-template.png" alt-text="Uso de esta plantilla":::
    
4. Verá una canalización creada tal y como se muestra en el ejemplo siguiente:

    :::image type="content" source="media/solution-template-bulk-copy-from-files-to-database/new-pipeline.png" alt-text="Revisión de la canalización":::

    > [!NOTE]
    > Si eligió **Azure Synapse Analytics** como destino de datos en el **paso 2** descrito anteriormente, debe especificar una conexión a una instancia de Azure Blob Storage como almacenamiento provisional, porque así lo requiere Polybase de Azure Synapse Analytics. Como se muestra en la siguiente captura de pantalla, la plantilla generará automáticamente una *ruta de acceso de almacenamiento* para Blob Storage. Consulte si el contenedor se ha creado después de la ejecución de la canalización.
        
    :::image type="content" source="media/solution-template-bulk-copy-from-files-to-database/staging-account.png" alt-text="Configuración de PolyBase":::

5. Seleccione **Depurar**, escriba los **parámetros** y, a continuación, seleccione **Finalizar**.

    :::image type="content" source="media/solution-template-bulk-copy-from-files-to-database/debug-run.png" alt-text="Clic en **Depurar**":::

6. Cuando la ejecución de la canalización se complete correctamente, verá resultados similares al ejemplo siguiente:

    :::image type="content" source="media/solution-template-bulk-copy-from-files-to-database/run-succeeded.png" alt-text="Revisión del resultado":::

       
## <a name="next-steps"></a>Pasos siguientes

- [Introducción al servicio Azure Data Factory](introduction.md)