---
title: 'Conceptos del servidor: Servidor flexible de Azure Database for MySQL'
description: En este tema se incluyen consideraciones e instrucciones para trabajar con un servidor flexible de Azure Database for MySQL
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: c368e94d8d2b07e3d1487d1b6e505d2a87a89e91
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129994088"
---
# <a name="server-concepts-in-azure-database-for-mysql-flexible-server-preview"></a>Conceptos del servidor: Servidor flexible de Azure Database for MySQL (versión preliminar)

[!INCLUDE[applies-to-mysql-flexible-server](../includes/applies-to-mysql-flexible-server.md)]

> [!IMPORTANT] 
> Actualmente, la opción de implementación Servidor flexible de Azure Database for MySQL se encuentra en versión preliminar pública.

En este artículo se incluyen consideraciones e instrucciones para trabajar con los servidores flexibles de Azure Database for MySQL.

## <a name="what-is-an-azure-database-for-mysql-flexible-server"></a>¿Qué es un servidor flexible de Azure Database for MySQL?

Servidor flexible de Azure Database for MySQL es un servicio de base de datos totalmente administrado que ejecuta la versión de la comunidad de MySQL. En general, el servicio está diseñado para proporcionar flexibilidad y personalizaciones de configuración en función de los requisitos del usuario. Se trata de la misma construcción de servidores MySQL con la que puede estar familiarizado en entornos locales. En concreto, administra un servidor flexible, ofrece un rendimiento inmediato y mayor capacidad de administración y control del servidor, y expone el acceso y las características en el nivel de servidor.

Un servidor flexible de Azure Database for MySQL:

- Se crea dentro de una suscripción de Azure.
- Es el recurso principal de las bases de datos.
- Permite que la configuración de MySQL se exponga a través de parámetros del servidor (vínculo a conceptos de parámetros del servidor).
- Realiza copias de seguridad automatizadas y admite las restauraciones a un momento dado.
- Proporciona un espacio de nombres para las bases de datos.
- Es un contenedor con semántica de duración segura. Si se elimina un servidor, se eliminan las bases de datos que contiene.
- Coloca recursos en una región.
- Compatibilidad con la programación de mantenimiento del servidor que proporciona el usuario
- Capacidad para implementar servidores flexibles en una configuración con redundancia de zona, con el fin de mejorar la alta disponibilidad.
- Proporciona una integración de red virtual para el acceso al servidor de bases de datos
- Permite ahorrar costos, ya que el servidor flexible se puede poner en pausa cuando no está en uso.
- Proporciona el ámbito de las directivas de administración que se aplican a sus bases de datos: inicio de sesión, firewall, usuarios, roles, configuración, etc.
- Admite la versión principal de MySQL 5.7 y MySQL 8.0. Para más información, consulte el artículo sobre las [versiones de las bases de datos de Azure Database for MySQL compatibles](./../concepts-supported-versions.md).

En un servidor flexible de Azure Database for MySQL, se pueden crear una o varias bases de datos. Puede optar por crear una sola base de datos por servidor para que se usen todos los recursos, o bien crear varias bases de datos para compartir los recursos. El precio se estructura por servidor y se basa en la configuración del nivel de proceso, los núcleos virtuales y el almacenamiento (GB). Para más información, consulte el artículo sobre [proceso y almacenamiento](./concepts-compute-storage.md).

## <a name="stopstart-an-azure-database-for-mysql-flexible-server"></a>Inicio o detención inicio de un servidor flexible de Azure Database for MySQL

La opción de implementación Servidor flexible de Azure Database for MySQL permite **detener** el servidor cuando no está en uso e **iniciar** el servidor cuando se reanuda la actividad. Esto se hace básicamente para ahorrar costos en los servidores de bases de datos, ya que el recurso solo se paga cuando se usa. Esto cobra aún más importancia en las cargas de trabajo de desarrollo y pruebas, y cuando el servidor solo se usa durante una parte del día. Al detener el servidor, se anularán todas las conexiones activas. Más adelante, cuando desee volver a poner el servidor en línea, puede usar [Azure Portal](how-to-stop-start-server-portal.md) o la CLI.

Si el servidor está en estado **detenido**, el proceso del mismo no se factura. Sin embargo, el almacenamiento se sigue facturando mientras se mantiene el almacenamiento del servidor, con el fin de asegurarse de que los archivos de datos están disponibles cuando se vuelve a iniciar el servidor.

> [!IMPORTANT]
> Cuando se **detiene** el servidor, permanece en ese estado durante los 30 días siguientes. Si no lo **inicia** de forma manual durante ese tiempo, se inicia automáticamente al final de los 30 días. Puede optar por volver a **detenerlo** si no va a usar el servidor.

Durante el tiempo en que el servidor está detenido, no se pueden realizar operaciones de administración en él. Para cambiar cualquier valor de la configuración del servidor, deberá [iniciar el servidor](how-to-stop-start-server-portal.md). Consulte las [limitaciones de inicio y parada](./concepts-limitations.md#stopstart-operation).

## <a name="how-do-i-manage-a-server"></a>¿Cómo se administra un servidor?

Puede administrar la creación, la eliminación, la configuración de parámetros del servidor (my.cnf), el escalado, la red, la seguridad, la alta disponibilidad, la copia de seguridad y la restauración, y la supervisión del servidor flexible de Azure Database for MySQL desde [Azure Portal](./quickstart-create-server-portal.md) o la [CLI de Azure](./quickstart-create-server-cli.md). Además, los siguientes procedimientos almacenados están disponibles en Azure Database for MySQL para realizar determinadas tareas de administración de bases de datos necesarias, ya que no se admite el privilegio de superusuario en el servidor.

|**Nombre del procedimiento almacenado**|**Parámetros de entrada**|**Parámetros de salida**|**Nota sobre el uso**|
|-----|-----|-----|-----|
|*mysql.az_kill*|processlist_id|N/D|Equivalente al comando [`KILL CONNECTION`](https://dev.mysql.com/doc/refman/8.0/en/kill.html). Finalizará la conexión asociada con el processlist_id proporcionado después de finalizar cualquier instrucción que la conexión esté ejecutando.|
|*mysql.az_kill_query*|processlist_id|N/D|Equivalente al comando [`KILL QUERY`](https://dev.mysql.com/doc/refman/8.0/en/kill.html). Finalizará la instrucción que la conexión está ejecutando actualmente. Deja la propia conexión activa.|
|*mysql.az_load_timezone*|N/D|N/D|Carga las [tablas de zonas horarias](../howto-server-parameters.md#working-with-the-time-zone-parameter) para que en el parámetro `time_zone` se puedan establecer valores con nombre (por ejemplo, "US/Pacific").|


## <a name="next-steps"></a>Pasos siguientes

-   Más información sobre la [creación de un servidor](./quickstart-create-server-portal.md).
-   Más información acerca de la [supervisión y las alertas](./how-to-alert-on-metric.md)

