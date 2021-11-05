---
title: 'Uso de métodos directos en Azure Video Analyzer: Azure'
description: El módulo perimetral de Azure Video Analyzer expone varios métodos directos que se describen en este tema.
ms.topic: conceptual
ms.date: 10/01/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: 8ccf325164705e9b375cb4fa1c32af2228dcbed5
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131090928"
---
# <a name="azure-video-analyzer-direct-methods"></a>Métodos directos de Azure Video Analyzer

[!INCLUDE [header](includes/edge-env.md)]

El módulo perimetral de Azure Video Analyzer `avaedge` expone varios métodos directos que se pueden invocar desde IoT Hub. Los métodos directos representan una interacción solicitud-respuesta con un dispositivo similar a una llamada HTTP en la cual se completan correctamente o generan un error de inmediato (tras un tiempo de espera que especifica el usuario). Este enfoque es útil en escenarios donde el curso de una acción inmediata es distinto en función de si el dispositivo pudo responder. Para obtener más información, vea el artículo [Conocimiento e invocación de los métodos directos de IoT Hub](../../../iot-hub/iot-hub-devguide-direct-methods.md).

En este tema se describen estos métodos, convenciones y el esquema de los métodos.

## <a name="conventions"></a>Convenciones

Los métodos directos se basan en las siguientes convenciones:

1. Los métodos directos tienen una versión especificada en el formato PRINCIPAL.SECUNDARIA (como se muestra en el ejemplo siguiente). Es el número "@apiVersion", donde "1" es la versión PRINCIPAL y "0" es la SECUNDARIA en este ejemplo:

    ```
    {
      "methodName": "pipelineTopologySet",
      "payload": {
          "@apiVersion": "1.1",
          "name": "{TopologyName}",
          "properties": {
              // Desired Topology properties
          }
      }
    }
    ```
