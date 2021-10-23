---
title: Configuración de alertas de disponibilidad con Azure Application Insights | Microsoft Docs
description: Obtenga información sobre cómo configurar las pruebas web en Application Insights. Obtenga alertas si un sitio web deja de estar disponible o responde con lentitud.
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.openlocfilehash: 5d5d746c1d1df00118a8b08816440c7d92372e0a
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2021
ms.locfileid: "130134096"
---
# <a name="availability-alerts"></a>Alertas de disponibilidad

[Azure Application Insights](./app-insights-overview.md) envía solicitudes web a su aplicación a intervalos regulares desde puntos de todo el mundo. Puede enviar una alerta si la aplicación no responde o si responde de manera demasiada lenta.

## <a name="enable-alerts"></a>Habilitación de alertas

Las alertas ahora se habilitan automáticamente de forma predeterminada, pero para configurar por completo la alerta, primero debe crear inicialmente la prueba de disponibilidad.

![Experiencia de creación](./media/availability-alerts/create-test.png)

> [!NOTE]
>  Con las [nuevas alertas unificadas](../alerts/alerts-overview.md), la gravedad de la regla de alertas y las preferencias de notificación con [grupos de acciones](../alerts/action-groups.md)**se tienen que** configurar en la experiencia de alertas. Sin los pasos siguientes, solo recibirá las notificaciones del portal.

1. Después de guardar la prueba de disponibilidad, en la pestaña de detalles, haga clic en el signo de puntos suspensivos junto a la prueba que acaba de realizar. Haga clic en “Editar alerta”.

   ![Captura de pantalla que muestra la opción Editar alerta seleccionada en el menú.](./media/availability-alerts/edit-alert.png)

2. Establezca el nivel de gravedad deseado, la descripción de la regla y lo más importante: el grupo de acciones con las preferencias de notificación que le gustaría usar para esta regla de alertas.

   ![Captura de pantalla que muestra la página Administración de reglas en la que puede editar la regla.](./media/availability-alerts/set-action-group.png)

> [!NOTE]
> Las alertas de disponibilidad creadas a través de esta experiencia están basadas en el estado. Esto significa que, cuando se cumplan los criterios de alerta, se generará una única alerta cuando el sitio se detecte como no disponible. Si el sitio sigue fuera de servicio la próxima vez que se evalúen los criterios de alerta, no se generará una nueva alerta. Por lo tanto, si el sitio estuvo fuera de servicio durante una hora y había configurado una alerta de correo electrónico, solo recibiría un correo electrónico cuando el sitio estuviera fuera de servicio y un correo electrónico posterior cuando se realizara una copia de seguridad del sitio. No recibiría alertas continuas recordándole que el sitio seguía sin estar disponible.

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Alertas para X de las Y ubicaciones que notifican errores

La regla de alertas X de las Y ubicaciones se habilita de forma predeterminada en la [nueva experiencia de alertas unificadas](../alerts/alerts-overview.md) cuando se crea una prueba de disponibilidad. Puede cancelar esta acción si selecciona la opción "clásica" o si deshabilita la regla de alertas.

> [!NOTE]
> Siga los pasos anteriores para configurar los grupos de acción para recibir notificaciones cuando se desencadene la alerta. Sin este paso, solo recibirá las notificaciones en el portal cuando se desencadene la regla.
>

### <a name="alert-on-availability-metrics"></a>Alertas sobre las métricas de disponibilidad

Mediante las [nuevas alertas unificadas](../alerts/alerts-overview.md), también puede generar alertas sobre la disponibilidad total segmentada y las métricas de duración de la prueba:

1. Seleccione un recurso de Application Insights en la experiencia de métricas y seleccione una métrica de disponibilidad:

    ![Selección de métricas de disponibilidad](./media/availability-alerts/select-metric.png)

2. Al configurar la opción de alertas desde el menú se abrirá la nueva experiencia, donde podrá seleccionar pruebas o ubicaciones específicas para establecer la regla de alertas. También puede configurar los grupos de acciones para esta regla de alertas.

### <a name="alert-on-custom-analytics-queries"></a>Alertas sobre las consultas de análisis personalizadas

Mediante las [nuevas alertas unificadas](../alerts/alerts-overview.md), puede generar alertas sobre las [consultas de registro personalizadas](../alerts/alerts-unified-log.md). Con las consultas personalizadas, puede enviar alertas sobre cualquier condición arbitraria que le ayude a obtener los indicadores de problemas de disponibilidad más fiables. Esta opción también la puede usar si envía resultados personalizados de disponibilidad mediante el SDK de TrackAvailability.

> [!Tip]
> Las métricas sobre datos de disponibilidad incluyen los resultados personalizados de disponibilidad que puede enviar mediante una llamada a nuestro SDK de TrackAvailability. Puede usar la compatibilidad con las alertas sobre métricas para generar alertas sobre resultados personalizados de disponibilidad.
>

## <a name="automate-alerts"></a>Automatización de alertas

Para automatizar este proceso con plantillas de Azure Resource Manager, consulte la documentación relativa a la [creación de una alerta de métrica con una plantilla de Resource Manager](../alerts/alerts-metric-create-templates.md#template-for-an-availability-test-along-with-a-metric-alert).

## <a name="troubleshooting"></a>Solución de problemas

[Artículo de solución de problemas](troubleshoot-availability.md) dedicado.

## <a name="next-steps"></a>Pasos siguientes

* [Pruebas web de varios pasos](availability-multistep.md)
* [Pruebas web de ping de URL](monitor-web-app-availability.md)

