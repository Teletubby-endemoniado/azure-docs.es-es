---
title: Transferencia de datos con Azure Files como origen o destino mediante AzCopy v10 | Microsoft Docs
description: Transferencia de datos con AzCopy y File Storage. AzCopy es una herramienta de línea de comandos para copiar blobs o archivos a una cuenta de almacenamiento o de ella. Use AzCopy con Azure Files.
author: normesta
ms.service: storage
ms.topic: how-to
ms.date: 04/02/2021
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 3e60815b2361f4ba14b6a40ded2734c748f8d4ae
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128633704"
---
# <a name="transfer-data-with-azcopy-and-file-storage"></a>Transferencia de datos con AzCopy y File Storage

AzCopy es una utilidad de línea de comandos que puede usar para copiar archivos en una cuenta de almacenamiento o desde esta. Este artículo contiene comandos de ejemplo que funcionan con Azure Files.

Antes de comenzar, consulte el artículo [Introducción a AzCopy](storage-use-azcopy-v10.md) para descargar AzCopy y familiarizarse con la herramienta.

> [!TIP]
> En los ejemplos de este artículo se delimitan los argumentos de ruta de acceso con comillas simples (''). Use comillas simples en todos los shells de comandos excepto en el shell de comandos de Windows (cmd.exe). Si usa un shell de comandos de Windows (cmd.exe), incluya los argumentos de la ruta de acceso entre comillas dobles ("") en lugar de comillas simples ('').

## <a name="create-file-shares"></a>Creación de recursos compartidos de archivos

Puede usar el comando [azcopy make](storage-ref-azcopy-make.md) para crear un recurso compartido de archivos. El ejemplo de esta sección crea un recurso compartido de archivos denominado `myfileshare`.

**Sintaxis**

`azcopy make 'https://<storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>'`

**Ejemplo**

```azcopy
azcopy make 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D'
```

Para obtener documentos de referencia detallados, consulte [azcopy make](storage-ref-azcopy-make.md).

## <a name="upload-files"></a>Carga de archivos

Puede usar el comando [azcopy copy](storage-ref-azcopy-copy.md) para cargar archivos y directorios desde el equipo local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> - Cargar un archivo
> - Subir un directorio
> - Subir el contenido de un directorio
> - Cargar un archivo específico

