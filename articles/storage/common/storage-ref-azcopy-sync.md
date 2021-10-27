---
title: azcopy sync | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy sync.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 09/01/2021
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: b1a9f54febcd2ebf36287590cb0579f8d2dea804
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2021
ms.locfileid: "130166361"
---
# <a name="azcopy-sync"></a>azcopy sync

Replica la ubicación de origen en la ubicación de destino. En este artículo se proporciona una referencia detallada del comando azcopy sync. Para más información sobre la sincronización de blobs entre las ubicaciones de origen y destino, consulte [Sincronización con Azure Blob Storage mediante AzCopy v10](storage-use-azcopy-blobs-synchronize.md). Para Azure Files, consulte [Sincronización de archivos](storage-use-azcopy-files.md#synchronize-files).

## <a name="synopsis"></a>Sinopsis

Las horas de última modificación se utilizan con fines de comparación. El archivo se omite si la hora de la última modificación del destino es más reciente.

Los pares admitidos son:

- local <-> blob de Azure (se puede emplear la autenticación con SAS u OAuth)
- Azure Blob <-> Azure Blob (el origen debe incluir SAS o ser de acceso público; se puede emplear la autenticación con SAS u OAuth para el destino).
- Azure File <-> Azure File (el origen debe incluir SAS o ser de acceso público; se debe usar la autenticación con SAS para el destino).

El comando sync difiere del comando copy de varias maneras:

1. De manera predeterminada, la marca recursiva es true y sync copia todos los subdirectorios. Sync solo copia los archivos de nivel superior dentro de un directorio si la marca recursiva es false.
2. Al sincronizar entre directorios virtuales, agregue una barra diagonal final a la ruta de acceso (consulte los ejemplos) si hay un blob con el mismo nombre que uno de los directorios virtuales.
3. Si la marca `deleteDestination` está establecida en true o prompt, sync eliminará los archivos y blobs del destino que no estén presentes en el origen.

## <a name="related-conceptual-articles"></a>Artículos conceptuales relacionados

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Tutorial: Migración de datos locales a un almacenamiento en la nube con AzCopy](storage-use-azcopy-migrate-on-premises-data.md)
- [Transferencia de datos con AzCopy y Blob Storage](./storage-use-azcopy-v10.md#transfer-data)
- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)

### <a name="advanced"></a>Avanzado

Si no especifica una extensión de archivo, AzCopy detecta automáticamente el tipo de contenido de los archivos cuando se cargan desde el disco local, basándose en la extensión del archivo o en el contenido (si no se especifica ninguna extensión).

La tabla de búsqueda integrada es pequeña pero, en Unix, los archivos mime.types del sistema local la pueden aumentar si están disponibles en una o varias de estas ubicaciones:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

En Windows, los tipos MIME se extraen del registro.

```azcopy
azcopy sync <source> <destination> [flags]
```

## <a name="examples"></a>Ejemplos

Sincronización de un solo archivo:

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Igual que antes, pero también procesa un hash MD5 del contenido del archivo y lo guarda como la propiedad Content-MD5 del blob.

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5
```

Sincronización de un directorio completo incluyendo los subdirectorios (tenga en cuenta que la recursividad está activada de forma predeterminada):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]"
```

or

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --put-md5
```

Sincronización solo de los archivos de un directorio, pero no de los subdirectorios ni de los archivos de estos:

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Sincronización de un subconjunto de archivos de un directorio (por ejemplo, solo los archivos jpg y pdf, o un archivo llamado `exactName`):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --include-pattern="*.jpg;*.pdf;exactName"
```

Sincronización de un directorio completo, pero con la exclusión de ciertos archivos del ámbito (por ejemplo: todos los archivos que comienzan por "foo" o terminan por "bar"):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --exclude-pattern="foo*;*bar"
```

Sincronización de un solo blob:

```azcopy
azcopy sync "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Sincronización de un directorio virtual:

```azcopy
azcopy sync "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]?[SAS]" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=true
```

Sincronización de un directorio virtual que tenga el mismo nombre que un blob (agregue una barra diagonal final a la ruta de acceso para eliminar la ambigüedad):

```azcopy
azcopy sync "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]/?[SAS]" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]/" --recursive=true
```

Sincronización de un directorio de Azure File:

```azcopy
azcopy sync "https://[account].file.core.windows.net/[share]/[path/to/dir]?[SAS]" "https://[account].file.core.windows.net/[share]/[path/to/dir]?[SAS]" --recursive=true
```

> [!NOTE]
> Si se usan juntas las marcas de inclusión y exclusión, solo los archivos que coincidan con los patrones de inclusión se examinarán, pero los que coincidan con los patrones de exclusión se ignorarán siempre.

## <a name="options"></a>Opciones

**--block-size-mb** float: usa este tamaño de bloque (especificado en MiB) al cargar o descargar de Azure Storage. El valor predeterminado se calcula automáticamente en función del tamaño del archivo. Se permiten fracciones decimales (por ejemplo: `0.25`).

**--check-md5** string: especifica cómo de estrictamente se deben validar los hashes MD5 al descargarse. Esta opción solo está disponible al descargar. Entre los valores disponibles se incluyen: `NoCheck`, `LogOnly`, `FailIfDifferent` y `FailIfDifferentOrMissing`. (El valor predeterminado es `FailIfDifferent`). (`FailIfDifferent` por defecto)

