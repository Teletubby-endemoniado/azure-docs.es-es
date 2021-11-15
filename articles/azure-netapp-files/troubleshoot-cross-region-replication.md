---
title: Solución de errores de replicación entre regiones de Azure NetApp Files | Microsoft Docs
description: Aquí se describen los mensajes de error y las resoluciones que pueden ayudar a solucionar problemas de replicación entre regiones de Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 03/10/2021
ms.author: b-juche
ms.openlocfilehash: bae0ed7fc15843c50af9dca860367d2074af33ec
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130256280"
---
# <a name="troubleshoot-cross-region-replication-errors"></a>Solución de errores de replicación entre regiones

En este artículo se describen los mensajes de error y las resoluciones que pueden ayudar a solucionar problemas de replicación entre regiones de Azure NetApp Files. 

## <a name="errors-creating-replication"></a>Errores al crear la replicación  

|     Mensaje de error    |     Solución    |
|-|-|
|     `Volume {0} cannot   be used as source because it is already in replication`    |     No se puede crear una replicación con un volumen de origen que ya esté en una relación de replicación de datos.    |
|     `Peered region   '{0}' is not accepted`    |     Se está intentando crear una replicación entre regiones no emparejadas.    |
|     `RemoteVolumeResource   '{0}' of wrong type '{1}'`    |     Compruebe que el identificador de recurso remoto es un identificador de recurso de volumen.    |

## <a name="errors-authorizing-volume"></a>Errores al autorizar el volumen  

|     Mensaje de error    |     Solución    |
|-|-|
|     `Missing value   for 'AuthorizeSourceReplication'`    |     Falta `RemoteResourceID` en la solicitud de la interfaz de usuario o la API o no es válido (mensaje de error de corrección).    |
|     `Missing value   for 'RemoteVolumeResourceId'`    |     Falta `RemoteResourceID` en la solicitud de la interfaz de usuario o la API o no es válido.    |
|     `Data   Protection volume not found for RemoteVolumeResourceId: {remoteResourceId}`    |     Compruebe si `RemoteResourceID` es correcto o existe para el usuario.    |
|     `Remote volume   '{0}' is not configured for replication`    |     El volumen de destino no es un volumen de protección de datos.    |
|     `Remote volume   '{0}' does not have source volume '{1}' as RemoteVolumeResourceId`    |     El volumen de protección de datos no tiene este volumen de origen en su identificador de recurso remoto (se ha especificado un identificador de origen incorrecto).    |
|     `The   destination volume replication creation failed (message: {0})`    |     Este error indica un error del servidor. póngase en contacto con el servicio de soporte técnico.    |

## <a name="errors-deleting-replication"></a>Errores al eliminar la replicación

|     Mensaje de error    |     Solución    |
|-|-|
|     `Replication   cannot be deleted, mirror state needs to be in status: Broken before deleting`    |     Compruebe que la replicación se ha interrumpido o no se ha inicializado y está inactiva (error de inicialización).    |
|     `Cannot delete   source replication`    |     No se permite eliminar la replicación desde el origen. Asegúrese de que está eliminando la replicación desde el destino.    |

## <a name="errors-deleting-volume"></a>Errores al eliminar el volumen

|     Mensaje de error    |     Solución    |
|-|-|
| `Volume is a member of an active volume replication relationship`  |  Elimine la replicación antes de eliminar el volumen. Consulte [Eliminación de replicaciones](cross-region-replication-delete.md). Esta operación requiere que se interrumpa el emparejamiento antes de eliminar la replicación del volumen. |
| `Volume with replication cannot be deleted`  |  Elimine la replicación antes de eliminar el volumen. Consulte [Eliminación de replicaciones](cross-region-replication-delete.md). Esta operación requiere que se interrumpa el emparejamiento antes de eliminar la replicación del volumen. 

## <a name="errors-resyncing-volume"></a>Errores al volver a sincronizar el volumen

|     Mensaje de error    |     Solución    |
|-|-|
|     `Volume Replication is in invalid status: (Mirrored|Uninitialized) for operation: 'ResyncReplication'`     |     Compruebe que la replicación del volumen está en estado "interrumpido".    |

## <a name="errors-deleting-snapshot"></a>Errores al eliminar instantánea 

|     Mensaje de error    |     Solución    |
|-|-|
|     `Snapshot   cannot be deleted, parent volume is a Data Protection volume with a   replication object`    |     Compruebe que ha interrumpido la replicación del volumen si quiere eliminar esta instantánea.    |
|     `Cannot delete   volume replication generated snapshot`    |     No se permite la eliminación de instantáneas de línea de base de replicación.    |

## <a name="errors-resizing-volumes"></a>Errores al cambiar el tamaño de los volúmenes

|     Mensaje de error    |     Solución    |
|-|-|
|   Al intentar cambiar el tamaño de un volumen de origen se produce el error `"PoolSizeTooSmall","message":"Pool size too small for total volume size."`  |  Asegúrese de que dispone de suficiente capacidad de aumento en los grupos de capacidad para los volúmenes de origen y de destino de la replicación entre regiones. Al cambiar el tamaño del volumen de origen, se cambia automáticamente el tamaño del volumen de destino. Sin embargo, si el grupo de capacidad que hospeda el volumen de destino no tiene suficiente capacidad de aumento, se producirá un error al cambiar de tamaño los volúmenes de origen y de destino. Consulte [Cambio de tamaño de un volumen de destino de replicación entre regiones](azure-netapp-files-resize-capacity-pools-or-volumes.md#resize-a-cross-region-replication-destination-volume)   |

## <a name="next-steps"></a>Pasos siguientes  

* [Replicación entre regiones](cross-region-replication-introduction.md)
* [Requisitos y consideraciones del uso de la replicación entre regiones](cross-region-replication-requirements-considerations.md)
* [Creación de replicación de volumen](cross-region-replication-create-peering.md)
* [Visualización del estado de mantenimiento de la relación de la replicación](cross-region-replication-display-health-status.md)
* [Administración de la recuperación ante desastres](cross-region-replication-manage-disaster-recovery.md)
* [Cambio de tamaño de un volumen de destino de replicación entre regiones](azure-netapp-files-resize-capacity-pools-or-volumes.md#resize-a-cross-region-replication-destination-volume)
* [Solución de problemas de la replicación entre regiones](troubleshoot-cross-region-replication.md)
