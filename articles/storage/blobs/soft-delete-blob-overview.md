---
title: Eliminación temporal para blobs
titleSuffix: Azure Storage
description: La eliminación temporal para blobs protege los datos para que pueda recuperarlos más fácilmente si una aplicación u otro usuario de la cuenta de almacenamiento los modifican o eliminan por error.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 07/23/2021
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 1e40e8cb31019fa8a9dd35f41dc677225d613180
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132720594"
---
# <a name="soft-delete-for-blobs"></a>Eliminación temporal para blobs

La eliminación temporal de blobs protege a cada uno de los blobs, instantáneas o versiones de errores accidentales al borrar o sobrescribir los datos, ya que conserva en el sistema los datos eliminados durante el período de tiempo que se especifique. Durante el período de retención, puede restaurar un objeto eliminado temporalmente a su estado en el momento en que se eliminó. Una vez vencido el período de retención especificado, el objeto se elimina permanentemente.

> [!IMPORTANT]
> La eliminación temporal en las cuentas que tienen habilitada la característica de espacio de nombres jerárquico está actualmente en versión preliminar y está disponible globalmente en todas las regiones de Azure.
> Consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.
>
>
> Para inscribirse en la versión preliminar, visite [este formulario](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR4mEEwKhLjlBjU3ziDwLH-pUOVRVOUpDRUtHVUtDUUtMVTZUR0tUMjZWNy4u).

## <a name="recommended-data-protection-configuration"></a>Configuración de protección de datos recomendada

La eliminación temporal de blobs forma parte de una exhaustiva estrategia para proteger los datos de los blobs. Para una protección óptima de los datos de blobs, Microsoft recomienda habilitar las siguientes características de protección de datos:

- Eliminación temporal de contenedores, para restaurar un contenedor que se ha eliminado. Para obtener información sobre cómo habilitar la eliminación temporal de contenedores, consulte [Habilitación y administración de la eliminación temporal de contenedores](soft-delete-container-enable.md).
- Control de versiones de blobs, para conservar automáticamente las versiones anteriores de un blob. Cuando el control de versiones de blobs está habilitado, puede restaurar una versión anterior de un blob para recuperar los datos si se modifican o eliminan por error. Para obtener información sobre cómo habilitar el control de versiones de blobs, consulte [Habilitación y administración del control de versiones de blobs](versioning-enable.md).
- Eliminación temporal de blobs, para restaurar un blob, una instantánea o una versión que se ha eliminado. Para obtener información sobre cómo habilitar la eliminación temporal de blobs, consulte [Habilitación y administración de la eliminación temporal para blobs](soft-delete-blob-enable.md).

Si necesita más información sobre las recomendaciones de Microsoft para proteger los datos, consulte [Información general sobre la protección de datos](data-protection-overview.md).

## <a name="how-blob-soft-delete-works"></a>Funcionamiento de la eliminación temporal de blobs

Al habilitar la eliminación temporal de blobs para una cuenta de almacenamiento, se especifica un período de retención para los objetos eliminados de entre 1 y 365 días. El período de retención indica cuánto tiempo permanecen disponibles los datos después de que se eliminen o se sobrescriban. El reloj del período de retención comienza en cuanto se elimina o se sobrescribe un objeto.

Mientras el período de retención está activo, puede restaurar un blob eliminado, junto con sus instantáneas, o una versión eliminada, mediante una llamada a la operación [Undelete Blob](/rest/api/storageservices/undelete-blob). En el diagrama siguiente se muestra cómo se puede restaurar un objeto eliminado cuando está habilitada la eliminación temporal de blobs:

:::image type="content" source="media/soft-delete-blob-overview/blob-soft-delete-diagram.png" alt-text="Diagrama que muestra cómo se puede restaurar un blob eliminado temporalmente":::

El período de retención de la eliminación temporal se puede cambiar en cualquier momento. Un período de retención actualizado solo se aplica a los datos que se eliminaron después de cambiar el período de retención. Los datos que se eliminaron antes de que se cambiara el período de retención están sujetos al período de retención que estaba en vigor cuando se eliminaron.

Si intenta eliminar un objeto eliminado temporalmente, su hora de expiración no se verá afectada.

Si deshabilita la eliminación temporal de blobs, puede seguir teniendo acceso a los objetos eliminados temporalmente y recuperarlos en la cuenta de almacenamiento hasta que haya transcurrido el período de retención de eliminación temporal.

