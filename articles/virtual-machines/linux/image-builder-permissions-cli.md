---
title: Configuración de permisos del servicio Azure Image Builder mediante la CLI de Azure
description: Configure los requisitos para el servicio Azure VM Image Builder, incluidos los permisos y privilegios mediante la CLI de Azure.
author: kof-f
ms.author: kofiforson
ms.reviewer: cynthn
ms.date: 04/02/2021
ms.topic: article
ms.service: virtual-machines
ms.subservice: image-builder
ms.openlocfilehash: 05195eb10355184fd2b8f8a44472099d85295406
ms.sourcegitcommit: 2da83b54b4adce2f9aeeed9f485bb3dbec6b8023
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2021
ms.locfileid: "122769089"
---
# <a name="configure-azure-image-builder-service-permissions-using-azure-cli"></a>Configuración de permisos del servicio Azure Image Builder mediante la CLI de Azure

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Conjuntos de escalado flexibles 

Al registrarse para el (AIB), se concede al servicio AIB permiso para crear, administrar y eliminar un grupo de recursos de almacenamiento provisional (IT_ *) y tener derechos para agregarle recursos, que son necesarios para la compilación de la imagen. Esto se realiza mediante un nombre de entidad de seguridad de servicio (SPN) AIB que se pone a disposición de la suscripción durante un registro correcto.

Para permitir que Azure VM Image Builder distribuya imágenes a las imágenes administradas o a Shared Image Gallery, debe crear una identidad asignada por el usuario de Azure que tenga permisos para leer y escribir imágenes. Si va a acceder a Azure Storage, necesitará permisos para leer contenedores privados y públicos.

Debe configurar los permisos y los privilegios antes de crear una imagen. En las secciones siguientes se detalla cómo configurar posibles escenarios mediante la CLI de Azure.


