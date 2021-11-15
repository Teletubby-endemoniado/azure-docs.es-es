---
title: Escalado y reducción horizontal del grupo de servidores de Hiperescala de Azure Database for PostgreSQL
description: Escalado y reducción horizontal del grupo de servidores de Hiperescala de Azure Database for PostgreSQL
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 11/03/2021
ms.topic: how-to
ms.openlocfilehash: 286754a121dc2aabeeacda47dd82dd4f3a1ed83b
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131564525"
---
# <a name="scale-out-and-in-your-azure-arc-enabled-postgresql-hyperscale-server-group-by-adding-more-worker-nodes"></a>Escalado y reducción horizontal del grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc agregando más nodos de trabajo
En este documento se explica cómo escalar y reducir horizontalmente un grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc. mediante un escenario. **Si no quiere seguir la explicación a través del escenario y solo quiere leer sobre cómo escalar horizontalmente, vaya al apartado [Escalado horizontal](#scale-out)** o [Reducción horizontal]().

El escalado horizontal se lleva a cabo cuando se agregan instancias de Postgres (nodos de trabajo de Hiperescala de Postgres) al grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc.

La reducción horizontal se lleva a cabo cuando se quitan instancias de Postgres (nodos de trabajo de Hiperescala de Postgres) del grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc.


[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="get-started"></a>Introducción
Si ya está familiarizado con el modelo de escalado de Hiperescala de PostgreSQL habilitada para Azure Arc o con Hiperescala (Citus) de Azure Database for PostgreSQL, puede saltarse este párrafo. Si no es así, se recomienda empezar a leer sobre este modelo de escalado en la página de documentación de Hiperescala (Citus) de Azure Database for PostgreSQL. Hiperescala (Citus) de Azure Database for PostgreSQL es la misma tecnología que se hospeda como un servicio en Azure (Plataforma como servicio, también conocida como PaaS) en lugar de ofrecerse como parte de Data Services habilitado para Azure Arc.
- [Nodos y tablas](../../postgresql/concepts-hyperscale-nodes.md)
- [Determinar el tipo de aplicación](../../postgresql/concepts-hyperscale-app-type.md)
- [Elección de una columna de distribución](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
- [Colocación de tabla](../../postgresql/concepts-hyperscale-colocation.md)
- [Distribución y modificación de tablas](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
- [Diseño de una base de datos multiinquilino](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
- [Diseño de un panel de análisis en tiempo real](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

> \* En los documentos anteriores, omita las secciones **Inicio de sesión en Azure Portal** y **Creación de una instancia de Azure Database for PostgreSQL: Hiperescala (Citus)** . Implemente los pasos restantes en la implementación de Azure Arc. Esas secciones son específicas de Hiperescala (Citus) de Azure Database for PostgreSQL, que se ofrece como un servicio PaaS en la nube de Azure, pero las demás partes de los documentos se aplican directamente a Hiperescala de PostgreSQL habilitada para Azure Arc.

## <a name="scenario"></a>Escenario
Este escenario hace referencia al grupo de servidores de Hiperescala de PostgreSQL que se creó como ejemplo en el documento [Creación de un grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc](create-postgresql-hyperscale-server-group.md).

### <a name="load-test-data"></a>Carga de datos de prueba
El escenario usa una muestra de datos de GitHub disponibles públicamente en el [sitio web de Citus Data](https://www.citusdata.com/) (Citus Data forma parte de Microsoft).

#### <a name="connect-to-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Conexión al grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc

##### <a name="list-the-connection-information"></a>Enumeración de la información de conexión
Para conectarse al grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc, primero obtenga la información de conexión. El formato general de este comando es:
```azurecli
az postgres arc-server endpoint list -n <server name>  --k8s-namespace <namespace> --use-k8s
```
Por ejemplo:
```azurecli
az postgres arc-server endpoint list -n postgres01  --k8s-namespace arc --use-k8s
```

Salida de ejemplo:

```console
{
  "instances": [
    {
      "endpoints": [
        {
          "description": "PostgreSQL Instance",
          "endpoint": "postgresql://postgres:<replace with password>@12.345.567.89:5432"
        },
        {
          "description": "Log Search Dashboard",
          "endpoint": "https://23.456.78.99:5601/app/kibana#/discover?_a=(query:(language:kuery,query:'custom_resource_name:postgres01'))"
        },
        {
          "description": "Metrics Dashboard",
          "endpoint": "https://34.567.890.12:3000/d/postgres-metrics?var-Namespace=arc&var-Name=postgres01"
        }
      ],
      "engine": "PostgreSql",
      "name": "postgres01"
    }
  ],
  "namespace": "arc"
}
```

##### <a name="connect-with-the-client-tool-of-your-choice"></a>Conexión con la herramienta de cliente de su elección

Ejecute la siguiente consulta para comprobar que actualmente tiene dos o más nodos de trabajo de Hiperescala, cada uno de los cuales corresponde a un pod de Kubernetes:

```sql
SELECT * FROM pg_dist_node;
```

```console
 nodeid | groupid |                       nodename                        | nodeport | noderack | hasmetadata | isactive | noderole | nodecluster | metadatasynced | shouldhaveshards
--------+---------+-------------------------------------------------------+----------+----------+-------------+----------+----------+-------------+----------------+------------------
      1 |       1 | pg1-1.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
      2 |       2 | pg1-2.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
(2 rows)
```

#### <a name="create-a-sample-schema"></a>Creación de un esquema de ejemplo
Ejecute la siguiente consulta para crear dos tablas:

```sql
CREATE TABLE github_events
(
    event_id bigint,
    event_type text,
    event_public boolean,
    repo_id bigint,
    payload jsonb,
    repo jsonb,
    user_id bigint,
    org jsonb,
    created_at timestamp
);

CREATE TABLE github_users
(
    user_id bigint,
    url text,
    login text,
    avatar_url text,
    gravatar_id text,
    display_login text
);
```

JSONB es el tipo de datos JSON en formato binario en PostgreSQL. Almacena un esquema flexible en una sola columna y con PostgreSQL. Este esquema tendrá un índice GIN para indexar todas las claves y los valores que contiene. Con un índice GIN, resulta más rápido y fácil realizar consultas con varias condiciones directamente en la carga. Ahora vamos a crear un par de índices antes de cargar los datos:

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

Para particionar tablas estándar, ejecute una consulta para cada tabla. Especifique la tabla que quiere particionar y la clave en la que se va a particionar. Vamos a particionar la tabla de usuarios y de eventos en user_id:

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

#### <a name="load-sample-data"></a>Carga de datos de muestra
Cargue los datos con el comando COPY … FROM PROGRAM:

```sql
COPY github_users FROM PROGRAM 'curl "https://examples.citusdata.com/users.csv"' WITH ( FORMAT CSV );
COPY github_events FROM PROGRAM 'curl "https://examples.citusdata.com/events.csv"' WITH ( FORMAT CSV );
```

#### <a name="query-the-data"></a>Consultar los datos
Ahora mida cuánto tiempo tarda una consulta simple con dos nodos:

```sql
SELECT COUNT(*) FROM github_events;
```
Tome nota del tiempo de ejecución de la consulta.


## <a name="scale-out"></a>Escalado horizontal
El formato general del comando de escalado horizontal es el siguiente:
```azurecli
az postgres arc-server edit -n <server group name> -w <target number of worker nodes> --k8s-namespace <namespace> --use-k8s
```


En este ejemplo, aumentamos el número de nodos de trabajo de 2 a 4, ejecute el siguiente comando:

```azurecli
az postgres arc-server edit -n postgres01 -w 4 --k8s-namespace arc --use-k8s 
```

Al agregar nodos, verá que el grupo de servidores tiene un estado pendiente. Por ejemplo:
```azurecli
az postgres arc-server list --k8s-namespace <namespace> --use-k8s
```

```console
{
    "name": "postgres01",
    "replicas": 1,
    "state": "Updating",
    "workers": 4
  }
```

Una vez que los nodos están disponibles, el reequilibrador de particiones de Hiperescala se ejecuta automáticamente y redistribuye los datos en los nuevos nodos. La operación de escalado horizontal se realiza en línea. Mientras se agregan los nodos y los datos se redistribuyen entre ellos, estos datos siguen estando disponibles para las consultas.

### <a name="verify-the-new-shape-of-the-server-group-optional"></a>Comprobación de la nueva forma del grupo de servidores (opcional)
Use cualquiera de los métodos siguientes para comprobar que el grupo de servidores ahora usa los nodos de trabajo adicionales que ha agregado.

#### <a name="with-azure-cli-az"></a>Con la CLI de Azure (az):

Ejecute el comando:

```azurecli
az postgres arc-server list --k8s-namespace arc --use-k8s
```

Devuelve la lista de los grupos de servidores creados en el espacio de nombres e indica su número de nodos de trabajo. Por ejemplo:
```console
{
    "name": "postgres01",
    "replicas": 1,
    "state": "Ready",
    "workers": 4
  }
```

#### <a name="with-kubectl"></a>kubectl
Ejecute el comando:
```console
kubectl get postgresqls -n arc
```

Devuelve la lista de los grupos de servidores creados en el espacio de nombres e indica su número de nodos de trabajo. Por ejemplo:
```console
NAME         STATE   READY-PODS   PRIMARY-ENDPOINT     AGE
postgres01   Ready   5/5          12.345.567.89:5432   9d
```
Tenga en cuenta que hay un pod más que el número de nodos de trabajo. El pod adicional se usa para hospedar la instancia de Postgres que tiene el rol de coordinación.

#### <a name="with-a-sql-query"></a>Consulta SQL
Conéctese al grupo de servidores con la herramienta cliente que prefiera y ejecute la siguiente consulta:

```sql
SELECT * FROM pg_dist_node;
```

```console
 nodeid | groupid |                       nodename                        | nodeport | noderack | hasmetadata | isactive | noderole | nodecluster | metadatasynced | shouldhaveshards
--------+---------+-------------------------------------------------------+----------+----------+-------------+----------+----------+-------------+----------------+------------------
      1 |       1 | pg1-1.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
      2 |       2 | pg1-2.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
      3 |       3 | pg1-3.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
      4 |       4 | pg1-4.pg1-svc.default.svc.cluster.local |     5432 | default  | f           | t        | primary  | default     | f              | t
(4 rows)
```

## <a name="return-to-the-scenario"></a>Vuelta al escenario

Si quiere comparar el tiempo de ejecución de la consulta de recuento seleccionada con el conjunto de datos de ejemplo, use la misma consulta de recuento. Se puede usar en los cuatro nodos de trabajo sin tener que realizar ningún cambio en la instrucción SQL.

```sql
SELECT COUNT(*) FROM github_events;
```
Anote el tiempo de ejecución.


> [!NOTE]
> En función de su entorno (por ejemplo, si ha implementado el grupo de servidores de prueba con `kubeadm` en una máquina virtual de un solo nodo), es posible que vea una leve mejora en el tiempo de ejecución. Para hacerse una idea más clara del tipo de mejora del rendimiento que podría obtener con Hiperescala de PostgreSQL habilitada para Azure Arc, vea los siguientes vídeos cortos:
>* [HTAP de alto rendimiento con Hiperescala (Citus) de Azure PostgreSQL](https://www.youtube.com/watch?v=W_3e07nGFxY)
>* [Creación de aplicaciones de HTAP con Python y con Hiperescala (Citus) de Azure PostgreSQL](https://www.youtube.com/watch?v=YDT8_riLLs0)

## <a name="scale-in"></a>Reducción horizontal
Para reducir horizontalmente (reducir el número de nodos de trabajo del grupo de servidores), use el mismo comando que para escalar horizontalmente, pero indique un número menor de nodos de trabajo. Los nodos de trabajo que se quitan son los que se han agregado más recientemente al grupo de servidores. Al ejecutar este comando, el sistema mueve los datos de los nodos que se quitan y los redistribuye (reajusta) automáticamente en los nodos restantes. 

El formato general del comando de reducción horizontal es el siguiente:
```azurecli
az postgres arc-server edit -n <server group name> -w <target number of worker nodes> --k8s-namespace <namespace> --use-k8s
```

La operación de reducción horizontal se realiza en línea. Las aplicaciones siguen teniendo acceso a los datos sin tiempo de inactividad mientras se quitan los nodos y los datos se redistribuyen entre los nodos restantes.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [escalar y reducir verticalmente (memoria y núcleos virtuales) el grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc](scale-up-down-postgresql-hyperscale-server-group-using-cli.md).
- Obtenga información sobre cómo establecer parámetros de servidor en el grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc.
- Lea los conceptos y las guías paso a paso de Hiperescala de Azure Database for PostgreSQL para distribuir los datos entre varios nodos de Hiperescala de PostgreSQL y poder beneficiarse de toda la eficacia de esta opción. :
    * [Nodos y tablas](../../postgresql/concepts-hyperscale-nodes.md)
    * [Determinar el tipo de aplicación](../../postgresql/concepts-hyperscale-app-type.md)
    * [Elección de una columna de distribución](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Colocación de tabla](../../postgresql/concepts-hyperscale-colocation.md)
    * [Distribución y modificación de tablas](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Diseño de una base de datos multiinquilino](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Diseño de un panel de análisis en tiempo real](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

 > \* En los documentos anteriores, omita las secciones **Inicio de sesión en Azure Portal** y **Creación de una instancia de Azure Database for PostgreSQL: Hiperescala (Citus)** . Implemente los pasos restantes en la implementación de Azure Arc. Esas secciones son específicas de Hiperescala (Citus) de Azure Database for PostgreSQL, que se ofrece como un servicio PaaS en la nube de Azure, pero las demás partes de los documentos se aplican directamente a Hiperescala de PostgreSQL habilitada para Azure Arc.

- [Conceptos de almacenamiento de Kubernetes y configuración de almacenamiento](storage-configuration.md)
- [Modelo de recursos de Kubernetes](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/scheduling/resources.md#resource-quantities)
