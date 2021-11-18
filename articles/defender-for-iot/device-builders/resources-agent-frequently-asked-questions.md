---
title: Preguntas frecuentes sobre Microsoft Defender para IoT para desarrolladores de dispositivos
description: Encuentre respuestas a las preguntas más frecuentes sobre el agente de Microsoft Defender para IoT.
ms.topic: conceptual
ms.date: 11/09/2021
ms.openlocfilehash: 9075424368a6d3e8c2e17306a573f768cb4a0e45
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132293485"
---
# <a name="microsoft-defender-for-iot-for-device-builders-frequently-asked-questions"></a>Preguntas frecuentes sobre Microsoft Defender para IoT para desarrolladores de dispositivos

En este artículo se proporciona una lista de las preguntas más frecuentes y las respuestas sobre el agente de Defender para IoT.

## <a name="do-i-have-to-install-an-embedded-security-agent"></a>¿Tengo que instalar un agente de seguridad incrustado?

La instalación de un agente en los dispositivos de IoT no es obligatoria para habilitar Defender para IoT. Puede elegir entre las dos opciones siguientes. Hay cuatro niveles diferentes de supervisión de seguridad, y funcionalidades de administración que proporcionarán diferentes niveles de protección:

- Instale el agente incrustado de seguridad de Defender para IoT con o sin modificaciones. Esta opción proporciona el máximo nivel de información de seguridad mejorada relativa al comportamiento y el acceso al dispositivo.

- Sin instalación de ningún agente de seguridad en los dispositivos de IoT. Esta opción permite la supervisión de la comunicación de IoT Hub, con funcionalidades reducidas de supervisión y administración de seguridad.

## <a name="what-does-the-defender-for-iot-agent-do"></a>¿Qué hace el agente de Defender para IoT?

El agente de Defender para IoT proporciona cobertura de amenazas a nivel del dispositivo para la configuración, el comportamiento, el acceso (mediante el examen de la configuración), los procesos y la conectividad de los dispositivos. El agente de seguridad de Defender para IoT no examina los datos ni actividades relacionados con el negocio.

El agente de seguridad de Defender para IoT es de código abierto y está disponible en GitHub para Windows y Linux en versiones de 32 y 64 bits: https://github.com/Azure/Azure-IoT-Security

## <a name="what-are-the-dependencies-and-prerequisites-of-the-agent"></a>¿Cuáles son las dependencias y los requisitos previos del agente?

Defender para IoT admite una amplia variedad de plataformas. Consulte [Plataformas compatibles](how-to-deploy-agent.md) para verificar la compatibilidad para sus dispositivos en concreto.

## <a name="which-data-is-collected-by-the-agent"></a>¿Qué datos recopila el agente?

El agente recopila datos de conectividad, acceso, configuración del firewall, lista de procesos y línea base del SO.

## <a name="how-much-data-will-the-agent-generate"></a>¿Cuántos datos generará el agente?

La generación de datos del agente depende del dispositivo, la aplicación, el tipo de conectividad y la configuración del agente del cliente. Debido a la gran variabilidad entre dispositivos y soluciones de IoT, se recomienda implementar primero el agente en un entorno de laboratorio o prueba para observar, aprender y establecer la configuración específica que se adapte a sus necesidades, a la vez que se mide la cantidad de datos generados. Después de iniciar el servicio, el agente de Defender para IoT proporciona recomendaciones operativas para optimizar el rendimiento del agente, lo que le ayudará en el proceso de configuración y personalización.

## <a name="do-agent-messages-use-up-quota-from-iot-hub"></a>¿Se cuentan los mensajes del agente en la cuota de IoT Hub?

Sí. Los datos transmitidos por el agente se contabilizan en su cuota de IoT Hub.

## <a name="what-next-ive-installed-an-agent-and-dont-see-any-activities-or-logs"></a>¿Qué más? He instalado un agente y no veo ninguna actividad ni registros...

1. Compruebe que el [tipo de agente se ajusta a la plataforma del SO designada de su dispositivo](how-to-deploy-agent.md).

1. Confirme que el [agente se esté ejecutando en el dispositivo](how-to-agent-configuration.md).

1. Compruebe que el [servicio se haya habilitado correctamente](quickstart-onboard-iot-hub.md) en **Seguridad** en IoT Hub.

1. Compruebe que el dispositivo esté [configurado en IoT Hub con el módulo de Defender para IoT](quickstart-create-security-twin.md).

Si las actividades o los registros aún no están disponibles, póngase en contacto con su asociado de Defender para IoT para obtener ayuda adicional.

## <a name="what-happens-when-the-internet-connection-stops-working"></a>¿Qué ocurre cuando la conexión a Internet deja de funcionar?

Los sensores y agentes siguen ejecutándose y almacenan los datos, siempre y cuando el dispositivo esté en ejecución. Los datos se almacenan en la caché de mensajes de seguridad según la configuración de tamaño. Cuando el dispositivo vuelve a tener conectividad, se reanuda el envío de los mensajes de seguridad.

## <a name="can-the-agent-affect-the-performance-of-the-device-or-other-installed-software"></a>¿Puede el agente afectar al rendimiento del dispositivo o de otro software instalado?

El agente consume recursos de la máquina como cualquier otra aplicación o proceso y no debería interrumpir la actividad normal del dispositivo. El consumo de recursos en el dispositivo donde se ejecuta el agente está condicionado por la instalación y la configuración. Se recomienda probar la configuración de agente en un entorno independiente, junto con la interoperabilidad con las otras aplicaciones y funcionalidades de IoT, antes de intentar implementar en un entorno de producción.

## <a name="im-making-some-maintenance-on-the-device-can-i-turn-off-the-agent"></a>Estoy realizando mantenimiento en el dispositivo. ¿Puedo desactivar el agente?

El agente no puede desactivarse.

## <a name="is-there-a-way-to-test-if-the-agent-is-working-correctly"></a>¿Hay alguna manera de probar si el agente funciona correctamente?

Si el agente deja de comunicarse o no envía mensajes de seguridad, se genera una alerta **Device is silent** (El dispositivo está silencioso).

## <a name="can-i-create-my-own-alerts"></a>¿Puedo crear mis propias alertas?

Sí, puede crear alertas personalizadas basadas en varios parámetros, como la dirección IP/MAC, el tipo de protocolo, la clase, el servicio, la función, el comando, etc., así como los valores de las etiquetas personalizadas contenidas en las cargas. Consulte [Creación de alertas personalizadas](quickstart-create-custom-alerts.md) para más información sobre las alertas personalizadas y cómo crearlas.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los primeros pasos con Defender para IoT, consulte los siguientes artículos:

- Lea la [información general](overview.md) de Defender para IoT.
- Información sobre las [alertas de seguridad de Defender para IoT](concept-security-alerts.md)
