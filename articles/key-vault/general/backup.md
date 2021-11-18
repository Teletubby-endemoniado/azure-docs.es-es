---
title: Copia de seguridad de un secreto, una clave o un certificado almacenados en Azure Key Vault | Microsoft Docs
description: Use este documento para ayudar a realizar copias de seguridad de un secreto, una clave o un certificado almacenados en Azure Key Vault.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 3/18/2021
ms.author: sudbalas
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 70f4e8627f22b400ee2b890dd05fa886137ff4b2
ms.sourcegitcommit: 901ea2c2e12c5ed009f642ae8021e27d64d6741e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2021
ms.locfileid: "132369747"
---
# <a name="azure-key-vault-backup-and-restore"></a>Copia de seguridad y restauración de Azure Key Vault

En este documento se muestra cómo realizar copias de seguridad de secretos, claves y certificados almacenados en el almacén de claves. Esta copia de seguridad está diseñada para proporcionarle una copia sin conexión de todos los secretos en el caso improbable de que pierda el acceso al almacén de claves.

## <a name="overview"></a>Información general

Azure Key Vault proporciona automáticamente características para ayudarle a mantener la disponibilidad y evitar la pérdida de datos. Haga una copia de seguridad de los secretos solo si tiene una justificación empresarial crítica. La copia de seguridad de los secretos del almacén de claves puede plantear desafíos operativos adicionales, como el mantenimiento de varios conjuntos de registros, permisos y copias de seguridad cuando los secretos expiren o roten.

Key Vault mantiene la disponibilidad en escenarios de desastre y realizará automáticamente una conmutación por error de las solicitudes a una región emparejada sin intervención del usuario. Para más información, consulte [Redundancia y disponibilidad de Azure Key Vault](./disaster-recovery-guidance.md).

Si desea protección contra la eliminación accidental o malintencionada de los secretos, configure las características de eliminación temporal y protección de purga en el almacén de claves. Para más información, consulte el artículo [Información general sobre la eliminación temporal de Azure Key Vault](./soft-delete-overview.md).

## <a name="limitations"></a>Limitaciones

> [!IMPORTANT]
> Key Vault no ofrece la posibilidad de realizar copias de seguridad de más de 500 versiones anteriores para un objeto de clave, secreto o certificado. Al intentar hacer una copia de seguridad de un objeto de clave, secreto o certificado, se puede producir un error. No es posible eliminar versiones anteriores de una clave, un secreto o un certificado.

Key Vault no proporciona actualmente una forma de realizar una copia de seguridad de un almacén de claves completo en una sola operación. Cualquier intento de usar los comandos enumerados en este documento para realizar una copia de seguridad automatizada de un almacén de claves puede producir errores y no será admitida por Microsoft ni el equipo de Azure Key Vault. 

Tenga en cuenta también las consecuencias siguientes:

* La copia de seguridad de secretos que tienen varias versiones podría producir errores de expiración del tiempo de espera.
* Una copia de seguridad crea una instantánea a un momento dado. Los secretos pueden renovarse durante una copia de seguridad, lo que provoca una falta de coincidencia de las claves de cifrado.
* Si se superan los límites de servicio de Key Vault en cuanto a solicitudes por segundo, el almacén de claves se limitará y se producirá un error en la copia de seguridad.

## <a name="design-considerations"></a>Consideraciones de diseño

Al realizar una copia de seguridad de un objeto almacenado en el almacén de claves (secreto, clave o certificado), la operación de copia de seguridad descargará el objeto como un blob cifrado. Este blob no se puede descifrar fuera de Azure. Para obtener datos que se puedan usar de este blob, debe restaurar el blob en un almacén de claves dentro de la misma suscripción de Azure y [zona geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/).

## <a name="prerequisites"></a>Requisitos previos

Para realizar una copia de seguridad de un objeto de Key Vault, debe tener: 

