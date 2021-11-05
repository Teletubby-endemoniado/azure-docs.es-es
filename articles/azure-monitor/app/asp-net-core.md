---
title: Azure Application Insights para aplicaciones de ASP.NET Core | Microsoft Docs
description: Supervise la disponibilidad, el rendimiento y el uso de las aplicaciones web de ASP.NET Core.
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.date: 10/12/2021
ms.openlocfilehash: 521cebd9117c1150e8c2abc2f5ff752e195a1808
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131070059"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights para aplicaciones de ASP.NET Core

En este artículo se describe cómo habilitar Application Insights para una aplicación de [ASP.NET Core](/aspnet/core). Al completar las instrucciones de este artículo, Application Insights recopilará solicitudes, dependencias, excepciones, contadores de rendimiento, latidos y registros de la aplicación de ASP.NET Core.

El ejemplo que vamos a usar aquí es una [aplicación de MVC](/aspnet/core/tutorials/first-mvc-app) que tiene como destino `netcoreapp3.0`. Estas instrucciones se pueden aplicar a todas las aplicaciones de ASP.NET Core. Si usa el [Servicio de trabajo](/aspnet/core/fundamentals/host/hosted-services#worker-service-template), siga las instrucciones que encontrará [aquí](./worker-service.md).

> [!NOTE]
> Hay disponible una [oferta de .NET basada en OpenTelemetry](opentelemetry-enable.md?tabs=net) en versión preliminar. [Más información](opentelemetry-overview.md).

## <a name="supported-scenarios"></a>Escenarios admitidos

El [SDK de Application Insights para ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) puede supervisar sus aplicaciones, independientemente de dónde y cómo se ejecuten. Si la aplicación se está ejecutando y tiene conectividad de red a Azure, se pueden recopilar datos de telemetría. La supervisión de Application Insights se admite siempre y cuando .NET Core sea compatible. La compatibilidad abarca lo siguiente:
* **Sistema operativo**: Windows, Linux o Mac.
* **Método de hospedaje**: en el proceso o fuera del proceso.
* **Método de implementación**: marco dependiente o independiente.
* **Servidor web**: IIS (Internet Information Server) o Kestrel.
* **Plataforma de hospedaje**: característica Web Apps de Azure App Service, VM de Azure, Docker, Azure Kubernetes Service (AKS), etc.
* **Versión de .NET Core**: todas las [versiones de .NET Core compatibles](https://dotnet.microsoft.com/download/dotnet-core) oficialmente que no están en versión preliminar.
* **IDE**: Visual Studio, Visual Studio Code o la línea de comandos.

> [!NOTE]
> ASP.NET Core 3.1 requiere [Application Insights 2.8.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/2.8.0) o posterior.

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación de ASP.NET Core en funcionamiento. Si necesita crear una aplicación de ASP .NET Core, siga este [tutorial de ASP.NET Core](/aspnet/core/getting-started/).
- Una clave de instrumentación de Application Insights válida. Esta clave es necesaria para enviar los datos de telemetría a Application Insights. Si necesita crear un nuevo recurso de Application Insights para obtener un clave de instrumentación, consulte [Creación de recursos en Application Insights](./create-new-resource.md).

> [!IMPORTANT]
> Se recomiendan las [cadenas de conexión](./sdk-connection-string.md?tabs=net) por encima de las claves de instrumentación. Las nuevas regiones de Azure **requieren** el uso de cadenas de conexión en lugar de claves de instrumentación. La cadena de conexión identifica el recurso con el que se quieren asociar los datos de telemetría. También permite modificar los puntos de conexión que va a usar el recurso como destino de la telemetría. Tiene que copiar la cadena de conexión y agregarla al código de la aplicación o a una variable de entorno.


## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Habilitación de la telemetría de Application Insights del lado servidor (Visual Studio)

En caso de Visual Studio para Mac, use las [instrucciones manuales](#enable-application-insights-server-side-telemetry-no-visual-studio). Solo la versión de Windows de Visual Studio admite este procedimiento.

1. Abra el proyecto en Visual Studio.

    > [!TIP]
    > Para realizar un seguimiento de todos los cambios que Application Insights realiza, puede configurar el control de código fuente para el proyecto. Para realizar esta configuración, seleccione **Archivo** > **Agregar al control de código fuente**.

2. Seleccione **Proyecto** > **Agregar Telemetría de Application Insights**.

3. Seleccione **Comenzar**. Dependiendo de la versión de Visual Studio, el nombre de este botón puede variar. En algunas versiones anteriores, se trataba del botón **Comenzar gratis**.

4. Seleccione la suscripción y luego seleccione **Recurso** > **Registrar**.

5. Después de agregar Application Insights al proyecto, confirme que está usando la versión estable más reciente del SDK. Vaya a **Proyecto** > **Administrar paquetes NuGet** > **Microsoft.ApplicationInsights.AspNetCore**. Si es necesario, seleccione **Actualizar**.

     ![Captura de pantalla que muestra dónde seleccionar el paquete de Application Insights para actualizar](./media/asp-net-core/update-nuget-package.png)

6. Si agregó el proyecto al control de código fuente, vaya a **Ver** > **Team Explorer** > **Cambios**. Puede seleccionar cada archivo para ver una vista de las diferencias de los cambios realizados por la telemetría de Application Insights.

## <a name="enable-application-insights-server-side-telemetry-no-visual-studio"></a>Habilitación de la telemetría de Application Insights del lado servidor (sin Visual Studio)

1. Instale el [SDK de Application Insights desde el paquete de NuGet para ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Se recomienda que use siempre la versión estable más reciente. Consulte las notas de la versión completa para el SDK en el [repositorio de GitHub de código abierto](https://github.com/Microsoft/ApplicationInsights-dotnet/releases).

    En el siguiente ejemplo de código se muestran los cambios que se agregarán al archivo `.csproj` del proyecto.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.16.0" />
        </ItemGroup>
    ```

2. Agregue `services.AddApplicationInsightsTelemetry();` al método `ConfigureServices()` en su clase `Startup`, como en este ejemplo:

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // This code adds other services for your application.
            services.AddMvc();
        }
    ```

3. Comprobar la clave de instrumentación.

    Aunque puede proporcionar la clave de instrumentación como un argumento a `AddApplicationInsightsTelemetry`, se recomienda que especifique la clave de instrumentación en la configuración. En el siguiente ejemplo de código se muestra cómo especificar una clave de instrumentación en `appsettings.json`. Asegúrese de que `appsettings.json` se copia en la carpeta raíz de la aplicación durante la publicación.

    ```json
        {
          "ApplicationInsights": {
            "InstrumentationKey": "putinstrumentationkeyhere"
          },
          "Logging": {
            "LogLevel": {
              "Default": "Warning"
            }
          }
        }
    ```

    Como alternativa, puede especificar la clave de instrumentación en cualquiera de las siguientes variables de entorno:

    * `APPINSIGHTS_INSTRUMENTATIONKEY`

    * `ApplicationInsights:InstrumentationKey`

    Por ejemplo:

    * `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    * `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    * Normalmente, `APPINSIGHTS_INSTRUMENTATIONKEY` se utiliza en [Azure Web Apps](./azure-web-apps.md?tabs=net), pero también se puede usar en todos los lugares donde se admita este SDK. (Si va a realizar la supervisión de aplicaciones web sin código, este formato es necesario si no está usando cadenas de conexión).

    En lugar de establecer las claves de instrumentación, ahora también puede utilizar [Cadenas de conexión](./sdk-connection-string.md?tabs=net).

    > [!NOTE]
    > Una clave de instrumentación especificada en el código prevalece frente a la variable del entorno `APPINSIGHTS_INSTRUMENTATIONKEY`, que prevalece sobre otras opciones.

### <a name="user-secrets-and-other-configuration-providers"></a>Secretos de usuario y otros proveedores de configuración

Si desea almacenar la clave de instrumentación en los secretos de usuario de ASP.NET Core o recuperarla de otro proveedor de configuración, puede usar la sobrecarga con un parámetro `Microsoft.Extensions.Configuration.IConfiguration`. Por ejemplo, `services.AddApplicationInsightsTelemetry(Configuration);`.
A partir de la versión [2.15.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) de Microsoft.ApplicationInsights.AspNetCore, al llamar a `services.AddApplicationInsightsTelemetry()` se lee automáticamente la clave de instrumentación desde `Microsoft.Extensions.Configuration.IConfiguration` de la aplicación. No es necesario proporcionar explícitamente el valor de `IConfiguration`.

## <a name="run-your-application"></a>Ejecución de la aplicación

Ejecute la aplicación y realícele solicitudes. Ahora, la telemetría debería fluir hacia Application Insights. El SDK de Application Insights recopila automáticamente las solicitudes web entrantes en la aplicación, junto con la telemetría siguiente.

### <a name="live-metrics"></a>Live Metrics

[Live Metrics](./live-stream.md) se puede usar para comprobar rápidamente si la supervisión de Application Insights está configurada correctamente. La telemetría puede tardar unos minutos en aparecer en el portal y el análisis, pero Live Metrics muestra el uso de la CPU del proceso en ejecución casi en tiempo real. También puede mostrar otros tipos de telemetría, como las solicitudes, las dependencias y los seguimientos.

### <a name="ilogger-logs"></a>Registros de ILogger

La configuración predeterminada recopila los registros `ILogger` `Warning` y los registros más graves. Puede [Personalizar esta configuración](#how-do-i-customize-ilogger-logs-collection).

### <a name="dependencies"></a>Dependencias

La recopilación de dependencias está habilitada de manera predeterminada. En [este](asp-net-dependencies.md#automatically-tracked-dependencies) artículo se explican las dependencias que se recopilan automáticamente; también contiene pasos para realizar el seguimiento manual.

### <a name="performance-counters"></a>Contadores de rendimiento

La compatibilidad con los [contadores de rendimiento](./performance-counters.md) en ASP.Net Core es limitada:

* Las versiones 2.4.1 y posteriores del SDK recopilan contadores de rendimiento si la aplicación se ejecuta en Azure Web Apps (Windows).
* Las versiones 2.7.1 y posteriores del SDK recopilan contadores de rendimiento si la aplicación se ejecuta en Windows y tiene como destino `NETSTANDARD2.0` o una versión posterior.
* Para las aplicaciones que tienen como destino .NET Framework, todas las versiones del SDK admiten contadores de rendimiento.
* Las versiones 2.8.0 y posteriores del SDK admiten el contador de CPU/memoria de Linux. No se admite ningún otro contador en Linux. La manera recomendada de obtener contadores del sistema en Linux (y otros entornos que no son Windows) es mediante [EventCounters](#eventcounter).

### <a name="eventcounter"></a>EventCounter

`EventCounterCollectionModule` está habilitado de forma predeterminada. Para obtener información sobre cómo configurar la lista de contadores que se recopilarán, consulte [Introducción a EventCounters](eventcounters.md).

## <a name="enable-client-side-telemetry-for-web-applications"></a>Habilitación de la telemetría del lado cliente para aplicaciones web

Los pasos anteriores son suficientes para ayudarle a empezar a recopilar datos de telemetría del lado servidor. Si la aplicación tiene componentes del lado cliente, siga estos pasos para comenzar a recopilar [telemetría de uso](./usage-overview.md).

1. En `_ViewImports.cshtml`, agregue la inserción:

```cshtml
    @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
```

2. En `_Layout.cshtml`, inserte `HtmlHelper` al final de la sección `<head>` pero antes de cualquier otro script. Si quiere informar de cualquier telemetría de JavaScript desde la página, insértela después de este fragmento de código:

```cshtml
    @Html.Raw(JavaScriptSnippet.FullScript)
    </head>
```

Como alternativa al uso de `FullScript`, `ScriptBody` está disponible a partir del SDK de Application Insights para ASP.NET Core, versión 2.14. Úselo si necesita controlar la etiqueta `<script>` para establecer una directiva de seguridad de contenido:

```cshtml
 <script> // apply custom changes to this script tag.
     @Html.Raw(JavaScriptSnippet.ScriptBody)
 </script>
```

Los nombres del archivo `.cshtml` mencionados anteriormente son de una plantilla de la aplicación de MVC predeterminada. En última instancia, si desea habilitar correctamente la supervisión del lado cliente para la aplicación, el fragmento de código de JavaScript debe aparecer en la sección `<head>` de cada página de la aplicación que quiere supervisar. Para hacer esto en esta plantilla de aplicación, agregue el fragmento de código de JavaScript a `_Layout.cshtml`. 

Si el proyecto no incluye `_Layout.cshtml`, aún puede agregar la [supervisión del lado cliente](./website-monitoring.md). Para ello, agregue el fragmento de código de JavaScript a un archivo equivalente que controle el objeto `<head>` de todas las páginas de la aplicación. O bien, puede agregar el fragmento de código en varias páginas, pero esta solución es difícil de mantener y no se recomienda.

> [!NOTE]
> La inserción de JavaScript proporciona una experiencia de configuración predeterminada. Si necesita una [configuración](./javascript.md#configuration) más allá de establecer la clave de instrumentación, debe quitar la inserción automática como se describió anteriormente y agregar manualmente el [SDK de JavaScript](./javascript.md#adding-the-javascript-sdk).

## <a name="configure-the-application-insights-sdk"></a>Configuración del SDK de Application Insights

Puede personalizar el SDK de Application Insights para ASP.NET Core para cambiar la configuración predeterminada. Es posible que los usuarios de los SDK de Application Insights ASP.NET estén familiarizados con el cambio de configuración mediante el uso de `ApplicationInsights.config` o modificando `TelemetryConfiguration.Active`. En ASP.NET Core, realice casi todos los cambios de configuración en el método `ConfigureServices()` de su clase `Startup.cs`, a menos que se indique lo contrario. Para más información, consulte las siguientes secciones.

> [!NOTE]
> En las aplicaciones de ASP.NET Core, no se admite cambiar la configuración modificando `TelemetryConfiguration.Active`.

### <a name="using-applicationinsightsserviceoptions"></a>Uso de ApplicationInsightsServiceOptions

Puede modificar algunos ajustes de configuración comunes pasando de `ApplicationInsightsServiceOptions` a `AddApplicationInsightsTelemetry`, como se muestra en este ejemplo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
    // Disables adaptive sampling.
    aiOptions.EnableAdaptiveSampling = false;

    // Disables QuickPulse (Live Metrics stream).
    aiOptions.EnableQuickPulseMetricStream = false;
    services.AddApplicationInsightsTelemetry(aiOptions);
}
```

Esta tabla contiene la lista completa de valores `ApplicationInsightsServiceOptions`:

|Configuración | Descripción | Valor predeterminado
|---------------|-------|-------
|EnablePerformanceCounterCollectionModule  | Habilitar o deshabilitar `PerformanceCounterCollectionModule` | true
|EnableRequestTrackingTelemetryModule   | Habilitar o deshabilitar `RequestTrackingTelemetryModule` | true
|EnableEventCounterCollectionModule   | Habilitar o deshabilitar `EventCounterCollectionModule` | true
|EnableDependencyTrackingTelemetryModule   | Habilitar o deshabilitar `DependencyTrackingTelemetryModule` | true
|EnableAppServicesHeartbeatTelemetryModule  |  Habilitar o deshabilitar `AppServicesHeartbeatTelemetryModule` | true
|EnableAzureInstanceMetadataTelemetryModule   |  Habilitar o deshabilitar `AzureInstanceMetadataTelemetryModule` | true
|EnableQuickPulseMetricStream | Habilitar o deshabilitar la característica LiveMetrics | true
|EnableAdaptiveSampling | Habilitar o deshabilitar el muestreo adaptable | true
|EnableHeartbeat | Habilitar o deshabilitar la característica de latidos, que envía periódicamente (cada 15 minutos de forma predeterminada) una métrica personalizada denominada "HeartBeatState" con información sobre el entorno de ejecución; por ejemplo, la versión de .NET, información del entorno de Azure, si procede, etc. | true
|AddAutoCollectedMetricExtractor | Habilitar o deshabilitar el extractor de AutoCollectedMetrics, que es un elemento TelemetryProcessor que envía métricas previamente agregadas sobre las solicitudes o dependencias antes de que tenga lugar el muestreo. | true
|RequestCollectionOptions.TrackExceptions | Habilitar o deshabilitar los informes de seguimiento de excepciones no controladas por el módulo de recopilación de solicitudes. | false en NETSTANDARD2.0 (porque se realiza un seguimiento de las excepciones con ApplicationInsightsLoggerProvider); en caso contrario, true.
|EnableDiagnosticsTelemetryModule | Habilitar o deshabilitar `DiagnosticsTelemetryModule`. Si se deshabilita, se omitirá la siguiente configuración: `EnableHeartbeat`, `EnableAzureInstanceMetadataTelemetryModule`, `EnableAppServicesHeartbeatTelemetryModule` | true

Para obtener una lista más actualizada, vea los [valores configurables en `ApplicationInsightsServiceOptions`](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/NETCORE/src/Shared/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="configuration-recommendation-for-microsoftapplicationinsightsaspnetcore-sdk-2150-and-later"></a>Recomendación de configuración para la versión 2.15.0 y posterior del SDK de Microsoft.ApplicationInsights.AspNetCore

En la versión [2.15.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/2.15.0) y posteriores del SDK de Microsoft.ApplicationInsights.AspNetCore, se recomienda configurar cada valor disponible en `ApplicationInsightsServiceOptions`, incluido **InstrumentationKey**, mediante la instancia `IConfiguration` de la aplicación. La configuración debe estar en la sección "ApplicationInsights", como se muestra en el ejemplo siguiente. La siguiente sección de appSettings.json configura la clave de instrumentación y deshabilita el muestreo adaptable y la recopilación de contadores de rendimiento.

```json
{
    "ApplicationInsights": {
    "InstrumentationKey": "putinstrumentationkeyhere",
    "EnableAdaptiveSampling": false,
    "EnablePerformanceCounterCollectionModule": false
    }
}
```

Si se utiliza `services.AddApplicationInsightsTelemetry(aiOptions)`, se invalida la configuración de `Microsoft.Extensions.Configuration.IConfiguration`.

### <a name="sampling"></a>muestreo

El SDK de Application Insights SDK para ASP.NET Core admite el muestreo adaptable y el de tipo fijo. El muestreo adaptable está habilitado de forma predeterminada.

Para obtener más información, consulte [Configuración del muestreo adaptable para aplicaciones ASP.NET Core](./sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications).

### <a name="adding-telemetryinitializers"></a>Adición de TelemetryInitializers

Cuando quiera enriquecer la telemetría con información adicional, use [inicializadores de telemetría](./api-filtering-sampling.md#addmodify-properties-itelemetryinitializer).

Agregue cualquier nuevo objeto `TelemetryInitializer` al contenedor `DependencyInjection` tal como se muestra en el código siguiente. El SDK recoge automáticamente cualquier objeto `TelemetryInitializer` que se agregue al contenedor `DependencyInjection`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
}
```

> [!NOTE]
> `services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();` funciona con inicializadores simples. Para otros, es necesario lo siguiente: `services.AddSingleton(new MyCustomTelemetryInitializer() { fieldName = "myfieldName" });`.
    
### <a name="removing-telemetryinitializers"></a>Eliminación de Telemetryinitializer

Los inicializadores de telemetría están presentes de forma predeterminada. Para quitar todos los inicializadores de telemetría o solo algunos específicos, use el siguiente código de ejemplo *después* de llamar a `AddApplicationInsightsTelemetry()`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry();

    // Remove a specific built-in telemetry initializer
    var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                        (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
    if (tiToRemove != null)
    {
        services.Remove(tiToRemove);
    }

    // Remove all initializers
    // This requires importing namespace by using Microsoft.Extensions.DependencyInjection.Extensions;
    services.RemoveAll(typeof(ITelemetryInitializer));
}
```

### <a name="adding-telemetry-processors"></a>Adición de procesadores de telemetría

Puede agregar procesadores de telemetría personalizados a `TelemetryConfiguration` mediante el método de extensión `AddApplicationInsightsTelemetryProcessor` en `IServiceCollection`. Los procesadores de telemetría se usan en [escenarios de filtrado avanzado](./api-filtering-sampling.md#itelemetryprocessor-and-itelemetryinitializer). Use el ejemplo siguiente.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddApplicationInsightsTelemetry();
    services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

    // If you have more processors:
    services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
}
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Configuración o eliminación de objetos TelemetryModules predeterminados

Application Insights usa módulos de telemetría para recopilar automáticamente información de telemetría útil sobre cargas de trabajo específicas sin necesidad de ninguna configuración adicional.

Los siguientes módulos de recopilación automática están habilitados de forma predeterminada. Estos módulos son responsables de recopilar automáticamente datos de telemetría. Puede deshabilitarlos o configurarlos para modificar su comportamiento predeterminado.

* `RequestTrackingTelemetryModule`: recopila información de RequestTelemetry de las solicitudes web entrantes.
* `DependencyTrackingTelemetryModule`: recopila información de [DependencyTelemetry](./asp-net-dependencies.md) de llamadas HTTP salientes y llamadas SQL.
* `PerformanceCollectorModule`: recopila información de PerformanceCounters de Windows.
* `QuickPulseTelemetryModule`: recopila telemetría para mostrar en el portal de Live Metrics.
* `AppServicesHeartbeatTelemetryModule`: recopila latidos (que se envían como métricas personalizadas) sobre el entorno de Azure App Service donde se hospeda la aplicación.
* `AzureInstanceMetadataTelemetryModule`: recopila latidos (que se envían como métricas personalizadas) sobre el entorno de las máquinas virtuales de Azure donde se hospeda la aplicación.
* `EventCounterCollectionModule`: recopila [EventCounters](eventcounters.md); este módulo es una característica nueva y está disponible en la versión 2.8.0 y versiones posteriores del SDK.

Para configurar cualquier objeto `TelemetryModule` predeterminado, use el método de la extensión `ConfigureTelemetryModule<T>` en `IServiceCollection`, como se muestra en el ejemplo siguiente.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry();

    // The following configures DependencyTrackingTelemetryModule.
    // Similarly, any other default modules can be configured.
    services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
            {
                module.EnableW3CHeadersInjection = true;
            });

    // The following removes all default counters from EventCounterCollectionModule, and adds a single one.
    services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );

    // The following removes PerformanceCollectorModule to disable perf-counter collection.
    // Similarly, any other default modules can be removed.
    var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
    if (performanceCounterService != null)
    {
        services.Remove(performanceCounterService);
    }
}
```

En las versiones 2.12.2 y posteriores, [`ApplicationInsightsServiceOptions`](#using-applicationinsightsserviceoptions) incluye una opción sencilla para deshabilitar cualquiera de los módulos predeterminados.

### <a name="configuring-a-telemetry-channel"></a>Configuración de un canal de telemetría

El [Canal de telemetría](./telemetry-channels.md) predeterminado es `ServerTelemetryChannel`. El ejemplo siguiente muestra cómo reemplazarlo.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="disable-telemetry-dynamically"></a>Deshabilitar la telemetría dinámicamente

Si desea deshabilitar la telemetría condicional y dinámicamente, puede resolver la instancia `TelemetryConfiguration` con el contenedor de inyección de dependencias de ASP.NET Core en cualquier parte del código y establecer `DisableTelemetry` como marca.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
    {
        configuration.DisableTelemetry = true;
        ...
    }
```

El ejemplo de código anterior impide el envío de telemetría a Application Insights. No impide que los módulos de recopilación automática recopilen la telemetría. Si quiere quitar un módulo de recopilación automática particular, vea [Quitar el módulo de telemetría](#configuring-or-removing-default-telemetrymodules).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="does-application-insights-support-aspnet-core-3x"></a>¿Admite Application Insights ASP.NET Core 3.X?

Sí. Actualice a la versión 2.8.0 o posterior del [SDK de Application Insights para ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Las versiones anteriores del SDK no admiten ASP.NET Core 3.X.

Además, si va a [habilitar la telemetría del servidor basada en Visual Studio](#enable-application-insights-server-side-telemetry-visual-studio), actualice a la última versión de Visual Studio 2019 (16.3.0) para la incorporación. Las versiones anteriores de Visual Studio no admiten la incorporación automática de aplicaciones ASP.NET Core 3.X.

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>¿Cómo puedo realizar un seguimiento de la telemetría que se recopila automáticamente?

Obtenga una instancia de `TelemetryClient` mediante el uso de la inserción del constructor y llame al método `TrackXXX()` necesario que incluye. No se recomienda crear nuevas instancias `TelemetryClient` o `TelemetryConfiguration` en una aplicación de ASP.NET Core. Una instancia singleton de `TelemetryClient` ya está registrada en el contenedor `DependencyInjection`, que comparte `TelemetryConfiguration` con el resto de los datos de telemetría. Se recomienda crear una nueva instancia `TelemetryClient` solo si se necesita una configuración independiente del resto de los datos de telemetría.

En el ejemplo siguiente se muestra cómo realizar el seguimiento de los datos de telemetría adicionales desde un controlador.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // Use constructor injection to get a TelemetryClient instance.
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // Call the required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

Para obtener más información acerca de los datos personalizados que reporta Application Insights, consulte [API de Application Insights para eventos y métricas personalizados](./api-custom-events-metrics.md). Se puede usar un enfoque similar para el envío de métricas personalizadas a Application Insights mediante la [API de GetMetric](./get-metric.md).

### <a name="how-do-i-customize-ilogger-logs-collection"></a>¿Cómo puedo personalizar la colección de registros de ILogger?

De forma predeterminada, solo se capturan automáticamente los registros `Warning` y los registros más graves. Para cambiar este comportamiento,debe invalidar explícitamente la configuración de registro del proveedor `ApplicationInsights` tal como se muestra a continuación.
La siguiente configuración permite que Application Insights capture todos los registros `Information` y los registros más graves.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    },
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Information"
      }
    }
  }
}
```

Es importante tener en cuenta que lo siguiente no hará que el proveedor de Application Insights capture los registros `Information`. No lo captura porque el SDK agrega un filtro de registro predeterminado que indica a `ApplicationInsights` que capture solo los registros `Warning` y los registros más graves. Application Insights requiere una invalidación explícita.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

Para más información, consulte [Configuración de ILogger](ilogger.md#logging-level).

### <a name="some-visual-studio-templates-used-the-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Algunas plantillas de Visual Studio usan el método de extensión UseApplicationInsights() en IWebHostBuilder para habilitar Application Insights. ¿Es este uso aún válido?

El método de extensión `UseApplicationInsights()` todavía es compatible, pero está marcado como obsoleto desde la versión 2.8.0 y posteriores del SDK de Application Insights. Se quitará en el siguiente cambio de versión principal del SDK. Para habilitar la telemetría de Application Insights, se recomienda usar `AddApplicationInsightsTelemetry()`, porque proporciona sobrecargas para controlar configuración. Además, en las aplicaciones ASP.NET Core 3.X, `services.AddApplicationInsightsTelemetry()` es la única manera de habilitar Application Insights.

### <a name="im-deploying-my-aspnet-core-application-to-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Estoy implementando mi aplicación de ASP.NET Core en Web Apps. ¿Debo habilitar la extensión de Application Insights desde Web Apps?

Si está instalado el SDK en tiempo de compilación, como se muestra en este artículo, no es necesario habilitar la [extensión de Application Insights](./azure-web-apps.md) desde el portal de App Service. Incluso si la extensión está instalada, se retirará cuando detecte que el SDK ya está agregado en la aplicación. Si habilita Application Insights desde la extensión, no tiene que instalar ni actualizar el SDK. Pero si habilita Application Insights siguiendo las instrucciones de este artículo, tendrá más flexibilidad porque:

   * La telemetría de Application Insights seguirá funcionando en:
       * Todos los sistemas operativos, incluidos Windows, Linux y Mac.
       * Todos los modos de publicación, incluidos los independientes o dependientes del marco.
       * Todas las plataformas de destino, incluida la versión completa de .NET Framework.
       * Todas las opciones de hospedaje, incluidos contenedores, Web Apps, VM, Linux, Azure Kubernetes Service y hospedaje independiente de Azure.
       * Todas las versiones de .NET Core, incluidas las versiones preliminares.
   * Puede ver datos de telemetría localmente cuando depure desde Visual Studio.
   * Puede realizar un seguimiento de telemetría personalizada adicional mediante la API `TrackXXX()`.
   * Tiene control total sobre la configuración.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-azure-monitor-application-insights-agent-formerly-status-monitor-v2"></a>¿Puedo habilitar la supervisión de Application Insights mediante herramientas como el agente de Application Insights para Azure Monitor (anteriormente, Monitor de estado v2)?

 Sí. A partir de [Application Insights Agent 2.0.0-beta1](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/2.0.0-beta1), se admiten aplicaciones ASP.NET Core hospedadas en IIS.

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Si ejecuto mi aplicación en Linux, ¿se admiten todas las características?

Sí. La compatibilidad de características para el SDK es la misma en todas las plataformas, con las siguientes excepciones:

* El SDK recopila [contadores de eventos](./eventcounters.md) en Linux, ya que los [contadores de rendimiento](./performance-counters.md) solo se admiten en Windows. La mayoría de las métricas son las mismas.
* Aunque el objeto `ServerTelemetryChannel` está habilitado de forma predeterminada, si la aplicación se ejecuta en Linux o macOS, el canal no crea automáticamente una carpeta de almacenamiento local para mantener los datos de telemetría temporalmente si hay problemas de red. Debido a esta limitación, la telemetría se pierde cuando hay problemas temporales de red o del servidor. Para solucionar este problema, configure una carpeta local para el canal:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure the channel to use the given folder to temporarily
        // store telemetry items during network or Application Insights server issues.
        // User should ensure that the given folder already exists
        // and that the application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```

Esta limitación no es aplicable en la versión [2.15.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/2.15.0) y posteriores.

### <a name="is-this-sdk-supported-for-the-new-net-core-3x-worker-service-template-applications"></a>¿Es este SDK compatible con las nuevas aplicaciones de plantilla de servicio de trabajo de .NET Core 3.X?

Este SDK requiere `HttpContext`; por lo tanto, no funciona en ninguna aplicación sin HTTP, incluidas las aplicaciones de servicio de trabajo de .NET Core 3.X. Para habilitar Application Insights en estas aplicaciones mediante el SDK de Microsoft.ApplicationInsights.WorkerService recién publicado, consulte [Application Insights para las aplicaciones de servicio de trabajo (aplicaciones sin HTTP)](worker-service.md).

## <a name="open-source-sdk"></a>SDK de código abierto

* [Lectura y contribución al código](https://github.com/microsoft/ApplicationInsights-dotnet).

Para obtener las actualizaciones y correcciones de errores más recientes, vea las [notas de la versión](./release-notes.md).

## <a name="next-steps"></a>Pasos siguientes

* [Explore los flujos de usuarios](./usage-flows.md) para saber cómo navegan por la aplicación.
* [Configure una colección de instantáneas](./snapshot-debugger.md) para ver el estado del código fuente y las variables en el momento en que se produzca una excepción.
* [Use la API](./api-custom-events-metrics.md) para enviar sus propios eventos y métricas para obtener una vista detallada del rendimiento y del uso de la aplicación.
* [Las pruebas de disponibilidad](./monitor-web-app-availability.md) comprueban la aplicación constantemente desde cualquier parte del mundo.
* [Inserción de dependencias en ASP.NET Core](/aspnet/core/fundamentals/dependency-injection)
