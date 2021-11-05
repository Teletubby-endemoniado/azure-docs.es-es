---
title: Introducción a las versiones de tiempo de ejecución de Azure Functions
description: Azure Functions admite varias versiones del runtime. Conozca las diferencias entre ellas y cómo elegir la más adecuada en su caso.
ms.topic: conceptual
ms.custom: devx-track-dotnet
ms.date: 10/26/2021
ms.openlocfilehash: 1a1d2cc0e5aab3daac5bb65881f9e891497fd994
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131048729"
---
# <a name="azure-functions-runtime-versions-overview"></a>Introducción a las versiones de tiempo de ejecución de Azure Functions

Actualmente, Azure Functions admite varias versiones del host del entorno de ejecución. En la tabla siguiente se detallan las versiones disponibles, su nivel de compatibilidad y cuándo se deben usar:

| Versión | Nivel de compatibilidad | Descripción |
| --- | --- | --- |
| 4.x | Vista previa | Admite todos los lenguajes. Use esta versión para [ejecutar funciones de C# en .NET 6.0](functions-dotnet-class-library.md#supported-versions). |
| 3.x | GA | _Versión del entorno de ejecución recomendada para funciones en todos los lenguajes._ |
| 2.x | GA | Compatible con [aplicaciones heredadas de la versión 2.x](#pinning-to-version-20). Esta versión está en modo de mantenimiento y solo se realizarán mejoras en versiones posteriores.|
| 1.x | Disponibilidad general | Se recomienda únicamente para las aplicaciones de C# que deben usar .NET Framework y solo admite el desarrollo en Azure Portal, el portal de Azure Stack Hub o localmente en equipos Windows. Esta versión está en modo de mantenimiento y solo se realizarán mejoras en versiones posteriores. |

En este artículo se detallan algunas de las diferencias entre estas versiones, cómo se puede crear cada versión y cómo cambiar la versión en la que se ejecutan las funciones.

[!INCLUDE [functions-support-levels](../../includes/functions-support-levels.md)]

## <a name="languages"></a>Lenguajes

A partir de la versión 2.x, el entorno de ejecución usa un modelo de extensibilidad de lenguaje y todas las funciones de una aplicación de funciones deben compartir el mismo lenguaje. Eligió el lenguaje de las funciones en su aplicación de funciones al crearla. El lenguaje de la aplicación de funciones se mantiene en la configuración [FUNCTIONS\_WORKER\_RUNTIME](functions-app-settings.md#functions_worker_runtime) y no se debe cambiar cuando hay funciones existentes. 

En la siguiente tabla se indican los lenguajes de programación que se admiten actualmente en cada versión del entorno de ejecución.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

## <a name="run-on-a-specific-version"></a><a name="creating-1x-apps"></a>Ejecución en una versión específica

De forma predeterminada, las aplicaciones de funciones que se crean en Azure Portal y en la CLI de Azure se establecen en la versión 3.x. Puede modificar esta versión según sea necesario. Solo puede degradar la versión del entorno en tiempo de ejecución a 1.x después de crear la aplicación de funciones, pero antes de agregar ninguna función.  Se permite el cambio entre 2.x y 3.x incluso con aplicaciones que tienen funciones. Antes de cambiar una aplicación con funciones existentes de 2.x a 3.x, tenga en cuenta los [cambios importantes entre las versiones 2.x y 3.x](#breaking-changes-between-2x-and-3x). 

Antes de realizar un cambio a la versión principal del tiempo de ejecución, primero debe probar el código existente mediante la implementación a otra aplicación de funciones que se ejecute en la versión principal más reciente. Esta prueba ayuda a comprobar que se ejecuta correctamente después de la actualización. 

No se admiten degradaciones de v3.x a v2.x. Siempre que sea posible, debe ejecutar las aplicaciones en la versión más reciente admitida del tiempo de ejecución de Functions. 

### <a name="changing-version-of-apps-in-azure"></a>Cambio de la versión de las aplicaciones en Azure

La versión del sistema en ejecución de Functions que usan las aplicaciones publicadas en Azure viene determinada por la configuración de la aplicación [`FUNCTIONS_EXTENSION_VERSION`](functions-app-settings.md#functions_extension_version). Se admiten los siguientes valores principales de la versión del entorno en tiempo de ejecución:

| Value | Destino del entorno en tiempo de ejecución |
| ------ | -------- |
| `~4` | 4.x (versión preliminar) |
| `~3` | 3.x |
| `~2` | 2.x |
| `~1` | 1.x |

>[!IMPORTANT]
> No cambie esta configuración sin motivo, ya que puede requerir otros cambios de configuración de la aplicación y cambios en el código de función.

Para más información, consulte [Cómo seleccionar un destino para versiones en tiempo de ejecución de Azure Functions](set-runtime-version.md).  

### <a name="pinning-to-a-specific-minor-version"></a>Anclaje a una versión secundaria específica

Para resolver problemas con la aplicación de funciones que se ejecuta en la versión principal más reciente, tiene que anclar la aplicación a una versión secundaria específica. Esto le proporciona tiempo para que la aplicación se ejecute correctamente en la versión principal más reciente. La forma de anclar a una versión secundaria difiere entre Windows y Linux. Para más información, consulte [Cómo seleccionar un destino para versiones en tiempo de ejecución de Azure Functions](set-runtime-version.md).

Las versiones secundarias anteriores se quitan periódicamente de Functions. Para obtener las últimas noticias sobre las versiones de Azure Functions, incluida la eliminación de versiones secundarias específicas anteriores, revise los [anuncios de Azure App Service](https://github.com/Azure/app-service-announcements/issues). 

### <a name="pinning-to-version-20"></a>Anclaje a la versión ~2.0

Las aplicaciones de funciones de .NET que se ejecutan en la versión 2.x (`~2`) se actualizan automáticamente para ejecutarse en .NET Core 3.1, que es una versión de compatibilidad a largo plazo de .NET Core 3. La ejecución de las funciones de .NET en .NET Core 3.1 le permite aprovechar las últimas actualizaciones de seguridad y las mejoras del producto. 

Cualquier aplicación de funciones anclada a `~2.0` se sigue ejecutando en .NET Core 2.2, que ya no recibe seguridad ni otras actualizaciones. Para más información, consulte [Consideraciones sobre Functions v2.x](functions-dotnet-class-library.md#functions-v2x-considerations).   

## <a name="migrating-from-3x-to-4x-preview"></a><a name="migrating-from-3x-to-4x"></a>Migración de 3.x a 4.x (versión preliminar)

Azure Functions versión 4.x (versión preliminar) es una versión muy compatible con la versión 3.x. Muchas aplicaciones se deben actualizar de forma segura a la versión 4.x sin ningún cambio de código significativo. Asegúrese de ejecutar unas pruebas exhaustivas antes de cambiar la versión principal en las aplicaciones de producción.

### <a name="upgrading-an-existing-app"></a>Actualización de una aplicación existente

#### <a name="local-project"></a>Proyecto local

# <a name="c"></a>[C\#](#tab/csharp)
 
Para actualizar una aplicación .NET a .NET 6 y Azure Functions 4.x, actualice `TargetFramework` y `AzureFunctionsVersion`:

```xml
<TargetFramework>net6.0</TargetFramework>
<AzureFunctionsVersion>v4</AzureFunctionsVersion>
```

Además, asegúrese de que las referencias de los paquetes NuGet de la aplicación estén actualizadas con las versiones más recientes. Consulte [Cambios importantes](#breaking-changes-between-3x-and-4x) para obtener más información.

##### <a name="net-6-in-process"></a>.NET 6 en proceso

* [Microsoft.NET.Sdk.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) 4.0.0 o posterior

##### <a name="net-6-isolated"></a>.NET 6 aislado

* [Microsoft.Azure.Functions.Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker/) 1.5.2 o posterior
* [Microsoft.Azure.Functions.Worker.Sdk](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker.Sdk/) 1.2.0 o posterior

# <a name="java"></a>[Java](#tab/java)

Para actualizar la aplicación Java a Azure Functions 4.x, actualice la instalación local de [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools) a 4.x y actualice el [conjunto de extensiones de Azure Functions](functions-bindings-register.md#extension-bundles) de la aplicación a 2.x o superior. Consulte [Cambios importantes](#breaking-changes-between-3x-and-4x) para obtener más información.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Para actualizar la aplicación Java.js a Azure Functions 4.x, actualice la instalación local de [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools) a 4.x y actualice el [conjunto de extensiones de Azure Functions](functions-bindings-register.md#extension-bundles) de la aplicación a 2.x o superior. Consulte [Cambios importantes](#breaking-changes-between-3x-and-4x) para obtener más información.

> [!NOTE]
> Node.js 10 y 12 no se admiten en Azure Functions 4.x.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Para actualizar la aplicación PowerShell a Azure Functions 4.x, actualice la instalación local de [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools) a 4.x y actualice el [conjunto de extensiones de Azure Functions](functions-bindings-register.md#extension-bundles) de la aplicación a 2.x o superior. Consulte [Cambios importantes](#breaking-changes-between-3x-and-4x) para obtener más información.

> [!NOTE]
> PowerShell 6 no se admite en Azure Functions 4.x.

# <a name="python"></a>[Python](#tab/python)

Para actualizar la aplicación Python a Azure Functions 4.x, actualice la instalación local de [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools) a 4.x y actualice el [conjunto de extensiones de Azure Functions](functions-bindings-register.md#extension-bundles) de la aplicación a 2.x o superior. Consulte [Cambios importantes](#breaking-changes-between-3x-and-4x) para obtener más información.

> [!NOTE]
> Python 3.6 no se admite en Azure Functions 4.x.

---


#### <a name="azure"></a>Azure

Para migrar una aplicación de la versión 3.x a la 4.x, establezca la configuración `FUNCTIONS_EXTENSION_VERSION` de la aplicación en `~4` con el siguiente comando de la CLI de Azure:

```bash
az functionapp config appsettings set --settings FUNCTIONS_EXTENSION_VERSION=~4 -n <APP_NAME> -g <RESOURCE_GROUP_NAME>

# For Windows function apps only, also enable .NET 6.0 that is needed by the runtime
az functionapp config set --net-framework-version v6.0 -n <APP_NAME> -g <RESOURCE_GROUP_NAME>
```

### <a name="breaking-changes-between-3x-and-4x"></a>Cambios importantes entre 3.x y 4.x

A continuación se indican algunos cambios que se deben tener en cuenta antes de actualizar una aplicación de 3.x a 4.x. Para obtener una lista completa, consulte las incidencias del repositorio de Azure Functions en GitHub que tienen la etiqueta [*Breaking Change: Approved*](https://github.com/Azure/azure-functions/issues?q=is%3Aissue+label%3A%22Breaking+Change%3A+Approved%22+is%3A%22closed+OR+open%22) (Cambio importante: aprobado). Se esperan más cambios durante el período de versión preliminar. Suscríbase a los [anuncios de App Service](https://github.com/Azure/app-service-announcements/issues) para recibir actualizaciones.

#### <a name="runtime"></a>Tiempo de ejecución

- Azure Functions Proxies ya no se admite en la versión 4.x. Se recomienda usar [Azure API Management](../api-management/import-function-app-as-api.md).

- El registro en Azure Storage con *AzureWebJobsDashboard* ya no se admite en la versión 4.x. Se recomienda usar [Application Insights](./functions-monitoring.md). ([#1923](https://github.com/Azure/Azure-Functions/issues/1923))

- Azure Functions 4.x exige [requisitos de versión mínima](https://github.com/Azure/Azure-Functions/issues/1987) para las extensiones. Actualice a la versión más reciente de las extensiones afectadas. Para lenguajes que no son .NET, [actualice](./functions-bindings-register.md#extension-bundles) a la versión 2.x del conjunto de extensiones o a una versión posterior. ([#1987](https://github.com/Azure/Azure-Functions/issues/1987))

- Ahora se aplican tiempos de espera predeterminados y máximos en las aplicaciones de funciones de consumo para Linux 4.x. ([#1915](https://github.com/Azure/Azure-Functions/issues/1915))

- Application Insights no se incluye de manera predeterminada en la versión preliminar de Azure Functions 4.0.0.16714. Está disponible como una extensión independiente. ([#2027](https://github.com/Azure/Azure-Functions/issues/2027))
    
    > [!NOTE]
    > Se trata de un cambio temporal en la versión 4.0.0.16714. No se requiere una extensión para usar Application Insights en versiones futuras. Si ha instalado la extensión, quítela de la aplicación si usa la versión 4.0.1.16815 o posterior de Azure Functions.
    
    - En el caso de las aplicaciones .NET en proceso, agregue el paquete de extensión [Microsoft.Azure.WebJobs.Extensions.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApplicationInsights/) a la aplicación de funciones.
    - Para aplicaciones :NET aisladas:
        - Agregue el paquete de extensión [Microsoft.Azure.Functions.Worker.Extensions.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker.Extensions.ApplicationInsights/) a la aplicación de funciones.
        - Actualice los paquetes [Microsoft.Azure.Functions.Worker](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker/) y [Microsoft.Azure.Functions.Worker.Sdk](https://www.nuget.org/packages/Microsoft.Azure.Functions.Worker.Sdk/) a las versiones más recientes.
    - Para otros lenguajes, una actualización futura para los [conjuntos de extensiones de Azure Functions](functions-bindings-register.md#extension-bundles) incluirá la extensión de Application Insights. La aplicación usará automáticamente el nuevo paquete cuando esté disponible. Hasta que el conjunto de extensiones actualizado esté listo, quite los valores de aplicación `APPINSIGHTS_INSTRUMENTATIONKEY` y `APPLICATIONINSIGHTS_CONNECTION_STRING` de la aplicación de funciones. Esto impedirá que el host no se inicie.

- Las aplicaciones de funciones que comparten cuentas de almacenamiento no se iniciarán si sus nombres de host calculados son los mismos. Use una cuenta de almacenamiento independiente para cada aplicación de funciones. ([#2049](https://github.com/Azure/Azure-Functions/issues/2049))

#### <a name="languages"></a>Idiomas

# <a name="c"></a>[C\#](#tab/csharp)

- Azure Functions 4.x admite aplicaciones en proceso y aisladas de .NET 6.

- `InvalidHostServicesException` ahora es un error irrecuperable. ([#2045](https://github.com/Azure/Azure-Functions/issues/2045))

- `EnableEnhancedScopes` se habilita de forma predeterminada. ([#1954](https://github.com/Azure/Azure-Functions/issues/1954))

- Quite `HttpClient` como servicio registrado. ([#1911](https://github.com/Azure/Azure-Functions/issues/1911))

# <a name="java"></a>[Java](#tab/java)

- Use un cargador de clase única en Java 11. ([#1997](https://github.com/Azure/Azure-Functions/issues/1997))

- Detenga la carga de archivos JAR de trabajo en Java 8. ([#1991](https://github.com/Azure/Azure-Functions/issues/1991))

# <a name="javascript"></a>[JavaScript](#tab/javascript)

- Node.js 10 y 12 no se admiten en Azure Functions 4.x. ([#1999](https://github.com/Azure/Azure-Functions/issues/1999))

- La serialización de salida en aplicaciones de Node.js se actualizó para solucionar las incoherencias anteriores. ([#2007](https://github.com/Azure/Azure-Functions/issues/2007))

# <a name="powershell"></a>[PowerShell](#tab/powershell)

- PowerShell 6 no se admite en Azure Functions 4.x. ([#1999](https://github.com/Azure/Azure-Functions/issues/1999))

- Se ha actualizado el recuento de subprocesos predeterminado. Las funciones que no son seguras para subprocesos o que tienen un uso elevado de memoria pueden estar afectadas. ([#1962](https://github.com/Azure/Azure-Functions/issues/1962))

# <a name="python"></a>[Python](#tab/python)

- Python 3.6 no se admite en Azure Functions 4.x. ([#1999](https://github.com/Azure/Azure-Functions/issues/1999))

- La transferencia de memoria compartida está habilitada de manera predeterminada. ([#1973](https://github.com/Azure/Azure-Functions/issues/1973))

- Se ha actualizado el recuento de subprocesos predeterminado. Las funciones que no son seguras para subprocesos o que tienen un uso elevado de memoria pueden estar afectadas. ([#1962](https://github.com/Azure/Azure-Functions/issues/1962))

---

## <a name="migrating-from-2x-to-3x"></a>Migración de 2.x a 3.x

Azure Functions versión 3.x es una versión muy compatible con la versión 2.x.  Muchas aplicaciones se pueden actualizar a 3.x sin ningún cambio de código. Aunque se recomienda pasar a 3.x, ejecute unas pruebas exhaustivas antes de cambiar la versión principal en las aplicaciones de producción.

### <a name="breaking-changes-between-2x-and-3x"></a>Cambios importantes entre 2.x y 3.x

A continuación se indican los cambios específicos para cada lenguaje que se deben tener en cuenta antes de actualizar una aplicación de 2.x a 3.x.

# <a name="c"></a>[C\#](#tab/csharp)

Las principales diferencias entre las versiones en las que se ejecutan las funciones de la biblioteca de clases de .NET es el tiempo de ejecución de .NET Core. La versión 2.x de Functions está diseñada para ejecutarse en .NET Core 2.2 y la versión 3.x está diseñada para ejecutarse en .NET Core 3.1.  

* [Las operaciones de servidor sincrónicas están deshabilitadas de forma predeterminada](/dotnet/core/compatibility/2.2-3.0#http-synchronous-io-disabled-in-all-servers).

* Cambios importantes introducidos por .NET Core en la [versión 3.1](/dotnet/core/compatibility/3.1) y la [versión 3.0](/dotnet/core/compatibility/3.0), que no son específicos de Functions, pero pueden afectar igualmente a la aplicación.

>[!NOTE]
>Debido a problemas de compatibilidad con .NET Core 2.2, las aplicaciones de funciones ancladas a la versión 2 (`~2`) se ejecutan esencialmente en .NET Core 3.1. Para obtener más información, consulte el tema sobre el [modo de compatibilidad de Functions v2.x](functions-dotnet-class-library.md#functions-v2x-considerations).

# <a name="java"></a>[Java](#tab/java)

Ninguno.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

* Los enlaces de salida asignados mediante `context.done` o valores devueltos ahora se comportan igual que cuando se establecen en `context.bindings`.

* El objeto del desencadenador de temporizador es camelCase en lugar de PascalCase

* Las funciones desencadenadas por el centro de eventos con el binario `dataType` recibirán una matriz de `binary` en lugar de `string`.

* Ya no se puede obtener acceso a la carga de la solicitud HTTP mediante `context.bindingData.req`.  Todavía se puede acceder a él como parámetro de entrada, `context.req`, y en `context.bindings`.

* Node.js 8 ya no se admite y no se ejecutará en las funciones de 3.x.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Ninguno.

# <a name="python"></a>[Python](#tab/python)

Ninguno.

---

## <a name="migrating-from-1x-to-later-versions"></a>Migración desde la versión 1.x a versiones posteriores

Puede migrar una aplicación existente escrita para usar la versión 1.x del entorno en tiempo de ejecución para que utilice en su lugar una versión más reciente. La mayoría de los cambios que debe realizar están relacionados con cambios en el entorno en tiempo de ejecución del lenguaje, como cambios en la API de C# entre .NET Framework 4.8 y .NET Core. Deberá asegurarse de que el código y las bibliotecas son compatibles con la versión del entorno de ejecución del lenguaje que utilice. Finalmente, asegúrese de anotar los cambios en el desencadenador, los enlaces y las características que se resaltan a continuación. Para obtener los mejores resultados de migración, debe crear una nueva aplicación de funciones en una nueva versión y portar el código de función de la versión 1.x existente a la nueva aplicación.  

Aunque es posible realizar una actualización "en contexto" actualizando manualmente la configuración de la aplicación, al pasar de 1.x a una versión posterior, se incluyen algunos cambios importantes. Por ejemplo, en C# se modifica el objeto de depuración de `TraceWriter` a `ILogger`. Al crear un nuevo proyecto con la versión 3.x, empieza por las funciones actualizadas en función de las plantillas más recientes de la versión 3.x.

### <a name="changes-in-triggers-and-bindings-after-version-1x"></a>Cambios en los desencadenadores y los enlaces después de la versión 1.x

A partir de la versión 2.x, debe instalar las extensiones para desencadenadores y enlaces específicos que las funciones de la aplicación utilizan. La única excepción son los desencadenadores HTTP y el temporizador, que no requieren extensión.  Para más información, consulte la sección sobre [Registro e instalación de extensiones de enlace](./functions-bindings-register.md).

También hay algunos cambios en *function.json* o los atributos de la función de una versión a otra. Por ejemplo, la propiedad `path` del centro de eventos es ahora `eventHubName`. Consulte la [tabla de enlaces existentes](#bindings) para vínculos a la documentación de cada enlace.

### <a name="changes-in-features-and-functionality-after-version-1x"></a>Cambios en las características y la funcionalidad después de la versión 1.x

Se han quitado, actualizado o reemplazado algunas características después de la versión 1.x. En esta sección se describen los cambios que se observan en versiones posteriores después de haber utilizado la versión 1.x.

En la versión 2.x se han realizado los siguientes cambios:

* Las claves para llamar a los puntos de conexión HTTP siempre se almacenan cifradas en Azure Blob Storage. En la versión 1.x, se almacenaban en Azure Files de manera predeterminada. Al actualizar una aplicación de la versión 1.x a la 2.x, se restablecen los secretos existentes que se encuentran en Azure Files.

* La versión 2.x del entorno de ejecución no incluye compatibilidad integrada con proveedores de webhooks. Este cambio se realizó para mejorar el rendimiento. Todavía puede usar desencadenadores HTTP como puntos de conexión para los webhooks.

* El archivo de configuración de host (host.json) debe estar vacío o tener la cadena `"version": "2.0"`.

* Para mejorar la supervisión, el panel de WebJobs del portal, que usaba la configuración [`AzureWebJobsDashboard`](functions-app-settings.md#azurewebjobsdashboard) se reemplaza por Azure Application Insights, que usa la configuración [`APPINSIGHTS_INSTRUMENTATIONKEY`](functions-app-settings.md#appinsights_instrumentationkey). Para más información, consulte [Supervisión de Azure Functions](functions-monitoring.md).

* Todas las funciones de una aplicación de función deben compartir el mismo lenguaje. Al crear una aplicación de función, debe elegir una pila del entorno de ejecución para la aplicación. La pila del entorno de ejecución se especifica con el valor [`FUNCTIONS_WORKER_RUNTIME`](functions-app-settings.md#functions_worker_runtime) en la configuración de la aplicación. Este requisito se agregó para mejorar el tiempo de inicio y la superficie de memoria. Al desarrollar localmente, también debe incluir esta configuración en el [archivo local.settings.json](functions-develop-local.md#local-settings-file).

* El tiempo de espera predeterminado para las funciones en un plan de App Service ha cambiado a 30 minutos. Puede cambiar manualmente el tiempo de espera de vuelta a ilimitado mediante la configuración [functionTimeout](functions-host-json.md#functiontimeout) de host.json.

* Se implementan de forma predeterminada limitaciones de simultaneidad HTTP para las funciones del plan de consumo, cuyo valor predeterminado es de 100 solicitudes simultáneas por instancia. Puede cambiar esto en la configuración [`maxConcurrentRequests`](functions-host-json.md#http) del archivo host.json.

* A causa de las [limitaciones de .NET Core](https://github.com/Azure/azure-functions-host/issues/3414), se ha eliminado la compatibilidad con las funciones de script (.fsx) de F#. Todavía se admiten las funciones compiladas de F# (.fs).

* El formato de dirección URL de los webhooks de desencadenador de Event Grid ha cambiado a `https://{app}/runtime/webhooks/{triggerName}`.

### <a name="locally-developed-application-versions"></a>Versiones de aplicaciones desarrolladas de forma local

Puede hacer que las siguientes actualizaciones de las aplicaciones de funciones cambien localmente las versiones de destino.

#### <a name="visual-studio-runtime-versions"></a>Versiones de Runtime en Visual Studio

En Visual Studio se selecciona la versión del entorno de ejecución al crear un proyecto. Las herramientas de Azure Functions para Visual Studio son compatibles con las tres versiones principales del entorno de ejecución. Se usa la versión correcta al depurar y publicar en función de configuración del proyecto. La configuración de la versión se define en el archivo `.csproj` en las siguientes propiedades:

# <a name="version-4x-preview"></a>[Versión 4.x (versión preliminar)](#tab/v4)

```xml
<TargetFramework>net6.0</TargetFramework>
<AzureFunctionsVersion>v4</AzureFunctionsVersion>
```

> [!NOTE]
> Azure Functions 4.x requiere que la extensión `Microsoft.NET.Sdk.Functions` sea al menos `4.0.0`.

# <a name="version-3x"></a>[Versión 3.x](#tab/v3)

```xml
<TargetFramework>netcoreapp3.1</TargetFramework>
<AzureFunctionsVersion>v3</AzureFunctionsVersion>
```

> [!NOTE]
> Azure Functions 3.x y .NET requieren que la extensión `Microsoft.NET.Sdk.Functions` sea al menos `3.0.0`.

# <a name="version-2x"></a>[Versión 2.x](#tab/v2)

```xml
<TargetFramework>netcoreapp2.1</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

# <a name="version-1x"></a>[Versión 1.x](#tab/v1)

```xml
<TargetFramework>net472</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```
---

###### <a name="updating-2x-apps-to-3x-in-visual-studio"></a>Actualización de las aplicaciones de 2.x a 3.x en Visual Studio

Puede abrir una función existente que tenga como destino 2.x y pasar a 3.x editando el archivo `.csproj` y actualizando los valores anteriores.  Visual Studio administra las versiones del entorno en tiempo de ejecución automáticamente en función de los metadatos del proyecto.  Sin embargo, es posible si nunca ha creado una aplicación 3.x antes de que Visual Studio no tuviera aún las plantillas y el entorno en tiempo de ejecución de 3.x en la máquina.  Esto puede presentar un error como "no hay ningún entorno en tiempo de ejecución de Functions disponible que coincida con la versión especificada en el proyecto".  Para obtener las plantillas y el entorno en tiempo de ejecución más recientes, consulte la experiencia para crear un nuevo proyecto de función.  Cuando llegue a la pantalla de selección de versión y plantilla, espere a que Visual Studio complete la captura de las plantillas más recientes. Cuando las plantillas más recientes de .NET Core 3 estén disponibles y se muestren, puede ejecutar y depurar cualquier proyecto configurado para la versión 3.x.

> [!IMPORTANT]
> Las funciones de la versión 3.x solo se pueden desarrollar en Visual Studio si se usa la Visual Studio versión 16.4 o una más reciente.

#### <a name="vs-code-and-azure-functions-core-tools"></a>VS Code y Azure Functions Core Tools

[Azure Functions Core Tools](functions-run-local.md) se usa para el desarrollo de la línea de comandos, pero también lo usa la [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para Visual Studio Code. Para desarrollar con la versión 3.x, instale la versión 3.x de Core Tools. El desarrollo con la versión 2.x requiere la versión 2.x de Core Tools, etc. Para más información, consulte [Instalación de Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools).

Para el desarrollo en Visual Studio Code es posible que deba actualizar la configuración de usuario en `azureFunctions.projectRuntime` para que coincida con la versión de las herramientas instaladas.  Esta configuración también actualiza las plantillas y los lenguajes utilizados durante la creación de la aplicación de función.  Para crear aplicaciones en `~3`, se debe actualizar la configuración de usuario de `azureFunctions.projectRuntime` a `~3`.

![Configuración del entorno en tiempo de ejecución de la extensión de Azure Functions](./media/functions-versions/vs-code-version-runtime.png)

#### <a name="maven-and-java-apps"></a>Aplicaciones de Maven y Java

Puede migrar aplicaciones Java de la versión 2.x a la versión 3.x [instalando la versión 3.x de las herramientas principales](functions-run-local.md#install-the-azure-functions-core-tools) necesaria para ejecutarse localmente.  Después de comprobar que la aplicación se ejecuta correctamente en la versión 3.x, actualice el archivo de `POM.xml` de la aplicación para modificar la configuración de `FUNCTIONS_EXTENSION_VERSION` a `~3`, como en el ejemplo siguiente:

```xml
<configuration>
    <resourceGroup>${functionResourceGroup}</resourceGroup>
    <appName>${functionAppName}</appName>
    <region>${functionAppRegion}</region>
    <appSettings>
        <property>
            <name>WEBSITE_RUN_FROM_PACKAGE</name>
            <value>1</value>
        </property>
        <property>
            <name>FUNCTIONS_EXTENSION_VERSION</name>
            <value>~3</value>
        </property>
    </appSettings>
</configuration>
```

## <a name="bindings"></a>Enlaces

A partir de la versión 2.x, el entorno de ejecución usa un nuevo [modelo de extensibilidad de enlaces](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) que ofrece estas ventajas:

* Compatibilidad con extensiones de enlace de terceros.

* Desacoplamiento de tiempo de ejecución y enlaces. Este cambio permite el control de versiones y la publicación de las extensiones de enlace de forma independiente. Por ejemplo, puede elegir actualizar a una versión de una extensión que se basa en una versión más reciente de un SDK subyacente.

* Un entorno de ejecución más ligero, donde solo se conocen y se cargan en tiempo de ejecución los enlaces que están en uso.

A excepción de los desencadenadores HTTP y el temporizador, todos los enlaces deben agregarse explícitamente al proyecto de aplicación de función o registrarse en el portal. Para más información, consulte [Registro de extensiones de enlace](./functions-bindings-expressions-patterns.md).

En la siguiente tabla se indica qué enlaces se admiten en cada versión del entorno de ejecución.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

* [Codificación y comprobación de Azure Functions en un entorno local](functions-run-local.md)
* [Cómo elegir las versiones del entorno de ejecución de Azure Functions](set-runtime-version.md)
* [Notas de la versión](https://github.com/Azure/azure-functions-host/releases)
