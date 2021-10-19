---
title: Configuración del uso compartido de recursos entre orígenes en el servicio FHIR
description: En este artículo se describe cómo configurar el uso compartido de recursos entre orígenes en el servicio FHIR
author: matjazl
ms.author: zxue
ms.date: 08/03/2021
ms.topic: reference
ms.service: healthcare-apis
ms.subservice: fhir
ms.openlocfilehash: e322b1fb848f9156b1d00d2d1ae051ef62536c4a
ms.sourcegitcommit: 28cd7097390c43a73b8e45a8b4f0f540f9123a6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2021
ms.locfileid: "122778893"
---
# <a name="configure-cross-origin-resource-sharing-in-fhir-service"></a>Configuración del uso compartido de recursos entre orígenes en el servicio FHIR

> [!IMPORTANT]
> Azure Healthcare APIs se encuentra actualmente en VERSIÓN PRELIMINAR. Los [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) incluyen términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado con disponibilidad general.

El servicio FHIR en las API de Azure Healthcare (aquí denominado servicio FHIR) admite el uso compartido de recursos entre orígenes [(CORS).](https://wikipedia.org/wiki/Cross-Origin_Resource_Sharing) CORS le permite configurar los parámetros para que las aplicaciones de un dominio (origen) puedan acceder a los recursos de un dominio diferente, lo que se conoce como una solicitud entre dominios.

A menudo se usa CORS en una aplicación de una única página que debe llamar a una API de RESTful a un dominio diferente.

Para configurar una configuración de CORS en el servicio FHIR, especifique la siguiente configuración:

- **Orígenes (Access-Control-Allow-Origin)** . Lista de dominios permitidos para realizar solicitudes entre orígenes al servicio FHIR. Cada dominio (origen) debe especificarse en una línea independiente. Puede indicar un asterisco (*) para permitir las llamadas de cualquier dominio, pero no se aconseja porque es un riesgo de seguridad.

- **Encabezados (Access-Control-Allow-Headers)** . Una lista de encabezados que contendrá la solicitud de origen. Para permitir todos los encabezados, especifique un asterisco (*).

- **Métodos (Access-Control-Allow-Methods)** . Los métodos permitidos (PUT, GET, POST, etc.) en una llamada API. Elija **Seleccionar todo** para todos los métodos.

- **Antigüedad máxima (Access-Control-Max-Age)** . El valor en segundos para almacenar en caché los resultados de la solicitud preparatoria para Access-Control-Allow-Headers y Access-Control-Allow-Methods.

- **Permitir credenciales (Access-Control-Allow-Credentials)** . Las solicitudes CORS normalmente no incluyen las cookies para evitar los ataques de [falsificación de solicitud entre sitios](https://en.wikipedia.org/wiki/Cross-site_request_forgery). Si selecciona esta opción, la solicitud se puede realizar para incluir credenciales, como las cookies. No se puede configurar esta opción si ya ha establecido Orígenes con un asterisco (*).

![Configuración del uso compartido de recursos entre orígenes (CORS)](media/cors/cors.png)

>[!NOTE]
>No se puede especificar distintas configuraciones para orígenes de dominio diferentes. Todas las configuraciones (**Encabezados**, **Métodos**, **Antigüedad máxima** y **Permitir credenciales**) se aplican a todos los orígenes especificados en la configuración Orígenes.