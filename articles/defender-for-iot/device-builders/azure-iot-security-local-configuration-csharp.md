---
title: Configuración local del agente de seguridad de Defender para IoT (C#)
description: Obtenga más información sobre el archivo de configuración local del agente de seguridad y del servicio de seguridad Defender para IoT en C#.
ms.custom: devx-track-csharp
ms.topic: conceptual
ms.date: 10/08/2020
ms.openlocfilehash: 810ca270fed350da8beaa1c63fafe39df4ab6a61
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128658285"
---
# <a name="understanding-the-local-configuration-file-c-agent"></a>Información sobre el archivo de configuración local (agente de C#)

El agente de seguridad de Defender para IoT usa configuraciones de un archivo de configuración local.

El agente de seguridad lee el archivo de configuración una vez, cuando se ejecuta el agente. Las configuraciones que se encuentran en el archivo de configuración local contienen la configuración de la autenticación y otras configuraciones relacionadas con el agente.

El agente de seguridad de C# utiliza varios archivos de configuración:

- **General.config**: configuraciones relacionadas con el agente.
- **Authentication.config**: configuración relacionada con la autenticación (incluidos los detalles de autenticación).
- **SecurityIotInterface.config**: configuraciones relacionadas con IoT.

Los archivos de configuración contienen la configuración predeterminada. La configuración de autenticación se rellena durante la instalación del agente, y se realizan cambios en el archivo de configuración cuando se reinicia el agente.

## <a name="configuration-file-location"></a>Ubicación del archivo de configuración

Para Linux:

- Los archivos de configuración del sistema operativo se encuentran en `/var/ASCIoTAgent`.

Para Windows:

- Los archivos de configuración del sistema operativo se encuentran en el directorio del agente de seguridad.

### <a name="generalconfig-configurations"></a>Configuraciones de General.config

| Nombre de la configuración | Valores posibles | Detalles |
|:-----------|:---------------|:--------|
| agentId | GUID | Identificador único del agente |
| readRemoteConfigurationTimeout | TimeSpan | Período de tiempo para capturar la configuración remota de IoT Hub. Si el agente no puede recuperar la configuración en el tiempo especificado, se agotará el tiempo de espera de la operación.|
| schedulerInterval | TimeSpan | Intervalo del programador interno. |
| producerInterval | TimeSpan | Intervalo de trabajo del productor de eventos. |
| consumerInterval | TimeSpan | Intervalo de trabajo del consumidor de eventos. |
| highPriorityQueueSizePercentage | 0 < número < 1 | La parte de la memoria caché total dedicada a los mensajes de prioridad alta. |
| logLevel | "Off", "Fatal", "Error", "Warning", "Information" y "Debug"  | Los mensajes de registro iguales y superiores a esta gravedad se registran en la consola de depuración (Syslog en Linux). |
| fileLogLevel |  "Off", "Fatal", "Error", "Warning", "Information" y "Debug"| Los mensajes de registro iguales y superiores a esta gravedad se registran en el archivo (Syslog en Linux). |
| diagnosticVerbosityLevel | "None", "Some" y "All", | Nivel de detalle de los eventos de diagnóstico. Ninguno: no se envían eventos de diagnóstico. Algunos: solo se envían los eventos de diagnóstico de gran importancia. Todos: también se envían todos los registros como eventos de diagnóstico. |
| logFilePath | Ruta a archivo | Si fileLogLevel > Off, los registros se escriben en este archivo. |
| defaultEventPriority | "High", "Low" y "Off" | Prioridad del evento predeterminada. |

### <a name="generalconfig-example"></a>Ejemplo de General.config

```xml
<?xml version="1.0" encoding="utf-8"?>
<General>
  <add key="agentId" value="da00006c-dae9-4273-9abc-bcb7b7b4a987" />
  <add key="readRemoteConfigurationTimeout" value="00:00:30" />
  <add key="schedulerInterval" value="00:00:01" />
  <add key="producerInterval" value="00:02:00" />
  <add key="consumerInterval" value="00:02:00" />
  <add key="highPriorityQueueSizePercentage" value="0.5" />
  <add key="logLevel" value="Information" />
  <add key="fileLogLevel" value="Off"/>
  <add key="diagnosticVerbosityLevel" value="Some" />
  <add key="logFilePath" value="IotAgentLog.log" />
  <add key="defaultEventPriority" value="Low"/>
</General>
```

### <a name="authenticationconfig"></a>Authentication.config

| Nombre de la configuración | Valores posibles | Detalles |
|:-----------|:---------------|:--------|
| moduleName | string | Nombre de la identidad de Defender-IoT-micro-agent. Este nombre debe corresponderse con el nombre de identidad del módulo del dispositivo. |
| deviceId | string | Identificador del dispositivo (como está registrado en Azure IoT Hub). |
| schedulerInterval | Cadena TimeSpan | Intervalo del programador interno. |
| gatewayHostname | string | Nombre de host de Azure IoT Hub. Normalmente \<my-hub\>.azure-devices.net. |
| filePath | string - path to file | Ruta de acceso al archivo que contiene el secreto de autenticación.|
| type | "SymmetricKey" y "SelfSignedCertificate" | Secreto de usuario para la autenticación. Elija *SymmetricKey* si el secreto de usuario es una clave simétrica y seleccione *self-signed certificate* si el secreto es un certificado autofirmado. |
| identity | "DPS", "Module" y "Device" | Identidad de autenticación: DPS si la autenticación se realiza a través de DPS, Module si la autenticación se realiza mediante las credenciales del módulo o Device si la autenticación se realiza con las credenciales del dispositivo.
| certificateLocationKind |  "LocalFile" y "Store" | LocalFile si el certificado se almacena en un archivo y Store si el certificado se encuentra en un almacén de certificados. |
| idScope | string | Ámbito del identificador de DPS |
| registrationId | string  | Id. de registro del dispositivo de DPS. |
|

### <a name="authenticationconfig-example"></a>Ejemplo de Authentication.config

```xml
<?xml version="1.0" encoding="utf-8"?>
<Authentication>
  <add key="moduleName" value="azureiotsecurity"/>
  <add key="deviceId" value="d1"/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value="c:\p-dps-d1.pfx"/>
  <add key="type" value="SelfSignedCertificate" />                     <!-- SymmetricKey, SelfSignedCertificate-->
  <add key="identity" value="DPS" />                 <!-- Device, Module, DPS -->
  <add key="certificateLocationKind" value="LocalFile" />  <!-- LocalFile, Store -->
  <add key="idScope" value="0ne0005335B"/>
  <add key="registrationId" value="d1"/>
</Authentication>
```

### <a name="securityiotinterfaceconfig"></a>SecurityIotInterface.config

| Nombre de la configuración | Valores posibles | Detalles |
|:-----------|:---------------|:--------|
| transportType | "Ampq" y "Mqtt" | Tipo de transporte de IoT Hub. |
|

### <a name="securityiotinterfaceconfig-example"></a>Ejemplo de SecurityIotInterface.config

```xml
<ExternalInterface>
  <add key="facadeType"  value="Microsoft.Azure.Security.IoT.Agent.Common.SecurityIoTHubInterface, Security.Common" />
  <add key="transportType" value="Amqp"/>
</ExternalInterface>
```

## <a name="next-steps"></a>Pasos siguientes

- Lea la [información general](overview.md) del servicio Defender for IoT.
- Más información sobre la [arquitectura de soluciones basadas en agente](architecture-agent-based.md) de Defender for IoT
- Habilite el [servicio](quickstart-onboard-iot-hub.md) Defender para IoT.
- Lea las [preguntas más frecuentes](resources-agent-frequently-asked-questions.md) del servicio Defender para IoT.
- Aprenda a acceder a [datos de seguridad sin procesar](how-to-security-data-access.md)
- Obtenga información acerca de las [recomendaciones](concept-recommendations.md)
- Obtenga información sobre las [alertas de seguridad](concept-security-alerts.md)
