---
title: Creación y administración de credenciales para exámenes
description: Conozca los pasos necesarios para crear y administrar credenciales en Azure Purview.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-map
ms.topic: how-to
ms.date: 05/08/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: fdce380d09cc2992f4e77f9385b1d176a6ae68eb
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131076668"
---
# <a name="credentials-for-source-authentication-in-azure-purview"></a>Credenciales para la autenticación de origen en Azure Purview

En este artículo se describe cómo se pueden crear credenciales en Azure Purview. Estas credenciales guardadas le permiten volver a usar y aplicar rápidamente la información de autenticación guardada a los exámenes del origen de datos.

## <a name="prerequisites"></a>Requisitos previos

- Un almacén de claves de Azure. Para aprender a crear una, consulte el [Inicio rápido: Creación de un almacén de claves mediante Azure Portal](../key-vault/general/quick-create-portal.md).

## <a name="introduction"></a>Introducción

Una credencial es información de autenticación que Azure Purview puede usar para autenticarse en los orígenes de datos registrados. Se puede crear un objeto de credencial para varios tipos de escenarios de autenticación, como la autenticación básica que requiere el nombre de usuario y la contraseña. La credencial captura información específica necesaria para autenticarse, en función del tipo de método de autenticación elegido. Las credenciales usan los secretos existentes de Azure Key Vault para recuperar información confidencial de autenticación durante el proceso de creación de la credencial.

En Azure Purview, hay algunas opciones que se pueden usar como método de autenticación para examinar orígenes de datos, como las siguientes opciones:

- Identidad administrada de Azure Purview
- Clave de cuenta (mediante Key Vault)
- Autenticación de SQL (mediante Key Vault)
- Entidad de servicio (mediante Key Vault)

Antes de crear credenciales, tenga en cuenta los tipos de origen de datos y los requisitos de red para decidir qué método de autenticación es necesario para su escenario. Revise el siguiente árbol de decisión para encontrar qué credencial es la más adecuada:

   :::image type="content" source="media/manage-credentials/manage-credentials-decision-tree-small.png" alt-text="Árbol de decisión de administración de credenciales" lightbox="media/manage-credentials/manage-credentials-decision-tree.png":::

## <a name="use-purview-managed-identity-to-set-up-scans"></a>Uso de una identidad administrada de Purview para configurar exámenes

Si usa la identidad administrada de Purview para configurar exámenes, no tendrá que crear explícitamente una credencial y vincular el almacén de claves a Purview para almacenarlos. Para encontrar instrucciones detalladas sobre cómo agregar la identidad administrada de Purview para acceder a los orígenes de datos y examinarlos, consulte a continuación las secciones de autenticación específicas de los orígenes de datos:

