---
title: Administración de la alta disponibilidad con redundancia de zona en un servidor flexible de Azure Database for MySQL mediante la CLI de Azure
description: En este artículo se describe cómo configurar la alta disponibilidad con redundancia de zona en un servidor flexible de Azure Database for MySQL con la CLI de Azure.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 04/1/2021
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: b6a430c70d59ff980063139e71daf76d1ede220a
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128610149"
---
# <a name="manage-zone-redundant-high-availability-in-azure-database-for-mysql-flexible-server-with-azure-cli"></a>Administración de la alta disponibilidad con redundancia de zona en un servidor flexible de Azure Database for MySQL con la CLI de Azure

[!INCLUDE[applies-to-mysql-flexible-server](../includes/applies-to-mysql-flexible-server.md)]

> [!NOTE]
> La opción de implementación Servidor flexible de Azure Database for MySQL está en versión preliminar pública.

En este artículo se describe cómo habilitar o deshabilitar la configuración de la alta disponibilidad con redundancia de zona en el momento de la creación del servidor en el servidor flexible. También puede deshabilitar la alta disponibilidad con redundancia de zona después de la creación del servidor. No se admite la habilitación de la alta disponibilidad con redundancia de zona después de la creación del servidor.

La característica de alta disponibilidad proporciona una réplica principal y una réplica en espera separadas físicamente en distintas zonas. Para más información, consulte la [documentación sobre los conceptos de alta disponibilidad](./concepts/../concepts-high-availability.md). La habilitación o deshabilitación de la alta disponibilidad no cambia las demás opciones, como la configuración de red virtual, la configuración del firewall y la retención de copias de seguridad. La deshabilitación de la alta disponibilidad no afecta a la conectividad ni a las operaciones de las aplicaciones.

> [!IMPORTANT]
> La alta disponibilidad con redundancia de zona está disponible en un conjunto limitado de regiones. Consulte [aquí](./overview.md#azure-regions) las regiones admitidas. 

## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. 

    [!INCLUDE [flexible-server-free-trial-note](../includes/flexible-server-free-trial-note.md)]
- Instale la CLI de Azure más reciente o actualice la que ya tiene a la versión más reciente. Consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).
- Inicie sesión en la cuenta de Azure mediante el comando [az login](/cli/azure/reference-index#az_login). Tenga en cuenta la propiedad **id**, que hace referencia al **identificador de suscripción** para su cuenta de Azure.

    ```azurecli-interactive
    az login
    ````

- Si tiene varias suscripciones, elija la más adecuada en la que quiera crear el servidor mediante el comando ```az account set```.

    ```azurecli
    az account set --subscription <subscription id>
    ```

## <a name="enable-high-availability-during-server-creation"></a>Habilitación de la alta disponibilidad durante la creación del servidor

Solo puede crear un servidor mediante planes de tarifa de uso general u optimizados para memoria con alta disponibilidad. Puede habilitar la alta disponibilidad con redundancia de zona para un servidor solo en el momento de la creación.

**Uso:**

   ```azurecli
    az mysql flexible-server create [--high-availability {Disabled, SameZone, ZoneRedundant}]
                                    [--sku-name]
                                    [--tier]
                                    [--resource-group]
                                    [--location]
                                    [--name]
   ```

**Ejemplo**:

   ```azurecli
    az mysql flexible-server create --name myservername --sku-name Standard_D2ds_v4 --tier Genaralpurpose --resource-group myresourcegroup --high-availability ZoneRedundant --location eastus
   ```

## <a name="disable-high-availability"></a>Deshabilitación de la alta disponibilidad

Puede deshabilitar la alta disponibilidad mediante el comando [az mysql flexible-server update](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_update). Tenga en cuenta que la deshabilitación de la alta disponibilidad solo se admite si el servidor se creó con alta disponibilidad. 

```azurecli
az mysql flexible-server update [--high-availability {Disabled, SameZone, ZoneRedundant}]
                                [--resource-group]
                                [--name]
```
>[!Note]
>Si desea pasar de ZoneRedundant a SameZone, primero tendría que deshabilitar la alta disponibilidad y, a continuación, habilitar la misma zona.

**Ejemplo**:

   ```azurecli
    az mysql flexible-server update --resource-group myresourcegroup --name myservername --high-availability Disabled
   ```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [continuidad empresarial](./concepts-business-continuity.md)
- Obtenga información sobre la [alta disponibilidad con redundancia de zona](./concepts-high-availability.md)