---
title: Selección de la ubicación de un grupo de servidores de Hiperescala de PostgreSQL en nodos de clúster de Kubernetes
description: Se explica cómo se colocan instancias de PostgreSQL, que forman un grupo de servidores de Hiperescala de PostgreSQL, en nodos de clúster de Kubernetes.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 11/03/2021
ms.topic: how-to
ms.openlocfilehash: 05dd347914d7be942c00232de78cf89484f07555
ms.sourcegitcommit: e41827d894a4aa12cbff62c51393dfc236297e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2021
ms.locfileid: "131555331"
---
# <a name="azure-arc-enabled-postgresql-hyperscale-server-group-placement"></a>Selección de la ubicación de un grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc

En este artículo, usamos un ejemplo para ilustrar cómo se selecciona la ubicación de las instancias de PostgreSQL de un grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc en los nodos físicos del clúster de Kubernetes que los hospeda. 

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="configuration"></a>Configuración

En este ejemplo, se usa un clúster de Azure Kubernetes Service (AKS) que tiene cuatro nodos físicos. 

:::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/1_cluster_portal.png" alt-text="Clúster de AKS de 4 nodos en Azure Portal":::

Enumere los nodos físicos del clúster de Kubernetes. Ejecute el comando:

```console
kubectl get nodes
```

`kubectl` devuelve los cuatro nodos físicos del clúster de Kubernetes:

```output
NAME                                STATUS   ROLES   AGE   VERSION
aks-agentpool-42715708-vmss000000   Ready    agent   11h   v1.17.9
aks-agentpool-42715708-vmss000001   Ready    agent   11h   v1.17.9
aks-agentpool-42715708-vmss000002   Ready    agent   11h   v1.17.9
aks-agentpool-42715708-vmss000003   Ready    agent   11h   v1.17.9
```

La arquitectura puede representarse como:

:::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/2_logical_cluster.png" alt-text="Representación lógica de 4 nodos agrupados en un clúster de Kubernetes":::

El clúster de Kubernetes hospeda un controlador de datos de Azure Arc y un grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc. Este grupo de servidores está compuesto por tres instancias de PostgreSQL: un coordinador y dos trabajos.

Enumere los pods con el comando:

```console
kubectl get pods -n arc3
```
`kubectl` devuelve lo siguiente:

```output
NAME                 READY   STATUS    RESTARTS   AGE
…
postgres01c-0         3/3     Running   0          9h
postgres01w-0         3/3     Running   0          9h
postgres01w-1         3/3     Running   0          9h
```
Cada uno de esos pods hospeda una instancia de PostgreSQL. Juntos, los pods forman el grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc:

```output
Pod name        Role in the server group
postgres01c-0 Coordinator
postgres01w-0   Worker
postgres01w-1   Worker
```

## <a name="placement"></a>Selección de ubicación
Echemos un vistazo a cómo Kubernetes coloca los pods del grupo de servidores. Describa cada pod e identifique en qué nodo físico del clúster de Kubernetes se coloca. Por ejemplo, para el coordinador, ejecute el siguiente comando:

```console
kubectl describe pod postgres01c-0 -n arc3
```

`kubectl` devuelve lo siguiente:

```output
Name:         postgres01c-0
Namespace:    arc3
Priority:     0
Node:         aks-agentpool-42715708-vmss000000
Start Time:   Thu, 17 Sep 2020 00:40:33 -0700
…
```

Al ejecutar este comando para cada uno de los pods, se resume la selección de ubicación actual de la siguiente manera:

| Rol de grupo de servidor|Pod de grupo de servidor|Nodo físico de Kubernetes que hospeda el pod |
|--|--|--|
| Trabajo|postgres01-1|aks-agentpool-42715708-vmss000002 |
| Trabajo|postgres01-2|aks-agentpool-42715708-vmss000003 |

Además, observe, en la descripción de los pods, los nombres de los contenedores que cada pod hospeda. Por ejemplo, para el segundo trabajo, ejecute el siguiente comando:

```console
kubectl describe pod postgres01w-1 -n arc3
```

