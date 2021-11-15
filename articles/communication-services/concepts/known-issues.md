---
title: 'Azure Communication Services: problemas conocidos'
description: Obtenga información acerca de Azure Communication Services.
author: rinarish
manager: chpalm
services: azure-communication-services
ms.author: rifox
ms.date: 06/30/2021
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 02c0d31ec07c210197968e514573e372ef24dd59
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130219117"
---
# <a name="known-issues"></a>Problemas conocidos
En este artículo se proporciona información sobre las limitaciones y los problemas conocidos relativos a los SDK de llamadas de Azure Communication Services y las API de automatización de llamadas e Azure Communication Services.

> [!IMPORTANT]
> Hay varios factores que pueden afectar a la calidad de la experiencia de llamada. Consulte la documentación sobre los **[requisitos de red](./voice-video-calling/network-requirements.md)** para más información sobre los procedimientos recomendados de configuración y prueba de la red de Communication Services.

## <a name="azure-communication-services-calling-sdks"></a>SDK de llamadas de Azure Communication Services

### <a name="javascript-sdk"></a>SDK de JavaScript

En esta sección se proporciona información sobre los problemas conocidos asociados a los SDK de llamada de voz y vídeo JavaScript de Azure Communication Services.

#### <a name="refreshing-a-page-doesnt-immediately-remove-the-user-from-their-call"></a>La actualización de una página no quita al usuario inmediatamente de su llamada

Si un usuario está en una llamada y decide actualizar la página, el servicio multimedia de Communication Services no quita este usuario inmediatamente de la llamada. Espera a que el usuario vuelva a unirse. Transcurrido el tiempo de espera del servicio multimedia, se quita al usuario de la llamada.

Es mejor crear experiencias de usuario en las que no sea necesario que los usuarios finales actualicen la página de la aplicación durante una llamada. Si un usuario actualiza la página, vuelva a usar el mismo identificador de usuario de Communication Services después de regresar a la aplicación.

Desde la perspectiva de otros participantes en la llamada, el usuario permanecerá en la llamada durante uno o dos minutos.

Si el usuario se vuelve a unir con el mismo identificador de usuario de Communication Services, se representará con el mismo objeto existente en la colección `remoteParticipants`.

Si antes de actualizar la página, el usuario estaba enviando vídeo, la colección `videoStreams` conservará la información de la secuencia anterior hasta que se agote el tiempo de espera del servicio y la quite. En este escenario, la aplicación puede decidir mantener las nuevas secuencias agregadas a la colección y representar una con el valor de `id` más alto. 


#### <a name="its-not-possible-to-render-multiple-previews-from-multiple-devices-on-web"></a>No es posible representar varias vistas previas de varios dispositivos en Web
Esta es una limitación conocida. Para obtener más información, vea la [introducción a los SDK de llamadas](./voice-video-calling/calling-sdk-features.md).

#### <a name="enumerating-devices-isnt-possible-in-safari-when-the-application-runs-on-ios-or-ipados"></a>No es posible enumerar dispositivos en Safari cuando la aplicación se ejecuta en iOS o iPadOS.

Las aplicaciones no pueden enumerar ni seleccionar dispositivos de micrófono o altavoz (por ejemplo, Bluetooth) en Safari en iOS o iPad. Hay una limitación conocida del sistema operativo.

Si usa Safari en macOS, la aplicación no podrá enumerar ni seleccionar altavoces mediante el Administrador de dispositivos de Communication Services. En este escenario, los dispositivos deben seleccionarse mediante el sistema operativo. Si usa Chrome en macOS, la aplicación sí puede enumerar y seleccionar dispositivos mediante el Administrador de dispositivos de Communication Services.

#### <a name="device-will-get-muted-and-incoming-video-will-stop-rendering-when-an-interruption-occurs-that-takes-over-device-access"></a>El dispositivo se silenciará y el vídeo entrante dejará de representarse cuando se produzca una interrupción que se haga cargo del acceso al dispositivo.
Este problema puede producirse principalmente cuando otra aplicación o sistema operativo se hace cargo del control del micrófono o de la cámara; se pueden ver algunos ejemplos a continuación:

- Mientras un usuario está en la llamada, llega una llamada RTC entrante y captura el acceso al dispositivo de micrófono.
- Mientras el usuario está en la llamada, se dirigirá a otra aplicación nativa que capturará el acceso al micrófono o a la cámara, por ejemplo, para reproducir un vídeo de YouTube o iniciar una llamada de FaceTime.
- Mientras el usuario está en la llamada, se habilitará Siri, que volverá a capturar el acceso al micrófono.

