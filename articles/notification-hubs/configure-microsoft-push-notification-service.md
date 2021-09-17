---
title: Configuración del Servicio de notificaciones push de Microsoft en Azure Notification Hubs | Microsoft Docs
description: Aprenda a configurar el Servicio de notificaciones push de Microsoft para un centro de notificaciones de Azure.
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 08/23/2021
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: c2b46d3f4831ed98e54a47d8daa77107a625e362
ms.sourcegitcommit: 2da83b54b4adce2f9aeeed9f485bb3dbec6b8023
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2021
ms.locfileid: "122770901"
---
# <a name="configure-microsoft-push-notification-service-mpns-settings-in-the-azure-portal"></a>Configuración del Servicio de notificaciones push de Microsoft (MPNS) en Azure Portal

> [!NOTE]
> El servicio de notificaciones push de Microsoft (MPNS) está en desuso y ya no se admite.

En este artículo podrá ver cómo establecer la configuración del Servicio de notificaciones push de Microsoft (MPNS) para un centro de notificaciones de Azure mediante Azure Portal.

## <a name="prerequisites"></a>Prerrequisitos

Si aún no ha creado un centro de notificaciones, cree uno ahora. Para más información, consulte [Creación de un centro de notificaciones de Azure en Azure Portal](create-notification-hub-portal.md).

## <a name="configure-microsoft-push-notification-service-mpns"></a>Configuración del Servicio de notificaciones push de Microsoft (MPNS)

En el siguiente procedimiento se describen los pasos para establecer la configuración del Servicio de notificaciones push de Microsoft (MPNS) para un centro de notificaciones:

1. En Azure Portal, en la página **Centro de notificaciones**, seleccione **Windows Phone (MPNS)** en el menú izquierdo.
2. Habilite las notificaciones push sin autenticar o autenticadas:

   a. Para habilitar las notificaciones push no autenticadas, seleccione **Habilitar notificaciones de inserción sin autenticar** > **Guardar**.

      ![Captura de pantalla que muestra cómo habilitar las notificaciones push no autenticadas](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. Para habilitar las notificaciones push no autenticadas:
      * Seleccione **Cargar certificado** en la barra de herramientas.
      * Seleccione el icono de archivo y el archivo de certificado.
      * Escriba la contraseña del certificado.
      * Seleccione **Aceptar**.
      * En la página **Windows Phone (MPNS)** , seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

Para ver un tutorial con instrucciones paso a paso sobre cómo insertar notificaciones en dispositivos de Windows Phone mediante Azure Notification Hubs y el Servicio de notificaciones push de Microsoft (MPNS), consulte [Envío de notificaciones push a aplicaciones de Windows Phone mediante Notification Hubs](notification-hubs-windows-mobile-push-notifications-mpns.md).