1. Una versión dada del módulo de Video Analyzer admite todas las versiones secundarias de una llamada de método directo hasta su versión actual. No se garantiza la compatibilidad entre las versiones principales.
1. Todos los métodos directos son sincrónicos.
1. Los resultados del error se basan en el [esquema de errores de OData](http://docs.oasis-open.org/odata/odata-json-format/v4.01/odata-json-format-v4.01.html#sec_ErrorResponse).
1. Los nombres presentan las restricciones siguientes:
    * Solo pueden estar formados por caracteres alfanuméricos y guiones, siempre y cuando no empiecen y acaben con un guion.
    * No deben tener espacios.
    * 32 caracteres como máximo

### <a name="example-of-response-from-a-direct-method"></a>Ejemplo de respuesta de un método directo

```
-----------------------  Request: livePipelineList  --------------------------------------------------

{
  "@apiVersion": "1.1"
}

---------------  Response: livePipelineList - Status: 200  ---------------

{
  "value": []
}

--------------------------------------------------------------------------
```    
### <a name="error-codes"></a>Códigos de error
Como se muestra en el ejemplo siguiente, cuando se obtiene una respuesta de error de un método directo, existe un código de error de nivel superior y se proporciona más información en `details`.


```json
{
  "status": 400,
  "payload": {
    "error": {
      "code": "BadRequest",
      "message": "The request is invalid",
      "details": [
        {
          "code": "ApiVersionNotSupported",
          "message": "The API version '1.4' is not supported. Supported version(s): 1.0, 1.1"
        }
      ]
    }
  }
}
```

A continuación se muestran los códigos de error que se usan en el nivel superior.

|Estado |Código   |Message|
|---|---|---|
|400|   BadRequest| La solicitud no es válida.|
|400|   InvalidResource|    El recurso no es válido|
|400|   InvalidVersion| La versión de la API no es válida|
|402|   ConnectivityRequired    |El módulo de Edge requiere conectividad en la nube para funcionar correctamente|
|404|   NotFound    |No se encontró el recurso|
|409|   Conflicto|   Conflicto de operación|
|500|   InternalServerError|    Error interno del servidor|
|503|   ServerBusy| Servidor ocupado|

A continuación se muestran algunos de los códigos de error que se usan en el nivel de detalle.

|Estado|    Código detallado   |Descripción|
|---|---|---|
|400|   PipelineValidationError|    Errores generales de las canalizaciones, como ciclos, particiones, etc.|
|400|   ModuleValidationError|  Errores de validación específicos del módulo.|
|409|   PipelineTopologyInUse|  Una o varias canalizaciones dinámicas siguen haciendo referencia a la topología de canalización.|
|409|   OperationNotAllowedInState| La operación solicitada no se puede realizar en el estado actual.|
|409|   ResourceValidationError|    El recurso al que se hace referencia (por ejemplo, un recurso de vídeo) no tiene un estado válido.|

## <a name="supported-direct-methods"></a>Métodos directos admitidos  
A continuación se muestran los métodos directos que expone el módulo Edge de Video Analyzer. El esquema de los métodos directos se puede encontrar [aquí](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/videoanalyzer/data-plane/VideoAnalyzer.Edge/preview/1.1.0/AzureVideoAnalyzerSdkDefinitions.json).

### <a name="pipelinetopologylist"></a>pipelineTopologyList

Este método directo enumera todas las topologías de canalización que se han creado.

#### <a name="request"></a>Solicitud

```
{
  "@apiVersion": "1.1"
}
```
#### <a name="response"></a>Response

```
{
  "status": 200,
  "value": [
    {
      "systemData": {
        "createdAt": "2021-05-05T14:19:22.16Z",
        "lastModifiedAt": "2021-05-05T16:20:41.505Z"
      },
      
      // first pipeline topology payload
      
    },
      "systemData": {
        "createdAt": "2021-05-06T14:19:22.16Z",
        "lastModifiedAt": "2021-05-06T16:20:41.505Z"
      },
      
      // next pipeline topology payload
      
    }    
  ]
}
```

#### <a name="status-codes"></a>Códigos de estado

| Condición | status code |
|--|--|
| Entidad encontrada | 200 |
| Errores de usuario generales | Intervalo 400 |
| No se ha encontrado la entidad | 404 |
| Errores generales del servidor | Intervalo 500 |

### <a name="pipelinetopologyset"></a>pipelineTopologySet

Crea una topología de canalización con el nombre especificado si no existe aún o actualiza una topología existente con ese nombre.

Aspectos clave:

* Una topología de canalización se puede actualizar libremente si no hay ninguna canalización dinámica que haga referencia a ella.
* Una topología de canalización se puede actualizar libremente si todas las canalizaciones dinámicas que hacen referencia a esta se desactivan, siempre y cuando se cumpla lo siguiente:

    * Los parámetros recién agregados tienen valores predeterminados.
    * Ninguna canalización hace referencia a los parámetros quitados.
* Solo se permiten algunas actualizaciones de la topología de canalización mientras una canalización dinámica esté activa.

#### <a name="request"></a>Solicitud

```
  {
    "methodName": "pipelineTopologySet",
    "payload": {
        "@apiVersion": "1.1",
        "name": "{TopologyName}",
        "properties": {
            // Desired pipeline topology properties
        }
    }
  }
```

#### <a name="response"></a>Response

```
  {
    "status": 201,
    "payload": {
        "systemData": {
           "createdAt": "2021-05-11T18:16:46.491Z",
           "lastModifiedAt": "2021-05-11T18:16:46.491Z"
        },
        "name": "{TopologyName}",
        "properties": {
            // Complete pipeline topology
        }
    }
  }
```
#### <a name="status-codes"></a>Códigos de estado

|Condición  |status code    |Código de error detallado|
|---|---|---|
Entidad existente actualizada |200|   N/D|
Nueva entidad creada  |201|   N/D|
Errores de usuario generales |Intervalo 400  ||
Errores de validación de la canalización  |400    |PipelineValidationError|
Errores de validación de módulos|   400 |ModuleValidationError|
Errores generales del servidor   |Intervalo 500  ||

### <a name="pipelinetopologyget"></a>pipelineTopologyGet

Recupera una topología de canalización con el nombre especificado, si existe.

#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1",
        "name": "{TopologyName}"       
  }
```
#### <a name="response"></a>Response

```
{
  "status": 200,
  "payload": {
    "value": [
      {
        "systemData": {
          "createdAt": "2021-05-06T10:28:04.560Z",
          "lastModifiedAt": "2021-05-06T10:28:04.560Z"
        },
        "name": "{TopologyName}",
        // Complete pipeline topology
      }
    ]
  }
}
```

#### <a name="status-codes"></a>Códigos de estado

| Condición | status code |
|--|--|
| Correcto | 200 |
| Errores de usuario generales | Intervalo 400 |
| Errores generales del servidor | Intervalo 500 |

### <a name="pipelinetopologydelete"></a>pipelineTopologyDelete

Elimina una topología de una única canalización.

* Tenga en cuenta que no puede haber ninguna canalización dinámica que haga [referencia a una topología de canalización que se esté eliminando](../pipeline.md#pipeline-states). Si la hubiera, recibirá un error `TopologyInUse`.

#### <a name="request"></a>Solicitud

```
  {
    "methodName": "pipelineTopologyDelete",
    "payload": {
        "@apiVersion": "1.1",
        "name": "{TopologyName}"
    }
  }
