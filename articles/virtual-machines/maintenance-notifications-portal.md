---
title: Uso del portal para notificaciones de mantenimiento
description: Use el portal para ver las notificaciones de mantenimiento de máquinas virtuales que se ejecutan en Azure e iniciar el mantenimiento de autoservicio.
author: shants123
ms.service: virtual-machines
ms.subservice: maintenance
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 09/08/2021
ms.author: shants
ms.openlocfilehash: a027014500fa9e4a6ca1f68e2da5fb62df37e5ec
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129216059"
---
# <a name="handling-planned-maintenance-notifications-using-the-portal"></a>Control de las notificaciones de mantenimiento planeado con el portal

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado flexibles :heavy_check_mark: Conjuntos de escalado uniformes

Una vez que se programa una tanda de [mantenimiento planeado](maintenance-notifications.md), puede consultar una lista de las máquinas virtuales que se ven afectadas. 

Puede usar Azure Portal y buscar máquinas virtuales con mantenimiento programado.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

2. En el panel de navegación de la izquierda, haga clic en **Máquinas virtuales**.

3. En el panel Máquinas virtuales, seleccione el botón **Mantenimiento -> Mantenimiento de máquinas virtuales** para abrir la lista con las columnas de mantenimiento.

   **Estado de mantenimiento**: muestra el estado de mantenimiento para la máquina virtual. Estos son los valores posibles:
      
    | Value | Descripción |
    |-------|-------------|
    | Empezar ahora | La máquina virtual está en la ventana de mantenimiento de autoservicio, lo que le permite iniciar el mantenimiento usted mismo. Vea a continuación cómo iniciar el mantenimiento en la máquina virtual. | 
    | Programado | La máquina virtual está programada para el mantenimiento sin opción de que inicie el mantenimiento. Puede obtener información de la ventana de mantenimiento mediante la selección de la ventana Mantenimiento: Programado en esta vista o haciendo clic en la máquina virtual. | 
    | Ya se actualizó | La máquina virtual ya se actualizó y no se requiere ninguna otra acción en este momento. | 
    | Inténtelo de nuevo más tarde | Ha intentado iniciar el mantenimiento sin conseguirlo. Podrá utilizar la opción de mantenimiento de autoservicio más adelante. | 
    | Intentar ahora | Puede volver a intentar un mantenimiento automático que se intentó anteriormente sin éxito. | 
    | - | La máquina virtual no forma parte de una oleada de mantenimiento planeado. |

   **Ventana Mantenimiento: Autoservicio**: muestra la ventana de tiempo en la que puede iniciar el mantenimiento usted mismo en las máquinas virtuales.
   
   **Ventana Mantenimiento: programado**: muestra la ventana de tiempo en la que Azure reiniciará la máquina virtual con el fin de realizar el mantenimiento. 



## <a name="notification-and-alerts-in-the-portal"></a>Notificaciones y alertas en el portal

Azure comunica una programación para el mantenimiento planeado enviando un correo electrónico al grupo de propietario y copropietarios de la suscripción. Puede agregar destinatarios y canales adicionales a esta comunicación creando alertas de registro de actividad de Azure. Para más información, consulte [Creación de alertas del registro de actividad en notificaciones del servicio](../service-health/alerts-activity-log-service-notifications-portal.md).

Asegúrese de establecer **Tipo de evento** en **Mantenimiento planeado** y **Servicios** en **Virtual Machine Scale Sets** o en **Virtual Machines**.

## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Inicio del mantenimiento en la máquina virtual desde el portal

Al examinar los detalles de la máquina virtual, podrá ver más detalles relacionados con el mantenimiento.  
En la parte superior de la vista de detalles de la máquina virtual, se agregará una nueva cinta de opciones de notificación si la máquina virtual se incluye en una tanda de mantenimiento planeado. Además, se agrega una nueva opción para iniciar el mantenimiento siempre que sea posible. 


Haga clic en la notificación de mantenimiento para ver la página de mantenimiento con más detalles en el mantenimiento planeado. Desde allí podrá **iniciar el mantenimiento** en su máquina virtual.

Una vez que comience el mantenimiento, en pocos minutos comenzará en la máquina virtual y el estado de mantenimiento se actualizará para reflejar el resultado.

Si se perdió la ventana de autoservicio, todavía podrá ver la ventana cuando Azure mantenga la máquina virtual. 


## <a name="next-steps"></a>Pasos siguientes

El mantenimiento planeado también se puede controlar con la [CLI de Azure](maintenance-notifications-cli.md) o con [PowerShell](maintenance-notifications-powershell.md).
