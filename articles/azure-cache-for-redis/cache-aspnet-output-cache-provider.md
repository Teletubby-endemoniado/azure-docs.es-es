---
title: Proveedor de caché de resultados de ASP.NET para Azure Cache for Redis
description: Obtenga información acerca de cómo almacenar en caché los resultados de página de ASP.NET con Azure Cache for Redis. El proveedor de la caché de salida de Redis es un mecanismo de almacenamiento fuera de proceso para los datos de la caché de salida.
author: curib
ms.author: cauribeg
ms.service: cache
ms.custom: devx-track-csharp
ms.topic: conceptual
ms.date: 04/22/2018
ms.openlocfilehash: 89c76331f9aad8238b596094ad2057df98eea9a9
ms.sourcegitcommit: e82ce0be68dabf98aa33052afb12f205a203d12d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2021
ms.locfileid: "129659533"
---
# <a name="aspnet-output-cache-provider-for-azure-cache-for-redis"></a>Proveedor de caché de resultados de ASP.NET para Azure Cache for Redis

El proveedor de la caché de salida de Redis es un mecanismo de almacenamiento fuera de proceso para los datos de la caché de salida. Estos datos resultan necesarios específicamente para respuestas HTTP completas (caché de resultados de la página). El proveedor se conecta al nuevo punto de extensibilidad del proveedor de caché de salida que se introdujo en ASP.NET 4. En el caso de las aplicaciones ASP.NET Core, lea [Almacenamiento en caché de respuestas en ASP.NET Core](/aspnet/core/performance/caching/response).

