---
title: 'Creación de un recurso compartido NFS (versión preliminar): Azure Files'
description: Obtenga información sobre cómo crear un recurso compartido de archivos de Azure que se pueda montar con el protocolo Network File System.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 10/25/2021
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: f146d51cdd43b8c4a52285476e47d0c6237efe0f
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131046610"
---
# <a name="how-to-create-an-nfs-share-preview"></a>Procedimiento para crear un recurso compartido de NFS (versión preliminar)
Los recursos compartidos de archivos de Azure son recursos compartidos de archivos totalmente administrados en la nube. En este artículo se trata la creación de un recurso compartido de archivos que usa el protocolo NFS (versión preliminar).

## <a name="applies-to"></a>Se aplica a
| Tipo de recurso compartido de archivos | SMB | NFS |
|-|:-:|:-:|
| Recursos compartidos de archivos Estándar (GPv2), LRS/ZRS | ![No](../media/icons/no-icon.png) | ![No](../media/icons/no-icon.png) |
| Recursos compartidos de archivos Estándar (GPv2), GRS/GZRS | ![No](../media/icons/no-icon.png) | ![No](../media/icons/no-icon.png) |
| Recursos compartidos de archivos Premium (FileStorage), LRS/ZRS | ![No](../media/icons/no-icon.png) | ![Sí](../media/icons/yes-icon.png) |

## <a name="limitations"></a>Limitaciones
[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]


