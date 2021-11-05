---
title: Creación de un registro de esquemas de Azure Event Hubs
description: En este artículo se muestra cómo crear un registro de esquemas en un espacio de nombres de Azure Event Hubs.
ms.topic: quickstart
ms.date: 06/01/2021
ms.custom: references_regions, ignite-fall-2021
ms.openlocfilehash: f15327c6e5f35dcdd37ee45222f351257c846f5f
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131070876"
---
# <a name="create-an-azure-event-hubs-schema-registry"></a>Creación de un registro de esquemas de Azure Event Hubs
En este artículo se muestra cómo crear un grupo de esquemas con esquemas en un registro de esquemas hospedado en Azure Event Hubs. Para obtener información general sobre la característica de Registro de esquemas de Azure Event Hubs, vea [Registro de esquemas de Azure en Event Hubs](schema-registry-overview.md).

> [!NOTE]
> - La característica no está disponible en el nivel **básico**.
> - Si el centro de eventos se encuentra en una **red virtual**, no podrá crear esquemas en Azure Portal a menos que tenga acceso al portal desde una máquina virtual de la misma red virtual. 

## <a name="prerequisites"></a>Requisitos previos
[Cree un espacio de nombres de Event Hubs](event-hubs-create.md#create-an-event-hubs-namespace). También puede usar un espacio de nombres existente. 

## <a name="create-a-schema-group"></a>Creación de un grupo de esquemas
1. Desplácese a la página **Espacio de nombres de Event Hubs**. 
1. Seleccione **Schema Registry** (Registro de esquemas) en el menú de la izquierda. Para crear un grupo de esquemas, seleccione **+ Schema Group** (+ Grupo de esquemas) en la barra de herramientas. 

    :::image type="content" source="./media/create-schema-registry/namespace-page.png" alt-text="Imagen que muestra la página del registro de esquema en Azure Portal":::
1. En la página **Crear grupo de esquemas**, siga estos pasos:
    1. Escriba un **nombre** para el grupo de esquemas.
    1. En **Tipo de serialización**, elija un formato de serialización que se aplique a todos los esquemas del grupo de esquemas. Actualmente, **Avro** es el único tipo admitido, así que seleccione **Avro**. 
    1. Seleccione un **modo de compatibilidad** para todos los esquemas del grupo. En el caso de **Avro**, se admiten los modos de compatibilidad con versiones anteriores y posteriores. 
    1. Después, seleccione **Crear** para crear el grupo de esquemas. 
    
        :::image type="content" source="./media/create-schema-registry/create-schema-group-page.png" alt-text="Imagen que muestra la página para crear un grupo de esquemas":::
1. Seleccione el nombre del **grupo de esquemas** en la lista de grupos de esquemas.

    :::image type="content" source="./media/create-schema-registry/select-schema-group.png" alt-text="Imagen que muestra el grupo de esquemas en la lista seleccionada.":::    
1. Verá la página de **Grupo de esquemas** para el grupo.

    :::image type="content" source="./media/create-schema-registry/schema-group-page.png" alt-text="Imagen que muestra la página del grupo de esquemas":::
    

## <a name="add-a-schema-to-the-schema-group"></a>Incorporación de un esquema al grupo de esquemas
En esta sección, agregará un esquema al grupo de esquemas mediante Azure Portal. 

1. En la página **Grupo de esquemas**, seleccione **+ Schema** (+ Esquema) en la barra de herramientas. 
1. En la página **Crear esquema**, siga estos pasos:
    1. En **Nombre**, escriba **orderschema**.
    1. Escriba el siguiente **esquema** en el cuadro de texto. También puede seleccionar un archivo con el esquema.
    
        ```json
        {
          "namespace": "com.azure.schemaregistry.samples",
          "type": "record",
          "name": "Order",
          "fields": [
            {
              "name": "id",
              "type": "string"
            },
            {
              "name": "amount",
              "type": "double"
            }
          ]
        }
        ```
    1. Seleccione **Crear**. 
1. Seleccione el **esquema** de la lista de esquemas. 

    :::image type="content" source="./media/create-schema-registry/select-schema.png" alt-text="Imagen que muestra el esquema seleccionado.":::
1. Verá la página de **Información general del esquema** del esquema. 

    :::image type="content" source="./media/create-schema-registry/schema-overview-page.png" alt-text="Imagen que muestra la página de información general del esquema.":::    
1. Si hay varias versiones de un esquema, se verán en la lista desplegable **Versiones**. Seleccione una versión para cambiar a ese esquema de versiones. 

## <a name="create-a-new-version-of-schema"></a>Creación de una nueva versión del esquema

1. Actualice el esquema en el cuadro de texto y seleccione **Validar**. En el ejemplo siguiente, se ha agregado un nuevo campo `description` al esquema. 

    :::image type="content" source="./media/create-schema-registry/update-schema.png" alt-text="Imagen que muestra la página de actualización del esquema":::    
    
1. Revise el estado y los cambios de validación y seleccione **Guardar**. 

    :::image type="content" source="./media/create-schema-registry/compare-save-schema.png" alt-text="Imagen que muestra la página de revisión que muestra el estado de validación, los cambios y Guardar":::     
1. Verá que se ha seleccionado `2` para la **versión** en la página de **Información general del esquema**. 

    :::image type="content" source="./media/create-schema-registry/new-version.png" alt-text="Imagen que muestra la nueva versión del esquema":::    
1. Seleccione `1` para ver la versión 1 del esquema. 


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre el registro de esquemas, vea [Registro de esquemas de Azure en Event Hubs](schema-registry-overview.md).
