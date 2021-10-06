---
title: Procedimientos recomendados para usar y supervisar la carga del servidor
titleSuffix: Azure Cache for Redis
description: Aprenda a usar y supervisar la carga del servidor en Azure Cache for Redis.
author: shpathak-msft
ms.service: cache
ms.topic: conceptual
ms.date: 08/25/2021
ms.author: shpathak
ms.openlocfilehash: 51a0a5ede1c9d978fcc7eea98c7519c70bd9126e
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128626140"
---
# <a name="manage-server-load-for-azure-cache-for-redis"></a>Administración de la carga del servidor en Azure Cache for Redis

## <a name="value-sizes"></a>Tamaños de valor

El diseño de la aplicación cliente determina si debe almacenar muchos valores pequeños o un número menor de valores mayores. Desde la perspectiva del servidor Redis, los valores menores ofrecen un mejor rendimiento. Se recomienda mantener un tamaño de valor inferior a 100 kB.

Si el diseño requiere que almacene valores mayores en Azure Cache for Redis, la carga del servidor será mayor. En este caso, es posible que tenga que usar un nivel de caché superior para asegurarse de que el uso de CPU no limita el rendimiento.

Incluso si la caché tiene suficiente capacidad de CPU, los valores más grandes aumentan las latencias, por lo que debe seguir las instrucciones que se indican en [Configuración de tiempos de espera adecuados](cache-best-practices-connection.md#configure-appropriate-timeouts).

Los valores más grandes también aumentan las posibilidades de fragmentación de memoria, por lo que debe seguir las instrucciones que se indican en [Configuración de la opción maxmemory-reserved](cache-best-practices-memory-management.md#configure-your-maxmemory-reserved-setting).

## <a name="avoid-client-connection-spikes"></a>Evitar picos de conexión cliente

La creación y el cierre de las conexiones es una operación costosa para el servidor Redis. Si la aplicación cliente crea o cierra demasiadas conexiones en un período de tiempo pequeño, podría sobrecargar el servidor Redis.

Si va a crear instancias de muchas instancias de cliente para que se conecten a Redis a la vez, considere la posibilidad de escalonar las nuevas creaciones de conexión para evitar un pico excesivo en el número de clientes conectados.

## <a name="memory-pressure"></a>Presión de memoria

El uso elevado de memoria en el servidor hace que sea más probable que el sistema tenga que paginar los datos en el disco, lo que da lugar a errores de página que pueden ralentizar significativamente el sistema.

## <a name="avoid-long-running-commands"></a>Evitar comandos de larga duración

El servidor Redis es un sistema de un solo subproceso. Los comandos de larga duración pueden provocar latencia o tiempos de espera en el lado cliente porque el servidor no puede responder a otras solicitudes mientras está ocupado trabajando en un comando de larga duración. Para más información, consulte [Solución de problemas del lado servidor de Azure Cache for Redis](cache-troubleshoot-server.md).  

## <a name="monitor-server-load"></a>Supervisión de la carga del servidor

Agregue supervisión en la carga del servidor para asegurarse de que recibe notificaciones cuando se produzca una carga elevada del servidor. La supervisión puede ayudarle a comprender las restricciones de la aplicación. Luego, puede trabajar de forma proactiva para mitigar los problemas. Se recomienda intentar mantener la carga del servidor por debajo del 80 % para evitar efectos negativos en el rendimiento.

## <a name="plan-for-server-maintenance"></a>Planeamiento del mantenimiento del servidor

Asegúrese de que tiene capacidad suficiente en el servidor para controlar la carga máxima mientras los servidores de caché se someten a mantenimiento. Para probar el sistema, reinicie los nodos mientras está bajo carga máxima. Para más información sobre cómo simular la implementación de una revisión, consulte [Reinicio](cache-administration.md#reboot).

## <a name="test-for-increased-server-load-after-failover"></a>Prueba del aumento de la carga del servidor después de la conmutación por error

En el caso de las SKU estándar y prémium, cada caché está hospedada en dos nodos. Un equilibrador de carga distribuye las conexiones de cliente a los dos nodos. Cuando se produce un mantenimiento planeado o no planeado en el nodo principal, este finaliza todas las conexiones de cliente. En este caso, todas las conexiones de cliente podrían distribuirse a un solo nodo, lo que provocaría que la carga del servidor aumentara en el nodo restante. Para probar este escenario, se recomienda reiniciar el nodo principal y asegurarse de que un nodo puede controlar todas las conexiones de cliente sin que la carga del servidor sea demasiado alta.

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas del lado servidor de Redis Cache](cache-troubleshoot-server.md)
- [Resistencia de la conexión](cache-best-practices-connection.md)
  - [Configuración de los tiempos de espera adecuados](cache-best-practices-connection.md#configure-appropriate-timeouts)
- [Administración de la memoria](cache-best-practices-memory-management.md)
  - [Configuración de la opción maxmemory-reserved](cache-best-practices-memory-management.md#configure-your-maxmemory-reserved-setting)