```
#### <a name="response"></a>Response

```
{
    "status": 200,
    "payload": null
}
```

#### <a name="status-codes"></a>Códigos de estado

| Condición | status code | Código de error detallado |
|--|--|--|
| Entidad eliminada | 200 | N/D |
| No se ha encontrado la entidad | 204 | N/D |
| Errores de usuario generales | Intervalo 400 |  |
| Una o varias canalizaciones hacen referencia a la topología de canalización. | 409 | PipelineTopologyInUse |
| Errores generales del servidor | Intervalo 500 |  |

### <a name="livepipelinelist"></a>livePipelineList

Enumera todas las canalizaciones dinámicas.

#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1"
  }
```
#### <a name="response"></a>Response

```
{
  "status": 200,
  "value": [
    {
      "systemData": {
        "createdAt": "2021-05-05T14:19:22.16Z",
        "lastModifiedAt": "2021-05-05T16:20:41.505Z"
      },      
      // first live pipeline payload  
    },
      "systemData": {
        "createdAt": "2021-05-06T14:19:22.16Z",
        "lastModifiedAt": "2021-05-06T16:20:41.505Z"
      },
      // next live pipeline payload
    }    
  ]
}
```
#### <a name="status-codes"></a>Códigos de estado

| Condición | status code |
|--|--|
| Entidad encontrada | 200 |
| Errores de usuario generales | Intervalo 400 |
| No se ha encontrado la entidad | 404 |
| Errores generales del servidor | Intervalo 500 |

### <a name="livepipelineset"></a>livePipelineSet

Crea una canalización dinámica con el nombre especificado si no existe aún o actualiza una canalización dinámica existente con ese nombre.

Aspectos clave:

* Una canalización dinámica se puede actualizar libremente si su estado es "Desactivado". En otros estados, solo se permiten algunas actualizaciones.
* Una canalización dinámica se revalida en cada actualización.

#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1",
        "name": "{livePipelineName}",
        "properties": {
            // Desired live pipeline properties
        }
  }
```
#### <a name="response"></a>Response

````
  {
    "status": 201,
    "payload": {
        "systemData": {
           "createdAt": "2021-05-11T18:16:46.491Z",
           "lastModifiedAt": "2021-05-11T18:16:46.491Z"
        },
        "name": "{livePipelineName}",
        "properties": {
            // Complete live pipeline
        }
    }
  }
````
#### <a name="status-codes"></a>Códigos de estado

| Condición | status code | Código de error detallado |
|--|--|--|
| Entidad existente actualizada | 200 | N/D |
| Nueva entidad creada | 201 | N/D |
| Errores de usuario generales | Intervalo 400 | N/D |
| Errores de validación de las canalizaciones dinámicas | 400 | PipelineValidationError |
| Errores de validación de módulos | 400 | ModuleValidationError |
| Errores de validación de recursos | 409 | ResourceValidationError |
| Errores generales del servidor | Intervalo 500 | N/D |

### <a name="livepipelinedelete"></a>livePipelineDelete

Elimina una única canalización dinámica.

Aspectos clave:

* Solo se pueden eliminar las canalizaciones dinámicas desactivadas.

#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1",
        "name": "{livePipelineName}"
  }
```
#### <a name="response"></a>Response

```
  {
    "status": 200,
    "payload": null
  }
```
#### <a name="status-codes"></a>Códigos de estado

| Condición | status code | Código de error detallado |
|--|--|--|
| La canalización dinámica se ha eliminado correctamente | 200 | N/D |
| No se ha encontrado la canalización dinámica | 204 | N/D |
| Errores de usuario generales | Intervalo 400 |  |
| La canalización no está en estado "Desactivado" | 409 | OperationNotAllowedInState |
| Errores generales del servidor | Intervalo 500 |  |

### <a name="livepipelineget"></a>livePipelineGet

Esto es similar a livePipelineTopologyGet. Recupera una canalización dinámica con el nombre especificado, si existe.

