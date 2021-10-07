---
title: 'Plantillas de implementación de Azure con la CLI de Azure : Azure Resource Manager | Microsoft Docs'
description: Use Azure Resource Manager y la CLI de Azure para crear e implementar grupos de recursos en Azure. Los recursos se definen en una plantilla de implementación de Azure.
ms.topic: conceptual
ms.date: 09/17/2021
ms.custom: devx-track-azurecli, seo-azure-cli
keywords: cli azure implementar plantilla arm, crear grupo recursos azure, plantilla implementación azure, recursos implementación, plantilla arm, plantilla azure arm
ms.openlocfilehash: 5b7734e3b91f7e842f17888f3f3c67d05655fc97
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128585021"
---
# <a name="how-to-use-azure-resource-manager-arm-deployment-templates-with-azure-cli"></a>Procedimientos para el uso de plantillas de implementación de Azure Resource Manager (ARM) con la CLI de Azure

En este artículo, se explica el uso de la CLI de Azure con plantillas de Azure Resource Manager (ARM) para implementar los recursos en Azure.  Si no está familiarizado con los conceptos de implementación y administración de las soluciones de Azure, vea [Información general sobre plantillas](overview.md).

Los comandos de implementación cambiaron en la CLI de Azure, versión 2.2.0. Los ejemplos de este artículo requieren la [CLI de Azure, versión 2.20.0 o posterior](/cli/azure/install-azure-cli).

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

Si no tiene instalada la CLI de Azure, puede usar Azure Cloud Shell. Para obtener más información, vea [Implementación de plantillas de Resource Manager desde Cloud Shell](deploy-cloud-shell.md).

## <a name="deployment-scope"></a>Ámbito de la implementación

La plantilla de implementación de Azure puede tener como destino un grupo de recursos, una suscripción, un grupo de administración o un inquilino. Según el ámbito de la implementación, usará comandos diferentes.

