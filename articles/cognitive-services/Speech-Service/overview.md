---
title: ¿Qué es Speech Service?
titleSuffix: Azure Cognitive Services
description: El Servicio de voz es la unificación de las funcionalidades de conversión de voz a texto, conversión de texto a voz y traducción de voz en una sola suscripción de Azure. Es fácil agregar voz a sus aplicaciones, herramientas y dispositivos con el SDK de Voz, el SDK de dispositivos de voz o las API de REST.
services: cognitive-services
author: eric-urban
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 11/23/2020
ms.author: eur
ms.openlocfilehash: ece5aa9a17f5f65b96cf85de96a8ce655e9c9ec9
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132485601"
---
# <a name="what-is-the-speech-service"></a>¿Qué es Speech Service?

El servicio de voz es la unificación de las funcionalidades de conversión de voz a texto, conversión de texto a voz y traducción de voz en una sola suscripción de Azure. Con la [CLI de Voz](spx-overview.md), el [SDK de voz](./speech-sdk.md), el [SDK de dispositivos de voz](./speech-devices-sdk-quickstart.md?pivots=platform-android), [Speech Studio](speech-studio-overview.md) o las [API REST](#reference-docs) es fácil habilitar aplicaciones, herramientas y dispositivos para el uso de la voz.

> [!IMPORTANT]
> El servicio de voz ha reemplazado a Bing Speech API y Translator Speech. Consulte la sección de _migración_ para obtener instrucciones sobre migración.

Las siguientes características forman parte del servicio Speech. Use los vínculos de esta tabla para obtener más información sobre los casos de uso comunes de cada característica o para examinar la referencia de API.

| Servicio | Característica | Descripción | SDK | REST |
|---------|---------|-------------|-----|------|
| [Voz a texto](speech-to-text.md) | Conversión de voz en texto en tiempo real | La conversión de voz en texto transcribe o traduce en tiempo real secuencias de audio o archivos de audio a texto que sus aplicaciones, herramientas o dispositivos pueden consumir o mostrar. Use voz a texto con [Language Understanding (LUIS)](../luis/index.yml) para derivar las intenciones del usuario a partir de voz transcrita y actuar en los comandos de voz. | [Sí](./speech-sdk.md) | [Sí](#reference-docs) |
| | [Conversión de voz en texto por lotes](batch-transcription.md) | La conversión de voz a texto por lotes permite la transcripción asincrónica de voz en texto de grandes volúmenes de datos de audio de voz almacenados en Azure Blob Storage. Además de convertir el audio de la voz en texto, la conversión de voz en texto por lotes también permite la diarización y el análisis de opiniones. | No | [Sí](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) |
| | [Conversación entre varios dispositivos](multi-device-conversation.md) | Conexión de varios dispositivos o clientes en una conversación para enviar mensajes basados en voz o texto, con compatibilidad sencilla con transcripción y traducción| Sí | No |
| | [Transcripción de conversaciones](./conversation-transcription.md) | Permite el reconocimiento de voz en tiempo real, la identificación del hablante y la diarización. Es perfecto para transcribir reuniones en persona con la capacidad de distinguir a los oradores. | Sí | No |
| | [Creación de modelos de Habla personalizada](#customize-your-speech-experience) | Si usa voz a texto para el reconocimiento y la transcripción en un entorno único, puede crear y entrenar modelos acústicos, de lenguaje y pronunciación personalizados para dirigir el sonido ambiental o vocabulario específico del sector. | No | [Sí](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) |
| | [Valoración de la pronunciación](./how-to-pronunciation-assessment.md) | La evaluación de la pronunciación evalúa la pronunciación de la voz y ofrece a los oradores información sobre la precisión y la fluidez del audio hablado. Con la evaluación de la pronunciación, los estudiantes de idiomas pueden practicar, obtener comentarios instantáneos y mejorar su pronunciación para poder hablar y realizar presentaciones con confianza. | [Sí](./how-to-pronunciation-assessment.md) | [Sí](./rest-speech-to-text.md#pronunciation-assessment-parameters) |
| [Texto a voz](text-to-speech.md) | Texto a voz | Texto a voz convierte el texto de entrada en voz sintetizada similar a la humana mediante el [Lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md). Use voces neuronales, que son voces similares a las humanas con tecnología de redes neuronales profundas. Consulte el artículo sobre la [compatibilidad con los distintos idiomas](language-support.md). | [Sí](./speech-sdk.md) | [Sí](#reference-docs) |
| | [Creación de voces personalizadas](#customize-your-speech-experience) | Cree fuentes de voz personalizadas únicas para su marca o producto. | No | [Sí](#reference-docs) |
| [Traducción de voz](speech-translation.md) | Traducción de voz | La traducción de voz habilita la traducción de voz en varios idiomas en tiempo real en sus aplicaciones, herramientas y dispositivos. Use este servicio para la traducción de voz a voz y voz a texto. | [Sí](./speech-sdk.md) | No |
| [Asistentes de voz](voice-assistants.md) | Asistentes de voz | Los asistentes de voz que utilizan el Servicio de voz permiten a los desarrolladores crear interfaces de conversación naturales, similares a la humana, para sus aplicaciones y experiencias. El servicio del asistente de voz proporciona una interacción rápida y confiable entre un dispositivo y una implementación de asistente que usa el canal de voz de Direct Line Speech de Bot Framework o el servicio integrado de comandos personalizados para la finalización de tareas. | [Sí](voice-assistants.md) | No |
| [Speaker Recognition](speaker-recognition-overview.md) | Verificación e identificación del hablante | El servicio Speaker Recognition proporciona algoritmos que comprueban e identifican a los hablantes por sus características de voz únicas. Speaker Recognition se usa para responder a la pregunta "¿quién está hablando?". | Sí | [Sí](/rest/api/speakerrecognition/) |

## <a name="try-the-speech-service-for-free"></a>Prueba gratuita del servicio Voz

En los pasos siguientes, necesitará un cuenta de Microsoft y una cuenta de Azure. Si no tiene un cuenta de Microsoft, puede registrarse para obtener una gratuita en el [portal de la cuenta de Microsoft](https://account.microsoft.com/account). Seleccione **Iniciar sesión con Microsoft** y, luego, cuando se le pida que inicie sesión, seleccione **Crear una cuenta de Microsoft**. Siga los pasos para crear y comprobar la nueva cuenta Microsoft.

Cuando tenga la cuenta de Microsoft, vaya a la [página de suscripción a Azure](https://azure.microsoft.com/free/ai/), seleccione **Comenzar gratis** y cree una cuenta de Azure con su cuenta de Microsoft. Este es un vídeo de [cómo registrarse para obtener una cuenta gratuita de Azure](https://www.youtube.com/watch?v=GWT2R1C_uUU).

> [!NOTE]
> Si se registra para obtener una cuenta gratuita de Azure, recibirá 200 USD en crédito de servicios que puede aplicar a una suscripción del servicio de voz de pago, válida durante 30 días. Los servicios de Azure se deshabilitan cuando el crédito se agota o expira al terminar los 30 días. Para seguir usando los servicios de Azure, debe actualizar la cuenta. Para más información, consulte [Actualización de una cuenta gratuita de Azure](../../cost-management-billing/manage/upgrade-azure-subscription.md). 
>
> El servicio de voz tiene dos niveles de servicio: gratis (f0) y suscripción (s0), que tienen diferentes limitaciones y ventajas. Si usa el nivel gratis del servicio Voz de bajo volumen, puede conservar esta suscripción gratuita incluso después de que expire la evaluación gratuita o el crédito del servicio. Para más información, consulte [Precios de Cognitive Services: servicio de voz](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="create-the-azure-resource"></a>Creación del recurso de Azure

Para agregar un recurso de servicio de voz (plan gratuito o de pago) a la cuenta de Azure:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con la cuenta Microsoft.

1. Seleccione **Crear un recurso** en la parte superior izquierda del portal. Si no ve **Crear un recurso**, siempre puede encontrarlo al seleccionar el menú contraído en la parte superior izquierda.

1. En la ventana **Nuevo**, escriba "speech" en el cuadro de búsqueda y presione ENTRAR.

1. En los resultados de la búsqueda, seleccione **Voz**.
   
   :::image type="content" source="media/index/speech-search.png" alt-text="Crear un recurso de Voz en Azure Portal.":::

1. Seleccione **Crear** y, después:

   - Dé un nombre único al nuevo recurso. El nombre ayuda a distinguir entre varias suscripciones vinculadas al mismo servicio.
   - Elija la suscripción de Azure a la que esté asociado el recurso nuevo para determinar cómo se facturan las tarifas. Esta es la introducción a [cómo crear una suscripción de Azure](../../cost-management-billing/manage/create-subscription.md#create-a-subscription-in-the-azure-portal) en Azure Portal.
   - Elija la [región](regions.md) donde se va a usar el recurso. Azure es una plataforma de nube global que está disponible con carácter general en muchas regiones de todo el mundo. Para obtener el mejor rendimiento, seleccione la región más cercana a usted o donde se ejecuta la aplicación. La disponibilidad del servicio de voz varía de una región a otra. Asegúrese de crear el recurso en una región admitida. Consulte [Regiones admitidas en los servicios de voz](./regions.md#speech-to-text-text-to-speech-and-translation).
   - Elija un plan de tarifa de pago (S0) o gratis (F0). Puede encontrar información completa sobre los precios y las cuotas de uso de cada plan en **Ver todos los detalles de los precios** o [Precios de Speech Services](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/). Para conocer los límites de los recursos, consulte [Límites de Azure Cognitive Services](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-cognitive-services-limits).
   - Cree un nuevo grupo de recursos para esta suscripción de voz o asígnela a un grupo de recursos existente. Los grupos de recursos ayudan a mantener organizadas las distintas suscripciones de Azure.
   - Seleccione **Crear**. Esto le llevará a la información general de la implementación y mostrará mensajes del progreso de la implementación.  
<!--
> [!NOTE]
> You can create an unlimited number of standard-tier subscriptions in one or multiple regions. However, you can create only one free-tier subscription. Model deployments on the free tier that remain unused for 7 days will be decommissioned automatically.
-->
La implementación del recurso de voz nuevo puede tardar unos instantes. 

### <a name="find-keys-and-locationregion"></a>Búsqueda de claves y ubicación o región

Para buscar las claves y la ubicación o región de una implementación completa, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con la cuenta Microsoft.

2. Seleccione **Todos los recursos** y el nombre del recurso de Cognitive Services.

3. En el panel izquierdo, en **ADMINISTRACIÓN DE RECURSOS**, seleccione **Claves y punto de conexión**.

Cada suscripción tiene dos claves; puede usar cualquiera de ellas en la aplicación. Para copiar y pegar una clave en el editor de código o en otra ubicación, seleccione el botón Copiar que se encuentra junto a cada clave y cambie de ventana para pegar el contenido del portapapeles en la ubicación deseada.

Además, copie el valor de `LOCATION`, que es el identificador de región (por ejemplo, `westus`, `westeurope`) para las llamadas de SDK.

> [!IMPORTANT]
> Estas claves de suscripción se usan para tener acceso a la API de Cognitive Services. No comparta las claves. Almacénelas de forma segura, por ejemplo, con Azure Key Vault. También se recomienda regenerar estas claves periódicamente. Solo se necesita una clave para realizar una llamada API. Al volver a generar la primera clave, puede usar la segunda clave para seguir teniendo acceso al servicio.

## <a name="complete-a-quickstart"></a>Completar una guía de inicio rápido

Ofrecemos guías de inicio rápido en los lenguajes de programación más populares, cuyo diseño individual le permite ejecutar el código en menos de 10 minutos. Consulte la siguiente lista para obtener la guía de inicio rápido de cada característica.

* [Inicio rápido de voz a texto](get-started-speech-to-text.md)
* [Inicio rápido para la conversión de texto a voz](get-started-text-to-speech.md)
* [Inicio rápido sobre la traducción de voz](./get-started-speech-translation.md)
* [Inicio rápido sobre el reconocimiento de intención](./get-started-intent-recognition.md)
* [Inicio rápido spbre el reconocimiento de los altavoces](./get-started-speaker-recognition.md)

Una vez que haya tenido la oportunidad de usar el servicio de voz, pruebe nuestros tutoriales, que le enseñarán a resolver distintos escenarios.

- [Tutorial: Reconocimiento de intenciones a partir de contenido de voz con el SDK de voz y LUIS, C#](how-to-recognize-intents-from-speech-csharp.md)
- [Tutorial: Habilitación del bot con voz mediante el SDK de voz, C#](tutorial-voice-enable-your-bot-speech-sdk.md)
- [Tutorial: Compilación de una aplicación de Flask para traducir texto, analizar opiniones y sintetizar la voz de texto a voz, REST](../translator/tutorial-build-flask-app-translation-synthesis.md?bc=%2fazure%2fcognitive-services%2fspeech-service%2fbreadcrumb%2ftoc.json%252c%2fen-us%2fazure%2fbread%2ftoc.json&toc=%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json%252c%2fen-us%2fazure%2fcognitive-services%2fspeech-service%2ftoc.json)

## <a name="get-sample-code"></a>Obtención de código de ejemplo

Hay código de ejemplo para el Servicio de voz disponible en GitHub. En estos ejemplos se tratan escenarios comunes como la lectura de audio de un archivo o flujo, el reconocimiento continuo y al inicio, y el trabajo con modelos personalizados. Use estos vínculos para ver ejemplos de SDK y REST:

- [Ejemplos de conversión de voz a texto, texto a voz y traducción de voz (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Ejemplos de transcripción de Azure Batch (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Ejemplos de texto a voz (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
- [Ejemplos del asistente de voz (SDK)](https://aka.ms/csspeech/samples)

## <a name="customize-your-speech-experience"></a>Personalización de su experiencia de voz

El Servicio de voz funciona bien con los modelos integrados; sin embargo, es posible que desee personalizar y optimizar más la experiencia para su producto o entorno. Las opciones de personalización abarcan desde la optimización de modelos acústicos a fuentes de voz únicas para su marca.

Otros productos ofrecen modelos de voz adaptados para fines específicos, como la atención sanitaria o el seguro, pero están disponibles para todo el mundo. La personalización de Azure Speech forma parte de su ventaja competitiva *única* que no está disponible para ningún otro usuario o cliente. En otras palabras, los modelos son privados y se ajustan de forma personalizada solo para su caso de uso.

| Speech Service | Plataforma | Descripción |
| -------------- | -------- | ----------- |
| Voz a texto | [Habla personalizada](./custom-speech-overview.md) | El reconocimiento de voz personalizado se adapta a sus necesidades y datos disponibles. Elimine las barreras del reconocimiento de voz, como el estilo de habla, el vocabulario y el ruido de fondo. |
| Text-to-Speech | [Voz personalizada](https://aka.ms/customvoice) | Cree una voz reconocible única para las aplicaciones de texto a voz con los datos de habla disponibles. Puede optimizar aún más las salidas de voz ajustando un conjunto de parámetros de voz. |

## <a name="deploy-on-premises-using-docker-containers"></a>Implementación local mediante contenedores de Docker

[Use los contenedores del servicio de voz](speech-container-howto.md) para implementar características de API de forma local. Estos contenedores de Docker permiten acercar el servicio a los datos para mejorar el cumplimiento, la seguridad o por otras razones operativas. El servicio de voz ofrece los siguientes contenedores:

* Conversión de voz en texto estándar
* Conversión de voz a texto personalizada
* Conversión de texto a voz estándar
* Texto a voz neuronal
* Conversión de texto a voz personalizada (versión preliminar)
* Identificación del idioma de Voz (versión preliminar)

## <a name="reference-docs"></a>Documentos de referencia

- [Acerca del SDK de Voz](./speech-sdk.md)
- [Speech Devices SDK](speech-devices-sdk.md)
- [API REST: Speech-to-text](rest-speech-to-text.md) (API de REST: Voz a texto)
- [API REST: Text-to-speech](rest-text-to-speech.md) (API de REST: Texto a voz)
- [API REST: Batch transcription and customization](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) (API de REST: Transcripción y personalización de Azure Batch)


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción a la conversión de voz en texto](./get-started-speech-to-text.md)
> [Introducción a la conversión de texto en voz](get-started-text-to-speech.md)
