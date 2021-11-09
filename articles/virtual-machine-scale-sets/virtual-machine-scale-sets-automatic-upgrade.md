---
title: Actualizaciones automáticas de imágenes del SO con conjuntos de escalado de máquinas virtuales de Azure
description: Aprenda a actualizar automáticamente la imagen del sistema operativo en instancias de máquina virtual de un conjunto de escalado
author: mayanknayar
ms.author: manayar
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: automatic-os-upgrade
ms.date: 07/29/2021
ms.reviewer: jushiman
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: f4b5c58eb8811db9042d92416c4c37fa30234149
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131432954"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Actualización automática de imágenes del sistema operativo en un conjunto de escalado de máquinas virtuales de Azure

**Se aplica a:** :heavy_check_mark: Máquinas virtuales Linux :heavy_check_mark: Máquinas virtuales Windows :heavy_check_mark: Conjuntos de escalado uniformes

La habilitación de las actualizaciones automáticas de imágenes del sistema operativo en el conjunto de escalado facilita la administración de actualizaciones al actualizarse de forma segura y automática el disco del sistema operativo de todas las instancias del conjunto de escalado.

La actualización automática del sistema operativo tiene las siguientes características:

- Una vez configurada, la imagen del sistema operativo más reciente publicada por los editores de la imagen se aplica automáticamente al conjunto de escalado sin la intervención del usuario.
- Actualiza lotes de instancias de forma gradual cada vez que el editor publica una nueva imagen.
- Se integra con sondeos de estado de la aplicación y la [extensión Estado de la aplicación](virtual-machine-scale-sets-health-extension.md).
- Funciona en todos los tamaños de máquina virtual y en imágenes de Windows y Linux, incluidas imágenes personalizadas mediante [Azure Compute Gallery](../virtual-machines/shared-image-galleries.md).
- Puede rechazar las actualizaciones automáticas en cualquier momento (las actualizaciones de sistema operativo también se pueden iniciar manualmente).
- El disco del sistema operativo de una máquina virtual se reemplaza por el nuevo disco de sistema operativo creado con la versión más reciente de la imagen. Las extensiones configuradas y los scripts de datos personalizados se ejecutan, mientras se conservan los discos de datos persistentes.
- Se admite la [secuenciación de extensiones](virtual-machine-scale-sets-extension-sequencing.md).
- Se puede habilitar en un conjunto de escalado de cualquier tamaño.

## <a name="how-does-automatic-os-image-upgrade-work"></a>¿Cómo funciona la actualización automática de imágenes del sistema operativo?

En una actualización se reemplaza el disco del sistema operativo de una máquina virtual por otro nuevo creado con la versión más reciente de la imagen. Las extensiones configuradas y los scripts de datos personalizados se ejecutan en el disco del sistema operativo, mientras que los discos de datos se conservan. Para minimizar el tiempo de inactividad de las aplicaciones, las actualizaciones se realizan por lotes, aunque nunca se actualiza más del 20 % del conjunto de escalado a la vez.

Puede integrar un sondeo de aplicación de Azure Load Balancer o la [extensión Estado de la aplicación](virtual-machine-scale-sets-health-extension.md) para realizar un seguimiento del estado de la aplicación después de una actualización. Se recomienda incorporar un latido de aplicación para comprobar si la actualización se ha realizado correctamente.

### <a name="availability-first-updates"></a>Actualizaciones por orden de disponibilidad
El modelo de orden de disponibilidad para actualizaciones orquestadas de la plataforma (descrito a continuación) garantiza que se respeten las configuraciones de disponibilidad de Azure en varios niveles.

**Entre regiones:**
- una actualización se moverá en Azure globalmente por fases para evitar errores de implementación en esta plataforma.
- Una "fase" puede tener una o más regiones, y una actualización solo cambia de fase si las máquinas virtuales válidas de la fase anterior se actualizan correctamente.
- Las regiones emparejadas geográficamente no se actualizan simultáneamente y no pueden estar en la misma fase regional.
- El éxito de una actualización se mide realizando un seguimiento del estado de la máquina virtual después de la actualización.

