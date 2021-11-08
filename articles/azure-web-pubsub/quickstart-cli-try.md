---
title: 'Inicio rápido: Conexión y reproducción con la instancia de Azure Web PubSub'
description: Inicio rápido en el que se muestra cómo reproducir con la instancia desde la CLI de Azure
author: vicancy
ms.author: lianwei
ms.service: azure-web-pubsub
ms.topic: quickstart
ms.date: 11/08/2021
ms.openlocfilehash: 99045ab4675527f4f0a821025bf6f870d63bb296
ms.sourcegitcommit: 27ddccfa351f574431fb4775e5cd486eb21080e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2021
ms.locfileid: "131998080"
---
# <a name="quickstart-connect-to-the-azure-web-pubsub-instance-from-cli"></a>Inicio rápido: Conexión a la instancia de Azure Web PubSub desde la CLI

En este inicio rápido, se muestra cómo conectarse a la instancia de Azure Web PubSub y publicar mensajes en los clientes conectados mediante la [interfaz de la línea de comandos de Azure (CLI de Azure)](/cli/azure).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- Para realizar este inicio rápido es necesaria la versión 2.22.0 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

[!INCLUDE [Create a resource group](includes/cli-rg-creation.md)]

## <a name="create-a-web-pubsub-instance"></a>Creación de una instancia de Web PubSub

[!INCLUDE [Create a Web PubSub instance](includes/cli-awps-creation.md)]

## <a name="play-with-the-instance"></a>Reproducción con la instancia

[!INCLUDE [Try the Web PubSub instance](includes/cli-awps-client.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, se proporciona una idea básica de cómo conectarse al servicio Web PubSub y cómo publicar mensajes en los clientes conectados.

En las aplicaciones del mundo real, puede usar SDK en varios lenguajes para crear su propia aplicación. También proporcionamos extensiones de función para que pueda crear aplicaciones sin servidor fácilmente.

[!INCLUDE [next step](includes/include-next-step.md)]
