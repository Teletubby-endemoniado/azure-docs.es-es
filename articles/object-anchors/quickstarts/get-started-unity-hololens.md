---
title: 'Inicio rápido: Creación de una aplicación HoloLens con Unity'
description: En este inicio rápido, aprenderá a compilar una aplicación HoloLens en Unity con Object Anchors.
author: craigktreasure
manager: virivera
ms.author: crtreasu
ms.date: 09/08/2021
ms.topic: quickstart
ms.service: azure-object-anchors
ms.openlocfilehash: 53bcc84fc63666e64ffaf502a4348de87c1ab8c7
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2021
ms.locfileid: "124797009"
---
# <a name="quickstart-create-a-hololens-app-with-azure-object-anchors-in-unity"></a>Inicio rápido. Creación de una aplicación de HoloLens con Azure Object Anchors, en Unity

En este artículo de inicio rápido se creará una aplicación de HoloLens en Unity con [Azure Object Anchors](../overview.md). Azure Object Anchors es un servicio en la nube administrado que convierte recursos 3D en modelos de inteligencia artificial (IA) que permiten experiencias de realidad mixta que reconocen objetos para HoloLens. Cuando haya terminado, tendrá una aplicación de HoloLens compilada con Unity, que puede detectar objetos en el mundo físico.

Aprenderá a:

> [!div class="checklist"]
> * Preparar la configuración de compilación de Unity.
> * Exportar el proyecto HoloLens de Visual Studio.
> * Implementar la aplicación y ejecutarla en un dispositivo HoloLens 2.

[!INCLUDE [Unity quickstart prerequisites](../../../includes/object-anchors-quickstart-unity-prerequisites.md)]

[!INCLUDE [Create Account](../../../includes/object-anchors-get-started-create-account.md)]

[!INCLUDE [Unity device setup](../../../includes/object-anchors-quickstart-unity-device-setup.md)]

[!INCLUDE [Unity upload your model](../../../includes/object-anchors-quickstart-unity-upload-model.md)]

## <a name="open-the-sample-project"></a>Apertura del proyecto de ejemplo

[!INCLUDE [Clone Sample Repo](../../../includes/object-anchors-clone-sample-repository.md)]

[!INCLUDE [Download Unity Package](../../../includes/object-anchors-quickstart-unity-download-package.md)]

En Unity, abra el proyecto `quickstarts/apps/unity/basic`.

[!INCLUDE [Import Unity Package](../../../includes/object-anchors-quickstart-unity-import-package.md)]

[!INCLUDE [Configure Account](../../../includes/object-anchors-get-started-configure-account.md)]

[!INCLUDE [Unity build sample scene 1](../../../includes/object-anchors-quickstart-unity-build-sample-scene-1.md)]

[!INCLUDE [Unity build sample scene 2](../../../includes/object-anchors-quickstart-unity-build-sample-scene-2.md)]

[!INCLUDE [Unity build and deploy](../../../includes/object-anchors-quickstart-unity-build-deploy.md)]

Después de la pantalla de presentación de Unity, verá un mensaje que indica que se inicializó el observador de objeto.

La aplicación busca objetos en el campo visual actual y, a continuación, realiza un seguimiento de ellos una vez detectados. Una instancia se quitará cuando esté a 6 metros de la ubicación del usuario. El texto de depuración muestra detalles sobre una instancia, como el identificador, la marca de tiempo actualizada y la relación de cobertura de superficie.

[!INCLUDE [Unity troubleshooting](../../../includes/object-anchors-quickstart-unity-troubleshooting.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [HoloLens para Unity con MRTK](get-started-unity-hololens-mrtk.md)

> [!div class="nextstepaction"]
> [Conceptos: Información general del SDK](../concepts/sdk-overview.md)

> [!div class="nextstepaction"]
> [P+F](../faq.md)

> [!div class="nextstepaction"]
> [Biblioteca cliente de Azure Object Anchors para .NET: versión 0.3.0-beta.1](/dotnet/api/overview/azure/mixedreality.objectanchors.conversion-readme-pre)

> [!div class="nextstepaction"]
> [Solución de problemas de detección de objetos](../troubleshoot/object-detection.md)
