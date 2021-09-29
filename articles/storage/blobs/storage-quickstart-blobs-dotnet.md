---
title: 'Inicio rápido: Biblioteca de Azure Blob Storage v12 para .NET'
description: En esta guía de inicio rápido, obtendrá información sobre cómo usar la versión 12 de la biblioteca cliente de Azure Blob Storage para .NET para crear un contenedor y un blob en Blob Storage (objeto). A continuación, aprenderá a descargar el blob en un equipo local y a enumerar todos los blobs en un contenedor.
author: normesta
ms.author: normesta
ms.date: 03/03/2021
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.custom: devx-track-csharp
ms.openlocfilehash: c71a362c7e8e3073929967abce4bcdc4566b8ce9
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128652919"
---
# <a name="quickstart-azure-blob-storage-client-library-v12-for-net"></a>Inicio rápido: Biblioteca cliente de Azure Blob Storage v12 para .NET

Introducción a la biblioteca cliente de Azure Blob Storage v12 para .NET. Azure Blob Storage es la solución de almacenamiento de objetos de Microsoft para la nube. Siga los pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas. Blob Storage está optimizado para el almacenamiento de cantidades masivas de datos no estructurados.

Use la biblioteca cliente de Azure Blob Storage v12 para .NET para:

- Crear un contenedor
- Cargar un blob en Azure Storage
- Enumerar todos los blobs de un contenedor
- Descargar el blob en el equipo local
- Eliminación de un contenedor

Recursos adicionales:

