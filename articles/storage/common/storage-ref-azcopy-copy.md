---
title: azcopy copy | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy copy.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 09/01/2021
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 2d22e449a2e3c1ce7d026d4eed6ca9992e0ace14
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131460268"
---
# <a name="azcopy-copy"></a>azcopy copy

Copia los datos de origen en una ubicación de destino.

## <a name="synopsis"></a>Sinopsis

Copia los datos de origen en una ubicación de destino. Estas son las direcciones que se admiten:

  - local <-> blob de Azure (autenticación con SAS u OAuth)
  - local <-> Azure Files (autenticación de recursos compartidos o directorios con SAS)
  - local <-> Azure Data Lake Storage Gen 2 (SAS, OAuth o autenticación de clave compartida)
  - Azure Blob (SAS o público) -> Azure Blob (autenticación con SAS u OAuth)
  - Azure Blob (SAS o público) -> Azure Files (SAS)
  - Azure Files (SAS) -> Azure Files (SAS)
  - Azure Files (SAS) -> Azure Blob (autenticación con SAS u OAuth)
  - Amazon Web Services (AWS) S3 (clave de acceso) -> blob en bloques de Azure (autenticación con SAS u OAuth)
  - Google Cloud Storage (Clave de cuenta de servicio) -> Blob en bloques de Azure (autenticación SAS o OAuth) [versión preliminar]

Para más información, consulte la sección de ejemplos de este artículo.

## <a name="related-conceptual-articles"></a>Artículos conceptuales relacionados

- [Introducción a AzCopy](storage-use-azcopy-v10.md)
- [Tutorial: Migración de datos locales a un almacenamiento en la nube con AzCopy](storage-use-azcopy-migrate-on-premises-data.md)
- [Transferencia de datos con AzCopy y Blob Storage](./storage-use-azcopy-v10.md#transfer-data)
- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)

## <a name="advanced"></a>Avanzado

AzCopy detecta automáticamente el tipo de contenido de los archivos al cargarlos desde el disco local. AzCopy detecta el tipo de contenido en función de la extensión del archivo o su contenido (si no se especifica ninguna extensión).

La tabla de búsqueda integrada es pequeña pero, en Unix, los archivos `mime.types` del sistema local la pueden aumentar si están disponibles en una o varias de estas ubicaciones:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

En Windows, los tipos MIME se extraen del registro. Esta característica se puede desactivar con la ayuda de una marca. Para más información, consulte la sección de marcas de este artículo.

Si establece una variable de entorno mediante la línea de comandos, esa variable se podrá leer en el historial de la línea de comandos. Considere la posibilidad de borrar las variables que contengan credenciales del historial de la línea de comandos. Para evitar que aparezcan variables en el historial, puede usar un script para pedir al usuario sus credenciales y establecer la variable de entorno.

```
azcopy copy [source] [destination] [flags]
```

## <a name="examples"></a>Ejemplos

Carga de un solo archivo mediante la autenticación de OAuth. Si aún no ha iniciado sesión en AzCopy, ejecute el comando `azcopy login` antes de ejecutar el siguiente comando.

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Igual que antes, pero en esta ocasión también procesa el hash MD5 del contenido del archivo y lo guarda como la propiedad Content-MD5 del blob:

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5
```

Carga de un solo archivo mediante un token de SAS:

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Carga de un solo archivo mediante un token de SAS y una canalización (solo blobs en bloques):

```azcopy
cat "/path/to/file.txt" | azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]
```

Carga de un directorio completo mediante un token de SAS:

```azcopy
azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive
```

or

```azcopy
azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive --put-md5
```

Carga de un conjunto de archivos mediante un token de SAS y caracteres comodín (*):

```azcopy
azcopy cp "/path/*foo/*bar/*.pdf" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

Carga de archivos y directorios mediante un token de SAS y caracteres comodín (*):

```azcopy
azcopy cp "/path/*foo/*bar*" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive
```

Cargue archivos y directorios en la cuenta de Azure Storage y establezca las etiquetas codificadas en la cadena de consulta del blob.

- Para establecer las etiquetas {key = "bla bla", val = "foo"} y {key = "bla bla 2", val = "bar"}, use la sintaxis siguiente: `azcopy cp "/path/*foo/*bar*" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --blob-tags="bla%20bla=foo&bla%20bla%202=bar"`

