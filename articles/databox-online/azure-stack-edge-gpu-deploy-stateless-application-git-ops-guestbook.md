---
title: Implementación de una aplicación `Guestbook` PHP en Kubernetes habilitado para Azure Arc en un dispositivo Azure Stack Edge Pro con GPU | Microsoft Docs
description: Se describe cómo se implementa una aplicación sin estado `Guestbook` PHP con Redis mediante GitOps en un clúster de Kubernetes habilitado para Azure Arc de un dispositivo Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: bcf2b8e97a00e693825124e92acd0f551ebfde14
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2021
ms.locfileid: "129709777"
---
# <a name="deploy-a-php-guestbook-stateless-application-with-redis-on-azure-arc-enabled-kubernetes-cluster-on-azure-stack-edge-pro-gpu"></a>Implementación de una aplicación sin estado `Guestbook` PHP con Redis en un clúster de Kubernetes habilitado para Azure Arc en Azure Stack Edge Pro con GPU

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

En este artículo se muestra cómo compilar e implementar una aplicación web sencilla de varios niveles con Kubernetes y Azure Arc. Este ejemplo consta de los siguientes componentes:

- Un maestro de Redis de instancia única para almacenar las entradas de `guestbook`
- Varias instancias de Redis replicadas para atender las lecturas
- Varias instancias de front-end web

La implementación se realiza mediante GitOps en el clúster de Kubernetes habilitado para Azure Arc en un dispositivo Azure Stack Edge Pro. 

Este procedimiento está dirigido a los usuarios que han examinado las [cargas de trabajo de Kubernetes en un dispositivo Azure Stack Edge Pro](azure-stack-edge-gpu-kubernetes-workload-management.md) y están familiarizados con los conceptos de [¿Qué es Kubernetes habilitado para Azure Arc (versión preliminar)?](../azure-arc/kubernetes/overview.md)

> [!NOTE]
> Este artículo contiene referencias al término *esclavo*, un término que Microsoft ya no usa. Cuando se quite el término del software, se quitará también del artículo.

## <a name="prerequisites"></a>Requisitos previos

Antes de implementar la aplicación sin estado, asegúrese de que ha completado los siguientes requisitos previos en el dispositivo y en el cliente que vaya a usar para acceder al dispositivo:

### <a name="for-device"></a>Para el dispositivo

1. Tiene credenciales de inicio de sesión en un dispositivo Azure Stack Edge Pro de un nodo.
    1. El dispositivo está activado. Consulte [Activación del dispositivo](azure-stack-edge-gpu-deploy-activate.md).
    1. El dispositivo tiene el rol de proceso configurado desde Azure Portal y un clúster de Kubernetes. Consulte [Configuración del proceso](azure-stack-edge-gpu-deploy-configure-compute.md).

1. Ha habilitado Azure Arc en el clúster de Kubernetes existente en el dispositivo y tiene un recurso de Arc de Azure correspondiente en Azure Portal. Para conocer los pasos detallados, consulte [Habilitación de Azure Arc en un dispositivo Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md).

### <a name="for-client-accessing-the-device"></a>Para el cliente que va a acceder al dispositivo

