---
title: Configuración del desarrollo local para Azure Static Web Apps
description: Aprenda a configurar su entorno de desarrollo local para Azure Static Web Apps
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: how-to
ms.date: 10/21/2021
ms.author: cshoe
ms.custom: devx-track-js
ms.openlocfilehash: 7419eea1503d8d0692bd1b112226c8ce626d9776
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130261583"
---
# <a name="set-up-local-development-for-azure-static-web-apps"></a>Configuración del desarrollo local para Azure Static Web Apps

Cuando se publica en la nube, un sitio de Azure Static Web Apps reúne muchos servicios que funcionan juntos como si fueran la misma aplicación. Estos servicios incluyen:

- La aplicación web estática
- API de Azure Functions
- Servicios de autenticación y autorización
- Servicios de enrutamiento y configuración

Estos servicios deben comunicarse entre sí y Azure Static Web Apps controla esta integración en la nube.

Sin embargo, cuando se ejecutan de forma local, estos servicios no se vinculan automáticamente.

Para proporcionar una experiencia similar a la que se obtiene en Azure, la [CLI de Azure Static Web Apps](https://github.com/Azure/static-web-apps-cli) proporciona los siguientes servicios:

- Un servidor de sitio estático local
- Un proxy para el servidor de desarrollo del marco front-end
- Un proxy para los puntos de conexión de API: disponible mediante Azure Functions Core Tools
- Un servidor ficticio de autenticación y autorización
- Aplicación de opciones de configuración y rutas locales

> [!NOTE]
> A menudo, los sitios creados con un marco de front-end requieren que una opción de configuración de proxy controle solicitudes correctamente en la ruta `api`. Al usar la CLI de Azure Static Web Apps el valor de ubicación del proxy es `/api` y, sin la CLI, el valor es `http://localhost:7071/api`.

## <a name="how-it-works"></a>Funcionamiento

En el gráfico siguiente se muestra cómo se administran las solicitudes localmente.

:::image type="content" source="media/local-development/cli-conceptual.png" alt-text="Flujo de solicitud y respuesta de la CLI de Azure Static Web Apps":::

> [!IMPORTANT]
> Vaya a `http://localhost:4280` para acceder a la aplicación suministrada por la CLI.

- Las **solicitudes** realizadas al puerto `4280` se reenvían al servidor adecuado según el tipo de solicitud.

- Las solicitudes de **contenido estático**, como HTML o CSS, se administran mediante el servidor interno de contenido estático de la CLI o el servidor del marco de front-end para la depuración.

- Las solicitudes de **autenticación y autorización** se administran mediante un emulador, que proporciona un perfil de identidad falso a la aplicación.

- El **entorno de ejecución de Functions Core Tools** administra las solicitudes a la API del sitio.

- Las **respuestas** de todos los servicios se devuelven al explorador como si fueran toda ellas una sola aplicación.

## <a name="prerequisites"></a>Requisitos previos

- **Sitio existente de Azure Static Web Apps**: si no tiene una, empiece por la aplicación de inicio [vanilla-api](https://github.com/staticwebdev/vanilla-api/generate?return_to=/staticwebdev/vanilla-api/generate).
- **[Node.js](https://nodejs.org) con npm**: ejecute la versión [LTS de Node.js](https://nodejs.org), que incluye acceso a [npm](https://www.npmjs.com/).
- **[Visual Studio Code](https://code.visualstudio.com/)** : se usa para depurar la aplicación de API, pero no es necesario para la CLI.
- **[Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing)** : se requiere para ejecutar la API de manera local.

## <a name="get-started"></a>Introducción

Abra un terminal en la carpeta raíz del sitio existente de Azure Static Web Apps.

1. Instale la CLI.

    ```console
    npm install -g @azure/static-web-apps-cli
    ```

1. Compile la aplicación si es necesario.

    Ejecute `npm run build` o el comando equivalente para el proyecto.

1. Cambie al directorio de salida de la aplicación. Las carpetas de salida se denominan a menudo _build_ o algo parecido.

1. Inicie la CLI.

    ```console
    swa start
    ```

1. Vaya a `http://localhost:4280` para ver la aplicación en el explorador.

### <a name="other-ways-to-start-the-cli"></a>Otras maneras de iniciar la CLI

| Descripción | Comando |
|--- | --- |
| Servir una carpeta específica | `swa start ./output-folder` |
| Usar un servidor de desarrollo de marco en ejecución | `swa start http://localhost:3000` |
| Iniciar una aplicación de Functions en una carpeta | `swa start ./output-folder --api-location ./api` |
| Usar una aplicación de Functions en ejecución | `swa start ./output-folder --api-location http://localhost:7071` |

## <a name="authorization-and-authentication-emulation"></a>Emulación de autorización y autenticación

La CLI de Static Web Apps emula el [flujo de seguridad](./authentication-authorization.md) implementado en Azure. Cuando un usuario inicia sesión, puede definir un perfil de identidad falso que se devuelva a la aplicación.

Por ejemplo, al intentar ir a `/.auth/login/github`, se devuelve una página que le permite definir un perfil de identidad.

> [!NOTE]
> El emulador funciona con cualquier proveedor de seguridad, no solo con GitHub.

:::image type="content" source="media/local-development/auth-emulator.png" alt-text="Funcionamiento de la autenticación y la autorización":::

El emulador ofrece una página que le permite proporcionar los siguientes valores de [entidad de seguridad del cliente](./user-information.md#client-principal-data):

| Value | Descripción |
| --- | --- |
| **Nombre de usuario** | Nombre de cuenta asociado al proveedor de seguridad. Este valor aparece como la propiedad `userDetails` en la entidad de seguridad del cliente y se genera automáticamente si no se proporciona ningún valor. |
| **Id. de usuario** | Valor generado automáticamente por la CLI.  |
| **Roles** | Una lista de nombres de rol, donde cada nombre está en una nueva línea.  |

Una vez que ha iniciado sesión:

- Puede usar el punto de conexión `/.auth/me` o un punto de conexión de función para recuperar la [entidad de seguridad del cliente](./user-information.md) del usuario.

- Al desplazarse a `/.auth/logout` se borra la entidad de seguridad del cliente y se cierra la sesión del usuario ficticio.

## <a name="debugging"></a>Depuración

En una aplicación web estática, hay dos contextos de depuración. El primero es para el sitio de contenido estático y el segundo es para las funciones de API. La depuración local es posible al permitir que la CLI de Static Web Apps use servidores de desarrollo para uno o ambos contextos.

En los pasos siguientes se muestra un escenario común en el que se emplean servidores de desarrollo para los contextos de depuración.

1. Inicie el servidor de desarrollo de sitio estático. Este comando es específico del marco de front-end que se usa, pero a menudo se incluye en forma de comandos como `npm run build`, `npm start` o `npm run dev`.

1. Abra la carpeta de la aplicación de API en Visual Studio Code e inicie una sesión de depuración.

1. Inicie la CLI de Static Web Apps mediante el comando siguiente.


    ```console
    swa start http://localhost:<DEV-SERVER-PORT-NUMBER> --api-location http://localhost:7071
    ```

    Reemplace `<DEV-SERVER-PORT-NUMBER>` por el número de puerto del servidor de desarrollo.

Las siguientes capturas de pantallas muestran los terminales en un escenario de depuración típico:

El sitio de contenido estático se está ejecutando mediante `npm run dev`.

:::image type="content" source="media/local-development/run-dev-static-server.png" alt-text="Servidor de desarrollo de sitio estático":::

La aplicación de API de Azure Functions ejecuta una sesión de depuración en Visual Studio Code.

:::image type="content" source="media/local-development/visual-studio-code-debugging.png" alt-text="Depuración con la API de Visual Studio Code":::

La CLI de Static Web Apps se inicia con ambos servidores de desarrollo.

:::image type="content" source="media/local-development/static-web-apps-cli-terminal.png" alt-text="Terminal de la CLI de Azure Static Web Apps":::

Ahora, las solicitudes que pasan por el puerto `4280` se enrutan al servidor de desarrollo de contenido estático o a la sesión de depuración de la API.

Para más información sobre los distintos escenarios de depuración, con instrucciones sobre cómo personalizar los puertos y las direcciones del servidor, consulte el [repositorio de la CLI de Azure Static Web Apps](https://github.com/Azure/static-web-apps-cli).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración de la aplicación](configuration.md)
