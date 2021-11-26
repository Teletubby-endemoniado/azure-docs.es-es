---
title: Autorice el acceso a blobs con Active Directory
titleSuffix: Azure Storage
description: Autorice el acceso a blobs de Azure con Azure Active Directory (Azure AD). Asigne roles de Azure para los derechos de acceso. Acceda a los datos con una cuenta de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/14/2021
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 9f52d0555e07a79956aed466e3f9a307946bbcf6
ms.sourcegitcommit: 0415f4d064530e0d7799fe295f1d8dc003f17202
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2021
ms.locfileid: "132724147"
---
# <a name="authorize-access-to-blobs-using-azure-active-directory"></a>Autorice el acceso a blobs con Azure Active Directory

Azure Storage admite el uso de Azure Active Directory (Azure AD) para autorizar solicitudes a datos de blobs. Con Azure AD, puede usar el control de acceso basado en rol de Azure (Azure RBAC) para conceder permisos a una entidad de seguridad, que puede ser un usuario, un grupo o una entidad de servicio de aplicación. Azure AD autentica la entidad de seguridad para devolver un token de OAuth 2.0. Después, el token se puede usar para autorizar una solicitud en Blob service.

La autorización de solicitudes en Azure Storage con Azure AD proporciona seguridad superior y facilidad de uso sobre la autorización de clave compartida. Microsoft recomienda usar la autorización de Azure AD con las aplicaciones de blobs cuando sea posible para garantizar el acceso con los privilegios mínimos necesarios.

La autorización con credenciales de Azure AD está disponible para todas las cuentas de propósito general y de Blob Storage en todas las regiones públicas y nubes nacionales. Solo las cuentas de almacenamiento creadas con el modelo de implementación de Azure Resource Manager admiten la autorización de Azure AD.

Blob Storage admite de forma adicional la creación de firmas de acceso compartido (SAS) firmadas con credenciales de Azure AD. Para más información, consulte [Concesión de acceso limitado a datos con firmas de acceso compartido](../common/storage-sas-overview.md).

## <a name="overview-of-azure-ad-for-blobs"></a>Información general sobre Azure AD para blobs

Cuando una entidad de seguridad (un usuario, un grupo o una aplicación) intenta acceder a un recurso de blob, la solicitud debe autorizarse, a menos que sea un blob disponible para acceso anónimo. Con Azure AD, el acceso a un recurso es un proceso de dos pasos. En primer lugar, se autentica la identidad de la entidad de seguridad y se devuelve un token de OAuth 2.0. Después, el token se pasa como parte de una solicitud a Blob service y el servicio lo usa para autorizar el acceso al recurso especificado.

El paso de autenticación exige que una aplicación solicite un token de acceso de OAuth 2.0 en tiempo de ejecución. Si una aplicación se ejecuta desde una entidad de Azure como una máquina virtual de Azure, un conjunto de escalado de máquinas virtuales o una aplicación de Azure Functions, puede usar una [identidad administrada](../../active-directory/managed-identities-azure-resources/overview.md) para acceder a los datos de blobs. Para más información sobre cómo autorizar solicitudes realizadas por una identidad administrada al servicio Blob de Azure, consulte [Autorización del acceso a datos de blobs con identidades administradas en recursos de Azure](authorize-managed-identity.md).

