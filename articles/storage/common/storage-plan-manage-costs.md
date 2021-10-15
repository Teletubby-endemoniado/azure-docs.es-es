---
title: Planeamiento y administración de costos de Azure Blob Storage
description: Aprenda a planear y administrar los costos de Azure Blob Storage mediante el análisis de costos en Azure Portal.
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 06/21/2021
ms.author: normesta
ms.subservice: common
ms.custom: subject-cost-optimization
ms.openlocfilehash: 51335fe48208423883706796fe4812ec0e0af5fe
ms.sourcegitcommit: 613789059b275cfae44f2a983906cca06a8706ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2021
ms.locfileid: "129275722"
---
# <a name="plan-and-manage-costs-for-azure-blob-storage"></a>Planeamiento y administración de costos de Azure Blob Storage

Este artículo le ayuda a planear y administrar los costos de Azure Blob Storage. En primer lugar, use la calculadora de precios de Azure para calcular los costos. Tras crear una cuenta de almacenamiento, optimícela para pagar solo lo que necesite. Use las características de la administración de costos para crear presupuestos y supervisar costos. También puede revisar los costos previstos y supervisar las tendencias de gasto para identificar las áreas en las que podría querer actuar.

Tenga en cuenta que los costos de Blob Storage son solo una parte de los costos mensuales de la factura de Azure. Aunque en este artículo se explica cómo calcular y administrar los costos de Blob Storage, se le facturarán todos los servicios y recursos de Azure que use para su suscripción de Azure, incluidos los servicios de terceros. Una vez que esté familiarizado con la administración de los costos de Blob Storage, puede aplicar métodos similares para administrar los costos de todos los servicios de Azure que se usan en su suscripción.

## <a name="estimate-costs"></a>cálculo de los costos

Use la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/) para calcular los costos antes de crear y comenzar a transferir datos a una cuenta de Azure Storage.

1. En la página [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/), elija el icono **Cuentas de almacenamiento**.

2. Desplácese hacia abajo en la página y busque la sección **Cuentas de almacenamiento** de la estimación.

3. Elija las opciones de las listas desplegables.

   A medida que se modifica el valor de estas listas desplegables, cambia el costo estimado. Esa estimación aparece en la esquina superior, así como en la parte inferior de la estimación.

   ![Captura de pantalla que muestra su cotización](media/storage-plan-manage-costs/price-calculator-storage-type.png)

   A medida que cambia el valor de la lista desplegable **Tipo**, también cambian otras opciones que aparecen en esta hoja de cálculo. Use los vínculos de la sección **Más información** para obtener más información sobre el significado de cada opción y cómo afectan al precio de las operaciones relacionadas con el almacenamiento.

4. Modifique las demás opciones cómo afectan a la estimación.

## <a name="understand-the-full-billing-model-for-azure-blob-storage"></a>Descripción del modelo de facturación completo de Azure Blob Storage

Azure Blob Storage se ejecuta en la infraestructura de Azure, que genera costos al implementar recursos nuevos. Es importante entender que podrían generarse otros costos de infraestructura adicionales.

### <a name="how-youre-charged-for-azure-blob-storage"></a>Cómo se le cobra por Azure Blob Storage

Al crear o usar recursos de Azure Blob Storage, se le cobrará por los siguientes medidores:

| Medidor | Unidad |
|---|---|
| Almacenamiento de datos | Por GB/mes|
| Operaciones | Por transacción |
| Transferencia de datos | Por GB |
| Metadatos | Por GB/mes<sup>1 |
| Etiquetas de índice de blobs | Por etiqueta<sup>2  |
| Fuente de cambios | Por cambio registrado<sup>2 |
| Ámbitos de cifrado | Por mes<sup>2 |
| Aceleración de consultas | Por GB analizados y por GB devueltos |

<sup>1</sup> Solo se aplica a las cuentas que tienen un espacio de nombres jerárquico.<br />
<sup>2</sup> Solo se aplica si habilita la característica.<br />