1. Tiene un sistema cliente de Windows que se usará para acceder al dispositivo Azure Stack Edge Pro.
  
    - El cliente ejecuta Windows PowerShell 5.0 o una versión posterior. Para descargar la última versión de Windows PowerShell, vaya a [Instalación de Windows PowerShell](/powershell/scripting/install/installing-windows-powershell).
    
    - También puede utilizar cualquier otro cliente con un [sistema operativo compatible](azure-stack-edge-gpu-system-requirements.md#supported-os-for-clients-connected-to-device). En este artículo, el procedimiento que se describe emplea un cliente Windows. 
    
1. Ha completado el procedimiento descrito en [Acceso al clúster de Kubernetes en el dispositivo Azure Stack Edge Pro](azure-stack-edge-gpu-create-kubernetes-cluster.md). Ha:
    
    - instalado `kubectl` en el cliente. <!--and saved the `kubeconfig` file with the user configuration to C:\\Users\\&lt;username&gt;\\.kube. -->
    
    - Asegúrese de que la versión del cliente de `kubectl` es como máximo una versión superior o inferior a la versión maestra de Kubernetes que se ejecuta en el dispositivo Azure Stack Edge Pro. 
      - Use `kubectl version` para comprobar la versión de kubectl que se ejecuta en el cliente. Anote la versión completa.
      - En la interfaz de usuario local del dispositivo Azure Stack Edge Pro, vaya a **Información general** y anote el número de software de Kubernetes. 
      - Compruebe la compatibilidad entre estas dos versiones con la asignación proporcionada en la versión de Kubernetes admitida. <!--insert link-->

1. Tiene una [configuración de GitOps que puede usar para ejecutar una implementación de Azure Arc](https://github.com/kagoyal/dbehaikudemo). En este ejemplo se usan los siguientes archivos `yaml` para la implementación en un dispositivo Azure Stack Edge Pro.

    - `frontend-deployment.yaml`<!-- - The guestbook application has a web frontend serving the HTTP requests written in PHP. It is configured to connect to the redis-master Service for write requests and the redis-slave service for Read requests. This file describes a deployment that runs the frontend of the guestbook application.-->
    - `frontend-service.yaml` <!-- - This allows you to configure an externally visible frontend Service that can be accessed from outside the Kubernetes cluster on your device.-->
    - `redis-master-deployment.yaml` <!-- - This deployment file runs a single replica Redis master Pod. The guestbook application uses Redis to store its data. It writes its data to a Redis master instance and reads data from multiple Redis slave instances.-->
    - `redis-master-service.yaml` <!-- - The guestbook application needs to communicate to the Redis master to write its data. You need to apply a Service to proxy the traffic to the Redis master Pod. A Service defines a policy to access the Pods.-->
    - `redis-slave-deployment.yaml` <!-- - Although the Redis master is a single pod, you can make it highly available to meet traffic demands by adding replica Redis slaves. The Redis Slave Deployment specifies two replicas.-->
    - `redis-slave-service.yaml` <!-- - The guestbook application needs to communicate to Redis slaves to read data. To make the Redis slaves discoverable, you need to set up a Service. A Service provides transparent load balancing to a set of Pods.-->

<!-- az cli version requirements-->


## <a name="deploy-configuration"></a>Configuración de la implementación

Siga estos pasos para configurar el recurso de Azure Arc para implementar una configuración de GitOps mediante Azure Portal: 

1. En Azure Portal, vaya al recurso de Azure Arc que ha creado al habilitar Azure Arc en el clúster de Kubernetes en el dispositivo. 

    ![Ir al recurso de Azure Arc](media/azure-stack-edge-gpu-connect-powershell-interface/verify-azure-arc-enabled-1.png)

1. Vaya a **Configuraciones** y seleccione **+Agregar configuración**.

    ![Captura de pantalla que muestra el clúster de Kubernetes habilitado para Azure Arc con la opción Agregar configuración seleccionada.](media/azure-stack-edge-gpu-connect-powershell-interface/select-configurations-1.png)

1. En **Agregar configuración**, escriba los valores adecuados para los campos y seleccione **Aplicar**.

    |Parámetro  |Descripción |
    |---------|---------|
    |Nombre de la configuración     | Nombre del recurso de configuración.        |
    |Nombre de la instancia del operador     |Nombre de instancia del operador que identifica una configuración concreta. Name es una cadena con un máximo de 253 caracteres; se admiten solo minúsculas, caracteres alfanuméricos, guiones y puntos.         |
    |Espacio de nombres del operador     | Se establece en **demotestguestbook** para coincidir con el espacio de nombres especificado en la implementación `yaml`. <br> El campo define el espacio de nombres en el que está instalado el operador. Name es una cadena con un máximo de 253 caracteres; se admiten solo minúsculas, caracteres alfanuméricos, guiones y puntos.         |
    |Dirección URL del repositorio     |<br>Ruta de acceso al repositorio de Git en formato `http://github.com/username/repo` o `git://github.com/username/repo` en el que se encuentra la configuración de GitOps.         |
    |Ámbito del operador     | Seleccione **Espacio de nombres**. <br>Este parámetro define el ámbito en el que se instala el operador. Seleccione Espacio de nombres para instalar el operador en el espacio de nombres especificado en los archivos yaml de implementación.       |
    |Tipo de operador     | Deje el valor predeterminado. <br>Este parámetro especifica el tipo de operador, que se establece como flujo de forma predeterminada.        |
    |Parámetros del operador     | Déjelo en blanco. <br>Este parámetro contiene los parámetros que se van a pasar al operador de flujo.        |
    |Helm     | Establezca este parámetro en **Deshabilitado**. <br>Habilite esta opción si va a realizar implementaciones basadas en gráficos.        |


    ![Adición de configuración](media/azure-stack-edge-gpu-connect-powershell-interface/add-configuration-1.png)


1. Se inicia la implementación de la configuración y **Operator state** (Estado del operador) aparece como **Pendiente**. 

    ![Captura de pantalla que muestra el clúster de Kubernetes habilitado para Azure Arc en un estado pendiente cuando se actualiza.](media/azure-stack-edge-gpu-connect-powershell-interface/view-configurations-1.png)

1. La implementación tarda un par de minutos. Una vez finalizada, **Operator state** (Estado del operador) se muestra como **Instalado**.

    ![Captura de pantalla que muestra el clúster de Kubernetes habilitado para Azure Arc en un estado instalado.](media/azure-stack-edge-gpu-connect-powershell-interface/view-configurations-2.png)

## <a name="verify-deployment"></a>Comprobación de la implementación

La implementación mediante la configuración de GitOps crea un espacio de nombres de `demotestguestbook` como se especifica en los archivos `yaml` de implementación ubicados en el repositorio de Git.

1. Después de aplicar la configuración de GitOps, [conéctese a la interfaz de PowerShell del dispositivo](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface).
1. Ejecute el siguiente comando para enumerar los pods que se ejecutan en el espacio de nombres de `demotestguestbook` correspondiente a la implementación.

    `kubectl get pods -n <your-namespace>`
  
    Esta es una salida de ejemplo.
  
    ```powershell
    [10.128.44.240]: PS>kubectl get pods -n demotestguestbook
    NAME                            READY   STATUS    RESTARTS   AGE
    aseoperator1-5569658644-cqtb5   1/1     Running   0          91m
    frontend-6cb7f8bd65-4xb4f       1/1     Running   0          91m
    frontend-6cb7f8bd65-q9cxj       1/1     Running   0          91m
    frontend-6cb7f8bd65-xpzs6       1/1     Running   0          91m
    memcached-86bdf9f56b-5l2fq      1/1     Running   0          91m
    redis-master-7db7f6579f-2z29w   1/1     Running   0          91m
    redis-slave-7664787fbc-lgr2n    1/1     Running   0          91m
    redis-slave-7664787fbc-vlvzn    1/1     Running   0          91m
    [10.128.44.240]: PS>
    ```  

1. En este ejemplo, el servicio de front-end se ha implementado como type:LoadBalancer. Tendrá que buscar la dirección IP de este servicio para ver `guestbook`. Ejecute el siguiente comando:

    `kubectl get service -n <your-namespace>`
    
    ```powershell
    [10.128.44.240]: PS>kubectl get service -n demotestguestbook
    NAME           TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)        AGE
    frontend       LoadBalancer   10.96.79.38      10.128.44.245   80:31238/TCP   85m
    memcached      ClusterIP      10.102.47.75     <none>          11211/TCP      85m
    redis-master   ClusterIP      10.104.32.99     <none>          6379/TCP       85m
    redis-slave    ClusterIP      10.104.215.146   <none>          6379/TCP       85m
    [10.128.44.240]: PS>
    ```
1. El servicio de front-end de `type:LoadBalancer` tiene una dirección IP externa. Esta dirección IP procede del intervalo de direcciones IP que especificó para los servicios externos al configurar las opciones de red de proceso en el dispositivo. Use esta dirección IP para ver `guestbook` en la dirección URL: `https://<external-IP-address>`.

    ![Visualización del libro de visitas](media/azure-stack-edge-gpu-connect-powershell-interface/view-guestbook-1.png)

## <a name="delete-deployment"></a>Eliminación de la implementación

Para eliminar la implementación, puede eliminar la configuración de Azure Portal. Al eliminar la configuración se eliminarán los objetos que se crearon, incluidas las implementaciones y los servicios.

1. En Azure Portal, vaya al recurso de Azure Arc > Configuraciones. 
1. Ubique la configuración que desea eliminar. Seleccione ... para invocar el menú contextual y, después, **Eliminar**.
    ![Eliminar configuración](media/azure-stack-edge-gpu-connect-powershell-interface/delete-configuration-1.png)

La configuración puede tardar varios minutos en eliminarse.
 
<!--```powershell
kubectl delete deployment <deployment-name> -n <your-namespace>
kubectl delete service <service-name> -n <your-namespace>
```

Here is a sample output of when you delete the deployments and the services.

```powershell
C:\Users\user>kubectl delete deployment,svc mysql -n userns1
deployment.apps "mysql" deleted
service "mysql" deleted
C:\Users\user>
```-->


## <a name="next-steps"></a>Pasos siguientes

Aprenda a [usar el panel de Kubernetes para supervisar las implementaciones en un dispositivo Azure Stack Edge Pro](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md).
