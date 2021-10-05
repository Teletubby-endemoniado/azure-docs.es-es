---
title: Transformación Clave suplente en el flujo de datos de asignación
titleSuffix: Azure Data Factory & Azure Synapse
description: Aprenda a usar la transformación Clave suplente de flujo de datos de asignación para generar valores de claves secuenciales en Azure Data Factory y Synapse Analytics.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.subservice: data-flows
ms.topic: conceptual
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: d125923d676b0adfd280a1882a39cacb74749a82
ms.sourcegitcommit: 48500a6a9002b48ed94c65e9598f049f3d6db60c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2021
ms.locfileid: "129059822"
---
# <a name="surrogate-key-transformation-in-mapping-data-flow"></a>Transformación Clave suplente en el flujo de datos de asignación 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

[!INCLUDE[data-flow-preamble](includes/data-flow-preamble.md)]

Use la transformación Clave suplente para agregar un valor de clave incremental a cada fila de datos. Esto resulta útil al diseñar tablas de dimensiones en un modelo de datos analíticos de esquema de estrella. En un esquema de estrella, cada miembro de las tablas de dimensiones requiere una clave única que no sea una clave empresarial.

## <a name="configuration"></a>Configuración

:::image type="content" source="media/data-flow/surrogate.png" alt-text="Transformación Clave suplente":::

**Columna de clave:** nombre de la columna de clave suplente generada.

**Valor inicial:** valor de clave más bajo que se generará.

## <a name="increment-keys-from-existing-sources"></a>Incrementar claves a partir de orígenes existentes

Para iniciar la secuencia desde un valor existente en un origen, se recomienda usar un receptor de caché para guardar ese valor y utilizar una transformación de columna derivada para agregar los dos valores juntos. Use una búsqueda almacenada en caché para obtener la salida y anexarla a la clave generada. Para obtener más información, consulte los temas sobre los [receptores de caché](data-flow-sink.md#cache-sink) y las [búsquedas almacenadas en caché](concepts-data-flow-expression-builder.md#cached-lookup).

:::image type="content" source="media/data-flow/cached-lookup-example.png" alt-text="Búsqueda de clave suplente":::

### <a name="increment-from-existing-maximum-value"></a>Incremento del valor máximo existente

Para inicializar el valor de clave con el máximo anterior, puede usar dos técnicas en función de la ubicación de los datos de origen.

#### <a name="database-sources"></a>Orígenes de base de datos

Use una opción de consulta SQL para seleccionar MAX() desde el origen. Por ejemplo, `Select MAX(<surrogateKeyName>) as maxval from <sourceTable>`.

:::image type="content" source="media/data-flow/surrogate-key-max-database.png" alt-text="Consulta de clave suplente":::

#### <a name="file-sources"></a>Orígenes de archivo

Si el valor máximo anterior se encuentra en un archivo, use la función `max()` en la transformación Agregado para obtener el valor máximo anterior:

:::image type="content" source="media/data-flow/surrogate-key-max-file.png" alt-text="Archivo de clave suplente":::

En ambos casos, tendrá que escribir en un receptor de caché y buscar el valor. 


## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream> 
    keyGenerate(
        output(<surrogateColumnName> as long),
        startAt: <number>L
    ) ~> <surrogateKeyTransformationName>
```

### <a name="example"></a>Ejemplo

:::image type="content" source="media/data-flow/surrogate.png" alt-text="Transformación Clave suplente":::

El script de flujo de datos para la configuración de clave suplente anterior se encuentra en el siguiente fragmento de código.

```
AggregateDayStats
    keyGenerate(
        output(key as long),
        startAt: 1L
    ) ~> SurrogateKey1
```

## <a name="next-steps"></a>Pasos siguientes

En estos ejemplos se usan las transformaciones [Combinación](data-flow-join.md) y [Columna derivada](data-flow-derived-column.md).
