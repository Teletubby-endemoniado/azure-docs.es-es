---
title: Uso de la solución Service Map en Azure | Microsoft Docs
description: Service Map es una solución de Azure que detecta automáticamente los componentes de la aplicación en sistemas Windows y Linux y asigna la comunicación entre servicios. En este artículo se proporciona información para implementar la solución Mapa de servicio en su entorno y utilizarla en distintos escenarios.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/24/2019
ms.openlocfilehash: f759308a32d3f1455c7147be512e7280810e1abe
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131427045"
---
# <a name="using-service-map-solution-in-azure"></a>Uso de la solución Service Map en Azure

Mapa de servicio detecta automáticamente los componentes de la aplicación en sistemas Windows y Linux y asigna la comunicación entre servicios. Con Service Map puede ver los servidores en la forma en que piensa en ellos: como sistemas interconectados que ofrecen servicios críticos. Service Map muestra las conexiones entre servidores, procesos, la latencia de conexión entrante y saliente y puertos en cualquier arquitectura conectada de TCP sin necesidad de ninguna configuración más allá de la instalación de un agente.

En este artículo se describen los detalles sobre la incorporación y utilización de Service Map. Los requisitos previos de la solución son los siguientes:

* Un área de trabajo de Log Analytics en una [región admitida](vminsights-configure-workspace.md#supported-regions).

* El [agente de Log Analytics](vminsights-enable-overview.md#agents) instalado en el equipo Windows o en el servidor Linux conectado a la misma área de trabajo con la que ha habilitado la solución.

* [Dependency Agent](vminsights-enable-overview.md#agents) instalado en el equipo con Windows o en el servidor Linux.

>[!NOTE]
>Si ya ha implementado Service Map, ahora también puede ver las asignaciones en VM Insights, lo que incluye características adicionales para supervisar el rendimiento y el estado de la VM. Para más información, consulte [Información general sobre VM Insights](../vm/vminsights-overview.md). Para obtener información sobre las diferencias entre la solución Service Map y la característica de asignación de VM Insights, consulte las siguientes [Preguntas más frecuentes](../faq.yml).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="enable-service-map"></a>Habilitar Service Map

1. Habilite la solución Service Map desde [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ServiceMapOMS?tab=Overview) o mediante el proceso descrito en el artículo sobre la [adición de soluciones de supervisión desde la Galería de soluciones](../insights/solutions.md).
1. [Instale Dependency Agent en Windows ](../vm/vminsights-enable-hybrid.md#install-the-dependency-agent-on-windows)o [en Linux](../vm/vminsights-enable-hybrid.md#install-the-dependency-agent-on-linux) en cada equipo en el que quiera obtener datos. Dependency Agent puede supervisar las conexiones a los vecinos inmediatos, por lo que es posible que no necesite un agente en cada equipo.

Se puede obtener acceso a Service Map en Azure Portal desde el área de trabajo de Log Analytics, y seleccionar la opción **Soluciones** en el panel de la izquierda.<br><br> ![Seleccione la opción Soluciones en el área de trabajo](./media/service-map/select-solution-from-workspace.png).<br> En la lista de soluciones, seleccione **ServiceMap(workspaceName)** y en la página de información general de la solución Service Map, haga clic en el mosaico de resumen Service Map.<br><br> ![Mosaico de resumen Service Map](./media/service-map/service-map-summary-tile.png).

## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Casos de uso: Haga que sus procesos de TI tengan en cuenta la dependencia

### <a name="discovery"></a>Detección

Mapa de servicio crea automáticamente una asignación de referencias comunes de dependencias entre servidores, procesos y servicios de terceros. Detecta y asigna todas las dependencias TCP al identificar las conexiones sorpresa, los sistemas remotos de terceros de los que depende y las dependencias con áreas oscuras tradicionales de la red, como Active Directory. Service Map detecta las conexiones de red con errores que los sistemas administrados están intentando realizar, lo que ayuda a identificar posibles errores de configuración del servidor, interrupciones de servicio y problemas de red.

### <a name="incident-management"></a>Administración de incidentes

Mapa de servicio ayuda a eliminar las suposiciones de aislamiento de problemas mostrando cómo se conectan los sistemas y afectan al resto. Además de identificar las conexiones con errores, ayuda a identificar equilibradores de carga configurados incorrectamente, cargas sorprendentes o excesivas en servicios críticos y clientes no autorizados, como equipos de desarrollador que se comunican con sistemas de producción. Al usar flujos de trabajo integrados con Change Tracking, también puede ver si un evento de cambio en un servicio o máquina back-end explica la causa raíz de un incidente.

### <a name="migration-assurance"></a>Garantía de migración

El empleo de Service Map permite planear, acelerar y validar de forma eficaz las migraciones de Azure, lo que ayuda a garantizar que nada se quede atrás y que no se produzcan interrupciones por sorpresa. Puede detectar todos los sistemas interdependientes que tienen que migrarse juntos, evaluar la capacidad y la configuración del sistema e identificar si un sistema en ejecución sigue ofreciendo servicio a los usuarios o si es un candidato para la retirada en lugar de la migración. Después de realizar la migración, puede ver la carga y la identidad del cliente para comprobar que los sistemas de prueba y los clientes se están conectando. Si las definiciones de firewall y planeación de la subred tienen problemas, los errores de conexión en las asignaciones de Service Map harán referencia a los sistemas que necesitan conectividad.

### <a name="business-continuity"></a>Continuidad empresarial

Si utiliza Azure Site Recovery y necesita ayuda para definir la secuencia de recuperación para su entorno de aplicaciones, Mapa de servicio puede automáticamente mostrarle cómo los sistemas dependen entre sí para asegurarse de que su plan de recuperación es confiable. Al elegir un servidor o grupo crítico y ver sus clientes, puede identificar los sistemas front-end que deben recuperarse después de que el servidor esté restaurado y disponible. A la inversa, al examinar las dependencias de back-end de los servidores críticos, puede identificar aquellos sistemas que deben recuperarse antes de restaurar los sistemas de foco.

### <a name="patch-management"></a>Administración de revisiones

Service Map mejora el uso de la evaluación de actualizaciones del sistema mostrando qué otros equipos y servidores dependen del servicio, por lo que puede notificarles por adelantado antes de que afecte a los sistemas para la revisión. Service Map también mejora la administración de revisiones mostrando si los servicios están disponibles y conectados correctamente después de que se revisen y se reinicien.

## <a name="mapping-overview"></a>Información general de asignación

Los agentes de Service Map recopilan información acerca de todos los procesos de conexión TCP en el servidor donde están instalados, así como los detalles de las conexiones entrantes y salientes para cada proceso.

En la lista del panel izquierdo, puede seleccionar equipos o grupos con agentes de Service Map para visualizar sus dependencias durante un intervalo de tiempo especificado. Las asignaciones de dependencias de equipos se centran en un equipo concreto y muestran todos los equipos que son clientes directos de TCP o servidores de ese equipo.  Las asignaciones de grupos de equipos muestran conjuntos de servidores y sus dependencias.

![Introducción a Mapa de servicio](media/service-map/service-map-overview.png)

Las máquinas se pueden expandir en la asignación para mostrar los grupos de procesos en ejecución y los procesos con conexiones de red activas durante el intervalo de tiempo seleccionado. Cuando un equipo remoto con un agente de Service Map se expande para mostrar los detalles del proceso, se muestran solo aquellos procesos que se comunican con el equipo de foco. Se indica el recuento de equipos front-end sin agente que se conectan al equipo de foco en el lado izquierdo de los procesos a los que se conectan. Si el equipo de foco establece una conexión con un equipo back-end sin agente, el servidor back-end se incluye en un grupo de puertos de servidor, junto con otras conexiones al mismo número de puerto.

De forma predeterminada, las asignaciones de Service Map muestran los 30 últimos minutos de la información de dependencia. Mediante los controles de tiempo de la parte superior izquierda, se pueden consultar las asignaciones de intervalos de tiempo históricos de hasta una hora para mostrar el aspecto de las dependencias en el pasado (por ejemplo, durante un incidente o antes de un cambio). Los datos de Mapa de servicio se almacenan durante 30 días en áreas de trabajo pagadas y durante 7 días en áreas de trabajo disponibles.

## <a name="status-badges-and-border-coloring"></a>Notificaciones de estado y colores en el borde

En la parte inferior de cada servidor en el mapa puede haber una lista de notificaciones de estado que expresan información de estado acerca del servidor. Las notificaciones indican que hay cierta información pertinente para el servidor de una de las integraciones de solución. Al hacer clic en una notificación se pasa directamente a los detalles del estado en el panel derecho. Las notificaciones de estado actualmente disponibles incluyen alertas, departamento de servicios, cambios, seguridad y actualizaciones.

Según la gravedad de las notificaciones de estado, los bordes del nodo de la máquina pueden ser rojos (crítico), amarillos (advertencia) o azules (informativo). El color representa el estado más grave de cualquiera de las notificaciones de estado. Un borde gris indica que un nodo no tiene indicadores de estado.

![Notificaciones de estado](media/service-map/status-badges.png)

## <a name="process-groups"></a>Grupos de procesos

Un grupo de procesos combina procesos que están asociados a un producto o servicio común de ese grupo.  Al expandirse un nodo de máquina, se muestran los procesos independientes junto con los grupos de procesos.  Si se ha producido un error de conexión entrante y saliente a un proceso en un grupo de procesos, la conexión se muestra como errónea para todo el grupo.

## <a name="machine-groups"></a>Grupos de equipos

Los grupos de equipos permiten ver asignaciones centradas en torno a un conjunto de servidores, no solo uno, para que pueda ver todos los miembros de una aplicación de varios niveles o un clúster de servidores de una asignación.

Los usuarios seleccionan qué servidores pertenecen a un grupo y eligen un nombre para el grupo.  Luego pueden ver el grupo con todos sus procesos y conexiones o verlo únicamente con los procesos y las conexiones que se relacionan directamente con los demás miembros del grupo.

![Grupo de equipos](media/service-map/machine-group.png)

### <a name="creating-a-machine-group"></a>Creación de un grupo de equipos

Para crear un grupo, seleccione el equipo o equipos que quiera en la lista Equipos y haga clic en **Agregar al grupo**.

![Crear grupo](media/service-map/machine-groups-create.png)

Ahí puede elegir **Crear nuevo** y proporcionarle un nombre al grupo.

![Dar nombre al grupo](media/service-map/machine-groups-name.png)

>[!NOTE]
>Los grupos de máquinas están limitados a 10 servidores.

### <a name="viewing-a-group"></a>Visualización de un grupo

Una vez que se hayan creado algunos grupos, puede verlos si elige la pestaña Grupos.

![Pestaña Grupos](media/service-map/machine-groups-tab.png)

Luego seleccione el nombre del grupo para ver la asignación de ese grupo de equipos.
![Grupo de equipos](media/service-map/machine-group.png) Los equipos que pertenecen al grupo tienen un contorno blanco en la asignación.

Al expandir el grupo se muestran los equipos que componen el grupo de equipos.

![Equipos del grupo de equipos](media/service-map/machine-groups-machines.png)

### <a name="filter-by-processes"></a>Filtrar por procesos

Puede alternar la vista de la asignación entre mostrar todos los procesos y las conexiones del grupo y solo aquellos que se relacionan directamente con el grupo de equipos.  La vista predeterminada es mostrar todos los procesos.  Puede cambiar la vista si hace clic en el icono de filtro situado encima de la asignación.

![Filtrar grupo](media/service-map/machine-groups-filter.png)

Cuando **Todos los procesos** está seleccionado, la asignación incluye todos los procesos y las conexiones de cada uno de los equipos del grupo.

![Todos los procesos del grupo de equipos](media/service-map/machine-groups-all.png)

Si cambia la vista para mostrar solo **procesos conectados al grupo**, la asignación se restringe solo a los procesos y las conexiones que están conectados directamente a otros equipos del grupo, con lo que se crea una vista simplificada.

![Procesos filtrados del grupo de equipos](media/service-map/machine-groups-filtered.png)
 
### <a name="adding-machines-to-a-group"></a>Adición de equipos a un grupo

Para agregar equipos a un grupo existente, active las casillas situadas junto a los equipos que quiere y luego haga clic en **Agregar al grupo**.  Luego elija el grupo al que quiere agregar los equipos.
 
### <a name="removing-machines-from-a-group"></a>Eliminación de equipos de un grupo

En la lista de grupos, expanda el nombre del grupo para ver los equipos del grupo de equipos.  Luego haga clic en el menú de puntos suspensivos situado junto al equipo que quiere quitar y elija **Quitar**.

![Quitar equipo del grupo](media/service-map/machine-groups-remove.png)

### <a name="removing-or-renaming-a-group"></a>Eliminación o cambio del nombre de un grupo

Haga clic en el menú de puntos suspensivos situado junto al nombre del grupo en la lista de grupos.

![Menú Grupo de equipos](media/service-map/machine-groups-menu.png)


## <a name="role-icons"></a>Iconos de rol

Ciertos procesos cumplen roles determinados en los equipos: servidores web, servidores de aplicaciones, base de datos, etc. Service Map marca las casillas de proceso y máquina con iconos de rol, lo que facilita la identificación rápida del rol que desempeña un proceso o un servidor.

| Icono de rol | Descripción |
|:--|:--|
| ![Servidor Web](media/service-map/role-web-server.png) | Servidor Web |
| ![Servidor de aplicaciones](media/service-map/role-application-server.png) | Servidor de aplicaciones |
| ![Servidor de bases de datos](media/service-map/role-database.png) | Servidor de bases de datos |
| ![Servidor LDAP](media/service-map/role-ldap.png) | Servidor LDAP |
| ![Servidor SMB](media/service-map/role-smb.png) | Servidor SMB |

![Iconos de rol](media/service-map/role-icons.png)


## <a name="failed-connections"></a>Conexiones con errores

En Service Map se muestran las conexiones con errores de procesos y equipos con una línea roja discontinua que indica que un sistema cliente no alcanza un proceso o puerto. Se informa de las conexiones erróneas de cualquier sistema con un agente de Mapa de servicio implementado si ese sistema es el que intenta establecer la conexión con errores. Service Map mide este proceso mediante la observación de los sockets TCP que no pueden establecer una conexión. Este error puede deberse a un firewall, una configuración incorrecta en el cliente o servidor o un servicio remoto que no está disponible.

![Captura de pantalla de una parte de un mapa de servicio resaltando una línea roja discontinua que indica una conexión con errores entre el proceso backup.pl y el puerto 4475.](media/service-map/failed-connections.png)

La comprensión de las conexiones con errores pueden ayudar a solucionar problemas, a la validación de la migración, el análisis de seguridad y al entendimiento general de la arquitectura. A veces las conexiones con errores son inofensivas, pero a menudo señalan directamente un problema, como un entorno de conmutación por error que de repente se convierte en inalcanzable o dos niveles de aplicación que no pueden comunicarse después de una migración a la nube.

## <a name="client-groups"></a>Grupos de clientes

Los grupos de clientes son cuadros en el mapa que representan los equipos cliente que no tienen agentes de dependencia. Un único grupo de clientes representa a los clientes de un proceso o equipo individual.

![Grupos de clientes](media/service-map/client-groups.png)

Para ver las direcciones IP de los servidores de un grupo de clientes, seleccione el grupo. El contenido del grupo se muestra en el panel de **propiedades del grupo de clientes**.

![Propiedades del grupo de clientes](media/service-map/client-group-properties.png)

## <a name="server-port-groups"></a>Grupos de puertos de servidor

Los grupos de puertos de servidor son cuadros que representan los puertos del servidor en servidores que no tienen agentes de dependencia. El cuadro contiene el puerto del servidor y un recuento del número de servidores con conexiones a ese puerto. Expanda el cuadro para ver los servidores y las conexiones individuales. Si hay un solo servidor en el cuadro, se muestra el nombre o la dirección IP.

![Grupos de puertos de servidor](media/service-map/server-port-groups.png)

## <a name="context-menu"></a>Menú contextual

Al hacer clic en los puntos suspensivos (...) de la parte superior derecha de cualquier servidor aparece el menú contextual de ese servidor.

![Captura de pantalla que muestra el menú contextual abierto para un servidor en Service Map. El menú tiene las opciones Cargar mapa del servidor y Mostrar autovínculos.](media/service-map/context-menu.png)

### <a name="load-server-map"></a>Cargar mapa del servidor

Al hacer clic en **Cargar mapa del servidor** se le remite a una nueva asignación con el servidor seleccionado como nuevo equipo de foco.

### <a name="show-self-links"></a>Mostrar autovínculos

La opción **Mostrar autovínculos** vuelve a dibujar el nodo de servidor, incluidos los autovínculos, que son conexiones TCP que empiezan y acaban en procesos del servidor. Si se muestran los autovínculos, el comando de menú cambia a **Ocultar autovínculos** para que pueda desactivarlos.

## <a name="computer-summary"></a>Resumen del equipo

El panel **Machine Summary** (Resumen de la máquina) incluye información general del sistema operativo de un servidor, recuentos de dependencias y datos de otras soluciones. Estos datos incluyen métricas de rendimiento, incidencias del departamento de servicios, seguimiento de cambios, seguridad y actualizaciones.

![Panel de resumen del equipo](media/service-map/machine-summary.png)

## <a name="computer-and-process-properties"></a>Propiedades de proceso y de equipo

Al navegar por una asignación de Service Map, puede seleccionar equipos y procesos para obtener contexto adicional sobre sus propiedades. Las máquinas proporcionan información acerca del nombre DNS, las direcciones IPv4, la capacidad de CPU y memoria, el tipo de máquina virtual, el sistema operativo y la versión, la hora del último reinicio, y los identificadores de sus agentes de OMS y Service Map.

![Panel Propiedades de la máquina](media/service-map/machine-properties.png)

Puede recopilar detalles del proceso de los metadatos del sistema operativo sobre procesos en ejecución, como el nombre del proceso, la descripción del proceso, el nombre de usuario y el dominio (en Windows), el nombre de la compañía, el nombre del producto, la versión del producto, el directorio de trabajo, la línea de comandos y la hora de inicio del proceso.

![Panel Propiedades del proceso](media/service-map/process-properties.png)

El panel **Resumen de proceso** proporciona información adicional sobre la conectividad del proceso, incluidos sus puertos enlazados, las conexiones entrantes y salientes, y las conexiones con errores.

![Panel Resumen de proceso](media/service-map/process-summary.png)

## <a name="alerts-integration"></a>Integración de alertas

Service Map se integra con Alertas de Azure para mostrar las alertas activadas del servidor seleccionado en el intervalo de tiempo seleccionado. El servidor muestra un icono si hay alertas actuales y el panel de **alertas del equipo** enumera las alertas.

![Panel de alertas del equipo](media/service-map/machine-alerts.png)

Para que Service Map muestre las alertas relevantes, cree una regla de alerta que se active para un equipo determinado. Para crear alertas apropiadas, realice estos pasos:
- Incluya una cláusula para agrupar por equipo (por ejemplo, **by Computer interval 1 minute**).
- Elija que se envíen alertas según las unidades métricas.

## <a name="log-events-integration"></a>Integración de eventos de registro

Mapa de servicio se integra con Búsqueda de registros para mostrar un recuento de todos los eventos de registro disponibles para el servidor seleccionado durante el intervalo de tiempo elegido. Puede hacer clic en cualquier fila de la lista de recuentos de eventos para acceder a Búsqueda de registros y ver los eventos de registro individuales.

![Panel de eventos de registro del equipo](media/service-map/log-events.png)

## <a name="service-desk-integration"></a>Integración de Service Desk

La integración de Service Map con Conector de Administración de servicios de TI es automática cuando ambas soluciones están habilitadas y configuradas en el área de trabajo de Log Analytics. La integración en Service Map se denomina "Departamento de servicios". Para más información, vea [Administración centralizada de los elementos de trabajo ITSM con IT Service Management Connector (versión preliminar)](../alerts/itsmc-overview.md).

El panel de **departamento de servicios del equipo** muestra todos los eventos de IT Service Management del servidor seleccionado en el intervalo de tiempo seleccionado. El servidor muestra un icono si hay elementos actuales y el panel de departamento de servicios del equipo los enumera.

![Panel de departamento de servicios del equipo](media/service-map/service-desk.png)

Para abrir el elemento en la solución ITSM conectada, haga clic en **Ver elemento de trabajo**.

Para ver los detalles del elemento en la búsqueda de registros, haga clic en **Mostrar en la búsqueda de registro**.
Las métricas de conexión se escriben en dos nuevas tablas en Log Analytics 

## <a name="change-tracking-integration"></a>Integración de Change Tracking

La integración de Service Map con Change Tracking es automática cuando ambas soluciones están habilitadas y configuradas en el área de trabajo de Log Analytics.

El panel de **seguimiento de cambios del equipo** muestra todos los cambios, con el más reciente en primer lugar, además de un vínculo para profundizar en la búsqueda de registros a fin de obtener más detalles.

![Captura de pantalla del panel de seguimiento de cambios de la máquina en Service Map.](media/service-map/change-tracking.png)

La imagen siguiente es una vista detallada de un evento ConfigurationChange que se puede ver después de seleccionar **Mostrar en Log Analytics**.

![Evento ConfigurationChange](media/service-map/configuration-change-event-01.png)

## <a name="performance-integration"></a>Integración del rendimiento

En el panel **Rendimiento de la máquina** se muestran las métricas de rendimiento estándar del servidor seleccionado. Las métricas incluyen uso de la CPU, uso de la memoria, bytes de red enviados y recibidos y una lista de los principales procesos por bytes de red enviados y recibidos.

![Panel Rendimiento de la máquina](media/service-map/machine-performance.png)

Para ver los datos de rendimiento, puede que necesite [habilitar los contadores de rendimiento adecuados de Log Analytics](../agents/data-sources-performance-counters.md).  Los contadores que probablemente le interese habilitar son los siguientes:

Windows:
- Procesador(*)\\% de tiempo de procesador
- Memoria\\% de bytes confirmados en uso
- Adaptador de red(*)\\Bytes enviados por segundo
- Adaptador de red(*)\\Bytes recibidos por segundo

Linux:
- Procesador(*)\\% de tiempo de procesador
- Memoria(*)\\% de memoria usada
- Adaptador de red(*)\\Bytes enviados por segundo
- Adaptador de red(*)\\Bytes recibidos por segundo

Para obtener los datos de rendimiento de red, también debe haber habilitado la solución Wire Data 2.0 en el área de trabajo.
 
## <a name="security-integration"></a>Integración de seguridad

La integración de Service Map con Security and Audit es automática cuando ambas soluciones están habilitadas y configuradas en el área de trabajo de Log Analytics.

En el panel **Machine Security** (Seguridad de la máquina) se muestran datos de la solución Security and Audit del servidor seleccionado. El panel muestra un resumen de los problemas de seguridad pendientes del servidor durante el intervalo de tiempo seleccionado. Al hacer clic en cualquiera de los problemas de seguridad, se profundiza en una búsqueda de registros para obtener detalles sobre ellos.

![Panel de seguridad del equipo](media/service-map/machine-security.png)

## <a name="updates-integration"></a>Integración de actualizaciones

La integración de Service Map con Update Management es automática cuando ambas soluciones están habilitadas y configuradas en el área de trabajo de Log Analytics.

En el panel **Machine Updates** (Actualizaciones de la máquina) se muestran datos de la solución Update Management del servidor seleccionado. El panel muestra un resumen de las actualizaciones que faltan en el servidor durante el intervalo de tiempo seleccionado.

![Captura de pantalla del panel de actualizaciones de la máquina en Service Map.](media/service-map/machine-updates.png)

## <a name="log-analytics-records"></a>Registros de Log Analytics

Los datos de inventario de equipos y procesos de Service Map están disponibles para [buscar](../logs/log-query-overview.md) en Log Analytics. Estos datos se pueden aplicar a escenarios que incluyen la planeación de la migración, el análisis de la capacidad, la detección y la solución de problemas de rendimiento a petición.

Se genera un registro por hora para cada equipo y proceso únicos, además de los registros generados cuando un proceso o equipo se inicia o se integra en Service Map. Estos registros tienen las propiedades de las tablas siguientes. Los campos y valores de los eventos ServiceMapComputer_CL se asignan a los campos del recurso Equipo en la API ServiceMap de Azure Resource Manager. Los campos y valores de los eventos ServiceMapProcess_CL se asignan a los campos del recurso Proceso en la API ServiceMap de Azure Resource Manager. El campo ResourceName_s coincide con el campo de nombre del recurso correspondiente de Resource Manager. 

>[!NOTE]
>A medida que aumentan las características de Service Map, estos campos están sujetos a cambios.

Hay propiedades generadas internamente que puede usar para identificar los equipos y procesos únicos:

- Equipo: Use *ResourceId* o *ResourceName_s* para identificar de forma exclusiva un equipo dentro de un área de trabajo de Log Analytics.
- Proceso: Use *ResourceId* para identificar de forma exclusiva un proceso dentro de un área de trabajo de Log Analytics. *ResourceName_s* es único dentro del contexto de la máquina en la que se está ejecutando el proceso (MachineResourceName_s) 

Puesto que pueden existir varios registros para un proceso y equipo especificados en un intervalo de tiempo concreto, las consultas pueden devolver más de un registro para el mismo proceso o equipo. Para incluir solo el registro más reciente agregue "| dedup ResourceId" a la consulta.

### <a name="connections"></a>Conexiones

Las métricas de conexión se escriben en una nueva tabla en Log Analytics: VMConnection. Esta tabla proporciona información sobre las conexiones para una máquina (entrantes y salientes). Las métricas de conexión también se exponen a través de API que proporcionan los medios para obtener una métrica específica durante un período de tiempo.  Las conexiones TCP resultantes de aceptar en un socket de escucha son de entrada, mientras que las creadas al conectarse a un puerto y una dirección IP determinados son de salida. La dirección de una conexión se representa mediante la propiedad Direction, que se puede definir como **inbound** u **outbound**. 

Los registros de estas tablas se generan a partir de los datos que notifica el agente de dependencia. Cada registro representa una observación en un intervalo de tiempo de un minuto. La propiedad TimeGenerated indica el inicio del intervalo de tiempo. Cada registro contiene información para identificar la entidad correspondiente; es decir, conexión o puerto, así como las métricas asociadas con esa entidad. Actualmente, solo se notifica la actividad de red que tiene lugar mediante TCP a través de IPv4.

Para administrar el costo y la complejidad, los registros de conexión no representan conexiones de red físicas individuales. Varias conexiones de red físicas se agrupan en una conexión lógica, que, a continuación, se refleja en la tabla correspondiente.  Lo que significa que los registros de la tabla *VMConnection* representan una agrupación lógica, y no las conexiones físicas individuales que se observan. Las conexiones de red físicas que comparten el mismo valor para los siguientes atributos durante un intervalo determinado de un minuto se agregan en un registro lógico único en *VMConnection*. 

| Propiedad | Descripción |
|:--|:--|
| `Direction` |Dirección de la conexión; el valor es *inbound* u *outbound* |
| `Machine` |FQDN del equipo |
| `Process` |Identidad de proceso o grupos de procesos; iniciar/aceptar la conexión |
| `SourceIp` |Dirección IP de origen |
| `DestinationIp` |Dirección IP de destino. |
| `DestinationPort` |Número de puerto de destino |
| `Protocol` |Protocolo utilizado para la conexión.  Los valores son *tcp*. |

Para tener en cuenta el impacto de la agrupación, se proporciona información sobre el número de conexiones físicas agrupadas en las siguientes propiedades del registro:

| Propiedad | Descripción |
|:--|:--|
| `LinksEstablished` |Número de conexiones de red físicas que se han establecido durante el período de tiempo de generación de informes |
| `LinksTerminated` |Número de conexiones de red físicas que han finalizado durante el período de tiempo de generación de informes |
| `LinksFailed` |Número de conexiones de red físicas que han generado errores durante el período de tiempo de generación de informes Actualmente, esta información está disponible solo para las conexiones salientes. |
| `LinksLive` |Número de conexiones de red físicas que estaban abiertas al final del período de tiempo de generación de informes|

#### <a name="metrics"></a>Métricas

Además de las métricas de recuento de conexión, también se incluye información sobre el volumen de datos enviado y recibido en una conexión lógica o puerto de red concreto en las siguientes propiedades del registro:

| Propiedad | Descripción |
|:--|:--|
| `BytesSent` |Número total de bytes enviados durante el período de tiempo de generación de informes |
| `BytesReceived` |Número total de bytes recibidos durante el período de tiempo de generación de informes |
| `Responses` |Número de respuestas observado durante el período de tiempo de generación de informes. 
| `ResponseTimeMax` |Mayor tiempo de respuesta (milisegundos) observado durante el período de tiempo de generación de informes.  Si no hay ningún valor, la propiedad está en blanco.|
| `ResponseTimeMin` |Menor tiempo de respuesta (milisegundos) observado durante el período de tiempo de generación de informes.  Si no hay ningún valor, la propiedad está en blanco.|
| `ResponseTimeSum` |Suma de todos los tiempos de respuesta (milisegundos) observados durante el período de tiempo de generación de informes.  Si no hay ningún valor, la propiedad está en blanco.|

El tercer tipo de datos que se va a notificar es el tiempo de respuesta: ¿cuánto tiempo pasa el autor de la llamada esperando que una solicitud enviada a través de una conexión se procese y reciba respuesta del punto de conexión remoto? El tiempo de respuesta notificado es una estimación del tiempo de respuesta real del protocolo de aplicación subyacente. Se calcula utilizando la heurística basada en la observación del flujo de datos entre el origen y destino de una conexión de red física. Conceptualmente, es la diferencia entre el momento en que el último byte de una solicitud sale del remitente y el momento en que llega el último byte de la respuesta. Estas dos marcas de tiempo se utilizan para delinear los eventos de solicitud y respuesta de una conexión física concreta. La diferencia entre ellas representa el tiempo de respuesta de una única solicitud. 

En esta primera versión de esta característica, el algoritmo es una aproximación que puede funcionar con cierto grado de éxito según el protocolo de aplicación real usado para una conexión de red concreta. Por ejemplo, el enfoque actual funciona bien para los protocolos basados en solicitud-respuesta, como HTTP(S), pero no funciona con protocolos unidireccionales ni basados en la cola de mensajes.

A continuación se incluyen puntos importantes que debe tener en cuenta:

1. Si un proceso acepta conexiones en la misma dirección IP, pero a través de varias interfaces de red, se notificará un registro independiente para cada interfaz. 
2. Los registros con IP comodín no contendrán ninguna actividad. Se incluyen para representar el hecho de que un puerto del equipo está abierto al tráfico de entrada.
3. Para reducir el nivel de detalle y el volumen de datos, los registros con dirección IP de carácter comodín se omitirán cuando haya un registro coincidente (para el mismo proceso, puerto y protocolo) con una dirección IP específica. Cuando un registro de dirección IP comodín se omite, la propiedad del registro IsWildcardBind con la dirección IP específica se definirá como "True" para indicar que el puerto se expone a través de cada interfaz de la máquina de generación de informes.
4. Los puertos que están enlazados solo en una interfaz específica tienen IsWildcardBind definido como "False".

#### <a name="naming-and-classification"></a>Nomenclatura y clasificación

Para mayor comodidad, la dirección IP del extremo remoto de una conexión se incluye en la propiedad RemoteIp. Para las conexiones entrantes, RemoteIp coincide con SourceIp, mientras que, para las conexiones salientes, coincide con DestinationIp. La propiedad RemoteDnsCanonicalNames representa los nombres canónicos DNS que notifica la máquina para RemoteIp. Las propiedades RemoteDnsQuestions y RemoteClassification están reservadas para uso futuro. 

#### <a name="geolocation"></a>Geolocalización

*VMConnection* también incluye información de ubicación geográfica para el extremo remoto de cada registro de conexión en las siguientes propiedades del registro: 

| Propiedad | Descripción |
|:--|:--|
| `RemoteCountry` |Nombre del país o región que hospeda la dirección IP de RemoteIp.  Por ejemplo: *Estados Unidos* |
| `RemoteLatitude` |Latitud de geolocalización.  Por ejemplo, *47.68* |
| `RemoteLongitude` |Longitud de geolocalización.  Por ejemplo, *-122.12* |

#### <a name="malicious-ip"></a>Direcciones IP malintencionadas

Todas las propiedades de RemoteIp de la tabla *VMConnection* se comparan con un conjunto de direcciones IP con actividad malintencionada conocida. Si el valor de RemoteIp se identifica como malintencionado, las propiedades siguientes se completarán (si la IP no se considera malintencionada, están vacías) en las siguientes propiedades del registro:

| Propiedad | Descripción |
|:--|:--|
| `MaliciousIp` |Dirección RemoteIp |
| `IndicatorThreadType` |El indicador de amenazas detectado es uno de los siguientes valores: *Botnet*, *C2*, *CryptoMining*, *Darknet*, *DDos* , *MaliciousUrl*, *Malware*, *Phishing*, *Proxy*, *PUA*, *Watchlist*.   |
| `Description` |Descripción de la amenaza observada. |
| `TLPLevel` |Nivel de protocolo de semáforo (TLP) es uno de los valores definidos: *blanco*, *verde*, *ámbar*, *rojo*. |
| `Confidence` |Los valores válidos se encuentran entre *0 y 100*. |
| `Severity` |Los valores se encuentran entre *0 y 5*, donde *5* es el más grave y *0* no es grave en absoluto. El valor predeterminado es *3*.  |
| `FirstReportedDateTime` |La primera vez que el proveedor informó sobre el indicador. |
| `LastReportedDateTime` |La última vez que Interflow ha visto el indicador. |
| `IsActive` |Indica que los indicadores se desactivan con el valor *True* o *False*. |
| `ReportReferenceLink` |Vincula a informes relacionados con un objeto observable especificado. |
| `AdditionalInformation` |Proporciona información adicional, si procede, sobre la amenaza observada. |

### <a name="servicemapcomputer_cl-records"></a>Registros de ServiceMapComputer_CL

Los registros con un tipo de *ServiceMapComputer_CL* tienen datos de inventario de servidores con agentes de Mapa de servicio. Estos registros tienen las propiedades de la tabla siguiente:

| Propiedad | Descripción |
|:--|:--|
| `Type` | *ServiceMapComputer_CL* |
| `SourceSystem` | *OpsManager* |
| `ResourceId` | Identificador único de una máquina en el área de trabajo |
| `ResourceName_s` | Identificador único de una máquina en el área de trabajo |
| `ComputerName_s` | FQDN del equipo |
| `Ipv4Addresses_s` | Lista de las direcciones IPv4 del servidor |
| `Ipv6Addresses_s` | Lista de las direcciones IPv6 del servidor |
| `DnsNames_s` | Matriz de nombres DNS |
| `OperatingSystemFamily_s` | Windows o Linux |
| `OperatingSystemFullName_s` | Nombre completo del sistema operativo  |
| `Bitness_s` | Valor de bits del equipo (32 o 64 bits)  |
| `PhysicalMemory_d` | Memoria física en MB |
| `Cpus_d` | Número de CPU |
| `CpuSpeed_d` | Velocidad de la CPU en MHz|
| `VirtualizationState_s` | *unknown*, *physical*, *virtual*, *hypervisor* |
| `VirtualMachineType_s` | *hyperv*, *vmware*, etc. |
| `VirtualMachineNativeMachineId_g` | Identificador de máquina virtual asignado por su hipervisor |
| `VirtualMachineName_s` | Nombre de la máquina virtual |
| `BootTime_t` | Tiempo de arranque |

### <a name="servicemapprocess_cl-type-records"></a>Registros con un tipo ServiceMapProcess_CL

Los registros con un tipo de *ServiceMapProcess_CL* tienen datos de inventario para procesos con conexión TCP en servidores con agentes de Mapa de servicio. Estos registros tienen las propiedades de la tabla siguiente:

| Propiedad | Descripción |
|:--|:--|
| `Type` | *ServiceMapProcess_CL* |
| `SourceSystem` | *OpsManager* |
| `ResourceId` | Identificador único de un proceso en el área de trabajo |
| `ResourceName_s` | Identificador único de un proceso en el equipo en el que se está ejecutando|
| `MachineResourceName_s` | Nombre de recurso del equipo |
| `ExecutableName_s` | Nombre del archivo ejecutable del proceso |
| `StartTime_t` | Hora de inicio del grupo de procesos |
| `FirstPid_d` | Primer PID del grupo de procesos |
| `Description_s` | Descripción del proceso |
| `CompanyName_s` | Nombre de la compañía |
| `InternalName_s` | Nombre interno |
| `ProductName_s` | Nombre del producto |
| `ProductVersion_s` | Versión del producto |
| `FileVersion_s` | Versión del archivo |
| `CommandLine_s` | Línea de comandos |
| `ExecutablePath _s` | Ruta de acceso al archivo ejecutable |
| `WorkingDirectory_s` | Directorio de trabajo |
| `UserName` | Cuenta en la que se está ejecutando el proceso |
| `UserDomain` | Dominio en el que se está ejecutando el proceso |

## <a name="sample-log-searches"></a>Búsquedas de registros de ejemplo

### <a name="list-all-known-machines"></a>Enumerar todas las máquinas conocidas

`ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Enumerar la capacidad de memoria física de todos los equipos administrados.

`ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project PhysicalMemory_d, ComputerName_s`

### <a name="list-computer-name-dns-ip-and-os"></a>Enumerar el nombre de equipo, DNS, IP y SO.

`ServiceMapComputer_CL | summarize arg_max(TimeGenerated, *) by ResourceId | project ComputerName_s, OperatingSystemFullName_s, DnsNames_s, Ipv4Addresses_s`

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Buscar todos los procesos con "sql" en la línea de comandos

`ServiceMapProcess_CL | where CommandLine_s contains_cs "sql" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Buscar una máquina (registro más reciente) por el nombre de recurso

`search in (ServiceMapComputer_CL) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>Buscar un equipo (registro más reciente) por dirección IP

`search in (ServiceMapComputer_CL) "10.229.243.232" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="list-all-known-processes-on-a-specified-machine"></a>Enumerar todos los procesos conocidos en un equipo determinado

`ServiceMapProcess_CL | where MachineResourceName_s == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | summarize arg_max(TimeGenerated, *) by ResourceId`

### <a name="list-all-computers-running-sql"></a>Enumerar todos los equipos que ejecutan SQL

`ServiceMapComputer_CL | where ResourceName_s in ((search in (ServiceMapProcess_CL) "\*sql\*" | distinct MachineResourceName_s)) | distinct ComputerName_s`

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Enumerar todas las versiones de producto únicas de curl en mi centro de datos

`ServiceMapProcess_CL | where ExecutableName_s == "curl" | distinct ProductVersion_s`

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Crear un grupo de equipos de todos los equipos con CentOS

`ServiceMapComputer_CL | where OperatingSystemFullName_s contains_cs "CentOS" | distinct ComputerName_s`

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Resumir las conexiones salientes desde un grupo de máquinas

```
// the machines of interest
let machines = datatable(m: string) ["m-82412a7a-6a32-45a9-a8d6-538354224a25"];
// map of ip to monitored machine in the environment
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
// all connections to/from the machines of interest
let out=materialize(VMConnection
| where Machine in (machines)
| summarize arg_max(TimeGenerated, *) by ConnectionId);
// connections to localhost augmented with RemoteMachine
let local=out
| where RemoteIp startswith "127."
| project ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=Machine;
// connections not to localhost augmented with RemoteMachine
let remote=materialize(out
| where RemoteIp !startswith "127."
| join kind=leftouter (ips) on $left.RemoteIp == $right.ips
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=MonitoredMachine);
// the remote machines to/from which we have connections
let remoteMachines = remote | summarize by RemoteMachine;
// all augmented connections
(local)
| union (remote)
//Take all outbound records but only inbound records that come from either //unmonitored machines or monitored machines not in the set for which we are computing dependencies.
| where Direction == 'outbound' or (Direction == 'inbound' and RemoteMachine !in (machines))
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine
// identify the remote port
| extend RemotePort=iff(Direction == 'outbound', DestinationPort, 0)
// construct the join key we'll use to find a matching port
| extend JoinKey=strcat_delim(':', RemoteMachine, RemoteIp, RemotePort, Protocol)
// find a matching port
| join kind=leftouter (VMBoundPort 
| where Machine in (remoteMachines) 
| summarize arg_max(TimeGenerated, *) by PortId 
| extend JoinKey=strcat_delim(':', Machine, Ip, Port, Protocol)) on JoinKey
// aggregate the remote information
| summarize Remote=makeset(iff(isempty(RemoteMachine), todynamic('{}'), pack('Machine', RemoteMachine, 'Process', Process1, 'ProcessName', ProcessName1))) by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol
```

## <a name="rest-api"></a>API DE REST

Todos los datos de servidores, procesos y dependencias de Service Map están disponibles a través de la [API de REST de Service Map](/rest/api/servicemap/).

## <a name="diagnostic-and-usage-data"></a>Datos de diagnóstico y uso

Microsoft recopila automáticamente datos de uso y rendimiento a través del servicio Mapa de servicio. Microsoft usa estos datos para proporcionar calidad, seguridad e integridad en el servicio Service Map y para mejorarlas. Con el fin de proporcionar funcionalidades de solución de problemas precisas y eficientes, los datos incluyen información sobre la configuración del software, como sistema operativo y versión, dirección IP, nombre DNS y nombre de la estación de trabajo. Microsoft no recopila nombres, direcciones ni otra información de contacto.

Para más información sobre el uso y la recopilación de datos, vea la [Declaración de privacidad de Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las [búsquedas de registros](../logs/log-query-overview.md) de Log Analytics para recuperar datos recopilados por Service Map.

## <a name="troubleshooting"></a>Solución de problemas

Esta sección puede ayudarle si tiene problemas para instalar o ejecutar Service Map. Si sigue sin poder resolver el problema, póngase en contacto con Soporte técnico de Microsoft.

### <a name="dependency-agent-installation-problems"></a>Problemas de instalación de Dependency Agent

#### <a name="installer-prompts-for-a-reboot"></a>El instalador solicita un reinicio
Dependency Agent, *por lo general*, no requiere un reinicio durante su instalación o retirada. Sin embargo, en algunos casos poco frecuentes, un servidor de Windows Server requiere un reinicio para continuar con una instalación. Esto ocurre cuando una dependencia, normalmente los redistribuibles de Microsoft Visual C++, requiere un reinicio debido a un archivo bloqueado.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--code_number-appears"></a>Aparece el mensaje "Unable to install Dependency agent: Visual Studio Runtime libraries failed to install (code = [code_number])" (No se puede instalar Dependency Agent: error al instalar las bibliotecas en tiempo de ejecución de Visual Studio (código = [número_código]))

Microsoft Dependency Agent se basa en las bibliotecas del entorno de tiempo de ejecución de Microsoft Visual Studio. Recibirá un mensaje si hay algún problema durante la instalación de las bibliotecas. 

Los instaladores de la biblioteca en tiempo de ejecución crean registros en la carpeta %LOCALAPPDATA%\temp. El archivo es `dd_vcredist_arch_yyyymmddhhmmss.log`, donde *arch* es `x86` o `amd64` y *yyyymmddhhmmss* es la fecha y hora (con formato de 24 horas) en que se creó el registro. El registro proporciona detalles sobre el problema que bloquea la instalación.

Puede ser útil instalar uno mismo las [últimas bibliotecas en entorno de ejecución](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) primero.

En la tabla siguiente se muestran números de código y resoluciones sugeridas.

| Código | Descripción | Resolución |
|:--|:--|:--|
| 0 x 17 | El instalador de la biblioteca requiere una actualización de Windows que no se ha instalado. | Mire el registro del instalador de la biblioteca más reciente.<br><br>Si una referencia a `Windows8.1-KB2999226-x64.msu` va seguida de una línea `Error 0x80240017: Failed to execute MSU package,`, no tiene los requisitos previos para instalar KB2999226. Siga las instrucciones que aparecen en la sección de requisitos previos en el artículo [Universal C Runtime en Windows](https://support.microsoft.com/kb/2999226). Es posible que tenga que ejecutar Windows Update y reiniciar varias veces para instalar los requisitos previos.<br><br>Ejecute de nuevo el instalador Microsoft Dependency Agent. |

### <a name="post-installation-issues"></a>Problemas posteriores a la instalación

#### <a name="server-doesnt-appear-in-service-map"></a>El servidor no aparece en Service Map

Si la instalación de Dependency Agent se ha realizado correctamente, pero no ve la máquina en la solución de Service Map:
* ¿Se ha instalado Dependency Agent correctamente? Para validarlo, compruebe si el servicio está instalado y se ejecuta.<br><br>
**Windows**: Busque el servicio llamado **Microsoft Dependency Agent**.
**Linux**: Busque el proceso en ejecución **microsoft-dependency-agent**.

* ¿Está en el [nivel Gratis de Log Analytics](https://azure.microsoft.com/pricing/details/monitor/)? El plan Gratis permite hasta cinco máquinas de Service Map únicas. Ninguna máquina posterior aparecerá en Service Map, incluso si los cinco anteriores ya no envían datos.

* ¿El servidor envía datos de registro y de rendimiento a los registros de Azure Monitor? Vaya a Azure Monitor\Logs y ejecute la siguiente consulta para el equipo: 

    ```kusto
    Usage | where Computer == "admdemo-appsvr" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

¿obtuvo una amplia variedad de eventos en los resultados? ¿Son los datos recientes? Si es así, el agente de Log Analytics funciona correctamente y se comunica con el área de trabajo. Si no es así, compruebe el agente en la máquina: [Log Analytics agent for Windows troubleshooting](../agents/agent-windows-troubleshoot.md) (Cómo solucionar problemas relacionados con el agente de Windows de Log Analytics) o [Cómo solucionar problemas relacionados con el agente de Linux de Log Analytics](../agents/agent-linux-troubleshoot.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>El servidor aparece en Service Map, pero no tiene procesos

Si ve la máquina en Service Map, pero no tiene datos de proceso ni de conexión, quiere decir que Dependency Agent está instalado y se ejecuta. Sin embargo, el controlador kernel no se ha cargado. 

Compruebe `C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log file` (Windows) o `/var/opt/microsoft/dependency-agent/log/service.log file` (Linux). Las últimas líneas del archivo deben indicar por qué no se cargó el kernel. Por ejemplo, es posible que el kernel no sea compatible con Linux si actualizó el kernel.

## <a name="suggestions"></a>Sugerencias

¿Quiere hacernos llegar algún comentario acerca de Mapa de servicio o esta documentación?  Visite la [página UserVoice](https://feedback.azure.com/d365community/forum/aa68334e-1925-ec11-b6e6-000d3a4f09d0?c=ad4304e4-1925-ec11-b6e6-000d3a4f09d0) para sugerir características o votar sugerencias existentes.