`kubectl` devuelve lo siguiente:

```output
…
Node:         aks-agentpool-42715708-vmss000003/10.240.0.7
..
Containers:
  Fluentbit:
…
  Postgres:
…
  Telegraf:
…
```

Cada pod que forma parte del grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc hospeda los tres contenedores siguientes:

|Contenedores|Descripción
|----|----|
|`Fluentbit` |Recopilador de registros * de datos: https://fluentbit.io/
|`Postgres`|La instancia de PostgreSQL forma parte del grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc.
|`Telegraf` |Recopilador de métricas: https://www.influxdata.com/time-series-platform/telegraf/

La arquitectura tiene el siguiente aspecto:

:::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/3_pod_placement.png" alt-text="3 pods, cada uno colocado en nodos independientes":::

Esto significa que, en este momento, cada instancia de PostgreSQL que constituye el grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc se hospeda en un host físico específico dentro del contenedor de Kubernetes. Esta configuración proporciona el máximo rendimiento del grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc, ya que cada rol (coordinador y trabajos) usa los recursos de cada nodo físico. Estos recursos no se comparten entre varios roles de PostgreSQL.

## <a name="scale-out-azure-arc-enabled-postgresql-hyperscale"></a>Escalado horizontal de Hiperescala de PostgreSQL habilitada para Azure Arc

Ahora, vamos a escalar horizontalmente para agregar un tercer nodo de trabajo al grupo de servidores y observar lo que sucede. Se creará una cuarta instancia de PostgreSQL que se hospedará en un cuarto pod.
Para escalar horizontalmente, ejecute el comando:

```azurecli
az postgres arc-server edit --name postgres01 --workers 3 --k8s-namespace arc3 --use-k8s
```

Produce el siguiente resultado:

```output
Updating postgres01 in namespace `arc3`
postgres01 is Ready
```

Enumere los grupos de servidores implementados en el controlador de datos de Azure Arc y compruebe que el grupo de servidores se ejecuta ahora con tres trabajos. Ejecute el comando:

```azurecli
az postgres arc-server list --k8s-namespace arc3 --use-k8s
```

Y observe que se ha escalado horizontalmente de dos a tres trabajos:

```output
[
  {
    "name": "postgres01",
    "replicas": 1,
    "state": "Ready",
    "workers": 3
  }
]
```

Como hicimos anteriormente, observe que el grupo de servidores usa ahora un total de cuatro pods:

```console
kubectl get pods -n arc3
```

```output
NAME                 READY   STATUS    RESTARTS   AGE
…
postgres01c-0         3/3     Running   0          11h
postgres01w-0         3/3     Running   0          11h
postgres01w-1         3/3     Running   0          11h
postgres01w-2         3/3     Running   0          5m2s
```

Y describa el nuevo pod para identificar en qué nodos físicos del clúster de Kubernetes está hospedado.
Ejecute el comando:

```console
kubectl describe pod postgres01w-2 -n arc3
```

Para identificar el nombre del nodo de hospedaje:

```output
Name:         postgres01w-2
Namespace:    arc3
Priority:     0
Node:         aks-agentpool-42715708-vmss000000
```

La selección de la ubicación de las instancias de PostgreSQL en los nodos físicos del clúster es ahora:

|Rol de grupo de servidor|Pod de grupo de servidor|Nodo físico de Kubernetes que hospeda el pod
|-----|-----|-----
|Coordinador|postgres01c-0|aks-agentpool-42715708-vmss000000
|Trabajo|postgres01w-1|aks-agentpool-42715708-vmss000002
|Trabajo|postgres01w-2|aks-agentpool-42715708-vmss000003
|Trabajo|postgres01w-3|aks-agentpool-42715708-vmss000000

Y observe que el pod del nuevo trabajo (postgres01w-2) se ha colocado en el mismo nodo que el coordinador. 

La arquitectura tiene el siguiente aspecto:

:::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/4_pod_placement_.png" alt-text="Cuarto pod en el mismo nodo que el coordinador":::

