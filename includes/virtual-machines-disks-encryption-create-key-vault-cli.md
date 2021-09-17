---
title: Archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/24/2021
ms.author: rogarana
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 23b6c4dddd839351e7eff49b6dea4c42fe6452d8
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122825041"
---
1. Asegúrese de que ha instalado la versión más reciente de la [CLI de Azure](/cli/azure/install-az-cli2) e iniciado sesión en una cuenta de Azure con [az login](/cli/azure/reference-index).

1. Cree una instancia de Azure Key Vault y la clave de cifrado.

    Al crear la instancia de Key Vault, debe habilitar la protección contra purgas. La protección de purgas garantiza que una clave eliminada no se puede eliminar permanentemente hasta que transcurra el período de retención. Esta configuración le protege contra la pérdida de datos debido a la eliminación accidental. Estos valores son obligatorios cuando se usa una instancia de Key Vault para cifrar discos administrados.

    > [!IMPORTANT]
    > No use el estilo CamelCase para la región. Si lo hace, podría experimentar problemas al asignar discos adicionales al recurso en Azure Portal.

    ```azurecli
    subscriptionId=yourSubscriptionID
    rgName=yourResourceGroupName
    location=westcentralus
    keyVaultName=yourKeyVaultName
    keyName=yourKeyName
    diskEncryptionSetName=yourDiskEncryptionSetName
    diskName=yourDiskName

    az account set --subscription $subscriptionId

    az keyvault create -n $keyVaultName \
    -g $rgName \
    -l $location \
    --enable-purge-protection true 
                       
    az keyvault key create --vault-name $keyVaultName \
    -n $keyName \
    --protection software
    ```

1.    Cree una instancia de un objeto DiskEncryptionSet. Puede establecer enable-auto-key-rotation en true para habilitar la rotación automática de la clave. Al habilitar la rotación automática, el sistema actualiza automáticamente todos los discos administrados, instantáneas e imágenes que hagan referencia al conjunto de cifrado de disco para usar la nueva versión de la clave en una hora.
    
        ```azurecli
        keyVaultId=$(az keyvault show --name $keyVaultName --query [id] -o tsv)
    
        keyVaultKeyUrl=$(az keyvault key show --vault-name $keyVaultName --name $keyName --query [key.kid] -o tsv)
    
        az disk-encryption-set create -n $diskEncryptionSetName 
        -l $location \
        -g $rgName \
        --source-vault $keyVaultId \
        --key-url $keyVaultKeyUrl \
        --enable-auto-key-rotation false
        ```

1.    Conceda al recurso DiskEncryptionSet acceso al almacén de claves. 

        > [!NOTE]
        > Azure puede tardar unos minutos en crear la identidad del elemento DiskEncryptionSet en Azure Active Directory. Si recibe un error similar a "No se encuentra el objeto en Active Directory" al ejecutar el siguiente comando, espere unos minutos y vuelva a intentarlo.

        ```azurecli
        desIdentity=$(az disk-encryption-set show -n $diskEncryptionSetName -g $rgName --query [identity.principalId] -o tsv)
    
        az keyvault set-policy -n $keyVaultName \
        -g $rgName \
        --object-id $desIdentity \
        --key-permissions wrapkey unwrapkey get
        ```