El tráfico de datos también puede incurrir en costos de red. Consulte los [precios de ancho de banda](https://azure.microsoft.com/pricing/details/data-transfers/).

Al final del ciclo de facturación, se suman los cargos de cada medidor. La factura muestra una sección para todos los costos de Azure Blob Storage. Hay un elemento de línea independiente para cada medidor.

El almacenamiento de datos y los metadatos se facturan por GB al mes. Puede calcular cuánto afectan a la factura mensual los datos y metadatos almacenados durante menos de un mes calculando el costo de cada GB al día. Puede usar un enfoque similar para calcular el costo de los ámbitos de cifrado que se usan menos de un mes. El número de días varía según el mes. Por lo tanto, para obtener la mejor aproximación de los costos de un mes determinado, asegúrese de dividir el costo mensual por el número de días de ese mes.

### <a name="finding-the-unit-price-for-each-meter"></a>Búsqueda del precio unitario de cada medidor

Para averiguar los precios unitarios, abra la página de precios correcta. Si ha habilitado la característica de espacio de nombres jerárquico en su cuenta, consulte la página de [precios de Azure Data Lake Storage Gen2](https://azure.microsoft.com/pricing/details/storage/data-lake/). Si no ha habilitado esta característica, consulte la página de [precios de los blobs en bloques](https://azure.microsoft.com/pricing/details/storage/blobs/).

En la página de precios, aplique los filtros de redundancia, región y moneda adecuados. Los precios de cada medidor aparecen en una tabla. Los precios difieren en función de otras configuraciones de la cuenta, como las opciones de redundancia de datos, el nivel de acceso y el nivel de rendimiento.

### <a name="flat-namespace-accounts-and-transaction-pricing"></a>Cuentas de espacio de nombres plano y precios de transacciones

Los clientes pueden realizar una solicitud mediante el punto de conexión de Blob Storage o el punto de conexión de Data Lake Storage de la cuenta. Para más información sobre los puntos de conexión de las cuentas de almacenamiento, consulte [Puntos de conexión de cuenta de almacenamiento](storage-account-overview.md#storage-account-endpoints).

Los precios de las transacciones que aparecen en la página de [precios de los blobs en bloques](https://azure.microsoft.com/pricing/details/storage/blobs/) solo se aplican a las solicitudes que usan el punto de conexión de Blob Storage (por ejemplo, `https://<storage-account>.blob.core.windows.net`). Los precios indicados no se aplican a las solicitudes que usan el punto de conexión de Data Lake Storage Gen2 (por ejemplo, `https://<storage-account>.dfs.core.windows.net`). Para conocer el precio de transacción de esas solicitudes, abra la página de [precios de Azure Data Lake Storage Gen2](https://azure.microsoft.com/pricing/details/storage/data-lake/) y seleccione la opción **Espacio de nombres plano**.

> [!div class="mx-imgBorder"]
> ![Opción de espacio de nombres plano](media/storage-plan-manage-costs/select-flat-namespace.png)

Las solicitudes al punto de conexión de Data Lake Storage Gen2 pueden originarse en cualquiera de los siguientes orígenes:

- Cargas de trabajo que usan el controlador Azure Blob File System o el [controlador ABFS](https://hadoop.apache.org/docs/stable/hadoop-azure/abfs.html).

- Llamadas REST que usan la [API REST de Azure Data Lake Store](/rest/api/storageservices/data-lake-storage-gen2).

- Aplicaciones que usan las API de Data Lake Storage Gen2 desde una biblioteca cliente de Azure Storage.

### <a name="using-azure-prepayment-with-azure-blob-storage"></a>Uso del pago por adelantado de Azure con Azure Blob Storage

Puede pagar los cargos de Azure Blob Storage con el crédito del pago por adelantado de Azure (antes conocido como compromiso monetario). Sin embargo, no puede usar el crédito del pago por adelantado de Azure para pagar los gastos de productos y servicios de terceros, incluidos los que proceden de Azure Marketplace.

## <a name="optimize-costs"></a>Optimización de costos

Considere la posibilidad de usar estas opciones para reducir costos.

- Reservar capacidad de almacenamiento.

- Organizar los datos en capas de almacenamiento.

- Mover automáticamente los datos entre las capas de almacenamiento.

En esta sección se tratan todas las opciones de forma más detallada.

#### <a name="reserve-storage-capacity"></a>Reserva de la capacidad de almacenamiento

Puede ahorrar dinero en costos de almacenamiento de datos de blobs con la capacidad reservada de Azure Storage. La capacidad reservada de Azure Storage ofrece un descuento en la capacidad para los blobs en bloques y los datos de Azure Data Lake Storage Gen2 en las cuentas de almacenamiento estándar cuando se compromete a una reserva durante un año o tres años. Una reserva proporciona una cantidad fija de capacidad de almacenamiento para el plazo de la reserva. La capacidad reservada de Azure Storage puede disminuir considerablemente los costos de capacidad de los blobs en bloques y los datos de Azure Data Lake Storage Gen2.

Para más información, consulte [Optimización de los costos de almacenamiento de blobs con capacidad reservada](../blobs/storage-blob-reserved-capacity.md).

#### <a name="organize-data-into-access-tiers"></a>Organización de datos en capas de almacenamiento

Para reducir los costos puede colocar los datos del blob en las capas de almacenamiento más económicas. Elija una de las tres capas que están diseñadas para optimizar los costos del uso de datos. Por ejemplo, el nivel de *acceso frecuente* tiene un mayor costo de almacenamiento, pero un menor costo de acceso. Por consiguiente, si planea acceder a los datos con frecuencia, es posible que este nivel sea la opción más económica. Si planea acceder a los datos con poca frecuencia, tendría más sentido elegir los niveles de acceso *esporádico* o *de archivo*, ya que su costo de acceso a los datos es mayor, pero se reduce el costo de almacenamiento.

Para más información, consulte [Niveles de acceso frecuente, esporádico y de archivo de los datos de blob](../blobs/access-tiers-overview.md?tabs=azure-portal).

#### <a name="automatically-move-data-between-access-tiers"></a>Movimiento automático de datos entre las capas de almacenamiento

Use las directivas de administración del ciclo de vida para mover periódicamente datos entre las distintas capas para ahorrar el mayor dinero posible. Para mover datos, estas directivas usan las reglas que especifique. Por ejemplo, puede crear una regla que mueva blobs al nivel de almacenamiento de archivo si el blob no se ha modificado en 90 días. La creación de directivas que ajusten el nivel de acceso de los datos le permite diseñar las opciones de almacenamiento más baratas que se ajusten a sus necesidades.

Para más información, consulte [Optimización de los costos mediante la administración automática del ciclo de vida de los datos](../blobs/lifecycle-management-overview.md?tabs=azure-portal).

## <a name="create-budgets"></a>Creación de presupuestos

Puede crear [presupuestos](../../cost-management-billing/costs/tutorial-acm-create-budgets.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) para administrar los costos y crear alertas que envíen notificaciones automáticamente a las partes interesadas sobre anomalías en los gastos y riesgos de gastos adicionales. Las alertas se basan en el gasto comparado con los umbrales de presupuesto y costo. Los presupuestos y las alertas se crean para las suscripciones y los grupos de recursos de Azure, por lo que son útiles como parte de una estrategia general de supervisión de costos. Sin embargo, pueden tener una funcionalidad limitada para administrar los costos de servicios individuales de Azure, como el costo de Azure Storage, porque están diseñados para realizar un seguimiento de los costos en un nivel más alto.

## <a name="monitor-costs"></a>Supervisión de costos

A medida que se usan recursos con Azure Storage, se incurre en costos. Los costos de la unidad de uso de recursos varían en función de intervalos de tiempo (segundos, minutos, horas y días) o en función del uso de unidades (bytes, megabytes, etc.). Tan pronto como empieza el uso de Azure Storage, se incurre en costos. Puede ver los costos en el panel [Análisis de costos](../../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) de Azure Portal.

Al usar el análisis de costos, puede ver los costos de Azure Storage en gráficos y tablas para diferentes intervalos de tiempo. Algunos ejemplos son: por día, mes actual y anterior y año. También puede ver los costos comparados con los presupuestos y los costos previstos. Cambiar a vistas más largas en el tiempo puede ayudarle a identificar las tendencias de gasto y ver dónde podría haber ocurrido un gasto excesivo. Si ha creado presupuestos, también podrá ver fácilmente dónde se han excedido.

> [!NOTE]
> El análisis de costos es compatible con varios tipos de cuenta de Azure. Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](../../cost-management-billing/costs/understand-cost-mgt-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) (Información sobre los datos de Cost Management). Para ver los datos de costos, se necesita al menos acceso de lectura en la cuenta de Azure. Para más información acerca de cómo asignar acceso a los datos de Azure Cost Management, consulte [Asignación de acceso a los datos](../../cost-management-billing/costs/assign-access-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).

Para ver los costos de Azure Storage en los análisis de costos:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).

2. Abra la ventana **Administración de costos y facturación**, seleccione **Administración de costos** en el menú y, a continuación, seleccione **Análisis de costos**. A continuación, puede cambiar el ámbito a una suscripción específica en la lista desplegable **Ámbito**.

   ![Captura de pantalla que muestra el ámbito](./media/storage-plan-manage-costs/cost-analysis-pane.png)

4. Para ver los costos de Azure Storage, seleccione **Agregar filtro** y, a continuación, seleccione **Nombre del servicio**. Después, elija **almacenamiento** en la lista.

   Este es un ejemplo que muestra solo los costos de Azure Storage:

   ![Captura de pantalla que muestra el filtrado por almacenamiento](./media/storage-plan-manage-costs/cost-analysis-pane-storage.png)

En el ejemplo anterior, hemos visto el costo actual del servicio. También se muestran los costos por regiones de Azure (ubicaciones) y por grupo de recursos. También puede agregar otros filtros (por ejemplo, un filtro para ver los costos de cuentas de almacenamiento concretas).

## <a name="export-cost-data"></a>Exportación de datos de costos

También puede [exportar los datos de costos](../../cost-management-billing/costs/tutorial-export-acm-data.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn) a una cuenta de almacenamiento. Esto resulta útil cuando usted u otro usuario necesita hacer un análisis de datos adicional para los costos. Por ejemplo, un equipo de finanzas puede analizar los datos con Excel o Power BI. Puede exportar los costos en una programación diaria, semanal o mensual y establecer un intervalo de fechas personalizado. La exportación de los datos de costos es la forma recomendada de recuperar conjuntos de datos de costos.

## <a name="faq"></a>Preguntas más frecuentes

**Si uso Azure Storage solo unos días al mes, ¿se prorratea el costo?**

La capacidad de almacenamiento se factura en unidades de la cantidad media diaria de datos almacenados, en gigabytes (GB), durante un periodo mensual. Por ejemplo, si ha utilizado de forma constante 10 GB de almacenamiento durante la primera mitad del mes y nada durante la segunda mitad, se le facturará un uso medio de 5 GB de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre cómo funcionan los precios con Azure Storage. Consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
- [Optimización de los costos de almacenamiento de blobs con capacidad reservada](../blobs/storage-blob-reserved-capacity.md).
- Aprenda a [optimizar su inversión en la nube con Azure Cost Management](../../cost-management-billing/costs/cost-mgt-best-practices.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Obtenga más información sobre la administración de costos con los [análisis de costos](../../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Obtenga información sobre cómo [evitar los costos inesperados](../../cost-management-billing/cost-management-billing-overview.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
- Haga el curso de aprendizaje guiado sobre [Cost Management](/learn/paths/control-spending-manage-bills?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn).
