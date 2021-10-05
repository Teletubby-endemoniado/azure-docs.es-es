---
title: Configuración del control de acceso basado en roles de la cuenta de Azure Cosmos DB con Azure AD
description: Aprenda a configurar el control de acceso basado en roles con Azure Active Directory para la cuenta de Azure Cosmos DB.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: how-to
ms.date: 07/21/2021
ms.author: thweiss
ms.openlocfilehash: 29aeee156ee87c055a3581e9dc2fd0bd86a2064d
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128551052"
---
# <a name="configure-role-based-access-control-with-azure-active-directory-for-your-azure-cosmos-db-account"></a>Configuración del control de acceso basado en roles con Azure Active Directory para la cuenta de Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

> [!NOTE]
> Este artículo trata sobre el control de acceso basado en roles para las operaciones del plano de datos de Azure Cosmos DB. Si usa operaciones del plano de administración, consulte la documentación sobre el [control de acceso basado en roles](role-based-access-control.md) aplicado a las operaciones de este plano.

Azure Cosmos DB expone un sistema de control de acceso basado en roles integrado que le permite:

- Autenticar las solicitudes de datos con una identidad de Azure Active Directory (Azure AD).
- Autorizar las solicitudes de datos con un modelo de permisos basado en roles específico.

## <a name="concepts"></a>Conceptos

El control de acceso basado en roles del plano de datos de Azure Cosmos DB se basa en conceptos que se encuentran normalmente en otros sistemas de control de acceso como el [RBAC de Azure](../role-based-access-control/overview.md):

