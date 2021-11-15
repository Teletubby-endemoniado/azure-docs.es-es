---
title: Configuración de permisos del servicio Azure Image Builder mediante PowerShell
description: Configure los requisitos para el servicio Azure VM Image Builder, incluidos los permisos y privilegios mediante PowerShell.
author: kof-f
ms.author: kofiforson
ms.reviewer: cynthn
ms.date: 03/05/2021
ms.topic: article
ms.service: virtual-machines
ms.subservice: image-builder
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e310a12248ab0f17c66de2561e090125cbec392e
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131444542"
---
# <a name="configure-azure-image-builder-service-permissions-using-powershell"></a>Configuración de permisos del servicio Azure Image Builder mediante PowerShell

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Conjuntos de escalado flexibles 

Al registrarse para el (AIB), se concede al servicio AIB permiso para crear, administrar y eliminar un grupo de recursos de almacenamiento provisional (IT_ *) y tener derechos para agregarle recursos, que son necesarios para la compilación de la imagen. Esto se realiza mediante un nombre de entidad de seguridad de servicio (SPN) AIB que se pone a disposición de la suscripción durante un registro correcto.

Para permitir que Azure VM Image Builder distribuya imágenes a las imágenes administradas o a Azure Compute Gallery (anteriormente conocido como Shared Image Gallery), debe crear una identidad asignada por el usuario de Azure que tenga permisos para leer y escribir imágenes. Si va a acceder a Azure Storage, necesitará permisos para leer contenedores privados y públicos.

Debe configurar los permisos y los privilegios antes de crear una imagen. En las secciones siguientes se detalla cómo configurar posibles escenarios mediante PowerShell.

## <a name="create-an-azure-user-assigned-managed-identity"></a>Creación de una identidad administrada de Azure asignada por el usuario

Azure Image Builder requiere la creación de una [identidad administrada de Azure asignada por el usuario](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md). Azure Image Builder usa la identidad administrada asignada por el usuario para leer imágenes, escribir imágenes y acceder a las cuentas de Azure Storage. El permiso de identidad se concede para realizar acciones específicas en una suscripción.

> [!NOTE]
> Anteriormente, Azure Image Builder usaba el nombre de entidad de seguridad de servicio (SPN) de Azure Image Builder para conceder permisos a los grupos de recursos de imagen. El uso del SPN caerá en desuso. Use una identidad administrada asignada por el usuario en su lugar.

En el ejemplo siguiente se muestra cómo crear una identidad administrada de Azure asignada por el usuario. Reemplace la configuración del marcador de posición para establecer las variables.

| Configuración | Descripción |
|---------|-------------|
| \<Resource group\> | Grupo de recursos donde se creará la identidad administrada asignada por el usuario. |

```powershell-interactive
## Add AZ PS module to support AzUserAssignedIdentity
Install-Module -Name Az.ManagedServiceIdentity

$parameters = @{
    Name = 'aibIdentity'
    ResourceGroupName = '<Resource group>'
}
# create identity
New-AzUserAssignedIdentity @parameters
```

Para obtener más información sobre las identidades de Azure asignadas por el usuario, consulte la documentación de la [Identidad administrada de Azure asignada por el usuario](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) sobre cómo crear una identidad.

## <a name="allow-image-builder-to-distribute-images"></a>Permiso para que Image Builder distribuya imágenes

Para que Azure Image Builder distribuya imágenes (imágenes administradas/Azure Compute Gallery), se debe permitir que el servicio Azure Image Builder inserte las imágenes en estos grupos de recursos. Para conceder los permisos necesarios, debe crear una identidad administrada asignada por el usuario y concederle derechos en el grupo de recursos donde se genera la imagen. Azure Image Builder **no** tiene permiso para acceder a los recursos de otros grupos de recursos de la suscripción. Debe realizar acciones explícitas para permitir el acceso para evitar errores en la creación.

No es necesario conceder derechos de colaborador de identidad administrada asignada por el usuario en el grupo de recursos para distribuir las imágenes. Sin embargo, la identidad administrada asignada por el usuario necesita los siguientes permisos `Actions` de Azure en el grupo de recursos de distribución:

```Actions
Microsoft.Compute/images/write
Microsoft.Compute/images/read
Microsoft.Compute/images/delete
```

Si se distribuye a una instancia de Azure Compute Gallery, también necesita:

```Actions
Microsoft.Compute/galleries/read
Microsoft.Compute/galleries/images/read
Microsoft.Compute/galleries/images/versions/read
Microsoft.Compute/galleries/images/versions/write
```

## <a name="permission-to-customize-existing-images"></a>Permiso para personalizar las imágenes existentes

Para que Azure Image Builder genere imágenes a partir de imágenes personalizadas de origen (imágenes administradas/Azure Compute Gallery), se debe permitir que el servicio Azure Image Builder lea las imágenes en estos grupos de recursos. Para conceder los permisos necesarios, debe crear una identidad administrada asignada por el usuario y concederle derechos en el grupo de recursos donde se ubica la imagen.

Generación a partir de una imagen personalizada existente:

```Actions
Microsoft.Compute/galleries/read
```

Compilación a partir de una versión existente de Azure Compute Gallery:

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

