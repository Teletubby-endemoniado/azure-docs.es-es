---
title: Referencia de funciones y expresiones de sincronización en la nube de Azure AD Connect
description: reference
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd08de2e826b4fab2d8c172057afc149ea32efc0
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131451775"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Escritura de expresiones para la asignación de atributos en Azure Active Directory
Al configurar la sincronización en la nube, uno de los tipos de asignaciones de atributos que se pueden especificar es una asignación de expresiones. 

La asignación de expresiones permite personalizar atributos mediante una expresión similar a un script.  Esto permite transformar los datos locales en un valor nuevo o diferente.  Por ejemplo, puede que desee combinar dos atributos para formar uno solo, ya que este atributo lo usa una de las aplicaciones en la nube.

En el siguiente documento se tratarán las expresiones similares a scripts que se usan para transformar los datos.  Esto es solo una parte del proceso.  A continuación, deberá usar esta expresión y colocarla en una solicitud web para el inquilino.  Para más información al respecto, consulte [Transformaciones](how-to-transformation.md)

## <a name="syntax-overview"></a>Información general sobre la sintaxis
La sintaxis de expresiones para asignaciones de atributos recuerda a las funciones de Visual Basic para Aplicaciones (VBA).

* Toda la expresión se tiene que definir en términos de funciones, que constan de un nombre seguido de argumentos entre paréntesis: <br>
  *FunctionName(`<<argument 1>>`,`<<argument N>>`)*
* Es posible anidar funciones dentro de otras. Por ejemplo: <br> *FunctionOne(FunctionTwo(`<<argument1>>`))*
* Puede transformar tres tipos diferentes de argumentos en funciones:
  
  1. Atributos, que deben ir entre corchetes. Por ejemplo: [NombreAtributo]
  2. Constantes de cadena, que deben ir entre comillas. Por ejemplo: "Estados Unidos"
  3. Otras funciones. Por ejemplo: FunctionOne(`<<argument1>>`, FunctionTwo(`<<argument2>>`))
