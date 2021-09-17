---
title: Copia y traslado de los proyectos de Custom Vision
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar las API ExportProject e ImportProject para copiar y trasladar los proyectos de Custom Vision.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: how-to
ms.date: 09/08/2020
ms.author: pafarley
ms.openlocfilehash: d546726e03420b41b4ff445418e30274904c6e5c
ms.sourcegitcommit: 9f1a35d4b90d159235015200607917913afe2d1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2021
ms.locfileid: "122635063"
---
# <a name="copy-and-move-your-custom-vision-projects"></a>Copia y traslado de los proyectos de Custom Vision

Después de crear y entrenar un proyecto de Custom Vision, puede que quiera copiar el proyecto en otro recurso. Por ejemplo, puede que quiera trasladar un proyecto desde un entorno de desarrollo a un entorno de producción, o realizar una copia de seguridad de un proyecto en una cuenta de otra región de Azure para aumentar la seguridad de los datos.

Las API **[ExportProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddeb3)** y **[ImportProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc7548b571998fddee3)** habilitan este escenario permitiéndole copiar proyectos de una cuenta de Custom Vision en otras. En esta guía se muestra cómo usar las API de REST con cURL. También puede usar un servicio de solicitud HTTP como Postman para emitir las solicitudes.

> [!TIP]
> Para obtener un ejemplo de este escenario mediante la biblioteca cliente de Python, consulte el repositorio [Move Custom Vision Project](https://github.com/Azure-Samples/custom-vision-move-project/tree/master/) en GitHub.

## <a name="business-scenarios"></a>Escenarios empresariales

Si su aplicación o empresa dependen del uso de un proyecto de Custom Vision, se recomienda copiar el modelo en otra cuenta de Custom Vision de otra región. Si se produce una interrupción regional, puede acceder al modelo en la región donde lo copió.

##  <a name="prerequisites"></a>Requisitos previos

- Dos recursos de Azure Custom Vision. Si no los tiene, vaya a Azure Portal y [cree un recurso de Custom Vision](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision?azure-portal=true).
- Las claves de entrenamiento y las direcciones URL del punto de conexión de los recursos de Custom Vision. Puede encontrar estos valores en la pestaña **Información general** del recurso en Azure Portal.
- Un proyecto de Custom Vision ya creado. Consulte [Creación de un clasificador](./getting-started-build-a-classifier.md) para encontrar instrucciones sobre cómo hacerlo.
* [PowerShell versión 6.0+](/powershell/scripting/install/installing-powershell-core-on-windows) o una utilidad similar de la línea de comandos.

## <a name="process-overview"></a>Información general del proceso

El proceso de copia de un proyecto consta de los siguientes pasos:

1. En primer lugar, obtenga el id. del proyecto en la cuenta de origen que quiere copiar.
1. Después, llame a la API **ExportProject** con el id. de proyecto y la clave de entrenamiento de la cuenta de origen. Obtendrá una cadena de token temporal.
1. Después, llame a la API **ImportProject** con la cadena de token y la clave de entrenamiento de la cuenta de destino. El proyecto aparecerá en su cuenta de destino.

## <a name="get-the-project-id"></a>Obtención del id. de proyecto

En primer lugar, llame a **[GetProjects](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddead)** para ver una lista de los proyectos de Custom Vision existentes y sus id. Use la clave de entrenamiento y el punto de conexión de la cuenta de origen.

```curl
curl -v -X GET "{endpoint}/customvision/v3.3/Training/projects"
-H "Training-key: {training key}"
```

Obtendrá una respuesta `200\OK` con una lista de proyectos y sus metadatos en el cuerpo. El valor `"id"` es la cadena que se debe copiar para los pasos siguientes.

```json
[
  {
    "id": "00000000-0000-0000-0000-000000000000",
    "name": "string",
    "description": "string",
    "settings": {
      "domainId": "00000000-0000-0000-0000-000000000000",
      "classificationType": "Multiclass",
      "targetExportPlatforms": [
        "CoreML"
      ],
      "useNegativeSet": true,
      "detectionParameters": "string",
      "imageProcessingSettings": {
        "augmentationMethods": {}
      }
    },
    "created": "string",
    "lastModified": "string",
    "thumbnailUri": "string",
    "drModeEnabled": true,
    "status": "Succeeded"
  }
]
```

## <a name="export-the-project"></a>Exportación del proyecto

Llame a **[ExportProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddeb3)** mediante el id. de proyecto y la clave de entrenamiento y punto de conexión del origen.

```curl
curl -v -X GET "{endpoint}/customvision/v3.3/Training/projects/{projectId}/export"
-H "Training-key: {training key}"
```

Obtendrá una respuesta `200/OK` con metadatos sobre el proyecto exportado y una cadena de referencia `"token"`. Copie el valor del token.

```json
{
  "iterationCount": 0,
  "imageCount": 0,
  "tagCount": 0,
  "regionCount": 0,
  "estimatedImportTimeInMS": 0,
  "token": "string"
}
```

> [!TIP]
> Si recibe un error de "Token no válido" al importar el proyecto, podría deberse a que la cadena de dirección URL del token no está codificada en web. Puede codificar el token mediante un [codificador de direcciones URL](https://meyerweb.com/eric/tools/dencoder/).

## <a name="import-the-project"></a>Importación del proyecto

Llame a **[ImportProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc7548b571998fddee3)** con la clave de entrenamiento y el punto de conexión de destino, junto con el token de referencia. También puede asignar un nombre a su proyecto en su cuenta nueva.

```curl
curl -v -G -X POST "{endpoint}/customvision/v3.3/Training/projects/import"
--data-urlencode "token={token}" --data-urlencode "name={name}"
-H "Training-key: {training key}" -H "Content-Length: 0"
```

Obtendrá una respuesta `200/OK` con metadatos sobre el proyecto recién importado.

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "description": "string",
  "settings": {
    "domainId": "00000000-0000-0000-0000-000000000000",
    "classificationType": "Multiclass",
    "targetExportPlatforms": [
      "CoreML"
    ],
    "useNegativeSet": true,
    "detectionParameters": "string",
    "imageProcessingSettings": {
      "augmentationMethods": {}
    }
  },
  "created": "string",
  "lastModified": "string",
  "thumbnailUri": "string",
  "drModeEnabled": true,
  "status": "Succeeded"
}
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía, aprendió a copiar y trasladar un proyecto entre recursos de Custom Vision. A continuación, consulte los documentos de referencia de la API para ver qué más puede hacer con Custom Vision.
* [Documentación de referencia de API REST](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddeb3)
