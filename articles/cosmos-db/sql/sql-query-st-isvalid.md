---
title: ST_ISVALID en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL ST_ISVALID en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/21/2021
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5719554a64ed7d028f09fd7263425d91bb221ba2
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128625285"
---
# <a name="st_isvalid-azure-cosmos-db"></a>ST_ISVALID (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

 Devuelve un valor booleano que indica si la expresión de Point, Polygon, MultiPolygon o LineString de GeoJSON especificada es válida.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
ST_ISVALID(<spatial_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*spatial_expr*  
   Es una expresión de tipo Point, Polygon o LineString de GeoJSON.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión condicional.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo comprobar si un punto es válido con ST_VALID.  
  
  Por ejemplo, este punto tiene un valor de latitud que no está en el intervalo válido de valores [-90, 90], por lo que la consulta devuelve false.  
  
  Para los polígonos, la especificación de GeoJSON requiere que el último par de coordenadas indicado sea igual que el primero, para crear una forma cerrada. El orden de los puntos dentro de un polígono debe especificarse siguiendo el sentido contrario al de las agujas del reloj. Un polígono cuyos puntos se hayan especificado en el sentido de las agujas del reloj representa el inverso de la región dentro de él.  
  
```sql
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[{ "b": false }]  
```  
> [!NOTE]
> La especificación GeoJSON requiere que los puntos dentro de un polígono se especifiquen en sentido contrario a las agujas del reloj. Un elemento Polygon cuyos puntos se hayan especificado en el sentido de las agujas del reloj representa el inverso de la región dentro de él.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones espaciales (Azure Cosmos DB)](sql-query-spatial-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](../introduction.md)
