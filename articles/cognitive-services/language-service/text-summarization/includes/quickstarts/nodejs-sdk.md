---
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-service
ms.topic: include
ms.date: 11/02/2021
ms.author: aahi
ms.custom: devx-track-js, ignite-fall-2021
ms.openlocfilehash: 6ca78f4e32495628d6597e2f6910e4b306d544df
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131017621"
---
[Documentación de referencia](/javascript/api/overview/azure/ai-text-analytics-readme?preserve-view=true&view=azure-node-preview) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/textanalytics/ai-text-analytics) | [Paquete (NPM)](https://www.npmjs.com/package/@azure/ai-text-analytics/v/5.2.0-beta.1) | [Ejemplos](https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/textanalytics/ai-text-analytics/samples)

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services)
* La versión actual de [Node.js](https://nodejs.org/).
* Una vez que tenga la suscripción de Azure, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Creación de un recurso de idioma"  target="_blank">cree un recurso de idioma</a> en Azure Portal para obtener la clave y el punto de conexión. Una vez que se implemente, haga clic en **Ir al recurso**.
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación a la API. En una sección posterior de este mismo inicio rápido pegará la clave y el punto de conexión en el código siguiente.
    * Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.
* Para usar la característica Analizar, necesitará un recurso de idioma con el plan de tarifa estándar (S).

## <a name="setting-up"></a>Instalación

### <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js

En una ventana de la consola (como cmd, PowerShell o Bash), cree un directorio para la aplicación y vaya a él. 

```console
mkdir myapp 

cd myapp
```

Ejecute el comando `npm init` para crear una aplicación de nodo con un archivo `package.json`. 

```console
npm init
```

### <a name="install-the-client-library"></a>Instalación de la biblioteca cliente

Instale los paquetes NPM:

```console
npm install --save @azure/ai-text-analytics@5.2.0-beta.1
```

## <a name="code-example"></a>Ejemplo de código 

Abra el archivo y copie el código siguiente. No olvide reemplazar la variable `key` por la clave del recurso y la variable `endpoint` por el punto de conexión del recurso. 

[!INCLUDE [find the key and endpoint for a resource](../../../includes/find-azure-resource-info.md)]

```javascript
"use strict";

const { TextAnalyticsClient, AzureKeyCredential } = require("@azure/ai-text-analytics");
const key = '<paste-your-key-here>';
const endpoint = '<paste-your-endpoint-here>';
// Authenticate the client with your key and endpoint
const textAnalyticsClient = new TextAnalyticsClient(endpoint, new AzureKeyCredential(key));

// Example method for summarizing text
async function summarization_example(client) {
    const documents = [`The extractive summarization feature uses natural language processing techniques to locate key sentences in an unstructured text document. 
        These sentences collectively convey the main idea of the document. This feature is provided as an API for developers. 
        They can use it to build intelligent solutions based on the relevant information extracted to support various use cases. 
        In the public preview, extractive summarization supports several languages. It is based on pretrained multilingual transformer models, part of our quest for holistic representations. 
        It draws its strength from transfer learning across monolingual and harness the shared nature of languages to produce models of improved quality and efficiency.`];

    console.log("== Analyze Sample For Extract Summary ==");

    const actions = {
        extractSummaryActions: [{ modelVersion: "latest", orderBy: "Rank", maxSentenceCount: 5 }],
    };
    const poller = await client.beginAnalyzeActions(documents, actions, "en");

    poller.onProgress(() => {
        console.log(
            `Number of actions still in progress: ${poller.getOperationState().actionsInProgressCount}`
        );
    });

    console.log(`The analyze actions operation created on ${poller.getOperationState().createdOn}`);

    console.log(
        `The analyze actions operation results will expire on ${poller.getOperationState().expiresOn}`
    );

    const resultPages = await poller.pollUntilDone();

    for await (const page of resultPages) {
        const extractSummaryAction = page.extractSummaryResults[0];
        if (!extractSummaryAction.error) {
            for (const doc of extractSummaryAction.results) {
                console.log(`- Document ${doc.id}`);
                if (!doc.error) {
                    console.log("\tSummary:");
                    for (const sentence of doc.sentences) {
                        console.log(`\t- ${sentence.text}`);
                    }
                } else {
                    console.error("\tError:", doc.error);
                }
            }
        }
    } 
}
summarization_example(textAnalyticsClient).catch((err) => {
    console.error("The sample encountered an error:", err);
});
```

### <a name="output"></a>Output

```console
== Analyze Sample For Extract Summary ==
The analyze actions operation created on Thu Sep 16 2021 13:12:31 GMT-0700 (Pacific Daylight Time)
The analyze actions operation results will expire on Fri Sep 17 2021 13:12:31 GMT-0700 (Pacific Daylight Time)

- Document 0
        Summary:
        - They can use it to build intelligent solutions based on the relevant information extracted to support various use cases.
        - This feature is provided as an API for developers.
        - The extractive summarization feature uses natural language processing techniques to locate key sentences in an unstructured 
text document.
        - These sentences collectively convey the main idea of the document.
        - In the public preview, extractive summarization supports several languages.
```
