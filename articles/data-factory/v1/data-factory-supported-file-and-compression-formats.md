---
title: Formatos de compresión de archivos en Azure Data Factory
description: Aprenda sobre los formatos de archivo compatibles con Azure Data Factory.
author: linda33wj
ms.service: data-factory
ms.subservice: v1
ms.topic: conceptual
ms.date: 10/22/2021
ms.author: jingwang
robots: noindex
ms.openlocfilehash: e17640e847b089610ba94b226dee50a31316ad90
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130249977"
---
# <a name="file-and-compression-formats-supported-by-azure-data-factory"></a>Formatos de compresión de archivos admitidos por Azure Data Factory
*Este tema se aplica a los conectores siguientes: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [sistema de archivos](data-factory-onprem-file-system-connector.md), [FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md) y [SFTP](data-factory-sftp-connector.md).*

> [!NOTE]
> Este artículo se aplica a la versión 1 de Azure Data Factory. Si usa la versión actual del servicio Data Factory, consulte los [formatos de archivo y códecs de compresión admitidos en Data Factory](../supported-file-formats-and-compression-codecs.md).

Azure Data Factory admite los siguientes tipos de formato de archivo:

* [Formato de texto](#text-format)
* [Formato JSON](#json-format)
* [Formato Avro](#avro-format)
* [Formato ORC](#orc-format)
* [Formato Parquet](#parquet-format)

## <a name="text-format"></a>Formato de texto
Si quiere leer un archivo de texto o escribir en él, establezca la propiedad `type` de la sección `format` del conjunto de datos en **TextFormat**. También puede especificar las siguientes propiedades **opcionales** en la sección `format`. Consulte la sección [Ejemplo de TextFormat](#textformat-example) sobre cómo realizar la configuración.

| Propiedad | Descripción | Valores permitidos | Obligatorio |
| --- | --- | --- | --- |
| columnDelimiter |El carácter utilizado para separar las columnas en un archivo. Puede considerar el uso de un carácter imprimible poco frecuente que es probable que no exista en sus datos. Por ejemplo, especifique "\u0001", que representa el inicio de encabezado (SOH). |Solo se permite un carácter. El valor **predeterminado** es **coma (",")** . <br/><br/>Para usar un carácter Unicode, consulte la [lista de caracteres Unicode](https://en.wikipedia.org/wiki/List_of_Unicode_characters) para obtener el código correspondiente. |No |
| rowDelimiter |carácter utilizado para separar filas en un archivo. |Solo se permite un carácter. El valor **predeterminado** es cualquiera de los siguientes en lectura: **["\r\n", "\r", "\n"]** y **"\r\n"** en escritura. |No |
| escapeChar |carácter especial utilizado para aplicar una secuencia de escape a un delimitador de columna en el contenido de un archivo de entrada. <br/><br/>No puede especificar ambos valores escapeChar y quoteChar para una tabla. |Solo se permite un carácter. Ningún valor predeterminado. <br/><br/>Ejemplo: si usa la coma (",") como delimitador de columna, pero quiere tener el carácter de coma en el texto (ejemplo: "Hello, world"), puede definir "$" como carácter de escape y usar la cadena "Hello$, world" en el origen. |No |
| quoteChar |carácter utilizado para citar el valor de una cadena. Los delimitadores de columna y fila de dentro de las comillas se tratan como parte del valor de la cadena. Esta propiedad es aplicable tanto al conjunto de datos de entrada como al de salida.<br/><br/>No puede especificar ambos valores escapeChar y quoteChar para una tabla. |Solo se permite un carácter. Ningún valor predeterminado. <br/><br/>Por ejemplo, si tiene la coma (',') como delimitador de columna, pero quiere tener el carácter de coma en el texto (por ejemplo: <Hello, world>), puede definir " (comillas dobles) como comillas y usar la cadena "Hello, world" en el origen. |No |
| nullValue |uno o varios caracteres empleados para representar un valor nulo. |Uno o más caracteres. Los valores **predeterminados** son **"\N" y "NULL"** en lectura y **"\N"** en escritura. |No |
| encodingName |permite especificar el nombre de la codificación. |Un nombre de codificación válido. Consulte la [propiedad Encoding.EncodingName](/dotnet/api/system.text.encoding). Por ejemplo: windows-1250 o shift_jis. El valor **predeterminado** es **UTF-8**. |No |
| firstRowAsHeader |Especifica si se tendrá en cuenta la primera fila como encabezado. Para un conjunto de datos de entrada, Data Factory lee la primera fila como encabezado. Para un conjunto de datos de salida, Data Factory escribe la primera fila como encabezado. <br/><br/>Consulte [Escenarios de uso `firstRowAsHeader` y `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) para ver ejemplos de escenarios. |True<br/><b>False (valor predeterminado)</b> |No |
| skipLineCount |Indica el número de filas que se omitirán al leer datos de archivos de entrada. Si se especifican ambos valores skipLineCount y firstRowAsHeader, las líneas se omiten primero y, luego, la información del encabezado se lee a partir del archivo de entrada. <br/><br/>Consulte [Escenarios de uso `firstRowAsHeader` y `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) para ver ejemplos de escenarios. |Entero |No |
| treatEmptyAsNull |permite especificar si hay que tratar una cadena nula o vacía como un valor nulo al leer datos de un archivo de entrada. |**True (predeterminado)**<br/>False |No |

### <a name="textformat-example"></a>Ejemplo de TextFormat
En la siguiente definición JSON de un conjunto de datos, se especifican algunas propiedades opcionales.

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

Para usar un `escapeChar` en lugar de `quoteChar`, reemplace la línea con `quoteChar` por el siguiente escapeChar:

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Escenarios de uso de firstRowAsHeader y skipLineCount
* Va a copiar de un origen que no es archivo a un archivo de texto y quiere agregar una línea de encabezado que contenga los metadatos de esquema (por ejemplo, esquema SQL). Especifique `firstRowAsHeader` como true en el conjunto de datos de salida de este escenario.
* Va a copiar de un archivo de texto que contiene una línea de encabezado a un receptor que no es archivo y quiere eliminar esa línea. Especifique `firstRowAsHeader` como true en el conjunto de datos de entrada.
* Va a copiar de un archivo de texto y quiere omitir unas cuantas líneas al comienzo que no contienen datos ni información de encabezado. Especifique `skipLineCount` para indicar el número de líneas que se omitirá. Si el resto del archivo contiene una línea de encabezado, también puede especificar `firstRowAsHeader`. Si se especifican `skipLineCount` y `firstRowAsHeader`, las líneas se omiten primero y luego la información del encabezado se lee del archivo de entrada.

## <a name="json-format"></a>Formato JSON
Para **importar un archivo JSON como está en Azure Cosmos DB o exportarlo**, vea la sección [Importación o exportación de documentos JSON](data-factory-azure-documentdb-connector.md#importexport-json-documents) en el artículo [Movimiento de datos hacia y desde DocumentDB mediante Azure Data Factory](data-factory-azure-documentdb-connector.md).

Si quiere analizar los archivos JSON o escribir los datos en formato JSON, establezca la propiedad `type``format` en **JsonFormat**. También puede especificar las siguientes propiedades **opcionales** en la sección `format`. Consulte la sección [Ejemplo de JsonFormat](#jsonformat-example) sobre cómo realizar la configuración.

| Propiedad | Descripción | Obligatorio |
| --- | --- | --- |
| filePattern |Indica el patrón de los datos almacenados en cada archivo JSON. Estos son los valores permitidos: **setOfObjects** y **arrayOfObjects**. El valor **predeterminado** es **setOfObjects**. Consulte la sección [patrones de archivo JSON](#json-file-patterns) para obtener más información acerca de estos patrones. |No |
| jsonNodeReference | Si desea iterar y extraer datos de los objetos dentro de un campo de matriz con el mismo patrón, especifique la ruta de acceso JSON de esa matriz. Esta propiedad se admite solo cuando se copian datos desde los archivos JSON. | No |
| jsonPathDefinition | Especifique la expresión de ruta de acceso JSON para cada asignación de columna con un nombre de columna personalizado (que empiece con minúscula). Esta propiedad se admite solo cuando se copian datos desde archivos JSON y puede extraer datos del objeto o matriz. <br/><br/> Para los campos en el objeto raíz, comience por root $; para los campos dentro de la matriz elegida mediante la propiedad `jsonNodeReference`, empiece desde el elemento de matriz. Consulte la sección [Ejemplo de JsonFormat](#jsonformat-example) sobre cómo realizar la configuración. | No |
| encodingName |permite especificar el nombre de la codificación. Para obtener la lista de nombres de codificación válidos, consulte la propiedad [Encoding.EncodingName](/dotnet/api/system.text.encoding). Por ejemplo: windows-1250 o shift_jis. El valor **predeterminado** es: **UTF-8**. |No |
| nestingSeparator |Carácter que se usa para separar los niveles de anidamiento. El valor predeterminado es '.' (punto). |No |

### <a name="json-file-patterns"></a>Patrones de archivo JSON

La actividad de copia puede analizar los siguientes patrones de archivos JSON:

- **Tipo I: setOfObjects**

    Cada archivo contiene un único objeto o bien varios objetos concatenados/delimitados por líneas. Si se elige esta opción en un conjunto de datos de salida, la actividad de copia genera un único archivo JSON con cada objeto por línea (delimitado por líneas).

    * **ejemplo de JSON de objeto único**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **ejemplo de JSON delimitado por líneas**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **ejemplo de JSON concatenado**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **Tipo II: arrayOfObjects**

    Cada archivo contiene una matriz de objetos.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

### <a name="jsonformat-example"></a>Ejemplo de JsonFormat

**Caso 1: Copia de datos desde archivos JSON**

Consulte los dos ejemplos siguientes al copiar datos desde archivos JSON. Puntos genéricos que deben tener en cuenta:

**Ejemplo 1: extracción de datos de objeto y matriz**

En esta ejemplo, se espera un objeto JSON de raíz que se asigna al registro individual en resultado tabulares. Si tiene un archivo JSON con el siguiente contenido:  

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagementProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```
y quiere copiarlo en una tabla de SQL de Azure con el formato siguiente extrayendo datos tanto de los objetos como de la matriz:

| id | deviceType | targetResourceType | resourceManagementProcessRunId | occurrenceTime |
| --- | --- | --- | --- | --- |
| ed0e4960-d9c5-11e6-85dc-d7996816aad3 | PC | Microsoft.Compute/virtualMachines | 827f8aaa-ab72-437c-ba48-d8917a7336a3 | 1/13/2017 11:24:37 AM |

El conjunto de datos de entrada con el tipo **JsonFormat** se define de la siguiente manera: (definición parcial en la que solo se ilustran las secciones relevantes). Más concretamente:

- La sección `structure` permite definir los nombres personalizados de columna y el tipo de datos correspondiente mientras los convierte a datos tabulares. Esta sección es **opcional** a menos que necesite realizar una asignación de columnas. Consulte la sección [Asignación de columnas de conjunto de datos de origen a columnas del conjunto de datos de destino](data-factory-map-columns.md) para más información.
- `jsonPathDefinition` especifica la ruta de acceso JSON para cada columna que indica de dónde se deben extraer los datos. Para copiar datos de matriz, puede usar **array[x].property** para extraer el valor de la propiedad especificada desde el objeto xº, o puede usar **array[*].property** para encontrar el valor de cualquier objeto que contenga dicha propiedad.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagementProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagementProcessRunId": "$.context.custom.dimensions[1].ResourceManagementProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}      
        }
    }
}
```

**Ejemplo 2: aplicación cruzada en varios objetos del mismo patrón de matriz**

En este ejemplo, se espera transformar un objeto JSON de raíz en varios registros en resultado tabular. Si tiene un archivo JSON con el siguiente contenido:  

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```
y desea copiarlo en una tabla de Azure SQL del formato siguiente, puede hacerlo mediante el acoplamiento de los datos dentro de la matriz y la combinación cruzada con la información de la raíz habitual:

| ordernumber | orderdate | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P2 | 13 | [{"sanmateo":"No 1"}] |
| 01 | 20170122 | P3 | 231 | [{"sanmateo":"No 1"}] |

El conjunto de datos de entrada con el tipo **JsonFormat** se define de la siguiente manera: (definición parcial en la que solo se ilustran las secciones relevantes). Más concretamente:

- La sección `structure` permite definir los nombres personalizados de columna y el tipo de datos correspondiente mientras los convierte a datos tabulares. Esta sección es **opcional** a menos que necesite realizar una asignación de columnas. Consulte la sección [Asignación de columnas de conjunto de datos de origen a columnas del conjunto de datos de destino](data-factory-map-columns.md) para más información.
- `jsonNodeReference` indica la iteración y extracción de datos de los objetos con el mismo patrón de las líneas de pedido de **matriz**.
- `jsonPathDefinition` especifica la ruta de acceso JSON para cada columna que indica de dónde se deben extraer los datos. En este ejemplo, "ordernumber", "orderdate" y "city" están bajo el objeto raíz con la ruta de acceso JSON que empieza por "$.", mientras que "order_pd" y "order_price" se definen con la ruta de acceso que se deriva del elemento de matriz sin "$.".

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}         
        }
    }
}
```

**Tenga en cuenta los siguientes puntos:**

* Si no se definen la `structure` y `jsonPathDefinition` en el conjunto de datos de Data Factory, la actividad de copia detecta el esquema del primer objeto y acopla el objeto en su conjunto.
* Si la entrada JSON tiene una matriz, la actividad de copia convierte de forma predeterminada el valor de toda la matriz en una cadena. Puede elegir extraer los datos de esta mediante `jsonNodeReference` o `jsonPathDefinition`, u omitir la extracción no especificándolo en `jsonPathDefinition`.
* Si hay algún nombre duplicado en el mismo nivel, la actividad de copia elige el último.
* Los nombres de propiedad distinguen entre mayúsculas y minúsculas. Dos propiedades con el mismo nombre, pero con distintas mayúsculas y minúsculas se consideran propiedades independientes.

**Caso 2: Escritura de datos en el archivo JSON**

Si tiene la siguiente tabla en SQL Database:

| id | order_date | order_price | order_by |
| --- | --- | --- | --- |
| 1 | 20170119 | 2000 | David |
| 2 | 20170120 | 3500 | Patrick |
| 3 | 20170121 | 4000 | Jason |

y para cada registro, espera escribir en un objeto JSON con el formato siguiente:
```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

El conjunto de datos de salida con el tipo **JsonFormat** se define de la siguiente manera: (definición parcial en la que solo se ilustran las secciones relevantes). Más concretamente, la sección `structure` permite definir los nombres de propiedad personalizados en el archivo de destino y `nestingSeparator` (el valor predeterminado es ".") se usa para identificar el nivel de anidamiento del nombre. Esta sección es **opcional** a menos que desee cambiar el nombre de la propiedad comparándolo con el nombre de la columna de origen, o anidar algunas de las propiedades.

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

## <a name="avro-format"></a>Formato AVRO
Si quiere analizar los archivos Avro o escribir los datos en formato Avro, establezca la propiedad `format` `type` en **AvroFormat**. No es preciso especificar propiedades en la sección Format de la sección typeProperties. Ejemplo:

```json
"format":
{
    "type": "AvroFormat",
}
```

Para usar el formato Avro en una tabla de Hive, puede consultar [Tutorial de Apache Hive](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Tenga en cuenta los siguientes puntos:  

* No se admiten [tipos de datos complejos](https://avro.apache.org/docs/current/spec.html#schema_complex) (registros, enumeraciones, matrices, asignaciones, uniones y fijos).

## <a name="orc-format"></a>Formato ORC
Si quiere analizar los archivos ORC o escribir los datos en formato ORC, establezca la propiedad `format` `type` en **OrcFormat**. No es preciso especificar propiedades en la sección Format de la sección typeProperties. Ejemplo:

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> Si no va a copiar archivos ORC **como están** entre almacenes de datos locales y en la nube, debe instalar JRE 8 (Java Runtime Environment) en la máquina de puerta de enlace. Una puerta de enlace de 64 bits requiere JRE de 64 bits y una de 32 bits, JRE de 32 bits. Puede encontrar las dos versiones [aquí](https://go.microsoft.com/fwlink/?LinkId=808605). Elija la más adecuada.
>
>

Tenga en cuenta los siguientes puntos:

* No se admiten tipos de daros complejos (STRUCT, MAP, LIST, UNION).
* El archivo ORC tiene tres [opciones relacionadas con la compresión](https://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB Y SNAPPY. Data Factory admite la lectura de datos del archivo ORC en cualquiera de los formatos comprimidos. Se utiliza el códec de compresión en los metadatos para leer los datos. Sin embargo, al escribir en un archivo ORC, Data Factory elige ZLIB que es el valor predeterminado para ORC. Por el momento, no hay ninguna opción para invalidar este comportamiento.

## <a name="parquet-format"></a>Formato Parquet
Si quiere analizar los archivos Parquet o escribir los datos en formato Parquet, establezca la propiedad `format` `type` en **ParquetFormat**. No es preciso especificar propiedades en la sección Format de la sección typeProperties. Ejemplo:

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> Si no va a copiar archivos Parquet **como están** entre almacenes de datos locales y en la nube, debe instalar JRE 8 (Java Runtime Environment) en la máquina de puerta de enlace. Una puerta de enlace de 64 bits requiere JRE de 64 bits y una de 32 bits, JRE de 32 bits. Puede encontrar las dos versiones [aquí](https://go.microsoft.com/fwlink/?LinkId=808605). Elija la más adecuada.
>
>

Tenga en cuenta los siguientes puntos:

* No se admiten tipos de daros complejos (MAP, LIST).
* El archivo Parquet tiene las siguientes opciones relacionadas con la compresión: NONE, SNAPPY, GZIP y LZO. Data Factory admite la lectura de datos del archivo ORC en cualquiera de los formatos comprimidos. Utiliza el códec de compresión en los metadatos para leer los datos. Sin embargo, al escribir en un archivo Parquet, Data Factory elige SNAPPY que es el valor predeterminado para Parquet. Por el momento, no hay ninguna opción para invalidar este comportamiento.

## <a name="compression-support"></a>Compatibilidad de compresión
El procesamiento de grandes conjuntos de datos puede provocar cuellos de botella de E/S y red. Por lo tanto, los datos comprimidos en almacenes pueden no solo acelerar la transferencia de datos a través de la red y ahorrar espacio en disco, sino también introducir importantes mejoras de rendimiento en el procesamiento de macrodatos. En este momento, se admite la compresión para almacenes de datos basados en archivos como Blob de Azure o el sistema de archivos local.  

Para especificar la compresión para un conjunto de datos, use la propiedad **compression** del conjunto de datos JSON como en el ejemplo siguiente:   

```json
{  
    "name": "AzureBlobDataSet",  
    "properties": {  
        "availability": {  
            "frequency": "Day",  
              "interval": 1  
        },  
        "type": "AzureBlob",  
        "linkedServiceName": "StorageLinkedService",  
        "typeProperties": {  
            "fileName": "pagecounts.csv.gz",  
            "folderPath": "compression/file/",  
            "compression": {  
                "type": "GZip",  
                "level": "Optimal"  
            }  
        }  
    }  
}  
```

Supongamos que el conjunto de datos de ejemplo se usa como salida de una actividad de copia; la actividad de copia comprime los datos de salida con el códec GZIP con una proporción óptima y, luego, escribe los datos comprimidos en un archivo denominado pagecounts.csv.gz en Azure Blob Storage.

> [!NOTE]
> La configuración de la compresión no se admite para los datos con los formatos **AvroFormat**, **OrcFormat** o **ParquetFormat**. Al leer archivos en estos formatos, Data Factory detecta y usa el códec de compresión en los metadatos. Sin embargo, al escribir en archivos en estos formatos, Data Factory elige el códec de compresión predeterminado para ese formato. Por ejemplo, ZLIB en el caso de OrcFormat y SNAPPY en el caso de ParquetFormat.   

La sección **compression** tiene dos propiedades:  

* **Type**: el códec de compresión, que puede ser **GZIP**, **Deflate** o **BZIP2** o **ZipDeflate**.  
* **Level**: la relación de compresión, que puede ser **Optimal** o **Fastest**.

  * **Fastest**: la operación de compresión debe completarse tan pronto como sea posible, incluso si el archivo resultante no se comprime de forma óptima.
  * **Optimal**: la operación de compresión se debe comprimir óptimamente, incluso si tarda más tiempo en completarse.

    Para más información, consulte el tema [Nivel de compresión](/dotnet/api/system.io.compression.compressionlevel) .

Cuando se especifica la propiedad `compression` en un JSON del conjunto de datos de entrada, la canalización puede leer los datos comprimidos del origen, mientras que cuando se especifica la propiedad en un conjunto del conjunto de datos de salida, la actividad de copia puede escribir datos comprimidos en el destino. Estos son algunos escenarios de ejemplo:

* Leer datos comprimidos GZIP de un blob de Azure, descomprimirlos y escribir los datos de resultado en Azure SQL Database. Defina el conjunto de datos del blob de Azure de entrada con la propiedad de JSON `compression` `type` como GZIP.
* Leer datos de un archivo de texto sin formato del sistema de archivos local, comprimirlos con formato GZip y escribir los datos comprimidos en un blob de Azure. Defina el conjunto de datos del blob de Azure de salida con la propiedad de JSON `compression` `type` como GZip.
* Leer el archivo .zip del servidor FTP, descomprimirlo para obtener los archivos que contiene y colocar dichos archivos en Azure Data Lake Store. Defina el conjunto de datos del FTP de entrada con la propiedad de JSON `compression` `type` como ZipDeflate.
* Leer datos comprimidos con GZIP de un blob de Azure, descomprimirlos, comprimirlos con BZIP2 y escribir los datos de resultado en un blob de Azure. Defina el conjunto de datos de blob de Azure de entrada con `compression` `type` establecidos en GZIP y el conjunto de datos de salida con `compression` `type` establecido en BZIP2 en este caso.   


## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes artículos relativos a los almacenes de datos basados en archivos compatibles con Azure Data Factory:

- [Azure Blob Storage](data-factory-azure-blob-connector.md)
- [Azure Data Lake Store](data-factory-azure-datalake-connector.md)
- [FTP](data-factory-ftp-connector.md)
- [HDFS](data-factory-hdfs-connector.md)
- [Sistema de archivos](data-factory-onprem-file-system-connector.md)
- [Amazon S3](data-factory-amazon-simple-storage-service-connector.md)