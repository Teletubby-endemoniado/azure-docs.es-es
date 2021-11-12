---
title: 'Funciones de plantillas: cadena'
description: Se describen las funciones que se usan en una plantilla de Azure Resource Manager para trabajar con cadenas.
ms.topic: conceptual
ms.date: 10/29/2021
ms.openlocfilehash: 44661751e3bd1f32a9b06ed6b7b6993716162cb1
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131455043"
---
# <a name="string-functions-for-arm-templates"></a>Funciones de cadena para plantillas de ARM

Resource Manager ofrece las siguientes funciones para trabajar con cadenas en la plantilla de Azure Resource Manager:

* [base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [concat](#concat)
* [contains](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [empty](#empty)
* [endsWith](#endswith)
* [first](#first)
* [format](#format)
* [guid](#guid)
* [indexOf](#indexof)
* [json](#json)
* [last](#last)
* [lastIndexOf](#lastindexof)
* [length](#length)
* [newGuid](#newguid)
* [padLeft](#padleft)
* [replace](#replace)
* [skip](#skip)
* [split](#split)
* [startsWith](#startswith)
* [string](#string)
* [substring](#substring)
* [take](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [trim](#trim)
* [uniqueString](#uniquestring)
* [uri](#uri)
* [uriComponent](#uricomponent)
* [uriComponentToString](#uricomponenttostring)

## <a name="base64"></a>base64

`base64(inputString)`

Devuelve la representación de base64 de la cadena de entrada.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| inputString |Sí |string |Valor que se va a devolver como una representación de base64. |

### <a name="return-value"></a>Valor devuelto

Una cadena que contiene la representación en base64.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo usar la función `base64`.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/base64.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | one, two, three |
| toJsonOutput | Object | {"one": "a", "two": "b"} |

## <a name="base64tojson"></a>base64ToJson

`base64tojson`

Convierte una representación en base64 a un objeto JSON.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| base64Value |Sí |string |La representación en base64 para convertir en un objeto JSON. |

### <a name="return-value"></a>Valor devuelto

Un objeto JSON.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se utiliza la función `base64ToJson` para convertir un valor base64:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/base64.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | one, two, three |
| toJsonOutput | Object | {"one": "a", "two": "b"} |

## <a name="base64tostring"></a>base64ToString

`base64ToString(base64Value)`

Convierte una representación en base64 en una cadena.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| base64Value |Sí |string |La representación en base64 para convertir en una cadena. |

### <a name="return-value"></a>Valor devuelto

Una cadena del valor convertido de base64.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se utiliza la función `base64ToString` para convertir un valor base64:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/base64.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | one, two, three |
| toJsonOutput | Object | {"one": "a", "two": "b"} |

## <a name="concat"></a>concat

`concat (arg1, arg2, arg3, ...)`

Combina varios valores de cadena y devuelve la cadena concatenada, o combina varias matrices y devuelve la matriz concatenada.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |cadena o matriz |La primera cadena o matriz para la concatenación. |
| más argumentos |No |cadena o matriz |Más matrices o cadenas en orden secuencial para la concatenación. |

Esta función puede tomar cualquier número de argumentos y puede aceptar cadenas o matrices para los parámetros. Sin embargo, no puede proporcionar ambas a la vez para los parámetros. Las cadenas solo se concatenan con otras cadenas.

### <a name="return-value"></a>Valor devuelto

Una cadena o matriz de valores concatenados.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo combinar dos valores de cadena y devolver una cadena concatenada.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/concat-string.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| concatOutput | String | prefix-5yj4yjf5mbg72 |

En el ejemplo siguiente se muestra cómo combinar dos matrices.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/array/concat-array.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| return | Array | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

## <a name="contains"></a>contains

`contains (container, itemToFind)`

Comprueba si una matriz contiene un valor, un objeto contiene una clave o una cadena contiene una subcadena. La comparación de cadena distingue mayúsculas de minúsculas. Pero, cuando se prueba si un objeto contiene una clave, la comparación no distingue mayúsculas de minúsculas.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| contenedor |Sí |matriz, objeto o cadena |El valor que contiene el valor para buscar. |
| itemToFind |Sí |cadena o entero |El valor para buscar. |

### <a name="return-value"></a>Valor devuelto

**True** si el elemento se encuentra; en caso contrario, **False**.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo utilizar contains con diferentes tipos:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/array/contains.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| stringTrue | Bool | True |
| stringFalse | Bool | False |
| objectTrue | Bool | True |
| objectFalse | Bool | False |
| arrayTrue | Bool | True |
| arrayFalse | Bool | False |

## <a name="datauri"></a>dataUri

`dataUri(stringToConvert)`

Convierte un valor en un identificador URI de datos.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToConvert |Sí |string |El valor para convertir en un identificador URI de datos. |

### <a name="return-value"></a>Valor devuelto

Una cadena con formato de identificador URI de datos.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se convierte un valor en un identificador URI de datos, y se convierte un identificador URI de datos en una cadena.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/datauri.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| dataUriOutput | String | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | String | Hola mundo. |

## <a name="datauritostring"></a>dataUriToString

`dataUriToString(dataUriToConvert)`

Convierte un valor con formato de identificador URI de datos en una cadena.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Sí |string |El valor del identificador URI para convertir. |

### <a name="return-value"></a>Valor devuelto

Una cadena que contiene el valor convertido.

### <a name="examples"></a>Ejemplos

En la plantilla de ejemplo siguiente se convierte un valor en un identificador URI de datos, y se convierte un identificador URI de datos en una cadena.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/datauri.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| dataUriOutput | String | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | String | Hola mundo. |

## <a name="empty"></a>empty

`empty(itemToTest)`

Determina si una matriz, un objeto o una cadena están vacíos.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| itemToTest |Sí |matriz, objeto o cadena |El valor para comprobar si está vacío. |

### <a name="return-value"></a>Valor devuelto

Devuelve **True** si el valor está vacío; en caso contrario, **False**.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se comprueba si una matriz, un objeto y una cadena están vacíos.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/array/empty.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| arrayEmpty | Bool | True |
| objectEmpty | Bool | True |
| stringEmpty | Bool | True |

## <a name="endswith"></a>endsWith

`endsWith(stringToSearch, stringToFind)`

Determina si una cadena termina con un valor. La comparación distingue entre mayúsculas y minúsculas.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sí |string |El valor que contiene el elemento para buscar. |
| stringToFind |Sí |string |El valor para buscar. |

### <a name="return-value"></a>Valor devuelto

**True** si el último carácter o caracteres de la cadena coinciden con el valor; en caso contrario, **False**.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo usar las funciones `startsWith` y `endsWith`:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/startsendswith.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| startsTrue | Bool | True |
| startsCapTrue | Bool | True |
| startsFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

## <a name="first"></a>first

`first(arg1)`

Devuelve el primer carácter de la cadena o el primer elemento de la matriz.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |matriz o cadena |El valor para recuperar el primer elemento o carácter. |

### <a name="return-value"></a>Valor devuelto

Una cadena del primer carácter, o el tipo (cadena, entero, matriz u objeto) del primer elemento en una matriz.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo utilizar la primera función con una matriz y una cadena.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/array/first.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| arrayOutput | String | one |
| stringOutput | String | O |

## <a name="format"></a>format

`format(formatString, arg1, arg2, ...)`

Crea una cadena con formato a partir de valores de entrada.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| formatString | Sí | string | La cadena de formato compuesta. |
| arg1 | Sí | valor booleano, entero o cadena | El valor que se va a incluir en la cadena con formato. |
| más argumentos | No | valor booleano, entero o cadena | Más valores para incluir en la cadena con formato. |

### <a name="remarks"></a>Observaciones

Utilice esta función para dar formato a una cadena en la plantilla. Usa las mismas opciones de formato que el método [System.String.Format](/dotnet/api/system.string.format) en. NET.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo usar la función format.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/format.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| formatTest | String | Hola, usuario. Número con formato: 8 175 133 |

## <a name="guid"></a>guid

`guid(baseString, ...)`

Crea un valor en el formato de un identificador único global en función de los valores proporcionados como parámetros.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| baseString |Sí |string |El valor utilizado en la función hash para crear el GUID. |
| más parámetros, según sea necesario |No |string |Puede agregar tantas cadenas como necesite para crear el valor que especifica el nivel de unicidad. |

### <a name="remarks"></a>Observaciones

Esta función es útil cuando se necesita crear un valor en el formato de un identificador único global. Proporciona valores de parámetros que limitan el ámbito de unicidad del resultado. Puede especificar si el nombre es único para la suscripción, el grupo de recursos o la implementación.

El valor devuelto no es una cadena aleatoria, sino que es el resultado de una función hash en los parámetros. El valor devuelto tiene 36 caracteres. No es único globalmente. Para crear un nuevo GUID que no se base en el valor hash de los parámetros, use la función [newGuid](#newguid).

En los ejemplos siguientes se muestra cómo utilizar un GUID para crear un valor único para niveles de uso común.

Único basado en la suscripción

```json
"[guid(subscription().subscriptionId)]"
```

Único basado en el grupo de recursos

```json
"[guid(resourceGroup().id)]"
```

Único basado en la implementación de un grupo de recursos

```json
"[guid(resourceGroup().id, deployment().name)]"
```

### <a name="return-value"></a>Valor devuelto

Una cadena que contiene 36 caracteres en el formato de un identificador único global.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se devuelven los resultados de `guid`:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/guid.json":::

## <a name="indexof"></a>indexOf

`indexOf(stringToSearch, stringToFind)`

Devuelve la primera posición de un valor dentro de una cadena. La comparación distingue entre mayúsculas y minúsculas.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sí |string |El valor que contiene el elemento para buscar. |
| stringToFind |Sí |string |El valor para buscar. |

### <a name="return-value"></a>Valor devuelto

Un entero que representa la posición del elemento que se va a buscar. El valor está basado en cero. Si no se encuentra el elemento, se devuelve -1.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo usar las funciones `indexOf` y `lastIndexOf`:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/indexof.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| firstT | Int | 0 |
| lastT | Int | 3 |
| firstString | Int | 2 |
| lastString | Int | 0 |
| notFound | Int | -1 |

<a id="json"></a>

## <a name="json"></a>json

`json(arg1)`

Convierte una cadena JSON válida en un tipo de datos JSON. Para más información, consulte [función de json](template-functions-object.md#json).

## <a name="last"></a>last

`last (arg1)`

Devuelve el último carácter de la cadena, o el último elemento de la matriz.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |matriz o cadena |El valor para recuperar el último elemento o carácter. |

### <a name="return-value"></a>Valor devuelto

Una cadena del último carácter, o el tipo (cadena, entero, matriz u objeto) del último elemento de una matriz.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo utilizar la función `last` con una matriz y una cadena.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/array/last.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| arrayOutput | String | three |
| stringOutput | String | e |

## <a name="lastindexof"></a>lastIndexOf

`lastIndexOf(stringToSearch, stringToFind)`

Devuelve la última posición de un valor dentro de una cadena. La comparación distingue entre mayúsculas y minúsculas.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sí |string |El valor que contiene el elemento para buscar. |
| stringToFind |Sí |string |El valor para buscar. |

### <a name="return-value"></a>Valor devuelto

Un entero que representa la última posición del elemento que se va a buscar. El valor está basado en cero. Si no se encuentra el elemento, se devuelve -1.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo usar las funciones `indexOf` y `lastIndexOf`:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/indexof.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| firstT | Int | 0 |
| lastT | Int | 3 |
| firstString | Int | 2 |
| lastString | Int | 0 |
| notFound | Int | -1 |

## <a name="length"></a>length

`length(string)`

Devuelve el número de caracteres en una cadena, elementos en una matriz o propiedades de nivel raíz en un objeto.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| arg1 |Sí |matriz, cadena u objeto |La matriz que se usará para obtener el número de elementos, la cadena que se usará para obtener el número de caracteres o el objeto que se usará para obtener el número de propiedades del nivel raíz. |

### <a name="return-value"></a>Valor devuelto

Un entero.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo usar la función `length` con una matriz y una cadena:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/array/length.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| arrayLength | Int | 3 |
| stringLength | Int | 13 |
| objectLength | Int | 4 |

## <a name="newguid"></a>newGuid

`newGuid()`

Devuelve un valor en el formato de un identificador único global. **Esta función solo puede utilizarse en el valor predeterminado para un parámetro.**

### <a name="remarks"></a>Observaciones

Solo puede usar esta función dentro de una expresión para el valor predeterminado de un parámetro. El uso de esta función en cualquier otro lugar de una plantilla genera un error. La función no se permite en otras partes de la plantilla porque devuelve un valor diferente cada vez que se le llama. La implementación de la misma plantilla con los mismos parámetros no produciría de forma confiable los mismos resultados.

La función newGuid difiere de la función [guid](#guid) en que no toma ningún parámetro. Cuando se llama a guid con el mismo parámetro, devuelve el mismo identificador cada vez. Use guid cuando necesite generar de forma confiable el mismo GUID para un entorno específico. Use newGuid cuando necesite un identificador diferente cada vez, como en la implementación de recursos en un entorno de prueba.

La función newGuid usa la [estructura de GUID](/dotnet/api/system.guid) en .NET Framework para generar el identificador único global.

Si usa la [opción de volver a implementar una implementación que se completó correctamente en un momento anterior](rollback-on-error.md) y esa implementación anterior incluye un parámetro que usa newGuid, el parámetro no se vuelve a evaluar. En su lugar, el valor del parámetro de la implementación anterior se reutiliza automáticamente en la implementación de reversión.

En un entorno de prueba, es posible que deba implementar repetidamente recursos que solo duran un corto tiempo. En lugar de construir nombres únicos, puede usar newGuid con [uniqueString](#uniquestring) para crear nombres únicos.

Tenga cuidado al volver a implementar una plantilla que se base en la función newGuid para un valor predeterminado. Si vuelve a implementar y no proporciona un valor para el parámetro, la función se vuelve a evaluar. Si desea actualizar un recurso existente en lugar de crear uno nuevo, pase el valor de parámetro de la implementación anterior.

### <a name="return-value"></a>Valor devuelto

Una cadena que contiene 36 caracteres en el formato de un identificador único global.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra un parámetro con un nuevo identificador.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/newguid.json":::

El resultado del ejemplo anterior varía para cada implementación, pero será similar a:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| guidOutput | string | b76a51fc-bd72-4a77-b9a2-3c29e7d2e551 |

En el ejemplo siguiente se usa la función `newGuid` para crear un nombre único para una cuenta de almacenamiento. Esta plantilla puede funcionar en el entorno de prueba donde la cuenta de almacenamiento existe durante un breve tiempo y no se vuelve a implementar.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/newguid-storageaccount.json":::

El resultado del ejemplo anterior varía para cada implementación, pero será similar a:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| nameOutput | string | storagenziwvyru7uxie |

## <a name="padleft"></a>padLeft

`padLeft(valueToPad, totalLength, paddingCharacter)`

Devuelve una cadena alineada a la derecha agregando caracteres a la izquierda hasta alcanzar la longitud total especificada.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| valueToPad |Sí |cadena o entero |Valor que se va a alinear a la derecha. |
| totalLength |Sí |int |El número total de caracteres de la cadena devuelta. |
| paddingCharacter |No |carácter individual |El carácter que se va a usar para el relleno a la izquierda hasta alcanza la longitud total. El valor predeterminado es un espacio. |

Si la cadena original es mayor que el número de caracteres que se va a rellenar, no se agrega ningún carácter.

### <a name="return-value"></a>Valor devuelto

Una cadena con al menos el número de caracteres especificados.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo rellenar el valor del parámetro proporcionado por el usuario agregando el carácter cero hasta que alcance el número total de caracteres.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/padleft.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| stringOutput | String | 0000000123 |

## <a name="replace"></a>replace

`replace(originalString, oldString, newString)`

Devuelve una nueva cadena con todas las instancias de una cadena reemplazadas por otra cadena.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| originalString |Sí |string |Valor que tiene todas las instancias de una cadena reemplazadas por otra cadena. |
| oldString |Sí |string |Cadena que se va a quitar de la cadena original. |
| newString |Sí |string |La cadena que se va a agregar en lugar de la cadena eliminada. |

### <a name="return-value"></a>Valor devuelto

Una cadena con los caracteres reemplazados.

### <a name="examples"></a>Ejemplos

El ejemplo siguiente muestra cómo quitar todos los guiones de la cadena proporcionada por el usuario y cómo reemplazar parte de la cadena por otra cadena.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/replace.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| firstOutput | String | 1231231234 |
| secondOutput | String | 123-123-xxxx |

## <a name="skip"></a>skip

`skip(originalValue, numberToSkip)`

Devuelve una cadena con todos los caracteres después del número especificado de caracteres, o una matriz con todos los elementos después del número especificado de elementos.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| originalValue |Sí |matriz o cadena |La matriz o cadena que se usará para la omisión. |
| numberToSkip |Sí |int |El número de elementos o caracteres que se van a omitir. Si este valor es 0 o un valor inferior, se devuelven todos los elementos o caracteres del valor. Si es mayor que la longitud de la matriz o la cadena, se devuelve una matriz o cadena vacía. |

### <a name="return-value"></a>Valor devuelto

Una matriz o cadena.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se omite el número especificado de elementos de la matriz, y el número especificado de caracteres de la cadena.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/array/skip.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| arrayOutput | Array | ["three"] |
| stringOutput | String | two three |

## <a name="split"></a>split

`split(inputString, delimiter)`

Devuelve una matriz de cadenas que contiene las subcadenas de la cadena de entrada que están delimitadas por los delimitadores especificados.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| inputString |Sí |string |La cadena que se va a dividir. |
| delimiter |Sí |cadena o matriz de cadenas |Delimitador que se utilizará para dividir la cadena. |

### <a name="return-value"></a>Valor devuelto

Una matriz de cadenas.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se divide la cadena de entrada con una coma, y con una coma o un punto y coma.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/split.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| firstOutput | Array | ["one", "two", "three"] |
| secondOutput | Array | ["one", "two", "three"] |

## <a name="startswith"></a>startsWith

`startsWith(stringToSearch, stringToFind)`

Determina si una cadena empieza con un valor. La comparación distingue entre mayúsculas y minúsculas.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sí |string |El valor que contiene el elemento para buscar. |
| stringToFind |Sí |string |El valor para buscar. |

### <a name="return-value"></a>Valor devuelto

**True** si el primer carácter o caracteres de la cadena coinciden con el valor; en caso contrario, **False**.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo usar las funciones `startsWith` y `endsWith`:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/startsendswith.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| startsTrue | Bool | True |
| startsCapTrue | Bool | True |
| startsFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

## <a name="string"></a>string

`string(valueToConvert)`

Convierte el valor especificado a una cadena.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| valueToConvert |Sí | Any |El valor que se convierte en cadena. Se puede convertir cualquier tipo de valor, incluidos objetos y matrices. |

### <a name="return-value"></a>Valor devuelto

Cadena del valor convertido.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo convertir distintos tipos de valores en cadenas.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/string.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| objectOutput | String | {"valueA":10,"valueB":"Example Text"} |
| arrayOutput | String | ["a","b","c"] |
| intOutput | String | 5 |

## <a name="substring"></a>substring

`substring(stringToParse, startIndex, length)`

Devuelve una subcadena que empieza en la posición de carácter especificada y que contiene el número especificado de caracteres.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToParse |Sí |string |La cadena original desde la que se extrae la subcadena. |
| startIndex |No |int |La posición de carácter inicial basado en cero de la subcadena. |
| length |No |int |El número de caracteres de la subcadena. Debe hacer referencia a una ubicación dentro de la cadena. Debe ser cero o mayor. Si se omite, se devolverá el resto de la cadena desde la posición inicial.|

### <a name="return-value"></a>Valor devuelto

Subcadena. O bien, una cadena vacía si la longitud es cero.

### <a name="remarks"></a>Observaciones

La función genera un error cuando la subcadena supera el final de la cadena, o bien cuando la longitud es menor que cero. En el ejemplo siguiente se produce el error "Los parámetros index y length deben hacer referencia a una ubicación dentro de la cadena. El parámetro index: "0", el parámetro length: "11", la longitud del parámetro string: "10".

```json
"parameters": {
  "inputString": {
    "type": "string",
    "value": "1234567890"
  }
}, "variables": {
  "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se extrae una subcadena de un parámetro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/substring.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| substringOutput | String | two |

## <a name="take"></a>take

`take(originalValue, numberToTake)`

Devuelve una matriz o una cadena. Una matriz tiene el número especificado de elementos desde el principio de la matriz. Una cadena tiene el número especificado de caracteres desde el principio de la cadena.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| originalValue |Sí |matriz o cadena |La matriz o cadena de la que se van a tomar los elementos. |
| numberToTake |Sí |int |El número de elementos o caracteres que se van a tomar. Si este valor es 0 o un valor inferior, se devolverá una matriz o cadena vacía. Si es mayor que la longitud de la matriz o cadena especificada, se devuelven todos los elementos de la matriz o cadena. |

### <a name="return-value"></a>Valor devuelto

Una matriz o cadena.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se toma el número especificado de elementos de la matriz y de caracteres de la cadena.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/array/take.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| arrayOutput | Array | ["one", "two"] |
| stringOutput | String | en |

## <a name="tolower"></a>toLower

`toLower(stringToChange)`

Convierte la cadena especificada a minúsculas.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToChange |Sí |string |Valor que se va a convertir a minúsculas. |

### <a name="return-value"></a>Valor devuelto

Cadena convertida a minúsculas.

### <a name="examples"></a>Ejemplos

En el siguiente ejemplo se convierte un valor de parámetro a minúsculas y a mayúsculas.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/tolower.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| toLowerOutput | String | one two three |
| toUpperOutput | String | ONE TWO THREE |

## <a name="toupper"></a>toUpper

`toUpper(stringToChange)`

Convierte la cadena especificada a mayúsculas.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToChange |Sí |string |Valor que se va a convertir a mayúsculas. |

### <a name="return-value"></a>Valor devuelto

Cadena convertida a mayúsculas.

### <a name="examples"></a>Ejemplos

En el siguiente ejemplo se convierte un valor de parámetro a minúsculas y a mayúsculas.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/tolower.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| toLowerOutput | String | one two three |
| toUpperOutput | String | ONE TWO THREE |

## <a name="trim"></a>trim

`trim (stringToTrim)`

Quita todos los caracteres de espacio en blanco iniciales y finales de la cadena especificada.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToTrim |Sí |string |Valor que se recortará. |

### <a name="return-value"></a>Valor devuelto

Cadena sin caracteres de espacio en blanco iniciales ni finales.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se recortan los caracteres de espacio en blanco del parámetro.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/trim.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| return | String | one two three |

## <a name="uniquestring"></a>uniqueString

`uniqueString (baseString, ...)`

Crea una cadena de hash determinista basada en los valores proporcionados como parámetros.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| baseString |Sí |string |Valor utilizado en la función hash para crear una cadena única. |
| más parámetros, según sea necesario |No |string |Puede agregar tantas cadenas como necesite para crear el valor que especifica el nivel de unicidad. |

### <a name="remarks"></a>Observaciones

Esta función es útil cuando se debe crear un nombre único para un recurso. Proporciona valores de parámetros que limitan el ámbito de unicidad del resultado. Puede especificar si el nombre es único para la suscripción, el grupo de recursos o la implementación.

El valor devuelto no es una cadena aleatoria, sino que es el resultado de una función hash. El valor devuelto tiene 13 caracteres. No es único globalmente. Puede que desee combinar el valor con un prefijo de su convención de nomenclatura para crear un nombre que sea más fácil de reconocer. En el ejemplo siguiente se muestra el formato del valor devuelto. El valor real varía según los parámetros proporcionados.

`tcvhiyu5h2o5o`

En los ejemplos siguientes se muestra cómo utilizar `uniqueString` para crear un valor único para niveles de uso común.

Único basado en la suscripción

```json
"[uniqueString(subscription().subscriptionId)]"
```

Único basado en el grupo de recursos

```json
"[uniqueString(resourceGroup().id)]"
```

Único basado en la implementación de un grupo de recursos

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

En el ejemplo siguiente se muestra cómo crear un nombre único para una cuenta de almacenamiento basada en el grupo de recursos. Dentro del grupo de recursos, el nombre no es único si se crea de la misma manera.

```json
"resources": [{
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  "type": "Microsoft.Storage/storageAccounts",
  ...
```

Si necesita crear un nuevo nombre único cada vez que implemente una plantilla y no tiene intención de actualizar el recurso, puede usar la función [utcNow](template-functions-date.md#utcnow) con `uniqueString`. Podría utilizar este enfoque en un entorno de prueba. Para ver un ejemplo, consulte [utcNow](template-functions-date.md#utcnow).

### <a name="return-value"></a>Valor devuelto

Cadena que contiene 13 caracteres.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se devuelven los resultados de `uniquestring`:

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/uniquestring.json":::

## <a name="uri"></a>uri

`uri (baseUri, relativeUri)`

Crea un URI absoluto mediante la combinación de la cadena de relativeUri y baseUri.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| baseUri |Sí |string |La cadena de uri base. Preste atención para observar el comportamiento relacionado con el control de la barra diagonal final (`/`), tal y como se describe a continuación en esta tabla.  |
| relativeUri |Sí |string |La cadena de uri relativo que se agregará a la cadena de uri base. |

* Si **baseUri** termina en una barra diagonal final, el resultado es **baseUri** seguido de **relativeUri**.

* Si **baseUri** no termina en una barra diagonal final, pueden pasar una de dos cosas.

   * Si **baseUri** no tiene barras diagonales (aparte de `//` al principio), el resultado es **baseUri** seguido de **relativeUri**.

   * Si **baseUri** tiene algunas barras diagonales, pero no termina con una barra diagonal, se elimina todo lo que hay a partir de la última barra diagonal de **baseUri** y el resultado es **baseUri** seguido de **relativeUri**.

Estos son algunos ejemplos:

```
uri('http://contoso.org/firstpath', 'myscript.sh') -> http://contoso.org/myscript.sh
uri('http://contoso.org/firstpath/', 'myscript.sh') -> http://contoso.org/firstpath/myscript.sh
uri('http://contoso.org/firstpath/azuredeploy.json', 'myscript.sh') -> http://contoso.org/firstpath/myscript.sh
uri('http://contoso.org/firstpath/azuredeploy.json/', 'myscript.sh') -> http://contoso.org/firstpath/azuredeploy.json/myscript.sh
```

Para obtener detalles completos, los parámetros **baseUri** y **relativeUri** se resuelven como se especifica en [la sección 5 de la especificación RFC 3986](https://tools.ietf.org/html/rfc3986#section-5).

### <a name="return-value"></a>Valor devuelto

Una cadena que representa el identificador URI absoluto para los valores base y relativos.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente se muestra cómo construir un vínculo a una plantilla anidada en función del valor de la plantilla principal.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

En la plantilla de ejemplo siguiente se muestra cómo usar `uri`, `uriComponent` y `uriComponentToString`.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/uri.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| uriOutput | String | `http://contoso.com/resources/nested/azuredeploy.json` |
| componentOutput | String | `http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json` |
| toStringOutput | String | `http://contoso.com/resources/nested/azuredeploy.json` |

## <a name="uricomponent"></a>uriComponent

`uricomponent(stringToEncode)`

Codifica un identificador URI.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| stringToEncode |Sí |string |El valor para codificar. |

### <a name="return-value"></a>Valor devuelto

Una cadena del valor codificado por el identificador URI.

### <a name="examples"></a>Ejemplos

En la plantilla de ejemplo siguiente se muestra cómo usar `uri`, `uriComponent` y `uriComponentToString`.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/uri.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| uriOutput | String | `http://contoso.com/resources/nested/azuredeploy.json` |
| componentOutput | String | `http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json` |
| toStringOutput | String | `http://contoso.com/resources/nested/azuredeploy.json` |

## <a name="uricomponenttostring"></a>uriComponentToString

`uriComponentToString(uriEncodedString)`

Devuelve una cadena del valor codificado por el identificador URI.

### <a name="parameters"></a>Parámetros

| Parámetro | Obligatorio | Tipo | Descripción |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Sí |string |El valor codificado por el identificador URI para convertir en una cadena. |

### <a name="return-value"></a>Valor devuelto

Una cadena descodificada del valor codificado por el identificador URI.

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente, se muestra cómo usar `uri`, `uriComponent` y `uriComponentToString`.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/functions/string/uri.json":::

La salida del ejemplo anterior con el valor predeterminado es:

| Nombre | Tipo | Value |
| ---- | ---- | ----- |
| uriOutput | String | `http://contoso.com/resources/nested/azuredeploy.json` |
| componentOutput | String | `http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json` |
| toStringOutput | String | `http://contoso.com/resources/nested/azuredeploy.json` |

## <a name="next-steps"></a>Pasos siguientes

* Puede encontrar una descripción de las secciones de una plantilla de Azure Resource Manager en [Nociones sobre la estructura y la sintaxis de las plantillas de Resource Manager](./syntax.md).
* Para combinar varias plantillas, consulte [Uso de plantillas vinculadas y anidadas al implementar recursos de Azure](linked-templates.md).
* Para iterar un número especificado de veces al crear un tipo de recurso, consulte [Iteración de recursos en las plantillas de Resource Manager](copy-resources.md).
* Para ver cómo implementar la plantilla que ha creado, consulte [Implementación de recursos con plantillas de Resource Manager y Azure PowerShell](deploy-powershell.md).
