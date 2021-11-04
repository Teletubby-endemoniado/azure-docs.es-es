---
title: Descarga de los datos de factura de Azure
description: En este artículo se describe cómo descargar o ver los datos de la factura de Azure.
keywords: billing invoice,invoice download,azure invoice
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 10/28/2021
ms.author: banders
ms.openlocfilehash: bc159ccab9d2a059d98de59e123d3e708ade9b2b
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131450084"
---
# <a name="download-or-view-your-azure-billing-invoice"></a>Descargar o visualización de los datos de la factura de Azure

En la mayoría de las suscripciones, puede descargar la factura desde [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) o recibirla por correo electrónico. Si es cliente de Azure con un Contrato Enterprise (cliente de EA), no puede descargar las facturas de su organización. Las facturas se envían a la persona que se configuró para recibir las facturas de la inscripción.

Solo determinados roles tienen permiso para obtener las facturas, como el administrador de la cuenta o el administrador de Enterprise. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](manage-billing-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

Si tiene un contrato de cliente de Microsoft, debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación para ver información de facturación. Para más información acerca de los roles de facturación para los contratos de cliente de Microsoft, consulte [Tareas y roles del perfil de facturación](understand-mca-roles.md#billing-profile-roles-and-tasks).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="download-your-azure-invoices-pdf"></a>Descarga de las facturas de Azure (.pdf)

En la mayoría de las suscripciones, puede descargar la factura desde Azure Portal. Si tiene un contrato de cliente de Microsoft, consulte cómo descargar facturas para un perfil de facturación.

### <a name="download-invoices-for-an-individual-subscription"></a>Descarga de facturas para una suscripción individual

1. Seleccione su suscripción en la [página Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) de Azure Portal como [un usuario con acceso a las facturas](manage-billing-access.md).

2. Seleccione **Facturas**.

    ![Captura de pantalla que muestra la opción Facturación y uso](./media/download-azure-invoice-daily-usage-date/billingandusage.png)

3. Haga clic en el botón Descargar para descargar una copia de la factura en PDF y, después, seleccione **Descargar factura**. Si muestra **No disponible**, vea [¿Por qué no veo una factura para el último período de facturación?](#noinvoice)

    ![Captura de pantalla que muestra los períodos de facturación, la opción de descarga y los cargos totales para cada período de facturación](./media/download-azure-invoice-daily-usage-date/downloadinvoice.png)

4. También puede descargar un desglose diario de las cantidades consumidas y los gastos estimados haciendo clic en **Descargar CSV**.

    ![Captura de pantalla que muestra la página Descargar factura y uso](./media/download-azure-invoice-daily-usage-date/usageandinvoice.png)

Para más información sobre la factura, consulte [Comprender la factura de Microsoft Azure](../understand/review-individual-bill.md). Para ayuda en la administración de los costos, consulte [Análisis de cargos inesperados](../understand/analyze-unexpected-charges.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Descarga de facturas para un contrato de cliente de Microsoft

Las facturas se generan para cada [perfil de facturación](../understand/mca-overview.md#billing-profiles) en el contrato de cliente de Microsoft. Debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación para descargar facturas desde Azure Portal.

1. Busque **Administración de costos + facturación**.
2. Seleccione un perfil de facturación.
3. Seleccione **Facturas**.
4. En la cuadrícula de la factura, busque la fila de la factura que desea descargar.
5. Haga clic en el botón Descargar al final de la fila.
6. En el menú contextual de descarga, seleccione **Factura**.

Si no ve una factura del último período de facturación, consulte **Información adicional**. <!-- Fix this -->
### <a name="why-dont-i-see-an-invoice-for-the-last-billing-period"></a><a name="noinvoice"></a> ¿Por qué no veo una factura para el último período de facturación?

Pueden existir varias razones por las que no ve una factura:

- Han transcurrido menos de 30 días desde el día que se suscribió a Azure.

- La factura no se ha generado aún. Espere hasta el final del período de facturación.

- No tiene permiso para ver facturas. Si tiene un contrato de cliente de Microsoft, debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación. Para otras suscripciones, es posible que no vea las facturas antiguas si no es el administrador de cuenta. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](manage-billing-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

- Si tiene una evaluación gratuita o una cantidad de crédito mensual con la suscripción que no ha superado, no obtendrá una factura a menos que tenga un contrato de cliente de Microsoft.

## <a name="get-your-invoice-in-email-pdf"></a>Obtención de la factura por correo electrónico (.pdf)

Puede optar a destinatarios adicionales y configurarlos para recibir la factura de Azure en un correo electrónico. Esta característica puede no estar disponible para determinadas suscripciones, como ofertas de soporte técnico, contratos Enterprise o Azure bajo licencia Open. Si tiene un contrato de cliente de Microsoft, consulte [cómo obtener las facturas del perfil de facturación por correo electrónico](../understand/download-azure-invoice.md#get-your-billing-profiles-invoice-in-email).

### <a name="get-your-subscriptions-invoices-in-email"></a>Obtención de las facturas de su suscripción por correo electrónico

1. Seleccione su suscripción en la [página Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Tendrá que habilitar la opción en cada suscripción que posea. Haga clic en **Facturas** y luego en **Email my invoice** (Enviar mi factura por correo electrónico).

    ![Captura de pantalla que muestra el flujo de participación](./media/download-azure-invoice-daily-usage-date/invoicesdeeplink01.png)

2. Haga clic en **Recibir notificaciones** y acepte los términos.

    ![Captura de pantalla que muestra el paso 2 del flujo de participación](./media/download-azure-invoice-daily-usage-date/invoicearticlestep02.png)

3. Una vez que ha aceptado el contrato, puede configurar destinatarios adicionales. Cuando se quita un destinatario, ya no se almacena la dirección de correo electrónico. Si cambia de opinión, deberá volver a agregarlos.

    ![Captura de pantalla que muestra el paso 3 del flujo de participación](./media/download-azure-invoice-daily-usage-date/invoicearticlestep03.png)

Si no recibe un correo electrónico después de seguir los pasos, asegúrese de que su dirección de correo electrónico sea correcta en las [preferencias de comunicación de su perfil](https://account.microsoft.com/profile) en el centro de cuentas de Microsoft.

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Opción de no recibir las facturas de su suscripción por correo electrónico

Puede optar por dejar de recibir la factura por correo electrónico; para ello, siga los pasos anteriores y haga clic en **Deshabilitar el envío de facturas por correo electrónico**. Esta opción eliminará todas las direcciones de correo electrónico configuradas para recibir las facturas por correo electrónico. Si cambia de opinión, tendrá que volver a configurar los destinatarios.

 ![Captura de pantalla que muestra el flujo para deshabilitar](./media/download-azure-invoice-daily-usage-date/invoicearticlestep04.png)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Obtención de las facturas del contrato de cliente de Microsoft por correo electrónico

Si tiene un contrato de cliente de Microsoft, puede optar por recibir la factura por correo electrónico. La factura se envía por correo electrónico a los propietarios del perfil de facturación, colaboradores, lectores y administradores de facturación. Los lectores no pueden actualizar la preferencia de factura por correo electrónico.

1. Busque **Administración de costos + facturación**.
1. Seleccione un perfil de facturación.
1. En **Configuración**, seleccione **Propiedades**.
1. En **Enviar factura por correo electrónico**, seleccione **Actualizar la preferencia de factura por correo electrónico**.
1. Seleccione **Habilitar envío**.
1. Haga clic en **Update**(Actualizar).

### <a name="opt-out-of-getting-your-billing-profile-invoices-in-email"></a>Opción de no recibir las facturas del perfil de facturación por correo electrónico

Puede optar por dejar de recibir la factura por correo electrónico; para ello, siga los pasos anteriores y haga clic en **Deshabilitar envío**. Todos los propietarios, colaboradores, lectores y administradores de facturación dejarán también de recibir la factura por correo electrónico. Si es lector, no puede cambiar la preferencia de factura por correo electrónico.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la factura y los cargos, consulte:

- [Comprender la factura de Microsoft Azure](../understand/review-individual-bill.md)
- [Descripción de los términos en su factura de Microsoft Azure](../understand/understand-invoice.md)
- [Descripción de los términos de uso detallados de Microsoft Azure](../understand/understand-usage.md)
- [Visualización de los precios de Azure de su organización](ea-pricing.md)

Si tiene un contrato de cliente de Microsoft, vea:

- [Comprender los cargos de facturación de su contrato de cliente de Microsoft](../understand/review-customer-agreement-bill.md)
- [Descripción de los términos en la factura del contrato de cliente de Microsoft](../understand/mca-understand-your-invoice.md)
- [Descripción de los términos en los CSV de cargos y el uso de Azure para un contrato de cliente de Microsoft](../understand/mca-understand-your-usage.md)
- [Ver los documentos de impuestos del contrato de cliente de Microsoft](../understand/mca-download-tax-document.md)
- [Visualización de los precios de Azure de su organización](ea-pricing.md)
