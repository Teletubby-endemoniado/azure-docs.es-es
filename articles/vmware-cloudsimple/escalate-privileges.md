---
title: 'Azure VMware Solution by CloudSimple: escalado de privilegios de CloudSimple'
description: Describe cómo escalar los permisos de CloudSimple para realizar funciones administrativas en la instancia de vCenter de la nube privada.
author: suzizuber
ms.author: v-szuber
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 36a8929b679f4c5aed1d7067dce855bef304986b
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2021
ms.locfileid: "132322253"
---
# <a name="escalate-cloudsimple-privileges-to-perform-administrative-functions-in-private-cloud-vcenter"></a>Escalado de privilegios de CloudSimple para realizar funciones administrativas en la instancia de vCenter de la nube privada

El enfoque de privilegios de CloudSimple está diseñado para proporcionar a los usuarios de vCenter los privilegios necesarios para realizar operaciones normales. En algunos casos, un usuario puede requerir privilegios adicionales para realizar una tarea determinada.  Puede escalar los privilegios de un usuario de inicio de sesión único de vCenter durante un período limitado.

Entre los motivos para escalar privilegios se pueden citar los siguientes:

* Configuración de orígenes de identidad
* Administración de usuarios
* Eliminación de un grupo de puertos distribuido
* Instalación de soluciones de vCenter (como aplicaciones de copia de seguridad)
* Creación de cuentas de servicio

> [!WARNING]
> Las acciones realizadas durante el estado con privilegios escalados pueden afectar negativamente a su sistema y pueden hacer que el sistema deje de estar disponible. Realice solo las acciones necesarias durante el período de escalado.

En el portal de CloudSimple, [escale los privilegios](escalate-private-cloud-privileges.md) del usuario local CloudOwner en el inicio de sesión único de vCenter.  Puede escalar los privilegios del usuario remoto solo si se configura un proveedor de identidades adicional en vCenter.  El escalado de privilegios implica agregar el usuario seleccionado al grupo de administradores integrado de vSphere.  Solo un usuario puede tener privilegios elevados.  Si necesita escalar los privilegios de otro usuario, primero debe anular el escalado de los privilegios de los usuarios actuales.

Los usuarios de otros orígenes de identidades se deben agregar como miembros del grupo CloudOwner.

> [!CAUTION]
> Los nuevos usuarios solo se deben agregar a *Cloud-Owner-Group*, *Cloud-Global-Cluster-Admin-Group*, *Cloud-Global-Storage-Admin-Group*, *Cloud-Global-Network-Admin-Group* o *Cloud-Global-VM-Admin-Group*.  Los usuarios agregados al grupo *Administradores* se quitarán automáticamente.  Solo se deben agregar cuentas de servicio al grupo *Administradores* y estas cuentas no se deben usar para iniciar sesión en la interfaz de usuario web de vSphere.

Durante el período de escalado, CloudSimple usa la supervisión automatizada con notificaciones de alerta asociadas para identificar los cambios involuntarios en el entorno.
