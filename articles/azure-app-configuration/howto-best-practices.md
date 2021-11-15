---
title: Procedimientos recomendados para Azure App Configuration | Microsoft Docs
description: Obtenga información sobre procedimientos recomendados al usar Azure App Configuration. Entre los temas tratados se incluyen las agrupaciones de claves, las composiciones de pares clave-valor, el arranque de App Configuration, etc.
services: azure-app-configuration
documentationcenter: ''
author: AlexandraKemperMS
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: alkemper
ms.custom: devx-track-csharp, mvc
ms.openlocfilehash: e75fb11379ccbdff90d1acd1a3bce36b62bd8a1d
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130250870"
---
# <a name="azure-app-configuration-best-practices"></a>Procedimientos recomendados para Azure App Configuration

En este artículo se describen patrones comunes y procedimientos recomendados cuando se usa Azure App Configuration.

## <a name="key-groupings"></a>Agrupaciones de claves

App Configuration proporciona dos opciones para organizar las claves:

* Prefijos de clave
* Etiquetas

Puede usar una o ambas opciones para agrupar sus claves.

*Los prefijos de clave* son lo que va al principio de la clave. Puede agrupar lógicamente un conjunto de claves utilizando el mismo prefijo en sus nombres. Los prefijos pueden contener varios componentes conectados por un delimitador, como `/`, de forma similar a una ruta de dirección de URL, para formar un espacio de nombres. Estas jerarquías son útiles cuando se están almacenando las claves para muchas aplicaciones y microservicios en un almacén de App Configuration.

Una cuestión importante que debe tener en cuenta es que las claves son a lo que el código de aplicación hace referencia para recuperar los valores de la configuración correspondiente. Las claves no deberían cambiar o bien tendrá que modificar el código cada vez que suceda.

*Las etiquetas* son un atributo en las claves. Se usan para crear variantes de una clave. Por ejemplo, puede asignar etiquetas a varias versiones de una clave. Una versión podría ser una iteración, un entorno o cualquier otra información contextual. La aplicación puede solicitar un conjunto completamente distinto de los valores de clave al especificar otra etiqueta. Como resultado, todas las referencias de clave permanecen sin cambios en el código.

## <a name="key-value-compositions"></a>Composiciones de pares clave-valor

App Configuration trata todas las claves que se almacenan con él como entidades independientes. App Configuration no intenta deducir ninguna relación entre las claves o heredar valores de clave en función de su jerarquía. Sin embargo, es posible agregar varios conjuntos de claves mediante el uso de etiquetas junto con la configuración correcta del apilamiento en el código de la aplicación.

Veamos un ejemplo. Suponga que tiene una opción denominada **Asset1** (Recurso1), cuyo valor puede variar según el entorno de desarrollo. Cree una clave denominada "Asset1" con una etiqueta vacía y una etiqueta denominada "Development" (Desarrollo). En la primera etiqueta, coloca el valor predeterminado de **Asset1** y debe incluir un valor específico para "Development" en la última.

En el código, primero recupera los valores de clave sin ninguna etiqueta y, a continuación, recupera el mismo conjunto de valores de clave con la etiqueta "Development". Cuando se recuperan los valores la segunda vez, se sobrescriben los valores anteriores de las claves. El sistema de configuración de .NET Core permite "apilar" varios conjuntos de datos de configuración en la parte superior entre sí. Si una clave existe en más de un conjunto, se utiliza el último conjunto que la contiene. Con un marco de programación moderno, como .NET Core, obtiene esta funcionalidad de apilamiento de forma gratuita si usa un proveedor de configuración nativo para tener acceso a App Configuration. El fragmento de código siguiente le muestra cómo puede implementar el apilamiento en una aplicación .NET Core:

```csharp
// Augment the ConfigurationBuilder with Azure App Configuration
// Pull the connection string from an environment variable
configBuilder.AddAzureAppConfiguration(options => {
    options.Connect(configuration["connection_string"])
           .Select(KeyFilter.Any, LabelFilter.Null)
           .Select(KeyFilter.Any, "Development");
});
```

