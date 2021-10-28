---
title: Creación de usuarios de Azure AD mediante entidades de servicio
description: Este tutorial le guiará en la creación de un usuario de Azure AD con una aplicación de Azure AD (entidades de servicio) en Azure SQL Database.
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 10/21/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 91daa2bb029eb8a4769ba10069b3aa9011f74380
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130248031"
---
# <a name="tutorial-create-azure-ad-users-using-azure-ad-applications"></a>Tutorial: Creación de usuarios de Azure AD mediante aplicaciones de Azure AD

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Este artículo le guía por el proceso de creación de usuarios de Azure AD en Azure SQL Database, mediante entidades de servicio de Azure (aplicaciones de Azure AD). Esta funcionalidad ya existe en Azure SQL Managed Instance, pero ahora se está introduciendo en Azure SQL Database. Para admitir este escenario, se debe generar una identidad de Azure AD y asignarla al servidor lógico de Azure SQL.

Para más información sobre la autenticación de Azure AD para Azure SQL, consulte el artículo [Uso de la autenticación de Azure Active Directory](authentication-aad-overview.md).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> - Asignar una identidad al servidor lógico de Azure SQL
> - Asignar el permiso Lectores de directorio a la identidad del servidor lógico de SQL
> - Crear una entidad de servicio (una aplicación de Azure AD) en Azure AD
> - Crear un usuario de entidad de servicio en Azure SQL Database
> - Crear un usuario de Azure AD diferente en SQL Database con un usuario de entidad de servicio de Azure AD

## <a name="prerequisites"></a>Requisitos previos

