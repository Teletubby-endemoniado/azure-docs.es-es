---
title: Propiedades de la ingesta de datos para el Explorador de datos de Azure Synapse (versión preliminar)
description: Obtenga información sobre las diversas propiedades de la ingesta de datos compatibles con el Explorador de datos de Azure Synapse.
ms.topic: conceptual
ms.date: 11/02/2021
author: shsagir
ms.author: shsagir
ms.reviewer: tzgitlin
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: data-explorer
ms.openlocfilehash: e598f3fdd03721def28ac8a0f90e3bd12958783c
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131478135"
---
# <a name="azure-synapse-data-explorer-data-ingestion-properties-preview"></a>Propiedades de la ingesta de datos del Explorador de datos de Azure Synapse (versión preliminar)

La ingesta de datos es el proceso por el que se agregan datos a una tabla y se ponen a disposición para su consulta en el Explorador de datos. Puede agregar propiedades al comando de ingesta después de la palabra clave `with`.

## <a name="ingestion-properties"></a>Propiedades de la ingesta

En la tabla siguiente se enumeran las propiedades compatibles con el Explorador de datos, se describen y se proporcionan ejemplos: 

|Propiedad              |Descripción                                              |Ejemplo                                             |
|----------------------|---------------------------------------------------------|----------------------------------------------------|
|`ingestionMapping`    |valor de cadena que indica cómo se asignan los datos del archivo de origen a las columnas reales de la tabla. Defina el valor `format` con el tipo de asignación pertinente. Vea [Asignaciones de datos](/azure/data-explorer/kusto/management/mappings?context=/azure/synapse-analytics/context/context).|`with (format="json", ingestionMapping = "[{\"column\":\"rownumber\", \"Properties\":{\"Path\":\"$.RowNumber\"}}, {\"column\":\"rowguid\", \"Properties\":{\"Path\":\"$.RowGuid\"}}]")`<br>(en desuso: `avroMapping`, `csvMapping` y `jsonMapping`) |
|`ingestionMappingReference`|valor de cadena que indica cómo se asignan los datos del archivo de origen a las columnas reales de la tabla mediante un objeto de directiva de asignación con nombre. Defina el valor `format` con el tipo de asignación pertinente. Vea [Asignaciones de datos](/azure/data-explorer/kusto/management/mappings?context=/azure/synapse-analytics/context/context).|`with (format="csv", ingestionMappingReference = "Mapping1")`<br>(en desuso: `avroMappingReference`, `csvMappingReference` y `jsonMappingReference`)|
|`creationTime` |El valor de fecha y hora (con formato de cadena ISO8601) que se usa en el momento de la creación de las extensiones de los datos ingeridos. Si no se especifica, se utilizará el valor actual (`now()`). Cuando se ingieren datos antiguos, es útil reemplazar el valor predeterminado para que la directiva de retención se aplique correctamente. Si se especifica, asegúrese de que la propiedad `Lookback` de la [directiva de combinación de extensiones](/azure/data-explorer/kusto/management/mergepolicy?context=/azure/synapse-analytics/context/context) vigente de la tabla de destino esté en línea con el valor especificado.|`with (creationTime="2017-02-13")`|
|`extend_schema`|valor booleano que, si se especifica, indica al comando que extienda el esquema de la tabla (el valor predeterminado es `false`). Esta opción solo se aplica a los comandos `.append` y `.set-or-append`. Las únicas extensiones de esquema permitidas tienen columnas adicionales agregadas a la tabla al final.|Si el esquema de tabla original es `(a:string, b:int)`, una extensión de esquema válida sería `(a:string, b:int, c:datetime, d:string)`, pero `(a:string, c:datetime)` no lo sería.|
|`folder` |En el caso de los comandos de [ingesta desde consulta](/azure/data-explorer/kusto/management/data-ingestion/ingest-from-query?context=/azure/synapse-analytics/context/context), la carpeta que se va a asignar a la tabla. Si la tabla ya existe, esta propiedad invalidará la carpeta de la tabla.|`with (folder="Tables/Temporary")`|
|`format` |El formato de los datos (consulte los [formatos de datos compatibles](data-explorer-ingest-data-supported-formats.md)).|`with (format="csv")`|
|`ingestIfNotExists`|valor de cadena que, si se especifica, impide que la ingesta se realice correctamente si la tabla ya tiene datos con la etiqueta `ingest-by:` con el mismo valor. Esto garantiza la ingesta de datos idempotente. Para más información, consulte [Etiquetas "ingerir por"](/azure/data-explorer/kusto/management/extents-overview?context=/azure/synapse-analytics/context/context#ingest-by-extent-tags).|Las propiedades `with (ingestIfNotExists='["Part0001"]', tags='["ingest-by:Part0001"]')` indican que, si ya existen datos con la etiqueta `ingest-by:Part0001`, no se completará la ingesta actual. Sin embargo, si aún no existen, esta nueva ingesta debería tener la etiqueta establecida (por si en el futuro se intentan ingerir de nuevo los mismos datos).|
|`ignoreFirstRecord` |valor booleano que, si se establece en `true`, indica que la ingesta debe omitir el primer registro de cada archivo. Esta propiedad es útil para los archivos con formato `CSV` y formatos similares, si el primer registro del archivo son los nombres de columna. De manera predeterminada, se presupone que es `false`.|`with (ignoreFirstRecord=false)`|
|`persistDetails` |valor booleano que, si se especifica, indica que el comando debe conservar los resultados detallados (aunque el resultado sea correcto) para que el comando [.show operation details](/azure/data-explorer/kusto/management/operations?context=/azure/synapse-analytics/context/context#show-operation-details) pueda recuperarlos. Tiene como valor predeterminado `false`.|`with (persistDetails=true)`|
|`policy_ingestiontime`|valor booleano que, si se especifica, describe si se habilita la [directiva de tiempo de ingesta](/azure/data-explorer/kusto/management/ingestiontimepolicy?context=/azure/synapse-analytics/context/context) en una tabla que este comando crea. El valor predeterminado es `true`.|`with (policy_ingestiontime=false)`|
|`recreate_schema` |valor booleano que, si se especifica, describe si el comando puede volver a crear el esquema de la tabla. Esta propiedad solo se aplica al comando `.set-or-replace`. Esta propiedad tiene prioridad sobre la propiedad `extend_schema` si ambas están establecidas.|`with (recreate_schema=true)`|
|`tags`|Una lista de [etiquetas](/azure/data-explorer/kusto/management/extents-overview?context=/azure/synapse-analytics/context/context#extent-tagging) que se asocian a los datos ingeridos, cuyo formato es una cadena JSON. |`with (tags="['Tag1', 'Tag2']")`|
|`validationPolicy`|cadena JSON que indica las validaciones que se ejecutan durante la ingesta. Consulte [Ingesta de datos](data-explorer-ingest-data-overview.md) para una explicación de las distintas opciones.| `with (validationPolicy='{"ValidationOptions":1, "ValidationImplications":1}')` (en realidad, esta es la directiva predeterminada).|
|`zipPattern`|Use esta propiedad al ingerir datos desde el almacenamiento que tiene un archivo ZIP. Se trata de un valor de cadena que indica la expresión regular que se va a usar al seleccionar los archivos del archivo ZIP que se van a ingerir.  Se omitirán los restantes archivos del archivo.|`with (zipPattern="*.csv")`|

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [ingesta de datos](data-explorer-ingest-data-overview.md).
- Más información sobre los [formatos de datos compatibles](data-explorer-ingest-data-supported-formats.md).
