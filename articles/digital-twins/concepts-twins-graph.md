---
title: Gemelos digitales y el grafo de gemelos
titleSuffix: Azure Digital Twins
description: Comprenda el concepto de un gemelo digital y cómo sus relaciones forman un grafo.
author: baanders
ms.author: baanders
ms.date: 8/26/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 42cce83683df789aeaabe53ca170f17319ec3603
ms.sourcegitcommit: 40866facf800a09574f97cc486b5f64fced67eb2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2021
ms.locfileid: "123224847"
---
# <a name="understand-digital-twins-and-their-twin-graph"></a>Explicación del concepto de gemelos digitales y su grafo gemelo

En una solución de Azure Digital Twins, las entidades del entorno se representan mediante **gemelos digitales**. Un gemelo digital es una instancia de uno de sus [modelos](concepts-models.md) definidos de forma personalizada. Se puede conectar a otros gemelos digitales a través de **relaciones** para formar una **grafo de gemelos**: este grafo de gemelos es la representación de todo el entorno.

> [!TIP]
> "Azure Digital Twins" hace referencia a este servicio de Azure en conjunto. "Gemelos digitales" o simplemente "gemelos" hace referencia a nodos gemelos individuales dentro de la instancia del servicio.

## <a name="digital-twins"></a>Gemelos digitales

Para poder crear un gemelo digital en la instancia de Azure Digital Twins, debe tener un *modelo* cargado en el servicio. Un modelo describe el conjunto de propiedades, los mensajes de telemetría y las relaciones que puede tener un gemelo determinado, entre otras cosas. Para conocer los tipos de información que se definen en un modelo, vea [Modelos personalizados](concepts-models.md).

Después de crear y cargar un modelo, su aplicación cliente puede crear una instancia del tipo, que es un gemelo digital. Por ejemplo, después de crear un modelo de Floor, puede crear uno o varios gemelos digitales que usen este tipo (como un gemelo de tipo Floor denominado "GroundFloor", otro denominado "Floor2", etc.).

[!INCLUDE [digital-twins-versus-device-twins](../../includes/digital-twins-versus-device-twins.md)]

## <a name="relationships-a-graph-of-digital-twins"></a>Relaciones: un grafo de gemelos digitales

Los gemelos se conectan a un grafo de gemelos a través de sus relaciones. Las relaciones que un gemelo puede tener se definen como parte de su modelo.  

Por ejemplo, el modelo Floor podría definir una relación *contains* dirigida a gemelos de tipo Room. Con esta definición, Azure Digital Twins le permitirá crear relaciones *contains* desde cualquier gemelo Floor en cualquier gemelo Room (incluidos los gemelos que son de subtipos Room). 

El resultado de este proceso es un conjunto de nodos (los gemelos digitales) conectado a través de bordes (sus relaciones) en un grafo.

[!INCLUDE [visualizing with Azure Digital Twins explorer](../../includes/digital-twins-visualization.md)]

:::image type="content" source="media/concepts-azure-digital-twins-explorer/azure-digital-twins-explorer-demo.png" alt-text="Captura de pantalla de Azure Digital Twins Explorer que muestra modelos y gemelos de ejemplo." lightbox="media/concepts-azure-digital-twins-explorer/azure-digital-twins-explorer-demo.png":::

## <a name="create-with-the-apis"></a>Creación con las API

En esta sección se muestra en qué se basa la creación de gemelos digitales y relaciones desde una aplicación cliente. Contiene ejemplos de código de .NET que usan las [API de DigitalTwins](/rest/api/digital-twins/dataplane/twins) para proporcionar más contexto sobre lo que sucede dentro de cada uno de estos conceptos.

### <a name="create-digital-twins"></a>Creación de gemelos digitales

A continuación se muestra un fragmento de código de cliente que usa las [API de DigitalTwins](/rest/api/digital-twins/dataplane/twins) para crear una instancia de un gemelo de tipo Room con un elemento `twinId` que se define durante dicha creación.

Puede inicializar las propiedades de un gemelo cuando se crea o más adelante. Para crear un gemelo con propiedades inicializadas, cree un documento JSON que proporcione los valores de inicialización necesarios.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="CreateTwin_noHelper":::

