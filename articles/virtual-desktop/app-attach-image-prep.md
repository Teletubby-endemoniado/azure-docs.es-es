---
title: 'Preparación de imágenes de conexión de aplicaciones MSIX para Azure Virtual Desktop: Azure'
description: Creación de una imagen de conexión de aplicaciones MSIX para un grupo de hosts de Azure Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 04/13/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: e804168d56092b6e5451aa26c7a2106b96672865
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2021
ms.locfileid: "128587354"
---
# <a name="prepare-an-msix-image-for-azure-virtual-desktop"></a>Preparación de una imagen de MSIX para Azure Virtual Desktop

La conexión de aplicaciones MSIX es una solución de aplicaciones en capas que le permite conectar dinámicamente las aplicaciones de un paquete MSIX a una sesión de usuario. El sistema de paquetes MSIX separa las aplicaciones del sistema operativo, lo que facilita la creación de imágenes para las máquinas virtuales. Los paquetes MSIX también proporcionan un mayor control sobre las aplicaciones a las que los usuarios pueden acceder en sus máquinas virtuales. Incluso puede separar las aplicaciones de la imagen maestra y proporcionarlas a los usuarios posteriormente.

Las instrucciones sobre cómo convertir un instalador de escritorio (como MSI, EXE, ClickOnce, App-V o Script) a MSIX están disponibles en [Creación de un paquete MSIX desde cualquier instalador de escritorio (MSI, EXE, ClickOnce o App-V)](/windows/msix/packaging-tool/create-app-package).

## <a name="create-a-vhd-or-vhdx-package-for-msix"></a>Creación de un paquete VHD o VHDX para MSIX

Los paquetes MSIX deben estar en un formato VHD o VHDX para que funcionen correctamente. Esto significa que, para empezar, debe crear un paquete VHD o VHDX.

>[!NOTE]
>Si aún no lo ha hecho, asegúrese de habilitar Hyper-V siguiendo las instrucciones de [Instalar Hyper-V en Windows 10](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

Para crear un paquete VHD o VHDX para MSIX:

1. Primero, abra PowerShell.
2. A continuación, ejecute el cmdlet siguiente para crear un VHD:

    ```powershell
    New-VHD -SizeBytes <size>MB -Path c:\temp\<name>.vhd -Dynamic -Confirm:$false
    ```

    >[!NOTE]
    > Asegúrese de que el disco duro virtual es lo suficientemente grande para contener el paquete MSIX expandido.

3. Ejecute el siguiente cmdlet para montar el disco duro virtual que acaba de crear:

    ```powershell
    $vhdObject = Mount-VHD c:\temp\<name>.vhd -Passthru
    ```

4. Después, ejecute este cmdlet para inicializar el disco duro virtual montado:

    ```powershell
    $disk = Initialize-Disk -Passthru -Number $vhdObject.Number
    ```

5. Luego, ejecute este cmdlet para crear una nueva partición del disco duro virtual inicializado:

    ```powershell
    $partition = New-Partition -AssignDriveLetter -UseMaximumSize -DiskNumber $disk.Number
    ```

6. Ejecute este cmdlet para dar formato a la partición:

    ```powershell
    Format-Volume -FileSystem NTFS -Confirm:$false -DriveLetter $partition.DriveLetter -Force
    ```

7. Finalmente, cree una carpeta principal en el disco duro virtual montado. Este paso es necesario porque el paquete MSIX debe tener una carpeta principal para que funcione correctamente. No importa cuál sea el nombre de la carpeta principal, siempre y cuando esta exista.

## <a name="expand-msix"></a>Expansión de MSIX

Después de eso, deberá descomprimir los archivos de la imagen de MSIX en el disco duro virtual para expandirla.

Para expandir la imagen de MSIX:

1. [Descargue la herramienta msixmgr](https://aka.ms/msixmgr) y guarde la carpeta ZIP en una carpeta dentro de una VM del host de sesión.

2. Descomprima la carpeta ZIP de la herramienta msixmgr.

3. Ponga el paquete de MSIX de origen en la misma carpeta donde descomprimió la herramienta msixmgr.

4. Abra un símbolo del sistema como administrador y vaya a la carpeta en la que descargó y descomprimió la herramienta msixmgr.

5. Ejecute el cmdlet siguiente para desempaquetar MSIX en el disco duro virtual que creó en la sección anterior.

    ```powershell
    msixmgr.exe -Unpack -packagePath <package>.msix -destination "f:\<name of folder you created earlier>" -applyacls
    ```

    El mensaje siguiente debería aparecer una vez que termine el desempaquetado:

    > Listas de control de acceso desempaquetadas y aplicadas correctamente para el paquete: \<package name\>.msix

    >[!NOTE]
    > Si usa paquetes de Microsoft Store para Empresas o Educación dentro de la red o en dispositivos que no están conectados a Internet, deberá descargar e instalar las licencias de paquete desde Store para ejecutar las aplicaciones. Para obtener las licencias, consulte [Uso de paquetes sin conexión](app-attach.md#use-packages-offline).

6. Vaya al disco duro virtual montado y abra la carpeta de la aplicación para asegurarse de que el contenido del paquete está ahí.

7. Desmonte el disco duro virtual.

## <a name="upload-msix-image-to-share"></a>Carga de la imagen de MSIX para compartir

Después de crear el paquete MSIX, deberá cargar el archivo VHD, VHDX o CIM resultante en un recurso compartido al que puedan acceder las máquinas virtuales de los usuarios.

## <a name="next-steps"></a>Pasos siguientes

Pregunte a la comunidad sobre esta característica en el espacio de [TechCommunity sobre Azure Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop).

También puede dejar comentarios sobre Azure Virtual Desktop en el [Centro de opiniones de Azure Virtual Desktop](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).

Estos son algunos otros artículos que pueden resultarle útiles:

- [Glosario sobre la conexión de aplicaciones en formato MSIX](app-attach-glossary.md)
- [Preguntas frecuentes sobre la asociación de aplicaciones en formato MSIX](app-attach-faq.yml)