En los ejemplos siguientes se crea una definición de roles de Azure a partir de las acciones descritas en las secciones anteriores. Los ejemplos se aplican en el nivel de grupo de recursos. Evalúe y pruebe si los ejemplos son lo suficientemente detallados para sus requisitos. En el caso de su escenario, puede que necesite reformularlo para una instancia específica de Azure Compute Gallery.

Las acciones en las imágenes permiten la lectura y escritura. Decida qué es adecuado para su entorno. Por ejemplo, cree un rol para permitir que Azure Image Builder lea imágenes del grupo de recursos *example-rg-1* y escriba imágenes en el grupo de recursos *example-rg-2*.

### <a name="custom-image-azure-role-example"></a>Ejemplo de rol de Azure de imagen personalizada

En el ejemplo siguiente se crea un rol de Azure para usar y distribuir una imagen personalizada de origen. A continuación, conceda el rol personalizado a la identidad administrada asignada por el usuario para Azure Image Builder.

Para simplificar la sustitución de valores en el ejemplo, establezca las siguientes variables primero. Reemplace la configuración del marcador de posición para establecer las variables.

| Configuración | Descripción |
|---------|-------------|
| \<Subscription ID\> | El identificador de suscripción a Azure |
| \<Resource group\> | Grupo de recursos para la imagen personalizada |

```powershell-interactive
$sub_id = "<Subscription ID>"
# Resource group - image builder will only support creating custom images in the same Resource Group as the source managed image.
$imageResourceGroup = "<Resource group>"
$identityName = "aibIdentity"

# Use a web request to download the sample JSON description
$sample_uri="https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleImageCreation.json"
$role_definition="aibRoleImageCreation.json"

Invoke-WebRequest -Uri $sample_uri -Outfile $role_definition -UseBasicParsing

# Create a unique role name to avoid clashes in the same Azure Active Directory domain
$timeInt=$(get-date -UFormat "%s")
$imageRoleDefName="Azure Image Builder Image Def"+$timeInt

# Update the JSON definition placeholders with variable values
((Get-Content -path $role_definition -Raw) -replace '<subscriptionID>',$sub_id) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace '<rgName>', $imageResourceGroup) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace 'Azure Image Builder Service Image Creation Role', $imageRoleDefName) | Set-Content -Path $role_definition

# Create a custom role from the aibRoleImageCreation.json description file. 
New-AzRoleDefinition -InputFile $role_definition

# Get the user-identity properties
$identityNameResourceId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).Id
$identityNamePrincipalId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).PrincipalId

# Grant the custom role to the user-assigned managed identity for Azure Image Builder.
$parameters = @{
    ObjectId = $identityNamePrincipalId
    RoleDefinitionName = $imageRoleDefName
    Scope = '/subscriptions/' + $sub_id + '/resourceGroups/' + $imageResourceGroup
}

New-AzRoleAssignment @parameters
```

### <a name="existing-vnet-azure-role-example"></a>Ejemplo de rol de Azure de red virtual existente

En el ejemplo siguiente se crea un rol de Azure para usar y distribuir una imagen de red virtual existente. A continuación, conceda el rol personalizado a la identidad administrada asignada por el usuario para Azure Image Builder.

Para simplificar la sustitución de valores en el ejemplo, establezca las siguientes variables primero. Reemplace la configuración del marcador de posición para establecer las variables.

| Configuración | Descripción |
|---------|-------------|
| \<Subscription ID\> | El identificador de suscripción a Azure |
| \<Resource group\> | Grupos de recursos de red virtual |

```powershell-interactive
$sub_id = "<Subscription ID>"
$res_group = "<Resource group>"
$identityName = "aibIdentity"

# Use a web request to download the sample JSON description
$sample_uri="https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleNetworking.json"
$role_definition="aibRoleNetworking.json"

Invoke-WebRequest -Uri $sample_uri -Outfile $role_definition -UseBasicParsing

# Create a unique role name to avoid clashes in the same AAD domain
$timeInt=$(get-date -UFormat "%s")
$networkRoleDefName="Azure Image Builder Network Def"+$timeInt

# Update the JSON definition placeholders with variable values
((Get-Content -path $role_definition -Raw) -replace '<subscriptionID>',$sub_id) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace '<vnetRgName>', $res_group) | Set-Content -Path $role_definition
((Get-Content -path $role_definition -Raw) -replace 'Azure Image Builder Service Networking Role',$networkRoleDefName) | Set-Content -Path $role_definition

# Create a custom role from the aibRoleNetworking.json description file
New-AzRoleDefinition -InputFile $role_definition

# Get the user-identity properties
$identityNameResourceId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).Id
$identityNamePrincipalId=$(Get-AzUserAssignedIdentity -ResourceGroupName $imageResourceGroup -Name $identityName).PrincipalId

# Assign the custom role to the user-assigned managed identity for Azure Image Builder
$parameters = @{
    ObjectId = $identityNamePrincipalId
    RoleDefinitionName = $networkRoleDefName
    Scope = '/subscriptions/' + $sub_id + '/resourceGroups/' + $res_group
}

New-AzRoleAssignment @parameters
```

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea [Introducción a Azure Image Builder](../image-builder-overview.md).
