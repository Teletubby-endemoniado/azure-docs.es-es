---
title: Supervisar el rendimiento de Azure App Services | Microsoft Docs
description: Supervisión del rendimiento de aplicaciones de Azure App Services. Carga y tiempo de respuesta de gráfico, información de dependencia y establecer alertas en el rendimiento.
ms.topic: conceptual
ms.date: 08/05/2021
ms.custom: devx-track-js, devx-track-dotnet, devx-track-azurepowershell
ms.openlocfilehash: 9fac3e79b62a1f354b4120afc965e331b0185e98
ms.sourcegitcommit: 1deb51bc3de58afdd9871bc7d2558ee5916a3e89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2021
ms.locfileid: "122428628"
---
# <a name="application-monitoring-for-azure-app-service"></a>Supervisión de aplicaciones para Azure App Service

Ahora, habilitar la supervisión en las aplicaciones web basadas en ASP.NET, ASP.NET Core, Java y Node.js que se ejecutan en [Azure App Services](../../app-service/index.yml) es más fácil que nunca. Mientras que antes era necesario instrumentar manualmente la aplicación, ahora el agente o la extensión más reciente están integrados en la imagen de App Service de manera predeterminada. En este artículo le guiaremos a través de la habilitación de la supervisión de Application Insights para Azure Monitor y proporcionaremos instrucciones preliminares para automatizar el proceso para implementaciones a gran escala.

> [!NOTE]
> Solo para .NET en Windows: la incorporación manual de una extensión de sitio de Application Insights mediante **Herramientas de desarrollo** > **Extensiones** está en desuso. Este método de instalación de extensiones dependía de actualizaciones manuales para cada nueva versión. La versión estable más reciente de la extensión ahora viene [preinstalada](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions) como parte de la imagen de App Service. Los archivos se encuentran en `d:\Program Files (x86)\SiteExtensions\ApplicationInsightsAgent` y se actualizan automáticamente con cada versión estable. Si sigue las instrucciones basadas en agentes para habilitar la supervisión a continuación, se quita automáticamente la extensión en desuso.

## <a name="enable-application-insights"></a>Habilitación de Application Insights

Hay dos maneras de habilitar la supervisión de aplicaciones para las aplicaciones hospedadas de Azure App Service:

* **Supervisión de aplicaciones basadas en agentes** (ApplicationInsightsAgent).  
    * Este método es el más fácil para habilitar y no se requiere ningún cambio de código ni configuraciones avanzadas. A menudo se conoce como supervisión de "entorno en tiempo de ejecución". Para Azure App Services se recomienda la habilitación de este nivel de supervisión como mínimo y, a continuación, en función del escenario específico, puede evaluar si es necesaria una supervisión más avanzada a través de instrumentación manual.

* **Instrumentación manual de la aplicación mediante código** mediante la instalación del SDK de Application Insights.

    * Este enfoque es más fácil de personalizar, pero exige los siguientes métodos: SDK de [.NET Core](./asp-net-core.md), [.NET](./asp-net.md), [Node.js](./nodejs.md), [Python](./opencensus-python.md), y un agente independiente para [Java](./java-in-process-agent.md). Este método también implica que el usuario tiene que administrar las actualizaciones a la versión más reciente de los paquetes.

    * Si necesita realizar llamadas de API personalizadas para supervisar eventos o dependencias no capturados de manera predeterminada con la supervisión basada en agentes, deberá usar este método. Consulte el [artículo API de Application Insights para eventos y métricas personalizados](./api-custom-events-metrics.md) para obtener más información. 