- El [modelo de permiso](#permission-model) se compone de un conjunto de **acciones**; cada una de estas acciones se asigna a una o varias operaciones de base de datos. Algunos ejemplos de acciones incluyen la lectura o escritura de un elemento, o la ejecución de una consulta.
- Los usuarios de Azure Cosmos DB crean **[definiciones de roles](#role-definitions)** que contienen una lista de acciones permitidas.
- Las definiciones de roles se asignan a identidades de Azure AD específicas a través de **[asignaciones de roles](#role-assignments)** . Una asignación de roles también define el ámbito al que se aplica la definición de roles. Actualmente, hay tres ámbitos:
    - Una cuenta de Azure Cosmos DB.
    - Una base de datos de Azure Cosmos DB.
    - Un contenedor de Azure Cosmos DB.

  :::image type="content" source="./media/how-to-setup-rbac/concepts.png" alt-text="Conceptos del control de acceso basado en roles":::

## <a name="permission-model"></a><a id="permission-model"></a> Modelo de permiso

> [!IMPORTANT]
> Este modelo de permisos cubre solo las operaciones de base de datos que implican la lectura y escritura de datos. *No* cubre ningún tipo de operación de administración en recursos de administración, por ejemplo:
> - Crear, reemplazar o eliminar base de datos
> - Crear, reemplazar o eliminar contenedor
> - Reemplazar el rendimiento del contenedor
> - Crear, reemplazar, eliminar o leer procedimientos almacenados
> - Crear, reemplazar, eliminar o leer desencadenadores
> - Crear, reemplazar, eliminar o leer funciones definidas por el usuario
>
> *No puede usar ningún SDK del plano de datos de Azure Cosmos DB* para autenticar las operaciones de administración con una identidad de Azure AD. En su lugar, debe usar [Azure RBAC](role-based-access-control.md) mediante una de las siguientes opciones:
> - [Plantillas de Azure Resource Manager (plantillas de ARM)](./sql/manage-with-templates.md)
> - [Scripts de Azure PowerShell](./sql/manage-with-powershell.md)
> - [Scripts de la CLI de Azure](./sql/manage-with-cli.md)
> - Bibliotecas de administración de Azure disponibles en:
>   - [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.CosmosDB/)
>   - [Java](https://search.maven.org/artifact/com.azure.resourcemanager/azure-resourcemanager-cosmos)
>   - [Python](https://pypi.org/project/azure-mgmt-cosmosdb/)
>   
> Leer base de datos y Leer contenedor se consideran [solicitudes de metadatos](#metadata-requests). Se puede conceder acceso a estas operaciones como se indica en la sección siguiente.

En la tabla siguiente se enumeran todas las acciones expuestas por el modelo de permiso.

| Nombre | Operaciones correspondientes de la base de datos |
|---|---|
| `Microsoft.DocumentDB/databaseAccounts/readMetadata` | Lectura de metadatos de la cuenta. Consulte [Solicitudes de metadatos](#metadata-requests) para más información. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/create` | Crear un nuevo elemento. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/read` | Lectura de un elemento individual por su identificador y clave de partición (lectura de punto). |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/replace` | Reemplazo de un elemento existente. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/upsert` | "Actualización/inserción (upsert)" de un elemento, lo que significa que se creará si no existe o se reemplazará si existe. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/delete` | Eliminación de un elemento. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeQuery` | Ejecución de una [consulta SQL](sql-query-getting-started.md). |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/readChangeFeed` | Lectura desde la [fuente de cambios](read-change-feed.md)del contenedor. |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeStoredProcedure` | Ejecución de un [procedimiento almacenado](stored-procedures-triggers-udfs.md). |
| `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/manageConflicts` | Administración de [conflictos](conflict-resolution-policies.md) de las cuentas de regiones con escritura múltiple (es decir, enumeración y eliminación de elementos de la fuente de conflictos). |

Se admiten caracteres comodín en los niveles de *contenedores* y *elementos*:

- `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/*`
- `Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/*`

### <a name="metadata-requests"></a><a id="metadata-requests"></a> Solicitudes de metadatos

Cuando se usan los SDK de Azure Cosmos DB, estos emiten solicitudes de metadatos de solo lectura durante la inicialización y atienden solicitudes de datos específicas. Estas solicitudes de metadatos permiten capturar varios detalles de configuración, como: 

- La configuración global de la cuenta, lo cual incluye las regiones de Azure en las que está disponible la cuenta.
- La clave de partición de los contenedores o su directiva de indexación.
- La lista de particiones físicas que crean un contenedor y sus direcciones.

*No* capturan ninguno de los datos almacenados en su cuenta.

Para garantizar la mejor transparencia de nuestro modelo de permisos, estas solicitudes de metadatos se describen explícitamente en la acción `Microsoft.DocumentDB/databaseAccounts/readMetadata`. Esta acción debe permitirse en todas las situaciones en las que se acceda a la cuenta de Azure Cosmos DB a través de uno de los SDK de Azure Cosmos DB. Se puede asignar (mediante una asignación de roles) en cualquier nivel de la jerarquía de Azure Cosmos DB (es decir, en los niveles de cuenta, base de datos o contenedor).

Las solicitudes de metadatos reales que permite la acción `Microsoft.DocumentDB/databaseAccounts/readMetadata` dependen del ámbito al que está asignada la acción:

| Ámbito | Solicitudes permitidas por la acción |
|---|---|
| Cuenta | - Enumeración de las bases de datos de la cuenta<br>- Para cada base de datos de la cuenta, las acciones permitidas en el ámbito de la base de datos |
| Base de datos | - Lectura de metadatos de la base de datos<br>- Enumeración de los contenedores de la base de datos<br>- Para cada contenedor de la base de datos, las acciones permitidas en el ámbito de contenedor |
| Contenedor | - Lectura de metadatos del contenedor<br>- Enumeración de las particiones físicas en el contenedor<br>- Resolución de la dirección de cada partición física |

## <a name="built-in-role-definitions"></a>Definiciones de roles integradas

Azure Cosmos DB expone dos definiciones de roles integrados:

| ID | Nombre | Acciones incluidas |
|---|---|---|
| 00000000-0000-0000-0000-000000000001 | Lector de datos integrado de Cosmos DB | `Microsoft.DocumentDB/databaseAccounts/readMetadata`<br>`Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/read`<br>`Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeQuery`<br>`Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/readChangeFeed` |
| 00000000-0000-0000-0000-000000000002 | Colaborador de datos integrado de Cosmos DB | `Microsoft.DocumentDB/databaseAccounts/readMetadata`<br>`Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/*`<br>`Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/*` |

## <a name="create-custom-role-definitions"></a><a id="role-definitions"></a> Creación de definiciones de roles personalizados

Al crear una definición de roles, debe proporcionar lo siguiente:

- El nombre de la cuenta de Azure Cosmos DB.
- El grupo de recursos que contiene la cuenta.
- El tipo de la definición de roles: `CustomRole`.
- El nombre de la definición de roles.
- Una lista de [acciones](#permission-model) que desea que permita el rol.
- Uno o varios ámbitos en los que se puede asignar la definición de roles; los ámbitos admitidos son:
    - `/` (nivel de cuenta),
    - `/dbs/<database-name>` (nivel de base de datos),
    - `/dbs/<database-name>/colls/<container-name>` (nivel de contenedor).

> [!NOTE]
> Las operaciones que se describen a continuación están disponibles en:
> - Azure PowerShell: [Az.CosmosDB, versión 1.2.0-preview](https://www.powershellgallery.com/packages/Az.CosmosDB/1.2.0) o superior
> - [CLI de Azure](/cli/azure/install-azure-cli): versión 2.24.0 o superior.

### <a name="using-azure-powershell"></a>Uso de Azure PowerShell

Cree un rol denominado *MyReadOnlyRole* que solo contenga acciones de lectura:

```powershell
$resourceGroupName = "<myResourceGroup>"
$accountName = "<myCosmosAccount>"
New-AzCosmosDBSqlRoleDefinition -AccountName $accountName `
    -ResourceGroupName $resourceGroupName `
    -Type CustomRole -RoleName MyReadOnlyRole `
    -DataAction @( `
        'Microsoft.DocumentDB/databaseAccounts/readMetadata',
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/read', `
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeQuery', `
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/readChangeFeed') `
    -AssignableScope "/"
```

Cree un rol denominado *MyReadWriteRole* que contenga todas las acciones:

```powershell
New-AzCosmosDBSqlRoleDefinition -AccountName $accountName `
    -ResourceGroupName $resourceGroupName `
    -Type CustomRole -RoleName MyReadWriteRole `
    -DataAction @( `
        'Microsoft.DocumentDB/databaseAccounts/readMetadata',
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/*', `
        'Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/*') `
    -AssignableScope "/"
```

Enumere las definiciones de roles que ha creado para capturar sus identificadores:

```powershell
Get-AzCosmosDBSqlRoleDefinition -AccountName $accountName `
    -ResourceGroupName $resourceGroupName
```

```
RoleName         : MyReadWriteRole
Id               : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAcc
                   ounts/<myCosmosAccount>/sqlRoleDefinitions/<roleDefinitionId>
Type             : CustomRole
Permissions      : {Microsoft.Azure.Management.CosmosDB.Models.Permission}
AssignableScopes : {/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAc
                   counts/<myCosmosAccount>}

RoleName         : MyReadOnlyRole
Id               : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAcc
                   ounts/<myCosmosAccount>/sqlRoleDefinitions/<roleDefinitionId>
Type             : CustomRole
Permissions      : {Microsoft.Azure.Management.CosmosDB.Models.Permission}
AssignableScopes : {/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAc
                   counts/<myCosmosAccount>}
```

### <a name="using-the-azure-cli"></a>Uso de la CLI de Azure

Cree un rol denominado *MyReadOnlyRole* que solo contenga acciones de lectura:

```json
// role-definition-ro.json
{
    "RoleName": "MyReadOnlyRole",
    "Type": "CustomRole",
    "AssignableScopes": ["/"],
    "Permissions": [{
        "DataActions": [
            "Microsoft.DocumentDB/databaseAccounts/readMetadata",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/read",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeQuery",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/readChangeFeed"
        ]
    }]
}
```

```azurecli
resourceGroupName='<myResourceGroup>'
accountName='<myCosmosAccount>'
az cosmosdb sql role definition create --account-name $accountName --resource-group $resourceGroupName --body @role-definition-ro.json
```

Cree un rol denominado *MyReadWriteRole* que contenga todas las acciones:

```json
// role-definition-rw.json
{
    "RoleName": "MyReadWriteRole",
    "Type": "CustomRole",
    "AssignableScopes": ["/"],
    "Permissions": [{
        "DataActions": [
            "Microsoft.DocumentDB/databaseAccounts/readMetadata",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/*",
            "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/*"
        ]
    }]
}
```

```azurecli
az cosmosdb sql role definition create --account-name $accountName --resource-group $resourceGroupName --body @role-definition-rw.json
```

Enumere las definiciones de roles que ha creado para capturar sus identificadores:

```azurecli
az cosmosdb sql role definition list --account-name $accountName --resource-group $resourceGroupName
```

```
[
  {
    "assignableScopes": [
      "/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAccounts/<myCosmosAccount>"
    ],
    "id": "/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAccounts/<myCosmosAccount>/sqlRoleDefinitions/<roleDefinitionId>",
    "name": "<roleDefinitionId>",
    "permissions": [
      {
        "dataActions": [
          "Microsoft.DocumentDB/databaseAccounts/readMetadata",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/*",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/*"
        ],
        "notDataActions": []
      }
    ],
    "resourceGroup": "<myResourceGroup>",
    "roleName": "MyReadWriteRole",
    "sqlRoleDefinitionGetResultsType": "CustomRole",
    "type": "Microsoft.DocumentDB/databaseAccounts/sqlRoleDefinitions"
  },
  {
    "assignableScopes": [
      "/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAccounts/<myCosmosAccount>"
    ],
    "id": "/subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDB/databaseAccounts/<myCosmosAccount>/sqlRoleDefinitions/<roleDefinitionId>",
    "name": "<roleDefinitionId>",
    "permissions": [
      {
        "dataActions": [
          "Microsoft.DocumentDB/databaseAccounts/readMetadata",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/items/read",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/executeQuery",
          "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers/readChangeFeed"
        ],
        "notDataActions": []
      }
    ],
    "resourceGroup": "<myResourceGroup>",
    "roleName": "MyReadOnlyRole",
    "sqlRoleDefinitionGetResultsType": "CustomRole",
    "type": "Microsoft.DocumentDB/databaseAccounts/sqlRoleDefinitions"
  }
]
```

### <a name="using-azure-resource-manager-templates"></a>Uso de plantillas del Administrador de recursos de Azure

Consulte [esta página](/rest/api/cosmos-db-resource-provider/2021-04-15/sqlresources2/create-update-sql-role-definition) para obtener una referencia y ejemplos del uso de plantillas de Azure Resource Manager para crear definiciones de roles.

## <a name="create-role-assignments"></a><a id="role-assignments"></a> Creación de asignaciones de roles

Puede asociar definiciones de roles integrados o personalizados con sus identidades de Azure AD. Al crear una asignación de roles, debe proporcionar:

- El nombre de la cuenta de Azure Cosmos DB.
- El grupo de recursos que contiene la cuenta.
- El identificador de la definición de rol que se va a asignar.
- El identificador de la entidad de seguridad de la identidad a la que debe asignarse la definición de roles.
- El ámbito de la asignación de roles; los ámbitos admitidos son:
    - `/` (nivel de cuenta)
    - `/dbs/<database-name>` (nivel de base de datos)
    - `/dbs/<database-name>/colls/<container-name>` (nivel de contenedor)

  El ámbito debe coincidir o ser un subámbito de uno de los ámbitos asignables de la definición de roles.

> [!NOTE]
> Si desea crear una asignación de roles para una entidad de servicio, asegúrese de usar el **identificador de objeto** que se encuentra en la sección **Aplicaciones empresariales** de la hoja del portal de **Azure Active Directory**.

> [!NOTE]
> Las operaciones que se describen a continuación están disponibles en:
> - Azure PowerShell: [Az.CosmosDB, versión 1.2.0-preview](https://www.powershellgallery.com/packages/Az.CosmosDB/1.2.0) o superior
> - [CLI de Azure](/cli/azure/install-azure-cli): versión 2.24.0 o superior.

### <a name="using-azure-powershell"></a>Uso de Azure PowerShell

Asigne un rol a una identidad:

```powershell
$resourceGroupName = "<myResourceGroup>"
$accountName = "<myCosmosAccount>"
$readOnlyRoleDefinitionId = "<roleDefinitionId>" # as fetched above
$principalId = "<aadPrincipalId>"
New-AzCosmosDBSqlRoleAssignment -AccountName $accountName `
    -ResourceGroupName $resourceGroupName `
    -RoleDefinitionId $readOnlyRoleDefinitionId `
    -Scope "/" `
    -PrincipalId $principalId
```

### <a name="using-the-azure-cli"></a>Uso de la CLI de Azure

Asigne un rol a una identidad:

```azurecli
resourceGroupName='<myResourceGroup>'
accountName='<myCosmosAccount>'
readOnlyRoleDefinitionId = '<roleDefinitionId>' # as fetched above
principalId = '<aadPrincipalId>'
az cosmosdb sql role assignment create --account-name $accountName --resource-group $resourceGroupName --scope "/" --principal-id $principalId --role-definition-id $readOnlyRoleDefinitionId
```

### <a name="using-azure-resource-manager-templates"></a>Uso de plantillas del Administrador de recursos de Azure

Consulte [esta página](/rest/api/cosmos-db-resource-provider/2021-04-15/sqlresources2/create-update-sql-role-assignment) para obtener una referencia y ejemplos del uso de plantillas de Azure Resource Manager para crear asignaciones de roles.

## <a name="initialize-the-sdk-with-azure-ad"></a>Inicialización del SDK con Azure AD

Para usar el control de acceso basado en roles de Azure Cosmos DB en la aplicación, tiene que actualizar la forma en que inicializa el SDK de Azure Cosmos DB. En lugar de pasar la clave principal de la cuenta, debe pasar una instancia de una clase `TokenCredential`. Esta instancia proporciona el SDK de Azure Cosmos DB con el contexto necesario para capturar un token de AAD en nombre de la identidad que desea usar.

La forma de crear una instancia de `TokenCredential` queda fuera del ámbito de este artículo. Hay muchas maneras de crear tal instancia en función del tipo de identidad de AAD que quiera usar (entidad de seguridad de usuario, entidad de servicio, grupo, etc.). Y lo que es más importante, la instancia de `TokenCredential` debe resolverse en la identidad (identificador de la entidad de seguridad) a la que ha asignado sus roles. Puede encontrar ejemplos de creación de una clase `TokenCredential`:

- [En .NET](/dotnet/api/overview/azure/identity-readme#credential-classes)
- [En Java](/java/api/overview/azure/identity-readme#credential-classes)
- [En JavaScript](/javascript/api/overview/azure/identity-readme#credential-classes)

En los ejemplos siguientes se usa una entidad de servicio con una instancia de `ClientSecretCredential`.

### <a name="in-net"></a>En .NET

RBAC de Azure Cosmos DB se admite actualmente en el [SDK V3 de .NET](sql-api-sdk-dotnet-standard.md).

```csharp
TokenCredential servicePrincipal = new ClientSecretCredential(
    "<azure-ad-tenant-id>",
    "<client-application-id>",
    "<client-application-secret>");
CosmosClient client = new CosmosClient("<account-endpoint>", servicePrincipal);
```

### <a name="in-java"></a>En Java

La característica RBAC de Azure Cosmos DB se admite actualmente en el [SDK de Java V4](sql-api-sdk-java-v4.md).

```java
TokenCredential ServicePrincipal = new ClientSecretCredentialBuilder()
    .authorityHost("https://login.microsoftonline.com")
    .tenantId("<azure-ad-tenant-id>")
    .clientId("<client-application-id>")
    .clientSecret("<client-application-secret>")
    .build();
CosmosAsyncClient Client = new CosmosClientBuilder()
    .endpoint("<account-endpoint>")
    .credential(ServicePrincipal)
    .build();
```

### <a name="in-javascript"></a>En JavaScript

La característica RBAC de Azure Cosmos DB se admite actualmente en el [SDK de JavaScript V3](sql-api-sdk-node.md).

```javascript
const servicePrincipal = new ClientSecretCredential(
    "<azure-ad-tenant-id>",
    "<client-application-id>",
    "<client-application-secret>");
const client = new CosmosClient({
    "<account-endpoint>",
    aadCredentials: servicePrincipal
});
```

## <a name="authenticate-requests-on-the-rest-api"></a>Autenticación de solicitudes en la API de REST

Al construir el [encabezado de autorización de API REST](/rest/api/cosmos-db/access-control-on-cosmosdb-resources), establezca el parámetro **type** en **aad** y la firma de hash **(sig)** en el **token de oauth** como se muestra en el ejemplo siguiente:

`type=aad&ver=1.0&sig=<token-from-oauth>`

## <a name="use-data-explorer"></a>Uso del Explorador de datos

> [!NOTE]
> El explorador de datos expuesto en Azure Portal todavía no es compatible con RBAC de Azure Cosmos DB. Para usar la identidad de Azure AD al explorar los datos, debe usar el [Explorador de Azure Cosmos DB](https://cosmos.azure.com/?feature.enableAadDataPlane=true) en su lugar.

Al acceder al [Explorador de Azure Cosmos DB](https://cosmos.azure.com/?feature.enableAadDataPlane=true) con el parámetro de consulta específico `?feature.enableAadDataPlane=true` e iniciar sesión, se usa la siguiente lógica para acceder a los datos:

1. Se intenta obtener la clave principal de la cuenta en nombre de la identidad que ha iniciado sesión. Si esta solicitud se realiza correctamente, se usa la clave principal para acceder a los datos de la cuenta.
1. Si no se permite que la identidad que ha iniciado sesión obtenga la clave principal de la cuenta, esta identidad se usará directamente para autenticar el acceso a los datos. En este modo, la identidad debe [asignarse con definiciones de roles adecuadas](#role-assignments) para garantizar el acceso a los datos.

## <a name="audit-data-requests"></a>Auditoría de solicitudes de datos

Al usar el control de acceso basado en roles de Azure Cosmos DB, los [registros de diagnóstico](cosmosdb-monitor-resource-logs.md) se incrementan con la información de identidad y autorización de cada operación de datos. Esto le permite realizar una auditoría detallada y recuperar la identidad de AAD que se usa para cada solicitud de datos enviada a su cuenta de Azure Cosmos DB.

Esta información adicional fluye en la categoría de registro **DataPlaneRequests** y consta de dos columnas adicionales:

- `aadPrincipalId_g` muestra el identificador de la entidad de seguridad de la identidad de AAD que se usó para autenticar la solicitud.
- `aadAppliedRoleAssignmentId_g` muestra la [asignación de roles](#role-assignments) que se ha respetado al autorizar la solicitud.

## <a name="enforcing-rbac-as-the-only-authentication-method"></a><a id="disable-local-auth"></a> Aplicación de RBAC como único método de autenticación

En situaciones en las que quiera forzar a los clientes a conectarse a Azure Cosmos DB exclusivamente por medio de RBAC, tiene la opción de deshabilitar las claves principales y secundarias de la cuenta. Al hacerlo, se rechazará de forma activa cualquier solicitud entrante que use una clave principal o secundaria, o un token de recurso.

### <a name="using-azure-resource-manager-templates"></a>Uso de plantillas del Administrador de recursos de Azure

Al crear o actualizar la cuenta de Azure Cosmos DB mediante plantillas de Azure Resource Manager, establezca la propiedad `disableLocalAuth` en `true`:

```json
"resources": [
    {
        "type": " Microsoft.DocumentDB/databaseAccounts",
        "properties": {
            "disableLocalAuth": true,
            // ...
        },
        // ...
    },
    // ...
 ]
```

## <a name="limits"></a>Límites

- Puede crear hasta 100 definiciones de roles y 2000 asignaciones de roles por cuenta de Azure Cosmos DB.
- Solo puede asignar definiciones de roles a identidades de Azure AD que pertenezcan al mismo inquilino de Azure AD que su cuenta de Azure Cosmos DB.
- La resolución de grupos de Azure AD no se admite actualmente para las identidades que pertenecen a más de 200 grupos.
- El token de Azure AD se pasa actualmente como un encabezado con cada solicitud que se envía al servicio Azure Cosmos DB, con lo que aumenta el tamaño total de la carga útil.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="which-azure-cosmos-db-apis-are-supported-by-rbac"></a>¿Qué API de Azure Cosmos DB admite el control de acceso basado en roles?

Actualmente solo se admite la API de SQL.

### <a name="is-it-possible-to-manage-role-definitions-and-role-assignments-from-the-azure-portal"></a>¿Es posible administrar las definiciones de roles y las asignaciones de roles desde Azure Portal?

Todavía no está disponible la compatibilidad de Azure Portal con la administración de roles.

### <a name="which-sdks-in-azure-cosmos-db-sql-api-support-rbac"></a>¿Qué SDK de la API de SQL para Azure Cosmos DB admite el control de acceso basado en roles?

Actualmente se admiten los SDK de [.NET V3](sql-api-sdk-dotnet-standard.md), [Java V4](sql-api-sdk-java-v4.md) y [JavaScript V3](sql-api-sdk-node.md).

### <a name="is-the-azure-ad-token-automatically-refreshed-by-the-azure-cosmos-db-sdks-when-it-expires"></a>¿Actualizan automáticamente los SDK de Azure Cosmos DB el token de Azure AD cuando este expira?

Sí.

### <a name="is-it-possible-to-disable-the-usage-of-the-account-primarysecondary-keys-when-using-rbac"></a>¿Es posible deshabilitar el uso de las claves principal o secundaria de la cuenta al usar el control de acceso basado en roles?

Sí, vea [Aplicación de RBAC como único método de autenticación](#disable-local-auth).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información general sobre el [acceso seguro a los datos en Cosmos DB](secure-access-to-data.md).
- Más información sobre la [administración del control de acceso basado en roles de Azure Cosmos DB](role-based-access-control.md).
