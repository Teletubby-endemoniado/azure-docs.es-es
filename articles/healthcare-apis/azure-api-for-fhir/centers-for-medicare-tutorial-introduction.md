---
title: 'Tutorial: Introducción a Centers for Fax and Fax Services (CMS): Azure API for FHIR'
description: En esta introducción se presenta una serie de tutoriales que pertenecen a la regla de interoperabilidad y acceso de pacientes de Center for Fax and Fax Services (CMS).
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 11/29/2021
ms.openlocfilehash: 1390ed2316c4891a23684223644bb976ff230fbc
ms.sourcegitcommit: 66b6e640e2a294a7fbbdb3309b4829df526d863d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2021
ms.locfileid: "133359671"
---
# <a name="centers-for-medicare-and-medicaid-services-cms-interoperability-and-patient-access-rule-introduction"></a>Introducción a la regla de interoperabilidad y acceso de pacientes de Centers for Centers for Fax and Fax Services (CMS)

En esta serie de tutoriales, se trata un resumen de alto nivel de la regla de interoperabilidad y acceso a los pacientes del Centro para La interoperabilidad y los servicios de Fax (CMS), así como los requisitos técnicos descritos en esta regla. Recorreremos las distintas guías de implementación a las que se hace referencia para esta regla. También se proporcionarán detalles sobre cómo configurar el Azure API for FHIR para admitir estas guías de implementación.


## <a name="rule-overview"></a>Introducción a las reglas

