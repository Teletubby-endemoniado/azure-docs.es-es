---
title: Uso de Text Analytics para el estado
titleSuffix: Azure Cognitive Services
description: Aprenda a extraer y etiquetar la información médica de textos clínicos no estructurados con Text Analytics para el estado.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 09/16/2021
ms.author: aahi
ms.openlocfilehash: d86e9d5cbd9168625f71ce7b4ebb0be598d00557
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128677429"
---
# <a name="how-to-use-text-analytics-for-health"></a>Uso de Text Analytics para el sector sanitario

> [!IMPORTANT] 
> Text Analytics para el sector sanitario es una funcionalidad que se proporciona "tal cual" y "con todos los errores". Text Analytics para el estado no está previsto ni está disponible para su uso como dispositivo médico, soporte clínico, herramienta de diagnóstico u otra tecnología que se pretenda usar en el diagnóstico, la cura, la mitigación, el tratamiento o la prevención de la enfermedad u otras condiciones, y Microsoft no concede ninguna licencia o derecho para usar esta funcionalidad con este fin. Esta funcionalidad no está diseñada o prevista para implementarse como un sustituto del asesoramiento médico profesional o la opinión de la atención sanitaria, el diagnóstico, el tratamiento o la opinión clínica de un profesional sanitario y no debe usarse como tal. El cliente es el único responsable de cualquier uso de Text Analytics para el estado. El cliente debe obtener una licencia por separado de todos y cada uno de los vocabularios fuente que va a usar en virtud de los términos establecidos en el [Apéndice al contrato de licencia del metatesauro de UMLS](https://www.nlm.nih.gov/research/umls/knowledge_sources/metathesaurus/release/license_agreement_appendix.html) o cualquier vínculo equivalente futuro. El cliente es responsable de garantizar el cumplimiento de esos términos de licencia, incluidas las restricciones geográficas u otras restricciones aplicables.


Text Analytics for Health es una característica de la API Text Analytics, que extrae y etiqueta información médica pertinente de textos no estructurados, como notas del doctor, resúmenes de altas médicas, historiales clínicos y registros electrónicos de salud.  Hay dos maneras de utilizar este servicio: 

* [La API basada en web (asincrónica)](#structure-the-api-request-for-the-hosted-asynchronous-web-api)
* [Un contenedor de Docker (sincrónica)](#hosted-asynchronous-web-api-response)   

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Introducing-Text-Analytics-for-Health/player]

## <a name="features"></a>Características

Text Analytics for Health realiza el reconocimiento de entidades con nombre (NER), la extracción de relaciones, la negación de entidades y la vinculación de entidades en el texto en inglés para revelar información en texto clínico y biomédico sin estructurar.

### <a name="named-entity-recognition"></a>[Reconocimiento de entidades con nombre](#tab/ner)

El reconocimiento de entidades con nombre detecta palabras y frases mencionadas en texto no estructurado que se pueden asociar a uno o varios tipos semánticos, como el diagnóstico, el nombre de los medicamentos, el síntoma, el signo o la edad.

> [!div class="mx-imgBorder"]
> ![Health NER](../media/ta-for-health/health-named-entity-recognition.png)

### <a name="relation-extraction"></a>[Extracción de relaciones](#tab/relation-extraction)

La extracción de relaciones identifica conexiones significativas entre los conceptos mencionados en el texto. Por ejemplo, una relación de "hora de condición" se encuentra al asociar un nombre de condición con una hora o entre una abreviatura y la descripción completa.  

> [!div class="mx-imgBorder"]
> ![Health RE](../media/ta-for-health/health-relation-extraction.png)


### <a name="entity-linking"></a>[Entity Linking](#tab/entity-linking)

La vinculación de entidad anula las distintas entidades al asociar las entidades con nombre mencionadas en el texto a los conceptos que se encuentran en una base de datos de conceptos predefinida, incluido el Sistema de Lenguaje Médico Unificado (UMLS). Los conceptos médicos también tienen asignada una denominación preferida, como forma adicional de normalización.

> [!div class="mx-imgBorder"]
> ![Health EL](../media/ta-for-health/health-entity-linking.png)

Text Analytics para el estado admite la vinculación a los vocabularios biomédicos y de salud que se encuentran en el origen de la información del metadiccionario de sinónimos del Sistema unificado de lenguaje médico ([UMLS](https://www.nlm.nih.gov/research/umls/sourcereleasedocs/index.html)).

### <a name="assertion-detection"></a>[Detección de aserciones](#tab/assertion-detection) 

El significado del contenido médico se ve muy afectado por los modificadores, como las aserciones negativas o condicionales, que pueden tener implicaciones críticas si se interpretan de manera errónea. Text Analytics for Health admite tres categorías de detección de aserciones para entidades del texto: 

* certeza
* condicionales
* correlación

> [!div class="mx-imgBorder"]
> ![Health NEG](../media/ta-for-health/assertions.png)

---

Consulte las [categorías de entidad](../named-entity-types.md?tabs=health) devueltas por Text Analytics para el estado a fin de obtener una lista completa de las entidades admitidas. Para obtener información sobre las puntuaciones de confianza, vea la [nota de transparencia de Text Analytics](/legal/cognitive-services/text-analytics/transparency-note#general-guidelines-to-understand-and-improve-performance?context=/azure/cognitive-services/text-analytics/context/context). 

### <a name="supported-languages"></a>Idiomas compatibles

Text Analytics para el estado solo admite documentos en inglés. 

## <a name="using-the-docker-container"></a>Uso del contenedor de Docker 

Para ejecutar el contenedor de Text Analytics for Heath en su propio entorno, siga estas [instrucciones para descargar e instalar el contenedor](../how-tos/text-analytics-how-to-install-containers.md?tabs=healthcare).

## <a name="using-the-client-library"></a>Uso de la biblioteca cliente

La versión preliminar más reciente de la biblioteca cliente de Text Analytics permite llamar a Text Analytics for Health utilizando esta biblioteca. Consulte la documentación de referencia y vea los ejemplos en GitHub:
* [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics)
* [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/)
* [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)



## <a name="sending-a-rest-api-request"></a>Envío de una solicitud de API REST 

### <a name="preparation"></a>Preparación

Debe tener documentos JSON en este formato: identificador, texto e idioma. 

El tamaño del documento debe ser inferior a 5120 caracteres por documento. Para conocer el número máximo de documentos permitidos en una colección, consulte el artículo sobre [límites de datos](../concepts/data-limits.md?tabs=version-3) en Conceptos. La colección se envía en el cuerpo de la solicitud. Si el texto supera este límite, considere la posibilidad de dividirlo en solicitudes independientes. Para obtener mejores resultados, divida el texto entre oraciones.

### <a name="structure-the-api-request-for-the-hosted-asynchronous-web-api"></a>Estructura de la solicitud de API para la API web asincrónica hospedada

Tanto para el contenedor como para la API web hospedada, debe crear una solicitud POST. Puede [usar Postman](text-analytics-how-to-call-api.md), un comando de cURL o la **consola de prueba de API** en la [referencia de la API hospedada Text Analytics for Health](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/Health) para construir y enviar rápidamente una solicitud POST a la API web hospedada en la región deseada. En el punto de conexión de API v3.1, se puede usar el parámetro de consulta booleano `loggingOptOut` para habilitar el registro con fines de solución de problemas.  El valor predeterminado es TRUE si no se especifica en la consulta de solicitud.

Envíe la solicitud POST a `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1/entities/health/jobs`. A continuación hay un ejemplo de un archivo JSON adjunto al cuerpo de POST de la solicitud de la API Text Analytics for Health:

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Subject was administered 100mg remdesivir intravenously over a period of 120 min"
    }
  ]
}
```

### <a name="hosted-asynchronous-web-api-response"></a>Respuesta de API web asincrónica hospedada 

Como esta solicitud POST se usa para enviar un trabajo para la operación asincrónica, no hay texto en el objeto de la respuesta.  Sin embargo, necesita el valor de KEY de operation-location en los encabezados de respuesta para realizar una solicitud GET para comprobar el estado del trabajo y la salida.  A continuación se muestra un ejemplo del valor de la KEY de operation-location en el encabezado de respuesta de la solicitud POST:

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1/entities/health/jobs/<jobID>`

Para comprobar el estado del trabajo, realice una solicitud GET a la dirección URL en el valor del encabezado KEY DE operation-Location de la respuesta POST.  Los siguientes estados se usan para reflejar el estado de un trabajo: `NotStarted`, `running`, `succeeded`, `failed`, `rejected`, `cancelling` y `cancelled`.  

Puede cancelar un trabajo con los estados `NotStarted` o `running` con una llamada HTTP DELETE a la misma dirección URL que la solicitud GET.  Puede encontrar más información sobre la llamada de eliminación en la [referencia de la API hospedada Text Analytics for Health](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1/operations/CancelHealthJob).

El siguiente es un ejemplo de la respuesta de una solicitud GET.  La salida está disponible para su recuperación hasta que transcurre el valor de `expirationDateTime` (24 horas desde el momento en que se creó el trabajo), tras lo cual se purga la salida.

```json
{
    "jobId": "69081148-055b-4f92-977d-115df343de69",
    "lastUpdateDateTime": "2021-07-06T19:06:03Z",
    "createdDateTime": "2021-07-06T19:05:41Z",
    "expirationDateTime": "2021-07-07T19:05:41Z",
    "status": "succeeded",
    "errors": [],
    "results": {
        "documents": [
            {
                "id": "1",
                "entities": [
                    {
                        "offset": 25,
                        "length": 5,
                        "text": "100mg",
                        "category": "Dosage",
                        "confidenceScore": 1.0
                    },
                    {
                        "offset": 31,
                        "length": 10,
                        "text": "remdesivir",
                        "category": "MedicationName",
                        "confidenceScore": 1.0,
                        "name": "remdesivir",
                        "links": [
                            {
                                "dataSource": "UMLS",
                                "id": "C4726677"
                            },
                            {
                                "dataSource": "DRUGBANK",
                                "id": "DB14761"
                            },
                            {
                                "dataSource": "GS",
                                "id": "6192"
                            },
                            {
                                "dataSource": "MEDCIN",
                                "id": "398132"
                            },
                            {
                                "dataSource": "MMSL",
                                "id": "d09540"
                            },
                            {
                                "dataSource": "MSH",
                                "id": "C000606551"
                            },
                            {
                                "dataSource": "MTHSPL",
                                "id": "3QKI37EEHE"
                            },
                            {
                                "dataSource": "NCI",
                                "id": "C152185"
                            },
                            {
                                "dataSource": "NCI_FDA",
                                "id": "3QKI37EEHE"
                            },
                            {
                                "dataSource": "NDDF",
                                "id": "018308"
                            },
                            {
                                "dataSource": "RXNORM",
                                "id": "2284718"
                            },
                            {
                                "dataSource": "SNOMEDCT_US",
                                "id": "870592005"
                            },
                            {
                                "dataSource": "VANDF",
                                "id": "4039395"
                            }
                        ]
                    },
                    {
                        "offset": 42,
                        "length": 13,
                        "text": "intravenously",
                        "category": "MedicationRoute",
                        "confidenceScore": 0.99
                    },
                    {
                        "offset": 73,
                        "length": 7,
                        "text": "120 min",
                        "category": "Time",
                        "confidenceScore": 0.98
                    }
                ],
                "relations": [
                    {
                        "relationType": "DosageOfMedication",
                        "entities": [
                            {
                                "ref": "#/results/documents/0/entities/0",
                                "role": "Dosage"
                            },
                            {
                                "ref": "#/results/documents/0/entities/1",
                                "role": "Medication"
                            }
                        ]
                    },
                    {
                        "relationType": "RouteOfMedication",
                        "entities": [
                            {
                                "ref": "#/results/documents/0/entities/1",
                                "role": "Medication"
                            },
                            {
                                "ref": "#/results/documents/0/entities/2",
                                "role": "Route"
                            }
                        ]
                    },
                    {
                        "relationType": "TimeOfMedication",
                        "entities": [
                            {
                                "ref": "#/results/documents/0/entities/1",
                                "role": "Medication"
                            },
                            {
                                "ref": "#/results/documents/0/entities/3",
                                "role": "Time"
                            }
                        ]
                    }
                ],
                "warnings": []
            }
        ],
        "errors": [],
        "modelVersion": "2021-05-15"
    }
}
```


### <a name="structure-the-api-request-for-the-container"></a>Estructuración de la solicitud de API para el contenedor

Puede [usar Postman](text-analytics-how-to-call-api.md) o la siguiente solicitud de cURL de ejemplo para enviar una consulta al contenedor que ha implementado, pero reemplace la variable `serverURL` por el valor adecuado.  Tenga en cuenta que la versión de la API en la dirección URL del contenedor no es la misma que la de la API hospedada.

```bash
curl -X POST 'http://<serverURL>:5000/text/analytics/v3.1/entities/health' --header 'Content-Type: application/json' --header 'accept: application/json' --data-binary @example.json

```

El siguiente JSON es un ejemplo de un archivo JSON adjunto al cuerpo POST de la solicitud de la API de Text Analytics para el estado:

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Patient reported itchy sores after swimming in the lake."
    },
    {
      "language": "en",
      "id": "2",
      "text": "Prescribed 50mg benadryl, taken twice daily."
    }
  ]
}
```

### <a name="container-response-body"></a>Cuerpo de respuesta del contenedor

El siguiente JSON es un ejemplo de cuerpo de respuesta de la API Text Analytics for Health desde la llamada sincrónica con contenedores:

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "offset": 25,
                    "length": 5,
                    "text": "100mg",
                    "category": "Dosage",
                    "confidenceScore": 1.0
                },
                {
                    "offset": 31,
                    "length": 10,
                    "text": "remdesivir",
                    "category": "MedicationName",
                    "confidenceScore": 1.0,
                    "name": "remdesivir",
                    "links": [
                        {
                            "dataSource": "UMLS",
                            "id": "C4726677"
                        },
                        {
                            "dataSource": "DRUGBANK",
                            "id": "DB14761"
                        },
                        {
                            "dataSource": "GS",
                            "id": "6192"
                        },
                        {
                            "dataSource": "MEDCIN",
                            "id": "398132"
                        },
                        {
                            "dataSource": "MMSL",
                            "id": "d09540"
                        },
                        {
                            "dataSource": "MSH",
                            "id": "C000606551"
                        },
                        {
                            "dataSource": "MTHSPL",
                            "id": "3QKI37EEHE"
                        },
                        {
                            "dataSource": "NCI",
                            "id": "C152185"
                        },
                        {
                            "dataSource": "NCI_FDA",
                            "id": "3QKI37EEHE"
                        },
                        {
                            "dataSource": "NDDF",
                            "id": "018308"
                        },
                        {
                            "dataSource": "RXNORM",
                            "id": "2284718"
                        },
                        {
                            "dataSource": "SNOMEDCT_US",
                            "id": "870592005"
                        },
                        {
                            "dataSource": "VANDF",
                            "id": "4039395"
                        }
                    ]
                },
                {
                    "offset": 42,
                    "length": 13,
                    "text": "intravenously",
                    "category": "MedicationRoute",
                    "confidenceScore": 1.0
                },
                {
                    "offset": 73,
                    "length": 7,
                    "text": "120 min",
                    "category": "Time",
                    "confidenceScore": 0.94
                }
            ],
            "relations": [
                {
                    "relationType": "DosageOfMedication",
                    "entities": [
                        {
                            "ref": "#/documents/0/entities/0",
                            "role": "Dosage"
                        },
                        {
                            "ref": "#/documents/0/entities/1",
                            "role": "Medication"
                        }
                    ]
                },
                {
                    "relationType": "RouteOfMedication",
                    "entities": [
                        {
                            "ref": "#/documents/0/entities/1",
                            "role": "Medication"
                        },
                        {
                            "ref": "#/documents/0/entities/2",
                            "role": "Route"
                        }
                    ]
                },
                {
                    "relationType": "TimeOfMedication",
                    "entities": [
                        {
                            "ref": "#/documents/0/entities/1",
                            "role": "Medication"
                        },
                        {
                            "ref": "#/documents/0/entities/3",
                            "role": "Time"
                        }
                    ]
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2021-03-01"
}
```

