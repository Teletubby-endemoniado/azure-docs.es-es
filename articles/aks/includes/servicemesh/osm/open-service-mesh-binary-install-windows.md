---
author: phillipgibson
ms.topic: include
ms.date: 03/15/2021
ms.author: pgibson
ms.openlocfilehash: 10759b4d80b39b1fc9a04d513e75d98b7c691281
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2021
ms.locfileid: "123440101"
---
## <a name="download-and-install-the-open-service-mesh-osm-client-binary"></a>Descarga e instalación del binario de cliente de Open Service Mesh (OSM)

En un shell basado en PowerShell en Windows, use `Invoke-WebRequest` para descargar la versión de OSM y, después, realice la extracción con `Expand-Archive`, como se indica a continuación:

```powershell
# Specify the OSM version that will be leveraged throughout these instructions
$OSM_VERSION="v0.9.1"

[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/openservicemesh/osm/releases/download/$OSM_VERSION/osm-$OSM_VERSION-windows-amd64.zip&quot; -OutFile &quot;osm-$OSM_VERSION.zip"
Expand-Archive -Path "osm-$OSM_VERSION.zip" -DestinationPath .
```

El binario de cliente `osm` se ejecuta en la máquina cliente y permite administrar el controlador de OSM en el clúster de AKS. Use los siguientes comandos para instalar el binario de cliente `osm` de OSM en un shell basado en PowerShell en Windows. Estos comandos copian los binarios de cliente `osm` en una carpeta de OSM y, luego, permiten que estén disponibles de inmediato (en el shell actual) y de forma permanente (entre reinicios del shell) mediante `PATH`. No es necesario tener privilegios elevados (administrador) para ejecutar estos comandos y no hace falta reiniciar el shell.

```powershell
# Copy osm.exe to C:\OSM
New-Item -ItemType Directory -Force -Path "C:\OSM"
Move-Item -Path .\windows-amd64\osm.exe -Destination "C:\OSM\"

# Add C:\OSM to PATH.
# Make the new PATH permanently available for the current User
$USER_PATH = [environment]::GetEnvironmentVariable("PATH&quot;, &quot;User&quot;) + &quot;;C:\OSM\"
[environment]::SetEnvironmentVariable("PATH", $USER_PATH, "User")
# Make the new PATH immediately available in the current shell
$env:PATH += ";C:\OSM\"
```
