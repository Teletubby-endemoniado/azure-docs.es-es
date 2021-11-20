---
title: Exportación de datos sin identificación (versión preliminar) para el servicio FHIR
description: En este artículo se indica cómo configurar y usar la exportación sin identificación.
author: ranvijaykumar
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 9/28/2020
ms.author: ranku
ms.openlocfilehash: a7757830dfb75f7ad111913ee4a8ea41926db600
ms.sourcegitcommit: b00a2d931b0d6f1d4ea5d4127f74fc831fb0bca9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2021
ms.locfileid: "132867381"
---
# <a name="exporting-de-identified-data-preview"></a>Exportación de datos sin identificación (versión preliminar)

> [!Note] 
> Los resultados cuando se usa la exportación sin identificación variarán en función de factores como los datos introducidos y las funciones seleccionadas por el cliente. Microsoft no puede evaluar las salidas de exportación sin identificación ni determinar la aceptabilidad para los casos de uso del cliente y las necesidades de cumplimiento. No se garantiza que la exportación sin identificación cumpla los requisitos legales, normativos o de cumplimiento específicos.

También se puede usar el comando $export para exportar datos sin identificación desde el servidor de FHIR. Usa el motor de anonimización de [Herramientas de FHIR para la anonimización](https://github.com/microsoft/FHIR-Tools-for-Anonymization) y toma los detalles de configuración de anonimización en los parámetros de la consulta. Puede crear su propio archivo de configuración de anonimización o usar el [archivo de configuración de ejemplo](https://github.com/microsoft/Tools-for-Health-Data-Anonymization/blob/master/docs/FHIR-anonymization.md#sample-configuration-file) para el método Safe Harbor de HIPAA como punto de partida. 

## <a name="configuration-file"></a>Archivo de configuración

El motor de anonimización incluye un archivo de configuración de ejemplo para ayudar a cumplir los requisitos de HIPAA Caja fuerte Método de puerto. El archivo de configuración es un archivo JSON con 4 secciones: `fhirVersion` , `processingErrors` , , `fhirPathRules` `parameters` . 
* `fhirVersion` especifica la versión de FHIR para el motor de anonimización.
* `processingErrors` especifica qué acción realizar para los errores de procesamiento que pueden surgir durante la anonimización. Puede generar _o_ _mantener las_ excepciones en función de sus necesidades.
* `fhirPathRules` especifica qué método de anonimización se va a usar. Las reglas se ejecutan en el orden de apariencia en el archivo de configuración.
* `parameters` establece reglas para los comportamientos de anonimización especificados _en fhirPathRules_.

Este es un archivo de configuración de ejemplo para R4:

```json
{
  "fhirVersion": "R4",
  "processingError":"raise",
  "fhirPathRules": [
    {"path": "nodesByType('Extension')", "method": "redact"},
    {"path": "Organization.identifier", "method": "keep"},
    {"path": "nodesByType('Address').country", "method": "keep"},
    {"path": "Resource.id", "method": "cryptoHash"},
    {"path": "nodesByType('Reference').reference", "method": "cryptoHash"},
    {"path": "Group.name", "method": "redact"}
  ],
  "parameters": {
    "dateShiftKey": "",
    "cryptoHashKey": "",
    "encryptKey": "",
    "enablePartialAgesForRedact": true
  }
}
```

Para obtener información más detallada sobre cada una de estas 4 secciones del archivo de configuración, consulte [aquí](https://github.com/microsoft/Tools-for-Health-Data-Anonymization/blob/master/docs/FHIR-anonymization.md#configuration-file-format).
## <a name="using-export-command-for-the-de-identified-data"></a>Uso $export comando para los datos no identificados
 `https://<<FHIR service base URL>>/$export?_container=<<container_name>>&_anonymizationConfig=<<config file name>>&_anonymizationConfigEtag=<<ETag on storage>>`

> [!Note] 
> En este momento, el servicio FHIR solo admite la exportación sin identificación en el nivel de sistema ($export).

|Parámetro de consulta            | Ejemplo |Opcionalidad| Descripción|
|---------------------------|---------|-----------|------------|
| _\_anonymizationConfig_   |DemoConfig.json|Obligatorio para la exportación sin identificación |Nombre del archivo de configuración. Consulte el formato del archivo de configuración [aquí](https://github.com/microsoft/FHIR-Tools-for-Anonymization#configuration-file-format). Este archivo debe mantenerse dentro de un contenedor llamado **anonymization** dentro de la misma cuenta de almacenamiento de Azure que está configurada como ubicación de exportación. |
| _\_anonymizationConfigEtag_|"0x8D8494A069489EC"|Opcional para la exportación sin identificación|Este es el valor de Etag del archivo de configuración. Puede obtener el valor de Etag mediante Explorador de Azure Storage desde la propiedad del blob.|

> [!IMPORTANT]
> Tanto la exportación sin formato como la exportación sin identificación escriben en la misma cuenta de almacenamiento de Azure especificada como parte de la configuración de la exportación. Se recomienda que use distintos contenedores que se correspondan con las diferentes configuraciones sin identificación y que administren el acceso de usuarios en el nivel de contenedor.
