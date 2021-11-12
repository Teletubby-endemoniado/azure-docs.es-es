---
title: Explicación conceptual de los aspectos básicos del microagente de Defender para IoT para Azure RTOS
description: Conozca los aspectos básicos de los conceptos y el flujo de trabajo del microagente de Defender para IoT para Azure RTOS.
ms.topic: conceptual
ms.date: 11/09/2021
ms.openlocfilehash: 084c7130c9fe033fb96fcb0a2e3c2820ef2c77e5
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132278956"
---
# <a name="defender-iot-micro-agent-for-azure-rtos"></a>Defender-IoT-micro-agent para Azure RTOS

Use este artículo para comprender mejor el microagente de Defender para IoT para Azure RTOS, incluidas las características y las ventajas, así como vínculos a recursos de referencia y configuración pertinentes. 

## <a name="azure-rtos-iot-defender-iot-micro-agent"></a>Microagente de Defender para IoT para Azure RTOS

El microagente de Defender para IoT para Azure RTOS proporciona una solución de seguridad completa para los dispositivos Azure RTOS como parte de la oferta de NetX Duo. Dentro de la oferta de NetX Duo, Azure RTOS se incluye con el microagente de Defender para IoT de Azure IoT integrado y proporciona cobertura para las amenazas comunes en los dispositivos del sistema operativo en tiempo real una vez activado.

El microagente de Defender para IoT para Azure RTOS se ejecuta en segundo plano y proporciona una experiencia de usuario fluida, al tiempo que envía mensajes de seguridad mediante las conexiones únicas de cada cliente a su IoT Hub. El microagente de Defender para IoT para Azure RTOS está habilitado de forma predeterminada.  

## <a name="azure-rtos-netx-duo"></a>Azure RTO NetX Duo

Azure RTOS NetX Duo es una pila de red TCP/IP avanzada y de nivel industrial diseñada específicamente para aplicaciones de IoT y en tiempo real insertadas. Azure RTOS NetX Duo es una pila de red IPv4 e IPv6 dual que proporciona un amplio conjunto de protocolos, como la seguridad y la nube. Obtenga más información sobre las soluciones de [Azure RTOS NetX Duo](/azure/rtos/netx-duo/).

El módulo ofrece las características siguientes:

- **Detección de actividades de red malintencionadas**
- **Líneas de base del comportamiento de los dispositivos basadas en alertas personalizadas**
- **Mejora de la higiene de seguridad de los dispositivos**

## <a name="defender-iot-micro-agent-for-azure-rtos-architecture"></a>Arquitectura del microagente de Defender para IoT para Azure RTOS

El microagente de Defender para IoT para Azure RTOS se inicializa con la plataforma de middleware de Azure IoT y usa clientes de IoT Hub para enviar telemetría de seguridad al concentrador.

:::image type="content" source="media/architecture/security-module-state-diagram.png" alt-text="Diagrama y flujo de información del estado del microagente de Defender para IoT de Azure IoT":::

El microagente de Defender para IoT de Azure RTOS supervisa la siguiente información y actividad del dispositivo mediante tres recopiladores:
- Actividad de red de dispositivo **TCP**, **UDP** e **ICM**
- Información del sistema como versiones de **ThreadX** y **NetX Duo**.
- Eventos de latido

Cada recopilador está vinculado a un grupo de prioridades y cada grupo de prioridades tiene su propio intervalo con valores posibles de **Bajo**, **Medio** y **Alto**. Los intervalos afectan al intervalo de tiempo en el que se recopilan y envían los datos.

Cada intervalo de tiempo se puede configurar y los conectores de IoT se pueden habilitar y deshabilitar con el fin de [personalizar la solución](how-to-azure-rtos-security-module.md). 

## <a name="supported-security-alerts-and-recommendations"></a>Alertas y recomendaciones de seguridad admitidas

El microagente de Defender para IoT para Azure RTOS admite alertas y recomendaciones de seguridad específicas. Asegúrese de [revisar y personalizar los valores de alerta y recomendación pertinentes](concept-rtos-security-alerts-recommendations.md) para el servicio después de completar la configuración inicial.

## <a name="ready-to-begin"></a>¿Está listo para comenzar?

El microagente de Defender para IoT para Azure RTOS se proporciona como descarga gratuita para los dispositivos de IoT. El servicio en la nube Defender para IoT está disponible con una versión de prueba de 30 días por suscripción de Azure. [Descargue el microagente de Defender para IoT ahora](https://github.com/azure-rtos/azure-iot-preview/releases) y comencemos. 

## <a name="next-steps"></a>Pasos siguientes

- Empiece con [los requisitos previos y el programa de instalación](quickstart-azure-rtos-security-module.md) del microagente de Defender para IoT para Azure RTOS.
- Obtenga más información sobre [la compatibilidad con alertas y recomendaciones de seguridad](concept-rtos-security-alerts-recommendations.md) del microagente de Defender para IoT para Azure RTOS. 
- Use la [API de referencia](azure-rtos-security-module-api.md) para el microagente de Defender para IoT para Azure RTOS.
