---
title: 'Inicio rápido: Creación de un registro (PowerShell)'
description: Aprenda rápidamente a crear un registro privado de Docker en Azure Container Registry con PowerShell
ms.date: 06/03/2021
ms.topic: quickstart
ms.custom: devx-track-azurepowershell, mvc, mode-api
ms.openlocfilehash: 448b692adb603c884f1034bf5d7234e459ce5f66
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131043497"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Inicio rápido: Creación de un registro de contenedor privado con Azure PowerShell

Azure Container Registry es un servicio de registro privado para compilar, almacenar y proporcionar imágenes de contenedor y artefactos relacionados. En este inicio rápido, creará una instancia de Azure Container Registry con Azure PowerShell. A continuación, utilice los comandos de Docker para insertar una imagen de contenedor en el registro y, finalmente, extraiga y ejecute la imagen desde el registro.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Este inicio rápido requiere el módulo Azure PowerShell. Ejecute `Get-Module -ListAvailable Az` para determinar la versión instalada. Si necesita instalarla o actualizarla, consulte el artículo sobre [cómo instalar el módulo de Azure PowerShell](/powershell/azure/install-az-ps).

También debe tener instalado Docker localmente. Docker ofrece paquetes para los sistemas [macOS][docker-mac], [Windows][docker-windows] y [Linux][docker-linux].

Dado que Azure Cloud Shell no incluye todos los componentes necesarios de Docker (como por ejemplo el demonio `dockerd`), no se puede usar Cloud Shell en este tutorial de inicio rápido.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en la suscripción de Azure con el comando [Connect-AzAccount][Connect-AzAccount] y siga las instrucciones de la pantalla.

```powershell
Connect-AzAccount
```

## <a name="create-resource-group"></a>Creación de un grupo de recursos

Una vez se haya autenticado con Azure, debe crear un grupo de recursos con [New-AzResourceGroup][New-AzResourceGroup]. Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

```powershell
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Crear un registro de contenedor

A continuación, cree un registro de contenedor en el nuevo grupo de recursos mediante el comando [New-AzContainerRegistry][New-AzContainerRegistry].

El nombre del registro debe ser único dentro de Azure y contener entre 5 y 50 caracteres alfanuméricos. En el ejemplo siguiente se crea un registro con el nombre "myContainerRegistry007". Reemplace *myContainerRegistry007* en el siguiente comando y, a continuación, ejecútelo para crear el registro:

```powershell
$registry = New-AzContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

[!INCLUDE [container-registry-quickstart-sku](../../includes/container-registry-quickstart-sku.md)]

## <a name="log-in-to-registry"></a>Iniciar sesión en el registro

Antes de insertar y extraer imágenes del contenedor, debe iniciar sesión en el registro con el cmdlet [Connect-AzContainerRegistry][connect-azcontainerregistry]. En el ejemplo siguiente se usan las mismas credenciales con las que inició sesión al autenticarse en Azure con el cmdlet `Connect-AzAccount`.

> [!NOTE]
> En el ejemplo siguiente, el valor de `$registry.Name` es el nombre del recurso, no el nombre completo del registro.

```powershell
Connect-AzContainerRegistry -Name $registry.Name
```

El comando devolverá `Login Succeeded` una vez completado.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de trabajar con los recursos que creó en este inicio rápido, use el comando [Remove-AzResourceGroup][Remove-AzResourceGroup] para quitar el grupo de recursos, el registro de contenedor y las imágenes de contenedor almacenadas allí:

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado una instancia de Azure Container Registry con Azure PowerShell, ha insertado una imagen de contenedor y ha extraído y ejecutado la imagen del registro. Siga los tutoriales de Azure Container Registry para información más detallada de ACR.

> [!div class="nextstepaction"]
> [Tutoriales de Azure Container Registry][container-registry-tutorial-prepare-registry]

> [!div class="nextstepaction"]
> [Tutoriales de Azure Container Registry Tasks][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzAccount]: /powershell/module/az.accounts/connect-azaccount
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzContainerRegistry]: /powershell/module/az.containerregistry/New-AzContainerRegistry
[New-AzResourceGroup]: /powershell/module/az.resources/new-azresourcegroup
[Remove-AzResourceGroup]: /powershell/module/az.resources/remove-azresourcegroup
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[container-registry-tutorial-prepare-registry]: container-registry-tutorial-prepare-registry.md
[connect-azcontainerregistry]: /powershell/module/az.containerregistry/connect-azcontainerregistry
