---
title: Supervisión de un dispositivo Azure Stack Edge Pro mediante el panel de Kubernetes | Microsoft Docs
description: Describe cómo acceder y usar el panel de Kubernetes para supervisar el dispositivo Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/31/2021
ms.author: alkohli
ms.openlocfilehash: 30e46f9425f4015893c08b94382e87cfa93c8be8
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2021
ms.locfileid: "129363130"
---
# <a name="use-kubernetes-dashboard-to-monitor-your-azure-stack-edge-pro-gpu-device"></a>Uso del panel de Kubernetes para supervisar el dispositivo Azure Stack Edge Pro con GPU

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

En este artículo se describe cómo acceder y usar el panel de Kubernetes para supervisar el dispositivo Azure Stack Edge Pro con GPU. Para supervisar el dispositivo, puede usar gráficos en Azure Portal, ver el panel de Kubernetes o ejecutar comandos `kubectl` en la interfaz de PowerShell del dispositivo. 

Este artículo se centra solo en las tareas de supervisión que se pueden realizar en el panel de Kubernetes.

En este artículo aprenderá a:

> [!div class="checklist"]
>
> * Acceder al panel de Kubernetes en el dispositivo.
> * Ver los módulos implementados en el dispositivo.
> * Obtener la dirección IP para las aplicaciones implementadas en el dispositivo.
> * Ver los registros de contenedor para los módulos implementados en el dispositivo.


## <a name="about-kubernetes-dashboard"></a>Acerca del panel de Kubernetes

El panel de Kubernetes es una interfaz de usuario basada en Web que puede usar para solucionar problemas de las aplicaciones en contenedor. El panel de Kubernetes es una alternativa basada en interfaz de usuario a la línea de comandos `kubectl` de Kubernetes. Para más información, consulte [Panel de Kubernetes](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). 

En un dispositivo Azure Stack Edge Pro, puede usar el panel de Kubernetes en modo de *solo lectura* para obtener información general de las aplicaciones que se ejecutan en el dispositivo Azure Stack Edge Pro, ver el estado de los recursos de clúster de Kubernetes y ver todos los errores que se hayan producido en el dispositivo.

## <a name="access-dashboard"></a>Panel de acceso

El panel de Kubernetes es de *solo lectura* y se ejecuta en el nodo maestro de Kubernetes en el puerto 31000. Siga estos pasos para acceder al panel: 

1. En la interfaz de usuario local del dispositivo, vaya a **Dispositivo** y, a continuación, vaya a **Device endpoints** (Puntos de conexión del dispositivo). 
1. Copie el punto de conexión del **panel de Kubernetes**. Cree una entrada DNS en el archivo `C:\Windows\System32\Drivers\etc\hosts` del cliente para conectarse al panel de Kubernetes. 

    `<IP address of the Kubernetes dashboard>    <Kubernetes dashboard endpoint suffix>` 
        
    ![Adición de una entrada DNS para el punto de conexión del panel de Kubernetes](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/add-domain-name-service-entry-hosts-1.png) 

1. En la fila del punto de conexión del **panel de Kubernetes**, seleccione **Descargar configuración**. Esta acción descarga un elemento `kubeconfig` que le permite acceder al panel. Guarde el archivo `config.json` en el sistema local.   

1. Descargue el certificado del panel de Kubernetes desde la interfaz de usuario local. 
    1. En la interfaz de usuario local, vaya a **Certificados**.
    1. Localice la entrada del **certificado del punto de conexión del panel de Kubernetes**. A la derecha de esta entrada, seleccione **Descargar** para descargar el certificado en el sistema cliente que usará para acceder al panel. 

    ![Descarga del certificado del punto de conexión del panel de Kubernetes](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/download-kubernetes-dashboard-endpoint-certificate-1.png)  

1. Instale el certificado descargado en el cliente. Si usa un cliente Windows, siga estos pasos: 
    1. Seleccione el certificado y, en el **Asistente para importar certificados**, seleccione la ubicación del almacén como **Máquina local**. 

        ![Instalación del certificado en el cliente 1](media/azure-stack-edge-gpu-edge-container-registry/install-certificate-1.png) 
    
    1. Instale el certificado en la máquina local del almacén raíz de confianza. 

        ![Instalación del certificado en el cliente 2](media/azure-stack-edge-gpu-edge-container-registry/install-certificate-2.png) 
1. Copie y use la dirección URL del panel de Kubernetes para abrir el panel en un explorador. En la página de **inicio de sesión del panel de Kubernetes**:
    
    1. Seleccione **kubeconfig**. 
    1. Seleccione los puntos suspensivos **...** . Busque y señale la `kubeconfig` que descargó anteriormente en el sistema local. Haga clic en **Iniciar sesión**.
        ![Examinar el archivo kubeconfig](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-sign-in-2.png)    

