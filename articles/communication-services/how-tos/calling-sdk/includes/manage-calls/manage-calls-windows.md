---
author: probableprime
ms.service: azure-communication-services
ms.topic: include
ms.date: 09/08/2021
ms.author: rifox
ms.openlocfilehash: 9737c4e756e034c88800264e5cc56a4ca1f81741
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131253661"
---
[!INCLUDE [Install SDK](../install-sdk/install-sdk-windows.md)]

### <a name="request-access-to-the-microphone"></a>Solicitud de acceso al micrófono

La aplicación requerirá acceso al micrófono para ejecutarse correctamente. En las aplicaciones para UWP, la funcionalidad de micrófono debe declararse en el archivo de manifiesto de la aplicación. Los pasos siguientes ejemplifican cómo lograrlo.

1. En el panel `Solution Explorer`, haga doble clic en el archivo con la extensión `.appxmanifest`.
2. Haga clic en la pestaña `Capabilities`.
3. Active la casilla `Microphone` de la lista de funcionalidades.

### <a name="create-ui-buttons-to-place-and-hang-up-the-call"></a>Creación de botones de UI para realizar la llamada y colgar

Esta sencilla aplicación de ejemplo incluirá dos botones. Uno para realizar la llamada y otro para colgar una llamada realizada.
Los pasos siguientes ejemplifican cómo agregar estos botones a la aplicación.

1. En el panel `Solution Explorer`, haga doble clic en el archivo denominado `MainPage.xaml`.
2. En el panel central, busque el código XAML en la versión preliminar de la interfaz de usuario.
3. Reemplace el código XAML de `<Grid>` a `</Grid>` por el fragmento siguiente:
```xml
<StackPanel Orientation="Horizontal" VerticalAlignment="Center" HorizontalAlignment="Center">
    <Button x:Name="callButton" Click="CallHandler" Margin="10,10,10,10" HorizontalAlignment="Stretch" VerticalAlignment="Stretch">Call</Button>
    <Button x:Name="hangupButton" Click="HangupHandler" Margin="10,10,10,10" HorizontalAlignment="Stretch" VerticalAlignment="Stretch">Hang up</Button>
</StackPanel>
```

### <a name="setting-up-the-app-with-calling-sdk-apis"></a>Configuración de la aplicación con las API de Calling SDK

Las API de Calling SDK se encuentran en dos espacios de nombres diferentes.
Los pasos siguientes informan al compilador de C# sobre estos espacios de nombres, lo que permite a IntelliSense de Visual Studio ayudar con el desarrollo del código.

1. En el panel `Solution Explorer`, haga clic en la flecha del lado izquierdo del archivo denominado `MainPage.xaml`.
2. Haga doble clic en el archivo denominado `MainPage.xaml.cs` que apareció.
3. Agregue los siguientes comandos al final de las instrucciones `using` actuales.

```csharp
using Azure.Communication;
using Azure.Communication.Calling;
```

Mantenga `MainPage.xaml.cs` abierto. En los pasos siguientes se agregará más código.

## <a name="allow-app-interactions"></a>Habilitación de interacciones de aplicaciones

Los botones de la interfaz de usuario agregados anteriormente tienen que funcionar sobre un `Call` colocado. Esto significa que un miembro de datos `Call` se debe agregar a la clase `MainPage`.
Además, para permitir que la operación asincrónica que se crea `CallAgent` se complete correctamente, también tiene que agregar un miembro de datos `CallAgent` a la misma clase.

Agregue los miembros de datos siguientes a la clase `MainPage`:
```csharp
CallAgent agent_;
Call call_;
```

## <a name="create-button-handlers"></a>Creación de controladores de botones

Anteriormente, se agregaron dos botones de interfaz de usuario al código XAML. En el código siguiente, se agregan los controladores que se ejecutarán cuando un usuario seleccione el botón.
El código siguiente debe agregarse después de los miembros de datos de la sección anterior.

```csharp
private void CallHandler(object sender, RoutedEventArgs e)
{
}

private void HangupHandler(object sender, RoutedEventArgs e)
{
}
```