- [Documentación de referencia de API](/dotnet/api/azure.storage.blobs)
- [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs)
- [Paquete (NuGet)](https://www.nuget.org/packages/Azure.Storage.Blobs)
- [Muestras](../common/storage-samples-dotnet.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-samples)

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
- Una cuenta de Azure Storage: [cree una cuenta de almacenamiento](../common/storage-account-create.md)
- El [SDK de NET Core](https://dotnet.microsoft.com/download/dotnet-core) actual del sistema operativo. Asegúrese de obtener el SDK y no el entorno de ejecución.

## <a name="setting-up"></a>Instalación

En esta sección se explica cómo preparar un proyecto para que funcione con la biblioteca cliente de Azure Blob Storage v12 para .NET.

### <a name="create-the-project"></a>Creación del proyecto

Cree una aplicación de .NET Core llamada *BlobQuickstartV12*.

1. En una ventana de consola (por ejemplo, cmd, PowerShell o Bash), use el comando `dotnet new` para crear una nueva aplicación de consola con el nombre *BlobQuickstartV12*. Este comando crea un sencillo proyecto "Hola mundo" de C# con un solo archivo de origen: *Program.cs*.

   ```console
   dotnet new console -n BlobQuickstartV12
   ```

1. Cambie al directorio *BlobQuickstartV12* recién creado.

   ```console
   cd BlobQuickstartV12
   ```

1. En el directorio *BlobQuickstartV12*, cree otro directorio denominado *data*. Aquí es donde se crearán y almacenarán los archivos de datos de blobs.

    ```console
    mkdir data
    ```

### <a name="install-the-package"></a>Instalar el paquete

Mientras sigue en el directorio de aplicaciones, instale el paquete de la biblioteca de cliente de Azure Blob Storage para .NET con el comando `dotnet add package`.

```console
dotnet add package Azure.Storage.Blobs
```

### <a name="set-up-the-app-framework"></a>Instalación del marco de la aplicación

Desde el directorio del proyecto:

1. Abra el archivo *Program.cs* en el editor.
1. Quite la instrucción `Console.WriteLine("Hello World!");` .
1. Agregue las directivas `using`.
1. Actualice la declaración del método `Main` para admitir el funcionamiento asincrónico.

    Este es el código:

    :::code language="csharp" source="~/azure-storage-snippets/blobs/quickstarts/dotnet/BlobQuickstartV12/app_framework.cs":::

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Modelo de objetos

Azure Blob Storage está optimizado para el almacenamiento de cantidades masivas de datos no estructurados. Los datos no estructurados son datos que no cumplen un modelo de datos o definición concreta, como texto o datos binarios. Blob Storage ofrece tres tipos de recursos:

- La cuenta de almacenamiento
- Un contenedor en la cuenta de almacenamiento
- Un blob en el contenedor

En el siguiente diagrama se muestra la relación entre estos recursos.

![Diagrama de arquitectura de Blob Storage](./media/storage-blobs-introduction/blob1.png)

Use las siguientes clases de .NET para interactuar con estos recursos:

- [BlobServiceClient](/dotnet/api/azure.storage.blobs.blobserviceclient): La clase `BlobServiceClient` permite manipular recursos de Azure Storage y contenedores de blobs.
- [BlobContainerClient](/dotnet/api/azure.storage.blobs.blobcontainerclient): La clase `BlobContainerClient` permite manipular contenedores de Azure Storage y sus blobs.
- [BlobClient](/dotnet/api/azure.storage.blobs.blobclient): La clase `BlobClient` permite manipular los blobs de Azure Storage.

## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código de ejemplo muestran cómo realizar las siguientes acciones con la biblioteca de cliente de Azure Blob Storage para. NET:

- [Obtención de la cadena de conexión](#get-the-connection-string)
- [Creación de un contenedor](#create-a-container)
- [Carga de los blobs en un contenedor](#upload-blobs-to-a-container)
- [Enumeración de los blobs de un contenedor](#list-the-blobs-in-a-container)
- [Descarga de los blobs](#download-blobs)
- [Eliminación de un contenedor](#delete-a-container)

### <a name="get-the-connection-string"></a>Obtención de la cadena de conexión

El código siguiente recupera la cadena de conexión de la cuenta de almacenamiento de la variable de entorno creada en la sección [Configuración de la cadena de conexión de almacenamiento](#configure-your-storage-connection-string).

Agregue este código dentro del método `Main`:

:::code language="csharp" source="~/azure-storage-snippets/blobs/quickstarts/dotnet/BlobQuickstartV12/Program.cs" id="Snippet_ConnectionString":::

### <a name="create-a-container"></a>Crear un contenedor

Decida un nombre para el nuevo contenedor. El código siguiente anexa un valor de GUID al nombre de contenedor para asegurarse de que sea único.

> [!IMPORTANT]
> Los nombres de contenedor deben estar en minúsculas. Para más información acerca de los contenedores de nomenclatura y los blobs, consulte [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Asignación de nombres y realización de referencias a contenedores, blobs y metadatos).

Cree una instancia de la clase [BlobServiceClient](/dotnet/api/azure.storage.blobs.blobserviceclient). A continuación, llame al método [CreateBlobContainerAsync](/dotnet/api/azure.storage.blobs.blobserviceclient.createblobcontainerasync) para crear el contenedor en la cuenta de almacenamiento.

Agregue este código al final del método `Main`:

:::code language="csharp" source="~/azure-storage-snippets/blobs/quickstarts/dotnet/BlobQuickstartV12/Program.cs" id="Snippet_CreateContainer":::

### <a name="upload-blobs-to-a-container"></a>Carga de los blobs en un contenedor

El siguiente fragmento de código:

1. Crea un archivo de texto en el directorio *data* local.
1. Obtiene una referencia a un objeto [BlobClient](/dotnet/api/azure.storage.blobs.blobclient) llamando al método [GetBlobClient](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobclient) en el contenedor de la sección [Crear un contenedor](#create-a-container).
1. Carga el archivo de texto local en el blob llamando al método [UploadAsync](/dotnet/api/azure.storage.blobs.blobclient.uploadasync#Azure_Storage_Blobs_BlobClient_UploadAsync_System_String_System_Boolean_System_Threading_CancellationToken_). Esta operación crea el blob si no existe y lo sobrescribe, en caso de que ya exista.

Agregue este código al final del método `Main`:

:::code language="csharp" source="~/azure-storage-snippets/blobs/quickstarts/dotnet/BlobQuickstartV12/Program.cs" id="Snippet_UploadBlobs":::

### <a name="list-the-blobs-in-a-container"></a>Enumerar los blobs de un contenedor

Enumere los blobs en el contenedor llamando al método [GetBlobsAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsasync). En este caso, solo se ha agregado un blob al contenedor, por lo que la operación de enumeración devuelve simplemente dicho blob.

Agregue este código al final del método `Main`:

:::code language="csharp" source="~/azure-storage-snippets/blobs/quickstarts/dotnet/BlobQuickstartV12/Program.cs" id="Snippet_ListBlobs":::

### <a name="download-blobs"></a>Descargar blobs

Llame al método [DownloadToAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.downloadtoasync) para descargar el blob creado previamente. El código de ejemplo agrega el sufijo "DOWNLOADED" al nombre de archivo para que pueda ver ambos archivos en el sistema de archivos local.

Agregue este código al final del método `Main`:

:::code language="csharp" source="~/azure-storage-snippets/blobs/quickstarts/dotnet/BlobQuickstartV12/Program.cs" id="Snippet_DownloadBlobs":::

### <a name="delete-a-container"></a>Eliminación de un contenedor

El código siguiente limpia los recursos que creó; para ello, elimina todo el contenedor con [DeleteAsync](/dotnet/api/azure.storage.blobs.blobcontainerclient.deleteasync). También elimina los archivos locales creados por la aplicación.

La aplicación se detiene para la entrada del usuario mediante una llamada a `Console.ReadLine` antes de eliminar el blob, el contenedor y los archivos locales. Esta es una buena oportunidad para verificar que los recursos se han creado correctamente antes de eliminarlos.

Agregue este código al final del método `Main`:

:::code language="csharp" source="~/azure-storage-snippets/blobs/quickstarts/dotnet/BlobQuickstartV12/Program.cs" id="Snippet_DeleteContainer":::

## <a name="run-the-code"></a>Ejecución del código

Esta aplicación crea un archivo de prueba en la carpeta *data* local y lo carga en Blob Storage. Después, en el ejemplo se enumeran los blobs del contenedor y se descarga el archivo con un nombre nuevo para que pueda comparar los archivos antiguo y nuevo.

Vaya al directorio de la aplicación y compile y ejecute la aplicación.

```console
dotnet build
```

```console
dotnet run
```

La salida de la aplicación es similar a la del ejemplo siguiente:

```output
Azure Blob Storage v12 - .NET quickstart sample

Uploading to Blob storage as blob:
         https://mystorageacct.blob.core.windows.net/quickstartblobs60c70d78-8d93-43ae-954d-8322058cfd64/quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31.txt

Listing blobs...
        quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31.txt

Downloading blob to
        ./data/quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31DOWNLOADED.txt

Press any key to begin clean up
Deleting blob container...
Deleting the local source and downloaded files...
Done
```

Antes de comenzar a limpiar el proceso, compruebe la carpeta *data* para los dos archivos. Puede abrirlos y ver que son idénticos.

Después de haber comprobado los archivos, presione la tecla **Entrar** para eliminar los archivos de prueba y terminar la demostración.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha aprendido a cargar, descargar y enumerar blobs mediante .NET.

Para ver las aplicaciones de ejemplo de Blob Storage, siga estos pasos:

> [!div class="nextstepaction"]
> [Ejemplos de Azure Blob Storage v12 para .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples)

- Para ver tutoriales, ejemplos, guías de inicio rápido y otra documentación, visite [Azure para desarrolladores de .NET y .NET Core](/dotnet/azure/).
- Para más información sobre .NET Core, consulte [Get started with .NET in 10 minutes](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro) (Introducción a .NET en 10 minutos).
