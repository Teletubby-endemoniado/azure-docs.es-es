---
title: Creación del controlador de datos de Azure Arc | Modo de conexión directa
description: Explica cómo crear el controlador de datos en modo de conexión directa.
author: dnethi
ms.author: dinethi
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 07/30/2021
ms.topic: overview
ms.openlocfilehash: 042d7fd04ca3a41016e67481f81237a56e0dfc8d
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2021
ms.locfileid: "121737177"
---
#  <a name="create-azure-arc-data-controller-in-direct-connectivity-mode-using-cli"></a>Creación del controlador de datos de Azure Arc en modo de conexión directa mediante la CLI

En este artículo se describe cómo crear el controlador de datos de Azure Arc en modo de conectividad **directa** mediante la CLI, durante la versión preliminar actual de esta característica. 


## <a name="complete-prerequisites"></a>Realización de los requisitos previos

Antes de empezar, compruebe que ha completado los requisitos previos que se describen en [Implementación del controlador de datos: modo de conexión directa (requisitos previos)](create-data-controller-direct-prerequisites.md).

La creación del controlador de datos de Azure Arc en modo de conectividad **directa** involucra los pasos siguientes:

1. Crear una extensión de servicios de datos habilitados para Azure Arc. 
1. Crear una ubicación personalizada.
1. Cree el controlador de datos.

> [!NOTE]
> Actualmente, este paso solo se puede realizar desde el portal. Para obtener información detallada, consulte las [notas de la versión](release-notes.md). 

## <a name="create-an-azure-arc-enabled-data-services-extension"></a>Creación de una extensión de servicios de datos habilitados para Azure Arc

Use la extensión k8s de la CLI para crear una extensión de servicios de datos.

### <a name="set-environment-variables"></a>Establecimiento de variables de entorno

Establezca las siguientes variables de entorno que se usarán en el paso siguiente.

#### <a name="linux"></a>Linux

``` terminal
# where you want the connected cluster resource to be created in Azure 
export subscription=<Your subscription ID>
export resourceGroup=<Your resource group>
export resourceName=<name of your connected kubernetes cluster>
export location=<Azure location>
```

#### <a name="windows-powershell"></a>Windows PowerShell
``` PowerShell
# where you want the connected cluster resource to be created in Azure 
$ENV:subscription="<Your subscription ID>"
$ENV:resourceGroup="<Your resource group>"
$ENV:resourceName="<name of your connected kubernetes cluster>"
$ENV:location="<Azure location>"
```

### <a name="create-the-arc-data-services-extension"></a>Creación de la extensión de servicios de datos de Arc

#### <a name="linux"></a>Linux

```bash
az k8s-extension create -c ${resourceName} -g ${resourceGroup} --name ${ADSExtensionName} --cluster-type connectedClusters --extension-type microsoft.arcdataservices --auto-upgrade false --scope cluster --release-namespace arc --config Microsoft.CustomLocation.ServiceAccount=sa-bootstrapper

az k8s-extension show -g ${resourceGroup} -c ${resourceName} --name ${ADSExtensionName} --cluster-type connectedclusters
```

#### <a name="windows-powershell"></a>Windows PowerShell
```PowerShell
$ENV:ADSExtensionName="ads-extension"

az k8s-extension create -c "$ENV:resourceName" -g "$ENV:resourceGroup" --name "$ENV:ADSExtensionName" --cluster-type connectedClusters --extension-type microsoft.arcdataservices --auto-upgrade false --scope cluster --release-namespace arc --config Microsoft.CustomLocation.ServiceAccount=sa-bootstrapper

az k8s-extension show -g "$ENV:resourceGroup" -c "$ENV:resourceName" --name "$ENV:ADSExtensionName" --cluster-type connectedclusters
```

#### <a name="deploy-azure-arc-data-services-extension-using-private-container-registry-and-credentials"></a>Implementación de la extensión de servicios de datos de Azure Arc mediante credenciales y registro de contenedor privado

Use el comando siguiente si va a realizar la implementación desde el repositorio privado:

```azurecli
az k8s-extension create -c "<connected cluster name>" -g "<resource group>" --name "<extension name>" --cluster-type connectedClusters --extension-type microsoft.arcdataservices --scope cluster --release-namespace "<namespace>" --config Microsoft.CustomLocation.ServiceAccount=sa-bootstrapper --config imageCredentials.registry=<registry info> --config imageCredentials.username=<username> --config systemDefaultValues.image=<registry/repo/arc-bootstrapper:<imagetag>> --config-protected imageCredentials.password=$ENV:DOCKER_PASSWORD --debug
```

 Por ejemplo
```azurecli
az k8s-extension create -c "my-connected-cluster" -g "my-resource-group" --name "arc-data-services" --cluster-type connectedClusters --extension-type microsoft.arcdataservices --scope cluster --release-namespace "arc" --config Microsoft.CustomLocation.ServiceAccount=sa-bootstrapper --config imageCredentials.registry=mcr.microsoft.com --config imageCredentials.username=arcuser --config systemDefaultValues.image=mcr.microsoft.com/arcdata/arc-bootstrapper:latest --config-protected imageCredentials.password=$ENV:DOCKER_PASSWORD --debug
```


> [!NOTE]
> La instalación de la extensión de servicios de datos de Arc puede tardar un par de minutos en finalizar.

### <a name="verify-the-arc-data-services-extension-is-created"></a>Comprobación de la creación de la extensión de servicios de datos de Arc

Puede comprobar si se ha creado la extensión de servicios de datos habilitados para Arc desde el portal o mediante la conexión directa al clúster de Kubernetes habilitado para Arc. 

