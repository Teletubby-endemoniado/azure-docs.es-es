---
title: 'Transcripción de conversaciones asincrónica: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar la transcripción de conversaciones asincrónica con el servicio de voz. Disponible solo para Java y C#.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-one
ms.openlocfilehash: 9dbee96d6b277ac76b07e3fd8b2b72b1c84b32f9
ms.sourcegitcommit: 05c8e50a5df87707b6c687c6d4a2133dc1af6583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2021
ms.locfileid: "132546412"
---
# <a name="asynchronous-conversation-transcription"></a>Transcripción de conversaciones asincrónica

En este artículo, se muestra la transcripción de conversaciones asincrónica mediante la API **RemoteConversationTranscriptionClient**. Si ha configurado la transcripción de conversaciones para realizar la transcripción asincrónica y tiene un `conversationId`, puede obtener la transcripción asociada a ese `conversationId` mediante la API **RemoteConversationTranscriptionClient**.

## <a name="asynchronous-vs-real-time--asynchronous"></a>Asincrónica frente a la combinación de tiempo real y asincrónica

Con la transcripción asincrónica, se transmite el audio de conversación, pero no es necesario que se devuelva una transcripción en tiempo real. En su lugar, después de enviar el audio, use el `conversationId` de `Conversation` para consultar el estado de la transcripción asincrónica. Cuando la transcripción asincrónica esté lista, obtendrá un `RemoteConversationTranscriptionResult`.

Con una combinación de transcripción en tiempo real y asincrónica, se obtiene la transcripción en tiempo real, pero también se obtiene la transcripción mediante consultas con el `conversationId` (similar a un escenario asincrónico).

Se requieren dos pasos para realizar la transcripción asincrónica. El primer paso consiste en cargar el audio y elegir solo transcripción asincrónica o en tiempo real y asincrónica. El segundo paso es obtener los resultados de la transcripción.

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/remote-conversation/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/remote-conversation/java/examples.md)]
::: zone-end


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos en GitHub](https://aka.ms/csspeech/samples)
