---
title: Pago por adelantado de recursos de proceso con capacidad reservada para Azure Database for MySQL
description: Pago por adelantado de recursos de proceso de Azure Database for MySQL con capacidad reservada
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 10/06/2021
ms.openlocfilehash: fb679c4cfdcda3a34ea43bace8a9aa546e542acf
ms.sourcegitcommit: e82ce0be68dabf98aa33052afb12f205a203d12d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2021
ms.locfileid: "129658163"
---
# <a name="prepay-for-azure-database-for-mysql-compute-resources-with-reserved-instances"></a>Pago por adelantado de recursos de proceso de Azure Database for MySQL con instancias reservadas

[!INCLUDE[applies-to-mysql-single-flexible-server](includes/applies-to-mysql-single-flexible-server.md)]

Ahora Azure Database for MySQL le ayuda a ahorrar mediante el pago por adelantado de los recursos de proceso en comparación con los precios de pago por uso. Con las instancias reservadas de Azure Database for MySQL se realiza un compromiso inicial en el servidor MySQL sobre un periodo de entre uno y tres años con el fin de obtener un descuento importante en los costos de proceso. Para comprar capacidad reservada de Azure Database for MySQL, debe especificar la región de Azure, el tipo de implementación, el nivel de rendimiento y el periodo. </br>