¿Por qué el nuevo trabajador o pod no se coloca en el nodo físico que queda del clúster Kubernetes, aks-agentpool-42715708-vmss000003?

El motivo es que el último nodo físico del clúster de Kubernetes hospeda en realidad varios pods que hospedan componentes adicionales necesarios para ejecutar los servicios de datos habilitados para Azure Arc. Kubernetes evaluó que el mejor candidato, en el momento de la programación, para hospedar el trabajo adicional, es el nodo físico aks-agentpool-42715708-vmss000000. 

Si se usan los mismos comandos que antes; vemos lo que hospeda cada nodo físico:

|Otros nombres de pods\* |Uso|Nodo físico de Kubernetes que hospeda los pods
|----|----|----
|bootstrapper-jh48b|Se trata de un servicio que controla las solicitudes entrantes para crear, editar y eliminar recursos personalizados, como instancias administradas de SQL, grupos de servidores de Hiperescala de PostgreSQL y controladores de datos.|aks-agentpool-42715708-vmss000003
|control-gwmbs||aks-agentpool-42715708-vmss000002
|controldb-0|Almacén de datos del controlador que se usa para almacenar la configuración y el estado del controlador de datos.|aks-agentpool-42715708-vmss000001
|controlwd-zzjp7|Servicio de "guardián" del controlador que vigila la disponibilidad del controlador de datos.|aks-agentpool-42715708-vmss000000
|logsdb-0|Instancia de búsqueda elástica que se usa para almacenar todos los registros recopilados en los pods de servicios de datos de Arc. Elasticsearch, recibe datos del contenedor `Fluentbit` de cada pod|aks-agentpool-42715708-vmss000003
|logsui-5fzv5|Instancia de Kibana basada en la base de datos de búsqueda elástica para presentar una GUI de Log Analytics.|aks-agentpool-42715708-vmss000003
|metricsdb-0|Instancia de InfluxDB se usa para almacenar todas las métricas recopiladas en todos los pods de servicios de datos de Arc. InfluxDB, recibe datos del contenedor `Telegraf` de cada pod|aks-agentpool-42715708-vmss000000
|metricsdc-47d47|Conjunto de demonios implementado en todos los nodos de Kubernetes del clúster para recopilar las métricas de nivel de nodo acerca de los nodos.|aks-agentpool-42715708-vmss000002
|metricsdc-864kj|Conjunto de demonios implementado en todos los nodos de Kubernetes del clúster para recopilar las métricas de nivel de nodo acerca de los nodos.|aks-agentpool-42715708-vmss000001
|metricsdc-l8jkf|Conjunto de demonios implementado en todos los nodos de Kubernetes del clúster para recopilar las métricas de nivel de nodo acerca de los nodos.|aks-agentpool-42715708-vmss000003
|metricsdc-nxm4l|Conjunto de demonios implementado en todos los nodos de Kubernetes del clúster para recopilar las métricas de nivel de nodo acerca de los nodos.|aks-agentpool-42715708-vmss000000
|metricsui-4fb7l|Instancia de Grafana basada en la base de datos InfluxDB para presentar una GUI del panel de supervisión.|aks-agentpool-42715708-vmss000003
|mgmtproxy-4qppp|Esta capa de proxy de aplicación web se sitúa delante de las instancias de Grafana y Kibana.|aks-agentpool-42715708-vmss000002

> \* El sufijo de los nombres de pod variará en otras implementaciones. Además, aquí solo se enumeran los pods hospedados dentro del espacio de nombres de Kubernetes del controlador de datos de Azure Arc.

La arquitectura tiene el siguiente aspecto:

:::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/5_full_list_of_pods.png" alt-text="Todos los pods del espacio de nombres en varios nodos":::

Como se ha descrito anteriormente, los nodos de coordinador (pod 1) del grupo de servidores Hiperescala de Postgres habilitada para Azure Arc comparten los mismos recursos físicos que el tercer nodo de trabajo (pod 4) del grupo de servidores. Esto es aceptable porque el nodo de coordinación suele usar muy pocos recursos en comparación con los que usa un nodo de trabajo. Por esta razón, elija cuidadosamente:
- el tamaño del clúster de Kubernetes y las características de cada uno de sus nodos físicos (memoria, núcleo virtual)
- el número de nodos físicos dentro del clúster de Kubernetes
- las aplicaciones o cargas de trabajo que hospeda en el clúster de Kubernetes.