* Para las constantes de cadena, si necesita una barra diagonal inversa (\) o comillas dobles (") en la cadena, se deben convertirse con el símbolo de barra diagonal inversa (\). Por ejemplo: "Nombre de la empresa: \\"Contoso\\""

## <a name="list-of-functions"></a>Lista de funciones
| Lista de funciones | Descripción |
|-----|----|
|[Append](#append)|adopta un valor de la cadena de origen y anexa el sufijo al final de la misma.|
|[BitAnd](#bitand)|: la función BitAnd establece los bits especificados en un valor.|
|[CBool](#cbool)|: la función CBool devuelve un valor booleano basado en la expresión evaluada.|
|[ConvertFromBase64](#convertfrombase64)|: la función ConvertFromBase64 convierte el valor codificado en base64 especificado en una cadena normal.|
|[ConvertToBase64](#converttobase64)|La función ConvertToBase64 convierte una cadena en una en base64 de Unicode. |
|[ConvertToUTF8Hex](#converttoutf8hex)|: la función ConvertToUTF8Hex convierte una cadena en un valor codificado hexadecimal UTF8.|
|[Recuento](#count)|: la función Count devuelve el número de elementos en un atributo multivalor.|
|[Cstr](#cstr)|: la función CStr se convierte en un tipo de datos de cadena.|
|[DateFromNum](#datefromnum)|: la función DateFromNum convierte un valor con formato de fecha de AD en un tipo DateTime.|
|[DNComponent](#dncomponent)|: la función DNComponent devuelve el valor de un componente DN especificado empezando por la izquierda.|
|[Error](#error)|: la función Error se usa para devolver un error personalizado.|
|[FormatDateTime](#formatdatetime) |adopta una cadena de fecha en un formato y la convierte a un formato distinto.| 
|[GUID](#guid)|La función GUID genera un identificador único global aleatorio.|           
|[IIF](#iif)|: la función IIF devuelve uno de un conjunto de valores posibles según una condición especificada.|
|[InStr](#instr)|La función InStr busca la primera repetición de una subcadena en una cadena.|
|[IsNull](#isnull)|si la expresión se evalúa como Null, la función IsNull devuelve True.|
|[IsNullOrEmpty](#isnullorempty)|si la expresión es Null o una cadena vacía, la función IsNullOrEmpty devuelve True.|         
|[IsPresent](#ispresent)|si la expresión se evalúa como una cadena que no es Null y no está vacía, la función IsPresent devuelve True.|    
|[IsString](#isstring)|si la expresión se puede evaluar en un tipo de cadena, la función IsString se evalúa en True.|
|[Elemento](#item)|la función Item devuelve un elemento de un atributo o una cadena de varios valores.|
|[Join](#join) |Join() es similar a Append(), excepto en que puede combinar varios valores de cadena de **source** en una sola cadena, y cada valor estará separado por una cadena de **separator**.| 
|[Left](#left)|la función Left devuelve un número especificado de caracteres desde la izquierda de una cadena.|
|[Mid](#mid) |devuelve una subcadena del valor de origen. Una subcadena es una cadena que contiene sólo algunos de los caracteres de la cadena de origen.|
|[NormalizeDiacritics](#normalizediacritics)|Requiere un argumento de cadena. Devuelve la cadena, pero se reemplazan todos los caracteres diacríticos por sus equivalentes no diacríticos.|
|[Not](#not) |Invierte el valor booleano de **source**. Si el valor de **source** es "*True*", devuelve "*False*". De lo contrario, devuelve "*True*".| 
|[RemoveDuplicates](#removeduplicates)|la función RemoveDuplicates toma una cadena multivalor y garantiza que cada valor es único.| 
|[Sustituya](#replace) |Reemplaza valores dentro de una cadena. | 
|[SelectUniqueValue](#selectuniquevalue)|Requiere dos argumentos como mínimo, que son las reglas de generación de valor único definidas con expresiones. La función evalúa cada regla y, a continuación, comprueba la unicidad del valor generado en el directorio o la aplicación de destino.| 
|[SingleAppRoleAssignment](#singleapproleassignment)|Devuelve una única función appRoleAssignment de la lista de todas las funciones appRoleAssignments asignadas a un usuario para una aplicación determinada.| 
|[Dividir](#split)|divide una cadena en una matriz de varios valores, usando el carácter delimitador especificado.|
|[StringFromSID](#stringfromsid)|la función StringFromSid convierte una matriz de bytes o una matriz de bytes multivalor que contiene un identificador de seguridad en una cadena o en una cadena multivalor.| 
|[StripSpaces](#stripspaces) |quita todos los caracteres de espacio (" ") de la cadena de origen.| 
|[Switch](#switch)|Cuando el valor de **source** coincide con una **key**, devuelve el **value** de dicha **key**. | 
|[ToLower](#tolower)|Toma un valor de cadena de *origen* y se convierte a minúsculas mediante las reglas de referencia cultural que se hayan especificado.| 
|[ToUpper](#toupper)|Toma un valor de cadena de *origen* y se convierte a mayúsculas mediante las reglas de referencia cultural que se hayan especificado.|
|[Trim](#trim)|la función Trim quita los espacios en blanco del principio y del final de una cadena.|
|[Word](#word)|: la función Word devuelve una palabra incluida en una cadena, según los parámetros que describen los delimitadores que se usarán y el número de palabras que se devolverán.|

---
### <a name="append"></a>Append
**Función:**<br> Append(source, suffix)

**Descripción:**<br> adopta un valor de la cadena de origen y anexa el sufijo al final de la misma.

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto de origen. |
   | **suffix** |Obligatorio |String |La cadena que se va a anexar al final del valor de origen. |

---
### <a name="bitand"></a>BitAnd
**Descripción:**  
: la función BitAnd establece los bits especificados en un valor.

**Sintaxis:**  
`num BitAnd(num value1, num value2)`

* value1, value2: valores numéricos que deberían estar unidos por el operador AND

**Observaciones**:  
: esta función convierte ambos parámetros en la representación binaria y establece un bit en los siguientes valores:

* 0: si uno o ambos de los bits correspondientes en *value1* y *value2* son 0.
* 1: si ambos de los bits correspondientes son 1.

En otras palabras, devuelve 0 en todos los casos excepto cuando los bits correspondientes de los dos parámetros son 1.

**Ejemplo**:  
 
 `BitAnd(&HF, &HF7)`</br>
 devuelve 7 porque los valores hexadecimales "F" y "F7" se evalúan en este valor.

---

### <a name="cbool"></a>CBool
**Descripción:**  
: la función CBool devuelve un valor booleano basado en la expresión evaluada.

**Sintaxis:**  
`bool CBool(exp Expression)`

**Observaciones**:  
Si la expresión se evalúa en un valor distinto de cero, CBool devuelve True. En caso contrario, devuelve False.

**Ejemplo**:  
`CBool([attrib1] = [attrib2])`  

Devuelve True si ambos atributos tienen el mismo valor.

---
### <a name="convertfrombase64"></a>ConvertFromBase64
**Descripción:**  
: la función ConvertFromBase64 convierte el valor codificado en base64 especificado en una cadena normal.

**Sintaxis:**  
`str ConvertFromBase64(str source)` da por hecho que se usará Unicode para la codificación.  
`str ConvertFromBase64(str source, enum Encoding)`

* source: cadena codificada en Base64  
* Encoding: Unicode, ASCII, UTF8

**Ejemplo**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Ambos ejemplos devuelven "*Hola a todos*"

---
### <a name="converttobase64"></a>ConvertToBase64
**Descripción:**  
La función ConvertToBase64 convierte una cadena en una en base64 de Unicode.  
Convierte el valor de una matriz de enteros en su representación de cadena equivalente que se codifica con dígitos de base 64.

**Sintaxis:**  
`str ConvertToBase64(str source)`

**Ejemplo**:  
`ConvertToBase64("Hello world!")`  
devuelve "SABlAGwAbABvACAAdwBvAHIAbABkACEA".

---
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Descripción:**  
: la función ConvertToUTF8Hex convierte una cadena en un valor codificado hexadecimal UTF8.

**Sintaxis:**  
`str ConvertToUTF8Hex(str source)`

**Observaciones**:  
: Azure Active Directory usa el formato de salida de esta función como formato de atributo DN.

**Ejemplo**:  
`ConvertToUTF8Hex("Hello world!")`  
devuelve 48656C6C6F20776F726C6421.

---
### <a name="count"></a>Count
**Descripción:**  
: la función Count devuelve el número de elementos en un atributo multivalor.

**Sintaxis:**  
`num Count(mvstr attribute)`

---
### <a name="cstr"></a>CStr
**Descripción:**  
: la función CStr se convierte en un tipo de datos de cadena.

**Sintaxis:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* value: puede ser un valor numérico, un atributo de referencia o un valor booleano.

**Ejemplo**:  
`CStr([dn])`  
podría devolver "cn=Joe,dc=contoso,dc=com"

---
### <a name="datefromnum"></a>DateFromNum
**Descripción:**  
: la función DateFromNum convierte un valor con formato de fecha de AD en un tipo DateTime.

**Sintaxis:**  
`dt DateFromNum(num value)`

**Ejemplo**:  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
devuelve un valor DateTime que representa 2012-01-01 23:00:00.

---
### <a name="dncomponent"></a>DNComponent
**Descripción:**  
: la función DNComponent devuelve el valor de un componente DN especificado empezando por la izquierda.

**Sintaxis:**  
`str DNComponent(ref dn, num ComponentNumber)`

* dn: el atributo de referencia que interpretar
* ComponentNumber: el componente en DN para devolver

**Ejemplo**:  
`DNComponent(CRef([dn]),1)`  
si dn es "cn=Joe,ou=…", devuelve Joe.

---
### <a name="error"></a>Error
**Descripción:**  
: la función Error se usa para devolver un error personalizado.

**Sintaxis:**  
`void Error(str ErrorMessage)`

**Ejemplo**:  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
si el atributo accountName no está presente, se produce un error en el objeto.

---
### <a name="formatdatetime"></a>FormatDateTime
**Función:**<br> FormatDateTime(source, inputFormat, outputFormat)

**Descripción:**<br> adopta una cadena de fecha en un formato y la convierte a un formato distinto.

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto de origen. |
   | **inputFormat** |Obligatorio |String |Formato esperado del valor de origen. Para conocer los formatos admitidos, consulte [/dotnet/standard/base-types/custom-date-and-time-format-strings](/dotnet/standard/base-types/custom-date-and-time-format-strings). |
   | **outputFormat** |Obligatorio |String |Formato de la fecha de salida. |

---
### <a name="guid"></a>Guid
**Descripción:**  
La función GUID genera un nuevo GUID aleatorio.

**Sintaxis:**  
`str Guid()`

---
### <a name="iif"></a>IIF
**Descripción:**  
: la función IIF devuelve uno de un conjunto de valores posibles según una condición especificada.

**Sintaxis:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* condition: cualquier valor o expresión que pueda evaluarse en True o False.
* valueIfTrue: si la condición se evalúa como true, el valor devuelto.
* valueIfFalse: si la condición se evalúa como false, el valor devuelto.

**Ejemplo**:  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Si el usuario está en prácticas, devuelve el alias de un usuario al que agrega "t-" al principio. De lo contrario, el alias del usuario se queda como está.

---
### <a name="instr"></a>InStr
**Descripción:**  
: la función InStr busca la primera repetición de una subcadena en una cadena.

**Sintaxis:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start, enum compare)`

* stringcheck: cadena que se va a buscar
* stringmatch: cadena que se tiene que encontrar
* start: posición de inicio para encontrar la subcadena
* compare: vbTextCompare o vbBinaryCompare

**Observaciones**:  
: devuelve la posición en la que se encontró la subcadena o 0 si no se encuentra.

**Ejemplo**:  
`InStr("The quick brown fox","quick")`  
se evalúa en 5.

`InStr("repEated","e",3,vbBinaryCompare)`  
Se evalúa en 7.

---
### <a name="isnull"></a>IsNull
**Descripción:**  
si la expresión se evalúa como Null, la función IsNull devuelve True.

**Sintaxis:**  
`bool IsNull(var Expression)`

**Observaciones**:  
para un atributo, un valor Null se expresa mediante la ausencia del atributo.

**Ejemplo**:  
`IsNull([displayName])`  
devuelve True si el atributo no está presente en CS o MV.

---
### <a name="isnullorempty"></a>IsNullOrEmpty
**Descripción:**  
si la expresión es Null o una cadena vacía, la función IsNullOrEmpty devuelve True.

**Sintaxis:**  
`bool IsNullOrEmpty(var Expression)`

**Observaciones**:  
para un atributo, esto se evaluaría como True si el atributo no está presente o está presente, pero es una cadena vacía.  
La función contraria a esta es IsPresent.

**Ejemplo**:  
`IsNullOrEmpty([displayName])`  
devuelve True si el atributo no está presente o si es una cadena vacía en CS o MV.

---
### <a name="ispresent"></a>IsPresent
**Descripción:**  
si la expresión se evalúa como una cadena que no es Null y no está vacía, la función IsPresent devuelve True.

**Sintaxis:**  
`bool IsPresent(var expression)`

**Observaciones**:  
la función contraria a esta es IsNullOrEmpty.

**Ejemplo**:  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

---
### <a name="item"></a>Elemento
**Descripción:**  
la función Item devuelve un elemento de un atributo o una cadena de varios valores.

**Sintaxis:**  
`var Item(mvstr attribute, num index)`

* attribute: atributo de varios valores
* index: índice para un elemento en la cadena de varios valores.

**Observaciones**:  
la función Item es útil con la función Contains puesto que esta última función devuelve el índice a un elemento en el atributo de varios valores.

Se produce un error si el índice está fuera de los límites.

**Ejemplo**:  
`Mid(Item([proxyAddresses],Contains([proxyAddresses], "SMTP:")),6)`  
devuelve la dirección de correo electrónico principal.

---
### <a name="isstring"></a>IsString
**Descripción:**  
si la expresión se puede evaluar en un tipo de cadena, la función IsString se evalúa en True.

**Sintaxis:**  
`bool IsString(var expression)`

**Observaciones**:  
se usa para determinar si CStr() puede analizar correctamente la expresión.

---
### <a name="join"></a>Join
**Función:**<br> Join(separator, source1, source2, …)

**Descripción:**<br> Join() es similar a Append(), excepto en que puede combinar varios valores de cadena de **source** en una sola cadena, y cada valor estará separado por una cadena de **separator**.

Si uno de los valores de origen es un atributo multivalor, cada valor de ese atributo se unirá, separado por el valor del separador.

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **separator** |Obligatorio |String |Cadena utilizada para separar los valores de origen cuando se concatenan en una sola cadena. Puede ser "" si no es necesario ningún separador. |
   | **source1  … sourceN** |Obligatorio, número variable de veces |String |Valores de cadena que se van a agrupar. |

---
### <a name="left"></a>Left
**Descripción:**  
la función Left devuelve un número especificado de caracteres desde la izquierda de una cadena.

**Sintaxis:**  
`str Left(str string, num NumChars)`

* string: la cadena desde la que devolver los caracteres
* NumChars: un número que identifica el número de caracteres que devolver desde el principio (izquierdo) de la cadena

**Observaciones**:  
una cadena que contiene los primeros caracteres numChars de la cadena:

* Con numChars = 0, se devuelve una cadena vacía.
* Con numChars < 0, se devuelve una cadena de entrada.
* Si la cadena es null, se devuelve una cadena vacía.

Si la cadena contiene menos caracteres que el número especificado en numChars, se devuelve una cadena idéntica a la cadena (es decir, que contiene todos los caracteres en el parámetro 1).

**Ejemplo**:  
`Left("John Doe", 3)`  
Devuelve `Joh`.

---
### <a name="mid"></a>Mid
**Función:**<br> Mid(source, start, length)

**Descripción:**<br> devuelve una subcadena del valor de origen. Una subcadena es una cadena que contiene sólo algunos de los caracteres de la cadena de origen.

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |String |Normalmente el nombre del atributo. |
   | **start** |Obligatorio |integer |Índice de la cadena de **source** donde debe empezar la subcadena. El primer carácter de la cadena tendrá el índice de 1, el segundo carácter tendrá el índice de 2, y así sucesivamente. |
   | **length** |Obligatorio |integer |Longitud de la subcadena. Si length acaba fuera de la cadena de **source**, la función devolverá una subcadena desde el índice de **start** hasta el final de la cadena de **source**. |

---
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Función:**<br> NormalizeDiacritics(source)

**Descripción:**<br> Requiere un argumento de cadena. Devuelve la cadena, pero se reemplazan todos los caracteres diacríticos por sus equivalentes no diacríticos. Normalmente se usa para convertir nombres y apellidos que contienen caracteres diacríticos (acentos) en valores legales que se pueden emplear en diversos identificadores de usuario como, nombres principales de usuario, nombres de cuenta de SAM y direcciones de correo electrónico.

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |String | Normalmente un atributo de nombre o de apellido. |

---
### <a name="not"></a>Not
**Función:**<br> Not(source)

**Descripción:**<br> Invierte el valor booleano de **source**. Si el valor de **source** es "*True*", devuelve "*False*". De lo contrario, devuelve "*True*".

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |Cadena booleana |Los valores de **source** esperados son "True" o "False". |

---
### <a name="removeduplicates"></a>RemoveDuplicates
**Descripción:**  
la función RemoveDuplicates toma una cadena multivalor y garantiza que cada valor es único.

**Sintaxis:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Ejemplo**:  
`RemoveDuplicates([proxyAddresses])`  
devuelve un atributo proxyAddress saneado donde se han quitado todos los valores duplicados.

---
### <a name="replace"></a>Replace
**Función:**<br> Replace(source, oldValue, regexPattern, regexGroupName, replacementValue, replacementAttributeName, template)

**Descripción:**<br>
Reemplaza valores dentro de una cadena. Funciona de forma diferente dependiendo de los parámetros proporcionados:

* Cuando se proporcionan **oldValue** y **replacementValue**:
  
  * Reemplaza todas las ocurrencias de **oldValue** en el **origen** por **replacementValue**
* Cuando se proporcionan **oldValue** y **template**:
  
  * Reemplaza todas las ocurrencias de **oldValue** en **template** por el valor de **source**
* Cuando se proporcionan **regexPattern** y **replacementValue**:

  * La función aplica **regexPattern** a la cadena de **source**, y usted puede usar los nombres de grupo regex para construir la cadena para **replacementValue**
* Cuando se proporcionan **regexPattern**, **regexGroupName** y **replacementValue**:
  
  * La función aplica **regexPattern** a la cadena de **source** y reemplaza todos los valores que coinciden con **regexGroupName** por **replacementValue**
* Cuando se proporcionan **regexPattern**, **regexGroupName** y **replacementAttributeName**:
  
  * Si **source** no tiene un valor, se devuelve **source**.
  * Si **source** tiene un valor, la función aplica **regexPattern** a la cadena de **source** y reemplaza todos los valores coincidentes de **regexGroupName** por el valor asociado con **replacementAttributeName**

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto **source**. |
   | **oldValue** |Opcional |String |Valor que se va a reemplazar en **source** o **template**. |
   | **regexPattern** |Opcional |String |Patrón Regex del valor que se va a reemplazar en **source**. O bien, cuando se usa **replacementPropertyName**, patrón para extraer el valor de la propiedad **replacementPropertyName**. |
   | **regexGroupName** |Opcional |String |Nombre del grupo dentro de **regexPattern**. Solo cuando se usa **replacementPropertyName**, extraeremos el valor de este grupo como **replacementValue** de **replacementPropertyName**. |
   | **replacementValue** |Opcional |String |Nuevo valor para  reemplazar uno anterior. |
   | **replacementAttributeName** |Opcional |String |Nombre del atributo que se usará para el valor de reemplazo |
   | **template** |Opcional |String |Cuando se proporcione el valor de **template**, buscaremos **oldValue** dentro de la plantilla y lo reemplazaremos por el valor de **source**. |

---
### <a name="selectuniquevalue"></a>SelectUniqueValue
**Función:**<br> SelectUniqueValue(uniqueValueRule1, uniqueValueRule2, uniqueValueRule3, …)

**Descripción:**<br> Requiere dos argumentos como mínimo, que son las reglas de generación de valor único definidas con expresiones. La función evalúa cada regla y, a continuación, comprueba la unicidad del valor generado en el directorio o la aplicación de destino. Se devolverá el primer valor único encontrado. Si todos los valores ya existen en el destino, la entrada se depositará y el motivo se anota en los registros de auditoría. No hay ningún límite superior para el número de argumentos que se pueden proporcionar.

> [!NOTE]
> - Esta es una función de nivel superior, no se puede anidar.
> - Esta función no se puede aplicar a los atributos que tienen una precedencia de coincidencia.  
> - Esta función solo está destinada a usarse para creaciones de entradas. Al usarla con un atributo, establezca la propiedad **Apply Mapping** (Aplicar asignación) en **Solo durante la creación del objeto**.
> - Esta función solo se admite actualmente para "WorkDay para el aprovisionamiento de usuarios de Active Directory". No se puede usar con otras aplicaciones de aprovisionamiento. 


**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **uniqueValueRule1  … uniqueValueRuleN** |Al menos se requieren dos, sin límite superior |String | Lista de reglas de generación de valor único para realizar la evaluación. |


---
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Función:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Descripción:**<br> Devuelve una única función appRoleAssignment de la lista de todas las funciones appRoleAssignments asignadas a un usuario para una aplicación determinada. Esta función es necesaria para convertir el objeto appRoleAssignments en una cadena de nombre de rol único. Tenga en cuenta que el procedimiento recomendado consiste en asegurarse de que solo una función appRoleAssignment esté asignada a un usuario a la vez. Si se asignan varios roles, la cadena del rol devuelta podría no ser predecible. 

**Parámetros:**<br> 

  | Nombre | Obligatorio/Repetición | Tipo | Notas |
  |--- | --- | --- | --- |
  | **[appRoleAssignments]** |Obligatorio |String |Objeto **[appRoleAssignments]** . |

---
### <a name="split"></a>Dividir
**Función:**<br> Split(source, delimiter)

**Descripción:**<br> divide una cadena en una matriz de varios valores, usando el carácter delimitador especificado.

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |String |**de origen** que se actualiza. |
   | **delimitador** |Obligatorio |String |Especifica el carácter que se usará para dividir la cadena (ejemplo: ","). |

---
### <a name="stringfromsid"></a>StringFromSid
**Descripción:**  
la función StringFromSid convierte una matriz de bytes o una matriz de bytes multivalor que contiene un identificador de seguridad en una cadena o en una cadena multivalor.

**Sintaxis:**  
`str StringFromSid(bin ObjectSID)`  

---
### <a name="stripspaces"></a>StripSpaces
**Función:**<br> StripSpaces(source)

**Descripción:**<br> quita todos los caracteres de espacio (" ") de la cadena de origen.

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |String |**de origen** que se actualiza. |

---
### <a name="switch"></a>Switch
**Función:**<br> Switch(source, defaultValue, key1, value1, key2, value2, …)

**Descripción:**<br> Cuando el valor de **source** coincide con una **key**, devuelve el **value** de dicha **key**. Si el valor de **source** no coincide con ninguna clave, devuelve **defaultValue**.  Los parámetros **key** y **value** siempre deben estar emparejados. La función espera siempre un número par de parámetros.

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |String |Valor de **origen** que se va a comprobar. |
   | **defaultValue** |Opcional |String |Valor predeterminado que se usará si el origen no coincide con ninguna clave. Puede tratarse de una cadena vacía (""). |
   | **key** |Obligatorio |String |**Key** con que se compara el valor de **source**. |
   | **value** |Obligatorio |String |Valor de reemplazo para el **source** que coincide con la clave. |

---
### <a name="tolower"></a>ToLower
**Función:**<br> ToLower (origen, referencia cultural)

**Descripción:**<br> Toma un valor de cadena de *origen* y se convierte a minúsculas mediante las reglas de referencia cultural que se hayan especificado. Si no hay ninguna información de *referencia cultural* especificada, se usará la referencia cultural invariable.

**Parámetros:**<br> 

   | Nombre | Obligatorio/Repetición | Tipo | Notas |
   | --- | --- | --- | --- |
   | **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto de origen |
   | **referencia cultural** |Opcional |String |El formato para el nombre de la referencia cultural según RFC 4646 es *languagecode2-country/regioncode2*, donde *languagecode2* es el código de idioma de dos letras y *country/regioncode2* es el código de referencia de subcultura de dos letras. Algunos ejemplos son a ja-JP para japonés (Japón) y en-US para inglés (Estados Unidos). En casos donde un código de idioma de dos letras no está disponible, se usa un código de tres letras derivado de ISO 639-2.|

---

### <a name="toupper"></a>ToUpper
**Función:**<br> ToUpper (origen, referencia cultural)

**Descripción:**<br> Toma un valor de cadena de *origen* y se convierte a mayúsculas mediante las reglas de referencia cultural que se hayan especificado. Si no hay ninguna información de *referencia cultural* especificada, se usará la referencia cultural invariable.

**Parámetros:**<br> 

  | Nombre | Obligatorio/Repetición | Tipo | Notas |
  | --- | --- | --- | --- |
  | **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto de origen. |
  | **referencia cultural** |Opcional |String |El formato para el nombre de la referencia cultural según RFC 4646 es *languagecode2-country/regioncode2*, donde *languagecode2* es el código de idioma de dos letras y *country/regioncode2* es el código de referencia de subcultura de dos letras. Algunos ejemplos son a ja-JP para japonés (Japón) y en-US para inglés (Estados Unidos). En casos donde un código de idioma de dos letras no está disponible, se usa un código de tres letras derivado de ISO 639-2.|

---

### <a name="trim"></a>Trim
**Descripción:**  
la función Trim quita los espacios en blanco del principio y del final de una cadena.

**Sintaxis:**  
`str Trim(str value)`  

**Ejemplo**:  
`Trim(" Test ")`  
devuelve "test".

`Trim([proxyAddresses])`  
Quita los espacios del principio y del final de cada valor en el atributo proxyAddress.

---
### <a name="word"></a>Word
**Descripción:**  
: la función Word devuelve una palabra incluida en una cadena, según los parámetros que describen los delimitadores que se usarán y el número de palabras que se devolverán.

**Sintaxis:**  
`str Word(str string, num WordNumber, str delimiters)`

* string: la cadena desde la que devolver una palabra.
* WordNumber: un número que identifica qué número de palabras debe devolver.
* delimiters: una cadena que representa los delimitadores que deben usarse para identificar palabras

**Observaciones**:  
: cada cadena de caracteres en la cadena separada por uno de los caracteres en los delimitadores se identifica como palabras:

* Si el número es < 1, se devuelve una cadena vacía.
* Si la cadena es Null, se devuelve una cadena vacía.

Si la cadena contiene menos palabras o si la cadena no contiene palabras identificadas por los delimitadores, se devuelve una cadena vacía.

**Ejemplo**:  
`Word("The quick brown fox",3," ")`  
devuelve "brown".

`Word("This,string!has&many separators",3,",!&#")`  
devolvería "has".

## <a name="examples"></a>Ejemplos
### <a name="strip-known-domain-name"></a>Seccionar un nombre de dominio conocido
Debe seccionar un nombre de dominio conocido de correo electrónico de un usuario para obtener un nombre de usuario. <br>
Por ejemplo, si el dominio es "contoso.com", puede usar la expresión siguiente:

**Expresión:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Ejemplo de entrada y salida:** <br>

* **ENTRADA** (mail): "john.doe@contoso.com"
* **SALIDA**: "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Anexar sufijos constantes a nombres de usuario
Si está utilizando un espacio aislado de Salesforce, deberá anexar un sufijo adicional a los nombres de usuario antes de sincronizarlas.

**Expresión:** <br>
`Append([userPrincipalName], ".test")`

**Entrada/salida de ejemplo:** <br>

* **ENTRADA**: (userPrincipalName): "John.Doe@contoso.com"
* **ENTRADA**:  "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Generar el alias de usuario concatenando partes de nombre y apellidos
Debe generar un alias de usuario con las tres primeras letras del nombre del usuario y las cinco primeras letras del apellido del usuario.

**Expresión:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Entrada/salida de ejemplo:** <br>

* **INPUT** (givenName): "John"
* **INPUT** (surname): "Doe"
* **OUTPUT**:  "JohnDoe"

### <a name="remove-diacritics-from-a-string"></a>Quitar los signos diacríticos de una cadena
Necesita reemplazar caracteres que contienen acentos por otros equivalentes que no los contienen.

**Expresión:** <br>
NormalizeDiacritics([givenName])

**Entrada/salida de ejemplo:** <br>

* **INPUT** (givenName): "Zoë"
* **OUTPUT**:  "Zoe"

### <a name="split-a-string-into-a-multi-valued-array"></a>Dividir una cadena en una matriz con varios valores
Debe tomar una lista de cadenas delimitada con comas y dividir esas cadenas en una matriz que se pueda conectar a un atributo de varios valores, como el atributo PermissionSets de Salesforce. En este ejemplo, una lista de conjuntos de permisos se ha rellenado en extensionAttribute5 en Azure AD.

**Expresión:** <br>
Split([extensionAttribute5], ",")

**Entrada/salida de ejemplo:** <br>

* **INPUT** (extensionAttribute5):
* **OUTPUT**:  ["PermissionSetOne", "PermissionSetTwo"]

### <a name="output-date-as-a-string-in-a-certain-format"></a>Fecha de resultado como una cadena en un formato determinado
Desea enviar las fechas a una aplicación SaaS con un formato determinado. <br>
Por ejemplo, desea dar formato a las fechas de ServiceNow.

**Expresión:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Entrada/salida de ejemplo:**

* **INPUT** (extensionAttribute1): "20150123105347.1Z"
* **OUTPUT**:  "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Reemplazar un valor basado en un conjunto predefinido de opciones

Debe definir la zona horaria del usuario según el código de estado almacenado en Azure AD. <br>
Si el código de estado no coincide con ninguna de las opciones predefinidas, use el valor predeterminado de "Australia/Sídney".

**Expresión:** <br>
`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Entrada/salida de ejemplo:**

* **INPUT** (state): "QLD"
* **OUTPUT**: "Australia/Brisbane"

### <a name="replace-characters-using-a-regular-expression"></a>Reemplazar los caracteres con una expresión regular
Debe buscar los caracteres que coincidan con un valor de expresión regular y quitarlos.

**Expresión:** <br>

Replace([mailNickname], , "[a-zA-Z_]*", , "", , )

**Entrada/salida de ejemplo:**

* **INPUT** (mailNickname: "john_doe72"
* **OUTPUT**: "72"

### <a name="convert-generated-userprincipalname-upn-value-to-lower-case"></a>Conversión del valor generado de userPrincipalName (UPN) a minúsculas
En el ejemplo siguiente, el valor de UPN se genera mediante la concatenación de los campos de origen PreferredFirstName y PreferredLastName, y la función ToLower opera en la cadena generada para convertir todos los caracteres a minúsculas. 

`ToLower(Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"))`

**Entrada/salida de ejemplo:**

* **INPUT** (PreferredFirstName): "John"
* **INPUT** (PreferredLastName): "Smith"
* **OUTPUT**: "john.smith@contoso.com"

### <a name="generate-unique-value-for-userprincipalname-upn-attribute"></a>Generación de un valor único para el atributo userPrincipalName (UPN)
Según el nombre del usuario, el segundo nombre y el apellido, deberá generar un valor para el atributo UPN y comprobar su unicidad en el directorio de AD de destino antes de asignar el valor al atributo UPN.

**Expresión:** <br>

```ad-attr-mapping-expr
    SelectUniqueValue( 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"), 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 1), [PreferredLastName]))), "contoso.com"),
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 2), [PreferredLastName]))), "contoso.com")
    )
```

**Entrada/salida de ejemplo:**

* **INPUT** (PreferredFirstName): "John"
* **INPUT** (PreferredLastName): "Smith"
* **OUTPUT**: "John.Smith@contoso.com" si el valor de UPN John.Smith@contoso.com no existe aún en el directorio
* **OUTPUT**: "J.Smith@contoso.com" si el valor de UPN John.Smith@contoso.com ya existe en el directorio
* **OUTPUT**: "Jo.Smith@contoso.com" si los dos valores de UPN anteriores ya existen en el directorio


## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es la sincronización en la nube de Azure AD Connect?](what-is-cloud-sync.md)
