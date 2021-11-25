---
title: 'Roles integrados de Azure: Azure RBAC'
description: En este artículo se describen los roles integrados de Azure para el control de acceso basado en roles (Azure RBAC) de Azure. Se enumeran Actions, NotActions, DataActions y NotDataActions.
services: active-directory
ms.service: role-based-access-control
ms.topic: reference
ms.workload: identity
author: rolyon
ms.author: rolyon
ms.date: 11/12/2021
ms.custom: generated
ms.openlocfilehash: 81b12ad1ae52c290e5c3ef573bf091fb64595701
ms.sourcegitcommit: 362359c2a00a6827353395416aae9db492005613
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2021
ms.locfileid: "132494417"
---
# <a name="azure-built-in-roles"></a>Roles integrados de Azure

El [control de acceso basado en rol de Azure (Azure RBAC)](overview.md) tiene varios roles integrados de Azure que se pueden asignar a usuarios, grupos, entidades de servicio e identidades administradas. Las asignaciones de roles sirven para controlar el acceso a los recursos de Azure. Si los roles integrados no satisfacen las necesidades específicas de la organización, puede crear [roles personalizados de Azure](custom-roles.md) propios. Para información sobre cómo asignar roles, consulte [Pasos para asignar un rol de Azure.](role-assignments-steps.md)

En este artículo se enumeran los roles integrados de Azure. Si busca los roles Administrador de Azure Active Directory (Azure AD), consulte [Roles integrados de Azure AD](../active-directory/roles/permissions-reference.md).

En la tabla siguiente se proporciona una breve descripción de cada rol integrado. Haga clic en el nombre del rol para ver la lista de `Actions`, `NotActions`, `DataActions` y `NotDataActions` para cada rol. Para obtener información sobre lo que significan estas acciones y cómo se aplican a los planos de control y de datos, consulte [Descripción de las definiciones de roles de Azure](role-definitions.md).

## <a name="all"></a>All

