---
title: Uso de tablas externas con Synapse SQL
description: Lectura o escritura de archivos de datos con tablas externas en Synapse SQL
services: synapse-analytics
author: ma77b
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 07/23/2021
ms.author: maburd
ms.reviewer: wiassaf
ms.openlocfilehash: a229bd769afa30b93cae9ca0f2073ad8a0621cdd
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "130001397"
---
# <a name="use-external-tables-with-synapse-sql"></a>Uso de tablas externas con Synapse SQL

Una tabla externa apunta a datos ubicados en Hadoop, Azure Storage Blob o Azure Data Lake Storage. Las tablas externas se usan para leer datos de archivos de Azure Storage o escribir datos en ellos. Con Synapse SQL se pueden usar tablas externas para leer datos externos mediante un grupo de SQL dedicado o un grupo de SQL sin servidor.

En función del tipo de origen de datos externo, puede usar dos tipos de tablas externas:
- Tablas externas de Hadoop, que puede usar para leer y exportar datos en varios formatos de datos, como CSV, Parquet y ORC. Las tablas externas de Hadoop están disponibles en los grupos de SQL dedicados, pero no en los grupos de SQL sin servidor.
- Tablas externas nativas, que puede usar para leer y exportar datos en varios formatos de datos, como CSV y Parquet. Las tablas externas nativas están disponibles en los grupos de SQL sin servidor, y están disponibles en **versión preliminar pública** en grupos de SQL dedicados.

Las principales diferencias entre Hadoop y las tablas externas nativas se presentan en la tabla siguiente:

| Tipo de tabla externa | Hadoop | Nativa |
| --- | --- | --- |
| Grupo de SQL dedicado | Disponible | Las tablas de Parquet están disponibles en **versión preliminar pública**. |
| Grupo de SQL sin servidor | No disponible | Disponible |
| Formatos compatibles | Delimitado/CSV, Parquet, ORC, Hive RC y RC | Grupo de SQL sin servidor: delimitado/CSV, Parquet y [Delta Lake (versión preliminar)](query-delta-lake-format.md)<br/>Grupo de SQL dedicado: Parquet |
| Eliminación de particiones de carpetas | No | Solo para las tablas con particiones sincronizadas desde grupos de Apache Spark en el área de trabajo de Synapse a grupos de SQL sin servidor |
| Formato personalizado para la ubicación | Yes | Sí, con caracteres comodín como `/year=*/month=*/day=*` |
| Examen recursivo de carpetas | No | Solo en grupos de SQL sin servidor cuando se especifica `/**` al final de la ruta de acceso de ubicación |
| Delegación del filtro de almacenamiento | No | Sí en el grupo de SQL sin servidor. Para la delegación de cadenas, debe usar la intercalación `Latin1_General_100_BIN2_UTF8` en las columnas `VARCHAR`. |
| Autenticación del almacenamiento | Clave de acceso de almacenamiento (SAK), acceso directo de AAD, identidad administrada, identidad de aplicación personalizada de Azure AD | Firma de acceso compartido (SAS), acceso directo de AAD, identidad administrada |

> [!NOTE]
> Las tablas externas nativas en formato Delta Lake están en versión preliminar pública. Para más información, consulte [Consulta de archivos de Delta Lake (versión preliminar)](query-delta-lake-format.md). [CETAS](develop-tables-cetas.md) no admite la exportación de contenido en formato Delta Lake.

## <a name="external-tables-in-dedicated-sql-pool-and-serverless-sql-pool"></a>Tablas externas en un grupo de SQL dedicado y en un grupo de SQL sin servidor

Puede usar tablas externas para:

- Consultar Azure Blob Storage y Azure Data Lake Gen2 con instrucciones Transact-SQL.
- Almacenar los resultados de las consultas en archivos de Azure Blob Storage o Azure Data Lake Storage mediante [CETAS](develop-tables-cetas.md).
- Importar datos de Azure Blob Storage y Azure Data Lake Storage y almacenarlos en un grupo de SQL dedicado (solo tablas de Hadoop en un grupo dedicado).