[Uso de etiquetas para habilitar diferentes configuraciones para distintos entornos](./howto-labels-aspnet-core.md) proporciona un ejemplo completo.

## <a name="references-to-external-data"></a>Referencias a datos externos

App Configuration está diseñado para almacenar los datos de configuración que normalmente se guardarían en archivos de configuración o variables de entorno. Sin embargo, algunos tipos de datos pueden ser más adecuados para residir en otros orígenes. Por ejemplo, almacene secretos en Key Vault, archivos en Azure Storage, información de pertenencia en grupos Azure AD o listas de clientes en una base de datos.

Aún puede aprovechar App Configuration si guarda una referencia a datos externos en un par clave-valor. Puede [utilizar el tipo de contenido](./concept-key-value.md#use-content-type) a fin de diferenciar cada origen de datos. Cuando la aplicación lee una referencia, los datos se cargan desde el origen al que se hace referencia. En caso de que cambie la ubicación de los datos externos, solo tendrá que actualizar la referencia en App Configuration en lugar de actualizar y volver a implementar toda la aplicación.

La característica de [referencias de Key Vault](use-key-vault-references-dotnet-core.md) de App Configuration es un ejemplo en este caso. Permite que los secretos necesarios para una aplicación se actualicen según sea necesario mientras los propios secretos subyacentes permanecen en Key Vault.

## <a name="app-configuration-bootstrap"></a>Arranque de App Configuration

Para acceder a un almacén de App Configuration, puede usar su cadena de conexión, que está disponible en Azure Portal. Dado que las cadenas de conexión contienen información de credenciales, se consideran secretos. Estos secretos deben almacenarse en Azure Key Vault y el código debe autenticarse en Key Vault para recuperarlos.

Una mejor opción es usar la característica de las identidades administradas en Azure Active Directory. Con las identidades administradas, solo necesita la dirección URL del punto de conexión de App Configuration para el acceso de arranque a su almacén de App Configuration. Puede insertar la dirección URL en el código de aplicación (por ejemplo, en el archivo *appsettings.json*). Consulte [Integración con las identidades administradas de Azure](howto-integrate-azure-managed-service-identity.md) para obtener detalles.

## <a name="app-or-function-access-to-app-configuration"></a>Acceso a funciones o aplicaciones para App Configuration

Puede proporcionar acceso a App Configuration para Web Apps o Azure Functions mediante alguno de los métodos siguientes:

* En Azure Portal, escriba la cadena de conexión para el almacén de App Configuration en la configuración de la aplicación de App Service.
* Almacene la cadena de conexión para su almacén de App Configuration en Key Vault y [haga referencia a ella desde App Service](../app-service/app-service-key-vault-references.md).
* Use las identidades administradas de Azure para acceder al almacén de App Configuration. Para más información, consulte [Integración con identidades administradas de Azure](howto-integrate-azure-managed-service-identity.md).
* Inserte la configuración de App Configuration en App Service. App Configuration proporciona una función de exportación (en Azure Portal y la CLI de Azure) que envía los datos directamente a App Service. Con este método, no es necesario cambiar el código de la aplicación.

## <a name="reduce-requests-made-to-app-configuration"></a>Reducción de las solicitudes realizadas a App Configuration

Una cantidad excesiva de solicitudes a App Configuration puede dar lugar a cargos por superar el límite de ancho de banda o el de uso. Para reducir el número de solicitudes realizadas:

* Aumente el tiempo de espera de la actualización, especialmente si los valores de configuración no cambian con frecuencia. Especifique un nuevo tiempo de espera de actualización mediante el [método `SetCacheExpiration`](/dotnet/api/microsoft.extensions.configuration.azureappconfiguration.azureappconfigurationrefreshoptions.setcacheexpiration).

* Vea una sola *clave de Sentinel*, en lugar de ver claves individuales. Actualice toda la configuración solo si cambia la clave de Sentinel. Consulte [Uso de la configuración dinámica en una aplicación de ASP.NET Core](enable-dynamic-configuration-aspnet-core.md) para obtener un ejemplo.

* Use Azure Event Grid para recibir notificaciones cuando cambie la configuración, en lugar de realizar un sondeo constante de los cambios. Para más información, consulte [Uso de Event Grid para las notificaciones de cambios de datos de App Configuration](./howto-app-configuration-event.md).

* Reparta las solicitudes entre varios almacenes de App Configuration. Por ejemplo, utilice un almacén distinto de cada región geográfica para una aplicación implementada globalmente. Cada almacén de App Configuration tiene su propia cuota de solicitudes. Esta configuración proporciona un modelo de escalabilidad y evita el único punto de error.

## <a name="importing-configuration-data-into-app-configuration"></a>Importación de datos de configuración en App Configuration

App Configuration ofrece la opción de [importación](./howto-import-export-data.md) masiva de las opciones de los archivos de configuración actuales mediante Azure Portal o la CLI de Azure. También puede usar las mismas opciones para exportar pares clave-valor de App Configuration, por ejemplo, entre almacenes relacionados. Si desea configurar una sincronización continua con el repositorio en GitHub o Azure DevOps, puede usar nuestra [acción de GitHub](./concept-github-action.md) o la [tarea de envío de Azure Pipelines](./push-kv-devops-pipeline.md) para poder seguir usando las prácticas de control de código fuente existentes a la vez que obtiene las ventajas de App Configuration.

## <a name="multi-region-deployment-in-app-configuration"></a>Implementación en varias regiones en App Configuration

App Configuration es el servicio regional. En el caso de las aplicaciones con distintas configuraciones por región, el almacenamiento de estas configuraciones en una instancia puede crear un único punto de error. La implementación de una instancia de App Configuration por región en varias regiones puede ser mejor opción. Puede ayudar con la recuperación ante desastres regional, el rendimiento y el silo de seguridad. La configuración por región también mejora la latencia y usa cuotas de limitación independientes, ya que la limitación es por instancia. Para aplicar la mitigación de recuperación ante desastres, puede utilizar [varios almacenes de configuración](./concept-disaster-recovery.md). 

## <a name="client-applications-in-app-configuration"></a>Aplicaciones cliente en App Configuration 

Cuando use App Configuration en aplicaciones cliente, asegúrese de tener en cuenta dos factores principales. En primer lugar, si va a usar la cadena de conexión en una aplicación cliente, corre el riesgo de exponer la clave de acceso del almacén de App Configuration al público. En segundo lugar, la escala típica de una aplicación cliente podría provocar demasiadas solicitudes a su almacén de App Configuration, lo que puede dar lugar a cargos por uso por encima del límite o ciertas limitaciones. Para más información acerca de las limitaciones, consulte las [preguntas frecuentes](./faq.yml#are-there-any-limits-on-the-number-of-requests-made-to-app-configuration).

Para solucionar estos problemas, se recomienda usar un servicio de proxy entre las aplicaciones cliente y el almacén de App Configuration. El servicio de proxy puede autenticarse de forma segura con el almacén de App Configuration sin que se produzca un problema de seguridad de pérdida de información de autenticación. Para crear cualquier servicio de proxy se puede usar una de las bibliotecas de proveedores de App Configuration, con el fin de que pueda aprovechar las funcionalidades integradas de almacenamiento en caché y actualización para optimizar el volumen de solicitudes enviadas a App Configuration. Para más información sobre el uso de proveedores de App Configuration, consulte los artículos que encontrará en los inicios rápidos y tutoriales. El servicio de proxy sirve la configuración desde su caché a las aplicaciones cliente, con lo que se evitan los dos posibles problemas que se tocan en esta sección.

## <a name="next-steps"></a>Pasos siguientes

* [Claves y valores](./concept-key-value.md) 
