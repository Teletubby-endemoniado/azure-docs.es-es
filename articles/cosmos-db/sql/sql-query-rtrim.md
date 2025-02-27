---
title: RTRIM en el lenguaje de consulta de Azure Cosmos DB
description: Obtenga información acerca de la función del sistema de SQL RTRIM en Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/14/2021
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: f1dfaa8de0f6fa6d04fe91618a68a79d4121b7bd
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128647791"
---
# <a name="rtrim-azure-cosmos-db"></a>RTRIM (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

 Devuelve una expresión de cadena después de quitar los espacios en blanco finales o los caracteres especificados.  
  
## <a name="syntax"></a>Sintaxis
  
```sql
RTRIM(<str_expr1>[, <str_expr2>])  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr1*  
   Es una expresión de cadena.

*str_expr2*  
   Es una expresión de cadena opcional que se extrae de str_expr1. Si no se establece, el valor predeterminado es un espacio en blanco.  
  
## <a name="return-types"></a>Tipos de valores devueltos
  
  Devuelve una expresión de cadena.  
  
## <a name="examples"></a>Ejemplos
  
  En el ejemplo siguiente se muestra cómo usar `RTRIM` en una consulta.  
  
```sql
SELECT RTRIM("   abc") AS t1, 
RTRIM("   abc   ") AS t2, 
RTRIM("abc   ") AS t3, 
RTRIM("abc") AS t4,
RTRIM("abc", "bc") AS t5,
RTRIM("abc", "abc") AS t6
```  
  
 El conjunto de resultados es el siguiente:  
  
```json
[
    {
        "t1": "   abc",
        "t2": "   abc",
        "t3": "abc",
        "t4": "abc",
        "t5": "a",
        "t6": ""
    }
]
``` 

## <a name="remarks"></a>Observaciones

Esta función del sistema no usará el índice.

## <a name="next-steps"></a>Pasos siguientes

- [Funciones de cadena Azure Cosmos DB](sql-query-string-functions.md)
- [Funciones del sistema (Azure Cosmos DB)](sql-query-system-functions.md)
- [Introducción a Azure Cosmos DB](../introduction.md)
