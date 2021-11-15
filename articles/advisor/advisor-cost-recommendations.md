---
title: Reducción de los costos de servicio con Azure Advisor
description: Utilice Azure Advisor para optimizar el costo de las implementaciones de Azure.
ms.topic: article
ms.date: 10/29/2021
ms.openlocfilehash: 32f5ca4f54eb5267abb9fe68655aa43226408610
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131443896"
---
# <a name="reduce-service-costs-by-using-azure-advisor"></a>Reducción de los costos de servicio con Azure Advisor

Azure Advisor ayuda a optimizar y reducir el gasto global de Azure mediante la identificación de recursos inactivos e infrautilizados.  Puede obtener recomendaciones sobre el costo en la pestaña **Cost** (Costo) del panel de Advisor.

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Obtención de acceso a las recomendaciones sobre el costo en Azure Advisor

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Busque y seleccione [**Advisor**](https://aka.ms/azureadvisordashboard) desde cualquier página.

1. En el panel **Advisor**, seleccione la pestaña **Costo**.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optimización del gasto en máquinas virtuales mediante la adecuación del tamaño o el apagado en instancias infrautilizadas 

Mientras que determinados escenarios de aplicaciones pueden dar lugar a un uso escaso debido al diseño, a menudo puede ahorrar dinero administrando el tamaño y número de máquinas virtuales. 

Las acciones recomendadas son apagar o cambiar de tamaño, en función de cada recurso que se vaya a evaluar.

El modelo de evaluación avanzada de Advisor considera el apagado de las máquinas virtuales cuando se cumplen todas estas condiciones: 
- El percentil 95 del valor máximo del uso de CPU es inferior al 3 %. 
- El uso de la red es inferior al 2 % en un período de siete días.
- La presión de la memoria es menor que los valores del umbral.

Advisor tiene en cuenta el cambio de tamaño de las máquinas virtuales cuando es posible ajustar la carga actual en una SKU más pequeña (dentro de la misma familia de SKU) o en un número menor de instancias, de modo que:
- La carga actual no supere el 80 % de uso para cargas de trabajo que no están dirigidas al usuario. 
- La carga no supere el 40 % para las cargas de trabajo orientadas al usuario. 

Aquí, Advisor determina el tipo de carga de trabajo mediante el análisis de las características de uso de la CPU de la carga de trabajo.

Advisor muestra el ahorro de costos estimado para cada acción recomendada: apagado o cambio de tamaño. Para cambiar el tamaño, Advisor proporciona información de la SKU actual y la de destino.

Si desea que sea más exigente en la identificación de las máquinas virtuales infrautilizadas, puede ajustar la regla de uso de la CPU para cada suscripción.

## <a name="optimize-spend-for-mariadb-mysql-and-postgresql-servers-by-right-sizing"></a>Optimización del gasto de los servidores de MariaDB, MySQL y PostgreSQL mediante el ajuste del tamaño adecuado 
Advisor analiza el uso y evalúa si los recursos de servidor de base de datos de MariaDB, MySQL o PostgreSQL han estado infrautilizados durante un período prolongado en los últimos siete días. El uso insuficiente de recursos provoca gastos no deseados que se pueden corregir sin un impacto significativo en el rendimiento. Para reducir los costos y administrar de forma eficaz los recursos, se recomienda reducir el tamaño de proceso (núcleos virtuales) a la mitad.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Reducción de los costos mediante la eliminación de circuitos ExpressRoute no aprovisionados

Advisor identifica los circuitos de Azure ExpressRoute que se encuentren en el estado de proveedor **No aprovisionado** durante más de un mes. Se recomienda eliminar el circuito si no tiene previsto aprovisionarlo con su proveedor de conectividad.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Reducción de costos mediante la eliminación o reconfiguración de puertas de enlace de red virtual inactivas

Advisor identifica las puertas de enlace de red virtual que han estado inactivas durante más de 90 días. Dado que estas puertas de enlace se facturan por hora, debe considerar volver a configurarlas, o eliminarlas si no va a utilizarlas. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Compra de instancias reservadas de máquina virtual para ahorrar dinero en los costos de pago por uso

Advisor revisa el uso de las máquinas virtuales durante los últimos 30 días y determinará si podría ahorrar dinero mediante la adquisición de una reserva de Azure. Advisor muestra las regiones y los tamaños con más posibilidades de ahorro, y el ahorro estimado de la adquisición de reservas. Con las reservas de Azure, puede adquirir previamente los costos de base de las máquinas virtuales. Se aplican descuentos automáticamente a las máquinas virtuales nuevas o existentes que tengan el mismo tamaño y la misma región que sus reservas. [Más información sobre Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Advisor también le notifica las instancias reservadas que van a expirar en los próximos 30 días. Le recomendará que compre instancias reservadas nuevas para evitar pagar el precio de pago por uso.

## <a name="buy-reserved-instances-for-several-resource-types-to-save-over-your-pay-as-you-go-costs"></a>Compre instancias reservadas para varios tipos de recursos con el fin de ahorrar en los costos de pago por uso

Advisor analiza los patrones de uso de los últimos 30 días para los siguientes recursos y recomienda las compras de capacidad reservada que optimizarán los costos.

### <a name="azure-cosmos-db-reserved-capacity"></a>Capacidad reservada de Azure Cosmos DB
Advisor analiza los patrones de uso de Azure Cosmos DB de los últimos 30 días y recomienda las compras de capacidad reservada para optimizar los costos. Con la capacidad reservada, puede comprar por adelantado un uso por horas de Azure Cosmos DB y ahorrar en los costos de pago por uso. La capacidad reservada es una ventaja de facturación que se aplicará automáticamente a las implementaciones nuevas y existentes. Para calcular las estimaciones de ahorro de las suscripciones individuales, Advisor utiliza los precios de una reserva de tres años y extrapola los patrones de uso observados en los últimos 30 días. Hay disponibles recomendaciones de compras de capacidad reservada para ámbitos compartidos que pueden aumentar el ahorro.

### <a name="sql-database-and-sql-managed-instance-reserved-capacity"></a>Capacidad reservada de SQL Database y SQL Managed Instance
Advisor analiza los patrones de uso de SQL Database y SQL Managed Instance de los últimos 30 días. A continuación, recomienda las compras de capacidad reservada que optimizan los costos. Con la capacidad reservada, puede comprar por adelantado un uso por horas de SQL Database y ahorrar en los costos de proceso de SQL. La licencia de SQL se cobra por separado y no se descuenta de la reserva. La capacidad reservada es una ventaja de facturación que se aplicará automáticamente a las implementaciones nuevas y existentes. Para calcular las estimaciones de ahorro de las suscripciones individuales, Advisor utiliza los precios de una reserva de tres años y extrapola los patrones de uso observados en los últimos 30 días. Hay disponibles recomendaciones de compras de capacidad reservada para ámbitos compartidos que pueden aumentar el ahorro. Para obtener más información, consulte [Capacidad reservada de SQL Database y SQL Managed Instance](../azure-sql/database/reserved-capacity-overview.md).

### <a name="app-service-stamp-fee-reserved-capacity"></a>Capacidad reservada del impuesto sobre el timbre de App Service
Advisor analiza el patrón de uso del impuesto sobre el timbre para el entorno aislado de Azure App Service durante los últimos 30 días y recomienda las compras de capacidad reservada que optimizan los costos. Con la capacidad reservada, puede comprar por adelantado un uso por horas del impuesto sobre el timbre para el entorno aislado y ahorrar en los costos de pago por uso. Tenga en cuenta que la capacidad reservada solo se aplica al impuesto sobre el timbre y no a las instancias de App Service. La capacidad reservada es una ventaja de facturación que se aplicará automáticamente a las implementaciones nuevas y existentes. Para calcular las estimaciones de ahorro de las suscripciones individuales, Advisor utiliza los precios de reserva de tres años y los patrones de uso de los últimos 30 días.

### <a name="blob-storage-reserved-capacity"></a>Capacidad reservada de Azure Storage
Advisor analiza el uso del almacenamiento de Azure Blob Storage y Azure Data Lake en los últimos 30 días. A continuación, calcula las compras de capacidad reservada que optimizan los costos. Con la capacidad reservada, puede comprar por adelantado un uso por horas y ahorrar en los costos de uso a petición. La capacidad reservada de almacenamiento en blobs se aplica solo a los datos almacenados en las cuentas de Azure Blob Storage de uso general v2 y de Azure Data Lake Storage Gen2. La capacidad reservada es una ventaja de facturación que se aplicará automáticamente a las implementaciones nuevas y existentes. Para calcular las estimaciones de ahorro de las suscripciones individuales, Advisor utiliza los precios de una reserva de tres años y los patrones de uso observados en los últimos 30 días. Hay disponibles recomendaciones de compras de capacidad reservada para ámbitos compartidos que pueden aumentar el ahorro.

### <a name="mariadb-mysql-and-postgresql-reserved-capacity"></a>Capacidad reservada de MariaDB, MySQL y PostgreSQL
Advisor analiza los patrones de uso de Azure Database for MariaDB, Azure Database for MySQL y Azure Database for PostgreSQL en los últimos 30 días. A continuación, recomienda las compras de capacidad reservada que optimizan los costos. Con la capacidad reservada, puede comprar por adelantado un uso por horas de MariaDB, MySQL y PostgreSQL, y ahorrar en los costos actuales. La capacidad reservada es una ventaja de facturación que se aplicará automáticamente a las implementaciones nuevas y existentes. Para calcular las estimaciones de ahorro de las suscripciones individuales, Advisor utiliza los precios de una reserva de tres años y los patrones de uso observados en los últimos 30 días. Hay disponibles recomendaciones de compras de capacidad reservada para ámbitos compartidos que pueden aumentar el ahorro.

### <a name="azure-synapse-analytics-reserved-capacity"></a>Capacidad reservada de Azure Synapse Analytics
Advisor analiza los patrones de uso de Azure Synapse Analytics de los últimos 30 días y recomienda las compras de capacidad reservada para optimizar los costos. Con la capacidad reservada, puede comprar por adelantado un uso por horas de Synapse Analytics y ahorrar en los costos de uso a petición. La capacidad reservada es una ventaja de facturación que se aplicará automáticamente a las implementaciones nuevas y existentes. Para calcular las estimaciones de ahorro de las suscripciones individuales, Advisor utiliza los precios de una reserva de tres años y los patrones de uso observados en los últimos 30 días. Hay disponibles recomendaciones de compras de capacidad reservada para ámbitos compartidos que pueden aumentar el ahorro.

## <a name="delete-unassociated-public-ip-addresses-to-save-money"></a>Eliminación de direcciones IP públicas no asociadas para ahorrar dinero

Advisor identifica las direcciones IP públicas que no están asociadas a recursos de Azure, como equilibradores de carga y máquinas virtuales. Una carga nominal está asociada a estas direcciones IP públicas. Si no tiene previsto usarlas, puede eliminarlas para ahorrar dinero.

## <a name="delete-azure-data-factory-pipelines-that-are-failing"></a>Eliminación de las canalizaciones de Azure Data Factory que dan error

Advisor detecta canalizaciones de Azure Data Factory que producen errores repetidamente. Recomienda resolver los problemas o eliminar las canalizaciones si no las necesita. Se le factura por estas canalizaciones aunque no le presten servicio mientras se producen los errores.

## <a name="use-standard-snapshots-for-managed-disks"></a>Uso de instantáneas estándar para discos administrados
Para ahorrar el 60 % del costo, se recomienda almacenar las instantáneas en una cuenta de almacenamiento estándar, independientemente del tipo de almacenamiento del disco principal. Esta opción es la predeterminada para las instantáneas de discos administrados. Advisor identifica las instantáneas que se almacenan en una cuenta de almacenamiento prémium y recomienda la migración de prémium a estándar. [Más información sobre los precios de discos administrados](https://aka.ms/aa_manageddisksnapshot_learnmore)

## <a name="use-lifecycle-management"></a>Uso de la administración del ciclo de vida
Al usar la inteligencia sobre el recuento de objetos de Azure Blob Storage, el tamaño total y las transacciones, Advisor detecta si debe habilitar la administración del ciclo de vida para almacenar los datos en el nivel de una o varias cuentas de almacenamiento. Se le pide que cree reglas de administración del ciclo de vida para organizar automáticamente los datos en un nivel de acceso esporádico o el de archivo para optimizar los costos de almacenamiento, al tiempo que conserva los datos en Azure Blob Storage para que sean compatibles con las aplicaciones.

## <a name="create-an-ephemeral-os-disk-recommendation"></a>Creación de una recomendación de disco del sistema operativo efímero
[El disco del sistema operativo efímero](../virtual-machines/ephemeral-os-disks.md) le permite: 
- Ahorrar en los costos de almacenamiento de los discos del sistema operativo. 
- Reducir la latencia de lectura y escritura en los discos del sistema operativo. 
- Realizar operaciones de restablecimiento de la imagen inicial de la máquina virtual más rápidas mediante el restablecimiento del sistema operativo (y del disco temporal) a su estado original.

Es preferible usar el disco del sistema operativo efímero para máquinas virtuales IaaS de corta duración o máquinas virtuales con cargas de trabajo sin estado. Advisor proporciona recomendaciones para los recursos que pueden beneficiarse del disco del sistema operativo efímero.

## <a name="reduce-azure-data-explorer-table-cache-period-policy-for-cluster-cost-optimization-preview"></a>(VERSIÓN PRELIMINAR) Reducción del período de caché (directiva) de la tabla de Azure Data Explorer para la optimización del coste de clúster (versión preliminar)
Advisor identifica los recursos en los que reducir la directiva de caché de la tabla liberará los nodos de clúster de Azure Data Explorer que tienen un bajo uso CPU, memoria y una configuración de tamaño de caché alto.

## <a name="configure-manual-throughput-instead-of-autoscale-on-your-azure-cosmos-db-database-or-container"></a>Configuración del rendimiento manual en lugar de la escalabilidad automática en la base de datos o el contenedor de Azure Cosmos DB
En función del uso de los últimos 7 días, la utilización del rendimiento manual antes que la escalabilidad automática puede suponerle ahorros en los costos. El rendimiento manual es más rentable cuando el uso medio del rendimiento máximo (RU/s) es mayor que un 66 % o menor o igual que un 10 %. La cantidad de ahorro en costes representa un ahorro potencial derivado del uso del rendimiento manual recomendado, en función de la utilización en los últimos 7 días. El ahorro real puede variar en función del rendimiento manual establecido y de si el uso medio del rendimiento sigue siendo similar al del período de tiempo analizado. El ahorro estimado no tiene en cuenta ningún descuento que se pueda aplicar a su cuenta.

## <a name="next-steps"></a>Pasos siguientes

Para aprender más sobre las recomendaciones de Advisor, consulte:
* [Introducción a Advisor](advisor-overview.md)
* [Puntuación de Advisor](azure-advisor-score.md)
* [Introducción a Advisor](advisor-get-started.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre alta disponibilidad de Advisor](advisor-high-availability-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
* [Recomendaciones de excelencia operativa de Advisor](advisor-operational-excellence-recommendations.md)