CMS publicó la [regla de interoperabilidad y](https://www.cms.gov/Regulations-and-Guidance/Guidance/Interoperability/index) acceso a pacientes el 1 de mayo de 2020. Esta regla requiere un flujo de datos gratuito y seguro entre todas las partes implicadas en la atención a los pacientes (pacientes, proveedores y pagadores) para permitir que los pacientes accedan a su información sanitaria cuando la necesiten. La interoperabilidad ha afectado al sector sanitario durante décadas, lo que ha dado lugar a datos almacenados en silos que provocan resultados sanitarios negativos con costos de atención más elevados e impredecibles. CMS usa su autoridad para regular a los emisores del Programa de seguros de salud de los niños (CHIP) y del Plan de salud calificado (QHP) en los intercambios facilitados por el gobierno federal (FFE) para aplicar esta regla. 

En agosto de 2020, CMS detalló cómo las organizaciones pueden cumplir el requisito. Para asegurarse de que los datos se pueden intercambiar de forma segura y estandarizada, CMS identificó la versión 4 (R4) de FHIR como el estándar fundamental necesario para el intercambio de datos. 

Hay tres partes principales de la interoperabilidad y el acceso a los pacientes:

* API de acceso de pacientes (obligatorio el 1 de julio de **2021):** los pagadores regulados por CMS (tal y como se definió anteriormente) deben implementar y mantener una API segura basada en estándares que permita a los pacientes acceder fácilmente a sus notificaciones y encontrar información, incluido el costo, así como un subconjunto definido de su información hospitalia a través de aplicaciones de terceros de su elección.  

* API de directorio del proveedor (obligatorio el 1 de julio de **2021):** esta parte de la regla requiere a los pagadores regulados por CMS que hagan que la información del directorio del proveedor esté disponible públicamente a través de una API basada en estándares. Al hacer que esta información esté disponible, los desarrolladores de aplicaciones de terceros podrán crear servicios que ayuden a los pacientes a encontrar proveedores para necesidades de atención específicas y a los médicos a buscar otros proveedores para la coordinación de la atención.  

* Datos de pago a pago Exchange (obligatorio el 1 de enero de **2022):** los pagadores regulados por CMS deben intercambiar determinados datos clínicos de pacientes a petición del paciente con otros pagadores. Aunque no hay ningún requisito para seguir ningún tipo de estándar, se recomienda aplicar FHIR para intercambiar estos datos. 

## <a name="key-fhir-concepts"></a>Conceptos clave de FHIR

Como se mencionó anteriormente, FHIR R4 es necesario para cumplir este requisito. Además, se han desarrollado varias guías de implementación que proporcionan instrucciones para la regla. [Las guías de](https://www.hl7.org/fhir/implementationguide.html) implementación proporcionan contexto adicional sobre la especificación base de FHIR. Esto incluye la definición de parámetros de búsqueda, perfiles, extensiones, operaciones, conjuntos de valores y sistemas de código adicionales.

El Azure API for FHIR tiene las siguientes funcionalidades para ayudarle a configurar la base de datos para las distintas guías de implementación:

* [Compatibilidad con interacciones RESTful](fhir-features-supported.md)
* [Almacenamiento y validación de perfiles](validation-against-profiles.md)
* [Definición e indexación de parámetros de búsqueda personalizados](how-to-do-custom-search.md)
* [Convertir datos](convert-data.md)

## <a name="patient-access-api-implementation-guides"></a>Guías de implementación de api de acceso de pacientes

Patient Access API describe el cumplimiento de cuatro guías de implementación de FHIR:

* [CARIN IG](http://hl7.org/fhir/us/carin-bb/STU1/index.html)para el botón azul® : los pagadores deben hacer las notificaciones de los pacientes y encuentra datos disponibles según la Guía de implementación del botón azul de CARIN IG (C4BB IG). C4BB IG proporciona un conjunto de recursos que los pagadores pueden mostrar a los consumidores a través de una API de FHIR e incluye los detalles necesarios para los datos de notificaciones en Interoperability and Patient Access API. En esta guía de implementación se usa el recurso ExplanationOfBenefit (EOB) como recurso principal y se extraen otros recursos a medida que se hace referencia a ellos.
* [HL7 FHIR DaHl PDex IG](http://hl7.org/fhir/us/davinci-pdex/STU1/index.html)Exchange: la guía de implementación de PDex IG (PDex IG) de datos de pago se centra en garantizar que los pagadores proporcionen todos los datos clínicos pertinentes de los pacientes para cumplir los requisitos de la API de acceso de pacientes. Esto usa los perfiles de US Core en recursos R4 e incluye (como mínimo) encuentros, proveedores, organizaciones, ubicaciones, fechas de servicio, diagnósticos, procedimientos y observaciones. Aunque estos datos pueden estar disponibles en formato FHIR, también pueden proceder de otros sistemas con el formato de datos de notificaciones, mensajes HL7 V2 y documentos C-CDA.
* [HL7 US Core IG:](https://www.hl7.org/fhir/us/core/toc.html)la Guía de implementación de HL7 US Core (US Core IG) es la red troncal de la IG de PDex descrita anteriormente. Aunque la IG de PDex limita algunos recursos incluso más allá de LA IG principal de EE. UU., muchos recursos solo siguen los estándares de LA IG principal de EE. UU.

* [HL7 FHIR DaHl - PDex US Drug Formulary IG](http://hl7.org/fhir/us/Davinci-drug-formulary/index.html): Part D Planes De Advantage de Hl7 FHIR DaHl tienen que hacer que la información de los formularios esté disponible a través de patient API. Para ello, usan la Guía de implementación de formularios de productos de estados unidos PDex (USDF IG). La IG de USDF define una interfaz FHIR para la información de los formularios de salud de una médica, que es una lista de productos genéricos y de nombre de marca que una médica está de acuerdo en pagar. El caso de uso principal de esto es para que los pacientes puedan comprender si hay algún tratamiento alternativo disponible para uno que se les haya prescrito y para comparar los costos de los pacientes.

## <a name="provider-directory-api-implementation-guide"></a>Guía de implementación de API de directorio de proveedor

La API de directorio del proveedor describe el cumplimiento de una guía de implementación:

* [HL7 DaDex Plan Network IG:](http://build.fhir.org/ig/HL7/davinci-pdex-plan-net/)esta guía de implementación define una interfaz FHIR para los planes de seguros de una entidad sanitaria, sus redes asociadas y las organizaciones y proveedores que participan en estas redes.

## <a name="touchstone"></a>Touchstone

Para probar el cumplimiento de las distintas guías de implementación, [Touchstone](https://touchstone.aegis.net/touchstone/) es un excelente recurso. A lo largo de los próximos tutoriales, nos centraremos en garantizar que el Azure API for FHIR esté configurado para pasar correctamente varias pruebas de Touchstone. El sitio de Touchstone tiene una gran cantidad de documentación excelente para ayudarle a empezar a funcionar.

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una comprensión básica de la regla de interoperabilidad y acceso a pacientes, las guías de implementación y la herramienta de pruebas disponibles (Touchstone), le guiará por la configuración del Azure API for FHIR para carin IG para el botón azul. 

>[!div class="nextstepaction"]
>[Guía de implementación de CARIN para Blue Button](carin-implementation-guide-blue-button-tutorial.md)  