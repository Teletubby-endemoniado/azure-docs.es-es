---
title: Revisión de los datos de facturación de las suscripciones de Azure con la API REST
description: Obtenga información acerca de cómo usar las API REST de Azure para revisar los detalles de facturación de la suscripción. Puede usar filtros para facilitar la personalización de los resultados.
author: lleonard-msft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: article
ms.date: 09/15/2021
ms.author: banders
ms.openlocfilehash: 42f9482316e2cc229f7e0fed18f647c14b3dcd45
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128634964"
---
# <a name="review-subscription-billing-using-rest-apis"></a>Revisión de la facturación de suscripción mediante las API REST

Las API de informes de Azure le ayudarán a revisar y administrar los costos de Azure.

Los filtros ayudan a personalizar los resultados para satisfacer sus necesidades.

Aquí, aprenderá a utilizar una API REST para devolver los detalles de facturación de la suscripción para un rango de fechas determinado.

``` http
GET https://management.azure.com/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}/providers/Microsoft.Consumption/usageDetails?$filter=properties/usageEnd ge '${startDate}' AND properties/usageEnd le '${endDate}'
Content-Type: application/json
Authorization: Bearer
```

## <a name="build-the-request"></a>Compilar la solicitud

El parámetro `{subscriptionID}` es necesario y se identifica la suscripción de destino.

El parámetro `{billingPeriod}` es necesario y especifica un [período de facturación](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) actual.

Los parámetros `${startDate}` y `${endDate}` son necesarios para este ejemplo, pero opcionales para el punto de conexión. Especifican el rango de fechas como cadenas en formato AAAA-MM-DD (ejemplos: `'20180501'` y `'20180615'`).

Los siguientes encabezados son obligatorios:

|Encabezado de solicitud|Descripción|
|--------------------|-----------------|
|*Content-Type:*|Necesario. Establézcalo en `application/json`.|
|*Authorization:*|Necesario. Establézcalo en un [token de acceso](/rest/api/azure/#authorization-code-grant-interactive-clients) `Bearer` válido. |

## <a name="response"></a>Response

Se devuelve el código de estado 200 (OK) en el caso de una respuesta correcta, que contiene una lista costos detallados para la cuenta.

``` json
{
  "value": [
    {
      "id": "/subscriptions/{$subscriptionID}/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/{$detailsID}",
      "name": "{$detailsID}",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}",
        "invoiceId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/invoices/${invoiceID}",
        "usageStart": "${startDate}}",
        "usageEnd": "${endDate}",
        "currency": "USD",
        "usageQuantity": "${usageQuantity}",
        "billableQuantity": "${billableQuantity}",
        "pretaxCost": "${cost}",
        "meterId": "${meterID}",
        "meterDetails": "${meterDetails}"
      }
    }
  ],
  "nextLink": "${nextLinkURL}"
}
```

Cada elemento de **valor** representa un detalle sobre el uso de un servicio:

|Propiedad Response|Descripción|
|----------------|----------|
|**subscriptionGuid** | Identificador único global de la suscripción. |
|**startDate** | Fecha de inicio del uso. |
|**endDate** | Fecha de finalización del uso. |
|**useageQuantity** | Cantidad usada. |
|**billableQuantity** | Cantidad facturada realmente. |
|**pretaxCost** | Costo facturado, antes de impuestos aplicables. |
|**meterDetails** | La información detallada sobre el uso. |
|**nextLink**| Cuando se establece, especifica una dirección URL para la "página" siguiente de detalles. En blanco cuando la página es la última. |

Este ejemplo está abreviado; consulte los [detalles de uso de List](/rest/api/consumption/usagedetails/list#usagedetailslistforbillingperiod-legacy) para una descripción completa de cada campo de respuesta.

Otros códigos de estado indican condiciones de error. En estos casos, el objeto de respuesta explica por qué se ha producido un error en la solicitud.

``` json
{
  "error": [
    {
      "code": "Error type.",
      "message": "Error response describing why the operation failed."
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes
- Revisión de la [introducción a Enterprise Reporting](./enterprise-api.md)
- Investigación de la [API REST de facturación de empresa](/rest/api/billing/)
- [Get started with Azure REST API](/rest/api/azure/) (Introducción a la API REST de Azure)