Para recuperarse de todos estos casos, el usuario tendrá que volver a la aplicación para reactivar el audio e iniciar el vídeo con el fin de que el audio y el vídeo empiecen a fluir después de la interrupción.

En algunas ocasiones, los dispositivos (micrófono o cámara) no se liberarán a tiempo, lo que puede causar problemas con la llamada original, por ejemplo, si el usuario intenta reactivar el audio mientras ve un vídeo de YouTube o una llamada RTC está activa al mismo tiempo. 

<br/>Biblioteca cliente: Calling (JavaScript)
<br/>Exploradores: Safari
<br/>Sistema operativo: iOS

#### <a name="repeatedly-switching-video-devices-may-cause-video-streaming-to-temporarily-stop"></a>El cambio repetido de dispositivos de los dispositivos de vídeo puede provocar la detención temporal del streaming de vídeo

El cambio entre dispositivos de vídeo puede hacer que la secuencia de vídeo se pause mientras se adquiere la secuencia desde el dispositivo seleccionado.

##### <a name="possible-causes"></a>Causas posibles
Cambiar entre dispositivos con frecuencia puede provocar una degradación del rendimiento. Se recomienda a los desarrolladores detener una secuencia del dispositivo antes de iniciar otra.

#### <a name="bluetooth-headset-microphone-is-not-detected-therefore-is-not-audible-during-the-call-on-safari-on-ios"></a>El micrófono de los auriculares Bluetooth no se detecta, por lo que no es audible durante la llamada en Safari en iOS
Safari en IOS no admite auriculares Bluetooth. El dispositivo Bluetooth no aparecerá entre las opciones de micrófono disponibles y otros participantes no podrán oírle si intenta usar Bluetooth en Safari.

##### <a name="possible-causes"></a>Causas posibles
Se trata de una limitación conocida de los sistemas operativos macOS, iOS e iPadOS. 

Con Safari en **macOS** e **iOS/iPadOS**, no es posible enumerar ni seleccionar dispositivos de altavoz mediante el Administrado de dispositivos de Communication Services, dado que esta característica no se admite en Safari. En este escenario, la selección del dispositivo debe actualizarse a través del sistema operativo.

#### <a name="rotation-of-a-device-can-create-poor-video-quality"></a>La rotación de un dispositivo puede crear una calidad de vídeo deficiente
Los usuarios pueden experimentar una calidad de vídeo degradada cuando se rotan los dispositivos.

<br/>Dispositivos afectados: Google Pixel 5, Google Pixel 3a, Apple iPad 8 y Apple iPad X
<br/>Biblioteca cliente: Calling (JavaScript)
<br/>Exploradores: Safari, Chrome
<br/>Sistema operativo: iOS, Android


#### <a name="camera-switching-makes-the-screen-freeze"></a>Al cambiar de cámara, la pantalla se bloquea 
Cuando un usuario de Communication Services se une a una llamada mediante Calling SDK de JavaScript y, luego, presiona el botón de cambio de cámara, la interfaz de usuario puede dejar de responder hasta que se actualice la aplicación o hasta que el usuario envíe el explorador a segundo plano.

<br/>Dispositivos afectados: Google Pixel 4A
<br/>Biblioteca cliente: Calling (JavaScript)
<br/>Exploradores: Chrome
<br/>Sistema operativo: iOS, Android


##### <a name="possible-causes"></a>Causas posibles
Bajo investigación.

#### <a name="if-the-video-signal-was-stopped-while-the-call-is-in-connecting-state-the-video-will-not-be-sent-after-the-call-started"></a>Si se ha detenido la señal de vídeo mientras la llamada está en el estado de conexión, el vídeo no se enviará después de iniciar la llamada 
Si los usuarios deciden activar o desactivar rápidamente el vídeo mientras la llamada está en estado `Connecting`, puede producirse un problema con la secuencia adquirida en la llamada. Animamos a los desarrolladores a crear sus aplicaciones de forma que no haga falta activar o desactivar el vídeo mientras la llamada está en estado `Connecting`. Este problema puede hacer que el rendimiento de vídeo se degrade en los siguientes escenarios:

 - Si el usuario comienza con audio y, luego, inicia y detiene el vídeo mientras la llamada está en estado `Connecting`.
 - Si el usuario comienza con audio y, luego, inicia y detiene el vídeo mientras la llamada está en estado `Lobby`.

