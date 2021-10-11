---
title: 'Inicio rápido: unirse a una reunión de Teams'
author: askaur
ms.author: askaur
ms.date: 06/30/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 616741c97b1e133dd9027b8622c181a9ca622b72
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129378700"
---
En este inicio rápido aprenderá a chatear en una reunión de Teams mediante el SDK de chat de Azure Communication Services para JavaScript.

## <a name="sample-code"></a>Código de ejemplo
Busque el código finalizado de este inicio rápido en [GitHub](https://github.com/Azure-Samples/communication-services-javascript-quickstarts/tree/main/join-chat-to-teams-meeting).

## <a name="prerequisites"></a>Requisitos previos 

* Una  [implementación de Teams](/deployoffice/teams-install). 
* Una [aplicación de chat](../get-started.md) activa. 

## <a name="joining-the-meeting-chat"></a>Unión al chat de la reunión 

Un usuario de Communication Services puede unirse a una reunión de Teams como un usuario anónimo mediante el SDK de llamada. Al unirse a la reunión, se le agregará también como participante en el chat de la reunión, donde podrá enviar mensajes a otros usuarios durante la reunión, y recibirlos de ellos. El usuario no tendrá acceso a los mensajes de chat que se enviaron antes de unirse a la reunión y no podrá enviar ni recibir mensajes una vez que finalice la esta. Para unirse a la reunión e iniciar el chat, puede seguir los pasos siguientes.

## <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js

Abra la ventana de comandos o el terminal, cree un nuevo directorio para la aplicación y navegue hasta él.

```console
mkdir chat-interop-quickstart && cd chat-interop-quickstart
```

Ejecute `npm init -y` para crear un archivo **package.json** con la configuración predeterminada.

```console
npm init -y
```

## <a name="install-the-chat-packages"></a>Instalación de los paquetes de chat

Use el comando `npm install` para instalar los SDK de Communication Services necesarios para JavaScript.

```console
npm install @azure/communication-common --save

npm install @azure/communication-identity --save

npm install @azure/communication-signaling --save

npm install @azure/communication-chat --save

npm install @azure/communication-calling --save
```

La opción `--save` muestra la biblioteca como una dependencia en el archivo **package.json**.

## <a name="set-up-the-app-framework"></a>Instalación del marco de la aplicación

Esta guía de inicio rápido usa webpack para agrupar los recursos de la aplicación. Ejecute el siguiente comando para instalar los paquetes de npm webpack, webpack-cli y webpack-dev-server, y mostrarlos como dependencias de desarrollo en el archivo **package.json**:

```console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```

Cree un archivo **index.html** en el directorio raíz del proyecto. Este archivo lo usaremos para configurar un diseño básico que permitirá al usuario unirse a una reunión y comenzar a chatear.

## <a name="add-the-teams-ui-controls"></a>Incorporación de los controles de la interfaz de usuario de Teams

Reemplace el código de index.html por el siguiente fragmento de código.
Los cuadros de texto de la parte superior de la página se usarán para especificar el contexto de la reunión de Teams y el identificador de la conversación de la reunión. El botón "Unirse a una reunión de Teams" se usará para unirse a la reunión especificada.
Aparecerá una ventana emergente de chat en la parte inferior de la página. Se puede usar para enviar mensajes en la conversación de la reunión, y los mensajes enviados se mostrarán en tiempo real en dicha conversación mientras el usuario de Communication Services sea miembro de esta.

```html
<!DOCTYPE html>
<html>
   <head>
      <title>Communication Client - Calling and Chat Sample</title>
      <style>
         body {box-sizing: border-box;}
         /* The popup chat - hidden by default */
         .chat-popup {
         display: none;
         position: fixed;
         bottom: 0;
         left: 15px;
         border: 3px solid #f1f1f1;
         z-index: 9;
         }
         .message-box {
         display: none;
         position: fixed;
         bottom: 0;
         left: 15px;
         border: 3px solid #FFFACD;
         z-index: 9;
         }
         .form-container {
         max-width: 300px;
         padding: 10px;
         background-color: white;
         }
         .form-container textarea {
         width: 90%;
         padding: 15px;
         margin: 5px 0 22px 0;
         border: none;
         background: #e1e1e1;
         resize: none;
         min-height: 50px;
         }
         .form-container .btn {
         background-color: #4CAF40;
         color: white;
         padding: 14px 18px;
         margin-bottom:10px;
         opacity: 0.6;
         border: none;
         cursor: pointer;
         width: 100%;
         }
         .container {
         border: 1px solid #dedede;
         background-color: #F1F1F1;
         border-radius: 3px;
         padding: 8px;
         margin: 8px 0;
         }
         .darker {
         border-color: #ccc;
         background-color: #ffdab9;
         margin-left: 25px;
         margin-right: 3px;
         }
         .lighter {
         margin-right: 20px;
         margin-left: 3px;
         }
         .container::after {
         content: "";
         clear: both;
         display: table;
         }
      </style>
   </head>
   <body>
      <h4>Azure Communication Services</h4>
      <h1>Calling and Chat Quickstart</h1>
          <input id="teams-link-input" type="text" placeholder="Teams meeting link"
        style="margin-bottom:1em; width: 300px;" />
          <input id="thread-id-input" type="text" placeholder="Chat thread id"
        style="margin-bottom:1em; width: 300px;" />
        <p>Call state <span style="font-weight: bold" id="call-state">-</span></p>
      <div>
        <button id="join-meeting-button" type="button">
            Join Teams Meeting
        </button>
        <button id="hang-up-button" type="button" disabled="true">
            Hang Up
        </button>
      </div>
      <div class="chat-popup" id="chat-box">
         <div id="messages-container"></div>
         <form class="form-container">
            <textarea placeholder="Type message.." name="msg" id="message-box" required></textarea>
            <button type="button" class="btn" id="send-message">Send</button>
         </form>
      </div>
      <script src="./bundle.js"></script>
   </body>
</html>
```

## <a name="enable-the-teams-ui-controls"></a>Habilitación de los controles de la interfaz de usuario de Teams

Reemplace el contenido del archivo client.js por el siguiente fragmento de código.

En el fragmento de código, reemplace lo siguiente: 
- `SECRET_CONNECTION_STRING` por la cadena de conexión de Communication Services. 
- `ENDPOINT_URL` por la URL del punto de conexión de Communication Services.

```javascript
import { CallClient, CallAgent } from "@azure/communication-calling";
import { AzureCommunicationTokenCredential } from "@azure/communication-common";
import { CommunicationIdentityClient } from "@azure/communication-identity";
import { ChatClient } from "@azure/communication-chat";

let call;
let callAgent;
let chatClient;
let chatThreadClient;

const meetingLinkInput = document.getElementById('teams-link-input');
const threadIdInput = document.getElementById('thread-id-input');
const callButton = document.getElementById("join-meeting-button");
const hangUpButton = document.getElementById("hang-up-button");
const callStateElement = document.getElementById('call-state');

const messagesContainer = document.getElementById("messages-container");
const chatBox = document.getElementById("chat-box");
const sendMessageButton = document.getElementById("send-message");
const messagebox = document.getElementById("message-box");

var userId = '';
var lastMessage = '';
var previousMessage = '';

async function init() {
  const connectionString = "<SECRET_CONNECTION_STRING>";
  const endpointUrl = "<ENDPOINT_URL>";

  const identityClient = new CommunicationIdentityClient(connectionString);

  let identityResponse = await identityClient.createUser();
  userId = identityResponse.communicationUserId;
  console.log(`\nCreated an identity with ID: ${identityResponse.communicationUserId}`);

  let tokenResponse = await identityClient.getToken(identityResponse, [
    "voip",
    "chat",
  ]);

  const { token, expiresOn } = tokenResponse;
  console.log(`\nIssued an access token that expires at: ${expiresOn}`);
  console.log(token);

  const callClient = new CallClient();
  const tokenCredential = new AzureCommunicationTokenCredential(token);
  callAgent = await callClient.createCallAgent(tokenCredential);
  callButton.disabled = false;

  chatClient = new ChatClient(
    endpointUrl,
    new AzureCommunicationTokenCredential(token)
  );

  console.log('Azure Communication Chat client created!');
}

init();

callButton.addEventListener("click", async () => {
  // join with meeting link
  call = callAgent.join({meetingLink: meetingLinkInput.value}, {});

  call.on('stateChanged', () => {
      callStateElement.innerText = call.state;
  })
  // toggle button and chat box states
  chatBox.style.display = "block";
  hangUpButton.disabled = false;
  callButton.disabled = true;

  messagesContainer.innerHTML = "";

  console.log(call);

  // open notifications channel
  await chatClient.startRealtimeNotifications();

  // subscribe to new message notifications
  chatClient.on("chatMessageReceived", (e) => {
    console.log("Notification chatMessageReceived!");

      // check whether the notification is intended for the current thread
    if (threadIdInput.value != e.threadId) {
      return;
    }

    if (e.sender.communicationUserId != userId) {
       renderReceivedMessage(e.message);
    }
    else {
       renderSentMessage(e.message);
    }
  });

  chatThreadClient = await chatClient.getChatThreadClient(threadIdInput.value);
});

async function renderReceivedMessage(message) {
  previousMessage = lastMessage;
  lastMessage = '<div class="container lighter">' + message + '</div>';
  messagesContainer.innerHTML = previousMessage + lastMessage;
}

async function renderSentMessage(message) {
  previousMessage = lastMessage;
  lastMessage = '<div class="container darker">' + message + '</div>';
  messagesContainer.innerHTML = previousMessage + lastMessage;
}

hangUpButton.addEventListener("click", async () => 
  {
    // end the current call
    await call.hangUp();

    // toggle button states
    hangUpButton.disabled = true;
    callButton.disabled = false;
    callStateElement.innerText = '-';

    // toggle chat states
    chatBox.style.display = "none";
    messages = "";
  });

sendMessageButton.addEventListener("click", async () =>
  {
    let message = messagebox.value;

    let sendMessageRequest = { content: message };
    let sendMessageOptions = { senderDisplayName : 'Jack' };
    let sendChatMessageResult = await chatThreadClient.sendMessage(sendMessageRequest, sendMessageOptions);
    let messageId = sendChatMessageResult.id;

    messagebox.value = '';
    console.log(`Message sent!, message id:${messageId}`);
  });
```

El cliente de Teams no establece los nombres para mostrar de los participantes de la conversación del chat. Los nombres se devolverán como NULL en la API para los participantes de la enumeración, en el evento `participantsAdded` y en el evento `participantsRemoved`. Los nombres para mostrar de los participantes del chat se pueden recuperar del campo `remoteParticipants` del objeto `call`. Al recibir una notificación sobre un cambio en la lista, puede usar este código para recuperar el nombre del usuario que se agregó o quitó:

```
var displayName = call.remoteParticipants.find(p => p.identifier.communicationUserId == '<REMOTE_USER_ID>').displayName;
```

## <a name="get-a-teams-meeting-chat-thread-for-a-communication-services-user"></a>Obtención de un subproceso del chat de la reunión para un usuario de Communication Services

Los detalles de la reunión de Teams se pueden recuperar mediante las instancias de Graph API, que se detallan en la [documentación de Graph](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta&preserve-view=true). El SDK de llamada de Communication Services acepta un vínculo completo a la reunión de Teams o un identificador de reunión. Ambos elementos se devuelven como parte del recurso `onlineMeeting`, al que se puede acceder bajo la [`joinWebUrl`propiedad](/graph/api/resources/onlinemeeting?view=graph-rest-beta&preserve-view=true).

Con [Graph API](/graph/api/onlinemeeting-createorget?tabs=http&view=graph-rest-beta&preserve-view=true), también se puede obtener `threadID`. La respuesta tendrá un objeto `chatInfo` que contiene el elemento `threadID`. 

También puede obtener la información de la reunión necesaria y el identificador de la conversación en la dirección URL **Unirse a la reunión** de la propia invitación a la reunión de Teams.
Así es el vínculo de una reunión en Teams: `https://teams.microsoft.com/l/meetup-join/meeting_chat_thread_id/1606337455313?context=some_context_here`. El valor de `threadId` estará donde se encuentra el elemento `meeting_chat_thread_id` en el vínculo. Asegúrese de que el elemento `meeting_chat_thread_id` se haya escapado antes de utilizarlo. Debería tener el siguiente formato: `19:meeting_ZWRhZDY4ZGUtYmRlNS00OWZaLTlkZTgtZWRiYjIxOWI2NTQ4@thread.v2`.


## <a name="run-the-code"></a>Ejecución del código

Los usuarios de WebPack pueden usar `webpack-dev-server` para compilar y ejecutar la aplicación. Ejecute el siguiente comando para agrupar el host de aplicación en un servidor web local:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Abra el explorador web y vaya a http://localhost:8080/. Verá lo siguiente:

:::image type="content" source="../join-teams-meeting-chat-quickstart.png" alt-text="Captura de pantalla de la aplicación JavaScript completada.":::

Inserte el vínculo de la reunión de Teams y el identificador de la conversación en los cuadros de texto. Presione *Unirse a una reunión de Teams* para unirse a dicha reunión. Una vez que el usuario de Communication Services se haya admitido en la reunión, puede chatear desde dentro de la aplicación de Communication Services. Navegue hasta el cuadro que hay en la parte inferior de la página para iniciar el chat. Por simplicidad, la aplicación solo mostrará los dos últimos mensajes en el chat.

> [!NOTE] 
> Actualmente, en los escenarios de interoperabilidad con Teams solo se admiten el envío, la recepción, la edición de mensajes y el envío de notificaciones de escritura. Otras características, como las confirmaciones de lectura y que los usuarios de Communication Services agreguen o quiten otros usuarios de la reunión de Teams, no se admiten.
