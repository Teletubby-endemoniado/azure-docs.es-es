---
title: 'Administración de reglas de firewall de Azure Database for PostgreSQL: servidor flexible mediante la CLI de Azure'
description: 'Creación y administración de reglas de firewall de Azure Database for PostgreSQL: servidor flexible mediante la línea de comandos de la CLI de Azure.'
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.devlang: azurecli
ms.topic: how-to
ms.date: 09/22/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: e571d29dc9bade1069f6c67257bf04552ff894ef
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130241617"
---
# <a name="create-and-manage-azure-database-for-postgresql---flexible-server-firewall-rules-using-the-azure-cli"></a>Creación y administración de reglas de firewall de Azure Database for PostgreSQL: servidor flexible mediante la CLI de Azure

> [!IMPORTANT]
> Azure Database for PostgreSQL: servidor flexible en versión preliminar

Azure Database for PostgreSQL: Servidor flexible admite dos tipos de métodos de conectividad de red mutuamente excluyentes para conectarse al servidor flexible. Las dos opciones son las siguientes:

* Acceso público (direcciones IP permitidas)
* Acceso privado (integración con red virtual)

En este artículo, nos centraremos en la creación de un servidor PostgreSQL con **Acceso público (direcciones IP permitidas)** mediante la CLI de Azure y se proporcionará información general sobre los comandos de la CLI de Azure que puede usar para crear, actualizar, eliminar, enumerar y mostrar reglas de firewall después de la creación de un servidor. Con *Acceso público (direcciones IP permitidas)* , las conexiones con el servidor PostgreSQL están restringidas únicamente a las direcciones IP permitidas. Las direcciones IP del cliente se deben permitir en reglas de firewall. Para obtener más información al respecto, vea [Acceso público (direcciones IP permitidas)](./concepts-networking.md#public-access-allowed-ip-addresses). Las reglas de firewall se pueden definir en el momento de crear el servidor (recomendado), pero también se pueden agregar después.

## <a name="launch-azure-cloud-shell"></a>Inicio de Azure Cloud Shell

[Azure Cloud Shell](../../cloud-shell/overview.md) es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta.

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. También puede abrir Cloud Shell en una pestaña independiente acudiendo a [https://shell.azure.com/bash](https://shell.azure.com/bash). Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, después, seleccione **Entrar** para ejecutarlos.

Si prefiere instalar y usar la CLI de forma local, en este inicio rápido se requiere la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Requisitos previos

Deberá iniciar sesión en la cuenta con el comando [az login](/cli/azure/reference-index#az_login). Tenga en cuenta la propiedad **id**, que hace referencia al **id. de suscripción** de su cuenta de Azure.

```azurecli-interactive
az login
```

Seleccione la suscripción específica en su cuenta mediante el comando [az account set](/cli/azure/account#az_account_set). Anote el valor de **id** de la salida de **az login** para usarlo como valor del argumento **subscription** del comando. Si tiene varias suscripciones, elija la suscripción adecuada en la que se debe facturar el recurso. Para obtener todas las suscripciones, use [az account list](/cli/azure/account#az_account_list).

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-firewall-rule-during-flexible-server-create-using-azure-cli"></a>Creación de una regla de firewall durante la creación de un servidor flexible mediante la CLI de Azure

Puede usar el comando `az postgres flexible-server --public access` para crear el servidor flexible con la opción *Acceso público (direcciones IP permitidas)* y configurar las reglas de firewall durante la creación del mismo. Puede usar el modificador **--public-access** para especificar las direcciones IP permitidas que podrán conectarse al servidor. Puede especificar una sola dirección IP o el intervalo de direcciones IP que se van a incluir en la lista de direcciones IP permitidas. El intervalo de direcciones IP debe estar separado por guiones y no contener espacios. Existen varias opciones para crear un servidor flexible mediante la CLI, como se muestra en el ejemplo siguiente.

Consulte la documentación de referencia de la CLI de Azure <!--FIXME --> para obtener la lista completa de parámetros configurables de la CLI. Por ejemplo, en los siguientes comandos puede especificar opcionalmente el grupo de recursos.

- Cree un servidor flexible con acceso público y agregue la dirección IP del cliente para tener acceso al servidor.
    ```azurecli-interactive
    az postgres flexible-server create --public-access <my_client_ip>
    ```
- Cree un servidor flexible con acceso público y agregue el intervalo de direcciones IP para tener acceso al servidor.

    ```azurecli-interactive
    az postgres flexible-server create --public-access <start_ip_address-end_ip_address>
    ```
- Cree un servidor flexible con acceso público y permita que las aplicaciones de las direcciones IP de Azure se conecten a él.
    ```azurecli-interactive
    az postgres flexible-server create --public-access 0.0.0.0
    ```
    > [!IMPORTANT]
    > Esta opción configura el firewall para permitir el acceso público desde los servicios y recursos de Azure a este servidor, incluidas las conexiones desde las suscripciones de otros clientes. Al seleccionar esta opción, asegúrese de que los permisos de usuario y el inicio de sesión limiten el acceso solamente a los usuarios autorizados.
    >
- - Creación de un servidor flexible con acceso público y permiso para todas las direcciones IP
    ```azurecli-interactive
    az postgres flexible-server create --public-access all
    ```
    >[!Note]
    > El comando anterior creará una regla de firewall cuya dirección IP inicial es 0.0.0.0 y la dirección IP final 255.255.255.255, y no se bloqueará ninguna dirección IP. Cualquier host de Internet puede acceder a este servidor. Se recomienda encarecidamente usar esta regla solo temporalmente y en servidores de prueba que no contengan información confidencial.
- Creación de un servidor flexible con acceso público y sin dirección IP
    ```azurecli-interactive
    az postgres flexible-server create --public-access none
    ```
    >[!Note]
    > no se recomienda crear un servidor sin ninguna regla de firewall. Si no agrega reglas de firewall, ningún cliente podrá conectarse al servidor.
## <a name="create-and-manage-firewall-rule-after-server-create"></a>Creación y administración de una regla de firewall después de crear el servidor
El comando **az postgres flexible-server firewall-rule** se utiliza desde la CLI de Azure para crear, eliminar, enumerar, mostrar y actualizar reglas de firewall.

Comandos:
- **create**: crea una regla de firewall de servidor flexible.
- **list**: enumera las reglas de firewall de servidor flexible.
- **update**: actualiza una regla de firewall de servidor flexible.
- **show**: muestra los detalles de una regla de firewall de servidor flexible.
- **delete**: elimina una regla de firewall de servidor flexible.

Consulte la documentación de referencia de la CLI de Azure <!--FIXME --> para obtener la lista completa de parámetros configurables de la CLI. Por ejemplo, en los siguientes comandos puede especificar opcionalmente el grupo de recursos.

### <a name="create-a-firewall-rule"></a>Creación de una regla de firewall
Use el comando `az postgres flexible-server firewall-rule create` para crear una regla de firewall en el servidor.
Para permitir el acceso a un intervalo de direcciones IP, especifique la dirección IP en forma de dirección IP inicial y dirección IP final, como en este ejemplo. Este comando también espera el nombre del grupo de recursos de Azure donde el servidor se encuentra como parámetro. 
```azurecli-interactive
az postgres flexible-server firewall-rule create --name mydemoserver --resource-group testGroup --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Para permitir el acceso a una única dirección IP, especifique una sola dirección IP, como en este ejemplo.
```azurecli-interactive
az postgres flexible-server firewall-rule create --name mydemoserver  --resource-group testGroup  --start-ip-address 1.1.1.1
```

Para permitir que las aplicaciones de las direcciones IP de Azure se conecten al servidor flexible, especifique la dirección IP 0.0.0.0 como IP inicial, como en este ejemplo.
```azurecli-interactive
az postgres flexible-server firewall-rule create --name mydemoserver --resource-group testGroup --start-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Esta opción configura el firewall para permitir el acceso público desde los servicios y recursos de Azure a este servidor, incluidas las conexiones desde las suscripciones de otros clientes. Al seleccionar esta opción, asegúrese de que los permisos de usuario y el inicio de sesión limiten el acceso solamente a los usuarios autorizados.
> 
Si se realiza correctamente, en la salida de cada comando create se mostrarán los detalles de la regla de firewall que ha creado, en formato JSON (de forma predeterminada). Si se produce un error, la salida muestra el texto del mensaje de error.

### <a name="list-firewall-rules"></a>Enumerar reglas de firewall 
Use el comando `az postgres flexible-server firewall-rule list` para mostrar las reglas de firewall de servidor existentes en el servidor. Observe que el atributo de nombre del servidor se especifica en el modificador **--name**. 
```azurecli-interactive
az postgres flexible-server firewall-rule list --name mydemoserver --resource-group testGroup
```
La salida enumera las reglas existentes, si existen, en formato JSON (de forma predeterminada). Puede usar el modificador --output table** para que los resultados se generen en un formato de tabla más legible.
```azurecli-interactive
az postgres flexible-server firewall-rule list --name mydemoserver --resource-group testGroup --output table
```

### <a name="update-a-firewall-rule"></a>Actualización de una regla de firewall
Use el comando `az postgres flexible-server firewall-rule update` para actualizar una regla de firewall existente en el servidor. Especifique el nombre de una regla de firewall existente como entrada, así como los atributos de dirección IP inicial y dirección IP final que se van a actualizar.
```azurecli-interactive
az postgres flexible-server firewall-rule update --name mydemoserver --rule-name FirewallRule1 --resource-group testGroup --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Cuando se realiza correctamente, la salida del comando muestra los detalles de la regla de firewall que ha actualizado, de forma predeterminada en formato JSON. Si se produce un error, la salida muestra el texto del mensaje de error.

> [!NOTE]
> Si la regla de firewall no existe, el comando update la crea.
### <a name="show-firewall-rule-details"></a>Mostrar los detalles de la regla de firewall
Use el comando `az postgres flexible-server firewall-rule show` para mostrar los detalles de la regla de firewall existente del servidor. Proporcione como entrada el nombre de una regla de firewall existente.
```azurecli-interactive
az postgres flexible-server firewall-rule show --name mydemoserver --rule-name FirewallRule1 --resource-group testGroup
```
Cuando se realiza correctamente, la salida del comando muestra los detalles de la regla de firewall que ha especificado, en formato JSON (de forma predeterminada). Si se produce un error, la salida muestra el texto del mensaje de error.

### <a name="delete-a-firewall-rule"></a>Eliminar una regla de firewall
Use el comando `az postgres flexible-server firewall-rule delete` para eliminar una regla de firewall existente del servidor. Proporcione el nombre de una regla de firewall existente.
```azurecli-interactive
az postgres flexible-server firewall-rule delete --name mydemoserver --rule-name FirewallRule1 --resource-group testGroup
```
Cuando se realiza correctamente, no hay ninguna salida. En caso de error, se muestra el texto del mensaje de error.

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre [Redes en Servidor flexible de Azure Database for PostgreSQL](./concepts-networking.md)
- Descripción más detallada sobre [Reglas de firewall para Azure Database for PostgreSQL: servidor flexible](./concepts-networking.md#public-access-allowed-ip-addresses).
- [Creación y administración de reglas de firewall de Azure Database for PostgreSQL: servidor flexible mediante Azure Portal](./how-to-manage-firewall-portal.md).