##### <a name="possible-causes"></a>Causas posibles
Bajo investigación.

#### <a name="enumeratingaccessing-devices-for-safari-on-macos-and-ios"></a>Enumeración y acceso a dispositivos para Safari en MacOS e iOS 
Si se concede acceso a los dispositivos después de un tiempo, se restablecen los permisos de los dispositivos. Safari en MacOS y en iOS no conserva los permisos durante mucho tiempo a menos que haya una secuencia adquirida. La manera más sencilla de solucionar este problema es llamar a la API DeviceManager.askDevicePermission() antes de llamar a las API de enumeración de dispositivos del Administrador de dispositivos [DeviceManager.getCameras(), DeviceManager.getSpeakers() y DeviceManager.getMicrophones()]. Si los permisos están allí, el usuario no verá nada; de lo contrario, se le volverán a solicitar.

<br/>Dispositivos afectados: iPhone
<br/>Biblioteca cliente: Calling (JavaScript)
<br/>Exploradores: Safari
<br/>Sistema operativo: iOS

####  <a name="sometimes-it-takes-a-long-time-to-render-remote-participant-videos"></a>A veces se tarda mucho tiempo en presentar los vídeos de participantes remotos
Durante una llamada de grupo en curso, el _usuario A_ envía el vídeo y, luego, el _usuario B_ se une a la llamada. A veces, el usuario B no ve el vídeo del usuario A, o el vídeo del usuario A comienza a representarse después de un retraso largo. Este problema puede deberse a que el entorno de red requiere una configuración adicional. Consulte la documentación sobre los [requisitos de red](./voice-video-calling/network-requirements.md) para obtener instrucciones sobre la configuración de red.

#### <a name="using-3rd-party-libraries-to-access-gum-during-the-call-may-result-in-audio-loss"></a>El uso de bibliotecas de terceros para acceder a GUM durante la llamada puede provocar una pérdida de audio
El uso de getUserMedia por separado dentro de la aplicación provocará la pérdida de secuencias de audio, ya que una biblioteca de terceros se hace cargo del acceso al dispositivo desde la biblioteca de ACS.
Se recomienda a los desarrolladores que hagan lo siguiente:
- No use bibliotecas de terceros que usen internamente GetUserMedia API durante la llamada.
- Si todavía necesita usar la biblioteca de terceros, la única manera de recuperarla es cambiar el dispositivo seleccionado (si el usuario tiene más de uno) o reiniciar la llamada.

<br/>Exploradores: Safari
<br/>Sistema operativo: iOS

##### <a name="possible-causes"></a>Causas posibles
En algunos exploradores (Safari, por ejemplo), la adquisición de su propia secuencia desde el mismo dispositivo tendrá un efecto secundario de ejecutarse en condiciones de carrera. La adquisición de flujos de otros dispositivos puede provocar que el usuario tenga un ancho de banda de USB o E/S insuficiente y que la tasa de sourceUnavailableError se dispare.  

#### <a name="support-for-simulcast"></a>Compatibilidad con Simulcast
Simulcast es una técnica por la que un cliente codifica la misma secuencia de vídeo dos veces en distintas resoluciones y velocidades de bits y permite que la infraestructura de ACS decida qué secuencia debe recibir un cliente. El SDK de la biblioteca de llamadas de ACS para Windows, Android o iOS admite el envío de secuencias de Simulcast. El SDK web de ACS no admite actualmente el envío de secuencias de Simulcast.

## <a name="azure-communication-services-call-automation-apis"></a>API de automatización de llamadas de Azure Communication Services

Estos son los problemas conocidos de las API de automatización de llamadas de Azure Communication Services.

- La única autenticación admitida en este momento para las aplicaciones de servidor consiste en usar una cadena de conexión.

- Las llamadas solo se deben realizar entre entidades del mismo recurso de Azure Communication Service. La comunicación entre recursos está bloqueada.

- No se permiten llamadas entre usuarios de inquilinos de Teams y entidades de aplicación de servidor o usuario de Azure Communication Service.

- Si una aplicación llama a dos o más identidades RTC y, después, sale de la llamada, se cancelará la llamada entre las otras entidades RTC.