- Una implementación de [Azure SQL Database](single-database-create-quickstart.md) existente. Suponemos que tiene una base de datos SQL Database en funcionamiento para este tutorial.
- Acceso a una instancia de Azure Active Directory existente.
- Se necesita el módulo [Az.Sql 2.9.0](https://www.powershellgallery.com/packages/Az.Sql/2.9.0) o posterior cuando se usa PowerShell para configurar una aplicación de Azure AD individual como administrador de Azure AD para Azure SQL. Asegúrese de que ha actualizado al módulo más reciente.

## <a name="assign-an-identity-to-the-azure-sql-logical-server"></a>Asignación de una identidad al servidor lógico de Azure SQL

1. Conéctese a Azure Active Directory. Tendrá que buscar el identificador de inquilino. Para encontrarlo, vaya a [Azure Portal](https://portal.azure.com) y vaya al recurso de **Azure Active Directory**. En el panel **Información general**, debería ver el **Id. de inquilino**. Ejecute el siguiente comando de PowerShell:

    - Remplace `<TenantId>` por su **identificador de inquilino**.

    ```powershell
    Connect-AzAccount -Tenant <TenantId>
    ```

    Anote el valor de `TenantId` para usarlo más adelante en este tutorial.

1. Genere una identidad de Azure AD y asígnela al servidor lógico de Azure SQL. Ejecute el siguiente comando de PowerShell:

    - Reemplace `<resource group>` y `<server name>` por sus recursos. Si el nombre del servidor es `myserver.database.windows.net`, reemplace `<server name>` por `myserver`.

    ```powershell
    Set-AzSqlServer -ResourceGroupName <resource group> -ServerName <server name> -AssignIdentity
    ```

    Para más información, consulte el comando [Set-AzSqlServer](/powershell/module/az.sql/set-azsqlserver).

    > [!IMPORTANT]
    > Si se configura una identidad de Azure AD para el servidor lógico de Azure SQL, se debe conceder el permiso [**Lectores de directorios**](../../active-directory/roles/permissions-reference.md#directory-readers) a la identidad. Veremos este paso en la sección siguiente. **No** omita este paso porque la autenticación de Azure AD dejará de funcionar.

    - Si ha usado el comando [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) con el parámetro `AssignIdentity` para crear un servidor de SQL Server en el pasado, tendrá que ejecutar el comando [Set-AzSqlServer](/powershell/module/az.sql/set-azsqlserver) posteriormente como un comando independiente para habilitar esta propiedad en el tejido de Azure.

1. Compruebe que la identidad del servidor se asignara correctamente. Ejecute el siguiente comando de PowerShell:

    - Reemplace `<resource group>` y `<server name>` por sus recursos. Si el nombre del servidor es `myserver.database.windows.net`, reemplace `<server name>` por `myserver`.
    
    ```powershell
    $xyz = Get-AzSqlServer  -ResourceGroupName <resource group> -ServerName <server name>
    $xyz.identity
    ```

    La salida debería mostrar `PrincipalId`, `Type` y `TenantId`. La identidad asignada es el `PrincipalId`.

1. También puede comprobar la identidad si va a [Azure Portal](https://portal.azure.com).

    - En el recurso de **Azure Active Directory**, vaya a **Aplicaciones empresariales**. Escriba el nombre del servidor lógico de SQL. Verá que tiene un **identificador de objeto** asociado al recurso.
    
    :::image type="content" source="media/authentication-aad-service-principals-tutorial/enterprise-applications-object-id.png" alt-text="object-id":::

## <a name="assign-directory-readers-permission-to-the-sql-logical-server-identity"></a>Asignar el permiso Lectores de directorios a la identidad del servidor lógico de SQL

Para que la identidad asignada de Azure AD funcione correctamente en Azure SQL, se debe conceder el permiso `Directory Readers` de Azure AD a la identidad del servidor.

Para conceder el permiso necesario, ejecute el siguiente script.

> [!NOTE] 
> Este script debe ejecutarlo un rol `Global Administrator` o `Privileged Roles Administrator` de Azure AD.
>
> Puede asignar el rol `Directory Readers` a un grupo en Azure AD. A continuación, los propietarios del grupo pueden agregar la identidad administrada como miembro de este grupo, lo que omitiría la necesidad de que un usuario `Global Administrator` o `Privileged Roles Administrator` concedieran el rol `Directory Readers`. Para más información sobre esta característica, consulte [Rol Lectores de directorio en Azure Active Directory de Azure SQL](authentication-aad-directory-readers-role.md).

- Reemplace `<TenantId>` por el valor de `TenantId` recopilado anteriormente.
- Reemplace `<server name>` por el nombre del servidor lógico de SQL. Si el nombre del servidor es `myserver.database.windows.net`, reemplace `<server name>` por `myserver`.

```powershell
# This script grants Azure "Directory Readers" permission to a Service Principal representing the Azure SQL logical server
# It can be executed only by a "Global Administrator" or "Privileged Roles Administrator" type of user.
# To check if the "Directory Readers" permission was granted, execute this script again

Import-Module AzureAD
Connect-AzureAD -TenantId "<TenantId>"    #Enter your actual TenantId
$AssignIdentityName = "<server name>"     #Enter Azure SQL logical server name
 
# Get Azure AD role "Directory Users" and create if it doesn't exist
$roleName = "Directory Readers"
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq $roleName}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq $roleName}
}
 
# Get service principal for server
$roleMember = Get-AzureADServicePrincipal -SearchString $AssignIdentityName
$roleMember.Count
if ($roleMember -eq $null) {
    Write-Output "Error: No Service Principals with name '$($AssignIdentityName)', make sure that AssignIdentityName parameter was entered correctly."
    exit
}

if (-not ($roleMember.Count -eq 1)) {
    Write-Output "Error: More than one service principal with name pattern '$($AssignIdentityName)'"
    Write-Output "Dumping selected service principals...."
    $roleMember
    exit
}
 
# Check if service principal is already member of readers role
$allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
$selDirReader = $allDirReaders | where{$_.ObjectId -match $roleMember.ObjectId}
 
if ($selDirReader -eq $null) {
    # Add principal to readers role
    Write-Output "Adding service principal '$($AssignIdentityName)' to 'Directory Readers' role'..."
    Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId
    Write-Output "'$($AssignIdentityName)' service principal added to 'Directory Readers' role'..."
 
    #Write-Output "Dumping service principal '$($AssignIdentityName)':"
    #$allDirReaders = Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
    #$allDirReaders | where{$_.ObjectId -match $roleMember.ObjectId}
} else {
    Write-Output "Service principal '$($AssignIdentityName)' is already member of 'Directory Readers' role'."
}
```

> [!NOTE]
> La salida de este script anterior indicará si se ha concedido el permiso Lectores de directorios a la identidad. Puede volver a ejecutar el script si no está seguro de si se concedió el permiso.

Para más información sobre un enfoque similar para establecer el permiso **Lectores de directorios** para SQL Managed Instance, consulte [Aprovisionamiento de un administrador de Azure AD (SQL Managed Instance)](authentication-aad-configure.md#powershell).

## <a name="create-a-service-principal-an-azure-ad-application-in-azure-ad"></a>Crear una entidad de servicio (una aplicación de Azure AD) en Azure AD

1. Siga esta guía para [registrar la aplicación](active-directory-interactive-connect-azure-sql-db.md#register-your-app-and-set-permissions).

2. También tendrá que crear un secreto de cliente para iniciar sesión. Siga esta guía para [cargar un certificado o crear un secreto para iniciar sesión](../../active-directory/develop/howto-create-service-principal-portal.md#authentication-two-options).

3. Anote la siguiente información del registro de aplicación. Debe estar disponible en el panel **Información general**:
    - **Identificador de la aplicación**
    - **Identificador de inquilino** debe ser el mismo que antes.

En este tutorial, se usará *AppSP* como entidad de servicio principal y *myapp* como usuario de la entidad de servicio secundaria que se creará en Azure SQL mediante *AppSP*. Tendrá que crear dos aplicaciones, *AppSP* y *myapp*.

Para más información sobre cómo crear una aplicación de Azure AD, consulte cómo [usar el portal para crear una aplicación de Azure AD y una entidad de servicio que puedan acceder a los recursos](../../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="create-the-service-principal-user-in-azure-sql-database"></a>Cree el usuario de la entidad de servicio en Azure SQL Database

Una vez creada una entidad de servicio en Azure AD, cree el usuario en SQL Database. Tendrá que conectarse a su base de datos de SQL Database con un inicio de sesión válido con permisos para crear usuarios en la base de datos.

1. Cree el usuario *AppSP* en la base de datos de SQL Database con el siguiente comando de T-SQL:

    ```sql
    CREATE USER [AppSP] FROM EXTERNAL PROVIDER
    GO
    ```

2. Conceda el permiso `db_owner` a *AppSP*, que permite al usuario crear otros usuarios de Azure AD en la base de datos.

    ```sql
    EXEC sp_addrolemember 'db_owner', [AppSP]
    GO
    ```

    Para más información, consulte [sp_addrolemember](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql).

    Como alternativa, se puede conceder el permiso `ALTER ANY USER` en lugar de asignar el rol `db_owner`. De esta forma, permitirá a la entidad de servicio agregar otros usuarios de Azure AD.

    ```sql
    GRANT ALTER ANY USER TO [AppSp]
    GO
    ```

    > [!NOTE]
    > La configuración anterior no es necesaria cuando se establece *AppSP* como administrador de Azure AD para el servidor. Para establecer la entidad de servicio como administrador de AD para el servidor lógico de SQL, puede usar los comandos de Azure Portal, PowerShell o la CLI de Azure. Para más información, consulte [Administración del aprovisionamiento de Azure AD (SQL Database) ](authentication-aad-configure.md?tabs=azure-powershell#powershell-for-sql-database-and-azure-synapse).

## <a name="create-an-azure-ad-user-in-sql-database-using-an-azure-ad-service-principal"></a>Creación de un usuario de Azure AD en SQL Database mediante una entidad de servicio de Azure AD

> [!IMPORTANT]
> La entidad de servicio usada para iniciar sesión en SQL Database debe tener un secreto de cliente. Si no lo tiene, siga el paso 2 de [Creación de una entidad de servicio (una aplicación de Azure AD) en Azure AD](#create-a-service-principal-an-azure-ad-application-in-azure-ad). Este secreto de cliente debe agregarse como parámetro de entrada en el script siguiente.

1. Use el siguiente script para crear un usuario de entidad de servicio de Azure AD *myapp* mediante la entidad de servicio *AppSP*.

    - Reemplace `<TenantId>` por el valor de `TenantId` recopilado anteriormente.
    - Reemplace `<ClientId>` por el valor de `ClientId` recopilado anteriormente.
    - Reemplace `<ClientSecret>` por el secreto de cliente que creó anteriormente.
    - Reemplace `<server name>` por el nombre del servidor lógico de SQL. Si el nombre del servidor es `myserver.database.windows.net`, reemplace `<server name>` por `myserver`.
    - Reemplace `<database name>` por el nombre de su base de datos SQL Database.

    ```powershell
    # PowerShell script for creating a new SQL user called myapp using application AppSP with secret
    # AppSP is part of an Azure AD admin for the Azure SQL server below
    
    # Download latest  MSAL  - https://www.powershellgallery.com/packages/MSAL.PS
    Import-Module MSAL.PS
    
    $tenantId = "<TenantId>"   # tenantID (Azure Directory ID) were AppSP resides
    $clientId = "<ClientId>"   # AppID also ClientID for AppSP     
    $clientSecret = "<ClientSecret>"   # Client secret for AppSP 
    $scopes = "https://database.windows.net/.default" # The end-point
    
    $result = Get-MsalToken -RedirectUri $uri -ClientId $clientId -ClientSecret (ConvertTo-SecureString $clientSecret -AsPlainText -Force) -TenantId $tenantId -Scopes $scopes
    
    $Tok = $result.AccessToken
    #Write-host "token"
    $Tok
      
    $SQLServerName = "<server name>"    # Azure SQL logical server name 
    $DatabaseName = "<database name>"     # Azure SQL database name
    
    Write-Host "Create SQL connection string"
    $conn = New-Object System.Data.SqlClient.SQLConnection 
    $conn.ConnectionString = "Data Source=$SQLServerName.database.windows.net;Initial Catalog=$DatabaseName;Connect Timeout=30"
    $conn.AccessToken = $Tok
    
    Write-host "Connect to database and execute SQL script"
    $conn.Open() 
    $ddlstmt = 'CREATE USER [myapp] FROM EXTERNAL PROVIDER;'
    Write-host " "
    Write-host "SQL DDL command"
    $ddlstmt
    $command = New-Object -TypeName System.Data.SqlClient.SqlCommand($ddlstmt, $conn)       
    
    Write-host "results"
    $command.ExecuteNonQuery()
    $conn.Close()
    ``` 

    Como alternativa, puede usar el ejemplo de código del blog, [Autenticación de entidades de servicio de Azure AD en una base de datos SQL: ejemplo de código](https://techcommunity.microsoft.com/t5/azure-sql-database/azure-ad-service-principal-authentication-to-sql-db-code-sample/ba-p/481467). Modifique el script para ejecutar una instrucción DDL `CREATE USER [myapp] FROM EXTERNAL PROVIDER`. El mismo script se puede usar para crear un usuario, o un grupo de usuarios, normales de Azure AD en SQL Database.

    
2. Compruebe si el usuario *myapp* existe en la base de datos; para ello, ejecute el comando siguiente:

    ```sql
    SELECT name, type, type_desc, CAST(CAST(sid as varbinary(16)) as uniqueidentifier) as appId from sys.database_principals WHERE name = 'myapp'
    GO
    ```

    Debería mostrarse una salida similar a la siguiente:

    ```output
    name    type    type_desc   appId
    myapp   E   EXTERNAL_USER   6d228f48-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Entidad de servicio de Azure Active Directory con Azure SQL](authentication-aad-service-principal.md)
- [¿Qué son las identidades administradas de recursos de Azure?](../../active-directory/managed-identities-azure-resources/overview.md)
- [Cómo usar identidades administradas para App Service y Azure Functions](../../app-service/overview-managed-identity.md)
- [Autenticación de entidades de servicio en Azure AD en una base de datos SQL: ejemplo de código](https://techcommunity.microsoft.com/t5/azure-sql-database/azure-ad-service-principal-authentication-to-sql-db-code-sample/ba-p/481467)
- [Objetos de aplicación y de entidad de servicio de Azure Active Directory](../../active-directory/develop/app-objects-and-service-principals.md)
- [Creación de una entidad de servicio de Azure con Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps)
- [Rol Lectores de directorio en Azure Active Directory para Azure SQL](authentication-aad-directory-readers-role.md)