6. Ahora puede ver el panel de Kubernetes del dispositivo Azure Stack Edge Pro en modo de solo lectura.

    ![Página principal del panel de Kubernetes](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-main-page-1.png)

## <a name="view-module-status"></a>Visualización del estado de un módulo

Los módulos de proceso son contenedores que tienen una lógica de negocios implementada. Puede usar el panel para comprobar si un módulo de proceso se ha implementado correctamente en un dispositivo Azure Stack Edge Pro.

Para ver el estado del módulo, siga estos pasos en el panel:

1. En el panel izquierdo del panel, vaya a **Espacio de nombres**. Filtre por el espacio de nombres donde se muestran los módulos de IoT Edge, en este caso **iotedge**.
1. En el panel izquierdo, vaya a **Cargas de trabajo > Implementaciones**.
1. En el panel de la derecha, verá todos los módulos implementados en el dispositivo. En este caso, se implementó un módulo GettingStartedWithGPU en Azure Stack Edge Pro. Puede ver que el módulo se ha implementado.

    ![Visualización de la implementación de módulos](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-module-deployment-1.png)

 
## <a name="get-ip-address-for-services-or-modules"></a>Obtención de la dirección IP para los servicios o módulos

Puede usar el panel para obtener las direcciones IP de los servicios o módulos que desea exponer fuera del clúster de Kubernetes. 

Asigne el intervalo IP para estos servicios externos mediante la interfaz de usuario web local del dispositivo en la página **Compute network settings** (Configuración de la red de proceso). Una vez que haya implementado los módulos de IoT Edge, puede desear conocer la dirección IP asignada a un módulo o servicio específico. 

Para obtener la dirección IP, siga estos pasos en el panel:

1. En el panel izquierdo del panel, vaya a **Espacio de nombres**. Filtre por el espacio de nombres en el que se implementa un servicio externo, en este caso **iotedge**.
1. En el panel izquierdo, vaya a **Discovery and Load Balancing > Services** (Detección y equilibrio de carga > Servicios).
1. En el panel de la derecha, verá todos los servicios que se ejecutan en el espacio de nombres `iotedge` en el dispositivo Azure Stack Edge Pro.

    ![Obtención de IP para servicios externos](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-get-ip-external-service-1.png)

## <a name="view-container-logs"></a>Visualización de registros de contenedores

Hay instancias en las que es necesario ver los registros del contenedor. Puede usar el panel para obtener los registros de un contenedor específico que haya implementado en el clúster de Kubernetes.

Para ver los registros de contenedor, siga estos pasos en el panel:

1. En el panel izquierdo del panel, vaya a **Espacio de nombres**. Filtre por el espacio de nombres donde se implementan los módulos de IoT Edge, en este caso **iotedge**.
1. En el panel izquierdo, vaya a **Cargas de trabajo > Pods**.
1. En el panel de la derecha, verá todos los pods que se ejecutan en el dispositivo. Identifique el pod que está ejecutando el módulo para el que desea ver los registros. Seleccione los puntos suspensivos verticales del pod que identificó y, en el menú contextual, seleccione **Registros**.

    ![Visualización de registros de contenedor 1](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-container-logs-1.png)

1. Los registros se muestran en un visor de registros que está integrado en el panel. También puede descargar los registros.

    ![Visualización de registros de contenedor 2](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-container-logs-1.png)
    

## <a name="view-cpu-memory-usage"></a>Visualización de uso de la CPU y la memoria

El panel de Kubernetes para el dispositivo Azure Stack Edge Pro también tiene un [complemento del servidor de métricas](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-metrics-pipeline/) que agrega el uso de la CPU y la memoria en los recursos de Kubernetes.
 
Por ejemplo, puede ver la CPU y la memoria consumida en las implementaciones en todos los espacios de nombres. 

![Visualización del uso de CPU y memoria en todas las implementaciones](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/view-cpu-memory-all-1.png)

También puede filtrar por un espacio de nombres específico. En el ejemplo siguiente, puede ver el consumo de CPU y memoria solo para implementaciones de Azure Arc.  

![Visualización del uso de CPU y memoria para implementaciones de Azure Arc](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/view-cpu-memory-azure-arc-1.png)

El servidor de métricas de Kubernetes proporciona canalizaciones de escalabilidad automática, como el [escalador automático de pods horizontal](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/).


## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [supervisar con Azure Monitor](azure-stack-edge-gpu-enable-azure-monitor.md).
Obtenga información sobre cómo [ejecutar y recopilar registros](azure-stack-edge-gpu-troubleshoot.md)