[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

## <a name="create-an-azure-user-assigned-managed-identity"></a>Creación de una identidad administrada de Azure asignada por el usuario

Azure Image Builder requiere la creación de una [identidad administrada de Azure asignada por el usuario](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md). Azure Image Builder usa la identidad administrada asignada por el usuario para leer imágenes, escribir imágenes y acceder a las cuentas de Azure Storage. El permiso de identidad se concede para realizar acciones específicas en una suscripción.

> [!NOTE]
> Anteriormente, Azure Image Builder usaba el nombre de entidad de seguridad de servicio (SPN) de Azure Image Builder para conceder permisos a los grupos de recursos de imagen. El uso del SPN caerá en desuso. Use una identidad administrada asignada por el usuario en su lugar.

En el ejemplo siguiente se muestra cómo crear una identidad administrada de Azure asignada por el usuario. Reemplace la configuración del marcador de posición para establecer las variables.

| Configuración | Descripción |
|---------|-------------|
| \<Resource group\> | Grupo de recursos donde se creará la identidad administrada asignada por el usuario. |

```azurecli-interactive
identityName="aibIdentity"
imageResourceGroup=<Resource group>

az identity create \
    --resource-group $imageResourceGroup \
    --name $identityName
```

Para obtener más información sobre las identidades de Azure asignadas por el usuario, consulte la documentación de la [Identidad administrada de Azure asignada por el usuario](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) sobre cómo crear una identidad.

## <a name="allow-image-builder-to-distribute-images"></a>Permiso para que Image Builder distribuya imágenes

Para que Azure Image Builder distribuya imágenes (imágenes administradas/galería de imágenes compartidas), se debe permitir que el servicio Azure Image Builder inserte las imágenes en estos grupos de recursos. Para conceder los permisos necesarios, debe crear una identidad administrada asignada por el usuario y concederle derechos en el grupo de recursos donde se genera la imagen. Azure Image Builder **no** tiene permiso para acceder a los recursos de otros grupos de recursos de la suscripción. Debe realizar acciones explícitas para permitir el acceso para evitar errores en la creación.

No es necesario conceder derechos de colaborador de identidad administrada asignada por el usuario en el grupo de recursos para distribuir las imágenes. Sin embargo, la identidad administrada asignada por el usuario necesita los siguientes permisos `Actions` de Azure en el grupo de recursos de distribución:

```Actions
Microsoft.Compute/images/write
Microsoft.Compute/images/read
Microsoft.Compute/images/delete
```

Si se distribuye a una galería de imágenes compartidas, también necesitará:

```Actions
Microsoft.Compute/galleries/read
Microsoft.Compute/galleries/images/read
Microsoft.Compute/galleries/images/versions/read
Microsoft.Compute/galleries/images/versions/write
```

## <a name="permission-to-customize-existing-images"></a>Permiso para personalizar las imágenes existentes

Para que Azure Image Builder genere imágenes a partir de imágenes personalizadas de origen (imágenes administradas/galería de imágenes compartidas), se debe permitir que el servicio Azure Image Builder lea las imágenes en estos grupos de recursos. Para conceder los permisos necesarios, debe crear una identidad administrada asignada por el usuario y concederle derechos en el grupo de recursos donde se ubica la imagen.

Generación a partir de una imagen personalizada existente:

```Actions
Microsoft.Compute/galleries/read
```

Generación a partir de una versión existente de una galería de imágenes compartidas:

```Actions
Microsoft.Compute/galleries/read
Microsoft.Compute/galleries/images/read
Microsoft.Compute/galleries/images/versions/read
```

## <a name="permission-to-customize-images-on-your-vnets"></a>Permiso para personalizar imágenes en redes virtuales

Azure Image Builder tiene la capacidad de implementar y usar una red virtual existente en su suscripción, lo que permite que las personalizaciones tengan acceso a los recursos conectados.

No es necesario conceder derechos de colaborador de identidad administrada asignada por el usuario en el grupo de recursos para implementar una VM en una red virtual existente. Sin embargo, la identidad administrada asignada por el usuario necesita los siguientes permisos `Actions` de Azure en el grupo de recursos de red virtual:

```Actions
Microsoft.Network/virtualNetworks/read
Microsoft.Network/virtualNetworks/subnets/join/action
```

## <a name="create-an-azure-role-definition"></a>Creación de una definición de roles de Azure

En los ejemplos siguientes se crea una definición de roles de Azure a partir de las acciones descritas en las secciones anteriores. Los ejemplos se aplican en el nivel de grupo de recursos. Evalúe y pruebe si los ejemplos son lo suficientemente detallados para sus requisitos. Para su escenario, puede que necesite refinarlo a una galería de imágenes compartidas específica.

Las acciones en las imágenes permiten la lectura y escritura. Decida qué es adecuado para su entorno. Por ejemplo, cree un rol para permitir que Azure Image Builder lea imágenes del grupo de recursos *example-rg-1* y escriba imágenes en el grupo de recursos *example-rg-2*.

### <a name="custom-image-azure-role-example"></a>Ejemplo de rol de Azure de imagen personalizada

En el ejemplo siguiente se crea un rol de Azure para usar y distribuir una imagen personalizada de origen. A continuación, conceda el rol personalizado a la identidad administrada asignada por el usuario para Azure Image Builder.

Para simplificar la sustitución de valores en el ejemplo, establezca las siguientes variables primero. Reemplace la configuración del marcador de posición para establecer las variables.

| Configuración | Descripción |
|---------|-------------|
| \<Subscription ID\> | El identificador de suscripción a Azure |
| \<Resource group\> | Grupo de recursos para la imagen personalizada |

```azurecli-interactive
# Subscription ID - You can get this using `az account show | grep id` or from the Azure portal.
subscriptionID=$(az account show --query id --output tsv)
# Resource group - image builder will only support creating custom images in the same Resource Group as the source managed image.
imageResourceGroup=<Resource group>
identityName="aibIdentity"

# Use *cURL* to download the a sample JSON description 
curl https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleImageCreation.json -o aibRoleImageCreation.json

# Create a unique role name to avoid clashes in the same Azure Active Directory domain
imageRoleDefName="Azure Image Builder Image Def"$(date +'%s')

# Update the JSON definition using stream editor
sed -i -e "s/<subscriptionID>/$subscriptionID/g" aibRoleImageCreation.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" aibRoleImageCreation.json
sed -i -e "s/Azure Image Builder Service Image Creation Role/$imageRoleDefName/g" aibRoleImageCreation.json

# Create a custom role from the sample aibRoleImageCreation.json description file.
az role definition create --role-definition ./aibRoleImageCreation.json

# Get the user-assigned managed identity id
imgBuilderCliId=$(az identity show -g $sigResourceGroup -n $identityName --query clientId -o tsv)

# Grant the custom role to the user-assigned managed identity for Azure Image Builder.
az role assignment create \
    --assignee $imgBuilderCliId \
    --role $imageRoleDefName \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```

### <a name="existing-vnet-azure-role-example"></a>Ejemplo de rol de Azure de red virtual existente

En el ejemplo siguiente se crea un rol de Azure para usar y distribuir una imagen de red virtual existente. A continuación, conceda el rol personalizado a la identidad administrada asignada por el usuario para Azure Image Builder.

Para simplificar la sustitución de valores en el ejemplo, establezca las siguientes variables primero. Reemplace la configuración del marcador de posición para establecer las variables.

| Configuración | Descripción |
|---------|-------------|
| \<Subscription ID\> | El identificador de suscripción a Azure |
| \<Resource group\> | Grupos de recursos de red virtual |

```azurecli-interactive
# Subscription ID - You can get this using `az account show | grep id` or from the Azure portal.
subscriptionID=$(az account show --query id --output tsv)
VnetResourceGroup=<Resource group>
identityName="aibIdentity"

# Use *cURL* to download the a sample JSON description 
curl https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleNetworking.json -o aibRoleNetworking.json

# Create a unique role name to avoid clashes in the same domain
netRoleDefName="Azure Image Builder Network Def"$(date +'%s')

# Update the JSON definition using stream editor
sed -i -e "s/<subscriptionID>/$subscriptionID/g" aibRoleNetworking.json
sed -i -e "s/<vnetRgName>/$vnetRgName/g" aibRoleNetworking.json
sed -i -e "s/Azure Image Builder Service Networking Role/$netRoleDefName/g" aibRoleNetworking.json

# Create a custom role from the aibRoleNetworking.json description file.
az role definition create --role-definition ./aibRoleNetworking.json

# Get the user-assigned managed identity id
imgBuilderCliId=$(az identity show -g $sigResourceGroup -n $identityName --query clientId -o tsv)

# Grant the custom role to the user-assigned managed identity for Azure Image Builder.
az role assignment create \
    --assignee $imgBuilderCliId \
    --role $netRoleDefName \
    --scope /subscriptions/$subscriptionID/resourceGroups/$VnetResourceGroup
```

## <a name="using-managed-identity-for-azure-storage-access"></a>Uso de la identidad administrada para el acceso a Azure Storage

Si quiere autenticarse sin problemas en Azure Storage y usar contenedores privados, Azure Image Builder necesita una identidad administrada asignada por el usuario. Azure Image Builder usa la identidad para autenticarse en Azure Storage.

> [!NOTE]
> Azure Image Builder solo usa la identidad en el momento del envío de la plantilla de imagen. La VM de compilación no tiene acceso a la identidad durante la compilación de la imagen.

Use la CLI de Azure para crear la identidad administrada asignada por el usuario.

```azurecli
az role assignment create \
    --assignee <Image Builder client ID> \
    --role "Storage Blob Data Reader" \
    --scope /subscriptions/<Subscription ID>/resourceGroups/<Resource group>/providers/Microsoft.Storage/storageAccounts/$scriptStorageAcc/blobServices/default/containers/<Storage account container>
```

En la plantilla de Image Builder, debe proporcionar la identidad administrada asignada por el usuario.

```json
    "type": "Microsoft.VirtualMachineImages/imageTemplates",
    "apiVersion": "2020-02-14",
    "location": "<Region>",
    ..
    "identity": {
    "type": "UserAssigned",
          "userAssignedIdentities": {
            "<Image Builder ID>": {}     
        }
```

Reemplace la configuración del marcador de posición siguiente:

| Configuración | Descripción |
|---------|-------------|
| \<Region\> | Región de la plantilla |
| \<Resource group\> | Grupo de recursos |
| \<Storage account container\> | El nombre del contenedor de la cuenta de almacenamiento |
| \<Subscription ID\> | Suscripción de Azure |

Para obtener más información sobre el uso de una identidad administrada asignada por el usuario, consulte [Creación de una imagen personalizada que usará una identidad administrada asignada por el usuario de Azure para tener acceso sin problemas a los archivos de Azure Storage](./image-builder-user-assigned-identity.md). En la guía de inicio rápido se explica cómo crear y configurar la identidad administrada asignada por el usuario para acceder a una cuenta de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea [Introducción a Azure Image Builder](../image-builder-overview.md).
