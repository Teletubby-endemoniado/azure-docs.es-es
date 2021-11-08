---
title: Tabla de referencia de todas las recomendaciones de Microsoft Defender for Cloud
description: En este artículo se muestra una lista de las recomendaciones de seguridad de Microsoft Defender for Cloud que permiten proteger los recursos.
author: memildin
ms.service: security-center
ms.topic: reference
ms.date: 09/05/2021
ms.author: memildin
ms.custom: generated
ms.openlocfilehash: 7f00f874cb1611e86867f19859ffcaa4800f323a
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131457209"
---
# <a name="security-recommendations---a-reference-guide"></a>Guía de referencia sobre las recomendaciones de seguridad

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

En este artículo se muestra una lista de las recomendaciones que puede ver en Microsoft Defender for Cloud. Las recomendaciones que se muestran en su entorno dependen de los recursos que se van a proteger y de la configuración personalizada.

Las recomendaciones de Defender for Cloud se basan en [Azure Security Benchmark](../security/benchmarks/introduction.md). Azure Security Benchmark es el conjunto de directrices específico de Azure creado por Microsoft para ofrecer los procedimientos recomendados de seguridad y cumplimiento basados en marcos de cumplimiento comunes. Este punto de referencia, que cuenta con un amplísimo respaldo, se basa en los controles del [Centro de seguridad de Internet (CIS)](https://www.cisecurity.org/benchmark/azure/) y del [Instituto Nacional de Normas y Tecnología (NIST)](https://www.nist.gov/), con un enfoque en seguridad centrada en la nube.

Para obtener información sobre cómo responder a estas recomendaciones, vea [Recomendaciones de corrección en Microsoft Defender for Cloud](implement-security-recommendations.md).

La puntuación de seguridad se basa en el número de recomendaciones de Defender for Cloud que se han completado. Para decidir qué recomendaciones se resuelven en primer lugar, examine la gravedad de cada una de ellas y su posible impacto en la puntuación de seguridad.

> [!TIP]
> Si la descripción de una recomendación indica "No hay ninguna directiva relacionada", suele deberse a que esta recomendación depende de una recomendación distinta y de _su_ directiva. Por ejemplo, la recomendación "Los errores de estado de Endpoint Protection deberían corregirse..." se basa en la recomendación que comprueba si una solución de Endpoint Protection está incluso _instalada_ ("La solución de Endpoint Protection debe estar instalada..."). La recomendación subyacente _tiene_ una directiva.
> Limitar las directivas a únicamente la recomendación básica simplifica la administración de directivas.

## <a name="appservices-recommendations"></a><a name='recs-appservices'></a>Recomendaciones de AppServices

[!INCLUDE [asc-recs-appservices](../../includes/asc-recs-appservices.md)]

## <a name="compute-recommendations"></a><a name='recs-compute'></a>Recomendaciones de proceso

[!INCLUDE [asc-recs-compute](../../includes/asc-recs-compute.md)]

## <a name="container-recommendations"></a><a name='recs-container'></a>Recomendaciones del contenedor

[!INCLUDE [asc-recs-container](../../includes/asc-recs-container.md)]

## <a name="data-recommendations"></a><a name='recs-data'></a>Recomendaciones de datos

[!INCLUDE [asc-recs-data](../../includes/asc-recs-data.md)]

## <a name="identityandaccess-recommendations"></a><a name='recs-identityandaccess'></a>Recomendaciones de IdentityAndAccess

[!INCLUDE [asc-recs-identityandaccess](../../includes/asc-recs-identityandaccess.md)]

## <a name="iot-recommendations"></a><a name='recs-iot'></a>Recomendaciones de IoT

[!INCLUDE [asc-recs-iot](../../includes/asc-recs-iot.md)]

## <a name="networking-recommendations"></a><a name='recs-networking'></a>Recomendaciones de redes

[!INCLUDE [asc-recs-networking](../../includes/asc-recs-networking.md)]

## <a name="deprecated-recommendations"></a>Recomendaciones en desuso

|Recomendación|Descripción y directiva relacionada|severity|
|----|----|----|
|Se debe restringir el acceso a App Services|Cambie la configuración de red para restringir el acceso a App Services y denegar el tráfico entrante desde intervalos demasiado amplios.<br>(Directiva relacionada [versión preliminar]: Se debe restringir el acceso a App Services).|Alto|
|Se deben proteger las reglas de las aplicaciones web en los NSG de IaaS|Se ha protegido el grupo de seguridad de red (NSG) de las máquinas virtuales que ejecutan aplicaciones web con reglas de NSG que son demasiado permisivas con respecto a los puertos de la aplicación web.<br>(Directiva relacionada: Se deben proteger las reglas de NSG para las aplicaciones web en IaaS).|Alto|
|Las directivas de seguridad de pod deben definirse para reducir el vector de ataque mediante la eliminación de privilegios de aplicación innecesarios (versión preliminar)|Defina las directivas de seguridad de pod para reducir el vector de ataque mediante la eliminación de privilegios de aplicación innecesarios. Se recomienda configurar las directivas de seguridad de pod para que los pods solo puedan obtener acceso a los recursos a los que se les permita el acceso.<br>(Directiva relacionada [versión preliminar]: Las directivas de seguridad de pod deben definirse en los servicios de Kubernetes).|Media|
|Instalación de Azure Security Center para el módulo de seguridad de IoT para obtener más visibilidad en los dispositivos de IoT|Instale Azure Security Center para el módulo de seguridad de IoT con el fin de obtener una mayor visibilidad en los dispositivos de IoT.|Bajo|
|Las máquinas deben reiniciarse para aplicar las actualizaciones del sistema|Reinicie sus máquinas para aplicar las actualizaciones del sistema y protegerlas ante vulnerabilidades. (Directiva relacionada: Se deben instalar actualizaciones del sistema en las máquinas).|Media|
| El agente de supervisión debe instalarse en las máquinas.|Esta acción instala un agente de supervisión en las máquinas virtuales seleccionadas. Seleccione un área de trabajo a la que informará el agente. (Ninguna directiva relacionada)|Alto|
||||

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las recomendaciones, consulte:

- [¿Qué son las directivas de seguridad, las iniciativas y las recomendaciones?](security-policy-concept.md)
- [Examen de las recomendaciones de seguridad](review-security-recommendations.md)
