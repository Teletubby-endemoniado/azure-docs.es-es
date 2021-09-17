---
title: Inicio rápido de la biblioteca cliente de Node.js para Lector inmersivo
titleSuffix: Azure Applied AI Services
description: En este inicio rápido, va a crear una aplicación web desde cero y a agregar la funcionalidad de la API de Lector inmersivo.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: applied-ai-services
ms.subservice: immersive-reader
ms.topic: include
ms.date: 09/14/2020
ms.author: nitinme
ms.custom: devx-track-js
ms.openlocfilehash: de4dda828b821db66d89387f1f63bf20372fd005
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121786689"
---
[Lector inmersivo](https://www.onenote.com/learningtools) es una herramienta diseñada de manera inclusiva que implementa técnicas demostradas para mejorar la comprensión lectora de nuevos lectores, estudiantes de idiomas y personas con dificultades de aprendizaje, como la dislexia. Puede usar Lector inmersivo en sus aplicaciones para aislar el texto con el fin de mejorar la concentración, mostrar imágenes para palabras de uso frecuente, resaltar partes del texto, leer texto seleccionado en voz alta, traducir palabras y frases en tiempo real y mucho más.

En este inicio rápido, se va a crear una aplicación web desde cero y se integrará el Lector inmersivo mediante la biblioteca cliente de esta herramienta. Hay disponible un ejemplo funcional completo de esta guía de inicio rápido [en GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/quickstart-nodejs).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services)
* Un recurso del Lector inmersivo configurado para la autenticación de Azure Active Directory. Siga [estas instrucciones](../../how-to-create-immersive-reader.md) para realizar la configuración. Necesitará algunos de los valores que se crean aquí cuando configure las propiedades del entorno. Guarde la salida de la sesión en un archivo de texto para futuras referencias.
* [Node.js](https://nodejs.org/) y [Yarn](https://yarnpkg.com)
* Un IDE como [Visual Studio Code](https://code.visualstudio.com/)

## <a name="create-a-nodejs-web-app-with-express"></a>Crear una aplicación web de Node.js con Express

Cree una aplicación web de Node.js con la herramienta `express-generator`.

```bash
npm install express-generator -g
express --view=pug quickstart-nodejs
cd quickstart-nodejs
```

Instale las dependencias de Yarn y agregue las dependencias `request` y `dotenv`, que se utilizarán más adelante en el inicio rápido.

```bash
yarn
yarn add request
yarn add dotenv
```

Puede instalar las bibliotecas **axios** y **qs** con el comando siguiente:

```bash
npm install axios qs
```

## <a name="set-up-authentication"></a>Configuración de la autenticación

### <a name="configure-authentication-values"></a>Configuración de los valores de autenticación

Cree un nuevo archivo denominado _. env_ en la raíz del proyecto. Pegue en él el siguiente código, y proporcione los valores especificados al crear el recurso del Lector inmersivo.
No incluya comillas ni los caracteres "{" y "}".

```text
TENANT_ID={YOUR_TENANT_ID}
CLIENT_ID={YOUR_CLIENT_ID}
CLIENT_SECRET={YOUR_CLIENT_SECRET}
SUBDOMAIN={YOUR_SUBDOMAIN}
```

Asegúrese de no confirmar este archivo en el control de código fuente, ya que contiene secretos que no deben hacerse públicos.

Después, abra el archivo _app.js_ y agregue lo siente en la parte superior. Esto carga las propiedades definidas en el archivo .env como variables de entorno en el nodo.

```javascript
require('dotenv').config();
```

### <a name="update-the-router-to-acquire-the-token"></a>Actualización del enrutador para adquirir el token

Abra el archivo _routes\index.js_ y reemplace el código generado automáticamente por el código siguiente.

Este código crea un punto de conexión de la API que adquiere un token de autenticación de Azure AD mediante la contraseña de la entidad de servicio. También recupera el subdominio. Luego, devuelve un objeto que contiene el token y el subdominio.

```javascript
var axios = require('axios');
var express = require('express');
var router = express.Router();
var qs = require('qs');

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

router.get('/GetTokenAndSubdomain', function(req, res) {
    try {
        var config ={
            headers: {
                'content-type': 'application/x-www-form-urlencoded'
            }
        }
        var data = {
            grant_type: 'client_credentials',
            client_id: process.env.CLIENT_ID,
            client_secret: process.env.CLIENT_SECRET,
            resource: 'https://cognitiveservices.azure.com/'
        };
        var url = `https://login.windows.net/${process.env.TENANT_ID}/oauth2/token`
        console.log(qs.stringify(data));
        axios.post(url, qs.stringify(data), config)
        .then(function (response) {
            var token = response.data.access_token;
            var subdomain = process.env.SUBDOMAIN;
            return res.send({token, subdomain});
        })
        .catch(function (response) {
            if (response.status !== 200) {
                return res.send({error :  "Unable to acquire Azure AD token. Check the debugger for more information."})
            }
        });
    } catch (error) {
        console.log(error);
        return res.status(500).send('CogSvcs IssueToken error');
    }
});