El control de versiones de blobs está disponible para las cuentas de uso general V2, blob en bloques y almacenamiento de blobs. Las cuentas de almacenamiento con un espacio de nombres jerárquico no se admiten actualmente.

La versión 2017-07-29 y las versiones posteriores de la API REST de Azure Storage admiten la eliminación temporal de blobs.

> [!IMPORTANT]
> Solo puede usar la eliminación temporal de blobs para restaurar un blob, una instantánea, un directorio (en un espacio de nombres jerárquico) o una versión individuales. Para restaurar un contenedor y su contenido, la eliminación temporal de contenedores también debe estar habilitada para la cuenta de almacenamiento. Microsoft recomienda habilitar la eliminación temporal de contenedores y el control de versiones de blobs junto con la eliminación temporal de blobs para asegurar la protección completa de los datos de blob. Para obtener más información,consulte [Información general sobre la protección de datos](data-protection-overview.md).
>
> La eliminación temporal de blobs no protege contra la eliminación de una cuenta de almacenamiento. Para proteger una cuenta de almacenamiento de la eliminación, configure un bloqueo en el recurso de la cuenta de almacenamiento. Para más información sobre el bloqueo de una cuenta de almacenamiento, consulte [Aplicación de un bloqueo de Azure Resource Manager a una cuenta de almacenamiento](../common/lock-account-resource.md).

### <a name="how-deletions-are-handled-when-soft-delete-is-enabled"></a>Administración de las eliminaciones cuando la eliminación temporal está habilitada

Cuando la eliminación temporal de blobs está habilitada, al eliminar un blob, se marca como eliminado temporalmente. No se crea ninguna instantánea. Cuando expire el período de retención, el blob eliminado temporalmente se eliminará de forma permanente.

No se puede eliminar un blob con instantáneas a menos que las instantáneas también se eliminen. Cuando se elimina un blob y sus instantáneas, se marcan como eliminados temporalmente. No se crean instantáneas nuevas.

También puede eliminar una o varias instantáneas activas sin eliminar el blob base. En este caso, la instantánea se elimina temporalmente.

Si se elimina un directorio en una cuenta que tiene habilitada la característica de espacio de nombres jerárquico, el directorio y todo su contenido se marcan como eliminados temporalmente.

Los objetos eliminados temporalmente son invisibles a menos que se muestren o enumeren explícitamente. Para obtener más información sobre cómo enumerar objetos eliminados temporalmente, consulte [Administración y restauración de blobs eliminados temporalmente](soft-delete-blob-manage.md).

### <a name="how-overwrites-are-handled-when-soft-delete-is-enabled"></a>Administración de las sobrescrituras cuando la eliminación temporal está habilitada

> [!IMPORTANT]
> Esta sección no se aplica a las cuentas que tienen un espacio de nombres jerárquico.

La llamada a una operación como [Put Blob](/rest/api/storageservices/put-blob), [Put Block List](/rest/api/storageservices/put-block-list)o [Copy Blob](/rest/api/storageservices/copy-blob) sobrescribe los datos en un blob. Cuando la eliminación temporal de blobs está habilitada, al sobrescribir un blob se crea automáticamente una instantánea de eliminación temporal del estado del blob antes de la operación de escritura. Cuando expire el período de retención, la instantánea eliminada temporalmente se eliminará de forma permanente.

Las instantáneas eliminadas temporalmente son invisibles a menos que los objetos eliminados temporalmente se muestren o enumeren explícitamente. Para obtener más información sobre cómo enumerar objetos eliminados temporalmente, consulte [Administración y restauración de blobs eliminados temporalmente](soft-delete-blob-manage.md).

Para proteger una operación de copia, se debe habilitar la eliminación temporal de blobs para la cuenta de almacenamiento de destino.

La eliminación temporal de blobs no protege contra las operaciones para escribir metadatos o propiedades de blob. Cuando se actualizan los metadatos o las propiedades de un blob, no se crea ninguna instantánea eliminada temporalmente.

La eliminación temporal de blobs no permite la protección contra sobrescritura de blobs en el nivel de archivo. Si un blob en el nivel de archivo se sobrescribe con un blob nuevo en cualquier nivel, el primero se elimina de forma permanente.

En el caso de las cuentas de Premium Storage, las instantáneas eliminadas temporalmente no cuentan para el límite de 100 instantáneas por blob.

### <a name="restoring-soft-deleted-objects"></a>Restauración de objetos eliminados temporalmente