También puede usar una clase auxiliar denominada `BasicDigitalTwin` para almacenar campos de propiedades en un objeto "gemelo" más directamente, como alternativa al uso de un diccionario. Para obtener más información sobre la clase auxiliar y observar ejemplos de su uso, vea la sección [Creación de un gemelo digital](how-to-manage-twin.md#create-a-digital-twin) de  *Administración de Digital Twins*.

>[!NOTE]
>Aunque las propiedades del gemelo se tratan como opcionales y, por lo tanto, no tienen que inicializarse, los [componentes](concepts-models.md#elements-of-a-model) del gemelo **deben** establecerse cuando este se crea. Aunque pueden ser objetos vacíos, los componentes en sí deben existir.

### <a name="create-relationships"></a>Crear relaciones

A continuación figura un ejemplo de código de cliente que usa las [API de DigitalTwins](/rest/api/digital-twins/dataplane/twins) para crear una relación entre dos gemelos digitales (el gemelo de "origen" y el de "destino").

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/graph_operations_other.cs" id="CreateRelationship_short":::

## <a name="json-representations-of-graph-elements"></a>Representaciones JSON de elementos de grafo

Los datos de gemelos digitales y los de relaciones se almacenan en formato JSON, es decir, cuando [consulte el grafo de gemelos](how-to-query-graph.md) en la instancia de Azure Digital Twins, el resultado será una representación JSON de los gemelos digitales y las relaciones que ha creado.

### <a name="digital-twin-json-format"></a>Formato JSON de gemelo digital

Si se representa como un objeto JSON, un gemelo digital mostrará los campos siguientes:

| Nombre del campo | Descripción |
| --- | --- |
| `$dtId` | Cadena proporcionada por el usuario que representa el identificador del gemelo digital. |
| `$etag` | Campo HTTP estándar asignado por el servidor web. |
| `$conformance` | Enumeración que contiene el estado de conformidad de este gemelo digital (*conforme*, *no conforme* o *desconocido*) |
| `<property-name>` | Valor de una propiedad en formato JSON (`string`, tipo de número u objeto). |
| `$relationships` | Dirección URL de la ruta de acceso a la colección de relaciones. Este campo no está presente si el gemelo digital no tiene bordes de relación salientes. |
| `$metadata.$model` | [Opcional] Identificador de la interfaz del modelo que caracteriza al gemelo digital. |
| `$metadata.<property-name>.desiredValue` | [Solo para propiedades grabables] Valor deseado de la propiedad especificada. |
| `$metadata.<property-name>.desiredVersion` | [Solo para propiedades grabables] Versión del valor deseado. |
| `$metadata.<property-name>.ackVersion` | Versión confirmada por la aplicación del dispositivo que implementa el gemelo digital. |
| `$metadata.<property-name>.ackCode` | [Solo para propiedades grabables] Código `ack` devuelto por la aplicación del dispositivo que implementa el gemelo digital. |
| `$metadata.<property-name>.ackDescription` | [Solo para propiedades grabables] Descripción `ack` devuelta por la aplicación del dispositivo que implementa el gemelo digital. |
| `<component-name>` | Objeto JSON que contiene los valores de propiedad y los metadatos del componente, similares a los del objeto raíz. Este objeto existe aunque el componente no tenga propiedades. |
| `<component-name>.<property-name>` | Valor de la propiedad del componente en formato JSON (`string`, tipo de número u objeto). |
| `<component-name>.$metadata` | Información de metadatos del componente, similar al nivel de raíz `$metadata`. |

A continuación se muestra un ejemplo de gemelo digital formateado como objeto JSON:

```json
{
  "$dtId": "Cafe",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "Temperature": 72,
  "Location": {
    "x": 101,
    "y": 33
  },
  "component": {
    "TableOccupancy": 1,
    "$metadata": {
      "TableOccupancy": {
        "desiredValue": 1,
        "desiredVersion": 3,
        "ackVersion": 2,
        "ackCode": 200,
        "ackDescription": "OK"
      }
    }
  },
  "$metadata": {
    "$model": "dtmi:com:contoso:Room;1",
    "Temperature": {
      "desiredValue": 72,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "Location": {
      "desiredValue": {
        "x": 101,
        "y": 33,
      },
      "desiredVersion": 8,
      "ackVersion": 8,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

### <a name="relationship-json-format"></a>Formato JSON de relación

Si se representa como un objeto JSON, una relación de un gemelo digital mostrará los campos siguientes:

| Nombre del campo | Descripción |
| --- | --- |
| `$relationshipId` | Cadena proporcionada por el usuario que representa el identificador de esta relación. Esta cadena es única en el contexto del gemelo digital de origen, lo que también significa que `sourceId` + `relationshipId` es único en el contexto de la instancia de Azure Digital Twins. |
| `$etag` | Campo HTTP estándar asignado por el servidor web. |
| `$sourceId` | Identificador del gemelo digital de origen. |
| `$targetId` | Identificador del gemelo digital de destino. |
| `$relationshipName` | Nombre de la relación. |
| `<property-name>` | [Opcional] Valor de una propiedad de esta relación en formato JSON (`string`, tipo de número u objeto). |

A continuación se muestra un ejemplo de relación formateada como objeto JSON:

```json
{
  "$relationshipId": "relationship-01",
  "$etag": "W/\"506e8391-2b21-4ac9-bca3-53e6620f6a90\"",
  "$sourceId": "GroundFloor",
  "$targetId": "Cafe",
  "$relationshipName": "contains",
  "startDate": "2020-02-04"
}
```

## <a name="next-steps"></a>Pasos siguientes

Consulte cómo administrar elementos de grafo con las API de Azure Digital Twins:
* [Administración de Digital Twins](how-to-manage-twin.md)
* [Administración del grafo de gemelos y las relaciones](how-to-manage-graph.md)

O bien, consulte el grafo de gemelos de Azure Digital Twins para obtener información:
* [Lenguaje de consulta](concepts-query-language.md)