**--cpk-by-name** string La clave proporcionada por el cliente por nombre permite a los clientes que realizan solicitudes en Azure Blob Storage una opción para proporcionar una clave de cifrado por solicitud. El nombre de clave proporcionado se capturará de Azure Key Vault y se usará para cifrar los datos

**--cpk-by-value** La clave proporcionada por el cliente por nombre permite a los clientes que realizan solicitudes en Azure Blob Storage una opción para proporcionar una clave de cifrado por solicitud. La clave proporcionada y su hash se capturarán de las variables de entorno

**--delete-destination** string   Define si se eliminan los archivos adicionales del destino que no están presentes en el origen. Se puede establecer en `true`, `false` o `prompt`. Si se establece en `prompt`, se le formulará al usuario una pregunta antes de programar archivos y blobs para su eliminación. (El valor predeterminado es `false`). (`false` por defecto)

**--dry-run** Imprime la ruta de los archivos que el comando sync copiará o quitará. Esta marca no copia ni quita los archivos reales.

**--exclude-attributes** string (solo Windows): excluye los archivos cuyos atributos coinciden con la lista de atributos. Por ejemplo: `A;S;R`

**--exclude-path** string: excluye estas rutas de acceso al comparar el origen con el destino. Esta opción no permite caracteres comodín (*). Comprueba el prefijo de ruta de acceso relativa (por ejemplo: `myFolder;myFolder/subDirName/file.pdf`).

**--exclude-pattern** string   Excluye los archivos en los que el nombre coincide con la lista de patrones. Por ejemplo: `*.jpg;*.pdf;exactName`

**--exclude-regex** string Se excluye la ruta de acceso relativa de los archivos que coinciden con expresiones regulares. Separe las expresiones regulares con ";".

**--help**: ayuda para la sincronización.

**--include-attributes** string (solo Windows): incluye solo los archivos cuyos atributos coinciden con la lista de atributos. Por ejemplo: `A;S;R`

**--include-pattern** string   Incluye solo los archivos en los que el nombre coincide con la lista de patrones. Por ejemplo: `*.jpg;*.pdf;exactName`

**--log-level** string: define el nivel de detalle del registro para el archivo de registro, niveles disponibles: `INFO`(todas las solicitudes y respuestas), `WARNING`(respuestas lentas), `ERROR`(solo solicitudes con errores) y `NONE`(sin registros de salida). (El valor predeterminado es `INFO`).

**--mirror mode** Deshabilita la comparación basada en la hora de última modificación y sobrescribe los archivos y blobs en conflicto en el destino si esta marca está establecida en `true`. El valor predeterminado es `false`.

**--preserve-smb-info** True de manera predeterminada. Conserva la información de las propiedades de SMB (hora de la última escritura, hora de creación, bits de atributo) entre los recursos compatibles con SMB (Windows y Azure Files). Esta marca se aplica tanto a archivos como a carpetas, salvo que se especifique un filtro que solo permita archivos (por ejemplo, include-pattern).  La información transferida a las carpetas es la misma que la transferida a los archivos, excepto la hora de la última escritura, que no se conserva para las carpetas.

**--preserve-permissions** False de manera predeterminada. Conserva las ACL entre los recursos compatibles (Windows y Azure Files, o bien de Data Lake Storage Gen 2 a Data Lake Storage Gen 2). Para las cuentas que tienen un espacio de nombres jerárquico, necesitará un token de SAS o OAuth de contenedor con permisos Modificar propiedad y Modificar permisos. En el caso de las descargas, también necesitará la marca --backup para restaurar los permisos si el nuevo propietario no será el usuario que ejecute AzCopy. Esta marca se aplica tanto a archivos como a carpetas, salvo que se especifique un filtro de que solo permita archivos (por ejemplo, include-pattern).

**--put-md5**: crea un hash MD5 de cada archivo y lo guarda como la propiedad Content-MD5 del blob o archivo de destino. (De forma predeterminada, NO se crea el hash). Solo está disponible al cargar.

**--recursive**    `True` de forma predeterminada, busca en los subdirectorios de forma recursiva al sincronizar entre directorios. (El valor predeterminado es `True`).

**--s2s-preserve-access-tier**: conserva el nivel de acceso durante la copia de servicio a servicio. Consulte [Niveles de acceso frecuente, esporádico y de archivo de los datos de blob](../blobs/access-tiers-overview.md) para asegurarse de que la cuenta de almacenamiento de destino admite la configuración del nivel de acceso. En los casos en los que no se admite el establecimiento del nivel de acceso, use `--s2s-preserve-access-tier=false` para omitir la copia del nivel de acceso. (El valor predeterminado es `true`).

**--s2s-preserve-blob-tags** Se conservan las etiquetas de índice durante la sincronización entre servicios de un almacenamiento de blobs a otro.

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|Descripción|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|
|--trusted-microsoft-suffixes string   |Especifica sufijos de dominio adicionales en los que se pueden enviar tokens de inicio de sesión de Azure Active Directory.  El valor predeterminado es " *.core.windows.net;* .core.chinacloudapi.cn; *.core.cloudapi.de;* .core.usgovcloudapi.net". Los valores que se muestran aquí se agregan al valor predeterminado. Por seguridad, solo debe poner aquí dominios de Microsoft Azure. Separe las entradas con punto y coma.|

## <a name="see-also"></a>Consulte también

- [azcopy](storage-ref-azcopy.md)