- Las claves y los valores están codificados como dirección URL y los pares clave-valor se separan con una y comercial ("&")

- Al establecer etiquetas en los blobs, hay permisos adicionales ("t" para etiquetas) en SAS sin que el servicio devuelva un error de autorización.

Descarga de un solo archivo mediante la autenticación de OAuth. Si aún no ha iniciado sesión en AzCopy, ejecute el comando `azcopy login` antes de ejecutar el siguiente comando.

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]" "/path/to/file.txt"
```

Descarga de un solo archivo mediante un token de SAS:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "/path/to/file.txt"
```

Descarga de un solo archivo mediante un token de SAS y, luego, canalización de la salida a un archivo (solo blobs en bloques):

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" > "/path/to/file.txt"
```

Descarga de un directorio completo mediante un token de SAS:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "/path/to/dir" --recursive
```

Una nota sobre el uso de un carácter comodín (*) en direcciones URL:

Hay solo dos formas admitidas de usar un carácter comodín en una dirección URL.

- Puede usar uno justo después de la barra diagonal final (/) de una dirección URL. Este uso del carácter comodín copia todos los archivos de un directorio directamente en el destino sin colocarlos en un subdirectorio.

- También puede usar un carácter comodín en el nombre de un contenedor siempre y cuando la dirección URL solo haga referencia a un contenedor y no a un blob. Puede usar este enfoque para obtener archivos de un subconjunto de contenedores.

