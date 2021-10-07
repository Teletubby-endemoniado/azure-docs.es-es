---
title: Cambio dinámico del nivel de servicio de un volumen para Azure NetApp Files | Microsoft Docs
description: Describe cómo cambiar dinámicamente el nivel de servicio de un volumen.
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
ms.topic: how-to
ms.date: 05/06/2021
ms.author: b-juche
ms.openlocfilehash: 7c9704549c68dce72c16f0b5386cdb76edb5fe90
ms.sourcegitcommit: f3f2ec7793ebeee19bd9ffc3004725fb33eb4b3f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2021
ms.locfileid: "129407007"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Cambio dinámico del nivel de servicio de un volumen

Puede cambiar el nivel de servicio de un volumen existente al moverlo a otro grupo de capacidad que use el [nivel de servicio](azure-netapp-files-service-levels.md) que quiere para dicho volumen. Este cambio de nivel de servicio local para el volumen no requiere que se migren los datos. Tampoco afecta al acceso al volumen.  

Esta funcionalidad le permite satisfacer sus necesidades de carga de trabajo a petición.  Puede cambiar un volumen existente para que use un nivel de servicio superior para mejorar el rendimiento, o para que use un nivel de servicio inferior para la optimización de costos. Por ejemplo, si actualmente el volumen está en un grupo de capacidad que usa el nivel de servicio *Estándar* y quiere que el volumen use el nivel de servicio *Premium*, puede trasladar el volumen de forma dinámica a un grupo de capacidad que use el nivel de servicio *Premium*.  

El grupo de capacidad al que quiera trasladar el volumen ya debe existir. Además, el grupo puede contener otros volúmenes.  Si quiere trasladar el volumen a un grupo de capacidad nuevo, debe [crear el grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md) antes de la migración.  

## <a name="considerations"></a>Consideraciones

* Después de que el volumen se haya movido a otro grupo de capacidad, ya no tendrá acceso a los registros de actividad del volumen anteriores, ni a las métricas del volumen. El volumen se iniciará con nuevos registros de actividad y métricas en el grupo de capacidad nuevo.

* Si mueve un volumen a un grupo de capacidad de un nivel de servicio superior (por ejemplo, al pasar del nivel de servicio *Estándar* a *Premium* o *Ultra*), deberá esperar al menos siete días para mover ese volumen *de nuevo* a un grupo de capacidad de un nivel de servicio inferior (por ejemplo, para pasar de *Ultra* a *Premium* o *Standard*).  

## <a name="register-the-feature"></a>Registrar la característica

La característica para trasladar un volumen a otro grupo de capacidad está actualmente en versión preliminar. Si usa esta característica por primera vez, debe registrarla primero.

Si tiene varias suscripciones de Azure, asegúrese de que se está registrando para la suscripción deseada mediante el comando ["Set-AzContext"](/powershell/module/az.accounts/set-azcontext). <!-- GitHub #74191 --> 

1. Registre la característica: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

2. Compruebe el estado del registro de la característica: 

    > [!NOTE]
    > **RegistrationState** puede estar en el estado `Registering` hasta 60 minutos antes de cambiar a `Registered`. Espere hasta que el estado sea **Registrado** antes de continuar.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```
También puede usar los comandos de la [CLI de Azure](/cli/azure/feature) `az feature register` y `az feature show` para registrar la característica y mostrar el estado del registro. 
 
## <a name="move-a-volume-to-another-capacity-pool"></a>Traslado de un volumen a otro grupo de capacidad

1.  En la página Volúmenes, haga clic con el botón secundario en el volumen cuyo nivel de servicio quiere cambiar. Seleccione **Cambiar grupo**.

    ![Clic con el botón derecho en el volumen](../media/azure-netapp-files/right-click-volume.png)

2. En la ventana Cambiar grupo, seleccione el grupo de capacidad al que quiere trasladar el volumen. 

    ![Cambiar grupo](../media/azure-netapp-files/change-pool.png)

3.  Haga clic en **OK**.


## <a name="next-steps"></a>Pasos siguientes  

* [Niveles de servicio para Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Creación de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)
* [Solución de problemas al cambiar el grupo de capacidad de un volumen](troubleshoot-capacity-pools.md#issues-when-changing-the-capacity-pool-of-a-volume)