Los blobs o los directorios (en un espacio de nombres jerárquico) eliminados temporalmente se pueden restaurar dentro del período de retención mediante una llamada a la operación [Recuperar Blob](/rest/api/storageservices/undelete-blob). La operación **Undelete Blob** restaura un blob y las instantáneas eliminadas temporalmente que tiene asociadas. Se restauran todas las instantáneas que se eliminaron durante el período de retención.

En las cuentas que tienen un espacio de nombres jerárquico, la operación **Recuperar blob** también se puede usar para restaurar un directorio eliminado temporalmente y todo su contenido. Si cambia el nombre de un directorio que contiene blobs eliminados temporalmente, esos blobs eliminados temporalmente se desconectan del directorio. Si desea restaurar esos blobs, tendrá que revertir el nombre del directorio al nombre original o crear un directorio independiente que use el nombre del directorio original. De lo contrario, recibirá un error al intentar restaurar esos blobs eliminados temporalmente.

La llamada a **Undelete Blob** en un blob que no está eliminado de forma temporal restaurará las instantáneas eliminadas temporalmente que estén asociadas al blob. Si el blob no tiene instantáneas y no se elimina temporalmente, la llamada a **Undelete Blob** no tiene ningún efecto.

Para promover una instantánea eliminada temporalmente en el blob de base, primero llame a **Undelete Blob** en el blob base para restaurar el blob y sus instantáneas. A continuación, copie la instantánea deseada en el blob base. También puede copiar la instantánea en un blob nuevo.

No se pueden leer los datos de un blob o una instantánea eliminados temporalmente hasta que se haya restaurado el objeto.

Para obtener más información sobre cómo restaurar objetos eliminados temporalmente, consulte [Administración y restauración de blobs eliminados temporalmente](soft-delete-blob-manage.md).

## <a name="blob-soft-delete-and-versioning"></a>Eliminación temporal y control de versiones de blobs

> [!IMPORTANT]
> No se admite el control de versiones para cuentas que tienen un espacio de nombres jerárquico.

Si están habilitados el control de versiones y la eliminación temporal de blobs para una cuenta de almacenamiento, al sobrescribir un blob se crea una versión automáticamente. La nueva versión no se elimina de forma temporal y no se quita cuando expira el período de retención de eliminación temporal. No se crea ninguna instantánea eliminada temporalmente. Cuando elimina un blob, la versión actual se convierte en otra anterior y deja de haber una versión actual. No se crea ninguna versión nueva ni ninguna instantánea eliminada temporalmente.

La habilitación de la eliminación temporal y el control de versiones protege las versiones de blobs contra la eliminación. Cuando la eliminación temporal está habilitada, al eliminar una versión se crea una versión eliminada temporalmente. Puede usar la operación **Undelete Blob** para restaurar versiones eliminadas temporalmente durante el período de retención de la eliminación. La operación **Undelete Blob** siempre restaura todas las versiones eliminadas temporalmente del blob. No es posible restaurar una sola versión eliminada temporalmente.

Una vez transcurrido el período de retención de eliminación temporal, cualquier versión del blob eliminada temporalmente se eliminará de forma permanente.

> [!NOTE]
> La llamada a la operación **Undelete Blob** en un blob eliminado cuando el control de versiones está habilitado restaura las versiones o instantáneas eliminadas temporalmente, pero no restaura la versión actual. Para restaurar la versión actual, promueva una versión anterior al copiarla en la versión actual.

