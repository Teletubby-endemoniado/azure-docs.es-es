---
title: 'Funciones de plantillas: numérico'
description: Describe las funciones para usar en una plantilla de Azure Resource Manager (plantilla de ARM) para trabajar con números.
ms.topic: conceptual
ms.date: 09/09/2021
ms.openlocfilehash: ccfed6794b81b7910310cc5a9fd02dcf0cdb0e0f
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124820434"
---
# <a name="numeric-functions-for-arm-templates"></a>Funciones numéricas para plantillas de ARM

Resource Manager ofrece las siguientes funciones para trabajar con enteros en la plantilla de Azure Resource Manager (plantilla de ARM):

* [add](#add)
* [copyIndex](#copyindex)
* [div](#div)
* [float](#float)
* [int](#int)
* [max](#max)
* [min](#min)
* [mod](#mod)
* [mul](#mul)
* [sub](#sub)

## <a name="add"></a>add

`add(operand1, operand2)`

Devuelve la suma de los dos enteros especificados.

La función `add` no se admite en Bicep. En su lugar, utilice el [operador `+`](../bicep/operators-numeric.md#add-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
|operand1 |Sí |int |Primer número que se agregará. |
|operand2 |Sí |int |Segundo número que se agregará. |

### <a name="return-value"></a>Valor devuelto

Un entero que contiene la suma de los parámetros.

### <a name="example"></a>Ejemplo

El ejemplo siguiente agrega dos parámetros.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/add.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| addResult | Int | 8 |

## <a name="copyindex"></a>copyIndex

`copyIndex(loopName, offset)`

Devuelve el índice actual de un bucle de iteración.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| loopName | No | string | El nombre del bucle para obtener la iteración. |
| offset |No |int |El número que se agregará al valor de iteración basado en cero. |

### <a name="remarks"></a>Comentarios

Esta función siempre se usa con un objeto **copy** . Si no se proporciona ningún valor para **offset**, se devuelve el valor de la iteración actual. El valor del iteración comienza en cero.

La propiedad **loopName** le permite especificar si copyIndex hace referencia a una iteración de recursos o una iteración de propiedades. Si no se proporciona ningún valor para **loopName**, se usa la iteración de tipo de recurso actual. Proporcione un valor para **loopName** al iterar en una propiedad.

Para más información sobre el uso de la copia, consulte:

* [Iteración de recursos en las plantillas de ARM](copy-resources.md)
* [Iteración de propiedades en las plantillas de ARM](copy-properties.md)
* [Iteración de variables en las plantillas de ARM](copy-variables.md)
* [Iteración de salida en las plantillas de ARM](copy-outputs.md)

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra un bucle de copia y el valor de índice incluido en el nombre.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/copyindex.json":::

### <a name="return-value"></a>Valor devuelto

Un entero que representa el índice actual de la iteración.

## <a name="div"></a>div

`div(operand1, operand2)`

Devuelve la división de enteros de los dos enteros especificados.

La función `div` no se admite en Bicep. En su lugar, utilice el [operador `/`](../bicep/operators-numeric.md#divide-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| operand1 |Sí |int |Número que se va a dividir. |
| operand2 |Sí |int |Número que se usa para dividir. No puede ser 0. |

### <a name="return-value"></a>Valor devuelto

Un entero que representa la división.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se divide un parámetro por otro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/div.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| divResult | Int | 2 |

## <a name="float"></a>FLOAT

`float(arg1)`

Convierte el valor en un número de punto flotante. Solo use esta función al pasar parámetros personalizados a una aplicación, como una aplicación lógica.

La función `float` no es compatible con Bicep.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |cadena o entero |El valor para convertir en número de punto flotante. |

### <a name="return-value"></a>Valor devuelto

Un número de punto flotante.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo usar float para pasar parámetros a una aplicación lógica:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/float.json":::

## <a name="int"></a>int

`int(valueToConvert)`

Convierte el valor especificado en un entero.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| valueToConvert |Sí |cadena o entero |Valor que se convierte en entero. |

### <a name="return-value"></a>Valor devuelto

Un entero del valor convertido.

### <a name="example"></a>Ejemplo

En la plantilla de ejemplo siguiente se convierte el valor del parámetro proporcionado por el usuario en entero.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/int.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| intResult | Int | 4 |

## <a name="max"></a>max

`max (arg1)`

Devuelve el valor máximo de una matriz de enteros o una lista separada por comas de enteros.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |matriz de enteros, o lista separada por comas de enteros |La colección para obtener el valor máximo. |

### <a name="return-value"></a>Valor devuelto

Un entero que representa el valor máximo de la colección.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo utilizar "max" con una matriz y una lista de enteros.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/max.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| arrayOutput | Int | 5 |
| intOutput | Int | 5 |

## <a name="min"></a>Min

`min (arg1)`

Devuelve el valor mínimo de una matriz de enteros o una lista separada por comas de enteros.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |matriz de enteros, o lista separada por comas de enteros |La colección para obtener el valor mínimo. |

### <a name="return-value"></a>Valor devuelto

Un entero que representa el valor mínimo de la colección.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo utilizar "min" con una matriz y una lista de enteros.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/min.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| arrayOutput | Int | 0 |
| intOutput | Int | 0 |

## <a name="mod"></a>mod

`mod(operand1, operand2)`

Devuelve el resto de la división de enteros de los dos enteros especificados.

La función `mod` no es compatible con Bicep. En su lugar, utilice el [operador %](../bicep/operators-numeric.md#modulo-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| operand1 |Sí |int |Número que se va a dividir. |
| operand2 |Sí |int |Número que se usa para dividir; no puede ser 0. |

### <a name="return-value"></a>Valor devuelto

Un entero que representa el resto.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se devuelve el resultado de dividir un parámetro por otro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/mod.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| modResult | Int | 1 |

## <a name="mul"></a>mul

`mul(operand1, operand2)`

Devuelve la multiplicación de los dos enteros especificados.

La función `mul` no es compatible con Bicep. En su lugar, utilice el [operador *](../bicep/operators-numeric.md#multiply-).

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| operand1 |Sí |int |Primer número que se va a multiplicar. |
| operand2 |Sí |int |Segundo número que se va a multiplicar. |

### <a name="return-value"></a>Valor devuelto

Un entero que representa la multiplicación.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se multiplica un parámetro por otro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/mul.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| mulResult | Int | 15 |

## <a name="sub"></a>sub

`sub(operand1, operand2)`

Devuelve la resta de los dos enteros especificados.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| operand1 |Sí |int |Número del que se resta. |
| operand2 |Sí |int |Número que se resta. |

### <a name="return-value"></a>Valor devuelto

Un entero que representa la resta.

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se resta un parámetro de otro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/numeric/sub.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| subResult | Int | 4 |

## <a name="next-steps"></a>Pasos siguientes

* Puede encontrar una descripción de las secciones de una plantilla de Azure Resource Manager en [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](./syntax.md).
* Para iterar un número especificado de veces al crear un tipo de recurso, consulte [Iteración de recursos en las plantillas de ARM](copy-resources.md).