### <a name="regional-availability"></a>Disponibilidad regional
[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Requisitos previos
- Los recursos compartidos NFS solo aceptan UID/GID numéricos. Para evitar que los clientes envíen UID/GID alfanuméricos, deshabilite la asignación de identificadores.
- Solo se puede tener acceso a los recursos compartidos de NFS desde redes de confianza. Las conexiones a su recurso compartido de NFS deben originarse en uno de los orígenes siguientes:
    - [Cree un punto de conexión privado](storage-files-networking-endpoints.md#create-a-private-endpoint) (recomendado) o [restrinja el acceso al punto de conexión público](storage-files-networking-endpoints.md#restrict-public-endpoint-access).
    - [Configure una VPN de punto a sitio (P2S) en Linux para su uso con Azure Files](storage-files-configure-p2s-vpn-linux.md).
    - [Configure una VPN de sitio a sitio para su uso con Azure Files](storage-files-configure-s2s-vpn.md).
    - Configure [ExpressRoute](../../expressroute/expressroute-introduction.md).

- Si planea usar la CLI de Azure, [instale la versión más reciente](/cli/azure/install-azure-cli).

## <a name="register-the-nfs-41-protocol"></a>Registro del protocolo NFS 4.1

Primero debe registrarse para la característica con el fin de crear recursos compartidos de archivos de Azure de NFS. No puede crear recursos compartidos de NFS en cuentas de almacenamiento que se crearon antes del registro.

Si usa el módulo Azure PowerShell o la CLI de Azure, registre la característica con los siguientes comandos:

# <a name="portal"></a>[Portal](#tab/azure-portal)
Use Azure PowerShell o la CLI de Azure para registrar la característica NFS 4.1 en Azure Files.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```azurepowershell
# Connect your PowerShell session to your Azure account, if you have not already done so.
Connect-AzAccount
# Set the actively selected subscription, if you have not already done so.
$subscriptionId = "<yourSubscriptionIDHere>"
$context = Get-AzSubscription -SubscriptionId $subscriptionId
Set-AzContext $context
# Register the NFS 4.1 feature with Azure Files to enable the preview.
Register-AzProviderFeature `
    -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowNfsFileShares 
    
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
```azurecli
# Connect your Azure CLI to your Azure account, if you have not already done so.
az login
# Provide the subscription ID for the subscription where you would like to 
# register the feature
subscriptionId="<yourSubscriptionIDHere>"
az feature register \
    --name AllowNfsFileShares \
    --namespace Microsoft.Storage \
    --subscription $subscriptionId
az provider register \
    --namespace Microsoft.Storage
```

---

La aprobación del registro puede tardar hasta una hora en completarse. Para comprobar si el registro se ha completado, ejecute los siguientes comandos:

# <a name="portal"></a>[Portal](#tab/azure-portal)
Use Azure PowerShell o la CLI de Azure para comprobar el registro de la característica NFS 4.1 en Azure Files. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
```azurepowershell
Get-AzProviderFeature `
    -ProviderNamespace Microsoft.Storage `
    -FeatureName AllowNfsFileShares
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
```azurecli
az feature show \
    --name AllowNfsFileShares \
    --namespace Microsoft.Storage \
    --subscription $subscriptionId
```
---

## <a name="create-a-filestorage-storage-account"></a>Creación de una cuenta de almacenamiento FileStorage
Actualmente, los recursos compartidos de NFS 4.1 solo están disponibles como recursos compartidos de archivos prémium. Para implementar un recurso compartido de archivos prémium que admita el protocolo NFS 4.1, primero debe crear una cuenta de almacenamiento de FileStorage. Una cuenta de almacenamiento es un objeto de nivel superior de Azure que representa un grupo compartido de almacenamiento que se puede usar para implementar varios recursos compartidos de archivos de Azure.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Para crear una cuenta de almacenamiento de FileStorage, vaya a Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), seleccione **Cuentas de almacenamiento** en el menú de la izquierda.

    ![Página principal de Azure Portal con la selección de la cuenta de almacenamiento.](media/storage-how-to-create-premium-fileshare/azure-portal-storage-accounts.png)

1. En la ventana **Cuentas de almacenamiento** que aparece, elija **Agregar**.
1. Seleccione la suscripción en la que se va a crear la cuenta de almacenamiento.
1. Selección del grupo de recursos en el que se va a crear la cuenta de almacenamiento
1. Después, escriba un nombre para la cuenta de almacenamiento. El nombre que elija debe ser único en Azure. El nombre debe tener también una longitud de entre 3 y 24 caracteres y solo puede contener números y letras minúsculas.
1. Seleccione una ubicación para la cuenta de almacenamiento o utilice la ubicación predeterminada.
1. En **Rendimiento**, seleccione **Premium**.

    Tiene que seleccionar **Premium** para que **Recursos compartidos de archivos** sea una opción disponible en la lista desplegable **Tipo de cuenta**.

1. En **Tipo de cuenta prémium**, elija **Recursos compartidos de archivos**.

    :::image type="content" source="media/storage-how-to-create-file-share/files-create-smb-share-performance-premium.png" alt-text="Captura de pantalla del rendimiento prémium seleccionado.":::

1. Mantenga la opción **Replicación** establecida en su valor predeterminado de **Almacenamiento con redundancia local (LRS)** .
1. Seleccione **Revisar y crear** para revisar la configuración de la cuenta de almacenamiento y crear la cuenta.
1. Seleccione **Crear**.

Una vez que se ha creado el recurso de la cuenta de almacenamiento, vaya hasta él.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Para crear una cuenta de almacenamiento de FileStorage, abra un símbolo del sistema de PowerShell y ejecute los siguientes comandos; no olvide reemplazar `<resource-group>` y `<storage-account>` por los valores adecuados para su entorno.

```powershell
$resourceGroupName = "<resource-group>"
$storageAccountName = "<storage-account>"
$location = "westus2"

$storageAccount = New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName `
    -SkuName Premium_LRS `
    -Location $location `
    -Kind FileStorage
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
Para crear una cuenta de almacenamiento de FileStorage, abra el terminal y ejecute los siguientes comandos; no olvide reemplazar `<resource-group>` y `<storage-account>` por los valores adecuados para su entorno.

```azurecli-interactive
resourceGroup="<resource-group>"
storageAccount="<storage-account>"
location="westus2"

az storage account create \
    --resource-group $resourceGroup \
    --name $storageAccount \
    --location $location \
    --sku Premium_LRS \
    --kind FileStorage
```
---

## <a name="disable-secure-transfer"></a>Deshabilitación de la transferencia segura

No puede montar un recurso compartido de archivos NFS a menos que deshabilite la transferencia segura.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Vaya a la cuenta de almacenamiento que ha creado.
1. Seleccione **Configuración**.
1. Seleccione **Deshabilitado** para **Se requiere transferencia segura**.
1. Seleccione **Guardar**.

    :::image type="content" source="media/storage-files-how-to-mount-nfs-shares/disable-secure-transfer.png" alt-text="Captura de la pantalla de configuración de la cuenta de almacenamiento con la transferencia segura deshabilitada." lightbox="media/storage-files-how-to-mount-nfs-shares/disable-secure-transfer.png":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Set-AzStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}" -EnableHttpsTrafficOnly $False
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

```azurecli
az storage account update -g {ResourceGroupName} -n {StorageAccountName} --https-only false
```
---

## <a name="create-an-nfs-share"></a>Creación de un recurso compartido NFS

# <a name="portal"></a>[Portal](#tab/azure-portal)

Ahora que ha creado una cuenta de FileStorage y ha configurado la red, puede crear un recurso compartido de archivos NFS. El proceso es similar a la creación de un recurso compartido SMB. Seleccione **NFS** en lugar de **SMB** al crear el recurso compartido.

1. Vaya a la cuenta de almacenamiento y seleccione **Recursos compartidos de archivos**.
1. Seleccione **+ Recurso compartido de archivos** para crear un recurso compartido de archivos.
1. Asigne un nombre al recurso compartido de archivos y seleccione una capacidad aprovisionada.
1. En **Protocolo** seleccione **NFS**.
1. Para **Squash raíz** realice una selección.

    - Squash raíz (predeterminado): el acceso al superusuario remoto (raíz) se asigna a UID (65534) y GID (65534).
    - Sin squash raíz: el superusuario remoto (raíz) recibe acceso como raíz.
    - Todos con squash: todo el acceso de usuario se asigna a UID (65534) y GID (65534).
    
1. Seleccione **Crear**.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/files-nfs-create-share.png" alt-text="Captura de pantalla de la hoja de creación de recursos compartidos de archivos.":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. Para crear un recurso compartido de archivos Premium con el módulo de Azure PowerShell, use el cmdlet [New-AzRmStorageShare](/powershell/module/az.storage/new-azrmstorageshare).

    > [!NOTE]
    > Los recursos compartidos de archivos prémium se facturan mediante un modelo aprovisionado. El tamaño aprovisionado del recurso compartido se especifica mediante `QuotaGiB` a continuación. Para más información, consulte [Descripción del modelo aprovisionado](understanding-billing.md#provisioned-model) y la [página de precios de Azure Files](https://azure.microsoft.com/pricing/details/storage/files/).

    ```powershell
    New-AzRmStorageShare `
        -StorageAccount $storageAccount `
        -Name myshare `
        -EnabledProtocol NFS `
        -RootSquash RootSquash `
        -QuotaGiB 1024
    ```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
Para crear un recurso compartido de archivos Premium con la CLI de Azure, utilice el comando [az storage share create](/cli/azure/storage/share-rm).

> [!NOTE]
> Los recursos compartidos de archivos prémium se facturan mediante un modelo aprovisionado. El tamaño aprovisionado del recurso compartido se especifica mediante `quota` a continuación. Para más información, consulte [Descripción del modelo aprovisionado](understanding-billing.md#provisioned-model) y la [página de precios de Azure Files](https://azure.microsoft.com/pricing/details/storage/files/).

```azurecli-interactive
az storage share-rm create \
    --resource-group $resourceGroup \
    --storage-account $storageAccount \
    --name "myshare" \
    --enabled-protocol NFS \
    --root-squash RootSquash \
    --quota 1024
```
---

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha creado un recurso compartido de NFS, para usarlo debe montarlo en el cliente Linux. Para obtener más información, consulte [Procedimiento para montar un recurso compartido NFS](storage-files-how-to-mount-nfs-shares.md).

Si tiene algún problema, consulte [Solución de problemas de los recursos compartidos de archivos NFS de Azure](storage-troubleshooting-files-nfs.md).
