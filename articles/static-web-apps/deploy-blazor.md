---
title: 'Tutorial: Creación de una aplicación web estática con Blazor en Azure Static Web Apps'
description: Aprenda a crear un sitio web con Azure Static Web Apps con Blazor.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 11/08/2021
ms.author: cshoe
ms.openlocfilehash: ac22d5ef720fb0701cb126e6fa0cb627614dd09c
ms.sourcegitcommit: 4cd97e7c960f34cb3f248a0f384956174cdaf19f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "132027173"
---
# <a name="tutorial-building-a-static-web-app-with-blazor-in-azure-static-web-apps"></a>Tutorial: Creación de una aplicación web estática con Blazor en Azure Static Web Apps

Azure Static Web Apps publica sitios web en entornos de producción mediante la creación de aplicaciones desde un repositorio de GitHub. En este tutorial, se implementa una aplicación web en Azure Static Web Apps mediante Azure Portal.

Si no tiene ninguna suscripción a Azure, [cree una cuenta de evaluación gratuita](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Prerrequisitos

- [GitHub](https://github.com)
- Cuenta de [Azure](https://portal.azure.com)

## <a name="application-overview"></a>Información general de la aplicación

Azure Static Web Apps permite crear aplicaciones web estáticas compatibles con un back-end sin servidor. En el siguiente tutorial, se muestra cómo implementar una aplicación WebAssembly de Blazor en C# que muestra los datos meteorológicos devueltos por una API sin servidor.

:::image type="content" source="./media/deploy-blazor/blazor-app-complete.png" alt-text="Aplicación Blazor completa":::

La aplicación que se incluye en este tutorial se compone de tres proyectos de Visual Studio diferentes:

- **API**: la aplicación de Azure Functions en C# que implementa el punto de conexión de API que proporciona información meteorológica a la aplicación WebAssembly de Blazor. **WeatherForecastFunction** devuelve una matriz de objetos `WeatherForecast`.

- **Cliente**: el proyecto WebAssembly de Blazor del servidor front-end. Se implementa una [ruta de reserva](#fallback-route) para asegurarse de que el enrutamiento del lado cliente sea funcional.

- **Shared:** contiene clases comunes a las que hacen referencia los proyectos de API y cliente, lo que permite que los datos fluyan desde el punto de conexión de la API a la aplicación web de front-end. La clase [`WeatherForecast`](https://github.com/staticwebdev/blazor-starter/blob/main/Shared/WeatherForecast.cs) se comparte entre ambas aplicaciones.

Juntos, estos proyectos constituyen los elementos necesarios para crear una aplicación WebAssembly de Blazor que se ejecuta en el explorador con el respaldo de un back-end de API de Azure Functions.

## <a name="fallback-route"></a>Ruta de reserva

La aplicación expone direcciones URL como _/counter_ y _/fetchdata_ que se asignan a rutas específicas de la aplicación. Puesto que esta aplicación se implementa como una aplicación de página única, cada ruta recibe el archivo _index.html_. Para asegurarse de que la solicitud de cualquier ruta de acceso devuelva _index.html_, se implementa una [ruta de reserva](./configuration.md#fallback-routes) en el archivo _staticwebapp.config.json_ que se encuentra en la carpeta raíz del proyecto del cliente.

```json
{
  "navigationFallback": {
    "rewrite": "/index.html"
  }
}
```

La configuración anterior garantiza que las solicitudes a cualquier ruta de la aplicación devuelvan la página _index.html_.

## <a name="create-a-repository"></a>Creación de un repositorio

En este artículo se usa un repositorio de plantillas de GitHub para facilitar los primeros pasos. La plantilla incluye una aplicación de inicio que se puede implementar en Azure Static Web Apps.

1. Asegúrese de que ha iniciado sesión en GitHub y, a continuación, vaya a la siguiente ubicación para crear un repositorio:
   - [https://github.com/staticwebdev/blazor-starter/generate](https://github.com/login?return_to=/staticwebdev/blazor-starter/generate)
1. Asigne el nombre **my-first-static-blazor-app** al repositorio.

## <a name="create-a-static-web-app"></a>Creación de una aplicación web estática

Ahora que se ha creado el repositorio, cree una aplicación web estática desde Azure Portal.

1. Acceda a [Azure Portal](https://portal.azure.com).
1. Seleccione **Crear un recurso**.
1. Busque **Static Web Apps**.
1. Seleccione **Static Web Apps**.
1. Seleccione **Crear**.
1. En la pestaña _Datos básicos_, especifique los valores siguientes.

    | Propiedad | Value |
    | --- | --- |
    | _Suscripción_ | El nombre de la suscripción de Azure. |
    | _Grupos de recursos_ | **my-blazor-group**  |
    | _Nombre_ | **my-first-static-blazor-app** |
    | _Tipo de plan_ | **Gratis** |
    | _Región para la API y los entornos de ensayo de Azure Functions_ | Seleccione la región más cercana a la suya. |
    | _Origen_ | **GitHub** |

1. Seleccione **Iniciar sesión con GitHub** y autentíquese con GitHub.

1. Escriba los siguientes valores de GitHub.

    | Propiedad | Value |
    | --- | --- |
    | _Organización_ | Seleccione la organización de GitHub que quiera. |
    | _Repositorio_ | Seleccione **my-first-static-blazor-app**. |
    | _Rama_ | Seleccione **main** (principal). |

1. En la sección _Detalles de la compilación_, seleccione **Blazor** en la lista desplegable _Valores preestablecidos de compilación_ y se rellenarán los siguientes valores.

    | Propiedad | Value | Descripción |
    | --- | --- | --- |
    | Ubicación de la aplicación | **Cliente** | Carpeta que contiene la aplicación WebAssembly de Blazor |
    | Ubicación de la API | **Api** | Carpeta que contiene la aplicación de Azure Functions |
    | Ubicación del resultado | **wwwroot** | Carpeta de la salida de la compilación que contiene la aplicación WebAssembly de Blazor publicada |
    
### <a name="review-and-create"></a>Revisar y crear

1. Seleccione el botón **Revisar y crear** para comprobar que todos los detalles sean correctos.

1. Seleccione **Crear** para comenzar la creación de la aplicación web estática de App Service y aprovisionar una Acción de GitHub para la implementación.

1. Cuando se complete la implementación, haga clic en **Ir al recurso**.

1. Seleccione **Ir al recurso**.

   :::image type="content" source="media/deploy-blazor/resource-button.png" alt-text="Botón Ir al recurso":::

## <a name="view-the-website"></a>Vista del sitio web

Hay dos aspectos para implementar una aplicación estática. El primero aprovisiona los recursos de Azure subyacentes que componen la aplicación. El segundo es un flujo de trabajo de Acciones de GitHub que compila y publica la aplicación.

Para poder acceder a la nueva aplicación web estática, debe terminar de ejecutarse la compilación de la implementación.

En la ventana Información general de Static Web Apps se muestran una serie de vínculos que le ayudarán a interactuar con la aplicación web.

:::image type="content" source="./media/deploy-blazor/overview-window.png" alt-text="Ventana Información general":::

1. Al hacer clic en el banner con el texto, _Click here to check the status of your GitHub Actions runs_ (Haga clic aquí para comprobar el estado de las ejecuciones de Acciones de GitHub en su repositorio). Una vez que haya verificado que el trabajo de implementación se ha completado, puede ir a su sitio web a través de la URL generada.

2. Una vez completado el flujo de trabajo de Acciones de GitHub, puede seleccionar el vínculo con la _dirección URL_ para abrir el sitio web en una pestaña nueva.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no va a seguir usando esta aplicación, puede eliminar la instancia de Azure Static Web Apps mediante los siguientes pasos:

1. Abra [Azure Portal](https://portal.azure.com).
1. Busque **my-blazor-group en** la barra de búsqueda superior.
1. Seleccione el nombre del grupo.
1. Seleccione el botón **Eliminar**.
1. Seleccione **Sí** para confirmar la acción de eliminación.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Autenticación y autorización](./authentication-authorization.md)
