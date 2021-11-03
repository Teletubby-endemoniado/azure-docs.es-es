---
title: Modelos de uso de Azure HPC Cache
description: Se describen los diferentes modelos de uso de la memoria caché y cómo elegir entre ellos para establecer el almacenamiento en caché de solo lectura o de lectura/escritura, así como controlar otras opciones de almacenamiento en caché
author: femila
ms.service: hpc-cache
ms.topic: how-to
ms.date: 07/12/2021
ms.author: femila
ms.openlocfilehash: 84964e8a5f188d03fb9e5bcb98d4466610732c75
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131015245"
---
<!-- filename is referenced from GUI in aka.ms/hpc-cache-usagemodel -->

# <a name="understand-cache-usage-models"></a>Descripción de los modelos de uso de caché

Los modelos de uso de caché permiten personalizar cómo Azure HPC Cache almacena los archivos para agilizar el flujo de trabajo.

## <a name="basic-file-caching-concepts"></a>Conceptos básicos del almacenamiento en caché de archivos

El almacenamiento en caché de archivos es la forma en que Azure HPC Cache agiliza las solicitudes de cliente. Usa estas prácticas básicas:

* **Almacenamiento en caché de lectura**: Azure HPC Cache conserva una copia de los archivos que los clientes solicitan del sistema de almacenamiento. La próxima vez que un cliente solicite el mismo archivo, HPC Cache puede proporcionar la versión de su memoria caché, en lugar de tener que recuperar el archivo del sistema de almacenamiento de back-end.

* **Almacenamiento en caché de escritura**: opcionalmente, Azure HPC Cache puede almacenar una copia de los archivos modificados que se enviaron desde los equipos cliente. Si varios clientes realizan cambios en el mismo archivo durante un breve período de tiempo, la memoria caché puede recopilar todos los cambios en la memoria caché, en lugar de que deba escribir cada cambio individualmente en el sistema de almacenamiento de back-end.

  Después de un período de tiempo especificado sin cambios, la memoria caché mueve el archivo al sistema de almacenamiento a largo plazo.

  Si el almacenamiento en caché de escritura está deshabilitado, la memoria caché no almacena el archivo modificado y lo escribe inmediatamente en el sistema de almacenamiento de back-end.

* **Retraso de reescritura**: en el caso de una memoria caché que tenga habilitado el almacenamiento en caché de escritura, el retraso de reescritura es la cantidad de tiempo que la memoria caché espera para que se produzcan cambios de archivo adicionales antes de copiar el archivo en el sistema de almacenamiento de back-end.

* **Comprobación de back-end**: la configuración de la comprobación de back-end determina la frecuencia con la que la memoria caché compara su copia local de un archivo con la versión remota en el sistema de almacenamiento de back-end. Si la copia de back-end es más reciente que la copia almacenada en caché, la memoria caché recupera la copia remota y la almacena para futuras solicitudes.

  La configuración de comprobación de back-end muestra cuándo la caché compara *automáticamente* sus archivos con los archivos de origen que están en el almacenamiento remoto. Sin embargo, puede forzar que Azure HPC Cache compare los archivos mediante una operación de directorio que incluye una solicitud readdirplus. Readdirplus es una API NFS estándar (también llamada lectura extendida) que devuelve metadatos de directorio, lo que hace que la caché compare y actualice los archivos.

Los modelos de uso integrados en Azure HPC Cache tienen diferentes valores para esta configuración, de modo que puede elegir la mejor combinación para su situación.

## <a name="choose-the-right-usage-model-for-your-workflow"></a>Elección del modelo de uso correcto para el flujo de trabajo

Debe elegir un modelo de uso para cada destino de almacenamiento con protocolo NFS que use. Los destinos de Azure Blob Storage tienen un modelo de uso integrado que no se puede personalizar.

Los modelos de uso de HPC Cache permiten elegir cómo equilibrar una respuesta rápida con el riesgo de obtener datos obsoletos. Si quiere optimizar la velocidad de lectura de los archivos, es posible que no le interese si los archivos de la memoria caché se comparan con los archivos de back-end. Por otro lado, si desea asegurarse de que los archivos estén siempre actualizados con el almacenamiento remoto, elija un modelo que realice comparaciones frecuentes.

Estas son las opciones del modelo de uso:

* **Lectura de textos densos y poco frecuentes**: utilice esta opción si desea acelerar el acceso de lectura a archivos que son estáticos o que no se suelen modificar.

  Esta opción almacena en caché las lecturas del cliente, pero no almacena en caché las escrituras. En su lugar, pasa las escrituras al almacenamiento de back-end inmediatamente.
  
  Los archivos almacenados en la memoria caché no se comparan automáticamente con los archivos del volumen de almacenamiento NFS. (Lea la descripción de la comprobación de back-end anterior para obtener información sobre cómo compararlos manualmente).

  No use esta opción si existe el riesgo de que un archivo se pueda modificar directamente en el sistema de almacenamiento sin escribirlo primero en la memoria caché. Si esto sucede, la versión en caché del archivo no estará sincronizada con el archivo de back-end.

* **Mayor que el 15 % de textos**: esta opción acelera el rendimiento de lectura y escritura. Al usar esta opción, todos los clientes deben acceder a los archivos mediante Azure HPC Cache en lugar de montar el almacenamiento de back-end directamente. Los archivos almacenados en caché contendrán cambios recientes que todavía no se han copiado en el back-end.

  En este modelo de uso, los archivos de la caché solo se comparan con los archivos del almacenamiento de back-end cada ocho horas. Se supone que la versión en caché del archivo es más actual. Un archivo modificado en la memoria caché se escribe en el sistema de almacenamiento de back-end después de que esté en la memoria caché durante una hora sin cambios adicionales.

