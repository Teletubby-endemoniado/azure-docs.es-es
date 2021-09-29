---
title: 'Tutorial: Cifrado y descifrado de blobs mediante Azure Key Vault'
titleSuffix: Azure Storage
description: Obtenga información sobre cómo cifrar y descifrar un blob mediante el cifrado del lado cliente con Azure Key Vault.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 02/18/2021
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: blobs
ms.custom: devx-track-csharp
ms.openlocfilehash: 78356745ee013b011f23a4bf42f903cf89bedd4b
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128603987"
---
# <a name="tutorial---encrypt-and-decrypt-blobs-using-azure-key-vault"></a>Tutorial: Cifrado y descifrado de blobs mediante Azure Key Vault

Este tutorial explica cómo hacer uso del cifrado de almacenamiento del lado cliente con Azure Key Vault. Se explica cómo cifrar y descifrar un blob en una aplicación de consola con estas tecnologías.

**Tiempo estimado para completar el tutorial:** 20 minutos

Para información general sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../../key-vault/general/overview.md).

Para obtener información general sobre el cifrado de cliente para Azure Storage, consulte [Cifrado del lado de cliente y Azure Key Vault para Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="prerequisites"></a>Prerrequisitos

Para realizar este tutorial, debe disponer de lo siguiente:

- Una cuenta de Azure Storage.
- Visual Studio 2013 o posterior.
- Azure PowerShell

## <a name="overview-of-client-side-encryption"></a>Información general sobre el cifrado del lado cliente

Para obtener información general acerca del cifrado de cliente para Azure Storage, consulte [Cifrado del lado de cliente y Azure Key Vault para Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Esta es una breve descripción de cómo funciona el cifrado del lado cliente:

1. El SDK de cliente de Azure Storage genera una clave de cifrado de contenido (CEK), que es una clave simétrica de un solo uso.
2. Los datos de usuario se cifran mediante esta CEK.
3. Se encapsula la CEK (cifrada) con la clave de cifrado de clave (KEK). La KEK se identifica mediante un identificador de clave y puede ser un par de clave asimétrico o una clave simétrica que puede administrarse de forma local o guardarse en Azure Key Vault. El propio cliente de almacenamiento no tiene acceso a KEK. Simplemente invoca el algoritmo de ajuste de clave proporcionado por Key Vault. Los clientes pueden elegir usar proveedores personalizados para el ajuste y desajuste de la clave si así lo desean.
4. A continuación, se cargan los datos cifrados en el servicio Azure Storage.

## <a name="set-up-your-azure-key-vault"></a>Configuración de Azure Key Vault

Para continuar con este tutorial, debe seguir los pasos que se indican a continuación y que se detallan en el tutorial [Inicio rápido: Establecimiento y recuperación de un secreto desde Azure Key Vault mediante una aplicación web de .NET](../../key-vault/secrets/quick-create-net.md):

- Cree un almacén de claves.
- Agregue una clave o un secreto al almacén de claves.
- Registre una aplicación con Azure Active Directory.
- Autorice la aplicación para usar la clave o el secreto.

Anote el ClientID y ClientSecret que se generaron al registrar una aplicación con Azure Active Directory.

Cree ambas claves en el almacén de claves. A partir de aquí asumiremos para el resto del tutorial que ha usado los nombres siguientes: ContosoKeyVault y TestRSAKey1.

## <a name="create-a-console-application-with-packages-and-appsettings"></a>Creación de una aplicación de consola con paquetes y AppSettings

En Visual Studio, cree una nueva aplicación de consola.

Agregue los paquetes de NuGet necesarios en la Consola del Administrador de paquetes.

```powershell
Install-Package Microsoft.Azure.ConfigurationManager
Install-Package Microsoft.Azure.Storage.Common
Install-Package Microsoft.Azure.Storage.Blob
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

Agregar AppSettings al archivo App.Config.

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

Agregue las siguientes directivas `using` y asegúrese de agregar una referencia el proyecto System.Configuration.

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Actualmente, estamos trabajando para crear fragmentos de código que reflejen la versión 12.x de las bibliotecas cliente de Azure Storage. Para más información, consulte el [anuncio de las bibliotecas cliente de Azure Storage v12](https://techcommunity.microsoft.com/t5/azure-storage/announcing-the-azure-storage-v12-client-libraries/ba-p/1482394).

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Configuration;
using Microsoft.Azure;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Auth;
using Microsoft.Azure.Storage.Blob;
using Microsoft.Azure.KeyVault;
using System.Threading;
using System.IO;
```

---

## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Agregar un método para obtener un token para la aplicación de consola

El siguiente método lo usan las clases de Key Vault que tienen que autenticarse para poder acceder a su almacén de claves.

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Actualmente, estamos trabajando para crear fragmentos de código que reflejen la versión 12.x de las bibliotecas cliente de Azure Storage. Para más información, consulte el [anuncio de las bibliotecas cliente de Azure Storage v12](https://techcommunity.microsoft.com/t5/azure-storage/announcing-the-azure-storage-v12-client-libraries/ba-p/1482394).

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

```csharp
private async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(
        CloudConfigurationManager.GetSetting("clientId"),
        CloudConfigurationManager.GetSetting("clientSecret"));
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

---

## <a name="access-azure-storage-and-key-vault-in-your-program"></a>Acceso a Azure Storage y Key Vault en el programa

Agregue el siguiente código al método Main().

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Actualmente, estamos trabajando para crear fragmentos de código que reflejen la versión 12.x de las bibliotecas cliente de Azure Storage. Para más información, consulte el [anuncio de las bibliotecas cliente de Azure Storage v12](https://techcommunity.microsoft.com/t5/azure-storage/announcing-the-azure-storage-v12-client-libraries/ba-p/1482394).

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

```csharp
// This is standard code to interact with Blob storage.
StorageCredentials creds = new StorageCredentials(
    CloudConfigurationManager.GetSetting("accountName"),
    CloudConfigurationManager.GetSetting("accountKey")
);
CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
CloudBlobClient client = account.CreateCloudBlobClient();
CloudBlobContainer contain = client.GetContainerReference(CloudConfigurationManager.GetSetting("container"));
contain.CreateIfNotExists();

// The Resolver object is used to interact with Key Vault for Azure Storage.
// This is where the GetToken method from above is used.
KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);
```

---

> [!NOTE]
> Modelos de objetos de Key Vault
>
> Es importante entender que hay en realidad dos modelos de objeto de Key Vault a tener en cuenta: uno se basa en la API de REST (espacio de nombres KeyVault) y el otro es una extensión para el cifrado de cliente.
>
> El cliente de Key Vault interactúa con la API de REST y entiende las claves web JSON y los secretos de los dos tipos de elementos que están contenidos en Key Vault.
>
> Las extensiones de Key Vault son clases que parecen creadas específicamente para el cifrado de cliente en Azure Storage. Contienen una interfaz para claves (IKey) y clases basadas en el concepto de un solucionador de claves. Hay dos implementaciones de IKey que necesita conocer: RSAKey y SymmetricKey. Ahora coinciden con las cosas que están contenidas en un almacén de datos, pero en este momento son clases independientes (la clave y el secreto recuperado por el cliente de almacén de claves no implementan IKey).
>
>

## <a name="encrypt-blob-and-upload"></a>Cifrado y carga de blob

Agregue el código siguiente para cifrar un blob y cargarlo en la cuenta de almacenamiento de Azure. El método **ResolveKeyAsync** que se usa devuelve una IKey.

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Actualmente, estamos trabajando para crear fragmentos de código que reflejen la versión 12.x de las bibliotecas cliente de Azure Storage. Para más información, consulte el [anuncio de las bibliotecas cliente de Azure Storage v12](https://techcommunity.microsoft.com/t5/azure-storage/announcing-the-azure-storage-v12-client-libraries/ba-p/1482394).

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

```csharp
// Retrieve the key that you created previously.
// The IKey that is returned here is an RsaKey.
var rsa = cloudResolver.ResolveKeyAsync(
            "https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", 
            CancellationToken.None).GetAwaiter().GetResult();

// Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Reference a block blob.
CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

// Upload using the UploadFromStream method.
using (var stream = System.IO.File.OpenRead(@"C:\Temp\MyFile.txt"))
    blob.UploadFromStream(stream, stream.Length, null, options, null);
```

---

> [!NOTE]
> Si observa el constructor BlobEncryptionPolicy, verá que puede aceptar una clave o una resolución. Tenga en cuenta que ahora no puede utilizar una resolución para el cifrado porque en la actualidad no admite una clave predeterminada.

## <a name="decrypt-blob-and-download"></a>Descifrado y carga del blob

Es en el descifrado realmente cuando las clases de solucionador tienen sentido. El identificador de la clave usada para el cifrado se asocia con el blob en sus metadatos, así que no hace falta que recupere la clave ni que recuerde la asociación entre la clave y el blob. Solo tiene que asegurarse de que la clave se mantiene en Key Vault.

La clave privada de una clave RSA permanece en Key Vault, por lo que para que se produzca el descifrado, la clave cifrada de los metadatos del blob que contiene la CEC (clave de cifrado de contenido) se envía a Key Vault.

Agregue lo siguiente para descifrar el blob que acaba de cargar.

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Actualmente, estamos trabajando para crear fragmentos de código que reflejen la versión 12.x de las bibliotecas cliente de Azure Storage. Para más información, consulte el [anuncio de las bibliotecas cliente de Azure Storage v12](https://techcommunity.microsoft.com/t5/azure-storage/announcing-the-azure-storage-v12-client-libraries/ba-p/1482394).

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

```csharp
// In this case, we will not pass a key and only pass the resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

---

> [!NOTE]
> Existen un par de otros tipos de solucionadores que facilitan la administración de claves, incluidos AggregateKeyResolver y CachingKeyResolver.

## <a name="use-key-vault-secrets"></a>Uso de secretos de Key Vault

La manera de usar un secreto con cifrado del lado cliente es mediante la clase SymmetricKey, porque un secreto es esencialmente una clave simétrica. Sin embargo, como se mencionó anteriormente, un secreto en Key Vault no se asigna exactamente a una SymmetricKey. Existen algunos aspectos que debe comprender:

- La clave en una SymmetricKey tiene que tener una longitud fija: 128, 192, 256, 384 o 512 bits
- La clave en una SymmetricKey debe estar codificada en Base64.
- Un secreto de Almacén de clave que se use como una SymmetricKey tiene que tener un tipo de contenido de "application/octet-stream" en Key Vault.

Este es un ejemplo en PowerShell de creación de un secreto en Key Vault que se puede usar como una SymmetricKey.
Tenga en cuenta que el valor codificado, $key, es solo para fines de demostración. En su código, es recomendable generar esta clave.

```powershell
// Here we are making a 128-bit key so we have 16 characters.
//     The characters are in the ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute the VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

En la aplicación de consola, puede usar la misma llamada que antes para recuperar este secreto como una SymmetricKey.

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Actualmente, estamos trabajando para crear fragmentos de código que reflejen la versión 12.x de las bibliotecas cliente de Azure Storage. Para más información, consulte el [anuncio de las bibliotecas cliente de Azure Storage v12](https://techcommunity.microsoft.com/t5/azure-storage/announcing-the-azure-storage-v12-client-libraries/ba-p/1482394).

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```

---

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el uso de Microsoft Azure Storage con C#, consulte [Biblioteca de cliente de Microsoft Azure Storage para .NET](/previous-versions/azure/dn261237(v=azure.100)).

Para más información sobre la API de REST de blobs, consulte [API de REST de Blob service](/rest/api/storageservices/Blob-Service-REST-API).

Para obtener la información más reciente sobre Microsoft Azure Storage, vaya al [blog del equipo de Microsoft Azure Storage](/archive/blogs/windowsazurestorage/).
