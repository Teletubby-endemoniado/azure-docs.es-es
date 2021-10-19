---
title: 'Conceptos: Plantillas de asignación en el conector de Azure IoT para FHIR (versión preliminar) de Azure API for FHIR'
description: Aprenda a crear dos tipos de plantillas de asignación en el conector de Azure IoT para FHIR (versión preliminar). La plantilla de asignación de dispositivos transforma los datos del dispositivo en un esquema normalizado. La plantilla de asignación de FHIR transforma un mensaje normalizado en un recurso de tipo Observation basado en FHIR.
services: healthcare-apis
author: ms-puneet-nagpal
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: conceptual
ms.date: 04/05/2021
ms.author: rabhaiya
ms.openlocfilehash: 4b32bf7a505fb7ca3958d8add69e3deb25cee0a1
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124748743"
---
# <a name="azure-iot-connector-for-fhir-preview-mapping-templates"></a>Plantillas de asignación del conector de Azure IoT para FHIR (versión preliminar)
En este artículo se detalla cómo configurar el conector de Azure IoT para Recursos Rápidos de Interoperabilidad en Salud (FHIR&#174;)* mediante plantillas de asignación.

El conector de Azure IoT para FHIR requiere dos tipos de plantillas de asignación basadas en JSON. El primer tipo, **Device mapping** (Asignación de dispositivos), es responsable de la asignación de las cargas de dispositivo enviadas al punto de conexión `devicedata` de Azure Event Hub. Extrae tipos, identificadores de dispositivo, fecha y hora de medición, y valores de medida. El segundo tipo, **FHIR mapping** (Asignación de FHIR), controla la asignación para el recurso FHIR. Permite configurar la duración del período de observación, el tipo de datos FHIR que se usa para almacenar los valores y los códigos terminológicos. 

Las plantillas de asignación se redactan en un documento JSON en función del tipo. Estos documentos JSON se agregan al conector de Azure IoT para FHIR mediante Azure Portal. El documento Device mapping (Asignación de dispositivos) se agrega desde la página **Configure Device mapping** (Configurar asignación de dispositivos) y el documento FHIR mapping (Asignación de FHIR) desde la página **Configure FHIR mapping** (Configurar asignación de FHIR).

> [!NOTE]
> Las plantillas de asignación se almacenan en un almacenamiento de blobs subyacente y se cargan desde el blob con cada ejecución de proceso. Al actualizarse, se aplican inmediatamente. 

## <a name="device-mapping"></a>Asignación de dispositivos
La asignación de dispositivos proporciona funcionalidad para extraer el contenido del dispositivo en un formato común para su evaluación. Los mensajes que se reciben se comparan con todas las plantillas. Este enfoque permite que un único mensaje entrante se proyecte en varios mensajes salientes, que después se asignan a diferentes observaciones en FHIR. El resultado es un objeto de datos normalizado que representa los valores analizados con las plantillas. Es necesario buscar y extraer algunas propiedades obligatorias en el modelo de datos normalizado:

| Propiedad | Descripción |
| - | - |
|**Type**|Nombre/tipo para clasificar la medida. Este valor se usa para el enlace a la plantilla de asignación de FHIR obligatoria.  Varias plantillas pueden generar salidas del mismo tipo, lo que permite asignar representaciones diferentes de varios dispositivos a una sola salida común.|
|**OccurenceTimeUtc**|Hora a la que se realizó la medida.|
|**DeviceId**|Identificador del dispositivo. Este valor debe coincidir con un identificador del recurso de dispositivo que exista en el servidor de FHIR de destino.|
 |**Propiedades**|Extraiga al menos una propiedad para que el valor se pueda guardar en el recurso de observación creado.  Las propiedades son una colección de pares clave-valor que se extraen durante la normalización.|

A continuación se muestra un ejemplo conceptual de lo que sucede durante la normalización.

![Ejemplos de normalización](media/concepts-iot-mapping-templates/normalization-example.png)

La propia carga de contenido es un mensaje de Azure Event Hubs compuesto de tres partes: Body, Properties y SystemProperties. `Body` es una matriz de bytes que representa una cadena con codificación UTF-8. Durante la evaluación de la plantilla, la matriz de bytes se convierte automáticamente en el valor de cadena. `Properties` es una colección de valores clave que usa el creador del mensaje. `SystemProperties` también es una colección de valores clave reservada por el marco Azure Event Hubs con entradas que se rellenan automáticamente.

```json
{
    "Body": {
        "content": "value"
    },
    "Properties": {
        "key1": "value1",
        "key2": "value2"
    },
    "SystemProperties": {
        "x-opt-sequence-number": 1,
        "x-opt-enqueued-time": "2019-02-01T22:46:01.8750000Z",
        "x-opt-offset": 1,
        "x-opt-partition-key": "1"
    }
}
```

### <a name="mapping-with-json-path"></a>Asignación con ruta de acceso JSON
Los tres tipos de plantilla de contenido de dispositivo admitidos hoy en día dependen de que la ruta de acceso JSON coincida con la plantilla y los valores extraídos necesarios. Más información sobre la ruta de acceso JSON [aquí](https://goessner.net/articles/JsonPath/). Los tres tipos de plantilla usan la [implementación de .NET de JSON](https://www.newtonsoft.com/json/help/html/QueryJsonSelectTokenJsonPath.htm) para resolver expresiones de ruta de acceso JSON.

#### <a name="jsonpathcontenttemplate"></a>JsonPathContentTemplate
JsonPathContentTemplate permite hacer coincidir valores de un mensaje del centro de eventos mediante la ruta de acceso JSON y extraerlos.

| Propiedad | Descripción |Ejemplo |
| --- | --- | --- |
|**TypeName**|Tipo que se va a asociar a las medidas que coincidan con la plantilla.|`heartrate`
|**TypeMatchExpression**|Expresión de la ruta de acceso JSON que se evalúa con la carga del centro de eventos. Si se encuentra un JToken coincidente, la plantilla se considera una coincidencia. Todas las expresiones subsiguientes se evalúan con los valores JToken coincidentes de aquí.|`$..[?(@heartRate)]`
|**TimestampExpression**|Expresión de la ruta de acceso JSON para extraer el valor de marca de tiempo para OccurenceTimeUtc de la medida.|`$.endDate`
|**DeviceIdExpression**|Expresión de la ruta de acceso JSON para extraer el identificador de dispositivo.|`$.deviceId`
|**PatientIdExpression**|*Opcional*: expresión de la ruta de acceso JSON para extraer el identificador del paciente.|`$.patientId`
|**EncounterIdExpression**|*Opcional*: expresión de la ruta de acceso JSON para extraer el identificador del encuentro.|`$.encounterId`
|**Values[].ValueName**|Nombre que se va a asociar al valor extraído por la expresión subsiguiente. Se usa para enlazar el valor o componente necesarios en la plantilla de asignación de FHIR. |`hr`
|**Values[].ValueExpression**|Expresión de la ruta de acceso JSON para extraer el valor necesario.|`$.heartRate`
|**Values[].Required**|Requerirá que el valor esté presente en la carga;  si no se encuentra, no se generará ninguna medida y se producirá una excepción InvalidOperationException.|`true`

##### <a name="examples"></a>Ejemplos
---
**Frecuencia cardíaca**

*Message*
```json
{
    "Body": {
        "heartRate": "78",
        "endDate": "2019-02-01T22:46:01.8750000Z",
        "deviceId": "device123"
    },
    "Properties": {},
    "SystemProperties": {}
}
```
*Plantilla*
```json
{
    "templateType": "JsonPathContent",
    "template": {
        "typeName": "heartrate",
        "typeMatchExpression": "$..[?(@heartRate)]",
        "deviceIdExpression": "$.deviceId",
        "timestampExpression": "$.endDate",
        "values": [
            {
                "required": "true",
                "valueExpression": "$.heartRate",
                "valueName": "hr"
            }
        ]
    }
}
```
---
**Presión sanguínea**

*Message*
```json
{
    "Body": {
        "systolic": "123",
        "diastolic" : "87",
        "endDate": "2019-02-01T22:46:01.8750000Z",
        "deviceId": "device123"
    },
    "Properties": {},
    "SystemProperties": {}
}
```
*Plantilla*
```json
{
    "typeName": "bloodpressure",
    "typeMatchExpression": "$..[?(@systolic && @diastolic)]",
    "deviceIdExpression": "$.deviceId",
    "timestampExpression": "$.endDate",
    "values": [
        {
            "required": "true",
            "valueExpression": "$.systolic",
            "valueName": "systolic"
        },
        {
            "required": "true",
            "valueExpression": "$.diastolic",
            "valueName": "diastolic"
        }
    ]
}
```
---

**Proyección de varias medidas de un solo mensaje**

*Message*
```json
{
    "Body": {
        "heartRate": "78",
        "steps": "2",
        "endDate": "2019-02-01T22:46:01.8750000Z",
        "deviceId": "device123"
    },
    "Properties": {},
    "SystemProperties": {}
}
```
*Plantilla 1*
```json
{
    "templateType": "JsonPathContent",
    "template": {
        "typeName": "heartrate",
        "typeMatchExpression": "$..[?(@heartRate)]",
        "deviceIdExpression": "$.deviceId",
        "timestampExpression": "$.endDate",
        "values": [
            {
                "required": "true",
                "valueExpression": "$.heartRate",
                "valueName": "hr"
            }
        ]
    }
}
```
*Plantilla 2*
```json
{
    "templateType": "JsonPathContent",
    "template": {
        "typeName": "stepcount",
        "typeMatchExpression": "$..[?(@steps)]",
        "deviceIdExpression": "$.deviceId",
        "timestampExpression": "$.endDate",
        "values": [
            {
                "required": "true",
                "valueExpression": "$.steps",
                "valueName": "steps"
            }
        ]
    }
}
```
---

**Proyección de varias medidas de la matriz en el mensaje**

*Message*
```json
{
    "Body": [
        {
            "heartRate": "78",
            "endDate": "2019-02-01T22:46:01.8750000Z",
            "deviceId": "device123"
        },
        {
            "heartRate": "81",
            "endDate": "2019-02-01T23:46:01.8750000Z",
            "deviceId": "device123"
        },
        {
            "heartRate": "72",
            "endDate": "2019-02-01T24:46:01.8750000Z",
            "deviceId": "device123"
        }
    ],
    "Properties": {},
    "SystemProperties": {}
}
```
*Plantilla*
```json
{
    "templateType": "JsonPathContent",
    "template": {
        "typeName": "heartrate",
        "typeMatchExpression": "$..[?(@heartRate)]",
        "deviceIdExpression": "$.deviceId",
        "timestampExpression": "$.endDate",
        "values": [
            {
                "required": "true",
                "valueExpression": "$.heartRate",
                "valueName": "hr"
            }
        ]
    }
}
```

#### <a name="iotjsonpathcontenttemplate"></a>IotJsonPathContentTemplate

IotJsonPathContentTemplate es similar a JsonPathContentTemplate, salvo que DeviceIdExpression y TimestampExpression no son necesarios.

Al usar esta plantilla se supone que los mensajes que se están evaluando se enviaron mediante los [SDK de dispositivo de Azure IoT Hub](../../iot-hub/iot-hub-devguide-sdks.md#azure-iot-hub-device-sdks) o la característica [Exportar datos (heredado)](../../iot-central/core/howto-export-data-legacy.md) de [Azure IoT Central](../../iot-central/core/overview-iot-central.md). Al usar estos SDK, se conocen la identidad del dispositivo (suponiendo que el identificador de dispositivo de Azure IoT Hub/Central se haya registrado como identificador de un recurso de dispositivo en el servidor de FHIR de destino) y la marca de tiempo del mensaje. Puede usar JsonPathContentTemplate aunque use el SDK de dispositivo de Azure IoT Hub, si hay propiedades personalizadas en el cuerpo del mensaje correspondientes a la identidad del dispositivo o a la marca de tiempo de la medida.

*Nota: Cuando se usa IotJsonPathContentTemplate, TypeMatchExpression debe resolverse en el mensaje completo como valor JToken. Consulte los ejemplos siguientes.* 
##### <a name="examples"></a>Ejemplos
---
**Frecuencia cardíaca**

*Message*
```json
{
    "Body": {
        "heartRate": "78"        
    },
    "Properties": {
        "iothub-creation-time-utc" : "2019-02-01T22:46:01.8750000Z"
    },
    "SystemProperties": {
        "iothub-connection-device-id" : "device123"
    }
}
```
*Plantilla*
```json
{
    "templateType": "JsonPathContent",
    "template": {
        "typeName": "heartrate",
        "typeMatchExpression": "$..[?(@Body.heartRate)]",
        "deviceIdExpression": "$.deviceId",
        "timestampExpression": "$.endDate",
        "values": [
            {
                "required": "true",
                "valueExpression": "$.Body.heartRate",
                "valueName": "hr"
            }
        ]
    }
}
```
---
**Presión sanguínea**

*Message*
```json
{
    "Body": {
        "systolic": "123",
        "diastolic" : "87"
    },
    "Properties": {
        "iothub-creation-time-utc" : "2019-02-01T22:46:01.8750000Z"
    },
    "SystemProperties": {
        "iothub-connection-device-id" : "device123"
    }
}
```
*Plantilla*
```json
{
    "typeName": "bloodpressure",
    "typeMatchExpression": "$..[?(@Body.systolic && @Body.diastolic)]",
    "values": [
        {
            "required": "true",
            "valueExpression": "$.Body.systolic",
            "valueName": "systolic"
        },
        {
            "required": "true",
            "valueExpression": "$.Body.diastolic",
            "valueName": "diastolic"
        }
    ]
}
```

#### <a name="iotcentraljsonpathcontenttemplate"></a>IotCentralJsonPathContentTemplate

IotCentralJsonPathContentTemplate tampoco requiere DeviceIdExpression ni TimestampExpression, y se usa cuando se envían los mensajes que se están evaluando mediante la característica [Exportar datos](../../iot-central/core/howto-export-data.md) de [Azure IoT Central](../../iot-central/core/overview-iot-central.md). Al usar esta característica, se conocen la identidad del dispositivo (suponiendo que el identificador de dispositivo de Azure IoT Central se haya registrado como identificador de un recurso de dispositivo en el servidor de FHIR de destino) y la marca de tiempo del mensaje. Si usa la característica de exportación de datos de Azure IoT Central, pero usa propiedades personalizadas en el cuerpo del mensaje correspondientes a la identidad del dispositivo o a la marca de tiempo de la medida, todavía puede usar JsonPathContentTemplate.

*Nota: Cuando se usa IotCentralJsonPathContentTemplate, TypeMatchExpression debe resolverse en el mensaje completo como valor JToken. Consulte los ejemplos siguientes.* 
##### <a name="examples"></a>Ejemplos
---
**Frecuencia cardíaca**

*Message*
```json
{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "telemetry",
    "deviceId": "1vzb5ghlsg1",
    "schema": "default@v1",
    "templateId": "urn:qugj6vbw5:___qbj_27r",
    "enqueuedTime": "2020-08-05T22:26:55.455Z",
    "telemetry": {
        "HeartRate": "88",
    },
    "enrichments": {
      "userSpecifiedKey": "sampleValue"
    },
    "messageProperties": {
      "messageProp": "value"
    }
}
```
*Plantilla*
```json
{
    "templateType": "IotCentralJsonPathContent",
    "template": {
        "typeName": "heartrate",
        "typeMatchExpression": "$..[?(@telemetry.HeartRate)]",
        "values": [
            {
                "required": "true",
                "valueExpression": "$.telemetry.HeartRate",
                "valueName": "hr"
            }
        ]
    }
}
```
---
**Presión sanguínea**

*Message*
```json
{
    "applicationId": "1dffa667-9bee-4f16-b243-25ad4151475e",
    "messageSource": "telemetry",
    "deviceId": "1vzb5ghlsg1",
    "schema": "default@v1",
    "templateId": "urn:qugj6vbw5:___qbj_27r",
    "enqueuedTime": "2020-08-05T22:26:55.455Z",
    "telemetry": {
        "BloodPressure": {
            "Diastolic": "87",
            "Systolic": "123"
        }
    },
    "enrichments": {
      "userSpecifiedKey": "sampleValue"
    },
    "messageProperties": {
      "messageProp": "value"
    }
}
```
*Plantilla*
```json
{
    "templateType": "IotCentralJsonPathContent",
    "template": {
        "typeName": "bloodPressure",
        "typeMatchExpression": "$..[?(@telemetry.BloodPressure.Diastolic && @telemetry.BloodPressure.Systolic)]",
        "values": [
            {
                "required": "true",
                "valueExpression": "$.telemetry.BloodPressure.Diastolic",
                "valueName": "bp_diastolic"
            },
            {
                "required": "true",
                "valueExpression": "$.telemetry.BloodPressure.Systolic",
                "valueName": "bp_systolic"
            }
        ]
    }
}
```

## <a name="fhir-mapping"></a>Asignación de FHIR
Una vez extraído el contenido del dispositivo en un modelo normalizado, los datos se recopilan y agrupan según el identificador de dispositivo, el tipo de medida y el período de tiempo. La salida de esta agrupación se envía para su conversión en un recurso FHIR (actualmente [Observation](https://www.hl7.org/fhir/observation.html) [Observación]). Aquí, la plantilla de asignación de FHIR controla la asignación de los datos a un recurso Observation (Observación) de FHIR. ¿Debe crearse un recurso de observación para un momento o para un período de una hora? ¿Qué códigos se deben agregar al recurso de observación? ¿Se debe representar el valor como [SampledData](https://www.hl7.org/fhir/datatypes.html#SampledData) o como una [cantidad](https://www.hl7.org/fhir/datatypes.html#Quantity)? Estos tipos de datos son todas las opciones que la configuración de asignación de FHIR controla.

### <a name="codevaluefhirtemplate"></a>CodeValueFhirTemplate
CodeValueFhirTemplate es la única plantilla admitida en la asignación de FHIR en este momento.  Permite definir los códigos, el período efectivo y el valor del recurso de observación. Se admiten varios tipos de valor: [SampledData](https://www.hl7.org/fhir/datatypes.html#SampledData), [CodeableConcept](https://www.hl7.org/fhir/datatypes.html#CodeableConcept) y [Quantity](https://www.hl7.org/fhir/datatypes.html#Quantity). Junto con estos valores configurables, el identificador del recurso Observation (Observación) y la vinculación a los recursos de dispositivo y paciente adecuados se administran automáticamente.

| Propiedad | Descripción 
| --- | ---
|**TypeName**| Tipo de medida a la que se debe enlazar esta plantilla. Debe haber al menos una plantilla Device mapping (Asignación de dispositivos) que genere este tipo.
|**PeriodInterval**|Tiempo que debe representar el recurso de observación creado. Los valores admitidos son 0 (una instancia), 60 (una hora), 1440 (un día).
|**Categoría**|Cualquier número de [CodeableConcepts](http://hl7.org/fhir/datatypes-definitions.html#codeableconcept) para clasificar el tipo de recurso de observación que se ha creado.
|**Codes**|Uno o varios valores de [Coding](http://hl7.org/fhir/datatypes-definitions.html#coding) que aplicar al recurso de observación creado.
|**Codes[].Code**|Código para [Coding](http://hl7.org/fhir/datatypes-definitions.html#coding).
|**Codes[].System**|Sistema de [Coding](http://hl7.org/fhir/datatypes-definitions.html#coding).
|**Codes[].Display**|Presentación de [Coding](http://hl7.org/fhir/datatypes-definitions.html#coding).
|**Valor**|Valor que se va a extraer y representar en el recurso de observación. Para más información, consulte [Plantillas de tipo de valor](#valuetypes).
|**Componentes**|*Opcional:* uno o más componentes que se van a crear en el recurso de observación.
|**Components[].Codes**|Uno o varios valores de [Coding](http://hl7.org/fhir/datatypes-definitions.html#coding) que aplicar al componente.
|**Components[].Value**|Valor que se va a extraer y representar en el componente. Para más información, consulte [Plantillas de tipo de valor](#valuetypes).

### <a name="value-type-templates"></a>Plantillas de tipo de valor <a name="valuetypes"></a>
A continuación se muestran las de tipo de valor admitidas. En el futuro se pueden agregar más plantillas.
#### <a name="sampleddata"></a>SampledData
Representa el tipo de datos [SampledData](http://hl7.org/fhir/datatypes.html#SampledData) de FHIR. Las medidas del recurso de observación se escriben en un flujo de valores a partir de un momento dado que se incrementa con el período definido. Si no hay ningún valor, se escribirá `E` en el flujo de datos. Si en el período dos valores más ocupan la misma posición en el flujo de datos, se usa el más reciente. La misma lógica se aplica cuando se actualiza un recurso de observación mediante SampledData.

| Propiedad | Descripción 
| --- | ---
|**DefaultPeriod**|Período predeterminado, en milisegundos, que se va a usar. 
|**Unidad**|Unidad que se va a establecer en el origen de SampledData. 

#### <a name="quantity"></a>Cantidad
Representa el tipo de datos [Quantity](http://hl7.org/fhir/datatypes.html#Quantity) de FHIR. Si hay más de un valor en la agrupación, solo se usa el primero. Cuando llega un nuevo valor que se asigna al mismo recurso de observación, sobrescribe el valor anterior.

| Propiedad | Descripción 
| --- | --- 
|**Unidad**| Representación de la unidad.
|**Código**| Formato codificado de la unidad.
|**Sistema**| Sistema que define el formato de unidad codificado.

### <a name="codeableconcept"></a>CodeableConcept
Representa el tipo de datos [CodeableConcept](http://hl7.org/fhir/datatypes.html#CodeableConcept) de FHIR. No se usa el valor real.

| Propiedad | Descripción 
| --- | --- 
|**Texto**|Representación de texto sin formato. 
|**Codes**|Uno o varios valores de [Coding](http://hl7.org/fhir/datatypes-definitions.html#coding) que aplicar al recurso de observación creado.
|**Codes[].Code**|Código para [Coding](http://hl7.org/fhir/datatypes-definitions.html#coding).
|**Codes[].System**|Sistema de [Coding](http://hl7.org/fhir/datatypes-definitions.html#coding).
|**Codes[].Display**|Presentación de [Coding](http://hl7.org/fhir/datatypes-definitions.html#coding).

### <a name="examples"></a>Ejemplos
**Frecuencia cardíaca: SampledData**
```json
{
    "templateType": "CodeValueFhir",
    "template": {
        "codes": [
            {
                "code": "8867-4",
                "system": "http://loinc.org",
                "display": "Heart rate"
            }
        ],
        "periodInterval": 60,
        "typeName": "heartrate",
        "value": {
            "defaultPeriod": 5000,
            "unit": "count/min",
            "valueName": "hr",
            "valueType": "SampledData"
        }
    }
}
```
---
**Pasos: SampledData**
```json
{
    "templateType": "CodeValueFhir",
    "template": {
        "codes": [
            {
                "code": "55423-8",
                "system": "http://loinc.org",
                "display": "Number of steps"
            }
        ],        
        "periodInterval": 60,
        "typeName": "stepsCount",
        "value": {
            "defaultPeriod": 5000,
            "unit": "",
            "valueName": "steps",
            "valueType": "SampledData"
        }
    }
}
```
---
**Presión sanguínea: SampledData**
```json
{
    "templateType": "CodeValueFhir",
    "template": {
        "codes": [
            {
                "code": "85354-9",
                "display": "Blood pressure panel with all children optional",
                "system": "http://loinc.org"
            }
        ],
        "periodInterval": 60,
        "typeName": "bloodpressure",
        "components": [
            {
                "codes": [
                    {
                        "code": "8867-4",
                        "display": "Diastolic blood pressure",
                        "system": "http://loinc.org"
                    }
                ],
                "value": {
                    "defaultPeriod": 5000,
                    "unit": "mmHg",
                    "valueName": "diastolic",
                    "valueType": "SampledData"
                }
            },
            {
                "codes": [
                    {
                        "code": "8480-6",
                        "display": "Systolic blood pressure",
                        "system": "http://loinc.org"
                    }
                ],
                "value": {
                    "defaultPeriod": 5000,
                    "unit": "mmHg",
                    "valueName": "systolic",
                    "valueType": "SampledData"
                }
            }
        ]
    }
}
```
---
**Presión sanguínea: Quantity**
```json
{
    "templateType": "CodeValueFhir",
    "template": {
        "codes": [
            {
                "code": "85354-9",
                "display": "Blood pressure panel with all children optional",
                "system": "http://loinc.org"
            }
        ],
        "periodInterval": 0,
        "typeName": "bloodpressure",
        "components": [
            {
                "codes": [
                    {
                        "code": "8867-4",
                        "display": "Diastolic blood pressure",
                        "system": "http://loinc.org"
                    }
                ],
                "value": {
                    "unit": "mmHg",
                    "valueName": "diastolic",
                    "valueType": "Quantity"
                }
            },
            {
                "codes": [
                    {
                        "code": "8480-6",
                        "display": "Systolic blood pressure",
                        "system": "http://loinc.org"
                    }
                ],
                "value": {
                    "unit": "mmHg",
                    "valueName": "systolic",
                    "valueType": "Quantity"
                }
            }
        ]
    }
}
```
---
**Dispositivo eliminado: CodeableConcept**
```json
{
    "templateType": "CodeValueFhir",
    "template": {
        "codes": [
            {
                "code": "deviceEvent",
                "system": "https://www.mydevice.com/v1",
                "display": "Device Event"
            }
        ],
        "periodInterval": 0,
        "typeName": "deviceRemoved",
        "value": {
            "text": "Device Removed",
            "codes": [
                {
                    "code": "deviceRemoved",
                    "system": "https://www.mydevice.com/v1",
                    "display": "Device Removed"
                }
            ],
            "valueName": "deviceRemoved",
            "valueType": "CodeableConcept"
        }
    }
}
```
---

## <a name="next-steps"></a>Pasos siguientes

Consulte las preguntas frecuentes sobre el conector de Azure IoT para FHIR (versión preliminar).

>[!div class="nextstepaction"]
>[P+F del conector de Azure IoT para FHIR](fhir-faq.yml)

*En Azure Portal, el Conector de Azure IoT para FHIR se conoce como Conector de IoT (versión preliminar). FHIR es una marca registrada de HL7 y se usa con el permiso de HL7.