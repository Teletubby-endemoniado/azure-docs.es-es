---
title: Solución de problemas de vistas de costos empresariales en Azure
description: Obtenga información para resolver los problemas que podría tener con vistas de costos de organización dentro de Azure Portal.
author: bandersmsft
ms.reviewer: amberb
ms.service: cost-management-billing
ms.subservice: enterprise
ms.topic: troubleshooting
ms.date: 10/22/2021
ms.author: banders
ms.custom: sapnakeshari
ms.openlocfilehash: 02507604c1194d726014453bde6cd03c2c21aa31
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130255433"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Solución de problemas de vistas de costos empresariales

Dentro de las inscripciones empresariales, hay varias configuraciones que podrían provocar que los usuarios de la inscripción no pudieran ver los costos.  El administrador de inscripciones administra estas configuraciones. O bien, si la inscripción no se compró directamente a través de Microsoft, es el asociado quien administra las configuraciones.  Este artículo le ayuda a comprender cuáles son las configuraciones y de qué manera pueden afectar a la inscripción. Estas opciones son independientes de los roles de Azure.

## <a name="enable-access-to-costs"></a>Habilitación del acceso a los costos

¿Aparece un mensaje de acceso no autorizado o que dice que *las vistas de costos están deshabilitadas en su inscripción?* cuando busca información de costos?
![Captura de pantalla que muestra "no autorizado" en el campo de costo actual de la suscripción.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/unauthorized.png)

El motivo podría ser uno de los siguientes:

1. Adquirió Azure a través de un asociado empresarial y el asociado no ha publicado aún los precios. Póngase en contacto con su asociado para que actualicen los precios en [Enterprise Portal](https://ea.azure.com).
2. Si usted es cliente directo de EA, hay un par de posibilidades:
    * Es el propietario de la cuenta y su administrador de inscripciones ha deshabilitado la opción para que el **propietario de la cuenta vea los cargos**.  
    * Es el administrador del departamento y su administrador de inscripciones ha deshabilitado la opción para que el **administrador del departamento vea los cargos**.
    * Póngase en contacto con el administrador de inscripciones para acceder. El administrador de inscripciones puede actualizar la configuración en [Azure Portal](https://portal.azure.com/). Vaya al menú **Directivas** para cambiar la configuración. 
    * El administrador de inscripciones puede actualizar la configuración en [Enterprise Portal](https://ea.azure.com/manage/enrollment).

      ![Captura de pantalla que muestra la configuración de Enterprise Portal para ver los cargos.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/ea-portal-settings.png)
    
 

## <a name="asset-is-unavailable"></a>Recurso no disponible

Si recibe el mensaje de error que indica que **este recurso no está disponible** al intentar acceder a una suscripción o un grupo de administración, significa que no tiene el rol correcto para ver este elemento.  

![Captura de pantalla que muestra el mensaje "recurso no disponible".](./media/enterprise-mgmt-grp-troubleshoot-cost-view/asset-not-found.png)

Pida acceso al administrador de suscripciones de Azure o al administrador del grupo de administración. Para más información, consulte [Asignación de roles de Azure mediante Azure Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Pasos siguientes
- Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).
