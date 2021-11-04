---
title: Metadatos y linaje de Looker
description: En este artículo se describe la extracción de linaje de datos del origen de Looker.
author: linda33wj
ms.author: jingwang
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 08/12/2021
ms.openlocfilehash: 254bbe548ffb95ce6bf43c8fbb8b107ac87997eb
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131428793"
---
# <a name="how-to-get-lineage-from-looker-into-azure-purview"></a>Extracción de linaje de Looker en Azure Purview

En este artículo se detallan los aspectos del linaje de datos del origen de Looker en Azure Purview. El requisito previo para ver el linaje de datos en Purview para Looker es [examinar su Looker.](../purview/register-scan-looker-source.md) 

## <a name="lineage-of-looker-artifacts-in-azure-purview"></a>Linaje de artefactos de Looker en Azure Purview

Los usuarios pueden buscar artefactos de Looker por el nombre, la descripción u otros detalles para ver los resultados pertinentes. En la pestaña de información general y propiedades de los recursos, se muestran los detalles básicos, como la descripción, las propiedades y otra información. En la pestaña de linaje, se muestran las relaciones de recursos entre los diseños y las vistas de Looker. Por lo tanto, hoy las vistas de Looker tendrán información de linaje de las tablas. 

:::image type="content" source="./media/how-to-lineage-looker/lineage.png" alt-text="Captura de pantalla que muestra cómo se representa el linaje para Looker." lightbox="./media/how-to-lineage-looker/lineage.png":::


## <a name="next-steps"></a>Pasos siguientes

- [Obtener información sobre el linaje de datos en Azure Purview](catalog-lineage-user-guide.md)
- [Vinculación de Azure Data Factory para insertar linaje automatizado](how-to-link-azure-data-factory.md)
