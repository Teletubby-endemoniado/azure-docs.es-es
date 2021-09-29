---
title: Unidades de consulta en Azure Digital Twins
titleSuffix: Azure Digital Twins
description: Descripción del concepto de facturación de las unidades de consulta en Azure Digital Twins
author: baanders
ms.author: baanders
ms.date: 9/16/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: eb5e7970c2ce44b5ee96f671464a3029cb9429b1
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128631008"
---
# <a name="query-units-in-azure-digital-twins"></a>Unidades de consulta en Azure Digital Twins 

Una **unidad de consulta** de Azure Digital Twins es una unidad de cálculo a petición que se usa para ejecutar las [consultas de Azure Digital Twins](how-to-query-graph.md) mediante la [API de consulta](/rest/api/digital-twins/dataplane/query). 

Abstrae los recursos del sistema, como CPU, IOPS y memoria, que son necesarios para realizar operaciones de consulta admitidas por Azure Digital Twins, lo que le permite realizar un seguimiento del uso en unidades de consulta en su lugar.

La cantidad de unidades de consulta consumidas para ejecutar una consulta se ve afectada por...

* la complejidad de la consulta
* el tamaño del conjunto de resultados (por lo que una consulta que devuelve diez resultados consumirá más unidades de consulta que una consulta de complejidad similar que devuelve solo un resultado)

En este artículo se explica cómo comprender las unidades de consulta y realizar el seguimiento del consumo de la unidad de consulta.

## <a name="find-the-query-unit-consumption-in-azure-digital-twins"></a>Búsqueda del consumo de unidades de consulta en Azure Digital Twins

Al ejecutar una consulta con la [API de consulta](/rest/api/digital-twins/dataplane/query) de Azure Digital Twins, puede examinar el encabezado de respuesta para realizar el seguimiento del número de unidades de consulta que la consulta consume. Busque "query-charge" en la respuesta que se devuelve desde Azure Digital Twins.

Los [SDK de Azure Digital Twins](concepts-apis-sdks.md) permiten extraer el encabezado de cargo de consulta de la respuesta paginable. En esta sección se muestra cómo consultar los gemelos digitales y cómo recorrer en iteración la respuesta paginable para extraer el encabezado del cargo de la consulta. 

En el fragmento de código siguiente se muestra cómo se pueden extraer los cargos de consulta en los que se incurre cuando se llama a la API de consulta. Recorre en iteración las páginas de respuesta en primer lugar para acceder al encabezado de cargos de consulta y, a continuación, recorre en iteración los resultados de los gemelos digitales dentro de cada página. 

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/getQueryCharges.cs":::

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo hacer consultas en Azure Digital Twins, consulte:

* [Lenguaje de consulta](concepts-query-language.md)
* [Consulta del grafo de gemelos](how-to-query-graph.md)
* [Documentación de referencia de la API de consulta](/rest/api/digital-twins/dataplane/query/querytwins)

Puede encontrar límites relacionados con las consultas de Azure Digital Twins en [Límites del servicio Azure Digital Twins](reference-service-limits.md).