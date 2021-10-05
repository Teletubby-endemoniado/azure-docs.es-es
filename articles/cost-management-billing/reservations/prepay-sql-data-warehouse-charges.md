---
title: Ahorre en Azure Synapse Analytics con la capacidad reservada de Azure
description: Aprenda a ahorrar costos en Azure Synapse Analytics gracias a la capacidad reservada para ahorrar dinero.
author: bandersmsft
ms.reviewer: primittal
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: how-to
ms.date: 09/15/2021
ms.author: banders
ms.openlocfilehash: f738b2e551ecb26426ef25603969fb471ecfa437
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128666597"
---
# <a name="save-costs-for-azure-synapse-analytics-charges-with-reserved-capacity"></a>Ahorro en Azure Synapse Analytics con la capacidad reservada

Para ahorrar dinero con Azure Synapse Analytics, debe realizar una reserva de uso de cDWU durante uno o tres años. Para comprar capacidad reservada de Azure Synapse Analytics, es preciso elegir la región de Azure y el período. Después, se debe agregar la SKU de Azure Synapse Analytics al carro de compra y elegir la cantidad de unidades de cDWU que se desea comprar.

Cuando compra una reserva, el uso de Azure Synapse Analytics que coincida con los atributos de la reserva no se cobra a las tarifas de pago por uso.

Las reservas no cubren los cargos de almacenamiento o de red asociados con el uso de Azure Synapse Analytics.

Cuando expira la capacidad reservada, las instancias de Azure Synapse Analytics no dejan de ejecutarse, pero se facturan a la tarifa de pago por uso. Las reservas no se renuevan automáticamente.

Para obtener información sobre los precios, consulte [Oferta de capacidad reservada de Azure Synapse Analytics](https://azure.microsoft.com/pricing/details/synapse-analytics/).

La capacidad reservada de Azure Synapse Analytics se puede comprar en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade). Pague la reserva [por adelantado o mensualmente](./prepare-buy-reservation.md). Para adquirir capacidad reservada:

- Debe tener rol de propietario al menos en una suscripción Enterprise o de Pago por uso.
- En el caso de las suscripciones Enterprise, la opción **Agregar instancias reservadas** debe estar habilitada en el [portal de EA](https://ea.azure.com/). O bien, si esa opción está deshabilitada, debe ser un administrador del Contrato Enterprise.
- En el caso del programa del Proveedor de soluciones en la nube (CSP), los únicos que pueden comprar la capacidad reservada de Azure Synapse Analytics son los agentes de administración o de ventas.

Para más información sobre cómo se les cobra a los clientes de empresa y a los de pago por uso las compras de reservas, consulte [Información sobre el uso de reservas de Azure para la inscripción Enterprise](understand-reserved-instance-usage-ea.md) y [Información sobre el uso de reservas de Azure para suscripciones de pago por uso](understand-reserved-instance-usage.md).

## <a name="choose-the-right-size-before-purchase"></a>Elección del tamaño adecuado antes de la compra

El tamaño de la reserva en Azure Synapse Analytics debe basarse en el total de unidades de almacenamiento de datos de proceso (cDWU) que se consumen. Las compras se realizan en incrementos de 100 cDWU.

Por ejemplo, suponga que el consumo total de Azure Synapse Analytics es DW3000c. Desea adquirir capacidad reservada para todo ello. Por tanto, debe comprar 30 unidades de capacidad reservada de cDWU.

## <a name="buy-azure-synapse-analytics-reserved-capacity"></a>Compra de capacidad reservada de Azure Synapse Analytics

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Seleccione una suscripción. Utilice la lista de suscripciones para elegir la que se va a usar para pagar la capacidad reservada. Los costos de la capacidad reservada se cobran en el método de pago de la suscripción. El tipo de suscripción debe ser Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P) o Pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P).
   - En el caso de una suscripción Enterprise, los cargos se deducirán del saldo de prepago de la inscripción de Azure (anteriormente llamado compromiso monetario) o se cobrarán como parte del uso por encima del límite.
   - Para la suscripción Pago por uso, los cargos se cobran en el método de pago de tarjeta de crédito o factura de la suscripción.
4. Seleccione un ámbito. Utilice la lista de ámbitos para elegir el ámbito de la suscripción.
   - **Single resource group scope** (Ámbito de grupo de recursos único): aplica el descuento por reserva a los recursos coincidentes solo en el grupo de recursos seleccionado.
   - **Single subscription scope** (Ámbito de suscripción única): aplica el descuento por reserva a los recursos coincidentes de la suscripción seleccionada.
   - **Ámbito compartido**: aplica el descuento por reserva a los recursos coincidentes en suscripciones aptas que están en el contexto de facturación. Para los clientes con Contrato Enterprise, el contexto de facturación es la inscripción. En el caso de suscripciones individuales con tarifas de pago por uso, el ámbito de facturación son todas las suscripciones aptas creadas por el administrador de la cuenta.
       - Para los clientes empresariales, el contexto de facturación es la inscripción en el Contrato Enterprise.
       - Para los clientes de Pago por uso, el ámbito compartido incluye todas las suscripciones de Pago por uso creadas por el administrador de la cuenta.
   - **Grupo de administración**: aplica el descuento de reserva al recurso correspondiente en la lista de suscripciones que forman parte del grupo de administración y del ámbito de facturación.
5. Seleccione una región para elegir una región de Azure que abarca la capacidad reservada.
6. Elija una cantidad. Especifique la cantidad de 100 unidades de almacenamiento de datos (cDWU) que desea comprar.    
   Por ejemplo, una cantidad de 30 le aportará 3000 cDWU de capacidad reservada cada hora.
7. Examine el costo de la reserva de capacidad reservada de Azure Synapse Analytics en la sección **Costos**.
8. Seleccione **Comprar**.
9. Seleccione **Ver esta reserva** para conocer el estado de la compra.

## <a name="cancel-exchange-or-refund-reservations"></a>Cancelación, intercambio o reembolso de reservas

Puede cancelar, intercambiar o reembolsar reservas con ciertas limitaciones. Para más información, consulte [Autoservicio de intercambios y reembolsos de reservas de Azure](exchange-and-refund-azure-reservations.md).

Se aplica un descuento por la reserva automáticamente al número de instancias de Azure Synapse Analytics que coincidan con la región y el ámbito de la capacidad reservada de Azure Synapse Analytics. El ámbito de la capacidad reservada de Azure Synapse Analytics se puede actualizar con [Azure Portal](https://portal.azure.com/), PowerShell, la CLI o la API.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo se aplican los descuentos por reserva a Azure Synapse Analytics, consulte [Aplicación de descuentos por reserva a Azure Synapse Analytics](prepay-sql-data-warehouse-charges.md).

- Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:
  - [¿Qué es Azure Reservations?](save-compute-costs-reservations.md)
  - [Administración de Azure Reservations](manage-reserved-vm-instance.md)
  - [Información sobre el descuento de Azure Reservations](understand-reservation-charges.md)
  - [Información sobre el uso de reservas para suscripciones de pago por uso](understand-reserved-instance-usage.md)
  - [Información sobre el uso de reservas para la inscripción Enterprise](understand-reserved-instance-usage-ea.md)