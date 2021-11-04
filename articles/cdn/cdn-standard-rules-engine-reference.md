---
title: Referencia del motor de reglas estándar de Azure CDN | Microsoft Docs
description: Documentación de referencia para las condiciones de coincidencia y las acciones del motor de reglas estándar de Azure Content Delivery Network (Azure CDN).
services: cdn
author: duongau
ms.service: azure-cdn
ms.topic: article
ms.date: 07/31/2021
ms.author: duau
ms.openlocfilehash: d3baf29ce8b5e452ed3c9ec22a5136188bac1d4e
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131446816"
---
# <a name="standard-rules-engine-reference-for-azure-cdn"></a>Referencia del motor de reglas estándar de Azure CDN

En el [motor de reglas estándar](cdn-standard-rules-engine.md) de Azure Content Delivery Network (Azure CDN), una regla consta de una o varias condiciones de coincidencia y una acción. En este artículo se muestran descripciones detalladas de las condiciones de coincidencia y las características disponibles en el motor de reglas estándar para Azure CDN.

El motor de reglas está diseñado para ser la autoridad definitiva sobre cómo Azure CDN estándar procesa los tipos específicos de solicitudes.

**Usos comunes de las reglas**:

- Invalidar o definir una directiva de memoria caché personalizada.
- Redirigir solicitudes.
- Modificar los encabezados de solicitudes y respuestas HTTP.

## <a name="terminology"></a>Terminología

Para definir una regla en el motor de reglas, establezca las [condiciones de coincidencia](cdn-standard-rules-engine-match-conditions.md) y las [acciones](cdn-standard-rules-engine-actions.md):

 ![Estructura de reglas de Azure CDN](./media/cdn-standard-rules-engine-reference/cdn-rules-structure.png)

Cada regla puede tener hasta diez condiciones de coincidencia y cinco acciones. Cada punto de conexión de Azure CDN puede tener hasta 25 reglas. 

En este límite se incluye una *regla global* predeterminada. La regla global no tiene condiciones de coincidencia, y las acciones que se definen en una regla global siempre se desencadenan.

   > [!IMPORTANT]
   > El orden en que se muestran varias reglas afecta a la manera en que se controlan. Las acciones que se especifican en una regla se pueden sobrescribir con una regla subsiguiente.

## <a name="limits-and-pricing"></a>Límites y precios 

Vea [Límites de escala de CDN](../azure-resource-manager/management/azure-subscription-service-limits.md#content-delivery-network-limits) para obtener el límite de las reglas. Para obtener los precios del motor de reglas, vea [Precios de Content Delivery Network](https://azure.microsoft.com/pricing/details/cdn/).

## <a name="syntax"></a>Sintaxis

La forma en que los caracteres especiales se tratan en una regla varía en función de cómo las diferentes condiciones de coincidencia y acciones controlan los valores de texto. Una condición de coincidencia o acción puede interpretar el texto de una de las siguientes maneras:

- [Valores literales](#literal-values)
- [Valores de carácter comodín](#wildcard-values)


### <a name="literal-values"></a>Valores literales

El texto que se interpreta como un valor literal trata todos los caracteres especiales, *excepto el símbolo %* , como parte del valor que debe coincidir en una regla. Por ejemplo, una condición de coincidencia literal establecida en `'*'` se cumple solo cuando se encuentra el valor exacto `'*'`.

Se usa un símbolo de porcentaje para indicar la codificación de direcciones URL (p. ej., `%20`).

### <a name="wildcard-values"></a>Valores de carácter comodín

Actualmente se admite el carácter comodín en la **condición de coincidencia UrlPath** en el motor de reglas estándar. El carácter \* es un carácter comodín que representa uno o más caracteres. 

## <a name="next-steps"></a>Pasos siguientes

- [Condiciones de coincidencia en el motor de reglas estándar](cdn-standard-rules-engine-match-conditions.md)
- [Acciones en el motor de reglas estándar](cdn-standard-rules-engine-actions.md)
- [Aplicación de HTTPS mediante el motor de reglas estándar](cdn-standard-rules-engine.md)
- [Información general de Azure CDN](cdn-overview.md)
