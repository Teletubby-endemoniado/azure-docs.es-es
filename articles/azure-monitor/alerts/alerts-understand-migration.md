---
title: Descripción de la migración para las alertas de Azure Monitor
description: Comprenda el funcionamiento de la migración de alertas y solucione problemas.
ms.topic: conceptual
ms.date: 09/06/2021
ms.author: yalavi
author: yalavi
ms.openlocfilehash: 2167e0ea05206bc9c991353d6518090773934635
ms.sourcegitcommit: f2d0e1e91a6c345858d3c21b387b15e3b1fa8b4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2021
ms.locfileid: "123543662"
---
# <a name="understand-migration-options-to-newer-alerts"></a>Descripción de las opciones de migración para las alertas más recientes

Las alertas públicas se han [retirado](./monitoring-classic-retirement.md) para los usuarios de la nube pública. Las alertas clásicas para la nube de Azure Government y Azure China 21Vianet se retirarán el **29 de febrero de 2024**.

En este artículo se explica cómo funcionan las herramientas de migración manual y de migración voluntaria, que se usarán para migrar las reglas de alertas restantes. También se describen soluciones para algunos problemas comunes.

> [!IMPORTANT]
> La migración no afecta a las alertas de registro de actividad (incluidas alertas de estado de servicio) ni las alertas de registros. La migración solo se aplica a las reglas de alertas clásicas que se describen [aquí](./monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform).

