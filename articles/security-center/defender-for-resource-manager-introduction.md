---
title: 'Azure Defender para Resource Manager: ventajas y características'
description: Obtenga información sobre las ventajas y características de Azure Defender para Resource Manager
author: memildin
ms.author: memildin
ms.date: 09/05/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: fff9c94af2c74612e8c07be1f7e125787265a72d
ms.sourcegitcommit: f2d0e1e91a6c345858d3c21b387b15e3b1fa8b4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2021
ms.locfileid: "123541376"
---
# <a name="introduction-to-azure-defender-for-resource-manager"></a>Introducción a Azure Defender para Resource Manager

[Azure Resource Manager](../azure-resource-manager/management/overview.md) es el servicio de implementación y administración para Azure. Proporciona una capa de administración que le permite crear, actualizar y eliminar recursos de la cuenta de Azure. Se usan las características de administración, como el control de acceso, la auditoría y las etiquetas, para proteger y organizar los recursos después de la implementación.

La capa de administración de la nube es un servicio crucial conectado a todos los recursos en la nube. Ese el motivo por el que es también es un objetivo potencial para los atacantes. Por lo tanto, se recomienda que los equipos de operaciones de seguridad supervisen estrechamente la capa de administración de recursos. 

Azure Defender para Resource Manager supervisa automáticamente las operaciones de administración de recursos de cualquier organización, independientemente de que se realicen a través de Azure Portal, las API REST de Azure, la CLI de Azure u otros clientes de programación de Azure. Azure Defender ejecuta análisis de seguridad avanzados para detectar amenazas y avisarle de cualquier actividad sospechosa.

>[!NOTE]
> Varios de estos análisis se realizan mediante [Microsoft Cloud App Security](/cloud-app-security/what-is-cloud-app-security). Para beneficiarse de estos análisis, debe activar una licencia de Cloud App Security. Si tiene una licencia de Cloud App Security, estas alertas están habilitadas de forma predeterminada. Para deshabilitar las alertas:
>
> 1. En el menú de Security Center, seleccione **Precios y configuración**.
> 1. Seleccione la suscripción que desea cambiar.
> 1. Seleccione **Integraciones**.
> 1. Desactive la opción **Permita que Microsoft Cloud App Security acceda a sus datos** y seleccione **Guardar**.


## <a name="availability"></a>Disponibilidad

|Aspecto|Detalles|
|----|:----|
|Estado de la versión:|Disponibilidad general (GA)|
|Precios:|**Azure Defender para Resource Manager** se factura como se muestra en [Precios de Security Center](https://azure.microsoft.com/pricing/details/security-center/).|
|Nubes:|:::image type="icon" source="./media/icons/yes-icon.png"::: Nubes comerciales<br>:::image type="icon" source="./media/icons/yes-icon.png"::: Azure Government<br>:::image type="icon" source="./media/icons/yes-icon.png"::: Azure China 21Vianet|
|||

## <a name="what-are-the-benefits-of-azure-defender-for-resource-manager"></a>¿Cuáles son las ventajas de Azure Defender para Resource Manager?

Azure Defender para Resource Manager sirve de protección para los siguientes problemas:

- **Operaciones de administración de recursos sospechosas**, como las operaciones desde direcciones IP malintencionadas, la deshabilitación de antimalware y scripts sospechosos que se ejecutan en las extensiones de máquina virtual
- **Uso de kits de herramientas de explotación** como Microburst o PowerZure
- **Movimiento lateral** del nivel de administración de Azure al plano de datos de los recursos de Azure

:::image type="content" source="media/defender-for-resource-manager-introduction/consistent-management-layer-with-defender.png" alt-text="Diagrama con información general sobre Azure Resource Manager.":::

En la [página de referencia de alertas](alerts-reference.md#alerts-resourcemanager) se encuentra una lista completa de las alertas que proporciona Azure Defender para Resource Manager.


 ## <a name="how-to-investigate-alerts-from-azure-defender-for-resource-manager"></a>Investigación de alertas de Azure Defender para Resource Manager

Las alertas de seguridad de Azure Defender para Resource Manager se basan en las amenazas detectadas por la supervisión de operaciones de Azure Resource Manager. Azure Defender usa orígenes de registros internos de Azure Resource Manager, así como el registro de actividad de Azure, un registro de plataforma en Azure que proporciona información detallada sobre los eventos de nivel de suscripción.

Obtenga más información sobre el [registros de actividad de Azure](../azure-monitor/essentials/activity-log.md).

Para investigar las alertas de Azure Defender para Resource Manager:

1. Abra el registro de actividad de Azure.

    :::image type="content" source="media/defender-for-resource-manager-introduction/opening-azure-activity-log.png" alt-text="Cómo abrir el registro de actividad de Azure.":::

1. Filtre los eventos para:
    - La suscripción mencionada en la alerta
    - El período de tiempo de la actividad detectada
    - La cuenta de usuario relacionada (si procede)

1. Busque actividades sospechosas.

> [!TIP]
> Para disfrutar de una experiencia de investigación mejor y más rica, transmita los registros de actividad de Azure a Azure Sentinel como se describe en [Conexión de datos del registro de actividad de Azure](../sentinel/connect-azure-activity.md).



## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha obtenido información sobre Azure Defender para Resource Manager. 

> [!div class="nextstepaction"]
> [Habilitación de Azure Defender](enable-azure-defender.md)

Para obtener material relacionado, vea el siguiente artículo: 

- Las alertas de seguridad las puede haber generado Security Center, o bien las puede haber recibido Security Center de distintos productos de seguridad. Para exportar todas estas alertas a Azure Sentinel, a cualquier SIEM de terceros o a cualquier otra herramienta externa, siga las instrucciones de [Exportación de alertas a un SIEM](continuous-export.md).