- [Azure Blob Storage](register-scan-azure-blob-storage-source.md#authentication-for-a-scan)
- [Azure Data Lake Storage Gen1](register-scan-adls-gen1.md#authentication-for-a-scan)
- [Azure Data Lake Storage Gen2](register-scan-adls-gen2.md#authentication-for-a-scan)
- [Azure SQL Database](register-scan-azure-sql-database.md)
- [Instancia administrada de Azure SQL Database](register-scan-azure-sql-database-managed-instance.md#authentication-for-registration)
- [Azure Synapse Analytics](register-scan-azure-synapse-analytics.md#authentication-for-registration)

## <a name="create-azure-key-vaults-connections-in-your-azure-purview-account"></a>Creación de conexiones de Azure Key Vault en la cuenta de Azure Purview

Para crear una credencial, primero debe asociar una o varias de las instancias de Azure Key Vault existentes a su cuenta de Azure Purview.

1. En [Azure Portal](https://portal.azure.com), seleccione su cuenta de Azure Purview y abra [Purview Studio](https://web.purview.azure.com/resource/). Vaya a **Management Center** (Centro de administración) en el estudio y, luego, a **Credentiales** (Credenciales).

2. En la página **Credentials** (Credenciales), seleccione **Manage Key Vault connections** (Administrar conexiones de Key Vault).

   :::image type="content" source="media/manage-credentials/manage-kv-connections.png" alt-text="Administración de conexiones de Azure Key Vault":::

3. Seleccione **+ New** (+ Nuevo) en la página Manage Key Vault connections (Administrar conexiones de Key Vault).

4. Rellene la información necesaria y seleccione **Create** (Crear).

5. Confirme que el almacén de claves se ha asociado correctamente a la cuenta de Azure Purview, como se muestra en este ejemplo:

   :::image type="content" source="media/manage-credentials/view-kv-connections.png" alt-text="Vista de conexiones de Azure Key Vault para confirmar.":::

## <a name="grant-the-purview-managed-identity-access-to-your-azure-key-vault"></a>Concesión de acceso a la identidad administrada de Purview a la instancia de Azure Key Vault

Actualmente Azure Key Vault admite dos modelos de permisos:

- Opción 1: Directivas de acceso 
- Opción 2: Control de acceso basado en roles 

Antes de asignar acceso a la identidad administrada de Purview, identifique el modelo de permisos de Azure Key Vault en las **directivas de acceso** del recurso de Key Vault en el menú. Siga los pasos que se indican a continuación en función del modelo de permisos pertinente.  

:::image type="content" source="media/manage-credentials/akv-permission-model.png" alt-text="Modelo de permisos de Azure Key Vault"::: 

### <a name="option-1---assign-access-using-key-vault-access-policy"></a>Opción 1: asignación de acceso mediante la directiva de acceso de Key Vault  

Siga estos pasos solo si el modelo de permisos del recurso de Azure Key Vault está establecido en **Directiva de acceso de Vault**:

1. Vaya a Azure Key Vault.

2. Seleccione la página **Directivas de acceso**.

3. Seleccione **Agregar directiva de acceso**.

   :::image type="content" source="media/manage-credentials/add-msi-to-akv-2.png" alt-text="Incorporación de Purview MSI a AKV.":::

4. En el menú desplegable **Secrets permissions** (Permisos de secretos), seleccione los permisos **Obtener** y **Enumerar**.

5. En **Seleccionar la entidad de seguridad**, elija la identidad administrada de Purview. Puede buscar el MSI de Purview con el nombre de la instancia de Purview **o** el identificador de aplicación de la identidad administrada. Actualmente no se admiten identidades compuestas (nombre de identidad administrada + identificador de la aplicación).

   :::image type="content" source="media/manage-credentials/add-access-policy.png" alt-text="Agregar directiva de acceso":::

6. Seleccione **Agregar**.

7. Seleccione **Guardar** para guardar la directiva de acceso.

   :::image type="content" source="media/manage-credentials/save-access-policy.png" alt-text="Guardado de la directiva de acceso.":::

### <a name="option-2---assign-access-using-key-vault-azure-role-based-access-control"></a>Opción 2: Asignación de acceso mediante control de acceso basado en roles de Azure Key Vault 

Siga estos pasos solo si el modelo de permisos del recurso de Azure Key Vault está establecido en **Control de acceso basado en roles de Azure**:

1. Vaya a Azure Key Vault.

2. Seleccione **Access Control (IAM)** (Control de acceso [IAM]) en el menú de navegación izquierdo.

3. Seleccione **+Agregar**.

4. Establezca **Role** (Rol) en **Key Vault Secrets User** (Usuario de secretos de Key Vault) y escriba el nombre de la cuenta de Azure Purview en el cuadro de entrada **Select** (Seleccionar). A continuación, seleccione Save (Guardar) para dar esta asignación de rol a su cuenta de Purview.

   :::image type="content" source="media/manage-credentials/akv-add-rbac.png" alt-text="Azure Key Vault RBAC":::


## <a name="create-a-new-credential"></a>Creación de una credencial

Estos tipos de credenciales se admiten en Purview:

- Autenticación básica: se agrega la **contraseña** como un secreto en el almacén de claves.
- Entidad de servicio: se agrega la **clave de entidad de servicio** como un secreto en el almacén de claves.
- Autenticación de SQL: se agrega la **contraseña** como un secreto en el almacén de claves.
- Clave de cuenta: se agrega la **clave de cuenta** como un secreto en el almacén de claves.
- ARN de rol: en los orígenes de datos de Amazon S3, agregue su **ARN de rol** en AWS. 

Para más información, consulte [Incorporación de un secreto a Key Vault](../key-vault/secrets/quick-create-portal.md#add-a-secret-to-key-vault) y [Creación de un rol de AWS para Purview](register-scan-amazon-s3.md#create-a-new-aws-role-for-purview).

Después de almacenar los secretos en el almacén de claves:

1. En Azure Purview, vaya a la página Credentials (Credenciales).

2. Cree la credencial seleccionando **+ New** (+ Nuevo).

3. Proporcione la información necesaria. Seleccione el **método de autenticación** y una **conexión a Key Vault** desde la que seleccionar un secreto.

4. Una vez rellenados todos los detalles, seleccione **Create** (Crear).

   :::image type="content" source="media/manage-credentials/new-credential.png" alt-text="Nueva credencial.":::

5. Compruebe que la nueva credencial aparece en la vista de lista y que está preparada para usarse.

   :::image type="content" source="media/manage-credentials/view-credentials.png" alt-text="Visualización de la credencial.":::

## <a name="manage-your-key-vault-connections"></a>Administración de las conexiones del almacén de claves

1. Busque las conexiones del almacén de claves por el nombre.

   :::image type="content" source="media/manage-credentials/search-kv.png" alt-text="Búsqueda del almacén de claves":::

2. Elimine una o más conexiones de almacén de claves.

   :::image type="content" source="media/manage-credentials/delete-kv.png" alt-text="Eliminar el almacén de claves":::

## <a name="manage-your-credentials"></a>Administración de las credenciales

1. Busque las credenciales por el nombre.
  
2. Seleccione y actualice una credencial existente.

3. Elimine una o varias credenciales.

## <a name="next-steps"></a>Pasos siguientes

[Creación de un conjunto de reglas de examen](create-a-scan-rule-set.md)