Microsoft recomienda habilitar el control de versiones y la eliminación temporal de blobs para las cuentas de almacenamiento con el fin de lograr una protección de datos óptima. Para obtener más información sobre el uso conjunto del control de versiones de blobs y la eliminación temporal, vea [Control de versiones de blobs y eliminación temporal](versioning-overview.md#blob-versioning-and-soft-delete).

## <a name="blob-soft-delete-protection-by-operation"></a>Protección de eliminación temporal de blobs por operación

En la tabla siguiente, se describe el comportamiento esperado de las operaciones de eliminación y escritura cuando la eliminación temporal de blobs está habilitada, ya sea con o sin control de versiones de blobs.

### <a name="storage-account-no-hierarchical-namespace"></a>Cuenta de almacenamiento (sin espacio de nombres jerárquico)

| Operaciones de API REST | Eliminación temporal habilitada | Eliminación temporal y control de versiones habilitadas |
|--|--|--|
| [Eliminación de la cuenta de almacenamiento](/rest/api/storagerp/storageaccounts/delete) | Sin cambios. Los contenedores y blobs de la cuenta eliminada no se pueden recuperar. | Sin cambios. Los contenedores y blobs de la cuenta eliminada no se pueden recuperar. |
| [Delete Container](/rest/api/storageservices/delete-container) | Sin cambios. Los blobs del contenedor eliminado no se pueden recuperar. | Sin cambios. Los blobs del contenedor eliminado no se pueden recuperar. |
| [Delete Blob](/rest/api/storageservices/delete-blob) | Si se usa para eliminar un blob, este se marca como eliminado temporalmente. <br /><br /> Si se usa para eliminar una instantánea de blob, esta se marca como eliminada temporalmente. | Si se usa para eliminar un blob, la versión actual se convierte en una versión anterior y se elimina la versión actual. No se crea ninguna versión nueva ni ninguna instantánea eliminada temporalmente.<br /><br /> Si se usa para eliminar la versión de un blob, esta se marca como eliminada temporalmente. |
| [Undelete Blob](/rest/api/storageservices/undelete-blob) | Restaura un blob y todas las instantáneas que se eliminaron durante el período de retención. | Restaura un blob y todas las versiones que se eliminaron durante el período de retención. |
| [Put Blob](/rest/api/storageservices/put-blob)<br />[Put Block List](/rest/api/storageservices/put-block-list)<br />[Copy Blob](/rest/api/storageservices/copy-blob)<br />[Copy Blob from URL](/rest/api/storageservices/copy-blob) | Si se llama en un blob activo, se genera automáticamente una instantánea del estado del blob antes de la operación. <br /><br /> Si se llama en un blob eliminado temporalmente, se genera una instantánea del estado anterior del blob solo si se está reemplazando por un blob del mismo tipo. Si el blob es de un tipo diferente, se eliminarán permanentemente todos los datos eliminados temporalmente. | Se genera automáticamente una nueva versión que captura el estado del blob antes de la operación. |
| [Put Block](/rest/api/storageservices/put-block) | Si se utiliza para confirmar un bloque en un blob activo, no habrá cambio alguno.<br /><br />Si utiliza para confirmar un bloque en un blob que se ha eliminado temporalmente, se crea un nuevo blob y se genera automáticamente una instantánea para capturar el estado del blob eliminado temporalmente. | Sin cambios |
| [Put Page](/rest/api/storageservices/put-page)<br />[Put Page from URL](/rest/api/storageservices/put-page-from-url) (Poner página de dirección URL) | Sin cambios. Los datos del blob en páginas que se sobrescriben o se borrar con esta operación no se guardan ni se pueden recuperar. | Sin cambios. Los datos del blob en páginas que se sobrescriben o se borrar con esta operación no se guardan ni se pueden recuperar. |
| [Append Block](/rest/api/storageservices/append-block)<br />[Append Block from URL](/rest/api/storageservices/append-block-from-url) (Anexar bloque desde dirección URL) | Sin cambios | Sin cambios. |
| [Set Blob Properties](/rest/api/storageservices/set-blob-properties) | Sin cambios. Las propiedades del blob sobrescribe no se pueden recuperar. | Sin cambios. Las propiedades del blob sobrescribe no se pueden recuperar. |
| [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata) | Sin cambios. Los metadatos del blob sobrescrito no se pueden recuperar. | Se genera automáticamente una nueva versión que captura el estado del blob antes de la operación. |
| [Set Blob Tier](/rest/api/storageservices/set-blob-tier) | El blob base se mueve al nuevo nivel. Las instantáneas activas o eliminadas temporalmente permanecen en el nivel original. No se crea ninguna instantánea eliminada temporalmente. | El blob base se mueve al nuevo nivel. Las versiones activas o eliminadas temporalmente permanecen en el nivel original. No se crea ninguna versión. |

### <a name="storage-account-hierarchical-namespace"></a>Cuenta de almacenamiento (espacio de nombres jerárquico)

|**Operación de API REST**|**Eliminación temporal habilitada**|
|---|---|
|[Ruta de acceso: eliminar](/rest/api/storageservices/datalakestoragegen2/path/delete) |Se crea un blob o directorio eliminado temporalmente. El objeto eliminado temporalmente se elimina después del período de retención.|
|[Delete Blob](/rest/api/storageservices/delete-blob)|Se crea un objeto eliminado temporalmente. El objeto eliminado temporalmente se elimina después del período de retención. No se admite la eliminación temporal para instantáneas y blobs con instantáneas.|
|[Ruta de acceso: Crear](/rest/api/storageservices/datalakestoragegen2/path/create), que cambia el nombre de un blob o un directorio. | El blob de destino existente o el directorio vacío se eliminarán temporalmente y el origen lo reemplazará. El objeto eliminado temporalmente se elimina después del período de retención.|

## <a name="feature-support"></a>Compatibilidad de características

En esta tabla se muestra cómo se admite esta característica en la cuenta y el impacto en la compatibilidad al habilitar determinadas funcionalidades.

| Tipo de cuenta de almacenamiento | Blob Storage (compatibilidad predeterminada) | Data Lake Storage Gen2 <sup>1</sup> | NFS 3.0 <sup>1</sup> | SFTP <sup>1</sup> |
|--|--|--|--|--|
| De uso general estándar, v2 | ![Sí](../media/icons/yes-icon.png) |![Sí](../media/icons/yes-icon.png)  <sup>2</sup>  <sup>3</sup>            | ![No](../media/icons/no-icon.png) | ![No](../media/icons/no-icon.png) |
| Blobs en bloques Premium          | ![Sí](../media/icons/yes-icon.png) |![Sí](../media/icons/yes-icon.png)  <sup>2</sup>  <sup>3</sup>            | ![No](../media/icons/no-icon.png) | ![No](../media/icons/no-icon.png) |

<sup>1</sup> La compatibilidad con Data Lake Storage Gen2, Network File System (NFS) 3.0 y el protocolo de transferencia de archivos segura (SFTP) requiere una cuenta de almacenamiento con un espacio de nombres jerárquico habilitado.

<sup>2</sup> La característica se admite en el nivel de versión preliminar.

<sup>3</sup>Para más información, consulte [Problemas conocidos con Azure Data Lake Storage Gen2](data-lake-storage-known-issues.md). Estos problemas se aplican a todas las cuentas que tienen habilitada la característica de espacio de nombres jerárquico.

## <a name="pricing-and-billing"></a>Precios y facturación

Todos los datos eliminados temporalmente se factura con la misma tasa que los datos activos. Sin embargo, los datos que se eliminen permanentemente después del periodo de retención no se cobrarán.

Cuando se habilita la eliminación temporal, Microsoft recomienda usar un período de retención pequeño para comprender mejor cómo afecta la característica a la facturación. El período de retención mínimo recomendado de las copias de seguridad es siete días.

Habilitar eliminación temporal para datos sobrescritos con frecuencia puede generar mayores cargos por capacidad de almacenamiento y una mayor latencia al enumerar los blobs. Tanto este costo adicional como la latencia se pueden mitigar almacenando los datos que se sobrescriben con frecuencia en una cuenta de almacenamiento independiente en la que la eliminación temporal esté deshabilitada.

No se facturan las transacciones relacionadas con la generación automática de instantáneas o versiones cuando se sobrescribe o elimina un blob. Se le facturarán las llamadas a la operación **Undelete Blob** según la tarifa de transacciones para las operaciones de escritura.

Para obtener más información sobre los precios de Blob Storage, consulte la página de [Precios de Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="blob-soft-delete-and-virtual-machine-disks"></a>Eliminación temporal de blobs y discos de máquinas virtuales

La eliminación temporal de blobs está disponible para discos no administrados prémium y estándar, que son blobs en páginas en segundo plano. La eliminación temporal solo le ayuda a recuperar los datos eliminados o sobrescritos por las operaciones [Delete Blob](/rest/api/storageservices/delete-blob), [Put Blob](/rest/api/storageservices/put-blob), [Put Block List](/rest/api/storageservices/put-block-list) y [Copy Blob](/rest/api/storageservices/copy-blob).

Los datos que se sobrescriben con una llamada a [Put Page](/rest/api/storageservices/put-page) no se pueden recuperar. Una máquina virtual de Azure escribe en un disco no administrado mediante llamadas a [Put Page](/rest/api/storageservices/put-page), por lo que el uso de la eliminación temporal para deshacer las escrituras en un disco no administrado desde una máquina virtual de Azure no es un escenario admitido.

## <a name="next-steps"></a>Pasos siguientes

- [Habilitación de la eliminación temporal para blobs](./soft-delete-blob-enable.md)
- [Administración y restauración de blobs eliminados temporalmente](soft-delete-blob-manage.md)
- [Control de versiones de blobs](versioning-overview.md)
