---
title: Retención y almacenamiento de datos en Azure Application Insights | Microsoft Docs
description: Declaración de directiva de retención y privacidad
ms.topic: conceptual
ms.date: 06/30/2020
ms.custom: devx-track-js, devx-track-csharp
ms.openlocfilehash: d0ef5131d97b16e63e40c903d456713eddb84218
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129232729"
---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Recopilación, retención y almacenamiento de datos en Application Insights

Al instalar el SDK de [Azure Application Insights][start] en su aplicación, se envía telemetría sobre la aplicación a la nube. Naturalmente, los desarrolladores responsables desean saber exactamente qué datos se envían, qué les ocurre a los datos y cómo puede mantener el control de los mismos. En concreto, si se puede enviar información confidencial, dónde se almacena y su nivel de seguridad. 

En primer lugar, la respuesta corta:

* Es improbable que los módulos de telemetría estándar que se ejecutan "de fábrica" envíen información confidencial al servicio. La telemetría se ocupa de la carga, las métricas de rendimiento y uso, los informes de excepciones y otros datos de diagnóstico. Los principales datos del usuario que aparecen los informes de diagnóstico son direcciones URL; pero, en cualquier caso, la aplicación no debería colocar información confidencial como texto sin formato en una dirección URL.
* Puede escribir código que envíe telemetría personalizada adicional que le ayude con el uso de la supervisión y el diagnóstico. (Esta extensibilidad es una excelente característica de Application Insights.) Por error, sería posible escribir este código de modo que incluya datos personales y otra información confidencial. Si la aplicación trabaja con estos datos, debe aplicar un proceso de revisión exhaustivo a todo el código que escriba.
* Al desarrollar y probar la aplicación, es fácil de inspeccionar lo que envía el SDK. Los datos aparecen en las ventanas de salida de depuración tanto del IDE como del explorador.
* Puede seleccionar la ubicación cuando se crea un nuevo recurso de Application Insights. [Aquí](https://azure.microsoft.com/global-infrastructure/services/?products=all) encontrará más información sobre la disponibilidad de Application Insights por región.
*   Revise los datos recopilados, ya que esto puede incluir datos que se permiten en algunas circunstancias, pero no en otras.  Un buen ejemplo de esto es el nombre de dispositivo. El nombre del dispositivo de un servidor no tiene ningún impacto en la privacidad y resulta útil, pero el nombre de dispositivo de un teléfono o portátil puede tener un impacto en la privacidad y resultar menos útil. Un SDK desarrollado principalmente para los servidores de destino, va a recopilar el nombre del dispositivo de forma predeterminada, por lo que es posible que deba sobrescribirse en eventos normales y excepciones.

En el resto de este artículo se describen más detalladamente estas respuestas. El artículo está diseñado para ser independiente, por lo que puede mostrarlo a compañeros que no formen parte de su equipo.

## <a name="what-is-application-insights"></a>¿Qué es Application Insights?
[Azure Application Insights][start] es un servicio que proporciona Microsoft que le ayuda a mejorar el rendimiento y la facilidad de uso de la aplicación activa. Supervisa la aplicación durante todo el tiempo que se ejecute, tanto durante las pruebas como después de haberla publicado o implementado. Application Insights crea gráficos y tablas que muestran, por ejemplo, en qué horas del día se obtiene la mayoría de los usuarios, la capacidad de respuesta de la aplicación y lo bien que la atienden los servicios externos de los que depende. Si hay bloqueos, errores o problemas de rendimiento, puede buscar en los datos de la telemetría para diagnosticar la causa. Y el servicio le enviará mensajes de correo electrónico si se produce cualquier tipo de cambio en la disponibilidad y rendimiento de la aplicación.

Para obtener esta funcionalidad, instale un SDK de Application Insights en la aplicación, que se convierte en parte de su código. Cuando se ejecuta la aplicación, el SDK supervisa su funcionamiento y envía la telemetría al servicio Application Insights. Se trata de un servicio en la nube hospedado por [Microsoft Azure](https://azure.com). (No obstante, Application Insights funciona con cualquier aplicación, no solo con las hospedadas en Azure).

El servicio de Application Insights almacena y analiza los datos de telemetría. Para ver el análisis o realizar búsquedas en la telemetría almacenada, inicie sesión en su cuenta de Azure y abra el recurso Application Insights de la aplicación. También puede compartir el acceso a los datos con otros miembros del equipo o con determinados suscriptores de Azure.

Los datos exportados del servicio Application Insights se pueden exportar, por ejemplo a una base de datos o a herramientas externas. Proporcione a cada herramienta una clave especial, que se obtiene desde el servicio. Si es necesario, dicha clave se puede revocar. 

Los SDK de Application Insights están disponible para varios tipos de aplicaciones: servicios web hospedados en Azure o en sus propios servidores de Java EE o ASP.NET; clientes web (es decir, el código se ejecuta en una página web); servicios y aplicaciones de escritorio, y aplicaciones para dispositivos, como Windows Phone, iOS y Android. Todos ellos envían telemetría al mismo servicio.

## <a name="what-data-does-it-collect"></a>¿Qué datos recopila?
Hay tres orígenes de datos:

* El SDK, que se integra con la aplicación en la [fase de desarrollo](./asp-net.md) o [en tiempo de ejecución](./status-monitor-v2-overview.md). Existen distintos SDK para los diferentes tipos de aplicaciones. También hay un [SDK para páginas web](./javascript.md), que se carga en el explorador del usuario final junto con la página.
  
  * Cada SDK tiene varios [módulos](./configuration-with-applicationinsights-config.md), que emplean diferentes técnicas para recopilar distintos tipos de datos de telemetría.
  * Si instala el SDK en la fase de desarrollo, puede usar su API para enviar su propia telemetría, además de los módulos estándar. Esta telemetría personalizada puede incluir los datos que desee enviar.
* En algunos servidores web, también hay agentes que se ejecutan junto con la aplicación y envían datos de telemetría de la CPU, memoria y ocupación de la red. Por ejemplo, las máquinas virtuales de Azure, los hosts de Docker y los [servidores de Java EE ](java-2x-agent.md) pueden tener dichos agentes.
* [pruebas de disponibilidad](./monitor-web-app-availability.md) son procesos que ejecuta Microsoft que envían solicitudes a una aplicación web a intervalos regulares. Los resultados se envían al servicio Application Insights.

### <a name="what-kinds-of-data-are-collected"></a>¿Qué tipos de datos se recopilan?
Las principales categorías son:

* [Telemetría de servidor web](./asp-net.md) : solicitudes HTTP.  Identificador URI, tiempo invertido en procesar la solicitud, código de respuesta y dirección IP del cliente. `Session id`.
* [Páginas web](./javascript.md) : contadores de sesión, página y usuario. Tiempos de carga de las páginas. Excepciones. Llamadas AJAX.
* Contadores de rendimiento: memoria, CPU, E/S, ocupación de la red.
* Contexto de cliente y servidor: SO, configuración regional, tipo de dispositivo, explorador y resolución de pantalla.
* [Excepciones](./asp-net-exceptions.md) y bloqueos: **volcados de pila**, `build id` y tipo de CPU. 
* [Dependencias](./asp-net-dependencies.md) : llamadas a servicios externos como REST, SQL y AJAX. Identificador URI o cadena de conexión, duración, con éxito, comando.
* [Pruebas de disponibilidad](./monitor-web-app-availability.md) : duración de prueba y pasos, respuestas.
* [Registros de seguimiento](./asp-net-trace-logs.md) y [telemetría personalizada](./api-custom-events-metrics.md) -  **: todo el código que se escribe en los registros o telemetría**.

[Más detalle](#data-sent-by-application-insights).

## <a name="how-can-i-verify-whats-being-collected"></a>¿Cómo se puede comprobar lo que se recopila?
Si desarrolla la aplicación mediante Visual Studio, ejecútela en modo de depuración (F5). Los datos de telemetría aparecen en la ventana de salida. Desde ahí, puede copiarlos y darles formato como JSON para facilitar su inspección. 

![Captura de pantalla que muestra la ejecución de la aplicación en modo de depuración en Visual Studio.](./media/data-retention-privacy/06-vs.png)

También existe una vista más legible en la ventana Diagnóstico.

En el caso de las páginas web, abra la ventana de depuración del explorador.

![Presione F12 y abra la pestaña Red.](./media/data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-to-filter-the-telemetry-before-it-is-sent"></a>¿Puedo escribir código para filtrar los datos de telemetría antes de enviarlos?
Sí, es posible, solo es preciso escribir un [complemento de procesador de telemetría](./api-filtering-sampling.md).

## <a name="how-long-is-the-data-kept"></a>¿Cuánto tiempo se conservan los datos?
Los puntos de datos sin procesar (es decir, elementos que puede consultar en Analytics e inspeccionar en la funcionalidad de búsqueda) se conservan hasta 730 días. Puede [seleccionar una duración de retención](./pricing.md#change-the-data-retention-period) de 30, 60, 90, 120, 180, 270, 365, 550 o 730 días. Si necesita mantener los datos más tiempo, puede usar la [exportación continua](./export-telemetry.md) para copiarlos a una cuenta de almacenamiento durante la ingesta de datos. 

Los datos que se mantengan más de 90 días generarán cargos adicionales. Más información sobre los precios de Application Insights en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

Los datos agregados (es decir, recuentos, promedios y otros datos estadísticos que se ven en el Explorador de métricas) se retienen con un nivel de detalle de un minuto durante 90 días.

[Las instantáneas de depuración](./snapshot-debugger.md) se guardan durante 15 días. Esta directiva de retención se establece para cada aplicación. Si necesita aumentar este valor, puede solicitar un aumento abriendo una incidencia de soporte técnico en Azure Portal.

## <a name="who-can-access-the-data"></a>¿Quién puede acceder a los datos?
Usted puede ver los datos y, si tiene una cuenta de organización, también pueden los miembros del equipo. 

Los puede exportar tanto usted como los miembros del equipo y pueden copiarse a otras ubicaciones y pasarse a otras personas.

#### <a name="what-does-microsoft-do-with-the-information-my-app-sends-to-application-insights"></a>¿Qué hace Microsoft con la información que la aplicación envía a Application Insights?
Microsoft usa los datos con el fin exclusivo de proporcionarle el servicio.

## <a name="where-is-the-data-held"></a>¿Donde se conservan los datos?
* Puede seleccionar la ubicación cuando se crea un nuevo recurso de Application Insights. [Aquí](https://azure.microsoft.com/global-infrastructure/services/?products=all) encontrará más información sobre la disponibilidad de Application Insights por región.

## <a name="how-secure-is-my-data"></a>¿Están seguros mis datos?
Application Insights es un servicio de Azure. Las directivas de seguridad se describen en las [notas del producto de seguridad, privacidad y cumplimiento de Azure](https://go.microsoft.com/fwlink/?linkid=392408).

Los datos se almacenan en servidores de Microsoft Azure. En el caso de las cuentas de Azure Portal, las restricciones se describen en el documento [Seguridad, privacidad y cumplimiento de Azure](https://go.microsoft.com/fwlink/?linkid=392408).

El acceso a los datos por parte del personal de Microsoft está restringido. El acceso a los datos solo se realiza con su permiso y si es necesario para prestarle soporte en el uso de Application Insights. 

Los datos que se agregan en todas las aplicaciones de nuestros clientes (por ejemplo, la velocidad de datos y el tamaño medio de los seguimientos) se usan para mejorar Application Insights.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>¿Puede interferir la telemetría de otro usuario con mis datos de Application Insights?
Podría enviarse telemetría adicional a su cuenta mediante el uso de la clave de instrumentación, que se encuentra en el código de las páginas web. Si tienen demasiados datos adicionales, las métricas no representan correctamente el rendimiento y el uso de la aplicación.

Si comparte el código con otros proyectos, no olvide quitar la clave de instrumentación.

## <a name="is-the-data-encrypted"></a>¿Se cifran los datos?
Todos los datos se cifran en reposo y al moverse entre centros de datos.

#### <a name="is-the-data-encrypted-in-transit-from-my-application-to-application-insights-servers"></a>¿Se cifran los datos en tránsito desde mi aplicación a los servidores de Application Insights?
Sí, se usa https para enviar datos al portal desde casi todos los SDK, incluidos servidores web, dispositivos y páginas web HTTPS. 

## <a name="does-the-sdk-create-temporary-local-storage"></a>¿El SDK crea almacenamiento local temporal?

Sí, algunos canales de telemetría conservarán los datos localmente si no se puede alcanzar un punto de conexión. Revise a continuación para ver los marcos de trabajo y los canales de telemetría afectados.

Los canales de telemetría que utilizan almacenamiento local crean archivos temporales en los directorios TEMP o APPDATA que están restringidos a la cuenta específica que ejecuta la aplicación. Esto puede ocurrir cuando un punto de conexión no estaba disponible temporalmente o se alcanza el límite. Una vez que se resuelva este problema, el canal de telemetría reanudará el envío de todos los datos nuevos y conservados.

Los datos conservados no se cifran localmente. Si esto supone un problema, revise los datos y limite la recopilación de datos privados. (Para obtener más información, consulte [Cómo exportar y eliminar datos privados](../logs/personal-data-mgmt.md#how-to-export-and-delete-private-data)).

Si un cliente necesita para configurar este directorio con requisitos de seguridad específicos, se puede configurar para cada marco de trabajo. Asegúrese de que el proceso que ejecuta la aplicación tiene acceso de escritura a este directorio, pero asegúrese también de que este directorio está protegido para evitar que usuarios no deseados puedan leer datos de telemetría.

### <a name="java"></a>Java

`C:\Users\username\AppData\Local\Temp` se utiliza para guardar los datos. Esta ubicación no es configurable desde el directorio de configuración y los permisos para acceder a esta carpeta están restringidos a un usuario determinado con las credenciales necesarias. (Para obtener más información, consulte [Implementación](https://github.com/Microsoft/ApplicationInsights-Java/blob/40809cb6857231e572309a5901e1227305c27c1a/core/src/main/java/com/microsoft/applicationinsights/internal/util/LocalFileSystemUtils.java#L48-L72)).

###  <a name="net"></a>.NET

De forma predeterminada, `ServerTelemetryChannel` usa la carpeta de datos de la aplicación local del usuario actual `%localAppData%\Microsoft\ApplicationInsights` o la carpeta temporal `%TMP%`. (Consulte la [implementación](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/91e9c91fcea979b1eec4e31ba8e0fc683bf86802/src/ServerTelemetryChannel/Implementation/ApplicationFolderProvider.cs#L54-L84) aquí).


Mediante el archivo de configuración:
```xml
<TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel,   Microsoft.AI.ServerTelemetryChannel">
    <StorageFolder>D:\NewTestFolder</StorageFolder>
</TelemetryChannel>
```

Mediante código:

- Eliminación de ServerTelemetryChannel del archivo de configuración
- Agregue este fragmento de código a la configuración:
  ```csharp
  ServerTelemetryChannel channel = new ServerTelemetryChannel();
  channel.StorageFolder = @"D:\NewTestFolder";
  channel.Initialize(TelemetryConfiguration.Active);
  TelemetryConfiguration.Active.TelemetryChannel = channel;
  ```

### <a name="netcore"></a>NetCore

De forma predeterminada, `ServerTelemetryChannel` usa la carpeta de datos de la aplicación local del usuario actual `%localAppData%\Microsoft\ApplicationInsights` o la carpeta temporal `%TMP%`. (Consulte la [implementación](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/91e9c91fcea979b1eec4e31ba8e0fc683bf86802/src/ServerTelemetryChannel/Implementation/ApplicationFolderProvider.cs#L54-L84) aquí). 

En un entorno Linux, se deshabilitará el almacenamiento local a menos que se especifique una carpeta de almacenamiento.

> [!NOTE]
> Con la versión 2.15.0-beta3 y superior, ahora se crea automáticamente almacenamiento local para Linux, Mac y Windows. En el caso de sistemas que no son Windows, el SDK creará automáticamente una carpeta de almacenamiento local basada en la siguiente lógica:
> - `${TMPDIR}`: si está establecida la variable de entorno `${TMPDIR}`, se usa esta ubicación.
> - `/var/tmp`: si la ubicación anterior no existe, se prueba `/var/tmp`.
> - `/tmp`: si las dos ubicaciones anteriores no existen, se prueba `tmp`. 
> - Si no existe ninguna de esas ubicaciones, no se crea almacenamiento local y todavía se requiere configuración manual. [Consulte aquí para conocer los detalles completos de implementación](https://github.com/microsoft/ApplicationInsights-dotnet/pull/1860).

El fragmento de código siguiente muestra cómo establecer `ServerTelemetryChannel.StorageFolder` en el método `ConfigureServices()` de la clase `Startup.cs`:

```csharp
services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
```

(Para obtener más información, consulte [Configuración personalizada de AspNetCore](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration)).

### <a name="nodejs"></a>Node.js

De forma predeterminada, se utiliza `%TEMP%/appInsights-node{INSTRUMENTATION KEY}` para guardar los datos. Los permisos para acceder a esta carpeta están restringidos al usuario actual y los administradores. (Consulte la [implementación](https://github.com/Microsoft/ApplicationInsights-node.js/blob/develop/Library/Sender.ts) aquí).

El prefijo de la carpeta `appInsights-node` se puede invalidar cambiando el valor en tiempo de ejecución de la variable estática `Sender.TEMPDIR_PREFIX` que se encuentra en [Sender.ts](https://github.com/Microsoft/ApplicationInsights-node.js/blob/7a1ecb91da5ea0febf5ceab13d6a4bf01a63933d/Library/Sender.ts#L384).

### <a name="javascript-browser"></a>JavaScript (explorador)

Se usa [almacenamiento de sesión de HTML5](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) para conservar los datos. Se usan dos búferes independientes: `AI_buffer` y `AI_sent_buffer`. La telemetría por lotes y en espera de ser enviada se almacena en `AI_buffer`. La telemetría que se acaba de enviar se coloca en `AI_sent_buffer` hasta que el servidor de ingesta responde que se ha recibido correctamente. Una vez que la telemetría se ha recibido correctamente, se quita de todos los búferes. En los errores transitorios (por ejemplo, un usuario pierde la conectividad de red), la telemetría permanece en `AI_buffer` hasta que se recibe correctamente o el servidor de ingesta responde que no es válida (esquema incorrecto o demasiado antiguo, por ejemplo).

Los búferes de telemetría se pueden deshabilitar si se establece [`enableSessionStorageBuffer`](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/legacy/JavaScript/JavaScriptSDK.Interfaces/IConfig.ts#L31) en `false`. Cuando se desactiva el almacenamiento de sesión, en su lugar se usa una matriz local como almacenamiento persistente. Dado que el SDK de JavaScript se ejecuta en un dispositivo cliente, el usuario tiene acceso a esta ubicación de almacenamiento mediante las herramientas de desarrollo del explorador.

### <a name="opencensus-python"></a>Python para OpenCensus

De forma predeterminada, el SDK de Python para OpenCensus usa la carpeta del usuario actual `%username%/.opencensus/.azure/`. Los permisos para acceder a esta carpeta están restringidos al usuario actual y los administradores. (Consulte la [implementación](https://github.com/census-instrumentation/opencensus-python/blob/master/contrib/opencensus-ext-azure/opencensus/ext/azure/common/storage.py) aquí). La carpeta con los datos persistentes se denominará según el archivo de Python que generó la telemetría.

Puede cambiar la ubicación del archivo de almacenamiento si pasa el parámetro `storage_path` en el constructor del exportador que usa.

```python
AzureLogHandler(
  connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000',
  storage_path='<your-path-here>',
)
```

## <a name="how-do-i-send-data-to-application-insights-using-tls-12"></a>¿Cómo se envían datos a Application Insights con TLS 1.2?

Para garantizar la seguridad de los datos en tránsito a los puntos de conexión de Application Insights, se recomienda encarecidamente a los clientes configurar su aplicación para que use al menos Seguridad de la capa de transporte (TLS) 1.2. Las versiones anteriores de TLS/Capa de sockets seguros (SSL) han demostrado ser vulnerables y, si bien todavía funcionan para permitir la compatibilidad con versiones anteriores, **no se recomiendan**, y el sector está cambiando rápidamente para abandonar la compatibilidad de estos protocolos más antiguos. 

[PCI Security Standards Council](https://www.pcisecuritystandards.org/) ha establecido una [fecha límite el 30 de junio de 2018](https://www.pcisecuritystandards.org/pdfs/PCI_SSC_Migrating_from_SSL_and_Early_TLS_Resource_Guide.pdf) para deshabilitar las versiones anteriores de TLS/SSL y actualizar a protocolos más seguros. Una vez que Azure quite la compatibilidad heredada, si la aplicación o los clientes no pueden comunicarse a través de TLS 1.2 como mínimo, podría no ser capaz de enviar datos a Application Insights. El enfoque que adopte para probar y validar la compatibilidad con TLS de la aplicación variará según la plataforma y sistema operativo, así como el lenguaje o marco que use la aplicación.

No se recomienda establecer explícitamente la aplicación para que solo use TLS 1.2, a menos que sea necesario. Eso se debe a que puede interrumpir las características de seguridad a nivel de la plataforma que le permiten detectar y aprovechar automáticamente las ventajas de los protocolos más seguros más recientes a medida que estén disponibles, como TLS 1.3. Se recomienda realizar una auditoría completa del código de la aplicación para comprobar la codificación rígida de versiones específicas de TLS/SSL.

### <a name="platformlanguage-specific-guidance"></a>Instrucciones específicas para la plataforma y el lenguaje

|Plataforma/lenguaje | Soporte técnico | Más información |
| --- | --- | --- |
| Azure App Services  | Compatible, puede requerir configuración. | La compatibilidad se anunció en abril de 2018. Lea el anuncio para obtener los [detalles de configuración](https://azure.github.io/AppService/2018/04/17/App-Service-and-Functions-hosted-apps-can-now-update-TLS-versions!).  |
| Azure Function Apps | Compatible, puede requerir configuración. | La compatibilidad se anunció en abril de 2018. Lea el anuncio para obtener los [detalles de configuración](https://azure.github.io/AppService/2018/04/17/App-Service-and-Functions-hosted-apps-can-now-update-TLS-versions!). |
|.NET | Compatible, la configuración varía según la versión. | Para obtener información detallada de la configuración para .NET 4.7 y versiones anteriores, consulte [estas instrucciones](/dotnet/framework/network-programming/tls#support-for-tls-12).  |
|Monitor de estado | Compatible, se requiere configuración | Monitor de estado se basa en la [configuración del sistema operativo](/windows-server/security/tls/tls-registry-settings) + [configuración de .NET](/dotnet/framework/network-programming/tls#support-for-tls-12) para admitir TLS 1.2.
|Node.js |  Compatible, en v10.5.0, puede requerir configuración. | Use la [documentación oficial de Node.js TLS/SSL](https://nodejs.org/api/tls.html) para cualquier configuración específica de la aplicación. |
|Java | Compatible, se agregó compatibilidad de JDK para TLS 1.2 en la [actualización 121 de JDK 6](https://www.oracle.com/technetwork/java/javase/overview-156328.html#R160_121) y [JDK 7](https://www.oracle.com/technetwork/java/javase/7u131-relnotes-3338543.html). | JDK 8 usa [TLS 1.2 de forma predeterminada](https://blogs.oracle.com/java-platform-group/jdk-8-will-use-tls-12-as-default).  |
|Linux | Las distribuciones de Linux tienden a basarse en [OpenSSL](https://www.openssl.org) para la compatibilidad con TLS 1.2.  | Compruebe el [registro de cambios de OpenSSL](https://www.openssl.org/news/changelog.html) para confirmar si su versión de OpenSSL es compatible.|
| Windows 8.0 a 10 | Compatible y habilitado de manera predeterminada. | Para confirmar que aún usa la [configuración predeterminada](/windows-server/security/tls/tls-registry-settings).  |
| Windows Server 2012 a 2016 | Compatible y habilitado de manera predeterminada. | Para confirmar que aún usa la [configuración predeterminada](/windows-server/security/tls/tls-registry-settings) |
| Windows 7 SP1 y Windows Server 2008 R2 SP1 | Compatible, pero no habilitado de manera predeterminada. | Consulte la página [Configuración del registro de TLS](/windows-server/security/tls/tls-registry-settings) para obtener más información sobre cómo se habilita.  |
| Windows Server 2008 SP2 | La compatibilidad con TLS 1.2 requiere una actualización. | Consulte [Actualización para agregar compatibilidad con TLS 1.2](https://support.microsoft.com/help/4019276/update-to-add-support-for-tls-1-1-and-tls-1-2-in-windows-server-2008-s) en Windows Server 2008 SP2. |
|Windows Vista | No compatible. | N/D

### <a name="check-what-version-of-openssl-your-linux-distribution-is-running"></a>Comprobación de qué versión de OpenSSL se ejecuta para su distribución de Linux

Para comprobar qué versión de OpenSSL tiene instalada, abra el terminal y ejecute:

```terminal
openssl version -a
```

### <a name="run-a-test-tls-12-transaction-on-linux"></a>Ejecución de una transacción de TLS 1.2 de prueba en Linux

Para ejecutar una prueba preliminar a fin de comprobar si el sistema de Linux puede comunicarse a través de TLS 1.2, abra el terminal y ejecute lo siguiente:

```terminal
openssl s_client -connect bing.com:443 -tls1_2
```

## <a name="personal-data-stored-in-application-insights"></a>Datos personales almacenados en Application Insights

Nuestra [Guía sobre datos personales almacenados en Application Insights](../logs/personal-data-mgmt.md) trata este problema en detalle.

#### <a name="can-my-users-turn-off-application-insights"></a>¿Pueden los usuarios desactivar Application Insights?
No directamente. No se proporciona un conmutador que los usuarios puedan operar para desactivar Application Insights.

Sin embargo, puede implementar esta característica en la aplicación. Todos los SDK incluyen un valor de configuración de la API que desactiva la recopilación de telemetría. 

## <a name="data-sent-by-application-insights"></a>Datos enviados por Application Insights
Los SDK varían entre las distintas plataformas, y hay varios componentes que se pueden instalar. (Consulte [Introducción a Application Insights][start]). Cada componente envía datos diferentes.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Clases de datos que se envían en distintos escenarios

| Acción del usuario | Clases de datos recopilados (ver tabla siguiente) |
| --- | --- |
| [Adición del SDK de Application Insights a un proyecto web de .NET][greenbrown] |ServerContext<br/>Inferidos<br/>Contadores de rendimiento<br/>Requests<br/>**Excepciones**<br/>Sesión<br/>users |
| [Instalación del Monitor de estado en IIS][redfield] |Dependencias<br/>ServerContext<br/>Inferidos<br/>Contadores de rendimiento |
| [Adición del SDK de Application Insights a una aplicación web de Java][java] |ServerContext<br/>Inferidos<br/>Solicitud<br/>Sesión<br/>users |
| [Incorporación del SDK de JavaScript a una página web][client] |ClientContext <br/>Inferidos<br/>Página<br/>ClientPerf<br/>Ajax |
| [Definición de propiedades predeterminadas][apiproperties] |**Propiedades** en todos los eventos estándar y personalizados |
| [Llamar a TrackMetric][api] |Valores numéricos<br/>**Propiedades** |
| [Llamar a Track*][api] |Nombre del evento<br/>**Propiedades** |
| [Llamar a TrackException][api] |**Excepciones**<br/>Volcado de la pila<br/>**Propiedades** |
| El SDK no puede recopilar datos. Por ejemplo: <br/> - no se puede acceder a los contadores de rendimiento<br/> - excepción en el inicializador de telemetría |Diagnóstico de SDK |

En el caso de los [SDK de otras plataformas][platforms], consulte los documentos correspondientes.

#### <a name="the-classes-of-collected-data"></a>Clases de los datos recopilados

| Clase de datos recopilados | Se incluyen (no es una lista exhaustiva) |
| --- | --- |
| **Propiedades** |**Cualquier dato - determinado por el código** |
| DeviceContext |`Id`, IP, configuración regional, modelo de dispositivo, red, tipo de red, nombre de OEM, resolución de pantalla, instancia de rol, nombre de rol, tipo de dispositivo |
| ClientContext |Sistema operativo, configuración regional, idioma, red, resolución de ventana |
| Sesión |`session id` |
| ServerContext |Nombre del equipo, configuración regional, sistema operativo, dispositivo, sesión de usuario, contexto de usuario, operación |
| Inferidos |ubicación geográfica de dirección IP, marca de tiempo, sistema operativo, explorador |
| Métricas |Nombre y valor de la métrica |
| Eventos |Nombre y valor del evento |
| PageViews |URL y nombre de página o nombre de pantalla |
| Rendimiento del cliente |URL o nombre de página, tiempo de carga del explorador |
| Ajax |Llamadas HTTP de la página web al servidor |
| Requests |URL, duración, código de respuesta |
| Dependencias |Tipo (SQL, HTTP,...), cadena de conexión o URI, sincrónico/asincrónico, duración, éxito, instrucción SQL (con monitor de estado) |
| **Excepciones** |Tipo, **mensaje**, pilas de llamadas, archivo de origen, número de línea, `thread id` |
| Bloqueos |`Process id`, `parent process id`, `crash thread id`; revisión de aplicación, `id`, compilación; tipo de excepción, dirección, razón; símbolos y registros confusos, direcciones binarias inicial y final, nombre binario y ruta de acceso, tipo de CPU |
| Seguimiento |**Mensaje** y nivel de gravedad |
| Contadores de rendimiento |Tiempo de procesador, memoria disponible, velocidad de solicitudes, tasa de excepciones, bytes privados del proceso, velocidad de E/S, duración de la solicitud, longitud de la cola de solicitudes |
| Disponibilidad |Código de respuesta de prueba web, duración de cada paso de la prueba, nombre de la prueba, marca de tiempo, éxito, tiempo de respuesta, ubicación de la prueba |
| Diagnóstico de SDK |Mensaje de seguimiento o de excepción |

También puede [desactivar algunos de los datos mediante la edición de ApplicationInsights.config][config].

> [!NOTE]
> La IP del cliente se usa para deducir la ubicación geográfica, pero de forma predeterminada ya no se almacenan datos de IP y todos los ceros se escriben en el campo asociado. Para más información sobre el control de los datos personales, se recomienda leer este [artículo](../logs/personal-data-mgmt.md#application-data). Si necesita almacenar datos de direcciones IP, nuestro [artículo sobre la colección de direcciones IP](./ip-collection.md) le guiará por las opciones.

## <a name="can-i-modify-or-update-data-after-it-has-been-collected"></a>¿Puedo modificar o actualizar los datos una vez recopilados?

No, los datos son de solo lectura y solo se pueden eliminar a través de la funcionalidad de purga. Para obtener más información, consulte [Guía sobre datos personales almacenados en Log Analytics y Application Insights](../logs/personal-data-mgmt.md#delete).

## <a name="credits"></a>Créditos
Este producto incluye datos GeoLite2 creados por MaxMind, disponible en [https://www.maxmind.com](https://www.maxmind.com).



<!--Link references-->

[api]: ./api-custom-events-metrics.md
[apiproperties]: ./api-custom-events-metrics.md#properties
[client]: ./javascript.md
[config]: ./configuration-with-applicationinsights-config.md
[greenbrown]: ./asp-net.md
[java]: ./java-in-process-agent.md
[platforms]: ./platforms.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/
[redfield]: ./status-monitor-v2-overview.md
[start]: ./app-insights-overview.md
