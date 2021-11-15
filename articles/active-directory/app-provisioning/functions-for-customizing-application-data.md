---
title: Referencia para la escritura de expresiones para la asignación de atributos en el aprovisionamiento de aplicaciones de Azure Active Directory
description: Obtenga información sobre cómo usar asignaciones de expresiones para transformar valores de atributos en un formato aceptable durante el aprovisionamiento automático de objetos de aplicaciones SaaS en Azure Active Directory. Incluye una lista de referencias de funciones.
services: active-directory
author: kenwith
manager: karenh444
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: reference
ms.date: 10/27/2021
ms.author: kenwith
ms.reviewer: arvinh
ms.openlocfilehash: 2962c033ee42b91913324f22dbba3ca3cae49fdf
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131456373"
---
# <a name="reference-for-writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Referencia para la escritura de expresiones para la asignación de atributos en Azure Active Directory

Al configurar el aprovisionamiento para una aplicación SaaS, uno de los tipos de asignaciones de atributos que puede especificar es una asignación de expresiones. Para estas asignaciones, debe escribir una expresión similar a un script que permite transformar los datos de los usuarios en formatos más aceptables para la aplicación SaaS.

## <a name="syntax-overview"></a>Información general sobre la sintaxis

La sintaxis de expresiones para asignaciones de atributos recuerda a las funciones de Visual Basic para Aplicaciones (VBA).

* Toda la expresión se tiene que definir en términos de funciones, que constan de un nombre seguido de argumentos entre paréntesis:  *FunctionName(`<<argument 1>>`,`<<argument N>>`)*
* Es posible anidar funciones dentro de otras. Por ejemplo:  *FunctionOne(FunctionTwo(`<<argument1>>`))*
* Puede transformar tres tipos diferentes de argumentos en funciones:
  
  1. Atributos, que deben ir entre corchetes. Por ejemplo: [NombreAtributo]
  2. Constantes de cadena, que deben ir entre comillas. Por ejemplo: "Estados Unidos"
  3. Otras funciones. Por ejemplo: FunctionOne(`<<argument1>>`, FunctionTwo(`<<argument2>>`))
