---
title: 'Inicio rápido: Conexión mediante la CLI de Azure a Azure Database for MySQL Servidor flexible'
description: Esta guía de inicio rápido proporciona varias maneras de conectarse con la CLI de Azure a Azure Database for MySQL Servidor flexible.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.custom: mvc, devx-track-azurecli
ms.topic: quickstart
ms.date: 03/01/2021
ms.openlocfilehash: 26c25afc997ee86f0fe23f944ae5afad34269e92
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131468126"
---
# <a name="quickstart-connect-and-query-with-azure-cli--with-azure-database-for-mysql---flexible-server"></a>Inicio rápido: Conexión y consulta mediante la CLI de Azure a Azure Database for MySQL Servidor flexible

[[!INCLUDE[applies-to-mysql-flexible-server](../includes/applies-to-mysql-flexible-server.md)]

En este inicio rápido se muestra cómo puede conectarse a un servidor flexible de Azure Database for MySQL mediante la CLI de Azure con ```az mysql flexible-server connect``` y ejecutar una consulta única o un archivo SQL con el comando ```az mysql flexible-server execute```. Este comando permite probar la conectividad con el servidor de base de datos y ejecutar consultas. También puede ejecutar varias consultas mediante el modo interactivo.

## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. 

    [!INCLUDE [flexible-server-free-trial-note](../includes/flexible-server-free-trial-note.md)]
- Instale la última versión de la [CLI de Azure](/cli/azure/install-azure-cli) (2.20.0 o superior).
- Inicie sesión mediante la CLI de Azure con el comando ```az login```.
- Active la persistencia de parámetros con ```az config param-persist on```. La persistencia de parámetros le ayudará a usar el contexto local sin tener que repetir muchos argumentos, como el grupo de recursos, la ubicación, etc.

## <a name="create-a-mysql-flexible-server"></a>Creación de un servidor flexible MySQL

Lo primero que crearemos es un servidor administrado de MySQL. En [Azure Cloud Shell](https://shell.azure.com/), ejecute el siguiente script y anote el **nombre del servidor**, el **nombre de usuario** y la **contraseña** generados a partir de este comando.

```azurecli
az mysql flexible-server create --public-access <your-ip-address>
```

Puede proporcionar argumentos adicionales a este comando para personalizarlo. Consulte todos los argumentos del comando [az mysql flexible-server create](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_create).

## <a name="create-a-database"></a>Crear una base de datos
Ejecute el siguiente comando para crear una base de datos, **newdatabase**, si aún no ha creado una.

```azurecli
az mysql flexible-server db create -d newdatabase
```

## <a name="view-all-the-arguments"></a>Visualización de todos los argumentos
Puede ver todos los argumentos de este comando con el argumento ```--help```.

```azurecli
az mysql flexible-server connect --help
```

## <a name="test-database-server-connection"></a>Prueba de la conexión al servidor de bases de datos
Ejecute el script siguiente para probar y validar la conexión a la base de datos desde el entorno de desarrollo.

```azurecli
az mysql flexible-server connect -n <servername> -u <username> -p <password> -d <databasename>
```

**Ejemplo**:
```azurecli
az mysql flexible-server connect -n mysqldemoserver1 -u dbuser -p "dbpassword" -d newdatabase
```

Debería ver la siguiente salida para una conexión correcta:

```output
Command group 'mysql flexible-server' is in preview and under development. Reference and support levels: https://aka.ms/CLI_refstatus
Connecting to newdatabase database.
Successfully connected to mysqldemoserver1.
```
Si se produjo un error en la conexión, pruebe estas soluciones:
- Compruebe si el puerto 3306 está abierto en la máquina cliente.
- Compruebe si el nombre de usuario y la contraseña del administrador del servidor son correctos.
- Compruebe si ha configurado una regla de firewall para la máquina cliente.
- Compruebe si ha configurado el servidor con acceso privado en redes virtuales y asegúrese de que la máquina cliente se encuentre en la misma red virtual.

## <a name="run-multiple-queries-using-interactive-mode"></a>Ejecución de varias consultas mediante el modo interactivo
Puede ejecutar varias consultas mediante el modo **interactivo**. Para habilitar el modo interactivo, ejecute el siguiente comando.

```azurecli
az mysql flexible-server connect -n <server-name> -u <username> -p <password> --interactive
```

**Ejemplo**:
```azurecli
az mysql flexible-server connect -n mysqldemoserver1 -u dbuser -p "dbpassword" -d newdatabase --interactive
```

Verá la experiencia del shell de **MySQL**, como se muestra a continuación:

```bash
Command group 'mysql flexible-server' is in preview and under development. Reference and support levels: https://aka.ms/CLI_refstatus
Password:
mysql 5.7.29-log
mycli 1.22.2
Chat: https://gitter.im/dbcli/mycli
Mail: https://groups.google.com/forum/#!forum/mycli-users
Home: http://mycli.net
Thanks to the contributor - Martijn Engler
newdatabase> CREATE TABLE table1 (id int NOT NULL, val int,txt varchar(200));
Query OK, 0 rows affected
Time: 2.290s
newdatabase1> INSERT INTO table1 values (1,100,'text1');
Query OK, 1 row affected
Time: 0.199s
newdatabase1> SELECT * FROM table1;
+----+-----+-------+
| id | val | txt   |
+----+-----+-------+
| 1  | 100 | text1 |
+----+-----+-------+
1 row in set
Time: 0.149s
newdatabase>exit;
Goodbye!
Local context is turned on. Its information is saved in working directory C:\mydir. You can run `az local-context off` to turn it off.
Your preference of  are now saved to local context. To learn more, type in `az local-context --help`
```

## <a name="run-single-query"></a>Ejecución de una sola consulta
Ejecute el siguiente comando para ejecutar una consulta única con el argumento ```--querytext```, ```-q```.

```azurecli
az mysql flexible-server execute -n <server-name> -u <username> -p "<password>" -d <database-name> --querytext "<query text>"
```

**Ejemplo**:
```azurecli
az mysql flexible-server execute -n mysqldemoserver1 -u dbuser -p "dbpassword" -d newdatabase -q "select * from table1;" --output table
```

Verá una salida como la que se muestra a continuación:

```output
Command group 'mysql flexible-server' is in preview and under development. Reference and support levels: https://aka.ms/CLI_refstatus
Successfully connected to mysqldemoserver1.
Ran Database Query: 'select * from table1;'
Retrieving first 30 rows of query output, if applicable.
Closed the connection to mysqldemoserver1
Local context is turned on. Its information is saved in working directory C:\Users\sumuth. You can run `az local-context off` to turn it off.
Your preference of  are now saved to local context. To learn more, type in `az local-context --help`
Txt    Val
-----  -----
test   200
test   200
test   200
test   200
test   200
test   200
test   200
```

## <a name="run-sql-file"></a>Ejecutar archivo SQL
Puede ejecutar un archivo SQL con el comando mediante el argumento ```--file-path```, ```-q```.

```azurecli
az mysql flexible-server execute -n <server-name> -u <username> -p "<password>" -d <database-name> --file-path "<file-path>"
```

**Ejemplo**:
```azurecli
az mysql flexible-server execute -n mysqldemoserver -u dbuser -p "dbpassword" -d flexibleserverdb -f "./test.sql"
```

Verá una salida como la que se muestra a continuación:

```output
Command group 'mysql flexible-server' is in preview and under development. Reference and support levels: https://aka.ms/CLI_refstatus
Running sql file '.\test.sql'...
Successfully executed the file.
Closed the connection to mysqldemoserver.
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
* [Conexión al servidor flexible de Azure Database for MySQL con conexiones cifradas](how-to-connect-tls-ssl.md)
* [Administración del servidor](./how-to-manage-server-cli.md)