* **Los clientes escriben en el destino NFS omitiendo la caché**: elija esta opción si algún cliente del flujo de trabajo escribe datos directamente en el sistema de almacenamiento sin escribir primero en la caché o si quiere optimizar la coherencia de los datos. Los archivos que solicitan los clientes se almacenan en caché (lecturas), pero los cambios que se realicen en esos archivos desde el cliente (escrituras) no se almacenan en caché. Se pasan directamente al sistema de almacenamiento de back-end.

  Con este modelo de uso, los archivos de la memoria caché se comparan frecuentemente con las versiones de back-end para determinar si hay actualizaciones cada 30 segundos. Esta comprobación permite cambiar los archivos fuera de la memoria caché al tiempo que se preserva la coherencia de los datos.

  > [!TIP]
  > Los tres primeros modelos de uso básico se pueden usar para administrar la mayoría de los flujos de trabajo de Azure HPC Cache. Las siguientes opciones son para los escenarios menos comunes.

* **Mayor que el 15 % de escrituras; comprobando si hay cambios en el servidor de copia de seguridad cada 30 segundos** y **Mayor que el 15 % de escrituras; comprobando si hay cambios en el servidor de copia de seguridad cada 60 segundos**: estas opciones están diseñadas para los flujos de trabajo en los que se quieren agilizar las lecturas y escrituras, pero existe la posibilidad de que otro usuario escriba directamente en el sistema de almacenamiento de back-end. Por ejemplo, si hay varios conjuntos de clientes trabajando en los mismos archivos desde ubicaciones diferentes, estos modelos de uso podrían servir para equilibrar la necesidad de acceso rápido a los archivos con una tolerancia baja para el contenido obsoleto del origen.

* **Mayor que el 15 % de escrituras; reescritura en el servidor cada 30 segundos**: esta opción está diseñada para el escenario en el que varios clientes están modificando activamente los mismos archivos, o si algunos clientes acceden directamente al almacenamiento de back-end en lugar de montar la memoria caché.

  Las operaciones de escritura frecuentes en el back-end afectan al rendimiento de la memoria caché, por lo que debe considerar la posibilidad de usar el modelo de uso **Mayor que el 15 % de textos** si hay un riesgo bajo de conflicto en los archivos; por ejemplo, si sabe que los distintos clientes están trabajando en áreas diferentes del mismo conjunto de archivos.

* **Lectura intensiva; comprobando el servidor de copia de seguridad cada 3 horas**: esta opción prioriza las lecturas rápidas en el lado del cliente, aunque también actualiza los archivos almacenados en caché desde el sistema de almacenamiento de back-end con regularidad, a diferencia del modelo de uso **Read heavy, infrequent writes** (lectura intensiva, operaciones de escritura poco frecuentes).

En esta tabla se resumen las diferencias de los modelos de uso:

[!INCLUDE [usage-models-table.md](includes/usage-models-table.md)]

Si tiene alguna pregunta sobre el mejor modelo de uso para su flujo de trabajo de Azure HPC Cache, hable con su representante de Azure o abra una solicitud de soporte técnico para obtener ayuda.

## <a name="change-usage-models"></a>Cambio de los modelos de uso

Puede cambiar los modelos de uso mediante la edición del destino de almacenamiento, pero no se permiten algunos cambios porque crean un pequeño riesgo de conflicto de versión de archivo.

No se puede cambiar **a** o **desde** el modelo denominado **Lectura intensiva, operaciones de escritura poco frecuentes**. Para cambiar un destino de almacenamiento a este modelo de uso o para cambiar de este modelo a otro modelo de uso, debe eliminar el destino de almacenamiento original y crear uno nuevo.

Esta restricción también se aplica al modelo de uso **De lectura pesada, comprobando el servidor de respaldo cada tres horas**, que se usa con menos frecuencia. Además, puede cambiar entre los dos modelos de uso de "lecturas pesadas...", pero no dentro o fuera de un estilo de modelo de uso diferente.

Esta restricción es necesaria debido a la manera en que los diferentes modelos de uso controlan las solicitudes de Network Lock Manager (NLM). Azure HPC Cache se ubica entre los clientes y el sistema de almacenamiento de back-end. Normalmente, la memoria caché pasa las solicitudes de NLM al sistema de almacenamiento de back-end, pero en algunas situaciones, la propia caché confirma la solicitud de NLM y devuelve un valor al cliente. En Azure HPC Cache, esto solo sucede cuando se usan los modelos de uso **Lectura intensiva, operaciones de escritura poco frecuentes** o **Lectura intensiva, comprobando el servidor de copia de seguridad cada 3 horas** o con un destino de almacenamiento de blobs estándar, que no tiene modelos de uso configurables.

Si cambia entre **Lectura intensiva, operaciones de escritura poco frecuentes** y un modelo de uso diferente, no hay ninguna manera de transferir el estado NLM actual de la memoria caché al sistema de almacenamiento o viceversa. Por lo tanto, el estado de bloqueo del cliente es inexacto.

> [!NOTE]
> ADLS-NFS no admite NLM. Debe deshabilitar NLM cuando los clientes monten el clúster para acceder a un destino de almacenamiento ADLS-NFS.
>
> Use la opción ``-o nolock`` del comando ``mount``. Compruebe la documentación de montaje del sistema operativo cliente (man 5 nfs) para obtener información sobre el comportamiento exacto de la opción ``nolock`` para los clientes.

## <a name="next-steps"></a>Pasos siguientes

* [Agregar destinos de almacenamiento](hpc-cache-add-storage.md) a Azure HPC Cache
