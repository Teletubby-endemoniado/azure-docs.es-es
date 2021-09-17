---
title: Extensión de las ontologías
titleSuffix: Azure Digital Twins
description: Más información acerca de las razones y estrategias que motivan la extensión de una ontología
author: baanders
ms.author: baanders
ms.date: 2/12/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 30d7282d6f6f30b34b522991d6fd4e79b194d7dd
ms.sourcegitcommit: 2da83b54b4adce2f9aeeed9f485bb3dbec6b8023
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2021
ms.locfileid: "122771591"
---
# <a name="extending-ontologies"></a>Extensión de las ontologías 

Una [ontología](concepts-ontologies.md) estándar del sector, como la [ontología RealEstateCore basada en DTDL para edificios inteligentes](https://github.com/Azure/opendigitaltwins-building), es una excelente manera de empezar a crear su solución de IoT. Las ontologías del sector proporcionan un completo conjunto de interfaces base diseñadas para su dominio y pensadas para trabajar de forma integrada en los servicios de Azure IoT como, por ejemplo, Azure Digital Twins. 

Sin embargo, es posible que la solución tenga necesidades específicas que no cubra la ontología del sector. Por ejemplo, puede que desee vincular los gemelos digitales a modelos 3D almacenados en un sistema independiente. En este caso, puede extender una de estas ontologías para agregar sus propias funcionalidades mientras conserva todas las ventajas de la ontología original.

En este artículo se usa la ontología [RealEstateCore](https://www.realestatecore.io/) basada en DTDL como base para los ejemplos de extensión de ontologías con nuevas propiedades de DTDL. No obstante, las técnicas descritas aquí son generales y se pueden aplicar a cualquier parte de una ontología basada en DTDL con cualquier funcionalidad de DTDL (telemetría, propiedad, relación, componente o comando). 

## <a name="realestatecore-space-hierarchy"></a>Jerarquía de espacios de RealEstateCore 

En la ontología RealEstateCore basada en DTDL, la jerarquía de espacios se usa para definir varios tipos de espacios: salas, edificios, zonas, etc. La jerarquía se extiende desde cada uno de estos modelos para definir varios tipos de salas, edificios y zonas. 

Una parte de la jerarquía tiene el aspecto del diagrama siguiente. 

:::image type="content" source="media/concepts-ontologies-extend/real-estate-core-original.png" alt-text="Diagrama que ilustra parte de la jerarquía del espacio RealEstateCore. Muestra elementos para Space, Room, ConferenceRoom y Office."::: 

Para más información sobre la ontología de RealEstateCore, consulte [Adopción de ontologías estándar del sector](concepts-ontologies-adopt.md#realestatecore-smart-building-ontology).

## <a name="extending-the-realestatecore-space-hierarchy"></a>Extensión de la jerarquía de espacios de RealEstateCore 

En algunas ocasiones, la solución presenta necesidades específicas que no cubre la ontología del sector. En este caso, la extensión de la jerarquía le permite seguir usando la ontología del sector y personalizar esta según sus necesidades. 

En este artículo se analizan dos casos diferentes en los que la extensión de la jerarquía de la ontología es útil: 

* La incorporación de nuevas interfaces para conceptos que no están en la ontología del sector. 
* La incorporación de propiedades adicionales (o relaciones, componentes, datos de telemetría o comandos) a las interfaces existentes.

### <a name="add-new-interfaces-for-new-concepts"></a>Incorporación de nuevas interfaces para nuevos conceptos 

En este caso, desea agregar interfaces para conceptos necesarios para la solución, pero que no están presentes en la ontología del sector. Por ejemplo, si la solución tiene otros tipos de salas que no están representados en la ontología RealEstateCore basada en DTDL, puede agregarlos mediante la extensión directa de las interfaces de RealEstateCore. 

En el ejemplo siguiente se presenta una solución que necesita representar "espacios de concentración", que no están presentes en la ontología RealEstateCore. Un espacio de concentración es un pequeño espacio diseñado para que los usuarios se concentren en una tarea durante un par de horas cada vez. 

Para ampliar la ontología del sector con este nuevo concepto, cree una nueva interfaz que se [extienda desde](concepts-models.md#model-inheritance) las interfaces de esta ontología. 

Después de agregar la interfaz del espacio de concentración, la jerarquía extendida muestra este nuevo tipo de sala. 

:::image type="content" source="media/concepts-ontologies-extend/real-estate-core-extended-1.png" alt-text="Diagrama que ilustra parte de la jerarquía del espacio RealEstateCore, incluida la nueva adición."::: 

### <a name="add-extra-capabilities-to-existing-interfaces"></a>Incorporación de funcionalidades adicionales a las interfaces existentes 

En este caso, desea agregar más propiedades (o relaciones, componentes, datos de telemetría o comandos) a las interfaces que se encuentran en la ontología del sector.

En esta sección, encontrará dos ejemplos: 
* Si va a crear una solución que muestre dibujos en 3D de espacios que ya tiene en un sistema existente, es posible que desee asociar cada gemelo digital a su dibujo en 3D (por identificador) para que cuando la solución muestre información sobre el espacio, también pueda recuperar el dibujo en 3D del sistema existente. 
* Si la solución necesita realizar un seguimiento del estado "en línea" o "sin conexión" de las salas de conferencias, es posible que desee realizar un seguimiento del estado de la sala de conferencias para su uso en pantallas o consultas. 

Ambos ejemplos se pueden implementar con nuevas propiedades: una propiedad `drawingId` que asocia el dibujo en 3D con el gemelo digital y una propiedad "online" que indica si la sala de conferencias está en línea o no. 

Normalmente, no desea modificar la ontología del sector directamente porque quiere poder incorporar actualizaciones a su solución en el futuro (lo cual sobrescribiría estas adiciones). En su lugar, estos tipos de adiciones se pueden realizar en su propia jerarquía de la interfaz que se extiende desde la ontología RealEstateCore basada en DTDL. Cada interfaz que se crea utiliza las herencias de varias interfaces para extender su interfaz de RealEstateCore primaria y su interfaz primaria desde la jerarquía de interfaz extendida. Este enfoque le permite hacer uso de la ontología del sector y de las adiciones de forma conjunta. 

Para extender la ontología del sector, crea sus propias interfaces que se extienden desde las interfaces de la ontología del sector y agrega las nuevas funcionalidades a las interfaces extendidas. Para cada interfaz que desee extender, cree una nueva interfaz. Las interfaces extendidas están escritas en DTDL (consulte la sección DTDL para interfaces extendidas más adelante en este documento). 

Después de extender la parte de la jerarquía mostrada anteriormente, la jerarquía extendida es similar al diagrama siguiente. Aquí, la interfaz extendida Espacio agrega la propiedad `drawingId` que contiene un identificador que asocia el gemelo digital al dibujo en 3D. Además, la interfaz SalaDeConferencias agrega una propiedad "online" que contiene el estado "en línea" de la sala de conferencias. Mediante herencia, la interfaz SalaDeConferencias contiene todas las funcionalidades de la interfaz SalaDeConferencias de RealEstateCore, así como todas las funcionalidades de la interfaz Espacio extendida. 

:::image type="content" source="media/concepts-ontologies-extend/real-estate-core-extended-2.png" alt-text="Diagrama que muestra la jerarquía del espacio RealEstateCore extendida, con más adiciones nuevas como se ha descrito."::: 

## <a name="using-the-extended-space-hierarchy"></a>Uso de la jerarquía de espacios extendida 

Al crear gemelos digitales mediante la jerarquía de espacios extendida, cada modelo de gemelo digital será uno en la jerarquía de espacios extendida (no en la ontología del sector original) e incluirá todas las funcionalidades de la ontología del sector y las interfaces extendidas mediante la herencia de las interfaces.

Cada modelo de gemelo digital será una interfaz de la jerarquía extendida como se muestra en el siguiente diagrama. 
 
:::image type="content" source="media/concepts-ontologies-extend/ontology-with-models.png" alt-text="Diagrama que ilustra la jerarquía extendida del espacio RealEstateCore, incluidos los modelos conectados Space, Room, ConferenceRoom, Office y FocusRoom."::: 

Al consultar los gemelos digitales mediante el identificador de modelo (el operador `IS_OF_MODEL`), se deben usar los identificadores de modelos de la jerarquía extendida. Por ejemplo, `SELECT * FROM DIGITALTWINS WHERE IS_OF_MODEL('dtmi:com:example:Office;1')`. 

## <a name="contributing-back-to-the-original-ontology"></a>Contribución a la ontología original 

En algunos casos, extenderá la ontología del sector de una forma que sea muy útil para la mayoría de los usuarios de la ontología. En este caso, debería considerar la posibilidad de contribuir con sus extensiones a la ontología original. Cada ontología tiene un proceso diferente de contribución, por lo que debe consultar el repositorio de GitHub de la ontología para obtener los detalles de la contribución. 

## <a name="dtdl-for-new-interfaces"></a>DTDL para nuevas interfaces 

El DTDL para las nuevas interfaces que se extienden directamente desde la ontología del sector tiene un aspecto similar al siguiente. 

```json
{
  "@id": "dtmi:com:example:FocusRoom;1", 
  "@type": "interface", 
  "extends": "dtmi:digitaltwins:rec_3_3:building:Office;1", 
  "@context": "dtmi:dtdl:context;2" 
} 
```

## <a name="dtdl-for-extended-interfaces"></a>DTDL para interfaces extendidas 

El DTDL de las interfaces extendidas, limitado a la parte descrita anteriormente, tendría el siguiente aspecto. 

```json
[
  {
    "@id": "dtmi:com:example:Space;1",
    "@type": "Interface",
    "extends": "dtmi:digitaltwins:rec_3_3:core:Space;1",
    "contents": [
      {
        "@type": "Property",
        "name": "drawingid",
        "schema": "string"
      }
    ],
    "@context": "dtmi:dtdl:context;2"
  },
  {
    "@id": "dtmi:com:example:Room;1",
    "@type": "Interface",
    "extends": [
      "dtmi:digitaltwins:rec_3_3:core:Room;1",
      "dtmi:com:example:Space;1"
    ],
    "@context": "dtmi:dtdl:context;2"
  },
  {
    "@id": "dtmi:com:example:ConferenceRoom;1",
    "@type": "Interface",
    "extends": [
      "dtmi:digitaltwins:rec_3_3:building:ConferenceRoom;1",
      "dtmi:com:example:Room;1"
    ],
    "contents": [
      {
        "@type": "Property",
        "name": "online",
        "schema": "boolean"
      }
    ],
    "@context": "dtmi:dtdl:context;2"
  },
  {
    "@id": "dtmi:com:example:Office;1",
    "@type": "Interface",
    "extends": [
      "dtmi:digitaltwins:rec_3_3:building:Office;1", 
      "dtmi:com:example:Room;1" 
    ],
    "@context": "dtmi:dtdl:context;2" 
  }, 
  {
    "@id": "dtmi:com:example:FocusRoom;1", 
    "@type": "Interface", 
    "extends": "dtmi:com:example:Office;1", 
    "@context": "dtmi:dtdl:context;2" 
  }
]
``` 

## <a name="next-steps"></a>Pasos siguientes

Continúe la ruta para desarrollar modelos basados en ontologías: [Uso de estrategias de ontología en un procedimiento de desarrollo de modelos](concepts-ontologies.md#using-ontology-strategies-in-a-model-development-path).