### <a name="assertion-output"></a>Salida de aserciones

Text Analytics for Health devuelve modificadores de aserciones, que son atributos informativos asignados a conceptos médicos que proporcionan una comprensión más profunda del contexto de los conceptos en el texto. Estos modificadores se dividen en tres categorías, cada uno de las cuales se centra en un aspecto diferente y contiene un conjunto de valores mutuamente excluyentes. Solo se asigna un valor por categoría a cada entidad. El valor más común de cada categoría es el valor predeterminado. La respuesta de salida del servicio solo contiene modificadores de aserciones que son diferentes del valor predeterminado.

**CERTAINTY**: proporciona información sobre la presencia (presencia frente a ausencia) del concepto y la certeza del texto en relación con su presencia (cierta frente a posible).
*   **Positive** [valor predeterminado]: el concepto existe o se ha producido.
* **Negative**: el concepto no existe o no se ha producido nunca.
* **Positive_Possible**: probablemente el concepto existe, pero existe alguna incertidumbre.
* **Negative_Possible**: la existencia del concepto es improbable pero existe alguna incertidumbre.
* **Neutral_Possible**: el concepto puede existir o no, sin tener a ninguna de estas opciones.

**CONDITIONALITY**: proporciona información sobre si la existencia de un concepto depende de determinadas condiciones. 
*   **None** [valor predeterminado]: el concepto es un hecho, no hipotético y no depende de determinadas condiciones.
*   **Hypothetical**: el concepto puede desarrollarse o producirse en el futuro.
*   **Conditional**: el concepto existe o solo se produce en determinadas condiciones.