## <a name="object-model"></a>Modelo de objetos

Las clases e interfaces siguientes controlan algunas de las características principales de la biblioteca cliente para llamadas de Azure Communication Services para UWP.

| Nombre                                  | Descripción                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| CallClient | CallClient es el punto de entrada principal a la biblioteca cliente para llamadas. |
| CallAgent | CallAgent se usa para iniciar llamadas y unirse a estas. |
| Call | Call se usa para administrar las llamadas realizadas o a las que se ha unido. |
| CommunicationTokenCredential | CommunicationTokenCredential se usa como la credencial de token para crear una instancia de CallAgent.|
| CallAgentOptions | CallAgentOptions contiene información para identificar al autor de la llamada. |
| HangupOptions | HangupOptions informa si se debe finalizar una llamada para todos los participantes. |

## <a name="initialize-the-callagent"></a>Inicialización de CallAgent

Para crear una instancia de `CallAgent` a partir de `CallClient`, tiene que usar el método `CallClient.CreateCallAgent` que devuelve de manera asincrónica un objeto `CallAgent` una vez que se inicializa.

Para crear `CallAgent`, tiene que pasar un objeto `CommunicationTokenCredential` y un objeto `CallAgentOptions`. Tenga en cuenta que se produce un error de `CommunicationTokenCredential` si se pasa un token con formato incorrecto.

El código siguiente debe agregarse dentro de `CallHandler`.

```csharp
CallClient client = new CallClient();
CommunicationTokenCredential credentials;

CallAgentOptions callAgentOptions = new CallAgentOptions
{
  DisplayName = "<CALLER NAME>"
};

try
{
    credentials = new CommunicationTokenCredential("<CREDENTIAL TOKEN>");
}
catch (Exception)
{
    throw new Exception("Invalid credential token");
}
```

`<USER ACCESS TOKEN>` debe reemplazarse por un token de credencial válido para el recurso. Consulte la documentación sobre [tokens de acceso de usuario](../../../../quickstarts/access-tokens.md) si es necesario proporcionar un token de credencial.

## <a name="create-callagent-and-place-a-call"></a>Creación de CallAgent y realización de una llamada

Los objetos necesarios para crear `CallAgent` ya están listos. Es el momento de crear `CallAgent` de manera asincrónica y realizar una llamada.

El código siguiente debe agregarse después de controlar la excepción del paso anterior.

```csharp
client.CreateCallAgent(creds, callAgentOptions).Completed +=
async (IAsyncOperation<CallAgent> asyncInfo, AsyncStatus asyncStatus) =>
{
    agent_ = asyncInfo.GetResults();

    CommunicationUserIdentifier target = new CommunicationUserIdentifier("<CALLEE>");

    StartCallOptions startCallOptions = new StartCallOptions();
    call_ = await agent_.StartCallAsync(new List<ICommunicationIdentifier>() { target }, startCallOptions);
};
```

Reemplace `<CALLEE>` por cualquier otra identidad del inquilino. Como alternativa, no dude en usar `8:echo123` para hablar con el bot de eco de ACS.

## <a name="end-a-call"></a>Finalizar una llamada

Una vez realizada una llamada, se debe usar el método `Hangup` del objeto `Call` para colgar la llamada.

También se debe usar una instancia de `HangupOptions` para informar si la llamada se debe finalizar para todos sus participantes.

El código siguiente debe agregarse dentro de `HangupHandler`.

```csharp
HangUpOptions hangupOptions = new HangUpOptions();
call_.HangUpAsync(hangupOptions).Completed += (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{

};
```

## <a name="run-the-code"></a>Ejecución del código

Asegúrese de que Visual Studio compilará la aplicación para `x64`, `x86` o `ARM64`, y presione `F5` para empezar a ejecutar la aplicación. Después de esto, haga clic en el botón `Call` para realizar una llamada al destinatario definido.

Tenga en cuenta que la primera vez que se ejecuta la aplicación, el sistema solicitará al usuario que conceda acceso al micrófono.