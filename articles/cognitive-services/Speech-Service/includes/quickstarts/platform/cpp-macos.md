---
title: 'Inicio rápido: configuración de la plataforma para usar C++ en macOS con el SDK de Voz: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Use esta guía para configurar la plataforma para usar C++ en macOS con el SDK del servicio de voz.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/15/2020
ms.author: eur
ms.openlocfilehash: 90c86bbe258ebc73ef0bcb4b0da1b0124333ac46
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131502426"
---
En esta guía se muestra cómo instalar el [SDK de Voz](~/articles/cognitive-services/speech-service/speech-sdk.md) para C++ en macOS 10.13 y versiones superiores.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="system-requirements"></a>Requisitos del sistema

macOS 10.13 y versiones superiores

## <a name="install-speech-sdk"></a>Instalación del SDK de Voz

1. Seleccione el directorio al que desea extraer los archivos del SDK de Voz y configure la variable de entorno `SPEECHSDK_ROOT` para que apunte a ese directorio. Esta variable facilita la referencia al directorio en futuros comandos. Por ejemplo, si desea usar el directorio `speechsdk` en el directorio principal, use un comando similar al siguiente:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Cree el directorio si aún no existe.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Descargue y extraiga el archivo `.zip` que contiene el marco del SDK de Voz:

   ```sh
   wget -O SpeechSDK-macOS.zip https://aka.ms/csspeech/macosbinary
   unzip SpeechSDK-macOS.zip -d "$SPEECHSDK_ROOT"
   ```

1. Valide el contenido del directorio de nivel superior del paquete extraído:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   La lista de directorios debe contener los archivos de licencia y aviso de otro fabricante, así como un directorio `MicrosoftCognitiveServicesSpeech.framework`.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [windows](../quickstart-list.md)]