> [!div class="mx-tableFixed"]
> | Rol integrado | Descripción | ID |
> | --- | --- | --- |
> | **General** |  |  |
> | [Colaborador](#contributor) | Concede acceso completo para administrar todos los recursos, pero no le permite asignar roles en Azure RBAC, administrar asignaciones en Azure Blueprints ni compartir galerías de imágenes. | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | [Propietario](#owner) | Permite conceder acceso total para administrar todos los recursos, incluida la posibilidad de asignar roles en Azure RBAC. | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | [Lector](#reader) | Permite ver todos los recursos, pero no realizar ningún cambio. | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | [Administrador de acceso de usuario](#user-access-administrator) | Permite administrar el acceso de usuario a los recursos de Azure. | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Proceso** |  |  |
> | [Colaborador de la máquina virtual clásica](#classic-virtual-machine-contributor) | Permite administrar máquinas virtuales clásicas, pero no acceder a ellas, ni tampoco a la red virtual ni la cuenta de almacenamiento a las que están conectadas. | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | [Inicio de sesión de administrador de Virtual Machine](#virtual-machine-administrator-login) | Visualización de máquinas virtuales en el portal e inicio de sesión como administrador | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | [Colaborador de la máquina virtual](#virtual-machine-contributor) | Cree y administre máquinas virtuales, administre discos e instantáneas de disco, instale y ejecute software, restablezca la contraseña del usuario raíz de la máquina virtual mediante extensiones de VM, y administre cuentas de usuario locales mediante extensiones de VM. Este rol no le concede acceso de administración a la red virtual ni a la cuenta de almacenamiento a la que están conectadas las máquinas virtuales. Este rol no le permite asignar roles en Azure RBAC. | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | [Inicio de sesión de usuario de Virtual Machine](#virtual-machine-user-login) | Visualización de máquinas virtuales en el portal e inicio de sesión como usuario normal. | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Redes** |  |  |
> | [Colaborador de punto de conexión de CDN](#cdn-endpoint-contributor) | Puede administrar puntos de conexión de CDN, pero no conceder acceso a otros usuarios. | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | [Lector de punto de conexión de CDN](#cdn-endpoint-reader) | Puede ver puntos de conexión de CDN, pero no hacer cambios. | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | [Colaborador de perfil de CDN](#cdn-profile-contributor) | Puede administrar perfiles de CDN y sus puntos de conexión, pero no conceder acceso a otros usuarios. | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | [Lector de perfil de CDN](#cdn-profile-reader) | Puede ver perfiles de CDN y sus puntos de conexión, pero no hacer cambios. | 8f96442b-4075-438f-813d-ad51ab4019af |
> | [Colaborador de la red clásica](#classic-network-contributor) | Permite administrar las redes clásicas, pero no acceder a ellas. | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | [Colaborador de zona DNS](#dns-zone-contributor) | Permite administrar zonas y conjuntos de registros DNS en Azure DNS, pero no controlar los usuarios que tienen acceso. | befefa01-2a29-4197-83a8-272ff33ce314 |
> | [Colaborador de la red](#network-contributor) | Permite administrar redes, pero no acceder a ellas. | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | [Colaborador de zona DNS privada](#private-dns-zone-contributor) | Permite administrar recursos de zonas DNS privadas, pero no las redes virtuales a las que están vinculados. | b12aa53e-6015-4669-85d0-8515ebb3ae7f |
> | [Colaborador de Traffic Manager](#traffic-manager-contributor) | Le permite administrar perfiles de Traffic Manager, pero no controlar los usuarios que tienen acceso a ellos. | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Storage** |  |  |
> | [Colaborador de Avere](#avere-contributor) | Puede crear y administrar un clúster de Avere vFXT. | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | [Operador de Avere](#avere-operator) | Lo usa el clúster de Avere vFXT para su administración. | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | [Colaborador de copias de seguridad](#backup-contributor) | Permite administrar el servicio de copias de seguridad, pero no puede crear almacenes ni conceder acceso a otros usuarios | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | [Operador de copias de seguridad](#backup-operator) | Permite administrar los servicios de copias de seguridad, excepto la eliminación de copias de seguridad, la creación de almacenes y la concesión de acceso a otros usuarios | 00c29273-979b-4161-815c-10b084fb9324 |
> | [Lector de copias de seguridad](#backup-reader) | Puede ver servicios de copia de seguridad, pero no puede realizar cambios. | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | [Colaborador de cuentas de almacenamiento clásico](#classic-storage-account-contributor) | Permite administrar cuentas de almacenamiento clásicas, pero no acceder a ellas. | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | [Rol de servicio de operador de claves de cuentas de almacenamiento clásicas](#classic-storage-account-key-operator-service-role) | Los operadores de claves de cuentas de almacenamiento clásicas pueden enumerar y regenerar claves en cuentas de almacenamiento clásicas | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | [Colaborador de Data Box](#data-box-contributor) | Permite administrarlo todo en el servicio Data Box, excepto dar acceso a otros usuarios. | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | [Lector de Data Box](#data-box-reader) | Permite administrar el servicio Data Box excepto la creación o edición de detalles de pedido y dar acceso a otros usuarios. | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | [Desarrollador de Data Lake Analytics](#data-lake-analytics-developer) | Le permite enviar, supervisar y administrar sus propios trabajos, pero no crear ni eliminar cuentas de Data Lake Analytics. | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | [Lector y acceso a los datos](#reader-and-data-access) | Permite ver todo el contenido, pero no eliminar ni crear una cuenta de almacenamiento ni un recurso incluido. También permitirá el acceso de lectura o escritura para todos los datos incluidos en una cuenta de almacenamiento a través del acceso a las claves de la cuenta de almacenamiento. | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | [Colaborador de la cuenta de almacenamiento](#storage-account-contributor) | Permite la administración de cuentas de almacenamiento. Proporciona acceso a la clave de cuenta, que puede usarse para tener acceso a datos a través de la autorización de clave compartida. | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | [Rol de servicio de operador de claves de cuentas de almacenamiento](#storage-account-key-operator-service-role) | Permite enumerar y regenerar claves de acceso de la cuenta de almacenamiento. | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | [Colaborador de datos de blobs de almacenamiento](#storage-blob-data-contributor) | Lee, escribe y elimina blobs y contenedores de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | [Propietario de datos de blobs de almacenamiento](#storage-blob-data-owner) | Proporciona acceso total a los contenedores de blobs y los datos de Azure Storage, incluida la asignación de control de acceso POSIX. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | [Lector de datos de blobs de almacenamiento](#storage-blob-data-reader) | Lee y enumera blobs y contenedores de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | [Delegador de Blob Storage](#storage-blob-delegator) | Obtiene una clave de delegación de usuarios, que se puede usar a continuación para crear una firma de acceso compartido para un contenedor o un blob firmado con credenciales de Azure AD. Para más información, vea [Creación de SAS de delegación de usuarios](/rest/api/storageservices/create-user-delegation-sas). | db58b8e5-c6ad-4a2a-8342-4190687cbf4a |
> | [Colaborador de recursos compartidos de SMB de datos de archivos de Storage](#storage-file-data-smb-share-contributor) | Permite el acceso de lectura, escritura y eliminación a los archivos y directorios de los recursos compartidos de Azure. Este rol no tiene ningún equivalente integrado en los servidores de archivos de Windows. | 0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb |
> | [Colaborador elevado de recursos compartidos de SMB de datos de archivos de Storage](#storage-file-data-smb-share-elevated-contributor) | Permite el acceso de lectura, escritura, eliminación y modificación de ACL en los archivos y directorios de los recursos compartidos de Azure. Este rol es equivalente a una ACL de recurso compartido de cambio en los servidores de archivos de Windows. | a7264617-510b-434b-a828-9731dc254ea7 |
> | [Lector de recursos compartidos de SMB de datos de archivos de Storage](#storage-file-data-smb-share-reader) | Permite el acceso de lectura a los archivos y directorios de los recursos compartidos de Azure. Este rol es equivalente a una ACL de recurso compartido de lectura en los servidores de archivos de Windows. | aba4ae5f-2193-4029-9191-0cb91df5e314 |
> | [Colaborador de datos de la cola de Storage](#storage-queue-data-contributor) | Lee, escribe y elimina los mensajes de la cola y a la cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | [Procesador de mensajes de datos de la cola de Storage](#storage-queue-data-message-processor) | Consulta, recupera y elimina un mensaje de una cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | 8a0f0c08-91a1-4084-bc3d-661d67233fed |
> | [Emisor de mensajes de datos de la cola de Storage](#storage-queue-data-message-sender) | Agrega mensaje a una cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | [Lector de datos de la cola de Storage](#storage-queue-data-reader) | Lee y enumera los mensajes de la cola y las colas de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). | 19e7f393-937e-4f77-808e-94535e297925 |
> | [Colaborador de datos de tabla de Storage](#storage-table-data-contributor) | Permite el acceso de lectura, escritura y eliminación a tablas y entidades de Azure Storage | 0a9a7e1f-b9d0-4cc4-a60d-0319b160aaa3 |
> | [Lector de datos de tabla de Storage](#storage-table-data-reader) | Permite el acceso de lectura a tablas y entidades de Azure Storage | 76199698-9eea-4c19-bc75-cec21354c6b6 |
> | **Web** |  |  |
> | [Colaborador de datos de Azure Maps](#azure-maps-data-contributor) | Conde acceso para leer, escribir y eliminar datos relacionados con mapas desde una cuenta de mapas de Azure. | 8f5e0ce6-4f7b-4dcf-bddf-e6f48634a204 |
> | [Azure Maps Data Reader](#azure-maps-data-reader) | Concede acceso de lectura a los datos de los mapas de una cuenta de Azure Maps. | 423170ca-a8f6-4b0f-8487-9e4eb8f49bfa |
> | [Colaborador en Config Server de Azure Spring Cloud](#azure-spring-cloud-config-server-contributor) | Permite el acceso de lectura, escritura y eliminación en Config Server en Azure Spring Cloud. | a06f5c24-21a7-4e1a-aa2b-f19eb6684f5b |
> | [Lectura en Config Server de Azure Spring Cloud](#azure-spring-cloud-config-server-reader) | Permite el acceso de lectura a Config Server en Azure Spring Cloud. | d04c6db6-4947-4782-9e91-30a88feb7be7 |
> | [Lector de datos de Azure Spring Cloud](#azure-spring-cloud-data-reader) | Permite el acceso de lectura a los datos de Azure Spring Cloud. | b5537268-8956-4941-a8f0-646150406f0c |
> | [Colaborador en Service Registry de Azure Spring Cloud](#azure-spring-cloud-service-registry-contributor) | Permite el acceso de lectura, escritura y eliminación en el registro de servicios de Azure Spring Cloud. | f5880b48-c26d-48be-b172-7927bfa1c8f1 |
> | [Lectura en Service Registry de Azure Spring Cloud](#azure-spring-cloud-service-registry-reader) | Permite el acceso de lectura en el registro de servicios de Azure Spring Cloud. | cff1b556-2399-4e7e-856d-a8f754be7b65 |
> | [Administrador de cuenta de Media Services](#media-services-account-administrator) | Crear, leer, modificar y eliminar cuentas de Media Services; acceso de solo lectura a otros recursos de Media Services. | 054126f8-9a2b-4f1c-a9ad-eca461f08466 |
> | [Administrador de eventos en directo de Media Services](#media-services-live-events-administrator) | Crear, leer, modificar y eliminar eventos en directo, recursos, filtros de recursos y localizadores de streaming; acceso de solo lectura a otros recursos de Media Services. | 532bc159-b25e-42c0-969e-a1d439f60d77 |
> | [Operador multimedia de Media Services](#media-services-media-operator) | Crear, leer, modificar y eliminar recursos, filtros de recursos, localizadores de streaming y trabajos; acceso de solo lectura a otros recursos de Media Services. | e4395492-1534-4db2-bedf-88c14621589c |
> | [Administrador de directivas de Media Services](#media-services-policy-administrator) | Crear, leer, modificar y eliminar filtros de cuenta, directivas de streaming, directivas de clave de contenido y transformaciones; acceso de solo lectura a otros recursos de Media Services. No puede crear trabajos, recursos o recursos de streaming. | c4bba371-dacd-4a26-b320-7250bca963ae |
> | [Administrador de puntos de conexión de streaming de Media Services](#media-services-streaming-endpoints-administrator) | Crear, leer, modificar y eliminar puntos de conexión de streaming; acceso de solo lectura a otros recursos de Media Services. | 99dba123-b5fe-44d5-874c-ced7199a5804 |
> | [Colaborador de datos de índice de búsqueda](#search-index-data-contributor) | Concede acceso total a los datos de índice de Azure Cognitive Search. | 8ebe5a00-799e-43f5-93ac-243d3dce84a7 |
> | [Lector de datos de índice de búsqueda](#search-index-data-reader) | Concede acceso de lectura a los datos de índice de Azure Cognitive Search. | 1407120a-92aa-4202-b7e9-c0e197c71c8f |
> | [Colaborador del servicio Search](#search-service-contributor) | Permite administrar los servicios de Búsqueda, pero no acceder a ellos. | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | [Lector AccessKey de SignalR](#signalr-accesskey-reader) | Lee las claves de acceso de SignalR Service. | 04165923-9d83-45d5-8227-78b77b0a687e |
> | [Servidor de aplicaciones de SignalR (versión preliminar)](#signalr-app-server-preview) | Permite que el servidor de aplicaciones acceda al servicio SignalR con opciones de autenticación de AAD. | 420fcaa2-552c-430f-98ca-3264be4806c7 |
> | [Propietario de la API REST de SignalR](#signalr-rest-api-owner) | Acceso completo a las API REST de Azure SignalR Service. | fd53cd77-2268-407a-8f46-7e7863d0f521 |
> | [Lector de la API REST de SignalR](#signalr-rest-api-reader) | Acceso de solo lectura a las API REST de Azure SignalR Service. | ddde6b66-c0df-4114-a159-3618637b3035 |
> | [Propietario de SignalR Service](#signalr-service-owner) | Acceso completo a las API REST de Azure SignalR Service. | 7e4f1700-ea5a-4f59-8f37-079cfe29dce3 |
> | [Colaborador de SignalR y Web PubSub](#signalrweb-pubsub-contributor) | Crea, lee, actualiza y elimina recursos del servicio SignalR. | 8cf5e20a-e4b2-4e9d-b3a1-5ceb692c2761 |
> | [Colaborador de plan web](#web-plan-contributor) | Administra los planes web para los sitios web. No le permite asignar roles en RBAC de Azure. | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | [Colaborador de sitio web](#website-contributor) | Administra sitios web, pero no planes web. No le permite asignar roles en RBAC de Azure. | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Contenedores** |  |  |
> | [AcrDelete](#acrdelete) | Permite eliminar repositorios, etiquetas o manifiestos de un registro de contenedor. | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | [AcrImageSigner](#acrimagesigner) | Permite insertar imágenes de confianza en un registro de contenedor habilitado para la confianza en el contenido, así como extraerlas de dicho registro. | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | [AcrPull](#acrpull) | Permite extraer artefactos de un registro de contenedor. | 7f951dda-4ed3-4680-a7ca-43fe172d538d |
> | [AcrPush](#acrpush) | Permite insertar artefactos en un registro de contenedor, así como extraerlos. | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | [AcrQuarantineReader](#acrquarantinereader) | Permite extraer imágenes en cuarentena de un registro de contenedor. | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | [AcrQuarantineWriter](#acrquarantinewriter) | Permite insertar imágenes en cuarentena en un registro de contenedor, así como extraerlas. | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | [Rol de administrador de clúster de Azure Kubernetes Service](#azure-kubernetes-service-cluster-admin-role) | Enumerar la acción de credenciales administrativas del clúster. | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | [Rol de usuario de clúster de Azure Kubernetes Service](#azure-kubernetes-service-cluster-user-role) | Enumerar la acción de credenciales de usuario del clúster. | 4abbcc35-e782-43D8-92c5-2d3f1bd2253f |
> | [Rol de colaborador de Azure Kubernetes Service](#azure-kubernetes-service-contributor-role) | Concede acceso de lectura y escritura a los clústeres de Azure Kubernetes Service | ed7f3fbd-7b88-4dd4-9017-9adb7ce333f8 |
> | [Administrador de Azure Kubernetes Service RBAC](#azure-kubernetes-service-rbac-admin) | Permite administrar todos los recursos en un clúster o espacio de nombres, excepto actualizar o eliminar cuotas de recursos y espacios de nombres. | 3498e952-d568-435e-9b2c-8d77e338d7f7 |
> | [Administrador de clúster de Azure Kubernetes Service RBAC](#azure-kubernetes-service-rbac-cluster-admin) | Permite administrar todos los recursos del clúster. | b1ff04bb-8a4e-4dc4-8eb5-8693973ce19b |
> | [Lector de Azure Kubernetes Service RBAC](#azure-kubernetes-service-rbac-reader) | Permite el acceso de solo lectura para ver la mayoría de los objetos en un espacio de nombres. No permite la visualización de roles o enlaces de roles. Este rol no permite visualización de secretos, ya que leer el contenido de estos permite el acceso a las credenciales de ServiceAccount en el espacio de nombres, que permitiría el acceso a la API como cualquier ServiceAccount en el espacio de nombres (una forma de elevación de privilegios). Al aplicar este rol en el ámbito del clúster, se proporcionará acceso a todos los espacios de nombres. | 7f6c6a51-bcf8-42ba-9220-52d62157d7db |
> | [Escritor de Azure Kubernetes Service RBAC](#azure-kubernetes-service-rbac-writer) | Permite el acceso de lectura y escritura a la mayoría de los objetos de un espacio de nombres. Este rol no permite ver ni modificar roles ni enlaces de roles. Sin embargo, este rol permite acceder a secretos y ejecutar pods como cualquier ServiceAccount en el espacio de nombres, por lo que se puede usar para obtener los niveles de acceso de la API de cualquier ServiceAccount en el espacio de nombres. Al aplicar este rol en el ámbito del clúster, se proporcionará acceso a todos los espacios de nombres. | a7ffa36f-339b-4b5c-8bdf-e2c188b2c0eb |
> | **Bases de datos** |  |  |
> | [Azure Connected SQL Server Onboarding](#azure-connected-sql-server-onboarding) | Permite el acceso de lectura y escritura a los recursos de Azure para SQL Server en servidores habilitados para Arc. | e8113dce-c529-4d33-91fa-e9b972617508 |
> | [Rol de lector de cuentas de Cosmos DB](#cosmos-db-account-reader-role) | Puede leer los datos de cuentas de Azure Cosmos DB. Vea [Colaborador de cuenta de DocumentDB](#documentdb-account-contributor) para administrar cuentas de Azure Cosmos DB. | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | [Operador de Cosmos DB](#cosmos-db-operator) | Permite administrar las cuentas de Azure Cosmos DB, pero no acceder a los datos que contienen. Evita el acceso a las claves de cuenta y a las cadenas de conexión. | 230815da-be43-4aae-9cb4-875f7bd000aa |
> | [CosmosBackupOperator](#cosmosbackupoperator) | Puede enviar una solicitud de restauración para una base de datos de Cosmos DB o un contenedor de una cuenta | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | [CosmosRestoreOperator](#cosmosrestoreoperator) | Puede realizar una acción de restauración en la cuenta de la base de datos de Cosmos DB con el modo de copia de seguridad continua | 5432c526-bc82-444a-b7ba-57c5b0b5b34f |
> | [Colaborador de cuenta de DocumentDB](#documentdb-account-contributor) | Puede administrar cuentas de Azure Cosmos DB. Azure Cosmos DB se llamaba anteriormente DocumentDB. | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | [Colaborador de la memoria caché de Redis](#redis-cache-contributor) | Permite administrar cachés de Redis, pero no acceder a ellas. | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | [Colaborador de Base de datos de SQL](#sql-db-contributor) | Permite administrar las bases de datos de SQL, pero no acceder a ellas. Además, no puede administrar sus directivas relacionadas con la seguridad ni los servidores SQL primarios. | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | [Colaborador de Instancia administrada de SQL](#sql-managed-instance-contributor) | Permite administrar Instancias administradas de SQL y la configuración de red necesaria, pero no puede conceder acceso a otros usuarios. | 4939a1f6-9ae0-4e48-a1e0-f2cbe897382d |
> | [Administrador de seguridad SQL](#sql-security-manager) | Permite administrar las directivas relacionadas con seguridad de bases de datos y servidores SQL, pero no acceder a ellas. | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | [Colaborador de SQL Server](#sql-server-contributor) | Permite administrar bases de datos y servidores SQL, pero no acceder a ellos, ni a sus directivas relacionadas con la seguridad. | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Analytics** |  |  |
> | [Propietario de los datos de Azure Event Hubs](#azure-event-hubs-data-owner) | Concede acceso total a los recursos de Azure Event Hubs. | f526a384-b230-433a-b45c-95f59c4a2dec |
> | [Receptor de datos de Azure Event Hubs](#azure-event-hubs-data-receiver) | Concede acceso de recepción a los recursos de Azure Event Hubs. | a638d3c7-ab3a-418d-83e6-5f17a39d4fde |
> | [Emisor de datos de Azure Event Hubs](#azure-event-hubs-data-sender) | Concede acceso de emisión a los recursos de Azure Event Hubs. | 2b629674-e913-4c01-ae53-ef4638d8f975 |
> | [Colaborador de Factoría de datos](#data-factory-contributor) | Crea y administra factorías de datos, así como recursos secundarios dentro de ellas. | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | [Purgador de datos](#data-purger) | Permite eliminar datos de un área de trabajo de Log Analytics. | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | [Operador de clústeres de HDInsight](#hdinsight-cluster-operator) | Permite leer y modificar las configuraciones de clúster de HDInsight. | 61ed4efc-Fab3-44fd-b111-e24485cc132a |
> | [Colaborador de Domain Services para HDInsight](#hdinsight-domain-services-contributor) | Puede leer, crear, modificar y eliminar operaciones relacionadas con Domain Services para HDInsight Enterprise Security Package | 8d8d5a11-05d3-4bda-a417-a08778121c7c |
> | [Colaborador de Log Analytics](#log-analytics-contributor) | Un colaborador de Log Analytics puede leer todos los datos de supervisión y editar la configuración de supervisión. La edición de la configuración de supervisión incluye la posibilidad de agregar la extensión de máquina virtual a las máquinas virtuales, leer las claves de las cuentas de almacenamiento para poder configurar la recopilación de registros de Azure Storage, agregar soluciones y configurar Azure Diagnostics en todos los recursos de Azure. | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | [Lector de Log Analytics](#log-analytics-reader) | Un lector de Log Analytics puede ver y buscar todos los datos de supervisión, así como consultar la configuración de supervisión, incluida la de Azure Diagnostics en todos los recursos de Azure. | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | [Conservador de datos de Purview (heredado)](#purview-data-curator-legacy) | El conservador de datos de Microsoft.Purview es un rol heredado puede crear, leer, modificar y eliminar objetos de datos del catálogo y establecer relaciones entre objetos. Este rol ha entrado en desuso recientemente en el acceso basado en roles de Azure y se ha introducido un nuevo conservador de datos en el plano de datos de Azure Purview. Consulte [Control de acceso en Azure Purview: roles](../purview/catalog-permissions.md#roles). | 8a3c2885-9b38-4fd2-9d99-91af537c1347 |
> | [Lector de datos de Purview (heredado)](#purview-data-reader-legacy) | El lector de datos de Microsoft.Purview es un rol heredado que puede leer objetos de datos del catálogo. Este rol ha entrado en desuso recientemente en el acceso basado en roles de Azure y se ha introducido un nuevo lector de datos en el plano de datos de Azure Purview. Consulte [Control de acceso en Azure Purview: roles](../purview/catalog-permissions.md#roles). | ff100721-1b9d-43d8-af52-42b69c1272db |
> | [Administrador de orígenes de datos de Purview (heredado)](#purview-data-source-administrator-legacy) | El administrador de orígenes de datos de Microsoft.Purview es un rol heredado que puede administrar orígenes de datos y exámenes de datos. Este rol ha entrado en desuso recientemente en el acceso basado en roles de Azure y se ha introducido un nuevo administrador de orígenes de datos en el plano de datos de Azure Purview. Consulte [Control de acceso en Azure Purview: roles](../purview/catalog-permissions.md#roles). | 200bba9e-f0c8-430f-892b-6f0794863803 |
> | [Colaborador del registro de esquemas (versión preliminar)](#schema-registry-contributor-preview) | Leer, escribir y eliminar esquemas y grupos del registro de esquemas. | 5dffeca3-4936-4216-b2bc-10343a5abb25 |
> | [Lector del registro de esquemas (versión preliminar)](#schema-registry-reader-preview) | Leer y enumerar grupos y esquemas del registro de esquemas. | 2c56ea50-c6b3-40a6-83c0-9d98858bc7d2 |
> | **Blockchain** |  |  |
> | [Acceso al nodo de miembro de la cadena de bloques (versión preliminar)](#blockchain-member-node-access-preview) | Permite acceder a los nodos de miembro de la cadena de bloques. | 31a002a1-acaf-453e-8a5b-297c9ca1ea24 |
> | **Inteligencia artificial y aprendizaje automático** |  |  |
> | [Científico de datos de AzureML](#azureml-data-scientist) | Puede realizar todas las acciones dentro de un área de trabajo de Azure Machine Learning, excepto crear o eliminar recursos de proceso, y modificar la propia área de trabajo. | f6c7c914-8db3-469d-8ca1-694a8f32e121 |
> | [Colaborador de Cognitive Services](#cognitive-services-contributor) | Le permite crear, leer, actualizar, eliminar y administrar las claves de Cognitive Services. | 25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68 |
> | [Colaborador de Custom Vision de Cognitive Services](#cognitive-services-custom-vision-contributor) | Acceso completo al proyecto, lo que incluye la capacidad para ver, crear, editar o eliminar proyectos. | c1ff6cc2-c111-46fe-8896-e0ef812ad9f3 |
> | [Implementación de Custom Vision de Cognitive Services](#cognitive-services-custom-vision-deployment) | Publicar, anular publicaciones o exportar modelos. La implementación puede ver el proyecto pero no puede actualizarlo. | 5c4089e1-6d96-4d2f-b296-c1bc7137275f |
> | [Etiquetador de Custom Vision de Cognitive Services](#cognitive-services-custom-vision-labeler) | Ver y editar imágenes de entrenamiento, además de crear, agregar, quitar o eliminar etiquetas de imágenes. Los etiquetadores pueden ver el proyecto, pero no pueden actualizar nada más que las imágenes y etiquetas de entrenamiento. | 88424f51-ebe7-446f-bc41-7fa16989e96c |
> | [Lector de Custom Vision de Cognitive Services](#cognitive-services-custom-vision-reader) | Acciones de solo lectura en el proyecto. Los lectores no pueden crear ni actualizar el proyecto. | 93586559-c37d-4a6b-ba08-b9f0940c2d73 |
> | [Entrenador de Custom Vision de Cognitive Services](#cognitive-services-custom-vision-trainer) | Ver, editar proyectos y entrenar los modelos, lo que incluye la capacidad de publicar, anular la publicación y exportar los modelos. Los entrenadores no pueden crear ni eliminar el proyecto. | 0a5ae4ab-0d65-4eeb-be61-29fc9b54394b |
> | [Lector de datos de Cognitive Services (versión preliminar)](#cognitive-services-data-reader-preview) | Permite leer los datos de Cognitive Services. | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | [Cognitive Services Face Recognizer](#cognitive-services-face-recognizer) | Permite detectar, comprobar, identificar, agrupar y buscar operaciones similares en Face API. Este rol no permite operaciones de creación o eliminación, de modo que es adecuado para los puntos de conexión que solo necesitan funcionalidades de inferencia, según los procedimientos recomendados de "privilegios mínimos". | 9894cab4-e18a-44aa-828b-cb588cd6f2d7 |
> | [Administrador de Metrics Advisor de Cognitive Services](#cognitive-services-metrics-advisor-administrator) | Acceso total al proyecto, lo que incluye la configuración del nivel de sistema. | cb43c632-a144-4ec5-977c-e80c4affc34a |
> | [Editor de QnA Maker de Cognitive Services](#cognitive-services-qna-maker-editor) | Permite crear, editar, importar y exportar un knowledge base. No se puede publicar ni eliminar un knowledge base. | f4cc2bf9-21be-47a1-bdf1-5c5804381025 |
> | [Lector de QnA Maker de Cognitive Services](#cognitive-services-qna-maker-reader) | Permite leer y probar solo un knowledge base. | 466ccd10-b268-4a11-b098-b4849f024126 |
> | [Usuario de Cognitive Services](#cognitive-services-user) | Le permite leer y mostrar las claves de Cognitive Services. | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **Internet de las cosas** |  |  |
> | [Device Update Administrator (Administrador de Device Update)](#device-update-administrator) | Proporciona acceso total a operaciones de contenido y administración. | 02ca0879-e8e4-47a5-a61e-5c618b76e64a |
> | [Device Update Content Administrator (Administrador de contenido de Device Update)](#device-update-content-administrator) | Proporciona acceso total a operaciones de contenido. | 0378884a-3af5-44ab-8323-f5b22f9f3c98 |
> | [Device Update Content Reader (Lector de contenido de Device Update)](#device-update-content-reader) | Proporciona acceso de lectura a operaciones de contenido, pero no permite realizar cambios. | d1ee9a80-8b14-47f0-bdc2-f4a351625a7b |
> | [Device Update Deployments Administrator (Administrador de implementaciones de Device Update)](#device-update-deployments-administrator) | Proporciona acceso total a operaciones de administración. | e4237640-0e3d-4a46-8fda-70bc94856432 |
> | [Device Update Deployments Reader (Lector de implementaciones de Device Update)](#device-update-deployments-reader) | Proporciona acceso de lectura a operaciones de administración, pero no permite realizar cambios. | 49e2f5d2-7741-4835-8efa-19e1fe35e47f |
> | [Device Update Reader (Lector de Device Update)](#device-update-reader) | Proporciona acceso de lectura a operaciones de contenido y administración, pero no permite realizar cambios. | e9dba6fb-3d52-4cf0-bce3-f06ce71b9e0f |
> | [Colaborador de datos de IoT Hub](#iot-hub-data-contributor) | Permite el acceso total a las operaciones del plano de datos de IoT Hub. | 4fc6c259-987e-4a07-842e-c321cc9d413f |
> | [Lector de datos de IoT Hub](#iot-hub-data-reader) | Permite el acceso de lectura total a las propiedades del plano de datos de IoT Hub. | b447c946-2db7-41ec-983d-d8bf3b1c77e3 |
> | [Colaborador del registro de IoT Hub](#iot-hub-registry-contributor) | Permite el acceso completo al registro de dispositivos de IoT Hub. | 4ea46cd5-c1b2-4a8e-910b-273211f9ce47 |
> | [Colaborador de gemelos de IoT Hub](#iot-hub-twin-contributor) | Permite el acceso de lectura y escritura a todos los dispositivos y módulos gemelos de IoT Hub. | 494bdba2-168f-4f31-a0a1-191d2f7c028c |
> | **Realidad mixta** |  |  |
> | [Administrador de Remote Rendering](#remote-rendering-administrator) | Proporciona al usuario funcionalidades de conversión, administración de sesiones, representación y diagnóstico para Azure Remote Rendering. | 3df8b902-2a6f-47c7-8cc5-360e9b272a7e |
> | [Cliente de Remote Rendering](#remote-rendering-client) | Proporciona al usuario funcionalidades de administración de sesiones, representación y diagnóstico para Azure Remote Rendering. | d39065c4-c120-43c9-ab0a-63eed9795f0a |
> | [Colaborador de la cuenta de Spatial Anchors](#spatial-anchors-account-contributor) | Permite administrar los anclajes espaciales en su cuenta, pero no eliminarlos. | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | [Propietario de la cuenta de Spatial Anchors](#spatial-anchors-account-owner) | Permite administrar los anclajes espaciales en su cuenta y eliminarlos. | 70bbe301-9835-447d-afdd-19eb3167307c |
> | [Lector de la cuenta de Spatial Anchors](#spatial-anchors-account-reader) | Permite encontrar y leer propiedades de los anclajes espaciales en la cuenta. | 5d51204f-eb77-4b1c-b86a-2ec626c49413 |
> | **Integración** |  |  |
> | [Colaborador de servicio de administración de API](#api-management-service-contributor) | Puede administrar servicios y las API. | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | [Rol del operador del servicio API Management](#api-management-service-operator-role) | Puede administrar el servicio, pero no las API. | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | [Rol de lector del servicio API Management](#api-management-service-reader-role) | Acceso de solo lectura al servicio y las API. | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | [Propietario de los datos de App Configuration](#app-configuration-data-owner) | Permite el acceso completo a los datos de App Configuration. | 5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b |
> | [Lector de los datos de App Configuration](#app-configuration-data-reader) | Permite el acceso de lectura a los datos de App Configuration. | 516239f1-63e1-4d78-a4de-a74fb236a071 |
> | [Cliente de escucha de Azure Relay](#azure-relay-listener) | Permite el acceso de escucha a recursos de Azure Relay. | 26e0b698-aa6d-4085-9386-aadae190014d |
> | [Propietario de Azure Relay](#azure-relay-owner) | Permite el acceso completo a los recursos de Azure Relay. | 2787bf04-f1f5-4bfe-8383-c8a24483ee38 |
> | [Emisor de Azure Relay](#azure-relay-sender) | Permite el acceso de envío a los recursos de Azure Relay. | 26baccc8-eea7-41f1-98f4-1762cc7f685d |
> | [Propietario de los datos de Azure Service Bus](#azure-service-bus-data-owner) | Concede acceso total a los recursos de Azure Service Bus. | 090c5cfd-751d-490a-894a-3ce6f1109419 |
> | [Receptor de datos de Azure Service Bus](#azure-service-bus-data-receiver) | Concede acceso de recepción a los recursos de Azure Service Bus. | 4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0 |
> | [Emisor de datos de Azure Service Bus](#azure-service-bus-data-sender) | Concede acceso de emisión a los recursos de Azure Service Bus. | 69a216fc-b8fb-44d8-bc22-1f3c2cd27a39 |
> | [Propietario del registro de Azure Stack](#azure-stack-registration-owner) | Permite administrar los registros de Azure Stack. | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | [Colaborador de Event Grid](#eventgrid-contributor) | Permite administrar las operaciones de Event Grid. | 1e241071-0855-49ea-94dc-649edcd759de |
> | [Emisor de datos de EventGrid](#eventgrid-data-sender) | Permite el acceso de envío a eventos de Event Grid. | d5a91429-5739-47e2-a06b-3470a27159e7 |
> | [Colaborador de EventGrid EventSubscription](#eventgrid-eventsubscription-contributor) | Permite administrar las operaciones de suscripción de eventos de EventGrid. | 428e0ff0-5e57-4d9c-a221-2c70d0e0a443 |
> | [Lector de EventGrid EventSubscription](#eventgrid-eventsubscription-reader) | Permite leer las suscripciones de eventos de EventGrid. | 2414bbcf-6497-4faf-8c65-045460748405 |
> | [Colaborador de datos de FHIR](#fhir-data-contributor) | El rol permite el acceso completo del usuario o la entidad de seguridad a los datos de FHIR. | 5a1fc7df-4bf1-4951-a576-89034ee01acd |
> | [Exportador de datos de FHIR](#fhir-data-exporter) | El rol permite al usuario o a la entidad de seguridad leer y exportar datos de FHIR. | 3db33094-8700-4567-8da5-1501d4e7e843 |
> | [Lector de datos de FHIR](#fhir-data-reader) | El rol permite al usuario o a la entidad de seguridad leer datos de FHIR. | 4c8d0bbc-75d3-4935-991f-5f3c56d81508 |
> | [Escritor de datos de FHIR](#fhir-data-writer) | El rol permite al usuario o a la entidad de seguridad leer y escribir datos de FHIR. | 3f88fce4-5892-4214-ae73-ba5294559913 |
> | [Colaborador del Entorno del servicio de integración](#integration-service-environment-contributor) | Permite administrar entornos de servicio de integración, pero no acceder a ellos. | a41e2c5b-bd99-4a07-88f4-9bf657a760b8 |
> | [Desarrollador del Entorno del servicio de integración](#integration-service-environment-developer) | Permite a los desarrolladores crear y actualizar flujos de trabajo, cuentas de integración y conexiones API en entornos de servicios de integración. | c7aa55d3-1abb-444a-a5ca-5e51e485d6ec |
> | [Colaborador de la cuenta de Sistemas inteligentes](#intelligent-systems-account-contributor) | Permite administrar las cuentas de Intelligent Systems, pero no acceder a ellas. | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | [Colaborador de aplicación lógica](#logic-app-contributor) | Le permite administrar aplicaciones lógicas, pero no cambiar el acceso a ellas. | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | [Operador de aplicación lógica](#logic-app-operator) | Le permite leer, habilitar y deshabilitar aplicaciones lógicas, pero no permite editarlas ni actualizarlas. | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Identidad** |  |  |
> | [Colaborador de identidad administrada](#managed-identity-contributor) | Le permite crear, leer, actualizar y eliminar identidades asignadas por el usuario. | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | [Operador de identidad administrada](#managed-identity-operator) | Le permite leer y asignar identidades asignadas por el usuario. | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Seguridad** |  |  |
> | [Colaborador de atestación](#attestation-contributor) | Puede leer, escribir o eliminar la instancia del proveedor de atestación | bbf86eb8-f7b4-4cce-96e4-18cddf81d86e |
> | [Lector de atestación](#attestation-reader) | No se pueden leer las propiedades del proveedor de atestación | fd1bd22b-8476-40bc-a0bc-69b95687b9f3 |
> | [Administrador de Key Vault](#key-vault-administrator) | Permite realizar todas las operaciones de plano de datos en un almacén de claves y en todos los objetos que contiene, incluidos los certificados, las claves y los secretos. No permite administrar los recursos del almacén de claves ni administrar las asignaciones de roles. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". | 00482a5a-887f-4fb3-b363-3b7fe8e74483 |
> | [Agente de certificados de Key Vault](#key-vault-certificates-officer) | Permite realizar cualquier acción en los certificados de un almacén de claves, excepto administrar permisos. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". | a4417e6f-fecd-4de8-b567-7b0420556985 |
> | [Colaborador de almacén de claves](#key-vault-contributor) | Permite administrar almacenes de claves, per no asignar roles en Azure RBAC ni acceder a secretos, claves o certificados. | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | [Agente criptográfico de Key Vault](#key-vault-crypto-officer) | Permite realizar cualquier acción en las claves de un almacén de claves, excepto administrar permisos. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". | 14b46e9e-c2b7-41b4-b07b-48a6ebf60603 |
> | [Usuario de cifrado de servicio criptográfico de Key Vault](#key-vault-crypto-service-encryption-user) | Permite leer los metadatos de las claves y realizar operaciones de encapsulado/desencapsulado. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". | e147488a-f6f5-4113-8e2d-b22465e65bf6 |
> | [Usuario criptográfico de Key Vault](#key-vault-crypto-user) | Permite realizar operaciones criptográficas mediante claves. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". | 12338af0-0e69-4776-bea7-57ae8d297424 |
> | [Lector de Key Vault](#key-vault-reader) | Permite leer metadatos de almacenes de claves y sus certificados, claves y secretos. No se pueden leer valores confidenciales, como el contenido de los secretos o el material de las claves. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". | 21090545-7ca7-4776-b22c-e363652d74d2 |
> | [Agente de secretos de Key Vault](#key-vault-secrets-officer) | Permite realizar cualquier acción en los secretos de un almacén de claves, excepto administrar permisos. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". | b86a8fe4-44ce-4948-aee5-eccb2c155cd7 |
> | [Usuario de secretos de Key Vault](#key-vault-secrets-user) | Permite leer el contenido de los secretos. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". | 4633458b-17de-408a-b874-0445c86b69e6 |
> | [Colaborador de HSM administrado](#managed-hsm-contributor) | Permite administrar grupos de HSM administrados, pero no accede a ellas. | 18500a29-7fe2-46b2-a342-b16a415e101d |
> | [Colaborador de automatización de Microsoft Sentinel](#microsoft-sentinel-automation-contributor) | Colaborador de automatización de Microsoft Sentinel | f4c81013-99ee-4d62-a7ee-b3f1f648599a |
> | [Colaborador de Microsoft Sentinel](#microsoft-sentinel-contributor) | Colaborador de Microsoft Sentinel | ab8e14d6-4a74-4a29-9ba8-549422addade |
> | [Lector de Microsoft Sentinel](#microsoft-sentinel-reader) | Lector de Microsoft Sentinel | 8d289c81-5878-46d4-8554-54e1e3d8b5cb |
> | [Respondedor de Microsoft Sentinel](#microsoft-sentinel-responder) | Respondedor de Microsoft Sentinel | 3e150937-b8fe-4cfb-8069-0eaf05ecd056 |
> | [Administrador de seguridad](#security-admin) | Vea y actualice los permisos para Security Center. Tiene los mismos permisos que el rol de lector de seguridad, y también puede actualizar la directiva de seguridad y descartar las alertas y las recomendaciones. | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | [Colaborador de evaluación de la seguridad](#security-assessment-contributor) | Permite insertar evaluaciones en Security Center. | 612c2aa1-cb24-443b-ac28-3ab7272de6f5 |
> | [Administrador de seguridad (heredado)](#security-manager-legacy) | Se trata de un rol heredado. En su lugar, use el Administrador de seguridad. | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | [Lector de seguridad](#security-reader) | Vea los permisos para Security Center. Puede ver las recomendaciones, las alertas, una directiva de seguridad y los estados de seguridad, pero no puede realizar cambios. | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **DevOps** |  |  |
> | [Usuario de DevTest Labs](#devtest-labs-user) | Permite conectarse a sus máquinas virtuales, así como iniciarlas, reiniciarlas y apagarlas, en su instancia de Azure DevTest Labs. | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | [Creador de laboratorio](#lab-creator) | Permite crear nuevos laboratorios en las cuentas de Azure Lab. | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Supervisión** |  |  |
> | [Colaborador de componentes de Application Insights](#application-insights-component-contributor) | Puede administrar los componentes de Application Insights | ae349356-3a1b-4a5e-921d-050484c6347e |
> | [Depurador de instantáneas de Application Insights](#application-insights-snapshot-debugger) | Concede permiso al usuario para ver y descargar las instantáneas de depuración que se recopilan con Snapshot Debugger de Application Insights. Tenga en cuenta que estos permisos no se incluyen en los roles [Propietario](#owner) ni [Colaborador](#contributor). Si concede el rol Depurador de instantáneas de Application Insights a los usuarios, debe concederlo directamente al usuario. El rol no se reconoce cuando se agrega a un rol personalizado. | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | [Colaborador de supervisión](#monitoring-contributor) | Puede leer todos los datos de supervisión y editar la configuración de supervisión. Consulte también [Introducción a roles, permisos y seguridad con Azure Monitor](../azure-monitor/roles-permissions-security.md#built-in-monitoring-roles). | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | [Supervisión del publicador de métricas](#monitoring-metrics-publisher) | Permite publicar las métricas de los recursos de Azure. | 3913510d-42f4-4e42-8a64-420c390055eb |
> | [Lector de supervisión](#monitoring-reader) | Puede leer todos los datos de supervisión (métricas, registros, etc.). Consulte también [Introducción a roles, permisos y seguridad con Azure Monitor](../azure-monitor/roles-permissions-security.md#built-in-monitoring-roles). | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | [Colaborador de libros](#workbook-contributor) | Puede guardar los libros compartidos. | e8ddcd69-c73f-4f9f-9844-4100522f16ad |
> | [Lector de libros](#workbook-reader) | Puede leer libros. | b279062a-9be3-42a0-92ae-8b3cf002ec4d |
> | **Administración y gobernanza** |  |  |
> | [Colaborador de Automation](#automation-contributor) | Administra los recursos de Azure Automation y otros recursos mediante Azure Automation. | f353d9bd-d4a6-484e-a77a-8050b599b867 |
> | [Operador de trabajos de Automation](#automation-job-operator) | Permite crear y administrar trabajos con los runbooks de Automation. | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | [Operador de Automation](#automation-operator) | Los operadores de automatización pueden iniciar, detener, suspender y reanudar trabajos. | d3881f73-407a-4167-8283-e981cbba0404 |
> | [Operador de runbooks de Automation](#automation-runbook-operator) | Permite leer las propiedades de runbook para poder crear trabajos del runbook. | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | [Rol de usuario del clúster de Kubernetes habilitado para Azure Arc](#azure-arc-enabled-kubernetes-cluster-user-role) | Permite enumerar las acciones de credenciales de usuario de clúster. | 00493d72-78f6-4148-b6c5-d3ce8e4799dd |
> | [Administrador de Azure Arc Kubernetes](#azure-arc-kubernetes-admin) | Permite administrar todos los recursos en un clúster o espacio de nombres, excepto actualizar o eliminar cuotas de recursos y espacios de nombres. | dffb1e0c-446f-4dde-a09f-99eb5cc68b96 |
> | [Administrador de clústeres de Azure Arc Kubernetes](#azure-arc-kubernetes-cluster-admin) | Permite administrar todos los recursos del clúster. | 8393591c-06b9-48a2-a542-1bd6b377f6a2 |
> | [Visor de Azure Arc Kubernetes](#azure-arc-kubernetes-viewer) | Permite ver todos los recursos del clúster o espacio de nombres, excepto los secretos. | 63f0a09d-1495-4db4-a681-037d84835eb4 |
> | [Escritor de Azure Arc Kubernetes](#azure-arc-kubernetes-writer) | Le permite actualizar todo el contenido del clúster o el espacio de nombres, excepto los roles (del clúster) y los enlaces de roles (del clúster). | 5b999177-9696-4545-85c7-50de3797e5a1 |
> | [Incorporación de Azure Connected Machine](#azure-connected-machine-onboarding) | Puede incorporar máquinas conectadas a Azure. | b64e21ea-ac4e-4cdf-9dc9-5b892992bee7 |
> | [Administrador de recursos de Azure Connected Machine](#azure-connected-machine-resource-administrator) | Puede leer, escribir, eliminar y volver a incorporar máquinas conectadas a Azure. | cd570a14-e51a-42ad-bac8-bafd67325302 |
> | [Lector de facturación](#billing-reader) | Permite acceso de lectura a los datos de facturación. | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | [Colaborador de plano técnico](#blueprint-contributor) | Puede administrar las definiciones del plano técnico, pero no asignarlas. | 41077137-e803-4205-871c-5a86e6a753b4 |
> | [Operador del plano técnico](#blueprint-operator) | Puede asignar los planos técnicos publicados existentes, pero no puede crear nuevos. Tenga en cuenta que esto solo funciona si la asignación se realiza con una identidad administrada asignada por el usuario. | 437d2ced-4a38-4302-8479-ed2bcb43d090 |
> | [Colaborador de Cost Management](#cost-management-contributor) | Puede ver los costos y administrar la configuración de estos (por ejemplo, presupuestos, exportaciones) | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | [Lector de Cost Management](#cost-management-reader) | Puede ver los datos de costo y la configuración (por ejemplo, presupuestos, exportaciones) | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | [Administrador de configuración de jerarquía](#hierarchy-settings-administrator) | Permite a los usuarios editar y eliminar la configuración de jerarquía. | 350f8d15-c687-4448-8ae1-157740a3936d |
> | [Clúster de Kubernetes: incorporación de Azure Arc](#kubernetes-cluster---azure-arc-onboarding) | Definición de roles para permitir crear el recurso connectedClusters a cualquier usuario o servicio | 34e09817-6cbe-4d01-b1a2-e0eac5743d41 |
> | [Kubernetes Extension Contributor](#kubernetes-extension-contributor) | Puede crear, actualizar, obtener, enumerar y eliminar extensiones de Kubernetes y obtener operaciones asincrónicas de extensión. | 85cb6faf-e071-4c9b-8136-154b5a04f717 |
> | [Rol Colaborador de la aplicación administrada](#managed-application-contributor-role) | Permite crear recursos de aplicaciones administradas. | 641177b8-a67a-45b9-a033-47bc880bb21e |
> | [Rol de operador de aplicación administrada](#managed-application-operator-role) | Permite leer y realizar acciones en los recursos de aplicación administrada. | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | [Lector de aplicaciones administradas](#managed-applications-reader) | Le permite leer los recursos de una aplicación administrada y solicitar acceso JIT. | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | [Rol para eliminar la asignación de registros de servicios administrados](#managed-services-registration-assignment-delete-role) | El rol para eliminar la asignación de registros de servicios administrados permite que los usuarios que administran el inquilino eliminen la asignación de registros asignada a su inquilino. | 91c1777a-f3dc-4fae-b103-61d183457e46 |
> | [Colaborador de grupo de administración](#management-group-contributor) | Rol de colaborador de grupo de administración | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | [Lector de grupo de administración](#management-group-reader) | Rol de lector de grupo de administración | ac63b705-f282-497d-ac71-919bf39d939d |
> | [Colaborador de la cuenta de NewRelic APM](#new-relic-apm-account-contributor) | Le permite administrar las aplicaciones y cuentas de Application Performance Management de New Relic, pero no acceder a ellas. | 5d28c62d-5b37-4476-8438-e587778df237 |
> | [Escritor de datos de Policy Insights (versión preliminar)](#policy-insights-data-writer-preview) | Permite el acceso de lectura a las directivas de los recursos y el acceso de escritura a los eventos de directiva de los componentes de los recursos. | 66bb4e9e-b016-4a94-8249-4c0511c2be84 |
> | [Operador de solicitud de cuota](#quota-request-operator) | Leer y crear solicitudes de cuota, obtener el estado de la solicitud de cuota y crear incidencias de soporte técnico. | 0e5f05e5-9ab9-446b-b98d-1e2157c94125 |
> | [Comprador de reservas](#reservation-purchaser) | Permite comprar reservas | f7b75c60-3036-4b75-91c3-6b41c27c1689 |
> | [Colaborador de directivas de recursos](#resource-policy-contributor) | Los usuarios con derechos para crear o modificar la directiva de recursos pueden crear solicitudes de soporte técnico y leer los recursos o la jerarquía. | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | [Colaborador de Site Recovery](#site-recovery-contributor) | Permite administrar el servicio Site Recovery, excepto la creación de almacenes y la asignación de roles. | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | [Operador de Site Recovery](#site-recovery-operator) | Permite realizar una conmutación por error o una conmutación por recuperación, pero no otras operaciones de administración de Site Recovery. | 494ae006-db33-4328-bf46-533a6560a3ca |
> | [Lector de Site Recovery](#site-recovery-reader) | Permite visualizar el estado de Site Recovery, pero no realizar otras operaciones de administración. | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | [Colaborador de la solicitud de soporte técnico](#support-request-contributor) | Permite crear y administrar solicitudes de soporte técnico. | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | [Colaborador de etiquetas](#tag-contributor) | Permite administrar etiquetas en las entidades sin proporcionar acceso a las entidades mismas. | 4a9ae827-6dc8-4573-8ac7-8239d42aa03f |
> | **Otros** |  |  |
> | [Propietario de datos de Azure Digital Twins](#azure-digital-twins-data-owner) | Rol de acceso completo para plano de datos de Digital Twins | bcd981a7-7f74-457b-83e1-cceb9e632ffe |
> | [Lector de datos de Azure Digital Twins](#azure-digital-twins-data-reader) | Rol de solo lectura para las propiedades del plano de datos de Digital Twins | d57506d4-4c8d-48b1-8587-93c323f6a5a3 |
> | [Colaborador de BizTalk](#biztalk-contributor) | Permite administrar los servicios de BizTalk, pero no acceder a ellos. | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | [Colaborador del grupo de aplicaciones de Desktop Virtualization](#desktop-virtualization-application-group-contributor) | Colaborador del grupo de aplicaciones de Desktop Virtualization. | 86240b0e-9422-4c43-887b-b61143f32ba8 |
> | [Lector del grupo de aplicaciones de Desktop Virtualization](#desktop-virtualization-application-group-reader) | Lector del grupo de aplicaciones de Desktop Virtualization. | aebf23d0-b568-4e86-b8f9-fe83a2c6ab55 |
> | [Colaborador de virtualización del escritorio](#desktop-virtualization-contributor) | Colaborador de Desktop Virtualization. | 082f0a83-3be5-4ba1-904c-961cca79b387 |
> | [Colaborador del grupo de hosts de Desktop Virtualization](#desktop-virtualization-host-pool-contributor) | Colaborador del grupo de hosts de Desktop Virtualization. | e307426c-f9b6-4e81-87de-d99efb3c32bc |
> | [Lector del grupo de hosts de Desktop Virtualization](#desktop-virtualization-host-pool-reader) | Lector del grupo de hosts de Desktop Virtualization. | ceadfde2-b300-400a-ab7b-6143895aa822 |
> | [Lector de virtualización del escritorio](#desktop-virtualization-reader) | Lector de Desktop Virtualization. | 49a72310-ab8d-41df-bbb0-79b649203868 |
> | [Operador de host de sesión de Desktop Virtualization](#desktop-virtualization-session-host-operator) | Operador del host de sesión de Desktop Virtualization. | 2ad6aaab-ead9-4eaa-8ac5-da422f562408 |
> | [Usuario de Desktop Virtualization](#desktop-virtualization-user) | Permite al usuario emplear las aplicaciones de un grupo de aplicaciones. | 1d18fff3-a72a-46b5-b4a9-0b38a3cd7e63 |
> | [Operador de sesión de usuario de Desktop Virtualization](#desktop-virtualization-user-session-operator) | Operador de sesión de usuario de Desktop Virtualization. | ea4bfff8-7fb4-485a-aadd-d4129a0ffaa6 |
> | [Colaborador del área de trabajo de Desktop Virtualization](#desktop-virtualization-workspace-contributor) | Colaborador del área de trabajo de Desktop Virtualization. | 21efdde3-836f-432b-bf3d-3e8e734d4b2b |
> | [Lector del área de trabajo de Desktop Virtualization](#desktop-virtualization-workspace-reader) | Lector del área de trabajo de Desktop Virtualization. | 0fa44ee9-7a7d-466b-9bb2-2bf446b1204d |
> | [Lector de copias de seguridad de disco](#disk-backup-reader) | Proporciona permiso para realizar copias de seguridad del almacén para realizar copias de seguridad de disco. | 3e5e47e6-65f7-47ef-90b5-e5dd4d455f24 |
> | [Operador de grupo de discos](#disk-pool-operator) | Proporcione permiso al proveedor de recursos StoragePool para administrar los discos agregados a un grupo de discos. | 60fc6e62-5479-42d4-8bf4-67625fcc2840 |
> | [Operador de restauración de disco](#disk-restore-operator) | Proporciona permiso para realizar copias de seguridad del almacén para realizar restauraciones de disco. | b50d9833-a0cb-478e-945f-707fcc997c13 |
> | [Colaborador de instantáneas de discos](#disk-snapshot-contributor) | Proporciona permiso para realizar copias de seguridad del almacén para administrar instantáneas de disco. | 7efff54f-a5b4-42b5-a1c5-5411624893ce |
> | [Colaborador de colecciones de trabajos de Scheduler](#scheduler-job-collections-contributor) | Permite administrar colecciones de trabajos de Scheduler, pero no acceder a ellas. | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | [Services Hub Operator](#services-hub-operator) | Services Hub Operator permite realizar todas las operaciones de lectura, escritura y eliminación relacionadas con los conectores de Services Hub. | 82200a5b-e217-47a5-b665-6d8765ee745b |


## <a name="general"></a>General


### <a name="contributor"></a>Colaborador

Concede acceso completo para administrar todos los recursos, pero no le permite asignar roles en Azure RBAC, administrar asignaciones en Azure Blueprints ni compartir galerías de imágenes. [Más información](rbac-and-directory-admin-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | * | Crear y administrar recursos de todos los tipos |
> | **NotActions** |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/Delete | Eliminar roles, asignaciones de directivas, definiciones de directiva y definiciones del conjunto de directivas |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/Write | Crear roles, asignaciones de roles, asignaciones de directivas, definiciones de directiva y definiciones del conjunto de directivas |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/elevateAccess/action | Concede al llamador acceso de administrador de acceso de usuario en el ámbito de inquilinos |
> | [Microsoft.Blueprint](resource-provider-operations.md#microsoftblueprint)/blueprintAssignments/write | Crear o actualizar cualquier asignación de planos técnicos |
> | [Microsoft.Blueprint](resource-provider-operations.md#microsoftblueprint)/blueprintAssignments/delete | Eliminar cualquier asignación de planos técnicos |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/galleries/share/action | Permite compartir una galería con ámbitos diferentes. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants full access to manage all resources, but does not allow you to assign roles in Azure RBAC, manage assignments in Azure Blueprints, or share image galleries.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
  "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [
        "Microsoft.Authorization/*/Delete",
        "Microsoft.Authorization/*/Write",
        "Microsoft.Authorization/elevateAccess/Action",
        "Microsoft.Blueprint/blueprintAssignments/write",
        "Microsoft.Blueprint/blueprintAssignments/delete",
        "Microsoft.Compute/galleries/share/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="owner"></a>Propietario

Permite conceder acceso total para administrar todos los recursos, incluida la posibilidad de asignar roles en Azure RBAC. [Más información](rbac-and-directory-admin-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | * | Crear y administrar recursos de todos los tipos |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants full access to manage all resources, including the ability to assign roles in Azure RBAC.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader"></a>Lector

Permite ver todos los recursos, pero no realizar ningún cambio. [Más información](rbac-and-directory-admin-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View all resources, but does not allow you to make any changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "permissions": [
    {
      "actions": [
        "*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="user-access-administrator"></a>Administrador de acceso de usuario

Permite administrar el acceso de usuario a los recursos de Azure. [Más información](rbac-and-directory-admin-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/* | Administrar la autorización |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage user access to Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "User Access Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="compute"></a>Proceso


### <a name="classic-virtual-machine-contributor"></a>Colaborador de la máquina virtual clásica

Permite administrar máquinas virtuales clásicas, pero no acceder a ellas, ni tampoco a la red virtual ni la cuenta de almacenamiento a las que están conectadas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/domainNames/* | Crear y administrar nombres de dominio de proceso clásico |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/virtualMachines/* | Crear y administrar máquinas virtuales |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/networkSecurityGroups/join/action |  |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/reservedIps/link/action | Vincula una IP reservada |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/reservedIps/read | Obtiene las IP reservadas |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/virtualNetworks/join/action | Une la red virtual. |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/virtualNetworks/read | Obtiene la red virtual. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/disks/read | Devuelve el disco de la cuenta de almacenamiento. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/images/read | Devuelve la imagen de la cuenta de almacenamiento. (En desuso. Use "Microsoft.ClassicStorage/storageAccounts/vmImages"). |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/listKeys/action | Enumera las claves de acceso de las cuentas de almacenamiento. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/read | Devuelve la cuenta de almacenamiento con la cuenta especificada. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "name": "d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/domainNames/*",
        "Microsoft.ClassicCompute/virtualMachines/*",
        "Microsoft.ClassicNetwork/networkSecurityGroups/join/action",
        "Microsoft.ClassicNetwork/reservedIps/link/action",
        "Microsoft.ClassicNetwork/reservedIps/read",
        "Microsoft.ClassicNetwork/virtualNetworks/join/action",
        "Microsoft.ClassicNetwork/virtualNetworks/read",
        "Microsoft.ClassicStorage/storageAccounts/disks/read",
        "Microsoft.ClassicStorage/storageAccounts/images/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-administrator-login"></a>Inicio de sesión de administrador de Virtual Machine

Visualización de máquinas virtuales en el portal e inicio de sesión como administrador. [Más información](../active-directory/devices/howto-vm-sign-in-azure-ad-windows.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/read | Obtiene una definición de la dirección ip pública. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/read | Obtiene una definición del equilibrador de carga |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/read | Obtiene una definición de interfaz de red.  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/*/read |  |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/login/action | Iniciar sesión en una máquina virtual como usuario habitual |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/loginAsAdmin/action | Iniciar de sesión en una máquina virtual con privilegios de administrador de Windows o de usuario raíz de Linux |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/login/action | Inicia sesión en una máquina de Azure Arc como usuario habitual |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/loginAsAdmin/action | Inicia sesión en una máquina de Azure Arc con privilegios de administrador de Windows o de usuario raíz de Linux |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as administrator",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "name": "1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read",
        "Microsoft.HybridCompute/machines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action",
        "Microsoft.Compute/virtualMachines/loginAsAdmin/action",
        "Microsoft.HybridCompute/machines/login/action",
        "Microsoft.HybridCompute/machines/loginAsAdmin/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Administrator Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-contributor"></a>Colaborador de la máquina virtual

Cree y administre máquinas virtuales, administre discos e instantáneas de disco, instale y ejecute software, restablezca la contraseña del usuario raíz de la máquina virtual mediante extensiones de VM, y administre cuentas de usuario locales mediante extensiones de VM. Este rol no le concede acceso de administración a la red virtual ni a la cuenta de almacenamiento a la que están conectadas las máquinas virtuales. Este rol no le permite asignar roles en Azure RBAC. [Más información](/azure/architecture/reference-architectures/n-tier/linux-vm)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/availabilitySets/* | Crear y administrar conjuntos de disponibilidad de proceso |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/locations/* | Crear y administrar ubicaciones de proceso |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/* | Realizar todas las acciones de las máquinas virtuales, como las de creación, actualización, eliminación, inicio, reinicio y apagado. Ejecutar scripts en máquinas virtuales. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachineScaleSets/* | Crear y administrar conjuntos de escalado de máquinas virtuales |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/cloudServices/* |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/write | Crea un nuevo disco o actualiza uno ya existente |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/read | Obtiene las propiedades de un disco |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/delete | Elimina el disco |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/schedules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/applicationGateways/backendAddressPools/join/action | Se une a un grupo de direcciones de back-end de una puerta de enlace de aplicaciones. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/backendAddressPools/join/action | Se une a un grupo de direcciones de back-end del equilibrador de carga. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/inboundNatPools/join/action | Se une a conjuntos NAT de entrada del equilibrador de carga. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/inboundNatRules/join/action | Se une a una regla NAT de entrada del equilibrador de carga. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/probes/join/action | Permite usar sondeos de un equilibrador de carga. Por ejemplo, con este permiso, la propiedad healthProbe de un conjunto de escalado de máquinas virtuales puede hacer referencia al sondeo. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/read | Obtiene una definición del equilibrador de carga |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/locations/* | Crear y administrar ubicaciones de red |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/* | Crear y administrar interfaces de red |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/join/action | Se une a un grupo de seguridad de red. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/read | Obtiene una definición de grupo de seguridad de red |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/join/action | Se une a una dirección IP pública. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/read | Obtiene una definición de la dirección ip pública. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/join/action | Se une a una red virtual. No genera alertas. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/write | Crea una intención de protección de la copia de seguridad. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devuelve detalles de objeto del elemento protegido |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/write | Crea un elemento protegido de copia de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/read | Devuelve todas las directivas de protección |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/write | Crea una directiva de protección |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/write | La operación Create Vault crea un recurso de Azure del tipo "almacén" |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | Microsoft.SerialConsole/serialPorts/connect/action | Conectar a un puerto serie |
> | [Microsoft.SqlVirtualMachine](resource-provider-operations.md#microsoftsqlvirtualmachine)/* |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listKeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/locations/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/virtualMachineScaleSets/*",
        "Microsoft.Compute/cloudServices/*",
        "Microsoft.Compute/disks/write",
        "Microsoft.Compute/disks/read",
        "Microsoft.Compute/disks/delete",
        "Microsoft.DevTestLab/schedules/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/loadBalancers/probes/join/action",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/locations/*",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Network/networkSecurityGroups/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/write",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.SerialConsole/serialPorts/connect/action",
        "Microsoft.SqlVirtualMachine/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-user-login"></a>Inicio de sesión de usuario de Virtual Machine

Visualización de máquinas virtuales en el portal e inicio de sesión como usuario normal. [Más información](../active-directory/devices/howto-vm-sign-in-azure-ad-windows.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/read | Obtiene una definición de la dirección ip pública. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/read | Obtiene una definición del equilibrador de carga |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/read | Obtiene una definición de interfaz de red.  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/*/read |  |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/login/action | Iniciar sesión en una máquina virtual como usuario habitual |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/login/action | Inicia sesión en una máquina de Azure Arc como usuario habitual |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as a regular user.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb879df8-f326-4884-b1cf-06f3ad86be52",
  "name": "fb879df8-f326-4884-b1cf-06f3ad86be52",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read",
        "Microsoft.HybridCompute/machines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action",
        "Microsoft.HybridCompute/machines/login/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine User Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="networking"></a>Redes


### <a name="cdn-endpoint-contributor"></a>Colaborador de punto de conexión de CDN

Puede administrar puntos de conexión de CDN, pero no conceder acceso a otros usuarios.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/edgenodes/read |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/operationresults/* |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/profiles/endpoints/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "name": "426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-endpoint-reader"></a>Lector de punto de conexión de CDN

Puede ver puntos de conexión de CDN, pero no hacer cambios.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/edgenodes/read |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/operationresults/* |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/profiles/endpoints/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "name": "871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-contributor"></a>Colaborador de perfil de CDN

Puede administrar perfiles de CDN y sus puntos de conexión, pero no conceder acceso a otros usuarios. [Más información](../cdn/cdn-app-dev-net.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/edgenodes/read |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/operationresults/* |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/profiles/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN profiles and their endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "name": "ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-reader"></a>Lector de perfil de CDN

Puede ver perfiles de CDN y sus puntos de conexión, pero no hacer cambios.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/edgenodes/read |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/operationresults/* |  |
> | [Microsoft.Cdn](resource-provider-operations.md#microsoftcdn)/profiles/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN profiles and their endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8f96442b-4075-438f-813d-ad51ab4019af",
  "name": "8f96442b-4075-438f-813d-ad51ab4019af",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-network-contributor"></a>Colaborador de la red clásica

Permite administrar las redes clásicas, pero no acceder a ellas. [Más información](../virtual-network/virtual-network-manage-peering.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/* | Crear y administrar redes clásicas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "name": "b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicNetwork/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="dns-zone-contributor"></a>Colaborador de zona DNS

Permite administrar zonas y conjuntos de registros DNS en Azure DNS, pero no controlar los usuarios que tienen acceso. [Más información](../dns/dns-protect-zones-recordsets.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/dnsZones/* | Crear y administrar registros y zonas DNS |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DNS zones and record sets in Azure DNS, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/befefa01-2a29-4197-83a8-272ff33ce314",
  "name": "befefa01-2a29-4197-83a8-272ff33ce314",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/dnsZones/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DNS Zone Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="network-contributor"></a>Colaborador de la red

Permite administrar redes, pero no acceder a ellas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/* | Crear y administrar redes |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4d97b98b-1d4f-4787-a291-c67834d212e7",
  "name": "4d97b98b-1d4f-4787-a291-c67834d212e7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="private-dns-zone-contributor"></a>Colaborador de zona DNS privada

Permite administrar recursos de zonas DNS privadas, pero no las redes virtuales a las que están vinculados. [Más información](../dns/dns-protect-private-zones-recordsets.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/privateDnsZones/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/privateDnsOperationResults/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/privateDnsOperationStatuses/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/join/action | Se une a una red virtual. No genera alertas. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage private DNS zone resources, but not the virtual networks they are linked to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b12aa53e-6015-4669-85d0-8515ebb3ae7f",
  "name": "b12aa53e-6015-4669-85d0-8515ebb3ae7f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/privateDnsZones/*",
        "Microsoft.Network/privateDnsOperationResults/*",
        "Microsoft.Network/privateDnsOperationStatuses/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/join/action",
        "Microsoft.Authorization/*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Private DNS Zone Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="traffic-manager-contributor"></a>Colaborador de Traffic Manager

Le permite administrar perfiles de Traffic Manager, pero no controlar los usuarios que tienen acceso a ellos.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/trafficManagerProfiles/* |  |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Traffic Manager profiles, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "name": "a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/trafficManagerProfiles/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Traffic Manager Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="storage"></a>Storage


### <a name="avere-contributor"></a>Colaborador de Avere

Puede crear y administrar un clúster de Avere vFXT. [Más información](../avere-vfxt/avere-vfxt-deploy-plan.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/*/read |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/availabilitySets/* |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/proximityPlacementGroups/* |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/* |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/*/read |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/read | Obtiene una definición de subred de red virtual |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/join/action | Se une a una red virtual. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/join/action | Se une a un grupo de seguridad de red. No genera alertas. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/*/read |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/* | Crear y administrar cuentas de almacenamiento |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/resources/read | Obtiene los recursos del grupo de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/delete | Devuelve el resultado de la eliminación de un blob. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/read | Devuelve un blob o una lista de blobs. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/write | Devuelve el resultado de la escritura de un blob. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can create and manage an Avere vFXT cluster.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "name": "4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/proximityPlacementGroups/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/disks/*",
        "Microsoft.Network/*/read",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/*/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="avere-operator"></a>Operador de Avere

Lo usa el clúster de Avere vFXT para su administración. [Más información](../avere-vfxt/avere-vfxt-manage-cluster.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/read | Obtiene las propiedades de una máquina virtual |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/read | Obtiene una definición de interfaz de red.  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/write | Crea una interfaz de red o actualiza una interfaz de red existente.  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/read | Obtiene una definición de subred de red virtual |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/join/action | Se une a una red virtual. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/join/action | Se une a un grupo de seguridad de red. No genera alertas. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/delete | Devuelve el resultado de la eliminación de un contenedor. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/read | Devuelve una lista de contenedores. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/write | Devuelve el resultado del contenedor de blobs de colocación. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/delete | Devuelve el resultado de la eliminación de un blob. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/read | Devuelve un blob o una lista de blobs. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/write | Devuelve el resultado de la escritura de un blob. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Used by the Avere vFXT cluster to manage the cluster",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "name": "c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "permissions": [
    {
      "actions": [
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-contributor"></a>Colaborador de copias de seguridad

Permite administrar el servicio de copias de seguridad, pero no puede crear almacenes ni conceder acceso a otros usuarios. [Más información](../backup/backup-rbac-rs-vault.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/operationResults/* | Administrar los resultados de la operación de administración de copias de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/* | Crear y administrar contenedores de copias de seguridad dentro de tejidos de copia de seguridad del almacén de Recovery Services |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/refreshContainers/action | Actualiza la lista de contenedores |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobs/* | Crear y administrar trabajos de copia de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobsExport/action | Exporta trabajos |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperationResults/* | Crear y administrar resultados de operaciones de administración de copias de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/* | Crear y administrar directivas de copia de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectableItems/* | Crear y administrar elementos de los que se puede realizar una copia de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectedItems/* | Crear y administrar elementos de los que se ha realizado una copia de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionContainers/* | Crear y administrar contenedores que incluyen elementos de copia de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupSecurityPIN/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupUsageSummaries/read | Devuelve resúmenes de los elementos y servidores protegidos para un almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/certificates/* | Crear y administrar certificados relacionados con copias de seguridad en el almacén de Recovery Services |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/* | Crear y administrar información ampliada relacionada con el almacén |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/read | Obtiene las alertas del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/* | Crear y administrar identidades registradas |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/* | Crear y administrar el uso del almacén de Recovery Services |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupstorageconfig/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupconfig/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupValidateOperation/action | Valida la operación en el elemento protegido. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/write | La operación Create Vault crea un recurso de Azure del tipo "almacén" |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperations/read | Devuelve el estado de la operación de Backup para el almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupEngines/read | Devuelve todos los servidores de administración de copia de seguridad que se registraron con el almacén. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectableContainers/read | Obtiene todos los contenedores que se pueden proteger. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupStatus/action | Comprueba el estado de la copia de seguridad de los almacenes de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupPreValidateProtection/action |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupValidateFeatures/action | Valida las características. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/write | Resuelve la alerta. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/operations/read | La operación devuelve la lista de operaciones de un proveedor de recursos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/operationStatus/read | Obtiene el estado de la operación para una operación determinada. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionIntents/read | Enumera todas las intenciones de protección de la copia de seguridad. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/getBackupStatus/action | Comprueba el estado de la copia de seguridad de los almacenes de Recovery Services. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/write | Crea una instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/delete | Elimina la instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Devuelve todas las instancias de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Devuelve todas las instancias de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/backup/action | Crea una copia de seguridad en la instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/validateRestore/action | Valida la restauración de la instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/restore/action | Desencadena la restauración en la instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/write | Crea una directiva de copia de seguridad. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/delete | Elimina la directiva de copia de seguridad. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Devuelve todas las directivas de copia de seguridad. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Devuelve todas las directivas de copia de seguridad. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Devuelve todos los puntos de recuperación. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Devuelve todos los puntos de recuperación. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/findRestorableTimeRanges/action | Busca intervalos de tiempo de restauración. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/write | La operación de creación de BackupVault crea un recurso de Azure del tipo "almacén de Backup". |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Obtiene la lista de almacenes de Backup de una suscripción. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/operationResults/read | Obtiene el resultado de la operación de una operación de revisión de un almacén de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/checkNameAvailability/action | Compruebe si el nombre de BackupVault solicitado está disponible. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Obtiene la lista de almacenes de Backup de una suscripción. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Obtiene la lista de almacenes de Backup de una suscripción. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationStatus/read | Devuelve el estado de la operación de copia de seguridad del almacén de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationResults/read | Devuelve el resultado de la operación de copia de seguridad para el almacén de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/validateForBackup/action | Valida la copia de seguridad de la instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/providers/operations/read | La operación devuelve la lista de operaciones de un proveedor de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup service,but can't create vaults and give access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e467623-bb1f-42f4-a55d-6e525e11384b",
  "name": "5e467623-bb1f-42f4-a55d-6e525e11384b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupSecurityPIN/*",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/*",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/Vaults/usages/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*",
        "Microsoft.DataProtection/locations/getBackupStatus/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/write",
        "Microsoft.DataProtection/backupVaults/backupInstances/delete",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/backup/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/validateRestore/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/restore/action",
        "Microsoft.DataProtection/backupVaults/backupPolicies/write",
        "Microsoft.DataProtection/backupVaults/backupPolicies/delete",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/findRestorableTimeRanges/action",
        "Microsoft.DataProtection/backupVaults/write",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/operationResults/read",
        "Microsoft.DataProtection/locations/checkNameAvailability/action",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/locations/operationStatus/read",
        "Microsoft.DataProtection/locations/operationResults/read",
        "Microsoft.DataProtection/backupVaults/validateForBackup/action",
        "Microsoft.DataProtection/providers/operations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-operator"></a>Operador de copias de seguridad

Permite administrar los servicios de copias de seguridad, excepto la eliminación de copias de seguridad, la creación de almacenes y la concesión de acceso a otros usuarios. [Más información](../backup/backup-rbac-rs-vault.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/operationResults/read | Devuelve el estado de la operación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/operationResults/read | Obtiene los resultados de la operación realizada en el contenedor de protección. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Hace una copia de seguridad del elemento protegido. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Obtiene el resultado de la operación realizada en los elementos protegidos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Devuelve el estado de la operación realizada en los elementos protegidos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devuelve detalles de objeto del elemento protegido |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Aprovisiona una recuperación de elementos instantánea para los elementos protegidos |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/accessToken/action | Obtiene AccessToken para la restauración entre regiones. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Obtiene los puntos de recuperación de los elementos protegidos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Restaura los puntos de recuperación de los elementos protegidos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Revoca la recuperación de elementos instantánea para los elementos protegidos |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/write | Crea un elemento protegido de copia de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/read | Devuelve todos los contenedores registrados |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/refreshContainers/action | Actualiza la lista de contenedores |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobs/* | Crear y administrar trabajos de copia de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobsExport/action | Exporta trabajos |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperationResults/* | Crear y administrar resultados de operaciones de administración de copias de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/operationResults/read | Obtiene los resultados de la operación de directiva. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/read | Devuelve todas las directivas de protección |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectableItems/* | Crear y administrar elementos de los que se puede realizar una copia de seguridad |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectedItems/read | Devuelve la lista de todos los elementos protegidos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionContainers/read | Devuelve todos los contenedores que pertenecen a la suscripción |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupUsageSummaries/read | Devuelve resúmenes de los elementos y servidores protegidos para un almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/certificates/write | La operación Actualizar certificado de recursos permite actualizar el certificado de credencial de recursos o almacenes. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/read | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/write | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/read | Obtiene las alertas del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/operationResults/read | La operación Obtener resultados de la operación se puede usar para obtener el estado y el resultado de la operación enviada de forma asincrónica |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/read | La operación Obtener contenedores se puede usar para obtener los contenedores registrados para un recurso. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/write | La operación Registrar contenedor de servicios se puede usar para registrar un contenedor con servicio de recuperación. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupstorageconfig/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupValidateOperation/action | Valida la operación en el elemento protegido. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperations/read | Devuelve el estado de la operación de Backup para el almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/operations/read | Obtiene el estado de la operación de directiva. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/write | Crea un contenedor registrado. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/inquire/action | Realiza consultas para las cargas de trabajo de un contenedor. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupEngines/read | Devuelve todos los servidores de administración de copia de seguridad que se registraron con el almacén. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/write | Crea una intención de protección de la copia de seguridad. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/read | Obtiene una intención de protección de la copia de seguridad. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectableContainers/read | Obtiene todos los contenedores que se pueden proteger. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/items/read | Obtiene todos los elementos de un contenedor. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupStatus/action | Comprueba el estado de la copia de seguridad de los almacenes de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupPreValidateProtection/action |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupValidateFeatures/action | Valida las características. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupAadProperties/read | Obtiene las propiedades de AAD para la autenticación en la tercera región para la restauración entre regiones. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrJobs/action | Enumera los trabajos de restauración entre regiones en la región secundaria del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrJob/action | Obtiene los detalles del trabajo de restauración entre regiones en la región secundaria del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrossRegionRestore/action | Desencadena la restauración entre regiones. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrOperationResults/read | Devuelve el resultado de la operación CRR para el almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrOperationsStatus/read | Devuelve el estado de la operación CRR para el almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/write | Resuelve la alerta. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/operations/read | La operación devuelve la lista de operaciones de un proveedor de recursos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/operationStatus/read | Obtiene el estado de la operación para una operación determinada. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionIntents/read | Enumera todas las intenciones de protección de la copia de seguridad. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Devuelve todas las instancias de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Devuelve todas las instancias de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Devuelve todas las directivas de copia de seguridad. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Devuelve todas las directivas de copia de seguridad. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Devuelve todos los puntos de recuperación. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Devuelve todos los puntos de recuperación. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/findRestorableTimeRanges/action | Busca intervalos de tiempo de restauración. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Obtiene la lista de almacenes de Backup de una suscripción. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/operationResults/read | Obtiene el resultado de la operación de una operación de revisión de un almacén de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Obtiene la lista de almacenes de Backup de una suscripción. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Obtiene la lista de almacenes de Backup de una suscripción. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationStatus/read | Devuelve el estado de la operación de copia de seguridad del almacén de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationResults/read | Devuelve el resultado de la operación de copia de seguridad para el almacén de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/providers/operations/read | La operación devuelve la lista de operaciones de un proveedor de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup services, except removal of backup, vault creation and giving access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00c29273-979b-4161-815c-10b084fb9324",
  "name": "00c29273-979b-4161-815c-10b084fb9324",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action",
        "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/accessToken/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/write",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/write",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/locations/backupAadProperties/read",
        "Microsoft.RecoveryServices/locations/backupCrrJobs/action",
        "Microsoft.RecoveryServices/locations/backupCrrJob/action",
        "Microsoft.RecoveryServices/locations/backupCrossRegionRestore/action",
        "Microsoft.RecoveryServices/locations/backupCrrOperationResults/read",
        "Microsoft.RecoveryServices/locations/backupCrrOperationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/findRestorableTimeRanges/action",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/operationResults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/locations/operationStatus/read",
        "Microsoft.DataProtection/locations/operationResults/read",
        "Microsoft.DataProtection/providers/operations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-reader"></a>Lector de copias de seguridad

Puede ver servicios de copia de seguridad, pero no puede realizar cambios. [Más información](../backup/backup-rbac-rs-vault.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocatedStamp/read | GetAllocatedStamp es una operación interna que el servicio usa |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/operationResults/read | Devuelve el estado de la operación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/operationResults/read | Obtiene los resultados de la operación realizada en el contenedor de protección. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Obtiene el resultado de la operación realizada en los elementos protegidos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Devuelve el estado de la operación realizada en los elementos protegidos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/read | Devuelve detalles de objeto del elemento protegido |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Obtiene los puntos de recuperación de los elementos protegidos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/read | Devuelve todos los contenedores registrados |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobs/operationResults/read | Devuelve el resultado de la operación de trabajo. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobs/read | Devuelve todos los objetos de trabajo |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupJobsExport/action | Exporta trabajos |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperationResults/read | Devuelve el resultado de la operación de Backup para el almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/operationResults/read | Obtiene los resultados de la operación de directiva. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/read | Devuelve todas las directivas de protección |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectedItems/read | Devuelve la lista de todos los elementos protegidos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionContainers/read | Devuelve todos los contenedores que pertenecen a la suscripción |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupUsageSummaries/read | Devuelve resúmenes de los elementos y servidores protegidos para un almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/read | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/read | Obtiene las alertas del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/operationResults/read | La operación Obtener resultados de la operación se puede usar para obtener el estado y el resultado de la operación enviada de forma asincrónica |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/read | La operación Obtener contenedores se puede usar para obtener los contenedores registrados para un recurso. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupstorageconfig/read | Devuelve la configuración de almacenamiento del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupconfig/read | Devuelve la configuración del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupOperations/read | Devuelve el estado de la operación de Backup para el almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupPolicies/operations/read | Obtiene el estado de la operación de directiva. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupEngines/read | Devuelve todos los servidores de administración de copia de seguridad que se registraron con el almacén. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/backupProtectionIntent/read | Obtiene una intención de protección de la copia de seguridad. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupFabrics/protectionContainers/items/read | Obtiene todos los elementos de un contenedor. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupStatus/action | Comprueba el estado de la copia de seguridad de los almacenes de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/write | Resuelve la alerta. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/operations/read | La operación devuelve la lista de operaciones de un proveedor de recursos. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/operationStatus/read | Obtiene el estado de la operación para una operación determinada. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/backupProtectionIntents/read | Enumera todas las intenciones de protección de la copia de seguridad. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupValidateFeatures/action | Valida las características. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrJobs/action | Enumera los trabajos de restauración entre regiones en la región secundaria del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrJob/action | Obtiene los detalles del trabajo de restauración entre regiones en la región secundaria del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrOperationResults/read | Devuelve el resultado de la operación CRR para el almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/backupCrrOperationsStatus/read | Devuelve el estado de la operación CRR para el almacén de Recovery Services. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/getBackupStatus/action | Comprueba el estado de la copia de seguridad de los almacenes de Recovery Services. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/write | Crea una instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Devuelve todas las instancias de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/read | Devuelve todas las instancias de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/backup/action | Crea una copia de seguridad en la instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/validateRestore/action | Valida la restauración de la instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/restore/action | Desencadena la restauración en la instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Devuelve todas las directivas de copia de seguridad. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupPolicies/read | Devuelve todas las directivas de copia de seguridad. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Devuelve todos los puntos de recuperación. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/recoveryPoints/read | Devuelve todos los puntos de recuperación. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/backupInstances/findRestorableTimeRanges/action | Busca intervalos de tiempo de restauración. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Obtiene la lista de almacenes de Backup de una suscripción. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/operationResults/read | Obtiene el resultado de la operación de una operación de revisión de un almacén de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Obtiene la lista de almacenes de Backup de una suscripción. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/read | Obtiene la lista de almacenes de Backup de una suscripción. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationStatus/read | Devuelve el estado de la operación de copia de seguridad del almacén de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/locations/operationResults/read | Devuelve el resultado de la operación de copia de seguridad para el almacén de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/backupVaults/validateForBackup/action | Valida la copia de seguridad de la instancia de Backup. |
> | [Microsoft.DataProtection](resource-provider-operations.md#microsoftdataprotection)/providers/operations/read | La operación devuelve la lista de operaciones de un proveedor de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view backup services, but can't make changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "name": "a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/read",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/locations/backupCrrJobs/action",
        "Microsoft.RecoveryServices/locations/backupCrrJob/action",
        "Microsoft.RecoveryServices/locations/backupCrrOperationResults/read",
        "Microsoft.RecoveryServices/locations/backupCrrOperationsStatus/read",
        "Microsoft.DataProtection/locations/getBackupStatus/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/write",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/backup/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/validateRestore/action",
        "Microsoft.DataProtection/backupVaults/backupInstances/restore/action",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupPolicies/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/recoveryPoints/read",
        "Microsoft.DataProtection/backupVaults/backupInstances/findRestorableTimeRanges/action",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/operationResults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/backupVaults/read",
        "Microsoft.DataProtection/locations/operationStatus/read",
        "Microsoft.DataProtection/locations/operationResults/read",
        "Microsoft.DataProtection/backupVaults/validateForBackup/action",
        "Microsoft.DataProtection/providers/operations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-contributor"></a>Colaborador de cuentas de almacenamiento clásico

Permite administrar cuentas de almacenamiento clásicas, pero no acceder a ellas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/* | Crear y administrar cuentas de almacenamiento |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic storage accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "name": "86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-key-operator-service-role"></a>Rol de servicio de operador de claves de cuentas de almacenamiento clásicas

Los operadores de claves de cuentas de almacenamiento clásicas pueden enumerar y regenerar claves en cuentas de almacenamiento clásicas. [Más información](../key-vault/secrets/overview-storage-keys.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/listkeys/action | Enumera las claves de acceso de las cuentas de almacenamiento. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/regeneratekey/action | Regenera las claves de acceso existentes de la cuenta de almacenamiento. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Classic Storage Account Key Operators are allowed to list and regenerate keys on Classic Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "name": "985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ClassicStorage/storageAccounts/listkeys/action",
        "Microsoft.ClassicStorage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-contributor"></a>Colaborador de Data Box

Permite administrarlo todo en el servicio Data Box, excepto dar acceso a otros usuarios. [Más información](../databox/data-box-logs.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything under Data Box Service except giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/add466c9-e687-43fc-8d98-dfcf8d720be5",
  "name": "add466c9-e687-43fc-8d98-dfcf8d720be5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Databox/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-reader"></a>Lector de Data Box

Permite administrar el servicio Data Box excepto la creación o edición de detalles de pedido y dar acceso a otros usuarios. [Más información](../databox/data-box-logs.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/*/read |  |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/jobs/listsecrets/action |  |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/jobs/listcredentials/action | Enumera las credenciales sin cifrar relacionadas con el pedido. |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/locations/availableSkus/action | Este método devuelve la lista de SKU disponibles. |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/locations/validateInputs/action | Este método realiza todo tipo de validaciones. |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/locations/regionConfiguration/action | Este método devuelve las configuraciones de la región. |
> | [Microsoft.Databox](resource-provider-operations.md#microsoftdatabox)/locations/validateAddress/action | Valida la dirección de envío y proporciona direcciones alternativas, si existen. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Data Box Service except creating order or editing order details and giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "name": "028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Databox/*/read",
        "Microsoft.Databox/jobs/listsecrets/action",
        "Microsoft.Databox/jobs/listcredentials/action",
        "Microsoft.Databox/locations/availableSkus/action",
        "Microsoft.Databox/locations/validateInputs/action",
        "Microsoft.Databox/locations/regionConfiguration/action",
        "Microsoft.Databox/locations/validateAddress/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-lake-analytics-developer"></a>Desarrollador de Data Lake Analytics

Le permite enviar, supervisar y administrar sus propios trabajos, pero no crear ni eliminar cuentas de Data Lake Analytics. [Más información](../data-lake-analytics/data-lake-analytics-manage-use-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | Microsoft.BigAnalytics/accounts/* |  |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/Delete | Elimina la cuenta de DataLakeAnalytics. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/TakeOwnership/action | Concede permisos para cancelar trabajos que enviaron otros usuarios. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/Write | Crea o actualiza una cuenta de DataLakeAnalytics. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/dataLakeStoreAccounts/Write | Crea o actualiza una cuenta vinculada de DataLakeStore en la cuenta de DataLakeAnalytics. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/dataLakeStoreAccounts/Delete | Anula la vinculación de una cuenta de DataLakeStore a la cuenta de DataLakeAnalytics. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/storageAccounts/Write | Crea o actualiza una cuenta de almacenamiento vinculada a una cuenta de DataLakeAnalytics. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/storageAccounts/Delete | Anula la vinculación de una cuenta de almacenamiento a la cuenta de DataLakeAnalytics. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/firewallRules/Write | Crea o actualiza una regla de firewall. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/firewallRules/Delete | Elimina una regla de firewall. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/computePolicies/Write | Crea o actualiza una directiva de proceso. |
> | [Microsoft.DataLakeAnalytics](resource-provider-operations.md#microsoftdatalakeanalytics)/accounts/computePolicies/Delete | Elimina una directiva de proceso. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you submit, monitor, and manage your own jobs but not create or delete Data Lake Analytics accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/47b7735b-770e-4598-a7da-8b91488b4c88",
  "name": "47b7735b-770e-4598-a7da-8b91488b4c88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BigAnalytics/accounts/*",
        "Microsoft.DataLakeAnalytics/accounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.BigAnalytics/accounts/Delete",
        "Microsoft.BigAnalytics/accounts/TakeOwnership/action",
        "Microsoft.BigAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action",
        "Microsoft.DataLakeAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Write",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Write",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Lake Analytics Developer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader-and-data-access"></a>Lector y acceso a los datos

Permite ver todo el contenido, pero no eliminar ni crear una cuenta de almacenamiento ni un recurso incluido. También permitirá el acceso de lectura o escritura para todos los datos incluidos en una cuenta de almacenamiento a través del acceso a las claves de la cuenta de almacenamiento.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listKeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/ListAccountSas/action | Devuelve el token de SAS de la cuenta de almacenamiento especificada. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view everything but will not let you delete or create a storage account or contained resource. It will also allow read/write access to all data contained in a storage account via access to storage account keys.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c12c1c16-33a1-487b-954d-41c89c60f349",
  "name": "c12c1c16-33a1-487b-954d-41c89c60f349",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/ListAccountSas/action",
        "Microsoft.Storage/storageAccounts/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader and Data Access",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-contributor"></a>Colaborador de la cuenta de almacenamiento

Permite la administración de cuentas de almacenamiento. Proporciona acceso a la clave de cuenta, que puede usarse para tener acceso a datos a través de la autorización de clave compartida. [Más información](../storage/common/storage-auth-aad.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Crea, actualiza o lee la configuración de diagnóstico de Analysis Server. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/* | Crear y administrar cuentas de almacenamiento |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage storage accounts, including accessing storage account keys which provide full access to storage account data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "name": "17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-key-operator-service-role"></a>Rol de servicio de operador de claves de cuentas de almacenamiento

Permite enumerar y regenerar claves de acceso de la cuenta de almacenamiento. [Más información](../storage/common/storage-account-keys-manage.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listkeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/regeneratekey/action | Regenera las claves de acceso de la cuenta de almacenamiento especificada. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Storage Account Key Operators are allowed to list and regenerate keys on Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/81a9662b-bebf-436f-a333-f67b29880f12",
  "name": "81a9662b-bebf-436f-a333-f67b29880f12",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-contributor"></a>Colaborador de datos de blobs de almacenamiento

Lee, escribe y elimina blobs y contenedores de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). [Más información](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/delete | Elimina un contenedor. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/read | Devuelve un contenedor o una lista de contenedores. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/write | Modifica los metadatos o las propiedades de un contenedor. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/generateUserDelegationKey/action | Devuelve una clave de delegación de usuarios para la instancia de Blob service. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/delete | Eliminar un blob. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/read | Devuelve un blob o una lista de blobs. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/write | Escribe en un blob. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/move/action | Mueve el blob de una ruta de acceso a otra. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/add/action | Devuelve el resultado de agregar el contenido del blob. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write and delete access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "name": "ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/move/action",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-owner"></a>Propietario de datos de blobs de almacenamiento

Proporciona acceso total a los contenedores de blobs y los datos de Azure Storage, incluida la asignación de control de acceso POSIX. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). [Más información](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/* | Todos los permisos en los contenedores. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/generateUserDelegationKey/action | Devuelve una clave de delegación de usuarios para la instancia de Blob service. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/* | Todos los permisos en los blobs. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "name": "b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/*",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-reader"></a>Lector de datos de blobs de almacenamiento

Lee y enumera blobs y contenedores de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). [Más información](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/read | Devuelve un contenedor o una lista de contenedores. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/generateUserDelegationKey/action | Devuelve una clave de delegación de usuarios para la instancia de Blob service. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/containers/blobs/read | Devuelve un blob o una lista de blobs. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-delegator"></a>Delegador de Blob Storage

Obtiene una clave de delegación de usuarios, que se puede usar a continuación para crear una firma de acceso compartido para un contenedor o un blob firmado con credenciales de Azure AD. Para más información, vea [Creación de SAS de delegación de usuarios](/rest/api/storageservices/create-user-delegation-sas). [Más información](/rest/api/storageservices/get-user-delegation-key)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/blobServices/generateUserDelegationKey/action | Devuelve una clave de delegación de usuarios para la instancia de Blob service. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for generation of a user delegation key which can be used to sign SAS tokens",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "name": "db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Delegator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-contributor"></a>Colaborador de recursos compartidos de SMB de datos de archivos de Storage

Permite el acceso de lectura, escritura y eliminación a los archivos y directorios de los recursos compartidos de Azure. Este rol no tiene ningún equivalente integrado en los servidores de archivos de Windows. [Más información](../storage/files/storage-files-identity-auth-active-directory-enable.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/read | Devuelve un archivo o una carpeta, o bien una lista de archivos o carpetas. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/write | Devuelve el resultado de escribir un archivo o de crear una carpeta. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/delete | Devuelve el resultado de eliminar un archivo o una carpeta. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "name": "0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-elevated-contributor"></a>Colaborador elevado de recursos compartidos de SMB de datos de archivos de Storage

Permite el acceso de lectura, escritura, eliminación y modificación de ACL en los archivos y directorios de los recursos compartidos de Azure. Este rol es equivalente a una ACL de recurso compartido de cambio en los servidores de archivos de Windows. [Más información](../storage/files/storage-files-identity-auth-active-directory-enable.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/read | Devuelve un archivo o una carpeta, o bien una lista de archivos o carpetas. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/write | Devuelve el resultado de escribir un archivo o de crear una carpeta. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/delete | Devuelve el resultado de eliminar un archivo o una carpeta. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/modifypermissions/action | Devuelve el resultado de modificar el permiso en un archivo o una carpeta. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, delete and modify NTFS permission access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a7264617-510b-434b-a828-9731dc254ea7",
  "name": "a7264617-510b-434b-a828-9731dc254ea7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Elevated Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-reader"></a>Lector de recursos compartidos de SMB de datos de archivos de Storage

Permite el acceso de lectura a los archivos y directorios de los recursos compartidos de Azure. Este rol es equivalente a una ACL de recurso compartido de lectura en los servidores de archivos de Windows. [Más información](../storage/files/storage-files-identity-auth-active-directory-enable.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/fileServices/fileshares/files/read | Devuelve un archivo o una carpeta, o bien una lista de archivos o carpetas. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure File Share over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/aba4ae5f-2193-4029-9191-0cb91df5e314",
  "name": "aba4ae5f-2193-4029-9191-0cb91df5e314",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-contributor"></a>Colaborador de datos de la cola de Storage

Lee, escribe y elimina los mensajes de la cola y a la cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). [Más información](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/delete | Elimina una cola. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/read | Devuelve una cola o una lista de colas. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/write | Modifica las propiedades o los metadatos de la cola. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/delete | Elimina uno o más mensajes de una cola. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/read | Consulta o recupera uno o más mensajes de una cola. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/write | Agrega un mensaje a una cola. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/process/action | Devuelve el resultado de procesar un mensaje. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "name": "974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/write",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-processor"></a>Procesador de mensajes de datos de la cola de Storage

Consulta, recupera y elimina un mensaje de una cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). [Más información](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/read | Consulta un mensaje. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/process/action | Recupera y elimina un mensaje. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for peek, receive, and delete access to Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "name": "8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Processor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-sender"></a>Emisor de mensajes de datos de la cola de Storage

Agrega mensaje a una cola de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). [Más información](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/add/action | Agrega un mensaje a una cola. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for sending of Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "name": "c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-reader"></a>Lector de datos de la cola de Storage

Lee y enumera los mensajes de la cola y las colas de Azure Storage. Para aprender qué acciones son necesarias para una operación de datos determinada, consulte [Permissions for calling blob and queue data operations](/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations) (Permisos para llamar a operaciones de datos de blob y de cola). [Más información](../storage/common/storage-auth-aad-rbac-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/read | Devuelve una cola o una lista de colas. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/queueServices/queues/messages/read | Consulta o recupera uno o más mensajes de una cola. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/19e7f393-937e-4f77-808e-94535e297925",
  "name": "19e7f393-937e-4f77-808e-94535e297925",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-table-data-contributor"></a>Colaborador de datos de tabla de Storage

Permite el acceso de lectura, escritura y eliminación a tablas y entidades de Azure Storage

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/read | Consulta de tablas |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/write | Crear tablas. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/delete | Elimina tablas. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/read | Consulta las entidades de tabla. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/write | Permite insertar, combinar o reemplazar entidades de tabla. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/delete | Eliminar entidades de tabla |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/add/action | Inserta entidades de tabla. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/update/action | Combina o actualiza las entidades de tabla. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write and delete access to Azure Storage tables and entities",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0a9a7e1f-b9d0-4cc4-a60d-0319b160aaa3",
  "name": "0a9a7e1f-b9d0-4cc4-a60d-0319b160aaa3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/tableServices/tables/read",
        "Microsoft.Storage/storageAccounts/tableServices/tables/write",
        "Microsoft.Storage/storageAccounts/tableServices/tables/delete"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/read",
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/write",
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/delete",
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/add/action",
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/update/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Table Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-table-data-reader"></a>Lector de datos de tabla de Storage

Permite el acceso de lectura a tablas y entidades de Azure Storage

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/read | Consulta de tablas |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/tableServices/tables/entities/read | Consulta las entidades de tabla. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage tables and entities",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/76199698-9eea-4c19-bc75-cec21354c6b6",
  "name": "76199698-9eea-4c19-bc75-cec21354c6b6",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/tableServices/tables/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/tableServices/tables/entities/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Table Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="web"></a>Web


### <a name="azure-maps-data-contributor"></a>Colaborador de datos de Azure Maps

Conde acceso para leer, escribir y eliminar datos relacionados con mapas desde una cuenta de mapas de Azure. [Más información](../azure-maps/azure-maps-authentication.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/read |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/write |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/delete |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants access to read, write, and delete access to map related data from an Azure maps account.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8f5e0ce6-4f7b-4dcf-bddf-e6f48634a204",
  "name": "8f5e0ce6-4f7b-4dcf-bddf-e6f48634a204",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Maps/accounts/*/read",
        "Microsoft.Maps/accounts/*/write",
        "Microsoft.Maps/accounts/*/delete",
        "Microsoft.Maps/accounts/*/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Maps Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-maps-data-reader"></a>Azure Maps Data Reader

Concede acceso de lectura a los datos de los mapas de una cuenta de Azure Maps. [Más información](../azure-maps/azure-maps-authentication.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Maps](resource-provider-operations.md#microsoftmaps)/accounts/*/read |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants access to read map related data from an Azure maps account.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "name": "423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Maps/accounts/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Maps Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-config-server-contributor"></a>Colaborador en Config Server de Azure Spring Cloud

Permite el acceso de lectura, escritura y eliminación en Config Server en Azure Spring Cloud. [Más información](../spring-cloud/how-to-access-data-plane-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/configService/read | Permite leer el contenido de la configuración (por ejemplo, application.yaml) para una instancia de servicio específica de Azure Spring Cloud. |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/configService/write | Escribe el contenido del servidor de configuración para una instancia de servicio específica de Azure Spring Cloud. |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/configService/delete | Elimina el contenido del servidor de configuración para una instancia de servicio específica de Azure Spring Cloud. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read, write and delete access to Azure Spring Cloud Config Server",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a06f5c24-21a7-4e1a-aa2b-f19eb6684f5b",
  "name": "a06f5c24-21a7-4e1a-aa2b-f19eb6684f5b",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/configService/read",
        "Microsoft.AppPlatform/Spring/configService/write",
        "Microsoft.AppPlatform/Spring/configService/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Config Server Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-config-server-reader"></a>Lectura en Config Server de Azure Spring Cloud

Permite el acceso de lectura a Config Server en Azure Spring Cloud. [Más información](../spring-cloud/how-to-access-data-plane-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/configService/read | Permite leer el contenido de la configuración (por ejemplo, application.yaml) para una instancia de servicio específica de Azure Spring Cloud. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read access to Azure Spring Cloud Config Server",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d04c6db6-4947-4782-9e91-30a88feb7be7",
  "name": "d04c6db6-4947-4782-9e91-30a88feb7be7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/configService/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Config Server Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-data-reader"></a>Lector de datos de Azure Spring Cloud

Permite el acceso de lectura a los datos de Azure Spring Cloud.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/*/read |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read access to Azure Spring Cloud Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b5537268-8956-4941-a8f0-646150406f0c",
  "name": "b5537268-8956-4941-a8f0-646150406f0c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-service-registry-contributor"></a>Colaborador en Service Registry de Azure Spring Cloud

Permite el acceso de lectura, escritura y eliminación en el registro de servicios de Azure Spring Cloud. [Más información](../spring-cloud/how-to-access-data-plane-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/eurekaService/read | Permite leer la información de registro de aplicaciones de usuario para una instancia de servicio específica de Azure Spring Cloud. |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/eurekaService/write | Permite escribir la información de registro de aplicaciones de usuario para una instancia de servicio específica de Azure Spring Cloud. |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/eurekaService/delete | Permite eliminar la información de registro de aplicaciones de usuario para una instancia de servicio específica de Azure Spring Cloud. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read, write and delete access to Azure Spring Cloud Service Registry",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f5880b48-c26d-48be-b172-7927bfa1c8f1",
  "name": "f5880b48-c26d-48be-b172-7927bfa1c8f1",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/eurekaService/read",
        "Microsoft.AppPlatform/Spring/eurekaService/write",
        "Microsoft.AppPlatform/Spring/eurekaService/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Service Registry Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-spring-cloud-service-registry-reader"></a>Lectura en Service Registry de Azure Spring Cloud

Permite el acceso de lectura en el registro de servicios de Azure Spring Cloud. [Más información](../spring-cloud/how-to-access-data-plane-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.AppPlatform](resource-provider-operations.md#microsoftappplatform)/Spring/eurekaService/read | Permite leer la información de registro de aplicaciones de usuario para una instancia de servicio específica de Azure Spring Cloud. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allow read access to Azure Spring Cloud Service Registry",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cff1b556-2399-4e7e-856d-a8f754be7b65",
  "name": "cff1b556-2399-4e7e-856d-a8f754be7b65",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppPlatform/Spring/eurekaService/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Spring Cloud Service Registry Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-account-administrator"></a>Administrador de cuenta de Media Services

Crear, leer, modificar y eliminar cuentas de Media Services; acceso de solo lectura a otros recursos de Media Services.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Lee definiciones de métricas |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/listStreamingLocators/action | Enumera los localizadores de streaming para el recurso. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listPaths/action | Enumera las rutas |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/write | Crea o actualiza cualquier cuenta de Media Services. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/delete | Elimina cualquier cuenta de Media Services. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/privateEndpointConnectionsApproval/action | Aprueba las conexiones de punto de conexión privado. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/privateEndpointConnections/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Media Services accounts; read-only access to other Media Services resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/054126f8-9a2b-4f1c-a9ad-eca461f08466",
  "name": "054126f8-9a2b-4f1c-a9ad-eca461f08466",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/listStreamingLocators/action",
        "Microsoft.Media/mediaservices/streamingLocators/listPaths/action",
        "Microsoft.Media/mediaservices/write",
        "Microsoft.Media/mediaservices/delete",
        "Microsoft.Media/mediaservices/privateEndpointConnectionsApproval/action",
        "Microsoft.Media/mediaservices/privateEndpointConnections/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Account Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-live-events-administrator"></a>Administrador de eventos en directo de Media Services

Crear, leer, modificar y eliminar eventos en directo, recursos, filtros de recursos y localizadores de streaming; acceso de solo lectura a otros recursos de Media Services.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Lee definiciones de métricas |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/assetfilters/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/liveEvents/* |  |
> | **NotActions** |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/getEncryptionKey/action | Obtiene una clave de cifrado de recursos |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listContentKeys/action | Enumera las claves de contenido |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Live Events, Assets, Asset Filters, and Streaming Locators; read-only access to other Media Services resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/532bc159-b25e-42c0-969e-a1d439f60d77",
  "name": "532bc159-b25e-42c0-969e-a1d439f60d77",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/*",
        "Microsoft.Media/mediaservices/assets/assetfilters/*",
        "Microsoft.Media/mediaservices/streamingLocators/*",
        "Microsoft.Media/mediaservices/liveEvents/*"
      ],
      "notActions": [
        "Microsoft.Media/mediaservices/assets/getEncryptionKey/action",
        "Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Live Events Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-media-operator"></a>Operador multimedia de Media Services

Crear, leer, modificar y eliminar recursos, filtros de recursos, localizadores de streaming y trabajos; acceso de solo lectura a otros recursos de Media Services.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Lee definiciones de métricas |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/assetfilters/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/transforms/jobs/* |  |
> | **NotActions** |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/getEncryptionKey/action | Obtiene una clave de cifrado de recursos |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listContentKeys/action | Enumera las claves de contenido |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Assets, Asset Filters, Streaming Locators, and Jobs; read-only access to other Media Services resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e4395492-1534-4db2-bedf-88c14621589c",
  "name": "e4395492-1534-4db2-bedf-88c14621589c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/*",
        "Microsoft.Media/mediaservices/assets/assetfilters/*",
        "Microsoft.Media/mediaservices/streamingLocators/*",
        "Microsoft.Media/mediaservices/transforms/jobs/*"
      ],
      "notActions": [
        "Microsoft.Media/mediaservices/assets/getEncryptionKey/action",
        "Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Media Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-policy-administrator"></a>Administrador de directivas de Media Services

Crear, leer, modificar y eliminar filtros de cuenta, directivas de streaming, directivas de clave de contenido y transformaciones; acceso de solo lectura a otros recursos de Media Services. No puede crear trabajos, recursos o recursos de streaming.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Lee definiciones de métricas |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/listStreamingLocators/action | Enumera los localizadores de streaming para el recurso. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listPaths/action | Enumera las rutas |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/accountFilters/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingPolicies/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/contentKeyPolicies/* |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/transforms/* |  |
> | **NotActions** |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Obtiene las propiedades de una directiva con secretos |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Account Filters, Streaming Policies, Content Key Policies, and Transforms; read-only access to other Media Services resources. Cannot create Jobs, Assets or Streaming resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c4bba371-dacd-4a26-b320-7250bca963ae",
  "name": "c4bba371-dacd-4a26-b320-7250bca963ae",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/listStreamingLocators/action",
        "Microsoft.Media/mediaservices/streamingLocators/listPaths/action",
        "Microsoft.Media/mediaservices/accountFilters/*",
        "Microsoft.Media/mediaservices/streamingPolicies/*",
        "Microsoft.Media/mediaservices/contentKeyPolicies/*",
        "Microsoft.Media/mediaservices/transforms/*"
      ],
      "notActions": [
        "Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Policy Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="media-services-streaming-endpoints-administrator"></a>Administrador de puntos de conexión de streaming de Media Services

Crear, leer, modificar y eliminar puntos de conexión de streaming; acceso de solo lectura a otros recursos de Media Services.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Lee definiciones de métricas |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/*/read |  |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/assets/listStreamingLocators/action | Enumera los localizadores de streaming para el recurso. |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingLocators/listPaths/action | Enumera las rutas |
> | [Microsoft.Media](resource-provider-operations.md#microsoftmedia)/mediaservices/streamingEndpoints/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, read, modify, and delete Streaming Endpoints; read-only access to other Media Services resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/99dba123-b5fe-44d5-874c-ced7199a5804",
  "name": "99dba123-b5fe-44d5-874c-ced7199a5804",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Media/mediaservices/*/read",
        "Microsoft.Media/mediaservices/assets/listStreamingLocators/action",
        "Microsoft.Media/mediaservices/streamingLocators/listPaths/action",
        "Microsoft.Media/mediaservices/streamingEndpoints/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Media Services Streaming Endpoints Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-index-data-contributor"></a>Colaborador de datos de índice de búsqueda

Concede acceso total a los datos de índice de Azure Cognitive Search.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Search](resource-provider-operations.md#microsoftsearch)/searchServices/indexes/documents/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants full access to Azure Cognitive Search index data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8ebe5a00-799e-43f5-93ac-243d3dce84a7",
  "name": "8ebe5a00-799e-43f5-93ac-243d3dce84a7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Search/searchServices/indexes/documents/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Search Index Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-index-data-reader"></a>Lector de datos de índice de búsqueda

Concede acceso de lectura a los datos de índice de Azure Cognitive Search.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Search](resource-provider-operations.md#microsoftsearch)/searchServices/indexes/documents/read | Lee documentos o los términos de consulta sugeridos de un índice. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants read access to Azure Cognitive Search index data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1407120a-92aa-4202-b7e9-c0e197c71c8f",
  "name": "1407120a-92aa-4202-b7e9-c0e197c71c8f",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Search/searchServices/indexes/documents/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Search Index Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-service-contributor"></a>Colaborador del servicio de búsqueda

Permite administrar los servicios de Búsqueda, pero no acceder a ellos. [Más información](../search/search-security-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Search](resource-provider-operations.md#microsoftsearch)/searchServices/* | Crear y administrar servicios de búsqueda |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Search services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "name": "7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Search/searchServices/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Search Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-accesskey-reader"></a>Lector AccessKey de SignalR

Lee las claves de acceso de SignalR Service.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/*/read |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/listkeys/action | Visualiza el valor de las claves de acceso de SignalR en el portal de administración o mediante de la API. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read SignalR Service Access Keys",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/04165923-9d83-45d5-8227-78b77b0a687e",
  "name": "04165923-9d83-45d5-8227-78b77b0a687e",
  "permissions": [
    {
      "actions": [
        "Microsoft.SignalRService/*/read",
        "Microsoft.SignalRService/SignalR/listkeys/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR AccessKey Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-app-server-preview"></a>Servidor de aplicaciones de SignalR (versión preliminar)

Permite que el servidor de aplicaciones acceda al servicio SignalR con opciones de autenticación de AAD.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/auth/accessKey/action | Genere una clave de acceso para firmar tokens de acceso; la clave expirará después de 90 minutos de manera predeterminada. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/serverConnection/write | Inicia una conexión de servidor. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets your app server access SignalR Service with AAD auth options.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/420fcaa2-552c-430f-98ca-3264be4806c7",
  "name": "420fcaa2-552c-430f-98ca-3264be4806c7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.SignalRService/SignalR/auth/accessKey/action",
        "Microsoft.SignalRService/SignalR/serverConnection/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR App Server (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-rest-api-owner"></a>Propietario de la API REST de SignalR

Acceso completo a las API REST de Azure SignalR Service.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/auth/clientToken/action | Genere un AccessToken para que el cliente se conecte a ASRS, el token expirará en 5 minutos de manera predeterminada. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/hub/send/action | Difunde mensajes a todas las conexiones de cliente en el concentrador. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/send/action | Difunde el mensaje al grupo. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/read | Comprueba la existencia del grupo o la existencia del usuario en el grupo. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/write | Se une a grupos o los abandona. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/send/action | Envía mensajes directamente a una conexión de cliente. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/read | Comprueba la existencia de la conexión de cliente. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/write | Cierra la conexión de cliente. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/send/action | Envía mensajes a un usuario, que puede tener varias conexiones de cliente. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/read | Comprueba la existencia del usuario. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/write | Modifica un usuario. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access to Azure SignalR Service REST APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fd53cd77-2268-407a-8f46-7e7863d0f521",
  "name": "fd53cd77-2268-407a-8f46-7e7863d0f521",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.SignalRService/SignalR/auth/clientToken/action",
        "Microsoft.SignalRService/SignalR/hub/send/action",
        "Microsoft.SignalRService/SignalR/group/send/action",
        "Microsoft.SignalRService/SignalR/group/read",
        "Microsoft.SignalRService/SignalR/group/write",
        "Microsoft.SignalRService/SignalR/clientConnection/send/action",
        "Microsoft.SignalRService/SignalR/clientConnection/read",
        "Microsoft.SignalRService/SignalR/clientConnection/write",
        "Microsoft.SignalRService/SignalR/user/send/action",
        "Microsoft.SignalRService/SignalR/user/read",
        "Microsoft.SignalRService/SignalR/user/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR REST API Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-rest-api-reader"></a>Lector de la API REST de SignalR

Acceso de solo lectura a las API REST de Azure SignalR Service.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/read | Comprueba la existencia del grupo o la existencia del usuario en el grupo. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/read | Comprueba la existencia de la conexión de cliente. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/read | Comprueba la existencia del usuario. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only access to Azure SignalR Service REST APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ddde6b66-c0df-4114-a159-3618637b3035",
  "name": "ddde6b66-c0df-4114-a159-3618637b3035",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.SignalRService/SignalR/group/read",
        "Microsoft.SignalRService/SignalR/clientConnection/read",
        "Microsoft.SignalRService/SignalR/user/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR REST API Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalr-service-owner"></a>Propietario de SignalR Service

Acceso completo a las API REST de Azure SignalR Service.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/auth/accessKey/action | Genere una clave de acceso para firmar tokens de acceso; la clave expirará después de 90 minutos de manera predeterminada. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/auth/clientToken/action | Genere un AccessToken para que el cliente se conecte a ASRS, el token expirará en 5 minutos de manera predeterminada. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/hub/send/action | Difunde mensajes a todas las conexiones de cliente en el concentrador. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/send/action | Difunde el mensaje al grupo. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/read | Comprueba la existencia del grupo o la existencia del usuario en el grupo. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/group/write | Se une a grupos o los abandona. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/send/action | Envía mensajes directamente a una conexión de cliente. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/read | Comprueba la existencia de la conexión de cliente. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/clientConnection/write | Cierra la conexión de cliente. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/serverConnection/write | Inicia una conexión de servidor. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/send/action | Envía mensajes a un usuario, que puede tener varias conexiones de cliente. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/read | Comprueba la existencia del usuario. |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/SignalR/user/write | Modifica un usuario. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access to Azure SignalR Service REST APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7e4f1700-ea5a-4f59-8f37-079cfe29dce3",
  "name": "7e4f1700-ea5a-4f59-8f37-079cfe29dce3",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.SignalRService/SignalR/auth/accessKey/action",
        "Microsoft.SignalRService/SignalR/auth/clientToken/action",
        "Microsoft.SignalRService/SignalR/hub/send/action",
        "Microsoft.SignalRService/SignalR/group/send/action",
        "Microsoft.SignalRService/SignalR/group/read",
        "Microsoft.SignalRService/SignalR/group/write",
        "Microsoft.SignalRService/SignalR/clientConnection/send/action",
        "Microsoft.SignalRService/SignalR/clientConnection/read",
        "Microsoft.SignalRService/SignalR/clientConnection/write",
        "Microsoft.SignalRService/SignalR/serverConnection/write",
        "Microsoft.SignalRService/SignalR/user/send/action",
        "Microsoft.SignalRService/SignalR/user/read",
        "Microsoft.SignalRService/SignalR/user/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR Service Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="signalrweb-pubsub-contributor"></a>Colaborador de SignalR y Web PubSub

Crea, lee, actualiza y elimina recursos del servicio SignalR.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.SignalRService](resource-provider-operations.md#microsoftsignalrservice)/* |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, Read, Update, and Delete SignalR service resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8cf5e20a-e4b2-4e9d-b3a1-5ceb692c2761",
  "name": "8cf5e20a-e4b2-4e9d-b3a1-5ceb692c2761",
  "permissions": [
    {
      "actions": [
        "Microsoft.SignalRService/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SignalR/Web PubSub Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="web-plan-contributor"></a>Colaborador de plan web

Administra los planes web para los sitios web. No le permite asignar roles en RBAC de Azure.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/* | Crear y administrar granjas de servidores |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/hostingEnvironments/Join/Action | Se une a App Service Environment. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the web plans for websites, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "name": "2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/serverFarms/*",
        "Microsoft.Web/hostingEnvironments/Join/Action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Web Plan Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="website-contributor"></a>Colaborador de sitio web

Administra sitios web, pero no planes web. No le permite asignar roles en RBAC de Azure.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/* | Crear y administrar componentes de Insights |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/certificates/* | Crear y administrar certificados de sitios web |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/listSitesAssignedToHostName/read | Obtiene los nombres de sitios asignados al nombre de host. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/join/action | Unirse a un plan de App Service |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/read | Obtiene las propiedades de un plan de App Service |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/sites/* | Crear y administrar sitios web (la creación de sitios también requiere permisos de escritura para el plan de App Service asociado) |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage websites (not web plans), but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/de139f84-1756-47ae-9be6-808fbbe84772",
  "name": "de139f84-1756-47ae-9be6-808fbbe84772",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/certificates/*",
        "Microsoft.Web/listSitesAssignedToHostName/read",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Website Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="containers"></a>Contenedores


### <a name="acrdelete"></a>AcrDelete

Permite eliminar repositorios, etiquetas o manifiestos de un registro de contenedor. [Más información](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/artifacts/delete | Eliminar artefacto de un registro de contenedor. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr delete",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "name": "c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/artifacts/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrDelete",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrimagesigner"></a>AcrImageSigner

Permite insertar imágenes de confianza en un registro de contenedor habilitado para la confianza en el contenido, así como extraerlas de dicho registro. [Más información](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/sign/write | Inserta o extrae metadatos de confianza en el contenido para un registro de contenedor. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/trustedCollections/write | Permite insertar o publicar colecciones de confianza de contenido del registro de contenedor. Se parece a Microsoft.ContainerRegistry/registries/sign/write, salvo que se trata de una acción de datos. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr image signer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6cef56e8-d556-48e5-a04f-b8e64114680f",
  "name": "6cef56e8-d556-48e5-a04f-b8e64114680f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/sign/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerRegistry/registries/trustedCollections/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "AcrImageSigner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpull"></a>AcrPull

Permite extraer artefactos de un registro de contenedor. [Más información](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/pull/read | Extrae u obtiene imágenes de un registro de contenedor. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr pull",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "name": "7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPull",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpush"></a>AcrPush

Permite insertar artefactos en un registro de contenedor, así como extraerlos. [Más información](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/pull/read | Extrae u obtiene imágenes de un registro de contenedor. |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/push/write | Inserta o escribe imágenes en un registro de contenedor. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr push",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8311e382-0749-4cb8-b61a-304f252e45ec",
  "name": "8311e382-0749-4cb8-b61a-304f252e45ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read",
        "Microsoft.ContainerRegistry/registries/push/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPush",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinereader"></a>AcrQuarantineReader

Permite extraer imágenes en cuarentena de un registro de contenedor. [Más información](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantine/read | Extrae u obtiene imágenes en cuarentena de un registro de contenedor |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantinedArtifacts/read | Permite extraer u obtener los artefactos en cuarentena del registro de contenedor. Se parece a Microsoft.ContainerRegistry/registries/quarantine/read, salvo que se trata de una acción de datos. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cdda3590-29a3-44f6-95f2-9f980659eb04",
  "name": "cdda3590-29a3-44f6-95f2-9f980659eb04",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerRegistry/registries/quarantinedArtifacts/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineReader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinewriter"></a>AcrQuarantineWriter

Permite insertar imágenes en cuarentena en un registro de contenedor, así como extraerlas. [Más información](../container-registry/container-registry-roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantine/read | Extrae u obtiene imágenes en cuarentena de un registro de contenedor |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantine/write | Escribe o modifica el estado de cuarentena de las imágenes que estén en cuarentena |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantinedArtifacts/read | Permite extraer u obtener los artefactos en cuarentena del registro de contenedor. Se parece a Microsoft.ContainerRegistry/registries/quarantine/read, salvo que se trata de una acción de datos. |
> | [Microsoft.ContainerRegistry](resource-provider-operations.md#microsoftcontainerregistry)/registries/quarantinedArtifacts/write | Permite escribir o actualizar el estado de cuarentena de los artefactos en cuarentena. Se parece a Microsoft.ContainerRegistry/registries/quarantine/write, salvo que se trata de una acción de datos. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data writer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "name": "c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read",
        "Microsoft.ContainerRegistry/registries/quarantine/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerRegistry/registries/quarantinedArtifacts/read",
        "Microsoft.ContainerRegistry/registries/quarantinedArtifacts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineWriter",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-admin-role"></a>Rol de administrador de clúster de Azure Kubernetes Service

Enumerar la acción de credenciales administrativas del clúster. [Más información](../aks/control-kubeconfig-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/listClusterAdminCredential/action | Muestra la credencial clusterAdmin de un clúster administrado. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/accessProfiles/listCredential/action | Obtiene el perfil de acceso de un clúster administrados por nombre de rol mediante las credenciales de la lista |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/read | Obtiene un clúster administrado |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster admin credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "name": "0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action",
        "Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action",
        "Microsoft.ContainerService/managedClusters/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster Admin Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-user-role"></a>Rol de usuario de clúster de Azure Kubernetes Service

Enumerar la acción de credenciales de usuario del clúster. [Más información](../aks/control-kubeconfig-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/listClusterUserCredential/action | Muestra la credencial clusterUser de un clúster administrado. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/read | Obtiene un clúster administrado |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster user credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "name": "4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterUserCredential/action",
        "Microsoft.ContainerService/managedClusters/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster User Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-contributor-role"></a>Rol de colaborador de Azure Kubernetes Service

Concede acceso de lectura y escritura a los clústeres de Azure Kubernetes Service [Más información](../aks/concepts-identity.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/read | Obtiene un clúster administrado |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/write | Crea un nuevo clúster administrado o actualiza uno existente. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants access to read and write Azure Kubernetes Service clusters",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ed7f3fbd-7b88-4dd4-9017-9adb7ce333f8",
  "name": "ed7f3fbd-7b88-4dd4-9017-9adb7ce333f8",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/read",
        "Microsoft.ContainerService/managedClusters/write",
        "Microsoft.Resources/deployments/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Contributor Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-rbac-admin"></a>Administrador de Azure Kubernetes Service RBAC

Permite administrar todos los recursos en un clúster o espacio de nombres, excepto actualizar o eliminar cuotas de recursos y espacios de nombres. [Más información](../aks/manage-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/listClusterUserCredential/action | Muestra la credencial clusterUser de un clúster administrado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/* |  |
> | **NotDataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/resourcequotas/write | Escribe resourcequotas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/resourcequotas/delete | Elimina resourcequotas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/namespaces/write | Escribe espacios de nombres. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/namespaces/delete | Elimina espacios de nombres. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage all resources under cluster/namespace, except update or delete resource quotas and namespaces.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3498e952-d568-435e-9b2c-8d77e338d7f7",
  "name": "3498e952-d568-435e-9b2c-8d77e338d7f7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.ContainerService/managedClusters/listClusterUserCredential/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerService/managedClusters/*"
      ],
      "notDataActions": [
        "Microsoft.ContainerService/managedClusters/resourcequotas/write",
        "Microsoft.ContainerService/managedClusters/resourcequotas/delete",
        "Microsoft.ContainerService/managedClusters/namespaces/write",
        "Microsoft.ContainerService/managedClusters/namespaces/delete"
      ]
    }
  ],
  "roleName": "Azure Kubernetes Service RBAC Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-rbac-cluster-admin"></a>Administrador de clúster de Azure Kubernetes Service RBAC

Permite administrar todos los recursos del clúster. [Más información](../aks/manage-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/listClusterUserCredential/action | Muestra la credencial clusterUser de un clúster administrado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage all resources in the cluster.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b1ff04bb-8a4e-4dc4-8eb5-8693973ce19b",
  "name": "b1ff04bb-8a4e-4dc4-8eb5-8693973ce19b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.ContainerService/managedClusters/listClusterUserCredential/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerService/managedClusters/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service RBAC Cluster Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-rbac-reader"></a>Lector de Azure Kubernetes Service RBAC

Permite el acceso de solo lectura para ver la mayoría de los objetos en un espacio de nombres. No permite la visualización de roles o enlaces de roles. Este rol no permite visualización de secretos, ya que leer el contenido de estos permite el acceso a las credenciales de ServiceAccount en el espacio de nombres, que permitiría el acceso a la API como cualquier ServiceAccount en el espacio de nombres (una forma de elevación de privilegios). Al aplicar este rol en el ámbito del clúster, se proporcionará acceso a todos los espacios de nombres. [Más información](../aks/manage-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/controllerrevisions/read | Lee controllerrevisions. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/daemonsets/read | Lee daemonsets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/deployments/read | Lee implementaciones. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/replicasets/read | Lee replicasets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/statefulsets/read | Lee statefulsets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/autoscaling/horizontalpodautoscalers/read | Lee horizontalpodautoscalers. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/batch/cronjobs/read | Lee cronjobs. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/batch/jobs/read | Lee trabajos. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/configmaps/read | Lee configmaps. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/endpoints/read | Lee puntos de conexión. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/events.k8s.io/events/read | Lee eventos. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/events/read | Lee eventos. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/daemonsets/read | Lee daemonsets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/deployments/read | Lee implementaciones. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/ingresses/read | Lee entradas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/networkpolicies/read | Lee networkpolicies. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/replicasets/read | Lee replicasets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/limitranges/read | Lee limitranges. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/namespaces/read | Lee espacios de nombres. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/networking.k8s.io/ingresses/read | Lee entradas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/networking.k8s.io/networkpolicies/read | Lee networkpolicies. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/persistentvolumeclaims/read | Lee persistentvolumeclaims. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/pods/read | Lee pods. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/policy/poddisruptionbudgets/read | Lee poddisruptionbudgets. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/replicationcontrollers/read | Lee replicationcontrollers. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/replicationcontrollers/read | Lee replicationcontrollers. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/resourcequotas/read | Lee resourcequotas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/serviceaccounts/read | Lee serviceaccounts. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/services/read | Lee servicios. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read-only access to see most objects in a namespace. It does not allow viewing roles or role bindings. This role does not allow viewing Secrets, since reading the contents of Secrets enables access to ServiceAccount credentials in the namespace, which would allow API access as any ServiceAccount in the namespace (a form of privilege escalation). Applying this role at cluster scope will give access across all namespaces.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7f6c6a51-bcf8-42ba-9220-52d62157d7db",
  "name": "7f6c6a51-bcf8-42ba-9220-52d62157d7db",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerService/managedClusters/apps/controllerrevisions/read",
        "Microsoft.ContainerService/managedClusters/apps/daemonsets/read",
        "Microsoft.ContainerService/managedClusters/apps/deployments/read",
        "Microsoft.ContainerService/managedClusters/apps/replicasets/read",
        "Microsoft.ContainerService/managedClusters/apps/statefulsets/read",
        "Microsoft.ContainerService/managedClusters/autoscaling/horizontalpodautoscalers/read",
        "Microsoft.ContainerService/managedClusters/batch/cronjobs/read",
        "Microsoft.ContainerService/managedClusters/batch/jobs/read",
        "Microsoft.ContainerService/managedClusters/configmaps/read",
        "Microsoft.ContainerService/managedClusters/endpoints/read",
        "Microsoft.ContainerService/managedClusters/events.k8s.io/events/read",
        "Microsoft.ContainerService/managedClusters/events/read",
        "Microsoft.ContainerService/managedClusters/extensions/daemonsets/read",
        "Microsoft.ContainerService/managedClusters/extensions/deployments/read",
        "Microsoft.ContainerService/managedClusters/extensions/ingresses/read",
        "Microsoft.ContainerService/managedClusters/extensions/networkpolicies/read",
        "Microsoft.ContainerService/managedClusters/extensions/replicasets/read",
        "Microsoft.ContainerService/managedClusters/limitranges/read",
        "Microsoft.ContainerService/managedClusters/namespaces/read",
        "Microsoft.ContainerService/managedClusters/networking.k8s.io/ingresses/read",
        "Microsoft.ContainerService/managedClusters/networking.k8s.io/networkpolicies/read",
        "Microsoft.ContainerService/managedClusters/persistentvolumeclaims/read",
        "Microsoft.ContainerService/managedClusters/pods/read",
        "Microsoft.ContainerService/managedClusters/policy/poddisruptionbudgets/read",
        "Microsoft.ContainerService/managedClusters/replicationcontrollers/read",
        "Microsoft.ContainerService/managedClusters/replicationcontrollers/read",
        "Microsoft.ContainerService/managedClusters/resourcequotas/read",
        "Microsoft.ContainerService/managedClusters/serviceaccounts/read",
        "Microsoft.ContainerService/managedClusters/services/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service RBAC Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-rbac-writer"></a>Escritor de Azure Kubernetes Service RBAC

Permite el acceso de lectura y escritura a la mayoría de los objetos de un espacio de nombres. Este rol no permite ver ni modificar roles ni enlaces de roles. Sin embargo, este rol permite acceder a secretos y ejecutar pods como cualquier ServiceAccount en el espacio de nombres, por lo que se puede usar para obtener los niveles de acceso de la API de cualquier ServiceAccount en el espacio de nombres. Al aplicar este rol en el ámbito del clúster, se proporcionará acceso a todos los espacios de nombres. [Más información](../aks/manage-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/controllerrevisions/read | Lee controllerrevisions. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/daemonsets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/deployments/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/replicasets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/apps/statefulsets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/autoscaling/horizontalpodautoscalers/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/batch/cronjobs/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/batch/jobs/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/configmaps/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/endpoints/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/events.k8s.io/events/read | Lee eventos. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/events/read | Lee eventos. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/daemonsets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/deployments/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/ingresses/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/networkpolicies/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/extensions/replicasets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/limitranges/read | Lee limitranges. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/namespaces/read | Lee espacios de nombres. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/networking.k8s.io/ingresses/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/networking.k8s.io/networkpolicies/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/persistentvolumeclaims/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/pods/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/policy/poddisruptionbudgets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/replicationcontrollers/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/replicationcontrollers/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/resourcequotas/read | Lee resourcequotas. |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/secrets/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/serviceaccounts/* |  |
> | [Microsoft.ContainerService](resource-provider-operations.md#microsoftcontainerservice)/managedClusters/services/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read/write access to most objects in a namespace.This role does not allow viewing or modifying roles or role bindings. However, this role allows accessing Secrets and running Pods as any ServiceAccount in the namespace, so it can be used to gain the API access levels of any ServiceAccount in the namespace. Applying this role at cluster scope will give access across all namespaces.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a7ffa36f-339b-4b5c-8bdf-e2c188b2c0eb",
  "name": "a7ffa36f-339b-4b5c-8bdf-e2c188b2c0eb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ContainerService/managedClusters/apps/controllerrevisions/read",
        "Microsoft.ContainerService/managedClusters/apps/daemonsets/*",
        "Microsoft.ContainerService/managedClusters/apps/deployments/*",
        "Microsoft.ContainerService/managedClusters/apps/replicasets/*",
        "Microsoft.ContainerService/managedClusters/apps/statefulsets/*",
        "Microsoft.ContainerService/managedClusters/autoscaling/horizontalpodautoscalers/*",
        "Microsoft.ContainerService/managedClusters/batch/cronjobs/*",
        "Microsoft.ContainerService/managedClusters/batch/jobs/*",
        "Microsoft.ContainerService/managedClusters/configmaps/*",
        "Microsoft.ContainerService/managedClusters/endpoints/*",
        "Microsoft.ContainerService/managedClusters/events.k8s.io/events/read",
        "Microsoft.ContainerService/managedClusters/events/read",
        "Microsoft.ContainerService/managedClusters/extensions/daemonsets/*",
        "Microsoft.ContainerService/managedClusters/extensions/deployments/*",
        "Microsoft.ContainerService/managedClusters/extensions/ingresses/*",
        "Microsoft.ContainerService/managedClusters/extensions/networkpolicies/*",
        "Microsoft.ContainerService/managedClusters/extensions/replicasets/*",
        "Microsoft.ContainerService/managedClusters/limitranges/read",
        "Microsoft.ContainerService/managedClusters/namespaces/read",
        "Microsoft.ContainerService/managedClusters/networking.k8s.io/ingresses/*",
        "Microsoft.ContainerService/managedClusters/networking.k8s.io/networkpolicies/*",
        "Microsoft.ContainerService/managedClusters/persistentvolumeclaims/*",
        "Microsoft.ContainerService/managedClusters/pods/*",
        "Microsoft.ContainerService/managedClusters/policy/poddisruptionbudgets/*",
        "Microsoft.ContainerService/managedClusters/replicationcontrollers/*",
        "Microsoft.ContainerService/managedClusters/replicationcontrollers/*",
        "Microsoft.ContainerService/managedClusters/resourcequotas/read",
        "Microsoft.ContainerService/managedClusters/secrets/*",
        "Microsoft.ContainerService/managedClusters/serviceaccounts/*",
        "Microsoft.ContainerService/managedClusters/services/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service RBAC Writer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="databases"></a>Bases de datos


### <a name="azure-connected-sql-server-onboarding"></a>Incorporación de SQL Server conectado a Azure

Permite el acceso de lectura y escritura a los recursos de Azure para SQL Server en servidores habilitados para Arc. [Más información](/sql/sql-server/azure-arc/connect)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | Microsoft.AzureArcData/sqlServerInstances/read | Recupera un recurso de instancia de SQL Server. |
> | Microsoft.AzureArcData/sqlServerInstances/write | Actualiza un recurso de instancia de SQL Server. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft.AzureArcData service role to access the resources of Microsoft.AzureArcData stored with RPSAAS.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e8113dce-c529-4d33-91fa-e9b972617508",
  "name": "e8113dce-c529-4d33-91fa-e9b972617508",
  "permissions": [
    {
      "actions": [
        "Microsoft.AzureArcData/sqlServerInstances/read",
        "Microsoft.AzureArcData/sqlServerInstances/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected SQL Server Onboarding",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmos-db-account-reader-role"></a>Rol de lector de cuentas de Cosmos DB

Puede leer los datos de cuentas de Azure Cosmos DB. Vea [Colaborador de cuenta de DocumentDB](#documentdb-account-contributor) para administrar cuentas de Azure Cosmos DB. [Más información](../cosmos-db/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/*/read | Leer cualquier colección |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/readonlykeys/action | Lee las claves de solo lectura de la cuenta de base de datos. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/MetricDefinitions/read | Lee definiciones de métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Metrics/read | Lee métricas |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read Azure Cosmos DB Accounts data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "name": "fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDB/*/read",
        "Microsoft.DocumentDB/databaseAccounts/readonlykeys/action",
        "Microsoft.Insights/MetricDefinitions/read",
        "Microsoft.Insights/Metrics/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Account Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmos-db-operator"></a>Operador de Cosmos DB

Permite administrar las cuentas de Azure Cosmos DB, pero no acceder a los datos que contienen. Evita el acceso a las claves de cuenta y a las cadenas de conexión. [Más información](../cosmos-db/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DocumentDb](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | **NotActions** |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/readonlyKeys/* |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/regenerateKey/* |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/listKeys/* |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/listConnectionStrings/* |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/sqlRoleDefinitions/write | Creación o actualización de una definición de roles de SQL |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/sqlRoleDefinitions/delete | Eliminación de una definición de roles de SQL |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/sqlRoleAssignments/write | Creación o actualización de una asignación de roles de SQL |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/sqlRoleAssignments/delete | Eliminación de una asignación de roles de SQL |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Cosmos DB accounts, but not access data in them. Prevents access to account keys and connection strings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/230815da-be43-4aae-9cb4-875f7bd000aa",
  "name": "230815da-be43-4aae-9cb4-875f7bd000aa",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [
        "Microsoft.DocumentDB/databaseAccounts/readonlyKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/regenerateKey/*",
        "Microsoft.DocumentDB/databaseAccounts/listKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/*",
        "Microsoft.DocumentDB/databaseAccounts/sqlRoleDefinitions/write",
        "Microsoft.DocumentDB/databaseAccounts/sqlRoleDefinitions/delete",
        "Microsoft.DocumentDB/databaseAccounts/sqlRoleAssignments/write",
        "Microsoft.DocumentDB/databaseAccounts/sqlRoleAssignments/delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmosbackupoperator"></a>CosmosBackupOperator

Puede enviar una solicitud de restauración para una base de datos de Cosmos DB o un contenedor de una cuenta. [Más información](../cosmos-db/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/backup/action | Envía una solicitud para configurar la copia de seguridad. |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/restore/action | Envía una solicitud de restauración. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can submit restore request for a Cosmos DB database or a container for an account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "name": "db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDB/databaseAccounts/backup/action",
        "Microsoft.DocumentDB/databaseAccounts/restore/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CosmosBackupOperator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmosrestoreoperator"></a>CosmosRestoreOperator

Puede realizar una acción de restauración en la cuenta de la base de datos de Cosmos DB con el modo de copia de seguridad continua

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/locations/restorableDatabaseAccounts/restore/action | Envía una solicitud de restauración. |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/locations/restorableDatabaseAccounts/*/read |  |
> | [Microsoft.DocumentDB](resource-provider-operations.md#microsoftdocumentdb)/locations/restorableDatabaseAccounts/read | Lee una cuenta de base de datos restaurable o enumera todas las cuentas de base de datos restaurables. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can perform restore action for Cosmos DB database account with continuous backup mode",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5432c526-bc82-444a-b7ba-57c5b0b5b34f",
  "name": "5432c526-bc82-444a-b7ba-57c5b0b5b34f",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restore/action",
        "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read",
        "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CosmosRestoreOperator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="documentdb-account-contributor"></a>Colaborador de cuenta de DocumentDB

Puede administrar cuentas de Azure Cosmos DB. Azure Cosmos DB se llamaba anteriormente DocumentDB. [Más información](../cosmos-db/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.DocumentDb](resource-provider-operations.md#microsoftdocumentdb)/databaseAccounts/* | Crear y administrar cuentas de Azure Cosmos DB |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DocumentDB accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5bd9cd88-fe45-4216-938b-f97437e15450",
  "name": "5bd9cd88-fe45-4216-938b-f97437e15450",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DocumentDB Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="redis-cache-contributor"></a>Colaborador de la memoria caché de Redis

Permite administrar cachés de Redis, pero no acceder a ellas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Cache](resource-provider-operations.md#microsoftcache)/register/action | Registra el proveedor de recursos "Microsoft.Cache" con una suscripción |
> | [Microsoft.Cache](resource-provider-operations.md#microsoftcache)/redis/* | Crear y administrar memorias caché de Redis |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Redis caches, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e0f68234-74aa-48ed-b826-c38b57376e17",
  "name": "e0f68234-74aa-48ed-b826-c38b57376e17",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cache/register/action",
        "Microsoft.Cache/redis/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Redis Cache Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-db-contributor"></a>Colaborador de Base de datos de SQL

Permite administrar las bases de datos de SQL, pero no acceder a ellas. Además, no puede administrar sus directivas relacionadas con la seguridad ni los servidores SQL primarios. [Más información](../data-share/concepts-roles-permissions.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/*/read |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/* | Crear y administrar bases de datos SQL |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/read | Devuelve la lista de servidores u obtiene las propiedades de un servidor específico. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Lee definiciones de métricas |
> | **NotActions** |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/ledgerDigestUploads/write | Habilita la carga de resúmenes de libros de contabilidad  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/ledgerDigestUploads/disable/action | Deshabilita la carga de resúmenes de libros de contabilidad |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditingSettings/* | Edita la configuración de auditoría |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditRecords/read | Recupera los registros de auditoría de blobs de bases de datos |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/dataMaskingPolicies/* | Edita directivas de enmascaramiento |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/extendedAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityAlertPolicies/* | Edita las directivas de alerta de seguridad |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityMetrics/* | Edita las métricas de seguridad |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentScans/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL databases, but not access to them. Also, you can't manage their security-related policies or their parent SQL servers.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "name": "9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/databases/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/servers/databases/ledgerDigestUploads/write",
        "Microsoft.Sql/servers/databases/ledgerDigestUploads/disable/action",
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL DB Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-managed-instance-contributor"></a>Colaborador de Instancia administrada de SQL

Permite administrar Instancias administradas de SQL y la configuración de red necesaria, pero no puede conceder acceso a otros usuarios.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkSecurityGroups/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/routeTables/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/*/read |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/instanceFailoverGroups/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/* |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/* |  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/* |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Lee definiciones de métricas |
> | **NotActions** |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/azureADOnlyAuthentications/delete | Elimina solo el objeto de autenticación de un servidor administrado específico de Azure Active Directory. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/azureADOnlyAuthentications/write | Agrega o actualiza solo el objeto de autenticación de un servidor administrado específico de Azure Active Directory. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL Managed Instances and required network configuration, but can't give access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "name": "4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Network/networkSecurityGroups/*",
        "Microsoft.Network/routeTables/*",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/locations/instanceFailoverGroups/*",
        "Microsoft.Sql/managedInstances/*",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/*",
        "Microsoft.Network/virtualNetworks/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/azureADOnlyAuthentications/delete",
        "Microsoft.Sql/managedInstances/azureADOnlyAuthentications/write"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Managed Instance Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-security-manager"></a>Administrador de seguridad SQL

Permite administrar las directivas relacionadas con seguridad de bases de datos y servidores SQL, pero no acceder a ellas. [Más información](../azure-sql/database/azure-defender-for-sql.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/joinViaServiceEndpoint/action | Combina un recurso como una cuenta de almacenamiento o una instancia de SQL Database con una subred. No genera alertas. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/administratorAzureAsyncOperation/read | Obtiene el resultado de las operaciones de administrador asincrónico de Azure de instancia administrada. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/transparentDataEncryption/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/auditingSettings/* | Crear y administrar configuración de auditoría de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/extendedAuditingSettings/read | Recupera los detalles de la directiva de auditoría de blobs del servidor extendido que está configurada en un servidor determinado. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditingSettings/* | Crear y administrar configuración de auditoría de bases de datos de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditRecords/read | Recupera los registros de auditoría de blobs de bases de datos |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/dataMaskingPolicies/* | Crear y administrar directivas de enmascaramiento de datos de bases de datos de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/extendedAuditingSettings/read | Recupera los detalles de la directiva de auditoría de blobs extendida y configurada en una base de datos determinada. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/read | Devuelve la lista de bases de datos u obtiene las propiedades de una base de datos específica. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/read | Obtiene un esquema de la base de datos. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/columns/read | Obtiene una columna de la base de datos. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/read | Obtiene una tabla de la base de datos. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityAlertPolicies/* | Crear y administrar directivas de alerta de seguridad de bases de datos de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityMetrics/* | Crear y administrar métricas de seguridad de bases de datos de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/transparentDataEncryption/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentScans/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/devOpsAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/firewallRules/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/read | Devuelve la lista de servidores u obtiene las propiedades de un servidor específico. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/securityAlertPolicies/* | Crear y administrar directivas de alerta de seguridad de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/vulnerabilityAssessments/* |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/azureADOnlyAuthentications/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/read | Devuelve la lista de instancias administradas u obtiene las propiedades de una instancia administrada específica. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/azureADOnlyAuthentications/* |  |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/sqlVulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/administrators/read | Obtiene una lista de administradores de la instancia administrada. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/administrators/read | Obtiene un objeto de administrador de Azure Active Directory específico. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the security-related policies of SQL servers and databases, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "name": "056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/administratorAzureAsyncOperation/read",
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/transparentDataEncryption/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/read",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/read",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/transparentDataEncryption/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/devOpsAuditingSettings/*",
        "Microsoft.Sql/servers/firewallRules/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*",
        "Microsoft.Support/*",
        "Microsoft.Sql/servers/azureADOnlyAuthentications/*",
        "Microsoft.Sql/managedInstances/read",
        "Microsoft.Sql/managedInstances/azureADOnlyAuthentications/*",
        "Microsoft.Security/sqlVulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/administrators/read",
        "Microsoft.Sql/servers/administrators/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Security Manager",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-server-contributor"></a>Colaborador de SQL Server

Permite administrar bases de datos y servidores SQL, pero no acceder a ellos, ni a sus directivas relacionadas con la seguridad. [Más información](../azure-sql/database/authentication-aad-configure.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/locations/*/read |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/* | Crear y administrar servidores de SQL Server |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/read | Lee definiciones de métricas |
> | **NotActions** |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/securityAlertPolicies/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/managedInstances/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/auditingSettings/* | Edita la configuración de auditoría de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditingSettings/* | Edita la configuración de auditoría de bases de datos de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/auditRecords/read | Recupera los registros de auditoría de blobs de bases de datos |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/currentSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/dataMaskingPolicies/* | Edita las directivas de enmascaramiento de datos de bases de datos de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/extendedAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/recommendedSensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityAlertPolicies/* | Edita las directivas de alerta de seguridad de bases de datos de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/securityMetrics/* | Edita las métricas de seguridad de bases de datos de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/sensitivityLabels/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentScans/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/devOpsAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/extendedAuditingSettings/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/securityAlertPolicies/* | Edita las directivas de alerta de seguridad de SQL Server |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/vulnerabilityAssessments/* |  |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/azureADOnlyAuthentications/delete | Elimina solo el objeto de autenticación de un servidor específico de Azure Active Directory. |
> | [Microsoft.Sql](resource-provider-operations.md#microsoftsql)/servers/azureADOnlyAuthentications/write | Agrega o actualiza solo el objeto de autenticación de un servidor específico de Azure Active Directory. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL servers and databases, but not access to them, and not their security -related policies.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "name": "6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/*",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/devOpsAuditingSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/azureADOnlyAuthentications/delete",
        "Microsoft.Sql/servers/azureADOnlyAuthentications/write"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Server Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="analytics"></a>Análisis


### <a name="azure-event-hubs-data-owner"></a>Propietario de los datos de Azure Event Hubs

Concede acceso total a los recursos de Azure Event Hubs. [Más información](../event-hubs/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f526a384-b230-433a-b45c-95f59c4a2dec",
  "name": "f526a384-b230-433a-b45c-95f59c4a2dec",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-receiver"></a>Receptor de datos de Azure Event Hubs

Concede acceso de recepción a los recursos de Azure Event Hubs. [Más información](../event-hubs/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/*/eventhubs/consumergroups/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/*/receive/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows receive access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "name": "a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/consumergroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-sender"></a>Emisor de datos de Azure Event Hubs

Concede acceso de emisión a los recursos de Azure Event Hubs. [Más información](../event-hubs/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/*/eventhubs/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/*/send/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows send access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2b629674-e913-4c01-ae53-ef4638d8f975",
  "name": "2b629674-e913-4c01-ae53-ef4638d8f975",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-factory-contributor"></a>Colaborador de Factoría de datos

Crea y administra factorías de datos, así como recursos secundarios dentro de ellas. [Más información](../data-factory/concepts-roles-permissions.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.DataFactory](resource-provider-operations.md#microsoftdatafactory)/dataFactories/* | Crear y administrar factorías de datos y recursos secundarios dentro de ellos. |
> | [Microsoft.DataFactory](resource-provider-operations.md#microsoftdatafactory)/factories/* | Crear y administrar factorías de datos y recursos secundarios dentro de ellos. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/write | Crea o actualiza una suscripción a eventos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and manage data factories, as well as child resources within them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/673868aa-7521-48a0-acc6-0f60742d39f5",
  "name": "673868aa-7521-48a0-acc6-0f60742d39f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DataFactory/dataFactories/*",
        "Microsoft.DataFactory/factories/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.EventGrid/eventSubscriptions/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Factory Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-purger"></a>Purgador de datos

Permite eliminar datos privados de un área de trabajo de Log Analytics. [Más información](../azure-monitor/logs/personal-data-mgmt.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/purge/action | Purga datos de Application Insights. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Consulta datos de Log Analytics. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/purge/action | Elimina los datos especificados del área de trabajo. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can purge analytics data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "name": "150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/components/*/read",
        "Microsoft.Insights/components/purge/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/purge/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Purger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-cluster-operator"></a>Operador de clústeres de HDInsight

Permite leer y modificar las configuraciones de clúster de HDInsight. [Más información](../hdinsight/hdinsight-migrate-granular-access-cluster-configurations.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.HDInsight](resource-provider-operations.md#microsofthdinsight)/*/read |  |
> | [Microsoft.HDInsight](resource-provider-operations.md#microsofthdinsight)/clusters/getGatewaySettings/action | Obtiene la configuración de puerta de enlace para el clúster de HDInsight. |
> | [Microsoft.HDInsight](resource-provider-operations.md#microsofthdinsight)/clusters/updateGatewaySettings/action | Actualiza la configuración de puerta de enlace para el clúster de HDInsight. |
> | [Microsoft.HDInsight](resource-provider-operations.md#microsofthdinsight)/clusters/configurations/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and modify HDInsight cluster configurations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/61ed4efc-fab3-44fd-b111-e24485cc132a",
  "name": "61ed4efc-fab3-44fd-b111-e24485cc132a",
  "permissions": [
    {
      "actions": [
        "Microsoft.HDInsight/*/read",
        "Microsoft.HDInsight/clusters/getGatewaySettings/action",
        "Microsoft.HDInsight/clusters/updateGatewaySettings/action",
        "Microsoft.HDInsight/clusters/configurations/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Cluster Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-domain-services-contributor"></a>Colaborador de Domain Services para HDInsight

Puede leer, crear, modificar y eliminar operaciones relacionadas con Domain Services para HDInsight Enterprise Security Package. [Más información](../hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.AAD](resource-provider-operations.md#microsoftaad)/*/read |  |
> | [Microsoft.AAD](resource-provider-operations.md#microsoftaad)/domainServices/*/read |  |
> | [Microsoft.AAD](resource-provider-operations.md#microsoftaad)/domainServices/oucontainer/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can Read, Create, Modify and Delete Domain Services related operations needed for HDInsight Enterprise Security Package",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "name": "8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "permissions": [
    {
      "actions": [
        "Microsoft.AAD/*/read",
        "Microsoft.AAD/domainServices/*/read",
        "Microsoft.AAD/domainServices/oucontainer/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Domain Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-contributor"></a>Colaborador de Log Analytics

Un colaborador de Log Analytics puede leer todos los datos de supervisión y editar la configuración de supervisión. La edición de la configuración de supervisión incluye la posibilidad de agregar la extensión de máquina virtual a las máquinas virtuales, leer las claves de las cuentas de almacenamiento para poder configurar la recopilación de registros de Azure Storage, agregar soluciones y configurar Azure Diagnostics en todos los recursos de Azure. [Más información](../azure-monitor/logs/manage-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/virtualMachines/extensions/* |  |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/listKeys/action | Enumera las claves de acceso de las cuentas de almacenamiento. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/extensions/* |  |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/extensions/write | Instala o actualiza las extensiones de Azure Arc. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Crea, actualiza o lee la configuración de diagnóstico de Analysis Server. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/* |  |
> | [Microsoft.OperationsManagement](resource-provider-operations.md#microsoftoperationsmanagement)/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourcegroups/deployments/* |  |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listKeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Contributor can read all monitoring data and edit monitoring settings. Editing monitoring settings includes adding the VM extension to VMs; reading storage account keys to be able to configure collection of logs from Azure Storage; adding solutions; and configuring Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "name": "92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.ClassicCompute/virtualMachines/extensions/*",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.Compute/virtualMachines/extensions/*",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.OperationalInsights/*",
        "Microsoft.OperationsManagement/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-reader"></a>Lector de Log Analytics

Un lector de Log Analytics puede ver y buscar todos los datos de supervisión, así como consultar la configuración de supervisión, incluida la de Azure Diagnostics en todos los recursos de Azure. [Más información](../azure-monitor/logs/manage-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/analytics/query/action | Realiza búsquedas mediante el nuevo motor. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/search/action | Ejecuta una consulta de búsqueda |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/sharedKeys/read | Recupera las claves compartidas del área de trabajo. Estas claves se utilizan para conectar los agentes de Microsoft Operational Insights al área de trabajo. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Reader can view and search all monitoring data as well as and view monitoring settings, including viewing the configuration of Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/73c42c96-874c-492b-b04d-ab87d138a893",
  "name": "73c42c96-874c-492b-b04d-ab87d138a893",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.OperationalInsights/workspaces/sharedKeys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="purview-data-curator-legacy"></a>Conservador de datos de Purview (heredado)

El conservador de datos de Microsoft.Purview es un rol heredado puede crear, leer, modificar y eliminar objetos de datos del catálogo y establecer relaciones entre objetos. Este rol ha entrado en desuso recientemente en el acceso basado en roles de Azure y se ha introducido un nuevo conservador de datos en el plano de datos de Azure Purview. Consulte [Control de acceso en Azure Purview: roles](../purview/catalog-permissions.md#roles).

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/read | Lea el recurso de cuenta del proveedor de Microsoft Purview. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/data/read | Lea objetos de datos. |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/data/write | Cree, actualice y elimine objetos de datos. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "The Microsoft.Purview data curator is a legacy role that can create, read, modify and delete catalog data objects and establish relationships between objects. We have recently deprecated this role from Azure role-based access and introduced a new data curator inside Azure Purview data plane. See https://docs.microsoft.com/azure/purview/catalog-permissions#roles",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8a3c2885-9b38-4fd2-9d99-91af537c1347",
  "name": "8a3c2885-9b38-4fd2-9d99-91af537c1347",
  "permissions": [
    {
      "actions": [
        "Microsoft.Purview/accounts/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Purview/accounts/data/read",
        "Microsoft.Purview/accounts/data/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Purview Data Curator (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="purview-data-reader-legacy"></a>Lector de datos de Purview (heredado)

El lector de datos de Microsoft.Purview es un rol heredado que puede leer objetos de datos del catálogo. Este rol ha entrado en desuso recientemente en el acceso basado en roles de Azure y se ha introducido un nuevo lector de datos en el plano de datos de Azure Purview. Consulte [Control de acceso en Azure Purview: roles](../purview/catalog-permissions.md#roles).

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/read | Lea el recurso de cuenta del proveedor de Microsoft Purview. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/data/read | Lea objetos de datos. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "The Microsoft.Purview data reader is a legacy role that can read catalog data objects. We have recently deprecated this role from Azure role-based access and introduced a new data reader inside Azure Purview data plane. See https://docs.microsoft.com/azure/purview/catalog-permissions#roles",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ff100721-1b9d-43d8-af52-42b69c1272db",
  "name": "ff100721-1b9d-43d8-af52-42b69c1272db",
  "permissions": [
    {
      "actions": [
        "Microsoft.Purview/accounts/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Purview/accounts/data/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Purview Data Reader (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="purview-data-source-administrator-legacy"></a>Administrador de orígenes de datos de Purview (heredado)

El administrador de orígenes de datos de Microsoft.Purview es un rol heredado que puede administrar orígenes de datos y exámenes de datos. Este rol ha entrado en desuso recientemente en el acceso basado en roles de Azure y se ha introducido un nuevo administrador de orígenes de datos en el plano de datos de Azure Purview. Consulte [Control de acceso en Azure Purview: roles](../purview/catalog-permissions.md#roles).

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/read | Lea el recurso de cuenta del proveedor de Microsoft Purview. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/scan/read | Lea análisis y orígenes de datos. |
> | [Microsoft.Purview](resource-provider-operations.md#microsoftpurview)/accounts/scan/write | Cree, actualice y elimine orígenes de datos, y administre exámenes. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "The Microsoft.Purview data source administrator is a legacy role that can manage data sources and data scans. We have recently deprecated this role from Azure role-based access and introduced a new data source admin inside Azure Purview data plane. See https://docs.microsoft.com/azure/purview/catalog-permissions#roles",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/200bba9e-f0c8-430f-892b-6f0794863803",
  "name": "200bba9e-f0c8-430f-892b-6f0794863803",
  "permissions": [
    {
      "actions": [
        "Microsoft.Purview/accounts/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Purview/accounts/scan/read",
        "Microsoft.Purview/accounts/scan/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Purview Data Source Administrator (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="schema-registry-contributor-preview"></a>Colaborador del registro de esquemas (versión preliminar)

Leer, escribir y eliminar esquemas y grupos del registro de esquemas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/namespaces/schemagroups/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/namespaces/schemas/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read, write, and delete Schema Registry groups and schemas.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5dffeca3-4936-4216-b2bc-10343a5abb25",
  "name": "5dffeca3-4936-4216-b2bc-10343a5abb25",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/namespaces/schemagroups/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/namespaces/schemas/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Schema Registry Contributor (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="schema-registry-reader-preview"></a>Lector del registro de esquemas (versión preliminar)

Leer y enumerar grupos y esquemas del registro de esquemas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/namespaces/schemagroups/read | Obtiene una lista de descripciones de recursos de SchemaGroup |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.EventHub](resource-provider-operations.md#microsofteventhub)/namespaces/schemas/read | Recuperación de esquemas |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read and list Schema Registry groups and schemas.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2c56ea50-c6b3-40a6-83c0-9d98858bc7d2",
  "name": "2c56ea50-c6b3-40a6-83c0-9d98858bc7d2",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/namespaces/schemagroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/namespaces/schemas/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Schema Registry Reader (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="blockchain"></a>Blockchain


### <a name="blockchain-member-node-access-preview"></a>Acceso al nodo de miembro de la cadena de bloques (versión preliminar)

Permite acceder a los nodos de miembro de la cadena de bloques. [Más información](../blockchain/service/configure-aad.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Blockchain](resource-provider-operations.md#microsoftblockchain)/blockchainMembers/transactionNodes/read | Obtiene o enumera los nodos de transacción de miembro de la cadena de bloques existentes. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Blockchain](resource-provider-operations.md#microsoftblockchain)/blockchainMembers/transactionNodes/connect/action | Se conecta a un nodo de transacción de miembro de la cadena de bloques. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for access to Blockchain Member nodes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "name": "31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "permissions": [
    {
      "actions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Blockchain Member Node Access (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="ai--machine-learning"></a>Inteligencia artificial y aprendizaje automático


### <a name="azureml-data-scientist"></a>Científico de datos de AzureML

Puede realizar todas las acciones dentro de un área de trabajo de Azure Machine Learning, excepto crear o eliminar recursos de proceso, y modificar la propia área de trabajo.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/*/read |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/*/action |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/*/delete |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/*/write |  |
> | **NotActions** |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/delete | Elimina las áreas de trabajo de Machine Learning Services |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/write | Crea o actualiza las áreas de trabajo de Machine Learning Services |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/computes/*/write |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/computes/*/delete |  |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/computes/listKeys/action | Enumera los secretos de los recursos de proceso en el área de trabajo de Machine Learning Services |
> | [Microsoft.MachineLearningServices](resource-provider-operations.md#microsoftmachinelearningservices)/workspaces/listKeys/action | Enumera los secretos un área de trabajo de Machine Learning Services |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can perform all actions within an Azure Machine Learning workspace, except for creating or deleting compute resources and modifying the workspace itself.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f6c7c914-8db3-469d-8ca1-694a8f32e121",
  "name": "f6c7c914-8db3-469d-8ca1-694a8f32e121",
  "permissions": [
    {
      "actions": [
        "Microsoft.MachineLearningServices/workspaces/*/read",
        "Microsoft.MachineLearningServices/workspaces/*/action",
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/*/write"
      ],
      "notActions": [
        "Microsoft.MachineLearningServices/workspaces/delete",
        "Microsoft.MachineLearningServices/workspaces/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/listKeys/action",
        "Microsoft.MachineLearningServices/workspaces/listKeys/action"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AzureML Data Scientist",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-contributor"></a>Colaborador de Cognitive Services

Le permite crear, leer, actualizar, eliminar y administrar las claves de Cognitive Services. [Más información](../cognitive-services/cognitive-services-virtual-networks.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/* |  |
> | [Microsoft.Features](resource-provider-operations.md#microsoftfeatures)/features/read | Obtiene las características de una suscripción. |
> | [Microsoft.Features](resource-provider-operations.md#microsoftfeatures)/providers/features/read | Obtiene la característica de una suscripción de un proveedor de recursos determinado. |
> | [Microsoft.Features](resource-provider-operations.md#microsoftfeatures)/providers/features/register/action | Registra la característica de una suscripción de un proveedor de recursos determinado. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Crea, actualiza o lee la configuración de diagnóstico de Analysis Server. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/logDefinitions/read | Lee definiciones de registro |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricdefinitions/read | Lee definiciones de métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourcegroups/deployments/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create, read, update, delete and manage keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "name": "25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.CognitiveServices/*",
        "Microsoft.Features/features/read",
        "Microsoft.Features/providers/features/read",
        "Microsoft.Features/providers/features/register/action",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-contributor"></a>Colaborador de Custom Vision de Cognitive Services

Acceso completo al proyecto, lo que incluye la capacidad para ver, crear, editar o eliminar proyectos. [Más información](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access to the project, including the ability to view, create, edit, or delete projects.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c1ff6cc2-c111-46fe-8896-e0ef812ad9f3",
  "name": "c1ff6cc2-c111-46fe-8896-e0ef812ad9f3",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Custom Vision Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-deployment"></a>Implementación de Custom Vision de Cognitive Services

Publicar, anular publicaciones o exportar modelos. La implementación puede ver el proyecto pero no puede actualizarlo. [Más información](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/*/read |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/predictions/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/iterations/publish/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/iterations/export/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/quicktest/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/classify/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/detect/* |  |
> | **NotDataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/export/read | Exporta un proyecto. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Publish, unpublish or export models. Deployment can view the project but can't update.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5c4089e1-6d96-4d2f-b296-c1bc7137275f",
  "name": "5c4089e1-6d96-4d2f-b296-c1bc7137275f",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*/read",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/predictions/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/iterations/publish/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/iterations/export/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/quicktest/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/classify/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/detect/*"
      ],
      "notDataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/export/read"
      ]
    }
  ],
  "roleName": "Cognitive Services Custom Vision Deployment",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-labeler"></a>Etiquetador de Custom Vision de Cognitive Services

Ver y editar imágenes de entrenamiento, además de crear, agregar, quitar o eliminar etiquetas de imágenes. Los etiquetadores pueden ver el proyecto, pero no pueden actualizar nada más que las imágenes y etiquetas de entrenamiento. [Más información](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/*/read |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/predictions/query/action | Obtiene las imágenes que se enviaron al punto de conexión de predicción. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/images/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/tags/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/images/suggested/* |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/tagsandregions/suggestions/action | Esta API obtendrá etiquetas y regiones sugeridas para una matriz o lote de imágenes sin etiquetar, junto con las confianzas de las etiquetas. Devuelve una matriz vacía si no se encuentra ninguna etiqueta. |
> | **NotDataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/export/read | Exporta un proyecto. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View, edit training images and create, add, remove, or delete the image tags. Labelers can view the project but can't update anything other than training images and tags.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/88424f51-ebe7-446f-bc41-7fa16989e96c",
  "name": "88424f51-ebe7-446f-bc41-7fa16989e96c",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*/read",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/predictions/query/action",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/images/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/tags/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/images/suggested/*",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/tagsandregions/suggestions/action"
      ],
      "notDataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/export/read"
      ]
    }
  ],
  "roleName": "Cognitive Services Custom Vision Labeler",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-reader"></a>Lector de Custom Vision de Cognitive Services

Acciones de solo lectura en el proyecto. Los lectores no pueden crear ni actualizar el proyecto. [Más información](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/*/read |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/predictions/query/action | Obtiene las imágenes que se enviaron al punto de conexión de predicción. |
> | **NotDataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/export/read | Exporta un proyecto. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only actions in the project. Readers can't create or update the project.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/93586559-c37d-4a6b-ba08-b9f0940c2d73",
  "name": "93586559-c37d-4a6b-ba08-b9f0940c2d73",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*/read",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/predictions/query/action"
      ],
      "notDataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/export/read"
      ]
    }
  ],
  "roleName": "Cognitive Services Custom Vision Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-custom-vision-trainer"></a>Entrenador de Custom Vision de Cognitive Services

Ver, editar proyectos y entrenar los modelos, lo que incluye la capacidad de publicar, anular la publicación y exportar los modelos. Los entrenadores no pueden crear ni eliminar el proyecto. [Más información](../cognitive-services/custom-vision-service/role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/* |  |
> | **NotDataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/action | Crear un proyecto. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/delete | Elimina un proyecto específico. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/import/action | Importa un proyecto. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/CustomVision/projects/export/read | Exporta un proyecto. |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View, edit projects and train the models, including the ability to publish, unpublish, export the models. Trainers can't create or delete the project.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0a5ae4ab-0d65-4eeb-be61-29fc9b54394b",
  "name": "0a5ae4ab-0d65-4eeb-be61-29fc9b54394b",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/*"
      ],
      "notDataActions": [
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/action",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/delete",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/import/action",
        "Microsoft.CognitiveServices/accounts/CustomVision/projects/export/read"
      ]
    }
  ],
  "roleName": "Cognitive Services Custom Vision Trainer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-data-reader-preview"></a>Lector de datos de Cognitive Services (versión preliminar)

Permite leer los datos de Cognitive Services.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read Cognitive Services data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "name": "b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Data Reader (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-face-recognizer"></a>Cognitive Services Face Recognizer

Permite detectar, comprobar, identificar, agrupar y buscar operaciones similares en Face API. Este rol no permite operaciones de creación o eliminación, de modo que es adecuado para los puntos de conexión que solo necesitan funcionalidades de inferencia, según los procedimientos recomendados de "privilegios mínimos".

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/detect/action | Detecta caras humanas en una imagen, devuelve rectángulos de caras, opcionalmente con identificadores de cara, puntos de referencia y atributos. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/verify/action | Comprueba si dos caras pertenecen a una misma persona o si una cara pertenece a una persona. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/identify/action | Identificación de 1 a varios para encontrar las coincidencias más cercanas de la cara de la persona de la consulta específica a partir de un grupo de personas o grupo de personas de gran tamaño. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/group/action | Divide las caras de los candidatos en grupos en función de la similitud de la cara. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/Face/findsimilars/action | Dado el elemento faceId de un cara de la consulta, busca las caras similares en una matriz de elementos faceId, una lista de caras o una lista de caras de gran tamaño. faceId |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you perform detect, verify, identify, group, and find similar operations on Face API. This role does not allow create or delete operations, which makes it well suited for endpoints that only need inferencing capabilities, following 'least privilege' best practices.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9894cab4-e18a-44aa-828b-cb588cd6f2d7",
  "name": "9894cab4-e18a-44aa-828b-cb588cd6f2d7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/Face/detect/action",
        "Microsoft.CognitiveServices/accounts/Face/verify/action",
        "Microsoft.CognitiveServices/accounts/Face/identify/action",
        "Microsoft.CognitiveServices/accounts/Face/group/action",
        "Microsoft.CognitiveServices/accounts/Face/findsimilars/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Face Recognizer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-metrics-advisor-administrator"></a>Administrador de Metrics Advisor de Cognitive Services

Acceso total al proyecto, lo que incluye la configuración del nivel de sistema. [Más información](../applied-ai-services/metrics-advisor/how-tos/alerts.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/MetricsAdvisor/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access to the project, including the system level configuration.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cb43c632-a144-4ec5-977c-e80c4affc34a",
  "name": "cb43c632-a144-4ec5-977c-e80c4affc34a",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/MetricsAdvisor/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Metrics Advisor Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-qna-maker-editor"></a>Editor de QnA Maker de Cognitive Services

Permite crear, editar, importar y exportar un knowledge base. No se puede publicar ni eliminar un knowledge base. [Más información](../cognitive-services/qnamaker/reference-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleAssignments/read | Obtiene información sobre una asignación de roles. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleDefinitions/read | Obtiene información sobre una definición de roles. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/read | Obtiene una lista de las bases de conocimientos o detalles de una base de conocimiento específica. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/download/read | Descarga la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/create/write | Operación asincrónica para crear una nueva base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/write | Operación asincrónica para modificar una base de conocimiento o reemplazar el contenido de la bases de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/generateanswer/action | Llamada a GenerateAnswer para consultar la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/train/action | Llama a Train para agregar sugerencias a la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/alterations/read | Descarga las modificaciones en el entorno de tiempo de ejecución. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/alterations/write | Reemplaza los datos de las modificaciones. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointkeys/read | Obtiene las claves de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointkeys/refreshkeys/action | Vuelve a generar una clave de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointsettings/read | Obtiene la configuración de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointsettings/write | Actualiza la configuración de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/operations/read | Obtiene los detalles de una operación de ejecución prolongada específica. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/read | Obtiene una lista de las bases de conocimientos o detalles de una base de conocimiento específica. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/download/read | Descarga la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/create/write | Operación asincrónica para crear una nueva base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/write | Operación asincrónica para modificar una base de conocimiento o reemplazar el contenido de la bases de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/generateanswer/action | Llamada a GenerateAnswer para consultar la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/train/action | Llama a Train para agregar sugerencias a la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/alterations/read | Descarga las modificaciones en el entorno de tiempo de ejecución. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/alterations/write | Reemplaza los datos de las modificaciones. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointkeys/read | Obtiene las claves de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointkeys/refreshkeys/action | Vuelve a generar una clave de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointsettings/read | Obtiene la configuración de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointsettings/write | Actualiza la configuración de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/operations/read | Obtiene los detalles de una operación de ejecución prolongada específica. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/read | Obtiene una lista de las bases de conocimientos o detalles de una base de conocimiento específica. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/download/read | Descarga la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/create/write | Operación asincrónica para crear una nueva base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/write | Operación asincrónica para modificar una base de conocimiento o reemplazar el contenido de la bases de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/generateanswer/action | Llamada a GenerateAnswer para consultar la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/train/action | Llama a Train para agregar sugerencias a la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/alterations/read | Descarga las modificaciones en el entorno de tiempo de ejecución. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/alterations/write | Reemplaza los datos de las modificaciones. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointkeys/read | Obtiene las claves de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointkeys/refreshkeys/action | Vuelve a generar una clave de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointsettings/read | Obtiene la configuración de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointsettings/write | Actualiza la configuración de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/operations/read | Obtiene los detalles de una operación de ejecución prolongada específica. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Let's you create, edit, import and export a KB. You cannot publish or delete a KB.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f4cc2bf9-21be-47a1-bdf1-5c5804381025",
  "name": "f4cc2bf9-21be-47a1-bdf1-5c5804381025",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read",
        "Microsoft.Authorization/roleAssignments/read",
        "Microsoft.Authorization/roleDefinitions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/create/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/train/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker/alterations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/alterations/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointkeys/refreshkeys/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointsettings/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker/operations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/create/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/train/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/alterations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/alterations/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointkeys/refreshkeys/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointsettings/write",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/operations/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/create/write",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/write",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/train/action",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/alterations/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/alterations/write",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointkeys/refreshkeys/action",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointsettings/write",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/operations/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services QnA Maker Editor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-qna-maker-reader"></a>Lector de QnA Maker de Cognitive Services

Permite leer y probar solo un knowledge base. [Más información](../cognitive-services/qnamaker/reference-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleAssignments/read | Obtiene información sobre una asignación de roles. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleDefinitions/read | Obtiene información sobre una definición de roles. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/read | Obtiene una lista de las bases de conocimientos o detalles de una base de conocimiento específica. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/download/read | Descarga la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/knowledgebases/generateanswer/action | Llamada a GenerateAnswer para consultar la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/alterations/read | Descarga las modificaciones en el entorno de tiempo de ejecución. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointkeys/read | Obtiene las claves de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker/endpointsettings/read | Obtiene la configuración de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/read | Obtiene una lista de las bases de conocimientos o detalles de una base de conocimiento específica. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/download/read | Descarga la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/knowledgebases/generateanswer/action | Llamada a GenerateAnswer para consultar la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/alterations/read | Descarga las modificaciones en el entorno de tiempo de ejecución. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointkeys/read | Obtiene las claves de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/QnAMaker.v2/endpointsettings/read | Obtiene la configuración de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/read | Obtiene una lista de las bases de conocimientos o detalles de una base de conocimiento específica. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/download/read | Descarga la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/knowledgebases/generateanswer/action | Llamada a GenerateAnswer para consultar la base de conocimiento. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/alterations/read | Descarga las modificaciones en el entorno de tiempo de ejecución. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointkeys/read | Obtiene las claves de punto de conexión de un punto de conexión. |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/TextAnalytics/QnAMaker/endpointsettings/read | Obtiene la configuración de punto de conexión de un punto de conexión. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Let's you read and test a KB only.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/466ccd10-b268-4a11-b098-b4849f024126",
  "name": "466ccd10-b268-4a11-b098-b4849f024126",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read",
        "Microsoft.Authorization/roleAssignments/read",
        "Microsoft.Authorization/roleDefinitions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker/alterations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/alterations/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/QnAMaker.v2/endpointsettings/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/download/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/knowledgebases/generateanswer/action",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/alterations/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointkeys/read",
        "Microsoft.CognitiveServices/accounts/TextAnalytics/QnAMaker/endpointsettings/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services QnA Maker Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-user"></a>Usuario de Cognitive Services

Le permite leer y mostrar las claves de Cognitive Services. [Más información](../cognitive-services/authentication.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/*/read |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/accounts/listkeys/action | Enumeración de claves |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Lee una alerta de métrica clásica. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/read | Lee la configuración de diagnóstico de un recurso. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/logDefinitions/read | Lee definiciones de registro |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricdefinitions/read | Lee definiciones de métricas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metrics/read | Lee métricas |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.CognitiveServices](resource-provider-operations.md#microsoftcognitiveservices)/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and list keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a97b65f3-24c7-4388-baec-2e87135dc908",
  "name": "a97b65f3-24c7-4388-baec-2e87135dc908",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read",
        "Microsoft.CognitiveServices/accounts/listkeys/action",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Insights/diagnosticSettings/read",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="internet-of-things"></a>Internet de las cosas


### <a name="device-update-administrator"></a>Device Update Administrator (Administrador de Device Update)

Proporciona acceso total a operaciones de contenido y administración. [Más información](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/read | Realiza una operación de lectura relacionada con las actualizaciones. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/write | Realiza una operación de escritura relacionada con las actualizaciones. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/delete | Realiza una operación de eliminación relacionada con las actualizaciones. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/read | Realiza una operación de lectura relacionada con la administración. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/write | Realiza una operación de escritura relacionada con la administración. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/delete | Realiza una operación de eliminación relacionada con la administración. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you full access to management and content operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/02ca0879-e8e4-47a5-a61e-5c618b76e64a",
  "name": "02ca0879-e8e4-47a5-a61e-5c618b76e64a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/updates/read",
        "Microsoft.DeviceUpdate/accounts/instances/updates/write",
        "Microsoft.DeviceUpdate/accounts/instances/updates/delete",
        "Microsoft.DeviceUpdate/accounts/instances/management/read",
        "Microsoft.DeviceUpdate/accounts/instances/management/write",
        "Microsoft.DeviceUpdate/accounts/instances/management/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-content-administrator"></a>Device Update Content Administrator (Administrador de contenido de Device Update)

Proporciona acceso total a operaciones de contenido. [Más información](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/read | Realiza una operación de lectura relacionada con las actualizaciones. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/write | Realiza una operación de escritura relacionada con las actualizaciones. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/delete | Realiza una operación de eliminación relacionada con las actualizaciones. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you full access to content operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0378884a-3af5-44ab-8323-f5b22f9f3c98",
  "name": "0378884a-3af5-44ab-8323-f5b22f9f3c98",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/updates/read",
        "Microsoft.DeviceUpdate/accounts/instances/updates/write",
        "Microsoft.DeviceUpdate/accounts/instances/updates/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Content Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-content-reader"></a>Device Update Content Reader (Lector de contenido de Device Update)

Proporciona acceso de lectura a operaciones de contenido, pero no permite realizar cambios. [Más información](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/read | Realiza una operación de lectura relacionada con las actualizaciones. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you read access to content operations, but does not allow making changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d1ee9a80-8b14-47f0-bdc2-f4a351625a7b",
  "name": "d1ee9a80-8b14-47f0-bdc2-f4a351625a7b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/updates/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Content Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-deployments-administrator"></a>Device Update Deployments Administrator (Administrador de implementaciones de Device Update)

Proporciona acceso total a operaciones de administración. [Más información](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/read | Realiza una operación de lectura relacionada con la administración. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/write | Realiza una operación de escritura relacionada con la administración. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/delete | Realiza una operación de eliminación relacionada con la administración. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you full access to management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e4237640-0e3d-4a46-8fda-70bc94856432",
  "name": "e4237640-0e3d-4a46-8fda-70bc94856432",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/management/read",
        "Microsoft.DeviceUpdate/accounts/instances/management/write",
        "Microsoft.DeviceUpdate/accounts/instances/management/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Deployments Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-deployments-reader"></a>Device Update Deployments Reader (Lector de implementaciones de Device Update)

Proporciona acceso de lectura a operaciones de administración, pero no permite realizar cambios. [Más información](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/read | Realiza una operación de lectura relacionada con la administración. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you read access to management operations, but does not allow making changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/49e2f5d2-7741-4835-8efa-19e1fe35e47f",
  "name": "49e2f5d2-7741-4835-8efa-19e1fe35e47f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/management/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Deployments Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="device-update-reader"></a>Device Update Reader (Lector de Device Update)

Proporciona acceso de lectura a operaciones de contenido y administración, pero no permite realizar cambios. [Más información](../iot-hub-device-update/device-update-control-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/updates/read | Realiza una operación de lectura relacionada con las actualizaciones. |
> | [Microsoft.DeviceUpdate](resource-provider-operations.md#microsoftdeviceupdate)/accounts/instances/management/read | Realiza una operación de lectura relacionada con la administración. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives you read access to management and content operations, but does not allow making changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e9dba6fb-3d52-4cf0-bce3-f06ce71b9e0f",
  "name": "e9dba6fb-3d52-4cf0-bce3-f06ce71b9e0f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/alertRules/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.DeviceUpdate/accounts/instances/updates/read",
        "Microsoft.DeviceUpdate/accounts/instances/management/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Device Update Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="iot-hub-data-contributor"></a>Colaborador de datos de IoT Hub

Permite el acceso total a las operaciones del plano de datos de IoT Hub. [Más información](../iot-hub/iot-hub-dev-guide-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to IoT Hub data plane operations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4fc6c259-987e-4a07-842e-c321cc9d413f",
  "name": "4fc6c259-987e-4a07-842e-c321cc9d413f",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Devices/IotHubs/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "IoT Hub Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="iot-hub-data-reader"></a>Lector de datos de IoT Hub

Permite el acceso de lectura total a las propiedades del plano de datos de IoT Hub [Más información](../iot-hub/iot-hub-dev-guide-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/*/read |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/fileUpload/notifications/action | Recibe, completa o abandona notificaciones de carga de archivos. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full read access to IoT Hub data-plane properties",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b447c946-2db7-41ec-983d-d8bf3b1c77e3",
  "name": "b447c946-2db7-41ec-983d-d8bf3b1c77e3",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Devices/IotHubs/*/read",
        "Microsoft.Devices/IotHubs/fileUpload/notifications/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "IoT Hub Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="iot-hub-registry-contributor"></a>Colaborador del registro de IoT Hub

Permite el acceso completo al registro de dispositivos de IoT Hub. [Más información](../iot-hub/iot-hub-dev-guide-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/devices/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to IoT Hub device registry.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4ea46cd5-c1b2-4a8e-910b-273211f9ce47",
  "name": "4ea46cd5-c1b2-4a8e-910b-273211f9ce47",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Devices/IotHubs/devices/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "IoT Hub Registry Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="iot-hub-twin-contributor"></a>Colaborador de gemelos de IoT Hub

Permite el acceso de lectura y escritura a todos los dispositivos y módulos gemelos de IoT Hub. [Más información](../iot-hub/iot-hub-dev-guide-azure-ad-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Devices](resource-provider-operations.md#microsoftdevices)/IotHubs/twins/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read and write access to all IoT Hub device and module twins.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/494bdba2-168f-4f31-a0a1-191d2f7c028c",
  "name": "494bdba2-168f-4f31-a0a1-191d2f7c028c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Devices/IotHubs/twins/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "IoT Hub Twin Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="mixed-reality"></a>Realidad mixta


### <a name="remote-rendering-administrator"></a>Administrador de Remote Rendering

Proporciona al usuario funcionalidades de conversión, administración de sesiones, representación y diagnóstico para Azure Remote Rendering. [Más información](../remote-rendering/how-tos/authentication.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/convert/action | Inicia una conversión de recursos. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/convert/read | Obtiene las propiedades de conversión de recursos. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/convert/delete | Detiene una conversión de recursos. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/read | Obtiene propiedades de una sesión. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/action | Inicia sesiones. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/delete | Detiene sesiones. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/render/read | Se conecta a una sesión. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/diagnostic/read | Se conecta al inspector de Remote Rendering. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides user with conversion, manage session, rendering and diagnostics capabilities for Azure Remote Rendering",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3df8b902-2a6f-47c7-8cc5-360e9b272a7e",
  "name": "3df8b902-2a6f-47c7-8cc5-360e9b272a7e",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/RemoteRenderingAccounts/convert/action",
        "Microsoft.MixedReality/RemoteRenderingAccounts/convert/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/convert/delete",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/action",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/delete",
        "Microsoft.MixedReality/RemoteRenderingAccounts/render/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/diagnostic/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Remote Rendering Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="remote-rendering-client"></a>Cliente de Remote Rendering

Proporciona al usuario funcionalidades de administración de sesiones, representación y diagnóstico para Azure Remote Rendering. [Más información](../remote-rendering/how-tos/authentication.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/read | Obtiene propiedades de una sesión. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/action | Inicia sesiones. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/managesessions/delete | Detiene sesiones. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/render/read | Se conecta a una sesión. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/RemoteRenderingAccounts/diagnostic/read | Se conecta al inspector de Remote Rendering. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides user with manage session, rendering and diagnostics capabilities for Azure Remote Rendering.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d39065c4-c120-43c9-ab0a-63eed9795f0a",
  "name": "d39065c4-c120-43c9-ab0a-63eed9795f0a",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/action",
        "Microsoft.MixedReality/RemoteRenderingAccounts/managesessions/delete",
        "Microsoft.MixedReality/RemoteRenderingAccounts/render/read",
        "Microsoft.MixedReality/RemoteRenderingAccounts/diagnostic/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Remote Rendering Client",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-contributor"></a>Colaborador de la cuenta de Spatial Anchors

Permite administrar los anclajes espaciales en su cuenta, pero no eliminarlos. [Más información](../spatial-anchors/concepts/authentication.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/create/action | Crea anclajes espaciales. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/discovery/read | Detecta anclajes espaciales próximos. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/properties/read | Obtiene las propiedades de los anclajes espaciales. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/query/read | Ubica anclajes espaciales. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/submitdiag/read | Envía datos de diagnóstico para ayudar a mejorar la calidad del servicio Azure Spatial Anchors. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/write | Actualiza las propiedades de los anclajes espaciales. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, but not delete them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "name": "8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-owner"></a>Propietario de la cuenta de Spatial Anchors

Permite administrar los anclajes espaciales en su cuenta y eliminarlos. [Más información](../spatial-anchors/concepts/authentication.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/create/action | Crea anclajes espaciales. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/delete | Elimina anclajes espaciales. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/discovery/read | Detecta anclajes espaciales próximos. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/properties/read | Obtiene las propiedades de los anclajes espaciales. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/query/read | Ubica anclajes espaciales. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/submitdiag/read | Envía datos de diagnóstico para ayudar a mejorar la calidad del servicio Azure Spatial Anchors. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/write | Actualiza las propiedades de los anclajes espaciales. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, including deleting them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/70bbe301-9835-447d-afdd-19eb3167307c",
  "name": "70bbe301-9835-447d-afdd-19eb3167307c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/delete",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-reader"></a>Lector de la cuenta de Spatial Anchors

Permite encontrar y leer propiedades de los anclajes espaciales en la cuenta. [Más información](../spatial-anchors/concepts/authentication.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/discovery/read | Detecta anclajes espaciales próximos. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/properties/read | Obtiene las propiedades de los anclajes espaciales. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/query/read | Ubica anclajes espaciales. |
> | [Microsoft.MixedReality](resource-provider-operations.md#microsoftmixedreality)/SpatialAnchorsAccounts/submitdiag/read | Envía datos de diagnóstico para ayudar a mejorar la calidad del servicio Azure Spatial Anchors. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you locate and read properties of spatial anchors in your account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "name": "5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="integration"></a>Integración


### <a name="api-management-service-contributor"></a>Colaborador de servicio de administración de API

Puede administrar servicios y las API. [Más información](../api-management/api-management-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/* | Crear y administrar servicio API Management |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service and the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c",
  "name": "312a565d-c81f-4fd8-895a-4e21e48d571c",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-operator-role"></a>Rol del operador del servicio API Management

Puede administrar el servicio, pero no las API. [Más información](../api-management/api-management-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/*/read | Lectura de instancias del servicio API Management |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/backup/action | Realiza una copia de seguridad del servicio API Management en el contenedor especificado de una cuenta de almacenamiento proporcionada por el usuario |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/delete | Elimina una instancia del servicio API Management |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/managedeployments/action | Cambia SKU y unidades, y agrega o quita las implementaciones regionales del servicio API Management |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/read | Lectura de los metadatos de una instancia del servicio API Management |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/restore/action | Restauración del servicio API Management desde el contenedor especificado de una cuenta de almacenamiento proporcionada por el usuario |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/updatecertificate/action | Carga el certificado TLS/SSL de un servicio API Management |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/updatehostname/action | Configura, actualiza o elimina los nombres de dominio personalizado de un servicio API Management |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/write | Creación o actualización de una instancia de servicio de API Management |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/users/keys/read | Obtener las claves asociadas con el usuario. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service but not the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "name": "e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/backup/action",
        "Microsoft.ApiManagement/service/delete",
        "Microsoft.ApiManagement/service/managedeployments/action",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.ApiManagement/service/restore/action",
        "Microsoft.ApiManagement/service/updatecertificate/action",
        "Microsoft.ApiManagement/service/updatehostname/action",
        "Microsoft.ApiManagement/service/write",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-reader-role"></a>Rol de lector del servicio API Management

Acceso de solo lectura al servicio y las API. [Más información](../api-management/api-management-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/*/read | Lectura de instancias del servicio API Management |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/read | Lectura de los metadatos de una instancia del servicio API Management |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | [Microsoft.ApiManagement](resource-provider-operations.md#microsoftapimanagement)/service/users/keys/read | Obtener las claves asociadas con el usuario. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only access to service and APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/71522526-b88f-4d52-b57f-d31fc3546d0d",
  "name": "71522526-b88f-4d52-b57f-d31fc3546d0d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-owner"></a>Propietario de los datos de App Configuration

Permite el acceso completo a los datos de App Configuration. [Más información](../azure-app-configuration/concept-enable-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.AppConfiguration](resource-provider-operations.md#microsoftappconfiguration)/configurationStores/*/read |  |
> | [Microsoft.AppConfiguration](resource-provider-operations.md#microsoftappconfiguration)/configurationStores/*/write |  |
> | [Microsoft.AppConfiguration](resource-provider-operations.md#microsoftappconfiguration)/configurationStores/*/delete |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows full access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "name": "5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read",
        "Microsoft.AppConfiguration/configurationStores/*/write",
        "Microsoft.AppConfiguration/configurationStores/*/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-reader"></a>Lector de los datos de App Configuration

Permite el acceso de lectura a los datos de App Configuration. [Más información](../azure-app-configuration/concept-enable-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.AppConfiguration](resource-provider-operations.md#microsoftappconfiguration)/configurationStores/*/read |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/516239f1-63e1-4d78-a4de-a74fb236a071",
  "name": "516239f1-63e1-4d78-a4de-a74fb236a071",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-relay-listener"></a>Cliente de escucha de Azure Relay

Permite el acceso de escucha a recursos de Azure Relay.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/wcfRelays/read |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/hybridConnections/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/listen/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for listen access to Azure Relay resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/26e0b698-aa6d-4085-9386-aadae190014d",
  "name": "26e0b698-aa6d-4085-9386-aadae190014d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Relay/*/wcfRelays/read",
        "Microsoft.Relay/*/hybridConnections/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Relay/*/listen/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Relay Listener",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-relay-owner"></a>Propietario de Azure Relay

Permite el acceso completo a los recursos de Azure Relay.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Relay resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2787bf04-f1f5-4bfe-8383-c8a24483ee38",
  "name": "2787bf04-f1f5-4bfe-8383-c8a24483ee38",
  "permissions": [
    {
      "actions": [
        "Microsoft.Relay/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Relay/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Relay Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-relay-sender"></a>Emisor de Azure Relay

Permite el acceso de envío a los recursos de Azure Relay.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/wcfRelays/read |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/hybridConnections/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Relay](resource-provider-operations.md#microsoftrelay)/*/send/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for send access to Azure Relay resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/26baccc8-eea7-41f1-98f4-1762cc7f685d",
  "name": "26baccc8-eea7-41f1-98f4-1762cc7f685d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Relay/*/wcfRelays/read",
        "Microsoft.Relay/*/hybridConnections/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Relay/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Relay Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-owner"></a>Propietario de los datos de Azure Service Bus

Concede acceso total a los recursos de Azure Service Bus. [Más información](../service-bus-messaging/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/090c5cfd-751d-490a-894a-3ce6f1109419",
  "name": "090c5cfd-751d-490a-894a-3ce6f1109419",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-receiver"></a>Receptor de datos de Azure Service Bus

Concede acceso de recepción a los recursos de Azure Service Bus. [Más información](../service-bus-messaging/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/queues/read |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/topics/read |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/receive/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for receive access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "name": "4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-sender"></a>Emisor de datos de Azure Service Bus

Concede acceso de emisión a los recursos de Azure Service Bus. [Más información](../service-bus-messaging/authenticate-application.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/queues/read |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/topics/read |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.ServiceBus](resource-provider-operations.md#microsoftservicebus)/*/send/action |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for send access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "name": "69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-stack-registration-owner"></a>Propietario del registro de Azure Stack

Permite administrar los registros de Azure Stack.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.AzureStack](resource-provider-operations.md#microsoftazurestack)/edgeSubscriptions/read |  |
> | [Microsoft.AzureStack](resource-provider-operations.md#microsoftazurestack)/registrations/products/*/action |  |
> | [Microsoft.AzureStack](resource-provider-operations.md#microsoftazurestack)/registrations/products/read | Obtiene las propiedades de un producto de Marketplace de Azure Stack. |
> | [Microsoft.AzureStack](resource-provider-operations.md#microsoftazurestack)/registrations/read | Obtiene las propiedades de un registro de Azure Stack. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Stack registrations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "name": "6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "permissions": [
    {
      "actions": [
        "Microsoft.AzureStack/edgeSubscriptions/read",
        "Microsoft.AzureStack/registrations/products/*/action",
        "Microsoft.AzureStack/registrations/products/read",
        "Microsoft.AzureStack/registrations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Stack Registration Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-contributor"></a>Colaborador de Event Grid

Permite administrar las operaciones de Event Grid.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/* | Crear y administrar recursos de Event Grid. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage EventGrid operations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1e241071-0855-49ea-94dc-649edcd759de",
  "name": "1e241071-0855-49ea-94dc-649edcd759de",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-data-sender"></a>Emisor de datos de EventGrid

Permite el acceso de envío a eventos de Event Grid.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/topics/read | Lee un tema. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/domains/read | Lee un dominio. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/partnerNamespaces/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/events/send/action | Envía eventos a los temas. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows send access to event grid events.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d5a91429-5739-47e2-a06b-3470a27159e7",
  "name": "d5a91429-5739-47e2-a06b-3470a27159e7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/topics/read",
        "Microsoft.EventGrid/domains/read",
        "Microsoft.EventGrid/partnerNamespaces/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventGrid/events/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-contributor"></a>Colaborador de EventGrid EventSubscription

Permite administrar las operaciones de suscripción de eventos de EventGrid. [Más información](../event-grid/security-authorization.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/* | Crear y administrar suscripciones de eventos regionales. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/topicTypes/eventSubscriptions/read | Enumera las suscripciones de eventos globales por tipo de tema. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/locations/eventSubscriptions/read | Enumera las suscripciones de eventos regionales. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/locations/topicTypes/eventSubscriptions/read | Enumera las suscripciones de eventos regionales por tipo de tema. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage EventGrid event subscription operations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "name": "428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/*",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-reader"></a>Lector de EventGrid EventSubscription

Permite leer las suscripciones de eventos de EventGrid. [Más información](../event-grid/security-authorization.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/read | Lee una clase eventSubscription. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/topicTypes/eventSubscriptions/read | Enumera las suscripciones de eventos globales por tipo de tema. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/locations/eventSubscriptions/read | Enumera las suscripciones de eventos regionales. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/locations/topicTypes/eventSubscriptions/read | Enumera las suscripciones de eventos regionales por tipo de tema. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read EventGrid event subscriptions.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2414bbcf-6497-4faf-8c65-045460748405",
  "name": "2414bbcf-6497-4faf-8c65-045460748405",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/read",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="fhir-data-contributor"></a>Colaborador de datos de FHIR

El rol permite el acceso completo del usuario o la entidad de seguridad a los datos de FHIR. [Más información](../healthcare-apis/azure-api-for-fhir/configure-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/* |  |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role allows user or principal full access to FHIR Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5a1fc7df-4bf1-4951-a576-89034ee01acd",
  "name": "5a1fc7df-4bf1-4951-a576-89034ee01acd",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/*",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "FHIR Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="fhir-data-exporter"></a>Exportador de datos de FHIR

El rol permite al usuario o a la entidad de seguridad leer y exportar datos de FHIR. [Más información](../healthcare-apis/azure-api-for-fhir/configure-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/read | Lee los recursos de FHIR (incluye la búsqueda y el historial de versiones).  |
> | Microsoft.HealthcareApis/services/fhir/resources/export/action | Operación de exportación ($export). |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/read | Lee los recursos de FHIR (incluye la búsqueda y el historial de versiones).  |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/export/action | Operación de exportación ($export). |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role allows user or principal to read and export FHIR Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3db33094-8700-4567-8da5-1501d4e7e843",
  "name": "3db33094-8700-4567-8da5-1501d4e7e843",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/read",
        "Microsoft.HealthcareApis/services/fhir/resources/export/action",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/read",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/export/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "FHIR Data Exporter",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="fhir-data-reader"></a>Lector de datos de FHIR

El rol permite al usuario o a la entidad de seguridad leer datos de FHIR. [Más información](../healthcare-apis/azure-api-for-fhir/configure-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/read | Lee los recursos de FHIR (incluye la búsqueda y el historial de versiones).  |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/read | Lee los recursos de FHIR (incluye la búsqueda y el historial de versiones).  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role allows user or principal to read FHIR Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4c8d0bbc-75d3-4935-991f-5f3c56d81508",
  "name": "4c8d0bbc-75d3-4935-991f-5f3c56d81508",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/read",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "FHIR Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="fhir-data-writer"></a>Escritor de datos de FHIR

El rol permite al usuario o a la entidad de seguridad leer y escribir datos de FHIR. [Más información](../healthcare-apis/azure-api-for-fhir/configure-azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/* |  |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/* |  |
> | **NotDataActions** |  |
> | Microsoft.HealthcareApis/services/fhir/resources/hardDelete/action | Eliminar hardware (incluido el historial de versiones). |
> | Microsoft.HealthcareApis/workspaces/fhirservices/resources/hardDelete/action | Eliminar hardware (incluido el historial de versiones). |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role allows user or principal to read and write FHIR Data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3f88fce4-5892-4214-ae73-ba5294559913",
  "name": "3f88fce4-5892-4214-ae73-ba5294559913",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/*",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/*"
      ],
      "notDataActions": [
        "Microsoft.HealthcareApis/services/fhir/resources/hardDelete/action",
        "Microsoft.HealthcareApis/workspaces/fhirservices/resources/hardDelete/action"
      ]
    }
  ],
  "roleName": "FHIR Data Writer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="integration-service-environment-contributor"></a>Colaborador del Entorno del servicio de integración

Permite administrar entornos de servicio de integración, pero no acceder a ellos. [Más información](../logic-apps/add-artifacts-integration-service-environment-ise.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/integrationServiceEnvironments/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage integration service environments, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a41e2c5b-bd99-4a07-88f4-9bf657a760b8",
  "name": "a41e2c5b-bd99-4a07-88f4-9bf657a760b8",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Support/*",
        "Microsoft.Logic/integrationServiceEnvironments/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Integration Service Environment Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="integration-service-environment-developer"></a>Desarrollador del Entorno del servicio de integración

Permite a los desarrolladores crear y actualizar flujos de trabajo, cuentas de integración y conexiones API en entornos de servicios de integración. [Más información](../logic-apps/add-artifacts-integration-service-environment-ise.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/integrationServiceEnvironments/read | Lee el entorno de servicio de integración. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/integrationServiceEnvironments/*/join/action |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows developers to create and update workflows, integration accounts and API connections in integration service environments.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c7aa55d3-1abb-444a-a5ca-5e51e485d6ec",
  "name": "c7aa55d3-1abb-444a-a5ca-5e51e485d6ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Support/*",
        "Microsoft.Logic/integrationServiceEnvironments/read",
        "Microsoft.Logic/integrationServiceEnvironments/*/join/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Integration Service Environment Developer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="intelligent-systems-account-contributor"></a>Colaborador de la cuenta de Sistemas inteligentes

Permite administrar las cuentas de Intelligent Systems, pero no acceder a ellas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | Microsoft.IntelligentSystems/accounts/* | Crear y administrar cuentas de sistemas inteligentes |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Intelligent Systems accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/03a6d094-3444-4b3d-88af-7477090a9e5e",
  "name": "03a6d094-3444-4b3d-88af-7477090a9e5e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.IntelligentSystems/accounts/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Intelligent Systems Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-contributor"></a>Colaborador de aplicación lógica

Le permite administrar aplicaciones lógicas, pero no cambiar el acceso a ellas. [Más información](../logic-apps/logic-apps-securing-a-logic-app.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/listKeys/action | Enumera las claves de acceso de las cuentas de almacenamiento. |
> | [Microsoft.ClassicStorage](resource-provider-operations.md#microsoftclassicstorage)/storageAccounts/read | Devuelve la cuenta de almacenamiento con la cuenta especificada. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricAlerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Crea, actualiza o lee la configuración de diagnóstico de Analysis Server. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/logdefinitions/* | Este permiso es necesario para los usuarios que necesitan acceder a registros de actividades a través del portal. Enumere las categorías de registro del registro de actividad. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/* | Leer definiciones de métrica (lista de tipos de métricas disponibles para un recurso). |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/* | Administra los recursos de Logic Apps. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listkeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/connectionGateways/* | Crea y administra una puerta de enlace de conexión. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/connections/* | Crea y administra una conexión. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/customApis/* | Crea y administra una API personalizada. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/join/action | Unirse a un plan de App Service |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/read | Obtiene las propiedades de un plan de App Service |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/sites/functions/listSecrets/action | Lista de secretos de función. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage logic app, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "name": "87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logdefinitions/*",
        "Microsoft.Insights/metricDefinitions/*",
        "Microsoft.Logic/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*",
        "Microsoft.Web/connections/*",
        "Microsoft.Web/customApis/*",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/functions/listSecrets/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-operator"></a>Operador de aplicación lógica

Le permite leer, habilitar y deshabilitar aplicaciones lógicas, pero no permite editarlas ni actualizarlas. [Más información](../logic-apps/logic-apps-securing-a-logic-app.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/*/read | Permite leer reglas de alerta de Insights. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricAlerts/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/*/read | Obtiene la configuración de diagnóstico de Logic Apps. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricDefinitions/*/read | Obtiene las métricas disponibles para Logic Apps. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/*/read | Lee los recursos de Logic Apps. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/disable/action | Deshabilita el flujo de trabajo. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/enable/action | Habilita el flujo de trabajo. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/validate/action | Valida el flujo de trabajo. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/connectionGateways/*/read | Lee las puertas de enlace de conexión. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/connections/*/read | Lee las conexiones. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/customApis/*/read | Lee la API personalizada. |
> | [Microsoft.Web](resource-provider-operations.md#microsoftweb)/serverFarms/read | Obtiene las propiedades de un plan de App Service |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read, enable and disable logic app.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "name": "515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*/read",
        "Microsoft.Insights/metricAlerts/*/read",
        "Microsoft.Insights/diagnosticSettings/*/read",
        "Microsoft.Insights/metricDefinitions/*/read",
        "Microsoft.Logic/*/read",
        "Microsoft.Logic/workflows/disable/action",
        "Microsoft.Logic/workflows/enable/action",
        "Microsoft.Logic/workflows/validate/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*/read",
        "Microsoft.Web/connections/*/read",
        "Microsoft.Web/customApis/*/read",
        "Microsoft.Web/serverFarms/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="identity"></a>Identidad


### <a name="managed-identity-contributor"></a>Colaborador de identidad administrada

Permite crear, leer, actualizar y eliminar identidades asignadas por el usuario. [Más información](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/read | Obtiene la identidad asignada a un usuario existente. |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/write | Crea una nueva identidad asignada a un usuario o actualiza las etiquetas asociadas a una identidad asignada a un usuario existente. |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/delete | Elimina la identidad asignada a un usuario existente. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, Read, Update, and Delete User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "name": "e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/write",
        "Microsoft.ManagedIdentity/userAssignedIdentities/delete",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-identity-operator"></a>Operador de identidad administrada

Permite leer y asignar identidades asignadas por el usuario. [Más información](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/*/read |  |
> | [Microsoft.ManagedIdentity](resource-provider-operations.md#microsoftmanagedidentity)/userAssignedIdentities/*/assign/action |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read and Assign User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f1a07417-d97a-45cb-824c-7a7467783830",
  "name": "f1a07417-d97a-45cb-824c-7a7467783830",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="security"></a>Seguridad


### <a name="attestation-contributor"></a>Colaborador de atestación

Puede leer, escribir o eliminar la instancia del proveedor de atestación [Más información](../attestation/quickstart-powershell.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | Microsoft.Attestation/attestationProviders/attestation/read |  |
> | Microsoft.Attestation/attestationProviders/attestation/write |  |
> | Microsoft.Attestation/attestationProviders/attestation/delete |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read write or delete the attestation provider instance",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/bbf86eb8-f7b4-4cce-96e4-18cddf81d86e",
  "name": "bbf86eb8-f7b4-4cce-96e4-18cddf81d86e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Attestation/attestationProviders/attestation/read",
        "Microsoft.Attestation/attestationProviders/attestation/write",
        "Microsoft.Attestation/attestationProviders/attestation/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Attestation Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="attestation-reader"></a>Lector de atestación

No se pueden leer las propiedades del proveedor de atestación [Más información](../attestation/troubleshoot-guide.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | Microsoft.Attestation/attestationProviders/attestation/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read the attestation provider properties",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fd1bd22b-8476-40bc-a0bc-69b95687b9f3",
  "name": "fd1bd22b-8476-40bc-a0bc-69b95687b9f3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Attestation/attestationProviders/attestation/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Attestation Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-administrator"></a>Administrador de Key Vault

Permite realizar todas las operaciones de plano de datos en un almacén de claves y en todos los objetos que contiene, incluidos los certificados, las claves y los secretos. No permite administrar los recursos del almacén de claves ni administrar las asignaciones de roles. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". [Más información](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Comprueba que un nombre de almacén de claves es válido y que no está en uso |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Ve las propiedades de los almacenes de claves eliminados temporalmente |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Enumera las operaciones disponibles en el proveedor de recursos de Microsoft.KeyVault. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform all data plane operations on a key vault and all objects in it, including certificates, keys, and secrets. Cannot manage key vault resources or manage role assignments. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00482a5a-887f-4fb3-b363-3b7fe8e74483",
  "name": "00482a5a-887f-4fb3-b363-3b7fe8e74483",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-certificates-officer"></a>Agente de certificados de Key Vault

Permite realizar cualquier acción en los certificados de un almacén de claves, excepto administrar permisos. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". [Más información](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Comprueba que un nombre de almacén de claves es válido y que no está en uso |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Ve las propiedades de los almacenes de claves eliminados temporalmente |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Enumera las operaciones disponibles en el proveedor de recursos de Microsoft.KeyVault. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/certificatecas/* |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/certificates/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform any action on the certificates of a key vault, except manage permissions. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a4417e6f-fecd-4de8-b567-7b0420556985",
  "name": "a4417e6f-fecd-4de8-b567-7b0420556985",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/certificatecas/*",
        "Microsoft.KeyVault/vaults/certificates/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Certificates Officer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-contributor"></a>Colaborador de almacén de claves

Permite administrar almacenes de claves, per no asignar roles en Azure RBAC ni acceder a secretos, claves o certificados. [Más información](../key-vault/general/security-features.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/deletedVaults/purge/action | Purga un almacén de claves eliminado temporalmente |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/hsmPools/* |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/managedHsms/* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage key vaults, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f25e0fa2-a7c8-4377-a976-54943a77a395",
  "name": "f25e0fa2-a7c8-4377-a976-54943a77a395",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.KeyVault/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.KeyVault/locations/deletedVaults/purge/action",
        "Microsoft.KeyVault/hsmPools/*",
        "Microsoft.KeyVault/managedHsms/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-crypto-officer"></a>Agente criptográfico de Key Vault

Permite realizar cualquier acción en las claves de un almacén de claves, excepto administrar permisos. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". [Más información](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Comprueba que un nombre de almacén de claves es válido y que no está en uso |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Ve las propiedades de los almacenes de claves eliminados temporalmente |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Enumera las operaciones disponibles en el proveedor de recursos de Microsoft.KeyVault. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform any action on the keys of a key vault, except manage permissions. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/14b46e9e-c2b7-41b4-b07b-48a6ebf60603",
  "name": "14b46e9e-c2b7-41b4-b07b-48a6ebf60603",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/keys/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Crypto Officer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-crypto-service-encryption-user"></a>Usuario de cifrado de servicio criptográfico de Key Vault

Permite leer los metadatos de las claves y realizar operaciones de encapsulado/desencapsulado. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". [Más información](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/write | Crea o actualiza una suscripción a eventos. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/read | Lee una clase eventSubscription. |
> | [Microsoft.EventGrid](resource-provider-operations.md#microsofteventgrid)/eventSubscriptions/delete | Elimina una suscripción a eventos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/read | Muestra las claves del almacén especificado, o lee las propiedades y el material público de una clave. En el caso de las claves asimétricas, esta operación expone la clave pública e incluye la posibilidad de realizar algoritmos de clave pública, como cifrar y comprobar una firma. Las claves privadas y las simétricas no se exponen nunca. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/wrap/action | Permite encapsular una clave simétrica con una clave de Key Vault. Tenga en cuenta que, si la clave de Key Vault es asimétrica, esta operación la pueden realizar entidades de seguridad con acceso de lectura. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/unwrap/action | Permite desencapsular una clave simétrica con una clave de Key Vault. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read metadata of keys and perform wrap/unwrap operations. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e147488a-f6f5-4113-8e2d-b22465e65bf6",
  "name": "e147488a-f6f5-4113-8e2d-b22465e65bf6",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventGrid/eventSubscriptions/write",
        "Microsoft.EventGrid/eventSubscriptions/read",
        "Microsoft.EventGrid/eventSubscriptions/delete"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/keys/read",
        "Microsoft.KeyVault/vaults/keys/wrap/action",
        "Microsoft.KeyVault/vaults/keys/unwrap/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Crypto Service Encryption User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-crypto-user"></a>Usuario criptográfico de Key Vault

Permite realizar operaciones criptográficas mediante claves. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". [Más información](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/read | Muestra las claves del almacén especificado, o lee las propiedades y el material público de una clave. En el caso de las claves asimétricas, esta operación expone la clave pública e incluye la posibilidad de realizar algoritmos de clave pública, como cifrar y comprobar una firma. Las claves privadas y las simétricas no se exponen nunca. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/update/action | Actualiza los atributos especificados asociados con la clave dada. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/backup/action | Crea el archivo de copia de seguridad de una clave. El archivo se puede usar para restaurar la clave en una instancia de Key Vault de la misma suscripción. Pueden aplicarse restricciones. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/encrypt/action | Cifra el texto no cifrado con una clave. Tenga en cuenta que, si la clave es asimétrica, esta operación la pueden realizar entidades de seguridad con acceso de lectura. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/decrypt/action | Descifra el texto cifrado con una clave. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/wrap/action | Permite encapsular una clave simétrica con una clave de Key Vault. Tenga en cuenta que, si la clave de Key Vault es asimétrica, esta operación la pueden realizar entidades de seguridad con acceso de lectura. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/unwrap/action | Permite desencapsular una clave simétrica con una clave de Key Vault. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/sign/action | Firma una síntesis del mensaje (hash) con una clave. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/keys/verify/action | Comprueba la firma de una síntesis del mensaje (hash) con una clave. Tenga en cuenta que, si la clave es asimétrica, esta operación la pueden realizar entidades de seguridad con acceso de lectura. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform cryptographic operations using keys. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/12338af0-0e69-4776-bea7-57ae8d297424",
  "name": "12338af0-0e69-4776-bea7-57ae8d297424",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/keys/read",
        "Microsoft.KeyVault/vaults/keys/update/action",
        "Microsoft.KeyVault/vaults/keys/backup/action",
        "Microsoft.KeyVault/vaults/keys/encrypt/action",
        "Microsoft.KeyVault/vaults/keys/decrypt/action",
        "Microsoft.KeyVault/vaults/keys/wrap/action",
        "Microsoft.KeyVault/vaults/keys/unwrap/action",
        "Microsoft.KeyVault/vaults/keys/sign/action",
        "Microsoft.KeyVault/vaults/keys/verify/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Crypto User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-reader"></a>Lector de Key Vault

Permite leer metadatos de almacenes de claves y sus certificados, claves y secretos. No se pueden leer valores confidenciales, como el contenido de los secretos o el material de las claves. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". [Más información](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Comprueba que un nombre de almacén de claves es válido y que no está en uso |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Ve las propiedades de los almacenes de claves eliminados temporalmente |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Enumera las operaciones disponibles en el proveedor de recursos de Microsoft.KeyVault. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/secrets/readMetadata/action | Permite enumerar o ver las propiedades de un secreto, pero no su valor. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read metadata of key vaults and its certificates, keys, and secrets. Cannot read sensitive values such as secret contents or key material. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/21090545-7ca7-4776-b22c-e363652d74d2",
  "name": "21090545-7ca7-4776-b22c-e363652d74d2",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/vaults/secrets/readMetadata/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-secrets-officer"></a>Agente de secretos de Key Vault

Permite realizar cualquier acción en los secretos de un almacén de claves, excepto administrar permisos. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". [Más información](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/checkNameAvailability/read | Comprueba que un nombre de almacén de claves es válido y que no está en uso |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/deletedVaults/read | Ve las propiedades de los almacenes de claves eliminados temporalmente |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/locations/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/*/read |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/operations/read | Enumera las operaciones disponibles en el proveedor de recursos de Microsoft.KeyVault. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/secrets/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Perform any action on the secrets of a key vault, except manage permissions. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b86a8fe4-44ce-4948-aee5-eccb2c155cd7",
  "name": "b86a8fe4-44ce-4948-aee5-eccb2c155cd7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.KeyVault/checkNameAvailability/read",
        "Microsoft.KeyVault/deletedVaults/read",
        "Microsoft.KeyVault/locations/*/read",
        "Microsoft.KeyVault/vaults/*/read",
        "Microsoft.KeyVault/operations/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/secrets/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Secrets Officer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-secrets-user"></a>Usuario de secretos de Key Vault

Permite leer el contenido de los secretos. Solo funciona para almacenes de claves que usan el modelo de permisos "Control de acceso basado en rol de Azure". [Más información](../key-vault/general/rbac-guide.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/secrets/getSecret/action | Obtiene el valor de un secreto. |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/vaults/secrets/readMetadata/action | Permite enumerar o ver las propiedades de un secreto, pero no su valor. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read secret contents. Only works for key vaults that use the 'Azure role-based access control' permission model.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4633458b-17de-408a-b874-0445c86b69e6",
  "name": "4633458b-17de-408a-b874-0445c86b69e6",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.KeyVault/vaults/secrets/getSecret/action",
        "Microsoft.KeyVault/vaults/secrets/readMetadata/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Secrets User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-hsm-contributor"></a>Colaborador de HSM administrado

Permite administrar grupos de HSM administrados, pero no accede a ellas. [Más información](../key-vault/managed-hsm/secure-your-managed-hsm.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.KeyVault](resource-provider-operations.md#microsoftkeyvault)/managedHSMs/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage managed HSM pools, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/18500a29-7fe2-46b2-a342-b16a415e101d",
  "name": "18500a29-7fe2-46b2-a342-b16a415e101d",
  "permissions": [
    {
      "actions": [
        "Microsoft.KeyVault/managedHSMs/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed HSM contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="microsoft-sentinel-automation-contributor"></a>Colaborador de automatización de Microsoft Sentinel

Colaborador de automatización de Microsoft Sentinel [Más información](../sentinel/roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/triggers/read | Lee el desencadenador. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/triggers/listCallbackUrl/action | Obtiene la dirección URL de devolución de llamada para el desencadenador. |
> | [Microsoft.Logic](resource-provider-operations.md#microsoftlogic)/workflows/runs/read | Lee la ejecución de flujo de trabajo. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft Sentinel Automation Contributor",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f4c81013-99ee-4d62-a7ee-b3f1f648599a",
  "name": "f4c81013-99ee-4d62-a7ee-b3f1f648599a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Logic/workflows/triggers/read",
        "Microsoft.Logic/workflows/triggers/listCallbackUrl/action",
        "Microsoft.Logic/workflows/runs/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Microsoft Sentinel Automation Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="microsoft-sentinel-contributor"></a>Colaborador de Microsoft Sentinel

Colaborador de Microsoft Sentinel [Más información](../sentinel/roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/* |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/analytics/query/action | Realiza búsquedas mediante el nuevo motor. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Consulta datos de Log Analytics. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/savedSearches/* |  |
> | [Microsoft.OperationsManagement](resource-provider-operations.md#microsoftoperationsmanagement)/solutions/read | Obtiene una solución OMS ya existente |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/read | Ejecuta consultas en los datos del área de trabajo. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/*/read |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/dataSources/read | Obtiene los orígenes de datos en un área de trabajo. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/querypacks/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/myworkbooks/read | Lee un libro privado. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft Sentinel Contributor",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ab8e14d6-4a74-4a29-9ba8-549422addade",
  "name": "ab8e14d6-4a74-4a29-9ba8-549422addade",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.OperationalInsights/querypacks/*/read",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Insights/myworkbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Microsoft Sentinel Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="microsoft-sentinel-reader"></a>Lector de Microsoft Sentinel

Lector de Microsoft Sentinel [Más información](../sentinel/roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/*/read |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/dataConnectorsCheckRequirements/action | Comprueba la autorización y la licencia del usuario. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/query/action | Obtiene indicadores de inteligencia sobre amenazas. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/queryIndicators/action | Obtiene indicadores de inteligencia sobre amenazas. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/analytics/query/action | Realiza búsquedas mediante el nuevo motor. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Consulta datos de Log Analytics. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/LinkedServices/read | Obtiene los servicios vinculados en función del área de trabajo establecida. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/savedSearches/read | Obtiene una consulta de búsqueda guardada |
> | [Microsoft.OperationsManagement](resource-provider-operations.md#microsoftoperationsmanagement)/solutions/read | Obtiene una solución OMS ya existente |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/read | Ejecuta consultas en los datos del área de trabajo. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/*/read |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/querypacks/*/read |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/dataSources/read | Obtiene los orígenes de datos en un área de trabajo. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/read | Lee un libro. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/myworkbooks/read | Lee un libro privado. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft Sentinel Reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "name": "8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/query/action",
        "Microsoft.SecurityInsights/threatIntelligence/queryIndicators/action",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/LinkedServices/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/querypacks/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Insights/myworkbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Microsoft Sentinel Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="microsoft-sentinel-responder"></a>Respondedor de Microsoft Sentinel

Respondedor de Microsoft Sentinel [Más información](../sentinel/roles.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/*/read |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/dataConnectorsCheckRequirements/action | Comprueba la autorización y la licencia del usuario. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/automationRules/* |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/cases/* |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/incidents/* |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/appendTags/action | Anexa etiquetas al indicador de inteligencia sobre amenazas. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/query/action | Obtiene indicadores de inteligencia sobre amenazas. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/bulkTag/action | Etiqueta de forma masiva los elementos de la inteligencia sobre amenazas. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/appendTags/action | Anexa etiquetas al indicador de inteligencia sobre amenazas. |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/indicators/replaceTags/action | Reemplazar etiquetas del indicador de inteligencia de amenazas |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/threatIntelligence/queryIndicators/action | Obtiene indicadores de inteligencia sobre amenazas. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/analytics/query/action | Realiza búsquedas mediante el nuevo motor. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Consulta datos de Log Analytics. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/dataSources/read | Obtiene los orígenes de datos en un área de trabajo. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/savedSearches/read | Obtiene una consulta de búsqueda guardada |
> | [Microsoft.OperationsManagement](resource-provider-operations.md#microsoftoperationsmanagement)/solutions/read | Obtiene una solución OMS ya existente |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/read | Ejecuta consultas en los datos del área de trabajo. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/query/*/read |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/dataSources/read | Obtiene los orígenes de datos en un área de trabajo. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/querypacks/*/read |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/read | Lee un libro. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/myworkbooks/read | Lee un libro privado. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/cases/*/Delete |  |
> | [Microsoft.SecurityInsights](resource-provider-operations.md#microsoftsecurityinsights)/incidents/*/Delete |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Microsoft Sentinel Responder",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "name": "3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.SecurityInsights/automationRules/*",
        "Microsoft.SecurityInsights/cases/*",
        "Microsoft.SecurityInsights/incidents/*",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/appendTags/action",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/query/action",
        "Microsoft.SecurityInsights/threatIntelligence/bulkTag/action",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/appendTags/action",
        "Microsoft.SecurityInsights/threatIntelligence/indicators/replaceTags/action",
        "Microsoft.SecurityInsights/threatIntelligence/queryIndicators/action",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.OperationalInsights/querypacks/*/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Insights/myworkbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.SecurityInsights/cases/*/Delete",
        "Microsoft.SecurityInsights/incidents/*/Delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Microsoft Sentinel Responder",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-admin"></a>Administrador de seguridad

Vea y actualice los permisos para Security Center. Tiene los mismos permisos que el rol de lector de seguridad, y también puede actualizar la directiva de seguridad y descartar las alertas y las recomendaciones. [Más información](../security-center/security-center-permissions.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyAssignments/* | Crear y administrar asignaciones de directivas |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyDefinitions/* | Crear y administrar definiciones de directivas |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyExemptions/* | Permite crear y administrar exenciones de directivas. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policySetDefinitions/* | Crear y administrar conjuntos de directivas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | [Microsoft.operationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Consulta datos de Log Analytics. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/* | Crear y administrar las directivas y los componentes de seguridad |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/* |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/defenderSettings/write | Crea o actualiza la configuración de IoT Defender. |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Admin Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "name": "fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Authorization/policyAssignments/*",
        "Microsoft.Authorization/policyDefinitions/*",
        "Microsoft.Authorization/policyExemptions/*",
        "Microsoft.Authorization/policySetDefinitions/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.IoTSecurity/*",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.IoTSecurity/defenderSettings/write"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-assessment-contributor"></a>Colaborador de evaluación de la seguridad

Permite insertar evaluaciones en Security Center.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/assessments/write | Crea o actualiza las evaluaciones de seguridad en la suscripción. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you push assessments to Security Center",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "name": "612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Security/assessments/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Assessment Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-manager-legacy"></a>Administrador de seguridad (heredado)

Se trata de un rol heredado. En su lugar, use el Administrador de seguridad.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/*/read | Leer información de configuración de máquinas virtuales clásicas |
> | [Microsoft.ClassicCompute](resource-provider-operations.md#microsoftclassiccompute)/virtualMachines/*/write | Escribir configuración de máquinas virtuales clásicas |
> | [Microsoft.ClassicNetwork](resource-provider-operations.md#microsoftclassicnetwork)/*/read | Leer información de configuración acerca de la red clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/* | Crear y administrar las directivas y los componentes de seguridad |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "This is a legacy role. Please use Security Administrator instead",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "name": "e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/*/read",
        "Microsoft.ClassicCompute/virtualMachines/*/write",
        "Microsoft.ClassicNetwork/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Manager (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-reader"></a>Lector de seguridad

Vea los permisos para Security Center. Puede ver las recomendaciones, las alertas, una directiva de seguridad y los estados de seguridad, pero no puede realizar cambios. [Más información](../security-center/security-center-permissions.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Lee una alerta de métrica clásica. |
> | [Microsoft.operationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/*/read | Consulta datos de Log Analytics. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/*/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/*/read | Leer directivas y componentes de seguridad |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/*/read |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/*/read |  |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/iotDefenderSettings/packageDownloads/action | Obtiene información de paquetes de IoT Defender que se pueden descargar. |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/iotDefenderSettings/downloadManagerActivation/action | Descarga del archivo de activación del administrador con datos de la cuota de suscripción |
> | [Microsoft.Security](resource-provider-operations.md#microsoftsecurity)/iotSensors/downloadResetPassword/action | Las descargas restablecen el archivo de contraseña para sensores de IoT |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/defenderSettings/packageDownloads/action | Obtiene información de paquetes de IoT Defender que se pueden descargar. |
> | [Microsoft.IoTSecurity](resource-provider-operations.md#microsoftiotsecurity)/defenderSettings/downloadManagerActivation/action | Descarga un archivo de activación del administrador. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "name": "39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*/read",
        "Microsoft.IoTSecurity/*/read",
        "Microsoft.Support/*/read",
        "Microsoft.Security/iotDefenderSettings/packageDownloads/action",
        "Microsoft.Security/iotDefenderSettings/downloadManagerActivation/action",
        "Microsoft.Security/iotSensors/downloadResetPassword/action",
        "Microsoft.IoTSecurity/defenderSettings/packageDownloads/action",
        "Microsoft.IoTSecurity/defenderSettings/downloadManagerActivation/action",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="devops"></a>DevOps


### <a name="devtest-labs-user"></a>Usuario de DevTest Labs

Permite conectarse a sus máquinas virtuales, así como iniciarlas, reiniciarlas y apagarlas, en su instancia de Azure DevTest Labs. [Más información](../devtest-labs/devtest-lab-add-devtest-user.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/availabilitySets/read | Obtiene las propiedades de un conjunto de disponibilidad |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/*/read | Leer las propiedades de una máquina virtual (tamaños de máquinas virtuales, el estado de tiempo de ejecución, extensiones de máquina virtual, etc.) |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/deallocate/action | Apaga la máquina virtual y libera los recursos de proceso |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/read | Obtiene las propiedades de una máquina virtual |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/restart/action | Reinicia la máquina virtual |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/start/action | Inicia la máquina virtual |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/*/read | Leer las propiedades de un laboratorio |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/claimAnyVm/action | Reclama una máquina virtual aleatoria en el laboratorio. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/createEnvironment/action | Crea máquinas virtuales en un laboratorio. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/ensureCurrentUserProfile/action | Asegúrese de que el usuario actual tiene un perfil válido en el laboratorio. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/formulas/delete | Elimina fórmulas. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/formulas/read | Lee fórmulas. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/formulas/write | Agrega o modifica fórmulas. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/policySets/evaluatePolicies/action | Evalúa la directiva de laboratorio. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/virtualMachines/claim/action | Toma la propiedad de una máquina virtual existente |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/virtualmachines/listApplicableSchedules/action | Muestra, si las hay, las programaciones de inicio y detención aplicables. |
> | [Microsoft.DevTestLab](resource-provider-operations.md#microsoftdevtestlab)/labs/virtualMachines/getRdpFileContents/action | Obtiene una cadena que representa el contenido del archivo RDP para la máquina virtual. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/backendAddressPools/join/action | Se une a un grupo de direcciones de back-end del equilibrador de carga. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/loadBalancers/inboundNatRules/join/action | Se une a una regla NAT de entrada del equilibrador de carga. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/*/read | Leer las propiedades de una interfaz de red (por ejemplo, todos los equilibradores de carga de los que forma parte de la interfaz de red) |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/join/action | Une una máquina virtual a una interfaz de red. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/read | Obtiene una definición de interfaz de red.  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/networkInterfaces/write | Crea una interfaz de red o actualiza una interfaz de red existente.  |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/*/read | Leer las propiedades de una dirección IP pública |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/join/action | Se une a una dirección IP pública. No genera alertas. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/publicIPAddresses/read | Obtiene una definición de la dirección ip pública. |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/subnets/join/action | Se une a una red virtual. No genera alertas. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/operations/read | Obtiene o enumera las operaciones de implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Obtiene o enumera implementaciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listKeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | **NotActions** |  |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/virtualMachines/vmSizes/read | Enumera los tamaños disponibles a los que se puede actualizar la máquina virtual |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you connect, start, restart, and shutdown your virtual machines in your Azure DevTest Labs.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/76283e04-6283-4c54-8f91-bcf1374a3c64",
  "name": "76283e04-6283-4c54-8f91-bcf1374a3c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/read",
        "Microsoft.Compute/virtualMachines/*/read",
        "Microsoft.Compute/virtualMachines/deallocate/action",
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Compute/virtualMachines/restart/action",
        "Microsoft.Compute/virtualMachines/start/action",
        "Microsoft.DevTestLab/*/read",
        "Microsoft.DevTestLab/labs/claimAnyVm/action",
        "Microsoft.DevTestLab/labs/createEnvironment/action",
        "Microsoft.DevTestLab/labs/ensureCurrentUserProfile/action",
        "Microsoft.DevTestLab/labs/formulas/delete",
        "Microsoft.DevTestLab/labs/formulas/read",
        "Microsoft.DevTestLab/labs/formulas/write",
        "Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action",
        "Microsoft.DevTestLab/labs/virtualMachines/claim/action",
        "Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action",
        "Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/networkInterfaces/*/read",
        "Microsoft.Network/networkInterfaces/join/action",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/publicIPAddresses/*/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listKeys/action"
      ],
      "notActions": [
        "Microsoft.Compute/virtualMachines/vmSizes/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DevTest Labs User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="lab-creator"></a>Creador de laboratorio

Permite crear nuevos laboratorios en las cuentas de Azure Lab. [Más información](../lab-services/add-lab-creator.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labAccounts/*/read |  |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labAccounts/createLab/action | Crea un laboratorio en una cuenta de laboratorio. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labAccounts/getPricingAndAvailability/action | Obtiene los precios y la disponibilidad de combinaciones de tamaños, zonas geográficas y sistemas operativos para la cuenta de laboratorio. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labAccounts/getRestrictionsAndUsage/action | Obtiene el uso y las restricciones de núcleos de esta suscripción. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labPlans/images/read | Obtiene las propiedades de una imagen. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labPlans/read | Obtiene las propiedades de un plan de laboratorio. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labPlans/saveImage/action | Crea una imagen a partir de una máquina virtual en la galería asociada al plan de laboratorio. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labs/read | Obtiene las propiedades de un laboratorio. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labs/schedules/read | Obtiene las propiedades de una programación. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labs/users/read | Obtiene las propiedades de un usuario. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labs/virtualMachines/read | Obtiene las propiedades de una máquina virtual. |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/locations/usages/read | Obtener uso en una ubicación |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/skus/read | Obtiene las propiedades de una SKU de Lab Services. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.LabServices](resource-provider-operations.md#microsoftlabservices)/labPlans/createLab/action | Crea un nuevo laboratorio a partir de un plan de laboratorio. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create new labs under your Azure Lab Accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "name": "b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.LabServices/labAccounts/*/read",
        "Microsoft.LabServices/labAccounts/createLab/action",
        "Microsoft.LabServices/labAccounts/getPricingAndAvailability/action",
        "Microsoft.LabServices/labAccounts/getRestrictionsAndUsage/action",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.LabServices/labPlans/images/read",
        "Microsoft.LabServices/labPlans/read",
        "Microsoft.LabServices/labPlans/saveImage/action",
        "Microsoft.LabServices/labs/read",
        "Microsoft.LabServices/labs/schedules/read",
        "Microsoft.LabServices/labs/users/read",
        "Microsoft.LabServices/labs/virtualMachines/read",
        "Microsoft.LabServices/locations/usages/read",
        "Microsoft.LabServices/skus/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.LabServices/labPlans/createLab/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Lab Creator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="monitor"></a>Supervisión


### <a name="application-insights-component-contributor"></a>Colaborador de componentes de Application Insights

Puede administrar los componentes de Application Insights. [Más información](../azure-monitor/app/resources-roles-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Crear y administrar reglas de alertas clásicas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/generateLiveToken/read | Obtención del token de las métricas en vivo |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricAlerts/* | Crear y administrar nuevas reglas de alertas |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/* | Crear y administrar componentes de Insights |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/scheduledqueryrules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/topology/read | Leer topología |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/transactions/read | Leer transacciones |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/webtests/* | Crear y administrar pruebas web de Insights |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage Application Insights components",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e",
  "name": "ae349356-3a1b-4a5e-921d-050484c6347e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/generateLiveToken/read",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/scheduledqueryrules/*",
        "Microsoft.Insights/topology/read",
        "Microsoft.Insights/transactions/read",
        "Microsoft.Insights/webtests/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Component Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="application-insights-snapshot-debugger"></a>Depurador de instantáneas de Application Insights

Concede permiso al usuario para ver y descargar las instantáneas de depuración que se recopilan con Snapshot Debugger de Application Insights. Tenga en cuenta que estos permisos no se incluyen en los roles [Propietario](#owner) ni [Colaborador](#contributor). Si concede el rol Depurador de instantáneas de Application Insights a los usuarios, debe concederlo directamente al usuario. El rol no se reconoce cuando se agrega a un rol personalizado. [Más información](../azure-monitor/app/snapshot-debugger.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/*/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives user permission to use Application Insights Snapshot Debugger features",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "name": "08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Snapshot Debugger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-contributor"></a>Colaborador de supervisión

Puede leer todos los datos de supervisión y editar la configuración de supervisión. Consulte también [Introducción a roles, permisos y seguridad con Azure Monitor](../azure-monitor/roles-permissions-security.md#built-in-monitoring-roles). [Más información](../azure-monitor/roles-permissions-security.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/alerts/* |  |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/alertsSummary/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/actiongroups/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/activityLogAlerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/AlertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/components/* | Crear y administrar componentes de Insights |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/dataCollectionEndpoints/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/dataCollectionRules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/dataCollectionRuleAssociations/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/DiagnosticSettings/* | Crea, actualiza o lee la configuración de diagnóstico de Analysis Server. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/eventtypes/* | Enumerar eventos del registro de actividades (eventos de administración) de una suscripción. Este permiso es aplicable para el acceso mediante programación y mediante el portal al registro de actividades. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/LogDefinitions/* | Este permiso es necesario para los usuarios que necesitan acceder a registros de actividades a través del portal. Enumere las categorías de registro del registro de actividad. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/metricalerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/MetricDefinitions/* | Leer definiciones de métrica (lista de tipos de métricas disponibles para un recurso). |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Metrics/* | Leer las métricas de un recurso. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Register/Action | Registra el proveedor de Microsoft Insights. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/scheduledqueryrules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/webtests/* | Crear y administrar pruebas web de Insights |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/privateLinkScopes/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/privateLinkScopeOperationStatuses/* |  |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/write | Crea una nueva área de trabajo o vincula a un área de trabajo que ya existe proporcionando el identificador de cliente de esta. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/intelligencepacks/* | Lee, escribe o elimina paquetes de soluciones de Log Analytics. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/savedSearches/* | Lee, escribe o elimina búsquedas guardadas de Log Analytics. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/search/action | Ejecuta una consulta de búsqueda |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/sharedKeys/action | Recupera las claves compartidas del área de trabajo. Estas claves se utilizan para conectar los agentes de Microsoft Operational Insights al área de trabajo. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/storageinsightconfigs/* | Lee, escribe o elimina configuraciones de visión de almacenamiento de Log Analytics. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.WorkloadMonitor](resource-provider-operations.md#microsoftworkloadmonitor)/monitors/* | Obtenga más información sobre los monitores de estado de las máquinas virtuales invitadas. |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/smartDetectorAlertRules/* |  |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/actionRules/* |  |
> | [Microsoft.AlertsManagement](resource-provider-operations.md#microsoftalertsmanagement)/smartGroups/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data and update monitoring settings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "name": "749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.AlertsManagement/alerts/*",
        "Microsoft.AlertsManagement/alertsSummary/*",
        "Microsoft.Insights/actiongroups/*",
        "Microsoft.Insights/activityLogAlerts/*",
        "Microsoft.Insights/AlertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/dataCollectionEndpoints/*",
        "Microsoft.Insights/dataCollectionRules/*",
        "Microsoft.Insights/dataCollectionRuleAssociations/*",
        "Microsoft.Insights/DiagnosticSettings/*",
        "Microsoft.Insights/eventtypes/*",
        "Microsoft.Insights/LogDefinitions/*",
        "Microsoft.Insights/metricalerts/*",
        "Microsoft.Insights/MetricDefinitions/*",
        "Microsoft.Insights/Metrics/*",
        "Microsoft.Insights/Register/Action",
        "Microsoft.Insights/scheduledqueryrules/*",
        "Microsoft.Insights/webtests/*",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Insights/privateLinkScopes/*",
        "Microsoft.Insights/privateLinkScopeOperationStatuses/*",
        "Microsoft.OperationalInsights/workspaces/write",
        "Microsoft.OperationalInsights/workspaces/intelligencepacks/*",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.OperationalInsights/workspaces/sharedKeys/action",
        "Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*",
        "Microsoft.Support/*",
        "Microsoft.WorkloadMonitor/monitors/*",
        "Microsoft.AlertsManagement/smartDetectorAlertRules/*",
        "Microsoft.AlertsManagement/actionRules/*",
        "Microsoft.AlertsManagement/smartGroups/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-metrics-publisher"></a>Supervisión del publicador de métricas

Permite publicar las métricas de los recursos de Azure. [Más información](../azure-monitor/insights/container-insights-update-metrics.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Register/Action | Registra el proveedor de Microsoft Insights. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/Metrics/Write | Escribe métricas. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Enables publishing metrics against Azure resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c390055eb",
  "name": "3913510d-42f4-4e42-8a64-420c390055eb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/Register/Action",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Insights/Metrics/Write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Metrics Publisher",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-reader"></a>Lector de supervisión

Puede leer todos los datos de supervisión (métricas, registros, etc.). Consulte también [Introducción a roles, permisos y seguridad con Azure Monitor](../azure-monitor/roles-permissions-security.md#built-in-monitoring-roles). [Más información](../azure-monitor/roles-permissions-security.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/search/action | Ejecuta una consulta de búsqueda |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "name": "43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-contributor"></a>Colaborador de libros

Puede guardar los libros compartidos. [Más información](../sentinel/tutorial-monitor-your-data.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/write | Crea o actualiza un libro. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/delete | Elimina un libro. |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/workbooks/read | Lee un libro. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can save shared workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "name": "e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/workbooks/write",
        "Microsoft.Insights/workbooks/delete",
        "Microsoft.Insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-reader"></a>Lector de libros

Puede leer libros. [Más información](../sentinel/tutorial-monitor-your-data.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [microsoft.insights](resource-provider-operations.md#microsoftinsights)/workbooks/read | Lee un libro. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "name": "b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "permissions": [
    {
      "actions": [
        "microsoft.insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="management--governance"></a>Administración y gobernanza


### <a name="automation-contributor"></a>Colaborador de Automation

Administra los recursos de Azure Automation y otros recursos mediante Azure Automation. [Más información](../automation/automation-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/* |  |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/ActionGroups/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/ActivityLogAlerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/MetricAlerts/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/ScheduledQueryRules/* |  |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/diagnosticSettings/* | Crea, actualiza o lee la configuración de diagnóstico de Analysis Server. |
> | [Microsoft.OperationalInsights](resource-provider-operations.md#microsoftoperationalinsights)/workspaces/sharedKeys/action | Recupera las claves compartidas del área de trabajo. Estas claves se utilizan para conectar los agentes de Microsoft Operational Insights al área de trabajo. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Manage azure automation resources and other resources using azure automation.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f353d9bd-d4a6-484e-a77a-8050b599b867",
  "name": "f353d9bd-d4a6-484e-a77a-8050b599b867",
  "permissions": [
    {
      "actions": [
        "Microsoft.Automation/automationAccounts/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/ActionGroups/*",
        "Microsoft.Insights/ActivityLogAlerts/*",
        "Microsoft.Insights/MetricAlerts/*",
        "Microsoft.Insights/ScheduledQueryRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.OperationalInsights/workspaces/sharedKeys/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-job-operator"></a>Operador de trabajos de Automation

Permite crear y administrar trabajos con los runbooks de Automation. [Más información](../automation/automation-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/hybridRunbookWorkerGroups/read | Lee los recursos de Hybrid Runbook Worker |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/read | Obtiene un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/resume/action | Reanuda un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/stop/action | Detiene un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/streams/read | Obtiene un flujo de trabajos de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/suspend/action | Suspende un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/write | Crea un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/output/read | Obtiene la salida de un trabajo |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and Manage Jobs using Automation Runbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "name": "4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Job Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-operator"></a>Operador de Automation

Los operadores de automatización pueden iniciar, detener, suspender y reanudar trabajos. [Más información](../automation/automation-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/hybridRunbookWorkerGroups/read | Lee los recursos de Hybrid Runbook Worker |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/read | Obtiene un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/resume/action | Reanuda un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/stop/action | Detiene un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/streams/read | Obtiene un flujo de trabajos de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/suspend/action | Suspende un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/write | Crea un trabajo de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobSchedules/read | Obtiene una programación de trabajos de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobSchedules/write | Crea una programación de trabajos de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/linkedWorkspace/read | Obtiene el área de trabajo vinculada a la cuenta de Automation. |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/read | Obtiene una cuenta de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/runbooks/read | Obtiene un runbook de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/schedules/read | Obtiene un recurso de programación de Azure Automation |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/schedules/write | Crea o actualiza un recurso de programación de Azure Automation |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/jobs/output/read | Obtiene la salida de un trabajo |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Automation Operators are able to start, stop, suspend, and resume jobs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d3881f73-407a-4167-8283-e981cbba0404",
  "name": "d3881f73-407a-4167-8283-e981cbba0404",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobSchedules/read",
        "Microsoft.Automation/automationAccounts/jobSchedules/write",
        "Microsoft.Automation/automationAccounts/linkedWorkspace/read",
        "Microsoft.Automation/automationAccounts/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Automation/automationAccounts/schedules/read",
        "Microsoft.Automation/automationAccounts/schedules/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-runbook-operator"></a>Operador de runbooks de Automation

Permite leer las propiedades de runbook para poder crear trabajos del runbook. [Más información](../automation/automation-role-based-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Automation](resource-provider-operations.md#microsoftautomation)/automationAccounts/runbooks/read | Obtiene un runbook de Azure Automation |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read Runbook properties - to be able to create Jobs of the runbook.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "name": "5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Runbook Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-enabled-kubernetes-cluster-user-role"></a>Rol de usuario del clúster de Kubernetes habilitado para Azure Arc

Permite enumerar las acciones de credenciales de usuario de clúster.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/listClusterUserCredentials/action | Enumera la credencial de usuario de clúster. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster user credentials action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00493d72-78f6-4148-b6c5-d3ce8e4799dd",
  "name": "00493d72-78f6-4148-b6c5-d3ce8e4799dd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Kubernetes/connectedClusters/listClusterUserCredentials/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Enabled Kubernetes Cluster User Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-kubernetes-admin"></a>Administrador de Azure Arc Kubernetes

Permite administrar todos los recursos en un clúster o espacio de nombres, excepto actualizar o eliminar cuotas de recursos y espacios de nombres. [Más información](../azure-arc/kubernetes/azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/controllerrevisions/read | Lee controllerrevisions. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/daemonsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/deployments/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/replicasets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/statefulsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/authorization.k8s.io/localsubjectaccessreviews/write | Escribe localsubjectaccessreviews. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/autoscaling/horizontalpodautoscalers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/cronjobs/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/jobs/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/configmaps/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/endpoints/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events.k8s.io/events/read | Lee eventos. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events/read | Lee eventos. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/daemonsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/deployments/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/ingresses/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/networkpolicies/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/replicasets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/limitranges/read | Lee limitranges. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/namespaces/read | Lee espacios de nombres. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/ingresses/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/networkpolicies/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/persistentvolumeclaims/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/pods/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/policy/poddisruptionbudgets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/rbac.authorization.k8s.io/rolebindings/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/rbac.authorization.k8s.io/roles/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/resourcequotas/read | Lee resourcequotas. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/secrets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/serviceaccounts/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/services/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage all resources under cluster/namespace, except update or delete resource quotas and namespaces.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/dffb1e0c-446f-4dde-a09f-99eb5cc68b96",
  "name": "dffb1e0c-446f-4dde-a09f-99eb5cc68b96",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Kubernetes/connectedClusters/apps/controllerrevisions/read",
        "Microsoft.Kubernetes/connectedClusters/apps/daemonsets/*",
        "Microsoft.Kubernetes/connectedClusters/apps/deployments/*",
        "Microsoft.Kubernetes/connectedClusters/apps/replicasets/*",
        "Microsoft.Kubernetes/connectedClusters/apps/statefulsets/*",
        "Microsoft.Kubernetes/connectedClusters/authorization.k8s.io/localsubjectaccessreviews/write",
        "Microsoft.Kubernetes/connectedClusters/autoscaling/horizontalpodautoscalers/*",
        "Microsoft.Kubernetes/connectedClusters/batch/cronjobs/*",
        "Microsoft.Kubernetes/connectedClusters/batch/jobs/*",
        "Microsoft.Kubernetes/connectedClusters/configmaps/*",
        "Microsoft.Kubernetes/connectedClusters/endpoints/*",
        "Microsoft.Kubernetes/connectedClusters/events.k8s.io/events/read",
        "Microsoft.Kubernetes/connectedClusters/events/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/daemonsets/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/deployments/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/ingresses/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/networkpolicies/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/replicasets/*",
        "Microsoft.Kubernetes/connectedClusters/limitranges/read",
        "Microsoft.Kubernetes/connectedClusters/namespaces/read",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/ingresses/*",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/networkpolicies/*",
        "Microsoft.Kubernetes/connectedClusters/persistentvolumeclaims/*",
        "Microsoft.Kubernetes/connectedClusters/pods/*",
        "Microsoft.Kubernetes/connectedClusters/policy/poddisruptionbudgets/*",
        "Microsoft.Kubernetes/connectedClusters/rbac.authorization.k8s.io/rolebindings/*",
        "Microsoft.Kubernetes/connectedClusters/rbac.authorization.k8s.io/roles/*",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/*",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/*",
        "Microsoft.Kubernetes/connectedClusters/resourcequotas/read",
        "Microsoft.Kubernetes/connectedClusters/secrets/*",
        "Microsoft.Kubernetes/connectedClusters/serviceaccounts/*",
        "Microsoft.Kubernetes/connectedClusters/services/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Kubernetes Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-kubernetes-cluster-admin"></a>Administrador de clústeres de Azure Arc Kubernetes

Permite administrar todos los recursos del clúster. [Más información](../azure-arc/kubernetes/azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage all resources in the cluster.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8393591c-06b9-48a2-a542-1bd6b377f6a2",
  "name": "8393591c-06b9-48a2-a542-1bd6b377f6a2",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Kubernetes/connectedClusters/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Kubernetes Cluster Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-kubernetes-viewer"></a>Visor de Azure Arc Kubernetes

Permite ver todos los recursos del clúster o espacio de nombres, excepto los secretos. [Más información](../azure-arc/kubernetes/azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/controllerrevisions/read | Lee controllerrevisions. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/daemonsets/read | Lee daemonsets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/deployments/read | Lee implementaciones. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/replicasets/read | Lee replicasets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/statefulsets/read | Lee statefulsets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/autoscaling/horizontalpodautoscalers/read | Lee horizontalpodautoscalers. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/cronjobs/read | Lee cronjobs. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/jobs/read | Lee trabajos. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/configmaps/read | Lee configmaps. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/endpoints/read | Lee puntos de conexión. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events.k8s.io/events/read | Lee eventos. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events/read | Lee eventos. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/daemonsets/read | Lee daemonsets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/deployments/read | Lee implementaciones. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/ingresses/read | Lee entradas. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/networkpolicies/read | Lee networkpolicies. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/replicasets/read | Lee replicasets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/limitranges/read | Lee limitranges. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/namespaces/read | Lee espacios de nombres. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/ingresses/read | Lee entradas. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/networkpolicies/read | Lee networkpolicies. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/persistentvolumeclaims/read | Lee persistentvolumeclaims. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/pods/read | Lee pods. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/policy/poddisruptionbudgets/read | Lee poddisruptionbudgets. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/read | Lee replicationcontrollers. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/read | Lee replicationcontrollers. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/resourcequotas/read | Lee resourcequotas. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/serviceaccounts/read | Lee serviceaccounts. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/services/read | Lee servicios. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view all resources in cluster/namespace, except secrets.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/63f0a09d-1495-4db4-a681-037d84835eb4",
  "name": "63f0a09d-1495-4db4-a681-037d84835eb4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Kubernetes/connectedClusters/apps/controllerrevisions/read",
        "Microsoft.Kubernetes/connectedClusters/apps/daemonsets/read",
        "Microsoft.Kubernetes/connectedClusters/apps/deployments/read",
        "Microsoft.Kubernetes/connectedClusters/apps/replicasets/read",
        "Microsoft.Kubernetes/connectedClusters/apps/statefulsets/read",
        "Microsoft.Kubernetes/connectedClusters/autoscaling/horizontalpodautoscalers/read",
        "Microsoft.Kubernetes/connectedClusters/batch/cronjobs/read",
        "Microsoft.Kubernetes/connectedClusters/batch/jobs/read",
        "Microsoft.Kubernetes/connectedClusters/configmaps/read",
        "Microsoft.Kubernetes/connectedClusters/endpoints/read",
        "Microsoft.Kubernetes/connectedClusters/events.k8s.io/events/read",
        "Microsoft.Kubernetes/connectedClusters/events/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/daemonsets/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/deployments/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/ingresses/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/networkpolicies/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/replicasets/read",
        "Microsoft.Kubernetes/connectedClusters/limitranges/read",
        "Microsoft.Kubernetes/connectedClusters/namespaces/read",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/ingresses/read",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/networkpolicies/read",
        "Microsoft.Kubernetes/connectedClusters/persistentvolumeclaims/read",
        "Microsoft.Kubernetes/connectedClusters/pods/read",
        "Microsoft.Kubernetes/connectedClusters/policy/poddisruptionbudgets/read",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/read",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/read",
        "Microsoft.Kubernetes/connectedClusters/resourcequotas/read",
        "Microsoft.Kubernetes/connectedClusters/serviceaccounts/read",
        "Microsoft.Kubernetes/connectedClusters/services/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Kubernetes Viewer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-arc-kubernetes-writer"></a>Escritor de Azure Arc Kubernetes

Le permite actualizar todo el contenido del clúster o el espacio de nombres, excepto los roles (del clúster) y los enlaces de roles (del clúster). [Más información](../azure-arc/kubernetes/azure-rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/controllerrevisions/read | Lee controllerrevisions. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/daemonsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/deployments/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/replicasets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/apps/statefulsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/autoscaling/horizontalpodautoscalers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/cronjobs/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/batch/jobs/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/configmaps/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/endpoints/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events.k8s.io/events/read | Lee eventos. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/events/read | Lee eventos. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/daemonsets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/deployments/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/ingresses/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/networkpolicies/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/extensions/replicasets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/limitranges/read | Lee limitranges. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/namespaces/read | Lee espacios de nombres. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/ingresses/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/networking.k8s.io/networkpolicies/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/persistentvolumeclaims/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/pods/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/policy/poddisruptionbudgets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/replicationcontrollers/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/resourcequotas/read | Lee resourcequotas. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/secrets/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/serviceaccounts/* |  |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/services/* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you update everything in cluster/namespace, except (cluster)roles and (cluster)role bindings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5b999177-9696-4545-85c7-50de3797e5a1",
  "name": "5b999177-9696-4545-85c7-50de3797e5a1",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Kubernetes/connectedClusters/apps/controllerrevisions/read",
        "Microsoft.Kubernetes/connectedClusters/apps/daemonsets/*",
        "Microsoft.Kubernetes/connectedClusters/apps/deployments/*",
        "Microsoft.Kubernetes/connectedClusters/apps/replicasets/*",
        "Microsoft.Kubernetes/connectedClusters/apps/statefulsets/*",
        "Microsoft.Kubernetes/connectedClusters/autoscaling/horizontalpodautoscalers/*",
        "Microsoft.Kubernetes/connectedClusters/batch/cronjobs/*",
        "Microsoft.Kubernetes/connectedClusters/batch/jobs/*",
        "Microsoft.Kubernetes/connectedClusters/configmaps/*",
        "Microsoft.Kubernetes/connectedClusters/endpoints/*",
        "Microsoft.Kubernetes/connectedClusters/events.k8s.io/events/read",
        "Microsoft.Kubernetes/connectedClusters/events/read",
        "Microsoft.Kubernetes/connectedClusters/extensions/daemonsets/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/deployments/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/ingresses/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/networkpolicies/*",
        "Microsoft.Kubernetes/connectedClusters/extensions/replicasets/*",
        "Microsoft.Kubernetes/connectedClusters/limitranges/read",
        "Microsoft.Kubernetes/connectedClusters/namespaces/read",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/ingresses/*",
        "Microsoft.Kubernetes/connectedClusters/networking.k8s.io/networkpolicies/*",
        "Microsoft.Kubernetes/connectedClusters/persistentvolumeclaims/*",
        "Microsoft.Kubernetes/connectedClusters/pods/*",
        "Microsoft.Kubernetes/connectedClusters/policy/poddisruptionbudgets/*",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/*",
        "Microsoft.Kubernetes/connectedClusters/replicationcontrollers/*",
        "Microsoft.Kubernetes/connectedClusters/resourcequotas/read",
        "Microsoft.Kubernetes/connectedClusters/secrets/*",
        "Microsoft.Kubernetes/connectedClusters/serviceaccounts/*",
        "Microsoft.Kubernetes/connectedClusters/services/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Arc Kubernetes Writer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-onboarding"></a>Incorporación de Azure Connected Machine

Puede incorporar máquinas conectadas a Azure. [Más información](../azure-arc/servers/onboard-service-principal.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/read | Lee cualquier máquina de Azure Arc. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/write | Escribe las máquinas de Azure Arc. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/privateLinkScopes/read | Lee cualquier privateLinkScopes de Azure Arc. |
> | [Microsoft.GuestConfiguration](resource-provider-operations.md#microsoftguestconfiguration)/guestConfigurationAssignments/read | Obtiene la asignación de configuración de invitado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "name": "b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.HybridCompute/privateLinkScopes/read",
        "Microsoft.GuestConfiguration/guestConfigurationAssignments/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Onboarding",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-resource-administrator"></a>Administrador de recursos de Azure Connected Machine

Puede leer, escribir, eliminar y volver a incorporar máquinas conectadas a Azure.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/read | Lee cualquier máquina de Azure Arc. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/write | Escribe las máquinas de Azure Arc. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/delete | Elimina las máquinas de Azure Arc. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/UpgradeExtensions/action | Actualiza extensiones en máquinas de Azure Arc |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/extensions/read | Lee todas las extensiones de Azure Arc. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/extensions/write | Instala o actualiza las extensiones de Azure Arc. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/machines/extensions/delete | Elimina las extensiones de Azure Arc. |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/privateLinkScopes/* |  |
> | [Microsoft.HybridCompute](resource-provider-operations.md#microsofthybridcompute)/*/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read, write, delete and re-onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cd570a14-e51a-42ad-bac8-bafd67325302",
  "name": "cd570a14-e51a-42ad-bac8-bafd67325302",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.HybridCompute/machines/delete",
        "Microsoft.HybridCompute/machines/UpgradeExtensions/action",
        "Microsoft.HybridCompute/machines/extensions/read",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.HybridCompute/machines/extensions/delete",
        "Microsoft.HybridCompute/privateLinkScopes/*",
        "Microsoft.HybridCompute/*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Resource Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="billing-reader"></a>Lector de facturación

Permite acceso de lectura a los datos de facturación. [Más información](../cost-management-billing/manage/manage-billing-access.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/*/read | Leer la información de facturación |
> | [Microsoft.Commerce](resource-provider-operations.md#microsoftcommerce)/*/read |  |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/*/read |  |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | [Microsoft.CostManagement](resource-provider-operations.md#microsoftcostmanagement)/*/read |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to billing data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "name": "fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Billing/*/read",
        "Microsoft.Commerce/*/read",
        "Microsoft.Consumption/*/read",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Billing Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-contributor"></a>Colaborador de plano técnico

Puede administrar las definiciones del plano técnico, pero no asignarlas. [Más información](../governance/blueprints/overview.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Blueprint](resource-provider-operations.md#microsoftblueprint)/blueprints/* | Crea y administra definiciones de plano técnico o artefactos de plano técnico. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage blueprint definitions, but not assign them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/41077137-e803-4205-871c-5a86e6a753b4",
  "name": "41077137-e803-4205-871c-5a86e6a753b4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprints/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-operator"></a>Operador del plano técnico

Puede asignar los planos técnicos publicados existentes, pero no puede crear nuevos. Tenga en cuenta que esto solo funciona si la asignación se realiza con una identidad administrada asignada por el usuario. [Más información](../governance/blueprints/overview.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Blueprint](resource-provider-operations.md#microsoftblueprint)/blueprintAssignments/* | Crea y administra asignaciones de plano técnico. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can assign existing published blueprints, but cannot create new blueprints. NOTE: this only works if the assignment is done with a user-assigned managed identity.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/437d2ced-4a38-4302-8479-ed2bcb43d090",
  "name": "437d2ced-4a38-4302-8479-ed2bcb43d090",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprintAssignments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-contributor"></a>Colaborador de Cost Management

Puede ver los costos y administrar la configuración de estos (por ejemplo, presupuestos, exportaciones). [Más información](../cost-management-billing/costs/understand-work-scopes.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/* |  |
> | [Microsoft.CostManagement](resource-provider-operations.md#microsoftcostmanagement)/* |  |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/billingPeriods/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Advisor](resource-provider-operations.md#microsoftadvisor)/configurations/read | Obtener configuraciones |
> | [Microsoft.Advisor](resource-provider-operations.md#microsoftadvisor)/recommendations/read | Lee las recomendaciones |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/billingProperty/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view costs and manage cost configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/434105ed-43f6-45c7-a02f-909b2ba83430",
  "name": "434105ed-43f6-45c7-a02f-909b2ba83430",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*",
        "Microsoft.CostManagement/*",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Billing/billingProperty/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-reader"></a>Lector de Cost Management

Puede ver los datos de costo y la configuración (por ejemplo, presupuestos, exportaciones). [Más información](../cost-management-billing/costs/understand-work-scopes.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/*/read |  |
> | [Microsoft.CostManagement](resource-provider-operations.md#microsoftcostmanagement)/*/read |  |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/billingPeriods/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Advisor](resource-provider-operations.md#microsoftadvisor)/configurations/read | Obtener configuraciones |
> | [Microsoft.Advisor](resource-provider-operations.md#microsoftadvisor)/recommendations/read | Lee las recomendaciones |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | [Microsoft.Billing](resource-provider-operations.md#microsoftbilling)/billingProperty/read |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view cost data and configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/72fafb9e-0641-4937-9268-a91bfd8191a3",
  "name": "72fafb9e-0641-4937-9268-a91bfd8191a3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Billing/billingProperty/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hierarchy-settings-administrator"></a>Administrador de configuración de jerarquía

Permite a los usuarios editar y eliminar la configuración de jerarquía.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/settings/write | Crea o actualiza la configuración de jerarquía del grupo de administración. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/settings/delete | Elimina la configuración de jerarquía del grupo de administración. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows users to edit and delete Hierarchy Settings",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/350f8d15-c687-4448-8ae1-157740a3936d",
  "name": "350f8d15-c687-4448-8ae1-157740a3936d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/settings/write",
        "Microsoft.Management/managementGroups/settings/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Hierarchy Settings Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="kubernetes-cluster---azure-arc-onboarding"></a>Clúster de Kubernetes: incorporación de Azure Arc

Definición de roles para permitir crear el recurso connectedClusters a cualquier usuario o servicio [Más información](../azure-arc/kubernetes/connect-cluster.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/write | Crea o actualiza una implementación. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/operationresults/read | Obtiene los resultados de la operación de suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/Write | Escribe connectedClusters. |
> | [Microsoft.Kubernetes](resource-provider-operations.md#microsoftkubernetes)/connectedClusters/read | Lee connectedClusters. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Role definition to authorize any user/service to create connectedClusters resource",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/34e09817-6cbe-4d01-b1a2-e0eac5743d41",
  "name": "34e09817-6cbe-4d01-b1a2-e0eac5743d41",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/write",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Kubernetes/connectedClusters/Write",
        "Microsoft.Kubernetes/connectedClusters/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Kubernetes Cluster - Azure Arc Onboarding",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="kubernetes-extension-contributor"></a>Kubernetes Extension Contributor

Puede crear, actualizar, obtener, enumerar y eliminar extensiones de Kubernetes y obtener operaciones asincrónicas de extensión.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.KubernetesConfiguration](resource-provider-operations.md#microsoftkubernetesconfiguration)/extensions/write | Crea o actualiza un recurso de extensión. |
> | [Microsoft.KubernetesConfiguration](resource-provider-operations.md#microsoftkubernetesconfiguration)/extensions/read | Obtiene el recurso de instancia de extensión. |
> | [Microsoft.KubernetesConfiguration](resource-provider-operations.md#microsoftkubernetesconfiguration)/extensions/delete | Elimina el recurso de instancia de extensión. |
> | [Microsoft.KubernetesConfiguration](resource-provider-operations.md#microsoftkubernetesconfiguration)/extensions/operations/read | Obtiene el estado de la operación asincrónica. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can create, update, get, list and delete Kubernetes Extensions, and get extension async operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/85cb6faf-e071-4c9b-8136-154b5a04f717",
  "name": "85cb6faf-e071-4c9b-8136-154b5a04f717",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.KubernetesConfiguration/extensions/write",
        "Microsoft.KubernetesConfiguration/extensions/read",
        "Microsoft.KubernetesConfiguration/extensions/delete",
        "Microsoft.KubernetesConfiguration/extensions/operations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Kubernetes Extension Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-contributor-role"></a>Rol Colaborador de la aplicación administrada

Permite crear recursos de aplicaciones administradas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/applications/* |  |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/register/action | Permite registrarse en Solutions. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for creating managed application resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/641177b8-a67a-45b9-a033-47bc880bb21e",
  "name": "641177b8-a67a-45b9-a033-47bc880bb21e",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/*",
        "Microsoft.Solutions/register/action",
        "Microsoft.Resources/subscriptions/resourceGroups/*",
        "Microsoft.Resources/deployments/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Contributor Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-operator-role"></a>Rol de operador de aplicación administrada

Permite leer y realizar acciones en los recursos de aplicación administrada.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/applications/read | Recupera una lista de aplicaciones. |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/*/action |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and perform actions on Managed Application resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "name": "c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/read",
        "Microsoft.Solutions/*/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-applications-reader"></a>Lector de aplicaciones administradas

Le permite leer los recursos de una aplicación administrada y solicitar acceso JIT.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Solutions](resource-provider-operations.md#microsoftsolutions)/jitRequests/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read resources in a managed app and request JIT access.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "name": "b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Solutions/jitRequests/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Applications Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-services-registration-assignment-delete-role"></a>Rol para eliminar la asignación de registros de servicios administrados

El rol para eliminar la asignación de registros de servicios administrados permite que los usuarios que administran el inquilino eliminen la asignación de registros asignada a su inquilino. [Más información](../lighthouse/how-to/remove-delegation.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.ManagedServices](resource-provider-operations.md#microsoftmanagedservices)/registrationAssignments/read | Recupera una lista de las asignaciones del registro de servicios administrados. |
> | [Microsoft.ManagedServices](resource-provider-operations.md#microsoftmanagedservices)/registrationAssignments/delete | Quita la asignación del registro de servicios administrados. |
> | [Microsoft.ManagedServices](resource-provider-operations.md#microsoftmanagedservices)/operationStatuses/read | Lee el estado de la operación de los recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Managed Services Registration Assignment Delete Role allows the managing tenant users to delete the registration assignment assigned to their tenant.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/91c1777a-f3dc-4fae-b103-61d183457e46",
  "name": "91c1777a-f3dc-4fae-b103-61d183457e46",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedServices/registrationAssignments/read",
        "Microsoft.ManagedServices/registrationAssignments/delete",
        "Microsoft.ManagedServices/operationStatuses/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Services Registration assignment Delete Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-contributor"></a>Colaborador de grupo de administración

Rol de colaborador de grupo de administración. [Más información](../governance/management-groups/overview.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/delete | Elimina un grupo de administración. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/subscriptions/delete | Anula la asociación de la suscripción con el grupo de administración. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/subscriptions/write | Asocia la suscripción existente con el grupo de administración. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/write | Crea o actualiza un grupo de administración. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/subscriptions/read | Muestra la suscripción en el grupo de administración dado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Contributor Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "name": "5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/delete",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Management/managementGroups/subscriptions/delete",
        "Microsoft.Management/managementGroups/subscriptions/write",
        "Microsoft.Management/managementGroups/write",
        "Microsoft.Management/managementGroups/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-reader"></a>Lector de grupo de administración

Rol de lector de grupo de administración

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/read | Enumera los grupos de administración del usuario autenticado. |
> | [Microsoft.Management](resource-provider-operations.md#microsoftmanagement)/managementGroups/subscriptions/read | Muestra la suscripción en el grupo de administración dado. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ac63b705-f282-497d-ac71-919bf39d939d",
  "name": "ac63b705-f282-497d-ac71-919bf39d939d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Management/managementGroups/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="new-relic-apm-account-contributor"></a>Colaborador de la cuenta de NewRelic APM

Le permite administrar las aplicaciones y cuentas de Application Performance Management de New Relic, pero no acceder a ellas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | NewRelic.APM/accounts/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage New Relic Application Performance Management accounts and applications, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d28c62d-5b37-4476-8438-e587778df237",
  "name": "5d28c62d-5b37-4476-8438-e587778df237",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "NewRelic.APM/accounts/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "New Relic APM Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="policy-insights-data-writer-preview"></a>Escritor de datos de Policy Insights (versión preliminar)

Permite el acceso de lectura a las directivas de los recursos y el acceso de escritura a los eventos de directiva de los componentes de los recursos. [Más información](../governance/policy/concepts/policy-for-kubernetes.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyassignments/read | Obtiene información sobre una asignación de directiva. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policydefinitions/read | Obtiene información sobre una definición de directiva. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyexemptions/read | Obtiene información sobre una la exención de una directiva. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policysetdefinitions/read | Obtiene información sobre una definición de un conjunto de directivas. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.PolicyInsights](resource-provider-operations.md#microsoftpolicyinsights)/checkDataPolicyCompliance/action | Compruebe el estado de cumplimiento de un componente determinado en relación con las directivas de datos. |
> | [Microsoft.PolicyInsights](resource-provider-operations.md#microsoftpolicyinsights)/policyEvents/logDataEvents/action | Registra los eventos de directivas de componentes del recurso. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to resource policies and write access to resource component policy events.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "name": "66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/policyassignments/read",
        "Microsoft.Authorization/policydefinitions/read",
        "Microsoft.Authorization/policyexemptions/read",
        "Microsoft.Authorization/policysetdefinitions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.PolicyInsights/checkDataPolicyCompliance/action",
        "Microsoft.PolicyInsights/policyEvents/logDataEvents/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Policy Insights Data Writer (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="quota-request-operator"></a>Operador de solicitud de cuota

Leer y crear solicitudes de cuota, obtener el estado de la solicitud de cuota y crear incidencias de soporte técnico. [Más información](/rest/api/reserved-vm-instances/quotaapi)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/resourceProviders/locations/serviceLimits/read | Obtiene el límite de servicio actual o de la cuota del recurso y la ubicación especificados. |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/resourceProviders/locations/serviceLimits/write | Crea el límite de servicio o de la cuota del recurso y la ubicación especificados. |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/resourceProviders/locations/serviceLimitsRequests/read | Crea la solicitud del límite del servicio para el recurso y la ubicación especificados. |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/register/action | Registra al proveedor de recursos de Capacity y habilita la creación de recursos de tipo Capacity. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read and create quota requests, get quota request status, and create support tickets.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0e5f05e5-9ab9-446b-b98d-1e2157c94125",
  "name": "0e5f05e5-9ab9-446b-b98d-1e2157c94125",
  "permissions": [
    {
      "actions": [
        "Microsoft.Capacity/resourceProviders/locations/serviceLimits/read",
        "Microsoft.Capacity/resourceProviders/locations/serviceLimits/write",
        "Microsoft.Capacity/resourceProviders/locations/serviceLimitsRequests/read",
        "Microsoft.Capacity/register/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Quota Request Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reservation-purchaser"></a>Comprador de reservas

Permite comprar reservas [Más información](../cost-management-billing/reservations/prepare-buy-reservation.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/read | Obtiene la lista de suscripciones. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/register/action | Registra al proveedor de recursos de Capacity y habilita la creación de recursos de tipo Capacity. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/register/action | Registra la suscripción con el proveedor de recursos de Microsoft.Compute |
> | [Microsoft.SQL](resource-provider-operations.md#microsoftsql)/register/action | Registra la suscripción del proveedor de recursos de Microsoft SQL Database y habilita la creación de bases de datos de Microsoft SQL Database. |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/register/action | Registro el RP de consumo. |
> | [Microsoft.Capacity](resource-provider-operations.md#microsoftcapacity)/catalogs/read | Lee el catálogo de reserva. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/roleAssignments/read | Obtiene información sobre una asignación de roles. |
> | [Microsoft.Consumption](resource-provider-operations.md#microsoftconsumption)/reservationRecommendations/read | Enumera recomendaciones individuales o compartidas de las instancias reservadas de una suscripción. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/supporttickets/write | Permite crear y actualizar una incidencia de soporte técnico. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you purchase reservations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f7b75c60-3036-4b75-91c3-6b41c27c1689",
  "name": "f7b75c60-3036-4b75-91c3-6b41c27c1689",
  "permissions": [
    {
      "actions": [
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Capacity/register/action",
        "Microsoft.Compute/register/action",
        "Microsoft.SQL/register/action",
        "Microsoft.Consumption/register/action",
        "Microsoft.Capacity/catalogs/read",
        "Microsoft.Authorization/roleAssignments/read",
        "Microsoft.Consumption/reservationRecommendations/read",
        "Microsoft.Support/supporttickets/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reservation Purchaser",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="resource-policy-contributor"></a>Colaborador de directivas de recursos

Los usuarios con derechos para crear o modificar la directiva de recursos pueden crear solicitudes de soporte técnico y leer los recursos o la jerarquía. [Más información](../governance/policy/overview.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | */read | Leer recursos de todos los tipos, excepto secretos. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyassignments/* | Crear y administrar asignaciones de directivas |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policydefinitions/* | Crear y administrar definiciones de directivas |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policyexemptions/* | Permite crear y administrar exenciones de directivas. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/policysetdefinitions/* | Crear y administrar conjuntos de directivas |
> | [Microsoft.PolicyInsights](resource-provider-operations.md#microsoftpolicyinsights)/* |  |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Users with rights to create/modify resource policy, create support ticket and read resources/hierarchy.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/36243c78-bf99-498c-9df9-86d9f8d28608",
  "name": "36243c78-bf99-498c-9df9-86d9f8d28608",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/policyassignments/*",
        "Microsoft.Authorization/policydefinitions/*",
        "Microsoft.Authorization/policyexemptions/*",
        "Microsoft.Authorization/policysetdefinitions/*",
        "Microsoft.PolicyInsights/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Resource Policy Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-contributor"></a>Colaborador de Site Recovery

Permite administrar el servicio Site Recovery, excepto la creación de almacenes y la asignación de roles. [Más información](../site-recovery/site-recovery-role-based-linked-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocatedStamp/read | GetAllocatedStamp es una operación interna que el servicio usa |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocateStamp/action | AllocateStamp es una operación interna que el servicio usa |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/certificates/write | La operación Actualizar certificado de recursos permite actualizar el certificado de credencial de recursos o almacenes. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/* | Crear y administrar información ampliada relacionada con el almacén |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/refreshContainers/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/* | Crear y administrar identidades registradas |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationAlertSettings/* | Crea o actualiza la configuración de las alertas de replicación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationEvents/read | Lee todos los evento |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/* | Crea y administra los tejidos de replicación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationJobs/* | Crea y administra los trabajos de replicación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationPolicies/* | Crea y administra las directivas de replicación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/* | Crea y administra planes de recuperación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationVaultSettings/* |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/storageConfig/* | Crea y administra la configuración de almacenamiento del almacén de Recovery Services |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/tokenInfo/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/vaultTokens/read | La operación Token de almacén se puede usar para obtener el token de almacén de las operaciones back-end a nivel de almacén. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/* | Lee las alertas del almacén de Recovery Services |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationOperationStatus/read | Lee los estados de operación de replicación del almacén. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Site Recovery service except vault creation and role assignment",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "name": "6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/*",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/*",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/*",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/*",
        "Microsoft.RecoveryServices/vaults/replicationVaultSettings/*",
        "Microsoft.RecoveryServices/Vaults/storageConfig/*",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/vaults/replicationOperationStatus/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-operator"></a>Operador de Site Recovery

Permite realizar una conmutación por error o una conmutación por recuperación, pero no otras operaciones de administración de Site Recovery. [Más información](../site-recovery/site-recovery-role-based-linked-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Network](resource-provider-operations.md#microsoftnetwork)/virtualNetworks/read | Obtiene la definición de red virtual |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocatedStamp/read | GetAllocatedStamp es una operación interna que el servicio usa |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocateStamp/action | AllocateStamp es una operación interna que el servicio usa |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/read | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/refreshContainers/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/operationResults/read | La operación Obtener resultados de la operación se puede usar para obtener el estado y el resultado de la operación enviada de forma asincrónica |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/read | La operación Obtener contenedores se puede usar para obtener los contenedores registrados para un recurso. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationAlertSettings/read | Lee todas las configuraciones de alerta |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationEvents/read | Lee todos los evento |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/checkConsistency/action | Comprueba la coherencia del tejido |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/read | Lee todas las fábricas |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/reassociateGateway/action | Vuelve a asociar la puerta de enlace |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/renewcertificate/action | Renueva un certificado para Fabric. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationNetworks/read | Lee todas las redes |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Lee todas las asignaciones de redes |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/read | Lee todos los contenedores de protección |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Lee todos los elementos que se pueden proteger |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Aplica un punto de recuperación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Confirma la conmutación por error |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Conmutación por error planeada |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Lee todos los elementos protegidos |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lee los puntos de recuperación de todas las replicaciones |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Repara una replicación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Vuelva a proteger el elemento protegido |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Cambia los contenedores de protección |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Test Failover |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Prueba la limpieza de la conmutación por error |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Conmutación por error |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Actualiza Mobility Service |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Lee todas las asignaciones de los contenedores de protección |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Lee todos los proveedores de Recovery Services |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Actualiza el proveedor |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationStorageClassifications/read | Lee todas las clasificaciones de almacenamiento |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Lee todas las asignaciones de clasificaciones de almacenamiento |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationvCenters/read | Lee todos los vCenters. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationJobs/* | Crea y administra los trabajos de replicación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationPolicies/read | Lee todas las directivas |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/failoverCommit/action | Plan de recuperación de confirmación de la conmutación por error |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/plannedFailover/action | Plan de recuperación de conmutación por error planeado |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/read | Lee todos los planes de recuperación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/reProtect/action | Vuelve a proteger el plan de recuperación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/testFailover/action | Prueba el plan de recuperación de conmutación por error |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Prueba el plan de recuperación de limpieza de la conmutación por error |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/unplannedFailover/action | Plan de recuperación de conmutación por error |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationVaultSettings/read | Lee cualquiera.  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/* | Lee las alertas del almacén de Recovery Services |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/storageConfig/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/tokenInfo/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/vaultTokens/read | La operación Token de almacén se puede usar para obtener el token de almacén de las operaciones back-end a nivel de almacén. |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you failover and failback but not perform other Site Recovery management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/494ae006-db33-4328-bf46-533a6560a3ca",
  "name": "494ae006-db33-4328-bf46-533a6560a3ca",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationVaultSettings/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-reader"></a>Lector de Site Recovery

Permite visualizar el estado de Site Recovery, pero no realizar otras operaciones de administración. [Más información](../site-recovery/site-recovery-role-based-linked-access-control.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/locations/allocatedStamp/read | GetAllocatedStamp es una operación interna que el servicio usa |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/extendedInformation/read | La operación Obtener información adicional obtiene la información adicional de un objeto que representa el recurso de Azure de tipo ?almacén? |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringAlerts/read | Obtiene las alertas del almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/read | La operación Get Vault obtiene un objeto que representa el recurso de Azure del tipo "almacén" |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/refreshContainers/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/operationResults/read | La operación Obtener resultados de la operación se puede usar para obtener el estado y el resultado de la operación enviada de forma asincrónica |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/registeredIdentities/read | La operación Obtener contenedores se puede usar para obtener los contenedores registrados para un recurso. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationAlertSettings/read | Lee todas las configuraciones de alerta |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationEvents/read | Lee todos los evento |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/read | Lee todas las fábricas |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationNetworks/read | Lee todas las redes |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Lee todas las asignaciones de redes |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/read | Lee todos los contenedores de protección |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Lee todos los elementos que se pueden proteger |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Lee todos los elementos protegidos |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lee los puntos de recuperación de todas las replicaciones |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Lee todas las asignaciones de los contenedores de protección |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Lee todos los proveedores de Recovery Services |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationStorageClassifications/read | Lee todas las clasificaciones de almacenamiento |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Lee todas las asignaciones de clasificaciones de almacenamiento |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationFabrics/replicationvCenters/read | Lee todos los vCenters. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationJobs/read | Lee todos los trabajos |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationPolicies/read | Lee todas las directivas |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationRecoveryPlans/read | Lee todos los planes de recuperación |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/vaults/replicationVaultSettings/read | Lee cualquiera.  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/storageConfig/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/tokenInfo/read |  |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/usages/read | Devuelve los detalles de uso de un almacén de Recovery Services. |
> | [Microsoft.RecoveryServices](resource-provider-operations.md#microsoftrecoveryservices)/Vaults/vaultTokens/read | La operación Token de almacén se puede usar para obtener el token de almacén de las operaciones back-end a nivel de almacén. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view Site Recovery status but not perform other management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "name": "dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/read",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/vaults/replicationVaultSettings/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="support-request-contributor"></a>Colaborador de la solicitud de soporte técnico

Permite crear y administrar solicitudes de soporte técnico. [Más información](../azure-portal/supportability/how-to-create-azure-support-request.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create and manage Support requests",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "name": "cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Support Request Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="tag-contributor"></a>Colaborador de etiquetas

Permite administrar etiquetas en las entidades sin proporcionar acceso a las entidades mismas. [Más información](../azure-resource-manager/management/tag-resources.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/resources/read | Obtiene los recursos del grupo de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resources/read | Obtiene recursos de una suscripción. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/tags/* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage tags on entities, without providing access to the entities themselves.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "name": "4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
        "Microsoft.Resources/subscriptions/resources/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/tags/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Tag Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="other"></a>Otros


### <a name="azure-digital-twins-data-owner"></a>Propietario de datos de Azure Digital Twins

Rol de acceso completo para plano de datos de Digital Twins [Más información](../digital-twins/concepts-security.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/eventroutes/* | Lee, elimina, crea o actualiza cualquier ruta de eventos. |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/* | Lee, crea, actualiza o elimina cualquier gemelo digital. |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/commands/* | Invoca cualquier comando en un gemelo digital. |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/relationships/* | Lee, crea, actualiza o elimina cualquier relación de gemelo digital. |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/models/* | Lee, crea, actualiza o elimina cualquier modelo. |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/query/* | Consulta cualquier grafo de Digital Twins. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Full access role for Digital Twins data-plane",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/bcd981a7-7f74-457b-83e1-cceb9e632ffe",
  "name": "bcd981a7-7f74-457b-83e1-cceb9e632ffe",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.DigitalTwins/eventroutes/*",
        "Microsoft.DigitalTwins/digitaltwins/*",
        "Microsoft.DigitalTwins/digitaltwins/commands/*",
        "Microsoft.DigitalTwins/digitaltwins/relationships/*",
        "Microsoft.DigitalTwins/models/*",
        "Microsoft.DigitalTwins/query/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Digital Twins Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-digital-twins-data-reader"></a>Lector de datos de Azure Digital Twins

Rol de solo lectura para las propiedades del plano de datos de Digital Twins [Más información](../digital-twins/concepts-security.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/read | Lee cualquier gemelo digital. |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/digitaltwins/relationships/read | Lee cualquier relación de gemelo digital. |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/eventroutes/read | Lee cualquier ruta de eventos. |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/models/read | Lee cualquier modelo. |
> | [Microsoft.DigitalTwins](resource-provider-operations.md#microsoftdigitaltwins)/query/action | Consulta cualquier grafo de Digital Twins. |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only role for Digital Twins data-plane properties",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d57506d4-4c8d-48b1-8587-93c323f6a5a3",
  "name": "d57506d4-4c8d-48b1-8587-93c323f6a5a3",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.DigitalTwins/digitaltwins/read",
        "Microsoft.DigitalTwins/digitaltwins/relationships/read",
        "Microsoft.DigitalTwins/eventroutes/read",
        "Microsoft.DigitalTwins/models/read",
        "Microsoft.DigitalTwins/query/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Digital Twins Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="biztalk-contributor"></a>Colaborador de BizTalk

Permite administrar los servicios de BizTalk, pero no acceder a ellos.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | Microsoft.BizTalkServices/BizTalk/* | Crear y administrar los servicios de BizTalk |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage BizTalk services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "name": "5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BizTalkServices/BizTalk/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "BizTalk Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-application-group-contributor"></a>Colaborador del grupo de aplicaciones de Desktop Virtualization

Colaborador del grupo de aplicaciones de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/* |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Leer grupos de hosts |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/read | Leer grupos de hosts/hosts de sesión |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Contributor of the Desktop Virtualization Application Group.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/86240b0e-9422-4c43-887b-b61143f32ba8",
  "name": "86240b0e-9422-4c43-887b-b61143f32ba8",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/applicationgroups/*",
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Application Group Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-application-group-reader"></a>Lector del grupo de aplicaciones de Desktop Virtualization

Lector del grupo de aplicaciones de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/*/read |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/read | Leer grupos de aplicaciones |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Leer grupos de hosts |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/read | Leer grupos de hosts/hosts de sesión |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Obtiene o enumera implementaciones. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Lee una alerta de métrica clásica. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Reader of the Desktop Virtualization Application Group.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/aebf23d0-b568-4e86-b8f9-fe83a2c6ab55",
  "name": "aebf23d0-b568-4e86-b8f9-fe83a2c6ab55",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/applicationgroups/*/read",
        "Microsoft.DesktopVirtualization/applicationgroups/read",
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Application Group Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-contributor"></a>Colaborador de virtualización del escritorio

Colaborador de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Contributor of Desktop Virtualization.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/082f0a83-3be5-4ba1-904c-961cca79b387",
  "name": "082f0a83-3be5-4ba1-904c-961cca79b387",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-host-pool-contributor"></a>Colaborador del grupo de hosts de Desktop Virtualization

Colaborador del grupo de hosts de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Contributor of the Desktop Virtualization Host Pool.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e307426c-f9b6-4e81-87de-d99efb3c32bc",
  "name": "e307426c-f9b6-4e81-87de-d99efb3c32bc",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/hostpools/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Host Pool Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-host-pool-reader"></a>Lector del grupo de hosts de Desktop Virtualization

Lector del grupo de hosts de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/*/read |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Leer grupos de hosts |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Obtiene o enumera implementaciones. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Lee una alerta de métrica clásica. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Reader of the Desktop Virtualization Host Pool.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ceadfde2-b300-400a-ab7b-6143895aa822",
  "name": "ceadfde2-b300-400a-ab7b-6143895aa822",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/hostpools/*/read",
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Host Pool Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-reader"></a>Lector de virtualización del escritorio

Lector de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/*/read |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Obtiene o enumera implementaciones. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Lee una alerta de métrica clásica. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Reader of Desktop Virtualization.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/49a72310-ab8d-41df-bbb0-79b649203868",
  "name": "49a72310-ab8d-41df-bbb0-79b649203868",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-session-host-operator"></a>Operador de host de sesión de Desktop Virtualization

Operador del host de sesión de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Leer grupos de hosts |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Operator of the Desktop Virtualization Session Host.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2ad6aaab-ead9-4eaa-8ac5-da422f562408",
  "name": "2ad6aaab-ead9-4eaa-8ac5-da422f562408",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Session Host Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-user"></a>Usuario de Desktop Virtualization

Permite al usuario emplear las aplicaciones de un grupo de aplicaciones. [Más información](../virtual-desktop/delegated-access-virtual-desktop.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | *Ninguna* |  |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationGroups/useApplications/action | Usar ApplicationGroup |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows user to use the applications in an application group.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1d18fff3-a72a-46b5-b4a9-0b38a3cd7e63",
  "name": "1d18fff3-a72a-46b5-b4a9-0b38a3cd7e63",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.DesktopVirtualization/applicationGroups/useApplications/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-user-session-operator"></a>Operador de sesión de usuario de Desktop Virtualization

Operador de sesión de usuario de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/read | Leer grupos de hosts |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/read | Leer grupos de hosts/hosts de sesión |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/hostpools/sessionhosts/usersessions/* |  |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Operator of the Desktop Virtualization Uesr Session.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ea4bfff8-7fb4-485a-aadd-d4129a0ffaa6",
  "name": "ea4bfff8-7fb4-485a-aadd-d4129a0ffaa6",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/hostpools/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/read",
        "Microsoft.DesktopVirtualization/hostpools/sessionhosts/usersessions/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization User Session Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-workspace-contributor"></a>Colaborador del área de trabajo de Desktop Virtualization

Colaborador del área de trabajo de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/workspaces/* |  |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/read | Leer grupos de aplicaciones |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Contributor of the Desktop Virtualization Workspace.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/21efdde3-836f-432b-bf3d-3e8e734d4b2b",
  "name": "21efdde3-836f-432b-bf3d-3e8e734d4b2b",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/workspaces/*",
        "Microsoft.DesktopVirtualization/applicationgroups/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Workspace Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="desktop-virtualization-workspace-reader"></a>Lector del área de trabajo de Desktop Virtualization

Lector del área de trabajo de Desktop Virtualization. [Más información](../virtual-desktop/rbac.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/workspaces/read | Leer áreas de trabajo |
> | [Microsoft.DesktopVirtualization](resource-provider-operations.md#microsoftdesktopvirtualization)/applicationgroups/read | Leer grupos de aplicaciones |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/read | Obtiene o enumera implementaciones. |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/read | Lee una alerta de métrica clásica. |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Reader of the Desktop Virtualization Workspace.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0fa44ee9-7a7d-466b-9bb2-2bf446b1204d",
  "name": "0fa44ee9-7a7d-466b-9bb2-2bf446b1204d",
  "permissions": [
    {
      "actions": [
        "Microsoft.DesktopVirtualization/workspaces/read",
        "Microsoft.DesktopVirtualization/applicationgroups/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Desktop Virtualization Workspace Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="disk-backup-reader"></a>Lector de copias de seguridad de disco

Proporciona permiso para realizar copias de seguridad del almacén para realizar copias de seguridad de disco. [Más información](../backup/disk-backup-faq.yml)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/read | Obtiene las propiedades de un disco |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/beginGetAccess/action | Obtiene el URI de SAS del disco de acceso de blob |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides permission to backup vault to perform disk backup.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3e5e47e6-65f7-47ef-90b5-e5dd4d455f24",
  "name": "3e5e47e6-65f7-47ef-90b5-e5dd4d455f24",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/disks/read",
        "Microsoft.Compute/disks/beginGetAccess/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Disk Backup Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="disk-pool-operator"></a>Operador de grupo de discos

Proporcione permiso al proveedor de recursos StoragePool para administrar los discos agregados a un grupo de discos.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/write | Crea un nuevo disco o actualiza uno ya existente |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/read | Obtiene las propiedades de un disco |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Used by the StoragePool Resource Provider to manage Disks added to a Disk Pool.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/60fc6e62-5479-42d4-8bf4-67625fcc2840",
  "name": "60fc6e62-5479-42d4-8bf4-67625fcc2840",
  "permissions": [
    {
      "actions": [
        "Microsoft.Compute/disks/write",
        "Microsoft.Compute/disks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Disk Pool Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="disk-restore-operator"></a>Operador de restauración de disco

Proporciona permiso para realizar copias de seguridad del almacén para realizar restauraciones de disco. [Más información](../backup/restore-managed-disks.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/write | Crea un nuevo disco o actualiza uno ya existente |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/read | Obtiene las propiedades de un disco |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides permission to backup vault to perform disk restore.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b50d9833-a0cb-478e-945f-707fcc997c13",
  "name": "b50d9833-a0cb-478e-945f-707fcc997c13",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Compute/disks/write",
        "Microsoft.Compute/disks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Disk Restore Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="disk-snapshot-contributor"></a>Colaborador de instantáneas de discos

Proporciona permiso para realizar copias de seguridad del almacén para administrar instantáneas de disco. [Más información](../backup/backup-managed-disks.md)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/delete | Elimina una instantánea |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/write | Crea una nueva instantánea o actualiza una ya existente |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/read | Obtiene las propiedades de una instantánea |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/beginGetAccess/action | Obtiene el URI de SAS de la instantánea para el acceso de blob. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/snapshots/endGetAccess/action | Revoca el URI de SAS de la instantánea. |
> | [Microsoft.Compute](resource-provider-operations.md#microsoftcompute)/disks/beginGetAccess/action | Obtiene el URI de SAS del disco de acceso de blob |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/listkeys/action | Devuelve las claves de acceso de la cuenta de almacenamiento especificada. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/write | Crea una cuenta de almacenamiento con los parámetros especificados o actualiza las propiedades o etiquetas, o agrega un dominio personalizado para la cuenta de almacenamiento especificada. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/read | Devuelve la lista de cuentas de almacenamiento u obtiene las propiedades de la cuenta de almacenamiento especificada. |
> | [Microsoft.Storage](resource-provider-operations.md#microsoftstorage)/storageAccounts/delete | Agrega una cuenta de almacenamiento existente. |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Provides permission to backup vault to manage disk snapshots.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7efff54f-a5b4-42b5-a1c5-5411624893ce",
  "name": "7efff54f-a5b4-42b5-a1c5-5411624893ce",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Compute/snapshots/delete",
        "Microsoft.Compute/snapshots/write",
        "Microsoft.Compute/snapshots/read",
        "Microsoft.Compute/snapshots/beginGetAccess/action",
        "Microsoft.Compute/snapshots/endGetAccess/action",
        "Microsoft.Compute/disks/beginGetAccess/action",
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/write",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Storage/storageAccounts/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Disk Snapshot Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="scheduler-job-collections-contributor"></a>Colaborador de colecciones de trabajos de Scheduler

Permite administrar colecciones de trabajos de Scheduler, pero no acceder a ellas.

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Insights](resource-provider-operations.md#microsoftinsights)/alertRules/* | Creación y administración de una alerta de métricas clásica |
> | [Microsoft.ResourceHealth](resource-provider-operations.md#microsoftresourcehealth)/availabilityStatuses/read | Obtiene los estados de disponibilidad de todos los recursos en el ámbito especificado |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Scheduler](resource-provider-operations.md#microsoftscheduler)/jobcollections/* | Crear y administrar colecciones de trabajos |
> | [Microsoft.Support](resource-provider-operations.md#microsoftsupport)/* | Creación y actualización de una incidencia de soporte técnico |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Scheduler job collections, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "name": "188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Scheduler/jobcollections/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Scheduler Job Collections Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="services-hub-operator"></a>Services Hub Operator

Services Hub Operator permite realizar todas las operaciones de lectura, escritura y eliminación relacionadas con los conectores de Services Hub. [Más información](/services-hub/health/sh-connector-roles)

> [!div class="mx-tableFixed"]
> | Acciones | Descripción |
> | --- | --- |
> | [Microsoft.Authorization](resource-provider-operations.md#microsoftauthorization)/*/read | Leer roles y asignaciones de roles |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/subscriptions/resourceGroups/read | Obtiene o enumera los grupos de recursos. |
> | [Microsoft.Resources](resource-provider-operations.md#microsoftresources)/deployments/* | Creación y administración de una implementación |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/connectors/write | Crear o actualizar un conector de Services Hub |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/connectors/read | Ver o enumerar conectores de Services Hub |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/connectors/delete | Eliminar conectores de Services Hub |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/connectors/checkAssessmentEntitlement/action | Enumera los derechos de evaluación de un área de trabajo determinada de Services Hub |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/supportOfferingEntitlement/read | Ver los derechos de oferta de soporte técnico de un área de trabajo determinada de Services Hub |
> | [Microsoft.ServicesHub](resource-provider-operations.md#microsoftserviceshub)/workspaces/read | Enumerar las áreas de trabajo de Services Hub de un usuario dado |
> | **NotActions** |  |
> | *Ninguna* |  |
> | **DataActions** |  |
> | *Ninguna* |  |
> | **NotDataActions** |  |
> | *Ninguna* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Services Hub Operator allows you to perform all read, write, and deletion operations related to Services Hub Connectors.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/82200a5b-e217-47a5-b665-6d8765ee745b",
  "name": "82200a5b-e217-47a5-b665-6d8765ee745b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.ServicesHub/connectors/write",
        "Microsoft.ServicesHub/connectors/read",
        "Microsoft.ServicesHub/connectors/delete",
        "Microsoft.ServicesHub/connectors/checkAssessmentEntitlement/action",
        "Microsoft.ServicesHub/supportOfferingEntitlement/read",
        "Microsoft.ServicesHub/workspaces/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Services Hub Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de Azure mediante Azure Portal](role-assignments-portal.md)
- [Roles personalizados de Azure](custom-roles.md)
- [Permisos en Azure Security Center](../security-center/security-center-permissions.md)
