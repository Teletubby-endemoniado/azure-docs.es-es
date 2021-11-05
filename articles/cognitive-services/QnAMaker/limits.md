---
title: 'Límites: QnA Maker'
description: QnA Maker tiene meta-límites para partes de la base de conocimiento y el servicio. Es importante mantener la base de conocimiento dentro de esos límites para probar y publicar.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 11/02/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: ac7741a6404f47e3cabdd9eff81f81c6ad39de36
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131011986"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Límites de la base de conocimiento de QnA Maker

Los límites de QnA Maker que se proporcionan a continuación son una combinación de los [límites del plan de tarifa de Azure Cognitive Search](../../search/search-limits-quotas-capacity.md) y los [límites del plan de tarifa de QnA Maker](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/). Debe conocer ambos conjuntos de límites para comprender el número de bases de conocimiento que puede crear por recurso y el tamaño que puede alcanzar cada base de conocimiento.

## <a name="knowledge-bases"></a>Bases de conocimiento

El número máximo de bases de conocimiento se basa en los [límites de plan de Azure Cognitive Search](../../search/search-limits-quotas-capacity.md).

|**Nivel de Azure Cognitive Search** | **Gratis** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Número máximo permitido de bases de conocimiento publicadas|2|14|49|199|199|2999|

 Por ejemplo, si el nivel tiene 15 índices permitidos, puede publicar 14 bases de conocimiento (un índice por cada base de conocimiento publicada). El decimoquinto índice, `testkb`, se utiliza para todas las bases de conocimiento en lo relativo a autoría y prueba.

## <a name="extraction-limits"></a>Límites de extracción

### <a name="file-naming-constraints"></a>Restricciones de nomenclatura de los archivos

Los nombres de archivo no pueden incluir los siguientes caracteres:

|No use el carácter|
|--|
|Comillas simples `'`|
|Comillas dobles `"`|

### <a name="maximum-file-size"></a>Tamaño de archivo máximo

|Formato|Tamaño máximo de archivo (MB)|
|--|--|
|`.docx`|10|
|`.pdf`|25|
|`.tsv`|10|
|`.txt`|10|
|`.xlsx`|3|

### <a name="maximum-number-of-files"></a>Número máximo de archivos

El número máximo de archivos que se pueden extraer y tamaño máximo del archivo se basa en los **[límites del plan de tarifa de QnA Maker](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)** .

### <a name="maximum-number-of-deep-links-from-url"></a>Número máximo de vínculos profundos de la dirección URL

El número máximo de vínculos profundos que se pueden rastrear para la extracción de QnA desde una páginas URL es **20**.

## <a name="metadata-limits"></a>Límites de metadatos

Los metadatos se presentan como un par clave-valor basado en texto, como `product:windows 10`. Se almacenan y se comparan en minúsculas. El número máximo de campos de metadatos se basa en los **[límites de los niveles de Azure Cognitive Search](../../search/search-limits-quotas-capacity.md)** .

En el caso de la versión GA, puesto que el índice de prueba se comparte entre todos los KB, el límite se aplica a todos los KB del servicio QnA Maker.

|**Nivel de Azure Cognitive Search** | **Gratis** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Número máximo de metadatos por servicio QnA Maker (en todas las KB)|1,000|100*|1,000|1,000|1,000|1,000|

### <a name="by-name-and-value"></a>Por nombre y valor

La longitud y los caracteres aceptables para el nombre y el valor de los metadatos se muestran en la tabla siguiente.

|Elemento|Caracteres permitidos|Coincidencia de patrón regex|Número máximo de caracteres|
|--|--|--|--|
|Nombre (clave)|Permite<br>Caracteres alfanuméricos (letras y dígitos)<br>`_` (subrayado)<br> No debe contener espacios.|`^[a-zA-Z0-9_]+$`|100|
|Value|Permite todo excepto<br>`:` (dos puntos)<br>`|` (barra vertical)<br>Solo se permite un valor.|`^[^:|]+$`|500|
|||||

## <a name="knowledge-base-content-limits"></a>Límites de contenido de la base de conocimiento
Límites generales del contenido de la base de conocimiento:
* Longitud del texto de la respuesta: 25 000 caracteres
* Longitud del texto de la pregunta: 1000 caracteres
* Longitud del texto de la clave de los metadatos: 100 caracteres
* Longitud del texto del valor de los metadatos: 500 caracteres
* Caracteres admitidos para el nombre de metadatos: alfabéticos, dígitos y `_`
* Caracteres admitidos para el valor de metadatos: Todos excepto `:` y `|`
* Longitud del nombre de archivo: 200
* Formatos de archivo admitidos: ".tsv", ".pdf", ".txt", ".docx", ".xlsx".
* Número máximo de preguntas alternativas: 300
* Número máximo de pares de preguntas y respuestas: en función del **[nivel de Azure Cognitive Search](../../search/search-limits-quotas-capacity.md#document-limits)** elegido. Un par de pregunta y respuesta se asigna a un documento en el índice de Azure Cognitive Search.
* Página HTML/URL: 1 millón de caracteres

## <a name="create-knowledge-base-call-limits"></a>Límites de llamada de creación de la base de conocimiento:
Representan los límites para cada acción de creación de base de conocimiento; es decir, al hacer clic en *Crear una base de conocimiento* o al llamar a la API CreateKnowledgeBase.
* Número máximo recomendado de preguntas alternativas por respuesta: 300
* Número máximo de direcciones URL: 10
* Número máximo de archivos: 10
* Número máximo de preguntas y respuestas permitidas por llamada: 1000

## <a name="update-knowledge-base-call-limits"></a>Límites de llamadas de actualización de la base de conocimiento
Representan los límites para cada acción de actualización; es decir, al hacer clic en *Save and train* (Guardar y entrenar) o al llamar a la API UpdateKnowledgeBase.
* Longitud de cada nombre de origen: 300
* Número máximo recomendado de preguntas alternativas agregadas o eliminadas: 300
* Número máximo de campos de metadatos agregados o eliminados: 10
* Número máximo de direcciones URL que se pueden actualizar: 5
* Número máximo de preguntas y respuestas permitidas por llamada: 1000

## <a name="add-unstructured-file-limits"></a>Adición de límites de archivo no estructurados

> [!NOTE]
> * Si necesita usar archivos con un tamaño superior al límite permitido, puede dividir el archivo en fragmentos más pequeños antes de enviarlos a la API. 

Estos representan los límites cuando se usan archivos no estructurados para *crear KB* o llamar a la API CreateKnowledgeBase:
* Longitud del archivo: extraeremos los primeros 32 000 caracteres.
* Máximo de tres respuestas por archivo.

## <a name="prebuilt-question-answering-limits"></a>Límites de respuesta a preguntas precompilados

> [!NOTE]
> * Si necesita usar documentos con un tamaño superior al límite permitido, puede dividir el texto en fragmentos más pequeños antes de enviarlos a la API. 
> * Un documento es una sola cadena de caracteres de texto.  

Estos representan los límites cuando se usa la API precompilada para *generar respuesta* o llamar a la API GenerateAnswer:
* Número de documentos: 5
* Tamaño máximo de un solo documento: 5120 caracteres.
* Máximo de tres respuestas por documento.

> [!IMPORTANT]
> Compatibilidad con archivos o contenidos no estructurados y solo está disponible en la respuesta a preguntas.

## <a name="next-steps"></a>Pasos siguientes

Aprenda cuándo y cómo cambiar los [planes de tarifa de servicio](How-To/set-up-qnamaker-service-azure.md#upgrade-qna-maker-sku).