**Dentro de una región:**
- Las máquinas virtuales de diferentes zonas de disponibilidad no se actualizan simultáneamente con la misma actualización.

**Dentro de un "conjunto":**
- Las máquinas virtuales de un conjunto de escalado común no se actualizan simultáneamente.  
- Las máquinas virtuales de un conjunto de escalado de máquinas virtuales común se agrupan en lotes y se actualizan en los límites del dominio de actualización, tal y como se describe a continuación.

Se sigue el proceso de las actualizaciones orquestadas de la plataforma para implementar las actualizaciones de imágenes de plataforma de sistema operativo admitidas cada mes. En el caso de las imágenes personalizadas mediante Azure Compute Gallery, una actualización de imagen solo se inicia en una determinada región de Azure cuando la nueva imagen se publica y se [replica](../virtual-machines/shared-image-galleries.md#replication) en la región de ese conjunto de escalado.

### <a name="upgrading-vms-in-a-scale-set"></a>Actualización de máquinas virtuales en un conjunto de escalado

La región de un conjunto de escalado pasa a ser apta para obtener actualizaciones de imágenes mediante el proceso de orden de disponibilidad para imágenes de plataforma o replicando nuevas versiones de imágenes personalizadas para Share Image Gallery. Posteriormente, la actualización de la imagen se aplica por lotes a un conjunto de escalado individual, como se muestra a continuación:
1. Antes de comenzar el proceso de actualización, el orquestador se asegurará de que no haya más del 20 % de las instancias de todo el conjunto de escalado en mal estado.
2. El orquestador de actualización identifica el lote de instancias de máquina virtual para actualizar, donde un lote puede tener como máximo el 20 % del recuento total de instancias (sujeto a un tamaño de lote mínimo de una máquina virtual). No hay ningún requisito de tamaño mínimo de conjunto de escalado. Los conjuntos de escalado con cinco o menos instancias tendrán una máquina virtual por lote de actualización (tamaño mínimo de lote).
3. El disco del sistema operativo de cada máquina virtual en el lote de actualización seleccionado se sustituye por un nuevo disco creado a partir de la imagen más reciente. Todas las extensiones y configuraciones especificadas en el modelo de conjunto de escalado se aplican a la instancia actualizada.
4. En conjuntos de escalado que tienen configurados sondeos de estado de la aplicación o la extensión Estado de la aplicación, la actualización espera hasta 5 minutos a que la instancia tenga un estado correcto antes de pasar a actualizar el siguiente lote. Si una instancia no recupera su estado en 5 minutos después de la actualización, se restaura su disco del sistema operativo anterior de forma predeterminada.
5. El orquestador de actualización también realiza un seguimiento del porcentaje de instancias que tienen un estado incorrecto después de una actualización. La actualización se detendrá si más del 20 % de las instancias actualizadas pasan a tener un estado incorrecto durante el proceso de actualización.
6. El proceso anterior continúa hasta que se han actualizado todas las instancias del conjunto de escalado.

El orquestador de actualización del sistema operativo del conjunto de escalado comprueba el mantenimiento global del conjunto de escalado antes de actualizar cada lote. Al actualizar un lote, podría haber otras actividades de mantenimiento simultáneas planeadas o sin planear que podrían afectar al mantenimiento de las instancias del conjunto de escalado. En tales casos, si más del 20 % de las instancias del conjunto de escalado tienen un estado incorrecto, la actualización del conjunto de escalado se detiene al final del lote actual.

> [!NOTE]
>La actualización automática del sistema operativo no actualiza la SKU de la imagen de referencia en el conjunto de escalado. Para cambiar la SKU (por ejemplo de Ubuntu 16.04-LTS a 18.04-LTS), debe actualizar el [modelo de conjunto de escalado](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model) directamente con la SKU de imagen deseada. No se puede cambiar la oferta y el publicador de imágenes para un conjunto de escalado existente.  

## <a name="supported-os-images"></a>Imágenes de sistema operativo compatibles
Actualmente se admiten solo determinadas imágenes de plataforma del sistema operativo. [Se admiten](virtual-machine-scale-sets-automatic-upgrade.md#automatic-os-image-upgrade-for-custom-images) imágenes personalizadas si el conjunto de escalado las usa por medio de [Azure Compute Gallery](../virtual-machines/shared-image-galleries.md).

Las siguientes SKU de plataforma se admiten actualmente (y periódicamente se agregan más):

| Publicador               | Sistema operativo      |  SKU               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04-LTS          |
| OpenLogic               | CentOS        | 7.5                |
| Microsoft Windows Server  | Windows Server | Centro de datos de 2012-R2 |
| Microsoft Windows Server  | Windows Server | 2016-Datacenter    |
| Microsoft Windows Server  | Windows Server | 2016-Datacenter-smalldisk |
| Microsoft Windows Server  | Windows Server | 2016-Datacenter-with-Containers |
| Microsoft Windows Server  | Windows Server | 2019-Datacenter |
| Microsoft Windows Server  | Windows Server | 2019-Datacenter-smalldisk |
| Microsoft Windows Server  | Windows Server | 2019-Datacenter-with-Containers |
| Microsoft Windows Server  | Windows Server | 2019-Datacenter-Core |
| Microsoft Windows Server  | Windows Server | 2019-Datacenter-Core-with-Containers |
| Microsoft Windows Server  | Windows Server | 2019-Datacenter-gensecond |


## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Requisitos para configurar la actualización automática de imágenes del sistema operativo

- La propiedad *versión* de la imagen debe establecerse en *más reciente*.
- Use sondeos de estado de aplicación o la [extensión Estado de la aplicación](virtual-machine-scale-sets-health-extension.md) para conjuntos de escalado que no sean de Service Fabric, o bien para conjuntos de escalado de Service Fabric con durabilidad Bronze y tipos de nodo que solo sean sin estado.
- Use la versión 2018-10-01 o una posterior de Compute API.
- Asegúrese de que los recursos externos especificados en el modelo del conjunto de escalado están disponibles y actualizados. Por ejemplo, el URI de SAS para arrancar la carga en las propiedades de extensión de máquina virtual, la carga de la cuenta de almacenamiento, la referencia a los secretos del modelo, etc.
- Para los conjuntos de escalado de máquinas virtuales Windows, a partir de la versión 2019-03-01 de Compute API, la propiedad *virtualMachineProfile.osProfile.windowsConfiguration.enableAutomaticUpdates* debe establecerse en *false* en la definición del modelo de conjunto de escalado. La propiedad *enableAutomaticUpdates* habilita la aplicación de revisiones en la máquina virtual, donde Windows Update aplica revisiones del sistema operativo sin reemplazar el disco de este. Con las actualizaciones automáticas de la imagen del sistema operativo habilitadas en el conjunto de escalado, no es necesario un proceso extra de aplicación de revisiones a través de Windows Update.

### <a name="service-fabric-requirements"></a>Requisitos de Service Fabric

Si utiliza Service Fabric, asegúrese de que se cumplen las condiciones siguientes:
-   El [nivel de durabilidad](../service-fabric/service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster) de Service Fabric es Silver o Gold, no Bronze (excepto para los tipos de nodo sin estado, que admiten actualizaciones automáticas de la imagen de sistema operativo).
-   La extensión de Service Fabric en la definición del modelo de conjunto de escalado debe tener la versión 1.1 o posterior de TypeHandlerVersion.
-   El nivel de durabilidad debe ser el mismo en el clúster de Service Fabric y la extensión Service Fabric de la definición del modelo de conjunto de escalado.
- No es necesario realizar un sondeo de estado adicional ni usar la extensión Estado de aplicación con la durabilidad Silver o Gold. La durabilidad Bronze con tipos de nodo que solo sean sin estado requiere un sondeo de estado adicional.
- La propiedad *virtualMachineProfile.osProfile.windowsConfiguration.enableAutomaticUpdates* debe establecerse en *false* en la definición del modelo de conjunto de escalado. La propiedad *enableAutomaticUpdates* habilita la aplicación de revisiones en la máquina virtual mediante Windows Update y no se admite en conjuntos de escalado de Service Fabric.

Asegúrese de que la configuración de durabilidad coincida con la del clúster y la extensión de Service Fabric, ya que la falta de coincidencia produce errores de actualización. Los niveles de durabilidad se pueden modificar según las directrices que se describen en [esta página](../service-fabric/service-fabric-cluster-capacity.md#changing-durability-levels).


## <a name="automatic-os-image-upgrade-for-custom-images"></a>Actualización automática de la imagen del sistema operativo para imágenes personalizadas

Se admite la actualización automática de imágenes del sistema operativo de las imágenes personalizadas implementadas por medio de [Azure Compute Gallery](../virtual-machines/shared-image-galleries.md). No se admiten otras imágenes personalizadas para las actualizaciones automáticas de imágenes del sistema operativo.

### <a name="additional-requirements-for-custom-images"></a>Requisitos adicionales de las imágenes personalizadas
- El proceso de instalación y configuración para la actualización automática de la imagen del sistema operativo es el mismo para todos los conjuntos de escalado que se detallan en la [sección configuración](virtual-machine-scale-sets-automatic-upgrade.md#configure-automatic-os-image-upgrade) de esta página.
- Las instancias de conjuntos de escalado configuradas para actualizaciones automáticas de imágenes del sistema operativo se actualizan a la versión más reciente de la imagen de Azure Compute Gallery cuando se publica una nueva versión de la imagen y se [replica](../virtual-machines/shared-image-galleries.md#replication) en la región de ese conjunto de escalado. Si la nueva imagen no se replica en la región donde se implementa la escala, las instancias del conjunto de escalado no se actualizarán a la versión más reciente. La replicación de imagen regional permite controlar el lanzamiento de la nueva imagen para los conjuntos de escalado.
- La nueva versión de la imagen no se debe excluir de la versión más reciente de esa imagen de la galería. Las versiones de las imágenes excluidas de la versión más reciente de la imagen de la galería no se implementan en el conjunto de escalado mediante la actualización automática de la imagen del sistema operativo.

> [!NOTE]
>Un conjunto de escalado puede tardar hasta 3 horas en desencadenar la primera implementación de actualización de imagen después de que el conjunto de escalado se configure por primera vez para las actualizaciones automáticas del sistema operativo. Se trata de un retraso puntual por conjunto de escalado. Los lanzamientos de imágenes posteriores se desencadenan en el conjunto de escalas en un plazo de 30-60 minutos.


## <a name="configure-automatic-os-image-upgrade"></a>Configuración de la actualización automática de imágenes del sistema operativo
Para configurar la actualización automática de las imágenes del sistema operativo, asegúrese de que la propiedad *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* está establecida en *true* en la definición del modelo del conjunto de escalado.

### <a name="rest-api"></a>API DE REST
En el ejemplo siguiente se describe cómo establecer las actualizaciones automáticas del sistema operativo en un modelo del conjunto de escalado:

```
PUT or PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version=2021-03-01`
```

```json
{
  "properties": {
    "upgradePolicy": {
      "automaticOSUpgradePolicy": {
        "enableAutomaticOSUpgrade":  true
      }
    }
  }
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Use el cmdlet [Update-AzVmss](/powershell/module/az.compute/update-azvmss) para configurar las actualizaciones automáticas de imágenes del sistema operativo para el conjunto de escalado. En el ejemplo siguiente se configuran actualizaciones automáticas para el conjunto de escalado llamado *myScaleSet* en el grupo de recursos llamado *myResourceGroup*:

```azurepowershell-interactive
Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -AutomaticOSUpgrade $true
```

### <a name="azure-cli-20"></a>CLI de Azure 2.0
Use el cmdlet [az vmss update](/cli/azure/vmss#az_vmss_update) para configurar las actualizaciones automáticas de imágenes del sistema operativo para el conjunto de escalado. Use la CLI de Azure 2.0.47 o superior. En el ejemplo siguiente se configuran actualizaciones automáticas para el conjunto de escalado llamado *myScaleSet* en el grupo de recursos llamado *myResourceGroup*:

```azurecli-interactive
az vmss update --name myScaleSet --resource-group myResourceGroup --set UpgradePolicy.AutomaticOSUpgradePolicy.EnableAutomaticOSUpgrade=true
```

> [!NOTE]
>Después de configurar las actualizaciones automáticas de la imagen del sistema operativo para el conjunto de escalado, también debe traer las máquinas virtuales del conjunto de escalado al modelo de conjunto de escalado más reciente si el conjunto de escalado usa el "Manual" [Directiva de actualización](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model).

## <a name="using-application-health-probes"></a>Uso de sondeos de estado de aplicaciones

Durante una actualización del sistema operativo, las instancias de máquina virtual de un conjunto de escalado se actualizan en lote de uno en uno. La actualización debe continuar solo si la aplicación cliente se encuentra en buen estado en las instancias de máquina virtual actualizadas. Se recomienda que la aplicación proporcione señales de estado al motor de actualización de sistema operativo del conjunto de escalado. De forma predeterminada, durante las actualizaciones del sistema operativo, la plataforma se fija en el estado de energía de la máquina virtual y el estado de aprovisionamiento de la extensión para determinar si una instancia de máquina virtual está en buen estado después de una actualización. Durante la actualización del sistema operativo de una instancia de máquina virtual, se reemplaza el disco del sistema operativo en una instancia de máquina virtual por un nuevo disco basado en la versión más reciente de la imagen. Una vez finalizada la actualización del sistema operativo, las extensiones configuradas se ejecutan en estas máquinas virtuales. Se considera que el estado de la aplicación es correcto solo cuando todas las extensiones de la instancia se han aprovisionado correctamente.

Un conjunto de escalado puede configurarse opcionalmente con sondeos de estado de aplicación para proporcionar a la plataforma información precisa sobre el estado actual de la aplicación. Los sondeos de estado de aplicación son sondeos de Load Balancer personalizados que se usan como una señal de estado. La aplicación que se ejecuta en una instancia de máquina virtual del conjunto de escalado puede responder a solicitudes HTTP o TCP externas para indicar si el mantenimiento es correcto. Para obtener más información sobre cómo funcionan los sondeos de Load Balancer personalizados, consulte [Descripción de los sondeos del equilibrador de carga](../load-balancer/load-balancer-custom-probe-overview.md). Los sondeos de mantenimiento de aplicaciones no se admiten con conjuntos de escalado de Service Fabric. Los conjuntos de escalado que no son de Service Fabric requieren sondeos de estado de la aplicación de Load Balancer o la [extensión Estado de la aplicación](virtual-machine-scale-sets-health-extension.md).

Si el conjunto de escalado está configurado para usar varios grupos de selección de ubicación, es necesario utilizar sondeos con una instancia de [Load Balancer estándar](../load-balancer/load-balancer-overview.md).

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Configuración de un sondeo de Load Balancer personalizado como sondeo de estado de aplicación en un conjunto de escalado
Como práctica recomendada, cree un sondeo del equilibrador de carga explícitamente para el estado del conjunto de escalado. Puede usarse el mismo punto de conexión para un sondeo HTTP o un sondeo TCP existente, pero un sondeo de mantenimiento puede requerir un comportamiento diferente al de un sondeo de equilibrador de carga tradicional. Por ejemplo, un sondeo de equilibrador de carga tradicional podría devolver un estado incorrecto si la carga en la instancia es demasiado alta, pero no sería adecuado para determinar el mantenimiento de la instancia durante una actualización automática del sistema operativo. Configure el sondeo para que tenga una tasa de sondeo elevada de menos de dos minutos.

Se puede hacer referencia al sondeo de equilibrador de carga en el valor *networkProfile* del conjunto de escalado, y se puede asociar con un equilibrador de carga interno o público del modo siguiente:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
}
```

> [!NOTE]
> Al utilizar actualizaciones automáticas del sistema operativo con Service Fabric, la nueva imagen del sistema operativo se implanta de dominio de actualización en dominio de actualización para mantener una alta disponibilidad de los servicios que se ejecutan en Service Fabric. Para usar las actualizaciones automáticas del sistema operativo en Service Fabric, el tipo de nodo del clúster debe estar configurado para usar el nivel de durabilidad Silver o superior. En el nivel de durabilidad Bronze, la actualización automática de la imagen de sistema operativo solo se admite para los tipos de nodo sin estado. Para más información sobre las características de durabilidad de los clústeres de Service Fabric, consulte [esta documentación](../service-fabric/service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster).

### <a name="keep-credentials-up-to-date"></a>Mantener actualizadas las credenciales
Si el conjunto de escalado usa credenciales para acceder a recursos externos (por ejemplo, si se configura una extensión de máquina virtual que usa un token de SAS para la cuenta de almacenamiento), asegúrese de que las credenciales estén actualizadas. Si las credenciales, incluidos los certificados y tokens, han expirado, se producirá un error en la actualización y el primer lote de máquinas virtuales se quedará en estado de error.

Los pasos recomendados para recuperar las máquinas virtuales y volver a habilitar la actualización automática del sistema operativo si se produce un error de autenticación de recursos son:

* Volver a generar el token (o cualquier otra credencial) pasada en las extensiones.
* Asegurarse de que cualquier credencial usada desde dentro de la máquina virtual para comunicarse con las entidades externas está actualizada.
* Actualizar las extensiones en el modelo de conjunto de escala con los tokens nuevos.
* Implementar el conjunto de escala actualizada, lo que actualizará todas las instancias de máquina virtual, incluyendo las que dieran error.

## <a name="using-application-health-extension"></a>Uso de la extensión de Estado de la aplicación
La extensión Estado de la aplicación se implementa dentro de una instancia de conjunto de escalado de máquinas virtuales e informa sobre el estado de la máquina virtual desde dentro de la instancia de conjunto de escalado. Puede configurar la extensión para hacer un sondeo en un punto de conexión de la aplicación y actualizar el estado de la aplicación en esa instancia. Azure comprueba el estado de esta instancia para determinar si una instancia es apta para las operaciones de actualización.

Dado que la extensión informa sobre el estado desde dentro de una máquina virtual, se puede usar en situaciones donde se pueden aprovechar los sondeos externos, como los de Estado de la aplicación (que usan [sondeos](../load-balancer/load-balancer-custom-probe-overview.md) de Azure Load Balancer personalizados).

Hay varias maneras de implementar la extensión Estado de la aplicación en los conjuntos de escalado tal como se detalla en los ejemplos de [este artículo](virtual-machine-scale-sets-health-extension.md#deploy-the-application-health-extension).

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>Obtención del historial de actualizaciones automáticas de imágenes del sistema operativo
Puede comprobar el historial de la actualización más reciente del sistema operativo realizada en el conjunto de escalado con Azure PowerShell, la CLI de Azure 2.0 o las API REST. Puede obtener el historial de los últimos cinco intentos de actualización del sistema operativo realizados en los últimos dos meses.

### <a name="rest-api"></a>API DE REST
En el ejemplo siguiente se usa la [API REST](/rest/api/compute/virtualmachinescalesets/getosupgradehistory) para comprobar el estado del conjunto de escalado denominado *ScaleSet* en el grupo de recursos denominado *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2021-03-01`
```

La llamada GET devuelve propiedades similares a la salida del ejemplo siguiente:

```json
{
    "value": [
        {
            "properties": {
        "runningStatus": {
          "code": "RollingForward",
          "startTime": "2018-07-24T17:46:06.1248429+00:00",
          "completedTime": "2018-04-21T12:29:25.0511245+00:00"
        },
        "progress": {
          "successfulInstanceCount": 16,
          "failedInstanceCount": 0,
          "inProgressInstanceCount": 4,
          "pendingInstanceCount": 0
        },
        "startedBy": "Platform",
        "targetImageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "2016.127.20180613"
        },
        "rollbackInfo": {
          "successfullyRolledbackInstanceCount": 0,
          "failedRolledbackInstanceCount": 0
        }
      },
      "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
      "location": "westeurope"
    }
  ]
}
```

### <a name="azure-powershell"></a>Azure PowerShell
Use el cmdlet [Get-AzVmss](/powershell/module/az.compute/get-azvmss) para comprobar el historial de actualizaciones del sistema operativo de su conjunto de escalado. En el ejemplo siguiente se detalla cómo revisar el estado de actualización del sistema operativo para un conjunto de escalado denominado *myScaleSet* en el grupo de recursos denominado *myResourceGroup*:

```azurepowershell-interactive
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>CLI de Azure 2.0
Use [az vmss get-os-upgrade-history](/cli/azure/vmss#az_vmss_get_os_upgrade_history) para comprobar el historial de actualizaciones del sistema operativo de su conjunto de escalado. Use la CLI de Azure 2.0.47 o superior. En el ejemplo siguiente se detalla cómo revisar el estado de actualización del sistema operativo para un conjunto de escalado denominado *myScaleSet* en el grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
az vmss get-os-upgrade-history --resource-group myResourceGroup --name myScaleSet
```

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>¿Cómo obtener la versión más reciente de una imagen del sistema operativo de una plataforma?

Puede obtener las versiones de las imágenes disponibles para la actualización automática del sistema operativo compatibles con las SKU utilizando los ejemplos siguientes:

### <a name="rest-api"></a>API DE REST
```
GET on `/subscriptions/subscription_id/providers/Microsoft.Compute/locations/{location}/publishers/{publisherName}/artifacttypes/vmimage/offers/{offer}/skus/{skus}/versions?api-version=2021-03-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
```azurepowershell-interactive
Get-AzVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

### <a name="azure-cli-20"></a>CLI de Azure 2.0
```azurecli-interactive
az vm image list --location "westus" --publisher "Canonical" --offer "UbuntuServer" --sku "16.04-LTS" --all
```

## <a name="manually-trigger-os-image-upgrades"></a>Desencadenamiento manual de las actualizaciones de imagen del sistema operativo
Con la actualización automática de la imagen del sistema operativo habilitada en el conjunto de escalado, no es necesario desencadenarlas manualmente. El orquestador de actualización del sistema operativo aplicará automáticamente la última versión disponible de la imagen a las instancias del conjunto de escalado sin intervención manual.

En casos concretos en los que no desee esperar a que el orquestador aplique la imagen más reciente, puede desencadenar manualmente una actualización de la imagen del sistema operativo mediante los siguientes ejemplos.

> [!NOTE]
> El desencadenador manual de la actualización de la imagen del sistema operativo no proporciona funciones de reversión automática. Si una instancia no recupera su estado después de una actualización, no se puede restaurar el disco del sistema operativo anterior.

### <a name="rest-api"></a>API DE REST
Use la llamada API [Start OS Upgrade](/rest/api/compute/virtualmachinescalesetrollingupgrades/startosupgrade) para iniciar una actualización gradual para mover todas las máquinas virtuales del conjunto de escalado a la versión más reciente disponible del sistema operativo con imagen. Las instancias que ya ejecutan la última versión del sistema operativo disponible no se ven afectadas. En el ejemplo siguiente se detalla cómo empezar una actualización de sistema operativo en un conjunto de escalado denominado *myScaleSet* en el grupo de recursos denominado *myResourceGroup*:

```
POST on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osRollingUpgrade?api-version=2021-03-01`
```

### <a name="azure-powershell"></a>Azure PowerShell
Use el cmdlet [Start-AzVmssRollingOSUpgrade](/powershell/module/az.compute/Start-AzVmssRollingOSUpgrade) para comprobar el historial de actualizaciones del sistema operativo de su conjunto de escalado. En el ejemplo siguiente se detalla cómo empezar una actualización de sistema operativo en un conjunto de escalado denominado *myScaleSet* en el grupo de recursos denominado *myResourceGroup*:

```azurepowershell-interactive
Start-AzVmssRollingOSUpgrade -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

### <a name="azure-cli-20"></a>CLI de Azure 2.0
Use [az vmss rolling-upgrade start](/cli/azure/vmss/rolling-upgrade#az_vmss_rolling_upgrade_start) para comprobar el historial de actualizaciones del sistema operativo del conjunto de escalado. Use la CLI de Azure 2.0.47 o superior. En el ejemplo siguiente se detalla cómo empezar una actualización de sistema operativo en un conjunto de escalado denominado *myScaleSet* en el grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
az vmss rolling-upgrade start --resource-group "myResourceGroup" --name "myScaleSet" --subscription "subscriptionId"
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Más información sobre la extensión Estado de la aplicación](../virtual-machine-scale-sets/virtual-machine-scale-sets-health-extension.md)
