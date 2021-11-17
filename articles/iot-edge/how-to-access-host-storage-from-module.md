---
title: 'Uso del almacenamiento local de dispositivos IoT Edge desde un módulo: Azure IoT Edge | Microsoft Docs'
description: Use variables de entorno y cree opciones para permitir que el módulo acceda al almacenamiento local de dispositivos IoT Edge.
author: kgremban
ms.author: kgremban
ms.date: 08/14/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: fc89cbfd01ef827be277d332ecd770b3fa6d7016
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130249806"
---
# <a name="give-modules-access-to-a-devices-local-storage"></a>Acceso de los módulos al almacenamiento local de un dispositivo

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Además de almacenar datos mediante los servicios de almacenamiento de Azure o en el almacenamiento de contenedor del dispositivo, también puede dedicar almacenamiento en el propio dispositivo IoT Edge para mejorar la confiabilidad, en especial cuando se trabaja sin conexión.

## <a name="link-module-storage-to-device-storage"></a>Vinculación del almacenamiento del módulo al almacenamiento del dispositivo

Para permitir un vínculo entre el almacenamiento del módulo y el almacenamiento del sistema host, cree una variable de entorno para el módulo que apunte a una carpeta de almacenamiento del contenedor. Después, tendrá que utilizar las opciones de creación para enlazar esa carpeta de almacenamiento con una carpeta del equipo host.

Por ejemplo, si quiere habilitar el centro de IoT Edge para almacenar mensajes en el almacenamiento local del dispositivo y recuperarlos más tarde, puede configurar las variables de entorno y las opciones de creación en la sección **Configuración del entorno de ejecución** de Azure Portal.

1. Tanto para el centro de IoT Edge como para el agente de IOT Edge, agregue una variable de entorno denominada **storageFolder** que apunte a un directorio en el módulo.
1. Tanto para el centro de IoT Edge como para el agente de IoT Edge, agregue enlaces para conectar un directorio local de la máquina host a un directorio del módulo. Por ejemplo:

   ![Agregar opciones de creación y variables de entorno para el almacenamiento local](./media/how-to-access-host-storage-from-module/offline-storage.png)

También puede configurar el almacenamiento local directamente en el manifiesto de implementación. Por ejemplo:

```json
"systemModules": {
    "edgeAgent": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-agent:1.1",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath>"]
                }
            }
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        }
    },
    "edgeHub": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-hub:1.1",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath>"],
                    "PortBindings":{"5671/tcp":[{"HostPort":"5671"}],"8883/tcp":[{"HostPort":"8883"}],"443/tcp":[{"HostPort":"443"}]}}}
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        },
        "status": "running",
        "restartPolicy": "always"
    }
}
```

Reemplace `<HostStoragePath>` y `<ModuleStoragePath>` por las rutas de almacenamiento de su host y de su módulo; ambos valores deben ser una ruta de acceso absoluta.

Por ejemplo, en un sistema Linux, `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]` significa que el directorio **/etc/iotedge/storage** del sistema host se asigna al directorio **/iotedge/storage/** del contenedor. En un sistema Windows, por poner otro ejemplo, `"Binds":["C:\\temp:C:\\contemp"]` significa que el directorio **C:\\temp** del sistema host se asigna al directorio **C:\\contemp** del contenedor.

Puede encontrar más detalles sobre las opciones de creación en la [documentación de Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="host-system-permissions"></a>Permisos del sistema host

En los dispositivos Linux, asegúrese de que el perfil de usuario del módulo tenga los permisos de lectura, escritura y ejecución necesarios para el directorio del sistema host. Volviendo al ejemplo anterior de permitir que el centro de IoT Edge almacene mensajes en el almacenamiento local del dispositivo, debe conceder permisos a su perfil de usuario, UID 1000. Hay varias maneras de administrar los permisos de directorio en los sistemas Linux, como usar `chown` para cambiar el propietario del directorio y, luego, `chmod` para cambiar los permisos. Por ejemplo:

```bash
sudo chown 1000 <HostStoragePath>
sudo chmod 700 <HostStoragePath>
```

En dispositivos Windows, también deberá configurar los permisos en el directorio del sistema host. Puede usar PowerShell para establecer permisos:

```powershell
$acl = get-acl <HostStoragePath>
$ace = new-object system.security.AccessControl.FileSystemAccessRule('Authenticated Users','FullControl','Allow')
$acl.AddAccessRule($ace)
$acl | Set-Acl
```

## <a name="encrypted-data-in-module-storage"></a>Datos cifrados en el almacenamiento de módulos

Cuando los módulos invocan la API de carga de trabajo del demonio de IoT Edge para cifrar datos, la clave de cifrado se deriva mediante el id. de módulo y el id. de generación del módulo. Un id. de generación se usa para proteger los secretos si se quita un módulo de la implementación y, posteriormente, se implementa otro módulo con el mismo id. de módulo en el mismo dispositivo. Puede ver el id. de generación de un módulo mediante el comando [az iot hub module-identity show](/cli/azure/iot/hub/module-identity) de la CLI de Azure.

Si quiere compartir archivos entre módulos de varias generaciones, no debe contener ningún secreto o se podrá descifrar.

## <a name="next-steps"></a>Pasos siguientes

Para ver otro ejemplo de acceso a almacenamiento host desde un módulo, consulte [Almacenamiento de datos en el perímetro con Azure Blob Storage en IoT Edge](how-to-store-data-blob.md).
