---
title: 'Copiado y pegado desde máquinas virtuales Windows: Azure Bastion'
description: Aprenda a copiar y pegar entre máquinas virtuales Windows mediante Bastion.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 08/30/2021
ms.author: cherylmc
ms.openlocfilehash: 5ce8faa76e1ddbd8d1d1adb52759dba0afe9c737
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128629623"
---
# <a name="copy-and-paste-to-a-windows-virtual-machine-azure-bastion"></a>Copiado y pegado a máquinas virtuales Windows: Azure Bastion

Este artículo le ayuda a copiar y pegar texto entre máquinas virtuales cuando se usa Azure Bastion. Antes de trabajar con una máquina virtual, asegúrese de que ha seguido los pasos indicados en [Creación de un host de Bastion](./tutorial-create-host-portal.md). Después, conéctese a la máquina virtual con la que quiere trabajar mediante [RDP](bastion-connect-vm-rdp-windows.md) o [SSH](bastion-connect-vm-ssh-windows.md).

En exploradores que admiten el acceso avanzado de la API de Portapapeles, puede copiar y pegar texto entre el dispositivo local y la sesión remota de la misma manera que copia y pega entre aplicaciones en el dispositivo local. En otros exploradores, puede utilizar la paleta de herramientas de acceso al Portapapeles de Bastion.

>[!NOTE]
>Actualmente solo se admiten operaciones de copiado y pegado de texto.
>

   ![Autorización del Portapapeles](./media/bastion-vm-manage/allow.png)

Solo se admiten operaciones de copiado y pegado de texto. En operaciones de copiado y pegado directo, es posible que el explorador le pida acceso al Portapapeles al inicializar la sesión de Bastion. Pulse **Permitir** para que la página web puede acceder al Portapapeles. Si está trabajando desde un equipo Mac, el método abreviado de teclado para pegar es **SHIFT-CTRL-V**.

## <a name="copy-to-a-remote-session"></a><a name="to"></a>Copiado en una sesión remota

Después de conectarse a la máquina virtual mediante [Azure Portal](https://portal.azure.com), complete los siguientes pasos:

1. Copie el texto/contenido del dispositivo local al Portapapeles local.
1. Durante la sesión remota, inicie la paleta de herramientas de acceso al Portapapeles de Bastion seleccionando las dos flechas. Las flechas se encuentran en la parte central izquierda de la sesión.

   ![Captura de pantalla que muestra las flechas de inicio de la paleta de herramientas resaltadas en el lado izquierdo de la ventana.](./media/bastion-vm-manage/left.png)

   ![Captura de pantalla que muestra un Portapapeles con el texto copiado en Bastion.](./media/bastion-vm-manage/clipboard.png)
1. Normalmente, el texto copiado se muestra automáticamente en la paleta de copiado y pegado de Bastion. Si el texto no se encuentra allí, péguelo en el área de texto de la paleta.
1. Cuando el texto esté en el área de texto, podrá pegarlo en la sesión remota.

   ![Captura de pantalla que muestra el botón de copiado y pegado resaltado y una cadena de texto de ejemplo copiada en la sesión remota.](./media/bastion-vm-manage/local.png)

## <a name="copy-from-a-remote-session"></a><a name="from"></a>Copiado desde una sesión remota

Después de conectarse a la máquina virtual mediante [Azure Portal](https://portal.azure.com), complete los siguientes pasos:

1. Copie el texto/contenido de la sesión remota al Portapapeles remoto (con Ctrl-C).

   ![paleta de herramientas](./media/bastion-vm-manage/remote.png)
1. Durante la sesión remota, inicie la paleta de herramientas de acceso al Portapapeles de Bastion seleccionando las dos flechas. Las flechas se encuentran en la parte central izquierda de la sesión.

   ![Portapapeles](./media/bastion-vm-manage/clipboard2.png)
1. Normalmente, el texto copiado se muestra automáticamente en la paleta de copiado y pegado de Bastion. Si el texto no se encuentra allí, péguelo en el área de texto de la paleta.
1. Cuando el texto esté en el área de texto, podrá pegarlo en el dispositivo local.

   ![pegar](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Pasos siguientes

Lea el artículo sobre [preguntas frecuentes de Bastion](bastion-faq.md).
