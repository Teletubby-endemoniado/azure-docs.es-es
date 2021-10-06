---
title: Cómo configurar la caché integrada de Azure Cosmos DB
description: Aprenda a configurar la caché integrada de Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/20/2021
ms.author: tisande
ms.openlocfilehash: dea28832bb607da722653c398cc234d54d2ffcc9
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128618635"
---
# <a name="how-to-configure-the-azure-cosmos-db-integrated-cache-preview"></a>Cómo configurar la caché integrada de Azure Cosmos DB (versión preliminar)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

En este artículo se describe cómo aprovisionar una puerta de enlace dedicada, cómo configurar la caché integrada y cómo conectar la aplicación. 

## <a name="prerequisites"></a>Requisitos previos

- Si no tiene una [suscripción a Azure](../guides/developer/azure-developer-guide.md#understanding-accounts-subscriptions-and-billing), cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- Una aplicación existente que usa Azure Cosmos DB. Si no tiene una, [aquí tiene algunos ejemplos](https://github.com/AzureCosmosDB/labs).
- Una [cuenta existente de API de Azure Cosmos DB SQL (núcleo)](create-cosmosdb-resources-portal.md).

## <a name="provision-a-dedicated-gateway-cluster"></a>Aprovisionamiento de un clúster de puerta de enlace dedicada

1. Vaya a una cuenta de Azure Cosmos DB en Azure Portal y seleccione la pestaña **Puerta de enlace dedicada**.

   :::image type="content" source="./media/how-to-configure-integrated-cache/dedicated-gateway-tab.png" alt-text="Imagen que muestra cómo ir a la pestaña de la puerta de enlace dedicada" lightbox="./media/how-to-configure-integrated-cache/dedicated-gateway-tab.png" border="false":::

2. Rellene el formulario **Puerta de enlace dedicada** con los detalles siguientes:

   * **Puerta de enlace dedicada:** establezca el botón de alternancia en **Aprovisionada**. 
   * **SKU:** seleccione una SKU con el tamaño de proceso y memoria necesarios. 
   *  **Número de instancias:** número de nodos. Con fines de desarrollo, se recomienda empezar con un nodo del tamaño D4. En función de la cantidad de datos que necesite almacenar en la caché, puede aumentar el tamaño del nodo después realizar las pruebas iniciales.

   :::image type="content" source="./media/how-to-configure-integrated-cache/dedicated-gateway-input.png" alt-text="Imagen que muestra la configuración de entrada de ejemplo para crear un clúster de puerta de enlace dedicado" lightbox="./media/how-to-configure-integrated-cache/dedicated-gateway-input.png" border="false":::

3. Seleccione **Guardar** y espere entre 5 y 10 minutos para que se complete el aprovisionamiento de la puerta de enlace dedicada. Cuando haya terminado el aprovisionamiento, verá la siguiente notificación:

   :::image type="content" source="./media/how-to-configure-integrated-cache/dedicated-gateway-notification.png" alt-text="Imagen que muestra cómo comprobar si se ha completado el aprovisionamiento de puerta de enlace dedicada" lightbox="./media/how-to-configure-integrated-cache/dedicated-gateway-notification.png" border="false":::

## <a name="configuring-the-integrated-cache"></a>Configuración de la memoria caché integrada

1. Al crear una puerta de enlace dedicada, se aprovisiona automáticamente una caché integrada. La memoria caché integrada usará aproximadamente el 70 % de la memoria en la puerta de enlace dedicada. El 30 % restante de memoria de la puerta de enlace dedicada se usa para enrutar las solicitudes a las particiones de back-end.

2.  Modifique la cadena de conexión de la aplicación para usar el nuevo punto de conexión de la puerta de enlace dedicada.

      La cadena de conexión de la puerta de enlace dedicada actualizada se encuentra en la hoja **Claves**:
   
      :::image type="content" source="./media/how-to-configure-integrated-cache/dedicated-gateway-connection-string.png" alt-text="Imagen que muestra la cadena de conexión de la puerta de enlace dedicada" lightbox="./media/how-to-configure-integrated-cache/dedicated-gateway-connection-string.png" border="false":::

      Todas las cadenas de conexión de las puertas de enlace dedicadas siguen el mismo patrón. Quite `documents.azure.com` de la cadena de conexión original y reemplácela por `sqlx.cosmos.azure.com`. Una puerta de enlace dedicada siempre tendrá la misma cadena de conexión, incluso si se quita y se vuelve a aprovisionar.

      No es necesario modificar la cadena de conexión en todas las aplicaciones mediante la misma cuenta de Azure Cosmos DB. Por ejemplo, es posible tener una conexión `CosmosClient` mediante el modo de puerta de enlace y el punto de conexión de la puerta de enlace dedicada, mientras que otra usa el modo directo `CosmosClient`. En otras palabras, agregar una puerta de enlace dedicada no afecta a las formas existentes de conectarse a Azure Cosmos DB.

3. Si usa el SDK de .NET o Java, establezca el modo de conexión en el [modo de puerta de enlace](sql-sdk-connection-modes.md#available-connectivity-modes). Este paso no es necesario para los SDK de Python y Node.js, ya que no tienen opciones adicionales de conexión aparte del modo de puerta de enlace.

> [!NOTE]
> Si usa la versión más reciente del SDK de .NET o Java, el modo de conexión predeterminado es el modo directo. Para usar la memoria caché integrada, debe invalidar este valor predeterminado.

Si usa el SDK de Java, también debe establecer manualmente [contentResponseOnWriteEnabled](/java/api/com.azure.cosmos.cosmosclientbuilder.contentresponseonwriteenabled?view=azure-java-stable&preserve-view=true) en `true` dentro de `CosmosClientBuilder`. Si usa cualquier otro SDK, este valor ya tiene `true` como valor predeterminado, por lo que no es necesario realizar ningún cambio.

## <a name="adjust-request-consistency"></a>Ajuste de la coherencia de la solicitud

Debe ajustar la coherencia de la solicitud para que tenga un estado de evento. Si no lo hace, la solicitud siempre omitirá la caché integrada. La manera más fácil de configurar la coherencia de los eventos para todas las operaciones de lectura, es [establecerla en el nivel de cuenta](consistency-levels.md#configure-the-default-consistency-level). También puede configurar la coherencia en el [nivel de solicitud](how-to-manage-consistency.md#override-the-default-consistency-level); esto se recomienda si solo quiere que un subconjunto de las lecturas use la memoria caché integrada.

> [!NOTE]
> Si usa el SDK de Python, **debe** establecer explícitamente el nivel de coherencia de cada solicitud. Tenga en cuenta que la configuración predeterminada del nivel de cuenta no se aplicará automáticamente.

## <a name="adjust-maxintegratedcachestaleness"></a>Ajuste de MaxIntegratedCacheStaleness

Configure `MaxIntegratedCacheStaleness`, que es el tiempo máximo en el que está dispuesto a tolerar datos obsoletos almacenados en caché. Se recomienda establecer `MaxIntegratedCacheStaleness` en el máximo posible, ya que aumentará la probabilidad de que las lecturas y consultas de punto repetidas puedan ser aciertos de caché. Si establece `MaxIntegratedCacheStaleness` en 0, la solicitud de lectura **nunca** usará la caché integrada, independientemente  del nivel de coherencia. Cuando no está configurado, el valor predeterminado de `MaxIntegratedCacheStaleness` es 5 minutos.

**.NET**

```csharp
FeedIterator<Food> myQuery = container.GetItemQueryIterator<Food>(new QueryDefinition("SELECT * FROM c"), requestOptions: new QueryRequestOptions
        {
            ConsistencyLevel = ConsistencyLevel.Eventual,
            DedicatedGatewayRequestOptions = new DedicatedGatewayRequestOptions 
            { 
                MaxIntegratedCacheStaleness = TimeSpan.FromMinutes(30) 
            }
        }
);
```

> [!NOTE]
> Actualmente, solo puede ajustar MaxIntegratedCacheStaleness mediante los SDK de versión preliminar de [.NET](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/3.17.0-preview) y [Java](https://mvnrepository.com/artifact/com.azure/azure-cosmos/4.16.0-beta.1) más recientes.

## <a name="verify-cache-hits"></a>Comprobación de los resultados de la caché

Por último, puede reiniciar la aplicación y comprobar los aciertos integrados de la caché para las lecturas o consultas de punto repetidas. Una vez que haya modificado `CosmosClient` para poder usar el punto de conexión de la puerta de enlace dedicada, todas las solicitudes se enrutarán a través de esa puerta de enlace dedicada.

Para que una solicitud de lectura (esto es, una lectura o consulta de punto) use la caché integrada, deben cumplirse **todos** los criterios siguientes:

-   El cliente debe conectarse al punto de conexión de la puerta de enlace dedicada.
-  El cliente usa el modo de puerta de enlace (los SDK de Python y Node.js siempre usan el modo de puerta de enlace).
-   La coherencia de la solicitud debe establecerse en un estado de sesión o evento.

> [!NOTE]
> ¿Tiene comentarios sobre la caché integrada? Queremos conocerlos. No dude en compartir sus comentarios directamente con el equipo de ingeniería de Azure Cosmos DB: cosmoscachefeedback@microsoft.com.


## <a name="next-steps"></a>Pasos siguientes

- [Preguntas más frecuentes sobre la caché integrada](integrated-cache-faq.md)
- [Información general de la caché integrada](integrated-cache.md)
- [Puerta de enlace dedicada](dedicated-gateway.md)