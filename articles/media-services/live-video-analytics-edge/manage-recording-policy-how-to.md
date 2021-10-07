---
title: Administración de la directiva de grabación con Live Video Analytics - Azure
description: En este tema se explica cómo administrar la directiva de grabación con Live Video Analytics.
ms.topic: how-to
ms.date: 04/27/2020
ms.openlocfilehash: c9ec5a2efd56a0a2872f946dd23db72d2e7836b9
ms.sourcegitcommit: 613789059b275cfae44f2a983906cca06a8706ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129273654"
---
# <a name="manage-recording-policy-with-live-video-analytics"></a>Administración de la directiva de grabación con Live Video Analytics

[!INCLUDE [redirect to Azure Video Analyzer](./includes/redirect-video-analyzer.md)]

Puede usar Live Video Analytics on IoT Edge para la [grabación continua de vídeo](continuous-video-recording-concept.md), lo que permite grabar vídeo en la nube durante semanas o meses. Además, puede administrar la longitud (en días) de ese archivo de la nube con las [herramientas de administración del ciclo de vida](../../storage/blobs/lifecycle-management-overview.md?tabs=azure-portal) integradas en Azure Storage.  

La cuenta de Media Services está vinculada a una cuenta de Azure Storage y, al grabar vídeo en la nube, el contenido se escribe en un [recurso](../latest/assets-concept.md) de Media Services. Cada recurso se asigna a un contenedor en la cuenta de almacenamiento. La administración del ciclo de vida le permite definir una [directiva](../../storage/blobs/lifecycle-management-overview.md?tabs=azure-portal) para una cuenta de Azure Storage, en la que puede especificar una [regla](../../storage/blobs/lifecycle-management-overview.md?tabs=azure-portal#lifecycle-management-rule-definition) como la siguiente.

```
{
  "rules": [
    {
      "name": "NinetyDayRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "delete": { "daysAfterModificationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

La regla anterior:

* Se aplica a todos los blobs en bloques de la cuenta de Azure Storage.
* Especifica que, cuando la antigüedad de los blobs supera los 30 días, se mueven desde el [nivel de acceso frecuente al de acceso esporádico](../../storage/blobs/access-tiers-overview.md?tabs=azure-portal).
* Indica que, cuando los blobs tengan más de 90 días, se eliminarán.

Al usar Live Video Analytics para grabar en un recurso, especifique una propiedad `segmentLength` que indique al módulo que agregue una duración mínima de vídeo (en segundos) antes de que se escriba en la nube. El recurso contendrá una serie de segmentos, cada uno con una marca de tiempo de creación que es `segmentLength` más reciente que la anterior. Cuando se inicia la directiva de administración del ciclo de vida, elimina los segmentos anteriores al umbral especificado. Sin embargo, seguirá pudiendo acceder y reproducir los segmentos restantes a través de las API de Media Services. Para obtener más información, consulte [Reproducción de grabaciones](playback-recordings-how-to.md). 

## <a name="limitations"></a>Limitaciones

A continuación se indican algunas de las limitaciones conocidas de la administración del ciclo de vida:

* Puede tener como máximo 100 reglas en la directiva, y cada regla puede especificar hasta 10 contenedores. Por lo tanto, si necesita tener distintas directivas de grabación (por ejemplo, un archivo de 3 días para la cámara del aparcamiento, 30 días para la cámara del muelle de carga y 180 días para la cámara de detrás del mostrador), con una cuenta de Media Services puede personalizar reglas para un máximo de 1000 cámaras.
* Las actualizaciones de la directiva de administración del ciclo de vida no son inmediatas. Consulte [esta sección de preguntas más frecuentes](../../storage/blobs/lifecycle-management-overview.md?tabs=azure-portal#faq) para obtener más detalles.
* Si decide aplicar una directiva en la que los blobs se mueven al nivel de acceso esporádico, la reproducción de esa parte del archivo puede verse afectada. Podría ver latencias adicionales o errores esporádicos. Media Services no admite la reproducción de contenido en el nivel de archivo.

## <a name="next-steps"></a>Pasos siguientes

[Reproducción de grabaciones](playback-recordings-how-to.md)