#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1",
        "name": "{livePipelineName}"       
  }
```
#### <a name="response"></a>Response

```
{
  "status": 200,
  "payload": {
    "value": [
      {
        "systemData": {
          "createdAt": "2021-05-06T10:28:04.560Z",
          "lastModifiedAt": "2021-05-06T10:28:04.560Z"
        },
        "name": "{livePipelineName}",
        // Complete live pipeline
      }
    ]
  }
}
```

#### <a name="status-codes"></a>Códigos de estado

| Condición | status code |
|--|--|
| Correcto | 200 |
| Errores de usuario generales | Intervalo 400 |
| Errores generales del servidor | Intervalo 500 |

### <a name="livepipelineactivate"></a>livePipelineActivate

Activa una canalización dinámica. 

Aspectos clave

* El método devuelve un valor cuando la canalización dinámica está en estado "Activando" o "Activado". 
* Una operación List/Set en la canalización dinámica devolvería el estado actual.
* Este método puede invocarse siempre que la canalización dinámica no esté en el estado (transitorio) "Desactivando".
* Idempotencia:
    * El comportamiento al llamar a `livePipelineActivate` en una canalización dinámica que ya está en estado "Activando" es el mismo que si la canalización dinámica se desactivara.
    * Al activar una canalización que está en estado "Activo", se devuelve inmediatamente un código de correcto.

#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1",
        "name": "{livePipelineName}"
  }
```
#### <a name="response"></a>Response

```
{
    "status": 200,
    "payload": null
}
```
#### <a name="status-codes"></a>Códigos de estado

| Condición | status code | Código de error detallado |
|--|--|--|
| Canalización activada correctamente | 200 | N/D |
| Nueva entidad creada | 201 | N/D |
| Errores de usuario generales | Intervalo 400 |  |
| Errores de validación de módulos | 400 | ModuleValidationError |
| Errores de validación de recursos | 409 | ResourceValidationError |
| La canalización dinámica está en estado de desactivación | 409 | OperationNotAllowedInState |
| Errores generales del servidor | Intervalo 500 |  |

### <a name="livepipelinedeactivate"></a>livePipelineDeactivate

Desactiva una canalización dinámica. Al desactivar una canalización, se desactiva correctamente el procesamiento de vídeo y se garantiza que todos los eventos y vídeos almacenados en caché en el perímetro se confirmen en la nube, si procede.

Aspectos clave:

* El método solo devuelve un valor cuando la canalización dinámica está desactivada.
* Este método puede invocarse siempre que la canalización dinámica no esté en el estado (transitorio) "Activando".
* La canalización entra en el estado "Desactivando" mientras se desactiva.
    * Una operación List/Set en la canalización dinámica devolvería el estado actual.
    * La desactivación se completa cuando todos los medios se han cargado en la nube (si la canalización tiene un nodo [receptor de vídeo](../pipeline.md#video-sink)).
* Idempotencia:
    * El comportamiento al llamar a `livePipelineDeactivate` en una canalización dinámica que ya está en estado "Desactivando" es el mismo que si la canalización dinámica se encontrara en estado "Activo".
    * Al desactivar una canalización que está en estado "Inactivo", se devuelve inmediatamente un código de correcto.

#### <a name="request"></a>Solicitud

```
  {
    "@apiVersion": "1.1",
    "name": "{livePipelineName}"
  }
```
#### <a name="response"></a>Response

```
{
 "status": 200,
 "payload": null
}
```
| Condición | status code | Código de error detallado |
|--|--|--|
| Canalización activada correctamente | 200 | N/D |
| Nueva entidad creada | 201 | N/D |
| Errores de usuario generales | Intervalo 400 |  |
| La canalización tiene el estado Activando | 409 | OperationNotAllowedInState |
| Errores generales del servidor | Intervalo 500 |  |

### <a name="remotedeviceadapterlist"></a>remoteDeviceAdapterList

Enumera todos los adaptadores de dispositivo remoto. El módulo perimetral de Video Analyzer puede actuar como puerta de enlace transparente para vídeo, lo que permite que los dispositivos IoT envíen vídeo a la nube desde detrás de un firewall. Debe crearse un adaptador de dispositivo remoto para cada uno de estos dispositivos IoT. La comunicación entre la nube y el dispositivo IoT fluiría a través del módulo perimetral de Video Analyzer.

#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1"
  }
```
#### <a name="response"></a>Response

```
{
  "status": 200,
  "value": [
    {
      "systemData": {
        "createdAt": "2021-10-05T14:19:22.16Z",
        "lastModifiedAt": "2021-10-05T16:20:41.505Z"
      },      
      // first remote device adapter payload
    },
      "systemData": {
        "createdAt": "2021-10-06T14:19:22.16Z",
        "lastModifiedAt": "2021-10-06T16:20:41.505Z"
      },
      // next remote device adapter  payload
    }    
  ]
}
```
#### <a name="status-codes"></a>Códigos de estado

| Condición | status code |
|--|--|
| Entidad encontrada | 200 |
| Errores de usuario generales | Intervalo 400 |
| No se ha encontrado la entidad | 404 |
| Errores generales del servidor | Intervalo 500 |

### <a name="remotedeviceadapterset"></a>remoteDeviceAdapterSet

Crea un adaptador de dispositivo remoto con el nombre especificado si no existe dicho adaptador o actualiza un adaptador existente con ese nombre. En la carga de respuesta y en las llamadas `remoteDeviceAdapterList` o`remoteDeviceAdapterGet` se omiten las credenciales o secretos.


#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1",
        "name": "{remoteDeviceAdapterName}",
        "properties": {
            // Desired remote device adapter properties
        }
  }
```
#### <a name="response"></a>Response

