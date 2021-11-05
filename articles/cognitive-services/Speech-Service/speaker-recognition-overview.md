---
title: 'Introducción a Speaker Recognition: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Speaker Recognition proporciona algoritmos que comprueban e identifican a los hablantes por sus características de voz únicas mediante la biometría de la voz. Speaker Recognition se usa para responder a la pregunta "¿quién está hablando?". En este artículo encontrará información general sobre las ventajas y las funcionalidades del servicio Speaker Recognition.
services: cognitive-services
author: eric-urban
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/02/2020
ms.author: eur
ms.custom: cog-serv-seo-aug-2020, ignite-fall-2021
keywords: speaker recognition, reconocimiento del hablante, biometría de voz
ms.openlocfilehash: 4096e04678c3ce0b10ac54a190f3074525469b54
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131509464"
---
# <a name="what-is-speaker-recognition"></a>¿Qué es Speaker Recognition?

Speaker Recognition se usa para determinar quién habla en un clip de audio. El servicio puede comprobar e identificar a los hablantes por sus características de voz únicas mediante la biometría de voz. Proporciona datos de entrenamiento de audio para un solo hablante, que crea un perfil de inscripción basado en las características únicas de la voz del hablante. Después, puede realizar una comprobación cruzada de los ejemplos de voz de audio con respecto a este perfil para comprobar que el hablante es la misma persona (verificación del hablante) o muestras de voz de audio de una consulta cruzada en un *grupo* de perfiles de altavoz inscritos, para ver si coincide con algún perfil del grupo (identificación del hablante).

