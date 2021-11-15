---
title: 'Descuento del plan de software: Azure'
description: Obtenga información sobre cómo se aplican los descuentos del plan de software al software en máquinas virtuales.
author: bandersmsft
ms.reviewer: primittal
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 10/28/2021
ms.author: banders
ms.openlocfilehash: fec1ecd33c47379d5c9f599aa97f47c8c543de78
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131425678"
---
# <a name="azure-software-plan-discount"></a>Descuento del plan de software de Azure

Los planes de software de Azure para SUSE y Red Hat son las reservas que se aplican a las máquinas virtuales implementadas. El descuento del plan de software se aplica al uso de máquinas virtuales implementadas que coincidan con la reserva de software.

Cuando apaga una máquina virtual, el descuento se aplica automáticamente a otra máquina virtual coincidente, si está disponible. Un plan del software cubre el costo de ejecutar el software SUSE en una máquina virtual. Otros gastos tales como proceso, almacenamiento y redes se cobran por separado.

Para comprar el plan adecuado, debe comprender el uso de la máquina virtual y el número de vCPU de dichas máquinas virtuales. Utilice las secciones siguientes para ayudar a identificar el plan que debe comprar en función de los datos de uso.

## <a name="how-reservation-discount-is-applied"></a>Aplicación del descuento por reserva

Un descuento de reserva significa "*usarlo o perderlo*". Por lo tanto, si no tiene recursos coincidentes para ninguna hora, perderá una cantidad de reserva para esa hora. No se pueden arrastrar las horas reservadas no utilizadas.

Al cerrar un recurso, el descuento por reserva se aplica automáticamente a otro recurso que coincida con el ámbito especificado. Si no se encuentran recursos coincidentes en el ámbito especificado, las horas reservadas se *pierden*.

## <a name="review-redhat-vm-usage-before-you-buy"></a>Revisión del uso de la máquina virtual de Red Hat antes de la compra

Obtenga el nombre del producto de los datos de uso y compre el plan de Red Hat con el mismo tipo y tamaño.

Por ejemplo, si su uso tiene el producto **Red Hat Enterprise Linux - 1-4 licencias de máquina virtual con vCPU**, debe comprar **Red Hat Enterprise Linux** para **máquinas virtuales con 1 a 4 vCPU**.

<!--ADD RHEL SCREENSHOT -->

## <a name="review-suse-vm-usage-before-you-buy"></a>Revisión del uso de máquinas virtuales SUSE antes de la compra

Obtenga el nombre del producto de los datos de uso y compre el plan de SUSE con el mismo tipo y tamaño.

Por ejemplo, si es el uso de producto **SUSE Linux Enterprise Server Prioritaria - Soporte técnico de máquinas virtuales con 2 a 4 vCPU**, debe comprar **SUSE Linux Enterprise Server Prioritaria** para **2 a 4 vCPU**.

![Ejemplo sobre cómo seleccionar el producto para comprar](./media/understand-suse-reservation-charges/select-suse-linux-enterprise-server-priority-2-4-vcpu.png)

## <a name="discount-applies-to-different-vm-sizes-for-suse-plans"></a>El descuento se aplica a los diferentes tamaños de VM para los planes SUSE

Del mismo modo que con las instancias de VM reservadas, las compras de planes SUSE ofrecen flexibilidad de tamaño de instancia. Esto significa que el descuento se aplica incluso cuando se implementa una VM con un recuento de vCPU diferente. El descuento se aplica a los diferentes tamaños de VM del plan de software.

El importe de descuento depende de la relación que aparece en las tablas siguientes. La relación compara la superficie relativa de cada medidor del grupo. La relación depende de las vCPU de VM. Use el valor de relación para calcular cuántas instancias de VM obtienen el descuento del plan SUSE Linux.

Por ejemplo, si compra un plan para SUSE Linux Enterprise Server para informática de alto rendimiento Prioritaria para una VM con 3 o 4 vCPU, la relación de esa reserva es 2. El descuento cubre el costo de software SUSE para:

- 2 VM implementadas con 1 o 2 vCPU,
- 1 VM implementada con 3 o 4 vCPU,
- o 0,77 o, aproximadamente, 77 % de una VM con 5 o más vCPU.

