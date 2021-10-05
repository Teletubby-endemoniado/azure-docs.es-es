---
title: 'Funciones de plantillas: comparación'
description: Describe las funciones para usar en una plantilla de Azure Resource Manager para comparar valores.
ms.topic: conceptual
ms.date: 09/08/2021
ms.openlocfilehash: 0f9243cdc61ad5e7710663328eb4f85c9471fda5
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124812359"
---
# <a name="comparison-functions-for-arm-templates"></a>Funciones de comparación para plantillas de ARM

Resource Manager ofrece varias funciones para realizar comparaciones en las plantillas de Azure Resource Manager:

* [coalesce](#coalesce)
* [equals](#equals)
* [greater](#greater)
* [greaterOrEquals](#greaterorequals)
* [less](#less)
* [lessOrEquals](#lessorequals)

## <a name="coalesce"></a>coalesce

`coalesce(arg1, arg2, arg3, ...)`

Devuelve el primer valor no nulo de los parámetros. Las cadenas vacías, las matrices vacías y los objetos vacíos no son nulos.

En Bicep, use el operador `??` en su lugar. Vea [Coalesce ??](../bicep/operators-logical.md#coalesce-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |int, string, array u object |El primer valor para comprobar si hay valores nulos. |
| más argumentos |No |int, string, array u object | Más valores para probar si hay valores nulos. |

### <a name="return-value"></a>Valor devuelto

El valor de los primeros parámetros que no son nulos, que puede ser una cadena, un entero, una matriz o un objeto. Es nulo si todos los parámetros son nulos.

### <a name="example"></a>Ejemplo

En la plantilla de ejemplo siguiente se muestra el resultado de los diferentes usos de coalesce.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/comparison/coalesce.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| stringOutput | String | default |
| intOutput | Int | 1 |
| objectOutput | Object | {"first": "default"} |
| arrayOutput | Array | [1] |
| emptyOutput | Bool | True |

## <a name="equals"></a>equals

`equals(arg1, arg2)`

Comprueba si dos valores son iguales.

En Bicep, use el operador `==` en su lugar. Vea [Equals ==](../bicep/operators-comparison.md#equals-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |int, string, array u object |El primer valor en el que comprobar la igualdad. |
| arg2 |Sí |int, string, array u object |El segundo valor en el que comprobar la igualdad. |

### <a name="return-value"></a>Valor devuelto

Devuelve **True** si los valores son iguales; en caso contrario, **False**.

### <a name="remarks"></a>Comentarios

La función equals se suele usar con el elemento `condition` para comprobar si está implementado un recurso.

```json
{
  "condition": "[equals(parameters('newOrExisting'),'new')]",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('storageAccountName')]",
  "apiVersion": "2017-06-01",
  "location": "[resourceGroup().location]",
  "sku": {
    "name": "[variables('storageAccountType')]"
  },
  "kind": "Storage",
  "properties": {}
}
```

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se comprueba la igualdad de diferentes tipos de valores. Todos los valores predeterminados devuelven True.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/comparison/equals.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | True |
| checkArrays | Bool | True |
| checkObjects | Bool | True |

En la plantilla de ejemplo siguiente se usa [not](template-functions-logical.md#not) con **equals**.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/logical/not-equals.json":::

El resultado del ejemplo anterior es:

| Nombre | Tipo | Valor |
| ---- | ---- | ----- |
| checkNotEquals | Bool | True |

## <a name="greater"></a>greater

`greater(arg1, arg2)`

Comprueba si el primer valor es mayor que el segundo.

En Bicep, use el operador `>` en su lugar. Vea [Mayor que >](../bicep/operators-comparison.md#greater-than-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |entero o cadena |El primer valor de la comparación mayor. |
| arg2 |Sí |entero o cadena |El segundo valor de la comparación mayor. |

### <a name="return-value"></a>Valor devuelto

Devuelve **True** si el primer valor es mayor que el segundo; en caso contrario, **False**.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se comprueba si un valor es mayor que el otro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/comparison/greater.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True |

## <a name="greaterorequals"></a>greaterOrEquals

`greaterOrEquals(arg1, arg2)`

Comprueba si el primer valor es mayor o igual que el segundo.

En Bicep, use el operador `>=` en su lugar. Vea [Mayor o igual que >=](../bicep/operators-comparison.md#greater-than-or-equal-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |entero o cadena |El primer valor de la comparación mayor o igual. |
| arg2 |Sí |entero o cadena |El segundo valor de la comparación mayor o igual. |

### <a name="return-value"></a>Valor devuelto

Devuelve **True** si el primer valor es mayor o igual que el segundo; en caso contrario, **False**.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se comprueba si un valor es mayor o igual que el otro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/comparison/greaterorequals.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True |

## <a name="less"></a>less

`less(arg1, arg2)`

Comprueba si el primer valor es menor que el segundo.

En Bicep, use el operador `<` en su lugar. Vea [Menor que <](../bicep/operators-comparison.md#less-than-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |entero o cadena |El primer valor de la comparación menor. |
| arg2 |Sí |entero o cadena |El segundo valor de la comparación menor. |

### <a name="return-value"></a>Valor devuelto

Devuelve **True** si el primer valor es menor que el segundo; en caso contrario, **False**.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se comprueba si un valor es menor que el otro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/comparison/less.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | False |

## <a name="lessorequals"></a>lessOrEquals

`lessOrEquals(arg1, arg2)`

Comprueba si el primer valor es menor o igual que el segundo.

En Bicep, use el operador `<=` en su lugar. Vea [Menor o igual que <=](../bicep/operators-comparison.md#less-than-or-equal-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |entero o cadena |El primer valor de la comparación menor o igual. |
| arg2 |Sí |entero o cadena |El segundo valor de la comparación menor o igual. |

### <a name="return-value"></a>Valor devuelto

Devuelve **True** si el primer valor es menor o igual que el segundo; en caso contrario, **False**.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se comprueba si un valor es menor o igual que el otro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/comparison/lessorequals.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | False |

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una descripción de las secciones de una plantilla de ARM, vea [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](./syntax.md).
