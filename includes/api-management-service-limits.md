---
title: Archivo de inclusión
description: archivo de inclusión
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 08/20/2021
ms.author: vlvinogr
ms.custom: include file
ms.openlocfilehash: d3317bafa2ff03e83bd79e9a3efa0ecfaffd4124
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2021
ms.locfileid: "122752880"
---
| Resource | Límite |
| ---------------------------------------------------------------------- | -------------------------- |
| Número máximo de unidades de escalado | 12 por región<sup>1</sup> |
| Tamaño de memoria caché | 5 GiB por unidad<sup>2</sup> |
| Conexiones simultáneas de back-end<sup>3</sup> por autoridad HTTP | 2048 por unidad<sup>4</sup> |
| Tamaño máximo de respuestas en caché | 2 MiB |
| Tamaño máximo del documento de directiva | 256 KiB<sup>5</sup> |
| Dominios de puerta de enlace personalizados máximos por cada instancia de servicio<sup>6</sup> | 20 |
| Número máximo de certificados de entidad de certificación por instancia de servicio<sup>7</sup> | 10 |
| Número máximo de instancias de servicio por suscripción<sup>8</sup> | 20 |
| Número máximo de suscripciones por instancia de servicio<sup>8</sup> | 500 |
| Número máximo de certificados de cliente por instancia de servicio<sup>8</sup> | 50 |
| Número máximo de API por instancia de servicio<sup>8</sup> | 50 |
| Número máximo de operaciones de administración de API por instancia de servicio<sup>8</sup> | 1,000 |
| Duración total máxima de la solicitud<sup>8</sup> | 30 segundos |
| Tamaño máximo de carga de solicitud<sup>8</sup> | 1 GiB |
| Tamaño máximo de carga útil en búfer<sup>8</sup> | 2 MiB |
| Tamaño máximo de la dirección URL de la solicitud<sup>9</sup> | 16384 bytes |
| Longitud máxima del segmento de la ruta de acceso de la dirección URL<sup>10</sup> | 260 caracteres |
| Tamaño máximo del esquema de API usado por la [directiva de validación](../articles/api-management/validation-policies.md)<sup>10</sup> | 4 MB |
| Tamaño máximo del cuerpo de solicitud o respuesta en la [directiva de validación de contenido](../articles/api-management/validation-policies.md#validate-content)<sup>10</sup> | 100 KB |
| Número máximo de puertas de enlace autohospedadas<sup>11</sup> | 25 |

<sup>1</sup>Los límites de escalado dependen del plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, consulte [Precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>2</sup>El tamaño de la caché por unidad depende del plan de tarifa. Para ver los planes de tarifa y sus límites de escalado, vea [Precios de API Management](https://azure.microsoft.com/pricing/details/api-management/).<br/>
<sup>3</sup>Las conexiones se agrupan y se vuelven a utilizar, a menos que el back-end las cierre explícitamente.<br/>
<sup>4</sup>Este límite es por unidad de los planes Básico, Estándar y Premium. El plan de desarrollador está limitado a 1024. Este límite no se aplica al plan de consumo.<br/>
<sup>5</sup>Este límite se aplica a los planes Básico, Estándar y Premium. En el plan de consumo, el tamaño del documento de directiva se limita a 16 KiB.<br/>
<sup>6</sup>Solo se admiten varios dominios personalizados en los planes Desarrollador y Premium.<br/>
<sup>7</sup>Los certificados de CA no se admiten en el plan de consumo.<br/>
<sup>8</sup>Este límite es aplicable solo al plan de consumo. No hay límites en estas categorías para otros niveles.<br/>
<sup>9</sup>Solo se aplica al plan de consumo. Incluye una cadena de consulta de hasta 2048 bytes de longitud.<br/>
<sup>10</sup> Para aumentar este límite, póngase en contacto con el [soporte técnico](https://azure.microsoft.com/support/options/).<br/>
<sup>11</sup>Las puertas de enlace autohospedadas solo se admiten en los planes Desarrollador y Premium. El límite se aplica al número de [recursos de puerta de enlace autohospedados](/rest/api/apimanagement/2020-06-01-preview/gateway). Para aumentar este límite, póngase en contacto con el departamento de [soporte técnico](https://azure.microsoft.com/support/options/). Tenga en cuenta que el número de nodos (o réplicas) asociados a un recurso de puerta de enlace autohospedado es ilimitado en el plan Premium y está limitado a un solo nodo en el plan Desarrollador.
