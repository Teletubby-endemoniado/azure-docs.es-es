---
title: Conexión a GitHub
titleSuffix: Azure Data Factory & Azure Synapse
description: Uso de GitHub para especificar referencias de entidad de Common Data Model
author: linda33wj
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.author: jingwang
ms.openlocfilehash: e68cb1e537fcf89a947a06ac11ff08f3ca6bec9d
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124815219"
---
# <a name="use-github-to-read-common-data-model-entity-references"></a>Uso de GitHub para leer referencias de entidad de Common Data Model

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

El conector de GitHub en las canalizaciones de Azure Data Factory y Synapse Analytics sirve únicamente para recibir el esquema de referencia de entidades para el formato de [Common Data Model](format-common-data-model.md) en el flujo de datos de asignación.

## <a name="create-a-linked-service-to-github-using-ui"></a>Creación de un servicio vinculado para GitHub mediante la interfaz de usuario

Siga estos pasos para crear un servicio vinculado para GitHub en la interfaz de usuario de Azure Portal.

1. Vaya a la pestaña Administrar del área de trabajo de Azure Data Factory o Synapse y seleccione Servicios vinculados; luego haga clic en Nuevo:

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory)

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Data Factory.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Captura de pantalla de la creación de un nuevo servicio vinculado con la interfaz de usuario de Azure Synapse.":::

2. Busque GitHub y seleccione el conector de GitHub.

   :::image type="content" source="media/connector-github/github-connector.png" alt-text="Captura de pantalla del GitHub conector.":::    


1. Configure los detalles del servicio, pruebe la conexión y cree el nuevo servicio vinculado.

   :::image type="content" source="media/connector-github/configure-github-linked-service.png" alt-text="Captura de pantalla de la configuración del servicio vinculado en GitHub.":::


## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de GitHub.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en **GitHub**. | sí
| userName | Nombre de usuario de GitHub | sí |
| password | Contraseña de GitHub | sí |

## <a name="next-steps"></a>Pasos siguientes

Cree un [conjunto de datos de origen](data-flow-source.md) en el flujo de datos de asignación.