La proporción de 5 o más vCPU es 2,6. Por lo tanto, una reserva para SUSE con una VM con 5 o más vCPU cubre solo una parte del costo de software, que es, aproximadamente, el 77 %.

Las tablas siguientes muestran los planes de software para los que puede comprar una reserva, los medidores de uso asociados y las relaciones para cada uno.

### <a name="suse-linux-enterprise-server-for-hpc-priority"></a>SUSE Linux Enterprise Server para informática de alto rendimiento Prioritaria

|VM SUSE | Id. del medidor| Relación| Tamaño de VM de ejemplo|
| -------| ------------------------| --- |--- |
|SUSE Linux Enterprise Server para informática de alto rendimiento Prioritaria (1-2 CPU virtuales)|e275a668-ce79-44e2-a659-f43443265e98|1|D2s_v3|
|SUSE Linux Enterprise Server para informática de alto rendimiento Prioritaria (3-4 CPU virtuales)|e531e1c0-09c9-4d83-b7d0-a2c6741faa22|2|D4s_v3|
|SUSE Linux Enterprise Server para informática de alto rendimiento Prioritaria (5+ CPU virtuales)|4edcd5a5-8510-49a8-a9fc-c9721f501913|2.6|D8s_v3|

### <a name="suse-linux-enterprise-server-for-hpc-standard"></a>SUSE Linux Enterprise Server para informática de alto rendimiento Estándar

|VM SUSE | Id. del medidor | Relación|Tamaño de VM de ejemplo|
| ------- | --- | ------------------------| --- |
|SUSE Linux Enterprise Server para informática de alto rendimiento Estándar (1-2 CPU virtuales) |8c94ad45-b93b-4772-aab1-ff92fcec6610|1|D2s_v3|
|SUSE Linux Enterprise Server para informática de alto rendimiento Estándar (3-4 CPU virtuales)|4ed70d2d-e2bb-4dcd-b6fa-42da71861a1c|1,92308|D4s_v3|
|SUSE Linux Enterprise Server para informática de alto rendimiento Estándar (5+ CPU virtuales) |907a85de-024f-4dd6-969c-347d47a1bdff|2;92308|D8s_v3|

### <a name="suse-linux-enterprise-server-for-sap-standard"></a>SUSE Linux Enterprise Server para SAP Estándar

Anteriormente, SUSE Linux Enterprise Server para SAP Estándar se denominaba SUSE Linux Enterprise Server para SAP Prioritaria.

|VM SUSE | Id. del medidor | Relación|Tamaño de VM de ejemplo|
| ------- |------------------------| --- | --- |
|SUSE Linux Enterprise Server para SAP Estándar (1-2 CPU virtuales)|497fe0b6-fa3c-4e3d-a66b-836097244142|1|D2s_v3|
|SUSE Linux Enterprise Server para SAP Estándar (3-4 CPU virtuales) |847887de-68ce-4adc-8a33-7a3f4133312f|2|D4s_v3|
|SUSE Linux Enterprise Server para SAP Estándar (5+ CPU virtuales) |18ae79cd-dfce-48c9-897b-ebd3053c6058|2,41176|D8s_v3|

### <a name="suse-linux-enterprise-server-standard"></a>SUSE Linux Enterprise Server Estándar

|VM SUSE | Id. del medidor | Relación|Tamaño de VM de ejemplo|
| ------- |------------------------| --- |--- |
|SUSE Linux Enterprise Server Estándar (CPU virtuales de 1-2 núcleos) |4b2fecfc-b110-4312-8f9d-807db1cb79ae|1|D2s_v3|
|SUSE Linux Enterprise Server Estándar (CPU virtuales de 3-4 núcleos) |0c3ebb4c-db7d-4125-b45a-0534764d4bda|1,92308|D4s_v3|
|SUSE Linux Enterprise Server Estándar (CPU virtuales de 5+ núcleos) |7b349b65-d906-42e5-833f-b2af38513468|2,30769| D8s_v3|

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reservas, consulte los siguientes artículos:

- [¿Qué es Azure Reservations?](save-compute-costs-reservations.md)
- [Pago por adelantado para planes de software SUSE con Azure Reservations](../../virtual-machines/linux/prepay-suse-software-charges.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../../virtual-machines/prepay-reserved-vm-instances.md)
- [Administración de Azure Reservations](manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](understand-reserved-instance-usage-ea.md)