````
  {
    "status": 201,
    "payload": {
        "systemData": {
           "createdAt": "2021-10-11T18:16:46.491Z",
           "lastModifiedAt": "2021-10-11T18:16:46.491Z"
        },
        "name": "{remoteDeviceAdapterName}",
        "properties": {
            // Remote device adapter properties, except the credentials
        }
    }
  }
````
#### <a name="status-codes"></a>Códigos de estado

| Condición | status code | Código de error detallado |
|--|--|--|
| Entidad existente actualizada | 200 | N/D |
| Nueva entidad creada | 201 | N/D |
| Errores de usuario generales | Intervalo 400 | N/D |
| Errores de validación del adaptador de dispositivo remoto | 400 | RemoteDeviceAdapterValidationError |
| Errores de validación de módulos | 400 | ModuleValidationError |
| Errores de validación de recursos | 409 | ResourceValidationError |
| Errores generales del servidor | Intervalo 500 | N/D |

### <a name="remotedeviceadapterget"></a>remoteDeviceAdapterGet

Esto es similar a `livePipelineTopologyGet`. Recupera un adaptador de dispositivo remoto con el nombre especificado, si existe (se omiten las credenciales o secretos).

#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1",
        "name": "{remoteDeviceAdapterName}"       
  }
```
#### <a name="response"></a>Response

```
{
  "status": 200,
  "payload": {
    "value": [
      {
        "systemData": {
          "createdAt": "2021-10-06T10:28:04.560Z",
          "lastModifiedAt": "2021-10-06T10:28:04.560Z"
        },
        "name": "{remoteDeviceAdapterName}",
        // Remote device adapter properties, except the credentials
      }
    ]
  }
}
```

#### <a name="status-codes"></a>Códigos de estado

| Condición | status code |
|--|--|
| Correcto | 200 |
| Errores de usuario generales | Intervalo 400 |
| Errores generales del servidor | Intervalo 500 |

### <a name="remotedeviceadapterdelete"></a>remoteDeviceAdapterDelete

Elimina un único adaptador de dispositivo remoto, si existe.

#### <a name="request"></a>Solicitud

```
  {
        "@apiVersion": "1.1",
        "name": "{remoteDeviceAdapterName}"
  }
```
#### <a name="response"></a>Response

```
  {
    "status": 200,
    "payload": null
  }
```
#### <a name="status-codes"></a>Códigos de estado

| Condición | status code | Código de error detallado |
|--|--|--|
| El adaptador de dispositivo remoto se eliminó correctamente | 200 | N/D |
| No se encontró el adaptador de dispositivo remoto | 204 | N/D |
| Errores de usuario generales | Intervalo 400 |  |
| Errores generales del servidor | Intervalo 500 |  |

### <a name="onvifdevicediscover-and-onvifdeviceget"></a>onvifDeviceDiscover y onvifDeviceGet
Los detalles de estas llamadas se proporcionan en el artículo sobre [cómo detectar dispositivos compatibles con ONVIF](camera-discovery.md).

## <a name="next-steps"></a>Pasos siguientes

[Esquema de configuración de módulos gemelos](module-twin-configuration-schema.md)
