---
title: 'Tutorial de respuesta a alertas: Microsoft Defender for Cloud'
description: En este tutorial aprenderá a realizar una evaluación de prioridades de las alertas de seguridad y a determinar la causa raíz y el ámbito de una alerta.
services: security-center
author: memildin
manager: rkarlin
ms.assetid: 181e3695-cbb8-4b4e-96e9-c4396754862f
ms.service: security-center
ms.topic: tutorial
ms.date: 02/17/2021
ms.author: memildin
ms.custom: ignite-fall-2021
ms.openlocfilehash: ac1a72efe9d71725e27162ee19fc3b8b02645023
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131014290"
---
# <a name="tutorial-triage-investigate-and-respond-to-security-alerts"></a>Tutorial: Evaluación de prioridades, investigación y respuesta a alertas de seguridad

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Microsoft Defender for Cloud analiza continuamente las cargas de trabajo de la nube híbrida y usa el análisis avanzado y la inteligencia sobre amenazas para alertarle de cualquier actividad potencialmente malintencionada en los recursos en la nube. También puede integrar alertas de otros productos y servicios de seguridad en Defender for Cloud. Una vez que se genera una alerta, es necesario investigarla y corregir el posible problema de seguridad. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Evaluar las prioridades de las alertas de seguridad
> * Investigar una alerta de seguridad para determinar la causa raíz
> * Responder a una alerta de seguridad y mitigar la causa principal

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos
Para examinar las características que se tratan en este tutorial, debe tener habilitadas las características de seguridad mejoradas de Defender for Cloud. Puede probarlas sin costo alguno. Para más información, consulte la [página de precios](https://azure.microsoft.com/pricing/details/security-center/). La guía de inicio rápido [Introducción a Defender for Cloud](get-started.md) le guiará en la actualización.


## <a name="triage-security-alerts"></a>Evaluación de las prioridades de las alertas de seguridad
Defender for Cloud proporciona una vista unificada de todas las alertas de seguridad. Las alertas de seguridad se clasifican en función de la gravedad de la actividad detectada. 

Evalúe la prioridad de las alertas en la página **Alertas de seguridad**:

:::image type="content" source="media/managing-and-responding-alerts/alerts-page.png" alt-text="Lista de alertas de seguridad de Microsoft Defender for Cloud":::

Esta página se usa para revisar las alertas de seguridad activas en su entorno para decidir qué alerta hay que investigar primero.

Al evaluar la prioridad de las alertas de seguridad, asigne prioridad a las alertas en función de la gravedad de la alerta solucionando en primer lugar las alertas con una mayor gravedad. Obtenga más información sobre la gravedad de las alertas en [¿Cómo se clasifican las alertas?](alerts-overview.md#how-are-alerts-classified).

> [!TIP]
> Puede conectar Microsoft Defender for Cloud a las soluciones SIEM más populares, incluido Microsoft Sentinel, y consumir las alertas desde la herramienta que prefiera. Puede encontrar más información en [Transmisión de alertas a una solución de administración de servicios de TI, SIEM o SOAR](export-to-siem.md).


## <a name="investigate-a-security-alert"></a>Investigación de una alerta de seguridad

Cuando haya decidido qué alerta desea investigar primero:

1. Seleccione la alerta que desee.
1. En la página de información general de alertas, seleccione el recurso que desea investigar en primer lugar.
1. Comience la investigación en el panel izquierdo, que muestra información de alto nivel sobre la alerta de seguridad.

    :::image type="content" source="./media/tutorial-security-incident/alert-details-left-pane.png" alt-text="El panel izquierdo de la página de detalles de la alerta que resalta la información de alto nivel.":::

    Este panel muestra:
    - Gravedad de la alerta, estado y tiempo de actividad.
    - Descripción que explica la actividad precisa detectada.
    - Recursos afectados.
    - Intención de la cadena de eliminación de la actividad en la matriz de MITRE ATT & CK

1. Para obtener información más detallada que pueda ayudarle a investigar la actividad sospechosa, examine la pestaña **Detalles de la alerta**.

1. Cuando haya repasado la información de esta página, puede que sea suficiente para continuar con una respuesta. Si necesita más detalles:

    - Póngase en contacto con el propietario del recurso para comprobar si la actividad detectada es un falso positivo.
    - Investigue los registros sin procesar que ha generado el recurso atacado.

## <a name="respond-to-a-security-alert"></a>Respuesta a una alerta de seguridad
Después de investigar una alerta de seguridad y comprender su ámbito, puede responder a la alerta desde Microsoft Defender for Cloud:

1.  Abra la pestaña **Tomar medidas** para ver las respuestas recomendadas.

    :::image type="content" source="./media/tutorial-security-incident/alert-details-take-action.png" alt-text="Pestaña para tomar medidas ante las alertas de seguridad." lightbox="./media/tutorial-security-incident/alert-details-take-action.png":::

1.  Consulte la sección **Mitigación de la amenaza** para ver los pasos de investigación manual necesarios para mitigar el problema.
1.  Para proteger los recursos y evitar futuros ataques de este tipo, lleve a cabo las recomendaciones de seguridad que se indican en la sección **Prevención ante futuros ataques**.
1.  Para desencadenar una aplicación lógica con pasos de respuesta automatizados, use la sección **Desencadenamiento de respuesta automatizada**.
1.  Si la actividad detectada *no es* malintencionada, puede suprimir las alertas futuras de este tipo mediante la sección **Supresión de alertas similares**.

1.  Cuando haya completado la investigación en la alerta y respondido de la manera adecuada, cambie el estado a **Descartado**.

    :::image type="content" source="./media/tutorial-security-incident/set-status-dismissed.png" alt-text="Establecimiento del estado de una alerta":::

    Así se elimina la alerta de la lista de alertas principales. Puede usar el filtro de la página de la lista de alertas para ver todas las alertas con el estado **Descartado**.

1.  Le recomendamos que proporcione comentarios sobre la alerta a Microsoft:
    1. Marque la alerta como **Útil** o **No útil**.
    1. Seleccione un motivo y agregue un comentario.

        :::image type="content" source="./media/tutorial-security-incident/alert-feedback.png" alt-text="Proporcionar a Microsoft comentarios sobre la utilidad de una alerta.":::

    > [!TIP]
    > Examinamos sus comentarios para mejorar nuestros algoritmos y proporcionar mejores alertas de seguridad.

## <a name="end-the-tutorial"></a>Finalización del tutorial

Otras guías de inicio rápido y tutoriales de esta colección se basan en los valores de esta. Si tiene previsto seguir trabajando con las guías de inicio rápido y los tutoriales posteriores, mantenga el aprovisionamiento automático y las características de seguridad mejoradas de Defender for Cloud habilitados. 

Si no tiene previsto continuar o desea deshabilitar cualquiera de estas características:

1. En el menú de Defender for Cloud, seleccione **Environment settings** (Configuración del entorno).
1. Seleccione la suscripción correspondiente.
1. Seleccione **Defender plans** (Planes de Defender) y, después, **Enhanced security off** (Seguridad mejorada desactivada).

    :::image type="content" source="./media/enable-enhanced-security/disable-plans.png" alt-text="Habilitación o deshabilitación de las características de seguridad mejoradas de Defender for Cloud.":::

1. Seleccione **Guardar**.

    > [!NOTE]
    > Después de deshabilitar las características de seguridad mejoradas (tanto si deshabilita un plan único como todos los planes a la vez) la recopilación de datos podrá continuar durante un breve período de tiempo. 

1. En el menú de Defender for Cloud, seleccione **Environment settings** (Configuración del entorno).
1. Seleccione la suscripción correspondiente.
1. Seleccione **Aprovisionamiento automático**.
1. Deshabilite las extensiones pertinentes.

    >[!NOTE]
    > La deshabilitación del aprovisionamiento automático no quita el agente de Log Analytics de las máquinas virtuales de Azure que ya lo tienen. La deshabilitación del aprovisionamiento automático limita la supervisión de seguridad de los recursos.

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha obtenido información acerca de las características de Defender for Cloud que se deben usar cuando se responde a una alerta de seguridad. Para obtener material relacionado, vea lo siguiente:

- [Respuesta a las alertas de Microsoft Defender para Key Vault](defender-for-key-vault-usage.md)
- [Alertas de seguridad: una guía de referencia](alerts-reference.md)
- [Introducción a Defender for Cloud](defender-for-cloud-introduction.md)
