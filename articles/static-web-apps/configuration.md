---
title: Configuración de Azure Static Web Apps
description: Aprenda a configurar rutas, a aplicar reglas de seguridad y a utilizar la configuración global de Azure Static Web Apps.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 08/27/2021
ms.author: cshoe
ms.openlocfilehash: 25358033e88fbfb1590289e6fbad8e4109e0dbbf
ms.sourcegitcommit: 03e84c3112b03bf7a2bc14525ddbc4f5adc99b85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2021
ms.locfileid: "129399591"
---
# <a name="configure-azure-static-web-apps"></a>Configuración de Azure Static Web Apps

La configuración de Azure Static Web Apps se define en el archivo _staticwebapp.config.json_, que controla las siguientes opciones:

- Enrutamiento
- Authentication
- Authorization
- Reglas de reserva
- Invalidación de respuestas HTTP
- Definiciones de encabezados HTTP globales
- Tipos MIME personalizados
- Redes

> [!NOTE]
> El archivo [_routes.json_](https://github.com/Azure/static-web-apps/wiki/routes.json-reference-(deprecated)) que se usó anteriormente para configurar el enrutamiento está en desuso. Use _staticwebapp.config.json_ como se describe en este artículo para configurar el enrutamiento y otras opciones para la aplicación web estática.
> 
> Este documento es sobre Azure Static Web Apps, que es un producto independiente de la característica de [hospedaje de sitios web estáticos](../storage/blobs/storage-blob-static-website.md) de Azure Storage.

## <a name="file-location"></a>Ubicación del archivo

La ubicación recomendada del archivo _staticwebapp.config.jsen_ es la carpeta establecida como `app_location` en el [archivo de flujo de trabajo](./build-configuration.md). Sin embargo, el archivo puede colocarse en cualquier subcarpeta dentro del conjunto de carpetas como `app_location`.

Consulte el [archivo de configuración de ejemplo](#example-configuration-file) para ver los detalles.

> [!IMPORTANT]
> El archivo [_routes.json_ en desuso](https://github.com/Azure/static-web-apps/wiki/routes.json-reference-(deprecated)) se omite si existe un archivo _staticwebapp.config.json_.

## <a name="routes"></a>Rutas

Las reglas de rutas permiten definir el patrón de direcciones URL que proporcionan acceso a la aplicación en la web. Las rutas se definen como una matriz de reglas de enrutamiento. Consulte el [archivo de configuración de ejemplo](#example-configuration-file) para ver cómo se usan.

- Las reglas se definen en la matriz `routes`, aunque solo tenga una ruta.
- Las reglas se ejecutan en el orden en que aparecen en la matriz `routes`.
- La evaluación de la regla se detiene en la primera coincidencia, ya que las reglas de enrutamiento no están encadenadas.
- Tiene control total sobre los nombres de los roles personalizados.
  - Hay algunos nombres de roles integrados, como [`anonymous`](./authentication-authorization.md) y [`authenticated`](./authentication-authorization.md).

Los aspectos relacionados con el enrutamiento se superponen en buena parte a los conceptos de autenticación (identificación del usuario) y autorización (concesión de capacidades al usuario). Asegúrese de leer la guía de [autenticación y autorización](authentication-authorization.md) junto con este artículo.

El archivo predeterminado para el contenido estático es _index.html_.

## <a name="defining-routes"></a>Definición de rutas

Cada una se compone de un patrón de ruta, junto con una o varias de las propiedades de regla opcionales. Las reglas de ruta se definen en la matriz `routes`. Consulte el [archivo de configuración de ejemplo](#example-configuration-file) para ver cómo se usan.

| Propiedad de regla | Obligatorio | Valor predeterminado | Comentario |
|--|--|--|--|
| `route` | Sí | N/D | Patrón de ruta solicitado por el autor de la llamada.<ul><li>Los [caracteres comodín](#wildcards) se admiten al final de las rutas.<ul><li>Por ejemplo, la ruta _admin/\*_ coincide con cualquier ruta que se encuentre en la ruta de acceso _admin_.</ul></ul> |
| `rewrite` | No | N/D | Define el archivo o la ruta de acceso que devuelve la solicitud.<ul><li>Es mutuamente excluyente con las reglas `redirect`<li>Las reglas de reescritura no cambian la ubicación del explorador.<li>Los valores deben hacer referencia a la raíz de la aplicación.</ul> |
| `redirect` | No | N/D | Define el destino de redireccionamiento del archivo o la ruta de acceso de una solicitud.<ul><li>Es mutuamente excluyente con las reglas `rewrite`<li>Las reglas de redireccionamiento cambian la ubicación del explorador.<li>El código de respuesta predeterminado es [`302`](https://developer.mozilla.org/docs/Web/HTTP/Status/302) (redireccionamiento temporal), pero puede invalidarse con [`301`](https://developer.mozilla.org/docs/Web/HTTP/Status/301) (redirección permanente).</ul> |
| `allowedRoles` | No | anonymous | Define una lista de nombres de rol necesarios para acceder a una ruta. <ul><li>Los caracteres válidos incluyen `a-z`, `A-Z`, `0-9` y `_`.<li>El rol integrado, [`anonymous`](./authentication-authorization.md), se aplica a todos los usuarios sin autenticar.<li>El rol integrado, [`authenticated`](./authentication-authorization.md), se aplica a todos los usuarios que han iniciado sesión.<li>Los usuarios deben pertenecer al menos a un rol.<li>Los roles se evalúan según el operador _OR_.<ul><li>Si un usuario tiene cualquiera de los roles enumerados, se le concede acceso.</ul><li>Los usuarios individuales se asocian a los roles a través de [invitaciones](authentication-authorization.md).</ul> |
| `headers`<a id="route-headers"></a> | No | N/D | Conjunto de [encabezados HTTP](https://developer.mozilla.org/docs/Web/HTTP/Headers) que se agregan a la respuesta. <ul><li>Los encabezados específicos de la ruta invalidan los encabezados [`globalHeaders`](#global-headers) cuando el encabezado específico de la ruta es igual que el encabezado global de la respuesta.<li>Para quitar un encabezado, establezca el valor en una cadena vacía.</ul> |
| `statusCode` | No | `200`, `301` o `302` para las redirecciones | [Código de estado HTTP](https://developer.mozilla.org/docs/Web/HTTP/Status) de la respuesta. |
| `methods` | No | Todos los métodos | Lista de los métodos de solicitud que coinciden con una ruta. Algunos de los métodos disponibles son: `GET`, `HEAD`, `POST`, `PUT`, `DELETE`, `CONNECT`, `OPTIONS`, `TRACE` y `PATCH`. |

Cada propiedad tiene un propósito específico en la canalización de solicitudes o respuestas.

| Propósito | Propiedades |
|--|--|
| Establecer correspondencias en las rutas | `route`, `methods` |
| Proporcionar autorización después de encontrar la correspondencia de una ruta | `allowedRoles` |
| Realizar los procesos correspondientes una vez que se cumple y se autoriza una regla | `rewrite` (modifica la solicitud) <br><br>`redirect`, `headers` y `statusCode` (modifica la respuesta) |

## <a name="securing-routes-with-roles"></a>Protección de rutas mediante roles

Las rutas están protegidas mediante la adición de uno o varios nombres de rol a la matriz `allowedRoles` de una regla. Consulte el [archivo de configuración de ejemplo](#example-configuration-file) para ver cómo se usan.

De forma predeterminada, todos los usuarios pertenecen al rol de `anonymous` integrado, y todos los usuarios que han iniciado sesión son miembros del rol `authenticated`. Opcionalmente, los usuarios están asociados a roles personalizados a través de [invitaciones](./authentication-authorization.md).

Por ejemplo, para restringir una ruta solo a los usuarios autenticados, agregue el rol `authenticated` integrado a la matriz `allowedRoles`.

```json
{
  "route": "/profile",
  "allowedRoles": ["authenticated"]
}
```

Puede crear nuevos roles según sea necesario en la matriz `allowedRoles`. Para restringir una ruta exclusivamente a los administradores, puede definir un rol propio llamado `administrator` en la matriz `allowedRoles`.

```json
{
  "route": "/admin",
  "allowedRoles": ["administrator"]
}
```

- Tiene control total sobre los nombres de rol, ya que no hay ninguna lista que los roles deban seguir.
- Los usuarios individuales se asocian a los roles a través de [invitaciones](authentication-authorization.md).

## <a name="wildcards"></a>Caracteres comodín

Las reglas de caracteres comodín coinciden con todas las solicitudes de un patrón de ruta, solo se admiten al final de una ruta de acceso y se pueden filtrar por extensión de archivo. Consulte el [archivo de configuración de ejemplo](#example-configuration-file) para ver cómo se usan.

Por ejemplo, si desea implementar las rutas de una aplicación de calendario, puede reescribir todas las direcciones URL que se encuentran en la ruta de _calendar_ para que proporcionen un único archivo.

```json
{
  "route": "/calendar/*",
  "rewrite": "/calendar.html"
}
```

El archivo _calendar.html_ puede usar el enrutamiento del lado cliente para generar una vista diferente para las variantes de las direcciones URL, como `/calendar/january/1`, `/calendar/2020` y `/calendar/overview`.

Puede filtrar las coincidencias con caracteres comodín por la extensión de archivo. Por ejemplo, si desea agregar una regla que solo coincida con los archivos HTML de una ruta de acceso determinada, puede crear la siguiente regla:

```json
{
  "route": "/articles/*.html",
  "headers": {
    "Cache-Control": "public, max-age=604800, immutable"
  }
}
```

Para filtrar por varias extensiones de archivo, incluya las opciones entre llaves, tal y como se muestra en este ejemplo:

```json
{
  "route": "/images/thumbnails/*.{png,jpg,gif}",
  "headers": {
    "Cache-Control": "public, max-age=604800, immutable"
  }
}
```

Algunos de los casos de uso de las rutas con caracteres comodín más frecuentes son:

- Proporcionar un archivo específico para todo un patrón de rutas de acceso
- Asignar distintos métodos HTTP a todo un patrón de rutas de acceso
- Aplicar reglas de autenticación y autorización
- Implementar reglas de almacenamiento en caché especializadas

## <a name="fallback-routes"></a>Rutas de reserva

Las aplicaciones de una sola página suelen utilizar el enrutamiento del lado cliente. Estas reglas de enrutamiento del lado cliente actualizan la ubicación de la ventana del explorador sin realizar solicitudes al servidor. Si actualiza la página o va directamente a las direcciones URL generadas por las reglas de enrutamiento del lado cliente, necesitará una ruta de reserva en el lado servidor para proporcionar la página HTML adecuada (que normalmente será el archivo _index.html_ de la aplicación del lado cliente).

Puede definir una regla de reserva agregando una sección `navigationFallback`. En el siguiente ejemplo se devuelve _/index.html_ para todas las solicitudes de archivos estáticos que no coinciden con un archivo implementado.

```json
{
  "navigationFallback": {
    "rewrite": "/index.html"
  }
}
```

Puede controlar qué solicitudes devuelven el archivo de reserva definiendo un filtro. En el siguiente ejemplo, las solicitudes de determinadas rutas en la carpeta _/images_ y todos los archivos de la carpeta _/css_ están excluidos de la devolución del archivo de reserva.

```json
{
  "navigationFallback": {
    "rewrite": "/index.html",
    "exclude": ["/images/*.{png,jpg,gif}", "/css/*"]
  }
}
```

En la estructura de archivos siguiente, se pueden conseguir los resultados que se indican a continuación con esta regla.

```files
├── images
│   ├── logo.png
│   ├── headshot.jpg
│   └── screenshot.gif
│
├── css
│   └── global.css
│
└── index.html
```

| Enviar una solicitud a... | devuelve... | con el estado... |
|--|--|--|
| _/about/_ | El archivo _index.html_ | `200` |
| _/images/logo.png_ | El archivo de imágenes | `200` |
| _/images/icon.svg_ | El archivo _/index.html_, ya que la extensión de archivo _svg_ no aparece en el filtro `/images/*.{png,jpg,gif}` | `200` |
| _/images/unknown.png_ | Error de archivo no encontrado | `404` |
| _/css/unknown.css_ | Error de archivo no encontrado | `404` |
| _/css/global.css_ | Archivo de hoja de estilos | `200` |
| Cualquier otro archivo que esté fuera de las carpetas _/images_ o _/css_ | El archivo _index.html_ | `200` |

> [!IMPORTANT]
> Si va a migrar desde el archivo [_routes.json_](https://github.com/Azure/static-web-apps/wiki/routes.json-reference-(deprecated)) en desuso, no incluya la ruta de reserva heredada (`"route": "/*"`) en las [reglas de enrutamiento](#routes).

## <a name="global-headers"></a>Encabezados globales

En la sección `globalHeaders` se proporciona un conjunto con los [encabezados HTTP](https://developer.mozilla.org/docs/Web/HTTP/Headers) que se aplican a cada respuesta, a menos que queden invalidados por una regla de [encabezado de ruta](#route-headers). De lo contrario, se devuelve la unión de los dos encabezados de la ruta y los encabezados globales.

Consulte el [archivo de configuración de ejemplo](#example-configuration-file) para ver cómo se usan.

Para quitar un encabezado, establezca el valor en una cadena vacía (`""`).

Algunos casos de uso comunes de los encabezados globales son:

- Reglas de almacenamiento en caché personalizadas
- Aplicación de directivas de seguridad
- Configuración de la codificación

## <a name="response-overrides"></a>Invalidación de respuestas

En la sección `responseOverrides`, se puede definir una respuesta personalizada en lugar de que el servidor devuelva un código de error. Consulte el [archivo de configuración de ejemplo](#example-configuration-file) para ver cómo se usan.

Los siguientes códigos HTTP se pueden reemplazar:

| Código de estado | Significado | Causa posible |
|--|--|--|
| [400](https://developer.mozilla.org/docs/Web/HTTP/Status/400) | Solicitud incorrecta | Vínculo de invitación no válido |
| [401](https://developer.mozilla.org/docs/Web/HTTP/Status/401) | No autorizado | Solicitud a páginas restringidas mientras no se está autenticado |
| [403](https://developer.mozilla.org/docs/Web/HTTP/Status/403) | Prohibido | <ul><li>El usuario ha iniciado sesión, pero no tiene los roles necesarios para ver la página.<li>El usuario ha iniciado sesión, pero el entorno de ejecución no puede obtener los detalles del usuario a partir de sus notificaciones de identidad.<li>Hay demasiados usuarios que han iniciado sesión en el sitio con roles personalizados, por lo que el entorno de ejecución no puede iniciar la sesión del usuario.</ul> |
| [404](https://developer.mozilla.org/docs/Web/HTTP/Status/404) | No encontrado | Archivo no encontrado |

En la configuración del ejemplo siguiente, se muestra cómo invalidar un código de error.

```json
{
  "responseOverrides": {
    "400": {
      "rewrite": "/invalid-invitation-error.html"
    },
    "401": {
      "statusCode": 302,
      "redirect": "/login"
    },
    "403": {
      "rewrite": "/custom-forbidden-page.html"
    },
    "404": {
      "rewrite": "/custom-404.html"
    }
  }
}
```

## <a name="networking"></a>Redes

La sección `networking` controla la configuración de red de su aplicación web estática. Para restringir el acceso a su aplicación, especifique una lista de bloques de direcciones IP permitidos en `allowedIpRanges`.

> [!NOTE]
> La configuración de red solo está disponible en el plan Estándar de Azure Static Web Apps.

Defina cada bloque de direcciones IPv4 en la notación Enrutamiento de interdominios sin clases (CIDR). Para más información acerca de la notación CIDR, consulte [Enrutamiento de interdominios sin clases](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). Cada bloque de direcciones IPv4 puede indicar un espacio de direcciones público o privado. Si solo desea permitir el acceso desde una sola dirección IP, puede usar el bloque CIDR `/32`.

```json
{
  "networking": {
    "allowedIpRanges": [
      "10.0.0.0/24",
      "100.0.0.0/32",
      "192.168.100.0/22"
    ]
  }
}
```

Cuando se especifican uno o más bloques de direcciones IP, a las solicitudes que se originan desde direcciones IP que no coinciden con un valor en `allowedIpRanges` se les deniega el acceso.

Además de los bloques de direcciones IP, también puede especificar [etiquetas de servicio](../virtual-network/service-tags-overview.md) en la matriz `allowedIpRanges` para restringir el tráfico a determinados servicios de Azure.

```json
"networking": {
  "allowedIpRanges": ["AzureFrontDoor.Backend"]
}
```

## <a name="authentication"></a>Authentication

* Los [proveedores de autenticación predeterminados](authentication-authorization.md#login) no necesitan valores en el archivo de configuración. 
* Los [proveedores de autenticación personalizados](authentication-custom.md) usan la sección `auth` del archivo de configuración.

## <a name="forwarding-gateway"></a>Puerta de enlace de reenvío

En la sección `forwardingGateway` se configura cómo se accede a una aplicación web estática desde una puerta de enlace de reenvío, como un CDN o Azure Front Door.

> [!NOTE]
> La configuración de la puerta de enlace de reenvío solo está disponible en el plan estándar de Azure Static Web Apps.

### <a name="allowed-forwarded-hosts"></a>Hosts reenviados permitidos
  
La lista `allowedForwardedHosts` especifica qué nombres de host se aceptan en el encabezado [X-Forwarded-Host](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Forwarded-Host). Si un dominio que coincide está en la lista, Static Web Apps usa el valor `X-Forwarded-Host` al construir direcciones URL de redireccionamiento, después de realizar un inicio de sesión correcto.

Para que Static Web Apps funcione correctamente detrás de una puerta de enlace de reenvío, la solicitud de la puerta de enlace debe incluir el nombre de host correcto en el encabezado `X-Forwarded-Host` y el mismo nombre de host debe aparecer enumerado en `allowedForwardedHosts`.

```json
"forwardingGateway": {
  "allowedForwardedHosts": [
    "example.org",
    "www.example.org",
    "staging.example.org"
  ]
}
```

Si el encabezado `X-Forwarded-Host` no coincide con un valor de la lista, las solicitudes siguen siendo correctas, pero el encabezado no se usa en la respuesta.

### <a name="required-headers"></a>Encabezados obligatorios

Los encabezados obligatorios son encabezados HTTP que se deben enviar con cada solicitud al sitio. Un uso de los encabezados necesarios es denegar el acceso a un sitio a menos que todos los encabezados necesarios estén presentes en cada solicitud.

Por ejemplo, la configuración siguiente muestra cómo puede agregar un identificador único para [Azure Front Door](../frontdoor/front-door-overview.md) que limite el acceso al sitio desde una instancia de Azure Front Door específica. Consulte el [tutorial de configuración de Azure Front Door](front-door-manual.md) para obtener todos los detalles.

```json
"forwardingGateway": {
  "requiredHeaders": {
    "X-Azure-FDID" : "692a448c-2b5d-4e4d-9fcc-2bc4a6e2335f"
  }
}
```

- Los pares clave-valor pueden ser cualquier conjunto de cadenas arbitrarias.
- Las claves no distinguen mayúsculas de minúsculas.
- Los valores distinguen mayúsculas de minúsculas.

## <a name="example-configuration-file"></a>Archivos de configuración de ejemplo

```json
{
  "routes": [
    {
      "route": "/profile",
      "allowedRoles": ["authenticated"]
    },
    {
      "route": "/admin/*",
      "allowedRoles": ["administrator"]
    },
    {
      "route": "/images/*",
      "headers": {
        "cache-control": "must-revalidate, max-age=15770000"
      }
    },
    {
      "route": "/api/*",
      "methods": ["GET"],
      "allowedRoles": ["registeredusers"]
    },
    {
      "route": "/api/*",
      "methods": ["PUT", "POST", "PATCH", "DELETE"],
      "allowedRoles": ["administrator"]
    },
    {
      "route": "/api/*",
      "allowedRoles": ["authenticated"]
    },
    {
      "route": "/customers/contoso",
      "allowedRoles": ["administrator", "customers_contoso"]
    },
    {
      "route": "/login",
      "rewrite": "/.auth/login/github"
    },
    {
      "route": "/.auth/login/twitter",
      "statusCode": 404
    },
    {
      "route": "/logout",
      "redirect": "/.auth/logout"
    },
    {
      "route": "/calendar/*",
      "rewrite": "/calendar.html"
    },
    {
      "route": "/specials",
      "redirect": "/deals",
      "statusCode": 301
    }
  ],
  "navigationFallback": {
    "rewrite": "index.html",
    "exclude": ["/images/*.{png,jpg,gif}", "/css/*"]
  },
  "responseOverrides": {
    "400": {
      "rewrite": "/invalid-invitation-error.html"
    },
    "401": {
      "redirect": "/login",
      "statusCode": 302
    },
    "403": {
      "rewrite": "/custom-forbidden-page.html"
    },
    "404": {
      "rewrite": "/404.html"
    }
  },
  "globalHeaders": {
    "content-security-policy": "default-src https: 'unsafe-eval' 'unsafe-inline'; object-src 'none'"
  },
  "mimeTypes": {
    ".json": "text/json"
  }
}
```

Consulte los escenarios siguientes teniendo en cuenta la configuración anterior.

| Enviar una solicitud a... | el resultado es... |
|--|--|
| _/profile_ | Los usuarios autenticados reciban el archivo _/profile/index.html_. Los usuarios sin autenticar acceden automáticamente a _/login_. |
| _/admin/_ | Se proporciona el archivo _/admin/reports/index.html_ a los usuarios autenticados con el rol _administrator_. Los usuarios autenticados que no tienen el rol _administrator_ reciben un error`403`.<sup>1</sup> Los usuarios sin autenticar acceden automáticamente a _/login_. |
| _/logo.png_ | Proporciona a la imagen una regla de caché personalizada en la que la antigüedad máxima es un poco superior a 182 días (15 770 000 segundos). |
| _/api/admin_ | Las solicitudes `GET` de los usuarios autenticados con el rol _registeredusers_ se envían a la API. Los usuarios autenticados que no tienen el rol _registeredusers_ y los que no se han autenticado reciben un error `401`.<br/><br/>Las solicitudes `POST`, `PUT`, `PATCH` y `DELETE` de los usuarios autenticados con el rol _administrator_ se envían a la API. Los usuarios autenticados que no tienen el rol _administrator_ y los que no se han autenticado reciben un error `401`. |
| _/customers/contoso_ | Los usuarios autenticados que pertenecen a los roles _Administrador_ o _customers_contoso_ reciben el archivo _/customers/contoso/index.html_. Los usuarios autenticados que no tienen los roles _Administrador_ o _customers_contoso_ reciben un error `403`<sup>1</sup>. Los usuarios sin autenticar acceden automáticamente a _/login_. |
| _/login_ | Se pide a los usuarios sin autenticar que lo hagan con GitHub. |
| _/.auth/login/twitter_ | Como la autorización con Twitter está deshabilitada por la regla de ruta, se devuelve el error `404`, que vuelve a proporcionar el archivo _/index.html_ con el código de estado `200`. |
| _/logout_ | Se cierra la sesión de los usuarios con cualquier proveedor de autenticación. |
| _/calendar/2021/01_ | El explorador proporciona el archivo _/calendar.html_. |
| _/specials_ | El explorador accede de forma automática y permanente a _/deals_. |
| _/data.json_ | El archivo se proporciona con el tipo MIME `text/json`. |
| _/about_ o cualquier carpeta que coincida con los patrones de enrutamiento del lado cliente | El archivo _/index.html_ se proporciona con el código de estado `200`. |
| Archivo que no existe en la carpeta _/images/_ | Error `404`. |

<sup>1</sup> Puede proporcionar una página de error personalizada utilizando una [regla de invalidación de respuestas](#response-overrides).

## <a name="restrictions"></a>Restricciones

El archivo _staticwebapps.config.json_ tiene las siguientes restricciones.

- El tamaño máximo del archivo es 100 KB.
- Hay 50 roles distintos como máximo.

Consulte el [artículo sobre cuotas](quotas.md) para conocer las restricciones y limitaciones generales.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de la autenticación y autorización](authentication-authorization.md)