> [!NOTE]
> Si las reglas de alertas clásicas no son válidas, es decir, se encuentran en [métricas en desuso](#classic-alert-rules-on-deprecated-metrics) o recursos eliminados, no se migrarán ni estarán disponibles tras retirarse el servicio.

## <a name="manually-migrating-classic-alerts-to-newer-alerts"></a>Migración manual de alertas clásicas a alertas más recientes

Los clientes que estén interesados en migrar manualmente las alertas restantes ya pueden hacerlo con las secciones siguientes. También incluye las métricas que se retiran y, por lo tanto, no se pueden migrar directamente.

### <a name="guest-metrics-on-virtual-machines"></a>Métricas de invitado en máquinas virtuales

Para poder crear nuevas alertas de métricas en las métricas de invitado, estas últimas se deben enviar al almacén de registros de Azure Monitor. Para crear alertas, siga estas instrucciones:

- [Habilitación de la recopilación de métricas de invitado en Log Analytics](../agents/agent-data-sources.md)
- [Creación de alertas de registro en Azure Monitor](./alerts-log.md)

Existen más opciones para recopilar métricas de invitado y alertar sobre ellas. Obtenga [más información](../agents/agents-overview.md).

### <a name="storage-and-classic-storage-account-metrics"></a>Métricas de cuenta de almacenamiento y de almacenamiento clásico

Todas las alertas clásicas en las cuentas de almacenamiento se pueden migrar, excepto las alertas de estas métricas:

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError
- ThrottlingError

La reglas de alertas clásicas en las métricas de porcentaje se deben migrar en función de la [asignación entre las métricas de almacenamiento antiguas y nuevas](../../storage/common/storage-metrics-migration.md#metrics-mapping-between-old-metrics-and-new-metrics). Los umbrales deberán modificarse según corresponda porque la nueva métrica disponible es absoluta.

Las reglas de alertas clásicas en AnonymousThrottlingError, SASThrottlingError y ThrottlingError deben dividirse en dos nuevas alertas porque no hay ninguna métrica combinada que proporcione la misma funcionalidad. Los umbrales deberán adaptarse según corresponda.

### <a name="cosmos-db-metrics"></a>Métricas de Cosmos DB

Todas las alertas clásicas de las métricas de Cosmos DB se pueden migrar, excepto las alertas de estas métricas:

- Solicitudes medias por segundo
- Nivel de coherencia
- Http 2xx
- Http 3xx
- Número máximo de RUPM consumidas por minuto
- Número máximo de RU por segundo
- Cargo de otras solicitudes de Mongo
- Velocidad de otras solicitudes de Mongo
- Latencia de lectura observada
- Latencia de escritura observada
- Disponibilidad del servicio
- Capacidad de almacenamiento

Actualmente no están disponibles en el [nuevo sistema](../essentials/metrics-supported.md#microsoftdocumentdbdatabaseaccounts) las solicitudes medias por segundo, el nivel de coherencia, el número máximo de RUPM consumidas por minuto, el número máximo de RU por segundo, la latencia de lectura observada, la latencia de escritura observada y la capacidad de almacenamiento.

Las alertas de métricas de solicitud, como HTTP 2xx, HTTP 3xx y la disponibilidad del servicio, no se migran porque la manera en que se cuentan las solicitudes es diferente entre las métricas clásicas y las nuevas. Las alertas sobre estas métricas deberán volver a crearse manualmente con umbrales ajustados.

### <a name="classic-alert-rules-on-deprecated-metrics"></a>Reglas de alertas clásicas en métricas en desuso

A continuación se incluyen las reglas de alertas clásicas en las métricas que se admitían anteriormente, pero finalmente han quedado en desuso. Un pequeño porcentaje de clientes podría tener reglas de alertas clásicas no válidas en esas métricas. Puesto que estas reglas de alertas no son válidas, no se migrarán.

| Tipo de recurso| Métricas en desuso |
|-------------|----------------- |
| Microsoft.DBforMySQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.DBforPostgreSQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.Network/publicIPAddresses | defaultddostriggerrate |
| Microsoft.SQL/servers/databases | service_level_objective, storage_limit, storage_used, throttling, dtu_consumption_percent, storage_used |
| Microsoft.Web/hostingEnvironments/multirolepools | averagememoryworkingset |
| Microsoft.Web/hostingEnvironments/workerpools | bytesreceived, httpqueuelength |

## <a name="how-equivalent-new-alert-rules-and-action-groups-are-created"></a>Cómo se crean nuevas reglas de alertas equivalente y grupos de acciones

La herramienta de migración convierte las reglas de alertas clásicas en nuevas reglas de alertas equivalentes y grupos de acciones. Para la mayoría las reglas de alertas clásicas, las nuevas reglas de alertas equivalentes están en la misma métrica con las mismas propiedades, como `windowSize` y `aggregationType`. Sin embargo, hay algunas reglas de alertas clásicas que se encuentran en las métricas que tienen una métrica equivalente diferente en el nuevo sistema. Los siguientes principios se aplican a la migración de las alertas clásicas, a menos que especifique en la sección siguiente:

- **Frecuencia**: define la frecuencia con la que se comprueba la condición de una regla de alertas clásicas o nueva. El usuario no podía configurar el valor de `frequency` en las reglas de alertas clásicas y siempre era de 5 minutos para todos los tipos de recursos. La frecuencia de las reglas equivalentes también se establece en 5 minutos.
- **Tipo de agregación**: define cómo se agrega la métrica a través de la ventana de interés. El elemento `aggregationType` también es el mismo entre las alertas clásicas y las alertas nuevas para la mayoría de las métricas. En algunos casos, dado que la métrica es diferente entre las alertas clásicas y las alertas nuevas, se usa el elemento `aggregationType` equivalente o el elemento `primary Aggregation Type` definido para la métrica.
- **Unidades**: propiedad de la métrica en la que se crea la alerta. Algunas métricas equivalentes tienen unidades diferentes. El umbral se ajusta según corresponda y en función de la necesidad. Por ejemplo, si la métrica original tiene segundos como unidades, pero la nueva métrica equivalente tiene milisegundos como unidades, el umbral original se multiplica por 1000 para garantizar el mismo comportamiento.
- **Tamaño de la ventana**: define la ventana durante la que los datos de la métrica se agregan para compararlos con el umbral. Para valores `windowSize` estándares, como 5 minutos, 15 minutos, 30 minutos, 1 hora, 3 horas, 6 horas, 12 horas o 1 día, no se ha realizado ningún cambio para la nueva regla de alertas equivalente. Para otros valores, se utiliza el valor de `windowSize` más cercano. Para la mayoría de los clientes, este cambio no tiene ningún impacto. Para un pequeño porcentaje de clientes, es posible que sea necesario ajustar el umbral para obtener exactamente el mismo comportamiento.

En las secciones siguientes, detallamos las métricas que tienen una métrica diferente equivalente en el nuevo sistema. Las métricas que permanecen iguales para las mismas reglas de alertas clásicas y nuevas no se indican. Encontrará una lista de las métricas que se admiten en el nuevo sistema [aquí](../essentials/metrics-supported.md).

### <a name="microsoftstoragestorageaccounts-and-microsoftclassicstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts y Microsoft.ClassicStorage/storageAccounts

Para servicios de cuenta de almacenamiento, como blob, tabla, archivo y cola, se asignan las siguientes métricas a las métricas equivalentes, tal como se muestra a continuación:

| Métrica en las alertas clásicas | Métrica equivalente en las alertas nuevas | Comentarios|
|--------------------------|---------------------------------|---------|
| AnonymousAuthorizationError| Métrica Transacciones con las dimensiones "ResponseType"="AuthorizationError" y "Authentication" = "Anonymous"| |
| AnonymousClientOtherError | Métrica Transacciones con las dimensiones "ResponseType"="ClientOtherError" y "Authentication" = "Anonymous" | |
| AnonymousClientTimeOutError| Métrica Transacciones con las dimensiones "ResponseType"="ClientTimeOutError" y "Authentication" = "Anonymous" | |
| AnonymousNetworkError | Métrica Transacciones con las dimensiones "ResponseType"="NetworkError" y "Authentication" = "Anonymous" | |
| AnonymousServerOtherError | Métrica Transacciones con las dimensiones "ResponseType"="ServerOtherError" y "Authentication" = "Anonymous" | |
| AnonymousServerTimeOutError | Métrica Transacciones con las dimensiones "ResponseType"="ServerTimeOutError" y "Authentication" = "Anonymous" | |
| AnonymousSuccess | Métrica Transacciones con las dimensiones "ResponseType"="Success" y "Authentication" = "Anonymous" | |
| AuthorizationError | Métrica Transacciones con las dimensiones "ResponseType"="AuthorizationError" | |
| AverageE2ELatency | SuccessE2ELatency | |
| AverageServerLatency | SuccessServerLatency | |
| Capacity | BlobCapacity | Use `aggregationType` "average" en lugar de "last". La métrica solo se aplica a Blob Service |
| ClientOtherError | Métrica Transacciones con las dimensiones "ResponseType"="ClientOtherError"  | |
| ClientTimeoutError | Métrica Transacciones con las dimensiones "ResponseType"="ClientTimeOutError" | |
| ContainerCount | ContainerCount | Use `aggregationType` "average" en lugar de "last". La métrica solo se aplica a Blob Service |
| NetworkError | Métrica Transacciones con las dimensiones "ResponseType"="NetworkError" | |
| ObjectCount | BlobCount| Use `aggregationType` "average" en lugar de "last". La métrica solo se aplica a Blob Service |
| SASAuthorizationError | Métrica Transacciones con las dimensiones "ResponseType"="AuthorizationError" y "Authentication" = "SAS" | |
| SASClientOtherError | Métrica Transacciones con las dimensiones "ResponseType"="ClientOtherError" y "Authentication" = "SAS" | |
| SASClientTimeOutError | Métrica Transacciones con las dimensiones "ResponseType"="ClientTimeOutError" y "Authentication" = "SAS" | |
| SASNetworkError | Métrica Transacciones con las dimensiones "ResponseType"="NetworkError" y "Authentication" = "SAS" | |
| SASServerOtherError | Métrica Transacciones con las dimensiones "ResponseType"="ServerOtherError" y "Authentication" = "SAS" | |
| SASServerTimeOutError | Métrica Transacciones con las dimensiones "ResponseType"="ServerTimeOutError" and "Authentication" = "SAS" | |
| SASSuccess | Métrica Transacciones con las dimensiones "ResponseType"="NetworkError" y "Authentication" = "SAS" | |
| ServerOtherError | Métrica Transacciones con las dimensiones "ResponseType"="ClientOtherError" | |
| ServerTimeOutError | Métrica Transacciones con las dimensiones "ResponseType"="ServerTimeOutError"  | |
| Correcto | Métrica Transacciones con las dimensiones "ResponseType"="Success" | |
| TotalBillableRequests| Transacciones | |
| TotalEgress | Salida | |
| TotalIngress | Entrada | |
| TotalRequests | Transacciones | |

### <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

Para Cosmos DB, las métricas equivalentes se muestran a continuación:

| Métrica en las alertas clásicas | Métrica equivalente en las alertas nuevas | Comentarios|
|--------------------------|---------------------------------|---------|
| AvailableStorage | AvailableStorage||
| Tamaño de datos | DataUsage| |
| Recuento de documentos | DocumentCount||
| Tamaño de índice | IndexUsage||
| Servicio no disponible | ServiceAvailability||
| TotalRequestUnits | TotalRequestUnits||
| Solicitudes limitadas | TotalRequests con la dimensión "StatusCode" = "429"| El tipo de agregación "Average" se corrige como "Count".|
| Errores internos del servidor | TotalRequests con la dimensión "StatusCode" = "500"}| El tipo de agregación "Average" se corrige como "Count".|
| Http 401 | TotalRequests con la dimensión "StatusCode" = "401"| El tipo de agregación "Average" se corrige como "Count".|
| Http 400 | TotalRequests con la dimensión "StatusCode" = "400"| El tipo de agregación "Average" se corrige como "Count".|
| Total de solicitudes | TotalRequests| El tipo de agregación "Max" se corrige como "Count".|
| Cargo de la solicitud de recuento de Mongo| MongoRequestCharge con dimensión "CommandName" = "count"||
| Velocidad de la solicitud de recuento de Mongo | MongoRequestsCount con dimensión "CommandName" = "count"||
| Carga de la solicitud de eliminación de Mongo | MongoRequestCharge con dimensión "CommandName" = "delete"||
| Velocidad de la solicitud de eliminación de Mongo | MongoRequestsCount con dimensión "CommandName" = "delete"||
| Cargo de la solicitud de inserción de Mongo | MongoRequestCharge con dimensión "CommandName" = "insert"||
| Velocidad de la solicitud de inserción de Mongo | MongoRequestsCount con dimensión "CommandName" = "insert"||
| Cargo de la solicitud de consulta de Mongo | MongoRequestCharge con dimensión "CommandName" = "find"||
| Velocidad de la solicitud de consulta de Mongo | MongoRequestsCount con dimensión "CommandName" = "find"||
| Carga de la solicitud de actualización de Mongo | MongoRequestCharge con dimensión "CommandName" = "update"||
| Solicitudes con error de inserción de Mongo | MongoRequestCount con las dimensiones "CommandName" = "insert" y "Status" = "failed"| El tipo de agregación "Average" se corrige como "Count".|
| Solicitudes con error de consultas de Mongo | MongoRequestCount con las dimensiones "CommandName" = "query" y "Status" = "failed"| El tipo de agregación "Average" se corrige como "Count".|
| Solicitudes con error de recuento de Mongo | MongoRequestCount con las dimensiones "CommandName" = "count" y "Status" = "failed"| El tipo de agregación "Average" se corrige como "Count".|
| Solicitudes con error de actualización de Mongo | MongoRequestCount con las dimensiones "CommandName" = "update" y "Status" = "failed"| El tipo de agregación "Average" se corrige como "Count".|
| Otras solicitudes con error de Mongo | MongoRequestCount con las dimensiones "CommandName" = "other" y "Status" = "failed"| El tipo de agregación "Average" se corrige como "Count".|
| Solicitudes con error de eliminación de Mongo | MongoRequestCount con las dimensiones "CommandName" = "delete" y "Status" = "failed"| El tipo de agregación "Average" se corrige como "Count".|

### <a name="how-equivalent-action-groups-are-created"></a>Creación de grupos de acción equivalentes

Las reglas de alertas clásicas tenían vinculadas acciones de correo electrónico, webhook, runbook y aplicación lógica. Las nuevas reglas de alertas usan grupos de acciones que se pueden volver a usar en varias reglas de alertas. La herramienta de migración crea el grupo de acciones único para las mismas acciones, independientemente de cuántas reglas de alertas usan la acción. Los grupos de acciones que crea la herramienta de migración usan el formato de nombres "Migrated_AG*".

> [!NOTE]
> Las alertas clásicas envían correos electrónicos localizados en función de la configuración regional del administrador clásico cuando se usan para notificar roles de administrador clásico. Los nuevos mensajes de alerta se envían a través de grupos de acciones y solo están en inglés.

## <a name="rollout-phases"></a>Fases de lanzamiento

La herramienta de migración se está lanzando en fases para los clientes que usan reglas de alertas clásicas. Los propietarios de suscripciones recibirán un correo electrónico cuando la suscripción esté lista para la migración mediante el uso de la herramienta.

> [!NOTE]
> Dado que la herramienta se está lanzando en fases, es posible que vea que algunas de sus suscripciones no están preparadas para la migración durante las fases iniciales.

Actualmente, la mayoría de las suscripciones están marcadas como listas para la migración. Solo las suscripciones que tienen alertas clásicas en los siguientes tipos de recursos todavía no están listas para la migración.

- Microsoft.classicCompute/domainNames/slots/roles
- Microsoft.insights/components

## <a name="who-can-trigger-the-migration"></a>¿Quién puede desencadenar la migración?

Cualquier usuario que tenga el rol integrado de colaborador de supervisión en el nivel de la suscripción puede desencadenar la migración. Los usuarios que tienen un rol personalizado con los siguientes permisos también pueden desencadenar la migración:

- */read
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*
- Microsoft.AlertsManagement/smartDetectorAlertRules/*

> [!NOTE]
> Además de tener los permisos mencionados anteriormente, su suscripción debería haberse registrado en el proveedor de recursos Microsoft.AlertsManagement. Esto es necesario para migrar correctamente las alertas de anomalías de error de Application Insights. 

## <a name="common-problems-and-remedies"></a>Problemas comunes y soluciones

Después de [desencadenar la migración](alerts-using-migration-tool.md), recibirá un correo electrónico en las direcciones que proporcionó para avisarle de que la migración se ha completado o si se requiere alguna acción de su parte. En esta sección se describen algunos problemas comunes y cómo resolverlos.

### <a name="validation-failed"></a>Error de validación

Debido a algunos cambios recientes en las reglas de alertas clásicas de la suscripción, esta no se puede migrar. Este problema es temporal. Puede reiniciar la migración una vez que el estado de migración vuelva a **Ready for migration** (Listo para la migración) en unos días.

### <a name="scope-lock-preventing-us-from-migrating-your-rules"></a>Ámbito que nos impide migrar las reglas

Como parte de la migración, se crearán nuevas alertas de métricas y nuevos grupos de acciones, y luego se eliminarán las reglas de alerta clásicas. Sin embargo, un bloqueo de ámbito puede impedir la creación o eliminación de recursos. Según el bloqueo de ámbito, no se pudieron migrar algunas reglas o ninguna de ellas. Puede resolver este problema si quita el bloqueo de ámbito de la suscripción, el grupo de recursos o el recurso que aparece en la [herramienta de migración](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel) y vuelve a desencadenar la migración. No se puede deshabilitar el bloqueo del ámbito y se debe quitar durante el proceso de migración. [Más información sobre cómo administrar los bloqueos de ámbito](../../azure-resource-manager/management/lock-resources.md#portal).

### <a name="policy-with-deny-effect-preventing-us-from-migrating-your-rules"></a>Directiva con efecto de denegación que nos impide migrar las reglas

Como parte de la migración, se crearán nuevas alertas de métricas y nuevos grupos de acciones, y luego se eliminarán las reglas de alerta clásicas. Sin embargo, una asignación de [Azure Policy](../../governance/policy/index.yml) puede impedir que se creen recursos. Según la asignación de directivas, no se pudieron migrar algunas reglas o ninguna de ellas. Las asignaciones de directiva que bloquean el proceso se enumeran en la [herramienta de migración](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel). Para resolver este problema:

- Excluya las suscripciones, los grupos de recursos o los recursos individuales durante el proceso de migración de la asignación de directivas. [Obtenga más información sobre la administración del ámbito de exclusión de las directivas](../../governance/policy/tutorials/create-and-manage.md#remove-a-non-compliant-or-denied-resource-from-the-scope-with-an-exclusion).
- Establezca el "modo de cumplimiento" en **Deshabilitado** en la asignación de directivas. [Obtenga más información sobre la propiedad enforcementMode de la asignación de directivas](../../governance/policy/concepts/assignment-structure.md#enforcement-mode).
- Establezca una exención de Azure Policy (versión preliminar) en las suscripciones, los grupos de recursos o los recursos individuales en la asignación de directivas. [Obtenga más información sobre la estructura de exención de Azure Policy](../../governance/policy/concepts/exemption-structure.md).
- Quite o cambie el efecto a "deshabilitado", "auditoría", "anexión" o "modificación" (lo que, por ejemplo, puede solucionar problemas relativos a la ausencia de etiquetas). [Obtenga más información sobre la administración de los efectos de directivas](../../governance/policy/concepts/definition-structure.md#policy-rule).

## <a name="next-steps"></a>Pasos siguientes

- [Uso de la herramienta de migración](alerts-using-migration-tool.md)
- [Preparación para la migración](alerts-prepare-migration.md)
