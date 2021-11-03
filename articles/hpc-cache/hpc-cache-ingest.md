---
title: Traslado de datos a un contenedor en la nube de Azure HPC Cache
description: Cómo rellenar Azure Blob Storage para usarlo con Azure HPC Cache
author: femila
ms.service: hpc-cache
ms.topic: how-to
ms.date: 06/30/2021
ms.author: femila
ms.openlocfilehash: 1c139bce487f734d8ce522b1ed83efa55d7894ea
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131087825"
---
# <a name="move-data-to-azure-blob-storage"></a>Traslado de datos a Azure Blob Storage

Si el flujo de trabajo incluye el movimiento de datos a Azure Blob Storage, asegúrese de que usa una estrategia eficaz. Puede precargar los datos en un nuevo contenedor de blobs antes de definirlo como destino de almacenamiento, o bien agregar el contenedor y, luego, copiar los datos mediante Azure HPC Cache.

En este artículo se explican las mejores formas de mover los datos a Blob Storage para usarlos con Azure HPC Cache.

> [!TIP]
>
> Este artículo no se aplica al almacenamiento de blobs montado en NFS (destinos de almacenamiento ADLS-NFS). Puede usar cualquier método basado en NFS para rellenar un contenedor de blobs ADLS-NFS antes de agregarlo a HPC Cache. Lea [Precarga de datos con el protocolo NFS](nfs-blob-considerations.md#pre-load-data-with-nfs-protocol) para más información.

Tenga en cuenta estos factores:

* Azure HPC Cache usa un formato de almacenamiento especializado para organizar los datos en Blob Storage. Este es el motivo de que un destino de Blob Storage deba ser un contenedor nuevo vacío o un contenedor de blobs que se usara anteriormente con los datos de Azure HPC Cache.

* La copia de datos mediante Azure HPC Cache a un destino de almacenamiento de back-end es más eficaz cuando se usan varios clientes y operaciones en paralelo. Un comando de copia sencillo desde un cliente moverá los datos lentamente.

Existe una utilidad basada en Python para cargar contenido en un contenedor de Blob Storage. Para más información, lea [Carga previa de datos en Blob Storage](#pre-load-data-in-blob-storage-with-clfsload).

Si no quiere usar la utilidad de carga o si quiere agregar contenido a un destino de almacenamiento existente, siga las sugerencias de ingesta de datos en paralelo en [Copia de datos mediante Azure HPC Cache](#copy-data-through-the-azure-hpc-cache).

## <a name="pre-load-data-in-blob-storage-with-clfsload"></a>Carga previa de datos en Blob Storage con CLFSLoad

Puede usar la utilidad CLFSLoad de Avere para copiar los datos en un nuevo contenedor de almacenamiento de blobs antes de agregarlo como destino de almacenamiento. Esta utilidad se ejecuta en un sistema Linux único y escribe los datos en el formato de propiedad necesario para Azure HPC Cache. CLFSLoad es la manera más eficaz de rellenar un contenedor de almacenamiento de blobs y usarlo con la caché.

La utilidad Avere CLFSLoad está disponible a petición de su equipo de Azure HPC Cache. Pida contacto a su equipo o abra una [incidencia de soporte técnico](hpc-cache-support-ticket.md) para solicitar ayuda.

Esta opción solo funciona con contenedores nuevos vacíos. Cree el contenedor antes de usar Avere CLFSLoad.

Se incluye información detallada en la distribución de Avere CLFSLoad, que está disponible a petición del equipo de Azure HPC Cache.

Información general del proceso:

1. Prepare un sistema Linux (máquina virtual o físico) con la versión 3.6 o posterior de Python. Se recomienda Python 3.7 para mejorar el rendimiento.
1. Instale el software Avere-CLFSLoad en el sistema Linux.
1. Ejecute la transferencia desde la línea de comandos de Linux.

La utilidad Avere CLFSLoad necesita la siguiente información:

* El identificador de la cuenta de almacenamiento que incluye el contenedor de almacenamiento de blobs.
* El nombre del contenedor vacío de almacenamiento de blobs.
* Un token de firma de acceso compartido (SAS) que permite a la utilidad escribir en el contenedor
* Una ruta de acceso local al origen de datos, ya sea un directorio local que contiene los datos que se van a copiar o una ruta de acceso local a un sistema remoto montado con los datos

## <a name="copy-data-through-the-azure-hpc-cache"></a>Copia de datos mediante Azure HPC Cache

Si no quiere usar la utilidad CLFSLoad de Avere o si quiere agregar una gran cantidad de datos a un destino existente de almacenamiento de blobs, puede copiarlos mediante la caché. Azure HPC Cache está diseñado para atender a varios clientes a la vez, así que para copiar datos mediante la caché, debe usar escrituras en paralelo desde varios clientes.

![Diagrama que muestra el movimiento de datos de varios clientes de múltiples subprocesos: en la parte superior izquierda, hay un icono para el almacenamiento de hardware local que tiene varias flechas que salen de él. Las flechas apuntan a cuatro equipos cliente diferentes. De cada máquina cliente tres flechas apuntan a Azure HPC Cache. Desde Azure HPC Cache, hay varias flechas que apuntan a Blob Storage.](media/hpc-cache-parallel-ingest.png)

Los comandos ``cp`` o ``copy`` que se usan habitualmente para transferir datos de un sistema de almacenamiento a otro son comandos de subproceso único que copian solo un archivo a la vez. Esto significa que el servidor de archivos solo puede ingerir un archivo a la vez, lo que es un desperdicio de los recursos de la caché.

En esta sección se explican las estrategias para crear un sistema de copia de archivos de varios subprocesos y varios clientes para mover datos a Blob Storage con Azure HPC Cache. Asimismo, se explican los conceptos de transferencia de archivos y los puntos de decisión que se pueden usar para copiar datos de manera eficiente mediante varios clientes y comandos de copia simples.

Por supuesto, también se explican algunas utilidades que pueden serle de ayuda. La utilidad ``msrsync`` se puede usar para automatizar parcialmente el proceso de dividir un conjunto de datos en cubos y usar los comandos rsync. El script ``parallelcp`` es otra utilidad que lee el directorio de origen y emite comandos de copia automáticamente.

### <a name="strategic-planning"></a>Plan estratégico

Al crear una estrategia para copiar datos en paralelo, debe comprender las ventajas y desventajas que acarrea el tamaño del archivo, el recuento de archivos y la profundidad del directorio.

* Cuando los archivos son pequeños, la métrica de interés se basa en los archivos por segundo.
* Cuando los archivos son grandes (de 10 MiBi o más), la métrica de interés se mide en función de los bytes por segundo.

Cada proceso de copia tiene una tasa de rendimiento y una tasa de transferencia de archivos que puede medirse en función de la longitud del comando de copia y factorizando el tamaño y número de archivos. La explicación referente a cómo medir estas tasas no se encuentra en este documento, pero es imperativo que sepa si usará archivos pequeños o grandes.

Las estrategias para la ingesta de datos en paralelo con Azure HPC Cache son las siguientes:

* Copia manual: puede crear manualmente una copia de varios subprocesos en un cliente mediante la ejecución de más de un comando de copia a la vez en segundo plano con los conjuntos predefinidos de archivos o rutas. Para más información, lea [Ingesta de datos de Azure HPC Cache: método de copia manual](hpc-cache-ingest-manual.md).

* Copia automatizada parcial con ``msrsync``:  - ``msrsync`` es una utilidad de contenedor que ejecuta varios procesos ``rsync`` en paralelo. Para más información, lea [Ingesta de datos de Azure HPC Cache: método msrsync](hpc-cache-ingest-msrsync.md).

* Copia de scripts con ``parallelcp``: aprenda a crear y ejecutar un script de copia en paralelo en [Ingesta de datos de Azure HPC Cache: método de script de copia parcial](hpc-cache-ingest-parallelcp.md).

## <a name="next-steps"></a>Pasos siguientes

Después de configurar el almacenamiento, conozca cómo los clientes pueden montar la caché.

* [Acceso al sistema de Azure HPC Cache](hpc-cache-mount.md)
