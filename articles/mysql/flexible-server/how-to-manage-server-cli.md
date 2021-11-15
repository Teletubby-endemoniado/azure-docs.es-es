---
title: Administración de un servidor flexible de Azure Database for MySQL mediante la CLI de Azure
description: Aprenda a administrar un servidor flexible de Azure Database for MySQL con la CLI de Azure.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: a2836e2c319810b48a20742cebaa816b6c20ffe1
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131438082"
---
# <a name="manage-an-azure-database-for-mysql---flexible-server-using-the-azure-cli"></a>Administración de un servidor flexible de Azure Database for MySQL mediante la CLI de Azure
[!INCLUDE[applies-to-mysql-flexible-server](../includes/applies-to-mysql-flexible-server.md)]

En este artículo se muestra cómo administrar el Servidor flexible implementado en Azure. Entre las tareas de administración se incluyen el escalado de proceso y almacenamiento, el restablecimiento de contraseñas de administración y la visualización de detalles del servidor.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [flexible-server-free-trial-note](../includes/flexible-server-free-trial-note.md)]

En este artículo es necesario que ejecute la versión 2.0 de la CLI de Azure, o cualquier versión posterior, de forma local. Para ver la versión instalada, ejecute el comando `az --version`. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

Será preciso que inicie sesión en su cuenta mediante el comando [az login](/cli/azure/reference-index#az_login). Tenga en cuenta la propiedad **id**, que hace referencia al **identificador de suscripción** para su cuenta de Azure.

```azurecli-interactive
az login
```

Seleccione la suscripción específica en su cuenta mediante el comando [az account set](/cli/azure/account). Anote el valor de **id** de la salida de **az login** para usarlo como valor del argumento **subscription** del comando. Si tiene varias suscripciones, elija la suscripción adecuada en la que se debe facturar el recurso. Para obtener todas las suscripciones, use [az account list](/cli/azure/account#az_account_list).

```azurecli
az account set --subscription <subscription id>
```

> [!IMPORTANT]
>Si aún no ha creado un servidor flexible, créelo para empezar a trabajar con esta guía de procedimientos.

## <a name="scale-compute-and-storage"></a>Escalado de proceso y almacenamiento

El nivel de proceso, los núcleos virtuales y el almacenamiento se pueden escalar verticalmente de forma sencilla con el siguiente comando. Para ver todas las operaciones de servidor, ejecute [az mysql flexible-server update](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_update).

```azurecli-interactive
az mysql flexible-server update --resource-group myresourcegroup --name mydemoserver --sku-name Standard_D4ds_v4 --storage-size 6144
```

Estos son los detalles de los argumentos anteriores:

**Configuración** | **Valor de ejemplo** | **Descripción**
---|---|---
name | mydemoserver | Escriba un nombre único para el servidor de Azure Database for MySQL. El nombre del servidor solo puede contener letras minúsculas, números y el carácter de guion (-). Debe contener entre 3 y 63 caracteres.
resource-group | myresourcegroup | Especifique el nombre del grupo de recursos de Azure.
sku-name|Standard_D4ds_v4|Escriba el nombre del nivel de proceso y el tamaño. Sigue la convención Standard_ {tamaño de máquina virtual} en abreviatura. Para más información, consulte los [planes de tarifa](../concepts-pricing-tiers.md).
storage-size | 6144 | La capacidad de almacenamiento del servidor (la unidad es megabytes). El mínimo es 5120 y aumenta en incrementos de 1024.

> [!IMPORTANT]
>- El almacenamiento se puede escalar verticalmente (pero no reducir)


## <a name="manage-mysql-databases-on-a-server"></a>Administre bases de datos de MySQL en un servidor.
Puede usar cualquiera de estos comandos para crear, eliminar, enumerar y ver las propiedades de una base de datos en el servidor.

| Cmdlet | Uso| Descripción |
| --- | ---| --- |
|[az mysql flexible-server db create](/cli/azure/mysql/flexible-server/db#az_mysql_flexible_server_db_create)|```az mysql flexible-server db create -g myresourcegroup -s mydemoserver -n mydatabasename``` |Crea una base de datos.|
|[az mysql flexible-server db delete](/cli/azure/mysql/flexible-server/db#az_mysql_flexible_server_db_delete)|```az mysql flexible-server db delete -g myresourcegroup -s mydemoserver -n mydatabasename```|Elimina la base de datos del servidor. Este comando no elimina el servidor. |
|[az mysql flexible-server db list](/cli/azure/mysql/flexible-server/db#az_mysql_flexible_server_db_list)|```az mysql flexible-server db list -g myresourcegroup -s mydemoserver```|Enumera todas las bases de datos del servidor.|
|[az mysql flexible-server db show](/cli/azure/mysql/flexible-server/db#az_mysql_flexible_server_db_show)|```az mysql flexible-server db show -g myresourcegroup -s mydemoserver -n mydatabasename```|Muestra más detalles de la base de datos.|

## <a name="update-admin-password"></a>Actualización de la contraseña del administrador
La contraseña del rol Administrador se puede cambiar con este comando.
```azurecli-interactive
az mysql flexible-server update --resource-group myresourcegroup --name mydemoserver --admin-password <new-password>
```

> [!IMPORTANT]
> Asegúrese de que tiene su longitud oscila entre 8 y 128 caracteres.
> Debe contener caracteres de tres de las categorías siguientes: Letras del alfabeto inglés mayúsculas y minúsculas, números y caracteres no alfanuméricos.

## <a name="delete-a-server"></a>Eliminación de un servidor
Si solo quiere eliminar el servidor flexible de MySQL, puede ejecutar el comando [az mysql flexible-server server delete](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_delete).

```azurecli-interactive
az mysql flexible-server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Pasos siguientes
- [Aprenda a iniciar o detener un servidor](how-to-stop-start-server-portal.md)
- [Aprenda a administrar una red virtual](how-to-manage-virtual-network-cli.md)
- [Solución de problemas de conexión a Azure Database for PostgreSQL- Hiperescala (Citus)](how-to-troubleshoot-common-connection-issues.md)
- [Creación y administración de un firewall](how-to-manage-firewall-cli.md)