* Permisos de nivel de colaborador o superior en una suscripción de Azure.
* Un almacén de claves principal que contenga los secretos de los que desea realizar una copia de seguridad.
* Un almacén de claves secundario en el que se restaurarán los secretos.

## <a name="back-up-and-restore-from-the-azure-portal"></a>Copia de seguridad y restauración desde Azure Portal

Siga los pasos de esta sección para realizar copias de seguridad y restaurar objetos mediante Azure Portal.

### <a name="back-up"></a>Copia de seguridad

1. Vaya a Azure Portal.
2. Seleccione el almacén de claves.
3. Vaya al objeto (secreto, clave o certificado) del que desea realizar una copia de seguridad.

    ![Captura de pantalla que muestra dónde seleccionar la configuración de claves y un objeto en un almacén de claves.](../media/backup-1.png)

4. Seleccione el objeto.
5. Seleccione **Descargar copia de seguridad**.

    ![Captura de pantalla que muestra dónde seleccionar el botón Descargar copia de seguridad en un almacén de claves.](../media/backup-2.png)
    
6. Seleccione **Descargar**.

    ![Captura de pantalla que muestra dónde seleccionar el botón Descargar en un almacén de claves.](../media/backup-3.png)
    
7. Almacene el blob cifrado en una ubicación segura.

### <a name="restore"></a>Restauración

1. Vaya a Azure Portal.
2. Seleccione el almacén de claves.
3. Desplácese hasta el tipo de objeto (secreto, clave o certificado) que desea restaurar.
4. Seleccione **Restaurar copia de seguridad**.

    ![Captura de pantalla que muestra dónde seleccionar Restaurar copia de seguridad en un almacén de claves.](../media/backup-4.png)
    
5. Vaya a la ubicación donde almacenó el blob cifrado.
6. Seleccione **Aceptar**.

## <a name="back-up-and-restore-from-the-azure-cli-or-azure-powershell"></a>Copia de seguridad y restauración desde la CLI de Azure o Azure PowerShell

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)
```azurecli
## Log in to Azure
az login

## Set your subscription
az account set --subscription {AZURE SUBSCRIPTION ID}

## Register Key Vault as a provider
az provider register -n Microsoft.KeyVault

## Back up a certificate in Key Vault
az keyvault certificate backup --file {File Path} --name {Certificate Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Back up a key in Key Vault
az keyvault key backup --file {File Path} --name {Key Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Back up a secret in Key Vault
az keyvault secret backup --file {File Path} --name {Secret Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a certificate in Key Vault
az keyvault certificate restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a key in Key Vault
az keyvault key restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a secret in Key Vault
az keyvault secret restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}
```
# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

```azurepowershell
## Log in to Azure
Connect-AzAccount

## Set your subscription
Set-AzContext -Subscription '{AZURE SUBSCRIPTION ID}'

## Back up a certificate in Key Vault
Backup-AzKeyVaultCertificate -VaultName '{Key Vault Name}' -Name '{Certificate Name}'

## Back up a key in Key Vault
Backup-AzKeyVaultKey -VaultName '{Key Vault Name}' -Name '{Key Name}'

## Back up a secret in Key Vault
Backup-AzKeyVaultSecret -VaultName '{Key Vault Name}' -Name '{Secret Name}'

## Restore a certificate in Key Vault
Restore-AzKeyVaultCertificate -VaultName '{Key Vault Name}' -InputFile '{File Path}'

## Restore a key in Key Vault
Restore-AzKeyVaultKey -VaultName '{Key Vault Name}' -InputFile '{File Path}'

## Restore a secret in Key Vault
Restore-AzKeyVaultSecret -VaultName '{Key Vault Name}' -InputFile '{File Path}'
```
---

## <a name="next-steps"></a>Pasos siguientes


- [Movimiento de un almacén de claves de Azure entre regiones](move-region.md)
- [Habilitación del registro de Key Vault](howto-logging.md) para Key Vault