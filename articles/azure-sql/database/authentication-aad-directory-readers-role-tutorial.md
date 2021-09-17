---
title: Asignación del rol Lectores de directorio a un grupo de Azure AD y administración de las asignaciones de roles
description: Este artículo le guía en la habilitación del rol Lectores de directorio mediante el uso de grupos de Azure AD para administrar las asignaciones de roles de Azure AD con Azure SQL Database, Azure SQL Managed Instance y Azure Synapse Analytics
ms.service: sql-db-mi
ms.subservice: security
ms.custom: azure-synapse
ms.topic: tutorial
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 07/30/2021
ms.openlocfilehash: 73fc178efb78ca1c305a65a154c60f3c49778df9
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121727137"
---
# <a name="tutorial-assign-directory-readers-role-to-an-azure-ad-group-and-manage-role-assignments"></a>Tutorial: Asignación del rol Lectores de directorio a un grupo de Azure AD y administración de las asignaciones de roles

[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

Este artículo le guía en la creación de un grupo en Azure Active Directory (Azure AD) y en la asignación a ese grupo del rol [**Lectores de directorio**](../../active-directory/roles/permissions-reference.md#directory-readers). Los permisos de Lectores de directorio permiten a los propietarios del grupo agregar miembros adicionales al mismo, como una [identidad administrada](../../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types) de [Azure SQL Database](sql-database-paas-overview.md), [Azure SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md) y [Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md). De esta forma, se evita la necesidad de que un [administrador global ](../../active-directory/roles/permissions-reference.md#global-administrator) o un [administrador de roles con privilegios](../../active-directory/roles/permissions-reference.md#privileged-role-administrator) asignen el rol Lectores de directorio directamente para cada identidad de servidor lógico de Azure SQL en el inquilino.

En este tutorial se usa la característica que se presentó en [Uso de grupos de Azure AD para administrar asignaciones de roles](../../active-directory/roles/groups-concept.md). 

Para obtener más información sobre las ventajas de asignar el rol Lectores de directorio a un grupo de Azure AD para Azure SQL, consulte [Rol Lectores de directorio en Azure Active Directory para Azure SQL](authentication-aad-directory-readers-role.md).

## <a name="prerequisites"></a>Requisitos previos

- Una instancia de Azure AD. Para más información, consulte [Configuración y administración de la autenticación de Azure AD con Azure SQL](authentication-aad-configure.md).
- Una base de datos SQL Database, SQL Managed Instance o Azure Synapse.

## <a name="directory-readers-role-assignment-using-the-azure-portal"></a>Asignación del rol Lectores de directorio mediante Azure Portal

### <a name="create-a-new-group-and-assign-owners-and-role"></a>Creación de un nuevo grupo y asignación de propietarios y roles

1. Para esta configuración inicial, se requiere un usuario con los permisos [Administrador global](../../active-directory/roles/permissions-reference.md#global-administrator) o [Administrador de roles con privilegios](../../active-directory/roles/permissions-reference.md#privileged-role-administrator).
1. Haga que el usuario con privilegios inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya al recurso **Azure Active Directory**. En **Administrado**, vaya a **Grupos**. Seleccione **Nuevo grupo** para crear un grupo.
1. Seleccione **Seguridad** como el tipo de grupo y rellene el resto de los campos. Asegúrese de que el valor de **Azure AD roles can be assigned to the group** (Los roles de Azure AD se pueden asignar a un grupo) se haya cambiado a **Sí**. A continuación, asigne el rol **Lectores de directorio** de Azure AD al grupo.
1. Asigne usuarios de Azure AD como propietarios al grupo que se creó. El propietario de un grupo puede ser un usuario de AD normal sin ningún rol administrativo de Azure AD asignado. El propietario debe ser un usuario que administre SQL Database, SQL Managed Instance o Azure Synapse.

   :::image type="content" source="media/authentication-aad-directory-readers-role/new-group.png" alt-text="aad-new-group":::

1. Seleccione **Crear**

### <a name="checking-the-group-that-was-created"></a>Comprobación del grupo que se creó

> [!NOTE]
> Asegúrese de que el valor de **Tipo de grupo** es **Seguridad**. Los grupos de *Microsoft 365* no se admiten para Azure SQL.

Para comprobar y administrar el grupo que se creó, vuelva al panel **Grupos** en Azure Portal y busque el nombre del grupo. Se pueden agregar más propietarios y miembros en el menú **Propietarios** y **Miembros** de la opción **Administrar** después de seleccionar el grupo. También puede revisar los **roles asignados** para el grupo.

:::image type="content" source="media/authentication-aad-directory-readers-role/azure-ad-group-created.png" alt-text="Captura de pantalla de un panel de grupo, con los vínculos que abren los menús de configuración para miembros, propietarios y roles asignados resaltados.":::

### <a name="add-azure-sql-managed-identity-to-the-group"></a>Adición de la identidad administrada de Azure SQL al grupo

> [!NOTE]
> En este ejemplo, vamos a usar SQL Managed Instance, pero se pueden aplicar pasos similares para SQL Database o Azure Synapse y lograr los mismos resultados.

En los pasos siguientes, ya no es necesario el usuario administrador global o administrador de roles con privilegios.

1. Inicie sesión en Azure Portal como el usuario que administra SQL Managed Instance y es propietario del grupo creado anteriormente.

1. Busque el nombre del recurso **SQL Managed Instance** en Azure Portal.

   :::image type="content" source="media/authentication-aad-directory-readers-role/azure-ad-managed-instance.png" alt-text="Captura de pantalla de la ventana de instancias administradas de SQL, con el nombre de la instancia de SQL ssomitest y el nombre de subred ManagedInstance resaltados.":::

   Durante la creación de SQL Managed Instance, se creó una identidad de Azure para la instancia. La identidad creada tiene el mismo nombre que el prefijo del nombre de SQL Managed Instance. Puede encontrar la entidad de servicio de la identidad de SQL Managed Instance que creó como una aplicación de Azure AD. Para ello, siga estos pasos:

    - Vaya al recurso **Azure Active Directory**. En la opción **Administrar**, seleccione **Aplicaciones empresariales**. El **identificador de objeto** es la identidad de la instancia.
    
    :::image type="content" source="media/authentication-aad-directory-readers-role/azure-ad-managed-instance-service-principal.png" alt-text="Captura de pantalla de la página Aplicaciones empresariales para un recurso Azure Active Directory, con el identificador de objeto de la instancia administrada de SQL resaltado.":::

1. Vaya al recurso **Azure Active Directory**. En **Administrado**, vaya a **Grupos**. Seleccione el grupo que creó. En la opción **Administrada** del grupo, seleccione **Miembros**. Seleccione **Agregar miembros** y agregue la entidad de servicio de SQL Managed Instance como miembro del grupo. Para ello, busque el nombre que se encuentra arriba.

   :::image type="content" source="media/authentication-aad-directory-readers-role/azure-ad-add-managed-instance-service-principal.png" alt-text="Captura de pantalla de la página Miembros de un recurso Azure Active Directory, con las opciones para agregar una instancia administrada de SQL como un nuevo miembro resaltadas.":::

> [!NOTE]
> Puede tardar unos minutos en propagar los permisos de la entidad de servicio mediante el sistema de Azure y permitir el acceso a Azure AD Graph API. Es posible que tenga que esperar unos minutos antes de aprovisionar un administrador de Azure AD para SQL Managed Instance.

### <a name="remarks"></a>Observaciones

Para SQL Database y Azure Synapse, la identidad del servidor se puede crear durante la creación del servidor lógico de Azure SQL o después. Para más información sobre cómo crear o establecer la identidad del servidor en SQL Database o Azure Synapse, consulte [Habilitación de entidades de servicio para crear usuarios de Azure AD](authentication-aad-service-principal.md#enable-service-principals-to-create-azure-ad-users).

En el caso de SQL Managed Instance, el rol **Lectores de directorio** debe estar asignado a la identidad de la instancia administrada si desea [configurar un administrador de Azure AD para la instancia administrada](authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance).

No es necesario asignar el rol **Lectores de directorio** a la identidad del servidor para SQL Database o Azure Synapse al configurar un administrador de Azure AD para el servidor lógico. Sin embargo, para habilitar la creación de un objeto de Azure AD en SQL Database o Azure Synapse en nombre de una aplicación de Azure AD, se requiere el rol **Lectores de directorio**. Si el rol no se asigna a la identidad del servidor lógico de SQL, se producirá un error al crear los usuarios de Azure AD en Azure SQL. Para más información, consulte [Entidad de servicio de Azure Active Directory con Azure SQL](authentication-aad-service-principal.md).

## <a name="directory-readers-role-assignment-using-powershell"></a>Asignación del rol Lectores de directorio mediante PowerShell

> [!IMPORTANT]
> Estos pasos iniciales tiene que ejecutarlos [administrador global](../../active-directory/roles/permissions-reference.md#global-administrator) o un [Administrador de roles con privilegios](../../active-directory/roles/permissions-reference.md#privileged-role-administrator). Además de PowerShell, Azure AD ofrece Microsoft Graph API para [crear un grupo al que se puedan asignar roles en Azure AD](../../active-directory/roles/groups-create-eligible.md#microsoft-graph-api).

1. Descargue el módulo de PowerShell de Azure AD con los siguientes comandos. Tiene que ejecutar PowerShell como administrador.

    ```powershell
    Install-Module azuread
    Import-Module azuread
    #To verify that the module is ready to use, use the following command:
    Get-Module azuread
    ```

1. Conéctese a su inquilino de Azure AD.

    ```powershell
    Connect-AzureAD
    ```

1. Cree un grupo de seguridad para asignar el rol **Lectores de directorio**.

    - `DirectoryReaderGroup`, `Directory Reader Group` y `DirRead` se pueden cambiar en función de sus preferencias.

    ```powershell
    $group = New-AzureADMSGroup -DisplayName "DirectoryReaderGroup" -Description "Directory Reader Group" -MailEnabled $False -SecurityEnabled $true -MailNickName "DirRead" -IsAssignableToRole $true
    $group
    ```

1. Asigne el rol **Lectores de directorio** al grupo.

    ```powershell
    # Displays the Directory Readers role information
    $roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Directory Readers'" 
    $roleDefinition
    ```

    ```powershell
    # Assigns the Directory Readers role to the group
    $roleAssignment = New-AzureADMSRoleAssignment -ResourceScope '/' -RoleDefinitionId $roleDefinition.Id -PrincipalId $group.Id
    $roleAssignment
    ```

1. Asigne propietarios al grupo.

    - Reemplace `<username>` por el usuario que desee que posea este grupo. Se pueden agregar varios propietarios si repite estos pasos.

    ```powershell
    $RefObjectID = Get-AzureADUser -ObjectId "<username>"
    $RefObjectID
    ```

    ```powershell
    $GrOwner = Add-AzureADGroupOwner -ObjectId $group.ID -RefObjectId $RefObjectID.ObjectID
    ```

    Compruebe los propietarios del grupo con el siguiente comando:

    ```powershell
    Get-AzureADGroupOwner -ObjectId $group.ID
    ```

    También puede comprobar los propietarios del grupo en [Azure Portal](https://portal.azure.com). Siga los pasos descritos en [Comprobación del grupo que se creó](#checking-the-group-that-was-created).

### <a name="assigning-the-service-principal-as-a-member-of-the-group"></a>Asignación de la entidad de servicio como miembro del grupo

En los pasos siguientes, ya no es necesario el usuario administrador global o administrador de roles con privilegios.

1. Mediante un propietario del grupo, que también administra el recurso de Azure SQL, ejecute el siguiente comando para conectarse a Azure AD.

    ```powershell
    Connect-AzureAD
    ```

1. Asigne la entidad de servicio como miembro del grupo que se creó.

    - Reemplace `<ServerName>` por el nombre del servidor lógico de Azure SQL o por el nombre de la instancia administrada. Para más información, consulte la sección [Incorporación de la identidad del servicio Azure SQL al grupo](#add-azure-sql-managed-identity-to-the-group).

    ```powershell
    # Returns the service principal of your Azure SQL resource
    $miIdentity = Get-AzureADServicePrincipal -SearchString "<ServerName>"
    $miIdentity
    ```

    ```powershell
    # Adds the service principal to the group as a member
    Add-AzureADGroupMember -ObjectId $group.ID -RefObjectId $miIdentity.ObjectId 
    ```

    El siguiente comando devolverá el identificador de objeto de la entidad de servicio que indica que se ha agregado al grupo:

    ```powershell
    Add-AzureADGroupMember -ObjectId $group.ID -RefObjectId $miIdentity.ObjectId
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Rol Lectores de directorio en Azure Active Directory para Azure SQL](authentication-aad-directory-readers-role.md)
- [Tutorial: Creación de usuarios de Azure AD mediante aplicaciones de Azure AD](authentication-aad-service-principal-tutorial.md)
- [Configuración y administración de la autenticación de Azure AD con Azure SQL](authentication-aad-configure.md)