> [!IMPORTANT]
> Microsoft limita el acceso a Speaker Recognition. Puede solicitar usarlo mediante la [revisión de acceso limitado para Speaker Recognition de Azure Cognitive Services](https://aka.ms/azure-speaker-recognition). Después de la aprobación, podrá acceder a las API de Speaker Recognition. Para obtener más información, visite [Acceso limitado para Speaker Recognition](/legal/cognitive-services/speech-service/speaker-recognition/limited-access-speaker-recognition).

## <a name="speaker-verification"></a>Verificación del hablante

Speaker Verification simplifica el proceso de verificación de la identidad de un hablante inscrito con frases de contraseña o con una entrada de voz de forma libre. Se puede usar para comprobar que los usuarios tienen compromisos de cliente seguros y sin problemas en una amplia gama de soluciones, desde la verificación de la identidad del cliente en los centros de llamadas hasta el acceso a la instalación sin conexión.

### <a name="how-does-speaker-verification-work"></a>¿Cómo funciona Speaker Verification?

:::image type="content" source="media/speaker-recognition/speaker-rec.png" alt-text="Diagrama de flujo de Speaker Verification.":::

La verificación del hablante puede ser dependiente del texto o independiente del texto. En la verificación **dependiente del texto**, los hablantes deben elegir la misma frase de contraseña para usarla durante las fases de inscripción y comprobación. En la verificación **independiente del texto**, los hablantes pueden hablar en el lenguaje cotidiano en las frases de inscripción y comprobación.

En la **verificación dependiente del texto**, la voz del hablante se inscribe indicando una frase de contraseña de un conjunto de frases predefinidas. Las características de voz se extraen de la grabación de audio para formar una firma de voz única, mientras que también se reconoce la frase de contraseña seleccionada. Juntos, la firma de voz y la frase de contraseña se usan para verificar el hablante. 

La **verificación independiente del texto** no tiene restricciones sobre lo que dice el hablante durante la inscripción, además de la frase de activación inicial para activar la inscripción. No tiene ninguna restricción en la muestra de audio que se va a comprobar, ya que solo extrae características de voz para puntuar la similitud. 

Las API no están pensadas para determinar si el audio proviene de una persona de carne y hueso, una imitación o una grabación de un hablante inscrito. 

## <a name="speaker-identification"></a>Identificación del hablante

Speaker Identification se usa para determinar la identidad de un hablante desconocido dentro de un grupo de hablantes inscritos. Speaker Identification permite atribuir voz a hablantes individuales y aprovechar el valor de escenarios con varios hablantes, como:

* Soluciones de soporte técnico para la productividad en reuniones remotas 
* Desarrollo de personalización de dispositivos multiusuario

### <a name="how-does-speaker-identification-work"></a>¿Cómo funciona Speaker Identification?

La inscripción para la identificación del hablante **no depende del texto**, lo cual significa que no hay restricciones con respecto a lo que el hablante dice en el audio, excepto la frase de activación inicial para activar la inscripción. De forma similar a Speaker Verification, se graba la voz del hablante en la fase de inscripción y se extraen las características de voz para formar una firma de voz única. En la fase de identificación, la muestra de voz de entrada se compara con una lista especificada de voces inscritas (hasta 50 en cada solicitud).

## <a name="data-security-and-privacy"></a>Privacidad y seguridad de los datos

Los datos de inscripción de hablantes se almacenan en un sistema protegido, incluido el audio de voz para las características de inscripción y firma de voz. El audio de voz para la inscripción solo se utiliza cuando se actualiza el algoritmo y las características deben volver a extraerse. El servicio no conserva la grabación de voz ni las características de voz extraídas que se le envían durante la fase de reconocimiento. 

Puede controlar cuánto tiempo se deben conservar los datos. Los clientes pueden crear, actualizar y quitar los datos de inscripción de un hablante mediante las llamadas API. Cuando se elimine la suscripción, todos los datos de inscripción del hablante asociados a la suscripción también se eliminarán. 

Al igual que sucede con todos los recursos de Cognitive Services, los desarrolladores que usan el servicio Speaker Recognition deben estar al tanto de las directivas de Microsoft sobre los datos de los clientes. Debe asegurarse de haber recibido los permisos adecuados de los usuarios para Speaker Recognition. Puede encontrar más detalles en [Datos y privacidad de Speaker Recognition](/legal/cognitive-services/speech-service/speaker-recognition/data-privacy-speaker-recognition). Para obtener más información, vea la  [página de Cognitive Services](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/)  en Microsoft Trust Center. 

## <a name="common-questions-and-solutions"></a>Preguntas comunes y soluciones

| Pregunta | Solución |
|---------|----------|
| ¿Para qué escenarios se puede usar Speaker Recognition? | Verificación del cliente del centro de llamadas, registro de pacientes basado en la voz, transcripción de reuniones y personalización de dispositivos multiusuario.|
| ¿Cuál es la diferencia entre la identificación y la verificación? | La identificación es el proceso de detectar qué miembro de un grupo de hablantes está hablando. La verificación es el acto de confirmar que un hablante coincide con una voz conocida o **inscrita**.|
| ¿Qué diferencia hay entre las comprobaciones dependiente e independiente del texto? | La verificación dependiente del texto requiere una frase de contraseña específica para la inscripción y el reconocimiento. La verificación independiente del texto requiere una muestra de voz más larga que debe comenzar con una frase de activación determinada para la inscripción, pero se puede decir cualquier cosa, incluso durante el reconocimiento.|
| ¿Qué idiomas se admiten? | Inglés, francés, español, chino, alemán, italiano, japonés y portugués. |
| ¿Qué regiones de Azure se admiten? | Speaker Recognition es un servicio en versión preliminar y actualmente solo está disponible en la región Oeste de EE. UU.|
| ¿Qué formatos de audio se admiten? | WAV de 16 bits en mono a 16 kHz con codificación PCM. |
| Las respuestas **Aceptar** y **Rechazar** no son precisas. ¿Cómo se ajusta el umbral? | Dado que el umbral óptimo varía en gran medida con los diferentes escenarios, la API toma una decisión basándose simplemente en un umbral predeterminado de 0,5. Se recomienda a los usuarios avanzados invalidar la decisión predeterminada y ajustar el resultado en función de su propio escenario. |
| ¿Se puede inscribir a un hablante varias veces? | Sí, para la verificación dependiente del texto, puede inscribir a un hablante hasta 50 veces. En el caso de la verificación independiente del texto o la identificación del hablante, puede inscribirse con un máximo de 300 segundos de audio. |
| ¿Qué datos se almacenan en Azure? | El audio de la inscripción se almacenará en el servicio hasta que el perfil de voz se [elimine](./get-started-speaker-recognition.md#deleting-voice-profile-enrollments). Las muestras de audio de reconocimiento no se conservarán ni se almacenarán. |

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> * Complete la lectura del [artículo sobre conceptos básicos](./get-started-speaker-recognition.md) de Speaker Recognition para realizar una revisión de los modelos de diseño comunes que puede usar en sus aplicaciones.
> * Consulte el [tutorial de vídeo](https://azure.microsoft.com/resources/videos/speaker-recognition-text-independent-verification-developer-tutorial/) para la verificación del hablante independiente del texto.

## <a name="responsible-use-of-ai"></a>Uso responsable de la inteligencia artificial
> [!div class="ResponsibleUseOfAI"]
> * [Nota de transparencia para Speaker Recognition](/legal/cognitive-services/speech-service/speaker-recognition/transparency-note-speaker-recognition)
> * [Características y limitaciones de Speaker Recognition](/legal/cognitive-services/speech-service/speaker-recognition/characteristics-and-limitations-speaker-recognition)
> * [Acceso limitado para Speaker Recognition](/legal/cognitive-services/speech-service/speaker-recognition/limited-access-speaker-recognition)
> * [Instrucciones para la integración y el uso responsable con Speaker Recognition](/legal/cognitive-services/speech-service/speaker-recognition/guidance-integration-responsible-use-speaker-recognition)
> * [Datos y privacidad de Speaker Recognition](/legal/cognitive-services/speech-service/speaker-recognition/data-privacy-speaker-recognition)
