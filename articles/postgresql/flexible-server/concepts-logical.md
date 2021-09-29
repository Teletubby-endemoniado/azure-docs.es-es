---
title: 'Replicación lógica y descodificación lógica: Azure Database for PostgreSQL con servidor flexible'
description: Obtenga más información sobre la replicación lógica y la descodificación lógica en Azure Database for PostgreSQL con servidor flexible
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/30/2021
ms.openlocfilehash: 5030abe5dbc93a1b04588d548d353701a5a77ad4
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128559273"
---
# <a name="logical-replication-and-logical-decoding-in-azure-database-for-postgresql---flexible-server"></a>Replicación lógica y descodificación lógica en Azure Database for PostgreSQL con servidor flexible



Azure Database for PostgreSQL: servidor flexible admite las siguientes metodologías de replicación y extracción de datos lógicos:
1. **Replicación lógica**
   1. Uso de la [replicación lógica nativa](https://www.postgresql.org/docs/12/logical-replication.html) de PostgreSQL para replicar objetos de datos. La replicación lógica permite un control preciso sobre la replicación de datos, incluida la replicación de datos de nivel de tabla.
   2. El uso de la extensión [pglogical](https://github.com/2ndQuadrant/pglogical) que proporciona replicación lógica de streaming y funcionalidades adicionales, como la copia del esquema inicial de la base de datos, la compatibilidad con TRUNCATE, la capacidad de replicar DDL, etc. 
2. La **descodificación lógica** que se implementa mediante la [descodificación](https://www.postgresql.org/docs/12/logicaldecoding-explanation.html) del contenido del registro de escritura previa (WAL). 

## <a name="comparing-logical-replication-and-logical-decoding"></a>Comparación de la replicación lógica y la descodificación lógica
La replicación lógica y la descodificación lógica tienen varias similitudes. Ambas
* le permiten replicar datos de Postgres
* usan el [registro de escritura previa (WAL)](https://www.postgresql.org/docs/current/wal.html) como origen de los cambios
* usan [ranuras de replicación lógica](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS) para enviar datos. Una ranura representa una secuencia de cambios.
* usan la [propiedad REPLICA IDENTITY](https://www.postgresql.org/docs/current/sql-altertable.html#SQL-CREATETABLE-REPLICA-IDENTITY) de una tabla para determinar qué cambios se pueden enviar
* no replican cambios de DDL


Las dos tecnologías tienen sus diferencias: Replicación lógica 
* permite especificar una tabla o un conjunto de tablas que se van a replicar
* replica datos entre instancias de PostgreSQL

Descodificación lógica 
* extrae los cambios en todas las tablas de una base de datos 
* no se pueden enviar datos directamente entre instancias de PostgreSQL.

>[!NOTE]
> En este momento, la opción de servidor flexible no admite réplicas de lectura entre regiones. En función del tipo de carga de trabajo, puede optar por usar la característica de replicación lógica para la recuperación ante desastres (DR) entre regiones.

## <a name="pre-requisites-for-logical-replication-and-logical-decoding"></a>Requisitos previos para la comparación de la replicación lógica y la descodificación lógica

1. Vaya a la página de parámetros de servidor en el portal.
2. Configure el parámetro `wal_level` del servidor como `logical`.
3. Si desea usar la extensión pglogical, busque el parámetro `shared_preload_libaries` y seleccione `pglogical` en el cuadro desplegable.
4. Actualice el valor del parámetro `max_worker_processes` a 16, como mínimo. De lo contrario, puede encontrarse con problemas, como `WARNING: out of background worker slots`.
5. Guarde los cambios y reinicie el servidor para aplicar el cambio de `wal_level`.
6. Confirme que la instancia de PostgreSQL permite el tráfico de red desde el recurso de conexión.
7. Conceda al usuario administrador permisos de replicación.
   ```SQL
   ALTER ROLE <adminname> WITH REPLICATION;
   ```
8. Es posible que quiera asegurarse de que el rol que usa tenga [privilegios](https://www.postgresql.org/docs/current/sql-grant.html) en el esquema que va a replicar. De lo contrario, puede encontrarse con errores, como `Permission denied for schema`. 

## <a name="using-logical-replication-and-logical-decoding"></a>Uso de la replicación lógica y la descodificación lógica

### <a name="native-logical-replication"></a>Replicación lógica nativa
La replicación lógica utiliza los términos "publicador" y "suscriptor". 
* El publicador es la base de datos de PostgreSQL **desde** la que va a enviar datos. 
* El suscriptor es la base de datos de PostgreSQL **a** la que va a enviar datos.

A continuación se muestra código de ejemplo que puede usar para probar la replicación lógica.

1. Conéctese a la base de datos del publicador. Cree una tabla y agregue algunos datos.
   ```SQL
   CREATE TABLE basic(id SERIAL, name varchar(40));
   INSERT INTO basic(name) VALUES ('apple');
   INSERT INTO basic(name) VALUES ('banana');
   ```

2. Cree una publicación para la tabla.
   ```SQL
   CREATE PUBLICATION pub FOR TABLE basic;
   ```

3. Conéctese a la base de datos del suscriptor. Cree una tabla con el mismo esquema que en el publicador.
   ```SQL
   CREATE TABLE basic(id SERIAL, name varchar(40));
   ```

4. Cree una suscripción que se conectará a la publicación que creó anteriormente.
   ```SQL
   CREATE SUBSCRIPTION sub CONNECTION 'host=<server>.postgres.database.azure.com user=<admin> dbname=<dbname> password=<password>' PUBLICATION pub;
   ```

5. Ahora puede consultar la tabla en el suscriptor. Verá que ha recibido los datos del publicador.
   ```SQL
   SELECT * FROM basic;
   ```
   Puede agregar más filas a la tabla del publicador y ver los cambios en el suscriptor.

   Si no puede ver los datos, habilite el privilegio de inicio de sesión para `azure_pg_admin` y compruebe el contenido de la tabla. 
   ```SQL 
   ALTER ROLE azure_pg_admin login;
   ```


Visite la documentación de PostgreSQL para comprender mejor la [replicación lógica](https://www.postgresql.org/docs/current/logical-replication.html).

### <a name="pglogical-extension"></a>Extensión pglogical

Este es un ejemplo de configuración de pglogical en el servidor de bases de datos de proveedor y suscriptor. Para más información, consulte la documentación de la extensión pglogical. Asegúrese también de que ha realizado las tareas de requisitos previos enumeradas anteriormente.

1. Instale la extensión pglogical en los servidores de bases de datos del proveedor y el suscriptor.
    ```SQL
   \C myDB
   CREATE EXTENSION pglogical;
   ```
2. En el servidor de bases de datos del **proveedor** (origen/publicador), cree el nodo de proveedor.
   ```SQL
   select pglogical.create_node( node_name := 'provider1', 
   dsn := ' host=myProviderServer.postgres.database.azure.com port=5432 dbname=myDB user=myUser password=myPassword');
   ```
3. Cree un conjunto de replicación.
   ```SQL
   select pglogical.create_replication_set('myreplicationset');
   ```
4. Agregue todas las tablas de la base de datos al conjunto de replicación.
   ```SQL
   SELECT pglogical.replication_set_add_all_tables('myreplicationset', '{public}'::text[]);
   ```

   Como método alternativo, también puede agregar tablas de un esquema específico (por ejemplo, testUser) a un conjunto de replicación predeterminado.
   ```SQL
   SELECT pglogical.replication_set_add_all_tables('default', ARRAY['testUser']);
   ```

5. En el servidor de bases de datos del **suscriptor**, cree un nodo de suscriptor.
   ```SQL
   select pglogical.create_node( node_name := 'subscriber1', 
   dsn := ' host=mySubscriberServer.postgres.database.azure.com port=5432 dbname=myDB user=myUser password=myPasword' );
   ```
6. Cree una suscripción para iniciar el proceso de sincronización y replicación.
    ```SQL
   select pglogical.create_subscription (
   subscription_name := 'subscription1',
   replication_sets := array['myreplicationset'],
   provider_dsn := 'host=myProviderServer.postgres.database.azure.com port=5432 dbname=myDB user=myUser password=myPassword');
   ```
7. A continuación, puede comprobar el estado de la suscripción.
   ```SQL
   SELECT subscription_name, status FROM pglogical.show_subscription_status();
   ```

### <a name="logical-decoding"></a>Descodificación lógica
La descodificación lógica se puede consumir mediante el protocolo de streaming o la interfaz SQL. 

#### <a name="streaming-protocol"></a>Protocolo de streaming
El consumo de los cambios mediante el protocolo de streaming suele ser preferible. Puede crear su propio consumidor o conector o usar un servicio de terceros como [Debezium](https://debezium.io/). 

Consulte la documentación de wal2json para obtener [un ejemplo de uso del protocolo de streaming con pg_recvlogical](https://github.com/eulerto/wal2json#pg_recvlogical).

#### <a name="sql-interface"></a>Interfaz SQL 
En el ejemplo siguiente, usamos la interfaz SQL con el complemento wal2json.
 
1. Cree una ranura.
   ```SQL
   SELECT * FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
   ```
 
2. Emita comandos SQL. Por ejemplo:
   ```SQL
   CREATE TABLE a_table (
      id varchar(40) NOT NULL,
      item varchar(40),
      PRIMARY KEY (id)
   );
   
   INSERT INTO a_table (id, item) VALUES ('id1', 'item1');
   DELETE FROM a_table WHERE id='id1';
   ```

3. Consuma los cambios.
   ```SQL
   SELECT data FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', '1');
   ```

   La salida tendrá el siguiente aspecto:
   ```
   {
         "change": [
         ]
   }
   {
         "change": [
                  {
                           "kind": "insert",
                           "schema": "public",
                           "table": "a_table",
                           "columnnames": ["id", "item"],
                           "columntypes": ["character varying(40)", "character varying(40)"],
                           "columnvalues": ["id1", "item1"]
                  }
         ]
   }
   {
         "change": [
                  {
                           "kind": "delete",
                           "schema": "public",
                           "table": "a_table",
                           "oldkeys": {
                                 "keynames": ["id"],
                                 "keytypes": ["character varying(40)"],
                                 "keyvalues": ["id1"]
                           }
                  }
         ]
   }
   ```

4. Elimine la ranura cuando haya terminado de usarla.
   ```SQL
   SELECT pg_drop_replication_slot('test_slot'); 
   ```

Visite la documentación de PostgreSQL para comprender mejor la [descodificación lógica](https://www.postgresql.org/docs/current/logicaldecoding.html).


## <a name="monitoring"></a>Supervisión
Debe supervisar la descodificación lógica. Se debe eliminar cualquier ranura de replicación no utilizada. Las ranuras se conservan en los registros de WAL de Postgres y los catálogos del sistema correspondientes hasta que se hayan leído los cambios. Si se produce un error en el consumidor o el suscriptor o no se ha configurado correctamente, los registros no consumidos se acumularán y llenarán el espacio de almacenamiento. Además, los registros no consumidos aumentan el riesgo de un salto del identificador de transacción. Ambas situaciones pueden hacer que el servidor deje de estar disponible. Por lo tanto, es fundamental que las ranuras de replicación lógicas se consuman continuamente. Si ya no se usa una ranura de replicación lógica, elimínela inmediatamente.

La columna "active" (activo) en la vista pg_replication_slots indicará si hay un consumidor conectado a una ranura.
```SQL
SELECT * FROM pg_replication_slots;
```
[Establezca alertas](howto-alert-on-metrics.md) para las métricas **Máximo de identificadores de transacción usados** y **Almacenamiento usado** del servidor flexible para recibir una notificación cuando los valores aumenten por encima de los umbrales normales. 

## <a name="limitations"></a>Limitaciones
* Las limitaciones de la **replicación lógica** se aplican como se documenta [aquí](https://www.postgresql.org/docs/12/logical-replication-restrictions.html).
* **Réplicas de lectura**: las réplicas de lectura de Azure Database for PostgreSQL no se admiten actualmente en los servidores flexibles.
* **Ranuras y conmutación por error de alta disponibilidad**: las ranuras de replicación de alta disponibilidad del servidor principal no están disponibles en el servidor en espera de la zona de disponibilidad secundaria. Esto se aplica a usted si el servidor usa la opción de alta disponibilidad con redundancia de zona. En el caso de una conmutación por error al servidor en espera, las ranuras de replicación lógica no estarán disponibles en dicho servidor.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre las [opciones de red](concepts-networking.md)
* Obtenga más información sobre las [extensiones](concepts-extensions.md) disponibles en el servidor flexible
* Obtenga más información sobre la [alta disponibilidad](concepts-high-availability.md)