* Para implementar en un **grupo de recursos**, use [az deployment group create](/cli/azure/deployment/group#az_deployment_group_create):

  ```azurecli-interactive
  az deployment group create --resource-group <resource-group-name> --template-file <path-to-template>
  ```

* Para implementar en una **suscripción**, use [az deployment sub create](/cli/azure/deployment/sub#az_deployment_sub_create):

  ```azurecli-interactive
  az deployment sub create --location <location> --template-file <path-to-template>
  ```

  Para más información sobre las implementaciones en el nivel de suscripción, consulte [Creación de grupos de recursos y otros recursos en el nivel de suscripción](deploy-to-subscription.md).

* Para implementar en un **grupo de administración**, use [az deployment mg create](/cli/azure/deployment/mg#az_deployment_mg_create):

  ```azurecli-interactive
  az deployment mg create --location <location> --template-file <path-to-template>
  ```

  Para obtener más información sobre las implementaciones de nivel de grupo de administración, consulte [Creación de recursos en el nivel de grupo de administración](deploy-to-management-group.md).

* Para implementar en un **inquilino**, use [az deployment tenant create](/cli/azure/deployment/tenant#az_deployment_tenant_create):

  ```azurecli-interactive
  az deployment tenant create --location <location> --template-file <path-to-template>
  ```

  Para obtener más información sobre las implementaciones a nivel de inquilino, consulte [Creación de recursos en el nivel de inquilino](deploy-to-tenant.md).

Para cada ámbito, el usuario que implementa la plantilla debe tener permisos para crear recursos.

## <a name="deploy-local-template"></a>Implementar una plantilla local

Puede implementar una plantilla de ARM desde la máquina local o una que esté almacenada externamente. En esta sección se describe la implementación de una plantilla local.

Si va a realizar la implementación en un grupo de recursos que no existe, cree el grupo de recursos. El nombre del grupo de recursos solo puede incluir caracteres alfanuméricos, puntos, guiones bajos, guiones y paréntesis. Puede tener hasta 90 caracteres. El nombre no puede terminar con un punto.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
```

Para implementar una plantilla local, use el parámetro `--template-file` en el comando de implementación. En el ejemplo siguiente también se muestra cómo establecer un valor de parámetro.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file <path-to-template> \
  --parameters storageAccountType=Standard_GRS
```

La plantilla de implementación de Azure puede tardar unos minutos en completarse. Cuando termine, verá un mensaje que incluye el resultado:

```output
"provisioningState": "Succeeded",
```

## <a name="deploy-remote-template"></a>Implementación de una plantilla remota

En lugar de almacenar las plantillas de ARM en el equipo local, quizás prefiera almacenarlas en una ubicación externa. Puede almacenar plantillas en un repositorio de control de código fuente (por ejemplo, GitHub). O bien, puede almacenarlas en una cuenta de Azure Storage para el acceso compartido en su organización.

[!INCLUDE [Deploy templates in private GitHub repo](../../../includes/resource-manager-private-github-repo-templates.md)]

Si va a realizar la implementación en un grupo de recursos que no existe, cree el grupo de recursos. El nombre del grupo de recursos solo puede incluir caracteres alfanuméricos, puntos, guiones bajos, guiones y paréntesis. Puede tener hasta 90 caracteres. El nombre no puede terminar con un punto.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
```

Para implementar una plantilla externa, use el parámetro `template-uri`.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.storage/storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

En el ejemplo anterior, se requiere un identificador URI accesible públicamente para la plantilla, que funciona con la mayoría de los escenarios porque la plantilla no debe incluir datos confidenciales. Si tiene que especificar datos confidenciales (por ejemplo, una contraseña de administrador), pase ese valor como un parámetro seguro. Sin embargo, si quiere administrar el acceso a la plantilla, considere la posibilidad de usar [especificaciones de plantilla](#deploy-template-spec).

Para implementar plantillas vinculadas remotas con una ruta de acceso relativa que estén almacenadas en una cuenta de almacenamiento, use `query-string` para especificar el token de SAS:

```azurecli-interactive
az deployment group create \
  --name linkedTemplateWithRelativePath \
  --resource-group myResourceGroup \
  --template-uri "https://stage20210126.blob.core.windows.net/template-staging/mainTemplate.json" \
  --query-string $sasToken
```

Para obtener más información, vea [Uso de rutas de acceso relativas para plantillas vinculadas](./linked-templates.md#linked-template).

## <a name="azure-deployment-template-name"></a>Nombre de la plantilla de implementación de Azure

Al implementar una plantilla de ARM, puede asignarle un nombre a la plantilla de implementación de Azure. Este nombre puede ayudarle a recuperar la implementación del historial de implementaciones. Si no especifica un nombre para la implementación, se utilizará el nombre del archivo de la plantilla. Por ejemplo, si implementa una plantilla llamada _azuredeploy.json_ y no especifica ningún nombre para la implementación, el nombre que se asignará será `azuredeploy`.

Cada vez que se ejecuta una implementación, se agrega una entrada al historial de implementación del grupo de recursos con el nombre de la implementación. Si ejecuta otra implementación y le asigna el mismo nombre, la entrada anterior se reemplazará por la implementación actual. Si desea que todas las entradas del historial de implementaciones sean diferentes, asigne un nombre único a cada implementación.

Para crear un nombre único, puede asignar un número aleatorio.

```azurecli-interactive
deploymentName='ExampleDeployment'$RANDOM
```

También puede agregar un valor de fecha.

```azurecli-interactive
deploymentName='ExampleDeployment'$(date +"%d-%b-%Y")
```

Si ejecuta implementaciones simultáneas en el mismo grupo de recursos utilizando el mismo nombre de implementación, solo se completará la última implementación. Aquellas implementaciones que tengan el mismo nombre y no hayan finalizado se sustituirán por la última implementación. Por ejemplo, si ejecuta una implementación llamada `newStorage` que implementa la cuenta de almacenamiento `storage1` y, al mismo tiempo, ejecuta otra implementación llamada `newStorage` que implementa la cuenta de almacenamiento `storage2`, solo se implementará una única cuenta de almacenamiento. La cuenta de almacenamiento resultante será `storage2`.

Sin embargo, si ejecuta una implementación llamada `newStorage` que implementa la cuenta de almacenamiento `storage1` e inmediatamente después ejecuta otra implementación llamada `newStorage` que implementa la cuenta de almacenamiento `storage2`, tendrá dos cuentas de almacenamiento. Una se llamará `storage1` y la otra, `storage2`. Sin embargo, solo tendrá una entrada en el historial de implementaciones.

Si especifica un nombre único para cada implementación, podrá ejecutarlas simultáneamente sin conflictos. Si ejecuta una implementación llamada `newStorage1` que implementa la cuenta de almacenamiento `storage1` y, al mismo tiempo, ejecuta otra implementación llamada `newStorage2` que implementa la cuenta de almacenamiento `storage2`, tendrá dos cuentas de almacenamiento y dos entradas en el historial de implementación.

Para evitar conflictos con las implementaciones simultáneas y garantizar que las entradas del historial de implementaciones son únicas, asigne un nombre diferente a cada implementación.

## <a name="deploy-template-spec"></a>Implementación de la especificación de plantilla

En lugar de implementar una plantilla local o remota, puede crear una [especificación de plantilla](template-specs.md). La especificación de plantilla es un recurso de su suscripción de Azure que contiene una plantilla de ARM. Facilita el uso compartido de la plantilla de forma segura con los usuarios de la organización. Use el control de acceso basado en rol de Azure (RBAC de Azure) para conceder acceso a la especificación de la plantilla. Esta funcionalidad actualmente está en su versión preliminar.

En los ejemplos siguientes se muestra cómo se crea e implementa una especificación de plantilla.

En primer lugar, proporcione la plantilla de Resource Manager para crear la especificación de plantilla.

```azurecli
az ts create \
  --name storageSpec \
  --version "1.0" \
  --resource-group templateSpecRG \
  --location "westus2" \
  --template-file "./mainTemplate.json"
```

A continuación, obtenga el identificador de la especificación de plantilla e impleméntelo.

```azurecli
id = $(az ts show --name storageSpec --resource-group templateSpecRG --version "1.0" --query "id")

az deployment group create \
  --resource-group demoRG \
  --template-spec $id
```

Para más información, vea [Especificaciones de plantilla de Azure Resource Manager](template-specs.md).

## <a name="preview-changes"></a>Vista previa de los cambios

Antes de implementar la plantilla de ARM, puede obtener una vista previa de los cambios que la plantilla realizará en su entorno. Use la [operación Y si](./deploy-what-if.md) para comprobar que la plantilla realiza los cambios esperados. La operación y si también valida que la plantilla no tenga errores.

## <a name="parameters"></a>Parámetros

Para pasar valores de parámetros, puede usar parámetros en línea o un archivo de parámetros.

### <a name="inline-parameters"></a>Parámetros en línea

Para pasar parámetros en línea, proporcione los valores en `parameters`. Por ejemplo, para pasar una cadena y una matriz a una plantilla en un shell de Bash, use:

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file <path-to-template> \
  --parameters exampleString='inline string' exampleArray='("value1", "value2")'
```

Si usa la CLI de Azure con el símbolo del sistema de Windows (CMD) o PowerShell, pase la matriz con el formato `exampleArray="['value1','value2']"`.

También puede obtener el contenido del archivo y proporcionar ese contenido como un parámetro en línea.

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file <path-to-template> \
  --parameters exampleString=@stringContent.txt exampleArray=@arrayContent.json
```

Obtener un valor de parámetro de un archivo es útil cuando se necesita proporcionar valores de configuración. Por ejemplo, puede proporcionar [valores de cloud-init para una máquina virtual Linux](../../virtual-machines/linux/using-cloud-init.md).

El formato de _arrayContent.json_ es:

```json
[
    "value1",
    "value2"
]
```

Para pasar un objeto, por ejemplo, para establecer etiquetas, use JSON. Por ejemplo, la plantilla podría incluir un parámetro como este:

```json
    "resourceTags": {
      "type": "object",
      "defaultValue": {
        "Cost Center": "IT Department"
      }
    }
```

En este caso, puede pasar una cadena JSON para establecer el parámetro como se muestra en el siguiente script de Bash:

```bash
tags='{"Owner":"Contoso","Cost Center":"2345-324"}'
az deployment group create --name addstorage  --resource-group myResourceGroup \
--template-file $templateFile \
--parameters resourceName=abcdef4556 resourceTags="$tags"
```

Use comillas dobles alrededor del código JSON que desee pasar al objeto.

Puede usar una variable para contener los valores de parámetro. En Bash, establezca la variable en todos los valores de parámetro y agréguela al comando de implementación.

```azurecli-interactive
params="prefix=start suffix=end"

az deployment group create \
  --resource-group testgroup \
  --template-file <path-to-template> \
  --parameters $params
``` 

Sin embargo, si usa CLI de Azure con el símbolo del sistema (CMD) de Windows o PowerShell, establezca la variable en una cadena JSON. Incluya las comillas: `$params = '{ \"prefix\": {\"value\":\"start\"}, \"suffix\": {\"value\":\"end\"} }'`.

### <a name="parameter-files"></a>Archivos de parámetros

En lugar de pasar parámetros como valores en línea en el script, quizá le resulte más fácil usar un archivo JSON que contiene los valores de parámetro. El archivo de parámetros debe ser un archivo local. No se admiten los archivos de parámetros externos con la CLI de Azure.

Para más información sobre el archivo de parámetro, consulte [Creación de un archivo de parámetros de Resource Manager](parameter-files.md).

Para pasar un archivo de parámetros local, use `@` para especificar un archivo local denominado _storage.parameters.json_.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters '@storage.parameters.json'
```

## <a name="handle-extended-json-format"></a>Control del formato JSON extendido

Para poder implementar una plantilla con cadenas o comentarios multilínea utilizando la versión 2.3.0 u otra anterior de la CLI de Azure, debe usar el conmutador `--handle-extended-json-format`.  Por ejemplo:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

## <a name="next-steps"></a>Pasos siguientes

* Para revertir a una implementación correcta cuando se produce un error, consulte [Revertir en caso de error a una implementación correcta](rollback-on-error.md).
* Para especificar cómo controlar los recursos que existen en el grupo de recursos, pero que no están definidos en la plantilla, consulte [Modos de implementación de Azure Resource Manager](deployment-modes.md).
* Para entender cómo definir parámetros en la plantilla, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](./syntax.md).
* Para obtener sugerencias para resolver los errores de implementación más comunes, consulte [Solución de errores comunes de implementación de Azure con Azure Resource Manager](common-deployment-errors.md).