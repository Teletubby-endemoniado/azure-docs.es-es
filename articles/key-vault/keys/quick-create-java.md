---
title: 'Inicio rápido: Biblioteca cliente de claves de Azure Key Vault para Java'
description: Proporciona un inicio rápido para la biblioteca cliente de claves de Azure Key Vault para Java.
author: msmbaldwin
ms.custom: devx-track-java, devx-track-azurecli, devx-track-azurepowershell
ms.author: mbaldwin
ms.date: 01/05/2021
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.openlocfilehash: 79668f9055d9e5715ed5deb04506c1c0b53e5563
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124771448"
---
# <a name="quickstart-azure-key-vault-key-client-library-for-java"></a>Inicio rápido: Biblioteca cliente de claves de Azure Key Vault para Java
Comience a trabajar con la biblioteca cliente de claves de Azure Key Vault para Java. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas.

Recursos adicionales:

* [Código fuente](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-keys)
* [Documentación de referencia de API](https://azure.github.io/azure-sdk-for-java/keyvault.html)
* [Documentación del producto](index.yml)
* [Muestras](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-keys/src/samples/java/com/azure/security/keyvault/keys)

## <a name="prerequisites"></a>Requisitos previos
- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Kit de desarrollo de Java (JDK)](/java/azure/jdk/), versión 8 o posterior
- [Apache Maven](https://maven.apache.org)
- [CLI de Azure](/cli/azure/install-azure-cli)

En esta guía de inicio rápido se supone que está ejecutando la [CLI de Azure](/cli/azure/install-azure-cli) y [Apache Maven](https://maven.apache.org) en una ventana de terminal de Linux.

## <a name="setting-up"></a>Instalación
En este inicio rápido se usa la biblioteca de identidades de Azure con la CLI de Azure para autenticar al usuario en Azure Services. Los desarrolladores también pueden usar Visual Studio o Visual Studio Code para autenticar sus llamadas. Para más información, consulte [Autenticación del cliente mediante la biblioteca cliente Azure Identity](/java/api/overview/azure/identity-readme).

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
1. Ejecute el comando `login`.

    ```azurecli-interactive
    az login
    ```

   Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión de Azure.

   En caso contrario, abra una página del explorador en [https://aka.ms/devicelogin](https://aka.ms/devicelogin) y escriba el código de autorización que se muestra en el terminal.

2. Inicie sesión con las credenciales de su cuenta en el explorador.

### <a name="create-a-new-java-console-app"></a>Creación de una aplicación de consola de Java
En una ventana de consola, utilice el comando `mvn` para crear una nueva aplicación de consola de Java con el nombre `akv-keys-java`.

```console
mvn archetype:generate -DgroupId=com.keyvault.keys.quickstart
                       -DartifactId=akv-keys-java
                       -DarchetypeArtifactId=maven-archetype-quickstart
                       -DarchetypeVersion=1.4
                       -DinteractiveMode=false
```

La salida a partir de la generación del proyecto será similar a la siguiente:

```console
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: com.keyvault.keys.quickstart
[INFO] Parameter: artifactId, Value: akv-keys-java
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.keyvault.keys.quickstart
[INFO] Parameter: packageInPathFormat, Value: com/keyvault/quickstart
[INFO] Parameter: package, Value: com.keyvault.keys.quickstart
[INFO] Parameter: groupId, Value: com.keyvault.keys.quickstart
[INFO] Parameter: artifactId, Value: akv-keys-java
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Project created from Archetype in dir: /home/user/quickstarts/akv-keys-java
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  38.124 s
[INFO] Finished at: 2019-11-15T13:19:06-08:00
[INFO] ------------------------------------------------------------------------
```

Cambie el directorio a la carpeta `akv-keys-java/` recién creada.

```console
cd akv-keys-java
```

### <a name="install-the-package"></a>Instalar el paquete
Abra el archivo *pom.xml* en el editor de texto. Agregue los siguientes elementos de dependencia al grupo de dependencias.

```xml
    <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-security-keyvault-keys</artifactId>
      <version>4.2.3</version>
    </dependency>

    <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-identity</artifactId>
      <version>1.2.0</version>
    </dependency>
```

### <a name="create-a-resource-group-and-key-vault"></a>Creación de un grupo de recursos y de un almacén de claves
[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

#### <a name="grant-access-to-your-key-vault"></a>Concesión de acceso al almacén de claves
Cree una directiva de acceso para el almacén de claves que conceda permisos de clave a la cuenta de usuario.

```console
az keyvault set-policy --name <your-key-vault-name> --upn user@domain.com --key-permissions delete get list create purge
```

#### <a name="set-environment-variables"></a>Establecimiento de variables de entorno
Esta aplicación usa el nombre del almacén de claves como variable de entorno llamada `KEY_VAULT_NAME`.

Windows
```cmd
set KEY_VAULT_NAME=<your-key-vault-name>
````
Windows PowerShell
```powershell
$Env:KEY_VAULT_NAME="<your-key-vault-name>"
```

macOS o Linux
```cmd
export KEY_VAULT_NAME=<your-key-vault-name>
```

## <a name="object-model"></a>Modelo de objetos
La biblioteca cliente de claves de Azure Key Vault para Java permite administrar las claves. En la sección [Ejemplos de código](#code-examples) se muestra cómo crear un cliente y cómo crear, recuperar y eliminar una clave.

Toda la aplicación de consola se encuentra [ a continuación](#sample-code).

## <a name="code-examples"></a>Ejemplos de código
### <a name="add-directives"></a>Adición de directivas
Agregue las siguientes directivas al principio del código:

```java
import com.azure.core.util.polling.SyncPoller;
import com.azure.identity.DefaultAzureCredentialBuilder;

import com.azure.security.keyvault.keys.KeyClient;
import com.azure.security.keyvault.keys.KeyClientBuilder;
import com.azure.security.keyvault.keys.models.DeletedKey;
import com.azure.security.keyvault.keys.models.KeyType;
import com.azure.security.keyvault.keys.models.KeyVaultKey;
```

### <a name="authenticate-and-create-a-client"></a>Autenticación y creación de un cliente
En este inicio rápido se emplea el usuario que ha iniciado sesión para autenticarlo en Key Vault, que es el método preferido para el desarrollo local. En el caso de las aplicaciones implementadas en Azure, se debe asignar una identidad administrada a App Service o a la máquina virtual. Para más información, consulte [Introducción a la identidad administrada](../../active-directory/managed-identities-azure-resources/overview.md).

En el ejemplo siguiente, el nombre del almacén de claves se expande al URI del almacén de claves, con el formato "https://\<your-key-vault-name\>.vault.azure.net". En este ejemplo se usa la clase ["DefaultAzureCredential()"](/java/api/com.azure.identity.defaultazurecredential), que permite usar el mismo código en entornos diferentes con distintas opciones para proporcionar la identidad. Para más información, consulte [Autenticación mediante las credenciales predeterminadas de Azure](/java/api/overview/azure/identity-readme).

```java
String keyVaultName = System.getenv("KEY_VAULT_NAME");
String keyVaultUri = "https://" + keyVaultName + ".vault.azure.net";

KeyClient keyClient = new KeyClientBuilder()
    .vaultUrl(keyVaultUri)
    .credential(new DefaultAzureCredentialBuilder().build())
    .buildClient();
```

### <a name="create-a-key"></a>Crear una clave
Ahora que la aplicación se ha autenticado, puede crear una clave en el almacén de claves con el método `keyClient.createKey`. Esto requiere un nombre para la clave y un tipo de clave: en este ejemplo, se ha asignado el valor "myKey" a la variable `keyName` y se usa RSA para `KeyType`.

```java
keyClient.createKey(keyName, KeyType.RSA);
```

Puede comprobar que la clave se ha establecido con el comando [az keyvault key show](/cli/azure/keyvault/key?#az_keyvault_key_show):

```azurecli
az keyvault key show --vault-name <your-unique-key-vault-name> --name myKey
```

### <a name="retrieve-a-key"></a>Recuperación de una clave
Ahora puede recuperar la clave creada anteriormente con el método `keyClient.getKey`.

```java
KeyVaultKey retrievedKey = keyClient.getKey(keyName);
 ```

Ahora puede acceder a los detalles de la clave recuperada con operaciones como `retrievedKey.getProperties`, `retrievedKey.getKeyOperations`, etc.

### <a name="delete-a-key"></a>Eliminación de una clave
Por último, vamos a eliminar la clave del almacén de claves con el método `keyClient.beginDeleteKey`.

La eliminación de claves es una operación de larga duración cuyo progreso puede sondear, o bien puede esperar a que se complete.

```java
SyncPoller<DeletedKey, Void> deletionPoller = keyClient.beginDeleteKey(keyName);
deletionPoller.waitForCompletion();
```

Puede comprobar que la clave se ha eliminado con el comando [az keyvault key show](/cli/azure/keyvault/key?#az_keyvault_key_show):

```azurecli
az keyvault key show --vault-name <your-unique-key-vault-name> --name myKey
```

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no lo necesite, puede usar la CLI de Azure o Azure PowerShell para quitar el almacén de claves y el grupo de recursos correspondiente.

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="sample-code"></a>Código de ejemplo
```java
package com.keyvault.keys.quickstart;

import com.azure.core.util.polling.SyncPoller;
import com.azure.identity.DefaultAzureCredentialBuilder;

import com.azure.security.keyvault.keys.KeyClient;
import com.azure.security.keyvault.keys.KeyClientBuilder;
import com.azure.security.keyvault.keys.models.DeletedKey;
import com.azure.security.keyvault.keys.models.KeyType;
import com.azure.security.keyvault.keys.models.KeyVaultKey;

public class App {
    public static void main(String[] args) throws InterruptedException, IllegalArgumentException {
        String keyVaultName = System.getenv("KEY_VAULT_NAME");
        String keyVaultUri = "https://" + keyVaultName + ".vault.azure.net";

        System.out.printf("key vault name = %s and key vault URI = %s \n", keyVaultName, keyVaultUri);

        KeyClient keyClient = new KeyClientBuilder()
                .vaultUrl(keyVaultUri)
                .credential(new DefaultAzureCredentialBuilder().build())
                .buildClient();

        String keyName = "myKey";

        System.out.print("Creating a key in " + keyVaultName + " called '" + keyName + " ... ");

        keyClient.createKey(keyName, KeyType.RSA);

        System.out.print("done.");
        System.out.println("Retrieving key from " + keyVaultName + ".");

        KeyVaultKey retrievedKey = keyClient.getKey(keyName);

        System.out.println("Your key's ID is '" + retrievedKey.getId() + "'.");
        System.out.println("Deleting your key from " + keyVaultName + " ... ");

        SyncPoller<DeletedKey, Void> deletionPoller = keyClient.beginDeleteKey(keyName);
        deletionPoller.waitForCompletion();

        System.out.print("done.");
    }
}
```

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido ha creado un almacén de claves, ha creado una clave, la ha recuperado y, a continuación, la ha eliminado. Para más información sobre Key Vault y cómo integrarlo con las aplicaciones, continúe con los artículos siguientes.

- Lea una [introducción a Azure Key Vault](../general/overview.md).
- Consulte [Introducción a la seguridad de Azure Key Vault](../general/security-features.md).
- Consulte la [guía del desarrollador de Azure Key Vault](../general/developers-guide.md).
- Procedimientos para [proteger el acceso a un almacén de claves](../general/security-features.md)
