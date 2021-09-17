---
title: Facturas de inscripciones de Azure Enterprise
description: En este artículo se explica cómo administrar y actuar en su factura de Azure Enterprise.
author: bandersmsft
ms.author: banders
ms.date: 08/19/2021
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: enterprise
ms.reviewer: ruturajd
ms.custom: contperf-fy21q1
ms.openlocfilehash: 77f463d34ebd05e39876933d65631371acff1707
ms.sourcegitcommit: d43193fce3838215b19a54e06a4c0db3eda65d45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2021
ms.locfileid: "122515722"
---
# <a name="azure-enterprise-enrollment-invoices"></a>Facturas de inscripciones de Azure Enterprise

En este artículo se explica cómo administrar y actuar en su factura de Contrato Enterprise de Azure (Azure EA). La factura es una representación del recibo. Revise que esté correcta. También debe familiarizarse con otras tareas que podrían ser necesarias para administrar la factura.

## <a name="view-usage-summary-and-download-reports"></a>Visualización de resumen de uso e informes de las descargas

Los administradores de empresa pueden ver un resumen de sus datos de uso, el pago por adelantado de Azure consumido y los cargos asociados con el uso adicional en Azure Enterprise Portal. Los cargos se presentan en el nivel de resumen en todas las cuentas y suscripciones.

Para ver el uso detallado para cuentas específicas, descargue el informe de detalles de uso:

1. Inicie sesión en Azure Enterprise Portal.
1. Seleccione **Informes**.
1. Seleccione la pestaña **Descargar uso**.
1. En la lista de informes, seleccione **Descargar** en el informe mensual que desee obtener.

   > [!NOTE]
   > El informe de detalles de uso no incluye ningún impuesto aplicable.
   >
   > Puede haber una latencia de hasta ocho horas desde el momento en que se realizó el uso hasta el momento en que se refleja en el informe.

Para ver los informes y gráficos del resumen de uso:

1. Inicie sesión en Azure Enterprise Portal.
1. Seleccione un plazo de pago por adelantado.
   Para cambiar el rango de fechas de **Resumen de uso**, puede alternar entre **M** (mensual) y **C** (personalizado) en la parte superior derecha de la página y, a continuación, especificar fechas de inicio y de finalización personalizadas.  
   ![Crear y ver el resumen de uso y descargar informes en la vista personalizada](./media/ea-portal-enrollment-invoices/create-ea-view-usage-summary-and-download-reports-custom-view.png)
1. Para ver detalles adicionales, puede seleccionar un periodo o un mes en el gráfico.
   - El gráfico muestra el uso mes a mes con un desglose del uso realizado, el cargo extra por los servicios, los cargos facturados por separado y los cargos por Azure Marketplace.
   - En el mes seleccionado, puede usar los campos debajo del gráfico para filtrar por departamentos, cuentas y suscripciones.
   - Puede alternar entre **Charge by Services** (Cargo por servicios) y **Charge by Hierarchy** (Cargo por jerarquía).
   - Expanda las secciones pertinentes para ver los detalles de **Servicio de Azure**, **Gastos facturados por separado** y **Azure Marketplace**.

Consulte este vídeo para conocer cómo ver el uso:

> [!VIDEO https://www.youtube.com/embed/Cv2IZ9QCn9E]

### <a name="download-csv-reports"></a>Descarga de informes CSV

Los administradores de empresa usan la página de descarga de informes mensuales para descargar los informes siguientes como archivos CSV:

- Saldo y cargo
- Detalles de uso
- Cargos de Azure Marketplace
- Hoja de precios

Para descargar informes:

1. En Azure Enterprise Portal, seleccione **Informes**.
2. Seleccione **Descargar uso** en la parte superior de la página.
3. Seleccione **Download** (Descargar) junto al informe del mes.

   > [!NOTE]
   > Puede haber una latencia de hasta 72 horas entre la fecha en que se realizó el uso y el momento en que el uso se muestra en los informes.
   >
   > Los usuarios que descarguen archivos CSV con Safari en Excel pueden encontrarse con errores de formato. Para evitar errores, abra el archivo con un editor de texto.

![Ejemplo que muestra la página Descargar uso](./media/ea-portal-enrollment-invoices/create-ea-download-csv-reports.png)

Vea este vídeo para ver cómo descargar la información de uso:

> [!VIDEO https://www.youtube.com/embed/eY797htT1qg]

### <a name="advanced-report-download"></a>Descarga de informes avanzada

Puede usar la descarga de informes avanzados para obtener informes que cubran cuentas o rangos de fechas específicos. El archivo de salida está en formato CSV para admitir grandes conjuntos de registros.

1. En Azure Enterprise Portal, seleccione **Descarga de informes avanzada**.
1. Seleccione un rango de fechas adecuado y las cuentas adecuadas.
1. Seleccione **Request Usage Data** (Solicitar datos de uso).
1. Seleccione el botón **Actualizar** hasta que el estado del informe se actualice a **Descargar**.
1. Descargue el informe.

### <a name="download-usage-reports-and-billing-information-for-a-prior-enrollment"></a>Descargar informes de uso e información de facturación de una inscripción anterior

Puede descargar los informes de uso y la información de facturación para una inscripción anterior después de que haya tenido lugar la transferencia de una inscripción. Los informes históricos están disponibles en Azure Enterprise Portal y en Cost Management.

Azure Enterprise Portal filtra las inscripciones inactivas para excluirlas. Tendrá que desactivar la casilla **Activa** para ver las inscripciones transferidas inactivas.  

![Al desactivar la casilla activa, el usuario puede ver las inscripciones inactivas.](./media/ea-portal-enrollment-invoices/unchecked-active-box.png)

## <a name="change-a-po-number-for-an-upcoming-overage-invoice"></a>Cambio de un número de pedido de compra en una próxima factura de uso por encima del límite

Azure Enterprise Portal genera automáticamente un número de pedido de compra predeterminado, a menos que el administrador de empresa defina uno antes de la fecha de la factura. Un administrador de empresa puede actualizar el número de pedido de compra hasta siete días después de recibir un correo electrónico de notificación de factura automática. 

Para evitar la generación automática de números de pedido todos los meses, puede bloquear el número de pedido de compra. Consulte [Bloqueo del número de pedido de compra](#lock-po-number-to-prevent-automatic-update-in-upcoming-billing-cycles).

### <a name="update-the-azure-services-purchase-order-number"></a>Actualización del número de pedido de compra de los servicios de Azure

1. En Azure Enterprise Portal, seleccione **Informes** > **Resumen de uso**.
1. Seleccione **Edit PO Numbers** (Editar números de pedidos de compra) en la esquina superior derecha.
1. Seleccione el botón de radio **Azure Services** (Servicios de Azure).
1. Seleccione un **período de facturación** en el menú desplegable de intervalos de fechas.
   Puede editar un número de pedido de compra durante un período de siete días después de recibir una notificación de factura, pero antes de haber pagado la factura.
1. Escriba un número de pedido de compra nuevo en el campo **PO Number** (Número de pedido de compra).
1. Seleccione **Guardar** para enviar el cambio.

### <a name="update-the-azure-marketplace-purchase-order-number"></a>Actualización del número de pedido de compra de Azure Marketplace

1. En Azure Enterprise Portal, seleccione **Informes** > **Resumen de uso**.
1. Seleccione **Edit PO Numbers** (Editar números de pedidos de compra) en la esquina superior derecha.
1. Seleccione el botón de radio **Marketplace**.
1. Seleccione un **período de facturación** en el menú desplegable de intervalos de fechas.  
    Puede editar un número de pedido de compra durante un período de siete días después de recibir una notificación de factura, pero antes de haber pagado la factura.
1. Escriba un número de pedido de compra nuevo en el campo **PO Number** (Número de pedido de compra).
1. Seleccione **Guardar** para enviar el cambio.

### <a name="lock-po-number-to-prevent-automatic-update-in-upcoming-billing-cycles"></a>Bloqueo del número de pedido de compra para evitar la actualización automática en los próximos ciclos de facturación

Después de bloquear el número de pedido de compra, este permanece bloqueado para todas las facturas nuevas y no es necesario actualizarlo.

1.  En Azure Enterprise Portal, seleccione **Informe** > **Resumen de uso**.
2.  Seleccione **Edit PO Numbers** (Editar números de pedidos de compra) en la esquina superior derecha.
3.  Escriba un número de pedido de compra nuevo en el campo **Número de pedido de compra**.
4.  Seleccione el cuadro **Lock PO number** (Número de pedido de compra).
5.  Seleccione **Guardar** para enviar el cambio.  
    :::image type="content" source="./media/ea-portal-enrollment-invoices/lock-po.png" alt-text="Captura de pantalla que muestra el cuadro View/Edit PO Numbers (Ver o editar números de pedido de compra)." lightbox="./media/ea-portal-enrollment-invoices/lock-po.png" :::


## <a name="azure-enterprise-billing-frequency"></a>Frecuencia de facturación de Azure Enterprise

Microsoft factura anualmente en la fecha de entrada en vigor de la inscripción las compras de prepago de los servicios de Microsoft Azure. Microsoft factura a período vencido el uso que supere las cantidades de prepago.

- Las tarifas de prepago se ofrecen en función de una tarifa mensual y se facturan anualmente por adelantado.
- Las tarifas de uso por encima del límite se calculan cada mes y se facturan a período vencido al final del período de facturación.

### <a name="billing-intervals"></a>Intervalos de facturación

El intervalo de facturación depende de cómo decida hacer sus compras de prepago. El prepago anual es coincide con una de las siguientes fechas:

- La fecha de aniversario de la inscripción
- La fecha efectiva de la suscripción de modificación de un año.

La fecha en que reciba la factura de uso por encima del límite depende de la fecha de inicio de su inscripción y de su configuración:

- **Inscripciones directas con una fecha de inicio anterior al 1 de mayo de 2018**:
  - En el caso de un Contrato Enterprise (EA) directo, se encuentra en un período de facturación anual para los servicios de Azure, excepto los servicios de Azure Marketplace. El período de facturación se basa en la fecha de aniversario, la fecha en la que el contrato entró en vigor.
  - Si supera el 150 % del umbral de su prepago de Azure de EA, se convertirá automáticamente en un período de facturación trimestral basado en la fecha de su aniversario. También recibirá una factura por uso por encima del límite de los servicios de Azure.
  - Si no supera el 150 % del umbral del prepago de Azure, la inscripción permanecerá en un período de facturación anual. La factura de uso por encima del límite se recibirá al final del año del prepago.

- **Inscripciones directas con una fecha de inicio posterior al 1 de mayo de 2018**:
  - Las facturas de consumo y cargos de Azure facturados por separado tendrán un período de facturación mensual.
  - Los cargos que no cubra el prepago de Azure se deben como pago de uso por encima del límite.  

- **Inscripciones indirectas con una inscripción iniciada antes del 1 de mayo de 2018**:

  Si es cliente de un Contrato Enterprise (EA) indirecto con una fecha de inicio anterior al 1 de mayo de 2018, se encuentra en un período de facturación trimestral. El partner de canal (CP) le factura directamente.  

- **Inscripciones indirectas con una fecha de inicio posterior al 1 de mayo de 2018**:

  Se encuentra en un período de facturación mensual.  

### <a name="increase-your-azure-prepayment"></a>Aumento del prepago de Azure

Puede aumentar su prepago en cualquier momento. Se le facturará el número de meses restantes en el período de prepago de ese año. Por ejemplo, si se registra en una suscripción de enmienda de un año y aumenta el prepago durante el sexto mes, se le facturarán los seis meses restantes de ese período. Las cantidades de prepago se actualizarán durante los últimos seis meses del período de prepago. Estas nuevas cantidades se usarán para determinar los cargos por uso por encima del límite.

### <a name="overage"></a>Superávit

En el caso del uso por encima del límite, se le facturará el uso o las reservas que superen su prepago durante el período de facturación. Para ver un desglose del cálculo de las cantidades de uso por encima del límite de elementos individuales, consulte el informe de resumen de uso o póngase en contacto con su partner de canal.

Para cada elemento de la factura, verá:

- **Extended Amount** (Importe extendido): El total de cargos.
- **Prepayment Usage** (Uso del prepago): el importe del prepago usado para cubrir los cargos.
- **Net Amount** (Importe neto): los cargos que superan el prepago.

Los impuestos correspondientes se calculan solo sobre el importe neto que supera el prepago.

La facturación de uso por encima del límite está automatizada. Los plazos para las notificaciones y las facturas dependen de la fecha de finalización del período de facturación.

- Normalmente, la notificación de uso por encima del límite se envía siete días después de la fecha de finalización de la facturación.
- Las facturas de uso por encima del límite se envían de siete a nueve días después de la notificación.
- Puede revisar los cargos y actualizar los números de pedido de compra generados por el sistema durante los siete días entre la notificación de uso por encima del límite y la facturación.

### <a name="azure-marketplace"></a>Azure Marketplace

A partir del período de facturación de abril de 2019, los clientes comenzaron a recibir una sola factura de Azure, que combina todos los cargos de Azure y Azure Marketplace en una sola factura, en lugar de dos facturas independientes. Este cambio no afecta a los clientes de Australia, Japón ni Singapur.

Durante la transición a una factura combinada, recibirá una factura parcial de Azure Marketplace. Esta factura final independiente mostrará los cargos de Azure Marketplace antes de la fecha de la actualización de la facturación. Los cargos de Azure Marketplace en los que incurra después de esa fecha aparecerán en la factura de Azure. Después del período de transición, verá todos los cargos de Azure y Azure Marketplace en la factura combinada.  

### <a name="adjust-billing-frequency"></a>Ajuste de la frecuencia de facturación

La frecuencia de facturación de un cliente es anual, trimestral o mensual. El período de facturación se determina cuando un cliente firma su contrato. La facturación mensual es el intervalo de facturación más pequeño.

- Es necesaria la **aprobación** del administrador de empresa para cambiar un período de facturación de anual a trimestral para las inscripciones directas. Es necesaria la aprobación de un administrador del partner para las inscripciones indirectas. El cambio se hace efectivo al final del trimestre de facturación actual.
- Es necesaria una **modificación** para cambiar un período de facturación de anual o trimestral a mensual.  Cualquier cambio en el período de facturación de una inscripción existente requiere la aprobación de un administrador de empresa o del usuario identificado como "contacto de facturación".
- **Envíe** la aprobación a [soporte técnico de Azure Enterprise Portal](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c). Seleccione la categoría del problema: **Billing and Invoicing** (Facturación).

El cambio se hace efectivo al final del trimestre de facturación actual.

Si se firma una enmienda M503, puede pasar cualquier contrato de cualquier frecuencia a la facturación mensual. 

### <a name="request-an-invoice-copy"></a>Solicitud de una copia de la factura

Para solicitar una copia de la factura, póngase en contacto con su asociado.

## <a name="credits-and-adjustments"></a>Créditos y ajustes

Puede ver todos los créditos o ajustes aplicados a su inscripción en la sección **Informes** de [Azure Enterprise Portal](https://ea.azure.com).

Para ver los créditos:

1. En [Azure Enterprise Portal](https://ea.azure.com), seleccione la sección **Informes**.
1. Seleccione **Resumen de uso**.
1. En la esquina superior derecha, cambie de la vista **M** a la **C**.
1. Amplíe el campo de ajustes de la tabla de prepago de servicio de Azure.
1. Verá los créditos aplicados a su inscripción y una explicación breve. Por ejemplo: Crédito de Acuerdo de Nivel de Servicio.

## <a name="pay-your-overage-with-your-azure-prepayment"></a>Pago del uso por encima del límite con el prepago de Azure

Para aplicar el prepago de Azure a los usos por encima del límite, debe cumplir los siguientes criterios:

- Haber incurrido en cargos de uso por encima del límite que no se han pagado y están en el plazo de un año de la fecha de finalización del servicio facturado.
- El importe del prepago de Azure disponible cubre el número total de cargos incurridos, incluidas todas las facturas de Azure pasadas no pagadas.
- El plazo de facturación que quiere completar debe estar cerrado por completo. La facturación se cierra por completo después del quinto día de cada mes.
- El período de facturación que quiere compensar debe estar cerrado por completo.
- El descuento del prepago de Azure (APD) se basa en el nuevo prepago real menos los fondos planeados para el consumo anterior. Este requisito solo se aplica a los cargos incurridos por uso por encima del límite. Esto solo es válido para los servicios que consumen el prepago de Azure, así que no se aplica a los cargos de Azure Marketplace. Los cargos de Azure Marketplace se facturan por separado.

Para completar una compensación de uso por encima del límite, usted o el equipo de cuentas pueden abrir una solicitud de soporte técnico. Se requiere una aprobación por correo electrónico del administrador de empresa o del contacto de facturación.

## <a name="move-charges-to-another-enrollment"></a>Traslado de cargos a otra inscripción

Los datos de uso solo se trasladan cuando se atrasa una transferencia. Hay dos opciones para trasladar los datos de uso de una inscripción a otra:

- Transferencia de cuentas desde una inscripción a otra
- Transferencias de inscripciones de una inscripción a otra

Para cualquiera de las opciones, debe enviar [una solicitud de soporte técnico](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) al equipo de soporte técnico del Contrato Enterprise para obtener ayuda. 

## <a name="enterprise-agreement-usage-calculations"></a>Cálculos de uso de Contratos Enterprise

Consulte [Servicios de Azure](https://azure.microsoft.com/services/) y [Precios de Azure](https://azure.microsoft.com/pricing/) para obtener información pública básica sobre precios, unidades de medida, preguntas frecuentes y directrices de informes de uso para cada servicio individual. Puede encontrar más información sobre los cálculos de EA en las secciones a continuación.

### <a name="enterprise-agreement-units-of-measure"></a>Unidades de medida de Contratos Enterprise

Las unidades de medida de los Contratos Enterprise suelen ser diferentes de las que se ven en nuestros otros programas, como el programa del Contrato Microsoft Online Subscription (MOSA). Esta disparidad significa que, para una serie de servicios, se agrega la unidad de medida para proporcionar los precios normalizados. La unidad de medida que se muestra en la vista del resumen de uso de Azure Enterprise Portal siempre es la medida empresarial. Para obtener una lista completa de las unidades de medida y conversiones actuales de cada servicio, envíe una [solicitud de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="conversion-between-usage-detail-report-and-the-usage-summary-page"></a>Conversión entre el informe de detalles de uso y la página de resumen de uso

En el informe de descarga de datos de uso, puede ver el uso de recursos sin procesar hasta seis lugares decimales. Sin embargo, los datos de uso que se muestran en Azure Enterprise Portal se redondean hasta cuatro posiciones decimales para las unidades de prepago y se truncan en cero decimales para las unidades de uso por encima del límite. Los datos de uso sin procesar se redondean primero a cuatro cifras antes de la conversión a las unidades usadas en Azure Enterprise Portal. A continuación, las unidades de Enterprise convertidas se redondean de nuevo a cuatro cifras. Solo puede ver el consumo de horas real antes de la conversión en el informe de uso descargado y no en Azure Enterprise Portal mismo.

Por ejemplo: Si se notifican 694,533404 horas reales de SQL Server en el informe de detalles de uso. Estas unidades se convierten en 6,94533404 unidades de 100 horas de proceso, y luego se redondean a 6,9453 y se muestran en Azure Enterprise Portal.

- Para determinar el importe de facturación ampliado, las unidades mostradas se multiplican por el precio de la unidad de prepago y el resultado se trunca a dos decimales. En el caso del yen japonés (JPY) y el won coreano (KRW), el importe extendido se redondea a cero decimales.
- Para el uso por encima del límite, las unidades de facturación se truncarían en seis cifras y, a continuación, se multiplicarían por el precio unitario de uso por encima del límite para determinar el importe de facturación ampliado.
- Para la facturación de proveedores de servicios administrados (MSP), todo el uso asociado a un departamento marcado como MSP se trunca a cero decimales después de la conversión a la unidad de medida de EA. Como resultado, la suma de este uso podría ser menor que la suma total de todo el uso notificado en Azure Enterprise Portal. Depende de si el MSP está dentro de su saldo de prepago de Azure o está en uso por encima del límite.

### <a name="graduated-pricing"></a>Precios escalonados

Los precios de los Contratos Enterprise no incluyen precios escalonados, donde los cargos por unidad disminuyen a medida que aumenta el uso. Al pasar de un programa MOSA con precios graduados a un Contrato Enterprise, los precios se establecen en proporción con el nivel base del servicio menos los ajustes correspondientes para los descuentos del Contrato Enterprise.

### <a name="partial-hour-billing"></a>Facturación por horas parciales

Todo el uso facturado se basa en minutos convertidos a horas parciales, y no en incrementos de horas completas. Las tarifas de Enterprise por hora enumeradas se aplican a las horas totales más las horas parciales.

### <a name="average-daily-consumption"></a>Consumo diario medio

Algunos servicios tienen un precio mensual, pero el uso se informa diariamente. En estos casos, el uso se evalúa una vez al día, se divide entre 31 y se suma según el número de días de ese mes de facturación. Por lo tanto, las tarifas nunca son superiores a lo previsto en ningún mes y son ligeramente inferiores en los meses con menos de 31 días.

### <a name="compute-hours-conversion"></a>Conversión de horas de proceso

Antes del 28 de enero de 2016, el uso de servicios en la nube y máquinas virtuales de nivel Básico y Estándar A0, A2, A3 y A4 se emitía como minutos del medidor para las máquinas virtuales A1. Las VM A0 se contaban como fracciones de minutos de VM A1, mientras que las A2, A3 y A4 se convertían en múltiplos. Debido a que esta directiva provocaba cierta confusión entre nuestros clientes, implementamos un cambio para asignar el uso por minuto a medidores dedicados para A0, A2, A3 y A4.

La nueva medición entró en vigor entre el 27 y el 28 de enero de 2016. En la descarga del archivo CSV que muestra el uso durante este período de transición, puede ver ambas:

- Uso emitido en el medidor A1
- Uso emitido en el nuevo medidor dedicado correspondiente al tamaño de la implementación.

| **Tamaño de la implementación** | **Uso emitido como múltiplo de A1 hasta el 26 de enero de 2016** | **Uso emitido conforme a una medida dedicada a partir del 27 de enero de 2016** |
| --- | --- | --- |
| A0 | 0,25 horas de A1 | 1 hora de A0 |
| A2 | 2 horas de A1 | 1 hora de A2 |
| A3 | 4 horas de A1 | 1 hora de A3 |
| A4 | 8 horas de A1 | 1 hora de A4 |

### <a name="regions"></a>Regions

En el caso de los servicios en los que la zona y la región afectan a los precios, consulte la tabla siguiente para ver un mapa de las regiones y zonas geográficas:

| Geoárea | Regiones de transferencia de datos | Regiones de CDN |
| --- | --- | --- |
| Zona 1 | Norte de Europa <br> Oeste de Europa <br> Oeste de EE. UU. <br> Este de EE. UU. <br> Centro y norte de EE. UU. <br> Centro y Sur de EE. UU. <br> Este de EE. UU. 2 <br> Centro de EE. UU. | Norteamérica <br> Europa |
| Zona 2 | Este de Asia Pacífico <br> Sudeste de Asia Pacífico <br> Japón Oriental <br> Japón Occidental <br> Este de Australia <br> Sudeste de Australia | Asia Pacífico <br> Japón <br> América Latina <br> Oriente Medio y África <br> Este de Australia <br> Sudeste de Australia |
| Zona 3 | Sur de Brasil |   |

No hay cargos por la salida de datos entre servicios hospedados en el mismo centro de datos. Por ejemplo, Microsoft 365 y Azure.

### <a name="azure-prepayment-and-unbilled-usage"></a>Prepago de Azure y uso no facturado

El prepago de Azure es un importe pagado por adelantado por los servicios de Azure. Se consume a medida que se usan los servicios. La facturación de los servicios propios de Azure utiliza el prepago de Azure. Sin embargo, algunos cargos se facturan por separado, y los servicios de Azure Marketplace no consumen el prepago de Azure.

### <a name="charges-billed-separately"></a>Gastos facturados por separado

Algunos productos y servicios proporcionados por orígenes de terceros no consumen el prepago de Azure. En su lugar, estos elementos se facturan por separado como parte de la factura de uso por encima del límite del período de facturación estándar.

Hemos combinado todos los cargos de Azure y Azure Marketplace en una sola factura alineada con el período de facturación de la inscripción. La factura combinada no se aplica a los clientes en Australia, Japón ni Singapur.

La factura combinada muestra el uso de Azure en primer lugar, seguido de los cargos de Azure Marketplace. Los clientes de Australia, Japón o Singapur ven los cargos de Azure Marketplace en una factura independiente.

Si no hay uso por encima del límite al final del período de facturación estándar, los cargos independientes se facturan por separado. Este proceso se aplica a los clientes de Australia, Japón y Singapur.

Los siguientes servicios son ejemplos de cargos facturados por separado. Para obtener una lista completa de los servicios en los que los cargos se facturan por separado, puede enviar una [solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

- Canonical
- Citrix XenApp Essentials
- Usuario registrado de Citrix XenDesktop
- Openlogic
- Usuario registrado de Remote Access Rights XenApp Essentials
- Ubuntu Advantage
- Visual Studio Enterprise (mensual)
- Visual Studio Enterprise (anual)
- Visual Studio Professional (mensual)
- Visual Studio Professional (anual)

## <a name="what-to-expect-after-change-of-channel-partner"></a>Qué esperar después de cambiar el asociado del canal

Si el cambio de asociado de canal (COCP) se produce a mediados de mes, el cliente recibirá una factura por el uso en el asociado anterior y otra factura por el uso en nuevo asociado.

Las facturas se publicarán al mes siguiente de que finalice el período de facturación. Si el ritmo de facturación es mensual, la factura de septiembre se publicará en octubre para ambos asociados. Si el período de facturación es trimestral o anual, el cliente puede esperar una factura para el asociado anterior por el uso en su período y el resto será para el nuevo asociado en función del ritmo de facturación.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la facturación y los cargos, consulte [Descripción de la factura de Azure Enterprise](../understand/review-enterprise-agreement-bill.md).
- Para empezar a usar Azure Enterprise Portal, consulte [Introducción al portal del Contrato Enterprise de Azure](ea-portal-get-started.md).
