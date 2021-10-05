---
title: Configuración de aplicaciones de Node.js
description: Aprenda a configurar una aplicación de Node.js en las instancias nativas de Windows, o en un contenedor de Linux pregenerado, en Azure App Service. En este artículo se muestran las tareas de configuración más comunes.
ms.custom: devx-track-js, devx-track-azurecli
ms.devlang: nodejs
ms.topic: article
ms.date: 04/23/2021
zone_pivot_groups: app-service-platform-windows-linux
ms.openlocfilehash: 8f65fff40419ef11dcb5d90c670d10573a114fd4
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128657070"
---
# <a name="configure-a-nodejs-app-for-azure-app-service"></a>Configuración de una aplicación de Node.js para Azure App Service

Las aplicaciones de Node.js deben implementarse con todas las dependencias de NPM. El motor de implementación de App Service ejecuta `npm install --production` automáticamente al implementar un [repositorio de Git](deploy-local-git.md) o un [paquete Zip](deploy-zip.md) [con la automatización de compilaciones habilitada](deploy-zip.md#enable-build-automation-for-zip-deploy). Sin embargo, si implementa los archivos con [FTP/S](deploy-ftp.md), deberá cargar manualmente los paquetes necesarios.

Esta guía incluye conceptos clave e instrucciones para los desarrolladores de Node.js que realizan implementaciones en App Service. Si nunca ha usado Azure App Service, siga primero la [guía de inicio rápido de Node.js](quickstart-nodejs.md) y el [tutorial de Node.js con MongoDB](tutorial-nodejs-mongodb-app.md).

## <a name="show-nodejs-version"></a>Mostrar la versión de Node.js

::: zone pivot="platform-windows"  

Para mostrar la versión actual de Node.js, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config appsettings list --name <app-name> --resource-group <resource-group-name> --query "[?name=='WEBSITE_NODE_DEFAULT_VERSION'].value"
```

Para mostrar todas las versiones compatibles de Node.js, vaya a `https://<sitename>.scm.azurewebsites.net/api/diagnostics/runtime` o ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes | grep node
```

::: zone-end

::: zone pivot="platform-linux"

Para mostrar la versión actual de Node.js, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Para mostrar todas las versiones compatibles de Node.js, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep NODE
```

::: zone-end

## <a name="set-nodejs-version"></a>Establecer la versión de Node.js

::: zone pivot="platform-windows"  

Para establecer la aplicación en una [versión compatible de Node.js](#show-nodejs-version), ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com) para configurar `WEBSITE_NODE_DEFAULT_VERSION` en una versión compatible:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_NODE_DEFAULT_VERSION="10.15"
```

Esta configuración especifica la versión de Node.js que usa, tanto en tiempo de ejecución como durante la restauración automatizada de paquetes durante la automatización de compilaciones de App Service. Esta configuración solo reconoce las versiones secundarias principales, no se admite el moniker _LTS_.

> [!NOTE]
> Debe establecer la versión de Node.js en el archivo `package.json` del proyecto. El motor de implementación se ejecuta en un proceso independiente que contiene todas las versiones compatibles de Node.js.

::: zone-end

::: zone pivot="platform-linux"

Para establecer la aplicación en una [versión compatible de Node.js](#show-nodejs-version), ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "NODE|10.14"
```

Esta configuración especifica la versión de Node.js que usa, tanto en tiempo de ejecución como durante la restauración automatizada de paquetes en Kudu.

> [!NOTE]
> Debe establecer la versión de Node.js en el archivo `package.json` del proyecto. El motor de implementación se ejecuta en un contenedor independiente que contiene todas las versiones compatibles de Node.js.

::: zone-end

## <a name="get-port-number"></a>Obtención del número de puerto

La aplicación de Node.js debe escuchar al puerto correcto para recibir las solicitudes entrantes.

::: zone pivot="platform-windows"  

En App Service de Windows, las aplicaciones de Node.js se hospedan con [IISNode](https://github.com/Azure/iisnode) y la aplicación de Node.js debe escuchar el puerto especificado en la variable `process.env.PORT`. En el ejemplo siguiente se muestra cómo hacerlo en una sencilla aplicación Rápida:

::: zone-end

::: zone pivot="platform-linux"  

App Service establece la variable de entorno `PORT` en el contenedor de Node.js y reenvía las solicitudes entrantes al contenedor en ese número de puerto. Para recibir las solicitudes, la aplicación debe escuchar ese puerto mediante `process.env.PORT`. En el ejemplo siguiente se muestra cómo hacerlo en una sencilla aplicación Rápida:

::: zone-end

```javascript
const express = require('express')
const app = express()
const port = process.env.PORT || 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})
```

::: zone pivot="platform-linux"

## <a name="customize-build-automation"></a>Personalización de la automatización de compilaciones

Si implementa la aplicación mediante paquetes Git o zip [con la automatización de compilaciones activada](deploy-zip.md#enable-build-automation-for-zip-deploy), la automatización de compilaciones de App Service se ejecutará en este orden:

1. Ejecute el script personalizado si lo especifica `PRE_BUILD_SCRIPT_PATH`.
1. Ejecute `npm install` sin marcas, lo que incluye los scripts de npm `preinstall` y `postinstall` y también instala `devDependencies`.
1. Ejecute `npm run build` si se especifica un script de compilación en *package.json*.
1. Ejecute `npm run build:azure` si se especifica un script build:azure en *package.json*.
1. Ejecute el script personalizado si lo especifica `POST_BUILD_SCRIPT_PATH`.

> [!NOTE]
> Como se describe en la [documentación de npm](https://docs.npmjs.com/misc/scripts), los scripts denominados `prebuild` y `postbuild` se ejecutan antes y después de `build`, respectivamente, si se especifican. `preinstall` y `postinstall` ejecutan antes y después de `install`, respectivamente.

`PRE_BUILD_COMMAND` y `POST_BUILD_COMMAND` son variables de entorno que, de forma predeterminada, están vacías. Para ejecutar comandos anteriores a la compilación, defina `PRE_BUILD_COMMAND`. Para ejecutar comandos posteriores a la compilación, defina `POST_BUILD_COMMAND`.

En el ejemplo siguiente se especifican las dos variables para una serie de comandos, separados por comas.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Para obtener más variables de entorno para personalizar la automatización de compilaciones, consulte la [configuración de Oryx](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

Para más información sobre cómo se ejecuta App Service y se compilan aplicaciones de Node.js en Linux, consulte la [documentación de Oryx sobre cómo se detectan y se compilan las aplicaciones de Node.js](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/nodejs.md).

## <a name="configure-nodejs-server"></a>Configurar el servidor Node.js

Los contenedores de Node.js se suministran con [PM2](https://pm2.keymetrics.io/), un administrador de procesos de producción. Puede configurar la aplicación para que se inicie con PM2, con NPM o con un comando personalizado.

|Herramienta|Propósito|
|--|--|
|[Ejecutar con PM2](#run-with-pm2)|**Recomendado:** uso en producción o ensayo. PM2 proporciona una plataforma de administración de aplicaciones de servicio completo.|
|[Ejecutar npm start](#run-npm-start)|Solo en entornos de desarrollo.|
|[Ejecutar un comando personalizado](#run-custom-command)|Entornos de desarrollo o ensayo.|


### <a name="run-with-pm2"></a>Ejecutar con PM2

El contenedor inicia automáticamente la aplicación con PM2 cuando se encuentra uno de los archivos comunes de Node.js en el proyecto:

- *bin/www*
- *server.js*
- *app.js*
- *index.js*
- *hostingstart.js*
- Uno de los siguientes [archivos PM2](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file): *process.json* y *ecosystem.config.js*

También puede configurar un archivo de inicio personalizado con las siguientes extensiones:

- Un archivo *.js*
- Un [archivo PM2](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file) con la extensión *.json*, *. config.js*, *.yaml* o *.yml*

Para agregar un archivo de inicio personalizado, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filname-with-extension>"
```

### <a name="run-custom-command"></a>Ejecutar un comando personalizado

App Service puede iniciar la aplicación con un comando personalizado, por ejemplo, un archivo ejecutable como *run.sh*. Por ejemplo, para ejecutar `npm run start:prod`, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "npm run start:prod"
```

### <a name="run-npm-start"></a>Ejecutar npm start

Para iniciar la aplicación con `npm start`, asegúrese de que haya un script `start` en el archivo *package.json*. Por ejemplo:

```json
{
  ...
  "scripts": {
    "start": "gulp",
    ...
  },
  ...
}
```

Para utilizar un archivo *package.json* personalizado en el proyecto, ejecute el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filename>.json"
```


## <a name="debug-remotely"></a>Depuración remota

> [!NOTE]
> La depuración remota se encuentra actualmente en versión preliminar.

Puede depurar la aplicación de Node.js de manera remota en [Visual Studio Code](https://code.visualstudio.com/) si la configura para [ejecutar con PM2](#run-with-pm2), excepto cuando se ejecuta con una extensión *.config.js, *.yml o *.yaml*.

En la mayoría de los casos, no es necesaria ninguna configuración adicional para la aplicación. Si la aplicación se ejecuta con un archivo *process.json* (predeterminado o personalizado), debe tener una `script` propiedad en la raíz JSON. Por ejemplo:

```json
{
  "name"        : "worker",
  "script"      : "./index.js",
  ...
}
```

Para configurar Visual Studio Code para la depuración remota, instale la [extensión App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Siga las instrucciones que aparecen en la página de la extensión e inicie sesión en Azure en Visual Studio Code.

En el Explorador de Azure, busque la aplicación que desea depurar, haga clic en ella y seleccione **Start Remote Debugging** (Iniciar depuración remota). Haga clic en **Sí** para habilitarla para la aplicación. App Service inicia un proxy de túnel y conecta el depurador. A continuación, puede realizar solicitudes a la aplicación y ver al depurador detenerse en los puntos de interrupción.

Una vez que finalice la depuración, detenga el depurador seleccionando **Desconectar**. Cuando se le solicite, debe hacer clic en **Sí** para deshabilitar la depuración remota. Para deshabilitarla más adelante, haga clic en su aplicación de nuevo en el explorador de Azure y seleccione **Disable Remote Debugging** (Deshabilitar la depuración remota).

::: zone-end

## <a name="access-environment-variables"></a>Acceso a variables de entorno

En App Service, puede [establecer la configuración de la aplicación](configure-common.md) fuera del código de la aplicación. Luego puede acceder a ellas mediante el patrón estándar de Node.js. Por ejemplo, para acceder a una configuración de una aplicación denominada `NODE_ENV`, use el código siguiente:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Ejecutar Grunt, Bower o Gulp

De manera predeterminada, la automatización de compilaciones de App Service ejecuta `npm install --production` cuando reconoce que una aplicación de Node.js se implementa a través de Git o a través de implementación zip [con la automatización de compilaciones habilitada](deploy-zip.md#enable-build-automation-for-zip-deploy). Si la aplicación requiere alguna de las herramientas de automatización más populares, como Grunt, Bower o Gulp, deberá suministrar un [script de implementación personalizado](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) para ejecutarla.

Para habilitar que el repositorio ejecute estas herramientas, deberá agregarlas a las dependencias en el archivo *package.json.* Por ejemplo:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Desde una ventana de terminal local, cambie el directorio a la raíz del repositorio y ejecute los siguientes comandos:

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

La raíz del repositorio ahora tiene dos archivos adicionales: *.deployment* y *deploy.sh*.

Abra *deploy.sh* y busque la sección `Deployment`, que tiene el siguiente aspecto:

```bash
##################################################################################################################################
# Deployment
# ----------
```

En esta sección termina con la ejecución de `npm install --production`. Agregue la sección de código que necesita para ejecutar la herramienta necesaria *al final* de la sección `Deployment`:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Consulte un [ejemplo en la muestra de MEAN.js](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135), donde el script de implementación también ejecuta un comando `npm install` personalizado.

### <a name="bower"></a>Bower

Este fragmento de código ejecuta `bower install`.

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Este fragmento de código ejecuta `gulp imagemin`.

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Este fragmento de código ejecuta `grunt`.

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>Detección de sesión de HTTPS

En App Service, la [terminación de TLS/SSL](https://wikipedia.org/wiki/TLS_termination_proxy) se produce en los equilibradores de carga de red, por lo que todas las solicitudes HTTPS llegan a la aplicación como solicitudes HTTP sin cifrar. Si su aplicación lógica necesita comprobar si las solicitudes de usuario están cifradas, inspeccione el encabezado `X-Forwarded-Proto`.

Los marcos web más usados le permiten acceder a la información de `X-Forwarded-*` en el patrón de aplicación estándar. En [Express](https://expressjs.com/), puede usar [trust proxy](https://expressjs.com/guide/behind-proxies.html). Por ejemplo:

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Acceso a los registros de diagnóstico

::: zone pivot="platform-windows"  

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

::: zone-end

::: zone pivot="platform-linux"

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-linux-no-h.md)]

::: zone-end


::: zone pivot="platform-linux"

## <a name="monitor-with-application-insights"></a>Supervisión con Application Insights

Application Insights permite supervisar el rendimiento, las excepciones y el uso de la aplicación sin realizar cambios en el código. Para asociar el agente de App Insights, vaya a la aplicación web en el portal y seleccione **Application Insights** en **Configuración** y elija **Activar Application Insights**. A continuación, seleccione un recurso de App Insights existente o cree uno. Por último, seleccione **Aplicar** en la parte inferior. Para instrumentar la aplicación web con PowerShell, consulte [estas instrucciones](../azure-monitor/app/azure-web-apps-nodejs.md#enable-through-powershell).

Este agente supervisará su aplicación de Node.js del lado servidor. Para supervisar el código JavaScript del lado cliente, [agregue el SDK de JavaScript al proyecto](../azure-monitor/app/javascript.md). 

Para más información, consulte las [notas de la versión de la extensión de Application Insights](../azure-monitor/app/web-app-extension-release-notes.md).

::: zone-end

## <a name="troubleshooting"></a>Solución de problemas

Cuando una aplicación de Node.js en funcionamiento se comporta de manera diferente en App Service o genera errores, intente lo siguiente:

- [Acceso a la secuencia de registros](#access-diagnostic-logs).
- Pruebe la aplicación localmente en modo de producción. App Service ejecuta las aplicaciones de Node.js en el modo de producción, por lo que deberá asegurarse de que el proyecto funciona según lo previsto en modo de producción localmente. Por ejemplo:
    - En función de su archivo *package.json*, pueden instalarse distintos paquetes para el modo de producción (`dependencies` frente a `devDependencies`).
    - Algunas plataformas web pueden implementar archivos estáticos de forma diferente en modo de producción.
    - Algunas plataformas web pueden usar scripts de inicio personalizados cuando se ejecutan en modo de producción.
- Ejecute la aplicación en App Service en el modo de desarrollo. Por ejemplo, en [MEAN.js](https://meanjs.org/), puede establecer la aplicación en modo de desarrollo en tiempo de ejecución [estableciendo la configuración de aplicación `NODE_ENV`](configure-common.md).

::: zone pivot="platform-windows"

#### <a name="you-do-not-have-permission-to-view-this-directory-or-page"></a>No tiene permiso para ver este directorio o esta página

Después de implementar el código de Node.js en una aplicación nativa de Windows en App Service, es posible que vea el mensaje `You do not have permission to view this directory or page.` en el explorador al ir a la dirección URL de la aplicación. Lo más probable es que se deba a que no tiene un archivo *web.config* (vea la [plantilla](https://github.com/projectkudu/kudu/blob/master/Kudu.Core/Scripts/iisnode.config.template) y un [ejemplo](https://github.com/Azure-Samples/nodejs-docs-hello-world/blob/master/web.config)).

Si implementa los archivos mediante Git, o mediante implementación ZIP [con la automatización de compilación habilitada](deploy-zip.md#enable-build-automation-for-zip-deploy), el motor de implementación genera automáticamente un archivo *web.config* en la raíz web de la aplicación (`%HOME%\site\wwwroot`) si se cumple alguna de las condiciones siguientes:

- La raíz del proyecto tiene un archivo *package.json* que define un script `start` que contiene la ruta de acceso de un archivo de JavaScript.
- La raíz del proyecto tiene un archivo *server.js* o *app.js*.

El archivo *web.config* generado se ajusta al script de inicio detectado. En otros métodos de implementación, agregue este archivo *web.config* manualmente. Asegúrese de que el archivo tiene el formato correcto. 

Si usa la [implementación ZIP](deploy-zip.md) (por medio de Visual Studio Code, por ejemplo), asegúrese de [habilitar la automatización de compilación](deploy-zip.md#enable-build-automation-for-zip-deploy), ya que no está habilitada de manera predeterminada. [`az webapp up`](/cli/azure/webapp#az_webapp_up) usa la implementación ZIP con la automatización de compilación habilitada.

::: zone-end

::: zone pivot="platform-linux"

[!INCLUDE [robots933456](../../includes/app-service-web-configure-robots933456.md)]

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Aplicación Node.js con MongoDB](tutorial-nodejs-mongodb-app.md)

::: zone pivot="platform-linux"

> [!div class="nextstepaction"]
> [P+F sobre App Service en Linux](faq-app-service-linux.yml)

::: zone-end

O consulte estos recursos adicionales:

[Variables de entorno y configuración de la aplicación en Azure App Service](reference-app-settings.md)