> [!TIP]
> Puede modificar las operaciones de carga mediante marcas opcionales. Estos son algunos ejemplos.  
>
> |Escenario|Marca|
> |---|---|
> |Copiar las listas de control de acceso (ACL) junto con los archivos.|**--preserve-smb-permissions**=\[true\|false\]|
> |Copiar la información de la propiedad SMB junto con los archivos.|**--preserve-smb-info**=\[true\|false\]|
>
> Para obtener una lista completa, vea las [opciones](storage-ref-azcopy-copy.md#options).

> [!NOTE]
> AzCopy no calcula de forma automática ni almacena el código hash md5 del archivo. Si quiere que AzCopy haga eso, anexe la marca `--put-md5` a cada comando de copia. De ese modo, cuando se descarga el archivo, AzCopy calcula un hash MD5 para los datos descargados y comprueba que el hash MD5 almacenado en la propiedad `Content-md5` del archivo coincide con el hash calculado.

### <a name="upload-a-file"></a>Cargar un archivo

**Sintaxis**

`azcopy copy '<local-file-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-name><SAS-token>'`

**Ejemplo**

```azcopy
azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' --preserve-smb-permissions=true --preserve-smb-info=true
```

También puede cargar un archivo mediante un símbolo comodín (*) en cualquier lugar de la ruta de acceso o del nombre de archivo. Por ejemplo: `'C:\myDirectory\*.txt'` o `C:\my*\*.txt`.

### <a name="upload-a-directory"></a>Subir un directorio

En este ejemplo se copia un directorio (y todos sus archivos) en un recurso compartido de archivos. El resultado es un directorio en el recurso compartido de archivos con el mismo nombre.

**Sintaxis**

`azcopy copy '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive`

**Ejemplo**

```azcopy
azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

Para copiar un directorio dentro del recurso compartido de archivos, simplemente especifique el nombre de ese directorio en la cadena de comandos.

**Ejemplo**

```azcopy
azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

Si especifica el nombre de un directorio que no existe en el recurso compartido de archivos, AzCopy crea un directorio con ese nombre.

### <a name="upload-the-contents-of-a-directory"></a>Subir el contenido de un directorio

Puede cargar el contenido de un directorio sin copiar el propio directorio contenedor mediante el carácter comodín (*).

**Sintaxis**

`azcopy copy '<local-directory-path>/*' 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path><SAS-token>'`

**Ejemplo**

```azcopy
azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --preserve-smb-permissions=true --preserve-smb-info=true
```

> [!NOTE]
> Anexe la marca `--recursive` para cargar archivos en todos los subdirectorios.

### <a name="upload-specific-files"></a>Carga de archivos específicos

Puede cargar archivos específicos mediante nombres de archivo completos, nombres parciales con caracteres comodín (*) o fechas y horas.

#### <a name="specify-multiple-complete-file-names"></a>Especificación de varios nombres de archivo completos

Use el comando [azcopy copy](storage-ref-azcopy-copy.md) con la opción `--include-path`. Separe los nombres de archivo individuales mediante punto y coma (`;`).

**Sintaxis**

`azcopy copy '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' --include-path <semicolon-separated-file-list>`

**Ejemplo**

```azcopy
azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --include-path 'photos;documents\myFile.txt' --preserve-smb-permissions=true --preserve-smb-info=true
```

En este ejemplo, AzCopy transfiere el directorio `C:\myDirectory\photos` y el archivo `C:\myDirectory\documents\myFile.txt`. Debe incluir la opción `--recursive` para transferir todos los archivos del directorio `C:\myDirectory\photos`.

También puede excluir archivos mediante la opción `--exclude-path`. Para más información, consulte los documentos de referencia de [azcopy copy ](storage-ref-azcopy-copy.md).

#### <a name="use-wildcard-characters"></a>Uso de caracteres comodín

Use el comando [azcopy copy](storage-ref-azcopy-copy.md) con la opción `--include-pattern`. Especifique nombres parciales que incluyan los caracteres comodín. Separe los nombres con punto y coma (`;`).

**Sintaxis**

`azcopy copy '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>`

**Ejemplo**

```azcopy
azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --include-pattern 'myFile*.txt;*.pdf*' --preserve-smb-permissions=true --preserve-smb-info=true
```

También puede excluir archivos mediante la opción `--exclude-pattern`. Para más información, consulte los documentos de referencia de [azcopy copy ](storage-ref-azcopy-copy.md).

Las opciones `--include-pattern` y `--exclude-pattern` solo se aplican a los nombres de archivo, no a la ruta de acceso.  Si quiere copiar todos los archivos de texto que existen en un árbol de directorios, use la opción `--recursive` para obtener todo el árbol de directorios y, a continuación, use el `--include-pattern` y especifique `*.txt` para obtener todos los archivos de texto.

#### <a name="upload-files-that-were-modified-after-a-date-and-time"></a>Carga de archivos modificados después de una fecha y hora

Use el comando [azcopy copy](storage-ref-azcopy-copy.md) con la opción `--include-after`. Especifique una fecha y hora en formato ISO 8601 (por ejemplo: `2020-08-19T15:04:00Z`).

**Sintaxis**

`azcopy copy '<local-directory-path>\*' 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>'  --include-after <Date-Time-in-ISO-8601-format>`

**Ejemplo**

```azcopy
azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --include-after '2020-08-19T15:04:00Z' --preserve-smb-permissions=true --preserve-smb-info=true
```

Para ver una referencia detallada, consulte la documentación de referencia de [azcopy copy](storage-ref-azcopy-copy.md).

## <a name="download-files"></a>Descarga de archivos

Puede usar el comando [azcopy copy](storage-ref-azcopy-copy.md) para descargar archivos, directorios y recursos compartidos de archivos en la máquina local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> - Descarga de un archivo
> - Descargar un directorio
> - Descargar el contenido de un directorio
> - Descarga de archivos específicos

> [!TIP]
> Puede modificar las operaciones de descarga mediante marcas opcionales. Estos son algunos ejemplos.
>
> |Escenario|Marca|
> |---|---|
> |Copiar las listas de control de acceso (ACL) junto con los archivos.|**--preserve-smb-permissions**=\[true\|false\]|
> |Copiar la información de la propiedad SMB junto con los archivos.|**--preserve-smb-info**=\[true\|false\]|
> |Descomprimir archivos automáticamente.|**DECOMPRESS**|
>
> Para obtener una lista completa, vea las [opciones](storage-ref-azcopy-copy.md#options).

> [!NOTE]
> Si el valor de la propiedad `Content-md5` de un archivo contiene un hash, AzCopy calcula un hash MD5 para los datos descargados y comprueba que el hash MD5 almacenado en la propiedad `Content-md5` del archivo coincide con el hash calculado. Si estos valores no coinciden, se produce un error en la descarga a menos que invalide este comportamiento mediante la anexión de `--check-md5=NoCheck` o `--check-md5=LogOnly` al comando de copia.

### <a name="download-a-file"></a>Descarga de un archivo

**Sintaxis**

`azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path><SAS-token>' '<local-file-path>'`

**Ejemplo**

```azcopy
azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' 'C:\myDirectory\myTextFile.txt' --preserve-smb-permissions=true --preserve-smb-info=true
```

### <a name="download-a-directory"></a>Descargar un directorio

**Sintaxis**

`azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path><SAS-token>' '<local-directory-path>' --recursive`

**Ejemplo**

```azcopy
azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' 'C:\myDirectory'  --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

En este ejemplo se crea un directorio denominado `C:\myDirectory\myFileShareDirectory` que contiene todos los archivos descargados.

### <a name="download-the-contents-of-a-directory"></a>Descargar el contenido de un directorio

Puede descargar el contenido de un directorio sin copiar el propio directorio contenedor mediante el carácter comodín (*).

**Sintaxis**

`azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/*<SAS-token>' '<local-directory-path>/'`

**Ejemplo**

```azcopy
azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory/*?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' 'C:\myDirectory' --preserve-smb-permissions=true --preserve-smb-info=true
```

> [!NOTE]
> Anexe la marca `--recursive` para descargar archivos en todos los subdirectorios.

### <a name="download-specific-files"></a>Descarga de archivos específicos

Puede descargar archivos específicos mediante nombres de archivo completos, nombres parciales con caracteres comodín (*) o fechas y horas.

#### <a name="specify-multiple-complete-file-names"></a>Especificación de varios nombres de archivo completos

Use el comando [azcopy copy](storage-ref-azcopy-copy.md) con la opción `--include-path`. Separe los nombres de archivo individuales mediante punto y coma (`;`).

**Sintaxis**

`azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' '<local-directory-path>'  --include-path <semicolon-separated-file-list>`

**Ejemplo**

```azcopy
azcopy copy 'https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

En este ejemplo, AzCopy transfiere el directorio `https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory/photos` y el archivo `https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory/documents/myFile.txt`. Incluya la opción `--recursive` para transferir todos los archivos del directorio `https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory/photos`.

También puede excluir archivos mediante la opción `--exclude-path`. Para más información, consulte los documentos de referencia de [azcopy copy ](storage-ref-azcopy-copy.md).

#### <a name="use-wildcard-characters"></a>Uso de caracteres comodín

Use el comando [azcopy copy](storage-ref-azcopy-copy.md) con la opción `--include-pattern`. Especifique nombres parciales que incluyan los caracteres comodín. Separe los nombres con punto y coma (`;`).

**Sintaxis**

`azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' '<local-directory-path>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>`

**Ejemplo**

```azcopy
azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*' --preserve-smb-permissions=true --preserve-smb-info=true
```

También puede excluir archivos mediante la opción `--exclude-pattern`. Para más información, consulte los documentos de referencia de [azcopy copy ](storage-ref-azcopy-copy.md).

Las opciones `--include-pattern` y `--exclude-pattern` solo se aplican a los nombres de archivo, no a la ruta de acceso.  Si quiere copiar todos los archivos de texto que existen en un árbol de directorios, use la opción `--recursive` para obtener todo el árbol de directorios y, a continuación, use el `--include-pattern` y especifique `*.txt` para obtener todos los archivos de texto.

#### <a name="download-files-that-were-modified-after-a-date-and-time"></a>Descarga de archivos modificados después de una fecha y hora

Use el comando [azcopy copy](storage-ref-azcopy-copy.md) con la opción `--include-after`. Especifique una fecha y hora en formato ISO-8601 (por ejemplo: `2020-08-19T15:04:00Z`).

**Sintaxis**

`azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name>/*<SAS-token>' '<local-directory-path>'  --include-after <Date-Time-in-ISO-8601-format>`

**Ejemplo**

```azcopy
azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/*?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'C:\myDirectory' --include-after '2020-08-19T15:04:00Z' --preserve-smb-permissions=true --preserve-smb-info=true
```

Para ver una referencia detallada, consulte la documentación de referencia de [azcopy copy](storage-ref-azcopy-copy.md).

#### <a name="download-from-a-share-snapshot"></a>Descarga desde una instantánea de recurso compartido

Puede descargar una versión específica de un archivo o directorio haciendo referencia al valor **DateTime** de una instantánea de recurso compartido. Para más información sobre el uso compartido de instantáneas, consulte [Introducción a las instantáneas de recurso compartido de Azure Files](../files/storage-snapshots-files.md).

**Sintaxis**

`azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path-or-directory-name><SAS-token>&sharesnapshot=<DateTime-of-snapshot>' '<local-file-or-directory-path>'`

**Ejemplo (Descargar un archivo)**

```azcopy
azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D&sharesnapshot=2020-09-23T08:21:07.0000000Z' 'C:\myDirectory\myTextFile.txt' --preserve-smb-permissions=true --preserve-smb-info=true
```

**Ejemplo (Descargar un directorio)**

```azcopy
azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D&sharesnapshot=2020-09-23T08:21:07.0000000Z' 'C:\myDirectory'  --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

## <a name="copy-files-between-storage-accounts"></a>Copia de archivos entre cuentas de almacenamiento

Puede usar AzCopy para copiar archivos a otras cuentas de almacenamiento. La operación de copia es sincrónica, por lo que cuando el comando devuelve un resultado, eso indica que se han copiado todos los archivos.

AzCopy usa interfaces [API](/rest/api/storageservices/put-page-from-url)[de servidor a servidor](/rest/api/storageservices/put-block-from-url), por lo que los datos se copian directamente entre servidores de almacenamiento. En estas operaciones de copia no se usa el ancho de banda de red del equipo. Para aumentar el rendimiento de estas operaciones puede establecer el valor de la variable de entorno `AZCOPY_CONCURRENCY_VALUE`. Para más información, consulte [Aumento de la simultaneidad](storage-use-azcopy-optimize.md#increase-concurrency).

También puede copiar versiones específicas de un archivo haciendo referencia al valor **DateTime** de una instantánea de recurso compartido. Para más información sobre el uso compartido de instantáneas, consulte [Introducción a las instantáneas de recurso compartido de Azure Files](../files/storage-snapshots-files.md).

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> - Copia de un archivo a otra cuenta de almacenamiento
> - Copia de un directorio a otra cuenta de almacenamiento
> - Copia de un recurso compartido de archivos a otra cuenta de almacenamiento
> - Copia de todos los recurso compartido de archivos, directorios y archivos a otra cuenta de almacenamiento

> [!TIP]
> Puede modificar las operaciones de copia mediante marcas opcionales. Estos son algunos ejemplos.
>
> |Escenario|Marca|
> |---|---|
> |Copiar las listas de control de acceso (ACL) junto con los archivos.|**--preserve-smb-permissions**=\[true\|false\]|
> |Copiar la información de la propiedad SMB junto con los archivos.|**--preserve-smb-info**=\[true\|false\]|
>
> Para obtener una lista completa, vea las [opciones](storage-ref-azcopy-copy.md#options).

### <a name="copy-a-file-to-another-storage-account"></a>Copia de un archivo a otra cuenta de almacenamiento

**Sintaxis**

`azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name>/<file-path><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name>/<file-path><SAS-token>'`

**Ejemplo**

```azcopy
azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --preserve-smb-permissions=true --preserve-smb-info=true
```

**Ejemplo (instantánea de recurso compartido)**

```azcopy
azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D&sharesnapshot=2020-09-23T08:21:07.0000000Z' 'https://mydestinationaccount.file.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --preserve-smb-permissions=true --preserve-smb-info=true
```

### <a name="copy-a-directory-to-another-storage-account"></a>Copia de un directorio a otra cuenta de almacenamiento

**Sintaxis**

`azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive`

**Ejemplo**

```azcopy
azcopy copy 'https://mysourceaccount.file.core.windows.net/myFileShare/myFileDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

**Ejemplo (instantánea de recurso compartido)**

```azcopy
azcopy copy 'https://mysourceaccount.file.core.windows.net/myFileShare/myFileDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D&sharesnapshot=2020-09-23T08:21:07.0000000Z' 'https://mydestinationaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

### <a name="copy-a-file-share-to-another-storage-account"></a>Copia de un recurso compartido de archivos a otra cuenta de almacenamiento

**Sintaxis**

`azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive`

**Ejemplo**

```azcopy
azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D --preserve-smb-permissions=true --preserve-smb-info=true
```

**Ejemplo (instantánea de recurso compartido)**

```azcopy
azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D&sharesnapshot=2020-09-23T08:21:07.0000000Z' 'https://mydestinationaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

### <a name="copy-all-file-shares-directories-and-files-to-another-storage-account"></a>Copia de todos los recurso compartido de archivos, directorios y archivos a otra cuenta de almacenamiento

**Sintaxis**

`azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<SAS-token>' --recursive'`

**Ejemplo**

```azcopy
azcopy copy 'https://mysourceaccount.file.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

**Ejemplo (instantánea de recurso compartido)**

```azcopy
azcopy copy 'https://mysourceaccount.file.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D&sharesnapshot=2020-09-23T08:21:07.0000000Z' 'https://mydestinationaccount.file.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

## <a name="synchronize-files"></a>Sincronización de archivos

Puede sincronizar el contenido de un sistema de archivos local con un recurso compartido de archivos, o bien sincronizar el contenido de un recurso compartido de archivos con otro. También puede sincronizar el contenido de un directorio de un recurso compartido de archivos con el contenido de un directorio que se encuentra en otro recurso compartido de archivos. La sincronización es unidireccional. En otras palabras, tendrá que elegir cuál de estos dos puntos de conexión es el origen y cuál es el destino. La sincronización también usa las API de servidor a servidor.

> [!NOTE]
> En la actualidad, este escenario solo se admite para las cuentas que no tienen un espacio de nombres jerárquico. La versión actual de AzCopy no se sincroniza entre Azure Files y Blob Storage.

### <a name="guidelines"></a>Directrices

- El comando [sync](storage-ref-azcopy-sync.md) compara nombres de archivo y las marcas de tiempo de la última modificación. Establezca la marca opcional `--delete-destination` en un valor de `true` o `prompt` para eliminar archivos en el directorio de destino si esos archivos ya no existen en el directorio de origen.

- Si establece la marca `--delete-destination` en `true`, AzCopy elimina los archivos sin proporcionar un aviso. Si quiere que aparezca un mensaje antes de que AzCopy elimine un archivo, establezca la marca `--delete-destination` en `prompt`.

- Si tiene previsto establecer la marca `--delete-destination` en `prompt` o `false`, considere la posibilidad de usar el comando [copy](storage-ref-azcopy-copy.md) en lugar del comando [sync](storage-ref-azcopy-sync.md) y establezca el parámetro `--overwrite` en `ifSourceNewer`. El comando [copy](storage-ref-azcopy-copy.md) consume menos memoria y genera menos costos de facturación porque una operación de copia no tiene que indexar el origen o el destino antes de mover archivos.

- La máquina en la que ejecute el comando de sincronización debe tener un reloj del sistema preciso porque las últimas horas modificadas son fundamentales para determinar si se debe transferir un archivo. Si el sistema tiene un sesgo de reloj importante, evite modificar los archivos en el destino demasiado cerca de la hora en que planea ejecutar un comando de sincronización.

> [!TIP]
> Puede modificar las operaciones de sincronización mediante marcas opcionales. Estos son algunos ejemplos.
>
> |Escenario|Marca|
> |---|---|
> |Copiar las listas de control de acceso (ACL) junto con los archivos.|**--preserve-smb-permissions**=\[true\|false\]|
> |Copiar la información de la propiedad SMB junto con los archivos.|**--preserve-smb-info**=\[true\|false\]|
> |Excluir archivos en función de un patrón.|**--exclude-path**|
> |Especifique el grado de detalles que quiere que sean las entradas de registro relacionadas con la sincronización.|**--log-level**=\[WARNING\|ERROR\|INFO\|NONE\]|
>
> Para obtener una lista completa, vea las [opciones](storage-ref-azcopy-sync.md#options).

### <a name="update-a-file-share-with-changes-to-a-local-file-system"></a>Actualización de un recurso compartido de archivos con los cambios realizados en un sistema de archivos local

En este caso, el recurso compartido de archivos es el destino y el sistema de archivos local es el origen.

> [!TIP]
> En este ejemplo los argumentos de ruta de acceso se encierran entre comillas simples ('). Use comillas simples en todos los shells de comandos excepto en el shell de comandos de Windows (cmd.exe). Si usa un shell de comandos de Windows (cmd.exe), incluya los argumentos de la ruta de acceso entre comillas dobles ("") en lugar de comillas simples ('').

**Sintaxis**

`azcopy sync '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive`

**Ejemplo**

```azcopy
azcopy sync 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileShare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive
```

### <a name="update-a-local-file-system-with-changes-to-a-file-share"></a>Actualización de un sistema de archivos local con los cambios realizados en un recurso compartido de archivos

En este caso, el sistema de archivos local es el destino y el recurso compartido de archivos es el origen.

> [!TIP]
> En este ejemplo los argumentos de ruta de acceso se encierran entre comillas simples ('). Use comillas simples en todos los shells de comandos excepto en el shell de comandos de Windows (cmd.exe). Si usa un shell de comandos de Windows (cmd.exe), incluya los argumentos de la ruta de acceso entre comillas dobles ("") en lugar de comillas simples ('').

**Sintaxis**

`azcopy sync 'https://<storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' 'C:\myDirectory' --recursive`

**Ejemplo**

```azcopy
azcopy sync 'https://mystorageaccount.file.core.windows.net/myfileShare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'C:\myDirectory' --recursive
```

### <a name="update-a-file-share-with-changes-to-another-file-share"></a>Actualización de un recurso compartido de archivos con los cambios realizados en otro recurso compartido de archivos

El primer recurso compartido de archivos que aparece en este comando es el origen. El segundo es el destino.

**Sintaxis**

`azcopy sync 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive`

**Ejemplo**

```azcopy
azcopy sync 'https://mysourceaccount.file.core.windows.net/myfileShare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

### <a name="update-a-directory-with-changes-to-a-directory-in-another-file-share"></a>Actualización de un directorio con cambios en un directorio de otro recurso compartido de archivos

El primer directorio que aparece en este comando es el origen. El segundo es el destino.

**Sintaxis**

`azcopy sync 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name>/<directory-name><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name>/<directory-name><SAS-token>' --recursive`

**Ejemplo**

```azcopy
azcopy sync 'https://mysourceaccount.file.core.windows.net/myFileShare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/myFileShare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

### <a name="update-a-file-share-to-match-the-contents-of-a-share-snapshot"></a>Actualización de un recurso compartido de archivos para que coincida con el contenido de la instantánea de un recurso compartido

El primer recurso compartido de archivos que aparece en este comando es el origen. Al final del identificador URI, anexe la cadena `&sharesnapshot=`, seguida del valor **DateTime** de la instantánea.

**Sintaxis**

`azcopy sync 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>&sharesnapsot<snapshot-ID>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive`

**Ejemplo**

```azcopy
azcopy sync 'https://mysourceaccount.file.core.windows.net/myfileShare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D&sharesnapshot=2020-03-03T20%3A24%3A13.0000000Z' 'https://mydestinationaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive --preserve-smb-permissions=true --preserve-smb-info=true
```

Para más información sobre el uso compartido de instantáneas, consulte [Introducción a las instantáneas de recurso compartido de Azure Files](../files/storage-snapshots-files.md).

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar más ejemplos en cualquiera de estos artículos:

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Transferencia de datos](storage-use-azcopy-v10.md#transfer-data)

Consulte estos artículos para configurar opciones, optimizar el rendimiento y solucionar problemas:

- [Parámetros de configuración de AzCopy](storage-ref-azcopy-configuration-settings.md)
- [Optimización del rendimiento de AzCopy](storage-use-azcopy-optimize.md)
- [Solución de problemas de AzCopy v10 en Azure Storage mediante el uso de archivos de registro](storage-use-azcopy-configure.md)
