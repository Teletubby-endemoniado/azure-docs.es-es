---
title: 'Inicio rápido: Establecimiento y visualización de certificados de Azure Key Vault mediante la CLI de Azure'
description: Inicio rápido en el que se muestra cómo establecer y recuperar un certificado de Azure Key Vault mediante la CLI de Azure
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019, devx-track-azurecli
ms.date: 01/27/2021
ms.author: mbaldwin
ms.openlocfilehash: 265a66b6aa80dddb6ca4d8da8567ded8c7a380ba
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128663808"
---
# <a name="quickstart-set-and-retrieve-a-certificate-from-azure-key-vault-using-azure-cli"></a>Inicio rápido: Establecimiento y recuperación de un certificado de Azure Key Vault mediante la CLI de Azure

En este inicio rápido, creará un almacén de claves en Azure Key Vault con la CLI de Azure. Azure Key Vault es un servicio en la nube que funciona como un almacén de secretos seguro. Puede almacenar de forma segura claves, contraseñas, certificados y otros secretos. Para más información sobre Key Vault, puede consultar esta [introducción](../general/overview.md). La CLI de Azure se usa para crear y administrar recursos de Azure mediante comandos o scripts. Una vez que haya terminado, almacenará un certificado.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Para realizar este inicio rápido es necesaria la versión 2.0.4 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [Create a resource group](../../../includes/key-vault-cli-rg-creation.md)]

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

[!INCLUDE [Create a key vault](../../../includes/key-vault-cli-kv-creation.md)]

## <a name="add-a-certificate-to-key-vault"></a>Incorporación de un certificado a Key Vault

Para agregar un certificado al almacén, no tiene más que realizar un par de pasos adicionales. Este certificado puede ser utilizado por una aplicación. 

Escriba los siguientes comandos para crear un certificado autofirmado con una directiva predeterminada denominada **ExampleCertificate**:

```azurecli
az keyvault certificate create --vault-name "<your-unique-keyvault-name>" -n ExampleCertificate -p "$(az keyvault certificate get-default-policy)"
```

Ahora puede hacer referencia a este certificado que ha agregado a Azure Key Vault utilizando su URI. Use **`https://<your-unique-keyvault-name>.vault.azure.net/certificates/ExampleCertificate`** para obtener la versión actual. 

Para ver el certificado que se ha almacenado previamente:

```azurecli

az keyvault certificate show --name "ExampleCertificate" --vault-name "<your-unique-keyvault-name>"
```

Ya ha creado una instancia de Key Vault, ha almacenado un certificado y, posteriormente, lo ha recuperado.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Create a key vault](../../../includes/key-vault-cli-delete-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha creado una instancia de Key Vault y ha almacenado un certificado en ella. Para más información sobre Key Vault y cómo integrarlo con las aplicaciones, continúe con los artículos siguientes.

- Lea una [introducción a Azure Key Vault](../general/overview.md).
- Consulte la referencia de los [comandos az keyvault de la CLI de Azure](/cli/azure/keyvault).
- Consulte [Introducción a la seguridad de Azure Key Vault](../general/security-features.md)
