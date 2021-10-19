---
title: Administración de Microsoft Azure Maps Creator
description: En este artículo, aprenderá a administrar Microsoft Azure Maps Creator.
author: stevemunk
ms.author: v-munksteve
ms.date: 10/08/2021
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
ms.openlocfilehash: 09a70e55d796e0eb16d0806fb9a0726ff67ea812
ms.sourcegitcommit: 216b6c593baa354b36b6f20a67b87956d2231c4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2021
ms.locfileid: "129729774"
---
# <a name="manage-azure-maps-creator"></a>Administración de Azure Maps Creator

Puede usar Azure Maps Creator para crear datos privados de mapas de interiores. Mediante el uso de la API de Azure Maps y el módulo Indoor Maps, puede desarrollar aplicaciones web interactivas y dinámicas de planos interiores. Para obtener información sobre precios, consulte [Elección del plan de tarifa adecuado en Azure Maps](choose-pricing-tier.md).

En este artículo le guiaremos por los pasos necesarios para crear y eliminar un recurso de Creator en una cuenta de Azure Maps.

## <a name="create-creator-resource"></a>Creación de un recurso de Creator

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)

2. Vaya al menú de Azure Portal. Seleccione **Todos los recursos** y, a continuación, seleccione la cuenta de Azure Maps.

      :::image type="content" border="true" source="./media/how-to-manage-creator/select-all-resources.png" alt-text="Selección de la cuenta de Azure Maps":::

3. En el panel de navegación, seleccione **Información general de Creator** y, después, seleccione **Crear**.

    :::image type="content" border="true" source="./media/how-to-manage-creator/creator-blade-settings.png" alt-text="Creación de una página de Azure Maps Creator":::

4. Escriba el nombre, la ubicación y las unidades de almacenamiento de aprovisionamiento del mapa para el recurso de Creator y luego seleccione **Revisar y crear**.

   :::image type="content" source="./media/how-to-manage-creator/creator-creation-dialog.png" alt-text="Escriba la página de información de cuenta de Creator":::

5. Revise la configuración y seleccione **Crear**.

    :::image type="content" source="./media/how-to-manage-creator/creator-create-dialog.png" alt-text="Confirmación de la página de configuración de la cuenta de Creator":::

    Después de finalizada la implementación, verá una página con un mensaje de error o de operación correcta.

    :::image type="content" source="./media/how-to-manage-creator/creator-resource-created.png" alt-text="Página de estado de la implementación de recursos":::

6. Haga clic en **Go to resource** (Ir al recurso). En la página con la vista de recursos de Creator se muestra el estado del recurso de Creator y la región demográfica elegida.
      :::image type="content" source="./media/how-to-manage-creator/creator-resource-view.png" alt-text="Página de estado de Creator":::

   >[!NOTE]
   >Para volver a la cuenta de Azure Maps, seleccione **Cuenta de Azure Maps** en el panel de navegación.

## <a name="delete-creator-resource"></a>Eliminación de un recurso de Creator

Para eliminar un recurso de Creator:

1. En la cuenta de Azure Maps, seleccione **Información general** en **Creator**.

2. Seleccione **Eliminar**.

    >[!WARNING]
    >Al eliminar el recurso de Creator de la cuenta de Azure Maps, también se eliminarán las conversiones, los conjuntos de datos, los conjuntos de mosaicos y los conjuntos de estados de características que se crearon con los servicios de Creator.

     :::image type="content" source="./media/how-to-manage-creator/creator-delete.png" alt-text="Página de Creator con el botón eliminar":::

3. Se le pedirá que confirme la eliminación escribiendo el nombre del recurso de Creator. Una vez eliminado el recurso, verá una página de confirmación similar a la siguiente:

     :::image type="content" source="./media/how-to-manage-creator/creator-confirm-delete.png" alt-text="Página de Creator con la confirmación de la eliminación":::

## <a name="authentication"></a>Authentication

Creator hereda la configuración de Access Control (IAM) de Azure Maps. Todas las llamadas de API para el acceso a los datos se deben enviar con reglas de autenticación y autorización.

Los datos de uso de Creator se incorporan en los gráficos de uso de Azure Maps y en el registro de actividad.  Para obtener más información, consulte [Administrar la autenticación en Azure Maps](./how-to-manage-authentication.md).

>[!Important]
>Recomendamos usar lo siguiente:
>
> * Azure Active Directory (Azure AD) en todas las soluciones que se han creado con una cuenta de Azure Maps mediante los servicios de Creator. Para más información sobre Azure AD, consulte [Autenticación de Azure AD](azure-maps-authentication.md#azure-ad-authentication).
>
>* Configuración del control de acceso basado en roles (RBAC) Con esta configuración, los creadores de mapas pueden actuar como si tuvieran el rol Colaborador de datos de Azure Maps, mientras que los usuarios de datos de mapas de Creator pueden actuar con el rol Lector de datos de Azure Maps. Para más información, consulte [Autorización con el control de acceso basado en rol](azure-maps-authentication.md#authorization-with-role-based-access-control).

## <a name="access-to-creator-services"></a>Acceso a los servicios de Creator

Los servicios de Creator y los servicios que usan datos hospedados en Creator (por ejemplo, el servicio Render) son accesibles en una dirección URL geográfica. La dirección URL geográfica viene determinada por la ubicación seleccionada durante la creación. Por ejemplo, si Creator se crea en una región de la ubicación geográfica Estados Unidos, todas las llamadas al servicio de conversión se deben enviar a `us.atlas.microsoft.com/conversions`. Para ver las asignaciones de región a una ubicación geográfica, [consulte el ámbito geográfico del servicio Creator](creator-geographic-scope.md).

Asimismo, todos los datos importados en Creator deben cargarse en la misma ubicación geográfica que el recurso de Creator. Por ejemplo, si Creator se aprovisiona en Estados Unidos, todos los datos sin procesar se deben cargar a través de `us.atlas.microsoft.com/mapData/upload`.

## <a name="next-steps"></a>Pasos siguientes

Introducción a los servicios de Creator para mapas de interiores:

> [!div class="nextstepaction"]
> [Carga de datos](creator-indoor-maps.md#upload-a-drawing-package)

> [!div class="nextstepaction"]
> [Conversión de datos](creator-indoor-maps.md#convert-a-drawing-package)

> [!div class="nextstepaction"]
> [Dataset](creator-indoor-maps.md#datasets)

> [!div class="nextstepaction"]
> [Conjunto de mosaicos](creator-indoor-maps.md#tilesets)

> [!div class="nextstepaction"]
> [Conjunto de estados de características](creator-indoor-maps.md#feature-statesets)

Aprenda a usar los servicios de Creator para representar mapas de interiores en la aplicación:

> [!div class="nextstepaction"]
> [Tutorial de Azure Maps Creator](tutorial-creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Estilo dinámico de planos interiores](indoor-map-dynamic-styling.md)

> [!div class="nextstepaction"]
> [Uso del módulo de planos interiores](how-to-use-indoor-module.md)
