---
title: 'Tutorial: Inicialización del hardware: Azure FXT Edge Filer'
description: Aprenda a conectarse al nodo de hardware y establecer una contraseña inicial en los nodos de Azure FXT Edge Filer.
author: femila
ms.author: femila
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.openlocfilehash: f78c7e60210d71aa94a20b4ce198a8d9d154a06f
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131033026"
---
# <a name="tutorial-set-hardware-passwords"></a>Tutorial: Establecimiento de las contraseñas del hardware

La primera vez que encienda un nodo Azure FXT Edge Filer debe establecer una contraseña raíz. Los nodos de hardware no se distribuyen con contraseña predeterminada.

Los puertos de red se deshabilitan hasta una vez establecida la contraseña e iniciada la sesión del usuario raíz.

Realice este paso después de instalar y cablear del nodo, pero antes de intentar crear el clúster.

En este tutorial se explica cómo conectar con el nodo de hardware y establecer la contraseña. También se describe cómo agregar una contraseña de configuración del BIOS para ayudar a proteger el nodo.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Conectar un teclado y un monitor al nodo y a encender este último
> * Establecimiento de una contraseña de configuración del BIOS
> * Establecer las contraseñas para el puerto iDRAC y el usuario raíz en este nodo
> * Iniciar sesión como usuario raíz

Repita estos pasos con todos los nodos que vaya a utilizar en el clúster.

Este tutorial se completa en unos 15 minutos.

## <a name="prerequisites"></a>Prerrequisitos

Antes de comenzar este tutorial, realice estos pasos:

* [Instale](install.md) los nodos de Azure FXT Edge Filer en un bastidor y conecte los cables de alimentación y el acceso a la red como se describe en el [tutorial anterior](network-power.md).
* Encuentre un teclado con conexión USB y un monitor con conexión VGA para conectarlos a los nodos del hardware (el puerto serie del nodo está inactivo hasta que se establece la contraseña).

## <a name="connect-a-keyboard-and-monitor-to-the-node"></a>Conexión de un teclado y un monitor al nodo

Conecte un monitor y un teclado físicos al nodo Azure FXT Edge Filer.

* Conecte el monitor al puerto VGA.
* Conecte el teclado a uno de los puertos USB.

En este diagrama de referencia encontrará los puertos en la parte posterior del bastidor.

> [!NOTE]
> El puerto serie permanece inactivo hasta que se establece la contraseña.

![Diagrama de parte posterior de Azure FXT Edge Filer con los puertos serie, VGA y USB indicados](media/fxt-back-serial-vga-usb.png)

Puede usar un interruptor de KVM si desea conectar más de un nodo a los mismos periféricos.

Presione el botón de encendido frontal para encender el nodo.

![Diagrama de la parte frontal de Azure FXT Edge Filer con el botón de encendido redondo indicado cerca de la esquina superior derecha](media/fxt-front-annotated.png)

## <a name="create-a-bios-setup-password"></a>Creación de una contraseña de configuración del BIOS

Una contraseña de configuración del BIOS protege la configuración del BIOS del nodo de cambios accidentales o no autorizados. Esta contraseña no es necesaria para crear un clúster, pero se recomienda como parte de la estrategia de seguridad del clúster.

Para crear una contraseña de configuración del BIOS:

1. Active o reinicie el nodo y presione F2 inmediatamente para abrir la utilidad de configuración del sistema.

1. En la pantalla **System Setup Main Menu** (Menú principal de configuración del sistema), elija **System BIOS** > **System Security** (BIOS del sistema > Seguridad del sistema).

1. Asegúrese de que la configuración **Password Status** (Estado de la contraseña) sea **Unlocked** (Desbloqueada).

1. Use el campo **Setup Password** (Configurar contraseña) para establecer la contraseña. (También puede establecer una contraseña del BIOS del sistema desde esta pantalla si desea utilizar una).

1. Presione ESC para volver a la pantalla **System BIOS** (BIOS del sistema) y, a continuación, vuelva a presionar ESC. Un mensaje le pedirá que guarde los cambios. Si el sistema no se reinicia automáticamente, reinícielo para llegar a la pantalla de inicio normal.<!-- how to exit this mode/do you need to reboot to get to the initial setup screen? -->

## <a name="set-initial-passwords"></a>Establecimiento inicial de las contraseñas

El nodo Azure FXT Edge Filer enviará varios mensajes al monitor durante el arranque. Transcurridos unos instantes, mostrará una pantalla de configuración inicial similar a la siguiente:

```
------------------------------------------------------
        Microsoft FXT node initial setup
------------------------------------------------------
Password Setup
---------------
Enter a password to set the iDRAC and temporary root password.
Minimum password length is 8.

NOTE: This does not set a BIOS setup password. For security,
Microsoft recommends using a BIOS setup password, restricting
physical access to the node, and other measures. Learn more at
https://aka.ms/fxt-security.

Enter new password:

```

La contraseña que escriba se utilizará para dos cosas:

* Es la contraseña raíz temporal del nodo Azure FXT Edge Filer.

  Esta contraseña se cambiará al crear un clúster que use este nodo o al agregar este nodo al clúster. La contraseña de administración del clúster (asociada con el usuario ``admin``) también es la contraseña raíz para todos los nodos del clúster.

* Es la contraseña permanente del puerto iDRAC/IPMI de administración del hardware.

  Debe recordar la contraseña para iniciar sesión con IPMI en el futuro si es necesario solucionar problemas de hardware.

Escriba y confirme la contraseña:

```
Enter new password:**********
Re-enter password:**********
Loading AvereOS......
```

Después de escribir la contraseña, el sistema continuará con el arranque. Cuando haya terminado, aparecerá ``login:``.

## <a name="sign-in-as-root"></a>Iniciar sesión como usuario raíz

Inicie sesión como ``root`` con la contraseña que acaba de establecer.

```
login: root
Password:**********
```

Después de iniciar sesión como raíz, los puertos de red estarán activos y se pondrán en contacto con el servidor DHCP para las direcciones IP.

## <a name="next-steps"></a>Pasos siguientes

El nodo está listo para formar parte de un clúster. Puede usarlo para crear el clúster de Azure FXT Edge Filer, o bien, puede [agregarlo a un clúster existente](add-nodes.md).

> [!div class="nextstepaction"]
> [Creación de un clúster](cluster-create.md)
