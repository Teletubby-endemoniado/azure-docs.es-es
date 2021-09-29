---
title: Requisito de transferencia segura para garantizar conexiones seguras
titleSuffix: Azure Storage
description: Obtenga información sobre cómo requerir una transferencia segura para las solicitudes a Azure Storage. Cuando se requiere una transferencia segura para una cuenta de almacenamiento, se rechazan todas las solicitudes que se originan en una conexión no segura.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 06/01/2021
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 65d877bce0bdcab35248d4b9a41b92f46c132903
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128630743"
---
# <a name="require-secure-transfer-to-ensure-secure-connections"></a>Requisito de transferencia segura para garantizar conexiones seguras

Puede configurar la cuenta de almacenamiento para que acepte solicitudes de conexiones seguras solo si establece la propiedad **Se requiere transferencia segura** para la cuenta de almacenamiento. Cuando se requiere una transferencia segura, se rechazan todas las solicitudes que se originan en una conexión no segura. Microsoft recomienda que siempre se requiera una transferencia segura para todas las cuentas de almacenamiento.

Cuando se requiere una transferencia segura, se debe realizar una llamada a una operación de API REST de Azure Storage a través de HTTPS. Se rechaza cualquier solicitud realizada a través de HTTP. De forma predeterminada, la propiedad **Se requiere transferencia segura** se habilita al crear una cuenta de almacenamiento.

Azure Policy proporciona una directiva integrada para garantizar que se requiere una transferencia segura para las cuentas de almacenamiento. Para obtener más información, consulte la sección **Almacenamiento** de las [definiciones de directivas integradas de Azure Policy](../../governance/policy/samples/built-in-policies.md#storage).

Se produce un error al establecer conexión con un recurso compartido de archivos de Azure a través de SMB sin cifrado cuando se requiere la transferencia segura para la cuenta de almacenamiento. Entre los ejemplos de conexiones no seguras se incluyen las realizadas a través de SMB 2.1 o SMB 3.x sin cifrado.

> [!NOTE]
> Dado que Azure Storage no admite HTTPS para los nombres de dominio personalizados, esta opción no se aplica cuando se utiliza un nombre de dominio personalizado.
>
> Esta configuración de transferencia segura no se aplica a TCP. Las conexiones por medio del protocolo NFS 3.0 que admiten Azure Blob Storage con TCP, que no está protegido, se realizarán correctamente.

## <a name="require-secure-transfer-in-the-azure-portal"></a>Requerir una transferencia segura en Azure Storage

Puede activar la propiedad **Se requiere transferencia segura** al crear una cuenta de almacenamiento en [Azure Portal](https://portal.azure.com). También puede habilitarla para cuentas de almacenamiento existentes.

### <a name="require-secure-transfer-for-a-new-storage-account"></a>Solicitud de transferencia segura en una nueva cuenta de almacenamiento

1. Abra el panel **Crear cuenta de almacenamiento** en Azure Portal.
1. En la página **Avanzado**, seleccione la casilla **Habilitar transferencia segura**.

   ![Creación de una hoja de la cuenta de almacenamiento](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Solicitud de transferencia segura en una cuenta de almacenamiento existente

1. Seleccione una cuenta de almacenamiento existente en Azure Portal.
1. En el panel de menú de la cuenta de almacenamiento, en **Configuración**, seleccione **Opciones de configuración**.
1. En **Se requiere transferencia segura**, seleccione **Habilitado**.

   ![Panel de menú de la cuenta de almacenamiento](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="require-secure-transfer-from-code"></a>Requerir una transferencia segura desde el código

Para requerir una transferencia segura mediante programación, establezca la propiedad *enableHttpsTrafficOnly* en *True* en la cuenta de almacenamiento. Puede establecer esta propiedad mediante la API REST del proveedor de recursos de almacenamiento, las bibliotecas de cliente o las herramientas siguientes:

- [REST API](/rest/api/storagerp/storageaccounts)
- [PowerShell](/powershell/module/az.storage/set-azstorageaccount)
- [CLI](/cli/azure/storage/account)
- [NodeJS](https://www.npmjs.com/package/@azure/arm-storage/)
- [SDK de .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage)
- [SDK de Python](https://pypi.org/project/azure-mgmt-storage)
- [SDK de Ruby](https://rubygems.org/gems/azure_mgmt_storage)

## <a name="require-secure-transfer-with-powershell"></a>Requerir una transferencia segura con PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

En este ejemplo, debe utilizarse la versión 0.7 del módulo Az de Azure PowerShell o una versión posterior. Ejecute `Get-Module -ListAvailable Az` para encontrar la versión. Si necesita instalarla o actualizarla, consulte el artículo sobre [cómo instalar el módulo de Azure PowerShell](/powershell/azure/install-Az-ps).

Ejecute `Connect-AzAccount` para crear una conexión con Azure.

 Utilice la siguiente línea de comandos siguiente para comprobar la configuración:

```powershell
Get-AzStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}"
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : False
...

```

Utilice la siguiente línea de comandos siguiente para habilitar la configuración:

```powershell
Set-AzStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}" -EnableHttpsTrafficOnly $True
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : True
...

```

## <a name="require-secure-transfer-with-azure-cli"></a>Requerir una transferencia segura con la CLI de Azure

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 Utilice el comando siguiente para comprobar esta configuración:

```azurecli-interactive
az storage account show -g {ResourceGroupName} -n {StorageAccountName}
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": false,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

Utilice el comando siguiente para habilitar la configuración:

```azurecli-interactive
az storage account update -g {ResourceGroupName} -n {StorageAccountName} --https-only true
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": true,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

## <a name="next-steps"></a>Pasos siguientes

[Recomendaciones de seguridad para Blob Storage](../blobs/security-recommendations.md)
