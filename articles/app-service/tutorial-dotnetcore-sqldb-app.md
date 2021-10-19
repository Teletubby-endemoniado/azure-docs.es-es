---
title: 'Tutorial: ASP.NET Core con Azure SQL Database'
description: Aprenda a poner en funcionamiento una aplicación .NET Core en Azure App Service, con conexión a una instancia de Azure SQL Database.
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 10/06/2021
ms.custom: devx-track-csharp, mvc, cli-validate, seodec18, devx-track-azurecli
zone_pivot_groups: app-service-platform-windows-linux
ms.openlocfilehash: 5db5a4a1d390164cff0f4acee56ca49687ebddfb
ms.sourcegitcommit: e82ce0be68dabf98aa33052afb12f205a203d12d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2021
ms.locfileid: "129658447"
---
# <a name="tutorial-build-an-aspnet-core-and-azure-sql-database-app-in-azure-app-service"></a>Tutorial: Compilación de una aplicación de ASP.NET Core y Azure SQL Database en Azure App Service

::: zone pivot="platform-windows"  

[Azure App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático en Azure. En este tutorial se muestra cómo crear una aplicación ASP.NET Core y conectarla a SQL Database. Cuando termine, tendrá una aplicación MVC de .NET que se ejecuta en App Service en Linux.

::: zone-end

::: zone pivot="platform-linux"

[Azure App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación automática de revisiones con el sistema operativo Linux. En este tutorial se muestra cómo crear una aplicación ASP.NET Core y conectarla a SQL Database. Cuando termine, tendrá una aplicación ASP.NET Core con MVC en ejecución en App Service en Linux.

::: zone-end

![aplicación que se ejecuta en App Service](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una base de datos SQL Database en Azure
> * Conectar una aplicación ASP.NET Core a SQL Database
> * Implementar la aplicación en Azure
> * Actualizar el modelo de datos y volver a implementar la aplicación
> * Transmitir registros de diagnóstico desde Azure
> * Administrar la aplicación en Azure Portal

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerrequisitos

Para completar este tutorial:

- <a href="https://git-scm.com/" target="_blank">Instalación de Git</a>
- <a href="https://dotnet.microsoft.com/download/dotnet/5.0" target="_blank">Instale el SDK de .NET 5.0 más reciente</a>.

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

## <a name="create-local-aspnet-core-app"></a>Creación de una aplicación ASP.NET Core local

En este paso, configurará el proyecto ASP.NET Core local.

### <a name="clone-the-sample-application"></a>Clonación de la aplicación de ejemplo

1. En la ventana del terminal, use `cd` para cambiar a un directorio de trabajo.

1. Ejecute los comandos siguientes para clonar el repositorio de ejemplo y cambiar a su raíz.

    ```bash
    git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
    cd dotnetcore-sqldb-tutorial
    ```

    El proyecto de ejemplo contiene una aplicación básica CRUD (crear, leer, actualizar, eliminar) que usa [Entity Framework Core](/ef/core/).

1. Asegúrese de que la rama predeterminada sea `main`.

    ```bash
    git branch -m main
    ```
    
    > [!TIP]
    > App Service no exige el cambio de nombre de rama. Sin embargo, dado que muchos repositorios cambian su rama predeterminada a `main` (consulte [Cambio de la rama de implementación](deploy-local-git.md#change-deployment-branch)), en este tutorial también se muestra cómo implementar un repositorio desde `main`.

### <a name="run-the-application"></a>Ejecución de la aplicación

1. Ejecute los comandos siguientes para instalar los paquetes necesarios, ejecutar las migraciones de bases de datos e iniciar la aplicación.

    ```bash
    dotnet tool install -g dotnet-ef
    dotnet ef database update
    dotnet run
    ```

1. Vaya a `http://localhost:5000` en un explorador. Seleccione el vínculo **Crear nuevo** y cree un par de elementos de _tareas pendientes_.

    ![Se conecta correctamente a SQL Database](./media/tutorial-dotnetcore-sqldb-app/local-app-in-browser.png)

1. Para detener ASP.NET Core en cualquier momento, presione `Ctrl+C` en el terminal.

## <a name="create-production-sql-database"></a>Creación de una instancia de SQL Database de producción

En este paso, creará una instancia de SQL Database en Azure. Cuando la aplicación se implementa en Azure, utiliza esta base de datos en la nube.

Para SQL Database, en este tutorial se usa [Azure SQL Database](/azure/sql-database/).

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>Creación de un servidor lógico de SQL Database

En Cloud Shell, cree un servidor lógico de SQL Database con el comando [`az sql server create`](/cli/azure/sql/server#az_sql_server_create).

Reemplace el marcador de posición *\<server-name>* por un nombre *único* de SQL Database. Este nombre se usa como parte del punto de conexión SQL Database único global `<server-name>.database.windows.net`. Los caracteres válidos son `a`-`z`, `0`-`9`, `-`. Reemplace también *\<db-username>* y *\<db-password>* por un nombre de usuario y una contraseña de su elección. 


```azurecli-interactive
az sql server create --name <server-name> --resource-group myResourceGroup --location "West Europe" --admin-user <db-username> --admin-password <db-password>
```

Cuando se crea el servidor lógico de SQL Database, la CLI de Azure muestra información similar al ejemplo siguiente:

<pre>
{
  "administratorLogin": "&lt;db-username&gt;",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "&lt;server-name&gt;.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/&lt;server-name&gt;",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "&lt;server-name&gt;",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
</pre>

### <a name="configure-a-server-firewall-rule"></a>Configuración de una regla de firewall del servidor

1. Cree una [regla de firewall de nivel de servidor de Azure SQL Database](../azure-sql/database/firewall-configure.md) mediante el comando [`az sql server firewall create`](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create). Cuando tanto la dirección IP de inicio como final están establecidas en 0.0.0.0., el firewall solo se abre para otros recursos de Azure. 

    ```azurecli-interactive
    az sql server firewall-rule create --resource-group myResourceGroup --server <server-name> --name AllowAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
    ```
    
    > [!TIP] 
    > Puede ser incluso más restrictivo con su regla de firewall [usando solo las direcciones IP de salida que utiliza su aplicación](overview-inbound-outbound-ips.md#find-outbound-ips).
    >

1. En Cloud Shell, ejecute de nuevo el comando para permitir el acceso desde el equipo local y reemplace *\<your-ip-address>* por su [dirección IP IPv4 local](https://www.whatsmyip.org/).

    ```azurecli-interactive
    az sql server firewall-rule create --name AllowLocalClient --server <server-name> --resource-group myResourceGroup --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address>
    ```

### <a name="create-a-database"></a>Crear una base de datos

Cree una base de datos con un [nivel de rendimiento S0](../azure-sql/database/service-tiers-dtu.md) en el servidor con el comando [`az sql db create`](/cli/azure/sql/db#az_sql_db_create).

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server-name> --name coreDB --service-objective S0
```

### <a name="retrieve-connection-string"></a>Recuperación de la cadena de conexión

Obtenga la cadena de conexión mediante el comando [`az sql db show-connection-string`](/cli/azure/sql/db#az_sql_db_show_connection_string).

```azurecli-interactive
az sql db show-connection-string --client ado.net --server <server-name> --name coreDB
```

En la salida del comando, reemplace *\<username>* y *\<password>* por las credenciales de administrador de base de datos que usó anteriormente.

Esta es la cadena de conexión de la aplicación ASP.NET Core. Cópiela para usarla más adelante.

### <a name="configure-app-to-connect-to-production-database"></a>Configuración de la aplicación para conectarse a la base de datos de producción

En el repositorio local, abra Startup.cs y busque el código siguiente:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Reemplácelo por el código que verá a continuación.

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
```

> [!IMPORTANT]
> Para las aplicaciones de producción que necesiten escalar horizontalmente, siga los procedimientos recomendados en [Aplicación de migraciones en producción](/aspnet/core/data/ef-rp/migrations#applying-migrations-in-production).
> 

### <a name="run-database-migrations-to-the-production-database"></a>Ejecución de migraciones de base de datos a la base de datos de producción

Actualmente, la aplicación se conecta a una base de datos SQLite local. Ahora que ha configurado una instancia de Azure SQL Database, vuelva a crear la migración inicial para establecerla como destino. 

En la raíz del repositorio, ejecute los siguientes comandos. Reemplace *\<connection-string>* por la cadena de conexión que creó anteriormente.

```
# Delete old migrations
rm -r Migrations
# Recreate migrations with UseSqlServer (see previous snippet)
dotnet ef migrations add InitialCreate

# Set connection string to production database
# PowerShell
$env:ConnectionStrings:MyDbConnection="<connection-string>"
# CMD (no quotes)
set ConnectionStrings:MyDbConnection=<connection-string>
# Bash (no quotes)
export ConnectionStrings__MyDbConnection=<connection-string>

# Run migrations
dotnet ef database update
```

### <a name="run-app-with-new-configuration"></a>Ejecución de la aplicación con una configuración nueva

1. Ahora que las migraciones de base de datos se ejecutan en la base de datos de producción, pruebe la aplicación mediante la ejecución de:

    ```
    dotnet run
    ```

1. Vaya a `http://localhost:5000` en un explorador. Seleccione el vínculo **Crear nuevo** y cree un par de elementos de _tareas pendientes_. La aplicación ahora lee y escribe datos en la base de datos de producción.

1. Guarde los cambios y confírmelos en el repositorio de Git. 

    ```bash
    git add .
    git commit -m "connect to SQLDB in Azure"
    ```

Ahora está preparado para implementar el código.

## <a name="deploy-app-to-azure"></a>Implementación de la aplicación en Azure

En este paso, va a implementar la aplicación ASP.NET Core conectada a SQL Database en App Service.

### <a name="configure-local-git-deployment"></a>Configuración de la implementación de Git local

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Creación de un plan de App Service

::: zone pivot="platform-windows"  

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

::: zone-end

::: zone pivot="platform-linux"

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

::: zone-end

### <a name="create-a-web-app"></a>Creación de una aplicación web

::: zone pivot="platform-windows"  

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-dotnetcore-win-no-h.md)]

::: zone-end

::: zone pivot="platform-linux"

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-dotnetcore-linux-no-h.md)]

::: zone-end

### <a name="configure-connection-string"></a>Configuración de la cadena de conexión

Para establecer las cadenas de conexión de la aplicación de Azure, use el comando [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) en Cloud Shell. En el comando siguiente, reemplace *\<app-name>* y el parámetro *\<connection-string>* por la cadena de conexión que creó anteriormente.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app-name> --settings MyDbConnection='<connection-string>' --connection-string-type SQLAzure
```

En ASP.NET Core, puede usar esta cadena de conexión con nombre (`MyDbConnection`) con el patrón estándar, como cualquier cadena de conexión especificada en *appsettings.jon*. En este caso, `MyDbConnection` también se define en *appsettings.json*. Cuando se ejecuta en App Service, la cadena de conexión definida en App Service tiene prioridad sobre la definida en *appsettings.json*. El código usa el valor de *appsettings.json* durante el desarrollo local y el mismo código usa el valor de App Service cuando se implementa.

Para ver cómo se hace referencia a la cadena de conexión en el código, consulte [Configuración de la aplicación para conectarse a la base de datos de producción](#configure-app-to-connect-to-production-database).

### <a name="push-to-azure-from-git"></a>Inserción en Azure desde Git

[!INCLUDE [push-to-azure-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

::: zone pivot="platform-windows"  

   <pre>
   Enumerating objects: 268, done.
   Counting objects: 100% (268/268), done.
   Compressing objects: 100% (171/171), done.
   Writing objects: 100% (268/268), 1.18 MiB | 1.55 MiB/s, done.
   Total 268 (delta 95), reused 251 (delta 87), pack-reused 0
   remote: Resolving deltas: 100% (95/95), done.
   remote: Updating branch 'main'.
   remote: Updating submodules.
   remote: Preparing deployment for commit id '64821c3558'.
   remote: Generating deployment script.
   remote: Project file path: .\DotNetCoreSqlDb.csproj
   remote: Generating deployment script for ASP.NET MSBuild16 App
   remote: Generated deployment script files
   remote: Running deployment command...
   remote: Handling ASP.NET Core Web Application deployment with MSBuild16.
   remote: .
   remote: .
   remote: .
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Triggering recycle (preview mode disabled).
   remote: App container will begin restart within 10 seconds.
   To https://&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git
    * [new branch]      main -> main
   </pre>

::: zone-end

::: zone pivot="platform-linux"

   <pre>
   Enumerating objects: 273, done.
   Counting objects: 100% (273/273), done.
   Delta compression using up to 4 threads
   Compressing objects: 100% (175/175), done.
   Writing objects: 100% (273/273), 1.19 MiB | 1.85 MiB/s, done.
   Total 273 (delta 96), reused 259 (delta 88)
   remote: Resolving deltas: 100% (96/96), done.
   remote: Deploy Async
   remote: Updating branch 'main'.
   remote: Updating submodules.
   remote: Preparing deployment for commit id 'cccecf86c5'.
   remote: Repository path is /home/site/repository
   remote: Running oryx build...
   remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
   remote: You can report issues at https://github.com/Microsoft/Oryx/issues
   remote: .
   remote: .
   remote: .
   remote: Done.
   remote: Running post deployment command(s)...
   remote: Triggering recycle (preview mode disabled).
   remote: Deployment successful.
   remote: Deployment Logs : 'https://&lt;app-name&gt;.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/cccecf86c56493ffa594e76ea1deb3abb3702d89/log'
   To https://&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git
    * [new branch]      main -> main
   </pre>

::: zone-end

### <a name="browse-to-the-azure-app"></a>Navegación hasta la aplicación de Azure

1. Vaya a la aplicación implementada mediante el explorador web.

    ```bash
    http://<app-name>.azurewebsites.net
    ```

1. Agregue algunos elementos de tareas pendientes.

    ![aplicación que se ejecuta en App Service](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**¡Enhorabuena!** Está ejecutando una aplicación ASP.NET Core controlada por datos en App Service.

## <a name="update-locally-and-redeploy"></a>Actualización local y nueva implementación

En este paso, modificará el esquema de la base de datos y lo publicará en Azure.

### <a name="update-your-data-model"></a>Actualización del modelo de datos

Abra _Models/Todo.cs_ en el editor de código. Agregue la siguiente propiedad a la clase `ToDo`:

```csharp
public bool Done { get; set; }
```

### <a name="rerun-database-migrations"></a>Nueva ejecución de migraciones de base de datos

Ejecute algunos comandos para realizar actualizaciones en la base de datos de producción.

```bash
dotnet ef migrations add AddProperty
dotnet ef database update
```

> [!NOTE]
> Si abre una nueva ventana de terminal, debe establecer la cadena de conexión en la base de datos de producción en el terminal, como hizo en [Ejecución de migraciones de base de datos a la base de datos de producción](#run-database-migrations-to-the-production-database).
>

### <a name="use-the-new-property"></a>Uso de la nueva propiedad

Realice algunos cambios en el código para usar la propiedad `Done`. Para simplificar, en este tutorial va a cambiar las vistas `Index` y `Create` para ver la propiedad en acción.

1. Abra _Controllers/TodosController.cs_.

1. Busque el método `Create([Bind("ID,Description,CreatedDate")] Todo todo)` y agregue `Done` a la lista de propiedades en el atributo `Bind`. Cuando haya terminado, la firma del método `Create()` se parecerá al código siguiente:

    ```csharp
    public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
    ```

1. Abra _Views/Todos/Create.cshtml_.

1. En el código Razor debería ver un elemento `<div class="form-group">` para `Description` y, luego, otro elemento `<div class="form-group">` para `CreatedDate`. Inmediatamente después de estos dos elementos, agregue otro elemento `<div class="form-group">` para `Done`:

    ```csharp
    <div class="form-group">
        <label asp-for="Done" class="col-md-2 control-label"></label>
        <div class="col-md-10">
            <input asp-for="Done" class="form-control" />
            <span asp-validation-for="Done" class="text-danger"></span>
        </div>
    </div>
    ```

1. Abra _Views/Todos/Index.cshtml_.

1. Busque el elemento vacío `<th></th>`. Justo encima de este elemento, agregue el siguiente código Razor:

    ```csharp
    <th>
        @Html.DisplayNameFor(model => model.Done)
    </th>
    ```

1. Busque el elemento `<td>` que contiene los asistentes de etiquetas `asp-action`. Justo encima de este elemento, agregue el siguiente código Razor:

    ```csharp
    <td>
        @Html.DisplayFor(modelItem => item.Done)
    </td>
    ```

Eso es todo lo que necesita para ver los cambios en las vistas `Index` y `Create`.

### <a name="test-your-changes-locally"></a>Prueba de los cambios localmente

1. Ejecute localmente la aplicación.

    ```bash
    dotnet run
    ```

    > [!NOTE]
    > Si abre una nueva ventana de terminal, debe establecer la cadena de conexión en la base de datos de producción en el terminal, como hizo en [Ejecución de migraciones de base de datos a la base de datos de producción](#run-database-migrations-to-the-production-database).
    >

1. Abra el explorador y vaya a `http://localhost:5000/`. Ahora puede agregar una tarea pendiente y marcar **Listo**. A continuación se debería mostrar en su página principal como un elemento completado. Recuerde que la vista `Edit` no muestra el campo `Done`, dado que no cambió la vista `Edit`.

### <a name="publish-changes-to-azure"></a>Publicación de los cambios en Azure

1. Confirme los cambios en Git e insértelo en la aplicación de App Service.

    ```bash
    git add .
    git commit -m "added done field"
    git push azure main
    ```

1. Una vez que `git push` esté completo, vaya a la aplicación de App Service e intente agregar un elemento de tarea y active **Listo**.

    ![Aplicación de Azure después de Migraciones de Code First](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Aún se muestran todas las tareas pendientes existentes. Cuando vuelva a publicar la aplicación ASP.NET Core, no se perderán los datos existentes en la instancia de SQL Database. Además, las migraciones de Entity Framework Core solo cambia el esquema de datos y deja intactos los datos existentes.

## <a name="stream-diagnostic-logs"></a>Transmisión de registros de diagnóstico

Mientras la aplicación ASP.NET Core se ejecuta en Azure App Service, puede hacer que los registros de la consola se canalicen a Cloud Shell. De este modo, puede obtener los mismos mensajes de diagnóstico para ayudarle a depurar errores de la aplicación.

El proyecto de ejemplo ya sigue las instrucciones del [proveedor de registro de Azure App Service](/dotnet/core/extensions/logging-providers#azure-app-service) con dos cambios de configuración:

- Incluye una referencia a `Microsoft.Extensions.Logging.AzureAppServices` en *DotNetCoreSqlDb.csproj*.
- Llama a `loggerFactory.AddAzureWebAppDiagnostics()` en *Program.cs*.

1. Para establecer el [nivel de registro](/dotnet/core/extensions/logging#log-level) de ASP.NET Core en App Service en `Information` desde el nivel predeterminado `Error`, utilice el comando [`az webapp log config`](/cli/azure/webapp/log#az_webapp_log_config) en Cloud Shell.

    ```azurecli-interactive
    az webapp log config --name <app-name> --resource-group myResourceGroup --application-logging filesystem --level information
    ```

    > [!NOTE]
    > El nivel de registro del proyecto ya está establecido en `Information` en *appsettings.json*.

1. Para iniciar la transmisión del registro, use el comando [`az webapp log tail`](/cli/azure/webapp/log#az_webapp_log_tail) en Cloud Shell.

    ```azurecli-interactive
    az webapp log tail --name <app-name> --resource-group myResourceGroup
    ```

1. Cuando las secuencias de registro se inicien, actualice la aplicación de Azure en el explorador para obtener tráfico web. Ahora puede ver los registros de la consola canalizados al terminal. Si no ve los registros de la consola de inmediato, vuelve a comprobarlo en 30 segundos.

1. Para detener el streaming del registro en cualquier momento, escriba `Ctrl`+`C`.

Para más información acerca de cómo personalizar los registros de ASP.NET Core, consulte [Registro en .NET](/dotnet/core/extensions/logging).

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Pasos siguientes

¿Qué ha aprendido?

> [!div class="checklist"]
> * Crear una base de datos SQL Database en Azure
> * Conectar una aplicación ASP.NET Core a SQL Database
> * Implementar la aplicación en Azure
> * Actualizar el modelo de datos y volver a implementar la aplicación
> * Transmitir registros desde Azure a un terminal
> * Administrar la aplicación en Azure Portal

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a una aplicación web.

> [!div class="nextstepaction"]
> [Tutorial: Asignar un nombre DNS personalizado a la aplicación](app-service-web-tutorial-custom-domain.md)

O bien, eche un vistazo a otros recursos:

> [!div class="nextstepaction"]
> [Configure ASP.NET Core app](configure-language-dotnetcore.md) (Configuración de una aplicación de ASP.NET Core)
