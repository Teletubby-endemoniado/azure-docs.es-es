---
title: Creación de una máquina virtual Windows con Azure Image Builder
description: Cree una máquina virtual Windows con Azure Image Builder.
author: kof-f
ms.author: kofiforson
ms.reviewer: cynthn
ms.date: 04/23/2021
ms.topic: how-to
ms.service: virtual-machines
ms.subervice: image-builder
ms.colletion: windows
ms.openlocfilehash: 256ae289b86ec2c16b850f16d379700dc69c2f6f
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123424643"
---
# <a name="create-a-windows-vm-with-azure-image-builder"></a>Creación de una máquina virtual Windows con Azure Image Builder

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Windows 

En este artículo se muestra cómo puede crear una imagen personalizada de Windows mediante el generador de imágenes de máquina virtual de Azure. En el ejemplo de este artículo se usan [personalizadores](../linux/image-builder-json.md#properties-customize) para personalizar la imagen:
- PowerShell (ScriptUri): para descargar y ejecutar un [script de PowerShell](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/testPsScript.ps1).
- Reinicio de Windows: reinicia la máquina virtual.
- PowerShell (insertado) ejecuta un comando específico. En este ejemplo, se crea un directorio en la máquina virtual mediante `mkdir c:\\buildActions`.
- Archivo: para copiar un archivo de GitHub en la máquina virtual. En este ejemplo se copia [index.md](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/exampleArtifacts/buildArtifacts/index.html) en `c:\buildArtifacts\index.html` en la máquina virtual.
- buildTimeoutInMinutes: aumente el tiempo de compilación para permitir compilaciones de larga duración (el valor predeterminado es de 240 minutos) y podrá aumentar el tiempo de compilación para permitir compilaciones de ejecución de mayor duración.
- vmProfile: especificación de las propiedades vmSize y Network.
- osDiskSizeGB: puede aumentar el tamaño de la imagen.
- identity: proporcione una identidad para que Azure Image Builder la use durante la compilación.


También puede especificar un valor de `buildTimeoutInMinutes`. El valor predeterminado es de 240 minutos, y puede aumentar el tiempo de compilación para permitir compilaciones de larga duración. El valor mínimo permitido es 6 minutos; los valores más cortos provocarán errores.

Se usará una plantilla .json de ejemplo para configurar la imagen. El archivo .json que se usa aquí es: [helloImageTemplateWin.json](https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json). 



> [!NOTE]
> En el caso de los usuarios de Windows, los ejemplos de la CLI de Azure siguientes se pueden ejecutar en [Azure Cloud Shell](https://shell.azure.com) mediante Bash.


## <a name="register-the-features"></a>Registro de las características

Para usar Azure Image Builder, debe registrar la característica.

Compruebe el registro.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState
az provider show -n Microsoft.KeyVault | grep registrationState
az provider show -n Microsoft.Compute | grep registrationState
az provider show -n Microsoft.Storage | grep registrationState
az provider show -n Microsoft.Network | grep registrationState
```

Si no se muestran como registradas, ejecute lo siguiente:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Compute
az provider register -n Microsoft.KeyVault
az provider register -n Microsoft.Storage
az provider register -n Microsoft.Network
```


## <a name="set-variables"></a>Configuración de variables

Usaremos algunos datos de forma repetida, por lo que crearemos diversas variables para almacenar esa información.


```azurecli-interactive
# Resource group name - we are using myImageBuilderRG in this example
imageResourceGroup=myWinImgBuilderRG
# Region location 
location=WestUS2
# Name for the image 
imageName=myWinBuilderImage
# Run output name
runOutputName=aibWindows
# name of the image to be created
imageName=aibWinImage
```

Cree una variable para el id. de suscripción.

```azurecli-interactive
subscriptionID=$(az account show --query id --output tsv)
```
## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Este grupo de recursos se usa para almacenar el artefacto de la plantilla de configuración de la imagen y la imagen misma.


```azurecli-interactive
az group create -n $imageResourceGroup -l $location
```

## <a name="create-a-user-assigned-identity-and-set-permissions-on-the-resource-group"></a>Creación de una identidad asignada por el usuario y establecimiento de los permisos en el grupo de recursos
Image Builder usará la [identidad de usuario](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#user-assigned-managed-identity) proporcionada para insertar la imagen en el grupo de recursos. En este ejemplo, se creará una definición de roles de Azure que tiene las acciones granulares necesarias para realizar la distribución de la imagen. La definición de roles se asignará a la identidad del usuario.

## <a name="create-user-assigned-managed-identity-and-grant-permissions"></a>Creación de una identidad administrada asignada por el usuario y concesión de permisos 
```bash
# create user assigned identity for image builder to access the storage account where the script is located
identityName=aibBuiUserId$(date +'%s')
az identity create -g $imageResourceGroup -n $identityName

# get identity id
imgBuilderCliId=$(az identity show -g $imageResourceGroup -n $identityName --query clientId -o tsv)

# get the user identity URI, needed for the template
imgBuilderId=/subscriptions/$subscriptionID/resourcegroups/$imageResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/$identityName

# download preconfigured role definition example
curl https://raw.githubusercontent.com/azure/azvmimagebuilder/master/solutions/12_Creating_AIB_Security_Roles/aibRoleImageCreation.json -o aibRoleImageCreation.json

imageRoleDefName="Azure Image Builder Image Def"$(date +'%s')

# update the definition
sed -i -e "s/<subscriptionID>/$subscriptionID/g" aibRoleImageCreation.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" aibRoleImageCreation.json
sed -i -e "s/Azure Image Builder Service Image Creation Role/$imageRoleDefName/g" aibRoleImageCreation.json

# create role definitions
az role definition create --role-definition ./aibRoleImageCreation.json

# grant role definition to the user assigned identity
az role assignment create \
    --assignee $imgBuilderCliId \
    --role "$imageRoleDefName" \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup
```



## <a name="download-the-image-configuration-template-example"></a>Descargar el ejemplo de plantilla de configuración de imagen

Se ha creado una plantilla de configuración de imagen con parámetros para que pueda probarla. Descargue el archivo .json de ejemplo y configúrelo con las variables que estableció anteriormente.

```azurecli-interactive
curl https://raw.githubusercontent.com/azure/azvmimagebuilder/master/quickquickstarts/0_Creating_a_Custom_Windows_Managed_Image/helloImageTemplateWin.json -o helloImageTemplateWin.json

sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateWin.json
sed -i -e "s/<rgName>/$imageResourceGroup/g" helloImageTemplateWin.json
sed -i -e "s/<region>/$location/g" helloImageTemplateWin.json
sed -i -e "s/<imageName>/$imageName/g" helloImageTemplateWin.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateWin.json
sed -i -e "s%<imgBuilderId>%$imgBuilderId%g" helloImageTemplateWin.json

```

Puede modificar este ejemplo en el terminal con un editor de texto, como `vi`.

```azurecli-interactive
vi helloImageTemplateWin.json
```

> [!NOTE]
> Para la imagen de origen, siempre debe [especificar una versión](../linux/image-builder-troubleshoot.md#build--step-failed-for-image-version); no puede usar `latest`.
> Si agrega o cambia el grupo de recursos donde se distribuye la imagen, tiene que asegurarse de que los [permisos estén establecidos](#create-a-user-assigned-identity-and-set-permissions-on-the-resource-group) en el grupo de recursos.
 
## <a name="create-the-image"></a>Crear la imagen

Envío de la configuración de la imagen al servicio de generador de imágenes de máquina virtual

```azurecli-interactive
az resource create \
    --resource-group $imageResourceGroup \
    --properties @helloImageTemplateWin.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

Cuando haya finalizado, devolverá un mensaje de confirmación a la consola y creará `Image Builder Configuration Template` en `$imageResourceGroup`. Puede ver este recurso en el grupo de recursos en Azure Portal si habilita "Mostrar tipos ocultos".

Asimismo, Image Builder creará en segundo plano un grupo de recursos de almacenamiento provisional en la suscripción. Este grupo de recursos se usa para la compilación de la imagen. Tendrá este formato: `IT_<DestinationResourceGroup>_<TemplateName>`

> [!Note]
> No debe eliminar el grupo de recursos de almacenamiento provisional directamente. Primero elimine el artefacto de la plantilla de imagen, esto hará que se elimine el grupo de recursos de almacenamiento provisional.

Si el servicio informa de un error durante el envío de la plantilla de configuración de la imagen:
-  Revise los pasos para [solucionar problemas](../linux/image-builder-troubleshoot.md#troubleshoot-image-template-submission-errors). 
- Tendrá que eliminar la plantilla con el siguiente fragmento de código antes de reintentar el envío.

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

## <a name="start-the-image-build"></a>Iniciar la generación de imágenes
Inicie el proceso de compilación de la imagen con [az resource invoke-action](/cli/azure/resource#az_resource_invoke_action).

```azurecli-interactive
az resource invoke-action \
     --resource-group $imageResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateWin01 \
     --action Run 
```

Espere hasta que se complete la compilación. Puede tardar unos 15 minutos.

Si encuentra algún error, revise estos pasos para la [solución de problemas](../linux/image-builder-troubleshoot.md#troubleshoot-common-build-errors).


## <a name="create-the-vm"></a>Creación de la máquina virtual

Cree la máquina virtual con la imagen que ha compilado. Reemplace *\<password>* con su propia contraseña para `aibuser` en la máquina virtual.

```azurecli-interactive
az vm create \
  --resource-group $imageResourceGroup \
  --name aibImgWinVm00 \
  --admin-username aibuser \
  --admin-password <password> \
  --image $imageName \
  --location $location
```

## <a name="verify-the-customization"></a>Comprobación de la personalización

Cree una conexión de Escritorio remoto a la máquina virtual con el nombre de usuario y la contraseña que ha establecido al crear la máquina virtual. Dentro de la máquina virtual, abra un símbolo del sistema y escriba lo siguiente:

```console
dir c:\
```

Debería ver estos dos directorios creados durante la personalización de la imagen:
- buildActions
- buildArtifacts

## <a name="clean-up"></a>Limpieza

Cuando haya terminado, elimine los recursos.

### <a name="delete-the-image-builder-template"></a>Eliminar la plantilla de Image Builder

```azurecli-interactive
az resource delete \
    --resource-group $imageResourceGroup \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateWin01
```

### <a name="delete-the-role-assignment-role-definition-and-user-identity"></a>Elimine la asignación de roles, la definición de roles y la identidad de usuario.
```azurecli-interactive
az role assignment delete \
    --assignee $imgBuilderCliId \
    --role "$imageRoleDefName" \
    --scope /subscriptions/$subscriptionID/resourceGroups/$imageResourceGroup

az role definition delete --name "$imageRoleDefName"

az identity delete --ids $imgBuilderId
```

### <a name="delete-the-image-resource-group"></a>Eliminar el grupo de recursos de imagen

```azurecli-interactive
az group delete -n $imageResourceGroup
```


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los componentes del archivo .json que se usan en este artículo, vea [Referencia de la plantilla de generador de imágenes](../linux/image-builder-json.md).
