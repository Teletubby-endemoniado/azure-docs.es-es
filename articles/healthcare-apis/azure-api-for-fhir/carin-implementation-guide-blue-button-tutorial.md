---
title: CARIN Blue Button Implementation Guide for Blue Button - Azure API for FHIR
description: Este tutorial le guía por los pasos de configuración de Azure API for FHIR para pasar las pruebas de Touchstone para la Guía de implementación de CARIN para el botón azul (C4BB IG).
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 11/29/2021
ms.openlocfilehash: e89cc67df87d69d447dd4110c9bca5516fd5c464
ms.sourcegitcommit: 66b6e640e2a294a7fbbdb3309b4829df526d863d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2021
ms.locfileid: "133361860"
---
# <a name="carin-implementation-guide-for-blue-button174-for-azure-api-for-fhir"></a>CARIN Implementation Guide for Blue Button&#174; for Azure API for FHIR

En este tutorial, le guiaremos a través de la configuración de Azure API for FHIR para pasar las pruebas de [Touchstone](https://touchstone.aegis.net/touchstone/) para la Guía de implementación de [CARIN](https://build.fhir.org/ig/HL7/carin-bb/index.html) para el botón azul (C4BB IG).

## <a name="touchstone-capability-statement"></a>Instrucción de funcionalidad de Touchstone

La primera prueba en la que nos centraremos es probar Azure API for FHIR con la instrucción de funcionalidad [C4BB IG](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/CARIN/CARIN-4-BlueButton/00-Capability&activeOnly=false&contentEntry=TEST_SCRIPTS). Si ejecuta esta prueba en Azure API for FHIR sin actualizaciones, se producirá un error en la prueba debido a que faltan parámetros de búsqueda y perfiles que faltan. 

### <a name="define-search-parameters"></a>Definición de parámetros de búsqueda

Como parte de C4BB IG, deberá definir tres nuevos parámetros de [búsqueda](how-to-do-custom-search.md) para el `ExplanationOfBenefit` recurso. Dos de ellos se prueban en la instrucción de funcionalidad (tipo y fecha de servicio) y uno es necesario para `_include` las búsquedas (mutua).  

* [type](https://build.fhir.org/ig/HL7/carin-bb/SearchParameter-explanationofbenefit-type.json)
* [service-date](https://build.fhir.org/ig/HL7/carin-bb/SearchParameter-explanationofbenefit-service-date.json)
* [Asegurador](https://build.fhir.org/ig/HL7/carin-bb/SearchParameter-explanationofbenefit-insurer.json)

> [!NOTE]
> En el json sin formato para estos parámetros de búsqueda, el nombre se establece en `ExplanationOfBenefit_<SearchParameter Name>` . La prueba de Touchstone espera que el nombre de estos sea **el** tipo , **la fecha de servicio** y la **seguros**.  
 
El resto de los parámetros de búsqueda necesarios para C4BB IG se definen mediante la especificación base y ya están disponibles en Azure API for FHIR sin actualizaciones adicionales.
 
### <a name="store-profiles"></a>Almacenamiento de perfiles

Fuera de definir los parámetros de búsqueda, la otra actualización que debe realizar para superar esta prueba es cargar los [perfiles necesarios.](validation-against-profiles.md) Hay ocho perfiles definidos dentro de C4BB IG. 

* [Cobertura C4BB](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-Coverage.html) 
* [Explicación de C4BBOfBenefit Inpatient Leal](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-ExplanationOfBenefit-Inpatient-Institutional.html) 
* [Explicación de C4BBOfBenefit Outpatient Entrelazamiento](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-ExplanationOfBenefit-Outpatient-Institutional.html) 
* [Explicación de C4BBOfBenefit(10000001)](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-ExplanationOfBenefit-Pharmacy.html) 
* [C4BB ExplanationOfBenefit Professional NonCcian](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-ExplanationOfBenefit-Professional-NonClinician.html) 
* [Organización C4BB](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-Organization.html) 
* [C4BB Patient](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-Patient.html) 
* [Profesional de C4BB](https://build.fhir.org/ig/HL7/carin-bb/StructureDefinition-C4BB-Practitioner.html) 

### <a name="sample-rest-file"></a>Archivo de rest de ejemplo

Para ayudar a crear estos parámetros y perfiles de búsqueda, tenemos un archivo [HTTP](https://github.com/microsoft/fhir-server/blob/main/docs/rest/C4BB/C4BB.http) de ejemplo que incluye todos los pasos descritos anteriormente en un único archivo. Una vez que haya cargado todos los perfiles y parámetros de búsqueda necesarios, puede ejecutar la prueba de la instrucción de funcionalidad en Touchstone.

:::image type="content" source="media/cms-tutorials/capability-test-script-execution-results.png" alt-text="Resultados de la ejecución del script de prueba de funcionalidad.":::

## <a name="touchstone-read-test"></a>Prueba de lectura de Touchstone

Después de probar la instrucción capabilities, probaremos las [funcionalidades](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/CARIN/CARIN-4-BlueButton/01-Read&activeOnly=false&contentEntry=TEST_SCRIPTS) de lectura Azure API for FHIR con la IG de C4BB. Esta prueba está probando la conformidad con los ocho perfiles que cargó en la primera prueba. Deberá tener los recursos cargados que se ajusten a los perfiles. La mejor ruta de acceso sería probar con los recursos que ya tiene en la base de datos, pero también tenemos un archivo [HTTP](https://github.com/microsoft/fhir-server/blob/main/docs/rest/C4BB/C4BB_Sample_Resources.http) disponible con recursos de ejemplo que se extraían de los ejemplos de la IG que puede usar para crear los recursos y realizar pruebas.

:::image type="content" source="media/cms-tutorials/test-execution-results-touchstone.png" alt-text="Touchstone lee los resultados de la ejecución de pruebas.":::

## <a name="touchstone-eob-query-test"></a>Prueba de consulta EOB de Touchstone

La siguiente prueba que revisaremos es la [prueba de consulta EOB](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/CARIN/CARIN-4-BlueButton/02-EOBQuery&activeOnly=false&contentEntry=TEST_SCRIPTS). Si ya ha completado la prueba de lectura, tiene todos los datos cargados que necesitará. Esta prueba valida que puede buscar recursos y `Patient` específicos `ExplanationOfBenefit` mediante varios parámetros.

:::image type="content" source="media/cms-tutorials/test-execution-touchstone-eob-query-test.png" alt-text="Resultados de la ejecución de consultas EOB de Touchstone.":::

## <a name="touchstone-error-handling-test"></a>Prueba de control de errores de Touchstone

La prueba final que se va a recorrer es probar el [control de errores.](https://touchstone.aegis.net/touchstone/testdefinitions?selectedTestGrp=/FHIRSandbox/CARIN/CARIN-4-BlueButton/99-ErrorHandling&activeOnly=false&contentEntry=TEST_SCRIPTS) El único paso que debe realizar es eliminar un recurso ExplanationOfBenefit de la base de datos y usar el identificador del recurso `ExplanationOfBenefit` eliminado en la prueba.

:::image type="content" source="media/cms-tutorials/test-execution-touchstone-error-handling.png" alt-text="Resultados del control de errores de EOB de Touchstone.":::


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, se explica cómo pasar las pruebas de CARIN IG para el botón azul en Touchstone. A continuación, puede revisar cómo probar las pruebas del formulario de Da Tests.

>[!div class="nextstepaction"]
>[DaVinci Health Formulary](davinci-drug-formulary-tutorial.md)       
 