Descarga del contenido de un directorio sin copiar el propio directorio contenedor.

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/folder]/*?[SAS]" "/path/to/dir"
```

Descarga de una cuenta de almacenamiento completa.

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/" "/path/to/dir" --recursive
```

Descarga de un subconjunto de contenedores en una cuenta de almacenamiento mediante un símbolo comodín (*) en el nombre del contenedor.

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container*name]" "/path/to/dir" --recursive
```

Copia de un blob único en otro blob mediante un token de SAS.

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Copia de un blob único en otro blob mediante un token de SAS y un token de autenticación. Tiene que usar un token de SAS al final de la dirección URL de la cuenta de origen, pero la cuenta de destino no necesita uno si inicia sesión en AzCopy mediante el comando `azcopy login`.

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]"
```

Copia de un directorio virtual de blobs a otro mediante un token de SAS:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Copia de todos los contenedores de blobs, directorios y blobs de la cuenta de almacenamiento a otra mediante un token de SAS:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net?[SAS]" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive
```

Copia de un solo objeto en Blob Storage de Amazon Web Services (AWS) S3 mediante una clave de acceso y un token de SAS. En primer lugar, establezca las variables de entorno `AWS_ACCESS_KEY_ID` y `AWS_SECRET_ACCESS_KEY` para el origen de AWS S3.

```azcopy
azcopy cp "https://s3.amazonaws.com/[bucket]/[object]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Copia de un directorio completo en Blob Storage de AWS S3 mediante una clave de acceso y un token de SAS. En primer lugar, establezca las variables de entorno `AWS_ACCESS_KEY_ID` y `AWS_SECRET_ACCESS_KEY` para el origen de AWS S3.

```azcopy
azcopy cp "https://s3.amazonaws.com/[bucket]/[folder]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive
```

  Consulte https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html para comprender mejor el marcador de posición [folder].

Copia de todos los cubos en Blob Storage de Amazon Web Services (AWS) mediante el uso de una clave de acceso y un token de SAS. En primer lugar, establezca las variables de entorno `AWS_ACCESS_KEY_ID` y `AWS_SECRET_ACCESS_KEY` para el origen de AWS S3.

```azcopy
azcopy cp "https://s3.amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive
```

Copia de todos los cubos en Blob Storage desde una región de Amazon Web Services (AWS) mediante el uso de una clave de acceso y un token de SAS. En primer lugar, establezca las variables de entorno `AWS_ACCESS_KEY_ID` y `AWS_SECRET_ACCESS_KEY` para el origen de AWS S3.

```azcopy
- azcopy cp "https://s3-[region].amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive
```

Copia de un subconjunto de cubos mediante un símbolo comodín (*) en el nombre del cubo. Al igual que en los ejemplos anteriores, necesitará una clave de acceso y un token de SAS. Asegúrese de establecer las variables de entorno `AWS_ACCESS_KEY_ID` y `AWS_SECRET_ACCESS_KEY` para el origen de AWS S3.

```azcopy
- azcopy cp "https://s3.amazonaws.com/[bucket*name]/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive
```

Transfiera archivos y directorios a la cuenta de Azure Storage y establezca las etiquetas codificadas de la cadena de consulta determinadas en el blob.

- Para establecer las etiquetas {key = "bla bla", val = "foo"} y {key = "bla bla 2", val = "bar"}, use la sintaxis siguiente: `azcopy cp "https://[account].blob.core.windows.net/[source_container]/[path/to/directory]?[SAS]" "https://[account].blob.core.windows.net/[destination_container]/[path/to/directory]?[SAS]" --blob-tags="bla%20bla=foo&bla%20bla%202=bar"`

- Las claves y los valores están codificados como dirección URL y los pares clave-valor se separan con una y comercial ("&")

- Al establecer etiquetas en los blobs, hay permisos adicionales ("t" para etiquetas) en SAS sin que el servicio devuelva un error de autorización.

Copie un solo objeto en Blob Storage desde Google Cloud Storage mediante una clave de cuenta de servicio y un token de SAS. En primer lugar, establezca la variable de entorno GOOGLE_APPLICATION_CREDENTIALS para el origen de Google Cloud Storage.

```azcopy
azcopy cp "https://storage.cloud.google.com/[bucket]/[object]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Copie un directorio completo en Blob Storage desde Google Cloud Storage mediante una clave de cuenta de servicio y un token de SAS. En primer lugar, establezca la variable de entorno GOOGLE_APPLICATION_CREDENTIALS para el origen de Google Cloud Storage.

```azcopy
  - azcopy cp "https://storage.cloud.google.com/[bucket]/[folder]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Copie un cubo completo en Blob Storage desde Google Cloud Storage mediante una clave de cuenta de servicio y un token de SAS. En primer lugar, establezca la variable de entorno GOOGLE_APPLICATION_CREDENTIALS para el origen de Google Cloud Storage.

```azcopy 
azcopy cp "https://storage.cloud.google.com/[bucket]" "https://[destaccount].blob.core.windows.net/?[SAS]" --recursive=true
```

Copie todos los cubos en Blob Storage desde Google Cloud Storage mediante una clave de cuenta de servicio y un token de SAS. En primer lugar, establezca las variables de entorno GOOGLE_APPLICATION_CREDENTIALS y GOOGLE_CLOUD_PROJECT=<`project-id`> para el origen de GCS.

```azcopy
  - azcopy cp "https://storage.cloud.google.com/" "https://[destaccount].blob.core.windows.net/?[SAS]" --recursive=true
```

Copie un subconjunto de cubos mediante un símbolo comodín (*) en el nombre del depósito de Google Cloud Storage, con una clave de cuenta de servicio y un token de SAS para el destino. En primer lugar, establezca las GOOGLE_APPLICATION_CREDENTIALS y GOOGLE_CLOUD_PROJECT=<`project-id`> para el origen de Google Cloud Storage.

```azcopy
azcopy cp "https://storage.cloud.google.com/[bucket*name]/" "https://[destaccount].blob.core.windows.net/?[SAS]" --recursive=true
```

## <a name="options"></a>Opciones

**--backup** activa los privilegios SeBackupPrivilege de Windows para cargas, o SeRestorePrivilege para descargas, para que AzCopy pueda ver y leer todos los archivos, independientemente de los permisos del sistema de archivos, y restaurar todos los permisos. Requiere que la cuenta que ejecuta AzCopy ya tenga estos permisos (por ejemplo, que tenga derechos de administrador o que sea miembro del grupo `Backup Operators`). Esta marca activa los privilegios que ya tiene la cuenta.

Cadena **--blob-tags**: establece etiquetas en blobs para clasificar los datos de la cuenta de almacenamiento.

**--blob-type** string. Define el tipo de blob del destino. Se usa para cargar blobs y al copiar entre cuentas (el valor predeterminado es `Detect`). Los valores válidos son `Detect`, `BlockBlob`, `PageBlob` y `AppendBlob`. Al copiar entre cuentas, un valor `Detect` hace que AzCopy use el tipo de blob de origen para determinar el tipo del blob de destino. Al cargar un archivo, `Detect` determina si el archivo es un VHD o VHDX en función de la extensión del archivo. Si el archivo es VHD o VHDX, AzCopy lo trata como un blob en páginas. (el valor predeterminado es "Detect")

**--block-blob-tier** string Carga un blob en bloques en Azure Storage con este nivel de blobs. (el valor predeterminado es "None")

**--block-size-mb** float Usa este tamaño de bloque (especificado en MiB) al cargar y descargar de Azure Storage. El valor predeterminado se calcula automáticamente en función del tamaño del archivo. Se permiten fracciones decimales (por ejemplo: 0,25).

**--cache-control** string Establece el encabezado cache-control. Devuelto al descargar.

**--check-length** Comprueba la longitud de un archivo en el destino después de la transferencia. Si hay una discrepancia entre el origen y el destino, la transferencia se marca como errónea. (El valor predeterminado es `true`)

**--check-md5** string Especifica qué tan estrictamente se deben validar los hashes MD5 al descargarse. Solo está disponible al descargar. Opciones disponibles: `NoCheck`, `LogOnly`, `FailIfDifferent`, `FailIfDifferentOrMissing`. (el valor predeterminado es `FailIfDifferent`)

**--content-disposition** string Establece el encabezado content-disposition. Devuelto al descargar.

**--content-encoding** string Establece el encabezado content-encoding. Devuelto al descargar.

**--content-language** string Establece el encabezado content-language. Devuelto al descargar.

**--content-type** string Especifica el tipo de contenido del archivo. Implica el uso de no-guess-mime-type. Devuelto al descargar.

**--cpk-by-name** string La clave proporcionada por el cliente por nombre permite a los clientes que realizan solicitudes en Azure Blob Storage una opción para proporcionar una clave de cifrado por solicitud. El nombre de clave proporcionado se capturará de Azure Key Vault y se usará para cifrar los datos.

**--cpk-by-value** La clave proporcionada por el cliente por nombre permite a los clientes que realizan solicitudes en Azure Blob Storage una opción para proporcionar una clave de cifrado por solicitud. La clave proporcionada y su hash se capturarán de las variables de entorno.

**--decompress** Descomprime automáticamente los archivos al realizar la descarga, si el encabezado content-encoding indica que están comprimidos. Los valores de content-encoding admitidos son `gzip` y `deflate`. Las extensiones de archivo de `.gz`,/`.gzip` o `.zz` no son necesarias, pero se quitarán si existen.

**--dry-run** Imprime las rutas de archivo que copiará este comando. Esta marca no copia los archivos reales.

**--disable-auto-decoding** En "False" (desactivado) de manera predeterminada para habilitar la decodificación automática de caracteres no admitidos en Windows. Se puede establecer en `true` para deshabilitar la decodificación automática.

**--exclude-attributes** string (solo Windows) Excluye los archivos cuyos atributos coinciden con la lista de atributos. Por ejemplo: A;S;R

**--exclude-blob-type** string Opcionalmente, especifica el tipo de blob (`BlockBlob`/ `PageBlob`/ `AppendBlob`) que se va a excluir al copiar blobs desde el contenedor o la cuenta. El uso de esta marca no es aplicable para copiar datos desde un servicio que no es de Azure a otro que sí lo es. Si hay más de un blob deberían separarse mediante `;`.

**--exclude-path** string Excluye estas rutas de acceso al copiar. Esta opción no permite caracteres comodín (*). Comprueba el prefijo de ruta de acceso relativa (por ejemplo: `myFolder;myFolder/subDirName/file.pdf`). Cuando se usa en combinación con recorrido de cuentas, las rutas de acceso no incluyen el nombre del contenedor.

**--exclude-pattern** string Excluye estos archivos durante el copiado. Esta opción admite caracteres comodín (*).

**--exclude-regex** string Se excluye toda la ruta de acceso relativa de los archivos que se alinean con expresiones regulares. Separe las expresiones regulares con ";".

**--follow-symlinks** Sigue los vínculos simbólicos al cargar desde el sistema de archivos local.

**--force-if-read-only** Al sobrescribir un archivo existente en Windows o Azure Files, fuerza la sobrescritura para que funcione incluso si el archivo existente tiene establecido el atributo de solo lectura.

**--from-to** string Opcionalmente, especifica la combinación de destino de origen. Por ejemplo: `LocalBlob`, `BlobLocal` y `LocalBlobFS`.

**--help** Ayuda de copy.

**--include-after** string Solo incluye los archivos modificados durante o después de la fecha y hora especificadas. El valor debe estar en el formato ISO8601. Si no se especifica ninguna zona horaria, se supone que el valor se encontrará en la zona horaria local de la máquina que ejecuta AzCopy. Por ejemplo, `2020-08-19T15:04:00Z` para una hora en UTC o `2020-08-19` para la medianoche (00:00) en la zona horaria local. Como en AzCopy 10.5, esta marca solo se aplica a archivos, no a carpetas, por lo que las propiedades de carpeta no se copiarán cuando use esta marca con `--preserve-smb-info` o `--preserve-permissions`.

 Cadena **--include-before** Solo incluye los archivos modificados antes o en la fecha y hora especificadas. El valor debe estar en el formato ISO8601. Si no se especifica ninguna zona horaria, se supone que el valor se encontrará en la zona horaria local de la máquina que ejecuta AzCopy. Por ejemplo, `2020-08-19T15:04:00Z` para una hora en UTC o `2020-08-19` para la medianoche (00:00) en la zona horaria local. A partir de AzCopy 10.7, esta marca solo se aplica a archivos, no a carpetas, por lo que las propiedades de carpeta no se copiarán cuando use esta marca con `--preserve-smb-info` o `--preserve-permissions`.

**--include-attributes** string (solo Windows) Incluye los archivos cuyos atributos coinciden con la lista de atributos. Por ejemplo: A;S;R

**--include-path** string Incluye solo estas rutas de acceso al copiar. Esta opción no permite caracteres comodín (*). Comprueba el prefijo de ruta de acceso relativa (por ejemplo: `myFolder;myFolder/subDirName/file.pdf`).

**--include-directory-stub** False de forma predeterminada para omitir los códigos auxiliares de directorio. Los códigos auxiliares de directorio son blobs con metadatos "hdi_isfolder:true". Si se establece el valor en true, se conservarán los códigos auxiliares de directorio durante las transferencias.

**--include-pattern** string Incluye solo estos archivos al copiar. Esta opción admite caracteres comodín (*). Separe los archivos con `;`.

**--include-regex** string Se incluye solo la ruta de acceso relativa de los archivos que se alinean con expresiones regulares. Separe las expresiones regulares con ";".

Cadena **--list-of-versions** Especifica un archivo en el que cada id. de versión aparece en una línea independiente. Asegúrese de que el origen apunte a un único blob y que todos los id. de versión especificados en el archivo con esta marca solo pertenezcan al blob de origen. AzCopy descargará las versiones especificadas en la carpeta de destino proporcionada. Para obtener más información, consulte [Descarga de versiones anteriores de un blob](./storage-use-azcopy-v10.md#transfer-data).

**--log-level** string Define el nivel de detalle para el archivo de registro; los niveles disponibles son: INFO (todas las solicitudes y respuestas), WARNING (respuestas lentas), ERROR (solo solicitudes con error) y NONE (sin registros de salida). (El valor predeterminado es `INFO`).

**--metadata** string Carga en Azure Storage con estos pares clave-valor como metadatos.

**--no-guess-mime-type** Impide que AzCopy detecte el tipo de contenido en función de la extensión o el contenido del archivo.

**--overwrite** string Sobrescribe los archivos y blobs en conflicto en el destino si esta marca está establecida en true. (El valor predeterminado es `true`). Los valores posibles incluyen `true`, `false`, `prompt` y `ifSourceNewer`. En el caso de los destinos que admiten carpetas, se sobrescribirán las propiedades de nivel de carpeta en conflicto si esta marca es `true` o si se proporciona una respuesta positiva al aviso. (el valor predeterminado es "true")

**--page-blob-tier** string Carga un blob en páginas en Azure Storage con este nivel de blobs. (El valor predeterminado es `None`). (el valor predeterminado es "None")

**--preserve-last-modified-time** Solo está disponible si el destino es un sistema de archivos.

**--preserve-owner** Solo tiene efecto en las descargas y solo cuando se usa `--preserve-permissions`. Si es true (valor predeterminado), el propietario del archivo y el grupo se conservan en las descargas. Si se establece en false, `--preserve-permissions` conservará las listas de control de acceso, pero el propietario y el grupo se basarán en el usuario que ejecuta AzCopy (el valor predeterminado es true).

**--preserve-smb-info** True de manera predeterminada. Conserva la información de las propiedades de SMB (hora de la última escritura, hora de creación, bits de atributo) entre los recursos compatibles con SMB (Windows y Azure Files). Solo se transferirán los bits de atributo que admite Azure Files; los restantes se omitirán. Esta marca se aplica tanto a archivos como a carpetas, salvo que se especifique un filtro que solo permita archivos (por ejemplo, include-pattern). La información transferida a las carpetas es la misma que la transferida a los archivos, excepto la hora de la última escritura, que nunca se conserva para las carpetas.

**--preserve-permissions** False de manera predeterminada. Conserva las ACL entre los recursos compatibles (Windows y Azure Files, o bien de Data Lake Storage Gen 2 a Data Lake Storage Gen 2). Para las cuentas que tienen un espacio de nombres jerárquico, necesitará un token de SAS o OAuth de contenedor con permisos Modificar propiedad y Modificar permisos. En el caso de las descargas, también necesitará la marca --backup para restaurar los permisos si el nuevo propietario no será el usuario que ejecute AzCopy. Esta marca se aplica tanto a archivos como a carpetas, salvo que se especifique un filtro de que solo permita archivos (por ejemplo, include-pattern).

**--put-md5** Crea un hash MD5 de cada archivo y lo guarda como la propiedad Content-MD5 del blob o archivo de destino. (De forma predeterminada, NO se crea el hash). Solo está disponible al cargar.

**--recursive** Busca en subdirectorios de forma recursiva al cargar desde el sistema de archivos local.

**--s2s-detect-source-changed** Detecta si el blob o archivo de origen cambia mientras se está leyendo. (Este parámetro solo se aplica a las copias entre servicios, ya que la comprobación correspondiente está habilitada permanentemente para las cargas y descargas).

**--s2s-handle-invalid-metadata** string   Especifica cómo se administran las claves de metadatos no válidas. Opciones disponibles: ExcludeIfInvalid, FailIfInvalid, RenameIfInvalid. (El valor predeterminado es `ExcludeIfInvalid`).

**--s2s-preserve-access-tier** Conserva el nivel de acceso durante la copia de servicio a servicio. Consulte [Niveles de acceso frecuente, esporádico y de archivo de los datos de blob](../blobs/access-tiers-overview.md) para asegurarse de que la cuenta de almacenamiento de destino admite la configuración del nivel de acceso. En los casos en los que no se admite la configuración del nivel de acceso, use s2sPreserveAccessTier=false para omitir la copia del nivel de acceso. (El valor predeterminado es `true`).

**--s2s-preserve-blob-tags** Se conservan las etiquetas de índice durante la transferencia entre servicios de un almacenamiento de blobs a otro.

**--s2s-preserve-properties** Conserva las propiedades completas durante la copia de servicio a servicio. Para el origen de archivo no único de AWS S3 y Azure Files, la operación de lista no devuelve las propiedades completas de objetos y archivos. Para conservar las propiedades completas, AzCopy debe enviar una solicitud adicional por objeto o archivo. (El valor predeterminado es true)

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

**--cap-mbps float** Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.

**--output-type** string   Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es `text`. (Valor predeterminado: "text").

La cadena **--trusted-microsoft-suffixes**   Especifica sufijos de dominio adicionales en los que se pueden enviar tokens de inicio de sesión de Azure Active Directory.  El valor predeterminado es `*.core.windows.net;*.core.chinacloudapi.cn;*.core.cloudapi.de;*.core.usgovcloudapi.net`. Los valores que se muestran aquí se agregan al valor predeterminado. Por seguridad, solo debe poner aquí dominios de Microsoft Azure. Separe las entradas con punto y coma.

## <a name="see-also"></a>Consulte también

- [azcopy](storage-ref-azcopy.md)