> [!NOTE]
> Si se detecta supervisión basada en agentes e instrumentación manual basada en SDK, en .NET solo se respeta la configuración de instrumentación manual, mientras que en Java solo la instrumentación basada en agentes emite la telemetría. Esto es para evitar que se envíen datos duplicados. Para más información sobre este tema, consulte la [sección de solución de problemas](#troubleshooting) que encontrará a continuación.

> [!NOTE]
> El depurador de instantáneas y el generador de perfiles solo están disponibles en .NET y .Net Core.

## <a name="enable-agent-based-monitoring"></a>Habilitación de la supervisión basada en agente

# <a name="aspnet"></a>[ASP.NET](#tab/net)

> [!NOTE]
> No se admite la combinación de APPINSIGHTS_JAVASCRIPT_ENABLED y urlCompression. Para más información, consulte la explicación de la [sección Solución de problemas](#appinsights_javascript_enabled-and-urlcompression-is-not-supported).

1. **Seleccione Application Insights** en el panel de control de Azure para el servicio de aplicaciones.

    ![En Configuración, elija Application Insights](./media/azure-web-apps/settings-app-insights-01.png)

   * Cree un nuevo recurso o seleccione un recurso de Application Insights existente para esta aplicación. 

    > [!NOTE]
    > Al hacer clic en **Aceptar** para crear el nuevo recurso, se le pedirá **Aplicar la configuración de supervisión**. Con la selección de **Continuar** se vinculará el nuevo recurso de Application Insights a su servicio de aplicaciones. Al hacerlo, también se **desencadenará un reinicio del servicio de aplicaciones**. 

    >[!div class="mx-imgBorder"]
    >![Instrumentación de la aplicación web](./media/azure-web-apps/ai-create-new.png)

2. Después de especificar qué recurso se debe usar, puede elegir cómo quiere que Application Insights recopile los datos de cada plataforma para la aplicación. La supervisión de aplicaciones ASP.NET está activada de manera predeterminada con dos niveles diferentes de la colección.

    ![Captura de pantalla que muestra la página de extensiones de sitio de Application Insights con la opción Crear recurso seleccionada.](./media/azure-web-apps/choose-options-new.png)
 
 A continuación se muestra un resumen de los datos recopilados para cada ruta:
        
| data | Recopilación básica de ASP.NET | Recopilación recomendada de ASP.NET |
| --- | --- | --- |
| Agrega las tendencias de uso de CPU, memoria y E/S. |Sí |Sí |
| Recopila las tendencias de uso y habilita la correlación entre los resultados de disponibilidad y las transacciones. | Sí |Sí |
| Recopila las excepciones no controladas por el proceso de host. | Sí |Sí |
| Mejora la precisión de las métricas de APM con carga, cuando se usa el muestreo. | Sí |Sí |
| Correlaciona los microservicios entre los límites de solicitud y dependencia. | No (solo funcionalidades de APM de instancia única) |Sí |

3. Para configurar valores como el muestreo, que anteriormente se podía controlar mediante el archivo applicationinsights.config, ahora puede interactuar con esos mismos valores a través de la configuración de la aplicación con un prefijo correspondiente. 

    * Por ejemplo, para cambiar el porcentaje de muestreo inicial, puede crear una configuración de la aplicación de `MicrosoftAppInsights_AdaptiveSamplingTelemetryProcessor_InitialSamplingPercentage` y un valor de `100`.

    * Para obtener la lista de valores de configuración de procesador de telemetría de muestreo adaptable compatibles, puede consultar el [código](https://github.com/microsoft/ApplicationInsights-dotnet/blob/master/BASE/Test/ServerTelemetryChannel.Test/TelemetryChannel.Tests/AdaptiveSamplingTelemetryProcessorTest.cs) y la [documentación asociada](./sampling.md).

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/netcore)

### <a name="windows"></a>Windows
> [!IMPORTANT]
> Se admiten las siguientes versiones de ASP.NET Core para la instrumentación automática en Windows: ASP.NET Core 3.1 y 5.0. Las versiones 2.0, 2.1, 2.2 y 3.0 se han retirado y ya no se admiten. Actualice a una [versión compatible](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) de .NET Core para que la instrumentación automática funcione.


En Windows **no se admite** el establecimiento como destino del marco completo desde ASP.NET Core. En su lugar, use [instrumentación manual](./asp-net-core.md) por medio de código.

En Windows, solo se admite la implementación dependiente del marco.

Vea la sección [Habilitar supervisión](#enable-monitoring ) a continuación para empezar a configurar Application Insights con el recurso de App Service. 

### <a name="linux"></a>Linux 

> [!IMPORTANT]
> Solo se admite ASP.NET Core 6.0 (versión preliminar) para la instrumentación automática en Linux.

> [!NOTE]
> La instrumentación automática de Linux del portal de App Services está en versión preliminar pública. Estas versiones preliminares se proporcionan sin contrato de nivel de servicio. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.

En Linux **no se admite** el establecimiento como destino del marco completo desde ASP.NET Core. En su lugar, use [instrumentación manual](./asp-net-core.md) por medio de código.

 En Linux, se admiten la implementación dependiente del marco y la implementación autocontenida. 

Vea la sección [Habilitar supervisión](#enable-monitoring ) a continuación para empezar a configurar Application Insights con el recurso de App Service. 

### <a name="enable-monitoring"></a>Habilitar supervisión 

1. **Seleccione Application Insights** en el panel de control de Azure para el servicio de aplicaciones.

    ![En Configuración, elija Application Insights](./media/azure-web-apps/settings-app-insights-01.png)

   * Cree un nuevo recurso o seleccione un recurso de Application Insights existente para esta aplicación.

    > [!NOTE]
    > Al hacer clic en **Aceptar** para crear el nuevo recurso, se le pedirá **Aplicar la configuración de supervisión**. Con la selección de **Continuar** se vinculará el nuevo recurso de Application Insights a su servicio de aplicaciones. Al hacerlo, también se **desencadenará un reinicio del servicio de aplicaciones**. 

    >[!div class="mx-imgBorder"]
    >![Instrumentación de la aplicación web](./media/azure-web-apps/ai-create-new.png)

2. Después de especificar qué recurso se debe usar, puede elegir cómo quiere que Application Insights recopile los datos de cada plataforma para la aplicación. ASP.NET Core ofrece **recopilación recomendada** o **deshabilitada** para ASP.NET Core 3.1.

    ![Opciones de elección para cada plataforma.](./media/azure-web-apps/choose-options-new-net-core.png)




# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Puede supervisar las aplicaciones de Node.js que se ejecutan en Azure App Service sin ningún cambio de código, solo con un par de pasos sencillos. Las aplicaciones de Application Insights para Node.js se integran con App Service en Linux, en los contenedores basados en código y los personalizados, y con App Service en Windows para las aplicaciones basadas en código.

1. **Seleccione Application Insights** en el panel de control de Azure para el servicio de aplicaciones.

    > [!div class="mx-imgBorder"]
    > ![En Configuración, elija Application Insights.](./media/azure-web-apps/ai-enable.png)

   * Elija crear un nuevo recurso a menos que ya haya configurado un recurso de Application Insights para esta aplicación. 

    > [!NOTE]
    > Al hacer clic en **Aceptar** para crear el nuevo recurso, se le pedirá **Aplicar la configuración de supervisión**. Con la selección de **Continuar** se vinculará el nuevo recurso de Application Insights a su servicio de aplicaciones. Al hacerlo, también se **desencadenará un reinicio del servicio de aplicaciones**. 

    >[!div class="mx-imgBorder"]
    >![Instrumente la aplicación web.](./media/azure-web-apps/ai-create-new.png)

2. Una vez que haya especificado qué recurso usar ya está listo para continuar. 

    > [!div class="mx-imgBorder"]
    > ![Opciones de elección para cada plataforma.](./media/azure-web-apps/app-service-node.png)



# <a name="java"></a>[Java](#tab/java)

Puede activar la supervisión de las aplicaciones Java que se ejecutan en Azure App Service con un solo clic, no se requiere ningún cambio de código. Application Insights para Java se integra con App Service en Linux, en los contenedores basados en código y los personalizados, y con App Service en Windows para las aplicaciones basadas en código. Es importante saber cómo se supervisará la aplicación. La integración agrega [Application Insights Java 3.x](./java-in-process-agent.md), y se recibe toda la telemetría que recopila automáticamente.

1. **Seleccione Application Insights** en el panel de control de Azure para el servicio de aplicaciones.

    > [!div class="mx-imgBorder"]
    > ![En Configuración, elija Application Insights.](./media/azure-web-apps/ai-enable.png)

   * Cree un nuevo recurso o seleccione un recurso de Application Insights existente para esta aplicación.

    > [!NOTE]
    > Al hacer clic en **Aceptar** para crear el nuevo recurso, se le pedirá **Aplicar la configuración de supervisión**. Con la selección de **Continuar** se vinculará el nuevo recurso de Application Insights a su servicio de aplicaciones. Al hacerlo, también se **desencadenará un reinicio del servicio de aplicaciones**. 

    >[!div class="mx-imgBorder"]
    >![Instrumente la aplicación web.](./media/azure-web-apps/ai-create-new.png)

2. Este paso no es necesario. Después de especificar qué recurso usar, puede configurar el agente de Java. Si no configura el agente de Java, se aplican las configuraciones predeterminadas. El [conjunto de configuraciones](./java-standalone-config.md) completo está disponible, solo tiene que pegar un archivo JSON válido. Excluya la cadena de conexión y las configuraciones que se encuentran en versión preliminar; puede agregarlos a medida que estén disponibles con carácter general.

    > [!div class="mx-imgBorder"]
    > ![Opciones de elección para cada plataforma.](./media/azure-web-apps/create-app-service-ai.png)

# <a name="python"></a>[Python](#tab/python)

Las aplicaciones web de App Service basadas en Python no admiten actualmente la supervisión automática basada en agente ni extensión. Para habilitar la supervisión de la aplicación Python, debe [instrumentar manualmente la aplicación](./opencensus-python.md).

---

## <a name="enable-client-side-monitoring"></a>Habilitación de la supervisión del lado cliente

# <a name="aspnet"></a>[ASP.NET](#tab/net)

La supervisión de cliente está habilitada en ASP.NET. Para habilitar la supervisión de cliente:

* **Parámetros** **>** **Configuración**
   * En Configuración de la aplicación, cree una **nueva configuración de la aplicación**:

     Nombre: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Valor: `true`

   * **Guarde** la configuración y **reinicie** la aplicación.

Para deshabilitar la supervisión de cliente, quite el par clave-valor asociado de la configuración de la aplicación o establezca el valor en false.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/netcore)

La supervisión de cliente está **habilitada de manera predeterminada** para las aplicaciones de ASP.NET Core con la **recopilación recomendada**, independientemente de si está presente el valor "APPINSIGHTS_JAVASCRIPT_ENABLED" de la aplicación.

Si por algún motivo quiere deshabilitar la supervisión de cliente:

* **Parámetros** **>** **Configuración**
   * En Configuración de la aplicación, cree una **nueva configuración de la aplicación**:

     Nombre: `APPINSIGHTS_JAVASCRIPT_ENABLED`

     Valor: `false`

   * **Guarde** la configuración y **reinicie** la aplicación.

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Para habilitar la supervisión del lado cliente para la aplicación de Node.js, debe [agregar manualmente el SDK de JavaScript del lado cliente a la aplicación](./javascript.md).

# <a name="java"></a>[Java](#tab/java)

Para habilitar la supervisión del lado cliente para la aplicación Java, debe [agregar manualmente el SDK de JavaScript del lado cliente a la aplicación](./javascript.md).

# <a name="python"></a>[Python](#tab/python)

Para habilitar la supervisión del lado cliente para la aplicación Python, debe [agregar manualmente el SDK de JavaScript del lado cliente a la aplicación](./javascript.md).

---

## <a name="automate-monitoring"></a>Automatización de la supervisión

Con el fin de habilitar la recopilación de datos de telemetría con Application Insights, la configuración de la aplicación deberá configurarse:

   ![Configuración de la aplicación de App Service con la configuración de Application Insights disponible](./media/azure-web-apps/application-settings.png)

### <a name="application-settings-definitions"></a>Definiciones de los valores de configuración de la aplicación

|Nombre del valor de configuración de la aplicación |  Definición | Value |
|-----------------|:------------|-------------:|
|ApplicationInsightsAgent_EXTENSION_VERSION | Extensión principal; controla la supervisión en el entorno en tiempo de ejecución. | `~2` para Windows o `~3` para Linux |
|XDT_MicrosoftApplicationInsights_Mode |  En el modo predeterminado, solo están habilitadas las características esenciales para garantizar un rendimiento óptimo. | `default` o `recommended`. |
|InstrumentationEngine_EXTENSION_VERSION | Controla si se activa el motor `InstrumentationEngine` de reescritura binaria. Esta configuración tiene implicaciones de rendimiento y afecta a la hora de inicio o al arranque en frío. | `~1` |
|XDT_MicrosoftApplicationInsights_BaseExtensions | Controla si el texto de la tabla SQL y Azure se captura junto con las llamadas de dependencia. Advertencia de rendimiento: el tiempo de inicio en frío de la aplicación se verá afectado. Esta configuración requiere el elemento `InstrumentationEngine`. | `~1` |
|XDT_MicrosoftApplicationInsights_PreemptSdk | Solo para aplicaciones de ASP.NET Core. Habilita la interoperabilidad con el SDK de Application Insights. Carga la extensión en paralelo con el SDK y la usa para enviar telemetría (deshabilita el SDK de Application Insights). |`1`|

### <a name="app-service-application-settings-with-azure-resource-manager"></a>Configuración de la aplicación de App Service con Azure Resource Manager

La configuración de la aplicación de App Service se puede administrar y configurar con [plantillas de Azure Resource Manager](../../azure-resource-manager/templates/syntax.md). Este método puede usarse al implementar nuevos recursos de App Service con la automatización de Azure Resource Manager o para modificar la configuración de los recursos existentes.

La estructura básica de JSON de la configuración de la aplicación para App Service se muestra a continuación:

```JSON
      "resources": [
        {
          "name": "appsettings",
          "type": "config",
          "apiVersion": "2015-08-01",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites', variables('webSiteName'))]"
          ],
          "tags": {
            "displayName": "Application Insights Settings"
          },
          "properties": {
            "key1": "value1",
            "key2": "value2"
          }
        }
      ]
```

Para obtener un ejemplo de una plantilla de Azure Resource Manager con la configuración de la aplicación establecida para Application Insights, esta [plantilla](https://github.com/Andrew-MSFT/BasicImageGallery) puede resultar útil, sobre todo en la sección que empieza a partir de la [línea 238](https://github.com/Andrew-MSFT/BasicImageGallery/blob/c55ada54519e13ce2559823c16ca4f97ddc5c7a4/CoreImageGallery/Deploy/CoreImageGalleryARM/azuredeploy.json#L238).

### <a name="automate-the-creation-of-an-application-insights-resource-and-link-to-your-newly-created-app-service"></a>Automatización de la creación de un recurso de Application Insights y vínculo a la instancia de App Service recién creada

Para crear una plantilla de Azure Resource Manager con toda la configuración predeterminada de Application Insights establecida, comience el proceso como si fuese a crear una nueva aplicación web con Application Insights habilitado.

Seleccione **Opciones de automatización**.

   ![Menú de creación de aplicación web de App Service](./media/azure-web-apps/create-web-app.png)

Esta opción genera la plantilla de Azure Resource Manager más reciente con toda la configuración necesaria establecida.

  ![Plantilla de aplicación web de App Service](./media/azure-web-apps/arm-template.png)

A continuación, se muestra un ejemplo; reemplace todas las instancias de `AppMonitoredSite` por el nombre del sitio:

```json
{
    "resources": [
        {
            "name": "[parameters('name')]",
            "type": "Microsoft.Web/sites",
            "properties": {
                "siteConfig": {
                    "appSettings": [
                        {
                            "name": "APPINSIGHTS_INSTRUMENTATIONKEY",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').InstrumentationKey]"
                        },
                        {
                            "name": "APPLICATIONINSIGHTS_CONNECTION_STRING",
                            "value": "[reference('microsoft.insights/components/AppMonitoredSite', '2015-05-01').ConnectionString]"
                        },
                        {
                            "name": "ApplicationInsightsAgent_EXTENSION_VERSION",
                            "value": "~2"
                        }
                    ]
                },
                "name": "[parameters('name')]",
                "serverFarmId": "[concat('/subscriptions/', parameters('subscriptionId'),'/resourcegroups/', parameters('serverFarmResourceGroup'), '/providers/Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Web/serverfarms/', parameters('hostingPlanName'))]",
                "microsoft.insights/components/AppMonitoredSite"
            ],
            "apiVersion": "2016-03-01",
            "location": "[parameters('location')]"
        },
        {
            "apiVersion": "2016-09-01",
            "name": "[parameters('hostingPlanName')]",
            "type": "Microsoft.Web/serverfarms",
            "location": "[parameters('location')]",
            "properties": {
                "name": "[parameters('hostingPlanName')]",
                "workerSizeId": "[parameters('workerSize')]",
                "numberOfWorkers": "1",
                "hostingEnvironment": "[parameters('hostingEnvironment')]"
            },
            "sku": {
                "Tier": "[parameters('sku')]",
                "Name": "[parameters('skuCode')]"
            }
        },
        {
            "apiVersion": "2015-05-01",
            "name": "AppMonitoredSite",
            "type": "microsoft.insights/components",
            "location": "West US 2",
            "properties": {
                "ApplicationId": "[parameters('name')]",
                "Request_Source": "IbizaWebAppExtensionCreate"
            }
        }
    ],
    "parameters": {
        "name": {
            "type": "string"
        },
        "hostingPlanName": {
            "type": "string"
        },
        "hostingEnvironment": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "sku": {
            "type": "string"
        },
        "skuCode": {
            "type": "string"
        },
        "workerSize": {
            "type": "string"
        },
        "serverFarmResourceGroup": {
            "type": "string"
        },
        "subscriptionId": {
            "type": "string"
        }
    },
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0"
}
```

### <a name="enabling-through-powershell"></a>Habilitación a través de PowerShell

Para habilitar la supervisión de la aplicación a través de PowerShell, solo se debe cambiar la configuración de la aplicación subyacente. A continuación, se muestra un ejemplo que habilita la supervisión de la aplicación para un sitio web denominado "AppMonitoredSite" en el grupo de recursos "AppMonitoredRG" y configura los datos para que se envíen a la clave de instrumentación "012345678-abcd-ef01-2345-6789abcd".

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```powershell
$app = Get-AzWebApp -ResourceGroupName "AppMonitoredRG" -Name "AppMonitoredSite" -ErrorAction Stop
$newAppSettings = @{} # case-insensitive hash map
$app.SiteConfig.AppSettings | %{$newAppSettings[$_.Name] = $_.Value} # preserve non Application Insights application settings.
$newAppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"] = "012345678-abcd-ef01-2345-6789abcd"; # set the Application Insights instrumentation key
$newAppSettings["APPLICATIONINSIGHTS_CONNECTION_STRING"] = "InstrumentationKey=012345678-abcd-ef01-2345-6789abcd"; # set the Application Insights connection string
$newAppSettings["ApplicationInsightsAgent_EXTENSION_VERSION"] = "~2"; # enable the ApplicationInsightsAgent
$app = Set-AzWebApp -AppSettings $newAppSettings -ResourceGroupName $app.ResourceGroup -Name $app.Name -ErrorAction Stop
```

## <a name="upgrade-monitoring-extensionagent---net"></a>Actualización del agente o la extensión de supervisión: .NET 

### <a name="upgrading-from-versions-289-and-up"></a>Actualización de las versiones 2.8.9 y posteriores

La actualización de la versión 2.8.9 se realiza automáticamente, sin acciones adicionales. Los nuevos bits de supervisión se proporcionan en segundo plano para el servicio de aplicación de destino y se recogen en el reinicio de la aplicación.

Para comprobar qué versión de la extensión se ejecuta, vaya a `https://yoursitename.scm.azurewebsites.net/ApplicationInsights`.

![Captura de pantalla de la ruta de acceso de URL para comprobar la versión de la extensión que está ejecutando](./media/azure-web-apps/extension-version.png)

### <a name="upgrade-from-versions-100---265"></a>Actualización de las versiones 1.0.0 a 2.6.5

A partir de la versión 2.8.9 se usa la extensión de sitio preinstalada. Si tiene una versión anterior, puede actualizarla de dos maneras:

* [Actualización mediante la habilitación a través del portal](#enable-application-insights). (Incluso si tiene la extensión Application Insights para Azure App Service instalada, la interfaz de usuario solo muestra el botón **Habilitar**. En segundo plano, la anterior extensión de sitio privada se quitará).

* [Actualización a través de PowerShell](#enabling-through-powershell):

    1. Establezca la configuración de la aplicación para habilitar la extensión de sitio preinstalada ApplicationInsightsAgent. Consulte [Habilitación a través de PowerShell](#enabling-through-powershell).
    2. Quite manualmente la extensión de sitio privada denominada extensión de Application Insights para Azure App Service.

Si se realiza la actualización desde una versión anterior a la 2.5.1, compruebe que los archivos DLL de ApplicationInsigths se quitan de la carpeta de la ubicación de la aplicación. [Vea los pasos para la solución de problemas](#troubleshooting).

## <a name="troubleshooting"></a>Solución de problemas

### <a name="aspnet-and-aspnet-core"></a>ASP.NET y ASP.NET CORE

A continuación, se muestra nuestra guía paso a paso de solución de problemas para la supervisión basada en extensiones o agentes para aplicaciones basadas en ASP.NET y ASP.NET Core que se ejecutan en Azure App Services.

#### <a name="windows-troubleshooting"></a>Solución de problemas de Windows 
1. Compruebe que la configuración de la aplicación `ApplicationInsightsAgent_EXTENSION_VERSION` se establece en un valor de "~2".
2. Vaya a `https://yoursitename.scm.azurewebsites.net/ApplicationInsights`.  

    ![Captura de pantalla de la página de resultados de https://yoursitename.scm.azurewebsites/applicationinsights](./media/azure-web-apps/app-insights-sdk-status.png)
    
    - Confirme que el `Application Insights Extension Status` es `Pre-Installed Site Extension, version 2.8.x.xxxx, is running.`. 
    
         Si no se está ejecutando, siga las [instrucciones para habilitar la supervisión de Application Insights](#enable-application-insights).

    - Confirme que el origen de estado existe y tiene el siguiente aspecto: `Status source D:\home\LogFiles\ApplicationInsights\status\status_RD0003FF0317B6_4248_1.json`.

         Si no está presente un valor similar, significa que la aplicación no se admite o no se está ejecutando actualmente. Para asegurarse de que la aplicación se está ejecutando, intente visitar manualmente los puntos de conexión de la aplicación o la dirección URL de la aplicación, lo que permitirá que la información del entorno en tiempo de ejecución esté disponible.

    - Confirme que `IKeyExists` es `true`. Si es `false`, agregue `APPINSIGHTS_INSTRUMENTATIONKEY` y `APPLICATIONINSIGHTS_CONNECTION_STRING` con el GUID de ikey a la configuración de la aplicación.

    - **Solo para aplicaciones de ASP.NET** confirme que no hay ninguna entrada para `AppAlreadyInstrumented`, `AppContainsDiagnosticSourceAssembly` y `AppContainsAspNetTelemetryCorrelationAssembly`.

         Si existe alguna de estas entradas, quite los siguientes paquetes de la aplicación: `Microsoft.ApplicationInsights`, `System.Diagnostics.DiagnosticSource` y `Microsoft.AspNet.TelemetryCorrelation`.

    - **Solo para aplicaciones de ASP.NET Core**: en caso de que la aplicación haga referencia a algún paquete de Application Insights, por ejemplo, si ya ha instrumentado (o ha intentado instrumentar) la aplicación con el [SDK de ASP.NET Core](./asp-net-core.md), es posible que la integración de App Service no surta efecto y que los datos no aparezcan en Application Insights. Para solucionar el problema, en el portal, active "Interop with Application Insights SDK" (Interoperabilidad con el SDK de Application Insights), y comenzará a ver los datos en Application Insights. 
        > [!IMPORTANT]
        > Esta funcionalidad está en versión preliminar. 

        ![Habilitación de la configuración de la aplicación existente](./media/azure-web-apps/netcore-sdk-interop.png)

        Ahora los datos se van a enviar mediante un enfoque sin código, incluso si el SDK de Application Insights se usó originalmente o se intentó usar.

        > [!IMPORTANT]
        > Si la aplicación usó el SDK de Application Insights para enviar cualquier telemetría, se deshabilitará la telemetría —es decir, la telemetría personalizada— si existe, por ejemplo, los métodos Track*(), y se deshabilitará toda configuración personalizada, como el muestreo. 

#### <a name="linux-troubleshooting"></a>Solución de problemas de Linux

1. Compruebe que la configuración de la aplicación `ApplicationInsightsAgent_EXTENSION_VERSION` está establecida en un valor de "~3".
2. Vaya a */home\LogFiles\ApplicationInsights\status* y abra *status_557de146e7fa_27_1.json*.

    Confirme que `AppAlreadyInstrumented` está establecido en false, `AiHostingStartupLoaded` en true y `IKeyExists` en true.

    A continuación se muestra un ejemplo del archivo JSON:

    ```json
        "AppType":".NETCoreApp,Version=v6.0",
                
        "MachineName":"557de146e7fa",
                
        "PID":"27",
                
        "AppDomainId":"1",
                
        "AppDomainName":"dotnet6demo",
                
        "InstrumentationEngineLoaded":false,
                
        "InstrumentationEngineExtensionLoaded":false,
                
        "HostingStartupBootstrapperLoaded":true,
                
        "AppAlreadyInstrumented":false,
                
        "AppDiagnosticSourceAssembly":"System.Diagnostics.DiagnosticSource, Version=6.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51",
                
        "AiHostingStartupLoaded":true,
                
        "IKeyExists":true,
                
        "IKey":"00000000-0000-0000-0000-000000000000",
                
        "ConnectionString":"InstrumentationKey=00000000-0000-0000-0000-000000000000;IngestionEndpoint=https://westus-0.in.applicationinsights.azure.com/"
    
    ```
    
    Si `AppAlreadyInstrumented` es true, indica que la extensión ha detectado que algún aspecto del SDK ya está presente en la aplicación y se interrumpirá.

##### <a name="no-data-for-linux"></a>No hay datos para Linux 

1. Enumere e identifique el proceso que hospeda una aplicación. Vaya al terminal y, en la línea de comandos, escriba `ps ax`. 
    
    La salida debe ser similar a: 

   ```bash
     PID TTY      STAT   TIME COMMAND
    
        1 ?        SNs    0:00 /bin/bash /opt/startup/startup.sh
    
       19 ?        SNs    0:00 /usr/sbin/sshd
    
       27 ?        SNLl   5:52 dotnet dotnet6demo.dll
    
       50 ?        SNs    0:00 sshd: root@pts/0
    
       53 pts/0    SNs+   0:00 -bash
    
       55 ?        SNs    0:00 sshd: root@pts/1
    
       57 pts/1    SNs+   0:00 -bash
   ``` 


1. Luego enumere las variables de entorno del proceso de la aplicación. En la línea de comandos, escriba `cat /proc/27/environ | tr '\0' '\n`.
    
    La salida debe ser similar a: 

    ```bash
    ASPNETCORE_HOSTINGSTARTUPASSEMBLIES=Microsoft.ApplicationInsights.StartupBootstrapper
    
    DOTNET_STARTUP_HOOKS=/DotNetCoreAgent/2.8.39/StartupHook/Microsoft.ApplicationInsights.StartupHook.dll
    
    APPLICATIONINSIGHTS_CONNECTION_STRING=InstrumentationKey=00000000-0000-0000-0000-000000000000;IngestionEndpoint=https://westus-0.in.applicationinsights.azure.com/
    
    ```
    

1. Compruebe que `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_STARTUP_HOOKS` y `APPLICATIONINSIGHTS_CONNECTION_STRING` están definidos.


#### <a name="default-website-deployed-with-web-apps-does-not-support-automatic-client-side-monitoring"></a>El sitio web predeterminado implementado con las aplicaciones web no admite la supervisión automática de cliente.

Cuando se crea una aplicación web con los entornos de ejecución `ASP.NET` o `ASP.NET Core` en instancias de Azure App Service, implementa una única página HTML estática como sitio web de inicio. La página web estática también carga un elemento web administrado de ASP.NET en IIS. Esto permite probar la supervisión sin código del lado servidor, pero no admite la supervisión automática de cliente.

Si desea probar el servidor sin código y la supervisión de cliente para ASP.NET o ASP.NET Core en una aplicación web de Azure App Services, se recomienda seguir las guías oficiales para [crear una aplicación web ASP.NET Core](../../app-service/quickstart-dotnetcore.md) y [crear una aplicación web de ASP.NET Framework](../../app-service/quickstart-dotnetcore.md?tabs=netframework48) y después usar las instrucciones del artículo actual para habilitar la supervisión.


### <a name="php-and-wordpress-are-not-supported"></a>PHP y WordPress no se admiten

Los sitios de PHP y WordPress no se admiten. Actualmente, no hay ningún SDK/agente compatible oficialmente para la supervisión del lado servidor de estas cargas de trabajo. Sin embargo, la instrumentación manual de transacciones del lado cliente en un sitio de PHP o WordPress agregando el JavaScript del lado cliente a las páginas web puede realizarse mediante el [SDK de JavaScript](./javascript.md).

La tabla siguiente proporciona una explicación más detallada de lo que significan estos valores, sus causas subyacente y las correcciones recomendadas:

|Valor del problema|Explicación|Fix
|---- |----|---|
| `AppAlreadyInstrumented:true` | Este valor indica que la extensión ha detectado que algún aspecto del SDK ya está presente en la aplicación y se interrumpirá. Puede deberse a una referencia a `System.Diagnostics.DiagnosticSource`, `Microsoft.AspNet.TelemetryCorrelation` o `Microsoft.ApplicationInsights`.  | Quite las referencias. Algunas de estas referencias se agregan de manera predeterminada en determinadas plantillas de Visual Studio y las versiones anteriores de Visual Studio pueden agregar referencias a `Microsoft.ApplicationInsights`.
|`AppAlreadyInstrumented:true` | Si la aplicación se dirige a ASP.NET Core 2.1 o 2.2, este valor indica que la extensión ha detectado que algún aspecto del SDK ya está presente en la aplicación y se interrumpirá. | Se [recomienda](https://github.com/aspnet/Announcements/issues/287) que los clientes con .NET Core 2.1 o 2.2 usen el metapaquete Microsoft.AspNetCore.App en su lugar. Además, active Interop with Application Insights SDK (Interoperabilidad con SDK de Application Insights) en el portal (consulte las instrucciones anteriores).|
|`AppAlreadyInstrumented:true` | Este valor también puede deberse a la presencia de los archivos DLL anteriores en la carpeta de la aplicación de una implementación anterior. | Limpie la carpeta de la aplicación para asegurarse de que se han quitado estos archivos DLL. Compruebe el directorio "bin" de la aplicación local y el directorio "wwwroot" de App Service. (Para comprobar el directorio "wwwroot" de la aplicación web de App Service: herramientas avanzadas (Kudu) > Consola de depuración > CMD > home\site\wwwroot).
|`AppContainsAspNetTelemetryCorrelationAssembly: true` | Este valor indica que la extensión detectó referencias a `Microsoft.AspNet.TelemetryCorrelation` en la aplicación y se interrumpirá. | Quite la referencia.
|`AppContainsDiagnosticSourceAssembly**:true`|Este valor indica que la extensión detectó referencias a `System.Diagnostics.DiagnosticSource` en la aplicación y se interrumpirá.| Para ASP.NET, quite la referencia. 
|`IKeyExists:false`|Este valor indica que la clave de instrumentación no está presente en la AppSetting, `APPINSIGHTS_INSTRUMENTATIONKEY`. Causas posibles: Es posible que los valores se hayan eliminado por accidente, que haya olvidado establecer los valores en el script de automatización, etc. | Asegúrese de que la configuración está presente en la configuración de la aplicación de App Service.

### <a name="appinsights_javascript_enabled-and-urlcompression-is-not-supported"></a>No se admite APPINSIGHTS_JAVASCRIPT_ENABLED ni urlCompression

Si usa APPINSIGHTS_JAVASCRIPT_ENABLED=true en casos donde el contenido está codificado, podría obtener errores como los siguientes: 

- Error de reescritura de dirección URL 500
- Error de módulo de reescritura de dirección URL 500.53 con el mensaje Las reglas de reescritura saliente no se pueden aplicar cuando el contenido de la respuesta HTTP está codificado ("gzip"). 

Esto se debe a que la configuración de la aplicación APPINSIGHTS_JAVASCRIPT_ENABLED está establecida en true y la codificación de contenido está presente al mismo tiempo. Este escenario aún no se admite. La solución consiste en quitar APPINSIGHTS_JAVASCRIPT_ENABLED de la configuración de la aplicación. Esto significa que si la instrumentación de JavaScript del lado cliente o explorador sigue siendo necesaria, se necesitan referencias del SDK manuales para las páginas web. Siga las [instrucciones](https://github.com/Microsoft/ApplicationInsights-JS#snippet-setup-ignore-if-using-npm-setup) para la instrumentación manual con el SDK de JavaScript.

Para obtener la información más reciente sobre la extensión o el agente de Application Insights, consulte las [notas de la versión](https://github.com/MohanGsk/ApplicationInsights-Home/blob/master/app-insights-web-app-extensions-releasenotes.md).


### <a name="connection-string-and-instrumentation-key"></a>Cadena de conexión y clave de instrumentación

Al utilizar la supervisión sin código, solo se requiere la cadena de conexión. Sin embargo, todavía se recomienda establecer la clave de instrumentación para mantener la compatibilidad con versiones anteriores de SDK cuando se realiza la instrumentación manual.

### <a name="difference-between-standard-metrics-from-application-insights-vs-azure-app-service-metrics"></a>¿Cuál es la diferencia entre las métricas estándar de Application Insights frente a las métricas de Azure App Service?

Application Insights recopila datos de telemetría de las solicitudes que se hicieron a la aplicación. Si el error se produjo en Web Apps/IIS y la solicitud no llegó a la aplicación de usuario, Application Insights no tendrá ningún dato de telemetría sobre él.

La duración de `serverresponsetime` calculada por Application Insights no coincide necesariamente con el tiempo de respuesta del servidor observado por Web Apps. Esto se debe a que Application Insights solo cuenta la duración cuando la solicitud real llega a la aplicación de usuario. Si la solicitud se bloquea o pone en cola en IIS, ese tiempo de espera se incluirá en las métricas de Web Apps, pero no en las métricas de Application Insights.

## <a name="release-notes"></a>Notas de la versión

Para obtener las actualizaciones y correcciones de errores más recientes, [consulte las notas de la versión](./web-app-extension-release-notes.md).

## <a name="next-steps"></a>Pasos siguientes
* [Ejecute el generador de perfiles en la aplicación activa](./profiler.md).
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample): supervisar Azure Functions con Application Insights
* [Microsoft Azure Diagnostics](../agents/diagnostics-extension-to-application-insights.md) para enviar este tipo de información a Application Insights.
* [Supervise las métricas del estado del servicio](../data-platform.md) para asegurarse de que el servicio está disponible y responde adecuadamente.
* [Reciba notificaciones de alerta](../alerts/alerts-overview.md) cada vez que se produzcan eventos de operaciones o las métricas traspasen un umbral.
* Use [aplicaciones y páginas web de Application Insights para JavaScript](javascript.md) para obtener la telemetría del cliente de los exploradores que visitan una página web.
* [Configure pruebas web de disponibilidad](monitor-web-app-availability.md) para recibir una alerta si el sitio deja de estar activo.
