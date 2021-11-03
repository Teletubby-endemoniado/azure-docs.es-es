---
title: Creación de un centro de notificaciones de Azure con Azure Portal | Microsoft Docs
description: En este tutorial, aprenderá a crear un centro de notificaciones de Azure mediante Azure Portal.
services: notification-hubs
author: sethmanheim
ms.author: sethm
manager: femila
ms.reviewer: thsomasu
ms.date: 08/04/2020
ms.lastreviewed: 02/14/2019
ms.topic: quickstart
ms.service: notification-hubs
ms.workload: mobile
ms.custom: mode-portal
ms.openlocfilehash: 1f9ba3f32a2b7f5b0845593ec3439cf826b50ce6
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131070135"
---
# <a name="quickstart-create-an-azure-notification-hub-in-the-azure-portal"></a>Inicio rápido: Creación de un centro de notificaciones de Azure en Azure Portal

Azure Notification Hubs proporciona un motor de inserción de escalabilidad horizontal y fácil de usar que le permite enviar notificaciones a cualquier plataforma (iOS, Android, Windows, Kindle, Baidu, etc.) desde cualquier back-end (en la nube o local). Para más información sobre el servicio, consulte [¿Qué es Azure Notification Hubs?](notification-hubs-push-notification-overview.md).

En este inicio rápido, creará un centro de notificaciones en Azure Portal. La primera sección proporciona los pasos para crear un espacio de nombres de Notification Hubs y un centro en ese espacio de nombres. La segunda sección proporciona los pasos para crear un centro de notificaciones en un espacio de nombres de Notification Hubs existente.

## <a name="create-a-namespace-and-a-notification-hub"></a>Creación de un espacio de nombres y un centro de notificaciones

En esta sección, creará un espacio de nombres y un centro en el espacio de nombres.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

## <a name="create-a-notification-hub-in-an-existing-namespace"></a>Creación de un centro de notificaciones en un espacio de nombres existente

En esta sección, creará un centro de notificaciones en un espacio de nombres existente.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el menú izquierdo, busque **Notification Hub** (Centro de notificaciones), seleccione la **estrella** (`*`) junto a **Notification Hub Namespaces** (Espacios de nombres de centros de notificaciones) para añadirlo a la sección **FAVORITOS** del menú izquierdo. Seleccione **Notification Hub Namespaces** (Espacios de nombres del centros de notificaciones).

      ![Azure Portal: selección de Notification Hub Namespaces](./media/create-notification-hub-portal/select-notification-hub-namespaces-all-services.png)
3. En la página **Notification Hub Namespaces** (Espacios de nombres del centros de notificaciones), seleccione el espacio de nombres en la lista.

      ![Seleccionar el espacio de nombres en la lista](./media/create-notification-hub-portal/select-namespace.png)
4. En la página **Espacio de nombres del centros de notificaciones**, seleccione **Agregar concentrador** en la barra de herramientas.

      ![Espacios de nombres del centros de notificaciones: botón Agregar concentrador](./media/create-notification-hub-portal/add-hub-button.png)
5. En la página **Nuevo centro de notificaciones**, escriba un nombre para el centro de notificaciones y seleccione **Aceptar**.

      ![Página Nuevo centro de notificaciones: introduzca un nombre para el centro](./media/create-notification-hub-portal/new-notification-hub-page.png)
6. Seleccione **Notificaciones** (icono de campana) en la parte superior para ver el estado de la implementación del nuevo centro. Seleccione **X** en la esquina derecha para cerrar la ventana de notificaciones.

      ![Notificación de implementación](./media/create-notification-hub-portal/deployment-notification.png)
7. Actualice la página web **Notification Hub Namespaces** (Espacios de nombres del centros de notificaciones) para ver el nuevo centro en la lista.

      ![Captura de pantalla que muestra la página web Espacios de nombres del centro de notificaciones con el nuevo centro en la lista.](./media/create-notification-hub-portal/new-hub-in-list.png)
8. Seleccione el **centro de notificaciones** para ver la página principal del centro de notificaciones.

      ![Captura de pantalla que muestra la página principal del centro de notificaciones.](./media/create-notification-hub-portal/hub-home-page.png)

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un centro de notificaciones. Para obtener información sobre cómo configurar el centro con la configuración del sistema de notificación de plataforma (PNS), consulte [Configuración de un centro de notificaciones con la configuración de PNS](configure-notification-hub-portal-pns-settings.md).