**ASSOCIATION**: describe si el concepto está asociado al sujeto del texto o a otra persona.
*   **Subject** [valor predeterminado]: el concepto está asociado al sujeto del texto, normalmente el paciente.
*   **Someone_Else**: el concepto está asociado a alguien que no es el sujeto del texto.


La detección de aserciones representa las entidades negadas como un valor negativo para la categoría de certeza, por ejemplo:

```json
{
                        "offset": 381,
                        "length": 3,
                        "text": "SOB",
                        "category": "SymptomOrSign",
                        "confidenceScore": 0.98,
                        "assertion": {
                            "certainty&quot;: &quot;negative"
                        },
                        "name": "Dyspnea",
                        "links": [
                            {
                                "dataSource": "UMLS",
                                "id&quot;: &quot;C0013404"
                            },
                            {
                                "dataSource": "AOD",
                                "id&quot;: &quot;0000005442"
                            },
    ...
```

### <a name="relation-extraction-output"></a>Salida de la extracción de relaciones

Text Analytics for Health reconoce las relaciones entre distintos conceptos, incluidas las relaciones entre el atributo y la entidad (por ejemplo, la dirección de la estructura corporal, la dosis de medicación) y entre las entidades (por ejemplo, la detección de abreviaturas).

**ABBREVIATION**

