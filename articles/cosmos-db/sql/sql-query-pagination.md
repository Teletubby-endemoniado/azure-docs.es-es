---
title: Paginación en Azure Cosmos DB
description: Más información sobre los conceptos de paginación y los tokens de continuación
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/15/2021
ms.openlocfilehash: 699906e99adabdd14520eb0b391d7b1d6018ffa5
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130219079"
---
# <a name="pagination-in-azure-cosmos-db"></a>Paginación en Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

En Azure Cosmos DB, las consultas pueden tener varias páginas de resultados. En este documento se explican los criterios que el motor de consultas de Azure Cosmos DB utiliza para decidir si dividir los resultados de la consulta en varias páginas. Opcionalmente, puede usar tokens de continuación para administrar los resultados de la consulta que abarcan varias páginas.

## <a name="understanding-query-executions"></a>Descripción de las ejecuciones de consultas

A veces, los resultados de la consulta se dividirán en varias páginas. Los resultados de cada página se generan mediante la ejecución de una consulta independiente. Cuando los resultados de la consulta no se pueden devolver en una sola ejecución, Azure Cosmos DB dividirá automáticamente los resultados en varias páginas.

Puede especificar el número máximo de elementos devueltos por una consulta estableciendo el elemento `MaxItemCount`. `MaxItemCount` se especifica por solicitud e indica que el motor de consulta devolverá ese número de elementos o menos. Puede establecer `MaxItemCount` en `-1` si no desea establecer un límite para el número de resultados por ejecución de consulta.

Además, hay otras razones por las que el motor de consultas puede necesitar dividir los resultados de la consulta en varias páginas. Entre ellas se incluyen las siguientes:

- El contenedor estaba limitado y no había ninguna RU disponible para devolver más resultados de la consulta.
- La respuesta de la ejecución de la consulta era demasiado grande.
- La hora de ejecución de la consulta era demasiado larga.
- Era más eficaz que el motor de consultas devolviera los resultados en ejecuciones adicionales.

El número de elementos devueltos por la ejecución de la consulta siempre será menor o igual que `MaxItemCount`. Sin embargo, es posible que otros criterios hayan limitado el número de resultados que la consulta podría devolver. Si ejecuta la misma consulta varias veces, el número de páginas puede no ser constante. Por ejemplo, si una consulta está limitada, puede haber menos resultados disponibles por página, lo que significa que la consulta tendrá páginas adicionales. En algunos casos, también es posible que la consulta devuelva una página vacía de resultados.

## <a name="handling-multiple-pages-of-results"></a>Administración de varias páginas de resultados

Para garantizar resultados de consulta precisos, debe supervisar todas las páginas. Debe continuar ejecutando las consultas hasta que no haya ninguna página adicional.

Estos son algunos ejemplos del procesamiento de los resultados de las consultas con varias páginas:

- [SDK de .NET](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L294)
- [SDK de Java](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/examples/documentcrud/sync/DocumentCRUDQuickstart.java#L162-L176)
- [SDK de Node.js](https://github.com/Azure/azure-sdk-for-js/blob/83fcc44a23ad771128d6e0f49043656b3d1df990/sdk/cosmosdb/cosmos/samples/IndexManagement.ts#L128-L140)
- [SDK de Python](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/samples/examples.py#L89)

## <a name="continuation-tokens"></a>Tokens de continuación

En el SDK de .NET y el SDK de Java, puede usar opcionalmente tokens de continuación como marcador del progreso de la consulta. Las ejecuciones de consultas de Azure Cosmos DB no tienen estado en el lado servidor y se pueden reanudar en cualquier momento con el token de continuación. En el SDK de Python y el SDK de Node.js, estos tokens se admiten para las consultas de partición única y se debe especificar la PK en el objeto de opciones porque no basta con tenerla en la propia consulta.

Este es un ejemplo de uso de los tokens de continuación:

- [SDK de .NET](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L230)
- [SDK de Java](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/main/src/main/java/com/azure/cosmos/examples/queries/sync/QueriesQuickstart.java#L216)
- [SDK de Node.js](https://github.com/Azure/azure-sdk-for-js/blob/2186357a6e6a64b59915d0cf3cba845be4d115c4/sdk/cosmosdb/cosmos/samples/src/BulkUpdateWithSproc.ts#L16-L31)
- [SDK de Python](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/cosmos/azure-cosmos/test/test_query.py#L533)

Si la consulta devuelve un token de continuación, significa que hay resultados de consulta adicionales.

En la API de REST de Azure Cosmos DB, puede administrar los tokens de continuación con el encabezado `x-ms-continuation`. Al igual que con las consultas mediante el SDK de .NET o Java, si el encabezado de respuesta `x-ms-continuation` no está vacío, significa que la consulta tiene resultados adicionales.

Siempre que use la misma versión del SDK, los tokens de continuación nunca expiran. Opcionalmente, puede [restringir el tamaño de un token de continuación](/dotnet/api/microsoft.azure.documents.client.feedoptions.responsecontinuationtokenlimitinkb#Microsoft_Azure_Documents_Client_FeedOptions_ResponseContinuationTokenLimitInKb). Independientemente de la cantidad de datos o el número de particiones físicas del contenedor, las consultas devuelven un único token de continuación.

No puede usar tokens de continuación para las consultas con [GROUP BY](sql-query-group-by.md) o [DISTINCT](sql-query-keywords.md#distinct), porque estas consultas requerirían almacenar una cantidad significativa de estados. En el caso de las consultas con `DISTINCT`, puede usar tokens de continuación si agrega `ORDER BY` a la consulta.

Este es un ejemplo de una consulta con `DISTINCT` que podría usar un token de continuación:

```sql
SELECT DISTINCT VALUE c.name
FROM c
ORDER BY c.name
```

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Cosmos DB](../introduction.md)
- [Ejemplos de .NET de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Cláusula ORDER BY](sql-query-order-by.md)
