---
title: Cifrado del lado de cliente con .NET para el Microsoft Azure Storage | Microsoft Docs
description: La biblioteca de cliente de Azure Storage para .NET ofrece compatibilidad para el cifrado de cliente e integración con Azure Key Vault para obtener una seguridad máxima para sus aplicaciones de Azure Storage.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/16/2021
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.custom: devx-track-csharp
ms.openlocfilehash: 577a288d8dbe6afd7c05aa78e7055bdae288ce82
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128589309"
---
# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Cifrado del lado de cliente y Azure Key Vault para Microsoft Azure Storage

[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Información general

La [Biblioteca de cliente de Azure Storage para .NET](/dotnet/api/overview/azure/storage) permite tanto el cifrado de datos dentro de las aplicaciones de cliente antes de cargarlos en Azure Storage, como el descifrado de datos mientras estos se descargan al cliente. Asimismo, la biblioteca también admite la integración con [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para la administración de claves de la cuenta de almacenamiento.

Para ver un tutorial paso a paso que le guíe por el proceso de cifrado de blobs mediante el cifrado de cliente y Azure Key Vault, consulte [Cifrado y descifrado de blobs en Microsoft Azure Storage con Azure Key Vault](../blobs/storage-encrypt-decrypt-blobs-key-vault.md).

Para el cifrado del lado cliente con Java, consulte el artículo [Cifrado del lado cliente con Java para Microsoft Azure Storage](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Cifrado y descifrado a través de la técnica de sobres

El proceso de cifrado y descifrado sigue la técnica de sobres.

### <a name="encryption-via-the-envelope-technique"></a>Cifrado a través de la técnica de sobres

El cifrado mediante la técnica de sobres funciona de la siguiente manera:

1. La biblioteca de cliente de almacenamiento de Azure genera una clave de cifrado de contenido (CEK), que es una clave simétrica de un solo uso.
2. Los datos de usuario se cifran mediante esta CEK.
3. Se encapsula la CEK (cifrada) con la clave de cifrado de clave (KEK). La KEK se identifica mediante un identificador de clave y puede ser un par de clave asimétrico o una clave simétrica que puede administrarse de forma local o guardarse en Azure Key Vault.

    La propia biblioteca de cliente de almacenamiento no tiene nunca acceso a la KEK. La biblioteca invoca el algoritmo de encapsulado de clave proporcionado por Key Vault. Los usuarios pueden elegir utilizar proveedores personalizados para el ajuste y desajuste clave si lo desean.

4. A continuación, se cargan los datos cifrados en el servicio Azure Storage. La clave encapsulada y algunos metadatos adicionales de cifrado se almacenan como metadatos (en un blob) o se interpolan con los datos cifrados (cola de mensajes y las entidades de tabla).

### <a name="decryption-via-the-envelope-technique"></a>Descifrado a través de la técnica de sobres

El descifrado mediante la técnica de sobres funciona de la siguiente manera:

1. La biblioteca de cliente asume que el usuario está administrando la clave de cifrado de claves (KEK), ya sea localmente o en almacenes de claves de Azure. El usuario no necesita conocer la clave específica que se usó para el cifrado. En su lugar, se puede configurar y usar una resolución de clave que resuelva distintos identificadores de clave para las claves.
2. La biblioteca de cliente descarga los datos cifrados junto con cualquier material de cifrado que esté almacenado en el servicio.
3. A continuación, la clave de cifrado de contenido encapsulado (CEK) se desencapsula (descifra) usando la clave de cifrado de claves (KEK). Aquí nuevamente, la biblioteca de cliente no tiene acceso a la KEK. Simplemente, invoca el algoritmo de desencapsulado personalizado o el del proveedor de Key Vault.
4. La clave de cifrado de contenido (CEK) se usa entonces para descifrar los datos cifrados del usuario.

## <a name="encryption-mechanism"></a>Mecanismo de cifrado

La biblioteca de cliente de almacenamiento usa [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) para cifrar los datos del usuario. En concreto, emplea el modo [Cipher Block Chaining (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) con AES. Cada servicio funciona de forma ligeramente diferente, por lo que describiremos aquí cada uno de ellos.

### <a name="blobs"></a>Datos BLOB

La biblioteca de cliente solo admite actualmente el cifrado de blobs completos. En el caso de las descargas, se admiten tanto las descargas de intervalo como las completas.

Durante el cifrado, la biblioteca de cliente generará un vector de inicialización (IV) aleatorio de 16 bytes, junto con una clave de cifrado de contenido (CEK) aleatoria de 32 bytes, y realiza el cifrado de sobres de los datos de blob con esta información. Posteriormente, la CEK encapsulada y algunos metadatos de cifrado adicionales se almacenan como metadatos de blob junto con el objeto blob cifrado en el servicio.

> [!WARNING]
> Si está modificando o cargando sus propios metadatos para el blob, deberá asegurarse de que estos metadatos se conserven. Si carga nuevos metadatos sin estos metadatos, la CEK encapsulada, IV y otros metadatos se perderán y el contenido del blob nunca podrá recuperarse nuevamente.

Al descargar un blob completo, la CEK encapsulada se desencapsula y se utiliza junto con el vector de inicialización (que en este caso se almacena como metadatos de blob) para devolver los datos descifrados a los usuarios.

Descargar un intervalo arbitrario en el blob cifrado implica ajustar el intervalo proporcionado por los usuarios para obtener una pequeña cantidad de datos adicionales que puedan usarse para descifrar correctamente el intervalo solicitado.

Todos los tipos de blobs (blobs en bloques, blobs de anexión) se pueden cifrar y descifrar usando este esquema.

### <a name="queues"></a>Colas

Dado que la cola de mensajes puede tener cualquier formato, la biblioteca de cliente define un formato personalizado que incluye el vector de inicialización (IV) y la clave de cifrado de contenido (CEK) cifrada en el texto del mensaje.

Durante el cifrado, la biblioteca de cliente genera un vector de inicialización aleatorio de 16 bytes junto con una CEK aleatoria de 32 bytes y realiza el cifrado de sobres del texto del mensaje de cola con esta información. La CEK encapsulada y algunos metadatos de cifrado adicionales se incluyen entonces en el mensaje de cola cifrado. Este mensaje modificado (que se muestra a continuación) se almacena en el servicio.

```xml
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

Durante el descifrado, la clave encapsulada se extrae del mensaje de cola y se desencapsula. El vector de inicialización también se extrae del mensaje de cola y se utiliza junto con la clave desencapsulada para descifrar los datos de mensaje de cola. Tenga en cuenta que los metadatos de cifrado son pequeños (menos de 500 bytes), por lo que, aunque se tienen en cuenta para el límite de 64 KB para un mensaje de la cola, el impacto debería ser fácil de administrar. Tenga en cuenta que el mensaje cifrado estará codificado en Base64, como se muestra en el fragmento de código anterior, lo que también expandirá el tamaño del mensaje que se envía.

### <a name="tables"></a>Tablas

> [!NOTE]
> Table service solo se admite hasta la versión 9.x de la biblioteca cliente de Azure Storage.

La biblioteca de cliente admite el cifrado de propiedades de entidad para operaciones de insertar y reemplazar.

> [!NOTE]
> Las operaciones de combinación no se admiten actualmente. Puesto que un subconjunto de propiedades puede haberse cifrado previamente con una clave distinta, si simplemente se combinan las nuevas propiedades y se actualizan los metadatos, se producirá una pérdida de datos. Para realizar una combinación es necesario realizar llamadas de servicio adicionales para leer la entidad existente desde el servicio. También puede usar una nueva clave por propiedad. Ninguno de estos procedimientos es adecuado por motivos de rendimiento.

El cifrado de datos de tabla funciona de la siguiente forma:

1. Los usuarios especifican las propiedades que se van a cifrar.
2. La biblioteca de cliente genera un vector de inicialización (IV) aleatorio de 16 bytes junto con una clave de cifrado de contenido (CEK) aleatoria de 32 bytes para cada entidad. Después, realiza el cifrado de sobres en las propiedades individuales que se van a cifrar derivando un nuevo vector de inicialización por propiedad. La propiedad de cifrado se almacena como datos binarios.
3. La CEK encapsulada y algunos metadatos de cifrado adicional se almacenan posteriormente como dos propiedades reservadas adicionales. La primera propiedad reservada (_ClientEncryptionMetadata1) es una propiedad de cadena que contiene la información sobre el vector de inicialización, la versión y la clave encapsulada. La segunda propiedad reservada (_ClientEncryptionMetadata2) es una propiedad binaria que contiene la información sobre las propiedades que se cifran. La información de esta segunda propiedad (_ClientEncryptionMetadata2) está ella misma cifrada.
4. A causa de estas propiedades reservadas adicionales necesarias para el cifrado, es posible que los usuarios ahora tengan solo 250 propiedades personalizadas en lugar de 252. El tamaño total de la entidad debe ser inferior a 1 MB.

Tenga en cuenta que solo se pueden cifrar las propiedades de cadena. Si hay que cifrar otros tipos de propiedades, habrá que convertirlas en cadenas. Las cadenas cifradas se almacenan en el servicio como propiedades binarias y se convierten de nuevo en cadenas después del descifrado.

Para las tablas, además de la directiva de cifrado, los usuarios deben especificar las propiedades que se van a cifrar. Para ello, pueden especificar un atributo EncryptProperty (para las entidades POCO que se derivan de TableEntity) o una resolución de cifrado en las opciones de solicitud. Una resolución de cifrado es un delegado que toma una clave de partición, una clave de fila y un nombre de propiedad y devuelve un valor booleano que indica si se debe cifrar dicha propiedad. Durante el cifrado, la biblioteca de cliente usará esta información para decidir si se debe cifrar una propiedad mientras se escribe en la conexión. El delegado también proporciona la posibilidad de lógica con respecto a la forma de cifrar las propiedades. (Por ejemplo, si el valor es X, hay que cifrar la propiedad A; en caso contrario, hay que cifrar las propiedades A y B). Tenga en cuenta que no es necesario proporcionar esta información para leer o consultar entidades.

### <a name="batch-operations"></a>Operaciones por lotes

En las operaciones por lotes, se usará la misma KEK en todas las filas de esa operación por lotes porque la biblioteca de cliente solo permite un objeto de opciones (y, por lo tanto, una directiva/KEK) por cada operación por lotes. Sin embargo, la biblioteca de cliente generará internamente un nuevo vector de inicialización aleatorio y una CEK aleatoria por cada fila del lote. Los usuarios también pueden optar por cifrar diferentes propiedades para cada operación del lote mediante la definición de este comportamiento en la resolución de cifrado.

### <a name="queries"></a>Consultas

> [!NOTE]
> Dado que las entidades están cifradas, no se pueden ejecutar consultas que filtran por una propiedad cifrada.  Si lo intenta, los resultados serán incorrectos, porque el servicio estaría intentando comparar los datos cifrados con los datos sin cifrar.
>
> Para realizar operaciones de consulta, debe especificar a una resolución de clave que sea capaz de resolver todas las claves en el conjunto de resultados. Si una entidad incluida en el resultado de la consulta no se puede resolver en un proveedor, la biblioteca de cliente producirá un error. Para cualquier consulta que realice proyecciones del lado servidor, la biblioteca de cliente agregará las propiedades de metadatos de cifrado especiales (_ClientEncryptionMetadata1 y _ClientEncryptionMetadata2) a las columnas seleccionadas de forma predeterminada.

## <a name="azure-key-vault"></a>Azure Key Vault

Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Con Azure Key Vault, los usuarios pueden cifrar claves y secretos (por ejemplo, claves de autenticación, claves de cuenta de almacenamiento, claves de cifrado de datos, archivos .PFX y contraseñas) usando claves que están protegidas por módulos de seguridad de hardware (HSM). Para obtener más información, consulte [¿Qué es Azure Key Vault?](../../key-vault/general/overview.md)

La biblioteca de cliente de almacenamiento utiliza las interfaces de Key Vault en la biblioteca básica a fin de proporcionar un marco común en Azure para administrar las claves. Los usuarios pueden aprovechar todas las ventajas adicionales que proporcionan las bibliotecas de Key Vault, como una funcionalidad útil para los proveedores de claves en la nube y locales simétricas/RSA así como también ayuda para la agregación y el almacenamiento en caché.

### <a name="interface-and-dependencies"></a>Interfaz y dependencias

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Hay dos paquetes necesarios para la integración de Key Vault:

- Azure.Core contiene las interfaces `IKeyEncryptionKey` y `IKeyEncryptionKeyResolver`. La biblioteca cliente de almacenamiento para .NET ya lo define como una dependencia.
- Azure.Security.KeyVault.Keys (versión 4.x) contiene el cliente REST de Key Vault así como clientes criptográficos que se usan con el cifrado del lado cliente.

Key Vault está diseñado para claves maestras de gran valor. Por su parte, los valores de limitación por cada almacén de claves se diseñan teniendo en cuenta este aspecto. A partir de Azure.Security.KeyVault.Keys 4.1.0, no hay ninguna implementación `IKeyEncryptionKeyResolver` que admita el almacenamiento en caché de claves. Si el almacenamiento en caché es necesario debido a una limitación, se puede seguir [este ejemplo](/samples/azure/azure-sdk-for-net/azure-key-vault-proxy/) para insertar una capa de almacenamiento en caché en una instancia de `Azure.Security.KeyVault.Keys.Cryptography.KeyResolver`.

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

Hay tres paquetes de Key Vault:

- Microsoft.Azure.KeyVault.Core contiene IKey e IKeyResolver. Es un paquete pequeño sin dependencias. La biblioteca de cliente de almacenamiento para .NET lo define como dependencia.
- Microsoft.Azure.KeyVault (versión 3.x) contiene el cliente REST de Key Vault.
- Microsoft.Azure.KeyVault.Extensions (versión 3.x) contiene el código de extensión que incluye implementaciones de algoritmos criptográficos, además de una RSAKey y una SymmetricKey. Depende de los espacios de nombres principales y KeyVault. Proporciona funcionalidad para definir una resolución de agregado (cuando los usuarios desean utilizar varios proveedores de clave) y una resolución de clave de almacenamiento en caché. Aunque la biblioteca de cliente de almacenamiento no depende directamente de este paquete, si los usuarios desean usar Azure Key Vault para almacenar sus claves o utilizar las extensiones de Key Vault para consumir los proveedores de servicios criptográficos locales y en la nube, necesitarán este paquete.

Key Vault está diseñado para claves maestras de gran valor. Por su parte, los valores de limitación por cada almacén de claves se diseñan teniendo en cuenta este aspecto. Al realizar el cifrado en el lado cliente con Key Vault, el modelo preferido es usar las claves maestras simétricas almacenadas como secretos en Key Vault y almacenadas en caché localmente. Los usuarios deben hacer lo siguiente:

1. Crear un secreto sin conexión y cargarlo en  Key Vault.
2. Usar el identificador de base del secreto como un parámetro para resolver la versión actual del secreto para el cifrado y el almacenamiento en caché de esta información localmente. Usar CachingKeyResolver para el almacenamiento en caché (los usuarios no deben implementar su propia lógica de almacenamiento en caché).
3. Utilizar la resolución de caché como una entrada al crear la directiva de cifrado.

Puede encontrar más información sobre el uso de Key Vault en la versión 11 en los [ejemplos de código de cifrado versión 11](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

---

## <a name="best-practices"></a>Procedimientos recomendados

La compatibilidad con el cifrado solo está disponible en la biblioteca de cliente de almacenamiento para .NET. Windows Phone y Windows en tiempo de ejecución no admiten actualmente cifrado.

> [!IMPORTANT]
> Tenga en cuenta estos puntos importantes al usar el cifrado del lado del cliente:
>
> - Al leer desde un blob cifrado o escribir en él, utilice comandos de carga completa del blob y comandos de descarga de blobs de intervalo/completos. Evite escribir en un blob cifrado mediante operaciones de protocolo, como Colocar bloque, Colocar lista de bloque, Escribir páginas, Borrar páginas o Anexar bloque; de lo contrario, puede dañar el objeto blob cifrado y que no sea legible.
> - Para las tablas, existe una restricción similar. Tenga cuidado de no actualizar propiedades cifradas sin actualizar los metadatos de cifrado.
> - Si establece los metadatos en el objeto blob cifrado, puede sobrescribir los metadatos relacionados con el cifrado necesarios para el descifrado, ya que el establecimiento de metadatos no es aditivo. Esto también se aplica a las instantáneas: evite la especificación de metadatos durante la creación de una instantánea de un blob cifrado. Si se deben establecer metadatos, asegúrese de llamar primero al método **FetchAttributes** , para así obtener los metadatos de cifrado actuales y evitar las escrituras simultáneas mientras se estos se establecen.
> - Habilite la propiedad **RequireEncryption** en las opciones de solicitud predeterminadas para los usuarios que deben trabajar solo con datos cifrados. Vea a continuación para obtener más información.

## <a name="client-api--interface"></a>Interfaz/API de cliente

Los usuarios pueden proporcionar solo una clave, solo una resolución, o ambas. Las claves se identifican con un identificador de claves y proporcionan la lógica de ajuste o desajuste. Las resoluciones se usan para resolver una clave durante el proceso de descifrado. Define un método de resolución que devuelve una clave en función de un identificador de clave. Esto ofrece a los usuarios la posibilidad de elegir entre varias claves que se administran en varias ubicaciones.

- Para el cifrado, se utiliza siempre la clave. Si no hay clave, se producirá un error.
- Para el descifrado:
  - Si se especifica la clave y su identificador coincide con el identificador de clave requerido, esa clase se usa para el descifrado. De lo contrario, se intenta con la resolución. Si no hay resolución para este intento, se genera un error.
  - La resolución de claves se invoca si se especifica para obtener la clave. Si se especifica la resolución, pero no se proporciona una asignación para el identificador de clave, se produce un error.

### <a name="requireencryption-mode-v11-only"></a>Modo RequireEncryption (solo la versión 11)

Los usuarios pueden habilitar opcionalmente un modo de operación en el que se deben cifrar todas las cargas y descargas. En este modo, los intentos de cargar datos sin una directiva de cifrado o de descargar datos no cifrados en el servicio generarán un error en el cliente. Este comportamiento lo controla la propiedad **RequireEncryption** de los controles de objeto de las opciones de solicitud. Si la aplicación va a cifrar todos los objetos almacenados en Azure Storage, puede establecer la propiedad **RequireEncryption** en las opciones de solicitud predeterminadas del objeto de cliente de servicio. Por ejemplo, establezca **CloudBlobClient.DefaultRequestOptions.RequireEncryption** en **true** para requerir que se realice el cifrado para todas las operaciones de blobs realizadas a través de ese objeto de cliente.

### <a name="blob-service-encryption"></a>Cifrado de Blob service

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Cree un objeto **ClientSideEncryptionOptions** y establézcalo en la creación de cliente con **SpecializedBlobClientOptions**. No se pueden establecer opciones de cifrado por API. Todo lo demás lo controlará la biblioteca de cliente internamente.

```csharp
// Your key and key resolver instances, either through KeyVault SDK or an external implementation
IKeyEncryptionKey key;
IKeyEncryptionKeyResolver keyResolver;

// Create the encryption options to be used for upload and download.
ClientSideEncryptionOptions encryptionOptions = new ClientSideEncryptionOptions(ClientSideEncryptionVersion.V1_0)
{
   KeyEncryptionKey = key,
   KeyResolver = keyResolver,
   // string the storage client will use when calling IKeyEncryptionKey.WrapKey()
   KeyWrapAlgorithm = "some algorithm name"
};

// Set the encryption options on the client options
BlobClientOptions options = new SpecializedBlobClientOptions() { ClientSideEncryption = encryptionOptions };

// Get your blob client with client-side encryption enabled.
// Client-side encryption options are passed from service to container clients, and container to blob clients.
// Attempting to construct a BlockBlobClient, PageBlobClient, or AppendBlobClient from a BlobContainerClient
// with client-side encryption options present will throw, as this functionality is only supported with BlobClient.
BlobClient blob = new BlobServiceClient(connectionString, options).GetBlobContainerClient("my-container").GetBlobClient("myBlob");

// Upload the encrypted contents to the blob.
blob.Upload(stream);

// Download and decrypt the encrypted contents from the blob.
MemoryStream outputStream = new MemoryStream();
blob.DownloadTo(outputStream);
```

No es necesario un **BlobServiceClient** para aplicar las opciones de cifrado. También se pueden pasar a las construcciones **BlobContainerClient**/**BlobClient** que aceptan objetos **BlobClientOptions**.

Si ya hay un objeto **BlobClient** deseado, pero sin las opciones de cifrado del lado cliente, existe un método de extensión para crear una copia de ese objeto con las **ClientSideEncryptionOptions** especificadas. Este método de extensión evita la sobrecarga que implica construir un objeto **BlobClient** desde cero.

```csharp
using Azure.Storage.Blobs.Specialized;

// Your existing BlobClient instance and encryption options
BlobClient plaintextBlob;
ClientSideEncryptionOptions encryptionOptions;

// Get a copy of plaintextBlob that uses client-side encryption
BlobClient clientSideEncryptionBlob = plaintextBlob.WithClientSideEncryptionOptions(encryptionOptions);
```

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

Cree un objeto **BlobEncryptionPolicy** y configúrelo en las opciones de solicitud (por API o en un nivel de cliente mediante el elemento **DefaultRequestOptions**). Todo lo demás lo controlará la biblioteca de cliente internamente.

```csharp
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

// Set the encryption policy on the request options.
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Upload the encrypted contents to the blob.
blob.UploadFromStream(stream, size, null, options, null);

// Download and decrypt the encrypted contents from the blob.
MemoryStream outputStream = new MemoryStream();
blob.DownloadToStream(outputStream, null, options, null);
```

---

### <a name="queue-service-encryption"></a>Cifrado del servicio Cola

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Cree un objeto **ClientSideEncryptionOptions** y establézcalo en la creación de cliente con **SpecializedQueueClientOptions**. No se pueden establecer opciones de cifrado por API. Todo lo demás lo controlará la biblioteca de cliente internamente.

```csharp
// Your key and key resolver instances, either through KeyVault SDK or an external implementation
IKeyEncryptionKey key;
IKeyEncryptionKeyResolver keyResolver;

// Create the encryption options to be used for upload and download.
ClientSideEncryptionOptions encryptionOptions = new ClientSideEncryptionOptions(ClientSideEncryptionVersion.V1_0)
{
   KeyEncryptionKey = key,
   KeyResolver = keyResolver,
   // string the storage client will use when calling IKeyEncryptionKey.WrapKey()
   KeyWrapAlgorithm = "some algorithm name"
};

// Set the encryption options on the client options
QueueClientOptions options = new SpecializedQueueClientOptions() { ClientSideEncryption = encryptionOptions };

// Get your queue client with client-side encryption enabled.
// Client-side encryption options are passed from service to queue clients.
QueueClient queue = new QueueServiceClient(connectionString, options).GetQueueClient("myQueue");

// Send an encrypted queue message.
queue.SendMessage("Hello, World!");

// Download queue messages, decrypting ones that are detected to be encrypted
QueueMessage[] queue.ReceiveMessages(); 
```

No es necesario un **QueueServiceClient** para aplicar las opciones de cifrado. También se pueden pasar a las construcciones **QueueClient** que aceptan objetos **QueueClientOptions**.

Si ya hay un objeto **QueueClient** deseado, pero sin las opciones de cifrado del lado cliente, existe un método de extensión para crear una copia de ese objeto con las **ClientSideEncryptionOptions** especificadas. Este método de extensión evita la sobrecarga que implica construir un objeto **QueueClient** desde cero.

```csharp
using Azure.Storage.Queues.Specialized;

// Your existing QueueClient instance and encryption options
QueueClient plaintextQueue;
ClientSideEncryptionOptions encryptionOptions;

// Get a copy of plaintextQueue that uses client-side encryption
QueueClient clientSideEncryptionQueue = plaintextQueue.WithClientSideEncryptionOptions(encryptionOptions);
```

Algunos usuarios pueden tener colas donde no todos los mensajes recibidos se pueden descifrar correctamente y se debe iniciar la clave o la resolución. En este caso, se generará la línea final del ejemplo anterior y no se podrá acceder a ninguno de los mensajes recibidos. En estos escenarios, se puede usar la subclase **QueueClientSideEncryptionOptions** para ofrecer opciones de cifrado a los clientes. Expone un evento **DecryptionFailed** que se desencadenará cada vez que no se pueda descifrar un mensaje de la cola, siempre que se haya agregado al menos una invocación al evento. Los mensajes con errores individuales se pueden administrar de esta manera y se filtrarán del **QueueMessage[]** final devuelto por **ReceiveMessages**.

```csharp
// Create your encryption options using the sub-class.
QueueClientSideEncryptionOptions encryptionOptions = new QueueClientSideEncryptionOptions(ClientSideEncryptionVersion.V1_0)
{
   KeyEncryptionKey = key,
   KeyResolver = keyResolver,
   // string the storage client will use when calling IKeyEncryptionKey.WrapKey()
   KeyWrapAlgorithm = "some algorithm name"
};

// Add a handler to the DecryptionFailed event.
encryptionOptions.DecryptionFailed += (source, args) => {
   QueueMessage failedMessage = (QueueMessage)source;
   Exception exceptionThrown = args.Exception;
   // do something
};

// Use these options with your client objects.
QueueClient queue = new QueueClient(connectionString, queueName, new SpecializedQueueClientOptions()
{
   ClientSideEncryption = encryptionOptions
});

// Retrieve 5 messages from the queue.
// Assume 5 messages come back and one throws during decryption.
QueueMessage[] messages = queue.ReceiveMessages(maxMessages: 5).Value;
Debug.Assert(messages.Length == 4)
```

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

Cree un objeto **QueueEncryptionPolicy** y configúrelo en las opciones de solicitud (por API o en el nivel del cliente usando **DefaultRequestOptions**). Todo lo demás lo controlará la biblioteca de cliente internamente.

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

 // Add message
 QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
 queue.AddMessage(message, null, null, options, null);

 // Retrieve message
 CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);
```

---

### <a name="table-service-encryption-v11-only"></a>Cifrado de Table service (solo la versión 11)

Además de crear una directiva de cifrado y configurarla en las opciones de solicitud, debe especificar un elemento **EncryptionResolver** en **TableRequestOptions** o establecer el atributo [EncryptProperty] en la entidad.

#### <a name="using-the-resolver"></a>Uso de la resolución

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

 TableRequestOptions options = new TableRequestOptions()
 {
    EncryptionResolver = (pk, rk, propName) =>
     {
        if (propName == "foo")
         {
            return true;
         }
         return false;
     },
     EncryptionPolicy = policy
 };

 // Insert Entity
 currentTable.Execute(TableOperation.Insert(ent), options, null);

 // Retrieve Entity
 // No need to specify an encryption resolver for retrieve
 TableRequestOptions retrieveOptions = new TableRequestOptions()
 {
    EncryptionPolicy = policy
 };

 TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
 TableResult result = currentTable.Execute(operation, retrieveOptions, null);
```

#### <a name="using-attributes"></a>Uso de atributos

Tal como se mencionó anteriormente, si la entidad implementa TableEntity, las propiedades se pueden completar con el atributo [EncryptProperty] en lugar de especificar **EncryptionResolver**.

```csharp
[EncryptProperty]
 public string EncryptedProperty1 { get; set; }
```

## <a name="encryption-and-performance"></a>Cifrado y rendimiento

Tenga en cuenta que el cifrado de sus resultados de datos de almacenamiento da lugar a la sobrecarga de rendimiento adicional. Se deben generar la clave de contenido e IV, se debe cifrar el propio contenido y se deben formatear y cargar metadatos adicionales. Esta sobrecarga variará según la cantidad de datos que se cifran. Se recomienda que los clientes prueben siempre sus aplicaciones para obtener un rendimiento durante el desarrollo.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Cifrado y descifrado de blobs en Microsoft Azure Storage con Azure Key Vault](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)
- Descargar la [Biblioteca de cliente de Azure Storage para el paquete NuGet de .NET](https://www.nuget.org/packages/WindowsAzure.Storage)
- Descargar los paquetes de NuGet [Básico](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [Cliente](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/) y [Extensiones](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) de Azure Key Vault
- Consulte la [documentación de Azure Key Vault](../../key-vault/general/overview.md)
