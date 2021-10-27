---
title: ¿Qué es Anomaly Detector API?
titleSuffix: Azure Cognitive Services
description: Use los algoritmos de Anomaly Detector API para aplicar la detección de anomalías a los datos de serie temporal.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: overview
ms.date: 02/16/2021
ms.author: mbullwin
keywords: anomaly detection, machine learning, algorithms
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: 24d30a475071667fa9f701776302aa81c652fbe2
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2021
ms.locfileid: "130045054"
---
# <a name="what-is-the-anomaly-detector-univariate-api"></a>¿Qué es la API univariante Anomaly Detector?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Anomaly Detector API permite supervisar y detectar anomalías en los datos de serie temporal sin tener que saber aprendizaje automático. Los algoritmos de Anomaly Detector API se adaptan al identificar y aplicar automáticamente los modelos más idóneos para sus datos, con independencia del sector, el escenario o el volumen de datos. Con el uso de datos de serie temporal, la API determina los límites de la detección de anomalías, los valores esperados y qué puntos de datos son anomalías.

![Detectar cambios de patrón en las solicitudes de servicio](./media/anomaly_detection2.png)

Para usar Anomaly Detector, no se necesita ninguna experiencia anterior con el aprendizaje automático, y la API REST le permite integrar fácilmente el servicio en sus aplicaciones y procesos.

Esta documentación contiene los siguientes tipos de artículos:
* Los [inicios rápidos](./Quickstarts/client-libraries.md) son instrucciones paso a paso que permiten realizar llamadas al servicio y obtener los resultados en un breve período de tiempo. 
* Las [guías paso a paso](./how-to/identify-anomalies.md) contienen instrucciones para usar el servicio de maneras más específicas o personalizadas.
* Los [artículos conceptuales](./concepts/anomaly-detection-best-practices.md) proporcionan explicaciones detalladas de la funcionalidad y las características del servicio.
* Los [tutoriales](./tutorials/batch-anomaly-detection-powerbi.md) son guías más largas que muestran cómo usar este servicio como componente en soluciones empresariales más amplias.

## <a name="features"></a>Características

Con Anomaly Detector, puede detectar automáticamente las anomalías existentes en los datos de serie temporal, o cuando se producen en tiempo real.

|Característica  |Descripción  |
|---------|---------|
|Detección de anomalías en tiempo real. | Detecte anomalías en sus datos de transmisión mediante los puntos de datos vistos anteriormente para determinar si el último de ellos es una anomalía. Esta operación genera un modelo con los puntos de datos enviados y determina si el punto de destino es una anomalía. La llamada a la API con cada nuevo punto de datos que genera le permite supervisar los datos que se crean. |
|Detección de anomalías en todo el conjunto de datos como un lote | Use la serie temporal para detectar las anomalías que podrían existir en los datos. Esta operación genera un modelo con todos los datos de serie temporal, donde cada punto de analiza con el mismo modelo.         |
|Detecte los puntos de cambio en el conjunto de datos como un lote. | Use la serie temporal para detectar los puntos de cambio de tendencia que existan en los datos. Esta operación genera un modelo con todos los datos de serie temporal, donde cada punto de analiza con el mismo modelo.    |
| Obtención de información adicional sobre los datos | Obtenga útiles detalles sobre los datos y las anomalías observadas, como los valores esperados, los límites de anomalías y las posiciones. |
| Ajuste de los límites de detección de anomalías | Anomaly Detector API crea automáticamente límites para la detección de anomalías. Configure estos límites para aumentar o disminuir la sensibilidad de la API a las anomalías de datos y ajustar mejor los datos. |

## <a name="demo"></a>Demostración

Consulte esta [demostración interactiva](https://aka.ms/adDemo) para comprender cómo funciona Anomaly Detector.
Para ejecutar la demostración, debe crear un recurso de Anomaly Detector y obtener la clave de API y el punto de conexión.

## <a name="notebook"></a>Notebook

Para saber cómo llamar a Anomaly Detector API, pruebe este [cuaderno](https://aka.ms/adNotebook). En esta instancia de Jupyter Notebook se muestra cómo enviar una solicitud de API y visualizar el resultado.

Para ejecutar el cuaderno, debe obtener una **clave de suscripción** de Anomaly Detector API y un **punto de conexión de API**. En el cuaderno, agregue la clave de suscripción de Anomaly Detector API válida a la variable `subscription_key` y cambie la variable `endpoint` al punto de conexión.

## <a name="workflow"></a>Flujo de trabajo

Anomaly Detector API es un servicio web RESTful, lo que significa que es fácil llamarlo desde cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar código JSON.

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

Después del registro:

1. Tome sus datos de serie temporal y conviértalos en un formato JSON válido. Use los [procedimientos recomendados](concepts/anomaly-detection-best-practices.md) al preparar los datos para obtener los mejores resultados.
1. Envíe una solicitud a Anomaly Detector API con sus datos.
1. Analice el mensaje JSON devuelto para procesar la respuesta de API.

## <a name="algorithms"></a>Algoritmos

* Consulte los siguientes blogs técnicos para obtener información acerca de los algoritmos usados:
    * [Introducing Azure Anomaly Detector API](https://techcommunity.microsoft.com/t5/AI-Customer-Engineering-Team/Introducing-Azure-Anomaly-Detector-API/ba-p/490162) (Introducción a Anomaly Detector API de Azure)
    * [Introducción al algoritmo SR-CNN de Azure Anomaly Detector](https://techcommunity.microsoft.com/t5/AI-Customer-Engineering-Team/Overview-of-SR-CNN-algorithm-in-Azure-Anomaly-Detector/ba-p/982798)

Puede leer el artículo [Time-Series Anomaly Detection Service at Microsoft](https://arxiv.org/abs/1906.03821) (Servicio de detección de anomalías de Time Series) (aceptado por KDD 2019) para conocer los algoritmos SR-CNN desarrollados por Microsoft.

> [!VIDEO https://www.youtube.com/embed/ERTaAnwCarM]

## <a name="service-availability-and-redundancy"></a>Disponibilidad y redundancia del servicio

### <a name="is-the-anomaly-detector-service-zone-resilient"></a>¿El servicio Anomaly Detector tiene resistencia de zona?

Sí. El servicio Anomaly Detector tiene resistencia de zona de manera predeterminada.

### <a name="how-do-i-configure-the-anomaly-detector-service-to-be-zone-resilient"></a>¿Cómo se configura el servicio Anomaly Detector para que tenga resistencia de zona?

No es necesaria ninguna configuración de parte del cliente para habilitar la resistencia de zonas. La resistencia de zona para los recursos de Anomaly Detector está disponible de manera predeterminada y la administra el propio servicio.

## <a name="deploy-on-premises-using-docker-containers"></a>Implementación local mediante contenedores de Docker

[Use contenedores de Anomaly Detector](anomaly-detector-container-howto.md) para implementar características de API de forma local. Los contenedores de Docker permiten acercar el servicio a los datos para mejorar el cumplimiento, la seguridad o por otras razones operativas.

## <a name="join-the-anomaly-detector-community"></a>Únase a la comunidad de Anomaly Detector

* Únase al [grupo de asesores de Anomaly Detector en Microsoft Teams](https://aka.ms/AdAdvisorsJoin)

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Uso de la biblioteca cliente de Anomaly Detector](quickstarts/client-libraries.md)
* [Demostración en línea](https://github.com/Azure-Samples/AnomalyDetector/tree/master/ipython-notebook) de Anomaly Detector API
* [Referencia de la API REST](https://aka.ms/anomaly-detector-rest-api-ref) de Anomaly Detector