module.exports = router;
```

El punto de conexión de la API **GetTokenAndSubdomain** debe estar protegido mediante algún tipo de autenticación (por ejemplo, [OAuth](https://oauth.net/2/)) para evitar que usuarios no autorizados obtengan tokens y los utilicen en el servicio Lector inmersivo y su facturación; este trabajo queda fuera del ámbito de este inicio rápido.

## <a name="add-sample-content"></a>Adición de contenido de ejemplo

Ahora, vamos a agregar contenido de ejemplo a esta aplicación web. Abra _views\index.pug_ y reemplace el código generado automáticamente por este ejemplo:

```pug
doctype html
html
   head
      title Immersive Reader Quickstart Node.js

      link(rel='icon', href='data:;base64,iVBORw0KGgo=')

      link(rel='stylesheet', href='https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css')

      // A polyfill for Promise is needed for IE11 support.
      script(src='https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js')

      script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.1.1.0.js')
      script(src='https://code.jquery.com/jquery-3.3.1.min.js')

      style(type="text/css").
        .immersive-reader-button {
          background-color: white;
          margin-top: 5px;
          border: 1px solid black;
          float: right;
        }
   body
      div(class="container")
        button(class="immersive-reader-button" data-button-style="iconAndText" data-locale="en")

        h1(id="ir-title") About Immersive Reader
        div(id="ir-content" lang="en-us")
          p Immersive Reader is a tool that implements proven techniques to improve reading comprehension for emerging readers, language learners, and people with learning differences. The Immersive Reader is designed to make reading more accessible for everyone. The Immersive Reader

            ul
                li Shows content in a minimal reading view
                li Displays pictures of commonly used words
                li Highlights nouns, verbs, adjectives, and adverbs
                li Reads your content out loud to you
                li Translates your content into another language
                li Breaks down words into syllables

          h3 The Immersive Reader is available in many languages.

          p(lang="es-es") El Lector inmersivo está disponible en varios idiomas.
          p(lang="zh-cn") 沉浸式阅读器支持许多语言
          p(lang="de-de") Der plastische Reader ist in vielen Sprachen verfügbar.
          p(lang="ar-eg" dir="rtl" style="text-align:right") يتوفر \"القارئ الشامل\" في العديد من اللغات.

script(type="text/javascript").
  function getTokenAndSubdomainAsync() {
        return new Promise(function (resolve, reject) {
            $.ajax({
                url: "/GetTokenAndSubdomain",
                type: "GET",
                success: function (data) {
                    if (data.error) {
                        reject(data.error);
                    } else {
                        resolve(data);
                    }
                },
                error: function (err) {
                    reject(err);
                }
            });
        });
    }

    $(".immersive-reader-button").click(function () {
        handleLaunchImmersiveReader();
    });

    function handleLaunchImmersiveReader() {
        getTokenAndSubdomainAsync()
            .then(function (response) {
                const token = response["token"];
                const subdomain = response["subdomain"];
                // Learn more about chunk usage and supported MIME types https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#chunk
                const data = {
                    title: $("#ir-title").text(),
                    chunks: [{
                        content: $("#ir-content").html(),
                        mimeType: "text/html"
                    }]
                };
                // Learn more about options https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#options
                const options = {
                    "onExit": exitCallback,
                    "uiZIndex": 2000
                };
                ImmersiveReader.launchAsync(token, subdomain, data, options)
                    .catch(function (error) {
                        alert("Error in launching the Immersive Reader. Check the console.");
                        console.log(error);
                    });
            })
            .catch(function (error) {
                alert("Error in getting the Immersive Reader token and subdomain. Check the console.");
                console.log(error);
            });
    }

    function exitCallback() {
        console.log("This is the callback function. It is executed when the Immersive Reader closes.");
    }
```

Tenga en cuenta que todo el texto tiene un atributo **lang**, que describe los idiomas del texto. Este atributo ayuda al Lector inmersivo a proporcionar características de idioma y gramática pertinentes.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

Nuestra aplicación web ya está lista. Inicie la aplicación; para ello, ejecute:

```bash
npm start
```

Abra el explorador web y vaya a _http://localhost:3000_ . Verá lo siguiente:

![Aplicación de ejemplo: Node.js](../../media/quickstart-nodejs/1-buildapp.png)

## <a name="launch-the-immersive-reader"></a>Inicio del Lector inmersivo

Al hacer clic en el botón "Lector inmersivo", verá que se inicia dicha herramienta con el contenido de la página.

![Lector inmersivo: Node.js](../../media/quickstart-nodejs/2-viewimmersivereader.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear un recurso y configurar AAD](../../how-to-create-immersive-reader.md)