---
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-service
ms.topic: include
ms.date: 11/02/2021
ms.author: aahi
ms.custom: language-service-pii, ignite-fall-2021
ms.openlocfilehash: 1b1e6d4a861d5ccd42f2649ce819a15c64f0ebd9
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131093353"
---
[Documentación de referencia](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/)


## <a name="prerequisites"></a>Prerrequisitos

* La versión actual de [cURL](https://curl.haxx.se/).
* Una vez que tenga la suscripción de Azure, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Creación de un recurso de idioma"  target="_blank">cree un recurso de idioma</a> en Azure Portal para obtener la clave y el punto de conexión. Una vez que se implemente, haga clic en **Ir al recurso**.
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación a la API. En una sección posterior de este mismo inicio rápido pegará la clave y el punto de conexión en el código siguiente.
    * Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.

> [!NOTE]
> * En los siguientes ejemplos de BASH se usa el carácter de continuación de línea `\`. Si la consola o el terminal usan un carácter de continuación de línea diferente, use ese carácter.
> * Puede encontrar ejemplos específicos del lenguaje en [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code).
> * Vaya a Azure Portal y busque la clave y el punto de conexión del recurso del idioma que creó en los requisitos previos. Los encontrará en la página de **clave y punto de conexión** del recurso, en **Administración de recursos**. A continuación, reemplace las cadenas del código siguiente por su clave y punto de conexión.
Para llamar a la API, necesita la siguiente información:


|parámetro  |Descripción  |
|---------|---------|
|`-X POST <endpoint>`     | Especifica el punto de conexión para acceder a la API.        |
|`-H Content-Type: application/json`     | Tipo de contenido para enviar datos JSON.          |
|`-H "Ocp-Apim-Subscription-Key:<key>`    | Especifica la clave para acceder a la API.        |
|`-d <documents>`     | JSON que contiene los documentos que desea enviar.         |

Los siguientes comandos de cURL se ejecutan desde un shell de BASH. Edite estos comandos con sus propios valores de nombre de recurso, clave de recurso y JSON.

## <a name="personally-identifying-information-pii-detection"></a>Detección de información de identificación personal

[!INCLUDE [REST API quickstart instructions](../../../includes/rest-api-instructions.md)]

```bash
curl -X POST https://your-text-analytics-endpoint-here>/text/analytics/v3.2-preview.1/entities/recognition/pii \
-H "Content-Type: application/json" \
-H "Ocp-Apim-Subscription-Key: <your-text-analytics-key-here>" \
-d '{ documents: [{ id: "1", language:"en", text: "Call our office at 312-555-1234, or send an email to support@contoso.com"}]}'
```

### <a name="json-response"></a>Respuesta JSON

```json
{
   "documents":[
      {
         "redactedText":"Call our office at ************, or send an email to *******************",
         "id":"1",
         "entities":[
            {
               "text":"312-555-1234",
               "category":"PhoneNumber",
               "offset":19,
               "length":12,
               "confidenceScore":0.8
            },
            {
               "text":"support@contoso.com",
               "category":"Email",
               "offset":53,
               "length":19,
               "confidenceScore":0.8
            }
         ],
         "warnings":[
            
         ]
      }
   ],
   "errors":[
      
   ],
   "modelVersion":"2021-01-15"
}
```