## <a name="how-does-the-instance-reservation-work"></a>Funcionamiento de la reserva de instancias
No es necesario asignar la reserva a servidores concretos de Azure Database for MySQL. Una instancia de Azure Database for MySQL que ya está en ejecución, o las que se han implementado recientemente, obtendrán de forma automática la ventaja de los precios reservados. Al comprar una reserva, se adelanta el pago de los costos de proceso durante un período de uno a tres años. En cuanto se compra una reserva, los costos de proceso de Azure Database for MySQL que coincidan con los atributos de reserva dejan de pagarse según las tarifas de pago por uso. La reserva no cubre los cargos por software, redes o almacenamiento asociados al servidor de bases de datos de MySQL. Al final del plazo de reserva, la ventaja en la facturación expira y las instancias de Azure Database for MySQL se facturan según los precios de pago por uso. Las reservas no se renuevan automáticamente. Para obtener información sobre precios, vea [Oferta de capacidad reservada de Azure Database for MySQL](https://azure.microsoft.com/pricing/details/mysql/). </br>

Puede comprar capacidad reservada de Azure Database for MySQL en [Azure Portal](https://portal.azure.com/). Pague la reserva [por adelantado o mensualmente](../cost-management-billing/reservations/prepare-buy-reservation.md). Para comprar capacidad reservada:

* Debe tener el rol de propietario al menos en una suscripción Enterprise o individual con tarifas de pago por uso.
* En el caso de las suscripciones Enterprise, la opción **Agregar instancias reservadas** debe estar habilitada en el [portal de EA](https://ea.azure.com/). O bien, si esa opción está deshabilitada, debe ser un administrador de EA en la suscripción.
* En el caso del programa del Proveedor de soluciones en la nube (CSP), los únicos que pueden comprar la capacidad reservada de Azure Database for MySQL son los agentes de administración o de ventas. </br>

Para información sobre cómo se les cobra a los clientes de empresa y a los de pago por uso las compras de reservas, consulte [Información sobre el uso de reservas de Azure para la inscripción Enterprise](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md) y [Información sobre el uso de reservas de Azure para suscripciones de pago por uso](../cost-management-billing/reservations/understand-reserved-instance-usage.md).

## <a name="reservation-exchanges-and-refunds"></a>Cambios de reserva y reembolsos

Puede intercambiar una reserva por otra del mismo tipo, también puede intercambiar una de Azure Database for MySQL: servidor único por servidor flexible. Además es posible reembolsar una reserva si ya no la necesita. Puede usar Azure Portal para intercambiar o reembolsar una reserva. Para más información, consulte [Autoservicio de intercambios y reembolsos de reservas de Azure](../cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

## <a name="reservation-discount"></a>Descuentos por reserva

Puede ahorrar hasta un 67 % en costos de proceso con las instancias reservadas. Para encontrar el descuento aplicable en su caso, visite la [hoja Reserva de Azure Portal](https://aka.ms/reservations) y compruebe el ahorro por plan de tarifa y región. Las instancias reservadas le ayudan a administrar mejor las cargas de trabajo, el presupuesto y la previsión, gracias al pago por adelantado en períodos de un año o de tres años. También se pueden intercambiar o cancelar las reservas a medida que cambien las necesidades del negocio.


## <a name="determine-the-right-database-size-before-purchase"></a>Determinación del tamaño de base de datos adecuado antes de la compra

El tamaño de la reserva se debe basar en la cantidad total de proceso que va a usar el servidor existente o que se va a implementar pronto en una región específica y con el mismo nivel de rendimiento y generación de hardware.</br>

Por ejemplo, imagine que ejecuta una base de datos MySQL de propósito general Gen5 de 32 núcleos virtuales y dos bases de datos MySQL optimizadas para memoria Gen5 de 16 núcleos virtuales. Además, supongamos que planea implementar en el próximo mes un grupo elástico Gen5 de uso general y 32 núcleos virtuales adicionales, y un servidor de bases de datos optimizado para memoria Gen5 de 16 núcleos virtuales. Vamos a suponer que sabe que necesitará estos recursos durante al menos 1 año. En este caso debe comprar una reserva de 1 año de una instancia de Gen5 con 64 núcleos virtuales (2 × 32) para una base de datos única de uso general y otra reserva de 1 año de una instancia de Gen 5 con 48 núcleos virtuales (2 × 16 + 16) para una base de datos única optimizada para memoria.


## <a name="buy-azure-database-for-mysql-reserved-capacity"></a>Capacidad reservada de Azure Database for MySQL

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Seleccione **Agregar** y, en el panel Comprar reservas, seleccione **Azure Database for MySQL** para comprar una nueva reserva para las bases de datos de MySQL.
4. Rellene todos los campos obligatorios. Las bases de datos existentes o nuevas que coincidan con los atributos seleccionados serán aptas para el descuento en la capacidad reservada. El número real de servidores de Azure Database for MySQL que obtienen el descuento depende del ámbito y la cantidad seleccionados.


:::image type="content" source="media/concepts-reserved-pricing/mysql-reserved-price.png" alt-text="Información general sobre los precios reservados":::


En la siguiente tabla se describen los campos obligatorios.

| Campo | Descripción |
| :------------ | :------- |
| Subscription   | La suscripción usada para pagar la reserva de capacidad reservada de Azure Database for MySQL. Los costos anticipados por la reserva de capacidad reservada de Azure Database for MySQL se cobran mediante el método de pago de la suscripción. El tipo de suscripción debe ser Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P) o un contrato individual con precios de pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P). Para una suscripción Enterprise, los cargos se deducen del pago por adelantado de Azure (antes conocido como saldo de compromiso monetario) de la inscripción, o se cobran como uso por encima del límite. Para una suscripción individual con precios de pago por uso, los cargos se cobran en el método de pago de la factura o la tarjeta de crédito de la suscripción.
| Ámbito | El ámbito de la reserva de núcleos virtuales puede cubrir una suscripción o varias (ámbito compartido). Si selecciona: </br></br> **Compartido**: el descuento por la reserva de núcleos virtuales se aplica a los servidores de Azure Database for MySQL en ejecución en cualquiera de las suscripciones en el contexto de facturación. Para los clientes Enterprise, el ámbito compartido es la inscripción e incluye todas las suscripciones que esta contiene. Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta.</br></br> **Suscripción única**: el descuento por la reserva de núcleos virtuales se aplica a los servidores de Azure Database for MySQL de esta suscripción. </br></br> **Grupo de recursos único**: el descuento de reserva se aplica a los servidores de Azure Database for MySQL de la suscripción seleccionada y al grupo de recursos seleccionado de esa suscripción.
| Region | La región de Azure que abarca la reserva de capacidad reservada de Azure Database for MySQL.
| Tipo de implementación | El tipo de recurso de Azure Database for MySQL para el que quiere comprar la reserva.
| Nivel de rendimiento | El nivel de servicio de los servidores de Azure Database for MySQL.
| Término | Un año
| Cantidad | Número de recursos de proceso que se compran dentro de la reserva de capacidad reservada de Azure Database for MySQL. La cantidad es un número de núcleos virtuales de la región de Azure y el nivel de rendimiento seleccionados que se están reservando y obtendrán el descuento de facturación. Por ejemplo, si ejecuta o planea ejecutar servidores de Azure Database for MySQL con la capacidad total de proceso de 16 núcleos virtuales Gen5 en la región Este de EE. UU., deberá especificar la cantidad como 16 para maximizar las ventajas de todos los servidores.

## <a name="reserved-instances-api-support"></a>Soporte técnico de la API de instancias reservadas

Use las API de Azure para obtener información mediante programación para su organización sobre el servicio de Azure o las reservas de software. Por ejemplo, use las API para:

- Buscar las reservas que desee comprar
- Adquisición de una reserva
- Visualización de las reservas adquiridas
- Ver y administrar el acceso a las reservas
- Dividir o combinar reservas
- Cambiar el ámbito de las reservas

Para más información, consulte [APIs for Azure reservation automation](../cost-management-billing/reservations/reservation-apis.md) (API de automatización de reservas de Azure).

## <a name="vcore-size-flexibility"></a>Flexibilidad de tamaño del núcleo virtual

La flexibilidad de tamaño del núcleo virtual le ayuda a escalar o reducir verticalmente dentro de un nivel de rendimiento y una región, sin perder los beneficios de la capacidad reservada. 

## <a name="how-to-view-reserved-instance-purchase-details"></a>Visualización de los detalles de la compra de instancias reservadas

Puede ver los detalles de la compra de una instancia reservada por medio del [menú Reservas en el lado izquierdo de Azure Portal](https://aka.ms/reservations). Para obtener más información, vea [Aplicación de un descuento por reserva a un servidor individual Azure Database for MySQL](../cost-management-billing/reservations/understand-reservation-charges-mysql.md).

## <a name="reserved-instance-expiration"></a>Expiración de las instancias reservadas

Recibirá notificaciones por correo electrónico, la primera, 30 días antes de la expiración de la reserva y otra al expirar. Una vez que expire la reserva, las máquinas virtuales implementadas seguirán ejecutándose y se facturarán mediante una tasa de pago por uso. Para obtener más información, vea [Instancias reservadas de Azure Database for MySQL](../cost-management-billing/reservations/understand-reservation-charges-mysql.md).

## <a name="need-help--contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

El descuento por la reserva de núcleos virtuales se aplica automáticamente al número de servidores de Azure Database for MySQL que coincidan con el ámbito y los atributos de la reserva de capacidad reservada de Azure Database for MySQL. Se puede actualizar el ámbito de la reserva de capacidad reservada de Azure Database for MySQL a través de Azure Portal, PowerShell, la CLI o la API. </br></br>
Para obtener información sobre cómo administrar la capacidad reservada de Azure Database for MySQL, vea la sección Administración de la capacidad reservada de Azure Database for MySQL.

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

* [¿Qué es Azure Reservations?](../cost-management-billing/reservations/save-compute-costs-reservations.md)
* [Administración de Azure Reservations](../cost-management-billing/reservations/manage-reserved-vm-instance.md)
* [Información sobre el descuento de Azure Reservations](../cost-management-billing/reservations/understand-reservation-charges.md)
* [Información sobre el uso de reservas para suscripciones de pago por uso](../cost-management-billing/reservations/understand-reservation-charges-mysql.md)
* [Información sobre el uso de reservas para la inscripción Enterprise](../cost-management-billing/reservations/understand-reserved-instance-usage-ea.md)
* [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](/partner-center/azure-reservations)