#### <a name="azure-portal"></a>Portal de Azure
1. Inicie sesión en Azure Portal y vaya al grupo de recursos donde se encuentra el recurso del clúster conectado de Kubernetes.
1. Seleccione el clúster de Kubernetes habilitado para Arc (tipo = "Kubernetes: Azure Arc") donde se implementó la extensión.
1. En el panel de navegación de la izquierda, en **Configuración**, seleccione "Extensiones".
1. Debería ver la extensión que se acaba de crear anteriormente en estado "Instalado".

:::image type="content" source="media/deploy-data-controller-direct-mode-prerequisites/dc-extensions-dashboard.png" alt-text="Panel Extensiones":::

#### <a name="kubectl-cli"></a>CLI de kubectl

1. Conéctese al clúster de Kubernetes mediante una ventana de terminal.
1. Ejecute el comando siguiente y asegúrese de que (1) se ha creado el espacio de nombres mencionado anteriormente y (2) el pod `bootstrapper` está en estado "En ejecución" antes de continuar con el paso siguiente.

``` console
kubectl get pods -n <name of namespace used in the json template file above>
```

Por ejemplo, los comandos siguientes obtienen los pods del espacio de nombres `arc`.

```console
#Example:
kubectl get pods -n arc
```

## <a name="create-a-custom-location-using-custom-location-cli-extension"></a>Creación de una ubicación personalizada mediante la extensión de la CLI de ubicación personalizada

Una ubicación personalizada es un recurso de Azure equivalente a un espacio de nombres en un clúster de Kubernetes.  Las ubicaciones personalizadas se usan como destino para implementar recursos en o desde Azure. Obtenga más información sobre las ubicaciones personalizadas en [Ubicaciones personalizadas basadas en Kubernetes habilitado para Azure Arc](../kubernetes/conceptual-custom-locations.md).

### <a name="set-environment-variables"></a>Establecimiento de variables de entorno

#### <a name="linux"></a>Linux

```bash
export clName=mycustomlocation
export clNamespace=arc
export hostClusterId=$(az connectedk8s show -g ${resourceGroup} -n ${resourceName} --query id -o tsv)
export extensionId=$(az k8s-extension show -g ${resourceGroup} -c ${resourceName} --cluster-type connectedClusters --name ${ADSExtensionName} --query id -o tsv)

az customlocation create -g ${resourceGroup} -n ${clName} --namespace ${clNamespace} \
  --host-resource-id ${hostClusterId} \
  --cluster-extension-ids ${extensionId} --location eastus
```

#### <a name="windows-powershell"></a>Windows PowerShell
```PowerShell
$ENV:clName="mycustomlocation"
$ENV:clNamespace="arc"
$ENV:hostClusterId = az connectedk8s show -g "$ENV:resourceGroup" -n "$ENV:resourceName" --query id -o tsv
$ENV:extensionId = az k8s-extension show -g "$ENV:resourceGroup" -c "$ENV:resourceName" --cluster-type connectedClusters --name "$ENV:ADSExtensionName" --query id -o tsv

az customlocation create -g "$ENV:resourceGroup" -n "$ENV:clName" --namespace "$ENV:clNamespace" --host-resource-id "$ENV:hostClusterId" --cluster-extension-ids "$ENV:extensionId"
```

## <a name="validate--the-custom-location-is-created"></a>Validación de la creación de la ubicación personalizada

Desde el terminal, ejecute el siguiente comando para enumerar las ubicaciones personalizadas y compruebe que el **estado de aprovisionamiento** se muestra como realizado correctamente:

```azurecli
az customlocation list -o table
```

## <a name="create-the-azure-arc-data-controller"></a>Creación del controlador de datos de Azure Arc

Después de crear la extensión y la ubicación personalizada, continúe con Azure Portal para implementar el controlador de datos de Azure Arc.

1. Inicie sesión en Azure Portal.
1. Busque "controlador de datos de Azure Arc" en Azure Marketplace e inicie el flujo de creación.
1. En la sección **Requisitos previos**, asegúrese de que el clúster de Kubernetes habilitado para Azure Arc (modo directo) esté seleccionado, y continúe con el paso siguiente.
1. En la sección **Detalles del controlador de datos**, elija una suscripción y un grupo de recursos.
1. Escriba un nombre para el controlador de datos.
1. Elija un perfil de configuración en función del proveedor de la distribución de Kubernetes en la que va a implementar.
1. Elija la ubicación personalizada que ha creado en el paso anterior.
1. Proporcione los detalles del nombre de inicio de sesión y la contraseña del administrador del controlador de datos.
1. Proporcione los detalles de ClientId, TenantId y secreto de cliente de la entidad de servicio que se usará para crear los objetos de Azure. Consulte [Carga de datos de uso, métricas y registros en Azure Monitor](upload-metrics-and-logs-to-azure-monitor.md) para obtener instrucciones detalladas sobre cómo crear una cuenta de entidad de servicio y los roles que se deben conceder para la cuenta.
1. Haga clic en **Siguiente**, revise todos los detalles de la página de resumen y haga clic en **Crear**.

## <a name="monitor-the-creation"></a>Supervisión de la creación

Cuando el estado de implementación de Azure Portal muestra que la implementación se ha realizado correctamente, puede comprobar el estado de la implementación del controlador de datos de Arc en el clúster de la siguiente manera:

```console
kubectl get datacontrollers -n arc
```

## <a name="next-steps"></a>Pasos siguientes

[Creación de un grupo de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc](create-postgresql-hyperscale-server-group.md)

[Creación de una Instancia administrada de Azure SQL en Azure Arc](create-sql-managed-instance.md)
