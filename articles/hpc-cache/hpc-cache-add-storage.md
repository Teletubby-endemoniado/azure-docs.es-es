---
title: Incorporación de almacenamiento a una instancia de Azure HPC Cache
description: Definición de los destinos de almacenamiento para que Azure HPC Cache pueda usar el sistema NFS local o los contenedores de Azure Blob Storage para el almacenamiento de archivos a largo plazo
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 09/22/2021
ms.custom: subject-rbac-steps
ms.author: v-erkel
ms.openlocfilehash: 016a62bc7713b389e8baf7d0a5b5aeefceb81f2b
ms.sourcegitcommit: 613789059b275cfae44f2a983906cca06a8706ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129271977"
---
# <a name="add-storage-targets"></a>Incorporación de destinos de almacenamiento

Los *destinos de almacenamiento* son espacios de almacenamiento en servidores de back-end para archivos a los que se accede mediante una instancia de Azure HPC Cache. Puede agregar almacenamiento NFS (como un sistema de hardware local) o almacenar datos en Azure Blob Storage.

Puede definir 10 destinos de almacenamiento diferentes para cualquier caché, y las memorias caché más grandes pueden [admitir hasta 20 destinos de almacenamiento](#size-your-cache-correctly-to-support-your-storage-targets).

La caché presenta todos los destinos de almacenamiento en un [espacio de nombres agregado](hpc-cache-namespace.md). Las rutas de acceso del espacio de nombres se configuran por separado después de agregar los destinos de almacenamiento.

Recuerde que las exportaciones de almacenamiento deben ser accesibles desde la red virtual de la caché. En el caso del almacenamiento en hardware local, es posible que tenga que configurar un servidor DNS que pueda resolver nombres de host para el acceso al almacenamiento de NFS. Obtenga más información en [Acceso DNS](hpc-cache-prerequisites.md#dns-access).

Agregue destinos de almacenamiento después de crear su caché. Siga este proceso:

1. [Creación de la caché](hpc-cache-create.md)
1. Definición de un destino de almacenamiento (información en este artículo)
1. [Cree las rutas de acceso orientadas al cliente](add-namespace-paths.md) (para el [espacio de nombres agregado](hpc-cache-namespace.md))

El procedimiento para agregar un destino de almacenamiento es ligeramente diferente según el tipo de almacenamiento que use. A continuación se muestran los detalles de cada uno.

Haga clic en la imagen siguiente para ver una [demostración en vídeo](https://azure.microsoft.com/resources/videos/set-up-hpc-cache/) sobre cómo crear una caché y agregar un destino de almacenamiento de Azure Portal.

[![Miniatura de vídeo: Azure HPC Cache: Instalación (haga clic para visitar la página del vídeo)](media/video-4-setup.png)](https://azure.microsoft.com/resources/videos/set-up-hpc-cache/)

## <a name="size-your-cache-correctly-to-support-your-storage-targets"></a>Asigne el tamaño correcto a la caché para respaldar a los destinos de almacenamiento

El número de destinos de almacenamiento admitidos depende del tamaño de la caché, el cual se establece durante su creación. La capacidad de la caché es una combinación de la capacidad de rendimiento (en GB/s) y la capacidad de almacenamiento (en TB).

* Hasta 10 destinos de almacenamiento: una caché estándar con el valor de almacenamiento de caché más pequeño o medio para el rendimiento seleccionado puede tener un máximo de 10 destinos de almacenamiento.

  Por ejemplo, si elige un rendimiento de 2 GB/segundo y no elige el tamaño de almacenamiento de caché más alto, la memoria caché admite un máximo de 10 destinos de almacenamiento.

* Hasta 20 destinos de almacenamiento:

  * Todas las cachés de alto rendimiento (que tienen tamaños de almacenamiento de caché preconfigurados) pueden admitir hasta 20 destinos de almacenamiento.
  * Las memorias caché estándar pueden admitir hasta 20 destinos de almacenamiento si elige el tamaño de caché más alto disponible para el valor de rendimiento seleccionado. (Si usa la CLI de Azure, elija el tamaño de caché válido más alto para la SKU de caché).

Consulte [Establecimiento de la capacidad de la memoria caché](hpc-cache-create.md#set-cache-capacity) para más información sobre la configuración de rendimiento y el tamaño de la caché.

## <a name="choose-the-correct-storage-target-type"></a>Elección del tipo de destino de almacenamiento correcto

Puede seleccionar entre tres tipos de destino de almacenamiento: **NFS**, **Blob** y **ADLS-NFS**. Elija el tipo que coincida con la clase de sistema de almacenamiento que usará para almacenar los archivos durante este proyecto de HPC Cache.

* **NFS**: cree un destino de almacenamiento de NFS para acceder a los datos en un sistema de almacenamiento conectado a la red (NAS). Puede ser un sistema de almacenamiento local u otro tipo de almacenamiento al que se pueda acceder con NFS.

  * Requisitos: [requisitos del almacenamiento de NFS](hpc-cache-prerequisites.md#nfs-storage-requirements)
  * Instrucciones. [Incorporación de un nuevo destino de almacenamiento NFS](#add-a-new-nfs-storage-target)

* **Blob**: use un destino de almacenamiento de blobs para almacenar los archivos de trabajo en un nuevo contenedor de blobs de Azure. Este contenedor solo se puede leer o escribir desde Azure HPC Cache.

  * Requisitos previos: [requisitos del almacenamiento de blobs](hpc-cache-prerequisites.md#blob-storage-requirements)
  * Instrucciones: [Incorporación de un nuevo destino de almacenamiento de Azure Blob Storage](#add-a-new-azure-blob-storage-target)

* **ADLS-NFS**: el destino de almacenamiento de ADLS-NFS accede a los datos de un contenedor de [blobs habilitado para NFS](../storage/blobs/network-file-system-protocol-support.md). Puede cargar previamente el contenedor mediante comandos NFS estándar y los archivos se pueden leer después con NFS.

  * Requisitos previos: [requisitos del almacenamiento de ADLS-NFS](hpc-cache-prerequisites.md#nfs-mounted-blob-adls-nfs-storage-requirements)
  * Instrucciones. [Incorporación de un nuevo destino de almacenamiento de ADLS-NFS](#add-a-new-adls-nfs-storage-target)

## <a name="add-a-new-azure-blob-storage-target"></a>Incorporación de un nuevo destino de almacenamiento de Azure Blob Storage

Un nuevo destino de almacenamiento de Azure Blob Storage necesita un contenedor de blobs vacío o un contenedor rellenado con datos con el formato de sistema de archivos en la nube de Azure HPC Cache. Obtenga más información sobre la carga previa de un contenedor de blobs en [Traslado de datos a Azure Blob Storage](hpc-cache-ingest.md).

En la página **Agregar destino de almacenamiento** de Azure Portal se incluye la opción de crear un nuevo contenedor de blobs justo antes de agregar el destino.

> [!NOTE]
>
> * En el caso de la instancia de Blob Storage montada en NFS, use el tipo de [destino de almacenamiento de ADLS-NFS](#add-a-new-adls-nfs-storage-target).
> * Las [configuraciones de caché de alto rendimiento](hpc-cache-create.md#choose-the-cache-type-for-your-needs) no admiten destinos estándar de almacenamiento de blobs de Azure. En su lugar, use el almacenamiento de blobs habilitado para NFS (ADLS-NFS).

### <a name="portal"></a>[Portal](#tab/azure-portal)

En Azure Portal, abra la instancia de caché y haga clic en **Destinos de almacenamiento** en la barra lateral izquierda.

![captura de pantalla de la página Configuración > Destino de almacenamiento, con dos destinos de almacenamiento en una tabla y el botón + Agregar destino de almacenamiento seleccionado encima de la tabla](media/add-storage-target-button.png)

En la página **Destinos de almacenamiento** se enumeran todos los destinos existentes y se proporciona un vínculo para agregar uno nuevo.

Haga clic en el botón **Agregar destino de almacenamiento**.

![captura de pantalla de la página de incorporación de destino de almacenamiento, rellenada con información para un nuevo destino de almacenamiento de Azure Blob Storage](media/hpc-cache-add-blob.png)

Para definir un contenedor de blobs de Azure, escriba esta información.

* **Storage target name** (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.
* **Target type** (Tipo de destino): elija **Blob**.
* **Cuenta de almacenamiento**: seleccione la cuenta que quiere usar.

  Tendrá que autorizar a la instancia de caché para acceder a la cuenta de almacenamiento, tal como se describe en [Incorporación de los roles de acceso](#add-the-access-control-roles-to-your-account).

  Para más información sobre el tipo de cuenta de almacenamiento que puede usar, consulte [Requisitos de Blob Storage](hpc-cache-prerequisites.md#blob-storage-requirements).

* **Contenedor de almacenamiento**: seleccione el contenedor de blobs para este destino o haga clic en **Crear**.

  ![Captura de pantalla del cuadro de diálogo para especificar el nombre y el nivel de acceso (privado) del nuevo contenedor](media/add-blob-new-container.png)

Cuando termine, haga clic en **Aceptar** para agregar el destino de almacenamiento.

> [!NOTE]
> Si el firewall de la cuenta de almacenamiento está configurado para restringir el acceso solo a "redes seleccionadas", use la solución alternativa documentada en [Solución alternativa para la configuración del firewall de la cuenta de Blob Storage](hpc-cache-blob-firewall-fix.md).

### <a name="add-the-access-control-roles-to-your-account"></a>Incorporación de los roles de control de acceso a la cuenta

Azure HPC Cache usa el [control de acceso basado en rol de Azure (Azure RBAC)](../role-based-access-control/index.yml) para autorizar el acceso del servicio de caché a la cuenta de almacenamiento de los destinos de Azure Blob Storage.

El propietario de la cuenta de almacenamiento debe agregar explícitamente los roles [Colaborador de la cuenta de almacenamiento](../role-based-access-control/built-in-roles.md#storage-account-contributor) y [Colaborador de datos de Storage Blob](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) para el usuario "HPC Cache Resource Provider" (Proveedor de recursos de HPC Cache).

Puede realizar esta tarea con anterioridad, o bien hacer clic en un vínculo de la página del portal donde se agrega un destino de almacenamiento de blobs. Tenga en cuenta que la configuración de roles puede tardar hasta cinco minutos en propagarse a través del entorno de Azure, por lo que debe esperar unos minutos después de agregar los roles antes de crear un destino de almacenamiento.

1. Abra **Control de acceso (IAM)** para su cuenta de almacenamiento.

1. Seleccione **Agregar** > **Agregar asignación de roles** para abrir la página Agregar asignación de roles.

1. Asigne los siguientes roles, de uno en uno. Para asignar roles, consulte [Asignación de roles de Azure mediante Azure Portal](../role-based-access-control/role-assignments-portal.md).
    
    | Configuración | Value |
    | --- | --- |
    | Roles | [Colaborador de la cuenta de almacenamiento](../role-based-access-control/built-in-roles.md#storage-account-contributor) <br/>  [Colaborador de datos de blobs de almacenamiento](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) |
    | Asignar acceso a | Proveedor de recursos de Azure HPC Cache |

    ![Página Agregar asignación de roles](../../includes/role-based-access-control/media/add-role-assignment-page.png)

   > [!NOTE]
   > Si no encuentra el proveedor de recursos HPC Cache, pruebe a buscar la cadena "storagecache". Se trata de un nombre previo a la disponibilidad general para la entidad de servicio.

<!-- 
Steps to add the Azure roles:

1. Open the **Access control (IAM)** page for the storage account. (The link in the **Add storage target** page automatically opens this page for the selected account.)

1. Click the **+** at the top of the page and choose **Add a role assignment**.

1. Select the role "Storage Account Contributor" from the list.

1. In the **Assign access to** field, leave the default value selected ("Azure AD user, group, or service principal").  

1. In the **Select** field, search for "hpc".  This string should match one service principal, named "HPC Cache Resource Provider". Click that principal to select it.

   > [!NOTE]
   > If a search for "hpc" doesn't work, try using the string "storagecache" instead. Users who participated in previews (before GA) might need to use the older name for the service principal.

1. Click the **Save** button at the bottom.

1. Repeat this process to assign the role "Storage Blob Data Contributor".  

![screenshot of add role assignment GUI](media/hpc-cache-add-role.png) -->

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

### <a name="prerequisite-storage-account-access"></a>Requisito previo: Acceso a las cuentas de almacenamiento

[Configuración de la CLI de Azure para Azure HPC Cache](./az-cli-prerequisites.md).

Antes de agregar un destino de almacenamiento de blobs, compruebe que la memoria caché tiene los roles correctos para acceder a la cuenta de almacenamiento y que la configuración del firewall permite la creación del destino de almacenamiento.

Azure HPC Cache usa el [control de acceso basado en rol de Azure (Azure RBAC)](../role-based-access-control/index.yml) para autorizar el acceso del servicio de caché a la cuenta de almacenamiento de los destinos de Azure Blob Storage.

El propietario de la cuenta de almacenamiento debe agregar explícitamente los roles [Colaborador de la cuenta de almacenamiento](../role-based-access-control/built-in-roles.md#storage-account-contributor) y [Colaborador de datos de Storage Blob](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) para el usuario "HPC Cache Resource Provider" (Proveedor de recursos de HPC Cache).

Se producirá un error en la creación del destino de almacenamiento si la memoria caché no tiene estos roles.

La configuración de roles puede tardar hasta cinco minutos en propagarse por el entorno de Azure, por lo que, después de agregar los roles, debe esperar para crear un destino de almacenamiento.

Lea [Incorporación o eliminación de asignaciones de roles de Azure mediante la CLI de Azure](../role-based-access-control/role-assignments-cli.md) para instrucciones detalladas.

Compruebe también la configuración del firewall de la cuenta de almacenamiento. Si restringe el acceso solo a las "redes seleccionadas", puede producirse un error al crearse el destino de almacenamiento. Use la solución alternativa que se indica en el documento [Solución alternativa para la configuración del firewall de la cuenta de Blob Storage](hpc-cache-blob-firewall-fix.md).

### <a name="add-a-blob-storage-target-with-azure-cli"></a>Incorporación de un destino de almacenamiento de blobs con la CLI de Azure

Use la interfaz [az hpc-cache blob-storage-target add](/cli/azure/hpc-cache/blob-storage-target#az_hpc_cache_blob_storage_target_add) para definir un destino de almacenamiento en Azure Blob Storage.

> [!NOTE]
> Actualmente, los comandos de la CLI de Azure requieren la creación de una ruta de acceso de espacio de nombres al agregar un destino de almacenamiento. Esto es diferente del proceso que se usa con la interfaz de Azure Portal.

Además de los parámetros estándar del grupo de recursos y del nombre de la memoria caché, para el destino de almacenamiento debe proporcionar estas opciones:

* ``--name`` (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.

* ``--storage-account`` - Identificador de la cuenta, en este formato: /subscriptions/ *<id_de_suscripción>* /resourceGroups/ *<grupo_de_recursos_de_almacenamiento>* /providers/Microsoft.Storage/storageAccounts/ *<nombre_de_la_cuenta>*

  Para más información sobre el tipo de cuenta de almacenamiento que puede usar, consulte [Requisitos de Blob Storage](hpc-cache-prerequisites.md#blob-storage-requirements).

* ``--container-name`` -Especifique el nombre del contenedor que se va a usar para este destino de almacenamiento.

* ``--virtual-namespace-path`` (Ruta de acceso del espacio de nombres virtual): establezca la ruta de archivo orientada al cliente para este destino de almacenamiento. Escriba las rutas de acceso entre comillas. Lea [Planeamiento del espacio de nombres agregado](hpc-cache-namespace.md) para más información acerca de la característica de espacio de nombres virtual.

Comando de ejemplo:

```azurecli
az hpc-cache blob-storage-target add --resource-group "hpc-cache-group" \
    --cache-name "cache1" --name "blob-target1" \
    --storage-account "/subscriptions/<subscriptionID>/resourceGroups/myrgname/providers/Microsoft.Storage/storageAccounts/myaccountname" \
    --container-name "container1" --virtual-namespace-path "/blob1"
```

---

## <a name="add-a-new-nfs-storage-target"></a>Incorporación de un nuevo destino de almacenamiento NFS

Un destino de almacenamiento de NFS tiene una configuración diferente de un destino de almacenamiento de blobs, incluida una configuración de modelo de uso que indica a la memoria caché cómo almacenar los datos de este sistema de almacenamiento.

![Captura de pantalla de la página para agregar destino de almacenamiento con destino NFS definido](media/add-nfs-target.png)

> [!NOTE]
> Antes de crear un destino de almacenamiento NFS, asegúrese de que el sistema de almacenamiento sea accesible desde la instancia de Azure HPC Cache y cumple los requisitos de permisos. Se producirá un error en la creación del destino de almacenamiento si la memoria caché no tiene acceso suficiente al sistema de almacenamiento. Consulte [Requisitos de almacenamiento NFS](hpc-cache-prerequisites.md#nfs-storage-requirements) y [Solución de problemas de configuración de NAS y problemas del destino de almacenamiento de NFS](troubleshoot-nas.md) para más información.

### <a name="choose-a-usage-model"></a>Selección de un modelo de uso

Cuando cree un destino de almacenamiento que use NFS para comunicarse con el sistema de almacenamiento, deberá elegir un modelo de uso de ese destino. Este modelo determina cómo se almacenan los datos en caché.

Consulte [Definición de los modelos de uso](cache-usage-models.md) para obtener más información sobre todos estos valores.

Los modelos de uso integrados de HPC Cache permiten elegir cómo equilibrar la respuesta rápida con el riesgo de obtener datos obsoletos. Si quiere optimizar la velocidad de lectura de los archivos, es posible que no le interese si los archivos de la memoria caché se comparan con los archivos de back-end. Por otro lado, si desea asegurarse de que los archivos estén siempre actualizados con el almacenamiento remoto, elija un modelo que realice comparaciones frecuentes.

> [!NOTE]
> Las [memorias caché de estilo de alto rendimiento](hpc-cache-create.md#choose-the-cache-type-for-your-needs) solo admiten el almacenamiento en caché de lectura.

Estas tres opciones cubren la mayoría de las situaciones:

* **Read heavy, infrequent writes (lectura intensiva, operaciones de escritura poco frecuentes)** : use esta opción si quiere acelerar el acceso de lectura a archivos que son estáticos o que no se suelen modificar.

  Esta opción almacena en caché los archivos de las lecturas del cliente, pero pasa las escrituras del cliente al almacenamiento de back-end inmediatamente. Los archivos almacenados en la memoria caché no se comparan automáticamente con los archivos del volumen de almacenamiento NFS.

  No use esta opción si existe el riesgo de que un archivo se pueda modificar directamente en el sistema de almacenamiento sin escribirlo primero en la memoria caché. Si esto sucede, la versión en caché del archivo no estará sincronizada con el archivo de back-end.

* **Mayor que el 15 % de textos**: esta opción acelera el rendimiento de lectura y escritura.

  Las lecturas de cliente y las escrituras de cliente se almacenan en caché. Se supone que los archivos de la memoria caché son más recientes que los archivos del sistema de almacenamiento de back-end. Los archivos almacenados en caché solo se comparan automáticamente con los archivos del almacenamiento de back-end cada ocho horas. Los archivos modificados en la memoria caché se escriben en el sistema de almacenamiento de back-end después de que han estado en la memoria caché durante 20 minutos sin cambios adicionales.

  No use esta opción si los clientes montan el volumen de almacenamiento back-end directamente, ya que existe el riesgo de que haya archivos obsoletos.

* **Los clientes escriben en el destino NFS omitiendo la caché**: elija esta opción si algún cliente del flujo de trabajo escribe datos directamente en el sistema de almacenamiento sin escribir primero en la caché o si quiere optimizar la coherencia de los datos.

  Los archivos que solicitan los clientes se almacenan en caché, pero los cambios que se realicen en esos archivos desde el cliente se pasan al sistema de almacenamiento de back-end inmediatamente. Los archivos de la memoria caché se comparan con frecuencia con las versiones de back-end para determinar si hay actualizaciones. Esta comprobación mantiene la coherencia de los datos cuando los archivos se modifican directamente en el sistema de almacenamiento en lugar de a través de la memoria caché.

Para obtener más información sobre las demás opciones, consulte [Definición de los modelos de uso](cache-usage-models.md).

En esta tabla se resumen las diferencias entre todos los modelos de uso:

[!INCLUDE [usage-models-table.md](includes/usage-models-table.md)]

> [!NOTE]
> El valor de la **Comprobación de back-end** muestra cuándo la caché compara automáticamente sus archivos con los archivos de origen que están en el almacenamiento remoto. Sin embargo, puede iniciar una comparación al enviar una solicitud de cliente que incluya una operación readdirplus en el sistema de almacenamiento de back-end. Readdirplus es una API NFS estándar (también llamada lectura extendida) que devuelve metadatos de directorio, lo que hace que la caché compare y actualice los archivos.

### <a name="create-an-nfs-storage-target"></a>Creación de un destino de almacenamiento de NFS

### <a name="portal"></a>[Portal](#tab/azure-portal)

En Azure Portal, abra la instancia de caché y haga clic en **Destinos de almacenamiento** en la barra lateral izquierda.

![captura de pantalla de la página Configuración > Destino de almacenamiento, con dos destinos de almacenamiento en una tabla y el botón + Agregar destino de almacenamiento seleccionado encima de la tabla](media/add-storage-target-button.png)

En la página **Destinos de almacenamiento** se enumeran todos los destinos existentes y se proporciona un vínculo para agregar uno nuevo.

Haga clic en el botón **Agregar destino de almacenamiento**.

![Captura de pantalla de la página para agregar destino de almacenamiento con destino NFS definido](media/add-nfs-target.png)

Proporcione esta información para un destino de almacenamiento respaldado por NFS:

* **Storage target name** (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.

* **Target type** (Tipo de destino): elija **NFS**.

* **Hostname** (Nombre de host): escriba la dirección IP o el nombre de dominio completo del sistema de almacenamiento NFS. (Use un nombre de dominio solo si la memoria caché tiene acceso a un servidor DNS que pueda resolver el nombre). Puede especificar varias direcciones IP si varias direcciones IP hacen referencia al sistema de almacenamiento.

* **Modelo de uso**: elija uno de los perfiles de almacenamiento en caché de datos en función del flujo de trabajo, tal como se describe en la sección [Selección de un modelo de uso](#choose-a-usage-model) anterior.

Cuando termine, haga clic en **Aceptar** para agregar el destino de almacenamiento.

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

[Configuración de la CLI de Azure para Azure HPC Cache](./az-cli-prerequisites.md).

Use el comando [az hpc-cache nfs-storage-target add](/cli/azure/hpc-cache/nfs-storage-target#az_hpc_cache_nfs_storage_target_add) de la CLI de Azure para crear el destino de almacenamiento.

> [!NOTE]
> Actualmente, los comandos de la CLI de Azure requieren la creación de una ruta de acceso de espacio de nombres al agregar un destino de almacenamiento. Esto es diferente del proceso que se usa con la interfaz de Azure Portal.

Proporcione estos valores además del nombre de la caché y el grupo de recursos de caché:

* ``--name`` (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.
* ``--nfs3-target``: dirección IP del sistema de almacenamiento NFS. (Puede usar un nombre de dominio completo aquí si la caché tiene acceso a un servidor DNS que lo resuelva).
* ``--nfs3-usage-model``: uno de los perfiles de almacenamiento en caché de datos, que se describe en la sección [Selección de un modelo de uso](#choose-a-usage-model) anterior.

  Compruebe los nombres de los modelos de uso con el comando [az hpc-cache usage-model list](/cli/azure/hpc-cache/usage-model#az_hpc_cache_usage_model_list).

* ``--junction``: parámetro de unión que vincula la ruta de acceso del archivo virtual orientado al cliente con una ruta de acceso de exportación del sistema de almacenamiento.

  Un destino de almacenamiento NFS puede tener varias rutas de acceso virtuales, siempre y cuando cada ruta represente una exportación o un subdirectorio diferente sen el mismo sistema de almacenamiento. Creación de todas las rutas de acceso de un sistema de almacenamiento en un destino de almacenamiento.

  Puede [agregar y editar rutas de acceso de espacio de nombres](add-namespace-paths.md) en un destino de almacenamiento en cualquier momento.

  El parámetro ``--junction`` usa estos valores:

  * ``namespace-path``: Ruta de acceso del archivo virtual orientado al cliente
  * ``nfs-export``: Exportación del sistema de almacenamiento que se va a asociar a la ruta de acceso orientada al cliente
  * ``target-path`` (opcional): Subdirectorio de la exportación, si es necesario

  Ejemplo: ``--junction namespace-path="/nas-1" nfs-export="/datadisk1" target-path="/test"``

  Lea [Configuración del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más información acerca de la característica de espacio de nombres virtual.

Comando de ejemplo:

```azurecli

az hpc-cache nfs-storage-target add --resource-group "hpc-cache-group" --cache-name "doc-cache0629" \
    --name nfsd1 --nfs3-target 10.0.0.4 --nfs3-usage-model WRITE_WORKLOAD_15 \
    --junction namespace-path="/nfs1/data1" nfs-export="/datadisk1" target-path=""
```

Salida:
```azurecli

{- Finished ..
  "clfs": null,
  "id": "/subscriptions/<subscriptionID>/resourceGroups/hpc-cache-group/providers/Microsoft.StorageCache/caches/doc-cache0629/storageTargets/nfsd1",
  "junctions": [
    {
      "namespacePath": "/nfs1/data1",
      "nfsExport": "/datadisk1",
      "targetPath": ""
    }
  ],
  "location": "eastus",
  "name": "nfsd1",
  "nfs3": {
    "target": "10.0.0.4",
    "usageModel": "WRITE_WORKLOAD_15"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "hpc-cache-group",
  "targetType": "nfs3",
  "type": "Microsoft.StorageCache/caches/storageTargets",
  "unknown": null
}

```

---

## <a name="add-a-new-adls-nfs-storage-target"></a>Incorporación de un nuevo destino de almacenamiento de ADLS-NFS

Los destinos de almacenamiento de ADLS-NFS usan contenedores de blobs de Azure que admiten el protocolo Network File System (NFS) 3.0.

Consulte [Compatibilidad con el protocolo NFS 3.0](../storage/blobs/network-file-system-protocol-support.md) para más información sobre esta característica.

Los destinos de almacenamiento de ADLS-NFS tienen algunas similitudes con los destinos de Blob Storage y otros con destinos de almacenamiento NFS. Por ejemplo:

* Al igual que con un destino de Blob Storage, debe conceder permiso a Azure HPC Cache para [acceder a la cuenta de almacenamiento](#add-the-access-control-roles-to-your-account).
* Al igual que con un destino de almacenamiento NFS, debe establecer un [modelo de uso](#choose-a-usage-model) de caché.
* Dado que los contenedores de blobs habilitados para NFS tienen una estructura jerárquica compatible con NFS, no es necesario que use la caché para ingerir datos; además, otros sistemas NFS pueden leer los contenedores.

  Puede cargar previamente los datos en un contenedor de ADLS-NFS y, a continuación, agregarlos a una instancia de HPC Cache como destino de almacenamiento para, finalmente, acceder a los datos más adelante desde fuera de HPC Cache. Cuando se usa un contenedor de blobs estándar como destino de almacenamiento en HPC Cache, los datos se escriben en un formato propietario y solo se puede acceder a ellos desde otros productos compatibles con Azure HPC Cache.

Para que pueda crear un destino de almacenamiento de ADLS-NFS, primero debe crear una cuenta de almacenamiento habilitada para NFS. Siga los pasos de [Requisitos previos de Azure HPC Cache](hpc-cache-prerequisites.md#nfs-mounted-blob-adls-nfs-storage-requirements) y las instrucciones de [Montaje de Blob Storage con NFS](../storage/blobs/network-file-system-protocol-support-how-to.md). Si no usa la misma red virtual para la caché y la cuenta de almacenamiento, asegúrese de que la red virtual de la caché pueda acceder a la red virtual de la cuenta de almacenamiento.

Una vez configurada la cuenta de almacenamiento, podrá crear un nuevo contenedor cuando cree el destino de almacenamiento.

Consulte [Uso del almacenamiento de blobs montado en NFS (versión preliminar) con Azure HPC Cache](nfs-blob-considerations.md) para más información sobre esta configuración.

Para crear un destino de almacenamiento de ADLS-NFS, abra la página **Agregar destino de almacenamiento** en Azure Portal. (Los métodos adicionales están en desarrollo).

![Captura de pantalla de la página para agregar destino de almacenamiento con el destino ADLS-NFS definido](media/add-adls-target.png)

Escriba esta información.

* **Storage target name** (Nombre de destino de almacenamiento): establezca un nombre que identifique este destino de almacenamiento en Azure HPC Cache.
* **Tipo de destino**: elija **ADLS-NFS**.
* **Cuenta de almacenamiento**: seleccione la cuenta que quiere usar. Si la cuenta de almacenamiento habilitada para NFS no aparece en la lista, compruebe que cumple los requisitos previos y que la memoria caché puede acceder a ella.

  Tendrá que autorizar a la instancia de caché para acceder a la cuenta de almacenamiento, tal como se describe en [Incorporación de los roles de acceso](#add-the-access-control-roles-to-your-account).

* **Contenedor de almacenamiento**: seleccione el contenedor de blobs habilitado para NFS para este destino o haga clic en **Crear**.

* **Modelo de uso**: elija uno de los perfiles de almacenamiento en caché de datos en función del flujo de trabajo, tal como se describe en la sección [Selección de un modelo de uso](#choose-a-usage-model) anterior.

Cuando termine, haga clic en **Aceptar** para agregar el destino de almacenamiento.

## <a name="view-storage-targets"></a>Visualización de los destinos de almacenamiento

Puede usar Azure Portal o la CLI de Azure para mostrar los destinos de almacenamiento ya definidos para la memoria caché.

### <a name="portal"></a>[Portal](#tab/azure-portal)

En Azure Portal, abra la instancia de caché y haga clic en **Destinos de almacenamiento**, que se encuentra debajo del título Configuración en la barra lateral izquierda. En la página Destinos de almacenamiento se enumeran todos los destinos y se proporciona un vínculo para agregar uno nuevo.

Haga clic en el nombre de un destino de almacenamiento para abrir su página de detalles.

Lea [Visualización y administración de destinos de almacenamiento](manage-storage-targets.md) y [Edición de los destinos de almacenamiento](hpc-cache-edit-storage.md) para más información.

### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

[Configuración de la CLI de Azure para Azure HPC Cache](./az-cli-prerequisites.md).

Use la opción [az hpc-cache storage-target list](/cli/azure/hpc-cache/storage-target#az_hpc_cache_storage-target-list) para mostrar los destinos de almacenamiento de la caché existentes. Proporcione el nombre de la caché y el grupo de recursos (a menos que lo haya establecido como global).

```azurecli
az hpc-cache storage-target list --resource-group "scgroup" --cache-name "sc1"
```

Use [az hpc-cache storage-target show](/cli/azure/hpc-cache/storage-target#az_hpc_cache_storage-target-list) para ver detalles sobre un destino de almacenamiento concreto (especifíquelo por su nombre).

Ejemplo:

```azurecli
$ az hpc-cache storage-target show --cache-name doc-cache0629 --name nfsd1

{
  "clfs": null,
  "id": "/subscriptions/<subscription_ID>/resourceGroups/scgroup/providers/Microsoft.StorageCache/caches/doc-cache0629/storageTargets/nfsd1",
  "junctions": [
    {
      "namespacePath": "/nfs1/data1",
      "nfsExport": "/datadisk1",
      "targetPath": ""
    }
  ],
  "location": "eastus",
  "name": "nfsd1",
  "nfs3": {
    "target": "10.0.0.4",
    "usageModel": "WRITE_WORKLOAD_15"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "scgroup",
  "targetType": "nfs3",
  "type": "Microsoft.StorageCache/caches/storageTargets",
  "unknown": null
}
$
```

---

## <a name="next-steps"></a>Pasos siguientes

Después de crear destinos de almacenamiento, continúe con estas tareas para preparar la caché para su uso:

* [Configuración del espacio de nombres agregado](add-namespace-paths.md)
* [Montaje de la instancia de Azure HPC Cache](hpc-cache-mount.md)
* [Traslado de datos a Azure Blob Storage](hpc-cache-ingest.md)

Si necesita actualizar cualquier configuración, puede [editar un destino de almacenamiento](hpc-cache-edit-storage.md).