La implicación de hospedar demasiadas cargas de trabajo en el clúster de Kubernetes es que puede producirse una limitación en el grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc. En ese caso, no se beneficiará de su capacidad de escalar horizontalmente. El rendimiento que se obtiene del sistema no se limita a la selección de la ubicación o a las características físicas de los nodos físicos o del sistema de almacenamiento. El rendimiento que obtiene también abarca cómo configurar cada uno de los recursos que se ejecutan en el clúster de Kubernetes (incluido Hiperescala de PostgreSQL habilitada para Azure Arc), por ejemplo, las solicitudes y los límites que establece para memoria y núcleos virtuales. La cantidad de carga de trabajo que puede hospedar en un clúster de Kubernetes determinado es relativa a las características del clúster de Kubernetes, la naturaleza de las cargas de trabajo, el número de usuarios, el modo en que se realizan las operaciones del clúster Kubernetes…

## <a name="scale-out-aks"></a>Escalado horizontal de AKS

Vamos a mostrar que el escalado horizontal del clúster de AKS y el servidor Hiperescala de PostgreSQL habilitada para Azure Arc es una forma de aprovechar al máximo el alto rendimiento de Hiperescala de PostgreSQL habilitada para Azure Arc.
Vamos a agregar un quinto nodo al clúster de AKS:

:::row:::
    :::column:::
        Antes
    :::column-end:::
    :::column:::
        Después
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        :::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/6_layout_before.png" alt-text="Diseño de Azure Portal antes":::
    :::column-end:::
    :::column:::
        :::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/7_layout_after.png" alt-text="Diseño de Azure Portal después":::
    :::column-end:::
:::row-end:::

La arquitectura tiene el siguiente aspecto:

:::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/8_logical_layout_after.png" alt-text="Diseño lógico en el clúster de Kubernetes después de la actualización":::

Echemos un vistazo a qué pods del espacio de nombres del controlador de datos de Arc se hospedan en el nuevo nodo físico AKS mediante la ejecución del comando:

```console
kubectl describe node aks-agentpool-42715708-vmss000004
```

Y vamos a actualizar la representación de la arquitectura de nuestro sistema:

:::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/9_updated_list_of_pods.png" alt-text="Todos los pods del diagrama lógico del clúster":::

Podemos observar que el nuevo nodo físico del clúster de Kubernetes hospeda solo el pod de métricas necesario para los servicios de datos de Azure Arc. Observe que, en este ejemplo, nos centramos solo en el espacio de nombres del controlador de datos de Arc, por lo que no se representan los demás pods.

## <a name="scale-out-azure-arc-enabled-postgresql-hyperscale-again"></a>Nuevo escalado horizontal de Hiperescala de PostgreSQL habilitada para Azure Arc

El quinto nodo físico no hospeda todavía ninguna carga de trabajo. A medida que escalamos horizontalmente Hiperescala de PostgreSQL habilitada para Azure Arc, Kubernetes optimizará la selección de la ubicación del nuevo pod de PostgreSQL y no lo colocará en los nodos físicos que ya hospedan más cargas de trabajo. Ejecute el siguiente comando para escalar Hiperescala de PostgreSQL habilitada para Azure Arc de 3 a 4 nodos de trabajo. Al final de la operación, el grupo de servidores se constituirá y distribuirá en cinco instancias de PostgreSQL, un coordinador y cuatro trabajos.

```azurecli
az postgres arc-server edit --name postgres01 --workers 4 --k8s-namespace arc3 --use-k8s
```

Produce el siguiente resultado:

```output
Updating postgres01 in namespace `arc3`
postgres01 is Ready
```

Enumere los grupos de servidores implementados en el controlador de datos y compruebe que el grupo de servidores se ejecuta ahora con cuatro trabajos:

```azurecli
az postgres arc-server list --k8s-namespace arc3 --use-k8s
```

