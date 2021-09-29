---
title: 'Inicio rápido: Obtención y administración de números de teléfono mediante Azure Communication Services'
description: Aprenda a administrar números de teléfono mediante Azure Communication Services.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 06/30/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.subservice: pstn
ms.custom: references_regions
zone_pivot_groups: acs-azp-java-net-python-csharp-js
ms.openlocfilehash: a99f0d3aaf65ad7188f02c2c587653bd53f9948b
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128677119"
---
# <a name="quickstart-get-and-manage-phone-numbers"></a>Inicio rápido: Obtención y administración de números de teléfono

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

[!INCLUDE [Bulk Acquisition Instructions](../../includes/phone-number-special-order.md)]

::: zone pivot="platform-azp"
[!INCLUDE [Azure portal](./includes/phone-numbers-portal.md)]
::: zone-end

::: zone pivot="programming-language-csharp"
[!INCLUDE [Azure portal](./includes/phone-numbers-net.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/phone-numbers-java.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/phone-numbers-python.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/phone-numbers-js.md)]
::: zone-end

## <a name="troubleshooting"></a>Solución de problemas

Preguntas y problemas comunes:

- La compra del teléfono solo se admite en Estados Unidos. Para comprar los números de teléfono, asegúrese de lo siguiente:
  - La dirección de facturación de la suscripción de Azure asociada se encuentra en Estados Unidos. En este momento no se puede mover un recurso a otra suscripción.
  - El recurso de Communication Services se aprovisiona en la ubicación de datos de Estados Unidos. En este momento no se puede mover un recurso a otra ubicación de datos.

- Si se libera un número de teléfono, este no se liberará ni se podrá volver a comprar hasta el final del período de facturación.

- Si se elimina un recurso de Communication Services, los números de teléfono asociados a ese recurso se liberarán automáticamente al mismo tiempo.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha aprendido a hacer lo siguiente:

> [!div class="checklist"]
> * Comprar un número de teléfono
> * Administrar el número de teléfono
> * Liberar un número de teléfono

> [!div class="nextstepaction"]
> [Enviar un SMS](../telephony-sms/send.md)
> [Introducción a las llamadas](../voice-video-calling/getting-started-with-calling.md)
