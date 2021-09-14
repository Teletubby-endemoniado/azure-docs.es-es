---
title: Creación de usuarios invitados en Azure AD
description: Aprenda a crear usuarios invitados en Azure AD y a configurarlos como administradores sin usar grupos de Azure AD en Azure SQL Database, Azure SQL Managed Instance y Azure Synapse Analytics
ms.service: sql-db-mi
ms.subservice: security
ms.custom: azure-synapse
ms.topic: how-to
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 05/10/2021
ms.openlocfilehash: 91c6893320273ae29cb504715b5f27365a0161be
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123434239"
---
# <a name="create-azure-ad-guest-users-and-set-as-an-azure-ad-admin"></a>Crear usuarios invitados de Azure AD y establecerlos como administradores de Azure AD

[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Los usuarios invitados de Azure Active Directory (Azure AD) son usuarios que se han importado en Azure AD desde otras instancias de Azure Active Directory o desde sitios externos. Por ejemplo, los usuarios invitados podrían ser usuarios de otras instancias de Azure Active Directory o de cuentas como *\@outlook.com*, *\@hotmail.com*, *\@live.com* o *\@gmail.com*. 

En este artículo se muestra cómo crear un usuario invitado de Azure AD y cómo establecerlo como administrador de Azure AD para Azure SQL Managed Instance o el [servidor lógico de Azure](logical-servers.md) que usa Azure SQL Database y Azure Synapse Analytics, sin tener que agregar el usuario invitado a un grupo dentro de Azure AD.

## <a name="feature-description"></a>Descripción de la característica

Esta característica mejora la limitación actual, que solo permite que los usuarios invitados puedan conectarse a Azure SQL Database, SQL Managed Instance o Azure Synapse Analytics cuando son miembros de un grupo creado en Azure AD. Además, el grupo tenía que asignarse manualmente a un usuario utilizando la instrucción [CREATE USER (Transact-SQL)](/sql/t-sql/statements/create-user-transact-sql) en una base de datos determinada. Una vez creado el usuario de base de datos para el grupo de Azure AD que contenía el usuario invitado, el usuario invitado podía iniciar sesión en la base de datos utilizando Azure Active Directory con la autenticación multifactor. Los usuarios invitados se pueden crear y conectar directamente a SQL Database, SQL Managed Instance o Azure Synapse sin necesidad de agregarlos primero a un grupo de Azure AD y de crear después un usuario de base de datos para ese grupo de Azure AD.

Esta característica también permite establecer directamente el usuario invitado como administrador de AD del servidor lógico o de una instancia administrada. La funcionalidad existente, en la que el usuario invitado puede formar parte de un grupo de Azure AD y ese grupo puede configurarse como administrador de Azure AD para el servidor lógico o la instancia administrada, *no* se ha visto afectada. Los usuarios invitados de la base de datos que forman parte de un grupo de Azure AD tampoco se han visto afectados por este cambio.

Para más información sobre la funcionalidad que permite usar usuarios invitados con grupos de Azure AD, consulte [Uso de la autenticación multifactor de Azure Active Directory](authentication-mfa-ssms-overview.md).

## <a name="prerequisite"></a>Requisito previo

- Si se utiliza PowerShell para configurar un usuario invitado como administrador de Azure AD para el servidor lógico o la instancia administrada, se necesita el módulo [Az.Sql 2.9.0](https://www.powershellgallery.com/packages/Az.Sql/2.9.0) o una versión posterior.

## <a name="create-database-user-for-azure-ad-guest-user"></a>Creación de un usuario de base de datos como usuario invitado de Azure AD 

Siga estos pasos para crear un usuario de base de datos que se utilice como usuario invitado de Azure AD.

### <a name="create-guest-user-in-sql-database-and-azure-synapse"></a>Creación de un usuario invitado en Azure SQL Database y Azure Synapse

1. Asegúrese de que el usuario invitado (por ejemplo, `user1@gmail.com`) ya esté agregado en Azure AD y de que se haya configurado un administrador de Azure AD para el servidor de bases de datos. Para poder autenticarse en Azure Active Directory, se necesita un administrador de Azure AD.

1. Conéctese a la base de datos SQL como administrador de Azure AD o como un usuario de Azure AD con permisos de SQL suficientes como para crear usuarios y ejecute el comando siguiente en la base de datos en la que debe agregarse el usuario invitado:

    ```sql
    CREATE USER [user1@gmail.com] FROM EXTERNAL PROVIDER
    ```

1. Ahora debería haber un usuario de base de datos para el usuario invitado `user1@gmail.com`.

1. Ejecute el comando siguiente para comprobar si el usuario de base de datos se creó correctamente:

    ```sql
    SELECT * FROM sys.database_principals
    ```

1. Desconéctese de la base de datos e inicie sesión de nuevo como el usuario invitado `user1@gmail.com` utilizando [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) y el método de autenticación **Azure Active Directory - Universal con MFA**. Para más información, consulte [Uso de la autenticación multifactor de Azure Active Directory](authentication-mfa-ssms-overview.md).

### <a name="create-guest-user-in-sql-managed-instance"></a>Creación de un usuario invitado en Azure SQL Managed Instance

> [!NOTE]
> Azure SQL Managed Instance admite los inicios de sesión de usuarios de Azure AD, así como de los usuarios de base de datos independiente de Azure AD. El procedimiento siguiente permite crear un inicio de sesión y un usuario para un usuario invitado de Azure AD en SQL Managed Instance. Si lo desea, también puede crear un [usuario de base de datos independiente](/sql/relational-databases/security/contained-database-users-making-your-database-portable) en Azure SQL Managed Instance siguiendo el método que se describe en la sección [Creación de un usuario invitado en Azure SQL Database y Azure Synapse](#create-guest-user-in-sql-database-and-azure-synapse).

1. Asegúrese de que el usuario invitado (por ejemplo, `user1@gmail.com`) ya esté agregado en Azure AD y de que se haya configurado un administrador de Azure AD para Azure SQL Managed Instance. Para poder autenticarse en Azure Active Directory, se necesita un administrador de Azure AD.

1. Conéctese al servidor de Azure SQL Managed Instance como administrador de Azure AD o como un usuario de Azure AD con permisos de SQL suficientes para crear usuarios y ejecute el comando siguiente en la base de datos `master`, con lo que creará un inicio de sesión para el usuario invitado:

    ```sql
    CREATE LOGIN [user1@gmail.com] FROM EXTERNAL PROVIDER
    ```

1. Ahora debería haber un usuario de base de datos para el usuario invitado `user1@gmail.com` en la base de datos `master`.

1. Ejecute el comando siguiente para comprobar si el inicio de sesión se creó correctamente:

    ```sql
    SELECT * FROM sys.server_principals
    ```

1. Ejecute el comando siguiente en la base de datos en la que debe agregarse el usuario invitado: 

    ```sql
    CREATE USER [user1@gmail.com] FROM LOGIN [user1@gmail.com]
    ```

1. Ahora debería haber un usuario de base de datos para el usuario invitado `user1@gmail.com`.

1. Desconéctese de la base de datos e inicie sesión de nuevo como el usuario invitado `user1@gmail.com` utilizando [SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) y el método de autenticación **Azure Active Directory - Universal con MFA**. Para más información, consulte [Uso de la autenticación multifactor de Azure Active Directory](authentication-mfa-ssms-overview.md).

## <a name="setting-a-guest-user-as-an-azure-ad-admin"></a>Configuración de un usuario invitado como administrador de Azure AD

Establezca el administrador de Azure AD mediante Azure Portal, Azure PowerShell o la CLI de Azure. 

### <a name="azure-portal"></a>Azure Portal 

Para configurar un administrador de Azure AD para un servidor lógico o una instancia administrada mediante Azure Portal, siga estos pasos: 

1. Abra [Azure Portal](https://portal.azure.com). 
1. Vaya a la configuración de **Azure Active Directory** del servidor SQL Server o de la instancia administrada. 
1. Seleccione **Establecer administrador**. 
1. En la solicitud emergente de Azure AD, escriba el usuario invitado como, por ejemplo, `guestuser@gmail.com`. 
1. Seleccione este nuevo usuario y guarde la operación. 

Para más información, consulte [Configuración del administrador de Azure AD](authentication-aad-configure.md#azure-ad-admin-with-a-server-in-sql-database). 


### <a name="azure-powershell-sql-database-and-azure-synapse"></a>Azure PowerShell (SQL Database y Azure Synapse)

Para configurar un usuario invitado de Azure AD para un servidor lógico, siga estos pasos:  

1. Asegúrese de que el usuario invitado (por ejemplo, `user1@gmail.com`) ya se haya agregado a Azure AD.

1. Ejecute el siguiente comando de PowerShell para agregar el usuario invitado como administrador de Azure AD en el servidor lógico:

    - Sustituya `<ResourceGroupName>` por el nombre del grupo de recursos de Azure que contiene el servidor lógico.
    - Reemplace `<ServerName>` por el nombre del servidor lógico. Si el nombre del servidor es `myserver.database.windows.net`, sustituya `<Server Name>` por `myserver`.
    - Sustituya `<DisplayNameOfGuestUser>` por el nombre del usuario invitado.

    ```powershell
    Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DisplayName <DisplayNameOfGuestUser>
    ```

También puede usar el comando [az sql server ad-admin](/cli/azure/sql/server/ad-admin) de la CLI de Azure para establecer el usuario invitado como un administrador de Azure AD en el servidor lógico.

### <a name="azure-powershell-sql-managed-instance"></a>Azure PowerShell (SQL Managed Instance)

Para configurar un usuario invitado de Azure AD para una instancia administrada, siga estos pasos:  

1. Asegúrese de que el usuario invitado (por ejemplo, `user1@gmail.com`) ya se haya agregado a Azure AD.

1. Vaya a [Azure Portal](https://portal.azure.com) y acceda al recurso **Azure Active Directory**. En **Administrar**, vaya al panel **Usuarios**. Seleccione el usuario invitado y anote el valor de `Object ID`. 

1. Ejecute el siguiente comando de PowerShell para agregar el usuario invitado como administrador de Azure AD en Azure SQL Managed Instance:

    - Sustituya `<ResourceGroupName>` por el nombre del grupo de recursos de Azure que contiene la instancia de Azure SQL Managed Instance.
    - Sustituya `<ManagedInstanceName>` por el nombre de la instancia de Azure SQL Managed Instance.
    - Sustituya `<DisplayNameOfGuestUser>` por el nombre del usuario invitado.
    - Sustituya `<AADObjectIDOfGuestUser>` por el valor de `Object ID` que anotó anteriormente.

    ```powershell
    Set-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName <ResourceGroupName> -InstanceName "<ManagedInstanceName>" -DisplayName <DisplayNameOfGuestUser> -ObjectId <AADObjectIDOfGuestUser>
    ```

También puede usar el comando [az sql server ad-admin](/cli/azure/sql/mi/ad-admin) de la CLI de Azure para establecer el usuario invitado como administrador de Azure AD en la instancia administrada.


## <a name="next-steps"></a>Pasos siguientes

- [Configuración y administración de la autenticación de Azure AD con Azure SQL](authentication-aad-configure.md)
- [Uso de la autenticación multifactor de Azure Active Directory](authentication-mfa-ssms-overview.md)
- [CREATE USER (Transact-SQL)](/sql/t-sql/statements/create-user-transact-sql)