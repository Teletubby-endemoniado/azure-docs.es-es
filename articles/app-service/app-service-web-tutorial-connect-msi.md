---
title: 'Tutorial: Acceso a los datos con una identidad administrada'
description: Aprenda a hacer que la conectividad con la base de datos sea más segura con una identidad administrada y también a aplicarlo a otros servicios de Azure.
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/27/2020
ms.custom: devx-track-csharp, mvc, cli-validate, devx-track-azurecli
ms.openlocfilehash: 7b415b161dd719dabd02ccde4bf0a57da6485c09
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121730732"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Tutorial: Protección de la conexión con Azure SQL Database desde App Service mediante una identidad administrada

[App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático en Azure. También proporciona una [identidad administrada](overview-managed-identity.md) para la aplicación, la cual constituye una solución inmediata para proteger el acceso a [Azure SQL Database](/azure/sql-database/) y a otros servicios de Azure. Las identidades administradas de App Service hacen que su aplicación sea más segura mediante la eliminación de los secretos de aplicación como, por ejemplo, las credenciales en las cadenas de conexión. En este tutorial, agregará una identidad administrada a la aplicación web de ejemplo que se creó en los tutoriales siguientes: 

- [Tutorial: Compilación de una aplicación ASP.NET en Azure con Azure SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)
- [Tutorial: Compilación de una aplicación ASP.NET Core y Azure SQL Database en Azure App Service](tutorial-dotnetcore-sqldb-app.md)

Cuando haya terminado, la aplicación de ejemplo se conectará a SQL Database de forma segura sin necesidad de nombres de usuario ni contraseñas.

> [!NOTE]
> Los pasos descritos en este tutorial son compatibles con las siguientes versiones:
> 
> - .NET Framework 4.7.2 y versiones posteriores.
> - .NET Core 2.2 y versiones posteriores.
>

Lo qué aprenderá:

> [!div class="checklist"]
> * Habilitar identidades administradas
> * Conceder a SQL Database acceso a la identidad administrada
> * Configurar Entity Framework para utilizar la autenticación de Azure AD con SQL Database
> * Conectarse a SQL Database desde Visual Studio mediante la autenticación de Azure AD

> [!NOTE]
>La autenticación de Azure AD es _distinta_ de la [autenticación integrada de Windows](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) en Active Directory (AD DS) local. AD DS y Azure AD utilizan protocolos de autenticación completamente diferentes. Para más información, consulte [Documentación de Azure AD Domain Services](../active-directory-domain-services/index.yml).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Este artículo continúa donde lo dejó en [Tutorial: Creación de una aplicación ASP.NET en Azure con SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md) o [Tutorial: Compilación de una aplicación ASP.NET Core y SQL Database en Azure App Service](tutorial-dotnetcore-sqldb-app.md). Si aún no lo ha hecho, siga uno de los dos tutoriales en primer lugar. Como alternativa, puede adaptar los pasos para su propia aplicación .NET con SQL Database.

Para depurar la aplicación con SQL Database como back-end, asegúrese de permitir la conexión de cliente desde el equipo. Si no es así, agregue la dirección IP del cliente siguiendo los pasos que se describen en [Administración de reglas de firewall de nivel de servidor mediante Azure Portal](../azure-sql/database/firewall-configure.md#use-the-azure-portal-to-manage-server-level-ip-firewall-rules).

Prepare el entorno para la CLI de Azure.

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

## <a name="grant-database-access-to-azure-ad-user"></a>Concesión del acceso a la base de datos a un usuario de Azure AD

Primero debe habilitar la autenticación de Azure AD para SQL Database mediante la asignación de un usuario de Azure AD como administrador de Active Directory para el servidor. Este usuario no es la cuenta Microsoft que usó para suscribirse a Azure. Debe ser un usuario que haya creado, importado, sincronizado o invitado en Azure AD. Para más información sobre los usuarios de Azure AD permitidos, consulte las [características de Azure AD y las limitaciones de en SQL Database](../azure-sql/database/authentication-aad-overview.md#azure-ad-features-and-limitations).

1. Si el inquilino de Azure AD aún no tiene un usuario, cree uno siguiendo los pasos que se indican en [Incorporación o eliminación de usuarios mediante Azure Active Directory](../active-directory/fundamentals/add-users-azure-active-directory.md).

1. Busque el identificador de objeto del usuario de Azure AD mediante [`az ad user list`](/cli/azure/ad/user#az_ad_user_list) y reemplace *\<user-principal-name>* . El resultado se guardará en una variable.

    ```azurecli-interactive
    azureaduser=$(az ad user list --filter "userPrincipalName eq '<user-principal-name>'" --query [].objectId --output tsv)
    ```

    > [!TIP]
    > Para ver la lista de todos los nombres principales de usuario de Azure AD, ejecute `az ad user list --query [].userPrincipalName`.
    >

1. Agregue este usuario de Azure AD como administrador de Active Directory mediante el comando [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin#az_sql_server_ad_admin_create) en Cloud Shell. En el siguiente comando, reemplace *\<server-name>* por el nombre del servidor (sin el sufijo `.database.windows.net`).

    ```azurecli-interactive
    az sql server ad-admin create --resource-group myResourceGroup --server-name <server-name> --display-name ADMIN --object-id $azureaduser
    ```

Para más información sobre cómo agregar un administrador de Active Directory, consulte [Provision an Azure Active Directory administrator for your Server](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance) (Aprovisionamiento de un administrador de Azure Active Directory para el servidor)

## <a name="set-up-visual-studio"></a>Configuración de Visual Studio

# <a name="windows-client"></a>[Cliente Windows](#tab/windowsclient)

1. Visual Studio para Windows está integrado con la autenticación de Azure AD. Para habilitar el desarrollo y la depuración en Visual Studio, agregue el usuario de Azure AD en Visual Studio; para ello, seleccione **Archivo** > **Configuración de la cuenta** en el menú y haga clic en **Agregar un cuenta**.

1. Para establecer el usuario de Azure AD para la autenticación de servicio de Azure, seleccione **Herramientas** > **Opciones** en el menú y, después, **Azure Service Authentication** (Autenticación del servicio de Azure) > **Selección de cuentas**. Seleccione el usuario de Azure AD que agregó y haga clic en **Aceptar**.

# <a name="macos-client"></a>[Cliente de macOS](#tab/macosclient)

1. Visual Studio para Mac no está integrado con la autenticación de Azure AD. No obstante, la biblioteca [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) que utilizará más adelante puede usar tokens de la CLI de Azure. Para habilitar el desarrollo y la depuración en Visual Studio, [instale la CLI de Azure](/cli/azure/install-azure-cli) en la máquina local.

1. Inicie sesión en la CLI de Azure con el siguiente comando con el usuario de Azure AD:

    ```azurecli
    az login --allow-no-subscriptions
    ```

-----

Ahora está listo para desarrollar y depurar la aplicación con SQL Database como back-end y mediante la autenticación de Azure AD.

## <a name="modify-your-project"></a>Modificación del proyecto

Los pasos que se siguen para el proyecto dependen de si se trata de un proyecto de ASP.NET o de ASP.NET Core.

# <a name="aspnet"></a>[ASP.NET](#tab/dotnet)

1. En Visual Studio, abra la consola del administrador de paquetes y agregue el paquete NuGet [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication):

    ```powershell
    Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.4.0
    ```

1. En *Web.config*, desde el principio del archivo, realice los siguientes cambios:

    - En `<configSections>`, agregue la siguiente declaración de la sección:
    
        ```xml
        <section name="SqlAuthenticationProviders" type="System.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        ```
    
    - Bajo la etiqueta de cierre `</configSections>`, agregue el siguiente código XML para `<SqlAuthenticationProviders>`.
    
        ```xml
        <SqlAuthenticationProviders>
          <providers>
            <add name="Active Directory Interactive" type="Microsoft.Azure.Services.AppAuthentication.SqlAppAuthenticationProvider, Microsoft.Azure.Services.AppAuthentication" />
          </providers>
        </SqlAuthenticationProviders>
        ```    
    
    - Busque la cadena de conexión denominada `MyDbConnection` y reemplace su valor `connectionString` por `"server=tcp:<server-name>.database.windows.net;database=<db-name>;UID=AnyString;Authentication=Active Directory Interactive"`. Reemplace _\<server-name>_ y _\<db-name>_ por el nombre del servidor y el de la base de datos.
    
    > [!NOTE]
    > La clase SqlAuthenticationProvider que acaba de registrar se basa en la biblioteca AppAuthentication que ha instalado antes. De forma predeterminada usa una identidad asignada por el sistema. Para aprovechar una identidad asignada por el usuario, necesitará proporcionar una configuración adicional. Consulte [Compatibilidad de la cadena de conexión](/dotnet/api/overview/azure/service-to-service-authentication#connection-string-support) para más información acerca de la biblioteca AppAuthentication.

    Eso es todo lo que necesita para conectarse a SQL Database. Al depurar en Visual Studio, el código utiliza el usuario de Azure AD que configuró en [Configuración de Visual Studio.](#set-up-visual-studio) Después, configurará SQL Database para permitir la conexión desde la identidad administrada de la aplicación App Service.

1. Escriba `Ctrl+F5` para ejecutar la aplicación de nuevo. Ahora, la misma aplicación CRUD del explorador se conectará a Azure SQL Database directamente con la autenticación de Azure AD. Esta configuración permite ejecutar migraciones de base de datos desde Visual Studio.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/dotnetcore)

> [!NOTE]
> Ya no se recomienda usar **Microsoft.Azure.Services.AppAuthentication** con el nuevo SDK de Azure. Se reemplaza por la nueva **biblioteca de clientes de identidades de Azure** disponible para .NET, Java, TypeScript y Python, y debe usarse para todo el desarrollo nuevo. Aquí puede encontrar información sobre cómo migrar a `Azure Identity`: [Guía de migración de AppAuthentication a Azure.Identity](/dotnet/api/overview/azure/app-auth-migration).

1. En Visual Studio, abra la consola del administrador de paquetes y agregue el paquete NuGet [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication):

    ```powershell
    Install-Package Microsoft.Data.SqlClient -Version 2.1.2
    Install-Package Azure.Identity -Version 1.4.0
    ```

1. En el [tutorial de ASP.NET Core y SQL Database](tutorial-dotnetcore-sqldb-app.md), la cadena de conexión `MyDbConnection` no se usa porque el entorno de desarrollo local utiliza un archivo de base de datos de SQLite y el entorno de producción de Azure usa una cadena de conexión de App Service. Con la autenticación de Active Directory, es conveniente que ambos entornos usen la misma cadena de conexión. En *appsettings.json*, reemplace el valor de la cadena de conexión `MyDbConnection` por:

    ```json
    "Server=tcp:<server-name>.database.windows.net;Authentication=Active Directory Device Code Flow; Database=<database-name>;"
    ```

    > [!NOTE]
    > Se usa el tipo de autenticación `Active Directory Device Code Flow` porque es lo más parecido a una opción personalizada. Idealmente, estaría disponible un tipo `Custom Authentication`. Sin un término mejor para usar en este momento, se usa `Device Code Flow`.
    >

1. A continuación, debe crear una clase de proveedor de autenticación personalizado para adquirir y proporcionar el token de acceso para SQL Database al contexto de base de datos de Entity Framework. En el directorio *Data\\* , agregue una nueva clase `CustomAzureSQLAuthProvider.cs` que contenga el código siguiente:

    ```csharp
    public class CustomAzureSQLAuthProvider : SqlAuthenticationProvider
    {
        private static readonly string[] _azureSqlScopes = new[]
        {
            "https://database.windows.net//.default"
        };
    
        private static readonly TokenCredential _credential = new DefaultAzureCredential();
    
        public override async Task<SqlAuthenticationToken> AcquireTokenAsync(SqlAuthenticationParameters parameters)
        {
            var tokenRequestContext = new TokenRequestContext(_azureSqlScopes);
            var tokenResult = await _credential.GetTokenAsync(tokenRequestContext, default);
            return new SqlAuthenticationToken(tokenResult.Token, tokenResult.ExpiresOn);
        }
    
        public override bool IsSupported(SqlAuthenticationMethod authenticationMethod) => authenticationMethod.Equals(SqlAuthenticationMethod.ActiveDirectoryDeviceCodeFlow);
    }
    ```

1. En *Startup.cs*, actualice el método `ConfigureServices()` por el código siguiente:

    ```csharp
    services.AddControllersWithViews();
    services.AddDbContext<MyDatabaseContext>(options =>
    {
        SqlAuthenticationProvider.SetProvider(
            SqlAuthenticationMethod.ActiveDirectoryDeviceCodeFlow, 
            new CustomAzureSQLAuthProvider());
        var sqlConnection = new SqlConnection(Configuration.GetConnectionString("MyDbConnection"));
        options.UseSqlServer(sqlConnection);
    });
    ```

    > [!NOTE]
    > Este código de demostración es sincrónico para mayor claridad y simplicidad.
    
    En el código anterior se usa la biblioteca `Azure.Identity` para que pueda autenticarse y recuperar un token de acceso para la base de datos, independientemente de dónde se ejecute el código. Si la ejecución se realiza en el equipo local, `DefaultAzureCredential()` recorre en bucle varias opciones para buscar una cuenta válida que haya iniciado sesión. Puede leer más sobre la [clase DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential).

    Eso es todo lo que necesita para conectarse a SQL Database. Al depurar en Visual Studio, el código utiliza el usuario de Azure AD que configuró en [Configuración de Visual Studio.](#set-up-visual-studio) Después, configurará SQL Database para permitir la conexión desde la identidad administrada de la aplicación App Service. La `DefaultAzureCredential` clase almacena el token en la memoria y lo recupera de Azure AD justo antes de que expire. Para actualizar el token no es necesario ningún código personalizado.

1. Escriba `Ctrl+F5` para ejecutar la aplicación de nuevo. Ahora, la misma aplicación CRUD del explorador se conectará a Azure SQL Database directamente con la autenticación de Azure AD. Esta configuración permite ejecutar migraciones de base de datos desde Visual Studio.

-----

## <a name="use-managed-identity-connectivity"></a>Uso de la conectividad de la identidad administrada

A continuación, configure la aplicación de App Service para conectarse a SQL Database con una identidad administrada asignada por el sistema.

> [!NOTE]
> Aunque las instrucciones de esta sección son para una identidad asignada por el sistema, es igual de fácil usar una identidad asignada por el usuario. Para ello necesitaría cambiar `az webapp identity assign command` para asignar la identidad asignada por el usuario deseada. Luego, al crear el usuario de SQL, asegúrese de usar el nombre del recurso de la identidad asignada por el usuario, en lugar del nombre del sitio.

### <a name="enable-managed-identity-on-app"></a>Habilitación de la identidad administrada en la aplicación

Para habilitar una identidad administrada para la aplicación de Azure, use el comando [az webapp identity assign](/cli/azure/webapp/identity#az_webapp_identity_assign) de Cloud Shell. En el siguiente comando, reemplace *\<app-name>* .

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app-name>
```

> [!NOTE]
> Para habilitar la identidad administrada para una [ranura de implementación](deploy-staging-slots.md), agregue `--slot <slot-name>` y use el nombre de la ranura en *\<slot-name>* .

Este es un ejemplo de la salida:

<pre>
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
</pre>

### <a name="grant-permissions-to-managed-identity"></a>Concesión	 de permisos a una identidad administrada

> [!NOTE]
> Si lo desea, puede agregar la identidad a un [grupo de Azure AD](../active-directory/fundamentals/active-directory-manage-groups.md) y, a continuación, conceder acceso a SQL Database al grupo de Azure AD en lugar de a la identidad. Por ejemplo, los siguientes comandos agregan la identidad administrada del paso anterior a un nuevo grupo llamado _myAzureSQLDBAccessGroup_:
> 
> ```azurecli-interactive
> groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
> msiobjectid=$(az webapp identity show --resource-group myResourceGroup --name <app-name> --query principalId --output tsv)
> az ad group member add --group $groupid --member-id $msiobjectid
> az ad group member list -g $groupid
> ```
>

1. En Cloud Shell, inicie sesión en SQL Database mediante el comando SQLCMD. Reemplace _\<server-name>_ por el nombre del servidor, _\<db-name>_ por el nombre de la base de datos que usa la aplicación, _\<aad-user-name>_ y _\<aad-password>_ por las credenciales del usuario de Azure AD.

    ```bash
    sqlcmd -S <server-name>.database.windows.net -d <db-name> -U <aad-user-name> -P "<aad-password>" -G -l 30
    ```

1. En el símbolo del sistema de SQL para la base de datos que desee, ejecute los siguientes comandos para conceder los permisos que la aplicación necesita. Por ejemplo, 

    ```sql
    CREATE USER [<identity-name>] FROM EXTERNAL PROVIDER;
    ALTER ROLE db_datareader ADD MEMBER [<identity-name>];
    ALTER ROLE db_datawriter ADD MEMBER [<identity-name>];
    ALTER ROLE db_ddladmin ADD MEMBER [<identity-name>];
    GO
    ```

    *\<identity-name>* es el nombre de la identidad administrada en Azure AD. Si la identidad la ha asignado el sistema, el nombre siempre coincide con el nombre de la aplicación de App Service. En el caso de una [ranura de implementación](deploy-staging-slots.md), el nombre de su identidad asignada por el sistema es *\<app-name>/slots/\<slot-name>* . Para conceder permisos para un grupo de Azure AD, utilice en su lugar el nombre para mostrar del grupo (por ejemplo, *myAzureSQLDBAccessGroup*).

1. Escriba `EXIT` para volver al símbolo del sistema de Cloud Shell.

    > [!NOTE]
    > Los servicios de back-end de las identidades administradas también [mantienen una caché de token](overview-managed-identity.md#obtain-tokens-for-azure-resources) que actualiza el token de un recurso de destino solo cuando expira. Si realiza algún error al configurar los permisos de SQL Database e intenta modificarlos *después* de intentar obtener un token con su aplicación, no obtendrá realmente un token nuevo con permisos actualizados hasta que expire el token de la caché.

    > [!NOTE]
    > No se admiten instancias administradas ni Azure Active Directory en el servidor local de SQL Server. 

### <a name="modify-connection-string"></a>Modificación de la cadena de conexión

Recuerde que los mismos cambios que haya realizado en *Web.config* o *appsettings.json* funcionan con la identidad administrada, por lo que lo único necesario es eliminar la cadena de conexión existente de App Service, que Visual Studio creó al implementar la aplicación inicialmente. En los siguientes comandos, reemplace *\<app-name>* por el nombre de la aplicación.

```azurecli-interactive
az webapp config connection-string delete --resource-group myResourceGroup --name <app-name> --setting-names MyDbConnection
```

## <a name="publish-your-changes"></a>Publicación de los cambios

Ahora lo único que queda es publicar los cambios en Azure.

# <a name="aspnet"></a>[ASP.NET](#tab/dotnet)

1. **Si venía del [Tutorial: Creación de una aplicación de ASP.NET en Azure con SQL Database](app-service-web-tutorial-dotnet-sqldatabase.md)** , publique los cambios en Visual Studio. En el **Explorador de soluciones**, haga clic con el botón derecho en su proyecto **DotNetAppSqlDb** y seleccione **Publicar**.

    ![Publicar desde el Explorador de soluciones](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

1. En la página de publicación, haga clic en **Publicar**. 

    > [!IMPORTANT]
    > Asegúrese de que el nombre del servicio de aplicaciones no coincide con ningún [registro de aplicaciones](../active-directory/manage-apps/add-application-portal.md) existente. De lo contrario, se producirían conflictos de identificadores de entidad de seguridad.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/dotnetcore)

**Si venía del [Tutorial: Cree una aplicación ASP.NET Core y SQL Database en Azure App Service](tutorial-dotnetcore-sqldb-app.md)** y publique los cambios con Git, con los siguientes comandos:

```bash
git commit -am "configure managed identity"
git push azure main
```

-----

Cuando la página web nueva muestra su lista de tareas pendientes, la aplicación se conecta a la base de datos mediante la identidad administrada.

![Aplicación de Azure después de Migraciones de Code First](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Ahora ya puede editar la lista de tareas pendientes como antes.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes

¿Qué ha aprendido?

> [!div class="checklist"]
> * Habilitar identidades administradas
> * Conceder a SQL Database acceso a la identidad administrada
> * Configurar Entity Framework para utilizar la autenticación de Azure AD con SQL Database
> * Conectarse a SQL Database desde Visual Studio mediante la autenticación de Azure AD

Vaya al siguiente tutorial para aprender a asignar un nombre DNS personalizado a una aplicación web.

> [!div class="nextstepaction"]
> [Asignación de un nombre DNS personalizado existente a Azure App Service](app-service-web-tutorial-custom-domain.md)