Observe que se ha escalado horizontalmente de tres a cuatro trabajos. 

```console
[
  {
    "name": "postgres01",
    "replicas": 1,
    "state": "Ready",
    "workers": 4
  }
]
```

Como hicimos anteriormente, observe que el grupo de servidores usa ahora cuatro pods:

```output
kubectl get pods -n arc3

NAME                 READY   STATUS    RESTARTS   AGE
…
postgres01c-0         3/3     Running   0          13h
postgres01w-0         3/3     Running   0          13h
postgres01w-1         3/3     Running   0          13h
postgres01w-2         3/3     Running   0          179m
postgres01w-3         3/3     Running   0          3m13s
```

La forma del grupo de servidores es ahora:

|Rol de grupo de servidor|Pod de grupo de servidor
|----|-----
|Coordinador|postgres01c-0
|Trabajo|postgres01w-0
|Trabajo|postgres01w-1
|Trabajo|postgres01w-2
|Trabajo|postgres01w-3

Vamos a describir el pod postgres01w-3 para identificar en qué nodo físico se hospeda:

```console
kubectl describe pod postgres01w-3 -n arc3
```

Observe en qué pod se ejecuta:

|Rol de grupo de servidor|Pod de grupo de servidor| Pod
|----|-----|------
|Coordinador|postgres01c-0|aks-agentpool-42715708-vmss000000
|Trabajo|postgres01w-0|aks-agentpool-42715708-vmss000002
|Trabajo|postgres01w-1|aks-agentpool-42715708-vmss000003
|Trabajo|postgres01w-2|aks-agentpool-42715708-vmss000000
|Trabajo|postgres01w-3|aks-agentpool-42715708-vmss000004

La arquitectura tiene el siguiente aspecto:

:::image type="content" source="media/migrate-postgresql-data-into-postgresql-hyperscale-server-group/10_kubernetes_schedules_newest_pod.png" alt-text="Kubernetes programa el pod más reciente en el nodo con menor uso":::

Kubernetes programó el nuevo pod de PostgreSQL en el nodo físico menos cargado del clúster de Kubernetes.

## <a name="summary"></a>Resumen

Para sacar el máximo partido de la escalabilidad y el rendimiento del escalado horizontal del grupo de servidores habilitados para Azure Arc, debe evitar la contención de recursos en el clúster de Kubernetes:
- entre el grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc y otras cargas de trabajo hospedadas en el mismo clúster de Kubernetes
- entre todas las instancias de PostgreSQL que constituyen el grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc

Existen varias maneras de conseguirlo:
- Escale horizontalmente Kubernetes e Hiperescala de Postgres habilitada para Azure Arc: considere la posibilidad de escalar horizontalmente el clúster de Kubernetes de la misma manera que escala el grupo de servidores Hiperescala de PostgreSQL habilitada para Azure Arc. Agregue un nodo físico al clúster para cada trabajo que agregue al grupo de servidores.
- Escale horizontalmente Hiperescala de Postgres habilitada para Azure Arc sin escalar horizontalmente Kubernetes: si establece las restricciones de recursos correctas (solicitud y límites de memoria y núcleos virtuales) en las cargas de trabajo hospedadas en Kubernetes (incluido Hiperescala de PostgreSQL habilitada para Azure Arc), permitirá la colocación de cargas de trabajo en Kubernetes y reducirá el riego de contención de recursos. Debe asegurarse de que las características físicas de los nodos físicos del clúster de Kubernetes pueden cumplir con las restricciones de recursos que defina. También debe asegurarse de que se mantiene el equilibrio a medida que las cargas de trabajo evolucionan a lo largo del tiempo o a medida que se agregan más cargas de trabajo en el clúster de Kubernetes.
- Use los mecanismos de Kubernetes (selector de pods, afinidad, antiafinidad) para influir en la colocación de los pods.

## <a name="next-steps"></a>Pasos siguientes

[Escalado horizontal del grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc mediante la adición de más nodos de trabajo](scale-out-in-postgresql-hyperscale-server-group.md)