> [!NOTE]
> Si se usa en combinación con la instrucción [CREATE TABLE AS SELECT](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?context=/azure/synapse-analytics/context/context), al realizar la selección desde una tabla externa se importan los datos en una tabla de un grupo de SQL **dedicado**. Además de para la [instrucción COPY](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest&preserve-view=true), las tablas externas son útiles para cargar datos. 
> 
> Si desea ver un tutorial de carga, consulte el artículo en el que se explica el [uso de PolyBase para cargar datos de Azure Blob Storage](../sql-data-warehouse/load-data-from-azure-blob-storage-using-copy.md?bc=%2fazure%2fsynapse-analytics%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fsynapse-analytics%2ftoc.json).

Para crear tablas externas en grupos de Synapse SQL, siga estos pasos:

1. [CREAR ORIGEN DE DATOS EXTERNO](#create-external-data-source) para hacer referencia a un almacenamiento externo de Azure y especificar la credencial que se debe usar para acceder al almacenamiento.
2. [CREAR FORMATO DE ARCHIVO EXTERNO](#create-external-file-format) para describir el formato de los archivos CSV o Parquet.
3. [CREAR TABLA EXTERNA](#create-external-table) sobre los archivos incluidos en el origen de datos con el mismo formato de archivo.

### <a name="security"></a>Seguridad

El usuario debe tener permiso `SELECT` en una tabla externa para leer los datos.
Las tablas externas acceden al almacenamiento de Azure subyacente mediante la credencial con ámbito de base de datos definida en el origen de datos mediante las siguientes reglas:
- El origen de datos sin credenciales permite que las tablas externas accedan a archivos disponibles públicamente en Azure Storage.
- El origen de datos puede tener una credencial que permita a las tablas externas acceder solo a los archivos de Azure Storage mediante el token de SAS o la identidad administrada del área de trabajo. Para consultar ejemplos, consulte el artículo sobre el [desarrollo del control de acceso al almacenamiento de archivos](develop-storage-files-storage-access-control.md#examples).

## <a name="create-external-data-source"></a>CREATE EXTERNAL DATA SOURCE

Los orígenes de datos externos se usan para conectarse a las cuentas de almacenamiento. La documentación completa se describe [aquí](/sql/t-sql/statements/create-external-data-source-transact-sql?view=azure-sqldw-latest&preserve-view=true).

### <a name="syntax-for-create-external-data-source"></a>Sintaxis de CREATE EXTERNAL DATA SOURCE

#### <a name="hadoop"></a>[Hadoop](#tab/hadoop)

Los orígenes de datos externos con `TYPE=HADOOP` solo están disponibles en los grupos de SQL dedicados.

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
(    LOCATION         = '<prefix>://<path>'
     [, CREDENTIAL = <database scoped credential> ]
     , TYPE = HADOOP
)
[;]
```

#### <a name="native"></a>[Nativa](#tab/native)

Los orígenes de datos externos sin `TYPE=HADOOP` están disponibles con carácter general en grupos de SQL sin servidor y en versión preliminar pública en grupos dedicados.

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
(    LOCATION         = '<prefix>://<path>'
     [, CREDENTIAL = <database scoped credential> ]
)
[;]
```

---

### <a name="arguments-for-create-external-data-source"></a>Argumentos de CREATE EXTERNAL DATA SOURCE

#### <a name="data_source_name"></a>data_source_name

Especifica el nombre definido por el usuario para el origen de datos. El nombre debe ser único en la base de datos.

#### <a name="location"></a>Location
LOCATION = `'<prefix>://<path>'`: proporciona el protocolo de conectividad y la ruta de acceso al origen de datos externo. Estos patrones se pueden usar en la ubicación:

| Origen de datos externo        | Prefijo de ubicación | Ruta de acceso de ubicación                                         |
| --------------------------- | --------------- | ----------------------------------------------------- |
| Azure Blob Storage          | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` |
| Azure Blob Storage          | `http[s]`       | `<storage_account>.blob.core.windows.net/<container>/subfolders` |
| Azure Data Lake Store Gen1 | `http[s]`       | `<storage_account>.azuredatalakestore.net/webhdfs/v1` |
| Azure Data Lake Store Gen2 | `http[s]`       | `<storage_account>.dfs.core.windows.net/<container>/subfolders`  |

El prefijo `https:` le permite usar la subcarpeta en la ruta de acceso.

#### <a name="credential"></a>Credential:
CREDENTIAL = `<database scoped credential>` es una credencial opcional que se usará para realizar la autenticación en Azure Storage. Un origen de datos externo sin credencial puede acceder a la cuenta de almacenamiento pública o usar la identidad de Azure AD del autor de llamada para acceder a los archivos del almacenamiento. 
- En un grupo de SQL dedicado, las credenciales cuyo ámbito es la base de datos pueden especificar una identidad de aplicación personalizada, una identidad administrada de área de trabajo o una clave SAK. 
- En un grupo de SQL sin servidor, las credenciales cuyo ámbito es la base de datos pueden especificar una identidad administrada de área de trabajo o una clave SAS. 

#### <a name="type"></a>TYPE
TYPE = `HADOOP` es la opción que especifica que se debe usar la tecnología basada en Java para acceder a los archivos subyacentes. Este parámetro no se puede usar en los grupos de SQL sin servidor que usen un lector nativo integrado.

### <a name="example-for-create-external-data-source"></a>Ejemplo de CREATE EXTERNAL DATA SOURCE

#### <a name="hadoop"></a>[Hadoop](#tab/hadoop)

En el ejemplo siguiente se crea un origen de datos externo de Hadoop en un grupo de SQL dedicado para Azure Data Lake Gen2 que apunta al conjunto de datos de Nueva York:

```sql
CREATE DATABASE SCOPED CREDENTIAL [ADLS_credential]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D'
GO

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH
  -- Please note the abfss endpoint when your account has secure transfer enabled
  ( LOCATION = 'abfss://data@newyorktaxidataset.dfs.core.windows.net' ,
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;
```

En el ejemplo siguiente se crea un origen de datos externo para Azure Data Lake Gen2 que apunta al conjunto de datos de Nueva York disponible públicamente:

```sql
CREATE EXTERNAL DATA SOURCE YellowTaxi
WITH ( LOCATION = 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/',
       TYPE = HADOOP)
```

#### <a name="native"></a>[Nativa](#tab/native)

En el ejemplo siguiente se crea un origen de datos externo en un grupo de SQL sin servidor o dedicado para Azure Data Lake Gen2 al que se puede acceder mediante las credenciales de SAS:

```sql
CREATE DATABASE SCOPED CREDENTIAL [sqlondemand]
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = 'sv=2018-03-28&ss=bf&srt=sco&sp=rl&st=2019-10-14T12%3A10%3A25Z&se=2061-12-31T12%3A10%3A00Z&sig=KlSU2ullCscyTS0An0nozEpo4tO5JAgGBvw%2FJX2lguw%3D'
GO

CREATE EXTERNAL DATA SOURCE SqlOnDemandDemo WITH (
    LOCATION = 'https://sqlondemandstorage.blob.core.windows.net',
    CREDENTIAL = sqlondemand
);
```

En el ejemplo siguiente se crea un origen de datos externo para Azure Data Lake Gen2 que apunta al conjunto de datos de Nueva York disponible públicamente:

```sql
CREATE EXTERNAL DATA SOURCE YellowTaxi
WITH ( LOCATION = 'https://azureopendatastorage.blob.core.windows.net/nyctlc/yellow/')
```
---

## <a name="create-external-file-format"></a>CREATE EXTERNAL FILE FORMAT

Crea un objeto de formato de archivo externo que define los datos externos almacenados en Azure Blob Storage o Azure Data Lake Store. La creación de un formato de archivo externo es un requisito previo para crear una tabla externa. La documentación completa se encuentra [aquí](/sql/t-sql/statements/create-external-file-format-transact-sql?view=azure-sqldw-latest&preserve-view=true).

Al crear un formato de archivo externo, se especifica el diseño real de los datos a los que hace referencia una tabla externa.

### <a name="syntax-for-create-external-file-format"></a>Sintaxis de CREATE EXTERNAL FILE FORMAT

```syntaxsql
-- Create an external file format for PARQUET files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = PARQUET  
    [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      }  
    ]);  

--Create an external file format for DELIMITED TEXT files
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT  
    [ , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec' ]
    [ , FORMAT_OPTIONS ( <format_options> [ ,...n  ] ) ]  
    );  

<format_options> ::=  
{  
    FIELD_TERMINATOR = field_terminator  
    | STRING_DELIMITER = string_delimiter
    | First_Row = integer
    | USE_TYPE_DEFAULT = { TRUE | FALSE }
    | Encoding = {'UTF8' | 'UTF16'}
    | PARSER_VERSION = {'parser_version'}
}
```

### <a name="arguments-for-create-external-file-format"></a>Argumentos para CREATE EXTERNAL FILE FORMAT

file_format_name: especifica un nombre para el formato de archivo externo.

FORMAT_TYPE = [ PARQUET | DELIMITEDTEXT]: especifica el formato de los datos externos.

- PARQUET: especifica un formato Parquet.
- DELIMITEDTEXT: especifica un formato de texto con delimitadores de columna, también denominados terminadores de campo.

FIELD_TERMINATOR = *field_terminator*: solo se aplica a archivos de texto delimitado. El terminador de campo especifica uno o varios caracteres que marcan el final de cada campo (columna) en el archivo de texto delimitado. El valor predeterminado es el carácter de barra vertical ("|").

Ejemplos:

- FIELD_TERMINATOR = '|'
- FIELD_TERMINATOR = ' '
- FIELD_TERMINATOR = ꞌ\tꞌ

STRING_DELIMITER = *string_delimiter*: especifica el terminador de campo de los datos de tipo cadena en el archivo de texto delimitado. El delimitador de cadena tiene una longitud de uno o más caracteres y se escribe entre comillas simples. El valor predeterminado es la cadena vacía ("").

Ejemplos:

- STRING_DELIMITER = '"'
- STRING_DELIMITER = '*'
- STRING_DELIMITER = ꞌ,ꞌ

FIRST_ROW = *First_row_int*: especifica el número de fila que se lee primero y se aplica a todos los archivos. Si establece el valor en dos hará que se omita la primera fila de cada archivo (fila de encabezado) al cargar los datos. Las filas se omiten en función de la existencia de terminadores de fila (/r/n, /r, /n).

USE_TYPE_DEFAULT = { TRUE | **FALSE** }: especifica cómo administrar valores que faltan en archivos de texto delimitado al recuperar datos del archivo de texto.

TRUE: si va a recuperar datos del archivo de texto, almacene todos los valores que falten mediante el tipo de datos del valor predeterminado para la columna correspondiente en la definición de la tabla externa. Por ejemplo, reemplace un valor que falta con:

- 0 si la columna se define como una columna numérica. Las columnas decimales no se admiten y provocarán un error.
- Cadena vacía ("") si la columna es una columna de cadena.
- 1900-01-01 si la columna es una columna de fecha.

FALSE: almacena todos los valores que faltan como NULL. Los valores NULL que se almacenan mediante la palabra NULL en el archivo de texto delimitado se importan como la cadena 'NULL'.

Encoding = {'UTF8' | 'UTF16'}: el grupo de SQL sin servidor puede leer archivos de texto delimitados con codificación UTF8 y UTF16.

DATA_COMPRESSION = *data_compression_method*: este argumento especifica el método de compresión de datos para los datos externos. 

El tipo de formato PARQUET admite estos métodos de compresión:

- DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
- DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

Al leer de tablas externas de PARQUET, este argumento se omite, pero se usa al escribir en tablas externas mediante [CETAS](develop-tables-cetas.md).

El tipo de formato de archivo DELIMITEDTEXT admite estos métodos de compresión:

- DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'

PARSER_VERSION = 'parser_version' especifica la versión del analizador que se utilizará al leer archivos CSV. Las versiones disponibles del analizador son la `1.0` y la `2.0`. Esta opción solo está disponible en los grupos de SQL sin servidor.

### <a name="example-for-create-external-file-format"></a>Ejemplo de CREATE EXTERNAL FILE FORMAT

En el ejemplo siguiente se crea un formato de archivo externo para los archivos del censo:

```sql
CREATE EXTERNAL FILE FORMAT census_file_format
WITH
(  
    FORMAT_TYPE = PARQUET,
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
)
```

## <a name="create-external-table"></a>CREATE EXTERNAL TABLE

El comando CREATE EXTERNAL TABLE crea una tabla externa para Synapse SQL para acceder a los datos almacenados en Azure Blob Storage o Azure Data Lake Storage. 

### <a name="syntax-for-create-external-table"></a>Sintaxis de CREATE EXTERNAL TABLE

```sql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name
        [, TABLE_OPTIONS = N'{"READ_OPTIONS":["ALLOW_INCONSISTENT_READS"]}' ]
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
```

### <a name="arguments-create-external-table"></a>Argumento de CREATE EXTERNAL TABLE

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }*

Nombre de entre una y tres partes de la tabla que se va a crear. Si se trata de una tabla externa, el grupo de Synapse SQL almacena solo los metadatos de la tabla. Ningún dato real se mueve o se almacena en la base de datos de Synapse SQL.

<column_definition>, ...*n* ]

CREATE EXTERNAL TABLE admite la capacidad de configurar el nombre de columna, el tipo de datos y la intercalación. No se puede usar DEFAULT CONSTRAINT en tablas externas.

>[!IMPORTANT]
>Las definiciones de columna, incluidos los tipos de datos y el número de columnas, deben coincidir con los datos de los archivos externos. Si hay algún error de coincidencia, se rechazarán las filas de archivo al consultar los datos reales.

Al leer archivos con formato Parquet, solo puede especificar las columnas que desea leer y omitir el resto.

#### <a name="location"></a>LOCATION

LOCATION = '*folder_or_filepath*'

Especifica la carpeta o la ruta de acceso del archivo y el nombre de archivo de los datos reales en Azure Blob Storage. La ubicación empieza desde la carpeta raíz. La carpeta raíz es la ubicación de datos especificada en el origen de datos externo.

![Datos recursivos para tablas externas](./media/develop-tables-external-tables/folder-traversal.png)

A diferencia de las tablas externas de Hadoop, las tablas externas nativas no devuelven subcarpetas a menos que especifique /** al final de la ruta de acceso. En este ejemplo, si LOCATION='/webdata/', una consulta del grupo de SQL sin servidor, devolverá filas de mydata.txt. No devolverá mydata2. txt y mydata3. txt porque se encuentran en una subcarpeta. Las tablas de Hadoop devolverán todos los archivos de cualquier subcarpeta.
 
Tanto Hadoop como las tablas externas nativas omitirán los archivos con nombres que comiencen por un subrayado (_) o un punto (.).

#### <a name="data_source"></a>DATA_SOURCE

DATA_SOURCE = *external_data_source_name*: especifica el nombre del origen de datos externo que contiene la ubicación de los datos externos. Para crear un origen de datos externo, use[CREATE EXTERNAL DATA SOURCE](#create-external-data-source).

FILE_FORMAT = *external_file_format_name*: especifica el nombre del objeto de formato de archivo externo que almacena el tipo de archivo y el método de compresión de los datos externos. Para crear un formato de archivo externo, use [CREATE EXTERNAL FILE FORMAT](#create-external-file-format).

#### <a name="table_options"></a>OPCIONES DE TABLA

TABLE_OPTIONS = *json options*. Especifica el conjunto de opciones que describen cómo leer los archivos subyacentes. Actualmente, la única opción disponible es `"READ_OPTIONS":["ALLOW_INCONSISTENT_READS"]`, que indica a la tabla externa que ignore las actualizaciones realizadas en los archivos subyacentes, aunque esto pueda provocar algunas operaciones de lectura incoherentes. Use esta opción solo en casos especiales en los que haya anexado archivos frecuentemente. Esta opción está disponible en el grupo de SQL sin servidor para el formato CSV.

### <a name="permissions-create-external-table"></a>Permisos de CREATE EXTERNAL TABLE

Para realizar la selección en una tabla externa, se necesitan las credenciales adecuadas con permisos de lectura y de lista.

### <a name="example-create-external-table"></a>Ejemplo de CREATE EXTERNAL TABLE

En el ejemplo siguiente se crea una tabla externa. Devuelve la primera fila:

```sql
CREATE EXTERNAL TABLE census_external_table
(
    decennialTime varchar(20),
    stateName varchar(100),
    countyName varchar(100),
    population int,
    race varchar(50),
    sex    varchar(10),
    minAge int,
    maxAge int
)  
WITH (
    LOCATION = '/parquet/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)
GO

SELECT TOP 1 * FROM census_external_table
```

## <a name="create-and-query-external-tables-from-a-file-in-azure-data-lake"></a>Creación y consulta de tablas externas a partir de un archivo en Azure Data Lake

Mediante las funcionalidades de exploración de Data Lake de Synapse Studio ya se puede crear y consultar una tabla externa mediante un grupo de Synapse SQL con un solo clic con el botón derecho en el archivo. El gesto de un solo clic para crear tablas externas desde la cuenta de almacenamiento de ADLS Gen2 solo se admite para los archivos con formato Parquet. 

### <a name="prerequisites"></a>Requisitos previos

- Debe tener acceso al área de trabajo con al menos el rol de acceso `Storage Blob Data Contributor` a la cuenta de ADLS Gen2 o a las listas de control de acceso (ACL) que le permiten consultar los archivos.

- Debe tener al menos [permisos para crear](/sql/t-sql/statements/create-external-table-transact-sql?view=azure-sqldw-latest#permissions-2&preserve-view=true) y consultar tablas externas en el grupo de Synapse SQL (dedicado o sin servidor).

En el panel Data (Datos), seleccione el archivo desde el que desea crear la tabla externa:
> [!div class="mx-imgBorder"]
>![externaltable1](./media/develop-tables-external-tables/external-table-1.png)

Se abrirá una ventana de diálogo. Seleccione un grupo de SQL dedicado o sin servidor, asígnele un nombre a la tabla y seleccione Abrir script:

> [!div class="mx-imgBorder"]
>![externaltable2](./media/develop-tables-external-tables/external-table-2.png)

El script de SQL se genera automáticamente e infiere el esquema del archivo:
> [!div class="mx-imgBorder"]
>![externaltable3](./media/develop-tables-external-tables/external-table-3.png)

Ejecute el script. El script ejecutará automáticamente una instrucción Select Top 100 *:
> [!div class="mx-imgBorder"]
>![externaltable4](./media/develop-tables-external-tables/external-table-4.png)

Ahora se crea la tabla externa, para una futura exploración del contenido de esta tabla externa el usuario puede consultarla directamente desde el panel Data (Datos):
> [!div class="mx-imgBorder"]
>![externaltable5](./media/develop-tables-external-tables/external-table-5.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte el artículo [CETAS](develop-tables-cetas.md) para obtener información sobre cómo guardar los resultados de una consulta en una tabla externa en Azure Storage. O bien puede empezar a consultar [Tablas externas de Apache Spark para Azure Synapse](develop-storage-files-spark-tables.md).