**BODY_SITE_OF_CONDITION**

**BODY_SITE_OF_TREATMENT**

**COURSE_OF_CONDITION**

**COURSE_OF_EXAMINATION**

**COURSE_OF_MEDICATION**

**COURSE_OF_TREATMENT**

**DIRECTION_OF_BODY_STRUCTURE**

**DIRECTION_OF_CONDITION**

**DIRECTION_OF_EXAMINATION**

**DIRECTION_OF_TREATMENT**

**DOSAGE_OF_MEDICATION**

**EXAMINATION_FINDS_CONDITION**

**EXPRESSION_OF_GENE**

**EXPRESSION_OF_VARIANT**

**FORM_OF_MEDICATION**

**FREQUENCY_OF_CONDITION**

**FREQUENCY_OF_MEDICATION**

**FREQUENCY_OF_TREATMENT**

**MUTATION_TYPE_OF_GENE**

**MUTATION_TYPE_OF_VARIANT**

**QUALIFIER_OF_CONDITION**

**RELATION_OF_EXAMINATION**

**ROUTE_OF_MEDICATION** 

**SCALE_OF_CONDITION**

**TIME_OF_CONDITION**

**TIME_OF_EVENT**

**TIME_OF_EXAMINATION**

**TIME_OF_MEDICATION**

