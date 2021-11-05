---
title: Adición de detalles técnicos de una oferta de SaaS en Azure Marketplace
description: Proporcione los detalles técnicos de la oferta de software como servicio (SaaS) en Azure Marketplace.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.date: 10/25/2021
ms.openlocfilehash: 6a0b7494d5ce33527640c144faf8630d523ab941
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131061378"
---
# <a name="add-technical-details-for-a-saas-offer"></a>Adición de detalles técnicos de una oferta de SaaS

En este artículo se describe cómo especificar detalles técnicos que ayuden al marketplace comercial de Microsoft a conectarse a su solución. Esta conexión nos permite aprovisionar su oferta para el cliente, en caso de que este elija adquirirla y administrarla. Para más información sobre esta configuración, consulte [Información técnica](plan-saas-offer.md#technical-information).

> [!NOTE]
> Si elige procesar las transacciones de forma independiente, no verá esta opción. En lugar de ello, vaya a [Comercialización de ofertas de SaaS](create-new-saas-offer-marketing.md).

## <a name="technical-configuration"></a>Configuración técnica

En la pestaña **Configuración técnica**, definirá los detalles técnicos que usa el marketplace comercial para comunicarse con la solución o la aplicación SaaS.

- **URL de la página de aterrizaje** (obligatorio): defina la dirección URL del sitio de SaaS (por ejemplo: `https://contoso.com/signup`) en la que los clientes finales aterrizarán después de adquirir la oferta del marketplace comercial y de desencadenar el proceso de configuración a partir de la suscripción de SaaS recién creada.

  > [!IMPORTANT]
  > La página de aterrizaje estará en funcionamiento de forma ininterrumpida. Esta es la única vía por la que se le notificarán nuevas compras de las ofertas de SaaS realizadas en el marketplace comercial o las solicitudes de configuración de una suscripción activa a una oferta. No incluya el carácter de almohadilla (#) en la dirección URL de la página de aterrizaje. De lo contrario, los clientes no podrán acceder a la página de aterrizaje.

- **Webhook de conexión** (obligatorio): para todos los eventos asincrónicos que Microsoft necesita enviarle (por ejemplo, si la suscripción a SaaS se ha cancelado), se le exige que [proporcione una dirección URL de webhook de conexión](./partner-center-portal/pc-saas-fulfillment-api-v2.md#implementing-a-webhook-on-the-saas-service). Se llamará a esta dirección URL para notificarle el evento.

  > [!IMPORTANT]
  > El webhook debe estar en funcionamiento de forma ininterrumpida, ya que esta es la única vía por la que se le notificarán las actualizaciones de las suscripciones de SaaS de sus clientes adquiridas a través del marketplace comercial.

- **Id. de inquilino de Azure Active Directory** (obligatorio): para buscar el identificador de inquilino de la aplicación Azure Active Directory (Azure AD), vaya a la hoja [Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) de Azure Active Directory. En la columna **Nombre para mostrar**, seleccione la aplicación. Luego, busque el número de **Id. de directorio (inquilino)** que se muestra (por ejemplo, `50c464d3-4930-494c-963c-1e951d15360e`).

- **Id. de aplicación de Azure Active Directory** (obligatorio): para encontrar el [identificador de aplicación](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in), vaya a la hoja [Registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) de Azure Active Directory. En la columna **Nombre para mostrar**, seleccione la aplicación. Luego, busque el número de identificador de la aplicación (cliente) que se muestra (por ejemplo, `50c464d3-4930-494c-963c-1e951d15360e`).

Seleccione **Guardar borrador** antes de continuar con la siguiente pestaña: Información general del plan.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de planes para una oferta de SaaS](create-new-saas-offer-plans.md)
