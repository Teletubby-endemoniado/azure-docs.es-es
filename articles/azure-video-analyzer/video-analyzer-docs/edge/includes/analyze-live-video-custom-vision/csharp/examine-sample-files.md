---
author: Juliako
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 11/04/2021
ms.author: juliako
ms.openlocfilehash: ad0a03aae3cee71ed98a790c1b75c855dbf62453
ms.sourcegitcommit: 8946cfadd89ce8830ebfe358145fd37c0dc4d10e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2021
ms.locfileid: "131860395"
---
1. En Visual Studio Code, vaya a src/edge. Verá el archivo .env que ha creado junto con algunos archivos de plantilla de implementación.

   La plantilla de implementación hace referencia al manifiesto de implementación del dispositivo perimetral con algunos valores de marcador de posición. El archivo .env contiene los valores de esas variables.
2. A continuación, vaya a la carpeta src/cloud-to-device-console-app. Aquí podrá ver el archivo appsettings.json que creó junto con algunos otros archivos:

   - c2d-console-app.csproj: archivo de proyecto de Visual Studio Code.
   - operations.json: en este archivo se enumeran las distintas operaciones que desea que ejecute el programa.
   - Program.cs: Este código de programa de ejemplo:
     - Carga la configuración de la aplicación.
     - Invoca los métodos directos del módulo de Azure Video Analyzer para crear una topología, crear una instancia de la canalización y activarla.
     - Se pone en pausa para que pueda examinar la salida de la canalización en la ventana **TERMINAL** y los eventos enviados a IoT Hub en la ventana **SALIDA**.
     - Desactiva la canalización en directo, la elimina y elimina la topología.