El paso de la autorización exige que se asignen uno o varios roles Rol RBAC de Azure a la entidad de seguridad que realiza la solicitud. Para más información, vea [Asignación de roles de Azure para derechos de acceso](#assign-azure-roles-for-access-rights).

Las aplicaciones nativas y las aplicaciones web que realizan solicitudes a Azure Blob service también pueden autorizar el acceso con Azure AD. Para información sobre cómo solicitar un token de acceso y usarlo para autorizar solicitudes de datos de blobs, consulte [Autorizar acceso a Azure Storage mediante Azure AD desde una aplicación de Azure Storage](../common/storage-auth-aad-app.md).

## <a name="assign-azure-roles-for-access-rights"></a>Asignación de roles de Azure para derechos de acceso

Azure Active Directory (Azure AD) concede derechos de acceso a recursos protegidos mediante Azure RBAC. Azure Storage define un conjunto de roles RBAC integrados que abarcan conjuntos comunes de permisos utilizados para acceder a los datos de blob. También puede definir roles personalizados para el acceso a datos de blobs. Para más información sobre la asignación de roles de Azure para acceder a blobs, consulte [Asignación de un rol de Azure para acceder a datos de blobs](../blobs/assign-azure-role-data-access.md).

Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación o una [identidad de servicio administrada para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md). Los roles RBAC que se asignan a una entidad de seguridad determinan los permisos que tiene esa entidad de seguridad. Para más información sobre la asignación de roles de Azure para acceder a blobs, consulte [Asignación de un rol de Azure para acceder a datos de blobs](../blobs/assign-azure-role-data-access.md).

En algunos casos, puede que necesite habilitar el acceso específico a los recursos de blob o simplificar los permisos cuando tenga un gran número de asignaciones de roles para un recurso de almacenamiento. Puede usar el control de acceso basado en atributos de Azure (Azure ABAC) para configurar las condiciones de asignación de roles. Puede usar condiciones con un [rol personalizado](../../role-based-access-control/custom-roles.md) o seleccionar roles integrados. Para más información sobre cómo configurar condiciones para los recursos de almacenamiento de Azure con ABAC, consulte [Autorización del acceso a blobs mediante condiciones de asignación de roles de Azure (versión preliminar)](../common/storage-auth-abac.md). Para más información sobre las condiciones admitidas para las operaciones de datos de blob, consulte [Acciones y atributos para las condiciones de asignación de roles de Azure en Azure Storage (versión preliminar)](../common/storage-auth-abac-attributes.md).

### <a name="resource-scope"></a>Ámbito de recursos

Antes de asignar un rol de Azure RBAC a una entidad de seguridad, determine el ámbito de acceso que debería tener la entidad de seguridad. Los procedimientos recomendados dictan que siempre es mejor conceder únicamente el ámbito más restringido posible. Los roles de Azure RBAC definidos en un ámbito más amplio los heredan los recursos que están debajo de ellos.

Puede limitar el acceso a los recursos de blobs de Azure en los niveles siguientes, empezando por el ámbito más reducido:

- **Un contenedor individual.** En este ámbito, se aplica una asignación de roles a todos los blobs del contenedor, así como las propiedades y los metadatos del contenedor.
- **La cuenta de almacenamiento.** En este ámbito, una asignación de roles se aplica a todos los contenedores y sus blobs.
- **El grupo de recursos.** En este ámbito, se aplica una asignación de roles a todos los contenedores de todas las cuentas de almacenamiento del grupo de recursos.
- **La suscripción.** En este ámbito, se aplica una asignación de roles a todos los contenedores de todas las cuentas de almacenamiento de todos los grupos de recursos de la suscripción.
- **Un grupo de administración.** En este ámbito, se aplica una asignación de roles a todos los contenedores de todas las cuentas de almacenamiento de todos los grupos de recursos de todas las suscripciones del grupo de administración.

Para obtener más información sobre el ámbito de las asignaciones de roles de RBAC de Azure, consulte [Información sobre el ámbito de RBAC de Azure](../../role-based-access-control/scope-overview.md).

### <a name="azure-built-in-roles-for-blobs"></a>Roles integrados de Azure para blobs

RBAC de Azure proporciona una serie de roles integrados para autorizar el acceso a datos de blobs con Azure AD y OAuth. Entre los roles que proporcionan permisos a los recursos de datos en Azure Storage están, por ejemplo, los siguientes:

- [Propietario de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): se usa para establecer la propiedad y administrar el control de acceso POSIX en Azure Data Lake Storage Gen2. Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](../../storage/blobs/data-lake-storage-access-control.md).
- [Colaborador de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): se usa para conceder permisos de lectura, escritura y eliminación a los recursos de almacenamiento de blobs.
- [Lector de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): se usa para conceder permisos de solo lectura a los recursos de almacenamiento de blobs.
- [Delegador de Blob Storage](../../role-based-access-control/built-in-roles.md#storage-blob-delegator): obtiene una clave de delegación de usuarios que se usa para crear una firma de acceso compartido que se firma con credenciales de Azure AD para un contenedor o un blob.

Para obtener información sobre cómo asignar un rol integrado de Azure a una entidad de seguridad, vea [Asignación de un rol de Azure para acceder a datos de blobs](../blobs/assign-azure-role-data-access.md). Para obtener información sobre cómo enumerar los roles RBAC de Azure y sus permisos, consulte [Enumeración de las definiciones de roles de Azure](../../role-based-access-control/role-definitions-list.md).

Para más información acerca de cómo se definen los roles integrados para Azure Storage, consulte [Descripción de definiciones de roles](../../role-based-access-control/role-definitions.md#control-and-data-actions). Para más información acerca de la creación de roles personalizados de Azure, consulte [Roles personalizados de Azure](../../role-based-access-control/custom-roles.md).

Solo los roles definidos explícitamente para el acceso a datos permiten a una entidad de seguridad acceder a los datos de blobs. Los roles integrados, como **Propietario**, **Colaborador** y **Colaborador de la cuenta de almacenamiento**, permiten que una entidad de seguridad administre una cuenta de almacenamiento, pero no proporcionan acceso a los datos de blobs dentro de esa cuenta a través de Azure AD. Sin embargo, si un rol incluye **Microsoft.Storage/storageAccounts/listKeys/action**, el usuario al que se haya asignado ese rol podrá acceder a los datos de la cuenta de almacenamiento mediante la autorización de clave compartida con las claves de acceso de la cuenta. Para obtener más información, vea [Elección de la forma de autorizar el acceso a los datos de blob en Azure Portal](../../storage/blobs/authorize-data-operations-portal.md).

Para obtener información detallada sobre los roles integrados de Azure para Azure Storage para los servicios de datos y el servicio de administración, consulte la sección **Almacenamiento** en [Roles integrados de Azure para RBAC de Azure](../../role-based-access-control/built-in-roles.md#storage). Además, para obtener información sobre los diferentes tipos de roles que proporcionan permisos en Azure, consulte [Roles de administrador de suscripciones clásico, roles de Azure y roles de Azure AD](../../role-based-access-control/rbac-and-directory-admin-roles.md).

> [!IMPORTANT]
> Las asignaciones de roles de Azure pueden tardar hasta 30 minutos en propagarse.

### <a name="access-permissions-for-data-operations"></a>Permisos de acceso para operaciones de datos

Para obtener información sobre los permisos necesarios para llamar a operaciones concretas de Blob service, consulte [Permisos para llamar a operaciones de datos](/rest/api/storageservices/authorize-with-azure-active-directory#permissions-for-calling-data-operations).

## <a name="access-data-with-an-azure-ad-account"></a>Acceso a datos con una cuenta de Azure AD

El acceso a los datos de blobs a través de Azure Portal, PowerShell o la CLI de Azure se puede autorizar mediante la cuenta de Azure AD del usuario o las claves de acceso de cuenta (autorización de clave compartida).

### <a name="data-access-from-the-azure-portal"></a>Acceso a datos desde Azure Portal

Azure Portal puede usar la cuenta de Azure AD o las claves de acceso de cuenta para acceder a datos de blobs en una cuenta de Azure Storage. El esquema de autorización que use Azure Portal depende de los roles de Azure que tenga asignados.

Si intenta acceder a datos de blobs, Azure Portal primero comprueba si tiene asignado un rol de Azure con **Microsoft.Storage/storageAccounts/listkeys/action**. Si tiene un rol asignado con esta acción, Azure Portal usa la clave de cuenta para acceder a los datos de blobs mediante la autorización de clave compartida. Si no tiene un rol asignado con esta acción, Azure Portal intenta acceder a los datos mediante la cuenta de Azure AD.

Para acceder a datos de blobs desde Azure Portal con la cuenta de Azure AD, necesita permisos para acceder a datos de blobs y, también, permisos para examinar los recursos de la cuenta de almacenamiento en Azure Portal. Los roles integrados que proporciona Azure Storage conceden acceso a recursos de blobs, pero no conceden permisos a los recursos de la cuenta de almacenamiento. Por este motivo, el acceso al portal también requiere la asignación de un rol de Azure Resource Manager, como el rol [Lector](../../role-based-access-control/built-in-roles.md#reader), con ámbito limitado al nivel de la cuenta de almacenamiento o superior. El rol **Lector** concede los permisos más restringidos, pero otro rol de Azure Resource Manager que conceda acceso a los recursos de administración de la cuenta de almacenamiento también es aceptable. Para obtener más información sobre cómo asignar permisos a los usuarios para el acceso a los datos de Azure Portal con una cuenta de Azure AD, consulte [Asignar un rol de Azure para el acceso a datos de blobs](../blobs/assign-azure-role-data-access.md).

Azure Portal indica qué esquema de autorización se está usando al examinar un contenedor. Para obtener más información sobre acceso a los datos en el portal, consulte [Elección de la forma de autorizar el acceso a los datos de blobs en Azure Portal](../blobs/authorize-data-operations-portal.md).

### <a name="data-access-from-powershell-or-azure-cli"></a>Acceso a datos desde PowerShell o la CLI de Azure

PowerShell y la CLI de Azure admiten el inicio de sesión con credenciales de Azure AD. Después de iniciar sesión, la sesión se ejecuta con esas credenciales. Para obtener más información, vea uno de los siguientes artículos:

- [Distintas formas de autorizar el acceso a datos en blobs con la CLI de Azure](authorize-data-operations-cli.md)
- [Ejecución de comandos de PowerShell con credenciales de Azure AD para acceder a los datos de blob](authorize-data-operations-powershell.md)

## <a name="feature-support"></a>Compatibilidad de características

En esta tabla se muestra cómo se admite esta característica en la cuenta y el impacto en la compatibilidad al habilitar determinadas funcionalidades.

| Tipo de cuenta de almacenamiento | Blob Storage (compatibilidad predeterminada) | Data Lake Storage Gen2 <sup>1</sup> | NFS 3.0 <sup>1</sup> | SFTP <sup>1</sup> |
|--|--|--|--|--|
| De uso general estándar, v2 | ![Sí](../media/icons/yes-icon.png) |![Sí](../media/icons/yes-icon.png)              | ![No](../media/icons/no-icon.png) | ![No](../media/icons/no-icon.png) |
| Blobs en bloques Premium          | ![Sí](../media/icons/yes-icon.png) | ![Sí](../media/icons/yes-icon.png)| ![No](../media/icons/no-icon.png) | ![No](../media/icons/no-icon.png) |

<sup>1</sup> La compatibilidad con Data Lake Storage Gen2, Network File System (NFS) 3.0 y el protocolo de transferencia de archivos segura (SFTP) requiere una cuenta de almacenamiento con un espacio de nombres jerárquico habilitado.

## <a name="next-steps"></a>Pasos siguientes

- [Autorización del acceso a datos en Azure Storage](../common/authorize-data-access.md)
- [Asignación de un rol de Azure el acceso a datos de blob](assign-azure-role-data-access.md).
