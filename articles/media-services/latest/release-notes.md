---
title: Notas de la versión de Azure Media Services v3
description: Para mantenerse al día con los últimos desarrollos, en este artículo se proporcionan las actualizaciones más reciente en Azure Media Services v3.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: na
ms.custom: references_regions
ms.topic: article
ms.date: 03/17/2021
ms.author: inhenkel
ms.openlocfilehash: 4a2c4959d6a84e8561ac23924207744b6c65f88b
ms.sourcegitcommit: 48500a6a9002b48ed94c65e9598f049f3d6db60c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2021
ms.locfileid: "129053489"
---
# <a name="azure-media-services-v3-release-notes"></a>Notas de la versión de Azure Media Services v3

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

* Versiones más recientes
* Problemas conocidos
* Corrección de errores
* Funciones obsoletas

## <a name="september-2021"></a>Septiembre de 2021


### <a name="improved-scale-management-and-monitoring-for-a-streaming-endpoint-in-the-portal"></a>Mejora de la administración y la supervisión del escalado de un punto de conexión de streaming en el portal

La página del portal del punto de conexión de streaming ahora proporciona una manera sencilla de administrar la capacidad de salida y calcular el alcance de la audiencia con y sin un CDN configurado.  Basta con ajustar la velocidad de bits de entrega y el porcentaje de aciertos de la caché de la CDN previsto para obtener estimaciones rápidas del tamaño de su audiencia y ayudarle a determinar si necesita escalar verticalmente a más puntos de conexión de streaming prémium.

   [ ![Escalado y supervisión de los puntos de conexión de streaming en el portal](./media/release-notes/streaming-endpoint-monitor-inline.png) ](./media/release-notes/streaming-endpoint-monitor.png#lightbox)

### <a name="streaming-endpoint-portal-page-now-shows-cpu-egress-and-latency-metrics"></a>La página del portal "Punto de conexión de streaming" muestra ahora las métricas de CPU, salida y latencia

Ahora puede visualizar la carga de CPU, el ancho de banda de salida y las métricas de latencia completas de sus puntos de conexión de streaming en Azure Portal. Ahora puede crear alertas de supervisión basadas en las métricas de CPU, salida o latencia directamente en el portal mediante la potencia de Azure Monitor.

### <a name="user-assigned-managed-identities-support-for-media-services-accounts"></a>Compatibilidad con identidades administradas asignadas por el usuario en cuentas de Media Services

Con las identidades administradas asignadas por el usuario, los clientes ahora podrán permitir una mejor seguridad de sus cuentas de almacenamiento y los almacenes de claves asociados. El acceso a la cuenta de almacenamiento del cliente y a los almacenes de claves se limitará a la identidad administrada asignada por el usuario.  Tendrá control total sobre la duración de las identidades administradas por el usuario y podrá revocar fácilmente el acceso de la cuenta de servicio multimedia a cualquier cuenta de almacenamiento específica según sea necesario.

### <a name="media-services-storage-accounts-page-in-the-portal-now-support-both-uami-and-sami"></a>La página de cuentas de almacenamiento de Media Services del portal ahora admite UAMI y SAMI.

Ahora puede asignar y administrar identidades administradas asignadas por el usuario (UAMI) o identidades administradas asignadas por el sistema (SAMI) para las cuentas de almacenamiento directamente en Azure Portal para Media Services.

### <a name="bring-your-own-key-page-now-also-supports-both-uami-and-sami"></a>La página "Bring your own key" ahora también admite UAMI y SAMI.
La página del portal de administración de claves de Media Services ahora admite la configuración y la administración de identidades administradas asignadas por el usuario (UAMI) o de identidades administradas asignadas por el sistema (SAMI).

   [ ![Traer sus propias claves para el cifrado de las cuentas](./media/release-notes/byok-managed-identity.png)](./media/release-notes/byok-managed-identity.png)


### <a name="private-link-support-for-media-services"></a>Compatibilidad con Private Link en Media Services
Ahora puede restringir el acceso público a los eventos en directo, los puntos de conexión de streaming y el punto de conexión de servicios de entrega de claves para la protección de contenido y la administración de derechos digitales mediante la creación de un punto de conexión privado para cada uno de los servicios. De esta forma, se limitará el acceso público a cada uno de estos servicios. Solo el tráfico que se origina en la red virtual (VNET) configurada en el punto de conexión privado podrá acceder a estos puntos de conexión.

### <a name="ip-allow-list-for-key-service"></a>Lista de direcciones IP permitidos para el servicio de claves
Ahora puede optar por permitir que determinadas direcciones IP públicas tengan acceso al servicio de entrega de claves para la protección de contenido y la administración de derechos digitales. Los puntos de conexión de streaming y de eventos en directo ya admiten la configuración de la lista de direcciones IP permitidas en sus respectivas páginas.

Ahora también dispone de una marca de características de nivel de cuenta para permitir o bloquear el acceso a la cuenta de Media Services desde la red pública de Internet.

## <a name="july-2021"></a>Julio de 2021

### <a name="net-sdk-microsoftazuremanagementmedia--500-release-available-in-nuget"></a>Versión del SDK de .NET (Microsoft.Azure.Management.Media ) 5.0.0 disponible en NuGet

La versión 5.0.0 del SDK de .NET [Microsoft.Azure.Management.Media](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/5.0.0) ya se ha publicado en NuGet. Esta versión se genera para funcionar con la versión [estable 2021-06-01](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2021-06-01) de la API REST de ARM de Open API (Swagger).

Para más información sobre los cambios de la versión 4.0.0, consulte el [registro de cambios](https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/mediaservices/Microsoft.Azure.Management.Media/CHANGELOG.md).

#### <a name="changes-in-the-500-net-sdk-release"></a>Cambios de la versión del SDK de .NET 5.0.0

* La cuenta de Media Services ahora admite identidades administradas asignadas por el usuario y por el sistema.
* Se ha agregado la opción **PublicNetworkAccess** para cuentas de Media Services. Esta opción se puede usar con la característica Private Link para permitir solo el acceso desde redes privadas, bloqueando todo el acceso a la red pública.
* Paso a través básico: se agrega un nuevo tipo de evento en directo. Los eventos en directo de "tránsito básico" tienen funcionalidades similares a los eventos en directo de paso a través estándar con algunas restricciones de entrada y salida, y se ofrecen a un precio reducido.
* **PresetConfigurations**: permite personalizar la configuración de salida y velocidades de bits mínimas y máximas que se usan para los [valores preestablecidos de codificación que tienen en cuenta el contenido](./encode-content-aware-concept.md). Esto le ayuda a calcular mejor y planear una facturación más precisa al usar la codificación que tiene en cuenta el contenido a través de números y resoluciones de seguimiento de salida restringidos.

#### <a name="breaking-changes-in-tht-500-net-sdk-release"></a>Cambios importantes en la versión 5.0.0 del SDK de .NET

* **ApiErrorException** se ha reemplazado por **ErrorResponseException** para ser coherente con todos los demás SDK de Azure. El cuerpo de la excepción no ha cambiado.
* Todas las llamadas que devuelven 404 No encontrado ahora inician una excepción **ErrorResponseException** en lugar de devolver un valor Null. Este cambio se realizó por coherencia con otros SDK de Azure.
* El constructor del servicio multimedia tiene un nuevo parámetro PublicNetworkAccess opcional después del parámetro KeyDelivery.
* La propiedad Type de **MediaServiceIdentity** ha cambiado de enumeración ManagedIdentityType a cadena, para dar cabida a varios valores separados por comas. Las cadenas válidas son **SystemAssigned** o **UserAssigned**.

## <a name="june-2021"></a>Junio de 2021

### <a name="additional-live-event-ingest-heartbeat-properties-for-improved-diagnostics"></a>Propiedades adicionales de latido de ingesta de eventos en directo para un diagnóstico mejorado

Se han agregado propiedades adicionales de latido de ingesta de eventos en directo al mensaje de Event Grid. Esto incluye los siguientes nuevos campos para ayudar con el diagnóstico de problemas durante la ingesta en directo.  **ingestDriftValue** es útil en escenarios en los que es necesario supervisar la latencia de red desde el codificador de ingesta de origen que se inserta en el evento en directo. Si este valor presenta un desfase demasiado grande, esto puede indicar que la latencia de red es demasiado alta para un evento de streaming en vivo correcto.

Consulte el [esquema de LiveEventIngestHeartbeat](./monitoring/media-services-event-schemas.md#liveeventingestheartbeat) para más información.

### <a name="private-links-support-is-now-ga"></a>La compatibilidad con vínculos privados ahora está disponible de forma general.

La compatibilidad del uso de Media Services con [vínculos privados](../../private-link/index.yml) ahora tiene disponibilidad general y está disponible en todas las regiones de Azure, incluidas las nubes de Azure Government.
Azure Private Link le permite acceder a los servicios PaaS de Azure y a los servicios hospedados en Azure que son propiedad de los clientes, o a los servicios de asociados, a través de un punto de conexión privado de la red virtual.
El tráfico entre la red virtual y el servicio atraviesa la red troncal de Microsoft, eliminando la exposición a la red pública de Internet.

Para más información sobre cómo usar Media Services con vínculos privados, consulte [Creación de una cuenta de Media Services y una cuenta de almacenamiento con un vínculo privado](./security-private-link-how-to.md).

### <a name="new-us-west-3-region-is-ga"></a>La nueva región Oeste de EE. UU. 3 tiene disponibilidad general.

La región Oeste de EE. UU. 3 ahora tiene disponibilidad general y está disponible para que los clientes la usen al crear nuevas cuentas de Media Services.

### <a name="key-delivery-supports-ip-allow-list-restrictions"></a>La entrega de claves admite restricciones de lista de direcciones IP permitidas.

Las cuentas de Media Services ahora se pueden configurar con restricciones de lista de direcciones IP permitidas en la entrega de claves. La nueva configuración de la lista de permitidos está disponible en el recurso de la cuenta de Media Services mediante el SDK, así como en el portal y la CLI.
Esto permite a los operadores restringir la entrega de licencias DRM y claves de contenido AES-128 a intervalos IPv4 específicos.

Esta característica también se puede usar para interrumpir toda entrega de licencias DRM o claves AES-128 desde la red pública de Internet y restringir la entrega a un punto de conexión de red privado.

Consulte el articulo [Restricción del acceso a la entrega de licencias de DRM y claves AES mediante listas de direcciones IP permitidas](./drm-content-protection-key-delivery-ip-allow.md) para más información.

### <a name="new-samples-for-python-and-nodejs-with-typescript"></a>Nuevos ejemplos para Python y Node.js (con Typescript)
Ejemplos actualizados de **Node.js** que emplean la compatibilidad más reciente de Typescript en el SDK de Azure.

|Muestra|Descripción|
|---|---|
|[Streaming en directo](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/Live/index.ts)| Ejemplo básico de streaming en vivo. **ADVERTENCIA**: asegúrese de comprobar que todos los recursos se han limpiado y ya no se facturan en el portal al usar en producción.|
|[Carga y transmitir con HLS y DASH](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/StreamFilesSample/index.ts)| Ejemplo básico para cargar un archivo local o codificación de una dirección URL de origen. En el ejemplo se muestra cómo usar el SDK de Storage para descargar contenido y se muestra cómo transmitir a un reproductor. |
|[Carga y transmisión mediante HLS y DASH con DRM de Playready y Widevine](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/StreamFilesWithDRMSample/index.ts)| Muestra cómo codificar y transmitir mediante DRM de Widevine y PlayReady. |
|[Carga y uso de IA para indexar vídeos y audio](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/main/AMSv3Samples/VideoIndexerSample/index.ts)| Ejemplo de uso de los valores preestablecidos del analizador de vídeo y audio para generar metadatos e información a partir de un archivo de vídeo o audio. |


Nuevo ejemplo de **Python** que muestra cómo usar Azure Functions y Event Grid para desencadenar los valores preestablecidos del difuminado de caras.

|Muestra|Descripción|
|---|---|
|[Difuminado de caras mediante eventos y funciones](https://github.com/Azure-Samples/media-services-v3-python/tree/main/VideoAnalytics/FaceRedactorEventBased) | Este es un ejemplo de un enfoque basado en eventos que desencadena un trabajo del difuminador de caras de Azure Media Services en un vídeo en cuanto llega a una cuenta de Azure Storage. Aprovecha las ventajas de Azure Media Services, Azure Functions, Event Grid y Azure Storage para la solución. Para obtener la descripción completa de la solución, consulte el archivo [README.md](https://github.com/Azure-Samples/media-services-v3-python/blob/main/VideoAnalytics/FaceRedactorEventBased/README.md). |


## <a name="may-2021"></a>Mayo de 2021

### <a name="availability-zones-default-support-in-media-services"></a>Compatibilidad predeterminada con Availability Zones en Media Services

Media Services ahora es compatible con [Availability Zones](concept-availability-zones.md), que proporciona ubicaciones con aislamiento de errores dentro de la misma región de Azure.  Las cuentas de Media Services ahora tienen redundancia de zona de forma predeterminada y no se requiere ninguna configuración adicional. Esto solo se aplica a las regiones que tienen [compatibilidad con Availability Zones](../../availability-zones/az-region.md#azure-regions-with-availability-zones).


## <a name="march-2021"></a>Marzo de 2021

### <a name="new-language-support-added-to-the-audioanalyzer-preset"></a>Se ha agregado compatibilidad con nuevos idiomas al valor preestablecido de AudioAnalyzer

Ahora hay más idiomas disponibles para la transcripción y el subtitulado de vídeos en el valor preestablecido de AudioAnalyzer (modos Básico y Estándar).

* Inglés (Australia), "en-AU"
* Francés (Canadá), "fr-CA"
* Árabe (Bahréin) estándar moderno, "ar-BH"
* Árabe (Egipto), "ar-EG"
* Árabe (Irak), "ar-IQ"
* Árabe (Israel), "ar-IL"
* Árabe (Jordania), "ar-JO"
* Árabe (Kuwait), "ar-KW"
* Árabe (Líbano), "ar-LB"
* Árabe (Omán), "ar-OM"
* Árabe (Qatar), "ar-QA"
* Árabe (Arabia Saudí), "ar-SA"
* Danés, "da-DK"
* Noruego, "nb-NO"
* Sueco, "sv-SE"
* Finés, "fi-FI"
* Tailandés, "th-TH"
* Turco, "tr-TR"

Los últimos idiomas disponibles se puede ver en el [artículo Análisis de archivos de audio y vídeo con Azure Media Services.](analyze-video-audio-files-concept.md)

## <a name="february-2021"></a>Febrero de 2021

### <a name="hevc-encoding-support-in-standard-encoder"></a>Compatibilidad con la codificación HEVC en el codificador estándar

El codificador estándar ahora admite la codificación HEVC (H.265) de 8 bits. El contenido de HEVC se puede entregar y empaquetar a través del empaquetador dinámico con el formato "hev1".  

Hay disponible una nueva codificación .NET personalizada con un ejemplo de HEVC en el [repositorio de Git Hub media-services-v3-dotnet](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/VideoEncoding/Encoding_HEVC).
Además de la codificación personalizada, ahora están disponibles los siguientes valores preestablecidos de la codificación HEVC integrados:

- H265ContentAwareEncoding
- H265AdaptiveStreaming
- H265SingleBitrate720P
- H265SingleBitrate1080p
- H265SingleBitrate4K

Los clientes que anteriormente utilizaban HEVC en el codificador Premium de la API v2 deberían migrar para usar la nueva compatibilidad con la codificación HEVC en el codificador estándar.

### <a name="azure-media-services-v2-api-and-sdks-deprecation-announcement"></a>Anuncio de desuso de los SDK y Azure Media Services v2 API

#### <a name="update-your-azure-media-services-rest-api-and-sdks-to-v3-by-29-february-2024"></a>Actualice los SDK y la API REST de Azure Media Services a la versión 3 antes del 29 de febrero de 2024

Dado que la versión 3 de la API REST de Azure Media Services y los SDK de cliente para .NET y Java ofrece más funcionalidades que la versión 2, se va a retirar la versión 2 de la API REST de Azure Media Services y los SDK de cliente para .NET y Java.

Es aconsejable que realice el cambio lo antes posible para aprovechar las ventajas de la versión 3 de la API REST de Azure Media Services y los SDK de cliente para .NET y Java.
La versión 3 proporciona:
 
- Compatibilidad ininterrumpida con eventos en directo
- API de REST de ARM y SDK de cliente para .NET Core, Node.js, Python, Java, Go y Ruby.
- Claves administradas por el cliente, integración de almacenamiento de confianza, compatibilidad con vínculos privados, [ etc.](./migrate-v-2-v-3-migration-benefits.md)

Como parte de la actualización a la API y los SDK de la versión 3, las unidades reservadas de multimedia (MRU) ya no son necesarias en las cuentas de Media Services, ya que el sistema se escalará y reducirá verticalmente de forma automática en función de la carga. Consulte la [guía de migración de MRU](./migrate-v-2-v-3-migration-scenario-based-media-reserved-units.md) para más información.

#### <a name="action-required"></a>Acción requerida

Para minimizar la interrupción de las cargas de trabajo, consulte la [guía de migración](./migrate-v-2-v-3-migration-introduction.md) para pasar del código de la API y los SDK de la versión 2 a la API y el SDK de la versión 3 antes del 29 de febrero de 2024.
**A partir del 29 de febrero de 2024**, Azure Media Services dejará de aceptar el tráfico en la versión 2 de la API REST, la versión 2015-10-01 de la API de administración de cuentas de ARM o de los SDK de cliente de .NET de la versión 2. Esto incluye los SDK de cliente de código abierto de terceros que puedan llamar a la versión 2 de la API.  

Vea el anuncio oficial de [las actualizaciones de Azure](https://azure.microsoft.com/updates/update-your-azure-media-services-rest-api-and-sdks-to-v3-by-29-february-2024/).

### <a name="standard-encoder-support-for-v2-api-features"></a>Compatibilidad del codificador estándar con las características de la versión 2 de la API

Además de la nueva compatibilidad agregada con la codificación HEVC (H.265), las siguientes características están disponibles en la versión 2020-05-01 de la API de codificación.

- Se admite la unión de varios archivos de entrada mediante la nueva compatibilidad con **JobInputClip**.
    - Hay un ejemplo disponible para .NET que muestra cómo [unir dos recursos](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/VideoEncoding/Encoding_StitchTwoAssets).
- La selección de pistas de audio permite a los clientes seleccionar y asignar las pistas de audio entrante y enrutarlas a la salida para la codificación
    - Consulte [OpenAPI API REST para más información](https://github.com/Azure/azure-rest-api-specs/blob/8d15dc681b081cca983e4d67fbf6441841d94ce4/specification/mediaservices/resource-manager/Microsoft.Media/stable/2020-05-01/Encoding.json#L385) sobre **AudioTrackDescriptor** y la selección de pistas
- Selección de pista para codificación: permite a los clientes elegir pistas en un archivo de origen ABR o un archivo activo que tenga pistas de varias velocidades de bits. Es muy útil para generar archivos MP4 a partir de los archivos de almacenamiento de eventos en directo.
    - Consulte [VideoTrackDescriptor](https://github.com/Azure/azure-rest-api-specs/blob/8d15dc681b081cca983e4d67fbf6441841d94ce4/specification/mediaservices/resource-manager/Microsoft.Media/stable/2020-05-01/Encoding.json#L1562)
- Se han agregado funcionalidades de difuminado (desenfoque) a FaceDetector
    - Vea los modos de [difuminar](https://github.com/Azure/azure-rest-api-specs/blob/8d15dc681b081cca983e4d67fbf6441841d94ce4/specification/mediaservices/resource-manager/Microsoft.Media/stable/2020-05-01/Encoding.json#L634) y [combinado](https://github.com/Azure/azure-rest-api-specs/blob/8d15dc681b081cca983e4d67fbf6441841d94ce4/specification/mediaservices/resource-manager/Microsoft.Media/stable/2020-05-01/Encoding.json#L649) del valor preestablecido de FaceDetector

### <a name="new-client-sdk-releases-for-2020-05-01-version-of-the-azure-media-services-api"></a>Nuevas versiones del SDK de cliente para la versión 2020-05-01 de API Azure Media Services

Las nuevas versiones del SDK de cliente para todos los idiomas disponibles ahora están disponibles con las características anteriores.
Actualice a los SDK de cliente más recientes en sus bases de código mediante el administrador de paquetes.

- [Paquete del SDK de .NET 3.0.4](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/)
- [Node.js Typescript, versión 8.1.0](https://www.npmjs.com/package/@azure/arm-mediaservices)
- [Python azure-mgmt-search 3.1.0](https://pypi.org/project/azure-mgmt-media/)
- [SDK de Java 1.0.0-beta.2](https://search.maven.org/artifact/com.azure.resourcemanager/azure-resourcemanager-mediaservices/1.0.0-beta.2/jar)

### <a name="new-security-features-available-in-the-2020-05-01-version-of-the-azure-media-services-api"></a>Nuevas características de seguridad disponibles en la versión 2020-05-01 de API Azure Media Services

- **[Claves administradas por el cliente](concept-use-customer-managed-keys-byok.md)** : las claves de contenido y otros datos almacenados en cuentas creadas con la versión "2020-05-01" de la API se cifran con una clave de cuenta. Los clientes pueden especificar una clave para cifrar la clave de cuenta.

- **[Almacenamiento de confianza](concept-trusted-storage.md)** : Media Services se puede configurar para acceder a Azure Storage mediante una instancia de Managed Identity asociada a la cuenta de Media Services. Cuando se accede a las cuentas de almacenamiento mediante una instancia de Managed Identity, los clientes pueden configurar listas de control de acceso de red más restrictivas en la cuenta de almacenamiento sin bloquear los escenarios de Media Services.

- **[Identidades administradas](concept-managed-identities.md)** : los clientes pueden habilitar una identidad administrada asignada por el sistema para una cuenta de Media Services para proporcionar acceso a los almacenes de claves (para las claves administradas por el cliente) y a las cuentas de almacenamiento (para el almacenamiento de confianza).

### <a name="updated-typescript-nodejs-samples-using-isomorphic-sdk-for-javascript"></a>Ejemplos de Typescript Node.js actualizados que usan SDK isomórfico para JavaScript

Los ejemplos de Node.js se han actualizado para usar el SDK isomórfico más reciente. Los ejemplos ahora muestran el uso de Typescript. Además, se ha agregado un nuevo ejemplo de streaming en vivo para Node.js/Typescript.

Vea los ejemplos más recientes en el repositorio de GitHub **[media-services-v3-node-tutorials](https://github.com/Azure-Samples/media-services-v3-node-tutorials)** .

### <a name="new-live-stand-by-mode-to-support-faster-startup-from-warm-state"></a>Nuevo modo de espera en directo para admitir un inicio más rápido desde un estado activo

Los eventos en directo ahora admiten un modo de facturación de menor costo para el estado "en espera", lo que permite a los clientes asignar previamente eventos en directo a un menor costo para la creación de "grupos de nivel de almacenamiento de acceso frecuente". Los clientes pueden usar los eventos en directo en espera para pasar al estado En ejecución más rápidamente que si empezaran a desde cero en la creación.  Esto reduce considerablemente el tiempo que tarda en iniciarse el canal y permite una rápida asignación rápida de grupos de nivel de almacenamiento de acceso frecuente que se ejecutan en un modo cuyo precio es inferior.
Vea [aquí](https://azure.microsoft.com/pricing/details/media-services) los detalles más recientes sobre los precios.
Para más información sobre el estado En espera y los otros estados de Eventos en directo, consulte el artículo sobre [Estados y facturación de eventos en directo.](./live-event-states-billing-concept.md)

## <a name="december-2020"></a>Diciembre de 2020

### <a name="regional-availability"></a>Disponibilidad regional

Azure Media Services ahora está disponible en la región este de Noruega en Azure Portal.  No hay ninguna restV2 en esta región.

## <a name="october-2020"></a>Octubre de 2020

### <a name="basic-audio-analysis"></a>Análisis de audio básico

El valor preestablecido del análisis de audio ahora incluye un plan de tarifa de modo básico. El nuevo modo básico del analizador de audio ofrece una opción de bajo costo para extraer transcripciones de voz y dar formato a los subtítulos y CC resultantes. Este modo realiza la transcripción de voz a texto y la generación de un archivo de subtítulos VTT. La salida de este modo incluye un archivo JSON de información, que incluye solo las palabras clave, la transcripción y la información de tiempo. La detección automática de idioma y la diarización de los altavoces no se incluyen en este modo. Consulte la lista de [idiomas admitidos](analyze-video-audio-files-concept.md#built-in-presets).

Los clientes que usan el indexador v1 y el indexador v2 deben migrar al valor preestablecido de análisis de audio básico.

Para obtener más información acerca del modo básico del analizador de audio, consulte [Análisis de archivos de audio y vídeo](analyze-video-audio-files-concept.md).  Para aprender a usar el modo básico del analizador de audio con la API REST, consulte [Creación de una transformación de audio básica](transform-create-basic-audio-how-to.md).

### <a name="live-events"></a>Eventos en vivo

Ahora puede actualizar la mayoría de las propiedades cuando se detienen los eventos en directo. Además, los usuarios pueden especificar un prefijo para el nombre de host estático de las direcciones URL de entrada y versión preliminar del evento activo. VanityUrl ahora se llama `useStaticHostName`, para reflejar mejor la intención de la propiedad.

Los eventos en directo ahora tienen un estado StandBy.  Consulte [Eventos en directo y salidas activas en Media Services](./live-event-outputs-concept.md).

Un evento en directo permite recibir varias relaciones de aspecto de entrada. El modo de ajuste permite a los clientes especificar el comportamiento de ajuste de la salida.

La codificación en directo ahora agrega la capacidad de generar fragmentos de intervalo con fotogramas clave fijos de entre 0,5 y 20 segundos.

### <a name="accounts"></a>Cuentas

> [!WARNING]
> Si crea una cuenta de Media Services con la versión de API 2020-05-01, no funcionará con RESTv2. 

## <a name="august-2020"></a>Agosto de 2020

### <a name="dynamic-encryption"></a>Cifrado dinámico

La compatibilidad con el cifrado Protected Interoperable File Format (PIFF 1.1) de PlayReady heredado ya está disponible en el empaquetador dinámico. Proporciona compatibilidad con los televisores inteligentes heredados de Samsung y LG que implementaron los borradores iniciales del estándar Common Encryption (CENC) publicado por Microsoft.  El formato PIFF 1.1 también se conoce como el formato de cifrado admitido anteriormente por la biblioteca cliente de Silverlight. En la actualidad, el único caso de uso para este formato de cifrado es la segmentación del mercado de los televisores inteligentes heredados, donde todavía existen una cantidad considerable de televisores inteligentes en algunas regiones que solo admiten Smooth Streaming con el cifrado PIFF 1.1.

Para usar la compatibilidad con el nuevo cifrado PIFF 1.1, cambie el valor de cifrado a "piff" en la ruta de acceso de la dirección URL del localizador de streaming. Para obtener más información, consulte [Introducción a Content Protection](drm-content-protection-concept.md).
Por ejemplo: `https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=piff)`|

> [!NOTE]
> La compatibilidad con PIFF 1.1 se proporciona como una solución compatible con versiones anteriores de televisores inteligentes (Samsung y LG) que implementó la versión "Silverlight" anterior de Common Encryption. Se recomienda usar solo el formato PIFF cuando sea necesario para la compatibilidad con los televisores inteligentes Samsung o LG heredados vendidos entre 2009-2015 compatibles con la versión PIFF 1.1 del cifrado de PlayReady. 

## <a name="july-2020"></a>Julio de 2020

### <a name="live-transcriptions"></a>Transcripciones en vivo

Las transcripciones en vivo ahora admiten 19 idiomas y 8 regiones.

### <a name="protecting-your-content-with-media-services-and-azure-ad"></a>Protección del contenido con Media Services y Azure AD

Publicamos un tutorial denominado [Protección de contenido de un extremo a otro con Azure AD](./architecture-azure-ad-content-protection.md).

### <a name="high-availability"></a>Alta disponibilidad

Publicamos una [introducción](./architecture-high-availability-encoding-concept.md) y un [ejemplo](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/HighAvailabilityEncodingStreaming) de alta disponibilidad con Media Services y Vídeo bajo demanda (VoD).

## <a name="june-2020"></a>Junio de 2020

### <a name="live-video-analytics-on-iot-edge-preview-release"></a>Live Video Analytics on IoT Edge (versión preliminar)

Ya se publicó la versión preliminar de Live Video Analytics on IoT Edge. Para más información, consulte las [notas de la versión](../live-video-analytics-edge/release-notes.md).

Live Video Analytics on IoT Edge es una expansión de la familia de Media Services. Le permite analizar vídeos en directo con los modelos de inteligencia artificial que prefiera en sus propios dispositivos perimetrales y, opcionalmente, capturar y grabar ese vídeo. Ahora puede crear aplicaciones con análisis de vídeo en tiempo real en el perímetro sin preocuparse por la complejidad de crear y operar una canalización de vídeo en directo.

## <a name="may-2020"></a>Mayo de 2020

Azure Media Services ya está disponible con carácter general en las regiones siguientes: "Norte de Alemania", "Centro-oeste de Alemania", "Norte de Suiza" y "Oeste de Suiza". Los clientes pueden implementar Media Services en estas regiones mediante Azure Portal.

## <a name="april-2020"></a>Abril de 2020

### <a name="improvements-in-documentation"></a>Mejoras en la documentación

La documentación de Azure Media Player se migró a la [documentación de Azure](../azure-media-player/azure-media-player-overview.md).

## <a name="january-2020"></a>Enero de 2020

### <a name="improvements-in-media-processors"></a>Mejoras en los procesadores de multimedia

- Compatibilidad mejorada con orígenes entrelazados en el análisis de vídeo: se ha eliminado correctamente el entrelazado de esos contenidos antes de enviarse a los motores de inferencia.
- Ahora, al generar miniaturas con el modo "Mejor", el codificador busca durante más de 30 segundos para seleccionar un fotograma que no sea monocromático.

### <a name="azure-government-cloud-updates"></a>Actualizaciones de la nube de Azure Government

Media Services está disponible con carácter temporal en las siguientes regiones de Azure Government: *USGov Arizona* y *USGov Texas*.

## <a name="december-2019"></a>Diciembre de 2019

Se agregó compatibilidad de la red CDN con los encabezados *Origin-Assist Prefetch* para streaming a petición tanto en directo como en vídeo; está disponible para clientes que tienen un contrato directo con CDN de Akamai. La característica Origin-Assist CDN-Prefetch supone los siguientes intercambios de encabezados HTTP entre CDN de Akamai y el origen de Azure Media Services:

|Encabezado HTTP|Valores|Remitente|Receptor|Propósito|
| ---- | ---- | ---- | ---- | ----- |
|CDN-Origin-Assist-Prefetch-Enabled | 1 (valor predeterminado) o 0 |CDN|Origen|Para indicar que la red CDN está habilitada para la captura previa.|
|CDN-Origin-Assist-Prefetch-Path| Ejemplo: <br/>Fragments(video=1400000000,format=mpd-time-cmaf)|Origen|CDN|Para proporcionar la ruta de acceso de captura previa a la red CDN.|
|CDN-Origin-Assist-Prefetch-Request|1 (solicitud de captura previa) o 0 (solicitud normal)|CDN|Origen|Para indicar que la solicitud de CDN es una captura previa.|

Para ver en acción parte del intercambio de encabezados, puede probar los pasos siguientes:

1. Use Postman o Curl para emitir una solicitud de un segmento o fragmento de audio o vídeo al origen de Media Services. Asegúrese de agregar el encabezado CDN-Origin-Assist-Prefetch-Enabled: 1 en la solicitud.
2. En la respuesta, debería ver el encabezado CDN-Origin-Assist-Prefetch-Path con una ruta de acceso relativa como su valor.

## <a name="november-2019"></a>Noviembre de 2019

### <a name="live-transcription-preview"></a>Transcripción en directo (versión preliminar)

La transcripción en directo está ahora en versión preliminar pública y disponible para su uso en la región Oeste de EE. UU. 2.

Está diseñada para funcionar en combinación con eventos en directo como una funcionalidad complementaria.  Es compatible con eventos en directo de codificación estándar y premium de paso a través.  Cuando esta característica está habilitada, el servicio usa la característica [Voz a texto](../../cognitive-services/speech-service/speech-to-text.md) de Cognitive Services para transcribir el texto oral del audio entrante en texto escrito. A continuación, se pone a disposición este texto para su entrega junto con el vídeo y el audio en los protocolos MPEG-DASH y HLS. La facturación se basa en un nuevo medidor complementario que supone un costo adicional para el evento en directo cuando está en estado "en ejecución".  Para más información sobre la transcripción en directo y la facturación, consulte [Transcripción en directo](live-event-live-transcription-how-to.md).

> [!NOTE]
> Actualmente, la transcripción en directo solo está disponible como una característica en vista previa en la región Oeste de EE. UU. 2. En este momento, solo admite la transcripción de texto oral en inglés (en-US).

### <a name="content-protection"></a>Protección de contenido

La característica de *prevención de reproducción de tokens* publicada en septiembre en algunas regiones está ahora disponible en todas las regiones.
Los clientes de Media Services ahora pueden establecer un límite en el número de veces que se puede usar el mismo token para solicitar una clave o una licencia. Para obtener más información, consulte [Prevención de reproducción de tokens](drm-content-protection-concept.md#token-replay-prevention).

### <a name="new-recommended-live-encoder-partners"></a>Nuevos asociados de codificador en directo recomendados

Se ha agregado compatibilidad con los siguientes nuevos asociados de codificador recomendados para el streaming en vivo RTMP:

- [Cambria Live 4.3](https://www.capellasystems.net/products/cambria-live/)
- [Cámaras de acción GoPro Hero7/8 y Max](https://gopro.com/help/articles/block/getting-started-with-live-streaming)
- [Restream.io](https://restream.io/)

### <a name="file-encoding-enhancements"></a>Mejoras en la codificación de archivos

- Ahora hay disponible un nuevo valor preestablecido de codificación compatible con el contenido. Genera un conjunto de archivos MP4 con alineación GOP mediante la codificación compatible con el contenido. Dado cualquier contenido de entrada, el servicio realiza un análisis ligero inicial del contenido de entrada. Utiliza esos resultados para determinar automáticamente el número óptimo de capas, la velocidad de bits adecuada y la configuración de resolución para la entrega a través del streaming adaptable. Este valor predefinido resulta particularmente eficaz en los vídeos de complejidad media y baja, donde los archivos de salida tendrán velocidades de bits más lentas, pero una calidad que seguirá ofreciendo una buena experiencia a los espectadores. La salida contendrá archivos MP4 con el vídeo y audio intercalados. Para obtener más información, consulte las [especificaciones de API abiertas](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json).
- Rendimiento mejorado y subprocesos múltiples en el cambio de tamaño de Standard Encoder. En condiciones específicas, el cliente apreciará un aumento del rendimiento de la codificación VOD entre un 5 % y un 40 %. El contenido de poca complejidad codificado en varias velocidades de bits experimentará los mayores aumentos del rendimiento. 
- La codificación estándar ahora mantiene una cadencia normal del GOP para el contenido de velocidad de fotogramas variable (VFR) durante la codificación VOD cuando se usa el valor GOP basado en el tiempo.  Esto significa que, por ejemplo, el cliente que envía contenido de velocidad de fotogramas mixta que varía entre 15 y 30 fps observará ahora distancias de GOP normales calculadas en la salida a archivos MP4 de streaming con velocidad de bits adaptable. Este hecho mejorará la posibilidad de cambiar sin problemas entre pistas al realizar las entregas a través de HLS o DASH. 
-  Sincronización de AV mejorada en el contenido de origen de velocidad de fotogramas variable (VFR)

### <a name="azure-video-analyzer-for-media-video-analytics"></a>Azure Video Analyzer for Media: análisis de vídeo

- Los fotogramas clave extraídos mediante el valor preestablecido de VideoAnalyzer ahora se encuentran en la resolución original del vídeo en lugar de cambiarse de tamaño. La extracción de fotogramas clave de alta resolución proporciona imágenes de calidad original y permite usar los modelos de inteligencia artificial basados en imágenes proporcionados por los servicios Microsoft Computer Vision y Custom Vision para obtener aún más información del vídeo.

## <a name="september-2019"></a>Septiembre de 2019

###  <a name="media-services-v3"></a>Media Services v3  

#### <a name="live-linear-encoding-of-live-events"></a>Codificación lineal de eventos en directo

Media Services V3 anuncia la versión preliminar de la codificación lineal de eventos en directo durante 24 horas al día los 365 días del año.

###  <a name="media-services-v2"></a>Media Services v2  

#### <a name="deprecation-of-media-processors"></a>Desuso de los procesadores de multimedia

Estamos anunciando el desuso de *Azure Media Indexer* y *Azure Media Indexer 2 Preview*. Para ver las fechas de retirada, consulte el artículo sobre [componentes heredados](../previous/legacy-components.md). Azure Video Analyzer for Media reemplaza a estos procesadores de multimedia heredados.

Para más información, consulte [Migración de Azure Media Indexer y Azure Media Indexer 2 a **Video Indexer de Azure Media Services**](../previous/migrate-indexer-v1-v2.md).

## <a name="august-2019"></a>Agosto de 2019

###  <a name="media-services-v3"></a>Media Services v3  

#### <a name="south-africa-regional-pair-is-open-for-media-services"></a>El par regional de Sudáfrica está abierto para Media Services 

Media Services ya está disponible en las regiones Norte de Sudáfrica y Oeste de Sudáfrica.

Si desea obtener más información, vea [Nubes y regiones donde existe Azure Media Services v3](azure-clouds-regions.md).

###  <a name="media-services-v2"></a>Media Services v2  

#### <a name="deprecation-of-media-processors"></a>Desuso de los procesadores de multimedia

Anunciamos el desuso de los procesadores de multimedia *Windows Azure Media Encoder* (WAME) y *Azure Media Encoder* (AME), que se van a retirar. Para ver las fechas de retirada, consulte este artículo sobre [componentes heredados](../previous/legacy-components.md).

Para más información, consulte [Migración de WAME a Media Encoder Standard](../previous/migrate-windows-azure-media-encoder.md) y [Migración de AME a Media Encoder Standard](../previous/migrate-azure-media-encoder.md).
 
## <a name="july-2019"></a>Julio de 2019

### <a name="content-protection"></a>Protección de contenido

Cuando el contenido de streaming está protegido con restricción de token, los usuarios finales deben obtener un token que se envía como parte de la solicitud de entrega de claves. La característica de *prevención de reproducción de tokens* permite a los clientes de Media Services establecer un límite en el número de veces que se puede usar el mismo token para solicitar una clave o una licencia. Para obtener más información, consulte [Prevención de reproducción de tokens](drm-content-protection-concept.md#token-replay-prevention).

Desde julio, la característica de vista previa solo estaba disponible en las regiones Centro de EE. UU. y Centro y oeste de EE. UU.

## <a name="june-2019"></a>Junio de 2019

### <a name="video-subclipping"></a>Creación de subclips de vídeo

Ahora puede recortar un vídeo, o crear un subclip de vídeo al codificarlo mediante la opción [Trabajo](/rest/api/media/jobs). 

Esta funcionalidad se puede usar con cualquier elemento [Transformación](/rest/api/media/transforms) compilado mediante los valores preestablecidos [BuiltInStandardEncoderPreset](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) o [StandardEncoderPreset](/rest/api/media/transforms/createorupdate#standardencoderpreset). 

Ver ejemplos:

* [Creación de un subclip de vídeo con .NET](transform-subclip-video-dotnet-how-to.md)
* [Creación de un subclip de vídeo con REST](transform-subclip-video-rest-how-to.md)

## <a name="may-2019"></a>Mayo de 2019

### <a name="azure-monitor-support-for-media-services-diagnostic-logs-and-metrics"></a>Soporte técnico de Azure Monitor para las métricas y los registros de diagnóstico de Media Services

Ya se puede usar Azure Monitor para ver los datos de telemetría emitidos por Media Services.

* Use los registros de diagnóstico de Azure Monitor para supervisar las solicitudes enviadas por el punto de conexión de la entrega de claves de Media Services. 
* Supervise las métricas emitidas por los [punto de conexión de streaming](stream-streaming-endpoint-concept.md) de Media Services.   

Para obtener más información, consulte [Supervisión de los registros de diagnóstico y las métricas de Media Services](monitoring/monitor-media-services-data-reference.md).

### <a name="multi-audio-tracks-support-in-dynamic-packaging"></a>Compatibilidad con varias pistas de audio en el empaquetado dinámico 

Al realizar el streaming de recursos que tienen varias pistas de audio con varios códecs y lenguajes, el [empaquetado dinámico](encode-dynamic-packaging-concept.md) ahora admite varias pistas de audio para la salida HLS (versión 4 o superior).

### <a name="korea-regional-pair-is-open-for-media-services"></a>El par regional de Corea está abierto para Media Services 

Media Services ya está disponible en las regiones Centro de Corea del Sur y Sur de Corea del Sur. 

Si desea obtener más información, vea [Nubes y regiones donde existe Azure Media Services v3](azure-clouds-regions.md).

### <a name="performance-improvements"></a>Mejoras en el rendimiento

Se han agregado actualizaciones que incluyen mejoras de rendimiento de Media Services.

* Se actualizó el tamaño de archivo máximo admitido para el procesamiento. Consulte [Cuotas y límites](limits-quotas-constraints-reference.md).
* [Mejoras de velocidades de codificación](concept-media-reserved-units.md).

## <a name="april-2019"></a>Abril de 2019

### <a name="new-presets"></a>Nuevos valores preestablecidos

* [FaceDetectorPreset](/rest/api/media/transforms/createorupdate#facedetectorpreset) se agregó a los valores preestablecidos del analizador integrado.
* [ContentAwareEncodingExperimental](/rest/api/media/transforms/createorupdate#encodernamedpreset) se agregó a los valores preestablecidos del codificador integrado. Para más información, consulte [Codificación que tiene en cuenta el contenido](encode-content-aware-concept.md). 

## <a name="march-2019"></a>Marzo de 2019

Ahora el empaquetado dinámico admite Dolby Atmos. Para más información, consulte [Códecs de audio compatibles con el empaquetado dinámico](encode-dynamic-packaging-concept.md#audio-codecs-supported-by-dynamic-packaging).

Ahora puede especificar una lista de filtros de recursos o cuentas, que se aplicarían a su localizador de streaming. Para más información, consulte el tema sobre la [asociación de filtros al localizador de streaming](filters-concept.md#associating-filters-with-streaming-locator).

## <a name="february-2019"></a>Febrero de 2019

Media Services v3 ya se admite en las nubes nacionales de Azure. Aún no todas las características están disponibles en todas las nubes. Para obtener más detalles, consulte [Nubes y regiones donde existe Azure Media Services v3](azure-clouds-regions.md).

El evento [Microsoft.Media.JobOutputProgress](monitoring/media-services-event-schemas.md#monitoring-job-output-progress) se ha agregado a los esquemas de Azure Event Grid para Media Services.

## <a name="january-2019"></a>Enero de 2019

### <a name="media-encoder-standard-and-mpi-files"></a>Media Encoder Standard y archivos MPI 

Al codificar con Media Encoder Standard para generar archivos MP4, se genera un archivo .mpi nuevo y se agrega a la salida de activos. Este archivo MPI está diseñado para mejorar el rendimiento de escenarios de streaming y [empaquetado dinámico](encode-dynamic-packaging-concept.md).

No debe modificar ni quitar el archivo MPI, así como tampoco tener ninguna dependencia en el servicio en la existencia (o no) de este tipo de archivo.

## <a name="december-2018"></a>Diciembre de 2018

Las actualizaciones de la versión de disponibilidad general de la API de V3 incluyen:
       
* Las propiedades de **PresentationTimeRange** ya no son necesarias para los **filtros de recursos** ni para los **filtros de cuenta**. 
* Las opciones de la consultas $top y $skip para **Jobs** y **Transforms** se han quitado y se ha agregado $orderby. Como parte de agregar la nueva funcionalidad de ordenación, se ha detectado que las opciones $top y $skip se expusieron previamente de forma accidental aunque no se habían implementado.
* La extensibilidad de enumeración se había activado de nuevo. Esta característica estaba habilitada en las versiones preliminares del SDK y se deshabilitó accidentalmente en la versión de disponibilidad general.
* Se ha cambiado el nombre de dos directivas de streaming predefinidas. **SecureStreaming** es ahora **MultiDrmCencStreaming**. **SecureStreamingWithFairPlay** es ahora **Predefined_MultiDrmStreaming**.

## <a name="november-2018"></a>Noviembre de 2018

El módulo de la CLI 2.0 está ahora disponible para [Azure Media Services v3 con disponibilidad general](/cli/azure/ams) – v 2.0.50.

### <a name="new-commands"></a>Nuevos comandos

- [az ams account](/cli/azure/ams/account)
- [az ams account-filter](/cli/azure/ams/account-filter)
- [az ams asset](/cli/azure/ams/asset)
- [az ams asset-filter](/cli/azure/ams/asset-filter)
- [az ams content-key-policy](/cli/azure/ams/content-key-policy)
- [az ams job](/cli/azure/ams/job)
- [az ams live-event](/cli/azure/ams/live-event)
- [az ams live-output](/cli/azure/ams/live-output)
- [az ams streaming-endpoint](/cli/azure/ams/streaming-endpoint)
- [az ams streaming-locator](/cli/azure/ams/streaming-locator)
- [az ams account mru](/cli/azure/ams/account/mru): permite administrar unidades reservadas de multimedia. Para más información, consulte [Escalado de unidades reservadas de multimedia](media-reserved-units-cli-how-to.md).

### <a name="new-features-and-breaking-changes"></a>Nuevas características y cambios importantes

#### <a name="asset-commands"></a>Comandos de recursos

- Se han agregado los argumentos ```--storage-account``` y ```--container```.
- Se han agregado valores predeterminados para la hora de expiración (ahora +23 h) y los permisos (lectura) al comando ```az ams asset get-sas-url```.

#### <a name="job-commands"></a>Comandos de trabajos

- Se han agregado los argumentos ```--correlation-data``` y ```--label```.
- ```--output-asset-names``` cambia su nombre a ```--output-assets```. Ahora acepta una lista separada por espacios de recursos en formato "assetName=label". Se puede enviar un recurso sin etiqueta del siguiente modo: "assetName=".

#### <a name="streaming-locator-commands"></a>Comandos de localizador de streaming

- Se ha reemplazado el comando base ```az ams streaming locator``` por ```az ams streaming-locator```.
- Se han agregado los argumentos ```--streaming-locator-id``` y ```--alternative-media-id support```.
- Se ha actualizado el argumento ```--content-keys argument```.
- ```--content-policy-name``` cambia su nombre a ```--content-key-policy-name```.

#### <a name="streaming-policy-commands"></a>Comandos de directiva de streaming

- Se ha reemplazado el comando base ```az ams streaming policy``` por ```az ams streaming-policy```.
- Se ha agregado compatibilidad con los parámetros de cifrado en ```az ams streaming-policy create```.

#### <a name="transform-commands"></a>Comandos de transformación

- Se ha reemplazado el argumento ```--preset-names``` por ```--preset```. Ahora solo puede establecer una salida o valor preestablecido cada vez (para agregar más tendrá que ejecutar ```az ams transform output add```). Ademas, puede pasar la ruta de acceso al código JSON personalizado para establecer un StandardEncoderPreset personalizado.
- ```az ams transform output remove``` se puede realizar pasando el índice de salida a eliminar.
- Se han agregado argumentos ```--relative-priority, --on-error, --audio-language and --insights-to-extract``` en los comandos ```az ams transform create``` y ```az ams transform output add```.

## <a name="october-2018---ga"></a>Octubre de 2018: disponibilidad general

En esta sección se describen las actualizaciones de octubre de Azure Media Services (AMS).

### <a name="rest-v3-ga-release"></a>Versión de disponibilidad general de REST v3

La [versión de disponibilidad general de REST v3](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) incluye otras API de Live, filtros de manifiesto de nivel de cuenta y recurso y compatibilidad con DRM.

#### <a name="azure-resource-management"></a>Administración de recursos de Azure 

La compatibilidad con la administración de recursos de Azure habilita la API de operaciones y administración unificada (ahora todo en un solo lugar).

A partir de esta versión, puede usar plantillas de Resource Manager para crear eventos en vivo.

#### <a name="improvement-of-asset-operations"></a>Mejora de las operaciones de recurso 

Se han introducido las siguientes mejoras:

- Ingesta de direcciones URL de HTTP o direcciones URL de SAS de Azure Blob Storage.
- Especifique sus propios nombres de contenedor para los recursos. 
- Compatibilidad con la salida más sencilla para crear flujos de trabajo personalizados con Azure Functions.

#### <a name="new-transform-object"></a>Nuevo objeto Transform

El nuevo objeto **Transform** simplifica el modelo de codificación. El nuevo objeto facilita la creación y el uso compartido de la codificación de plantillas y valores preestablecidos de Resource Manager. 

#### <a name="azure-active-directory-authentication-and-azure-rbac"></a>Autenticación de Azure Active Directory y Azure RBAC

El control de acceso basado en rol de Azure (Azure RBAC) y la autenticación de Azure AD habilitan transformaciones seguras, objetos LiveEvent, directivas de clave de contenido o recursos por rol o usuarios en Azure AD.

#### <a name="client-sdks"></a>SDK de cliente  

Idiomas admitidos en Media Services v3: .NET Core, Java, Node.js, Ruby, Typescript, Python y Go.

#### <a name="live-encoding-updates"></a>Actualizaciones de Live Encoding

Se presentan las siguientes actualizaciones de Live Encoding:

- Nuevo modo en vivo de baja latencia (10 segundos de principio a fin).
- Compatibilidad mejorada con RTMP (mayor estabilidad y mejor compatibilidad con codificadores de origen).
- Ingesta segura de RTMPS.

    Cuando se crea un evento en directo, ahora obtiene 4 direcciones URL de ingesta. Las cuatro direcciones URL de ingesta son casi idénticas, tienen el mismo token de streaming (AppId) y solo se diferencian en componente de número de puerto. Dos de las direcciones URL son principal y de respaldo para RTMPS. 
- Soporte técnico de transcodificación las 24 horas. 
- Mejor compatibilidad con señalización de anuncios en RTMP a través de SCTE35.

#### <a name="improved-event-grid-support"></a>Soporte técnico mejorado de Event Grid

Puede ver las siguientes mejoras de soporte técnico de Event Grid:

- Integración de Azure Event Grid para facilitar el desarrollo con Logic Apps y Azure Functions. 
- Suscríbase a eventos sobre codificación, canales en vivo y mucho más.

### <a name="cmaf-support"></a>Compatibilidad con CMAF

Compatibilidad con el cifrado de CMAF y “cbcs” para reproductores Apple HLS (iOS 11+) y MPEG-DASH que admiten CMAF.

### <a name="video-indexer"></a>Video Indexer

La versión de disponibilidad general Video Indexer se anunció en agosto. Para información reciente sobre las características admitidas actualmente, vea [Novedades de Video Indexer](../../azure-video-analyzer/video-analyzer-for-media-docs/video-indexer-overview.md?bc=%2fazure%2fmedia-services%2fvideo-indexer%2fbreadcrumb%2ftoc.json&toc=%2fazure%2fmedia-services%2fvideo-indexer%2ftoc.json). 

### <a name="plans-for-changes"></a>Planes de cambios

#### <a name="azure-cli-20"></a>CLI de Azure 2.0
 
Próximamente estará disponible el módulo de la CLI de Azure 2.0 que incluye operaciones en todas las características (como Live, directivas de clave de contenido, filtros de cuenta/recurso, directivas de streaming). 

### <a name="known-issues"></a>Problemas conocidos

Solo los clientes que usan la API de versión preliminar con filtros de cuenta o recurso se ven afectados por el problema siguiente.

Si creó filtros de cuenta o recurso entre el 28/09 y el 12/10 con las API o la CLI de Media Services v3, debe quitar todos los filtros de cuenta y recurso y volver a crearlos debido a un conflicto de versiones. 

## <a name="may-2018---preview"></a>Mayo de 2018: versión preliminar

### <a name="net-sdk"></a>.NET SDK

Las características siguientes están disponibles en el SDK de .NET:

* **Transformaciones** y **trabajos** para codificar o analizar el contenido multimedia. Para ver ejemplos, consulte los artículos sobre [streaming de archivos](stream-files-tutorial-with-api.md) y [análisis](analyze-videos-tutorial.md).
* **Localizadores de streaming** para publicar y transmitir contenido a los dispositivos de usuarios finales.
* **Directivas de streaming** y **directivas de claves de contenido** para configurar la entrega de claves y la protección de contenido (DRM) al entregar el contenido.
* **Eventos en directo** y **salidas de eventos** para configurar la ingesta y el archivo de contenido de streaming en vivo.
* **Recursos** para almacenar y publicado contenido multimedia en Azure Storage. 
* **Puntos de conexión de streaming** para configurar y escalar el empaquetado dinámico, cifrado y streaming tanto de contenido multimedia en vivo como a petición.

### <a name="known-issues"></a>Problemas conocidos

* Al enviar un trabajo, puede especificar que se ingiera el vídeo de origen mediante direcciones URL HTTPS, URL SAS o rutas de acceso a archivos ubicados en Azure Blob Storage. Actualmente, Media Services v3 no admite la codificación de transferencia fragmentada a través de direcciones URL HTTPS.

## <a name="ask-questions-give-feedback-get-updates"></a>Formule preguntas, realice comentarios y obtenga actualizaciones

Consulte el artículo [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="see-also"></a>Consulte también

[Guía de migración para migrar de Media Services v2 a v3](migrate-v-2-v-3-migration-introduction.md).

## <a name="next-steps"></a>Pasos siguientes

- [Información general](media-services-overview.md)
- [Notas de la versión de Media Services v2](../previous/media-services-release-notes.md)