* Para las constantes de cadena, si necesita una barra diagonal inversa (\) o comillas dobles (") en la cadena, se deben convertirse con el símbolo de barra diagonal inversa (\). Por ejemplo: "Nombre de la empresa: \\"Contoso\\""
* La sintaxis distingue entre mayúsculas y minúsculas, lo que se debe tener en cuenta al escribirlas como cadenas en una función, por oposición a copiarlas y pegarlas directamente desde aquí. 

## <a name="list-of-functions"></a>Lista de funciones

[Append](#append) &nbsp;&nbsp;&nbsp;&nbsp; [AppRoleAssignmentsComplex](#approleassignmentscomplex) &nbsp;&nbsp;&nbsp;&nbsp; [BitAnd](#bitand) &nbsp;&nbsp;&nbsp;&nbsp; [CBool](#cbool) &nbsp;&nbsp;&nbsp;&nbsp; [CDate](#cdate) &nbsp;&nbsp;&nbsp;&nbsp; [Coalesce](#coalesce) &nbsp;&nbsp;&nbsp;&nbsp; [ConvertToBase64](#converttobase64) &nbsp;&nbsp;&nbsp;&nbsp; [ConvertToUTF8Hex](#converttoutf8hex) &nbsp;&nbsp;&nbsp;&nbsp; [Count](#count) &nbsp;&nbsp;&nbsp;&nbsp; [CStr](#cstr) &nbsp;&nbsp;&nbsp;&nbsp; [DateAdd](#dateadd) &nbsp;&nbsp;&nbsp;&nbsp; [DateDiff](#datediff) &nbsp;&nbsp;&nbsp;&nbsp; [DateFromNum](#datefromnum) &nbsp;[FormatDateTime](#formatdatetime) &nbsp;&nbsp;&nbsp;&nbsp; [Guid](#guid) &nbsp;&nbsp;&nbsp;&nbsp; [IgnoreFlowIfNullOrEmpty](#ignoreflowifnullorempty) &nbsp;&nbsp;&nbsp;&nbsp;[IIF](#iif) &nbsp;&nbsp;&nbsp;&nbsp;[InStr](#instr) &nbsp;&nbsp;&nbsp;&nbsp; [IsNull](#isnull) &nbsp;&nbsp;&nbsp;&nbsp; [IsNullOrEmpty](#isnullorempty) &nbsp;&nbsp;&nbsp;&nbsp; [IsPresent](#ispresent) &nbsp;&nbsp;&nbsp;&nbsp; [IsString](#isstring) &nbsp;&nbsp;&nbsp;&nbsp; [Item](#item) &nbsp;&nbsp;&nbsp;&nbsp; [Join](#join) &nbsp;&nbsp;&nbsp;&nbsp; [Left](#left) &nbsp;&nbsp;&nbsp;&nbsp; [Mid](#mid) &nbsp;&nbsp;&nbsp;&nbsp; [NormalizeDiacritics](#normalizediacritics) &nbsp;&nbsp; &nbsp;&nbsp; [Not](#not) &nbsp;&nbsp;&nbsp;&nbsp; [Now](#now) &nbsp;&nbsp;&nbsp;&nbsp; [NumFromDate](#numfromdate) &nbsp;&nbsp;&nbsp;&nbsp; [PCase](#pcase) &nbsp;&nbsp;&nbsp;&nbsp; [RandomString](#randomstring) &nbsp;&nbsp;&nbsp;&nbsp; [RemoveDuplicates](#removeduplicates) &nbsp;&nbsp;&nbsp;&nbsp; [Replace](#replace) &nbsp;&nbsp;&nbsp;&nbsp; [SelectUniqueValue](#selectuniquevalue)&nbsp;&nbsp;&nbsp;&nbsp; [SingleAppRoleAssignment](#singleapproleassignment)&nbsp;&nbsp;&nbsp;&nbsp; [Split](#split)&nbsp;&nbsp;&nbsp;&nbsp;[StripSpaces](#stripspaces) &nbsp;&nbsp;&nbsp;&nbsp; [Switch](#switch)&nbsp;&nbsp;&nbsp;&nbsp; [ToLower](#tolower)&nbsp;&nbsp;&nbsp;&nbsp; [ToUpper](#toupper)&nbsp;&nbsp;&nbsp;&nbsp; [Word](#word)

---
### <a name="append"></a>Append

**Función:**  Append(source, suffix)

**Descripción:**  adopta un valor de la cadena de origen y anexa el sufijo al final de la misma.

**Parámetros:**

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto de origen. |
| **suffix** |Obligatorio |String |La cadena que se va a anexar al final del valor de origen. |


#### <a name="append-constant-suffix-to-user-name"></a>Anexar sufijos constantes a nombres de usuario
Ejemplo: si está utilizando un espacio aislado de Salesforce, deberá anexar otro sufijo a todos los nombres de usuario antes de sincronizarlos.

**Expresión:**  
`Append([userPrincipalName], ".test")`

**Entrada/salida de ejemplo:** 

* **ENTRADA**: (userPrincipalName): "John.Doe@contoso.com"
* **ENTRADA**:  "John.Doe@contoso.com.test"

---
### <a name="approleassignmentscomplex"></a>AppRoleAssignmentsComplex

**Función:** AppRoleAssignmentsComplex([appRoleAssignments])

**Descripción:** se usa para aprovisionar varios roles para un usuario. Para obtener información detallada, consulte [Tutorial: Personalización de las asignaciones de atributos de aprovisionamiento de usuarios para aplicaciones SaaS en Azure Active Directory](customize-application-attributes.md#provisioning-a-role-to-a-scim-app).

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Obligatorio |String |Objeto **[appRoleAssignments]** . |

---
### <a name="bitand"></a>BitAnd
**Función:** BitAnd(value1, value2)

**Descripción:** : esta función convierte ambos parámetros en la representación binaria y establece un bit en los siguientes valores:

- 0: si uno o ambos de los bits correspondientes en value1 y value2 son 0.
- 1: si ambos de los bits correspondientes son 1.

En otras palabras, devuelve 0 en todos los casos excepto cuando los bits correspondientes de los dos parámetros son 1.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **value1** |Obligatorio |N.º |Valor numérico que se debe agregar con AND a value2|
| **value2** |Obligatorio |N.º |Valor numérico que se debe agregar con AND a value1|

**Ejemplo:** 
`BitAnd(&HF, &HF7)`

11110111 AND 00000111 = 00000111 de modo que `BitAnd` devuelve 7, el valor binario de 00000111.

---
### <a name="cbool"></a>CBool
**Función:**  
`CBool(Expression)`

**Descripción:**  
`CBool` devuelve un valor booleano basado en la expresión evaluada. Si la expresión se evalúa en un valor distinto de cero, `CBool` devuelve *True*. En caso contrario, devuelve *False*.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **Expression** |Obligatorio | expresión | Cualquier expresión válida |

**Ejemplo:** 
`CBool([attribute1] = [attribute2])`                                                                    
Devuelve True si ambos atributos tienen el mismo valor.

---
### <a name="cdate"></a>CDate
**Función:**  
`CDate(expression)`

**Descripción:**  
: la función CDate devuelve un valor DateTime UTC a partir de una cadena. DateTime no es un tipo de atributo nativo, pero se puede usar en funciones de fecha como [FormatDateTime](#formatdatetime) y [DateAdd](#dateadd).

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **Expression** |Requerido | Expression | Cualquier cadena válida que represente una fecha y hora. Para conocer los formatos admitidos, consulte [Cadenas de formato de fecha y hora personalizadas de .NET](/dotnet/standard/base-types/custom-date-and-time-format-strings). |

**Observaciones**:  
La cadena devuelta siempre está en UTC y sigue el formato **M/d/yyyy h:mm:ss tt**.

**Ejemplo 1:** <br> 
`CDate([StatusHireDate])`  
**Entrada/salida de ejemplo:** 

* **INPUT** (StatusHireDate): "2020-03-16-07:00"
* **OUTPUT**:  "3/16/2020 7:00:00 AM" <-- *Tenga en cuenta que se devuelve la hora UTC equivalente del valor DateTime anterior*

**Ejemplo 2:** <br> 
`CDate("2021-06-30+08:00")`  
**Entrada/salida de ejemplo:** 

* **INPUT**: "2021-06-30+08:00"
* **OUTPUT**:  "6/29/2021 4:00:00 AM" <-- *Tenga en cuenta que se devuelve la hora UTC equivalente del valor DateTime anterior*

**Ejemplo 3:** <br> 
`CDate("2009-06-15T01:45:30-07:00")`  
**Entrada/salida de ejemplo:** 

* **INPUT**: "2009-06-15T01:45:30-07:00"
* **OUTPUT**:  "6/15/2009 8:45:30 AM" <-- *Tenga en cuenta que se devuelve la hora UTC equivalente del valor DateTime anterior*

---
### <a name="coalesce"></a>Coalesce
**Función:** Coalesce(source1, source2, ..., defaultValue)

**Descripción:** Devuelve el primer valor de origen que no es NULL. Si todos los argumentos son NULL y defaultValue está presente, se devolverá defaultValue. Si todos los argumentos son NULL y defaultValue no está presente, Coalesce devuelve NULL.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **source1  … sourceN** | Obligatorio | String |Obligatorio, número variable de veces. Normalmente el nombre del atributo del objeto de origen. |
| **defaultValue** | Opcional | String | Valor predeterminado que usará cuando todos los valores de origen sean NULL. Puede tratarse de una cadena vacía ("").

#### <a name="flow-mail-value-if-not-null-otherwise-flow-userprincipalname"></a>Proporciona el valor de correo si no es NULL; de lo contrario, proporciona userPrincipalName
Ejemplo: Desea proporcionar el atributo de correo si está presente. Si no es así, desea proporcionar el valor de userPrincipalName en su lugar.

**Expresión:**  
`Coalesce([mail],[userPrincipalName])`

**Entrada/salida de ejemplo:** 

* **ENTRADA** (mail): NULL
* **ENTRADA**: (userPrincipalName): "John.Doe@contoso.com"
* **ENTRADA**:  "John.Doe@contoso.com"


---
### <a name="converttobase64"></a>ConvertToBase64
**Función:** ConvertToBase64(source)

**Descripción:** La función ConvertToBase64 convierte una cadena en una en base64 de Unicode.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |Cadena que se va a convertir a base 64.|

**Ejemplo:** 
`ConvertToBase64("Hello world!")`

devuelve "SABlAGwAbABvACAAdwBvAHIAbABkACEA".

---
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Función:** ConvertToUTF8Hex(source)

**Descripción:** : la función ConvertToUTF8Hex convierte una cadena en un valor codificado hexadecimal UTF8.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |Cadena que se va a convertir a UTF8 Hex.|

**Ejemplo:** 
`ConvertToUTF8Hex("Hello world!")`

devuelve 48656C6C6F20776F726C6421.

---
### <a name="count"></a>Count
**Función:** Count(attribute)

**Descripción:** : la función Count devuelve el número de elementos en un atributo multivalor.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **attribute** |Obligatorio |atributo |Atributo con varios valores que tendrá elementos contados.|

---
### <a name="cstr"></a>CStr
**Función:** CStr(value)

**Descripción:** La función CStr convierte un valor en un tipo de datos de cadena.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **value** |Obligatorio | Numérico, referencia o booleano | puede ser un valor numérico, un atributo de referencia o un valor booleano. |

**Ejemplo:** 
`CStr([dn])`

Devuelve "cn=Joe,dc=contoso,dc=com".

---
### <a name="dateadd"></a>DateAdd
**Función:**  
`DateAdd(interval, value, dateTime)`

**Descripción:**  
devuelve una cadena de fecha y hora que representa una fecha a la que se ha agregado un intervalo de tiempo especificado. La fecha devuelta tiene el formato: **M/d/yyyy h:mm:ss tt**.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **interval** |Requerido | String | Intervalo de tiempo que desea agregar. Consulte los valores aceptados se debajo de esta tabla. |
| **value** |Obligatorio | Number | el número de unidades que desea agregar. Puede ser positivo (para obtener fechas futuras) o negativo (para obtener fechas del pasado). |
| **dateTime** |Requerido | DateTime | DateTime que representa la fecha a la que se agrega el intervalo. |

La cadena de **intervalo** debe tener uno de los valores siguientes: 
 * yyyy Año 
 * m Mes
 * d Día
 * ww Semana
 * h Hora
 * n Minuto
 * s Segundo

**Ejemplo 1: Adición de 7 días a la fecha de contratación**  
`DateAdd("d", 7, CDate([StatusHireDate]))`
* **INPUT** (StatusHireDate): 2012-03-16-07:00
* **OUTPUT**: 3/23/2012 7:00:00 AM

**Ejemplo 2: Obtención de una fecha 10 días antes de la fecha de contratación**  
`DateAdd("d", -10, CDate([StatusHireDate]))`
* **INPUT** (StatusHireDate): 2012-03-16-07:00
* **OUTPUT**: 3/6/2012 7:00:00 AM

**Ejemplo 3: Adición de 2 semanas a la fecha de contratación**  
`DateAdd("ww", 2, CDate([StatusHireDate]))`
* **INPUT** (StatusHireDate): 2012-03-16-07:00
* **OUTPUT**: 3/30/2012 7:00:00 AM

**Ejemplo 4: Adición de 10 meses a la fecha de contratación**  
`DateAdd("m", 10, CDate([StatusHireDate]))`
* **INPUT** (StatusHireDate): 2012-03-16-07:00
* **OUTPUT**: 1/16/2013 7:00:00 AM

**Ejemplo 5: Adición de 2 años a la fecha de contratación**  
`DateAdd("yyyy", 2, CDate([StatusHireDate]))`
* **INPUT** (StatusHireDate): 2012-03-16-07:00
* **OUTPUT**: 3/16/2014 7:00:00 AM
---
### <a name="datediff"></a>DateDiff
**Función:**  
`DateDiff(interval, date1, date2)`

**Descripción:**  
Esta función usa el parámetro de *interval* para devolver un número que indique la diferencia entre las dos fechas de entrada. Devuelve  
  * un número positivo si date2 > date1, 
  * un número negativo si date2 < date1, 
  * 0 si date2 == date1

**Parámetros:** 

| Nombre | Obligatorio/opcional | Tipo | Notas |
| --- | --- | --- | --- |
| **interval** |Requerido | String | Intervalo de tiempo que se usará para calcular la diferencia. |
| **date1** |Requerido | DateTime | Valor de DateTime que representa una fecha válida. |
| **date2** |Requerido | DateTime | Valor de DateTime que representa una fecha válida. |

La cadena de **intervalo** debe tener uno de los valores siguientes: 
 * yyyy Año 
 * m Mes
 * d Día
 * ww Semana
 * h Hora
 * n Minuto
 * s Segundo

**Ejemplo 1: comparación de la fecha actual con la fecha de contratación de Workday con intervalos diferentes** <br>
`DateDiff("d", Now(), CDate([StatusHireDate]))`

| Ejemplo | interval | date1 | date2 | output |
| --- | --- | --- | --- | --- |
| Diferencia positiva en días entre dos fechas | d | 2021-08-18+08:00 | 2021-08-31+08:00 | 13 |
| Diferencia negativa en días entre dos fechas | d | 8/25/2021 5:41:18 PM | 2012-03-16-07:00 | -3449 |
| Diferencia en semanas entre dos fechas | ww | 8/25/2021 5:41:18 PM | 2012-03-16-07:00 | -493 | 
| Diferencia en meses entre dos fechas | m | 8/25/2021 5:41:18 PM | 2012-03-16-07:00 | -113 | 
| Diferencia en años entre dos fechas | aaaa | 8/25/2021 5:41:18 PM | 2012-03-16-07:00 | -9 | 
| Diferencia cuando ambas fechas son iguales | d | 2021-08-31+08:00 | 2021-08-31+08:00 | 0 | 
| Diferencia en horas entre dos fechas | h | 2021-08-24 | 2021-08-25 | 24 | 
| Diferencia en minutos entre dos fechas | n | 2021-08-24 | 2021-08-25 | 1440 | 
| Diferencia en segundos entre dos fechas | s | 2021-08-24 | 2021-08-25 | 86400 | 

**Ejemplo 2: Combinación de DateDiff con la función IIF para establecer el valor del atributo** <br>
Si una cuenta está activa en Workday, establezca el atributo *accountEnabled* del usuario en True solo si la fecha de contratación está dentro de los próximos 5 días. 

```
Switch([Active], , 
  "1", IIF(DateDiff("d", Now(), CDate([StatusHireDate])) > 5, "False", "True"), 
  "0", "False")
```

---

### <a name="datefromnum"></a>DateFromNum
**Función:** DateFromNum(value)

**Descripción:** la función DateFromNum convierte un valor con formato de fecha de AD en un tipo DateTime.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **value** |Obligatorio | Date | Fecha de AD que se va a convertir al tipo DateTime. |

**Ejemplo:** 
`DateFromNum([lastLogonTimestamp])`

`DateFromNum(129699324000000000)`

Devuelve un valor DateTime que representa el 1 de enero de 2012 a las 23 h.

---
### <a name="formatdatetime"></a>FormatDateTime
**Función:** FormatDateTime(source, dateTimeStyles, inputFormat, outputFormat)

**Descripción:**  adopta una cadena de fecha en un formato y la convierte a un formato distinto.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto de origen. |
| **dateTimeStyles** | Opcional | String | Utilice este parámetro para especificar las opciones de formato que personalizan el análisis de cadenas en algunos métodos de análisis de fecha y hora. Para conocer los valores admitidos, consulte la [documentación de DateTimeStyles](/dotnet/api/system.globalization.datetimestyles). Si se deja vacío, se utiliza el valor predeterminado: DateTimeStyles.RoundtripKind, DateTimeStyles.AllowLeadingWhite, DateTimeStyles.AllowTrailingWhite.  |
| **inputFormat** |Obligatorio |String |Formato esperado del valor de origen. Para conocer los formatos admitidos, consulte [Cadenas de formato de fecha y hora personalizadas de .NET](/dotnet/standard/base-types/custom-date-and-time-format-strings). |
| **outputFormat** |Obligatorio |String |Formato de la fecha de salida. |



#### <a name="output-date-as-a-string-in-a-certain-format"></a>Fecha de resultado como una cadena en un formato determinado
Ejemplo: desea enviar fechas a una aplicación SaaS como ServiceNow en un formato determinado. Puede usar la siguiente expresión. 

**Expresión:** 

`FormatDateTime([extensionAttribute1], , "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Entrada/salida de ejemplo:**

* **INPUT** (extensionAttribute1): "20150123105347.1Z"
* **OUTPUT**:  "2015-01-23"


---
### <a name="guid"></a>Guid
**Función:** Guid()

**Descripción:** La función GUID genera un nuevo GUID aleatorio.

**Ejemplo**: <br>
`Guid()`<br>
Sample output: "1088051a-cd4b-4288-84f8-e02042ca72bc"

---
### <a name="ignoreflowifnullorempty"></a>IgnoreFlowIfNullOrEmpty
**Function:** IgnoreFlowIfNullOrEmpty(expression)

**Descripción:** la función IgnoreFlowIfNullOrEmpty indica al servicio de aprovisionamiento que omita el atributo y lo descarte del flujo si la función o el atributo incluidos son NULL o están vacíos.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **Expression** | Requerido | Expression | Expresión que se va a evaluar. |

**Ejemplo 1: No pase un atributo si es NULL** <br>
`IgnoreFlowIfNullOrEmpty([department])` <br>
La expresión anterior quitará el atributo department del flujo de aprovisionamiento si es NULL o está vacío. <br>

**Ejemplo 2: No pase un atributo si la asignación de expresiones se evalúa como cadena vacía o NULL** <br>
Supongamos que el atributo SuccessFactors *prefix* se asigna al atributo de Active Directory local *personalTitle* mediante la siguiente asignación de expresiones: <br>
`IgnoreFlowIfNullOrEmpty(Switch([prefix], "", "3443", "Dr.", "3444", "Prof.", "3445", "Prof. Dr."))` <br>
La expresión anterior evalúa primero la función [Switch](#switch). Si el atributo *prefix* no tiene ninguno de los valores enumerados en la función *Switch*, *Switch* devolverá una cadena vacía y el atributo *personalTitle* no se incluirá en el flujo de aprovisionamiento para Active Directory local.

---
### <a name="iif"></a>IIF
**Función:** IIF(condition,valueIfTrue,valueIfFalse)

**Descripción:** : la función IIF devuelve uno de un conjunto de valores posibles según una condición especificada.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **condition** |Obligatorio |Variable o expresión |Cualquier valor o expresión que pueda evaluarse como true o false. |
| **valueIfTrue** |Obligatorio |Variable o cadena | si la condición se evalúa como true, el valor devuelto. |
| **valueIfFalse** |Obligatorio |Variable o cadena |si la condición se evalúa como false, el valor devuelto.|

**Ejemplo:** 
`IIF([country]="USA",[country],[department])`

---
### <a name="instr"></a>InStr
**Función:** InStr(value1, value2, start, compareType)

**Descripción:** : la función InStr busca la primera repetición de una subcadena en una cadena.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **value1** |Obligatorio |String |Cadena que se va a buscar. |
| **value2** |Obligatorio |String |Cadena que se va a encontrar. |
| **start** |Opcional |Entero |Posición inicial para buscar la subcadena.|
| **compareType** |Opcional |Enum |Puede ser vbTextCompare o vbBinaryCompare. |

**Ejemplo:** 
`InStr("The quick brown fox","quick")`

Se evalúa en 5

`InStr("repEated","e",3,vbBinaryCompare)`

Se evalúa en 7.

---
### <a name="isnull"></a>IsNull
**Función:** IsNull(Expression)

**Descripción:**  si la expresión se evalúa como Null, la función IsNull devuelve True. para un atributo, un valor Null se expresa mediante la ausencia del atributo.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **Expression** |Requerido |Expression |Expresión que se va a evaluar. |

**Ejemplo:** 
`IsNull([displayName])`

Devuelve True si el atributo no está presente.

---
### <a name="isnullorempty"></a>IsNullOrEmpty
**Función:** IsNullOrEmpty(Expression)

**Descripción:**  si la expresión es Null o una cadena vacía, la función IsNullOrEmpty devuelve True. para un atributo, esto se evaluaría como True si el atributo no está presente o está presente, pero es una cadena vacía.
La función contraria a esta es IsPresent.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **Expression** |Requerido |Expression |Expresión que se va a evaluar. |

**Ejemplo:** 
`IsNullOrEmpty([displayName])`

Devuelve True si el atributo no está presente o si es una cadena vacía.

---
### <a name="ispresent"></a>IsPresent
**Función:** IsPresent(Expression)

**Descripción:**  si la expresión se evalúa como una cadena que no es Null y no está vacía, la función IsPresent devuelve True. la función contraria a esta es IsNullOrEmpty.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **Expression** |Requerido |Expression |Expresión que se va a evaluar. |

**Ejemplo:** 
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

---
### <a name="isstring"></a>IsString
**Función:** IsString(Expression)

**Descripción:**  si la expresión se puede evaluar en un tipo de cadena, la función IsString se evalúa en True.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **Expression** |Requerido |Expression |Expresión que se va a evaluar. |

---
### <a name="item"></a>Elemento
**Función:** Item(attribute, index)

**Descripción:**  la función Item devuelve un elemento de un atributo o una cadena de varios valores.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **attribute** |Obligatorio |Atributo |Atributo multivalor en el que se va a buscar. |
| **índice** |Obligatorio |Entero | Índice de un elemento en la cadena multivalor.|

**Ejemplo:** 
`Item([proxyAddresses], 1)` devuelve el primer elemento en el atributo multivalor. No se debe usar el índice 0. 

---
### <a name="join"></a>Join
**Función:**  Join(separator, source1, source2, …)

**Descripción:** Join() es similar a Append(), excepto en que puede combinar varios valores de cadena de **source** en una sola cadena, y cada valor estará separado por una cadena de **separator**.

Si uno de los valores de origen es un atributo multivalor, cada valor de ese atributo se unirá, separado por el valor del separador.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **separator** |Obligatorio |String |Cadena utilizada para separar los valores de origen cuando se concatenan en una sola cadena. Puede ser "" si no es necesario ningún separador. |
| **source1  … sourceN** |Obligatorio, número variable de veces |String |Valores de cadena que se van a agrupar. |

---
### <a name="left"></a>Left
**Función:** Left(String, NumChars)

**Descripción:**  la función Left devuelve un número especificado de caracteres desde la izquierda de una cadena. Con numChars = 0, se devuelve una cadena vacía.
Con numChars < 0, se devuelve una cadena de entrada.
Si la cadena es null, se devuelve una cadena vacía.
Si la cadena contiene menos caracteres que el número especificado en numChars, se devuelve una cadena idéntica a la cadena (es decir, que contiene todos los caracteres en el parámetro 1).

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **String** |Obligatorio |Atributo | Cadena de la que se van a devolver caracteres. |
| **NumChars** |Obligatorio |Entero | Número que identifica el número de caracteres que se va a devolver desde el principio (la izquierda) de la cadena.|

**Ejemplo:** 
`Left("John Doe", 3)`

devuelve “Joh”.

---
### <a name="mid"></a>Mid
**Función:**  Mid(source, start, length)

**Descripción:**  devuelve una subcadena del valor de origen. Una subcadena es una cadena que contiene sólo algunos de los caracteres de la cadena de origen.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |Normalmente el nombre del atributo. |
| **start** |Obligatorio |Entero |Índice de la cadena de **source** donde debe empezar la subcadena. El primer carácter de la cadena tendrá el índice de 1, el segundo carácter tendrá el índice de 2, y así sucesivamente. |
| **length** |Obligatorio |Entero |Longitud de la subcadena. Si length acaba fuera de la cadena de **source**, la función devolverá una subcadena desde el índice de **start** hasta el final de la cadena de **source**. |

---
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Función:** NormalizeDiacritics(source)

**Descripción:** Requiere un argumento de cadena. Devuelve la cadena, pero se reemplazan todos los caracteres diacríticos por sus equivalentes no diacríticos. Normalmente se usa para convertir nombres y apellidos que contienen caracteres diacríticos (acentos) en valores legales que se pueden emplear en diversos identificadores de usuario como, nombres principales de usuario, nombres de cuenta de SAM y direcciones de correo electrónico.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String | Normalmente un atributo de nombre o de apellido. |


| Carácter con marca diacrítica  | Carácter normalizado | Carácter con marca diacrítica  | Carácter normalizado | 
| --- | --- | --- | --- | 
| ä, à, â, ã, å, á, ą, ă, ā, ā́, ā̀, ā̂, ā̃, ǟ, ā̈, ǡ, a̱, å̄ | a | Ä, À, Â, Ã, Å, Á, Ą, Ă, Ā, Ā́, Ā̀, Ā̂, Ā̃, Ǟ, Ā̈, Ǡ, A̱, Å̄ | A | 
| æ, ǣ | ae | Æ, Ǣ | AE | 
| ç, č, ć, c̄, c̱ | c | Ç, Č, Ć, C̄, C̱ | C | 
| ď, d̄, ḏ | d | Ď, D̄, Ḏ | D | 
| ë, è, é, ê, ę, ě, ė, ē, ḗ, ḕ, ē̂, ē̃, ê̄, e̱, ë̄, e̊̄ | e | Ë, È, É, Ê, Ę, Ě, Ė, Ē, Ḗ, Ḕ, Ē̂, Ē̃, Ê̄, E̱, Ë̄, E̊̄ | E | 
| ğ, ḡ, g̱ | e | Ğ, Ḡ, G̱ | G | 
| ï, î, ì, í, ı, ī, ī́, ī̀, ī̂, ī̃, i̱ | i | Ï, Î, Ì, Í, İ, Ī, Ī́, Ī̀, Ī̂, Ī̃, I̱ | I |  
| ľ, ł, l̄, ḹ, ḻ | l |  Ł, Ľ, L̄, Ḹ, Ḻ | L | 
| ñ, ń, ň, n̄, ṉ | n |  Ñ, Ń, Ň, N̄, Ṉ | No | 
| ö, ò, ő, õ, ô, ó, ō, ṓ, ṑ, ō̂, ō̃, ȫ, ō̈, ǭ, ȭ, ȱ, o̱ | o |  Ö, Ò, Ő, Õ, Ô, Ó, Ō, Ṓ, Ṑ, Ō̂, Ō̃, Ȫ, Ō̈, Ǭ, Ȭ, Ȱ, O̱ | O | 
| ø, ø̄, œ̄  | oe |  Ø, Ø̄, Œ̄ | OE | 
| ř, r̄, ṟ, ṝ | r |  Ř, R̄, Ṟ, Ṝ | R | 
| ß | ss | | | 
| š, ś, ș, ş, s̄, s̱ | s |  Š, Ś, Ș, Ş, S̄, S̱ | S | 
| ť, ț, t̄, ṯ | t | Ť, Ț, T̄, Ṯ | T | 
| ü, ù, û, ú, ů, ű, ū, ū́, ū̀, ū̂, ū̃, u̇̄, ǖ, ṻ, ṳ̄, u̱ | u |  Ü, Ù, Û, Ú, Ů, Ű, Ū, Ū́, Ū̀, Ū̂, Ū̃, U̇̄, Ǖ, Ṻ, Ṳ̄, U̱ | U | 
| ÿ, ý, ȳ, ȳ́, ȳ̀, ȳ̃, y̱ | y | Ÿ, Ý, Ȳ, Ȳ́, Ȳ̀, Ȳ̃, Y̱ | Y | 
| ź, ž, ż, z̄, ẕ | z | Ź, Ž, Ż, Z̄, Ẕ | Z | 


#### <a name="remove-diacritics-from-a-string"></a>Quitar los signos diacríticos de una cadena
Ejemplo: reemplazar caracteres que contienen acentos por otros equivalentes que no los contienen.

**Expresión:** NormalizeDiacritics([givenName])

**Entrada/salida de ejemplo:** 

* **INPUT** (givenName): "Zoë"
* **OUTPUT**:  "Zoe"


---
### <a name="not"></a>Not
**Función:**  Not(source)

**Descripción:** Invierte el valor booleano de **source**. Si el valor de **source** es True, devuelve False. De lo contrario, devuelve True.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |Cadena booleana |Los valores de **source** esperados son "True" o "False". |

---
### <a name="now"></a>Ahora
**Función:** Now()

**Descripción:**  
La función Now devuelve una cadena que representa el valor UTC DateTime actual en el formato **M/d/yyyy h:mm:ss tt**.

**Ejemplo:** 
`Now()` <br>
Valor de ejemplo devuelto *7/2/2021 3:33:38 PM*

---
### <a name="numfromdate"></a>NumFromDate
**Función:** NumFromDate(value)

**Descripción:** La función NumFromDate convierte un valor DateTime a un formato de Active Directory necesario para establecer atributos como [accountExpires](/windows/win32/adschema/a-accountexpires). Use esta función para convertir valores DateTime recibidos de aplicaciones de recursos humanos en la nube, como WorkDay y SuccessFactors, a su representación de AD equivalente. 

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **value** |Obligatorio | String | Cadena de fecha y hora en el formato admitido. Para conocer los formatos admitidos, consulte https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx. |

**Ejemplo**:
* Ejemplo de Workday. Suponiendo que desee asignar el atributo *ContractEndDate* desde WorkDay, que tiene el formato *2020-12-31-08:00* al campo *accountExpires* en AD, aquí se muestra cómo puede usar esta función y cambiar el desplazamiento de zona horaria para que coincida con la configuración regional. 
  `NumFromDate(Join("", FormatDateTime([ContractEndDate], ,"yyyy-MM-ddzzz", "yyyy-MM-dd"), " 23:59:59-08:00"))`

* Ejemplo de SuccessFactors Suponiendo que desea asignar el atributo *endDate* de SuccessFactors, que tiene el formato *M/d/yyyy hh:mm:ss tt* para el campo *accountExpires* en AD, aquí se muestra cómo puede usar esta función y cambiar el ajuste de zona horaria para que coincida con la configuración regional.
  `NumFromDate(Join("",FormatDateTime([endDate], ,"M/d/yyyy hh:mm:ss tt","yyyy-MM-dd")," 23:59:59-08:00"))`


---
### <a name="pcase"></a>PCase
**Función:** PCase(source, wordSeparators)

**Descripción:** la función PCase convierte el primer carácter de cada palabra de una cadena a mayúsculas, y todos los demás caracteres se convierten a minúsculas.

**Parámetros:** 

| Nombre | Obligatorio/opcional | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |Un valor **source** que se va a convertir en mayúsculas o minúsculas. |
| **wordSeparators** |Opcional |String |Especifique un conjunto de caracteres que se usarán como separadores de palabras (por ejemplo: " ,-'"). |

**Observaciones**:

* Si no se especifica el parámetro *wordSeparators,* , PCase invoca internamente la función de .NET [ToTitleCase](/dotnet/api/system.globalization.textinfo.totitlecase) para convertir la cadena de *origen* en mayúsculas y minúsculas correctas. La función de .NET *ToTitleCase* admite un conjunto completo de [categorías de caracteres Unicode](https://www.unicode.org/reports/tr44/#General_Category_Values) como separadores de palabras. 
  * Carácter de espacio
  * Carácter de nueva línea
  * Caracteres de *control*, como CRLF
  * Caracteres de control *Format*
  * Caracteres *ConnectorPunctuation*, como el guion bajo
  * Caracteres *DashPunctuation*, como guiones (incluidos caracteres como guion corto, guion largo, guion doble, etc.)
  * Caracteres *OpenPunctuation* y *ClosePunctuation* que se producen en pares, como paréntesis, corchetes, corchetes angulares, etc. 
  * Caracteres *InitialQuotePunctuation* and *FinalQuotePunctuation*, como comillas simples, comillas dobles y comillas angulares. 
  * Caracteres *OtherPunctuation*, como signo de exclamación, signo de número, signo de porcentaje, Y comercial, asterisco, coma, punto, dos puntos, punto y coma, etc. 
  * Caracteres *MathSymbol*, como signo más, signo menor y mayor que, línea vertical, tilde, signo igual, etc.
  * Caracteres *CurrencySymbol*, como signo de dólar, signo de céntimos, signo de libras, signo de euro, etc. 
  * Caracteres *ModifierSymbol*, como acento largo, acentos, puntas de flecha, etc. 
  * Caracteres *OtherSymbol*, como el signo de copyright, el signo de grado, el signo registrado, etc. 
* Si se especifica el parámetro *wordSeparators*, PCase solo usa los caracteres especificados como separadores de palabras. 

**Ejemplo**:

Supongamos que va a obtener los atributos *firstName* y *lastName* de SAP SuccessFactors y en RR. HH. ambos atributos están en mayúsculas. Con la función PCase, puede convertir el nombre a mayúsculas y minúsculas como se muestra a continuación. 

| Expression | Entrada | Resultados | Notas |
| --- | --- | --- | --- |
| `PCase([firstName])` | *firstName* = "PABLO GONSALVES (SECOND)" | "Pablo Gonsalves (Second)" | Como no se especifica el parámetro *wordSeparators*, la función *PCase* usa el juego de caracteres separadores de palabras predeterminado. |
| `PCase([lastName]," '-")` | *lastName* = "PINTO-DE'SILVA" | "Pinto-De'Silva" | La función *PCase* usa caracteres en el parámetro *wordSeparators* para identificar palabras y transformarlas en mayúsculas y minúsculas correctas. |
| `PCase(Join(" ",[firstName],[lastName]))` | *firstName* = GREGORY, *lastName* = "JAMES" | "Gregory James" | Puede anidar la función Join en PCase. Como no se especifica el parámetro *wordSeparators*, la función *PCase* usa el juego de caracteres separadores de palabras predeterminado.  |


---

### <a name="randomstring"></a>RandomString
**Función:** RandomString(Length, MinimumNumbers, MinimumSpecialCharacters , MinimumCapital, MinimumLowerCase, CharactersToAvoid)

**Descripción:** La función RandomString genera una cadena aleatoria en función de las condiciones especificadas. Los caracteres permitidos pueden identificarse [aquí](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements#reference).

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **Duración** |Obligatorio |Number |Longitud total de la cadena aleatoria. Debe ser mayor o igual que la suma de MinimumNumbers, MinimumSpecialCharacters y MinimumCapital. 256 caracteres como máximo.|
| **MinimumNumbers** |Obligatorio |Number |Números mínimos en la cadena aleatoria.|
| **MinimumSpecialCharacters** |Obligatorio |Number |Número mínimo de caracteres especiales.|
| **MinimumCapital** |Obligatorio |Number |Número mínimo de letras mayúsculas en la cadena aleatoria.|
| **MinimumLowerCase** |Obligatorio |Number |Número mínimo de letras minúsculas en la cadena aleatoria.|
| **CharactersToAvoid** |Opcional |String |Caracteres que se excluirán al generar la cadena aleatoria.|


**Ejemplo 1:** Generación de una cadena aleatoria sin restricciones de caracteres especiales: `RandomString(6,3,0,0,3)`
Genera una cadena aleatoria con 6 caracteres. La cadena contiene 3 números y 3 caracteres en minúsculas (1a73qt).

**Ejemplo 2:** Generación de una cadena aleatoria con restricciones de caracteres especiales: `RandomString(10,2,2,2,1,"?,")`
Genera una cadena aleatoria con 10 caracteres. La cadena contiene al menos 2 números, 2 caracteres especiales, 2 letras mayúsculas, 1 letra minúscula y excluye los caracteres "?" y "," (1@!2BaRg53).

---

### <a name="removeduplicates"></a>RemoveDuplicates
**Función:** RemoveDuplicates(attribute)

**Descripción:**  la función RemoveDuplicates toma una cadena multivalor y garantiza que cada valor es único.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **attribute** |Obligatorio |Atributo multivalor |Atributo multivalor en el que se habrán quitado los duplicados.|

**Ejemplo:** 
`RemoveDuplicates([proxyAddresses])`  devuelve un atributo proxyAddress saneado donde se han quitado todos los valores duplicados.

---
### <a name="replace"></a>Replace
**Función:** Replace(source, oldValue, regexPattern, regexGroupName, replacementValue, replacementAttributeName, template)

**Descripción:** Reemplaza los valores de una cadena de manera que distinga entre mayúsculas y minúsculas. La función se comporta de forma diferente según los parámetros proporcionados:

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

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto **source**. |
| **oldValue** |Opcional |String |Valor que se va a reemplazar en **source** o **template**. |
| **regexPattern** |Opcional |String |Patrón Regex del valor que se va a reemplazar en **source**. O bien, cuando se usa **replacementPropertyName**, patrón para extraer el valor de la propiedad **replacementPropertyName**. |
| **regexGroupName** |Opcional |String |Nombre del grupo dentro de **regexPattern**. Solo cuando se usa **replacementPropertyName**, extraeremos el valor de este grupo como **replacementValue** de **replacementPropertyName**. |
| **replacementValue** |Opcional |String |Nuevo valor para  reemplazar uno anterior. |
| **replacementAttributeName** |Opcional |String |Nombre del atributo que se usará para el valor de reemplazo |
| **template** |Opcional |String |Cuando se proporcione el valor de **template**, buscaremos **oldValue** dentro de la plantilla y lo reemplazaremos por el valor de **source**. |

#### <a name="replace-characters-using-a-regular-expression"></a>Reemplazar los caracteres con una expresión regular
Ejemplo: debe buscar los caracteres que coincidan con un valor de expresión regular y quitarlos.

**Expresión:** 

Replace([mailNickname], , "[a-zA-Z_]*", , "", , )

**Entrada/salida de ejemplo:**

* **INPUT** (mailNickname: "john_doe72"
* **OUTPUT**: "72"


---
### <a name="selectuniquevalue"></a>SelectUniqueValue
**Función:** SelectUniqueValue(uniqueValueRule1, uniqueValueRule2, uniqueValueRule3, …)

**Descripción:** Requiere dos argumentos como mínimo, que son las reglas de generación de valor único definidas con expresiones. La función evalúa cada regla y, a continuación, comprueba la unicidad del valor generado en el directorio o la aplicación de destino. Se devolverá el primer valor único encontrado. Si todos los valores ya existen en el destino, la entrada se depositará y el motivo se anota en los registros de auditoría. No hay ningún límite superior para el número de argumentos que se pueden proporcionar.


 - Esta función debe estar en el nivel superior y no se puede anidar.
 - Esta función no se puede aplicar a los atributos que tienen una precedencia de coincidencia.     
 - Esta función solo está destinada a usarse para creaciones de entradas. Al usarla con un atributo, establezca la propiedad **Apply Mapping** (Aplicar asignación) en **Solo durante la creación del objeto**.
 - Actualmente, esta función solo se admite en el aprovisionamiento de usuarios de Workday en Active Directory y en el de SuccessFactors en Active Directory. No se puede usar con otras aplicaciones de aprovisionamiento. 
 - La búsqueda LDAP que realiza la función *SelectUniqueValue* en Active Directory local no aplica el escape a caracteres especiales como signos diacríticos. Si pasa una cadena como "Jéssica Smith" que contiene un carácter especial, se producen errores de procesamiento. Anide la función [NormalizeDiacritics](#normalizediacritics) como se muestra en el siguiente ejemplo para normalizar los caracteres especiales. 


**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **uniqueValueRule1  … uniqueValueRuleN** |Al menos se requieren dos, sin límite superior |String | Lista de reglas de generación de valor único para realizar la evaluación. |

#### <a name="generate-unique-value-for-userprincipalname-upn-attribute"></a>Generación de un valor único para el atributo userPrincipalName (UPN)
Ejemplo: según el nombre del usuario, el segundo nombre y los apellidos, deberá generar un valor para el atributo UPN y comprobar si es único en el directorio de AD de destino antes de asignarlo al atributo UPN.

**Expresión:** 

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



---
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Función:** SingleAppRoleAssignment([appRoleAssignments])

**Descripción:** Devuelve una única función appRoleAssignment de la lista de todas las funciones appRoleAssignments asignadas a un usuario para una aplicación determinada. Esta función es necesaria para convertir el objeto appRoleAssignments en una cadena de nombre de rol único. El procedimiento recomendado consiste en asegurarse de que solo una función appRoleAssignment esté asignada a un usuario a la vez. Si se asignan varios roles, la cadena del rol devuelta podría no ser predecible. 

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Obligatorio |String |Objeto **[appRoleAssignments]** . |

---
### <a name="split"></a>Dividir
**Función:** Split(source, delimiter)

**Descripción:** divide una cadena en una matriz de varios valores, usando el carácter delimitador especificado.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |**de origen** que se actualiza. |
| **delimitador** |Obligatorio |String |Especifica el carácter que se usará para dividir la cadena (ejemplo: ","). |

#### <a name="split-a-string-into-a-multi-valued-array"></a>Dividir una cadena en una matriz con varios valores
Ejemplo: debe tomar una lista de cadenas delimitadas con comas y dividir esas cadenas en una matriz que se pueda incluir en un atributo de varios valores, como el atributo PermissionSets de Salesforce. En este ejemplo, una lista de conjuntos de permisos se ha rellenado en extensionAttribute5 en Azure AD.

**Expresión:** Split([extensionAttribute5], ",")

**Entrada/salida de ejemplo:** 

* **INPUT** (extensionAttribute5):
* **OUTPUT**:  ["PermissionSetOne", "PermissionSetTwo"]


---
### <a name="stripspaces"></a>StripSpaces
**Función:**  StripSpaces(source)

**Descripción:**  quita todos los caracteres de espacio (" ") de la cadena de origen.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |**de origen** que se actualiza. |

---
### <a name="switch"></a>Switch
**Función:**  Switch(source, defaultValue, key1, value1, key2, value2, …)

**Descripción:** Cuando el valor de **source** coincide con una **key**, devuelve el **value** de dicha **key**. Si el valor de **source** no coincide con ninguna clave, devuelve **defaultValue**.  Los parámetros **key** y **value** siempre deben estar emparejados. La función espera siempre un número par de parámetros. La función no se debe usar para atributos referenciales, como manager. 

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |**Source** que se actualiza. |
| **defaultValue** |Opcional |String |Valor predeterminado que se usará si el origen no coincide con ninguna clave. Puede tratarse de una cadena vacía (""). |
| **key** |Obligatorio |String |**Key** con que se compara el valor de **source**. |
| **value** |Obligatorio |String |Valor de reemplazo para el **source** que coincide con la clave. |

#### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Reemplazar un valor basado en un conjunto predefinido de opciones
Ejemplo: definir la zona horaria del usuario según el código de estado almacenado en Azure AD. Si el código de estado no coincide con ninguna de las opciones predefinidas, use el valor predeterminado de "Australia/Sídney".

**Expresión:**  
`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Entrada/salida de ejemplo:**

* **INPUT** (state): "QLD"
* **OUTPUT**: "Australia/Brisbane"


---
### <a name="tolower"></a>ToLower
**Función:** ToLower (origen, referencia cultural)

**Descripción:** Toma un valor de cadena de *origen* y se convierte a minúsculas mediante las reglas de referencia cultural que se hayan especificado. Si no hay ninguna información de *referencia cultural* especificada, se usará la referencia cultural invariable.

Si desea establecer los valores existentes en el sistema de destino en minúsculas, [actualice el esquema de la aplicación de destino](./customize-application-attributes.md#editing-the-list-of-supported-attributes) y establezca la propiedad caseExact en "true" para el atributo que sea de su interés. 

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto de origen |
| **referencia cultural** |Opcional |String |El formato para el nombre de la referencia cultural según RFC 4646 es *languagecode2-country/regioncode2*, donde *languagecode2* es el código de idioma de dos letras y *country/regioncode2* es el código de referencia de subcultura de dos letras. Algunos ejemplos son a ja-JP para japonés (Japón) y en-US para inglés (Estados Unidos). En casos donde un código de idioma de dos letras no está disponible, se usa un código de tres letras derivado de ISO 639-2.|

#### <a name="convert-generated-userprincipalname-upn-value-to-lower-case"></a>Conversión del valor generado de userPrincipalName (UPN) a minúsculas
Ejemplo: desea generar el valor de UPN concatenando los campos de origen PreferredFirstName y PreferredLastName, y convirtiendo todos los caracteres a minúsculas. 

`ToLower(Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"))`

**Entrada/salida de ejemplo:**

* **INPUT** (PreferredFirstName): "John"
* **INPUT** (PreferredLastName): "Smith"
* **OUTPUT**: "john.smith@contoso.com"


---
### <a name="toupper"></a>ToUpper
**Función:** ToUpper (origen, referencia cultural)

**Descripción:** Toma un valor de cadena de *origen* y se convierte a mayúsculas mediante las reglas de referencia cultural que se hayan especificado. Si no hay ninguna información de *referencia cultural* especificada, se usará la referencia cultural invariable.

Si desea establecer los valores existentes en el sistema de destino en mayúsculas, [actualice el esquema de la aplicación de destino](./customize-application-attributes.md#editing-the-list-of-supported-attributes) y establezca la propiedad caseExact en "true" para el atributo que sea de su interés. 

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **de origen** |Obligatorio |String |Normalmente el nombre del atributo del objeto de origen. |
| **referencia cultural** |Opcional |String |El formato para el nombre de la referencia cultural según RFC 4646 es *languagecode2-country/regioncode2*, donde *languagecode2* es el código de idioma de dos letras y *country/regioncode2* es el código de referencia de subcultura de dos letras. Algunos ejemplos son a ja-JP para japonés (Japón) y en-US para inglés (Estados Unidos). En casos donde un código de idioma de dos letras no está disponible, se usa un código de tres letras derivado de ISO 639-2.|

---
### <a name="word"></a>Word
**Función:** Word(String,WordNumber,Delimiters)

**Descripción:** : la función Word devuelve una palabra incluida en una cadena, según los parámetros que describen los delimitadores que se usarán y el número de palabras que se devolverán. : cada cadena de caracteres en la cadena separada por uno de los caracteres en los delimitadores se identifica como palabras:

Si el número es < 1, se devuelve una cadena vacía.
Si la cadena es Null, se devuelve una cadena vacía.
Si la cadena contiene menos palabras o si la cadena no contiene palabras identificadas por los delimitadores, se devuelve una cadena vacía.

**Parámetros:** 

| Nombre | Obligatorio/Repetición | Tipo | Notas |
| --- | --- | --- | --- |
| **String** |Obligatorio |Atributo multivalor |Cadena de la que se va a devolver una palabra.|
| **WordNumber** |Obligatorio | Entero | Número que identifica qué número de palabras debe devolver.|
| **delimiters** |Obligatorio |String| Cadena que representa los delimitadores que deben usarse para identificar palabras.|

**Ejemplo:** 
`Word("The quick brown fox",3," ")`

Devuelve "brown".

`Word("This,string!has&many separators",3,",!&#")`

Devuelve "has".

---

## <a name="examples"></a>Ejemplos
En esta sección se proporcionan más ejemplos de uso de las funciones de expresión. 

### <a name="strip-known-domain-name"></a>Seccionar un nombre de dominio conocido
Seccione un nombre de dominio conocido del correo electrónico de un usuario para obtener el nombre de usuario. Por ejemplo, si el dominio es "contoso.com", puede usar la expresión siguiente:

**Expresión:**  
`Replace([mail], "@contoso.com", , ,"", ,)`

**Ejemplo de entrada y salida:** 

* **ENTRADA** (mail): "john.doe@contoso.com"
* **SALIDA**: "john.doe"


### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Generar el alias de usuario concatenando partes de nombre y apellidos
Genere un alias de usuario con las tres primeras letras del nombre del usuario y las cinco primeras letras del apellido del usuario.

**Expresión:**  
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Entrada/salida de ejemplo:** 

* **INPUT** (givenName): "John"
* **INPUT** (surname): "Doe"
* **OUTPUT**:  "JohnDoe"


## <a name="related-articles"></a>Artículos relacionados
* [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md)
* [Personalización de asignaciones de atributos para el aprovisionamiento de usuarios](../app-provisioning/customize-application-attributes.md)
* [Filtros de ámbito para el aprovisionamiento de usuario](define-conditional-rules-for-provisioning-user-accounts.md)
* [Uso de SCIM para habilitar el aprovisionamiento automático de usuarios y grupos de Azure Active Directory a aplicaciones](../app-provisioning/use-scim-to-provision-users-and-groups.md)
* [Notificaciones de aprovisionamiento de cuentas](../app-provisioning/user-provisioning.md)
* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS](../saas-apps/tutorial-list.md)