Para usar el proveedor de la caché de salida de Redis, configure primero la caché y luego configure la aplicación de ASP.NET mediante el paquete NuGet del proveedor de la caché de salida de Redis. En este tema se proporcionan instrucciones sobre cómo configurar la aplicación para usar el proveedor de la caché de salida de Redis. Para más información acerca de cómo crear y configurar una instancia de Azure Cache for Redis, consulte [Creación de una caché](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>Almacenamiento de los resultados de página de ASP.NET en la caché

Para configurar una aplicación cliente en Visual Studio usando el paquete NuGet de estado de sesión de Azure Cache for Redis, en el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** y **Consola del Administrador de paquetes**.

Ejecute el siguiente comando desde la ventana `Package Manager Console`.

```powershell
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

El paquete NuGet del proveedor de la caché de salida de Redis tiene una dependencia del paquete StackExchange.Redis. Si el paquete StackExchange.Redis no existe en el proyecto, se instalará. Para obtener más información sobre el paquete de NuGet del proveedor de caché de salida de Redis, vea la página [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) de NuGet.

El paquete NuGet descarga y agrega las referencias de ensamblado necesarias y agrega la siguiente sección al archivo web.config. Esta sección contiene la configuración necesaria para que la aplicación ASP.NET use el proveedor de memoria caché de salida de Redis.

```xml
<caching>
  <outputCache defaultProvider="MyRedisOutputCache">
    <providers>
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider"
           host=""
           accessKey=""
           ssl="true" />
    </providers>
  </outputCache>
</caching>
```

Configure los atributos de la izquierda con los valores de la memoria caché en Microsoft Azure Portal. Asimismo, configure los demás valores que desee. Para obtener instrucciones acerca de cómo acceder a las propiedades de la caché, consulte [Configuración de Azure Cache for Redis](cache-configure.md#configure-azure-cache-for-redis-settings).

| Atributo | Tipo | Valor predeterminado | Descripción |
| --------- | ---- | ------- | ----------- |
| *host* | string | "localhost" | Nombre de host o dirección del IP del servidor de Redis |
| *port* | entero positivo | 6379 (non-TLS/SSL)<br/>6380 (TLS/SSL) | Puerto del servidor de Redis |
| *accessKey* | string | "" | Contraseña del servidor de Redis con la autorización de Redis habilitada. El valor es una cadena vacía de forma predeterminada, lo que significa que el proveedor de estado de sesión no utilizará ninguna contraseña al conectarse al servidor de Redis. **Si el servidor de Redis está en una red de acceso público como Azure Cache for Redis, asegúrese de habilitar la autorización de Redis mejorar la seguridad y proporcione una contraseña segura.** |
| *ssl* | boolean | **false** | Si desea conectarse al servidor de Redis a través de TLS. Este valor es **false** de forma predeterminada, dado que Redis no admite TLS de forma predeterminada. **Si usa Azure Cache for Redis, que admite SSL de forma predeterminada, no olvide establecer este valor en true para mejorar la seguridad.**<br/><br/>El puerto no TLS está deshabilitado de forma predeterminada para las memorias caché nuevas. Especifique **true** en este valor para usar el puerto que no es TLS. Para más información sobre cómo habilitar el puerto no TLS, vea la sección [Puertos de acceso](cache-configure.md#access-ports) del tema sobre la [configuración de una caché](cache-configure.md). |
| *databaseIdNumber* | entero positivo | 0 | *Este atributo se puede especificar únicamente mediante web.config o AppSettings.*<br/><br/>Especifique qué base de datos de Redis usar. |
| *connectionTimeoutInMilliseconds* | entero positivo | Suministrado por StackExchange.Redis | Se usa para establecer *ConnectTimeout* al crear StackExchange.Redis.ConnectionMultiplexer. |
| *operationTimeoutInMilliseconds* | entero positivo | Suministrado por StackExchange.Redis | Se usa para establecer *SyncTimeout* al crear StackExchange.Redis.ConnectionMultiplexer. |
| *connectionString* (cadena de conexión StackExchange.Redis válida) | string | *n/a* | Una referencia de parámetro a AppSettings o web.config, o una cadena de conexión de StackExchange.Redis válida. Este atributo puede proporcionar valores para *host*, *port*, *accessKey*, *ssl* y otros atributos de StackExchange.Redis. Para más detalles de *connectionString*, consulte [Valor de connectionString](#setting-connectionstring) en la sección [Notas sobre los atributos](#attribute-notes). |
| *settingsClassName*<br/>*settingsMethodName* | string<br/>string | *n/a* | *Estos atributos se pueden especificar únicamente mediante web.config o AppSettings.*<br/><br/>Use estos atributos para proporcionar una cadena de conexión. *settingsClassName* debe ser un nombre de clase completo del ensamblado que contenga el método especificado por *settingsMethodName*.<br/><br/>El método especificado por *settingsMethodName* debe ser público, estático y nulo (no acepta ningún parámetro), con un tipo de valor devuelto de **cadena**. Este método devuelve la cadena de conexión real. |
| *loggingClassName*<br/>*loggingMethodName* | string<br/>string | *n/a* | *Estos atributos se pueden especificar únicamente mediante web.config o AppSettings.*<br/><br/>Use estos atributos para depurar la aplicación proporcionando los registros desde la caché de estado de sesión y salida, junto con los registros de StackExchange.Redis. *loggingClassName* debe ser un nombre de clase completo del ensamblado que contenga el método especificado por *loggingMethodName*.<br/><br/>El método especificado por *loggingMethodName* debe ser público, estático y nulo (no acepta ningún parámetro), con un tipo de valor devuelto de **System.IO.TextWriter**. |
| *applicationName* | string | El nombre del módulo del proceso actual o "/" | *SessionStateProvider only*<br/>*Este atributo se puede especificar únicamente mediante web.config o AppSettings.*<br/><br/>El prefijo del nombre de aplicación que usar en la memoria caché de Redis. El cliente podría usar la misma caché de Redis para distintos fines. Para garantizar que las claves de sesión no entren en conflicto, se puede prefijar con el nombre de la aplicación. |
| *throwOnError* | boolean | true | *SessionStateProvider only*<br/>*Este atributo se puede especificar únicamente mediante web.config o AppSettings.*<br/><br/>Si se produce una excepción cuando se produce un error.<br/><br/>Para más información acerca de *throwOnError*, consulte [Notas sobre *throwOnError*](#notes-on-throwonerror) en la sección [Notas sobre los atributos](#attribute-notes). |
| *retryTimeoutInMilliseconds* | entero positivo | 5000 | *SessionStateProvider only*<br/>*Este atributo se puede especificar únicamente mediante web.config o AppSettings.*<br/><br/>Tiempo para volver a intentarlo cuando falla una operación. Si este valor es menor que el de *operationTimeoutInMilliseconds*, el proveedor no volverá a intentarlo.<br/><br/>Para más información acerca de *retryTimeoutInMilliseconds*, consulte [Notas sobre *retryTimeoutInMilliseconds*](#notes-on-retrytimeoutinmilliseconds) en la sección [Notas sobre los atributos](#attribute-notes). |
| *redisSerializerType* | string | *n/a* | Especifica el nombre de tipo calificado de ensamblado de una clase que implementa Microsoft.Web.Redis. Serializador que contiene la lógica personalizada para serializar y deserializar los valores. Para más información, consulte [Acerca de *redisSerializerType*](#about-redisserializertype) en la sección [Notas sobre los atributos](#attribute-notes). |

## <a name="attribute-notes"></a>Notas sobre los atributos

### <a name="setting-connectionstring"></a>Valor de *connectionString*

El valor de *connectionString* se utiliza como clave para capturar la cadena de conexión real de AppSettings (si este tipo de cadena existe en AppSettings). Si no se encuentra en AppSettings, el valor de *connectionString* se utilizará como clave para recuperar la cadena de conexión real desde la sección **ConnectionString** de web.config (si existe esa sección). Si la cadena de conexión no existe en AppSettings o la sección **ConnectionString** de web.config, se usará el valor literal de *connectionString* como cadena de conexión al crear StackExchange.Redis.ConnectionMultiplexer.

Los siguientes ejemplos muestran cómo se utiliza *connectionString*.

#### <a name="example-1"></a>Ejemplo 1

```xml
<connectionStrings>
    <add name="MyRedisConnectionString" connectionString="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</connectionStrings>
```

En `web.config`, utilice la clave anterior como valor del parámetro en lugar del valor real.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-2"></a>Ejemplo 2

```xml
<appSettings>
    <add key="MyRedisConnectionString" value="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</appSettings>
```

En `web.config`, utilice la clave anterior como valor del parámetro en lugar del valor real.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-3"></a>Ejemplo 3

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False"/>
    </providers>
</sessionState>
```

### <a name="notes-on-throwonerror"></a>Notas sobre *throwOnError*

Actualmente, si se produce un error durante una operación de sesión, el proveedor de estado de sesión produce una excepción. Al iniciar la excepción se cierra la aplicación.

Este comportamiento se modificó para ajustarse a las expectativas de los usuarios existentes del proveedor de estado de sesión de ASP.NET, al tiempo que permite actuar sobre las excepciones. El comportamiento predeterminado todavía produce una excepción cuando se produce un error, de manera coherente con otros proveedores de estado de sesión de ASP.NET. El código existente debería funcionar igual que antes.

Si establece *throwOnError* en **false**, en lugar de producir una excepción cuando se produce un error, el error se produce en modo silencioso. Para ver si se produjo un error y, si es así, detectar la excepción, compruebe la propiedad estática *Microsoft.Web.Redis.RedisSessionStateProvider.LastException*.

### <a name="notes-on-retrytimeoutinmilliseconds"></a>Notas sobre *retryTimeoutInMilliseconds*

La opción *retryTimeoutInMilliseconds* proporciona cierta lógica para simplificar el caso en el que una operación de sesión debe reintentar en caso de error debido a un problema de red u otro incidente. La opción *retryTimeoutInMilliseconds* también permite controlar el tiempo de espera del reintento o rechazar completamente el reintento.

Si establece *retryTimeoutInMilliseconds* en un número, por ejemplo, 2000, cuando se produce un error en una operación de sesión, se vuelve a intentar durante 2000 milisegundos antes de tratarse como error. Para que el proveedor de estado de sesión aplique esta lógica de reintento, solo tiene que configurar el tiempo de espera. Cuando se produzca un problema de red, el primer reintento se realizará a los 20 milisegundos, tiempo suficiente en la mayoría de los casos. Después de eso, volverá a intentarlo cada segundo hasta que se agote el tiempo de espera. Justo después del tiempo de espera, volverá a intentarlo para asegurarse de que no se corta el tiempo de espera en un segundo (como máximo).

Si cree que no necesita un reintento o si desea controlar la lógica de reintento personalmente, establezca *retryTimeoutInMilliseconds* en 0. Por ejemplo, es posible que no quiera volver a intentarlo cuando ejecute el servidor de Redis en la misma máquina que la aplicación.

### <a name="about-redisserializertype"></a>Acerca de *redisSerializerType*

De forma predeterminada, la serialización para almacenar los valores en Redis se realiza en un formato binario proporcionado por la clase **BinaryFormatter**. Use *redisSerializerType* para especificar el nombre del tipo calificado de ensamblado de una clase que implemente **Microsoft.Web.Redis.ISerializer** y tenga la lógica personalizada para serializar y deserializar los valores. Por ejemplo, esta es una clase de serializador de JSON con JSON.NET:

```cs
namespace MyCompany.Redis
{
    public class JsonSerializer : ISerializer
    {
        private static JsonSerializerSettings _settings = new JsonSerializerSettings() { TypeNameHandling = TypeNameHandling.All };

        public byte[] Serialize(object data)
        {
            return Encoding.UTF8.GetBytes(JsonConvert.SerializeObject(data, _settings));
        }

        public object Deserialize(byte[] data)
        {
            if (data == null)
            {
                return null;
            }
            return JsonConvert.DeserializeObject(Encoding.UTF8.GetString(data), _settings);
        }
    }
}
```

Supongamos que esta clase se define en un ensamblado con nombre **MyCompanyDll**, puede establecer el parámetro *redisSerializerType* para usarlo:

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvider"
             name = "MySessionStateStore"
             redisSerializerType = "MyCompany.Redis.JsonSerializer,MyCompanyDll"
             ... />
    </providers>
</sessionState>
```

## <a name="output-cache-directive"></a>Directiva de caché de salida

Incorpore una directiva OutputCache a cada página cuyos resultados desea almacenar en caché.

```xml
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

En el ejemplo anterior, los datos de la página almacenados en la memoria caché permanecerán ahí durante 60 segundos y se almacenará en la memoria caché una versión diferente de la página para cada combinación de parámetros. Para obtener más información sobre la directiva OutputCache, consulte [@OutputCache](/previous-versions/dotnet/netframework-4.0/hdxfb6cy(v=vs.100)).

Después de realizar estos pasos, la aplicación está configurada para usar el proveedor de la caché de salida de Redis.

## <a name="third-party-output-cache-providers"></a>Proveedores de caché de resultados de terceros

* [NCache](https://www.alachisoft.com/blogs/how-to-use-a-distributed-cache-for-asp-net-output-cache/)
* [Apache Ignite](https://apacheignite-net.readme.io/docs/aspnet-output-caching)

## <a name="next-steps"></a>Pasos siguientes

Consulte el [proveedor de estado de sesión de ASP.NET para Azure Cache for Redis](cache-aspnet-session-state-provider.md).
