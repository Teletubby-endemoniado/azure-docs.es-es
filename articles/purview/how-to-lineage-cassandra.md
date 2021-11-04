---
title: Metadatos y linaje de Cassandra
description: En este artículo se describe la extracción del linaje de datos del origen Cassandra.
author: linda33wj
ms.author: jingwang
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 08/12/2021
ms.openlocfilehash: a46649187403c5825f580da167f9fe77dc9ba210
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131453333"
---
# <a name="how-to-get-lineage-from-cassandra-into-azure-purview"></a>Procedimiento para obtener el linaje de Cassandra en Azure Purview

En este artículo se detallan los aspectos del linaje de datos del origen Cassandra en Azure Purview. El requisito previo para ver el linaje de datos en Purview para Cassandra es [examinar su servidor de Cassandra.](../purview/register-scan-cassandra-source.md) 

## <a name="lineage-of-cassandra-artifacts-in-azure-purview"></a>Linaje de artefactos de Cassandra en Azure Purview

Los usuarios pueden buscar artefactos de Cassandra por el nombre, la descripción u otros detalles para ver los resultados pertinentes. En la pestaña de información general y propiedades de los recursos, se muestran los detalles básicos, como la descripción, las propiedades y otra información. En la pestaña del linaje, se muestran las relaciones de recursos entre las tablas de Cassandra y las vistas materializadas. Por lo tanto, hoy en día, las vistas materializadas de Cassandra tendrán información de linaje de las tablas. 

El linaje derivado también está disponible en las columnas.

:::image type="content" source="./media/how-to-lineage-cassandra/lineage.png" alt-text="Captura de pantalla en la que se muestra cómo se representa el linaje para Cassandra." lightbox="./media/how-to-lineage-cassandra/lineage.png":::


## <a name="next-steps"></a>Pasos siguientes

- [Obtener información sobre el linaje de datos en Azure Purview](catalog-lineage-user-guide.md)
- [Vinculación de Azure Data Factory para insertar linaje automatizado](how-to-link-azure-data-factory.md)
