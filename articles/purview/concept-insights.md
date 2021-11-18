---
title: Descripción de los informes de Insights en Azure Purview
description: En este artículo se explica qué es la característica Insights en Azure Purview.
author: SunetraVirdi
ms.author: suvirdi
ms.service: purview
ms.subservice: purview-insights
ms.topic: conceptual
ms.date: 12/02/2020
ms.openlocfilehash: 405be2ec031e95a6c59128b5f23310a4c0a711ac
ms.sourcegitcommit: 512e6048e9c5a8c9648be6cffe1f3482d6895f24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2021
ms.locfileid: "132157948"
---
# <a name="understand-insights-in-azure-purview"></a>Descripción de Insights en Azure Purview

En este artículo se proporciona información general sobre la característica Insights en Azure Purview.

Insights es uno de los pilares principales de Purview. Esta característica proporciona a los clientes una visualización sencilla del catálogo en un único panel y, además, proporciona información específica a los administradores de los orígenes de datos, los usuarios empresariales, los administradores de datos, los responsables de datos y los administradores de seguridad. Actualmente, Purview cuenta con los siguientes informes de Insights que estarán disponibles para los clientes en la versión preliminar pública de Insights.

> [!IMPORTANT]
> Insights de Azure Purview se encuentra actualmente en versión preliminar. Los [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) incluyen términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

## <a name="asset-insights"></a>Información de recursos

Este informe proporciona una vista general de los datos y su distribución por tipo de origen, por clasificación y por tamaño de archivo, así como algunas dimensiones. Este informe sirve para diferentes tipos de partes interesadas en los roles de catalogación y gobernanza de datos que están interesados en conocer el estado de su DataMap, por clasificación y extensiones de archivo.

El informe proporciona amplios detalles a través de gráficos y KPI, para profundizar más delante en anomalías específicas, como archivos en ubicaciones incorrectas. El informe también admite una experiencia de cliente de un extremo a otro, gracias a la cual el cliente puede ver el recuento de recursos con una clasificación específica, puede desglosar la información por tipos de fuente y carpetas principales, y también puede ver la lista de recursos para realizar una investigación más detallada.

> [!NOTE]
> La información sobre extensiones de archivo se ha combinado con la información sobre recursos dando lugar a un informe enriquecido sobre tendencias que muestra el crecimiento en el tamaño de los datos por extensión de archivo. Para más información, consulte [Información sobre recursos](asset-insights.md). 
>
>

## <a name="scan-insights"></a>Información de exámenes

Este informe permite a los administradores del origen de datos comprender el estado general de los exámenes: cuántos se han realizado correctamente, cuántos no se han podido realizar y cuántos se han cancelado. Asimismo, el informe proporciona una actualización de estado de los exámenes que se han ejecutado en la cuenta de Purview durante los últimos siete días o los últimos 30 días.

El informe también permite a los administradores profundizar y explorar qué exámenes han devuelto errores y en qué tipos de origen específicos. Para que los usuarios puedan investigar aún más, el informe les permite navegar a la página del historial de exámenes de la experiencia "Orígenes".

## <a name="glossary-insights"></a>Información de glosarios

En este informe se proporciona a los administradores de datos un informe de estado sobre el glosario. Los administradores de datos pueden ver este informe para comprender la distribución de los términos del glosario por estado, conocer cuántos términos del glosario están asociados a los recursos y cuántos aún no se han adjuntado a ningún recurso. Igualmente, los usuarios empresariales también pueden obtener información sobre la integridad de los términos del glosario. 

Este informe resume los elementos principales en los que un administrador de datos tiene que centrarse para crear un glosario completo y utilizable para su organización. Los administradores también pueden ir a la experiencia "Glosario" de la sección "Información de glosarios", si quieren realizar cambios en un término específico del glosario.

## <a name="classification-insights"></a>Información de la clasificación

En este informe se proporcionan detalles acerca de dónde se encuentran los datos clasificados, las clasificaciones detectadas durante un examen, y obtiene los detalles de los propios archivos clasificados. Permite a los administradores de datos, administradores provisionales y administradores de seguridad comprender los tipos de información que se encuentran en los datos de su organización. 

En Azure Purview, las clasificaciones son similares a las etiquetas de asunto y se usan para marcar e identificar el contenido de un tipo específico de los datos.

Use el informe de Información de la clasificación para identificar el contenido con clasificaciones específicas y comprender acciones necesarias como agregar seguridad adicional a los repositorios o mover el contenido a una ubicación más segura.

Para obtener más información, consulte [Información de la clasificación sobre los datos de Azure Purview](classification-insights.md).

## <a name="sensitivity-labeling-insights"></a>Información de la etiqueta de confidencialidad

En este informe se proporcionan detalles acerca de las etiquetas de confidencialidad que se encuentran durante un examen, así como los detalles de los propios archivos etiquetados. Permite a los administradores de seguridad garantizar la seguridad de la información que se encuentra en los datos de la organización. 

En Azure Purview, las etiquetas de confidencialidad se usan para identificar categorías de tipos de clasificación, así como el grupo de las directivas de seguridad que quiere aplicar en cada categoría.

Use el informe de Información de la etiqueta para identificar las etiquetas de confidencialidad que se encuentran en el contenido y comprender acciones necesarias como administrar el acceso a repositorios o a archivos específicos.

Para obtener más información, consulte [Información de la etiqueta de confidencialidad sobre los datos de Azure Purview](sensitivity-insights.md).

## <a name="next-steps"></a>Pasos siguientes

* [Información de glosarios](glossary-insights.md)
* [Conclusiones de exámenes](scan-insights.md)
* [Información de la clasificación](./classification-insights.md)
