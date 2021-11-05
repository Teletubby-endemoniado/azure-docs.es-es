---
title: Preparación para la producción y procedimientos recomendados
description: En este artículo se ofrecen instrucciones sobre cómo configurar e implementar el módulo perimetral de Azure Video Analyzer en entornos de producción.
ms.topic: reference
ms.date: 10/01/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: 1baf85b21c9b6b2792eb93ca2d4c94ef50170395
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2021
ms.locfileid: "131093382"
---
# <a name="production-readiness-and-best-practices"></a>Preparación para la producción y procedimientos recomendados

[!INCLUDE [header](includes/edge-env.md)]

En este artículo se ofrecen instrucciones sobre cómo configurar e implementar el módulo perimetral y el servicio en la nube de Azure Video Analyzer en entornos de producción. También recomendamos que lea el artículo [Preparación para implementar la solución IoT Edge en producción](../../../iot-edge/production-checklist.md) sobre la preparación de la solución IoT Edge.

Debe consultar a al departamento de TI de su organización sobre los aspectos relacionados con la seguridad.

## <a name="creating-the-video-analyzer-account"></a>Creación de la cuenta de Video Analyzer

Al [crear](../create-video-analyzer-account.md) una cuenta de Video Analyzer, se recomienda lo siguiente:

1. El propietario de la suscripción debe crear un grupo de recursos en el que se van a crear todos los recursos necesarios para Video Analyzer.
1. A continuación, el propietario debe concederle los roles [Colaborador](../../../role-based-access-control/built-in-roles.md#contributor) y [Administrador de acceso de usuario](../../../role-based-access-control/built-in-roles.md#user-access-administrator) para ese grupo de recursos.
1. A continuación, puede crear los recursos pertinentes: cuenta de almacenamiento,IoT Hub, identidad administrada asignada por el usuario y cuenta de Video Analyzer en ese grupo de recursos.

## <a name="running-the-module-as-a-local-user"></a>Ejecución del módulo como un usuario local

Al implementar el módulo perimetral de Video Analyzer en un dispositivo IoT Edge, se ejecuta de forma predeterminada con privilegios elevados. Puede comprobarlo mediante los registros del módulo (`sudo iotedge logs {name-of-module}`), que deben mostrar:

```
!! production readiness: user accounts – Warning
       LOCAL_USER_ID and LOCAL_GROUP_ID environment variables are not set. The program will run as root!
       For optimum security, make sure to set LOCAL_USER_ID and LOCAL_GROUP_ID environment variables to a non-root user and group.
```

En las secciones siguientes se explica cómo puede resolver la advertencia anterior.

### <a name="creating-and-using-a-local-user-account"></a>Creación y uso de una cuenta de usuario local

Puede y debe ejecutar el módulo perimetral de Video Analyzer en producción con una cuenta que tenga el menor número de privilegios posible. Por ejemplo, los siguientes comandos muestran cómo se puede crear una cuenta de usuario local en una máquina virtual de Linux:

```
sudo groupadd -g 1010 localedgegroup
sudo useradd --home-dir /home/localedgeuser --uid 1010 --gid 1010 localedgeuser
```

Después, en el manifiesto de implementación, puede establecer las variables de entorno LOCAL_USER_ID y LOCAL_GROUP_ID en ese grupo y usuario no raíz:

```
"avaedge": {
  "version": "1.0",
  …
  "env": {
    "LOCAL_USER_ID": {
       "value": "1010"
    },
    "LOCAL_GROUP_ID": {
     "value": "1010"
    }
  }
},
…
```

### <a name="granting-permissions-to-device-storage"></a>Concesión de permisos para el almacenamiento de dispositivos

El módulo perimetral de Video Analyzer requiere la capacidad de escribir archivos en el sistema de archivos local al:

- Usar una propiedad del módulo gemelo [`applicationDataDirectory`](module-twin-configuration-schema.md), donde debe especificar un directorio en el sistema de archivos local para almacenar los datos de configuración.
- Usar una canalización para grabar vídeo en la nube; en este caso, el módulo requiere el uso de un directorio en el dispositivo perimetral como memoria caché (consulte el artículo [Grabación continua de vídeo](../continuous-video-recording.md) para más información).
- [Grabar en un archivo local](../event-based-video-recording-concept.md), donde se especifica una ruta de acceso de archivo para el vídeo grabado.

Si tiene previsto hacer uso de cualquiera de los anteriores, debe asegurarse de que la cuenta de usuario anterior tiene acceso al directorio pertinente. Supongamos, por ejemplo, `applicationDataDirectory`. Puede crear un directorio en el dispositivo perimetral y vincular el almacenamiento del dispositivo al almacenamiento de módulos.

```
sudo mkdir /var/lib/videoanalyzer
sudo chown -R localedgeuser:localedgegroup /var/lib/videoanalyzer
```

Después, en las opciones de creación para el módulo perimetral del manifiesto de implementación, puede agregar una configuración `binds` que asigne el directorio ("/var/lib/videoanalyzer") anterior a un directorio del módulo (por ejemplo, "/var/lib/videoanalyzer"). Y se usaría el último directorio como el valor de `applicationDataDirectory`.

```
        "modules": {
          "avaedge": {
            "version": "1.1",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/media/video-analyzer:1",
              "createOptions": "{ \"HostConfig\": { \"LogConfig\": { \"Type\": \"\", \"Config\": { \"max-size\": \"10m\", \"max-file\": \"10\" } }, \"Binds\": [ \"/var/media/:/var/media/\", \"/var/lib/videoanalyzer/:/var/lib/videoanalyzer\" ], \"IpcMode\": \"host\", \"ShmSize\": 1536870912 } }"
            },
            "env": {
              "LOCAL_USER_ID": {
                "value": "1010"
              },
              "LOCAL_GROUP_ID": {
                "value": "1010"
              }
            }
          },
          …
        },
        
    …
    
    "avaedge": {
       "properties.desired": {
          "applicationDataDirectory": "/var/lib/videoanalyzer",
          "ProvisioningToken": "{your-token}",
          "diagnosticsEventsOutputName": "diagnostics",
          "operationalEventsOutputName": "operational",
          "logLevel": "information",
          "LogCategories": "Application,Events",
          "allowUnsecuredEndpoints": false,
          "telemetryOptOut": false
    }
}
```

Si observa las canalizaciones de ejemplo de la guía de inicio rápido y los tutoriales, como la [Grabación continua de vídeo](use-continuous-video-recording.md), observará que el directorio de memoria caché multimedia (`localMediaCachePath`) usa un subdirectorio en `applicationDataDirectory`. Este es el enfoque recomendado, ya que la memoria caché contiene datos transitorios.

Observe también que `allowedUnsecuredEndpoints` se establece en `false`, como se recomienda para entornos de producción en lo que usará el cifrado TLS para proteger el tráfico.

### <a name="tips-about-maintaining-your-edge-device"></a>Sugerencias sobre el mantenimiento del dispositivo perimetral

Las sugerencias siguientes no son una lista exhaustiva, pero deberían ayudar con los problemas conocidos más comunes que hemos encontrado.

La máquina virtual Linux que se usa como dispositivo IoT Edge puede dejar de responder si no se administra de forma periódica. Es fundamental mantener limpias las memorias caché, eliminar los paquetes innecesarios y quitar también los contenedores no usados de la máquina virtual. Para hacerlo, recomendamos este conjunto de comandos que puede usar en la máquina virtual perimetral.

- `sudo apt-get clean`

El comando apt-get clean borra el repositorio local de los archivos de paquete recuperados que se dejan en /var/cache. Los directorios que limpia son /var/cache/apt/archives/ y /var/cache/apt/archives/partial/. Los únicos archivos que se conservan en /var/cache/apt/archives son el archivo de bloqueo y el subdirectorio parcial. El comando apt-get clean se usa generalmente para borrar el espacio en disco cuando sea necesario, normalmente como parte del mantenimiento programado regularmente. Para más información, consulte [Limpieza con apt-get](https://www.networkworld.com/article/3453032/cleaning-up-with-apt-get.html).

- `sudo apt-get autoclean`

La opción apt-get autoclean, como apt-get clean, borra el repositorio local de archivos de paquete recuperados, pero solo quita los archivos que ya no se pueden descargar y que no son útiles. Esto ayuda a evitar que la memoria caché crezca demasiado.

- `sudo apt-get autoremove`

La opción de quitar automáticamente quita los paquetes que se instalaron automáticamente porque algún otro paquete los necesitaba pero, con esos otros paquetes quitados, ya no son necesarios.

- `sudo docker image ls`: proporciona una lista de imágenes de Docker en el sistema perimetral

- `sudo docker system prune`

Docker toma un enfoque conservador para limpiar los objetos no usados (a menudo denominados "recolección de elementos no utilizados"), como imágenes, contenedores, volúmenes y redes: normalmente, estos objetos no se quitan a menos que se pida explícitamente a Docker que lo haga. Esto puede hacer que Docker use espacio en disco adicional. Para cada tipo de objeto, Docker proporciona un comando de eliminación. Además, puede usar `docker system prune` para limpiar varios tipos de objetos a la vez. Para más información, consulte [Eliminación de los objetos de Docker no utilizados](https://docs.docker.com/config/pruning/).

- `sudo docker rmi REPOSITORY:TAG`

A medida que las actualizaciones se producen en el módulo Edge, el Docker puede tener versiones anteriores del módulo Edge todavía presentes. En tal caso, se recomienda usar el comando `docker rmi` para quitar imágenes específicas identificadas por la etiqueta de versión de la imagen.

## <a name="next-steps"></a>Pasos siguientes

[Inicio rápido: Introducción a Azure Video Analyzer](get-started-detect-motion-emit-events.md)
