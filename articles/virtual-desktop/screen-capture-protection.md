---
title: Protección de captura de pantalla de Azure Virtual Desktop
titleSuffix: Azure
description: Procedimiento para configurar la protección de captura de pantalla para Azure Virtual Desktop.
author: gundarev
ms.topic: conceptual
ms.date: 08/30/2021
ms.author: denisgun
ms.service: virtual-desktop
ms.openlocfilehash: f9571f89bdb3acab7f042c63f2c8bba05044a758
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2021
ms.locfileid: "129992256"
---
# <a name="screen-capture-protection"></a>Protección de la captura de pantalla

La característica de protección de capturas de pantalla evita que se capture información confidencial en los puntos de conexión de cliente. Al habilitar esta característica, el contenido remoto se bloqueará o se ocultará automáticamente en las capturas de pantalla y los recursos compartidos de pantalla. Además, el cliente Escritorio remoto ocultará el contenido del software malintencionado que pueda capturar la pantalla.

## <a name="prerequisites"></a>Requisitos previos

La característica de protección de captura de pantalla se configura en el nivel de host de sesión y se aplica en el cliente. Solo los clientes que admiten esta característica se pueden conectar a la sesión remota.
Los siguientes clientes admiten actualmente la protección de captura de pantalla:

* El cliente Escritorio de Windows solo admite la protección de captura de pantalla para escritorios completos.
* El cliente macOS versión 10.7.0 o anterior admite la protección de captura de pantalla para RemoteApp y escritorios completos.

Imagine que el usuario intenta utilizar un cliente no compatible para conectarse al host de sesión protegido. En ese caso, se producirá un error 0x1151 en la conexión.

## <a name="configure-screen-capture-protection"></a>Configuración de la protección de captura de pantalla

1. Para configurar la protección de captura de pantalla, debe instalar plantillas administrativas que agreguen reglas y configuraciones para Azure Virtual Desktop.
2. Descargue el [archivo de plantillas de directiva de Azure Virtual Desktop](https://aka.ms/avdgpo) (AVDGPTemplate.cab) y extraiga el contenido de los archivos cab y ZIP.
3. Copie el archivo *terminalserver-avd.admx* en la carpeta *%windir%\PolicyDefinitions*.
4. Copie el archivo *en-us\terminalserver-avd.adml* en la carpeta *%windir%\PolicyDefinitions\en-us*.
5. Para confirmar que los archivos se han copiado correctamente, abra el Editor de directivas de grupo local y vaya a **Configuración del equipo** -> **Plantillas administrativas** -> **Componentes de Windows** -> **Servicios de escritorio remoto** -> **Host de sesión de escritorio remoto** -> **Azure Virtual Desktop**.
6. Debería ver una o varias directivas de Azure Virtual Desktop, como se muestra a continuación.

   :::image type="content" source="media/azure-virtual-desktop-gpo.png" alt-text="Captura de pantalla del editor de directivas de grupo" lightbox="media/azure-virtual-desktop-gpo.png":::

   > [!TIP]
   > También puede instalar plantillas administrativas en la directiva de grupo Almacén central en el dominio de Active Directory.
   > Para obtener más información sobre el almacén central para plantillas administrativas de directiva de grupo, vea [Procedimiento para crear y administrar el almacén central de plantillas administrativas de la directiva de grupo en Windows](/troubleshoot/windows-client/group-policy/create-and-manage-central-store).

7. Abra la directiva **"Habilitar protección de captura de pantalla"** y establézcala en **"Habilitado"** .

## <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos

* Esta característica evita la captura de la ventana Escritorio remoto por medio de un conjunto específico de características y API públicas del sistema operativo. Pero no hay ninguna garantía de que esta característica proteja de forma estricta el contenido, por ejemplo, cuando alguien toma fotografías de la pantalla.
* Los clientes deben usar la característica junto con la deshabilitación del Portapapeles, la unidad y el redireccionamiento de impresoras. Deshabilitar el redireccionamiento le ayudará a evitar que el usuario copie el contenido de la pantalla capturada de la sesión remota.
* Cuando la característica está habilitada, los usuarios no pueden compartir la ventana Escritorio remoto mediante software de colaboración local, como Microsoft Teams. Si se usa Microsoft Teams, la aplicación de Teams local y la que se ejecuta con optimizaciones multimedia no pueden compartir el contenido protegido.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre los procedimientos recomendados de seguridad de Azure Virtual Desktop, vea [Procedimientos recomendados de seguridad de Azure Virtual Desktop](security-guide.md).
