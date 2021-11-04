---
title: Metadatos y linaje de BigQuery
description: En este artículo se describe la extracción del linaje de datos del origen de BigQuery.
author: linda33wj
ms.author: jingwang
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 08/12/2021
ms.openlocfilehash: 7c576c8fe61d61d1f399106be361267fa1dc5ab0
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131475604"
---
# <a name="how-to-get-lineage-from-bigquery-into-azure-purview"></a>Procedimiento para extraer el linaje de BigQuery a Azure Purview

En este artículo se detallan los aspectos del linaje de datos del origen de BigQuery en Azure Purview. El requisito previo para ver el linaje de datos en Purview para BigQuery es [examinar su proyecto de BigQuery.](../purview/register-scan-google-bigquery-source.md) 

## <a name="lineage-of-bigquery-artifacts-in-azure-purview"></a>Linaje de artefactos de BigQuery en Azure Purview

Los usuarios pueden buscar artefactos de BigQuery por el nombre, la descripción u otros detalles para ver los resultados pertinentes. En la pestaña de información general y propiedades de los recursos, se muestran los detalles básicos, como la descripción, las propiedades y otra información. En la pestaña de linaje, se muestran las relaciones de recursos entre tablas y vistas de BigQuery. Por lo tanto, las vistas de BigQuery tendrán información de linaje de las tablas. 

El linaje derivado también está disponible en las columnas.

:::image type="content" source="./media/how-to-lineage-google-bigquery/lineage.png" alt-text="Captura de pantalla en la que se muestra cómo se representa el linaje para BigQuery." lightbox="./media/how-to-lineage-google-bigquery/lineage.png":::


## <a name="next-steps"></a>Pasos siguientes

- [Obtener información sobre el linaje de datos en Azure Purview](catalog-lineage-user-guide.md)
- [Vinculación de Azure Data Factory para insertar linaje automatizado](how-to-link-azure-data-factory.md)
