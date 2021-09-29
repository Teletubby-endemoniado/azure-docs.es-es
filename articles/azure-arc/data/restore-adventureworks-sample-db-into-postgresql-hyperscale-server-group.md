---
title: Importación de la base de datos de ejemplo AdventureWorks en Hiperescala de PostgreSQL habilitada para Azure Arc
description: Restauración de la base de datos de ejemplo AdventureWorks en Hiperescala de PostgreSQL habilitada para Azure Arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 06/02/2021
ms.topic: how-to
ms.openlocfilehash: f3174e0980470f9ad23a53810767d75ba375bd9e
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128650180"
---
# <a name="import-the-adventureworks-sample-database-to-azure-arc-enabled-postgresql-hyperscale"></a>Importación de la base de datos de ejemplo AdventureWorks en Hiperescala de PostgreSQL habilitada para Azure Arc

[AdventureWorks](/sql/samples/adventureworks-install-configure) es una base de datos de ejemplo que contiene una base de datos OLTP que se usa en los tutoriales y ejemplos. Microsoft la proporciona y mantiene como parte del [repositorio de GitHub de ejemplos de SQL Server](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases).

Un proyecto de código abierto ha convertido la base de datos AdventureWorks para que sea compatible con Hiperescala de PostgreSQL habilitada para Azure Arc.
- [Proyecto original](https://github.com/lorint/AdventureWorks-for-Postgres)
- [Seguimiento del proyecto que hace una conversión previa de los archivos CSV para que sean compatibles con PostgreSQL](https://github.com/NorfolkDataSci/adventure-works-postgres)

En este documento se describe un proceso sencillo para importar la base de datos de ejemplo AdventureWorks en el grupo de servidores Hiperescala de PostgreSQL.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="download-the-adventureworks-backup-file"></a>Descarga del archivo de copia de seguridad de AdventureWorks

Descargue el archivo AdventureWorks.sql en el contenedor del grupo de servidores de Hiperescala de PostgreSQL. En este ejemplo, usaremos el comando `kubectl exec` para ejecutar de forma remota un comando en el contenedor del grupo de servidores de Hiperescala de PostgreSQL para descargar el archivo en ese contenedor. Puede descargar este archivo desde cualquier ubicación a la que tenga acceso `curl`. Use este mismo método si tiene otros archivos de copia de seguridad de base de datos que desea extraer en el contenedor del grupo de servidores de Hiperescala de PostgreSQL. Una vez que la copia de seguridad se encuentra en el contenedor del grupo de servidores de Hiperescala de PostgreSQL, es fácil crear la base de datos, el esquema y rellenar los datos.

Ejecute un comando como este para descargar los archivos. Reemplace antes el valor del nombre del pod y del espacio de nombres:

> [!NOTE]
>  El contenedor deberá tener conectividad a Internet a través del puerto 443 para descargar el archivo de GitHub.

> [!NOTE]
>  Use el nombre de pod del nodo de coordinación del grupo de servidores de Hiperescala de PostgreSQL. Su nombre es \<server group name\>c-0 (por ejemplo, postgres01c-0, donde c significa nodo de coordinación).  Si no está seguro del nombre del pod, ejecute el comando `kubectl get pod`.

```console
kubectl exec <PostgreSQL pod name> -n <namespace name> -c postgres  -- /bin/bash -c "cd /tmp && curl -k -O https://raw.githubusercontent.com/microsoft/azure_arc/main/azure_arc_data_jumpstart/aks/arm_template/postgres_hs/AdventureWorks.sql"

#Example:
#kubectl exec postgres02-0 -n arc -c postgres -- /bin/bash -c "cd /tmp && curl -k -O https://raw.githubusercontent.com/microsoft/azure_arc/main/azure_arc_data_jumpstart/aks/arm_template/postgres_hs/AdventureWorks.sql"
```

## <a name="step-2-import-the-adventureworks-database"></a>Paso 2: Importación de la base de datos AdventureWorks

Del mismo modo, puede ejecutar un comando kubectl exec para usar la herramienta CLI de psql que se incluye en los contenedores de grupos de servidores de Hiperescala de PostgreSQL para crear y cargar la base de datos.

Ejecute un comando como este para crear la base de datos vacía. Antes, sustituya el valor del nombre del pod y del espacio de nombres.

```console
kubectl exec <PostgreSQL pod name> -n <namespace name> -c postgres -- psql --username postgres -c 'CREATE DATABASE "adventureworks";'

#Example
#kubectl exec postgres02-0 -n arc -c postgres -- psql --username postgres -c 'CREATE DATABASE "adventureworks";'
```

A continuación, ejecute un comando como este para importar la base de datos; sustituya el valor del nombre del pod y del espacio de nombres antes de ejecutarlo.

```console
kubectl exec <PostgreSQL pod name> -n <namespace name> -c postgres -- psql --username postgres -d adventureworks -f /tmp/AdventureWorks.sql

#Example
#kubectl exec postgres02-0 -n arc -c postgres -- psql --username postgres -d adventureworks -f /tmp/AdventureWorks.sql
```


> **Nota: No verá muchas ventajas en cuando a rendimiento al realizar la ejecución en Hiperescala de PostgreSQL habilitada para Azure Arc hasta que escale horizontalmente y cree particiones o distribuya los datos y las tablas entre los nodos de trabajo del grupo de servidores de Hiperescala de PostgreSQL. Consulte la sección [Pasos siguientes sugeridos](#suggested-next-steps).**

## <a name="suggested-next-steps"></a>Pasos siguientes sugeridos
- Consulte los conceptos y las guías paso a paso de la hiperescala de Azure Database for PostgreSQL para distribuir los datos entre varios nodos de Hiperescala de PostgreSQL y beneficiarse de toda la eficacia de esta opción. :
    * [Nodos y tablas](../../postgresql/concepts-hyperscale-nodes.md)
    * [Determinar el tipo de aplicación](../../postgresql/concepts-hyperscale-app-type.md)
    * [Elección de una columna de distribución](../../postgresql/concepts-hyperscale-choose-distribution-column.md)
    * [Colocación de tabla](../../postgresql/concepts-hyperscale-colocation.md)
    * [Distribución y modificación de tablas](../../postgresql/howto-hyperscale-modify-distributed-tables.md)
    * [Diseño de una base de datos multiinquilino](../../postgresql/tutorial-design-database-hyperscale-multi-tenant.md)*
    * [Diseño de un panel de análisis en tiempo real](../../postgresql/tutorial-design-database-hyperscale-realtime.md)*

   > \* En los documentos anteriores, omita las secciones **Inicio de sesión en Azure Portal** y **Creación de una instancia de Azure Database for PostgreSQL: Hiperescala (Citus)** . Implemente los pasos restantes en la implementación de Azure Arc. Esas secciones son específicas de Hiperescala (Citus) de Azure Database for PostgreSQL que se ofrece como un servicio PaaS en la nube de Azure, pero las demás partes de los documentos se aplican directamente a Hiperescala de PostgreSQL habilitada para Azure Arc.

- [Escalado del grupo de servidores Hiperescala de Azure Database for PostgreSQL](scale-out-in-postgresql-hyperscale-server-group.md)
