---
title: Creación de una directiva de acceso almacenada con .NET
titleSuffix: Azure Storage
description: Use Azure Storage y .NET para crear una directiva de acceso almacenada. Ejerza niveles adicionales de control sobre las firmas de acceso compartido de nivel de servicio en el servidor.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/12/2021
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: devx-track-csharp
ms.openlocfilehash: 1be2b8200cd9e7b83fb3d113966a13c32e126d34
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128589271"
---
# <a name="create-a-stored-access-policy-with-net"></a>Creación de una directiva de acceso almacenada con .NET

Una directiva de acceso almacenada proporciona un nivel de control adicional sobre las firmas de acceso compartido (SAS) de nivel de servicio en el lado del servidor. La definición de una directiva de acceso almacenada sirve para agrupar firmas de acceso compartido y proporcionar restricciones adicionales para las firmas de acceso compartido que están enlazadas por la directiva. Puede usar una directiva de acceso almacenada para cambiar la hora de inicio, la hora de expiración, los permisos de una SAS ni para revocarla una vez se ha emitido.

Los siguientes recursos de Azure Storage admiten directivas de acceso almacenadas:

- Contenedores de blobs
- Recursos compartidos de archivos
- Colas
- Tablas

> [!NOTE]
> Una directiva de acceso almacenada de un contenedor se puede asociar a una firma de acceso compartido que conceda permisos al propio contenedor o a los blobs que contiene. De la misma manera, una directiva de acceso almacenada de un recurso compartido de archivos se puede asociar a una firma de acceso compartido que conceda permisos al propio recurso compartido o a los archivos que contiene.  
>
> Las directivas de acceso almacenadas son compatibles para una SAS de servicio únicamente. Las directivas de acceso almacenadas no son compatibles para SAS de cuenta ni de delegación de usuarios.

Para obtener más información sobre las directivas de acceso almacenadas, consulte [Definición de una directiva de acceso almacenada](/rest/api/storageservices/define-stored-access-policy).

## <a name="create-a-stored-access-policy"></a>Crear una directiva de acceso almacenada

La operación de REST subyacente para crear una directiva de acceso almacenada es [Set Container ACL](/rest/api/storageservices/set-container-acl). Debe autorizar la operación para crear una directiva de acceso almacenada a través de la clave compartida mediante las claves de acceso de la cuenta en una cadena de conexión. No se admite la autorización de la operación **Set Container ACL** con credenciales de Azure AD. Para obtener más información, consulte [Permisos para llamar a operaciones de datos](/rest/api/storageservices/authorize-with-azure-active-directory#permissions-for-calling-data-operations).

Los ejemplos de código siguientes crean una directiva de acceso almacenada en un contenedor. Puede usar la directiva de acceso para especificar restricciones para una SAS de servicio en el contenedor o sus blobs.

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Para crear una directiva de acceso almacenada en un contenedor con la versión 12 de la biblioteca cliente de .NET para Azure Storage, llame a uno de los métodos siguientes:

- [BlobContainerClient.SetAccessPolicy](/dotnet/api/azure.storage.blobs.blobcontainerclient.setaccesspolicy)
- [BlobContainerClient.SetAccessPolicyAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.setaccesspolicyasync)

En el ejemplo siguiente se crea una directiva de acceso almacenada que está en vigor durante un día y que concede permisos de lectura y escritura:

```csharp
async static Task CreateStoredAccessPolicyAsync(string containerName)
{
    string connectionString = "";

    // Use the connection string to authorize the operation to create the access policy.
    // Azure AD does not support the Set Container ACL operation that creates the policy.
    BlobContainerClient containerClient = new BlobContainerClient(connectionString, containerName);

    try
    {
        await containerClient.CreateIfNotExistsAsync();

        // Create one or more stored access policies.
        List<BlobSignedIdentifier> signedIdentifiers = new List<BlobSignedIdentifier>
        {
            new BlobSignedIdentifier
            {
                Id = "mysignedidentifier",
                AccessPolicy = new BlobAccessPolicy
                {
                    StartsOn = DateTimeOffset.UtcNow.AddHours(-1),
                    ExpiresOn = DateTimeOffset.UtcNow.AddDays(1),
                    Permissions = "rw"
                }
            }
        };
        // Set the container's access policy.
        await containerClient.SetAccessPolicyAsync(permissions: signedIdentifiers);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.ErrorCode);
        Console.WriteLine(e.Message);
    }
    finally
    {
        await containerClient.DeleteAsync();
    }
}
```

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

Para crear una directiva de acceso almacenada en un contenedor con la versión 12 de la biblioteca cliente de .NET para Azure Storage, llame a uno de los métodos siguientes:

- [CloudBlobContainer.SetPermissions](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setpermissions)
- [CloudBlobContainer.SetPermissionsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setpermissionsasync)

En el ejemplo siguiente se crea una directiva de acceso almacenada que está en vigor durante un día y que concede permisos de lectura, escritura y enumeración:

```csharp
private static async Task CreateStoredAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new stored access policy and define its constraints.
    // The access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When the start time for the SAS is omitted, the start time is assumed to be the time when Azure Storage receives the request.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write
    };

    // Get the container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

---

## <a name="see-also"></a>Consulte también

- [Grant limited access to Azure Storage resources using shared access signatures (SAS)](storage-sas-overview.md) (Otorgar acceso limitado a recursos de Azure Storage con firmas de acceso compartido [SAS])
- [Definición de una directiva de acceso almacenada](/rest/api/storageservices/define-stored-access-policy)
- [Configuración de las cadenas de conexión de Azure Storage](storage-configure-connection-string.md)
