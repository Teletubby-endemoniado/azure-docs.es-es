---
title: 'Esquema de metadatos de inferencia: Azure'
description: En Azure Video Analyzer, cada objeto de inferencia, independientemente de que se use el contrato basado en HTTP o el contrato basado en gRPC, sigue el modelo de objetos que se describe en este tema.
ms.topic: reference
ms.date: 06/01/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: 9d22e4b05a19b7d0c120f02f4a97017865563314
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131091054"
---
# <a name="inference-metadata-schema"></a>Esquema de metadatos de inferencia 

[!INCLUDE [header](includes/edge-env.md)]

En Azure Video Analyzer, cada objeto de inferencia, independientemente de que se use el contrato basado en HTTP o el contrato basado en gRPC, sigue el modelo de objetos que se describe en este tema.

## <a name="object-model"></a>Modelo de objetos

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/inference-metadata-schema/object-model.png" alt-text="Modelo de objetos de Azure Video Analyzer" lightbox="./media/inference-metadata-schema/object-model.png":::
 

|Definición de tipo|Descripción|
|---|---|
|Etiqueta|Etiqueta asociada al resultado. Junto con el etiquetado, también puede obtener el valor de confianza asociado a la etiqueta.|
|Atributo|Atributos adicionales asociados al resultado. Puede agregar nuevos atributos que reciba del motor de inferencia junto con el valor de confianza.|
|Lista de atributos|Lista de atributos opcionales.|
|Rectángulo|Región rectangular relativa a la esquina superior izquierda de la imagen. Las propiedades obligatorias serán `left`, `width`, `height` y `top` desde el origen. <br/>Los valores de izquierda, superior, ancho y alto del cuadro de límite se normalizan en una escala de 0,0 a 1,0. `left` (coordenada X) y `top` (coordenada Y) son coordenadas que representan los lados izquierdo y superior del cuadro de límite. Los valores `left` y `top` devueltos son relaciones de las dimensiones de vídeo generales. Por ejemplo, si las dimensiones de vídeo son de 1920×1080 píxeles, y la coordenada superior izquierda del cuadro de límite es (350,50), el cuadro de límite tiene un valor `left` de 0,1822 (350/1920) y un valor `top` de 0,0462 (50/1080). <br/>Los valores `width` y `height` representan las dimensiones del cuadro de límite como relación de la dimensión de vídeo general. Para un vídeo con dimensiones de 1920×1080, si el ancho del cuadro de límite es de 70 píxeles, el ancho devuelto es 0,0364 (70/1920).|
|clasificación|La etiqueta del clasificador suele ser aplicable a todo el ejemplo. Con la ayuda del atributo "tag" (Etiqueta), puede clasificar el resultado.|
|Entidad|Entidad detectada o identificada en el ejemplo. Cuando el motor de inferencia detecta una entidad, obtiene una "etiqueta" y se devuelven los atributos adicionales que se infirieron y las coordenadas de un cuadro rectangular alrededor de la entidad encontrada.|
|Evento|Evento detectado en el ejemplo. Cuando se detecta un evento en la muestra, se devuelven el nombre del evento y las propiedades específicas del mismo.|
|Movimiento|Movimiento detectado en el ejemplo. Cuando se detecta movimiento en la muestra, se devuelven las coordenadas de un cuadro de límite rectangular en el que se ha detectado movimiento.|
|Texto|Se devuelve el texto asociado al ejemplo junto con la marca de tiempo de inicio y finalización del texto.|
|Otros|Devuelve otra información de carga genérica.|

El ejemplo siguiente contiene un solo evento con algunos de los tipos de inferencia admitidos:

```
[ 
  // Light Detection 
  { 
    "type": "classification", 
    "subtype": "lightDetection", 
    "classification": { 
      "tag": { "value": "daylight", "confidence": 0.86 }, 
      "attributes": [ 
          { "name": "isBlackAndWhite", "value": "false", "confidence": 0.71 } 
      ] 
    } 
  }, 
 
  // Motion Detection 
  { 
    "type": "motion", 
    "subtype": "motionDetection", 
    "motion": 
    { 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // Yolo V3 
  { 
    "type": "entity", 
    "subtype": "objectDetection",     
    "entity": 
    { 
      "tag": { "value": "dog", "confidence": 0.97 }, 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // Vehicle Identification 
  { 
    "type": "entity", 
    "subtype": "vehicleIdentification",     
    "entity": 
    { 
      "tag": { "value": "007-SPY", "confidence": 0.82 }, 
      "attributes":[   
        { "name": "color", "value": "black", "confidence": 0.90 }, 
        { "name": "body", "value": "coupe", "confidence": 0.87 }, 
        { "name": "make", "value": "Aston Martin", "confidence": 0.35 }, 
        { "name": "model", "value": "DBS V12", "confidence": 0.33 } 
      ], 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // People Identification 
  { 
    "type": "entity", 
    "subtype": "peopleIdentification",     
    "entity": 
    { 
      "tag": { "value":"Erwin Schrödinger", "confidence": 0.50 }, 
      "attributes":[   
        { "name": "age", "value": "73", "confidence": 0.87 }, 
        { "name": "glasses", "value": "yes", "confidence": 0.94 } 
      ], 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    }, 
 
    // Open type coming from the gRPC Map 
    "extensions":  
    { 
      "vector": "e1xkaXNwbGF5c3R5bGUgaVxoYmFyIHtcZnJhYyB7ZH17ZHR9fVx2ZXJ0IFxQc2kgKHQpXHJhbmdsZSA9e1xoYXQge0h9fVx2ZXJ0IFxQc2kgKHQpXHJhbmdsZSB9KQ==", 
      "skeleton": "p1,p2,p3,p4" 
    } 
  }, 
 
  // Captions 
  {     
    "type": "text", 
    "subtype": "speechToText",   
    "text": 
    { 
      "value": "Humor 75%. Confirmed. Self-destruct sequence in T minus 10… 9…", 
      "language": "en-US", 
      "startTimestamp": 12000, 
      "endTimestamp": 13000 
    } 
]
```

## <a name="next-steps"></a>Pasos siguientes

- [Contrato de datos de gRPC](./grpc-extension-protocol.md)
- [Contrato de datos de HTTP](./http-extension-protocol.md)
