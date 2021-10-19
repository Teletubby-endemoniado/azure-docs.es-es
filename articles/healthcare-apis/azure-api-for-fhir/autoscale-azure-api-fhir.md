---
title: Escalabilidad automática para Azure API para FHIR
description: En este artículo se describe la característica de escalabilidad automática para Azure API for FHIR.
author: stevewohl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 07/26/2021
ms.author: zxue
ms.openlocfilehash: d1aa4a6797fe6d7794f07b5da7832d80ac026e71
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2021
ms.locfileid: "122823339"
---
# <a name="autoscale-for-azure-api-for-fhir"></a>Escalabilidad automática para Azure API para FHIR 

Azure API for FHIR como servicio administrado permite a los clientes conservar los datos de atención sanitaria compatibles con FHIR e intercambiarlos de forma segura por medio de la API del servicio. Para dar cabida a diferentes cargas de trabajo de transacciones, los clientes pueden usar la escalabilidad manual o automática.

## <a name="what-is-autoscale"></a>¿Qué es el escalado automático?

De manera predeterminada, Azure API for FHIR se establece en la escalabilidad manual. Esta opción funciona bien cuando las cargas de trabajo de las transacciones son conocidas y coherentes. Los clientes pueden ajustar las `RU/s` de proceso por medio del portal hasta 10 000 y enviar una solicitud para aumentar el límite. 

Con la escalabilidad automática, los clientes pueden ejecutar varias cargas de trabajo; las `RU/s` de proceso se escalan y reducen verticalmente de forma automática sin necesidad de ajustes manuales.

## <a name="how-to-enable-autoscale"></a>Habilitación de la escalabilidad automática

Para habilitar la característica de escalabilidad automática, puede crear una incidencia de soporte técnico para solicitarla. El equipo de soporte técnico de Microsoft habilitará esta característica en función de la prioridad de soporte técnico.

> [!NOTE]
> La característica de escalabilidad automática no está disponible en Azure Portal.

## <a name="how-to-adjust-the-maximum-throughput-rus"></a>Ajuste del valor máximo de RU/s de proceso

Cuando se habilita la escalabilidad automática, el sistema calcula y establece el valor inicial de `Tmax`. La escalabilidad se rige por el valor de rendimiento máximo, o , y se escala `RU/s` `Tmax` entre `0.1 *Tmax` (o el 10 `Tmax` % ) y `Tmax RU/s` . 

Puede aumentar el valor máximo de `RU/s`, o `Tmax`, hasta el valor más alto que admita el servicio. Cuando el servicio está ocupado, las `RU/s` de proceso se escalan verticalmente hasta el valor `Tmax`. Si el servicio está inactivo, las `RU/s` de proceso se reducen verticalmente hasta el 10% de `Tmax`.
 
También puede reducir el valor máximo de `RU/s`, o `Tmax`. Al reducir el número máximo de `RU/s`, el valor mínimo que se puede establecer es: `MAX (4000, highest max RU/s ever provisioned / 10, current storage in GB * 400)`, redondeado en miles de `RU/s`.

* **Ejemplo 1**: tiene datos de 1 GB y el valor máximo de `RU/s` aprovisionado es 10 000. El valor mínimo es el máximo de (**4000**, 10 000/10, 1 x 400) = 4000. Se usa el primer número, **4000**.
* **Ejemplo 2**: tiene datos de 20 GB y el valor más alto de `RU/s` aprovisionado es 100 000. El valor mínimo es el máximo de (4000, **100 000/10**, 20 x 400) = 10 000. Se usa el segundo número, **100 000/10 =10 000**.
* **Ejemplo 3**: tiene datos de 80 GB y el valor más alto de RU/s aprovisionado es 300 000. El valor mínimo es el máximo de (4000, 300 000/10, **80 x 400**) = 32 000. Se usa el tercer número, **80 x 400 = 32 000**.

Puede ajustar el valor máximo o a través del portal si es un número válido y no mayor `RU/s` `Tmax` que 10 000 `RU/s` . Puede crear una incidencia de soporte técnico para solicitar un valor de `Tmax` mayor que 10 000.

>[!Note] 
>A medida que crece el almacenamiento de datos, el sistema aumentará automáticamente el rendimiento máximo a las siguientes RU/s más altas que puedan admitir ese nivel de almacenamiento.


## <a name="how-to-migrate-to-manual-scale"></a>¿Cómo migrar a escala manual?

Se requiere una vale de soporte técnico para cambiar el escalado automático a escala manual y especificar las RU/s de rendimiento. El valor mínimo para la escala manual que puede establecer en es: , redondeado al `MAX (400, highest max RU/s ever provisioned / 100, current storage in GB * 40)` 1000 más `RU/s` cercano. Los números que se usan aquí son diferentes de los que se usan en el escalado automático.

Una vez completado el cambio, las nuevas tarifas de facturación se basarán en la escala manual.

## <a name="what-is-the-cost-impact-of-autoscale"></a>¿Cómo afecta la escalabilidad automática al costo?

La característica de escalabilidad automática incurre en costos debido a la administración automática de las unidades de procesamiento aprovisionadas. Este aumento del costo no se aplica a los costos de almacenamiento y tiempo de ejecución. Para información sobre los precios, consulte [Precios de Azure Healthcare APIs](https://azure.microsoft.com/pricing/details/azure-api-for-fhir/).

>[!div class="nextstepaction"]
>[Acerca de la API de Azure para FHIR](overview.md)