**TIME_OF_TREATMENT**

**UNIT_OF_CONDITION**

**UNIT_OF_EXAMINATION**

**VALUE_OF_CONDITION**  

**VALUE_OF_EXAMINATION**

**VARIANT_OF_GENE**

> [!NOTE]
> * Las relaciones que hacen referencia a CONDITION pueden referirse al tipo de entidad DIAGNOSIS o al tipo de entidad SYMPTOM_OR_SIGN.
> * Las relaciones que hacen referencia a MEDICATION pueden referirse al tipo de entidad MEDICATION_NAME o al tipo de entidad MEDICATION_CLASS.
> * Las relaciones que hacen referencia a TIME pueden referirse al tipo de entidad TIME o al tipo de entidad DATE.

La salida de la extracción de relaciones contiene referencias de URI y roles asignados de las entidades del tipo de relación. Por ejemplo:

```json
                "relations": [
                    {
                        "relationType": "DosageOfMedication",
                        "entities": [
                            {
                                "ref": "#/results/documents/0/entities/0",
                                "role": "Dosage"
                            },
                            {
                                "ref": "#/results/documents/0/entities/1",
                                "role": "Medication"
                            }
                        ]
                    },
                    {
                        "relationType": "RouteOfMedication",
                        "entities": [
                            {
                                "ref": "#/results/documents/0/entities/1",
                                "role": "Medication"
                            },
                            {
                                "ref": "#/results/documents/0/entities/2",
                                "role": "Route"
                            }
                        ]
...
]
```

## <a name="see-also"></a>Consulte también

* [Información general de Text Analytics](../overview.md)
* [Categorías de entidad con nombres](../named-entity-types.md)
* [Novedades](../whats-new.md)
