---
title: Funciones de expresiones en el flujo de datos de asignación
titleSuffix: Azure Data Factory & Azure Synapse
description: Obtenga información sobre las funciones de expresiones de Asignación de Data Flow.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.subservice: data-flows
ms.custom: synapse
ms.topic: conceptual
ms.date: 08/24/2021
ms.openlocfilehash: c0f0f36694e4eadccc4f7b2e9298e2a5ef27d31f
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122825186"
---
# <a name="data-transformation-expressions-in-mapping-data-flow"></a>Expresiones de transformación de datos en Asignación de Data Flow

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

## <a name="expression-functions"></a>Funciones de expresiones

En las canalizaciones de Data Factory y Synapse, use el lenguaje de expresiones de la característica de flujo de datos de asignación para configurar transformaciones de datos.
___
### <code>abs</code>
<code><b>abs(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
El valor absoluto de un número.  
* ``abs(-20) -> 20``  
* ``abs(10) -> 10``  
___   
### <code>acos</code>
<code><b>acos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula un valor inverso de coseno.  
* ``acos(1) -> 0.0``  
___
### <code>add</code>
<code><b>add(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Agrega un par de cadenas o números. Agrega una fecha a un número de días. Agrega una vigencia a una marca de tiempo. Anexa una matriz de tipo similar a otra. Igual que el operador +.  
* ``add(10, 20) -> 30``  
* ``10 + 20 -> 30``  
* ``add('ice', 'cream') -> 'icecream'``  
* ``'ice' + 'cream' + ' cone' -> 'icecream cone'``  
* ``add(toDate('2012-12-12'), 3) -> toDate('2012-12-15')``  
* ``toDate('2012-12-12') + 3 -> toDate('2012-12-15')``  
* ``[10, 20] + [30, 40] -> [10, 20, 30, 40]``  
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``  
___
### <code>addDays</code>
<code><b>addDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Agrega días a una fecha o marca de tiempo. Igual que el operador + para la fecha.  
* ``addDays(toDate('2016-08-08'), 1) -> toDate('2016-08-09')``  
___
### <code>addMonths</code>
<code><b>addMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to add&gt;</i> : integral, [<i>&lt;value3&gt;</i> : string]) => datetime</b></code><br/><br/>
Suma meses a una fecha o marca de tiempo. Opcionalmente, puede pasar una zona horaria.  
* ``addMonths(toDate('2016-08-31'), 1) -> toDate('2016-09-30')``  
* ``addMonths(toTimestamp('2016-09-30 10:10:10'), -1) -> toTimestamp('2016-08-31 10:10:10')``  
___
### <code>and</code>
<code><b>and(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Operador lógico AND. Igual que &&.  
* ``and(true, false) -> false``  
* ``true && false -> false``  
___
### <code>asin</code>
<code><b>asin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula un valor inverso de seno.  
* ``asin(0) -> 0.0``  
___
### <code>atan</code>
<code><b>atan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula un valor inverso de tangente.  
* ``atan(0) -> 0.0``  
___
### <code>atan2</code>
<code><b>atan2(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Devuelve el ángulo en radianes entre el eje X positivo de un plano y el punto que especificaron las coordenadas.  
* ``atan2(0, 0) -> 0.0``  
___
### <code>between</code>
<code><b>between(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any, <i>&lt;value3&gt;</i> : any) => boolean</b></code><br/><br/>
Comprueba si el primer valor se encuentra entre otros dos valores, ambos inclusive. Se pueden comparar valores numéricos, de cadena y de fecha y hora * ``between(10, 5, 24)``
* ``true``
* ``between(currentDate(), currentDate() + 10, currentDate() + 20)``
* ``false``
___
### <code>bitwiseAnd</code>
<code><b>bitwiseAnd(<i>&lt;value1&gt;</i> : integral, <i>&lt;value2&gt;</i> : integral) => integral</b></code><br/><br/>
Operador AND bit a bit entre tipos enteros. Igual que el operador & * ``bitwiseAnd(0xf4, 0xef)``
* ``0xe4``
* ``(0xf4 & 0xef)``
* ``0xe4``
___
### <code>bitwiseOr</code>
<code><b>bitwiseOr(<i>&lt;value1&gt;</i> : integral, <i>&lt;value2&gt;</i> : integral) => integral</b></code><br/><br/>
Operador OR bit a bit entre tipos enteros. Igual que el operador | * ``bitwiseOr(0xf4, 0xef)``
* ``0xff``
* ``(0xf4 | 0xef)``
* ``0xff``
___
### <code>bitwiseXor</code>
<code><b>bitwiseXor(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Operador OR bit a bit entre tipos enteros. Igual que el operador | * ``bitwiseXor(0xf4, 0xef)``
* ``0x1b``
* ``(0xf4 ^ 0xef)``
* ``0x1b``
* ``(true ^ false)``
* ``true``
* ``(true ^ true)``
* ``false``
___
### <code>blake2b</code>
<code><b>blake2b(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Calcula la síntesis del mensaje Blake2 del conjunto de columnas de diferentes tipos de datos primitivos dada una longitud en bits que solo pueden ser múltiplos de 8 entre 8 y 512. Se puede usar para calcular una huella digital de una fila * ``blake2b(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4'))``
* ``'c9521a5080d8da30dffb430c50ce253c345cc4c4effc315dab2162dac974711d'``
___
### <code>blake2bBinary</code>
<code><b>blake2bBinary(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => binary</b></code><br/><br/>
Calcula la síntesis del mensaje Blake2 del conjunto de columnas de diferentes tipos de datos primitivos dada una longitud en bits que solo pueden ser múltiplos de 8 entre 8 y 512. Se puede usar para calcular una huella digital de una fila * ``blake2bBinary(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4'))``
* ``unHex('c9521a5080d8da30dffb430c50ce253c345cc4c4effc315dab2162dac974711d')``
___
### <code>case</code>
<code><b>case(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, <i>&lt;false_expression&gt;</i> : any, ...) => any</b></code><br/><br/>
Basándose en una condición alternativa, aplica un valor o el otro. Si el número de entradas es par, el otro se establece de manera predeterminada en NULL para la última condición.  
* ``case(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``  
* ``case(10 + 20 == 25, 'bojjus', 'do' < 'go', 'gunchus') -> 'gunchus'``  
* ``isNull(case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus')) -> true``  
* ``case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus', 'dumbo') -> 'dumbo'``  
___
### <code>cbrt</code>
<code><b>cbrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula la raíz cúbica de un número.  
* ``cbrt(8) -> 2.0``  
___
### <code>ceil</code>
<code><b>ceil(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Devuelve el entero más pequeño no menor que el número.  
* ``ceil(-0.1) -> 0``  
___
### <code>coalesce</code>
<code><b>coalesce(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Devuelve el primer valor no NULL de un conjunto de entradas. Todas las entradas deben ser del mismo tipo.  
* ``coalesce(10, 20) -> 10``  
* ``coalesce(toString(null), toString(null), 'dumbo', 'bo', 'go') -> 'dumbo'``  
___
### <code>columnNames</code>
<code><b>columnNames(<i>&lt;value1&gt;</i> : string) => array</b></code><br/><br/>
Obtiene los nombres de todas las columnas de salida de una secuencia. Puede pasar un nombre de secuencia opcional como segundo argumento.  
* ``columnNames()``
* ``columnNames('DeriveStream')``
___
### <code>columns</code>
<code><b>columns([<i>&lt;stream name&gt;</i> : string]) => any</b></code><br/><br/>
Obtiene los valores de todas las columnas de salida de una secuencia. Puede pasar un nombre de secuencia opcional como segundo argumento.   
* ``columns()``
* ``columns('DeriveStream')``
___
### <code>compare</code>
<code><b>compare(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => integer</b></code><br/><br/>
Compara dos valores del mismo tipo. Devuelve un entero negativo si value1 < value2, 0 si value1 == value2, un valor positivo si value1 > value2.  
* ``(compare(12, 24) < 1) -> true``  
* ``(compare('dumbo', 'dum') > 0) -> true``  
___
### <code>concat</code>
<code><b>concat(<i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Concatena un número variable de cadenas. Igual que el operador + con cadenas.  
* ``concat('dataflow', 'is', 'awesome') -> 'dataflowisawesome'``  
* ``'dataflow' + 'is' + 'awesome' -> 'dataflowisawesome'``  
* ``isNull('sql' + null) -> true``  
___
### <code>concatWS</code>
<code><b>concatWS(<i>&lt;separator&gt;</i> : string, <i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Concatena un número variable de cadenas con un separador. El primer parámetro es el separador.  
* ``concatWS(' ', 'dataflow', 'is', 'awesome') -> 'dataflow is awesome'``  
* ``isNull(concatWS(null, 'dataflow', 'is', 'awesome')) -> true``  
* ``concatWS(' is ', 'dataflow', 'awesome') -> 'dataflow is awesome'``  
___
### <code>cos</code>
<code><b>cos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula un valor de coseno.  
* ``cos(10) -> -0.8390715290764524``  
___
### <code>cosh</code>
<code><b>cosh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula un coseno hiperbólico de un valor.  
* ``cosh(0) -> 1.0``  
___
### <code>crc32</code>
<code><b>crc32(<i>&lt;value1&gt;</i> : any, ...) => long</b></code><br/><br/>
Calcula la síntesis del mensaje CRC32 del conjunto de columnas de diferentes tipos de datos primitivos dada una longitud en bits, que solo puede tener los valores 0(256), 224, 256, 384 y 512. Se puede usar para calcular una huella digital de una fila.  
* ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689L``  
___
### <code>currentDate</code>
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
Obtiene la fecha actual cuando este trabajo empieza a ejecutarse. Puede pasar una zona horaria opcional en forma de "GMT", "PST", "UTC", "America/Cayman". La zona horaria local se utiliza como el valor predeterminado. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html). 
* ``currentDate() == toDate('2250-12-31') -> false``  
* ``currentDate('PST')  == toDate('2250-12-31') -> false``  
* ``currentDate('America/New_York')  == toDate('2250-12-31') -> false``  
___
### <code>currentTimestamp</code>
<code><b>currentTimestamp() => timestamp</b></code><br/><br/>
Obtiene la marca de tiempo actual cuando se inicia el trabajo para ejecutarse con la zona horaria local.  
* ``currentTimestamp() == toTimestamp('2250-12-31 12:12:12') -> false``  
___
### <code>currentUTC</code>
<code><b>currentUTC([<i>&lt;value1&gt;</i> : string]) => timestamp</b></code><br/><br/>
Obtiene la marca de tiempo actual como hora UTC. Si quiere que la hora actual se interprete en una zona horaria distinta a la del clúster, puede pasar una zona horaria opcional con el formato "GMT", "PST", "UTC ", "America/Caimán". La zona horaria actual se establece como predeterminada. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html). Para convertir la zona horaria UTC a una distinta, use `fromUTC()`.  
* ``currentUTC() == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``currentUTC() != toTimestamp('2050-12-12 19:18:12') -> true``  
* ``fromUTC(currentUTC(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>dayOfMonth</code>
<code><b>dayOfMonth(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Obtiene el día del mes dada una fecha.  
* ``dayOfMonth(toDate('2018-06-08')) -> 8``  
___
### <code>dayOfWeek</code>
<code><b>dayOfWeek(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Obtiene el día de la semana dada una fecha. 1 corresponde al domingo, 2 al lunes... y 7 al sábado.  
* ``dayOfWeek(toDate('2018-06-08')) -> 6``  
___
### <code>dayOfYear</code>
<code><b>dayOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Obtiene el día del año dada una fecha.  
* ``dayOfYear(toDate('2016-04-09')) -> 100``  
___
### <code>days</code>
<code><b>days(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duración en milisegundos para el número de días.  
* ``days(2) -> 172800000L``  
___
### <code>degrees</code>
<code><b>degrees(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Convierte radianes en grados.  
* ``degrees(3.141592653589793) -> 180``  
___
### <code>divide</code>
<code><b>divide(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Divide dos números. Igual que el operador `/`.  
* ``divide(20, 10) -> 2``  
* ``20 / 10 -> 2``
___
### <code>dropLeft</code>
<code><b>dropLeft(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : integer) => string</b></code><br/><br/>
Quita los mismos caracteres de la izquierda de la cadena. Si la colocación solicitada supera la longitud de la cadena, se devuelve una cadena vacía.
*   dropLeft('bojjus', 2) => 'jjus' *   dropLeft('cake', 10) => '' ___
### <code>dropRight</code>
<code><b>dropRight(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : integer) => string</b></code><br/><br/>
Quita los mismos caracteres de la derecha de la cadena. Si la colocación solicitada supera la longitud de la cadena, se devuelve una cadena vacía.
*   dropRight('bojjus', 2) => 'bojj' *   dropRight('cake', 10) => '' ___
### <code>endsWith</code>
<code><b>endsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Comprueba si la cadena finaliza con la cadena proporcionada.  
* ``endsWith('dumbo', 'mbo') -> true``  
___
### <code>equals</code>
<code><b>equals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Operador de comparación igual que. Igual que el operador ==.  
* ``equals(12, 24) -> false``  
* ``12 == 24 -> false``  
* ``'bad' == 'bad' -> true``  
* ``isNull('good' == toString(null)) -> true``  
* ``isNull(null == null) -> true``  
___
### <code>equalsIgnoreCase</code>
<code><b>equalsIgnoreCase(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => boolean</b></code><br/><br/>
Operador de comparación igual que, sin distinción entre mayúsculas y minúsculas. Igual que el operador <=>.  
* ``'abc'<=>'Abc' -> true``  
* ``equalsIgnoreCase('abc', 'Abc') -> true``  
___
### <code>escape</code>
<code><b>escape(<i>&lt;string_to_escape&gt;</i> : string, <i>&lt;format&gt;</i> : string) => string</b></code><br/><br/>
Escapa una cadena según un formato. Los valores literales para el formato aceptable son "json", "xml", "ecmascript", "html", "java".
___
### <code>expr</code>
<code><b>expr(<i>&lt;expr&gt;</i> : string) => any</b></code><br/><br/>
Da como resultado una expresión de una cadena. Esto es lo mismo que escribir esta expresión de forma no literal. Se puede usar para pasar parámetros como representaciones de cadena.
*   expr(‘price * discount’) => any ___
### <code>factorial</code>
<code><b>factorial(<i>&lt;value1&gt;</i> : number) => long</b></code><br/><br/>
Calcula el valor factorial de un número.  
* ``factorial(5) -> 120``  
___
### <code>false</code>
<code><b>false() => boolean</b></code><br/><br/>
Siempre devuelve un valor false. Utilice la función `syntax(false())` si hay una columna denominada "false".  
* ``(10 + 20 > 30) -> false``  
* ``(10 + 20 > 30) -> false()``
___
### <code>floor</code>
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Devuelve el entero más grande no mayor que el número.  
* ``floor(-0.1) -> -1``  
___
### <code>fromBase64</code>
<code><b>fromBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Descodifica la cadena con codificación base64 especificada.
* ``fromBase64('Z3VuY2h1cw==') -> 'gunchus'``  
___
### <code>fromUTC</code>
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Convierte a la marca de tiempo de UTC. Si lo desea, puede pasar una zona horaria opcional en forma de "GMT", "PST", "UTC", "America/Cayman". La zona horaria actual se establece como predeterminada. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``fromUTC(currentTimestamp()) == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``fromUTC(currentTimestamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>greater</code>
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Operador de comparación mayor que. Igual que el operador >.  
* ``greater(12, 24) -> false``  
* ``('dumbo' > 'dum') -> true``  
* ``(toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS') > toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``  
___
### <code>greaterOrEqual</code>
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Operador de comparación mayor que o igual que. Igual que el operador >=.  
* ``greaterOrEqual(12, 12) -> true``  
* ``('dumbo' >= 'dum') -> true``  
___
### <code>greatest</code>
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Devuelve el valor mayor entre la lista de valores como entrada, omitiendo los valores NULL. Devuelve NULL si todas las entradas son NULL.  
* ``greatest(10, 30, 15, 20) -> 30``  
* ``greatest(10, toInteger(null), 20) -> 20``  
* ``greatest(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2011-12-12')``  
* ``greatest(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS'), toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')``  
___
### <code>hasColumn</code>
<code><b>hasColumn(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => boolean</b></code><br/><br/>
Comprueba un valor de columna por el nombre en la secuencia. Puede pasar un nombre de flujo opcional como segundo argumento. Los nombres de columna que se conocen en tiempo de diseño deben tratarse simplemente por su nombre. No se admiten entradas calculadas, pero se pueden usar sustituciones de parámetros.  
* ``hasColumn('parent')``  
___
### <code>hour</code>
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Obtiene el valor de la hora de una marca de tiempo. Puede pasar una zona horaria opcional en forma de "GMT", "PST", "UTC", "America/Cayman". La zona horaria local se utiliza como el valor predeterminado. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``hour(toTimestamp('2009-07-30 12:58:59')) -> 12``  
* ``hour(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 12``  
___
### <code>hours</code>
<code><b>hours(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duración en milisegundos para el número de horas.  
* ``hours(2) -> 7200000L``  
___
### <code>iif</code>
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></code><br/><br/>
Basándose en una condición, aplica un valor o el otro. Si "Otro" si no se especifica, se considera NULL. Ambos valores deben ser compatibles (numéricos, cadena...). * ``iif(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``  
* ``iif(10 > 30, 'dumbo', 'gumbo') -> 'gumbo'``  
* ``iif(month(toDate('2018-12-01')) == 12, 345.12, 102.67) -> 345.12``  
___
### <code>iifNull</code>
<code><b>iifNull(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => any</b></code><br/><br/>
Comprueba si el primer parámetro es NULL. Si no es NULL, se devuelve el primer parámetro. Si es NULL, se devuelve el segundo parámetro. Si se especifican tres parámetros, el comportamiento es el mismo que iif(isNull(value1), value2, value3) y se devuelve el tercer parámetro si el primer valor no es NULL.  
* ``iifNull(10, 20) -> 10``  
* ``iifNull(null, 20, 40) -> 20``  
* ``iifNull('azure', 'data', 'factory') -> 'factory'``  
* ``iifNull(null, 'data', 'factory') -> 'data'``  
___
### <code>initCap</code>
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Convierte la primera letra de cada palabra en mayúsculas. Las palabras se identifican como separadas por espacios en blanco.  
* ``initCap('cool iceCREAM') -> 'Cool Icecream'``  
___
### <code>instr</code>
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></code><br/><br/>
Busca la posición (basada en 1) de la subcadena dentro de una cadena. Se devuelve 0 si no se encuentra.  
* ``instr('dumbo', 'mbo') -> 3``  
* ``instr('microsoft', 'o') -> 5``  
* ``instr('good', 'bad') -> 0``  
___
### <code>isDelete</code>
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Comprueba si la fila está marcada para eliminar. En las transformaciones que toman más de un flujo de entrada, se puede pasar el índice (basado en 1) del flujo. El índice de flujo debe ser 1 o 2 y el valor predeterminado es 1.  
* ``isDelete()``  
* ``isDelete(1)``  
___
### <code>isError</code>
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Comprueba si la fila se marca como error. En las transformaciones que toman más de un flujo de entrada, se puede pasar el índice (basado en 1) del flujo. El índice de flujo debe ser 1 o 2 y el valor predeterminado es 1.  
* ``isError()``  
* ``isError(1)``  
___
### <code>isIgnore</code>
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Comprueba si la fila se marca para pasarse por alto. En las transformaciones que toman más de un flujo de entrada, se puede pasar el índice (basado en 1) del flujo. El índice de flujo debe ser 1 o 2 y el valor predeterminado es 1.  
* ``isIgnore()``  
* ``isIgnore(1)``  
___
### <code>isInsert</code>
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Comprueba si la fila está marcada para insertar. En las transformaciones que toman más de un flujo de entrada, se puede pasar el índice (basado en 1) del flujo. El índice de flujo debe ser 1 o 2 y el valor predeterminado es 1.  
* ``isInsert()``  
* ``isInsert(1)``  
___
### <code>isMatch</code>
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Comprueba si la fila cumplía los criterios de coincidencia en la búsqueda. En las transformaciones que toman más de un flujo de entrada, se puede pasar el índice (basado en 1) del flujo. El índice de flujo debe ser 1 o 2 y el valor predeterminado es 1.  
* ``isMatch()``  
* ``isMatch(1)``  
___
### <code>isNull</code>
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Comprueba si el valor es NULL.  
* ``isNull(NULL()) -> true``  
* ``isNull('') -> false``  
___
### <code>isUpdate</code>
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Comprueba si la fila está marcada para actualizar. En las transformaciones que toman más de un flujo de entrada, se puede pasar el índice (basado en 1) del flujo. El índice de flujo debe ser 1 o 2 y el valor predeterminado es 1.  
* ``isUpdate()``  
* ``isUpdate(1)``  
___
### <code>isUpsert</code>
<code><b>isUpsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Comprueba si la fila está marcada para insertar. En las transformaciones que toman más de un flujo de entrada, se puede pasar el índice (basado en 1) del flujo. El índice de flujo debe ser 1 o 2 y el valor predeterminado es 1.  
* ``isUpsert()``  
* ``isUpsert(1)``  
___
### <code>jaroWinkler</code>
<code><b>jaroWinkler(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => double</b></code><br/><br/>
Obtiene la distancia de JaroWrinkler entre dos cadenas. 
* ``jaroWinkler('frog', 'frog') => 1.0``  
___
### <code>lastDayOfMonth</code>
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></code><br/><br/>
Obtiene el último día del mes dada una fecha.  
* ``lastDayOfMonth(toDate('2009-01-12')) -> toDate('2009-01-31')``  
___
### <code>least</code>
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Operador de comparación menor que o igual que. Igual que el operador <=.  
* ``least(10, 30, 15, 20) -> 10``  
* ``least(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2000-12-12')``  
___
### <code>left</code>
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Extrae un inicio de la subcadena en el índice 1 con el número de caracteres. Igual que SUBSTRING(str, 1, n).  
* ``left('bojjus', 2) -> 'bo'``  
* ``left('bojjus', 20) -> 'bojjus'``  
___
### <code>length</code>
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></code><br/><br/>
Devuelve la longitud de la cadena.  
* ``length('dumbo') -> 5``  
___
### <code>lesser</code>
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Operador de comparación menor que. Igual que el operador <.  
* ``lesser(12, 24) -> true``  
* ``('abcd' < 'abc') -> false``  
* ``(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') < toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``  
___
### <code>lesserOrEqual</code>
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Operador de comparación menor que o igual que. Igual que el operador <=.  
* ``lesserOrEqual(12, 12) -> true``  
* ``('dumbo' <= 'dum') -> false``  
___
### <code>levenshtein</code>
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></code><br/><br/>
Obtiene la distancia de edición entre dos cadenas.  
* ``levenshtein('boys', 'girls') -> 4``  
___
### <code>like</code>
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
El patrón es una cadena que se hace coincidir literalmente. La excepción son los siguientes símbolos especiales: _ coincide con cualquier carácter en la entrada (similar a . en expresiones regulares ```posix```) y % coincide con cero o más caracteres en la entrada (similar a .* en expresiones regulares ```posix```).
El carácter de escape es ". Si un carácter de escape precede a un símbolo especial u otro carácter de escape, el carácter siguiente se hace coincidir literalmente. No es válido escapar cualquier otro carácter.  
* ``like('icecream', 'ice%') -> true``  
___
### <code>locate</code>
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Busca la posición (basada en 1) de la subcadena dentro de una cadena a partir de una posición determinada. Si la posición se omite se considera desde el principio de la cadena. Se devuelve 0 si no se encuentra.  
* ``locate('mbo', 'dumbo') -> 3``  
* ``locate('o', 'microsoft', 6) -> 7``  
* ``locate('bad', 'good') -> 0``  
___
### <code>log</code>
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></code><br/><br/>
Calcula el valor del logaritmo. Se puede suministrar una base opcional o un número de Euler si se usa.  
* ``log(100, 10) -> 2``  
___
### <code>log10</code>
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula el valor del logaritmo en base 10.  
* ``log10(100) -> 2``  
___
### <code>lower</code>
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Pone en minúsculas una cadena.  
* ``lower('GunChus') -> 'gunchus'``  
___
### <code>lpad</code>
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Left rellena la cadena mediante el carácter de relleno proporcionado hasta que se alcanza una determinada longitud. Si la cadena es igual o mayor que la longitud, se recorta para cumplir con la longitud.  
* ``lpad('dumbo', 10, '-') -> '-----dumbo'``  
* ``lpad('dumbo', 4, '-') -> 'dumb'``  
* ' ' LPAD (' Dumbo ', 8, ' < > ')-> ' < > <dumbo'``  
___
### <code>ltrim</code>
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Left recorta una cadena de caracteres iniciales. Si no se especifica el segundo parámetro, recorta el espacio en blanco. Si no, recorta cualquier carácter especificado en el segundo parámetro.  
* ``ltrim('  dumbo  ') -> 'dumbo  '``  
* ``ltrim('!--!du!mbo!', '-!') -> 'du!mbo!'``  
___
### <code>md5</code>
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Calcula la síntesis del mensaje MD5 del conjunto de columnas de diferentes tipos de datos primitivos y devuelve una cadena hexadecimal de 32 caracteres. Se puede usar para calcular una huella digital de una fila.  
* ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '4ce8a880bd621a1ffad0bca905e1bc5a'``  
___
### <code>millisecond</code>
<code><b>millisecond(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Obtiene el valor de los milisegundos de una fecha. Puede pasar una zona horaria opcional en forma de "GMT", "PST", "UTC", "America/Cayman". La zona horaria local se utiliza como el valor predeterminado. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``millisecond(toTimestamp('2009-07-30 12:58:59.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``  
___
### <code>milliseconds</code>
<code><b>milliseconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duración en milisegundos para el número de milisegundos.  
* ``milliseconds(2) -> 2L``  
___
### <code>minus</code>
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Resta números. Restar el número de días a partir de una fecha. Resta la vigencia de una marca de tiempo. Resta dos marcas de tiempo para obtener la diferencia en milisegundos. Igual que el operador -.  
* ``minus(20, 10) -> 10``  
* ``20 - 10 -> 10``  
* ``minus(toDate('2012-12-15'), 3) -> toDate('2012-12-12')``  
* ``toDate('2012-12-15') - 3 -> toDate('2012-12-12')``  
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``  
* ``toTimestamp('2019-02-03 05:21:34.851', 'yyyy-MM-dd HH:mm:ss.SSS') - toTimestamp('2019-02-03 05:21:36.923', 'yyyy-MM-dd HH:mm:ss.SSS') -> -2072``  
___
### <code>minute</code>
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Obtiene el valor de los minutos de una marca de tiempo. Puede pasar una zona horaria opcional en forma de "GMT", "PST", "UTC", "America/Cayman". La zona horaria local se utiliza como el valor predeterminado. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``minute(toTimestamp('2009-07-30 12:58:59')) -> 58``  
* ``minute(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 58``  
___
### <code>minutes</code>
<code><b>minutes(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duración en milisegundos para el número de minutos.  
* ``minutes(2) -> 120000L``  
___
### <code>mod</code>
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Módulo de par de números. Igual que el operador %.  
* ``mod(20, 8) -> 4``  
* ``20 % 8 -> 4``  
___
### <code>month</code>
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Obtiene el valor del mes de una fecha o una marca de tiempo.  
* ``month(toDate('2012-8-8')) -> 8``  
___
### <code>monthsBetween</code>
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;roundoff&gt;</i> : boolean], [<i>&lt;time zone&gt;</i> : string]) => double</b></code><br/><br/>
Obtiene el número de meses entre dos fechas. Puede redondear el cálculo. Puede pasar una zona horaria opcional con el formato "GMT", "PST", "UTC", "America/Cayman". La zona horaria local se utiliza como el valor predeterminado. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``monthsBetween(toTimestamp('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``  
___
### <code>multiply</code>
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Multiplica dos números. Igual que el operador *.  
* ``multiply(20, 10) -> 200``  
* ``20 * 10 -> 200``  
___
### <code>negate</code>
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Niega un número. Convierte los números positivos en negativos y viceversa.  
* ``negate(13) -> -13``  
___
### <code>nextSequence</code>
<code><b>nextSequence() => long</b></code><br/><br/>
Devuelve la siguiente secuencia única. El número es consecutivo solo dentro de una partición y viene precedido por el identificador de la partición.  
* ``nextSequence() == 12313112 -> false``  
___
### <code>normalize</code>
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></code><br/><br/>
Normaliza el valor de la cadena para separar caracteres Unicode acentuados.  
* ``regexReplace(normalize('bo²s'), `\p{M}`, '') -> 'boys'``
___
### <code>not</code>
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></code><br/><br/>
Operador de negación lógica.  
* ``not(true) -> false``  
* ``not(10 == 20) -> true``  
___
### <code>notEquals</code>
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Operador de comparación no igual que. Igual que el operador !=.  
* ``12 != 24 -> true``  
* ``'bojjus' != 'bo' + 'jjus' -> false``  
___
### <code>notNull</code>
<code><b>notNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Comprueba si el valor no es NULL.  
* ``notNull(NULL()) -> false``  
* ``notNull('') -> true``  
___
### <code>null</code>
<code><b>null() => null</b></code><br/><br/>
Devuelve un valor NULL. Use la función `syntax(null())` si hay una columna denominada "null". Cualquier operación que use dará como resultado un valor NULL.  
* ``isNull('dumbo' + null) -> true``  
* ``isNull(10 * null) -> true``  
* ``isNull('') -> false``  
* ``isNull(10 + 20) -> false``  
* ``isNull(10/0) -> true``  
___
### <code>or</code>
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Operador lógico OR. Igual que ||.  
* ``or(true, false) -> true``  
* ``true || false -> true``  
___
### <code>pMod</code>
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Módulo positivo de par de números.  
* ``pmod(-20, 8) -> 4``  
___
### <code>partitionId</code>
<code><b>partitionId() => integer</b></code><br/><br/>
Devuelve el identificador de la partición actual en donde se encuentra la fila de entrada.  
* ``partitionId()``  
___
### <code>power</code>
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Eleva un número a la potencia de otro.  
* ``power(10, 2) -> 100``  
___
### <code>radians</code>
<code><b>radians(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Convierte los grados en radianes * ``radians(180) => 3.141592653589793``  
___
### <code>random</code>
<code><b>random(<i>&lt;value1&gt;</i> : integral) => long</b></code><br/><br/>
Devuelve un número aleatorio dado un valor de inicialización opcional dentro de una partición. El valor de inicialización debe ser un valor fijo y se usa junto con partitionId para generar valores aleatorios. * ``random(1) == 1 -> false``
___
### <code>regexExtract</code>
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></code><br/><br/>
Extrae una subcadena coincidente para un patrón de expresión regular especificado. El último parámetro identifica el grupo de coincidencia y, si se omite, se usa el valor predeterminado 1. Use `<regex>` (comilla inversa) para que coincida con una cadena sin escape.  
* ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``  
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``  
___
### <code>regexMatch</code>
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></code><br/><br/>
Comprueba si la cadena coincide con el patrón de expresión regular especificado. Use `<regex>` (comilla inversa) para que coincida con una cadena sin escape.  
* ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``  
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``  
___
### <code>regexReplace</code>
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Reemplaza todas las repeticiones de un patrón de expresión regular por otra subcadena en la cadena dada. Use `<regex>` (comilla inversa) para que coincida con una cadena sin escape.  
* ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``  
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``  
___
### <code>regexSplit</code>
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></code><br/><br/>
Divide una cadena según un delimitador basándose en la expresión regular y devuelve una matriz de cadenas.  
* ``regexSplit('bojjusAgunchusBdumbo', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo']``  
* ``regexSplit('bojjusAgunchusBdumboC', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo', '']``  
* ``(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[1]) -> 'bojjus'``  
* ``isNull(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[20]) -> true``  
___
### <code>replace</code>
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, [<i>&lt;substring to replace&gt;</i> : string]) => string</b></code><br/><br/>
Reemplaza todas las repeticiones de una subcadena por otra subcadena en la cadena dada. Si se omite el último parámetro, el valor predeterminado es una cadena vacía.  
* ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'``  
* ``replace('doggie dog', 'dog', '') -> 'gie '``  
* ``replace('doggie dog', 'dog') -> 'gie '``  
___
### <code>reverse</code>
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Invierte una cadena.  
* ``reverse('gunchus') -> 'suhcnug'``  
___
### <code>right</code>
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Extrae una subcadena con el número de caracteres de la derecha. Igual que SUBSTRING(str, LENGTH(str) - n, n).  
* ``right('bojjus', 2) -> 'us'``  
* ``right('bojjus', 20) -> 'bojjus'``  
___
### <code>rlike</code>
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Comprueba si la cadena coincide con el patrón de expresión regular especificado.  
* ``rlike('200.50', `(\d+).(\d+)`) -> true``  
* ``rlike('bogus', `M[0-9]+.*`) -> false``  
___
### <code>round</code>
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></code><br/><br/>
Redondea un número con una escala opcional y un modo de redondeo opcional dados. Si la escala se omite, se establece en 0 de forma predeterminada. Si el modo se omite, se establece en ROUND_HALF_UP(5) de forma predeterminada. Los valores de redondeo incluyen 1 - ROUND_UP 2 - ROUND_DOWN 3 - ROUND_CEILING 4 - ROUND_FLOOR 5 - ROUND_HALF_UP 6 - ROUND_HALF_DOWN 7 - ROUND_HALF_EVEN 8 - ROUND_UNNECESSARY.  
* ``round(100.123) -> 100.0``  
* ``round(2.5, 0) -> 3.0``  
* ``round(5.3999999999999995, 2, 7) -> 5.40``  
___
### <code>rpad</code>
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Right rellena la cadena mediante el carácter de relleno proporcionado hasta que se alcanza una determinada longitud. Si la cadena es igual o mayor que la longitud, se recorta para cumplir con la longitud.  
* ``rpad('dumbo', 10, '-') -> 'dumbo-----'``  
* ``rpad('dumbo', 4, '-') -> 'dumb'``  
* ``rpad('dumbo', 8, '<>') -> 'dumbo<><'``  
___
### <code>rtrim</code>rtrim</code>
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Right recorta una cadena de caracteres finales. Si no se especifica el segundo parámetro, recorta el espacio en blanco. Si no, recorta cualquier carácter especificado en el segundo parámetro.  
* ``rtrim('  dumbo  ') -> '  dumbo'``  
* ``rtrim('!--!du!mbo!', '-!') -> '!--!du!mbo'``  
___
### <code>second</code>
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Obtiene el valor de los segundos de una fecha. Puede pasar una zona horaria opcional en forma de "GMT", "PST", "UTC", "America/Cayman". La zona horaria local se utiliza como el valor predeterminado. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``second(toTimestamp('2009-07-30 12:58:59')) -> 59``  
___
### <code>seconds</code>
<code><b>seconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duración en milisegundos para el número de segundos.  
* ``seconds(2) -> 2000L``  
___
### <code>sha1</code>
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Calcula la síntesis del mensaje SHA-1 del conjunto de columnas de diferentes tipos de datos primitivos y devuelve una cadena hexadecimal de 40 caracteres. Se puede usar para calcular una huella digital de una fila.  
* ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '46d3b478e8ec4e1f3b453ac3d8e59d5854e282bb'``  
___
### <code>sha2</code>
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Calcula la síntesis del mensaje SHA-2 del conjunto de columnas de diferentes tipos de datos primitivos dada una longitud en bits, que solo puede tener los valores 0(256), 224, 256, 384 y 512. Se puede usar para calcular una huella digital de una fila.  
* ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'afe8a553b1761c67d76f8c31ceef7f71b66a1ee6f4e6d3b5478bf68b47d06bd3'``  
___
### <code>sin</code>
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula un valor de seno.  
* ``sin(2) -> 0.9092974268256817``  
___
### <code>sinh</code>
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula un valor de seno hiperbólico.  
* ``sinh(0) -> 0.0``  
___
### <code>soundex</code>
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Obtiene el código ```soundex``` de la cadena.  
* ``soundex('genius') -> 'G520'``  
___
### <code>split</code>
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></code><br/><br/>
Divide una cadena según un delimitador y devuelve una matriz de cadenas.  
* ``split('bojjus,guchus,dumbo', ',') -> ['bojjus', 'guchus', 'dumbo']``  
* ``split('bojjus,guchus,dumbo', '|') -> ['bojjus,guchus,dumbo']``  
* ``split('bojjus, guchus, dumbo', ', ') -> ['bojjus', 'guchus', 'dumbo']``  
* ``split('bojjus, guchus, dumbo', ', ')[1] -> 'bojjus'``  
* ``isNull(split('bojjus, guchus, dumbo', ', ')[0]) -> true``  
* ``isNull(split('bojjus, guchus, dumbo', ', ')[20]) -> true``  
* ``split('bojjusguchusdumbo', ',') -> ['bojjusguchusdumbo']``  
___
### <code>sqrt</code>
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula la raíz cuadrada de un número.  
* ``sqrt(9) -> 3``  
___
### <code>startsWith</code>
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Comprueba si la cadena comienza por la cadena proporcionada.  
* ``startsWith('dumbo', 'du') -> true``  
___
### <code>subDays</code>
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Resta días de una fecha o una marca de tiempo. Igual que el operador - para la fecha.  
* ``subDays(toDate('2016-08-08'), 1) -> toDate('2016-08-07')``  
___
### <code>subMonths</code>
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Resta meses a una fecha o marca de tiempo.  
* ``subMonths(toDate('2016-09-30'), 1) -> toDate('2016-08-31')``  
___
### <code>substring</code>
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></code><br/><br/>
Extrae una subcadena de una determinada longitud de una posición. La posición se basa en 1. Si la longitud se omite, se establece el final de la cadena como valor predeterminado.  
* ``substring('Cat in the hat', 5, 2) -> 'in'``  
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``  
* ``substring('Cat in the hat', 5) -> 'in the hat'``  
* ``substring('Cat in the hat', 100, 100) -> ''``  
___
### <code>tan</code>
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula un valor de tangente.  
* ``tan(0) -> 0.0``  
___
### <code>tanh</code>
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Calcula un valor de tangente hiperbólica.  
* ``tanh(0) -> 0.0``  
___
### <code>translate</code>
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></code><br/><br/>
Reemplaza un conjunto de caracteres por otro conjunto de caracteres de la cadena. Los caracteres tienen un reemplazo de 1 a 1.  
* ``translate('(bojjus)', '()', '[]') -> '[bojjus]'``  
* ``translate('(gunchus)', '()', '[') -> '[gunchus'``  
___
### <code>trim</code>
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Recorta una cadena de caracteres iniciales y finales. Si no se especifica el segundo parámetro, recorta el espacio en blanco. Si no, recorta cualquier carácter especificado en el segundo parámetro.  
* ``trim('  dumbo  ') -> 'dumbo'``  
* ``trim('!--!du!mbo!', '-!') -> 'du!mbo'``  
___
### <code>true</code>
<code><b>true() => boolean</b></code><br/><br/>
Siempre devuelve un valor true. Use la función `syntax(true())` si hay una columna denominada "true".  
* ``(10 + 20 == 30) -> true``  
* ``(10 + 20 == 30) -> true()``  
___
### <code>typeMatch</code>
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></code><br/><br/>
Coincide con el tipo de la columna. Solo se puede usar en expresiones de patrón. "number" coincide con los tipos short, entero, long, double, float o decimal; "integral" coincide con los tipos short, entero y long; "fractional" coincide con los tipos double, float y decimal; "datetime" coincide con los tipos de date o timestamp.  
* ``typeMatch(type, 'number')``  
* ``typeMatch('date', 'datetime')``  
___
### <code>unescape</code>
<code><b>unescape(<i>&lt;string_to_escape&gt;</i> : string, <i>&lt;format&gt;</i> : string) => string</b></code><br/><br/>
Elimina el escape de una cadena según un formato. Los valores literales para el formato aceptable son "json", "xml", "ecmascript", "html", "java".
* ```unescape('{\\\\\"value\\\\\": 10}', 'json')```
* ```'{\\\"value\\\": 10}'```
___
### <code>upper</code>
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Pone en mayúsculas una cadena.  
* ``upper('bojjus') -> 'BOJJUS'``  
___
### <code>uuid</code>
<code><b>uuid() => string</b></code><br/><br/>
Devuelve el UUID generado.  
* ``uuid()``  
___
### <code>weekOfYear</code>
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Obtiene la semana del año dada una fecha.  
* ``weekOfYear(toDate('2008-02-20')) -> 8``  
___
### <code>weeks</code>
<code><b>weeks(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Duración en milisegundos para el número de semanas.  
* ``weeks(2) -> 1209600000L``  
___
### <code>xor</code>
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Operador XOR lógico. Igual que el operador ^.  
* ``xor(true, false) -> true``  
* ``xor(true, true) -> false``  
* ``true ^ false -> true``  
___
### <code>year</code>
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Obtiene el valor de año de una fecha.  
* ``year(toDate('2012-8-8')) -> 2012``  

## <a name="aggregate-functions"></a>Funciones de agregado
Las siguientes funciones solo están disponibles para las transformaciones de agregado, dinamización, anulación de dinamización y ventana.

___
### <code>approxDistinctCount</code>
<code><b>approxDistinctCount(<i>&lt;value1&gt;</i> : any, [ <i>&lt;value2&gt;</i> : double ]) => long</b></code><br/><br/>
Obtiene el recuento agregado aproximado de valores distintos para una columna. El segundo parámetro opcional es controlar el error de estimación.
* ``approxDistinctCount(ProductID, .05) => long``  
___
### <code>avg</code>
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Obtiene el promedio de valores de una columna.  
* ``avg(sales)``  
___
### <code>avgIf</code>
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
En función de los criterios, obtiene el promedio de valores de una columna.  
* ``avgIf(region == 'West', sales)``  
___
### <code>collect</code>
<code><b>collect(<i>&lt;value1&gt;</i> : any) => array</b></code><br/><br/>
Recopila todos los valores de la expresión del grupo agregado en una matriz. Las estructuras se pueden recopilar y transformar en estructuras alternativas durante este proceso. El número de elementos será igual al número de filas de ese grupo y puede contener valores NULL. El número de elementos recopilados debe ser pequeño.  
* ``collect(salesPerson)``
* ``collect(firstName + lastName))``
* ``collect(@(name = salesPerson, sales = salesAmount) )``
___
### <code>count</code>
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></code><br/><br/>
Obtiene el recuento agregado de valores. Si se especifican las columnas opcionales, omite los valores NULL en el recuento.  
* ``count(custId)``  
* ``count(custId, custName)``  
* ``count()``  
* ``count(iif(isNull(custId), 1, NULL))``  
___
### <code>countDistinct</code>
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></code><br/><br/>
Obtiene el recuento agregado de valores distintos de un conjunto de columnas.  
* ``countDistinct(custId, custName)``  
___
### <code>countIf</code>
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></code><br/><br/>
En función de los criterios, obtiene el recuento agregado de valores. Si se especifica la columna opcional, omite los valores NULL en el recuento.  
* ``countIf(state == 'CA' && commission < 10000, name)``  
___
### <code>covariancePopulation</code>
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la covarianza de la población entre dos columnas.  
* ``covariancePopulation(sales, profit)``  
___
### <code>covariancePopulationIf</code>
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la covarianza de la población de dos columnas.  
* ``covariancePopulationIf(region == 'West', sales)``  
___
### <code>covarianceSample</code>
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la covarianza de muestra de dos columnas.  
* ``covarianceSample(sales, profit)``  
___
### <code>covarianceSampleIf</code>
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la covarianza de muestra de dos columnas.  
* ``covarianceSampleIf(region == 'West', sales, profit)``  
___

### <code>first</code>
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Obtiene el primer valor de un grupo de columnas. Si el segundo parámetro ignoreNulls se omite, se supone que es falso.  
* ``first(sales)``  
* ``first(sales, false)``  
___

### <code>isDistinct</code>
<code><b>isDistinct(<i>&lt;value1&gt;</i> : any , <i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Busca si una columna o un conjunto de columnas es distinto. No cuenta NULL como valor distinto *    ``isDistinct(custId, custName) => boolean``
___

### <code>kurtosis</code>
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la curtosis de una columna.  
* ``kurtosis(sales)``  
___
### <code>kurtosisIf</code>
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la curtosis de una columna.  
* ``kurtosisIf(region == 'West', sales)``  
___
### <code>last</code>
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Obtiene el último valor de un grupo de columnas. Si el segundo parámetro ignoreNulls se omite, se supone que es falso.  
* ``last(sales)``  
* ``last(sales, false)``  
___
### <code>max</code>
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Obtiene el valor máximo de una columna.  
* ``max(sales)``  
___
### <code>maxIf</code>
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
En función de los criterios, obtiene el valor máximo de una columna.  
* ``maxIf(region == 'West', sales)``  
___
### <code>mean</code>
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Obtiene la media de valores de una columna. Igual que AVG.  
* ``mean(sales)``  
___
### <code>meanIf</code>
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
En función de los criterios, obtiene la media de valores de una columna. Igual que avgIf.  
* ``meanIf(region == 'West', sales)``  
___
### <code>min</code>
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Obtiene el valor mínimo de una columna.  
* ``min(sales)``  
___
### <code>minIf</code>
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
En función de los criterios, obtiene el valor mínimo de una columna.  
* ``minIf(region == 'West', sales)``  
___
### <code>skewness</code>
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la asimetría de una columna.  
* ``skewness(sales)``  
___
### <code>skewnessIf</code>
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la asimetría de una columna.  
* ``skewnessIf(region == 'West', sales)``  
___
### <code>stddev</code>
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la desviación estándar de una columna.  
* ``stdDev(sales)``  
___
### <code>stddevIf</code>
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la desviación estándar de una columna.  
* ``stddevIf(region == 'West', sales)``  
___
### <code>stddevPopulation</code>
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la desviación estándar de población de una columna.  
* ``stddevPopulation(sales)``  
___
### <code>stddevPopulationIf</code>
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la desviación estándar de población de una columna.  
* ``stddevPopulationIf(region == 'West', sales)``  
___
### <code>stddevSample</code>
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la desviación estándar de muestra de una columna.  
* ``stddevSample(sales)``  
___
### <code>stddevSampleIf</code>
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la desviación estándar de muestra de una columna.  
* ``stddevSampleIf(region == 'West', sales)``  
___
### <code>sum</code>
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Obtiene la suma de agregados de una columna numérica.  
* ``sum(col)``  
___
### <code>sumDistinct</code>
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Obtiene la suma de agregados de valores distintos de una columna numérica.  
* ``sumDistinct(col)``  
___
### <code>sumDistinctIf</code>
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
En función de los criterios, obtiene la suma de agregados de una columna numérica. La condición se puede basar en cualquier columna.  
* ``sumDistinctIf(state == 'CA' && commission < 10000, sales)``  
* ``sumDistinctIf(true, sales)``  
___
### <code>sumIf</code>
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
En función de los criterios, obtiene la suma de agregados de una columna numérica. La condición se puede basar en cualquier columna.  
* ``sumIf(state == 'CA' && commission < 10000, sales)``  
* ``sumIf(true, sales)``  
___
### <code>variance</code>
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la varianza de una columna.  
* ``variance(sales)``  
___
### <code>varianceIf</code>
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la varianza de una columna.  
* ``varianceIf(region == 'West', sales)``  
___
### <code>variancePopulation</code>
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la varianza de población de una columna.  
* ``variancePopulation(sales)``  
___
### <code>variancePopulationIf</code>
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la varianza de población de una columna.  
* ``variancePopulationIf(region == 'West', sales)``  
___
### <code>varianceSample</code>
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Obtiene la varianza no sesgada de una columna.  
* ``varianceSample(sales)``  
___
### <code>varianceSampleIf</code>
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
En función de los criterios, obtiene la varianza no sesgada de una columna.  
* ``varianceSampleIf(region == 'West', sales)``  

## <a name="array-functions"></a>Funciones de matriz
Las funciones de matriz realizan transformaciones sobre estructuras de datos que son matrices. Estas incluyen palabras clave especiales para direccionar elementos de matriz e índices:

* ```#acc``` representa un valor que se desea incluir en la salida única al reducir una matriz.
* ```#index``` representa el índice de la matriz actual, junto con los números de índice de la matriz ```#index2, #index3 ...```.
* ```#item``` representa el valor del elemento actual de la matriz.

### <code>array</code>
<code><b>array([<i>&lt;value1&gt;</i> : any], ...) => array</b></code><br/><br/>
Crea una matriz de elementos. Todos los elementos deben ser del mismo tipo. Si no se especifica ningún elemento, una matriz de cadenas vacías es el valor predeterminado. Igual que un operador de creación [].  
* ``array('Seattle', 'Washington')``
* ``['Seattle', 'Washington']``
* ``['Seattle', 'Washington'][1]``
* ``'Washington'``
___
### <code>at</code>
<code><b>at(<i>&lt;value1&gt;</i> : array/map, <i>&lt;value2&gt;</i> : integer/key type) => array</b></code><br/><br/>
Busca el elemento en un índice de matrices. El índice es de base 1. El índice fuera de los límites da como resultado un valor NULL. Busca un valor en una asignación dada una clave. Si no se encuentra la clave, devuelve NULL.
*   ``at(['apples', 'pears'], 1) => 'apples'``
*   ``at(['fruit' -> 'apples', 'vegetable' -> 'carrot'], 'fruit') => 'apples'``
___
### <code>contains</code>
<code><b>contains(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => boolean</b></code><br/><br/>
Devuelve true si algún elemento de la matriz proporcionada se evalúa como true en el predicado proporcionado. "Contains" espera una referencia a un elemento de la función de predicado como #item.  
* ``contains([1, 2, 3, 4], #item == 3) -> true``  
* ``contains([1, 2, 3, 4], #item > 5) -> false``  
___
### <code>distinct</code>
<code><b>distinct(<i>&lt;value1&gt;</i> : array) => array</b></code><br/><br/>
Devuelve un conjunto distinto de elementos de una matriz.
* ``distinct([10, 20, 30, 10]) => [10, 20, 30]``
___
### <code>except</code>
<code><b>except(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : array) => array</b></code><br/><br/>
Devuelve un conjunto de diferencias de una matriz desde otros duplicados eliminados.
* ``except([10, 20, 30], [20, 40]) => [10, 30]``  
___
### <code>filter</code>
<code><b>filter(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => array</b></code><br/><br/>
Filtra los elementos de la matriz que no cumplen el predicado proporcionado. "Filter" espera una referencia a un elemento de la función de predicado como #item.  
* ``filter([1, 2, 3, 4], #item > 2) -> [3, 4]``  
* ``filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') -> ['a', 'b']``  
___
### <code>find</code>
<code><b>find(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Find the first item from an array that match the condition. It takes a filter function where you can address the item in the array as #item. For deeply nested maps you can refer to the parent maps using the #item_n(#item_1, #item_2...) notation.  
* ``find([10, 20, 30], #item > 10) -> 20``
* ``find(['azure', 'data', 'factory'], length(#item) > 4) -> 'azure'``
* ``find([
      @(
         name = 'Daniel',
         types = [
                   @(mood = 'jovial', behavior = 'terrific'),
                   @(mood = 'grumpy', behavior = 'bad')
                 ]
        ),
      @(
         name = 'Mark',
         types = [
                   @(mood = 'happy', behavior = 'awesome'),
                   @(mood = 'calm', behavior = 'reclusive')
                 ]
        )
      ],
      contains(#item.types, #item.mood=='happy')  /*Filter out the happy kid*/
    )``
* ``
     @(
           name = 'Mark',
           types = [
                     @(mood = 'happy', behavior = 'awesome'),
                     @(mood = 'calm', behavior = 'reclusive')
                   ]
      )
    ``  
___
### <code>flatten</code>
<code><b>flatten(<i>&lt;array&gt;</i> : array, <i>&lt;value2&gt;</i> : array ..., <i>&lt;value2&gt;</i> : boolean) => array</b></code><br/><br/> Acopla una matriz o matrices en una sola. Las matrices de elementos atómicos se devuelven sin modificar. El último argumento es opcional y su valor predeterminado es false para acoplar de manera recursiva más de one level deep.
*   ``flatten([['bojjus', 'girl'], ['gunchus', 'boy']]) => ['bojjus', 'girl', 'gunchus', 'boy']``
*   ``flatten([[['bojjus', 'gunchus']]] , true) => ['bojjus', 'gunchus']``
___
### <code>in</code>
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/> Checks if an item is in the array.  
* ``in([10, 20, 30], 10) -> true``  
* ``in(['good', 'kid'], 'bad') -> false``  
___
### <code>intersect</code>
<code><b>intersect(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : array) => array</b></code><br/><br/> Devuelve un conjunto de intersección de elementos distintoss from 2 arrays.
* ``intersect([10, 20, 30], [20, 40]) => [20]``  
___
### <code>map</code>
<code><b>map(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => any</b></code><br/><br/> Maps each element of the array to a new element using the provided expression. Map expects a reference to one element in the expression function as #item.  
* ``map([1, 2, 3, 4], #item + 2) -> [3, 4, 5, 6]``  
* ``map(['a', 'b', 'c', 'd'], #item + '_processed') -> ['a_processed', 'b_processed', 'c_processed', 'd_processed']``  
___
### <code>mapIf</code>
<code><b>mapIf (<i>\<value1\></i> : array, <i>\<value2\></i> : binaryfunction, \<value3\>: binaryFunction) => any</b></code><br/><br/> Conditionally maps an array to another array of same or smaller length. The values can be of any datatype including structTypes. It takes a mapping function where you can address the item in the array as #item and current index as #index. For deeply nested maps you can refer to the parent maps using the ``#item_[n](#item_1, #index_1...)`` notation.
*    ``mapIf([10, 20, 30], #item > 10, #item + 5) -> [25, 35]``
* ``mapIf(['icecream', 'cake', 'soda'], length(#item) > 4, upper(#item)) -> ['ICECREAM', 'CAKE']``
___
### <code>mapIndex</code>
<code><b>mapIndex(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => any</b></code><br/><br/> Maps each element of the array to a new element using the provided expression. Map expects a reference to one element in the expression function as #item and a reference to the element index as #index.  
* ``mapIndex([1, 2, 3, 4], #item + 2 + #index) -> [4, 6, 8, 10]``  
___
### <code>mapLoop</code>
<code><b>mapLoop(<i>\<value1\></i> : integer, <i>\<value2\></i> : unaryfunction) => any</b></code><br/><br/> Loops through from 1 to length to create an array of that length. It takes a mapping function where you can address the index in the array as #index. For deeply nested maps you can refer to the parent maps using the #index_n(#index_1, #index_2...) notation.
*    ``mapLoop(3, #index * 10) -> [10, 20, 30]``
___
### <code>reduce</code>
<code><b>reduce(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : any, <i>&lt;value3&gt;</i> : binaryfunction, <i>&lt;value4&gt;</i> : unaryfunction) => any</b></code><br/><br/> Accumulates elements in an array. Reduce expects a reference to an accumulator and one element in the first expression function as #acc and #item and it expects the resulting value as #result to be used in the second expression function.  
* ``toString(reduce(['1', '2', '3', '4'], '0', #acc + #item, #result)) -> '01234'``  
___
### <code>size</code>
<code><b>size(<i>&lt;value1&gt;</i> : any) => integer</b></code><br/><br/> Finds the size of an array or map type  
* ``size(['element1', 'element2']) -> 2``
* ``size([1,2,3]) -> 3``
___
### <code>slice</code>
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/> Extracts a subset of an array from a position. La posición se basa en 1. If the length is omitted, it is defaulted to end of the string.  
* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``  
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``  
* ``slice([10, 20, 30, 40], 2)[1] -> 20``  
* ``isNull(slice([10, 20, 30, 40], 2)[0]) -> true``  
* ``isNull(slice([10, 20, 30, 40], 2)[20]) -> true``  
* ``slice(['a', 'b', 'c', 'd'], 8) -> []``  
___
### <code>sort</code>
<code><b>sort(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => array</b></code><br/><br/> Sorts the array using the provided predicate function. Sort expects a reference to two consecutive elements in the expression function as #item1 and #item2.  
* ``sort([4, 8, 2, 3], compare(#item1, #item2)) -> [2, 3, 4, 8]``  
* ``sort(['a3', 'b2', 'c1'], iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) -> ['c1', 'b2', 'a3']``  
___
### <code>unfold</code>
<code><b>unfold (<i>&lt;value1&gt;</i>: array) => any</b></code><br/><br/> Despliega una matriz en un conjunto de filas y repite los valores de la columna restantens in every row.
*   ``unfold(addresses) => any``
*   ``unfold( @(name = salesPerson, sales = salesAmount) ) => any``
___  
### <code>union</code>
<code><b>union(<i>&lt;value1&gt;</i>: array, <i>&lt;value2&gt;</i> : array) => array</b></code><br/><br/> Devuelve un conjunto de unión de elementos distintoss from 2 arrays.
* ``union([10, 20, 30], [20, 40]) => [10, 20, 30, 40]``
___* ``
 @(
       name = 'Mark',
       types = [
                 @(mood = 'happy', behavior = 'awesome'),
                 @(mood = 'calm', behavior = 'reclusive')
               ]
  )
``  
___
### <code>flatten</code>
<code><b>flatten(<i>&lt;array&gt;</i> : array, <i>&lt;value2&gt;</i> : array ..., <i>&lt;value2&gt;</i> : boolean) => array</b></code><br/><br/>
Flattens array or arrays into a single array. Arrays of atomic items are returned unaltered. The last argument is optional and is defaulted to false to flatten recursively more than one level deep.
*   ``flatten([['bojjus', 'girl'], ['gunchus', 'boy']]) => ['bojjus', 'girl', 'gunchus', 'boy']``
*   ``flatten([[['bojjus', 'gunchus']]] , true) => ['bojjus', 'gunchus']``
___
### <code>in</code>
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/>
Checks if an item is in the array.  
* ``in([10, 20, 30], 10) -> true``  
* ``in(['good', 'kid'], 'bad') -> false``  
___
### <code>intersect</code>
<code><b>intersect(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : array) => array</b></code><br/><br/>
Returns an intersection set of distinct items from 2 arrays.
* ``intersect([10, 20, 30], [20, 40]) => [20]``  
___
### <code>map</code>
<code><b>map(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Maps each element of the array to a new element using the provided expression. Map expects a reference to one element in the expression function as #item.  
* ``map([1, 2, 3, 4], #item + 2) -> [3, 4, 5, 6]``  
* ``map(['a', 'b', 'c', 'd'], #item + '_processed') -> ['a_processed', 'b_processed', 'c_processed', 'd_processed']``  
___
### <code>mapIf</code>
<code><b>mapIf (<i>\<value1\></i> : array, <i>\<value2\></i> : binaryfunction, \<value3\>: binaryFunction) => any</b></code><br/><br/>
Conditionally maps an array to another array of same or smaller length. The values can be of any datatype including structTypes. It takes a mapping function where you can address the item in the array as #item and current index as #index. For deeply nested maps you can refer to the parent maps using the ``#item_[n](#item_1, #index_1...)`` notation.
*    ``mapIf([10, 20, 30], #item > 10, #item + 5) -> [25, 35]``
* ``mapIf(['icecream', 'cake', 'soda'], length(#item) > 4, upper(#item)) -> ['ICECREAM', 'CAKE']``
___
### <code>mapIndex</code>
<code><b>mapIndex(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => any</b></code><br/><br/>
Maps each element of the array to a new element using the provided expression. Map expects a reference to one element in the expression function as #item and a reference to the element index as #index.  
* ``mapIndex([1, 2, 3, 4], #item + 2 + #index) -> [4, 6, 8, 10]``  
___
### <code>mapLoop</code>
<code><b>mapLoop(<i>\<value1\></i> : integer, <i>\<value2\></i> : unaryfunction) => any</b></code><br/><br/>
Loops through from 1 to length to create an array of that length. It takes a mapping function where you can address the index in the array as #index. For deeply nested maps you can refer to the parent maps using the #index_n(#index_1, #index_2...) notation.
*    ``mapLoop(3, #index * 10) -> [10, 20, 30]``
___
### <code>reduce</code>
<code><b>reduce(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : any, <i>&lt;value3&gt;</i> : binaryfunction, <i>&lt;value4&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Accumulates elements in an array. Reduce expects a reference to an accumulator and one element in the first expression function as #acc and #item and it expects the resulting value as #result to be used in the second expression function.  
* ``toString(reduce(['1', '2', '3', '4'], '0', #acc + #item, #result)) -> '01234'``  
___
### <code>size</code>
<code><b>size(<i>&lt;value1&gt;</i> : any) => integer</b></code><br/><br/>
Finds the size of an array or map type  
* ``size(['element1', 'element2']) -> 2``
* ``size([1,2,3]) -> 3``
___
### <code>slice</code>
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/>
Extracts a subset of an array from a position. Position is 1 based. If the length is omitted, it is defaulted to end of the string.  
* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``  
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``  
* ``slice([10, 20, 30, 40], 2)[1] -> 20``  
* ``isNull(slice([10, 20, 30, 40], 2)[0]) -> true``  
* ``isNull(slice([10, 20, 30, 40], 2)[20]) -> true``  
* ``slice(['a', 'b', 'c', 'd'], 8) -> []``  
___
### <code>sort</code>
<code><b>sort(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => array</b></code><br/><br/>
Sorts the array using the provided predicate function. Sort expects a reference to two consecutive elements in the expression function as #item1 and #item2.  
* ``sort([4, 8, 2, 3], compare(#item1, #item2)) -> [2, 3, 4, 8]``  
* ``sort(['a3', 'b2', 'c1'], iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) -> ['c1', 'b2', 'a3']``  
___
### <code>unfold</code>
<code><b>unfold (<i>&lt;value1&gt;</i>: array) => any</b></code><br/><br/>
Unfolds an array into a set of rows and repeats the values for the remaining columns in every row.
*   ``unfold(addresses) => any``
*   ``unfold( @(name = salesPerson, sales = salesAmount) ) => any``
___  
### <code>union</code>
<code><b>union(<i>&lt;value1&gt;</i>: array, <i>&lt;value2&gt;</i> : array) => array</b></code><br/><br/>
Returns a union set of distinct items from 2 arrays.
* ``union([10, 20, 30], [20, 40]) => [10, 20, 30, 40]``
___  
  
## <a name="cached-lookup-functions"></a>Funciones de búsqueda almacenadas en caché
Las siguientes funciones solo están disponibles cuando se usa una búsqueda almacenada en caché y ha incluido un receptor almacenado en caché.
___
### <code>lookup</code>
<code><b>lookup(key, key2, ...) => complex[]</b></code><br/><br/>
Busca la primera fila del receptor almacenado en caché con las claves especificadas, que coinciden con las claves del receptor almacenado en caché.
* ``cacheSink#lookup(movieId)``  
___
### <code>mlookup</code>
<code><b>mlookup(key, key2, ...) => complex[]</b></code><br/><br/>
Busca todas las filas coincidentes del receptor almacenado en caché con las claves especificadas, que coinciden con las claves del receptor almacenado en caché.
* ``cacheSink#mlookup(movieId)``  
___
### <code>output</code>
<code><b>output() => any</b></code><br/><br/>
Devuelve la primera fila de los resultados del receptor almacenado en caché * ``cacheSink#output()``.  
___
### <code>outputs</code>
<code><b>output() => any</b></code><br/><br/>
Devuelve el conjunto completo de filas de salida de los resultados del receptor almacenado en caché * ``cacheSink#outputs()``
___.

## <a name="conversion-functions"></a>Funciones de conversión

Las funciones de conversión se usan para convertir datos y probar tipos de datos.

### <code>isBitSet</code>
<code><b>isBitSet (<i><i>\<value1\></i></i> : array, <i>\<value2\></i>:integer ) => boolean</b></code><br/><br/>
Comprueba si se establece una posición de bit en este conjunto de bits * ``isBitSet(toBitSet([10, 32, 98]), 10) => true``
___
### <code>setBitSet</code>
<code><b>setBitSet (<i>\<value1\></i>: array, <i>\<value2\></i>:array) => array</b></code>.<br/><br/>
Establece las posiciones de bit en este conjunto de bits * ``setBitSet(toBitSet([10, 32]), [98]) => [4294968320L, 17179869184L]``
___  
### <code>isBoolean</code>
<code><b>isBoolean(<i>\<value1\></i>: string) => boolean</b></code>.<br/><br/>
Permite comprobar si el valor de la cadena es booleano según las reglas de ``toBoolean()``
* ``isBoolean('true') -> true``
* ``isBoolean('no') -> true``
* ``isBoolean('microsoft') -> false``
___
### <code>isByte</code>
<code><b>isByte(<i>\<value1\></i> : string) => boolean</b></code>.<br/><br/>
Permite comprobar si el valor de la cadena es un byte con un formato opcional según las reglas de ``toByte()``
* ``isByte('123') -> true``
* ``isByte('chocolate') -> false``
___
### <code>isDate</code>
<code><b>isDate (<i>\<value1\></i> : string, [&lt;format&gt;: string]) => boolean</b></code>.<br/><br/>
Permite comprobar si la cadena de fecha de entrada es una fecha con un formato de fecha de entrada opcional. Consulte SimpleDateFormat de Java para conocer los formatos disponibles. Si se omite el formato de fecha de entrada, el formato predeterminado es ``yyyy-[M]M-[d]d``. Los formatos aceptados son ``[ yyyy, yyyy-[M]M, yyyy-[M]M-[d]d, yyyy-[M]M-[d]dT* ]``
* ``isDate('2012-8-18') -> true``
* ``isDate('12/18--234234' -> 'MM/dd/yyyy') -> false``
___
### <code>isShort</code>
<code><b>isShort (<i>\<value1\></i> : string, [&lt;format&gt;: string]) => boolean</b></code>.<br/><br/>
Permite comprobar si el valor de la cadena es un elemento corto con un formato opcional según las reglas de ``toShort()``
* ``isShort('123') -> true``
* ``isShort('$123' -> '$###') -> true``
* ``isShort('microsoft') -> false``
___
### <code>isInteger</code>
<code><b>isInteger (<i>\<value1\></i> : string, [&lt;format&gt;: string]) => boolean</b></code>.<br/><br/>
Permite comprobar si el valor de la cadena es un elemento entero con un formato opcional según las reglas de ``toInteger()``
* ``isInteger('123') -> true``
* ``isInteger('$123' -> '$###') -> true``
* ``isInteger('microsoft') -> false``
___
### <code>isLong</code>
<code><b>isLong (<i>\<value1\></i> : string, [&lt;format&gt;: string]) => boolean</b></code>.<br/><br/>
Permite comprobar si el valor de la cadena es un elemento largo con un formato opcional según las reglas de ``toLong()``
* ``isLong('123') -> true``
* ``isLong('$123' -> '$###') -> true``
* ``isLong('gunchus') -> false``
___
### <code>isNan</code>
<code><b>isNan (<i>\<value1\></i> : integral) => boolean</b></code>.<br/><br/>
Comprueba si no es un número.
* ``isNan(10.2) => false``
___
### <code>isFloat</code>
<code><b>isFloat (<i>\<value1\></i> : string, [&lt;format&gt;: string]) => boolean</b></code><br/><br/>
Permite comprobar si el valor de la cadena es un elemento flotante con un formato opcional según las reglas de ``toFloat()``
* ``isFloat('123') -> true``
* ``isFloat('$123.45' -> '$###.00') -> true``
* ``isFloat('icecream') -> false``
___
### <code>isDouble</code>
<code><b>isDouble (<i>\<value1\></i> : string, [&lt;format&gt;: string]) => boolean</b></code>.<br/><br/>
Permite comprobar si el valor de la cadena es un elemento doble con un formato opcional según las reglas de ``toDouble()``
* ``isDouble('123') -> true``
* ``isDouble('$123.45' -> '$###.00') -> true``
* ``isDouble('icecream') -> false``
___
### <code>isDecimal</code>
<code><b>isDecimal (<i>\<value1\></i> : string) => boolean</b></code>.<br/><br/>
Permite comprobar si el valor de la cadena es un elemento decimal con un formato opcional según las reglas de ``toDecimal()``
* ``isDecimal('123.45') -> true``
* ``isDecimal('12/12/2000') -> false``
___
### <code>isTimestamp</code>
<code><b>isTimestamp (<i>\<value1\></i> : string, [&lt;format&gt;: string]) => boolean</b></code>.<br/><br/>
Permite comprobar si la cadena de fecha de entrada es una marca de tiempo con un formato de marca de tiempo de entrada opcional. Consulte la clase SimpleDateFormat de Java para conocer los formatos disponibles. Si la marca de tiempo se omite, se usa el patrón predeterminado ``yyyy-[M]M-[d]d hh:mm:ss[.f...]``. Puede pasar una zona horaria opcional en forma de "GMT", "PST", "UTC", "America/Cayman". La marca de tiempo admite una precisión de hasta milisegundos con un valor de 999. Consulte la clase SimpleDateFormat de Java para conocer los formatos disponibles.
* ``isTimestamp('2016-12-31 00:12:00') -> true``
* ``isTimestamp('2016-12-31T00:12:00' -> 'yyyy-MM-dd\\'T\\'HH:mm:ss' -> 'PST') -> true``
* ``isTimestamp('2012-8222.18') -> false``
___
### <code>toBase64</code>
<code><b>toBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Codifica la cadena especificada en Base64.  
* ``toBase64('bojjus') -> 'Ym9qanVz'``  
___
### <code>toBinary</code>
<code><b>toBinary(<i>&lt;value1&gt;</i> : any) => binary</b></code><br/><br/>
Convierte cualquier valor numérico/fecha/marca de tiempo/cadena en una representación binaria.  
* ``toBinary(3) -> [0x11]``  
___
### <code>toBoolean</code>
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></code><br/><br/>
Convierte un valor de (" t", "true", "y", "yes", "1") en true y ("f", "false", "n", "no", "0") en false y NULL para cualquier otro valor.  
* ``toBoolean('true') -> true``  
* ``toBoolean('n') -> false``  
* ``isNull(toBoolean('truthy')) -> true``  
___
### <code>toByte</code>
<code><b>toByte(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => byte</b></code><br/><br/>
Convierte cualquier valor numérico o cadena en un valor de tipo byte. Se puede utilizar un formato decimal de Java opcional para la conversión.  
* ``toByte(123)``
* ``123``
* ``toByte(0xFF)``
* ``-1``
* ``toByte('123')``
* ``123``
___
### <code>toDate</code>
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></code><br/><br/>
Convierte la cadena de fecha de entrada en fecha con un formato de fecha de entrada opcional. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. Si se omite el formato de fecha de entrada, el formato predeterminado es aaaa-[M]M-[d]d. Los formatos aceptados son :[ aaaa, aaaa-[M]M, aaaa-[M]M-[d]d, aaaa-[M]M-[d]dH* ].  
* ``toDate('2012-8-18') -> toDate('2012-08-18')``  
* ``toDate('12/18/2012', 'MM/dd/yyyy') -> toDate('2012-12-18')``  
___
### <code>toDecimal</code>
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;precision&gt;</i> : integral], [<i>&lt;scale&gt;</i> : integral], [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => decimal(10,0)</b></code><br/><br/>
Convierte cualquier valor numérico o cadena en un valor decimal. Si no se especifican la precisión y la escala, se utiliza el valor predeterminado (10,2). Se puede utilizar un formato decimal de Java opcional para la conversión. Formato de configuración regional opcional en el formulario del lenguaje BCP47, como en-US, de, zh-CN.  
* ``toDecimal(123.45) -> 123.45``  
* ``toDecimal('123.45', 8, 4) -> 123.4500``  
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``  
* ``toDecimal('Ç123,45', 10, 2, 'Ç###,##', 'de') -> 123.45``  
___
### <code>toDouble</code>
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => double</b></code><br/><br/>
Convierte cualquier valor numérico o cadena en un valor doble. Se puede utilizar un formato decimal de Java opcional para la conversión. Formato de configuración regional opcional en el formulario del lenguaje BCP47, como en-US, de, zh-CN.  
* ``toDouble(123.45) -> 123.45``  
* ``toDouble('123.45') -> 123.45``  
* ``toDouble('$123.45', '$###.00') -> 123.45``  
* ``toDouble('Ç123,45', 'Ç###,##', 'de') -> 123.45``  
___
### <code>toFloat</code>
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => float</b></code><br/><br/>
Convierte cualquier valor numérico o cadena en un valor flotante. Se puede utilizar un formato decimal de Java opcional para la conversión. Trunca cualquier valor doble.  
* ``toFloat(123.45) -> 123.45f``  
* ``toFloat('123.45') -> 123.45f``  
* ``toFloat('$123.45', '$###.00') -> 123.45f``  
___
### <code>toInteger</code>
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => integer</b></code><br/><br/>
Convierte cualquier valor numérico o cadena en un valor entero. Se puede utilizar un formato decimal de Java opcional para la conversión. Trunca cualquier valor long, float o double.  
* ``toInteger(123) -> 123``  
* ``toInteger('123') -> 123``  
* ``toInteger('$123', '$###') -> 123``  
___
### <code>toLong</code>
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => long</b></code><br/><br/>
Convierte cualquier valor numérico o cadena en un valor largo. Se puede utilizar un formato decimal de Java opcional para la conversión. Trunca cualquier valor float o double.  
* ``toLong(123) -> 123``  
* ``toLong('123') -> 123``  
* ``toLong('$123', '$###') -> 123``  
___
### <code>toShort</code>
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => short</b></code><br/><br/>
Convierte cualquier valor numérico o cadena en un valor corto. Se puede utilizar un formato decimal de Java opcional para la conversión. Trunca cualquier valor entero, long, float o double.  
* ``toShort(123) -> 123``  
* ``toShort('123') -> 123``  
* ``toShort('$123', '$###') -> 123``  
___
### <code>toString</code>
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></code><br/><br/>
Convierte un tipo de datos primitivo en una cadena. Para los números y la fecha se puede especificar un formato. Si no se especifica, se elige el valor predeterminado del sistema. Para los números, se usa el formato decimal de Java. Consulte el formato SimpleDateFormat de Java para ver todos los formatos de fecha posibles; el formato predeterminado es aaaa-MM-dd.  
* ``toString(10) -> '10'``  
* ``toString('engineer') -> 'engineer'``  
* ``toString(123456.789, '##,###.##') -> '123,456.79'``  
* ``toString(123.78, '000000.000') -> '000123.780'``  
* ``toString(12345, '##0.#####E0') -> '12.345E3'``  
* ``toString(toDate('2018-12-31')) -> '2018-12-31'``  
* ``isNull(toString(toDate('2018-12-31', 'MM/dd/yy'))) -> true``  
* ``toString(4 == 20) -> 'false'``  
___
### <code>toTimestamp</code>
<code><b>toTimestamp(<i>&lt;string&gt;</i> : any, [<i>&lt;timestamp format&gt;</i> : string], [<i>&lt;time zone&gt;</i> : string]) => timestamp</b></code><br/><br/>
Convierte una cadena en una marca de tiempo dado un formato de marca de tiempo opcional. Si la marca de tiempo se omite, se usa el patrón predeterminado aaaa-[M]M-[d]d hh:mm:ss[.f...]. Puede pasar una zona horaria opcional en forma de "GMT", "PST", "UTC", "America/Cayman". La marca de tiempo admite una precisión de hasta milisegundos, con el valor de 999. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``toTimestamp('2016-12-31 00:12:00') -> toTimestamp('2016-12-31 00:12:00')``  
* ``toTimestamp('2016-12-31T00:12:00', 'yyyy-MM-dd\'T\'HH:mm:ss', 'PST') -> toTimestamp('2016-12-31 00:12:00')``  
* ``toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss') -> toTimestamp('2016-12-31 00:12:00')``  
* ``millisecond(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``  
___
### <code>toUTC</code>
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Convierte la marca de tiempo en UTC. Puede pasar una zona horaria opcional en forma de "GMT", "PST", "UTC", "America/Cayman". La zona horaria actual se establece como predeterminada. Consulte la clase `SimpleDateFormat` de Java para obtener los formatos disponibles. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html.  
* ``toUTC(currentTimestamp()) == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``toUTC(currentTimestamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  

## <a name="map-functions"></a>Funciones de asignación
  
Las funciones de asignación realizan operaciones en tipos de datos de asignación

### <code>associate</code>
<code><b>reassociate(<i>&lt;value1&gt;</i> : map, <i>&lt;value2&gt;</i> : binaryFunction) => map</b></code><br/><br/>
Crea una asignación de clave/valores. Todas las claves y valores deben ser del mismo tipo. Si no se especifica ningún elemento, su valor predeterminado es una asignación de cadena a tipo de cadena. Igual que un operador de creación ```[ -> ]```. Las claves y los valores deben alternarse.
*   ``associate('fruit', 'apple', 'vegetable', 'carrot' )=> ['fruit' -> 'apple', 'vegetable' -> 'carrot']``
___
### <code>keyValues</code>
<code><b>keyValues(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : array) => map</b></code><br/><br/>
Crea una asignación de clave/valores. El primer parámetro es una matriz de claves y el segundo es la matriz de valores. Ambas matrices deben tener la misma longitud.
*   ``keyValues(['bojjus', 'appa'], ['gunchus', 'ammi']) => ['bojjus' -> 'gunchus', 'appa' -> 'ammi']``
___ 
### <code>mapAssociation</code>
<code><b>mapAssociation(<i>&lt;value1&gt;</i> : map, <i>&lt;value2&gt;</i> : binaryFunction) => array</b></code><br/><br/>
Transforma una asignación al asociar las claves a nuevos valores. Devuelve una matriz. Toma una función de asignación donde se puede hacer referencia al elemento como #key y al índice actual como #value. 
*   ``mapAssociation(['bojjus' -> 'gunchus', 'appa' -> 'ammi'], @(key = #key, value = #value)) => [@(key = 'bojjus', value = 'gunchus'), @(key = 'appa', value = 'ammi')]``
___ 
### <code>reassociate</code>
<code><b>reassociate(<i>&lt;value1&gt;</i> : map, <i>&lt;value2&gt;</i> : binaryFunction) => map</b></code><br/><br/>
Transforma una asignación al asociar las claves a nuevos valores. Toma una función de asignación donde se puede hacer referencia al elemento como #key y al índice actual como #value.  
* ``reassociate(['fruit' -> 'apple', 'vegetable' -> 'tomato'], substring(#key, 1, 1) + substring(#value, 1, 1)) => ['fruit' -> 'fa', 'vegetable' -> 'vt']``
___
  
## <a name="metafunctions"></a>Metafunciones

Las metafunciones aparecen principalmente en los metadatos del flujo de datos.

### <code>byItem</code>
<code><b>byItem(<i>&lt;parent column&gt;</i> : any, <i>&lt;column name&gt;</i> : string) => any</b></code><br/><br/>
Busque un subelemento en una estructura o matriz de estructuras; si hay varias coincidencias, se devuelve la primera coincidencia. Si no hay coincidencias, se devuelve un valor NULL. El tipo del valor devuelto debe haberse convertido mediante una de las acciones de conversión de tipo (? fecha, ? cadena ...).  Los nombres de columna que se conocen en tiempo de diseño deben tratarse simplemente por su nombre. No se admiten entradas calculadas, pero se pueden usar sustituciones de parámetros. * ``byItem( byName('customer'), 'orderItems') ? (itemName as string, itemQty as integer)``
* ````
* ``byItem( byItem( byName('customer'), 'orderItems'), 'itemName') ? string``
* ````
___
### <code>byOrigin</code>
<code><b>byOrigin(<i>&lt;column name&gt;</i> : string, [<i>&lt;origin stream name&gt;</i> : string]) => any</b></code><br/><br/>
Selecciona un valor de columna en función del nombre de la secuencia de origen. El segundo argumento es el nombre de la secuencia de origen. Si hay varias coincidencias, se devuelve la primera. Si no hay coincidencias, se devuelve un valor NULL. El tipo del valor devuelto debe haberse convertido mediante una de las funciones de conversión de tipo (TO_DATE, TO_STRING...). Los nombres de columna que se conocen en tiempo de diseño deben tratarse simplemente por su nombre. No se admiten entradas calculadas, pero se pueden usar sustituciones de parámetros.  
* ``toString(byOrigin('ancestor', 'ancestorStream'))``
___
### <code>byOrigins</code>
<code><b>byOrigins(<i>&lt;column names&gt;</i> : array, [<i>&lt;origin stream name&gt;</i> : string]) => any</b></code><br/><br/>
Selecciona una matriz de columnas en función del nombre en la secuencia. El segundo argumento es la secuencia desde la que se originó. Si hay varias coincidencias, se devuelve la primera. Si no hay coincidencias, se devuelve un valor NULL. El tipo del valor devuelto debe haberse convertido mediante una de las funciones de conversión de tipo (TO_DATE, TO_STRING...). Los nombres de columna que se conocen en tiempo de diseño deben tratarse simplemente por su nombre. No se admiten entradas calculadas, pero se pueden usar sustituciones de parámetros.
* ``toString(byOrigins(['ancestor1', 'ancestor2'], 'ancestorStream'))``
___
### <code>byName</code>
<code><b>byName(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => any</b></code><br/><br/>
Selecciona un valor de columna por el nombre en la secuencia. Puede pasar un nombre de flujo opcional como segundo argumento. Si hay varias coincidencias, se devuelve la primera. Si no hay coincidencias, se devuelve un valor NULL. El tipo del valor devuelto debe haberse convertido mediante una de las funciones de conversión de tipo (TO_DATE, TO_STRING...).  Los nombres de columna que se conocen en tiempo de diseño deben tratarse simplemente por su nombre. No se admiten entradas calculadas, pero se pueden usar sustituciones de parámetros.  
* ``toString(byName('parent'))``  
* ``toLong(byName('income'))``  
* ``toBoolean(byName('foster'))``  
* ``toLong(byName($debtCol))``  
* ``toString(byName('Bogus Column'))``  
* ``toString(byName('Bogus Column', 'DeriveStream'))``  
___
### <code>byNames</code>
<code><b>byNames(<i>&lt;column names&gt;</i> : array, [<i>&lt;stream name&gt;</i> : string]) => any</b></code><br/><br/>
Seleccione una matriz de columnas por nombre en la secuencia. Puede pasar un nombre de flujo opcional como segundo argumento. Si hay varias coincidencias, se devuelve la primera. Si no hay ninguna coincidencia para una columna, la salida completa es un valor NULL. El valor devuelto requiere una función de conversión de tipos (toDate, toString, etc.).  Los nombres de columna que se conocen en tiempo de diseño deben tratarse simplemente por su nombre. No se admiten entradas calculadas, pero se pueden usar sustituciones de parámetros.
* ``toString(byNames(['parent', 'child']))``
* ``byNames(['parent']) ? string``
* ``toLong(byNames(['income']))``
* ``byNames(['income']) ? long``
* ``toBoolean(byNames(['foster']))``
* ``toLong(byNames($debtCols))``
* ``toString(byNames(['a Column']))``
* ``toString(byNames(['a Column'], 'DeriveStream'))``
* ``byNames(['orderItem']) ? (itemName as string, itemQty as integer)``
___
### <code>byPath</code>
<code><b>byPath(<i>&lt;value1&gt;</i> : string, [<i>&lt;streamName&gt;</i> : string]) => any</b></code><br/><br/>
Busca una ruta de acceso jerárquica por nombre en la secuencia. Puede pasar un nombre de secuencia opcional como segundo argumento. Si no se encuentra ninguna ruta de acceso, devuelve el valor NULL. Los nombres de columna o las rutas de acceso que se conocen en tiempo de diseño deben usarse solo mediante su nombre o mediante la ruta de acceso de la notación de puntos. No se admiten entradas calculadas, pero se pueden usar sustituciones de parámetros.  
* ``byPath('grandpa.parent.child') => column`` 
___
### <code>byPosition</code>
<code><b>byPosition(<i>&lt;position&gt;</i> : integer) => any</b></code><br/><br/>
Selecciona un valor de columna por su posición relativa (de base 1) en la secuencia. Si la posición está fuera de los límites, devuelve un valor Null. El tipo del valor devuelto debe haberse convertido mediante una de las funciones de conversión de tipo (TO_DATE, TO_STRING...). No se admiten entradas calculadas, pero se pueden usar sustituciones de parámetros.  
* ``toString(byPosition(1))``  
* ``toDecimal(byPosition(2), 10, 2)``  
* ``toBoolean(byName(4))``  
* ``toString(byName($colName))``  
* ``toString(byPosition(1234))``  
___
### <code>hasPath</code>
<code><b>hasPath(<i>&lt;value1&gt;</i> : string, [<i>&lt;streamName&gt;</i> : string]) => boolean</b></code><br/><br/>
Comprueba si existe una determinada ruta de acceso jerárquica por nombre en la secuencia. Puede pasar un nombre de secuencia opcional como segundo argumento. Los nombres de columna o las rutas de acceso que se conocen en tiempo de diseño deben usarse solo mediante su nombre o mediante la ruta de acceso de la notación de puntos. No se admiten entradas calculadas, pero se pueden usar sustituciones de parámetros.  
* ``hasPath('grandpa.parent.child') => boolean``
___  
### <code>originColumns</code>
<code><b>originColumns(<i>&lt;streamName&gt;</i> : string) => any</b></code><br/><br/>
Obtiene todas las columnas de salida de un flujo de origen donde se crearon columnas. Debe incluirse en otra función.
* ``array(toString(originColumns('source1')))``
___  
### <code>hex</code>
<code><b>hex(<i>\<value1\></i>: binary) => string</b></code><br/><br/>
Devuelve una representación de cadena hexa de un valor binario * ``hex(toBinary([toByte(0x1f), toByte(0xad), toByte(0xbe)])) -> '1fadbe'``
___
### <code>unhex</code>
<code><b>unhex(<i>\<value1\></i>: string) => binary</b></code>.<br/><br/>
Deshace la representación hexa de un valor binario de su representación de cadena. Se puede usar junto con SHA2, MD5 para convertir de cadena a representación binaria *    ``unhex('1fadbe') -> toBinary([toByte(0x1f), toByte(0xad), toByte(0xbe)])``
*    ``unhex(md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4'))) -> toBinary([toByte(0x4c),toByte(0xe8),toByte(0xa8),toByte(0x80),toByte(0xbd),toByte(0x62),toByte(0x1a),toByte(0x1f),toByte(0xfa),toByte(0xd0),toByte(0xbc),toByte(0xa9),toByte(0x05),toByte(0xe1),toByte(0xbc),toByte(0x5a)])``.

## <a name="window-functions"></a>Funciones de ventana
Las siguientes funciones solo están disponibles en las transformaciones de ventana.
___
### <code>cumeDist</code>
<code><b>cumeDist() => integer</b></code><br/><br/>
La función CumeDist calcula la posición de un valor respecto a todos los valores de la partición. El resultado es el número de filas anteriores o iguales a la fila actual en la ordenación de la partición dividido por el número total de filas de la partición de la ventana. Cualquier valor de empate en la ordenación se evaluará en la misma posición.  
* ``cumeDist()``  
___
### <code>denseRank</code>
<code><b>denseRank() => integer</b></code><br/><br/>
Calcula el rango de un valor en un grupo de valores especificado en la cláusula order by de una ventana. El resultado es uno más el número de filas anteriores o iguales a la fila actual en la ordenación de la partición. Los valores no generarán intervalos en la secuencia. La clasificación densa funciona incluso cuando los datos no están ordenados y busca cambios en los valores.  
* ``denseRank()``  
___
### <code>lag</code>
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Obtiene el valor del primer parámetro evaluado n filas antes de la fila actual. El segundo parámetro es el número de filas que se debe mirar hacia atrás, y el valor predeterminado es 1. Si no hay tantas filas, se devuelve un valor NULL, a menos que se especifique un valor predeterminado.  
* ``lag(amount, 2)``  
* ``lag(amount, 2000, 100)``  
___
### <code>lead</code>
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Obtiene el valor del primer parámetro evaluado n filas después de la fila actual. El segundo parámetro es el número de filas que se debe mirar hacia adelante, y el valor predeterminado es 1. Si no hay tantas filas, se devuelve un valor NULL, a menos que se especifique un valor predeterminado.  
* ``lead(amount, 2)``  
* ``lead(amount, 2000, 100)``  
___
### <code>nTile</code>
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></code><br/><br/>
La función ```NTile``` divide las filas para cada partición de ventana en `n` cubos comprendidos entre los valores 1 y `n` como máximo. Los valores de cubo variarán en 1 como máximo. Si el número de filas de la partición no se divide uniformemente en el número de cubos, los valores del resto se distribuyen uno por cubo, empezando por el primer cubo. La función ```NTile``` es útil para el cálculo de ```tertiles```, cuartiles, deciles y otras estadísticas de resumen comunes. La función calcula dos variables durante la inicialización: El tamaño de un cubo normal tendrá una fila extra agregada. Ambas variables se basan en el tamaño de la partición actual. Durante el proceso de cálculo, la función realiza un seguimiento del número de fila actual, el número de cubo actual y el número de fila en el que el cubo cambiará (bucketThreshold). Cuando el número de la fila actual alcance el umbral del cubo, el valor del cubo se incrementa en uno y el umbral aumentará el tamaño del cubo (más uno adicional si el cubo actual se rellena).  
* ``nTile()``  
* ``nTile(numOfBuckets)``  
___
### <code>rank</code>
<code><b>rank() => integer</b></code><br/><br/>
Calcula el rango de un valor en un grupo de valores especificado en la cláusula order by de una ventana. El resultado es uno más el número de filas anteriores o iguales a la fila actual en la ordenación de la partición. Los valores generarán intervalos en la secuencia. La clasificación funciona incluso cuando no está ordenada y busca cambios en los valores.  
* ``rank()``  
___
### <code>rowNumber</code>
<code><b>rowNumber() => integer</b></code><br/><br/>
Asigna una numeración secuencial de filas para las filas en una ventana que comienzan con 1.  
* ``rowNumber()``  

## <a name="next-steps"></a>Pasos siguientes

[Aprenda a usar el Generador de expresiones](concepts-data-flow-expression-builder.md).
