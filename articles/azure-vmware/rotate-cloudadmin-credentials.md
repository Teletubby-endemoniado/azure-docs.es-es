---
title: Rotación de las credenciales de administrador de la nube en Azure VMware Solution
description: Aprenda a rotar las credenciales de vCenter Server para la nube privada de Azure VMware Solution.
ms.topic: how-to
ms.date: 09/10/2021
ms.openlocfilehash: f8ab35888d2d298d1dcb6acd2abd6d659b782fd7
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131461783"
---
# <a name="rotate-the-cloudadmin-credentials-for-azure-vmware-solution"></a>Rotación de las credenciales de administrador de la nube en Azure VMware Solution

>[!IMPORTANT]
>Actualmente, no se admite la rotación de credenciales de *administrador* de NSX-T Manager.  Para rotar la contraseña NSX-T Manager, envíe una [solicitud de soporte técnico](https://rc.portal.azure.com/#create/Microsoft.Support). Este proceso puede afectar a la ejecución de servicios HCX.

En este artículo, rotará las credenciales de cloudadmin (credenciales de vCenter Server *CloudAdmin*) para la nube privada de Azure VMware Solution.  Aunque la contraseña de esta cuenta no expira, puede generar una nueva en cualquier momento.

>[!CAUTION]
>Si usa sus credenciales de cloudadmin para conectar los servicios a vCenter en la nube privada, esas conexiones dejarán de funcionar una vez que rote la contraseña. Esas conexiones también bloquearán la cuenta cloudadmin a menos que detenga esos servicios antes de rotar la contraseña.

## <a name="prerequisites"></a>Requisitos previos

Tenga en cuenta y determine qué servicios se conectan a vCenter como *cloudadmin@vsphere.local* antes de rotar la contraseña. Estos servicios pueden incluir servicios de VMware como HCX, vRealize Orchestrator, vRealize Operations Manager, VMware Horizon u otras herramientas de terceros que se usan para la supervisión o el aprovisionamiento. 

Una manera de determinar qué servicios se autentican en vCenter con el usuario cloudadmin es inspeccionar los eventos de vSphere mediante el cliente de vSphere para la nube privada. Después de identificar estos servicios y antes de rotar la contraseña, debe detener estos servicios. De lo contrario, los servicios no funcionarán después de rotar la contraseña. También habrá bloqueos temporales en la cuenta de vCenter CloudAdmin, ya que estos servicios intentan autenticarse continuamente mediante una versión almacenada en caché de las credenciales antiguas. 

En lugar de usar el usuario cloudadmin para conectar servicios a vCenter, se recomiendan cuentas individuales para cada servicio. Para obtener más información sobre cómo configurar cuentas independientes para los servicios conectados, vea [Conceptos de acceso e identidad](./concepts-identity.md).

## <a name="reset-your-vcenter-credentials"></a>Restablecimiento de las credenciales de vCenter

### <a name="portal"></a>[Portal](#tab/azure-portal)
 
1. En la nube privada de Azure VMware Solution, seleccione **Identidad**.

1. Haga clic en **Generar nueva contraseña**.

   :::image type="content" source="media/rotate-cloudadmin-credentials/reset-vcenter-credentials-1.png" alt-text="Captura de pantalla que muestra las credenciales de vCenter y una manera de copiarlas o generar una contraseña nueva." lightbox="media/rotate-cloudadmin-credentials/reset-vcenter-credentials-1.png":::

1. Active la casilla de confirmación y, a continuación, seleccione **Generar contraseña**.


### <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para empezar a usar la CLI de Azure:

[!INCLUDE [azure-cli-prepare-your-environment-no-header](../../includes/azure-cli-prepare-your-environment-no-header.md)]

1. En la nube privada de Azure VMware Solution, abra una sesión de Azure Cloud Shell.

2. Actualice las credenciales de vCenter *CloudAdmin*.  No olvide reemplazar **{SubscriptionID},** **{ResourceGroup}** y **{PrivateCloudName}** por la información de su nube privada. 

   ```azurecli-interactive
   az resource invoke-action --action rotateVcenterPassword --ids "/subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroup}/providers/Microsoft.AVS/privateClouds/{PrivateCloudName}" --api-version "2020-07-17-preview"
   ```

---




 
## <a name="update-hcx-connector"></a>Actualización del conector HCX 

1. Vaya al conector HCX local en https://{IP del dispositivo del conector HCX}:443 e inicie sesión con las nuevas credenciales.

   Asegúrese de usar el puerto **443**. 

2. En el panel de VMware HCX, seleccione **Site Pairing** (Emparejamiento de sitios).
    
   :::image type="content" source="media/tutorial-vmware-hcx/site-pairing-complete.png" alt-text="Captura de pantalla del panel de VMware HCX con el emparejamiento de sitios resaltado.":::
 
3. Seleccione la conexión correcta en Azure VMware Solution y seleccione **Edit Connection**.
 
4. Proporcione las nuevas credenciales de vCenter y seleccione **Editar**, que guarda las credenciales. La operación de guardar debe mostrarse como correcta.


## <a name="next-steps"></a>Pasos siguientes

Ahora que ha examinado el restablecimiento de las credenciales de vCenter para Azure VMware Solution, puede que quiera obtener información sobre lo siguiente:

- [Integración de servicios nativos de Azure en Azure VMware Solution](integrate-azure-native-services.md)
- [Implementación de la recuperación ante desastres para las cargas de trabajo de Azure VMware Solution con VMware HCX](deploy-disaster-recovery-using-vmware-hcx.md)
